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

# Gestión de marca en la app
{: #branding}

Puede mostrar sus propias pantallas personalizadas, utilizar sus propios flujos de inicio de sesión y aprovechar las posibilidades de autenticación y autorización de {{site.data.keyword.appid_full}}. Utilizando el Directorio en la nube como proveedor de identidad, sus usuarios pueden interactuar con su app requiriendo menos ayuda. Son capaces de iniciar la sesión, registrarse, modificar su contraseña, etc., sin necesidad de solicitar ayuda.
{: shortdesc}

**¿Por qué debería mostrar mis propias pantallas?**

Cuando reutiliza las IU existentes, puede crear un flujo de inicio de sesión coherente para la app. Utilizando las mismas imágenes, colores y marcas, es más probable que los usuarios reconozcan su marca, incluso cuando no interactúen directamente con la app.

</br>

**¿Qué tipo de configuración se necesita para mostrar mis propias pantallas?**

Para mostrar sus propias IU, debe utilizar [Directorio en la nube(/docs/services/appid/cloud-directory.html)] como proveedor de identidad. Hay varias formas de [configurar](cloud-directory.html) el directorio en la nube. Puede decidir los tipos de mensajes que desea enviar y personalizar el contenido y diseño. ¿No sabe qué decir? Ningún problema. Puede utilizar los mensajes de muestra de la GUI.

¿Desea utilizar un [idioma](cloud-directory.html#languages) que no sea el inglés? Puede eligir otro idioma utilizando las <a href="https://appid-management.ng.bluemix.net/swagger-ui/#!/Config/updateLocalization" target="_blank">API de gestión de idiomas <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> para visualizar su propio contenido traducido.
{: tip}

</br>



**¿En qué se diferencian técnicamente los flujos?**

El servicio utiliza flujos de otorgamiento de OAuth2 para asignar el proceso de autorización. Cuando configura proveedores de identidad sociales como Facebook, se utiliza el <a href="https://oauthlib.readthedocs.io/en/stable/oauth2/grants/authcode.html" target="_blank">Flujo de otorgamiento de autorización <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> para llamar al widget de inicio de sesión. Cuando utilice sus propias pantallas, se utiliza el <a href="https://oauthlib.readthedocs.io/en/stable/oauth2/grants/password.html" target="_blank">flujo de ROPC (credenciales de contraseña de propietario de recurso)<img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> para proporcionar señales de identidad y acceso que le permitan llamar a las pantallas de inicio de sesión.

</br>

**¿Tiene alguna app de ejemplo que muestre cómo funciona?**

Sí. Compruebe cualquiera de los ejemplos siguientes para ver el directorio en la nube en acción:

* <a href="https://www.ibm.com/blogs/bluemix/2018/01/use-branded-ui-user-sign-app-id/" target="_blank">Utilice una IU de su propia marca para el inicio de sesión del usuario con {{site.data.keyword.appid_short_notm}} <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>
* <a href="https://www.ibm.com/blogs/bluemix/2018/06/use-ui-flows-user-sign-sign-app-id/" target="_blank">Utilice una IU y flujos de su propia marca para el registro e inicio de sesión con {{site.data.keyword.appid_short_notm}} <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>
* <a href="https://www.ibm.com/blogs/bluemix/2018/06/custom-login-page-app-id-integration/" target="_blank">Utilice una página de inicio de sesión personalizada con {{site.data.keyword.appid_short_notm}} <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>

</br>
</br>

## Gestión de marca en la app con el SDK de Android
{: #branded-ui-android}

Con el Directorio en la nube habilitado, puede llamar a pantallas personalizadas con el SDK de Android. Puede elegir la combinación de las pantallas con las que desea que sus usuarios puedan interactuar.<a href="https://www.ibm.com/blogs/bluemix/2018/01/use-branded-ui-user-sign-app-id/" target="blank">Consulte este blog<img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo"></a> para obtener un ejemplo detallado.
{: shortdesc}

</br>

**Iniciar la sesión**

1. Configure los [valores](cloud-directory.html#cd-settings) del directorio en la nube en la GUI.
2. Añada el código siguiente a la aplicación. El flujo de inicio de sesión se activa cuando un usuario pulsa en el inicio de sesión de la pantalla personalizada. Obtendrá las señales de renovación, identidad y acceso proporcionando el nombre de usuario y la contraseña del usuario final.

  ```java
  AppID.getInstance().signinWithResourceOwnerPassword(getApplicationContext(), username, password, new TokenResponseListener() {
      @Override
        public void onAuthorizationFailure (AuthorizationException exception) {
          //Se ha producido una excepción
        }

      @Override
          public void onAuthorizationSuccess (AccessToken accessToken, IdentityToken identityToken, RefreshToken refreshToken) {
          //Usuario autenticado
        }
  });
  ```
  {: pre}

</br>
</br>

## Gestión de marca en la app con el SDK de iOS Swift
{: #branded-ui-ios-swift}

Con el Directorio en la nube habilitado, puede llamar a las pantallas de su propia marca con el [SDK de iOS Swift](https://github.com/ibm-cloud-security/appid-clientsdk-swift).
{: shortdesc}

</br>

**Iniciar la sesión**

1. Configure los [valores](cloud-directory.html#cd-settings) del directorio en la nube en la GUI.
2. Añada el código siguiente en la aplicación. Cuando un usuario intenta iniciar sesión, se llama a la pantalla de inicio de sesión y el proceso de autorización y autenticación se inicia con la página de inicio de sesión personalizada.

  ```swift
  class delegate : TokenResponseDelegate {
      public func onAuthorizationSuccess(accessToken: AccessToken?, identityToken: IdentityToken?, response:Response?) {
      //Usuario autenticado
      }

      public func onAuthorizationFailure(error: AuthorizationError) {
      //Se ha producido una excepción
        }
  }

  AppID.sharedInstance.signinWithResourceOwnerPassword(username: username, password: password, delegate: delegate())
  ```
  {: codeblock}

</br>
</br>

## Gestión de marca en la app con el SDK de Node.js
{: #branded-ui-nodejs}

Con el Directorio en la nube habilitado, puede llamar a pantallas personalizadas con el SDK de Node.js.
{: shortdesc}

**Iniciar la sesión**

Mediante WebAppStrategy, los usuarios pueden iniciar sesión en sus apps web con su nombre de usuario y una contraseña. Una vez que un usuario inicia sesión correctamente en la app, su señal de acceso persiste en una sesión HTTP durante el tiempo que se mantiene activa. Una vez que la sesión HTTP se cierra o caduca, la señal de acceso también se destruye.


1. Configure los [valores](cloud-directory.html#cd-settings) del directorio en la nube en la GUI.
2. Añada el código siguiente en la aplicación. Cuando un usuario intenta iniciar sesión, se llama a la pantalla de inicio de sesión y el proceso de autorización y autenticación se inicia con la página de inicio de sesión personalizada.

  ```javascript
  app.post("/form/submit", bodyParser.urlencoded({extended: false}), passport.authenticate(WebAppStrategy.STRATEGY_NAME, {
  	successRedirect: LANDING_PAGE_URL,
  	failureRedirect: ROP_LOGIN_PAGE_URL,
  	failureFlash : true //permitir mensajes flash
  }));
  ```
  {: codeblock}

  <table>
    <thead>
      <th colspan=2><img src="images/idea.png" alt="Icono de más información"/> Parámetros de mandato </th>
    </thead>
    <tbody>
      <tr>
        <td><code> successRedirect </code></td>
        <td>El URL al que desea redirigir al usuario tras una autenticación correcta.</td>
      </tr>
      <tr>
        <td><code> failureRedirect </code></td>
        <td>El URL al que desea redirigir al usuario si la autenticación falla.</td>
      </tr>
      <tr>
        <td><code> failureFlash </code></td>
        <td>Si se establece en <code>true</code>, se devuelve un mensaje de error desde el servicio del Directorio en la nube. De manera predeterminada, el valor se establece en <code>false</code>.</td>
      </tr>
    </tbody>
  </table>

**Nota**: Si envía la solicitud en HTML, puede utilizar middleware <a href="https://www.npmjs.com/package/body-parser" target="blank">analizador de cuerpo <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>. Para ver el mensaje de error devuelto, puede utilizar <a href="https://www.npmjs.com/package/connect-flash" target="blank">connect-flash <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>. Para verlo en acción, consulte el <a href="https://github.com/ibm-cloud-security/appid-serversdk-nodejs/blob/master/samples/web-app-sample.js" target="blank">ejemplo de app web <img src="../../icons/launch-glyph.svg" alt="Icono de enlace externo"></a>.

</br>
</br>

## Gestión de marca en la app con la API
{: #branded-ui-api}

Puede mostrar sus propias pantallas personalizadas y aprovechar las posibilidades de autenticación y autorización de {{site.data.keyword.appid_short_notm}}. Con el Directorio en la nube como proveedor de identidad, sus usuarios pueden interactuar con su app requiriendo menos ayuda. Son capaces de iniciar la sesión, registrarse, restablecer su contraseña, etc., sin necesidad de solicitar ayuda.
{: shortdesc}

Para hacer esto posible, {{site.data.keyword.appid_short_notm}} expone API REST. Puede utilizar las API REST para crear un servidor de fondo que sirva a sus apps web, o para interactuar con una app móvil con sus propias pantallas personalizadas.

La API de gestión está protegida con las señales generadas de IBM Cloud Identity and Access Management. Esto significa que los propietarios de las cuentas pueden especificar qué persona de su equipo tiene qué nivel de acceso para cada instancia de servicio. Para obtener más información sobre cómo funcionan juntos IAM y {{site.data.keyword.appid_short_notm}}, consulte [Gestión de acceso de servicio](/docs/services/appid/iam.html).

Después de haber configurado los [valores](/docs/services/appid/cloud-directory.html), puede llamar a los siguientes puntos finales para visualizar cada pantalla.

**Registro**

Puede utilizar el punto final `/sign_up` para permitir que los usuarios se registren ellos mismos para su app.
Proporcione los datos siguientes en el cuerpo de solicitud:
  * Su tenantID.
  * Datos de usuario del directorio en la nube. Consulte [Representación de usuario completo de SCIM](https://tools.ietf.org/html/rfc7643#section-8.2) para obtener más detalles.
    * Un atributo `password`.
    * En la matriz de correo electrónico con un atributo `primary` establecido en `true`, debe tener al menos una dirección de correo electrónico.

Dependiendo de las [configuración de correo electrónico](/docs/services/appid/cloud-directory.html), un usuario podría recibir una solicitud para su verificación, o un correo electrónico que le da la bienvenida cuando se registra para su app o ambos. Ambos tipos de correos electrónicos se desencadenan cuando un usuario se registra para su app. El correo electrónico de verificación contiene un enlace en el que el usuario puede pulsar para confirmar su identidad; se muestra una pantalla que le agradece que realice una verificación o confirme que la misma se ha completado.  

Para presentar su propia página postverificación:

1. Vaya al proveedor de identidad del directorio en la nube en el panel de control de {site.data.keyword.appid_short_notm}}.
2. Pulse en el separador **Verificación de correo electrónico**.
3. En **Personalizar URL de página de verificación** introduzca el URL de su página de destino.

Cuando se proporcione este valor, {{site.data.keyword.appid_short_notm}} llama al URL junto con una consulta `context`. Cuando llama al punto final `/sign_up/confirmation_result` y pasa el parámetro `context` recibido, el resultado le indica si el usuario ha verificado su cuenta. Si lo ha hecho, puede mostrar su página personalizada.

</br>
**Contraseña olvidada**

Puede utilizar el punto final `/forgot_password` para permitir que los usuarios recuperen su contraseña si la olvidan.

Proporcione los datos siguientes en el cuerpo de solicitud:
  * Su tenantID.
  * El correo electrónico del usuario del directorio en la nube.

Cuando se llame al punto final, se enviará un correo electrónico de restablecimiento de contraseña al usuario. El correo electrónico contiene un botón **Restablecer**. Una vez que pulsen el botón, se mostrará una pantalla mediante {{site.data.keyword.appid_short_notm}} que les permite restablecer su contraseña.

Puede presentar su propia página de restablecimiento de contraseña:

1. Configure los [valores](cloud-directory.html#cd-settings) del directorio en la nube en la GUI. **Permitir a los usuarios gestionar la cuenta desde la app** se debe establecer en **Activado**.
2. En el separador **Restablecer contraseña** del panel de control del servicio, asegúrese de que **Correo electrónico de contraseña olvidada** se haya establecido en **Activado**.
3. Especifique el URL de la página de destino en el **URL para su página de restablecimiento de contraseña personalizada**  

Cuando se proporcione este valor, {{site.data.keyword.appid_short_notm}} llama al URL junto con una consulta `context`. El parámetro `context` se utiliza para recibir el resultado cuando se llame a `/forgot_password/confirmation_result`. Si el resultado es correcto, puede mostrar su página personalizada.

Añada una serie aleatoria a la página de restablecimiento de contraseña personalizada y pásela a su programa de fondo cuando se envíe la solicitud. Haga que el manejador valide la serie y llame al punto final `/change_password` solo si es válido. De esta manera, puede reducir la vulnerabilidad del punto final de programa de fondo del programa de fondo.
{: tip}

</br>
**Cambiar contraseña**

Puede utilizar el punto final `/change_password` de dos formas. Cuando un usuario envía una solicitud de restablecimiento, o cuando un usuario ha iniciado sesión en la app y desea actualizar su contraseña.

Proporcione los datos siguientes en el cuerpo de solicitud para actualizar su contraseña después de una solicitud de restablecimiento:
  * Su tenantID.
  * La nueva contraseña de los usuarios.
  * El UUID de usuario del directorio en la nube.
  * Opcional: la dirección IP desde la que se ha realizado el restablecimiento de contraseña. Si elige pasar la dirección IP, el marcador `%{passwordChangeInfo.ipAddress}` estará disponible para la plantilla de correo electrónico de cambio de contraseña.

Dependiendo de la configuración, cuando se modifica una contraseña, {{site.data.keyword.appid_short_notm}} podría enviar un correo electrónico al usuario haciéndole saber que hubo un cambio.

</br>
Para permitir a los usuarios cambiar su contraseña mientras estén dentro de su app:

Proporcione los datos siguientes en el cuerpo de solicitud:
  * Su tenantID.
  * La nueva contraseña de los usuarios.
  * El UUID de usuario del directorio en la nube.

La página de cambio de contraseña debe aparecerle al usuario para que especifique su contraseña actual y su nueva contraseña.
{: tip}

El programa de fondo valida la contraseña actual del usuario con la API ROP, y si es válida, llama al punto final con la contraseña nueva. Dependiendo de la configuración, cuando se modifica una contraseña, {{site.data.keyword.appid_short_notm}} podría enviar un correo electrónico al usuario haciéndole saber que hubo un cambio.

</br>
**Reenviar**

Puede utilizar `/resend/{templateName}` para volver a enviar un correo electrónico cuando un usuario no lo recibe por cualquier motivo.

Proporcione los datos siguientes en el cuerpo de solicitud:
  * El tenantID.
  * El nombre de la plantilla.
  * El UUID de usuario del directorio en la nube.

**Cambiar detalles**

Cuando un usuario haya iniciado sesión en su app, puede actualizar una parte de su información. Puede utilizar `/Users/{userId}` para obtener y actualizar su información.

Cuando se actualizan los detalles de usuario, el punto final obtiene los datos de usuario actualizados en el cuerpo de solicitud en [formato SCIM](https://tools.ietf.org/html/rfc7643#section-8.2). Asegúrese de cambiar únicamente los detalles relevantes.

Su dirección de correo electrónico no se puede cambiar.
{: tip}
