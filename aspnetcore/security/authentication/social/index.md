---
title: Authentification à l’aide de fournisseurs externes (Facebook, Google et autres) dans ASP.NET Core
author: rick-anderson
description: Ce didacticiel montre comment générer une application ASP.NET Core 2.x à l’aide d’OAuth 2.0 avec des fournisseurs d’authentification externes.
ms.author: riande
ms.date: 11/01/2016
uid: security/authentication/social/index
ms.openlocfilehash: b2176ef40faaee02c5ef53dbb92a802212e58e8b
ms.sourcegitcommit: e12f45ddcbe99102a74d4077df27d6c0ebba49c1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/15/2018
ms.locfileid: "39063323"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="6a2c9-103">Authentification à l’aide de fournisseurs externes (Facebook, Google et autres) dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6a2c9-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="6a2c9-104">Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6a2c9-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6a2c9-105">Ce didacticiel montre comment créer une application ASP.NET Core 2.x qui permet aux utilisateurs de se connecter à l’aide d’OAuth 2.0 avec des informations d’identification provenant de fournisseurs d’authentification externes.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-105">This tutorial demonstrates how to build an ASP.NET Core 2.x app that enables users to log in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="6a2c9-106">Les fournisseurs [Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins) et [Microsoft](xref:security/authentication/microsoft-logins) sont traités dans les sections qui suivent.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="6a2c9-107">D’autres fournisseurs sont disponibles dans des packages tiers, comme [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) et [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="6a2c9-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Icônes de réseau social pour Facebook, Twitter, Google plus et Windows](index/_static/social.png)

<span data-ttu-id="6a2c9-109">Permettre aux utilisateurs de se connecter avec leurs informations d’identification existantes est pratique pour eux et transfère une grande partie de la complexité de la gestion du processus de connexion sur un tiers.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-109">Enabling users to sign in with their existing credentials is convenient for the users and shifts many of the complexities of managing the sign-in process onto a third party.</span></span> <span data-ttu-id="6a2c9-110">Pour obtenir des exemples de la façon dont les connexions des réseaux sociaux peuvent amener du trafic et des conversions de clients, consultez les études de cas par [Facebook](https://www.facebook.com/unsupportedbrowser) et [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="6a2c9-110">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

<span data-ttu-id="6a2c9-111">Remarque : Les packages présentés font abstraction d’une grande partie de la complexité du flux d’authentification OAuth, mais en comprendre les détails peut devenir nécessaire pour la résolution des problèmes.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-111">Note: Packages presented here abstract a great deal of complexity of the OAuth authentication flow, but understanding the details may become necessary when troubleshooting.</span></span> <span data-ttu-id="6a2c9-112">De nombreuses ressources sont disponibles. Vous pouvez par exemple consulter [Introduction à OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) ou [Présentation d’OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span><span class="sxs-lookup"><span data-stu-id="6a2c9-112">Many resources are available; for example, see [Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) or [Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/).</span></span> <span data-ttu-id="6a2c9-113">Certains problèmes peuvent être résolus en examinant le [code source d’ASP.NET Core pour les packages de fournisseur](https://github.com/aspnet/Security/tree/master/src).</span><span class="sxs-lookup"><span data-stu-id="6a2c9-113">Some issues can be resolved by looking at the [ASP.NET Core source code for the provider packages](https://github.com/aspnet/Security/tree/master/src).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="6a2c9-114">Créer un projet ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6a2c9-114">Create a New ASP.NET Core Project</span></span>

* <span data-ttu-id="6a2c9-115">Dans Visual Studio 2017, créez un projet à partir de la page de démarrage, ou via **Fichier > Nouveau > Projet**.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-115">In Visual Studio 2017, create a new project from the Start Page, or via **File > New > Project**.</span></span>

* <span data-ttu-id="6a2c9-116">Sélectionnez le modèle **Application web ASP.NET Core** disponible dans la catégorie **Visual C# > .NET Core** :</span><span class="sxs-lookup"><span data-stu-id="6a2c9-116">Select the **ASP.NET Core Web Application** template available in **Visual C# > .NET Core** category:</span></span>

![Boîte de dialogue Nouveau projet](index/_static/new-project.png)

* <span data-ttu-id="6a2c9-118">Appuyez sur **Application Web** et vérifiez que **Authentification** est défini sur **Comptes d’utilisateur individuels** :</span><span class="sxs-lookup"><span data-stu-id="6a2c9-118">Tap **Web Application** and verify **Authentication** is set to **Individual User Accounts**:</span></span>

![Boîte de dialogue Nouvelle application web](index/_static/select-project.png)

<span data-ttu-id="6a2c9-120">Remarque : Ce didacticiel s’applique à la version ASP.NET Core 2.0 SDK, qui peut être sélectionnée en haut de l’Assistant.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-120">Note: This tutorial applies to ASP.NET Core 2.0 SDK version which can be selected at the top of the wizard.</span></span>

## <a name="apply-migrations"></a><span data-ttu-id="6a2c9-121">Appliquer des migrations</span><span class="sxs-lookup"><span data-stu-id="6a2c9-121">Apply migrations</span></span>

* <span data-ttu-id="6a2c9-122">Exécutez l’application et sélectionnez le lien **Se connecter**.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-122">Run the app and select the **Log in** link.</span></span>
* <span data-ttu-id="6a2c9-123">Sélectionnez le lien **S’inscrire comme nouvel utilisateur**.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-123">Select the **Register as a new user** link.</span></span>
* <span data-ttu-id="6a2c9-124">Entrez l’adresse e-mail et le mot de passe du nouveau compte, puis sélectionnez **S’inscrire**.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-124">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="6a2c9-125">Suivez les instructions pour appliquer des migrations.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-125">Follow the instructions to apply migrations.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="6a2c9-126">Exiger SSL</span><span class="sxs-lookup"><span data-stu-id="6a2c9-126">Require SSL</span></span>

<span data-ttu-id="6a2c9-127">OAuth 2.0 nécessite l’utilisation de SSL pour l’authentification avec le protocole HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-127">OAuth 2.0 requires the use of SSL for authentication over the HTTPS protocol.</span></span>

<span data-ttu-id="6a2c9-128">Remarque : Les projets créés à partir des modèles de projet **Application web** ou **API web** pour ASP.NET Core 2.x sont configurés automatiquement pour activer SSL, et pour se lancer avec des URL HTTPS si l’option **Comptes d’utilisateur individuels** a été sélectionnée dans la boîte de dialogue **Modifier l’authentification** dans l’Assistant Projet, comme indiqué ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-128">Note: Projects created using **Web Application** or **Web API** project templates for ASP.NET Core 2.x are automatically configured to enable SSL and launch with https URL if the **Individual User Accounts** option was selected on **Change Authentication dialog** in the project wizard as shown above.</span></span>

* <span data-ttu-id="6a2c9-129">Imposez l’utilisation de SSL sur votre site en suivant les étapes de la rubrique [Imposer SSL dans une application ASP.NET Core](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="6a2c9-129">Require SSL on your site by following the steps in [Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl) topic.</span></span>

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="6a2c9-130">Utilisez SecretManager pour stocker les jetons affectés par les fournisseurs de connexion</span><span class="sxs-lookup"><span data-stu-id="6a2c9-130">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="6a2c9-131">Les fournisseurs de connexion de réseaux sociaux affectent des jetons **ID d’application** et **Secret de l’application** lors du processus d’inscription.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-131">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="6a2c9-132">Les noms de jeton exacts varient selon le fournisseur.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-132">The exact token names vary by provider.</span></span> <span data-ttu-id="6a2c9-133">Ces jetons représentent les informations d’identification que votre application utilise pour accéder à son API.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-133">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="6a2c9-134">Les jetons constituent les « secrets » qui peuvent être liés à la configuration de votre application à l’aide de [Secret Manager](xref:security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="6a2c9-134">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="6a2c9-135">Secret Manager est une alternative plus sécurisée pour stocker les jetons dans un fichier de configuration, tel que *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-135">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6a2c9-136">Secret Manager est uniquement réservé au développement.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-136">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="6a2c9-137">Vous pouvez stocker et protéger les secrets de test et de production Azure avec le [fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="6a2c9-137">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="6a2c9-138">Suivez les étapes de la rubrique [Stockage sécurisé des secrets d’application lors du développement dans ASP.NET Core](xref:security/app-secrets) pour stocker les jetons affectés par chaque fournisseur de connexion ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-138">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="6a2c9-139">Configurer les fournisseurs de connexion nécessaires à votre application</span><span class="sxs-lookup"><span data-stu-id="6a2c9-139">Setup login providers required by your application</span></span>

<span data-ttu-id="6a2c9-140">Utilisez les rubriques suivantes pour configurer votre application pour utiliser ces différents fournisseurs :</span><span class="sxs-lookup"><span data-stu-id="6a2c9-140">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="6a2c9-141">Instructions pour [Facebook](xref:security/authentication/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="6a2c9-141">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="6a2c9-142">Instructions pour [Twitter](xref:security/authentication/twitter-logins)</span><span class="sxs-lookup"><span data-stu-id="6a2c9-142">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="6a2c9-143">Instructions pour [Google](xref:security/authentication/google-logins)</span><span class="sxs-lookup"><span data-stu-id="6a2c9-143">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="6a2c9-144">Instructions pour [Microsoft](xref:security/authentication/microsoft-logins)</span><span class="sxs-lookup"><span data-stu-id="6a2c9-144">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="6a2c9-145">Instructions pour les [autres fournisseurs](xref:security/authentication/otherlogins)</span><span class="sxs-lookup"><span data-stu-id="6a2c9-145">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](~/includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="6a2c9-146">Définition facultative d’un mot de passe</span><span class="sxs-lookup"><span data-stu-id="6a2c9-146">Optionally set password</span></span>

<span data-ttu-id="6a2c9-147">Quand vous vous inscrivez auprès d’un fournisseur de connexion externe, vous n’avez pas de mot de passe inscrit auprès de l’application.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-147">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="6a2c9-148">Ceci vous évite de devoir créer et mémoriser un mot de passe pour le site, mais vous rend aussi dépendant du fournisseur de connexion externe.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-148">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="6a2c9-149">Si le fournisseur de connexion externe est indisponible, vous ne pouvez pas vous connecter au site web.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-149">If the external login provider is unavailable, you won't be able to log in to the web site.</span></span>

<span data-ttu-id="6a2c9-150">Pour créer un mot de passe et vous connecter à l’aide de l’e-mail que vous avez défini lors du processus de connexion avec des fournisseurs externes :</span><span class="sxs-lookup"><span data-stu-id="6a2c9-150">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="6a2c9-151">Appuyez sur le lien **Bonjour &lt;alias d’e-mail&gt;** en haut à droite pour accéder à la vue **Gérer**.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-151">Tap the **Hello &lt;email alias&gt;** link at the top right corner to navigate to the **Manage** view.</span></span>

![Vue Gérer de l’application web](index/_static/pass1a.png)

* <span data-ttu-id="6a2c9-153">Appuyez sur **Créer**.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-153">Tap **Create**</span></span>

![Page Définir votre mot de passe](index/_static/pass2a.png)

* <span data-ttu-id="6a2c9-155">Définissez un mot de passe valide à utiliser pour vous connecter avec votre e-mail.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-155">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6a2c9-156">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="6a2c9-156">Next steps</span></span>

* <span data-ttu-id="6a2c9-157">Cet article a présenté l’authentification externe et expliqué les prérequis nécessaires pour ajouter des connexions externes à votre application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-157">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="6a2c9-158">Référencez les pages spécifiques au fournisseur pour configurer les connexions pour les fournisseurs nécessaires à votre application.</span><span class="sxs-lookup"><span data-stu-id="6a2c9-158">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>
