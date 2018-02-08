---
title: "Stockage sécurisé des secrets d’application pendant le développement d'application ASP.NET Core"
author: rick-anderson
description: "Montre comment stocker des clés secrètes en toute sécurité dans le développement d'une application ASP.NET Core"
manager: wpickett
ms.author: riande
ms.date: 09/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: 337782a0530a37916b04aa562174b5921ddbc46b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="safe-storage-of-app-secrets-during-development-in-aspnet-core"></a>Stockage sécurisé des secrets d’application dans le développement d'application ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Michel Roth](https://github.com/danroth27), et [Scott Addie](https://scottaddie.com) 

Ce document montre comment vous pouvez utiliser l’outil Secret Manager dans le développement et comment conserver les clés secrètes en dehors de votre code. Le point le plus important est de vous ne devez jamais stocker les mots de passe ou d’autres données sensibles dans le code source, et vous ne devez pas utiliser les clés secrètes de production en mode de développement et de test. Vous pouvez utiliser à la place la [configuration du système](xref:fundamentals/configuration/index)  et lire ces valeurs à partir de variables d’environnement ou à partir de valeurs stockées à l’aide de l'outil Secret Manager. L’outil Secret Manager empêche les données sensibles d'être archivées dans le contrôle de code source. La [configuration du système](xref:fundamentals/configuration/index) peut lire les clés secrètes stockées avec l’outil Secret Manager décrit dans cet article.

L’outil Secret Manager est utilisée uniquement dans le développement. Vous pouvez protéger des secrets de test et de production Azure avec le fournisseur de configuration [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/). Consultez [fournisseur de configuration d’Azure Key Vault](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) pour plus d’informations.

## <a name="environment-variables"></a>Variables d’environnement

Pour éviter de stocker des secrets de l’application dans le code ou dans les fichiers de configuration local, vous pouvez stockez des secrets dans des variables d’environnement. Vous pouvez installer la [configuration du framework](xref:fundamentals/configuration/index) pour lire les valeurs de variables d’environnement en utilisant la méthode d'extension `AddEnvironmentVariables` dans la configuration. Vous pouvez ensuite utiliser des variables d’environnement pour remplacer des valeurs de configuration pour toutes les sources de configuration spécifié précédemment.

Par exemple, si vous créez une application web ASP.NET Core avec les comptes d’utilisateur individuels, ça ajoutera une chaîne de connexion par défaut pour le le fichier de configuration *appsettings.json* du projet avec la clé `DefaultConnection`. La chaîne de connexion par défaut est configurée pour utiliser la base de données locale, qui s’exécute en mode utilisateur et ne nécessite pas de mot de passe. Lorsque vous déployez votre application sur un serveur de test ou de production, vous pouvez remplacer la valeur de la clé `DefaultConnection` avec un paramètre de variable d’environnement qui contient la chaîne de connexion (avec éventuellement des informations d’identification sensibles) pour une base de données de test ou de production.

>[!WARNING]
> Les variables d’environnement sont généralement stockées en texte brut et ne sont pas chiffrées, donc si l’ordinateur ou le processus est compromis, et les variables d’environnement deviennent accessibles par des tiers non approuvés, des mesures supplémentaires pour empêcher la divulgation de secrets de l’utilisateur peuvent toujours être nécessaire.

## <a name="secret-manager"></a>L'outil Secret Manager (Gestionnaire de clé secrète)

