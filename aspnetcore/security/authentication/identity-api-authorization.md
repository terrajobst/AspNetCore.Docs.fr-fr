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
# <a name="authentication-and-authorization-for-spas"></a><span data-ttu-id="fbe55-103">Authentification et autorisation pour les applications SPA</span><span class="sxs-lookup"><span data-stu-id="fbe55-103">Authentication and authorization for SPAs</span></span>

<span data-ttu-id="fbe55-104">Dans ASP.NET 3.0, nous vous présentons la prise en charge pour l’authentification dans les applications à page unique à l’aide de notre nouvelle prise en charge pour l’autorisation de l’API.</span><span class="sxs-lookup"><span data-stu-id="fbe55-104">In ASP.NET 3.0 we are introducing support for authentication in single page applications using our new support for API authorization.</span></span> <span data-ttu-id="fbe55-105">Cette prise en charge est basé sur une combinaison de ASP.NET Core Identity pour l’authentification et le stockage des utilisateurs et le serveur d’identité pour l’implémentation Open ID Connect.</span><span class="sxs-lookup"><span data-stu-id="fbe55-105">This support is based on a combination of ASP.NET Core Identity for authenticating and storing users and Identity Server for implementing Open ID Connect.</span></span>

<span data-ttu-id="fbe55-106">Nous avons ajouté un nouveau paramètre d’authentification à notre Angular et les modèles de React est similaire au paramètre d’authentification dans nos modèles de pages mvc et razor avec valeurs autorisées « None » et « Individuel ».</span><span class="sxs-lookup"><span data-stu-id="fbe55-106">We have added a new authentication parameter to our Angular and React templates that is similar to the authentication parameter in our mvc and razor pages templates with allowed values 'None' and 'Individual'.</span></span>

## <a name="create-an-angular-app-with-api-authorization-support"></a><span data-ttu-id="fbe55-107">Créer une application Angular avec prise en charge des API d’autorisation</span><span class="sxs-lookup"><span data-stu-id="fbe55-107">Create an Angular app with API authorization support</span></span>

<span data-ttu-id="fbe55-108">Pour créer une nouvelle application Angular avec prise en charge pour l’authentification et autorisation des utilisateurs, ouvrez une invite de commandes et exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="fbe55-108">To create a new Angular app with support for authentication and authorization of users, open a command shell and run the following command:</span></span>

```console
dotnet new angular -o <output_directory_name> -au Individual
```

<span data-ttu-id="fbe55-109">La commande précédente crée une application ASP.NET Core avec un *ClientApp* répertoire contenant l’application Angular.</span><span class="sxs-lookup"><span data-stu-id="fbe55-109">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the Angular app.</span></span>

## <a name="create-a-react-app-with-api-authorization-support"></a><span data-ttu-id="fbe55-110">Créer une application de React avec prise en charge des API d’autorisation</span><span class="sxs-lookup"><span data-stu-id="fbe55-110">Create a React app with API authorization support</span></span>

<span data-ttu-id="fbe55-111">Pour créer une nouvelle application React avec prise en charge pour l’authentification et autorisation des utilisateurs, ouvrez une invite de commandes et exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="fbe55-111">To create a new React app with support for authentication and authorization of users, open a command shell and run the following command:</span></span>

```console
dotnet new react -o <output_directory_name> -au Individual
```

<span data-ttu-id="fbe55-112">La commande précédente crée une application ASP.NET Core avec un *ClientApp* répertoire contenant l’application React.</span><span class="sxs-lookup"><span data-stu-id="fbe55-112">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the React app.</span></span>

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a><span data-ttu-id="fbe55-113">Description générale des composants de l’application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fbe55-113">General description of the ASP.NET Core components of the app</span></span>

<span data-ttu-id="fbe55-114">Il existe plusieurs ajouts au projet lorsque nous incluons la prise en charge pour l’authentification :</span><span class="sxs-lookup"><span data-stu-id="fbe55-114">There are several additions to the project when we include support for authentication:</span></span>

### <a name="startup-class"></a><span data-ttu-id="fbe55-115">Classe de démarrage</span><span class="sxs-lookup"><span data-stu-id="fbe55-115">Startup class</span></span>

