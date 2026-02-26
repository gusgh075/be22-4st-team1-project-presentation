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

# <span class="ml-logo-gradient" v-motion :initial="{ y: 50, opacity: 0 }" :enter="{ y: 0, opacity: 1, transition: { duration: 1000 } }">MapLog</span>
### <span v-motion :initial="{ y: 20, opacity: 0 }" :enter="{ y: 0, opacity: 1, transition: { duration: 800, delay: 400 } }">공간의 맥락을 기록하는 소셜 다이어리</span>

<p v-motion :initial="{ opacity: 0 }" :enter="{ opacity: 1, transition: { duration: 1000, delay: 1000 } }" class="text-gray-400 mt-2">나의 발자취를 지도 위에 기록하고 공유하는 서비스</p>

<div class="pt-12" v-motion :initial="{ opacity: 0, scale: 0.8 }" :enter="{ opacity: 1, scale: 1, transition: { duration: 500, delay: 1500 } }">
  <span @click="$slidev.nav.next" class="px-4 py-2 rounded-full border border-blue-500/30 cursor-pointer hover:bg-blue-500/10 transition-all duration-300">
    발표 시작 <carbon:arrow-right class="inline ml-1"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2" v-motion :initial="{ x: 50, opacity: 0 }" :enter="{ x: 0, opacity: 1, transition: { delay: 2000 } }">
  <button @click="$slidev.nav.prev" class="text-xl icon-btn opacity-50 !border-none hover:opacity-100"><carbon:chevron-left/></button>
  <button @click="$slidev.nav.next" class="text-xl icon-btn opacity-50 !border-none hover:opacity-100"><carbon:chevron-right/></button>
</div>

<!--
안녕하세요. MapLog 팀입니다.
저희 서비스를 한 문장으로 소개하자면, "나의 발자취를 지도 위에 기록하고, 친구와 공유하는 위치 기반 소셜 다이어리" 입니다.
기술적 한계를 극복하며 구축한 전체 아키텍처와 치열했던 트러블슈팅 경험을 중심으로 발표를 시작하겠습니다.
-->

---
layout: center
class: text-left
---

# Table of Contents

<v-clicks>
<div class="space-y-4 text-2xl">
  <div class="flex items-center gap-4 text-blue-400 font-bold scale-110">
    <carbon:checkbox-checked-filled />
    <span>01. 프로젝트 개요</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox />
    <span>02. 기술 아키텍처</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox />
    <span>03. 백엔드 설계</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox />
    <span>04. 인프라 운영</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox />
    <span>05. 서비스 주요 기능</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox />
    <span>06. 핵심 트러블슈팅</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox />
    <span>07. 마무리 및 회고</span>
  </div>
</div>
</v-clicks>

---
layout: default
---

# 01-1. 팀 소개 및 역할 분담

<div class="grid grid-cols-2 gap-4 pt-4 text-left">
  <div v-click="1" v-motion :initial="{ x: -100, opacity: 0 }" :enter="{ x: 0, opacity: 1 }" class="border border-gray-800 bg-[#161b22] p-4 rounded-xl shadow-lg hover:border-blue-500/50 transition-all">
    <h3 class="text-blue-400 mb-2 flex items-center gap-2"><carbon:cloud-service-management/> 정현호 (CI/CD 총괄)</h3>
    <ul class="opacity-90 text-[10px] space-y-1">
      <li>• <b>파이프라인 설계/운영</b>: Jenkins & ArgoCD 전체 자동화 구조 설계</li>
      <li>• <b>브랜치 전략 수립</b>: GitFlow 기반 배포 전략 및 자동화 기준 확립</li>
      <li>• <b>안정성 확보</b>: 배포 실패 시 롤백 기준 수립 및 배포 자동화 구현</li>
    </ul>
  </div>
  <div v-click="2" v-motion :initial="{ x: 100, opacity: 0 }" :enter="{ x: 0, opacity: 1 }" class="border border-gray-800 bg-[#161b22] p-4 rounded-xl shadow-lg hover:border-green-500/50 transition-all">
    <h3 class="text-green-400 mb-2 flex items-center gap-2"><carbon:rocket/> 김태형 (CI/CD 서포트)</h3>
    <ul class="opacity-90 text-[10px] space-y-1">
      <li>• <b>파이프라인 보강</b>: CI 테스트 및 이미지 빌드 단계 고도화 지원</li>
      <li>• <b>인프라 관리</b>: 환경변수 및 K8s Secret 관리, 배포 스크립트 보조</li>
      <li>• <b>운영 지원</b>: 모니터링 연동 및 배포 파이프라인 트러블슈팅 지원</li>
    </ul>
  </div>
  <div v-click="3" v-motion :initial="{ y: 50, opacity: 0 }" :enter="{ y: 0, opacity: 1 }" class="border border-gray-800 bg-[#161b22] p-4 rounded-xl shadow-lg hover:border-orange-500/50 transition-all">
    <h3 class="text-orange-400 mb-2 flex items-center gap-2"><carbon:document/> 김선일 (서기 · 문서화)</h3>
    <ul class="opacity-90 text-[10px] space-y-1">
      <li>• <b>히스토리 관리</b>: 회의록 작성 및 프로젝트 전체 진행 상황 기록</li>
      <li>• <b>기술 문서화</b>: 아키텍처 다이어그램 및 배포 흐름 가이드 정리</li>
      <li>• <b>발표 지원</b>: API 명세서 정리 및 최종 발표 자료 시각화/정리</li>
    </ul>
  </div>
  <div v-click="4" v-motion :initial="{ scale: 0.8, opacity: 0 }" :enter="{ scale: 1, opacity: 1 }" class="border border-gray-800 bg-[#161b22] p-4 rounded-xl shadow-lg hover:border-purple-500/50 transition-all">
    <h3 class="text-purple-400 mb-2 flex items-center gap-2"><carbon:code/> 정병진 (Fullstack Dev)</h3>
    <ul class="opacity-90 text-[10px] space-y-1">
      <li>• <b>기능 구현</b>: Backend & Frontend 핵심 비즈니스 로직 개발</li>
      <li>• <b>시스템 통합</b>: 배포 친화적 코드 구조 개선 및 API 연동 최적화</li>
      <li>• <b>품질 검증</b>: 테스트 코드 작성 및 시연 시나리오 기반 기능 완성</li>
    </ul>
  </div>
</div>

<!--
저희 팀원들을 소개하겠습니다. 모든 팀원이 기술적 도전과 운영 자동화에 깊이 몰입했습니다.

김선일 님은 독감으로 아쉬운 상황 속에서도 도커부터 쿠버네티스까지 이어지는 파이프라인을 직접 경험하며 운영의 중요성을 깨달았습니다.
김태형 님은 Jenkins와 ArgoCD를 연결하며 코드 Push가 실제 서비스로 배포되는 GitOps의 전체 흐름을 완벽히 체감했습니다.
정병진 님은 CI/CD 파이프라인을 직접 설계하고 무중단 배포를 구현하며 DevOps 엔지니어로서의 값진 경험을 쌓았습니다.
마지막으로 정현호 님은 웹훅 자동화와 S3 연동을 통해 개발 효율성을 극대화했으며, 신기술을 대하는 성숙한 태도로 MVP 배포 프로세스를 확립했습니다.
-->

---
layout: default
---

# 01-2. 기획 배경 및 문제 정의

<div class="grid grid-cols-2 gap-10 pt-10">
  <div v-click>
    <h3 class="text-red-500 mb-4 flex items-center gap-2"><carbon:warning/> Pain Point</h3>
    <p class="text-lg mb-2">기존 SNS 및 다이어리 앱의 한계</p>
    <ul class="space-y-2 opacity-80">
      <li>"언제" 기준의 시간 순 나열 위주</li>
      <li>"어디서"라는 <b>공간적 맥락</b> 부재</li>
      <li>장소 중심의 추억 회상이 어려움</li>
    </ul>
  </div>
  <div v-click>
    <h3 class="text-green-500 mb-4 flex items-center gap-2"><carbon:idea/> Solution</h3>
    <p class="text-lg mb-2">MapLog의 차별화된 경험</p>
    <ul class="space-y-2 opacity-80">
      <li><b>지도를 인터페이스 전면</b>에 배치</li>
      <li>지도 위 핀(Marker)을 통한 발자취 시각화</li>
      <li>위치 정보를 결합한 직관적인 기록과 소통</li>
    </ul>
  </div>
