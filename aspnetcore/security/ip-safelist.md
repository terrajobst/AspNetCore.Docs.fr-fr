---
title: Client IP safelier pour ASP.NET Core
author: damienbod
description: Découvrez comment écrire des filtres d’intergiciel ou d’action pour valider des adresses IP distantes par rapport à une liste d’adresses IP approuvées.
ms.author: riande
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: ca5b0f8088773027f7403120247cbeca8900bcf5
ms.sourcegitcommit: 16cf016035f0c9acf3ff0ad874c56f82e013d415
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034342"
---
# <a name="client-ip-safelist-for-aspnet-core"></a>Client IP safelier pour ASP.NET Core

Par [Damien Bowden](https://twitter.com/damien_bod) et [Tom Dykstra](https://github.com/tdykstra)
 
Cet article présente trois méthodes permettant d’implémenter une liste d’adresses IP (également appelée liste verte) dans une application ASP.NET Core. Vous pouvez utiliser les éléments suivants :

* Intergiciel pour vérifier l’adresse IP distante de chaque demande.
* Filtres d’action pour vérifier l’adresse IP distante des demandes pour des contrôleurs ou des méthodes d’action spécifiques.
* Razor Pages filtres pour vérifier l’adresse IP distante des requêtes pour les pages Razor.

Dans chaque cas, une chaîne contenant des adresses IP clientes approuvées est stockée dans un paramètre d’application. L’intergiciel (middleware) ou le filtre analyse la chaîne dans une liste et vérifie si l’adresse IP distante figure dans la liste. Si ce n’est pas le cas, un code d’état HTTP 403 interdit est retourné.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="the-safelist"></a>Safelit

La liste est configurée dans le fichier *appSettings. JSON* . Il s’agit d’une liste délimitée par des points-virgules qui peut contenir des adresses IPv4 et IPv6.

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a>Intergiciel (middleware)

La méthode `Configure` ajoute l’intergiciel (middleware) et lui transmet la chaîne safeli dans un paramètre de constructeur.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=10)]

L’intergiciel analyse la chaîne dans un tableau et recherche l’adresse IP distante dans le tableau. Si l’adresse IP distante est introuvable, l’intergiciel (middleware) renvoie HTTP 401 interdit. Ce processus de validation est contourné pour les requêtes HTTP d’extraction.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a>Filtre d’action

Si vous souhaitez un safelir uniquement pour des contrôleurs ou des méthodes d’action spécifiques, utilisez un filtre d’action. Voici un exemple : 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIpCheckFilter.cs)]

Le filtre d’action est ajouté au conteneur de services.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Le filtre peut ensuite être utilisé sur un contrôleur ou une méthode d’action.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

Dans l’exemple d’application, le filtre est appliqué à la méthode `Get`. Ainsi, lorsque vous testez l’application en envoyant une demande d’API `Get`, l’attribut valide l’adresse IP du client. Lorsque vous testez en appelant l’API avec une autre méthode HTTP, l’intergiciel (middleware) valide l’adresse IP du client.

## <a name="razor-pages-filter"></a>Filtre de Razor Pages 

Si vous souhaitez obtenir une application Razor Pages, utilisez un filtre Razor Pages. Voici un exemple : 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIpCheckPageFilter.cs)]

Ce filtre est activé en l’ajoutant à la collection de filtres MVC.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

Lorsque vous exécutez l’application et demandez une page Razor, le filtre de Razor Pages valide l’adresse IP du client.

## <a name="next-steps"></a>Étapes suivantes

[En savoir plus sur les intergiciels (middleware) ASP.net Core](xref:fundamentals/middleware/index).
