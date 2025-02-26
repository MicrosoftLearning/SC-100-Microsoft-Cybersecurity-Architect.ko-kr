# 보안 운영 센터
          

## 연습 개요

Contoso에는 엔터프라이즈 전체의 보안 인시던트를 모니터링하고 대응하는 SOC(보안 운영 센터)가 있습니다. SOC에는 보안 분석가, 보안 엔지니어 및 네트워크 엔지니어가 있습니다. SOC는 Microsoft Sentinel을 SIEM(보안 정보 및 이벤트 관리) 솔루션으로 사용하기로 결정했습니다. 엔터프라이즈 전체에서 보안 로그를 수집하고 분석하기 위해 SOC에는 Log Analytics 작업 영역이 있습니다. SOC에는 최소 권한 원칙에 따라 Log Analytics 작업 영역에 대한 액세스를 보호해야 하는 요구 사항이 있습니다. SOC에는 서로 다른 권한 요구 사항을 가진 보안 분석가와 보안 엔지니어의 두 가지 역할이 있습니다. 네트워크 팀은 Cisco Umbrella 로그에만 액세스해야 합니다.

## 1부: 솔루션 설계(필수)

이 작업에서는 Contoso의 보안 운영 센터에 대한 특정 액세스 권한을 사용하여 보안 이벤트를 모니터링하고 응답하는 개념을 설계합니다.

### 디자인 접근 방법

초기 단계에는 설명된 시나리오에 따라 요구 사항을 분석하고, 목표를 이해하고, 요구 사항을 정의하는 작업이 포함됩니다.

제공된 사용 사례에 따라 다음 요구 사항을 설명할 수 있습니다.

- SIEM/SOAR 솔루션 배포
- 특정 SOC 역할에 대한 액세스 제한
- 인시던트 및 해당 경고에 대한 사용자 지정 보기를 사용하여 대시보드 만들기

이 시나리오에서는 Microsoft Sentinel을 기반으로 SIEM SOAR 솔루션을 배포하고, 작업 영역 컨텍스트에서 역할 기반 액세스 제어를 설정하고, 네트워크 팀의 액세스를 Log Analytics 작업 영역의 단일 테이블로 제한합니다. 통합 문서를 사용하면 보안 분석가와 관리자가 그래픽 디스플레이를 사용하여 보안 데이터를 시각화할 수 있습니다. 대시보드에서 데이터를 표시하고 분석하기 위한 도구를 제공합니다.

### 제안된 솔루션

| 요건 | 솔루션 | 작업 플랜: |
| ---- | ---- | ---- |
| SIEM/SOAR 솔루션 배포 | Microsoft Sentinel, Log Analytics 작업 영역 | Log Analytics 작업 영역 설정 및 Microsoft Sentinel 배포 |
| 특정 SOC 역할에 대한 액세스 제한 | Log Analytics 작업 영역, 역할 기반 액세스 제어 | Log Analytics 작업 영역을 위한 RBAC 설정 |
| 인시던트 및 해당 경고에 대한 사용자 지정 보기를 사용하여 대시보드 만들기 | Microsoft Sentinel, 통합 문서 | 현재 인시던트 및 경고에 대한 사용자 지정 보기를 사용하여 통합 문서 만들기 |

## 파트 2: 솔루션 구현(선택 사항)

### 작업 1: Log Analytics 작업 영역 만들기

이 작업에서는 Microsoft Sentinel이 감지 및 분석을 위해 수집하고 사용할 모든 데이터를 보관하는 데 필요한 Log Analytics 작업 영역을 만듭니다.

