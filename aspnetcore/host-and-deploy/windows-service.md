---
title: Héberger ASP.NET Core dans un service Windows
author: guardrex
description: Découvrez comment héberger une application ASP.NET Core dans un service Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/04/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 544eefa87898e82ec2bf8f9f61ce4e26dd554bb7
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068334"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="677f7-103">Héberger ASP.NET Core dans un service Windows</span><span class="sxs-lookup"><span data-stu-id="677f7-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="677f7-104">Par [Luke Latham](https://github.com/guardrex) et [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="677f7-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="677f7-105">Une application ASP.NET Core peut être hébergée sur Windows en tant que [service Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) sans utiliser IIS.</span><span class="sxs-lookup"><span data-stu-id="677f7-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="677f7-106">Lorsqu’elle est hébergée en tant que service Windows, l’application démarre automatiquement après le redémarrage.</span><span class="sxs-lookup"><span data-stu-id="677f7-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="677f7-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="677f7-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="677f7-108">Prérequis</span><span class="sxs-lookup"><span data-stu-id="677f7-108">Prerequisites</span></span>

* [<span data-ttu-id="677f7-109">PowerShell 6.2 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="677f7-109">PowerShell 6.2 or later</span></span>](https://github.com/PowerShell/PowerShell)

> [!NOTE]
> <span data-ttu-id="677f7-110">Pour une version du système d’exploitation Windows antérieure à la Mise à jour d’octobre 2018 de Windows 10 (version 1809/build 10.0.17763), le module [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts) doit être importé avec le module [WindowsCompatibility](https://github.com/PowerShell/WindowsCompatibility) pour permettre l’accès à la cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) utilisée dans la section [Créer un compte d’utilisateur](#create-a-user-account) :</span><span class="sxs-lookup"><span data-stu-id="677f7-110">For Windows OS earlier than the Windows 10 October 2018 Update (version 1809/build 10.0.17763), the [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts) module must be imported with the [WindowsCompatibility module](https://github.com/PowerShell/WindowsCompatibility) to gain access to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet used in the [Create a user account](#create-a-user-account) section:</span></span>
>
> ```powershell
> Install-Module WindowsCompatibility -Scope CurrentUser
> Import-WinModule Microsoft.PowerShell.LocalAccounts
> ```

## <a name="deployment-type"></a><span data-ttu-id="677f7-111">Type de déploiement</span><span class="sxs-lookup"><span data-stu-id="677f7-111">Deployment type</span></span>

<span data-ttu-id="677f7-112">Vous pouvez créer un déploiement Windows Service dépendant du framework ou autonome.</span><span class="sxs-lookup"><span data-stu-id="677f7-112">You can create either a framework-dependent or self-contained Windows Service deployment.</span></span> <span data-ttu-id="677f7-113">Pour des informations et des conseils sur les scénarios de déploiement, consultez [Déploiement d’applications .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="677f7-113">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment"></a><span data-ttu-id="677f7-114">Déploiement dépendant du framework</span><span class="sxs-lookup"><span data-stu-id="677f7-114">Framework-dependent deployment</span></span>

<span data-ttu-id="677f7-115">Un déploiement dépendant du framework s’appuie sur la présence d’une version partagée à l’échelle du système de .NET Core sur le système cible.</span><span class="sxs-lookup"><span data-stu-id="677f7-115">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="677f7-116">Lorsque le scénario de déploiement dépendant du framework est utilisé avec une application de service Windows ASP.NET Core, le Kit de développement logiciel (SDK) génère un fichier exécutable (*\*.exe*), appelé *exécutable dépendant du framework*.</span><span class="sxs-lookup"><span data-stu-id="677f7-116">When the FDD scenario is used with an ASP.NET Core Windows Service app, the SDK produces an executable (*\*.exe*), called a *framework-dependent executable*.</span></span>

### <a name="self-contained-deployment"></a><span data-ttu-id="677f7-117">Déploiement autonome</span><span class="sxs-lookup"><span data-stu-id="677f7-117">Self-contained deployment</span></span>

<span data-ttu-id="677f7-118">Un déploiement autonome ne s’appuie sur la présence d’aucun composant partagé sur le système cible.</span><span class="sxs-lookup"><span data-stu-id="677f7-118">Self-contained deployment (SCD) doesn't rely on the presence of shared components on the target system.</span></span> <span data-ttu-id="677f7-119">Le runtime et les dépendances de l’application sont déployés avec l’application sur le système d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="677f7-119">The runtime and the app's dependencies are deployed with the app to the hosting system.</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="677f7-120">Convertir un projet en service Windows</span><span class="sxs-lookup"><span data-stu-id="677f7-120">Convert a project into a Windows Service</span></span>

<span data-ttu-id="677f7-121">Apportez les modifications suivantes à un projet ASP.NET Core existant pour exécuter l’application en tant que service :</span><span class="sxs-lookup"><span data-stu-id="677f7-121">Make the following changes to an existing ASP.NET Core project to run the app as a service:</span></span>

### <a name="project-file-updates"></a><span data-ttu-id="677f7-122">Mises à jour du fichier projet</span><span class="sxs-lookup"><span data-stu-id="677f7-122">Project file updates</span></span>

<span data-ttu-id="677f7-123">Selon le [type de déploiement](#deployment-type) que vous avez choisi, mettez à jour le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="677f7-123">Based on your choice of [deployment type](#deployment-type), update the project file:</span></span>

#### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="677f7-124">Déploiement dépendant du framework</span><span class="sxs-lookup"><span data-stu-id="677f7-124">Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="677f7-125">Ajoutez un [identificateur de runtime (RID)](/dotnet/core/rid-catalog) Windows au `<PropertyGroup>` qui contient la version cible du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="677f7-125">Add a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="677f7-126">Dans l’exemple suivant, le RID est défini sur `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="677f7-126">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="677f7-127">Ajoutez la propriété `<SelfContained>` définie sur `false`.</span><span class="sxs-lookup"><span data-stu-id="677f7-127">Add the `<SelfContained>` property set to `false`.</span></span> <span data-ttu-id="677f7-128">Ces propriétés demandent au Kit SDK de générer un fichier exécutable (*.exe*) pour Windows.</span><span class="sxs-lookup"><span data-stu-id="677f7-128">These properties instruct the SDK to generate an executable (*.exe*) file for Windows.</span></span>

<span data-ttu-id="677f7-129">Un fichier *web.config*, qui est normalement produit lors de la publication d’une application ASP.NET Core, n’est pas nécessaire pour une application de Windows Services.</span><span class="sxs-lookup"><span data-stu-id="677f7-129">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="677f7-130">Pour désactiver la création d’un fichier *web.config*, ajoutez la propriété `<IsTransformWebConfigDisabled>` définie sur `true`.</span><span class="sxs-lookup"><span data-stu-id="677f7-130">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="677f7-131">Ajoutez la propriété `<UseAppHost>` définie sur `true`.</span><span class="sxs-lookup"><span data-stu-id="677f7-131">Add the `<UseAppHost>` property set to `true`.</span></span> <span data-ttu-id="677f7-132">Cette propriété fournit au service un chemin d’activation (un fichier exécutable *.exe*) pour un déploiement dépendant du framework (FDD).</span><span class="sxs-lookup"><span data-stu-id="677f7-132">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.1</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <UseAppHost>true</UseAppHost>
  <SelfContained>false</SelfContained>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

::: moniker-end

#### <a name="self-contained-deployment-scd"></a><span data-ttu-id="677f7-133">Déploiement autonome</span><span class="sxs-lookup"><span data-stu-id="677f7-133">Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="677f7-134">Vérifiez la présence d’un [identificateur de runtime (RID)](/dotnet/core/rid-catalog) Windows ou ajoutez-le au `<PropertyGroup>` qui contient la version cible du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="677f7-134">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add a RID to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="677f7-135">Désactivez la création d’un fichier *web.config* en ajoutant la propriété `<IsTransformWebConfigDisabled>` définie sur `true`.</span><span class="sxs-lookup"><span data-stu-id="677f7-135">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="677f7-136">Pour publier pour plusieurs RID :</span><span class="sxs-lookup"><span data-stu-id="677f7-136">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="677f7-137">Fournissez les RID dans une liste séparée par des points-virgules.</span><span class="sxs-lookup"><span data-stu-id="677f7-137">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="677f7-138">Utilisez le nom de la propriété `<RuntimeIdentifiers>` (pluriel).</span><span class="sxs-lookup"><span data-stu-id="677f7-138">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

  <span data-ttu-id="677f7-139">Pour plus d’informations, consultez le [Catalogue RID .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="677f7-139">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="677f7-140">Ajoutez une référence de package pour [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="677f7-140">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

<span data-ttu-id="677f7-141">Pour activer l’enregistrement du journal des événements Windows, ajoutez une référence de package pour [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="677f7-141">To enable Windows Event Log logging, add a package reference for [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="677f7-142">Pour plus d’informations, consultez la section [Gérer les événements de démarrage et d’arrêt](#handle-starting-and-stopping-events).</span><span class="sxs-lookup"><span data-stu-id="677f7-142">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

### <a name="programmain-updates"></a><span data-ttu-id="677f7-143">Mises à jour Program.Main</span><span class="sxs-lookup"><span data-stu-id="677f7-143">Program.Main updates</span></span>

<span data-ttu-id="677f7-144">Dans `Program.Main`, effectuez les changements suivants :</span><span class="sxs-lookup"><span data-stu-id="677f7-144">Make the following changes in `Program.Main`:</span></span>

* <span data-ttu-id="677f7-145">Pour effectuer des tests et un débogage lors de l’exécution en dehors d’un service, ajoutez un code pour déterminer si l’application s’exécute comme un service ou comme une application console.</span><span class="sxs-lookup"><span data-stu-id="677f7-145">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="677f7-146">Vérifiez si le débogueur est attaché ou si un argument de ligne de commande `--console` est présent.</span><span class="sxs-lookup"><span data-stu-id="677f7-146">Inspect if the debugger is attached or a `--console` command-line argument is present.</span></span>

  <span data-ttu-id="677f7-147">Si l’une de ces deux conditions est remplie (l’application n’est pas exécutée en tant que service), appelez <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> sur l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="677f7-147">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> on the Web Host.</span></span>

  <span data-ttu-id="677f7-148">Si les conditions ne sont pas remplies (l’application est exécutée en tant que service) :</span><span class="sxs-lookup"><span data-stu-id="677f7-148">If the conditions are false (the app is run as a service):</span></span>

  * <span data-ttu-id="677f7-149">Appelez <xref:System.IO.Directory.SetCurrentDirectory*> et utilisez un chemin vers l’emplacement publié de l’application.</span><span class="sxs-lookup"><span data-stu-id="677f7-149">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="677f7-150">N’appelez pas <xref:System.IO.Directory.GetCurrentDirectory*> pour obtenir le chemin d’accès car une application Windows Service retourne le dossier *C:\\WINDOWS\\system32* lorsque <xref:System.IO.Directory.GetCurrentDirectory*> est appelée.</span><span class="sxs-lookup"><span data-stu-id="677f7-150">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="677f7-151">Pour plus d’informations, consultez la section [Répertoire actif et racine du contenu](#current-directory-and-content-root).</span><span class="sxs-lookup"><span data-stu-id="677f7-151">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
  * <span data-ttu-id="677f7-152">Appelez <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> pour exécuter l’application en tant que service.</span><span class="sxs-lookup"><span data-stu-id="677f7-152">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

  <span data-ttu-id="677f7-153">Étant donné que le [fournisseur de configuration de ligne de commande](xref:fundamentals/configuration/index#command-line-configuration-provider) nécessite des paires nom/valeur pour les arguments de ligne de commande, le commutateur `--console` est supprimé des arguments avant que <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> ne les reçoive.</span><span class="sxs-lookup"><span data-stu-id="677f7-153">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives them.</span></span>

* <span data-ttu-id="677f7-154">Pour consigner des informations dans le journal des événements Windows, ajoutez le fournisseur EventLog à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="677f7-154">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="677f7-155">Définissez le niveau de journalisation à l’aide de clé `Logging:LogLevel:Default` dans le fichier *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="677f7-155">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span> <span data-ttu-id="677f7-156">À des fins de démonstration et de test, le fichier des paramètres de l’exemple d’application Production définit le niveau de journalisation sur `Information`.</span><span class="sxs-lookup"><span data-stu-id="677f7-156">For demonstration and testing purposes, the sample app's Production settings file sets the logging level to `Information`.</span></span> <span data-ttu-id="677f7-157">En production, la valeur est généralement définie sur `Error`.</span><span class="sxs-lookup"><span data-stu-id="677f7-157">In production, the value is typically set to `Error`.</span></span> <span data-ttu-id="677f7-158">Pour plus d'informations, consultez <xref:fundamentals/logging/index#windows-eventlog-provider>.</span><span class="sxs-lookup"><span data-stu-id="677f7-158">For more information, see <xref:fundamentals/logging/index#windows-eventlog-provider>.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

## <a name="publish-the-app"></a><span data-ttu-id="677f7-159">Publier l'application</span><span class="sxs-lookup"><span data-stu-id="677f7-159">Publish the app</span></span>

<span data-ttu-id="677f7-160">Publiez l’application avec [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), un [profil de publication Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) ou Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="677f7-160">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="677f7-161">Si vous utilisez Visual Studio, sélectionnez **FolderProfile** et configurez **Emplacement cible** avant de sélectionner le bouton **Publier**.</span><span class="sxs-lookup"><span data-stu-id="677f7-161">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

<span data-ttu-id="677f7-162">Pour publier l’exemple d’application avec des outils d’interface de ligne de commande (CLI), exécutez la commande [dotnet publish](/dotnet/core/tools/dotnet-publish) dans une interface de commande Windows à partir du dossier du projet, en passant la configuration de mise en production (Release) à l’option [-c|--configuration](/dotnet/core/tools/dotnet-publish#options).</span><span class="sxs-lookup"><span data-stu-id="677f7-162">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command in a Windows command shell from the project folder with a Release configuration passed to the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option.</span></span> <span data-ttu-id="677f7-163">Utilisez l’option [-o|--output](/dotnet/core/tools/dotnet-publish#options) avec un chemin d'accès pour publier dans un dossier à l’extérieur de l’application.</span><span class="sxs-lookup"><span data-stu-id="677f7-163">Use the [-o|--output](/dotnet/core/tools/dotnet-publish#options) option with a path to publish to a folder outside of the app.</span></span>

### <a name="publish-a-framework-dependent-deployment-fdd"></a><span data-ttu-id="677f7-164">Publier un déploiement dépendant du framework</span><span class="sxs-lookup"><span data-stu-id="677f7-164">Publish a Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="677f7-165">Dans l’exemple suivant, l’application est publiée dans le dossier *c:\\svc* :</span><span class="sxs-lookup"><span data-stu-id="677f7-165">In the following example, the app is published to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --output c:\svc
```

### <a name="publish-a-self-contained-deployment-scd"></a><span data-ttu-id="677f7-166">Publier un déploiement autonome</span><span class="sxs-lookup"><span data-stu-id="677f7-166">Publish a Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="677f7-167">Le RID doit être spécifié dans la propriété `<RuntimeIdenfifier>` (ou `<RuntimeIdentifiers>`) du fichier projet.</span><span class="sxs-lookup"><span data-stu-id="677f7-167">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="677f7-168">Fournissez le runtime à l’option [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) de la commande `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="677f7-168">Supply the runtime to the [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) option of the `dotnet publish` command.</span></span>

<span data-ttu-id="677f7-169">Dans l’exemple suivant, l’application est publiée pour le runtime `win7-x64` dans le dossier *c:\\svc* :</span><span class="sxs-lookup"><span data-stu-id="677f7-169">In the following example, the app is published for the `win7-x64` runtime to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

## <a name="create-a-user-account"></a><span data-ttu-id="677f7-170">Créer un compte d’utilisateur</span><span class="sxs-lookup"><span data-stu-id="677f7-170">Create a user account</span></span>

<span data-ttu-id="677f7-171">Créez un compte d’utilisateur pour le service avec la cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) dans une interface de commande d’administration PowerShell 6 :</span><span class="sxs-lookup"><span data-stu-id="677f7-171">Create a user account for the service using the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet from an administrative PowerShell 6 command shell:</span></span>

```powershell
New-LocalUser -Name {NAME}
```

<span data-ttu-id="677f7-172">Fournissez un [mot de passe fort](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) à l’invite.</span><span class="sxs-lookup"><span data-stu-id="677f7-172">Provide a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements) when prompted.</span></span>

<span data-ttu-id="677f7-173">Pour l’exemple d’application, créez un compte d’utilisateur portant le nom `ServiceUser`.</span><span class="sxs-lookup"><span data-stu-id="677f7-173">For the sample app, create a user account with the name `ServiceUser`.</span></span>

```powershell
New-LocalUser -Name ServiceUser
```

<span data-ttu-id="677f7-174">Le compte n’expire pas, sauf si le paramètre `-AccountExpires` est fourni à la cmdlet [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) avec un <xref:System.DateTime> d’expiration.</span><span class="sxs-lookup"><span data-stu-id="677f7-174">Unless the `-AccountExpires` parameter is supplied to the [New-LocalUser](/powershell/module/microsoft.powershell.localaccounts/new-localuser) cmdlet with an expiration <xref:System.DateTime>, the account doesn't expire.</span></span>

<span data-ttu-id="677f7-175">Pour plus d’informations, voir [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) et [Comptes d’utilisateurs de service](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="677f7-175">For more information, see [Microsoft.PowerShell.LocalAccounts](/powershell/module/microsoft.powershell.localaccounts/) and [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="677f7-176">Une approche alternative à la gestion des utilisateurs lors de l’utilisation d’Active Directory consiste à utiliser des Comptes de service administrés.</span><span class="sxs-lookup"><span data-stu-id="677f7-176">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="677f7-177">Pour plus d’informations, consultez [Vue d’ensemble des comptes de service administrés de groupe](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="677f7-177">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="set-permission-log-on-as-a-service"></a><span data-ttu-id="677f7-178">Définir l’autorisation : Ouvrir une session en tant que service</span><span class="sxs-lookup"><span data-stu-id="677f7-178">Set permission: Log on as a service</span></span>

<span data-ttu-id="677f7-179">Accordez l’accès en écriture/lecture/exécution au dossier de l’application avec la commande [icacls](/windows-server/administration/windows-commands/icacls) dans une interface de commande d’administration PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="677f7-179">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command an administrative PowerShell 6 command shell.</span></span>

```powershell
icacls "{PATH}" /grant "{USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS}" /t
```

* `{PATH}` <span data-ttu-id="677f7-180">&ndash; Chemin du dossier de l’application.</span><span class="sxs-lookup"><span data-stu-id="677f7-180">&ndash; Path to the app's folder.</span></span>
* `{USER ACCOUNT}` <span data-ttu-id="677f7-181">&ndash; Compte d’utilisateur (SID).</span><span class="sxs-lookup"><span data-stu-id="677f7-181">&ndash; The user account (SID).</span></span>
* `(OI)` <span data-ttu-id="677f7-182">&ndash; L’indicateur Object Inherit propage les autorisations aux fichiers subordonnés.</span><span class="sxs-lookup"><span data-stu-id="677f7-182">&ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
* `(CI)` <span data-ttu-id="677f7-183">&ndash; L’indicateur Container Inherit propage les autorisations aux dossiers subordonnés.</span><span class="sxs-lookup"><span data-stu-id="677f7-183">&ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
* `{PERMISSION FLAGS}` <span data-ttu-id="677f7-184">&ndash; Définit les autorisations d’accès de l’application.</span><span class="sxs-lookup"><span data-stu-id="677f7-184">&ndash; Sets the app's access permissions.</span></span>
  * <span data-ttu-id="677f7-185">Écriture (`W`)</span><span class="sxs-lookup"><span data-stu-id="677f7-185">Write (`W`)</span></span>
  * <span data-ttu-id="677f7-186">Lecture (`R`)</span><span class="sxs-lookup"><span data-stu-id="677f7-186">Read (`R`)</span></span>
  * <span data-ttu-id="677f7-187">Exécution (`X`)</span><span class="sxs-lookup"><span data-stu-id="677f7-187">Execute (`X`)</span></span>
  * <span data-ttu-id="677f7-188">Complet (`F`)</span><span class="sxs-lookup"><span data-stu-id="677f7-188">Full (`F`)</span></span>
  * <span data-ttu-id="677f7-189">Modification (`M`)</span><span class="sxs-lookup"><span data-stu-id="677f7-189">Modify (`M`)</span></span>
* `/t` <span data-ttu-id="677f7-190">&ndash; Appliquer de manière récursive aux dossiers et fichiers subordonnés existants.</span><span class="sxs-lookup"><span data-stu-id="677f7-190">&ndash; Apply recursively to existing subordinate folders and files.</span></span>

<span data-ttu-id="677f7-191">Pour l’exemple d’application publiée dans le dossier *c:\\svc* et le compte `ServiceUser` avec des autorisations en écriture/lecture/exécution, utilisez la commande suivante dans une interface de commande d’administration PowerShell 6.</span><span class="sxs-lookup"><span data-stu-id="677f7-191">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command an administrative PowerShell 6 command shell.</span></span>

```powershell
icacls "c:\svc" /grant "ServiceUser:(OI)(CI)WRX" /t
```

<span data-ttu-id="677f7-192">Pour plus d’informations, consultez [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="677f7-192">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

## <a name="create-the-service"></a><span data-ttu-id="677f7-193">Créer le service</span><span class="sxs-lookup"><span data-stu-id="677f7-193">Create the service</span></span>

<span data-ttu-id="677f7-194">Utilisez le script PowerShell [RegisterService.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts) pour inscrire le service.</span><span class="sxs-lookup"><span data-stu-id="677f7-194">Use the [RegisterService.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts) PowerShell script to register the service.</span></span> <span data-ttu-id="677f7-195">Dans une interface de commande d’administration PowerShell 6, exécutez le script avec la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="677f7-195">From an administrative PowerShell 6 command shell, execute the script with the following command:</span></span>

```powershell
.\RegisterService.ps1 
    -Name {NAME} 
    -DisplayName "{DISPLAY NAME}" 
    -Description "{DESCRIPTION}" 
    -Exe "{PATH TO EXE}\{ASSEMBLY NAME}.exe" 
    -User {DOMAIN\USER}
```

<span data-ttu-id="677f7-196">Dans l’exemple suivant pour l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="677f7-196">In the following example for the sample app:</span></span>

* <span data-ttu-id="677f7-197">Le service s’appelle **MyService**.</span><span class="sxs-lookup"><span data-stu-id="677f7-197">The service is named **MyService**.</span></span>
* <span data-ttu-id="677f7-198">Le service publié réside dans le dossier *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="677f7-198">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="677f7-199">L’application exécutable s’appelle *SampleApp.exe*.</span><span class="sxs-lookup"><span data-stu-id="677f7-199">The app executable is named *SampleApp.exe*.</span></span>
* <span data-ttu-id="677f7-200">Le service s’exécute sous le compte `ServiceUser`.</span><span class="sxs-lookup"><span data-stu-id="677f7-200">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="677f7-201">Dans l’exemple de commande suivant, le nom de l’ordinateur local est `Desktop-PC`.</span><span class="sxs-lookup"><span data-stu-id="677f7-201">In the following example command, the local machine name is `Desktop-PC`.</span></span> <span data-ttu-id="677f7-202">Remplacez `Desktop-PC` par le nom de l’ordinateur ou le domaine de votre système.</span><span class="sxs-lookup"><span data-stu-id="677f7-202">Replace `Desktop-PC` with the computer name or domain for your system.</span></span>

```powershell
.\RegisterService.ps1 
    -Name MyService 
    -DisplayName "My Cool Service" 
    -Description "This is the Sample App service." 
    -Exe "c:\svc\SampleApp.exe" 
    -User Desktop-PC\ServiceUser
```

## <a name="manage-the-service"></a><span data-ttu-id="677f7-203">Gérer le service</span><span class="sxs-lookup"><span data-stu-id="677f7-203">Manage the service</span></span>

### <a name="start-the-service"></a><span data-ttu-id="677f7-204">Démarrer le service</span><span class="sxs-lookup"><span data-stu-id="677f7-204">Start the service</span></span>

<span data-ttu-id="677f7-205">Démarrez le service avec la commande PowerShell 6 `Start-Service -Name {NAME}`.</span><span class="sxs-lookup"><span data-stu-id="677f7-205">Start the service with the `Start-Service -Name {NAME}` PowerShell 6 command.</span></span>

<span data-ttu-id="677f7-206">Pour démarrer l’exemple de service d’application, utilisez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="677f7-206">To start the sample app service, use the following command:</span></span>

```powershell
Start-Service -Name MyService
```

<span data-ttu-id="677f7-207">La commande prend quelques secondes pour démarrer le service.</span><span class="sxs-lookup"><span data-stu-id="677f7-207">The command takes a few seconds to start the service.</span></span>

### <a name="determine-the-service-status"></a><span data-ttu-id="677f7-208">Déterminer l’état du service</span><span class="sxs-lookup"><span data-stu-id="677f7-208">Determine the service status</span></span>

<span data-ttu-id="677f7-209">Pour vérifier l’état du service, utilisez la commande PowerShell 6 `Get-Service -Name {NAME}`.</span><span class="sxs-lookup"><span data-stu-id="677f7-209">To check the status of the service, use the `Get-Service -Name {NAME}` PowerShell 6 command.</span></span> <span data-ttu-id="677f7-210">L’état est signalé comme étant l’une des valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="677f7-210">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

<span data-ttu-id="677f7-211">Utilisez la commande suivante pour vérifier l’état de l’exemple de service d’application :</span><span class="sxs-lookup"><span data-stu-id="677f7-211">Use the following command to check the status of the sample app service:</span></span>

```powershell
Get-Service -Name MyService
```

### <a name="browse-a-web-app-service"></a><span data-ttu-id="677f7-212">Parcourir un service d’application web</span><span class="sxs-lookup"><span data-stu-id="677f7-212">Browse a web app service</span></span>

<span data-ttu-id="677f7-213">Quand le service est une application web et que son état est `RUNNING`, accédez au chemin de l'application (par défaut, `http://localhost:5000`, qui redirige vers `https://localhost:5001` si le [middleware (intergiciel) de redirection HTTPS](xref:security/enforcing-ssl) est utilisé).</span><span class="sxs-lookup"><span data-stu-id="677f7-213">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

<span data-ttu-id="677f7-214">Pour l’exemple de service d’application, accédez au chemin de l'application sur `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="677f7-214">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

### <a name="stop-the-service"></a><span data-ttu-id="677f7-215">Arrêter le service</span><span class="sxs-lookup"><span data-stu-id="677f7-215">Stop the service</span></span>

<span data-ttu-id="677f7-216">Arrêtez le service avec la commande PowerShell 6 `Stop-Service -Name {NAME}`.</span><span class="sxs-lookup"><span data-stu-id="677f7-216">Stop the service with the `Stop-Service -Name {NAME}` Powershell 6 command.</span></span>

<span data-ttu-id="677f7-217">La commande suivante arrête l’exemple de service d’application :</span><span class="sxs-lookup"><span data-stu-id="677f7-217">The following command stops the sample app service:</span></span>

```powershell
Stop-Service -Name MyService
```

### <a name="remove-the-service"></a><span data-ttu-id="677f7-218">Supprimer le service</span><span class="sxs-lookup"><span data-stu-id="677f7-218">Remove the service</span></span>

<span data-ttu-id="677f7-219">Après un court délai pour arrêter un service, supprimez le service avec la commande PowerShell 6 `Remove-Service -Name {NAME}`.</span><span class="sxs-lookup"><span data-stu-id="677f7-219">After a short delay to stop a service, remove the service with the `Remove-Service -Name {NAME}` Powershell 6 command.</span></span>

<span data-ttu-id="677f7-220">Vérifiez l’état de l’exemple de service d’application :</span><span class="sxs-lookup"><span data-stu-id="677f7-220">Check the status of the sample app service:</span></span>

```powershell
Remove-Service -Name MyService
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="677f7-221">Gérer les événements de démarrage et d’arrêt</span><span class="sxs-lookup"><span data-stu-id="677f7-221">Handle starting and stopping events</span></span>

<span data-ttu-id="677f7-222">Pour gérer les événements <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> et <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>, apportez les modifications supplémentaires suivantes :</span><span class="sxs-lookup"><span data-stu-id="677f7-222">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events, perform the following additional changes:</span></span>

1. <span data-ttu-id="677f7-223">Créez une classe qui dérive de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> à l’aide des méthodes `OnStarting`, `OnStarted` et `OnStopping` :</span><span class="sxs-lookup"><span data-stu-id="677f7-223">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="677f7-224">Créez une méthode d’extension pour <xref:Microsoft.AspNetCore.Hosting.IWebHost> qui transmet `CustomWebHostService` à <xref:System.ServiceProcess.ServiceBase.Run*> :</span><span class="sxs-lookup"><span data-stu-id="677f7-224">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="677f7-225">Dans `Program.Main`, appelez la méthode d’extension `RunAsCustomService` au lieu de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> :</span><span class="sxs-lookup"><span data-stu-id="677f7-225">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="677f7-226">Pour afficher l’emplacement de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> dans `Program.Main`, reportez-vous à l’exemple de code indiqué dans la section [Convertir un projet en service Windows](#convert-a-project-into-a-windows-service).</span><span class="sxs-lookup"><span data-stu-id="677f7-226">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Convert a project into a Windows Service](#convert-a-project-into-a-windows-service) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="677f7-227">Scénarios avec un serveur proxy et un équilibreur de charge</span><span class="sxs-lookup"><span data-stu-id="677f7-227">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="677f7-228">Les services qui interagissent avec les requêtes provenant d’Internet ou d’un réseau d’entreprise et qui se trouvent derrière un proxy ou équilibreur de charge peuvent nécessiter une configuration supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="677f7-228">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="677f7-229">Pour plus d'informations, consultez <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="677f7-229">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="677f7-230">Configurer HTTPS</span><span class="sxs-lookup"><span data-stu-id="677f7-230">Configure HTTPS</span></span>

<span data-ttu-id="677f7-231">Pour configurer le service avec un point de terminaison sécurisé :</span><span class="sxs-lookup"><span data-stu-id="677f7-231">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="677f7-232">Créez un certificat X.509 pour le système d’hébergement à l’aide des mécanismes d’acquisition et de déploiement de certificat de votre plateforme.</span><span class="sxs-lookup"><span data-stu-id="677f7-232">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="677f7-233">Spécifiez la [configuration de point de terminaison HTTPS d’un serveur Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) pour utiliser le certificat.</span><span class="sxs-lookup"><span data-stu-id="677f7-233">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="677f7-234">L’utilisation du certificat de développement HTTPS ASP.NET Core pour sécuriser un point de terminaison de service n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="677f7-234">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="677f7-235">Répertoire actif et racine du contenu</span><span class="sxs-lookup"><span data-stu-id="677f7-235">Current directory and content root</span></span>

<span data-ttu-id="677f7-236">Le répertoire de travail actif retourné par l’appel à <xref:System.IO.Directory.GetCurrentDirectory*> pour un service Windows est le dossier *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="677f7-236">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="677f7-237">Le dossier *system32* n’est pas un emplacement approprié pour stocker les fichiers d’un service (tels que les fichiers de paramètres).</span><span class="sxs-lookup"><span data-stu-id="677f7-237">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="677f7-238">Utilisez une des approches suivantes pour gérer les ressources ainsi que les fichiers de paramètres d’un service, et y accéder.</span><span class="sxs-lookup"><span data-stu-id="677f7-238">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="677f7-239">Définir le dossier de l’application comme chemin d’accès racine du contenu</span><span class="sxs-lookup"><span data-stu-id="677f7-239">Set the content root path to the app's folder</span></span>

<span data-ttu-id="677f7-240"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> est le même chemin que celui fourni à l’argument `binPath` quand le service est créé.</span><span class="sxs-lookup"><span data-stu-id="677f7-240">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="677f7-241">Au lieu d’appeler `GetCurrentDirectory` pour créer des chemins d’accès aux fichiers de paramètres, appelez <xref:System.IO.Directory.SetCurrentDirectory*> en utilisant le chemin d’accès à la racine du contenu de l’application.</span><span class="sxs-lookup"><span data-stu-id="677f7-241">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="677f7-242">Dans `Program.Main`, définissez le chemin d’accès au dossier du fichier exécutable du service ainsi que le chemin d’accès pour établir la racine du contenu de l’application :</span><span class="sxs-lookup"><span data-stu-id="677f7-242">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="677f7-243">Stocker les fichiers du service dans un emplacement approprié sur le disque</span><span class="sxs-lookup"><span data-stu-id="677f7-243">Store the service's files in a suitable location on disk</span></span>

<span data-ttu-id="677f7-244">Spécifiez un chemin absolu avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>, si vous utilisez <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>, vers le dossier contenant les fichiers.</span><span class="sxs-lookup"><span data-stu-id="677f7-244">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="677f7-245">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="677f7-245">Additional resources</span></span>

* <span data-ttu-id="677f7-246">[Configuration de point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (inclut la configuration de HTTPS et la prise en charge de SNI)</span><span class="sxs-lookup"><span data-stu-id="677f7-246">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