<span data-ttu-id="fbe55-116">Si nous examinons le code dans la classe de démarrage ci-dessous nous pouvons le constater les inclusions suivantes :</span><span class="sxs-lookup"><span data-stu-id="fbe55-116">If we look at the code in the Startup class below we can appreciate the following inclusions:</span></span>
* <span data-ttu-id="fbe55-117">À l’intérieur de `public void ConfigureServices(IServiceCollection services)`:</span><span class="sxs-lookup"><span data-stu-id="fbe55-117">Inside `public void ConfigureServices(IServiceCollection services)`:</span></span>
  * <span data-ttu-id="fbe55-118">Identité avec l’interface utilisateur par défaut.</span><span class="sxs-lookup"><span data-stu-id="fbe55-118">Identity with the default UI.</span></span>
    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
      options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
      .AddDefaultUI(UIFramework.Bootstrap4)
      .AddEntityFrameworkStores<ApplicationDbContext>();
    ```
  * <span data-ttu-id="fbe55-119">Serveur d’identité avec une méthode d’assistance de AddApiAuthorization supplémentaire que paramétrages certains par défaut des Conventions de ASP.NET sur le serveur d’identité.</span><span class="sxs-lookup"><span data-stu-id="fbe55-119">Identity Server with an additional AddApiAuthorization helper method that setups some default ASP.NET Conventions on top of Identity Server.</span></span>
    ```csharp
    services.AddIdentityServer()
      .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```
  * <span data-ttu-id="fbe55-120">Authentification avec une méthode d’assistance AddIdentityServerJwt supplémentaire qui configure l’application pour valider les jetons Jwt générés par le serveur d’identité.</span><span class="sxs-lookup"><span data-stu-id="fbe55-120">Authentication with an additional AddIdentityServerJwt helper method that configures the application to validate Jwt tokens produced by Identity Server.</span></span> 
    ```csharp
    services.AddAuthentication()
      .AddIdentityServerJwt();
    ```
* <span data-ttu-id="fbe55-121">À l’intérieur de `public void Configure(IApplicationBuilder app)`:</span><span class="sxs-lookup"><span data-stu-id="fbe55-121">Inside `public void Configure(IApplicationBuilder app)`:</span></span>
  * <span data-ttu-id="fbe55-122">Le middleware d’authentification qui est chargé de valider les informations d’identification dans la demande entrante et de définition de l’utilisateur sur le contexte de requête.</span><span class="sxs-lookup"><span data-stu-id="fbe55-122">The authentication middleware that is responsible for validating the credentials in the incoming request and setting the user on the request context.</span></span>
    ```csharp
    app.UseAuthentication();
    ```
  * <span data-ttu-id="fbe55-123">Le middleware de serveur d’identité qui expose les points de terminaison Open ID Connect.</span><span class="sxs-lookup"><span data-stu-id="fbe55-123">The identity server middleware that exposes the Open ID Connect endpoints.</span></span>
    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="fbe55-124">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="fbe55-124">AddApiAuthorization</span></span> 
<span data-ttu-id="fbe55-125">Cette méthode d’assistance configure Identity Server pour utiliser notre configuration prise en charge.</span><span class="sxs-lookup"><span data-stu-id="fbe55-125">This helper method configures Identity Server to use our supported configuration.</span></span> <span data-ttu-id="fbe55-126">Serveur d’identité est une infrastructure très puissante et extensible pour la gestion des problèmes de sécurité d’application, mais en même temps qui expose une grande complexité que nous n’avez pas besoin de connaître pour les scénarios les plus courants, par conséquent, nous choisissons un ensemble de conventions et options de configuration pour vous que nous considérons constituent un bon point de départ.</span><span class="sxs-lookup"><span data-stu-id="fbe55-126">Identity Server is a very powerful and extensible framework for handling application security concerns but at the same time that exposes a lot of complexity that we don't need to know about for the most common scenarios, so we choose a set of conventions and configuration options for you that we consider are a good starting point.</span></span> <span data-ttu-id="fbe55-127">Une fois que l’authentification de vos besoins évoluent toute la puissance de serveur d’identité est toujours disponible pour vous afin de pouvoir personnaliser selon vos besoins.</span><span class="sxs-lookup"><span data-stu-id="fbe55-127">Once your authentication needs change the full power of Identity Server is still available to you so you can customize it to suit your needs.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="fbe55-128">AddIdentityServerJwt</span><span class="sxs-lookup"><span data-stu-id="fbe55-128">AddIdentityServerJwt</span></span>
<span data-ttu-id="fbe55-129">Cette méthode d’assistance configure un jeu de stratégie pour l’application en tant que le Gestionnaire d’authentification par défaut.</span><span class="sxs-lookup"><span data-stu-id="fbe55-129">This helper method configures a policy scheme for the application as the default authentication handler.</span></span> <span data-ttu-id="fbe55-130">La stratégie est configurée pour l’identité permettent de gérer toutes les demandes qui mènent à n’importe quel sous-tracé dans l’espace de l’url d’identité « / identité » et de laisser le JwtBearerHandler gérer toutes les autres demandes.</span><span class="sxs-lookup"><span data-stu-id="fbe55-130">The policy is configured to let identity handle all the requests that go to any subpath in the Identity url space "/Identity" and to let the JwtBearerHandler handle all other requests.</span></span>
<span data-ttu-id="fbe55-131">Plus cette méthode inscrit une `<<ApplicationName>>API` avec identity server avec une étendue par défaut de ressource de l’Api `<<ApplicationName>>API` et configure l’intergiciel de jeton du porteur JWT pour valider les jetons émis par le serveur d’identité pour l’application.</span><span class="sxs-lookup"><span data-stu-id="fbe55-131">Addionally this method registers an `<<ApplicationName>>API` Api resource with identity server with a default scope of `<<ApplicationName>>API` and configures the JWT Bearer token middleware to validate tokens issued by Identity Server for the application.</span></span>

### <a name="sampledatacontroller"></a><span data-ttu-id="fbe55-132">SampleDataController</span><span class="sxs-lookup"><span data-stu-id="fbe55-132">SampleDataController</span></span>
<span data-ttu-id="fbe55-133">Si nous examinons le fichier Controllers\SampleDataController.cs, nous pouvons observer le `[Authorize]` basée sur les attributs appliqués à la classe qui indique que l’utilisateur doit être autorisé sur la stratégie par défaut pour accéder à la ressource.</span><span class="sxs-lookup"><span data-stu-id="fbe55-133">If we look at the file Controllers\SampleDataController.cs we can observe the `[Authorize]` attribute applied to the class that indicates that the user needs to be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="fbe55-134">La stratégie d’autorisation par défaut se trouve être configuré pour utiliser le schéma d’authentification par défaut qui est configuré par `AddIdentityServerJwt` au jeu de stratégie que nous avons mentionnés ci-dessus, ce qui le gestionnaire JwtBearer configuré par cette méthode d’assistance le gestionnaire par défaut requêtes à l’application.</span><span class="sxs-lookup"><span data-stu-id="fbe55-134">The default authorization policy happens to be configured to use the default authentication scheme which is set up by `AddIdentityServerJwt` to the policy scheme that we mentioned above, making the JwtBearer handler configured by such helper method the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="fbe55-135">ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="fbe55-135">ApplicationDbContext</span></span>
<span data-ttu-id="fbe55-136">Si nous examinons le fichier dans Data\ApplicationDbContext.cs, nous pouvons voir le même DbContext que nous utilisons dans identité avec l’exception qu’il étend ApiAuthorizationDbContext (une classe plus dérivée à partir de l’IdentityDbContext) pour inclure le schéma pour le serveur d’identité.</span><span class="sxs-lookup"><span data-stu-id="fbe55-136">If we look at the file in Data\ApplicationDbContext.cs we can see the same DbContext we use in identity with the exception that it extends ApiAuthorizationDbContext (a more derived class from IdentityDbContext) to include the schema for Identity Server.</span></span>
<span data-ttu-id="fbe55-137">Si nous voulons que le contrôle total sur le schéma de base de données nous pouvons simplement héritent d’une des classes DbContext d’identité disponibles et configurer le contexte pour inclure le schéma de l’identité en appelant `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` sur la `OnModelCreating` (méthode).</span><span class="sxs-lookup"><span data-stu-id="fbe55-137">If we want full control of the database schema we can simply inherit from one of the available Identity DbContext classes and configure the context to include the identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` on the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="fbe55-138">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="fbe55-138">OidcConfigurationController</span></span>
<span data-ttu-id="fbe55-139">Si nous examinons le fichier Controllers\OidcConfigurationController.cs le point de terminaison, nous pouvons voir que nous consultons pour traiter les paramètres OIDC que le client doit utiliser.</span><span class="sxs-lookup"><span data-stu-id="fbe55-139">If we look at the file Controllers\OidcConfigurationController.cs we can see the endpoint that we stand-up to serve the OIDC parameters that the client needs to use.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="fbe55-140">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="fbe55-140">appsettings.json</span></span>
<span data-ttu-id="fbe55-141">Si nous examinons le fichier appsettings.json sur la racine du projet, nous pouvons voir une nouvelle `IdentityServer` section qui décrit la liste des configuré les clients et nous pouvons voir qu’il existe un seul client.</span><span class="sxs-lookup"><span data-stu-id="fbe55-141">If we look at the appsettings.json file on the root of the project, we can see a new `IdentityServer` section that describes the list of configured clients and we can see that there is a single client.</span></span> <span data-ttu-id="fbe55-142">Le nom du client correspond au nom de l’application et est mappé par convention pour le paramètre d’ID client oAuth.</span><span class="sxs-lookup"><span data-stu-id="fbe55-142">The name of the client corresponds to the name of the application and is mapped by convention to the oAuth ClientId parameter.</span></span> <span data-ttu-id="fbe55-143">Le profil indique le type d’application que nous allons configurer et nous l’utilisons en interne pour les conventions de lecteur qui simplifient le processus de configuration pour le serveur.</span><span class="sxs-lookup"><span data-stu-id="fbe55-143">The profile indicates what type of application we are configuring, and we use it internally to drive conventions that simplify the configuration process for the server.</span></span> <span data-ttu-id="fbe55-144">Il existe plusieurs profils disponibles sont expliqués dans la section ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="fbe55-144">There are several profiles available explained in the section below.</span></span>

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a><span data-ttu-id="fbe55-145">appsettings.Development.json</span><span class="sxs-lookup"><span data-stu-id="fbe55-145">appsettings.Development.json</span></span>
<span data-ttu-id="fbe55-146">Si nous examinons l’appsettings. Fichier de Development.JSON sur la racine du projet, nous pouvons voir une nouvelle `IdentityServer` section qui décrit la clé que nous utilisons pour signer les jetons.</span><span class="sxs-lookup"><span data-stu-id="fbe55-146">If we look at the appsettings.Development.json file on the root of the project, we can see a new `IdentityServer` section that describes the key we are using to sign tokens.</span></span> <span data-ttu-id="fbe55-147">Lors du déploiement en production une clé doit être configuré et déployé en même temps que l’application, comme expliqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="fbe55-147">When deploying to production a key needs to be provisioned and deployed alongside the application as explained below.</span></span>

