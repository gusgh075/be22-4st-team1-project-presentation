# K8s 포트 구조 — MapLog 프로젝트 기반

> `k8s-manifests` 레포지토리의 실제 YAML 파일을 기준으로 설명합니다.
> 관련 파일: `service.yaml`, `ingress.yaml`, `deployment.yaml`

---

## 1. 전체 트래픽 흐름

```
외부 사용자
    │
    │  :80
    ▼
Nginx Ingress Controller  ← 유일한 외부 진입점
    │
    ├─ path: /api  ──→  Service(map-log-backend)  :8080
    │                          │
    │                          ▼
    │                   Pod × 3 (Spring Boot)  containerPort: 8080
    │
    └─ path: /     ──→  Service(map-log-frontend) :80
                               │
                               ▼
                        Pod × 1 (Vue+Nginx)   containerPort: 80

                   [클러스터 내부 전용]
                   Service(mariadb)           :3306
                          │
                          ▼
                   Pod × 1 (MariaDB)          containerPort: 3306
                          ▲
                          │ jdbc:mariadb://mariadb:3306
                   Pod (Spring Boot) 에서만 접근
```

---

## 2. 쿠버네티스 포트 4종류

트래픽이 들어오는 방향 순서로 정리하면 다음과 같습니다.

| 포트 종류 | 위치 | 접근 범위 | 설명 |
|----------|------|----------|------|
| `nodePort` | Node | 외부 | 노드 IP로 외부에서 접근하는 포트 (30000~32767) |
| `port` | Service | 클러스터 내부 | 클러스터 내부에서 Service를 부를 때 쓰는 포트 |
| `targetPort` | Service → Pod | - | Service가 실제로 Pod에 전달하는 포트 |
| `containerPort` | Pod | - | 컨테이너가 실제로 리슨하는 포트 (선언용) |

```
외부 요청
    │  nodePort (30000~32767)
    ▼
  Node
    │  port (Service 포트)
    ▼
  Service
    │  targetPort
    ▼
  Pod
    │  containerPort
    ▼
  컨테이너 프로세스
```

---

## 3. service.yaml 분석

```yaml
# map-log-backend
apiVersion: v1
kind: Service
metadata:
  name: map-log-backend
  namespace: map-log
spec:
  selector:
    app: map-log-backend     # 이 라벨을 가진 Pod으로 라우팅
  ports:
  - port: 8080               # 클러스터 내부에서 이 Service를 호출할 때 쓰는 포트
    targetPort: 8080         # Pod의 8080으로 전달
```

```yaml
# map-log-frontend
spec:
  ports:
  - port: 80
    targetPort: 80
```

```yaml
# mariadb
spec:
  ports:
  - port: 3306
    targetPort: 3306
```

**핵심 포인트**: 세 Service 모두 `type`이 명시되지 않았습니다.
쿠버네티스 기본값은 **ClusterIP**이므로 모두 외부에서 직접 접근이 불가합니다.

| Service | type | 외부 접근 |
|---------|------|----------|
| map-log-backend | ClusterIP (기본값) | 불가 |
| map-log-frontend | ClusterIP (기본값) | 불가 |
| mariadb | ClusterIP (기본값) | 불가 |

---

## 4. ingress.yaml 분석

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: map-log-ingress
  namespace: map-log
  annotations:
    kubernetes.io/ingress.class: nginx   # Nginx Ingress Controller 사용
spec:
  rules:
  - http:
      paths:
      - path: /api            # /api 로 시작하는 요청
        pathType: Prefix
        backend:
          service:
            name: map-log-backend
            port:
              number: 8080    # Service의 port (ClusterIP:8080)

      - path: /               # 나머지 모든 요청
        pathType: Prefix
        backend:
          service:
            name: map-log-frontend
            port:
              number: 80      # Service의 port (ClusterIP:80)
```

여기서 `port.number`는 **Service의 `port`** 를 가리킵니다. Pod의 containerPort가 아닙니다.

### 라우팅 규칙

```
외부 GET /api/diaries   →  map-log-backend:8080  →  Spring Boot Pod:8080
외부 GET /              →  map-log-frontend:80   →  Vue+Nginx Pod:80
외부 GET /api/users/me  →  map-log-backend:8080  →  Spring Boot Pod:8080
```

`/api`가 `/`보다 더 구체적이기 때문에 우선 매칭됩니다.

---

## 5. deployment.yaml 포트 선언

### Spring Boot (백엔드)

```yaml
spec:
  replicas: 3                    # Pod 3개 운영
  template:
    spec:
      containers:
      - name: map-log-backend
        image: gusgh07/map-log-backend:49
        ports:
        - containerPort: 8080    # 컨테이너가 8080을 리슨함을 선언
        env:
        - name: SPRING_PROFILES_ACTIVE
          value: "dev"           # S3FileStorageService 활성화
