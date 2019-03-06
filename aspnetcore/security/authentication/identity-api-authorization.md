---
title: Présentation de l’authentification pour les Applications à Page unique sur ASP.NET Core
author: ''
description: Utiliser l’identité avec une application à page unique hébergée à l’intérieur d’une application ASP.NET Core.
ms.author: ''
ms.date: 03/05/2018
uid: security/authentication/identity/spa
ms.openlocfilehash: cf04ec1ff0ae9afea066fd1864ab0a7956ace32c
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346750"
---
# <a name="authentication-and-authorization-for-spas"></a>Authentification et autorisation pour les applications SPA

Dans ASP.NET 3.0, nous vous présentons la prise en charge pour l’authentification dans les applications à page unique à l’aide de notre nouvelle prise en charge pour l’autorisation de l’API. Cette prise en charge est basé sur une combinaison de ASP.NET Core Identity pour l’authentification et le stockage des utilisateurs et le serveur d’identité pour l’implémentation Open ID Connect.

Nous avons ajouté un nouveau paramètre d’authentification à notre Angular et les modèles de React est similaire au paramètre d’authentification dans nos modèles de pages mvc et razor avec valeurs autorisées « None » et « Individuel ».

## <a name="create-an-angular-app-with-api-authorization-support"></a>Créer une application Angular avec prise en charge des API d’autorisation

Pour créer une nouvelle application Angular avec prise en charge pour l’authentification et autorisation des utilisateurs, ouvrez une invite de commandes et exécutez la commande suivante :

```console
dotnet new angular -o <output_directory_name> -au Individual
```

La commande précédente crée une application ASP.NET Core avec un *ClientApp* répertoire contenant l’application Angular.

## <a name="create-a-react-app-with-api-authorization-support"></a>Créer une application de React avec prise en charge des API d’autorisation

Pour créer une nouvelle application React avec prise en charge pour l’authentification et autorisation des utilisateurs, ouvrez une invite de commandes et exécutez la commande suivante :

```console
dotnet new react -o <output_directory_name> -au Individual
```

La commande précédente crée une application ASP.NET Core avec un *ClientApp* répertoire contenant l’application React.

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a>Description générale des composants de l’application ASP.NET Core

Il existe plusieurs ajouts au projet lorsque nous incluons la prise en charge pour l’authentification :

### <a name="startup-class"></a>Classe de démarrage

