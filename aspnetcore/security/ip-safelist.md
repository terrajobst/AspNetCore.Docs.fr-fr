---
title: Listes d’IP client fiables pour ASP.NET Core
author: damienbod
description: Apprenez à écrire des filtres d’intergiciel (middleware) ou une action pour valider les adresses IP distantes par rapport à une liste des adresses IP approuvées.
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: cfbb50ea33ae3af577f13b00bccc75fe0be57f79
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898146"
---
# <a name="client-ip-safelist-for-aspnet-core"></a>Listes d’IP client fiables pour ASP.NET Core

Par [Damien Bowden](https://twitter.com/damien_bod) et [Tom Dykstra](https://github.com/tdykstra)
 
Cet article montre trois façons d’implémenter un safelist IP (également appelé liste verte) dans une application ASP.NET Core. Vous pouvez utiliser :

* Intergiciel (middleware) pour vérifier l’adresse IP distante de chaque demande.
* Filtres d’action pour vérifier l’adresse IP distante de requêtes pour des contrôleurs spécifiques ou des méthodes d’action.
* Filtres de Pages Razor pour vérifier l’adresse IP distante de requêtes pour les pages Razor.

L’exemple d’application illustre les deux approches. Dans chaque cas, une chaîne contenant les adresses IP de client approuvé est stockée dans un paramètre d’application. L’intergiciel (middleware) ou le filtre analyse la chaîne dans une liste et vérifie si l’adresse IP distante est dans la liste. Si ce n’est pas le cas, un code d’état HTTP 403 Interdit retourné.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="the-safelist"></a>Les listes fiables

La liste est configurée dans le *appsettings.json* fichier. Il est une liste délimitée par des points-virgules et peut contenir des adresses IPv4 et IPv6.

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a>Intergiciel (middleware)

Le `Configure` méthode ajoute l’intergiciel (middleware) et lui passe la chaîne de listes fiables dans un paramètre de constructeur.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=7)]

L’intergiciel (middleware) analyse la chaîne en un tableau et recherche de l’adresse IP distante dans le tableau. Si l’adresse IP distante n’est pas trouvée, le middleware renvoie HTTP 401 interdit. Ce processus de validation est ignoré pour les requêtes HTTP Get.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a>Filtre d’action

Si vous souhaitez un safelist uniquement pour les contrôleurs spécifiques ou des méthodes d’action, utilisez un filtre d’action. Voici un exemple : 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

Le filtre d’action est ajouté au conteneur de services.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

Le filtre peut ensuite être utilisé sur une méthode de contrôleur ou d’action.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

Dans l’exemple d’application, le filtre est appliqué à la `Get` (méthode). Par conséquent, lorsque vous testez l’application en envoyant un `Get` demande d’API, l’attribut est validation de l’adresse IP du client. Lorsque vous testez en appelant l’API avec toute autre méthode HTTP, le middleware valide l’adresse IP du client.

## <a name="razor-pages-filter"></a>Filtrer les Pages Razor 

Si vous souhaitez une listes fiables pour une application Pages Razor, utilisez un filtre de Pages Razor. Voici un exemple : 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

Ce filtre est activé en l’ajoutant à la collection de filtres MVC.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

Lorsque vous exécutez l’application et demandez une page Razor, le filtre de Pages Razor est validation de l’adresse IP du client.

## <a name="next-steps"></a>Étapes suivantes

[En savoir plus sur ASP.NET Core Middleware](xref:fundamentals/middleware/index).
