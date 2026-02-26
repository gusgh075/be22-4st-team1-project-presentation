# AWS S3 ↔ 백엔드 서버 연동 구조

> MapLog 프로젝트에서 파일(이미지)이 업로드되고 조회되기까지의 전체 흐름을 설명합니다.

---

## 1. 전체 구조 개요

```
[클라이언트 (Vue)]
       │
       │  multipart/form-data (이미지 파일)
       ▼
[Spring Boot 백엔드]
       │
       │  AWS SDK (putObject)
       ▼
[AWS S3 버킷: diary-side-project]
       │
       │  Presigned URL (1시간 유효)
       ▼
[클라이언트 브라우저 → S3에 직접 GET 요청]
```

백엔드는 **업로드 중개자** 역할만 수행합니다.
파일을 S3에 저장한 뒤 URL을 DB에 기록하고, 조회 시에는 해당 URL을 **Presigned URL**로 변환해 응답합니다.
클라이언트는 이미지를 볼 때 백엔드를 거치지 않고 S3에 직접 요청합니다.

---

## 2. 환경별 저장소 전략 (`@Profile`)

로컬 개발 환경과 운영 환경의 파일 저장소가 다르기 때문에, `@Profile` 어노테이션과 전략 패턴으로 구현체를 분리합니다.

### 인터페이스 (공통 계약)

```java
// common/storage/FileStorageService.java
public interface FileStorageService {
    String store(MultipartFile file);          // 파일 저장 → 저장된 URL 반환
    void delete(String fileUrl);               // 파일 삭제
    String generatePresignedUrl(String fileUrl); // 조회용 서명 URL 생성
}
```

서비스 레이어는 이 인터페이스만 의존합니다.
환경 변수(`SPRING_PROFILES_ACTIVE`) 하나로 구현체가 자동 교체되며, 비즈니스 로직은 변경이 없습니다.

### 구현체 분기

| Profile | 활성 구현체 | 저장 위치 | Presigned URL |
|---------|------------|----------|---------------|
| `local`, `test` | `LocalFileStorageService` | 서버 로컬 파일시스템 (`uploads/`) | 그대로 반환 (서명 없음) |
| `dev`, `aws` | `S3FileStorageService` | AWS S3 버킷 (`diary-side-project`) | 1시간짜리 서명 URL 생성 |

```java
// 로컬 환경
@Service
@Profile({"local", "test"})
public class LocalFileStorageService implements FileStorageService { ... }

// 운영 환경
@Service
@Profile({"dev", "aws"})
public class S3FileStorageService implements FileStorageService { ... }
```

---

## 3. S3 클라이언트 빈 설정

`S3Config`는 `@Profile({"dev", "aws"})` 환경에서만 `AmazonS3` 빈을 등록합니다.

```java
// common/config/S3Config.java
@Configuration
@Profile({"dev", "aws"})
public class S3Config {

    @Value("${cloud.aws.credentials.access-key}") private String accessKey;
    @Value("${cloud.aws.credentials.secret-key}") private String secretKey;
    @Value("${cloud.aws.region.static}")           private String region;

    @Bean
    public AmazonS3 amazonS3() {
        BasicAWSCredentials creds = new BasicAWSCredentials(accessKey, secretKey);
        return AmazonS3ClientBuilder.standard()
                .withRegion(region)                                  // ap-southeast-2 (시드니)
                .withCredentials(new AWSStaticCredentialsProvider(creds))
                .build();
    }
}
```

설정 값은 `application-aws.yml`에서 주입됩니다.

```yaml
# application-aws.yml
cloud:
  aws:
    s3:
      bucket: diary-side-project
    region:
      static: ap-southeast-2
    credentials:
      access-key: ${AWS_ACCESS_KEY}
      secret-key: ${AWS_SECRET_KEY}
```

---

## 4. 업로드 흐름 (Write)

### 4-1. 일기 이미지 업로드

```
POST /api/diaries  (multipart/form-data)
```

```
클라이언트
  │  images: [file1, file2, ...]
  ▼
DiaryCommandController
  │  @RequestPart("images") List<MultipartFile> images
  ▼
DiaryCommandService.createDiary()
  │
  ├─ Diary 엔티티 저장 (JPA)
  │
  └─ 이미지별 반복
       │  fileStorageService.store(file)
       ▼
       S3FileStorageService.store()
         │  1. UUID로 고유 키 생성: "diaries/{UUID}.jpg"
         │  2. ObjectMetadata 설정 (ContentType, ContentLength)
         │  3. amazonS3.putObject(bucket, key, inputStream, metadata)
         │  4. amazonS3.getUrl(bucket, key) → S3 퍼블릭(원본) URL 반환
         ▼
       DiaryImage 엔티티에 URL 저장 (JPA)
```

**핵심 코드 (`S3FileStorageService.store`)**

```java
@Override
public String store(MultipartFile file) {
    String key = "diaries/" + UUID.randomUUID() + extension;   // 고유 키

    ObjectMetadata metadata = new ObjectMetadata();
    metadata.setContentType(file.getContentType());
    metadata.setContentLength(file.getSize());

    amazonS3.putObject(bucket, key, file.getInputStream(), metadata); // S3에 저장

    return amazonS3.getUrl(bucket, key).toString(); // DB에 저장될 원본 URL
    // 예: https://diary-side-project.s3.ap-southeast-2.amazonaws.com/diaries/uuid.jpg
}
```

