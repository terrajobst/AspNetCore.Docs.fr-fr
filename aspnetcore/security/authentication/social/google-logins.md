---
title: Programme d’installation de la connexion externe Google dans ASP.NET Core
author: rick-anderson
description: Ce didacticiel montre l’intégration de l’authentification d’utilisateur de compte Google dans une application ASP.NET Core existante.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/19/2019
uid: security/authentication/google-logins
ms.openlocfilehash: b0edac411e73cd2eec7c4e212b99971577f59cfb
ms.sourcegitcommit: 06a455d63ff7d6b571ca832e8117f4ac9d646baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67316448"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="48282-103">Programme d’installation de la connexion externe Google dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="48282-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="48282-104">Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="48282-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="48282-105">[Les API Google + héritées ont été arrêtées à compter du 7 mars 2019](https://developers.google.com/+/api-shutdown).</span><span class="sxs-lookup"><span data-stu-id="48282-105">[Legacy Google+ APIs have been shut down as of March 7, 2019](https://developers.google.com/+/api-shutdown).</span></span> <span data-ttu-id="48282-106">Google + se connecter et les développeurs doivent déplacer vers une nouvelle connexion de Google dans le système.</span><span class="sxs-lookup"><span data-stu-id="48282-106">Google+ sign in and developers must move to a new Google sign in system.</span></span> <span data-ttu-id="48282-107">Les packages ASP.NET Core 2.1 et 2.2 pour l’authentification Google ont mis à jour pour prendre en compte les modifications.</span><span class="sxs-lookup"><span data-stu-id="48282-107">The ASP.NET Core 2.1 and 2.2 packages for Google Authentication have be updated to accommodate the changes.</span></span> <span data-ttu-id="48282-108">Pour plus d’informations et d’atténuation temporaire pour ASP.NET Core, consultez [ce problème GitHub](https://github.com/aspnet/AspNetCore/issues/6486).</span><span class="sxs-lookup"><span data-stu-id="48282-108">For more information and temporary mitigations for ASP.NET Core, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/6486).</span></span> <span data-ttu-id="48282-109">Ce didacticiel a été mis à jour avec le nouveau processus d’installation.</span><span class="sxs-lookup"><span data-stu-id="48282-109">This tutorial has been updated with the new setup process.</span></span>

