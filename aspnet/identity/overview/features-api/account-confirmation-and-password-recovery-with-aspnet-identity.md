---
uid: identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
title: Confirmation de compte et récupération de mot de passe avec ASP.NET Identity (c#) | Microsoft Docs
author: HaoK
description: Avant d’effectuer ce didacticiel, que vous devez effectuer tout d’abord créer une application web de ASP.NET MVC 5 sécurisée avec connexion, réinitialisation de confirmation et le mot de passe de messagerie. Ce didacticiel...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: 8d54180d-f826-4df7-b503-7debf5ed9fb3
ms.technology: ''
msc.legacyurl: /identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 38b908d145986102ff1b1734cdcef75b320bd0ab
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384129"
---
<a name="account-confirmation-and-password-recovery-with-aspnet-identity-c"></a>Confirmation de compte et de récupération de mot de passe avec ASP.NET Identity (c#)
====================
par [arts martiaux Hao](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Avant de procéder à ce didacticiel vous devez commencer par suivre [créer une application web de ASP.NET MVC 5 sécurisée avec connexion, réinitialisation de confirmation et le mot de passe de messagerie](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md). Ce didacticiel contient plus de détails et vous montrera comment paramétrer la messagerie électronique de confirmation de compte local et aux utilisateurs de réinitialiser leur mot de passe oublié dans ASP.NET Identity. Cet article a été écrit par Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), arts martiaux Hao et Suhas Joshi. L’exemple de NuGet a été écrit principalement par arts martiaux Hao.


Un compte d’utilisateur local, l’utilisateur doit créer un mot de passe pour le compte et ce mot de passe est stocké (en toute sécurité) dans l’application web. ASP.NET Identity prend également en charge les comptes de réseaux sociaux, qui ne nécessitent pas l’utilisateur de créer un mot de passe pour l’application. [Comptes de réseaux sociaux](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) utiliser un tiers (par exemple, Google, Twitter, Facebook ou Microsoft) pour authentifier les utilisateurs. Cette rubrique aborde les thèmes suivants :

- [Créer une application ASP.NET MVC](#createMvc) et Explorer les fonctionnalités d’identité ASP.NET.
- [Création de l’exemple d’identité](#build)
- [Configurer la confirmation par courrier électronique](#email)

Nouveaux utilisateurs inscrivent leurs alias de messagerie, ce qui crée un compte local.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image1.png)

En cliquant sur le bouton inscrire envoie un e-mail de confirmation contenant un jeton de validation à une adresse e-mail.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image2.png)

L’utilisateur reçoit un e-mail avec un jeton de confirmation pour leur compte.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image3.png)

Cliquant sur le lien confirme le compte.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image4.png)

<a id="passwordReset"></a>

## <a name="password-recoveryreset"></a>Récupération de mot de passe/réinitialisation

Les utilisateurs locaux qui oublient leur mot de passe peuvent avoir un jeton de sécurité envoyé à son compte de messagerie, ce qui leur permet de réinitialiser leur mot de passe.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image5.png)  
  
L’utilisateur sera bientôt un e-mail avec un lien leur permettant de réinitialiser leur mot de passe.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image6.png)  
Cliquant sur le lien mène à la page de réinitialisation.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image7.png)  
  
En cliquant sur le **réinitialiser** bouton pour confirmer le mot de passe a été réinitialisé.  
  
![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image8.png)

<a id="createMvc"></a>

## <a name="create-an-aspnet-web-app"></a>Créer une application Web ASP.NET

Démarrez en installant et en cours d’exécution [Visual Studio Express 2013 pour le Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installer Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) ou une version ultérieure.

> [!NOTE]
> Avertissement : Vous devez installer Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) pour suivre ce didacticiel.


1. Créez un projet Web ASP.NET et sélectionnez le modèle MVC. Web Forms prend également en charge ASP.NET Identity : vous pouvez donc suivre des étapes similaires dans une application Web Forms.
2. Laissez l’authentification par défaut à **Comptes d’utilisateur individuels**.
3. Exécutez l’application, cliquez sur le lien **S'inscrire** et inscrivez un utilisateur. À ce stade, la seule validation sur l’adresse e-mail se fait avec l'attribut [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx).
4. Dans l’Explorateur de serveurs, accédez à **Data Connections\DefaultConnection\Tables\AspNetUsers**, cliquez avec le bouton droit et sélectionnez **Ouvrir la définition de la table**.

    L’illustration suivante montre le schéma `AspNetUsers` :

    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image9.png)
