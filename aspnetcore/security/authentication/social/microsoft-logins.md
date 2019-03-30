---
title: Programme d’installation de Microsoft Account connexion externe avec ASP.NET Core
author: rick-anderson
description: Ce didacticiel montre l’intégration de l’authentification d’utilisateur de compte Microsoft à une application ASP.NET Core existante.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 1733d049d6752c24d7749b5b5ae2a4b866492358
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58751005"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="46c9b-103">Programme d’installation de Microsoft Account connexion externe avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="46c9b-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="46c9b-104">Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="46c9b-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="46c9b-105">Ce didacticiel vous montre comment autoriser vos utilisateurs à se connecter avec leur compte Microsoft à l’aide d’un exemple de projet ASP.NET Core 2.0 créée sur le [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="46c9b-105">This tutorial shows you how to enable your users to sign in with their Microsoft account using a sample ASP.NET Core 2.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="46c9b-106">Créer l’application dans le portail des développeurs de Microsoft</span><span class="sxs-lookup"><span data-stu-id="46c9b-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="46c9b-107">Accédez à [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com) et créer ou vous connecter à un compte Microsoft :</span><span class="sxs-lookup"><span data-stu-id="46c9b-107">Navigate to [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com) and create or sign into a Microsoft account:</span></span>

![Boîte de dialogue se connecter](index/_static/MicrosoftDevLogin.png)

