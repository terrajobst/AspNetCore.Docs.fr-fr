---
title: Configuration de la connexion externe Twitter avec ASP.NET Core
author: rick-anderson
description: Ce didacticiel illustre l’intégration de l’authentification utilisateur de compte Twitter dans une application ASP.NET Core existante.
ms.author: riande
ms.custom: mvc
ms.date: 12/06/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/twitter-logins
ms.openlocfilehash: 5d0695160d90d0c5d31b8e35bc6c4cc984829333
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944211"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a><span data-ttu-id="8f717-103">Configuration de la connexion externe Twitter avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8f717-103">Twitter external sign-in setup with ASP.NET Core</span></span>

<span data-ttu-id="8f717-104">Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8f717-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8f717-105">Cet exemple montre comment permettre aux utilisateurs de [se connecter avec leur compte Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) à l’aide d’un exemple de projet ASP.net Core 3,0 créé sur la [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="8f717-105">This sample shows how to enable users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="8f717-106">Créer l’application dans Twitter</span><span class="sxs-lookup"><span data-stu-id="8f717-106">Create the app in Twitter</span></span>

* <span data-ttu-id="8f717-107">Ajoutez le package NuGet [Microsoft. AspNetCore. Authentication. Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) au projet.</span><span class="sxs-lookup"><span data-stu-id="8f717-107">Add the [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) NuGet package to the project.</span></span>

