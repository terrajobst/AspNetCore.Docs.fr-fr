---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Conseils de sécurité pour ASP.NET Web API 2 OData | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/06/2013
ms.topic: article
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 72aee37e715fa11ccfe5d02eef4e450dd747095a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378029"
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a><span data-ttu-id="9de8c-102">Conseils de sécurité pour ASP.NET Web API 2 OData</span><span class="sxs-lookup"><span data-stu-id="9de8c-102">Security Guidance for ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="9de8c-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9de8c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="9de8c-104">Cette rubrique décrit certains des problèmes de sécurité dont vous devez tenir compte lorsque vous exposez via OData, un jeu de données.</span><span class="sxs-lookup"><span data-stu-id="9de8c-104">This topic describes some of the security issues that you should consider when exposing a dataset through OData.</span></span>

## <a name="edm-security"></a><span data-ttu-id="9de8c-105">Sécurité du modèle EDM</span><span class="sxs-lookup"><span data-stu-id="9de8c-105">EDM Security</span></span>

<span data-ttu-id="9de8c-106">La sémantique de requête est basée sur l’entity data model (EDM), pas les types de modèle sous-jacent.</span><span class="sxs-lookup"><span data-stu-id="9de8c-106">The query semantics are based on the entity data model (EDM), not the underlying model types.</span></span> <span data-ttu-id="9de8c-107">Vous pouvez exclure une propriété de l’EDM, et il ne sera pas visible à la requête.</span><span class="sxs-lookup"><span data-stu-id="9de8c-107">You can exclude a property from the EDM and it will not be visible to the query.</span></span> <span data-ttu-id="9de8c-108">Par exemple, supposons que votre modèle inclut un type d’employé avec une propriété de salaire.</span><span class="sxs-lookup"><span data-stu-id="9de8c-108">For example, suppose your model includes an Employee type with a Salary property.</span></span> <span data-ttu-id="9de8c-109">Vous souhaiterez peut-être exclure cette propriété de l’EDM pour la masquer à partir de clients.</span><span class="sxs-lookup"><span data-stu-id="9de8c-109">You might want to exclude this property from the EDM to hide it from clients.</span></span>

<span data-ttu-id="9de8c-110">Il existe deux façons d’exclure une propriété à partir du modèle EDM.</span><span class="sxs-lookup"><span data-stu-id="9de8c-110">There are two ways to exlude a property from the EDM.</span></span> <span data-ttu-id="9de8c-111">Vous pouvez définir le **[IgnoreDataMember]** attribut sur la propriété dans la classe de modèle :</span><span class="sxs-lookup"><span data-stu-id="9de8c-111">You can set the **[IgnoreDataMember]** attribute on the property in the model class:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

<span data-ttu-id="9de8c-112">Vous pouvez également supprimer par programmation la propriété à partir du modèle EDM :</span><span class="sxs-lookup"><span data-stu-id="9de8c-112">You can also remove the property from the EDM programmatically:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a><span data-ttu-id="9de8c-113">Sécurité de la requête</span><span class="sxs-lookup"><span data-stu-id="9de8c-113">Query Security</span></span>

<span data-ttu-id="9de8c-114">Un client malveillant ou naïve peut être en mesure de construire une requête qui prend beaucoup de temps à exécuter.</span><span class="sxs-lookup"><span data-stu-id="9de8c-114">A malicious or naive client may be able to construct a query that takes a very long time to execute.</span></span> <span data-ttu-id="9de8c-115">Dans le pire des cas cela peut perturber l’accès à votre service.</span><span class="sxs-lookup"><span data-stu-id="9de8c-115">In the worst case this can disrupt access to your service.</span></span>

<span data-ttu-id="9de8c-116">Le **[Queryable]** attribut est un filtre d’action qui analyse, valide et applique la requête.</span><span class="sxs-lookup"><span data-stu-id="9de8c-116">The **[Queryable]** attribute is an action filter that parses, validates, and applies the query.</span></span> <span data-ttu-id="9de8c-117">Le filtre convertit les options de requête dans une expression LINQ.</span><span class="sxs-lookup"><span data-stu-id="9de8c-117">The filter converts the query options into a LINQ expression.</span></span> <span data-ttu-id="9de8c-118">Lorsque le contrôleur OData retourne un **IQueryable** type, le **IQueryable** fournisseur LINQ convertit l’expression LINQ dans une requête.</span><span class="sxs-lookup"><span data-stu-id="9de8c-118">When the OData controller returns an **IQueryable** type, the **IQueryable** LINQ provider converts the LINQ expression into a query.</span></span> <span data-ttu-id="9de8c-119">Par conséquent, les performances dépendent du fournisseur LINQ qui est utilisé, ainsi que sur les caractéristiques particulières de votre schéma de base de données ou le jeu de données.</span><span class="sxs-lookup"><span data-stu-id="9de8c-119">Therefore, performance depends on the LINQ provider that is used, and also on the particular characteristics of your dataset or database schema.</span></span>

