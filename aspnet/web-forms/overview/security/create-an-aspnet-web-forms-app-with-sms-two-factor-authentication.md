---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: Créer un site Web ASP.NET application Forms avec authentification à deux facteurs SMS (c#) | Microsoft Docs
author: Erikre
description: Ce didacticiel vous montre comment créer une application ASP.NET Web Forms avec authentification à deux facteurs. Ce didacticiel a été conçu pour compléter le didacticiel intitulé Cr...
ms.author: aspnetcontent
ms.date: 10/09/2014
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 16045b116ca5c797e7840f2ee5944e5f2c6282eb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803547"
---
<a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>Créer un site Web ASP.NET application Forms avec authentification à deux facteurs SMS (c#)
====================
par [Erik Reitan](https://github.com/Erikre)

[Télécharger l’application Web Forms ASP.NET avec messagerie et d’authentification à deux facteurs SMS](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> Ce didacticiel vous montre comment créer une application ASP.NET Web Forms avec authentification à deux facteurs. Ce didacticiel a été conçu pour compléter le didacticiel intitulé [créer une application Web Forms ASP.NET sécurisée avec inscription de l’utilisateur, un e-mail de confirmation et le mot de passe de la réinitialisation](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md). En outre, ce didacticiel est basé sur de Rick Anderson [didacticiel MVC](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).


## <a name="introduction"></a>Introduction

Ce didacticiel vous guide à travers les étapes requises pour créer une application Web Forms ASP.NET qui prend en charge l’authentification à deux facteurs à l’aide de Visual Studio. Authentification à deux facteurs est une étape d’authentification utilisateur supplémentaire. Cette étape supplémentaire génère un numéro unique d’identification personnel (PIN) pendant la connexion. Le code PIN est généralement envoyé à l’utilisateur par e-mail ou SMS. L’utilisateur de votre application entre ensuite le code confidentiel comme une mesure supplémentaire d’authentification lorsque vous vous connectez.

### <a name="tutorial-tasks-and-information"></a>Tâches de didacticiel et les informations sur :

- [Créer une application Web Forms ASP.NET](#createWebForms)
- [Le programme d’installation de SMS et authentification à deux facteurs](#SMS)
- [Activer l’authentification à deux facteurs pour l’utilisateur inscrit](#use2FA)
- [Ressources supplémentaires pour MSBuild](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Créer une application Web Forms ASP.NET

Démarrez en installant et en cours d’exécution [Visual Studio Express 2013 pour le Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installer [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou une version ultérieure ainsi. En outre, vous devez créer un [Twilio](https://www.twilio.com/try-twilio) de compte, comme expliqué ci-dessous.

> [!NOTE]
> Important : Vous devez installer [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou une version ultérieure pour suivre ce didacticiel.


1. Créez un projet (**fichier**  - &gt; **nouveau projet**) et sélectionnez le **Application Web ASP.NET** modèle ainsi que le .NET Framework la version 4.5.2 à partir de la **nouveau projet** boîte de dialogue.
2. À partir de la **nouveau projet ASP.NET** boîte de dialogue, sélectionnez le **Web Forms** modèle. Laissez l’authentification par défaut à **Comptes d’utilisateur individuels**. Ensuite, cliquez sur **OK** pour créer le nouveau projet.  
    ![Boîte de dialogue Nouveau projet ASP.NET](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. Activer Secure Sockets Layer (SSL) pour le projet. Suivez les étapes disponibles dans le **activer SSL pour le projet** section de la [mise en route de la série de didacticiels de Web Forms](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. Dans Visual Studio, ouvrez le **Console du Gestionnaire de Package** (**outils**  - &gt; **Gestionnaire de Package NuGet**  - &gt; **Console du Gestionnaire de package**), puis entrez la commande suivante :  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>Le programme d’installation de SMS et authentification à deux facteurs

Ce didacticiel utilise Twilio, mais vous pouvez utiliser n’importe quel fournisseur SMS.

1. Créer un [Twilio](https://www.twilio.com/try-twilio) compte.
2. À partir de la **tableau de bord** onglet de votre compte Twilio, copie le **SID de compte** et **du jeton d’authentification.** Vous allez ajouter les à votre application plus tard.
3. À partir de la **numéros** onglet, copiez votre Twilio **numéro de téléphone** également.
4. Le de twilio **SID de compte**, **du jeton d’authentification** et **numéro de téléphone** disponibles pour l’application. Pour simplifier les choses, vous allez stocker ces valeurs dans le *web.config* fichier. Lorsque vous déployez dans Azure, vous pouvez stocker les valeurs de manière sécurisée dans le **appSettings** section sur le site web de l’onglet Configurer. En outre, lorsque vous ajoutez le numéro de téléphone, utiliser uniquement des chiffres.   
   Notez que vous pouvez également ajouter des informations d’identification SendGrid. SendGrid est un service de notification de messagerie. Pour plus d’informations sur l’activation de SendGrid, consultez la section « Hook de SendGrid » du didacticiel intitulé [créer une application sécurisée ASP.NET Web Forms avec inscription de l’utilisateur, un e-mail de confirmation et le mot de passe de la réinitialisation.](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > Sécurité - Ne jamais stocker de données sensibles dans votre code source. Dans cet exemple, le compte et les informations d’identification sont stockées dans le **appSettings** section de la *Web.config* fichier. Sur Azure, vous pouvez stocker en toute sécurité ces valeurs sur l'onglet **[Configurer](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** dans le portail Azure. Pour plus d’informations, consultez rubrique de Rick Anderson [meilleures pratiques pour le déploiement des mots de passe et autres données sensibles sur ASP.NET et Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
5. Configurer le `SmsService` classe dans le *application\_Start\IdentityConfig.cs* fichier en effectuant ce qui suit est modifié en jaune : 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. Ajoutez le code suivant `using` instructions au début de la *IdentityConfig.cs* fichier : 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. Mise à jour le *Account/Manage.aspx* fichier en supprimant les lignes mises en surbrillance en jaune :  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. Dans le `Page_Load` Gestionnaire de la *Manage.aspx.cs* code-behind, supprimez les commentaires de la ligne de code mis en surbrillance en jaune afin qu’il apparaisse comme suit : 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. Dans le codebehind de *compte*/*TwoFactorAuthenticationSignIn.aspx.cs*, mettre à jour le `Page_Load` gestionnaire en ajoutant le code suivant mis en surbrillance en jaune : 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

   En rendant le code ci-dessus à modifier, DropDownList « Fournisseurs » contenant les options d’authentification ne rétablit pas à la première valeur. Cela permettra à l’utilisateur de sélectionner correctement toutes les options à utiliser lors de l’authentification, pas seulement la première.
10. Dans **l’Explorateur de solutions**, avec le bouton droit *Default.aspx* et sélectionnez **définir comme Page de démarrage**.
11. En testant votre application, commencez par créer l’application (**Ctrl**+**MAJ**+**B**), puis exécutez l’application (**F5**) et Sélectionnez **inscrire** pour créer un nouveau compte d’utilisateur ou sélectionnez **connectez-vous** si le compte d’utilisateur a déjà été inscrit.
12. Une fois que vous (l’utilisateur) avez connecté, cliquez sur l’ID d’utilisateur (adresse e-mail) dans la barre de navigation pour afficher le **gérer le compte** page (Manage.aspx).  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. Cliquez sur **ajouter** regard **numéro de téléphone** sur le **gérer le compte** page.  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. Ajouter le numéro de téléphone où vous (l’utilisateur) souhaitez recevoir des messages SMS (SMS) et cliquez sur le **envoyer** bouton.   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
    À ce stade, l’application utilisera les informations d’identification à partir de la *Web.config* contacter Twilio. Un message SMS (SMS) sera envoyé au numéro de téléphone associé au compte d’utilisateur. Vous pouvez vérifier que le message Twilio a été envoyé en consultant le tableau de bord Twilio.
15. En quelques secondes, le téléphone associé au compte d’utilisateur obtiendra un message texte contenant le code de vérification. Entrez le code de vérification et appuyez sur **envoyer**.  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>Activer l’authentification à deux facteurs pour un utilisateur inscrit

À ce stade, vous avez activé l’authentification à deux facteurs pour votre application. Pour un utilisateur à utiliser l’authentification à deux facteurs, ils peuvent simplement modifier leurs paramètres à l’aide de l’interface utilisateur. 

1. En tant qu’utilisateur de votre application vous pouvez activer l’authentification à deux facteurs pour votre compte spécifique en cliquant sur l’ID d’utilisateur (alias de messagerie) dans la barre de navigation pour afficher le **gérer le compte** page. Ensuite, cliquez sur le **activer** lien pour activer l’authentification à deux facteurs pour le compte.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. Déconnectez-vous, puis reconnectez-vous. Si vous avez activé la messagerie, vous pouvez sélectionner des SMS ou e-mail pour l’authentification à deux facteurs. Si vous n’avez pas activé la messagerie, consultez le didacticiel intitulé [créer une application sécurisée ASP.NET Web Forms avec l’enregistrement de l’utilisateur, Confirmation par courrier électronique et mot de passe réinitialisé](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. La page d’authentification à deux facteurs s’affiche où vous pouvez entrer le code (à partir de SMS ou e-mail).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 En cliquant sur le **mémoriser ce navigateur** case à cocher vous exempter d’avoir à utiliser l’authentification à deux facteurs pour vous connecter lorsque vous utilisez le navigateur et l’appareil où vous avez coché la case. Tant que les utilisateurs malveillants ne peuvent pas accéder à votre appareil, l’activation de l’authentification à deux facteurs et en cliquant sur le **mémoriser ce navigateur** vous fournira un accès mot de passe une seule étape pratique, tout en conservant strong protection de l’authentification à deux facteurs pour tous les accès à partir d’appareils non approuvé. Vous pouvez effectuer cela sur n’importe quel appareil privé que vous utilisez régulièrement.

<a id="addRes"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

- [Authentification à deux facteurs par SMS et e-mail avec ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Des liens vers les ressources recommandées sur ASP.NET Identity](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Déployer une application de formulaires Web ASP.NET sécurisée avec appartenance, OAuth et base de données SQL sur un site Web Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Série de didacticiels ASP.NET Web Forms - ajouter le fournisseur OAuth 2.0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Série de didacticiels Web Forms ASP.NET - activer SSL pour le projet](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [Confirmation de compte et de récupération de mot de passe avec ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Création de l’application dans Facebook et connexion de l’application au projet](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
