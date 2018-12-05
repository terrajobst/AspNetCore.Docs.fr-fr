---
title: Compression des réponses dans ASP.NET Core
author: guardrex
description: Découvrez ce qu’est la compression des réponses et comment utiliser le middleware de compression des réponses dans les applications ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: performance/response-compression
ms.openlocfilehash: 2516fbb30e55990dc4ad0d92069853bc26874bc9
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861886"
---
# <a name="response-compression-in-aspnet-core"></a>Compression des réponses dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

La bande passante réseau est une ressource limitée. Le fait de réduire la taille de la réponse augmente généralement la réactivité d’une application, parfois de manière considérable. Une façon de réduire la taille de la charge utile consiste à compresser les réponses de l’application.

## <a name="when-to-use-response-compression-middleware"></a>Quand utiliser l’intergiciel (middleware) de compression de réponse

Utilisez les technologies de compression de réponse basées sur le serveur dans IIS, Apache ou Nginx. Les performances de l’intergiciel (middleware) ne correspondront probablement pas à celles des modules de serveur. [Serveur HTTP.sys](xref:fundamentals/servers/httpsys) serveur et [Kestrel](xref:fundamentals/servers/kestrel) serveur n’offrent actuellement pas prise en charge de la compression intégrée.

Utilisez l’intergiciel (middleware) de compression de réponse dans les cas suivants :

