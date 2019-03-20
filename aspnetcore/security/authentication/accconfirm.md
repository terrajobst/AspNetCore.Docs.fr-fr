---
title: Confirmation de compte et de récupération de mot de passe dans ASP.NET Core
author: rick-anderson
description: Découvrez comment créer une application ASP.NET Core avec la réinitialisation de confirmation et le mot de passe de messagerie.
ms.author: riande
ms.date: 3/11/2019
uid: security/authentication/accconfirm
ms.openlocfilehash: 3bfc2ce46cfbc2ee308940f9e04eb2ffeec09073
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265496"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a>Confirmation de compte et de récupération de mot de passe dans ASP.NET Core

::: moniker range="<= aspnetcore-2.0"

Consultez [ce fichier PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) pour l’ASP.NET Core 1.1 et la version 2.1.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Ponant](https://github.com/Ponant), et [Joe Audette](https://twitter.com/joeaudette)

Ce didacticiel montre comment créer une application ASP.NET Core avec la réinitialisation de confirmation et le mot de passe de messagerie. Ce didacticiel n'est **pas** un sujet pour débuter. Vous devez être familiarisé avec :

* [ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start)
* [Authentification](xref:security/authentication/identity)
* [Entity Framework Core](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a>Prérequis

[SDK .NET core 2.2 ou version ultérieure](https://www.microsoft.com/net/download/all)

## <a name="create-a-web--app-and-scaffold-identity"></a>Créer une application web et de structurer des identités

Exécutez les commandes suivantes pour créer une application web avec l’authentification.

```console
dotnet new webapp -au Individual -uld -o WebPWrecover
cd WebPWrecover
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator identity -dc WebPWrecover.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout;Account.ConfirmEmail"
dotnet ef database drop -f
dotnet ef database update
dotnet run

```

## <a name="test-new-user-registration"></a>Tester l’inscription d’un nouvel utilisateur

Exécutez l’application, sélectionnez le lien **S'inscrire** et inscrivez un utilisateur. À ce stade, la seule validation sur l’adresse e-mail se fait avec l'attribut [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute). Après avoir soumis l’inscription, vous êtes connecté à l’application. Plus loin dans ce didacticiel, le code est mis à jour pour les nouveaux utilisateurs ne peuvent pas se connecter jusqu'à ce que leur courrier électronique est validé.

[!INCLUDE[](~/includes/view-identity-db.md)]

Notez que le champ `EmailConfirmed` de la table est à `False`.

Vous pouvez souhaiter réutiliser cet e-mail à l’étape suivante quand l’application envoie un e-mail de confirmation. Cliquez avec le bouton droit sur la ligne, puis sélectionnez **Supprimer**. La suppression de l’alias d’e-mail facilite les étapes suivantes.

<a name="prevent-login-at-registration"></a>

## <a name="require-email-confirmation"></a>Demander confirmation de l’e-mail

Il est recommandé de confirmer l’adresse de messagerie d’un nouvel enregistrement d’utilisateur. Envoyer un courrier électronique de confirmation permet de vérifier qu’ils n'empruntent pas l'identité d'une autre personne (autrement dit, ils ne se sont pas inscrits avec l'adresse de messageriedquelqu'un d’autre). Supposons que vous ayez un forum de discussion, et que vous souhaitiez empêcher "yli@example.com" de s’inscription en tant que "nolivetto@contoso.com". Sans la confirmation par courrier électronique, "nolivetto@contoso.com" peut recevoir un e-mail indésirable de votre application. Supposons que l’utilisateur s'inscrit par inadvertance en tant que "ylo@example.com" et que vous n’avez pas remarqué la faute d’orthographe de "yli". Ils ne pourraient pas utiliser la récupération de mot de passe, car l’application n’a pas leur courrier électronique correct. E-mail de confirmation offre une protection limitée contre les robots. L'email de confirmation ne fournit pas une protection contre les utilisateurs malveillants avec plusieurs comptes de messagerie.

En règle générale, vous souhaitez empêcher les nouveaux utilisateurs d'envoyer des données à votre site web tant que leur adresse e-mail n’est pas confirmée.

Mise à jour `Startup.ConfigureServices` pour exiger un e-mail de confirmation :

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=8-11)]

`config.SignIn.RequireConfirmedEmail = true;` empêche les utilisateurs enregistrés de se connecter jusqu'à ce que leur adresse de messagerie soit confirmée.

### <a name="configure-email-provider"></a>Configurer le fournisseur de messagerie

Dans ce didacticiel, [SendGrid](https://sendgrid.com) est utilisé pour envoyer un courrier électronique. Vous avez besoin d’un compte SendGrid et une clé pour envoyer un courrier électronique. Vous pouvez utiliser d’autres fournisseurs de messagerie. ASP.NET Core 2.x inclut `System.Net.Mail`, qui vous permet d’envoyer par courrier électronique à partir de votre application. Nous vous recommandons d'utiliser SendGrid ou un autre service de messagerie pour envoyer un courrier électronique. SMTP est difficile à sécuriser et à correctement configurer.

Créez une classe pour extraire la clé de messagerie électronique sécurisée. Pour cet exemple, créez *Services/AuthMessageSenderOptions.cs*:

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a>Configurez des secrets utilisateur SendGrid

Définissez `SendGridUser` et `SendGridKey` avec l'[outil Gestionnaire de secret](xref:security/app-secrets). Exemple :

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

Sur Windows, le gestionnaire de secret stocke des paires de clés/valeur dans un fichier *secrets.json* dans le répertoire `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>`.

Le contenu du fichier *secrets.json* n’est pas chiffré. L’exemple de balisage suivant le *secrets.json* fichier. Le `SendGridKey` valeur a été supprimée.

```json
{
  "SendGridUser": "RickAndMSFT",
  "SendGridKey": "<key removed>"
}
```

Pour plus d’informations, consultez le [modèle Options](xref:fundamentals/configuration/options) et [configuration](xref:fundamentals/configuration/index).

### <a name="install-sendgrid"></a>Installer SendGrid

Ce didacticiel montre comment ajouter des notifications par courrier électronique via [SendGrid](https://sendgrid.com/), mais vous pouvez envoyer des e-mails en utilisant SMTP et d'autres mécanismes.

Installer le package NuGet `SendGrid` :

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

À partir de la Console du Gestionnaire de Package, entrez la commande suivante :

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

À partir de la console, entrez la commande suivante :

```cli
dotnet add package SendGrid
```

------

Consultez [Débuter avec SendGrid gratuitement](https://sendgrid.com/free/) pour vous inscrire pour un compte SendGrid gratuit.

### <a name="implement-iemailsender"></a>Implémenter IEmailSender

L’implémentation `IEmailSender`, créer *Services/EmailSender.cs* avec un code similaire à ce qui suit :

[!code-csharp[](accconfirm/sample/WebPWrecover22/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a>Configurer le démarrage pour prendre en charge de messagerie

Ajoutez le code suivant à la `ConfigureServices` méthode dans le *Startup.cs* fichier :

* Ajouter `EmailSender` comme un service temporaire.
* Inscrire le `AuthMessageSenderOptions` instance de configuration.

[!code-csharp[](accconfirm/sample/WebPWrecover22/Startup.cs?name=snippet1&highlight=15-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a>Activer la récupération de confirmation et le mot de passe du compte

Le modèle contient du code pour la confirmation du compte et la récupération de mot de passe. Rechercher la `OnPostAsync` méthode dans *Areas/Identity/Pages/Account/Register.cshtml.cs*.

Empêcher les utilisateurs qui vient d’être inscrits d’être connecté automatiquement en commentant la ligne suivante :

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

La méthode complète est montrée avec la ligne modifiée mise en surbrillance :

[!code-csharp[](accconfirm/sample/WebPWrecover22/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a>Inscrire, confirmer l’adresse e-mail et réinitialiser le mot de passe

Exécuter l’application web, et testez le flux de confirmation du compte et de récupération du mot de passe.

* Exécutez l’application et inscrire un nouvel utilisateur
* Recherchez le lien de confirmation du compte dans votre messagerie. Consultez [Déboguer la messagerie](#debug) si vous ne recevez pas l’e-mail.
* Cliquez sur le lien pour confirmer votre adresse de messagerie.
* Connectez-vous à votre e-mail et un mot de passe.
* Se déconnecter.

### <a name="view-the-manage-page"></a>Afficher la page de gestion

Sélectionnez votre nom d’utilisateur dans le navigateur : ![fenêtre de navigateur avec le nom d’utilisateur](accconfirm/_static/un.png)

La page de gestion s’affiche avec l'onglet **profil** sélectionné. L'**Email** affiche une case à cocher indiquant que l’adresse de messagerie a été confirmée.

### <a name="test-password-reset"></a>Tester la réinitialisation du mot de passe

* Si vous êtes connecté, sélectionnez **déconnexion**.
* Sélectionnez le lien **Connexion** et sélectionnez le lien **Vous avez oublié votre mot de passe ?**.
* Entrez l’adresse e-mail que vous avez utilisé pour inscrire le compte.
* Un e-mail avec un lien pour réinitialiser votre mot de passe est envoyé. Vérifier votre adresse e-mail et cliquez sur le lien pour réinitialiser votre mot de passe. Une fois que votre mot de passe a été réinitialisé avec succès, vous pouvez connecter avec votre e-mail et un nouveau mot de passe.

## <a name="change-email-and-activity-timeout"></a>Modifier le délai d’attente de messagerie et d’activité

Le délai d’inactivité par défaut est de 14 jours. Le code suivant définit le délai d’inactivité à 5 jours :

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAppCookie.cs?name=snippet1)]

### <a name="change-all-data-protection-token-lifespans"></a>Modifier toutes les données protection jeton validité

Le code suivant modifie la période d’expiration de toutes les données protection jetons à 3 heures :

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupAllTokens.cs?name=snippet1&highlight=15-16)]

Intégré dans les jetons d’identité utilisateur (voir [AspNetCore/src/Identity/Extensions.Core/src/TokenOptions.cs](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) ) ont un [délai d’expiration d’une journée](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs).

### <a name="change-the-email-token-lifespan"></a>Modifier la durée de vie de jeton de courrier électronique

La durée de vie de jeton de valeur par défaut de [les jetons d’identité utilisateur](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Extensions.Core/src/TokenOptions.cs) est [un jour](https://github.com/aspnet/AspNetCore/blob/v2.2.2/src/Identity/Core/src/DataProtectionTokenProviderOptions.cs). Cette section montre comment modifier la durée de vie de jeton de courrier électronique.

Ajouter un formulaire personnalisé [DataProtectorTokenProvider\<TUser >](/dotnet/api/microsoft.aspnetcore.identity.dataprotectortokenprovider-1) et <xref:Microsoft.AspNetCore.Identity.DataProtectionTokenProviderOptions>:

[!code-csharp[](accconfirm/sample/WebPWrecover22/TokenProviders/CustomTokenProvider.cs?name=snippet1)]

Ajoutez le fournisseur personnalisé pour le conteneur de service :

[!code-csharp[](accconfirm/sample/WebPWrecover22/StartupEmail.cs?name=snippet1&highlight=10-13,18)]

### <a name="resend-email-confirmation"></a>Renvoyer l’e-mail de confirmation

Consultez [ce problème GitHub](https://github.com/aspnet/AspNetCore/issues/5410).

<a name="debug"></a>

### <a name="debug-email"></a>Déboguer le courrier électronique

Si vous ne parvenez à faire fonctionner l'email :

* Définir un point d’arrêt dans `EmailSender.Execute` pour vérifier `SendGridClient.SendEmailAsync` est appelée.
* Créer un [application console pour envoyer un e-mail](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) à l’aide d’un code similaire à `EmailSender.Execute`.
* Examinez la page [Activité de la messagerie](https://sendgrid.com/docs/User_Guide/email_activity.html) page.
* Vérifiez votre dossier courrier indésirable.
* Essayez un autre alias de messagerie sur un fournisseur de messagerie différents (Microsoft, Yahoo, Gmail, etc.).
* Essayer d’envoyer à différents comptes e-mail.

Une **bonne pratique de sécurité** consiste à **ne pas** utiliser des secrets de production en développement et en test. Si vous publiez l’application sur Azure, vous pouvez définir les secrets SendGrid en tant que paramètres d’application dans le portail Azure Web App. Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.

## <a name="combine-social-and-local-login-accounts"></a>Combiner des comptes de connexion de réseaux sociaux et local

Pour terminer cette section, vous devez d’abord activer un fournisseur d’authentification externe. Consultez [Facebook, Google et l’authentification du fournisseur externe](xref:security/authentication/social/index).

Vous pouvez combiner des comptes locaux et de réseaux sociaux en cliquant sur le lien de votre e-mail. Dans la séquence suivante, "RickAndMSFT@gmail.com" est d’abord créé en tant que connexion locale ; cependant, vous pouvez d’abord créer le compte en tant que connexion de réseau social, puis ajouter une connexion locale.

![Application Web : RickAndMSFT@gmail.com utilisateur authentifié](accconfirm/_static/rick.png)

Cliquez sur le lien **Gérer**. Notez la valeur 0 externes (connexions sociales) associé à ce compte.

![Gérer la vue](accconfirm/_static/manage.png)

Cliquez sur le lien vers un autre service de connexion et acceptez les demandes d’application. Dans l’image suivante, Facebook est le fournisseur d’authentification externe :

![Gérer votre vue de logins externes répertoriant Facebook](accconfirm/_static/fb.png)

Les deux comptes ont été combinés. Vous êtes en mesure de vous connecter avec un compte. Vous pouvez faire en sorte que vos utilisateurs ajoutent des comptes locaux au cas où leur service d’authentification de connexion sociale soit indisponible ou que, plus probablement, ils aient perdu l’accès à leur compte social.

## <a name="enable-account-confirmation-after-a-site-has-users"></a>Activer la confirmation de compte après qu’un site a des utilisateurs

Activer la confirmation du compte sur un site avec des utilisateurs bloque tous les utilisateurs existants. Ceux-ci sont bloqués, car leurs comptes ne sont pas confirmés. Pour contourner le blocage des utilisateurs existants, utilisez une des approches suivantes :

* Mise à jour de la base de données pour marquer tous les utilisateurs existants comme étant confirmés.
* Confirmer les utilisateurs existants. Par exemple, en envoyant par lot des e-mails avec des liens de confirmation.

::: moniker-end
