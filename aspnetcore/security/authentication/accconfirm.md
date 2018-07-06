---
title: Confirmation de compte et de récupération de mot de passe dans ASP.NET Core
author: rick-anderson
description: Découvrez comment créer une application ASP.NET Core avec la réinitialisation de confirmation et le mot de passe de messagerie.
ms.author: riande
ms.date: 2/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 12265903f60ff6d62befc445434db025c244c178
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803270"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="fc40f-103">Confirmation de compte et de récupération de mot de passe dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fc40f-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="fc40f-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="fc40f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="fc40f-105">Ce didacticiel vous montre comment créer une application ASP.NET Core avec confirmation par courrier électronique et réinitialisation de mot de passe.</span><span class="sxs-lookup"><span data-stu-id="fc40f-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="fc40f-106">Ce didacticiel n'est **pas** un sujet pour débuter.</span><span class="sxs-lookup"><span data-stu-id="fc40f-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="fc40f-107">Vous devez être familiarisé avec :</span><span class="sxs-lookup"><span data-stu-id="fc40f-107">You should be familiar with:</span></span>

* [<span data-ttu-id="fc40f-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fc40f-108">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="fc40f-109">Authentification</span><span class="sxs-lookup"><span data-stu-id="fc40f-109">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="fc40f-110">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="fc40f-110">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="fc40f-111">Consultez [ce fichier PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) pour les versions d’ASP.NET Core MVC 1.1 et 2.x.</span><span class="sxs-lookup"><span data-stu-id="fc40f-111">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC 1.1 and 2.x versions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fc40f-112">Prérequis</span><span class="sxs-lookup"><span data-stu-id="fc40f-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a><span data-ttu-id="fc40f-113">Créer un nouveau projet ASP.NET Core avec l’interface en ligne de commande .NET Core</span><span class="sxs-lookup"><span data-stu-id="fc40f-113">Create a new ASP.NET Core project with the .NET Core CLI</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fc40f-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fc40f-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

* <span data-ttu-id="fc40f-115">`--auth Individual` Spécifie le modèle de projet Comptes d’utilisateur individuels.</span><span class="sxs-lookup"><span data-stu-id="fc40f-115">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="fc40f-116">Sur Windows, ajoutez l'option `-uld`.</span><span class="sxs-lookup"><span data-stu-id="fc40f-116">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="fc40f-117">Elle spécifie que la base de données locale doit être utilisée au lieu de SQLite.</span><span class="sxs-lookup"><span data-stu-id="fc40f-117">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="fc40f-118">Exécutez `new mvc --help` pour obtenir de l’aide sur cette commande.</span><span class="sxs-lookup"><span data-stu-id="fc40f-118">Run `new mvc --help` to get help on this command.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fc40f-119">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fc40f-119">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="fc40f-120">Si vous utilisez l’interface CLI ou SQLite, exécutez la commande suivante dans une fenêtre de commande :</span><span class="sxs-lookup"><span data-stu-id="fc40f-120">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="fc40f-121">`--auth Individual` Spécifie le modèle de projet Comptes d’utilisateur individuels.</span><span class="sxs-lookup"><span data-stu-id="fc40f-121">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="fc40f-122">Sur Windows, ajoutez l'option `-uld`.</span><span class="sxs-lookup"><span data-stu-id="fc40f-122">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="fc40f-123">Elle spécifie que la base de données locale doit être utilisée au lieu de SQLite.</span><span class="sxs-lookup"><span data-stu-id="fc40f-123">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="fc40f-124">Exécutez `new mvc --help` pour obtenir de l’aide sur cette commande.</span><span class="sxs-lookup"><span data-stu-id="fc40f-124">Run `new mvc --help` to get help on this command.</span></span>

---

<span data-ttu-id="fc40f-125">Vous pouvez également créer un nouveau projet ASP.NET Core avec Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="fc40f-125">Alternatively, you can create a new ASP.NET Core project with Visual Studio:</span></span>

* <span data-ttu-id="fc40f-126">Dans Visual Studio, créez un nouveau **Web Application** projet.</span><span class="sxs-lookup"><span data-stu-id="fc40f-126">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="fc40f-127">Sélectionnez **ASP.NET Core 2.0**.</span><span class="sxs-lookup"><span data-stu-id="fc40f-127">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="fc40f-128">**.NET core** est sélectionné dans l’image suivante, mais vous pouvez sélectionner **.NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="fc40f-128">**.NET Core** is selected in the following image, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="fc40f-129">Sélectionnez **modifier l’authentification** et la valeur **comptes d’utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="fc40f-129">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="fc40f-130">Conservez la valeur par défaut **dans l’application des comptes d’utilisateurs de Store**.</span><span class="sxs-lookup"><span data-stu-id="fc40f-130">Keep the default **Store user accounts in-app**.</span></span>

![Nouvelle boîte de dialogue projet affichant « Radio de comptes d’utilisateur individuels » sélectionnée](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a><span data-ttu-id="fc40f-132">Tester l’inscription d’un nouvel utilisateur</span><span class="sxs-lookup"><span data-stu-id="fc40f-132">Test new user registration</span></span>

<span data-ttu-id="fc40f-133">Exécutez l’application, sélectionnez le lien **S'inscrire** et inscrivez un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="fc40f-133">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="fc40f-134">Suivez les instructions pour exécuter les migrations d’Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="fc40f-134">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="fc40f-135">À ce stade, la seule validation sur l’adresse e-mail se fait avec l'attribut [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute).</span><span class="sxs-lookup"><span data-stu-id="fc40f-135">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="fc40f-136">Après avoir soumis l’inscription, vous êtes connecté à l’application.</span><span class="sxs-lookup"><span data-stu-id="fc40f-136">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="fc40f-137">Plus loin dans ce tutoriel, le code est mis à jour pour que les nouveaux utilisateurs ne puissent pas se connecter tant que leur e-mail n’a pas été validé.</span><span class="sxs-lookup"><span data-stu-id="fc40f-137">Later in the tutorial, the code is updated so new users can't log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="fc40f-138">Afficher la base de données d’identité</span><span class="sxs-lookup"><span data-stu-id="fc40f-138">View the Identity database</span></span>

<span data-ttu-id="fc40f-139">Consultez [utiliser SQLite dans un projet ASP.NET Core MVC](xref:tutorials/first-mvc-app-xplat/working-with-sql) pour obtenir des instructions sur l’affichage de la base de données SQLite.</span><span class="sxs-lookup"><span data-stu-id="fc40f-139">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

<span data-ttu-id="fc40f-140">Pour Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="fc40f-140">For Visual Studio:</span></span>

* <span data-ttu-id="fc40f-141">À partir du menu **Affichage**, sélectionnez **l’Explorateur d’objets SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="fc40f-141">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="fc40f-142">Accédez à **(localdb) MSSQLLocalDB (SQL Server 13)**.</span><span class="sxs-lookup"><span data-stu-id="fc40f-142">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="fc40f-143">Cliquez avec le bouton droit sur **dbo. AspNetUsers** > **Afficher les données** :</span><span class="sxs-lookup"><span data-stu-id="fc40f-143">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Menu contextuel sur la table AspNetUsers dans l’Explorateur d’objets SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="fc40f-145">Notez que le champ `EmailConfirmed` de la table est à `False`.</span><span class="sxs-lookup"><span data-stu-id="fc40f-145">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="fc40f-146">Vous pouvez souhaiter réutiliser cet e-mail à l’étape suivante quand l’application envoie un e-mail de confirmation.</span><span class="sxs-lookup"><span data-stu-id="fc40f-146">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="fc40f-147">Cliquez avec le bouton droit sur la ligne, puis sélectionnez **Supprimer**.</span><span class="sxs-lookup"><span data-stu-id="fc40f-147">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="fc40f-148">La suppression de l’alias d’e-mail facilite les étapes suivantes.</span><span class="sxs-lookup"><span data-stu-id="fc40f-148">Deleting the email alias makes it easier in the following steps.</span></span>

---

## <a name="require-https"></a><span data-ttu-id="fc40f-149">Exiger s-http</span><span class="sxs-lookup"><span data-stu-id="fc40f-149">Require HTTPS</span></span>

<span data-ttu-id="fc40f-150">Consultez [Exiger HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="fc40f-150">See [Require HTTPS](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="fc40f-151">Demander confirmation de l’e-mail</span><span class="sxs-lookup"><span data-stu-id="fc40f-151">Require email confirmation</span></span>

<span data-ttu-id="fc40f-152">Il est recommandé de confirmer l’adresse de messagerie d’un nouvel enregistrement d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="fc40f-152">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="fc40f-153">Envoyer un courrier électronique de confirmation permet de vérifier qu’ils n'empruntent pas l'identité d'une autre personne (autrement dit, ils ne se sont pas inscrits avec l'adresse de messageriedquelqu'un d’autre).</span><span class="sxs-lookup"><span data-stu-id="fc40f-153">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="fc40f-154">Supposons que vous ayez un forum de discussion, et que vous souhaitiez empêcher "yli@example.com" de s’inscription en tant que "nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="fc40f-154">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="fc40f-155">Sans la confirmation par courrier électronique, "nolivetto@contoso.com" peut recevoir un e-mail indésirable de votre application.</span><span class="sxs-lookup"><span data-stu-id="fc40f-155">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="fc40f-156">Supposons que l’utilisateur s'inscrit par inadvertance en tant que "ylo@example.com" et que vous n’avez pas remarqué la faute d’orthographe de "yli".</span><span class="sxs-lookup"><span data-stu-id="fc40f-156">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="fc40f-157">Ils ne pourraient pas utiliser la récupération de mot de passe, car l’application n’a pas leur courrier électronique correct.</span><span class="sxs-lookup"><span data-stu-id="fc40f-157">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="fc40f-158">l'email de confirmation fournit uniquement une protection limitée contre les robots.</span><span class="sxs-lookup"><span data-stu-id="fc40f-158">Email confirmation provides only limited protection from bots.</span></span> <span data-ttu-id="fc40f-159">L'email de confirmation ne fournit pas une protection contre les utilisateurs malveillants avec plusieurs comptes de messagerie.</span><span class="sxs-lookup"><span data-stu-id="fc40f-159">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="fc40f-160">En règle générale, vous souhaitez empêcher les nouveaux utilisateurs d'envoyer des données à votre site web tant que leur adresse e-mail n’est pas confirmée.</span><span class="sxs-lookup"><span data-stu-id="fc40f-160">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="fc40f-161">Mettez à jour `ConfigureServices` pour exiger une adresse e-mail confirmée :</span><span class="sxs-lookup"><span data-stu-id="fc40f-161">Update `ConfigureServices` to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

<span data-ttu-id="fc40f-162">`config.SignIn.RequireConfirmedEmail = true;` empêche les utilisateurs enregistrés de se connecter jusqu'à ce que leur adresse de messagerie soit confirmée.</span><span class="sxs-lookup"><span data-stu-id="fc40f-162">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="fc40f-163">Configurer le fournisseur de messagerie</span><span class="sxs-lookup"><span data-stu-id="fc40f-163">Configure email provider</span></span>

<span data-ttu-id="fc40f-164">Dans ce didacticiel, SendGrid est utilisé pour envoyer un courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="fc40f-164">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="fc40f-165">Vous avez besoin d’un compte SendGrid et une clé pour envoyer un courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="fc40f-165">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="fc40f-166">Vous pouvez utiliser d’autres fournisseurs de messagerie.</span><span class="sxs-lookup"><span data-stu-id="fc40f-166">You can use other email providers.</span></span> <span data-ttu-id="fc40f-167">ASP.NET Core 2.x inclut `System.Net.Mail`, qui vous permet d’envoyer par courrier électronique à partir de votre application.</span><span class="sxs-lookup"><span data-stu-id="fc40f-167">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="fc40f-168">Nous vous recommandons d'utiliser SendGrid ou un autre service de messagerie pour envoyer un courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="fc40f-168">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="fc40f-169">SMTP est difficile à sécuriser et à correctement configurer.</span><span class="sxs-lookup"><span data-stu-id="fc40f-169">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="fc40f-170">Le [modèle Options](xref:fundamentals/configuration/options) est utilisé pour accéder aux paramètres de compte et clé utilisateur.</span><span class="sxs-lookup"><span data-stu-id="fc40f-170">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="fc40f-171">Pour plus d’informations, consultez [configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="fc40f-171">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="fc40f-172">Créez une classe pour extraire la clé de messagerie électronique sécurisée.</span><span class="sxs-lookup"><span data-stu-id="fc40f-172">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="fc40f-173">Pour cet exemple, la classe `AuthMessageSenderOptions` est créée dans le fichier *Services/AuthMessageSenderOptions.cs* :</span><span class="sxs-lookup"><span data-stu-id="fc40f-173">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="fc40f-174">Définissez `SendGridUser` et `SendGridKey` avec l'[outil Gestionnaire de secret](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="fc40f-174">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="fc40f-175">Exemple :</span><span class="sxs-lookup"><span data-stu-id="fc40f-175">For example:</span></span>

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="fc40f-176">Sur Windows, le gestionnaire de secret stocke des paires de clés/valeur dans un fichier *secrets.json* dans le répertoire `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`.</span><span class="sxs-lookup"><span data-stu-id="fc40f-176">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="fc40f-177">Le contenu du fichier *secrets.json* n’est pas chiffré.</span><span class="sxs-lookup"><span data-stu-id="fc40f-177">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="fc40f-178">Le fichier *secrets.json* est présenté ci-dessous (la valeur `SendGridKey` a été supprimée.)</span><span class="sxs-lookup"><span data-stu-id="fc40f-178">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="fc40f-179">Configurer le démarrage pour utiliser AuthMessageSenderOptions</span><span class="sxs-lookup"><span data-stu-id="fc40f-179">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="fc40f-180">Ajouter `AuthMessageSenderOptions` au conteneur de service à la fin de la méthode `ConfigureServices` dans le fichier *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="fc40f-180">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fc40f-181">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fc40f-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fc40f-182">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fc40f-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="fc40f-183">Configurer la classe AuthMessageSender</span><span class="sxs-lookup"><span data-stu-id="fc40f-183">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="fc40f-184">Ce didacticiel montre comment ajouter des notifications par courrier électronique via [SendGrid](https://sendgrid.com/), mais vous pouvez envoyer des e-mails en utilisant SMTP et d'autres mécanismes.</span><span class="sxs-lookup"><span data-stu-id="fc40f-184">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="fc40f-185">Installer le package NuGet `SendGrid` :</span><span class="sxs-lookup"><span data-stu-id="fc40f-185">Install the `SendGrid` NuGet package:</span></span>

* <span data-ttu-id="fc40f-186">À partir de la ligne de commande :</span><span class="sxs-lookup"><span data-stu-id="fc40f-186">From the command line:</span></span>

    `dotnet add package SendGrid`

* <span data-ttu-id="fc40f-187">À partir de la Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="fc40f-187">From the Package Manager Console, enter the following command:</span></span>

  `Install-Package SendGrid`

<span data-ttu-id="fc40f-188">Consultez [Débuter avec SendGrid gratuitement](https://sendgrid.com/free/) pour vous inscrire pour un compte SendGrid gratuit.</span><span class="sxs-lookup"><span data-stu-id="fc40f-188">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="fc40f-189">Configuration de SendGrid</span><span class="sxs-lookup"><span data-stu-id="fc40f-189">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fc40f-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fc40f-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="fc40f-191">Pour configurer SendGrid, ajoutez le code similaire à ce qui suit dans *Services/EmailSender.cs*:</span><span class="sxs-lookup"><span data-stu-id="fc40f-191">To configure SendGrid, add code similar to the following in *Services/EmailSender.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fc40f-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fc40f-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="fc40f-193">Ajoutez le code dans *Services/MessageServices.cs* similaire à ce qui suit pour configurer SendGrid :</span><span class="sxs-lookup"><span data-stu-id="fc40f-193">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="fc40f-194">Activer la récupération de confirmation et le mot de passe du compte</span><span class="sxs-lookup"><span data-stu-id="fc40f-194">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="fc40f-195">Le modèle contient du code pour la confirmation du compte et la récupération de mot de passe.</span><span class="sxs-lookup"><span data-stu-id="fc40f-195">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="fc40f-196">Recherchez la méthode `OnPostAsync` dans *Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="fc40f-196">Find the `OnPostAsync` method in *Pages/Account/Register.cshtml.cs*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fc40f-197">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fc40f-197">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="fc40f-198">Empêchez les utilisateurs nouvellement inscrits d'être connectés automatiquement en plaçant la ligne suivante en commentaire :</span><span class="sxs-lookup"><span data-stu-id="fc40f-198">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="fc40f-199">La méthode complète est montrée avec la ligne modifiée mise en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="fc40f-199">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fc40f-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fc40f-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="fc40f-201">Pour activer la confirmation de compte, les commentaires du code suivant :</span><span class="sxs-lookup"><span data-stu-id="fc40f-201">To enable account confirmation, uncomment the following code:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="fc40f-202">**Remarque :** le code empêche un utilisateur nouvellement inscrit d'ouvrir automatiquement une session via la mise en commentaire de la ligne suivante :</span><span class="sxs-lookup"><span data-stu-id="fc40f-202">**Note:** The code is preventing a newly registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="fc40f-203">Activez la récupération de mot de passe en décommentant le code dans l'action `ForgotPassword` de *Controllers/AccountController.cs*:</span><span class="sxs-lookup"><span data-stu-id="fc40f-203">Enable password recovery by uncommenting the code in the `ForgotPassword` action of *Controllers/AccountController.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="fc40f-204">Supprimez les commentaires de l’élément de formulaire dans *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="fc40f-204">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="fc40f-205">Vous souhaiterez peut-être supprimer le `<p> For more information on how to enable reset password ... </p>` élément, qui contient un lien vers cet article.</span><span class="sxs-lookup"><span data-stu-id="fc40f-205">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element, which contains a link to this article.</span></span>

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="fc40f-206">Inscrire, confirmer l’adresse e-mail et réinitialiser le mot de passe</span><span class="sxs-lookup"><span data-stu-id="fc40f-206">Register, confirm email, and reset password</span></span>

<span data-ttu-id="fc40f-207">Exécuter l’application web, et testez le flux de confirmation du compte et de récupération du mot de passe.</span><span class="sxs-lookup"><span data-stu-id="fc40f-207">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="fc40f-208">Exécutez l’application et inscrire un nouvel utilisateur</span><span class="sxs-lookup"><span data-stu-id="fc40f-208">Run the app and register a new user</span></span>

  ![Affichage du livre de comptes application Web](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="fc40f-210">Recherchez le lien de confirmation du compte dans votre messagerie.</span><span class="sxs-lookup"><span data-stu-id="fc40f-210">Check your email for the account confirmation link.</span></span> <span data-ttu-id="fc40f-211">Consultez [Déboguer la messagerie](#debug) si vous ne recevez pas l’e-mail.</span><span class="sxs-lookup"><span data-stu-id="fc40f-211">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="fc40f-212">Cliquez sur le lien pour confirmer votre adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="fc40f-212">Click the link to confirm your email.</span></span>
* <span data-ttu-id="fc40f-213">Connectez-vous à votre e-mail et un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="fc40f-213">Log in with your email and password.</span></span>
* <span data-ttu-id="fc40f-214">Fermez la session.</span><span class="sxs-lookup"><span data-stu-id="fc40f-214">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="fc40f-215">Afficher la page de gestion</span><span class="sxs-lookup"><span data-stu-id="fc40f-215">View the manage page</span></span>

<span data-ttu-id="fc40f-216">Sélectionnez votre nom d’utilisateur dans le navigateur : ![fenêtre de navigateur avec le nom d’utilisateur](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="fc40f-216">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="fc40f-217">Vous devrez peut-être développer la barre de navigation pour afficher le nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="fc40f-217">You might need to expand the navbar to see user name.</span></span>

![barre de navigation](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fc40f-219">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fc40f-219">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="fc40f-220">La page de gestion s’affiche avec l'onglet **profil** sélectionné.</span><span class="sxs-lookup"><span data-stu-id="fc40f-220">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="fc40f-221">L'**Email** affiche une case à cocher indiquant que l’adresse de messagerie a été confirmée.</span><span class="sxs-lookup"><span data-stu-id="fc40f-221">The **Email** shows a check box indicating the email has been confirmed.</span></span>

![page Gérer](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fc40f-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fc40f-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="fc40f-224">Cela est indiqué plus loin dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="fc40f-224">This is mentioned later in the tutorial.</span></span>
<span data-ttu-id="fc40f-225">![page Gérer](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="fc40f-225">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="fc40f-226">Tester la réinitialisation du mot de passe</span><span class="sxs-lookup"><span data-stu-id="fc40f-226">Test password reset</span></span>

* <span data-ttu-id="fc40f-227">Si vous êtes connecté, sélectionnez **Déconnexion**.</span><span class="sxs-lookup"><span data-stu-id="fc40f-227">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="fc40f-228">Sélectionnez le lien **Connexion** et sélectionnez le lien **Vous avez oublié votre mot de passe ?**.</span><span class="sxs-lookup"><span data-stu-id="fc40f-228">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="fc40f-229">Entrez l’adresse e-mail que vous avez utilisé pour inscrire le compte.</span><span class="sxs-lookup"><span data-stu-id="fc40f-229">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="fc40f-230">Un e-mail avec un lien pour réinitialiser votre mot de passe est envoyé.</span><span class="sxs-lookup"><span data-stu-id="fc40f-230">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="fc40f-231">Vérifier votre adresse e-mail et cliquez sur le lien pour réinitialiser votre mot de passe.</span><span class="sxs-lookup"><span data-stu-id="fc40f-231">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="fc40f-232">Une fois que votre mot de passe a été réinitialisé avec succès, vous pouvez vous connecter avec votre e-mail et votre nouveau mot de passe.</span><span class="sxs-lookup"><span data-stu-id="fc40f-232">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="fc40f-233">Déboguer le courrier électronique</span><span class="sxs-lookup"><span data-stu-id="fc40f-233">Debug email</span></span>

<span data-ttu-id="fc40f-234">Si vous ne parvenez à faire fonctionner l'email :</span><span class="sxs-lookup"><span data-stu-id="fc40f-234">If you can't get email working:</span></span>

* <span data-ttu-id="fc40f-235">Créer une [application console pour envoyer un courrier électronique](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="fc40f-235">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="fc40f-236">Examinez la page [Activité de la messagerie](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span><span class="sxs-lookup"><span data-stu-id="fc40f-236">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="fc40f-237">Vérifiez votre dossier courrier indésirable.</span><span class="sxs-lookup"><span data-stu-id="fc40f-237">Check your spam folder.</span></span>
* <span data-ttu-id="fc40f-238">Essayez un autre alias de messagerie sur un fournisseur de messagerie différents (Microsoft, Yahoo, Gmail, etc.).</span><span class="sxs-lookup"><span data-stu-id="fc40f-238">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="fc40f-239">Essayer d’envoyer à différents comptes e-mail.</span><span class="sxs-lookup"><span data-stu-id="fc40f-239">Try sending to different email accounts.</span></span>

<span data-ttu-id="fc40f-240">Une **bonne pratique de sécurité** consiste à **ne pas** utiliser des secrets de production en développement et en test.</span><span class="sxs-lookup"><span data-stu-id="fc40f-240">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="fc40f-241">Si vous publiez l’application sur Azure, vous pouvez définir les secrets SendGrid en tant que paramètres d’application dans le portail Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="fc40f-241">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="fc40f-242">Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="fc40f-242">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="fc40f-243">Combiner des comptes de connexion de réseaux sociaux et local</span><span class="sxs-lookup"><span data-stu-id="fc40f-243">Combine social and local login accounts</span></span>

<span data-ttu-id="fc40f-244">Pour terminer cette section, vous devez d’abord activer un fournisseur d’authentification externe.</span><span class="sxs-lookup"><span data-stu-id="fc40f-244">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="fc40f-245">Consultez [Facebook, Google et l’authentification du fournisseur externe](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="fc40f-245">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="fc40f-246">Vous pouvez combiner des comptes locaux et de réseaux sociaux en cliquant sur le lien de votre e-mail.</span><span class="sxs-lookup"><span data-stu-id="fc40f-246">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="fc40f-247">Dans la séquence suivante, "RickAndMSFT@gmail.com" est d’abord créé en tant que connexion locale ; cependant, vous pouvez d’abord créer le compte en tant que connexion de réseau social, puis ajouter une connexion locale.</span><span class="sxs-lookup"><span data-stu-id="fc40f-247">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Application Web : RickAndMSFT@gmail.com utilisateur authentifié](accconfirm/_static/rick.png)

<span data-ttu-id="fc40f-249">Cliquez sur le lien **Gérer**.</span><span class="sxs-lookup"><span data-stu-id="fc40f-249">Click on the **Manage** link.</span></span> <span data-ttu-id="fc40f-250">Notez la valeur 0 externes (connexions sociales) associé à ce compte.</span><span class="sxs-lookup"><span data-stu-id="fc40f-250">Note the 0 external (social logins) associated with this account.</span></span>

![Gérer la vue](accconfirm/_static/manage.png)

<span data-ttu-id="fc40f-252">Cliquez sur le lien vers un autre service de connexion et acceptez les demandes d’application.</span><span class="sxs-lookup"><span data-stu-id="fc40f-252">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="fc40f-253">Dans l’image suivante, Facebook est le fournisseur d’authentification externe :</span><span class="sxs-lookup"><span data-stu-id="fc40f-253">In the following image, Facebook is the external authentication provider:</span></span>

![Gérer votre vue de logins externes répertoriant Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="fc40f-255">Les deux comptes ont été combinés.</span><span class="sxs-lookup"><span data-stu-id="fc40f-255">The two accounts have been combined.</span></span> <span data-ttu-id="fc40f-256">Vous pouvez vous connecter avec l'un ou l'autre compte.</span><span class="sxs-lookup"><span data-stu-id="fc40f-256">You are able to log on with either account.</span></span> <span data-ttu-id="fc40f-257">Vous pouvez faire en sorte que vos utilisateurs ajoutent des comptes locaux au cas où leur service d’authentification de connexion sociale soit indisponible ou que, plus probablement, ils aient perdu l’accès à leur compte social.</span><span class="sxs-lookup"><span data-stu-id="fc40f-257">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="fc40f-258">Activer la confirmation de compte après qu’un site a des utilisateurs</span><span class="sxs-lookup"><span data-stu-id="fc40f-258">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="fc40f-259">Activer la confirmation du compte sur un site avec des utilisateurs bloque tous les utilisateurs existants.</span><span class="sxs-lookup"><span data-stu-id="fc40f-259">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="fc40f-260">Ceux-ci sont bloqués, car leurs comptes ne sont pas confirmés.</span><span class="sxs-lookup"><span data-stu-id="fc40f-260">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="fc40f-261">Pour contourner le blocage des utilisateurs existants, utilisez une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="fc40f-261">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="fc40f-262">Mise à jour de la base de données pour marquer tous les utilisateurs existants comme étant confirmés.</span><span class="sxs-lookup"><span data-stu-id="fc40f-262">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="fc40f-263">Confirmation des utilisateurs existants.</span><span class="sxs-lookup"><span data-stu-id="fc40f-263">Confirm exiting users.</span></span> <span data-ttu-id="fc40f-264">Par exemple, en envoyant par lot des e-mails avec des liens de confirmation.</span><span class="sxs-lookup"><span data-stu-id="fc40f-264">For example, batch-send emails with confirmation links.</span></span>
