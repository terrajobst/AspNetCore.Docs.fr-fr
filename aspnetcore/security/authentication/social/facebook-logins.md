---
title: Programme d’installation de Facebook login externe dans ASP.NET Core
author: rick-anderson
description: Ce didacticiel montre l’intégration de l’authentification d’utilisateur de compte Facebook dans une application ASP.NET Core existante.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/11/2018
uid: security/authentication/facebook-logins
ms.openlocfilehash: d4f3e210b0d3c79eaf2233f97a29a6d96cd69b39
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284381"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a><span data-ttu-id="360f9-103">Programme d’installation de Facebook login externe dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="360f9-103">Facebook external login setup in ASP.NET Core</span></span>

<span data-ttu-id="360f9-104">Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="360f9-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="360f9-105">Ce didacticiel vous montre comment autoriser vos utilisateurs à se connecter avec leur compte Facebook à l’aide d’un exemple de projet ASP.NET Core 2.0 créée sur le [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="360f9-105">This tutorial shows you how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="360f9-106">L’authentification Facebook nécessite le [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package NuGet.</span><span class="sxs-lookup"><span data-stu-id="360f9-106">Facebook authentication requires the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) NuGet package.</span></span> <span data-ttu-id="360f9-107">Nous commençons par créer un ID d’application Facebook en suivant le [étapes officiels](https://developers.facebook.com).</span><span class="sxs-lookup"><span data-stu-id="360f9-107">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="360f9-108">Créer l’application dans Facebook</span><span class="sxs-lookup"><span data-stu-id="360f9-108">Create the app in Facebook</span></span>