```json
  "IdentityServer": {
    "Key": {
      "Type": "Development"
    }
  }
}
```

## <a name="general-description-of-the-angular-application"></a><span data-ttu-id="fbe55-148">Description générale de l’application Angular</span><span class="sxs-lookup"><span data-stu-id="fbe55-148">General description of the Angular application</span></span>
<span data-ttu-id="fbe55-149">La prise en charge pour l’authentification et l’autorisation d’API dans le modèle Angular se trouve dans son propre module Angular.</span><span class="sxs-lookup"><span data-stu-id="fbe55-149">The support for authentication and API authorization in the Angular template lives in its own Angular module.</span></span> <span data-ttu-id="fbe55-150">Sous l’autorisation ClientApp\src\api et il sont composés des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="fbe55-150">Under ClientApp\src\api-authorization and it is composed of the following elements:</span></span>
* <span data-ttu-id="fbe55-151">3 composants :</span><span class="sxs-lookup"><span data-stu-id="fbe55-151">3 Components:</span></span>
  * <span data-ttu-id="fbe55-152">Composant de la connexion : Gère le flux de connexion pour l’application.</span><span class="sxs-lookup"><span data-stu-id="fbe55-152">Login component: Handles the login flow for the app.</span></span>
  * <span data-ttu-id="fbe55-153">Composant de déconnexion : Gère le flux de déconnexion de l’application.</span><span class="sxs-lookup"><span data-stu-id="fbe55-153">Logout component: Handles the logout flow for the app.</span></span>
  * <span data-ttu-id="fbe55-154">Composant de menu de connexion : Un widget qui affiche l’utilisateur authentifié actuel avec des liens pour gérer le profil utilisateur et se déconnecter ou de liens pour se connecter ou inscrire lorsque l’utilisateur n’est pas authentifié.</span><span class="sxs-lookup"><span data-stu-id="fbe55-154">Login menu component: A widget that displays the current authenticated user with links to manage the user profile and log out or links to log in or register when the user is not authenticated.</span></span>