</div>

<!--
여러분은 여행을 다녀온 뒤 기억을 어떻게 남기시나요? 인스타그램이나 메모 앱은 "언제" 기준의 시간 순 나열에는 강하지만, "어디서"라는 공간적 맥락을 직관적으로 보여주지 못합니다. 
내가 작년에 갔던 그 카페, 친구와 방문한 그 골목을 다시 찾고 싶을 때 텍스트만으로는 한계가 있죠.
그래서 저희는 지도를 전면에 배치해, 사용자가 핀을 꽂으며 발자취를 시각화하고 공간 기록을 공유하는 서비스를 설계했습니다.
-->

---
layout: center
class: text-left
---

# Table of Contents

<div class="space-y-4 text-2xl">
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox-checked />
    <span>01. 프로젝트 개요</span>
  </div>
  <div class="flex items-center gap-4 text-blue-400 font-bold scale-110">
    <carbon:checkbox-checked-filled />
    <span>02. 기술 아키텍처</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox />
    <span>03. 백엔드 설계</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox />
    <span>04. 인프라 운영</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox />
    <span>05. 서비스 주요 기능</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox />
    <span>06. 핵심 트러블슈팅</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox />
    <span>07. 마무리 및 회고</span>
  </div>
</div>

---
layout: default
---

# 02. 기술 스택 및 서비스 아키텍처

<div class="grid grid-cols-3 gap-4 pt-4">
  <div v-click class="border border-blue-900/30 bg-blue-900/5 p-4 rounded-xl shadow-sm hover:bg-blue-900/10 transition-colors">
    <h3 class="text-blue-400 border-b border-blue-900/30 mb-3 pb-1 flex items-center gap-2"><carbon:logo-vue/> Frontend</h3>
    <ul class="text-sm opacity-90 space-y-1">
      <li>Vue 3 (Composition API)</li>
      <li>Pinia (State Management)</li>
      <li>Vite / Axios</li>
      <li>Kakao Maps SDK</li>
    </ul>
  </div>
  <div v-click class="border border-green-900/30 bg-green-900/5 p-4 rounded-xl shadow-sm hover:bg-green-900/10 transition-colors">
    <h3 class="text-green-400 border-b border-green-900/30 mb-3 pb-1 flex items-center gap-2"><carbon:cafe/> Backend</h3>
    <ul class="text-sm opacity-90 space-y-1">
      <li>Spring Boot 3.5 / Java 21</li>
      <li>Spring Data JPA / MyBatis</li>
      <li>MariaDB 11 / JWT</li>
      <li><b>CQRS Pattern Applied</b></li>
    </ul>
  </div>
  <div v-click class="border border-purple-900/30 bg-purple-900/5 p-4 rounded-xl shadow-sm hover:bg-purple-900/10 transition-colors">
    <h3 class="text-purple-400 border-b border-purple-900/30 mb-3 pb-1 flex items-center gap-2"><carbon:cloud-service-management/> Infrastructure</h3>
    <ul class="text-sm opacity-90 space-y-1">
      <li>Docker / Kubernetes</li>
      <li>Jenkins (CI) / ArgoCD (CD)</li>
      <li>AWS S3 / ngrok</li>
      <li><b>GitOps Workflow</b></li>
    </ul>
  </div>
</div>

<div class="mt-6 p-4 bg-[#161b22] border border-gray-800/50 rounded-xl text-[10px] font-mono text-center flex items-center justify-around">
  <div v-click class="flex flex-col items-center gap-1"><carbon:user-speaker class="text-blue-400 text-lg"/><span>Dev Push</span></div>
  <carbon:chevron-right v-click class="text-gray-600"/>
  <div v-click class="flex flex-col items-center gap-1"><carbon:logo-github class="text-white text-lg"/><span>Webhook</span></div>
  <carbon:chevron-right v-click class="text-gray-600"/>
  <div v-click class="flex flex-col items-center gap-1"><carbon:settings-check class="text-orange-400 text-lg"/><span>Jenkins Build</span></div>
  <carbon:chevron-right v-click class="text-gray-600"/>
  <div v-click class="flex flex-col items-center gap-1"><carbon:container-software class="text-blue-500 text-lg"/><span>Docker Hub</span></div>
  <carbon:chevron-right v-click class="text-gray-600"/>
  <div v-click class="flex flex-col items-center gap-1"><carbon:rocket class="text-green-400 text-lg"/><span>ArgoCD Sync</span></div>
  <carbon:chevron-right v-click class="text-gray-600"/>
  <div v-click="11" class="flex flex-col items-center gap-1"><carbon:kubernetes class="text-blue-300 text-lg"/><span>K8s Cluster</span></div>
</div>

<div v-click="12" class="mt-4 p-3 bg-blue-900/10 border border-blue-800/20 rounded-lg text-[10px] text-blue-200/80 italic text-center">
  <b>GitOps</b>: Git 레포지토리를 <b>단일 진실 공급원(SSOT)</b>으로 사용하여 인프라의 '원하는 상태'를 관리하고, ArgoCD가 실제 환경과 실시간 동기화하는 운영 모델입니다.
</div>

<!--
기술 스택 및 아키텍처입니다. 프론트엔드는 Vue 3와 Pinia를 사용해 상태를 중앙 관리합니다. 
백엔드는 Spring Boot 3.5와 Java 21 기반이며, 특히 CQRS 패턴을 도입해 읽기와 쓰기 책임을 분리했습니다.
인프라는 저희가 가장 공을 들인 부분으로, 화면 하단의 도식처럼 Docker와 K8s 환경 위에 Jenkins와 ArgoCD를 결합한 GitOps 자동 배포 파이프라인을 구축했습니다.
코드 한 줄 Push로 빌드부터 K8s 배포까지 이어지는 이 흐름은 팀원들에게 가장 값진 학습 경험이었습니다.
-->

---
layout: center
---

# 02-1. 서비스 아키텍처 다이어그램

<div class="flex justify-center mt-4">
  <img src="/project_architecture.png" class="rounded-2xl border border-gray-700 shadow-2xl max-h-100 object-contain bg-white/5 p-4" v-motion :initial="{ opacity: 0, scale: 0.95 }" :enter="{ opacity: 1, scale: 1, transition: { duration: 1000 } }" />
</div>

<!--
앞서 설명해 드린 기술 스택들이 실제 서비스에서 어떻게 유기적으로 연결되는지 보여주는 전체 아키텍처 다이어그램입니다. 
사용자의 요청이 Ingress를 통해 들어와 프론트엔드와 백엔드로 라우팅되는 과정, 그리고 Jenkins와 ArgoCD가 매니페스트 레포지토리를 매개로 클러스터를 업데이트하는 GitOps 흐름을 한눈에 확인하실 수 있습니다.
-->

---
layout: center
class: text-left
---

# Table of Contents

<div class="space-y-4 text-2xl">
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox-checked />
    <span>01. 프로젝트 개요</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox-checked />
    <span>02. 기술 아키텍처</span>
  </div>
  <div class="flex items-center gap-4 text-blue-400 font-bold scale-110">
    <carbon:checkbox-checked-filled />
    <span>03. 백엔드 설계</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox />
    <span>04. 인프라 운영</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox />
    <span>05. 서비스 주요 기능</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox />
    <span>06. 핵심 트러블슈팅</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox />
    <span>07. 마무리 및 회고</span>
  </div>
