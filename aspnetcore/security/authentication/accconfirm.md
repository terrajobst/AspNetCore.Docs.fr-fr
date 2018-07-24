---
title: Confirmation de compte et de récupération de mot de passe dans ASP.NET Core
author: rick-anderson
description: Découvrez comment créer une application ASP.NET Core avec la réinitialisation de confirmation et le mot de passe de messagerie.
ms.author: riande
ms.date: 7/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 84eb3580107572f66f0c3b565b8e76ba401c0ddb
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219405"
---
::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="d3e91-103">Consultez [ce fichier PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) pour l’ASP.NET Core 1.1 et la version 2.1.</span><span class="sxs-lookup"><span data-stu-id="d3e91-103">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 and 2.1 version.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="d3e91-104">Confirmation de compte et de récupération de mot de passe dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d3e91-104">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="d3e91-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="d3e91-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="d3e91-106">Ce didacticiel montre comment créer une application ASP.NET Core avec la réinitialisation de confirmation et le mot de passe de messagerie.</span><span class="sxs-lookup"><span data-stu-id="d3e91-106">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="d3e91-107">Ce didacticiel n'est **pas** un sujet pour débuter.</span><span class="sxs-lookup"><span data-stu-id="d3e91-107">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="d3e91-108">Vous devez être familiarisé avec :</span><span class="sxs-lookup"><span data-stu-id="d3e91-108">You should be familiar with:</span></span>

