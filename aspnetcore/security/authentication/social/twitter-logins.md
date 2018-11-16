---
title: Programme d’installation de la connexion externe Twitter avec ASP.NET Core
author: rick-anderson
description: Ce didacticiel montre l’intégration de l’authentification d’utilisateur de compte Twitter dans une application ASP.NET Core existante.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/twitter-logins
ms.openlocfilehash: 43a5ea59d8853d297ae2c1ec3f4b1c0c14ec80c3
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708424"
---
# <a name="twitter-external-login-setup-with-aspnet-core"></a><span data-ttu-id="fba3a-103">Programme d’installation de la connexion externe Twitter avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fba3a-103">Twitter external login setup with ASP.NET Core</span></span>

<span data-ttu-id="fba3a-104">Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fba3a-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fba3a-105">Ce didacticiel vous montre comment autoriser vos utilisateurs à [vous connecter avec leur compte Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) à l’aide d’un exemple de projet ASP.NET Core 2.0 créée sur le [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="fba3a-105">This tutorial shows you how to enable your users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="fba3a-106">Créer l’application dans Twitter</span><span class="sxs-lookup"><span data-stu-id="fba3a-106">Create the app in Twitter</span></span>

* <span data-ttu-id="fba3a-107">Accédez à [ https://apps.twitter.com/ ](https://apps.twitter.com/) et s’y connecter.</span><span class="sxs-lookup"><span data-stu-id="fba3a-107">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="fba3a-108">Si vous ne disposez pas d’un compte Twitter, utilisez le **[s’inscrire maintenant](https://twitter.com/signup)** lien pour en créer un.</span><span class="sxs-lookup"><span data-stu-id="fba3a-108">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span> <span data-ttu-id="fba3a-109">Après vous être connecté, le **gestion des applications** page est affichée :</span><span class="sxs-lookup"><span data-stu-id="fba3a-109">After signing in, the **Application Management** page is shown:</span></span>

  ![Gestion des applications Twitter ouvert dans Microsoft Edge](index/_static/TwitterAppManage.png)

* <span data-ttu-id="fba3a-111">Appuyez sur **créer une application** et remplissez la demande **nom**, **Description** et publics **site Web** URI (Cela peut être temporaire jusqu'à ce que vous Inscrivez le nom de domaine) :</span><span class="sxs-lookup"><span data-stu-id="fba3a-111">Tap **Create New App** and fill out the application **Name**, **Description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

  ![Créer une page d’application](index/_static/TwitterCreate.png)

* <span data-ttu-id="fba3a-113">Entrez votre développement URI avec `/signin-twitter` ajoutées dans le **URI de redirection OAuth valides** champ (par exemple : `https://localhost:44320/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="fba3a-113">Enter your development URI with `/signin-twitter` appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-twitter`).</span></span> <span data-ttu-id="fba3a-114">Le schéma d’authentification Twitter configuré plus loin dans ce didacticiel gère automatiquement les demandes à `/signin-twitter` itinéraire pour implémenter le flux OAuth.</span><span class="sxs-lookup"><span data-stu-id="fba3a-114">The Twitter authentication scheme configured later in this tutorial will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="fba3a-115">Le segment d’URI `/signin-twitter` est défini en tant que le rappel par défaut du fournisseur d’authentification Twitter.</span><span class="sxs-lookup"><span data-stu-id="fba3a-115">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="fba3a-116">Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel d’authentification de Twitter par le biais de l’élément hérité [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propriété de la [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) classe.</span><span class="sxs-lookup"><span data-stu-id="fba3a-116">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="fba3a-117">Remplissez le reste du formulaire, puis appuyez sur **créer votre application Twitter**.</span><span class="sxs-lookup"><span data-stu-id="fba3a-117">Fill out the rest of the form and tap **Create your Twitter application**.</span></span> <span data-ttu-id="fba3a-118">Détails de l’application sont affichent :</span><span class="sxs-lookup"><span data-stu-id="fba3a-118">New application details are displayed:</span></span>

  ![Onglet Détails sur la page d’Application](index/_static/TwitterAppDetails.png)

* <span data-ttu-id="fba3a-120">Lorsque vous déployez le site, vous devez revoir la **gestion des applications** page et inscrire un nouvel URI public.</span><span class="sxs-lookup"><span data-stu-id="fba3a-120">When deploying the site you'll need to revisit the **Application Management** page and register a new public URI.</span></span>

## <a name="storing-twitter-consumerkey-and-consumersecret"></a><span data-ttu-id="fba3a-121">Stockage Twitter ConsumerKey et ConsumerSecret</span><span class="sxs-lookup"><span data-stu-id="fba3a-121">Storing Twitter ConsumerKey and ConsumerSecret</span></span>

<span data-ttu-id="fba3a-122">Lier des paramètres sensibles comme Twitter `Consumer Key` et `Consumer Secret` à votre configuration d’application en utilisant le [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="fba3a-122">Link sensitive settings like Twitter `Consumer Key` and `Consumer Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="fba3a-123">Dans le cadre de ce didacticiel, nommez les jetons `Authentication:Twitter:ConsumerKey` et `Authentication:Twitter:ConsumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="fba3a-123">For the purposes of this tutorial, name the tokens `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`.</span></span>

<span data-ttu-id="fba3a-124">Vous trouverez ces jetons sur le **Keys and Access Tokens** onglet après avoir créé votre nouvelle application Twitter :</span><span class="sxs-lookup"><span data-stu-id="fba3a-124">These tokens can be found on the **Keys and Access Tokens** tab after creating your new Twitter application:</span></span>

![Onglet clés et jetons d’accès](index/_static/TwitterKeys.png)

## <a name="configure-twitter-authentication"></a><span data-ttu-id="fba3a-126">Configurer l’authentification Twitter</span><span class="sxs-lookup"><span data-stu-id="fba3a-126">Configure Twitter Authentication</span></span>

<span data-ttu-id="fba3a-127">Le modèle de projet utilisé dans ce didacticiel s’assure que [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package est déjà installé.</span><span class="sxs-lookup"><span data-stu-id="fba3a-127">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter) package is already installed.</span></span>

* <span data-ttu-id="fba3a-128">Pour installer ce package avec Visual Studio 2017, cliquez sur le projet, puis sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="fba3a-128">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="fba3a-129">Pour installer avec l’interface CLI .NET Core, exécutez le code suivant dans votre répertoire de projet :</span><span class="sxs-lookup"><span data-stu-id="fba3a-129">To install with .NET Core CLI, execute the following in your project directory:</span></span>

  `dotnet add package Microsoft.AspNetCore.Authentication.Twitter`

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="fba3a-130">Ajoutez le service Twitter dans le `ConfigureServices` méthode dans *Startup.cs* fichier :</span><span class="sxs-lookup"><span data-stu-id="fba3a-130">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddTwitter(twitterOptions =>
{
    twitterOptions.ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"];
    twitterOptions.ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="fba3a-131">Ajoutez l’intergiciel (middleware) Twitter dans le `Configure` méthode dans *Startup.cs* fichier :</span><span class="sxs-lookup"><span data-stu-id="fba3a-131">Add the Twitter middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseTwitterAuthentication(new TwitterOptions()
{
    ConsumerKey = Configuration["Authentication:Twitter:ConsumerKey"],
    ConsumerSecret = Configuration["Authentication:Twitter:ConsumerSecret"]
});
```

::: moniker-end

<span data-ttu-id="fba3a-132">Consultez le [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) référence des API pour plus d’informations sur les options de configuration prises en charge par l’authentification Twitter.</span><span class="sxs-lookup"><span data-stu-id="fba3a-132">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="fba3a-133">Cela peut être utilisé pour demander différentes informations sur l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="fba3a-133">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="fba3a-134">Connectez-vous à Twitter</span><span class="sxs-lookup"><span data-stu-id="fba3a-134">Sign in with Twitter</span></span>

<span data-ttu-id="fba3a-135">Exécutez votre application et cliquez sur **connectez-vous**.</span><span class="sxs-lookup"><span data-stu-id="fba3a-135">Run your application and click **Log in**.</span></span> <span data-ttu-id="fba3a-136">Une option pour vous connecter avec Twitter s’affiche :</span><span class="sxs-lookup"><span data-stu-id="fba3a-136">An option to sign in with Twitter appears:</span></span>

![Application Web : utilisateur non authentifié](index/_static/DoneTwitter.png)

<span data-ttu-id="fba3a-138">En cliquant sur **Twitter** redirige vers Twitter pour l’authentification :</span><span class="sxs-lookup"><span data-stu-id="fba3a-138">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

![Page d’authentification Twitter](index/_static/TwitterLogin.png)

<span data-ttu-id="fba3a-140">Après avoir entré vos informations d’identification Twitter, vous êtes redirigé vers le site web où vous pouvez définir votre adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="fba3a-140">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="fba3a-141">Vous êtes maintenant connecté à l’aide de vos informations d’identification Twitter :</span><span class="sxs-lookup"><span data-stu-id="fba3a-141">You are now logged in using your Twitter credentials:</span></span>

![Application Web : utilisateur authentifié](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="fba3a-143">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="fba3a-143">Troubleshooting</span></span>

* <span data-ttu-id="fba3a-144">**ASP.NET Core 2.x uniquement :** si identité n’est pas configurée en appelant `services.AddIdentity` dans `ConfigureServices`, toute tentative authentifier entraîne *ArgumentException : l’option 'SignInScheme' doit être fournie*.</span><span class="sxs-lookup"><span data-stu-id="fba3a-144">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="fba3a-145">Le modèle de projet utilisé dans ce didacticiel permet de s’assurer que cela est fait.</span><span class="sxs-lookup"><span data-stu-id="fba3a-145">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="fba3a-146">Si la base de données de site n’a pas été créé en appliquant la migration initiale, vous obtiendrez *une opération de base de données a échoué lors du traitement de la demande* erreur.</span><span class="sxs-lookup"><span data-stu-id="fba3a-146">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="fba3a-147">Appuyez sur **appliquer les Migrations** pour créer la base de données et actualiser pour passer à l’erreur.</span><span class="sxs-lookup"><span data-stu-id="fba3a-147">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fba3a-148">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="fba3a-148">Next steps</span></span>

* <span data-ttu-id="fba3a-149">Cet article vous a montré comment vous pouvez vous authentifier avec Twitter.</span><span class="sxs-lookup"><span data-stu-id="fba3a-149">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="fba3a-150">Vous pouvez suivre une approche similaire pour s’authentifier auprès d’autres fournisseurs répertoriés sur le [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="fba3a-150">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="fba3a-151">Une fois que vous publiez votre site web à l’application web Azure, vous devez réinitialiser le `ConsumerSecret` dans le portail des développeurs Twitter.</span><span class="sxs-lookup"><span data-stu-id="fba3a-151">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="fba3a-152">Définir le `Authentication:Twitter:ConsumerKey` et `Authentication:Twitter:ConsumerSecret` en tant que paramètres d’application dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="fba3a-152">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="fba3a-153">Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="fba3a-153">The configuration system is set up to read keys from environment variables.</span></span>
