---
title: Tag Helper Cache distribué dans ASP.NET Core
author: pkellner
description: Montre comment utiliser un Tag Helper Cache
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: 929156633048b8ee68a66290f44b12026a08c8c9
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a><span data-ttu-id="ad137-103">Tag Helper Cache distribué dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ad137-103">Distributed Cache Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="ad137-104">Par [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="ad137-104">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="ad137-105">Le Tag Helper Cache distribué permet d’améliorer considérablement les performances de votre application ASP.NET Core en mettant en cache son contenu dans une source de cache distribué.</span><span class="sxs-lookup"><span data-stu-id="ad137-105">The Distributed Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to a distributed cache source.</span></span>

<span data-ttu-id="ad137-106">Le Tag Helper Cache distribué hérite de la même classe de base que le Tag Helper Cache.</span><span class="sxs-lookup"><span data-stu-id="ad137-106">The Distributed Cache Tag Helper inherits from the same base class as the Cache Tag Helper.</span></span>  <span data-ttu-id="ad137-107">Tous les attributs associés au Tag Helper Cache fonctionnent également sur le Tag Helper Cache distribué.</span><span class="sxs-lookup"><span data-stu-id="ad137-107">All attributes associated with the Cache Tag Helper will also work on the Distributed Tag Helper.</span></span>


<span data-ttu-id="ad137-108">Le Tag Helper Cache distribué suit le **principe des dépendances explicites** appelé **injection de constructeur**.</span><span class="sxs-lookup"><span data-stu-id="ad137-108">The Distributed Cache Tag Helper follows the **Explicit Dependencies Principle** known as **Constructor Injection**.</span></span>  <span data-ttu-id="ad137-109">Plus précisément, le conteneur d’interface `IDistributedCache` est passé dans le constructeur du Tag Helper Cache distribué.</span><span class="sxs-lookup"><span data-stu-id="ad137-109">Specifically, the `IDistributedCache` interface container is passed into the Distributed Cache Tag Helper's constructor.</span></span>  <span data-ttu-id="ad137-110">Si aucune implémentation concrète d’`IDistributedCache` n’a été créée dans `ConfigureServices`, qui se trouve généralement dans startup.cs, le Tag Helper Cache distribué utilise le même fournisseur en mémoire pour le stockage des données mises en cache que le Tag Helper Cache de base.</span><span class="sxs-lookup"><span data-stu-id="ad137-110">If no specific concrete implementation of `IDistributedCache` has been created in `ConfigureServices`, usually found in startup.cs, then the Distributed Cache Tag Helper will use the same in-memory provider for storing cached data as the basic Cache Tag Helper.</span></span>

## <a name="distributed-cache-tag-helper-attributes"></a><span data-ttu-id="ad137-111">Attributs de Tag Helper Cache distribué</span><span class="sxs-lookup"><span data-stu-id="ad137-111">Distributed Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a><span data-ttu-id="ad137-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span><span class="sxs-lookup"><span data-stu-id="ad137-112">enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority</span></span>

<span data-ttu-id="ad137-113">Consultez le Tag Helper Cache pour obtenir les définitions.</span><span class="sxs-lookup"><span data-stu-id="ad137-113">See Cache Tag Helper for definitions.</span></span> <span data-ttu-id="ad137-114">Comme le Tag Helper Cache distribué hérite de la même classe que le Tag Helper Cache, tous ces attributs sont communs à partir du Tag Helper Cache.</span><span class="sxs-lookup"><span data-stu-id="ad137-114">Distributed Cache Tag Helper inherits from the same class as Cache Tag Helper so all these attributes are common from Cache Tag Helper.</span></span>

- - -

### <a name="name-required"></a><span data-ttu-id="ad137-115">name (obligatoire)</span><span class="sxs-lookup"><span data-stu-id="ad137-115">name (required)</span></span>

| <span data-ttu-id="ad137-116">Type d’attribut</span><span class="sxs-lookup"><span data-stu-id="ad137-116">Attribute Type</span></span>    | <span data-ttu-id="ad137-117">Exemple de valeur</span><span class="sxs-lookup"><span data-stu-id="ad137-117">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="ad137-118">chaîne</span><span class="sxs-lookup"><span data-stu-id="ad137-118">string</span></span>    | <span data-ttu-id="ad137-119">"my-distributed-cache-unique-key-101"</span><span class="sxs-lookup"><span data-stu-id="ad137-119">"my-distributed-cache-unique-key-101"</span></span>     |

<span data-ttu-id="ad137-120">L’attribut `name` obligatoire est utilisé comme clé de ce cache stockée pour chaque instance d’un Tag Helper Cache distribué.</span><span class="sxs-lookup"><span data-stu-id="ad137-120">The required `name` attribute is used as a key to that cache stored for each instance of a Distributed Cache Tag Helper.</span></span>  <span data-ttu-id="ad137-121">Contrairement au Tag Helper Cache de base qui affecte une clé à chaque instance de Tag Helper Cache selon le nom de la page Razor et l’emplacement du Tag Helper dans la page Razor, le Tag Helper Cache distribué base uniquement sa clé sur l’attribut `name`</span><span class="sxs-lookup"><span data-stu-id="ad137-121">Unlike the basic Cache Tag Helper that assigns a key to each Cache Tag Helper instance based on the Razor page name and location of the tag helper in the razor page, the Distributed Cache Tag Helper only bases it's key on the attribute `name`</span></span>

<span data-ttu-id="ad137-122">Exemple d’utilisation :</span><span class="sxs-lookup"><span data-stu-id="ad137-122">Usage Example:</span></span>

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a><span data-ttu-id="ad137-123">Implémentations IDistributedCache de Tag Helper Cache distribué</span><span class="sxs-lookup"><span data-stu-id="ad137-123">Distributed Cache Tag Helper IDistributedCache Implementations</span></span>

<span data-ttu-id="ad137-124">Il existe deux implémentations d’`IDistributedCache` intégrées à ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ad137-124">There are two implementations of `IDistributedCache` built in to ASP.NET Core.</span></span>  <span data-ttu-id="ad137-125">L’une est basée sur **Sql Server** et l’autre sur **Redis**.</span><span class="sxs-lookup"><span data-stu-id="ad137-125">One is based on **Sql Server** and the other is based on **Redis**.</span></span> <span data-ttu-id="ad137-126">Vous trouverez les détails de ces implémentations au niveau de la ressource référencée ci-dessous intitulée « Utilisation d’un cache distribué ».</span><span class="sxs-lookup"><span data-stu-id="ad137-126">Details of these implementations can be found at the resource referenced below named "Working with a distributed cache".</span></span> <span data-ttu-id="ad137-127">Les deux implémentations impliquent la définition d’une instance d’`IDistributedCache` dans le fichier **startup.cs** d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ad137-127">Both implementations involve setting an instance of `IDistributedCache` in ASP.NET Core's **startup.cs**.</span></span>

<span data-ttu-id="ad137-128">Aucun attribut de balise n’est spécifiquement associé à l’utilisation d’une implémentation d’`IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="ad137-128">There are no tag attributes specifically associated with using any specific implementation of `IDistributedCache`.</span></span>



- - -



## <a name="additional-resources"></a><span data-ttu-id="ad137-129">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="ad137-129">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
