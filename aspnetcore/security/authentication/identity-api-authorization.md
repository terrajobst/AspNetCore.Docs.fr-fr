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
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64894146"
---
# <a name="authentication-and-authorization-for-spas"></a><span data-ttu-id="8a40d-103">Authentification et autorisation pour les applications SPA</span><span class="sxs-lookup"><span data-stu-id="8a40d-103">Authentication and authorization for SPAs</span></span>

<span data-ttu-id="8a40d-104">ASP.NET Core 3.0 ou version ultérieure offre l’authentification dans les applications à Page unique (SPA) à l’aide de la prise en charge pour l’autorisation de l’API.</span><span class="sxs-lookup"><span data-stu-id="8a40d-104">ASP.NET Core 3.0 or later offers authentication in Single Page Apps (SPAs) using the support for API authorization.</span></span> <span data-ttu-id="8a40d-105">ASP.NET Core Identity pour l’authentification et le stockage des utilisateurs est combiné avec [IdentityServer](https://identityserver.io/) pour l’implémentation Open ID Connect.</span><span class="sxs-lookup"><span data-stu-id="8a40d-105">ASP.NET Core Identity for authenticating and storing users is combined with [IdentityServer](https://identityserver.io/) for implementing Open ID Connect.</span></span>

<span data-ttu-id="8a40d-106">Un paramètre d’authentification a été ajouté à la **Angular** et **réagir** est similaire au paramètre d’authentification dans les modèles de projet la **l’Application Web (Model-View-Controller)**  (MVC) et **Web Application** (Pages Razor) des modèles de projet.</span><span class="sxs-lookup"><span data-stu-id="8a40d-106">An authentication parameter was added to the **Angular** and **React** project templates that is similar to the authentication parameter in the **Web Application (Model-View-Controller)** (MVC) and **Web Application** (Razor Pages) project templates.</span></span> <span data-ttu-id="8a40d-107">Les valeurs de paramètre autorisés sont **aucun** et **individuels**.</span><span class="sxs-lookup"><span data-stu-id="8a40d-107">The allowed parameter values are **None** and **Individual**.</span></span> <span data-ttu-id="8a40d-108">Le **React.js et Redux** modèle de projet ne prend pas en charge le paramètre d’authentification pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="8a40d-108">The **React.js and Redux** project template doesn't support the authentication parameter at this time.</span></span>

## <a name="create-an-app-with-api-authorization-support"></a><span data-ttu-id="8a40d-109">Créer une application avec prise en charge des API d’autorisation</span><span class="sxs-lookup"><span data-stu-id="8a40d-109">Create an app with API authorization support</span></span>

<span data-ttu-id="8a40d-110">Autorisation et authentification utilisateur utilisable avec Angular et React applications à page unique.</span><span class="sxs-lookup"><span data-stu-id="8a40d-110">User authentication and authorization can be used with both Angular and React SPAs.</span></span> <span data-ttu-id="8a40d-111">Ouvrez une invite de commandes et exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="8a40d-111">Open a command shell, and run the following command:</span></span>

<span data-ttu-id="8a40d-112">**Angular**:</span><span class="sxs-lookup"><span data-stu-id="8a40d-112">**Angular**:</span></span>

```console
dotnet new angular -o <output_directory_name> -au Individual
```

<span data-ttu-id="8a40d-113">**Réagir**:</span><span class="sxs-lookup"><span data-stu-id="8a40d-113">**React**:</span></span>

```console
dotnet new react -o <output_directory_name> -au Individual
```

<span data-ttu-id="8a40d-114">La commande précédente crée une application ASP.NET Core avec un *ClientApp* répertoire contenant l’application SPA.</span><span class="sxs-lookup"><span data-stu-id="8a40d-114">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the SPA.</span></span>

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a><span data-ttu-id="8a40d-115">Description générale des composants de l’application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8a40d-115">General description of the ASP.NET Core components of the app</span></span>

<span data-ttu-id="8a40d-116">Les sections suivantes décrivent les ajouts au projet lors de la prise en charge de l’authentification est inclus :</span><span class="sxs-lookup"><span data-stu-id="8a40d-116">The following sections describe additions to the project when authentication support is included:</span></span>

### <a name="startup-class"></a><span data-ttu-id="8a40d-117">Classe de démarrage</span><span class="sxs-lookup"><span data-stu-id="8a40d-117">Startup class</span></span>

<span data-ttu-id="8a40d-118">Le `Startup` classe a les ajouts suivants :</span><span class="sxs-lookup"><span data-stu-id="8a40d-118">The `Startup` class has the following additions:</span></span>

* <span data-ttu-id="8a40d-119">À l’intérieur de la `Startup.ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="8a40d-119">Inside the `Startup.ConfigureServices` method:</span></span>
  * <span data-ttu-id="8a40d-120">Identité de l’interface utilisateur par défaut :</span><span class="sxs-lookup"><span data-stu-id="8a40d-120">Identity with the default UI:</span></span>

    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();
    ```

  * <span data-ttu-id="8a40d-121">IdentityServer avec un autre `AddApiAuthorization` méthode d’assistance que paramétrages certains par défaut des conventions d’ASP.NET Core sur IdentityServer :</span><span class="sxs-lookup"><span data-stu-id="8a40d-121">IdentityServer with an additional `AddApiAuthorization` helper method that setups some default ASP.NET Core conventions on top of IdentityServer:</span></span>

    ```csharp
    services.AddIdentityServer()
        .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```

  * <span data-ttu-id="8a40d-122">Authentification avec un autre `AddIdentityServerJwt` méthode d’assistance qui configure l’application pour valider les jetons JWT produites par IdentityServer :</span><span class="sxs-lookup"><span data-stu-id="8a40d-122">Authentication with an additional `AddIdentityServerJwt` helper method that configures the app to validate JWT tokens produced by IdentityServer:</span></span>

    ```csharp
    services.AddAuthentication()
        .AddIdentityServerJwt();
    ```

* <span data-ttu-id="8a40d-123">À l’intérieur de la `Startup.Configure` méthode :</span><span class="sxs-lookup"><span data-stu-id="8a40d-123">Inside the `Startup.Configure` method:</span></span>
  * <span data-ttu-id="8a40d-124">Le middleware d’authentification qui est chargé de valider les informations d’identification de la demande et de définition de l’utilisateur sur le contexte de requête :</span><span class="sxs-lookup"><span data-stu-id="8a40d-124">The authentication middleware that is responsible for validating the request credentials and setting the user on the request context:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

  * <span data-ttu-id="8a40d-125">Le middleware IdentityServer qui expose les points de terminaison Open ID Connect :</span><span class="sxs-lookup"><span data-stu-id="8a40d-125">The IdentityServer middleware that exposes the Open ID Connect endpoints:</span></span>

    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="8a40d-126">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="8a40d-126">AddApiAuthorization</span></span>

<span data-ttu-id="8a40d-127">Cette méthode d’assistance configure IdentityServer pour utiliser notre configuration prise en charge.</span><span class="sxs-lookup"><span data-stu-id="8a40d-127">This helper method configures IdentityServer to use our supported configuration.</span></span> <span data-ttu-id="8a40d-128">IdentityServer est une infrastructure puissante et extensible pour la gestion des problèmes de sécurité d’application.</span><span class="sxs-lookup"><span data-stu-id="8a40d-128">IdentityServer is a powerful and extensible framework for handling app security concerns.</span></span> <span data-ttu-id="8a40d-129">En même temps, qui expose une complexité inutile pour les scénarios les plus courants.</span><span class="sxs-lookup"><span data-stu-id="8a40d-129">At the same time, that exposes unnecessary complexity for the most common scenarios.</span></span> <span data-ttu-id="8a40d-130">Par conséquent, un ensemble de conventions et les options de configuration est fourni pour vous qui sont considérés comme un bon point de départ.</span><span class="sxs-lookup"><span data-stu-id="8a40d-130">Consequently, a set of conventions and configuration options is provided to you that are considered a good starting point.</span></span> <span data-ttu-id="8a40d-131">Une fois que l’authentification de vos besoins évoluent, toute la puissance de IdentityServer est toujours disponible pour personnaliser l’authentification pour l’adapter à vos besoins.</span><span class="sxs-lookup"><span data-stu-id="8a40d-131">Once your authentication needs change, the full power of IdentityServer is still available to customize authentication to suit your needs.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="8a40d-132">AddIdentityServerJwt</span><span class="sxs-lookup"><span data-stu-id="8a40d-132">AddIdentityServerJwt</span></span>

<span data-ttu-id="8a40d-133">Cette méthode d’assistance configure un jeu de stratégie pour l’application en tant que le Gestionnaire d’authentification par défaut.</span><span class="sxs-lookup"><span data-stu-id="8a40d-133">This helper method configures a policy scheme for the app as the default authentication handler.</span></span> <span data-ttu-id="8a40d-134">La stratégie est configurée pour l’identité permettent de gérer toutes les demandes sont acheminées vers n’importe quel sous-tracé dans l’espace de l’URL d’identité « / identité ».</span><span class="sxs-lookup"><span data-stu-id="8a40d-134">The policy is configured to let Identity handle all requests routed to any subpath in the Identity URL space "/Identity".</span></span> <span data-ttu-id="8a40d-135">Le `JwtBearerHandler` gère toutes les autres demandes.</span><span class="sxs-lookup"><span data-stu-id="8a40d-135">The `JwtBearerHandler` handles all other requests.</span></span> <span data-ttu-id="8a40d-136">En outre, cette méthode inscrit une `<<ApplicationName>>API` ressource de l’API avec IdentityServer avec une étendue par défaut de `<<ApplicationName>>API` et configure l’intergiciel de jeton du porteur JWT pour valider les jetons émis par IdentityServer pour l’application.</span><span class="sxs-lookup"><span data-stu-id="8a40d-136">Additionally, this method registers an `<<ApplicationName>>API` API resource with IdentityServer with a default scope of `<<ApplicationName>>API` and configures the JWT Bearer token middleware to validate tokens issued by IdentityServer for the app.</span></span>

### <a name="sampledatacontroller"></a><span data-ttu-id="8a40d-137">SampleDataController</span><span class="sxs-lookup"><span data-stu-id="8a40d-137">SampleDataController</span></span>

<span data-ttu-id="8a40d-138">Dans le *Controllers\SampleDataController.cs* fichier, notez le `[Authorize]` basée sur les attributs appliqués à la classe qui indique que l’utilisateur doit être autorisé sur la stratégie par défaut pour accéder à la ressource.</span><span class="sxs-lookup"><span data-stu-id="8a40d-138">In the *Controllers\SampleDataController.cs* file, notice the `[Authorize]` attribute applied to the class that indicates that the user needs to be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="8a40d-139">La stratégie d’autorisation par défaut se trouve être configuré pour utiliser le schéma d’authentification par défaut, qui est configuré par `AddIdentityServerJwt` avec le schéma de la stratégie qui a été mentionné ci-dessus, ce qui le `JwtBearerHandler` configuré par cette méthode d’assistance, le gestionnaire par défaut requêtes à l’application.</span><span class="sxs-lookup"><span data-stu-id="8a40d-139">The default authorization policy happens to be configured to use the default authentication scheme, which is set up by `AddIdentityServerJwt` to the policy scheme that was mentioned above, making the `JwtBearerHandler` configured by such helper method the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="8a40d-140">ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="8a40d-140">ApplicationDbContext</span></span>

<span data-ttu-id="8a40d-141">Dans le *Data\ApplicationDbContext.cs* de fichier, notez que la même `DbContext` est utilisé dans l’identité avec l’exception qu’il étend `ApiAuthorizationDbContext` (plus dérivé de classe de `IdentityDbContext`) à inclure le schéma pour IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="8a40d-141">In the *Data\ApplicationDbContext.cs* file, notice the same `DbContext` is used in Identity with the exception that it extends `ApiAuthorizationDbContext` (a more derived class from `IdentityDbContext`) to include the schema for IdentityServer.</span></span>

<span data-ttu-id="8a40d-142">Pour obtenir un contrôle total du schéma de base de données, héritent d’un de l’identité disponible `DbContext` classes et configurer le contexte pour inclure le schéma de l’identité en appelant `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` sur la `OnModelCreating` (méthode).</span><span class="sxs-lookup"><span data-stu-id="8a40d-142">To gain full control of the database schema, inherit from one of the available Identity `DbContext` classes and configure the context to include the Identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` on the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="8a40d-143">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="8a40d-143">OidcConfigurationController</span></span>

<span data-ttu-id="8a40d-144">Dans le *Controllers\OidcConfigurationController.cs* de fichier, notez le point de terminaison est configuré pour servir les paramètres OIDC que le client doit utiliser.</span><span class="sxs-lookup"><span data-stu-id="8a40d-144">In the *Controllers\OidcConfigurationController.cs* file, notice the endpoint that's provisioned to serve the OIDC parameters that the client needs to use.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="8a40d-145">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="8a40d-145">appsettings.json</span></span>

<span data-ttu-id="8a40d-146">Dans le *appsettings.json* fichier de la racine du projet, il y a une nouvelle `IdentityServer` section qui décrit la liste des configuré les clients.</span><span class="sxs-lookup"><span data-stu-id="8a40d-146">In the *appsettings.json* file of the project root, there's a new `IdentityServer` section that describes the list of configured clients.</span></span> <span data-ttu-id="8a40d-147">Dans l’exemple suivant, il est un client unique.</span><span class="sxs-lookup"><span data-stu-id="8a40d-147">In the following example, there's a single client.</span></span> <span data-ttu-id="8a40d-148">Le nom du client correspond au nom de l’application et est mappé par convention pour le OAuth `ClientId` paramètre.</span><span class="sxs-lookup"><span data-stu-id="8a40d-148">The client name corresponds to the app name and is mapped by convention to the OAuth `ClientId` parameter.</span></span> <span data-ttu-id="8a40d-149">Le profil indique le type d’application en cours de configuration.</span><span class="sxs-lookup"><span data-stu-id="8a40d-149">The profile indicates the app type being configured.</span></span> <span data-ttu-id="8a40d-150">Il est utilisé en interne aux conventions de lecteur qui simplifient le processus de configuration pour le serveur.</span><span class="sxs-lookup"><span data-stu-id="8a40d-150">It's used internally to drive conventions that simplify the configuration process for the server.</span></span> <span data-ttu-id="8a40d-151">Plusieurs profils sont disponibles, comme expliqué dans la [profils d’Application](#application-profiles) section.</span><span class="sxs-lookup"><span data-stu-id="8a40d-151">There are several profiles available, as explained in the [Application profiles](#application-profiles) section.</span></span>

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a><span data-ttu-id="8a40d-152">appsettings.Development.json</span><span class="sxs-lookup"><span data-stu-id="8a40d-152">appsettings.Development.json</span></span>

<span data-ttu-id="8a40d-153">Dans le *appsettings. Development.JSON* fichier de la racine du projet, il existe un `IdentityServer` section qui décrit la clé utilisée pour signer les jetons.</span><span class="sxs-lookup"><span data-stu-id="8a40d-153">In the *appsettings.Development.json* file of the project root, there's an `IdentityServer` section that describes the key used to sign tokens.</span></span> <span data-ttu-id="8a40d-154">Lors du déploiement en production, une clé doit être configuré et déployé en même temps que l’application, comme expliqué dans la [déployer en production](#deploy-to-production) section.</span><span class="sxs-lookup"><span data-stu-id="8a40d-154">When deploying to production, a key needs to be provisioned and deployed alongside the app, as explained in the [Deploy to production](#deploy-to-production) section.</span></span>

```json
"IdentityServer": {
  "Key": {
    "Type": "Development"
  }
}
```

## <a name="general-description-of-the-angular-app"></a><span data-ttu-id="8a40d-155">Description générale de l’application Angular</span><span class="sxs-lookup"><span data-stu-id="8a40d-155">General description of the Angular app</span></span>

<span data-ttu-id="8a40d-156">Prend en charge l’authentification et l’autorisation de l’API dans Angular modèle réside dans son propre module Angular dans le *ClientApp\src\api-autorisation* directory.</span><span class="sxs-lookup"><span data-stu-id="8a40d-156">The authentication and API authorization support in the Angular template resides in its own Angular module in the *ClientApp\src\api-authorization* directory.</span></span> <span data-ttu-id="8a40d-157">Le module se compose des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="8a40d-157">The module is composed of the following elements:</span></span>

* <span data-ttu-id="8a40d-158">3 composants :</span><span class="sxs-lookup"><span data-stu-id="8a40d-158">3 components:</span></span>
  * <span data-ttu-id="8a40d-159">*Login.Component.TS*: Gère le flux de connexion de l’application.</span><span class="sxs-lookup"><span data-stu-id="8a40d-159">*login.component.ts*: Handles the app's login flow.</span></span>
  * <span data-ttu-id="8a40d-160">*Logout.Component.TS*: Gère le flux de déconnexion de l’application.</span><span class="sxs-lookup"><span data-stu-id="8a40d-160">*logout.component.ts*: Handles the app's logout flow.</span></span>
  * <span data-ttu-id="8a40d-161">*connexion-menu.component.ts*: Un widget qui affiche l’un des ensembles de liens suivants :</span><span class="sxs-lookup"><span data-stu-id="8a40d-161">*login-menu.component.ts*: A widget that displays one of the following sets of links:</span></span>
    * <span data-ttu-id="8a40d-162">Gestion de profil utilisateur et déconnexion de liens lorsque l’utilisateur est authentifié.</span><span class="sxs-lookup"><span data-stu-id="8a40d-162">User profile management and log out links when the user is authenticated.</span></span>
    * <span data-ttu-id="8a40d-163">L’inscription et journal dans les liens lorsque l’utilisateur n’est pas authentifié.</span><span class="sxs-lookup"><span data-stu-id="8a40d-163">Registration and log in links when the user isn't authenticated.</span></span>
* <span data-ttu-id="8a40d-164">Un garde itinéraire `AuthorizeGuard` qui peuvent être ajoutés à des itinéraires et nécessite un utilisateur de s’authentifier avant de visiter l’itinéraire.</span><span class="sxs-lookup"><span data-stu-id="8a40d-164">A route guard `AuthorizeGuard` that can be added to routes and requires a user to be authenticated before visiting the route.</span></span>
* <span data-ttu-id="8a40d-165">Un intercepteur HTTP `AuthorizeInterceptor` qui attache le jeton d’accès à des requêtes HTTP sortantes ciblant l’API lorsque l’utilisateur est authentifié.</span><span class="sxs-lookup"><span data-stu-id="8a40d-165">An HTTP interceptor `AuthorizeInterceptor` that attaches the access token to outgoing HTTP requests targeting the API when the user is authenticated.</span></span>
* <span data-ttu-id="8a40d-166">Un service `AuthorizeService` qui gère les détails de niveau inférieur du processus d’authentification et expose des informations sur l’utilisateur authentifié au reste de l’application pour la consommation.</span><span class="sxs-lookup"><span data-stu-id="8a40d-166">A service `AuthorizeService` that handles the lower-level details of the authentication process and exposes information about the authenticated user to the rest of the app for consumption.</span></span>
* <span data-ttu-id="8a40d-167">Un module Angular qui définit les itinéraires associés avec les parties de l’authentification de l’application.</span><span class="sxs-lookup"><span data-stu-id="8a40d-167">An Angular module that defines routes associated with the authentication parts of the app.</span></span> <span data-ttu-id="8a40d-168">Il expose le composant de menu de connexion, l’intercepteur, le module de protection et le service pour la consommation du reste de l’application.</span><span class="sxs-lookup"><span data-stu-id="8a40d-168">It exposes the login menu component, the interceptor, the guard, and the service for consumption from the rest of the app.</span></span>

## <a name="general-description-of-the-react-app"></a><span data-ttu-id="8a40d-169">Description générale de l’application React</span><span class="sxs-lookup"><span data-stu-id="8a40d-169">General description of the React app</span></span>

<span data-ttu-id="8a40d-170">La prise en charge pour l’authentification et l’autorisation d’API dans le modèle de React réside dans le *ClientApp\src\components\api-autorisation* directory.</span><span class="sxs-lookup"><span data-stu-id="8a40d-170">The support for authentication and API authorization in the React template resides in the *ClientApp\src\components\api-authorization* directory.</span></span> <span data-ttu-id="8a40d-171">Il est composé des éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="8a40d-171">It's composed of the following elements:</span></span>

* <span data-ttu-id="8a40d-172">4 composants :</span><span class="sxs-lookup"><span data-stu-id="8a40d-172">4 components:</span></span>
  * <span data-ttu-id="8a40d-173">*Login.js*: Gère le flux de connexion de l’application.</span><span class="sxs-lookup"><span data-stu-id="8a40d-173">*Login.js*: Handles the app's login flow.</span></span>
  * <span data-ttu-id="8a40d-174">*Logout.js*: Gère le flux de déconnexion de l’application.</span><span class="sxs-lookup"><span data-stu-id="8a40d-174">*Logout.js*: Handles the app's logout flow.</span></span>
  * <span data-ttu-id="8a40d-175">*LoginMenu.js*: Un widget qui affiche l’un des ensembles de liens suivants :</span><span class="sxs-lookup"><span data-stu-id="8a40d-175">*LoginMenu.js*: A widget that displays one of the following sets of links:</span></span>
    * <span data-ttu-id="8a40d-176">Gestion de profil utilisateur et déconnexion de liens lorsque l’utilisateur est authentifié.</span><span class="sxs-lookup"><span data-stu-id="8a40d-176">User profile management and log out links when the user is authenticated.</span></span>
    * <span data-ttu-id="8a40d-177">L’inscription et journal dans les liens lorsque l’utilisateur n’est pas authentifié.</span><span class="sxs-lookup"><span data-stu-id="8a40d-177">Registration and log in links when the user isn't authenticated.</span></span>
  * <span data-ttu-id="8a40d-178">*AuthorizeRoute.js*: Un composant de routage qui nécessite un utilisateur de s’authentifier avant de le restituer le composant indiqué dans le `Component` paramètre.</span><span class="sxs-lookup"><span data-stu-id="8a40d-178">*AuthorizeRoute.js*: A route component that requires a user to be authenticated before rendering the component indicated in the `Component` parameter.</span></span>
* <span data-ttu-id="8a40d-179">Un exporté `authService` instance de classe `AuthorizeService` qui gère les détails de niveau inférieur du processus d’authentification et expose des informations sur l’utilisateur authentifié au reste de l’application pour la consommation.</span><span class="sxs-lookup"><span data-stu-id="8a40d-179">An exported `authService` instance of class `AuthorizeService` that handles the lower-level details of the authentication process and exposes information about the authenticated user to the rest of the app for consumption.</span></span>

<span data-ttu-id="8a40d-180">Maintenant que vous avez vu les principaux composants de la solution, vous pouvez tirer étudier des scénarios individuels pour l’application.</span><span class="sxs-lookup"><span data-stu-id="8a40d-180">Now that you've seen the main components of the solution, you can take a deeper look at individual scenarios for the app.</span></span>

## <a name="require-authorization-on-a-new-api"></a><span data-ttu-id="8a40d-181">Requièrent une autorisation sur une nouvelle API</span><span class="sxs-lookup"><span data-stu-id="8a40d-181">Require authorization on a new API</span></span>

<span data-ttu-id="8a40d-182">Par défaut, le système est configuré pour exiger facilement l’autorisation de nouvelles API.</span><span class="sxs-lookup"><span data-stu-id="8a40d-182">By default, the system is configured to easily require authorization for new APIs.</span></span> <span data-ttu-id="8a40d-183">Pour ce faire, créez un nouveau contrôleur et ajoutez le `[Authorize]` attribut à la classe de contrôleur ou à des actions dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="8a40d-183">To do so, create a new controller and add the `[Authorize]` attribute to the controller class or to any action within the controller.</span></span>

## <a name="protect-a-client-side-route-angular"></a><span data-ttu-id="8a40d-184">Protéger un itinéraire côté client (Angular)</span><span class="sxs-lookup"><span data-stu-id="8a40d-184">Protect a client-side route (Angular)</span></span>

<span data-ttu-id="8a40d-185">Protéger un itinéraire côté client s’effectue en ajoutant de la garde authorize à la liste des gardes à exécuter lors de la configuration d’un itinéraire.</span><span class="sxs-lookup"><span data-stu-id="8a40d-185">Protecting a client-side route is done by adding the authorize guard to the list of guards to run when configuring a route.</span></span> <span data-ttu-id="8a40d-186">Par exemple, vous pouvez voir comment la `fetch-data` itinéraire est configuré dans le module d’Angular principal d’application :</span><span class="sxs-lookup"><span data-stu-id="8a40d-186">As an example, you can see how the `fetch-data` route is configured within the main app Angular module:</span></span>

```typescript
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

<span data-ttu-id="8a40d-187">Il est important de mentionner que protégeant un itinéraire ne protège pas le point de terminaison réel (ce qui nécessite toujours un `[Authorize]` attribut appliqué à ce dernier), mais qu’elle empêche uniquement l’utilisateur de naviguer vers l’itinéraire côté client donnée lorsqu’il n’est pas authentifié.</span><span class="sxs-lookup"><span data-stu-id="8a40d-187">It's important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client-side route when it isn't authenticated.</span></span>

## <a name="authenticate-api-requests-angular"></a><span data-ttu-id="8a40d-188">Authentifier les demandes d’API (Angular)</span><span class="sxs-lookup"><span data-stu-id="8a40d-188">Authenticate API requests (Angular)</span></span>

<span data-ttu-id="8a40d-189">L’authentification des demandes pour les API hébergées en même temps que l’application est effectuée automatiquement via l’utilisation de l’intercepteur de client HTTP défini par l’application.</span><span class="sxs-lookup"><span data-stu-id="8a40d-189">Authenticating requests to APIs hosted alongside the app is done automatically through the use of the HTTP client interceptor defined by the app.</span></span>

## <a name="protect-a-client-side-route-react"></a><span data-ttu-id="8a40d-190">Protéger un itinéraire côté client (React)</span><span class="sxs-lookup"><span data-stu-id="8a40d-190">Protect a client-side route (React)</span></span>

<span data-ttu-id="8a40d-191">Protéger un itinéraire côté client à l’aide de la `AuthorizeRoute` composant au lieu du brut `Route` composant.</span><span class="sxs-lookup"><span data-stu-id="8a40d-191">Protect a client-side route by using the `AuthorizeRoute` component instead of the plain `Route` component.</span></span> <span data-ttu-id="8a40d-192">Par exemple, notez comment la `fetch-data` itinéraire est configuré dans le `App` composant :</span><span class="sxs-lookup"><span data-stu-id="8a40d-192">For example, notice how the `fetch-data` route is configured within the `App` component:</span></span>

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

<span data-ttu-id="8a40d-193">Protéger un itinéraire :</span><span class="sxs-lookup"><span data-stu-id="8a40d-193">Protecting a route:</span></span>

* <span data-ttu-id="8a40d-194">Ne protège pas le point de terminaison réel (ce qui nécessite toujours un `[Authorize]` attribut appliqué à celui-ci).</span><span class="sxs-lookup"><span data-stu-id="8a40d-194">Doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it).</span></span>
* <span data-ttu-id="8a40d-195">Uniquement empêche l’utilisateur de naviguer vers l’itinéraire côté client donnée lorsqu’il n’est pas authentifié.</span><span class="sxs-lookup"><span data-stu-id="8a40d-195">Only prevents the user from navigating to the given client-side route when it isn't authenticated.</span></span>