* <span data-ttu-id="360f9-109">Accédez à la [développeurs Facebook app](https://developers.facebook.com/apps/) page et s’y connecter.</span><span class="sxs-lookup"><span data-stu-id="360f9-109">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="360f9-110">Si vous ne disposez pas d’un compte Facebook, utilisez le **s’inscrire à Facebook** sur la page de connexion pour créer un lien.</span><span class="sxs-lookup"><span data-stu-id="360f9-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="360f9-111">Appuyez sur la **ajouter une nouvelle application** bouton dans le coin supérieur droit pour créer un ID d’application.</span><span class="sxs-lookup"><span data-stu-id="360f9-111">Tap the **Add a New App** button in the upper right corner to create a new App ID.</span></span>

   ![Facebook pour le portail des développeurs ouverte dans Microsoft Edge](index/_static/FBMyApps.png)

* <span data-ttu-id="360f9-113">Remplissez le formulaire, puis appuyez sur la **Create App ID** bouton.</span><span class="sxs-lookup"><span data-stu-id="360f9-113">Fill out the form and tap the **Create App ID** button.</span></span>

  ![Créer un formulaire de nouvel ID d’application](index/_static/FBNewAppId.png)

* <span data-ttu-id="360f9-115">Sur le **sélectionner un produit** , cliquez sur **Set Up** sur le **connexion Facebook** carte.</span><span class="sxs-lookup"><span data-stu-id="360f9-115">On the **Select a product** page, click **Set Up** on the **Facebook Login** card.</span></span>

  ![Page d’installation du produit](index/_static/FBProductSetup.png)

* <span data-ttu-id="360f9-117">Le **Quickstart** Assistant lancera avec **choisir une plate-forme** en tant que la première page.</span><span class="sxs-lookup"><span data-stu-id="360f9-117">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="360f9-118">Ignorer l’Assistant pour l’instant en cliquant sur le **paramètres** lien dans le menu de gauche :</span><span class="sxs-lookup"><span data-stu-id="360f9-118">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

  ![Démarrage rapide de Skip](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="360f9-120">Vous voyez la **paramètres du Client OAuth** page :</span><span class="sxs-lookup"><span data-stu-id="360f9-120">You are presented with the **Client OAuth Settings** page:</span></span>

  ![Page Paramètres OAuth du client](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="360f9-122">Entrez votre développement URI avec */signin-facebook* ajoutées dans le **URI de redirection OAuth valides** champ (par exemple : `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="360f9-122">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="360f9-123">L’authentification Facebook configurée plus loin dans ce didacticiel gère automatiquement les demandes à */signin-facebook* itinéraire pour implémenter le flux OAuth.</span><span class="sxs-lookup"><span data-stu-id="360f9-123">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="360f9-124">L’URI */signin-facebook* est défini en tant que le rappel par défaut du fournisseur d’authentification Facebook.</span><span class="sxs-lookup"><span data-stu-id="360f9-124">The URI */signin-facebook* is set as the default callback of the Facebook authentication provider.</span></span> <span data-ttu-id="360f9-125">Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel d’authentification Facebook via héritées [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propriété de la [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) classe.</span><span class="sxs-lookup"><span data-stu-id="360f9-125">You can change the default callback URI while configuring the Facebook authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) class.</span></span>

* <span data-ttu-id="360f9-126">Cliquez sur **enregistrer les modifications**.</span><span class="sxs-lookup"><span data-stu-id="360f9-126">Click **Save Changes**.</span></span>

* <span data-ttu-id="360f9-127">Cliquez sur **paramètres** > **base** lien dans le volet de navigation gauche.</span><span class="sxs-lookup"><span data-stu-id="360f9-127">Click **Settings** > **Basic** link in the left navigation.</span></span>

  <span data-ttu-id="360f9-128">Dans cette page, prenez note de votre `App ID` et votre `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="360f9-128">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="360f9-129">Vous allez ajouter à la fois dans votre application ASP.NET Core dans la section suivante :</span><span class="sxs-lookup"><span data-stu-id="360f9-129">You will add both into your ASP.NET Core application in the next section:</span></span>

* <span data-ttu-id="360f9-130">Lorsque vous déployez le site dont vous avez besoin de revenir sur le **connexion Facebook** page d’installation et inscrire un nouvel URI public.</span><span class="sxs-lookup"><span data-stu-id="360f9-130">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="360f9-131">ID d’application de Store Facebook et Secret d’application</span><span class="sxs-lookup"><span data-stu-id="360f9-131">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="360f9-132">Lier des paramètres sensibles comme Facebook `App ID` et `App Secret` à votre configuration d’application en utilisant le [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="360f9-132">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="360f9-133">Dans le cadre de ce didacticiel, nommez les jetons `Authentication:Facebook:AppId` et `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="360f9-133">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

<span data-ttu-id="360f9-134">Exécutez les commandes suivantes à stocker en toute sécurité `App ID` et `App Secret` à l’aide de Secret Manager :</span><span class="sxs-lookup"><span data-stu-id="360f9-134">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="360f9-135">Configurer l’authentification Facebook</span><span class="sxs-lookup"><span data-stu-id="360f9-135">Configure Facebook Authentication</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="360f9-136">Ajoutez le service de Facebook dans le `ConfigureServices` méthode dans le *Startup.cs* fichier :</span><span class="sxs-lookup"><span data-stu-id="360f9-136">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="360f9-137">Installer le [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package.</span><span class="sxs-lookup"><span data-stu-id="360f9-137">Install the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package.</span></span>

* <span data-ttu-id="360f9-138">Pour installer ce package avec Visual Studio 2017, cliquez sur le projet, puis sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="360f9-138">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="360f9-139">Pour installer avec l’interface CLI .NET Core, exécutez le code suivant dans votre répertoire de projet :</span><span class="sxs-lookup"><span data-stu-id="360f9-139">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

<span data-ttu-id="360f9-140">Ajoutez l’intergiciel (middleware) Facebook dans le `Configure` méthode dans *Startup.cs* fichier :</span><span class="sxs-lookup"><span data-stu-id="360f9-140">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

::: moniker-end

<span data-ttu-id="360f9-141">Consultez le [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) référence des API pour plus d’informations sur les options de configuration prises en charge par l’authentification Facebook.</span><span class="sxs-lookup"><span data-stu-id="360f9-141">See the [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="360f9-142">Options de configuration peuvent être utilisées pour :</span><span class="sxs-lookup"><span data-stu-id="360f9-142">Configuration options can be used to:</span></span>

* <span data-ttu-id="360f9-143">Demander différentes informations sur l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="360f9-143">Request different information about the user.</span></span>
* <span data-ttu-id="360f9-144">Ajouter des arguments de chaîne de requête pour personnaliser l’expérience de connexion.</span><span class="sxs-lookup"><span data-stu-id="360f9-144">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="360f9-145">Se connecter avec Facebook</span><span class="sxs-lookup"><span data-stu-id="360f9-145">Sign in with Facebook</span></span>

<span data-ttu-id="360f9-146">Exécutez votre application et cliquez sur **connectez-vous**.</span><span class="sxs-lookup"><span data-stu-id="360f9-146">Run your application and click **Log in**.</span></span> <span data-ttu-id="360f9-147">Vous voyez une option pour vous connecter avec Facebook.</span><span class="sxs-lookup"><span data-stu-id="360f9-147">You see an option to sign in with Facebook.</span></span>

![Application Web : Utilisateur non authentifié](index/_static/DoneFacebook.png)

<span data-ttu-id="360f9-149">Lorsque vous cliquez sur **Facebook**, vous êtes redirigé vers Facebook pour l’authentification :</span><span class="sxs-lookup"><span data-stu-id="360f9-149">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Page d’authentification Facebook](index/_static/FBLogin.png)

<span data-ttu-id="360f9-151">Les demandes d’authentification de Facebook publique profil et adresse de messagerie par défaut :</span><span class="sxs-lookup"><span data-stu-id="360f9-151">Facebook authentication requests public profile and email address by default:</span></span>

![Écran de consentement de page de l’authentification Facebook](index/_static/FBLoginDone.png)

<span data-ttu-id="360f9-153">Une fois que vous entrez vos informations d’identification Facebook, vous êtes redirigé vers votre site où vous pouvez définir votre adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="360f9-153">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="360f9-154">Vous êtes maintenant connecté à l’aide de vos informations d’identification Facebook :</span><span class="sxs-lookup"><span data-stu-id="360f9-154">You are now logged in using your Facebook credentials:</span></span>

![Application Web : Utilisateur authentifié](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="360f9-156">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="360f9-156">Troubleshooting</span></span>

* <span data-ttu-id="360f9-157">**ASP.NET Core 2.x uniquement :** Si l’identité n’est pas configurée en appelant `services.AddIdentity` dans `ConfigureServices`, toute tentative authentifier entraîne *ArgumentException : L’option 'SignInScheme' doit être fournie*.</span><span class="sxs-lookup"><span data-stu-id="360f9-157">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="360f9-158">Le modèle de projet utilisé dans ce didacticiel permet de s’assurer que cela est fait.</span><span class="sxs-lookup"><span data-stu-id="360f9-158">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="360f9-159">Si la base de données de site n’a pas été créé en appliquant la migration initiale, vous obtenez *une opération de base de données a échoué lors du traitement de la demande* erreur.</span><span class="sxs-lookup"><span data-stu-id="360f9-159">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="360f9-160">Appuyez sur **appliquer les Migrations** pour créer la base de données et actualiser pour passer à l’erreur.</span><span class="sxs-lookup"><span data-stu-id="360f9-160">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="360f9-161">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="360f9-161">Next steps</span></span>

* <span data-ttu-id="360f9-162">Cet article vous a montré comment vous pouvez vous authentifier avec Facebook.</span><span class="sxs-lookup"><span data-stu-id="360f9-162">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="360f9-163">Vous pouvez suivre une approche similaire pour s’authentifier auprès d’autres fournisseurs répertoriés sur le [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="360f9-163">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="360f9-164">Une fois que vous publiez votre site web à l’application web Azure, vous devez réinitialiser le `AppSecret` dans le portail des développeurs Facebook.</span><span class="sxs-lookup"><span data-stu-id="360f9-164">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="360f9-165">Définir le `Authentication:Facebook:AppId` et `Authentication:Facebook:AppSecret` en tant que paramètres d’application dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="360f9-165">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="360f9-166">Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="360f9-166">The configuration system is set up to read keys from environment variables.</span></span>
