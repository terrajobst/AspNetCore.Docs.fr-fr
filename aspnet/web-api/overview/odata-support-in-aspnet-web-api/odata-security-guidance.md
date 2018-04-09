---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Guide de sécurité pour ASP.NET Web API 2 OData | Documents Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/06/2013
ms.topic: article
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 41b05f2a2f8247853d8358e6cc1246c8b438a6db
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a><span data-ttu-id="9ab32-102">Guide de sécurité pour ASP.NET Web API 2 OData</span><span class="sxs-lookup"><span data-stu-id="9ab32-102">Security Guidance for ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="9ab32-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9ab32-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="9ab32-104">Cette rubrique décrit certains des problèmes de sécurité vous devez prendre en compte lors de l’exposition d’un jeu de données via OData.</span><span class="sxs-lookup"><span data-stu-id="9ab32-104">This topic describes some of the security issues that you should consider when exposing a dataset through OData.</span></span>

## <a name="edm-security"></a><span data-ttu-id="9ab32-105">Sécurité du modèle EDM</span><span class="sxs-lookup"><span data-stu-id="9ab32-105">EDM Security</span></span>

<span data-ttu-id="9ab32-106">La sémantique de la requête est basée sur l’entity data model (EDM), pas les types de modèle sous-jacent.</span><span class="sxs-lookup"><span data-stu-id="9ab32-106">The query semantics are based on the entity data model (EDM), not the underlying model types.</span></span> <span data-ttu-id="9ab32-107">Vous pouvez exclure une propriété de l’EDM et il ne sera pas visible à la requête.</span><span class="sxs-lookup"><span data-stu-id="9ab32-107">You can exclude a property from the EDM and it will not be visible to the query.</span></span> <span data-ttu-id="9ab32-108">Par exemple, supposons que votre modèle inclut un type d’employé avec une propriété de salaire.</span><span class="sxs-lookup"><span data-stu-id="9ab32-108">For example, suppose your model includes an Employee type with a Salary property.</span></span> <span data-ttu-id="9ab32-109">Vous pouvez souhaiter exclure cette propriété de l’EDM pour la masquer à partir de clients.</span><span class="sxs-lookup"><span data-stu-id="9ab32-109">You might want to exclude this property from the EDM to hide it from clients.</span></span>

<span data-ttu-id="9ab32-110">Il existe deux façons d’exclure une propriété à partir du modèle EDM.</span><span class="sxs-lookup"><span data-stu-id="9ab32-110">There are two ways to exlude a property from the EDM.</span></span> <span data-ttu-id="9ab32-111">Vous pouvez définir le **[IgnoreDataMember]** attribut sur la propriété dans la classe de modèle :</span><span class="sxs-lookup"><span data-stu-id="9ab32-111">You can set the **[IgnoreDataMember]** attribute on the property in the model class:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

<span data-ttu-id="9ab32-112">Vous pouvez également supprimer par programme la propriété dans le modèle EDM :</span><span class="sxs-lookup"><span data-stu-id="9ab32-112">You can also remove the property from the EDM programmatically:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a><span data-ttu-id="9ab32-113">Sécurité de la requête</span><span class="sxs-lookup"><span data-stu-id="9ab32-113">Query Security</span></span>

<span data-ttu-id="9ab32-114">Un client malveillant ou naïve peut être en mesure de créer une requête qui prend beaucoup de temps à exécuter.</span><span class="sxs-lookup"><span data-stu-id="9ab32-114">A malicious or naive client may be able to construct a query that takes a very long time to execute.</span></span> <span data-ttu-id="9ab32-115">Dans le pire des cas cela peut perturber l’accès à votre service.</span><span class="sxs-lookup"><span data-stu-id="9ab32-115">In the worst case this can disrupt access to your service.</span></span>

<span data-ttu-id="9ab32-116">Le **[Queryable]** attribut est un filtre d’action qui analyse, valide et applique la requête.</span><span class="sxs-lookup"><span data-stu-id="9ab32-116">The **[Queryable]** attribute is an action filter that parses, validates, and applies the query.</span></span> <span data-ttu-id="9ab32-117">Le filtre convertit les options de requête dans une expression LINQ.</span><span class="sxs-lookup"><span data-stu-id="9ab32-117">The filter converts the query options into a LINQ expression.</span></span> <span data-ttu-id="9ab32-118">Lorsque le contrôleur OData retourne un **IQueryable** type, le **IQueryable** fournisseur LINQ convertit l’expression LINQ dans une requête.</span><span class="sxs-lookup"><span data-stu-id="9ab32-118">When the OData controller returns an **IQueryable** type, the **IQueryable** LINQ provider converts the LINQ expression into a query.</span></span> <span data-ttu-id="9ab32-119">Par conséquent, les performances dépendent sur le fournisseur LINQ qui est utilisé, ainsi que les caractéristiques particulières de votre schéma de base de données ou du jeu de données.</span><span class="sxs-lookup"><span data-stu-id="9ab32-119">Therefore, performance depends on the LINQ provider that is used, and also on the particular characteristics of your dataset or database schema.</span></span>