## <a name="authenticate-api-requests-react"></a><span data-ttu-id="8a40d-196">Authentifier les demandes d’API (React)</span><span class="sxs-lookup"><span data-stu-id="8a40d-196">Authenticate API requests (React)</span></span>

<span data-ttu-id="8a40d-197">L’authentification des demandes avec React s’effectue en important d’abord le `authService` instance à partir de la `AuthorizeService`.</span><span class="sxs-lookup"><span data-stu-id="8a40d-197">Authenticating requests with React is done by first importing the `authService` instance from the `AuthorizeService`.</span></span> <span data-ttu-id="8a40d-198">Le jeton d’accès est récupéré à partir de la `authService` et est associé à la demande, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="8a40d-198">The access token is retrieved from the `authService` and is attached to the request as shown below.</span></span> <span data-ttu-id="8a40d-199">Dans composants React, ce travail est généralement effectué le `componentDidMount` méthode de cycle de vie ou en tant que le résultat à partir d’une interaction utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8a40d-199">In React components, this work is typically done in the `componentDidMount` lifecycle method or as the result from some user interaction.</span></span>

### <a name="import-the-authservice-into-your-component"></a><span data-ttu-id="8a40d-200">Importer l’authService dans votre composant</span><span class="sxs-lookup"><span data-stu-id="8a40d-200">Import the authService into your component</span></span>

