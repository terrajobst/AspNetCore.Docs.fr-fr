---
title: Héberger ASP.NET Core dans un service Windows
author: guardrex
description: Découvrez comment héberger une application ASP.NET Core dans un service Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/13/2019
uid: host-and-deploy/windows-service
ms.openlocfilehash: 081a631c9c3e74c01e15f4b0b272d650c162bd20
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248249"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="5a17a-103">Héberger ASP.NET Core dans un service Windows</span><span class="sxs-lookup"><span data-stu-id="5a17a-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="5a17a-104">Par [Luke Latham](https://github.com/guardrex) et [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="5a17a-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="5a17a-105">Une application ASP.NET Core peut être hébergée sur Windows en tant que [service Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications) sans utiliser IIS.</span><span class="sxs-lookup"><span data-stu-id="5a17a-105">An ASP.NET Core app can be hosted on Windows as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications) without using IIS.</span></span> <span data-ttu-id="5a17a-106">Lorsqu’elle est hébergée en tant que service Windows, l’application démarre automatiquement après le redémarrage.</span><span class="sxs-lookup"><span data-stu-id="5a17a-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="5a17a-107">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5a17a-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="deployment-type"></a><span data-ttu-id="5a17a-108">Type de déploiement</span><span class="sxs-lookup"><span data-stu-id="5a17a-108">Deployment type</span></span>

<span data-ttu-id="5a17a-109">Vous pouvez créer un déploiement Windows Service dépendant du framework ou autonome.</span><span class="sxs-lookup"><span data-stu-id="5a17a-109">You can create either a framework-dependent or self-contained Windows Service deployment.</span></span> <span data-ttu-id="5a17a-110">Pour des informations et des conseils sur les scénarios de déploiement, consultez [Déploiement d’applications .NET Core](/dotnet/core/deploying/).</span><span class="sxs-lookup"><span data-stu-id="5a17a-110">For information and advice on deployment scenarios, see [.NET Core application deployment](/dotnet/core/deploying/).</span></span>

### <a name="framework-dependent-deployment"></a><span data-ttu-id="5a17a-111">Déploiement dépendant du framework</span><span class="sxs-lookup"><span data-stu-id="5a17a-111">Framework-dependent deployment</span></span>

<span data-ttu-id="5a17a-112">Un déploiement dépendant du framework s’appuie sur la présence d’une version partagée à l’échelle du système de .NET Core sur le système cible.</span><span class="sxs-lookup"><span data-stu-id="5a17a-112">Framework-dependent deployment (FDD) relies on the presence of a shared system-wide version of .NET Core on the target system.</span></span> <span data-ttu-id="5a17a-113">Lorsque le scénario de déploiement dépendant du framework est utilisé avec une application de service Windows ASP.NET Core, le Kit de développement logiciel (SDK) génère un fichier exécutable (*\*.exe*), appelé *exécutable dépendant du framework*.</span><span class="sxs-lookup"><span data-stu-id="5a17a-113">When the FDD scenario is used with an ASP.NET Core Windows Service app, the SDK produces an executable (*\*.exe*), called a *framework-dependent executable*.</span></span>

### <a name="self-contained-deployment"></a><span data-ttu-id="5a17a-114">Déploiement autonome</span><span class="sxs-lookup"><span data-stu-id="5a17a-114">Self-contained deployment</span></span>

<span data-ttu-id="5a17a-115">Un déploiement autonome ne s’appuie sur la présence d’aucun composant partagé sur le système cible.</span><span class="sxs-lookup"><span data-stu-id="5a17a-115">Self-contained deployment (SCD) doesn't rely on the presence of shared components on the target system.</span></span> <span data-ttu-id="5a17a-116">Le runtime et les dépendances de l’application sont déployés avec l’application sur le système d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="5a17a-116">The runtime and the app's dependencies are deployed with the app to the hosting system.</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="5a17a-117">Convertir un projet en service Windows</span><span class="sxs-lookup"><span data-stu-id="5a17a-117">Convert a project into a Windows Service</span></span>

<span data-ttu-id="5a17a-118">Apportez les modifications suivantes à un projet ASP.NET Core existant pour exécuter l’application en tant que service :</span><span class="sxs-lookup"><span data-stu-id="5a17a-118">Make the following changes to an existing ASP.NET Core project to run the app as a service:</span></span>

### <a name="project-file-updates"></a><span data-ttu-id="5a17a-119">Mises à jour du fichier projet</span><span class="sxs-lookup"><span data-stu-id="5a17a-119">Project file updates</span></span>

<span data-ttu-id="5a17a-120">Selon le [type de déploiement](#deployment-type) que vous avez choisi, mettez à jour le fichier projet :</span><span class="sxs-lookup"><span data-stu-id="5a17a-120">Based on your choice of [deployment type](#deployment-type), update the project file:</span></span>

#### <a name="framework-dependent-deployment-fdd"></a><span data-ttu-id="5a17a-121">Déploiement dépendant du framework</span><span class="sxs-lookup"><span data-stu-id="5a17a-121">Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="5a17a-122">Ajoutez un [identificateur de runtime (RID)](/dotnet/core/rid-catalog) Windows au `<PropertyGroup>` qui contient la version cible du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5a17a-122">Add a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="5a17a-123">Dans l’exemple suivant, le RID est défini sur `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="5a17a-123">In the following example, the RID is set to `win7-x64`.</span></span> <span data-ttu-id="5a17a-124">Ajoutez la propriété `<SelfContained>` définie sur `false`.</span><span class="sxs-lookup"><span data-stu-id="5a17a-124">Add the `<SelfContained>` property set to `false`.</span></span> <span data-ttu-id="5a17a-125">Ces propriétés demandent au Kit SDK de générer un fichier exécutable (*.exe*) pour Windows.</span><span class="sxs-lookup"><span data-stu-id="5a17a-125">These properties instruct the SDK to generate an executable (*.exe*) file for Windows.</span></span>

<span data-ttu-id="5a17a-126">Un fichier *web.config*, qui est normalement produit lors de la publication d’une application ASP.NET Core, n’est pas nécessaire pour une application de Windows Services.</span><span class="sxs-lookup"><span data-stu-id="5a17a-126">A *web.config* file, which is normally produced when publishing an ASP.NET Core app, is unnecessary for a Windows Services app.</span></span> <span data-ttu-id="5a17a-127">Pour désactiver la création d’un fichier *web.config*, ajoutez la propriété `<IsTransformWebConfigDisabled>` définie sur `true`.</span><span class="sxs-lookup"><span data-stu-id="5a17a-127">To disable the creation of the *web.config* file, add the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

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

<span data-ttu-id="5a17a-128">Ajoutez la propriété `<UseAppHost>` définie sur `true`.</span><span class="sxs-lookup"><span data-stu-id="5a17a-128">Add the `<UseAppHost>` property set to `true`.</span></span> <span data-ttu-id="5a17a-129">Cette propriété fournit au service un chemin d’activation (un fichier exécutable *.exe*) pour un déploiement dépendant du framework (FDD).</span><span class="sxs-lookup"><span data-stu-id="5a17a-129">This property provides the service with an activation path (an executable, *.exe*) for an FDD.</span></span>

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

#### <a name="self-contained-deployment-scd"></a><span data-ttu-id="5a17a-130">Déploiement autonome</span><span class="sxs-lookup"><span data-stu-id="5a17a-130">Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="5a17a-131">Vérifiez la présence d’un [identificateur de runtime (RID)](/dotnet/core/rid-catalog) Windows ou ajoutez-le au `<PropertyGroup>` qui contient la version cible du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="5a17a-131">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add a RID to the `<PropertyGroup>` that contains the target framework.</span></span> <span data-ttu-id="5a17a-132">Désactivez la création d’un fichier *web.config* en ajoutant la propriété `<IsTransformWebConfigDisabled>` définie sur `true`.</span><span class="sxs-lookup"><span data-stu-id="5a17a-132">Disable the creation of a *web.config* file by adding the `<IsTransformWebConfigDisabled>` property set to `true`.</span></span>

```xml
<PropertyGroup>
  <TargetFramework>netcoreapp2.2</TargetFramework>
  <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="5a17a-133">Pour publier pour plusieurs RID :</span><span class="sxs-lookup"><span data-stu-id="5a17a-133">To publish for multiple RIDs:</span></span>

* <span data-ttu-id="5a17a-134">Fournissez les RID dans une liste séparée par des points-virgules.</span><span class="sxs-lookup"><span data-stu-id="5a17a-134">Provide the RIDs in a semicolon-delimited list.</span></span>
* <span data-ttu-id="5a17a-135">Utilisez le nom de la propriété `<RuntimeIdentifiers>` (pluriel).</span><span class="sxs-lookup"><span data-stu-id="5a17a-135">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

  <span data-ttu-id="5a17a-136">Pour plus d’informations, consultez le [Catalogue RID .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="5a17a-136">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

<span data-ttu-id="5a17a-137">Ajoutez une référence de package pour [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="5a17a-137">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

<span data-ttu-id="5a17a-138">Pour activer l’enregistrement du journal des événements Windows, ajoutez une référence de package pour [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span><span class="sxs-lookup"><span data-stu-id="5a17a-138">To enable Windows Event Log logging, add a package reference for [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog).</span></span>

<span data-ttu-id="5a17a-139">Pour plus d’informations, consultez la section [Gérer les événements de démarrage et d’arrêt](#handle-starting-and-stopping-events).</span><span class="sxs-lookup"><span data-stu-id="5a17a-139">For more information, see the [Handle starting and stopping events](#handle-starting-and-stopping-events) section.</span></span>

### <a name="programmain-updates"></a><span data-ttu-id="5a17a-140">Mises à jour Program.Main</span><span class="sxs-lookup"><span data-stu-id="5a17a-140">Program.Main updates</span></span>

<span data-ttu-id="5a17a-141">Dans `Program.Main`, effectuez les changements suivants :</span><span class="sxs-lookup"><span data-stu-id="5a17a-141">Make the following changes in `Program.Main`:</span></span>

* <span data-ttu-id="5a17a-142">Pour effectuer des tests et un débogage lors de l’exécution en dehors d’un service, ajoutez un code pour déterminer si l’application s’exécute comme un service ou comme une application console.</span><span class="sxs-lookup"><span data-stu-id="5a17a-142">To test and debug when running outside of a service, add code to determine if the app is running as a service or a console app.</span></span> <span data-ttu-id="5a17a-143">Vérifiez si le débogueur est attaché ou si un argument de ligne de commande `--console` est présent.</span><span class="sxs-lookup"><span data-stu-id="5a17a-143">Inspect if the debugger is attached or a `--console` command-line argument is present.</span></span>

  <span data-ttu-id="5a17a-144">Si l’une de ces deux conditions est remplie (l’application n’est pas exécutée en tant que service), appelez <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> sur l’hôte web.</span><span class="sxs-lookup"><span data-stu-id="5a17a-144">If either condition is true (the app isn't run as a service), call <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> on the Web Host.</span></span>

  <span data-ttu-id="5a17a-145">Si les conditions ne sont pas remplies (l’application est exécutée en tant que service) :</span><span class="sxs-lookup"><span data-stu-id="5a17a-145">If the conditions are false (the app is run as a service):</span></span>

  * <span data-ttu-id="5a17a-146">Appelez <xref:System.IO.Directory.SetCurrentDirectory*> et utilisez un chemin vers l’emplacement publié de l’application.</span><span class="sxs-lookup"><span data-stu-id="5a17a-146">Call <xref:System.IO.Directory.SetCurrentDirectory*> and use a path to the app's published location.</span></span> <span data-ttu-id="5a17a-147">N’appelez pas <xref:System.IO.Directory.GetCurrentDirectory*> pour obtenir le chemin d’accès car une application Windows Service retourne le dossier *C:\\WINDOWS\\system32* lorsque <xref:System.IO.Directory.GetCurrentDirectory*> est appelée.</span><span class="sxs-lookup"><span data-stu-id="5a17a-147">Don't call <xref:System.IO.Directory.GetCurrentDirectory*> to obtain the path because a Windows Service app returns the *C:\\WINDOWS\\system32* folder when <xref:System.IO.Directory.GetCurrentDirectory*> is called.</span></span> <span data-ttu-id="5a17a-148">Pour plus d’informations, consultez la section [Répertoire actif et racine du contenu](#current-directory-and-content-root).</span><span class="sxs-lookup"><span data-stu-id="5a17a-148">For more information, see the [Current directory and content root](#current-directory-and-content-root) section.</span></span>
  * <span data-ttu-id="5a17a-149">Appelez <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> pour exécuter l’application en tant que service.</span><span class="sxs-lookup"><span data-stu-id="5a17a-149">Call <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> to run the app as a service.</span></span>

  <span data-ttu-id="5a17a-150">Étant donné que le [fournisseur de configuration de ligne de commande](xref:fundamentals/configuration/index#command-line-configuration-provider) nécessite des paires nom/valeur pour les arguments de ligne de commande, le commutateur `--console` est supprimé des arguments avant que <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> ne les reçoive.</span><span class="sxs-lookup"><span data-stu-id="5a17a-150">Because the [Command-line Configuration Provider](xref:fundamentals/configuration/index#command-line-configuration-provider) requires name-value pairs for command-line arguments, the `--console` switch is removed from the arguments before <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> receives them.</span></span>

* <span data-ttu-id="5a17a-151">Pour consigner des informations dans le journal des événements Windows, ajoutez le fournisseur EventLog à <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span><span class="sxs-lookup"><span data-stu-id="5a17a-151">To write to the Windows Event Log, add the EventLog provider to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureLogging*>.</span></span> <span data-ttu-id="5a17a-152">Définissez le niveau de journalisation à l’aide de clé `Logging:LogLevel:Default` dans le fichier *appsettings.Production.json*.</span><span class="sxs-lookup"><span data-stu-id="5a17a-152">Set the logging level with the `Logging:LogLevel:Default` key in the *appsettings.Production.json* file.</span></span> <span data-ttu-id="5a17a-153">À des fins de démonstration et de test, le fichier des paramètres de l’exemple d’application Production définit le niveau de journalisation sur `Information`.</span><span class="sxs-lookup"><span data-stu-id="5a17a-153">For demonstration and testing purposes, the sample app's Production settings file sets the logging level to `Information`.</span></span> <span data-ttu-id="5a17a-154">En production, la valeur est généralement définie sur `Error`.</span><span class="sxs-lookup"><span data-stu-id="5a17a-154">In production, the value is typically set to `Error`.</span></span> <span data-ttu-id="5a17a-155">Pour plus d'informations, consultez <xref:fundamentals/logging/index#windows-eventlog-provider>.</span><span class="sxs-lookup"><span data-stu-id="5a17a-155">For more information, see <xref:fundamentals/logging/index#windows-eventlog-provider>.</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=snippet_Program)]

### <a name="publish-the-app"></a><span data-ttu-id="5a17a-156">Publier l'application</span><span class="sxs-lookup"><span data-stu-id="5a17a-156">Publish the app</span></span>

<span data-ttu-id="5a17a-157">Publiez l’application avec [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), un [profil de publication Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) ou Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5a17a-157">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="5a17a-158">Si vous utilisez Visual Studio, sélectionnez **FolderProfile** et configurez **Emplacement cible** avant de sélectionner le bouton **Publier**.</span><span class="sxs-lookup"><span data-stu-id="5a17a-158">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

<span data-ttu-id="5a17a-159">Pour publier l’exemple d’application avec des outils de l’interface de ligne de commande (CLI), exécutez la commande [dotnet publish](/dotnet/core/tools/dotnet-publish) à une invite de commandes à partir du dossier du projet, en passant la configuration de mise en production (Release) à l’option [-c|--configuration](/dotnet/core/tools/dotnet-publish#options).</span><span class="sxs-lookup"><span data-stu-id="5a17a-159">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder with a Release configuration passed to the [-c|--configuration](/dotnet/core/tools/dotnet-publish#options) option.</span></span> <span data-ttu-id="5a17a-160">Utilisez l’option [-o|--output](/dotnet/core/tools/dotnet-publish#options) avec un chemin d'accès pour publier dans un dossier à l’extérieur de l’application.</span><span class="sxs-lookup"><span data-stu-id="5a17a-160">Use the [-o|--output](/dotnet/core/tools/dotnet-publish#options) option with a path to publish to a folder outside of the app.</span></span>

#### <a name="publish-a-framework-dependent-deployment-fdd"></a><span data-ttu-id="5a17a-161">Publier un déploiement dépendant du framework</span><span class="sxs-lookup"><span data-stu-id="5a17a-161">Publish a Framework-dependent Deployment (FDD)</span></span>

<span data-ttu-id="5a17a-162">Dans l’exemple suivant, l’application est publiée dans le dossier *c:\\svc* :</span><span class="sxs-lookup"><span data-stu-id="5a17a-162">In the following example, the app is published to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --output c:\svc
```

#### <a name="publish-a-self-contained-deployment-scd"></a><span data-ttu-id="5a17a-163">Publier un déploiement autonome</span><span class="sxs-lookup"><span data-stu-id="5a17a-163">Publish a Self-contained Deployment (SCD)</span></span>

<span data-ttu-id="5a17a-164">Le RID doit être spécifié dans la propriété `<RuntimeIdenfifier>` (ou `<RuntimeIdentifiers>`) du fichier projet.</span><span class="sxs-lookup"><span data-stu-id="5a17a-164">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="5a17a-165">Fournissez le runtime à l’option [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) de la commande `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="5a17a-165">Supply the runtime to the [-r|--runtime](/dotnet/core/tools/dotnet-publish#options) option of the `dotnet publish` command.</span></span>

<span data-ttu-id="5a17a-166">Dans l’exemple suivant, l’application est publiée pour le runtime `win7-x64` dans le dossier *c:\\svc* :</span><span class="sxs-lookup"><span data-stu-id="5a17a-166">In the following example, the app is published for the `win7-x64` runtime to the *c:\\svc* folder:</span></span>

```console
dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
```

### <a name="create-a-user-account"></a><span data-ttu-id="5a17a-167">Créer un compte d’utilisateur</span><span class="sxs-lookup"><span data-stu-id="5a17a-167">Create a user account</span></span>

<span data-ttu-id="5a17a-168">Créez un compte d’utilisateur pour le service en utilisant la commande `net user` à partir d’un shell de commande d’administration :</span><span class="sxs-lookup"><span data-stu-id="5a17a-168">Create a user account for the service using the `net user` command from an administrative command shell:</span></span>

```console
net user {USER ACCOUNT} {PASSWORD} /add
```

<span data-ttu-id="5a17a-169">L’expiration du mot de passe par défaut est de six semaines.</span><span class="sxs-lookup"><span data-stu-id="5a17a-169">The default password expiration is six weeks.</span></span>

<span data-ttu-id="5a17a-170">Pour l’exemple d’application, créez un compte d’utilisateur avec le nom `ServiceUser` et un mot de passe.</span><span class="sxs-lookup"><span data-stu-id="5a17a-170">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="5a17a-171">Dans la commande suivante, remplacez `{PASSWORD}` par un [mot de passe fort](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span><span class="sxs-lookup"><span data-stu-id="5a17a-171">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

```console
net user ServiceUser {PASSWORD} /add
```

<span data-ttu-id="5a17a-172">Si vous devez ajouter l’utilisateur à un groupe, utilisez la commande `net localgroup`, où `{GROUP}` est le nom du groupe :</span><span class="sxs-lookup"><span data-stu-id="5a17a-172">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

```console
net localgroup {GROUP} {USER ACCOUNT} /add
```

<span data-ttu-id="5a17a-173">Pour plus d’informations, consultez [Comptes d’utilisateur de service](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="5a17a-173">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

<span data-ttu-id="5a17a-174">Une approche alternative à la gestion des utilisateurs lors de l’utilisation d’Active Directory consiste à utiliser des Comptes de service administrés.</span><span class="sxs-lookup"><span data-stu-id="5a17a-174">An alternative approach to managing users when using Active Directory is to use Managed Service Accounts.</span></span> <span data-ttu-id="5a17a-175">Pour plus d’informations, consultez [Vue d’ensemble des comptes de service administrés de groupe](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span><span class="sxs-lookup"><span data-stu-id="5a17a-175">For more information, see [Group Managed Service Accounts Overview](/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview).</span></span>

### <a name="set-permissions"></a><span data-ttu-id="5a17a-176">Définir les autorisations</span><span class="sxs-lookup"><span data-stu-id="5a17a-176">Set permissions</span></span>

#### <a name="access-to-the-app-folder"></a><span data-ttu-id="5a17a-177">Accès au dossier d’application</span><span class="sxs-lookup"><span data-stu-id="5a17a-177">Access to the app folder</span></span>

<span data-ttu-id="5a17a-178">Accordez l’accès en écriture/lecture/exécution au dossier de l’application à l’aide de la commande [icacls](/windows-server/administration/windows-commands/icacls) à partir d’un shell de commande d’administration :</span><span class="sxs-lookup"><span data-stu-id="5a17a-178">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command from an administrative command shell:</span></span>

```console
icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
```

* <span data-ttu-id="5a17a-179">`{PATH}` &ndash; Chemin au dossier de l’application.</span><span class="sxs-lookup"><span data-stu-id="5a17a-179">`{PATH}` &ndash; Path to the app's folder.</span></span>
* <span data-ttu-id="5a17a-180">`{USER ACCOUNT}` &ndash; Compte d’utilisateur (SID).</span><span class="sxs-lookup"><span data-stu-id="5a17a-180">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
* <span data-ttu-id="5a17a-181">`(OI)` &ndash; L’indicateur Object Inherit propage les autorisations aux fichiers subordonnés.</span><span class="sxs-lookup"><span data-stu-id="5a17a-181">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
* <span data-ttu-id="5a17a-182">`(CI)` &ndash; L’indicateur Container Inherit propage les autorisations aux dossiers subordonnés.</span><span class="sxs-lookup"><span data-stu-id="5a17a-182">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
* <span data-ttu-id="5a17a-183">`{PERMISSION FLAGS}` &ndash; Définit les autorisations d’accès de l’application.</span><span class="sxs-lookup"><span data-stu-id="5a17a-183">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
  * <span data-ttu-id="5a17a-184">Écriture (`W`)</span><span class="sxs-lookup"><span data-stu-id="5a17a-184">Write (`W`)</span></span>
  * <span data-ttu-id="5a17a-185">Lecture (`R`)</span><span class="sxs-lookup"><span data-stu-id="5a17a-185">Read (`R`)</span></span>
  * <span data-ttu-id="5a17a-186">Exécution (`X`)</span><span class="sxs-lookup"><span data-stu-id="5a17a-186">Execute (`X`)</span></span>
  * <span data-ttu-id="5a17a-187">Complet (`F`)</span><span class="sxs-lookup"><span data-stu-id="5a17a-187">Full (`F`)</span></span>
  * <span data-ttu-id="5a17a-188">Modification (`M`)</span><span class="sxs-lookup"><span data-stu-id="5a17a-188">Modify (`M`)</span></span>
* <span data-ttu-id="5a17a-189">`/t` &ndash; Appliquer de manière récursive aux dossiers et fichiers subordonnés existants.</span><span class="sxs-lookup"><span data-stu-id="5a17a-189">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

<span data-ttu-id="5a17a-190">Pour l’exemple d’application publiée dans le dossier *c:\\svc* et le compte `ServiceUser` avec des autorisations en écriture/lecture/exécution, utilisez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="5a17a-190">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

```console
icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
```

<span data-ttu-id="5a17a-191">Pour plus d’informations, consultez [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="5a17a-191">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

#### <a name="log-on-as-a-service"></a><span data-ttu-id="5a17a-192">Ouvrir une session en tant que service</span><span class="sxs-lookup"><span data-stu-id="5a17a-192">Log on as a service</span></span>

<span data-ttu-id="5a17a-193">Pour accorder le privilège [Ouvrir une session en tant que service](/windows/security/threat-protection/security-policy-settings/log-on-as-a-service) au compte utilisateur :</span><span class="sxs-lookup"><span data-stu-id="5a17a-193">To grant the [Log on as a service](/windows/security/threat-protection/security-policy-settings/log-on-as-a-service) privilege to the user account:</span></span>

1. <span data-ttu-id="5a17a-194">Recherchez les stratégies **Attribution des droits utilisateur** dans la console Stratégie de sécurité locale ou la console Éditeur de stratégie de groupe locale.</span><span class="sxs-lookup"><span data-stu-id="5a17a-194">Locate the **User Rights Assignment** policies in either the Local Security Policy console or Local Group Policy Editor console.</span></span> <span data-ttu-id="5a17a-195">Pour obtenir des instructions, voir : [Configurer les paramètres de stratégie de sécurité](/windows/security/threat-protection/security-policy-settings/how-to-configure-security-policy-settings).</span><span class="sxs-lookup"><span data-stu-id="5a17a-195">For instructions, see: [Configure security policy settings](/windows/security/threat-protection/security-policy-settings/how-to-configure-security-policy-settings).</span></span>
1. <span data-ttu-id="5a17a-196">Recherchez la stratégie `Log on as a service`.</span><span class="sxs-lookup"><span data-stu-id="5a17a-196">Locate the `Log on as a service` policy.</span></span> <span data-ttu-id="5a17a-197">Double-cliquez sur la stratégie pour l’ouvrir.</span><span class="sxs-lookup"><span data-stu-id="5a17a-197">Double-click the policy to open it.</span></span>
1. <span data-ttu-id="5a17a-198">Sélectionnez **Ajouter un utilisateur ou un groupe**.</span><span class="sxs-lookup"><span data-stu-id="5a17a-198">Select **Add User or Group**.</span></span>
1. <span data-ttu-id="5a17a-199">Sélectionnez **Avancé** et **Rechercher maintenant**.</span><span class="sxs-lookup"><span data-stu-id="5a17a-199">Select **Advanced** and select **Find Now**.</span></span>
1. <span data-ttu-id="5a17a-200">Sélectionnez le compte utilisateur créé précédemment dans la section [Créer un compte utilisateur](#create-a-user-account).</span><span class="sxs-lookup"><span data-stu-id="5a17a-200">Select the user account created in the [Create a user account](#create-a-user-account) section earlier.</span></span> <span data-ttu-id="5a17a-201">Sélectionnez **OK** pour accepter la sélection.</span><span class="sxs-lookup"><span data-stu-id="5a17a-201">Select **OK** to accept the selection.</span></span>
1. <span data-ttu-id="5a17a-202">Sélectionnez **OK** après avoir confirmé que le nom d’objet est correct.</span><span class="sxs-lookup"><span data-stu-id="5a17a-202">Select **OK** after confirming that the object name is correct.</span></span>
1. <span data-ttu-id="5a17a-203">Sélectionnez **Appliquer**.</span><span class="sxs-lookup"><span data-stu-id="5a17a-203">Select **Apply**.</span></span> <span data-ttu-id="5a17a-204">Sélectionnez **OK** pour fermer la fenêtre de la stratégie.</span><span class="sxs-lookup"><span data-stu-id="5a17a-204">Select **OK** to close the policy window.</span></span>

## <a name="manage-the-service"></a><span data-ttu-id="5a17a-205">Gérer le service</span><span class="sxs-lookup"><span data-stu-id="5a17a-205">Manage the service</span></span>

### <a name="create-the-service"></a><span data-ttu-id="5a17a-206">Créer le service</span><span class="sxs-lookup"><span data-stu-id="5a17a-206">Create the service</span></span>

<span data-ttu-id="5a17a-207">Utilisez l’outil de ligne de commande [sc.exe](https://technet.microsoft.com/library/bb490995) pour créer le service à partir d’un shell de commande d’administration.</span><span class="sxs-lookup"><span data-stu-id="5a17a-207">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service from an administrative command shell.</span></span> <span data-ttu-id="5a17a-208">La valeur `binPath` est le chemin du fichier exécutable de l’application, qui inclut le nom du fichier exécutable.</span><span class="sxs-lookup"><span data-stu-id="5a17a-208">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="5a17a-209">**L’espace entre le signe égal à la fin du paramètre et le guillemet au début de la valeur est obligatoire.**</span><span class="sxs-lookup"><span data-stu-id="5a17a-209">**The space between the equal sign and the quote character of each parameter and value is required.**</span></span>

```console
sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
```

* <span data-ttu-id="5a17a-210">`{SERVICE NAME}` &ndash; Nom à attribuer au service dans le [Gestionnaire de contrôle des services](/windows/desktop/services/service-control-manager).</span><span class="sxs-lookup"><span data-stu-id="5a17a-210">`{SERVICE NAME}` &ndash; The name to assign to the service in [Service Control Manager](/windows/desktop/services/service-control-manager).</span></span>
* <span data-ttu-id="5a17a-211">`{PATH}` &ndash; Chemin à l’exécutable du service.</span><span class="sxs-lookup"><span data-stu-id="5a17a-211">`{PATH}` &ndash; The path to the service executable.</span></span>
* <span data-ttu-id="5a17a-212">`{DOMAIN}` &ndash; Domaine d’un ordinateur joint au domaine.</span><span class="sxs-lookup"><span data-stu-id="5a17a-212">`{DOMAIN}` &ndash; The domain of a domain-joined machine.</span></span> <span data-ttu-id="5a17a-213">Si l’ordinateur n’est pas joint au domaine, utilisez le nom de l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="5a17a-213">If the machine isn't domain-joined, use the local machine name.</span></span>
* <span data-ttu-id="5a17a-214">`{USER ACCOUNT}` &ndash; Compte d’utilisateur sous lequel le service s’exécute.</span><span class="sxs-lookup"><span data-stu-id="5a17a-214">`{USER ACCOUNT}` &ndash; The user account under which the service runs.</span></span>
* <span data-ttu-id="5a17a-215">`{PASSWORD}` &ndash; Mot de passe du compte d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="5a17a-215">`{PASSWORD}` &ndash; The user account password.</span></span>

> [!WARNING]
> <span data-ttu-id="5a17a-216">**N’oubliez pas** le paramètre `obj`.</span><span class="sxs-lookup"><span data-stu-id="5a17a-216">Do **not** omit the `obj` parameter.</span></span> <span data-ttu-id="5a17a-217">`obj` a pour valeur par défaut le compte [LocalSystem](/windows/desktop/services/localsystem-account).</span><span class="sxs-lookup"><span data-stu-id="5a17a-217">The default value for `obj` is the [LocalSystem account](/windows/desktop/services/localsystem-account) account.</span></span> <span data-ttu-id="5a17a-218">L’exécution d’un service sous le compte `LocalSystem` présente un risque de sécurité important.</span><span class="sxs-lookup"><span data-stu-id="5a17a-218">Running a service under the `LocalSystem` account presents a significant security risk.</span></span> <span data-ttu-id="5a17a-219">Veillez à toujours exécuter un service sous un compte d’utilisateur disposant de privilèges limités.</span><span class="sxs-lookup"><span data-stu-id="5a17a-219">Always run a service with a user account that has restricted privileges.</span></span>

<span data-ttu-id="5a17a-220">Dans l’exemple suivant pour l’exemple d’application :</span><span class="sxs-lookup"><span data-stu-id="5a17a-220">In the following example for the sample app:</span></span>

* <span data-ttu-id="5a17a-221">Le service s’appelle **MyService**.</span><span class="sxs-lookup"><span data-stu-id="5a17a-221">The service is named **MyService**.</span></span>
* <span data-ttu-id="5a17a-222">Le service publié réside dans le dossier *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="5a17a-222">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="5a17a-223">L’application exécutable s’appelle *SampleApp.exe*.</span><span class="sxs-lookup"><span data-stu-id="5a17a-223">The app executable is named *SampleApp.exe*.</span></span> <span data-ttu-id="5a17a-224">Mettez la valeur `binPath` entre guillemets doubles (").</span><span class="sxs-lookup"><span data-stu-id="5a17a-224">Enclose the `binPath` value in double quotation marks (").</span></span>
* <span data-ttu-id="5a17a-225">Le service s’exécute sous le compte `ServiceUser`.</span><span class="sxs-lookup"><span data-stu-id="5a17a-225">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="5a17a-226">Remplacez `{DOMAIN}` par le nom de l’ordinateur local ou le domaine du compte d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="5a17a-226">Replace `{DOMAIN}` with the user account's domain or local machine name.</span></span> <span data-ttu-id="5a17a-227">Mettez la valeur `obj` entre guillemets doubles (").</span><span class="sxs-lookup"><span data-stu-id="5a17a-227">Enclose the `obj` value in double quotation marks (").</span></span> <span data-ttu-id="5a17a-228">Exemple : si le système d’hébergement est un ordinateur local nommé `MairaPC`, définissez `obj` avec la valeur `"MairaPC\ServiceUser"`.</span><span class="sxs-lookup"><span data-stu-id="5a17a-228">Example: If the hosting system is a local machine named `MairaPC`, set `obj` to `"MairaPC\ServiceUser"`.</span></span>
* <span data-ttu-id="5a17a-229">Remplacez `{PASSWORD}` par le mot de passe du compte d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="5a17a-229">Replace `{PASSWORD}` with the user account's password.</span></span> <span data-ttu-id="5a17a-230">Mettez la valeur `password` entre guillemets doubles (").</span><span class="sxs-lookup"><span data-stu-id="5a17a-230">Enclose the `password` value in double quotation marks (").</span></span>

```console
sc create MyService binPath= "c:\svc\sampleapp.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
```

> [!IMPORTANT]
> <span data-ttu-id="5a17a-231">Vérifiez la présence d’espaces entre les signes égal et les valeurs des paramètres.</span><span class="sxs-lookup"><span data-stu-id="5a17a-231">Make sure that the spaces between the parameters' equal signs and the parameters' values are present.</span></span>

### <a name="start-the-service"></a><span data-ttu-id="5a17a-232">Démarrer le service</span><span class="sxs-lookup"><span data-stu-id="5a17a-232">Start the service</span></span>

<span data-ttu-id="5a17a-233">Démarrez le service avec la commande `sc start {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="5a17a-233">Start the service with the `sc start {SERVICE NAME}` command.</span></span>

<span data-ttu-id="5a17a-234">Pour démarrer l’exemple de service d’application, utilisez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="5a17a-234">To start the sample app service, use the following command:</span></span>

```console
sc start MyService
```

<span data-ttu-id="5a17a-235">La commande prend quelques secondes pour démarrer le service.</span><span class="sxs-lookup"><span data-stu-id="5a17a-235">The command takes a few seconds to start the service.</span></span>

### <a name="determine-the-service-status"></a><span data-ttu-id="5a17a-236">Déterminer l’état du service</span><span class="sxs-lookup"><span data-stu-id="5a17a-236">Determine the service status</span></span>

<span data-ttu-id="5a17a-237">Pour vérifier l’état du service, utilisez la commande `sc query {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="5a17a-237">To check the status of the service, use the `sc query {SERVICE NAME}` command.</span></span> <span data-ttu-id="5a17a-238">L’état est signalé comme étant l’une des valeurs suivantes :</span><span class="sxs-lookup"><span data-stu-id="5a17a-238">The status is reported as one of the following values:</span></span>

* `START_PENDING`
* `RUNNING`
* `STOP_PENDING`
* `STOPPED`

<span data-ttu-id="5a17a-239">Utilisez la commande suivante pour vérifier l’état de l’exemple de service d’application :</span><span class="sxs-lookup"><span data-stu-id="5a17a-239">Use the following command to check the status of the sample app service:</span></span>

```console
sc query MyService
```

### <a name="browse-a-web-app-service"></a><span data-ttu-id="5a17a-240">Parcourir un service d’application web</span><span class="sxs-lookup"><span data-stu-id="5a17a-240">Browse a web app service</span></span>

<span data-ttu-id="5a17a-241">Quand le service est une application web et que son état est `RUNNING`, accédez au chemin de l'application (par défaut, `http://localhost:5000`, qui redirige vers `https://localhost:5001` si le [middleware (intergiciel) de redirection HTTPS](xref:security/enforcing-ssl) est utilisé).</span><span class="sxs-lookup"><span data-stu-id="5a17a-241">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

<span data-ttu-id="5a17a-242">Pour l’exemple de service d’application, accédez au chemin de l'application sur `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="5a17a-242">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

### <a name="stop-the-service"></a><span data-ttu-id="5a17a-243">Arrêter le service</span><span class="sxs-lookup"><span data-stu-id="5a17a-243">Stop the service</span></span>

<span data-ttu-id="5a17a-244">Arrêtez le service avec la commande `sc stop {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="5a17a-244">Stop the service with the `sc stop {SERVICE NAME}` command.</span></span>

<span data-ttu-id="5a17a-245">La commande suivante arrête l’exemple de service d’application :</span><span class="sxs-lookup"><span data-stu-id="5a17a-245">The following command stops the sample app service:</span></span>

```console
sc stop MyService
```

### <a name="delete-the-service"></a><span data-ttu-id="5a17a-246">Supprimer le service</span><span class="sxs-lookup"><span data-stu-id="5a17a-246">Delete the service</span></span>

<span data-ttu-id="5a17a-247">Après un court délai pour arrêter un service, désinstallez le service avec la commande `sc delete {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="5a17a-247">After a short delay to stop a service, uninstall the service with the `sc delete {SERVICE NAME}` command.</span></span>

<span data-ttu-id="5a17a-248">Vérifiez l’état de l’exemple de service d’application :</span><span class="sxs-lookup"><span data-stu-id="5a17a-248">Check the status of the sample app service:</span></span>

```console
sc query MyService
```

<span data-ttu-id="5a17a-249">Quand l’exemple de service d’application est dans l’état `STOPPED`, utilisez la commande suivante pour le désinstaller :</span><span class="sxs-lookup"><span data-stu-id="5a17a-249">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

```console
sc delete MyService
```

## <a name="handle-starting-and-stopping-events"></a><span data-ttu-id="5a17a-250">Gérer les événements de démarrage et d’arrêt</span><span class="sxs-lookup"><span data-stu-id="5a17a-250">Handle starting and stopping events</span></span>

<span data-ttu-id="5a17a-251">Pour gérer les événements <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*> et <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*>, apportez les modifications supplémentaires suivantes :</span><span class="sxs-lookup"><span data-stu-id="5a17a-251">To handle <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarting*>, <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStarted*>, and <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService.OnStopping*> events, perform the following additional changes:</span></span>

1. <span data-ttu-id="5a17a-252">Créez une classe qui dérive de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> à l’aide des méthodes `OnStarting`, `OnStarted` et `OnStopping` :</span><span class="sxs-lookup"><span data-stu-id="5a17a-252">Create a class that derives from <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostService> with the `OnStarting`, `OnStarted`, and `OnStopping` methods:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=snippet_CustomWebHostService)]

2. <span data-ttu-id="5a17a-253">Créez une méthode d’extension pour <xref:Microsoft.AspNetCore.Hosting.IWebHost> qui transmet `CustomWebHostService` à <xref:System.ServiceProcess.ServiceBase.Run*> :</span><span class="sxs-lookup"><span data-stu-id="5a17a-253">Create an extension method for <xref:Microsoft.AspNetCore.Hosting.IWebHost> that passes the `CustomWebHostService` to <xref:System.ServiceProcess.ServiceBase.Run*>:</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="5a17a-254">Dans `Program.Main`, appelez la méthode d’extension `RunAsCustomService` au lieu de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> :</span><span class="sxs-lookup"><span data-stu-id="5a17a-254">In `Program.Main`, call the `RunAsCustomService` extension method instead of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*>:</span></span>

   ```csharp
   host.RunAsCustomService();
   ```

   <span data-ttu-id="5a17a-255">Pour afficher l’emplacement de <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> dans `Program.Main`, reportez-vous à l’exemple de code indiqué dans la section [Convertir un projet en service Windows](#convert-a-project-into-a-windows-service).</span><span class="sxs-lookup"><span data-stu-id="5a17a-255">To see the location of <xref:Microsoft.AspNetCore.Hosting.WindowsServices.WebHostWindowsServiceExtensions.RunAsService*> in `Program.Main`, refer to the code sample shown in the [Convert a project into a Windows Service](#convert-a-project-into-a-windows-service) section.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="5a17a-256">Scénarios avec un serveur proxy et un équilibreur de charge</span><span class="sxs-lookup"><span data-stu-id="5a17a-256">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="5a17a-257">Les services qui interagissent avec les requêtes provenant d’Internet ou d’un réseau d’entreprise et qui se trouvent derrière un proxy ou équilibreur de charge peuvent nécessiter une configuration supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="5a17a-257">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="5a17a-258">Pour plus d'informations, consultez <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="5a17a-258">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="5a17a-259">Configurer HTTPS</span><span class="sxs-lookup"><span data-stu-id="5a17a-259">Configure HTTPS</span></span>

<span data-ttu-id="5a17a-260">Pour configurer le service avec un point de terminaison sécurisé :</span><span class="sxs-lookup"><span data-stu-id="5a17a-260">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="5a17a-261">Créez un certificat X.509 pour le système d’hébergement à l’aide des mécanismes d’acquisition et de déploiement de certificat de votre plateforme.</span><span class="sxs-lookup"><span data-stu-id="5a17a-261">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>

1. <span data-ttu-id="5a17a-262">Spécifiez la [configuration de point de terminaison HTTPS d’un serveur Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) pour utiliser le certificat.</span><span class="sxs-lookup"><span data-stu-id="5a17a-262">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="5a17a-263">L’utilisation du certificat de développement HTTPS ASP.NET Core pour sécuriser un point de terminaison de service n’est pas prise en charge.</span><span class="sxs-lookup"><span data-stu-id="5a17a-263">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="5a17a-264">Répertoire actif et racine du contenu</span><span class="sxs-lookup"><span data-stu-id="5a17a-264">Current directory and content root</span></span>

<span data-ttu-id="5a17a-265">Le répertoire de travail actif retourné par l’appel à <xref:System.IO.Directory.GetCurrentDirectory*> pour un service Windows est le dossier *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="5a17a-265">The current working directory returned by calling <xref:System.IO.Directory.GetCurrentDirectory*> for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="5a17a-266">Le dossier *system32* n’est pas un emplacement approprié pour stocker les fichiers d’un service (tels que les fichiers de paramètres).</span><span class="sxs-lookup"><span data-stu-id="5a17a-266">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="5a17a-267">Utilisez une des approches suivantes pour gérer les ressources ainsi que les fichiers de paramètres d’un service, et y accéder.</span><span class="sxs-lookup"><span data-stu-id="5a17a-267">Use one of the following approaches to maintain and access a service's assets and settings files.</span></span>

### <a name="set-the-content-root-path-to-the-apps-folder"></a><span data-ttu-id="5a17a-268">Définir le dossier de l’application comme chemin d’accès racine du contenu</span><span class="sxs-lookup"><span data-stu-id="5a17a-268">Set the content root path to the app's folder</span></span>

<span data-ttu-id="5a17a-269"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> est le même chemin que celui fourni à l’argument `binPath` quand le service est créé.</span><span class="sxs-lookup"><span data-stu-id="5a17a-269">The <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootPath*> is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="5a17a-270">Au lieu d’appeler `GetCurrentDirectory` pour créer des chemins d’accès aux fichiers de paramètres, appelez <xref:System.IO.Directory.SetCurrentDirectory*> en utilisant le chemin d’accès à la racine du contenu de l’application.</span><span class="sxs-lookup"><span data-stu-id="5a17a-270">Instead of calling `GetCurrentDirectory` to create paths to settings files, call <xref:System.IO.Directory.SetCurrentDirectory*> with the path to the app's content root.</span></span>

<span data-ttu-id="5a17a-271">Dans `Program.Main`, définissez le chemin d’accès au dossier du fichier exécutable du service ainsi que le chemin d’accès pour établir la racine du contenu de l’application :</span><span class="sxs-lookup"><span data-stu-id="5a17a-271">In `Program.Main`, determine the path to the folder of the service's executable and use the path to establish the app's content root:</span></span>

```csharp
var pathToExe = Process.GetCurrentProcess().MainModule.FileName;
var pathToContentRoot = Path.GetDirectoryName(pathToExe);
Directory.SetCurrentDirectory(pathToContentRoot);

CreateWebHostBuilder(args)
    .Build()
    .RunAsService();
```

### <a name="store-the-services-files-in-a-suitable-location-on-disk"></a><span data-ttu-id="5a17a-272">Stocker les fichiers du service dans un emplacement approprié sur le disque</span><span class="sxs-lookup"><span data-stu-id="5a17a-272">Store the service's files in a suitable location on disk</span></span>

<span data-ttu-id="5a17a-273">Spécifiez un chemin absolu avec <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>, si vous utilisez <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>, vers le dossier contenant les fichiers.</span><span class="sxs-lookup"><span data-stu-id="5a17a-273">Specify an absolute path with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*> when using an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5a17a-274">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="5a17a-274">Additional resources</span></span>

* <span data-ttu-id="5a17a-275">[Configuration de point de terminaison Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) (inclut la configuration de HTTPS et la prise en charge de SNI)</span><span class="sxs-lookup"><span data-stu-id="5a17a-275">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
* <xref:test/troubleshoot>
