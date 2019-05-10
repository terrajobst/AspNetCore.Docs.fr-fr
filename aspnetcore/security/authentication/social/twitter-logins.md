---
title: Twitter externe signe dans le programme d’installation avec ASP.NET Core
author: rick-anderson
description: Ce didacticiel montre l’intégration de l’authentification d’utilisateur de compte Twitter dans une application ASP.NET Core existante.
ms.author: riande
ms.custom: mvc
ms.date: 5/11/2019
uid: security/authentication/twitter-logins
ms.openlocfilehash: 486d58b600ca5326a0728de40bb386fbb9440f67
ms.sourcegitcommit: 3376f224b47a89acf329b2d2f9260046a372f924
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65516881"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a><span data-ttu-id="c5a3b-103">Twitter externe signe dans le programme d’installation avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c5a3b-103">Twitter external sign-in setup with ASP.NET Core</span></span>

<span data-ttu-id="c5a3b-104">Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c5a3b-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c5a3b-105">Cet exemple montre comment permettre aux utilisateurs de [vous connecter avec leur compte Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) à l’aide d’un exemple de projet ASP.NET Core 2.2 créés sur le [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="c5a3b-105">This sample shows how to enable users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="c5a3b-106">Créer l’application dans Twitter</span><span class="sxs-lookup"><span data-stu-id="c5a3b-106">Create the app in Twitter</span></span>

