---
title: Confirmation du compte et récupération de mot de passe dans ASP.NET Core
author: rick-anderson
description: Découvrez comment créer une application ASP.NET Core à la réinitialisation par courrier électronique de confirmation et le mot de passe.
manager: wpickett
ms.author: riande
ms.date: 2/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: b6dbe234973431448c18d3cc82a6ac98d4f53a3b
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34730449"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="6a7e7-103">Confirmation du compte et récupération de mot de passe dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6a7e7-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="6a7e7-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="6a7e7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="6a7e7-105">Ce didacticiel vous montre comment créer une application ASP.NET Core à la réinitialisation par courrier électronique de confirmation et le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="6a7e7-106">Ce didacticiel est **pas** une rubrique de début.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="6a7e7-107">Vous devez être familiarisé avec :</span><span class="sxs-lookup"><span data-stu-id="6a7e7-107">You should be familiar with:</span></span>

* [<span data-ttu-id="6a7e7-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6a7e7-108">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="6a7e7-109">Authentification</span><span class="sxs-lookup"><span data-stu-id="6a7e7-109">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="6a7e7-110">Confirmation de compte et récupération de mot de passe</span><span class="sxs-lookup"><span data-stu-id="6a7e7-110">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="6a7e7-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="6a7e7-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="6a7e7-112">Consultez [ce fichier PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) de la version 1.1 de MVC ASP.NET Core et 2.x.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-112">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC 1.1 and 2.x versions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6a7e7-113">Prérequis</span><span class="sxs-lookup"><span data-stu-id="6a7e7-113">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a><span data-ttu-id="6a7e7-114">Créer un nouveau projet ASP.NET Core avec l’interface de ligne de base .NET</span><span class="sxs-lookup"><span data-stu-id="6a7e7-114">Create a new ASP.NET Core project with the .NET Core CLI</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6a7e7-115">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6a7e7-115">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

* <span data-ttu-id="6a7e7-116">`--auth Individual` Spécifie le modèle de projet de comptes d’utilisateur individuels.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-116">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="6a7e7-117">Sur Windows, ajoutez le `-uld` option.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-117">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="6a7e7-118">Il spécifie la que base de données locale doit être utilisé au lieu de SQLite.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-118">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="6a7e7-119">Exécutez `new mvc --help` pour obtenir de l’aide sur cette commande.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-119">Run `new mvc --help` to get help on this command.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6a7e7-120">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6a7e7-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6a7e7-121">Si vous utilisez l’interface CLI ou SQLite, exécutez la commande suivante dans une fenêtre de commande :</span><span class="sxs-lookup"><span data-stu-id="6a7e7-121">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="6a7e7-122">`--auth Individual` Spécifie le modèle de projet de comptes d’utilisateur individuels.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-122">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="6a7e7-123">Sur Windows, ajoutez le `-uld` option.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-123">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="6a7e7-124">Il spécifie la que base de données locale doit être utilisé au lieu de SQLite.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-124">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="6a7e7-125">Exécutez `new mvc --help` pour obtenir de l’aide sur cette commande.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-125">Run `new mvc --help` to get help on this command.</span></span>

---

<span data-ttu-id="6a7e7-126">Vous pouvez également créer un nouveau projet ASP.NET Core avec Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="6a7e7-126">Alternatively, you can create a new ASP.NET Core project with Visual Studio:</span></span>

* <span data-ttu-id="6a7e7-127">Dans Visual Studio, créez un **Application Web** projet.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-127">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="6a7e7-128">Sélectionnez **ASP.NET Core 2.0**.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-128">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="6a7e7-129">**.NET core** est sélectionné dans l’image suivante, mais vous pouvez sélectionner **.NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-129">**.NET Core** is selected in the following image, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="6a7e7-130">Sélectionnez **modifier l’authentification** et la valeur **comptes d’utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-130">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="6a7e7-131">Conservez la valeur par défaut **dans l’application des comptes d’utilisateurs de magasin**.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-131">Keep the default **Store user accounts in-app**.</span></span>

![Nouvelle boîte de dialogue projet indiquant « Radio de comptes d’utilisateur individuels » sélectionnée](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a><span data-ttu-id="6a7e7-133">Nouvelle inscription de l’utilisateur de test</span><span class="sxs-lookup"><span data-stu-id="6a7e7-133">Test new user registration</span></span>

<span data-ttu-id="6a7e7-134">Exécuter l’application, sélectionnez le **inscrire** lier et inscrire un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-134">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="6a7e7-135">Suivez les instructions pour exécuter les migrations d’Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-135">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="6a7e7-136">À ce stade, la seule validation sur l’adresse e-mail se fait avec l'attribut [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute).</span><span class="sxs-lookup"><span data-stu-id="6a7e7-136">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="6a7e7-137">Après avoir soumis l’inscription, vous êtes connecté à l’application.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-137">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="6a7e7-138">Plus loin dans ce didacticiel, le code est mis à jour pour les nouveaux utilisateurs ne peuvent pas se connecter jusqu'à ce que leur courrier électronique a été validée.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-138">Later in the tutorial, the code is updated so new users can't log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="6a7e7-139">Afficher la base de données d’identité</span><span class="sxs-lookup"><span data-stu-id="6a7e7-139">View the Identity database</span></span>

<span data-ttu-id="6a7e7-140">Consultez [de travail dans un projet ASP.NET MVC de base avec SQLite](xref:tutorials/first-mvc-app-xplat/working-with-sql) pour obtenir des instructions sur l’affichage de la base de données SQLite.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-140">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

<span data-ttu-id="6a7e7-141">Pour Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="6a7e7-141">For Visual Studio:</span></span>

* <span data-ttu-id="6a7e7-142">À partir de la **vue** menu, sélectionnez **l’Explorateur d’objets SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="6a7e7-142">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="6a7e7-143">Accédez à **(localdb) MSSQLLocalDB (SQL Server 13)**.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-143">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="6a7e7-144">Avec le bouton droit sur **dbo. AspNetUsers** > **afficher les données**:</span><span class="sxs-lookup"><span data-stu-id="6a7e7-144">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Menu contextuel sur la table AspNetUsers dans l’Explorateur d’objets SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="6a7e7-146">Notez la table `EmailConfirmed` champ est `False`.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-146">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="6a7e7-147">Vous pouvez souhaiter réutiliser ce courrier électronique à l’étape suivante lors de l’application envoie un message électronique de confirmation.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-147">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="6a7e7-148">Avec le bouton droit sur la ligne, puis sélectionnez **supprimer**.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-148">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="6a7e7-149">Suppression de l’alias de messagerie facilite dans les étapes suivantes.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-149">Deleting the email alias makes it easier in the following steps.</span></span>

---

## <a name="require-https"></a><span data-ttu-id="6a7e7-150">Exiger HTTPS</span><span class="sxs-lookup"><span data-stu-id="6a7e7-150">Require HTTPS</span></span>

<span data-ttu-id="6a7e7-151">Consultez [exiger HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="6a7e7-151">See [Require HTTPS](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="6a7e7-152">Demander confirmation de courrier électronique</span><span class="sxs-lookup"><span data-stu-id="6a7e7-152">Require email confirmation</span></span>

<span data-ttu-id="6a7e7-153">Il est recommandé de confirmer l’adresse de messagerie d’un nouvel enregistrement d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-153">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="6a7e7-154">Envoyer par courrier électronique de confirmation permet de vérifier qu’ils empruntez pas une autre personne (autrement dit, ils n’ont pas inscrit avec par quelqu'un d’autre adresse de messagerie).</span><span class="sxs-lookup"><span data-stu-id="6a7e7-154">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="6a7e7-155">Supposons que vous aviez un forum de discussion, et vous souhaitez empêcher «yli@example.com« lors de l’inscription en tant que »nolivetto@contoso.com».</span><span class="sxs-lookup"><span data-stu-id="6a7e7-155">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="6a7e7-156">Sans la confirmation par courrier électronique, «nolivetto@contoso.com» peut recevoir un e-mail indésirable de votre application.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-156">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="6a7e7-157">Supposons que l’utilisateur inscrit par inadvertance en tant que «ylo@example.com» et vous n’aviez pas remarqué la faute d’orthographe de « yli ».</span><span class="sxs-lookup"><span data-stu-id="6a7e7-157">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="6a7e7-158">Ils ne pourrez pas utiliser la récupération de mot de passe, car l’application n’a pas leur courrier électronique correct.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-158">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="6a7e7-159">E-mail de confirmation fournit uniquement une protection limitée de robots.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-159">Email confirmation provides only limited protection from bots.</span></span> <span data-ttu-id="6a7e7-160">E-mail de confirmation ne fournit pas une protection contre les utilisateurs malveillants avec plusieurs comptes de messagerie.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-160">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="6a7e7-161">En règle générale, vous souhaitez empêcher les nouveaux utilisateurs à partir de la validation des données à votre site web avant d’avoir un message électronique de confirmation.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-161">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="6a7e7-162">Mise à jour `ConfigureServices` pour exiger un message électronique de confirmation :</span><span class="sxs-lookup"><span data-stu-id="6a7e7-162">Update `ConfigureServices` to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

<span data-ttu-id="6a7e7-163">`config.SignIn.RequireConfirmedEmail = true;` empêche les utilisateurs enregistrés de se connecter jusqu'à ce que leur adresse de messagerie est confirmée.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-163">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="6a7e7-164">Configurer le fournisseur de messagerie</span><span class="sxs-lookup"><span data-stu-id="6a7e7-164">Configure email provider</span></span>

<span data-ttu-id="6a7e7-165">Dans ce didacticiel, SendGrid est utilisé pour envoyer un courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-165">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="6a7e7-166">Vous avez besoin d’un compte SendGrid et une clé pour envoyer un courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-166">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="6a7e7-167">Vous pouvez utiliser d’autres fournisseurs de messagerie.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-167">You can use other email providers.</span></span> <span data-ttu-id="6a7e7-168">ASP.NET Core 2.x inclut `System.Net.Mail`, qui vous permet d’envoyer par courrier électronique à partir de votre application.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-168">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="6a7e7-169">Nous vous recommandons de qu'utiliser SendGrid ou un autre service de messagerie pour envoyer un courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-169">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="6a7e7-170">SMTP est difficile de sécuriser et correctement configuré.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-170">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="6a7e7-171">Le [modèle d’Options](xref:fundamentals/configuration/options) est utilisé pour accéder aux paramètres de compte et une clé utilisateur.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-171">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="6a7e7-172">Pour plus d’informations, consultez [configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="6a7e7-172">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="6a7e7-173">Créez une classe pour extraire la clé de sécuriser la messagerie électronique.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-173">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="6a7e7-174">Pour cet exemple, le `AuthMessageSenderOptions` classe est créée dans le *Services/AuthMessageSenderOptions.cs* fichier :</span><span class="sxs-lookup"><span data-stu-id="6a7e7-174">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="6a7e7-175">Définir le `SendGridUser` et `SendGridKey` avec la [outil Gestionnaire de secret](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="6a7e7-175">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="6a7e7-176">Exemple :</span><span class="sxs-lookup"><span data-stu-id="6a7e7-176">For example:</span></span>

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="6a7e7-177">Sur Windows, le gestionnaire de secret stocke des paires de clés/valeur dans un fichier *secrets.json* dans le répertoire `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-177">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="6a7e7-178">Le contenu du fichier *secrets.json* n’est pas chiffré.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-178">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="6a7e7-179">Le fichier *secrets.json* est présenté ci-dessous (la valeur `SendGridKey` a été supprimée.)</span><span class="sxs-lookup"><span data-stu-id="6a7e7-179">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="6a7e7-180">Configurer le démarrage pour utiliser AuthMessageSenderOptions</span><span class="sxs-lookup"><span data-stu-id="6a7e7-180">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="6a7e7-181">Ajouter `AuthMessageSenderOptions` au conteneur de service à la fin de la méthode `ConfigureServices` dans le fichier *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="6a7e7-181">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6a7e7-182">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6a7e7-182">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6a7e7-183">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6a7e7-183">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="6a7e7-184">Configurer la classe AuthMessageSender</span><span class="sxs-lookup"><span data-stu-id="6a7e7-184">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="6a7e7-185">Ce didacticiel montre comment ajouter des notifications par courrier électronique via [SendGrid](https://sendgrid.com/), mais vous pouvez envoyer des e-mails en utilisant SMTP et d'autres mécanismes.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-185">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="6a7e7-186">Installer le package NuGet `SendGrid` :</span><span class="sxs-lookup"><span data-stu-id="6a7e7-186">Install the `SendGrid` NuGet package:</span></span>

* <span data-ttu-id="6a7e7-187">À partir de la ligne de commande :</span><span class="sxs-lookup"><span data-stu-id="6a7e7-187">From the command line:</span></span>

    `dotnet add package SendGrid`

* <span data-ttu-id="6a7e7-188">À partir de la Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="6a7e7-188">From the Package Manager Console, enter the following command:</span></span>

  `Install-Package SendGrid`

<span data-ttu-id="6a7e7-189">Consultez [Débuter avec SendGrid gratuitement](https://sendgrid.com/free/) pour vous inscrire pour un compte SendGrid gratuit.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-189">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="6a7e7-190">Configurer SendGrid</span><span class="sxs-lookup"><span data-stu-id="6a7e7-190">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6a7e7-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6a7e7-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="6a7e7-192">Pour configurer SendGrid, ajoutez du code semblable au suivant dans *Services/EmailSender.cs*:</span><span class="sxs-lookup"><span data-stu-id="6a7e7-192">To configure SendGrid, add code similar to the following in *Services/EmailSender.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6a7e7-193">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6a7e7-193">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="6a7e7-194">Ajoutez le code dans *Services/MessageServices.cs* similaire à ce qui suit pour configurer SendGrid :</span><span class="sxs-lookup"><span data-stu-id="6a7e7-194">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="6a7e7-195">Activer la récupération de confirmation et le mot de passe du compte</span><span class="sxs-lookup"><span data-stu-id="6a7e7-195">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="6a7e7-196">Le modèle contient du code pour la confirmation du compte et la récupération de mot de passe.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-196">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="6a7e7-197">Recherchez la méthode `OnPostAsync` dans *Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-197">Find the `OnPostAsync` method in *Pages/Account/Register.cshtml.cs*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6a7e7-198">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6a7e7-198">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="6a7e7-199">Empêchez les utilisateurs nouvellement inscrits d'être connectés automatiquement en plaçant la ligne suivante en commentaire :</span><span class="sxs-lookup"><span data-stu-id="6a7e7-199">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="6a7e7-200">La méthode complète est montrée avec la ligne modifiée mise en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="6a7e7-200">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6a7e7-201">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6a7e7-201">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="6a7e7-202">Pour activer la confirmation du compte, supprimez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="6a7e7-202">To enable account confirmation, uncomment the following code:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="6a7e7-203">**Remarque :** le code empêche un utilisateur nouvellement inscrit d'ouvrir automatiquement une session via la mise en commentaire de la ligne suivante :</span><span class="sxs-lookup"><span data-stu-id="6a7e7-203">**Note:** The code is preventing a newly registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="6a7e7-204">Activez la récupération de mot de passe en décommentant le code dans l'action `ForgotPassword` de *Controllers/AccountController.cs*:</span><span class="sxs-lookup"><span data-stu-id="6a7e7-204">Enable password recovery by uncommenting the code in the `ForgotPassword` action of *Controllers/AccountController.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="6a7e7-205">Supprimez les commentaires de l’élément form *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-205">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="6a7e7-206">Vous souhaiterez peut-être supprimer le `<p> For more information on how to enable reset password ... </p>` élément, qui contient un lien vers cet article.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-206">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element, which contains a link to this article.</span></span>

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="6a7e7-207">Inscrire, confirmer l’adresse e-mail et réinitialiser le mot de passe</span><span class="sxs-lookup"><span data-stu-id="6a7e7-207">Register, confirm email, and reset password</span></span>

<span data-ttu-id="6a7e7-208">Exécuter l’application web, et testez le flux de confirmation du compte et de récupération du mot de passe.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-208">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="6a7e7-209">Exécutez l’application et inscrire un nouvel utilisateur</span><span class="sxs-lookup"><span data-stu-id="6a7e7-209">Run the app and register a new user</span></span>

  ![Affichage du livre de comptes application Web](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="6a7e7-211">Recherchez le lien de confirmation du compte dans votre messagerie.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-211">Check your email for the account confirmation link.</span></span> <span data-ttu-id="6a7e7-212">Consultez [Déboguer la messagerie](#debug) si vous ne recevez pas l’e-mail.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-212">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="6a7e7-213">Cliquez sur le lien pour confirmer votre adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-213">Click the link to confirm your email.</span></span>
* <span data-ttu-id="6a7e7-214">Connectez-vous avec votre adresse électronique et un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-214">Log in with your email and password.</span></span>
* <span data-ttu-id="6a7e7-215">Fermez la session.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-215">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="6a7e7-216">Afficher la page de gestion</span><span class="sxs-lookup"><span data-stu-id="6a7e7-216">View the manage page</span></span>

<span data-ttu-id="6a7e7-217">Sélectionnez votre nom d’utilisateur dans le navigateur : ![fenêtre de navigateur avec le nom d’utilisateur](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="6a7e7-217">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="6a7e7-218">Vous devrez peut-être développer la barre de navigation pour afficher le nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-218">You might need to expand the navbar to see user name.</span></span>

![barre de navigation](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6a7e7-220">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6a7e7-220">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6a7e7-221">La page de gestion s’affiche avec l'onglet **profil** sélectionné.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-221">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="6a7e7-222">L'**Email** affiche une case à cocher indiquant que l’adresse de messagerie a été confirmée.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-222">The **Email** shows a check box indicating the email has been confirmed.</span></span>

![page Gérer](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6a7e7-224">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6a7e7-224">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6a7e7-225">Cela est mentionné plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-225">This is mentioned later in the tutorial.</span></span>
<span data-ttu-id="6a7e7-226">![page Gérer](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="6a7e7-226">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="6a7e7-227">Tester la réinitialisation du mot de passe</span><span class="sxs-lookup"><span data-stu-id="6a7e7-227">Test password reset</span></span>

* <span data-ttu-id="6a7e7-228">Si vous êtes connecté, sélectionnez **Déconnexion**.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-228">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="6a7e7-229">Sélectionnez le lien **Connexion** et sélectionnez le lien **Vous avez oublié votre mot de passe ?**.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-229">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="6a7e7-230">Entrez l’e-mail que vous avez utilisé pour enregistrer le compte.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-230">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="6a7e7-231">Un message électronique contenant un lien pour réinitialiser votre mot de passe est envoyé.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-231">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="6a7e7-232">Vérifiez votre adresse de messagerie et cliquez sur le lien pour réinitialiser votre mot de passe.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-232">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="6a7e7-233">Une fois votre mot de passe a été réinitialisé avec succès, vous pouvez vous connecter avec votre adresse électronique et un nouveau mot de passe.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-233">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="6a7e7-234">Déboguer le courrier électronique</span><span class="sxs-lookup"><span data-stu-id="6a7e7-234">Debug email</span></span>

<span data-ttu-id="6a7e7-235">Si vous ne parvenez à faire fonctionner l'email :</span><span class="sxs-lookup"><span data-stu-id="6a7e7-235">If you can't get email working:</span></span>

* <span data-ttu-id="6a7e7-236">Créer une [application console pour envoyer un courrier électronique](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="6a7e7-236">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="6a7e7-237">Examinez la page [Activité de la messagerie](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-237">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="6a7e7-238">Vérifiez votre dossier courrier indésirable.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-238">Check your spam folder.</span></span>
* <span data-ttu-id="6a7e7-239">Essayez un autre alias de messagerie électronique sur un fournisseur de messagerie différents (Microsoft, Yahoo, Gmail, etc.).</span><span class="sxs-lookup"><span data-stu-id="6a7e7-239">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="6a7e7-240">Essayez d’envoyer à des comptes de messagerie différents.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-240">Try sending to different email accounts.</span></span>

<span data-ttu-id="6a7e7-241">Une **bonne pratique de sécurité** consiste à **ne pas** utiliser des secrets de production en développement et en test.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-241">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="6a7e7-242">Si vous publiez l’application sur Azure, vous pouvez définir les secrets SendGrid en tant que paramètres d’application dans le portail Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-242">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="6a7e7-243">Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-243">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="6a7e7-244">Combiner des comptes de connexion de réseaux sociaux et local</span><span class="sxs-lookup"><span data-stu-id="6a7e7-244">Combine social and local login accounts</span></span>

<span data-ttu-id="6a7e7-245">Pour terminer cette section, vous devez d’abord activer un fournisseur d’authentification externe.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-245">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="6a7e7-246">Consultez [Facebook, Google et l’authentification du fournisseur externe](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="6a7e7-246">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="6a7e7-247">Vous pouvez combiner des comptes locaux et de réseaux sociaux en cliquant sur le lien de votre e-mail.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-247">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="6a7e7-248">Dans la séquence suivante, "RickAndMSFT@gmail.com" est d’abord créé en tant que connexion locale ; cependant, vous pouvez d’abord créer le compte en tant que connexion de réseau social, puis ajouter une connexion locale.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-248">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Application Web : RickAndMSFT@gmail.com utilisateur authentifié](accconfirm/_static/rick.png)

<span data-ttu-id="6a7e7-250">Cliquez sur le lien **Gérer**.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-250">Click on the **Manage** link.</span></span> <span data-ttu-id="6a7e7-251">Notez la valeur 0 externe (connexions sociales) associé à ce compte.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-251">Note the 0 external (social logins) associated with this account.</span></span>

![Gérer les affichages](accconfirm/_static/manage.png)

<span data-ttu-id="6a7e7-253">Cliquez sur le lien vers un autre service de connexion et acceptez les demandes d’application.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-253">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="6a7e7-254">Dans l’image suivante, Facebook est le fournisseur d’authentification externe :</span><span class="sxs-lookup"><span data-stu-id="6a7e7-254">In the following image, Facebook is the external authentication provider:</span></span>

![Gestion de votre vue de logins externes répertoriant Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="6a7e7-256">Les deux comptes ont été combinés.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-256">The two accounts have been combined.</span></span> <span data-ttu-id="6a7e7-257">Vous pouvez vous connecter avec l'un ou l'autre compte.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-257">You are able to log on with either account.</span></span> <span data-ttu-id="6a7e7-258">Vous pouvez faire en sorte que vos utilisateurs ajoutent des comptes locaux au cas où leur service d’authentification de connexion sociale soit indisponible ou que, plus probablement, ils aient perdu l’accès à leur compte social.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-258">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="6a7e7-259">Activer la confirmation du compte après qu’un site a des utilisateurs</span><span class="sxs-lookup"><span data-stu-id="6a7e7-259">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="6a7e7-260">Activer la confirmation du compte sur un site avec des utilisateurs bloque tous les utilisateurs existants.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-260">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="6a7e7-261">Ceux-ci sont bloqués, car leurs comptes ne sont pas confirmés.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-261">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="6a7e7-262">Pour contourner le blocage des utilisateurs existants, utilisez une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="6a7e7-262">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="6a7e7-263">Mise à jour de la base de données pour marquer tous les utilisateurs existants comme étant confirmés.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-263">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="6a7e7-264">Confirmation des utilisateurs existants.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-264">Confirm exiting users.</span></span> <span data-ttu-id="6a7e7-265">Par exemple, en envoyant par lot des e-mails avec des liens de confirmation.</span><span class="sxs-lookup"><span data-stu-id="6a7e7-265">For example, batch-send emails with confirmation links.</span></span>
