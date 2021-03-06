---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-14"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:pre: .pre}
{:tip: .tip}
{:screen: .screen}
{:codeblock: .codeblock}

# 사인인하기 전에 속성 추가
{: #sign-in}

{{site.data.keyword.appid_full}}를 사용하는 경우 초기 사인인 이전에 앱에 대한 액세스가 필요할 것으로 판단되는 사용자의 프로파일을 빌드하기 시작할 수 있습니다.
{: shortdesc}

속성 유형에 대한 자세한 정보는 [사용자 프로파일에 대한 정보](user-profile.html)를 참조하십시오. 사용자 정의 속성 및 해당 보안 고려사항에 대한 자세한 정보는 [사용자 정의 속성](custom-attributes.html)을 참조하십시오.
{: tip}

**처음으로 사인인하기 전에 앱에 사용자에 대한 정보를 추가하는 이유가 무엇입니까?**

{{site.data.keyword.appid_short_notm}}를 사용하여 SAML ID 제공자의 기존 사용자를 연합하는 애플리케이션을 고려해 보십시오. 특정 사용자가 처음으로 애플리케이션에 사인인하는 즉시 `admin` 액세스 권한을 보유해야 할 수도 있습니다. 이렇게 되도록 하기 위해 사전 등록 엔드포인트를 사용하여 해당 사용자에 대한 사용자 정의 `admin` 속성을 설정하고 사용자 측에서 추가적인 조치를 수행하지 않고도 관리 콘솔에 대한 액세스 권한을 부여할 수 있습니다. 기본 설정을 변경함으로써 발생할 수 있는 [보안 문제](custom-attributes.html)를 고려하십시오.

**사용자를 식별하는 방법은 무엇입니까?**

다음 항목 중 하나를 사용하여 사용자를 식별할 수 있습니다.

* ID 제공자에 있는 **GUID**라는 사용자 고유 ID. 이 ID는 항상 존재하고 고유한 상태로 유지되지만 항상 즉시 사용 가능하거나 간단히 파악할 수 있는 것은 아닙니다. 예를 들어 클라우드 디렉토리의 경우 랜덤 16바이트 랜덤 GUID를 사용합니다.
* 사용 가능한 경우 사용자의 **이메일**.

**어떤 ID 제공자가 어떤 정보를 제공하는지 파악하는 방법은 무엇입니까?**

다음 표에서 사용 가능한 ID 정보의 유형을 참조하십시오.

<table>
  <thead>
    <tr>
      <th>ID
제공자</th>
      <th>GUID</th>
      <th>이메일</th>
      <th>Sub</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>클라우드 디렉토리</td>
      <td><img src="images/confirm.png" width="32" alt="사용 가능한 기능" style="width:32px;" /></td>
      <td><img src="images/confirm.png" width="32" alt="사용 가능한 기능" style="width:32px;" /></td>
      <td> </td>
    </tr>
    <tr>
      <td>Facebook</td>
      <td><img src="images/confirm.png" width="32" alt="사용 가능한 기능" style="width:32px;" /></td>
      <td><img src="images/confirm.png" width="32" alt="사용 가능한 기능" style="width:32px;" /></td>
      <td> </td>
    </tr>
    <tr>
      <td>Google</td>
      <td><img src="images/confirm.png" width="32" alt="사용 가능한 기능" style="width:32px;" /></td>
      <td><img src="images/confirm.png" width="32" alt="사용 가능한 기능" style="width:32px;" /></td>
      <td> </td>
    </tr>
    <tr>
      <td>SAML</td>
      <td></td>
      <td><img src="images/confirm.png" width="32" alt="사용 가능한 기능" style="width:32px;" /></td>
      <td> </td>
    </tr>
    <tr>
      <td>사용자 정의</td>
      <td> </td>
      <td> </td>
      <td><img src="images/confirm.png" width="32" alt="사용 가능한 기능" style="width:32px;" /></td>
    </tr>
  </tbody>
</table>

**클라우드 디렉토리는 다르게 처리됩니까?**

사전 등록된 사용자 속성의 무결성을 위해 클라우드 디렉토리는 해당 사용자에 대한 추가적인 요구사항을 배치합니다. 사전 등록은 이메일 유효성 검증이 사용으로 설정되고 확인된 경우에만 수행할 수 있습니다. 특정 속성이 포함된 클라우드 디렉토리 사용자를 사전 등록하는 경우 해당 속성은 특정 사용자를 위한 것입니다. 이메일이 먼저 확인되지 않은 경우 다른 사용자가 이메일 주소 및 해당 이메일에 지정된 속성을 청구할 수 있습니다.

이 작업을 수행하는 방법은 무엇입니까?

1. 클라우드 디렉토리를 이메일 및 비밀번호 모드로 설정하십시오. **클라우드 디렉토리** 탭의 일반 설정에 있는 UI를 통해 이 작업을 수행할 수 있습니다. [관리 API](https://appid-management.ng.bluemix.net/swagger-ui/#!/Cloud_Directory_Users/createCloudDirectoryUser)를 통해 설정할 수도 있습니다.

2. 다음 방법 중 하나를 통해 사용자 이메일 주소를 검증하여 해당 ID를 확인하십시오.

  * 이메일을 통해 사용자 ID를 확인하려면 서비스 대시보드의 **클라우드 디렉토리** 탭에서 **이메일 검증**을 **켜기**로 설정하십시오. 추가된 사용자가 먼저 해당 이메일을 확인하지 않고 앱에 사인인하는 경우 사인인은 정상적으로 완료되지만 사전 정의된 속성이 삭제됩니다.
  * 수동으로 사용자를 확인하려면 관리자여야 하며 클라우드 디렉토리 [관리 API](https://appid-management.ng.bluemix.net/swagger-ui/#!/Cloud_Directory_Users/createCloudDirectoryUser)를 사용해야 합니다. 사용자를 작성하거나 업데이트할 때 사용자 데이터 페이로드 내에서 명시적으로 `status` 필드를 `CONFIRMED`로 설정해야 합니다.

**사용자 정의 ID 제공자를 사용할 때 특별히 필요한 것이 있습니까?**

사전에 애플리케이션에 사용자 정보를 추가하는 경우 인증 플로우에서 제공되는 고유한 ID를 사용할 수 있습니다. 이 ID는 인증 요청 중에 전송되는 서명된 JSON 웹 토큰의 `sub`와 _정확히_ 일치해야 합니다. ID가 일치하지 않을 경우 추가하려는 프로파일이 정상적으로 링크되지 않습니다.



## 앱에 사용자 정보 추가
{: #add}

프로세스에 대한 정보를 확인하고 보안에 미치는 영향을 고려했으면 사용자를 추가하십시오.

**시작하기 전에:**

[/users](https://appid-management.ng.bluemix.net/swagger-ui/#!/Users/users_search_user_profile) 관리 API 엔드포인트를 사용하여 특정 사용자에 대한 사용자 정의 속성을 추가하려면 다음과 같은 정보를 알고 있어야 합니다.

* 사용자가 사인인하기 위해 사용할 ID 제공자
* ID 제공자에서 제공되는 사용자의 고유 ID

사용자가 처음으로 앱에 사인인하면 {{site.data.keyword.appid_short_notm}}에서 해당 사용자를 검색합니다. 사용자를 찾은 경우 해당 사용자는 지정된 ID를 상속받습니다. 사용자를 찾을 수 없는 경우 ID 제공자에서 제공되는 정보를 기반으로 새 사용자가 작성됩니다.

**사용자를 추가하려면 다음 작업을 수행하십시오.**

1. IBM Cloud에 로그인하십시오. 
  ```
  ibmcloud login
  ```
  {: pre}

2. 다음 명령을 실행하여 IAM 토큰을 찾으십시오.
  ```
  ibmcloud iam oauth-tokens
  ```
  {: pre}

3. 사용자에 대한 설명 및 JSON 오브젝트로 설정할 속성이 포함된 `/users` 엔드포인트에 대한 POST 요청을 작성하십시오.

  헤더:
  ```
  POST {management-url}/management/v4/{tenantId}/users
       Host: <management-server-url>
       Authorization: 'Bearer <IAM_TOKEN>'
       Content-Type: application/json
  ```
  {: pre}

  본문:
  ```
   {
       "idp": "<Identity Provider>",
       "idp-identity": "<User's unique identifier>",
       "profile": {
           "attributes": {
             "mealPreference":"vegeterian"
           }
       }
   }
  ```
  {: pre}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="아이디어 아이콘"/> 컴포넌트에 대한 정보</th>
    </thead>
    <tbody>
      <tr>
        <td><code><em>idp</em></code></td>
        <td>사용자가 인증할 ID 제공자입니다. 옵션에는 `saml`, `cloud_directory`, `facebook`, `google`, `appid_custom`, `ibmid` 등이 있습니다.</td>
      </tr>
      <tr>
        <td><code><em>idp-identity</em></code></td>
        <td>ID 제공자에서 제공하는 고유 ID입니다.</td>
      </tr>
      <tr>
        <td><code><em>profile</em></code></td>
        <td>사용자 정의 속성 JSON 맵핑이 포함된 사용자 프로파일입니다.</td>
      </tr>
    </tbody>
  </table>

  요청 예제:
  ```
  $ curl --request POST \
       --url 'https://{Management_URI}/users \
       --header 'Authorization: Bearer {IAM_TOKEN}' \
       --header 'Content-Type: application/json' \
       --data '{"idp": "saml", "idp-identity": "user@ibm.com", "profile": { "attributes": { "role": "admin",
       "frequent_flyer_points": 1000 }}}'
  ```
  {: screen}

3. 다음 방법 중 하나를 사용하여 등록이 정상적으로 완료되었는지 확인하십시오.
  * 응답에서 사용자 ID를 확인하십시오.
    ```
    {
        "id": "<{{site.data.keyword.appid_short_notm}} User Id>"
    }
    ```
    {: screen}
  * 작성된 사용자 프로파일을 확인하십시오.

사용자의 사전 정의된 속성은 해당 사용자가 처음으로 인증할 때까지 비어 있지만 사용자는 모든 면에서 완전히 인증된 사용자임을 유의하십시오. 이미 사인인했었던 사용자와 마찬가지로 해당 사용자의 고유 ID를 사용할 수 있습니다. 예를 들어 프로파일을 수정, 검색 또는 삭제할 수 있습니다.

사용자를 특정 속성과 연관시켰으면 [속성에 액세스](/docs/services/appid/custom-attributes.html)하십시오!


</br>
