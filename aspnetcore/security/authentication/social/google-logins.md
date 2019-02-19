---
title: Programme d’installation de la connexion externe Google dans ASP.NET Core
author: rick-anderson
description: Ce didacticiel montre l’intégration de l’authentification d’utilisateur de compte Google dans une application ASP.NET Core existante.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 1/11/2019
uid: security/authentication/google-logins
ms.openlocfilehash: 5b6bfaafba68eaf15a60b7c512a9e7406e3112ee
ms.sourcegitcommit: 6d8c362a3cb4c634e64cbb3d28069c36bc973581
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2019
ms.locfileid: "56418664"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="25e1c-103">Programme d’installation de la connexion externe Google dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="25e1c-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="25e1c-104">Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="25e1c-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="25e1c-105">En janvier 2019 Google commencé à [arrêter](https://developers.google.com/+/api-shutdown) Google + se connecter et les développeurs doivent déplacer vers une nouvelle connexion de Google dans le système en mars.</span><span class="sxs-lookup"><span data-stu-id="25e1c-105">In January 2019 Google started to [shut down](https://developers.google.com/+/api-shutdown) Google+ sign in and developers must move to a new Google sign in system by March.</span></span> <span data-ttu-id="25e1c-106">L’ASP.NET Core 2.1 et 2.2 packages pour l’authentification Google seront mis à jour en février pour prendre en compte les modifications.</span><span class="sxs-lookup"><span data-stu-id="25e1c-106">The ASP.NET Core 2.1 and 2.2 packages for Google Authentication will be updated in February to accommodate the changes.</span></span> <span data-ttu-id="25e1c-107">Pour plus d’informations et d’atténuation temporaire pour ASP.NET Core, consultez [ce problème GitHub](https://github.com/aspnet/AspNetCore/issues/6486).</span><span class="sxs-lookup"><span data-stu-id="25e1c-107">For more information and temporary mitigations for ASP.NET Core, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/6486).</span></span> <span data-ttu-id="25e1c-108">Ce didacticiel a été mis à jour avec le nouveau processus d’installation.</span><span class="sxs-lookup"><span data-stu-id="25e1c-108">This tutorial has been updated with the new setup process.</span></span>

<span data-ttu-id="25e1c-109">Ce didacticiel vous montre comment permettre aux utilisateurs de se connecter avec leur compte Google à l’aide du projet ASP.NET Core 2.2 créé sur le [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="25e1c-109">This tutorial shows you how to enable users to sign in with their Google account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-a-google-api-console-project-and-client-id"></a><span data-ttu-id="25e1c-110">Créer un ID de client et de projet de Console d’API Google</span><span class="sxs-lookup"><span data-stu-id="25e1c-110">Create a Google API Console project and client ID</span></span>

* <span data-ttu-id="25e1c-111">Accédez à [l’intégration de Google Sign-In dans votre application web](https://developers.google.com/identity/sign-in/web/devconsole-project) et sélectionnez **configurer un projet**.</span><span class="sxs-lookup"><span data-stu-id="25e1c-111">Navigate to [Integrating Google Sign-In into your web app](https://developers.google.com/identity/sign-in/web/devconsole-project) and select **CONFIGURE A PROJECT**.</span></span>
* <span data-ttu-id="25e1c-112">Dans le **configurer votre client OAuth** boîte de dialogue, sélectionnez **serveur Web**.</span><span class="sxs-lookup"><span data-stu-id="25e1c-112">In the **Configure your OAuth client** dialog, select **Web server**.</span></span>
* <span data-ttu-id="25e1c-113">Dans le **URI de redirection autorisée** zone de texte, définissez l’URI de redirection.</span><span class="sxs-lookup"><span data-stu-id="25e1c-113">In the **Authorized redirect URIs** text entry box, set the redirect URI.</span></span> <span data-ttu-id="25e1c-114">Par exemple, `https://localhost:5001/signin-google`.</span><span class="sxs-lookup"><span data-stu-id="25e1c-114">For example, `https://localhost:5001/signin-google`</span></span>
* <span data-ttu-id="25e1c-115">Enregistrer le **ID Client** et **clé secrète Client**.</span><span class="sxs-lookup"><span data-stu-id="25e1c-115">Save the **Client ID** and **Client Secret**.</span></span>
* <span data-ttu-id="25e1c-116">Lorsque vous déployez le site, inscrire la nouvelle url publique à partir de la **Google Console**.</span><span class="sxs-lookup"><span data-stu-id="25e1c-116">When deploying the site, register the new public url from the **Google Console**.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="25e1c-117">Store Google ClientID et ClientSecret</span><span class="sxs-lookup"><span data-stu-id="25e1c-117">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="25e1c-118">Store paramètres sensibles tels que Google `Client ID` et `Client Secret` avec la [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="25e1c-118">Store sensitive settings such as the Google `Client ID` and `Client Secret` with the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="25e1c-119">Dans le cadre de ce didacticiel, nommez les jetons `Authentication:Google:ClientId` et `Authentication:Google:ClientSecret`:</span><span class="sxs-lookup"><span data-stu-id="25e1c-119">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`:</span></span>

```console
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

<span data-ttu-id="25e1c-120">Vous pouvez gérer vos informations d’identification de l’API et l’utilisation dans le [Console d’API](https://console.developers.google.com/apis/dashboard).</span><span class="sxs-lookup"><span data-stu-id="25e1c-120">You can manage your API credentials and usage in the [API Console](https://console.developers.google.com/apis/dashboard).</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="25e1c-121">Configurer l’authentification Google</span><span class="sxs-lookup"><span data-stu-id="25e1c-121">Configure Google authentication</span></span>

<span data-ttu-id="25e1c-122">Ajouter le service Google `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="25e1c-122">Add the Google service to `Startup.ConfigureServices`.</span></span>

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a><span data-ttu-id="25e1c-123">Se connecter avec Google</span><span class="sxs-lookup"><span data-stu-id="25e1c-123">Sign in with Google</span></span>

* <span data-ttu-id="25e1c-124">Exécutez l’application et cliquez sur **connectez-vous**.</span><span class="sxs-lookup"><span data-stu-id="25e1c-124">Run the app and click **Log in**.</span></span> <span data-ttu-id="25e1c-125">Une option pour vous connecter avec Google s’affiche.</span><span class="sxs-lookup"><span data-stu-id="25e1c-125">An option to sign in with Google appears.</span></span>
* <span data-ttu-id="25e1c-126">Cliquez sur le **Google** bouton, qui redirige vers Google pour l’authentification.</span><span class="sxs-lookup"><span data-stu-id="25e1c-126">Click the **Google** button, which redirects to Google for authentication.</span></span>
* <span data-ttu-id="25e1c-127">Après avoir entré vos informations d’identification Google, vous êtes redirigé vers le site web.</span><span class="sxs-lookup"><span data-stu-id="25e1c-127">After entering your Google credentials, you are redirected back to the web site.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="25e1c-128">Consultez le [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) référence des API pour plus d’informations sur les options de configuration prises en charge par l’authentification Google.</span><span class="sxs-lookup"><span data-stu-id="25e1c-128">See the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="25e1c-129">Cela peut être utilisé pour demander différentes informations sur l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="25e1c-129">This can be used to request different information about the user.</span></span>

## <a name="change-the-default-callback-uri"></a><span data-ttu-id="25e1c-130">Modifier l’URI de rappel par défaut</span><span class="sxs-lookup"><span data-stu-id="25e1c-130">Change the default callback URI</span></span>

<span data-ttu-id="25e1c-131">Le segment d’URI `/signin-google` est défini en tant que le rappel par défaut du fournisseur d’authentification Google.</span><span class="sxs-lookup"><span data-stu-id="25e1c-131">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="25e1c-132">Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel d’authentification Google via héritées [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propriété de la [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) classe.</span><span class="sxs-lookup"><span data-stu-id="25e1c-132">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="25e1c-133">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="25e1c-133">Troubleshooting</span></span>

* <span data-ttu-id="25e1c-134">Si l’authentification dans ne fonctionne pas et vous ne recevez pas les erreurs, basculer en mode de développement pour rendre le problème plus facile à déboguer.</span><span class="sxs-lookup"><span data-stu-id="25e1c-134">If the sign-in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="25e1c-135">Si l’identité n’est pas configurée en appelant `services.AddIdentity` dans `ConfigureServices`, la tentative d’authentifier les résultats dans *ArgumentException : L’option 'SignInScheme' doit être fournie*.</span><span class="sxs-lookup"><span data-stu-id="25e1c-135">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate results in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="25e1c-136">Le modèle de projet utilisé dans ce didacticiel permet de s’assurer que cela est fait.</span><span class="sxs-lookup"><span data-stu-id="25e1c-136">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="25e1c-137">Si la base de données de site n’a pas été créé en appliquant la migration initiale, vous obtenez *une opération de base de données a échoué lors du traitement de la demande* erreur.</span><span class="sxs-lookup"><span data-stu-id="25e1c-137">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="25e1c-138">Appuyez sur **appliquer les Migrations** pour créer la base de données et actualiser pour passer à l’erreur.</span><span class="sxs-lookup"><span data-stu-id="25e1c-138">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="25e1c-139">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="25e1c-139">Next steps</span></span>

* <span data-ttu-id="25e1c-140">Cet article vous a montré comment vous pouvez vous authentifier avec Google.</span><span class="sxs-lookup"><span data-stu-id="25e1c-140">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="25e1c-141">Vous pouvez suivre une approche similaire pour s’authentifier auprès d’autres fournisseurs répertoriés sur le [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="25e1c-141">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>
* <span data-ttu-id="25e1c-142">Une fois que vous publiez l’application sur Azure, réinitialiser le `ClientSecret` dans la Console d’API Google.</span><span class="sxs-lookup"><span data-stu-id="25e1c-142">Once you publish the app to Azure, reset the `ClientSecret` in the Google API Console.</span></span>
* <span data-ttu-id="25e1c-143">Définir le `Authentication:Google:ClientId` et `Authentication:Google:ClientSecret` en tant que paramètres d’application dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="25e1c-143">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="25e1c-144">Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="25e1c-144">The configuration system is set up to read keys from environment variables.</span></span>
