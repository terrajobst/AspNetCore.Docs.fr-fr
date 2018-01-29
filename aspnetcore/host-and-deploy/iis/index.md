---
title: "Héberger ASP.NET Core sur Windows avec IIS"
author: guardrex
description: "Découvrez comment héberger des applications ASP.NET Core sur Windows Server Internet Information Services (IIS)."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 03/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/iis/index
ms.openlocfilehash: 18c7448ad79891d04eca1e939a0aeeabe417bde8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="host-aspnet-core-on-windows-with-iis"></a><span data-ttu-id="81b5f-103">Héberger ASP.NET Core sur Windows avec IIS</span><span class="sxs-lookup"><span data-stu-id="81b5f-103">Host ASP.NET Core on Windows with IIS</span></span>

<span data-ttu-id="81b5f-104">Par [Luke Latham](https://github.com/guardrex) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="81b5f-104">By [Luke Latham](https://github.com/guardrex) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="supported-operating-systems"></a><span data-ttu-id="81b5f-105">Systèmes d'exploitation pris en charge</span><span class="sxs-lookup"><span data-stu-id="81b5f-105">Supported operating systems</span></span>

<span data-ttu-id="81b5f-106">Les systèmes d’exploitation suivants sont pris en charge :</span><span class="sxs-lookup"><span data-stu-id="81b5f-106">The following operating systems are supported:</span></span>

* <span data-ttu-id="81b5f-107">Windows 7 et ultérieur</span><span class="sxs-lookup"><span data-stu-id="81b5f-107">Windows 7 and newer</span></span>
* <span data-ttu-id="81b5f-108">Windows Server 2008 R2 et ultérieur&#8224;</span><span class="sxs-lookup"><span data-stu-id="81b5f-108">Windows Server 2008 R2 and newer&#8224;</span></span>

<span data-ttu-id="81b5f-109">&#8224;D’un point de vue conceptuel, la configuration d’IIS décrite dans ce document s’applique également à l’hébergement d’applications ASP.NET Core sur Nano Server IIS, mais pour obtenir des instructions spécifiques, consultez [ASP.NET Core avec IIS sur Nano Server](xref:tutorials/nano-server).</span><span class="sxs-lookup"><span data-stu-id="81b5f-109">&#8224;Conceptually, the IIS configuration described in this document also applies to hosting ASP.NET Core apps on Nano Server IIS, but refer to [ASP.NET Core with IIS on Nano Server](xref:tutorials/nano-server) for specific instructions.</span></span>

<span data-ttu-id="81b5f-110">Le [serveur HTTP.sys](xref:fundamentals/servers/httpsys) (anciennement [WebListener](xref:fundamentals/servers/weblistener)) ne fonctionne pas dans une configuration de proxy inverse avec IIS.</span><span class="sxs-lookup"><span data-stu-id="81b5f-110">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)) won't work in a reverse proxy configuration with IIS.</span></span> <span data-ttu-id="81b5f-111">Utilisez le [serveur Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="81b5f-111">Use the [Kestrel server](xref:fundamentals/servers/kestrel).</span></span>

## <a name="iis-configuration"></a><span data-ttu-id="81b5f-112">Configuration d’IIS</span><span class="sxs-lookup"><span data-stu-id="81b5f-112">IIS configuration</span></span>

<span data-ttu-id="81b5f-113">Activez le rôle **Serveur Web (IIS)** et établissez des services de rôle.</span><span class="sxs-lookup"><span data-stu-id="81b5f-113">Enable the **Web Server (IIS)** role and establish role services.</span></span>

### <a name="windows-desktop-operating-systems"></a><span data-ttu-id="81b5f-114">Systèmes d’exploitation de bureau Windows</span><span class="sxs-lookup"><span data-stu-id="81b5f-114">Windows desktop operating systems</span></span>

<span data-ttu-id="81b5f-115">Accédez à **Panneau de configuration** > **Programmes** > **Programmes et fonctionnalités** > **Activer ou désactiver des fonctionnalités Windows** (à gauche de l’écran).</span><span class="sxs-lookup"><span data-stu-id="81b5f-115">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span> <span data-ttu-id="81b5f-116">Ouvrez le groupe **Internet Information Services** et **Outils d’administration Web**.</span><span class="sxs-lookup"><span data-stu-id="81b5f-116">Open the group for **Internet Information Services** and **Web Management Tools**.</span></span> <span data-ttu-id="81b5f-117">Cochez la case **Console de gestion IIS**.</span><span class="sxs-lookup"><span data-stu-id="81b5f-117">Check the box for **IIS Management Console**.</span></span> <span data-ttu-id="81b5f-118">Cochez la case **Services World Wide Web**.</span><span class="sxs-lookup"><span data-stu-id="81b5f-118">Check the box for **World Wide Web Services**.</span></span> <span data-ttu-id="81b5f-119">Acceptez les fonctionnalités par défaut pour **Services World Wide Web** ou personnalisez les fonctionnalités d’IIS.</span><span class="sxs-lookup"><span data-stu-id="81b5f-119">Accept the default features for **World Wide Web Services** or customize the IIS features.</span></span>

![Console de gestion IIS et Services World Wide Web sont sélectionnés dans Fonctionnalités de Windows.](index/_static/windows-features-win10.png)

### <a name="windows-server-operating-systems"></a><span data-ttu-id="81b5f-121">Systèmes d’exploitation Windows Server</span><span class="sxs-lookup"><span data-stu-id="81b5f-121">Windows Server operating systems</span></span>

<span data-ttu-id="81b5f-122">Pour les systèmes d’exploitation de serveur, utilisez l’Assistant **Ajout de rôles et de fonctionnalités** par le biais du menu **Gérer** ou du lien dans **Gestionnaire de serveur**.</span><span class="sxs-lookup"><span data-stu-id="81b5f-122">For server operating systems, use the **Add Roles and Features** wizard via the **Manage** menu or the link in **Server Manager**.</span></span> <span data-ttu-id="81b5f-123">À l’étape **Rôles de serveurs**, cochez la case **Serveur Web (IIS)**.</span><span class="sxs-lookup"><span data-stu-id="81b5f-123">On the **Server Roles** step, check the box for **Web Server (IIS)**.</span></span>

![Le rôle Serveur Web IIS est sélectionné à l’étape Sélectionner des rôles de serveurs.](index/_static/server-roles-ws2016.png)

<span data-ttu-id="81b5f-125">À l’étape **Services de rôle**, sélectionnez les services de rôle IIS souhaités ou acceptez les services de rôle par défaut.</span><span class="sxs-lookup"><span data-stu-id="81b5f-125">On the **Role services** step, select the IIS role services desired or accept the default role services provided.</span></span>

![Les services de rôle par défaut sont sélectionnés à l’étape Sélectionner des services de rôle.](index/_static/role-services-ws2016.png)

<span data-ttu-id="81b5f-127">Validez l’étape de **Confirmation** pour installer les services et le rôle de serveur web.</span><span class="sxs-lookup"><span data-stu-id="81b5f-127">Proceed through the **Confirmation** step to install the web server role and services.</span></span> <span data-ttu-id="81b5f-128">Un redémarrage du serveur/d’IIS n’est pas nécessaire après l’installation du rôle Serveur Web (IIS).</span><span class="sxs-lookup"><span data-stu-id="81b5f-128">A server/IIS restart isn't required after installing the Web Server (IIS) role.</span></span>