Si nous examinons le code dans la classe de démarrage ci-dessous nous pouvons le constater les inclusions suivantes :
* À l’intérieur de `public void ConfigureServices(IServiceCollection services)`:
  * Identité avec l’interface utilisateur par défaut.
    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
      options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
      .AddDefaultUI(UIFramework.Bootstrap4)
      .AddEntityFrameworkStores<ApplicationDbContext>();
    ```
  * Serveur d’identité avec une méthode d’assistance de AddApiAuthorization supplémentaire que paramétrages certains par défaut des Conventions de ASP.NET sur le serveur d’identité.
    ```csharp
    services.AddIdentityServer()
      .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```
  * Authentification avec une méthode d’assistance AddIdentityServerJwt supplémentaire qui configure l’application pour valider les jetons Jwt générés par le serveur d’identité. 
    ```csharp
    services.AddAuthentication()
      .AddIdentityServerJwt();
    ```
* À l’intérieur de `public void Configure(IApplicationBuilder app)`:
  * Le middleware d’authentification qui est chargé de valider les informations d’identification dans la demande entrante et de définition de l’utilisateur sur le contexte de requête.
    ```csharp
    app.UseAuthentication();
    ```
  * Le middleware de serveur d’identité qui expose les points de terminaison Open ID Connect.
    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a>AddApiAuthorization 
Cette méthode d’assistance configure Identity Server pour utiliser notre configuration prise en charge. Serveur d’identité est une infrastructure très puissante et extensible pour la gestion des problèmes de sécurité d’application, mais en même temps qui expose une grande complexité que nous n’avez pas besoin de connaître pour les scénarios les plus courants, par conséquent, nous choisissons un ensemble de conventions et options de configuration pour vous que nous considérons constituent un bon point de départ. Une fois que l’authentification de vos besoins évoluent toute la puissance de serveur d’identité est toujours disponible pour vous afin de pouvoir personnaliser selon vos besoins.

### <a name="addidentityserverjwt"></a>AddIdentityServerJwt
Cette méthode d’assistance configure un jeu de stratégie pour l’application en tant que le Gestionnaire d’authentification par défaut. La stratégie est configurée pour l’identité permettent de gérer toutes les demandes qui mènent à n’importe quel sous-tracé dans l’espace de l’url d’identité « / identité » et de laisser le JwtBearerHandler gérer toutes les autres demandes.
Plus cette méthode inscrit une `<<ApplicationName>>API` avec identity server avec une étendue par défaut de ressource de l’Api `<<ApplicationName>>API` et configure l’intergiciel de jeton du porteur JWT pour valider les jetons émis par le serveur d’identité pour l’application.

### <a name="sampledatacontroller"></a>SampleDataController
Si nous examinons le fichier Controllers\SampleDataController.cs, nous pouvons observer le `[Authorize]` basée sur les attributs appliqués à la classe qui indique que l’utilisateur doit être autorisé sur la stratégie par défaut pour accéder à la ressource. La stratégie d’autorisation par défaut se trouve être configuré pour utiliser le schéma d’authentification par défaut qui est configuré par `AddIdentityServerJwt` au jeu de stratégie que nous avons mentionnés ci-dessus, ce qui le gestionnaire JwtBearer configuré par cette méthode d’assistance le gestionnaire par défaut requêtes à l’application.

### <a name="applicationdbcontext"></a>ApplicationDbContext
Si nous examinons le fichier dans Data\ApplicationDbContext.cs, nous pouvons voir le même DbContext que nous utilisons dans identité avec l’exception qu’il étend ApiAuthorizationDbContext (une classe plus dérivée à partir de l’IdentityDbContext) pour inclure le schéma pour le serveur d’identité.
Si nous voulons que le contrôle total sur le schéma de base de données nous pouvons simplement héritent d’une des classes DbContext d’identité disponibles et configurer le contexte pour inclure le schéma de l’identité en appelant `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` sur la `OnModelCreating` (méthode).

### <a name="oidcconfigurationcontroller"></a>OidcConfigurationController
Si nous examinons le fichier Controllers\OidcConfigurationController.cs le point de terminaison, nous pouvons voir que nous consultons pour traiter les paramètres OIDC que le client doit utiliser.

### <a name="appsettingsjson"></a>appsettings.json
Si nous examinons le fichier appsettings.json sur la racine du projet, nous pouvons voir une nouvelle `IdentityServer` section qui décrit la liste des configuré les clients et nous pouvons voir qu’il existe un seul client. Le nom du client correspond au nom de l’application et est mappé par convention pour le paramètre d’ID client oAuth. Le profil indique le type d’application que nous allons configurer et nous l’utilisons en interne pour les conventions de lecteur qui simplifient le processus de configuration pour le serveur. Il existe plusieurs profils disponibles sont expliqués dans la section ci-dessous.

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
Si nous examinons l’appsettings. Fichier de Development.JSON sur la racine du projet, nous pouvons voir une nouvelle `IdentityServer` section qui décrit la clé que nous utilisons pour signer les jetons. Lors du déploiement en production une clé doit être configuré et déployé en même temps que l’application, comme expliqué ci-dessous.

```json
  "IdentityServer": {
    "Key": {
      "Type": "Development"
    }
  }
}
```

## <a name="general-description-of-the-angular-application"></a>Description générale de l’application Angular
La prise en charge pour l’authentification et l’autorisation d’API dans le modèle Angular se trouve dans son propre module Angular. Sous l’autorisation ClientApp\src\api et il sont composés des éléments suivants :
* 3 composants :
  * Composant de la connexion : Gère le flux de connexion pour l’application.
  * Composant de déconnexion : Gère le flux de déconnexion de l’application.
  * Composant de menu de connexion : Un widget qui affiche l’utilisateur authentifié actuel avec des liens pour gérer le profil utilisateur et se déconnecter ou de liens pour se connecter ou inscrire lorsque l’utilisateur n’est pas authentifié.
* Un garde itinéraire `AuthorizeGuard` qui peuvent être ajoutés à des itinéraires et nécessite un utilisateur de s’authentifier avant de visiter l’itinéraire.
* Un intercepteur http `AuthorizeInterceptor` qui attache le jeton d’accès à des requêtes HTTP sortantes ciblant l’API lorsque l’utilisateur est authentifié.
* Un service `AuthorizeService` qui gère les détails de niveau inférieur du processus d’authentification et expose des informations sur l’utilisateur authentifié au reste de l’application pour la consommation.
* Un module angular qui définit les itinéraires associés avec les parties de l’authentification de l’application et expose le composant de menu de connexion, l’intercepteur, le module de protection et le service pour la consommation du reste de l’application.

## <a name="general-description-of-the-react-application"></a>Description générale de l’application React
La prise en charge pour l’authentification et l’autorisation d’API dans la vie de modèle React sous ClientApp\src\components\api-authorization\ et il est composé des éléments suivants :
* 4 composants :
  * Composant de la connexion : Gère le flux de connexion pour l’application.
  * Composant de déconnexion : Gère le flux de déconnexion de l’application.
  * Composant de menu de connexion : Un widget qui affiche l’utilisateur authentifié actuel avec des liens pour gérer le profil utilisateur et se déconnecter ou de liens pour se connecter ou inscrire lorsque l’utilisateur n’est pas authentifié.
  * AuthorizeRoute : Un composant de routage qui oblige l’utilisateur de s’authentifier avant le composant indiqué dans le paramètre de composant de rendu.
  * Un exporté `authService` instance de classe `AuthorizeService` qui gère les détails de niveau inférieur du processus d’authentification et expose des informations sur l’utilisateur authentifié au reste de l’application pour la consommation.

Maintenant que nous avons vu les principaux composants de la solution, nous pouvons prendre une apparence spécifique à des scénarios individuels pour l’application :

## <a name="requiring-authorization-on-a-new-api"></a>Nécessitant une autorisation sur une nouvelle API
Le système est configuré en dehors de la zone pour le rendre facile à exiger l’autorisation de nouvelles API. Pour ce faire, créez simplement un nouveau contrôleur et ajouter la `[Authorize]` attribut à la classe de contrôleur ou à des actions dans le contrôleur.

## <a name="protecting-a-client-side-route-angular"></a>Protection d’un itinéraire côté client (Angular)
Protéger un itinéraire de côté client s’effectue en ajoutant de la garde authorize à la liste des gardes à exécuter lors de la configuration d’un itinéraire. Par exemple, vous pouvez voir comment l’itinéraire de récupérer des données est configuré dans le module d’angular principal d’application :

```ts
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