* <span data-ttu-id="fbe55-155">Un garde itinéraire `AuthorizeGuard` qui peuvent être ajoutés à des itinéraires et nécessite un utilisateur de s’authentifier avant de visiter l’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="fbe55-155">A route guard `AuthorizeGuard` that can be added to routes and requires a user to be authenticated before visiting the route.</span></span>
* <span data-ttu-id="fbe55-156">Un intercepteur http `AuthorizeInterceptor` qui attache le jeton d’accès à des requêtes HTTP sortantes ciblant l’API lorsque l’utilisateur est authentifié.</span><span class="sxs-lookup"><span data-stu-id="fbe55-156">An http interceptor `AuthorizeInterceptor` that attaches the access token to outgoing HTTP requests targeting the API when the user is authenticated.</span></span>
* <span data-ttu-id="fbe55-157">Un service `AuthorizeService` qui gère les détails de niveau inférieur du processus d’authentification et expose des informations sur l’utilisateur authentifié au reste de l’application pour la consommation.</span><span class="sxs-lookup"><span data-stu-id="fbe55-157">A service `AuthorizeService` that handles the lower level details of the authentication process and exposes information about the authenticated user to the rest of the application for consumption.</span></span>
* <span data-ttu-id="fbe55-158">Un module angular qui définit les itinéraires associés avec les parties de l’authentification de l’application et expose le composant de menu de connexion, l’intercepteur, le module de protection et le service pour la consommation du reste de l’application.</span><span class="sxs-lookup"><span data-stu-id="fbe55-158">An angular module that defines routes associated with the authentication parts of the application and exposes the login menu component, the interceptor, the guard and the service for consumption from the rest of the application.</span></span>

## <a name="general-description-of-the-react-application"></a><span data-ttu-id="fbe55-159">Description générale de l’application React</span><span class="sxs-lookup"><span data-stu-id="fbe55-159">General description of the React application</span></span>
<span data-ttu-id="fbe55-160">La prise en charge pour l’authentification et l’autorisation d’API dans la vie de modèle React sous ClientApp\src\components\api-authorization\ et il est composé des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="fbe55-160">The support for authentication and API authorization in the React template lives under ClientApp\src\components\api-authorization\ and it is composed of the following elements:</span></span>
* <span data-ttu-id="fbe55-161">4 composants :</span><span class="sxs-lookup"><span data-stu-id="fbe55-161">4 Components:</span></span>
  * <span data-ttu-id="fbe55-162">Composant de la connexion : Gère le flux de connexion pour l’application.</span><span class="sxs-lookup"><span data-stu-id="fbe55-162">Login component: Handles the login flow for the app.</span></span>
  * <span data-ttu-id="fbe55-163">Composant de déconnexion : Gère le flux de déconnexion de l’application.</span><span class="sxs-lookup"><span data-stu-id="fbe55-163">Logout component: Handles the logout flow for the app.</span></span>
  * <span data-ttu-id="fbe55-164">Composant de menu de connexion : Un widget qui affiche l’utilisateur authentifié actuel avec des liens pour gérer le profil utilisateur et se déconnecter ou de liens pour se connecter ou inscrire lorsque l’utilisateur n’est pas authentifié.</span><span class="sxs-lookup"><span data-stu-id="fbe55-164">Login menu component: A widget that displays the current authenticated user with links to manage the user profile and log out or links to log in or register when the user is not authenticated.</span></span>
  * <span data-ttu-id="fbe55-165">AuthorizeRoute : Un composant de routage qui oblige l’utilisateur de s’authentifier avant le composant indiqué dans le paramètre de composant de rendu.</span><span class="sxs-lookup"><span data-stu-id="fbe55-165">AuthorizeRoute: A route component that requires a user to be authenticated before rendering the component indicated in the Component parameter.</span></span>
  * <span data-ttu-id="fbe55-166">Un exporté `authService` instance de classe `AuthorizeService` qui gère les détails de niveau inférieur du processus d’authentification et expose des informations sur l’utilisateur authentifié au reste de l’application pour la consommation.</span><span class="sxs-lookup"><span data-stu-id="fbe55-166">An exported `authService` instance of class `AuthorizeService` that handles the lower level details of the authentication process and exposes information about the authenticated user to the rest of the application for consumption.</span></span>

