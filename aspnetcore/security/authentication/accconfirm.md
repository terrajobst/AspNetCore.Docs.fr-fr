---
title: Confirmation de compte et récupération de mot de passe dans ASP.NET Core
author: rick-anderson
description: Découvrez comment créer une application ASP.NET Core avec une confirmation par e-mail et une réinitialisation du mot de passe.
ms.author: riande
ms.date: 03/11/2019
uid: security/authentication/accconfirm
ms.openlocfilehash: a4ecc2d91fb72915703dfaa146260f0c1360bded
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880766"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="d8ca0-103">Confirmation de compte et récupération de mot de passe dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d8ca0-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="d8ca0-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant)et [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="d8ca0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="d8ca0-105">Ce didacticiel montre comment créer une application ASP.NET Core avec une confirmation par e-mail et une réinitialisation du mot de passe.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-105">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="d8ca0-106">Ce didacticiel n'est **pas** un sujet pour débuter.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="d8ca0-107">Vous devez être familiarisé avec :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-107">You should be familiar with:</span></span>

* [<span data-ttu-id="d8ca0-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d8ca0-108">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="d8ca0-109">Authentification</span><span class="sxs-lookup"><span data-stu-id="d8ca0-109">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="d8ca0-110">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="d8ca0-110">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="d8ca0-111">Consultez [ce fichier PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) pour obtenir la version ASP.net Core 1,1.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-111">See [this PDF file](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 version.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

## <a name="prerequisites"></a><span data-ttu-id="d8ca0-112">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="d8ca0-112">Prerequisites</span></span>

[<span data-ttu-id="d8ca0-113">.NET Core 3,0 SDK ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="d8ca0-113">.NET Core 3.0 SDK or later</span></span>](https://dotnet.microsoft.com/download/dotnet-core/3.0)

## <a name="create-and-test-a-web-app-with-authentication"></a><span data-ttu-id="d8ca0-114">Créer et tester une application Web avec l’authentification</span><span class="sxs-lookup"><span data-stu-id="d8ca0-114">Create and test a web app with authentication</span></span>

<span data-ttu-id="d8ca0-115">Exécutez les commandes suivantes pour créer une application Web avec l’authentification.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-115">Run the following commands to create a web app with authentication.</span></span>

```dotnetcli
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet run
```

<span data-ttu-id="d8ca0-116">Exécutez l’application, sélectionnez le lien **S'inscrire** et inscrivez un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-116">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="d8ca0-117">Une fois inscrit, vous êtes redirigé vers la page à `/Identity/Account/RegisterConfirmation` qui contient un lien pour simuler la confirmation de l’e-mail :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-117">Once registered, you are redirected to the to `/Identity/Account/RegisterConfirmation` page which contains a link to simulate email confirmation:</span></span>

* <span data-ttu-id="d8ca0-118">Sélectionnez le lien `Click here to confirm your account`.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-118">Select the `Click here to confirm your account` link.</span></span>
* <span data-ttu-id="d8ca0-119">Sélectionnez le lien de **connexion** et connectez-vous avec les mêmes informations d’identification.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-119">Select the **Login** link and sign-in with the same credentials.</span></span>
* <span data-ttu-id="d8ca0-120">Sélectionnez le lien `Hello YourEmail@provider.com!`, qui vous redirige vers la page `/Identity/Account/Manage/PersonalData`.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-120">Select the `Hello YourEmail@provider.com!` link, which redirects you to the `/Identity/Account/Manage/PersonalData` page.</span></span>
* <span data-ttu-id="d8ca0-121">Sélectionnez l’onglet **données personnelles** sur la gauche, puis sélectionnez **supprimer**.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-121">Select the **Personal data** tab on the left, and then select **Delete**.</span></span>

### <a name="configure-an-email-provider"></a><span data-ttu-id="d8ca0-122">Configurer un fournisseur de messagerie</span><span class="sxs-lookup"><span data-stu-id="d8ca0-122">Configure an email provider</span></span>

<span data-ttu-id="d8ca0-123">Dans ce didacticiel, [SendGrid](https://sendgrid.com) est utilisé pour envoyer des messages électroniques.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-123">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="d8ca0-124">Vous avez besoin d’un compte SendGrid et une clé pour envoyer un courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-124">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="d8ca0-125">Vous pouvez utiliser d’autres fournisseurs de messagerie.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-125">You can use other email providers.</span></span> <span data-ttu-id="d8ca0-126">Nous vous recommandons d'utiliser SendGrid ou un autre service de messagerie pour envoyer un courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-126">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="d8ca0-127">SMTP est difficile à sécuriser et à correctement configurer.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-127">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="d8ca0-128">Créez une classe pour extraire la clé de messagerie électronique sécurisée.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-128">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="d8ca0-129">Pour cet exemple, créez *services/AuthMessageSenderOptions. cs*:</span><span class="sxs-lookup"><span data-stu-id="d8ca0-129">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="d8ca0-130">Configurer des secrets d’utilisateur SendGrid</span><span class="sxs-lookup"><span data-stu-id="d8ca0-130">Configure SendGrid user secrets</span></span>

<span data-ttu-id="d8ca0-131">Définissez `SendGridUser` et `SendGridKey` avec l'[outil Gestionnaire de secret](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="d8ca0-131">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="d8ca0-132">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-132">For example:</span></span>

```dotnetcli
dotnet user-secrets set SendGridUser RickAndMSFT
dotnet user-secrets set SendGridKey <key>

Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="d8ca0-133">Sur Windows, le gestionnaire de secret stocke des paires de clés/valeur dans un fichier *secrets.json* dans le répertoire `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-133">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="d8ca0-134">Le contenu du fichier *secrets.json* n’est pas chiffré.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-134">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="d8ca0-135">Le balisage suivant montre le fichier *secrets. JSON* .</span><span class="sxs-lookup"><span data-stu-id="d8ca0-135">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="d8ca0-136">La valeur de `SendGridKey` a été supprimée.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-136">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="d8ca0-137">Pour plus d’informations, consultez le [modèle d’options](xref:fundamentals/configuration/options) et la [configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="d8ca0-137">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="d8ca0-138">Installer SendGrid</span><span class="sxs-lookup"><span data-stu-id="d8ca0-138">Install SendGrid</span></span>

<span data-ttu-id="d8ca0-139">Ce didacticiel montre comment ajouter des notifications par courrier électronique via [SendGrid](https://sendgrid.com/), mais vous pouvez envoyer des e-mails en utilisant SMTP et d'autres mécanismes.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-139">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="d8ca0-140">Installer le package NuGet `SendGrid` :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-140">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d8ca0-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d8ca0-141">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d8ca0-142">À partir de la console du gestionnaire de package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-142">From the Package Manager Console, enter the following command:</span></span>

```powershell
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d8ca0-143">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="d8ca0-143">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d8ca0-144">À partir de la console, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-144">From the console, enter the following command:</span></span>

```dotnetcli
dotnet add package SendGrid
```

---

<span data-ttu-id="d8ca0-145">Consultez [Débuter avec SendGrid gratuitement](https://sendgrid.com/free/) pour vous inscrire pour un compte SendGrid gratuit.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-145">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="d8ca0-146">Implémenter IEmailSender</span><span class="sxs-lookup"><span data-stu-id="d8ca0-146">Implement IEmailSender</span></span>

<span data-ttu-id="d8ca0-147">Pour implémenter `IEmailSender`, créez *services/EMailSender. cs* avec un code similaire à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-147">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="d8ca0-148">Configurer le démarrage pour la prise en charge de la messagerie</span><span class="sxs-lookup"><span data-stu-id="d8ca0-148">Configure startup to support email</span></span>

<span data-ttu-id="d8ca0-149">Ajoutez le code suivant à la méthode `ConfigureServices` dans le fichier *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-149">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="d8ca0-150">Ajoutez `EmailSender` en tant que service temporaire.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-150">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="d8ca0-151">Inscrivez l’instance de configuration `AuthMessageSenderOptions`.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-151">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/Startup.cs?name=snippet1&highlight=11-15)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="d8ca0-152">Inscrire, confirmer l’adresse e-mail et réinitialiser le mot de passe</span><span class="sxs-lookup"><span data-stu-id="d8ca0-152">Register, confirm email, and reset password</span></span>

<span data-ttu-id="d8ca0-153">Exécuter l’application web, et testez le flux de confirmation du compte et de récupération du mot de passe.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-153">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="d8ca0-154">Exécutez l’application et inscrire un nouvel utilisateur</span><span class="sxs-lookup"><span data-stu-id="d8ca0-154">Run the app and register a new user</span></span>
* <span data-ttu-id="d8ca0-155">Recherchez le lien de confirmation du compte dans votre messagerie.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-155">Check your email for the account confirmation link.</span></span> <span data-ttu-id="d8ca0-156">Consultez [Déboguer la messagerie](#debug) si vous ne recevez pas l’e-mail.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-156">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="d8ca0-157">Cliquez sur le lien pour confirmer votre adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-157">Click the link to confirm your email.</span></span>
* <span data-ttu-id="d8ca0-158">Connectez-vous avec votre adresse de messagerie et votre mot de passe.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-158">Sign in with your email and password.</span></span>
* <span data-ttu-id="d8ca0-159">Déconnectez-vous.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-159">Sign out.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="d8ca0-160">Tester la réinitialisation du mot de passe</span><span class="sxs-lookup"><span data-stu-id="d8ca0-160">Test password reset</span></span>

* <span data-ttu-id="d8ca0-161">Si vous êtes connecté, sélectionnez **déconnexion**.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-161">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="d8ca0-162">Sélectionnez le lien **Connexion** et sélectionnez le lien **Vous avez oublié votre mot de passe ?** .</span><span class="sxs-lookup"><span data-stu-id="d8ca0-162">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="d8ca0-163">Entrez l’adresse de messagerie que vous avez utilisée pour inscrire le compte.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-163">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="d8ca0-164">Un e-mail contenant un lien pour réinitialiser votre mot de passe est envoyé.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-164">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="d8ca0-165">Vérifiez votre adresse de messagerie, puis cliquez sur le lien pour réinitialiser votre mot de passe.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-165">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="d8ca0-166">Une fois que votre mot de passe a été réinitialisé avec succès, vous pouvez vous connecter avec votre adresse de messagerie et votre nouveau mot de passe.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-166">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="d8ca0-167">Modifier le délai d’expiration de l’e-mail et de l’activité</span><span class="sxs-lookup"><span data-stu-id="d8ca0-167">Change email and activity timeout</span></span>

<span data-ttu-id="d8ca0-168">Le délai d’inactivité par défaut est de 14 jours.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-168">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="d8ca0-169">Le code suivant définit le délai d’expiration d’inactivité sur 5 jours :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-169">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="d8ca0-170">Modifier toutes les durées de vie des jetons de protection des données</span><span class="sxs-lookup"><span data-stu-id="d8ca0-170">Change all data protection token lifespans</span></span>

<span data-ttu-id="d8ca0-171">Le code suivant modifie le délai d’expiration de tous les jetons de protection des données à 3 heures :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-171">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupAllTokens.cs?name=snippet1&highlight=11-12)]

<span data-ttu-id="d8ca0-172">Les jetons utilisateur d’identité intégrés (consultez [AspNetCore/SRC/Identity/extensions. Core/SRC/TokenOptions. cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) ont un [délai d’expiration d’un jour](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="d8ca0-172">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="d8ca0-173">Modifier la durée de vie du jeton d’e-mail</span><span class="sxs-lookup"><span data-stu-id="d8ca0-173">Change the email token lifespan</span></span>

<span data-ttu-id="d8ca0-174">La durée de vie des jetons par défaut des [jetons d’utilisateur d’identité](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) est d' [un jour](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="d8ca0-174">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="d8ca0-175">Cette section montre comment modifier la durée de vie des jetons de courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-175">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="d8ca0-176">Ajoutez un [DataProtectorTokenProvider personnalisé\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) et <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="d8ca0-176">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="d8ca0-177">Ajoutez le fournisseur personnalisé au conteneur de service :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-177">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover30/StartupEmail.cs?name=snippet1&highlight=10-16)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="d8ca0-178">Renvoyer l’e-mail de confirmation</span><span class="sxs-lookup"><span data-stu-id="d8ca0-178">Resend email confirmation</span></span>

<span data-ttu-id="d8ca0-179">Consultez [ce problème GitHub](https://github.com/aspnet/AspNetCore/issues/5410).</span><span class="sxs-lookup"><span data-stu-id="d8ca0-179">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="d8ca0-180">Message de débogage</span><span class="sxs-lookup"><span data-stu-id="d8ca0-180">Debug email</span></span>

<span data-ttu-id="d8ca0-181">Si vous ne parvenez à faire fonctionner l'email :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-181">If you can't get email working:</span></span>

* <span data-ttu-id="d8ca0-182">Définissez un point d’arrêt dans `EmailSender.Execute` pour vérifier que `SendGridClient.SendEmailAsync` est appelé.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-182">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="d8ca0-183">Créez une [application console pour envoyer du courrier électronique](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) à l’aide d’un code similaire à `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-183">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="d8ca0-184">Examinez la page [Activité de la messagerie](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-184">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="d8ca0-185">Vérifiez votre dossier de courrier indésirable.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-185">Check your spam folder.</span></span>
* <span data-ttu-id="d8ca0-186">Essayez un autre alias de messagerie sur un autre fournisseur de messagerie (Microsoft, Yahoo, Gmail, etc.)</span><span class="sxs-lookup"><span data-stu-id="d8ca0-186">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="d8ca0-187">Essayez d’envoyer à différents comptes de messagerie.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-187">Try sending to different email accounts.</span></span>

<span data-ttu-id="d8ca0-188">Une **bonne pratique de sécurité** consiste à **ne pas** utiliser des secrets de production en développement et en test.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-188">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="d8ca0-189">Si vous publiez l’application sur Azure, définissez les secrets SendGrid en tant que paramètres d’application dans le portail d’application Web Azure.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-189">If you publish the app to Azure, set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="d8ca0-190">Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-190">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="d8ca0-191">Combiner les comptes de connexion sociale et locale</span><span class="sxs-lookup"><span data-stu-id="d8ca0-191">Combine social and local login accounts</span></span>

<span data-ttu-id="d8ca0-192">Pour compléter cette section, vous devez d’abord activer un fournisseur d’authentification externe.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-192">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="d8ca0-193">Consultez [l’authentification Facebook, Google et fournisseur externe](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="d8ca0-193">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="d8ca0-194">Vous pouvez combiner des comptes locaux et de réseaux sociaux en cliquant sur le lien de votre e-mail.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-194">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="d8ca0-195">Dans la séquence suivante, "RickAndMSFT@gmail.com" est d’abord créé en tant que connexion locale ; cependant, vous pouvez d’abord créer le compte en tant que connexion de réseau social, puis ajouter une connexion locale.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-195">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Application Web : RickAndMSFT@gmail.com utilisateur authentifié](accconfirm/_static/rick.png)

<span data-ttu-id="d8ca0-197">Cliquez sur le lien **Gérer**.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-197">Click on the **Manage** link.</span></span> <span data-ttu-id="d8ca0-198">Notez la valeur 0 externe (connexions sociales) associée à ce compte.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-198">Note the 0 external (social logins) associated with this account.</span></span>

![Gérer l’affichage](accconfirm/_static/manage.png)

<span data-ttu-id="d8ca0-200">Cliquez sur le lien vers un autre service de connexion et acceptez les demandes d’application.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-200">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="d8ca0-201">Dans l’image suivante, Facebook est le fournisseur d’authentification externe :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-201">In the following image, Facebook is the external authentication provider:</span></span>

![Gérer l’affichage de vos connexions externes à l’objet Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="d8ca0-203">Les deux comptes ont été combinés.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-203">The two accounts have been combined.</span></span> <span data-ttu-id="d8ca0-204">Vous pouvez vous connecter avec l’un ou l’autre de ces comptes.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-204">You are able to sign in with either account.</span></span> <span data-ttu-id="d8ca0-205">Vous pouvez faire en sorte que vos utilisateurs ajoutent des comptes locaux au cas où leur service d’authentification de connexion sociale soit indisponible ou que, plus probablement, ils aient perdu l’accès à leur compte social.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-205">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="d8ca0-206">Activer la confirmation du compte une fois qu’un site a des utilisateurs</span><span class="sxs-lookup"><span data-stu-id="d8ca0-206">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="d8ca0-207">Activer la confirmation du compte sur un site avec des utilisateurs bloque tous les utilisateurs existants.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-207">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="d8ca0-208">Ceux-ci sont bloqués, car leurs comptes ne sont pas confirmés.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-208">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="d8ca0-209">Pour contourner le blocage des utilisateurs existants, utilisez une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-209">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="d8ca0-210">Mise à jour de la base de données pour marquer tous les utilisateurs existants comme étant confirmés.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-210">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="d8ca0-211">Confirmez les utilisateurs existants.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-211">Confirm existing users.</span></span> <span data-ttu-id="d8ca0-212">Par exemple, en envoyant par lot des e-mails avec des liens de confirmation.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-212">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0 < aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="d8ca0-213">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="d8ca0-213">Prerequisites</span></span>

[<span data-ttu-id="d8ca0-214">.NET Core 2,2 SDK ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="d8ca0-214">.NET Core 2.2 SDK or later</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="d8ca0-215">Créer une application Web et une identité de structure</span><span class="sxs-lookup"><span data-stu-id="d8ca0-215">Create a web  app and scaffold Identity</span></span>

<span data-ttu-id="d8ca0-216">Exécutez les commandes suivantes pour créer une application Web avec l’authentification.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-216">Run the following commands to create a web app with authentication.</span></span>

```dotnetcli
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator identity -dc WebPWrecover.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout;Account.ConfirmEmail"
dotnet ef database drop -f
dotnet ef database update
dotnet run

```

## <a name="test-new-user-registration"></a><span data-ttu-id="d8ca0-217">Tester l’inscription d’un nouvel utilisateur</span><span class="sxs-lookup"><span data-stu-id="d8ca0-217">Test new user registration</span></span>

<span data-ttu-id="d8ca0-218">Exécutez l’application, sélectionnez le lien **S'inscrire** et inscrivez un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-218">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="d8ca0-219">À ce stade, la seule validation sur l’e-mail est avec l’attribut [`[EmailAddress]`](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) .</span><span class="sxs-lookup"><span data-stu-id="d8ca0-219">At this point, the only validation on the email is with the [`[EmailAddress]`](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="d8ca0-220">Après avoir soumis l’inscription, vous êtes connecté à l’application.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-220">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="d8ca0-221">Plus loin dans ce didacticiel, le code est mis à jour afin que les nouveaux utilisateurs ne puissent pas se connecter tant que leur adresse de messagerie n’a pas été validée.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-221">Later in the tutorial, the code is updated so new users can't sign in until their email is validated.</span></span>

[!INCLUDE[](~/includes/view-identity-db.md)]

<span data-ttu-id="d8ca0-222">Notez que le champ `EmailConfirmed` de la table est à `False`.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-222">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="d8ca0-223">Vous pouvez souhaiter réutiliser cet e-mail à l’étape suivante quand l’application envoie un e-mail de confirmation.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-223">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="d8ca0-224">Cliquez avec le bouton droit sur la ligne, puis sélectionnez **Supprimer**.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-224">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="d8ca0-225">La suppression de l’alias d’e-mail facilite les étapes suivantes.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-225">Deleting the email alias makes it easier in the following steps.</span></span>

<a name="prevent-login-at-registration"></a>

## <a name="require-email-confirmation"></a><span data-ttu-id="d8ca0-226">Demander une confirmation par courrier électronique</span><span class="sxs-lookup"><span data-stu-id="d8ca0-226">Require email confirmation</span></span>

<span data-ttu-id="d8ca0-227">Il est recommandé de confirmer l’adresse de messagerie d’un nouvel enregistrement d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-227">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="d8ca0-228">Envoyer un courrier électronique de confirmation permet de vérifier qu’ils n'empruntent pas l'identité d'une autre personne (autrement dit, ils ne se sont pas inscrits avec l'adresse de messageriedquelqu'un d’autre).</span><span class="sxs-lookup"><span data-stu-id="d8ca0-228">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="d8ca0-229">Supposons que vous ayez un forum de discussion, et que vous souhaitiez empêcher "yli@example.com" de s’inscription en tant que "nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="d8ca0-229">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="d8ca0-230">Sans la confirmation par courrier électronique, "nolivetto@contoso.com" peut recevoir un e-mail indésirable de votre application.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-230">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="d8ca0-231">Supposons que l’utilisateur s'inscrit par inadvertance en tant que "ylo@example.com" et que vous n’avez pas remarqué la faute d’orthographe de "yli".</span><span class="sxs-lookup"><span data-stu-id="d8ca0-231">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="d8ca0-232">Ils ne pourraient pas utiliser la récupération de mot de passe, car l’application n’a pas leur courrier électronique correct.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-232">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="d8ca0-233">La confirmation par e-mail fournit une protection limitée des robots.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-233">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="d8ca0-234">L'email de confirmation ne fournit pas une protection contre les utilisateurs malveillants avec plusieurs comptes de messagerie.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-234">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="d8ca0-235">En règle générale, vous souhaitez empêcher les nouveaux utilisateurs d'envoyer des données à votre site web tant que leur adresse e-mail n’est pas confirmée.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-235">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="d8ca0-236">Mettre à jour `Startup.ConfigureServices` pour exiger un e-mail confirmé :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-236">Update `Startup.ConfigureServices`  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=8-11)]

<span data-ttu-id="d8ca0-237">`config.SignIn.RequireConfirmedEmail = true;` empêche les utilisateurs enregistrés de se connecter jusqu'à ce que leur adresse de messagerie soit confirmée.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-237">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="d8ca0-238">Configurer le fournisseur de messagerie</span><span class="sxs-lookup"><span data-stu-id="d8ca0-238">Configure email provider</span></span>

<span data-ttu-id="d8ca0-239">Dans ce didacticiel, [SendGrid](https://sendgrid.com) est utilisé pour envoyer des messages électroniques.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-239">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="d8ca0-240">Vous avez besoin d’un compte SendGrid et une clé pour envoyer un courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-240">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="d8ca0-241">Vous pouvez utiliser d’autres fournisseurs de messagerie.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-241">You can use other email providers.</span></span> <span data-ttu-id="d8ca0-242">ASP.NET Core 2.x inclut `System.Net.Mail`, qui vous permet d’envoyer par courrier électronique à partir de votre application.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-242">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="d8ca0-243">Nous vous recommandons d'utiliser SendGrid ou un autre service de messagerie pour envoyer un courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-243">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="d8ca0-244">SMTP est difficile à sécuriser et à correctement configurer.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-244">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="d8ca0-245">Créez une classe pour extraire la clé de messagerie électronique sécurisée.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-245">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="d8ca0-246">Pour cet exemple, créez *services/AuthMessageSenderOptions. cs*:</span><span class="sxs-lookup"><span data-stu-id="d8ca0-246">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="d8ca0-247">Configurer des secrets d’utilisateur SendGrid</span><span class="sxs-lookup"><span data-stu-id="d8ca0-247">Configure SendGrid user secrets</span></span>

<span data-ttu-id="d8ca0-248">Définissez `SendGridUser` et `SendGridKey` avec l'[outil Gestionnaire de secret](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="d8ca0-248">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="d8ca0-249">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-249">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="d8ca0-250">Sur Windows, le gestionnaire de secret stocke des paires de clés/valeur dans un fichier *secrets.json* dans le répertoire `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-250">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="d8ca0-251">Le contenu du fichier *secrets.json* n’est pas chiffré.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-251">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="d8ca0-252">Le balisage suivant montre le fichier *secrets. JSON* .</span><span class="sxs-lookup"><span data-stu-id="d8ca0-252">The following markup shows the *secrets.json* file.</span></span> <span data-ttu-id="d8ca0-253">La valeur de `SendGridKey` a été supprimée.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-253">The `SendGridKey` value has been removed.</span></span>

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

<span data-ttu-id="d8ca0-254">Pour plus d’informations, consultez le [modèle d’options](xref:fundamentals/configuration/options) et la [configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="d8ca0-254">For more information, see the [Options pattern](xref:fundamentals/configuration/options) and [configuration](xref:fundamentals/configuration/index).</span></span>

### <a name="install-sendgrid"></a><span data-ttu-id="d8ca0-255">Installer SendGrid</span><span class="sxs-lookup"><span data-stu-id="d8ca0-255">Install SendGrid</span></span>

<span data-ttu-id="d8ca0-256">Ce didacticiel montre comment ajouter des notifications par courrier électronique via [SendGrid](https://sendgrid.com/), mais vous pouvez envoyer des e-mails en utilisant SMTP et d'autres mécanismes.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-256">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="d8ca0-257">Installer le package NuGet `SendGrid` :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-257">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d8ca0-258">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d8ca0-258">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="d8ca0-259">À partir de la console du gestionnaire de package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-259">From the Package Manager Console, enter the following command:</span></span>

```powershell
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d8ca0-260">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="d8ca0-260">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d8ca0-261">À partir de la console, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-261">From the console, enter the following command:</span></span>

```dotnetcli
dotnet add package SendGrid
```

---

<span data-ttu-id="d8ca0-262">Consultez [Débuter avec SendGrid gratuitement](https://sendgrid.com/free/) pour vous inscrire pour un compte SendGrid gratuit.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-262">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

### <a name="implement-iemailsender"></a><span data-ttu-id="d8ca0-263">Implémenter IEmailSender</span><span class="sxs-lookup"><span data-stu-id="d8ca0-263">Implement IEmailSender</span></span>

<span data-ttu-id="d8ca0-264">Pour implémenter `IEmailSender`, créez *services/EMailSender. cs* avec un code similaire à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-264">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="d8ca0-265">Configurer le démarrage pour la prise en charge de la messagerie</span><span class="sxs-lookup"><span data-stu-id="d8ca0-265">Configure startup to support email</span></span>

<span data-ttu-id="d8ca0-266">Ajoutez le code suivant à la méthode `ConfigureServices` dans le fichier *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-266">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="d8ca0-267">Ajoutez `EmailSender` en tant que service temporaire.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-267">Add `EmailSender` as a transient service.</span></span>
* <span data-ttu-id="d8ca0-268">Inscrivez l’instance de configuration `AuthMessageSenderOptions`.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-268">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=15-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="d8ca0-269">Activer la récupération du mot de passe et la confirmation du compte</span><span class="sxs-lookup"><span data-stu-id="d8ca0-269">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="d8ca0-270">Le modèle contient du code pour la confirmation du compte et la récupération de mot de passe.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-270">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="d8ca0-271">Recherchez la méthode `OnPostAsync` dans *Areas/Identity/pages/Account/Register. cshtml. cs*.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-271">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="d8ca0-272">Empêchez les utilisateurs nouvellement inscrits d’être automatiquement connectés en commentant la ligne suivante :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-272">Prevent newly registered users from being automatically signed in by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="d8ca0-273">La méthode complète est montrée avec la ligne modifiée mise en surbrillance :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-273">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="d8ca0-274">Inscrire, confirmer l’adresse e-mail et réinitialiser le mot de passe</span><span class="sxs-lookup"><span data-stu-id="d8ca0-274">Register, confirm email, and reset password</span></span>

<span data-ttu-id="d8ca0-275">Exécuter l’application web, et testez le flux de confirmation du compte et de récupération du mot de passe.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-275">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="d8ca0-276">Exécutez l’application et inscrire un nouvel utilisateur</span><span class="sxs-lookup"><span data-stu-id="d8ca0-276">Run the app and register a new user</span></span>
* <span data-ttu-id="d8ca0-277">Recherchez le lien de confirmation du compte dans votre messagerie.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-277">Check your email for the account confirmation link.</span></span> <span data-ttu-id="d8ca0-278">Consultez [Déboguer la messagerie](#debug) si vous ne recevez pas l’e-mail.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-278">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="d8ca0-279">Cliquez sur le lien pour confirmer votre adresse de messagerie.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-279">Click the link to confirm your email.</span></span>
* <span data-ttu-id="d8ca0-280">Connectez-vous avec votre adresse de messagerie et votre mot de passe.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-280">Sign in with your email and password.</span></span>
* <span data-ttu-id="d8ca0-281">Déconnectez-vous.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-281">Sign out.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="d8ca0-282">Afficher la page gérer</span><span class="sxs-lookup"><span data-stu-id="d8ca0-282">View the manage page</span></span>

<span data-ttu-id="d8ca0-283">Sélectionnez votre nom d’utilisateur dans le navigateur : ![fenêtre du navigateur avec le nom d’utilisateur](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="d8ca0-283">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="d8ca0-284">La page de gestion s’affiche avec l'onglet **profil** sélectionné.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-284">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="d8ca0-285">L'**Email** affiche une case à cocher indiquant que l’adresse de messagerie a été confirmée.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-285">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="d8ca0-286">Tester la réinitialisation du mot de passe</span><span class="sxs-lookup"><span data-stu-id="d8ca0-286">Test password reset</span></span>

* <span data-ttu-id="d8ca0-287">Si vous êtes connecté, sélectionnez **déconnexion**.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-287">If you're signed in, select **Logout**.</span></span>
* <span data-ttu-id="d8ca0-288">Sélectionnez le lien **Connexion** et sélectionnez le lien **Vous avez oublié votre mot de passe ?** .</span><span class="sxs-lookup"><span data-stu-id="d8ca0-288">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="d8ca0-289">Entrez l’adresse de messagerie que vous avez utilisée pour inscrire le compte.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-289">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="d8ca0-290">Un e-mail contenant un lien pour réinitialiser votre mot de passe est envoyé.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-290">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="d8ca0-291">Vérifiez votre adresse de messagerie, puis cliquez sur le lien pour réinitialiser votre mot de passe.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-291">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="d8ca0-292">Une fois que votre mot de passe a été réinitialisé avec succès, vous pouvez vous connecter avec votre adresse de messagerie et votre nouveau mot de passe.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-292">After your password has been successfully reset, you can sign in with your email and new password.</span></span>

## <a name="change-email-and-activity-timeout"></a><span data-ttu-id="d8ca0-293">Modifier le délai d’expiration de l’e-mail et de l’activité</span><span class="sxs-lookup"><span data-stu-id="d8ca0-293">Change email and activity timeout</span></span>

<span data-ttu-id="d8ca0-294">Le délai d’inactivité par défaut est de 14 jours.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-294">The default inactivity timeout is 14 days.</span></span> <span data-ttu-id="d8ca0-295">Le code suivant définit le délai d’expiration d’inactivité sur 5 jours :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-295">The following code sets the inactivity timeout to 5 days:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a><span data-ttu-id="d8ca0-296">Modifier toutes les durées de vie des jetons de protection des données</span><span class="sxs-lookup"><span data-stu-id="d8ca0-296">Change all data protection token lifespans</span></span>

<span data-ttu-id="d8ca0-297">Le code suivant modifie le délai d’expiration de tous les jetons de protection des données à 3 heures :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-297">The following code changes all data protection tokens timeout period to 3 hours:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAllTokens.cs?name=snippet1&highlight=15-16)]

<span data-ttu-id="d8ca0-298">Les jetons utilisateur d’identité intégrés (consultez [AspNetCore/SRC/Identity/extensions. Core/SRC/TokenOptions. cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) ont un [délai d’expiration d’un jour](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="d8ca0-298">The built in Identity user tokens (see [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) )have a [one day timeout](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span>

### <a name="change-the-email-token-lifespan"></a><span data-ttu-id="d8ca0-299">Modifier la durée de vie du jeton d’e-mail</span><span class="sxs-lookup"><span data-stu-id="d8ca0-299">Change the email token lifespan</span></span>

<span data-ttu-id="d8ca0-300">La durée de vie des jetons par défaut des [jetons d’utilisateur d’identité](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) est d' [un jour](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="d8ca0-300">The default token lifespan of [the Identity user tokens](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) is [one day](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).</span></span> <span data-ttu-id="d8ca0-301">Cette section montre comment modifier la durée de vie des jetons de courrier électronique.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-301">This section shows how to change the email token lifespan.</span></span>

<span data-ttu-id="d8ca0-302">Ajoutez un [DataProtectorTokenProvider personnalisé\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) et <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span><span class="sxs-lookup"><span data-stu-id="d8ca0-302">Add a custom [DataProtectorTokenProvider\<TUser>](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) and <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

<span data-ttu-id="d8ca0-303">Ajoutez le fournisseur personnalisé au conteneur de service :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-303">Add the custom provider to the service container:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupEmail.cs?name=snippet1&highlight=10-13,18)]

### <a name="resend-email-confirmation"></a><span data-ttu-id="d8ca0-304">Renvoyer l’e-mail de confirmation</span><span class="sxs-lookup"><span data-stu-id="d8ca0-304">Resend email confirmation</span></span>

<span data-ttu-id="d8ca0-305">Consultez [ce problème GitHub](https://github.com/aspnet/AspNetCore/issues/5410).</span><span class="sxs-lookup"><span data-stu-id="d8ca0-305">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5410).</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="d8ca0-306">Message de débogage</span><span class="sxs-lookup"><span data-stu-id="d8ca0-306">Debug email</span></span>

<span data-ttu-id="d8ca0-307">Si vous ne parvenez à faire fonctionner l'email :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-307">If you can't get email working:</span></span>

* <span data-ttu-id="d8ca0-308">Définissez un point d’arrêt dans `EmailSender.Execute` pour vérifier que `SendGridClient.SendEmailAsync` est appelé.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-308">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="d8ca0-309">Créez une [application console pour envoyer du courrier électronique](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) à l’aide d’un code similaire à `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-309">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="d8ca0-310">Examinez la page [Activité de la messagerie](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-310">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="d8ca0-311">Vérifiez votre dossier de courrier indésirable.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-311">Check your spam folder.</span></span>
* <span data-ttu-id="d8ca0-312">Essayez un autre alias de messagerie sur un autre fournisseur de messagerie (Microsoft, Yahoo, Gmail, etc.)</span><span class="sxs-lookup"><span data-stu-id="d8ca0-312">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="d8ca0-313">Essayez d’envoyer à différents comptes de messagerie.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-313">Try sending to different email accounts.</span></span>

<span data-ttu-id="d8ca0-314">Une **bonne pratique de sécurité** consiste à **ne pas** utiliser des secrets de production en développement et en test.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-314">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="d8ca0-315">Si vous publiez l’application sur Azure, vous pouvez définir les secrets SendGrid en tant que paramètres d’application dans le portail Azure Web App.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-315">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="d8ca0-316">Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-316">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="d8ca0-317">Combiner les comptes de connexion sociale et locale</span><span class="sxs-lookup"><span data-stu-id="d8ca0-317">Combine social and local login accounts</span></span>

<span data-ttu-id="d8ca0-318">Pour compléter cette section, vous devez d’abord activer un fournisseur d’authentification externe.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-318">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="d8ca0-319">Consultez [l’authentification Facebook, Google et fournisseur externe](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="d8ca0-319">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="d8ca0-320">Vous pouvez combiner des comptes locaux et de réseaux sociaux en cliquant sur le lien de votre e-mail.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-320">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="d8ca0-321">Dans la séquence suivante, "RickAndMSFT@gmail.com" est d’abord créé en tant que connexion locale ; cependant, vous pouvez d’abord créer le compte en tant que connexion de réseau social, puis ajouter une connexion locale.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-321">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Application Web : RickAndMSFT@gmail.com utilisateur authentifié](accconfirm/_static/rick.png)

<span data-ttu-id="d8ca0-323">Cliquez sur le lien **Gérer**.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-323">Click on the **Manage** link.</span></span> <span data-ttu-id="d8ca0-324">Notez la valeur 0 externe (connexions sociales) associée à ce compte.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-324">Note the 0 external (social logins) associated with this account.</span></span>

![Gérer l’affichage](accconfirm/_static/manage.png)

<span data-ttu-id="d8ca0-326">Cliquez sur le lien vers un autre service de connexion et acceptez les demandes d’application.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-326">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="d8ca0-327">Dans l’image suivante, Facebook est le fournisseur d’authentification externe :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-327">In the following image, Facebook is the external authentication provider:</span></span>

![Gérer l’affichage de vos connexions externes à l’objet Facebook](accconfirm/_static/fb.png)

<span data-ttu-id="d8ca0-329">Les deux comptes ont été combinés.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-329">The two accounts have been combined.</span></span> <span data-ttu-id="d8ca0-330">Vous pouvez vous connecter avec l’un ou l’autre de ces comptes.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-330">You are able to sign in with either account.</span></span> <span data-ttu-id="d8ca0-331">Vous pouvez faire en sorte que vos utilisateurs ajoutent des comptes locaux au cas où leur service d’authentification de connexion sociale soit indisponible ou que, plus probablement, ils aient perdu l’accès à leur compte social.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-331">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="d8ca0-332">Activer la confirmation du compte une fois qu’un site a des utilisateurs</span><span class="sxs-lookup"><span data-stu-id="d8ca0-332">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="d8ca0-333">Activer la confirmation du compte sur un site avec des utilisateurs bloque tous les utilisateurs existants.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-333">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="d8ca0-334">Ceux-ci sont bloqués, car leurs comptes ne sont pas confirmés.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-334">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="d8ca0-335">Pour contourner le blocage des utilisateurs existants, utilisez une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="d8ca0-335">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="d8ca0-336">Mise à jour de la base de données pour marquer tous les utilisateurs existants comme étant confirmés.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-336">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="d8ca0-337">Confirmez les utilisateurs existants.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-337">Confirm existing users.</span></span> <span data-ttu-id="d8ca0-338">Par exemple, en envoyant par lot des e-mails avec des liens de confirmation.</span><span class="sxs-lookup"><span data-stu-id="d8ca0-338">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
