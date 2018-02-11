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
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>Implémentation du serveur web HTTP.sys dans ASP.NET Core

Par [Tom Dykstra](https://github.com/tdykstra) et [Chris Ross](https://github.com/Tratcher)

> [!NOTE]
> Cette rubrique s’applique uniquement à ASP.NET Core 2.0 et ultérieur. Dans les versions antérieures d’ASP.NET Core, HTTP.sys est appelé [WebListener](xref:fundamentals/servers/weblistener).

HTTP.sys est un [serveur web pour ASP.NET Core](index.md) qui s’exécute uniquement sur Windows. Il repose sur le [pilote en mode noyau Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx). Offrant certaines fonctionnalités qui lui sont propres, HTTP.sys est une alternative à [Kestrel](kestrel.md). **HTTP.sys ne peut pas être utilisé avec IIS ou IIS Express, car il est incompatible avec le [module ASP.NET Core](aspnet-core-module.md).**

HTTP.sys prend en charge les fonctionnalités suivantes :

- [Authentification Windows](xref:security/authentication/windowsauth)
- Partage de port
- HTTPS avec SNI
- HTTP/2 sur TLS (Windows 10)
- Transmission de fichier directe
- Mise en cache des réponses
- WebSockets (Windows 8)

Versions Windows prises en charge :

- Windows 7 et Windows Server 2008 R2 et ultérieur

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>Quand utiliser HTTP.sys

HTTP.sys est utile pour les déploiements où vous devez exposer le serveur directement à Internet sans l’aide d’IIS.

![HTTP.sys communique directement avec Internet](httpsys/_static/httpsys-to-internet.png)

Comme il repose sur Http.Sys, HTTP.sys ne nécessite pas de serveur proxy inverse en guise de protection contre les attaques. Http.Sys est une technologie en pleine maturité qui offre une protection contre de nombreux types d’attaques et fournit la fiabilité, la sécurité et la scalabilité d’un serveur web complet. Pour sa part, IIS s’exécute en tant qu’écouteur HTTP sur Http.Sys. 

HTTP.sys est un bon choix pour les déploiements internes quand vous avez besoin d’une fonctionnalité qui n’est pas disponible dans Kestrel, telle que l’authentification Windows.

![HTTP.sys communique directement avec votre réseau interne](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a>Comment utiliser HTTP.sys

Voici une vue d’ensemble des tâches de configuration pour le système d’exploitation hôte et votre application ASP.NET Core.

### <a name="configure-windows-server"></a>Configurer Windows Server

* Installez la version de .NET requise par votre application, telle que [.NET Core](https://www.microsoft.com/net/download/core) ou [.NET Framework](https://www.microsoft.com/net/download/framework).

* Préenregistrez les préfixes d’URL à lier à HTTP.sys et configurez les certificats SSL.

   Si vous ne préenregistrez pas de préfixes d’URL dans Windows, vous devez exécuter votre application avec des privilèges d’administrateur. C’est nécessaire, sauf si vous effectuez une liaison à localhost à l’aide de HTTP (pas HTTPS) avec un numéro de port supérieur à 1024.

   Pour plus d’informations, consultez [Préenregistrer les préfixes d’URL et configurer SSL](#preregister-url-prefixes-and-configure-ssl) plus loin dans cet article.

* Ouvrez des ports du pare-feu pour autoriser le trafic à atteindre HTTP.sys.

   Vous pouvez utiliser *netsh.exe* ou des [applets de commande PowerShell](https://technet.microsoft.com/library/jj554906).

Vous pouvez également recourir aux [paramètres de Registre Http.Sys](https://support.microsoft.com/kb/820129).

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a>Configurer votre application ASP.NET Core pour qu’elle utilise HTTP.sys

* Aucune installation de package n’est nécessaire si vous utilisez le métapackage [Microsoft.AspNetCore.All](xref:fundamentals/metapackage). Le package [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) est inclus dans le métapackage.

* Appelez la méthode d’extension `UseHttpSys` sur `WebHostBuilder` dans votre méthode `Main`, en spécifiant les [options HTTP.sys](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) dont vous avez besoin, comme indiqué dans l’exemple suivant :

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a>Configurer les options HTTP.sys

Voici certains paramètres et limites HTTP.sys que vous pouvez configurer.

**Nombre maximal de connexions client**

Le nombre maximal de connexions TCP ouvertes simultanées peut être défini pour l’application entière avec le code suivant dans *Program.cs* :

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

Le nombre maximal de connexions est illimité (null) par défaut.

**Taille maximale du corps de la demande**

La taille maximale par défaut du corps de la demande est de 30 000 000 octets, soit environ 28,6 Mo.

Pour remplacer la limite dans une application MVC ASP.NET Core, nous vous recommandons d’utiliser l’attribut [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) sur une méthode d’action :

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

Voici un exemple qui montre comment configurer la contrainte pour l’application entière et chaque demande :

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

Vous pouvez remplacer le paramètre sur une demande spécifique dans *Startup.cs* :

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
Une exception est levée si vous essayez de configurer la limite sur une demande dont l’application a commencé la lecture. Il existe une propriété `IsReadOnly` qui indique si la propriété `MaxRequestBodySize` est en lecture seule ; si tel est le cas, il est trop tard pour configurer la limite.

Pour plus d’informations sur les autres options de HTTP.sys, consultez [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). 

### <a name="configure-urls-and-ports-to-listen-on"></a>Configurer les ports et les URL sur lesquels écouter 

Par défaut, ASP.NET Core est lié à `http://localhost:5000`. Pour configurer les ports et les préfixes d’URL, vous pouvez utiliser la méthode d’extension `UseUrls`, l’argument de ligne de commande `urls`, la variable d’environnement ASPNETCORE_URLS ou la propriété `UrlPrefixes` sur [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs). L’exemple de code suivant utilise `UrlPrefixes`.

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

Grâce à `UrlPrefixes`, vous obtenez un message d’erreur immédiatement si vous essayez d’ajouter un préfixe au format incorrect. Pour sa part, `UseUrls` (partagé avec `urls` et ASPNETCORE_URLS) vous permet de basculer plus facilement entre Kestrel et HTTP.sys.

Si vous utilisez à la fois `UseUrls` (ou `urls` ou ASPNETCORE_URLS) et `UrlPrefixes`, les paramètres dans `UrlPrefixes` remplacent ceux dans `UseUrls`. Pour plus d’informations, consultez [Hébergement](xref:fundamentals/hosting).

HTTP.sys utilise les [formats de chaîne UrlPrefix de l’API de serveur HTTP](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

> [!NOTE]
> Veillez à spécifier dans `UseUrls` ou `UrlPrefixes` les mêmes chaînes de préfixe que celles que vous préenregistrez sur le serveur. 

### <a name="dont-use-iis"></a>Ne pas utiliser IIS

Vérifiez que votre application n’est pas configurée pour exécuter IIS ou IIS Express.

Dans Visual Studio, le profil de démarrage par défaut est destiné à IIS Express. Pour exécuter le projet en tant qu’application console, changez manuellement le profil sélectionné, comme indiqué dans la capture d’écran suivante.

![Sélectionner le profil d’application console](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a>Préenregistrer les préfixes d’URL et configurer SSL

IIS et HTTP.sys s’appuient sur le pilote en mode noyau Http.Sys sous-jacent pour écouter les demandes et effectuer le traitement initial. Dans IIS, l’interface utilisateur de gestion vous permet de tout configurer avec une certaine aisance. Toutefois, vous devez configurer Http.Sys vous-même. Pour effectuer cette opération, vous disposez de l’outil intégré *netsh.exe*. 

Avec *netsh.exe*, vous pouvez réserver les préfixes d’URL et assigner les certificats SSL. L’outil exige des privilèges d’administrateur.

L’exemple suivant montre le minimum nécessaire pour réserver des préfixes d’URL pour les ports 80 et 443 :

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

L’exemple suivant montre comment assigner un certificat SSL :

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

Voici la documentation de référence de *netsh.exe* :

* [Commandes netsh pour HTTP (Hypertext Transfer Protocol)](https://technet.microsoft.com/library/cc725882.aspx)
* [Chaînes UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

Les ressources suivantes fournissent des instructions détaillées pour plusieurs scénarios. Les articles qui font référence à HttpListener s’appliquent aussi à HTTP.sys, tous deux reposant sur Http.Sys.

* [Guide pratique pour configurer un port avec un certificat SSL](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* [HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) Blog tiers et assez ancien, mais qui contient néanmoins des informations utiles.
* [How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) Autre blog ancien comportant des informations utiles.

Voici certains outils tiers qui peuvent être plus faciles à utiliser que la ligne de commande *netsh.exe*. Ils ne sont pas fournis ou recommandés par Microsoft. Les outils s’exécutent en tant qu’administrateur par défaut, étant donné que *netsh.exe* lui-même nécessite des privilèges d’administrateur.

* [HTTP.sys Manager](http://httpsysmanager.codeplex.com/), dont l’interface utilisateur permet de répertorier et de configurer les options et certificats SSL, les réservations de préfixe et les listes de certificats de confiance. 
* [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) vous permet de répertorier ou de configurer les certificats SSL et les préfixes d’URL. L’interface utilisateur est plus précise que celle de http.sys Manager et offre quelques options de configuration supplémentaires ; sinon, elle fournit des fonctionnalités similaires. Cet outil ne peut pas créer de liste de certificats de confiance, mais peut assigner des listes existantes.

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a>Étapes suivantes

Pour plus d'informations, reportez-vous aux ressources suivantes :

* [Exemple d’application pour cet article](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [Code source HTTP.sys](https://github.com/aspnet/HttpSysServer/)
* [Hébergement d’applications WPF](xref:fundamentals/hosting)
