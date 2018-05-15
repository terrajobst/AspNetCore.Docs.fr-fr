---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Ouvrir des Types dans OData v4 avec ASP.NET Web API | Documents Microsoft
author: microsoft
description: Dans OData v4, un type ouvert est un type de stuctured qui contient les propriétés dynamiques, en plus de toutes les propriétés qui sont déclarées dans la définition de type. Ouvrir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2014
ms.topic: article
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: fe67b9a11a82b55d5f3e0e5f1b0cee10a58833d2
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2018
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="1620a-104">Ouvrir des Types dans OData v4 avec l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1620a-104">Open Types in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="1620a-105">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1620a-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="1620a-106">Dans OData v4, un *ouvrir type* est un type de stuctured qui contient les propriétés dynamiques, en plus de toutes les propriétés qui sont déclarées dans la définition de type.</span><span class="sxs-lookup"><span data-stu-id="1620a-106">In OData v4, an *open type* is a stuctured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="1620a-107">Les types ouverts permettent d’ajouter une grande souplesse à vos modèles de données.</span><span class="sxs-lookup"><span data-stu-id="1620a-107">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="1620a-108">Ce didacticiel montre comment utiliser les types ouverts dans ASP.NET Web API OData.</span><span class="sxs-lookup"><span data-stu-id="1620a-108">This tutorial shows how to use open types in ASP.NET Web API OData.</span></span>
> 
> <span data-ttu-id="1620a-109">Ce didacticiel suppose que vous savez déjà comment créer un point de terminaison OData dans l’API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1620a-109">This tutorial assumes that you already know how to create an OData endpoint in ASP.NET Web API.</span></span> <span data-ttu-id="1620a-110">Dans le cas contraire, commencez par lire [créer un point de terminaison OData v4](create-an-odata-v4-endpoint.md) première.</span><span class="sxs-lookup"><span data-stu-id="1620a-110">If not, start by reading [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md) first.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="1620a-111">Versions du logiciel utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="1620a-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="1620a-112">OData d’API Web 5.3</span><span class="sxs-lookup"><span data-stu-id="1620a-112">Web API OData 5.3</span></span>
> - <span data-ttu-id="1620a-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="1620a-113">OData v4</span></span>


<span data-ttu-id="1620a-114">Première, certains termes OData :</span><span class="sxs-lookup"><span data-stu-id="1620a-114">First, some OData terminology:</span></span>

- <span data-ttu-id="1620a-115">Type d’entité : un type structuré avec une clé.</span><span class="sxs-lookup"><span data-stu-id="1620a-115">Entity type: A structured type with a key.</span></span>
- <span data-ttu-id="1620a-116">Type complexe : un type structuré sans une clé.</span><span class="sxs-lookup"><span data-stu-id="1620a-116">Complex type: A structured type without a key.</span></span>
- <span data-ttu-id="1620a-117">Ouvrez le type : un type avec des propriétés dynamiques.</span><span class="sxs-lookup"><span data-stu-id="1620a-117">Open type: A type with dynamic properties.</span></span> <span data-ttu-id="1620a-118">Types d’entités et types complexes peuvent être ouverts.</span><span class="sxs-lookup"><span data-stu-id="1620a-118">Both entity types and complex types can be open.</span></span>

