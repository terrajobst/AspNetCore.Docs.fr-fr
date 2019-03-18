---
ms.openlocfilehash: 07abb12af390c0f2a50e98fc5e53545b6635f968
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665507"
---
# <a name="aspnet-core-web-api-controller-sample"></a><span data-ttu-id="87be1-101">Exemple de contrôleur d’API web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="87be1-101">ASP.NET Core Web API Controller Sample</span></span>

<span data-ttu-id="87be1-102">Cet exemple d’application est composé des projets suivants :</span><span class="sxs-lookup"><span data-stu-id="87be1-102">This sample app consists of the following projects:</span></span>

- <span data-ttu-id="87be1-103">**WebApiSample.Api.22** : projet ASP.NET Core 2.2 ciblant .NET Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="87be1-103">**WebApiSample.Api.22**: An ASP.NET Core 2.2 project targeting .NET Core 2.2.</span></span>
- <span data-ttu-id="87be1-104">**WebApiSample.Api.21** : projet ASP.NET Core 2.1 ciblant .NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="87be1-104">**WebApiSample.Api.21**: An ASP.NET Core 2.1 project targeting .NET Core 2.1.</span></span>
- <span data-ttu-id="87be1-105">**WebApiSample.Api.Pre21** : projet ASP.NET Core 2.0 ciblant .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="87be1-105">**WebApiSample.Api.Pre21**: An ASP.NET Core 2.0 project targeting .NET Core 2.0.</span></span>
- <span data-ttu-id="87be1-106">**WebApiSample.DataAccess** : bibliothèque de classes .NET Standard 2.0 servant de niveau d’accès aux données pour les deux projets d’API web.</span><span class="sxs-lookup"><span data-stu-id="87be1-106">**WebApiSample.DataAccess**: A .NET Standard 2.0 class library serving as a data access tier for the 2 Web API projects.</span></span>

<span data-ttu-id="87be1-107">Cet exemple illustre différentes façons de créer un contrôleur d’API web :</span><span class="sxs-lookup"><span data-stu-id="87be1-107">This sample illustrates variations of Web API controller creation:</span></span>

- [<span data-ttu-id="87be1-108">Dériver la classe de ControllerBase</span><span class="sxs-lookup"><span data-stu-id="87be1-108">Derive class from ControllerBase</span></span>](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [<span data-ttu-id="87be1-109">Annoter la classe avec ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="87be1-109">Annotate class with ApiControllerAttribute</span></span>](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
