---
title: Tag Helper Cache distribué dans ASP.NET Core
author: pkellner
description: Découvrez comment utiliser le Tag Helper Cache distribué.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: a5b33451a763c297c6d7885855a321c43435abb4
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64890454"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a><span data-ttu-id="ed503-103">Tag Helper Cache distribué dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ed503-103">Distributed Cache Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="ed503-104">Par [Peter Kellner](http://peterkellner.net) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ed503-104">By [Peter Kellner](http://peterkellner.net) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ed503-105">Le Tag Helper Cache distribué permet d’améliorer considérablement les performances de votre application ASP.NET Core en mettant en cache son contenu dans une source de cache distribué.</span><span class="sxs-lookup"><span data-stu-id="ed503-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="ed503-106">Pour avoir une vue d’ensemble des Tag Helpers, consultez <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="ed503-106">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="ed503-107">Le Tag Helper Cache distribué hérite de la même classe de base que le Tag Helper Cache.</span><span class="sxs-lookup"><span data-stu-id="ed503-107">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span> <span data-ttu-id="ed503-108">Tous les attributs [Tag Helper Cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper) sont disponibles pour Tag Helper distribué.</span><span class="sxs-lookup"><span data-stu-id="ed503-108">All of the [Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper) attributes are available to the Distributed Tag Helper.</span></span>

<span data-ttu-id="ed503-109">Le Tag Helper Cache distribué utilise [l’injection de constructeurs](xref:fundamentals/dependency-injection#constructor-injection-behavior).</span><span class="sxs-lookup"><span data-stu-id="ed503-109">The Distributed Cache Tag Helper uses [constructor injection](xref:fundamentals/dependency-injection#constructor-injection-behavior).</span></span> <span data-ttu-id="ed503-110">L’interface <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> est passée dans le constructeur du Tag Helper Cache distribué.</span><span class="sxs-lookup"><span data-stu-id="ed503-110">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface is passed into the Distributed Cache Tag Helper's constructor.</span></span> <span data-ttu-id="ed503-111">Si aucune implémentation concrète de `IDistributedCache` n’est créée dans `Startup.ConfigureServices` (*Startup.cs*), le Tag Helper Cache distribué utilise le même fournisseur en mémoire pour le stockage des données mises en cache que le [Tag Helper Cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="ed503-111">If no concrete implementation of `IDistributedCache` is created in `Startup.ConfigureServices` (*Startup.cs*), the Distributed Cache Tag Helper uses the same in-memory provider for storing cached data as the [Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="ed503-112">Attributs de Tag Helper Cache distribué</span><span class="sxs-lookup"><span data-stu-id="ed503-112">Distributed Cache Tag Helper Attributes</span></span>

### <a name="attributes-shared-with-the-cache-tag-helper"></a><span data-ttu-id="ed503-113">Attributs partagés avec le Tag Helper Cache</span><span class="sxs-lookup"><span data-stu-id="ed503-113">Attributes shared with the Cache Tag Helper</span></span>

* `enabled`
* `expires-on`
* `expires-after`
* `expires-sliding`
* `vary-by-header`
* `vary-by-query`
* `vary-by-route`
* `vary-by-cookie`
* `vary-by-user`
* `vary-by priority`

<span data-ttu-id="ed503-114">Le Tag Helper Cache distribué hérite de la même classe que le Tag Helper Cache.</span><span class="sxs-lookup"><span data-stu-id="ed503-114">The Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper.</span></span> <span data-ttu-id="ed503-115">Pour obtenir une description de ces attributs, consultez le [Tag Helper Cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="ed503-115">For descriptions of these attributes, see the [Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="name"></a><span data-ttu-id="ed503-116">name</span><span class="sxs-lookup"><span data-stu-id="ed503-116">name</span></span>

| <span data-ttu-id="ed503-117">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="ed503-117">Attribute Type</span></span> | <span data-ttu-id="ed503-118">Exemple</span><span class="sxs-lookup"><span data-stu-id="ed503-118">Example</span></span>                               |
| -------------- | ------------------------------------- |
| <span data-ttu-id="ed503-119">Chaîne</span><span class="sxs-lookup"><span data-stu-id="ed503-119">String</span></span>         | `my-distributed-cache-unique-key-101` |

<span data-ttu-id="ed503-120">`name` est obligatoire.</span><span class="sxs-lookup"><span data-stu-id="ed503-120">`name` is required.</span></span> <span data-ttu-id="ed503-121">L’attribut `name` est utilisé en tant que clé pour chaque instance de cache stockée.</span><span class="sxs-lookup"><span data-stu-id="ed503-121">The `name` attribute is used as a key for each stored cache instance.</span></span> <span data-ttu-id="ed503-122">Contrairement au Tag Helper Cache qui affecte une clé de cache à chaque instance selon le nom de la page Razor et l’emplacement dans la page Razor, le Tag Helper Cache distribué base uniquement sa clé sur l’attribut `name`.</span><span class="sxs-lookup"><span data-stu-id="ed503-122">Unlike the Cache Tag Helper that assigns a cache key to each instance based on the Razor page name and location in the Razor page, the Distributed Cache Tag Helper only bases its key on the attribute `name`.</span></span>

<span data-ttu-id="ed503-123">Exemple :</span><span class="sxs-lookup"><span data-stu-id="ed503-123">Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="ed503-124">Implémentations IDistributedCache de Tag Helper Cache distribué</span><span class="sxs-lookup"><span data-stu-id="ed503-124">Distributed Cache Tag Helper IDistributedCache implementations</span></span>

<span data-ttu-id="ed503-125">Il existe deux implémentations d’<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> intégrées à ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ed503-125">There are two implementations of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> built in to ASP.NET Core.</span></span> <span data-ttu-id="ed503-126">L’une est basée sur SQL Server et l’autre sur Redis.</span><span class="sxs-lookup"><span data-stu-id="ed503-126">One is based on SQL Server, and the other is based on Redis.</span></span> <span data-ttu-id="ed503-127">Pour obtenir les détails de ces implémentations, consultez <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="ed503-127">Details of these implementations can be found at <xref:performance/caching/distributed>.</span></span> <span data-ttu-id="ed503-128">Les deux implémentations impliquent la définition d’une instance de `IDistributedCache` dans `Startup`.</span><span class="sxs-lookup"><span data-stu-id="ed503-128">Both implementations involve setting an instance of `IDistributedCache` in `Startup`.</span></span>

<span data-ttu-id="ed503-129">Aucun attribut de balise n’est spécifiquement associé à l’utilisation d’une implémentation d’`IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="ed503-129">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ed503-130">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ed503-130">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
