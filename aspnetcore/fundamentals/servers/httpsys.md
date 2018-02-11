---
title: "Implémentation du serveur web HTTP.sys dans ASP.NET Core"
author: rick-anderson
description: "Présente HTTP.sys, serveur web pour ASP.NET Core sur Windows. Basé sur le pilote en mode noyau Http.Sys, HTTP.sys est une alternative à Kestrel qui peut être utilisée pour une connexion directe à Internet sans IIS."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/httpsys
ms.openlocfilehash: f36a86fc67e715165afd0c38992f9767f90cf3b4
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="d8145-104">Implémentation du serveur web HTTP.sys dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d8145-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="d8145-105">Par [Tom Dykstra](https://github.com/tdykstra) et [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="d8145-105">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="d8145-106">Cette rubrique s’applique uniquement à ASP.NET Core 2.0 et ultérieur.</span><span class="sxs-lookup"><span data-stu-id="d8145-106">This topic applies only to ASP.NET Core 2.0 and later.</span></span> <span data-ttu-id="d8145-107">Dans les versions antérieures d’ASP.NET Core, HTTP.sys est appelé [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="d8145-107">In earlier versions of ASP.NET Core, HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener).</span></span>

<span data-ttu-id="d8145-108">HTTP.sys est un [serveur web pour ASP.NET Core](index.md) qui s’exécute uniquement sur Windows.</span><span class="sxs-lookup"><span data-stu-id="d8145-108">HTTP.sys is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="d8145-109">Il repose sur le [pilote en mode noyau Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="d8145-109">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="d8145-110">Offrant certaines fonctionnalités qui lui sont propres, HTTP.sys est une alternative à [Kestrel](kestrel.md).</span><span class="sxs-lookup"><span data-stu-id="d8145-110">HTTP.sys is an alternative to [Kestrel](kestrel.md) that offers some features that Kestel doesn't.</span></span> <span data-ttu-id="d8145-111">**HTTP.sys ne peut pas être utilisé avec IIS ou IIS Express, car il est incompatible avec le [module ASP.NET Core](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="d8145-111">**HTTP.sys can't be used with IIS or IIS Express, as it's incompatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="d8145-112">HTTP.sys prend en charge les fonctionnalités suivantes :</span><span class="sxs-lookup"><span data-stu-id="d8145-112">HTTP.sys supports the following features:</span></span>

- [<span data-ttu-id="d8145-113">Authentification Windows</span><span class="sxs-lookup"><span data-stu-id="d8145-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="d8145-114">Partage de port</span><span class="sxs-lookup"><span data-stu-id="d8145-114">Port sharing</span></span>
- <span data-ttu-id="d8145-115">HTTPS avec SNI</span><span class="sxs-lookup"><span data-stu-id="d8145-115">HTTPS with SNI</span></span>
- <span data-ttu-id="d8145-116">HTTP/2 sur TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="d8145-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="d8145-117">Transmission de fichier directe</span><span class="sxs-lookup"><span data-stu-id="d8145-117">Direct file transmission</span></span>
- <span data-ttu-id="d8145-118">Mise en cache des réponses</span><span class="sxs-lookup"><span data-stu-id="d8145-118">Response caching</span></span>
- <span data-ttu-id="d8145-119">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="d8145-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="d8145-120">Versions Windows prises en charge :</span><span class="sxs-lookup"><span data-stu-id="d8145-120">Supported Windows versions:</span></span>

- <span data-ttu-id="d8145-121">Windows 7 et Windows Server 2008 R2 et ultérieur</span><span class="sxs-lookup"><span data-stu-id="d8145-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="d8145-122">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d8145-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="d8145-123">Quand utiliser HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="d8145-123">When to use HTTP.sys</span></span>

<span data-ttu-id="d8145-124">HTTP.sys est utile pour les déploiements où vous devez exposer le serveur directement à Internet sans l’aide d’IIS.</span><span class="sxs-lookup"><span data-stu-id="d8145-124">HTTP.sys is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![HTTP.sys communique directement avec Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="d8145-126">Comme il repose sur Http.Sys, HTTP.sys ne nécessite pas de serveur proxy inverse en guise de protection contre les attaques.</span><span class="sxs-lookup"><span data-stu-id="d8145-126">Because it's built on Http.Sys, HTTP.sys doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="d8145-127">Http.Sys est une technologie en pleine maturité qui offre une protection contre de nombreux types d’attaques et fournit la fiabilité, la sécurité et la scalabilité d’un serveur web complet.</span><span class="sxs-lookup"><span data-stu-id="d8145-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="d8145-128">Pour sa part, IIS s’exécute en tant qu’écouteur HTTP sur Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="d8145-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="d8145-129">HTTP.sys est un bon choix pour les déploiements internes quand vous avez besoin d’une fonctionnalité qui n’est pas disponible dans Kestrel, telle que l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="d8145-129">HTTP.sys is a good choice for internal deployments when you need a feature not available in Kestrel, such as Windows authentication.</span></span>

![HTTP.sys communique directement avec votre réseau interne](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a><span data-ttu-id="d8145-131">Comment utiliser HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="d8145-131">How to use HTTP.sys</span></span>

<span data-ttu-id="d8145-132">Voici une vue d’ensemble des tâches de configuration pour le système d’exploitation hôte et votre application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d8145-132">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="d8145-133">Configurer Windows Server</span><span class="sxs-lookup"><span data-stu-id="d8145-133">Configure Windows Server</span></span>

* <span data-ttu-id="d8145-134">Installez la version de .NET requise par votre application, telle que [.NET Core](https://www.microsoft.com/net/download/core) ou [.NET Framework](https://www.microsoft.com/net/download/framework).</span><span class="sxs-lookup"><span data-stu-id="d8145-134">Install the version of .NET that your application requires, such as [.NET Core](https://www.microsoft.com/net/download/core) or [.NET Framework](https://www.microsoft.com/net/download/framework).</span></span>

* <span data-ttu-id="d8145-135">Préenregistrez les préfixes d’URL à lier à HTTP.sys et configurez les certificats SSL.</span><span class="sxs-lookup"><span data-stu-id="d8145-135">Preregister URL prefixes to bind to HTTP.sys, and set up SSL certificates</span></span>

   <span data-ttu-id="d8145-136">Si vous ne préenregistrez pas de préfixes d’URL dans Windows, vous devez exécuter votre application avec des privilèges d’administrateur.</span><span class="sxs-lookup"><span data-stu-id="d8145-136">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="d8145-137">C’est nécessaire, sauf si vous effectuez une liaison à localhost à l’aide de HTTP (pas HTTPS) avec un numéro de port supérieur à 1024.</span><span class="sxs-lookup"><span data-stu-id="d8145-137">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case, administrator privileges aren't required.</span></span>

   <span data-ttu-id="d8145-138">Pour plus d’informations, consultez [Préenregistrer les préfixes d’URL et configurer SSL](#preregister-url-prefixes-and-configure-ssl) plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="d8145-138">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="d8145-139">Ouvrez des ports du pare-feu pour autoriser le trafic à atteindre HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="d8145-139">Open firewall ports to allow traffic to reach HTTP.sys.</span></span>

   <span data-ttu-id="d8145-140">Vous pouvez utiliser *netsh.exe* ou des [applets de commande PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="d8145-140">You can use *netsh.exe* or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="d8145-141">Vous pouvez également recourir aux [paramètres de Registre Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="d8145-141">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a><span data-ttu-id="d8145-142">Configurer votre application ASP.NET Core pour qu’elle utilise HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="d8145-142">Configure your ASP.NET Core application to use HTTP.sys</span></span>

* <span data-ttu-id="d8145-143">Aucune installation de package n’est nécessaire si vous utilisez le métapackage [Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="d8145-143">No package install is needed if you use the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="d8145-144">Le package [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) est inclus dans le métapackage.</span><span class="sxs-lookup"><span data-stu-id="d8145-144">The [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) package is included in the metapackage.</span></span>

* <span data-ttu-id="d8145-145">Appelez la méthode d’extension `UseHttpSys` sur `WebHostBuilder` dans votre méthode `Main`, en spécifiant les [options HTTP.sys](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) dont vous avez besoin, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="d8145-145">Call the `UseHttpSys` extension method on `WebHostBuilder` in your `Main` method, specifying any [HTTP.sys options](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a><span data-ttu-id="d8145-146">Configurer les options HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="d8145-146">Configure HTTP.sys options</span></span>

<span data-ttu-id="d8145-147">Voici certains paramètres et limites HTTP.sys que vous pouvez configurer.</span><span class="sxs-lookup"><span data-stu-id="d8145-147">Here are some of the HTTP.sys settings and limits that you can configure.</span></span>

<span data-ttu-id="d8145-148">**Nombre maximal de connexions client**</span><span class="sxs-lookup"><span data-stu-id="d8145-148">**Maximum client connections**</span></span>

<span data-ttu-id="d8145-149">Le nombre maximal de connexions TCP ouvertes simultanées peut être défini pour l’application entière avec le code suivant dans *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="d8145-149">The maximum number of concurrent open TCP connections can be set for the entire application with the following code in *Program.cs*:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

<span data-ttu-id="d8145-150">Le nombre maximal de connexions est illimité (null) par défaut.</span><span class="sxs-lookup"><span data-stu-id="d8145-150">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="d8145-151">**Taille maximale du corps de la demande**</span><span class="sxs-lookup"><span data-stu-id="d8145-151">**Maximum request body size**</span></span>

<span data-ttu-id="d8145-152">La taille maximale par défaut du corps de la demande est de 30 000 000 octets, soit environ 28,6 Mo.</span><span class="sxs-lookup"><span data-stu-id="d8145-152">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span>

<span data-ttu-id="d8145-153">Pour remplacer la limite dans une application MVC ASP.NET Core, nous vous recommandons d’utiliser l’attribut [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) sur une méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="d8145-153">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="d8145-154">Voici un exemple qui montre comment configurer la contrainte pour l’application entière et chaque demande :</span><span class="sxs-lookup"><span data-stu-id="d8145-154">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

<span data-ttu-id="d8145-155">Vous pouvez remplacer le paramètre sur une demande spécifique dans *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="d8145-155">You can override the setting on a specific request in *Startup.cs*:</span></span>

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
<span data-ttu-id="d8145-156">Une exception est levée si vous essayez de configurer la limite sur une demande dont l’application a commencé la lecture.</span><span class="sxs-lookup"><span data-stu-id="d8145-156">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="d8145-157">Il existe une propriété `IsReadOnly` qui indique si la propriété `MaxRequestBodySize` est en lecture seule ; si tel est le cas, il est trop tard pour configurer la limite.</span><span class="sxs-lookup"><span data-stu-id="d8145-157">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="d8145-158">Pour plus d’informations sur les autres options de HTTP.sys, consultez [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="d8145-158">For information about other HTTP.sys options, see [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> 

### <a name="configure-urls-and-ports-to-listen-on"></a><span data-ttu-id="d8145-159">Configurer les ports et les URL sur lesquels écouter</span><span class="sxs-lookup"><span data-stu-id="d8145-159">Configure URLs and ports to listen on</span></span> 

<span data-ttu-id="d8145-160">Par défaut, ASP.NET Core est lié à `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d8145-160">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="d8145-161">Pour configurer les ports et les préfixes d’URL, vous pouvez utiliser la méthode d’extension `UseUrls`, l’argument de ligne de commande `urls`, la variable d’environnement ASPNETCORE_URLS ou la propriété `UrlPrefixes` sur [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="d8145-161">To configure URL prefixes and ports, you can use the `UseUrls` extension method, the `urls` command-line argument, the ASPNETCORE_URLS environment variable, or the `UrlPrefixes` property on [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> <span data-ttu-id="d8145-162">L’exemple de code suivant utilise `UrlPrefixes`.</span><span class="sxs-lookup"><span data-stu-id="d8145-162">The following code example uses `UrlPrefixes`.</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

<span data-ttu-id="d8145-163">Grâce à `UrlPrefixes`, vous obtenez un message d’erreur immédiatement si vous essayez d’ajouter un préfixe au format incorrect.</span><span class="sxs-lookup"><span data-stu-id="d8145-163">An advantage of `UrlPrefixes` is that you get an error message immediately if you try to add a prefix that's formatted wrong.</span></span> <span data-ttu-id="d8145-164">Pour sa part, `UseUrls` (partagé avec `urls` et ASPNETCORE_URLS) vous permet de basculer plus facilement entre Kestrel et HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="d8145-164">An advantage of `UseUrls` (shared with `urls` and ASPNETCORE_URLS) is that you can more easily switch between Kestrel and HTTP.sys.</span></span>

<span data-ttu-id="d8145-165">Si vous utilisez à la fois `UseUrls` (ou `urls` ou ASPNETCORE_URLS) et `UrlPrefixes`, les paramètres dans `UrlPrefixes` remplacent ceux dans `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="d8145-165">If you use both `UseUrls` (or `urls` or ASPNETCORE_URLS) and `UrlPrefixes`, the settings in `UrlPrefixes` override the ones in `UseUrls`.</span></span> <span data-ttu-id="d8145-166">Pour plus d’informations, consultez [Hébergement](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="d8145-166">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

<span data-ttu-id="d8145-167">HTTP.sys utilise les [formats de chaîne UrlPrefix de l’API de serveur HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="d8145-167">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="d8145-168">Veillez à spécifier dans `UseUrls` ou `UrlPrefixes` les mêmes chaînes de préfixe que celles que vous préenregistrez sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="d8145-168">Make sure that you specify the same prefix strings in `UseUrls` or `UrlPrefixes` that you preregister on the server.</span></span> 

### <a name="dont-use-iis"></a><span data-ttu-id="d8145-169">Ne pas utiliser IIS</span><span class="sxs-lookup"><span data-stu-id="d8145-169">Don't use IIS</span></span>

<span data-ttu-id="d8145-170">Vérifiez que votre application n’est pas configurée pour exécuter IIS ou IIS Express.</span><span class="sxs-lookup"><span data-stu-id="d8145-170">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="d8145-171">Dans Visual Studio, le profil de démarrage par défaut est destiné à IIS Express.</span><span class="sxs-lookup"><span data-stu-id="d8145-171">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="d8145-172">Pour exécuter le projet en tant qu’application console, changez manuellement le profil sélectionné, comme indiqué dans la capture d’écran suivante.</span><span class="sxs-lookup"><span data-stu-id="d8145-172">To run the project as a console application, manually change the selected profile, as shown in the following screen shot.</span></span>

![Sélectionner le profil d’application console](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="d8145-174">Préenregistrer les préfixes d’URL et configurer SSL</span><span class="sxs-lookup"><span data-stu-id="d8145-174">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="d8145-175">IIS et HTTP.sys s’appuient sur le pilote en mode noyau Http.Sys sous-jacent pour écouter les demandes et effectuer le traitement initial.</span><span class="sxs-lookup"><span data-stu-id="d8145-175">Both IIS and HTTP.sys rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="d8145-176">Dans IIS, l’interface utilisateur de gestion vous permet de tout configurer avec une certaine aisance.</span><span class="sxs-lookup"><span data-stu-id="d8145-176">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="d8145-177">Toutefois, vous devez configurer Http.Sys vous-même.</span><span class="sxs-lookup"><span data-stu-id="d8145-177">However, you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="d8145-178">Pour effectuer cette opération, vous disposez de l’outil intégré *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="d8145-178">The built-in tool for doing that's *netsh.exe*.</span></span> 

<span data-ttu-id="d8145-179">Avec *netsh.exe*, vous pouvez réserver les préfixes d’URL et assigner les certificats SSL.</span><span class="sxs-lookup"><span data-stu-id="d8145-179">With *netsh.exe* you can reserve URL prefixes and assign SSL certificates.</span></span> <span data-ttu-id="d8145-180">L’outil exige des privilèges d’administrateur.</span><span class="sxs-lookup"><span data-stu-id="d8145-180">The tool requires administrative privileges.</span></span>

<span data-ttu-id="d8145-181">L’exemple suivant montre le minimum nécessaire pour réserver des préfixes d’URL pour les ports 80 et 443 :</span><span class="sxs-lookup"><span data-stu-id="d8145-181">The following example shows the minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="d8145-182">L’exemple suivant montre comment assigner un certificat SSL :</span><span class="sxs-lookup"><span data-stu-id="d8145-182">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

<span data-ttu-id="d8145-183">Voici la documentation de référence de *netsh.exe* :</span><span class="sxs-lookup"><span data-stu-id="d8145-183">Here is the reference documentation for *netsh.exe*:</span></span>

* [<span data-ttu-id="d8145-184">Commandes netsh pour HTTP (Hypertext Transfer Protocol)</span><span class="sxs-lookup"><span data-stu-id="d8145-184">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="d8145-185">Chaînes UrlPrefix</span><span class="sxs-lookup"><span data-stu-id="d8145-185">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="d8145-186">Les ressources suivantes fournissent des instructions détaillées pour plusieurs scénarios.</span><span class="sxs-lookup"><span data-stu-id="d8145-186">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="d8145-187">Les articles qui font référence à HttpListener s’appliquent aussi à HTTP.sys, tous deux reposant sur Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="d8145-187">Articles that refer to HttpListener apply equally to HTTP.sys, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="d8145-188">Guide pratique pour configurer un port avec un certificat SSL</span><span class="sxs-lookup"><span data-stu-id="d8145-188">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="d8145-189">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) Blog tiers et assez ancien, mais qui contient néanmoins des informations utiles.</span><span class="sxs-lookup"><span data-stu-id="d8145-189">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="d8145-190">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) Autre blog ancien comportant des informations utiles.</span><span class="sxs-lookup"><span data-stu-id="d8145-190">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>

<span data-ttu-id="d8145-191">Voici certains outils tiers qui peuvent être plus faciles à utiliser que la ligne de commande *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="d8145-191">Here are some third-party tools that can be easier to use than the *netsh.exe* command line.</span></span> <span data-ttu-id="d8145-192">Ils ne sont pas fournis ou recommandés par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d8145-192">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="d8145-193">Les outils s’exécutent en tant qu’administrateur par défaut, étant donné que *netsh.exe* lui-même nécessite des privilèges d’administrateur.</span><span class="sxs-lookup"><span data-stu-id="d8145-193">The tools run as administrator by default, since *netsh.exe* itself requires administrator privileges.</span></span>

* <span data-ttu-id="d8145-194">[HTTP.sys Manager](http://httpsysmanager.codeplex.com/), dont l’interface utilisateur permet de répertorier et de configurer les options et certificats SSL, les réservations de préfixe et les listes de certificats de confiance.</span><span class="sxs-lookup"><span data-stu-id="d8145-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="d8145-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) vous permet de répertorier ou de configurer les certificats SSL et les préfixes d’URL.</span><span class="sxs-lookup"><span data-stu-id="d8145-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="d8145-196">L’interface utilisateur est plus précise que celle de http.sys Manager et offre quelques options de configuration supplémentaires ; sinon, elle fournit des fonctionnalités similaires.</span><span class="sxs-lookup"><span data-stu-id="d8145-196">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="d8145-197">Cet outil ne peut pas créer de liste de certificats de confiance, mais peut assigner des listes existantes.</span><span class="sxs-lookup"><span data-stu-id="d8145-197">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a><span data-ttu-id="d8145-198">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="d8145-198">Next steps</span></span>

<span data-ttu-id="d8145-199">Pour plus d'informations, reportez-vous aux ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="d8145-199">For more information, see the following resources:</span></span>

* [<span data-ttu-id="d8145-200">Exemple d’application pour cet article</span><span class="sxs-lookup"><span data-stu-id="d8145-200">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [<span data-ttu-id="d8145-201">Code source HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="d8145-201">HTTP.sys source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="d8145-202">Hébergement d’applications WPF</span><span class="sxs-lookup"><span data-stu-id="d8145-202">Hosting</span></span>](xref:fundamentals/hosting)