```javascript
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a><span data-ttu-id="8a40d-201">Récupérer et joignez le jeton d’accès à la réponse</span><span class="sxs-lookup"><span data-stu-id="8a40d-201">Retrieve and attach the access token to the response</span></span>

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

## <a name="deploy-to-production"></a><span data-ttu-id="8a40d-202">Déployer en production</span><span class="sxs-lookup"><span data-stu-id="8a40d-202">Deploy to production</span></span>

<span data-ttu-id="8a40d-203">Pour déployer l’application en production, les ressources suivantes doivent être approvisionnés :</span><span class="sxs-lookup"><span data-stu-id="8a40d-203">To deploy the app to production, the following resources need to be provisioned:</span></span>

* <span data-ttu-id="8a40d-204">Une base de données pour stocker les comptes d’utilisateur de l’identité et les privilèges accordés IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="8a40d-204">A database to store the Identity user accounts and the IdentityServer grants.</span></span>
* <span data-ttu-id="8a40d-205">Un certificat de production à utiliser pour la signature de jetons.</span><span class="sxs-lookup"><span data-stu-id="8a40d-205">A production certificate to use for signing tokens.</span></span>
  * <span data-ttu-id="8a40d-206">Il n’y a aucune exigence particulière pour ce certificat ; Il peut être un certificat auto-signé ou un certificat configuré via une autorité de certification.</span><span class="sxs-lookup"><span data-stu-id="8a40d-206">There are no specific requirements for this certificate; it can be a self-signed certificate or a certificate provisioned through a CA authority.</span></span>
  * <span data-ttu-id="8a40d-207">Il peut être généré par le biais des outils standards tels que PowerShell ou OpenSSL.</span><span class="sxs-lookup"><span data-stu-id="8a40d-207">It can be generated through standard tools like PowerShell or OpenSSL.</span></span>
  * <span data-ttu-id="8a40d-208">Il peut être installé dans le magasin de certificats sur les ordinateurs cibles ou déployé comme un *.pfx* fichier avec un mot de passe fort.</span><span class="sxs-lookup"><span data-stu-id="8a40d-208">It can be installed into the certificate store on the target machines or deployed as a *.pfx* file with a strong password.</span></span>

### <a name="example-deploy-to-azure-websites"></a><span data-ttu-id="8a40d-209">Exemple : Déployer des sites Web Azure</span><span class="sxs-lookup"><span data-stu-id="8a40d-209">Example: Deploy to Azure Websites</span></span>

<span data-ttu-id="8a40d-210">Cette section décrit le déploiement de l’application à des sites Web Azure à l’aide d’un certificat stocké dans le magasin de certificats.</span><span class="sxs-lookup"><span data-stu-id="8a40d-210">This section describes deploying the app to Azure websites using a certificate stored in the certificate store.</span></span> <span data-ttu-id="8a40d-211">Pour modifier l’application pour charger un certificat du magasin de certificats, le plan App Service doit s’exécuter sur au moins le niveau Standard lorsque vous configurez dans une étape ultérieure.</span><span class="sxs-lookup"><span data-stu-id="8a40d-211">To modify the app to load a certificate from the certificate store, the App Service plan needs to be on at least the Standard tier when you configure in a later step.</span></span> <span data-ttu-id="8a40d-212">Dans l’application *appsettings.json* fichier, modifiez le `IdentityServer` section pour inclure les détails de la clé :</span><span class="sxs-lookup"><span data-stu-id="8a40d-212">In the app's *appsettings.json* file, modify the `IdentityServer` section to include the key details:</span></span>

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

* <span data-ttu-id="8a40d-213">La propriété nom de certificat correspond à l’objet du certificat unique.</span><span class="sxs-lookup"><span data-stu-id="8a40d-213">The name property on certificate corresponds with the distinguished subject for the certificate.</span></span>
* <span data-ttu-id="8a40d-214">L’emplacement du magasin représente l’emplacement de charger le certificat à partir de (`CurrentUser` ou `LocalMachine`).</span><span class="sxs-lookup"><span data-stu-id="8a40d-214">The store location represents where to load the certificate from (`CurrentUser` or `LocalMachine`).</span></span>
* <span data-ttu-id="8a40d-215">Le nom du magasin représente le nom du magasin de certificats dans lequel le certificat est stocké.</span><span class="sxs-lookup"><span data-stu-id="8a40d-215">The store name represents the name of the certificate store where the certificate is stored.</span></span> <span data-ttu-id="8a40d-216">Dans ce cas, il pointe vers le magasin utilisateur personnel.</span><span class="sxs-lookup"><span data-stu-id="8a40d-216">In this case, it points to the personal user store.</span></span>

<span data-ttu-id="8a40d-217">Pour déployer sur Azure Websites, déployez l’application en suivant les étapes décrites dans [déployer l’application sur Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) pour créer les ressources Azure nécessaires et de déployer l’application en production.</span><span class="sxs-lookup"><span data-stu-id="8a40d-217">To deploy to Azure Websites, deploy the app following the steps in [Deploy the app to Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) to create the necessary Azure resources and deploy the app to production.</span></span>

<span data-ttu-id="8a40d-218">Après avoir suivi les instructions précédentes, l’application est déployée sur Azure, mais n’est pas encore fonctionnelle.</span><span class="sxs-lookup"><span data-stu-id="8a40d-218">After following the preceding instructions, the app is deployed to Azure but isn't yet functional.</span></span> <span data-ttu-id="8a40d-219">Le certificat utilisé par l’application doit toujours être configuré.</span><span class="sxs-lookup"><span data-stu-id="8a40d-219">The certificate used by the app still needs to be set up.</span></span> <span data-ttu-id="8a40d-220">Localiser l’empreinte numérique du certificat à utiliser, puis suivez les étapes décrites dans [charger vos certificats](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).</span><span class="sxs-lookup"><span data-stu-id="8a40d-220">Locate the thumbprint for the certificate to be used, and follow the steps described in [Load your certificates](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).</span></span>

<span data-ttu-id="8a40d-221">Bien que ces étapes mentionner SSL, il existe un **certificats privés** section du portail où vous pouvez télécharger le certificat configuré à utiliser avec l’application.</span><span class="sxs-lookup"><span data-stu-id="8a40d-221">While these steps mention SSL, there's a **Private certificates** section on the portal where you can upload the provisioned certificate to use with the app.</span></span>

<span data-ttu-id="8a40d-222">Après cette étape, redémarrez l’application et il doit être fonctionnel.</span><span class="sxs-lookup"><span data-stu-id="8a40d-222">After this step, restart the app and it should be functional.</span></span>

## <a name="other-configuration-options"></a><span data-ttu-id="8a40d-223">Autres options de configuration</span><span class="sxs-lookup"><span data-stu-id="8a40d-223">Other configuration options</span></span>

<span data-ttu-id="8a40d-224">La prise en charge pour l’autorisation de l’API s’appuie sur IdentityServer avec un ensemble de conventions, les valeurs par défaut et les améliorations apportées à simplifier l’expérience pour les applications SPA.</span><span class="sxs-lookup"><span data-stu-id="8a40d-224">The support for API authorization builds on top of IdentityServer with a set of conventions, default values, and enhancements to simplify the experience for SPAs.</span></span> <span data-ttu-id="8a40d-225">Inutile de dire que toute la puissance de IdentityServer est disponible dans les coulisses si votre scénario ne couvrent pas les intégrations d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8a40d-225">Needless to say, the full power of IdentityServer is available behind the scenes if the ASP.NET Core integrations don't cover your scenario.</span></span> <span data-ttu-id="8a40d-226">La prise en charge d’ASP.NET Core se concentre sur les applications « internes », où toutes les applications sont créées et déployées par notre organisation.</span><span class="sxs-lookup"><span data-stu-id="8a40d-226">The ASP.NET Core support is focused on "first-party" apps, where all the apps are created and deployed by our organization.</span></span> <span data-ttu-id="8a40d-227">Par conséquent, il n’est pas pris en charge pour les éléments tels que le consentement ou la fédération.</span><span class="sxs-lookup"><span data-stu-id="8a40d-227">As such, support isn't offered for things like consent or federation.</span></span> <span data-ttu-id="8a40d-228">Pour ces scénarios, utilisez IdentityServer et suivre leur documentation.</span><span class="sxs-lookup"><span data-stu-id="8a40d-228">For those scenarios, use IdentityServer and follow their documentation.</span></span>

### <a name="application-profiles"></a><span data-ttu-id="8a40d-229">Profils d’application</span><span class="sxs-lookup"><span data-stu-id="8a40d-229">Application profiles</span></span>

<span data-ttu-id="8a40d-230">Profils d’application sont des configurations prédéfinies pour les applications qui définissent leurs paramètres.</span><span class="sxs-lookup"><span data-stu-id="8a40d-230">Application profiles are predefined configurations for apps that further define their parameters.</span></span> <span data-ttu-id="8a40d-231">À ce stade, les profils suivants sont pris en charge :</span><span class="sxs-lookup"><span data-stu-id="8a40d-231">At this time, the following profiles are supported:</span></span>

* <span data-ttu-id="8a40d-232">`IdentityServerSPA`: Représente une application SPA hébergée avec IdentityServer comme une seule unité.</span><span class="sxs-lookup"><span data-stu-id="8a40d-232">`IdentityServerSPA`: Represents a SPA hosted alongside IdentityServer as a single unit.</span></span>
  * <span data-ttu-id="8a40d-233">Le `redirect_uri` par défaut est `/authentication/login-callback`.</span><span class="sxs-lookup"><span data-stu-id="8a40d-233">The `redirect_uri` defaults to `/authentication/login-callback`.</span></span>
  * <span data-ttu-id="8a40d-234">Le `post_logout_redirect_uri` par défaut est `/authentication/logout-callback`.</span><span class="sxs-lookup"><span data-stu-id="8a40d-234">The `post_logout_redirect_uri` defaults to `/authentication/logout-callback`.</span></span>
  * <span data-ttu-id="8a40d-235">Le jeu d’étendues inclut le `openid`, `profile`et chaque étendue définie pour les API dans l’application.</span><span class="sxs-lookup"><span data-stu-id="8a40d-235">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the app.</span></span>
  * <span data-ttu-id="8a40d-236">Est l’ensemble des types de réponse autorisés OIDC `id_token token` ou chacun d’eux individuellement (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="8a40d-236">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="8a40d-237">Le mode de réponse autorisés est `fragment`.</span><span class="sxs-lookup"><span data-stu-id="8a40d-237">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="8a40d-238">`SPA`: Représente une application à page unique qui n’est pas hébergée avec IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="8a40d-238">`SPA`: Represents a SPA that isn't hosted with IdentityServer.</span></span>
  * <span data-ttu-id="8a40d-239">Le jeu d’étendues inclut le `openid`, `profile`et chaque étendue définie pour les API dans l’application.</span><span class="sxs-lookup"><span data-stu-id="8a40d-239">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the app.</span></span>
  * <span data-ttu-id="8a40d-240">Est l’ensemble des types de réponse autorisés OIDC `id_token token` ou chacun d’eux individuellement (`id_token`, `token`).</span><span class="sxs-lookup"><span data-stu-id="8a40d-240">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="8a40d-241">Le mode de réponse autorisés est `fragment`.</span><span class="sxs-lookup"><span data-stu-id="8a40d-241">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="8a40d-242">`IdentityServerJwt`: Représente une API qui est hébergée en même temps qu’avec IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="8a40d-242">`IdentityServerJwt`: Represents an API that is hosted alongside with IdentityServer.</span></span>
  * <span data-ttu-id="8a40d-243">L’application est configurée pour avoir une seule étendue par défaut est le nom de l’application.</span><span class="sxs-lookup"><span data-stu-id="8a40d-243">The app is configured to have a single scope that defaults to the app name.</span></span>
* <span data-ttu-id="8a40d-244">`API`: Représente une API qui n’est pas hébergée avec IdentityServer.</span><span class="sxs-lookup"><span data-stu-id="8a40d-244">`API`: Represents an API that isn't hosted with IdentityServer.</span></span>
  * <span data-ttu-id="8a40d-245">L’application est configurée pour avoir une seule étendue par défaut est le nom de l’application.</span><span class="sxs-lookup"><span data-stu-id="8a40d-245">The app is configured to have a single scope that defaults to the app name.</span></span>

### <a name="configuration-through-appsettings"></a><span data-ttu-id="8a40d-246">Configuration via AppSettings</span><span class="sxs-lookup"><span data-stu-id="8a40d-246">Configuration through AppSettings</span></span>

<span data-ttu-id="8a40d-247">Configurer les applications via le système de configuration en les ajoutant à la liste des `Clients` ou `Resources`.</span><span class="sxs-lookup"><span data-stu-id="8a40d-247">Configure the apps through the configuration system by adding them to the list of `Clients` or `Resources`.</span></span>

<span data-ttu-id="8a40d-248">Configurer de chaque client `redirect_uri` et `post_logout_redirect_uri` propriété, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="8a40d-248">Configure each client's `redirect_uri` and `post_logout_redirect_uri` property, as shown in the following example:</span></span>

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

<span data-ttu-id="8a40d-249">Lors de la configuration des ressources, vous pouvez configurer les étendues pour la ressource, comme indiqué ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="8a40d-249">When configuring resources, you can configure the scopes for the resource as shown below:</span></span>

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

### <a name="configuration-through-code"></a><span data-ttu-id="8a40d-250">Configuration via le code</span><span class="sxs-lookup"><span data-stu-id="8a40d-250">Configuration through code</span></span>

<span data-ttu-id="8a40d-251">Vous pouvez également configurer les clients et les ressources dans le code à l’aide d’une surcharge de `AddApiAuthorization` qui effectue une action pour configurer les options.</span><span class="sxs-lookup"><span data-stu-id="8a40d-251">You can also configure the clients and resources through code using an overload of `AddApiAuthorization` that takes an action to configure options.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="8a40d-252">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="8a40d-252">Additional resources</span></span>

* <xref:spa/angular>
* <xref:spa/react>