<span data-ttu-id="46c9b-109">Si vous n’avez pas déjà un compte Microsoft, appuyez sur  **[créez-en un !](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span><span class="sxs-lookup"><span data-stu-id="46c9b-109">If you don't already have a Microsoft account, tap **[Create one!](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)**</span></span> <span data-ttu-id="46c9b-110">Après vous être connecté, vous êtes redirigé vers **mes applications** page :</span><span class="sxs-lookup"><span data-stu-id="46c9b-110">After signing in you are redirected to **My applications** page:</span></span>

![Portail des développeurs Microsoft ouvert dans Microsoft Edge](index/_static/MicrosoftDev.png)

* <span data-ttu-id="46c9b-112">Appuyez sur **ajouter une application** dans le coin supérieur droit d’angle, puis entrez votre **nom de l’Application** et **adresse E-mail de Contact**:</span><span class="sxs-lookup"><span data-stu-id="46c9b-112">Tap **Add an app** in the upper right corner and enter your **Application Name** and **Contact Email**:</span></span>

![Boîte de dialogue nouvelle inscription d’Application](index/_static/MicrosoftDevAppCreate.png)

* <span data-ttu-id="46c9b-114">Dans le cadre de ce didacticiel, désactivez le **guidée par le programme d’installation** case à cocher.</span><span class="sxs-lookup"><span data-stu-id="46c9b-114">For the purposes of this tutorial, clear the **Guided Setup** check box.</span></span>

* <span data-ttu-id="46c9b-115">Appuyez sur **créer** pour continuer au **inscription** page.</span><span class="sxs-lookup"><span data-stu-id="46c9b-115">Tap **Create** to continue to the **Registration** page.</span></span> <span data-ttu-id="46c9b-116">Fournir un **nom** et notez la valeur de la **Id d’Application**, que vous utilisez en tant que `ClientId` plus loin dans le didacticiel :</span><span class="sxs-lookup"><span data-stu-id="46c9b-116">Provide a **Name** and note the value of the **Application Id**, which you use as `ClientId` later in the tutorial:</span></span>

![Page d’inscription](index/_static/MicrosoftDevAppReg.png)

* <span data-ttu-id="46c9b-118">Appuyez sur **ajouter une plateforme** dans le **plateformes** section et sélectionnez le **Web** plateforme :</span><span class="sxs-lookup"><span data-stu-id="46c9b-118">Tap **Add Platform** in the **Platforms** section and select the **Web** platform:</span></span>

![Ajouter la boîte de dialogue plateforme](index/_static/MicrosoftDevAppPlatform.png)

* <span data-ttu-id="46c9b-120">Dans le nouveau **Web** plateforme section, entrez l’URL de votre développement avec `/signin-microsoft` ajoutées dans le **URL de redirection** champ (par exemple : `https://localhost:44320/signin-microsoft`).</span><span class="sxs-lookup"><span data-stu-id="46c9b-120">In the new **Web** platform section, enter your development URL with `/signin-microsoft` appended into the **Redirect URLs** field (for example: `https://localhost:44320/signin-microsoft`).</span></span> <span data-ttu-id="46c9b-121">Le schéma d’authentification Microsoft configuré plus loin dans ce didacticiel gère automatiquement les demandes à `/signin-microsoft` itinéraire pour implémenter le flux OAuth :</span><span class="sxs-lookup"><span data-stu-id="46c9b-121">The Microsoft authentication scheme configured later in this tutorial will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow:</span></span>

![Section de plateforme Web](index/_static/MicrosoftRedirectUri.png)

> [!NOTE]
> <span data-ttu-id="46c9b-123">Le segment d’URI `/signin-microsoft` est défini en tant que le rappel par défaut du fournisseur d’authentification de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="46c9b-123">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="46c9b-124">Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel d’authentification Microsoft via le hérité [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propriété de la [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) classe.</span><span class="sxs-lookup"><span data-stu-id="46c9b-124">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

* <span data-ttu-id="46c9b-125">Appuyez sur **ajouter une URL** afin de vérifier l’URL a été ajoutée.</span><span class="sxs-lookup"><span data-stu-id="46c9b-125">Tap **Add URL** to ensure the URL was added.</span></span>

* <span data-ttu-id="46c9b-126">Remplissez les autres paramètres de l’application si nécessaire, puis appuyez sur **enregistrer** en bas de la page pour enregistrer les modifications apportées à la configuration de l’application.</span><span class="sxs-lookup"><span data-stu-id="46c9b-126">Fill out any other application settings if necessary and tap **Save** at the bottom of the page to save changes to app configuration.</span></span>

* <span data-ttu-id="46c9b-127">Lorsque vous déployez le site, vous devez revoir la **inscription** page et définir une nouvelle URL publique.</span><span class="sxs-lookup"><span data-stu-id="46c9b-127">When deploying the site you'll need to revisit the **Registration** page and set a new public URL.</span></span>

## <a name="store-microsoft-application-id-and-password"></a><span data-ttu-id="46c9b-128">Store Id d’Application de Microsoft et le mot de passe</span><span class="sxs-lookup"><span data-stu-id="46c9b-128">Store Microsoft Application Id and Password</span></span>

* <span data-ttu-id="46c9b-129">Remarque la `Application Id` affichée sur la **inscription** page.</span><span class="sxs-lookup"><span data-stu-id="46c9b-129">Note the `Application Id` displayed on the **Registration** page.</span></span>

* <span data-ttu-id="46c9b-130">Appuyez sur **générer un nouveau mot de passe** dans le **Secrets d’Application** section.</span><span class="sxs-lookup"><span data-stu-id="46c9b-130">Tap **Generate New Password** in the **Application Secrets** section.</span></span> <span data-ttu-id="46c9b-131">Cela affiche une zone où vous pouvez copier le mot de passe d’application :</span><span class="sxs-lookup"><span data-stu-id="46c9b-131">This displays a box where you can copy the application password:</span></span>

![Boîte de dialogue Nouveau mot de passe généré](index/_static/MicrosoftDevPassword.png)

<span data-ttu-id="46c9b-133">Lier des paramètres sensibles telles que Microsoft `Application ID` et `Password` à votre configuration d’application en utilisant le [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="46c9b-133">Link sensitive settings like Microsoft `Application ID` and `Password` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="46c9b-134">Dans le cadre de ce didacticiel, nommez les jetons `Authentication:Microsoft:ApplicationId` et `Authentication:Microsoft:Password`.</span><span class="sxs-lookup"><span data-stu-id="46c9b-134">For the purposes of this tutorial, name the tokens `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="46c9b-135">Configurer l’authentification de compte Microsoft</span><span class="sxs-lookup"><span data-stu-id="46c9b-135">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="46c9b-136">Le modèle de projet utilisé dans ce didacticiel s’assure que [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) package est déjà installé.</span><span class="sxs-lookup"><span data-stu-id="46c9b-136">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) package is already installed.</span></span>

* <span data-ttu-id="46c9b-137">Pour installer ce package avec Visual Studio 2017, cliquez sur le projet, puis sélectionnez **gérer les Packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="46c9b-137">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="46c9b-138">Pour installer avec l’interface CLI .NET Core, exécutez le code suivant dans votre répertoire de projet :</span><span class="sxs-lookup"><span data-stu-id="46c9b-138">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="46c9b-139">Ajoutez le service Account Microsoft dans le `ConfigureServices` méthode dans *Startup.cs* fichier :</span><span class="sxs-lookup"><span data-stu-id="46c9b-139">Add the Microsoft Account service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="46c9b-140">Ajoutez le middleware Account Microsoft dans le `Configure` méthode dans *Startup.cs* fichier :</span><span class="sxs-lookup"><span data-stu-id="46c9b-140">Add the Microsoft Account middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

::: moniker-end

<span data-ttu-id="46c9b-141">Bien que la terminologie utilisée sur le portail des développeurs Microsoft nomme ces jetons `ApplicationId` et `Password`, elles sont exposées en tant que `ClientId` et `ClientSecret` à l’API de configuration.</span><span class="sxs-lookup"><span data-stu-id="46c9b-141">Although the terminology used on Microsoft Developer Portal names these tokens `ApplicationId` and `Password`, they're exposed as `ClientId` and `ClientSecret` to the configuration API.</span></span>

<span data-ttu-id="46c9b-142">Consultez le [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) référence des API pour plus d’informations sur les options de configuration prises en charge par l’authentification de Microsoft Account.</span><span class="sxs-lookup"><span data-stu-id="46c9b-142">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="46c9b-143">Cela peut être utilisé pour demander différentes informations sur l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="46c9b-143">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="46c9b-144">Connectez-vous avec un compte Microsoft</span><span class="sxs-lookup"><span data-stu-id="46c9b-144">Sign in with Microsoft Account</span></span>

<span data-ttu-id="46c9b-145">Exécutez votre application et cliquez sur **connectez-vous**.</span><span class="sxs-lookup"><span data-stu-id="46c9b-145">Run your application and click **Log in**.</span></span> <span data-ttu-id="46c9b-146">Une option permettant de se connecter avec Microsoft s’affiche :</span><span class="sxs-lookup"><span data-stu-id="46c9b-146">An option to sign in with Microsoft appears:</span></span>

![Application Web Log dans la page : Utilisateur non authentifié](index/_static/DoneMicrosoft.png)

<span data-ttu-id="46c9b-148">Lorsque vous cliquez sur Microsoft, vous êtes redirigé vers Microsoft pour l’authentification.</span><span class="sxs-lookup"><span data-stu-id="46c9b-148">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="46c9b-149">Après vous être connecté avec votre Account Microsoft (si pas déjà connecté), vous devez permettre à l’application d’accéder à vos informations :</span><span class="sxs-lookup"><span data-stu-id="46c9b-149">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

![Boîte de dialogue d’authentification Microsoft](index/_static/MicrosoftLogin.png)

<span data-ttu-id="46c9b-151">Appuyez sur **Oui** et vous êtes redirigé vers le site web où vous pouvez définir votre adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="46c9b-151">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="46c9b-152">Vous êtes maintenant connecté à l’aide de vos informations d’identification Microsoft :</span><span class="sxs-lookup"><span data-stu-id="46c9b-152">You are now logged in using your Microsoft credentials:</span></span>

![Application Web : Utilisateur authentifié](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="46c9b-154">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="46c9b-154">Troubleshooting</span></span>

* <span data-ttu-id="46c9b-155">Si le fournisseur Microsoft Account vous redirige vers une page d’erreur de connexion, notez l’erreur title et description paramètres chaîne de requête directement après le `#` (hashtag) dans l’Uri.</span><span class="sxs-lookup"><span data-stu-id="46c9b-155">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="46c9b-156">Bien que le message d’erreur semble indiquer un problème avec l’authentification Microsoft, la cause la plus courante est votre application Uri ne pas corresponde à l’un de le **URI de redirection** spécifié pour le **Web** plateforme .</span><span class="sxs-lookup"><span data-stu-id="46c9b-156">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="46c9b-157">**ASP.NET Core 2.x uniquement :** Si l’identité n’est pas configurée en appelant `services.AddIdentity` dans `ConfigureServices`, toute tentative authentifier entraîne *ArgumentException : L’option 'SignInScheme' doit être fournie*.</span><span class="sxs-lookup"><span data-stu-id="46c9b-157">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="46c9b-158">Le modèle de projet utilisé dans ce didacticiel permet de s’assurer que cela est fait.</span><span class="sxs-lookup"><span data-stu-id="46c9b-158">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="46c9b-159">Si la base de données de site n’a pas été créé en appliquant la migration initiale, vous obtiendrez *une opération de base de données a échoué lors du traitement de la demande* erreur.</span><span class="sxs-lookup"><span data-stu-id="46c9b-159">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="46c9b-160">Appuyez sur **appliquer les Migrations** pour créer la base de données et actualiser pour passer à l’erreur.</span><span class="sxs-lookup"><span data-stu-id="46c9b-160">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="46c9b-161">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="46c9b-161">Next steps</span></span>

* <span data-ttu-id="46c9b-162">Cet article vous a montré comment vous pouvez vous authentifier auprès de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="46c9b-162">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="46c9b-163">Vous pouvez suivre une approche similaire pour s’authentifier auprès d’autres fournisseurs répertoriés sur le [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="46c9b-163">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="46c9b-164">Une fois que vous publiez votre site web à l’application web Azure, vous devez créer un nouveau `Password` dans le portail des développeurs Microsoft.</span><span class="sxs-lookup"><span data-stu-id="46c9b-164">Once you publish your web site to Azure web app, you should create a new `Password` in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="46c9b-165">Définir le `Authentication:Microsoft:ApplicationId` et `Authentication:Microsoft:Password` en tant que paramètres d’application dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="46c9b-165">Set the `Authentication:Microsoft:ApplicationId` and `Authentication:Microsoft:Password` as application settings in the Azure portal.</span></span> <span data-ttu-id="46c9b-166">Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="46c9b-166">The configuration system is set up to read keys from environment variables.</span></span>
