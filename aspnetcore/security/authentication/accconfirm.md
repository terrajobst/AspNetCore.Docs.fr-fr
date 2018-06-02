---
title: Confirmation du compte et récupération de mot de passe dans ASP.NET Core
author: rick-anderson
description: Découvrez comment créer une application ASP.NET Core à la réinitialisation par courrier électronique de confirmation et le mot de passe.
manager: wpickett
ms.author: riande
ms.date: 2/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: 397d8bf04abf6be811ad8c91d52565251ac61678
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2018
ms.locfileid: "34688968"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Confirmation du compte et récupération de mot de passe dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Joe Audette](https://twitter.com/joeaudette)

Ce didacticiel vous montre comment créer une application ASP.NET Core avec confirmation par courrier électronique et réinitialisation de mot de passe. Ce didacticiel n'est **pas** un sujet pour débuter. Vous devez être familiarisé avec :

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Authentification](xref:security/authentication/index)
* [Confirmation de compte et récupération de mot de passe](xref:security/authentication/accconfirm)
* [Entity Framework Core](xref:data/ef-mvc/intro)

Consultez [ce fichier PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) de la version 1.1 de MVC ASP.NET Core et 2.x.

## <a name="prerequisites"></a>Prérequis

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a>Créer un nouveau projet ASP.NET Core avec l’interface en ligne de commande .NET Core

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

* `--auth Individual` Spécifie le modèle de projet de comptes utilisateurs individuels.
* Sur Windows, ajoutez l'option `-uld`. Elle spécifie que la base de données locale doit être utilisée au lieu de SQLite.
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

## <a name="test-new-user-registration"></a>Tester une nouvelle inscription d’utilisateur

Exécuter l’application, sélectionnez le lien **S'inscrire** et inscrire un utilisateur. Suivez les instructions pour exécuter les migrations d’Entity Framework Core. À ce stade, la seule validation sur l’adresse e-mail se fait avec l'attribut [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute). Après avoir soumis l’inscription, vous êtes connecté à l’application. Plus loin dans ce didacticiel, le code est mis à jour pour les nouveaux utilisateurs ne peuvent pas se connecter jusqu'à ce que leur courrier électronique ait été validé.

## <a name="view-the-identity-database"></a>Afficher la base de données d’identité

Consultez [de travail dans un projet ASP.NET MVC de base avec SQLite](xref:tutorials/first-mvc-app-xplat/working-with-sql) pour obtenir des instructions sur l’affichage de la base de données SQLite.

Pour Visual Studio :

* À partir du menu **Affichage**, sélectionnez **l’Explorateur d’objets SQL Server** (SSOX).
* Accédez à **(localdb) MSSQLLocalDB (SQL Server 13)**. Avec le bouton droit sur **dbo. AspNetUsers** > **Afficher les données** :

![Menu contextuel sur la table AspNetUsers dans l’Explorateur d’objets SQL Server](accconfirm/_static/ssox.png)

Notez que le champ `EmailConfirmed` de la table est à `False`.

Vous pouvez souhaiter réutiliser ce courrier électronique à l’étape suivante lorsque l’application envoie un message électronique de confirmation. Cliquez avec le bouton droit sur la ligne, puis sélectionnez **Supprimer**. La suppression de l’alias de messagerie facilite dans les étapes suivantes.

---

## <a name="require-https"></a>Exiger HTTPS

Consultez [Exiger HTTPS](xref:security/enforcing-ssl).

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a>Demander confirmation de courrier électronique

Il est recommandé de confirmer l’adresse de messagerie d’un nouvel enregistrement d’utilisateur. Envoyer un courrier électronique de confirmation permet de vérifier qu’ils n'empruntent pas l'identité d'une autre personne (autrement dit, ils ne se sont pas inscrits avec l'adresse de messageriedquelqu'un d’autre). Supposons que vous ayez un forum de discussion, et que vous souhaitiez empêcher "yli@example.com" de s’inscription en tant que "nolivetto@contoso.com". Sans la confirmation par courrier électronique, "nolivetto@contoso.com" peut recevoir un e-mail indésirable de votre application. Supposons que l’utilisateur s'inscrit par inadvertance en tant que "ylo@example.com" et que vous n’avez pas remarqué la faute d’orthographe de "yli". Ils ne pourraient pas utiliser la récupération de mot de passe, car l’application n’a pas leur courrier électronique correct. l'email de confirmation fournit uniquement une protection limitée contre les robots. L'email de confirmation ne fournit pas une protection contre les utilisateurs malveillants avec plusieurs comptes de messagerie.

