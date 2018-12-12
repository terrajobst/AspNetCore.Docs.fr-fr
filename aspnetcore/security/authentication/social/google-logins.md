---
title: Programme d’installation de la connexion externe Google dans ASP.NET Core
author: rick-anderson
description: Ce didacticiel montre l’intégration de l’authentification d’utilisateur de compte Google dans une application ASP.NET Core existante.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/11/2018
uid: security/authentication/google-logins
ms.openlocfilehash: 372504eb4f6fea412b5b160e0d5e9251dafe0d56
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284485"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>Programme d’installation de la connexion externe Google dans ASP.NET Core

Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)

Ce didacticiel vous montre comment autoriser vos utilisateurs à se connecter avec leur compte Google + à l’aide d’un exemple de projet ASP.NET Core 2.0 créée sur le [page précédente](xref:security/authentication/social/index). Nous commençons en suivant le [étapes officiels](https://developers.google.com/identity/sign-in/web/devconsole-project) pour créer une nouvelle application dans la Console d’API Google.

## <a name="create-the-app-in-google-api-console"></a>Créer l’application dans la Console d’API Google

* Accédez à [ https://console.developers.google.com/projectselector/apis/library ](https://console.developers.google.com/projectselector/apis/library) et s’y connecter. Si vous ne disposez pas d’un compte Google, utilisez **davantage d’options** > **[créer compte](https://accounts.google.com/SignUpWithoutGmail?service=cloudconsole&continue=https%3A%2F%2Fconsole.developers.google.com%2Fprojectselector%2Fapis%2Flibrary&ltmpl=api)**  lien pour en créer un :

![Console d’API Google](index/_static/GoogleConsoleLogin.png)

* Vous êtes redirigé vers **bibliothèque API Manager** page :

![Dans la page de la bibliothèque du Gestionnaire de l’API de lancement](index/_static/GoogleConsoleSwitchboard.png)

* Appuyez sur **créer** et entrez votre **nom_projet**:

![Boîte de dialogue Nouveau projet](index/_static/GoogleConsoleNewProj.png)

* Après avoir accepté la boîte de dialogue, vous êtes redirigé vers la page de bibliothèque vous permettant de choisir des fonctionnalités pour votre nouvelle application. Rechercher **API Google +** dans la liste et cliquez sur son lien pour ajouter la fonctionnalité d’API :

![Recherchez « API Google + » dans la page de la bibliothèque API Manager](index/_static/GoogleConsoleChooseApi.png)

* La page de l’API nouvellement ajoutée s’affiche. Appuyez sur **activer** pour ajouter Google + signe fonctionnalité à votre application :

![Sur la page Gestionnaire de l’API Google + API de lancement](index/_static/GoogleConsoleEnableApi.png)

* Après l’activation de l’API, appuyez sur **créer les informations d’identification** pour configurer les clés secrètes :

![Créer un bouton informations d’identification sur la page Gestionnaire de l’API Google + API](index/_static/GoogleConsoleGoCredentials.png)

* Choisissez :
  * **Google + API**
  * **Serveur Web (par exemple, node.js, Tomcat)**, et
  * **Données utilisateur**:

![Page d’informations d’identification de l’API Manager : Découvrez quel type d’informations d’identification vous avez besoin de panneau](index/_static/GoogleConsoleChooseCred.png)

* Appuyez sur **les informations d’identification ai-je besoin ?** afin d’accéder à la deuxième étape de configuration de l’application, **créer un ID de client OAuth 2.0**:

![Page d’informations d’identification de l’API Manager : Créer un ID de client OAuth 2.0](index/_static/GoogleConsoleCreateClient.png)

* Étant donné que nous créons un projet Google + avec une seule caractéristique (connexion), nous pouvons saisir les mêmes **nom** pour l’ID de client OAuth 2.0 que celui que nous avons utilisé pour le projet.

* Entrez votre développement URI avec `/signin-google` ajoutées dans le **URI de redirection autorisée** champ (par exemple : `https://localhost:44320/signin-google`). L’authentification Google configurée plus loin dans ce didacticiel gère automatiquement les demandes à `/signin-google` itinéraire pour implémenter le flux OAuth.

> [!NOTE]
> Le segment d’URI `/signin-google` est défini en tant que le rappel par défaut du fournisseur d’authentification Google. Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel d’authentification Google via héritées [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propriété de la [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) classe.

* Appuyez sur TAB pour ajouter le **URI de redirection autorisée** entrée.

* Appuyez sur **créer un identifiant client**, ce qui vous amène à la troisième étape, **configurer à l’écran de consentement OAuth 2.0**:

![Page d’informations d’identification de l’API Manager : Configurer l’écran de consentement OAuth 2.0](index/_static/GoogleConsoleAddCred.png)

* Entrez votre publics **adresse de messagerie** et **Product name** indiqué pour votre application lorsque Google + invite l’utilisateur à se connecter. Options supplémentaires sont disponibles sous **les options de personnalisation plus**.

* Appuyez sur **continuer** pour passer à la dernière étape, **télécharger les informations d’identification**:

![Page d’informations d’identification de l’API Manager : Télécharger les informations d’identification](index/_static/GoogleConsoleFinish.png)

* Appuyez sur **télécharger** pour enregistrer un fichier JSON avec des secrets d’application, et **fait** pour terminer la création de la nouvelle application.

* Lorsque vous déployez le site, vous devez revoir la **Google Console** et inscrire une nouvelle url publique.

## <a name="store-google-clientid-and-clientsecret"></a>Store Google ClientID et ClientSecret

Lier des paramètres sensibles comme Google `Client ID` et `Client Secret` à votre configuration d’application en utilisant le [Secret Manager](xref:security/app-secrets). Dans le cadre de ce didacticiel, nommez les jetons `Authentication:Google:ClientId` et `Authentication:Google:ClientSecret`.

Vous trouverez les valeurs de ces jetons dans le fichier JSON téléchargé à l’étape précédente sous `web.client_id` et `web.client_secret`.

## <a name="configure-google-authentication"></a>Configurer l’authentification Google

::: moniker range=">= aspnetcore-2.0"

Ajoutez le service Google dans le `ConfigureServices` méthode dans *Startup.cs* fichier :

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddGoogle(googleOptions =>
{
    googleOptions.ClientId = Configuration["Authentication:Google:ClientId"];
    googleOptions.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Le modèle de projet utilisé dans ce didacticiel s’assure que [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google) package est installé.

* Pour installer ce package avec Visual Studio 2017, cliquez sur le projet, puis sélectionnez **gérer les Packages NuGet**.
* Pour installer avec l’interface CLI .NET Core, exécutez le code suivant dans votre répertoire de projet :

`dotnet add package Microsoft.AspNetCore.Authentication.Google`

Ajoutez l’intergiciel (middleware) Google dans le `Configure` méthode dans *Startup.cs* fichier :

```csharp
app.UseGoogleAuthentication(new GoogleOptions()
{
    ClientId = Configuration["Authentication:Google:ClientId"],
    ClientSecret = Configuration["Authentication:Google:ClientSecret"]
});
```

::: moniker-end

Consultez le [GoogleOptions](/dotnet/api/microsoft.aspnetcore.builder.googleoptions) référence des API pour plus d’informations sur les options de configuration prises en charge par l’authentification Google. Cela peut être utilisé pour demander différentes informations sur l’utilisateur.

## <a name="sign-in-with-google"></a>Se connecter avec Google

Exécutez votre application et cliquez sur **connectez-vous**. Une option pour vous connecter avec Google s’affiche :

![Application Web en cours d’exécution dans Microsoft Edge : Utilisateur non authentifié](index/_static/DoneGoogle.png)

Lorsque vous cliquez sur Google, vous êtes redirigé vers Google pour l’authentification :

![Boîte de dialogue authentification Google](index/_static/GoogleLogin.png)

Après avoir entré vos informations d’identification Google, puis vous êtes redirigé vers le site web où vous pouvez définir votre adresse de messagerie.

Vous êtes maintenant connecté à l’aide de vos informations d’identification Google :

![Application Web en cours d’exécution dans Microsoft Edge : Utilisateur authentifié](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Résolution des problèmes

* Si vous recevez un `403 (Forbidden)` page d’erreur à partir de votre propre application lors de l’exécution en mode de développement (ou s’arrêter dans le débogueur avec la même erreur), vérifiez que **API Google +** a été activée dans le **bibliothèque d’API Manager** en suivant les étapes répertoriées [antérieures sur cette page](#create-the-app-in-google-api-console). Si la connexion ne fonctionne pas et vous ne recevez pas les erreurs, basculer en mode de développement pour rendre le problème plus facile à déboguer.
* **ASP.NET Core 2.x uniquement :** Si l’identité n’est pas configurée en appelant `services.AddIdentity` dans `ConfigureServices`, toute tentative authentifier entraîne *ArgumentException : L’option 'SignInScheme' doit être fournie*. Le modèle de projet utilisé dans ce didacticiel permet de s’assurer que cela est fait.
* Si la base de données de site n’a pas été créé en appliquant la migration initiale, vous obtiendrez *une opération de base de données a échoué lors du traitement de la demande* erreur. Appuyez sur **appliquer les Migrations** pour créer la base de données et actualiser pour passer à l’erreur.

## <a name="next-steps"></a>Étapes suivantes

* Cet article vous a montré comment vous pouvez vous authentifier avec Google. Vous pouvez suivre une approche similaire pour s’authentifier auprès d’autres fournisseurs répertoriés sur le [page précédente](xref:security/authentication/social/index).

* Une fois que vous publiez votre site web à l’application web Azure, vous devez réinitialiser le `ClientSecret` dans la Console d’API Google.

* Définir le `Authentication:Google:ClientId` et `Authentication:Google:ClientSecret` en tant que paramètres d’application dans le portail Azure. Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.
