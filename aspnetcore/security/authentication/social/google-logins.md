---
title: Programme d’installation de la connexion externe Google dans ASP.NET Core
author: rick-anderson
description: Ce didacticiel montre l’intégration de l’authentification d’utilisateur de compte Google dans une application ASP.NET Core existante.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/19/2019
uid: security/authentication/google-logins
ms.openlocfilehash: e12d831d2e0a5c9acae5ea41fb4187ad4ca6b0ea
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082485"
---
# <a name="google-external-login-setup-in-aspnet-core"></a><span data-ttu-id="62fe1-103">Programme d’installation de la connexion externe Google dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="62fe1-103">Google external login setup in ASP.NET Core</span></span>

<span data-ttu-id="62fe1-104">Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="62fe1-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="62fe1-105">[Les API Google + héritées ont été arrêtées depuis le 7 mars 2019](https://developers.google.com/+/api-shutdown).</span><span class="sxs-lookup"><span data-stu-id="62fe1-105">[Legacy Google+ APIs have been shut down as of March 7, 2019](https://developers.google.com/+/api-shutdown).</span></span> <span data-ttu-id="62fe1-106">Google + Connect et les développeurs doivent passer à un nouveau système de connexion Google.</span><span class="sxs-lookup"><span data-stu-id="62fe1-106">Google+ sign in and developers must move to a new Google sign in system.</span></span> <span data-ttu-id="62fe1-107">Les packages ASP.NET Core 2,1 et 2,2 pour l’authentification Google ont été mis à jour pour prendre en compte les modifications.</span><span class="sxs-lookup"><span data-stu-id="62fe1-107">The ASP.NET Core 2.1 and 2.2 packages for Google Authentication have be updated to accommodate the changes.</span></span> <span data-ttu-id="62fe1-108">Pour plus d’informations et pour obtenir des solutions de contournement temporaires pour ASP.NET Core, consultez [ce problème GitHub](https://github.com/aspnet/AspNetCore/issues/6486).</span><span class="sxs-lookup"><span data-stu-id="62fe1-108">For more information and temporary mitigations for ASP.NET Core, see [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/6486).</span></span> <span data-ttu-id="62fe1-109">Ce didacticiel a été mis à jour avec le nouveau processus d’installation.</span><span class="sxs-lookup"><span data-stu-id="62fe1-109">This tutorial has been updated with the new setup process.</span></span>

