---
title: Configuration de la connexion externe Twitter avec ASP.NET Core
author: rick-anderson
description: Ce didacticiel illustre l’intégration de l’authentification utilisateur de compte Twitter dans une application ASP.NET Core existante.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/twitter-logins
ms.openlocfilehash: 5182f1647acb664bf35f086fcddbe909559a62f7
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082302"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a>Configuration de la connexion externe Twitter avec ASP.NET Core

Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)

Cet exemple montre comment permettre aux utilisateurs de [se connecter avec leur compte Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) à l’aide d’un exemple de projet ASP.net Core 2,2 créé sur la [page précédente](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>Créer l’application dans Twitter

* Accédez à [ https://apps.twitter.com/ ](https://apps.twitter.com/) et s’y connecter. Si vous ne disposez pas déjà d’un compte Twitter, utilisez le lien **[Inscrivez-vous maintenant](https://twitter.com/signup)** pour en créer un.

* Appuyez sur **créer une nouvelle application** et renseignez le **nom**de l’application, la **Description** et l’URI du **site Web** public (cela peut être temporaire jusqu’à ce que vous enregistriez le nom de domaine) :

* Entrez votre URI de développement `/signin-twitter` avec ajouté dans le champ **URI de redirection OAuth valide** (par exemple `https://webapp128.azurewebsites.net/signin-twitter`:). Le schéma d’authentification Twitter configuré plus tard dans cet exemple gère automatiquement les `/signin-twitter` demandes à l’itinéraire pour implémenter le Flow OAuth.

  > [!NOTE]
  > Le segment `/signin-twitter` d’URI est défini en tant que rappel par défaut du fournisseur d’authentification Twitter. Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel d’authentification Twitter via la propriété héritée [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) de la classe [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) .

* Remplissez le reste du formulaire et appuyez sur **créer votre application Twitter**. Les détails de la nouvelle application s’affichent :

## <a name="storing-twitter-consumer-api-key-and-secret"></a>Stockage de la clé et du secret de l’API du consommateur Twitter

Exécutez les commandes suivantes pour stocker `ClientId` et `ClientSecret` utiliser le [Gestionnaire de secret](xref:security/app-secrets)en toute sécurité :

```dotnetcli
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerSecret <Secret>
```

Liez des paramètres sensibles tels `Consumer Key` que `Consumer Secret` Twitter et à la configuration de votre application à l’aide du [Gestionnaire de secret](xref:security/app-secrets). Pour les besoins de cet exemple, nommez les jetons `Authentication:Twitter:ConsumerKey` et `Authentication:Twitter:ConsumerSecret`.

Ces jetons se trouvent sous l’onglet **clés et jetons d’accès** après la création d’une application Twitter :

## <a name="configure-twitter-authentication"></a>Configurer l’authentification Twitter

Ajoutez le service Twitter dans la `ConfigureServices` méthode dans le fichier *Startup.cs* :

[!code-csharp[](~/security/authentication/social/social-code/StartupTwitter.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Pour plus d’informations sur les options de configuration prises en charge par l’authentification Twitter, consultez la référence de l’API [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) . Cela peut être utilisé pour demander différentes informations sur l’utilisateur.

## <a name="sign-in-with-twitter"></a>Se connecter avec Twitter

Exécutez l’application et sélectionnez **se connecter**. Une option permettant de se connecter avec Twitter s’affiche :

En cliquant sur **Twitter** , vous redirigez vers Twitter pour l’authentification :

Après avoir entré vos informations d’identification Twitter, vous êtes redirigé vers le site Web où vous pouvez définir votre adresse de messagerie.

Vous êtes maintenant connecté à l’aide de vos informations d’identification Twitter :

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Résolution des problèmes

* **ASP.NET Core 2. x uniquement :** Si l’identité n’est pas `services.AddIdentity` configurée en appelant dans `ConfigureServices`, toute tentative *d’authentification entraînera l’exception ArgumentException : L’option « SignInScheme » doit être fournie*. Le modèle de projet utilisé dans cet exemple permet de s’assurer que cette opération est effectuée.
* Si la base de données de site n’a pas été créé en appliquant la migration initiale, vous obtiendrez *une opération de base de données a échoué lors du traitement de la demande* erreur. Appuyez sur **appliquer les Migrations** pour créer la base de données et actualiser pour passer à l’erreur.

## <a name="next-steps"></a>Étapes suivantes

* Cet article vous a montré comment vous pouvez vous authentifier avec Twitter. Vous pouvez suivre une approche similaire pour s’authentifier auprès d’autres fournisseurs répertoriés sur le [page précédente](xref:security/authentication/social/index).

* Une fois que vous publiez votre site Web sur Azure Web App, vous `ConsumerSecret` devez réinitialiser le dans le portail des développeurs Twitter.

* Définir le `Authentication:Twitter:ConsumerKey` et `Authentication:Twitter:ConsumerSecret` en tant que paramètres d’application dans le portail Azure. Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.
