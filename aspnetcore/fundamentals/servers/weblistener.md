---
title: Implémentation du serveur web WebListener dans ASP.NET Core
author: rick-anderson
description: Découvrez plus d’informations sur WebListener, un serveur web pour ASP.NET Core sur Windows, qui peut être utilisé pour une connexion directe à Internet sans IIS.
manager: wpickett
ms.author: riande
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/weblistener
ms.openlocfilehash: d40243454632550147a7d42ab26a8f1d2d100db2
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2018
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="e10af-103">Implémentation du serveur web WebListener dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e10af-103">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="e10af-104">Par [Tom Dykstra](https://github.com/tdykstra) et [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="e10af-104">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="e10af-105">Cette rubrique s’applique uniquement à ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="e10af-105">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="e10af-106">Dans ASP.NET Core 2.0, WebListener est nommé [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="e10af-106">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="e10af-107">WebListener est un [serveur web pour ASP.NET Core](index.md) qui s’exécute uniquement sur Windows.</span><span class="sxs-lookup"><span data-stu-id="e10af-107">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="e10af-108">Il repose sur le [pilote en mode noyau Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="e10af-108">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="e10af-109">WebListener est une alternative à [Kestrel](kestrel.md) qui peut être utilisée pour une connexion directe à Internet sans recourir à IIS comme serveur proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="e10af-109">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="e10af-110">En fait, **WebListener ne peut pas être utilisé avec IIS ou IIS Express, car il n’est pas compatible avec le [module ASP.NET Core](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="e10af-110">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="e10af-111">Bien que WebListener ait été développé pour ASP.NET Core, il peut être utilisé directement dans n’importe quelle application .NET Core ou .NET Framework par le biais du package NuGet [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).</span><span class="sxs-lookup"><span data-stu-id="e10af-111">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="e10af-112">WebListener prend en charge les fonctionnalités suivantes :</span><span class="sxs-lookup"><span data-stu-id="e10af-112">WebListener supports the following features:</span></span>

