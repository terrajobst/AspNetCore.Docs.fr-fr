---
title: Configurer ASP.NET Core pour l’utilisation de serveurs proxy et d’équilibreurs de charge
author: guardrex
description: Découvrez la configuration des applications hébergées derrière des serveurs proxy et des équilibreurs de charge, qui masquent souvent des informations de requête importantes.
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2018
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: 1623c6dbe377ce24c380b75a828c3ec5653dd7dd
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827496"
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a>Configurer ASP.NET Core pour l’utilisation de serveurs proxy et d’équilibreurs de charge

Par [Luke Latham](https://github.com/guardrex) et [Chris Ross](https://github.com/Tratcher)

Dans la configuration recommandée d’ASP.NET Core, l’application est hébergée à l’aide du module IIS/ASP.NET Core, de Nginx ou d’Apache. Les serveurs proxy, les équilibreurs de charge et autres dispositifs réseau masquent souvent les informations sur la requête avant qu’elle n’atteigne l’application :

* Quand les requêtes HTTPS sont transmises par proxy via HTTP, le schéma d’origine (HTTPS) est perdu et doit être transféré dans un en-tête.
* Étant donné qu’une requête adressée à une application transite par le proxy au lieu de provenir directement de sa source sur Internet ou le réseau d’entreprise, l’adresse IP cliente d’origine doit également être transférée dans un en-tête.

Ces informations peuvent être importantes pour traiter les requêtes, par exemple dans les redirections, l’authentification, la génération de lien, l’évaluation des stratégies et la géolocalisation des clients.

## <a name="forwarded-headers"></a>En-têtes transférés

Par convention, les proxys transfèrent les informations dans des en-têtes HTTP.

| Header | Description |
| ------ | ----------- |
| X-Forwarded-For | Contient des informations sur le client à l’origine de la requête et les proxys suivants dans une chaîne de proxys. Ce paramètre peut contenir des adresses IP (et, éventuellement, des numéros de port). Dans une chaîne de serveurs proxy, le premier paramètre indique le client sur lequel la requête a été effectuée pour la première fois. Viennent ensuite les identificateurs des proxy suivants. Le dernier proxy dans la chaîne ne figure pas dans la liste des paramètres. L’adresse IP du dernier proxy et éventuellement un numéro de port sont disponibles en tant qu’adresse IP distante au niveau de la couche transport. |
| X-Forwarded-Proto | Valeur du schéma d’origine (HTTP/HTTPS). La valeur peut également être une liste de schémas si la requête a franchi plusieurs proxys. |
| X-Forwarded-Host | Valeur d’origine du champ d’en-tête de l’hôte. En règle générale, les proxys ne modifient pas l’en-tête de l’hôte. Consultez le document [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) pour plus d’informations sur une vulnérabilité par élévation de privilèges qui affecte les systèmes où le proxy ne valide pas les en-têtes d’hôte ou ne les limite pas à des valeurs correctes connues. |

Le middleware (intergiciel) des en-têtes transférés, dans le package [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/), lit ces en-têtes et renseigne les champs associés sur [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).

Le middleware met à jour les éléments suivants :

* [HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; Défini à l’aide de la valeur d’en-tête `X-Forwarded-For`. Des paramètres supplémentaires déterminent la façon dont le middleware définit `RemoteIpAddress`. Pour plus d’informations, consultez les [options du middleware des en-têtes transférés](#forwarded-headers-middleware-options).
* [HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; Défini à l’aide de la valeur d’en-tête `X-Forwarded-Proto`.
* [HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; Défini à l’aide de la valeur d’en-tête `X-Forwarded-Host`.

Notez que toutes les appliances réseau n’ajoutent pas les en-têtes `X-Forwarded-For` et `X-Forwarded-Proto` sans qu’il soit nécessaire d’effectuer une configuration supplémentaire. Consultez les instructions du fabricant de votre appliance si les requêtes transmises par proxy ne contiennent pas ces en-têtes quand elles atteignent l’application.

Vous pouvez configurer les [paramètres par défaut](#forwarded-headers-middleware-options) du middleware des en-têtes transférés. Le paramétrage par défaut est le suivant :

* Il y a *un seul proxy* entre l’application et la source des requêtes.
* Seules des adresses de bouclage sont configurées pour les proxys connus et les réseaux connus.

## <a name="iisiis-express-and-aspnet-core-module"></a>Module ASP.NET Core et IIS/IIS Express

Le middleware des en-têtes transférés est activé par défaut par le middleware d’intégration IIS quand l’application est exécutée derrière IIS et le module ASP.NET Core. Le middleware des en-têtes transférés est activé pour s’exécuter en premier dans le pipeline des middlewares avec une configuration limitée propre au module ASP.NET Core en raison de problèmes d’approbation liés aux en-têtes transférés (par exemple, [usurpation d’adresse IP](https://www.iplocation.net/ip-spoofing)). Le middleware est configuré pour transférer les en-têtes `X-Forwarded-For` et `X-Forwarded-Proto` et est limité à un proxy localhost unique. Si une configuration supplémentaire est requise, consultez les [options du middleware des en-têtes transférés](#forwarded-headers-middleware-options).

## <a name="other-proxy-server-and-load-balancer-scenarios"></a>Autres scénarios avec un serveur proxy et un équilibreur de charge

En dehors de l’utilisation du middleware d’intégration IIS, le middleware des en-têtes transférés n’est pas activé par défaut. Le middleware des en-têtes transférés doit être activé pour qu’une application puisse traiter les en-têtes transférés avec [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders). Une fois activé le middleware si aucune option [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) ne lui est spécifiée, les propriétés [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) par défaut ont pour valeur [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).

Configurez le middleware avec [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) pour transférer les en-têtes `X-Forwarded-For` et `X-Forwarded-Proto` dans `Startup.ConfigureServices`. Appelez la méthode [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) dans `Startup.Configure` avant d’appeler d’autres middlewares :

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
> Si aucune option [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) n’est spécifiée dans `Startup.ConfigureServices` ou directement dans la méthode d’extension avec [UseForwardedHeaders(IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), les en-têtes par défaut à transférer ont pour valeur [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). La propriété [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) doit être configurée avec les en-têtes à transférer.

## <a name="nginx-configuration"></a>Configuration de Nginx

Pour transférer les en-têtes `X-Forwarded-For` et `X-Forwarded-Proto`, consultez [Host on Linux with Nginx: Configure Nginx](xref:host-and-deploy/linux-nginx#configure-nginx) (Héberger sur Linux avec Nginx : configurer Nginx). Pour plus d’informations, consultez [NGINX: Using the Forwarded header](https://www.nginx.com/resources/wiki/start/topics/examples/forwarded/) (NGINX : utilisation de l’en-tête Forwarded).

## <a name="apache-configuration"></a>Configuration d’Apache

`X-Forwarded-For` est ajouté automatiquement (consultez [Module Apache mod_proxy : en-têtes de requête du mandataire inverse](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html#x-headers)). Pour plus d’informations sur la façon de transférer l’en-tête `X-Forwarded-Proto`, consultez [Héberger sur Linux avec Apache : configurer Apache](xref:host-and-deploy/linux-apache#configure-apache).

## <a name="forwarded-headers-middleware-options"></a>Options du middleware des en-têtes transférés

Les options [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) contrôlent le comportement du middleware des en-têtes transférés :

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-Custom-Header-Name";
});
```

::: moniker range="<= aspnetcore-2.0"
| Option | Description |
| ------ | ----------- |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | Utilisez l’en-tête spécifié par cette propriété à la place de celui spécifié par [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).<br><br>La valeur par défaut est `X-Forwarded-For`. |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | Identifie les redirecteurs à traiter. Consultez [l’énumération ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) pour obtenir la liste des champs qui s’appliquent. En règle générale, les valeurs attribuées à cette propriété sont <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.<br><br>La valeur par défaut est [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | Utilisez l’en-tête spécifié par cette propriété à la place de celui spécifié par [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).<br><br>La valeur par défaut est `X-Forwarded-Host`. |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | Utilisez l’en-tête spécifié par cette propriété à la place de celui spécifié par [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).<br><br>La valeur par défaut est `X-Forwarded-Proto`. |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | Limite le nombre d’entrées dans les en-têtes qui sont traités. Définissez cette option sur `null` pour désactiver la limite, mais seulement si `KnownProxies` ou `KnownNetworks` sont configurés.<br><br>La valeur par défaut est 1. |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | Plages d’adresses des proxys connus à partir desquels les en-têtes transférés peuvent être acceptés. Indiquez les plages d’adresses IP à l’aide de la notation CIDR (Classless Interdomain Routing).<br><br>La valeur par défaut est un [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> contenant une seule entrée pour `IPAddress.Loopback`. |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | Adresses des proxy connus à partir desquels les en-têtes transférés peuvent être acceptés. Utilisez `KnownProxies` pour spécifier des correspondances d’adresse IP exactes.<br><br>La valeur par défaut est un [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> contenant une seule entrée pour `IPAddress.IPv6Loopback`. |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | Utilisez l’en-tête spécifié par cette propriété à la place de celui spécifié par [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).<br><br>La valeur par défaut est `X-Original-For`. |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | Utilisez l’en-tête spécifié par cette propriété à la place de celui spécifié par [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).<br><br>La valeur par défaut est `X-Original-Host`. |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | Utilisez l’en-tête spécifié par cette propriété à la place de celui spécifié par [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).<br><br>La valeur par défaut est `X-Original-Proto`. |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | Cette option impose que le nombre de valeurs d’en-tête soit synchronisé avec les [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) en cours de traitement.<br><br>La valeur par défaut dans ASP.NET Core 1.x est `true`. La valeur par défaut dans ASP.NET Core versions 2.0 ou ultérieures est `false`. |
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
| Option | Description |
| ------ | ----------- |
| AllowedHosts | Limite les hôtes par l’en-tête `X-Forwarded-Host` aux valeurs fournies.<ul><li>Les valeurs sont comparées à l’aide de l’option ordinal-ignore-case.</li><li>Les numéros de port doivent être exclus.</li><li>Si la liste est vide, tous les hôtes sont autorisés.</li><li>Un caractère générique de niveau supérieur `*` autorise tous les hôtes non vides.</li><li>Les caractères génériques de sous-domaine sont autorisés, mais ne correspondent pas au domaine racine. Par exemple, `*.contoso.com` correspond au sous-domaine `foo.contoso.com`, mais pas au domaine racine `contoso.com`.</li><li>Les noms d’hôte Unicode sont autorisés, mais sont convertis en [Punycode](https://tools.ietf.org/html/rfc3492) à des fins de correspondance.</li><li>Les [adresses IPv6](https://tools.ietf.org/html/rfc4291) doivent inclure des crochets de délimitation et adopter une [forme conventionnelle](https://tools.ietf.org/html/rfc4291#section-2.2) (par exemple, `[ABCD:EF01:2345:6789:ABCD:EF01:2345:6789]`). Les adresses IPv6 ne sont pas les seules à rechercher une égalité logique entre différents formats, et aucune canonicalisation n’est effectuée.</li><li>La non-restriction des hôtes autorisés peut permettre à un attaquant d’usurper les liens générés par le service.</li></ul>La valeur par défaut est un [IList\<string>](/dotnet/api/system.collections.generic.ilist-1) vide. |
| [ForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | Utilisez l’en-tête spécifié par cette propriété à la place de celui spécifié par [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).<br><br>La valeur par défaut est `X-Forwarded-For`. |
| [ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | Identifie les redirecteurs à traiter. Consultez [l’énumération ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) pour obtenir la liste des champs qui s’appliquent. En règle générale, les valeurs attribuées à cette propriété sont <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.<br><br>La valeur par défaut est [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders). |
| [ForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | Utilisez l’en-tête spécifié par cette propriété à la place de celui spécifié par [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).<br><br>La valeur par défaut est `X-Forwarded-Host`. |
| [ForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | Utilisez l’en-tête spécifié par cette propriété à la place de celui spécifié par [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).<br><br>La valeur par défaut est `X-Forwarded-Proto`. |
| [ForwardLimit](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | Limite le nombre d’entrées dans les en-têtes qui sont traités. Définissez cette option sur `null` pour désactiver la limite, mais seulement si `KnownProxies` ou `KnownNetworks` sont configurés.<br><br>La valeur par défaut est 1. |
| [KnownNetworks](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | Plages d’adresses des proxys connus à partir desquels les en-têtes transférés peuvent être acceptés. Indiquez les plages d’adresses IP à l’aide de la notation CIDR (Classless Interdomain Routing).<br><br>La valeur par défaut est un [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> contenant une seule entrée pour `IPAddress.Loopback`. |
| [KnownProxies](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | Adresses des proxy connus à partir desquels les en-têtes transférés peuvent être acceptés. Utilisez `KnownProxies` pour spécifier des correspondances d’adresse IP exactes.<br><br>La valeur par défaut est un [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> contenant une seule entrée pour `IPAddress.IPv6Loopback`. |
| [OriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | Utilisez l’en-tête spécifié par cette propriété à la place de celui spécifié par [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).<br><br>La valeur par défaut est `X-Original-For`. |
| [OriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | Utilisez l’en-tête spécifié par cette propriété à la place de celui spécifié par [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).<br><br>La valeur par défaut est `X-Original-Host`. |
| [OriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | Utilisez l’en-tête spécifié par cette propriété à la place de celui spécifié par [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).<br><br>La valeur par défaut est `X-Original-Proto`. |
| [RequireHeaderSymmetry](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | Cette option impose que le nombre de valeurs d’en-tête soit synchronisé avec les [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) en cours de traitement.<br><br>La valeur par défaut dans ASP.NET Core 1.x est `true`. La valeur par défaut dans ASP.NET Core versions 2.0 ou ultérieures est `false`. |
::: moniker-end

## <a name="scenarios-and-use-cases"></a>Scénarios et cas d’usage

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a>Quand il est impossible d’ajouter des en-têtes transférés et que toutes les requêtes sont sécurisées

Dans certains cas, il peut être impossible d’ajouter des en-têtes transférés aux requêtes qui sont transmises par proxy à l’application. Si le proxy impose que toutes les requêtes externes publiques soient de type HTTPS, vous pouvez définir le schéma manuellement dans `Startup.Configure` avant d’utiliser tout type de middleware :

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

Vous pouvez désactiver ce code avec une variable d’environnement ou un autre paramètre de configuration dans un environnement de préproduction ou de développement.

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a>Traiter la base du chemin et les proxys qui changent le chemin de la requête

Certains proxys passent le chemin tel quel, mais avec un chemin de base d’application qui doit être supprimé afin que le routage fonctionne correctement. Le middleware [UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) fractionne le chemin en [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) et le chemin de base d’application en [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).

Si `/foo` est le chemin de base d’application pour un chemin de proxy passé en tant que `/foo/api/1`, le middleware définit `Request.PathBase` sur `/foo` et `Request.Path` sur `/api/1` avec la commande suivante :

```csharp
app.UsePathBase("/foo");
```

Le chemin et la base du chemin d’origine sont réappliqués quand le middleware est rappelé dans l’ordre inverse. Pour plus d’informations sur l’ordre de traitement des middlewares, consultez [Middleware](xref:fundamentals/middleware/index).

Si le proxy supprime le chemin (par exemple, en transférant `/foo/api/1` vers `/api/1`), corrigez les redirections et les liens en définissant la propriété [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) de la requête :

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

Si le proxy ajoute des données de chemin, ignorez la partie du chemin pour corriger les redirections et les liens en utilisant [StartsWithSegments(PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) et en définissant la propriété [Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) :

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

Quand des en-têtes ne sont pas transférés comme prévu, activez la [journalisation](xref:fundamentals/logging/index). Si les journaux ne fournissent pas suffisamment d’informations pour résoudre le problème, énumérez les en-têtes de requête reçus par le serveur. Les en-têtes peuvent être écrits dans une réponse d’application à l’aide d’un middleware inline :

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
    });
}
```

Vérifiez que les en-têtes X-Forwarded-*sont reçus par le serveur avec les valeurs attendues. S’il existe plusieurs valeurs dans un en-tête donné, notez que le middleware des en-têtes transférés traite les en-têtes dans l’ordre inverse de droite à gauche.

L’adresse IP distante d’origine de la requête doit correspondre à une entrée dans les listes `KnownProxies` ou `KnownNetworks` pour que X-Forwarded-For puisse être traité. L’usurpation des en-têtes s’en trouve limitée, car les redirecteurs émanant de proxys non approuvés ne sont pas acceptés.

## <a name="additional-resources"></a>Ressources supplémentaires

* [Microsoft Security Advisory CVE-2018-0787 : vulnérabilité d’élévation de privilèges ASP.NET Core](https://github.com/aspnet/Announcements/issues/295)