<span data-ttu-id="48282-110">Ce didacticiel vous montre comment permettre aux utilisateurs de se connecter avec leur compte Google à l’aide du projet ASP.NET Core 2.2 créé sur le [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="48282-110">This tutorial shows you how to enable users to sign in with their Google account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-a-google-api-console-project-and-client-id"></a><span data-ttu-id="48282-111">Créer un ID de client et de projet de Console d’API Google</span><span class="sxs-lookup"><span data-stu-id="48282-111">Create a Google API Console project and client ID</span></span>

* <span data-ttu-id="48282-112">Accédez à [l’intégration de Google Sign-In dans votre application web](https://developers.google.com/identity/sign-in/web/devconsole-project) et sélectionnez **configurer un projet**.</span><span class="sxs-lookup"><span data-stu-id="48282-112">Navigate to [Integrating Google Sign-In into your web app](https://developers.google.com/identity/sign-in/web/devconsole-project) and select **CONFIGURE A PROJECT**.</span></span>
* <span data-ttu-id="48282-113">Dans le **configurer votre client OAuth** boîte de dialogue, sélectionnez **serveur Web**.</span><span class="sxs-lookup"><span data-stu-id="48282-113">In the **Configure your OAuth client** dialog, select **Web server**.</span></span>
* <span data-ttu-id="48282-114">Dans le **URI de redirection autorisée** zone de texte, définissez l’URI de redirection.</span><span class="sxs-lookup"><span data-stu-id="48282-114">In the **Authorized redirect URIs** text entry box, set the redirect URI.</span></span> <span data-ttu-id="48282-115">Par exemple, `https://localhost:5001/signin-google`.</span><span class="sxs-lookup"><span data-stu-id="48282-115">For example, `https://localhost:5001/signin-google`</span></span>
* <span data-ttu-id="48282-116">Enregistrer le **ID Client** et **clé secrète Client**.</span><span class="sxs-lookup"><span data-stu-id="48282-116">Save the **Client ID** and **Client Secret**.</span></span>
* <span data-ttu-id="48282-117">Lorsque vous déployez le site, inscrire la nouvelle url publique à partir de la **Google Console**.</span><span class="sxs-lookup"><span data-stu-id="48282-117">When deploying the site, register the new public url from the **Google Console**.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="48282-118">Store Google ClientID et ClientSecret</span><span class="sxs-lookup"><span data-stu-id="48282-118">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="48282-119">Store paramètres sensibles tels que Google `Client ID` et `Client Secret` avec la [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="48282-119">Store sensitive settings such as the Google `Client ID` and `Client Secret` with the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="48282-120">Dans le cadre de ce didacticiel, nommez les jetons `Authentication:Google:ClientId` et `Authentication:Google:ClientSecret`:</span><span class="sxs-lookup"><span data-stu-id="48282-120">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`:</span></span>

```console
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="48282-121">Vous pouvez gérer vos informations d’identification de l’API et l’utilisation dans le [Console d’API](https://console.developers.google.com/apis/dashboard).</span><span class="sxs-lookup"><span data-stu-id="48282-121">You can manage your API credentials and usage in the [API Console](https://console.developers.google.com/apis/dashboard).</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="48282-122">Configurer l’authentification Google</span><span class="sxs-lookup"><span data-stu-id="48282-122">Configure Google authentication</span></span>

<span data-ttu-id="48282-123">Ajouter le service Google `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="48282-123">Add the Google service to `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupGoogle.cs?name=snippet_ConfigureServices&highlight=10-18)]

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a><span data-ttu-id="48282-124">Se connecter avec Google</span><span class="sxs-lookup"><span data-stu-id="48282-124">Sign in with Google</span></span>

* <span data-ttu-id="48282-125">Exécutez l’application et cliquez sur **connectez-vous**.</span><span class="sxs-lookup"><span data-stu-id="48282-125">Run the app and click **Log in**.</span></span> <span data-ttu-id="48282-126">Une option pour vous connecter avec Google s’affiche.</span><span class="sxs-lookup"><span data-stu-id="48282-126">An option to sign in with Google appears.</span></span>
* <span data-ttu-id="48282-127">Cliquez sur le **Google** bouton, qui redirige vers Google pour l’authentification.</span><span class="sxs-lookup"><span data-stu-id="48282-127">Click the **Google** button, which redirects to Google for authentication.</span></span>
* <span data-ttu-id="48282-128">Après avoir entré vos informations d’identification Google, vous êtes redirigé vers le site web.</span><span class="sxs-lookup"><span data-stu-id="48282-128">After entering your Google credentials, you are redirected back to the web site.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="48282-129">Consultez le <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> référence des API pour plus d’informations sur les options de configuration prises en charge par l’authentification Google.</span><span class="sxs-lookup"><span data-stu-id="48282-129">See the <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="48282-130">Cela peut être utilisé pour demander différentes informations sur l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="48282-130">This can be used to request different information about the user.</span></span>

## <a name="change-the-default-callback-uri"></a><span data-ttu-id="48282-131">Modifier l’URI de rappel par défaut</span><span class="sxs-lookup"><span data-stu-id="48282-131">Change the default callback URI</span></span>

<span data-ttu-id="48282-132">Le segment d’URI `/signin-google` est défini en tant que le rappel par défaut du fournisseur d’authentification Google.</span><span class="sxs-lookup"><span data-stu-id="48282-132">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="48282-133">Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel d’authentification Google via héritées [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propriété de la [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) classe.</span><span class="sxs-lookup"><span data-stu-id="48282-133">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="48282-134">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="48282-134">Troubleshooting</span></span>

* <span data-ttu-id="48282-135">Si l’authentification dans ne fonctionne pas et vous ne recevez pas les erreurs, basculer en mode de développement pour rendre le problème plus facile à déboguer.</span><span class="sxs-lookup"><span data-stu-id="48282-135">If the sign-in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="48282-136">Si l’identité n’est pas configurée en appelant `services.AddIdentity` dans `ConfigureServices`, la tentative d’authentifier les résultats dans *ArgumentException : L’option 'SignInScheme' doit être fournie*.</span><span class="sxs-lookup"><span data-stu-id="48282-136">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate results in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="48282-137">Le modèle de projet utilisé dans ce didacticiel permet de s’assurer que cela est fait.</span><span class="sxs-lookup"><span data-stu-id="48282-137">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="48282-138">Si la base de données de site n’a pas été créé en appliquant la migration initiale, vous obtenez *une opération de base de données a échoué lors du traitement de la demande* erreur.</span><span class="sxs-lookup"><span data-stu-id="48282-138">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="48282-139">Sélectionnez **appliquer les Migrations** pour créer la base de données et actualisez la page pour passer à l’erreur.</span><span class="sxs-lookup"><span data-stu-id="48282-139">Select **Apply Migrations** to create the database, and refresh the page to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="48282-140">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="48282-140">Next steps</span></span>

* <span data-ttu-id="48282-141">Cet article vous a montré comment vous pouvez vous authentifier avec Google.</span><span class="sxs-lookup"><span data-stu-id="48282-141">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="48282-142">Vous pouvez suivre une approche similaire pour s’authentifier auprès d’autres fournisseurs répertoriés sur le [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="48282-142">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>
* <span data-ttu-id="48282-143">Une fois que vous publiez l’application sur Azure, réinitialiser le `ClientSecret` dans la Console d’API Google.</span><span class="sxs-lookup"><span data-stu-id="48282-143">Once you publish the app to Azure, reset the `ClientSecret` in the Google API Console.</span></span>
* <span data-ttu-id="48282-144">Définir le `Authentication:Google:ClientId` et `Authentication:Google:ClientSecret` en tant que paramètres d’application dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="48282-144">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="48282-145">Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="48282-145">The configuration system is set up to read keys from environment variables.</span></span>