<span data-ttu-id="62fe1-110">Ce didacticiel vous montre comment permettre aux utilisateurs de se connecter avec leur compte Google à l’aide du projet ASP.NET Core 2,2 créé sur la [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="62fe1-110">This tutorial shows you how to enable users to sign in with their Google account using the ASP.NET Core 2.2 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-a-google-api-console-project-and-client-id"></a><span data-ttu-id="62fe1-111">Créer un projet de console d’API Google et un ID client</span><span class="sxs-lookup"><span data-stu-id="62fe1-111">Create a Google API Console project and client ID</span></span>

* <span data-ttu-id="62fe1-112">Accédez à [l’intégration de la connexion Google à votre application Web](https://developers.google.com/identity/sign-in/web/devconsole-project) , puis sélectionnez **configurer un projet**.</span><span class="sxs-lookup"><span data-stu-id="62fe1-112">Navigate to [Integrating Google Sign-In into your web app](https://developers.google.com/identity/sign-in/web/devconsole-project) and select **CONFIGURE A PROJECT**.</span></span>
* <span data-ttu-id="62fe1-113">Dans la boîte de dialogue **configurer votre client OAuth** , sélectionnez **serveur Web**.</span><span class="sxs-lookup"><span data-stu-id="62fe1-113">In the **Configure your OAuth client** dialog, select **Web server**.</span></span>
* <span data-ttu-id="62fe1-114">Dans la zone d’entrée de texte **URI de redirection autorisés** , définissez l’URI de redirection.</span><span class="sxs-lookup"><span data-stu-id="62fe1-114">In the **Authorized redirect URIs** text entry box, set the redirect URI.</span></span> <span data-ttu-id="62fe1-115">Par exemple, `https://localhost:5001/signin-google`.</span><span class="sxs-lookup"><span data-stu-id="62fe1-115">For example, `https://localhost:5001/signin-google`</span></span>
* <span data-ttu-id="62fe1-116">Enregistrez l' **ID client** et la **clé secrète client**.</span><span class="sxs-lookup"><span data-stu-id="62fe1-116">Save the **Client ID** and **Client Secret**.</span></span>
* <span data-ttu-id="62fe1-117">Lors du déploiement du site, inscrivez la nouvelle URL publique à partir de la **console Google**.</span><span class="sxs-lookup"><span data-stu-id="62fe1-117">When deploying the site, register the new public url from the **Google Console**.</span></span>

## <a name="store-google-clientid-and-clientsecret"></a><span data-ttu-id="62fe1-118">Store Google ClientID et ClientSecret</span><span class="sxs-lookup"><span data-stu-id="62fe1-118">Store Google ClientID and ClientSecret</span></span>

<span data-ttu-id="62fe1-119">Stockez les paramètres sensibles tels que `Client ID` Google `Client Secret` et avec le [Gestionnaire de secret](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="62fe1-119">Store sensitive settings such as the Google `Client ID` and `Client Secret` with the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="62fe1-120">Dans le cadre de ce didacticiel, nommez les jetons `Authentication:Google:ClientId` et `Authentication:Google:ClientSecret`:</span><span class="sxs-lookup"><span data-stu-id="62fe1-120">For the purposes of this tutorial, name the tokens `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret`:</span></span>

```dotnetcli
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="62fe1-121">Vous pouvez gérer les informations d’identification et l’utilisation de votre API dans la [console d’API](https://console.developers.google.com/apis/dashboard).</span><span class="sxs-lookup"><span data-stu-id="62fe1-121">You can manage your API credentials and usage in the [API Console](https://console.developers.google.com/apis/dashboard).</span></span>

## <a name="configure-google-authentication"></a><span data-ttu-id="62fe1-122">Configurer l’authentification Google</span><span class="sxs-lookup"><span data-stu-id="62fe1-122">Configure Google authentication</span></span>

<span data-ttu-id="62fe1-123">Ajoutez le service Google à `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="62fe1-123">Add the Google service to `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/StartupGoogle.cs?name=snippet_ConfigureServices&highlight=10-18)]

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a><span data-ttu-id="62fe1-124">Se connecter avec Google</span><span class="sxs-lookup"><span data-stu-id="62fe1-124">Sign in with Google</span></span>

* <span data-ttu-id="62fe1-125">Exécutez l’application, puis cliquez sur **se connecter**.</span><span class="sxs-lookup"><span data-stu-id="62fe1-125">Run the app and click **Log in**.</span></span> <span data-ttu-id="62fe1-126">Une option de connexion avec Google s’affiche.</span><span class="sxs-lookup"><span data-stu-id="62fe1-126">An option to sign in with Google appears.</span></span>
* <span data-ttu-id="62fe1-127">Cliquez sur le bouton **Google** , qui redirige vers Google pour l’authentification.</span><span class="sxs-lookup"><span data-stu-id="62fe1-127">Click the **Google** button, which redirects to Google for authentication.</span></span>
* <span data-ttu-id="62fe1-128">Après avoir entré vos informations d’identification Google, vous êtes redirigé vers le site Web.</span><span class="sxs-lookup"><span data-stu-id="62fe1-128">After entering your Google credentials, you are redirected back to the web site.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="62fe1-129">Pour plus <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> d’informations sur les options de configuration prises en charge par l’authentification Google, consultez la référence de l’API.</span><span class="sxs-lookup"><span data-stu-id="62fe1-129">See the <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> API reference for more information on configuration options supported by Google authentication.</span></span> <span data-ttu-id="62fe1-130">Cela peut être utilisé pour demander différentes informations sur l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="62fe1-130">This can be used to request different information about the user.</span></span>

## <a name="change-the-default-callback-uri"></a><span data-ttu-id="62fe1-131">Modifier l’URI de rappel par défaut</span><span class="sxs-lookup"><span data-stu-id="62fe1-131">Change the default callback URI</span></span>

<span data-ttu-id="62fe1-132">Le segment d’URI `/signin-google` est défini en tant que le rappel par défaut du fournisseur d’authentification Google.</span><span class="sxs-lookup"><span data-stu-id="62fe1-132">The URI segment `/signin-google` is set as the default callback of the Google authentication provider.</span></span> <span data-ttu-id="62fe1-133">Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel d’authentification Google via héritées [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propriété de la [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) classe.</span><span class="sxs-lookup"><span data-stu-id="62fe1-133">You can change the default callback URI while configuring the Google authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) class.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="62fe1-134">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="62fe1-134">Troubleshooting</span></span>

* <span data-ttu-id="62fe1-135">Si la connexion ne fonctionne pas et que vous ne recevez pas d’erreurs, passez en mode développement pour faciliter le débogage du problème.</span><span class="sxs-lookup"><span data-stu-id="62fe1-135">If the sign-in doesn't work and you aren't getting any errors, switch to development mode to make the issue easier to debug.</span></span>
* <span data-ttu-id="62fe1-136">Si l’identité n’est pas `services.AddIdentity` configurée en appelant dans `ConfigureServices`, une *tentative d’authentification des résultats est ArgumentException : L’option « SignInScheme » doit être fournie*.</span><span class="sxs-lookup"><span data-stu-id="62fe1-136">If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate results in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="62fe1-137">Le modèle de projet utilisé dans ce didacticiel permet de s’assurer que cela est fait.</span><span class="sxs-lookup"><span data-stu-id="62fe1-137">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="62fe1-138">Si la base de données de site n’a pas été créé en appliquant la migration initiale, vous obtenez *une opération de base de données a échoué lors du traitement de la demande* erreur.</span><span class="sxs-lookup"><span data-stu-id="62fe1-138">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="62fe1-139">Sélectionnez **appliquer les migrations** pour créer la base de données, puis actualisez la page pour poursuivre l’erreur.</span><span class="sxs-lookup"><span data-stu-id="62fe1-139">Select **Apply Migrations** to create the database, and refresh the page to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="62fe1-140">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="62fe1-140">Next steps</span></span>

* <span data-ttu-id="62fe1-141">Cet article vous a montré comment vous pouvez vous authentifier avec Google.</span><span class="sxs-lookup"><span data-stu-id="62fe1-141">This article showed how you can authenticate with Google.</span></span> <span data-ttu-id="62fe1-142">Vous pouvez suivre une approche similaire pour s’authentifier auprès d’autres fournisseurs répertoriés sur le [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="62fe1-142">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>
* <span data-ttu-id="62fe1-143">Une fois que vous avez publié l’application dans Azure `ClientSecret` , réinitialisez la dans la console de l’API Google.</span><span class="sxs-lookup"><span data-stu-id="62fe1-143">Once you publish the app to Azure, reset the `ClientSecret` in the Google API Console.</span></span>
* <span data-ttu-id="62fe1-144">Définir le `Authentication:Google:ClientId` et `Authentication:Google:ClientSecret` en tant que paramètres d’application dans le portail Azure.</span><span class="sxs-lookup"><span data-stu-id="62fe1-144">Set the `Authentication:Google:ClientId` and `Authentication:Google:ClientSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="62fe1-145">Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="62fe1-145">The configuration system is set up to read keys from environment variables.</span></span>
