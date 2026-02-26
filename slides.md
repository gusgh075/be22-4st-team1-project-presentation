---
theme: default
background: /main_photo.png
class: text-center
highlighter: shiki
lineNumbers: true
info: |
  ## MapLog Presentation
  위치 기반 소셜 다이어리 서비스 발표 자료
drawings:
  persist: false
transition: slide-left
title: MapLog - 공간의 맥락을 기록하다
# Theme configuration
colorSchema: dark
fonts:
  sans: 'Inter'
  serif: 'Inter'
  mono: 'Fira Code'
---

# <span class="ml-logo-gradient">MapLog</span>
### 공간의 맥락을 기록하는 소셜 다이어리

나의 발자취를 지도 위에 기록하고 공유하는 서비스

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer hover:bg-white hover:bg-opacity-10">
    발표 시작 <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <button @click="$slidev.nav.prev" class="text-xl icon-btn opacity-50 !border-none hover:opacity-100"><carbon:chevron-left/></button>
  <button @click="$slidev.nav.next" class="text-xl icon-btn opacity-50 !border-none hover:opacity-100"><carbon:chevron-right/></button>
</div>

<!--
안녕하세요. MapLog 팀 발표를 맡은 [이름]입니다. 저희 서비스를 한 문장으로 소개하자면, "나의 발자취를 지도 위에 기록하고, 친구와 공유하는 위치 기반 소셜 다이어리"입니다. 기술적 한계를 극복하며 구축한 전체 아키텍처와 치열했던 트러블슈팅 경험을 중심으로 발표를 시작하겠습니다.
-->

---
layout: default
---

# 00. 팀 소개 및 역할 분담

<div class="grid grid-cols-2 gap-10 pt-10 text-left">
  <div v-click class="border border-gray-800 bg-[#161b22] p-6 rounded-xl shadow-lg">
    <h3 class="text-blue-400 mb-4"><carbon:user-activity class="inline mr-2"/> Backend & DevOps</h3>
    <ul class="space-y-3 opacity-90 text-sm">
      <li><b>홍길동 (Leader)</b></li>
      <li>Spring Boot 기반 API 설계 및 구현</li>
      <li>K8s 클러스터 구축 및 CI/CD 파이프라인</li>
      <li>AWS S3 연동 및 이미지 처리</li>
    </ul>
  </div>
  <div v-click class="border border-gray-800 bg-[#161b22] p-6 rounded-xl shadow-lg">
    <h3 class="text-purple-400 mb-4"><carbon:application class="inline mr-2"/> Frontend & UI/UX</h3>
    <ul class="space-y-3 opacity-90 text-sm">
      <li><b>이순신</b></li>
      <li>Vue 3 Composition API 기반 웹 개발</li>
      <li>카카오 맵 SDK 연동 및 마커 최적화</li>
      <li>Pinia 상태 관리 및 SSE 알림 구현</li>
    </ul>
  </div>
</div>

---
layout: default
---

# 01. 기획 배경 및 문제 정의

<div class="grid grid-cols-2 gap-10 pt-10">
  <div v-click>
    <h3 class="text-red-500 mb-4"><carbon:warning class="inline mr-2"/> Pain Point</h3>
    <p>기존 SNS 및 다이어리 앱의 한계</p>
    <ul>
      <li>"언제" 기준의 시간 순 나열 위주</li>
      <li>"어디서"라는 <b>공간적 맥락</b> 부재</li>
      <li>장소 중심의 추억 회상이 어려움</li>
    </ul>
  </div>
  <div v-click>
    <h3 class="text-green-500 mb-4"><carbon:idea class="inline mr-2"/> Solution</h3>
    <p>MapLog의 차별화된 경험</p>
    <ul>
      <li><b>지도를 인터페이스 전면</b>에 배치</li>
      <li>지도 위 핀(Marker)을 통한 발자취 시각화</li>
      <li>위치 정보를 결합한 직관적인 기록과 소통</li>
    </ul>
  </div>
</div>

<!--
기존 SNS나 다이어리 앱들은 "언제" 기준의 시간 순 나열에는 강하지만, "어디서"라는 공간적 맥락을 직관적으로 보여주지 못한다는 한계가 있었습니다. 그래서 저희는 지도를 서비스의 전면으로 가져와, 사용자가 지도 위에 핀을 꽂고 발자취를 시각화하며 친구들과 소통할 수 있는 차별화된 경험을 설계했습니다.
-->

