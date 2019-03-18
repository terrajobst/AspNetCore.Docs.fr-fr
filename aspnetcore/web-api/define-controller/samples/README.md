---
ms.openlocfilehash: 07abb12af390c0f2a50e98fc5e53545b6635f968
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665507"
---
# <a name="aspnet-core-web-api-controller-sample"></a>Exemple de contrôleur d’API web ASP.NET Core

Cet exemple d’application est composé des projets suivants :

- **WebApiSample.Api.22** : projet ASP.NET Core 2.2 ciblant .NET Core 2.2.
- **WebApiSample.Api.21** : projet ASP.NET Core 2.1 ciblant .NET Core 2.1.
- **WebApiSample.Api.Pre21** : projet ASP.NET Core 2.0 ciblant .NET Core 2.0.
- **WebApiSample.DataAccess** : bibliothèque de classes .NET Standard 2.0 servant de niveau d’accès aux données pour les deux projets d’API web.

Cet exemple illustre différentes façons de créer un contrôleur d’API web :

- [Dériver la classe de ControllerBase](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [Annoter la classe avec ApiControllerAttribute](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
