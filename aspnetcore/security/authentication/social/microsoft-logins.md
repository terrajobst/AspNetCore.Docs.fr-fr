---
title: Configuration de la connexion externe à un compte Microsoft avec ASP.NET Core
author: rick-anderson
description: Cet exemple illustre l’intégration de compte Microsoft l’authentification utilisateur dans une application ASP.NET Core existante.
ms.author: riande
ms.custom: mvc
ms.date: 12/4/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/microsoft-logins
ms.openlocfilehash: ddaae1a25a1dcf167ffae0f24b480e2cde6aca5b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659795"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="32c6a-103">Configuration de la connexion externe à un compte Microsoft avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="32c6a-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="32c6a-104">Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="32c6a-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="32c6a-105">Cet exemple montre comment permettre aux utilisateurs de se connecter avec leur compte Microsoft à l’aide du projet ASP.NET Core 3,0 créé sur la [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="32c6a-105">This sample shows you how to enable users to sign in with their Microsoft account using the ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="32c6a-106">Créer l’application dans le portail des développeurs Microsoft</span><span class="sxs-lookup"><span data-stu-id="32c6a-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="32c6a-107">Ajoutez le package NuGet [Microsoft. AspNetCore. Authentication. MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount/) au projet.</span><span class="sxs-lookup"><span data-stu-id="32c6a-107">Add the [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount/) NuGet package to the project.</span></span>
* <span data-ttu-id="32c6a-108">Accédez à la page de [inscriptions d’applications portail Azure](https://go.microsoft.com/fwlink/?linkid=2083908) et créez ou connectez-vous à une compte Microsoft :</span><span class="sxs-lookup"><span data-stu-id="32c6a-108">Navigate to the [Azure portal - App registrations](https://go.microsoft.com/fwlink/?linkid=2083908) page and create or sign into a Microsoft account:</span></span>

<span data-ttu-id="32c6a-109">Si vous n’avez pas de compte Microsoft, sélectionnez en **créer un**.</span><span class="sxs-lookup"><span data-stu-id="32c6a-109">If you don't have a Microsoft account, select **Create one**.</span></span> <span data-ttu-id="32c6a-110">Une fois connecté, vous êtes redirigé vers la page **inscriptions d’applications** :</span><span class="sxs-lookup"><span data-stu-id="32c6a-110">After signing in, you are redirected to the **App registrations** page:</span></span>

* <span data-ttu-id="32c6a-111">Sélectionnez **Nouvelle inscription**.</span><span class="sxs-lookup"><span data-stu-id="32c6a-111">Select **New registration**</span></span>
* <span data-ttu-id="32c6a-112">Saisissez un **Nom**.</span><span class="sxs-lookup"><span data-stu-id="32c6a-112">Enter a **Name**.</span></span>
* <span data-ttu-id="32c6a-113">Sélectionnez une option pour les **types de comptes pris en charge**.</span><span class="sxs-lookup"><span data-stu-id="32c6a-113">Select an option for **Supported account types**.</span></span>  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* <span data-ttu-id="32c6a-114">Sous **URI de redirection**, entrez votre URL de développement avec `/signin-microsoft` ajoutée.</span><span class="sxs-lookup"><span data-stu-id="32c6a-114">Under **Redirect URI**, enter your development URL with `/signin-microsoft` appended.</span></span> <span data-ttu-id="32c6a-115">Par exemple : `https://localhost:5001/signin-microsoft`.</span><span class="sxs-lookup"><span data-stu-id="32c6a-115">For example, `https://localhost:5001/signin-microsoft`.</span></span> <span data-ttu-id="32c6a-116">Le schéma d’authentification Microsoft configuré plus tard dans cet exemple gère automatiquement les demandes à `/signin-microsoft` itinéraire pour implémenter le Flow OAuth.</span><span class="sxs-lookup"><span data-stu-id="32c6a-116">The Microsoft authentication scheme configured later in this sample will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow.</span></span>
* <span data-ttu-id="32c6a-117">Sélectionnez **Inscrire**.</span><span class="sxs-lookup"><span data-stu-id="32c6a-117">Select **Register**</span></span>

### <a name="create-client-secret"></a><span data-ttu-id="32c6a-118">Créer un secret client</span><span class="sxs-lookup"><span data-stu-id="32c6a-118">Create client secret</span></span>

* <span data-ttu-id="32c6a-119">Dans le volet gauche, sélectionnez **Certificats et secrets**.</span><span class="sxs-lookup"><span data-stu-id="32c6a-119">In the left pane, select **Certificates & secrets**.</span></span>
* <span data-ttu-id="32c6a-120">Sous **secrets client**, sélectionnez **nouvelle clé secrète client** .</span><span class="sxs-lookup"><span data-stu-id="32c6a-120">Under **Client secrets**, select **New client secret**</span></span>

  * <span data-ttu-id="32c6a-121">Ajoutez une description pour la clé secrète client.</span><span class="sxs-lookup"><span data-stu-id="32c6a-121">Add a description for the client secret.</span></span>
  * <span data-ttu-id="32c6a-122">Sélectionnez le bouton **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="32c6a-122">Select the **Add** button.</span></span>

* <span data-ttu-id="32c6a-123">Sous **secrets clients**, copiez la valeur de la clé secrète client.</span><span class="sxs-lookup"><span data-stu-id="32c6a-123">Under **Client secrets**, copy the value of the client secret.</span></span>

> [!NOTE]
> <span data-ttu-id="32c6a-124">Le segment d’URI `/signin-microsoft` est défini comme rappel par défaut du fournisseur d’authentification Microsoft.</span><span class="sxs-lookup"><span data-stu-id="32c6a-124">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="32c6a-125">Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel (middleware) d’authentification Microsoft via la propriété héritée [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) de la classe [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) .</span><span class="sxs-lookup"><span data-stu-id="32c6a-125">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

## <a name="store-the-microsoft-client-id-and-client-secret"></a><span data-ttu-id="32c6a-126">Stocker l’ID client et la clé secrète client Microsoft</span><span class="sxs-lookup"><span data-stu-id="32c6a-126">Store the Microsoft client ID and client secret</span></span>

<span data-ttu-id="32c6a-127">Exécutez les commandes suivantes pour stocker `ClientId` et `ClientSecret` en toute sécurité à l’aide du [Gestionnaire de secret](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="32c6a-127">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```dotnetcli
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

<span data-ttu-id="32c6a-128">Liez des paramètres sensibles tels que Microsoft `ClientId` et `ClientSecret` à la configuration de votre application à l’aide du [Gestionnaire de secret](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="32c6a-128">Link sensitive settings like Microsoft `ClientId` and `ClientSecret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="32c6a-129">Dans le cadre de cet exemple, nommez les jetons `Authentication:Microsoft:ClientId` et `Authentication:Microsoft:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="32c6a-129">For the purposes of this sample, name the tokens `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="32c6a-130">Configurer l’authentification de compte Microsoft</span><span class="sxs-lookup"><span data-stu-id="32c6a-130">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="32c6a-131">Ajoutez le service de compte Microsoft au `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="32c6a-131">Add the Microsoft Account service to the `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupMS3x.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="32c6a-132">Pour plus d’informations sur les options de configuration prises en charge par l’authentification de compte Microsoft, consultez la référence de l’API [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) .</span><span class="sxs-lookup"><span data-stu-id="32c6a-132">For more information about configuration options supported by Microsoft Account authentication, see the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference.</span></span> <span data-ttu-id="32c6a-133">Cela peut être utilisé pour demander différentes informations sur l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="32c6a-133">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="32c6a-134">Compte Se connecter avec Microsoft</span><span class="sxs-lookup"><span data-stu-id="32c6a-134">Sign in with Microsoft Account</span></span>

<span data-ttu-id="32c6a-135">Exécutez l’application, puis cliquez sur **se connecter**.</span><span class="sxs-lookup"><span data-stu-id="32c6a-135">Run the app and click **Log in**.</span></span> <span data-ttu-id="32c6a-136">Une option permettant de se connecter à Microsoft s’affiche.</span><span class="sxs-lookup"><span data-stu-id="32c6a-136">An option to sign in with Microsoft appears.</span></span> <span data-ttu-id="32c6a-137">Lorsque vous cliquez sur Microsoft, vous êtes redirigé vers Microsoft pour l’authentification.</span><span class="sxs-lookup"><span data-stu-id="32c6a-137">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="32c6a-138">Une fois que vous êtes connecté avec votre compte Microsoft, vous êtes invité à autoriser l’application à accéder à vos informations :</span><span class="sxs-lookup"><span data-stu-id="32c6a-138">After signing in with your Microsoft Account, you will be prompted to let the app access your info:</span></span>

<span data-ttu-id="32c6a-139">Appuyez sur **Oui** . vous êtes alors redirigé vers le site Web où vous pouvez définir votre adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="32c6a-139">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="32c6a-140">Vous êtes maintenant connecté à l’aide de vos informations d’identification Microsoft :</span><span class="sxs-lookup"><span data-stu-id="32c6a-140">You are now logged in using your Microsoft credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="32c6a-141">Dépannage</span><span class="sxs-lookup"><span data-stu-id="32c6a-141">Troubleshooting</span></span>

* <span data-ttu-id="32c6a-142">Si le fournisseur de comptes Microsoft vous redirige vers une page d’erreur de connexion, notez le titre et la description des paramètres de chaîne de requête en suivant directement le `#` (mot-dièse) dans l’URI.</span><span class="sxs-lookup"><span data-stu-id="32c6a-142">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="32c6a-143">Bien que le message d’erreur semble indiquer un problème avec l’authentification Microsoft, la cause la plus courante est que l’URI de votre application ne correspond à aucun des **URI de redirection** spécifiés pour la plateforme **Web** .</span><span class="sxs-lookup"><span data-stu-id="32c6a-143">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="32c6a-144">Si l’identité n’est pas configurée en appelant `services.AddIdentity` dans `ConfigureServices`, toute tentative d’authentification entraînera *l’exception ArgumentException : l’option « SignInScheme » doit être fournie*.</span><span class="sxs-lookup"><span data-stu-id="32c6a-144">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="32c6a-145">Le modèle de projet utilisé dans cet exemple permet de s’assurer que cette opération est effectuée.</span><span class="sxs-lookup"><span data-stu-id="32c6a-145">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="32c6a-146">Si la base de données de site n’a pas été créée en appliquant la migration initiale, vous obtiendrez *une opération de base de données qui a échoué lors du traitement de l’erreur de demande* .</span><span class="sxs-lookup"><span data-stu-id="32c6a-146">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="32c6a-147">Appuyez sur **appliquer des migrations** pour créer la base de données, puis sur Actualiser pour poursuivre l’erreur.</span><span class="sxs-lookup"><span data-stu-id="32c6a-147">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="32c6a-148">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="32c6a-148">Next steps</span></span>

* <span data-ttu-id="32c6a-149">Cet article vous a montré comment vous pouvez vous authentifier auprès de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="32c6a-149">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="32c6a-150">Vous pouvez suivre une approche similaire pour vous authentifier auprès d’autres fournisseurs listés dans la [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="32c6a-150">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="32c6a-151">Une fois que vous publiez votre site Web dans l’application Web Azure, créez un nouveau secret client dans le portail des développeurs Microsoft.</span><span class="sxs-lookup"><span data-stu-id="32c6a-151">Once you publish your web site to Azure web app, create a new client secrets in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="32c6a-152">Définissez les `Authentication:Microsoft:ClientId` et `Authentication:Microsoft:ClientSecret` en tant que paramètres d’application dans le Portail Azure.</span><span class="sxs-lookup"><span data-stu-id="32c6a-152">Set the `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="32c6a-153">Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="32c6a-153">The configuration system is set up to read keys from environment variables.</span></span>