<span data-ttu-id="fbe55-167">Maintenant que nous avons vu les principaux composants de la solution, nous pouvons prendre une apparence spécifique à des scénarios individuels pour l’application :</span><span class="sxs-lookup"><span data-stu-id="fbe55-167">Now that we've seen the main components of the solution, we can take a specific look at individual scenarios for the application:</span></span>

## <a name="requiring-authorization-on-a-new-api"></a><span data-ttu-id="fbe55-168">Nécessitant une autorisation sur une nouvelle API</span><span class="sxs-lookup"><span data-stu-id="fbe55-168">Requiring authorization on a new API</span></span>
<span data-ttu-id="fbe55-169">Le système est configuré en dehors de la zone pour le rendre facile à exiger l’autorisation de nouvelles API.</span><span class="sxs-lookup"><span data-stu-id="fbe55-169">The system is configured out of the box to make it trivial to require authorization for new APIs.</span></span> <span data-ttu-id="fbe55-170">Pour ce faire, créez simplement un nouveau contrôleur et ajouter la `[Authorize]` attribut à la classe de contrôleur ou à des actions dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="fbe55-170">In order to do so, simply create a new controller and add the `[Authorize]` attribute to the controller class or to any action within the controller.</span></span>

## <a name="protecting-a-client-side-route-angular"></a><span data-ttu-id="fbe55-171">Protection d’un itinéraire côté client (Angular)</span><span class="sxs-lookup"><span data-stu-id="fbe55-171">Protecting a client-side route (Angular)</span></span>
<span data-ttu-id="fbe55-172">Protéger un itinéraire de côté client s’effectue en ajoutant de la garde authorize à la liste des gardes à exécuter lors de la configuration d’un itinéraire.</span><span class="sxs-lookup"><span data-stu-id="fbe55-172">Protecting a client side route is done by adding the authorize guard to the list of guards to run when configuring a route.</span></span> <span data-ttu-id="fbe55-173">Par exemple, vous pouvez voir comment l’itinéraire de récupérer des données est configuré dans le module d’angular principal d’application :</span><span class="sxs-lookup"><span data-stu-id="fbe55-173">As an example you can see how the fetch-data route is configured within the main app angular module:</span></span>

```ts
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

<span data-ttu-id="fbe55-174">Il est important de mentionner que protégeant un itinéraire ne protège pas le point de terminaison réel (ce qui nécessite toujours un `[Authorize]` attribut appliqué à ce dernier), mais qu’elle empêche uniquement l’utilisateur de naviguer vers l’itinéraire du côté client donné quand il n’est pas authentifiée.</span><span class="sxs-lookup"><span data-stu-id="fbe55-174">It is important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client side route when it is not authenticated.</span></span>

## <a name="authenticate-api-requests-angular"></a><span data-ttu-id="fbe55-175">Authentifier les demandes d’API (Angular)</span><span class="sxs-lookup"><span data-stu-id="fbe55-175">Authenticate API requests (Angular)</span></span>

<span data-ttu-id="fbe55-176">Authentification des demandes aux API hébergé côté que l’application est effectuée automatiquement via l’utilisation de l’intercepteur de client HTTP défini par l’application.</span><span class="sxs-lookup"><span data-stu-id="fbe55-176">Authenticating requests to APIs hosted along side the application is done automatically through the use of the HTTP client interceptor defined by the application.</span></span>

## <a name="protect-a-client-side-route-react"></a><span data-ttu-id="fbe55-177">Protéger un itinéraire côté client (React)</span><span class="sxs-lookup"><span data-stu-id="fbe55-177">Protect a client-side route (React)</span></span>

<span data-ttu-id="fbe55-178">Protection d’un itinéraire de côté client est effectuée à l’aide du composant AuthorizeRoute au lieu du composant de routage simple.</span><span class="sxs-lookup"><span data-stu-id="fbe55-178">Protecting a client side route is done by using the AuthorizeRoute component instead of the plain Route component.</span></span> <span data-ttu-id="fbe55-179">Par exemple, vous pouvez voir comment l’itinéraire de récupérer des données est configuré dans le composant d’application :</span><span class="sxs-lookup"><span data-stu-id="fbe55-179">As an example you can see how the fetch-data route is configured within the App component:</span></span>

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

<span data-ttu-id="fbe55-180">Il est important de mentionner que protégeant un itinéraire ne protège pas le point de terminaison réel (ce qui nécessite toujours un `[Authorize]` attribut appliqué à ce dernier), mais qu’elle empêche uniquement l’utilisateur de naviguer vers l’itinéraire du côté client donné quand il n’est pas authentifiée.</span><span class="sxs-lookup"><span data-stu-id="fbe55-180">It is important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client side route when it is not authenticated.</span></span>

## <a name="authenticate-api-requests-react"></a><span data-ttu-id="fbe55-181">Authentifier les demandes d’API (React)</span><span class="sxs-lookup"><span data-stu-id="fbe55-181">Authenticate API requests (React)</span></span>

<span data-ttu-id="fbe55-182">L’authentification des demandes avec react s’effectue en important d’abord le `authService` instance à partir de la `AuthorizeService` puis récupérer le jeton d’accès à partir de l’authService et attachement à la demande, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="fbe55-182">Authenticating requests with react is done by first importing the `authService` instance from the `AuthorizeService` and then retrieving the access token from the authService and attaching it to the request as shown below.</span></span> <span data-ttu-id="fbe55-183">Dans les composants de react est généralement cela dans la méthode de cycle de vie componentDidMount ou en tant que le résultat à partir d’une interaction utilisateur.</span><span class="sxs-lookup"><span data-stu-id="fbe55-183">In react components this is typically done in the componentDidMount lifecycle method or as the result from some user interaction.</span></span>

### <a name="import-the-authservice-into-your-component"></a><span data-ttu-id="fbe55-184">Importer l’authService dans votre composant</span><span class="sxs-lookup"><span data-stu-id="fbe55-184">Import the authService into your component</span></span>

```js
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a><span data-ttu-id="fbe55-185">Récupérer et joignez le jeton d’accès à la réponse</span><span class="sxs-lookup"><span data-stu-id="fbe55-185">Retrieve and attach the access token to the response</span></span>

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

