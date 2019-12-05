---
title: Microsoft Account external login setup with ASP.NET Core
author: rick-anderson
description: This sample demonstrates the integration of Microsoft account user authentication into an existing ASP.NET Core app.
ms.author: riande
ms.custom: mvc
ms.date: 12/4/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/microsoft-logins
ms.openlocfilehash: ddaae1a25a1dcf167ffae0f24b480e2cde6aca5b
ms.sourcegitcommit: f4cd3828e26e6d549ba8d0c36a17be35ad9e5a51
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74825468"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="f1117-103">Microsoft Account external login setup with ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f1117-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="f1117-104">Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f1117-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f1117-105">This sample shows you how to enable users to sign in with their Microsoft account using the ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="f1117-105">This sample shows you how to enable users to sign in with their Microsoft account using the ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="f1117-106">Create the app in Microsoft Developer Portal</span><span class="sxs-lookup"><span data-stu-id="f1117-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="f1117-107">Add the [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount/) NuGet package to the project.</span><span class="sxs-lookup"><span data-stu-id="f1117-107">Add the [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount/) NuGet package to the project.</span></span>
* <span data-ttu-id="f1117-108">Navigate to the [Azure portal - App registrations](https://go.microsoft.com/fwlink/?linkid=2083908) page and create or sign into a Microsoft account:</span><span class="sxs-lookup"><span data-stu-id="f1117-108">Navigate to the [Azure portal - App registrations](https://go.microsoft.com/fwlink/?linkid=2083908) page and create or sign into a Microsoft account:</span></span>

<span data-ttu-id="f1117-109">If you don't have a Microsoft account, select **Create one**.</span><span class="sxs-lookup"><span data-stu-id="f1117-109">If you don't have a Microsoft account, select **Create one**.</span></span> <span data-ttu-id="f1117-110">After signing in, you are redirected to the **App registrations** page:</span><span class="sxs-lookup"><span data-stu-id="f1117-110">After signing in, you are redirected to the **App registrations** page:</span></span>

* <span data-ttu-id="f1117-111">Sélectionnez **Nouvelle inscription**.</span><span class="sxs-lookup"><span data-stu-id="f1117-111">Select **New registration**</span></span>
* <span data-ttu-id="f1117-112">Entrez un **nom**.</span><span class="sxs-lookup"><span data-stu-id="f1117-112">Enter a **Name**.</span></span>
* <span data-ttu-id="f1117-113">Select an option for **Supported account types**.</span><span class="sxs-lookup"><span data-stu-id="f1117-113">Select an option for **Supported account types**.</span></span>  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* <span data-ttu-id="f1117-114">Under **Redirect URI**, enter your development URL with `/signin-microsoft` appended.</span><span class="sxs-lookup"><span data-stu-id="f1117-114">Under **Redirect URI**, enter your development URL with `/signin-microsoft` appended.</span></span> <span data-ttu-id="f1117-115">Par exemple, `https://localhost:5001/signin-microsoft`.</span><span class="sxs-lookup"><span data-stu-id="f1117-115">For example, `https://localhost:5001/signin-microsoft`.</span></span> <span data-ttu-id="f1117-116">The Microsoft authentication scheme configured later in this sample will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow.</span><span class="sxs-lookup"><span data-stu-id="f1117-116">The Microsoft authentication scheme configured later in this sample will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow.</span></span>
* <span data-ttu-id="f1117-117">Sélectionnez **Inscrire**.</span><span class="sxs-lookup"><span data-stu-id="f1117-117">Select **Register**</span></span>

### <a name="create-client-secret"></a><span data-ttu-id="f1117-118">Créer un secret client</span><span class="sxs-lookup"><span data-stu-id="f1117-118">Create client secret</span></span>

* <span data-ttu-id="f1117-119">Dans le volet gauche, sélectionnez **Certificats et secrets**.</span><span class="sxs-lookup"><span data-stu-id="f1117-119">In the left pane, select **Certificates & secrets**.</span></span>
* <span data-ttu-id="f1117-120">Under **Client secrets**, select **New client secret**</span><span class="sxs-lookup"><span data-stu-id="f1117-120">Under **Client secrets**, select **New client secret**</span></span>

  * <span data-ttu-id="f1117-121">Add a description for the client secret.</span><span class="sxs-lookup"><span data-stu-id="f1117-121">Add a description for the client secret.</span></span>
  * <span data-ttu-id="f1117-122">Sélectionnez le bouton **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="f1117-122">Select the **Add** button.</span></span>

* <span data-ttu-id="f1117-123">Under **Client secrets**, copy the value of the client secret.</span><span class="sxs-lookup"><span data-stu-id="f1117-123">Under **Client secrets**, copy the value of the client secret.</span></span>

> [!NOTE]
> <span data-ttu-id="f1117-124">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span><span class="sxs-lookup"><span data-stu-id="f1117-124">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="f1117-125">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span><span class="sxs-lookup"><span data-stu-id="f1117-125">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

## <a name="store-the-microsoft-client-id-and-client-secret"></a><span data-ttu-id="f1117-126">Store the Microsoft client ID and client secret</span><span class="sxs-lookup"><span data-stu-id="f1117-126">Store the Microsoft client ID and client secret</span></span>

<span data-ttu-id="f1117-127">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="f1117-127">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

<span data-ttu-id="f1117-128">Link sensitive settings like Microsoft `ClientId` and `ClientSecret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="f1117-128">Link sensitive settings like Microsoft `ClientId` and `ClientSecret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="f1117-129">For the purposes of this sample, name the tokens `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="f1117-129">For the purposes of this sample, name the tokens `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="f1117-130">Configure Microsoft Account Authentication</span><span class="sxs-lookup"><span data-stu-id="f1117-130">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="f1117-131">Add the Microsoft Account service to the `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f1117-131">Add the Microsoft Account service to the `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupMS3x.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="f1117-132">For more information about configuration options supported by Microsoft Account authentication, see the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference.</span><span class="sxs-lookup"><span data-stu-id="f1117-132">For more information about configuration options supported by Microsoft Account authentication, see the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference.</span></span> <span data-ttu-id="f1117-133">Cela peut être utilisé pour demander différentes informations sur l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f1117-133">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="f1117-134">Sign in with Microsoft Account</span><span class="sxs-lookup"><span data-stu-id="f1117-134">Sign in with Microsoft Account</span></span>

<span data-ttu-id="f1117-135">Run the app and click **Log in**.</span><span class="sxs-lookup"><span data-stu-id="f1117-135">Run the app and click **Log in**.</span></span> <span data-ttu-id="f1117-136">An option to sign in with Microsoft appears.</span><span class="sxs-lookup"><span data-stu-id="f1117-136">An option to sign in with Microsoft appears.</span></span> <span data-ttu-id="f1117-137">When you click on Microsoft, you are redirected to Microsoft for authentication.</span><span class="sxs-lookup"><span data-stu-id="f1117-137">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="f1117-138">After signing in with your Microsoft Account, you will be prompted to let the app access your info:</span><span class="sxs-lookup"><span data-stu-id="f1117-138">After signing in with your Microsoft Account, you will be prompted to let the app access your info:</span></span>

<span data-ttu-id="f1117-139">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span><span class="sxs-lookup"><span data-stu-id="f1117-139">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="f1117-140">You are now logged in using your Microsoft credentials:</span><span class="sxs-lookup"><span data-stu-id="f1117-140">You are now logged in using your Microsoft credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="f1117-141">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="f1117-141">Troubleshooting</span></span>

* <span data-ttu-id="f1117-142">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span><span class="sxs-lookup"><span data-stu-id="f1117-142">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="f1117-143">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span><span class="sxs-lookup"><span data-stu-id="f1117-143">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="f1117-144">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span><span class="sxs-lookup"><span data-stu-id="f1117-144">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="f1117-145">The project template used in this sample ensures that this is done.</span><span class="sxs-lookup"><span data-stu-id="f1117-145">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="f1117-146">Si la base de données de site n’a pas été créé en appliquant la migration initiale, vous obtiendrez *une opération de base de données a échoué lors du traitement de la demande* erreur.</span><span class="sxs-lookup"><span data-stu-id="f1117-146">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="f1117-147">Appuyez sur **appliquer les Migrations** pour créer la base de données et actualiser pour passer à l’erreur.</span><span class="sxs-lookup"><span data-stu-id="f1117-147">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f1117-148">Étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="f1117-148">Next steps</span></span>

* <span data-ttu-id="f1117-149">This article showed how you can authenticate with Microsoft.</span><span class="sxs-lookup"><span data-stu-id="f1117-149">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="f1117-150">Vous pouvez suivre une approche similaire pour s’authentifier auprès d’autres fournisseurs répertoriés sur le [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="f1117-150">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="f1117-151">Once you publish your web site to Azure web app, create a new client secrets in the Microsoft Developer Portal.</span><span class="sxs-lookup"><span data-stu-id="f1117-151">Once you publish your web site to Azure web app, create a new client secrets in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="f1117-152">Définir le `Authentication:Microsoft:ClientId` et `Authentication:Microsoft:ClientSecret` en tant que paramètres d’application dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="f1117-152">Set the `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="f1117-153">Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="f1117-153">The configuration system is set up to read keys from environment variables.</span></span>
