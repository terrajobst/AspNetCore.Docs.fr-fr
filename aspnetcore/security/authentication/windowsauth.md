---
title: Configurer l’authentification Windows dans ASP.NET Core
author: ardalis
description: Cet article décrit comment configurer l’authentification Windows dans ASP.NET Core, à l’aide d’IIS Express, IIS, HTTP.sys et WebListener.
manager: wpickett
ms.author: riande
ms.date: 10/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/windowsauth
ms.openlocfilehash: dbcef095561fe656bdd28c4fa6560c6b269a2db0
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2018
ms.locfileid: "34689007"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="f318c-103">Configurer l’authentification Windows dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f318c-103">Configure Windows authentication in ASP.NET Core</span></span>

<span data-ttu-id="f318c-104">Par [Steve Smith](https://ardalis.com) et [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="f318c-104">By [Steve Smith](https://ardalis.com) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="f318c-105">L’authentification Windows peut être configurée pour les applications ASP.NET Core hébergées par IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), ou [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="f318c-105">Windows authentication can be configured for ASP.NET Core apps hosted with IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), or [WebListener](xref:fundamentals/servers/weblistener).</span></span>

## <a name="what-is-windows-authentication"></a><span data-ttu-id="f318c-106">Qu’est l’authentification Windows ?</span><span class="sxs-lookup"><span data-stu-id="f318c-106">What is Windows authentication?</span></span>

<span data-ttu-id="f318c-107">L’authentification Windows s’appuie sur le système d’exploitation pour authentifier les utilisateurs d’applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f318c-107">Windows authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="f318c-108">Vous pouvez utiliser l’authentification Windows lorsque votre serveur s’exécute sur un réseau d’entreprise à l’aide des identités de domaine Active Directory ou d’autres comptes Windows pour identifier les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="f318c-108">You can use Windows authentication when your server runs on a corporate network using Active Directory domain identities or other Windows accounts to identify users.</span></span> <span data-ttu-id="f318c-109">L’authentification Windows est idéale pour les environnements intranet dans lequel les utilisateurs, les applications clientes et les serveurs web appartiennent au même domaine Windows.</span><span class="sxs-lookup"><span data-stu-id="f318c-109">Windows authentication is best suited to intranet environments in which users, client applications, and web servers belong to the same Windows domain.</span></span>

<span data-ttu-id="f318c-110">[En savoir plus sur l’authentification Windows et l’installation pour IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="f318c-110">[Learn more about Windows authentication and installing it for IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="f318c-111">Activer l’authentification Windows dans une application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f318c-111">Enable Windows authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="f318c-112">Le modèle d’Application Web de Visual Studio peut être configuré pour prendre en charge l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="f318c-112">The Visual Studio Web Application template can be configured to support Windows authentication.</span></span>

### <a name="use-the-windows-authentication-app-template"></a><span data-ttu-id="f318c-113">Utilisez le modèle d’application de l’authentification Windows</span><span class="sxs-lookup"><span data-stu-id="f318c-113">Use the Windows authentication app template</span></span>

<span data-ttu-id="f318c-114">Dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="f318c-114">In Visual Studio:</span></span>
1. <span data-ttu-id="f318c-115">Créez une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f318c-115">Create a new ASP.NET Core Web Application.</span></span> 
1. <span data-ttu-id="f318c-116">Sélectionnez l’Application Web à partir de la liste des modèles.</span><span class="sxs-lookup"><span data-stu-id="f318c-116">Select Web Application from the list of templates.</span></span>
1. <span data-ttu-id="f318c-117">Sélectionnez le **modifier l’authentification** sélectionnez **l’authentification Windows**.</span><span class="sxs-lookup"><span data-stu-id="f318c-117">Select the **Change Authentication** button and select **Windows Authentication**.</span></span> 

<span data-ttu-id="f318c-118">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="f318c-118">Run the app.</span></span> <span data-ttu-id="f318c-119">Le nom d’utilisateur s’affiche dans le coin supérieur droit de l’application.</span><span class="sxs-lookup"><span data-stu-id="f318c-119">The username appears in the top right of the app.</span></span>

![Capture d’écran de navigateur de l’authentification Windows](windowsauth/_static/browser-screenshot.png)

<span data-ttu-id="f318c-121">Pour le travail de développement à l’aide d’IIS Express, le modèle fournit la configuration nécessaire pour utiliser l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="f318c-121">For development work using IIS Express, the template provides all the configuration necessary to use Windows authentication.</span></span> <span data-ttu-id="f318c-122">La section suivante montre comment configurer manuellement une application ASP.NET Core pour l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="f318c-122">The following section shows how to manually configure an ASP.NET Core app for Windows authentication.</span></span>

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a><span data-ttu-id="f318c-123">Paramétrage de Visual Studio pour Windows et l’authentification anonyme</span><span class="sxs-lookup"><span data-stu-id="f318c-123">Visual Studio settings for Windows and anonymous authentication</span></span>

<span data-ttu-id="f318c-124">Le projet Visual Studio **propriétés** la page **déboguer** onglet fournit des cases à cocher pour l’authentification Windows et l’authentification anonyme.</span><span class="sxs-lookup"><span data-stu-id="f318c-124">The Visual Studio project **Properties** page's **Debug** tab provides check boxes for Windows authentication and anonymous authentication.</span></span>

![Capture d’écran de navigateur de l’authentification Windows](windowsauth/_static/vs-auth-property-menu.png)

<span data-ttu-id="f318c-126">Vous pouvez également ces deux propriétés peuvent être configurées dans le *launchSettings.json* fichier :</span><span class="sxs-lookup"><span data-stu-id="f318c-126">Alternatively, these two properties can be configured in the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="f318c-127">Activer l’authentification Windows avec IIS</span><span class="sxs-lookup"><span data-stu-id="f318c-127">Enable Windows authentication with IIS</span></span>

<span data-ttu-id="f318c-128">IIS utilise le [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) pour les applications ASP.NET Core hôte.</span><span class="sxs-lookup"><span data-stu-id="f318c-128">IIS uses the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="f318c-129">Le module permet l’authentification Windows à circuler vers IIS par défaut.</span><span class="sxs-lookup"><span data-stu-id="f318c-129">The module allows Windows authentication to flow to IIS by default.</span></span> <span data-ttu-id="f318c-130">L’authentification Windows est configurée dans IIS, pas l’application.</span><span class="sxs-lookup"><span data-stu-id="f318c-130">Windows authentication is configured in IIS, not the app.</span></span> <span data-ttu-id="f318c-131">Les sections suivantes montrent comment utiliser le Gestionnaire des services Internet pour configurer une application ASP.NET Core pour utiliser l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="f318c-131">The following sections show how to use IIS Manager to configure an ASP.NET Core app to use Windows authentication.</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="f318c-132">Créer un nouveau site IIS</span><span class="sxs-lookup"><span data-stu-id="f318c-132">Create a new IIS site</span></span>

<span data-ttu-id="f318c-133">Spécifiez un nom et un dossier et lui permettre de créer un nouveau pool d’applications.</span><span class="sxs-lookup"><span data-stu-id="f318c-133">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="customize-authentication"></a><span data-ttu-id="f318c-134">Personnaliser l’authentification</span><span class="sxs-lookup"><span data-stu-id="f318c-134">Customize authentication</span></span>

<span data-ttu-id="f318c-135">Ouvrez le menu de l’authentification pour le site.</span><span class="sxs-lookup"><span data-stu-id="f318c-135">Open the Authentication menu for the site.</span></span>

![Menu de l’authentification IIS](windowsauth/_static/iis-authentication-menu.png)

<span data-ttu-id="f318c-137">Désactivez l’authentification anonyme et activer l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="f318c-137">Disable Anonymous Authentication and enable Windows Authentication.</span></span>

![Paramètres d’authentification IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a><span data-ttu-id="f318c-139">Publier votre projet dans le dossier de site IIS</span><span class="sxs-lookup"><span data-stu-id="f318c-139">Publish your project to the IIS site folder</span></span>

<span data-ttu-id="f318c-140">À l’aide de Visual Studio ou l’interface de ligne de base de .NET, de publier l’application dans le dossier de destination.</span><span class="sxs-lookup"><span data-stu-id="f318c-140">Using Visual Studio or the .NET Core CLI, publish the app to the destination folder.</span></span>

![Boîte de dialogue Publier de Visual Studio](windowsauth/_static/vs-publish-app.png)

<span data-ttu-id="f318c-142">En savoir plus sur [publication sur IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="f318c-142">Learn more about [publishing to IIS](xref:host-and-deploy/iis/index).</span></span>

<span data-ttu-id="f318c-143">Lancez l’application pour vérifier l’utilisation de l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="f318c-143">Launch the app to verify Windows authentication is working.</span></span>

## <a name="enable-windows-authentication-with-httpsys-or-weblistener"></a><span data-ttu-id="f318c-144">Activer l’authentification Windows avec HTTP.sys ou WebListener</span><span class="sxs-lookup"><span data-stu-id="f318c-144">Enable Windows authentication with HTTP.sys or WebListener</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f318c-145">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f318c-145">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="f318c-146">Bien que Kestrel ne prend pas en charge l’authentification Windows, vous pouvez utiliser [HTTP.sys](xref:fundamentals/servers/httpsys) pour prendre en charge les scénarios auto-hébergés sur Windows.</span><span class="sxs-lookup"><span data-stu-id="f318c-146">Although Kestrel doesn't support Windows authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="f318c-147">L’exemple suivant configure l’hôte web de l’application pour utiliser HTTP.sys avec l’authentification Windows :</span><span class="sxs-lookup"><span data-stu-id="f318c-147">The following example configures the app's web host to use HTTP.sys with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f318c-148">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f318c-148">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="f318c-149">Bien que Kestrel ne prend pas en charge l’authentification Windows, vous pouvez utiliser [WebListener](xref:fundamentals/servers/weblistener) pour prendre en charge les scénarios auto-hébergés sur Windows.</span><span class="sxs-lookup"><span data-stu-id="f318c-149">Although Kestrel doesn't support Windows authentication, you can use [WebListener](xref:fundamentals/servers/weblistener) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="f318c-150">L’exemple suivant configure l’hôte web de l’application pour utiliser WebListener avec l’authentification Windows :</span><span class="sxs-lookup"><span data-stu-id="f318c-150">The following example configures the app's web host to use WebListener with Windows authentication:</span></span>

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

---

## <a name="work-with-windows-authentication"></a><span data-ttu-id="f318c-151">Utiliser l’authentification Windows</span><span class="sxs-lookup"><span data-stu-id="f318c-151">Work with Windows authentication</span></span>

<span data-ttu-id="f318c-152">L’état de configuration de l’accès anonyme détermine la façon dont le `[Authorize]` et `[AllowAnonymous]` attributs sont utilisés dans l’application.</span><span class="sxs-lookup"><span data-stu-id="f318c-152">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="f318c-153">Les deux sections suivantes expliquent comment gérer les États de configuration non autorisés et autorisées de l’accès anonyme.</span><span class="sxs-lookup"><span data-stu-id="f318c-153">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="f318c-154">Interdire l’accès anonyme</span><span class="sxs-lookup"><span data-stu-id="f318c-154">Disallow anonymous access</span></span>

<span data-ttu-id="f318c-155">Lorsque l’authentification Windows est activée et l’accès anonyme est désactivé, le `[Authorize]` et `[AllowAnonymous]` attributs n’ont aucun effet.</span><span class="sxs-lookup"><span data-stu-id="f318c-155">When Windows authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="f318c-156">Si le site IIS (ou serveur HTTP.sys ou WebListener) est configuré pour interdire l’accès anonyme, la demande n’atteint jamais votre application.</span><span class="sxs-lookup"><span data-stu-id="f318c-156">If the IIS site (or HTTP.sys or WebListener server) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="f318c-157">Pour cette raison, le `[AllowAnonymous]` attribut n’est pas applicable.</span><span class="sxs-lookup"><span data-stu-id="f318c-157">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="f318c-158">Autoriser l’accès anonyme</span><span class="sxs-lookup"><span data-stu-id="f318c-158">Allow anonymous access</span></span>

<span data-ttu-id="f318c-159">Lorsque l’authentification Windows et l’accès anonyme sont activées, utiliser le `[Authorize]` et `[AllowAnonymous]` attributs.</span><span class="sxs-lookup"><span data-stu-id="f318c-159">When both Windows authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="f318c-160">Le `[Authorize]` attribut vous permet de sécuriser des parties de l’application qui a réellement requièrent l’authentification de Windows.</span><span class="sxs-lookup"><span data-stu-id="f318c-160">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows authentication.</span></span> <span data-ttu-id="f318c-161">Le `[AllowAnonymous]` les substitutions d’attributs `[Authorize]` attribut d’utilisation dans les applications qui autorisent l’accès anonyme.</span><span class="sxs-lookup"><span data-stu-id="f318c-161">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="f318c-162">Consultez [une autorisation Simple](xref:security/authorization/simple) pour des détails d’utilisation de l’attribut.</span><span class="sxs-lookup"><span data-stu-id="f318c-162">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="f318c-163">Dans ASP.NET Core 2.x, le `[Authorize]` attribut nécessite une configuration supplémentaire dans *Startup.cs* contester les demandes anonymes pour l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="f318c-163">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows authentication.</span></span> <span data-ttu-id="f318c-164">La configuration recommandée varie légèrement selon le serveur web utilisé.</span><span class="sxs-lookup"><span data-stu-id="f318c-164">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="f318c-165">Par défaut, les utilisateurs qui ne disposent pas d’autorisation pour accéder à une page sont présentées avec une réponse HTTP 403 vide.</span><span class="sxs-lookup"><span data-stu-id="f318c-165">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="f318c-166">Le [StatusCodePages intergiciel (middleware)](xref:fundamentals/error-handling#configuring-status-code-pages) peut être configuré pour fournir aux utilisateurs une meilleure expérience « Accès refusé ».</span><span class="sxs-lookup"><span data-stu-id="f318c-166">The [StatusCodePages middleware](xref:fundamentals/error-handling#configuring-status-code-pages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="f318c-167">IIS</span><span class="sxs-lookup"><span data-stu-id="f318c-167">IIS</span></span>

<span data-ttu-id="f318c-168">Si vous utilisez IIS, ajoutez le code suivant à la `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="f318c-168">If using IIS, add the following to the `ConfigureServices` method:</span></span> 

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="f318c-169">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="f318c-169">HTTP.sys</span></span>

<span data-ttu-id="f318c-170">Si l’aide de HTTP.sys, ajoutez le code suivant à la `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="f318c-170">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="f318c-171">Emprunt d'identité</span><span class="sxs-lookup"><span data-stu-id="f318c-171">Impersonation</span></span>

<span data-ttu-id="f318c-172">ASP.NET Core n’implémente pas l’emprunt d’identité.</span><span class="sxs-lookup"><span data-stu-id="f318c-172">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="f318c-173">Les applications s’exécutent avec l’identité de l’application pour toutes les demandes, à l’aide d’identité de processus ou pool d’application.</span><span class="sxs-lookup"><span data-stu-id="f318c-173">Apps run with the application identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="f318c-174">Si vous avez besoin effectuer explicitement une action de la part d’un utilisateur, utilisez `WindowsIdentity.RunImpersonated`.</span><span class="sxs-lookup"><span data-stu-id="f318c-174">If you need to explicitly perform an action on behalf of a user, use `WindowsIdentity.RunImpersonated`.</span></span> <span data-ttu-id="f318c-175">Exécuter une action unique dans ce contexte, puis fermez le contexte.</span><span class="sxs-lookup"><span data-stu-id="f318c-175">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

<span data-ttu-id="f318c-176">Notez que `RunImpersonated` ne prend pas en charge les opérations asynchrones et ne doivent pas être utilisés pour des scénarios complexes.</span><span class="sxs-lookup"><span data-stu-id="f318c-176">Note that `RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="f318c-177">Par exemple, habillage demandes entières ou des chaînes d’intergiciel (middleware) n’est pas pris en charge ou recommandé.</span><span class="sxs-lookup"><span data-stu-id="f318c-177">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>

---