## <a name="deploy-into-production"></a><span data-ttu-id="fbe55-186">Déployer en production</span><span class="sxs-lookup"><span data-stu-id="fbe55-186">Deploy into production</span></span>

<span data-ttu-id="fbe55-187">Pour déployer l’application en production, nous devons configurer plusieurs ressources :</span><span class="sxs-lookup"><span data-stu-id="fbe55-187">In order to deploy the application into production we need to provision several resources:</span></span>
* <span data-ttu-id="fbe55-188">Accorde à une base de données pour stocker les comptes d’identité utilisateur et le serveur d’identité.</span><span class="sxs-lookup"><span data-stu-id="fbe55-188">A database to store the Identity user accounts and the identity server grants.</span></span>
* <span data-ttu-id="fbe55-189">Un certificat de production à utiliser pour la signature de jetons.</span><span class="sxs-lookup"><span data-stu-id="fbe55-189">A production certificate to use for signing tokens.</span></span>
  * <span data-ttu-id="fbe55-190">Il n’y a aucune exigence particulière pour ce certificat ; Il peut être un certificat auto-signé ou un certificat configuré via une autorité de certification.</span><span class="sxs-lookup"><span data-stu-id="fbe55-190">There are no specific requirements for this certificate; it can be a self-signed certificate or a certificate provisioned through a CA authority.</span></span>
  * <span data-ttu-id="fbe55-191">Il peut être généré par le biais des outils standards tels que powershell ou openssl.</span><span class="sxs-lookup"><span data-stu-id="fbe55-191">It can be generated through standard tools like powershell or openssl.</span></span>
  * <span data-ttu-id="fbe55-192">Il peut être installé dans le magasin de certificats sur les ordinateurs cibles ou déployé dans un fichier pfx avec un mot de passe fort.</span><span class="sxs-lookup"><span data-stu-id="fbe55-192">It can be installed into the certificate store on the target machines or deployed as a pfx file with a strong password.</span></span>

### <a name="example-deploy-into-azure-websites"></a><span data-ttu-id="fbe55-193">Exemple : Déployer des sites Web Azure</span><span class="sxs-lookup"><span data-stu-id="fbe55-193">Example: Deploy into Azure Websites</span></span>

<span data-ttu-id="fbe55-194">Dans cette section, nous allons déployer l’application sur sites Web Azure à l’aide d’un certificat stocké dans le magasin de certificats.</span><span class="sxs-lookup"><span data-stu-id="fbe55-194">In this section we are going to deploy the application to Azure websites using a certificate stored in the certificate store.</span></span> <span data-ttu-id="fbe55-195">Nous devons modifier l’application pour charger un certificat du magasin de certificats.</span><span class="sxs-lookup"><span data-stu-id="fbe55-195">We need to modify the application to load a certicate from the certificate store.</span></span> <span data-ttu-id="fbe55-196">Pour ce faire, notre plan app service doit être au moins du niveau standard lorsque nous configurons dans une étape ultérieure.</span><span class="sxs-lookup"><span data-stu-id="fbe55-196">To do so, our app service plan needs to be at least on the standard tier when we configure in a later step.</span></span> <span data-ttu-id="fbe55-197">Dans notre application, nous devons simplement modifier la section IdentityServer sur appsettings.json pour inclure les détails de la clé :</span><span class="sxs-lookup"><span data-stu-id="fbe55-197">In our application we simply need to modify the IdentityServer section on appsettings.json to include the key details:</span></span>
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
* <span data-ttu-id="fbe55-198">La propriété nom de certificat correspond à l’objet du certificat unique.</span><span class="sxs-lookup"><span data-stu-id="fbe55-198">The name property on certificate corresponds with the distinguished subject for the certificate.</span></span>
* <span data-ttu-id="fbe55-199">L’emplacement du magasin représente l’emplacement de charger le certificat à partir de (CurrentUrser ou LocalMachine).</span><span class="sxs-lookup"><span data-stu-id="fbe55-199">The store location represents where to load the certificate from (CurrentUrser or LocalMachine).</span></span>
* <span data-ttu-id="fbe55-200">Le nom du magasin représente le nom du magasin de certificats dans lequel le certificat est stocké dans ce cas, qu'il pointe vers le magasin utilisateur personnel.</span><span class="sxs-lookup"><span data-stu-id="fbe55-200">The store name represents the name of the certificate store where the certificate is stored, in this case it points to the personal user store.</span></span>

