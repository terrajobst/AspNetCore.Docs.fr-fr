---
title: Intergiciel (middleware) de Compression de réponse pour ASP.NET Core
author: guardrex
description: En savoir plus sur la compression de la réponse et comment utiliser un intergiciel (middleware) de Compression de réponse dans les applications ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/response-compression
ms.openlocfilehash: 152799500577dd09247bcee8c87cde39ca20aa79
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729572"
---
# <a name="response-compression-middleware-for-aspnet-core"></a>Intergiciel (middleware) de Compression de réponse pour ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

La bande passante réseau est une ressource limitée. Le fait de réduire la taille de la réponse augmente généralement la réactivité d’une application, parfois de manière considérable. Une façon de réduire la taille de la charge utile consiste à compresser les réponses de l’application.

## <a name="when-to-use-response-compression-middleware"></a>Quand utiliser l’intergiciel (middleware) de compression de réponse

Utilisez les technologies de compression de réponse basées sur le serveur dans IIS, Apache ou Nginx. Les performances de l’intergiciel (middleware) ne correspondront probablement pas à celles des modules de serveur. Les serveurs [HTTP.sys](xref:fundamentals/servers/httpsys) et [Kestrel](xref:fundamentals/servers/kestrel) ne prennent pas actuellement en charge la compression intégrée.

Utilisez l’intergiciel (middleware) de compression de réponse dans les cas suivants :

