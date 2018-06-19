---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Héritage de Type complexe dans OData v4 avec ASP.NET Web API | Documents Microsoft
author: microsoft
description: Selon la spécification OData v4, un type complexe peut hériter d’un autre type complex. (Un type complexe est un type structuré sans une clé.) API Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: be2dbfa82b99b6c48928e4e767716852c14a463b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508418"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="50f04-104">Héritage de Type complexe dans OData v4 avec l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="50f04-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="50f04-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="50f04-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="50f04-106">Selon la norme OData v4 [spécification](http://www.odata.org/documentation/odata-version-4-0/), un type complex peut hériter d’un autre type complexe.</span><span class="sxs-lookup"><span data-stu-id="50f04-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="50f04-107">(A *complexes* type est un type structuré sans une clé.) API Web OData 5.3 prend en charge l’héritage de type complexe.</span><span class="sxs-lookup"><span data-stu-id="50f04-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="50f04-108">Cette rubrique montre comment créer un entity data model (EDM) avec des types complexes d’héritage.</span><span class="sxs-lookup"><span data-stu-id="50f04-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="50f04-109">Pour le code source complet, consultez [exemple d’héritage de Type complexe de OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="50f04-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="50f04-110">Versions du logiciel utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="50f04-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="50f04-111">OData d’API Web 5.3</span><span class="sxs-lookup"><span data-stu-id="50f04-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="50f04-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="50f04-112">OData v4</span></span>


## <a name="model-hierarchy"></a><span data-ttu-id="50f04-113">Hiérarchie de modèle</span><span class="sxs-lookup"><span data-stu-id="50f04-113">Model Hierarchy</span></span>

<span data-ttu-id="50f04-114">Pour illustrer l’héritage de type complexe, nous allons utiliser la hiérarchie de classe suivante.</span><span class="sxs-lookup"><span data-stu-id="50f04-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="50f04-115">`Shape`est un type complexe abstrait.</span><span class="sxs-lookup"><span data-stu-id="50f04-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="50f04-116">`Rectangle`, `Triangle`, et `Circle` sont des types complexes dérivés `Shape`, et `RoundRectangle` dérive `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="50f04-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="50f04-117">`Window`est un type d’entité et contient un `Shape` instance.</span><span class="sxs-lookup"><span data-stu-id="50f04-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="50f04-118">Voici les classes CLR qui définissent ces types.</span><span class="sxs-lookup"><span data-stu-id="50f04-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="50f04-119">Générer le modèle EDM</span><span class="sxs-lookup"><span data-stu-id="50f04-119">Build the EDM Model</span></span>

<span data-ttu-id="50f04-120">Pour créer le modèle EDM, vous pouvez utiliser **ODataConventionModelBuilder**, lequel déduit les relations d’héritage à partir des types CLR.</span><span class="sxs-lookup"><span data-stu-id="50f04-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="50f04-121">Vous pouvez également générer le modèle EDM explicitement, à l’aide de **odatamodelbuilder ayant**.</span><span class="sxs-lookup"><span data-stu-id="50f04-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="50f04-122">Cela prend plus de code, mais vous donne davantage de contrôle sur le modèle EDM.</span><span class="sxs-lookup"><span data-stu-id="50f04-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="50f04-123">Ces deux exemples de créent le même schéma EDM.</span><span class="sxs-lookup"><span data-stu-id="50f04-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="50f04-124">Document de métadonnées</span><span class="sxs-lookup"><span data-stu-id="50f04-124">Metadata Document</span></span>

<span data-ttu-id="50f04-125">Voici le document de métadonnées OData, montrant l’héritage de type complexe.</span><span class="sxs-lookup"><span data-stu-id="50f04-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="50f04-126">À partir du document de métadonnées, vous pouvez voir que :</span><span class="sxs-lookup"><span data-stu-id="50f04-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="50f04-127">Le `Shape` type complexe est abstraite.</span><span class="sxs-lookup"><span data-stu-id="50f04-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="50f04-128">Le `Rectangle`, `Triangle`, et `Circle` type complexe ont le type de base `Shape`.</span><span class="sxs-lookup"><span data-stu-id="50f04-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="50f04-129">Le `RoundRectangle` type a le type de base `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="50f04-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="50f04-130">Conversion de Types complexes</span><span class="sxs-lookup"><span data-stu-id="50f04-130">Casting Complex Types</span></span>

<span data-ttu-id="50f04-131">Effectuer un cast sur des types complexes est maintenant pris en charge.</span><span class="sxs-lookup"><span data-stu-id="50f04-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="50f04-132">Par exemple, la requête suivante casts un `Shape` à un `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="50f04-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="50f04-133">Voici la charge utile de réponse :</span><span class="sxs-lookup"><span data-stu-id="50f04-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