<span data-ttu-id="fbe55-201">Pour déployer sur Azure Websites, déployez l’application en suivant les étapes décrites dans [déployer l’application sur Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) pour créer les ressources Azure nécessaires et de déployer l’application en production.</span><span class="sxs-lookup"><span data-stu-id="fbe55-201">To deploy to Azure Websites, deploy the app following the steps in [Deploy the app to Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) to create the necessary Azure resources and deploy the app to production.</span></span>

<span data-ttu-id="fbe55-202">Après cette opération, l’application est déployée dans Azure, mais n’est pas encore complètement fonctionnelle que nous devons encore configurer le certificat à utiliser par l’application.</span><span class="sxs-lookup"><span data-stu-id="fbe55-202">After doing this, the application is deployed into Azure but is not yet completely functional as we still need to setup the certificate to be used by the application.</span></span> <span data-ttu-id="fbe55-203">Pour ce faire, nous devons disposer de l’empreinte numérique du certificat que nous allons utiliser et suivez les étapes décrites dans [charger vos certificats](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).</span><span class="sxs-lookup"><span data-stu-id="fbe55-203">To do so, we need to have the thumbprint for the certificate we are going to use and follow the steps described in [Load your certificates](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).</span></span>

<span data-ttu-id="fbe55-204">Ces étapes mentionnent SSL, mais il existe une section « Certificats privés » sur le portail où nous pouvons télécharger notre certificat mis en service à utiliser avec notre application.</span><span class="sxs-lookup"><span data-stu-id="fbe55-204">While these steps mention SSL, there is a "Private certificates" section on the portal where we can upload our provisioned certificate to use with our app.</span></span>

<span data-ttu-id="fbe55-205">Après cette étape, nous devrions pouvoir redémarrer notre application, et il doit être complètement fonctionnel.</span><span class="sxs-lookup"><span data-stu-id="fbe55-205">After this step, we should be able to restart our application and it should be completely functional.</span></span>

## <a name="other-configuration-options"></a><span data-ttu-id="fbe55-206">Autres options de configuration</span><span class="sxs-lookup"><span data-stu-id="fbe55-206">Other configuration options</span></span>
<span data-ttu-id="fbe55-207">La prise en charge pour l’autorisation de l’API s’appuie sur Identity Server avec un ensemble de conventions, les valeurs par défaut et les améliorations apportées à simplifier l’expérience des Applications à Page unique.</span><span class="sxs-lookup"><span data-stu-id="fbe55-207">Our support for API authorization builds on top of Identity Server with a set of conventions, default values and enhancements to simplify the experience for Single Page Applications.</span></span> <span data-ttu-id="fbe55-208">Inutile de dire que toute la puissance de serveur d’identité est disponible dans les coulisses si votre scénario ne couvrent pas les intégrations que nous offrons.</span><span class="sxs-lookup"><span data-stu-id="fbe55-208">Needless to say, the full power of Identity Server is available behind the scenes if the integrations that we offer don't cover your scenario.</span></span> <span data-ttu-id="fbe55-209">La prise en charge se concentre sur ce que nous appelons « internes » des applications, où toutes les applications sont créées et déployées par notre organisation.</span><span class="sxs-lookup"><span data-stu-id="fbe55-209">Our support is focused on what we call "first-party" applications, where all the applications are created and deployed by our organization.</span></span> <span data-ttu-id="fbe55-210">Par conséquent nous n’offrent pas prise en charge des éléments tels que le consentement ou la fédération.</span><span class="sxs-lookup"><span data-stu-id="fbe55-210">As such we don't offer support for things like consent or federation.</span></span> <span data-ttu-id="fbe55-211">Pour ces scénarios, notre recommandation est Identity Server et suivez leur documentation.</span><span class="sxs-lookup"><span data-stu-id="fbe55-211">For those scenarios our recommendation is to use Identity Server and follow their documentation.</span></span>

