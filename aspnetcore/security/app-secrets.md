---
title: "Stockage sécurisé des secrets d’application dans le développement d'application ASP.NET Core"
author: rick-anderson
description: "Montre comment stocker des clés secrètes en toute sécurité pendant le développement"
manager: wpickett
ms.author: riande
ms.date: 09/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: a23c9dc9ee1e20c0e0551a372e1cd706bb82070e
ms.sourcegitcommit: 6548a3dd0cd1e3e92ac2310dee757ddad9fd6456
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/16/2018
---
# <a name="safe-storage-of-app-secrets-during-development-in-aspnet-core"></a>Stockage sécurisé des secrets d’application dans le développement d'application ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Michel Roth](https://github.com/danroth27), et [Scott Addie](https://scottaddie.com) 

Ce document montre comment vous pouvez utiliser l’outil Secret Manager dans le développement et comment conserver les clés secrètes en dehors de votre code.  Le point le plus important est que vous ne devez jamais stocker les mots de passe ou d’autres données sensibles dans le code source, et vous ne devez pas utiliser les clés secrètes de production en mode de développement et de test. Vous pouvez utiliser à la place la [configuration du système](xref:fundamentals/configuration/index) et lire ces valeurs à partir de variables d’environnement ou à partir de valeurs stockées à l’aide de l'outil Secret Manager.  L’outil Secret Manager empêche les données sensibles d'être archivées dans le contrôle de code source. La [configuration du système](xref:fundamentals/configuration/index) peut lire les clés secrètes stockées avec l’outil Secret Manager décrit dans cet article.

L’outil Secret Manager est utilisé uniquement dans le développement. Vous pouvez protéger des secrets de test et de production Azure avec le fournisseur de configuration [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/). Consultez [fournisseur de configuration d’Azure Key Vault](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) pour plus d’informations.

## <a name="environment-variables"></a>Variables d’environnement

Vous pouvez ensuite utiliser des variables d’environnement pour remplacer des valeurs de configuration pour toutes les sources de configuration spécifiées précédemment. Vous pouvez installer la [configuration du framework](xref:fundamentals/configuration/index) pour lire les valeurs de variables d’environnement en utilisant la méthode d'extension `AddEnvironmentVariables` dans la configuration. Vous pouvez ensuite utiliser des variables d’environnement pour remplacer des valeurs de configuration pour toutes les sources de configuration spécifié précédemment.

Par exemple, si vous créez une application web ASP.NET Core avec les comptes d’utilisateur individuels, ça ajoutera une chaîne de connexion par défaut pour le fichier de configuration *appsettings.json* du projet avec la clé `DefaultConnection`. La chaîne de connexion par défaut est configurée pour utiliser la base de données locale, qui s’exécute en mode utilisateur et ne nécessite pas de mot de passe. Lorsque vous déployez votre application sur un serveur de test ou de production, vous pouvez remplacer la valeur de la clé `DefaultConnection` avec un paramètre de variable d’environnement qui contient la chaîne de connexion (avec éventuellement des informations d’identification sensibles) pour une base de données de test ou de production.

>[!WARNING]
> Les variables d’environnement sont généralement stockées en texte brut et ne sont pas chiffrées. Donc si l’ordinateur ou le processus est compromis, et les variables d’environnement deviennent accessibles par des tiers non approuvés. Des mesures supplémentaires pour empêcher la divulgation de secrets de l’utilisateur peuvent toujours être nécessaires.

## <a name="secret-manager"></a>L'outil Secret Manager (Gestionnaire de secrets)