Il est important de mentionner que protégeant un itinéraire ne protège pas le point de terminaison réel (ce qui nécessite toujours un `[Authorize]` attribut appliqué à ce dernier), mais qu’elle empêche uniquement l’utilisateur de naviguer vers l’itinéraire du côté client donné quand il n’est pas authentifiée.

## <a name="authenticate-api-requests-angular"></a>Authentifier les demandes d’API (Angular)

Authentification des demandes aux API hébergé côté que l’application est effectuée automatiquement via l’utilisation de l’intercepteur de client HTTP défini par l’application.

## <a name="protect-a-client-side-route-react"></a>Protéger un itinéraire côté client (React)

Protection d’un itinéraire de côté client est effectuée à l’aide du composant AuthorizeRoute au lieu du composant de routage simple. Par exemple, vous pouvez voir comment l’itinéraire de récupérer des données est configuré dans le composant d’application :

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

Il est important de mentionner que protégeant un itinéraire ne protège pas le point de terminaison réel (ce qui nécessite toujours un `[Authorize]` attribut appliqué à ce dernier), mais qu’elle empêche uniquement l’utilisateur de naviguer vers l’itinéraire du côté client donné quand il n’est pas authentifiée.

## <a name="authenticate-api-requests-react"></a>Authentifier les demandes d’API (React)

L’authentification des demandes avec react s’effectue en important d’abord le `authService` instance à partir de la `AuthorizeService` puis récupérer le jeton d’accès à partir de l’authService et attachement à la demande, comme indiqué ci-dessous. Dans les composants de react est généralement cela dans la méthode de cycle de vie componentDidMount ou en tant que le résultat à partir d’une interaction utilisateur.

### <a name="import-the-authservice-into-your-component"></a>Importer l’authService dans votre composant

