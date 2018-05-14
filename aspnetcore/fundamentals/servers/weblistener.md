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
ms.openlocfilehash: cd2e477824d916afcf1a7901e935dd465a466922
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a>Implémentation du serveur web WebListener dans ASP.NET Core

Par [Tom Dykstra](https://github.com/tdykstra) et [Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> Cette rubrique s’applique uniquement à ASP.NET Core 1.x. Dans ASP.NET Core 2.0, WebListener est nommé [HTTP.sys](httpsys.md).

WebListener est un [serveur web pour ASP.NET Core](index.md) qui s’exécute uniquement sur Windows. Il repose sur le [pilote en mode noyau Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). WebListener est une alternative à [Kestrel](kestrel.md) qui peut être utilisée pour une connexion directe à Internet sans recourir à IIS comme serveur proxy inverse. En fait, **WebListener ne peut pas être utilisé avec IIS ou IIS Express, car il n’est pas compatible avec le [module ASP.NET Core](aspnet-core-module.md).**

Bien que WebListener ait été développé pour ASP.NET Core, il peut être utilisé directement dans n’importe quelle application .NET Core ou .NET Framework par le biais du package NuGet [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).

WebListener prend en charge les fonctionnalités suivantes :

- [Authentification Windows](xref:security/authentication/windowsauth)
- Partage de port
- HTTPS avec SNI
- HTTP/2 sur TLS (Windows 10)
- Transmission de fichier directe
- Mise en cache des réponses
- WebSockets (Windows 8)

Versions Windows prises en charge :

- Windows 7 et Windows Server 2008 R2 et ultérieur

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-weblistener"></a>Quand utiliser WebListener

WebListener est utile pour les déploiements où vous devez exposer le serveur directement à Internet sans l’aide d’IIS.

![WebListener communique directement avec Internet](weblistener/_static/weblistener-to-internet.png)

Comme il repose sur Http.Sys, WebListener ne nécessite pas un serveur proxy inverse en guise de protection contre les attaques. Http.Sys est une technologie en pleine maturité qui offre une protection contre de nombreux types d’attaques et fournit la fiabilité, la sécurité et la scalabilité d’un serveur web complet. Pour sa part, IIS s’exécute en tant qu’écouteur HTTP sur Http.Sys. 

WebListener est également un bon choix pour les déploiements internes si vous avez besoin d’utiliser une de ses fonctionnalités et que cette fonctionnalité n’est pas disponible avec Kestrel.

![WebListener communique directement avec votre réseau interne](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a>Comment utiliser WebListener

Voici une vue d’ensemble des tâches de configuration pour le système d’exploitation hôte et votre application ASP.NET Core.

### <a name="configure-windows-server"></a>Configurer Windows Server

* Installez la version de .NET requise par votre application, telle que [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) ou .NET Framework 4.5.1.

* Préenregistrez les préfixes d’URL à lier à WebListener et configurez les certificats SSL.

   Si vous ne préenregistrez pas de préfixes d’URL dans Windows, vous devez exécuter votre application avec des privilèges d’administrateur. Cela est nécessaire, sauf si vous effectuez une liaison à localhost à l’aide de HTTP (pas HTTPS) avec un numéro de port supérieur à 1024.

   Pour plus d’informations, consultez [Préenregistrer les préfixes d’URL et configurer SSL](#preregister-url-prefixes-and-configure-ssl) plus loin dans cet article.

* Ouvrez des ports du pare-feu pour autoriser le trafic à atteindre WebListener.

   Vous pouvez utiliser netsh.exe ou des [applets de commande PowerShell](https://technet.microsoft.com/library/jj554906).

Vous pouvez également recourir aux [paramètres de Registre Http.Sys](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application"></a>Configurer votre application ASP.NET Core

* Installez le package NuGet [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/). Cette opération installe également [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) en tant que dépendance.

* Appelez la méthode d’extension `UseWebListener` sur [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) dans votre méthode `Main`, en spécifiant les [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) et [paramètres](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) WebListener dont vous avez besoin, comme indiqué dans l’exemple suivant :

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* Configurez les ports et les URL sur lesquels écouter. 

  Par défaut, ASP.NET Core est lié à `http://localhost:5000`. Pour configurer les ports et les préfixes d’URL, vous pouvez utiliser la méthode d’extension `UseURLs`, l’argument de ligne de commande `urls` ou le système de configuration ASP.NET Core. Pour plus d’informations, consultez [Hébergement](../../fundamentals/hosting.md).

  WebListener utilise les [formats de chaîne de préfixe Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx). WebListener n’exige aucun format de chaîne de préfixe spécifique.

  > [!WARNING]
  > Les liaisons génériques de niveau supérieur (`http://*:80/` et `http://+:80`) ne doivent **pas** être utilisées. Les liaisons génériques de niveau supérieur peuvent exposer votre application à des failles de sécurité. Cela s’applique aux caractères génériques forts et faibles. Utilisez des noms d’hôte explicites plutôt que des caractères génériques. Une liaison générique de sous-domaine (par exemple, `*.mysub.com`) ne présente pas ce risque de sécurité si vous contrôlez le domaine parent en entier (par opposition à `*.com`, qui est vulnérable). Consultez la [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) pour plus d’informations.

  > [!NOTE]
  > Veillez à spécifier dans `UseUrls` les mêmes chaînes de préfixe que celles que vous préenregistrez sur le serveur. 

* Vérifiez que votre application n’est pas configurée pour exécuter IIS ou IIS Express.

  Dans Visual Studio, le profil de démarrage par défaut est destiné à IIS Express.  Pour exécuter le projet en tant qu’application console, vous devez changer manuellement le profil sélectionné, comme indiqué dans la capture d’écran suivante.

  ![Sélectionner le profil d’application console](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a>Comment utiliser WebListener en dehors d’ASP.NET Core

* Installez le package NuGet [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).

* [Préenregistrez les préfixes d’URL à lier à WebListener et configurez les certificats SSL](#preregister-url-prefixes-and-configure-ssl) comme vous le feriez pour une utilisation dans ASP.NET Core.

Vous pouvez également recourir aux [paramètres de Registre Http.Sys](https://support.microsoft.com/kb/820129).


Voici un exemple de code qui illustre l’utilisation de WebListener en dehors d’ASP.NET Core :

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Préenregistrer les préfixes d’URL et configurer SSL

IIS et WebListener s’appuient sur le pilote en mode noyau Http.Sys sous-jacent pour écouter les demandes et effectuer le traitement initial. Dans IIS, l’interface utilisateur de gestion vous permet de tout configurer avec une certaine aisance. Toutefois, si vous utilisez WebListener, vous devez configurer Http.Sys vous-même. Pour effectuer cette opération, vous disposez de l’outil intégré netsh.exe. 

Les tâches les plus courantes pour lesquelles vous devez utiliser netsh.exe sont la réservation des préfixes d’URL et l’assignation des certificats SSL.

NetSh.exe n’est pas un outil simple à utiliser pour les débutants. L’exemple suivant montre le strict minimum nécessaire pour réserver des préfixes d’URL pour les ports 80 et 443 :

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

L’exemple suivant montre comment assigner un certificat SSL :

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

Voici la documentation de référence officielle :

* [Commandes netsh pour HTTP (Hypertext Transfer Protocol)](https://technet.microsoft.com/library/cc725882.aspx)
* [Chaînes UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Les ressources suivantes fournissent des instructions détaillées pour plusieurs scénarios. Les articles qui font référence à `HttpListener` s’appliquent aussi à `WebListener`, tous deux reposant sur Http.Sys.

* [Guide pratique pour configurer un port avec un certificat SSL](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) Blog tiers et assez ancien, mais qui contient néanmoins des informations utiles.
* [How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) Autre blog ancien comportant des informations utiles.
* [How Do I Set Up A .NET Core WebListener With SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/) (Comment faire pour configurer un WebListener .NET Core avec SSL ?)

Voici certains outils tiers qui peuvent être plus faciles à utiliser que la ligne de commande netsh.exe. Ils ne sont pas fournis ou recommandés par Microsoft. Les outils s’exécutent en tant qu’administrateur par défaut, étant donné que netsh.exe lui-même nécessite des privilèges d’administrateur.

* [HTTP.sys Manager](http://httpsysmanager.codeplex.com/), dont l’interface utilisateur permet de répertorier et de configurer les options et certificats SSL, les réservations de préfixe et les listes de certificats de confiance. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) vous permet de répertorier ou de configurer les certificats SSL et les préfixes d’URL. L’interface utilisateur est plus précise que celle de http.sys Manager et offre quelques options de configuration supplémentaires ; sinon, elle fournit des fonctionnalités similaires. Cet outil ne peut pas créer de liste de certificats de confiance, mais peut assigner des listes existantes.

Pour générer des certificats SSL auto-signés, Microsoft fournit des outils en ligne de commande : [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) et l’applet de commande PowerShell [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate). Il existe également des outils d’interface utilisateur tiers qui facilitent la génération de certificats SSL auto-signés :

* [SelfCert](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [Interface utilisateur de Makecert](http://makecertui.codeplex.com/)

## <a name="next-steps"></a>Étapes suivantes

Pour plus d'informations, reportez-vous aux ressources suivantes :

* [Exemple d’application pour cet article](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [Code source de WebListener](https://github.com/aspnet/HttpSysServer/)
* [Hébergement](../hosting.md)