* [<span data-ttu-id="d3e91-109">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d3e91-109">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="d3e91-110">Authentification</span><span class="sxs-lookup"><span data-stu-id="d3e91-110">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="d3e91-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="d3e91-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a><span data-ttu-id="d3e91-112">Prérequis</span><span class="sxs-lookup"><span data-stu-id="d3e91-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="d3e91-113">Créer une application web et de structurer des identités</span><span class="sxs-lookup"><span data-stu-id="d3e91-113">Create a web  app and scaffold Identity</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d3e91-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3e91-114">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="d3e91-115">Dans Visual Studio, créez un nouveau **Web Application** projet nommé **WebPWrecover**.</span><span class="sxs-lookup"><span data-stu-id="d3e91-115">In Visual Studio, create a new **Web Application** project named **WebPWrecover**.</span></span>
* <span data-ttu-id="d3e91-116">Sélectionnez **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="d3e91-116">Select **ASP.NET Core 2.1**.</span></span>
* <span data-ttu-id="d3e91-117">Conservez la valeur par défaut **authentification** définie sur **aucune authentification**.</span><span class="sxs-lookup"><span data-stu-id="d3e91-117">Keep the default **Authentication** set to **No Authentication**.</span></span> <span data-ttu-id="d3e91-118">L’authentification est ajoutée à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="d3e91-118">Authentication is added in the next step.</span></span>

<span data-ttu-id="d3e91-119">Dans l’étape suivante :</span><span class="sxs-lookup"><span data-stu-id="d3e91-119">In the next step:</span></span>

* <span data-ttu-id="d3e91-120">La valeur est la page de disposition *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d3e91-120">Set the layout page to *~/Pages/Shared/_Layout.cshtml*</span></span>
* <span data-ttu-id="d3e91-121">Sélectionnez *compte/inscrire*</span><span class="sxs-lookup"><span data-stu-id="d3e91-121">Select *Account/Register*</span></span>
* <span data-ttu-id="d3e91-122">Créer un nouveau **classe de contexte de données**</span><span class="sxs-lookup"><span data-stu-id="d3e91-122">Create a new **Data context class**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d3e91-123">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="d3e91-123">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet new webapp -o WebPWrecover
cd WebPWrecover
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -fi Account.Register -dc WebPWrecover.Models.WebPWrecoverContext
dotnet ef migrations add CreateIdentitySchema
dotnet ef database drop -f
dotnet ef database update
dotnet build
```

<span data-ttu-id="d3e91-124">Exécutez `dotnet aspnet-codegenerator identity --help` pour obtenir de l’aide sur l’outil de génération de modèles automatique.</span><span class="sxs-lookup"><span data-stu-id="d3e91-124">Run `dotnet aspnet-codegenerator identity --help` to get help on the scaffolding tool.</span></span>

------

<span data-ttu-id="d3e91-125">Suivez les instructions de [activer l’authentification](xref:security/authentication/scaffold-identity#useauthentication):</span><span class="sxs-lookup"><span data-stu-id="d3e91-125">Follow the instructions in [Enable authentication](xref:security/authentication/scaffold-identity#useauthentication):</span></span>

* <span data-ttu-id="d3e91-126">Ajouter `app.UseAuthentication();` à `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="d3e91-126">Add `app.UseAuthentication();` to `Startup.Configure`</span></span>
* <span data-ttu-id="d3e91-127">Ajouter `<partial name="_LoginPartial" />` pour le fichier de disposition.</span><span class="sxs-lookup"><span data-stu-id="d3e91-127">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="d3e91-128">Tester l’inscription d’un nouvel utilisateur</span><span class="sxs-lookup"><span data-stu-id="d3e91-128">Test new user registration</span></span>

<span data-ttu-id="d3e91-129">Exécutez l’application, sélectionnez le lien **S'inscrire** et inscrivez un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d3e91-129">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="d3e91-130">À ce stade, la seule validation sur l’adresse e-mail se fait avec l'attribut [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute).</span><span class="sxs-lookup"><span data-stu-id="d3e91-130">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="d3e91-131">Après avoir soumis l’inscription, vous êtes connecté à l’application.</span><span class="sxs-lookup"><span data-stu-id="d3e91-131">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="d3e91-132">Plus loin dans ce didacticiel, le code est mis à jour pour les nouveaux utilisateurs ne peuvent pas se connecter jusqu'à ce que leur courrier électronique est validé.</span><span class="sxs-lookup"><span data-stu-id="d3e91-132">Later in the tutorial, the code is updated so new users can't log in until their email is validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="d3e91-133">Afficher la base de données d’identité</span><span class="sxs-lookup"><span data-stu-id="d3e91-133">View the Identity database</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d3e91-134">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3e91-134">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="d3e91-135">À partir du menu **Affichage**, sélectionnez **l’Explorateur d’objets SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="d3e91-135">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="d3e91-136">Accédez à **(localdb) MSSQLLocalDB (SQL Server 13)**.</span><span class="sxs-lookup"><span data-stu-id="d3e91-136">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="d3e91-137">Cliquez avec le bouton droit sur **dbo. AspNetUsers** > **Afficher les données** :</span><span class="sxs-lookup"><span data-stu-id="d3e91-137">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Menu contextuel sur la table AspNetUsers dans l’Explorateur d’objets SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="d3e91-139">Notez que le champ `EmailConfirmed` de la table est à `False`.</span><span class="sxs-lookup"><span data-stu-id="d3e91-139">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="d3e91-140">Vous pouvez souhaiter réutiliser cet e-mail à l’étape suivante quand l’application envoie un e-mail de confirmation.</span><span class="sxs-lookup"><span data-stu-id="d3e91-140">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="d3e91-141">Cliquez avec le bouton droit sur la ligne, puis sélectionnez **Supprimer**.</span><span class="sxs-lookup"><span data-stu-id="d3e91-141">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="d3e91-142">La suppression de l’alias d’e-mail facilite les étapes suivantes.</span><span class="sxs-lookup"><span data-stu-id="d3e91-142">Deleting the email alias makes it easier in the following steps.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d3e91-143">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="d3e91-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d3e91-144">Consultez [utiliser SQLite dans un projet ASP.NET Core MVC](xref:tutorials/first-mvc-app-xplat/working-with-sql) pour obtenir des instructions sur l’affichage de la base de données SQLite.</span><span class="sxs-lookup"><span data-stu-id="d3e91-144">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

------

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="d3e91-145">Demander confirmation de l’e-mail</span><span class="sxs-lookup"><span data-stu-id="d3e91-145">Require email confirmation</span></span>

<span data-ttu-id="d3e91-146">Il est recommandé de confirmer l’adresse de messagerie d’un nouvel enregistrement d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d3e91-146">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="d3e91-147">Envoyer un courrier électronique de confirmation permet de vérifier qu’ils n'empruntent pas l'identité d'une autre personne (autrement dit, ils ne se sont pas inscrits avec l'adresse de messageriedquelqu'un d’autre).</span><span class="sxs-lookup"><span data-stu-id="d3e91-147">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="d3e91-148">Supposons que vous ayez un forum de discussion, et que vous souhaitiez empêcher "yli@example.com" de s’inscription en tant que "nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="d3e91-148">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="d3e91-149">Sans la confirmation par courrier électronique, "nolivetto@contoso.com" peut recevoir un e-mail indésirable de votre application.</span><span class="sxs-lookup"><span data-stu-id="d3e91-149">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="d3e91-150">Supposons que l’utilisateur s'inscrit par inadvertance en tant que "ylo@example.com" et que vous n’avez pas remarqué la faute d’orthographe de "yli".</span><span class="sxs-lookup"><span data-stu-id="d3e91-150">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="d3e91-151">Ils ne pourraient pas utiliser la récupération de mot de passe, car l’application n’a pas leur courrier électronique correct.</span><span class="sxs-lookup"><span data-stu-id="d3e91-151">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="d3e91-152">E-mail de confirmation offre une protection limitée contre les robots.</span><span class="sxs-lookup"><span data-stu-id="d3e91-152">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="d3e91-153">L'email de confirmation ne fournit pas une protection contre les utilisateurs malveillants avec plusieurs comptes de messagerie.</span><span class="sxs-lookup"><span data-stu-id="d3e91-153">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="d3e91-154">En règle générale, vous souhaitez empêcher les nouveaux utilisateurs d'envoyer des données à votre site web tant que leur adresse e-mail n’est pas confirmée.</span><span class="sxs-lookup"><span data-stu-id="d3e91-154">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="d3e91-155">Mise à jour *Areas/Identity/IdentityHostingStartup.cs* pour exiger un e-mail de confirmation :</span><span class="sxs-lookup"><span data-stu-id="d3e91-155">Update *Areas/Identity/IdentityHostingStartup.cs*  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/IdentityHostingStartup.cs?name=snippet1&highlight=10-13)]

<span data-ttu-id="d3e91-156">`config.SignIn.RequireConfirmedEmail = true;` empêche les utilisateurs enregistrés de se connecter jusqu'à ce que leur adresse de messagerie soit confirmée.</span><span class="sxs-lookup"><span data-stu-id="d3e91-156">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="d3e91-157">Configurer le fournisseur de messagerie</span><span class="sxs-lookup"><span data-stu-id="d3e91-157">Configure email provider</span></span>

<span data-ttu-id="d3e91-158">Dans ce didacticiel, [SendGrid](https://sendgrid.com) est utilisé pour envoyer un courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="d3e91-158">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="d3e91-159">Vous avez besoin d’un compte SendGrid et une clé pour envoyer un courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="d3e91-159">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="d3e91-160">Vous pouvez utiliser d’autres fournisseurs de messagerie.</span><span class="sxs-lookup"><span data-stu-id="d3e91-160">You can use other email providers.</span></span> <span data-ttu-id="d3e91-161">ASP.NET Core 2.x inclut `System.Net.Mail`, qui vous permet d’envoyer par courrier électronique à partir de votre application.</span><span class="sxs-lookup"><span data-stu-id="d3e91-161">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="d3e91-162">Nous vous recommandons d'utiliser SendGrid ou un autre service de messagerie pour envoyer un courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="d3e91-162">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="d3e91-163">SMTP est difficile à sécuriser et à correctement configurer.</span><span class="sxs-lookup"><span data-stu-id="d3e91-163">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="d3e91-164">Le [modèle Options](xref:fundamentals/configuration/options) est utilisé pour accéder aux paramètres de compte et clé utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d3e91-164">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="d3e91-165">Pour plus d’informations, consultez [configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="d3e91-165">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="d3e91-166">Créez une classe pour extraire la clé de messagerie électronique sécurisée.</span><span class="sxs-lookup"><span data-stu-id="d3e91-166">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="d3e91-167">Pour cet exemple, créez *Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="d3e91-167">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="d3e91-168">Configurez des secrets utilisateur SendGrid</span><span class="sxs-lookup"><span data-stu-id="d3e91-168">Configure SendGrid user secrets</span></span>

<span data-ttu-id="d3e91-169">Ajouter une valeur unique `<UserSecretsId>` valeur pour le `<PropertyGroup>` élément du fichier projet :</span><span class="sxs-lookup"><span data-stu-id="d3e91-169">Add a unique `<UserSecretsId>` value to the `<PropertyGroup>` element of the project file:</span></span>

[!code-xml[](accconfirm/sample/WebPWrecover21/WebPWrecover.csproj?highlight=5)]

<span data-ttu-id="d3e91-170">Définissez `SendGridUser` et `SendGridKey` avec l'[outil Gestionnaire de secret](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="d3e91-170">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="d3e91-171">Exemple :</span><span class="sxs-lookup"><span data-stu-id="d3e91-171">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="d3e91-172">Sur Windows, le gestionnaire de secret stocke des paires de clés/valeur dans un fichier *secrets.json* dans le répertoire `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`.</span><span class="sxs-lookup"><span data-stu-id="d3e91-172">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="d3e91-173">Le contenu du fichier *secrets.json* n’est pas chiffré.</span><span class="sxs-lookup"><span data-stu-id="d3e91-173">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="d3e91-174">Le fichier *secrets.json* est présenté ci-dessous (la valeur `SendGridKey` a été supprimée.)</span><span class="sxs-lookup"><span data-stu-id="d3e91-174">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="install-sendgrid"></a><span data-ttu-id="d3e91-175">Installer SendGrid</span><span class="sxs-lookup"><span data-stu-id="d3e91-175">Install SendGrid</span></span>

<span data-ttu-id="d3e91-176">Ce didacticiel montre comment ajouter des notifications par courrier électronique via [SendGrid](https://sendgrid.com/), mais vous pouvez envoyer des e-mails en utilisant SMTP et d'autres mécanismes.</span><span class="sxs-lookup"><span data-stu-id="d3e91-176">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="d3e91-177">Installer le package NuGet `SendGrid` :</span><span class="sxs-lookup"><span data-stu-id="d3e91-177">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d3e91-178">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3e91-178">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="d3e91-179">À partir de la Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="d3e91-179">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d3e91-180">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="d3e91-180">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d3e91-181">À partir de la console, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="d3e91-181">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

------

<span data-ttu-id="d3e91-182">Consultez [Débuter avec SendGrid gratuitement](https://sendgrid.com/free/) pour vous inscrire pour un compte SendGrid gratuit.</span><span class="sxs-lookup"><span data-stu-id="d3e91-182">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>
### <a name="implement-iemailsender"></a><span data-ttu-id="d3e91-183">Implémenter IEmailSender</span><span class="sxs-lookup"><span data-stu-id="d3e91-183">Implement IEmailSender</span></span>

<span data-ttu-id="d3e91-184">L’implémentation `IEmailSender`, créer *Services/EmailSender.cs* avec un code similaire à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="d3e91-184">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="d3e91-185">Configurer le démarrage pour prendre en charge de messagerie</span><span class="sxs-lookup"><span data-stu-id="d3e91-185">Configure startup to support email</span></span>

<span data-ttu-id="d3e91-186">Ajoutez le code suivant à la `ConfigureServices` méthode dans le *Startup.cs* fichier :</span><span class="sxs-lookup"><span data-stu-id="d3e91-186">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="d3e91-187">Ajouter `EmailSender` comme un service singleton.</span><span class="sxs-lookup"><span data-stu-id="d3e91-187">Add `EmailSender` as a singleton service.</span></span>
* <span data-ttu-id="d3e91-188">Inscrire le `AuthMessageSenderOptions` instance de configuration.</span><span class="sxs-lookup"><span data-stu-id="d3e91-188">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Startup.cs?name=snippet2&highlight=12-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="d3e91-189">Activer la récupération de confirmation et le mot de passe du compte</span><span class="sxs-lookup"><span data-stu-id="d3e91-189">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="d3e91-190">Le modèle contient du code pour la confirmation du compte et la récupération de mot de passe.</span><span class="sxs-lookup"><span data-stu-id="d3e91-190">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="d3e91-191">Rechercher la `OnPostAsync` méthode dans *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="d3e91-191">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="d3e91-192">Empêchez les utilisateurs nouvellement inscrits d'être connectés automatiquement en plaçant la ligne suivante en commentaire :</span><span class="sxs-lookup"><span data-stu-id="d3e91-192">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="d3e91-193">La méthode complète est montrée avec la ligne modifiée mise en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="d3e91-193">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="d3e91-194">Inscrire, confirmer l’adresse e-mail et réinitialiser le mot de passe</span><span class="sxs-lookup"><span data-stu-id="d3e91-194">Register, confirm email, and reset password</span></span>

<span data-ttu-id="d3e91-195">Exécuter l’application web, et testez le flux de confirmation du compte et de récupération du mot de passe.</span><span class="sxs-lookup"><span data-stu-id="d3e91-195">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="d3e91-196">Exécutez l’application et inscrire un nouvel utilisateur</span><span class="sxs-lookup"><span data-stu-id="d3e91-196">Run the app and register a new user</span></span>

  ![Affichage du livre de comptes application Web](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="d3e91-198">Recherchez le lien de confirmation du compte dans votre messagerie.</span><span class="sxs-lookup"><span data-stu-id="d3e91-198">Check your email for the account confirmation link.</span></span> <span data-ttu-id="d3e91-199">Consultez [Déboguer la messagerie](#debug) si vous ne recevez pas l’e-mail.</span><span class="sxs-lookup"><span data-stu-id="d3e91-199">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="d3e91-200">Cliquez sur le lien pour confirmer votre adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="d3e91-200">Click the link to confirm your email.</span></span>
* <span data-ttu-id="d3e91-201">Connectez-vous à votre e-mail et un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="d3e91-201">Log in with your email and password.</span></span>
* <span data-ttu-id="d3e91-202">Fermez la session.</span><span class="sxs-lookup"><span data-stu-id="d3e91-202">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="d3e91-203">Afficher la page de gestion</span><span class="sxs-lookup"><span data-stu-id="d3e91-203">View the manage page</span></span>

<span data-ttu-id="d3e91-204">Sélectionnez votre nom d’utilisateur dans le navigateur : ![fenêtre de navigateur avec le nom d’utilisateur](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="d3e91-204">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="d3e91-205">Vous devrez peut-être développer la barre de navigation pour afficher le nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d3e91-205">You might need to expand the navbar to see user name.</span></span>

![barre de navigation](accconfirm/_static/x.png)

<span data-ttu-id="d3e91-207">La page de gestion s’affiche avec l'onglet **profil** sélectionné.</span><span class="sxs-lookup"><span data-stu-id="d3e91-207">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="d3e91-208">L'**Email** affiche une case à cocher indiquant que l’adresse de messagerie a été confirmée.</span><span class="sxs-lookup"><span data-stu-id="d3e91-208">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="d3e91-209">Tester la réinitialisation du mot de passe</span><span class="sxs-lookup"><span data-stu-id="d3e91-209">Test password reset</span></span>

* <span data-ttu-id="d3e91-210">Si vous êtes connecté, sélectionnez **Déconnexion**.</span><span class="sxs-lookup"><span data-stu-id="d3e91-210">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="d3e91-211">Sélectionnez le lien **Connexion** et sélectionnez le lien **Vous avez oublié votre mot de passe ?**.</span><span class="sxs-lookup"><span data-stu-id="d3e91-211">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="d3e91-212">Entrez l’adresse e-mail que vous avez utilisé pour inscrire le compte.</span><span class="sxs-lookup"><span data-stu-id="d3e91-212">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="d3e91-213">Un e-mail avec un lien pour réinitialiser votre mot de passe est envoyé.</span><span class="sxs-lookup"><span data-stu-id="d3e91-213">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="d3e91-214">Vérifier votre adresse e-mail et cliquez sur le lien pour réinitialiser votre mot de passe.</span><span class="sxs-lookup"><span data-stu-id="d3e91-214">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="d3e91-215">Une fois que votre mot de passe a été réinitialisé avec succès, vous pouvez vous connecter avec votre e-mail et votre nouveau mot de passe.</span><span class="sxs-lookup"><span data-stu-id="d3e91-215">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="d3e91-216">Déboguer le courrier électronique</span><span class="sxs-lookup"><span data-stu-id="d3e91-216">Debug email</span></span>

<span data-ttu-id="d3e91-217">Si vous ne parvenez à faire fonctionner l'email :</span><span class="sxs-lookup"><span data-stu-id="d3e91-217">If you can't get email working:</span></span>

* <span data-ttu-id="d3e91-218">Définir un point d’arrêt dans `EmailSender.Execute` pour vérifier `SendGridClient.SendEmailAsync` est appelée.</span><span class="sxs-lookup"><span data-stu-id="d3e91-218">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="d3e91-219">Créer un [application console pour envoyer un e-mail](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) à l’aide d’un code similaire à `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="d3e91-219">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="d3e91-220">Examinez la page [Activité de la messagerie](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span><span class="sxs-lookup"><span data-stu-id="d3e91-220">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="d3e91-221">Vérifiez votre dossier courrier indésirable.</span><span class="sxs-lookup"><span data-stu-id="d3e91-221">Check your spam folder.</span></span>
* <span data-ttu-id="d3e91-222">Essayez un autre alias de messagerie sur un fournisseur de messagerie différents (Microsoft, Yahoo, Gmail, etc.).</span><span class="sxs-lookup"><span data-stu-id="d3e91-222">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="d3e91-223">Essayer d’envoyer à différents comptes e-mail.</span><span class="sxs-lookup"><span data-stu-id="d3e91-223">Try sending to different email accounts.</span></span>

<span data-ttu-id="d3e91-224">Une **bonne pratique de sécurité** consiste à **ne pas** utiliser des secrets de production en développement et en test.</span><span class="sxs-lookup"><span data-stu-id="d3e91-224">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="d3e91-225">Si vous publiez l’application sur Azure, vous pouvez définir les secrets SendGrid en tant que paramètres d’application dans le portail Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="d3e91-225">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="d3e91-226">Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="d3e91-226">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="d3e91-227">Combiner des comptes de connexion de réseaux sociaux et local</span><span class="sxs-lookup"><span data-stu-id="d3e91-227">Combine social and local login accounts</span></span>

<span data-ttu-id="d3e91-228">Pour terminer cette section, vous devez d’abord activer un fournisseur d’authentification externe.</span><span class="sxs-lookup"><span data-stu-id="d3e91-228">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="d3e91-229">Consultez [Facebook, Google et l’authentification du fournisseur externe](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="d3e91-229">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="d3e91-230">Vous pouvez combiner des comptes locaux et de réseaux sociaux en cliquant sur le lien de votre e-mail.</span><span class="sxs-lookup"><span data-stu-id="d3e91-230">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="d3e91-231">Dans la séquence suivante, "RickAndMSFT@gmail.com" est d’abord créé en tant que connexion locale ; cependant, vous pouvez d’abord créer le compte en tant que connexion de réseau social, puis ajouter une connexion locale.</span><span class="sxs-lookup"><span data-stu-id="d3e91-231">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Application Web : RickAndMSFT@gmail.com utilisateur authentifié](accconfirm/_static/rick.png)

<span data-ttu-id="d3e91-233">Cliquez sur le lien **Gérer**.</span><span class="sxs-lookup"><span data-stu-id="d3e91-233">Click on the **Manage** link.</span></span> <span data-ttu-id="d3e91-234">Notez la valeur 0 externes (connexions sociales) associé à ce compte.</span><span class="sxs-lookup"><span data-stu-id="d3e91-234">Note the 0 external (social logins) associated with this account.</span></span>

![Gérer la vue](accconfirm/_static/manage.png)

<span data-ttu-id="d3e91-236">Cliquez sur le lien vers un autre service de connexion et acceptez les demandes d’application.</span><span class="sxs-lookup"><span data-stu-id="d3e91-236">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="d3e91-237">Dans l’image suivante, Facebook est le fournisseur d’authentification externe :</span><span class="sxs-lookup"><span data-stu-id="d3e91-237">In the following image, Facebook is the external authentication provider:</span></span>

![Gérer votre vue de logins externes répertoriant Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="d3e91-239">Les deux comptes ont été combinés.</span><span class="sxs-lookup"><span data-stu-id="d3e91-239">The two accounts have been combined.</span></span> <span data-ttu-id="d3e91-240">Vous pouvez vous connecter avec l'un ou l'autre compte.</span><span class="sxs-lookup"><span data-stu-id="d3e91-240">You are able to log on with either account.</span></span> <span data-ttu-id="d3e91-241">Vous pouvez faire en sorte que vos utilisateurs ajoutent des comptes locaux au cas où leur service d’authentification de connexion sociale soit indisponible ou que, plus probablement, ils aient perdu l’accès à leur compte social.</span><span class="sxs-lookup"><span data-stu-id="d3e91-241">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="d3e91-242">Activer la confirmation de compte après qu’un site a des utilisateurs</span><span class="sxs-lookup"><span data-stu-id="d3e91-242">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="d3e91-243">Activer la confirmation du compte sur un site avec des utilisateurs bloque tous les utilisateurs existants.</span><span class="sxs-lookup"><span data-stu-id="d3e91-243">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="d3e91-244">Ceux-ci sont bloqués, car leurs comptes ne sont pas confirmés.</span><span class="sxs-lookup"><span data-stu-id="d3e91-244">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="d3e91-245">Pour contourner le blocage des utilisateurs existants, utilisez une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="d3e91-245">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="d3e91-246">Mise à jour de la base de données pour marquer tous les utilisateurs existants comme étant confirmés.</span><span class="sxs-lookup"><span data-stu-id="d3e91-246">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="d3e91-247">Confirmation des utilisateurs existants.</span><span class="sxs-lookup"><span data-stu-id="d3e91-247">Confirm exiting users.</span></span> <span data-ttu-id="d3e91-248">Par exemple, en envoyant par lot des e-mails avec des liens de confirmation.</span><span class="sxs-lookup"><span data-stu-id="d3e91-248">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