<span data-ttu-id="1620a-119">La valeur d’une propriété dynamique peut être un type primitif, un type complexe ou un type énumération ; ou une collection d’un de ces types.</span><span class="sxs-lookup"><span data-stu-id="1620a-119">The value of a dynamic property can be a primitive type, complex type, or enumeration type; or a collection of any of those types.</span></span> <span data-ttu-id="1620a-120">Pour plus d’informations sur les types ouverts, consultez la [OData v4 spécification](http://www.odata.org/documentation/odata-version-4-0/).</span><span class="sxs-lookup"><span data-stu-id="1620a-120">For more information about open types, see the [OData v4 specification](http://www.odata.org/documentation/odata-version-4-0/).</span></span>

## <a name="install-the-web-odata-libraries"></a><span data-ttu-id="1620a-121">Installer les bibliothèques d’OData Web</span><span class="sxs-lookup"><span data-stu-id="1620a-121">Install the Web OData Libraries</span></span>

<span data-ttu-id="1620a-122">Utilisez le Gestionnaire de Package NuGet pour installer les dernières bibliothèques d’OData d’API Web.</span><span class="sxs-lookup"><span data-stu-id="1620a-122">Use NuGet Package Manager to install the latest Web API OData libraries.</span></span> <span data-ttu-id="1620a-123">À partir de la fenêtre de Console du Gestionnaire de Package :</span><span class="sxs-lookup"><span data-stu-id="1620a-123">From the Package Manager Console window:</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a><span data-ttu-id="1620a-124">Définir les Types CLR</span><span class="sxs-lookup"><span data-stu-id="1620a-124">Define the CLR Types</span></span>

<span data-ttu-id="1620a-125">Commencez par définir les modèles EDM en tant que types CLR.</span><span class="sxs-lookup"><span data-stu-id="1620a-125">Start by defining the EDM models as CLR types.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="1620a-126">Lorsque le modèle EDM (Entity Data Model) est créé,</span><span class="sxs-lookup"><span data-stu-id="1620a-126">When the Entity Data Model (EDM) is created,</span></span>

- <span data-ttu-id="1620a-127">`Category`est un type énumération.</span><span class="sxs-lookup"><span data-stu-id="1620a-127">`Category` is an enumeration type.</span></span>
- <span data-ttu-id="1620a-128">`Address`est un type complexe.</span><span class="sxs-lookup"><span data-stu-id="1620a-128">`Address` is a complex type.</span></span> <span data-ttu-id="1620a-129">(Il n’a pas une clé, il n’est pas un type d’entité.)</span><span class="sxs-lookup"><span data-stu-id="1620a-129">(It does not have a key, so it is not an entity type.)</span></span>
- <span data-ttu-id="1620a-130">`Customer`est un type d’entité.</span><span class="sxs-lookup"><span data-stu-id="1620a-130">`Customer` is an entity type.</span></span> <span data-ttu-id="1620a-131">(Elle a une clé.)</span><span class="sxs-lookup"><span data-stu-id="1620a-131">(It has a key.)</span></span>
- <span data-ttu-id="1620a-132">`Press`est un type complex ouvert.</span><span class="sxs-lookup"><span data-stu-id="1620a-132">`Press` is an open complex type.</span></span>
- <span data-ttu-id="1620a-133">`Book`est un type d’entité ouvert.</span><span class="sxs-lookup"><span data-stu-id="1620a-133">`Book` is an open entity type.</span></span>

<span data-ttu-id="1620a-134">Pour créer un type ouvert, le type CLR doit avoir une propriété de type `IDictionary<string, object>`, qui contient les propriétés dynamiques.</span><span class="sxs-lookup"><span data-stu-id="1620a-134">To create an open type, the CLR type must have a property of type `IDictionary<string, object>`, which holds the dynamic properties.</span></span>

## <a name="build-the-edm-model"></a><span data-ttu-id="1620a-135">Générer le modèle EDM</span><span class="sxs-lookup"><span data-stu-id="1620a-135">Build the EDM Model</span></span>

<span data-ttu-id="1620a-136">Si vous utilisez **ODataConventionModelBuilder** pour créer le modèle EDM, `Press` et `Book` sont automatiquement ajoutés en tant que types ouverts, en fonction de la présence d’un `IDictionary<string, object>` propriété.</span><span class="sxs-lookup"><span data-stu-id="1620a-136">If you use **ODataConventionModelBuilder** to create the EDM, `Press` and `Book` are automatically added as open types, based on the presence of a `IDictionary<string, object>` property.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="1620a-137">Vous pouvez également générer le modèle EDM explicitement, à l’aide de **odatamodelbuilder ayant**.</span><span class="sxs-lookup"><span data-stu-id="1620a-137">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a><span data-ttu-id="1620a-138">Ajouter un contrôleur OData</span><span class="sxs-lookup"><span data-stu-id="1620a-138">Add an OData Controller</span></span>

<span data-ttu-id="1620a-139">Ensuite, ajoutez un contrôleur OData.</span><span class="sxs-lookup"><span data-stu-id="1620a-139">Next, add an OData controller.</span></span> <span data-ttu-id="1620a-140">Pour ce didacticiel, nous allons utiliser un contrôleur simplifié que prend en charge uniquement obtenir et valider des demandes et utilise une liste en mémoire pour stocker des entités.</span><span class="sxs-lookup"><span data-stu-id="1620a-140">For this tutorial, we'll use a simplified controller that just supports GET and POST requests, and uses an in-memory list to store entities.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="1620a-141">Notez que la première `Book` instance ne possède pas de propriétés dynamiques.</span><span class="sxs-lookup"><span data-stu-id="1620a-141">Notice that the first `Book` instance has no dynamic properties.</span></span> <span data-ttu-id="1620a-142">La seconde `Book` instance a les propriétés dynamiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="1620a-142">The second `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="1620a-143">« Publié » : type primitif</span><span class="sxs-lookup"><span data-stu-id="1620a-143">"Published": Primitive type</span></span>
- <span data-ttu-id="1620a-144">« Auteurs » : Collection de types primitifs</span><span class="sxs-lookup"><span data-stu-id="1620a-144">"Authors": Collection of primitive types</span></span>
- <span data-ttu-id="1620a-145">« OtherCategories » : Collection de types d’énumération.</span><span class="sxs-lookup"><span data-stu-id="1620a-145">"OtherCategories": Collection of enumeration types.</span></span>

<span data-ttu-id="1620a-146">En outre, le `Press` propriété dudit `Book` instance a les propriétés dynamiques suivantes :</span><span class="sxs-lookup"><span data-stu-id="1620a-146">Also, the `Press` property of that `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="1620a-147">« Blog » : type primitif</span><span class="sxs-lookup"><span data-stu-id="1620a-147">"Blog": Primitive type</span></span>
- <span data-ttu-id="1620a-148">"Address": Complex type</span><span class="sxs-lookup"><span data-stu-id="1620a-148">"Address": Complex type</span></span>

## <a name="query-the-metadata"></a><span data-ttu-id="1620a-149">Interroger les métadonnées</span><span class="sxs-lookup"><span data-stu-id="1620a-149">Query the Metadata</span></span>

<span data-ttu-id="1620a-150">Pour obtenir le document de métadonnées OData, envoyez une demande GET pour `~/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="1620a-150">To get the OData metadata document, send a GET request to `~/$metadata`.</span></span> <span data-ttu-id="1620a-151">Le corps de réponse doit ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="1620a-151">The response body should look similar to this:</span></span>

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

<span data-ttu-id="1620a-152">À partir du document de métadonnées, vous pouvez voir que :</span><span class="sxs-lookup"><span data-stu-id="1620a-152">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="1620a-153">Pour le `Book` et `Press` types, la valeur de la `OpenType` attribut a la valeur true.</span><span class="sxs-lookup"><span data-stu-id="1620a-153">For the `Book` and `Press` types, the value of the `OpenType` attribute is true.</span></span> <span data-ttu-id="1620a-154">Le `Customer` et `Address` types ne possèdent pas cet attribut.</span><span class="sxs-lookup"><span data-stu-id="1620a-154">The `Customer` and `Address` types don't have this attribute.</span></span>
- <span data-ttu-id="1620a-155">Le `Book` type d’entité possède trois propriétés déclarées : ISBN, le titre et appuyez sur.</span><span class="sxs-lookup"><span data-stu-id="1620a-155">The `Book` entity type has three declared properties: ISBN, Title, and Press.</span></span> <span data-ttu-id="1620a-156">Les métadonnées OData n’incluent pas le `Book.Properties` propriété à partir de la classe CLR.</span><span class="sxs-lookup"><span data-stu-id="1620a-156">The OData metadata does not include the `Book.Properties` property from the CLR class.</span></span>
- <span data-ttu-id="1620a-157">De même, la `Press` type complexe possède uniquement deux propriétés déclarées : nom et la catégorie.</span><span class="sxs-lookup"><span data-stu-id="1620a-157">Similarly, the `Press` complex type has only two declared properties: Name and Category.</span></span> <span data-ttu-id="1620a-158">Les métadonnées n’incluent pas le `Press.DynamicProperties` propriété à partir de la classe CLR.</span><span class="sxs-lookup"><span data-stu-id="1620a-158">The metadata does not include the `Press.DynamicProperties` property from the CLR class.</span></span>

## <a name="query-an-entity"></a><span data-ttu-id="1620a-159">Une entité de requête</span><span class="sxs-lookup"><span data-stu-id="1620a-159">Query an Entity</span></span>

<span data-ttu-id="1620a-160">Pour obtenir le livre avec ISBN égal à « 978-0-7356-7942-9 », envoyer une demande GET pour `~/Books('978-0-7356-7942-9')`.</span><span class="sxs-lookup"><span data-stu-id="1620a-160">To get the book with ISBN equal to "978-0-7356-7942-9", send a GET request to `~/Books('978-0-7356-7942-9')`.</span></span> <span data-ttu-id="1620a-161">Le corps de réponse doit ressembler à ce qui suit.</span><span class="sxs-lookup"><span data-stu-id="1620a-161">The response body should look similar to the following.</span></span> <span data-ttu-id="1620a-162">(Mise en retrait pour le rendre plus lisible.)</span><span class="sxs-lookup"><span data-stu-id="1620a-162">(Indented to make it more readable.)</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

<span data-ttu-id="1620a-163">Notez que les propriétés dynamiques sont incluses inline avec les propriétés déclarées.</span><span class="sxs-lookup"><span data-stu-id="1620a-163">Notice that the dynamic properties are included inline with the declared properties.</span></span>

## <a name="post-an-entity"></a><span data-ttu-id="1620a-164">VALIDER une entité</span><span class="sxs-lookup"><span data-stu-id="1620a-164">POST an Entity</span></span>

<span data-ttu-id="1620a-165">Pour ajouter une entité Book, envoyer une demande POST à `~/Books`.</span><span class="sxs-lookup"><span data-stu-id="1620a-165">To add a Book entity, send a POST request to `~/Books`.</span></span> <span data-ttu-id="1620a-166">Le client peut définir des propriétés dynamiques dans la charge utile de demande.</span><span class="sxs-lookup"><span data-stu-id="1620a-166">The client can set dynamic properties in the request payload.</span></span>

<span data-ttu-id="1620a-167">Voici un exemple de demande.</span><span class="sxs-lookup"><span data-stu-id="1620a-167">Here is an example request.</span></span> <span data-ttu-id="1620a-168">Notez les propriétés « Price » et « Publié ».</span><span class="sxs-lookup"><span data-stu-id="1620a-168">Note the "Price" and "Published" properties.</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

<span data-ttu-id="1620a-169">Si vous définissez un point d’arrêt dans la méthode du contrôleur, vous pouvez voir que les API Web ajouté à ces propriétés pour le `Properties` dictionnaire.</span><span class="sxs-lookup"><span data-stu-id="1620a-169">If you set a breakpoint in the controller method, you can see that Web API added these properties to the `Properties` dictionary.</span></span>

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="1620a-170">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="1620a-170">Additional Resources</span></span>

[<span data-ttu-id="1620a-171">Exemple de Type OData ouvrir</span><span class="sxs-lookup"><span data-stu-id="1620a-171">OData Open Type Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
