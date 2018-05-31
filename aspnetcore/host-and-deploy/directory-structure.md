---
title: Structure de répertoires ASP.NET Core
author: guardrex
description: Découvrez la structure de répertoires des applications ASP.NET Core publiées.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/directory-structure
ms.openlocfilehash: a5cc1f23d624643facddc9e2006fb246e5ae66dc
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33838434"
---
# <a name="aspnet-core-directory-structure"></a>Structure de répertoires ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

Dans ASP.NET Core, le répertoire des applications publiées, *publish*, se compose des fichiers d’application, des fichiers de configuration, des ressources statiques, des packages et du runtime (pour les [déploiements autonomes](/dotnet/core/deploying/#self-contained-deployments-scd)).


| Type d’application | Structure de répertoires |
| -------- | ------------------- |
| [Déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li>publish&dagger;<ul><li>logs&dagger; (facultatif, sauf si nécessaire pour recevoir des journaux stdout)</li><li>Views&dagger; (applications MVC, si les vues ne sont pas précompilées)</li><li>Pages&dagger; (applications de pages Razor ou MVC, si les pages ne sont pas précompilées)</li><li>wwwroot&dagger;</li><li>Fichiers *\.dll</li><li>\<assembly-name>.deps.json</li><li>\<assembly-name>.dll</li><li>\<assembly-name>.pdb</li><li>\<assembly-name>.PrecompiledViews.dll</li><li>\<assembly-name>.PrecompiledViews.pdb</li><li>\<assembly-name>.runtimeconfig.json</li><li>web.config (déploiements IIS)</li></ul></li></ul> |
| [Déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>publish&dagger;<ul><li>logs&dagger; (facultatif, sauf si nécessaire pour recevoir des journaux stdout)</li><li>refs&dagger;</li><li>Views&dagger; (applications MVC, si les vues ne sont pas précompilées)</li><li>Pages&dagger; (applications de pages Razor ou MVC, si les pages ne sont pas précompilées)</li><li>wwwroot&dagger;</li><li>Fichiers \*.dll</li><li>\<assembly-name>.deps.json</li><li>\<assembly-name>.exe</li><li>\<assembly-name>.pdb</li><li>\<assembly-name>.PrecompiledViews.dll</li><li>\<assembly-name>.PrecompiledViews.pdb</li><li>\<assembly-name>.runtimeconfig.json</li><li>web.config (déploiements IIS)</li></ul></li></ul> |

&dagger;Indique un répertoire

Le répertoire *publish* représente le *chemin racine du contenu*, également appelé *chemin de base de l’application*, du déploiement. Quel que soit le nom donné au répertoire *publish* de l’application déployée sur le serveur, son emplacement sert de chemin physique, sur le serveur, de l’application hébergée.

Le répertoire *wwwroot*, s’il existe, contient uniquement des ressources statiques.

Vous pouvez créer le répertoire *logs* stdout pour le déploiement à l’aide d’une des deux méthodes suivantes :

* Ajoutez l’élément `<Target>` suivant au fichier projet :

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

   L’élément `<MakeDir>` crée un dossier *Logs* vide dans la sortie publiée. L’élément utilise la propriété `PublishDir` pour déterminer l’emplacement cible en vue de la création du dossier. Plusieurs méthodes de déploiement, telles que Web Deploy, ignorent les dossiers vides pendant le déploiement. L’élément `<WriteLinesToFile>` génère un fichier dans le dossier *Logs*, ce qui garantit le déploiement du dossier sur le serveur. Notez que la création du dossier peut échouer si le processus de travail n’a pas accès en écriture au dossier cible.

* Créez physiquement le répertoire *Logs* sur le serveur dans le déploiement.

Le répertoire de déploiement requiert des autorisations de lecture et d’exécution. Le répertoire *Logs* requiert des autorisations de lecture et d’écriture. D’autres répertoires où des fichiers sont écrits nécessitent des autorisations de lecture et d’écriture.