1. **lon-sc1\admin** 계정으로 클라이언트 1 VM(LON-SC1)에 로그인합니다. 암호는 랩 호스팅 공급자가 제공합니다.
2. **Microsoft Edge**를 열고 주소 표시줄을 선택하고 **`https://portal.azure.com`** 으로 이동하고 Azure Porta에 **User1-*******@LODSUATMCA.onmicrosoft.com**으로 로그인합니다(여기서 ******은(는) 랩 호스팅 공급자가 제공하는 고유한 테넌트 ID).  암호는 랩 호스팅 공급자가 제공합니다.
3. 로그인 상태를 유지하시겠습니까? 대화 상자에서 이 메시지를 다시 표시 안 함 체크박스를 선택하고 **아니요**를 선택합니다.
4. 브라우저에 기본 전역 관리자 자격 증명을 저장하지 않으려면 안 함을 선택하여 하단에서 암호 저장 대화 상자를 닫습니다.
5. Microsoft Azure 시작 화면에서 취소를 선택합니다.
6. **리소스 만들기**를 선택하고 **Log Analytics 작업 영역**을 검색합니다.
7. **Log Analytics 작업 영역 타일**을 찾아 **만들기**를 선택합니다.
8. Log Analytics 작업 영역 만들기 사이트에서 새 **리소스 그룹**을 만들고 **`rg_eastus_soc`**(으)로 이름을 지정합니다.
9. 인스턴스 세부 정보에서 이름을 **`law-sentinel`**(으)로 입력하고 지역에서 **미국 동부**를 선택합니다.
10. **검토 및 만들기**를 선택합니다.
11. **만들기**를 선택하여 배포를 시작합니다.

Sentinel 배포를 위한 Log Analytics 작업 영역을 성공적으로 만들었습니다.

### 작업 2 - Sentinel 만들기

이 작업에서는 만든 Log Analytics 작업 영역에 Sentinel을 추가하고, 데모 로그를 추가합니다. 데모 테넌트는 Log Analytics 작업 영역에 기존 데이터가 없기 때문에 데모 로그를 가져오면 Sentinel의 작동 방식을 더 잘 파악할 수 있습니다.

1. 아직 Azure Portal **https://portal.azure.com**에 로그인한 상태여야 합니다.
1. 페이지 맨 위의 파란색 배너에 있는 검색 창에 **Microsoft Sentinel**을 입력한 다음 서비스 아래의 검색 결과 목록에서 선택합니다.
1. **Microsoft Sentinel** 페이지에서 **만들기**를 선택합니다.
1. **작업 영역에 Microsoft Sentinel 추가 페이지**의 목록에 앞서 만든 Log Analytics 작업 영역이 있어야 합니다.  **law-sentinel**을 선택한 다음 **추가**를 선택합니다.
1. Sentinel을 작업 영역에 추가하는 데 몇 분 정도 걸릴 수 있습니다.  추가되면 **Microsoft Sentinel | 새 기능 및 가이드** 페이지가 표시됩니다.  Microsoft Sentinel 평가판이 활성화되었다는 알림을 받게 됩니다.  **확인**을 선택합니다.
1. 페이지 가운데에서 **콘텐츠 허브로 이동**을 선택합니다.  또는 왼쪽 탐색 패널에서 **콘텐츠 관리**를 펼친 다음 **콘텐츠 허브**를 선택합니다.
1. 콘텐츠 허브에서 **Microsoft Sentinel 학습 랩**을 설치 합니다.  이 솔루션을 찾으려면 **공급자 = Microsoft** 및 **카테고리 = 학습 및 자습서**를 필터링합니다. 이러한 필터를 설정하고 적용하면 **Microsoft Sentinel 학습 랩**이 나열됩니다. 검색 결과에서 선택한후 솔루션을 **설치합니다**.
1. **만들기**를 실행합니다.
1. 리소스 그룹 **rg_eastus_soc** 및 작업 영역**law-sentinel**을 선택합니다.
1. **검토 및 만들기**, **만들기**를 차례로 선택합니다.
1. 솔루션이 성공적으로 설치될 때까지 기다립니다.

Log Analytics 작업 영역에 Sentinel을 성공적으로 배포하고 데이터를 추가했습니다. 

### 작업 3 - RBAC 설정

