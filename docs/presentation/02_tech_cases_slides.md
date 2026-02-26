# 핵심 트러블슈팅 슬라이드 — 사례 1·2·3

> 발표 4번 섹션 "핵심 트러블슈팅: 기술적 도전과 해결" 中
> 각 사례당 약 1분 분량

---

## 사례 1 — 인증 시스템 (Axios Interceptor)

### 헤드라인

> **"토큰 만료를 사용자 몰래 해결하다 — Axios Interceptor 기반 자동 갱신 & 요청 큐"**

---

### 슬라이드 코드 (핵심 로직만, 축약)

```javascript
// api/axios.js
let isRefreshing = false, queue = []

// ① 모든 요청에 Access Token 자동 첨부
api.interceptors.request.use(config => {
  const token = localStorage.getItem('ml_access_token')
  if (token) config.headers.Authorization = `Bearer ${token}`
  return config
})

// ② 401 응답 → 토큰 갱신 → 대기 중인 요청 일괄 재시도
api.interceptors.response.use(res => res.data, async error => {
  if (error.response?.status !== 401 || error.config._retry)
    return Promise.reject(error)

  error.config._retry = true
  if (isRefreshing)                              // 갱신 진행 중 → 큐 대기
    return new Promise(r => queue.push(r)).then(() => api(error.config))

  isRefreshing = true
  const newToken = await callRefreshApi()        // Refresh Token으로 재발급
  localStorage.setItem('ml_access_token', newToken)
  queue.forEach(r => r()); queue = []; isRefreshing = false

  error.config.headers.Authorization = `Bearer ${newToken}`
  return api(error.config)                       // 원본 요청 재시도
})
```

---

### 1분 발표 스크립트

Access Token 만료 시간을 30분으로 설정하면서 한 가지 문제가 생겼습니다.
토큰이 만료된 순간 API 호출이 갑자기 실패해 사용자가 영문도 모른 채 오류를 마주하는 경험이었습니다.

이를 해결하기 위해 Axios 인터셉터를 두 단계로 구성했습니다.
요청 인터셉터에서는 모든 API 호출에 Access Token을 자동으로 첨부합니다.
응답 인터셉터에서는 401 에러가 오면, Refresh Token으로 새 토큰을 발급받고 원본 요청을 재시도합니다.

하지만 한 화면에서 여러 API가 동시에 터지는 상황이 있었습니다.
이때 모든 요청이 각자 갱신을 시도하면 Refresh Token이 중복 소비되는 문제가 발생합니다.

이를 위해 `isRefreshing` 플래그와 `queue` 배열을 도입했습니다.
갱신이 시작되면 이후 들어오는 실패 요청들을 큐에 쌓아두고,
갱신이 완료되는 순간 큐에 있는 요청들을 한 번에 재시도합니다.

결과적으로 사용자는 토큰 만료를 전혀 인식하지 못한 채 서비스를 끊김 없이 사용할 수 있게 됐습니다.

---
---

## 사례 2 — 환경별 파일 저장 전략 (@Profile)

### 헤드라인

> **"if 분기 대신 @Profile — 환경이 바뀌어도 코드는 그대로"**

---

### 슬라이드 코드 (핵심 로직만, 축약)

**① 공통 인터페이스 — 저장소 구현을 숨기는 추상화 경계**

```java
// common/storage/FileStorageService.java
public interface FileStorageService {
    String store(MultipartFile file);
    void delete(String fileUrl);
    String generatePresignedUrl(String fileUrl);
}
```

**② 로컬 개발 환경 (`@Profile("local")`) — 파일시스템 저장**

```java
@Service
@Profile("local")                              // spring.profiles.active=local 일 때만 등록
public class LocalFileStorageService implements FileStorageService {

    public String store(MultipartFile file) {
        String name = UUID.randomUUID() + getExt(file);
        file.transferTo(Paths.get(uploadDir, name).toFile());
        return "/uploads/" + name;             // 로컬 경로 반환 (서명 불필요)
    }

    public String generatePresignedUrl(String url) { return url; }
}
```

**③ 운영 환경 (`@Profile({"dev","aws"})`) — AWS S3 저장**

```java
@Service
@Profile({"dev", "aws"})                       // spring.profiles.active=dev|aws 일 때 등록
public class S3FileStorageService implements FileStorageService {

    public String store(MultipartFile file) {
        String key = "diaries/" + UUID.randomUUID() + getExt(file);
        s3.putObject(bucket, key, file.getInputStream(), metadata);
        return s3.getUrl(bucket, key).toString();
    }

    public String generatePresignedUrl(String url) {
        Date exp = new Date(System.currentTimeMillis() + 3_600_000L); // 1시간
        return s3.generatePresignedUrl(bucket, extractKey(url), exp, GET).toString();
    }
}
```

