---
title: "Implémentation du serveur web WebListener ASP.NET Core"
author: rick-anderson
description: "Introduit WebListener, un serveur web pour ASP.NET Core sur Windows. Basé sur le pilote de mode noyau Http.Sys, WebListener est une alternative à Kestrel qui peut être utilisé pour une connexion directe à Internet sans IIS."
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/weblistener
ms.openlocfilehash: 5073a1663ec99a1b161092d74ab035ee9782becd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="9b238-104">Implémentation du serveur web WebListener ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9b238-104">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="9b238-105">Par [Tom Dykstra](https://github.com/tdykstra) et [Ross de Chris](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="9b238-105">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="9b238-106">Cette rubrique s’applique uniquement aux ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="9b238-106">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="9b238-107">Dans ASP.NET 2.0 de noyaux, nommé WebListener [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="9b238-107">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="9b238-108">WebListener est un [serveur web pour ASP.NET Core](index.md) qui s’exécute uniquement sous Windows.</span><span class="sxs-lookup"><span data-stu-id="9b238-108">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="9b238-109">Il repose sur le [pilote de mode noyau Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="9b238-109">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="9b238-110">WebListener est une alternative à [Kestrel](kestrel.md) qui peut être utilisé pour une connexion directe à Internet sans se baser sur IIS comme un serveur proxy inverse.</span><span class="sxs-lookup"><span data-stu-id="9b238-110">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="9b238-111">En fait, **WebListener ne peut pas être utilisée avec IIS ou IIS Express, car il n’est pas compatible avec le [ASP.NET Core Module](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="9b238-111">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="9b238-112">Bien que WebListener a été développé pour ASP.NET Core, il peut être utilisé directement dans n’importe quelle application .NET Core ou .NET Framework via la [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) package NuGet.</span><span class="sxs-lookup"><span data-stu-id="9b238-112">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="9b238-113">WebListener prend en charge les fonctionnalités suivantes :</span><span class="sxs-lookup"><span data-stu-id="9b238-113">WebListener supports the following features:</span></span>

