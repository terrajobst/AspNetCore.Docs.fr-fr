---
title: "Structure de l'arborescence physique des applications ASP.NET Core"
author: guardrex
description: "Description de l'arborescence physique des applications ASP.NET Core publiées."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/directory-structure
ms.openlocfilehash: 55e1e0dac32609446243098dbb4a4373f06b4212
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="directory-structure-of-published-aspnet-core-apps"></a>Description de l'arborescence physique des applications ASP.NET Core publiées.

Par [Luke Latham](https://github.com/guardrex)

Dans ASP.NET Core, le répertoire de l’application, *publish*, est constitué des fichiers de l'application, les fichiers de configuration, les ressources statiques, les packages, et l’environnement d'exécution (pour les applications autonomes).

| Type d’application                       | Structure de l'arborescence |
| ------------------------------ | ------------------- |
| Déploiement dépendant du framework | <ul><li>publish\*<ul><li>logs\* (si inclus dans publishOptions)</li><li>Refs\*</li><li>runtimes\*</li><li>Views\* (si inclus dans publishOptions)</li><li>wwwroot\* (si inclus dans publishOptions)</li><li>fichiers .dll</li><li>myapp.deps.json</li><li>myapp.dll</li><li>myapp.pdb</li><li>MyApp. PrecompiledViews.dll (si la précompilation des vues Razor)</li><li>MyApp. PrecompiledViews.pdb (si on précompile les vues Razor)</li><li>myapp.runtimeconfig.json</li><li>Web.config (si inclus dans publishOptions)</li></ul></li></ul> |
| Déploiement autonome      | <ul><li>publish\*<ul><li>logs\* (si est inclus dans publishOptions)</li><li>Refs\*</li><li>Views\* (si inclus dans publishOptions)</li><li>wwwroot\* (si inclus dans publishOptions)</li><li>fichiers .dll</li><li>myapp.deps.json</li><li>myapp.exe</li><li>myapp.pdb</li><li>MyApp. PrecompiledViews.dll (si on précompile les vues Razor)</li><li>MyApp. PrecompiledViews.pdb (si on précompile les vues Razor)</li><li>myapp.runtimeconfig.json</li><li>Web.config (si inclus dans publishOptions)</li></ul></li></ul> |
\*Indique un répertoire

Le contenu de répertoire *publish* représente le *chemin d’accès racine du contenu*, également appelé le *chemin de base d’application*, du déploiement. Quelque soit le nom donné au répertoire *publish* dans le déploiement, son emplacement sert de chemin d’accès physique à  l’application hébergée. Le répertoire *wwwroot*, si présent, contient uniquement les éléments statiques. Le répertoire *logs* peut être inclus dans le déploiement en le créant dans le projet et en ajoutant l'élément `<Target>` décrit ci-dessous pour votre fichier *.csproj* ou en créant physiquement le répertoire sur le serveur.

```xml
<Target Name="CreateLogsFolder" AfterTargets="Publish">
  <MakeDir Directories="$(PublishDir)Logs" 
           Condition="!Exists('$(PublishDir)Logs')" />
  <WriteLinesToFile File="$(PublishDir)Logs\.log" 
                    Lines="Generated file" 
                    Overwrite="True" 
                    Condition="!Exists('$(PublishDir)Logs\.log')" />
</Target>
```

L'élément `<MakeDir>` crée un dossier *logs* vide dans le projet publié. L’élément utilise la propriété `PublishDir` pour déterminer l’emplacement cible pour la création du dossier. Plusieurs méthodes de déploiement, telles que Web Deploy, ignorent les dossiers vides pendant le déploiement. L'élément `<WriteLinesToFile>` génère un fichier dans le dossier *logs*, ce qui garantit le déploiement du dossier sur le serveur. Notez que la création du dossier peut quand même échouer si le processus de travail n’a pas accès en écriture au  dossier cible.

Le répertoire de déploiement requiert des autorisations de lecture et d’exécution, tandis que la dossier *logs* nécessite des autorisations de lecture/écriture. Les répertoires complémentaires où seront écrit des ressources nécessitent des autorisations de lecture/écriture.
