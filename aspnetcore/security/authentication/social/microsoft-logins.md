---
title: Programme d’installation de Microsoft Account connexion externe avec ASP.NET Core
author: rick-anderson
description: Ce didacticiel montre l’intégration de l’authentification d’utilisateur de compte Microsoft à une application ASP.NET Core existante.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 4909a0084994654777ad7a6ebda866ac727f0528
ms.sourcegitcommit: 3e94d192b2ed9409fe72e3735e158b333354964c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53735750"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>Programme d’installation de Microsoft Account connexion externe avec ASP.NET Core

Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)

Ce didacticiel vous montre comment autoriser vos utilisateurs à se connecter avec leur compte Microsoft à l’aide d’un exemple de projet ASP.NET Core 2.0 créée sur le [page précédente](xref:security/authentication/social/index).

## <a name="create-the-app-in-microsoft-developer-portal"></a>Créer l’application dans le portail des développeurs de Microsoft

* Accédez à [ https://apps.dev.microsoft.com ](https://apps.dev.microsoft.com) et créer ou vous connecter à un compte Microsoft :

![Boîte de dialogue se connecter](index/_static/MicrosoftDevLogin.png)

Si vous n’avez pas déjà un compte Microsoft, appuyez sur  **[créez-en un !](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=13&ct=1478151035&rver=6.7.6643.0&wp=SAPI_LONG&wreply=https%3a%2f%2fapps.dev.microsoft.com%2fLoginPostBack&id=293053&aadredir=1&contextid=D70D4F21246BAB50&bk=1478151036&uiflavor=web&uaid=f0c3de863a914c358b8dc01b1ff49e85&mkt=EN-US&lc=1033&lic=1)** Après vous être connecté, vous êtes redirigé vers **mes applications** page :

![Portail des développeurs Microsoft ouvert dans Microsoft Edge](index/_static/MicrosoftDev.png)

* Appuyez sur **ajouter une application** dans le coin supérieur droit d’angle, puis entrez votre **nom de l’Application** et **adresse E-mail de Contact**:

![Boîte de dialogue nouvelle inscription d’Application](index/_static/MicrosoftDevAppCreate.png)

* Dans le cadre de ce didacticiel, désactivez le **guidée par le programme d’installation** case à cocher.

* Appuyez sur **créer** pour continuer au **inscription** page. Fournir un **nom** et notez la valeur de la **Id d’Application**, que vous utilisez en tant que `ClientId` plus loin dans le didacticiel :

![Page d’inscription](index/_static/MicrosoftDevAppReg.png)

* Appuyez sur **ajouter une plateforme** dans le **plateformes** section et sélectionnez le **Web** plateforme :

![Ajouter la boîte de dialogue plateforme](index/_static/MicrosoftDevAppPlatform.png)

* Dans le nouveau **Web** plateforme section, entrez l’URL de votre développement avec `/signin-microsoft` ajoutées dans le **URL de redirection** champ (par exemple : `https://localhost:44320/signin-microsoft`). Le schéma d’authentification Microsoft configuré plus loin dans ce didacticiel gère automatiquement les demandes à `/signin-microsoft` itinéraire pour implémenter le flux OAuth :

![Section de plateforme Web](index/_static/MicrosoftRedirectUri.png)

> [!NOTE]
> Le segment d’URI `/signin-microsoft` est défini en tant que le rappel par défaut du fournisseur d’authentification de Microsoft. Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel d’authentification Microsoft via le hérité [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propriété de la [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) classe.

* Appuyez sur **ajouter une URL** afin de vérifier l’URL a été ajoutée.

* Remplissez les autres paramètres de l’application si nécessaire, puis appuyez sur **enregistrer** en bas de la page pour enregistrer les modifications apportées à la configuration de l’application.

* Lorsque vous déployez le site, vous devez revoir la **inscription** page et définir une nouvelle URL publique.

## <a name="store-microsoft-application-id-and-password"></a>Store Id d’Application de Microsoft et le mot de passe

* Remarque la `Application Id` affichée sur la **inscription** page.

* Appuyez sur **générer un nouveau mot de passe** dans le **Secrets d’Application** section. Cela affiche une zone où vous pouvez copier le mot de passe d’application :

![Boîte de dialogue Nouveau mot de passe généré](index/_static/MicrosoftDevPassword.png)

Lier des paramètres sensibles telles que Microsoft `Application ID` et `Password` à votre configuration d’application en utilisant le [Secret Manager](xref:security/app-secrets). Dans le cadre de ce didacticiel, nommez les jetons `Authentication:Microsoft:ApplicationId` et `Authentication:Microsoft:Password`.

## <a name="configure-microsoft-account-authentication"></a>Configurer l’authentification de compte Microsoft

Le modèle de projet utilisé dans ce didacticiel s’assure que [Microsoft.AspNetCore.Authentication.MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount) package est déjà installé.

* Pour installer ce package avec Visual Studio 2017, cliquez sur le projet, puis sélectionnez **gérer les Packages NuGet**.
* Pour installer avec l’interface CLI .NET Core, exécutez le code suivant dans votre répertoire de projet :

   `dotnet add package Microsoft.AspNetCore.Authentication.MicrosoftAccount`

::: moniker range=">= aspnetcore-2.0"

Ajoutez le service Account Microsoft dans le `ConfigureServices` méthode dans *Startup.cs* fichier :

```csharp
services.AddDefaultIdentity<IdentityUser>()
        .AddDefaultUI(UIFramework.Bootstrap4)
        .AddEntityFrameworkStores<ApplicationDbContext>();

services.AddAuthentication().AddMicrosoftAccount(microsoftOptions =>
{
    microsoftOptions.ClientId = Configuration["Authentication:Microsoft:ApplicationId"];
    microsoftOptions.ClientSecret = Configuration["Authentication:Microsoft:Password"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Ajoutez le middleware Account Microsoft dans le `Configure` méthode dans *Startup.cs* fichier :

```csharp
app.UseMicrosoftAccountAuthentication(new MicrosoftAccountOptions()
{
    ClientId = Configuration["Authentication:Microsoft:ApplicationId"],
    ClientSecret = Configuration["Authentication:Microsoft:Password"]
});
```

::: moniker-end

Bien que la terminologie utilisée sur le portail des développeurs Microsoft nomme ces jetons `ApplicationId` et `Password`, elles sont exposées en tant que `ClientId` et `ClientSecret` à l’API de configuration.

Consultez le [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) référence des API pour plus d’informations sur les options de configuration prises en charge par l’authentification de Microsoft Account. Cela peut être utilisé pour demander différentes informations sur l’utilisateur.

## <a name="sign-in-with-microsoft-account"></a>Connectez-vous avec un compte Microsoft

Exécutez votre application et cliquez sur **connectez-vous**. Une option permettant de se connecter avec Microsoft s’affiche :

![Application Web Log dans la page : Utilisateur non authentifié](index/_static/DoneMicrosoft.png)

Lorsque vous cliquez sur Microsoft, vous êtes redirigé vers Microsoft pour l’authentification. Après vous être connecté avec votre Account Microsoft (si pas déjà connecté), vous devez permettre à l’application d’accéder à vos informations :

![Boîte de dialogue d’authentification Microsoft](index/_static/MicrosoftLogin.png)

Appuyez sur **Oui** et vous êtes redirigé vers le site web où vous pouvez définir votre adresse de messagerie.

Vous êtes maintenant connecté à l’aide de vos informations d’identification Microsoft :

![Application Web : Utilisateur authentifié](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Résolution des problèmes

* Si le fournisseur Microsoft Account vous redirige vers une page d’erreur de connexion, notez l’erreur title et description paramètres chaîne de requête directement après le `#` (hashtag) dans l’Uri.

  Bien que le message d’erreur semble indiquer un problème avec l’authentification Microsoft, la cause la plus courante est votre application Uri ne pas corresponde à l’un de le **URI de redirection** spécifié pour le **Web** plateforme .
* **ASP.NET Core 2.x uniquement :** Si l’identité n’est pas configurée en appelant `services.AddIdentity` dans `ConfigureServices`, toute tentative authentifier entraîne *ArgumentException : L’option 'SignInScheme' doit être fournie*. Le modèle de projet utilisé dans ce didacticiel permet de s’assurer que cela est fait.
* Si la base de données de site n’a pas été créé en appliquant la migration initiale, vous obtiendrez *une opération de base de données a échoué lors du traitement de la demande* erreur. Appuyez sur **appliquer les Migrations** pour créer la base de données et actualiser pour passer à l’erreur.

## <a name="next-steps"></a>Étapes suivantes

* Cet article vous a montré comment vous pouvez vous authentifier auprès de Microsoft. Vous pouvez suivre une approche similaire pour s’authentifier auprès d’autres fournisseurs répertoriés sur le [page précédente](xref:security/authentication/social/index).

* Une fois que vous publiez votre site web à l’application web Azure, vous devez créer un nouveau `Password` dans le portail des développeurs Microsoft.

* Définir le `Authentication:Microsoft:ApplicationId` et `Authentication:Microsoft:Password` en tant que paramètres d’application dans le portail Azure. Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.