> **사용 측 (DiaryCommandService)**
> `private final FileStorageService fileStorageService;`
> → 인터페이스만 주입받으면 끝. 환경이 바뀌어도 이 코드는 손댈 필요 없음.

---

### 1분 발표 스크립트

개발 환경과 운영 환경 간의 파일 저장소 문제를 해결한 사례입니다.
로컬에서는 로컬 파일 시스템을 쓰고, 운영 서버에서는 AWS S3를 사용해야 했는데,
처음에는 `if (profile.equals("aws"))` 같은 분기문을 생각했습니다.

하지만 이 방식은 저장 로직 안에 환경 판단이 섞여 코드가 복잡해지고,
구현체가 늘어날수록 조건 분기가 쌓이는 문제가 있습니다.

저희는 Spring의 `@Profile` 어노테이션을 활용한 전략 패턴으로 접근했습니다.
`FileStorageService` 인터페이스를 정의하고, 구현체를 환경별로 분리했습니다.
`LocalFileStorageService`는 `@Profile("local")`로 로컬에서만,
`S3FileStorageService`는 `@Profile({"dev","aws"})`로 운영 환경에서만 Spring Bean으로 등록됩니다.

서비스 레이어는 `FileStorageService` 인터페이스만 주입받습니다.
환경 변수 하나만 바꾸면 구현체가 자동으로 교체되고,
다이어리를 저장하는 코드는 단 한 줄도 수정할 필요가 없습니다.

덕분에 로컬·개발·운영 세 환경을 하나의 코드베이스로 완전히 커버할 수 있었습니다.

---
---

## 사례 3 — K8s 환경 이미지 401 권한 이슈

### 헤드라인

> **"에러는 하나인데 원인은 셋 — 레이어별로 쪼개야 보이는 복합 장애"**

---

### 슬라이드 에러 흐름도 (3-레이어 연쇄 구조)

```
[증상]
브라우저 콘솔: GET /api/uploads/xxx.png → 401 Unauthorized

──────────────────────────────────────────────────────────────
Layer 1 │ Spring Profile 활성화 오류
        │  SPRING_PROFILES_ACTIVE 미지정
        │  → LocalFileStorageService 활성화 (S3 대신)
        │  → 이미지가 Pod 로컬 파일시스템에 저장됨
──────────────────────────────────────────────────────────────
Layer 2 │ 프론트엔드 URL 조합 오류
        │  toImageUrl("/uploads/uuid.png")
        │  + VITE_API_BASE_URL=/api
        │  → <img src="/api/uploads/uuid.png">
        │  → <img> 태그는 Axios 인터셉터 미경유
        │  → Authorization 헤더 없음
──────────────────────────────────────────────────────────────
Layer 3 │ Spring Security 허용 경로 불일치
        │  permitAll → /uploads/**   ← 실제 요청 경로와 불일치
        │  인증 필요 → /api/uploads/** ← Nginx 프록시 후 실제 도달 경로
──────────────────────────────────────────────────────────────

[해결]
@Profile("aws")  →  @Profile({"dev", "aws"}) 로 확장
이미지 → S3 저장 & Presigned URL 제공
→ Nginx·Spring Security 완전 우회, 브라우저가 S3에 직접 요청
```

---

### 1분 발표 스크립트

K8s 배포 후 일기 상세 페이지에서 이미지가 전혀 로드되지 않고,
브라우저 콘솔에 `GET /api/uploads/xxx.png 401 Unauthorized`가 찍혔습니다.

원인을 추적해 보니 세 개의 레이어가 동시에 맞물린 복합 장애였습니다.

첫 번째로, K8s 배포 시 `SPRING_PROFILES_ACTIVE` 환경변수가 지정되지 않아
S3가 활성화돼야 할 환경에서 `LocalFileStorageService`가 동작하고 있었습니다.

두 번째로, 프론트엔드의 `toImageUrl` 함수가 `/uploads/uuid.png`에
`VITE_API_BASE_URL=/api`를 붙여 `/api/uploads/uuid.png`를 만들어냈는데,
`<img>` 태그는 Axios 인터셉터를 거치지 않아 Authorization 헤더가 전혀 없었습니다.

세 번째로, Spring Security 설정에서 `permitAll`은 `/uploads/**`만 열려 있었고
Nginx가 프록시한 실제 경로인 `/api/uploads/**`는 인증 필요로 막혀 있었습니다.

해결은 `@Profile` 매핑을 재설계하는 것이었습니다.
`S3FileStorageService`의 프로필을 `"aws"`에서 `{"dev", "aws"}`로 확장하자
이미지가 S3에 저장되고 Presigned URL로 제공되면서
Nginx와 Spring Security를 완전히 우회하게 됐습니다.

이 경험으로 `spring.config.import`로 설정값을 로드하는 것과
`@Profile`로 Bean을 활성화하는 것이 완전히 별개의 레이어라는 점을 명확히 이해했습니다.
