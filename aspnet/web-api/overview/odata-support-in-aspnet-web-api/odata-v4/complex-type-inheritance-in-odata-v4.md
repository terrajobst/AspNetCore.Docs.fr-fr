---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Héritage de Type complexe dans OData v4 avec l’API Web ASP.NET | Microsoft Docs
author: microsoft
description: Selon la spécification OData v4, un type complexe peut hériter d’un autre type complex. (Un type complexe est un type structuré sans une clé.) API Web...
ms.author: aspnetcontent
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: d295a6ae20f5771ae1f4f28166f7e651b6ec5c58
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824028"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="03eb8-104">Héritage de Type complexe dans OData v4 avec l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="03eb8-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="03eb8-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="03eb8-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="03eb8-106">En fonction de la norme OData v4 [spécification](http://www.odata.org/documentation/odata-version-4-0/), un type complex peut hériter d’un autre type complexe.</span><span class="sxs-lookup"><span data-stu-id="03eb8-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="03eb8-107">(Un *complexes* type est un type structuré sans une clé.) API Web OData 5.3 prend en charge l’héritage de type complexe.</span><span class="sxs-lookup"><span data-stu-id="03eb8-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="03eb8-108">Cette rubrique montre comment créer un entity data model (EDM) avec des types complexes d’héritage.</span><span class="sxs-lookup"><span data-stu-id="03eb8-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="03eb8-109">Pour le code source complet, consultez [exemple de l’héritage de Type complexe OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="03eb8-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="03eb8-110">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="03eb8-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="03eb8-111">API Web OData 5.3</span><span class="sxs-lookup"><span data-stu-id="03eb8-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="03eb8-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="03eb8-112">OData v4</span></span>


## <a name="model-hierarchy"></a><span data-ttu-id="03eb8-113">Hiérarchie du modèle</span><span class="sxs-lookup"><span data-stu-id="03eb8-113">Model Hierarchy</span></span>

<span data-ttu-id="03eb8-114">Pour illustrer l’héritage de type complexe, nous allons utiliser la hiérarchie de classe suivante.</span><span class="sxs-lookup"><span data-stu-id="03eb8-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="03eb8-115">`Shape` est un type complexe abstrait.</span><span class="sxs-lookup"><span data-stu-id="03eb8-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="03eb8-116">`Rectangle`, `Triangle`, et `Circle` sont des types complexes dérivés de `Shape`, et `RoundRectangle` dérive `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="03eb8-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="03eb8-117">`Window` est un type d’entité et contient un `Shape` instance.</span><span class="sxs-lookup"><span data-stu-id="03eb8-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="03eb8-118">Voici les classes CLR qui définissent ces types.</span><span class="sxs-lookup"><span data-stu-id="03eb8-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="03eb8-119">Générer le modèle EDM</span><span class="sxs-lookup"><span data-stu-id="03eb8-119">Build the EDM Model</span></span>

<span data-ttu-id="03eb8-120">Pour créer le modèle EDM, vous pouvez utiliser **ODataConventionModelBuilder**, qui déduit les relations d’héritage à partir des types CLR.</span><span class="sxs-lookup"><span data-stu-id="03eb8-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="03eb8-121">Vous pouvez également créer le modèle EDM explicitement, à l’aide de **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="03eb8-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="03eb8-122">Cela prend plus de code, mais vous donne davantage de contrôle sur le modèle EDM.</span><span class="sxs-lookup"><span data-stu-id="03eb8-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="03eb8-123">Ces deux exemples créent le même schéma EDM.</span><span class="sxs-lookup"><span data-stu-id="03eb8-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="03eb8-124">Document de métadonnées</span><span class="sxs-lookup"><span data-stu-id="03eb8-124">Metadata Document</span></span>

<span data-ttu-id="03eb8-125">Voici le document de métadonnées OData, montrant l’héritage de type complexe.</span><span class="sxs-lookup"><span data-stu-id="03eb8-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="03eb8-126">À partir du document de métadonnées, vous pouvez voir que :</span><span class="sxs-lookup"><span data-stu-id="03eb8-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="03eb8-127">Le `Shape` type complexe est abstrait.</span><span class="sxs-lookup"><span data-stu-id="03eb8-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="03eb8-128">Le `Rectangle`, `Triangle`, et `Circle` type complexe ont le type de base `Shape`.</span><span class="sxs-lookup"><span data-stu-id="03eb8-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="03eb8-129">Le `RoundRectangle` type a le type de base `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="03eb8-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="03eb8-130">Conversion de Types complexes</span><span class="sxs-lookup"><span data-stu-id="03eb8-130">Casting Complex Types</span></span>

<span data-ttu-id="03eb8-131">Un cast sur des types complexes est désormais pris en charge.</span><span class="sxs-lookup"><span data-stu-id="03eb8-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="03eb8-132">Par exemple, la requête suivante casts un `Shape` à un `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="03eb8-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="03eb8-133">Voici la charge utile de réponse :</span><span class="sxs-lookup"><span data-stu-id="03eb8-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