L’outil principal Secret Manager stocke des données sensibles pour les travaux de développement en dehors de l’arborescence de votre projet. L’outil Secret Manager est un outil de projet qui peut être utilisé pour stocker des secrets pour un projet [.NET Core](https://www.microsoft.com/net/core) pendant le développement. Avec l’outil Secret Manager, vous pouvez associer des secrets de l’application à un projet spécifique et les partager entre plusieurs projets.

>[!WARNING]
> L'outil Secret Manager ne chiffre pas les clés secrètes stockées et donc ne doit pas être traité comme un magasin approuvé. Il est util uniquement à des fins de développement. Les clés et valeurs sont stockées dans un fichier de configuration JSON dans le répertoire de profil utilisateur.

## <a name="installing-the-secret-manager-tool"></a>Installation de l’outil Secret Manager

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Cliquez sur le projet dans l’Explorateur de solutions, puis sélectionnez dans le menu contextuel **modifier \<project_name\>.csproj** . Ajoutez la ligne en surbrillance à le *.csproj* de fichiers et enregistrer pour restaurer le package NuGet associé :

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

Cliquez à nouveau sur le projet dans l’Explorateur de solutions, puis sélectionnez **gérer les Secrets utilisateur** dans le menu contextuel. Cette opération ajoute un nouveau `UserSecretsId` nœud au sein d’un `PropertyGroup` du fichier *.csproj*, comme illustré dans l’exemple suivant :

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

L’enregistrement du fichier *.csproj* modifié ouvre également un fichier `secrets.json` dans l’éditeur de texte.
Remplacez le contenu du fichier `secrets.json` avec le code suivant :

```json
{
    "MySecret": "ValueOfMySecret"
}
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Ajouter la référence à `Microsoft.Extensions.SecretManager.Tools` dans le fichier *.csproj* et exécutez la commande `dotnet restore`. Vous pouvez utiliser les mêmes étapes pour installer Secret Manager à l’aide de la ligne de commande.

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

Pour tester et obtenir l'aide de l’outil Secret Manager éxécuter la commande suivante :

```console
dotnet user-secrets -h
```

L’outil Secret Manager affiche l’utilisation, options et l’aide de la commande.

> [!NOTE]
> Vous devez être dans le même répertoire que le fichier *.csproj* pour exécuter les outils définis dans le fichier *.csproj* du nœuds `DotNetCliToolReference`.

L’outil Secret Manager opère sur les paramètres de configuration de projet spécifiques qui sont stockées dans votre profil utilisateur. Pour utiliser les secrets de l’utilisateur, le projet doit spécifier la valeur du `UserSecretsId` dans son fichier *.csproj*. La valeur de `UserSecretsId` est arbitraire, mais est généralement unique au projet. Les développeurs génèrent généralement un GUID pour le `UserSecretsId`.

Ajouter un `UserSecretsId` pour votre projet dans le fichier *.csproj*:

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

Utilisez l’outil Secret Manager pour définir une clé secrète. Par exemple, dans une fenêtre de commande à partir du répertoire de projet, exécuter la commande suivante :

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

Vous pouvez accédez aux secrets stockés dans Secret Manager via le système de configuration. Ajouter le package `Microsoft.Extensions.Configuration.UserSecrets` et exécuter la commande `dotnet restore`.

Ajouter la source secret de configuration utilisateur pour la méthode `Startup`:

[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]

Vous pouvez accéder à des secrets de l’utilisateur via l’API de configuration :

[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]

## <a name="how-the-secret-manager-tool-works"></a>Fonctionnement de l’outil Secret Manager

L’outil Secret Manager élimine les détails d’implémentation, tels qu’où et comment les valeurs sont stockées. Vous pouvez utiliser l’outil sans connaître ces détails d’implémentation. Dans la version actuelle, les valeurs sont stockées dans un fichier [JSON](http://json.org/) de configuration dans le répertoire de profil utilisateur :

* Windows :`%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`

* Linux :`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`

* Mac :`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`

La valeur de `userSecretsId` provient de la valeur spécifiée dans *.csproj* fichier.

Vous ne devez pas écrire du code qui dépend de l’emplacement ou le format des données enregistrées avec l’outil Secret Manager, car ces détails d’implémentation peuvent changer. Par exemple, les valeurs de clé secrètes sont actuellement *pas* chiffrés aujourd'hui, mais le seront peut être un jour.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Configuration](xref:fundamentals/configuration/index)