5. Cliquez avec le bouton droit sur la table **AspNetUsers** et sélectionnez **Afficher les données de la table**.  
  
    ![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image10.png)  
  
   À ce stade, l’adresse de messagerie n’a pas été confirmée.


Le magasin de données par défaut pour ASP.NET Identity est Entity Framework, mais vous pouvez le configurer pour utiliser d’autres magasins de données et ajouter des champs supplémentaires. Consultez [des ressources supplémentaires](#addRes) section à la fin de ce didacticiel.

Le [classe de démarrage OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) ( *Startup.cs* ) est appelée lorsque l’application démarre et appelle le `ConfigureAuth` méthode dans *application\_Start\Startup.Auth.cs*, qui configure le pipeline OWIN et initialise l’identité ASP.NET. Examiner le `ConfigureAuth` (méthode). Chaque `CreatePerOwinContext` appel enregistre un rappel (enregistré dans le `OwinContext`) qui sera appelé une fois par demande pour créer une instance du type spécifié. Vous pouvez définir un point d’arrêt dans le constructeur et `Create` de chaque type (méthode) (`ApplicationDbContext, ApplicationUserManager`) et vérifiez qu’ils sont appelés à chaque demande. Une instance de `ApplicationDbContext` et `ApplicationUserManager` est stocké dans le contexte OWIN, qui est accessible dans toute l’application. ASP.NET Identity raccorde le pipeline OWIN via middleware de cookie. Pour plus d’informations, consultez [par la gestion des demandes de durée de vie pour la classe UserManager dans ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx).

Lorsque vous modifiez votre profil de sécurité, un nouvel horodatage de sécurité est généré et stocké dans le `SecurityStamp` champ la *AspNetUsers* table. Notez que le `SecurityStamp` champ diffère le cookie de sécurité. Le cookie de sécurité n’est pas stocké dans le `AspNetUsers` table (ou tout autre emplacement de la base de données d’identité). Le jeton de cookie de sécurité est auto-signé à l’aide de [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) et est créé avec le `UserId, SecurityStamp` et les informations d’heure d’expiration.

Le middleware de cookie vérifie le cookie à chaque demande. Le `SecurityStampValidator` méthode dans le `Startup` classe accède à la base de données et vérifie le tampon de sécurité régulièrement, comme spécifié par le `validateInterval`. Cela se produit uniquement toutes les 30 minutes (dans notre exemple), sauf si vous modifiez votre profil de sécurité. L’intervalle de 30 minutes a été choisi pour réduire les allers-retours vers la base de données. Consultez mon [didacticiel d’authentification à deux facteurs](index.md) pour plus d’informations.

Par les commentaires dans le code, le `UseCookieAuthentication` méthode prend en charge l’authentification des cookies. Le `SecurityStamp` code de champ et associé fournit une couche supplémentaire de sécurité à votre application, lorsque vous modifiez votre mot de passe, vous serez connecté en dehors du navigateur que vous êtes connecté. Le `SecurityStampValidator.OnValidateIdentity` méthode permet à l’application valider le jeton de sécurité lors de l’utilisateur se connecte, qui est utilisée lorsque vous modifiez un mot de passe ou utilisez la connexion externe. Cela est nécessaire pour vous assurer que tous les jetons (cookies) générés avec l’ancien mot de passe sont invalidés. Dans l’exemple de projet, si vous modifiez le mot de passe utilisateurs, puis un nouveau jeton est généré pour l’utilisateur, tous les jetons précédentes sont invalidées et `SecurityStamp` champ est mis à jour.

Le système d’identité vous autorise à configurer votre application, lorsque le profil de sécurité des utilisateurs change (par exemple, lorsque l’utilisateur modifie son mot de passe ou les modifications associé nom de connexion (par exemple, de Facebook, Google, compte Microsoft, etc.), l’utilisateur est déconnecté tous les instances de navigateur. Par exemple, l’image ci-dessous montre le [exemple de déconnexion unique](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) application, ce qui permet à l’utilisateur de se déconnecter de toutes les instances de navigateur (dans ce cas, Internet Explorer, Firefox et Chrome) en cliquant sur un bouton. Vous pouvez également l’exemple vous permet d’enregistrer uniquement en dehors d’une instance du navigateur spécifique.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image11.png)

Le [exemple de déconnexion unique](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SingleSignOutSample/readme.txt) application montre comment ASP.NET Identity vous permet de régénérer le jeton de sécurité. Cela est nécessaire pour vous assurer que tous les jetons (cookies) générés avec l’ancien mot de passe sont invalidés. Cette fonctionnalité fournit une couche supplémentaire de sécurité à votre application ; Lorsque vous modifiez votre mot de passe, vous allez être déconnecté d’où vous êtes connecté à cette application.

Le *application\_Start\IdentityConfig.cs* fichier contient le `ApplicationUserManager`, `EmailService` et `SmsService` classes. Le `EmailService` et `SmsService` classes chaque implémentent la `IIdentityMessageService` interface, afin d’avoir des méthodes courantes dans chaque classe pour configurer l’e-mail et SMS. Bien que ce didacticiel montre uniquement comment ajouter la notification par courrier électronique via [SendGrid](http://sendgrid.com/), vous pouvez envoyer des e-mails à l’aide de SMTP et autres mécanismes.

Le `Startup` classe contient également standard pour ajouter des connexions sociales (Facebook, Twitter, etc.), consultez mon didacticiel [application MVC 5 avec Facebook, Twitter, LinkedIn et authentification Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) pour plus d’informations.

Examiner la `ApplicationUserManager` classe, qui contient les informations d’identité utilisateurs et configure les fonctionnalités suivantes :

- Exigences de force de mot de passe.
- Verrouillage utilisateur (tentatives et l’heure).
- Authentification à deux facteurs (2FA). Je vais aborder 2FA et SMS dans un autre didacticiel.
- Raccorder l’e-mail et les services SMS. (J’aborderai SMS dans un autre didacticiel).

Le `ApplicationUserManager` classe est dérivée de l’objet générique `UserManager<ApplicationUser>` classe. `ApplicationUser` dérive de [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser.aspx). `IdentityUser` dérive le générique `IdentityUser` classe :

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample1.cs)]

