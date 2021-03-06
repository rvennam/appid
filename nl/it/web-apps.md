---

copyright:
  years: 2017, 2018
lastupdated: "2018-12-19"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:pre: .pre}
{:tip: .tip}
{:screen: .screen}
{:codeblock: .codeblock}


# Applicazioni web
{: #adding-web}

Con {{site.data.keyword.appid_full}}, puoi velocemente costruire un livello di autenticazione per le tue applicazioni web.
{: shortdesc}

## Descrizione del flusso
{: #understanding}

**Quando questo flusso sarebbe utile?**

Quando stai sviluppando un'applicazione web, puoi utilizzare il flusso web {{site.data.keyword.appid_short}} per autenticare in modo sicuro gli utenti. Gli utenti sono quindi in grado di accedere al contenuto protetto lato server nelle tue applicazioni web.

**Quali sono le basi tecniche del flusso?**

Le applicazioni web spesso richiedono agli utenti di autenticarsi per accedere al contenuto protetto. {{site.data.keyword.appid_short_notm}} utilizza il flusso di codice dell'autorizzazione OIDC per autenticare in modo sicuro gli utenti. Con questo flusso, dopo che l'utente è stato autenticato, l'applicazione riceve un codice di autorizzazione. Il codice viene quindi scambiato con un token di accesso, identità e aggiornamento. Nel codice, le istruzioni per lo scambio dei token vengono sempre inviate tramite un backchannel sicuro tra l'applicazione e il server OIDC. Questo fornisce un ulteriore livello di sicurezza poiché l'aggressore non è in grado di intercettare i token. Questi token possono essere inviati direttamente all'applicazione che contiene il server web per l'autenticazione dell'utente.

**Come funziona questo flusso?**

![{{site.data.keyword.appid_short_notm}} flusso richiesta](images/web-flow.png)

1. Un utente avvia il flusso di autorizzazione inviando una richiesta all'endpoint `/authorization` tramite l'API o l'SDK {{site.data.keyword.appid_short_notm}}.

2. Se l'utente non è autorizzato, il flusso di autenticazione viene avviato con un reindirizzamento a {{site.data.keyword.appid_short_notm}}.

3. A seconda dei parametri della richiesta `/authorization` o della configurazione del provider di identità dell'utente, viene avviato il widget di accesso nel browser degli utenti.

4. L'utente sceglie un provider di identità per l'autenticazione e completa il processo di accesso.

5. Il provider di identità esegue il reindirizzamento all'applicazione client con il codice di autorizzazione.

6. L'SDK {{site.data.keyword.appid_short_notm}} scambia il codice di autorizzazione con i token di accesso, identità e aggiornamento facoltativo dal servizio {{site.data.keyword.appid_short_notm}}.

7. I token vengono salvati dall'SDK {{site.data.keyword.appid_short_notm}} e si verifica un reindirizzamento all'applicazione client.

8. All'utente viene concesso l'accesso all'applicazione.

</br>
</br>

## Configurazione dell'SDK Node.js
{: #configuring-nodejs}

Puoi configurare {{site.data.keyword.appid_short_notm}} per utilizzare le tue applicazioni web Node.js.
{: shortdesc}

**Prima di cominciare**

Devi avere i seguenti prerequisiti:

* Un'istanza del servizio {{site.data.keyword.appid_short_notm}}
* Una serie di credenziali del servizio
* NPM versione 4 o superiore
* Node versione 6 o superiore
* Il tuo URI di reindirizzamento impostato nel dashboard del servizio {{site.data.keyword.appid_short_notm}} 


### Installazione dell'SDK Node.js

1. Utilizzando la riga di comando, passa alla directory che contiene la tua applicazione Node.js.

2. Installa il servizio {{site.data.keyword.appid_short_notm}}.
  ```bash
  npm install --save ibmcloud-appid
  ```
  {: codeblock}

### Inizializzazione dell'SDK Node.js

1. Aggiungi le seguenti definizioni `require` al tuo file `server.js`.

    ```javascript
    const express = require('express');
    const session = require('express-session')
    const passport = require('passport');
    const WebAppStrategy = require("ibmcloud-appid").WebAppStrategy;
    const CALLBACK_URL = "/ibm/cloud/appid/callback";
    ```
    {: codeblock}

2. Configurare la tua applicazione Express per utilizzare il middleware della sessione Express. **Nota**: devi configurare il middleware con l'archivio della sessione appropriato per gli ambienti di produzione. Per ulteriori informazioni consulta <a href="https://github.com/expressjs/session" target="_blank">expressjs docs <img src="../icons/launch-glyph.svg" alt="Icona link esterno"></a>.

    ```javascript
    const app = express();
    app.use(session({
        secret: "123456",
        resave: true,
        saveUninitialized: true
        }));
    app.use(passport.initialize());
    app.use(passport.session());
    ```
    {: codeblock}

3. Passa le tue credenziali del servizio per inizializzare l'SDK.

  ```javascript
    passport.use(new WebAppStrategy({
    	  tenantId: "{tenant-id}",
   	    clientId: "{client-id}",
      	secret: "{secret}",
      	oauthServerUrl: "{oauth-server-url}",
      	redirectUri: "{app-url}" + CALLBACK_URL
      }));
  ```
  {: codeblock}

   <table summary="Componenti comando: applicazioni Node.js">
      <caption>Componenti del comando per le applicazioni Node.js</caption>
        <tr>
          <th>Componenti</th>
          <th>Descrizione</th>
        </tr>
      <tr>
          <td><i>tenantId</i> </br> <i>clientId</i> </br> <i> secret </i> </br> <i> oauth-server-url </i> </br> </td>
          <td>Puoi trovare questi valori facendo clic su **View Credentials** nella scheda **Service Credentials** del tuo dashboard del servizio.</td>
      </tr>
      <tr>
        <td><i>redirectUri</i></td>
        <td>Il valore dell'URI di reindirizzamento può essere fornito in tre modi:</br>
            1. Manualmente nel nuovo `WebAppStrategy({redirectUri: "...."})`</br>
            2. Come variabile di ambiente denominata `redirectUri`</br>
            3. Se non viene fornita alcuna di queste opzioni, l'SDK {{site.data.keyword.appid_short_notm}} tenta di richiamare l'`application_uri` dell'applicazione in esecuzione su {{site.data.keyword.Bluemix_notm}} e accoda un suffisso predefinito `/ibm/cloud/appid/callback`.
        </td>
      </tr>
    </table>

4. Configura passport con la serializzazione e la deserializzazione. Questo passo di configurazione è obbligatorio per la persistenza della sessione autenticata attraverso le richieste HTTP. Per ulteriori informazioni, visita il <a href="http://passportjs.org/docs" target="_blank">passport docs <img src="../icons/launch-glyph.svg" alt="icona link esterno"></a>.

  ```javascript
  passport.serializeUser(function(user, cb) {
    cb(null, user);
    });

  passport.deserializeUser(function(obj, cb) {
    cb(null, obj);
    });
  ```
  {: codeblock}

5. Aggiungi il seguente codice al tuo file `server.js` per emettere i reindirizzamenti del servizio.

   ```javascript
   app.get(CALLBACK_URL, passport.authenticate(WebAppStrategy.STRATEGY_NAME));
   ```
   {: codeblock}

6. Registra il tuo endpoint protetto.

   ```javascript
   app.get(‘/protected’, passport.authenticate(WebAppStrategy.STRATEGY_NAME)), function(req, res) res.json(req.user); });
   ```
   {: codeblock}

Per ulteriori informazioni, visita il <a href="https://github.com/ibm-cloud-security/appid-serversdk-nodejs" target="_blank">{{site.data.keyword.appid_short_notm}} Node.js GitHub repository <img src="../icons/launch-glyph.svg" alt="icona link esterno"></a>.

</br>
</br>

## Configurazione dell'SDK Liberty for Java
{: #configuring-liberty}

Puoi configurare {{site.data.keyword.appid_short_notm}} per utilizzare le tue applicazioni web Liberty for Java.
{:shortdesc}

**Prima di cominciare**

Devi avere i seguenti prerequisiti:
* Un'istanza del servizio {{site.data.keyword.appid_short_notm}}
* Una serie di credenziali del servizio
* Apache Maven 3.5 o superiore
* Java 1.8
* Un'applicazione web Liberty for Java

### Installazione dell'SDK Liberty for Java

1. Aggiungi una funzione OpenID Connect al tuo `server.xml`.

  ```xml
  <featureManager>
      <feature>ssl-1.0</feature>
      <feature>appSecurity-2.0</feature>
      <feature>openidConnectClient-1.0</feature>
  </featureManager>
  ```
  {: codeblock}

2. Crea una funzione Open ID Connect Client e definisci i seguenti segnaposto. Utilizza le credenziali del servizio per compilare i segnaposto.

  ```xml
  <openidConnectClient
    clientId='App ID client_ID'
    clientSecret='App ID Secret'
    authorizationEndpointUrl='oauthServerUrl/authorization'
    tokenEndpointUrl='oauthServerUrl/token'
    jwkEndpointUrl='oauthServerUrl/publickeys'
    issuerIdentifier='Changed according to the region'
    tokenEndpointAuthMethod="basic"
    signatureAlgorithm="RS256"
    authFilterid="myAuthFilter"
    trustAliasName="my.bluemix.certificate"
  />
  ```
  {: codeblock}

  <table>
  <caption>Tabella. Variabili dell'elemento OIDC per le applicazioni Liberty for Java</caption>
    <tr>
      <th> Componente </th>
      <th> Descrizione </th>
    </tr>
    <tr>
    <td><code>clientID</code> </br> <code>secret</code> </br> <code>oauth-server-url</code> </br></td>
    <td>Puoi trovare questi valori facendo clic su **Visualizza credenziali** nella scheda **credenziali del servizio** del tuo dashboard del servizio.</td>
    </tr>
    <tr>
      <td><code>authorizationEndpointURL</code></td>
      <td> Aggiungi `/authorization` alla file di oauthServerURL.</td>
    </tr>
    <tr>
      <td><code>tokenEndpointUrl</code></td>
      <td>Aggiungi `/token` alla file di oauthServerURL.</td>
    </tr>
    <tr>
      <td><code>jwkEndpointUrl</code></td>
      <td>Aggiungi `/publickeys` alla file di oauthServerURL.</td>
    </tr>
    <tr>
      <td><code>issuerIdentifier</code></td>
      <td>Diversa a seconda della regione. Può essere uno dei seguenti valori: </br><ul><li>issuerIdentifier="appid-oauth.ng.bluemix.net" </br><li> issuerIdentifier="appid-oauth.eu-gb.bluemix.net" </br><li>issuerIdentifier="appid-oauth.au-syd.bluemix.net"</ul></td>
    </tr>
    <tr>
      <td><code>tokenEndpointAuthMethod</code></td>
      <td>Specificata come "basic".</td>
    </tr>
    <tr>
      <td><code>signatureAlgorithm</code></td>
      <td>Specificata come "RS256".</td>
    </tr>
    <tr>
      <td><code> authFilterid </code></td>
      <td>L'elenco delle risorse da proteggere.</td>
    </tr>
    <tr>
      <td><code> trustAliasName </code></td>
      <td>Il nome del tuo certificato nel tuo truststore.</td>
    </tr>
  </table>

### Inizializzazione dell'SDK Liberty for Java

1. Nel tuo file `server.xml`, definisci un filtro di autorizzazione per specificare le risorse protette. Se non viene <a href="https://www.ibm.com/support/knowledgecenter/en/SSD28V_8.5.5/com.ibm.websphere.wlp.core.doc/ae/rwlp_auth_filter.html" target="_blank">definito un filtro <img src="../../icons/launch-glyph.svg" alt="Icona link esterno"></a>, il servizio protegge tutte le risorse.

  ```xml
  <authFilter id="myAuthFilter">
             <requestUrl id="myRequestUrl" urlPattern="/protected" matchType="contains"/>
    </authFilter>
  ```
  {: codeblock}

2. Definisci il tuo tipo di oggetto speciale come `ALL_AUTHENTICATED_USERS`.

  ```xml
  <application type="war" id="ProtectedServlet" context-root="/appidSample"
  location="${server.config.dir}/apps/libertySample-1.0.0.war">
    <application-bnd>
        <security-role name="myrole">
        <special-subject type="ALL_AUTHENTICATED_USERS"/>
        </security-role>
            </application-bnd>
        </application>
  ```
  {: codeblock}

3. Scarica il file `libertySample-1.0.0.war` da <a href="https://github.com/ibm-cloud-security/appid-sample-code-snippets/tree/master/liberty-for-java" target="_blank">GitHub <img src="../../icons/launch-glyph.svg" alt="Icona link esterno"></a> e posizionalo nella cartella apps del tuo server. Ad esempio, se il tuo server è denominato defaultServer, il file war dovrebbe essere posizionato qui `target/liberty/wlp/usr/servers/defaultServer/apps/`.

4. Configura SSL aggiungendo quanto segue al tuo file `server.xml`. Dovrai anche creare un truststore.

```xml
  <keyStore id="defaultKeyStore" password="myPassword"/>
  <keyStore id="appidtruststore" password="Liberty" location="${server.config.dir}/mytruststore.jks"/>
  <ssl id="defaultSSLConfig" keyStoreRef="defaultKeyStore" trustStoreRef="appidtruststore"/>
```
{: codeblock}

Per impostazione predefinita, la configurazione SSL richiede che il truststore sia configurato per OpenID Connect. Trova ulteriori informazioni in <a href="https://www.ibm.com/support/knowledgecenter/en/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/twlp_config_oidc_rp.html" target="_blank">configuring an OpenID Connect Client in Liberty <img src="../../icons/launch-glyph.svg" alt="Icona link esterno"></a>
{: tip}

</br>
</br>

## Configurazione dell'SDK Spring Boot for Java
{: #configuring-spring-boot}

Puoi configurare {{site.data.keyword.appid_short_notm}} per utilizzare le tue applicazioni Spring Boot.
{:shortdesc}

**Prima di cominciare**

Devi avere i seguenti prerequisiti:

* Un'istanza del servizio {{site.data.keyword.appid_short_notm}}
* Una serie di credenziali del servizio
* Un progetto Java + Maven
* Apache Maven 3.5 o superiore
* Java 1.8
* Spring Boot 2.0 e Security OAuth 2.0 o superiore


### Inizializzazione del framework Spring Boot

1. Aggiungi quanto segue tra le tag `<project> </project>` nel tuo file `pom.xml` Maven.

  ```xml
  <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-parent</artifactId>
      <version>2.0.2.RELEASE</version>
      <relativePath/>
  </parent>
  ```
  {: codeblock}

2. Aggiungi le seguenti dipendenze al tuo file `pom.xml` Maven.

  ```xml
  <dependencies>
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-web</artifactId>
      </dependency>
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-security</artifactId>
      </dependency>
      <dependency>
          <groupId>org.springframework.security.oauth.boot</groupId>
          <artifactId>spring-security-oauth2-autoconfigure</artifactId>
          <version>2.0.0.RELEASE</version>
      </dependency>
  </dependencies>
  ```
  {: codeblock}

3. Nello stesso file, includi il plugin Maven.

  ```xml
  <plugin>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-maven-plugin</artifactId>
  </plugin>
  ```
  {: codeblock}

### Inizializzazione di OAuth2

1. Aggiungi le seguenti annotazioni al tuo file Java.

  ```java
  @SpringBootApplication
  @EnableOAuth2Sso
  ```
  {: codeblock}

2. Estendi la classe con `WebSecurityConfigurerAdapter`.
3. Sovrascrivi tutta la configurazione di sicurezza e registra il tuo endpoint protetto.

  ```java
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                .antMatchers("/protectedResource").authenticated()
                .and().logout().logoutSuccessUrl("/").permitAll();
    }
  ```
  {: codeblock}


### Aggiunta di credenziali

1. Aggiungi un file di configurazione `application.yml` alla directory `/springbootsample/src/main/resources/`. Puoi completare la tua configurazione con le informazioni dalle tue credenziali del servizio.

  ```
  security:
  oauth2:
    client:
      clientId: {client ID}
      clientSecret: {client Secret}
      accessTokenUri: {oauthServerUrl}/token
      userAuthorizationUri: {oauthServerUrl}/authorization
    resource:
      userInfoUri: {oauthServerUrl}/userinfo
  ```
  {: codeblock}


Per un esempio dettagliato, consulta <a href="https://www.ibm.com/blogs/bluemix/2018/06/creating-spring-boot-applications-app-id/" target="_blank">questo blog</a>!

</br>
</br>

## Utilizzo di {{site.data.keyword.appid_short_notm}} con altri linguaggi
{: #other}

Con un SDK client conforme a OIDC, puoi utilizzare {{site.data.keyword.appid_short_notm}} con altri linguaggi. Per ulteriori informazioni, consulta un elenco di <a href="https://openid.net/developers/certified/">librerie certificate</a>.


</br>
</br>

## Fasi successive
{: #next}

Con {{site.data.keyword.appid_short_notm}} installato nella tua applicazione, sei quasi pronto ad iniziare ad autenticare gli utenti. Prova ad eseguire una delle seguenti attività:

* Configura i tuoi [provider di identità](/docs/services/appid/identity-providers.html)
* Personalizza e configura [il widget di accesso](/docs/services/appid/login-widget.html)
* Informazioni sull'<a href="https://github.com/ibm-cloud-security/appid-serversdk-nodejs" target="_blank">SDK Node.js<img src="../../icons/launch-glyph.svg" alt="Icona link esterno"></a>
