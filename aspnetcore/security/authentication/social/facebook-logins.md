---
title: Programme d’installation de Facebook login externe dans ASP.NET Core
author: rick-anderson
description: Ce didacticiel montre l’intégration de l’authentification d’utilisateur de compte Facebook dans une application ASP.NET Core existante.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 11/11/2018
uid: security/authentication/facebook-logins
ms.openlocfilehash: d4f3e210b0d3c79eaf2233f97a29a6d96cd69b39
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284381"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a>Programme d’installation de Facebook login externe dans ASP.NET Core

Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)

Ce didacticiel vous montre comment autoriser vos utilisateurs à se connecter avec leur compte Facebook à l’aide d’un exemple de projet ASP.NET Core 2.0 créée sur le [page précédente](xref:security/authentication/social/index). L’authentification Facebook nécessite le [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package NuGet. Nous commençons par créer un ID d’application Facebook en suivant le [étapes officiels](https://developers.facebook.com).

## <a name="create-the-app-in-facebook"></a>Créer l’application dans Facebook

* Accédez à la [développeurs Facebook app](https://developers.facebook.com/apps/) page et s’y connecter. Si vous ne disposez pas d’un compte Facebook, utilisez le **s’inscrire à Facebook** sur la page de connexion pour créer un lien.

* Appuyez sur la **ajouter une nouvelle application** bouton dans le coin supérieur droit pour créer un ID d’application.

   ![Facebook pour le portail des développeurs ouverte dans Microsoft Edge](index/_static/FBMyApps.png)

* Remplissez le formulaire, puis appuyez sur la **Create App ID** bouton.

  ![Créer un formulaire de nouvel ID d’application](index/_static/FBNewAppId.png)

* Sur le **sélectionner un produit** , cliquez sur **Set Up** sur le **connexion Facebook** carte.

  ![Page d’installation du produit](index/_static/FBProductSetup.png)

* Le **Quickstart** Assistant lancera avec **choisir une plate-forme** en tant que la première page. Ignorer l’Assistant pour l’instant en cliquant sur le **paramètres** lien dans le menu de gauche :

  ![Démarrage rapide de Skip](index/_static/FBSkipQuickStart.png)

* Vous voyez la **paramètres du Client OAuth** page :

  ![Page Paramètres OAuth du client](index/_static/FBOAuthSetup.png)

* Entrez votre développement URI avec */signin-facebook* ajoutées dans le **URI de redirection OAuth valides** champ (par exemple : `https://localhost:44320/signin-facebook`). L’authentification Facebook configurée plus loin dans ce didacticiel gère automatiquement les demandes à */signin-facebook* itinéraire pour implémenter le flux OAuth.

> [!NOTE]
> L’URI */signin-facebook* est défini en tant que le rappel par défaut du fournisseur d’authentification Facebook. Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel d’authentification Facebook via héritées [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propriété de la [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) classe.

* Cliquez sur **enregistrer les modifications**.

* Cliquez sur **paramètres** > **base** lien dans le volet de navigation gauche.

  Dans cette page, prenez note de votre `App ID` et votre `App Secret`. Vous allez ajouter à la fois dans votre application ASP.NET Core dans la section suivante :

* Lorsque vous déployez le site dont vous avez besoin de revenir sur le **connexion Facebook** page d’installation et inscrire un nouvel URI public.

## <a name="store-facebook-app-id-and-app-secret"></a>ID d’application de Store Facebook et Secret d’application

Lier des paramètres sensibles comme Facebook `App ID` et `App Secret` à votre configuration d’application en utilisant le [Secret Manager](xref:security/app-secrets). Dans le cadre de ce didacticiel, nommez les jetons `Authentication:Facebook:AppId` et `Authentication:Facebook:AppSecret`.

Exécutez les commandes suivantes à stocker en toute sécurité `App ID` et `App Secret` à l’aide de Secret Manager :

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a>Configurer l’authentification Facebook

::: moniker range=">= aspnetcore-2.0"

Ajoutez le service de Facebook dans le `ConfigureServices` méthode dans le *Startup.cs* fichier :

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Installer le [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package.

* Pour installer ce package avec Visual Studio 2017, cliquez sur le projet, puis sélectionnez **gérer les Packages NuGet**.
* Pour installer avec l’interface CLI .NET Core, exécutez le code suivant dans votre répertoire de projet :

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

Ajoutez l’intergiciel (middleware) Facebook dans le `Configure` méthode dans *Startup.cs* fichier :

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

::: moniker-end

Consultez le [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) référence des API pour plus d’informations sur les options de configuration prises en charge par l’authentification Facebook. Options de configuration peuvent être utilisées pour :

* Demander différentes informations sur l’utilisateur.
* Ajouter des arguments de chaîne de requête pour personnaliser l’expérience de connexion.

## <a name="sign-in-with-facebook"></a>Se connecter avec Facebook

Exécutez votre application et cliquez sur **connectez-vous**. Vous voyez une option pour vous connecter avec Facebook.

![Application Web : Utilisateur non authentifié](index/_static/DoneFacebook.png)

Lorsque vous cliquez sur **Facebook**, vous êtes redirigé vers Facebook pour l’authentification :

![Page d’authentification Facebook](index/_static/FBLogin.png)

Les demandes d’authentification de Facebook publique profil et adresse de messagerie par défaut :

![Écran de consentement de page de l’authentification Facebook](index/_static/FBLoginDone.png)

Une fois que vous entrez vos informations d’identification Facebook, vous êtes redirigé vers votre site où vous pouvez définir votre adresse de messagerie.

Vous êtes maintenant connecté à l’aide de vos informations d’identification Facebook :

![Application Web : Utilisateur authentifié](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Résolution des problèmes

* **ASP.NET Core 2.x uniquement :** Si l’identité n’est pas configurée en appelant `services.AddIdentity` dans `ConfigureServices`, toute tentative authentifier entraîne *ArgumentException : L’option 'SignInScheme' doit être fournie*. Le modèle de projet utilisé dans ce didacticiel permet de s’assurer que cela est fait.
* Si la base de données de site n’a pas été créé en appliquant la migration initiale, vous obtenez *une opération de base de données a échoué lors du traitement de la demande* erreur. Appuyez sur **appliquer les Migrations** pour créer la base de données et actualiser pour passer à l’erreur.

## <a name="next-steps"></a>Étapes suivantes

* Cet article vous a montré comment vous pouvez vous authentifier avec Facebook. Vous pouvez suivre une approche similaire pour s’authentifier auprès d’autres fournisseurs répertoriés sur le [page précédente](xref:security/authentication/social/index).

* Une fois que vous publiez votre site web à l’application web Azure, vous devez réinitialiser le `AppSecret` dans le portail des développeurs Facebook.

* Définir le `Authentication:Facebook:AppId` et `Authentication:Facebook:AppSecret` en tant que paramètres d’application dans le portail Azure. Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.