```js
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a>Récupérer et joignez le jeton d’accès à la réponse

```js
async populateWeatherData() {
  const token = await authService.getAccessToken();
  const response = await fetch('api/SampleData/WeatherForecasts', {
    headers: !token ? {} : { 'Authorization': `Bearer ${token}` }
  });
  const data = await response.json();
  this.setState({ forecasts: data, loading: false });
}
```

## <a name="deploy-into-production"></a>Déployer en production

Pour déployer l’application en production, nous devons configurer plusieurs ressources :
* Accorde à une base de données pour stocker les comptes d’identité utilisateur et le serveur d’identité.
* Un certificat de production à utiliser pour la signature de jetons.
  * Il n’y a aucune exigence particulière pour ce certificat ; Il peut être un certificat auto-signé ou un certificat configuré via une autorité de certification.
  * Il peut être généré par le biais des outils standards tels que powershell ou openssl.
  * Il peut être installé dans le magasin de certificats sur les ordinateurs cibles ou déployé dans un fichier pfx avec un mot de passe fort.

### <a name="example-deploy-into-azure-websites"></a>Exemple : Déployer des sites Web Azure

Dans cette section, nous allons déployer l’application sur sites Web Azure à l’aide d’un certificat stocké dans le magasin de certificats. Nous devons modifier l’application pour charger un certificat du magasin de certificats. Pour ce faire, notre plan app service doit être au moins du niveau standard lorsque nous configurons dans une étape ultérieure. Dans notre application, nous devons simplement modifier la section IdentityServer sur appsettings.json pour inclure les détails de la clé :
```json
  "IdentityServer": {
    "Key": {
      "Type": "Store",
      "StoreName": "My",
      "StoreLocation": "CurrentUser",
      "Name": "CN=MyApplication"
    }
  }
}
```
* La propriété nom de certificat correspond à l’objet du certificat unique.
* L’emplacement du magasin représente l’emplacement de charger le certificat à partir de (CurrentUrser ou LocalMachine).
* Le nom du magasin représente le nom du magasin de certificats dans lequel le certificat est stocké dans ce cas, qu'il pointe vers le magasin utilisateur personnel.

Pour déployer sur Azure Websites, déployez l’application en suivant les étapes décrites dans [déployer l’application sur Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) pour créer les ressources Azure nécessaires et de déployer l’application en production.

Après cette opération, l’application est déployée dans Azure, mais n’est pas encore complètement fonctionnelle que nous devons encore configurer le certificat à utiliser par l’application. Pour ce faire, nous devons disposer de l’empreinte numérique du certificat que nous allons utiliser et suivez les étapes décrites dans [charger vos certificats](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).

Ces étapes mentionnent SSL, mais il existe une section « Certificats privés » sur le portail où nous pouvons télécharger notre certificat mis en service à utiliser avec notre application.

Après cette étape, nous devrions pouvoir redémarrer notre application, et il doit être complètement fonctionnel.

## <a name="other-configuration-options"></a>Autres options de configuration
La prise en charge pour l’autorisation de l’API s’appuie sur Identity Server avec un ensemble de conventions, les valeurs par défaut et les améliorations apportées à simplifier l’expérience des Applications à Page unique. Inutile de dire que toute la puissance de serveur d’identité est disponible dans les coulisses si votre scénario ne couvrent pas les intégrations que nous offrons. La prise en charge se concentre sur ce que nous appelons « internes » des applications, où toutes les applications sont créées et déployées par notre organisation. Par conséquent nous n’offrent pas prise en charge des éléments tels que le consentement ou la fédération. Pour ces scénarios, notre recommandation est Identity Server et suivez leur documentation.

### <a name="application-profiles"></a>Profils d’application
Profils d’application sont des configurations prédéfinies pour les applications qui définissent leurs paramètres. À ce stade, nous prenons en charge deux profils :
* IdentityServerSPA: Représente une application à page unique hébergée en même temps que le serveur d’identité comme une seule unité.
  * Par défaut est le paramètre redirect_uri `/authentication/login-callback`.
  * Par défaut est le post_logout_redirect_uri `/authentication/logout-callback`.
  * Le jeu d’étendues inclut le `openid`, `profile`et chaque étendue définie pour les API dans l’application.
  * Est l’ensemble des types de réponse autorisés OIDC `id_token token` ou chacun d’eux individuellement (`id_token`, `token`).
  * Le mode de réponse autorisés est `fragment`.
* SPA : Représente une application à page unique qui n’est pas hébergée avec Identity Server.
  * Le jeu d’étendues inclut le `openid`, `profile`et chaque étendue définie pour les API dans l’application.
  * Est l’ensemble des types de réponse autorisés OIDC `id_token token` ou chacun d’eux individuellement (`id_token`, `token`).
  * Le mode de réponse autorisés est `fragment`.
* IdentityServerJwt: Représente une API qui est hébergée en même temps qu’avec Identity Server.
  * L’application est configurée pour avoir une étendue par défaut est le nom de l’application.
* API : Représente une API qui n’est pas hébergée avec serveur d’identité.
  * L’application est configurée pour avoir une étendue par défaut est le nom de l’application.

### <a name="configuration-through-appsettings"></a>Configuration via AppSettings
Nous pouvons configurer les applications via notre système de configuration en les ajoutant à la liste des Clients ou des ressources, respectivement. 

Lors de la configuration des clients, nous pouvons configurer la `redirect_uri` et `post_logout_redirect_uri` comme indiqué ci-dessous :
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

Lors de la configuration des ressources, nous pouvons configurer les étendues pour la ressource comme indiqué ci-dessous :
```json
"IdentityServer": {
  "Resources": {
    "MyExternalApi": {
      "Profile": "API",
      "Scopes": "a b c",
    }
  }
}
```

### <a name="configuration-through-code"></a>Configuration via le code
Nous pouvons également configurer les clients et les ressources dans le code à l’aide d’une surcharge de AddApiAuthorization qui effectue une action pour configurer les options.
```csharp
AddApiAuthorization<ApplicationUser, ApplicationDbContext>(options =>
{
    options.Clients.AddSPA(
        "My SPA",
        spa => spa.WithRedirectUri("http://www.example.com/authentication/login-callback")
            .WithLogoutRedirectUri("http://www.example.com/authentication/logout-callback"));

    options.ApiResources.AddApiResource("MyExternalApi", resource => resource.WithScopes("a", "b", "c"));
});
```
