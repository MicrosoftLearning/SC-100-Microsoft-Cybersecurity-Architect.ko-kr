---
casestudy:
  title: '사례 연구: 랜섬웨어 공격에 대한 복원력 전략 디자인'
  module: 'Module 04: Design a resiliency strategy for common cyberthreats like ransomware'
---
이 사례 연구 연습은 이 모듈에서 학습한 주제와 관련된 몇 가지 개념적 디자인 작업을 수행하는 경험을 제공하도록 설계되었습니다.

## 사례 연구: 랜섬웨어 공격에 대한 복원력 전략 디자인
 
CONTOSO는 시카고와 영국 런던에 본사를 둔 의료 서비스 회사입니다.  
이 회사는 시카고와 영국 대도시 런던 근교의 시설에서 고객에게 의료 서비스를 제공합니다.  Contoso의 시설에는 병원이 있으며, 의사, 간호사 및 기타 의료 전문가가 재직 중입니다. Contoso는 회사 전체적으로 모든 중요한 서비스를 클라우드에 마이그레이션하기 시작했습니다. 이번 마이그레이션에는 온-프레미스 서버, VM 및 지원 관리 및 모니터링 어플라이언스가 포함됩니다.

Contoso는 규정 준수 및 규정 요구 사항으로 인해 온-프레미스에서 호스트되어야 하는 일부 데이터와 애플리케이션을 제외하고 클라우드 마이그레이션을 시작했습니다. Contoso에는 Azure AD(Azure Active Directory) 테넌트가 있습니다. 이 Azure AD 온-프레미스 도메인 컨트롤러에서 유지 관리되는 AD DS(Active Directory Domain Services)와 동기화됩니다. 마이그레이션이 진행됨에 따라 SaaS 애플리케이션 등 각 부서의 필요에 따른 추가 Azure 구독이 만들어집니다. 대부분의 직원은 일상 업무를 용이하게 하기 위해 Microsoft 365 애플리케이션을 활용합니다.  
 
최근 의료 서비스 시장에서 Contoso의 경쟁업체와 관련된 여러 유명 랜섬웨어 사례가 있었습니다. CEO와 CISO는 Contoso에 랜섬웨어 공격으로 인한 위험을 완화하기 위한 계획이 아직 마련되어 있지 않다고 우려하고 있습니다. CISO는 2주 안에 회사 임원에게 제시할 랜섬웨어 대응 및 완화 계획 초안을 작성하도록 개인적으로 요청했습니다. 고위 IT 직원 중 일부는 랜섬웨어 위협이 과장되어 있으며 모든 회사에 정말로 필요한 것은 제대로 된 백업과 견고한 경계 보안이라고 목소리를 높였습니다.
 
### 요구 사항

* 랜섬웨어 대응 및 완화 계획에는 중요 온-프레미스 인프라 및 클라우드 서비스뿐만 아니라 현장 직원이 사용하는 회사 노트북 및 모바일 디바이스에 저장된 회사 데이터도 포함되어야 합니다.
* CEO와 CISO는 랜섬웨어 해커와 협상하거나 해커에게 돈을 지불하지 않을 것이라는 강경한 접근 방식을 취했습니다. 그들은 랜섬웨어 공격이 발생할 경우 12시간 이내에 중요한 시스템을 다시 작동하고 48시간 이내에 전체 기능을 복원하는 것이 목표라고 말했습니다.
* 데이터 처리 전략은 미국의 HIPAA 및 영국의 모든 관련 표준을 포함해 개인 정보 보호 요구 사항을 준수해야 합니다.
* 또한 계획에서는 랜섬웨어의 위협을 완화하기에 좋은 백업만으로 충분하지 않은 이유를 자세히 설명해야 합니다.

### 초기 질문

* 랜섬웨어 완화 계획을 수립하는 데 어떤 부서와 직원이 참여해야 하나요? 
* 계획을 통해 해결해야 하는 서비스 및 인프라 요소는 무엇인가요? 
* 강력한 랜섬웨어 대응 계획을 만들기 위한 현실적인 타임라인은 무엇인가요?
* 랜섬웨어 보호 계획을 구현하기 위한 주요 성공 측정값은 무엇인가요?

### 디자인 작업

* Azure 및 M365 서비스의 목록 작성은 데이터를 백업하고 보호하는 데 중요합니다. 각 서비스가 랜섬웨어 대응에 중요한 이유와 서로의 장단점을 설명하세요.
* 회사에서 기존 Azure 및 M365 솔루션으로 해결되지 않은 특별한 요구 사항을 기록해 둡니다.
* 계획을 통해 해결해야 하는 서비스 및 인프라 요소의 목록을 작성합니다.
* 강력한 랜섬웨어 대응 계획을 만든 다음 해당 계획을 실행하기 위해 몇 가지 현실적인 타임라인을 예측합니다. 