</div>

---
layout: two-cols
---

# 03-1. 백엔드 설계:<br>CQRS 패턴

<div class="pr-4">
  <div v-click>
    <h3 class="text-orange-400 mb-2 flex items-center gap-2"><carbon:edit/> Command (쓰기)</h3>
    <div class="bg-orange-900/10 border border-orange-800/20 p-4 rounded-xl text-xs mb-4">
      <p class="mb-1"><b class="text-orange-300">JPA 기반 강타입 엔티티 관리</b></p>
      <ul class="opacity-80 list-disc pl-4">
        <li>비즈니스 로직 및 유효성 검증</li>
        <li>도메인 상태 변경 (CUD) 집중</li>
        <li>안정적인 연관관계 처리</li>
      </ul>
    </div>
  </div>
  <div v-click>
    <h3 class="text-blue-400 mb-2 flex items-center gap-2"><carbon:search/> Query (읽기)</h3>
    <div class="bg-blue-900/10 border border-blue-800/20 p-4 rounded-xl text-xs">
      <p class="mb-1"><b class="text-blue-300">MyBatis 활용 조회 최적화</b></p>
      <ul class="opacity-80 list-disc pl-4">
        <li>복잡한 조인 쿼리 및 성능 최적화</li>
        <li>화면 요구사항에 맞는 DTO 매핑</li>
        <li>읽기 전용 모델로 시스템 복잡도 완화</li>
      </ul>
    </div>
  </div>
</div>

<template v-slot:right>
<div class="pl-4 pt-8" v-click>
  <p class="text-xs text-gray-500 mb-2 font-mono flex items-center gap-1"><carbon:folder-parent/> 패키지 수준의 책임 분리</p>
  <div class="bg-gray-900 text-green-300 p-4 rounded text-[11px] font-mono border border-gray-800">
    <p>com.maplog.diary</p>
    <p class="pl-2 text-gray-600">├── <span class="text-orange-300">command/</span></p>
    <p class="pl-4 text-orange-200/70">│   ├── DiaryCommandController</p>
    <p class="pl-4 text-orange-200/70">│   └── DiaryCommandService <span class="text-gray-600">(JPA)</span></p>
    <p class="pl-2 text-gray-600">└── <span class="text-blue-300">query/</span></p>
    <p class="pl-4 text-blue-200/70">    ├── DiaryQueryController</p>
    <p class="pl-4 text-blue-200/70">    └── DiaryQueryService <span class="text-gray-600">(MyBatis)</span></p>
  </div>
  <p class="text-[10px] text-gray-500 mt-4 italic">"읽기와 쓰기를 분리하여 팀원 간 코드 충돌을 최소화하고,<br>각 작업의 특성에 맞는 라이브러리를 선택했습니다."</p>
</div>
</template>

<!--
백엔드 설계의 핵심은 CQRS(명령과 조회의 책임 분리) 패턴의 도입입니다. 
다이어리 도메인 특성상 쓰기 작업은 연관관계와 검증이 복잡한 반면, 읽기 작업은 지도 마커 조회처럼 특정 데이터만 가볍게 가져오는 경우가 많습니다. 
이를 패키지 수준에서 분리하여 쓰기는 JPA로 안정성을, 읽기는 MyBatis로 최적화를 챙겼습니다. 
각 도메인의 책임이 명확해지면서 클래스가 비대해지는 것을 막고 유지보수성을 높였습니다.
-->

---
layout: default
---

# 03-2. 백엔드 설계:<br>S3 스토리지 전략

<div class="grid grid-cols-2 gap-6 pt-4">
  <div v-click>
    <h4 class="text-orange-500 font-bold mb-2 flex items-center gap-2"><carbon:upload/> 업로드 흐름 (서버 경유)</h4>
    <div class="bg-[#161b22] p-4 rounded-xl text-[11px] font-mono border border-orange-900/20">
      Client <span class="text-gray-600">→</span> Spring Boot <br>
      <span class="text-gray-600 ml-4">↳ multipart/form-data</span> <br>
      Spring Boot <span class="text-gray-600">→</span> AWS S3 <br>
      <span class="text-gray-600 ml-4">↳ putObject()</span> <br>
      <span class="text-green-400">Result: DB에 S3 원본 URL 저장</span>
    </div>
  </div>
  <div v-click>
    <h4 class="text-blue-500 font-bold mb-2 flex items-center gap-2"><carbon:download/> 조회 흐름 (백엔드 우회)</h4>
    <div class="bg-[#161b22] p-4 rounded-xl text-[11px] font-mono border border-blue-900/20">
      Client <span class="text-gray-600">→</span> Spring Boot <br>
      <span class="text-gray-600 ml-4">↳ generatePresignedUrl()</span> <br>
      <span class="text-yellow-400 ml-4">↳ 1시간 유효 서명 URL 발급</span> <br>
      Client <span class="text-gray-600">→</span> <b>AWS S3 (Direct GET)</b> <br>
      <span class="text-green-400">Result: 서버 부하 분산 및 보안 확보</span>
    </div>
  </div>
</div>

<div class="grid grid-cols-2 gap-4 mt-6">
  <div class="bg-yellow-900/10 border border-yellow-800/20 p-3 rounded-xl text-sm" v-click>
    <b class="text-yellow-500 flex items-center gap-2"><carbon:security/> 🔒 보안 최적화</b>
    S3 버킷 퍼블릭 차단 상태 유지. <br>임시 서명된 URL로만 한시적 접근 허용.
  </div>
  <div class="bg-green-900/10 border border-green-800/20 p-3 rounded-xl text-sm" v-click>
    <b class="text-green-500 flex items-center gap-2"><carbon:flash/> ⚡ 성능 최적화</b>
    대용량 이미지 트래픽이 백엔드를 우회. <br>S3의 강력한 인프라 자원을 직접 활용.
  </div>
</div>

<!--
이미지 저장 시 AWS S3 Presigned URL 방식을 채택했습니다. 
백엔드는 업로드 시에만 중개자 역할을 하고, 조회 시에는 1시간 동안만 유효한 임시 서명 URL을 발급합니다.
이를 통해 클라이언트는 서버를 거치지 않고 S3에서 이미지를 직접 가져오기 때문에 백엔드 부하를 획기적으로 줄였습니다. 
또한 S3 버킷을 퍼블릭으로 열지 않고도 안전하게 이미지를 제공할 수 있는 보안성까지 확보했습니다.
-->

---
layout: center
class: text-left
---

# Table of Contents

<div class="space-y-4 text-2xl">
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox-checked />
    <span>01. 프로젝트 개요</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox-checked />
    <span>02. 기술 아키텍처</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox-checked />
    <span>03. 백엔드 설계</span>
  </div>
  <div class="flex items-center gap-4 text-blue-400 font-bold scale-110">
    <carbon:checkbox-checked-filled />
    <span>04. 인프라 운영</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox />
    <span>05. 서비스 주요 기능</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox />
    <span>06. 핵심 트러블슈팅</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox />
    <span>07. 마무리 및 회고</span>
  </div>
</div>

---
layout: default
---

# 04-1. 인프라:<br>GitOps 기반 CI/CD