* <span data-ttu-id="c5a3b-107">Accédez à [ https://apps.twitter.com/ ](https://apps.twitter.com/) et s’y connecter.</span><span class="sxs-lookup"><span data-stu-id="c5a3b-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="c5a3b-108">Si vous ne disposez pas d’un compte Twitter, utilisez le **[s’inscrire maintenant](https://twitter.com/signup)** lien pour en créer un.</span><span class="sxs-lookup"><span data-stu-id="c5a3b-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span>

* <span data-ttu-id="c5a3b-109">Appuyez sur **créer une application** et remplissez la demande **nom**, **Description** et publics **site Web** URI (Cela peut être temporaire jusqu'à ce que vous Inscrivez le nom de domaine) :</span><span class="sxs-lookup"><span data-stu-id="c5a3b-109">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

* <span data-ttu-id="c5a3b-110">Entrez votre développement URI avec `/signin-twitter` ajoutées dans le **URI de redirection OAuth valides** champ (par exemple : `https://webapp128.azurewebsites.net/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="c5a3b-110">Enter your development URI with `/signin-twitter` appended into the **Valid OAuth Redirect URIs** field (for example: `https://webapp128.azurewebsites.net/signin-twitter`).</span></span> <span data-ttu-id="c5a3b-111">Le schéma d’authentification Twitter configuré plus loin dans cet exemple gère automatiquement les demandes à `/signin-twitter` itinéraire pour implémenter le flux OAuth.</span><span class="sxs-lookup"><span data-stu-id="c5a3b-111">The Twitter authentication scheme configured later in this sample will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="c5a3b-112">Le segment d’URI `/signin-twitter` est défini en tant que le rappel par défaut du fournisseur d’authentification Twitter.</span><span class="sxs-lookup"><span data-stu-id="c5a3b-112">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="c5a3b-113">Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel d’authentification de Twitter par le biais de l’élément hérité [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propriété de la [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) classe.</span><span class="sxs-lookup"><span data-stu-id="c5a3b-113">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="c5a3b-114">Remplissez le reste du formulaire, puis appuyez sur **créer votre application Twitter**.</span><span class="sxs-lookup"><span data-stu-id="c5a3b-114">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="c5a3b-115">Détails de l’application sont affichent :</span><span class="sxs-lookup"><span data-stu-id="c5a3b-115">New application details are displayed:</span></span>

## <a name="storing-twitter-consumer-api-key-and-secret"></a><span data-ttu-id="c5a3b-116">Stocker le secret et la clé d’API de consommateur Twitter</span><span class="sxs-lookup"><span data-stu-id="c5a3b-116">Storing Twitter Consumer API key and secret</span></span>

<span data-ttu-id="c5a3b-117">Exécutez les commandes suivantes pour stocker en toute sécurité `ClientId` et `ClientSecret` à l’aide de [Secret Manager](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="c5a3b-117">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```console
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerAPISecret <Secret>
```

<span data-ttu-id="c5a3b-118">Lier des paramètres sensibles comme Twitter `Consumer Key` et `Consumer Secret` à votre configuration d’application en utilisant le [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="c5a3b-118">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="c5a3b-119">Dans le cadre de cet exemple, nommez les jetons `Authentication:Twitter:ConsumerKey` et `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="c5a3b-119">For the purposes of this sample, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="c5a3b-120">Vous trouverez ces jetons sur le **Keys and Access Tokens** onglet après la création d’une application Twitter :</span><span class="sxs-lookup"><span data-stu-id="c5a3b-120">These tokens can be found on the **Keys and Access Tokens** tab after creating a new Twitter application:</span></span>

## <a name="configure-twitter-authentication"></a><span data-ttu-id="c5a3b-121">Configurer l’authentification Twitter</span><span class="sxs-lookup"><span data-stu-id="c5a3b-121">Configure Twitter Authentication</span></span>

<span data-ttu-id="c5a3b-122">Ajoutez le service Twitter dans le `ConfigureServices` méthode dans *Startup.cs* fichier :</span><span class="sxs-lookup"><span data-stu-id="c5a3b-122">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupTwitter.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="c5a3b-123">Consultez le [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) référence des API pour plus d’informations sur les options de configuration prises en charge par l’authentification Twitter.</span><span class="sxs-lookup"><span data-stu-id="c5a3b-123">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="c5a3b-124">Cela peut être utilisé pour demander différentes informations sur l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="c5a3b-124">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="c5a3b-125">Connectez-vous à Twitter</span><span class="sxs-lookup"><span data-stu-id="c5a3b-125">Sign in with Twitter</span></span>

<span data-ttu-id="c5a3b-126">Exécutez l’application et sélectionnez **connectez-vous**.</span><span class="sxs-lookup"><span data-stu-id="c5a3b-126">Run the app and select **Log in**.</span></span> <span data-ttu-id="c5a3b-127">Une option pour vous connecter avec Twitter s’affiche :</span><span class="sxs-lookup"><span data-stu-id="c5a3b-127">An option to sign in with Twitter appears:</span></span>

<span data-ttu-id="c5a3b-128">En cliquant sur **Twitter** redirige vers Twitter pour l’authentification :</span><span class="sxs-lookup"><span data-stu-id="c5a3b-128">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

<span data-ttu-id="c5a3b-129">Après avoir entré vos informations d’identification Twitter, vous êtes redirigé vers le site web où vous pouvez définir votre adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="c5a3b-129">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="c5a3b-130">Vous êtes maintenant connecté à l’aide de vos informations d’identification Twitter :</span><span class="sxs-lookup"><span data-stu-id="c5a3b-130">You are now logged in using your Twitter credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="c5a3b-131">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="c5a3b-131">Troubleshooting</span></span>

* <span data-ttu-id="c5a3b-132">**ASP.NET Core 2.x uniquement :** Si l’identité n’est pas configurée en appelant `services.AddIdentity` dans `ConfigureServices`, toute tentative authentifier entraîne *ArgumentException : L’option 'SignInScheme' doit être fournie*.</span><span class="sxs-lookup"><span data-stu-id="c5a3b-132">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="c5a3b-133">Le modèle de projet utilisé dans cet exemple permet de s’assurer que cela est fait.</span><span class="sxs-lookup"><span data-stu-id="c5a3b-133">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="c5a3b-134">Si la base de données de site n’a pas été créé en appliquant la migration initiale, vous obtiendrez *une opération de base de données a échoué lors du traitement de la demande* erreur.</span><span class="sxs-lookup"><span data-stu-id="c5a3b-134">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="c5a3b-135">Appuyez sur **appliquer les Migrations** pour créer la base de données et actualiser pour passer à l’erreur.</span><span class="sxs-lookup"><span data-stu-id="c5a3b-135">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c5a3b-136">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="c5a3b-136">Next steps</span></span>

* <span data-ttu-id="c5a3b-137">Cet article vous a montré comment vous pouvez vous authentifier avec Twitter.</span><span class="sxs-lookup"><span data-stu-id="c5a3b-137">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="c5a3b-138">Vous pouvez suivre une approche similaire pour s’authentifier auprès d’autres fournisseurs répertoriés sur le [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="c5a3b-138">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="c5a3b-139">Une fois que vous publiez votre site web à l’application web Azure, vous devez réinitialiser le `ConsumerSecret` dans le portail des développeurs Twitter.</span><span class="sxs-lookup"><span data-stu-id="c5a3b-139">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="c5a3b-140">Définir le `Authentication:Twitter:ConsumerKey` et `Authentication:Twitter:ConsumerSecret` en tant que paramètres d’application dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="c5a3b-140">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="c5a3b-141">Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="c5a3b-141">The configuration system is set up to read keys from environment variables.</span></span>