* <span data-ttu-id="8f717-108">Accédez à [ https://apps.twitter.com/ ](https://apps.twitter.com/) et s’y connecter.</span><span class="sxs-lookup"><span data-stu-id="8f717-108">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="8f717-109">Si vous ne disposez pas déjà d’un compte Twitter, utilisez le lien **[Inscrivez-vous maintenant](https://twitter.com/signup)** pour en créer un.</span><span class="sxs-lookup"><span data-stu-id="8f717-109">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span>

* <span data-ttu-id="8f717-110">Sélectionnez **Créer une application**.</span><span class="sxs-lookup"><span data-stu-id="8f717-110">Select **Create an app**.</span></span> <span data-ttu-id="8f717-111">Renseignez le **nom**de l' **application** , la description de l’application et l’URI du **site Web** public (cela peut être temporaire jusqu’à ce que vous enregistriez le nom de domaine) :</span><span class="sxs-lookup"><span data-stu-id="8f717-111">Fill out the **App name**, **Application description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

* <span data-ttu-id="8f717-112">Entrez votre URI de développement avec `/signin-twitter` ajouté dans le champ **URL de rappel** (par exemple : `https://webapp128.azurewebsites.net/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="8f717-112">Enter your development URI with `/signin-twitter` appended into the **Callback URLs** field (for example: `https://webapp128.azurewebsites.net/signin-twitter`).</span></span> <span data-ttu-id="8f717-113">Le schéma d’authentification Twitter configuré plus tard dans cet exemple gère automatiquement les demandes à `/signin-twitter` itinéraire pour implémenter le Flow OAuth.</span><span class="sxs-lookup"><span data-stu-id="8f717-113">The Twitter authentication scheme configured later in this sample will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="8f717-114">Le segment d’URI `/signin-twitter` est défini comme rappel par défaut du fournisseur d’authentification Twitter.</span><span class="sxs-lookup"><span data-stu-id="8f717-114">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="8f717-115">Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel d’authentification Twitter via la propriété héritée [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) de la classe [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) .</span><span class="sxs-lookup"><span data-stu-id="8f717-115">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="8f717-116">Remplissez le reste du formulaire et sélectionnez **créer**.</span><span class="sxs-lookup"><span data-stu-id="8f717-116">Fill out the rest of the form and select **Create**.</span></span> <span data-ttu-id="8f717-117">Les détails de la nouvelle application s’affichent :</span><span class="sxs-lookup"><span data-stu-id="8f717-117">New application details are displayed:</span></span>

## <a name="storing-twitter-consumer-api-key-and-secret"></a><span data-ttu-id="8f717-118">Stockage de la clé et du secret de l’API du consommateur Twitter</span><span class="sxs-lookup"><span data-stu-id="8f717-118">Storing Twitter Consumer API key and secret</span></span>

<span data-ttu-id="8f717-119">Exécutez les commandes suivantes pour stocker `ClientId` et `ClientSecret` en toute sécurité à l’aide du [Gestionnaire de secret](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="8f717-119">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerSecret <Secret>
```

<span data-ttu-id="8f717-120">Liez des paramètres sensibles comme Twitter `Consumer Key` et `Consumer Secret` à la configuration de votre application à l’aide du [Gestionnaire de secret](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="8f717-120">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="8f717-121">Dans le cadre de cet exemple, nommez les jetons `Authentication:Twitter:ConsumerKey` et `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="8f717-121">For the purposes of this sample, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="8f717-122">Ces jetons se trouvent sous l’onglet **clés et jetons d’accès** après la création d’une application Twitter :</span><span class="sxs-lookup"><span data-stu-id="8f717-122">These tokens can be found on the **Keys and Access Tokens** tab after creating a new Twitter application:</span></span>

## <a name="configure-twitter-authentication"></a><span data-ttu-id="8f717-123">Configurer l’authentification Twitter</span><span class="sxs-lookup"><span data-stu-id="8f717-123">Configure Twitter Authentication</span></span>

<span data-ttu-id="8f717-124">Ajoutez le service Twitter dans la méthode `ConfigureServices` du fichier *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="8f717-124">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupTwitter3x.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="8f717-125">Pour plus d’informations sur les options de configuration prises en charge par l’authentification Twitter, consultez la référence de l’API [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) .</span><span class="sxs-lookup"><span data-stu-id="8f717-125">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="8f717-126">Cela peut être utilisé pour demander différentes informations sur l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8f717-126">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="8f717-127">Se connecter avec Twitter</span><span class="sxs-lookup"><span data-stu-id="8f717-127">Sign in with Twitter</span></span>

<span data-ttu-id="8f717-128">Exécutez l’application et sélectionnez **se connecter**.</span><span class="sxs-lookup"><span data-stu-id="8f717-128">Run the app and select **Log in**.</span></span> <span data-ttu-id="8f717-129">Une option permettant de se connecter avec Twitter s’affiche :</span><span class="sxs-lookup"><span data-stu-id="8f717-129">An option to sign in with Twitter appears:</span></span>

<span data-ttu-id="8f717-130">En cliquant sur **Twitter** , vous redirigez vers Twitter pour l’authentification :</span><span class="sxs-lookup"><span data-stu-id="8f717-130">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

<span data-ttu-id="8f717-131">Après avoir entré vos informations d’identification Twitter, vous êtes redirigé vers le site Web où vous pouvez définir votre adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="8f717-131">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="8f717-132">Vous êtes maintenant connecté à l’aide de vos informations d’identification Twitter :</span><span class="sxs-lookup"><span data-stu-id="8f717-132">You are now logged in using your Twitter credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="8f717-133">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="8f717-133">Troubleshooting</span></span>

* <span data-ttu-id="8f717-134">**ASP.NET Core 2.x uniquement :** si identité n’est pas configurée en appelant `services.AddIdentity` dans `ConfigureServices`, toute tentative authentifier entraîne *ArgumentException : l’option 'SignInScheme' doit être fournie*.</span><span class="sxs-lookup"><span data-stu-id="8f717-134">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="8f717-135">Le modèle de projet utilisé dans cet exemple permet de s’assurer que cette opération est effectuée.</span><span class="sxs-lookup"><span data-stu-id="8f717-135">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="8f717-136">Si la base de données de site n’a pas été créé en appliquant la migration initiale, vous obtiendrez *une opération de base de données a échoué lors du traitement de la demande* erreur.</span><span class="sxs-lookup"><span data-stu-id="8f717-136">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="8f717-137">Appuyez sur **appliquer les Migrations** pour créer la base de données et actualiser pour passer à l’erreur.</span><span class="sxs-lookup"><span data-stu-id="8f717-137">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8f717-138">Étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="8f717-138">Next steps</span></span>

* <span data-ttu-id="8f717-139">Cet article vous a montré comment vous pouvez vous authentifier avec Twitter.</span><span class="sxs-lookup"><span data-stu-id="8f717-139">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="8f717-140">Vous pouvez suivre une approche similaire pour s’authentifier auprès d’autres fournisseurs répertoriés sur le [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="8f717-140">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="8f717-141">Une fois que vous publiez votre site Web dans l’application Web Azure, vous devez réinitialiser le `ConsumerSecret` dans le portail des développeurs Twitter.</span><span class="sxs-lookup"><span data-stu-id="8f717-141">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="8f717-142">Définir le `Authentication:Twitter:ConsumerKey` et `Authentication:Twitter:ConsumerSecret` en tant que paramètres d’application dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="8f717-142">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="8f717-143">Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="8f717-143">The configuration system is set up to read keys from environment variables.</span></span>