<div class="grid grid-cols-5 items-start pt-10 gap-2">
  <div class="text-center" v-click>
    <div class="text-4xl mb-3">👨‍💻</div>
    <h4 class="font-bold text-sm text-blue-400">Developer</h4>
    <p class="text-[10px] text-gray-500 mt-2 leading-relaxed">Code Push to<br>GitHub Main</p>
  </div>
  <div class="text-center text-gray-700 text-3xl pt-8" v-click><carbon:arrow-right/></div>
  <div class="text-center" v-click>
    <div class="text-4xl mb-3 text-orange-400"><carbon:settings-check/></div>
    <h4 class="font-bold text-sm text-orange-400">Jenkins (CI)</h4>
    <div class="text-[10px] text-gray-300 text-left mt-3 bg-gray-900/50 p-3 rounded-lg border border-gray-800 space-y-1">
      <p>① Build (Gradle/NPM)</p>
      <p>② Docker Image Push</p>
      <p>③ <b>Manifest Tag Update</b></p>
    </div>
  </div>
  <div class="text-center text-gray-700 text-3xl pt-8" v-click><carbon:arrow-right/></div>
  <div class="text-center" v-click>
    <div class="text-4xl mb-3 text-green-400"><carbon:cloud-service-management/></div>
    <h4 class="font-bold text-sm text-green-400">ArgoCD (CD)</h4>
    <div class="text-[10px] text-gray-300 text-left mt-3 bg-gray-900/50 p-3 rounded-lg border border-gray-800 space-y-1">
      <p>① Watch Manifest Repo</p>
      <p>② <b>K8s Auto Sync</b></p>
      <p>③ Rolling Update</p>
    </div>
  </div>
</div>

<div class="mt-10 bg-blue-900/10 border border-blue-800/20 p-4 rounded-xl text-xs flex items-center gap-4" v-click>
  <div class="bg-blue-500/20 p-2 rounded-lg text-blue-400 font-bold">ngrok</div>
  <p class="text-blue-200/80 leading-relaxed">로컬 Kubernetes 환경의 Jenkins를 외부 GitHub Webhook과 연결하기 위한 <b>보안 터널링</b> 도구로 활용.</p>
</div>

<!--
배포 파이프라인은 Jenkins와 ArgoCD를 결합한 GitOps 모델을 구축했습니다.
개발자가 코드를 푸시하면 Jenkins가 빌드 및 도커 이미지 푸시를 수행하고, 매니페스트 레포지토리의 태그를 업데이트합니다. 
이후 ArgoCD가 매니페스트 변경을 감지해 K8s 클러스터에 동기화하는 배포 파이프라인을 완성했습니다. 
특히 ngrok을 통해 로컬 환경의 한계를 극복하고 실시간 배포가 가능하도록 설계했습니다.
-->

---
layout: two-cols
---

# 04-2. 인프라:<br>Docker 빌드 전략

<div class="pr-6 pt-4">

```dockerfile {1-7|9-13}
# Backend Multi-stage Build
FROM gradle:8.5-jdk21 AS build
WORKDIR /app
COPY . .
RUN ./gradlew bootJar

# Runtime Environment
FROM eclipse-temurin:21-jre
COPY --from=build /app/build/libs/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

</div>

<template v-slot:right>
<div class="pl-4 pt-12 space-y-6">
  <div v-click class="bg-[#161b22] border-l-4 border-blue-500 p-4 rounded-r-xl shadow-lg">
    <b class="text-blue-400 text-sm flex items-center gap-2"><carbon:layers/> Multi-stage Build</b>
    <p class="text-[11px] text-gray-400 mt-2 leading-relaxed">빌드 환경과 실행 환경을 분리하여 컨테이너 이미지 크기를 최소화하고 보안성을 높였습니다.</p>
  </div>
  <div v-click class="bg-[#161b22] border-l-4 border-green-500 p-4 rounded-r-xl shadow-lg">
    <b class="text-green-400 text-sm flex items-center gap-2"><carbon:checkmark-filled/> JDK 21 LTS</b>
    <p class="text-[11px] text-gray-400 mt-2 leading-relaxed">최신 LTS 버전의 Java를 사용하여 가상 스레드 등 최신 기술 활용 및 안정적인 런타임을 확보했습니다.</p>
  </div>
</div>
</template>

<!--
효율적인 빌드를 위해 Docker Multi-stage 빌드를 적용했습니다. 
빌드 단계에서 소스를 컴파일하고, 실행 단계에서는 가벼운 JRE 이미지로 결과물만 복사해 이미지 크기를 획기적으로 줄였습니다. 
이를 통해 배포 속도를 높이고 실행 환경의 보안성을 강화했습니다.
-->

---
layout: two-cols
---

# 04-3. 인프라:<br>K8s 배포 설정

<div class="pr-6 pt-2">

```yaml {1-5|13-16|17-21}
# deployment.yaml (map-log-backend)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: map-log-backend
  namespace: map-log
spec:
  replicas: 3
  template:
    spec:
      containers:
      - name: map-log-backend
        image: gusgh07/map-log-backend:${TAG}
        ports:
        - containerPort: 8080
        env:
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: map-log-secrets
              key: DB_PASSWORD
```

</div>

<template v-slot:right>
<div class="pl-4 pt-10 space-y-4">
  <div v-click class="bg-purple-900/10 border border-purple-800/20 p-4 rounded-xl">
    <b class="text-purple-400 text-sm flex items-center gap-2"><carbon:locked/> Secret Management</b>
    <p class="text-[11px] text-purple-200/70 mt-1 leading-relaxed text-left text-left text-left">DB 비밀번호, AWS 키 등 민감 정보를 <b>Secret 객체</b>로 분리하여 보안성을 극대화했습니다.</p>
  </div>
  <div v-click class="bg-orange-900/10 border border-orange-800/20 p-4 rounded-xl">
    <b class="text-orange-400 text-sm flex items-center gap-2"><carbon:group-objects/> High Availability</b>
    <p class="text-[11px] text-orange-200/70 mt-1 leading-relaxed text-left text-left text-left"><b>Replicas: 3</b> 설정을 통해 다중 파드(Pod)를 운영하며 고가용성과 부하 분산을 구현했습니다.</p>
  </div>
</div>
</template>

<!--
Kubernetes 매니페스트는 ArgoCD를 통해 관리됩니다. 
선언적인 YAML 파일을 기반으로 클러스터의 상태를 관리하며, 새로운 이미지가 배포될 때는 Rolling Update 방식을 통해 서비스 중단 없이 시스템을 업데이트합니다. 
이를 통해 안정적인 운영 환경을 구축했습니다.
-->

---
layout: default
---

# 04-4. 인프라:<br>Jenkins 파이프라인 (Core Logic)

<div class="text-[9.5px] font-mono leading-relaxed pt-4 pr-4 bg-[#0d1117]/50 rounded-xl p-4 border border-gray-800">

```groovy {1-3|5-8|10-14}
// 1. 환경 변수 및 태그 정의
env.TAG = "${env.BUILD_NUMBER}"; env.IMG = "gusgh07/map-log"

// 2. 멀티 프로젝트 컨테이너 빌드 및 푸시
stage('Build & Push') {
    sh "docker build -t ${IMG}-be:${TAG} ./map-log-backend"
    sh "docker build --build-arg VITE_KAKAO_MAP_KEY=${KEY} -t ${IMG}-fe:${TAG} ./map-log-frontend"
    sh "docker push ${IMG}-be:${TAG} && docker push ${IMG}-fe:${TAG}"
}

