---

copyright:
  years: 2017, 2018
lastupdated: "2018-10-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:tip: .tip}

# 將您的應用程式加上品牌
{: #branding}

您可以顯示自己的自訂畫面、使用自己的登入流程，以及利用 {{site.data.keyword.appid_full}} 的鑑別及授權功能。藉由使用「雲端目錄」作為身分提供者，使用者就可以在您提供較少協助的情況下，與您的應用程式互動。他們可以登入、註冊、變更其密碼及其他作業，而不需要要求協助。
{: shortdesc}

**為何我想要顯示自己的畫面？**

當您重複使用現有的使用者介面時，可以為您的應用程式建立統一的登入流程。藉由使用相同的影像、顏色和品牌行銷，即使沒有直接與您的應用程式互動，您的使用者還更有可能辨識您的品牌。

</br>

**需要哪種類型的配置來顯示我自己的畫面？**

若要顯示您自己的使用者介面，您必須使用 [雲端目錄 (/docs/services/appid/cloud-directory.html)] 作為身分提供者。有數種不同的方式，可以用來[配置](cloud-directory.html)「雲端目錄」。您可以決定要傳送的訊息類型，以及自訂內容和設計。不知道該說些什麼？沒問題。GUI 中有您可以使用的範例訊息。

想要使用英文以外的[語言](cloud-directory.html#languages)嗎？您可以使用<a href="https://appid-management.ng.bluemix.net/swagger-ui/#!/Config/updateLocalization" target="_blank">語言管理 API <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 來選擇另一種語言，以顯示您自己的翻譯內容。
{: tip}

</br>



**流程在技術上有何不同？**

服務會使用 OAuth2 授與流程來對映授權處理程序。當您配置社交身分提供者（例如 Facebook）時，<a href="https://oauthlib.readthedocs.io/en/stable/oauth2/grants/authcode.html" target="_blank">授權流程 <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 是用來呼叫登入小組件。當您使用自己的畫面時，<a href="https://oauthlib.readthedocs.io/en/stable/oauth2/grants/password.html" target="_blank">資源擁有者密碼認證流程 <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 是用來提供存取及身分記號，以容許您呼叫登入畫面。

</br>

**您是否具有任何顯示此運作方式的範例應用程式嗎？**

是！請檢查下列任何範例，來查看「雲端目錄」是否運作中：

* <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-branded-ui-user-sign-app-id/" target="_blank">Use your own branded UI for user sign-in with {{site.data.keyword.appid_short_notm}} <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>
* <a href="https://www.ibm.com/blogs/bluemix/2018/06/use-ui-flows-user-sign-sign-app-id/" target="_blank">Use your own UI and Flows for User Sign-Up and Sign-in with with {{site.data.keyword.appid_short_notm}} <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>
* <a href="https://www.ibm.com/blogs/bluemix/2018/06/custom-login-page-app-id-integration/" target="_blank">Use a custom login page with  {{site.data.keyword.appid_short_notm}} <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>

</br>
</br>

## 使用 Android SDK 將應用程式加上品牌
{: #branded-ui-android}

啟用「雲端目錄」之後，即可使用 Android SDK 來呼叫自訂的畫面。您可以選擇您要使用者可與之互動的畫面組合。<a href="https://www.ibm.com/blogs/bluemix/2018/01/use-branded-ui-user-sign-app-id/" target="blank">如需詳細範例，請參閱此部落格 <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>！
{: shortdesc}

</br>

**登入**

1. 在 GUI 中配置「雲端目錄」[設定](cloud-directory.html#cd-settings)。
2. 將下列程式碼新增至應用程式。當使用者按一下您自訂畫面上的登入時，即會觸發登入流程。您可以提供一般使用者的使用者名稱及密碼，來取得存取、身分及重新整理記號。

  ```java
  AppID.getInstance().signinWithResourceOwnerPassword(getApplicationContext(), username, password,
         new TokenResponseListener() {
         @Override
        public void onAuthorizationFailure (AuthorizationException exception) {
          //Exception occurred
      }
  @Override
          public void onAuthorizationSuccess (AccessToken accessToken, IdentityToken identityToken, RefreshToken refreshToken) {
            //User authenticated
        }
      });
  ```
  {: pre}

</br>
</br>

## 使用 iOS Swift SDK 將應用程式加上品牌
{: #branded-ui-ios-swift}

啟用「雲端目錄」之後，即可使用 [iOS Swift SDK](https://github.com/ibm-cloud-security/appid-clientsdk-swift) 來呼叫您自己的品牌畫面。
{: shortdesc}

</br>

**登入**

1. 在 GUI 中配置「雲端目錄」[設定](cloud-directory.html#cd-settings)。
2. 將下列程式碼放在您的應用程式中。當使用者嘗試登入時，會呼叫您的登入畫面，而且授權及鑑別處理程序會從您的自訂登入頁面開始。

  ```swift
  class delegate : TokenResponseDelegate {
      public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
      //User authenticated
      }

      public func onAuthorizationFailure(error: AuthorizationError) {
      //Exception occurred
      }
  }

  AppID.sharedInstance.signinWithResourceOwnerPassword(username: username, password: password, delegate: delegate())
  ```
  {: codeblock}

</br>
</br>

## 使用 Node.js SDK 將應用程式加上品牌
{: #branded-ui-nodejs}

啟用「雲端目錄」之後，即可使用 Node.js SDK 來呼叫自訂的畫面。
{: shortdesc}

**登入**

藉由使用 WebAppStrategy，使用者可以利用其使用者名稱和密碼來登入您的 Web 應用程式。在使用者順利登入您的應用程式之後，只要 HTTP 階段作業保持作用中，他們的存取記號就會持續保存在 HTTP 階段作業中。HTTP 階段作業關閉或過期之後，也會破壞存取記號。


1. 在 GUI 中配置「雲端目錄」[設定](cloud-directory.html#cd-settings)。
2. 將下列程式碼放在您的應用程式中。當使用者嘗試登入時，會呼叫您的登入畫面，而且授權及鑑別處理程序會從您的自訂登入頁面開始。

  ```javascript
  app.post("/form/submit", bodyParser.urlencoded({extended: false}), passport.authenticate(WebAppStrategy.STRATEGY_NAME, {
  	successRedirect: LANDING_PAGE_URL,
  	failureRedirect: ROP_LOGIN_PAGE_URL,
  	failureFlash : true // allow flash messages
  }));
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="相關資訊圖示"/> 指令參數</th>
    </thead>
    <tbody>
      <tr>
        <td><code>successRedirect</code></td>
        <td>在成功鑑別之後，您要將使用者重新導向至其中的 URL。</td>
      </tr>
      <tr>
        <td><code>failureRedirect</code></td>
        <td>鑑別失敗時，您要將使用者重新導向至其中的 URL。</td>
      </tr>
      <tr>
        <td><code>failureFlash</code></td>
        <td>設為 <code>true</code> 時，會從「雲端目錄」服務傳回錯誤訊息。依預設，值會設為 <code>false</code>。</td>
      </tr>
    </tbody>
  </table>

**附註**：如果您在 HTML 中提交要求，則可以使用<a href="https://www.npmjs.com/package/body-parser" target="blank">主體剖析器 <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示"></a> 中介軟體。若要查看傳回的錯誤訊息，您可以使用 <a href="https://www.npmjs.com/package/connect-flash" target="blank">connect-flash <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。若要查看它是否運作中，請檢查 <a href="https://github.com/ibm-cloud-security/appid-serversdk-nodejs/blob/master/samples/web-app-sample.js" target="blank">Web 應用程式範例 <img src="../../icons/launch-glyph.svg" alt="外部鏈結圖示"></a>。

</br>
</br>

## 使用 API 將應用程式加上品牌
{: #branded-ui-api}

您可以顯示自己的自訂畫面，並充分運用 {{site.data.keyword.appid_short_notm}} 的鑑別及授權功能。使用「雲端目錄」作為身分提供者，使用者就可以在您提供較少協助的情況下，與您的應用程式互動。他們可以登入、註冊、重設其密碼及其他作業，而不需要要求協助。
{: shortdesc}

為了使此動作可行，{{site.data.keyword.appid_short_notm}} 會公開 REST API。您可以使用 REST API 來建置用於服務 Web 應用程式的後端伺服器，或使用您自己的自訂畫面與行動應用程式互動。

管理 API 是使用 IBM Cloud Identity and Access Management 產生的記號來保護。這表示帳戶擁有者可以指定其團隊成員對每一個服務實例的存取層次。如需 IAM 與 {{site.data.keyword.appid_short_notm}} 如何合作的相關資訊，請參閱[服務存取管理](/docs/services/appid/iam.html)。

在您配置[設定](/docs/services/appid/cloud-directory.html)之後，即可呼叫下列端點來顯示每個畫面。

**註冊**

您可以使用 `/sign_up` 端點，容許使用者自行註冊您的應用程式。
在要求內文中提供下列資料：
  * 您的承租戶 ID。
  * 「雲端目錄」使用者資料。如需詳細資料，請參閱 [SCIM 完整使用者呈現](https://tools.ietf.org/html/rfc7643#section-8.2)。
    * `password` 屬性。
    * 在 `primary` 屬性設為 `true` 的電子郵件陣列中，您必須至少具有 1 個電子郵件位址。

根據您的[電子郵件配置](/docs/services/appid/cloud-directory.html)，使用者可能會收到一個驗證要求、一封在他們註冊您的應用程式時歡迎他們的電子郵件，或兩者。當使用者註冊您的應用程式時，會觸發這兩種類型的電子郵件。驗證電子郵件包含一個鏈結，使用者可以按一下此鏈結來確認其身分；這時會顯示一個畫面，感謝他們驗證或確認其驗證完成。  

若要呈現自己的後置驗證頁面，請執行下列動作：

1. 在 {site.data.keyword.appid_sort_notm}} 儀表板中導覽至「雲端目錄」身分提供者。
2. 按一下**電子郵件驗證**標籤。
3. 在**自訂驗證頁面 URL** 中，輸入登入頁面的 URL。

當提供此值時，{{site.data.keyword.appid_short_notm}} 會呼叫 URL 以及 `context` 查詢。當您呼叫 `/sign_up/confirmation_result` 端點，並傳遞收到的 `context` 參數時，結果會告知您的使用者是否已驗證其帳戶。如果已驗證，則您可以顯示自訂頁面。

</br>
**忘記密碼**

您可以使用 `/forgot_password` 端點，容許使用者萬一忘記其密碼時可以予以回復。

在要求內文中提供下列資料：
  * 您的承租戶 ID。
  * 「雲端目錄」使用者的電子郵件。

呼叫端點時，會將重設密碼電子郵件傳送給使用者。此電子郵件包含**重設**按鈕。在他們按下按鈕之後，{{site.data.keyword.appid_short_notm}} 即會顯示一個畫面，容許他們重設其密碼。

您可以呈現自己的後置重設密碼頁面：

1. 在 GUI 中配置「雲端目錄」[設定](cloud-directory.html#cd-settings)。**容許使用者從您的應用程式管理其帳戶**必須設為**開啟**。
2. 在服務儀表板的**重設密碼**標籤中，請確定**忘記密碼電子郵件**已設為**開啟**。
3. 在**自訂重設密碼頁面的 URL** 中輸入登入頁面的 URL  

當提供此值時，{{site.data.keyword.appid_short_notm}} 會呼叫 URL 以及 `context` 查詢。呼叫 `/forgot_password/confirmation_result` 時，會使用 `context` 參數來接收結果。如果結果成功，則您可以顯示自訂頁面。

將隨機字串新增至您的自訂重設密碼頁面，並在提交要求時將它傳遞給您的後端。讓您的處理程式驗證字串，並只在 `/change_password` 端點有效時，才會呼叫它。這樣做可以減少後端重設密碼端點的漏洞。
{: tip}

</br>
**變更密碼**

您可以採用兩種方式來使用 `/change_password` 端點。當使用者提交重設要求時，或當使用者登入您的應用程式，並想要更新其密碼時。

在重設要求之後，於要求內文中提供下列資料來更新其密碼：
  * 您的承租戶 ID。
  * 使用者的新密碼
  * 「雲端目錄」使用者 UUID。
  * 選用項目：從中執行密碼重設的 IP 位址。如果您選擇傳遞 IP 位址，則位置保留元 `%{passwordChangeInfo.ipAddress}` 可供變更密碼電子郵件範本使用。

取決於您的配置，當密碼變更時，{{site.data.keyword.appid_short_notm}} 可能會傳送電子郵件給使用者，讓他們知道發生變更。

</br>
若要容許使用者在登入您的應用程式時變更其密碼，請執行下列動作：

在要求內文中提供下列資料：
  * 您的承租戶 ID。
  * 使用者的新密碼
  * 「雲端目錄」使用者 UUID。

您的變更密碼頁面應該會提示使用者輸入其現行密碼及其新密碼。
{: tip}

您的後端會使用 ROP API 來驗證使用者的現行密碼，如果有效，則會使用新密碼來呼叫端點。取決於您的配置，當密碼變更時，{{site.data.keyword.appid_short_notm}} 可能會傳送電子郵件給使用者，讓他們知道發生變更。

</br>
**重新傳送**

當使用者因某種原因而未接收到電子郵件時，您可以使用 `/resend/{templateName}` 來重新傳送電子郵件。

在要求內文中提供下列資料：
  * 承租戶 ID。
  * 範本名稱
  * 「雲端目錄」使用者 UUID。

**變更詳細資料**

當使用者登入您的應用程式時，他們可以更新其部分資訊。您可以使用 `/Users/{userId}` 來取得及更新其資訊。

更新使用者詳細資料時，端點會以 [SCIM 格式](https://tools.ietf.org/html/rfc7643#section-8.2)取得要求內文中更新的使用者資料。請確定您只變更相關的詳細資料。

無法變更其電子郵件位址。
{: tip}
