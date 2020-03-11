---
title: Authentification à deux facteurs avec SMS dans ASP.NET Core
author: rick-anderson
description: Découvrez comment configurer l’authentification à deux facteurs (2FA) avec une application ASP.NET Core.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 09/22/2018
ms.custom: mvc, seodec18
uid: security/authentication/2fa
ms.openlocfilehash: 424d21e446de02b39daa7685205a00931361b326
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661454"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="52637-103">Authentification à deux facteurs avec SMS dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="52637-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="52637-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Suisse-développeurs](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="52637-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

>[!WARNING]
> <span data-ttu-id="52637-105">Deux facteurs (2FA) authentificateur applications d’authentification, à l’aide d’une heure basée à usage unique mot de passe algorithme définie (TOTP), sont le secteur de l’approche 2fa recommandé.</span><span class="sxs-lookup"><span data-stu-id="52637-105">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="52637-106">À 2 facteurs à l’aide de TOTP est préférée à SMS 2FA.</span><span class="sxs-lookup"><span data-stu-id="52637-106">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="52637-107">Pour plus d’informations, consultez [activer la génération de code QR pour les applications TOTP Authenticator dans ASP.net Core](xref:security/authentication/identity-enable-qrcodes) pour ASP.net Core 2,0 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="52637-107">For more information, see [Enable QR Code generation for TOTP authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="52637-108">Ce didacticiel montre comment configurer l’authentification à deux facteurs (2FA) à l’aide de SMS.</span><span class="sxs-lookup"><span data-stu-id="52637-108">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="52637-109">Des instructions sont fournies pour [Twilio](https://www.twilio.com/) et [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), mais vous pouvez utiliser n’importe quel autre fournisseur SMS.</span><span class="sxs-lookup"><span data-stu-id="52637-109">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="52637-110">Nous vous recommandons de terminer la [confirmation du compte et la récupération du mot de passe avant de](xref:security/authentication/accconfirm) commencer ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="52637-110">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="52637-111">[Affichez ou téléchargez l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="52637-111">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="52637-112">[Comment télécharger](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="52637-112">[How to download](xref:index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="52637-113">Créer un projet ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="52637-113">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="52637-114">Créez une nouvelle ASP.NET Core application Web nommée `Web2FA` avec des comptes d’utilisateur individuels.</span><span class="sxs-lookup"><span data-stu-id="52637-114">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="52637-115">Suivez les instructions de <xref:security/enforcing-ssl> pour configurer et exiger le protocole HTTPs.</span><span class="sxs-lookup"><span data-stu-id="52637-115">Follow the instructions in <xref:security/enforcing-ssl> to set up and require HTTPS.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="52637-116">Créer un compte SMS</span><span class="sxs-lookup"><span data-stu-id="52637-116">Create an SMS account</span></span>

<span data-ttu-id="52637-117">Créez un compte SMS, par exemple, à partir de [Twilio](https://www.twilio.com/) ou [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="52637-117">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="52637-118">Enregistrer les informations d’identification d’authentification (pour twilio : accountSid et authToken, pour ASPSMS : Userkey et mot de passe).</span><span class="sxs-lookup"><span data-stu-id="52637-118">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="52637-119">Déterminer les informations d’identification du fournisseur SMS</span><span class="sxs-lookup"><span data-stu-id="52637-119">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="52637-120">**Twilio**</span><span class="sxs-lookup"><span data-stu-id="52637-120">**Twilio:**</span></span>

<span data-ttu-id="52637-121">À partir de l’onglet tableau de bord de votre compte Twilio, copiez le **SID de compte** et le **jeton d’authentification**.</span><span class="sxs-lookup"><span data-stu-id="52637-121">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="52637-122">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="52637-122">**ASPSMS:**</span></span>

<span data-ttu-id="52637-123">Dans les paramètres de votre compte, accédez à **sensibles** et copiez-le avec votre **mot de passe**.</span><span class="sxs-lookup"><span data-stu-id="52637-123">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="52637-124">Nous stockerons ces valeurs ultérieurement dans avec l’outil secret-Manager dans les clés `SMSAccountIdentification` et `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="52637-124">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="52637-125">Spécifiant l’ID d’expéditeur / donneur d’ordre</span><span class="sxs-lookup"><span data-stu-id="52637-125">Specifying SenderID / Originator</span></span>

<span data-ttu-id="52637-126">**Twilio :** Dans l’onglet nombres, copiez votre **numéro de téléphone**Twilio.</span><span class="sxs-lookup"><span data-stu-id="52637-126">**Twilio:** From the Numbers tab, copy your Twilio **phone number**.</span></span>

<span data-ttu-id="52637-127">**ASPSMS :** Dans le menu déverrouiller les expéditeurs, déverrouillez un ou plusieurs expéditeurs ou choisissez un expéditeur alphanumérique (non pris en charge par tous les réseaux).</span><span class="sxs-lookup"><span data-stu-id="52637-127">**ASPSMS:** Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>

<span data-ttu-id="52637-128">Nous stockerons cette valeur ultérieurement avec l’outil secret-Manager dans le `SMSAccountFrom`de clé.</span><span class="sxs-lookup"><span data-stu-id="52637-128">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>

### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="52637-129">Fournir des informations d’identification pour le service SMS</span><span class="sxs-lookup"><span data-stu-id="52637-129">Provide credentials for the SMS service</span></span>

<span data-ttu-id="52637-130">Nous allons utiliser le [modèle d’options](xref:fundamentals/configuration/options) pour accéder au compte d’utilisateur et aux paramètres de clé.</span><span class="sxs-lookup"><span data-stu-id="52637-130">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span>

* <span data-ttu-id="52637-131">Créez une classe pour extraire la clé SMS sécurisée.</span><span class="sxs-lookup"><span data-stu-id="52637-131">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="52637-132">Pour cet exemple, la classe `SMSoptions` est créée dans le fichier *services/SMSoptions. cs* .</span><span class="sxs-lookup"><span data-stu-id="52637-132">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="52637-133">Définissez les `SMSAccountIdentification`, `SMSAccountPassword` et `SMSAccountFrom` à l’aide de l' [outil de gestion des secrets](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="52637-133">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="52637-134">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="52637-134">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```

* <span data-ttu-id="52637-135">Ajoutez le package NuGet pour le fournisseur SMS.</span><span class="sxs-lookup"><span data-stu-id="52637-135">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="52637-136">À partir de la Manager Console (package) exécuter :</span><span class="sxs-lookup"><span data-stu-id="52637-136">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="52637-137">**Twilio**</span><span class="sxs-lookup"><span data-stu-id="52637-137">**Twilio:**</span></span>

`Install-Package Twilio`

<span data-ttu-id="52637-138">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="52637-138">**ASPSMS:**</span></span>

`Install-Package ASPSMS`

* <span data-ttu-id="52637-139">Ajoutez du code dans le fichier *services/MessageServices. cs* pour activer SMS.</span><span class="sxs-lookup"><span data-stu-id="52637-139">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="52637-140">Utilisez la Twilio ou la section ASPSMS :</span><span class="sxs-lookup"><span data-stu-id="52637-140">Use either the Twilio or the ASPSMS section:</span></span>

<span data-ttu-id="52637-141">**Twilio**</span><span class="sxs-lookup"><span data-stu-id="52637-141">**Twilio:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="52637-142">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="52637-142">**ASPSMS:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="52637-143">Configurer le démarrage pour utiliser `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="52637-143">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="52637-144">Ajoutez `SMSoptions` au conteneur de service dans la méthode `ConfigureServices` dans le *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="52637-144">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="52637-145">Activer l’authentification à deux facteurs</span><span class="sxs-lookup"><span data-stu-id="52637-145">Enable two-factor authentication</span></span>

<span data-ttu-id="52637-146">Ouvrez le fichier de vue Razor *views/Manage/index. cshtml* et supprimez les caractères de commentaire (aucun balisage n’est donc mis en commentaire).</span><span class="sxs-lookup"><span data-stu-id="52637-146">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commented out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="52637-147">Se connecter avec l’authentification à deux facteurs</span><span class="sxs-lookup"><span data-stu-id="52637-147">Log in with two-factor authentication</span></span>

* <span data-ttu-id="52637-148">Exécutez l’application et inscrire un nouvel utilisateur</span><span class="sxs-lookup"><span data-stu-id="52637-148">Run the app and register a new user</span></span>

![Affichage de Registre application Web ouverte dans Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="52637-150">Appuyez sur votre nom d’utilisateur, ce qui active la méthode d’action `Index` dans gérer le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="52637-150">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="52637-151">Cliquez ensuite sur le lien numéro de téléphone **Ajouter** .</span><span class="sxs-lookup"><span data-stu-id="52637-151">Then tap the phone number **Add** link.</span></span>

![Gérer la vue - appuyez sur le lien « Ajouter »](2fa/_static/login2fa2.png)

* <span data-ttu-id="52637-153">Ajoutez un numéro de téléphone qui recevra le code de vérification, puis appuyez sur **Envoyer le code de vérification**.</span><span class="sxs-lookup"><span data-stu-id="52637-153">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Ajouter une page de numéro de téléphone](2fa/_static/login2fa3.png)

* <span data-ttu-id="52637-155">Vous obtiendrez un message texte avec le code de vérification.</span><span class="sxs-lookup"><span data-stu-id="52637-155">You will get a text message with the verification code.</span></span> <span data-ttu-id="52637-156">Entrez-le et appuyez sur **Envoyer**</span><span class="sxs-lookup"><span data-stu-id="52637-156">Enter it and tap **Submit**</span></span>

![Vérifiez la page de numéro de téléphone](2fa/_static/login2fa4.png)

<span data-ttu-id="52637-158">Si vous n’obtenez pas un message texte, consultez la page du journal twilio.</span><span class="sxs-lookup"><span data-stu-id="52637-158">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="52637-159">La vue de gérer affiche que votre numéro de téléphone a été ajouté avec succès.</span><span class="sxs-lookup"><span data-stu-id="52637-159">The Manage view shows your phone number was added successfully.</span></span>

![Gestion de vue - numéro de téléphone a été ajouté avec succès](2fa/_static/login2fa5.png)

* <span data-ttu-id="52637-161">Appuyez sur **activer** pour activer l’authentification à deux facteurs.</span><span class="sxs-lookup"><span data-stu-id="52637-161">Tap **Enable** to enable two-factor authentication.</span></span>

![Gérer la vue - activer l’authentification à deux facteurs](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="52637-163">Authentification à deux facteurs test</span><span class="sxs-lookup"><span data-stu-id="52637-163">Test two-factor authentication</span></span>

* <span data-ttu-id="52637-164">Fermez la session.</span><span class="sxs-lookup"><span data-stu-id="52637-164">Log off.</span></span>

* <span data-ttu-id="52637-165">Connectez-vous.</span><span class="sxs-lookup"><span data-stu-id="52637-165">Log in.</span></span>

* <span data-ttu-id="52637-166">Le compte d’utilisateur a activé l’authentification à deux facteurs, donc vous devez fournir le second facteur d’authentification.</span><span class="sxs-lookup"><span data-stu-id="52637-166">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="52637-167">Dans ce didacticiel vous avez activé la vérification par téléphone.</span><span class="sxs-lookup"><span data-stu-id="52637-167">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="52637-168">Les modèles intégrés vous permettent également à configurer la messagerie en tant que le second facteur.</span><span class="sxs-lookup"><span data-stu-id="52637-168">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="52637-169">Vous pouvez configurer d’autres facteurs deuxième pour l’authentification comme les codes QR.</span><span class="sxs-lookup"><span data-stu-id="52637-169">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="52637-170">Appuyez sur **Envoyer**.</span><span class="sxs-lookup"><span data-stu-id="52637-170">Tap **Submit**.</span></span>

![Envoyer l’affichage du Code de vérification](2fa/_static/login2fa7.png)

* <span data-ttu-id="52637-172">Entrez le code que vous obtenez dans le message SMS.</span><span class="sxs-lookup"><span data-stu-id="52637-172">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="52637-173">Si vous cliquez sur la case à cocher **mémoriser ce navigateur** , vous ne devez pas utiliser 2FA pour vous connecter lorsque vous utilisez le même appareil et le même navigateur.</span><span class="sxs-lookup"><span data-stu-id="52637-173">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="52637-174">L’activation de 2FA et le clic sur **mémoriser ce navigateur** vous offriront une protection 2FA renforcée contre les utilisateurs malveillants qui essaient d’accéder à votre compte, tant qu’ils n’ont pas accès à votre appareil.</span><span class="sxs-lookup"><span data-stu-id="52637-174">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="52637-175">Vous pouvez effectuer cela sur n’importe quel appareil privé que vous utilisez régulièrement.</span><span class="sxs-lookup"><span data-stu-id="52637-175">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="52637-176">En définissant **mémoriser ce navigateur**, vous bénéficiez de la sécurité supplémentaire de 2FA à partir des appareils que vous n’utilisez pas régulièrement, et vous avez l’avantage de ne pas devoir passer par 2FA sur vos propres appareils.</span><span class="sxs-lookup"><span data-stu-id="52637-176">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Vérifier l’affichage](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="52637-178">Verrouillage de compte pour la protection contre les attaques par force brute</span><span class="sxs-lookup"><span data-stu-id="52637-178">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="52637-179">Verrouillage de compte est recommandé avec 2FA.</span><span class="sxs-lookup"><span data-stu-id="52637-179">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="52637-180">Une fois qu’un utilisateur se connecte via un compte local ou un compte de réseau social, chaque tentative ayant échoué à 2 facteurs est stocké.</span><span class="sxs-lookup"><span data-stu-id="52637-180">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="52637-181">Si les tentatives d’accès ayant échoué maximale est atteinte, l’utilisateur est verrouillé (par défaut : verrouillage de 5 minutes après 5 tentatives infructueuses accès).</span><span class="sxs-lookup"><span data-stu-id="52637-181">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="52637-182">Une authentification réussie réinitialise le nombre de tentatives d’accès ayant échoué et réinitialise l’horloge.</span><span class="sxs-lookup"><span data-stu-id="52637-182">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="52637-183">Les tentatives d’accès et l’heure de verrouillage maximales peuvent être définies avec [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) et [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span><span class="sxs-lookup"><span data-stu-id="52637-183">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="52637-184">Les éléments suivants configurent le verrouillage de compte pendant 10 minutes après que 10 échecs de tentatives d’accès :</span><span class="sxs-lookup"><span data-stu-id="52637-184">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="52637-185">Vérifiez que [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) définit `lockoutOnFailure` sur `true`:</span><span class="sxs-lookup"><span data-stu-id="52637-185">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
