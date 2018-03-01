---
title: "L’application HTTPS dans une application ASP.NET Core"
author: rick-anderson
description: "Montre comment exiger HTTPS/TLS dans un cœur d’ASP.NET application web."
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 636077ea21581716308384ebf8d47c1e417a256a
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/11/2018
---
# <a name="enforcing-https-in-an-aspnet-core-app"></a>L’application HTTPS dans une application ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Ce document montre comment :

- Exiger HTTPS pour toutes les demandes.
- Rediriger toutes les demandes HTTP vers HTTPS.

> [!WARNING]
> Faire **pas** utiliser `RequireHttpsAttribute` sur les API Web qui reçoivent des informations sensibles. `RequireHttpsAttribute`utilise les codes d’état HTTP pour rediriger les navigateurs de HTTP vers HTTPS. Les clients d’API ne peuvent pas comprendre ou obéissent aux règles de redirections HTTP vers HTTPS. Ces clients peuvent envoyer des informations sur HTTP. API Web doit soit :
>
>* Pas écouter sur HTTP.
>* Fermer la connexion avec le code d’état 400 (demande incorrecte) et pas desservir la demande.

## <a name="require-https"></a>Exiger HTTPS

Le [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) est utilisée pour exiger HTTPS. `[RequireHttpsAttribute]`peut décorer contrôleurs ou méthodes, ou peut être appliqué globalement. Pour appliquer l’attribut global, ajoutez le code suivant à `ConfigureServices` dans `Startup`:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Le code en surbrillance précédent requiert que toutes les demandes utilisent `HTTPS`; par conséquent, les requêtes HTTP sont ignorés. Le code en surbrillance suivant redirige toutes les demandes HTTP vers HTTPS :
Le code en surbrillance suivant redirige toutes les demandes HTTP vers HTTPS :

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Pour plus d’informations, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting).

Nécessitant HTTPS globalement (`options.Filters.Add(new RequireHttpsAttribute());`) est une meilleure pratique de sécurité. Appliquer le `[RequireHttps]` attribut à toutes les Pages de contrôleurs/Razor n’est pas aussi sécurisée comme nécessitant HTTPS globalement. Vous ne pouvez pas garantir la `[RequireHttps]` attribut est appliqué lors de l’ajout de nouveaux contrôleurs et les Pages Razor.