---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: >
  Créer une application web ASP.NET MVC 5 sécurisée avec connexion, confirmation par e-mail et réinitialisation de mot de passe (C#) | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous montre comment créer une application web ASP.NET MVC 5 avec confirmation par courrier électronique et réinitialisation de mot de passe en utilisant le système d’appartenance d'ASP.NET Identity. Autorité de certification vous...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: e15595cab2d1f51374d4577a67f0f190a531acb5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830782"
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>Créer une application web ASP.NET MVC 5 sécurisée avec connexion, confirmation par courrier électronique et réinitialisation de mot de passe (C#)

====================
par [Rick Anderson](https://github.com/Rick-Anderson)

> Ce didacticiel vous montre comment créer une application web ASP.NET MVC 5 avec confirmation par courrier électronique et réinitialisation de mot de passe en utilisant le système d’appartenance d'ASP.NET Identity. Vous pouvez télécharger l’application terminée [ici](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Le téléchargement contient les helpers de débogage qui vous permettent de tester une confirmation par e-mail et par SMS sans configurer un fournisseur de messagerie ou de SMS.
> 
> Ce didacticiel a été écrit par [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter : [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Créer une application ASP.NET MVC

Démarrez en installant et en cours d’exécution [Visual Studio Express 2013 pour le Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installer [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou une version ultérieure.

> [!NOTE]
> Avertissement : Vous devez installer [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou une version ultérieure pour suivre ce didacticiel.


1. Créez un projet Web ASP.NET et sélectionnez le modèle MVC. Web Forms prend également en charge ASP.NET Identity : vous pouvez donc suivre des étapes similaires dans une application Web Forms.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. Laissez l’authentification par défaut à **Comptes d’utilisateur individuels**. Si vous souhaitez héberger l’application dans Azure, laissez la case à cocher activée. Plus loin dans ce didacticiel, nous déploierons sur Azure. Vous pouvez [ouvrir un compte Azure gratuitement](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Définir le [projet pour utiliser SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Exécutez l’application, cliquez sur le lien **S'inscrire** et inscrivez un utilisateur. À ce stade, la seule validation sur l’adresse e-mail se fait avec l'attribut [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx).
5. Dans l’Explorateur de serveurs, accédez à **Data Connections\DefaultConnection\Tables\AspNetUsers**, cliquez avec le bouton droit et sélectionnez **Ouvrir la définition de la table**.

    L’illustration suivante montre le schéma `AspNetUsers` :

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. Cliquez avec le bouton droit sur la table **AspNetUsers** et sélectionnez **Afficher les données de la table**.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 À ce stade, l’adresse de messagerie n’a pas été confirmée.

7. Cliquez sur la ligne, puis sélectionnez Supprimer. Vous ajoutez de nouveau ce courrier électronique à l’étape suivante et envoyer un e-mail de confirmation.

## <a name="email-confirmation"></a>E-mail de confirmation

Il est recommandé de confirmer l’adresse de messagerie d’un nouvel enregistrement d’utilisateur pour vérifier qu’ils n’empruntent pas l'identité d'une autre personne (autrement dit, qu'ils ne se sont pas inscrits avec par l'adresse de messagerie de quelqu'un d’autre). Supposons que vous ayez un forum de discussion : vous voudriez empêcher `"bob@example.com"` de s'inscrire en tant que `"joe@contoso.com"`. ans la confirmation par courrier électronique, `"joe@contoso.com"` pourrait recevoir du courrier indésirable à partir de votre application. Supposons que Bob se soit accidentellement inscrit en tant que `"bib@example.com"` et ne l'ait pas remarqué, il ne serait pas en mesure d’utiliser le mot de passe de récupération, car l’application n’. E-mail de confirmation fournit uniquement une protection limitée des robots et ne fournit pas une protection à partir des expéditeurs de courrier indésirable déterminés, ils ont des alias de messagerie travail nombreux, qu'ils peuvent utiliser pour inscrire.

En règle générale, vous souhaitez empêcher les nouveaux utilisateurs d'envoyer des données à votre site web avant qu’ils soient confirmés par courrier électronique, par SMS ou via un autre mécanisme. <a id="build"></a>Dans les sections ci-dessous, nous allons activer la confirmation de courrier électronique et modifier le code pour empêcher les utilisateurs qui viennent d’être inscrits de se connecter jusqu'à ce que leur courrier électronique ait été confirmé.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Se connecter à SendGrid

Bien que ce didacticiel montre uniquement comment ajouter la notification par courrier électronique via [SendGrid](http://sendgrid.com/), vous pouvez envoyer un e-mail à l’aide de SMTP et d'autres mécanismes (consultez [les ressources additionnelles](#addRes)).

1. Dans la Console du Gestionnaire de Package, entrez la commande suivante : 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. Accédez à la [page d’inscription d'Azure SendGrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) et inscrivez-vous à un compte SendGrid gratuit. Configurez SendGrid en ajoutant du code semblable au suivant dans *App_Start/IdentityConfig.cs* :

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

Vous devrez ajouter les inclusions suivantes :

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

Pour simplifier cet exemple, nous allons stocker les paramètres d’application dans le fichier *web.config* :

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> Sécurité - Ne jamais stocker de données sensibles dans votre code source. Le compte et les informations d’identification sont stockés dans l’appSetting. Sur Azure, vous pouvez stocker en toute sécurité ces valeurs sur l'onglet **[Configurer](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** dans le portail Azure. Consultez [les meilleures pratiques pour le déploiement des mots de passe et autres données sensibles sur ASP.NET et Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).


### <a name="enable-email-confirmation-in-the-account-controller"></a>Activer la confirmation par courrier électronique dans le contrôleur de compte

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

Vérifiez que le fichier *Views\Account\ConfirmEmail.cshtml* a une syntaxe razor correcte. (Le @ caractère dans la première ligne est peut-être manquant. )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

Exécutez l’application et cliquez sur le lien S’inscrire.  Une fois que vous avez envoyé le formulaire d’inscription, vous êtes connecté.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

Vérifiez votre compte de messagerie, puis cliquez sur le lien pour confirmer votre adresse de messagerie.

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Demander une confirmation par courrier électronique avant la connexion

Actuellement, une fois qu’un utilisateur complète le formulaire d’inscription, il est connecté. En règle générale, vous voulez vérifier son adresse e-mail avant de le connecter. Dans la section ci-dessous, nous modifions le code pour demander aux nouveaux utilisateurs un e-mail vérifié avant de les connecter (les authentifier). Mettez à jour la méthode `HttpPost Register` avec les modifications en surbrillance suivantes :

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

Si vous décommentez la méthode `SignInAsync`, l’utilisateur ne sera pas connecté lors de l’inscription. La ligne `TempData["ViewBagLink"] = callbackUrl;` peut être utilisée pour [déboguer l’application](#dbg) et tester l’inscription sans envoyer de courrier électronique. `ViewBag.Message` permet d’afficher les instructions de confirmation. L'[exemple à télécharger](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contient du code pour tester l’e-mail de confirmation sans configurer la messagerie et peut également être utilisé pour déboguer l’application.

Créez un fichier `Views\Shared\Info.cshtml` et ajoutez le balisage razor suivant :

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

Ajoutez l' [attribut Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) à la méthode d’action `Contact` du contrôleur Home. Vous pouvez cliquer sur le **Contact** lien pour vérifier les utilisateurs anonymes n’ont accès et les utilisateurs authentifiés ont accès.

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

Vous devez également mettre à jour la méthode d’action `HttpPost Login`:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

Mettez à jour la vue *Views\Shared\Error.cshtml* pour afficher le message d’erreur :

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

Supprimez les comptes de la table **AspNetUsers** qui contiennent l’alias d’e-mail que vous voulez tester. Exécutez l’application et vérifiez que vous ne pouvez pas vous connecter avant d’avoir confirmé votre adresse e-mail. Après avoir confirmé votre adresse e-mail, cliquez sur le lien **Contact**.

<a id="reset"></a>
## <a name="password-recoveryreset"></a>Récupération de mot de passe/réinitialisation

Supprimez les caractères de commentaire de la `HttpPost ForgotPassword` méthode d’action dans le contrôleur de compte :

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

Supprimez les caractères de commentaire de la méthode `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) dans le fichier de vue razor *Views\Account\Login.cshtml* :

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

La page de connexion affiche maintenant un lien pour réinitialiser le mot de passe.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Renvoyer le lien de confirmation de courrier électronique

Une fois qu’un utilisateur crée un compte local, un lien de confirmation lui est envoyé par e-mail, qu’il doit utiliser avant de pouvoir se connecter. Si l’utilisateur supprime accidentellement l’e-mail de confirmation ou si l’e-mail n’arrive jamais, le lien de confirmation doit lui être renvoyé. Les modifications suivantes du code montrent comment faire cela.


Ajoutez la méthode helper suivante en bas du fichier *Controllers\AccountController.cs*:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

Mettez à jour la méthode Register de façon à utiliser le nouveau helper :

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

Mettez à jour la méthode Login de façon à renvoyer le mot de passe si le compte d’utilisateur n’a pas été confirmé :

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>Combiner des comptes de connexion de réseaux sociaux et local

Vous pouvez combiner des comptes locaux et de réseaux sociaux en cliquant sur le lien de votre messagerie. Dans la séquence suivante, **RickAndMSFT@gmail.com** est d’abord créé en tant que connexion locale, mais vous pouvez d’abord créer le compte en tant que connexion de réseau social, puis ajouter une connexion locale.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

Cliquez sur le lien **Gérer**. Notez l’information **Connexions externes : 0** associée à ce compte.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

Cliquez sur le lien vers un autre service de connexion et acceptez les demandes d’application. Les deux comptes ont été combinés : vous pourrez donc vous connecter avec l'un ou l'autre compte. Vous pouvez permettre à vos utilisateurs d’ajouter des comptes locaux au cas où leur service d’authentification de connexion de réseau social serait indisponible, ou au cas plus probable où ils auraient perdu l’accès à leur compte social.

Dans l’image suivante, Tom est une connexion de réseau social (comme l'indique **Connexions externes : 1** affiché dans la page).

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

Le fait de cliquer sur **Choisir un mot de passe** vous permet d’ajouter une connexion locale associée au même compte.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>E-mail de confirmation de façon plus approfondie

Mon didacticiel [Confirmation du compte et récupération de mot de passe avec ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) aborde ce sujet avec plus de détails.

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Débogage de l’application

Si vous n’obtenez pas un message électronique contenant le lien :

- Vérifiez votre dossier des courriers indésirables.
- Connectez-vous à votre compte SendGrid, puis cliquez sur le [lien activité de messagerie](https://sendgrid.com/logs/index).

Pour tester le lien de vérification sans e-mail, vous devez télécharger l'[exemple terminé](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). Le lien de confirmation et les codes de confirmation seront affichés dans la page.

<a id="addRes"></a>
## <a name="additional-resources"></a>Ressources supplémentaires

- [Des liens vers les ressources recommandées sur ASP.NET Identity](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Confirmation de compte et récupération de mot de passe avec ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Décrit plus en détails la confirmation de compte et la récupération de mot de passe.
- [Application MVC 5 avec connexion Facebook, Twitter, LinkedIn et Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Ce tutoriel vous montre comment écrire une application ASP.NET MVC 5 avec une autorisation Facebook et Google OAuth2. Il montre également comment ajouter des données supplémentaires à la base de données Identity.
- [Déployer une application sécurisée ASP.NET MVC avec l’appartenance, OAuth et SQL Database sur Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Ce tutoriel ajoute le déploiement Azure, la sécurisation de votre application avec des rôles, l’utilisation de l’API d’appartenance pour ajouter des utilisateurs et des rôles, et des fonctionnalités de sécurité supplémentaires.
- [Création d’une application pour Google OAuth 2 et connexion de l’application au projet](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Création de l’application dans Facebook et connexion de l’application au projet](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Configuration de SSL dans le projet](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
