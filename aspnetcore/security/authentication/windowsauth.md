---
title: Configurer l’authentification Windows dans ASP.NET Core
author: scottaddie
description: Découvrez comment configurer l’authentification Windows dans ASP.NET Core, à l’aide d’IIS Express, IIS, HTTP.sys et WebListener.
ms.author: riande
ms.custom: mvc
ms.date: 11/01/2018
uid: security/authentication/windowsauth
ms.openlocfilehash: 87fcab75555c1dae0b2815c30d79fd4615df9660
ms.sourcegitcommit: 85f2939af7a167b9694e1d2093277ffc9a741b23
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50968291"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="08cf6-103">Configurer l’authentification Windows dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="08cf6-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="08cf6-104">Par [Steve Smith](https://ardalis.com) et [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="08cf6-104">By [Steve Smith](https://ardalis.com) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="08cf6-105">L’authentification Windows peut être configurée pour les applications ASP.NET Core hébergées avec IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), ou [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="08cf6-105">Windows Authentication can be configured for ASP.NET Core apps hosted with IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), or [WebListener](xref:fundamentals/servers/weblistener).</span></span>

## <a name="windows-authentication"></a><span data-ttu-id="08cf6-106">Authentification Windows</span><span class="sxs-lookup"><span data-stu-id="08cf6-106">Windows Authentication</span></span>

<span data-ttu-id="08cf6-107">L’authentification Windows s’appuie sur le système d’exploitation pour authentifier les utilisateurs des applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="08cf6-107">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="08cf6-108">Vous pouvez utiliser l’authentification Windows lorsque votre serveur s’exécute sur un réseau d’entreprise à l’aide d’identités de domaine Active Directory ou d’autres comptes Windows pour identifier les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="08cf6-108">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="08cf6-109">L’authentification Windows est mieux adaptée aux environnements intranet dans lequel les utilisateurs, les applications clientes et les serveurs web appartiennent au même domaine Windows.</span><span class="sxs-lookup"><span data-stu-id="08cf6-109">Windows Authentication is best suited to intranet environments in which users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="08cf6-110">[En savoir plus sur l’authentification Windows et l’installation pour IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="08cf6-110">[Learn more about Windows Authentication and installing it for IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="08cf6-111">Activer l’authentification Windows dans une application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="08cf6-111">Enable Windows Authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="08cf6-112">Le modèle d’Application Web de Visual Studio peut être configuré pour prendre en charge l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="08cf6-112">The Visual Studio Web Application template can be configured to support Windows Authentication.</span></span>

### <a name="use-the-windows-authentication-app-template"></a><span data-ttu-id="08cf6-113">Utiliser le modèle d’application de l’authentification Windows</span><span class="sxs-lookup"><span data-stu-id="08cf6-113">Use the Windows Authentication app template</span></span>

<span data-ttu-id="08cf6-114">Dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="08cf6-114">In Visual Studio:</span></span>

1. <span data-ttu-id="08cf6-115">Créez une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="08cf6-115">Create a new ASP.NET Core Web Application.</span></span>
1. <span data-ttu-id="08cf6-116">Sélectionnez l’Application Web à partir de la liste des modèles.</span><span class="sxs-lookup"><span data-stu-id="08cf6-116">Select Web Application from the list of templates.</span></span>
1. <span data-ttu-id="08cf6-117">Sélectionnez le **modifier l’authentification** bouton et sélectionnez **l’authentification Windows**.</span><span class="sxs-lookup"><span data-stu-id="08cf6-117">Select the **Change Authentication** button and select **Windows Authentication**.</span></span>

<span data-ttu-id="08cf6-118">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="08cf6-118">Run the app.</span></span> <span data-ttu-id="08cf6-119">Le nom d’utilisateur s’affiche dans le coin supérieur droit de l’application.</span><span class="sxs-lookup"><span data-stu-id="08cf6-119">The username appears in the top right of the app.</span></span>

![Capture d’écran de navigateur de l’authentification Windows](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="08cf6-121">Pour le travail de développement à l’aide d’IIS Express, le modèle fournit toute la configuration nécessaire pour utiliser l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="08cf6-121">For development work using IIS Express, the template provides all the configuration necessary to use Windows Authentication.</span></span> <span data-ttu-id="08cf6-122">La section suivante montre comment configurer manuellement une application ASP.NET Core pour l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="08cf6-122">The following section shows how to manually configure an ASP.NET Core app for Windows Authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="08cf6-123">Paramètres de Visual Studio pour Windows et l’authentification anonyme</span><span class="sxs-lookup"><span data-stu-id="08cf6-123">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="08cf6-124">Le projet Visual Studio **propriétés** de page **déboguer** onglet fournit des cases à cocher pour l’authentification Windows et l’authentification anonyme.</span><span class="sxs-lookup"><span data-stu-id="08cf6-124">The Visual Studio project **Properties** page's **Debug** tab provides check boxes for Windows Authentication and anonymous authentication.</span></span>

![Capture d’écran de navigateur de l’authentification Windows](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="08cf6-126">Vous pouvez également ces deux propriétés peuvent être configurées dans le *launchSettings.json* fichier :</span><span class="sxs-lookup"><span data-stu-id="08cf6-126">Alternatively, these two properties can be configured in the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="08cf6-127">Activer l’authentification Windows avec IIS</span><span class="sxs-lookup"><span data-stu-id="08cf6-127">Enable Windows Authentication with IIS</span></span>

<span data-ttu-id="08cf6-128">IIS utilise le [Module ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) pour héberger des applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="08cf6-128">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="08cf6-129">L’authentification Windows est configurée dans IIS, pas l’application.</span><span class="sxs-lookup"><span data-stu-id="08cf6-129">Windows Authentication is configured in IIS, not the app.</span></span> <span data-ttu-id="08cf6-130">Les sections suivantes vous montrent comment utiliser le Gestionnaire des services Internet pour configurer une application ASP.NET Core pour utiliser l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="08cf6-130">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows Authentication.</span></span>

### <a name="iis-configuration"></a><span data-ttu-id="08cf6-131">Configuration d’IIS</span><span class="sxs-lookup"><span data-stu-id="08cf6-131">IIS configuration</span></span>

<span data-ttu-id="08cf6-132">Activer le Service de rôle IIS pour l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="08cf6-132">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="08cf6-133">Pour plus d’informations, consultez [activer l’authentification Windows dans les Services de rôle IIS (voir étape 2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="08cf6-133">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="08cf6-134">Intergiciel d’intégration IIS est configuré pour authentifier les demandes automatiquement par défaut.</span><span class="sxs-lookup"><span data-stu-id="08cf6-134">IIS Integration Middleware is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="08cf6-135">Pour plus d’informations, consultez [héberger ASP.NET Core sur Windows avec IIS : options d’IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="08cf6-135">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="08cf6-136">Le Module ASP.NET Core est configuré pour transférer le jeton d’authentification de Windows à l’application par défaut.</span><span class="sxs-lookup"><span data-stu-id="08cf6-136">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="08cf6-137">Pour plus d’informations, consultez [référence de configuration de Module ASP.NET Core : attributs de l’élément aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="08cf6-137">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="08cf6-138">Créer un nouveau site IIS</span><span class="sxs-lookup"><span data-stu-id="08cf6-138">Create a new IIS site</span></span>

<span data-ttu-id="08cf6-139">Spécifiez un nom et le dossier et lui permettre de créer un nouveau pool d’applications.</span><span class="sxs-lookup"><span data-stu-id="08cf6-139">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="08cf6-140">Personnaliser l’authentification</span><span class="sxs-lookup"><span data-stu-id="08cf6-140">Customize authentication</span></span>

<span data-ttu-id="08cf6-141">Ouvrez les fonctionnalités d’authentification pour le site.</span><span class="sxs-lookup"><span data-stu-id="08cf6-141">Open the Authentication features for the site.</span></span>

![Menu de l’authentification IIS](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="08cf6-143">Désactivez l’authentification anonyme et activer l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="08cf6-143">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![Paramètres d’authentification IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="08cf6-145">Publier votre projet dans le dossier de site IIS</span><span class="sxs-lookup"><span data-stu-id="08cf6-145">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="08cf6-146">À l’aide de Visual Studio ou l’interface CLI .NET Core, de publier l’application dans le dossier de destination.</span><span class="sxs-lookup"><span data-stu-id="08cf6-146">Using Visual Studio or the .NET Core CLI, publish the app to the destination folder.</span></span>

![Boîte de dialogue de publication de Visual Studio](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="08cf6-148">En savoir plus sur [publication sur IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="08cf6-148">Learn more about [publishing to IIS](xref:host-and-deploy/iis/index).</span></span>

<span data-ttu-id="08cf6-149">Lancez l’application pour vérifier que l’utilisation de l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="08cf6-149">Launch the app to verify Windows Authentication is working.</span></span>

::: moniker range=">= aspnetcore-2.0"

## <a name="enable-windows-authentication-with-httpsys"></a><span data-ttu-id="08cf6-150">Activer l’authentification Windows avec HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="08cf6-150">Enable Windows Authentication with HTTP.sys</span></span>

<span data-ttu-id="08cf6-151">Bien que Kestrel ne prend pas en charge l’authentification Windows, vous pouvez utiliser [HTTP.sys](xref:fundamentals/servers/httpsys) pour prendre en charge les scénarios auto-hébergés sur Windows.</span><span class="sxs-lookup"><span data-stu-id="08cf6-151">Although Kestrel doesn't support Windows Authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="08cf6-152">L’exemple suivant configure un hôte web de l’application pour utiliser HTTP.sys avec l’authentification Windows :</span><span class="sxs-lookup"><span data-stu-id="08cf6-152">The following example configures the app's web host to use HTTP.sys with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

> [!NOTE]
> <span data-ttu-id="08cf6-153">HTTP.sys délègue l’authentification en mode noyau avec le protocole d’authentification Kerberos.</span><span class="sxs-lookup"><span data-stu-id="08cf6-153">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="08cf6-154">L’authentification en mode utilisateur n’est pas prise en charge avec Kerberos et HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="08cf6-154">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="08cf6-155">Le compte d’ordinateur doit être utilisé pour déchiffrer le ticket/jeton Kerberos obtenu à partir d’Active Directory et transféré par le client au serveur afin d’authentifier l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="08cf6-155">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="08cf6-156">Inscrivez le nom de principal du service (SPN) pour l’hôte, et non l’utilisateur de l’application.</span><span class="sxs-lookup"><span data-stu-id="08cf6-156">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="08cf6-157">HTTP.sys n’est pas pris en charge sur Nano Server version 1709 ou ultérieure.</span><span class="sxs-lookup"><span data-stu-id="08cf6-157">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="08cf6-158">Pour utiliser l’authentification Windows et HTTP.sys avec Nano Server, utilisez un [conteneur de Server Core (microsoft/windowsservercore)](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="08cf6-158">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="08cf6-159">Pour plus d’informations sur Server Core, consultez [quel est l’option d’installation Server Core de Windows Server ?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="08cf6-159">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

## <a name="enable-windows-authentication-with-weblistener"></a><span data-ttu-id="08cf6-160">Activer l’authentification Windows avec WebListener</span><span class="sxs-lookup"><span data-stu-id="08cf6-160">Enable Windows Authentication with WebListener</span></span>

<span data-ttu-id="08cf6-161">Bien que Kestrel ne prend pas en charge l’authentification Windows, vous pouvez utiliser [WebListener](xref:fundamentals/servers/weblistener) pour prendre en charge les scénarios auto-hébergés sur Windows.</span><span class="sxs-lookup"><span data-stu-id="08cf6-161">Although Kestrel doesn't support Windows Authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="08cf6-162">L’exemple suivant configure un hôte web de l’application pour utiliser WebListener avec l’authentification Windows :</span><span class="sxs-lookup"><span data-stu-id="08cf6-162">The following example configures the app's web host to use WebListener with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

> [!NOTE]
> <span data-ttu-id="08cf6-163">WebListener délègue l’authentification en mode noyau avec le protocole d’authentification Kerberos.</span><span class="sxs-lookup"><span data-stu-id="08cf6-163">WebListener delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="08cf6-164">L’authentification en mode utilisateur n’est pas prise en charge avec Kerberos et WebListener.</span><span class="sxs-lookup"><span data-stu-id="08cf6-164">User mode authentication isn't supported with Kerberos and WebListener.</span></span> <span data-ttu-id="08cf6-165">Le compte d’ordinateur doit être utilisé pour déchiffrer le ticket/jeton Kerberos obtenu à partir d’Active Directory et transféré par le client au serveur afin d’authentifier l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="08cf6-165">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="08cf6-166">Inscrivez le nom de principal du service (SPN) pour l’hôte, et non l’utilisateur de l’application.</span><span class="sxs-lookup"><span data-stu-id="08cf6-166">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

::: moniker-end

## <a name="work-with-windows-authentication"></a><span data-ttu-id="08cf6-167">Utiliser l’authentification Windows</span><span class="sxs-lookup"><span data-stu-id="08cf6-167">Work with Windows Authentication</span></span>

<span data-ttu-id="08cf6-168">L’état de configuration de l’accès anonyme détermine la façon dont le `[Authorize]` et `[AllowAnonymous]` attributs sont utilisés dans l’application.</span><span class="sxs-lookup"><span data-stu-id="08cf6-168">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="08cf6-169">Les deux sections suivantes expliquent comment gérer les États de configuration non autorisés et autorisées de l’accès anonyme.</span><span class="sxs-lookup"><span data-stu-id="08cf6-169">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="08cf6-170">Interdire l’accès anonyme</span><span class="sxs-lookup"><span data-stu-id="08cf6-170">Disallow anonymous access</span></span>

<span data-ttu-id="08cf6-171">Lorsque l’authentification Windows est activée et que l’accès anonyme est désactivé, le `[Authorize]` et `[AllowAnonymous]` attributs n’ont aucun effet.</span><span class="sxs-lookup"><span data-stu-id="08cf6-171">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="08cf6-172">Si le site IIS (ou serveur HTTP.sys ou WebListener) est configuré pour interdire l’accès anonyme, la demande n’atteint jamais votre application.</span><span class="sxs-lookup"><span data-stu-id="08cf6-172">If the IIS site (or HTTP.sys or WebListener server) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="08cf6-173">Pour cette raison, le `[AllowAnonymous]` attribut n’est pas applicable.</span><span class="sxs-lookup"><span data-stu-id="08cf6-173">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="08cf6-174">Autoriser l’accès anonyme</span><span class="sxs-lookup"><span data-stu-id="08cf6-174">Allow anonymous access</span></span>

<span data-ttu-id="08cf6-175">Lorsque l’authentification Windows et l’accès anonyme sont activées, utiliser le `[Authorize]` et `[AllowAnonymous]` attributs.</span><span class="sxs-lookup"><span data-stu-id="08cf6-175">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="08cf6-176">Le `[Authorize]` attribut vous permet de sécuriser les éléments de l’application qui véritablement nécessitent l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="08cf6-176">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows Authentication.</span></span> <span data-ttu-id="08cf6-177">Le `[AllowAnonymous]` substitutions d’attribut `[Authorize]` attribut l’utilisation dans les applications qui permettent l’accès anonyme.</span><span class="sxs-lookup"><span data-stu-id="08cf6-177">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="08cf6-178">Consultez [autorisation Simple](xref:security/authorization/simple) pour des détails d’utilisation de l’attribut.</span><span class="sxs-lookup"><span data-stu-id="08cf6-178">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="08cf6-179">Dans ASP.NET Core 2.x, le `[Authorize]` attribut nécessite une configuration supplémentaire dans *Startup.cs* en question les demandes anonymes pour l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="08cf6-179">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows Authentication.</span></span> <span data-ttu-id="08cf6-180">La configuration recommandée varie légèrement selon le serveur web utilisé.</span><span class="sxs-lookup"><span data-stu-id="08cf6-180">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="08cf6-181">Par défaut, les utilisateurs qui ne disposent pas d’autorisation pour accéder à une page sont présentées avec une réponse HTTP 403 vide.</span><span class="sxs-lookup"><span data-stu-id="08cf6-181">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="08cf6-182">Le [StatusCodePages intergiciel (middleware)](xref:fundamentals/error-handling#configure-status-code-pages) peut être configuré pour fournir aux utilisateurs une meilleure expérience « Accès refusé ».</span><span class="sxs-lookup"><span data-stu-id="08cf6-182">The [StatusCodePages middleware](xref:fundamentals/error-handling#configure-status-code-pages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="08cf6-183">IIS</span><span class="sxs-lookup"><span data-stu-id="08cf6-183">IIS</span></span>

<span data-ttu-id="08cf6-184">Si vous utilisez IIS, ajoutez le code suivant à la `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="08cf6-184">If using IIS, add the following to the `ConfigureServices` method:</span></span>

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="08cf6-185">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="08cf6-185">HTTP.sys</span></span>

<span data-ttu-id="08cf6-186">Si vous utilisez HTTP.sys, ajoutez le code suivant à la `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="08cf6-186">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="08cf6-187">Emprunt d'identité</span><span class="sxs-lookup"><span data-stu-id="08cf6-187">Impersonation</span></span>

<span data-ttu-id="08cf6-188">ASP.NET Core n’implémente pas l’emprunt d’identité.</span><span class="sxs-lookup"><span data-stu-id="08cf6-188">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="08cf6-189">Applications s’exécutent avec l’identité de l’application pour toutes les demandes, à l’aide d’identité de pool ou les processus de l’application.</span><span class="sxs-lookup"><span data-stu-id="08cf6-189">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="08cf6-190">Si vous avez besoin effectuer explicitement une action de la part d’un utilisateur, utilisez `WindowsIdentity.RunImpersonated`.</span><span class="sxs-lookup"><span data-stu-id="08cf6-190">If you need to explicitly perform an action on behalf of a user, use `WindowsIdentity.RunImpersonated`.</span></span> <span data-ttu-id="08cf6-191">Exécuter une action unique dans ce contexte, puis fermez le contexte.</span><span class="sxs-lookup"><span data-stu-id="08cf6-191">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

<span data-ttu-id="08cf6-192">Notez que `RunImpersonated` ne prend pas en charge les opérations asynchrones et ne doit pas être utilisé pour les scénarios complexes.</span><span class="sxs-lookup"><span data-stu-id="08cf6-192">Note that `RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="08cf6-193">Par exemple, encapsulant les demandes entières ou des chaînes d’intergiciel (middleware) n’est pas pris en charge ni recommandé.</span><span class="sxs-lookup"><span data-stu-id="08cf6-193">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>