Les propriétés ci-dessus coïncident avec les propriétés dans le `AspNetUsers` table, ci-dessus.

Les arguments génériques sur `IUser` vous permettent de dériver une classe à l’aide de différents types pour la clé primaire. Consultez le [ChangePK](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) exemple qui montre comment modifier la clé primaire à partir de la chaîne en int ou GUID.

### <a name="applicationuser"></a>ApplicationUser

`ApplicationUser` (`public class ApplicationUserManager : UserManager<ApplicationUser>`) est défini dans *Models\IdentityModels.cs* en tant que :

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample2.cs?highlight=8-9)]

Le code en surbrillance ci-dessus génère un [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). ASP.NET Identity et l’authentification des cookies OWIN basée sur les revendications, par conséquent, le framework requiert l’application pour générer un `ClaimsIdentity` pour l’utilisateur. `ClaimsIdentity` contient des informations sur toutes les revendications pour l’utilisateur, telles que le nom d’utilisateur, l’âge et les rôles de l’utilisateur appartient. Vous pouvez également ajouter plusieurs revendications de l’utilisateur à ce stade.

OWIN `AuthenticationManager.SignIn` méthode passe dans le `ClaimsIdentity` et se connecte l’utilisateur :

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample3.cs?highlight=4-6)]

[Application MVC 5 avec Facebook, Twitter, LinkedIn et authentification Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) montre comment vous pouvez ajouter des propriétés supplémentaires à la `ApplicationUser` classe.

## <a name="email-confirmation"></a>E-mail de confirmation