---
layout: default
---

# 02. 기술 스택 및 서비스 아키텍처

<div class="grid grid-cols-3 gap-4 pt-4">
  <div class="border border-blue-900/30 bg-blue-900/5 p-4 rounded-xl shadow-sm">
    <h3 class="text-blue-400 border-b border-blue-900/30 mb-2">Frontend</h3>
    <ul class="text-sm opacity-90">
      <li>Vue 3 (Composition API)</li>
      <li>Pinia (State Management)</li>
      <li>Vite / Axios</li>
      <li>Kakao Maps SDK</li>
    </ul>
  </div>
  <div class="border border-green-900/30 bg-green-900/5 p-4 rounded-xl shadow-sm">
    <h3 class="text-green-400 border-b border-green-900/30 mb-2">Backend</h3>
    <ul class="text-sm opacity-90">
      <li>Spring Boot 3.5</li>
      <li>Java 21</li>
      <li>Spring Data JPA / MyBatis</li>
      <li>MariaDB 11 / JWT</li>
    </ul>
  </div>
  <div class="border border-purple-900/30 bg-purple-900/5 p-4 rounded-xl shadow-sm">
    <h3 class="text-purple-400 border-b border-purple-900/30 mb-2">Infrastructure</h3>
    <ul class="text-sm opacity-90">
      <li>Docker / Kubernetes</li>
      <li>Jenkins (CI)</li>
      <li>ArgoCD (CD / GitOps)</li>
      <li>AWS S3</li>
    </ul>
  </div>
</div>

<div class="mt-6 p-4 bg-[#161b22] border border-gray-800/50 rounded-xl text-xs font-mono text-center" v-click>
  <span class="text-blue-400">Developer Push</span>
  <span class="text-gray-500 mx-2">→</span>
  <span class="text-yellow-500">GitHub Webhook (ngrok)</span>
  <span class="text-gray-500 mx-2">→</span>
  <span class="text-orange-400">Jenkins (Build + Push)</span>
  <span class="text-gray-500 mx-2">→</span>
  <span class="text-purple-400">Manifest Repo</span>
  <span class="text-gray-500 mx-2">→</span>
  <span class="text-green-400">ArgoCD → K8s Cluster</span>
</div>

<!--
저희 서비스의 전체적인 아키텍처와 기술 스택입니다. 프론트엔드는 Vue 3의 Composition API와 Pinia를 사용해 상태를 중앙 관리했습니다. 백엔드는 Spring Boot 3.5와 Java 21 기반으로 구축했으며, 특히 인프라 영역에 공을 들여 Docker와 Kubernetes 환경 위에서 Jenkins와 ArgoCD를 결합한 GitOps 방식의 자동 배포 파이프라인을 완성했습니다.
-->

---
layout: two-cols
---

# 03. 백엔드 설계: CQRS 패턴

<div class="pr-4">
  <div v-click>
    <h3 class="text-orange-400 mb-2">Command (쓰기)</h3>
    <div class="bg-orange-900/10 border border-orange-800/20 p-4 rounded-xl text-sm mb-4">
      <p><b class="text-orange-300">JPA 기반 강타입 엔티티 관리</b></p>
      <ul class="opacity-90">
        <li>비즈니스 로직 및 유효성 검증</li>
        <li>도메인 상태 변경 (CUD) 집중</li>
        <li>안정적인 연관관계 처리</li>
      </ul>
    </div>
  </div>
  <div v-click>
    <h3 class="text-blue-400 mb-2">Query (읽기)</h3>
    <div class="bg-blue-900/10 border border-blue-800/20 p-4 rounded-xl text-sm">
      <p><b class="text-blue-300">MyBatis 활용 조회 최적화</b></p>
      <ul class="opacity-90">
        <li>복잡한 조인 쿼리 및 성능 최적화</li>
        <li>화면 요구사항에 맞는 DTO 매핑</li>
        <li>읽기 전용 모델로 시스템 복잡도 완화</li>
      </ul>
    </div>
  </div>
