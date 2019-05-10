---
title: Twitter externe signe dans le programme d’installation avec ASP.NET Core
author: rick-anderson
description: Ce didacticiel montre l’intégration de l’authentification d’utilisateur de compte Twitter dans une application ASP.NET Core existante.
ms.author: riande
ms.custom: mvc
ms.date: 5/11/2019
uid: security/authentication/twitter-logins
ms.openlocfilehash: 486d58b600ca5326a0728de40bb386fbb9440f67
ms.sourcegitcommit: 3376f224b47a89acf329b2d2f9260046a372f924
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65516881"
---
# <a name="twitter-external-sign-in-setup-with-aspnet-core"></a>Twitter externe signe dans le programme d’installation avec ASP.NET Core

Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)

Cet exemple montre comment permettre aux utilisateurs de [vous connecter avec leur compte Twitter](https://dev.twitter.com/web/sign-in/desktop-browser) à l’aide d’un exemple de projet ASP.NET Core 2.2 créés sur le [page précédente](xref:security/authentication/social/index).

## <a name="create-the-app-in-twitter"></a>Créer l’application dans Twitter

* Accédez à [ https://apps.twitter.com/ ](https://apps.twitter.com/) et s’y connecter. Si vous ne disposez pas d’un compte Twitter, utilisez le **[s’inscrire maintenant](https://twitter.com/signup)** lien pour en créer un.

* Appuyez sur **créer une application** et remplissez la demande **nom**, **Description** et publics **site Web** URI (Cela peut être temporaire jusqu'à ce que vous Inscrivez le nom de domaine) :

* Entrez votre développement URI avec `/signin-twitter` ajoutées dans le **URI de redirection OAuth valides** champ (par exemple : `https://webapp128.azurewebsites.net/signin-twitter`). Le schéma d’authentification Twitter configuré plus loin dans cet exemple gère automatiquement les demandes à `/signin-twitter` itinéraire pour implémenter le flux OAuth.

  > [!NOTE]
  > Le segment d’URI `/signin-twitter` est défini en tant que le rappel par défaut du fournisseur d’authentification Twitter. Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel d’authentification de Twitter par le biais de l’élément hérité [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propriété de la [TwitterOptions](/dotnet/api/microsoft.aspnetcore.authentication.twitter.twitteroptions) classe.

* Remplissez le reste du formulaire, puis appuyez sur **créer votre application Twitter**. Détails de l’application sont affichent :

## <a name="storing-twitter-consumer-api-key-and-secret"></a>Stocker le secret et la clé d’API de consommateur Twitter

Exécutez les commandes suivantes pour stocker en toute sécurité `ClientId` et `ClientSecret` à l’aide de [Secret Manager](xref:security/app-secrets):

```console
dotnet user-secrets set Authentication:Twitter:ConsumerAPIKey <Key>
dotnet user-secrets set Authentication:Twitter:ConsumerAPISecret <Secret>
```

Lier des paramètres sensibles comme Twitter `Consumer Key` et `Consumer Secret` à votre configuration d’application en utilisant le [Secret Manager](xref:security/app-secrets). Dans le cadre de cet exemple, nommez les jetons `Authentication:Twitter:ConsumerKey` et `Authentication:Twitter:ConsumerSecret`.

Vous trouverez ces jetons sur le **Keys and Access Tokens** onglet après la création d’une application Twitter :

## <a name="configure-twitter-authentication"></a>Configurer l’authentification Twitter

Ajoutez le service Twitter dans le `ConfigureServices` méthode dans *Startup.cs* fichier :

[!code-csharp[](~/security/authentication/social/social-code/StartupTwitter.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Consultez le [TwitterOptions](/dotnet/api/microsoft.aspnetcore.builder.twitteroptions) référence des API pour plus d’informations sur les options de configuration prises en charge par l’authentification Twitter. Cela peut être utilisé pour demander différentes informations sur l’utilisateur.

## <a name="sign-in-with-twitter"></a>Connectez-vous à Twitter

Exécutez l’application et sélectionnez **connectez-vous**. Une option pour vous connecter avec Twitter s’affiche :

En cliquant sur **Twitter** redirige vers Twitter pour l’authentification :

Après avoir entré vos informations d’identification Twitter, vous êtes redirigé vers le site web où vous pouvez définir votre adresse de messagerie.

Vous êtes maintenant connecté à l’aide de vos informations d’identification Twitter :

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Résolution des problèmes

* **ASP.NET Core 2.x uniquement :** Si l’identité n’est pas configurée en appelant `services.AddIdentity` dans `ConfigureServices`, toute tentative authentifier entraîne *ArgumentException : L’option 'SignInScheme' doit être fournie*. Le modèle de projet utilisé dans cet exemple permet de s’assurer que cela est fait.
* Si la base de données de site n’a pas été créé en appliquant la migration initiale, vous obtiendrez *une opération de base de données a échoué lors du traitement de la demande* erreur. Appuyez sur **appliquer les Migrations** pour créer la base de données et actualiser pour passer à l’erreur.

## <a name="next-steps"></a>Étapes suivantes

* Cet article vous a montré comment vous pouvez vous authentifier avec Twitter. Vous pouvez suivre une approche similaire pour s’authentifier auprès d’autres fournisseurs répertoriés sur le [page précédente](xref:security/authentication/social/index).

* Une fois que vous publiez votre site web à l’application web Azure, vous devez réinitialiser le `ConsumerSecret` dans le portail des développeurs Twitter.

* Définir le `Authentication:Twitter:ConsumerKey` et `Authentication:Twitter:ConsumerSecret` en tant que paramètres d’application dans le portail Azure. Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.