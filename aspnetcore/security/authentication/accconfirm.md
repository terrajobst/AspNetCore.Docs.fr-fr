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
ms.openlocfilehash: d7c1aea2b533fc614eb25c537b72bea773e76077
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252280"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="81537-103">Confirmation du compte et récupération de mot de passe dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="81537-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="81537-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="81537-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="81537-105">Ce didacticiel vous montre comment créer une application ASP.NET Core avec confirmation par courrier électronique et réinitialisation de mot de passe.</span><span class="sxs-lookup"><span data-stu-id="81537-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="81537-106">Ce didacticiel n'est **pas** un sujet pour débuter.</span><span class="sxs-lookup"><span data-stu-id="81537-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="81537-107">Vous devez être familiarisé avec :</span><span class="sxs-lookup"><span data-stu-id="81537-107">You should be familiar with:</span></span>

* [<span data-ttu-id="81537-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="81537-108">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="81537-109">Authentification</span><span class="sxs-lookup"><span data-stu-id="81537-109">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="81537-110">Confirmation de compte et récupération de mot de passe</span><span class="sxs-lookup"><span data-stu-id="81537-110">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="81537-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="81537-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="81537-112">Consultez [ce fichier PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) de la version 1.1 de MVC ASP.NET Core et 2.x.</span><span class="sxs-lookup"><span data-stu-id="81537-112">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC 1.1 and 2.x versions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="81537-113">Prérequis</span><span class="sxs-lookup"><span data-stu-id="81537-113">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a><span data-ttu-id="81537-114">Créer un nouveau projet ASP.NET Core avec l’interface en ligne de commande .NET Core</span><span class="sxs-lookup"><span data-stu-id="81537-114">Create a new ASP.NET Core project with the .NET Core CLI</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81537-115">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81537-115">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp --auth Individual -o WebPWrecover
cd WebPWrecover
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

* <span data-ttu-id="81537-117">`--auth Individual` Spécifie le modèle de projet Comptes d’utilisateur individuels.</span><span class="sxs-lookup"><span data-stu-id="81537-117">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="81537-118">Sur Windows, ajoutez l'option `-uld`.</span><span class="sxs-lookup"><span data-stu-id="81537-118">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="81537-119">Elle spécifie que la base de données locale doit être utilisée au lieu de SQLite.</span><span class="sxs-lookup"><span data-stu-id="81537-119">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="81537-120">Exécutez `new mvc --help` pour obtenir de l’aide sur cette commande.</span><span class="sxs-lookup"><span data-stu-id="81537-120">Run `new mvc --help` to get help on this command.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81537-121">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81537-121">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="81537-122">Si vous utilisez l’interface CLI ou SQLite, exécutez la commande suivante dans une fenêtre de commande :</span><span class="sxs-lookup"><span data-stu-id="81537-122">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="81537-123">`--auth Individual` Spécifie le modèle de projet Comptes d’utilisateur individuels.</span><span class="sxs-lookup"><span data-stu-id="81537-123">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="81537-124">Sur Windows, ajoutez l'option `-uld`.</span><span class="sxs-lookup"><span data-stu-id="81537-124">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="81537-125">Elle spécifie que la base de données locale doit être utilisée au lieu de SQLite.</span><span class="sxs-lookup"><span data-stu-id="81537-125">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="81537-126">Exécutez `new mvc --help` pour obtenir de l’aide sur cette commande.</span><span class="sxs-lookup"><span data-stu-id="81537-126">Run `new mvc --help` to get help on this command.</span></span>

---

<span data-ttu-id="81537-127">Vous pouvez également créer un nouveau projet ASP.NET Core avec Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="81537-127">Alternatively, you can create a new ASP.NET Core project with Visual Studio:</span></span>

* <span data-ttu-id="81537-128">Dans Visual Studio, créez un **Application Web** projet.</span><span class="sxs-lookup"><span data-stu-id="81537-128">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="81537-129">Sélectionnez **ASP.NET Core 2.0**.</span><span class="sxs-lookup"><span data-stu-id="81537-129">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="81537-130">**.NET core** est sélectionné dans l’image suivante, mais vous pouvez sélectionner **.NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="81537-130">**.NET Core** is selected in the following image, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="81537-131">Sélectionnez **modifier l’authentification** et la valeur **comptes d’utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="81537-131">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="81537-132">Conservez la valeur par défaut **dans l’application des comptes d’utilisateurs de magasin**.</span><span class="sxs-lookup"><span data-stu-id="81537-132">Keep the default **Store user accounts in-app**.</span></span>

![Nouvelle boîte de dialogue projet indiquant « Radio de comptes d’utilisateur individuels » sélectionnée](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a><span data-ttu-id="81537-134">Tester l’inscription d’un nouvel utilisateur</span><span class="sxs-lookup"><span data-stu-id="81537-134">Test new user registration</span></span>

<span data-ttu-id="81537-135">Exécutez l’application, sélectionnez le lien **S'inscrire** et inscrivez un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="81537-135">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="81537-136">Suivez les instructions pour exécuter les migrations d’Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="81537-136">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="81537-137">À ce stade, la seule validation sur l’adresse e-mail se fait avec l'attribut [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute).</span><span class="sxs-lookup"><span data-stu-id="81537-137">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="81537-138">Après avoir soumis l’inscription, vous êtes connecté à l’application.</span><span class="sxs-lookup"><span data-stu-id="81537-138">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="81537-139">Plus loin dans ce tutoriel, le code est mis à jour pour que les nouveaux utilisateurs ne puissent pas se connecter tant que leur e-mail n’a pas été validé.</span><span class="sxs-lookup"><span data-stu-id="81537-139">Later in the tutorial, the code is updated so new users can't log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="81537-140">Afficher la base de données d’identité</span><span class="sxs-lookup"><span data-stu-id="81537-140">View the Identity database</span></span>

<span data-ttu-id="81537-141">Consultez [de travail dans un projet ASP.NET MVC de base avec SQLite](xref:tutorials/first-mvc-app-xplat/working-with-sql) pour obtenir des instructions sur l’affichage de la base de données SQLite.</span><span class="sxs-lookup"><span data-stu-id="81537-141">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

<span data-ttu-id="81537-142">Pour Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="81537-142">For Visual Studio:</span></span>

* <span data-ttu-id="81537-143">À partir du menu **Affichage**, sélectionnez **l’Explorateur d’objets SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="81537-143">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="81537-144">Accédez à **(localdb) MSSQLLocalDB (SQL Server 13)**.</span><span class="sxs-lookup"><span data-stu-id="81537-144">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="81537-145">Cliquez avec le bouton droit sur **dbo. AspNetUsers** > **Afficher les données** :</span><span class="sxs-lookup"><span data-stu-id="81537-145">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Menu contextuel sur la table AspNetUsers dans l’Explorateur d’objets SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="81537-147">Notez que le champ `EmailConfirmed` de la table est à `False`.</span><span class="sxs-lookup"><span data-stu-id="81537-147">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="81537-148">Vous pouvez souhaiter réutiliser cet e-mail à l’étape suivante quand l’application envoie un e-mail de confirmation.</span><span class="sxs-lookup"><span data-stu-id="81537-148">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="81537-149">Cliquez avec le bouton droit sur la ligne, puis sélectionnez **Supprimer**.</span><span class="sxs-lookup"><span data-stu-id="81537-149">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="81537-150">La suppression de l’alias d’e-mail facilite les étapes suivantes.</span><span class="sxs-lookup"><span data-stu-id="81537-150">Deleting the email alias makes it easier in the following steps.</span></span>

---

## <a name="require-https"></a><span data-ttu-id="81537-151">Exiger HTTPS</span><span class="sxs-lookup"><span data-stu-id="81537-151">Require HTTPS</span></span>

<span data-ttu-id="81537-152">Consultez [Exiger HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="81537-152">See [Require HTTPS](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="81537-153">Demander confirmation de courrier électronique</span><span class="sxs-lookup"><span data-stu-id="81537-153">Require email confirmation</span></span>

<span data-ttu-id="81537-154">Il est recommandé de confirmer l’adresse de messagerie d’un nouvel enregistrement d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="81537-154">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="81537-155">Envoyer un courrier électronique de confirmation permet de vérifier qu’ils n'empruntent pas l'identité d'une autre personne (autrement dit, ils ne se sont pas inscrits avec l'adresse de messageriedquelqu'un d’autre).</span><span class="sxs-lookup"><span data-stu-id="81537-155">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="81537-156">Supposons que vous ayez un forum de discussion, et que vous souhaitiez empêcher "yli@example.com" de s’inscription en tant que "nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="81537-156">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="81537-157">Sans la confirmation par courrier électronique, "nolivetto@contoso.com" peut recevoir un e-mail indésirable de votre application.</span><span class="sxs-lookup"><span data-stu-id="81537-157">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="81537-158">Supposons que l’utilisateur s'inscrit par inadvertance en tant que "ylo@example.com" et que vous n’avez pas remarqué la faute d’orthographe de "yli".</span><span class="sxs-lookup"><span data-stu-id="81537-158">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="81537-159">Ils ne pourraient pas utiliser la récupération de mot de passe, car l’application n’a pas leur courrier électronique correct.</span><span class="sxs-lookup"><span data-stu-id="81537-159">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="81537-160">l'email de confirmation fournit uniquement une protection limitée contre les robots.</span><span class="sxs-lookup"><span data-stu-id="81537-160">Email confirmation provides only limited protection from bots.</span></span> <span data-ttu-id="81537-161">L'email de confirmation ne fournit pas une protection contre les utilisateurs malveillants avec plusieurs comptes de messagerie.</span><span class="sxs-lookup"><span data-stu-id="81537-161">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="81537-162">En règle générale, vous souhaitez empêcher les nouveaux utilisateurs d'envoyer des données à votre site web tant que leur adresse e-mail n’est pas confirmée.</span><span class="sxs-lookup"><span data-stu-id="81537-162">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="81537-163">Mettez à jour `ConfigureServices` pour exiger une adresse e-mail confirmée :</span><span class="sxs-lookup"><span data-stu-id="81537-163">Update `ConfigureServices` to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

<span data-ttu-id="81537-164">`config.SignIn.RequireConfirmedEmail = true;` empêche les utilisateurs enregistrés de se connecter jusqu'à ce que leur adresse de messagerie soit confirmée.</span><span class="sxs-lookup"><span data-stu-id="81537-164">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="81537-165">Configurer le fournisseur de messagerie</span><span class="sxs-lookup"><span data-stu-id="81537-165">Configure email provider</span></span>

<span data-ttu-id="81537-166">Dans ce didacticiel, SendGrid est utilisé pour envoyer un courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="81537-166">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="81537-167">Vous avez besoin d’un compte SendGrid et une clé pour envoyer un courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="81537-167">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="81537-168">Vous pouvez utiliser d’autres fournisseurs de messagerie.</span><span class="sxs-lookup"><span data-stu-id="81537-168">You can use other email providers.</span></span> <span data-ttu-id="81537-169">ASP.NET Core 2.x inclut `System.Net.Mail`, qui vous permet d’envoyer par courrier électronique à partir de votre application.</span><span class="sxs-lookup"><span data-stu-id="81537-169">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="81537-170">Nous vous recommandons d'utiliser SendGrid ou un autre service de messagerie pour envoyer un courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="81537-170">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="81537-171">SMTP est difficile à sécuriser et à correctement configurer.</span><span class="sxs-lookup"><span data-stu-id="81537-171">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="81537-172">Le [modèle d’Options](xref:fundamentals/configuration/options) est utilisé pour accéder aux paramètres de compte et une clé utilisateur.</span><span class="sxs-lookup"><span data-stu-id="81537-172">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="81537-173">Pour plus d’informations, consultez [configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="81537-173">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="81537-174">Créez une classe pour extraire la clé de messagerie électronique sécurisée.</span><span class="sxs-lookup"><span data-stu-id="81537-174">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="81537-175">Pour cet exemple, la classe `AuthMessageSenderOptions` est créée dans le fichier *Services/AuthMessageSenderOptions.cs* :</span><span class="sxs-lookup"><span data-stu-id="81537-175">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="81537-176">Définissez `SendGridUser` et `SendGridKey` avec l'[outil Gestionnaire de secret](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="81537-176">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="81537-177">Exemple :</span><span class="sxs-lookup"><span data-stu-id="81537-177">For example:</span></span>

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="81537-178">Sur Windows, le gestionnaire de secret stocke des paires de clés/valeur dans un fichier *secrets.json* dans le répertoire `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`.</span><span class="sxs-lookup"><span data-stu-id="81537-178">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="81537-179">Le contenu du fichier *secrets.json* n’est pas chiffré.</span><span class="sxs-lookup"><span data-stu-id="81537-179">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="81537-180">Le fichier *secrets.json* est présenté ci-dessous (la valeur `SendGridKey` a été supprimée.)</span><span class="sxs-lookup"><span data-stu-id="81537-180">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="81537-181">Configurer le démarrage pour utiliser AuthMessageSenderOptions</span><span class="sxs-lookup"><span data-stu-id="81537-181">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="81537-182">Ajouter `AuthMessageSenderOptions` au conteneur de service à la fin de la méthode `ConfigureServices` dans le fichier *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="81537-182">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81537-183">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81537-183">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81537-184">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81537-184">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="81537-185">Configurer la classe AuthMessageSender</span><span class="sxs-lookup"><span data-stu-id="81537-185">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="81537-186">Ce didacticiel montre comment ajouter des notifications par courrier électronique via [SendGrid](https://sendgrid.com/), mais vous pouvez envoyer des e-mails en utilisant SMTP et d'autres mécanismes.</span><span class="sxs-lookup"><span data-stu-id="81537-186">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="81537-187">Installer le package NuGet `SendGrid` :</span><span class="sxs-lookup"><span data-stu-id="81537-187">Install the `SendGrid` NuGet package:</span></span>

* <span data-ttu-id="81537-188">À partir de la ligne de commande :</span><span class="sxs-lookup"><span data-stu-id="81537-188">From the command line:</span></span>

    `dotnet add package SendGrid`

* <span data-ttu-id="81537-189">À partir de la Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="81537-189">From the Package Manager Console, enter the following command:</span></span>

  `Install-Package SendGrid`

<span data-ttu-id="81537-190">Consultez [Débuter avec SendGrid gratuitement](https://sendgrid.com/free/) pour vous inscrire pour un compte SendGrid gratuit.</span><span class="sxs-lookup"><span data-stu-id="81537-190">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="81537-191">Configurer SendGrid</span><span class="sxs-lookup"><span data-stu-id="81537-191">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81537-192">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81537-192">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="81537-193">Pour configurer SendGrid, ajoutez du code semblable au suivant dans *Services/EmailSender.cs*:</span><span class="sxs-lookup"><span data-stu-id="81537-193">To configure SendGrid, add code similar to the following in *Services/EmailSender.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81537-194">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81537-194">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="81537-195">Ajoutez le code dans *Services/MessageServices.cs* similaire à ce qui suit pour configurer SendGrid :</span><span class="sxs-lookup"><span data-stu-id="81537-195">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="81537-196">Activer la récupération de confirmation et le mot de passe du compte</span><span class="sxs-lookup"><span data-stu-id="81537-196">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="81537-197">Le modèle contient du code pour la confirmation du compte et la récupération de mot de passe.</span><span class="sxs-lookup"><span data-stu-id="81537-197">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="81537-198">Recherchez la méthode `OnPostAsync` dans *Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="81537-198">Find the `OnPostAsync` method in *Pages/Account/Register.cshtml.cs*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81537-199">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81537-199">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="81537-200">Empêchez les utilisateurs nouvellement inscrits d'être connectés automatiquement en plaçant la ligne suivante en commentaire :</span><span class="sxs-lookup"><span data-stu-id="81537-200">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="81537-201">La méthode complète est montrée avec la ligne modifiée mise en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="81537-201">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81537-202">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81537-202">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="81537-203">Pour activer la confirmation du compte, supprimez le code suivant :</span><span class="sxs-lookup"><span data-stu-id="81537-203">To enable account confirmation, uncomment the following code:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="81537-204">**Remarque :** le code empêche un utilisateur nouvellement inscrit d'ouvrir automatiquement une session via la mise en commentaire de la ligne suivante :</span><span class="sxs-lookup"><span data-stu-id="81537-204">**Note:** The code is preventing a newly registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="81537-205">Activez la récupération de mot de passe en décommentant le code dans l'action `ForgotPassword` de *Controllers/AccountController.cs*:</span><span class="sxs-lookup"><span data-stu-id="81537-205">Enable password recovery by uncommenting the code in the `ForgotPassword` action of *Controllers/AccountController.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="81537-206">Supprimez les commentaires de l’élément form *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="81537-206">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="81537-207">Vous souhaiterez peut-être supprimer le `<p> For more information on how to enable reset password ... </p>` élément, qui contient un lien vers cet article.</span><span class="sxs-lookup"><span data-stu-id="81537-207">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element, which contains a link to this article.</span></span>

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="81537-208">Inscrire, confirmer l’adresse e-mail et réinitialiser le mot de passe</span><span class="sxs-lookup"><span data-stu-id="81537-208">Register, confirm email, and reset password</span></span>

<span data-ttu-id="81537-209">Exécuter l’application web, et testez le flux de confirmation du compte et de récupération du mot de passe.</span><span class="sxs-lookup"><span data-stu-id="81537-209">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="81537-210">Exécutez l’application et inscrire un nouvel utilisateur</span><span class="sxs-lookup"><span data-stu-id="81537-210">Run the app and register a new user</span></span>

  ![Affichage du livre de comptes application Web](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="81537-212">Recherchez le lien de confirmation du compte dans votre messagerie.</span><span class="sxs-lookup"><span data-stu-id="81537-212">Check your email for the account confirmation link.</span></span> <span data-ttu-id="81537-213">Consultez [Déboguer la messagerie](#debug) si vous ne recevez pas l’e-mail.</span><span class="sxs-lookup"><span data-stu-id="81537-213">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="81537-214">Cliquez sur le lien pour confirmer votre adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="81537-214">Click the link to confirm your email.</span></span>
* <span data-ttu-id="81537-215">Connectez-vous avec votre adresse électronique et un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="81537-215">Log in with your email and password.</span></span>
* <span data-ttu-id="81537-216">Fermez la session.</span><span class="sxs-lookup"><span data-stu-id="81537-216">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="81537-217">Afficher la page de gestion</span><span class="sxs-lookup"><span data-stu-id="81537-217">View the manage page</span></span>

<span data-ttu-id="81537-218">Sélectionnez votre nom d’utilisateur dans le navigateur : ![fenêtre de navigateur avec le nom d’utilisateur](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="81537-218">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="81537-219">Vous devrez peut-être développer la barre de navigation pour afficher le nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="81537-219">You might need to expand the navbar to see user name.</span></span>

![barre de navigation](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81537-221">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81537-221">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="81537-222">La page de gestion s’affiche avec l'onglet **profil** sélectionné.</span><span class="sxs-lookup"><span data-stu-id="81537-222">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="81537-223">L'**Email** affiche une case à cocher indiquant que l’adresse de messagerie a été confirmée.</span><span class="sxs-lookup"><span data-stu-id="81537-223">The **Email** shows a check box indicating the email has been confirmed.</span></span>

![page Gérer](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81537-225">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81537-225">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="81537-226">Cela est mentionné plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="81537-226">This is mentioned later in the tutorial.</span></span>
<span data-ttu-id="81537-227">![page Gérer](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="81537-227">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="81537-228">Tester la réinitialisation du mot de passe</span><span class="sxs-lookup"><span data-stu-id="81537-228">Test password reset</span></span>

* <span data-ttu-id="81537-229">Si vous êtes connecté, sélectionnez **Déconnexion**.</span><span class="sxs-lookup"><span data-stu-id="81537-229">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="81537-230">Sélectionnez le lien **Connexion** et sélectionnez le lien **Vous avez oublié votre mot de passe ?**.</span><span class="sxs-lookup"><span data-stu-id="81537-230">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="81537-231">Entrez l’e-mail que vous avez utilisé pour enregistrer le compte.</span><span class="sxs-lookup"><span data-stu-id="81537-231">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="81537-232">Un message électronique contenant un lien pour réinitialiser votre mot de passe est envoyé.</span><span class="sxs-lookup"><span data-stu-id="81537-232">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="81537-233">Vérifiez votre adresse de messagerie et cliquez sur le lien pour réinitialiser votre mot de passe.</span><span class="sxs-lookup"><span data-stu-id="81537-233">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="81537-234">Une fois votre mot de passe a été réinitialisé avec succès, vous pouvez vous connecter avec votre adresse électronique et un nouveau mot de passe.</span><span class="sxs-lookup"><span data-stu-id="81537-234">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="81537-235">Déboguer le courrier électronique</span><span class="sxs-lookup"><span data-stu-id="81537-235">Debug email</span></span>

<span data-ttu-id="81537-236">Si vous ne parvenez à faire fonctionner l'email :</span><span class="sxs-lookup"><span data-stu-id="81537-236">If you can't get email working:</span></span>

* <span data-ttu-id="81537-237">Créer une [application console pour envoyer un courrier électronique](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="81537-237">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="81537-238">Examinez la page [Activité de la messagerie](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span><span class="sxs-lookup"><span data-stu-id="81537-238">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="81537-239">Vérifiez votre dossier courrier indésirable.</span><span class="sxs-lookup"><span data-stu-id="81537-239">Check your spam folder.</span></span>
* <span data-ttu-id="81537-240">Essayez un autre alias de messagerie électronique sur un fournisseur de messagerie différents (Microsoft, Yahoo, Gmail, etc.).</span><span class="sxs-lookup"><span data-stu-id="81537-240">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="81537-241">Essayez d’envoyer à des comptes de messagerie différents.</span><span class="sxs-lookup"><span data-stu-id="81537-241">Try sending to different email accounts.</span></span>

<span data-ttu-id="81537-242">Une **bonne pratique de sécurité** consiste à **ne pas** utiliser des secrets de production en développement et en test.</span><span class="sxs-lookup"><span data-stu-id="81537-242">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="81537-243">Si vous publiez l’application sur Azure, vous pouvez définir les secrets SendGrid en tant que paramètres d’application dans le portail Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="81537-243">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="81537-244">Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="81537-244">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="81537-245">Combiner des comptes de connexion de réseaux sociaux et local</span><span class="sxs-lookup"><span data-stu-id="81537-245">Combine social and local login accounts</span></span>

<span data-ttu-id="81537-246">Pour terminer cette section, vous devez d’abord activer un fournisseur d’authentification externe.</span><span class="sxs-lookup"><span data-stu-id="81537-246">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="81537-247">Consultez [Facebook, Google et l’authentification du fournisseur externe](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="81537-247">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="81537-248">Vous pouvez combiner des comptes locaux et de réseaux sociaux en cliquant sur le lien de votre e-mail.</span><span class="sxs-lookup"><span data-stu-id="81537-248">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="81537-249">Dans la séquence suivante, "RickAndMSFT@gmail.com" est d’abord créé en tant que connexion locale ; cependant, vous pouvez d’abord créer le compte en tant que connexion de réseau social, puis ajouter une connexion locale.</span><span class="sxs-lookup"><span data-stu-id="81537-249">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Application Web : RickAndMSFT@gmail.com utilisateur authentifié](accconfirm/_static/rick.png)

<span data-ttu-id="81537-251">Cliquez sur le lien **Gérer**.</span><span class="sxs-lookup"><span data-stu-id="81537-251">Click on the **Manage** link.</span></span> <span data-ttu-id="81537-252">Notez la valeur 0 externe (connexions sociales) associé à ce compte.</span><span class="sxs-lookup"><span data-stu-id="81537-252">Note the 0 external (social logins) associated with this account.</span></span>

![Gérer les affichages](accconfirm/_static/manage.png)

<span data-ttu-id="81537-254">Cliquez sur le lien vers un autre service de connexion et acceptez les demandes d’application.</span><span class="sxs-lookup"><span data-stu-id="81537-254">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="81537-255">Dans l’image suivante, Facebook est le fournisseur d’authentification externe :</span><span class="sxs-lookup"><span data-stu-id="81537-255">In the following image, Facebook is the external authentication provider:</span></span>

![Gestion de votre vue de logins externes répertoriant Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="81537-257">Les deux comptes ont été combinés.</span><span class="sxs-lookup"><span data-stu-id="81537-257">The two accounts have been combined.</span></span> <span data-ttu-id="81537-258">Vous pouvez vous connecter avec l'un ou l'autre compte.</span><span class="sxs-lookup"><span data-stu-id="81537-258">You are able to log on with either account.</span></span> <span data-ttu-id="81537-259">Vous pouvez faire en sorte que vos utilisateurs ajoutent des comptes locaux au cas où leur service d’authentification de connexion sociale soit indisponible ou que, plus probablement, ils aient perdu l’accès à leur compte social.</span><span class="sxs-lookup"><span data-stu-id="81537-259">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="81537-260">Activer la confirmation du compte après qu’un site a des utilisateurs</span><span class="sxs-lookup"><span data-stu-id="81537-260">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="81537-261">Activer la confirmation du compte sur un site avec des utilisateurs bloque tous les utilisateurs existants.</span><span class="sxs-lookup"><span data-stu-id="81537-261">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="81537-262">Ceux-ci sont bloqués, car leurs comptes ne sont pas confirmés.</span><span class="sxs-lookup"><span data-stu-id="81537-262">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="81537-263">Pour contourner le blocage des utilisateurs existants, utilisez une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="81537-263">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="81537-264">Mise à jour de la base de données pour marquer tous les utilisateurs existants comme étant confirmés.</span><span class="sxs-lookup"><span data-stu-id="81537-264">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="81537-265">Confirmation des utilisateurs existants.</span><span class="sxs-lookup"><span data-stu-id="81537-265">Confirm exiting users.</span></span> <span data-ttu-id="81537-266">Par exemple, en envoyant par lot des e-mails avec des liens de confirmation.</span><span class="sxs-lookup"><span data-stu-id="81537-266">For example, batch-send emails with confirmation links.</span></span>