</div>

<template v-slot:right>
<div class="pl-4 pt-8" v-click>
  <p class="text-xs text-gray-600 mb-2 font-mono">패키지 구조</p>
  <div class="bg-gray-900 text-green-300 p-4 rounded text-xs font-mono leading-6">
    <p>com.maplog</p>
    <p class="pl-2">├── diary</p>
    <p class="pl-4 text-orange-300">│   ├── command/</p>
    <p class="pl-6 text-orange-200">│   │   ├── DiaryCommandController</p>
    <p class="pl-6 text-orange-200">│   │   └── DiaryCommandService (JPA)</p>
    <p class="pl-4 text-blue-300">│   └── query/</p>
    <p class="pl-6 text-blue-200">│       ├── DiaryQueryController</p>
    <p class="pl-6 text-blue-200">│       └── DiaryQueryService (MyBatis)</p>
    <p class="pl-2">└── user/ ...</p>
  </div>
  <p class="text-xs text-gray-600 mt-3">→ 읽기/쓰기 분리로 팀원 간 코드 충돌 최소화</p>
</div>
</template>

<!--
백엔드 설계의 핵심은 CQRS(명령과 조회의 책임 분리) 패턴의 도입입니다. 다이어리 도메인의 특성상 쓰기 작업은 연관관계와 검증이 복잡한 반면, 읽기 작업은 화면에 필요한 특정 데이터만 가볍게 가져와야 합니다. 이를 패키지 수준에서 분리하여 쓰기는 JPA로 안정성을 챙기고, 읽기는 MyBatis 등을 통해 부분적으로 최적화하여 시스템 복잡도를 크게 낮췄습니다.
-->

---
layout: default
---

# 04. 스토리지 전략: AWS S3 Presigned URL

<div class="grid grid-cols-2 gap-6 pt-4">
  <div v-click>
    <h4 class="text-orange-700 font-bold mb-2">📤 업로드 흐름</h4>
    <div class="bg-gray-50 p-3 rounded text-xs font-mono leading-6 border text-gray-800">
      Client<br>
      &nbsp;↓ multipart/form-data<br>
      Spring Boot (putObject)<br>
      &nbsp;↓ S3 원본 URL → DB 저장<br>
      AWS S3 버킷
    </div>
  </div>
  <div v-click>
    <h4 class="text-blue-700 font-bold mb-2">📥 조회 흐름 (백엔드 우회)</h4>
    <div class="bg-gray-50 p-3 rounded text-xs font-mono leading-6 border text-gray-800">
      Client → Spring Boot<br>
      &nbsp;↓ generatePresignedUrl()<br>
      1시간 유효 서명 URL 발급<br>
      &nbsp;↓ (백엔드 우회!)<br>
      Client → S3 직접 GET
    </div>
  </div>
</div>

<div class="grid grid-cols-2 gap-4 mt-4">
  <div class="bg-yellow-50 p-3 rounded text-sm text-gray-800" v-click>
    <b>🔒 보안</b><br>
    S3 버킷 퍼블릭 차단 유지<br>
    URL 만료로 자동 접근 차단
  </div>
  <div class="bg-green-50 p-3 rounded text-sm text-gray-800" v-click>
    <b>⚡ 성능</b><br>
    이미지 트래픽이 백엔드를 우회<br>
    S3 인프라 자원 직접 활용
  </div>
</div>

<!--
이미지 파일 저장 시 AWS S3를 연동하며 Presigned URL 방식을 채택했습니다. 백엔드는 업로드 중개자 역할만 수행하며 DB에는 원본 URL을 기록하고, 조회 시마다 1시간 동안만 유효한 임시 서명 URL을 발급해 줍니다. 이를 통해 클라이언트는 서버를 거치지 않고 S3에 직접 이미지를 요청하므로 백엔드의 부하를 없애고, S3 버킷의 퍼블릭 차단 상태를 유지해 보안성도 확보했습니다.
-->

---
layout: default
---

# 05. DevOps: GitOps 기반 CI/CD

