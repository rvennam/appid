---

copyright:
  years: 2017, 2018
lastupdated: "2018-12-19"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:tip: .tip}


# OIDC 검색 엔드포인트 사용
{: #discovery}

OpenID Connect는 앱을 구성하고 사용자를 인증하기 위해 사용할 수 있는 정보(예: 토큰 및 공개 키)가 포함된 검색 프로토콜을 지원합니다.
{: shortdesc}


## 엔드포인트 호출
{: #call-wellknown}

`.well-known` 엔드포인트를 호출하여 검색 문서 및 해당 검색 문서에 포함된 정보를 얻을 수 있습니다.
{: shortdesc}


**어디에서 엔드포인트를 찾을 수 있습니까?**

다음 URL에서 엔드포인트를 찾을 수 있습니다.

  ```
  https://appid-oauth.[region].bluemix.net/oauth/v3/{tenantId}/.well-known/openid-configuration
  ```
  {: codeblock}

</br>

**엔드포인트에 대한 호출을 작성하는 방법은 무엇입니까?**

엔드포인트에 대한 호출을 작성하려면 올바른 `tenantID`가 있어야 하며 애플리케이션에서 검색 문서 URI를 하드코딩해야 합니다.

다음 샘플 cURL 요청을 참조하십시오.

  ```bash
  curl -X GET --header 'Accept: application/json'  'https://appid-oauth.ng.bluemix.net/oauth/v3/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/.well-known/openid-configuration'
  ```
  {:codeblock}

</br>

**호출에서 리턴할 것으로 예상할 수 있는 항목은 무엇입니까?**

응답은 다음 예제와 유사합니다.

  ```bash
  {
    "issuer" : "appid-oauth.ng.bluemix.net",
    "authorization_endpoint": "https://appid-oauth.ng.bluemix.net/oauth/v3/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/authorization",
    "token_endpoint": "https://appid-oauth.ng.bluemix.net/oauth/v3/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/token",
    "jwks_uri": "https://appid-oauth.ng.bluemix.net/oauth/v3/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/publickeys",
    "subject_types_supported": [
      "public"
    ],
    "id_token_signing_alg_values_supported": [
      "RS256"
    ],
    "userinfo_endpoint": "https://appid-oauth.ng.bluemix.net/oauth/v3/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/userinfo",
    "scopes_supported": [
      "openid"
    ],
    "response_types_supported": [
      "code"
    ],
    "claims_supported": [
      "iss",
      "aud",
      "exp",
      "tenant",
      "iat",
      "sub",
      "nonce",
      "amr",
      "oauth_client"
    ],
    "grant_types_supported": [
      "authorization_code",
      "password",
      "refresh_token",
      "client_credentials",
      "urn:ietf:params:oauth:grant-type:jwt-bearer"
    ],
    "profiles_endpoint": "https://appid-profiles.ng.bluemix.net",
    "service_documentation": "https://console.bluemix.net/docs/services/appid/index.html"
  }
  ```
  {: screen}

  <table>
    <tr>
      <th> 컴포넌트 </th>
      <th> 설명 </th>
    </tr>
    <tr>
    <td><code>issuer</code></td>
    <td>OIDC 제공자의 위치입니다.</td>
    </tr>
    <tr>
      <td><code>authorization_endpoint</code></td>
      <td>{{site.data.keyword.appid_short_notm}} OAuth 2.0 권한 엔드포인트의 URL입니다.</td>
    </tr>
    <tr>
      <td><code>token_endpoint</code></td>
      <td>{{site.data.keyword.appid_short_notm}} OAuth 2.0 토큰 엔드포인트의 URL입니다.</td>
    </tr>
    <tr>
      <td><code>jwks_uri</code></td>
      <td>{{site.data.keyword.appid_short_notm}} 웹 키 세트 문서의 URL입니다.</td>
    </tr>
    <tr>
      <td><code>subject_types_supported</code></td>
      <td>{{site.data.keyword.appid_short_notm}}에서 지원하는 주제 ID 유형의 목록이 포함된 JSON 배열입니다.</td>
    </tr>
    <tr>
      <td><code>id_token_signing_alg_values_supported</code></td>
      <td>{{site.data.keyword.appid_short_notm}} 서버에서 지원하는 JWS 서명 알고리즘의 목록이 포함된 JSON 배열입니다.</td>
    </tr>
    <tr>
      <td><code>userinfo_endpoint</code></td>
      <td>{{site.data.keyword.appid_short_notm}} 사용자 정보 엔드포인트의 URL입니다.</td>
    </tr>
    <tr>
      <td><code>scopes_supported</code></td>
      <td>{{site.data.keyword.appid_short_notm}}에서 지원하는 OAuth 2.0 범위 값의 목록이 포함된 JSON 배열입니다.</td>
    </tr>
    <tr>
      <td><code>response_types_supported</code></td>
      <td>{{site.data.keyword.appid_short_notm}}에서 지원하는 OAuth 2.0 응답 유형 값의 목록이 포함된 JSON 배열입니다.</td>
    </tr>
    <tr>
      <td><code>claims_supported</code></td>
      <td>청구 이름의 목록이 포함된 JSON 배열입니다.</td>
    </tr>
    <tr>
      <td><code>grant_types_supported</code></td>
      <td>{{site.data.keyword.appid_short_notm}}에서 지원하는 OAuth 2.0 권한 부여 유형 값의 목록이 포함된 JSON 배열입니다.</td>
    </tr>
    <tr>
      <td><code>profiles_endpoint</code></td>
      <td>{{site.data.keyword.appid_short_notm}} 사용자 프로파일 엔드포인트의 URL입니다.</td>
    </tr>
  </table>

</br>
</br>


