---
title: Configuration de la connexion à Google External dans ASP.NET Core
author: rick-anderson
description: Ce didacticiel illustre l’intégration de l’authentification utilisateur de compte Google dans une application ASP.NET Core existante.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/30/2019
uid: security/authentication/google-logins
ms.openlocfilehash: 83f45143eca1be43410880bfd875a3fce1d2e9c9
ms.sourcegitcommit: de0fc77487a4d342bcc30965ec5c142d10d22c03
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73143453"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="34bb3-103">Configuration de la connexion à Google External dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="34bb3-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="34bb3-104">Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="34bb3-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="34bb3-105">Ce didacticiel vous montre comment permettre aux utilisateurs de se connecter avec leur compte Google à l’aide du projet ASP.NET Core 3,0 créé sur la [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="34bb3-105">This tutorial shows you how to enable users to sign in with their Google account using the ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-a-google-api-console-project-and-client-id"></a><span data-ttu-id="34bb3-106">Créer un projet de console d’API Google et un ID client</span><span class="sxs-lookup"><span data-stu-id="34bb3-106">Create a Google API Console project and client ID</span></span>

* <span data-ttu-id="34bb3-107">Installez [Microsoft. AspNetCore. Authentication. Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google).</span><span class="sxs-lookup"><span data-stu-id="34bb3-107">Install [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google).</span></span>
* <span data-ttu-id="34bb3-108">Accédez à [l’intégration de la connexion Google à votre application Web](https://developers.google.com/identity/sign-in/web/devconsole-project) , puis sélectionnez **configurer un projet**.</span><span class="sxs-lookup"><span data-stu-id="34bb3-108">Navigate to [Integrating Google Sign-In into your web app](https://developers.google.com/identity/sign-in/web/devconsole-project) and select **CONFIGURE A PROJECT**.</span></span>
* <span data-ttu-id="34bb3-109">Dans la boîte de dialogue **configurer votre client OAuth** , sélectionnez **serveur Web**.</span><span class="sxs-lookup"><span data-stu-id="34bb3-109">In the **Configure your OAuth client** dialog, select **Web server**.</span></span>
* <span data-ttu-id="34bb3-110">Dans la zone d’entrée de texte **URI de redirection autorisés** , définissez l’URI de redirection.</span><span class="sxs-lookup"><span data-stu-id="34bb3-110">In the **Authorized redirect URIs** text entry box, set the redirect URI.</span></span> <span data-ttu-id="34bb3-111">Par exemple, `https://localhost:44312/signin-google`.</span><span class="sxs-lookup"><span data-stu-id="34bb3-111">For example, `https://localhost:44312/signin-google`</span></span>
* <span data-ttu-id="34bb3-112">Enregistrez l' **ID client** et la **clé secrète client**.</span><span class="sxs-lookup"><span data-stu-id="34bb3-112">Save the **Client ID** and **Client Secret**.</span></span>
* <span data-ttu-id="34bb3-113">Lors du déploiement du site, inscrivez la nouvelle URL publique à partir de la **console Google**.</span><span class="sxs-lookup"><span data-stu-id="34bb3-113">When deploying the site, register the new public url from the **Google Console**.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="34bb3-114">Stocker Google ClientID et ClientSecret</span><span class="sxs-lookup"><span data-stu-id="34bb3-114">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="34bb3-115">Stockez les paramètres sensibles tels que Google `Client ID` et `Client Secret` avec le [Gestionnaire de secret](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="34bb3-115">Store sensitive settings such as the Google `Client ID` and `Client Secret` with the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="34bb3-116">Dans le cadre de ce didacticiel, nommez les jetons `Authentication:Google:ClientId` et `Authentication:Google:ClientSecret`:</span><span class="sxs-lookup"><span data-stu-id="34bb3-116">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`:</span></span>

```dotnetcli
dotnet user-secrets set "Authentication:Google:ClientId" "<client id>"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="34bb3-117">Vous pouvez gérer les informations d’identification et l’utilisation de votre API dans la [console d’API](https://console.developers.google.com/apis/dashboard).</span><span class="sxs-lookup"><span data-stu-id="34bb3-117">You can manage your API credentials and usage in the [API Console](https://console.developers.google.com/apis/dashboard).</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="34bb3-118">Configurer l’authentification Google</span><span class="sxs-lookup"><span data-stu-id="34bb3-118">Configure Google authentication</span></span>

<span data-ttu-id="34bb3-119">Ajoutez le service Google à `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="34bb3-119">Add the Google service to `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupGoogle3x.cs?highlight=11-19)]

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a><span data-ttu-id="34bb3-120">Se connecter avec Google</span><span class="sxs-lookup"><span data-stu-id="34bb3-120">Sign in with Google</span></span>

* <span data-ttu-id="34bb3-121">Exécutez l’application, puis cliquez sur **se connecter**.</span><span class="sxs-lookup"><span data-stu-id="34bb3-121">Run the app and click **Log in**.</span></span> <span data-ttu-id="34bb3-122">Une option de connexion avec Google s’affiche.</span><span class="sxs-lookup"><span data-stu-id="34bb3-122">An option to sign in with Google appears.</span></span>
* <span data-ttu-id="34bb3-123">Cliquez sur le bouton **Google** , qui redirige vers Google pour l’authentification.</span><span class="sxs-lookup"><span data-stu-id="34bb3-123">Click the **Google** button, which redirects to Google for authentication.</span></span>
* <span data-ttu-id="34bb3-124">Après avoir entré vos informations d’identification Google, vous êtes redirigé vers le site Web.</span><span class="sxs-lookup"><span data-stu-id="34bb3-124">After entering your Google credentials, you are redirected back to the web site.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="34bb3-125">Pour plus d’informations sur les options de configuration prises en charge par l’authentification Google, consultez les informations de référence sur l’API <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions>.</span><span class="sxs-lookup"><span data-stu-id="34bb3-125">See the <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="34bb3-126">Cela peut être utilisé pour demander différentes informations sur l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="34bb3-126">This can be used to request different information about the user.</span></span>

## <a name="change-the-default-callback-uri"></a><span data-ttu-id="34bb3-127">Modifier l’URI de rappel par défaut</span><span class="sxs-lookup"><span data-stu-id="34bb3-127">Change the default callback URI</span></span>

<span data-ttu-id="34bb3-128">Le segment d’URI `/signin-google` est défini comme rappel par défaut du fournisseur d’authentification Google.</span><span class="sxs-lookup"><span data-stu-id="34bb3-128">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="34bb3-129">Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel d’authentification Google via la propriété héritée [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) de la classe [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) .</span><span class="sxs-lookup"><span data-stu-id="34bb3-129">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="34bb3-130">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="34bb3-130">Troubleshooting</span></span>

* <span data-ttu-id="34bb3-131">Si la connexion ne fonctionne pas et que vous ne recevez pas d’erreurs, passez en mode développement pour faciliter le débogage du problème.</span><span class="sxs-lookup"><span data-stu-id="34bb3-131">If the sign-in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="34bb3-132">Si l’identité n’est pas configurée en appelant `services.AddIdentity` dans `ConfigureServices`, toute tentative d’authentification des résultats est *ArgumentException : l’option’SignInScheme’doit être fournie*.</span><span class="sxs-lookup"><span data-stu-id="34bb3-132">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate results in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="34bb3-133">Le modèle de projet utilisé dans ce didacticiel permet d’effectuer cette opération.</span><span class="sxs-lookup"><span data-stu-id="34bb3-133">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="34bb3-134">Si la base de données de site n’a pas été créée en appliquant la migration initiale, vous recevez *une opération de base de données qui a échoué lors du traitement de l’erreur de demande* .</span><span class="sxs-lookup"><span data-stu-id="34bb3-134">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="34bb3-135">Sélectionnez **appliquer les migrations** pour créer la base de données, puis actualisez la page pour poursuivre l’erreur.</span><span class="sxs-lookup"><span data-stu-id="34bb3-135">Select **Apply Migrations** to create the database, and refresh the page to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="34bb3-136">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="34bb3-136">Next steps</span></span>

* <span data-ttu-id="34bb3-137">Cet article vous a montré comment vous pouvez vous authentifier auprès de Google.</span><span class="sxs-lookup"><span data-stu-id="34bb3-137">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="34bb3-138">Vous pouvez suivre une approche similaire pour vous authentifier auprès d’autres fournisseurs listés dans la [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="34bb3-138">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>
* <span data-ttu-id="34bb3-139">Une fois que vous avez publié l’application dans Azure, réinitialisez la `ClientSecret` dans la console de l’API Google.</span><span class="sxs-lookup"><span data-stu-id="34bb3-139">Once you publish the app to Azure, reset the `ClientSecret` in the Google API Console.</span></span>
* <span data-ttu-id="34bb3-140">Définissez les `Authentication:Google:ClientId` et `Authentication:Google:ClientSecret` en tant que paramètres d’application dans le Portail Azure.</span><span class="sxs-lookup"><span data-stu-id="34bb3-140">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="34bb3-141">Le système de configuration est configuré pour lire des clés à partir de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="34bb3-141">The configuration system is set up to read keys from environment variables.</span></span>
