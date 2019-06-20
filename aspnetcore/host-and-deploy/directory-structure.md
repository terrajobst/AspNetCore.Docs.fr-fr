---
title: Structure de répertoires ASP.NET Core
author: guardrex
description: Découvrez la structure de répertoires des applications ASP.NET Core publiées.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 06/17/2019
uid: host-and-deploy/directory-structure
ms.openlocfilehash: f1df047decc7a0a6b7dcee57a690c55eea428b05
ms.sourcegitcommit: 28a2874765cefe9eaa068dceb989a978ba2096aa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67166977"
---
# <a name="aspnet-core-directory-structure"></a><span data-ttu-id="5b6b8-103">Structure de répertoires ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5b6b8-103">ASP.NET Core directory structure</span></span>

<span data-ttu-id="5b6b8-104">Par [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5b6b8-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5b6b8-105">Le répertoire *publier* contient les ressources de l’application qui peuvent être déployées et produites par la commande [dotnet publish](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="5b6b8-105">The *publish* directory contains the app's deployable assets produced by the [dotnet publish](/dotnet/core/tools/dotnet-publish) command.</span></span> <span data-ttu-id="5b6b8-106">Le répertoire contient :</span><span class="sxs-lookup"><span data-stu-id="5b6b8-106">The directory contains:</span></span>

* <span data-ttu-id="5b6b8-107">Les fichiers de l’application</span><span class="sxs-lookup"><span data-stu-id="5b6b8-107">Application files</span></span>
* <span data-ttu-id="5b6b8-108">Les fichiers de configuration</span><span class="sxs-lookup"><span data-stu-id="5b6b8-108">Configuration files</span></span>
* <span data-ttu-id="5b6b8-109">Les ressources statiques</span><span class="sxs-lookup"><span data-stu-id="5b6b8-109">Static assets</span></span>
* <span data-ttu-id="5b6b8-110">Packages</span><span class="sxs-lookup"><span data-stu-id="5b6b8-110">Packages</span></span>
* <span data-ttu-id="5b6b8-111">Un runtime ([déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) uniquement)</span><span class="sxs-lookup"><span data-stu-id="5b6b8-111">A runtime ([self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) only)</span></span>

| <span data-ttu-id="5b6b8-112">Type d’application</span><span class="sxs-lookup"><span data-stu-id="5b6b8-112">App Type</span></span> | <span data-ttu-id="5b6b8-113">Structure de répertoires</span><span class="sxs-lookup"><span data-stu-id="5b6b8-113">Directory Structure</span></span> |
| -------- | ------------------- |
| [<span data-ttu-id="5b6b8-114">Déploiement dépendant du framework</span><span class="sxs-lookup"><span data-stu-id="5b6b8-114">Framework-dependent Deployment</span></span>](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li><span data-ttu-id="5b6b8-115">publish&dagger;</span><span class="sxs-lookup"><span data-stu-id="5b6b8-115">publish&dagger;</span></span><ul><li><span data-ttu-id="5b6b8-116">Views&dagger; (applications MVC, si les vues ne sont pas précompilées)</span><span class="sxs-lookup"><span data-stu-id="5b6b8-116">Views&dagger; (MVC apps; if views aren't precompiled)</span></span></li><li><span data-ttu-id="5b6b8-117">Pages&dagger; (applications de pages Razor ou MVC, si les pages ne sont pas précompilées)</span><span class="sxs-lookup"><span data-stu-id="5b6b8-117">Pages&dagger; (MVC or Razor Pages apps; if pages aren't precompiled)</span></span></li><li><span data-ttu-id="5b6b8-118">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="5b6b8-118">wwwroot&dagger;</span></span></li><li><span data-ttu-id="5b6b8-119">Fichiers \*\.dll</span><span class="sxs-lookup"><span data-stu-id="5b6b8-119">\*\.dll files</span></span></li><li><span data-ttu-id="5b6b8-120">{NOM de l’ASSEMBLY}.deps.json</span><span class="sxs-lookup"><span data-stu-id="5b6b8-120">{ASSEMBLY NAME}.deps.json</span></span></li><li><span data-ttu-id="5b6b8-121">{NOM de l’ASSEMBLY}.dll</span><span class="sxs-lookup"><span data-stu-id="5b6b8-121">{ASSEMBLY NAME}.dll</span></span></li><li><span data-ttu-id="5b6b8-122">{NOM de l’ASSEMBLY}.pdb</span><span class="sxs-lookup"><span data-stu-id="5b6b8-122">{ASSEMBLY NAME}.pdb</span></span></li><li><span data-ttu-id="5b6b8-123">{NOM de l’ASSEMBLY}.Views.dll</span><span class="sxs-lookup"><span data-stu-id="5b6b8-123">{ASSEMBLY NAME}.Views.dll</span></span></li><li><span data-ttu-id="5b6b8-124">{NOM de l’ASSEMBLY}.Views.pdb</span><span class="sxs-lookup"><span data-stu-id="5b6b8-124">{ASSEMBLY NAME}.Views.pdb</span></span></li><li><span data-ttu-id="5b6b8-125">{NOM de l’ASSEMBLY}.runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="5b6b8-125">{ASSEMBLY NAME}.runtimeconfig.json</span></span></li><li><span data-ttu-id="5b6b8-126">web.config (déploiements IIS)</span><span class="sxs-lookup"><span data-stu-id="5b6b8-126">web.config (IIS deployments)</span></span></li></ul></li></ul> |
| [<span data-ttu-id="5b6b8-127">Déploiement autonome</span><span class="sxs-lookup"><span data-stu-id="5b6b8-127">Self-contained Deployment</span></span>](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li><span data-ttu-id="5b6b8-128">publish&dagger;</span><span class="sxs-lookup"><span data-stu-id="5b6b8-128">publish&dagger;</span></span><ul><li><span data-ttu-id="5b6b8-129">Views&dagger; (applications MVC, si les vues ne sont pas précompilées)</span><span class="sxs-lookup"><span data-stu-id="5b6b8-129">Views&dagger; (MVC apps; if views aren't precompiled)</span></span></li><li><span data-ttu-id="5b6b8-130">Pages&dagger; (applications de pages Razor ou MVC, si les pages ne sont pas précompilées)</span><span class="sxs-lookup"><span data-stu-id="5b6b8-130">Pages&dagger; (MVC or Razor Pages apps; if pages aren't precompiled)</span></span></li><li><span data-ttu-id="5b6b8-131">wwwroot&dagger;</span><span class="sxs-lookup"><span data-stu-id="5b6b8-131">wwwroot&dagger;</span></span></li><li><span data-ttu-id="5b6b8-132">Fichiers \*.dll</span><span class="sxs-lookup"><span data-stu-id="5b6b8-132">\*.dll files</span></span></li><li><span data-ttu-id="5b6b8-133">{NOM de l’ASSEMBLY}.deps.json</span><span class="sxs-lookup"><span data-stu-id="5b6b8-133">{ASSEMBLY NAME}.deps.json</span></span></li><li><span data-ttu-id="5b6b8-134">{NOM de l’ASSEMBLY}.dll</span><span class="sxs-lookup"><span data-stu-id="5b6b8-134">{ASSEMBLY NAME}.dll</span></span></li><li><span data-ttu-id="5b6b8-135">{NOM de l’ASSEMBLY}.exe</span><span class="sxs-lookup"><span data-stu-id="5b6b8-135">{ASSEMBLY NAME}.exe</span></span></li><li><span data-ttu-id="5b6b8-136">{NOM de l’ASSEMBLY}.pdb</span><span class="sxs-lookup"><span data-stu-id="5b6b8-136">{ASSEMBLY NAME}.pdb</span></span></li><li><span data-ttu-id="5b6b8-137">{NOM de l’ASSEMBLY}.Views.dll</span><span class="sxs-lookup"><span data-stu-id="5b6b8-137">{ASSEMBLY NAME}.Views.dll</span></span></li><li><span data-ttu-id="5b6b8-138">{NOM de l’ASSEMBLY}.Views.pdb</span><span class="sxs-lookup"><span data-stu-id="5b6b8-138">{ASSEMBLY NAME}.Views.pdb</span></span></li><li><span data-ttu-id="5b6b8-139">{NOM de l’ASSEMBLY}.runtimeconfig.json</span><span class="sxs-lookup"><span data-stu-id="5b6b8-139">{ASSEMBLY NAME}.runtimeconfig.json</span></span></li><li><span data-ttu-id="5b6b8-140">web.config (déploiements IIS)</span><span class="sxs-lookup"><span data-stu-id="5b6b8-140">web.config (IIS deployments)</span></span></li></ul></li></ul> |

<span data-ttu-id="5b6b8-141">&dagger;Indique un répertoire</span><span class="sxs-lookup"><span data-stu-id="5b6b8-141">&dagger;Indicates a directory</span></span>

<span data-ttu-id="5b6b8-142">Le répertoire *publish* représente le *chemin racine du contenu*, également appelé *chemin de base de l’application*, du déploiement.</span><span class="sxs-lookup"><span data-stu-id="5b6b8-142">The *publish* directory represents the *content root path*, also called the *application base path*, of the deployment.</span></span> <span data-ttu-id="5b6b8-143">Quel que soit le nom donné au répertoire *publish* de l’application déployée sur le serveur, son emplacement sert de chemin physique, sur le serveur, de l’application hébergée.</span><span class="sxs-lookup"><span data-stu-id="5b6b8-143">Whatever name is given to the *publish* directory of the deployed app on the server, its location serves as the server's physical path to the hosted app.</span></span>

<span data-ttu-id="5b6b8-144">Le répertoire *wwwroot*, s’il existe, contient uniquement des ressources statiques.</span><span class="sxs-lookup"><span data-stu-id="5b6b8-144">The *wwwroot* directory, if present, only contains static assets.</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5b6b8-145">La création d’un dossier *Logs* est utile à la [journalisation de débogage améliorée du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span><span class="sxs-lookup"><span data-stu-id="5b6b8-145">Creating a *Logs* folder is useful for [ASP.NET Core Module enhanced debug logging](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs).</span></span> <span data-ttu-id="5b6b8-146">Les dossiers situés dans le chemin fourni pour la valeur `<handlerSetting>` ne sont pas créés automatiquement par le module. Ils doivent préexister dans le déploiement pour permettre au module d’écrire dans le journal de débogage.</span><span class="sxs-lookup"><span data-stu-id="5b6b8-146">Folders in the path provided to the `<handlerSetting>` value aren't created by the module automatically and should pre-exist in the deployment to allow the module to write the debug log.</span></span>

<span data-ttu-id="5b6b8-147">Vous pouvez créer le répertoire *Logs* pour le déploiement à l’aide de l’une des deux approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="5b6b8-147">A *Logs* directory can be created for the deployment using one of the following two approaches:</span></span>

* <span data-ttu-id="5b6b8-148">Ajoutez l’élément `<Target>` suivant au fichier projet :</span><span class="sxs-lookup"><span data-stu-id="5b6b8-148">Add the following `<Target>` element to the project file:</span></span>

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

   <span data-ttu-id="5b6b8-149">L’élément `<MakeDir>` crée un dossier *Logs* vide dans la sortie publiée.</span><span class="sxs-lookup"><span data-stu-id="5b6b8-149">The `<MakeDir>` element creates an empty *Logs* folder in the published output.</span></span> <span data-ttu-id="5b6b8-150">L’élément utilise la propriété `PublishDir` pour déterminer l’emplacement cible en vue de la création du dossier.</span><span class="sxs-lookup"><span data-stu-id="5b6b8-150">The element uses the `PublishDir` property to determine the target location for creating the folder.</span></span> <span data-ttu-id="5b6b8-151">Plusieurs méthodes de déploiement, telles que Web Deploy, ignorent les dossiers vides pendant le déploiement.</span><span class="sxs-lookup"><span data-stu-id="5b6b8-151">Several deployment methods, such as Web Deploy, skip empty folders during deployment.</span></span> <span data-ttu-id="5b6b8-152">L’élément `<WriteLinesToFile>` génère un fichier dans le dossier *Logs*, ce qui garantit le déploiement du dossier sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="5b6b8-152">The `<WriteLinesToFile>` element generates a file in the *Logs* folder, which guarantees deployment of the folder to the server.</span></span> <span data-ttu-id="5b6b8-153">La création d’un dossier à l’aide de cette approche échoue si le processus de travail n’a pas accès en écriture au dossier cible.</span><span class="sxs-lookup"><span data-stu-id="5b6b8-153">Folder creation using this approach fails if the worker process doesn't have write access to the target folder.</span></span>

* <span data-ttu-id="5b6b8-154">Créez physiquement le répertoire *Logs* sur le serveur dans le déploiement.</span><span class="sxs-lookup"><span data-stu-id="5b6b8-154">Physically create the *Logs* directory on the server in the deployment.</span></span>

<span data-ttu-id="5b6b8-155">Le répertoire de déploiement requiert des autorisations de lecture et d’exécution.</span><span class="sxs-lookup"><span data-stu-id="5b6b8-155">The deployment directory requires Read/Execute permissions.</span></span> <span data-ttu-id="5b6b8-156">Le répertoire *Logs* requiert des autorisations de lecture et d’écriture.</span><span class="sxs-lookup"><span data-stu-id="5b6b8-156">The *Logs* directory requires Read/Write permissions.</span></span> <span data-ttu-id="5b6b8-157">D’autres répertoires où des fichiers sont écrits nécessitent des autorisations de lecture et d’écriture.</span><span class="sxs-lookup"><span data-stu-id="5b6b8-157">Additional directories where files are written require Read/Write permissions.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="5b6b8-158">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="5b6b8-158">Additional resources</span></span>

* [<span data-ttu-id="5b6b8-159">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="5b6b8-159">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* [<span data-ttu-id="5b6b8-160">Déploiement d’applications .NET Core</span><span class="sxs-lookup"><span data-stu-id="5b6b8-160">.NET Core application deployment</span></span>](/dotnet/core/deploying/)
* [<span data-ttu-id="5b6b8-161">Frameworks cibles</span><span class="sxs-lookup"><span data-stu-id="5b6b8-161">Target frameworks</span></span>](/dotnet/standard/frameworks)
* [<span data-ttu-id="5b6b8-162">Catalogue RID .NET Core</span><span class="sxs-lookup"><span data-stu-id="5b6b8-162">.NET Core RID Catalog</span></span>](/dotnet/core/rid-catalog)