<span data-ttu-id="9ab32-120">Pour plus d’informations sur l’utilisation des options de requête OData dans l’API Web ASP.NET, consultez [prise en charge des Options de requête OData](supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="9ab32-120">For more information about using OData query options in ASP.NET Web API, see [Supporting OData Query Options](supporting-odata-query-options.md).</span></span>

<span data-ttu-id="9ab32-121">Si vous savez que tous les clients approuvés (par exemple, dans un environnement d’entreprise), ou si votre jeu de données est petite, les performances des requêtes est peut-être pas un problème.</span><span class="sxs-lookup"><span data-stu-id="9ab32-121">If you know that all clients are trusted (for example, in an enterprise environment), or if your dataset is small, query performance might not be an issue.</span></span> <span data-ttu-id="9ab32-122">Dans le cas contraire, vous devez envisager les recommandations suivantes.</span><span class="sxs-lookup"><span data-stu-id="9ab32-122">Otherwise, you should consider the following recommendations.</span></span>

- <span data-ttu-id="9ab32-123">Tester votre service avec différentes requêtes et la base de données de profil.</span><span class="sxs-lookup"><span data-stu-id="9ab32-123">Test your service with various queries and profile the DB.</span></span>
- <span data-ttu-id="9ab32-124">Activer la pagination orientée serveur éviter le renvoi d’un grand ensemble de données dans une requête.</span><span class="sxs-lookup"><span data-stu-id="9ab32-124">Enable server-driven paging, to avoid returning a large data set in one query.</span></span> <span data-ttu-id="9ab32-125">Pour plus d’informations, consultez [Server-Driven pagination](supporting-odata-query-options.md#server-paging).</span><span class="sxs-lookup"><span data-stu-id="9ab32-125">For more information, see [Server-Driven Paging](supporting-odata-query-options.md#server-paging).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- <span data-ttu-id="9ab32-126">Vous devez $filter et $orderby ?</span><span class="sxs-lookup"><span data-stu-id="9ab32-126">Do you need $filter and $orderby?</span></span> <span data-ttu-id="9ab32-127">Certaines applications peuvent autoriser la pagination, à l’aide de $top et $skip de client, mais désactivez les autres options de requête.</span><span class="sxs-lookup"><span data-stu-id="9ab32-127">Some applications might allow client paging, using $top and $skip, but disable the other query options.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- <span data-ttu-id="9ab32-128">Pensez à restreindre $orderby aux propriétés dans un index cluster.</span><span class="sxs-lookup"><span data-stu-id="9ab32-128">Consider restricting $orderby to properties in a clustered index.</span></span> <span data-ttu-id="9ab32-129">Le tri des données de grande taille sans index cluster est lent.</span><span class="sxs-lookup"><span data-stu-id="9ab32-129">Sorting large data without a clustered index is slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- <span data-ttu-id="9ab32-130">Nombre de nœuds maximale : le **à maxnodecount doit** propriété sur **[Queryable]** définit les nœuds de nombre maximales autorisés dans l’arborescence de syntaxe $filter.</span><span class="sxs-lookup"><span data-stu-id="9ab32-130">Maximum node count: The **MaxNodeCount** property on **[Queryable]** sets the maximum number nodes allowed in the $filter syntax tree.</span></span> <span data-ttu-id="9ab32-131">La valeur par défaut est 100, mais vous pouvez définir une valeur inférieure, car un grand nombre de nœuds peut être lent à compiler.</span><span class="sxs-lookup"><span data-stu-id="9ab32-131">The default value is 100, but you may want to set a lower value, because a large number of nodes can be slow to compile.</span></span> <span data-ttu-id="9ab32-132">Cela est particulièrement vrai si vous utilisez LINQ to Objects (autrement dit, les requêtes LINQ sur une collection en mémoire, sans l’utilisation d’un fournisseur LINQ intermédiaire).</span><span class="sxs-lookup"><span data-stu-id="9ab32-132">This is particularly true if you are using LINQ to Objects (i.e., LINQ queries on a collection in memory, without the use of an intermediate LINQ provider).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- <span data-ttu-id="9ab32-133">Pensez à désactiver les fonctions any() et all(), car il peuvent être lentes.</span><span class="sxs-lookup"><span data-stu-id="9ab32-133">Consider disabling the any() and all() functions, as these can be slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- <span data-ttu-id="9ab32-134">Si toutes les propriétés de chaîne contient des chaînes de grande taille #8212for exemple, une description de produit ou une entrée de blog & 8212consider # désactiver les fonctions de chaîne.</span><span class="sxs-lookup"><span data-stu-id="9ab32-134">If any string properties contain large strings&#8212for example, a product description or a blog entry&#8212consider disabling the string functions.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- <span data-ttu-id="9ab32-135">Envisagez d’en interdisant le filtrage sur les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="9ab32-135">Consider disallowing filtering on navigation properties.</span></span> <span data-ttu-id="9ab32-136">Le filtrage sur les propriétés de navigation peut entraîner une jointure qui peut être lente, en fonction de votre schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="9ab32-136">Filtering on navigation properties can result in a join, which might be slow, depending on your database schema.</span></span> <span data-ttu-id="9ab32-137">Le code suivant montre un validateur de requête qui empêche le filtrage sur les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="9ab32-137">The following code shows a query validator that prevents filtering on navigation properties.</span></span> <span data-ttu-id="9ab32-138">Pour plus d’informations sur les programmes de validation de requête, consultez [Validation de requête](supporting-odata-query-options.md#query-validation).</span><span class="sxs-lookup"><span data-stu-id="9ab32-138">For more information about query validators, see [Query Validation](supporting-odata-query-options.md#query-validation).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- <span data-ttu-id="9ab32-139">Pensez à limiter les requêtes $filter en écrivant un validateur personnalisé pour votre base de données.</span><span class="sxs-lookup"><span data-stu-id="9ab32-139">Consider restricting $filter queries by writing a validator that is customized for your database.</span></span> <span data-ttu-id="9ab32-140">Par exemple, considérez les deux requêtes suivantes :</span><span class="sxs-lookup"><span data-stu-id="9ab32-140">For example, consider these two queries:</span></span> 

  - <span data-ttu-id="9ab32-141">Tous les films avec acteurs dont le nom commence par « A ».</span><span class="sxs-lookup"><span data-stu-id="9ab32-141">All movies with actors whose last name starts with ‘A'.</span></span>
  - <span data-ttu-id="9ab32-142">Tous les films publiées en 1994.</span><span class="sxs-lookup"><span data-stu-id="9ab32-142">All movies released in 1994.</span></span>

    <span data-ttu-id="9ab32-143">À moins que les films sont indexés par acteurs, la première requête peut nécessiter le moteur de base de données à analyser l’intégralité de la liste de films.</span><span class="sxs-lookup"><span data-stu-id="9ab32-143">Unless movies are indexed by actors, the first query might require the DB engine to scan the entire list of movies.</span></span> <span data-ttu-id="9ab32-144">Tandis que la deuxième requête peut être acceptable, en supposant que films sont indexées par année de sortie.</span><span class="sxs-lookup"><span data-stu-id="9ab32-144">Whereas the second query might be acceptable, assuming movies are indexed by release year.</span></span>

    <span data-ttu-id="9ab32-145">Le code suivant montre un validateur qui permet de filtrer sur les propriétés « ReleaseYear » et « Title », mais aucune autre propriété.</span><span class="sxs-lookup"><span data-stu-id="9ab32-145">The following code shows a validator that allows filtering on the "ReleaseYear" and "Title" properties but no other properties.</span></span>

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- <span data-ttu-id="9ab32-146">En règle générale, envisagez les fonctions $filter que vous avez besoin.</span><span class="sxs-lookup"><span data-stu-id="9ab32-146">In general, consider which $filter functions you need.</span></span> <span data-ttu-id="9ab32-147">Si vos clients ne doivent pas l’expressivité complète de $filter, vous pouvez limiter les fonctions admises.</span><span class="sxs-lookup"><span data-stu-id="9ab32-147">If your clients do not need the full expressiveness of $filter, you can limit the allowed functions.</span></span>
