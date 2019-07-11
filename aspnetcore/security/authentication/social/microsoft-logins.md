---
title: Programme d’installation de Microsoft Account connexion externe avec ASP.NET Core
author: rick-anderson
description: Cet exemple montre l’intégration de l’authentification d’utilisateur de compte Microsoft à une application ASP.NET Core existante.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 2c690e5bd8465806d42091616917cfdd747ef8f0
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815571"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a><span data-ttu-id="65b49-103">Programme d’installation de Microsoft Account connexion externe avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="65b49-103">Microsoft Account external login setup with ASP.NET Core</span></span>

<span data-ttu-id="65b49-104">Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="65b49-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="65b49-105">Cet exemple vous montre comment permettre aux utilisateurs de se connecter avec leur compte Microsoft à l’aide du projet ASP.NET Core 2.2 créé sur le [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="65b49-105">This sample shows you how to enable users to sign in with their Microsoft account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-microsoft-developer-portal"></a><span data-ttu-id="65b49-106">Créer l’application dans le portail des développeurs de Microsoft</span><span class="sxs-lookup"><span data-stu-id="65b49-106">Create the app in Microsoft Developer Portal</span></span>

* <span data-ttu-id="65b49-107">Accédez à la [portail Azure - inscriptions](https://go.microsoft.com/fwlink/?linkid=2083908) page et de créer ou de vous connecter à un compte Microsoft :</span><span class="sxs-lookup"><span data-stu-id="65b49-107">Navigate to the [Azure portal - App registrations](https://go.microsoft.com/fwlink/?linkid=2083908) page and create or sign into a Microsoft account:</span></span>

<span data-ttu-id="65b49-108">Si vous n’avez pas un compte Microsoft, sélectionnez **créez-le**.</span><span class="sxs-lookup"><span data-stu-id="65b49-108">If you don't have a Microsoft account, select **Create one**.</span></span> <span data-ttu-id="65b49-109">Après vous être connecté, vous êtes redirigé vers la **inscriptions** page :</span><span class="sxs-lookup"><span data-stu-id="65b49-109">After signing in you are redirected to the **App registrations** page:</span></span>

* <span data-ttu-id="65b49-110">Sélectionnez **nouvelle inscription**</span><span class="sxs-lookup"><span data-stu-id="65b49-110">Select **New registration**</span></span>
* <span data-ttu-id="65b49-111">Entrez un **nom**.</span><span class="sxs-lookup"><span data-stu-id="65b49-111">Enter a **Name**.</span></span>
* <span data-ttu-id="65b49-112">Sélectionnez une option pour **pris en charge les types de comptes**.</span><span class="sxs-lookup"><span data-stu-id="65b49-112">Select an option for **Supported account types**.</span></span>  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* <span data-ttu-id="65b49-113">Sous **URI de redirection**, entrez l’URL de votre développement avec `/signin-microsoft` ajouté.</span><span class="sxs-lookup"><span data-stu-id="65b49-113">Under **Redirect URI**, enter your development URL with `/signin-microsoft` appended.</span></span> <span data-ttu-id="65b49-114">Par exemple, `https://localhost:44389/signin-microsoft`.</span><span class="sxs-lookup"><span data-stu-id="65b49-114">For example, `https://localhost:44389/signin-microsoft`.</span></span> <span data-ttu-id="65b49-115">Le schéma d’authentification Microsoft configuré plus loin dans cet exemple gère automatiquement les demandes à `/signin-microsoft` itinéraire pour implémenter le flux OAuth.</span><span class="sxs-lookup"><span data-stu-id="65b49-115">The Microsoft authentication scheme configured later in this sample will automatically handle requests at `/signin-microsoft` route to implement the OAuth flow.</span></span>
* <span data-ttu-id="65b49-116">Sélectionnez **inscrire**</span><span class="sxs-lookup"><span data-stu-id="65b49-116">Select **Register**</span></span>

### <a name="create-client-secret"></a><span data-ttu-id="65b49-117">Créer la clé secrète client</span><span class="sxs-lookup"><span data-stu-id="65b49-117">Create client secret</span></span>

* <span data-ttu-id="65b49-118">Dans le volet gauche, sélectionnez **certificats et clés secrètes**.</span><span class="sxs-lookup"><span data-stu-id="65b49-118">In the left pane, select **Certificates & secrets**.</span></span>
* <span data-ttu-id="65b49-119">Sous **clés secrètes de Client**, sélectionnez **nouvelle clé secrète client**</span><span class="sxs-lookup"><span data-stu-id="65b49-119">Under **Client secrets**, select **New client secret**</span></span>

  * <span data-ttu-id="65b49-120">Ajoutez une description pour la clé secrète client.</span><span class="sxs-lookup"><span data-stu-id="65b49-120">Add a description for the client secret.</span></span>
  * <span data-ttu-id="65b49-121">Sélectionnez le **ajouter** bouton.</span><span class="sxs-lookup"><span data-stu-id="65b49-121">Select the **Add** button.</span></span>

* <span data-ttu-id="65b49-122">Sous **clés secrètes de Client**, copiez la valeur de la clé secrète client.</span><span class="sxs-lookup"><span data-stu-id="65b49-122">Under **Client secrets**, copy the value of the client secret.</span></span>

> [!NOTE]
> <span data-ttu-id="65b49-123">Le segment d’URI `/signin-microsoft` est défini en tant que le rappel par défaut du fournisseur d’authentification de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="65b49-123">The URI segment `/signin-microsoft` is set as the default callback of the Microsoft authentication provider.</span></span> <span data-ttu-id="65b49-124">Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel d’authentification Microsoft via le hérité [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propriété de la [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) classe.</span><span class="sxs-lookup"><span data-stu-id="65b49-124">You can change the default callback URI while configuring the Microsoft authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) class.</span></span>

## <a name="store-the-microsoft-client-id-and-client-secret"></a><span data-ttu-id="65b49-125">Store le secret de client et ID de client Microsoft</span><span class="sxs-lookup"><span data-stu-id="65b49-125">Store the Microsoft client ID and client secret</span></span>

<span data-ttu-id="65b49-126">Exécutez les commandes suivantes pour stocker en toute sécurité `ClientId` et `ClientSecret` à l’aide de [Secret Manager](xref:security/app-secrets):</span><span class="sxs-lookup"><span data-stu-id="65b49-126">Run the following commands to securely store `ClientId` and `ClientSecret` using [Secret Manager](xref:security/app-secrets):</span></span>

```console
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

<span data-ttu-id="65b49-127">Lier des paramètres sensibles telles que Microsoft `ClientId` et `ClientSecret` à votre configuration d’application en utilisant le [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="65b49-127">Link sensitive settings like Microsoft `ClientId` and `ClientSecret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="65b49-128">Dans le cadre de cet exemple, nommez les jetons `Authentication:Microsoft:ClientId` et `Authentication:Microsoft:ClientSecret`.</span><span class="sxs-lookup"><span data-stu-id="65b49-128">For the purposes of this sample, name the tokens `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret`.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a><span data-ttu-id="65b49-129">Configurer l’authentification de compte Microsoft</span><span class="sxs-lookup"><span data-stu-id="65b49-129">Configure Microsoft Account Authentication</span></span>

<span data-ttu-id="65b49-130">Ajouter le service Microsoft Account `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="65b49-130">Add the Microsoft Account service to `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupMS.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="65b49-131">Consultez le [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) référence des API pour plus d’informations sur les options de configuration prises en charge par l’authentification de Microsoft Account.</span><span class="sxs-lookup"><span data-stu-id="65b49-131">See the [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API reference for more information on configuration options supported by Microsoft Account authentication.</span></span> <span data-ttu-id="65b49-132">Cela peut être utilisé pour demander différentes informations sur l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="65b49-132">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-microsoft-account"></a><span data-ttu-id="65b49-133">Connectez-vous avec un compte Microsoft</span><span class="sxs-lookup"><span data-stu-id="65b49-133">Sign in with Microsoft Account</span></span>

<span data-ttu-id="65b49-134">Exécuter l’et cliquez sur **connectez-vous**.</span><span class="sxs-lookup"><span data-stu-id="65b49-134">Run the and click **Log in**.</span></span> <span data-ttu-id="65b49-135">Une option permettant de se connecter avec Microsoft s’affiche.</span><span class="sxs-lookup"><span data-stu-id="65b49-135">An option to sign in with Microsoft appears.</span></span> <span data-ttu-id="65b49-136">Lorsque vous cliquez sur Microsoft, vous êtes redirigé vers Microsoft pour l’authentification.</span><span class="sxs-lookup"><span data-stu-id="65b49-136">When you click on Microsoft, you are redirected to Microsoft for authentication.</span></span> <span data-ttu-id="65b49-137">Après vous être connecté avec votre Account Microsoft (si pas déjà connecté), vous devez permettre à l’application d’accéder à vos informations :</span><span class="sxs-lookup"><span data-stu-id="65b49-137">After signing in with your Microsoft Account (if not already signed in) you will be prompted to let the app access your info:</span></span>

<span data-ttu-id="65b49-138">Appuyez sur **Oui** et vous êtes redirigé vers le site web où vous pouvez définir votre adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="65b49-138">Tap **Yes** and you will be redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="65b49-139">Vous êtes maintenant connecté à l’aide de vos informations d’identification Microsoft :</span><span class="sxs-lookup"><span data-stu-id="65b49-139">You are now logged in using your Microsoft credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="65b49-140">Résolution de problèmes</span><span class="sxs-lookup"><span data-stu-id="65b49-140">Troubleshooting</span></span>

* <span data-ttu-id="65b49-141">Si le fournisseur Microsoft Account vous redirige vers une page d’erreur de connexion, notez l’erreur title et description paramètres chaîne de requête directement après le `#` (hashtag) dans l’Uri.</span><span class="sxs-lookup"><span data-stu-id="65b49-141">If the Microsoft Account provider redirects you to a sign in error page, note the error title and description query string parameters directly following the `#` (hashtag) in the Uri.</span></span>

  <span data-ttu-id="65b49-142">Bien que le message d’erreur semble indiquer un problème avec l’authentification Microsoft, la cause la plus courante est votre application Uri ne pas corresponde à l’un de le **URI de redirection** spécifié pour le **Web** plateforme .</span><span class="sxs-lookup"><span data-stu-id="65b49-142">Although the error message seems to indicate a problem with Microsoft authentication, the most common cause is your application Uri not matching any of the **Redirect URIs** specified for the **Web** platform.</span></span>
* <span data-ttu-id="65b49-143">Si l’identité n’est pas configurée en appelant `services.AddIdentity` dans `ConfigureServices`, toute tentative authentifier entraîne *ArgumentException : L’option 'SignInScheme' doit être fournie*.</span><span class="sxs-lookup"><span data-stu-id="65b49-143">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="65b49-144">Le modèle de projet utilisé dans cet exemple permet de s’assurer que cela est fait.</span><span class="sxs-lookup"><span data-stu-id="65b49-144">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="65b49-145">Si la base de données de site n’a pas été créé en appliquant la migration initiale, vous obtiendrez *une opération de base de données a échoué lors du traitement de la demande* erreur.</span><span class="sxs-lookup"><span data-stu-id="65b49-145">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="65b49-146">Appuyez sur **appliquer les Migrations** pour créer la base de données et actualiser pour passer à l’erreur.</span><span class="sxs-lookup"><span data-stu-id="65b49-146">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="65b49-147">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="65b49-147">Next steps</span></span>

* <span data-ttu-id="65b49-148">Cet article vous a montré comment vous pouvez vous authentifier auprès de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="65b49-148">This article showed how you can authenticate with Microsoft.</span></span> <span data-ttu-id="65b49-149">Vous pouvez suivre une approche similaire pour s’authentifier auprès d’autres fournisseurs répertoriés sur le [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="65b49-149">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="65b49-150">Une fois que vous publiez votre site web à l’application web Azure, créez un nouveau client secrets dans le portail des développeurs Microsoft.</span><span class="sxs-lookup"><span data-stu-id="65b49-150">Once you publish your web site to Azure web app, create a new client secrets in the Microsoft Developer Portal.</span></span>

* <span data-ttu-id="65b49-151">Définir le `Authentication:Microsoft:ClientId` et `Authentication:Microsoft:ClientSecret` en tant que paramètres d’application dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="65b49-151">Set the `Authentication:Microsoft:ClientId` and `Authentication:Microsoft:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="65b49-152">Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="65b49-152">The configuration system is set up to read keys from environment variables.</span></span>