최소 권한에 따라 액세스를 보호해야 하며 특정 역할 요구 사항에 대한 역할 할당을 만듭니다. 예정된 생산적 배포에서는 보안 운영 센터에 두 가지 역할이 있습니다.
또한 네트워크 팀은 Cisco Umbrella 로그에 액세스해야 합니다. 네트워크 팀이 이러한 로그에만 액세스할 수 있는지 확인해야 합니다.

#### 사용 권한 요구 사항

| 역할 | 사용 권한 |
|---|---|
| 보안 분석가 | 데이터,인시던트, 통합 문서 및 기타 Azure 리소스 보기 |
| | 인시던트 할당/해제 |
| 보안 엔지니어 | 통합 문서 및 분석 규칙 만들기 및 편집 |
| | 콘텐츠 허브에서 솔루션 설치 및  |
| 네트워크 팀 | 그룹에 대한 읽기 권한: 테이블에 대한 **NOC** : **Cisco_Umbrella_dns_CL**|

---

1. 아직 Azure Portal **https://portal.azure.com**에 로그인한 상태여야 합니다.
1. 위쪽 검색 표시줄에서 **리소스 그룹**을 검색하고 이전에 만든 리소스 그룹 **rg_eastus_soc**를 선택합니다.
1. 왼쪽 탐색 창에서 **액세스 제어(IAM)** 를 선택합니다.
1. **추가**를 선택하고 드롭다운에서 **역할 할당 추가**를 선택합니다.
1. **`Microsoft Sentinel Responder`** 을(를) 검색하고 세부 정보 열에서 **보기**를 검색합니다.
1. 사용 권한이 요구 사항에 부합하는지 검토합니다.
1. 오른쪽 위 모서리에 있는 **X**를 눌러 창을 닫습니다.
1. **다음**을 선택합니다.
1. **+구성원 선택**을 선택합니다.
1. **`SOC Analysts`** 그룹을 검색하고, 검색 결과에서 **SOC 분석가**를 선택하고, **선택**을 누르고 역할 할당을 추가합니다.
1. **검토 + 할당**을 선택합니다.
1. Sentinel 자 역할에 대한 단계를 반복합니다. **추가**를 선택하고 드롭다운에서 **역할 할당 추가**를 선택합니다.
1. **`Microsoft Sentinel Contributor`** 검색 후 이 역할을 선택합니다.
1. **다음**을 선택합니다.
1. **+구성원 선택**을 선택합니다.
1. **구성원 선택** 블레이드에서 **SOC 엔지니어** 그룹을 검색합니다.  검색 결과에서 **SOC 엔지니어**를 선택하고 **Select**를 눌러 역할 할당을 추가합니다.
1. **검토 + 할당**을 다시 선택합니다.
1. **역할 할당 탭**을 선택하고 역할 할당이 설정되어 있음을 확인합니다.
1. 이제 사용자 지정 역할을 추가해 보겠습니다. **추가**를 선택하고 드롭다운에서 **사용자 지정 역할 추가**를 선택합니다.
1. 이름을 **`NOC-CiscoUmbrellaCL-Read`**(으)로 지정합니다.
1. **기준 권한**에서는 **처음부터 시작**을 선택합니다.
1. **다음**을 선택합니다.
1. **사용 권한** 탭에서 **권한 추가**를 선택합니다.
1. **`Microsoft.OperationalInsights`** 검색 후 **Azure Log Analytics** 카드를 선택합니다.
1. 다음 권한을 추가합니다.
    - Microsoft.OperationalInsights/workspaces
        - 읽기: 작업 영역 가져오기
        - 기타: 작업 영역 데이터 검색

    - Microsoft.OperationalInsights/workspaces/analytics
        - 기타: 검색

    - Microsoft.OperationalInsights/workspaces/query
        - 읽기: 작업 영역에서 데이터 쿼리

    - Microsoft.OperationalInsights/workspaces/tables/query
        - 읽기: 작업 영역 테이블 데이터 쿼리

