---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: Quelles sont les nouveautés dans ASP.NET Web API OData 5.3 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 09/16/2014
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: c185e7ef9bfe6e21601ab61c418c63c8a81b680a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827027"
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a><span data-ttu-id="44a9f-102">Quelles sont les nouveautés dans ASP.NET Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="44a9f-102">What's New in ASP.NET Web API OData 5.3</span></span>
====================
<span data-ttu-id="44a9f-103">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="44a9f-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="44a9f-104">Cette rubrique décrit les nouveautés de ASP.NET Web API OData 5.3.</span><span class="sxs-lookup"><span data-stu-id="44a9f-104">This topic describes what's new for ASP.NET Web API OData 5.3.</span></span>

- [<span data-ttu-id="44a9f-105">Télécharger</span><span class="sxs-lookup"><span data-stu-id="44a9f-105">Download</span></span>](#download)
- [<span data-ttu-id="44a9f-106">Documentation</span><span class="sxs-lookup"><span data-stu-id="44a9f-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="44a9f-107">Bibliothèques OData Core</span><span class="sxs-lookup"><span data-stu-id="44a9f-107">OData Core Libraries</span></span>](#corelib)
- [<span data-ttu-id="44a9f-108">Nouvelles fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="44a9f-108">New Features</span></span>](#newf)
- [<span data-ttu-id="44a9f-109">Problèmes connus et les modifications avec rupture</span><span class="sxs-lookup"><span data-stu-id="44a9f-109">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="44a9f-110">Correctifs de bogues</span><span class="sxs-lookup"><span data-stu-id="44a9f-110">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="44a9f-111">API Web ASP.NET OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="44a9f-111">ASP.NET Web API OData 5.3.1</span></span>](#OD)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="44a9f-112">Téléchargement</span><span class="sxs-lookup"><span data-stu-id="44a9f-112">Download</span></span>

<span data-ttu-id="44a9f-113">Les fonctionnalités de runtime sont publiées sous forme de packages NuGet dans la galerie NuGet.</span><span class="sxs-lookup"><span data-stu-id="44a9f-113">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="44a9f-114">Vous pouvez installer ou mettre à jour vers les packages NuGet publiées à l’aide de la Console du Gestionnaire de Package NuGet :</span><span class="sxs-lookup"><span data-stu-id="44a9f-114">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="44a9f-115">Documentation</span><span class="sxs-lookup"><span data-stu-id="44a9f-115">Documentation</span></span>

<span data-ttu-id="44a9f-116">Vous trouverez des didacticiels et autres documentations sur les API Web ASP.NET OData à le [site web ASP.NET](../odata-support-in-aspnet-web-api/index.md).</span><span class="sxs-lookup"><span data-stu-id="44a9f-116">You can find tutorials and other documentation about ASP.NET Web API OData at the [ASP.NET web site](../odata-support-in-aspnet-web-api/index.md).</span></span>

<a id="corelib"></a>
## <a name="odata-core-libraries"></a><span data-ttu-id="44a9f-117">Bibliothèques OData Core</span><span class="sxs-lookup"><span data-stu-id="44a9f-117">OData Core Libraries</span></span>

<span data-ttu-id="44a9f-118">Pour OData v4, API Web utilise désormais ODataLib version 6.5.0</span><span class="sxs-lookup"><span data-stu-id="44a9f-118">For OData v4, Web API now uses ODataLib version 6.5.0</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a><span data-ttu-id="44a9f-119">Nouvelles fonctionnalités dans ASP.NET Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="44a9f-119">New Features in ASP.NET Web API OData 5.3</span></span>

### <a name="support-for-levels-in-expand"></a><span data-ttu-id="44a9f-120">Prise en charge pour $levels dans $développez</span><span class="sxs-lookup"><span data-stu-id="44a9f-120">Support for $levels in $expand</span></span>

<span data-ttu-id="44a9f-121">Vous pouvez utiliser la $levels option dans $développer des requêtes de requête.</span><span class="sxs-lookup"><span data-stu-id="44a9f-121">You can use the $levels query option in $expand queries.</span></span> <span data-ttu-id="44a9f-122">Exemple :</span><span class="sxs-lookup"><span data-stu-id="44a9f-122">For example:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

<span data-ttu-id="44a9f-123">Cette requête est équivalente à :</span><span class="sxs-lookup"><span data-stu-id="44a9f-123">This query is equivalent to:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a><span data-ttu-id="44a9f-124">Prise en charge pour les Types d’entité ouverte</span><span class="sxs-lookup"><span data-stu-id="44a9f-124">Support for Open Entity Types</span></span>

<span data-ttu-id="44a9f-125">Un *ouvrir type* est un type stuctured qui contient les propriétés dynamiques, en plus de toutes les propriétés qui sont déclarées dans la définition de type.</span><span class="sxs-lookup"><span data-stu-id="44a9f-125">An *open type* is a stuctured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="44a9f-126">Les types ouverts vous permettent d’ajouter de la flexibilité à vos modèles de données.</span><span class="sxs-lookup"><span data-stu-id="44a9f-126">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="44a9f-127">Pour plus d’informations, consultez xxxx.</span><span class="sxs-lookup"><span data-stu-id="44a9f-127">For more information, see xxxx.</span></span>

### <a name="support-for-dynamic-collection-properties-in-open-types"></a><span data-ttu-id="44a9f-128">Prise en charge pour les propriétés de regroupement dynamique de types ouverts</span><span class="sxs-lookup"><span data-stu-id="44a9f-128">Support for dynamic collection properties in open types</span></span>

<span data-ttu-id="44a9f-129">Auparavant, une propriété dynamique devait être une valeur unique.</span><span class="sxs-lookup"><span data-stu-id="44a9f-129">Previously, a dynamic property had to be a single value.</span></span> <span data-ttu-id="44a9f-130">5.3, les propriétés dynamiques peuvent avoir des valeurs de la collection.</span><span class="sxs-lookup"><span data-stu-id="44a9f-130">In 5.3, dynamic properties can have collection values.</span></span> <span data-ttu-id="44a9f-131">Par exemple, dans la charge utile JSON ci-après, le `Emails` propriété est une propriété dynamique et est de collection de type chaîne :</span><span class="sxs-lookup"><span data-stu-id="44a9f-131">For example, in the following JSON payload, the `Emails` property is a dynamic property and is of collection of string type:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a><span data-ttu-id="44a9f-132">Prise en charge de l’héritage pour les types complexes</span><span class="sxs-lookup"><span data-stu-id="44a9f-132">Support for inheritance for complex types</span></span>

<span data-ttu-id="44a9f-133">Désormais, les types complexes peuvent hériter d’un type de base.</span><span class="sxs-lookup"><span data-stu-id="44a9f-133">Now complex types can inherit from a base type.</span></span> <span data-ttu-id="44a9f-134">Par exemple, un service OData peut définir des types complexes suivants :</span><span class="sxs-lookup"><span data-stu-id="44a9f-134">For example, an OData service could define the following complex types:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

<span data-ttu-id="44a9f-135">Voici le modèle EDM pour cet exemple :</span><span class="sxs-lookup"><span data-stu-id="44a9f-135">Here is the EDM for this example:</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

<span data-ttu-id="44a9f-136">Pour plus d’informations, consultez [exemple de l’héritage de Type complexe OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="44a9f-136">For more information, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="44a9f-137">Problèmes connus et les modifications avec rupture</span><span class="sxs-lookup"><span data-stu-id="44a9f-137">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="44a9f-138">Cette section décrit les problèmes connus et les modifications avec rupture dans ASP.NET Web API OData 5.3.</span><span class="sxs-lookup"><span data-stu-id="44a9f-138">This section describes known issues and breaking changes in the ASP.NET Web API OData 5.3.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="44a9f-139">OData v4</span><span class="sxs-lookup"><span data-stu-id="44a9f-139">OData v4</span></span>

#### <a name="query-options"></a><span data-ttu-id="44a9f-140">Options de requête</span><span class="sxs-lookup"><span data-stu-id="44a9f-140">Query Options</span></span>

<span data-ttu-id="44a9f-141">Problème : À l’aide de $ imbriquée développez avec $levels = nombre maximal de résultats dans une profondeur d’expansion incorrect.</span><span class="sxs-lookup"><span data-stu-id="44a9f-141">Issue: Using nested $expand with $levels=max results in an incorrect expansion depth.</span></span>

<span data-ttu-id="44a9f-142">Par exemple, étant donné la requête suivante :</span><span class="sxs-lookup"><span data-stu-id="44a9f-142">For example, given the following request:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

<span data-ttu-id="44a9f-143">Si `MaxExpansionDepth` est 5, cette requête entraînerait une profondeur d’expansion de 6.</span><span class="sxs-lookup"><span data-stu-id="44a9f-143">If `MaxExpansionDepth` is 5, this query would result in an expansion depth of 6.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="44a9f-144">Correctifs de bogues et mises à jour de fonctionnalité secondaire</span><span class="sxs-lookup"><span data-stu-id="44a9f-144">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="44a9f-145">Cette version inclut également plusieurs correctifs de bogues et fonctionnalités mineures mises à jour.</span><span class="sxs-lookup"><span data-stu-id="44a9f-145">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="44a9f-146">Vous trouverez la liste complète ici :</span><span class="sxs-lookup"><span data-stu-id="44a9f-146">You can find the complete list here:</span></span>

- [<span data-ttu-id="44a9f-147">Correctifs de bogues</span><span class="sxs-lookup"><span data-stu-id="44a9f-147">Bug fixes</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a><span data-ttu-id="44a9f-148">API Web ASP.NET OData 5.3.1</span><span class="sxs-lookup"><span data-stu-id="44a9f-148">ASP.NET Web API OData 5.3.1</span></span>

<span data-ttu-id="44a9f-149">Dans cette version, nous avons apporté un [correctif de bogue](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) à certaines des énumérations AllowedFunctions.</span><span class="sxs-lookup"><span data-stu-id="44a9f-149">In this release we made a [bug fix](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) to some of the AllowedFunctions enums.</span></span> <span data-ttu-id="44a9f-150">Cette version n’a pas les autres correctifs de bogues ou de nouvelles fonctionnalités.</span><span class="sxs-lookup"><span data-stu-id="44a9f-150">This release doesn't have any other bug fixes or new features.</span></span>
