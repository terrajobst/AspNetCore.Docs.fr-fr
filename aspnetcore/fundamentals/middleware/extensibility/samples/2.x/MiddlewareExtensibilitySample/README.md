---
ms.openlocfilehash: 41021e30ae85dd0ae42cbe6f1606727e21bd7707
ms.sourcegitcommit: 5995f44e9e13d7e7aa8d193e2825381c42184e47
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58809386"
---
# <a name="aspnet-core-middleware-extensibility-sample"></a>Exemple d’extensibilité d’intergiciel (middleware) ASP.NET Core

Cet exemple illustre les scénarios décrits dans [Activation d’un intergiciel (middleware) basé sur une fabrique dans ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/middleware-extensibility).

L’exemple d’application montre un middleware activé par :

* Convention. Pour plus d’informations sur l’activation conventionnelle de middleware, consultez la rubrique [Intergiciel (middleware)](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/).
* Une implémentation de [IMiddleware](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddleware). La classe [IMiddlewareFactory](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) par défaut active le middleware.

Les implémentations de middleware fonctionnent de façon identique et enregistrent la valeur fournie par un paramètre de la chaîne de requête (`key`). Les middlewares utilisent un contexte de base de données injecté (un service délimité) pour enregistrer la valeur de la chaîne de requête dans une base de données en mémoire.
