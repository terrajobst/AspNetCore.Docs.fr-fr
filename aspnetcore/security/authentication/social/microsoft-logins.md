---
title: Configuration de la connexion externe à un compte Microsoft avec ASP.NET Core
author: rick-anderson
description: Cet exemple illustre l’intégration de compte Microsoft l’authentification utilisateur dans une application ASP.NET Core existante.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 91ace293fd16cd180b3d5c183c637af6db1d08c3
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082339"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>Configuration de la connexion externe à un compte Microsoft avec ASP.NET Core

Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)

Cet exemple montre comment permettre aux utilisateurs de se connecter avec leur compte Microsoft à l’aide du projet ASP.NET Core 2,2 créé sur la [page précédente](xref:security/authentication/social/index).

## <a name="create-the-app-in-microsoft-developer-portal"></a>Créer l’application dans le portail des développeurs Microsoft

* Accédez à la page de [inscriptions d’applications portail Azure](https://go.microsoft.com/fwlink/?linkid=2083908) et créez ou connectez-vous à une compte Microsoft :

Si vous n’avez pas de compte Microsoft, sélectionnez en **créer un**. Après vous être connecté, vous êtes redirigé vers la page **inscriptions d’applications** :

* Sélectionner une **nouvelle inscription**
* Entrez un **nom**.
* Sélectionnez une option pour les **types de comptes pris en charge**.  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* Sous **URI de redirection**, entrez votre URL de `/signin-microsoft` développement avec le suffixe. Par exemple, `https://localhost:44389/signin-microsoft`. Le schéma d’authentification Microsoft configuré plus tard dans cet exemple gère automatiquement les `/signin-microsoft` demandes à l’itinéraire pour implémenter le Flow OAuth.
* Sélectionner un **Registre**

### <a name="create-client-secret"></a>Créer une clé secrète client

* Dans le volet gauche, sélectionnez **certificats & secrets**.
* Sous **secrets client**, sélectionnez **nouvelle clé secrète client** .

  * Ajoutez une description pour la clé secrète client.
  * Sélectionnez le bouton **Ajouter** .

* Sous **secrets clients**, copiez la valeur de la clé secrète client.

> [!NOTE]
> Le segment `/signin-microsoft` d’URI est défini en tant que rappel par défaut du fournisseur d’authentification Microsoft. Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel (middleware) d’authentification Microsoft via la propriété héritée [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) de la classe [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) .

## <a name="store-the-microsoft-client-id-and-client-secret"></a>Stocker l’ID client et la clé secrète client Microsoft

Exécutez les commandes suivantes pour stocker `ClientId` et `ClientSecret` utiliser le [Gestionnaire de secret](xref:security/app-secrets)en toute sécurité :

```dotnetcli
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

Liez des paramètres sensibles tels `ClientId` que `ClientSecret` Microsoft et à la configuration de votre application à l’aide du [Gestionnaire de secret](xref:security/app-secrets). Pour les besoins de cet exemple, nommez les jetons `Authentication:Microsoft:ClientId` et `Authentication:Microsoft:ClientSecret`.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a>Configurer l’authentification de compte Microsoft

Ajoutez le service de compte Microsoft `Startup.ConfigureServices`à :

[!code-csharp[](~/security/authentication/social/social-code/StartupMS.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Pour plus d’informations sur les options de configuration prises en charge par l’authentification de compte Microsoft, consultez la référence de l’API [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) . Cela peut être utilisé pour demander différentes informations sur l’utilisateur.

## <a name="sign-in-with-microsoft-account"></a>Compte Se connecter avec Microsoft

Exécutez le, puis cliquez sur **se connecter**. Une option permettant de se connecter à Microsoft s’affiche. Lorsque vous cliquez sur Microsoft, vous êtes redirigé vers Microsoft pour l’authentification. Après vous être connecté avec votre compte Microsoft (s’il n’est pas déjà connecté), vous êtes invité à autoriser l’application à accéder à vos informations :

Appuyez sur **Oui** . vous êtes alors redirigé vers le site Web où vous pouvez définir votre adresse de messagerie.

Vous êtes maintenant connecté à l’aide de vos informations d’identification Microsoft :

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Résolution des problèmes

* Si le fournisseur de comptes Microsoft vous redirige vers une page d’erreur de connexion, notez le titre d’erreur et la description paramètres de chaîne `#` de requête qui suivent directement le (mot-dièse) dans l’URI.

  Bien que le message d’erreur semble indiquer un problème avec l’authentification Microsoft, la cause la plus courante est que l’URI de votre application ne correspond à aucun des **URI de redirection** spécifiés pour la plateforme **Web** .
* Si l’identité n’est pas `services.AddIdentity` configurée en appelant dans `ConfigureServices`, toute tentative *d’authentification entraînera l’exception ArgumentException : L’option « SignInScheme » doit être fournie*. Le modèle de projet utilisé dans cet exemple permet de s’assurer que cette opération est effectuée.
* Si la base de données de site n’a pas été créé en appliquant la migration initiale, vous obtiendrez *une opération de base de données a échoué lors du traitement de la demande* erreur. Appuyez sur **appliquer les Migrations** pour créer la base de données et actualiser pour passer à l’erreur.

## <a name="next-steps"></a>Étapes suivantes

* Cet article vous a montré comment vous pouvez vous authentifier auprès de Microsoft. Vous pouvez suivre une approche similaire pour s’authentifier auprès d’autres fournisseurs répertoriés sur le [page précédente](xref:security/authentication/social/index).

* Une fois que vous publiez votre site Web dans l’application Web Azure, créez un nouveau secret client dans le portail des développeurs Microsoft.

* Définir le `Authentication:Microsoft:ClientId` et `Authentication:Microsoft:ClientSecret` en tant que paramètres d’application dans le portail Azure. Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.