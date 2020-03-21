---
title: Configuration de la connexion externe Twitter avec ASP.NET Core
author: rick-anderson
description: Ce didacticiel illustre l’intégration de l’authentification utilisateur de compte Twitter dans une application ASP.NET Core existante.
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2020
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/twitter-logins
ms.openlocfilehash: b848486415fd72ce6180b4cf8fc1ba00410d694a
ms.sourcegitcommit: 9b6e7f421c243963d5e419bdcfc5c4bde71499aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2020
ms.locfileid: "79989743"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a><span data-ttu-id="3e635-103">Configuration de la connexion externe Twitter avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3e635-103">Twitter external sign-in setup with ASP.NET Core</span></span>

<span data-ttu-id="3e635-104">Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3e635-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3e635-105">Cet exemple montre comment permettre aux utilisateurs de [se connecter avec leur compte Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) à l’aide d’un exemple de projet ASP.net Core 3,0 créé sur la [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="3e635-105">This sample shows how to enable users to [sign in with their Twitter account](https://dev.twitter.com/web/sign-in/desktop-browser) using a sample ASP.NET Core 3.0 project created on the [previous page](xref:security/authentication/social/index).</span></span>

## <a name="create-the-app-in-twitter"></a><span data-ttu-id="3e635-106">Créer l’application dans Twitter</span><span class="sxs-lookup"><span data-stu-id="3e635-106">Create the app in Twitter</span></span>

* <span data-ttu-id="3e635-107">Ajoutez le package NuGet [Microsoft. AspNetCore. Authentication. Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) au projet.</span><span class="sxs-lookup"><span data-stu-id="3e635-107">Add the [Microsoft.AspNetCore.Authentication.Twitter](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Twitter/3.0.0) NuGet package to the project.</span></span>

* <span data-ttu-id="3e635-108">Accédez à [https://apps.twitter.com/](https://apps.twitter.com/) et connectez-vous.</span><span class="sxs-lookup"><span data-stu-id="3e635-108">Navigate to [https://apps.twitter.com/](https://apps.twitter.com/) and sign in.</span></span> <span data-ttu-id="3e635-109">Si vous ne disposez pas déjà d’un compte Twitter, utilisez le lien **[Inscrivez-vous maintenant](https://twitter.com/signup)** pour en créer un.</span><span class="sxs-lookup"><span data-stu-id="3e635-109">If you don't already have a Twitter account, use the **[Sign up now](https://twitter.com/signup)** link to create one.</span></span>

* <span data-ttu-id="3e635-110">Sélectionnez **Créer une application**.</span><span class="sxs-lookup"><span data-stu-id="3e635-110">Select **Create an app**.</span></span> <span data-ttu-id="3e635-111">Renseignez le **nom**de l' **application** , la description de l’application et l’URI du **site Web** public (cela peut être temporaire jusqu’à ce que vous enregistriez le nom de domaine) :</span><span class="sxs-lookup"><span data-stu-id="3e635-111">Fill out the **App name**, **Application description** and public **Website** URI (this can be temporary until you register the domain name):</span></span>

* <span data-ttu-id="3e635-112">Cochez la case en regard de **activer la connexion avec Twitter** .</span><span class="sxs-lookup"><span data-stu-id="3e635-112">Check the box next to **Enable Sign in with Twitter**</span></span>

* <span data-ttu-id="3e635-113">Microsoft. AspNetCore. Identity exige que les utilisateurs aient une adresse de messagerie par défaut.</span><span class="sxs-lookup"><span data-stu-id="3e635-113">Microsoft.AspNetCore.Identity requires users to have an email address by default.</span></span> <span data-ttu-id="3e635-114">Accédez à l’onglet **autorisations** , cliquez sur le bouton **modifier** et cochez la case en regard de **demander l’adresse de messagerie des utilisateurs**.</span><span class="sxs-lookup"><span data-stu-id="3e635-114">Go to the **Permissions** tab, click the **Edit** button and check the box next to **Request email address from users**.</span></span>

* <span data-ttu-id="3e635-115">Entrez votre URI de développement avec `/signin-twitter` ajouté dans le champ **URL de rappel** (par exemple : `https://webapp128.azurewebsites.net/signin-twitter`).</span><span class="sxs-lookup"><span data-stu-id="3e635-115">Enter your development URI with `/signin-twitter` appended into the **Callback URLs** field (for example: `https://webapp128.azurewebsites.net/signin-twitter`).</span></span> <span data-ttu-id="3e635-116">Le schéma d’authentification Twitter configuré plus tard dans cet exemple gère automatiquement les demandes à `/signin-twitter` itinéraire pour implémenter le Flow OAuth.</span><span class="sxs-lookup"><span data-stu-id="3e635-116">The Twitter authentication scheme configured later in this sample will automatically handle requests at `/signin-twitter` route to implement the OAuth flow.</span></span>

  > [!NOTE]
  > <span data-ttu-id="3e635-117">Le segment d’URI `/signin-twitter` est défini comme rappel par défaut du fournisseur d’authentification Twitter.</span><span class="sxs-lookup"><span data-stu-id="3e635-117">The URI segment `/signin-twitter` is set as the default callback of the Twitter authentication provider.</span></span> <span data-ttu-id="3e635-118">Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel d’authentification Twitter via la propriété héritée [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) de la classe [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) .</span><span class="sxs-lookup"><span data-stu-id="3e635-118">You can change the default callback URI while configuring the Twitter authentication middleware via the inherited [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) property of the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) class.</span></span>