- [<span data-ttu-id="e10af-113">Authentification Windows</span><span class="sxs-lookup"><span data-stu-id="e10af-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="e10af-114">Partage de port</span><span class="sxs-lookup"><span data-stu-id="e10af-114">Port sharing</span></span>
- <span data-ttu-id="e10af-115">HTTPS avec SNI</span><span class="sxs-lookup"><span data-stu-id="e10af-115">HTTPS with SNI</span></span>
- <span data-ttu-id="e10af-116">HTTP/2 sur TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="e10af-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="e10af-117">Transmission de fichier directe</span><span class="sxs-lookup"><span data-stu-id="e10af-117">Direct file transmission</span></span>
- <span data-ttu-id="e10af-118">Mise en cache des réponses</span><span class="sxs-lookup"><span data-stu-id="e10af-118">Response caching</span></span>
- <span data-ttu-id="e10af-119">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="e10af-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="e10af-120">Versions Windows prises en charge :</span><span class="sxs-lookup"><span data-stu-id="e10af-120">Supported Windows versions:</span></span>

- <span data-ttu-id="e10af-121">Windows 7 et Windows Server 2008 R2 et ultérieur</span><span class="sxs-lookup"><span data-stu-id="e10af-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="e10af-122">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e10af-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="e10af-123">Quand utiliser WebListener</span><span class="sxs-lookup"><span data-stu-id="e10af-123">When to use WebListener</span></span>

<span data-ttu-id="e10af-124">WebListener est utile pour les déploiements où vous devez exposer le serveur directement à Internet sans l’aide d’IIS.</span><span class="sxs-lookup"><span data-stu-id="e10af-124">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![WebListener communique directement avec Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="e10af-126">Comme il repose sur Http.Sys, WebListener ne nécessite pas un serveur proxy inverse en guise de protection contre les attaques.</span><span class="sxs-lookup"><span data-stu-id="e10af-126">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="e10af-127">Http.Sys est une technologie en pleine maturité qui offre une protection contre de nombreux types d’attaques et fournit la fiabilité, la sécurité et la scalabilité d’un serveur web complet.</span><span class="sxs-lookup"><span data-stu-id="e10af-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="e10af-128">Pour sa part, IIS s’exécute en tant qu’écouteur HTTP sur Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="e10af-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="e10af-129">WebListener est également un bon choix pour les déploiements internes si vous avez besoin d’utiliser une de ses fonctionnalités et que cette fonctionnalité n’est pas disponible avec Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e10af-129">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![WebListener communique directement avec votre réseau interne](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a><span data-ttu-id="e10af-131">Comment utiliser WebListener</span><span class="sxs-lookup"><span data-stu-id="e10af-131">How to use WebListener</span></span>

<span data-ttu-id="e10af-132">Voici une vue d’ensemble des tâches de configuration pour le système d’exploitation hôte et votre application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e10af-132">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="e10af-133">Configurer Windows Server</span><span class="sxs-lookup"><span data-stu-id="e10af-133">Configure Windows Server</span></span>

* <span data-ttu-id="e10af-134">Installez la version de .NET requise par votre application, telle que [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) ou .NET Framework 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="e10af-134">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="e10af-135">Préenregistrez les préfixes d’URL à lier à WebListener et configurez les certificats SSL.</span><span class="sxs-lookup"><span data-stu-id="e10af-135">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="e10af-136">Si vous ne préenregistrez pas de préfixes d’URL dans Windows, vous devez exécuter votre application avec des privilèges d’administrateur.</span><span class="sxs-lookup"><span data-stu-id="e10af-136">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="e10af-137">Cela est nécessaire, sauf si vous effectuez une liaison à localhost à l’aide de HTTP (pas HTTPS) avec un numéro de port supérieur à 1024.</span><span class="sxs-lookup"><span data-stu-id="e10af-137">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="e10af-138">Pour plus d’informations, consultez [Préenregistrer les préfixes d’URL et configurer SSL](#preregister-url-prefixes-and-configure-ssl) plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="e10af-138">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="e10af-139">Ouvrez des ports du pare-feu pour autoriser le trafic à atteindre WebListener.</span><span class="sxs-lookup"><span data-stu-id="e10af-139">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="e10af-140">Vous pouvez utiliser netsh.exe ou des [applets de commande PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="e10af-140">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="e10af-141">Vous pouvez également recourir aux [paramètres de Registre Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="e10af-141">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="e10af-142">Configurer votre application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e10af-142">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="e10af-143">Installez le package NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span><span class="sxs-lookup"><span data-stu-id="e10af-143">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="e10af-144">Cette opération installe également [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) en tant que dépendance.</span><span class="sxs-lookup"><span data-stu-id="e10af-144">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="e10af-145">Appelez la méthode d’extension `UseWebListener` sur [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) dans votre méthode `Main`, en spécifiant les [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) et [paramètres](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) WebListener dont vous avez besoin, comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="e10af-145">Call the `UseWebListener` extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="e10af-146">Configurez les ports et les URL sur lesquels écouter.</span><span class="sxs-lookup"><span data-stu-id="e10af-146">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="e10af-147">Par défaut, ASP.NET Core est lié à `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e10af-147">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="e10af-148">Pour configurer les ports et les préfixes d’URL, vous pouvez utiliser la méthode d’extension `UseURLs`, l’argument de ligne de commande `urls` ou le système de configuration ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e10af-148">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="e10af-149">Pour plus d’informations, consultez [Hébergement](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="e10af-149">For more information, see [Hosting](../../fundamentals/hosting.md).</span></span>

  <span data-ttu-id="e10af-150">WebListener utilise les [formats de chaîne de préfixe Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="e10af-150">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="e10af-151">WebListener n’exige aucun format de chaîne de préfixe spécifique.</span><span class="sxs-lookup"><span data-stu-id="e10af-151">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!WARNING]
  > <span data-ttu-id="e10af-152">Les liaisons génériques de niveau supérieur (`http://*:80/` et `http://+:80`) ne doivent **pas** être utilisées.</span><span class="sxs-lookup"><span data-stu-id="e10af-152">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="e10af-153">Les liaisons génériques de niveau supérieur peuvent exposer votre application à des failles de sécurité.</span><span class="sxs-lookup"><span data-stu-id="e10af-153">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="e10af-154">Cela s’applique aux caractères génériques forts et faibles.</span><span class="sxs-lookup"><span data-stu-id="e10af-154">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="e10af-155">Utilisez des noms d’hôte explicites plutôt que des caractères génériques.</span><span class="sxs-lookup"><span data-stu-id="e10af-155">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="e10af-156">Une liaison générique de sous-domaine (par exemple, `*.mysub.com`) ne présente pas ce risque de sécurité si vous contrôlez le domaine parent en entier (par opposition à `*.com`, qui est vulnérable).</span><span class="sxs-lookup"><span data-stu-id="e10af-156">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="e10af-157">Consultez la [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="e10af-157">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

  > [!NOTE]
  > <span data-ttu-id="e10af-158">Veillez à spécifier dans `UseUrls` les mêmes chaînes de préfixe que celles que vous préenregistrez sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="e10af-158">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="e10af-159">Vérifiez que votre application n’est pas configurée pour exécuter IIS ou IIS Express.</span><span class="sxs-lookup"><span data-stu-id="e10af-159">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="e10af-160">Dans Visual Studio, le profil de démarrage par défaut est destiné à IIS Express.</span><span class="sxs-lookup"><span data-stu-id="e10af-160">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="e10af-161">Pour exécuter le projet en tant qu’application console, vous devez changer manuellement le profil sélectionné, comme indiqué dans la capture d’écran suivante.</span><span class="sxs-lookup"><span data-stu-id="e10af-161">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![Sélectionner le profil d’application console](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="e10af-163">Comment utiliser WebListener en dehors d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e10af-163">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="e10af-164">Installez le package NuGet [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).</span><span class="sxs-lookup"><span data-stu-id="e10af-164">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="e10af-165">[Préenregistrez les préfixes d’URL à lier à WebListener et configurez les certificats SSL](#preregister-url-prefixes-and-configure-ssl) comme vous le feriez pour une utilisation dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e10af-165">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="e10af-166">Vous pouvez également recourir aux [paramètres de Registre Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="e10af-166">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="e10af-167">Voici un exemple de code qui illustre l’utilisation de WebListener en dehors d’ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="e10af-167">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

```csharp
var settings = new WebListenerSettings();
settings.UrlPrefixes.Add("http://localhost:8080");

using (WebListener listener = new WebListener(settings))
{
    listener.Start();

    while (true)
    {
        var context = await listener.AcceptAsync();
        byte[] bytes = Encoding.ASCII.GetBytes("Hello World: " + DateTime.Now);
        context.Response.ContentLength = bytes.Length;
        context.Response.ContentType = "text/plain";

        await context.Response.Body.WriteAsync(bytes, 0, bytes.Length);
        context.Dispose();
    }
}
```

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="e10af-168">Préenregistrer les préfixes d’URL et configurer SSL</span><span class="sxs-lookup"><span data-stu-id="e10af-168">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="e10af-169">IIS et WebListener s’appuient sur le pilote en mode noyau Http.Sys sous-jacent pour écouter les demandes et effectuer le traitement initial.</span><span class="sxs-lookup"><span data-stu-id="e10af-169">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="e10af-170">Dans IIS, l’interface utilisateur de gestion vous permet de tout configurer avec une certaine aisance.</span><span class="sxs-lookup"><span data-stu-id="e10af-170">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="e10af-171">Toutefois, si vous utilisez WebListener, vous devez configurer Http.Sys vous-même.</span><span class="sxs-lookup"><span data-stu-id="e10af-171">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="e10af-172">Pour effectuer cette opération, vous disposez de l’outil intégré netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="e10af-172">The built-in tool for doing that's netsh.exe.</span></span> 

<span data-ttu-id="e10af-173">Les tâches les plus courantes pour lesquelles vous devez utiliser netsh.exe sont la réservation des préfixes d’URL et l’assignation des certificats SSL.</span><span class="sxs-lookup"><span data-stu-id="e10af-173">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="e10af-174">NetSh.exe n’est pas un outil simple à utiliser pour les débutants.</span><span class="sxs-lookup"><span data-stu-id="e10af-174">NetSh.exe isn't an easy tool to use for beginners.</span></span> <span data-ttu-id="e10af-175">L’exemple suivant montre le strict minimum nécessaire pour réserver des préfixes d’URL pour les ports 80 et 443 :</span><span class="sxs-lookup"><span data-stu-id="e10af-175">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="e10af-176">L’exemple suivant montre comment assigner un certificat SSL :</span><span class="sxs-lookup"><span data-stu-id="e10af-176">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="e10af-177">Voici la documentation de référence officielle :</span><span class="sxs-lookup"><span data-stu-id="e10af-177">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="e10af-178">Commandes netsh pour HTTP (Hypertext Transfer Protocol)</span><span class="sxs-lookup"><span data-stu-id="e10af-178">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="e10af-179">Chaînes UrlPrefix</span><span class="sxs-lookup"><span data-stu-id="e10af-179">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="e10af-180">Les ressources suivantes fournissent des instructions détaillées pour plusieurs scénarios.</span><span class="sxs-lookup"><span data-stu-id="e10af-180">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="e10af-181">Les articles qui font référence à `HttpListener` s’appliquent aussi à `WebListener`, tous deux reposant sur Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="e10af-181">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="e10af-182">Guide pratique pour configurer un port avec un certificat SSL</span><span class="sxs-lookup"><span data-stu-id="e10af-182">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="e10af-183">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) Blog tiers et assez ancien, mais qui contient néanmoins des informations utiles.</span><span class="sxs-lookup"><span data-stu-id="e10af-183">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="e10af-184">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) Autre blog ancien comportant des informations utiles.</span><span class="sxs-lookup"><span data-stu-id="e10af-184">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* <span data-ttu-id="e10af-185">[How Do I Set Up A .NET Core WebListener With SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/) (Comment faire pour configurer un WebListener .NET Core avec SSL ?)</span><span class="sxs-lookup"><span data-stu-id="e10af-185">[How Do I Set Up A .NET Core WebListener With SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)</span></span>

<span data-ttu-id="e10af-186">Voici certains outils tiers qui peuvent être plus faciles à utiliser que la ligne de commande netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="e10af-186">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="e10af-187">Ils ne sont pas fournis ou recommandés par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e10af-187">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="e10af-188">Les outils s’exécutent en tant qu’administrateur par défaut, étant donné que netsh.exe lui-même nécessite des privilèges d’administrateur.</span><span class="sxs-lookup"><span data-stu-id="e10af-188">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="e10af-189">[HTTP.sys Manager](http://httpsysmanager.codeplex.com/), dont l’interface utilisateur permet de répertorier et de configurer les options et certificats SSL, les réservations de préfixe et les listes de certificats de confiance.</span><span class="sxs-lookup"><span data-stu-id="e10af-189">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="e10af-190">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) vous permet de répertorier ou de configurer les certificats SSL et les préfixes d’URL.</span><span class="sxs-lookup"><span data-stu-id="e10af-190">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="e10af-191">L’interface utilisateur est plus précise que celle de http.sys Manager et offre quelques options de configuration supplémentaires ; sinon, elle fournit des fonctionnalités similaires.</span><span class="sxs-lookup"><span data-stu-id="e10af-191">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="e10af-192">Cet outil ne peut pas créer de liste de certificats de confiance, mais peut assigner des listes existantes.</span><span class="sxs-lookup"><span data-stu-id="e10af-192">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="e10af-193">Pour générer des certificats SSL auto-signés, Microsoft fournit des outils en ligne de commande : [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) et l’applet de commande PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="e10af-193">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="e10af-194">Il existe également des outils d’interface utilisateur tiers qui facilitent la génération de certificats SSL auto-signés :</span><span class="sxs-lookup"><span data-stu-id="e10af-194">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="e10af-195">SelfCert</span><span class="sxs-lookup"><span data-stu-id="e10af-195">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="e10af-196">Interface utilisateur de Makecert</span><span class="sxs-lookup"><span data-stu-id="e10af-196">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="e10af-197">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="e10af-197">Next steps</span></span>

<span data-ttu-id="e10af-198">Pour plus d'informations, reportez-vous aux ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="e10af-198">For more information, see the following resources:</span></span>

* [<span data-ttu-id="e10af-199">Exemple d’application pour cet article</span><span class="sxs-lookup"><span data-stu-id="e10af-199">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="e10af-200">Code source de WebListener</span><span class="sxs-lookup"><span data-stu-id="e10af-200">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="e10af-201">Hébergement</span><span class="sxs-lookup"><span data-stu-id="e10af-201">Hosting</span></span>](../hosting.md)
