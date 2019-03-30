---
title: Programme d’installation de la connexion externe Google dans ASP.NET Core
author: rick-anderson
description: Ce didacticiel montre l’intégration de l’authentification d’utilisateur de compte Google dans une application ASP.NET Core existante.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 1/11/2019
uid: security/authentication/google-logins
ms.openlocfilehash: 1328bcbce3e6e4786f9d410d1f28f309dc9d2722
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750547"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>Programme d’installation de la connexion externe Google dans ASP.NET Core

Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)

En janvier 2019 Google commencé à [arrêter](https://developers.google.com/+/api-shutdown) Google + se connecter et les développeurs doivent déplacer vers une nouvelle connexion de Google dans le système en mars. L’ASP.NET Core 2.1 et 2.2 packages pour l’authentification Google seront mis à jour en février pour prendre en compte les modifications. Pour plus d’informations et d’atténuation temporaire pour ASP.NET Core, consultez [ce problème GitHub](https://github.com/aspnet/AspNetCore/issues/6486). Ce didacticiel a été mis à jour avec le nouveau processus d’installation.

Ce didacticiel vous montre comment permettre aux utilisateurs de se connecter avec leur compte Google à l’aide du projet ASP.NET Core 2.2 créé sur le [page précédente](xref:security/authentication/social/index).

## <a name="create-a-google-api-console-project-and-client-id"></a>Créer un ID de client et de projet de Console d’API Google

* Accédez à [l’intégration de Google Sign-In dans votre application web](https://developers.google.com/identity/sign-in/web/devconsole-project) et sélectionnez **configurer un projet**.
* Dans le **configurer votre client OAuth** boîte de dialogue, sélectionnez **serveur Web**.
* Dans le **URI de redirection autorisée** zone de texte, définissez l’URI de redirection. Par exemple, `https://localhost:5001/signin-google`.
* Enregistrer le **ID Client** et **clé secrète Client**.
* Lorsque vous déployez le site, inscrire la nouvelle url publique à partir de la **Google Console**.

## <a name="store-google-clientid-and-clientsecret"></a>Store Google ClientID et ClientSecret

Store paramètres sensibles tels que Google `Client ID` et `Client Secret` avec la [Secret Manager](xref:security/app-secrets). Dans le cadre de ce didacticiel, nommez les jetons `Authentication:Google:ClientId` et `Authentication:Google:ClientSecret`:

```console
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

Vous pouvez gérer vos informations d’identification de l’API et l’utilisation dans le [Console d’API](https://console.developers.google.com/apis/dashboard).

## <a name="configure-google-authentication"></a>Configurer l’authentification Google

Ajouter le service Google `Startup.ConfigureServices`.

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a>Se connecter avec Google

* Exécutez l’application et cliquez sur **connectez-vous**. Une option pour vous connecter avec Google s’affiche.
* Cliquez sur le **Google** bouton, qui redirige vers Google pour l’authentification.
* Après avoir entré vos informations d’identification Google, vous êtes redirigé vers le site web.

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Consultez le [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) référence des API pour plus d’informations sur les options de configuration prises en charge par l’authentification Google. Cela peut être utilisé pour demander différentes informations sur l’utilisateur.

## <a name="change-the-default-callback-uri"></a>Modifier l’URI de rappel par défaut

Le segment d’URI `/signin-google` est défini en tant que le rappel par défaut du fournisseur d’authentification Google. Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel d’authentification Google via héritées [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propriété de la [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) classe.

## <a name="troubleshooting"></a>Résolution des problèmes

* Si l’authentification dans ne fonctionne pas et vous ne recevez pas les erreurs, basculer en mode de développement pour rendre le problème plus facile à déboguer.
* Si l’identité n’est pas configurée en appelant `services.AddIdentity` dans `ConfigureServices`, la tentative d’authentifier les résultats dans *ArgumentException : L’option 'SignInScheme' doit être fournie*. Le modèle de projet utilisé dans ce didacticiel permet de s’assurer que cela est fait.
* Si la base de données de site n’a pas été créé en appliquant la migration initiale, vous obtenez *une opération de base de données a échoué lors du traitement de la demande* erreur. Appuyez sur **appliquer les Migrations** pour créer la base de données et actualiser pour passer à l’erreur.

## <a name="next-steps"></a>Étapes suivantes

* Cet article vous a montré comment vous pouvez vous authentifier avec Google. Vous pouvez suivre une approche similaire pour s’authentifier auprès d’autres fournisseurs répertoriés sur le [page précédente](xref:security/authentication/social/index).
* Une fois que vous publiez l’application sur Azure, réinitialiser le `ClientSecret` dans la Console d’API Google.
* Définir le `Authentication:Google:ClientId` et `Authentication:Google:ClientSecret` en tant que paramètres d’application dans le portail Azure. Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.