<div class="grid grid-cols-5 items-start pt-6 gap-2">
  <div class="text-center" v-click>
    <div class="text-3xl mb-2">👨‍💻</div>
    <h4 class="font-bold text-sm text-blue-400">Developer</h4>
    <p class="text-xs text-gray-400 mt-1">main 브랜치<br>Push</p>
  </div>
  <div class="text-center text-gray-600 text-2xl pt-6" v-click>→</div>
  <div class="text-center" v-click>
    <div class="text-3xl mb-2"><carbon:settings-check class="text-orange-400"/></div>
    <h4 class="font-bold text-sm text-orange-400">Jenkins (CI)</h4>
    <div class="text-xs text-gray-300 text-left mt-2 space-y-1">
      <p>① Gradle / NPM Build</p>
      <p>② Docker Image Build</p>
      <p>③ Docker Hub Push</p>
      <p>④ Manifest Tag Update</p>
    </div>
  </div>
  <div class="text-center text-gray-600 text-2xl pt-6" v-click>→</div>
  <div class="text-center" v-click>
    <div class="text-3xl mb-2"><carbon:cloud-service-management class="text-green-400"/></div>
    <h4 class="font-bold text-sm text-green-400">ArgoCD (CD)</h4>
    <div class="text-xs text-gray-300 text-left mt-2 space-y-1">
      <p>① Manifest 변경 감지</p>
      <p>② K8s 클러스터 동기화</p>
      <p>③ Rolling Update</p>
    </div>
  </div>
</div>

<div class="mt-8 bg-blue-900/10 border border-blue-800/20 p-4 rounded-xl text-xs text-blue-300" v-click>
  <b class="text-blue-400">ngrok</b>: 로컬 K8s 환경의 Jenkins를 외부 GitHub Webhook과 연결하는 터널링 도구
</div>

<!--
운영 자동화를 위해 Jenkins와 ArgoCD를 결합한 GitOps 배포 모델을 구축했습니다. 개발자가 main 브랜치에 코드를 푸시하면, GitHub Webhook이 터널링을 통해 Jenkins를 트리거하여 빌드 및 Docker Hub 푸시를 수행합니다. 이후 매니페스트가 업데이트되면, K8s 클러스터 내의 ArgoCD가 변경을 감지하여 최신 상태로 자동 동기화하는 배포 파이프라인을 완성했습니다.
-->

---
layout: two-cols
---

# 05-1. Build: Dockerfile

<div class="text-[10px] leading-tight pr-4">

```dockerfile
# Backend Dockerfile (Multi-stage)
FROM gradle:8.5-jdk21 AS build
WORKDIR /app
COPY . .
RUN ./gradlew bootJar

FROM eclipse-temurin:21-jre
COPY --from=build /app/build/libs/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

</div>

<template v-slot:right>
<div class="pl-4 pt-10 text-[11px] space-y-4">
  <div v-click class="bg-[#161b22] border border-gray-800 p-3 rounded-lg">
    <b class="text-blue-400">Multi-stage Build</b><br>
    빌드와 실행 환경 분리 → 이미지 경량화
  </div>
  <div v-click class="bg-[#161b22] border border-gray-800 p-3 rounded-lg">
    <b class="text-green-400">JDK 21 & JRE 21</b><br>
    LTS 버전 기반의 안정적인 런타임
  </div>
</div>
</template>

---
layout: two-cols
---

# 05-2. Deploy: K8s Manifest

<div class="text-[10px] leading-tight pr-4">

```yaml
# Deployment.yaml (ArgoCD managed)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: maplog-backend
spec:
  replicas: 2
  template:
    spec:
      containers:
      - name: backend
        image: maplog/backend:${TAG}
        ports:
        - containerPort: 8080
        env:
        - name: SPRING_PROFILES_ACTIVE
          value: "dev"
