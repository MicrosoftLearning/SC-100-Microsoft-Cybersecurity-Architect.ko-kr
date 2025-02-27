---
lab: 3
title: 연습 7 - 위협 헌팅
---


# 랩 3 - 연습 7 - 콘텐츠 검색 Microsoft Purview

회사는 무결성 및 보안에 심각한 위협이 되는 상황을 직면하고 있습니다. 상당수의 악성 전자 메일이 감지되었습니다. 수행할 작엄은 이 전자 메일의 보낸 사람 주소 또는 제목 줄을 식별하는 것입니다. 이러한 악성 전자 메일을 식별한 후에는 메일을 한꺼번에 삭제해야 합니다.

## 1부: 솔루션 설계(필수)

### 디자인 접근 방법

첫 번째 단계는 악의적인 전자 메일을 식별하는 것입니다. 따라서 현재 메일 흐름을 분석해야 합니다. Microsoft Defender 포털은 이 트래픽을 조사하기 위한 추가 기능과 함께 모든 발신 및 수신 메일 흐름에 대한 포괄적인 개요를 제공합니다. 수정 작업에는 악성 전자 메일 삭제가 포함됩니다. 

### 제안된 솔루션

|요건|솔루션|작업 플랜:|
|----|----|----|
|악성 전자 메일 식별|Office 365용 Microsoft Defender|메일 흐름 조사 및 메일 헤더 분석|
|악성 전자 메일에서 발생하는 위험 제거|Office 365용 Microsoft Defender|악성 메일 삭제|

### 작업 1: 악성 메일 조사

Microsoft Defender 포털에서 수신 메일을 조사하고 의심스러운 메일과 악성 콘텐츠가 포함된 메일을 식별합니다.

1. 관리자 계정 **MOD Administrator**를 사용하여 Allan Deyoung으로 Microsoft Defender 포털 **https://security.microsoft.com**에 로그인합니다.
1. Microsoft Security 포털에서 **이메일 및 협업**을 확장한 다음 **탐색기**를 선택합니다.
1. **탐색기** 페이지에서 지난 30일의 모든 메일 흐름을 확인할 수 있도록 쿼리 기간을 조정하고 **새로 고침**을 선택합니다.
1. 결과에는 모든 수신 및 발신 메일이 표시됩니다. 메일 흐름 및 메일 제목을 조사합니다.
1. "기한이 오늘까지인 작업이 있습니다"라는 제목의 메일은 의심스러워 보입니다.
1. 추가 조사를 수행할 대상을 선택합니다.
1. 새로 나타난 **기한이 오늘까지인 작업이 있습니다** 창에서 제공된 정보를 확인하고 **헤더 보기**를 선택합니다.
1. **전자 메일 메시지 머리글** 창에서 **메시지 헤더 복사**를 선택하고 **Microsoft 메시지 헤더 분석기**를 선택합니다.
1. 브라우저에서 새 탭이 열립니다.
1. **메시지 헤더 분석기** 페이지에서 메시지 헤더를 붙여넣고 **헤더 분석**을 선택합니다.
1. 결과를 검토합니다.

Microsoft Defender 포털에서 메일 흐름을 성공적으로 검토했습니다.

### 작업 2: 악성 메일 대량 삭제 수행

"기한이 오늘까지인 작업이 있습니다"라는 제목의 메일이 피싱 공격이라는 결론에 도달했습니다. 수정 작업에는 Powershell을 사용하여 해당 메일을 대량으로 삭제하는 작업이 포함됩니다.

>[!NOTE] Exchange Online PowerShell 모듈이 이미 설치되어 있어야 합니다. 모듈이 없는 경우 다음 모듈 설치 지침을 따릅니다.

1. 관리자 권한 PowerShell 창을 엽니다. 이렇게 하려면 마우스 오른쪽 단추로 Windows 단추를 선택한 다음, **Windows PowerShell(관리자)** 을 선택합니다.
1. **사용자 계정 컨트롤** 창에서 **예**를 선택하여 실행을 확인합니다.
1. 다음 cmdlet을 입력하여 최신 Exchange Online PowerShell 모듈 버전을 설치합니다.

    ```powershell
    Install-Module ExchangeOnlineManagement
    ```
1. 신뢰할 수 없는 리포지토리 보안 대화 상자가 표시되면 Yes에 해당하는 **Y** 키를 누르고 **Enter** 키를 누릅니다.  이 프로세스를 완료하는 데 약간의 시간이 걸릴 수 있습니다.
1. 관리자 권한 PowerShell 창을 엽니다. 이렇게 하려면 마우스 오른쪽 단추로 Windows 단추를 선택한 다음, **Windows PowerShell(관리자)** 을 선택합니다.
1. **사용자 계정 컨트롤** 창에서 **예**를 선택하여 실행을 확인합니다.
1. 다음 cmdlet을 입력하여 보안 및 규정 준수 PowerShell에 연결합니다.

    ```powershell
    Connect-IPPSSession
    ```

1. **로그인** 창이 표시되면 admin@WWLxZZZZZZ.onmicrosoft.com으로 로그인하고(여기서 ZZZZZZ는 랩 호스팅 공급자가 제공한 고유 테넌트 ID임) 테넌트의 관리 암호를 사용합니다.
1. 다음 cmdlet을 입력하여 콘텐츠 검색을 수행하고 cmdlet의 기간을 지정합니다.

    ```powershell
    $Search=New-ComplianceSearch -Name "Purge phishing messages" -ExchangeLocation All -ContentMatchQuery '(Received:mm/dd/yyyy..mm/dd/yyyy) AND (Subject:"You have tasks due today")'
    Start-ComplianceSearch -Identity $Search.Identity
    ```
1. 메시지를 영구 삭제하려면 다음 cmdlet을 입력합니다.

    ```powershell
    New-ComplianceSearchAction -SearchName "Purge phishing messages" -Purge -PurgeType HardDelete
    ---
You have successfully performed a mass-deletion of malicious mails.