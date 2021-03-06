---

copyright:
  years: 2017, 2018
lastupdated: "2018-11-02"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:tip: .tip}


# Gestion
{: #managing}

Les fournisseurs d'identité (IdP) ajoutent un niveau de sécurité pour vos applications mobiles et Web via l'authentification. Avec {{site.data.keyword.appid_full}}, vous pouvez configurer un ou plusieurs fournisseurs d'identité pour créer une expérience de connexion personnalisée pour vos utilisateurs.
{: shortdesc}


**Qu'est-ce qu'un fournisseur d'identité ?**

Un fournisseur d'identité crée et gère des informations sur une entité telle qu'un utilisateur, un ID fonctionnel ou une application. Il vérifie l'identité de l'entité à l'aide de données d'identification, par exemple un mot de passe. Le fournisseur d'identité envoie ensuite les informations d'identité à un autre fournisseur de services. Le fait que le fournisseur d'identité authentifie une entité permet à {{site.data.keyword.appid_short_notm}} d'accorder à cette entité des droits et un accès à vos applications.

</br>

**Quels sont les fournisseurs d'identité pour lesquels {{site.data.keyword.appid_short_notm}} fournit une intégration ?**

Le service est préconfiguré pour utiliser plusieurs fournisseurs.

<table>
  <tr>
    <th>Fournisseur</th>
    <th>Type</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>[Cloud Directory](/docs/services/appid/cloud-directory.html)</td>
    <td>Registre géré</td>
    <td>Vous pouvez gérer votre propre registre d'utilisateurs dans le cloud. Lorsqu'un utilisateur s'inscrit à votre application, il est ajouté à votre répertoire d'utilisateurs. Cette option offre à vos utilisateurs une plus grande liberté pour gérer leurs propres comptes au sein de votre application.</td>
  </tr>
  <tr>
    <td>[SAML](/docs/services/appid/enterprise.html)</td>
    <td>Entreprise</td>
    <td>Vous pouvez créer une expérience de connexion unique pour vos utilisateurs finaux.</td>
  </tr>
  <tr>
    <td>[Facebook](/docs/services/appid/identity-providers.html#facebook)</td>
    <td>Social</td>
    <td>Les utilisateurs finaux peuvent se connecter à votre application à l'aide de leurs données d'identification Facebook.</td>
  </tr>
  <tr>
    <td>[Google+](/docs/services/appid/identity-providers.html#google)</td>
    <td>Social</td>
    <td>Les utilisateurs finaux peuvent se connecter à votre application à l'aide de leurs données d'identification Google+.</td>
  </tr>
  <tr>
    <td>[Personnalisé](/docs/services/appid/custom.html)</td>
    <td> </td>
    <td>Si aucune des options fournies ne répond à vos besoins particuliers, vous pouvez configurer votre propre flux d'identité pour l'utiliser avec {{site.data.keyword.appid_short_notm}}.</td>
  </tr>
  
</table>

</br>

**Comment {{site.data.keyword.appid_short_notm}} interagit-il avec un fournisseur d'identité ?**

{{site.data.keyword.appid_short_notm}} interagit avec les fournisseurs d'identité en utilisant plusieurs protocoles tels qu'OpenID Connect, SAML, etc. Par exemple, OpenID Connect est le protocole utilisé avec de nombreux fournisseurs sociaux tels que Facebook, Google. Les fournisseurs d'entreprise tels qu'<a href="https://www.ibm.com/blogs/bluemix/2018/03/setting-ibm-cloud-app-id-azure-active-directory/" target="_blank">Azure Active Directory <img src="../../icons/launch-glyph.svg" alt="Icône de lien externe"></a> ou <a href="https://www.ibm.com/blogs/bluemix/2018/03/setting-ibm-cloud-app-id-active-directory-federation-service/" target="_blank">Active Directory Federation Service <img src="../../icons/launch-glyph.svg" alt="Icône de lien externe"></a> utilisent généralement SAML comme protocole d'identité. Pour [Cloud Directory](cloud-directory.html), le service utilise SCIM pour vérifier les informations d'identité.

Vous utilisez une identité d'application ? Consultez [Application identity](app-to-app.html).
{: tip}

</br>
</br>

## Configuration des paramètres
{: #configuring}

Vous pouvez choisir les fournisseurs que vous souhaitez utiliser, vos URL de redirection et les informations de jeton pour votre application. Les paramètres que vous configurez s'appliquent à tous vos fournisseurs d'identité dans cette instance du service. Par exemple, lorsque vous définissez l'expiration du jeton, elle s'applique à tous les fournisseurs que vous avez configurés, qu'ils soient sociaux ou d'entreprise.

</br>

**Pour configurer vos fournisseurs :**

1. Accédez à votre tableau de bord du service.
2. Dans la section **Fournisseurs d'identité** de la navigation, sélectionnez la page **Gérer**.
3. Dans l'onglet **Fournisseurs d'identité**, définissez les fournisseurs que vous souhaitez utiliser sur **On**.
4. Facultatif : indiquez si vous souhaitez désactiver **Utilisateurs anonymes** ou conserver la valeur par défaut **On**. Lorsque la valeur est définie sur **On**, des attributs utilisateur personnalisés sont associés à l'utilisateur à partir du moment où il commence à interagir avec votre application. Pour plus d'informations sur la marche à suivre pour devenir un utilisateur identifié, voir [Authentification progressive](progressive.html#progressive).

{{site.data.keyword.appid_short_notm}} fournit des données d'identification par défaut pour vous aider lors de la configuration initiale de Facebook et Google+. Ces données d'identification sont limitées à 100 utilisations par instance, par jour. Comme il s'agit de données d'identification IBM, elles sont destinées à être utilisées uniquement pour le développement. Avant de publier votre application, mettez à jour la configuration avec vos propres données d'identification.
{: tip}

</br>

**Pour configurer vos paramètres :**

1. Ajoutez vos URL de redirection. Une URL de redirection est le noeud final de rappel de votre application. Pour empêcher les attaques par hameçonnage, {{site.data.keyword.appid_short_notm}} valide les URL par rapport à la liste blanche.
  1. Dans la zone **Ajouter une URL de redirection Web**, entrez l'URL et cliquez sur l'icône **+**.
  2. Répétez l'étape 1 jusqu'à ce que toutes les URL de redirection possibles soient ajoutées à la liste.

  N'incluez pas de paramètres de demande dans votre URL. Ils sont ignorés lors du processus de validation. Par exemple : `http://host:[port]/path`
  {: tip}

2. Configurez vos jetons. La durée de vie des jetons recommence à chaque connexion d'utilisateur. Par exemple, vous définissez la durée de vie de votre jeton d'actualisation sur 10 jours. Un jeton d'accès et un jeton d'actualisation sont créés lorsque l'utilisateur se connecte pour la première fois. Si l'utilisateur revient dans votre application quelques jours plus tard, 3 jours pour être précis, il n'aura pas besoin de se connecter à nouveau. Toutefois, si l'utilisateur attend 12 jours après la connexion initiale pour revenir dans votre application, il devra se connecter à nouveau et un ensemble de jetons lui sera associé.
  1. Pour autoriser la connexion sans interaction de l'utilisateur, définissez les **jetons d'actualisation** sur **On**.
  2. Définissez la durée de vie de votre jeton d'actualisation. L'expiration est définie en jours et peut être n'importe quelle valeur comprise entre 1 et 90. Plus le nombre est petit, plus l'utilisateur doit s'inscrire souvent.
  3. Définissez la durée de vie de votre jeton d'accès. L'expiration est définie en minutes et peut être comprise entre 5 et 1440. Plus la valeur est petite, plus la protection est importante en cas de vol de jetons.
  4. Définissez la durée de vie de votre jeton anonyme. Un [jeton anonyme](/docs/services/appid/progressive.html#anonymous) est affecté aux utilisateurs à partir du moment où ils commencent à interagir avec votre application. Lorsqu'un utilisateur se connecte, les informations contenues dans le jeton anonyme sont alors transférées au jeton associé à l'utilisateur. L'expiration est définie en jours et peut être n'importe quelle valeur comprise entre 1 et 90. 

</br>
</br>