// 3. GitOps: 매니페스트 레포지토리 업데이트
stage('Update Manifest') {
    dir('k8s-manifests') {
        git credentialsId: 'github', url: 'https://github.com/gusgh075/k8s-manifests.git'
        sh "sed -i 's|be:.*|be:${TAG}|g' deployment.yaml"
        sh "git commit -am '[UPDATE] ${TAG} version' && git push origin main"
    }
}
```

</div>

<!--
화면 구성을 위해 실제 Jenkinsfile의 핵심 로직만 압축했습니다. 
빌드 단계에서는 백엔드와 프론트엔드 이미지를 각각 생성하며, 특히 프론트엔드 빌드 시 카카오맵 API 키를 안전하게 주입합니다. 
이후 GitOps의 핵심인 매니페스트 업데이트 단계를 통해 deployment.yaml의 태그를 자동 갱신하고 푸시함으로써 ArgoCD가 최신 상태를 유지하도록 설계했습니다.
-->

<!--
실제 프로젝트에서 사용 중인 Jenkins Pipeline의 핵심 로직입니다. 
환경 변수 영역에서 빌드 번호를 태그로 정의하고, 백엔드와 프론트엔드 이미지를 각각 빌드합니다. 
특히 프론트엔드 빌드 시 카카오맵 API 키를 build-arg로 안전하게 주입하는 것이 특징입니다. 
빌드 후에는 별도의 매니페스트 레포지토리를 클론하여 deployment.yaml의 이미지 태그를 sed 명령어로 자동 업데이트하고 푸시함으로써 ArgoCD의 동기화를 유도하는 GitOps 프로세스를 완성했습니다.
마지막으로 빌드 결과는 Discord를 통해 실시간으로 알림을 받습니다.
-->

---
layout: two-cols
---

# 04-5. GitOps:<br>Manifest Repository

<div class="pr-4 pt-4">
  <div v-click>
    <h3 class="text-blue-400 mb-2 flex items-center gap-2"><carbon:code/> Single Source of Truth</h3>
    <p class="text-[11px] text-gray-400 leading-relaxed mb-4">
      애플리케이션의 <b>원하는 상태(Desired State)</b>가 선언적으로 정의된 독립적인 레포지토리입니다.
    </p>
  </div>
  <div v-click>
    <h3 class="text-green-400 mb-2 flex items-center gap-2"><carbon:flow/> Pipeline Bridge</h3>
    <ul class="text-[10px] text-gray-400 space-y-2">
      <li>• <b>CI (Jenkins)</b>: 빌드 후 이미지 태그 자동 업데이트 및 푸시</li>
      <li>• <b>CD (ArgoCD)</b>: 레포지토리의 변경을 감지하여 클러스터 동기화</li>
    </ul>
  </div>
</div>

<template v-slot:right>
<div class="pl-4 pt-10" v-click>
  <p class="text-xs text-gray-500 mb-2 font-mono flex items-center gap-1"><carbon:folder/> k8s-manifests Structure</p>
  <div class="bg-gray-900 text-yellow-300 p-4 rounded text-[11px] font-mono border border-gray-800 shadow-xl">
    <p>k8s-manifests/</p>
    <p class="pl-2">├── <carbon:document class="inline text-blue-400"/> deployment.yaml</p>
    <p class="pl-2">├── <carbon:document class="inline text-blue-400"/> service.yaml</p>
    <p class="pl-2">├── <carbon:document class="inline text-blue-400"/> ingress.yaml</p>
    <p class="pl-2">└── <carbon:document class="inline text-blue-400"/> secret.yaml <span class="text-gray-600">(Managed by Vault/Secret)</span></p>
  </div>
  <div class="mt-6 bg-blue-900/10 border border-blue-800/20 p-3 rounded-lg">
    <p class="text-[10px] text-blue-200/70 italic">"인프라의 모든 변경 사항은 이 레포지토리의 <b>Git History</b>를 통해 추적 및 관리됩니다."</p>
  </div>
</div>
</template>

<!--
Jenkins와 ArgoCD 사이에서 가장 중요한 역할을 하는 Manifest Repository를 소개합니다. 
이 레포지토리는 인프라의 '단일 진실 공급원'으로, 애플리케이션이 구동되어야 하는 최종 상태를 코드로 정의하고 있습니다. 
Jenkins가 이미지를 빌드한 뒤 이 레포지토리의 deployment.yaml 태그를 수정하면, ArgoCD가 이 변화를 실시간으로 감지해 K8s 클러스터로 배포를 수행하게 됩니다. 
모든 배포 이력이 Git 커밋으로 남기 때문에 언제든 과거 시점으로 롤백하거나 배포 기록을 추적할 수 있는 GitOps의 핵심 자산입니다.
-->

---
layout: center
---

# 04-6. 인프라:<br>CI/CD 테스트 결과

<div class="flex flex-col items-center justify-center">
  <div class="w-full max-w-2xl border-4 border-green-500/30 rounded-2xl overflow-hidden shadow-2xl">
    <img src="/CI-CD결과화면.gif" class="w-full aspect-video object-cover" />
  </div>
  <p class="mt-6 text-gray-400" v-click>성공적인 파이프라인 가동 및 무중단 배포 확인</p>
</div>

---
layout: center
---

# 🎥 시연 영상: 1. 회원가입, 로그인, 로그아웃

<div class="w-full max-w-3xl aspect-video bg-black/50 rounded-2xl border border-gray-700 flex items-center justify-center shadow-2xl overflow-hidden">
  <img src="/1.회원가입,로그인,로그아웃.gif" class="w-full h-full object-cover" />
</div>

<!--
먼저 서비스의 시작인 회원가입과 로그인 과정입니다. 
깔끔한 UI를 통해 사용자가 손쉽게 계정을 생성하고 접속할 수 있도록 구현했습니다.
-->

---
layout: center
---

# 🎥 시연 영상: 2. 내 프로필 조회, 수정, 삭제

<div class="w-full max-w-3xl aspect-video bg-black/50 rounded-2xl border border-gray-700 flex items-center justify-center shadow-2xl overflow-hidden">
  <img src="/2.내프로필 조회,수정,삭제.gif" class="w-full h-full object-cover" />
</div>

<!--
다음은 개인화 경험의 핵심인 프로필 관리 기능입니다. 
자신의 정보를 실시간으로 조회하고, 필요에 따라 수정하거나 계정을 관리하는 과정을 확인하실 수 있습니다.
-->

---
layout: center
---

# 🎥 시연 영상: 3. 사용자 검색, 친구 요청, 수락, 삭제

<div class="w-full max-w-3xl aspect-video bg-black/50 rounded-2xl border border-gray-700 flex items-center justify-center shadow-2xl overflow-hidden">
  <img src="/3.사용자검색,친구요청,수락,삭제.gif" class="w-full h-full object-cover" />
</div>

<!--
마지막으로 소셜 기능인 친구 시스템입니다. 
다른 사용자를 검색하여 친구 요청을 보내고, 알림을 통해 수락하거나 관계를 관리하는 전 과정을 구현했습니다.
-->

---
layout: center
---

# 🎥 시연 영상: 4. 일기 스크랩 목록 추가 조회

<div class="w-full max-w-3xl aspect-video bg-black/50 rounded-2xl border border-gray-700 flex items-center justify-center shadow-2xl overflow-hidden">
  <img src="/4.일기 스크랩 목록 추가 조회.gif" class="w-full h-full object-cover" />
</div>

<!--
추가로 구현된 스크랩 기능입니다. 
마음에 드는 다른 사용자의 일기를 스크랩하여 나만의 목록으로 모아보고 관리할 수 있는 기능을 시연합니다.
-->

---
layout: center
class: text-left
---

# Table of Contents

<v-clicks>
<div class="space-y-4 text-2xl">
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox-checked />
    <span>01. 프로젝트 개요</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox-checked />
    <span>02. 기술 아키텍처</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox-checked />
    <span>03. 백엔드 설계</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox-checked />
    <span>04. 인프라 운영</span>
  </div>
  <div class="flex items-center gap-4 text-blue-400 font-bold scale-110">
    <carbon:checkbox-checked-filled />
    <span>05. 서비스 주요 기능</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox />
    <span>06. 핵심 트러블슈팅</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox />
    <span>07. 마무리 및 회고</span>
  </div>
</div>
</v-clicks>

---
layout: default
---

# 05. 서비스 주요 기능:<br>공간과 관계의 기록

<div class="grid grid-cols-2 gap-4 pt-4 text-left">
  <div v-click v-motion :initial="{ x: -50, opacity: 0 }" :enter="{ x: 0, opacity: 1 }" class="bg-[#161b22] border border-gray-800 p-4 rounded-xl shadow-lg hover:border-blue-500/30 transition-colors">
    <h3 class="text-blue-400 mb-2 flex items-center gap-2 font-bold"><carbon:map/> 지도 기반 일기 쓰기</h3>
    <p class="text-[11px] text-gray-400 leading-relaxed">
      카카오 맵 SDK를 활용해 지도 위에 직접 마커를 생성합니다. 장소 검색을 통해 위경도 좌표를 기록하고, 해당 위치의 맥락이 담긴 사진과 글을 저장합니다.
    </p>
  </div>
  <div v-click v-motion :initial="{ x: 50, opacity: 0 }" :enter="{ x: 0, opacity: 1 }" class="bg-[#161b22] border border-gray-800 p-4 rounded-xl shadow-lg hover:border-purple-500/30 transition-colors">
    <h3 class="text-purple-400 mb-2 flex items-center gap-2 font-bold"><carbon:share/> 친구에게 일기 공유</h3>
    <p class="text-[11px] text-gray-400 leading-relaxed">
      일기별 공개 범위(Public/Private) 설정을 지원합니다. 친구 관계를 맺은 사용자끼리 서로의 공간 발자취를 공유하며, '나만 보기' 기능을 통해 개인적인 기록도 가능합니다.
    </p>
  </div>
  <div v-click v-motion :initial="{ x: -50, opacity: 0 }" :enter="{ x: 0, opacity: 1 }" class="bg-[#161b22] border border-gray-800 p-4 rounded-xl shadow-lg hover:border-green-500/30 transition-colors">
    <h3 class="text-green-400 mb-2 flex items-center gap-2 font-bold"><carbon:list/> 공간 타임라인 피드</h3>
    <p class="text-[11px] text-gray-400 leading-relaxed">
      내가 방문한 장소와 친구들의 새로운 기록을 모아보는 피드 기능을 제공합니다. 단순 목록을 넘어 지도를 이동하며 장소 중심의 추억 회상을 지원합니다.
    </p>
  </div>
  <div v-click v-motion :initial="{ x: 50, opacity: 0 }" :enter="{ x: 0, opacity: 1 }" class="bg-[#161b22] border border-gray-800 p-4 rounded-xl shadow-lg hover:border-red-500/30 transition-colors">
    <h3 class="text-red-400 mb-2 flex items-center gap-2 font-bold"><carbon:user-admin/> 관리자 유저 관리</h3>
    <p class="text-[11px] text-gray-400 leading-relaxed">
      RBAC(Role-Based Access Control) 기반의 관리자 페이지를 구축했습니다. 전체 사용자 현황 파악, 비정상 유저 제재 등 안정적인 서비스 운영을 위한 대시보드를 제공합니다.
    </p>
  </div>
</div>

<!--
MapLog의 4대 핵심 기능을 소개하겠습니다. 
첫째, 지도 기반 일기 쓰기입니다. 카카오 맵 SDK를 제어해 사용자가 지도 위 특정 위치에 마커를 찍고 장소와 함께 기억을 남길 수 있게 했습니다. 
둘째, 친구 공유 기능입니다. 일기마다 공개 여부를 설정할 수 있어, 개인적인 기록은 물론 친구들과의 공간 공유까지 조절이 가능합니다. 
셋째, 피드 기능입니다. 시간과 공간이 결합된 타임라인을 통해 나만의 발자취와 친구들의 소식을 한눈에 확인할 수 있습니다. 
마지막으로 관리자 기능입니다. 안정적인 서비스 운영을 위해 권한 체계를 설계하고 전체 유저를 관리할 수 있는 전용 대시보드를 구축했습니다.
-->

---
layout: center
class: text-left
---

# Table of Contents

<v-clicks>
<div class="space-y-4 text-2xl">
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox-checked />
    <span>01. 프로젝트 개요</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox-checked />
    <span>02. 기술 아키텍처</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox-checked />
    <span>03. 백엔드 설계</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox-checked />
    <span>04. 인프라 운영</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox-checked />
    <span>05. 서비스 주요 기능</span>
  </div>
  <div class="flex items-center gap-4 text-blue-400 font-bold scale-110">
    <carbon:checkbox-checked-filled />
    <span>06. 핵심 트러블슈팅</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox />
    <span>07. 마무리 및 회고</span>
  </div>
</div>
</v-clicks>

---
layout: default
---

# 06-1. 핵심 트러블슈팅:<br>Ingress 80포트 노출

<div v-click v-motion :initial="{ x: -20, opacity: 0 }" :enter="{ x: 0, opacity: 1 }" class="bg-red-900/10 border border-red-800/20 p-5 border-l-4 border-l-red-500 mb-8 rounded-r-xl text-left">
  <h4 class="font-bold text-red-400 flex items-center gap-2 text-lg mb-2"><carbon:error/> Problem</h4>
  <p class="text-sm text-red-100/80 leading-relaxed">K8s NodePort의 기본 제한(30000-32767)으로 인해 서비스 진입점에 표준 HTTP 80포트를 사용할 수 없는 물리적 한계 발생.</p>
</div>

<div v-click v-motion :initial="{ y: 30, opacity: 0 }" :enter="{ y: 0, opacity: 1 }" class="bg-green-900/10 border border-green-800/20 p-5 border-l-4 border-l-green-500 rounded-r-xl text-left shadow-lg">
  <h4 class="font-bold text-green-400 flex items-center gap-2 text-lg mb-2"><carbon:checkmark-filled/> Solution: NGINX Ingress Controller</h4>
  <ul class="space-y-3 text-sm text-green-200/80 pl-4 list-disc">
    <li><b>hostPort</b> 방식으로 노드의 80포트를 직접 바인딩하여 진입점 확보.</li>
    <li><code>ingress.yaml</code> 경로 기반 라우팅: <code>/api</code> → Backend, <code>/</code> → Frontend.</li>
    <li>사용자에게 <b>http://localhost</b> 와 같은 깔끔한 URL 경험 제공.</li>
  </ul>
</div>

<!--
첫 번째 트러블슈팅 사례입니다. K8s 환경에서 서비스 포트를 표준 80번으로 설정하려 했으나, NodePort는 3만 번대 이상의 포트만 허용하는 한계가 있었습니다. 
이를 해결하기 위해 NGINX Ingress Controller를 도입했습니다. 호스트의 80포트를 점유하여 내부 서비스로 라우팅하는 방식을 통해, 
사용자가 복잡한 포트 번호 없이 깔끔한 URL로 서비스에 접속할 수 있도록 개선했습니다.
-->

---
layout: two-cols
---

# 06-2. 핵심 트러블슈팅:<br>인증 안정성 확보

<div class="text-[9.5px] leading-tight -mt-2 pr-2">

```javascript {1-2|4-8|10-13}
let isRef = false, q = [];