<span data-ttu-id="9de8c-120">Pour plus d’informations sur l’utilisation des options de requête OData dans l’API Web ASP.NET, consultez [prenant en charge des Options de requête OData](supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="9de8c-120">For more information about using OData query options in ASP.NET Web API, see [Supporting OData Query Options](supporting-odata-query-options.md).</span></span>

<span data-ttu-id="9de8c-121">Si vous savez que tous les clients sont de confiance (par exemple, un environnement d’entreprise), ou si votre jeu de données est petite, les performances des requêtes peut-être pas un problème.</span><span class="sxs-lookup"><span data-stu-id="9de8c-121">If you know that all clients are trusted (for example, in an enterprise environment), or if your dataset is small, query performance might not be an issue.</span></span> <span data-ttu-id="9de8c-122">Sinon, vous devez envisager les recommandations suivantes.</span><span class="sxs-lookup"><span data-stu-id="9de8c-122">Otherwise, you should consider the following recommendations.</span></span>

- <span data-ttu-id="9de8c-123">Tester votre service avec différentes requêtes et la base de données de profil.</span><span class="sxs-lookup"><span data-stu-id="9de8c-123">Test your service with various queries and profile the DB.</span></span>
- <span data-ttu-id="9de8c-124">Activer la pagination orientée serveur éviter le renvoi d’un jeu de données volumineux dans une seule requête.</span><span class="sxs-lookup"><span data-stu-id="9de8c-124">Enable server-driven paging, to avoid returning a large data set in one query.</span></span> <span data-ttu-id="9de8c-125">Pour plus d’informations, consultez [Server-Driven pagination](supporting-odata-query-options.md#server-paging).</span><span class="sxs-lookup"><span data-stu-id="9de8c-125">For more information, see [Server-Driven Paging](supporting-odata-query-options.md#server-paging).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- <span data-ttu-id="9de8c-126">Vous devez $filter et $orderby ?</span><span class="sxs-lookup"><span data-stu-id="9de8c-126">Do you need $filter and $orderby?</span></span> <span data-ttu-id="9de8c-127">Certaines applications peuvent autoriser la pagination, à l’aide de $top et $skip de client, mais désactive les autres options de requête.</span><span class="sxs-lookup"><span data-stu-id="9de8c-127">Some applications might allow client paging, using $top and $skip, but disable the other query options.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- <span data-ttu-id="9de8c-128">Pensez à restreindre $orderby aux propriétés dans un index cluster.</span><span class="sxs-lookup"><span data-stu-id="9de8c-128">Consider restricting $orderby to properties in a clustered index.</span></span> <span data-ttu-id="9de8c-129">Le tri des données volumineuses sans index cluster est lente.</span><span class="sxs-lookup"><span data-stu-id="9de8c-129">Sorting large data without a clustered index is slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- <span data-ttu-id="9de8c-130">Nombre maximal de nœuds : le **MaxNodeCount** propriété sur **[Queryable]** définit le nombre maximal de nœuds nombre autorisé dans l’arborescence de syntaxe $filter.</span><span class="sxs-lookup"><span data-stu-id="9de8c-130">Maximum node count: The **MaxNodeCount** property on **[Queryable]** sets the maximum number nodes allowed in the $filter syntax tree.</span></span> <span data-ttu-id="9de8c-131">La valeur par défaut est 100, mais peut vouloir définir une valeur inférieure, car un grand nombre de nœuds peut être lent à compiler.</span><span class="sxs-lookup"><span data-stu-id="9de8c-131">The default value is 100, but you may want to set a lower value, because a large number of nodes can be slow to compile.</span></span> <span data-ttu-id="9de8c-132">Cela est particulièrement vrai si vous utilisez LINQ to Objects (par exemple, les requêtes LINQ sur une collection en mémoire, sans l’utilisation d’un fournisseur LINQ intermédiaire).</span><span class="sxs-lookup"><span data-stu-id="9de8c-132">This is particularly true if you are using LINQ to Objects (i.e., LINQ queries on a collection in memory, without the use of an intermediate LINQ provider).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- <span data-ttu-id="9de8c-133">Envisagez de désactiver les fonctions any() et all(), car ceux-ci peuvent être lentes.</span><span class="sxs-lookup"><span data-stu-id="9de8c-133">Consider disabling the any() and all() functions, as these can be slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- <span data-ttu-id="9de8c-134">Si les propriétés de chaîne contient des chaînes de grande taille #8212for exemple, une description de produit ou une entrée de blog & 8212consider # désactiver les fonctions de chaîne.</span><span class="sxs-lookup"><span data-stu-id="9de8c-134">If any string properties contain large strings&#8212for example, a product description or a blog entry&#8212consider disabling the string functions.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- <span data-ttu-id="9de8c-135">Envisagez d’interdiction de filtrage sur les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="9de8c-135">Consider disallowing filtering on navigation properties.</span></span> <span data-ttu-id="9de8c-136">Filtrage sur les propriétés de navigation peut entraîner une jointure, qui peut être lente, en fonction de votre schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="9de8c-136">Filtering on navigation properties can result in a join, which might be slow, depending on your database schema.</span></span> <span data-ttu-id="9de8c-137">Le code suivant montre un validateur de requête qui empêche le filtrage sur les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="9de8c-137">The following code shows a query validator that prevents filtering on navigation properties.</span></span> <span data-ttu-id="9de8c-138">Pour plus d’informations sur les validateurs de requête, consultez [Validation de requête](supporting-odata-query-options.md#query-validation).</span><span class="sxs-lookup"><span data-stu-id="9de8c-138">For more information about query validators, see [Query Validation](supporting-odata-query-options.md#query-validation).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- <span data-ttu-id="9de8c-139">Envisagez de limiter les requêtes $filter en écrivant un validateur personnalisé pour votre base de données.</span><span class="sxs-lookup"><span data-stu-id="9de8c-139">Consider restricting $filter queries by writing a validator that is customized for your database.</span></span> <span data-ttu-id="9de8c-140">Par exemple, considérez ces deux requêtes :</span><span class="sxs-lookup"><span data-stu-id="9de8c-140">For example, consider these two queries:</span></span> 

  - <span data-ttu-id="9de8c-141">Tous les films avec les acteurs dont le nom commence par « A ».</span><span class="sxs-lookup"><span data-stu-id="9de8c-141">All movies with actors whose last name starts with ‘A'.</span></span>
  - <span data-ttu-id="9de8c-142">Tous les films publiées en 1994.</span><span class="sxs-lookup"><span data-stu-id="9de8c-142">All movies released in 1994.</span></span>

    <span data-ttu-id="9de8c-143">À moins que les films sont indexés par des acteurs, la première requête peut nécessiter le moteur de base de données à analyser l’intégralité de la liste de films.</span><span class="sxs-lookup"><span data-stu-id="9de8c-143">Unless movies are indexed by actors, the first query might require the DB engine to scan the entire list of movies.</span></span> <span data-ttu-id="9de8c-144">Tandis que la deuxième requête peut être acceptable, en supposant que films sont indexés par année de sortie.</span><span class="sxs-lookup"><span data-stu-id="9de8c-144">Whereas the second query might be acceptable, assuming movies are indexed by release year.</span></span>

    <span data-ttu-id="9de8c-145">Le code suivant montre un validateur qui permet de filtrer sur les propriétés « ReleaseYear » et « Title », mais aucune autre propriété.</span><span class="sxs-lookup"><span data-stu-id="9de8c-145">The following code shows a validator that allows filtering on the "ReleaseYear" and "Title" properties but no other properties.</span></span>

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- <span data-ttu-id="9de8c-146">En règle générale, envisagez les fonctions de $filter que vous avez besoin.</span><span class="sxs-lookup"><span data-stu-id="9de8c-146">In general, consider which $filter functions you need.</span></span> <span data-ttu-id="9de8c-147">Si vos clients n’avez pas besoin de l’expressivité complète de $filter, vous pouvez limiter les fonctions admises.</span><span class="sxs-lookup"><span data-stu-id="9de8c-147">If your clients do not need the full expressiveness of $filter, you can limit the allowed functions.</span></span>
