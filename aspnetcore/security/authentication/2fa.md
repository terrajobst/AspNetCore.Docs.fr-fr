---
title: Authentification à deux facteurs avec SMS dans ASP.NET Core
author: rick-anderson
description: Découvrez comment configurer une authentification à deux facteurs (2FA) avec une application ASP.NET Core.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 08/15/2017
uid: security/authentication/2fa
ms.openlocfilehash: 335edfd5cd4dfbb9d223ba0ae888a6d2386cd4a5
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272307"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="bf2d2-103">Authentification à deux facteurs avec SMS dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bf2d2-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="bf2d2-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [développeurs-Suisse](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="bf2d2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="bf2d2-105">Consultez [génération activer le Code QR pour les applications d’authentification dans ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) pour ASP.NET Core 2.0 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-105">See [Enable QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="bf2d2-106">Ce didacticiel montre comment configurer l’authentification à deux facteurs (2FA) à l’aide de SMS.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-106">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="bf2d2-107">Des instructions sont fournies pour [twilio](https://www.twilio.com/) et [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), mais vous pouvez utiliser n’importe quel autre fournisseur SMS.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-107">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="bf2d2-108">Nous vous recommandons de terminer [Confirmation du compte et la récupération de mot de passe](xref:security/authentication/accconfirm) avant de commencer ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-108">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="bf2d2-109">Afficher le [exemple terminé](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="bf2d2-109">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="bf2d2-110">[Comment télécharger](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="bf2d2-110">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="bf2d2-111">Créez un nouveau projet ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bf2d2-111">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="bf2d2-112">Créer une application web ASP.NET Core nommée `Web2FA` avec les comptes d’utilisateur individuels.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-112">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="bf2d2-113">Suivez les instructions de [appliquer de SSL dans une application ASP.NET Core](xref:security/enforcing-ssl) pour configurer et exiger le protocole SSL.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-113">Follow the instructions in [Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="bf2d2-114">Créer un compte SMS</span><span class="sxs-lookup"><span data-stu-id="bf2d2-114">Create an SMS account</span></span>

<span data-ttu-id="bf2d2-115">Créer un compte SMS, par exemple, à partir de [twilio](https://www.twilio.com/) ou [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="bf2d2-115">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="bf2d2-116">Enregistrer les informations d’identification d’authentification (pour twilio : accountSid et du jeton d’authentification, pour ASPSMS : clé d’utilisateur a et le mot de passe).</span><span class="sxs-lookup"><span data-stu-id="bf2d2-116">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="bf2d2-117">Déterminer les informations d’identification du fournisseur SMS</span><span class="sxs-lookup"><span data-stu-id="bf2d2-117">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="bf2d2-118">**Twilio :**</span><span class="sxs-lookup"><span data-stu-id="bf2d2-118">**Twilio:**</span></span>  
<span data-ttu-id="bf2d2-119">Sous l’onglet tableau de bord de votre compte Twilio, copiez la **SID de compte** et **Auth jeton**.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-119">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="bf2d2-120">**ASPSMS :**</span><span class="sxs-lookup"><span data-stu-id="bf2d2-120">**ASPSMS:**</span></span>  
<span data-ttu-id="bf2d2-121">À partir des paramètres de votre compte, accédez à **clé d’utilisateur a** et copiez-le avec votre **mot de passe**.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-121">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="bf2d2-122">Nous allons stocker ultérieurement ces valeurs avec l’outil Gestionnaire de secret dans les clés de `SMSAccountIdentification` et `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-122">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="bf2d2-123">Spécifiant l’ID d’expéditeur / donneur d’ordre</span><span class="sxs-lookup"><span data-stu-id="bf2d2-123">Specifying SenderID / Originator</span></span>

<span data-ttu-id="bf2d2-124">**Twilio :**</span><span class="sxs-lookup"><span data-stu-id="bf2d2-124">**Twilio:**</span></span>  
<span data-ttu-id="bf2d2-125">À partir de l’onglet numéros, copiez votre Twilio **numéro de téléphone**.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-125">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="bf2d2-126">**ASPSMS :**</span><span class="sxs-lookup"><span data-stu-id="bf2d2-126">**ASPSMS:**</span></span>  
<span data-ttu-id="bf2d2-127">Dans le Menu des expéditeurs déverrouiller, déverrouillage d’un ou plusieurs expéditeurs, ou choisissez un donneur d’ordre alphanumérique (non pris en charge par tous les réseaux).</span><span class="sxs-lookup"><span data-stu-id="bf2d2-127">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="bf2d2-128">Nous allons stocker plus tard cette valeur avec l’outil Gestionnaire de la clé secrète au sein de la clé `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-128">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="bf2d2-129">Fournissez les informations d’identification pour le service SMS</span><span class="sxs-lookup"><span data-stu-id="bf2d2-129">Provide credentials for the SMS service</span></span>

<span data-ttu-id="bf2d2-130">Nous allons utiliser la [modèle d’Options](xref:fundamentals/configuration/options) pour accéder aux paramètres de compte et une clé utilisateur.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-130">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="bf2d2-131">Créez une classe pour extraire la clé SMS sécurisée.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-131">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="bf2d2-132">Pour cet exemple, le `SMSoptions` classe est créée dans le *Services/SMSoptions.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-132">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="bf2d2-133">Définir le `SMSAccountIdentification`, `SMSAccountPassword` et `SMSAccountFrom` avec la [outil Gestionnaire de secret](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="bf2d2-133">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="bf2d2-134">Exemple :</span><span class="sxs-lookup"><span data-stu-id="bf2d2-134">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="bf2d2-135">Ajoutez le package NuGet pour le fournisseur SMS.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-135">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="bf2d2-136">À partir du Package Manager Console (PMC) exécuter :</span><span class="sxs-lookup"><span data-stu-id="bf2d2-136">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="bf2d2-137">**Twilio :**</span><span class="sxs-lookup"><span data-stu-id="bf2d2-137">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="bf2d2-138">**ASPSMS :**</span><span class="sxs-lookup"><span data-stu-id="bf2d2-138">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="bf2d2-139">Ajoutez le code dans le *Services/MessageServices.cs* fichier pour activer les SMS.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-139">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="bf2d2-140">Utilisez la Twilio ou la section ASPSMS :</span><span class="sxs-lookup"><span data-stu-id="bf2d2-140">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="bf2d2-141">**Twilio :**</span><span class="sxs-lookup"><span data-stu-id="bf2d2-141">**Twilio:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="bf2d2-142">**ASPSMS :**</span><span class="sxs-lookup"><span data-stu-id="bf2d2-142">**ASPSMS:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="bf2d2-143">Configurer le démarrage à utiliser `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="bf2d2-143">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="bf2d2-144">Ajouter `SMSoptions` au conteneur de service dans le `ConfigureServices` méthode dans le *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="bf2d2-144">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="bf2d2-145">Activer l’authentification à deux facteurs</span><span class="sxs-lookup"><span data-stu-id="bf2d2-145">Enable two-factor authentication</span></span>

<span data-ttu-id="bf2d2-146">Ouvrez le *Views/Manage/Index.cshtml* fichier de vue Razor et supprimer les caractères de commentaire (aucune balise n’est donc commenté).</span><span class="sxs-lookup"><span data-stu-id="bf2d2-146">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="bf2d2-147">Se connecter avec l’authentification à deux facteurs</span><span class="sxs-lookup"><span data-stu-id="bf2d2-147">Log in with two-factor authentication</span></span>

* <span data-ttu-id="bf2d2-148">Exécutez l’application et inscrire un nouvel utilisateur</span><span class="sxs-lookup"><span data-stu-id="bf2d2-148">Run the app and register a new user</span></span>

![Affichage de Registre Web application ouverte dans Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="bf2d2-150">Cliquez sur votre nom d’utilisateur, ce qui active la `Index` méthode d’action de contrôleur de gestion.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-150">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="bf2d2-151">Puis cliquez sur le numéro de téléphone **ajouter** lien.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-151">Then tap the phone number **Add** link.</span></span>

![Gérer les affichages](2fa/_static/login2fa2.png)

* <span data-ttu-id="bf2d2-153">Ajouter un numéro de téléphone qui recevra le code de vérification et appuyez sur **envoyer le code de vérification**.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-153">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Ajouter une page de numéro de téléphone](2fa/_static/login2fa3.png)

* <span data-ttu-id="bf2d2-155">Vous obtenez un message texte avec le code de vérification.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-155">You will get a text message with the verification code.</span></span> <span data-ttu-id="bf2d2-156">Entrer et appuyez sur **envoyer**</span><span class="sxs-lookup"><span data-stu-id="bf2d2-156">Enter it and tap **Submit**</span></span>

![Vérifier la page du numéro de téléphone](2fa/_static/login2fa4.png)

<span data-ttu-id="bf2d2-158">Si vous n’obtenez pas un message texte, consultez la page du journal twilio.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-158">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="bf2d2-159">L’affichage de gestion affiche que votre numéro de téléphone a été ajouté avec succès.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-159">The Manage view shows your phone number was added successfully.</span></span>

![Gérer les affichages](2fa/_static/login2fa5.png)

* <span data-ttu-id="bf2d2-161">Appuyez sur **activer** pour activer l’authentification à deux facteurs.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-161">Tap **Enable** to enable two-factor authentication.</span></span>

![Gérer les affichages](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="bf2d2-163">Authentification à deux facteurs test</span><span class="sxs-lookup"><span data-stu-id="bf2d2-163">Test two-factor authentication</span></span>

* <span data-ttu-id="bf2d2-164">Fermez la session.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-164">Log off.</span></span>

* <span data-ttu-id="bf2d2-165">Se connecter.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-165">Log in.</span></span>

* <span data-ttu-id="bf2d2-166">Le compte d’utilisateur a activé l’authentification à deux facteurs, vous devez fournir le second facteur d’authentification.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-166">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="bf2d2-167">Dans ce didacticiel vous avez activé la vérification par téléphone.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-167">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="bf2d2-168">Les modèles intégrés vous permettent également à configurer la messagerie en tant que le second facteur.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-168">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="bf2d2-169">Vous pouvez configurer des facteurs de deuxième supplémentaires pour l’authentification telles que des codes QR.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-169">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="bf2d2-170">Appuyez sur **envoyer**.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-170">Tap **Submit**.</span></span>

![Affichage du Code de vérification d’envoi](2fa/_static/login2fa7.png)

* <span data-ttu-id="bf2d2-172">Entrez le code que vous obtenez dans le message SMS.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-172">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="bf2d2-173">En cliquant sur le **n’oubliez pas de ce navigateur** case à cocher vous exempter de devoir utiliser 2FA pour vous connecter lorsque vous utilisez le même périphérique et le navigateur.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-173">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="bf2d2-174">L’activation de 2FA et en cliquant sur **n’oubliez pas de ce navigateur** vous fournira fort 2FA protection contre les utilisateurs malveillants, essayez d’accéder à votre compte, tant qu’ils n’ont pas accès à votre appareil.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-174">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="bf2d2-175">Pour cela, sur n’importe quel appareil privé que vous utilisez régulièrement.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-175">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="bf2d2-176">En définissant **n’oubliez pas de ce navigateur**, vous obtenez la sécurité renforcée de 2FA à partir d’appareils que vous n’utilisez pas régulièrement, et la commodité sur ne pas avoir à passer par 2FA sur vos propres appareils.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-176">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Vérifier l’affichage](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="bf2d2-178">Verrouillage de compte pour la protection contre les attaques en force brute</span><span class="sxs-lookup"><span data-stu-id="bf2d2-178">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="bf2d2-179">Le verrouillage de compte est recommandé avec 2FA.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-179">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="bf2d2-180">Une fois qu’un utilisateur se connecte via un compte local ou sociaux, chaque tentative ayant échoué 2FA est stocké.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-180">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="bf2d2-181">Si les tentatives d’accès ayant échoué maximale est atteinte, l’utilisateur est verrouillé (valeur par défaut : verrouillage de 5 minutes après l’échec de la tentative d’accès 5).</span><span class="sxs-lookup"><span data-stu-id="bf2d2-181">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="bf2d2-182">Une authentification réussie réinitialise le nombre de tentatives d’accès ayant échoué et réinitialise l’horloge.</span><span class="sxs-lookup"><span data-stu-id="bf2d2-182">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="bf2d2-183">Le nombre maximal de tentatives d’accès infructueuses et l’heure de verrouillage peut être défini avec [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) et [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span><span class="sxs-lookup"><span data-stu-id="bf2d2-183">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="bf2d2-184">La commande suivante configure le verrouillage de compte pendant 10 minutes après que 10 échecs de tentatives d’accès :</span><span class="sxs-lookup"><span data-stu-id="bf2d2-184">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="bf2d2-185">Vérifiez que [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) définit `lockoutOnFailure` à `true`:</span><span class="sxs-lookup"><span data-stu-id="bf2d2-185">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