api.interceptors.response.use(r => r.data, async err => {
  const { config: cfg, response: res } = err;
  if (!res || res.status !== 401 || cfg._retry) return Promise.reject(err);
  cfg._retry = true;

  if (isRef) return new Promise(r => q.push(r)).then(() => api(cfg));
  
  isRef = true;
  const tk = await callRefreshApi();
  localStorage.setItem('ml_access_token', tk);
  q.forEach(r => r()); q = []; isRef = false;

  cfg.headers.Authorization = `Bearer ${tk}`;
  return api(cfg);
});
```

</div>

<template v-slot:right>
<div class="pl-4 pt-6 space-y-4 text-left">
  <div v-click v-motion :initial="{ x: -20, opacity: 0 }" :enter="{ x: 0, opacity: 1, transition: { type: 'spring', stiffness: 300, damping: 10 } }" class="bg-red-900/10 border border-red-800/20 p-4 rounded-xl text-xs">
    <b class="text-red-400 flex items-center gap-2"><carbon:error-filled/> Problem: Race Condition</b>
    <p class="text-red-100/80 mt-1 leading-relaxed">동시 다발적 401 에러 시 여러 번의 토큰 갱신 시도로 인한 Refresh Token 중복 소비 문제.</p>
  </div>
  <div v-click v-motion :initial="{ y: 50, opacity: 0 }" :enter="{ y: 0, opacity: 1, transition: { duration: 600 } }" class="bg-green-900/10 border border-green-800/20 p-4 rounded-xl text-xs shadow-[0_0_15px_rgba(74,222,128,0.1)]">
    <b class="text-green-400 flex items-center gap-2"><carbon:checkmark-outline-error/> Solution: Request Queue</b>
    <p class="text-green-100/80 mt-1 leading-relaxed">갱신 시작 시 이후 요청을 <b>Queue에 대기</b>시키고 완료 후 한 번에 실행하여 안정성 확보.</p>
  </div>
  <div v-click v-motion :initial="{ opacity: 0 }" :enter="{ opacity: 1, transition: { delay: 300 } }" class="bg-[#161b22] border border-gray-800/50 p-4 rounded-xl text-[10px] font-mono leading-5">
    <p class="text-blue-400 font-bold mb-1">Impact:</p>
    <p class="text-gray-400">- 사용자에게 투명한 인증 갱신</p>
    <p class="text-gray-400">- 인증 서버 부하 및 동기화 에러 방지</p>
  </div>
</div>
</template>

<!--
인증 시스템 트러블슈팅 사례입니다. Access Token 만료 시 여러 API 호출이 동시에 실패하면서 각 요청이 독립적으로 토큰 갱신을 시도하여 Refresh Token이 중복 소비되는 Race Condition이 발생했습니다. 
이를 해결하기 위해 isRefreshing 플래그와 pendingQueue를 도입했습니다. 갱신이 시작되면 이후 실패 요청들을 큐에 쌓아두고, 갱신이 완료되는 순간 큐의 모든 요청을 한 번에 재시도합니다.
결과적으로 사용자는 토큰 만료를 인식하지 못한 채 서비스를 끊김 없이 이용할 수 있게 됐습니다.
-->

---
layout: two-cols
---

# 06-3. 핵심 트러블슈팅:<br>환경별 저장 전략 (@Profile)

<div class="text-[10.5px] leading-tight -mt-2 pr-2">

```java {1-5|7-12|14-19}
// 1. 추상화 인터페이스
public interface FileStorageService {
    String store(MultipartFile file);
    String generatePresignedUrl(String fileUrl);
}