Il est judicieux de confirmer l’adresse de messagerie un nouvel utilisateur auprès de vérifier qu’ils ne sont pas emprunter l’identité d’une autre personne (autrement dit, ils n’ont pas inscrit auprès de quelqu'un d’autre e-mail). Supposons que vous ayez un forum de discussion : vous voudriez empêcher `"bob@example.com"` de s'inscrire en tant que `"joe@contoso.com"`. ans la confirmation par courrier électronique, `"joe@contoso.com"` pourrait recevoir du courrier indésirable à partir de votre application. Supposons que Bob se soit accidentellement inscrit en tant que `"bib@example.com"` et ne l'ait pas remarqué, il ne serait pas en mesure d’utiliser le mot de passe de récupération, car l’application n’. E-mail de confirmation fournit uniquement une protection limitée des robots et ne fournit pas une protection à partir des expéditeurs de courrier indésirable déterminés, ils ont des alias de messagerie travail nombreux, qu'ils peuvent utiliser pour inscrire. Dans l’exemple ci-dessous, l’utilisateur ne pourra modifier son mot de passe jusqu'à ce que son compte a été confirmé (par ces derniers en cliquant sur un lien de confirmation reçu sur le compte de messagerie ils enregistrés avec.) Vous pouvez appliquer ce flux de travail à d’autres scénarios, par exemple envoyer un lien pour confirmer et réinitialiser le mot de passe sur les nouveaux comptes créés par l’administrateur, en envoyant à l’utilisateur un e-mail lorsque qu’ils ont modifié leur profil et ainsi de suite. En règle générale, vous souhaitez empêcher les nouveaux utilisateurs d'envoyer des données à votre site web avant qu’ils soient confirmés par courrier électronique, par SMS ou via un autre mécanisme. <a id="build"></a>

## <a name="building-a-more-complete-sample"></a>Création d’un exemple plus complet

Dans cette section, vous allez utiliser NuGet pour télécharger un exemple plus complet, de que nous nous efforcerons.

1. Créer un nouveau ***vide*** projet Web ASP.NET.
2. Dans la Console du Gestionnaire de Package, entrez ce qui suit les commandes suivantes : 

    [!code-console[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample4.cmd)]

   Dans ce didacticiel, nous allons utiliser [SendGrid](http://sendgrid.com/) pour envoyer un e-mail. Le `Identity.Samples` package installe le code que nous travaillons avec.
3. Définir le [projet pour utiliser SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Tester la création du compte local en exécutant l’application, en cliquant sur le **inscrire** de liaison et de publier le formulaire d’inscription.
5. Cliquez sur le lien de messagerie de démonstration, qui simule l’e-mail de confirmation.
6. Supprimer le code de confirmation de lien e-mail démonstration de l’exemple (le `ViewBag.Link` code dans le contrôleur de compte. Consultez le `DisplayEmail` et `ForgotPasswordConfirmation` méthodes d’action et les vues razor).

> [!NOTE]
> Avertissement : Si vous modifiez les paramètres de sécurité dans cet exemple, les applications de productions devez sont soumis à un audit de sécurité qui appelle explicitement les modifications apportées.


## <a name="examine-the-code-in-appstartidentityconfigcs"></a>Examinez le code dans l’application\_Start\IdentityConfig.cs

L’exemple montre comment créer un compte et l’ajouter à la *administrateur* rôle. Vous devez remplacer l’e-mail dans l’exemple avec l’e-mail que vous utiliserez pour le compte d’administrateur. Le moyen le plus simple pour créer un compte d’administrateur est par programmation dans le `Seed` (méthode). Nous espérons que dans le futur un outil qui vous permet de créer et administrer des utilisateurs et des rôles. L’exemple de code vous permet à créer et gérer des utilisateurs et des rôles, mais vous devez disposer d’un compte d’administrateurs pour exécuter les rôles et les pages d’administration utilisateur. Dans cet exemple, le compte d’administrateur est créé lors de la base de données est amorcée.

Remplacez le mot de passe et le nom à un compte dans lequel vous pouvez recevoir des notifications par courrier électronique.

> [!WARNING]
> Sécurité - Ne jamais stocker de données sensibles dans votre code source.

Comme mentionné précédemment, le `app.CreatePerOwinContext` appel de la classe startup ajoute des rappels pour le `Create` méthode l’application DB contenus, manager et le rôle Gestionnaire de classes d’utilisateurs. Appels de pipeline OWIN le `Create` méthode sur ces classes pour chaque demande et stocke le contexte pour chaque classe. Le contrôleur de compte expose le Gestionnaire des utilisateurs à partir du contexte HTTP (qui contient le contexte OWIN) :

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample5.cs)]

Lorsqu’un utilisateur s’inscrit à un compte local, le `HTTP Post Register` méthode est appelée :

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample6.cs)]

Le code ci-dessus utilise les données du modèle pour créer un nouveau compte d’utilisateur à l’aide de l’e-mail et le mot de passe entré. Si l’alias de messagerie se trouve dans le magasin de données, la création du compte échoue et le formulaire s’affiche à nouveau. Le `GenerateEmailConfirmationTokenAsync` méthode crée un jeton de confirmation sécurisé et les stocke dans le magasin de données d’identité ASP.NET. Le [Url.Action](https://msdn.microsoft.com/library/dd505232(v=vs.118).aspx) méthode crée un lien contenant le `UserId` et jeton de confirmation. Ce lien est ensuite envoyé par e-mail à l’utilisateur, l’utilisateur peut cliquer sur le lien dans leur application de messagerie pour confirmer son compte.

<a id="email"></a>

## <a name="set-up-email-confirmation"></a>Configurer la confirmation par courrier électronique

Accédez à la [page d’inscription Azure SendGrid](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) et vous inscrire gratuitement compte. Ajoutez un code similaire à ce qui suit pour configurer SendGrid :

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample7.cs?highlight=5)]