## <a name="install-the-net-core-windows-server-hosting-bundle"></a><span data-ttu-id="81b5f-129">Installer le bundle d’hébergement .NET Core Windows Server</span><span class="sxs-lookup"><span data-stu-id="81b5f-129">Install the .NET Core Windows Server Hosting bundle</span></span>

1. <span data-ttu-id="81b5f-130">Installez le [bundle d’hébergement .NET Core Windows Server](https://aka.ms/dotnetcore-2-windowshosting) sur le système hôte.</span><span class="sxs-lookup"><span data-stu-id="81b5f-130">Install the [.NET Core Windows Server Hosting bundle](https://aka.ms/dotnetcore-2-windowshosting) on the hosting system.</span></span> <span data-ttu-id="81b5f-131">Le bundle installe le Runtime .NET Core, la bibliothèque .NET Core et le [Module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="81b5f-131">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="81b5f-132">Le module crée le proxy inverse entre IIS et le serveur Kestrel.</span><span class="sxs-lookup"><span data-stu-id="81b5f-132">The module creates the reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="81b5f-133">Si le système n’a pas de connexion Internet, obtenez et installez [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) avant d’installer le bundle d’hébergement .NET Core Windows Server.</span><span class="sxs-lookup"><span data-stu-id="81b5f-133">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Windows Server Hosting bundle.</span></span>

2. <span data-ttu-id="81b5f-134">Redémarrez le système ou exécutez **net stop was /y** suivi de **net start w3svc** à partir d’une invite de commandes pour prendre en compte une modification de la variable système PATH.</span><span class="sxs-lookup"><span data-stu-id="81b5f-134">Restart the system or execute **net stop was /y** followed by **net start w3svc** from a command prompt to pick up a change to the system PATH.</span></span>

> [!NOTE]
> <span data-ttu-id="81b5f-135">Pour plus d’informations sur la configuration partagée IIS, consultez [Module ASP.NET Core avec configuration partagée des services Internet (IIS)](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span><span class="sxs-lookup"><span data-stu-id="81b5f-135">For information on IIS Shared Configuration, see [ASP.NET Core Module with IIS Shared Configuration](xref:host-and-deploy/aspnet-core-module#aspnet-core-module-with-an-iis-shared-configuration).</span></span>

## <a name="install-web-deploy-when-publishing-with-visual-studio"></a><span data-ttu-id="81b5f-136">Installer Web Deploy lors de la publication avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="81b5f-136">Install Web Deploy when publishing with Visual Studio</span></span>

<span data-ttu-id="81b5f-137">Quand vous déployez des applications sur un serveur avec [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), installez la dernière version de Web Deploy sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="81b5f-137">When deploying apps to servers with [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy), install the latest version of Web Deploy on the server.</span></span> <span data-ttu-id="81b5f-138">Pour installer Web Deploy, utilisez [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) ou obtenez un programme d’installation directement à partir du [Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=43717).</span><span class="sxs-lookup"><span data-stu-id="81b5f-138">To install Web Deploy, use the [Web Platform Installer (WebPI)](https://www.microsoft.com/web/downloads/platform.aspx) or obtain an installer directly from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=43717).</span></span> <span data-ttu-id="81b5f-139">La méthode recommandée est d’utiliser WebPI.</span><span class="sxs-lookup"><span data-stu-id="81b5f-139">The preferred method is to use WebPI.</span></span> <span data-ttu-id="81b5f-140">WebPI offre une installation autonome et une configuration pour les fournisseurs d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="81b5f-140">WebPI offers a standalone setup and a configuration for hosting providers.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="81b5f-141">Configuration d’application</span><span class="sxs-lookup"><span data-stu-id="81b5f-141">Application configuration</span></span>

### <a name="enabling-the-iisintegration-components"></a><span data-ttu-id="81b5f-142">Activation des composants IISIntegration</span><span class="sxs-lookup"><span data-stu-id="81b5f-142">Enabling the IISIntegration components</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="81b5f-143">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="81b5f-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="81b5f-144">Un fichier *Program.cs* standard appelle [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) pour commencer à configurer un hôte.</span><span class="sxs-lookup"><span data-stu-id="81b5f-144">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to begin setting up a host.</span></span> <span data-ttu-id="81b5f-145">`CreateDefaultBuilder` configure [Kestrel](xref:fundamentals/servers/kestrel) comme serveur web et active l’intégration d’IIS en configurant le chemin et le port de base pour le [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) :</span><span class="sxs-lookup"><span data-stu-id="81b5f-145">`CreateDefaultBuilder` configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and enables IIS integration by configuring the base path and port for the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="81b5f-146">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="81b5f-146">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="81b5f-147">Incluez une dépendance envers le package [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) dans les dépendances de l’application.</span><span class="sxs-lookup"><span data-stu-id="81b5f-147">Include a dependency on the [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/) package in the app's dependencies.</span></span> <span data-ttu-id="81b5f-148">Incorporez l’intergiciel (middleware) d’intégration IIS dans l’application en ajoutant la méthode d’extension *UseIISIntegration* à *WebHostBuilder* :</span><span class="sxs-lookup"><span data-stu-id="81b5f-148">Incorporate IIS Integration middleware into the app by adding the *UseIISIntegration* extension method to *WebHostBuilder*:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .UseIISIntegration()
    ...
```

<span data-ttu-id="81b5f-149">`UseKestrel` et `UseIISIntegration` sont tous deux obligatoires.</span><span class="sxs-lookup"><span data-stu-id="81b5f-149">Both `UseKestrel` and `UseIISIntegration` are required.</span></span> <span data-ttu-id="81b5f-150">Le code qui appelle `UseIISIntegration` n’affecte pas la portabilité du code.</span><span class="sxs-lookup"><span data-stu-id="81b5f-150">Code calling `UseIISIntegration` doesn't affect code portability.</span></span> <span data-ttu-id="81b5f-151">Si l’application n’est pas exécutée derrière IIS (par exemple si l’application est exécutée directement sur Kestrel), `UseIISIntegration` n’effectue pas d’opérations.</span><span class="sxs-lookup"><span data-stu-id="81b5f-151">If the app isn't run behind IIS (for example, the app is run directly on Kestrel), `UseIISIntegration` no-ops.</span></span>

---

<span data-ttu-id="81b5f-152">Pour plus d’informations sur l’hébergement, consultez [Hébergement dans ASP.NET Core](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="81b5f-152">For more information on hosting, see [Hosting in ASP.NET Core](xref:fundamentals/hosting).</span></span>

### <a name="iis-options"></a><span data-ttu-id="81b5f-153">Options IIS</span><span class="sxs-lookup"><span data-stu-id="81b5f-153">IIS options</span></span>

<span data-ttu-id="81b5f-154">Pour configurer les options IIS, incluez une configuration de service pour `IISOptions` dans `ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="81b5f-154">To configure IIS options, include a service configuration for `IISOptions` in `ConfigureServices`:</span></span>

```csharp
services.Configure<IISOptions>(options => 
{
    ...
});
```

| <span data-ttu-id="81b5f-155">Option</span><span class="sxs-lookup"><span data-stu-id="81b5f-155">Option</span></span>                         | <span data-ttu-id="81b5f-156">Par défaut</span><span class="sxs-lookup"><span data-stu-id="81b5f-156">Default</span></span> | <span data-ttu-id="81b5f-157">Paramètre</span><span class="sxs-lookup"><span data-stu-id="81b5f-157">Setting</span></span> |
| ------------------------------ | ------- | ------- |
| `AutomaticAuthentication`      | `true`  | <span data-ttu-id="81b5f-158">Si `true`, l’intergiciel (middleware) d’authentification définit `HttpContext.User` et répond aux questions génériques.</span><span class="sxs-lookup"><span data-stu-id="81b5f-158">If `true`, the authentication middleware sets the `HttpContext.User` and responds to generic challenges.</span></span> <span data-ttu-id="81b5f-159">Si `false`, l’intergiciel (middleware) d’authentification fournit uniquement une identité (`HttpContext.User`) et répond aux questions explicitement posées par `AuthenticationScheme`.</span><span class="sxs-lookup"><span data-stu-id="81b5f-159">If `false`, the authentication middleware only provides an identity (`HttpContext.User`) and responds to challenges when explicitly requested by the `AuthenticationScheme`.</span></span> <span data-ttu-id="81b5f-160">L’authentification Windows doit être activée dans IIS pour que `AutomaticAuthentication` fonctionne.</span><span class="sxs-lookup"><span data-stu-id="81b5f-160">Windows Authentication must be enabled in IIS for `AutomaticAuthentication` to function.</span></span> |
| `AuthenticationDisplayName`    | `null`  | <span data-ttu-id="81b5f-161">Définit le nom d’affichage montré aux utilisateurs sur les pages de connexion.</span><span class="sxs-lookup"><span data-stu-id="81b5f-161">Sets the display name shown to users on login pages.</span></span> |
| `ForwardClientCertificate`     | `true`  | <span data-ttu-id="81b5f-162">Si la valeur est `true` et que l’en-tête de requête `MS-ASPNETCORE-CLIENTCERT` est présent, `HttpContext.Connection.ClientCertificate` est renseigné.</span><span class="sxs-lookup"><span data-stu-id="81b5f-162">If `true` and the `MS-ASPNETCORE-CLIENTCERT` request header is present, the `HttpContext.Connection.ClientCertificate` is populated.</span></span> |

### <a name="webconfig"></a><span data-ttu-id="81b5f-163">web.config</span><span class="sxs-lookup"><span data-stu-id="81b5f-163">web.config</span></span>

<span data-ttu-id="81b5f-164">Le fichier *web.config* sert principalement à configurer le [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="81b5f-164">The *web.config* file's primary purpose is to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="81b5f-165">Il peut éventuellement fournir d’autres paramètres de configuration IIS.</span><span class="sxs-lookup"><span data-stu-id="81b5f-165">It may optionally provide additional IIS configuration settings.</span></span> <span data-ttu-id="81b5f-166">La création, la transformation et la publication de *web.config* sont gérées par le Kit de développement logiciel (SDK) web .NET Core (`Microsoft.NET.Sdk.Web`).</span><span class="sxs-lookup"><span data-stu-id="81b5f-166">Creating, transforming, and publishing *web.config* is handled by the .NET Core Web SDK (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="81b5f-167">Celui-ci est défini en haut du fichier projet, `<Project Sdk="Microsoft.NET.Sdk.Web">`.</span><span class="sxs-lookup"><span data-stu-id="81b5f-167">The SDK is set  at the top of the project file, `<Project Sdk="Microsoft.NET.Sdk.Web">`.</span></span> <span data-ttu-id="81b5f-168">Pour empêcher le Kit SDK de transformer le fichier *web.config*, ajoutez la propriété **\<IsTransformWebConfigDisabled>** au fichier projet avec le paramètre `true` :</span><span class="sxs-lookup"><span data-stu-id="81b5f-168">To prevent the SDK from transforming the *web.config* file, add the **\<IsTransformWebConfigDisabled>** property to the project file with a setting of `true`:</span></span>

```xml
<PropertyGroup>
  <IsTransformWebConfigDisabled>true</IsTransformWebConfigDisabled>
</PropertyGroup>
```

<span data-ttu-id="81b5f-169">Si un fichier *web.config* se trouve dans le projet, il est transformé avec le *processPath* et les *arguments* corrects pour configurer le [module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), puis il est déplacé vers la [sortie publiée](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="81b5f-169">If a *web.config* file is in the project, it's transformed with the correct *processPath* and *arguments* to configure the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and moved to [published output](xref:host-and-deploy/directory-structure).</span></span> <span data-ttu-id="81b5f-170">La transformation ne modifie pas les paramètres de configuration IIS dans le fichier.</span><span class="sxs-lookup"><span data-stu-id="81b5f-170">The transformation doesn't modify IIS configuration settings in the file.</span></span>

### <a name="webconfig-location"></a><span data-ttu-id="81b5f-171">Emplacement du fichier web.config</span><span class="sxs-lookup"><span data-stu-id="81b5f-171">web.config location</span></span>

<span data-ttu-id="81b5f-172">Les applications .NET Core sont hébergées par le biais d’un proxy inverse entre IIS et le serveur Kestrel.</span><span class="sxs-lookup"><span data-stu-id="81b5f-172">.NET Core apps are hosted via a reverse proxy between IIS and the Kestrel server.</span></span> <span data-ttu-id="81b5f-173">Pour créer le proxy inverse, le fichier *web.config* doit être présent dans le chemin de racine de contenu (généralement le chemin de base de l’application) de l’application déployée, qui est le chemin physique du site web fourni à IIS.</span><span class="sxs-lookup"><span data-stu-id="81b5f-173">In order to create the reverse proxy, the *web.config* file must be present at the content root path (typically the app base path) of the deployed app, which is the website physical path provided to IIS.</span></span> <span data-ttu-id="81b5f-174">Le fichier *web.config* est nécessaire à la racine de l’application pour permettre la publication de plusieurs applications à l’aide de Web Deploy.</span><span class="sxs-lookup"><span data-stu-id="81b5f-174">The *web.config* file is required at the root of the app to enable the publishing of multiple apps using Web Deploy.</span></span>

<span data-ttu-id="81b5f-175">Il existe des fichiers sensibles sur le chemin physique de l’application, notamment des sous-dossiers, tels que *\<nom_assembly>.runtimeconfig.json*, *\<nom_assembly>.xml* (commentaires de documentation XML) et *\<nom_assembly>.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="81b5f-175">Sensitive files exist on the app's physical path, including subfolders, such as *\<assembly_name>.runtimeconfig.json*, *\<assembly_name>.xml* (XML Documentation comments), and *\<assembly_name>.deps.json*.</span></span> <span data-ttu-id="81b5f-176">Quand le fichier *web.config* est présent et configure le site, IIS empêche l’utilisation de ces fichiers sensibles.</span><span class="sxs-lookup"><span data-stu-id="81b5f-176">When the *web.config* file is present and configures the site, IIS prevents these sensitive files from being served.</span></span> <span data-ttu-id="81b5f-177">**Par conséquent, le fichier *web.config* ne doit jamais être accidentellement renommé ou supprimé du déploiement.**</span><span class="sxs-lookup"><span data-stu-id="81b5f-177">**Therefore, it's important that the *web.config* file isn't accidently renamed or removed from the deployment.**</span></span>

## <a name="create-the-iis-website"></a><span data-ttu-id="81b5f-178">Créer le site web IIS</span><span class="sxs-lookup"><span data-stu-id="81b5f-178">Create the IIS Website</span></span>

1. <span data-ttu-id="81b5f-179">Sur le système IIS cible, créez un dossier pour contenir les dossiers et fichiers publiés de l’application, qui sont décrits dans [Structure de répertoires](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="81b5f-179">On the target IIS system, create a folder to contain the app's published folders and files, which are described in [Directory Structure](xref:host-and-deploy/directory-structure).</span></span>

2. <span data-ttu-id="81b5f-180">Dans le dossier, créez un dossier *logs* où stocker les journaux stdout quand la journalisation stdout est activée.</span><span class="sxs-lookup"><span data-stu-id="81b5f-180">Within the folder, create a *logs* folder to hold stdout logs when stdout logging is enabled.</span></span> <span data-ttu-id="81b5f-181">Si l’application est déployée avec un dossier *logs* dans la charge utile, ignorez cette étape.</span><span class="sxs-lookup"><span data-stu-id="81b5f-181">If the app is deployed with a *logs* folder in the payload, skip this step.</span></span> <span data-ttu-id="81b5f-182">Pour obtenir des instructions sur la façon de créer le dossier *logs* à l’aide de MSBuild, consultez la rubrique [Structure de répertoires](xref:host-and-deploy/directory-structure).</span><span class="sxs-lookup"><span data-stu-id="81b5f-182">For instructions on how to make MSBuild create the *logs* folder, see the [Directory structure](xref:host-and-deploy/directory-structure) topic.</span></span>

3. <span data-ttu-id="81b5f-183">Dans **IIS Manager**, créez un site web.</span><span class="sxs-lookup"><span data-stu-id="81b5f-183">In **IIS Manager**, create a new website.</span></span> <span data-ttu-id="81b5f-184">Spécifiez le **Nom du site** et définissez le **Chemin physique** sur le dossier de déploiement de l’application.</span><span class="sxs-lookup"><span data-stu-id="81b5f-184">Provide a **Site name** and set the **Physical path** to the app's deployment folder.</span></span> <span data-ttu-id="81b5f-185">Spécifiez la configuration de **Liaison** et créez le site web.</span><span class="sxs-lookup"><span data-stu-id="81b5f-185">Provide the **Binding** configuration and create the website.</span></span>

4. <span data-ttu-id="81b5f-186">Sélectionnez **Aucun code managé** pour le pool d’applications.</span><span class="sxs-lookup"><span data-stu-id="81b5f-186">Set the application pool to **No Managed Code**.</span></span> <span data-ttu-id="81b5f-187">ASP.NET Core s’exécute dans un processus séparé et gère l’exécution.</span><span class="sxs-lookup"><span data-stu-id="81b5f-187">ASP.NET Core runs in a separate process and manages the runtime.</span></span>

5. <span data-ttu-id="81b5f-188">Ouvrez la fenêtre **Ajouter un site Web**.</span><span class="sxs-lookup"><span data-stu-id="81b5f-188">Open the **Add Website** window.</span></span>

   ![Sélectionnez « Ajouter un site web » dans le menu contextuel Sites.](index/_static/add-website-context-menu-ws2016.png)

6. <span data-ttu-id="81b5f-190">Configurez le site web.</span><span class="sxs-lookup"><span data-stu-id="81b5f-190">Configure the website.</span></span>

   ![Spécifiez le nom du site, le chemin physique et le nom d’hôte à l’étape Ajouter un site Web.](index/_static/add-website-ws2016.png)

7. <span data-ttu-id="81b5f-192">Dans le panneau **Pools d’applications**, ouvrez la fenêtre **Modifier le pool d’applications** en cliquant avec le bouton droit sur le pool d’applications du site web et en sélectionnant **Paramètres de base...** dans le menu contextuel.</span><span class="sxs-lookup"><span data-stu-id="81b5f-192">In the **Application Pools** panel, open the **Edit Application Pool** window by right-clicking on the website's app pool and selecting **Basic Settings...** from the popup menu.</span></span>

   ![Sélectionnez Paramètres de base dans le menu contextuel du pool d’applications.](index/_static/apppools-basic-settings-ws2016.png)

8. <span data-ttu-id="81b5f-194">Affectez la valeur **Aucun code managé** à **Version du CLR .NET**.</span><span class="sxs-lookup"><span data-stu-id="81b5f-194">Set the **.NET CLR version** to **No Managed Code**.</span></span>

   ![Définissez Aucun code managé pour la Version du CLR .NET.](index/_static/edit-apppool-ws2016.png)
     
    <span data-ttu-id="81b5f-196">Remarque : L’affectation de la valeur **Aucun code managé** à **Version du CLR .NET** est facultative.</span><span class="sxs-lookup"><span data-stu-id="81b5f-196">Note: Setting the **.NET CLR version** to **No Managed Code** is optional.</span></span> <span data-ttu-id="81b5f-197">ASP.NET Core ne repose pas sur le chargement du CLR de bureau.</span><span class="sxs-lookup"><span data-stu-id="81b5f-197">ASP.NET Core doesn't rely on loading the desktop CLR.</span></span>

9. <span data-ttu-id="81b5f-198">Vérifiez que l’identité de modèle de processus dispose des autorisations appropriées.</span><span class="sxs-lookup"><span data-stu-id="81b5f-198">Confirm the process model identity has the proper permissions.</span></span>

   <span data-ttu-id="81b5f-199">Si l’identité par défaut du pool d’applications (**Modèle de processus** > **Identité**) **ApplicationPoolIdentity** est remplacée par une autre identité, vérifiez que la nouvelle identité dispose des autorisations nécessaires pour accéder au dossier de l’application, à la base de données et aux autres ressources nécessaires.</span><span class="sxs-lookup"><span data-stu-id="81b5f-199">If the default identity of the app pool (**Process Model** > **Identity**) is changed from **ApplicationPoolIdentity** to another identity, verify that the new identity has the required permissions to access the app's folder, database, and other required resources.</span></span>
   
## <a name="deploy-the-app"></a><span data-ttu-id="81b5f-200">Déployer l’application</span><span class="sxs-lookup"><span data-stu-id="81b5f-200">Deploy the app</span></span>

<span data-ttu-id="81b5f-201">Déployez l’application sur le dossier créé sur le système IIS cible.</span><span class="sxs-lookup"><span data-stu-id="81b5f-201">Deploy the app to the folder created on the target IIS system.</span></span> <span data-ttu-id="81b5f-202">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) est le mécanisme recommandé pour le déploiement.</span><span class="sxs-lookup"><span data-stu-id="81b5f-202">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) is the recommended mechanism for deployment.</span></span>

<span data-ttu-id="81b5f-203">Vérifiez que l’application publiée pour le déploiement n’est pas en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="81b5f-203">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="81b5f-204">Les fichiers dans le dossier *publish* sont verrouillés quand l’application est en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="81b5f-204">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="81b5f-205">Les fichiers verrouillés ne peuvent pas être remplacés.</span><span class="sxs-lookup"><span data-stu-id="81b5f-205">Locked files can't be overwritten.</span></span> <span data-ttu-id="81b5f-206">Pour libérer les fichiers verrouillés dans un déploiement, arrêtez le pool d’applications :</span><span class="sxs-lookup"><span data-stu-id="81b5f-206">To release locked files in a deployment, stop the app pool:</span></span>

* <span data-ttu-id="81b5f-207">Manuellement dans le Gestionnaire des services IIS sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="81b5f-207">Manually in the IIS Manager on the server.</span></span>
* <span data-ttu-id="81b5f-208">En utilisant Web Deploy et en référençant `Microsoft.NET.Sdk.Web` dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="81b5f-208">Using Web Deploy and referencing `Microsoft.NET.Sdk.Web` in the project file.</span></span> <span data-ttu-id="81b5f-209">Un fichier *app_offline.htm* est placé à la racine du répertoire de l’application web.</span><span class="sxs-lookup"><span data-stu-id="81b5f-209">An *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="81b5f-210">Quand le fichier est présent, le module ASP.NET Core ferme l’application normalement et sert le fichier *app_offline.htm* pendant le déploiement.</span><span class="sxs-lookup"><span data-stu-id="81b5f-210">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="81b5f-211">Pour plus d’informations, consultez les [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span><span class="sxs-lookup"><span data-stu-id="81b5f-211">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>
* <span data-ttu-id="81b5f-212">Utilisez PowerShell pour arrêter et redémarrer le pool d’applications (cette opération requiert PowerShell version 5 ou ultérieure) :</span><span class="sxs-lookup"><span data-stu-id="81b5f-212">Use PowerShell to stop and restart the app pool (requires PowerShell 5 or later):</span></span>
  ```PowerShell
  $webAppPoolName = 'APP_POOL_NAME'

  # Stop the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
      Stop-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Stopped') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Stopped
  }

  # Provide script commands here to deploy the app

  # Restart the AppPool
  if((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
      Start-WebAppPool -Name $webAppPoolName
      while((Get-WebAppPoolState $webAppPoolName).Value -ne 'Started') {
          Start-Sleep -s 1
      }
      Write-Host `-AppPool Started
  }
  ```

### <a name="web-deploy-with-visual-studio"></a><span data-ttu-id="81b5f-213">Web Deploy avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="81b5f-213">Web Deploy with Visual Studio</span></span>

<span data-ttu-id="81b5f-214">Pour découvrir comment créer un profil de publication pour une utilisation avec Web Deploy, consultez la rubrique [Profils de publication Visual Studio pour le déploiement d’applications ASP.NET Core](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="81b5f-214">See the [Visual Studio publish profiles for ASP.NET Core app deployment](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles) topic to learn how to create a publish profile for use with Web Deploy.</span></span> <span data-ttu-id="81b5f-215">Si le fournisseur d’hébergement fournit un profil de publication ou prend en charge sa création, téléchargez son profil et importez-le à l’aide de la boîte de dialogue **Publier** de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="81b5f-215">If the hosting provider supplies a Publish Profile or support for creating one, download their profile and import it using the Visual Studio **Publish** dialog.</span></span>

![Boîte de dialogue Publier](index/_static/pub-dialog.png)

### <a name="web-deploy-outside-of-visual-studio"></a><span data-ttu-id="81b5f-217">Web Deploy en dehors de Visual Studio</span><span class="sxs-lookup"><span data-stu-id="81b5f-217">Web Deploy outside of Visual Studio</span></span>

<span data-ttu-id="81b5f-218">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) peut également être utilisé en dehors de Visual Studio à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="81b5f-218">[Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) can also be used outside of Visual Studio from the command line.</span></span> <span data-ttu-id="81b5f-219">Pour plus d’informations, consultez [Web Deployment Tool (Outil de déploiement Web)](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span><span class="sxs-lookup"><span data-stu-id="81b5f-219">For more information, see [Web Deployment Tool](/iis/publish/using-web-deploy/use-the-web-deployment-tool).</span></span>

### <a name="alternatives-to-web-deploy"></a><span data-ttu-id="81b5f-220">Alternatives à Web Deploy</span><span class="sxs-lookup"><span data-stu-id="81b5f-220">Alternatives to Web Deploy</span></span>

<span data-ttu-id="81b5f-221">Utilisez la méthode de votre choix, telle que Xcopy, Robocopy ou PowerShell, pour déplacer l’application vers le système hôte.</span><span class="sxs-lookup"><span data-stu-id="81b5f-221">Use any of several methods to move the app to the hosting system, such as Xcopy, Robocopy, or PowerShell.</span></span> <span data-ttu-id="81b5f-222">Les utilisateurs de Visual Studio peuvent utiliser les [Exemples de publication](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).</span><span class="sxs-lookup"><span data-stu-id="81b5f-222">Visual Studio users may use the [Publish Samples](https://github.com/aspnet/vsweb-publish/blob/master/samples/samples.md).</span></span>

## <a name="browse-the-website"></a><span data-ttu-id="81b5f-223">Parcourir le site web</span><span class="sxs-lookup"><span data-stu-id="81b5f-223">Browse the website</span></span>

![Le navigateur Microsoft Edge a chargé la page de démarrage IIS.](index/_static/browsewebsite.png)

## <a name="data-protection"></a><span data-ttu-id="81b5f-225">Protection des données</span><span class="sxs-lookup"><span data-stu-id="81b5f-225">Data protection</span></span>

<span data-ttu-id="81b5f-226">Protection des données est utilisée par plusieurs intergiciels ASP.NET, notamment ceux utilisés lors de l’authentification.</span><span class="sxs-lookup"><span data-stu-id="81b5f-226">Data Protection is used by several ASP.NET middlewares, including those used in authentication.</span></span> <span data-ttu-id="81b5f-227">Même si les API de protection des données ne sont pas appelées à partir du code de l’utilisateur, la protection des données doit être configurée avec un script de déploiement ou dans un code utilisateur pour créer un magasin de clés persistantes.</span><span class="sxs-lookup"><span data-stu-id="81b5f-227">Even if Data Protection APIs aren't called from user's code, Data Protection should be configured with a deployment script or in user code to create a persistent key store.</span></span> <span data-ttu-id="81b5f-228">Si la protection des données n’est pas configurée, les clés sont conservées en mémoire et ignorées au redémarrage de l’application.</span><span class="sxs-lookup"><span data-stu-id="81b5f-228">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="81b5f-229">Si le porte-clés est stocké en mémoire, au redémarrage de l’application :</span><span class="sxs-lookup"><span data-stu-id="81b5f-229">If the keyring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="81b5f-230">Tous les jetons d’authentification de formulaires deviennent non valides.</span><span class="sxs-lookup"><span data-stu-id="81b5f-230">All forms authentication tokens are invalidated.</span></span> 
* <span data-ttu-id="81b5f-231">Les utilisateurs doivent se reconnecter pour envoyer leur prochaine demande.</span><span class="sxs-lookup"><span data-stu-id="81b5f-231">Users are required to sign in again on their next request.</span></span> 
* <span data-ttu-id="81b5f-232">Toutes les données protégées par le porte-clés ne peuvent plus être déchiffrées.</span><span class="sxs-lookup"><span data-stu-id="81b5f-232">Any data protected with the keyring can no longer be decrypted.</span></span>

<span data-ttu-id="81b5f-233">Pour configurer la protection des données sous IIS, adoptez **l’une** des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="81b5f-233">To configure Data Protection under IIS, use **one** of the following approaches:</span></span>

### <a name="create-a-data-protection-registry-hive"></a><span data-ttu-id="81b5f-234">Créer une ruche de Registre de Protection des données</span><span class="sxs-lookup"><span data-stu-id="81b5f-234">Create a Data Protection Registry Hive</span></span>

<span data-ttu-id="81b5f-235">Les clés de Protection des données utilisées par les applications ASP.NET sont stockées dans des ruches de Registre externes aux applications.</span><span class="sxs-lookup"><span data-stu-id="81b5f-235">Data Protection keys used by ASP.NET apps are stored in registry hives external to the apps.</span></span> <span data-ttu-id="81b5f-236">Afin de conserver les clés pour une application donnée, créez une ruche de Registre pour le pool d’applications.</span><span class="sxs-lookup"><span data-stu-id="81b5f-236">To persist the keys for a given app, create a registry hive for the app pool.</span></span>

<span data-ttu-id="81b5f-237">Pour les installations IIS autonomes hors d’une batterie de serveurs, le [script PowerShell Provision-AutoGenKeys.ps1 de protection des données](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) peut être utilisé pour chaque pool d’applications utilisé avec une application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="81b5f-237">For standalone, non-webfarm IIS installations, the [Data Protection Provision-AutoGenKeys.ps1 PowerShell script](https://github.com/aspnet/DataProtection/blob/dev/Provision-AutoGenKeys.ps1) can be used for each app pool used with an ASP.NET Core app.</span></span> <span data-ttu-id="81b5f-238">Ce script crée une clé de Registre spéciale dans le Registre HKLM, gérée par liste de contrôle d’accès uniquement pour le compte du processus Worker.</span><span class="sxs-lookup"><span data-stu-id="81b5f-238">This script creates a special registry key in the HKLM registry that's ACLed only to the worker process account.</span></span> <span data-ttu-id="81b5f-239">Les clés sont chiffrées au repos à l’aide de DPAPI avec une clé à l’échelle de la machine.</span><span class="sxs-lookup"><span data-stu-id="81b5f-239">Keys are encrypted at rest using DPAPI with a machine-wide key.</span></span>

<span data-ttu-id="81b5f-240">Dans les scénarios de batterie de serveurs web, vous pouvez configurer une application afin qu’elle utilise un chemin UNC pour stocker son porte-clés de protection des données.</span><span class="sxs-lookup"><span data-stu-id="81b5f-240">In web farm scenarios, an app can be configured to use a UNC path to store its data protection keyring.</span></span> <span data-ttu-id="81b5f-241">Par défaut, les clés de protection des données ne sont pas chiffrées.</span><span class="sxs-lookup"><span data-stu-id="81b5f-241">By default, the data protection keys are not encrypted.</span></span> <span data-ttu-id="81b5f-242">Vérifiez que les autorisations de fichiers pour ce type de partage sont limitées au compte Windows sous lequel s’exécute l’application.</span><span class="sxs-lookup"><span data-stu-id="81b5f-242">Ensure that the file permissions for such a share are limited to the Windows account the app runs as.</span></span> <span data-ttu-id="81b5f-243">En outre, un certificat X509 peut être utilisé pour protéger les clés au repos.</span><span class="sxs-lookup"><span data-stu-id="81b5f-243">In addition, an X509 certificate can be used to protect keys at rest.</span></span> <span data-ttu-id="81b5f-244">Mettez en œuvre un mécanisme permettant aux utilisateurs de charger des certificats : placez les certificats dans le magasin de certificats approuvés de l’utilisateur et vérifiez qu’ils sont disponibles sur toutes les machines où s’exécute l’application de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="81b5f-244">Consider a mechanism to allow users to upload certificates: Place certificates into the user's trusted certificate store and ensure they're available on all machines where the user's app runs.</span></span> <span data-ttu-id="81b5f-245">Pour plus d’informations, consultez [Configuration de la Protection des données](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="81b5f-245">See [Configuring Data Protection](xref:security/data-protection/configuration/overview) for details.</span></span>

### <a name="configure-the-iis-application-pool-to-load-the-user-profile"></a><span data-ttu-id="81b5f-246">Configurer le pool d’applications IIS pour charger le profil utilisateur</span><span class="sxs-lookup"><span data-stu-id="81b5f-246">Configure the IIS Application Pool to load the user profile</span></span>

<span data-ttu-id="81b5f-247">Ce paramètre se trouve dans la section **Modèle de processus** sous les **Paramètres avancés** du pool d’applications.</span><span class="sxs-lookup"><span data-stu-id="81b5f-247">This setting is in the **Process Model** section under the **Advanced Settings** for the app pool.</span></span> <span data-ttu-id="81b5f-248">Définissez Charger le profil utilisateur sur `True`.</span><span class="sxs-lookup"><span data-stu-id="81b5f-248">Set Load User Profile to `True`.</span></span> <span data-ttu-id="81b5f-249">Cela permet de stocker les clés sous le répertoire du profil utilisateur et de les protéger à l’aide de DPAPI avec une clé propre au compte d’utilisateur utilisé pour le pool d’applications.</span><span class="sxs-lookup"><span data-stu-id="81b5f-249">This stores keys under the user profile directory and protects them using DPAPI with a key specific to the user account used for the app pool.</span></span>

### <a name="use-the-file-system-as-a-key-ring-store"></a><span data-ttu-id="81b5f-250">Utiliser le système de fichiers comme magasin de porte-clés</span><span class="sxs-lookup"><span data-stu-id="81b5f-250">Use the file system as a key ring store</span></span>

<span data-ttu-id="81b5f-251">Ajustez le code d’application pour [utiliser le système de fichiers comme magasin de porte-clés](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="81b5f-251">Adjust the app code to [use the file system as a key ring store](xref:security/data-protection/configuration/overview).</span></span> <span data-ttu-id="81b5f-252">Utilisez un certificat X509 pour protéger le porte-clés et vérifiez qu’il s’agit d’un certificat approuvé.</span><span class="sxs-lookup"><span data-stu-id="81b5f-252">Use an X509 certificate to protect the keyring and ensure the certificate is a trusted certificate.</span></span> <span data-ttu-id="81b5f-253">S’il s’agit d’un certificat auto-signé, placez-le dans le magasin racine approuvé.</span><span class="sxs-lookup"><span data-stu-id="81b5f-253">If it's a self signed certificate, place the certificate in the Trusted Root store.</span></span>

<span data-ttu-id="81b5f-254">Quand vous utilisez IIS dans une batterie de serveurs web :</span><span class="sxs-lookup"><span data-stu-id="81b5f-254">When using IIS in a web farm:</span></span>

* <span data-ttu-id="81b5f-255">Utilisez un partage de fichiers accessible par tous les ordinateurs.</span><span class="sxs-lookup"><span data-stu-id="81b5f-255">Use a file share that all machines can access.</span></span>
* <span data-ttu-id="81b5f-256">Déployez un certificat X509 sur chaque ordinateur.</span><span class="sxs-lookup"><span data-stu-id="81b5f-256">Deploy an X509 certificate to each machine.</span></span> <span data-ttu-id="81b5f-257">Configurez la [protection des données dans le code](xref:security/data-protection/configuration/overview).</span><span class="sxs-lookup"><span data-stu-id="81b5f-257">Configure [data protection in code](xref:security/data-protection/configuration/overview).</span></span>

### <a name="set-a-machine-wide-policy-for-data-protection"></a><span data-ttu-id="81b5f-258">Définir une stratégie au niveau de l’ordinateur pour la protection des données</span><span class="sxs-lookup"><span data-stu-id="81b5f-258">Set a machine-wide policy for data protection</span></span>

<span data-ttu-id="81b5f-259">Le système de protection des données offre une prise en charge limitée de la définition d’une [stratégie au niveau de l’ordinateur](xref:security/data-protection/configuration/machine-wide-policy) par défaut pour toutes les applications qui utilisent les API de protection des données.</span><span class="sxs-lookup"><span data-stu-id="81b5f-259">The data protection system has limited support for setting a default [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy) for all apps that consume the Data Protection APIs.</span></span> <span data-ttu-id="81b5f-260">Pour plus d’informations, consultez la documentation de [protection des données](xref:security/data-protection/index).</span><span class="sxs-lookup"><span data-stu-id="81b5f-260">See the [data protection](xref:security/data-protection/index) documentation for more details.</span></span>

## <a name="configuration-of-sub-applications"></a><span data-ttu-id="81b5f-261">Configuration des sous-applications</span><span class="sxs-lookup"><span data-stu-id="81b5f-261">Configuration of sub-applications</span></span>

<span data-ttu-id="81b5f-262">Les sous-applications ajoutées sous l’application racine ne doivent pas inclure le module ASP.NET Core en tant que gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="81b5f-262">Sub-apps added under the root app shouldn't include the ASP.NET Core Module as a handler.</span></span> <span data-ttu-id="81b5f-263">Si le module est ajouté en guise de gestionnaire dans le fichier *web.config* d’une sous-application, une erreur 500.19 (erreur interne du serveur) référençant le fichier de configuration défectueux est reçue au moment de la tentative d’accès à la sous-application.</span><span class="sxs-lookup"><span data-stu-id="81b5f-263">If the module is added as a handler in a sub-app's *web.config* file, a 500.19 (Internal Server Error) referencing the faulty config file is received when attempting to browse the sub-app.</span></span> <span data-ttu-id="81b5f-264">L’exemple suivant montre le contenu d’un fichier *web.config* publié pour une sous-application ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="81b5f-264">The following example shows the contents of a published *web.config* file for an ASP.NET Core sub-app:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="81b5f-265">Si vous hébergez une sous-application non-ASP.NET Core sous une application ASP.NET Core, supprimez explicitement le gestionnaire hérité dans le fichier *web.config* de la sous-application :</span><span class="sxs-lookup"><span data-stu-id="81b5f-265">When hosting a non-ASP.NET Core sub-app underneath an ASP.NET Core app, explicitly remove the inherited handler in the sub-app *web.config* file:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <remove name="aspNetCore"/>
    </handlers>
    <aspNetCore processPath="dotnet" 
      arguments=".\<assembly_name>.dll" 
      stdoutLogEnabled="false" 
      stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="81b5f-266">Pour plus d’informations sur la configuration du module ASP.NET Core, consultez la rubrique [Introduction au module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) et les [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="81b5f-266">For more information on configuring the ASP.NET Core Module, see the [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic and the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="configuration-of-iis-with-webconfig"></a><span data-ttu-id="81b5f-267">Configuration d’IIS avec web.config</span><span class="sxs-lookup"><span data-stu-id="81b5f-267">Configuration of IIS with web.config</span></span>

<span data-ttu-id="81b5f-268">La configuration d’IIS est influencée par la section **\<system.webServer>** de *web.config* pour les fonctionnalités d’IIS qui s’appliquent à une configuration de proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="81b5f-268">IIS configuration is influenced by the **\<system.webServer>** section of *web.config* for those IIS features that apply to a reverse proxy configuration.</span></span> <span data-ttu-id="81b5f-269">Si IIS est configuré au niveau du système pour utiliser la compression dynamique, ce paramètre peut être désactivé pour une application avec l’élément **\<urlCompression>** dans le fichier *web.config* de l’application.</span><span class="sxs-lookup"><span data-stu-id="81b5f-269">If IIS is configured at the system level to use dynamic compression, that setting can be disabled for an app with the **\<urlCompression>** element in the app's *web.config* file.</span></span> <span data-ttu-id="81b5f-270">Pour plus d’informations, consultez les [informations de référence sur configuration de \<system.webServer>](https://docs.microsoft.com/iis/configuration/system.webServer/), les [Informations de référence sur la configuration du module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) et [Utilisation des modules IIS avec ASP.NET Core](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="81b5f-270">For more information, see the [configuration reference for \<system.webServer>](https://docs.microsoft.com/iis/configuration/system.webServer/), [ASP.NET Core Module Configuration Reference](xref:host-and-deploy/aspnet-core-module) and [Using IIS Modules with ASP.NET Core](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="81b5f-271">S’il est nécessaire de définir des variables d’environnement pour des applications qui s’exécutent dans des pools d’applications isolés (prise en charge sur IIS 10.0+), consultez la section *Commande AppCmd.exe* de la rubrique [Variables d’environnement \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) dans la documentation de référence IIS.</span><span class="sxs-lookup"><span data-stu-id="81b5f-271">If there's a need to set environment variables for individual apps running in isolated app pools (supported on IIS 10.0+), see the *AppCmd.exe command* section of the [Environment Variables \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) topic in the IIS reference documentation.</span></span>

## <a name="configuration-sections-of-webconfig"></a><span data-ttu-id="81b5f-272">Sections de configuration de web.config</span><span class="sxs-lookup"><span data-stu-id="81b5f-272">Configuration sections of web.config</span></span>

<span data-ttu-id="81b5f-273">Les sections de configuration des applications ASP.NET Framework dans *web.config* ne sont pas utilisées par les applications ASP.NET Core pour la configuration :</span><span class="sxs-lookup"><span data-stu-id="81b5f-273">Configruation sections of ASP.NET Framework apps in *web.config* aren't used by ASP.NET Core apps for configuration:</span></span>

* <span data-ttu-id="81b5f-274">**\<system.web>**</span><span class="sxs-lookup"><span data-stu-id="81b5f-274">**\<system.web>**</span></span>
* <span data-ttu-id="81b5f-275">**\<appSettings>**</span><span class="sxs-lookup"><span data-stu-id="81b5f-275">**\<appSettings>**</span></span>
* <span data-ttu-id="81b5f-276">**\<connectionStrings>**</span><span class="sxs-lookup"><span data-stu-id="81b5f-276">**\<connectionStrings>**</span></span>
* <span data-ttu-id="81b5f-277">**\<location>**</span><span class="sxs-lookup"><span data-stu-id="81b5f-277">**\<location>**</span></span>

<span data-ttu-id="81b5f-278">Les applications ASP.NET Core sont configurées à l’aide d’autres fournisseurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="81b5f-278">ASP.NET Core apps are configured using other configuration providers.</span></span> <span data-ttu-id="81b5f-279">Pour plus d’informations, consultez [Configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="81b5f-279">For more information, see [Configuration](xref:fundamentals/configuration/index).</span></span>

## <a name="application-pools"></a><span data-ttu-id="81b5f-280">Pools d'applications</span><span class="sxs-lookup"><span data-stu-id="81b5f-280">Application Pools</span></span>

<span data-ttu-id="81b5f-281">Quand vous hébergez plusieurs sites web sur un même système, isolez les applications les unes des autres en exécutant chaque application dans son propre pool d’applications.</span><span class="sxs-lookup"><span data-stu-id="81b5f-281">When hosting multiple websites on a single system, isolate the apps from each other by running each app in its own app pool.</span></span> <span data-ttu-id="81b5f-282">La boîte de dialogue **Ajouter un site Web** d’IIS applique cette configuration par défaut.</span><span class="sxs-lookup"><span data-stu-id="81b5f-282">The IIS **Add Website** dialog defaults to this configuration.</span></span> <span data-ttu-id="81b5f-283">Quand un **Nom du site** est fourni, le texte est automatiquement transféré vers la zone de texte **Pool d’applications**.</span><span class="sxs-lookup"><span data-stu-id="81b5f-283">When **Site name** is provided, the text is automatically transferred to the **Application pool** textbox.</span></span> <span data-ttu-id="81b5f-284">Un pool d’applications est créé avec le nom du site quand le site web est ajouté.</span><span class="sxs-lookup"><span data-stu-id="81b5f-284">A new app pool is created using the site name when the website is added.</span></span>

## <a name="application-pool-identity"></a><span data-ttu-id="81b5f-285">Identité du pool d’applications</span><span class="sxs-lookup"><span data-stu-id="81b5f-285">Application Pool Identity</span></span>

<span data-ttu-id="81b5f-286">Un compte d’identité du pool d’applications permet à une application de s’exécuter sous un compte unique sans qu’il soit nécessaire de créer et de gérer des domaines ou des comptes locaux.</span><span class="sxs-lookup"><span data-stu-id="81b5f-286">An app pool identity account allows an app to run under a unique account without having to create and manage domains or local accounts.</span></span> <span data-ttu-id="81b5f-287">Sur IIS 8.0+, le processus Worker d’administration IIS (WAS) crée un compte virtuel avec le nom du nouveau pool d’applications et exécute les processus Worker du pool d’applications sous ce compte par défaut.</span><span class="sxs-lookup"><span data-stu-id="81b5f-287">On IIS 8.0+, the IIS Admin Worker Process (WAS) creates a virtual account with the name of the new app pool and runs the app pool's worker processes under this account by default.</span></span> <span data-ttu-id="81b5f-288">Dans la console de gestion IIS, sous **Paramètres avancés** pour le pool d’applications, vérifiez que **l’Identité** est configurée pour utiliser **ApplicationPoolIdentity** :</span><span class="sxs-lookup"><span data-stu-id="81b5f-288">In the IIS Management Console under **Advanced Settings** for the app pool, ensure that the **Identity** is set to use **ApplicationPoolIdentity**:</span></span>

![Boîte de dialogue Paramètres avancés du pool applications](index/_static/apppool-identity.png)

<span data-ttu-id="81b5f-290">Le processus de gestion IIS crée un identificateur sécurisé avec le nom du pool d’applications dans le système de sécurité Windows.</span><span class="sxs-lookup"><span data-stu-id="81b5f-290">The IIS management process creates a secure identifier with the name of the app pool in the Windows Security System.</span></span> <span data-ttu-id="81b5f-291">Les ressources peuvent être sécurisées à l’aide de cette identité. Toutefois, cette identité n’est pas un compte d’utilisateur réel et n’apparaît pas dans la console de gestion d’utilisateur Windows.</span><span class="sxs-lookup"><span data-stu-id="81b5f-291">Resources can be secured by using this identity; however, this identity isn't a real user account and won't show up in the Windows User Management Console.</span></span>

<span data-ttu-id="81b5f-292">Si le processus Worker IIS a besoin d’un accès élevé à l’application, modifiez la liste de contrôle d’accès (ACL) du répertoire contenant l’application :</span><span class="sxs-lookup"><span data-stu-id="81b5f-292">If the IIS worker process requires elevated access to the app, modify the Access Control List (ACL) for the directory containing the app:</span></span>

1. <span data-ttu-id="81b5f-293">Ouvrez l’Explorateur Windows et accédez au répertoire.</span><span class="sxs-lookup"><span data-stu-id="81b5f-293">Open Windows Explorer and navigate to the directory.</span></span>

2. <span data-ttu-id="81b5f-294">Cliquez avec le bouton droit sur le répertoire et sélectionnez **Propriétés**.</span><span class="sxs-lookup"><span data-stu-id="81b5f-294">Right-click on the directory and select **Properties**.</span></span>

3. <span data-ttu-id="81b5f-295">Sous l’onglet **Sécurité**, sélectionnez le bouton **Modifier**, puis le bouton **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="81b5f-295">Under the **Security** tab, select the **Edit** button and then the **Add** button.</span></span>

4. <span data-ttu-id="81b5f-296">Sélectionnez le bouton **Emplacements**, puis vérifiez que le système est sélectionné.</span><span class="sxs-lookup"><span data-stu-id="81b5f-296">Select the **Locations** button and make sure the system is selected.</span></span>

5. <span data-ttu-id="81b5f-297">Entrez **IIS AppPool\DefaultAppPool** dans la zone de texte **Entrez les noms d’objets à sélectionner**.</span><span class="sxs-lookup"><span data-stu-id="81b5f-297">Enter **IIS AppPool\DefaultAppPool** in **Enter the object names to select** textbox.</span></span>

   ![Boîte de dialogue de sélection d’utilisateurs ou de groupes pour le dossier d’application](index/_static/select-users-or-groups-1.png)

6. <span data-ttu-id="81b5f-299">Sélectionnez le bouton **Vérifier les noms**.</span><span class="sxs-lookup"><span data-stu-id="81b5f-299">Select the **Check Names** button.</span></span> <span data-ttu-id="81b5f-300">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="81b5f-300">Select **OK**.</span></span>

   ![Boîte de dialogue de sélection d’utilisateurs ou de groupes pour le dossier d’application](index/_static/select-users-or-groups-2.png)

<span data-ttu-id="81b5f-302">Vous pouvez également effectuer cette opération par le biais d’une invite de commandes à l’aide de l’outil **ICACLS** :</span><span class="sxs-lookup"><span data-stu-id="81b5f-302">This can also be accomplished via a command prompt using the **ICACLS** tool:</span></span>

```console
ICACLS C:\sites\MyWebApp /grant "IIS AppPool\DefaultAppPool":F
```

## <a name="additional-resources"></a><span data-ttu-id="81b5f-303">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="81b5f-303">Additional resources</span></span>

* [<span data-ttu-id="81b5f-304">Résoudre les problèmes liés à ASP.NET Core sur IIS</span><span class="sxs-lookup"><span data-stu-id="81b5f-304">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="81b5f-305">Informations de référence sur les erreurs courantes pour Azure App Service et IIS avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="81b5f-305">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="81b5f-306">Introduction au Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="81b5f-306">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="81b5f-307">Informations de référence sur la configuration du Module ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="81b5f-307">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="81b5f-308">Utilisation de modules IIS avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="81b5f-308">Using IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="81b5f-309">Présentation d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="81b5f-309">Introduction to ASP.NET Core</span></span>](../index.md)
* [<span data-ttu-id="81b5f-310">Site officiel de Microsoft IIS</span><span class="sxs-lookup"><span data-stu-id="81b5f-310">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="81b5f-311">Bibliothèque Microsoft TechNet : Windows Server</span><span class="sxs-lookup"><span data-stu-id="81b5f-311">Microsoft TechNet Library: Windows Server</span></span>](https://docs.microsoft.com/windows-server/windows-server-versions)
