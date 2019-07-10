---
title: Réutilisation d’un objet avec le pool de threads dans ASP.NET Core
author: rick-anderson
description: Conseils pour augmenter les performances dans les applications ASP.NET Core à l’aide du pool de threads.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 4/11/2019
uid: performance/ObjectPool
ms.openlocfilehash: 92106d5add7dbda36e451614429baa0db420f0e8
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724833"
---
# <a name="object-reuse-with-objectpool-in-aspnet-core"></a><span data-ttu-id="e4e75-103">Réutilisation d’un objet avec le pool de threads dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e4e75-103">Object reuse with ObjectPool in ASP.NET Core</span></span>

<span data-ttu-id="e4e75-104">Par [Steve Gordon](https://twitter.com/stevejgordon), [Ryan Nowak](https://github.com/rynowak), et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e4e75-104">By [Steve Gordon](https://twitter.com/stevejgordon), [Ryan Nowak](https://github.com/rynowak), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e4e75-105"><xref:Microsoft.Extensions.ObjectPool> fait partie de l’infrastructure ASP.NET Core qui prend en charge en conservant un groupe d’objets en mémoire pour une réutilisation au lieu d’objets garbage collectées.</span><span class="sxs-lookup"><span data-stu-id="e4e75-105"><xref:Microsoft.Extensions.ObjectPool> is part of the ASP.NET Core infrastructure that supports keeping a group of objects in memory for reuse rather than allowing the objects to be garbage collected.</span></span>

<span data-ttu-id="e4e75-106">Vous souhaiterez peut-être utiliser le pool d’objets si les objets qui sont gérés sont :</span><span class="sxs-lookup"><span data-stu-id="e4e75-106">You might want to use the object pool if the objects that are being managed are:</span></span>

- <span data-ttu-id="e4e75-107">Cher / d’initialiser allouer.</span><span class="sxs-lookup"><span data-stu-id="e4e75-107">Expensive to allocate/initialize.</span></span>
- <span data-ttu-id="e4e75-108">Représentent une ressource limitée.</span><span class="sxs-lookup"><span data-stu-id="e4e75-108">Represent some limited resource.</span></span>
- <span data-ttu-id="e4e75-109">Utilisé de manière prévisible et fréquemment.</span><span class="sxs-lookup"><span data-stu-id="e4e75-109">Used predictably and frequently.</span></span>

<span data-ttu-id="e4e75-110">Par exemple, le framework ASP.NET Core utilise le pool d’objets à certains endroits à réutiliser <xref:System.Text.StringBuilder> instances.</span><span class="sxs-lookup"><span data-stu-id="e4e75-110">For example, the ASP.NET Core framework uses the object pool in some places to reuse <xref:System.Text.StringBuilder> instances.</span></span> <span data-ttu-id="e4e75-111">`StringBuilder` alloue et gère son propre mémoires tampons pour conserver les données de caractère.</span><span class="sxs-lookup"><span data-stu-id="e4e75-111">`StringBuilder` allocates and manages its own buffers to hold character data.</span></span> <span data-ttu-id="e4e75-112">ASP.NET Core utilise régulièrement `StringBuilder` pour implémenter les fonctionnalités, et réutilisez permet un gain de performances.</span><span class="sxs-lookup"><span data-stu-id="e4e75-112">ASP.NET Core regularly uses `StringBuilder` to implement features, and reusing them provides a performance benefit.</span></span>

<span data-ttu-id="e4e75-113">Le pool d’objets ne toujours améliore les performances :</span><span class="sxs-lookup"><span data-stu-id="e4e75-113">Object pooling doesn't always improve performance:</span></span>

- <span data-ttu-id="e4e75-114">À moins que le coût de l’initialisation d’un objet est élevé, il est généralement plus lent obtenir l’objet à partir du pool.</span><span class="sxs-lookup"><span data-stu-id="e4e75-114">Unless the initialization cost of an object is high, it's usually slower to get the object from the pool.</span></span>
- <span data-ttu-id="e4e75-115">Objets gérés par le pool ne sont pas désalloués jusqu'à ce que le pool est désallouée.</span><span class="sxs-lookup"><span data-stu-id="e4e75-115">Objects managed by the pool aren't de-allocated until the pool is de-allocated.</span></span>

<span data-ttu-id="e4e75-116">Utiliser le pool d’objets uniquement après la collecte de données de performances à l’aide de scénarios réalistes pour votre application ou une bibliothèque.</span><span class="sxs-lookup"><span data-stu-id="e4e75-116">Use object pooling only after collecting performance data using realistic scenarios for your app or library.</span></span>

<span data-ttu-id="e4e75-117">**AVERTISSEMENT : Le `ObjectPool` n’implémente pas `IDisposable`. Nous ne recommandons pas l’utiliser avec les types qui ont besoin de disposition.**</span><span class="sxs-lookup"><span data-stu-id="e4e75-117">**WARNING: The `ObjectPool` doesn't implement `IDisposable`. We don't recommend using it with types that need disposal.**</span></span>

<span data-ttu-id="e4e75-118">**REMARQUE : Le pool de threads ne place pas une limite sur le nombre d’objets il alloue, elle place une limite sur le nombre d’objets, qu'il conserve.**</span><span class="sxs-lookup"><span data-stu-id="e4e75-118">**NOTE: The ObjectPool doesn't place a limit on the number of objects that it will allocate, it places a limit on the number of objects it will retain.**</span></span>

## <a name="concepts"></a><span data-ttu-id="e4e75-119">Concepts</span><span class="sxs-lookup"><span data-stu-id="e4e75-119">Concepts</span></span>

<span data-ttu-id="e4e75-120"><xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> -l’abstraction de pool d’objet de base.</span><span class="sxs-lookup"><span data-stu-id="e4e75-120"><xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> - the basic object pool abstraction.</span></span> <span data-ttu-id="e4e75-121">Utilisé pour obtenir et retourner des objets.</span><span class="sxs-lookup"><span data-stu-id="e4e75-121">Used to get and return objects.</span></span>

<span data-ttu-id="e4e75-122"><xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> -implémenter cela pour personnaliser la façon dont un objet est créé et comment il est *réinitialiser* lorsque retournée au pool.</span><span class="sxs-lookup"><span data-stu-id="e4e75-122"><xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> - implement this to customize how an object is created and how it is *reset* when returned to the pool.</span></span> <span data-ttu-id="e4e75-123">Cela peut être passé dans un pool d’objets que vous construisez directement... OU</span><span class="sxs-lookup"><span data-stu-id="e4e75-123">This can be passed into an object pool that you construct directly.... OR</span></span>

<span data-ttu-id="e4e75-124"><xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> sert de fabrique pour la création de pools d’objet.</span><span class="sxs-lookup"><span data-stu-id="e4e75-124"><xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> acts as a factory for creating object pools.</span></span>
<!-- REview, there is no ObjectPoolProvider<T> -->

<span data-ttu-id="e4e75-125">Le pool de threads peut être utilisé dans une application de plusieurs façons :</span><span class="sxs-lookup"><span data-stu-id="e4e75-125">The ObjectPool can be used in an app in multiple ways:</span></span>

* <span data-ttu-id="e4e75-126">Instanciation d’un pool.</span><span class="sxs-lookup"><span data-stu-id="e4e75-126">Instantiating a pool.</span></span>
* <span data-ttu-id="e4e75-127">L’inscription d’un pool dans [l’injection de dépendances](xref:fundamentals/dependency-injection) (DI) en tant qu’instance.</span><span class="sxs-lookup"><span data-stu-id="e4e75-127">Registering a pool in [Dependency injection](xref:fundamentals/dependency-injection) (DI) as an instance.</span></span>
* <span data-ttu-id="e4e75-128">L’inscription du `ObjectPoolProvider<>` dans l’injection de dépendances et l’utiliser comme une fabrique.</span><span class="sxs-lookup"><span data-stu-id="e4e75-128">Registering the `ObjectPoolProvider<>` in DI and using it as a factory.</span></span>

## <a name="how-to-use-objectpool"></a><span data-ttu-id="e4e75-129">Comment utiliser le pool de threads</span><span class="sxs-lookup"><span data-stu-id="e4e75-129">How to use ObjectPool</span></span>

<span data-ttu-id="e4e75-130">Appelez <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> pour obtenir un objet et <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> pour retourner l’objet.</span><span class="sxs-lookup"><span data-stu-id="e4e75-130">Call <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> to get an object and <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> to return the object.</span></span>  <span data-ttu-id="e4e75-131">N’est pas obligatoire que renvoie chaque objet.</span><span class="sxs-lookup"><span data-stu-id="e4e75-131">There's no requirement that you return every object.</span></span> <span data-ttu-id="e4e75-132">Si vous ne retournez un objet, il sera par le garbage collecté.</span><span class="sxs-lookup"><span data-stu-id="e4e75-132">If you don't return an object, it will be garbage collected.</span></span>

## <a name="objectpool-sample"></a><span data-ttu-id="e4e75-133">Exemple de pool de threads</span><span class="sxs-lookup"><span data-stu-id="e4e75-133">ObjectPool sample</span></span>

<span data-ttu-id="e4e75-134">L'exemple de code suivant :</span><span class="sxs-lookup"><span data-stu-id="e4e75-134">The following code:</span></span>

* <span data-ttu-id="e4e75-135">Ajoute `ObjectPoolProvider` à la [l’injection de dépendances](xref:fundamentals/dependency-injection) (DI) conteneur.</span><span class="sxs-lookup"><span data-stu-id="e4e75-135">Adds `ObjectPoolProvider` to the [Dependency injection](xref:fundamentals/dependency-injection) (DI) container.</span></span>
* <span data-ttu-id="e4e75-136">Ajoute et configure `ObjectPool<StringBuilder>` vers le conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="e4e75-136">Adds and configures `ObjectPool<StringBuilder>` to the DI container.</span></span>
* <span data-ttu-id="e4e75-137">Ajoute le `BirthdayMiddleware`.</span><span class="sxs-lookup"><span data-stu-id="e4e75-137">Adds the `BirthdayMiddleware`.</span></span>

[!code-csharp[](ObjectPool/ObjectPoolSample/Startup.cs?name=snippet)]

<span data-ttu-id="e4e75-138">Le code suivant implémente `BirthdayMiddleware`</span><span class="sxs-lookup"><span data-stu-id="e4e75-138">The following code implements `BirthdayMiddleware`</span></span>

[!code-csharp[](ObjectPool/ObjectPoolSample/BirthdayMiddleware.cs?name=snippet)]