1. **검토 + 생성**를 선택합니다.
1. **만들기**를 선택하고 **확인**을 선택합니다.
1. 위쪽 검색 창에서 **`Resource groups`** 검색 후 **rg_eastus_soc**를 선택합니다.
1. Log Analytics 작업 영역 **law-sentinel**을 엽니다.
1. 왼쪽 탐색 창에서 **설정**을 펼치고 **테이블**을 선택합니다.
1. **`Cisco_Umbrella_dns_CL`** 를 검색합니다.
1. 줄임표(...)를 클릭하고 **액세스 제어(IAM)** 를 선택합니다.
1. **추가** > **역할 할당 추가**를 선택합니다.
1. **`NOC-CiscoUmbrellaCL-Read`** 검색 후 사용자 지정 역할을 선택합니다.
1. **다음**을 선택합니다.
1. **구성원 선택**을 선택하고 **NOC**로 검색하여 검색 결과에서 선택한 다음 **Select** 키를 누릅니다.
1. **검토 + 할당**을 다시 선택합니다.

Contoso 보안 운영 팀의 역할 요구 사항에 맞춘 역할 기반 액세스 모델을 만들고, 네트워크 팀을 위한 사용자 지정 역할을 만들고, Log Analytics 작업 영역의 특정 테이블에 역할을 할당하는 데 성공했습니다.

### 작업 4 - 통합 문서 만들기

이 작업에서는 통합 문서를 만들어 사용자 지정 보기 및 현재 인시던트와 그 경고가 포함된 대시보드를 생성합니다.

1. 아직 Azure Portal **https://portal.azure.com**에 로그인한 상태여야 합니다.
1. 위쪽 검색 창에서 **`Microsoft Sentinel`** 검색 후 엽니다.
1. **law-sentinel**을 선택합니다.
1. 왼쪽 탐색 창에서 **위협 관리**를 펼치고 **통합 문서**를 선택합니다.
1. **통합 문서 추가**를 선택합니다.
1. **편집**을 선택합니다.
1. 오른쪽에서 첫 번째 **편집** 단추를 선택합니다.
1. **추가** > **매개 변수 추가**를 선택합니다.
1. **매개 변수 추가**를 선택하고 다음 정보를 입력합니다.
     - **매개 변수 이름:** TimeRange
     - **매개 변수 형식:** 시간 범위 선택기
1. 다음 설정을 확인하세요.
     - **필수 여부**
1. **저장**을 선택합니다.
1. 왼쪽 아래의 **TimeRange:** 드롭다운 메뉴에서 **지난 7일**을 선택합니다.
1. **매개 변수 추가**를 선택하고 다음 정보를 입력합니다.
     - **매개 변수 이름:** AlertSeverity
     - **매개 변수 형식:** 드롭다운
1. 다음 설정을 확인하세요.
     - **필수 여부**
     - **복수 선택 허용**
     - **읽기 모드에서 매개 변수 숨기기**
1. **Log Analytics 작업 영역 로그 쿼리** 아래의 다음에 붙여넣습니다.

    ```KQL
    SecurityAlert
    | summarize Count = count() by AlertSeverity
    | order by Count desc, AlertSeverity
    | project Value = AlertSeverity, Label = strcat(AlertSeverity, ' - ', Count)
    ```

1. **시간 범위** 드롭다운 메뉴에서 **TimeRange**를 선택합니다.
1. **드롭다운에서 포함**으로 스크롤하여 **모두**를 선택하고 **기본으로 선택한 항목**을 **모두**로 설정합니다.
1. **저장**을 선택합니다.
1. **매개 변수 추가**를 선택하고 다음 정보를 입력합니다.
     - **매개 변수 이름:** ProductName
     - **매개 변수 형식:** 드롭다운
1. 다음 설정을 확인하세요.
     - **필수 여부**
     - **복수 선택 허용**
     - **읽기 모드에서 매개 변수 숨기기**