* <span data-ttu-id="3e635-119">Remplissez le reste du formulaire et sélectionnez **créer**.</span><span class="sxs-lookup"><span data-stu-id="3e635-119">Fill out the rest of the form and select **Create**.</span></span> <span data-ttu-id="3e635-120">Les détails de la nouvelle application s’affichent :</span><span class="sxs-lookup"><span data-stu-id="3e635-120">New application details are displayed:</span></span>

## <a name="store-the-twitter-consumer-api-key-and-secret"></a><span data-ttu-id="3e635-121">Stocker la clé et le secret de l’API du consommateur Twitter</span><span class="sxs-lookup"><span data-stu-id="3e635-121">Store the Twitter consumer API key and secret</span></span>

<span data-ttu-id="3e635-122">Stockez les paramètres sensibles, tels que la clé de l’API du consommateur Twitter et le secret avec le [Gestionnaire de secret](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="3e635-122">Store sensitive settings such as the Twitter consumer API key and secret with [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="3e635-123">Pour cet exemple, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="3e635-123">For this sample, use the following steps:</span></span>

1. <span data-ttu-id="3e635-124">Initialisez le projet pour le stockage secret conformément aux instructions de la procédure [activer le stockage secret](xref:security/app-secrets#enable-secret-storage).</span><span class="sxs-lookup"><span data-stu-id="3e635-124">Initialize the project for secret storage per the instructions at [Enable secret storage](xref:security/app-secrets#enable-secret-storage).</span></span>
1. <span data-ttu-id="3e635-125">Stockez les paramètres sensibles dans le magasin de secret local avec les clés de secrets `Authentication:Twitter:ConsumerKey` et `Authentication:Twitter:ConsumerSecret`:</span><span class="sxs-lookup"><span data-stu-id="3e635-125">Store the sensitive settings in the local secret store with the secrets keys `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret`:</span></span>

    ```dotnetcli
    dotnet user-secrets set "Authentication:Twitter:ConsumerAPIKey" "<consumer-api-key>"
    dotnet user-secrets set "Authentication:Twitter:ConsumerSecret" "<consumer-secret>"
    ```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="3e635-126">Ces jetons se trouvent sous l’onglet **clés et jetons d’accès** après la création d’une application Twitter :</span><span class="sxs-lookup"><span data-stu-id="3e635-126">These tokens can be found on the **Keys and Access Tokens** tab after creating a new Twitter application:</span></span>

## <a name="configure-twitter-authentication"></a><span data-ttu-id="3e635-127">Configurer l’authentification Twitter</span><span class="sxs-lookup"><span data-stu-id="3e635-127">Configure Twitter Authentication</span></span>

<span data-ttu-id="3e635-128">Ajoutez le service Twitter dans la méthode `ConfigureServices` du fichier *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="3e635-128">Add the Twitter service in the `ConfigureServices` method in *Startup.cs* file:</span></span>

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupTwitter3x.cs?name=snippet&highlight=10-15)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

<span data-ttu-id="3e635-129">Pour plus d’informations sur les options de configuration prises en charge par l’authentification Twitter, consultez la référence de l’API [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) .</span><span class="sxs-lookup"><span data-stu-id="3e635-129">See the [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) API reference for more information on configuration options supported by Twitter authentication.</span></span> <span data-ttu-id="3e635-130">Cela peut être utilisé pour demander différentes informations sur l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="3e635-130">This can be used to request different information about the user.</span></span>

## <a name="sign-in-with-twitter"></a><span data-ttu-id="3e635-131">Se connecter avec Twitter</span><span class="sxs-lookup"><span data-stu-id="3e635-131">Sign in with Twitter</span></span>

<span data-ttu-id="3e635-132">Exécutez l’application et sélectionnez **se connecter**.</span><span class="sxs-lookup"><span data-stu-id="3e635-132">Run the app and select **Log in**.</span></span> <span data-ttu-id="3e635-133">Une option permettant de se connecter avec Twitter s’affiche :</span><span class="sxs-lookup"><span data-stu-id="3e635-133">An option to sign in with Twitter appears:</span></span>

<span data-ttu-id="3e635-134">En cliquant sur **Twitter** , vous redirigez vers Twitter pour l’authentification :</span><span class="sxs-lookup"><span data-stu-id="3e635-134">Clicking on **Twitter** redirects to Twitter for authentication:</span></span>

<span data-ttu-id="3e635-135">Après avoir entré vos informations d’identification Twitter, vous êtes redirigé vers le site Web où vous pouvez définir votre adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="3e635-135">After entering your Twitter credentials, you are redirected back to the web site where you can set your email.</span></span>

<span data-ttu-id="3e635-136">Vous êtes maintenant connecté à l’aide de vos informations d’identification Twitter :</span><span class="sxs-lookup"><span data-stu-id="3e635-136">You are now logged in using your Twitter credentials:</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a><span data-ttu-id="3e635-137">Dépannage</span><span class="sxs-lookup"><span data-stu-id="3e635-137">Troubleshooting</span></span>

* <span data-ttu-id="3e635-138">**ASP.net Core 2. x uniquement :** Si l’identité n’est pas configurée en appelant `services.AddIdentity` dans `ConfigureServices`, toute tentative d’authentification entraînera *l’exception ArgumentException : l’option « SignInScheme » doit être fournie*.</span><span class="sxs-lookup"><span data-stu-id="3e635-138">**ASP.NET Core 2.x only:** If Identity isn't configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="3e635-139">Le modèle de projet utilisé dans cet exemple permet de s’assurer que cette opération est effectuée.</span><span class="sxs-lookup"><span data-stu-id="3e635-139">The project template used in this sample ensures that this is done.</span></span>
* <span data-ttu-id="3e635-140">Si la base de données de site n’a pas été créée en appliquant la migration initiale, vous obtiendrez *une opération de base de données qui a échoué lors du traitement de l’erreur de demande* .</span><span class="sxs-lookup"><span data-stu-id="3e635-140">If the site database has not been created by applying the initial migration, you will get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="3e635-141">Appuyez sur **appliquer des migrations** pour créer la base de données, puis sur Actualiser pour poursuivre l’erreur.</span><span class="sxs-lookup"><span data-stu-id="3e635-141">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3e635-142">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="3e635-142">Next steps</span></span>

* <span data-ttu-id="3e635-143">Cet article vous a montré comment vous pouvez vous authentifier avec Twitter.</span><span class="sxs-lookup"><span data-stu-id="3e635-143">This article showed how you can authenticate with Twitter.</span></span> <span data-ttu-id="3e635-144">Vous pouvez suivre une approche similaire pour vous authentifier auprès d’autres fournisseurs listés dans la [page précédente](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="3e635-144">You can follow a similar approach to authenticate with other providers listed on the [previous page](xref:security/authentication/social/index).</span></span>

* <span data-ttu-id="3e635-145">Une fois que vous publiez votre site Web dans l’application Web Azure, vous devez réinitialiser le `ConsumerSecret` dans le portail des développeurs Twitter.</span><span class="sxs-lookup"><span data-stu-id="3e635-145">Once you publish your web site to Azure web app, you should reset the `ConsumerSecret` in the Twitter developer portal.</span></span>

* <span data-ttu-id="3e635-146">Définissez les `Authentication:Twitter:ConsumerKey` et `Authentication:Twitter:ConsumerSecret` en tant que paramètres d’application dans le Portail Azure.</span><span class="sxs-lookup"><span data-stu-id="3e635-146">Set the `Authentication:Twitter:ConsumerKey` and `Authentication:Twitter:ConsumerSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="3e635-147">Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="3e635-147">The configuration system is set up to read keys from environment variables.</span></span>