> DB에는 S3 **원본 URL**이 저장됩니다. Presigned URL은 조회 시점마다 새로 생성합니다.

### 4-2. 프로필 이미지 업로드

```
PATCH /api/users/me  (multipart/form-data)
```

```
UserCommandController
  │  fileStorageService.store(profileImage)  ← 컨트롤러 레이어에서 직접 처리
  │  → profileImageUrl (S3 원본 URL)
  ▼
UserCommandService.updateProfile(email, nickname, profileImageUrl)
  │  User 엔티티의 profileImageUrl 필드 업데이트 (JPA)
```

---

## 5. 조회 흐름 (Read) — Presigned URL 변환

S3 버킷은 **퍼블릭 액세스 차단** 상태입니다.
원본 URL로 직접 접근하면 403이 발생하므로, 조회 시마다 서명된 임시 URL을 생성해 응답합니다.

```
클라이언트
  │  GET /api/diaries/{id}
  ▼
DiaryQueryService.getDiaryDetail()
  │
  ├─ DB에서 DiaryDetailResponse 조회 (MyBatis)
  │    → images[].imageUrl = "https://...s3.../diaries/uuid.jpg"  (원본 URL)
  │
  └─ 이미지별 Presigned URL 변환
       │  fileStorageService.generatePresignedUrl(originalUrl)
       ▼
       S3FileStorageService.generatePresignedUrl()
         │  1. 원본 URL에서 S3 키 추출
         │  2. 만료 시간 = 현재 + 1시간
         │  3. amazonS3.generatePresignedUrl(bucket, key, expiration, GET)
         ▼
       "https://...s3.../diaries/uuid.jpg?X-Amz-Algorithm=...&X-Amz-Expires=3600&..."
  ▼
클라이언트 브라우저
  │  <img src="Presigned URL"> → S3에 직접 GET
  ▼
AWS S3 (백엔드 우회, 직접 응답)
```

**핵심 코드 (`S3FileStorageService.generatePresignedUrl`)**

```java
@Override
public String generatePresignedUrl(String fileUrl) {
    String key = extractKey(fileUrl);          // URL에서 S3 키 추출

    Date expiration = new Date();
    expiration.setTime(expiration.getTime() + 1000 * 60 * 60); // 1시간 후 만료

    return amazonS3.generatePresignedUrl(bucket, key, expiration, HttpMethod.GET)
                   .toString();
}

private String extractKey(String fileUrl) {
    // "https://bucket.s3.region.amazonaws.com/diaries/uuid.jpg"
    //                                         ^^^^^^^^^^^^^^^^^^^ 이 부분 추출
    return fileUrl.substring(fileUrl.lastIndexOf(".com/") + 5);
}
```

프로필 이미지 조회도 동일한 방식으로 `UserQueryService`에서 처리합니다.

```java
// UserQueryService.java
public UserProfileQueryResponse getMyProfile(String email) {
    UserProfileQueryResponse response = userQueryMapper.findMyProfile(email);

    if (response.getProfileImageUrl() != null) {
        response.setProfileImageUrl(
            fileStorageService.generatePresignedUrl(response.getProfileImageUrl())
        );
    }
    return response;
}
```

---

## 6. 삭제 흐름 (Delete)

일기 수정 시 특정 이미지를 삭제하면 S3와 DB 양쪽 모두 정리됩니다.

```java
// DiaryCommandService.updateDiary()
deleteImageIds.forEach(imgId -> {
    diaryImageRepository.findById(imgId).ifPresent(img -> {
        fileStorageService.delete(img.getImageUrl()); // S3에서 삭제
        diaryImageRepository.delete(img);              // DB에서 삭제
    });
});
```

```java
// S3FileStorageService.delete()
@Override
public void delete(String fileUrl) {
    if (fileUrl == null || !fileUrl.contains(bucket)) return;
    String key = extractKey(fileUrl);
    amazonS3.deleteObject(bucket, key); // S3 오브젝트 삭제
}
```

---

## 7. S3를 직접 사용하는 이유 — Presigned URL의 이점

| 항목 | 백엔드 경유 방식 | Presigned URL 방식 (현재) |
|------|----------------|--------------------------|
| 이미지 전송 경로 | 클라이언트 → 백엔드 → 클라이언트 | 클라이언트 → S3 직접 |
| 백엔드 부하 | 이미지 트래픽이 백엔드에 집중 | 이미지 트래픽 없음 |
| S3 버킷 보안 | 퍼블릭 공개 필요 | 퍼블릭 차단 유지 가능 |
| URL 유효 기간 | 영구 | 1시간 (만료 후 자동 무효화) |
| 구현 복잡도 | 단순 | AWS SDK 필요 |

---

## 8. 전체 데이터 흐름 요약

```
[업로드]
클라이언트 → (multipart) → Spring Boot → (putObject) → S3
                                  │
                          S3 원본 URL → DB 저장

[조회]
클라이언트 → Spring Boot → DB에서 원본 URL 조회
                  │
          generatePresignedUrl() → Presigned URL 생성
                  │
          응답에 Presigned URL 포함
                  │
클라이언트 → (GET) → S3 직접 요청 → 이미지 수신

[삭제]
클라이언트 → Spring Boot → S3 deleteObject + DB delete
```
