---
title: Configuration de la connexion externe à un compte Microsoft avec ASP.NET Core
author: rick-anderson
description: Cet exemple illustre l’intégration de compte Microsoft l’authentification utilisateur dans une application ASP.NET Core existante.
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2020
monikerRange: '>= aspnetcore-3.0'
uid: security/authentication/microsoft-logins
ms.openlocfilehash: bd75efb1d7ce08538d1a67be74d2f40f3964614f
ms.sourcegitcommit: 9b6e7f421c243963d5e419bdcfc5c4bde71499aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2020
ms.locfileid: "79989762"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>Configuration de la connexion externe à un compte Microsoft avec ASP.NET Core

Par [Valeriy Novytskyy](https://github.com/01binary) et [Rick Anderson](https://twitter.com/RickAndMSFT)

Cet exemple montre comment permettre aux utilisateurs de se connecter avec leur compte Microsoft à l’aide du projet ASP.NET Core 3,0 créé sur la [page précédente](xref:security/authentication/social/index).

## <a name="create-the-app-in-microsoft-developer-portal"></a>Créer l’application dans le portail des développeurs Microsoft

* Ajoutez le package NuGet [Microsoft. AspNetCore. Authentication. MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount/) au projet.
* Accédez à la page de [inscriptions d’applications portail Azure](https://go.microsoft.com/fwlink/?linkid=2083908) et créez ou connectez-vous à une compte Microsoft :

Si vous n’avez pas de compte Microsoft, sélectionnez en **créer un**. Une fois connecté, vous êtes redirigé vers la page **inscriptions d’applications** :

* Sélectionnez **Nouvelle inscription**.
* Saisissez un **Nom**.
* Sélectionnez une option pour les **types de comptes pris en charge**.  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts -->
* Sous **URI de redirection**, entrez votre URL de développement avec `/signin-microsoft` ajoutée. Par exemple : `https://localhost:5001/signin-microsoft`. Le schéma d’authentification Microsoft configuré plus tard dans cet exemple gère automatiquement les demandes à `/signin-microsoft` itinéraire pour implémenter le Flow OAuth.
* Sélectionnez **Inscrire**.

### <a name="create-client-secret"></a>Créer un secret client

* Dans le volet gauche, sélectionnez **Certificats et secrets**.
* Sous **secrets client**, sélectionnez **nouvelle clé secrète client** .

  * Ajoutez une description pour la clé secrète client.
  * Sélectionnez le bouton **Ajouter**.

* Sous **secrets clients**, copiez la valeur de la clé secrète client.

> [!NOTE]
> Le segment d’URI `/signin-microsoft` est défini comme rappel par défaut du fournisseur d’authentification Microsoft. Vous pouvez modifier l’URI de rappel par défaut lors de la configuration de l’intergiciel (middleware) d’authentification Microsoft via la propriété héritée [RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath) de la classe [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions) .

## <a name="store-the-microsoft-client-id-and-secret"></a>Stocker l’ID client et le secret Microsoft

Stockez les paramètres sensibles tels que l’ID client Microsoft et les valeurs secrètes avec le [Gestionnaire de secret](xref:security/app-secrets). Pour cet exemple, procédez comme suit :

1. Initialisez le projet pour le stockage secret conformément aux instructions de la procédure [activer le stockage secret](xref:security/app-secrets#enable-secret-storage).
1. Stockez les paramètres sensibles dans le magasin de secret local avec les clés secrètes `Authentication:Microsoft:ClientId` et `Authentication:Microsoft:ClientSecret`:

    ```dotnetcli
    dotnet user-secrets set "Authentication:Microsoft:ClientId" "<client-id>"
    dotnet user-secrets set "Authentication:Microsoft:ClientSecret" "<client-secret>"
    ```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a>Configurer l’authentification de compte Microsoft

Ajoutez le service de compte Microsoft au `Startup.ConfigureServices`:

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupMS3x.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

[!INCLUDE[](includes/chain-auth-providers.md)]

Pour plus d’informations sur les options de configuration prises en charge par l’authentification de compte Microsoft, consultez la référence de l’API [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) . Cela peut être utilisé pour demander différentes informations sur l’utilisateur.

## <a name="sign-in-with-microsoft-account"></a>Compte Se connecter avec Microsoft

Exécutez l’application, puis cliquez sur **se connecter**. Une option permettant de se connecter à Microsoft s’affiche. Lorsque vous cliquez sur Microsoft, vous êtes redirigé vers Microsoft pour l’authentification. Une fois que vous êtes connecté avec votre compte Microsoft, vous êtes invité à autoriser l’application à accéder à vos informations :

Appuyez sur **Oui** . vous êtes alors redirigé vers le site Web où vous pouvez définir votre adresse de messagerie.

Vous êtes maintenant connecté à l’aide de vos informations d’identification Microsoft :

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>Dépannage

* Si le fournisseur de comptes Microsoft vous redirige vers une page d’erreur de connexion, notez le titre et la description des paramètres de chaîne de requête en suivant directement le `#` (mot-dièse) dans l’URI.

  Bien que le message d’erreur semble indiquer un problème avec l’authentification Microsoft, la cause la plus courante est que l’URI de votre application ne correspond à aucun des **URI de redirection** spécifiés pour la plateforme **Web** .
* Si l’identité n’est pas configurée en appelant `services.AddIdentity` dans `ConfigureServices`, toute tentative d’authentification entraînera *l’exception ArgumentException : l’option « SignInScheme » doit être fournie*. Le modèle de projet utilisé dans cet exemple permet de s’assurer que cette opération est effectuée.
* Si la base de données de site n’a pas été créée en appliquant la migration initiale, vous obtiendrez *une opération de base de données qui a échoué lors du traitement de l’erreur de demande* . Appuyez sur **appliquer des migrations** pour créer la base de données, puis sur Actualiser pour poursuivre l’erreur.

## <a name="next-steps"></a>Étapes suivantes

* Cet article vous a montré comment vous pouvez vous authentifier auprès de Microsoft. Vous pouvez suivre une approche similaire pour vous authentifier auprès d’autres fournisseurs listés dans la [page précédente](xref:security/authentication/social/index).

* Une fois que vous publiez votre site Web dans l’application Web Azure, créez un nouveau secret client dans le portail des développeurs Microsoft.

* Définissez les `Authentication:Microsoft:ClientId` et `Authentication:Microsoft:ClientSecret` en tant que paramètres d’application dans le Portail Azure. Le système de configuration est conçu pour lire les clés à partir de variables d’environnement.