- [<span data-ttu-id="9b238-114">Authentification Windows</span><span class="sxs-lookup"><span data-stu-id="9b238-114">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="9b238-115">Le partage de port</span><span class="sxs-lookup"><span data-stu-id="9b238-115">Port sharing</span></span>
- <span data-ttu-id="9b238-116">HTTPS avec SNI</span><span class="sxs-lookup"><span data-stu-id="9b238-116">HTTPS with SNI</span></span>
- <span data-ttu-id="9b238-117">HTTP/2 sur TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="9b238-117">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="9b238-118">Transmission de fichier direct</span><span class="sxs-lookup"><span data-stu-id="9b238-118">Direct file transmission</span></span>
- <span data-ttu-id="9b238-119">Réponse mise en cache</span><span class="sxs-lookup"><span data-stu-id="9b238-119">Response caching</span></span>
- <span data-ttu-id="9b238-120">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="9b238-120">WebSockets (Windows 8)</span></span>

<span data-ttu-id="9b238-121">Versions de Windows prises en charge :</span><span class="sxs-lookup"><span data-stu-id="9b238-121">Supported Windows versions:</span></span>

- <span data-ttu-id="9b238-122">Windows 7 et Windows Server 2008 R2 et versions ultérieures</span><span class="sxs-lookup"><span data-stu-id="9b238-122">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="9b238-123">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9b238-123">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="9b238-124">Quand utiliser WebListener</span><span class="sxs-lookup"><span data-stu-id="9b238-124">When to use WebListener</span></span>

<span data-ttu-id="9b238-125">WebListener est utile pour les déploiements où vous devez exposer le serveur directement à Internet sans l’aide d’IIS.</span><span class="sxs-lookup"><span data-stu-id="9b238-125">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![WebListener communique directement avec Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="9b238-127">Comme il repose sur Http.Sys, WebListener ne nécessite pas un serveur proxy inverse pour la protection contre les attaques.</span><span class="sxs-lookup"><span data-stu-id="9b238-127">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="9b238-128">Http.Sys est une technologie mature qui protège contre les nombreux types d’attaques et fournit la fiabilité, la sécurité et l’évolutivité d’un serveur web complet.</span><span class="sxs-lookup"><span data-stu-id="9b238-128">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="9b238-129">IIS s’exécute comme un écouteur HTTP sur Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="9b238-129">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="9b238-130">WebListener est également un bon choix pour les déploiements internes lorsque vous avez besoin de l’une des fonctionnalités que vous ne pouvez pas obtenir à l’aide de Kestrel propose.</span><span class="sxs-lookup"><span data-stu-id="9b238-130">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![WebListener communique directement avec votre réseau interne](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a><span data-ttu-id="9b238-132">Comment utiliser WebListener</span><span class="sxs-lookup"><span data-stu-id="9b238-132">How to use WebListener</span></span>

<span data-ttu-id="9b238-133">Voici une vue d’ensemble des tâches de configuration pour le système d’exploitation hôte et de votre application ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9b238-133">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="9b238-134">Configurer Windows Server</span><span class="sxs-lookup"><span data-stu-id="9b238-134">Configure Windows Server</span></span>

* <span data-ttu-id="9b238-135">Installez la version de .NET requis par votre application, tel que [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) ou .NET Framework 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="9b238-135">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="9b238-136">Préenregistrer des préfixes d’URL pour les lier à WebListener et configurer des certificats SSL</span><span class="sxs-lookup"><span data-stu-id="9b238-136">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="9b238-137">Si vous ne préenregistrer des préfixes d’URL dans Windows, vous devez exécuter votre application avec des privilèges d’administrateur.</span><span class="sxs-lookup"><span data-stu-id="9b238-137">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="9b238-138">La seule exception est si vous liez à localhost, à l’aide de HTTP pas HTTPS avec un numéro de port supérieur à 1 024 ; Dans ce cas des privilèges d’administrateur ne sont pas nécessaires.</span><span class="sxs-lookup"><span data-stu-id="9b238-138">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="9b238-139">Pour plus d’informations, consultez [préenregistrer préfixes et de configuration SSL](#preregister-url-prefixes-and-configure-ssl) plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="9b238-139">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="9b238-140">Ouvrir les ports du pare-feu pour autoriser le trafic atteindre WebListener.</span><span class="sxs-lookup"><span data-stu-id="9b238-140">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="9b238-141">Vous pouvez utiliser netsh.exe ou [applets de commande PowerShell](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="9b238-141">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="9b238-142">Il existe également [paramètres de Registre Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="9b238-142">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="9b238-143">Configurer votre application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9b238-143">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="9b238-144">Installez le package NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span><span class="sxs-lookup"><span data-stu-id="9b238-144">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="9b238-145">Cette commande installe également [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) en tant que dépendance.</span><span class="sxs-lookup"><span data-stu-id="9b238-145">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="9b238-146">Appelez le `UseWebListener` méthode d’extension sur [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) dans votre `Main` méthode, en spécifiant les WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) et [paramètres](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) dont vous avez besoin , comme indiqué dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="9b238-146">Call the `UseWebListener` extension method on [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="9b238-147">Configurer les ports et les URL pour écouter sur</span><span class="sxs-lookup"><span data-stu-id="9b238-147">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="9b238-148">Par défaut, ASP.NET Core lie à `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="9b238-148">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="9b238-149">Pour configurer les ports et les préfixes d’URL, vous pouvez utiliser la `UseURLs` méthode d’extension, le `urls` argument de ligne de commande ou le système de configuration ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9b238-149">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="9b238-150">Pour plus d’informations, consultez [Hébergement](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="9b238-150">For more information, see [Hosting](../../fundamentals/hosting.md).</span></span>

  <span data-ttu-id="9b238-151">Web récepteur utilise le [formats de chaîne de préfixe Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="9b238-151">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="9b238-152">Il n’existe aucune exigence de format de chaîne de préfixe qui sont spécifiques à WebListener.</span><span class="sxs-lookup"><span data-stu-id="9b238-152">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!NOTE]
  > <span data-ttu-id="9b238-153">Assurez-vous que vous spécifiez les mêmes chaînes de préfixe dans `UseUrls` qui vous préenregistrer sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="9b238-153">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="9b238-154">Assurez-vous que votre application n’est pas configurée pour exécuter les services IIS ou IIS Express.</span><span class="sxs-lookup"><span data-stu-id="9b238-154">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="9b238-155">Dans Visual Studio, le profil de démarrage par défaut est pour IIS Express.</span><span class="sxs-lookup"><span data-stu-id="9b238-155">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="9b238-156">Vous devez modifier manuellement le profil sélectionné, pour exécuter le projet en tant qu’une application console comme indiqué dans la capture d’écran suivante.</span><span class="sxs-lookup"><span data-stu-id="9b238-156">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![Sélectionnez le profil d’application console](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="9b238-158">L’utilisation de WebListener en dehors d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9b238-158">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="9b238-159">Installer le [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) package NuGet.</span><span class="sxs-lookup"><span data-stu-id="9b238-159">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="9b238-160">[Préenregistrer des préfixes d’URL pour les lier à WebListener et configurer des certificats SSL](#preregister-url-prefixes-and-configure-ssl) comme vous le feriez pour une utilisation dans ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9b238-160">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="9b238-161">Il existe également [paramètres de Registre Http.Sys](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="9b238-161">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="9b238-162">Voici un exemple de code qui illustre l’utilisation de WebListener en dehors d’ASP.NET Core :</span><span class="sxs-lookup"><span data-stu-id="9b238-162">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="9b238-163">Préenregistrer des préfixes d’URL et de configurer le protocole SSL</span><span class="sxs-lookup"><span data-stu-id="9b238-163">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="9b238-164">IIS et WebListener s’appuient sur le pilote sous-jacent de mode noyau Http.Sys pour écouter les demandes et le traitement initiale.</span><span class="sxs-lookup"><span data-stu-id="9b238-164">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="9b238-165">Dans IIS, l’interface utilisateur de gestion vous donne un moyen relativement facile à configurer tous les éléments.</span><span class="sxs-lookup"><span data-stu-id="9b238-165">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="9b238-166">Toutefois, si vous utilisez WebListener, vous devez configurer Http.Sys vous-même.</span><span class="sxs-lookup"><span data-stu-id="9b238-166">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="9b238-167">L’outil intégré pour cela est netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="9b238-167">The built-in tool for doing that's netsh.exe.</span></span> 

<span data-ttu-id="9b238-168">Les tâches les plus courantes que vous devez utiliser netsh.exe pour sont réservation des préfixes d’URL et affectation des certificats SSL.</span><span class="sxs-lookup"><span data-stu-id="9b238-168">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="9b238-169">NetSh.exe n’est pas un outil simple à utiliser pour les débutants.</span><span class="sxs-lookup"><span data-stu-id="9b238-169">NetSh.exe isn't an easy tool to use for beginners.</span></span> <span data-ttu-id="9b238-170">L’exemple suivant montre le strict minimum nécessaire pour réserver des préfixes d’URL pour les ports 80 et 443 :</span><span class="sxs-lookup"><span data-stu-id="9b238-170">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="9b238-171">L’exemple suivant montre comment assigner un certificat SSL :</span><span class="sxs-lookup"><span data-stu-id="9b238-171">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="9b238-172">Voici la documentation de référence officiel :</span><span class="sxs-lookup"><span data-stu-id="9b238-172">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="9b238-173">Commandes Netsh pour Hypertext Transfer Protocol (HTTP)</span><span class="sxs-lookup"><span data-stu-id="9b238-173">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="9b238-174">Chaînes UrlPrefix</span><span class="sxs-lookup"><span data-stu-id="9b238-174">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="9b238-175">Les ressources suivantes fournissent des instructions détaillées pour plusieurs scénarios.</span><span class="sxs-lookup"><span data-stu-id="9b238-175">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="9b238-176">Les articles qui font référence aux `HttpListener` s’appliquent aussi à `WebListener`, comme les deux reposent sur Http.Sys.</span><span class="sxs-lookup"><span data-stu-id="9b238-176">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="9b238-177">Guide pratique pour configurer un port avec un certificat SSL</span><span class="sxs-lookup"><span data-stu-id="9b238-177">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="9b238-178">[Les communications HTTPS - HttpListener basé hébergement et la Certification Client](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) blog de tiers et est assez ancien mais dispose toujours des informations utiles.</span><span class="sxs-lookup"><span data-stu-id="9b238-178">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="9b238-179">[Comment : HttpListener à l’aide de procédure pas à pas ou le serveur Http du code non managé (C++) comme serveur SSL Simple](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) ce qui est également un blog plus anciens avec des informations utiles.</span><span class="sxs-lookup"><span data-stu-id="9b238-179">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* [<span data-ttu-id="9b238-180">Comment configurer un WebListener de noyaux .NET avec SSL ?</span><span class="sxs-lookup"><span data-stu-id="9b238-180">How Do I Set Up A .NET Core WebListener With SSL?</span></span>](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

<span data-ttu-id="9b238-181">Voici certains des outils tiers qui peuvent être plus faciles à utiliser que la ligne de commande netsh.exe.</span><span class="sxs-lookup"><span data-stu-id="9b238-181">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="9b238-182">Ils ne sont pas fournies par ou recommandés par Microsoft.</span><span class="sxs-lookup"><span data-stu-id="9b238-182">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="9b238-183">Les outils exécutés en tant qu’administrateur par défaut, étant donné que netsh.exe lui-même nécessite des privilèges d’administrateur.</span><span class="sxs-lookup"><span data-stu-id="9b238-183">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="9b238-184">[HTTP.sys Manager](http://httpsysmanager.codeplex.com/) fournit l’interface utilisateur pour la liste et la configuration des options et des certificats SSL, les réservations de préfixe et listes de certificats fiables.</span><span class="sxs-lookup"><span data-stu-id="9b238-184">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="9b238-185">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) vous permet de répertorier ou configurer des certificats SSL et les préfixes d’URL.</span><span class="sxs-lookup"><span data-stu-id="9b238-185">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="9b238-186">L’interface utilisateur est plus précis que http.sys Manager et expose quelques options de configuration supplémentaires, mais dans le cas contraire, il fournit des fonctionnalités similaires.</span><span class="sxs-lookup"><span data-stu-id="9b238-186">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="9b238-187">Il ne peut pas créer une nouvelle liste de certificats (CTL), mais que vous pouvez affecter existants.</span><span class="sxs-lookup"><span data-stu-id="9b238-187">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="9b238-188">Pour générer des certificats SSL auto-signés, Microsoft fournit des outils de ligne de commande : [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) et l’applet de commande PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="9b238-188">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="9b238-189">Il existe également des outils d’interface utilisateur tiers qui le rendent plus facile de générer des certificats SSL auto-signés :</span><span class="sxs-lookup"><span data-stu-id="9b238-189">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="9b238-190">SelfCert</span><span class="sxs-lookup"><span data-stu-id="9b238-190">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="9b238-191">Interface utilisateur de Makecert</span><span class="sxs-lookup"><span data-stu-id="9b238-191">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="9b238-192">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="9b238-192">Next steps</span></span>

<span data-ttu-id="9b238-193">Pour plus d'informations, reportez-vous aux ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="9b238-193">For more information, see the following resources:</span></span>

* [<span data-ttu-id="9b238-194">Exemple d’application pour cet article</span><span class="sxs-lookup"><span data-stu-id="9b238-194">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="9b238-195">Code source de WebListener</span><span class="sxs-lookup"><span data-stu-id="9b238-195">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="9b238-196">Hébergement d’applications WPF</span><span class="sxs-lookup"><span data-stu-id="9b238-196">Hosting</span></span>](../hosting.md)
