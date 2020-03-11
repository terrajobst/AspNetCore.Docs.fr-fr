---
title: Programme d’installation de Facebook login externe dans ASP.NET Core
author: rick-anderson
description: Didacticiel avec des exemples de code illustrant l’intégration de l’authentification utilisateur de compte Facebook dans une application ASP.NET Core existante.
ms.author: riande
ms.custom: seoapril2019, mvc, seodec18
ms.date: 12/02/2019
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/facebook-logins
ms.openlocfilehash: 2e4cc04c6e7ff8e5f5701cc7f9ede73dbc1b4685
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667467"
---
# <a name="facebook-external-login-setup-in-aspnet-core"></a>Programme d’installation de Facebook login externe dans ASP.NET Core

Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)

Ce didacticiel avec des exemples de code montre comment permettre à vos utilisateurs de se connecter avec leur compte Facebook à l’aide d’un exemple de projet ASP.NET Core 3,0 créé sur la [page précédente](xref:security/authentication/social/index). Nous commençons par créer un ID d’application Facebook en suivant les [étapes officielles](https://developers.facebook.com).

## <a name="create-the-app-in-facebook"></a>Créer l’application dans Facebook

* Ajoutez le package NuGet [Microsoft. AspNetCore. Authentication. Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) au projet.

* Accédez à la page de l' [application développeurs Facebook](https://developers.facebook.com/apps/) et connectez-vous. Si vous ne disposez pas déjà d’un compte Facebook, utilisez le lien s' **inscrire à Facebook** sur la page de connexion pour en créer un.  Une fois que vous disposez d’un compte Facebook, suivez les instructions pour vous inscrire en tant que développeur Facebook.

* Dans le menu **mes applications** , sélectionnez **créer une application** pour créer un nouvel ID d’application.

   ![Facebook pour le portail des développeurs ouverte dans Microsoft Edge](index/_static/FBMyApps.png)

* Remplissez le formulaire et appuyez sur le bouton **créer un ID d’application** .

  ![Créer un formulaire de nouvel ID d’application](index/_static/FBNewAppId.png)

* Sur la nouvelle carte d’application, sélectionnez **Ajouter un produit**.  Sur la carte de **connexion Facebook** , cliquez sur **configurer** . 

  ![Page d’installation du produit](index/_static/FBProductSetup.png)

* L’Assistant **démarrage rapide** démarre avec **choisir une plateforme** comme première page. Ignorez l’Assistant pour le moment en cliquant sur le lien **paramètres** de **connexion Facebook** dans le menu situé en bas à gauche :

  ![Démarrage rapide de Skip](index/_static/FBSkipQuickStart.png)

* La page **paramètres OAuth du client** s’affiche :

  ![Page Paramètres OAuth du client](index/_static/FBOAuthSetup.png)

* Entrez votre URI de développement avec */SignIn-Facebook* ajouté dans le champ **URI de redirection OAuth valide** (par exemple : `https://localhost:44320/signin-facebook`). L’authentification Facebook configurée plus tard dans ce didacticiel gère automatiquement les demandes à l’itinéraire */SignIn-Facebook* pour implémenter le Flow OAuth.

> [!NOTE]
> L’URI */SignIn-Facebook* est défini comme rappel par défaut du fournisseur d’authentification Facebook. Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel d’authentification Facebook à l’aide de la propriété héritée [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) de la classe [FacebookOptions](/dotnet/api/microsoft.aspnetcore.authentication.facebook.facebookoptions) .

* Cliquez sur **Enregistrer les modifications**.

* Cliquez sur **paramètres** > lien de **base** dans le volet de navigation gauche.

  Sur cette page, prenez note de votre `App ID` et de votre `App Secret`. Vous allez ajouter à la fois dans votre application ASP.NET Core dans la section suivante :

* Lors du déploiement du site, vous devez revisiter la page de configuration de la **connexion Facebook** et inscrire un nouvel URI public.

## <a name="store-facebook-app-id-and-app-secret"></a>ID d’application de Store Facebook et Secret d’application

Liez des paramètres sensibles comme Facebook `App ID` et `App Secret` à la configuration de votre application à l’aide du [Gestionnaire de secret](xref:security/app-secrets). Dans le cadre de ce didacticiel, nommez les jetons `Authentication:Facebook:AppId` et `Authentication:Facebook:AppSecret`.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

Exécutez les commandes suivantes pour stocker `App ID` et `App Secret` en toute sécurité à l’aide du gestionnaire de secret :

```dotnetcli
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a>Configurer l’authentification Facebook

Ajoutez le service Facebook dans la méthode `ConfigureServices` dans le fichier *Startup.cs* :

```csharp
services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Pour plus d’informations sur les options de configuration prises en charge par l’authentification Facebook, consultez la référence de l’API [FacebookOptions](/dotnet/api/microsoft.aspnetcore.builder.facebookoptions) . Options de configuration peuvent être utilisées pour :

* Demander différentes informations sur l’utilisateur.
* Ajouter des arguments de chaîne de requête pour personnaliser l’expérience de connexion.

## <a name="sign-in-with-facebook"></a>Se connecter avec Facebook

Exécutez votre application, puis cliquez sur **se connecter**. Vous voyez une option pour vous connecter avec Facebook.

![Application Web : utilisateur non authentifié](index/_static/DoneFacebook.png)

Lorsque vous cliquez sur **Facebook**, vous êtes redirigé vers Facebook pour l’authentification :

![Page d’authentification Facebook](index/_static/FBLogin.png)

Les demandes d’authentification de Facebook publique profil et adresse de messagerie par défaut :

![Écran de consentement de page de l’authentification Facebook](index/_static/FBLoginDone.png)

Une fois que vous entrez vos informations d’identification Facebook, vous êtes redirigé vers votre site où vous pouvez définir votre adresse de messagerie.

Vous êtes maintenant connecté à l’aide de vos informations d’identification Facebook :

![Application Web : utilisateur authentifié](index/_static/Done.png)

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Dépannage

* **ASP.net Core 2. x uniquement :** Si l’identité n’est pas configurée en appelant `services.AddIdentity` dans `ConfigureServices`, toute tentative d’authentification entraînera *l’exception ArgumentException : l’option « SignInScheme » doit être fournie*. Le modèle de projet utilisé dans ce didacticiel permet de s’assurer que cela est fait.
* Si la base de données de site n’a pas été créée en appliquant la migration initiale, vous recevez *une opération de base de données qui a échoué lors du traitement de l’erreur de demande* . Appuyez sur **appliquer des migrations** pour créer la base de données, puis sur Actualiser pour poursuivre l’erreur.

## <a name="next-steps"></a>Étapes suivantes

* Cet article vous a montré comment vous pouvez vous authentifier avec Facebook. Vous pouvez suivre une approche similaire pour vous authentifier auprès d’autres fournisseurs listés dans la [page précédente](xref:security/authentication/social/index).

* Une fois que vous publiez votre site Web sur Azure Web App, vous devez réinitialiser le `AppSecret` dans le portail des développeurs Facebook.

* Définissez les `Authentication:Facebook:AppId` et `Authentication:Facebook:AppSecret` en tant que paramètres d’application dans le Portail Azure. Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.