```

</div>

<template v-slot:right>
<div class="pl-4 pt-10 text-[11px] space-y-4">
  <div v-click class="bg-[#161b22] border border-gray-800 p-3 rounded-lg">
    <b class="text-purple-400">GitOps (ArgoCD)</b><br>
    선언적 인프라 관리 및 자동 동기화
  </div>
  <div v-click class="bg-[#161b22] border border-gray-800 p-3 rounded-lg">
    <b class="text-orange-400">Rolling Update</b><br>
    배포 시 무중단 서비스 유지
  </div>
</div>
</template>

---
layout: default
---

# 05-3. CI/CD: Jenkins Pipeline

<div class="text-[10.5px] leading-tight pt-2">

```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') { steps { checkout scm } }
        stage('Build') { steps { sh './gradlew build' } }
        stage('Docker Push') {
            steps {
                sh "docker build -t maplog/backend:${env.BUILD_NUMBER} ."
                sh "docker push maplog/backend:${env.BUILD_NUMBER}"
            }
        }
        stage('Update Manifest') {
            steps {
                sh "sed -i 's/TAG/${env.BUILD_NUMBER}/' k8s/deploy.yaml"
                git commit: 'Update image tag', push: true
            }
        }
    }
}
```

</div>

---
layout: center
---

# 05-4. CI/CD 테스트 결과

<div class="flex flex-col items-center justify-center">
  <div class="w-full max-w-2xl border-4 border-green-500/30 rounded-2xl overflow-hidden shadow-2xl" v-click>
    <!-- Jenkins GIF Placeholder -->
    <div class="bg-black/40 aspect-video flex items-center justify-center text-green-400 font-mono">
      [ Jenkins CI/CD Success GIF Placeholder ]<br>
      (Build Success -> Docker Push -> ArgoCD Sync)
    </div>
  </div>
  <p class="mt-6 text-gray-400" v-click>성공적인 파이프라인 가동 및 무중단 배포 확인</p>
</div>

---
layout: center
---

# 🎥 시연 영상

<div class="w-full max-w-3xl aspect-video bg-black/50 rounded-2xl border border-gray-700 flex items-center justify-center shadow-2xl">
  <carbon:play-filled-alt class="text-6xl text-blue-400 opacity-80 cursor-pointer hover:scale-110 transition-transform"/>
  <p class="ml-4 text-xl font-bold">MapLog 서비스 시연 (Click to Play)</p>
</div>

<!--
시연 영상에서는 실제 동작 화면을 중심으로 보여드리겠습니다.
-->

---
layout: default
---

# 06. 주요 기능 및 실시간 알림 시스템

<div class="grid grid-cols-2 gap-8 pt-8">
  <div v-click>
    <h3 class="text-blue-500 mb-4"><carbon:map class="inline mr-2"/> Map & Markers</h3>
    <ul>
      <li>카카오 맵 SDK 직접 제어</li>
      <li>영역 변경 시 화면 내 마커만 로딩</li>
      <li>장소 검색 및 일기 작성 모달 연동</li>
    </ul>
  </div>
  <div v-click>
    <h3 class="text-yellow-500 mb-4"><carbon:notification-new class="inline mr-2"/> Real-time Notification</h3>
    <ul>
      <li><b>SSE (Server-Sent Events)</b> 채택</li>
      <li>단방향 푸시 알림 최적화</li>
      <li>HTTP 표준 기반의 가벼운 통신</li>
      <li>별도 프로토콜 업그레이드 불필요</li>
    </ul>
  </div>
</div>

<!--
카카오 맵 SDK를 직접 제어해 지도 영역 변경 시 화면 내의 마커만 효율적으로 렌더링하고 일기를 기록할 수 있습니다. 친구 추가나 요청 시 발생하는 실시간 알림은 SSE(Server-Sent Events)로 구현했습니다. WebSocket과 달리 HTTP 표준 위에서 동작하기 때문에 별도 프로토콜 업그레이드 없이 기존 보안 설정을 유지하며 가볍게 푸시 알림을 구현할 수 있었습니다.
-->

---
layout: default
---

# 07. 트러블슈팅: K8s Ingress 80포트 노출

<div class="bg-red-900/10 border border-red-800/20 p-4 border-l-4 border-l-red-500 mb-6 rounded-r-xl" v-click>
  <h4 class="font-bold text-red-400">Problem</h4>
  <p class="text-sm text-red-200/80">K8s NodePort 제한(30000-32767)으로 인해 표준 HTTP 80포트 사용 불가</p>
</div>

<div class="bg-green-900/10 border border-green-800/20 p-4 border-l-4 border-l-green-500 rounded-r-xl" v-click>
  <h4 class="font-bold text-green-400">Solution: NGINX Ingress Controller</h4>
  <ul class="text-sm text-green-200/80">
    <li><b>hostPort</b> 방식으로 노드의 80포트 직접 바인딩</li>
    <li><code>ingress.yaml</code>을 통한 경로 기반 라우팅 (<code>/api</code> → Backend:8080, <code>/</code> → Frontend:80)</li>
    <li>사용자 관점의 깔끔한 URL (<code>http://localhost</code>) 완성</li>
  </ul>
</div>

<!--
이제 개발 과정에서 겪은 핵심 트러블슈팅 사례입니다. 첫 번째는 K8s 환경에서의 서비스 포트 노출 문제입니다. 서비스 진입점을 표준 HTTP 포트인 80으로 설정하려 했으나, NodePort는 30000번대 이상의 포트만 할당 가능한 물리적 한계가 있었습니다. 이를 극복하기 위해 NGINX Ingress Controller를 클러스터에 도입했고, 호스트의 80포트를 점유하여 내부 서비스로 깔끔하게 라우팅할 수 있었습니다.
-->

---
layout: two-cols
---

# 08. 트러블슈팅: 인증 안정성 확보

<div class="text-[11px] leading-tight -mt-2">

```javascript
// api/axios.js (토큰 자동 갱신 및 재시도 로직)
let isRefreshing = false, queue = [];