En règle générale, vous souhaitez empêcher les nouveaux utilisateurs d'envoyer des données à votre site web avant d’avoir un message électronique de confirmation.

Mettez à jour `ConfigureServices` pour exiger un message électronique de confirmation :

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

`config.SignIn.RequireConfirmedEmail = true;` empêche les utilisateurs enregistrés de se connecter jusqu'à ce que leur adresse de messagerie soit confirmée.

### <a name="configure-email-provider"></a>Configurer le fournisseur de messagerie

Dans ce didacticiel, SendGrid est utilisé pour envoyer un courrier électronique. Vous avez besoin d’un compte SendGrid et une clé pour envoyer un courrier électronique. Vous pouvez utiliser d’autres fournisseurs de messagerie. ASP.NET Core 2.x inclut `System.Net.Mail`, qui vous permet d’envoyer par courrier électronique à partir de votre application. Nous vous recommandons d'utiliser SendGrid ou un autre service de messagerie pour envoyer un courrier électronique. SMTP est difficile à sécuriser et à correctement configurer.

Le [modèle d’Options](xref:fundamentals/configuration/options) est utilisé pour accéder aux paramètres de compte et une clé utilisateur. Pour plus d’informations, consultez [configuration](xref:fundamentals/configuration/index).

Créez une classe pour extraire la clé de messagerie électronique sécurisée. Pour cet exemple, la classe `AuthMessageSenderOptions` est créée dans le fichier *Services/AuthMessageSenderOptions.cs* :

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

Définir `SendGridUser` et `SendGridKey` avec l'[outil Gestionnaire de secret](xref:security/app-secrets). Exemple :

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

Sur Windows, le gestionnaire de secret stocke des paires de clés/valeur dans un fichier *secrets.json* dans le répertoire `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`.

Les contenus du fichier *secrets.json* ne sont pas chiffrés. Le fichier *secrets.json* est présenté ci-dessous (la valeur `SendGridKey` a été supprimée.)

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a>Configurer le démarrage pour utiliser AuthMessageSenderOptions

Ajouter `AuthMessageSenderOptions` au conteneur de service à la fin de la méthode `ConfigureServices` dans le fichier *Startup.cs* :

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a>Configurer la classe AuthMessageSender

