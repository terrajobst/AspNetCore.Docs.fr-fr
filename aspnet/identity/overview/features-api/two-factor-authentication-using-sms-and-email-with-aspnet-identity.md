---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: "Authentification à deux facteurs à l’aide de SMS et des messages électroniques avec ASP.NET Identity | Documents Microsoft"
author: HaoK
description: "Ce didacticiel vous indiquera comment configurer l’authentification à deux facteurs (2FA) à l’aide de SMS et des messages électroniques. Cet article a été rédigé par Rick Anderson ( @RickAndMSFT ), par..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0f9ff7cf74048a008b150da1e843ff15333269ab
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a><span data-ttu-id="0039d-104">Authentification à deux facteurs avec l’identité de ASP.NET à l’aide de SMS et par courrier électronique</span><span class="sxs-lookup"><span data-stu-id="0039d-104">Two-factor authentication using SMS and email with ASP.NET Identity</span></span>
====================
<span data-ttu-id="0039d-105">par [arts martiaux de Hao](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span><span class="sxs-lookup"><span data-stu-id="0039d-105">by [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span></span>

> <span data-ttu-id="0039d-106">Ce didacticiel vous indiquera comment configurer l’authentification à deux facteurs (2FA) à l’aide de SMS et des messages électroniques.</span><span class="sxs-lookup"><span data-stu-id="0039d-106">This tutorial will show you how to set up Two-factor authentication (2FA) using SMS and email.</span></span>
> 
> <span data-ttu-id="0039d-107">Cet article a été rédigé par Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), arts martiaux de Hao et Suhas Joshi.</span><span class="sxs-lookup"><span data-stu-id="0039d-107">This article was written by Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung, and Suhas Joshi.</span></span> <span data-ttu-id="0039d-108">L’exemple de NuGet a été écrit principalement par arts martiaux de Hao.</span><span class="sxs-lookup"><span data-stu-id="0039d-108">The NuGet sample was written primarily by Hao Kung.</span></span>


<span data-ttu-id="0039d-109">Cette rubrique couvre les sujets suivants :</span><span class="sxs-lookup"><span data-stu-id="0039d-109">This topic covers the following:</span></span>

- [<span data-ttu-id="0039d-110">Création de l’exemple d’identité</span><span class="sxs-lookup"><span data-stu-id="0039d-110">Building the Identity sample</span></span>](#build)
- [<span data-ttu-id="0039d-111">Configurer SMS pour l’authentification à deux facteurs</span><span class="sxs-lookup"><span data-stu-id="0039d-111">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="0039d-112">Activer l’authentification à deux facteurs</span><span class="sxs-lookup"><span data-stu-id="0039d-112">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="0039d-113">Comment inscrire un fournisseur d’authentification à deux facteurs</span><span class="sxs-lookup"><span data-stu-id="0039d-113">How to register a Two-factor authentication provider</span></span>](#reg)
- [<span data-ttu-id="0039d-114">Combiner des comptes de connexion de réseaux sociaux et local</span><span class="sxs-lookup"><span data-stu-id="0039d-114">Combine social and local login accounts</span></span>](#combine)
- [<span data-ttu-id="0039d-115">Verrouillage de compte à partir des attaques en force brute</span><span class="sxs-lookup"><span data-stu-id="0039d-115">Account lockout from brute force attacks</span></span>](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a><span data-ttu-id="0039d-116">Création de l’exemple d’identité</span><span class="sxs-lookup"><span data-stu-id="0039d-116">Building the Identity sample</span></span>

<span data-ttu-id="0039d-117">Dans cette section, vous utiliserez NuGet pour télécharger un exemple que nous travaillons avec.</span><span class="sxs-lookup"><span data-stu-id="0039d-117">In this section, you'll use NuGet to download a sample we will work with.</span></span> <span data-ttu-id="0039d-118">Commencez par installer et exécuter [Visual Studio Express 2013 pour le Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="0039d-118">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="0039d-119">Installation de Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) ou une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="0039d-119">Install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="0039d-120">Avertissement : Vous devez installer Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) pour effectuer ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="0039d-120">Warning: You must install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) to complete this tutorial.</span></span>


1. <span data-ttu-id="0039d-121">Créer un nouveau ***vide*** projet Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0039d-121">Create a new ***empty*** ASP.NET Web project.</span></span>
2. <span data-ttu-id="0039d-122">Dans la Console du Gestionnaire de Package, entrez ce qui suit les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="0039d-122">In the Package Manager Console, enter the following the following commands:</span></span>  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
 <span data-ttu-id="0039d-123">Dans ce didacticiel, nous allons utiliser [SendGrid](http://sendgrid.com/) pour envoyer un courrier électronique et [Twilio](https://www.twilio.com/) ou [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) pour sms texto.</span><span class="sxs-lookup"><span data-stu-id="0039d-123">In this tutorial, we'll use [SendGrid](http://sendgrid.com/) to send email and [Twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) for sms texting.</span></span> <span data-ttu-id="0039d-124">Le `Identity.Samples` package installe le code que nous travaillerons avec.</span><span class="sxs-lookup"><span data-stu-id="0039d-124">The `Identity.Samples` package installs the code we will be working with.</span></span>
3. <span data-ttu-id="0039d-125">Définir le [projet pour utiliser SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="0039d-125">Set the [project to use SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="0039d-126">*Facultatif*: suivez les instructions de mon [didacticiel de confirmation par courrier électronique](account-confirmation-and-password-recovery-with-aspnet-identity.md) pour raccorder SendGrid puis exécuter l’application et inscrire un compte de messagerie.</span><span class="sxs-lookup"><span data-stu-id="0039d-126">*Optional*: Follow the instructions in my [Email confirmation tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md) to hook up SendGrid and then run the app and register an email account.</span></span>
5. <span data-ttu-id="0039d-127">* Facultatif : * supprimez le code de confirmation de lien démonstration par courrier électronique à partir de l’exemple (le `ViewBag.Link` code dans le contrôleur de compte.</span><span class="sxs-lookup"><span data-stu-id="0039d-127">*Optional: *Remove the demo email link confirmation code from the sample (The `ViewBag.Link` code in the account controller.</span></span> <span data-ttu-id="0039d-128">Consultez le `DisplayEmail` et `ForgotPasswordConfirmation` les méthodes d’action et des vues razor).</span><span class="sxs-lookup"><span data-stu-id="0039d-128">See the `DisplayEmail` and `ForgotPasswordConfirmation` action methods and razor views ).</span></span>
6. <span data-ttu-id="0039d-129">* Facultatif : * supprimez la `ViewBag.Status` code à partir de contrôleurs de compte et de gérer et de la *Views\Account\VerifyCode.cshtml* et *Views\Manage\VerifyPhoneNumber.cshtml* vues razor.</span><span class="sxs-lookup"><span data-stu-id="0039d-129">*Optional: *Remove the `ViewBag.Status` code from the Manage and Account controllers and from the *Views\Account\VerifyCode.cshtml* and *Views\Manage\VerifyPhoneNumber.cshtml* razor views.</span></span> <span data-ttu-id="0039d-130">Vous pouvez également, garder la `ViewBag.Status` affichage pour tester le fonctionnement de cette application localement sans avoir à connecter et envoyer par courrier électronique et les messages SMS.</span><span class="sxs-lookup"><span data-stu-id="0039d-130">Alternatively, you can keep the `ViewBag.Status` display to test how this app works locally without having to hook up and send email and SMS messages.</span></span>

> [!NOTE]
> <span data-ttu-id="0039d-131">Avertissement : Si vous modifiez les paramètres de sécurité dans cet exemple, les applications de production devrez subir un audit de sécurité qui appelle explicitement les modifications apportées.</span><span class="sxs-lookup"><span data-stu-id="0039d-131">Warning: If you change any of the security settings in this sample, productions apps will need to undergo a security audit that explicitly calls out the changes made.</span></span>


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="0039d-132">Configurer SMS pour l’authentification à deux facteurs</span><span class="sxs-lookup"><span data-stu-id="0039d-132">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="0039d-133">Ce didacticiel fournit des instructions relatives à l’aide de Twilio ou ASPSMS, mais vous pouvez utiliser n’importe quel autre fournisseur SMS.</span><span class="sxs-lookup"><span data-stu-id="0039d-133">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="0039d-134">**Création d’un compte d’utilisateur avec un fournisseur SMS**</span><span class="sxs-lookup"><span data-stu-id="0039d-134">**Creating a User Account with an SMS provider**</span></span>  
  
 <span data-ttu-id="0039d-135">Créer un [Twilio](https://www.twilio.com/try-twilio) ou [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) compte.</span><span class="sxs-lookup"><span data-stu-id="0039d-135">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="0039d-136">**Installation de packages supplémentaires ou l’ajout de références de service**</span><span class="sxs-lookup"><span data-stu-id="0039d-136">**Installing additional packages or adding service references**</span></span>  
  
 <span data-ttu-id="0039d-137">Twilio :</span><span class="sxs-lookup"><span data-stu-id="0039d-137">Twilio:</span></span>  
 <span data-ttu-id="0039d-138">Dans la Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="0039d-138">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
 <span data-ttu-id="0039d-139">ASPSMS :</span><span class="sxs-lookup"><span data-stu-id="0039d-139">ASPSMS:</span></span>  
 <span data-ttu-id="0039d-140">La référence de service suivant doit être ajouté :</span><span class="sxs-lookup"><span data-stu-id="0039d-140">The following service reference needs to be added:</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
 <span data-ttu-id="0039d-141">Adresse :</span><span class="sxs-lookup"><span data-stu-id="0039d-141">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
 <span data-ttu-id="0039d-142">Espace de noms :</span><span class="sxs-lookup"><span data-stu-id="0039d-142">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="0039d-143">**Déterminer les informations d’identification de l’utilisateur du fournisseur SMS**</span><span class="sxs-lookup"><span data-stu-id="0039d-143">**Figuring out SMS Provider User credentials**</span></span>  
  
 <span data-ttu-id="0039d-144">Twilio :</span><span class="sxs-lookup"><span data-stu-id="0039d-144">Twilio:</span></span>  
 <span data-ttu-id="0039d-145">À partir de la **tableau de bord** onglet de votre compte Twilio, copie le **SID de compte** et **Auth jeton**.</span><span class="sxs-lookup"><span data-stu-id="0039d-145">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
 <span data-ttu-id="0039d-146">ASPSMS :</span><span class="sxs-lookup"><span data-stu-id="0039d-146">ASPSMS:</span></span>  
 <span data-ttu-id="0039d-147">À partir des paramètres de votre compte, accédez à **clé d’utilisateur a** et copiez-le avec votre définis par l’utilisateur **mot de passe**.</span><span class="sxs-lookup"><span data-stu-id="0039d-147">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
 <span data-ttu-id="0039d-148">Nous allons stocker plus tard de ces valeurs dans les variables `SMSAccountIdentification` et `SMSAccountPassword` .</span><span class="sxs-lookup"><span data-stu-id="0039d-148">We will later store these values in the variables `SMSAccountIdentification` and `SMSAccountPassword` .</span></span>
4. <span data-ttu-id="0039d-149">**Spécifiant l’ID d’expéditeur / donneur d’ordre**</span><span class="sxs-lookup"><span data-stu-id="0039d-149">**Specifying SenderID / Originator**</span></span>  
  
 <span data-ttu-id="0039d-150">Twilio :</span><span class="sxs-lookup"><span data-stu-id="0039d-150">Twilio:</span></span>  
 <span data-ttu-id="0039d-151">À partir de la **numéros** , onglet de la copie de votre numéro de téléphone Twilio.</span><span class="sxs-lookup"><span data-stu-id="0039d-151">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
 <span data-ttu-id="0039d-152">ASPSMS :</span><span class="sxs-lookup"><span data-stu-id="0039d-152">ASPSMS:</span></span>  
 <span data-ttu-id="0039d-153">Dans le **déverrouiller les expéditeurs** déverrouiller un ou plusieurs expéditeurs de Menu, ou choisissez un donneur d’ordre alphanumérique (non pris en charge par tous les réseaux).</span><span class="sxs-lookup"><span data-stu-id="0039d-153">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
 <span data-ttu-id="0039d-154">Nous allons stocker plus loin de cette valeur dans la variable `SMSAccountFrom` .</span><span class="sxs-lookup"><span data-stu-id="0039d-154">We will later store this value in the variable `SMSAccountFrom` .</span></span>
5. <span data-ttu-id="0039d-155">**Transfert des informations d’identification du fournisseur SMS dans l’application**</span><span class="sxs-lookup"><span data-stu-id="0039d-155">**Transferring SMS provider credentials into app**</span></span>  
  
 <span data-ttu-id="0039d-156">Rendre les informations d’identification et le numéro de téléphone de l’expéditeur disponible à l’application :</span><span class="sxs-lookup"><span data-stu-id="0039d-156">Make the credentials and sender phone number available to the app:</span></span>

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > <span data-ttu-id="0039d-157">Sécurité - magasin jamais des données sensibles dans votre code source.</span><span class="sxs-lookup"><span data-stu-id="0039d-157">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="0039d-158">Le compte et les informations d’identification sont ajoutées au code ci-dessus pour simplifier l’exemple.</span><span class="sxs-lookup"><span data-stu-id="0039d-158">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="0039d-159">Consultez de Jon Atten [ASP.NET MVC : conserver les paramètres privés hors contrôle de code Source](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span><span class="sxs-lookup"><span data-stu-id="0039d-159">See Jon Atten's [ASP.NET MVC: Keep Private Settings Out of Source Control](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span></span>
6. <span data-ttu-id="0039d-160">**Implémentation du transfert de données au fournisseur SMS**</span><span class="sxs-lookup"><span data-stu-id="0039d-160">**Implementation of data transfer to SMS provider**</span></span>  
  
 <span data-ttu-id="0039d-161">Configurer le `SmsService` classe dans le *application\_Start\IdentityConfig.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="0039d-161">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
 <span data-ttu-id="0039d-162">Selon le fournisseur SMS utilisé activer soit le **Twilio** ou **ASPSMS** section :</span><span class="sxs-lookup"><span data-stu-id="0039d-162">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. <span data-ttu-id="0039d-163">Exécutez l’application et connectez-vous avec le compte que vous avez enregistré précédemment.</span><span class="sxs-lookup"><span data-stu-id="0039d-163">Run the app and log in with the account you previously registered.</span></span>
8. <span data-ttu-id="0039d-164">Cliquez sur votre nom d’utilisateur, ce qui active le `Index` méthode d’action de `Manage` contrôleur.</span><span class="sxs-lookup"><span data-stu-id="0039d-164">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. <span data-ttu-id="0039d-165">Cliquez sur Ajouter.</span><span class="sxs-lookup"><span data-stu-id="0039d-165">Click Add.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. <span data-ttu-id="0039d-166">En quelques secondes, vous obtiendrez un message texte avec le code de vérification.</span><span class="sxs-lookup"><span data-stu-id="0039d-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="0039d-167">Entrer et appuyez sur **Submit**.</span><span class="sxs-lookup"><span data-stu-id="0039d-167">Enter it and press **Submit**.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. <span data-ttu-id="0039d-168">L’affichage de gestion affiche que votre numéro de téléphone a été ajouté.</span><span class="sxs-lookup"><span data-stu-id="0039d-168">The Manage view shows your phone number was added.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a><span data-ttu-id="0039d-169">Examinez le code</span><span class="sxs-lookup"><span data-stu-id="0039d-169">Examine the code</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

<span data-ttu-id="0039d-170">Le `Index` méthode d’action de `Manage` contrôleur définit le message d’état en fonction de votre action précédente et fournit des liens pour modifier votre mot de passe local ou d’ajouter un compte local.</span><span class="sxs-lookup"><span data-stu-id="0039d-170">The `Index` action method in `Manage` controller sets the status message based on your previous action and provides links to change your local password or add a local account.</span></span> <span data-ttu-id="0039d-171">Le `Index` méthode affiche également l’état ou de votre 2FA téléphone numéro, logins externes, 2FA activée, n’oubliez pas de méthode 2FA pour ce navigateur (expliqué plus loin).</span><span class="sxs-lookup"><span data-stu-id="0039d-171">The `Index` method also displays the state or your 2FA phone number, external logins, 2FA enabled, and remember 2FA method for this browser(explained later).</span></span> <span data-ttu-id="0039d-172">En cliquant sur votre nom d’utilisateur (courrier électronique) dans la barre de titre ne transmet un message.</span><span class="sxs-lookup"><span data-stu-id="0039d-172">Clicking on your user ID (email) in the title bar doesn't pass a message.</span></span> <span data-ttu-id="0039d-173">En cliquant sur le **numéro de téléphone : supprimez** lien passe `Message=RemovePhoneSuccess` comme une chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="0039d-173">Clicking on the **Phone Number : remove** link passes `Message=RemovePhoneSuccess` as a query string.</span></span>  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

<span data-ttu-id="0039d-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span><span class="sxs-lookup"><span data-stu-id="0039d-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span></span>

<span data-ttu-id="0039d-175">Le `AddPhoneNumber` méthode d’action affiche une boîte de dialogue pour entrer un numéro de téléphone qui permettre recevoir des messages SMS.</span><span class="sxs-lookup"><span data-stu-id="0039d-175">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

<span data-ttu-id="0039d-176">En cliquant sur le **envoyer le code de vérification** bouton publie le numéro de téléphone pour la requête HTTP POST `AddPhoneNumber` méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="0039d-176">Clicking on the **Send verification code** button posts the phone number to the HTTP POST `AddPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

<span data-ttu-id="0039d-177">Le `GenerateChangePhoneNumberTokenAsync` méthode génère le jeton de sécurité qui sera défini dans le message SMS.</span><span class="sxs-lookup"><span data-stu-id="0039d-177">The `GenerateChangePhoneNumberTokenAsync` method generates the security token which will be set in the SMS message.</span></span> <span data-ttu-id="0039d-178">Si le service SMS a été configuré, le jeton est envoyé en tant que la chaîne &quot;votre code de sécurité est &lt;jeton&gt;&quot;.</span><span class="sxs-lookup"><span data-stu-id="0039d-178">If the SMS service has been configured, the token is sent as the string &quot;Your security code is &lt;token&gt;&quot;.</span></span> <span data-ttu-id="0039d-179">Le `SmsService.SendAsync` méthode est appelée de façon asynchrone, puis l’application est redirigée vers la `VerifyPhoneNumber` méthode d’action (qui affiche la boîte de dialogue), où vous pouvez entrer le code de vérification.</span><span class="sxs-lookup"><span data-stu-id="0039d-179">The `SmsService.SendAsync` method to is called asynchronously, then the app is redirected to the `VerifyPhoneNumber` action method (which displays the following dialog), where you can enter the verification code.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

<span data-ttu-id="0039d-180">Une fois que vous entrez le code et cliquez sur Envoyer, le code est validé dans la requête HTTP POST `VerifyPhoneNumber` méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="0039d-180">Once you enter the code and click submit, the code is posted to the HTTP POST `VerifyPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

<span data-ttu-id="0039d-181">Le `ChangePhoneNumberAsync` méthode vérifie le code de sécurité publié.</span><span class="sxs-lookup"><span data-stu-id="0039d-181">The `ChangePhoneNumberAsync` method checks the posted security code.</span></span> <span data-ttu-id="0039d-182">Si le code est correct, le numéro de téléphone est ajouté à la `PhoneNumber` champ le `AspNetUsers` table.</span><span class="sxs-lookup"><span data-stu-id="0039d-182">If the code is correct, the phone number is added to the `PhoneNumber` field of the `AspNetUsers` table.</span></span> <span data-ttu-id="0039d-183">Si cet appel réussit, la `SignInAsync` méthode est appelée :</span><span class="sxs-lookup"><span data-stu-id="0039d-183">If that call is successful, the `SignInAsync` method is called:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

<span data-ttu-id="0039d-184">Le `isPersistent` paramètre définit si la session d’authentification est maintenue entre plusieurs demandes.</span><span class="sxs-lookup"><span data-stu-id="0039d-184">The `isPersistent` parameter sets whether the authentication session is persisted across multiple requests.</span></span>

<span data-ttu-id="0039d-185">Lorsque vous modifiez votre profil de sécurité, un nouvel horodatage de sécurité est généré et stocké dans le `SecurityStamp` champ le *AspNetUsers* table.</span><span class="sxs-lookup"><span data-stu-id="0039d-185">When you change your security profile, a new security stamp is generated and stored in the `SecurityStamp` field of the *AspNetUsers* table.</span></span> <span data-ttu-id="0039d-186">Notez que le `SecurityStamp` champ est différent à partir du cookie de sécurité.</span><span class="sxs-lookup"><span data-stu-id="0039d-186">Note, the `SecurityStamp` field is different from the security cookie.</span></span> <span data-ttu-id="0039d-187">Le cookie de sécurité n’est pas stocké dans le `AspNetUsers` table (ou ailleurs dans la base de données d’identité).</span><span class="sxs-lookup"><span data-stu-id="0039d-187">The security cookie is not stored in the `AspNetUsers` table (or anywhere else in the Identity DB).</span></span> <span data-ttu-id="0039d-188">Le jeton de cookie de sécurité est auto-signé à l’aide de [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) et est créé avec le `UserId, SecurityStamp` et les informations d’heure d’expiration.</span><span class="sxs-lookup"><span data-stu-id="0039d-188">The security cookie token is self-signed using [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) and is created with the `UserId, SecurityStamp` and expiration time information.</span></span>

<span data-ttu-id="0039d-189">Le middleware du cookie vérifie le cookie à chaque demande.</span><span class="sxs-lookup"><span data-stu-id="0039d-189">The cookie middleware checks the cookie on each request.</span></span> <span data-ttu-id="0039d-190">Le `SecurityStampValidator` méthode dans le `Startup` classe accède à la base de données et vérifie périodiquement, de tampon de sécurité tel que spécifié par le `validateInterval`.</span><span class="sxs-lookup"><span data-stu-id="0039d-190">The `SecurityStampValidator` method in the `Startup` class hits the DB and checks security stamp periodically, as specified with the `validateInterval`.</span></span> <span data-ttu-id="0039d-191">Cela se produit uniquement toutes les 30 minutes (dans notre exemple), sauf si vous modifiez votre profil de sécurité.</span><span class="sxs-lookup"><span data-stu-id="0039d-191">This only happens every 30 minutes (in our sample) unless you change your security profile.</span></span> <span data-ttu-id="0039d-192">L’intervalle de 30 minutes a été choisi pour réduire les allers-retours vers la base de données.</span><span class="sxs-lookup"><span data-stu-id="0039d-192">The 30 minute interval was chosen to minimize trips to the database.</span></span>

<span data-ttu-id="0039d-193">Le `SignInAsync` méthode doit être appelée lorsqu’une modification est apportée au profil de sécurité.</span><span class="sxs-lookup"><span data-stu-id="0039d-193">The `SignInAsync` method needs to be called when any change is made to the security profile.</span></span> <span data-ttu-id="0039d-194">Lorsque le profil de sécurité change, la base de données est mises à jour le `SecurityStamp` champ et sans appeler le `SignInAsync` méthode vous restera connecté *uniquement* jusqu'à ce que la prochaine fois que le pipeline OWIN accède à la base de données (la `validateInterval`).</span><span class="sxs-lookup"><span data-stu-id="0039d-194">When the security profile changes, the database is updates the `SecurityStamp` field, and without calling the `SignInAsync` method you would stay logged in *only* until the next time the OWIN pipeline hits the database (the `validateInterval`).</span></span> <span data-ttu-id="0039d-195">Vous pouvez le tester en modifiant le `SignInAsync` méthode pour retourner immédiatement et la définition du cookie `validateInterval` propriété de 30 minutes à 5 secondes :</span><span class="sxs-lookup"><span data-stu-id="0039d-195">You can test this by changing the `SignInAsync` method to return immediately, and setting the cookie `validateInterval` property from 30 minutes to 5 seconds:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

<span data-ttu-id="0039d-196">Avec les modifications de code ci-dessus, vous pouvez modifier votre profil de sécurité (par exemple, en modifiant l’état de **activé à deux facteurs**) et vous allez être déconnecté dans 5 secondes lorsque la `SecurityStampValidator.OnValidateIdentity` méthode échoue.</span><span class="sxs-lookup"><span data-stu-id="0039d-196">With the above code changes, you can change your security profile (for example, by changing the state of **Two Factor Enabled**) and you will be logged out in 5 seconds when the `SecurityStampValidator.OnValidateIdentity` method fails.</span></span> <span data-ttu-id="0039d-197">Supprimer la ligne de retour dans le `SignInAsync` (méthode), modifiez un autre profil de sécurité et que vous n’allez pas être déconnecté. Le `SignInAsync` méthode génère un nouveau cookie de sécurité.</span><span class="sxs-lookup"><span data-stu-id="0039d-197">Remove the return line in the `SignInAsync` method, make another security profile change and you will not be logged out. The `SignInAsync` method generates a new security cookie.</span></span>

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a><span data-ttu-id="0039d-198">Activer l’authentification à deux facteurs</span><span class="sxs-lookup"><span data-stu-id="0039d-198">Enable two-factor authentication</span></span>

<span data-ttu-id="0039d-199">Dans l’exemple d’application, vous devez utiliser l’interface utilisateur pour activer l’authentification à deux facteurs (2FA).</span><span class="sxs-lookup"><span data-stu-id="0039d-199">In the sample app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="0039d-200">Pour activer 2FA, cliquez sur votre nom d’utilisateur (alias de messagerie) dans la barre de navigation.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="0039d-200">To enable 2FA, click on your user ID (email alias) in the navigation bar.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span></span>  
<span data-ttu-id="0039d-201">Cliquez sur Activer 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="0039d-201">Click on enable 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span></span> <span data-ttu-id="0039d-202">Déconnectez-vous, puis reconnectez-vous.</span><span class="sxs-lookup"><span data-stu-id="0039d-202">Log out, then log back in.</span></span> <span data-ttu-id="0039d-203">Si vous avez activé la messagerie (voir mon [didacticiel précédent](account-confirmation-and-password-recovery-with-aspnet-identity.md)), vous pouvez sélectionner les SMS ou par courrier électronique pour 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="0039d-203">If you've enabled email (see my [previous tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span></span> <span data-ttu-id="0039d-204">La page de codes de vérification s’affiche où vous pouvez saisir le code (à partir de SMS ou par courrier électronique).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="0039d-204">The Verify Code page is displayed where you can enter the code (from SMS or email).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span></span> <span data-ttu-id="0039d-205">En cliquant sur le **n’oubliez pas de ce navigateur** case à cocher vous exempter de devoir utiliser 2FA pour vous connecter avec cet ordinateur et le navigateur.</span><span class="sxs-lookup"><span data-stu-id="0039d-205">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on with that computer and browser.</span></span> <span data-ttu-id="0039d-206">L’activation de 2FA et en cliquant sur le **n’oubliez pas de ce navigateur** vous fournira fort 2FA protection contre les utilisateurs malveillants, essayez d’accéder à votre compte, tant qu’ils n’ont pas accès à votre ordinateur.</span><span class="sxs-lookup"><span data-stu-id="0039d-206">Enabling 2FA and clicking on the **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your computer.</span></span> <span data-ttu-id="0039d-207">Pour cela, sur n’importe quel ordinateur privé que vous utilisez régulièrement.</span><span class="sxs-lookup"><span data-stu-id="0039d-207">You can do this on any private machine you regularly use.</span></span> <span data-ttu-id="0039d-208">En définissant **n’oubliez pas de ce navigateur**, vous obtenez la sécurité renforcée de 2FA à partir d’ordinateurs que vous n’utilisez pas régulièrement, et la commodité sur ne pas avoir à passer par 2FA sur vos propres ordinateurs.</span><span class="sxs-lookup"><span data-stu-id="0039d-208">By setting **Remember this browser**, you get the added security of 2FA from computers you don't regularly use, and you get the convenience on not having to go through 2FA on your own computers.</span></span> 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a><span data-ttu-id="0039d-209">Comment inscrire un fournisseur d’authentification à deux facteurs</span><span class="sxs-lookup"><span data-stu-id="0039d-209">How to register a Two-factor authentication provider</span></span>

<span data-ttu-id="0039d-210">Lorsque vous créez un projet MVC, la *IdentityConfig.cs* fichier contient le code suivant pour inscrire un fournisseur d’authentification à deux facteurs :</span><span class="sxs-lookup"><span data-stu-id="0039d-210">When you create a new MVC project, the *IdentityConfig.cs* file contains the following code to register a Two-factor authentication provider:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a><span data-ttu-id="0039d-211">Ajouter un numéro de téléphone pour 2FA</span><span class="sxs-lookup"><span data-stu-id="0039d-211">Add a phone number for 2FA</span></span>

<span data-ttu-id="0039d-212">Le `AddPhoneNumber` méthode d’action dans le `Manage` génère un jeton de sécurité et l’envoie à téléphone numéro que vous avez fourni.</span><span class="sxs-lookup"><span data-stu-id="0039d-212">The `AddPhoneNumber` action method in the `Manage` controller generates a security token and sends it to the phone number you have provided.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

<span data-ttu-id="0039d-213">Après avoir envoyé le jeton, il redirige vers le `VerifyPhoneNumber` méthode d’action, où vous pouvez entrer le code pour inscrire des SMS pour 2FA.</span><span class="sxs-lookup"><span data-stu-id="0039d-213">After sending the token, it redirects to the `VerifyPhoneNumber` action method, where you can enter the code to register SMS for 2FA.</span></span> <span data-ttu-id="0039d-214">SMS 2FA n’est pas utilisé jusqu'à ce que vous avez vérifié le numéro de téléphone.</span><span class="sxs-lookup"><span data-stu-id="0039d-214">SMS 2FA is not used until you have verified the phone number.</span></span>

## <a name="enabling-2fa"></a><span data-ttu-id="0039d-215">L’activation de 2FA</span><span class="sxs-lookup"><span data-stu-id="0039d-215">Enabling 2FA</span></span>

<span data-ttu-id="0039d-216">Le `EnableTFA` 2FA permet de méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="0039d-216">The `EnableTFA` action method enables 2FA:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

<span data-ttu-id="0039d-217">Remarque la `SignInAsync` doit être appelé pour activer 2FA étant une modification pour le profil de sécurité.</span><span class="sxs-lookup"><span data-stu-id="0039d-217">Note the `SignInAsync` must be called because enable 2FA is a change to the security profile.</span></span> <span data-ttu-id="0039d-218">Lorsque 2FA est activé, l’utilisateur devra utiliser 2FA pour vous connecter, à l’aide de l’une des approches 2FA qu’ils ont enregistrés (SMS et par courrier électronique dans l’exemple).</span><span class="sxs-lookup"><span data-stu-id="0039d-218">When 2FA is enabled, the user will need to use 2FA to log in, using the 2FA approaches they have registered (SMS and email in the sample).</span></span>

<span data-ttu-id="0039d-219">Vous pouvez ajouter plusieurs fournisseurs 2FA tels que des générateurs de code QR ou vous pouvez écrire vous possédez (consultez [à l’aide de Google Authenticator avec ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span><span class="sxs-lookup"><span data-stu-id="0039d-219">You can add more 2FA providers such as QR code generators or you can write you own (See [Using Google Authenticator with ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span></span>

> [!NOTE]
> <span data-ttu-id="0039d-220">Les codes 2FA sont générés à l’aide de [algorithme de mot de passe à usage unique temporels](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) et les codes sont valides pendant six minutes.</span><span class="sxs-lookup"><span data-stu-id="0039d-220">The 2FA codes are generated using [Time-based One-time Password Algorithm](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) and codes are valid for six minutes.</span></span> <span data-ttu-id="0039d-221">Si vous mettez plus de six minutes à entrer le code, vous obtenez un message d’erreur de code non valide.</span><span class="sxs-lookup"><span data-stu-id="0039d-221">If you take more than six minutes to enter the code, you'll get an Invalid code error message.</span></span>


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="0039d-222">Combiner des comptes de connexion de réseaux sociaux et local</span><span class="sxs-lookup"><span data-stu-id="0039d-222">Combine social and local login accounts</span></span>

<span data-ttu-id="0039d-223">Vous pouvez combiner les comptes locaux et réseaux sociaux en cliquant sur le lien de votre messagerie.</span><span class="sxs-lookup"><span data-stu-id="0039d-223">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="0039d-224">Dans l’ordre suivant &quot; RickAndMSFT@gmail.com &quot; est tout d’abord créé en tant qu’une connexion locale, mais vous pouvez créer le compte en tant qu’un journal social en premier, puis ajouter une connexion locale.</span><span class="sxs-lookup"><span data-stu-id="0039d-224">In the following sequence &quot;RickAndMSFT@gmail.com&quot; is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

<span data-ttu-id="0039d-225">Cliquez sur le **gérer** lien.</span><span class="sxs-lookup"><span data-stu-id="0039d-225">Click on the **Manage** link.</span></span> <span data-ttu-id="0039d-226">Notez la valeur 0 externe (connexions sociales) associé à ce compte.</span><span class="sxs-lookup"><span data-stu-id="0039d-226">Note the 0 external (social logins) associated with this account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

<span data-ttu-id="0039d-227">Cliquez sur le lien vers un autre journal dans le service et accepter les demandes d’application.</span><span class="sxs-lookup"><span data-stu-id="0039d-227">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="0039d-228">Les deux comptes ont été combinés, vous ne pourrez pas vous connecter avec un compte.</span><span class="sxs-lookup"><span data-stu-id="0039d-228">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="0039d-229">Vous pourriez vos utilisateurs pour ajouter des comptes locaux au cas où le journal des réseaux sociaux dans le service d’authentification est arrêté, ou plus probable qu’ils ont perdu l’accès à leur compte sociaux.</span><span class="sxs-lookup"><span data-stu-id="0039d-229">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="0039d-230">Dans l’image suivante, Tom est un journal des réseaux sociaux dans (que vous pouvez visualiser à partir de la **Logins externes : 1** présentée sur la page).</span><span class="sxs-lookup"><span data-stu-id="0039d-230">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

<span data-ttu-id="0039d-231">En cliquant sur **choisir un mot de passe** vous permet d’ajouter un journal local sur avec le même compte.</span><span class="sxs-lookup"><span data-stu-id="0039d-231">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a><span data-ttu-id="0039d-232">Verrouillage de compte à partir des attaques en force brute</span><span class="sxs-lookup"><span data-stu-id="0039d-232">Account lockout from brute force attacks</span></span>

<span data-ttu-id="0039d-233">Vous pouvez protéger les comptes sur votre application contre les attaques de dictionnaire en activant le verrouillage de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="0039d-233">You can protect the accounts on your app from dictionary attacks by enabling user lockout.</span></span> <span data-ttu-id="0039d-234">Le code suivant dans la `ApplicationUserManager Create` méthode configure de verrouillage :</span><span class="sxs-lookup"><span data-stu-id="0039d-234">The following code in the `ApplicationUserManager Create` method configures lock out:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

<span data-ttu-id="0039d-235">Le code ci-dessus permet le verrouillage pour l’authentification à deux facteurs uniquement.</span><span class="sxs-lookup"><span data-stu-id="0039d-235">The code above enables lockout for two factor authentication only.</span></span> <span data-ttu-id="0039d-236">Pendant que vous pouvez activer le verrouillage pour les connexions en modifiant `shouldLockout` à true dans le `Login` méthode du contrôleur de compte, nous vous recommandons de vous ne permettent pas de verrouillage pour les connexions, car elle rend le compte vulnérable aux [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) connexion attaques.</span><span class="sxs-lookup"><span data-stu-id="0039d-236">While you can enable lock out for logins by changing `shouldLockout` to true in the `Login` method of the account controller, we recommend you not enable lock out for logins because it makes the account susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) login attacks.</span></span> <span data-ttu-id="0039d-237">Dans l’exemple de code, le verrouillage est désactivé pour le compte administrateur créé dans le `ApplicationDbInitializer Seed` méthode :</span><span class="sxs-lookup"><span data-stu-id="0039d-237">In the sample code, lockout is disabled for the admin account created in the `ApplicationDbInitializer Seed` method:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a><span data-ttu-id="0039d-238">Exiger un utilisateur possède un compte de messagerie validé</span><span class="sxs-lookup"><span data-stu-id="0039d-238">Requiring a user to have a validated email account</span></span>

<span data-ttu-id="0039d-239">Le code suivant requiert un utilisateur d’avoir un compte de messagerie validé avant de se connecter :</span><span class="sxs-lookup"><span data-stu-id="0039d-239">The following code requires a user to have a validated email account before they can log in:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a><span data-ttu-id="0039d-240">Comment SignInManager vérifie pour la condition de 2FA</span><span class="sxs-lookup"><span data-stu-id="0039d-240">How SignInManager checks for 2FA requirement</span></span>

<span data-ttu-id="0039d-241">Les journaux locaux dans et journal sociaux dans, vérifiez si 2FA est activé.</span><span class="sxs-lookup"><span data-stu-id="0039d-241">Both the local log in and social log in check to see if 2FA is enabled.</span></span> <span data-ttu-id="0039d-242">Si 2FA est activé, le `SignInManager` méthode d’ouverture de session retourne `SignInStatus.RequiresVerification`, et l’utilisateur sera redirigé vers la `SendCode` méthode d’action, où ils doivent entrer le code pour terminer le journal dans la séquence.</span><span class="sxs-lookup"><span data-stu-id="0039d-242">If 2FA is enabled, the `SignInManager` logon method returns `SignInStatus.RequiresVerification`, and the user will be redirected to the `SendCode` action method, where they will have to enter the code to complete the log in sequence.</span></span> <span data-ttu-id="0039d-243">Si l’utilisateur a RememberMe est défini sur le cookie d’utilisateurs locaux, les `SignInManager` retournera `SignInStatus.Success` et ils n’ont pas passer par 2FA.</span><span class="sxs-lookup"><span data-stu-id="0039d-243">If the user has RememberMe is set on the users local cookie, the `SignInManager` will return `SignInStatus.Success` and they will not have to go through 2FA.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

<span data-ttu-id="0039d-244">Le code suivant illustre la `SendCode` méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="0039d-244">The following code shows the `SendCode` action method.</span></span> <span data-ttu-id="0039d-245">A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) est créée avec toutes les méthodes 2FA activées pour l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="0039d-245">A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is created with all the 2FA methods enabled for the user.</span></span> <span data-ttu-id="0039d-246">Le [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) est passé à la [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) assistance, ce qui permet à l’utilisateur à sélectionner l’approche 2FA (généralement par courrier électronique et SMS).</span><span class="sxs-lookup"><span data-stu-id="0039d-246">The [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is passed to the [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) helper, which allows the user to select the 2FA approach (typically email and SMS).</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

<span data-ttu-id="0039d-247">Une fois que l’utilisateur enregistre l’approche 2FA, le `HTTP POST SendCode` l’appel de méthode d’action, le `SignInManager` envoie le code 2FA et l’utilisateur est redirigé vers la `VerifyCode` méthode d’action dans lequel ils peuvent entrer le code pour terminer la connexion.</span><span class="sxs-lookup"><span data-stu-id="0039d-247">Once the user posts the 2FA approach, the `HTTP POST SendCode` action method is called, the `SignInManager` sends the 2FA code, and the user is redirected to the `VerifyCode` action method where they can enter the code to complete the log in.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a><span data-ttu-id="0039d-248">Verrouillage de 2FA</span><span class="sxs-lookup"><span data-stu-id="0039d-248">2FA Lockout</span></span>

<span data-ttu-id="0039d-249">Bien que vous pouvez définir le verrouillage de compte sur les échecs de tentatives de mot de passe de connexion, cette approche rend votre connexion susceptibles d’être [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) verrouillages.</span><span class="sxs-lookup"><span data-stu-id="0039d-249">Although you can set account lockout on login password attempt failures, that approach makes your login susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) lockouts.</span></span> <span data-ttu-id="0039d-250">Nous vous recommandons de qu'utiliser le verrouillage de compte uniquement avec 2FA.</span><span class="sxs-lookup"><span data-stu-id="0039d-250">We recommend you use account lockout only with 2FA.</span></span> <span data-ttu-id="0039d-251">Lorsque le `ApplicationUserManager` est créé, l’exemple de code définit le verrouillage de 2FA et `MaxFailedAccessAttemptsBeforeLockout` à cinq.</span><span class="sxs-lookup"><span data-stu-id="0039d-251">When the `ApplicationUserManager` is created, the sample code sets 2FA lockout and `MaxFailedAccessAttemptsBeforeLockout` to five.</span></span> <span data-ttu-id="0039d-252">Une fois qu’un utilisateur se connecte (via un compte local ou compte social), chaque tentative ayant échoué 2FA est stocké, et si le nombre maximal de tentatives est atteint, l’utilisateur est verrouillé pendant cinq minutes (vous pouvez définir l’heure avec verrouillage `DefaultAccountLockoutTimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="0039d-252">Once a user logs in (through local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span>

<a id="addRes"></a>

## <a name="additional-resources"></a><span data-ttu-id="0039d-253">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="0039d-253">Additional Resources</span></span>

- <span data-ttu-id="0039d-254">[ASP.NET Identity recommandé ressources](../getting-started/aspnet-identity-recommended-resources.md) la liste complète des blogs, vidéos, didacticiels et great identité lie donc.</span><span class="sxs-lookup"><span data-stu-id="0039d-254">[ASP.NET Identity Recommended Resources](../getting-started/aspnet-identity-recommended-resources.md) Complete list of Identity blogs, videos, tutorials and great SO links.</span></span>
- <span data-ttu-id="0039d-255">[Application MVC est 5 avec Facebook, Twitter, LinkedIn et d’authentification Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) montre également comment ajouter des informations de profil à la table des utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="0039d-255">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) also shows how to add profile information to the users table.</span></span>
- <span data-ttu-id="0039d-256">[ASP.NET MVC et identité 2.0 : comprendre les aspects fondamentaux](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) par John Atten.</span><span class="sxs-lookup"><span data-stu-id="0039d-256">[ASP.NET MVC and Identity 2.0: Understanding the Basics](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) by John Atten.</span></span>
- [<span data-ttu-id="0039d-257">Confirmation du compte et la récupération de mot de passe avec l’identité de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0039d-257">Account Confirmation and Password Recovery with ASP.NET Identity</span></span>](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [<span data-ttu-id="0039d-258">Introduction à ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="0039d-258">Introduction to ASP.NET Identity</span></span>](../getting-started/introduction-to-aspnet-identity.md)
- <span data-ttu-id="0039d-259">[Annonce de la version RTM d’ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) par Pranav Rastogi.</span><span class="sxs-lookup"><span data-stu-id="0039d-259">[Announcing RTM of ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) by Pranav Rastogi.</span></span>
- <span data-ttu-id="0039d-260">[Identité ASP.NET 2.0 : Configuration de la Validation du compte et l’autorisation de deux facteurs](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) par John Atten.</span><span class="sxs-lookup"><span data-stu-id="0039d-260">[ASP.NET Identity 2.0: Setting Up Account Validation and Two-Factor Authorization](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) by John Atten.</span></span>
