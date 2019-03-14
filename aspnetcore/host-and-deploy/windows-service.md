---
title: Héberger ASP.NET Core dans un service Windows
author: guardrex
description: Découvrez comment héberger une application ASP.NET Core dans un service Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/08/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: ecc7f3a8cd813c2803d03294e38d726905eeb1b8
ms.sourcegitcommit: 34bf9fc6ea814c039401fca174642f0acb14be3c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57841421"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="4fb69-103">Héberger ASP.NET Core dans un service Windows</span><span class="sxs-lookup"><span data-stu-id="4fb69-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="4fb69-104">Par [Luke Latham](https://github.com/guardrex) et [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="4fb69-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="4fb69-105">Une application ASP.NET Core peut être hébergée sur Windows en tant que [service Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) sans utiliser IIS.</span><span class="sxs-lookup"><span data-stu-id="4fb69-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="4fb69-106">Lorsqu’elle est hébergée en tant que service Windows, l’application démarre automatiquement après le redémarrage.</span><span class="sxs-lookup"><span data-stu-id="4fb69-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="4fb69-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4fb69-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4fb69-108">Prérequis</span><span class="sxs-lookup"><span data-stu-id="4fb69-108">Prerequisites</span></span>

* [<span data-ttu-id="4fb69-109">PowerShell 6</span><span class="sxs-lookup"><span data-stu-id="4fb69-109">PowerShell 6</span></span>](https://github.com/PowerShell/PowerShell)

## <a name="deployment-type"></a><span data-ttu-id="4fb69-110">Type de déploiement</span><span class="sxs-lookup"><span data-stu-id="4fb69-110">Deployment type</span></span>

<span data-ttu-id="4fb69-111">Vous pouvez créer un déploiement Windows Service dépendant du framework ou autonome.</span><span class="sxs-lookup"><span data-stu-id="4fb69-111">You can create either a framework-dependent or self-contained Windows Service deployment.</span></span> <span data-ttu-id="4fb69-112">Pour des informations et des conseils sur les scénarios de déploiement, consultez [Déploiement d’applications .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="4fb69-112">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment"></a><span data-ttu-id="4fb69-113">Déploiement dépendant du framework</span><span class="sxs-lookup"><span data-stu-id="4fb69-113">Framework-dependent deployment</span></span>

<span data-ttu-id="4fb69-114">Un déploiement dépendant du framework s’appuie sur la présence d’une version partagée à l’échelle du système de .NET Core sur le système cible.</span><span class="sxs-lookup"><span data-stu-id="4fb69-114">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="4fb69-115">Lorsque le scénario de déploiement dépendant du framework est utilisé avec une application de service Windows ASP.NET Core, le Kit de développement logiciel (SDK) génère un fichier exécutable (*\*.exe*), appelé *exécutable dépendant du framework*.</span><span class="sxs-lookup"><span data-stu-id="4fb69-115">When the FDD scenario is used with an ASP.NET Core Windows Service app, the SDK produces an executable (*\*.exe*), called a *framework-dependent executable*.</span></span>

### <a name="self-contained-deployment"></a><span data-ttu-id="4fb69-116">Déploiement autonome</span><span class="sxs-lookup"><span data-stu-id="4fb69-116">Self-contained deployment</span></span>

<span data-ttu-id="4fb69-117">Un déploiement autonome ne s’appuie sur la présence d’aucun composant partagé sur le système cible.</span><span class="sxs-lookup"><span data-stu-id="4fb69-117">Self-contained deployment (SCD) doesn't rely on the presence of shared components on the target system.</span></span> <span data-ttu-id="4fb69-118">Le runtime et les dépendances de l’application sont déployés avec l’application sur le système d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="4fb69-118">The runtime and the app's dependencies are deployed with the app to the hosting system.</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="4fb69-119">Convertir un projet en service Windows</span><span class="sxs-lookup"><span data-stu-id="4fb69-119">Convert a project into a Windows Service</span></span>

<span data-ttu-id="4fb69-120">Apportez les modifications suivantes à un projet ASP.NET Core existant pour exécuter l’application en tant que service :</span><span class="sxs-lookup"><span data-stu-id="4fb69-120">Make the following changes to an existing ASP.NET Core project to run the app as a service:</span></span>

### <a name="project-file-updates"></a><span data-ttu-id="4fb69-121">Mises à jour du fichier projet</span><span class="sxs-lookup"><span data-stu-id="4fb69-121">Project file updates</span></span>

<span data-ttu-id="4fb69-122">Selon le [type de déploiement](#deployment-type) que vous avez choisi, mettez à jour le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="4fb69-122">Based on your choice of [deployment type](#deployment-type), update the project file:</span></span>

#### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="4fb69-123">Déploiement dépendant du framework</span><span class="sxs-lookup"><span data-stu-id="4fb69-123">Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="4fb69-124">Ajoutez un [identificateur de runtime (RID)](/dotnet/core/rid-catalog) Windows au `<PropertyGroup>` qui contient la version cible du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4fb69-124">Add a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="4fb69-125">Dans l’exemple suivant, le RID est défini sur `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="4fb69-125">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="4fb69-126">Ajoutez la propriété `<SelfContained>` définie sur `false`.</span><span class="sxs-lookup"><span data-stu-id="4fb69-126">Add the `<SelfContained>` property set to `false`.</span></span> <span data-ttu-id="4fb69-127">Ces propriétés demandent au Kit SDK de générer un fichier exécutable (*.exe*) pour Windows.</span><span class="sxs-lookup"><span data-stu-id="4fb69-127">These properties instruct the SDK to generate an executable (*.exe*) file for Windows.</span></span>

<span data-ttu-id="4fb69-128">Un fichier *web.config*, qui est normalement produit lors de la publication d’une application ASP.NET Core, n’est pas nécessaire pour une application de Windows Services.</span><span class="sxs-lookup"><span data-stu-id="4fb69-128">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="4fb69-129">Pour désactiver la création d’un fichier *web.config*, ajoutez la propriété `<IsTransformWebConfigDisabled>` définie sur `true`.</span><span class="sxs-lookup"><span data-stu-id="4fb69-129">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="4fb69-130">Ajoutez la propriété `<UseAppHost>` définie sur `true`.</span><span class="sxs-lookup"><span data-stu-id="4fb69-130">Add the `<UseAppHost>` property set to `true`.</span></span> <span data-ttu-id="4fb69-131">Cette propriété fournit au service un chemin d’activation (un fichier exécutable *.exe*) pour un déploiement dépendant du framework (FDD).</span><span class="sxs-lookup"><span data-stu-id="4fb69-131">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

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

#### <a name="self-contained-deployment-scd"></a><span data-ttu-id="4fb69-132">Déploiement autonome</span><span class="sxs-lookup"><span data-stu-id="4fb69-132">Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="4fb69-133">Vérifiez la présence d’un [identificateur de runtime (RID)](/dotnet/core/rid-catalog) Windows ou ajoutez-le au `<PropertyGroup>` qui contient la version cible du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4fb69-133">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add a RID to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="4fb69-134">Désactivez la création d’un fichier *web.config* en ajoutant la propriété `<IsTransformWebConfigDisabled>` définie sur `true`.</span><span class="sxs-lookup"><span data-stu-id="4fb69-134">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="4fb69-135">Pour publier pour plusieurs RID :</span><span class="sxs-lookup"><span data-stu-id="4fb69-135">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="4fb69-136">Fournissez les RID dans une liste séparée par des points-virgules.</span><span class="sxs-lookup"><span data-stu-id="4fb69-136">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="4fb69-137">Utilisez le nom de la propriété `<RuntimeIdentifiers>` (pluriel).</span><span class="sxs-lookup"><span data-stu-id="4fb69-137">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

  <span data-ttu-id="4fb69-138">Pour plus d’informations, consultez le [Catalogue RID .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="4fb69-138">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="4fb69-139">Ajoutez une référence de package pour [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="4fb69-139">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

<span data-ttu-id="4fb69-140">Pour activer l’enregistrement du journal des événements Windows, ajoutez une référence de package pour [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="4fb69-140">To enable Windows Event Log logging, add a package reference for [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="4fb69-141">Pour plus d’informations, consultez la section [Gérer les événements de démarrage et d’arrêt](#handle-starting-and-stopping-events).</span><span class="sxs-lookup"><span data-stu-id="4fb69-141">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

### <a name="programmain-updates"></a><span data-ttu-id="4fb69-142">Mises à jour Program.Main</span><span class="sxs-lookup"><span data-stu-id="4fb69-142">Program.Main updates</span></span>

<span data-ttu-id="4fb69-143">Dans `Program.Main`, effectuez les changements suivants :</span><span class="sxs-lookup"><span data-stu-id="4fb69-143">Make the following changes in `Program.Main`:</span></span>

* <span data-ttu-id="4fb69-144">Pour effectuer des tests et un débogage lors de l’exécution en dehors d’un service, ajoutez un code pour déterminer si l’application s’exécute comme un service ou comme une application console.</span><span class="sxs-lookup"><span data-stu-id="4fb69-144">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="4fb69-145">Vérifiez si le débogueur est attaché ou si un argument de ligne de commande `--console` est présent.</span><span class="sxs-lookup"><span data-stu-id="4fb69-145">Inspect if the debugger is attached or a `--console` command-line argument is present.</span></span>

  <span data-ttu-id="4fb69-146">Si l’une de ces deux conditions est remplie (l’application n’est pas exécutée en tant que service), appelez <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> sur l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="4fb69-146">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> on the Web Host.</span></span>

  <span data-ttu-id="4fb69-147">Si les conditions ne sont pas remplies (l’application est exécutée en tant que service) :</span><span class="sxs-lookup"><span data-stu-id="4fb69-147">If the conditions are false (the app is run as a service):</span></span>

  * <span data-ttu-id="4fb69-148">Appelez <xref:System.IO.Directory.SetCurrentDirectory*> et utilisez un chemin vers l’emplacement publié de l’application.</span><span class="sxs-lookup"><span data-stu-id="4fb69-148">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="4fb69-149">N’appelez pas <xref:System.IO.Directory.GetCurrentDirectory*> pour obtenir le chemin d’accès car une application Windows Service retourne le dossier *C:\\WINDOWS\\system32* lorsque <xref:System.IO.Directory.GetCurrentDirectory*> est appelée.</span><span class="sxs-lookup"><span data-stu-id="4fb69-149">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="4fb69-150">Pour plus d’informations, consultez la section [Répertoire actif et racine du contenu](#current-directory-and-content-root).</span><span class="sxs-lookup"><span data-stu-id="4fb69-150">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
  * <span data-ttu-id="4fb69-151">Appelez <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> pour exécuter l’application en tant que service.</span><span class="sxs-lookup"><span data-stu-id="4fb69-151">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

  <span data-ttu-id="4fb69-152">Étant donné que le [fournisseur de configuration de ligne de commande](xref:fundamentals/configuration/index#command-line-configuration-provider) nécessite des paires nom/valeur pour les arguments de ligne de commande, le commutateur `--console` est supprimé des arguments avant que <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> ne les reçoive.</span><span class="sxs-lookup"><span data-stu-id="4fb69-152">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives them.</span></span>

* <span data-ttu-id="4fb69-153">Pour consigner des informations dans le journal des événements Windows, ajoutez le fournisseur EventLog à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="4fb69-153">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="4fb69-154">Définissez le niveau de journalisation à l’aide de clé `Logging:LogLevel:Default` dans le fichier *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="4fb69-154">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span> <span data-ttu-id="4fb69-155">À des fins de démonstration et de test, le fichier des paramètres de l’exemple d’application Production définit le niveau de journalisation sur `Information`.</span><span class="sxs-lookup"><span data-stu-id="4fb69-155">For demonstration and testing purposes, the sample app's Production settings file sets the logging level to `Information`.</span></span> <span data-ttu-id="4fb69-156">En production, la valeur est généralement définie sur `Error`.</span><span class="sxs-lookup"><span data-stu-id="4fb69-156">In production, the value is typically set to `Error`.</span></span> <span data-ttu-id="4fb69-157">Pour plus d'informations, consultez <xref:fundamentals/logging/index#windows-eventlog-provider>.</span><span class="sxs-lookup"><span data-stu-id="4fb69-157">For more information, see <xref:fundamentals/logging/index#windows-eventlog-provider>.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

## <a name="publish-the-app"></a><span data-ttu-id="4fb69-158">Publier l'application</span><span class="sxs-lookup"><span data-stu-id="4fb69-158">Publish the app</span></span>

<span data-ttu-id="4fb69-159">Publiez l’application avec [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), un [profil de publication Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) ou Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4fb69-159">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="4fb69-160">Si vous utilisez Visual Studio, sélectionnez **FolderProfile** et configurez **Emplacement cible** avant de sélectionner le bouton **Publier**.</span><span class="sxs-lookup"><span data-stu-id="4fb69-160">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

<span data-ttu-id="4fb69-161">Pour publier l’exemple d’application avec des outils de l’interface de ligne de commande (CLI), exécutez la commande [dotnet publish](/dotnet/core/tools/dotnet-publish) à une invite de commandes à partir du dossier du projet, en passant la configuration de mise en production (Release) à l’option [-c|--configuration](/dotnet/core/tools/dotnet-publish#options).</span><span class="sxs-lookup"><span data-stu-id="4fb69-161">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder with a Release configuration passed to the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option.</span></span> <span data-ttu-id="4fb69-162">Utilisez l’option [-o|--output](/dotnet/core/tools/dotnet-publish#options) avec un chemin d'accès pour publier dans un dossier à l’extérieur de l’application.</span><span class="sxs-lookup"><span data-stu-id="4fb69-162">Use the [-o|--output](/dotnet/core/tools/dotnet-publish#options) option with a path to publish to a folder outside of the app.</span></span>

### <a name="publish-a-framework-dependent-deployment-fdd"></a><span data-ttu-id="4fb69-163">Publier un déploiement dépendant du framework</span><span class="sxs-lookup"><span data-stu-id="4fb69-163">Publish a Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="4fb69-164">Dans l’exemple suivant, l’application est publiée dans le dossier *c:\\svc* :</span><span class="sxs-lookup"><span data-stu-id="4fb69-164">In the following example, the app is published to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --output c:\svc
```

### <a name="publish-a-self-contained-deployment-scd"></a><span data-ttu-id="4fb69-165">Publier un déploiement autonome</span><span class="sxs-lookup"><span data-stu-id="4fb69-165">Publish a Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="4fb69-166">Le RID doit être spécifié dans la propriété `<RuntimeIdenfifier>` (ou `<RuntimeIdentifiers>`) du fichier projet.</span><span class="sxs-lookup"><span data-stu-id="4fb69-166">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="4fb69-167">Fournissez le runtime à l’option [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) de la commande `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="4fb69-167">Supply the runtime to the [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) option of the `dotnet publish` command.</span></span>

<span data-ttu-id="4fb69-168">Dans l’exemple suivant, l’application est publiée pour le runtime `win7-x64` dans le dossier *c:\\svc* :</span><span class="sxs-lookup"><span data-stu-id="4fb69-168">In the following example, the app is published for the `win7-x64` runtime to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

## <a name="create-a-user-account"></a><span data-ttu-id="4fb69-169">Créer un compte d’utilisateur</span><span class="sxs-lookup"><span data-stu-id="4fb69-169">Create a user account</span></span>

<span data-ttu-id="4fb69-170">Créez un compte d’utilisateur pour le service en utilisant la commande `net user` à partir d’un shell de commande d’administration PowerShell 6 :</span><span class="sxs-lookup"><span data-stu-id="4fb69-170">Create a user account for the service using the `net user` command from an administrative PowerShell 6 command shell:</span></span>

```powershell
net user {USER ACCOUNT} {PASSWORD} /add
```

<span data-ttu-id="4fb69-171">L’expiration du mot de passe par défaut est de six semaines.</span><span class="sxs-lookup"><span data-stu-id="4fb69-171">The default password expiration is six weeks.</span></span>

<span data-ttu-id="4fb69-172">Pour l’exemple d’application, créez un compte d’utilisateur avec le nom `ServiceUser` et un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="4fb69-172">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="4fb69-173">Dans la commande suivante, remplacez `{PASSWORD}` par un [mot de passe fort](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span><span class="sxs-lookup"><span data-stu-id="4fb69-173">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

```powershell
net user ServiceUser {PASSWORD} /add
```

<span data-ttu-id="4fb69-174">Si vous devez ajouter l’utilisateur à un groupe, utilisez la commande `net localgroup`, où `{GROUP}` est le nom du groupe :</span><span class="sxs-lookup"><span data-stu-id="4fb69-174">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

```powershell
net localgroup {GROUP} {USER ACCOUNT} /add
```

<span data-ttu-id="4fb69-175">Pour plus d’informations, consultez [Comptes d’utilisateur de service](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="4fb69-175">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="4fb69-176">Une approche alternative à la gestion des utilisateurs lors de l’utilisation d’Active Directory consiste à utiliser des Comptes de service administrés.</span><span class="sxs-lookup"><span data-stu-id="4fb69-176">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="4fb69-177">Pour plus d’informations, consultez [Vue d’ensemble des comptes de service administrés de groupe](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="4fb69-177">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

## <a name="set-permission-log-on-as-a-service"></a><span data-ttu-id="4fb69-178">Définir l’autorisation : Ouvrir une session en tant que service</span><span class="sxs-lookup"><span data-stu-id="4fb69-178">Set permission: Log on as a service</span></span>

<span data-ttu-id="4fb69-179">Accordez l’accès en écriture/lecture/exécution au dossier de l’application à l’aide de la commande [icacls](/windows-server/administration/windows-commands/icacls) :</span><span class="sxs-lookup"><span data-stu-id="4fb69-179">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command:</span></span>

```powershell
icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
```

* <span data-ttu-id="4fb69-180">`{PATH}` &ndash; Chemin au dossier de l’application.</span><span class="sxs-lookup"><span data-stu-id="4fb69-180">`{PATH}` &ndash; Path to the app's folder.</span></span>
* <span data-ttu-id="4fb69-181">`{USER ACCOUNT}` &ndash; Compte d’utilisateur (SID).</span><span class="sxs-lookup"><span data-stu-id="4fb69-181">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
* <span data-ttu-id="4fb69-182">`(OI)` &ndash; L’indicateur Object Inherit propage les autorisations aux fichiers subordonnés.</span><span class="sxs-lookup"><span data-stu-id="4fb69-182">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
* <span data-ttu-id="4fb69-183">`(CI)` &ndash; L’indicateur Container Inherit propage les autorisations aux dossiers subordonnés.</span><span class="sxs-lookup"><span data-stu-id="4fb69-183">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
* <span data-ttu-id="4fb69-184">`{PERMISSION FLAGS}` &ndash; Définit les autorisations d’accès de l’application.</span><span class="sxs-lookup"><span data-stu-id="4fb69-184">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
  * <span data-ttu-id="4fb69-185">Écriture (`W`)</span><span class="sxs-lookup"><span data-stu-id="4fb69-185">Write (`W`)</span></span>
  * <span data-ttu-id="4fb69-186">Lecture (`R`)</span><span class="sxs-lookup"><span data-stu-id="4fb69-186">Read (`R`)</span></span>
  * <span data-ttu-id="4fb69-187">Exécution (`X`)</span><span class="sxs-lookup"><span data-stu-id="4fb69-187">Execute (`X`)</span></span>
  * <span data-ttu-id="4fb69-188">Complet (`F`)</span><span class="sxs-lookup"><span data-stu-id="4fb69-188">Full (`F`)</span></span>
  * <span data-ttu-id="4fb69-189">Modification (`M`)</span><span class="sxs-lookup"><span data-stu-id="4fb69-189">Modify (`M`)</span></span>
* <span data-ttu-id="4fb69-190">`/t` &ndash; Appliquer de manière récursive aux dossiers et fichiers subordonnés existants.</span><span class="sxs-lookup"><span data-stu-id="4fb69-190">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

<span data-ttu-id="4fb69-191">Pour l’exemple d’application publiée dans le dossier *c:\\svc* et le compte `ServiceUser` avec des autorisations en écriture/lecture/exécution, utilisez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="4fb69-191">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

```powershell
icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
```

<span data-ttu-id="4fb69-192">Pour plus d’informations, consultez [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="4fb69-192">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

## <a name="create-the-service"></a><span data-ttu-id="4fb69-193">Créer le service</span><span class="sxs-lookup"><span data-stu-id="4fb69-193">Create the service</span></span>

<span data-ttu-id="4fb69-194">Utilisez le script PowerShell [RegisterService.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts) pour inscrire le service.</span><span class="sxs-lookup"><span data-stu-id="4fb69-194">Use the [RegisterService.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/scripts) PowerShell script to register the service.</span></span> <span data-ttu-id="4fb69-195">À partir d’une invite de commandes d’administration PowerShell 6, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="4fb69-195">From an administrative PowerShell 6 command prompt, execute the following command:</span></span>

```powershell
.\RegisterService.ps1 
    -Name {NAME} 
    -DisplayName "{DISPLAY NAME}" 
    -Description "{DESCRIPTION}" 
    -Path "{PATH}" 
    -Exe {ASSEMBLY}.exe 
    -User {DOMAIN\USER}
```

<span data-ttu-id="4fb69-196">Dans l’exemple suivant pour l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="4fb69-196">In the following example for the sample app:</span></span>

* <span data-ttu-id="4fb69-197">Le service s’appelle **MyService**.</span><span class="sxs-lookup"><span data-stu-id="4fb69-197">The service is named **MyService**.</span></span>
* <span data-ttu-id="4fb69-198">Le service publié réside dans le dossier *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="4fb69-198">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="4fb69-199">L’application exécutable s’appelle *SampleApp.exe*.</span><span class="sxs-lookup"><span data-stu-id="4fb69-199">The app executable is named *SampleApp.exe*.</span></span>
* <span data-ttu-id="4fb69-200">Le service s’exécute sous le compte `ServiceUser`.</span><span class="sxs-lookup"><span data-stu-id="4fb69-200">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="4fb69-201">Dans l’exemple suivant, le nom de l’ordinateur local est `Desktop-PC`.</span><span class="sxs-lookup"><span data-stu-id="4fb69-201">In the following example, the local machine name is `Desktop-PC`.</span></span>

```powershell
.\RegisterService.ps1 
    -Name MyService 
    -DisplayName "My Cool Service" 
    -Description "This is the Sample App service." 
    -Path "c:\svc" 
    -Exe SampleApp.exe 
    -User Desktop-PC\ServiceUser
```

## <a name="manage-the-service"></a><span data-ttu-id="4fb69-202">Gérer le service</span><span class="sxs-lookup"><span data-stu-id="4fb69-202">Manage the service</span></span>

### <a name="start-the-service"></a><span data-ttu-id="4fb69-203">Démarrer le service</span><span class="sxs-lookup"><span data-stu-id="4fb69-203">Start the service</span></span>

<span data-ttu-id="4fb69-204">Démarrez le service avec la commande PowerShell 6 `Start-Service -Name {NAME}`.</span><span class="sxs-lookup"><span data-stu-id="4fb69-204">Start the service with the `Start-Service -Name {NAME}` PowerShell 6 command.</span></span>

<span data-ttu-id="4fb69-205">Pour démarrer l’exemple de service d’application, utilisez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="4fb69-205">To start the sample app service, use the following command:</span></span>

```powershell
Start-Service -Name MyService
```

<span data-ttu-id="4fb69-206">La commande prend quelques secondes pour démarrer le service.</span><span class="sxs-lookup"><span data-stu-id="4fb69-206">The command takes a few seconds to start the service.</span></span>

### <a name="determine-the-service-status"></a><span data-ttu-id="4fb69-207">Déterminer l’état du service</span><span class="sxs-lookup"><span data-stu-id="4fb69-207">Determine the service status</span></span>

<span data-ttu-id="4fb69-208">Pour vérifier l’état du service, utilisez la commande PowerShell 6 `Get-Service -Name {NAME}`.</span><span class="sxs-lookup"><span data-stu-id="4fb69-208">To check the status of the service, use the `Get-Service -Name {NAME}` PowerShell 6 command.</span></span> <span data-ttu-id="4fb69-209">L’état est signalé comme étant l’une des valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="4fb69-209">The status is reported as one of the following values:</span></span>

* `Starting`
* `Running`
* `Stopping`
* `Stopped`

<span data-ttu-id="4fb69-210">Utilisez la commande suivante pour vérifier l’état de l’exemple de service d’application :</span><span class="sxs-lookup"><span data-stu-id="4fb69-210">Use the following command to check the status of the sample app service:</span></span>

```powershell
Get-Service -Name MyService
```

### <a name="browse-a-web-app-service"></a><span data-ttu-id="4fb69-211">Parcourir un service d’application web</span><span class="sxs-lookup"><span data-stu-id="4fb69-211">Browse a web app service</span></span>

<span data-ttu-id="4fb69-212">Quand le service est une application web et que son état est `RUNNING`, accédez au chemin de l'application (par défaut, `http://localhost:5000`, qui redirige vers `https://localhost:5001` si le [middleware (intergiciel) de redirection HTTPS](xref:security/enforcing-ssl) est utilisé).</span><span class="sxs-lookup"><span data-stu-id="4fb69-212">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

<span data-ttu-id="4fb69-213">Pour l’exemple de service d’application, accédez au chemin de l'application sur `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="4fb69-213">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

### <a name="stop-the-service"></a><span data-ttu-id="4fb69-214">Arrêter le service</span><span class="sxs-lookup"><span data-stu-id="4fb69-214">Stop the service</span></span>

<span data-ttu-id="4fb69-215">Arrêtez le service avec la commande PowerShell 6 `Stop-Service -Name {NAME}`.</span><span class="sxs-lookup"><span data-stu-id="4fb69-215">Stop the service with the `Stop-Service -Name {NAME}` Powershell 6 command.</span></span>

<span data-ttu-id="4fb69-216">La commande suivante arrête l’exemple de service d’application :</span><span class="sxs-lookup"><span data-stu-id="4fb69-216">The following command stops the sample app service:</span></span>

```powershell
Stop-Service -Name MyService
```

### <a name="remove-the-service"></a><span data-ttu-id="4fb69-217">Supprimer le service</span><span class="sxs-lookup"><span data-stu-id="4fb69-217">Remove the service</span></span>

<span data-ttu-id="4fb69-218">Après un court délai pour arrêter un service, supprimez le service avec la commande PowerShell 6 `Remove-Service -Name {NAME}`.</span><span class="sxs-lookup"><span data-stu-id="4fb69-218">After a short delay to stop a service, remove the service with the `Remove-Service -Name {NAME}` Powershell 6 command.</span></span>

<span data-ttu-id="4fb69-219">Vérifiez l’état de l’exemple de service d’application :</span><span class="sxs-lookup"><span data-stu-id="4fb69-219">Check the status of the sample app service:</span></span>

```powershell
Remove-Service -Name MyService
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="4fb69-220">Gérer les événements de démarrage et d’arrêt</span><span class="sxs-lookup"><span data-stu-id="4fb69-220">Handle starting and stopping events</span></span>

<span data-ttu-id="4fb69-221">Pour gérer les événements <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> et <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>, apportez les modifications supplémentaires suivantes :</span><span class="sxs-lookup"><span data-stu-id="4fb69-221">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events, perform the following additional changes:</span></span>

1. <span data-ttu-id="4fb69-222">Créez une classe qui dérive de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> à l’aide des méthodes `OnStarting`, `OnStarted` et `OnStopping` :</span><span class="sxs-lookup"><span data-stu-id="4fb69-222">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="4fb69-223">Créez une méthode d’extension pour <xref:Microsoft.AspNetCore.Hosting.IWebHost> qui transmet `CustomWebHostService` à <xref:System.ServiceProcess.ServiceBase.Run*> :</span><span class="sxs-lookup"><span data-stu-id="4fb69-223">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="4fb69-224">Dans `Program.Main`, appelez la méthode d’extension `RunAsCustomService` au lieu de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> :</span><span class="sxs-lookup"><span data-stu-id="4fb69-224">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="4fb69-225">Pour afficher l’emplacement de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> dans `Program.Main`, reportez-vous à l’exemple de code indiqué dans la section [Convertir un projet en service Windows](#convert-a-project-into-a-windows-service).</span><span class="sxs-lookup"><span data-stu-id="4fb69-225">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Convert a project into a Windows Service](#convert-a-project-into-a-windows-service) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="4fb69-226">Scénarios avec un serveur proxy et un équilibreur de charge</span><span class="sxs-lookup"><span data-stu-id="4fb69-226">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="4fb69-227">Les services qui interagissent avec les requêtes provenant d’Internet ou d’un réseau d’entreprise et qui se trouvent derrière un proxy ou équilibreur de charge peuvent nécessiter une configuration supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="4fb69-227">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="4fb69-228">Pour plus d'informations, consultez <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="4fb69-228">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="4fb69-229">Configurer HTTPS</span><span class="sxs-lookup"><span data-stu-id="4fb69-229">Configure HTTPS</span></span>

<span data-ttu-id="4fb69-230">Pour configurer le service avec un point de terminaison sécurisé :</span><span class="sxs-lookup"><span data-stu-id="4fb69-230">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="4fb69-231">Créez un certificat X.509 pour le système d’hébergement à l’aide des mécanismes d’acquisition et de déploiement de certificat de votre plateforme.</span><span class="sxs-lookup"><span data-stu-id="4fb69-231">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="4fb69-232">Spécifiez la [configuration de point de terminaison HTTPS d’un serveur Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) pour utiliser le certificat.</span><span class="sxs-lookup"><span data-stu-id="4fb69-232">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="4fb69-233">L’utilisation du certificat de développement HTTPS ASP.NET Core pour sécuriser un point de terminaison de service n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="4fb69-233">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="4fb69-234">Répertoire actif et racine du contenu</span><span class="sxs-lookup"><span data-stu-id="4fb69-234">Current directory and content root</span></span>

<span data-ttu-id="4fb69-235">Le répertoire de travail actif retourné par l’appel à <xref:System.IO.Directory.GetCurrentDirectory*> pour un service Windows est le dossier *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="4fb69-235">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="4fb69-236">Le dossier *system32* n’est pas un emplacement approprié pour stocker les fichiers d’un service (tels que les fichiers de paramètres).</span><span class="sxs-lookup"><span data-stu-id="4fb69-236">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="4fb69-237">Utilisez une des approches suivantes pour gérer les ressources ainsi que les fichiers de paramètres d’un service, et y accéder.</span><span class="sxs-lookup"><span data-stu-id="4fb69-237">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="4fb69-238">Définir le dossier de l’application comme chemin d’accès racine du contenu</span><span class="sxs-lookup"><span data-stu-id="4fb69-238">Set the content root path to the app's folder</span></span>

<span data-ttu-id="4fb69-239"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> est le même chemin que celui fourni à l’argument `binPath` quand le service est créé.</span><span class="sxs-lookup"><span data-stu-id="4fb69-239">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="4fb69-240">Au lieu d’appeler `GetCurrentDirectory` pour créer des chemins d’accès aux fichiers de paramètres, appelez <xref:System.IO.Directory.SetCurrentDirectory*> en utilisant le chemin d’accès à la racine du contenu de l’application.</span><span class="sxs-lookup"><span data-stu-id="4fb69-240">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="4fb69-241">Dans `Program.Main`, définissez le chemin d’accès au dossier du fichier exécutable du service ainsi que le chemin d’accès pour établir la racine du contenu de l’application :</span><span class="sxs-lookup"><span data-stu-id="4fb69-241">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="4fb69-242">Stocker les fichiers du service dans un emplacement approprié sur le disque</span><span class="sxs-lookup"><span data-stu-id="4fb69-242">Store the service's files in a suitable location on disk</span></span>

<span data-ttu-id="4fb69-243">Spécifiez un chemin absolu avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>, si vous utilisez <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>, vers le dossier contenant les fichiers.</span><span class="sxs-lookup"><span data-stu-id="4fb69-243">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4fb69-244">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="4fb69-244">Additional resources</span></span>

* <span data-ttu-id="4fb69-245">[Configuration de point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (inclut la configuration de HTTPS et la prise en charge de SNI)</span><span class="sxs-lookup"><span data-stu-id="4fb69-245">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
