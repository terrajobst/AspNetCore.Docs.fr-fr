---
title: Présentation de l’authentification des applications à Page unique sur ASP.NET Core
author: javiercn
description: Utiliser l’identité avec une application à Page unique hébergées à l’intérieur d’une application ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 03/07/2019
uid: security/authentication/identity/spa
ms.openlocfilehash: 4afc9ac0a3c54b452c6a1b23e4de31d7e2fc5284
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665335"
---
# <a name="authentication-and-authorization-for-spas"></a>Authentification et autorisation pour les applications SPA

ASP.NET Core 3.0 ou version ultérieure offre l’authentification dans les applications à Page unique (SPA) à l’aide de la prise en charge pour l’autorisation de l’API. ASP.NET Core Identity pour l’authentification et le stockage des utilisateurs est combiné avec [IdentityServer](https://identityserver.io/) pour l’implémentation Open ID Connect.

Un paramètre d’authentification a été ajouté à la **Angular** et **réagir** est similaire au paramètre d’authentification dans les modèles de projet la **l’Application Web (Model-View-Controller)**  (MVC) et **Web Application** (Pages Razor) des modèles de projet. Les valeurs de paramètre autorisés sont **aucun** et **individuels**. Le **React.js et Redux** modèle de projet ne prend pas en charge le paramètre d’authentification pour l’instant.

## <a name="create-an-app-with-api-authorization-support"></a>Créer une application avec prise en charge des API d’autorisation

Autorisation et authentification utilisateur utilisable avec Angular et React applications à page unique. Ouvrez une invite de commandes et exécutez la commande suivante :

**Angular**:

```console
dotnet new angular -o <output_directory_name> -au Individual
```

**Réagir**:

```console
dotnet new react -o <output_directory_name> -au Individual
```

La commande précédente crée une application ASP.NET Core avec un *ClientApp* répertoire contenant l’application SPA.

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a>Description générale des composants de l’application ASP.NET Core

Les sections suivantes décrivent les ajouts au projet lors de la prise en charge de l’authentification est inclus :

### <a name="startup-class"></a>Classe de démarrage

Le `Startup` classe a les ajouts suivants :

* À l’intérieur de la `Startup.ConfigureServices` méthode :
  * Identité de l’interface utilisateur par défaut :

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * IdentityServer avec un autre `AddApiAuthorization` méthode d’assistance que paramétrages certains par défaut des conventions d’ASP.NET Core sur IdentityServer :

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * Authentification avec un autre `AddIdentityServerJwt` méthode d’assistance qui configure l’application pour valider les jetons JWT produites par IdentityServer :

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* À l’intérieur de la `Startup.Configure` méthode :
  * Le middleware d’authentification qui est chargé de valider les informations d’identification de la demande et de définition de l’utilisateur sur le contexte de requête :

    ```csharp
    app.UseAuthentication();
    ```

  * Le middleware IdentityServer qui expose les points de terminaison Open ID Connect :

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a>AddApiAuthorization

Cette méthode d’assistance configure IdentityServer pour utiliser notre configuration prise en charge. IdentityServer est une infrastructure puissante et extensible pour la gestion des problèmes de sécurité d’application. En même temps, qui expose une complexité inutile pour les scénarios les plus courants. Par conséquent, un ensemble de conventions et les options de configuration est fourni pour vous qui sont considérés comme un bon point de départ. Une fois que l’authentification de vos besoins évoluent, toute la puissance de IdentityServer est toujours disponible pour personnaliser l’authentification pour l’adapter à vos besoins.

### <a name="addidentityserverjwt"></a>AddIdentityServerJwt

Cette méthode d’assistance configure un jeu de stratégie pour l’application en tant que le Gestionnaire d’authentification par défaut. La stratégie est configurée pour l’identité permettent de gérer toutes les demandes sont acheminées vers n’importe quel sous-tracé dans l’espace de l’URL d’identité « / identité ». Le `JwtBearerHandler` gère toutes les autres demandes. En outre, cette méthode inscrit une `<<ApplicationName>>API` ressource de l’API avec IdentityServer avec une étendue par défaut de `<<ApplicationName>>API` et configure l’intergiciel de jeton du porteur JWT pour valider les jetons émis par IdentityServer pour l’application.

### <a name="sampledatacontroller"></a>SampleDataController

Dans le *Controllers\SampleDataController.cs* fichier, notez le `[Authorize]` basée sur les attributs appliqués à la classe qui indique que l’utilisateur doit être autorisé sur la stratégie par défaut pour accéder à la ressource. La stratégie d’autorisation par défaut se trouve être configuré pour utiliser le schéma d’authentification par défaut, qui est configuré par `AddIdentityServerJwt` avec le schéma de la stratégie qui a été mentionné ci-dessus, ce qui le `JwtBearerHandler` configuré par cette méthode d’assistance, le gestionnaire par défaut requêtes à l’application.

### <a name="applicationdbcontext"></a>ApplicationDbContext

Dans le *Data\ApplicationDbContext.cs* de fichier, notez que la même `DbContext` est utilisé dans l’identité avec l’exception qu’il étend `ApiAuthorizationDbContext` (plus dérivé de classe de `IdentityDbContext`) à inclure le schéma pour IdentityServer.

Pour obtenir un contrôle total du schéma de base de données, héritent d’un de l’identité disponible `DbContext` classes et configurer le contexte pour inclure le schéma de l’identité en appelant `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` sur la `OnModelCreating` (méthode).

### <a name="oidcconfigurationcontroller"></a>OidcConfigurationController

Dans le *Controllers\OidcConfigurationController.cs* de fichier, notez le point de terminaison est configuré pour servir les paramètres OIDC que le client doit utiliser.

### <a name="appsettingsjson"></a>appsettings.json

Dans le *appsettings.json* fichier de la racine du projet, il y a une nouvelle `IdentityServer` section qui décrit la liste des configuré les clients. Dans l’exemple suivant, il est un client unique. Le nom du client correspond au nom de l’application et est mappé par convention pour le OAuth `ClientId` paramètre. Le profil indique le type d’application en cours de configuration. Il est utilisé en interne aux conventions de lecteur qui simplifient le processus de configuration pour le serveur. Plusieurs profils sont disponibles, comme expliqué dans la [profils d’Application](#application-profiles) section.

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a>appsettings.Development.json

Dans le *appsettings. Development.JSON* fichier de la racine du projet, il existe un `IdentityServer` section qui décrit la clé utilisée pour signer les jetons. Lors du déploiement en production, une clé doit être configuré et déployé en même temps que l’application, comme expliqué dans la [déployer en production](#deploy-to-production) section.

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="general-description-of-the-angular-app"></a>Description générale de l’application Angular

Prend en charge l’authentification et l’autorisation de l’API dans Angular modèle réside dans son propre module Angular dans le *ClientApp\src\api-autorisation* directory. Le module se compose des éléments suivants :

* 3 composants :
  * *Login.Component.TS*: Gère le flux de connexion de l’application.
  * *Logout.Component.TS*: Gère le flux de déconnexion de l’application.
  * *connexion-menu.component.ts*: Un widget qui affiche l’un des ensembles de liens suivants :
    * Gestion de profil utilisateur et déconnexion de liens lorsque l’utilisateur est authentifié.
    * L’inscription et journal dans les liens lorsque l’utilisateur n’est pas authentifié.
* Un garde itinéraire `AuthorizeGuard` qui peuvent être ajoutés à des itinéraires et nécessite un utilisateur de s’authentifier avant de visiter l’itinéraire.
* Un intercepteur HTTP `AuthorizeInterceptor` qui attache le jeton d’accès à des requêtes HTTP sortantes ciblant l’API lorsque l’utilisateur est authentifié.
* Un service `AuthorizeService` qui gère les détails de niveau inférieur du processus d’authentification et expose des informations sur l’utilisateur authentifié au reste de l’application pour la consommation.
* Un module Angular qui définit les itinéraires associés avec les parties de l’authentification de l’application. Il expose le composant de menu de connexion, l’intercepteur, le module de protection et le service pour la consommation du reste de l’application.

## <a name="general-description-of-the-react-app"></a>Description générale de l’application React

La prise en charge pour l’authentification et l’autorisation d’API dans le modèle de React réside dans le *ClientApp\src\components\api-autorisation* directory. Il est composé des éléments suivants :

* 4 composants :
  * *Login.js*: Gère le flux de connexion de l’application.
  * *Logout.js*: Gère le flux de déconnexion de l’application.
  * *LoginMenu.js*: Un widget qui affiche l’un des ensembles de liens suivants :
    * Gestion de profil utilisateur et déconnexion de liens lorsque l’utilisateur est authentifié.
    * L’inscription et journal dans les liens lorsque l’utilisateur n’est pas authentifié.
  * *AuthorizeRoute.js*: Un composant de routage qui nécessite un utilisateur de s’authentifier avant de le restituer le composant indiqué dans le `Component` paramètre.
* Un exporté `authService` instance de classe `AuthorizeService` qui gère les détails de niveau inférieur du processus d’authentification et expose des informations sur l’utilisateur authentifié au reste de l’application pour la consommation.

Maintenant que vous avez vu les principaux composants de la solution, vous pouvez tirer étudier des scénarios individuels pour l’application.

## <a name="require-authorization-on-a-new-api"></a>Requièrent une autorisation sur une nouvelle API

Par défaut, le système est configuré pour exiger facilement l’autorisation de nouvelles API. Pour ce faire, créez un nouveau contrôleur et ajoutez le `[Authorize]` attribut à la classe de contrôleur ou à des actions dans le contrôleur.

## <a name="protect-a-client-side-route-angular"></a>Protéger un itinéraire côté client (Angular)

Protéger un itinéraire côté client s’effectue en ajoutant de la garde authorize à la liste des gardes à exécuter lors de la configuration d’un itinéraire. Par exemple, vous pouvez voir comment la `fetch-data` itinéraire est configuré dans le module d’Angular principal d’application :

```typescript
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

Il est important de mentionner que protégeant un itinéraire ne protège pas le point de terminaison réel (ce qui nécessite toujours un `[Authorize]` attribut appliqué à ce dernier), mais qu’elle empêche uniquement l’utilisateur de naviguer vers l’itinéraire côté client donnée lorsqu’il n’est pas authentifié.

## <a name="authenticate-api-requests-angular"></a>Authentifier les demandes d’API (Angular)

L’authentification des demandes pour les API hébergées en même temps que l’application est effectuée automatiquement via l’utilisation de l’intercepteur de client HTTP défini par l’application.

## <a name="protect-a-client-side-route-react"></a>Protéger un itinéraire côté client (React)

Protéger un itinéraire côté client à l’aide de la `AuthorizeRoute` composant au lieu du brut `Route` composant. Par exemple, notez comment la `fetch-data` itinéraire est configuré dans le `App` composant :

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

Protéger un itinéraire :

* Ne protège pas le point de terminaison réel (ce qui nécessite toujours un `[Authorize]` attribut appliqué à celui-ci).
* Uniquement empêche l’utilisateur de naviguer vers l’itinéraire côté client donnée lorsqu’il n’est pas authentifié.

## <a name="authenticate-api-requests-react"></a>Authentifier les demandes d’API (React)

L’authentification des demandes avec React s’effectue en important d’abord le `authService` instance à partir de la `AuthorizeService`. Le jeton d’accès est récupéré à partir de la `authService` et est associé à la demande, comme indiqué ci-dessous. Dans composants React, ce travail est généralement effectué le `componentDidMount` méthode de cycle de vie ou en tant que le résultat à partir d’une interaction utilisateur.

### <a name="import-the-authservice-into-your-component"></a>Importer l’authService dans votre composant

```javascript
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a>Récupérer et joignez le jeton d’accès à la réponse

```javascript
async populateWeatherData() {
  const token = await authService.getAccessToken();
  const response = await fetch('api/SampleData/WeatherForecasts', {
    headers: !token ? {} : { 'Authorization': `Bearer ${token}` }
  });
  const data = await response.json();
  this.setState({ forecasts: data, loading: false });
}
```

## <a name="deploy-to-production"></a>Déployer en production

Pour déployer l’application en production, les ressources suivantes doivent être approvisionnés :

* Une base de données pour stocker les comptes d’utilisateur de l’identité et les privilèges accordés IdentityServer.
* Un certificat de production à utiliser pour la signature de jetons.
  * Il n’y a aucune exigence particulière pour ce certificat ; Il peut être un certificat auto-signé ou un certificat configuré via une autorité de certification.
  * Il peut être généré par le biais des outils standards tels que PowerShell ou OpenSSL.
  * Il peut être installé dans le magasin de certificats sur les ordinateurs cibles ou déployé comme un *.pfx* fichier avec un mot de passe fort.

### <a name="example-deploy-to-azure-websites"></a>Exemple : Déployer des sites Web Azure

Cette section décrit le déploiement de l’application à des sites Web Azure à l’aide d’un certificat stocké dans le magasin de certificats. Pour modifier l’application pour charger un certificat du magasin de certificats, le plan App Service doit s’exécuter sur au moins le niveau Standard lorsque vous configurez dans une étape ultérieure. Dans l’application *appsettings.json* fichier, modifiez le `IdentityServer` section pour inclure les détails de la clé :

```json
"IdentityServer": {
  "Key": {
    "Type": "Store",
    "StoreName": "My",
    "StoreLocation": "CurrentUser",
    "Name": "CN=MyApplication"
  }
}
```

* La propriété nom de certificat correspond à l’objet du certificat unique.
* L’emplacement du magasin représente l’emplacement de charger le certificat à partir de (`CurrentUser` ou `LocalMachine`).
* Le nom du magasin représente le nom du magasin de certificats dans lequel le certificat est stocké. Dans ce cas, il pointe vers le magasin utilisateur personnel.

Pour déployer sur Azure Websites, déployez l’application en suivant les étapes décrites dans [déployer l’application sur Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) pour créer les ressources Azure nécessaires et de déployer l’application en production.

Après avoir suivi les instructions précédentes, l’application est déployée sur Azure, mais n’est pas encore fonctionnelle. Le certificat utilisé par l’application doit toujours être configuré. Localiser l’empreinte numérique du certificat à utiliser, puis suivez les étapes décrites dans [charger vos certificats](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).

Bien que ces étapes mentionner SSL, il existe un **certificats privés** section du portail où vous pouvez télécharger le certificat configuré à utiliser avec l’application.

Après cette étape, redémarrez l’application et il doit être fonctionnel.

## <a name="other-configuration-options"></a>Autres options de configuration

La prise en charge pour l’autorisation de l’API s’appuie sur IdentityServer avec un ensemble de conventions, les valeurs par défaut et les améliorations apportées à simplifier l’expérience pour les applications SPA. Inutile de dire que toute la puissance de IdentityServer est disponible dans les coulisses si votre scénario ne couvrent pas les intégrations d’ASP.NET Core. La prise en charge d’ASP.NET Core se concentre sur les applications « internes », où toutes les applications sont créées et déployées par notre organisation. Par conséquent, il n’est pas pris en charge pour les éléments tels que le consentement ou la fédération. Pour ces scénarios, utilisez IdentityServer et suivre leur documentation.

### <a name="application-profiles"></a>Profils d’application

Profils d’application sont des configurations prédéfinies pour les applications qui définissent leurs paramètres. À ce stade, les profils suivants sont pris en charge :

* `IdentityServerSPA`: Représente une application SPA hébergée avec IdentityServer comme une seule unité.
  * Le `redirect_uri` par défaut est `/authentication/login-callback`.
  * Le `post_logout_redirect_uri` par défaut est `/authentication/logout-callback`.
  * Le jeu d’étendues inclut le `openid`, `profile`et chaque étendue définie pour les API dans l’application.
  * Est l’ensemble des types de réponse autorisés OIDC `id_token token` ou chacun d’eux individuellement (`id_token`, `token`).
  * Le mode de réponse autorisés est `fragment`.
* `SPA`: Représente une application à page unique qui n’est pas hébergée avec IdentityServer.
  * Le jeu d’étendues inclut le `openid`, `profile`et chaque étendue définie pour les API dans l’application.
  * Est l’ensemble des types de réponse autorisés OIDC `id_token token` ou chacun d’eux individuellement (`id_token`, `token`).
  * Le mode de réponse autorisés est `fragment`.
* `IdentityServerJwt`: Représente une API qui est hébergée en même temps qu’avec IdentityServer.
  * L’application est configurée pour avoir une seule étendue par défaut est le nom de l’application.
* `API`: Représente une API qui n’est pas hébergée avec IdentityServer.
  * L’application est configurée pour avoir une seule étendue par défaut est le nom de l’application.

### <a name="configuration-through-appsettings"></a>Configuration via AppSettings

Configurer les applications via le système de configuration en les ajoutant à la liste des `Clients` ou `Resources`.

Configurer de chaque client `redirect_uri` et `post_logout_redirect_uri` propriété, comme indiqué dans l’exemple suivant :

```json
"IdentityServer": {
  "Clients": {
    "MySPA": {
      "Profile": "SPA",
      "RedirectUri": "https://www.example.com/authentication/login-callback",
      "LogoutUri": "https://www.example.com/authentication/logout-callback"
    }
  }
}
```

Lors de la configuration des ressources, vous pouvez configurer les étendues pour la ressource, comme indiqué ci-dessous :

```json
"IdentityServer": {
  "Resources": {
    "MyExternalApi": {
      "Profile": "API",
      "Scopes": "a b c"
    }
  }
}
```

### <a name="configuration-through-code"></a>Configuration via le code

Vous pouvez également configurer les clients et les ressources dans le code à l’aide d’une surcharge de `AddApiAuthorization` qui effectue une action pour configurer les options.

```csharp
AddApiAuthorization<ApplicationUser, ApplicationDbContext>(options =>
{
    options.Clients.AddSPA(
        "My SPA", spa =>
        spa.WithRedirectUri("http://www.example.com/authentication/login-callback")
           .WithLogoutRedirectUri(
               "http://www.example.com/authentication/logout-callback"));

    options.ApiResources.AddApiResource("MyExternalApi", resource =>
        resource.WithScopes("a", "b", "c"));
});
```

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:spa/angular>
* <xref:spa/react>