// ① 요청 인터셉터: 헤더에 Access Token 자동 주입
api.interceptors.request.use(cfg => {
  const tk = localStorage.getItem('ml_access_token');
  if (tk) cfg.headers.Authorization = `Bearer ${tk}`;
  return cfg;
});

// ② 응답 인터셉터: 401 에러 감지 시 갱신 및 재시도
api.interceptors.response.use(r => r.data, async err => {
  if (err.response?.status !== 401 || err.config._retry) return Promise.reject(err);
  err.config._retry = true;

  if (isRefreshing) // 갱신 중이면 큐에서 대기 후 나중에 재시도
    return new Promise(r => queue.push(r)).then(() => api(err.config));

  isRefreshing = true;
  const tk = await callRefreshApi(); // Refresh Token으로 새 토큰 획득
  localStorage.setItem('ml_access_token', tk);
  queue.forEach(r => r()); queue = []; isRefreshing = false;

  err.config.headers.Authorization = `Bearer ${tk}`;
  return api(err.config); // 실패했던 원본 요청 재실행
});
```

</div>

<template v-slot:right>
<div class="pl-4 pt-6 space-y-4">
  <div v-click class="bg-red-900/10 border border-red-800/20 p-4 rounded-xl text-sm">
    <b class="text-red-400">문제</b><br>
    <span class="text-red-100/80">Access Token 만료 시 사용자가 영문 없이 에러를 마주하는 UX 저하</span>
  </div>
  <div v-click class="bg-orange-900/10 border border-orange-800/20 p-4 rounded-xl text-sm">
    <b class="text-orange-400">Race Condition</b><br>
    <span class="text-orange-100/80">여러 API 동시 실패 시 각 요청이 독립적으로 갱신 시도<br>
    → Refresh Token 중복 소비</span>
  </div>
  <div v-click class="bg-green-900/10 border border-green-800/20 p-4 rounded-xl text-sm">
    <b class="text-green-400">해결: isRefreshing + Queue</b><br>
    <span class="text-green-100/80">갱신 시작 시 이후 실패 요청을 Queue에 대기<br>
    갱신 완료 후 일괄 재시도 → 사용자에게 투명</span>
  </div>
</div>
</template>

<!--
두 번째는 프론트엔드 통신 과정의 최적화입니다. 토큰 만료 시 사용자가 겪는 401 에러를 방지하고자 Axios 인터셉터를 구축했고, 동시에 여러 API가 실패할 때를 대비해 실패 요청들을 큐(Queue)에 쌓아두었다가 Refresh Token 갱신 후 한 번에 재시도하도록 개선했습니다. 또한 API URL에 /api/api/가 중복으로 붙는 현상을 파악하고, 코드 내부의 /api 접두사를 전면 제거하여 환경변수(baseURL)로 일괄 관리되도록 리팩토링했습니다.
-->

---
layout: two-cols
---

# 09. 트러블슈팅: 환경별 파일 저장 전략 (@Profile)

<div class="text-[11.5px] leading-tight -mt-2">

```java
public interface FileStorageService {
    String store(MultipartFile file);
    String generatePresignedUrl(String fileUrl);
}