### <a name="application-profiles"></a><span data-ttu-id="fbe55-212">Profils d’application</span><span class="sxs-lookup"><span data-stu-id="fbe55-212">Application profiles</span></span>
<span data-ttu-id="fbe55-213">Profils d’application sont des configurations prédéfinies pour les applications qui définissent leurs paramètres.</span><span class="sxs-lookup"><span data-stu-id="fbe55-213">Application profiles are predefined configurations for applications that further define their parameters.</span></span> <span data-ttu-id="fbe55-214">À ce stade, nous prenons en charge deux profils :</span><span class="sxs-lookup"><span data-stu-id="fbe55-214">At this time we support two profiles:</span></span>
* <span data-ttu-id="fbe55-215">IdentityServerSPA: Représente une application à page unique hébergée en même temps que le serveur d’identité comme une seule unité.</span><span class="sxs-lookup"><span data-stu-id="fbe55-215">IdentityServerSPA: Represents a single page application hosted alongside Identity Server as a single unit.</span></span>
  * <span data-ttu-id="fbe55-216">Par défaut est le paramètre redirect_uri `/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="fbe55-216">The redirect_uri defaults to `/authentication/login-callback`.</span></span>
  * <span data-ttu-id="fbe55-217">Par défaut est le post_logout_redirect_uri `/authentication/logout-callback`.</span><span class="sxs-lookup"><span data-stu-id="fbe55-217">The post_logout_redirect_uri defaults to `/authentication/logout-callback`.</span></span>
  * <span data-ttu-id="fbe55-218">Le jeu d’étendues inclut le `openid`, `profile`et chaque étendue définie pour les API dans l’application.</span><span class="sxs-lookup"><span data-stu-id="fbe55-218">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the application.</span></span>
  * <span data-ttu-id="fbe55-219">Est l’ensemble des types de réponse autorisés OIDC `id_token token` ou chacun d’eux individuellement (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="fbe55-219">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="fbe55-220">Le mode de réponse autorisés est `fragment`.</span><span class="sxs-lookup"><span data-stu-id="fbe55-220">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="fbe55-221">SPA : Représente une application à page unique qui n’est pas hébergée avec Identity Server.</span><span class="sxs-lookup"><span data-stu-id="fbe55-221">SPA: Represents a single page application that is not hosted with Identity Server.</span></span>
  * <span data-ttu-id="fbe55-222">Le jeu d’étendues inclut le `openid`, `profile`et chaque étendue définie pour les API dans l’application.</span><span class="sxs-lookup"><span data-stu-id="fbe55-222">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the application.</span></span>
  * <span data-ttu-id="fbe55-223">Est l’ensemble des types de réponse autorisés OIDC `id_token token` ou chacun d’eux individuellement (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="fbe55-223">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="fbe55-224">Le mode de réponse autorisés est `fragment`.</span><span class="sxs-lookup"><span data-stu-id="fbe55-224">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="fbe55-225">IdentityServerJwt: Représente une API qui est hébergée en même temps qu’avec Identity Server.</span><span class="sxs-lookup"><span data-stu-id="fbe55-225">IdentityServerJwt: Represents an API that is hosted alongside with Identity Server.</span></span>
  * <span data-ttu-id="fbe55-226">L’application est configurée pour avoir une étendue par défaut est le nom de l’application.</span><span class="sxs-lookup"><span data-stu-id="fbe55-226">The application is configured to have a single scope that defaults to the application name.</span></span>
* <span data-ttu-id="fbe55-227">API : Représente une API qui n’est pas hébergée avec serveur d’identité.</span><span class="sxs-lookup"><span data-stu-id="fbe55-227">API: Represents an API that is not hosted with Identity Server.</span></span>
  * <span data-ttu-id="fbe55-228">L’application est configurée pour avoir une étendue par défaut est le nom de l’application.</span><span class="sxs-lookup"><span data-stu-id="fbe55-228">The application is configured to have a single scope that defaults to the application name.</span></span>

### <a name="configuration-through-appsettings"></a><span data-ttu-id="fbe55-229">Configuration via AppSettings</span><span class="sxs-lookup"><span data-stu-id="fbe55-229">Configuration through AppSettings</span></span>
<span data-ttu-id="fbe55-230">Nous pouvons configurer les applications via notre système de configuration en les ajoutant à la liste des Clients ou des ressources, respectivement.</span><span class="sxs-lookup"><span data-stu-id="fbe55-230">We can configure the applications through our configuration system by adding them to the list of Clients or Resources respectively.</span></span> 

<span data-ttu-id="fbe55-231">Lors de la configuration des clients, nous pouvons configurer la `redirect_uri` et `post_logout_redirect_uri` comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="fbe55-231">When configuring clients we can configure the `redirect_uri` and the `post_logout_redirect_uri` as shown below:</span></span>
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

<span data-ttu-id="fbe55-232">Lors de la configuration des ressources, nous pouvons configurer les étendues pour la ressource comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="fbe55-232">When configuring resources we can configure the scopes for the resource as shown below:</span></span>
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

### <a name="configuration-through-code"></a><span data-ttu-id="fbe55-233">Configuration via le code</span><span class="sxs-lookup"><span data-stu-id="fbe55-233">Configuration through code</span></span>
<span data-ttu-id="fbe55-234">Nous pouvons également configurer les clients et les ressources dans le code à l’aide d’une surcharge de AddApiAuthorization qui effectue une action pour configurer les options.</span><span class="sxs-lookup"><span data-stu-id="fbe55-234">We can also configure the clients and resources through code using an overload of AddApiAuthorization that takes an action to configure options.</span></span>
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
