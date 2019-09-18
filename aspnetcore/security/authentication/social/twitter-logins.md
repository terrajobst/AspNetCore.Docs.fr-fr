---
title: Configuration de la connexion externe Twitter avec ASP.NET Core
author: rick-anderson
description: Ce didacticiel illustre l’intégration de l’authentification utilisateur de compte Twitter dans une application ASP.NET Core existante.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/twitter-logins
ms.openlocfilehash: 5182f1647acb664bf35f086fcddbe909559a62f7
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082302"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a><span data-ttu-id="9aa11-103">Configuration de la connexion externe Twitter avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9aa11-103">Twitter external sign-in setup with ASP.NET Core</span></span>

<span data-ttu-id="9aa11-104">Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9aa11-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9aa11-105">Cet exemple montre comment permettre aux utilisateurs de [se connecter avec leur compte Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) à l’aide d’un exemple de projet ASP.net Core 2,2 créé sur la [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="9aa11-105">This sample shows how to enable users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="9aa11-106">Créer l’application dans Twitter</span><span class="sxs-lookup"><span data-stu-id="9aa11-106">Create the app in Twitter</span></span>

* <span data-ttu-id="9aa11-107">Accédez à [ https://apps.twitter.com/ ](https://apps.twitter.com/) et s’y connecter.</span><span class="sxs-lookup"><span data-stu-id="9aa11-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="9aa11-108">Si vous ne disposez pas déjà d’un compte Twitter, utilisez le lien **[Inscrivez-vous maintenant](https://twitter.com/signup)** pour en créer un.</span><span class="sxs-lookup"><span data-stu-id="9aa11-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span>

* <span data-ttu-id="9aa11-109">Appuyez sur **créer une nouvelle application** et renseignez le **nom**de l’application, la **Description** et l’URI du **site Web** public (cela peut être temporaire jusqu’à ce que vous enregistriez le nom de domaine) :</span><span class="sxs-lookup"><span data-stu-id="9aa11-109">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

* <span data-ttu-id="9aa11-110">Entrez votre URI de développement `/signin-twitter` avec ajouté dans le champ **URI de redirection OAuth valide** (par exemple `https://webapp128.azurewebsites.net/signin-twitter`:).</span><span class="sxs-lookup"><span data-stu-id="9aa11-110">Enter your development URI with `/signin-twitter` appended into the **Valid OAuth Redirect URIs** field (for example: `https://webapp128.azurewebsites.net/signin-twitter`).</span></span> <span data-ttu-id="9aa11-111">Le schéma d’authentification Twitter configuré plus tard dans cet exemple gère automatiquement les `/signin-twitter` demandes à l’itinéraire pour implémenter le Flow OAuth.</span><span class="sxs-lookup"><span data-stu-id="9aa11-111">The Twitter authentication scheme configured later in this sample will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="9aa11-112">Le segment `/signin-twitter` d’URI est défini en tant que rappel par défaut du fournisseur d’authentification Twitter.</span><span class="sxs-lookup"><span data-stu-id="9aa11-112">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="9aa11-113">Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel d’authentification Twitter via la propriété héritée [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) de la classe [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) .</span><span class="sxs-lookup"><span data-stu-id="9aa11-113">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="9aa11-114">Remplissez le reste du formulaire et appuyez sur **créer votre application Twitter**.</span><span class="sxs-lookup"><span data-stu-id="9aa11-114">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="9aa11-115">Les détails de la nouvelle application s’affichent :</span><span class="sxs-lookup"><span data-stu-id="9aa11-115">New application details are displayed:</span></span>

## <a name="storing-twitter-consumer-api-key-and-secret"></a><span data-ttu-id="9aa11-116">Stockage de la clé et du secret de l’API du consommateur Twitter</span><span class="sxs-lookup"><span data-stu-id="9aa11-116">Storing Twitter Consumer API key and secret</span></span>

<span data-ttu-id="9aa11-117">Exécutez les commandes suivantes pour stocker `ClientId` et `ClientSecret` utiliser le [Gestionnaire de secret](xref:security/app-secrets)en toute sécurité :</span><span class="sxs-lookup"><span data-stu-id="9aa11-117">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerSecret <Secret>
```

<span data-ttu-id="9aa11-118">Liez des paramètres sensibles tels `Consumer Key` que `Consumer Secret` Twitter et à la configuration de votre application à l’aide du [Gestionnaire de secret](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="9aa11-118">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="9aa11-119">Pour les besoins de cet exemple, nommez les jetons `Authentication:Twitter:ConsumerKey` et `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="9aa11-119">For the purposes of this sample, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="9aa11-120">Ces jetons se trouvent sous l’onglet **clés et jetons d’accès** après la création d’une application Twitter :</span><span class="sxs-lookup"><span data-stu-id="9aa11-120">These tokens can be found on the **Keys and Access Tokens** tab after creating a new Twitter application:</span></span>

## <a name="configure-twitter-authentication"></a><span data-ttu-id="9aa11-121">Configurer l’authentification Twitter</span><span class="sxs-lookup"><span data-stu-id="9aa11-121">Configure Twitter Authentication</span></span>

<span data-ttu-id="9aa11-122">Ajoutez le service Twitter dans la `ConfigureServices` méthode dans le fichier *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="9aa11-122">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupTwitter.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="9aa11-123">Pour plus d’informations sur les options de configuration prises en charge par l’authentification Twitter, consultez la référence de l’API [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) .</span><span class="sxs-lookup"><span data-stu-id="9aa11-123">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="9aa11-124">Cela peut être utilisé pour demander différentes informations sur l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="9aa11-124">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="9aa11-125">Se connecter avec Twitter</span><span class="sxs-lookup"><span data-stu-id="9aa11-125">Sign in with Twitter</span></span>

<span data-ttu-id="9aa11-126">Exécutez l’application et sélectionnez **se connecter**.</span><span class="sxs-lookup"><span data-stu-id="9aa11-126">Run the app and select **Log in**.</span></span> <span data-ttu-id="9aa11-127">Une option permettant de se connecter avec Twitter s’affiche :</span><span class="sxs-lookup"><span data-stu-id="9aa11-127">An option to sign in with Twitter appears:</span></span>

<span data-ttu-id="9aa11-128">En cliquant sur **Twitter** , vous redirigez vers Twitter pour l’authentification :</span><span class="sxs-lookup"><span data-stu-id="9aa11-128">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

<span data-ttu-id="9aa11-129">Après avoir entré vos informations d’identification Twitter, vous êtes redirigé vers le site Web où vous pouvez définir votre adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="9aa11-129">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="9aa11-130">Vous êtes maintenant connecté à l’aide de vos informations d’identification Twitter :</span><span class="sxs-lookup"><span data-stu-id="9aa11-130">You are now logged in using your Twitter credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="9aa11-131">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="9aa11-131">Troubleshooting</span></span>

* <span data-ttu-id="9aa11-132">**ASP.NET Core 2. x uniquement :** Si l’identité n’est pas `services.AddIdentity` configurée en appelant dans `ConfigureServices`, toute tentative *d’authentification entraînera l’exception ArgumentException : L’option « SignInScheme » doit être fournie*.</span><span class="sxs-lookup"><span data-stu-id="9aa11-132">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="9aa11-133">Le modèle de projet utilisé dans cet exemple permet de s’assurer que cette opération est effectuée.</span><span class="sxs-lookup"><span data-stu-id="9aa11-133">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="9aa11-134">Si la base de données de site n’a pas été créé en appliquant la migration initiale, vous obtiendrez *une opération de base de données a échoué lors du traitement de la demande* erreur.</span><span class="sxs-lookup"><span data-stu-id="9aa11-134">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="9aa11-135">Appuyez sur **appliquer les Migrations** pour créer la base de données et actualiser pour passer à l’erreur.</span><span class="sxs-lookup"><span data-stu-id="9aa11-135">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9aa11-136">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="9aa11-136">Next steps</span></span>

* <span data-ttu-id="9aa11-137">Cet article vous a montré comment vous pouvez vous authentifier avec Twitter.</span><span class="sxs-lookup"><span data-stu-id="9aa11-137">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="9aa11-138">Vous pouvez suivre une approche similaire pour s’authentifier auprès d’autres fournisseurs répertoriés sur le [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="9aa11-138">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="9aa11-139">Une fois que vous publiez votre site Web sur Azure Web App, vous `ConsumerSecret` devez réinitialiser le dans le portail des développeurs Twitter.</span><span class="sxs-lookup"><span data-stu-id="9aa11-139">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="9aa11-140">Définir le `Authentication:Twitter:ConsumerKey` et `Authentication:Twitter:ConsumerSecret` en tant que paramètres d’application dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="9aa11-140">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="9aa11-141">Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="9aa11-141">The configuration system is set up to read keys from environment variables.</span></span>
