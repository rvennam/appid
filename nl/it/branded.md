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

# Personalizzazione della tua applicazione
{: #branding}

Puoi visualizzare le tue schermate personalizzate, utilizzare i tuoi flussi di accesso e sfruttare le funzionalità di autenticazione e autorizzazione di {{site.data.keyword.appid_full}}. Utilizzando Cloud Directory come tuo provider di identità, i tuoi utenti potranno interagire con la tua applicazione con meno aiuto da parte tua. Sono in grado di accedere, registrarsi, modificare la propria password e altro ancora senza richiedere assistenza.
{: shortdesc}

**Perché dovrei voler visualizzare le mie schermate?**

Quando riutilizzi le tue IU esistenti, puoi creare un flusso di accesso coerente per la tua applicazione. Utilizzando le stesse immagini, gli stessi colori e personalizzazioni, è più probabile che i tuoi utenti riconoscano il tuo marchio, anche quando non interagiscono direttamente con la tua applicazione.

</br>

**Quale tipo di configurazione è necessaria per visualizzare le mie schermate?**

Per visualizzare le tue IU, devi utilizzare [Cloud Directory(/docs/services/appid/cloud-directory.html)] come tuo provider di identità. Esistono molti modi in cui può essere [configurato](cloud-directory.html) Cloud Directory. Puoi decidere i tipi di messaggi che vuoi inviare e personalizzarne il contenuto e il design. Non sai cosa dire? Non è un problema. Sono presenti dei messaggi di esempio nella GUI che puoi utilizzare.

Vuoi utilizzare una [lingua](cloud-directory.html#languages) diversa dall'inglese? Puoi scegliere un'altra lingua utilizzando le <a href="https://appid-management.ng.bluemix.net/swagger-ui/#!/Config/updateLocalization" target="_blank">API di gestione della lingua <img src="../../icons/launch-glyph.svg" alt="Icona link esterno"></a>, per visualizzare il tuo contenuto tradotto.
{: tip}

</br>



**In che modo i flussi sono tecnicamente diversi?**

Il servizio utilizza i flussi di concessione OAuth2 per associare il processo di autorizzazione. Quando configuri i provider di identità social come Facebook, viene utilizzato il <a href="https://oauthlib.readthedocs.io/en/stable/oauth2/grants/authcode.html" target="_blank">flusso Authorization Grant <img src="../../icons/launch-glyph.svg" alt="Icona link esterno"></a> per richiamare il Widget di accesso. Quando utilizzi le tue schermate, viene utilizzato il <a href="https://oauthlib.readthedocs.io/en/stable/oauth2/grants/password.html" target="_blank">flusso Resource Owner Password Credentials <img src="../../icons/launch-glyph.svg" alt="Icona link esterno"></a> per fornire i token di accesso e identità che ti consentono di richiamare le tue schermate di accesso.

</br>

**Avete una qualsiasi applicazione di esempio che ne mostra il funzionamento?**

Sì. Consulta uno qualsiasi dei seguenti esempi per vedere come funziona Cloud Directory:

* <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-branded-ui-user-sign-app-id/" target="_blank">Use your own branded UI for user sign-in with {{site.data.keyword.appid_short_notm}} <img src="../../icons/launch-glyph.svg" alt="Icona link esterno"></a>
* <a href="https://www.ibm.com/blogs/bluemix/2018/06/use-ui-flows-user-sign-sign-app-id/" target="_blank">Use your own UI and Flows for User Sign-Up and Sign-in with with {{site.data.keyword.appid_short_notm}} <img src="../../icons/launch-glyph.svg" alt="Icona link esterno"></a>
* <a href="https://www.ibm.com/blogs/bluemix/2018/06/custom-login-page-app-id-integration/" target="_blank">Use a custom login page with  {{site.data.keyword.appid_short_notm}} <img src="../../icons/launch-glyph.svg" alt="Icona link esterno"></a>

</br>
</br>

## Personalizzazione della tua applicazione con l'SDK Android
{: #branded-ui-android}

Con Cloud Directory abilitato, puoi richiamare le schermate personalizzate con l'SDK Android. Puoi scegliere la combinazione delle schermate con cui desideri che i tuoi utenti possano interagire. <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-branded-ui-user-sign-app-id/" target="blank">Consulta questo blog<img src="../../icons/launch-glyph.svg" alt="Icona link esterno"></a> per un esempio dettagliato.
{: shortdesc}

</br>

**Accedi**

1. Configura le tue [impostazioni](cloud-directory.html#cd-settings) di Cloud Directory nella GUI.
2. Aggiungi il seguente codice alla tua applicazione. Il flusso di accesso viene attivato quando un utente fa clic su Accedi sulla tua schermata personalizzata. Ottieni i token di accesso, identità e aggiornamento fornendo la password e il nome utente dell'utente finale. 

  ```java
  AppID.getInstance().signinWithResourceOwnerPassword(getApplicationContext(), username, password,
         new TokenResponseListener() {
      @Override
        public void onAuthorizationFailure (AuthorizationException exception) {
          //Si è verificata un'eccezione
      }

      @Override
          public void onAuthorizationSuccess (AccessToken accessToken, IdentityToken identityToken, RefreshToken refreshToken) {
          //Utente autenticato
        }
  });
  ```
  {: pre}

</br>
</br>

## Personalizzazione della tua applicazione con l'SDK Swift iOS
{: #branded-ui-ios-swift}

Con Cloud Directory abilitato, puoi richiamare le schermate personalizzate con l'[SDK iOS Swift](https://github.com/ibm-cloud-security/appid-clientsdk-swift).
{: shortdesc}

</br>

**Accedi**

1. Configura le tue [impostazioni](cloud-directory.html#cd-settings) di Cloud Directory nella GUI.
2. Inserisci il seguente codice nella tua applicazione. Quando un utente tenta di accedere, viene richiamata la tua schermata di accesso e il processo di autorizzazione e autenticazione presenta la tua pagina di accesso personalizzata.

  ```swift
  class delegate : TokenResponseDelegate {
      public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
      //Utente autenticato
      }

      public func onAuthorizationFailure(error: AuthorizationError) {
      //Si è verificata un'eccezione
      }
  }

  AppID.sharedInstance.signinWithResourceOwnerPassword(username: username, password: password, delegate: delegate())
  ```
  {: codeblock}

</br>
</br>

## Personalizzazione della tua applicazione con l'SDK Node.js
{: #branded-ui-nodejs}

Con Cloud Directory abilitato, puoi richiamare le schermate personalizzate con l'SDK Node.js.
{: shortdesc}

**Accedi**

Utilizzando WebAppStrategy, gli utenti possono accedere alle tue applicazioni web con i propri nome utente e password. Dopo che un utente ha eseguito correttamente l'accesso alla tua applicazione, il loro token di accesso viene memorizzato in una sessione HTTP finché resta attiva. Dopo che la sessione HTTP è stata chiusa o è scaduta, viene distrutto anche il token di accesso.


1. Configura le tue [impostazioni](cloud-directory.html#cd-settings) di Cloud Directory nella GUI.
2. Inserisci il seguente codice nella tua applicazione. Quando un utente tenta di accedere, viene richiamata la tua schermata di accesso e il processo di autorizzazione e autenticazione presenta la tua pagina di accesso personalizzata.

  ```javascript
  app.post("/form/submit", bodyParser.urlencoded({extended: false}), passport.authenticate(WebAppStrategy.STRATEGY_NAME, {
  	successRedirect: LANDING_PAGE_URL,
    	failureRedirect: ROP_LOGIN_PAGE_URL,
    	failureFlash : true // consenti messaggi flash
    }));
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Icona ulteriori informazioni"/> Parametri comando </th>
    </thead>
    <tbody>
      <tr>
        <td><code>successRedirect</code></td>
        <td>L'URL a cui vuoi reindirizzare l'utente dopo un'autenticazione riuscita.</td>
      </tr>
      <tr>
        <td><code>failureRedirect</code></td>
        <td>L'URL a cui vuoi reindirizzare l'utente se l'autenticazione ha esito negativo.</td>
      </tr>
      <tr>
        <td><code>failureFlash</code></td>
        <td>Se impostato su <code>true</code>, viene restituito un messaggio di errore dal servizio Cloud Directory. Per impostazione predefinita, il valore è impostato su <code>false</code>.</td>
      </tr>
    </tbody>
  </table>

**Nota**: se invii la richiesta in HTML, puoi utilizzare il middleware <a href="https://www.npmjs.com/package/body-parser" target="blank">body parser <img src="../../icons/launch-glyph.svg" alt="Icona link esterno"></a>. Per visualizzare il messaggio di errore restituito puoi utilizzare <a href="https://www.npmjs.com/package/connect-flash" target="blank">connect-flash <img src="../../icons/launch-glyph.svg" alt="Icona link esterno"></a>. Per vederlo in azione, consulta l'<a href="https://github.com/ibm-cloud-security/appid-serversdk-nodejs/blob/master/samples/web-app-sample.js" target="blank">esempio di applicazione web <img src="../../icons/launch-glyph.svg" alt="Icona link esterno"></a>.

</br>
</br>

## Personalizzazione della tua applicazione con l'API
{: #branded-ui-api}

Puoi visualizzare le tue proprie schermate personalizzate e sfruttare le funzionalità di autenticazione e autorizzazione di {{site.data.keyword.appid_short_notm}}. Con Cloud Directory come tuo provider di identità, i tuoi utenti potranno interagire con la tua applicazione con meno aiuto da parte tua. Sono in grado di accedere, registrarsi, reimpostare la loro password e altro ancora senza richiedere assistenza.
{: shortdesc}

Per renderlo possibile, {{site.data.keyword.appid_short_notm}} espone le API REST. Puoi utilizzare le API REST per creare un server di back-end che funzioni per le tue applicazioni web o per interagire con un'applicazione mobile con le tue schermate personalizzate.

L'API di gestione è protetta con i token generati da IBM Cloud Identity and Access Management. Ciò significa che i proprietari degli account possono specificare chi ha nel proprio team un determinato livello di accesso per ciascuna istanza del servizio. Per ulteriori informazioni su come funzionano insieme IAM e {{site.data.keyword.appid_short_notm}}, vedi [Gestione dell'accesso al servizio](/docs/services/appid/iam.html).

Dopo aver configurato le tue [impostazioni](/docs/services/appid/cloud-directory.html), puoi richiamare i seguenti endpoint per visualizzare ogni schermata.

**Registrati**

Puoi utilizzare l'endpoint `/sign_up` per consentire agli utenti di registrarsi alla tua applicazione.
Fornisci i seguenti dati nel corpo della richiesta:
  * Il tuo ID tenant.
  * I dati utente Cloud Directory. Vedi [SCIM Full User Representation](https://tools.ietf.org/html/rfc7643#section-8.2) per ulteriori dettagli.
    * Un attributo `password`.
    * Nell'array di email con un attributo `primary` impostato su `true`, devi avere almeno 1 indirizzo email.

A seconda della tua [configurazione email](/docs/services/appid/cloud-directory.html), un utente potrebbe ricevere una richiesta di verifica, un'email di benvenuto quando si registra alla tua applicazione o entrambe. Entrambi i tipi di email vengono attivati quando un utente si registra alla tua applicazione. L'email di verifica contiene un link su cui può fare clic l'utente per confermare la sua identità; viene visualizzata una schermata che ringrazia della verifica o che conferma che la verifica è stata completata.  

Per presentare la tua pagina di post-verifica:

1. Passa al provider di identità Cloud Directory nel dashboard {site.data.keyword.appid_short_notm}}.
2. Fai clic sulla scheda **Email verification**.
3. In **custom verification page URL** immetti l'URL per la tua pagina di destinazione.

Quando viene fornito questo valore, {{site.data.keyword.appid_short_notm}} richiama l'URL insieme a una query `context`. Quando richiami l'endpoint `/sign_up/confirmation_result` e passi il parametro `context` ricevuto, il risultato indica se il tuo utente ha verificato il suo account. Se lo ha fatto, puoi visualizzare la tua pagina personalizzata.

</br>
**Password dimenticata**

Puoi utilizzare l'endpoint `/forgot_password` per consentire agli utenti di recuperare la propria password nel caso in cui la dimentichino.

Fornisci i seguenti dati nel corpo della richiesta:
  * Il tuo ID tenant.
  * L'email dell'utente Cloud Directory.

Quando l'endpoint viene richiamato, all'utente viene inviata un'email di reimpostazione della password. L'email contiene un pulsante **Reset**. Dopo aver premuto il pulsante, {{site.data.keyword.appid_short_notm}} visualizza una schermata che gli permette di reimpostare la propria password.

Puoi presentare la tua propria pagina di post-reimpostazione della password:

1. Configura le tue [impostazioni](cloud-directory.html#cd-settings) di Cloud Directory nella GUI. **Allow users to manage their account from your app** deve essere impostato su **On**.
2. Nella scheda **Reset Password** del dashboard del servizio, assicurati che **Forgot password email** sia impostato su **On**.
3. Immetti l'URL per la tua pagina di destinazione in **URL for your custom reset password page**  

Quando viene fornito questo valore, {{site.data.keyword.appid_short_notm}} richiama l'URL insieme a una query `context`. Il parametro `context` è utilizzato per ricevere il risultato quando viene richiamato `/forgot_password/confirmation_result`. Se il risultato ha esito positivo, puoi visualizzare la tua pagina personalizzata.

Aggiungi una stringa casuale alla pagina di reimpostazione della password personalizzata e passala al tuo back-end quando la richiesta viene inviata. Chiedi al tuo gestore di convalidare la stringa e chiama l'endpoint `/change_password` solo se è valida. In questo modo, puoi ridurre la vulnerabilità del tuo endpoint di reimpostazione della password di back-end.
{: tip}

</br>
**Modifica della password**

Puoi utilizzare l'endpoint `/change_password` in due modi. Quando un utente invia una richiesta di reimpostazione o quando un utente ha effettuato l'accesso alla tua applicazione e vuole aggiornare la propria password.

Fornisci i seguenti dati nel corpo della richiesta per aggiornare la password dopo una richiesta di reimpostazione:
  * Il tuo ID tenant.
  * La nuova password dell'utente.
  * L'UUID utente Cloud Directory.
  * Facoltativo: l'indirizzo IP da cui è stata eseguita la reimpostazione della password. Se scegli di passare l'indirizzo IP, è disponibile il segnaposto `%{passwordChangeInfo.ipAddress}` per il template di email di modifica della password.

A seconda della tua configurazione, quando una password viene modificata {{site.data.keyword.appid_short_notm}} potrebbe inviare un'email all'utente facendogli sapere che c'è stata una modifica.

</br>
Per consentire agli utenti di modificare la loro password mentre sono connessi alla tua applicazione:

Fornisci i seguenti dati nel corpo della richiesta:
  * Il tuo ID tenant.
  * La nuova password dell'utente.
  * L'UUID utente Cloud Directory.

La tua pagina di modifica della password dovrebbe richiedere all'utente di immettere la password corrente e la nuova password.
{: tip}

Il tuo back-end convalida la password corrente dell'utente con l'API ROP e, se valida, richiama l'endpoint con la nuova password. A seconda della tua configurazione, quando una password viene modificata {{site.data.keyword.appid_short_notm}} potrebbe inviare un'email all'utente facendogli sapere che c'è stata una modifica.

</br>
**Invia nuovamente**

Puoi utilizzare `/resend/{templateName}` per inviare nuovamente un'email quando un utente non la riceve per qualche motivo.

Fornisci i seguenti dati nel corpo della richiesta:
  * L'ID tenant.
  * Il nome del template.
  * L'UUID utente Cloud Directory.

**Modifica dettagli**

Quando un utente accede alla tua applicazione, può aggiornare alcune delle sue informazioni. Puoi utilizzare `/Users/{userId}` per ottenere e aggiornare le sue informazioni.

Quando i dettagli dell'utente vengono aggiornati, l'endpoint riceve i dati utente aggiornati nel corpo della richiesta in [formato SCIM](https://tools.ietf.org/html/rfc7643#section-8.2). Assicurati di modificare solo i dettagli pertinenti.

L'indirizzo email non può essere modificato.
{: tip}
