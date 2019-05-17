---
title: Authentification à l’aide de fournisseurs externes (Facebook, Google et autres) dans ASP.NET Core
author: rick-anderson
description: Ce didacticiel montre comment générer une application ASP.NET Core 2.x à l’aide d’OAuth 2.0 avec des fournisseurs d’authentification externes.
ms.author: riande
ms.custom: mvc
ms.date: 4/19/2019
uid: security/authentication/social/index
ms.openlocfilehash: e2d68ac93bdcfa2fc015e8447ea38626787cdb02
ms.sourcegitcommit: a3926eae3f687013027a2828830c12a89add701f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65451045"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a><span data-ttu-id="dba93-103">Authentification à l’aide de fournisseurs externes (Facebook, Google et autres) dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dba93-103">Facebook, Google, and external provider authentication in ASP.NET Core</span></span>

<span data-ttu-id="dba93-104">Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="dba93-104">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="dba93-105">Ce tutoriel montre comment créer une application ASP.NET Core 2.2 qui permet aux utilisateurs de se connecter à l’aide d’OAuth 2.0 avec des informations d’identification provenant de fournisseurs d’authentification externes.</span><span class="sxs-lookup"><span data-stu-id="dba93-105">This tutorial demonstrates how to build an ASP.NET Core 2.2 app that enables users to sign in using OAuth 2.0 with credentials from external authentication providers.</span></span>

