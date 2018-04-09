---
title: Configurez ASP.NET Core pour travailler avec des serveurs proxy et des équilibrages de charge
author: guardrex
description: En savoir plus sur la configuration pour les applications hébergées derrière des serveurs proxy et les équilibreurs de charge, souvent masquent important demandent des informations.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: b153a7406ae1b31a2aa453135c6bd0e5ce0b2997
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2018
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a>Configurez ASP.NET Core pour travailler avec des serveurs proxy et des équilibrages de charge

Par [Luke Latham](https://github.com/guardrex) et [Ross de Chris](https://github.com/Tratcher)

Dans la configuration recommandée pour ASP.NET Core, l’application est hébergée à l’aide du Module de base IIS/ASP.NET, Nginx ou Apache. Serveurs proxy, les équilibreurs de charge et autres dispositifs réseau souvent masquent les informations sur la demande avant d’atteindre l’application :

* Lorsque les requêtes HTTPS sont transmis via HTTP, le schéma d’origine (HTTPS) est perdu et doit être transmis dans un en-tête.
* Car une application reçoit une demande le proxy et pas sa source true sur Internet ou au réseau d’entreprise, l’adresse IP du client d’origine doit également être transféré dans un en-tête.

Ces informations peuvent être importantes dans le traitement de requêtes, par exemple dans les redirections, l’authentification, génération de lien, évaluation de la stratégie et geoloation du client.

## <a name="forwarded-headers"></a>En-têtes transférés

Par convention, proxy transférer des informations dans les en-têtes HTTP.

| Header | Description |
| ------ | ----------- |
| X transmis pour | Contient des informations sur le client qui a initié la demande et les proxys suivantes dans une chaîne de proxys. Ce paramètre peut contenir IP adresses (et, éventuellement, les numéros de port). Dans une chaîne de serveurs proxy, le premier paramètre indique le client sur lequel la demande a été effectuée pour la première fois. Les identificateurs suivants proxy suivre. Le proxy du dernier dans la chaîne n’est pas dans la liste des paramètres. Adresse IP du proxy de la dernière et éventuellement un numéro de port sont disponibles en tant que l’adresse IP distante au niveau de la couche de transport. |
| Proto transféré X | La valeur de la méthode d’origine (HTTP/HTTPS). La valeur peut également être une liste de jeux si la demande a parcouru plusieurs serveurs proxy. |
| Hôte X transmis | La valeur d’origine du champ d’en-tête hôte. En règle générale, proxys ne modifiez pas l’en-tête d’hôte. Consultez [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) pour plus d’informations sur une vulnérabilité d’élévation de privilèges qui affecte les systèmes où le proxy ne valide pas ou les en-têtes d’hôtes restict aux valeurs corrects connus. |

L’intergiciel en-têtes transférés, à partir de la [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) du package, lit ces en-têtes et renseigne les champs associés sur [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext). 

Les mises à jour de l’intergiciel (middleware) :

* [HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; à l’aide de la `X-Forwarded-For` valeur d’en-tête. Comment l’intergiciel (middleware) définit les paramètres supplémentaires influencent `RemoteIpAddress`. Pour plus d’informations, consultez la [transféré intergiciel (middleware) en-têtes options](#forwarded-headers-middleware-options).
* [HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; à l’aide de la `X-Forwarded-Proto` valeur d’en-tête.
* [HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; à l’aide de la `X-Forwarded-Host` valeur d’en-tête.

Notez que pas tous les dispositifs réseau ajouter la `X-Forwarded-For` et `X-Forwarded-Proto` en-têtes sans configuration supplémentaire. Consultez les instructions du fabricant de votre matériel si les demandes traitées ne contiennent pas ces en-têtes lorsqu’ils atteignent l’application.

Transféré en-têtes intergiciel (middleware) [paramètres par défaut](#forwarded-headers-middleware-options) peuvent être configurés. Les paramètres par défaut sont :

* Il est uniquement *un proxy* entre l’application et la source des requêtes.
* Seules les adresses de bouclage sont configurés pour proxys connus et connus de réseaux.

## <a name="iisiis-express-and-aspnet-core-module"></a>IIS/IIS Express et Module ASP.NET Core

Transféré en-têtes intergiciel (middleware) est activé par défaut par un intergiciel (middleware) intégration de IIS lors de l’exécution de l’application IIS et le Module de base ASP.NET. Transféré en-têtes intergiciel (middleware) est activée pour l’exécuter en premier dans le pipeline de l’intergiciel (middleware) avec une configuration limitée spécifique pour le Module de base ASP.NET en raison des problèmes d’approbation avec les en-têtes transférés (par exemple, [usurpation d’adresse IP](https://www.iplocation.net/ip-spoofing)). L’intergiciel (middleware) est configuré pour transmettre le `X-Forwarded-For` et `X-Forwarded-Proto` en-têtes et est limitée à un serveur proxy unique localhost. Si une configuration supplémentaire est requise, consultez le [transféré intergiciel (middleware) en-têtes options](#forwarded-headers-middleware-options).

## <a name="other-proxy-server-and-load-balancer-scenarios"></a>Autres scénarios d’équilibrage de charge et du serveur proxy

En dehors d’à l’aide d’intergiciel (middleware) intégration de IIS, transféré intergiciel (middleware) en-têtes n’est pas activé par défaut. Transféré en-têtes intergiciel (middleware) doit être activé pour une application aux en-têtes de processus transmis avec [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders). Après l’activation de l’intergiciel (middleware) si aucun [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) sont spécifiés à l’intergiciel (middleware), la valeur par défaut [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) sont [ForwardedHeaders.None ](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).

Configurer l’intergiciel (middleware) avec [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) pour transférer le `X-Forwarded-For` et `X-Forwarded-Proto` en-têtes dans `Startup.ConfigureServices`. Appeler le [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) méthode `Startup.Configure` avant d’appeler d’autres intergiciel (middleware) :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    
    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders = 
            ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
    });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseForwardedHeaders();

    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseStaticFiles();
    // In ASP.NET Core 1.x, replace the following line with: app.UseIdentity();
    app.UseAuthentication();
    app.UseMvc();
}
```

> [!NOTE]
> Si aucun [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) sont spécifiés dans `Startup.ConfigureServices` ou directement à la méthode d’extension avec [UseForwardedHeaders (IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), la valeur par défaut pour transférer les en-têtes sont [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). Le [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) propriété doit être configurée avec les en-têtes à transférer.

## <a name="forwarded-headers-middleware-options"></a>Options d’en-têtes intergiciel (middleware) transmises

[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) contrôler le comportement de l’intergiciel (middleware) en-têtes transféré :

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-Custom-Header-Name";
});
```

| Option | Description |
| ------ | ----------- |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | Utiliser l’en-tête spécifié par cette propriété à la place de celui spécifié par [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).<br><br>La valeur par défaut est `X-Forwarded-For`. |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | Identifie les redirecteurs doivent être traités. Consultez le [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) pour obtenir la liste des champs qui s’appliquent. Les valeurs par défaut affectés à cette propriété sont <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.<br><br>La valeur par défaut est [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | Utiliser l’en-tête spécifié par cette propriété à la place de celui spécifié par [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).<br><br>La valeur par défaut est `X-Forwarded-Host`. |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | Utiliser l’en-tête spécifié par cette propriété à la place de celui spécifié par [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).<br><br>La valeur par défaut est `X-Forwarded-Proto`. |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | Limite le nombre d’entrées dans les en-têtes qui sont traités. La valeur `null` pour désactiver la limite, mais cela ne doit être effectuée si `KnownProxies` ou `KnownNetworks` sont configurés.<br><br>La valeur par défaut est 1. |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | Plages de proxys connus pour accepter les en-têtes transférés à partir d’adresses. Fournir des plages IP à l’aide de la notation de routage inter-domaines sans classe (CIDR).<br><br>La valeur par défaut est une [IList](/dotnet/api/system.collections.generic.ilist-1)\<[réseau IP](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> contenant une seule entrée pour `IPAddress.Loopback`. |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | Adresses de proxy connus pour accepter les en-têtes transférés à partir de. Utilisez `KnownProxies` pour spécifier l’adresse IP exacte des correspondances.<br><br>La valeur par défaut est une [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> contenant une seule entrée pour `IPAddress.IPv6Loopback`. |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | Utiliser l’en-tête spécifié par cette propriété à la place de celui spécifié par [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).<br><br>La valeur par défaut est `X-Original-For`. |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | Utiliser l’en-tête spécifié par cette propriété à la place de celui spécifié par [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).<br><br>La valeur par défaut est `X-Original-Host`. |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | Utiliser l’en-tête spécifié par cette propriété à la place de celui spécifié par [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).<br><br>La valeur par défaut est `X-Original-Proto`. |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | Nécessitent le nombre de valeurs d’en-tête à une synchronisation entre le [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) en cours de traitement.<br><br>La valeur par défaut dans ASP.NET Core 1.x est `true`. La valeur par défaut dans ASP.NET Core 2.0 ou version ultérieure est `false`. |

## <a name="scenarios-and-use-cases"></a>Scénarios et cas d’usage

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a>Lorsqu’il n’est pas possible d’ajouter transféré les en-têtes et toutes les demandes sont sécurisées

Dans certains cas, il ne peut pas être possible d’ajouter des en-têtes transférés aux requêtes traitées à l’application. Si le proxy en œuvre que toutes les demandes externes publiques sont de type HTTPS, le modèle peut être défini manuellement `Startup.Configure` avant d’utiliser n’importe quel type d’intergiciel (middleware) :

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

Ce code peut être désactivé avec une variable d’environnement ou un autre paramètre de configuration dans un environnement intermédiaire ou de développement.

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a>Traitent de chemin d’accès de base et les proxys de modifier le chemin d’accès de la demande

Certains proxys passer le chemin d’accès intact, mais avec une application chemin d’accès de base qui doit être supprimée pour que le routage fonctionne correctement. [UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) intergiciel (middleware) fractionne le chemin d’accès dans [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) et le chemin de base d’application dans [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).

Si `/foo` est le chemin de base d’application pour un chemin d’accès au proxy passée en tant que `/foo/api/1`, les jeux d’intergiciel (middleware) `Request.PathBase` à `/foo` et `Request.Path` à `/api/1` avec la commande suivante :

```csharp
app.UsePathBase("/foo");
```

Le chemin d’accès et le chemin d’accès de base d’origine sont réappliquées lors de l’intergiciel (middleware) est de nouveau appelée dans l’ordre inverse. Pour plus d’informations sur le traitement des commandes intergiciel (middleware), consultez [intergiciel (middleware)](xref:fundamentals/middleware/index).

Si le serveur proxy supprime le chemin d’accès (par exemple, transfert `/foo/api/1` à `/api/1`), correctif redirige et des liens en définissant la demande [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) propriété :

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

Si le proxy ajoute des données de chemin d’accès, ignorer la partie du chemin d’accès à la résolution des redirections et les liens à l’aide de [StartsWithSegments (PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) et en attribuant à la [chemin d’accès](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) propriété :

```csharp
app.Use((context, next) =>
{
    if (context.Request.Path.StartsWithSegments("/foo", out var remainder))
    {
        context.Request.Path = remainder;
    }

    return next();
});
```

## <a name="troubleshoot"></a>Résoudre les problèmes

Lorsque les en-têtes ne sont pas transférés comme prévu, activer [journalisation](xref:fundamentals/logging/index). Si les journaux ne fournissent pas d’informations suffisantes pour résoudre le problème, permet d’énumérer les en-têtes de demande reçus par le serveur. Les en-têtes peuvent être écrites dans une réponse de l’application à l’aide d’intergiciel (middleware) inline :

```csharp
public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.Run(async (context) =>
    {
        context.Response.ContentType = "text/plain";

        // Request method, scheme, and path
        await context.Response.WriteAsync(
            $"Request Method: {context.Request.Method}{Environment.NewLine}");
        await context.Response.WriteAsync(
            $"Request Scheme: {context.Request.Scheme}{Environment.NewLine}");
        await context.Response.WriteAsync(
            $"Request Path: {context.Request.Path}{Environment.NewLine}");

        // Headers
        await context.Response.WriteAsync($"Request Headers:{Environment.NewLine}");

        foreach (var header in context.Request.Headers)
        {
            await context.Response.WriteAsync($"{header.Key}: " +
                $"{header.Value}{Environment.NewLine}");
        }

        await context.Response.WriteAsync(Environment.NewLine);

        // Connection: RemoteIp
        await context.Response.WriteAsync(
            $"Request RemoteIp: {context.Connection.RemoteIpAddress}");
    }
}
```

Vérifiez que le X - transféré-* en-têtes sont reçus par le serveur avec les valeurs attendues. S’il existe plusieurs valeurs dans un en-tête donné, notez l’intergiciel (middleware) de transféré en-têtes des en-têtes de processus dans l’ordre inverse de droite à gauche.

Adresse IP distante d’origine à la demande doit correspondre à une entrée dans le `KnownProxies` ou `KnownNetworks` répertorie avant X transmis pour le traitement. Cela limite l’usurpation d’identité d’en-tête en n’acceptant ne pas de redirecteurs de proxys non approuvés.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Microsoft Security Advisory CVE-2018-0787 : ASP.NET Core vulnérabilité d’élévation de privilèges](https://github.com/aspnet/Announcements/issues/295)