1. **Log Analytics 작업 영역 로그 쿼리** 아래의 다음에 붙여넣습니다.

    ```KQL
    SecurityAlert
    | summarize Count = count() by ProductName
    | order by Count desc, ProductName asc
    | project Value = ProductName, Label = strcat(ProductName, ' - ', Count)
    ```

1. **시간 범위** 드롭다운 메뉴에서 **TimeRange**을 선택합니다.
1. **드롭다운에서 포함**으로 스크롤하여 **모두**를 선택하고 **기본으로 선택한 항목**을 **모두**로 설정합니다.
1. **저장**을 선택합니다.
1. **추가**를 선택하고 **쿼리 추가**를 선택합니다.
1. **Log Analytics 작업 영역 로그 쿼리** 아래의 다음에 붙여넣습니다.

    ```KQL
    SecurityIncident
    | where CreatedTime {TimeRange:value}
    | summarize arg_max(TimeGenerated,*) by tostring(IncidentNumber)
    | extend IncidentID = IncidentName
    | extend Alerts = extract("\\[(.*?)\\]", 1, tostring(AlertIds))
    | mv-expand AlertIds to typeof(string)
    | join
    (
        SecurityAlert
        | extend AlertEntities = parse_json(Entities)
        | mv-expand AlertEntities
    ) on $left.AlertIds == $right.SystemAlertId
    | summarize AlertCount=dcount(AlertIds) by IncidentNumber, Status, Severity, Title, Alerts, IncidentUrl, IncidentID
    | project IncidentNumber, IncidentID, Title, Severity, Status, AlertCount, Alerts, IncidentUrl
    | order by Severity
    ```

1. 시간 범위 드롭다운 메뉴를 통해 **TimeRange**를 .
동적 콘텐츠를 설정하여 선택한 인시던트에 대한 모든 경고를 활성화합니다. 경고를 내보내고 이 쿼리 외부에서 사용할 수 있습니다.
1. **편집 쿼리 **창의 맨 위에 있는 **고급 설정** 탭을 선택합니다.
1. 다음 설정을 확인하세요.
    - **항목이 선택되면 매개 변수 내보내기** 
1. **매개 변수 추가**를 선택하고 다음 정보를 입력합니다.
    - **내보낼 필드:** 경고
    - **매개 변수 이름:** 경고
1. **저장**을 선택합니다.
1. **설정** 탭으로 되돌아갑니다.
1. **쿼리 실행**을 선택합니다.
1. **열 설정**을 선택합니다.
1. **IncidentUrl**을 선택합니다.
1. 열 렌더러를 **링크**로 설정합니다.
1. 링크 설정에서 **열어 보기**를 **URL**로 설정합니다.
1. **저장 후 닫기**를 선택합니다.
1. 다음으로 선택한 인시던트에 따라 경고 보기를 만듭니다.
1. **쿼리 항목 편집** 창 아래쪽에서 **+ 추가**를 선택합니다. **쿼리 추가**를 선택합니다.
1. Log Analytics 작업 영역 로그 쿼리에 KQL 붙여넣기

    ```KQL
    SecurityAlert
    | where SystemAlertId in ({Alerts})
    | summarize by  DisplayName, StartTime, EndTime,  SystemAlertId
    | sort by EndTime desc
    ```

1. 시간 범위 드롭다운에서 **TimeRange**를 선택합니다.
1. **편집 완료**를 선택합니다.
1. **새 통합 문서** 창의 상단 바에서 **편집 완료**를 선택합니다.
1. **인시던트**를 선택합니다.
1. 연결된 인시던트에 대한 경고는 아래에 표시됩니다.
1. 저장 아이콘을 선택하여 쿼리를 저장합니다.  
1. **다른 이름으로 저장** 창에서 새 통합 문서의 제목을 입력하고 드롭다운에서 **rg_eastus_soc** 리소스 그룹을 선택한 다음 **다른 이름으로 저장**을 선택합니다.

인시던트 및 관련 경고에 대한 사용자 지정 보기가 있는 대시보드를 성공적으로 만들었습니다.
