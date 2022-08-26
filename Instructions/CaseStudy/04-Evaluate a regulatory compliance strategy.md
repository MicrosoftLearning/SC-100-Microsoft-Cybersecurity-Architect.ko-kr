---
casestudy:
  title: '사례 연구: 규정 준수 평가'
  module: 'Module 4: Evaluate a regulatory compliance strategy'
---

이 사례 연구 연습은 이 모듈에서 학습한 주제와 관련된 몇 가지 개념적 디자인 작업을 수행하는 경험을 제공하도록 설계되었습니다.

## <a name="case-study-evaluate-regulatory-compliance"></a>사례 연구: 규정 준수 평가

Contoso Pharma is an international pharmaceutical industry with a presence in North America and Europe. Contoso Pharma has workloads on-premises and in Azure. The goal is that in the next two years, all workloads will be fully in Azure and there will be minimum workloads on-premises. Below is a list of their major workloads:

- VM(Windows 및 Linux)
- Storage 계정
- Key Vault
- SQL PaaS 및 VM의 SQL

Contoso Pharma also has a Site-to-Site VPN between the headquarters in Redmond and the main office in London. This VPN is used to allow resources on-premises to communicate.

Contoso Pharma has a legacy environment in Redmond composed by a couple of Windows Server 2012 running a Web Server that is used by the application that queries the database to check for customer's information. Upon investigation it was noted that the communication of the legacy web server with the database is done via HTTP.

### <a name="design-requirements"></a>디자인 요구 사항

Contoso Pharma는 아래 표와 같이 워크로드에 따라 규정 준수 요구 사항이 다릅니다.

| **작업** | **데이터 형식** | **규정 준수 표준** |
|:---:|:---:|:---:|
| Windows VM | 신용 카드 소유자 정보 | PCI DSS |
| SQL PaaS | 환자 건강 정보 | HIPAA |
| 스토리지 계정 | 신용 카드 소유자 정보 | PCI DSS 계정 |

이러한 표준을 준수하려면 Contoso Pharma에서 다음을 수행할 수 있어야 합니다.

- 시간 경과에 따른 규정 준수 모니터링
- 워크로드 소유자가 해당 표준을 준수하지 않는 리소스를 프로비전하지 못하도록 금지
- 환경에 배포된 새 구독이 기본적으로 필요한 표준을 사용하고 있는지 확인
- 각 지리적 위치에 프로비전된 리소스가 데이터 주권을 위해 원본 지역의 데이터를 유지하도록 보장

### <a name="design-tasks"></a>디자인 작업

* To ensure that Contoso Pharma can analyze their compliance status over time, which tool should be utilized? Select the most appropriate option.
* 워크로드 소유자가 필수 표준을 따르는 리소스만 만들도록 하려면 Azure에서 어떤 서비스를 사용해야 하나요?
* 워크로드 소유자가 리소스를 만들 때 데이터를 올바른 지리적 위치에 보관하도록 하려면 어떤 옵션을 활용해야 하나요?
* Contoso Pharma는 프로비전된 VM이 PCI DSS를 준수하는지 어떻게 확인할 수 있으며 준수하지 않을 경우 수정하기 위해 어떤 작업을 수행해야 하나요?
* Contoso Pharma는 북아메리카 및 유럽에 있는 국제 제약 회사입니다.
* 워크로드 간에 데이터 암호화를 적용하는 데 사용할 수 있는 Azure 서비스는 무엇인가요?
