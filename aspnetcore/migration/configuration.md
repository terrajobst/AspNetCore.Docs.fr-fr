---
title: Migrer la configuration d’ASP.NET Core
author: ardalis
description: Découvrez comment migrer la configuration à partir d’un projet ASP.NET MVC vers un projet ASP.NET Core MVC.
ms.author: riande
ms.date: 10/14/2016
uid: migration/configuration
ms.openlocfilehash: 5a1c4d0cbbdf74a00073c654e78a05f44948caae
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50205910"
---
# <a name="migrate-configuration-to-aspnet-core"></a>Migrer la configuration d’ASP.NET Core

Par [Steve Smith](https://ardalis.com/) et [Scott Addie](https://scottaddie.com)

Dans l’article précédent, nous avons commencé [migrer un projet ASP.NET MVC vers ASP.NET Core MVC](xref:migration/mvc). Dans cet article, nous migrer la configuration.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="setup-configuration"></a>Configuration de l’installation

ASP.NET Core n’utilise plus les fichiers *Global.asax* et *web.config* utilisées par les versions précédentes d’ASP.NET. Dans les versions antérieures d’ASP.NET, la logique de démarrage d’application a été placée dans une méthode `Application_StartUp` du fichier *Global.asax*. Ensuite, dans ASP.NET MVC, un fichier *Startup.cs* a été inclus dans la racine du projet ; et, il été appelée au démarrage de l’application. ASP.NET Core a arrêté complètement cette approche, en plaçant toute logique de démarrage dans le *Startup.cs* fichier.

Le fichier *web.config* a également été remplacé dans ASP.NET Core. La configuration proprement dite peut maintenant être configurée, dans le cadre de la procédure de démarrage d’application décrite dans le fichier *Startup.cs*. La configuration peut toujours utiliser des fichiers XML, mais en général, les projets ASP.NET Core place les valeurs de configuration dans un fichier au format JSON, tel que *appsettings.json*. Système de configuration d’ASP.NET Core également facilement accessible des variables d’environnement, qui peuvent fournir un [plus un emplacement sécurisé et robuste](xref:security/app-secrets) pour les valeurs spécifiques à l’environnement. Cela est particulièrement vrai pour les clés secrètes comme des chaînes de connexion et les clés API qui ne doivent pas être archivés dans le contrôle de code source. Consultez [Configuration](xref:fundamentals/configuration/index) pour en savoir plus sur la configuration dans ASP.NET Core.

Pour cet article, nous avons commencé avec le projet ASP.NET Core partiellement migré à partir de [l’article précédent](xref:migration/mvc). Pour installer la configuration, ajoutez le constructeur et la propriété suivante dans le fichier *Startup.cs* situé à la racine du projet :

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-16)]

Notez qu’à ce stade, le fichier *Startup.cs* fichier ne sera pas compilé, qu’il faut toujours ajouter les éléments suivants avec l'instruction `using`:

```csharp
using Microsoft.Extensions.Configuration;
```

Ajouter un fichier *appsettings.json* à la racine du projet à l’aide du modèle d’élément approprié :

![Ajouter AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a>Migrer les paramètres de configuration à partir de web.config

Notre projet ASP.NET MVC inclut  la chaîne de connexion de base de données requises dans le fichier *web.config*, dans l'élément `<connectionStrings>`. Dans notre projet ASP.NET Core, nous allons stocker ces informations dans le fichier *appsettings.json*. Ouvrez le fichier *appsettings.json*et notez qu’il contient déjà les éléments suivants :

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]

Dans la ligne en surbrillance mentionnée ci-dessus, remplacez le nom de la base de données à partir de **_CHANGE_ME** avec le nom de votre base de données.

## <a name="summary"></a>Récapitulatif

ASP.NET Core place toute la logique de démarrage de l’application dans un seul fichier dans lequel les services nécessaires et les dépendances peuvent être définis et configurés. Il remplace le fichier *web.config* avec une fonctionnalité de configuration souples qui peut tirer parti des divers formats de fichier, tels que JSON, ainsi que les variables d’environnement.