```

### Vue + Nginx (프론트엔드)

```yaml
spec:
  replicas: 1
  template:
    spec:
      containers:
      - name: map-log-frontend
        image: gusgh07/map-log-frontend:49
        ports:
        - containerPort: 80      # Nginx가 80을 리슨
        env:
        - name: VITE_API_BASE_URL
          value: "/api"          # 상대경로 → Ingress가 /api를 백엔드로 라우팅
```

### MariaDB

```yaml
spec:
  replicas: 1
  template:
    spec:
      containers:
      - name: mariadb
        image: mariadb:11
        ports:
        - containerPort: 3306
```

---

## 6. MariaDB — 내부 DNS 통신

MariaDB는 Ingress에 등록되어 있지 않습니다.
백엔드 Pod은 **쿠버네티스 내부 DNS**를 통해 Service 이름으로 직접 접근합니다.

```yaml
# deployment.yaml 백엔드 환경변수
- name: SPRING_DATASOURCE_URL
  value: "jdbc:mariadb://mariadb:3306/maplog_dev?..."
#                        ^^^^^^^^
#                        Service 이름 = 클러스터 내부 DNS 주소
```

```
Spring Boot Pod
    │  mariadb:3306 (DNS 조회)
    ▼
쿠버네티스 DNS → mariadb Service (ClusterIP)
    │
    ▼
MariaDB Pod:3306
```

`mariadb`라는 이름은 `service.yaml`의 `metadata.name: mariadb`와 일치합니다.
같은 네임스페이스(`map-log`)에 있기 때문에 Service 이름만으로 DNS 조회가 됩니다.
다른 네임스페이스라면 `mariadb.map-log.svc.cluster.local`처럼 전체 주소가 필요합니다.

---

## 7. Nginx Ingress Controller의 외부 노출 방식

모든 Service가 ClusterIP인데 어떻게 외부에서 80으로 접근할 수 있을까요?
Nginx Ingress Controller 자체는 별도로 설치되며, 이 환경(Docker Desktop K8s)에서는
**`hostPort`** 방식으로 노드의 80번 포트를 Ingress Controller Pod에 직접 바인딩합니다.

```
외부 :80
    │
    │  hostPort: 80 (NodePort 범위 우회)
    ▼
Nginx Ingress Controller Pod
    │
    │  ClusterIP 내부 통신
    ▼
map-log-frontend / map-log-backend Service
```

NodePort를 사용했다면 30000번대 포트로만 접근 가능했을 것이고,
클라이언트는 `노드IP:30080` 같은 주소를 직접 써야 했을 것입니다.

---

## 8. 서비스별 포트 전체 요약

| 구성 요소 | containerPort | Service port | Service targetPort | 외부 접근 경로 |
|----------|--------------|--------------|-------------------|--------------|
| Spring Boot | 8080 | 8080 | 8080 | Ingress `/api` → :8080 |
| Vue+Nginx | 80 | 80 | 80 | Ingress `/` → :80 |
| MariaDB | 3306 | 3306 | 3306 | 불가 (내부 전용) |
| Ingress Controller | 80 | - | - | hostPort :80 직접 바인딩 |

---

## 9. `containerPort` 선언이 선택적인 이유

`deployment.yaml`에서 `containerPort`는 사실 **문서화 목적**입니다.
이 값이 없어도 실제 트래픽 라우팅에는 영향이 없습니다.

트래픽 라우팅에 실제로 영향을 주는 값은 Service의 **`targetPort`** 입니다.
`targetPort`가 `containerPort`와 일치해야 Pod 안의 프로세스에 정상적으로 전달됩니다.

```yaml
# 이 두 값이 일치해야 실제로 통신이 됨
# deployment.yaml
- containerPort: 8080     # Spring Boot가 8080을 리슨

# service.yaml
  targetPort: 8080        # Service가 Pod의 8080으로 전달
```