// 2. 로컬 개발 환경용 구현체
@Service @Profile("local")
public class LocalFileStorageService implements FileStorageService {
    public String store(MultipartFile f) { ... }
    public String generatePresignedUrl(String url) { return url; }
}

// 3. 운영 환경용 구현체 (S3)
@Service @Profile({"dev", "aws"})
public class S3FileStorageService implements FileStorageService {
    public String store(MultipartFile f) { ... }
    public String generatePresignedUrl(String url) { ... }
}
```

</div>

<template v-slot:right>
<div class="pl-4 pt-6 space-y-4 text-left">
  <div v-click v-motion :initial="{ x: -20, opacity: 0 }" :enter="{ x: 0, opacity: 1 }" class="bg-red-900/10 border border-red-800/20 p-4 rounded-xl text-xs">
    <b class="text-red-400 flex items-center gap-2"><carbon:incomplete-cancel/> Infrastructure Coupling</b>
    <p class="text-red-100/80 mt-1">로컬(파일시스템) vs 운영(S3) 환경 차이로 비즈니스 로직 내에 조건문(if)이 섞이는 문제.</p>
  </div>
  <div v-click v-motion :initial="{ y: 30, opacity: 0 }" :enter="{ y: 0, opacity: 1 }" class="bg-green-900/10 border border-green-800/20 p-4 rounded-xl text-xs">
    <b class="text-green-400 flex items-center gap-2"><carbon:connect/> Strategy Pattern + @Profile</b>
    <p class="text-green-100/80 mt-1">인터페이스를 주입받고 환경 변수에 따라 <b>구현체를 자동 교체</b>하여 로직과 인프라를 분리.</p>
  </div>
  <div v-click v-motion :initial="{ opacity: 0 }" :enter="{ opacity: 1 }" class="bg-blue-900/10 border border-blue-800/20 p-4 rounded-xl text-[10px] font-mono">
    <p class="text-blue-300"># application-dev.yaml</p>
    <p class="text-blue-300">spring.profiles.active: dev</p>
    <p class="text-gray-500 mt-1">→ 코드 수정 없이 저장 전략 동적 변경</p>
  </div>
</div>
</template>

<!--
파일 저장 전략 트러블슈팅입니다. 로컬과 운영 서버의 저장소가 달랐는데, if 분기문을 쓰면 비즈니스 로직 안에 인프라 판단이 섞여 복잡해집니다. 
저희는 Spring의 @Profile을 활용한 전략 패턴으로 접근했습니다. FileStorageService 인터페이스를 정의하고, 로컬과 S3용 구현체를 분리했습니다.
서비스 레이어는 인터페이스만 주입받기 때문에 환경 변수 하나로 구현체가 자동 교체되고, 다이어리 저장 코드는 단 한 줄도 수정할 필요가 없도록 설계했습니다.
-->

---
layout: default
---

# 06-4. 핵심 트러블슈팅:<br>401 복합 장애 해결

<div class="grid grid-cols-3 gap-3 text-xs pt-8 text-left">
  <div v-click="1" v-motion :initial="{ y: -50, opacity: 0 }" :enter="{ y: 0, opacity: 1 }" class="border-t-4 border-red-500 bg-red-900/10 border border-red-800/20 p-4 rounded-b-xl hover:bg-red-900/20 transition-colors">
    <div class="font-bold text-red-400 mb-3 flex items-center gap-2"><carbon:settings/> Layer 1: Infrastructure</div>
    <p class="text-red-100/80 leading-relaxed">
      <b>Profile 활성화 오류</b><br>
      K8s 환경변수 미지정으로 S3 대신 로컬 스토리지가 활성화되어 데이터 저장 및 경로 불일치 발생.
    </p>
  </div>
  <div v-click="2" v-motion :initial="{ y: -50, opacity: 0 }" :enter="{ y: 0, opacity: 1, transition: { delay: 200 } }" class="border-t-4 border-yellow-500 bg-yellow-900/10 border border-yellow-800/20 p-4 rounded-b-xl hover:bg-yellow-900/20 transition-colors">
    <div class="font-bold text-yellow-400 mb-3 flex items-center gap-2"><carbon:link/> Layer 2: Frontend</div>
    <p class="text-yellow-100/80 leading-relaxed">
      <b>URL 조합 및 인증 누락</b><br>
      이미지 태그 요청은 Axios 인터셉터를 거치지 않아 Authorization 헤더가 누락된 채 전송됨.
    </p>
  </div>
  <div v-click="3" v-motion :initial="{ y: -50, opacity: 0 }" :enter="{ y: 0, opacity: 1, transition: { delay: 400 } }" class="border-t-4 border-orange-500 bg-orange-900/10 border border-orange-800/20 p-4 rounded-b-xl hover:bg-orange-900/20 transition-colors">
    <div class="font-bold text-orange-400 mb-3 flex items-center gap-2"><carbon:security/> Layer 3: Security</div>
    <p class="text-orange-100/80 leading-relaxed">
      <b>Security 필터 불일치</b><br>
      Nginx 프록시 경로(/api)와 Spring Security의 permitAll 설정이 일치하지 않아 인증 오류 발생.
    </p>
  </div>
</div>

<div class="mt-8 bg-green-900/10 border border-green-800/20 p-4 rounded-xl text-sm border-l-4 border-l-green-500 text-left shadow-lg" v-click="4" v-motion :initial="{ scale: 0.9, opacity: 0 }" :enter="{ scale: 1, opacity: 1 }">
  <b class="text-green-400 flex items-center gap-2"><carbon:rocket/> Final Fix: Infrastructure Decoupling</b>
  <p class="text-green-100/80 mt-1">프로필 매핑 재설계 및 <b>Presigned URL</b> 도입. Nginx와 Security를 우회하여 브라우저가 S3에 직접 요청하도록 개선.</p>
</div>

<!--
가장 복잡했던 복합 장애 사례입니다. K8s 배포 후 이미지가 401 권한 에러로 깨졌는데, 원인을 추적해보니 세 개의 독립된 레이어가 맞물린 문제였습니다. 
첫째, 프로필 설정 오류로 엉뚱한 스토리지가 활성화됐고, 둘째, 브라우저의 이미지 요청 시 인증 헤더가 누락됐으며, 셋째, 해당 프록시 경로가 Security 설정에서 막혀 있었습니다.
원인을 레이어별로 분리해서 분석한 끝에, Presigned URL을 통해 브라우저가 직접 S3에 접근하게 하여 인프라 종속성을 제거하고 근본적으로 해결했습니다.
-->

---
layout: center
class: text-left
---

# Table of Contents

<v-clicks>
<div class="space-y-4 text-2xl">
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox-checked />
    <span>01. 프로젝트 개요</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox-checked />
    <span>02. 기술 아키텍처</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox-checked />
    <span>03. 백엔드 설계</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox-checked />
    <span>04. 인프라 운영</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox-checked />
    <span>05. 서비스 주요 기능</span>
  </div>
  <div class="flex items-center gap-4 opacity-30">
    <carbon:checkbox-checked />
    <span>06. 핵심 트러블슈팅</span>
  </div>
  <div class="flex items-center gap-4 text-blue-400 font-bold scale-110">
    <carbon:checkbox-checked-filled />
    <span>07. 마무리 및 회고</span>
  </div>
</div>
</v-clicks>

---
layout: center
class: text-center
---

# 07-1. 마무리:<br>성과 및 향후 발전 방향

<div class="grid grid-cols-2 gap-10 text-left pt-10">
  <div v-click v-motion :initial="{ x: -50, opacity: 0 }" :enter="{ x: 0, opacity: 1 }">
    <h3 class="border-b border-blue-900/50 text-blue-400 pb-2 mb-4">🏆 Key Achievements</h3>
    <ul class="text-sm space-y-2 opacity-90">
      <li>DevOps 전체 라이프사이클(GitOps) 체감</li>
      <li>레이어 분리 디버깅 사고방식 확보</li>
      <li>현대적 아키텍처(CQRS) 실전 적용</li>
    </ul>
  </div>
  <div v-click v-motion :initial="{ x: 50, opacity: 0 }" :enter="{ x: 0, opacity: 1 }">
    <h3 class="border-b border-purple-900/50 text-purple-400 pb-2 mb-4">🚀 Future Roadmap</h3>
    <ul class="text-sm space-y-2 opacity-90">
      <li>방문 장소 통계 및 히트맵 시각화</li>
      <li>WebSocket 기반 실시간 위치 공유</li>
      <li>모바일 경험 확장을 위한 PWA 도입</li>
    </ul>
  </div>
</div>

<h2 class="mt-20 text-2xl font-bold italic" v-click v-motion :initial="{ opacity: 0 }" :enter="{ opacity: 1 }">감사합니다!</h2>

<!--
이번 프로젝트에서 얻은 가장 큰 수확은 CI/CD 파이프라인을 직접 운영하며 DevOps 전체 흐름을 체감한 것과, 장애 발생 시 원인을 레이어별로 분리해서 분석하는 디버깅 사고방식을 기른 점입니다. 앞으로는 방문 장소 기반 히트맵 시각화와, SSE의 한계를 넘은 WebSocket 기반 실시간 위치 공유를 도입할 계획입니다. 단순한 기록을 넘어, 사용자의 삶의 지도를 완성해가는 MapLog로 계속 발전시켜 나가겠습니다. 감사합니다.
-->

---
layout: default
---

# 07-2. 마무리:<br>팀원별 프로젝트 회고

<div class="grid grid-cols-2 gap-4 pt-2 text-left">
  <div v-click="1" v-motion :initial="{ scale: 0.9, opacity: 0 }" :enter="{ scale: 1, opacity: 1 }" class="bg-[#161b22] border border-gray-800 p-4 rounded-xl shadow-lg relative overflow-hidden">
    <carbon:quotes class="absolute top-2 right-2 text-blue-500/10 text-4xl" />
    <h3 class="text-blue-400 mb-2 font-bold flex items-center gap-2"><carbon:user-avatar/> 김선일</h3>
    <p class="text-[10px] text-gray-300 italic leading-relaxed">
      "독감으로 참여가 적어 아쉬웠지만 도커-젠킨스-쿠버네티스 파이프라인을 직접 경험해본 건 큰 수확이었어요. YAML 설정에 애를 먹기도 했지만 서비스가 자동으로 살아나는 걸 보며 운영의 중요성을 깨달았습니다. 팀원들 고생 많으셨습니다."
    </p>
  </div>
  <div v-click="2" v-motion :initial="{ scale: 0.9, opacity: 0 }" :enter="{ scale: 1, opacity: 1 }" class="bg-[#161b22] border border-gray-800 p-4 rounded-xl shadow-lg relative overflow-hidden">
    <carbon:quotes class="absolute top-2 right-2 text-green-500/10 text-4xl" />
    <h3 class="text-green-400 mb-2 font-bold flex items-center gap-2"><carbon:user-avatar/> 김태형</h3>
    <p class="text-[10px] text-gray-300 italic leading-relaxed">
      "Jenkins CI 파이프라인과 ArgoCD GitOps 기반 CD 흐름을 직접 연결하며, 코드 push부터 k8s 자동 배포까지의 전체 흐름을 체감하고 깊이 이해할 수 있었던 시간이었습니다."
    </p>
  </div>
  <div v-click="3" v-motion :initial="{ scale: 0.9, opacity: 0 }" :enter="{ scale: 1, opacity: 1 }" class="bg-[#161b22] border border-gray-800 p-4 rounded-xl shadow-lg relative overflow-hidden">
    <carbon:quotes class="absolute top-2 right-2 text-orange-500/10 text-4xl" />
    <h3 class="text-orange-400 mb-2 font-bold flex items-center gap-2"><carbon:user-avatar/> 정병진</h3>
    <p class="text-[10px] text-gray-300 italic leading-relaxed">
      "단순히 도구를 사용하는 것에 그치지 않고 운영 자동화의 각 단계를 직접 설계하고 구성하며 DevOps 전반에 대한 이해를 높였습니다. 코드 변경부터 무중단 배포까지 이어지는 흐름은 정말 값진 경험이었습니다."
    </p>
  </div>
  <div v-click="4" v-motion :initial="{ scale: 0.9, opacity: 0 }" :enter="{ scale: 1, opacity: 1 }" class="bg-[#161b22] border border-gray-800 p-4 rounded-xl shadow-lg relative overflow-hidden">
    <carbon:quotes class="absolute top-2 right-2 text-purple-500/10 text-4xl" />
    <h3 class="text-purple-400 mb-2 font-bold flex items-center gap-2"><carbon:user-avatar/> 정현호</h3>
    <p class="text-[10px] text-gray-300 italic leading-relaxed">
      "자동화를 통해 개발에만 집중할 수 있는 환경의 중요성을 알게 되었고, AWS S3 등 새로운 기술을 학습해 접목하며 기술을 대하는 태도가 한층 성숙해졌습니다. 앞으로도 운영/배포 설정을 우선하여 개발 효율을 높이고자 합니다."
    </p>
  </div>
</div>

---
layout: center
class: text-center
---

# <span class="ml-logo-gradient text-5xl">Q & A</span>
### 감사합니다. 질문 있으시면 편하게 말씀해 주세요!

<div class="mt-20 flex justify-center gap-10">
  <div class="flex items-center gap-2 text-gray-400">
    <carbon:logo-github class="text-2xl"/>
    <span>github.com<br>/20251029-hanhwa-swcamp-22th<br>/be22-3rd-team3-project</span>
  </div>
  <div class="flex items-center gap-2 text-gray-400">
    <carbon:email class="text-2xl"/>
    <span>gusgh075@gmail.com</span>
  </div>
</div>
