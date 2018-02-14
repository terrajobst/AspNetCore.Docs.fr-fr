---
title: "Confirmation du compte et récupération de mot de passe dans ASP.NET Core"
author: rick-anderson
description: "Découvrez comment créer une application ASP.NET Core à la réinitialisation par courrier électronique de confirmation et le mot de passe."
manager: wpickett
ms.author: riande
ms.date: 2/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: e8f73d58bdf626910b2101ef310385f588315e26
ms.sourcegitcommit: 725cb18ad23013e15d3dbb527958481dee79f9f8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/13/2018
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Confirmation du compte et récupération de mot de passe dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Joe Audette](https://twitter.com/joeaudette)

Ce didacticiel vous montre comment créer une application ASP.NET Core à la réinitialisation par courrier électronique de confirmation et le mot de passe. Ce didacticiel est **pas** une rubrique de début. Vous devez être familiarisé avec :

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Authentification](xref:security/authentication/index)
* [Confirmation de compte et récupération de mot de passe](xref:security/authentication/accconfirm)
* [Entity Framework Core](xref:data/ef-mvc/intro)

Consultez [ce fichier PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) de la version 1.1 de MVC ASP.NET Core et 2.x.

## <a name="prerequisites"></a>Prérequis

[.NET core 2.1.4 SDK](https://www.microsoft.com/net/core) ou version ultérieure.

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a>Créer un nouveau projet ASP.NET Core avec l’interface de ligne de base .NET

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

* `--auth Individual` Spécifie le modèle de projet de comptes d’utilisateur individuels.
* Sur Windows, ajoutez le `-uld` option. Il spécifie la que base de données locale doit être utilisé au lieu de SQLite.
* Exécutez `new mvc --help` pour obtenir de l’aide sur cette commande.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Si vous utilisez l’interface CLI ou SQLite, exécutez la commande suivante dans une fenêtre de commande :

```console
dotnet new mvc --auth Individual
```

* `--auth Individual` Spécifie le modèle de projet de comptes d’utilisateur individuels.
* Sur Windows, ajoutez le `-uld` option. Il spécifie la que base de données locale doit être utilisé au lieu de SQLite.
* Exécutez `new mvc --help` pour obtenir de l’aide sur cette commande.

---

Vous pouvez également créer un nouveau projet ASP.NET Core avec Visual Studio :

* Dans Visual Studio, créez un **Application Web** projet.
* Sélectionnez **ASP.NET Core 2.0**. **.NET core** est sélectionné dans l’image suivante, mais vous pouvez sélectionner **.NET Framework**.
* Sélectionnez **modifier l’authentification** et la valeur **comptes d’utilisateur individuels**.
* Conservez la valeur par défaut **dans l’application des comptes d’utilisateurs de magasin**.

![Nouvelle boîte de dialogue projet indiquant « Radio de comptes d’utilisateur individuels » sélectionnée](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a>Nouvelle inscription de l’utilisateur de test

Exécuter l’application, sélectionnez le **inscrire** lier et inscrire un utilisateur. Suivez les instructions pour exécuter les migrations d’Entity Framework Core. À ce stade, la seule validation de l’adresse de messagerie est avec le [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribut. Après avoir soumis l’inscription, vous êtes connecté à l’application. Plus loin dans ce didacticiel, le code est mis à jour pour les nouveaux utilisateurs ne peuvent pas se connecter jusqu'à ce que leur courrier électronique a été validée.

## <a name="view-the-identity-database"></a>Afficher la base de données d’identité

Consultez [utilisation de SQLite dans un projet ASP.NET MVC de base](xref:tutorials/first-mvc-app-xplat/working-with-sql) pour obtenir des instructions sur l’affichage de la base de données SQLite.

Pour Visual Studio :

* À partir de la **vue** menu, sélectionnez **l’Explorateur d’objets SQL Server** (SSOX).
* Accédez à **(localdb) MSSQLLocalDB (SQL Server 13)**. Avec le bouton droit sur **dbo. AspNetUsers** > **afficher les données**:

![Menu contextuel sur la table AspNetUsers dans l’Explorateur d’objets SQL Server](accconfirm/_static/ssox.png)

Notez la table `EmailConfirmed` champ est `False`.

Vous pouvez souhaiter réutiliser ce courrier électronique à l’étape suivante lors de l’application envoie un message électronique de confirmation. Avec le bouton droit sur la ligne, puis sélectionnez **supprimer**. Suppression de l’alias de messagerie facilite dans les étapes suivantes.

---

## <a name="require-https"></a>Exiger HTTPS

Consultez [exiger HTTPS](xref:security/enforcing-ssl).

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>Demander confirmation de courrier électronique

Il est recommandé de confirmer l’adresse de messagerie d’un nouvel enregistrement d’utilisateur. Envoyer par courrier électronique de confirmation permet de vérifier qu’ils empruntez pas une autre personne (autrement dit, ils n’ont pas inscrit avec par quelqu'un d’autre adresse de messagerie). Supposons que vous aviez un forum de discussion, et vous souhaitez empêcher «yli@example.com« lors de l’inscription en tant que »nolivetto@contoso.com. » Sans la confirmation par courrier électronique, «nolivetto@contoso.com» peut recevoir un e-mail indésirable de votre application. Supposons que l’utilisateur inscrit par inadvertance en tant que «ylo@example.com» et vous n’aviez pas remarqué la faute d’orthographe de « yli ». Ils ne pourrez pas utiliser la récupération de mot de passe, car l’application n’a pas leur courrier électronique correct. E-mail de confirmation fournit uniquement une protection limitée de robots. E-mail de confirmation ne fournit pas une protection contre les utilisateurs malveillants avec plusieurs comptes de messagerie.

En règle générale, vous souhaitez empêcher les nouveaux utilisateurs à partir de la validation des données à votre site web avant d’avoir un message électronique de confirmation.

Mise à jour `ConfigureServices` pour exiger un message électronique de confirmation :

[!code-csharp[Main](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

`config.SignIn.RequireConfirmedEmail = true;` empêche les utilisateurs enregistrés de se connecter jusqu'à ce que leur adresse de messagerie est confirmée.

### <a name="configure-email-provider"></a>Configurer le fournisseur de messagerie

Dans ce didacticiel, SendGrid est utilisé pour envoyer un courrier électronique. Vous avez besoin d’un compte SendGrid et une clé pour envoyer un courrier électronique. Vous pouvez utiliser d’autres fournisseurs de messagerie. ASP.NET Core 2.x inclut `System.Net.Mail`, qui vous permet d’envoyer par courrier électronique à partir de votre application. Nous vous recommandons de qu'utiliser SendGrid ou un autre service de messagerie pour envoyer un courrier électronique. SMTP est difficile de sécuriser et correctement configuré.

Le [modèle d’Options](xref:fundamentals/configuration/options) est utilisé pour accéder aux paramètres de compte et une clé utilisateur. Pour plus d’informations, consultez [configuration](xref:fundamentals/configuration/index).

Créez une classe pour extraire la clé de sécuriser la messagerie électronique. Pour cet exemple, le `AuthMessageSenderOptions` classe est créée dans le *Services/AuthMessageSenderOptions.cs* fichier :

[!code-csharp[Main](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

Définir le `SendGridUser` et `SendGridKey` avec la [outil Gestionnaire de secret](xref:security/app-secrets). Exemple :

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

Sur Windows, Gestionnaire de secret principal stocke des paires de clés/valeur dans un *secrets.json* de fichiers dans le `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` active.

Le contenu de la *secrets.json* fichier ne sont pas chiffrées. Le *secrets.json* fichier est présenté ci-dessous (le `SendGridKey` valeur a été supprimée.)

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>Configurer le démarrage pour utiliser AuthMessageSenderOptions

Ajouter `AuthMessageSenderOptions` au conteneur de service à la fin de la `ConfigureServices` méthode dans le *Startup.cs* fichier :

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>Configurer la classe AuthMessageSender

Ce didacticiel montre comment ajouter des notifications par courrier électronique via [SendGrid](https://sendgrid.com/), mais vous pouvez envoyer par courrier électronique à l’aide de SMTP et autres mécanismes.

Installer le `SendGrid` package NuGet :

* À partir de la ligne de commande :

    `dotnet add package SendGrid`

* À partir de la Console du Gestionnaire de Package, entrez la commande suivante :

 `Install-Package SendGrid`

Consultez [prise en main SendGrid gratuitement](https://sendgrid.com/free/) s’inscrire pour un compte SendGrid gratuit.

#### <a name="configure-sendgrid"></a>Configurer SendGrid

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Pour configurer SendGrid, ajoutez du code semblable au suivant dans *Services/EmailSender.cs*:

[!code-csharp[Main](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
* Ajoutez le code dans *Services/MessageServices.cs* similaire à ce qui suit pour configurer SendGrid :

[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>Activer la récupération de confirmation et le mot de passe du compte

Le modèle a le code pour la récupération de confirmation et le mot de passe du compte. Rechercher les `OnPostAsync` méthode dans *Pages/Account/Register.cshtml.cs*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Interdire nouvellement inscrit qui est automatiquement connecté en supprimant la ligne suivante :

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

La méthode complète s’affiche avec la ligne modifiée mis en surbrillance :

[!code-csharp[Main](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Pour activer la confirmation du compte, supprimez le code suivant :

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

**Remarque :** le code empêche un utilisateur nouvellement inscrit à partir d’en cours automatiquement une session en supprimant la ligne suivante :

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

Activer la récupération de mot de passe par le code de commentaire de la `ForgotPassword` action de *Controllers/AccountController.cs*:

[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

Supprimez les commentaires de l’élément form *Views/Account/ForgotPassword.cshtml*. Vous souhaiterez peut-être supprimer le `<p> For more information on how to enable reset password ... </p>` élément, qui contient un lien vers cet article.

[!code-cshtml[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a>Inscrire, par courrier électronique de confirmation et réinitialiser le mot de passe

Exécuter l’application web et testez la confirmation du compte et le flux de récupération de mot de passe.

* Exécutez l’application et inscrire un nouvel utilisateur

 ![Affichage du livre de comptes application Web](accconfirm/_static/loginaccconfirm1.png)

* Vérifiez votre adresse de messagerie pour le lien de confirmation du compte. Consultez [déboguer messagerie](#debug) si vous n’obtenez pas le message électronique.
* Cliquez sur le lien pour confirmer votre adresse de messagerie.
* Connectez-vous avec votre adresse électronique et un mot de passe.
* Fermez la session.

### <a name="view-the-manage-page"></a>Afficher la page de gestion

Sélectionnez votre nom d’utilisateur dans le navigateur : ![fenêtre de navigateur avec le nom d’utilisateur](accconfirm/_static/un.png)

Vous devrez peut-être développer la barre de navigation pour afficher le nom d’utilisateur.

![navbar](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

La page de gestion s’affiche avec le **profil** onglet sélectionné. Le **messagerie** affiche une case à cocher indiquant l’adresse de messagerie a été confirmée.

![page Gérer](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Cela est mentionné plus loin dans le didacticiel.
![page Gérer](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>Réinitialisation de mot de passe de test

* Si vous êtes connecté, sélectionnez **déconnexion**.
* Sélectionnez le **connecter** lien et sélectionnez le **vous avez oublié votre mot de passe ?** lien.
* Entrez l’e-mail que vous avez utilisé pour enregistrer le compte.
* Un message électronique contenant un lien pour réinitialiser votre mot de passe est envoyé. Vérifiez votre adresse de messagerie et cliquez sur le lien pour réinitialiser votre mot de passe. Une fois votre mot de passe a été réinitialisé avec succès, vous pouvez vous connecter avec votre adresse électronique et un nouveau mot de passe.

<a name="debug"></a>

### <a name="debug-email"></a>Déboguer le courrier électronique

Si vous ne parvenez le travail de la messagerie :

* Créer un [application console pour envoyer un courrier électronique](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).
* Examinez le [activité de messagerie](https://sendgrid.com/docs/User_Guide/email_activity.html) page.
* Vérifiez votre dossier courrier indésirable.
* Essayez un autre alias de messagerie électronique sur un fournisseur de messagerie différents (Microsoft, Yahoo, Gmail, etc.).
* Essayez d’envoyer à des comptes de messagerie différents.

**Meilleure pratique de sécurité** consiste à **pas** utiliser des secrets de fabrication dans le développement et de test. Si vous publiez l’application sur Azure, vous pouvez définir les clés secrètes SendGrid en tant que paramètres de l’application dans le portail de l’application Web Azure. Le système de configuration est configuré pour lire les clés à partir de variables d’environnement.

## <a name="combine-social-and-local-login-accounts"></a>Combiner des comptes de connexion de réseaux sociaux et local

Pour terminer cette section, vous devez d’abord activer un fournisseur d’authentification externe. Consultez [activation de l’authentification à l’aide de Facebook, Google et autres fournisseurs externes](social/index.md).

Vous pouvez combiner les comptes locaux et réseaux sociaux en cliquant sur le lien de votre messagerie. Dans l’ordre suivant, «RickAndMSFT@gmail.com» est tout d’abord créé en tant qu’une connexion locale ; Toutefois, vous pouvez créer le compte en tant qu’une connexion sociale tout d’abord, puis ajouter une connexion locale.

![Application Web : RickAndMSFT@gmail.com utilisateur authentifié](accconfirm/_static/rick.png)

Cliquez sur le **gérer** lien. Notez la valeur 0 externe (connexions sociales) associé à ce compte.

![Gérer les affichages](accconfirm/_static/manage.png)

Cliquez sur le lien vers un autre service de connexion et d’accepter les demandes d’application. Dans l’image suivante, Facebook est le fournisseur d’authentification externes :

![Gestion de votre vue de logins externes répertoriant Facebook](accconfirm/_static/fb.png)

Les deux comptes ont été combinés. Vous ne parvenez pas à vous connecter avec un compte. Vous pourriez vos utilisateurs pour ajouter des comptes locaux en cas de leur service d’authentification de connexion sociale est arrêté, ou plus probable qu’ils avez perdu l’accès à leur compte sociaux.

## <a name="enable-account-confirmation-after-a-site-has-users"></a>Activer la confirmation du compte après qu’un site a des utilisateurs

Confirmation du compte l’activation sur un site avec les utilisateurs verrouille tous les utilisateurs existants. Les utilisateurs existants sont verrouillés, car leurs comptes ne sont pas confirmées. Pour contourner le verrouillage de l’utilisateur en cours de fermeture, utilisez une des approches suivantes :

* Mise à jour de la base de données pour marquer tous les utilisateurs existants comme étant confirmée
* Confirmez les utilisateurs existants. Par exemple, lot-envoi des messages électroniques avec des liens de confirmation.
