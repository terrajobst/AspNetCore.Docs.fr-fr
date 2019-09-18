---
title: Programme d’installation de la connexion externe Google dans ASP.NET Core
author: rick-anderson
description: Ce didacticiel montre l’intégration de l’authentification d’utilisateur de compte Google dans une application ASP.NET Core existante.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/19/2019
uid: security/authentication/google-logins
ms.openlocfilehash: e12d831d2e0a5c9acae5ea41fb4187ad4ca6b0ea
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082485"
---
# <a name="google-external-login-setup-in-aspnet-core"></a>Programme d’installation de la connexion externe Google dans ASP.NET Core

Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)

[Les API Google + héritées ont été arrêtées depuis le 7 mars 2019](https://developers.google.com/+/api-shutdown). Google + Connect et les développeurs doivent passer à un nouveau système de connexion Google. Les packages ASP.NET Core 2,1 et 2,2 pour l’authentification Google ont été mis à jour pour prendre en compte les modifications. Pour plus d’informations et pour obtenir des solutions de contournement temporaires pour ASP.NET Core, consultez [ce problème GitHub](https://github.com/aspnet/AspNetCore/issues/6486). Ce didacticiel a été mis à jour avec le nouveau processus d’installation.

Ce didacticiel vous montre comment permettre aux utilisateurs de se connecter avec leur compte Google à l’aide du projet ASP.NET Core 2,2 créé sur la [page précédente](xref:security/authentication/social/index).

## <a name="create-a-google-api-console-project-and-client-id"></a>Créer un projet de console d’API Google et un ID client

* Accédez à [l’intégration de la connexion Google à votre application Web](https://developers.google.com/identity/sign-in/web/devconsole-project) , puis sélectionnez **configurer un projet**.
* Dans la boîte de dialogue **configurer votre client OAuth** , sélectionnez **serveur Web**.
* Dans la zone d’entrée de texte **URI de redirection autorisés** , définissez l’URI de redirection. Par exemple, `https://localhost:5001/signin-google`.
* Enregistrez l' **ID client** et la **clé secrète client**.
* Lors du déploiement du site, inscrivez la nouvelle URL publique à partir de la **console Google**.

## <a name="store-google-clientid-and-clientsecret"></a>Store Google ClientID et ClientSecret

Stockez les paramètres sensibles tels que `Client ID` Google `Client Secret` et avec le [Gestionnaire de secret](xref:security/app-secrets). Dans le cadre de ce didacticiel, nommez les jetons `Authentication:Google:ClientId` et `Authentication:Google:ClientSecret`:

```dotnetcli
dotnet user-secrets set "Authentication:Google:ClientId" "X.apps.googleusercontent.com"
dotnet user-secrets set "Authentication:Google:ClientSecret" "<client secret>"
```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

Vous pouvez gérer les informations d’identification et l’utilisation de votre API dans la [console d’API](https://console.developers.google.com/apis/dashboard).

## <a name="configure-google-authentication"></a>Configurer l’authentification Google

Ajoutez le service Google à `Startup.ConfigureServices`:

[!code-csharp[](~/security/authentication/social/social-code/StartupGoogle.cs?name=snippet_ConfigureServices&highlight=10-18)]

[!INCLUDE [default settings configuration](includes/default-settings2-2.md)]

## <a name="sign-in-with-google"></a>Se connecter avec Google

* Exécutez l’application, puis cliquez sur **se connecter**. Une option de connexion avec Google s’affiche.
* Cliquez sur le bouton **Google** , qui redirige vers Google pour l’authentification.
* Après avoir entré vos informations d’identification Google, vous êtes redirigé vers le site Web.

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Pour plus <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> d’informations sur les options de configuration prises en charge par l’authentification Google, consultez la référence de l’API. Cela peut être utilisé pour demander différentes informations sur l’utilisateur.

## <a name="change-the-default-callback-uri"></a>Modifier l’URI de rappel par défaut

Le segment d’URI `/signin-google` est défini en tant que le rappel par défaut du fournisseur d’authentification Google. Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel d’authentification Google via héritées [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propriété de la [GoogleOptions](/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) classe.

## <a name="troubleshooting"></a>Résolution des problèmes

* Si la connexion ne fonctionne pas et que vous ne recevez pas d’erreurs, passez en mode développement pour faciliter le débogage du problème.
* Si l’identité n’est pas `services.AddIdentity` configurée en appelant dans `ConfigureServices`, une *tentative d’authentification des résultats est ArgumentException : L’option « SignInScheme » doit être fournie*. Le modèle de projet utilisé dans ce didacticiel permet de s’assurer que cela est fait.
* Si la base de données de site n’a pas été créé en appliquant la migration initiale, vous obtenez *une opération de base de données a échoué lors du traitement de la demande* erreur. Sélectionnez **appliquer les migrations** pour créer la base de données, puis actualisez la page pour poursuivre l’erreur.

## <a name="next-steps"></a>Étapes suivantes

* Cet article vous a montré comment vous pouvez vous authentifier avec Google. Vous pouvez suivre une approche similaire pour s’authentifier auprès d’autres fournisseurs répertoriés sur le [page précédente](xref:security/authentication/social/index).
* Une fois que vous avez publié l’application dans Azure `ClientSecret` , réinitialisez la dans la console de l’API Google.
* Définir le `Authentication:Google:ClientId` et `Authentication:Google:ClientSecret` en tant que paramètres d’application dans le portail Azure. Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.