L’outil principal Secret Manager stocke des données sensibles pour les travaux de développement en dehors de l’arborescence de votre projet. L’outil Secret Manager est un outil de projet qui peut être utilisé pour stocker des secrets pour un projet [.NET Core](https://www.microsoft.com/net/core) pendant le développement. Avec l’outil Secret Manager, vous pouvez associer des secrets de l’application à un projet spécifique et les partager entre plusieurs projets.

>[!WARNING]
> L'outil Secret Manager ne chiffre pas les clés secrètes stockées et donc ne doit pas être traité comme un magasin approuvé. Il est utile uniquement à des fins de développement. Les clés et valeurs sont stockées dans un fichier de configuration JSON dans le répertoire de profil utilisateur.

## <a name="installing-the-secret-manager-tool"></a>Installation de l’outil Secret Manager

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Cliquez avec le bouton droit sur le projet dans l’Explorateur de solutions, puis sélectionnez dans le menu contextuel **Modifier \<project_name\>.csproj** . Ajoutez la ligne en surbrillance dans le fichier *.csproj* et enregistrez pour restaurer le package NuGet associé :

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

Cliquez à nouveau sur le projet dans l’Explorateur de solutions, puis sélectionnez **Gérer les secrets utilisateur** dans le menu contextuel. Cette opération ajoute un nouveau nœud `UserSecretsId` au sein d’un `PropertyGroup` du fichier *.csproj*, comme illustré dans l’exemple suivant :

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

L’enregistrement du fichier *.csproj* modifié ouvre également un fichier `secrets.json` dans l’éditeur de texte. Remplacez le contenu du fichier `secrets.json` avec le code suivant :

```json
{
    "MySecret": "ValueOfMySecret"
}
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Ajoutez la référence à `Microsoft.Extensions.SecretManager.Tools` dans le fichier *.csproj* et exécutez la commande [dotnet restauration](/dotnet/core/tools/dotnet-restore). Vous pouvez utiliser les mêmes étapes pour installer Secret Manager à l’aide de la ligne de commande.

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

Pour tester et obtenir l'aide de l’outil Secret Manager exécutez la commande suivante :

```console
dotnet user-secrets -h
```

L’outil Secret Manager affiche l’utilisation, les options et l’aide de la commande.

> [!NOTE]
> Vous devez être dans le même répertoire que le fichier *.csproj* pour exécuter les outils définis dans le fichier *.csproj* du nœud `DotNetCliToolReference`.

L’outil Secret Manager opère sur les paramètres de configuration de projet spécifiques qui sont stockés dans votre profil utilisateur. Pour utiliser les secrets de l’utilisateur, le projet doit spécifier la valeur de `UserSecretsId` dans son fichier *.csproj*. La valeur de `UserSecretsId` est arbitraire, mais est généralement unique au projet. Les développeurs génèrent généralement un GUID pour le `UserSecretsId`.

Ajoutez un `UserSecretsId` pour votre projet dans le fichier *.csproj*:

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

Utilisez l’outil Secret Manager pour définir une clé secrète. Par exemple, dans une fenêtre de commande à partir du répertoire de projet, exécutez la commande suivante :

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

Vous pouvez exécuter l’outil Secret Manager à partir d’autres annuaires, mais vous devez utiliser l'option `--project` à passer dans le chemin d’accès au fichier *.csproj*:
 
```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

Vous pouvez également utiliser l’outil Secret Manager pour répertorier, supprimer et effacer les secrets de l’application.

-----

## <a name="accessing-user-secrets-via-configuration"></a>L’accès à des secrets de l’utilisateur via la configuration

Vous pouvez accéder aux secrets stockés dans Secret Manager via le système de configuration. Ajoutez le package `Microsoft.Extensions.Configuration.UserSecrets` et exécutez la commande [dotnet restore](/dotnet/core/tools/dotnet-restore).

Ajouter la source de configuration des secrets utilisateurs pour la méthode `Startup`:

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]

Vous pouvez accéder à des secrets de l’utilisateur via l’API de configuration :

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]

## <a name="how-the-secret-manager-tool-works"></a>Fonctionnement de l’outil Secret Manager

L’outil Secret Manager élimine les détails d’implémentation, tels qu’où et comment les valeurs sont stockées. Vous pouvez utiliser l’outil sans connaître ces détails d’implémentation. Dans la version actuelle, les valeurs sont stockées dans un fichier [JSON](http://json.org/) de configuration dans le répertoire de profil utilisateur :

* Windows : `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`

* Linux : `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`

* macOS : `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`

La valeur de `userSecretsId` provient de la valeur spécifiée dans *.csproj* fichier.

Vous ne devez pas écrire du code qui dépend de l’emplacement ou du format des données enregistrées avec l’outil Secret Manager, car ces détails d’implémentation peuvent changer. Par exemple, les valeurs de clé secrètes ne sont actuellement *pas* chiffrés, mais le seront peut être un jour.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Configuration](xref:fundamentals/configuration/index)
