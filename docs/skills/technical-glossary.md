# MapLog 기술 용어 해설집 (Technical Glossary)

프로젝트 발표 자료(`slides.md`) 및 문서 전반에 사용된 기술 용어들을 정리한 해설집입니다.

---

## 🎨 Frontend
- **Vue 3 (Composition API)**: 사용자 인터페이스를 구축하기 위한 프로그레시브 프레임워크입니다. Composition API를 사용해 로직을 유연하게 결합하고 재사용할 수 있습니다.
- **Pinia**: Vue.js를 위한 최신 상태 관리 라이브러리로, 인증 상태나 알림 데이터 등을 중앙에서 관리하는 저장소 역할을 합니다.
- **Vite**: 매우 빠른 빌드 속도를 제공하는 프론트엔드 빌드 도구이자 번들러입니다.
- **Axios**: 브라우저와 Node.js를 위한 Promise 기반 HTTP 클라이언트입니다. 백엔드 API와의 통신에 사용됩니다.
- **Axios Interceptor**: 요청이나 응답이 처리되기 전에 가로채어 로직을 삽입하는 기능입니다. 본 프로젝트에서는 토큰 자동 갱신 로직에 활용되었습니다.
- **Kakao Maps SDK**: 카카오에서 제공하는 지도 서비스 개발 도구로, 지도 렌더링 및 마커 표시 기능을 담당합니다.
- **SSE (Server-Sent Events)**: 서버에서 클라이언트로 실시간 데이터를 푸시하기 위한 단방향 통신 기술입니다. 실시간 알림 시스템에 사용되었습니다.
- **WebSocket**: 서버와 클라이언트 간의 양방향 실시간 통신을 지원하는 프로토콜입니다. (향후 로드맵 항목)
- **PWA (Progressive Web App)**: 웹 앱을 모바일 앱처럼 사용할 수 있게 해주는 기술로, 오프라인 지원 및 설치 기능을 제공합니다. (향후 로드맵 항목)

## ☕ Backend
- **Spring Boot 3.5**: 자바 기반의 독립 실행형 애플리케이션을 빠르게 개발할 수 있게 도와주는 프레임워크입니다.
- **Java 21**: 자바의 최신 LTS(Long Term Support) 버전으로, 가상 스레드 등을 통한 성능 향상을 지원합니다.
- **Spring Data JPA**: 인터페이스 기반으로 데이터베이스를 쉽게 다룰 수 있게 해주는 ORM(Object-Relational Mapping) 기술입니다. (Command 측 사용)
- **MyBatis**: SQL 쿼리를 직접 작성하여 객체와 매핑할 수 있게 해주는 SQL Mapping 프레임워크입니다. (Query 측 사용)
- **JWT (JSON Web Token)**: 당사자 간에 정보를 JSON 객체로 안전하게 전송하기 위한 컴팩트하고 독립적인 방식의 인증 토큰입니다.
- **CQRS (Command Query Responsibility Segregation)**: 명령(상태 변경)과 조회(읽기)의 책임을 분리하는 아키텍처 패턴입니다.
- **DTO (Data Transfer Object)**: 계층 간 데이터 교환을 위해 사용하는 객체입니다.
- **RBAC (Role-Based Access Control)**: 사용자 역할에 따라 권한을 부여하고 제어하는 보안 모델입니다. (관리자 기능에 적용)

## 🏗 Infrastructure & DevOps
- **Docker**: 애플리케이션을 컨테이너라는 가볍고 독립된 환경으로 격리하여 실행하는 기술입니다.
- **Multi-stage Build**: Docker 빌드 시 여러 단계(Build -> Run)를 거쳐 최종 이미지 크기를 경량화하는 기법입니다.
- **Kubernetes (K8s)**: 컨테이너화된 애플리케이션의 배포, 확장 및 관리를 자동화하는 오케스트레이션 툴입니다.
- **Pod**: 쿠버네티스에서 생성하고 관리할 수 있는 배포 가능한 가장 작은 컴퓨팅 단위입니다.
- **NodePort**: 클러스터 외부의 트래픽을 서비스로 전달하기 위해 노드의 특정 포트를 개방하는 서비스 타입입니다.
- **NGINX Ingress Controller**: 외부 요청(HTTP/HTTPS)을 클러스터 내부의 서비스로 라우팅하는 규칙을 관리하는 도구입니다.
- **Jenkins**: 지속적 통합(CI)을 위한 오픈소스 자동화 서버로, 빌드 및 테스트 과정을 자동화합니다.
- **ArgoCD**: 지속적 배포(CD)를 위한 GitOps 도구로, Git 레포지토리와 K8s 클러스터의 상태를 동기화합니다.
- **GitOps**: Git 레포지토리를 '단일 진실 공급원'으로 삼아 인프라와 배포를 자동화하는 운영 방식입니다.
- **Manifest Repository**: K8s 설정(YAML 파일)을 별도로 관리하여 인프라의 형상을 추적하는 전용 저장소입니다.
- **Rolling Update**: 배포 시 기존 인스턴스를 하나씩 새로운 버전으로 교체하여 무중단 서비스를 유지하는 방식입니다.
- **ngrok**: 로컬 서버를 외부 인터넷에서 접속 가능한 공인 URL로 노출해주는 보안 터널링 서비스입니다.
- **K8s Secret**: 비밀번호, 토큰, 키 등 민감한 정보를 안전하게 보관하고 관리하는 쿠버네티스 객체입니다.

## ☁️ Cloud & Security
- **AWS S3 (Simple Storage Service)**: 확장성, 데이터 가용성 및 보안을 제공하는 클라우드 객체 스토리지 서비스입니다.
- **Presigned URL**: AWS S3 버킷에 대한 한시적인 접근 권한을 부여하는 서명된 URL입니다. 서버 부하 분산과 보안을 동시에 챙길 수 있습니다.

## 📐 Design Patterns & Concepts
- **Strategy Pattern (전략 패턴)**: 실행 중에 알고리즘(구현체)을 선택할 수 있게 하는 디자인 패턴입니다. (파일 저장소 교체에 사용)
- **@Profile (Spring Profile)**: 로컬, 개발, 운영 등 환경에 따라 다른 설정이나 빈(Bean)을 로드할 수 있게 해주는 기능입니다.
- **Race Condition**: 두 개 이상의 프로세스/스레드가 공유 자원에 동시에 접근할 때 결과값에 영향을 주는 경쟁 상태입니다. (인증 갱신 이슈에서 발생)
- **SSOT (Single Source of Truth)**: 모든 데이터와 설정이 하나의 마스터 저장소(Git 등)를 기준으로 관리되어야 한다는 '단일 진실 공급원' 원칙입니다.
- **MVP (Minimum Viable Product)**: 핵심 기능만을 포함하여 빠르게 출시하고 검증하는 '최소 기능 제품' 개발 방식입니다.
