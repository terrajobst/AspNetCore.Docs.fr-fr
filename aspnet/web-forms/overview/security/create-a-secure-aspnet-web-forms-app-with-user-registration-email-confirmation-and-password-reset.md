---
uid: web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
title: Créer une application Web Forms ASP.NET sécurisée avec inscription des utilisateurs, par e-mail de réinitialisation de mot de passe et de confirmation (c#) | Microsoft Docs
author: Erikre
description: Ce didacticiel vous montre comment créer une application Web Forms ASP.NET avec inscription utilisateur, confirmation par courrier électronique et mot de passe réinitialisé à l’aide du membre de l’identité ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 0a8d6044-5fab-4213-82d6-5618d5601358
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 79755282f5f146ac6969c3adfeb285610db530ee
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391276"
---
<a name="create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset-c"></a>Créer une application Web Forms ASP.NET sécurisée avec inscription des utilisateurs, par e-mail de réinitialisation de mot de passe et de confirmation (c#)
====================
par [Erik Reitan](https://github.com/Erikre)

> Ce didacticiel vous montre comment créer une application Web Forms ASP.NET avec inscription de l’utilisateur, confirmation par courrier électronique et mot de passe réinitialisé à l’aide du système d’appartenance ASP.NET Identity. Ce didacticiel a été basé sur de Rick Anderson [didacticiel MVC](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).


## <a name="introduction"></a>Introduction

Ce didacticiel vous guide à travers les étapes requises pour créer une application Web Forms ASP.NET à l’aide de Visual Studio et ASP.NET 4.5 pour créer une application Web Forms sécurisée avec inscription des utilisateurs, envoyer par courrier électronique de confirmation et le mot de passe de la réinitialisation.

### <a name="tutorial-tasks-and-information"></a>Tâches de didacticiel et les informations sur :

- [Créer un site Web ASP.NET application Forms](#createWebForms)
- [Raccorder SendGrid](#SG)
- [Demander Confirmation de l’E-mail avant de journal dans](#require)
- [Réinitialisation et la récupération de mot de passe](#reset)
- [Renvoyer le lien de Confirmation de courrier électronique](#rsend)
- [Résolution des problèmes de l’application](#dbg)
- [Ressources supplémentaires pour MSBuild](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Créer une application Web Forms ASP.NET

Démarrez en installant et en cours d’exécution [Visual Studio Express 2013 pour le Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installer [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou une version ultérieure ainsi.

> [!NOTE]
> Avertissement : Vous devez installer [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou une version ultérieure pour suivre ce didacticiel.


1. Créez un projet (**fichier**  - &gt; **nouveau projet**) et sélectionnez le **Application Web ASP.NET** modèle et la plus récente du .NET Framework version à partir de la **nouveau projet** boîte de dialogue.
2. À partir de la **nouveau projet ASP.NET** boîte de dialogue, sélectionnez le **Web Forms** modèle. Laissez l’authentification par défaut à **Comptes d’utilisateur individuels**. Si vous souhaitez héberger l’application dans Azure, laissez le **hôte dans le cloud** case est cochée.   
 Ensuite, cliquez sur **OK** pour créer le nouveau projet.  
    ![Boîte de dialogue Nouveau projet ASP.NET](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image1.png)
3. Activer Secure Sockets Layer (SSL) pour le projet. Suivez les étapes disponibles dans le **activer SSL pour le projet** section de la [mise en route de la série de didacticiels de Web Forms](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. Exécutez l’application, cliquez sur le **inscrire** lier et inscrire un nouvel utilisateur. À ce stade, la seule validation de l’e-mail est basée sur le [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attribut pour garantir l’adresse e-mail est bien formé. Vous allez modifier le code pour ajouter l’e-mail de confirmation. Fermez la fenêtre du navigateur.
5. Dans **Explorateur de serveurs** de Visual Studio (**vue**  - &gt; **Explorateur de serveurs**), accédez à **données\ de données DefaultConnection\Tables\AspNetUsers**, avec le bouton droit cliquez et sélectionnez **ouvrir la définition de la table**. 

    L’illustration suivante montre le `AspNetUsers` schéma de table :

    ![Schéma de la table AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image2.png)
6. Dans **Explorateur de serveurs**, avec le bouton droit sur le **AspNetUsers** de table et sélectionnez **afficher les données de Table**.  
  
    ![Données de la table AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image3.png)  
 À ce stade le courrier électronique pour l’utilisateur inscrit n’a pas été confirmé.
7. Cliquez sur la ligne, puis sélectionnez Supprimer pour supprimer l’utilisateur. Vous ajoutez de nouveau ce courrier électronique à l’étape suivante et envoyer un message de confirmation à l’adresse de messagerie.

## <a name="email-confirmation"></a>E-mail de Confirmation

Il est recommandé de confirmer l’adresse de messagerie lors de l’inscription d’un nouvel utilisateur pour vérifier qu’ils ne sont pas emprunter l’identité d’une autre personne (autrement dit, ils n’ont pas inscrit auprès de quelqu'un d’autre e-mail). Supposons que vous ayez un forum de discussion : vous voudriez empêcher `"bob@cpandl.com"` de s'inscrire en tant que `"joe@contoso.com"`. ans la confirmation par courrier électronique, `"joe@contoso.com"` pourrait recevoir du courrier indésirable à partir de votre application. Supposons que Bob accidentellement inscrit en tant que `"bib@cpandl.com"` et vous l’auriez pas remarqué, il ne serait pas en mesure d’utiliser la récupération de mot de passe, car l’application n’a pas son adresse de messagerie correcte. E-mail de confirmation fournit uniquement une protection limitée des robots et ne fournit pas une protection à partir des expéditeurs de courrier indésirable déterminés.

En règle générale, vous souhaitez empêcher les nouveaux utilisateurs à partir de la validation des données à votre site Web avant qu’ils ont été confirmés par un message électronique, un SMS ou un autre mécanisme. Dans les sections ci-dessous, nous activer la confirmation de courrier électronique et modifier le code pour empêcher les utilisateurs qui vient d’être inscrits de se connecter jusqu'à ce que leur adresse de messagerie a été confirmée. Vous allez utiliser le service de messagerie SendGrid dans ce didacticiel.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Se connecter à SendGrid

Bien que ce didacticiel montre uniquement comment ajouter la notification par courrier électronique via [SendGrid](http://sendgrid.com/), vous pouvez envoyer un e-mail à l’aide de SMTP et d'autres mécanismes (consultez [les ressources additionnelles](#addRes)).

1. Dans Visual Studio, ouvrez le **Console du Gestionnaire de Package** (**outils**  - &gt; **Gestionnaire de Package NuGet**  - &gt; **Console du Gestionnaire de package**), puis entrez la commande suivante :  
    `Install-Package SendGrid`
2. Accédez à la [page d’inscription Azure SendGrid](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) et vous inscrire gratuitement compte SendGrid. Vous pouvez également vous inscrire pour un compte SendGrid gratuit directement sur [site de SendGrid](http://www.sendgrid.com).
3. À partir de **l’Explorateur de solutions** ouvrir le *IdentityConfig.cs* de fichiers dans le *application\_Démarrer* dossier et ajoutez le code suivant mis en surbrillance en jaune pour le `EmailService` classe permettant de configurer **SendGrid**:

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample1.cs?highlight=3,5,8-37)]
4. En outre, ajoutez le code suivant `using` instructions au début de la *IdentityConfig.cs* fichier : 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample2.cs?highlight=1-4)]
5. Pour simplifier cet exemple, vous allez stocker les valeurs de compte de service de messagerie dans le `appSettings` section de la *web.config* fichier. Ajoutez le code XML suivant mis en surbrillance en jaune pour le *web.config* fichier à la racine de votre projet :

    [!code-xml[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample3.xml?highlight=2-5)]

    > [!WARNING]
    > Sécurité - Ne jamais stocker de données sensibles dans votre code source. Dans cet exemple, le compte et les informations d’identification sont stockées dans le **appSetting** section de la *Web.config* fichier. Sur Azure, vous pouvez stocker en toute sécurité ces valeurs sur l'onglet **[Configurer](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** dans le portail Azure. Pour plus d’informations, consultez rubrique de Rick Anderson [meilleures pratiques pour le déploiement des mots de passe et autres données sensibles sur ASP.NET et Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
6. Ajoutez les valeurs de service de messagerie pour refléter vos valeurs de l’authentification SendGrid (nom d’utilisateur et mot de passe) pour que vous puissiez réussie Envoyer e-mail à partir de votre application. Veillez à utiliser le nom de votre compte SendGrid plutôt que l’adresse de messagerie que vous avez fourni SendGrid.

### <a name="enable-email-confirmation"></a>Activer la Confirmation de courrier électronique

 Pour activer la confirmation par courrier électronique, vous allez modifier le code d’inscription en procédant comme suit.  
 

1. Dans le *compte* dossier, ouvrez le *Register.aspx.cs* code-behind et mettre à jour le `CreateUser_Click` méthode pour permettre les modifications suivantes : 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample4.cs?highlight=9-11)]
2. Dans **l’Explorateur de solutions**, avec le bouton droit *Default.aspx* et sélectionnez **définir comme Page de démarrage**.
3. Exécutez l’application en appuyant sur **F5.** Une fois la page s’affiche, cliquez sur le **inscrire** lien pour afficher la page d’inscription.
4. Entrez votre adresse électronique et le mot de passe, puis cliquez sur le **inscrire** bouton pour envoyer un e-mail via SendGrid.  
   L’état actuel de votre projet et de code permet à l’utilisateur pour se connecter une fois qu’elles terminent le formulaire d’inscription, même si elles ne l’avez pas confirmé son compte.
5. Vérifiez votre compte de messagerie, puis cliquez sur le lien pour confirmer votre adresse de messagerie.  
   Une fois que vous envoyez le formulaire d’inscription, vous serez connecté.  
    ![Exemple de site Web - connecté](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image4.png)

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Demander Confirmation de l’E-mail avant de journal dans

Bien que vous avez confirmé que le compte de messagerie, à ce stade vous serait inutile de cliquer sur le lien contenu dans l’e-mail de vérification pour être entièrement connectés. Dans la section suivante, vous allez modifier le code exigeant que les nouveaux utilisateurs pour avoir un e-mail de confirmation avant d’être enregistrées (authentifié).

1. Dans **l’Explorateur de solutions** de Visual Studio, mettre à jour le `CreateUser_Click` événement dans le *Register.aspx.cs* code-behind contenue dans le *comptes* dossier avec le modifications en surbrillance suivantes : 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample5.cs?highlight=13-14,17-21)]
2. Mise à jour le `LogIn` méthode dans le *Login.aspx.cs* code-behind avec les modifications suivantes : 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample6.cs?highlight=9-19,45-46)]

### <a name="run-the-application"></a>Exécution de l'application

 Maintenant que vous avez implémenté le code pour vérifier si l’adresse de messagerie d’un utilisateur a été confirmé, vous pouvez vérifier les fonctionnalités à la fois sur le **inscrire** et **connexion** pages. 

1. Supprimez tous les comptes dans le **AspNetUsers** table qui contient l’alias de messagerie que vous souhaitez tester.
2. Exécutez l’application (**F5**) et vérifiez que vous ne pouvez pas inscrire en tant qu’utilisateur jusqu'à ce que vous avez confirmé votre adresse de messagerie.
3. Avant de confirmer votre nouveau compte via le courrier électronique qui a été envoyé uniquement, tentent de se connecter avec le nouveau compte.  
 Vous verrez que vous ne parvenez pas à se connecter et que vous devez avoir un compte de messagerie confirmée.
4. Après avoir confirmé votre adresse de messagerie, ouvrez une session l’application.

<a id="reset"></a>
## <a name="password-recovery-and-reset"></a>Réinitialisation et la récupération de mot de passe

1. Dans Visual Studio, supprimez les caractères de commentaire de la `Forgot` méthode dans le *Forgot.aspx.cs* code-behind contenue dans le *compte* dossier, afin que la méthode apparaît comme suit : 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample7.cs?highlight=16-18)]
2. Ouvrez le *Login.aspx* page. Remplacez le balisage à la fin de la **loginForm** section comme indiqué ci-dessous : 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample8.aspx?highlight=52-53)]
3. Ouvrez le *Login.aspx.cs* code-behind et supprimez les commentaires de la ligne suivante de code mis en surbrillance en jaune à partir de la `Page_Load` Gestionnaire d’événements : 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample9.cs?highlight=5)]
4. Exécutez l’application en appuyant sur **F5.** Une fois la page s’affiche, cliquez sur le **connectez-vous** lien.
5. Cliquez sur le **votre mot de passe oublié ?** lien pour afficher le **mot de passe oublié** page.
6. Entrez votre adresse e-mail et cliquez sur le **envoyer** bouton pour envoyer un e-mail à votre adresse de ce qui vous permettra de réinitialiser votre mot de passe.   
   Vérifiez votre compte de messagerie, puis cliquez sur le lien pour afficher le **réinitialiser le mot de passe** page.
7. Sur le **réinitialiser le mot de passe** , entrez votre e-mail, le mot de passe et le mot de passe confirmé. Ensuite, appuyez sur la **réinitialiser** bouton.  
   Lorsque vous a correctement réinitialisez votre mot de passe, le **mot de passe modifié** page s’affichera. Vous pouvez maintenant vous connecter avec votre nouveau mot de passe.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Renvoyer le lien de Confirmation de courrier électronique

Une fois qu’un utilisateur crée un compte local, un lien de confirmation lui est envoyé par e-mail, qu’il doit utiliser avant de pouvoir se connecter. Si l’utilisateur supprime accidentellement l’e-mail de confirmation ou si l’e-mail n’arrive jamais, le lien de confirmation doit lui être renvoyé. Les modifications suivantes du code montrent comment faire cela.


1. Dans Visual Studio, ouvrez le **Login.aspx.cs** code-behind et ajoutez le Gestionnaire d’événements suivant après le `LogIn` Gestionnaire d’événements :   

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample10.cs)]
2. Modifier le `LogIn` Gestionnaire d’événements dans le *Login.aspx.cs* code-behind en modifiant le code mis en surbrillance en jaune comme suit : 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample11.cs?highlight=15-17)]
3. Mise à jour le *Login.aspx* page en ajoutant le code mis en surbrillance en jaune comme suit : 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample12.aspx?highlight=45-46)]
4. Supprimez tous les comptes dans le **AspNetUsers** table qui contient l’alias de messagerie que vous souhaitez tester.
5. Exécutez l’application (**F5**) et inscrire votre adresse de messagerie.
6. Avant de confirmer votre nouveau compte via le courrier électronique qui a été envoyé uniquement, tentent de se connecter avec le nouveau compte.  
   Vous verrez que vous ne parvenez pas à se connecter et que vous devez avoir un compte de messagerie confirmée. En outre, vous pouvez désormais renvoyer un message de confirmation à votre compte de messagerie.
7. Entrez votre adresse de messagerie et le mot de passe, puis appuyez sur la **renvoyer confirmation** bouton.
8. Après avoir confirmé votre adresse de messagerie basée sur le message électronique qui vient d’être envoyé, connectez-vous à l’application.

<a id="dbg"></a>
## <a name="troubleshooting-the-app"></a>Résolution des problèmes de l’application

Si vous ne recevez un e-mail contenant le lien pour vérifier vos informations d’identification :

- Vérifiez votre dossier des courriers indésirables.
- Connectez-vous à votre compte SendGrid, puis cliquez sur le [lien activité de messagerie](https://sendgrid.com/logs/index).
- Être certain que vous avez utilisé le nom de votre compte d’utilisateur SendGrid comme un *Web.config* valeur plutôt que votre adresse de messagerie du compte SendGrid.

<a id="addRes"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

- [Des liens vers les ressources recommandées sur ASP.NET Identity](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Confirmation de compte et de récupération de mot de passe avec ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Série de didacticiels ASP.NET Web Forms - ajouter le fournisseur OAuth 2.0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Déployer une application de formulaires Web ASP.NET sécurisée avec appartenance, OAuth et base de données SQL dans Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Série de didacticiels Web Forms ASP.NET - activer SSL pour le projet](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
