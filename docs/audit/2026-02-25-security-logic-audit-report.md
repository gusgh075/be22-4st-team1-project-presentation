# MapLog System Security & Logic Audit Report (2026-02-25)

## 1. Executive Summary
전체 시스템의 보안 및 비즈니스 로직을 점검한 결과, 전반적으로 **JPA를 활용한 권한 검증 및 전역 예외 처리 체계가 안정적으로 구축**되어 있음을 확인했습니다. 하지만 인증 필터에서의 예외 전파 방식과 정적 리소스 접근 권한 설정에서 개선이 필요한 포인트가 발견되었습니다.

## 2. Audit Findings

### [Major] JWT 인증 실패 시 예외 전파 미흡
- **위치**: `JwtAuthenticationFilter.java`
- **현상**: 토큰 검증 과정에서 `BusinessException`(EXPIRED_TOKEN, INVALID_TOKEN)이 발생해도 catch 블록에서 로그만 남기고 필터 체인을 계속 진행함.
- **영향**: 인증이 필요한 API 호출 시, 사용자는 토큰이 왜 잘못되었는지 구체적인 정보를 알기 어렵고 단순히 Spring Security의 기본 401 에러(인증되지 않음)만 받게 될 가능성이 높음.
- **권장 사항**: 인증 실패 사유를 `HttpServletResponse`에 직접 쓰거나, SecurityContext에 관련 예외를 기록하여 `AuthenticationEntryPoint`에서 세밀하게 처리하도록 개선 필요.

### [Minor] 정적 리소스(/uploads/**) 전역 공개
- **위치**: `SecurityConfig.java`
- **현상**: 업로드된 이미지 파일 경로가 `permitAll()`로 설정되어 있음.
- **영향**: 사용자의 개인적인 일기 사진이라도 파일 URL만 알면 인증 없이 누구나 접근 가능함.
- **권장 사항**: 이미지 조회 시에도 최소한의 인증(JWT 확인)을 거치도록 필터를 적용하거나, 서비스 요구사항에 따라 접근 제어 수준을 재검토할 것을 권장.

### [Low] CORS 허용 범위 관리
- **위치**: `SecurityConfig.java`
- **현상**: `*.ngrok-free.dev` 등 와일드카드 패턴 허용.
- **영향**: 개발 편의성은 높으나 운영 환경에서는 피싱 등 보안 공격의 통로가 될 수 있음.
- **권장 사항**: 운영 환경 프로필(`application-aws.yml` 등)에서는 허용할 도메인을 명확히 한정할 것.

## 3. Positive Observations
- **데이터 소유권 검증**: `DiaryCommandService`에서 `isOwner()` 체크를 통해 타인의 데이터 수정을 완벽히 차단하고 있음.
- **일관된 응답 규격**: `ApiResponse<T>`와 `GlobalExceptionHandler`를 통해 모든 예외 상황이 동일한 규격으로 클라이언트에 전달됨.
- **CQRS 구조**: Command와 Query 패키지가 분리되어 있어 복잡한 비즈니스 로직과 단순 조회 로직의 간섭이 최소화됨.

## 4. Conclusion
MapLog 프로젝트는 핵심 비즈니스 로직의 안정성이 높으며, 위에 언급된 **JWT 예외 전파 방식**과 **리소스 접근 제어** 부분만 보완한다면 더욱 견고한 시스템이 될 것으로 판단됩니다.