> [!NOTE]
> Souvent, les clients de messagerie acceptent uniquement les messages de texte (aucun HTML). Vous devez fournir le message en texte et HTML. Dans l’exemple de SendGrid ci-dessus, cela se fait par le `myMessage.Text` et `myMessage.Html` code ci-dessus.


Le code suivant montre comment envoyer des e-mails à l’aide du [MailMessage](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) classe where `message.Body` retourne uniquement le lien.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample8.cs)]

> [!WARNING]
> Sécurité - Ne jamais stocker de données sensibles dans votre code source. Le compte et les informations d’identification sont stockés dans l’appSetting. Sur Azure, vous pouvez stocker en toute sécurité ces valeurs sur l'onglet **[Configurer](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** dans le portail Azure. Consultez [les meilleures pratiques pour le déploiement des mots de passe et autres données sensibles sur ASP.NET et Azure](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


Entrez vos informations d’identification SendGrid, exécutez l’application, s’enregistrer auprès d’un alias de messagerie pouvez cliquer sur le lien de confirmation dans votre adresse de messagerie. Pour voir comment procéder avec votre [Outlook.com](http://outlook.com) compte de messagerie, consultez de John Atten [Configuration SMTP c# pour l’hôte SMTP de Outlook.Com](http://typecastexception.com/post/2013/12/20/C-SMTP-Configuration-for-OutlookCom-SMTP-Host.aspx) et son[ASP.NET Identity 2.0 : Validation de compte et l’autorisation de deux facteurs](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) valide.

Une fois qu’un utilisateur clique sur le **inscrire** bouton un e-mail de confirmation contenant un jeton de validation est envoyé à une adresse e-mail.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image12.png)

L’utilisateur reçoit un e-mail avec un jeton de confirmation pour leur compte.

![](account-confirmation-and-password-recovery-with-aspnet-identity/_static/image13.png)

## <a name="examine-the-code"></a>Examinez le code

Le code suivant montre la méthode `POST ForgotPassword`.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample9.cs)]

La méthode échoue sans avertissement si l’e-mail de l’utilisateur n’a pas été confirmée. Si une erreur a été validée pour une adresse de messagerie non valide, les utilisateurs malveillants peuvent utiliser ces informations pour rechercher le nom d’utilisateur valide (alias de messagerie) aux attaques.

Le code suivant illustre la `ConfirmEmail` méthode dans le contrôleur de compte qui est appelé lorsque l’utilisateur clique sur le lien de confirmation dans le message électronique qui leur sont envoyé :

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample10.cs)]

Une fois qu’un jeton de mot de passe oublié a été utilisé, elle est invalidée. Le changement de code suivant dans le `Create` (méthode) (dans le *application\_Start\IdentityConfig.cs* fichier) définit les jetons arrivent à expiration dans les 3 heures.

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample11.cs?highlight=6-8)]

Avec le code ci-dessus, le mot de passe oublié et les jetons de confirmation de courrier électronique va expirer dans les 3 heures. La valeur par défaut `TokenLifespan` est un jour.

Le code suivant illustre la méthode de confirmation de courrier électronique :

[!code-csharp[Main](account-confirmation-and-password-recovery-with-aspnet-identity/samples/sample12.cs)]

 Pour renforcer la sécurité de votre application, ASP.NET Identity prend en charge l’authentification à deux facteurs (2FA). Consultez [ASP.NET Identity 2.0 : configuration de la Validation du compte et l’autorisation de deux facteurs](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) par John Atten. Bien que vous pouvez définir le verrouillage de compte sur les échecs de tentative de mot de passe de connexion, cette approche rend votre identifiant de connexion susceptibles d’être [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) verrouillages. Nous vous recommandons de qu'utiliser le verrouillage de compte uniquement avec 2FA.  
<a id="addRes"></a>

## <a name="additional-resources"></a>Ressources supplémentaires

- [Vue d’ensemble des fournisseurs de stockage personnalisés pour ASP.NET Identity](../extensibility/overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Application MVC 5 avec Facebook, Twitter, LinkedIn et authentification Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) montre également comment ajouter des informations de profil à la table d’utilisateurs.
- [ASP.NET MVC et identité 2.0 : comprendre les notions de base](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) par John Atten.
- [Introduction à ASP.NET Identity](../getting-started/introduction-to-aspnet-identity.md)
- [Annonce de version RTM d’ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) Pranav rastogi.