* Impossibilité d’utiliser les technologies de compression suivantes basées sur le serveur :
  * [Module de compression dynamique IIS](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [Module Apache mod_deflate](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [Décompression et compression Nginx](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* Hébergement directement sur :
  * [HTTP.sys](xref:fundamentals/servers/httpsys) server (anciennement appelé [WebListener](xref:fundamentals/servers/weblistener))
  * [Kestrel](xref:fundamentals/servers/kestrel) server

## <a name="response-compression"></a>Compression des réponses

En règle générale, toute réponse ne compressée pas en mode natif peut bénéficier de la compression de réponse. Les réponses pas en mode natif compressés en général : CSS, JavaScript, HTML, XML et JSON. Vous ne doivent pas compresser des ressources compressées en mode natif, telles que des fichiers PNG. Si vous tentez de compresser davantage une réponse compressée en mode natif, une petite réduction supplémentaire dans le temps de taille et la transmission sera probablement être été éclipsée par le temps de traitement de la compression. Ne pas compresser les fichiers inférieurs à environ 150-1000 octets (selon le contenu du fichier et l’efficacité de la compression). La surcharge de la compression des fichiers de petite taille peut produire un fichier compressé plus volumineux que le fichier non compressé.

Quand un client peut traiter du contenu compressé, il doit en informer le serveur en envoyant l'en-tête `Accept-Encoding` avec la requête. Quand un serveur envoie du contenu compressé, il doit inclure dans l’en-tête `Content-Encoding` des informations sur le mode d’encodage de la réponse compressée. Les désignations d'encodage de contenu prises en charge par l’intergiciel sont affichées dans le tableau suivant.

::: moniker range=">= aspnetcore-2.2"

| Valeurs d’en-tête `Accept-Encoding` | Intergiciel pris en charge | Description |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | Oui (valeur par défaut)        | [Format de données compressées Brotli](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | Non                   | [Format de données compressées DEFLATE](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | Non                   | [Échange efficace de XML W3C](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | Oui                  | [Format de fichier GZIP](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | Oui                  | Identificateur "No encoding" : la réponse ne doit pas être encodée. |
| `pack200-gzip`                  | Non                   | [Format de transfert de réseau d’Archives Java](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | Oui                  | N'importe quel encodage de contenu disponible non explicitement demandé |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| Valeurs d’en-tête `Accept-Encoding` | Intergiciel pris en charge | Description |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | Non                   | [Format de données compressées Brotli](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | Non                   | [Format de données compressées DEFLATE](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | Non                   | [Échange efficace de XML W3C](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | Oui (valeur par défaut)        | [Format de fichier GZIP](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | Oui                  | Identificateur "No encoding" : la réponse ne doit pas être encodée. |
| `pack200-gzip`                  | Non                   | [Format de transfert de réseau d’Archives Java](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | Oui                  | N'importe quel encodage de contenu disponible non explicitement demandé |

::: moniker-end

Pour plus d’informations, consultez la [liste IANA officielle des encodages de contenu](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).

L’intergiciel vous permet d’ajouter des fournisseurs de compression supplémentaires pour des valeurs d’en-tête `Accept-Encoding` personnalisées. Pour plus d’informations, consultez [Fournisseurs personnalisés](#custom-providers) ci-dessous.

L’intergiciel peut réagir à la pondération de la valeur de qualité (qvalue, `q`) envoyée par le client pour hiérarchiser les schémas de compression. Pour plus d’informations, consultez le [document RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).

Les algorithmes de compression donnent lieu à un compromis entre vitesse de compression et efficacité de la compression. L’*Efficacité* dans ce contexte fait référence à la taille de la sortie après la compression. La plus petite taille est obtenue par la compression la plus *optimale*.

Les en-têtes impliqués dans la demande, l'envoi, la mise en cache et la réception de contenu compressé sont décrits dans le tableau ci-dessous.

| Header             | Rôle |
| ------------------ | ---- |
| `Accept-Encoding`  | Envoyé à partir du client au serveur pour indiquer les schémas d’encodage de contenu acceptables pour le client. |
| `Content-Encoding` | Envoyé à partir du serveur au client pour indiquer l'encodage du contenu de la charge utile. |
| `Content-Length`   | Au moment de la compression, l'en-tête `Content-Length` est supprimé car le contenu du corps change quand la réponse est compressée. |
| `Content-MD5`      | Durant la compression, l’en-tête `Content-MD5` est supprimé puisque le contenu du corps a changé et que le hachage n’est plus valide. |
| `Content-Type`     | Spécifie le type MIME du contenu. Chaque réponse doit spécifier son `Content-Type`. L’intergiciel vérifie cette valeur pour déterminer si la réponse doit être compressée. L’intergiciel (middleware) spécifie un ensemble de [par défaut des types MIME](#mime-types) qui peut donc coder, mais vous pouvez remplacer ou ajouter des types MIME. |
| `Vary`             | Quand l’en-tête `Vary` est envoyé par le serveur avec la valeur `Accept-Encoding` à des clients et proxys, ces derniers doivent mettre en cache les réponses (vary) en fonction de la valeur de l’en-tête `Accept-Encoding` de la requête. Si du contenu est retourné avec l'en-tête `Vary: Accept-Encoding`, les réponses (compressées et non compressées) sont mises en cache séparément. |

Explorez les fonctionnalités de l’intergiciel de Compression de réponse avec le [exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples). L’exemple illustre :

* La compression des réponses d’application à l’aide de Gzip et fournisseurs de compression personnalisé.
* Comment ajouter un type MIME à la liste par défaut des types MIME pour la compression.

## <a name="package"></a>Package

::: moniker range=">= aspnetcore-2.1"

Pour inclure l’intergiciel (middleware) dans un projet, ajoutez une référence à la [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app), qui inclut le [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Pour inclure l’intergiciel (middleware) dans un projet, ajoutez une référence à la [métapackage Microsoft.AspNetCore.All](xref:fundamentals/metapackage), qui inclut le [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Pour inclure l’intergiciel (middleware) dans un projet, ajoutez une référence à la [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.

::: moniker-end

## <a name="configuration"></a>Configuration

::: moniker range=">= aspnetcore-2.2"

Le code suivant montre comment activer le Middleware de Compression de réponse pour les types MIME par défaut et les fournisseurs de compression ([Brotli](#brotli-compression-provider) et [Gzip](#gzip-compression-provider)) :

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Le code suivant montre comment activer le Middleware de Compression de réponse pour les types MIME par défaut et le [fournisseur de Compression Gzip](#gzip-compression-provider):

::: moniker-end

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddResponseCompression();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        app.UseResponseCompression();
    }
}
```

> [!NOTE]
> Utilisez un outil tel que [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), ou [Postman](https://www.getpostman.com/) pour définir l'en-tête `Accept-Encoding` de la requête et étudier les en-têtes, la taille et le corps de la réponse.

Soumettez une requête à l’exemple d’application sans l’en-tête `Accept-Encoding` et observez que la réponse n’est pas compressée. Les en-têtes  `Content-Encoding` et `Vary` ne sont pas présents sur la réponse.

![Fenêtre Fiddler indiquant le résultat d’une requête sans l’en-tête Accept-Encoding. La réponse n’est pas compressée.](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

Soumettre une demande de l’exemple d’application avec le `Accept-Encoding: br` en-tête (compression Brotli) et observez que la réponse est compressée. Les en-têtes `Content-Encoding` et `Vary` sont présents sur la réponse.

![Fenêtre Fiddler montrant le résultat d’une demande avec l’en-tête Accept-Encoding et la valeur br. Les en-têtes Vary et Content-Encoding sont ajoutés à la réponse. La réponse est compressée.](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Soumettez une requête à l’exemple d’application avec l’en-tête `Accept-Encoding: gzip` et observez que la réponse est compressée. Les en-têtes `Content-Encoding` et `Vary` sont présents sur la réponse.

![Fenêtre Fiddler montrant le résultat d’une demande avec l’en-tête Accept-Encoding et la valeur gzip. Les en-têtes Vary et Content-Encoding sont ajoutés à la réponse. La réponse est compressée.](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a>Fournisseurs

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a>Fournisseur de Compression Brotli

Utilisez le `BrotliCompressionProvider` pour compresser les réponses avec le [format de données compressées Brotli](https://tools.ietf.org/html/rfc7932).

Si aucun fournisseur de compression n’est ajoutés explicitement à la <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:

* Le fournisseur de Compression Brotli est ajouté par défaut dans le tableau de fournisseurs de compression avec la [fournisseur de compression Gzip](#gzip-compression-provider).
* La compression par défaut de compression Brotli lorsque le format de données compressées Brotli est pris en charge par le client. Si Brotli n’est pas pris en charge par le client, la compression par défaut Gzip lorsque le client prend en charge la compression Gzip.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

Le fournisseur de Compression Brotoli doit être ajouté lorsque des fournisseurs de compression sont ajoutés explicitement :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

Définir la compression au niveau avec `BrotliCompressionProviderOptions`. Le fournisseur de Compression Brotli par défaut est le niveau de compression plus rapide ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), ce qui ne peut pas produire la compression la plus efficace. Si la compression la plus efficace est souhaitée, configurez l’intergiciel de compression optimale.

| Niveau de compression | Description |
| ----------------- | ----------- |
| [CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel) | La compression doit se terminer aussi rapidement que possible, même si le résultat n’est pas compressé de manière optimale. |
| [CompressionLevel.NoCompression](xref:System.IO.Compression.CompressionLevel) | Aucune compression ne doit être effectuée. |
| [CompressionLevel.Optimal](xref:System.IO.Compression.CompressionLevel) | Les réponses doivent être compressées optimale, même si la compression prend plus de temps. |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<BrotliCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

::: moniker-end

### <a name="gzip-compression-provider"></a>Fournisseur de Compression GZIP

Utilisez le <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> pour compresser les réponses avec le [format de fichier Gzip](https://tools.ietf.org/html/rfc1952).

Si aucun fournisseur de compression n’est ajoutés explicitement à la <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:

::: moniker range=">= aspnetcore-2.2"

* Le fournisseur de la Compression Gzip est ajouté par défaut dans le tableau de fournisseurs de compression avec la [fournisseur de Compression Brotli](#brotli-compression-provider).
* La compression par défaut de compression Brotli lorsque le format de données compressées Brotli est pris en charge par le client. Si Brotli n’est pas pris en charge par le client, la compression par défaut Gzip lorsque le client prend en charge la compression Gzip.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* Le fournisseur de la Compression Gzip est ajouté au tableau de fournisseurs de compression par défaut.
* Par défaut, la compression est Gzip lorsque le client prend en charge la compression Gzip.

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

Le fournisseur de la Compression Gzip doit être ajouté lorsque des fournisseurs de compression sont ajoutés explicitement :

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5)]

::: moniker-end

Définir la compression au niveau avec <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>. Le fournisseur de la Compression Gzip par défaut est le niveau de compression plus rapide ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), ce qui ne peut pas produire la compression la plus efficace. Si la compression la plus efficace est souhaitée, configurez l’intergiciel de compression optimale.

| Niveau de compression | Description |
| ----------------- | ----------- |
| [CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel) | La compression doit se terminer aussi rapidement que possible, même si le résultat n’est pas compressé de manière optimale. |
| [CompressionLevel.NoCompression](xref:System.IO.Compression.CompressionLevel) | Aucune compression ne doit être effectuée. |
| [CompressionLevel.Optimal](xref:System.IO.Compression.CompressionLevel) | Les réponses doivent être compressées optimale, même si la compression prend plus de temps. |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<GzipCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

### <a name="custom-providers"></a>Fournisseurs personnalisés

Créer des implémentations de compression personnalisé avec <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>. <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> représente le contenu que cet encodage `ICompressionProvider` produit. L’intergiciel utilise ces informations pour choisir le fournisseur en fonction de la liste spécifiée dans l'en-tête `Accept-Encoding` de la requête.

À l’aide de l’exemple d’application, le client envoie une requête avec l'en-tête `Accept-Encoding: mycustomcompression`. L’intergiciel utilise l’implémentation de compression personnalisée et retourne la réponse avec un en-tête `Content-Encoding: mycustomcompression`. Pour que cette implémentation fonctionne, le client doit pouvoir décompresser l’encodage personnalisé.

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

```csharp
public class CustomCompressionProvider : ICompressionProvider
{
    public string EncodingName => "mycustomcompression";
    public bool SupportsFlush => true;

    public Stream CreateStream(Stream outputStream)
    {
        // Create a custom compression stream wrapper here
        return outputStream;
    }
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=6,12-15)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6,12-15)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

Soumettez une requête à l’exemple d’application avec l’en-tête `Accept-Encoding: mycustomcompression` et observez les en-têtes de réponse. Les en-têtes `Vary` et `Content-Encoding` sont présents sur la réponse. Le corps de la réponse (non affiché) n’est pas compressé par l’exemple. Il n’y a pas d’implémentation de compression dans la classe `CustomCompressionProvider` de l’exemple. Toutefois, ce dernier montre où vous pouvez implémenter un tel algorithme de compression.

![Fenêtre Fiddler montrant le résultat d’une demande avec l’en-tête Accept-Encoding et la valeur mycustomcompression. Les en-têtes Vary et Content-Encoding sont ajoutés à la réponse.](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a>types MIME

L’intergiciel (middleware) spécifie un ensemble de types MIME pour la compression par défaut :

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

Remplacer ou ajouter des types MIME avec les options de l’intergiciel de Compression des réponses. Notez que les caractères génériques MIME types, tels que `text/*` ne sont pas pris en charge. L’exemple d’application ajoute un type MIME pour `image/svg+xml` et compresse et sert à ASP.NET Core image de bannière (*banner.svg*).

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7-9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a>Compression avec protocole sécurisé

Les réponses compressées sur des connexions sécurisées peuvent être contrôlées à l'aide de l'option `EnableForHttps` (désactivée par défaut). L’utilisation de la compression avec des pages générées dynamiquement peut mener à des problèmes de sécurité tels que les attaques [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) et [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)).

## <a name="adding-the-vary-header"></a>Ajout de l’en-tête Vary

::: moniker range=">= aspnetcore-2.0"

Lors de la compression des réponses basés sur le `Accept-Encoding` en-tête, il existe potentiellement plusieurs versions compressées de la réponse et une version non compressée. Pour indiquer les caches de client et de proxy que plusieurs versions existent et doivent être stockées, les `Vary` en-tête est ajouté avec un `Accept-Encoding` valeur. Dans ASP.NET Core 2.0 ou version ultérieure, l’intergiciel (middleware) ajoute le `Vary` en-tête automatiquement lors de la réponse est compressée.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Lors de la compression des réponses basés sur le `Accept-Encoding` en-tête, il existe potentiellement plusieurs versions compressées de la réponse et une version non compressée. Pour indiquer les caches de client et de proxy que plusieurs versions existent et doivent être stockées, les `Vary` en-tête est ajouté avec un `Accept-Encoding` valeur. Dans ASP.NET Core 1.x, ajout de la `Vary` en-tête à la réponse s’effectue manuellement :

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a>Problème au niveau de l’intergiciel s'il est derrière un proxy inverse Nginx

Quand une requête est transmise par Nginx, l'en-tête `Accept-Encoding` est supprimé. Suppression de la `Accept-Encoding` en-tête empêche l’intergiciel (middleware) de la compression de la réponse. Pour plus d’informations, consultez [NGINX : Compression et décompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/). Ce problème est suivi par [déterminer compression pass-through pour Nginx (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123).

## <a name="working-with-iis-dynamic-compression"></a>Utilisation de la compression dynamique IIS

Si vous avez une active IIS Compression Module dynamique configuré au niveau du serveur que vous souhaitez désactiver pour une application, désactiver le module avec un ajout à la *web.config* fichier. Pour plus d’informations, consultez [Désactivation de modules IIS](xref:host-and-deploy/iis/modules#disabling-iis-modules).

## <a name="troubleshooting"></a>Résolution des problèmes

Utilisez un outil tel que [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/) ou [Postman](https://www.getpostman.com/) pour définir l'en-tête `Accept-Encoding` de la requête et étudier les en-têtes, la taille et le corps de la réponse. Par défaut, intergiciel de Compression des réponses compresse les réponses qui remplissent les conditions suivantes :

::: moniker range=">= aspnetcore-2.2"

* Le `Accept-Encoding` en-tête est présent avec la valeur `br`, `gzip`, `*`, ou de l’encodage personnalisé qui correspond à un fournisseur de compression personnalisé que vous avez établi. La valeur ne doit pas être `identity` ou avoir une valeur de qualité (qvalue, `q`) égale à 0 (zéro).
* Le type MIME (`Content-Type`) doit être défini et doit correspondre à un type MIME configuré sur le <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.
* La requête ne doit pas inclure l'en-tête `Content-Range`.
* La requête doit utiliser un protocole non sécurisé (http), sauf si un protocole sécurisé (https) est configuré dans les options de l’intergiciel de compression de la réponse. *Notez le danger [décrit ci-dessus](#compression-with-secure-protocol) lors de l’activation de la compression de contenu sécurisée.*

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* L'en-tête `Accept-Encoding` est présent avec la valeur `gzip`, `*`, ou un encodage personnalisé qui correspond à un fournisseur de compression personnalisé que vous avez établi. La valeur ne doit pas être `identity` ou avoir une valeur de qualité (qvalue, `q`) égale à 0 (zéro).
* Le type MIME (`Content-Type`) doit être défini et doit correspondre à un type MIME configuré sur le <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.
* La requête ne doit pas inclure l'en-tête `Content-Range`.
* La requête doit utiliser un protocole non sécurisé (http), sauf si un protocole sécurisé (https) est configuré dans les options de l’intergiciel de compression de la réponse. *Notez le danger [décrit ci-dessus](#compression-with-secure-protocol) lors de l’activation de la compression de contenu sécurisée.*

::: moniker-end

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [Réseau de développeurs de Mozilla : Encodage](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [RFC 7231 relative aux Section 3.1.2.1 : Codings contenus](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [RFC 7230 Section 4.2.3 : Codage de Gzip](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [Version de spécification du format fichier GZIP 4.3](http://www.ietf.org/rfc/rfc1952.txt)