<span data-ttu-id="dba93-106">Les fournisseurs [Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins) et [Microsoft](xref:security/authentication/microsoft-logins) sont traités dans les sections qui suivent.</span><span class="sxs-lookup"><span data-stu-id="dba93-106">[Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins), and [Microsoft](xref:security/authentication/microsoft-logins) providers are covered in the following sections.</span></span> <span data-ttu-id="dba93-107">D’autres fournisseurs sont disponibles dans des packages tiers, comme [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) et [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span><span class="sxs-lookup"><span data-stu-id="dba93-107">Other providers are available in third-party packages such as [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) and [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).</span></span>

![Icônes de réseau social pour Facebook, Twitter, Google plus et Windows](index/_static/social.png)

<span data-ttu-id="dba93-109">Permettre aux utilisateurs de se connecter avec leurs informations d’identification existantes :</span><span class="sxs-lookup"><span data-stu-id="dba93-109">Enabling users to sign in with their existing credentials:</span></span>
* <span data-ttu-id="dba93-110">Est pratique pour les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="dba93-110">Is convenient for the users.</span></span>
* <span data-ttu-id="dba93-111">Transfère une grande partie des complexités de la gestion du processus de connexion à un tiers.</span><span class="sxs-lookup"><span data-stu-id="dba93-111">Shifts many of the complexities of managing the sign-in process onto a third party.</span></span> 

<span data-ttu-id="dba93-112">Pour obtenir des exemples de la façon dont les connexions des réseaux sociaux peuvent amener du trafic et des conversions de clients, consultez les études de cas par [Facebook](https://www.facebook.com/unsupportedbrowser) et [Twitter](https://dev.twitter.com/resources/case-studies).</span><span class="sxs-lookup"><span data-stu-id="dba93-112">For examples of how social logins can drive traffic and customer conversions, see case studies by [Facebook](https://www.facebook.com/unsupportedbrowser) and [Twitter](https://dev.twitter.com/resources/case-studies).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="dba93-113">Créer un projet ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dba93-113">Create a New ASP.NET Core Project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dba93-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dba93-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="dba93-115">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="dba93-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="dba93-116">Créez une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dba93-116">Create a new ASP.NET Core Web Application.</span></span>
* <span data-ttu-id="dba93-117">Sélectionnez **ASP.NET Core 2.2** dans la liste déroulante, puis sélectionnez **Application web**.</span><span class="sxs-lookup"><span data-stu-id="dba93-117">Select **ASP.NET Core 2.2** in the dropdown, and then select **Web Application**.</span></span>
* <span data-ttu-id="dba93-118">Sélectionnez **Modifier l’authentification** et définissez l’authentification sur **Comptes d’utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="dba93-118">Select **Change Authentication** and set authentication to **Individual User Accounts**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dba93-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dba93-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="dba93-120">Ouvrez le [terminal intégré](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="dba93-120">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="dba93-121">Accédez à un répertoire (`cd`) destiné à contenir le projet.</span><span class="sxs-lookup"><span data-stu-id="dba93-121">Change directories (`cd`) to a folder which will contain the project.</span></span>

* <span data-ttu-id="dba93-122">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="dba93-122">Run the following commands:</span></span>

  ```console
  dotnet new webapp -o WebApp1 -au Individual -uld
  code -r WebApp1
  ```

  * <span data-ttu-id="dba93-123">La commande `dotnet new` crée un nouveau projet Razor Pages dans le dossier *WebApp1*.</span><span class="sxs-lookup"><span data-stu-id="dba93-123">The `dotnet new` command creates a new Razor Pages project in the *WebApp1* folder.</span></span>
  * <span data-ttu-id="dba93-124">`-uld` utilise la Base de données locale au lieu de SQLite.</span><span class="sxs-lookup"><span data-stu-id="dba93-124">`-uld` uses LocalDB instead of SQLite.</span></span> <span data-ttu-id="dba93-125">Omettez `-uld` pour utiliser SQLite.</span><span class="sxs-lookup"><span data-stu-id="dba93-125">Omit `-uld` to use SQLite.</span></span>
  * <span data-ttu-id="dba93-126">`-au Individual` crée le code servant à l’authentification individuelle.</span><span class="sxs-lookup"><span data-stu-id="dba93-126">`-au Individual` creates the code for Individual authentication.</span></span>
  * <span data-ttu-id="dba93-127">La commande `code` ouvre le dossier *WebApp1* dans une nouvelle instance de Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="dba93-127">The `code` command opens the *WebApp1* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="dba93-128">Une boîte de dialogue apparaît et affiche **Les composants nécessaires à la build et au débogage sont manquants dans « WebApp1 ». Faut-il les ajouter ?**</span><span class="sxs-lookup"><span data-stu-id="dba93-128">A dialog box appears with **Required assets to build and debug are missing from 'WebApp1'. Add them?**</span></span>

* <span data-ttu-id="dba93-129">Sélectionnez **Oui**.</span><span class="sxs-lookup"><span data-stu-id="dba93-129">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dba93-130">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="dba93-130">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="dba93-131">À partir d’un terminal, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="dba93-131">From a terminal, run the following command:</span></span>

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o WebApp1 -au Individual
```

<span data-ttu-id="dba93-132">Les commandes précédentes utilisent le [CLI .NET Core](/dotnet/core/tools/dotnet) pour créer un projet Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="dba93-132">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a Razor Pages project.</span></span>

## <a name="open-the-project"></a><span data-ttu-id="dba93-133">Ouvrir le projet</span><span class="sxs-lookup"><span data-stu-id="dba93-133">Open the project</span></span>

<span data-ttu-id="dba93-134">Dans Visual Studio, sélectionnez **Fichier > Ouvrir**, puis sélectionnez le fichier *WebApp1.csproj*.</span><span class="sxs-lookup"><span data-stu-id="dba93-134">From Visual Studio, select **File > Open**, and then select the *WebApp1.csproj* file.</span></span>

<!-- End of VS tabs -->

---

## <a name="apply-migrations"></a><span data-ttu-id="dba93-135">Appliquer des migrations</span><span class="sxs-lookup"><span data-stu-id="dba93-135">Apply migrations</span></span>

* <span data-ttu-id="dba93-136">Exécutez l’application et sélectionnez le lien **S’inscrire**.</span><span class="sxs-lookup"><span data-stu-id="dba93-136">Run the app and select the **Register** link.</span></span>
* <span data-ttu-id="dba93-137">Entrez l’adresse e-mail et le mot de passe du nouveau compte, puis sélectionnez **S’inscrire**.</span><span class="sxs-lookup"><span data-stu-id="dba93-137">Enter the email and password for the new account, and then select **Register**.</span></span>
* <span data-ttu-id="dba93-138">Suivez les instructions pour appliquer des migrations.</span><span class="sxs-lookup"><span data-stu-id="dba93-138">Follow the instructions to apply migrations.</span></span>

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a><span data-ttu-id="dba93-139">Utilisez SecretManager pour stocker les jetons affectés par les fournisseurs de connexion</span><span class="sxs-lookup"><span data-stu-id="dba93-139">Use SecretManager to store tokens assigned by login providers</span></span>

<span data-ttu-id="dba93-140">Les fournisseurs de connexion de réseaux sociaux affectent des jetons **ID d’application** et **Secret de l’application** lors du processus d’inscription.</span><span class="sxs-lookup"><span data-stu-id="dba93-140">Social login providers assign **Application Id** and **Application Secret** tokens during the registration process.</span></span> <span data-ttu-id="dba93-141">Les noms de jeton exacts varient selon le fournisseur.</span><span class="sxs-lookup"><span data-stu-id="dba93-141">The exact token names vary by provider.</span></span> <span data-ttu-id="dba93-142">Ces jetons représentent les informations d’identification que votre application utilise pour accéder à son API.</span><span class="sxs-lookup"><span data-stu-id="dba93-142">These tokens represent the credentials your app uses to access their API.</span></span> <span data-ttu-id="dba93-143">Les jetons constituent les « secrets » qui peuvent être liés à la configuration de votre application à l’aide de [Secret Manager](xref:security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="dba93-143">The tokens constitute the "secrets" that can be linked to your app configuration with the help of [Secret Manager](xref:security/app-secrets#secret-manager).</span></span> <span data-ttu-id="dba93-144">Secret Manager est une alternative plus sécurisée pour stocker les jetons dans un fichier de configuration, tel que *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="dba93-144">Secret Manager is a more secure alternative to storing the tokens in a configuration file, such as *appsettings.json*.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dba93-145">Secret Manager est uniquement réservé au développement.</span><span class="sxs-lookup"><span data-stu-id="dba93-145">Secret Manager is for development purposes only.</span></span> <span data-ttu-id="dba93-146">Vous pouvez stocker et protéger les secrets de test et de production Azure avec le [fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="dba93-146">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

<span data-ttu-id="dba93-147">Suivez les étapes de la rubrique [Stockage sécurisé des secrets d’application lors du développement dans ASP.NET Core](xref:security/app-secrets) pour stocker les jetons affectés par chaque fournisseur de connexion ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="dba93-147">Follow the steps in [Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets) topic to store tokens assigned by each login provider below.</span></span>

## <a name="setup-login-providers-required-by-your-application"></a><span data-ttu-id="dba93-148">Configurer les fournisseurs de connexion nécessaires à votre application</span><span class="sxs-lookup"><span data-stu-id="dba93-148">Setup login providers required by your application</span></span>

<span data-ttu-id="dba93-149">Utilisez les rubriques suivantes pour configurer votre application pour utiliser ces différents fournisseurs :</span><span class="sxs-lookup"><span data-stu-id="dba93-149">Use the following topics to configure your application to use the respective providers:</span></span>

* <span data-ttu-id="dba93-150">Instructions pour [Facebook](xref:security/authentication/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="dba93-150">[Facebook](xref:security/authentication/facebook-logins) instructions</span></span>
* <span data-ttu-id="dba93-151">Instructions pour [Twitter](xref:security/authentication/twitter-logins)</span><span class="sxs-lookup"><span data-stu-id="dba93-151">[Twitter](xref:security/authentication/twitter-logins) instructions</span></span>
* <span data-ttu-id="dba93-152">Instructions pour [Google](xref:security/authentication/google-logins)</span><span class="sxs-lookup"><span data-stu-id="dba93-152">[Google](xref:security/authentication/google-logins) instructions</span></span>
* <span data-ttu-id="dba93-153">Instructions pour [Microsoft](xref:security/authentication/microsoft-logins)</span><span class="sxs-lookup"><span data-stu-id="dba93-153">[Microsoft](xref:security/authentication/microsoft-logins) instructions</span></span>
* <span data-ttu-id="dba93-154">Instructions pour les [autres fournisseurs](xref:security/authentication/otherlogins)</span><span class="sxs-lookup"><span data-stu-id="dba93-154">[Other provider](xref:security/authentication/otherlogins) instructions</span></span>

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a><span data-ttu-id="dba93-155">Définition facultative d’un mot de passe</span><span class="sxs-lookup"><span data-stu-id="dba93-155">Optionally set password</span></span>

<span data-ttu-id="dba93-156">Quand vous vous inscrivez auprès d’un fournisseur de connexion externe, vous n’avez pas de mot de passe inscrit auprès de l’application.</span><span class="sxs-lookup"><span data-stu-id="dba93-156">When you register with an external login provider, you don't have a password registered with the app.</span></span> <span data-ttu-id="dba93-157">Ceci vous évite de devoir créer et mémoriser un mot de passe pour le site, mais vous rend aussi dépendant du fournisseur de connexion externe.</span><span class="sxs-lookup"><span data-stu-id="dba93-157">This alleviates you from creating and remembering a password for the site, but it also makes you dependent on the external login provider.</span></span> <span data-ttu-id="dba93-158">Si le fournisseur de connexion externe n’est pas disponible, vous ne pouvez pas vous connecter au site web.</span><span class="sxs-lookup"><span data-stu-id="dba93-158">If the external login provider is unavailable, you won't be able to sign in to the web site.</span></span>

<span data-ttu-id="dba93-159">Pour créer un mot de passe et vous connecter à l’aide de l’e-mail que vous avez défini lors du processus de connexion avec des fournisseurs externes :</span><span class="sxs-lookup"><span data-stu-id="dba93-159">To create a password and sign in using your email that you set during the sign in process with external providers:</span></span>

* <span data-ttu-id="dba93-160">Sélectionnez le lien **Bonjour &lt;alias d’e-mail&gt;** en haut à droite pour accéder à la vue **Gérer**.</span><span class="sxs-lookup"><span data-stu-id="dba93-160">Select the **Hello &lt;email alias&gt;** link at the top-right corner to navigate to the **Manage** view.</span></span>

![Vue Gérer de l’application web](index/_static/pass1a.png)

* <span data-ttu-id="dba93-162">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="dba93-162">Select **Create**</span></span>

![Page Définir votre mot de passe](index/_static/pass2a.png)

* <span data-ttu-id="dba93-164">Définissez un mot de passe valide à utiliser pour vous connecter avec votre e-mail.</span><span class="sxs-lookup"><span data-stu-id="dba93-164">Set a valid password and you can use this to sign in with your email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dba93-165">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="dba93-165">Next steps</span></span>

* <span data-ttu-id="dba93-166">Cet article a présenté l’authentification externe et expliqué les prérequis nécessaires pour ajouter des connexions externes à votre application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dba93-166">This article introduced external authentication and explained the prerequisites required to add external logins to your ASP.NET Core app.</span></span>

* <span data-ttu-id="dba93-167">Référencez les pages spécifiques au fournisseur pour configurer les connexions pour les fournisseurs nécessaires à votre application.</span><span class="sxs-lookup"><span data-stu-id="dba93-167">Reference provider-specific pages to configure logins for the providers required by your app.</span></span>

* <span data-ttu-id="dba93-168">Vous souhaiterez peut-être conserver des données supplémentaires relatives à l’utilisateur et à ses jetons d’accès et d’actualisation.</span><span class="sxs-lookup"><span data-stu-id="dba93-168">You may want to persist additional data about the user and their access and refresh tokens.</span></span> <span data-ttu-id="dba93-169">Pour plus d'informations, consultez <xref:security/authentication/social/additional-claims>.</span><span class="sxs-lookup"><span data-stu-id="dba93-169">For more information, see <xref:security/authentication/social/additional-claims>.</span></span>