* Impossibilité d’utiliser les technologies de compression suivantes basées sur le serveur :
  * [Module de compression dynamique IIS](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [Module Apache mod_deflate](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [Décompression et compression Nginx](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* Hébergement directement sur :
  * [Serveur HTTP.sys](xref:fundamentals/servers/httpsys) (anciennement appelé [WebListener](xref:fundamentals/servers/weblistener))
  * [Kestrel](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a>Compression de la réponse

En règle générale, toute réponse ne compressée pas en mode natif peut bénéficier de la compression de la réponse. Pas en mode natif compressées en général, les réponses sont : CSS, JavaScript, HTML, XML et JSON. Vous ne doivent pas compresser les actifs compressés en mode natif, telles que des fichiers PNG. Si vous tentez de davantage de compresser une réponse compressée en mode natif, toute petite réduction supplémentaire dans le temps de taille et de transmission sera probablement être éclipsée par le temps de traitement de la compression. Ne pas compresser les fichiers inférieurs à environ 150-1000 octets (selon le contenu du fichier et l’efficacité de compression). La surcharge de la compression des petits fichiers peut produire un fichier compressé plus volumineux que le fichier non compressé.

Quand un client peut traiter du contenu compressé, il doit en informer le serveur en envoyant l'en-tête `Accept-Encoding` avec la requête. Quand un serveur envoie du contenu compressé, il doit inclure dans l’en-tête `Content-Encoding` des informations sur le mode d’encodage de la réponse compressée. Les désignations d'encodage de contenu prises en charge par l’intergiciel sont affichées dans le tableau suivant.

| Valeurs d’en-tête `Accept-Encoding` | Intergiciel pris en charge | Description                                                 |
| ------------------------------- | :------------------: | ----------------------------------------------------------- |
| `br`                            | Non                   | Format de données compressées Brotli                               |
| `compress`                      | Non                   | Format de données UNIX « compress »                                 |
| `deflate`                       | Non                   | Données compressées au format de données « zlib » « deflate »     |
| `exi`                           | Non                   | Efficient XML Interchange (W3C)                               |
| `gzip`                          | Oui (valeur par défaut)        | format de fichier GZIP                                            |
| `identity`                      | Oui                  | Identificateur "No encoding" : la réponse ne doit pas être encodée. |
| `pack200-gzip`                  | Non                   | Format de transfert réseau pour les Archives de Java                   |
| `*`                             | Oui                  | N'importe quel encodage de contenu disponible non explicitement demandé     |

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
| `Content-Type`     | Spécifie le type MIME du contenu. Chaque réponse doit spécifier son `Content-Type`. L’intergiciel vérifie cette valeur pour déterminer si la réponse doit être compressée. L’intergiciel (middleware) spécifie un jeu de [par défaut des types MIME](#mime-types) il peut encoder, mais vous pouvez remplacer ou ajouter des types MIME. |
| `Vary`             | Quand l’en-tête `Vary` est envoyé par le serveur avec la valeur `Accept-Encoding` à des clients et proxys, ces derniers doivent mettre en cache les réponses (vary) en fonction de la valeur de l’en-tête `Accept-Encoding` de la requête. Si du contenu est retourné avec l'en-tête `Vary: Accept-Encoding`, les réponses (compressées et non compressées) sont mises en cache séparément. |

Vous pouvez explorer les fonctionnalités de l’intergiciel de compression de réponse avec l’[exemple d’application](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples). L’exemple illustre :

* La compression des réponses d’application à l’aide de gzip et fournisseurs de compression personnalisé.
* Comment ajouter un type MIME à la liste par défaut des types MIME pour la compression.

## <a name="package"></a>Package

::: moniker range="< aspnetcore-2.0"

Pour inclure l’intergiciel (middleware) dans votre projet, ajoutez une référence à la [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package. Cette fonctionnalité est disponible pour les applications qui ciblent ASP.NET Core 1.1 ou versions ultérieures.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Pour inclure l’intergiciel (middleware) dans votre projet, ajoutez une référence à la [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) du package ou utilisez le [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).

::: moniker-end

::: moniker range="> aspnetcore-2.0"

Pour inclure l’intergiciel (middleware) dans votre projet, ajoutez une référence à la [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) du package ou utilisez le [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).

::: moniker-end

## <a name="configuration"></a>Configuration

Le code suivant montre comment activer l’intergiciel de compression de la réponse avec la compression gzip par défaut et pour les types MIME par défaut.

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

Soumettez une requête à l’exemple d’application avec l’en-tête `Accept-Encoding: gzip` et observez que la réponse est compressée. Les en-têtes `Content-Encoding` et `Vary` sont présents sur la réponse.

![Fenêtre Fiddler affichant le résultat d’une demande avec l’en-tête Accept-Encoding et la valeur gzip. Les en-têtes Content-Encoding et de variable sont ajoutés à la réponse. La réponse est compressée.](response-compression/_static/request-compressed.png)

## <a name="providers"></a>Fournisseurs

### <a name="gzipcompressionprovider"></a>GzipCompressionProvider

Utilisez le [GzipCompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovider) pour compresser les réponses avec gzip. Il s’agit du fournisseur de compression par défaut si aucun n’est spécifié. Vous pouvez définir la compression au niveau de la [GzipCompressionProviderOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.gzipcompressionprovideroptions).

Le fournisseur de la compression gzip par défaut est le niveau de compression plus rapide ([CompressionLevel.Fastest](/dotnet/api/system.io.compression.compressionlevel)), ce qui ne peut pas produire la compression la plus efficace. Si la compression la plus efficace est souhaitée, vous pouvez configurer l’intergiciel pour optimiser la compression.

| Niveau de compression | Description |
| ----------------- | ----------- |
| [CompressionLevel.Fastest](/dotnet/api/system.io.compression.compressionlevel) | La compression doit se terminer aussi rapidement que possible, même si le résultat n’est pas compressé de manière optimale. |
| [CompressionLevel.NoCompression](/dotnet/api/system.io.compression.compressionlevel) | Aucune compression ne doit être effectuée. |
| [CompressionLevel.Optimal](/dotnet/api/system.io.compression.compressionlevel) | Les réponses doivent être compressées optimale, même si la compression prend plus de temps pour terminer. |

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5,12-15)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,12-15)]

---

## <a name="mime-types"></a>types MIME

L’intergiciel (middleware) spécifie un ensemble par défaut des types MIME pour la compression :

* `text/plain`
* `text/css`
* `application/javascript`
* `text/html`
* `application/xml`
* `text/xml`
* `application/json`
* `text/json`

Vous pouvez remplacer ou ajouter des types MIME avec les options de l’intergiciel de compression de la réponse. Notez que les caractères génériques MIME types, tels que `text/*` ne sont pas pris en charge. L’exemple d’application ajoute un type MIME pour `image/svg+xml` et compresse et sert les ASP.NET Core image de bannière (*banner.svg*).

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7-9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

---

### <a name="custom-providers"></a>Fournisseurs personnalisés

Vous pouvez créer des implémentations personnalisées de la compression avec [ICompressionProvider](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider). Le [EncodingName](/dotnet/api/microsoft.aspnetcore.responsecompression.icompressionprovider.encodingname) représente le contenu que cet encodage `ICompressionProvider` produit. L’intergiciel utilise ces informations pour choisir le fournisseur en fonction de la liste spécifiée dans l'en-tête `Accept-Encoding` de la requête.

À l’aide de l’exemple d’application, le client envoie une requête avec l'en-tête `Accept-Encoding: mycustomcompression`. L’intergiciel utilise l’implémentation de compression personnalisée et retourne la réponse avec un en-tête `Content-Encoding: mycustomcompression`. Pour que cette implémentation fonctionne, le client doit pouvoir décompresser l’encodage personnalisé.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5,12-15)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,12-15)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

---

Soumettez une requête à l’exemple d’application avec l’en-tête `Accept-Encoding: mycustomcompression` et observez les en-têtes de réponse. Les en-têtes `Vary` et `Content-Encoding` sont présents sur la réponse. Le corps de la réponse (non affiché) n’est pas compressé par l’exemple. Il n’y a pas d’implémentation de compression dans la classe `CustomCompressionProvider` de l’exemple. Toutefois, ce dernier montre où vous pouvez implémenter un tel algorithme de compression.

![Fenêtre Fiddler affichant le résultat d’une demande avec l’en-tête Accept-Encoding et la valeur mycustomcompression. Les en-têtes Content-Encoding et de variable sont ajoutés à la réponse.](response-compression/_static/request-custom-compression.png)

## <a name="compression-with-secure-protocol"></a>Compression avec un protocole sécurisé

Les réponses compressées sur des connexions sécurisées peuvent être contrôlées à l'aide de l'option `EnableForHttps` (désactivée par défaut). L’utilisation de la compression avec des pages générées dynamiquement peut mener à des problèmes de sécurité tels que les attaques [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) et [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)).

## <a name="adding-the-vary-header"></a>Ajout de l’en-tête Vary

::: moniker range=">= aspnetcore-2.0"

Si la compression des réponses basée sur la `Accept-Encoding` en-tête, il existe potentiellement plusieurs versions compressées de la réponse et une version non compressée. Afin d’indiquer les caches de client et le proxy que plusieurs versions existent et doivent être stockées, les `Vary` en-tête est ajouté avec un `Accept-Encoding` valeur. Dans ASP.NET Core 2.0 ou version ultérieure, l’intergiciel (middleware) ajoute le `Vary` en-tête automatiquement lors de la réponse est compressée.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Si la compression des réponses basée sur la `Accept-Encoding` en-tête, il existe potentiellement plusieurs versions compressées de la réponse et une version non compressée. Afin d’indiquer les caches de client et le proxy que plusieurs versions existent et doivent être stockées, les `Vary` en-tête est ajouté avec un `Accept-Encoding` valeur. Dans ASP.NET Core 1.x, ajout de la `Vary` en-tête dans la réponse s’effectue manuellement :

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a>Problème au niveau de l’intergiciel s'il est derrière un proxy inverse Nginx

Quand une requête est transmise par Nginx, l'en-tête `Accept-Encoding` est supprimé. L’intergiciel ne peut donc pas compresser la réponse. Pour plus d’informations, consultez [NGINX : Compression et décompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/). Ce problème est suivi par [Comprendre la compression pass-through pour Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).

## <a name="working-with-iis-dynamic-compression"></a>Utilisation de la compression dynamique IIS

Pour désactiver un module de compression IIS dynamique actif configuré au niveau du serveur pour une application, vous pouvez faire un ajout à votre fichier *web.config*. Pour plus d’informations, consultez [Désactivation de modules IIS](xref:host-and-deploy/iis/modules#disabling-iis-modules).

## <a name="troubleshooting"></a>Résolution des problèmes

Utilisez un outil tel que [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/) ou [Postman](https://www.getpostman.com/) pour définir l'en-tête `Accept-Encoding` de la requête et étudier les en-têtes, la taille et le corps de la réponse. L’intergiciel de compression de réponse compresse les réponses qui remplissent les conditions suivantes :

* L'en-tête `Accept-Encoding` est présent avec la valeur `gzip`, `*`, ou un encodage personnalisé qui correspond à un fournisseur de compression personnalisé que vous avez établi. La valeur ne doit pas être `identity` ou avoir une valeur de qualité (qvalue, `q`) égale à 0 (zéro).
* Le type MIME (`Content-Type`) doit être défini et doit correspondre à un type MIME configuré sur le [ResponseCompressionOptions](/dotnet/api/microsoft.aspnetcore.responsecompression.responsecompressionoptions).
* La requête ne doit pas inclure l'en-tête `Content-Range`.
* La requête doit utiliser un protocole non sécurisé (http), sauf si un protocole sécurisé (https) est configuré dans les options de l’intergiciel de compression de la réponse. *Notez le danger [décrit ci-dessus](#compression-with-secure-protocol) lors de l’activation de la compression de contenu sécurisée.*

## <a name="additional-resources"></a>Ressources supplémentaires

* [Démarrage d’une application](xref:fundamentals/startup)
* [Intergiciel (middleware)](xref:fundamentals/middleware/index)
* [Mozilla MSDN : Accepter-Encoding](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [La Section 7231 relative aux RFC 3.1.2.1 : Codings de contenu](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [La Section 7230 RFC 4.2.3 : Codage Gzip](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [Version de spécification du format fichier GZIP 4.3](http://www.ietf.org/rfc/rfc1952.txt)
