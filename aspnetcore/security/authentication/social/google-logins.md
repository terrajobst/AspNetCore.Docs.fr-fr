---
title: Programme d’installation de la connexion externe Google dans ASP.NET Core
author: rick-anderson
description: Ce didacticiel montre l’intégration de l’authentification d’utilisateur de compte Google dans une application ASP.NET Core existante.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/11/2018
uid: security/authentication/google-logins
ms.openlocfilehash: e5deda5d521643e3155be00f4630a86c6a82575c
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121529"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="b43ed-103">Programme d’installation de la connexion externe Google dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b43ed-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="b43ed-104">Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b43ed-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b43ed-105">Ce didacticiel vous montre comment autoriser vos utilisateurs à se connecter avec leur compte Google + à l’aide d’un exemple de projet ASP.NET Core 2.0 créée sur le [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="b43ed-105">This tutorial shows you how to enable your users to sign in with their Google+ account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span> <span data-ttu-id="b43ed-106">Nous commençons en suivant le [étapes officiels](https://developers.google.com/identity/sign-in/web/devconsole-project) pour créer une nouvelle application dans la Console d’API Google.</span><span class="sxs-lookup"><span data-stu-id="b43ed-106">We start by following the [official steps](https://developers.google.com/identity/sign-in/web/devconsole-project) to create a new app in Google API Console.</span></span>

## <a name="create-the-app-in-google-api-console"></a><span data-ttu-id="b43ed-107">Créer l’application dans la Console d’API Google</span><span class="sxs-lookup"><span data-stu-id="b43ed-107">Create the app in Google API Console</span></span>

* <span data-ttu-id="b43ed-108">Accédez à [ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library) et s’y connecter.</span><span class="sxs-lookup"><span data-stu-id="b43ed-108">Navigate to [https://console.developers.google.com/projectselector/apis/library](https://console.developers.google.com/projectselector/apis/library) and sign in.</span></span> <span data-ttu-id="b43ed-109">Si vous ne disposez pas d’un compte Google, utilisez **davantage d’options** > **[créer compte](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  lien pour en créer un :</span><span class="sxs-lookup"><span data-stu-id="b43ed-109">If you don't already have a Google account, use **More options** > **[Create account](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)** link to create one:</span></span>

![Console d’API Google](index/_static/GoogleConsoleLogin.png)

* <span data-ttu-id="b43ed-111">Vous êtes redirigé vers **bibliothèque API Manager** page :</span><span class="sxs-lookup"><span data-stu-id="b43ed-111">You are redirected to **API Manager Library** page:</span></span>

![Dans la page de la bibliothèque du Gestionnaire de l’API de lancement](index/_static/GoogleConsoleSwitchboard.png)

* <span data-ttu-id="b43ed-113">Appuyez sur **créer** et entrez votre **nom_projet**:</span><span class="sxs-lookup"><span data-stu-id="b43ed-113">Tap **Create** and enter your **Project name**:</span></span>

![Boîte de dialogue Nouveau projet](index/_static/GoogleConsoleNewProj.png)

* <span data-ttu-id="b43ed-115">Après avoir accepté la boîte de dialogue, vous êtes redirigé vers la page de bibliothèque vous permettant de choisir des fonctionnalités pour votre nouvelle application.</span><span class="sxs-lookup"><span data-stu-id="b43ed-115">After accepting the dialog, you are redirected back to the Library page allowing you to choose features for your new app.</span></span> <span data-ttu-id="b43ed-116">Rechercher **API Google +** dans la liste et cliquez sur son lien pour ajouter la fonctionnalité d’API :</span><span class="sxs-lookup"><span data-stu-id="b43ed-116">Find **Google+ API** in the list and click on its link to add the API feature:</span></span>

![Recherchez « API Google + » dans la page de la bibliothèque API Manager](index/_static/GoogleConsoleChooseApi.png)

* <span data-ttu-id="b43ed-118">La page de l’API nouvellement ajoutée s’affiche.</span><span class="sxs-lookup"><span data-stu-id="b43ed-118">The page for the newly added API is displayed.</span></span> <span data-ttu-id="b43ed-119">Appuyez sur **activer** pour ajouter Google + signe fonctionnalité à votre application :</span><span class="sxs-lookup"><span data-stu-id="b43ed-119">Tap **Enable** to add Google+ sign in feature to your app:</span></span>

![Sur la page Gestionnaire de l’API Google + API de lancement](index/_static/GoogleConsoleEnableApi.png)

* <span data-ttu-id="b43ed-121">Après l’activation de l’API, appuyez sur **créer les informations d’identification** pour configurer les clés secrètes :</span><span class="sxs-lookup"><span data-stu-id="b43ed-121">After enabling the API, tap **Create credentials** to configure the secrets:</span></span>

![Créer un bouton informations d’identification sur la page Gestionnaire de l’API Google + API](index/_static/GoogleConsoleGoCredentials.png)

* <span data-ttu-id="b43ed-123">Choisissez :</span><span class="sxs-lookup"><span data-stu-id="b43ed-123">Choose:</span></span>
  * <span data-ttu-id="b43ed-124">**Google + API**</span><span class="sxs-lookup"><span data-stu-id="b43ed-124">**Google+ API**</span></span>
  * <span data-ttu-id="b43ed-125">**Serveur Web (par exemple, node.js, Tomcat)**, et</span><span class="sxs-lookup"><span data-stu-id="b43ed-125">**Web server (e.g. node.js, Tomcat)**, and</span></span>
  * <span data-ttu-id="b43ed-126">**Données utilisateur**:</span><span class="sxs-lookup"><span data-stu-id="b43ed-126">**User data**:</span></span>

![Page informations d’identification de l’API Manager : Découvrez quel type d’informations d’identification vous avez besoin de panneau](index/_static/GoogleConsoleChooseCred.png)

* <span data-ttu-id="b43ed-128">Appuyez sur **les informations d’identification ai-je besoin ?** afin d’accéder à la deuxième étape de configuration de l’application, **créer un ID de client OAuth 2.0**:</span><span class="sxs-lookup"><span data-stu-id="b43ed-128">Tap **What credentials do I need?** which takes you to the second step of app configuration, **Create an OAuth 2.0 client ID**:</span></span>

![Page informations d’identification de l’API Manager : créer un ID de client OAuth 2.0](index/_static/GoogleConsoleCreateClient.png)

* <span data-ttu-id="b43ed-130">Étant donné que nous créons un projet Google + avec une seule caractéristique (connexion), nous pouvons saisir les mêmes **nom** pour l’ID de client OAuth 2.0 que celui que nous avons utilisé pour le projet.</span><span class="sxs-lookup"><span data-stu-id="b43ed-130">Because we are creating a Google+ project with just one feature (sign in), we can enter the same **Name** for the OAuth 2.0 client ID as the one we used for the project.</span></span>

* <span data-ttu-id="b43ed-131">Entrez votre développement URI avec `/signin-google` ajoutées dans le **URI de redirection autorisée** champ (par exemple : `https://localhost:44320/signin-google`).</span><span class="sxs-lookup"><span data-stu-id="b43ed-131">Enter your development URI with `/signin-google` appended into the **Authorized redirect URIs** field (for example: `https://localhost:44320/signin-google`).</span></span> <span data-ttu-id="b43ed-132">L’authentification Google configurée plus loin dans ce didacticiel gère automatiquement les demandes à `/signin-google` itinéraire pour implémenter le flux OAuth.</span><span class="sxs-lookup"><span data-stu-id="b43ed-132">The Google authentication configured later in this tutorial will automatically handle requests at `/signin-google` route to implement the OAuth flow.</span></span>

> [!NOTE]
> <span data-ttu-id="b43ed-133">Le segment d’URI `/signin-google` est défini en tant que le rappel par défaut du fournisseur d’authentification Google.</span><span class="sxs-lookup"><span data-stu-id="b43ed-133">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="b43ed-134">Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel d’authentification Google via héritées [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propriété de la [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) classe.</span><span class="sxs-lookup"><span data-stu-id="b43ed-134">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

* <span data-ttu-id="b43ed-135">Appuyez sur TAB pour ajouter le **URI de redirection autorisée** entrée.</span><span class="sxs-lookup"><span data-stu-id="b43ed-135">Press TAB to add the **Authorized redirect URIs** entry.</span></span>

* <span data-ttu-id="b43ed-136">Appuyez sur **créer un identifiant client**, ce qui vous amène à la troisième étape, **configurer à l’écran de consentement OAuth 2.0**:</span><span class="sxs-lookup"><span data-stu-id="b43ed-136">Tap **Create client ID**, which takes you to the third step, **Set up the OAuth 2.0 consent screen**:</span></span>

![Page informations d’identification de l’API Manager : configurer l’écran de consentement OAuth 2.0](index/_static/GoogleConsoleAddCred.png)

* <span data-ttu-id="b43ed-138">Entrez votre publics **adresse de messagerie** et **Product name** indiqué pour votre application lorsque Google + invite l’utilisateur à se connecter.</span><span class="sxs-lookup"><span data-stu-id="b43ed-138">Enter your public facing **Email address** and the **Product name** shown for your app when Google+ prompts the user to sign in.</span></span> <span data-ttu-id="b43ed-139">Options supplémentaires sont disponibles sous **les options de personnalisation plus**.</span><span class="sxs-lookup"><span data-stu-id="b43ed-139">Additional options are available under **More customization options**.</span></span>

* <span data-ttu-id="b43ed-140">Appuyez sur **continuer** pour passer à la dernière étape, **télécharger les informations d’identification**:</span><span class="sxs-lookup"><span data-stu-id="b43ed-140">Tap **Continue** to proceed to the last step, **Download credentials**:</span></span>

![Page informations d’identification de l’API Manager : télécharger les informations d’identification](index/_static/GoogleConsoleFinish.png)

* <span data-ttu-id="b43ed-142">Appuyez sur **télécharger** pour enregistrer un fichier JSON avec des secrets d’application, et **fait** pour terminer la création de la nouvelle application.</span><span class="sxs-lookup"><span data-stu-id="b43ed-142">Tap **Download** to save a JSON file with application secrets, and **Done** to complete creation of the new app.</span></span>

* <span data-ttu-id="b43ed-143">Lorsque vous déployez le site, vous devez revoir la **Google Console** et inscrire une nouvelle url publique.</span><span class="sxs-lookup"><span data-stu-id="b43ed-143">When deploying the site you'll need to revisit the **Google Console** and register a new public url.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="b43ed-144">Store Google ClientID et ClientSecret</span><span class="sxs-lookup"><span data-stu-id="b43ed-144">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="b43ed-145">Lier des paramètres sensibles comme Google `Client ID` et `Client Secret` à votre configuration d’application en utilisant le [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="b43ed-145">Link sensitive settings like Google `Client ID` and `Client Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="b43ed-146">Dans le cadre de ce didacticiel, nommez les jetons `Authentication:Google:ClientId` et `Authentication:Google:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="b43ed-146">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`.</span></span>

<span data-ttu-id="b43ed-147">Vous trouverez les valeurs de ces jetons dans le fichier JSON téléchargé à l’étape précédente sous `web.client_id` et `web.client_secret`.</span><span class="sxs-lookup"><span data-stu-id="b43ed-147">The values for these tokens can be found in the JSON file downloaded in the previous step under `web.client_id` and `web.client_secret`.</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="b43ed-148">Configurer l’authentification Google</span><span class="sxs-lookup"><span data-stu-id="b43ed-148">Configure Google Authentication</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="b43ed-149">Ajoutez le service Google dans le `ConfigureServices` méthode dans *Startup.cs* fichier :</span><span class="sxs-lookup"><span data-stu-id="b43ed-149">Add the Google service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="b43ed-150">Le modèle de projet utilisé dans ce didacticiel s’assure que [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) package est installé.</span><span class="sxs-lookup"><span data-stu-id="b43ed-150">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) package is installed.</span></span>

* <span data-ttu-id="b43ed-151">Pour installer ce package avec Visual Studio 2017, cliquez sur le projet, puis sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b43ed-151">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="b43ed-152">Pour installer avec l’interface CLI .NET Core, exécutez le code suivant dans votre répertoire de projet :</span><span class="sxs-lookup"><span data-stu-id="b43ed-152">To install with .NET Core CLI, execute the following in your project directory:</span></span>

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

<span data-ttu-id="b43ed-153">Ajoutez l’intergiciel (middleware) Google dans le `Configure` méthode dans *Startup.cs* fichier :</span><span class="sxs-lookup"><span data-stu-id="b43ed-153">Add the Google middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

::: moniker-end

<span data-ttu-id="b43ed-154">Consultez le [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) référence des API pour plus d’informations sur les options de configuration prises en charge par l’authentification Google.</span><span class="sxs-lookup"><span data-stu-id="b43ed-154">See the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="b43ed-155">Cela peut être utilisé pour demander différentes informations sur l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b43ed-155">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-google"></a><span data-ttu-id="b43ed-156">Se connecter avec Google</span><span class="sxs-lookup"><span data-stu-id="b43ed-156">Sign in with Google</span></span>

<span data-ttu-id="b43ed-157">Exécutez votre application et cliquez sur **connectez-vous**.</span><span class="sxs-lookup"><span data-stu-id="b43ed-157">Run your application and click **Log in**.</span></span> <span data-ttu-id="b43ed-158">Une option pour vous connecter avec Google s’affiche :</span><span class="sxs-lookup"><span data-stu-id="b43ed-158">An option to sign in with Google appears:</span></span>

![Application Web s’exécutant dans Microsoft Edge : utilisateur non authentifié](index/_static/DoneGoogle.png)

<span data-ttu-id="b43ed-160">Lorsque vous cliquez sur Google, vous êtes redirigé vers Google pour l’authentification :</span><span class="sxs-lookup"><span data-stu-id="b43ed-160">When you click on Google, you are redirected to Google for authentication:</span></span>

![Boîte de dialogue authentification Google](index/_static/GoogleLogin.png)

<span data-ttu-id="b43ed-162">Après avoir entré vos informations d’identification Google, puis vous êtes redirigé vers le site web où vous pouvez définir votre adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="b43ed-162">After entering your Google credentials, then you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="b43ed-163">Vous êtes maintenant connecté à l’aide de vos informations d’identification Google :</span><span class="sxs-lookup"><span data-stu-id="b43ed-163">You are now logged in using your Google credentials:</span></span>

![Application Web s’exécutant dans Microsoft Edge : utilisateur authentifié](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="b43ed-165">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="b43ed-165">Troubleshooting</span></span>

* <span data-ttu-id="b43ed-166">Si vous recevez un `403 (Forbidden)` page d’erreur à partir de votre propre application lors de l’exécution en mode de développement (ou s’arrêter dans le débogueur avec la même erreur), vérifiez que **API Google +** a été activée dans le **bibliothèque d’API Manager** en suivant les étapes répertoriées [antérieures sur cette page](#create-the-app-in-google-api-console).</span><span class="sxs-lookup"><span data-stu-id="b43ed-166">If you receive a `403 (Forbidden)` error page from your own app when running in development mode (or break into the debugger with the same error), ensure that **Google+ API** has been enabled in the **API Manager Library** by following the steps listed [earlier on this page](#create-the-app-in-google-api-console).</span></span> <span data-ttu-id="b43ed-167">Si la connexion ne fonctionne pas et vous ne recevez pas les erreurs, basculer en mode de développement pour rendre le problème plus facile à déboguer.</span><span class="sxs-lookup"><span data-stu-id="b43ed-167">If the sign in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="b43ed-168">**ASP.NET Core 2.x uniquement :** si identité n’est pas configurée en appelant `services.AddIdentity` dans `ConfigureServices`, toute tentative authentifier entraîne *ArgumentException : l’option 'SignInScheme' doit être fournie*.</span><span class="sxs-lookup"><span data-stu-id="b43ed-168">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="b43ed-169">Le modèle de projet utilisé dans ce didacticiel permet de s’assurer que cela est fait.</span><span class="sxs-lookup"><span data-stu-id="b43ed-169">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="b43ed-170">Si la base de données de site n’a pas été créé en appliquant la migration initiale, vous obtiendrez *une opération de base de données a échoué lors du traitement de la demande* erreur.</span><span class="sxs-lookup"><span data-stu-id="b43ed-170">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="b43ed-171">Appuyez sur **appliquer les Migrations** pour créer la base de données et actualiser pour passer à l’erreur.</span><span class="sxs-lookup"><span data-stu-id="b43ed-171">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b43ed-172">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="b43ed-172">Next steps</span></span>

* <span data-ttu-id="b43ed-173">Cet article vous a montré comment vous pouvez vous authentifier avec Google.</span><span class="sxs-lookup"><span data-stu-id="b43ed-173">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="b43ed-174">Vous pouvez suivre une approche similaire pour s’authentifier auprès d’autres fournisseurs répertoriés sur le [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="b43ed-174">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="b43ed-175">Une fois que vous publiez votre site web à l’application web Azure, vous devez réinitialiser le `ClientSecret` dans la Console d’API Google.</span><span class="sxs-lookup"><span data-stu-id="b43ed-175">Once you publish your web site to Azure web app, you should reset the `ClientSecret` in the Google API Console.</span></span>

* <span data-ttu-id="b43ed-176">Définir le `Authentication:Google:ClientId` et `Authentication:Google:ClientSecret` en tant que paramètres d’application dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="b43ed-176">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="b43ed-177">Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="b43ed-177">The configuration system is set up to read keys from environment variables.</span></span>