@Service @Profile({"local", "test"})
public class LocalFileStorageService implements FileStorageService {
    public String store(MultipartFile f) { return "/uploads/" + f.getName(); }
    public String generatePresignedUrl(String url) { return url; }
}

@Service @Profile({"dev", "aws"})
public class S3FileStorageService implements FileStorageService {
    public String store(MultipartFile f) { /* S3 upload logic */ return s3Url; }
    public String generatePresignedUrl(String url) { return s3.getPresignedUrl(url); }
}
```

</div>

<template v-slot:right>
<div class="pl-4 pt-6 space-y-4">
  <div v-click class="bg-red-900/10 border border-red-800/20 p-4 rounded-xl text-sm">
    <b class="text-red-400">문제</b><br>
    <span class="text-red-100/80">로컬 파일시스템 vs AWS S3<br>
    if 분기문은 비즈니스 로직과 인프라 로직이 혼재</span>
  </div>
  <div v-click class="bg-green-900/10 border border-green-800/20 p-4 rounded-xl text-sm">
    <b class="text-green-400">해결: 전략 패턴 + @Profile</b><br>
    <span class="text-green-100/80">서비스 레이어는 인터페이스만 주입받음<br>
    환경 변수 하나로 구현체 자동 교체</span>
  </div>
  <div v-click class="bg-[#161b22] border border-gray-800/50 p-4 rounded-xl text-xs font-mono leading-5">
    <p class="text-blue-400">local → LocalFileStorageService</p>
    <p class="text-orange-400">dev&nbsp;&nbsp; → S3FileStorageService</p>
    <p class="text-orange-400">aws&nbsp;&nbsp; → S3FileStorageService</p>
    <p class="text-gray-500 mt-1">// 서비스 코드 수정 불필요</p>
  </div>
</div>
</template>

<!--
세 번째는 파일 저장 전략입니다. 로컬과 운영 서버의 저장소가 달랐는데, if 분기문을 쓰면 비즈니스 로직 안에 인프라 판단이 섞여 복잡해집니다. 저희는 Spring의 @Profile을 활용한 전략 패턴으로 접근했습니다. FileStorageService 인터페이스를 정의하고, 로컬 전용 구현체와 S3 전용 구현체를 분리하여 서비스 코드 단 한 줄의 수정 없이 환경 변수에 따라 구현체가 자동 교체되도록 설계했습니다.
-->

---
layout: default
---

# 10. 트러블슈팅: K8s 환경 이미지 401 복합 장애

<div class="bg-red-900/10 border border-red-800/20 rounded-xl p-3 text-sm text-center font-mono mb-6 text-red-400" v-click>
  브라우저 콘솔: GET /api/uploads/xxx.png → <b>401 Unauthorized</b>
</div>

<div class="grid grid-cols-3 gap-3 text-xs">
  <div class="border-l-4 border-l-red-500 bg-red-900/10 border border-red-800/20 p-4 rounded-r-xl" v-click>
    <div class="font-bold text-red-400 mb-2">Layer 1 — Spring Profile</div>
    <p class="text-red-100/80"><code>SPRING_PROFILES_ACTIVE</code> 미지정</p>
    <p class="text-red-100/80">→ <code>LocalFileStorageService</code> 활성화</p>
    <p class="text-red-100/80">→ Pod 파일시스템 저장</p>
    <p class="text-gray-500 mt-2">(Pod 재시작 시 데이터 소실)</p>
  </div>
  <div class="border-l-4 border-l-yellow-500 bg-yellow-900/10 border border-yellow-800/20 p-4 rounded-r-xl" v-click>
    <div class="font-bold text-yellow-400 mb-2">Layer 2 — Frontend URL</div>
    <p class="text-yellow-100/80"><code>toImageUrl('/uploads/...')</code></p>
    <p class="text-yellow-100/80"><code>+ VITE_API_BASE_URL=/api</code></p>
    <p class="text-yellow-100/80">→ <code>&lt;img&gt;</code> 태그는 Axios 미경유</p>
    <p class="text-yellow-100/80">→ Authorization 헤더 누락</p>
  </div>
  <div class="border-l-4 border-l-orange-500 bg-orange-900/10 border border-orange-800/20 p-4 rounded-r-xl" v-click>
    <div class="font-bold text-orange-400 mb-2">Layer 3 — Spring Security</div>
    <p class="text-orange-100/80">허용: <code>/uploads/**</code></p>
    <p class="text-orange-100/80">실제 도달: <code>/api/uploads/**</code></p>
    <p class="text-orange-100/80">(Nginx 프록시 경로)</p>
    <p class="text-orange-100/80">→ 인증 필요로 인식 → 401</p>
  </div>
</div>

<div class="mt-6 bg-green-900/10 border border-green-800/20 p-4 rounded-xl text-sm border-l-4 border-l-green-500" v-click>
  <b class="text-green-400">✅ 해결:</b> <code class="text-green-300">@Profile({"dev","aws"})</code> 확장 및 Presigned URL 도입<br>
  <span class="text-green-100/80 mt-1 block">Nginx와 Spring Security를 우회하여 브라우저가 S3에 직접 요청하도록 개선</span>
</div>

<!--
가장 까다로웠던 복합 장애 사례입니다. K8s 배포 후 이미지가 401 권한 에러로 깨졌는데, 추적 결과 세 개의 독립된 레이어가 맞물린 문제였습니다. 첫째, 환경변수 누락으로 S3 대신 로컬 스토리지가 활성화됐고, 둘째, 프론트엔드 이미지 태그에 API 경로가 잘못 붙어 인증 헤더가 누락되었으며, 셋째, 해당 프록시 경로가 Security 설정에서 막혀 있었습니다. @Profile 매핑을 재설계하고, Presigned URL을 통해 브라우저가 직접 S3에 접근하도록 트래픽 흐름을 수정하여 근본적으로 해결했습니다.
-->

---
layout: center
class: text-center
---

# 마무리: 성과 및 향후 발전 방향

<div class="grid grid-cols-2 gap-10 text-left pt-10">
  <div v-click>
    <h3 class="border-b border-blue-900/50 text-blue-400 pb-2 mb-4">🏆 Key Achievements</h3>
    <ul class="text-sm space-y-2 opacity-90">
      <li>DevOps 전체 라이프사이클(GitOps) 체감</li>
      <li>레이어 분리 디버깅 사고방식 확보</li>
      <li>현대적 아키텍처(CQRS) 실전 적용</li>
    </ul>
  </div>
  <div v-click>
    <h3 class="border-b border-purple-900/50 text-purple-400 pb-2 mb-4">🚀 Future Roadmap</h3>
    <ul class="text-sm space-y-2 opacity-90">
      <li>방문 장소 통계 및 히트맵 시각화</li>
      <li>WebSocket 기반 실시간 위치 공유</li>
      <li>모바일 경험 확장을 위한 PWA 도입</li>
    </ul>
  </div>
</div>

<h2 class="mt-20 text-2xl font-bold italic">감사합니다!</h2>

<!--
이번 프로젝트에서 얻은 가장 큰 수확은 CI/CD 파이프라인을 직접 운영하며 DevOps 전체 흐름을 체감한 것과, 장애 발생 시 원인을 레이어별로 분리해서 분석하는 디버깅 사고방식을 기른 점입니다. 앞으로는 방문 장소 기반 히트맵 시각화와, SSE의 한계를 넘은 WebSocket 기반 실시간 위치 공유를 도입할 계획입니다. 단순한 기록을 넘어, 사용자의 삶의 지도를 완성해가는 MapLog로 계속 발전시켜 나가겠습니다. 감사합니다.
-->

---
layout: center
class: text-center
---

# <span class="ml-logo-gradient text-5xl">Q & A</span>
### 감사합니다. 질문 있으시면 편하게 말씀해 주세요!

<div class="mt-20 flex justify-center gap-10">
  <div class="flex items-center gap-2 text-gray-400">
    <carbon:logo-github class="text-2xl"/>
    <span>github.com/MapLog</span>
  </div>
  <div class="flex items-center gap-2 text-gray-400">
    <carbon:email class="text-2xl"/>
    <span>contact@maplog.com</span>
  </div>
</div>
