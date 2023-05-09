---
casestudy:
  title: 'PaaS, IaaS 및 SaaS 서비스 보안 전략 설계'
  module: 'Module 8: Design a strategy for securing PaaS_IaaS_and_SaaS services'
---

이 사례 연구 연습은 이 모듈에서 학습한 주제와 관련된 몇 가지 개념적 디자인 작업을 수행하는 경험을 제공하도록 설계되었습니다.

## <a name="case-study-securing-paas-iaas-and-saas-services"></a>사례 연구: PaaS, IaaS 및 SaaS 서비스 보안

Tailwind Traders는 가상의 주택 개조용품 소매업체입니다. 이 회사는 전 세계와 온라인에서 철물 소매 매장을 운영하고 있습니다. Tailwind Traders CISO는 Azure에서 제공하는 기회를 알고 있지만 강력한 보안과 견고한 클라우드 아키텍처의 필요성도 이해하고 있습니다. 강력한 보안과 훌륭한 참조 아키텍처가 없으면 회사는 추적 및 제어하기 어려운 Azure 환경 및 비용을 관리하는 데 어려움을 겪을 수 있습니다. CISO는 Azure가 보안 표준을 관리하고 적용하는 방법을 이해하는 데 관심이 있습니다.

## <a name="requirements"></a>요구 사항

이 비전을 달성하기 위해 CIO는 새로운 CISO(최고 정보 보안 책임자)를 고용했습니다. 새로운 CISO는 PaaS, IaaS 및 SaaS 워크로드를 보호하기 위한 전략을 계획하기 시작했으며, 이 전략의 일환으로 회사에서 다음을 수행해야 한다고 설정했습니다.

-   VM 및 컨테이너에 대한 네이티브 취약성 평가를 제공하고 Cosmos DB에 대한 위협 탐지를 지원할 수 있는 클라우드 보안 태세 관리 플랫폼을 구현합니다.
-   SQL 데이터베이스 및 스토리지 계정에서 데이터를 분류하고 레이블을 지정할 수 있는 Azure 워크로드에 대한 데이터 분류 시스템을 구현합니다.
-   Microsoft 365 SaaS 워크로드에 대한 보안 기준 구현 
-   IoT 워크로드에 대한 보안 상태 관리 및 위협 탐지 지원

## <a name="design-tasks"></a>디자인 작업

* 다음과 같은 기능을 제공하는 솔루션:
   - Azure에서 데이터 분류 및 레이블 지정을 제공
   - VM, 컨테이너 및 Cosmos DB에 대한 클라우드 보안 상태 관리 및 위협 탐지를 제공
* IoT에 대한 클라우드 보안 상태 관리 및 위협 탐지를 제공하는 데 사용해야 하는 솔루션은 무엇인가요?

