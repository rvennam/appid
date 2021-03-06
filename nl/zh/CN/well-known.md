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


# 使用 OIDC 发现端点
{: #discovery}

OpenID Connect 支持一种发现协议，该协议包含可用于配置应用程序和认证用户的信息，如令牌和公用密钥。
{: shortdesc}


## 调用端点
{: #call-wellknown}

您可以通过调用 `.well-known` 端点来获取发现文档及其包含的信息。
{: shortdesc}


**在哪里可以找到该端点？**

您可以在以下 URL 处找到该端点：

  ```
  https://appid-oauth.[region].bluemix.net/oauth/v3/{tenantId}/.well-known/openid-configuration
  ```
  {: codeblock}

</br>

**如何对该端点进行调用？**

要对该端点进行调用，您必须具有有效的 `tenantID`，并且必须对应用程序中的发现文档 URI 进行硬编码。

请查看以下样本 cURL 请求：

  ```bash
  curl -X GET --header 'Accept: application/json'  'https://appid-oauth.ng.bluemix.net/oauth/v3/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/.well-known/openid-configuration'
  ```
  {:codeblock}

</br>

**调用应该会返回什么内容？**

响应应该类似于以下示例：

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
      <th> 组成部分</th>
      <th> 描述</th>
    </tr>
    <tr>
    <td><code>issuer</code></td>
    <td>OIDC 提供者的位置。</td>
    </tr>
    <tr>
      <td><code>authorization_endpoint</code></td>
      <td>{{site.data.keyword.appid_short_notm}} OAuth 2.0 授权端点的 URL。</td>
    </tr>
    <tr>
      <td><code>token_endpoint</code></td>
      <td>{{site.data.keyword.appid_short_notm}} OAuth 2.0 令牌端点的 URL。</td>
    </tr>
    <tr>
      <td><code>jwks_uri</code></td>
      <td>{{site.data.keyword.appid_short_notm}} Web 密钥集文档的 URL。</td>
    </tr>
    <tr>
      <td><code>subject_types_supported</code></td>
      <td>一个 JSON 数组，包含 {{site.data.keyword.appid_short_notm}} 支持的主体标识类型的列表。</td>
    </tr>
    <tr>
      <td><code>id_token_signing_alg_values_supported</code></td>
      <td>一个 JSON 数组，包含 {{site.data.keyword.appid_short_notm}} 服务器支持的 JWS 签名算法的列表。</td>
    </tr>
    <tr>
      <td><code>userinfo_endpoint</code></td>
      <td>{{site.data.keyword.appid_short_notm}} userinfo 端点的 URL。</td>
    </tr>
    <tr>
      <td><code>scopes_supported</code></td>
      <td>一个 JSON 数组，包含 {{site.data.keyword.appid_short_notm}} 支持的 OAuth 2.0 作用域值的列表。</td>
    </tr>
    <tr>
      <td><code>response_types_supported</code></td>
      <td>一个 JSON 数组，包含 {{site.data.keyword.appid_short_notm}} 支持的 OAuth 2.0 响应类型值的列表。</td>
    </tr>
    <tr>
      <td><code>claims_supported</code></td>
      <td>一个 JSON 数组，包含声明名称的列表。</td>
    </tr>
    <tr>
      <td><code>grant_types_supported</code></td>
      <td>一个 JSON 数组，包含 {{site.data.keyword.appid_short_notm}} 支持的 OAuth 2.0 授权类型值的列表。</td>
    </tr>
    <tr>
      <td><code>profiles_endpoint</code></td>
      <td>{{site.data.keyword.appid_short_notm}} 用户概要文件端点的 URL。</td>
    </tr>
  </table>

</br>
</br>


