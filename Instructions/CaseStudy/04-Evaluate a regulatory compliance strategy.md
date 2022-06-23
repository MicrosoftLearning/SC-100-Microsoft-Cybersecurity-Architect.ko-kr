이 사례 연구 연습은 이 모듈에서 학습한 주제와 관련된 몇 가지 개념적 디자인 작업을 수행하는 경험을 제공하도록 설계되었습니다.

## <a name="case-study-evaluate-regulatory-compliance"></a>사례 연구: 규정 준수 평가

Contoso Pharma는 북아메리카 및 유럽에 있는 국제 제약 회사입니다. Contoso Pharma는 온-프레미스 및 Azure에 워크로드가 있습니다. 목표는 향후 2년 안에 모든 워크로드를 완전히 Azure에 두고 온-프레미스에는 최소한의 워크로드만 두는 것입니다. 다음은 주요 워크로드 목록입니다.

- VM(Windows 및 Linux)
- Storage 계정
- Key Vault
- SQL PaaS 및 VM의 SQL

Contoso Pharma는 또한 레드몬드에 있는 본사와 런던의 본사 사이에 사이트 간 VPN을 사용하고 있습니다. 이 VPN은 온-프레미스 리소스가 통신할 수 있도록 하는 데 사용됩니다.

Contoso Pharma는 데이터베이스를 쿼리하여 고객의 정보를 확인하는 애플리케이션에서 사용하는 웹 서버를 실행하는 Windows Server 2012 몇 개로 구성된 레거시 환경을 레드몬드에 보유하고 있습니다. 조사 결과 레거시 웹 서버와 데이터베이스의 통신은 HTTP를 통해 수행되었습니다.

### <a name="design-requirements"></a>디자인 요구 사항

Contoso Pharma는 아래 표와 같이 워크로드에 따라 규정 준수 요구 사항이 다릅니다.

| **작업** | **데이터 형식** | **규정 준수 표준** |
|:---:|:---:|:---:|
| Windows VM | 신용 카드 소유자 정보 | PCI DSS |
| SQL PaaS | 환자 건강 정보 | HIPAA |
| 스토리지 계정 | 신용 카드 소유자 정보 | PCI DSS 계정 |

이러한 표준을 준수하려면 Contoso Pharma에서 다음을 수행할 수 있어야 합니다.

- 시간 경과에 따라 규정 준수 모니터링
- 워크로드 소유자가 해당 표준을 준수하지 않는 리소스를 프로비전하지 못하도록 금지
- 환경에 배포된 새 구독이 기본적으로 필요한 표준을 사용하고 있는지 확인
- 각 지리적 위치에 프로비전된 리소스가 데이터 주권을 위해 원본 지역의 데이터를 유지하도록 보장

### <a name="design-tasks"></a>디자인 작업

* Contoso Pharma가 시간별로 규정 준수 상태를 분석할 수 있도록 하려면 어떤 도구를 사용해야 하나요? 가장 적합한 옵션을 선택하세요.
* 워크로드 소유자가 필수 표준을 따르는 리소스만 만들도록 하려면 Azure에서 어떤 서비스를 사용해야 하나요?
* 워크로드 소유자가 리소스를 만들 때 데이터를 올바른 지리적 위치에 보관하도록 하려면 어떤 옵션을 활용해야 하나요?
* Contoso Pharma는 프로비전된 VM이 PCI DSS를 준수하는지 어떻게 확인할 수 있으며 준수하지 않을 경우 수정하기 위해 어떤 작업을 수행해야 하나요?
* 데이터 암호화는 개인 정보 요구 사항을 해결하기 위한 필수 구성 요소입니다. 암호화를 적용해야 하는 데이터 스테이지는 무엇인가요?
* 워크로드 간에 데이터 암호화를 적용하는 데 사용할 수 있는 Azure 서비스는 무엇인가요?