Ce didacticiel montre comment ajouter des notifications par courrier électronique via [SendGrid](https://sendgrid.com/), mais vous pouvez envoyer par courrier électronique en utilisant SMTP et d'autres mécanismes.

Installer le package NuGet `SendGrid` :

* À partir de la ligne de commande :

    `dotnet add package SendGrid`

* À partir de la Console du Gestionnaire de Package, entrez la commande suivante :

  `Install-Package SendGrid`

Consultez [Débuter avec SendGrid gratuitement](https://sendgrid.com/free/) pour vous inscrire pour un compte SendGrid gratuit.

#### <a name="configure-sendgrid"></a>Configurer SendGrid

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Pour configurer SendGrid, ajoutez du code semblable au suivant dans *Services/EmailSender.cs*:

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

* Ajoutez le code dans *Services/MessageServices.cs* similaire à ce qui suit pour configurer SendGrid :

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a>Activer la récupération de confirmation et le mot de passe du compte

Le modèle a le code pour la récupération de confirmation et le mot de passe du compte. Rechercher la méthode `OnPostAsync` dans *Pages/Account/Register.cshtml.cs*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Interdire les utilisateurs nouvellement inscrits d'être connectés automatiquement en supprimant la ligne suivante :

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

La méthode complète s’affiche avec la ligne modifiée mise en surbrillance :

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Pour activer la confirmation du compte, supprimez le code suivant :

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

**Remarque :** le code empêche un utilisateur nouvellement inscrit d'ouvrir automatiquement une session en supprimant la ligne suivante :

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

Activez la récupération de mot de passe en commentant le code de l'action `ForgotPassword` de *Controllers/AccountController.cs*:

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

Supprimez les commentaires de l’élément form *Views/Account/ForgotPassword.cshtml*. Vous souhaiterez peut-être supprimer le `<p> For more information on how to enable reset password ... </p>` élément, qui contient un lien vers cet article.

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a>S'inscrire, confirmer le courrier électronique et réinitialiser le mot de passe

Exécuter l’application web et testez la confirmation du compte et le processus de récupération de mot de passe.

* Exécutez l’application et inscrire un nouvel utilisateur

  ![Affichage du livre de comptes application Web](accconfirm/_static/loginaccconfirm1.png)

* Vérifiez votre adresse de messagerie pour le lien de confirmation du compte. Consultez [déboguer la messagerie](#debug) si vous n’obtenez pas le message électronique.
* Cliquez sur le lien pour confirmer votre adresse de messagerie.
* Connectez-vous avec votre adresse électronique et un mot de passe.
* Fermez la session.

### <a name="view-the-manage-page"></a>Afficher la page de gestion

Sélectionnez votre nom d’utilisateur dans le navigateur : ![fenêtre de navigateur avec le nom d’utilisateur](accconfirm/_static/un.png)

Vous devrez peut-être développer la barre de navigation pour afficher le nom d’utilisateur.

![barre de navigation](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

La page de gestion s’affiche avec l'onglet **profil** sélectionné. L'**Email** affiche une case à cocher indiquant que l’adresse de messagerie a été confirmée.

![page Gérer](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Cela est mentionné plus loin dans le didacticiel.
![page Gérer](accconfirm/_static/rick2.png)

---

### <a name="test-password-reset"></a>Test de réinitialisation de mot de passe

* Si vous êtes connecté, sélectionnez **Déconnexion**.
* Sélectionnez le lien **Connexion** et sélectionnez le lien **Mot de passe oublié ?**.
* Entrez l’e-mail que vous avez utilisé pour enregistrer le compte.
* Un message électronique contenant un lien pour réinitialiser votre mot de passe est envoyé. Vérifiez votre adresse de messagerie et cliquez sur le lien pour réinitialiser votre mot de passe. Une fois votre mot de passe a été réinitialisé avec succès, vous pouvez vous connecter avec votre adresse électronique et un nouveau mot de passe.

<a name="debug"></a>

### <a name="debug-email"></a>Déboguer le courrier électronique

Si vous ne parvenez à faire fonctionner l'email :

* Créer une [application console pour envoyer un courrier électronique](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).
* Examinez la page[activité de la messagerie](https://sendgrid.com/docs/User_Guide/email_activity.html).
* Vérifiez votre dossier courrier indésirable.
* Essayez un autre alias de messagerie électronique sur un fournisseur de messagerie différents (Microsoft, Yahoo, Gmail, etc.).
* Essayez d’envoyer à des comptes de messagerie différents.

Une **meilleure pratique de sécurité** consiste à **ne pas** utiliser des secrets de production en développement et en test. Si vous publiez l’application sur Azure, vous pouvez définir les clés secrètes SendGrid en tant que paramètres de l’application dans le portail de l’application Web Azure. Le système de configuration est configuré pour lire les clés à partir de variables d’environnement.

## <a name="combine-social-and-local-login-accounts"></a>Combiner des comptes de connexion de réseaux sociaux et local

Pour terminer cette section, vous devez d’abord activer un fournisseur d’authentification externe. Consultez [Facebook, Google et l’authentification du fournisseur externe](xref:security/authentication/social/index).

Vous pouvez combiner des comptes locaux et de réseaux sociaux en cliquant sur le lien de votre messagerie. Dans l’ordre suivant, "RickAndMSFT@gmail.com" est tout d’abord créé en tant que connexion locale; Toutefois, vous pouvez d’abord créer le compte en tant que connexion sociale, puis ajouter une connexion locale.

![Application Web : RickAndMSFT@gmail.com utilisateur authentifié](accconfirm/_static/rick.png)

Cliquez sur le lien **Gérer**. Notez la valeur 0 externe (connexions sociales) associé à ce compte.

![Gérer les affichages](accconfirm/_static/manage.png)

Cliquez sur le lien vers un autre service de connexion et accepter les demandes d’application. Dans l’image suivante, Facebook est le fournisseur d’authentification externe :

![Gestion de votre vue de logins externes répertoriant Facebook](accconfirm/_static/fb.png)

Les deux comptes ont été combinés. Vous popuvez vous connecter avec l'un ou l'autre compte. Vous pourriez vouloir que vos utilisateurs ajoutent des comptes locaux au cas où leur service d’authentification de connexion sociale est arrêté, ou plus probable qu’ils aient perdu l’accès à leur compte social.

## <a name="enable-account-confirmation-after-a-site-has-users"></a>Activer la confirmation du compte après qu’un site a des utilisateurs

Activer la confirmation du compte sur un site avec des utilisateurs verrouille tous les utilisateurs existants. Les utilisateurs existants sont verrouillés, car leurs comptes ne sont pas confirmées. Pour contourner le verrouillage d’utilisateur existant, utilisez une des approches suivantes :

* Mise à jour de la base de données pour marquer tous les utilisateurs existants comme étant confirmés.
* Confirmez les utilisateurs existants. Par exemple, en envoyant en masse des messages électroniques avec des liens de confirmation.
