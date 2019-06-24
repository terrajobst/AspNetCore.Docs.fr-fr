---
title: Programme d’installation de Microsoft Account connexion externe avec ASP.NET Core
author: rick-anderson
description: Cet exemple montre l’intégration de l’authentification d’utilisateur de compte Microsoft à une application ASP.NET Core existante.
ms.author: riande
ms.custom: mvc
ms.date: 5/11/2019
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 16ec2d5f2bccc59958b884869ef42af9cfa13df0
ms.sourcegitcommit: 06a455d63ff7d6b571ca832e8117f4ac9d646baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67316590"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>Programme d’installation de Microsoft Account connexion externe avec ASP.NET Core

Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)

Cet exemple vous montre comment permettre aux utilisateurs de se connecter avec leur compte Microsoft à l’aide du projet ASP.NET Core 2.2 créé sur le [page précédente](xref:security/authentication/social/index).

## <a name="create-the-app-in-microsoft-developer-portal"></a>Créer l’application dans le portail des développeurs de Microsoft

* Accédez à la [portail Azure - inscriptions](https://go.microsoft.com/fwlink/?linkid=2083908) page et de créer ou de vous connecter à un compte Microsoft :

Si vous n’avez pas un compte Microsoft, sélectionnez **créez-le**. Après vous être connecté, vous êtes redirigé vers la **inscriptions** page :

* Sélectionnez **nouvelle inscription**
* Entrez un **nom**.
* Sélectionnez une option pour **pris en charge les types de comptes**.  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* Sous **URI de redirection**, entrez l’URL de votre développement avec `/signin-microsoft` ajouté. Par exemple, `https://localhost:44389/signin-microsoft`. Le schéma d’authentification Microsoft configuré plus loin dans cet exemple gère automatiquement les demandes à `/signin-microsoft` itinéraire pour implémenter le flux OAuth.
* Sélectionnez **inscrire**

### <a name="create-client-secret"></a>Créer la clé secrète client

* Dans le volet gauche, sélectionnez **certificats et clés secrètes**.
* Sous **clés secrètes de Client**, sélectionnez **nouvelle clé secrète client**

  * Ajoutez une description pour la clé secrète client.
  * Sélectionnez le **ajouter** bouton.

* Sous **clés secrètes de Client**, copiez la valeur de la clé secrète client.

> [!NOTE]
> Le segment d’URI `/signin-microsoft` est défini en tant que le rappel par défaut du fournisseur d’authentification de Microsoft. Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel d’authentification Microsoft via le hérité [RemoteAuthenticationOptions.CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) propriété de la [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) classe.

## <a name="store-the-microsoft-client-id-and-client-secret"></a>Store le secret de client et ID de client Microsoft

Exécutez les commandes suivantes pour stocker en toute sécurité `ClientId` et `ClientSecret` à l’aide de [Secret Manager](xref:security/app-secrets):

```console
dotnet user-secrets set Authentication:Microsoft:ClientId <Client-Id>
dotnet user-secrets set Authentication:Microsoft:ClientSecret <Client-Secret>
```

Lier des paramètres sensibles telles que Microsoft `ClientId` et `ClientSecret` à votre configuration d’application en utilisant le [Secret Manager](xref:security/app-secrets). Dans le cadre de cet exemple, nommez les jetons `Authentication:Microsoft:ClientId` et `Authentication:Microsoft:ClientSecret`.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a>Configurer l’authentification de compte Microsoft

Ajouter le service Microsoft Account `Startup.ConfigureServices`:

[!code-csharp[](~/security/authentication/social/social-code/StartupMS.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Consultez le [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) référence des API pour plus d’informations sur les options de configuration prises en charge par l’authentification de Microsoft Account. Cela peut être utilisé pour demander différentes informations sur l’utilisateur.

## <a name="sign-in-with-microsoft-account"></a>Connectez-vous avec un compte Microsoft

Exécuter l’et cliquez sur **connectez-vous**. Une option permettant de se connecter avec Microsoft s’affiche. Lorsque vous cliquez sur Microsoft, vous êtes redirigé vers Microsoft pour l’authentification. Après vous être connecté avec votre Account Microsoft (si pas déjà connecté), vous devez permettre à l’application d’accéder à vos informations :

Appuyez sur **Oui** et vous êtes redirigé vers le site web où vous pouvez définir votre adresse de messagerie.

Vous êtes maintenant connecté à l’aide de vos informations d’identification Microsoft :

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Résolution des problèmes

* Si le fournisseur Microsoft Account vous redirige vers une page d’erreur de connexion, notez l’erreur title et description paramètres chaîne de requête directement après le `#` (hashtag) dans l’Uri.

  Bien que le message d’erreur semble indiquer un problème avec l’authentification Microsoft, la cause la plus courante est votre application Uri ne pas corresponde à l’un de le **URI de redirection** spécifié pour le **Web** plateforme .
* Si l’identité n’est pas configurée en appelant `services.AddIdentity` dans `ConfigureServices`, toute tentative authentifier entraîne *ArgumentException : L’option 'SignInScheme' doit être fournie*. Le modèle de projet utilisé dans cet exemple permet de s’assurer que cela est fait.
* Si la base de données de site n’a pas été créé en appliquant la migration initiale, vous obtiendrez *une opération de base de données a échoué lors du traitement de la demande* erreur. Appuyez sur **appliquer les Migrations** pour créer la base de données et actualiser pour passer à l’erreur.

## <a name="next-steps"></a>Étapes suivantes

* Cet article vous a montré comment vous pouvez vous authentifier auprès de Microsoft. Vous pouvez suivre une approche similaire pour s’authentifier auprès d’autres fournisseurs répertoriés sur le [page précédente](xref:security/authentication/social/index).

* Une fois que vous publiez votre site web à l’application web Azure, créez un nouveau client secrets dans le portail des développeurs Microsoft.

* Définir le `Authentication:Microsoft:ClientId` et `Authentication:Microsoft:ClientSecret` en tant que paramètres d’application dans le portail Azure. Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.