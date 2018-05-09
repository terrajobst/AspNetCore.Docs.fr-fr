---
title: Structure de l'arborescence physique des applications ASP.NET Core
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
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="aspnet-core-directory-structure"></a>Structure de l'arborescence physique des applications ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

Dans ASP.NET Core, le répertoire de l’application publiée, *publier*, se compose de fichiers d’application, les fichiers de configuration, les ressources statiques, packages et l’exécution (pour [déploiements autonomes](/dotnet/core/deploying/#self-contained-deployments-scd)).


| Type d’application | Structure de l'arborescence |
| -------- | ------------------- |
| [Déploiement d’infrastructure dépendant](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li>Publier&dagger;<ul><li>journaux&dagger; (facultatif, sauf si nécessaire pour recevoir les journaux de stdout)</li><li>Vues&dagger; (applications MVC ; si les vues ne sont pas précompilés)</li><li>Pages&dagger; (MVC ou des Pages Razor applications ; si les pages ne sont pas précompilées)</li><li>wwwroot&dagger;</li><li>*\.fichiers DLL</li><li>\<nom de l’assembly >. deps.json</li><li>\<nom de l’assembly > .dll</li><li>\<nom de l’assembly > .pdb</li><li>\<nom de l’assembly >. PrecompiledViews.dll</li><li>\<nom de l’assembly >. PrecompiledViews.pdb</li><li>\<nom de l’assembly >. runtimeconfig.json</li><li>Web.config (déploiements IIS)</li></ul></li></ul> |
| [Déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>Publier&dagger;<ul><li>journaux&dagger; (facultatif, sauf si nécessaire pour recevoir les journaux de stdout)</li><li>Refs&dagger;</li><li>Vues&dagger; (applications MVC ; si les vues ne sont pas précompilés)</li><li>Pages&dagger; (MVC ou des Pages Razor applications ; si les pages ne sont pas précompilées)</li><li>wwwroot&dagger;</li><li>\*fichiers .dll</li><li>\<nom de l’assembly >. deps.json</li><li>\<nom de l’assembly > .exe</li><li>\<nom de l’assembly > .pdb</li><li>\<nom de l’assembly >. PrecompiledViews.dll</li><li>\<nom de l’assembly >. PrecompiledViews.pdb</li><li>\<nom de l’assembly >. runtimeconfig.json</li><li>Web.config (déploiements IIS)</li></ul></li></ul> |

&dagger;Indique un répertoire

Le *publier* répertoire représente le *chemin d’accès racine du contenu*, également appelé le *chemin d’accès de base application*, du déploiement. Le nom est donné à la *publier* répertoire de l’application déployée sur le serveur, son emplacement sert de chemin d’accès physique à celle du serveur pour l’application hébergée.

Le *wwwroot* active, le cas échéant, contient uniquement des éléments statiques.

Stdout *journaux* répertoire peut être créé pour le déploiement à l’aide d’une des deux méthodes suivantes :

* Ajoutez le code suivant `<Target>` élément dans le fichier projet :

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

   Le `<MakeDir>` élément crée vide *journaux* dossier dans le projet publié. L’élément utilise le `PublishDir` propriété pour déterminer l’emplacement cible pour la création du dossier. Plusieurs méthodes de déploiement, telles que Web Deploy, ignorent les dossiers vides pendant le déploiement. Le `<WriteLinesToFile>` élément génère un fichier dans le *journaux* dossier, ce qui garantit le déploiement du dossier sur le serveur. Notez que la création du dossier peut-être encore échouer si le processus de travail n’a pas accès en écriture dans le dossier cible.

* Créer physiquement le *journaux* répertoire sur le serveur dans le déploiement.

Le répertoire de déploiement requiert des autorisations de lecture et d’exécution. Le *journaux* directory nécessite des autorisations de lecture/écriture. Autres répertoires où les fichiers sont écrits nécessitent des autorisations de lecture/écriture.
