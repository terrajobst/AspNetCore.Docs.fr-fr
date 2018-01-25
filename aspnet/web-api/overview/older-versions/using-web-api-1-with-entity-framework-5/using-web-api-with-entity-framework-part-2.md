---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: "Partie 2 : Créer les modèles de domaine | Documents Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: a573b47d27767dc78d557cd2b6c73714eb9e94f4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
<a name="part-2-creating-the-domain-models"></a><span data-ttu-id="5fd17-102">Partie 2 : Créer les modèles de domaine</span><span class="sxs-lookup"><span data-stu-id="5fd17-102">Part 2: Creating the Domain Models</span></span>
====================
<span data-ttu-id="5fd17-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5fd17-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="5fd17-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="5fd17-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a><span data-ttu-id="5fd17-105">Ajouter des modèles</span><span class="sxs-lookup"><span data-stu-id="5fd17-105">Add Models</span></span>

<span data-ttu-id="5fd17-106">Il existe trois façons de l’approche Entity Framework :</span><span class="sxs-lookup"><span data-stu-id="5fd17-106">There are three ways to approach Entity Framework:</span></span>

- <span data-ttu-id="5fd17-107">Base de données en premier : vous démarrez avec une base de données, et Entity Framework génère le code.</span><span class="sxs-lookup"><span data-stu-id="5fd17-107">Database-first: You start with a database, and Entity Framework generates the code.</span></span>
- <span data-ttu-id="5fd17-108">Modèle d’abord : vous démarrez avec un modèle visual et Entity Framework génère la base de données et le code.</span><span class="sxs-lookup"><span data-stu-id="5fd17-108">Model-first: You start with a visual model, and Entity Framework generates both the database and code.</span></span>
- <span data-ttu-id="5fd17-109">Code-first : vous démarrez avec le code et Entity Framework génère la base de données.</span><span class="sxs-lookup"><span data-stu-id="5fd17-109">Code-first: You start with code, and Entity Framework generates the database.</span></span>

<span data-ttu-id="5fd17-110">Nous utilisons l’approche code-first, donc nous allons commencer en définissant les objets de domaine en tant que POCOs (des objets brut-old CLR).</span><span class="sxs-lookup"><span data-stu-id="5fd17-110">We are using the code-first approach, so we start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="5fd17-111">Avec l’approche code-first, objets de domaine n’avez pas besoin tout code supplémentaire pour prendre en charge de la couche de base de données, telles que les transactions ou la persistance.</span><span class="sxs-lookup"><span data-stu-id="5fd17-111">With the code-first approach, domain objects don't need any extra code to support the database layer, such as transactions or persistence.</span></span> <span data-ttu-id="5fd17-112">(Plus précisément, ils n’avez pas besoin d’hériter la [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) classe.) Vous pouvez toujours utiliser des annotations de données pour contrôler comment Entity Framework crée le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="5fd17-112">(Specifically, they do not need to inherit from the [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) class.) You can still use data annotations to control how Entity Framework creates the database schema.</span></span>

<span data-ttu-id="5fd17-113">Étant donné que POCOs ne s’appliquent pas toutes les propriétés supplémentaires qui décrivent [de base de données d’état](https://msdn.microsoft.com/library/system.data.entitystate.aspx), ils peuvent facilement être sérialisés vers JSON ou XML.</span><span class="sxs-lookup"><span data-stu-id="5fd17-113">Because POCOs do not carry any extra properties that describe [database state](https://msdn.microsoft.com/library/system.data.entitystate.aspx), they can easily be serialized to JSON or XML.</span></span> <span data-ttu-id="5fd17-114">Toutefois, cela ne signifie pas vous devez toujours exposer vos modèles Entity Framework directement aux clients, comme nous le verrons plus tard dans le didacticiel.</span><span class="sxs-lookup"><span data-stu-id="5fd17-114">However, that does not mean you should always expose your Entity Framework models directly to clients, as we'll see later in the tutorial.</span></span>

<span data-ttu-id="5fd17-115">Nous allons créer les POCOs suivants :</span><span class="sxs-lookup"><span data-stu-id="5fd17-115">We will create the following POCOs:</span></span>

- <span data-ttu-id="5fd17-116">Produit</span><span class="sxs-lookup"><span data-stu-id="5fd17-116">Product</span></span>
- <span data-ttu-id="5fd17-117">Trier</span><span class="sxs-lookup"><span data-stu-id="5fd17-117">Order</span></span>
- <span data-ttu-id="5fd17-118">OrderDetail</span><span class="sxs-lookup"><span data-stu-id="5fd17-118">OrderDetail</span></span>

<span data-ttu-id="5fd17-119">Pour créer chaque classe, cliquez sur le dossier de modèles dans l’Explorateur de solutions.</span><span class="sxs-lookup"><span data-stu-id="5fd17-119">To create each class, right-click the Models folder in Solution Explorer.</span></span> <span data-ttu-id="5fd17-120">Dans le menu contextuel, sélectionnez **ajouter** , puis sélectionnez **classe.**</span><span class="sxs-lookup"><span data-stu-id="5fd17-120">From the context menu, select **Add** and then select **Class.**</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

<span data-ttu-id="5fd17-121">Ajouter un `Product` classe avec l’implémentation suivante :</span><span class="sxs-lookup"><span data-stu-id="5fd17-121">Add a `Product` class with the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

<span data-ttu-id="5fd17-122">Par convention, Entity Framework utilise le `Id` propriété comme clé primaire et le mappe à une colonne d’identité dans la table de base de données.</span><span class="sxs-lookup"><span data-stu-id="5fd17-122">By convention, Entity Framework uses the `Id` property as the primary key and maps it to an identity column in the database table.</span></span> <span data-ttu-id="5fd17-123">Lorsque vous créez un nouveau `Product` instance, vous ne définissez une valeur pour `Id`, car la base de données génère la valeur.</span><span class="sxs-lookup"><span data-stu-id="5fd17-123">When you create a new `Product` instance, you won't set a value for `Id`, because the database generates the value.</span></span>

<span data-ttu-id="5fd17-124">Le **ScaffoldColumn** attribut indique à ASP.NET MVC à ignorer le `Id` propriété lors de la génération d’un éditeur de formulaire.</span><span class="sxs-lookup"><span data-stu-id="5fd17-124">The **ScaffoldColumn** attribute tells ASP.NET MVC to skip the `Id` property when generating an editor form.</span></span> <span data-ttu-id="5fd17-125">Le **requis** attribut est utilisé pour valider le modèle.</span><span class="sxs-lookup"><span data-stu-id="5fd17-125">The **Required** attribute is used to validate the model.</span></span> <span data-ttu-id="5fd17-126">Il spécifie que le `Name` propriété doit être une chaîne non vide.</span><span class="sxs-lookup"><span data-stu-id="5fd17-126">It specifies that the `Name` property must be a non-empty string.</span></span>

<span data-ttu-id="5fd17-127">Ajouter la `Order` classe :</span><span class="sxs-lookup"><span data-stu-id="5fd17-127">Add the `Order` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

<span data-ttu-id="5fd17-128">Ajouter la `OrderDetail` classe :</span><span class="sxs-lookup"><span data-stu-id="5fd17-128">Add the `OrderDetail` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a><span data-ttu-id="5fd17-129">Relations de clés étrangères</span><span class="sxs-lookup"><span data-stu-id="5fd17-129">Foreign Key Relations</span></span>

<span data-ttu-id="5fd17-130">Une commande contient de nombreux détails de commande, et chaque détail de la commande fait référence à un seul produit.</span><span class="sxs-lookup"><span data-stu-id="5fd17-130">An order contains many order details, and each order detail refers to a single product.</span></span> <span data-ttu-id="5fd17-131">Pour représenter ces relations, la `OrderDetail` classe définit des propriétés nommées `OrderId` et `ProductId`.</span><span class="sxs-lookup"><span data-stu-id="5fd17-131">To represent these relations, the `OrderDetail` class defines properties named `OrderId` and `ProductId`.</span></span> <span data-ttu-id="5fd17-132">Entity Framework déduit que ces propriétés représentent des clés étrangères et ajoute les contraintes foreign key à la base de données.</span><span class="sxs-lookup"><span data-stu-id="5fd17-132">Entity Framework will infer that these properties represent foreign keys, and will add foreign-key constraints to the database.</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

<span data-ttu-id="5fd17-133">Le `Order` et `OrderDetail` classes incluent également des propriétés de « navigation », qui contiennent des références aux objets connexes.</span><span class="sxs-lookup"><span data-stu-id="5fd17-133">The `Order` and `OrderDetail` classes also include "navigation" properties, which contain references to the related objects.</span></span> <span data-ttu-id="5fd17-134">Étant donné une commande, vous pouvez accéder à des produits dans l’ordre en suivant les propriétés de navigation.</span><span class="sxs-lookup"><span data-stu-id="5fd17-134">Given an order, you can navigate to the products in the order by following the navigation properties.</span></span>

<span data-ttu-id="5fd17-135">Compilez le projet maintenant.</span><span class="sxs-lookup"><span data-stu-id="5fd17-135">Compile the project now.</span></span> <span data-ttu-id="5fd17-136">Entity Framework utilise la réflexion pour découvrir les propriétés des modèles, de sorte qu’elle nécessite un assembly compilé créer le schéma de base de données.</span><span class="sxs-lookup"><span data-stu-id="5fd17-136">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

## <a name="configure-the-media-type-formatters"></a><span data-ttu-id="5fd17-137">Configurer les formateurs de Type de média</span><span class="sxs-lookup"><span data-stu-id="5fd17-137">Configure the Media-Type Formatters</span></span>

<span data-ttu-id="5fd17-138">A [formateur de type de média](../../formats-and-model-binding/media-formatters.md) est un objet qui sérialise les données lors de l’API Web écrit le corps de réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="5fd17-138">A [media-type formatter](../../formats-and-model-binding/media-formatters.md) is an object that serializes your data when Web API writes the HTTP response body.</span></span> <span data-ttu-id="5fd17-139">Les formateurs intégrés prend en charge la sortie JSON et XML.</span><span class="sxs-lookup"><span data-stu-id="5fd17-139">The built-in formatters support JSON and XML output.</span></span> <span data-ttu-id="5fd17-140">Par défaut, les deux de ces formateurs de sérialiser tous les objets par valeur.</span><span class="sxs-lookup"><span data-stu-id="5fd17-140">By default, both of these formatters serialize all objects by value.</span></span>

<span data-ttu-id="5fd17-141">Sérialisation par valeur pose un problème si un graphique d’objet contient des références circulaires.</span><span class="sxs-lookup"><span data-stu-id="5fd17-141">Serialization by value creates a problem if an object graph contains circular references.</span></span> <span data-ttu-id="5fd17-142">C’est exactement le cas avec les `Order` et `OrderDetail` des classes, car chacun conserve une référence à l’autre.</span><span class="sxs-lookup"><span data-stu-id="5fd17-142">That's exactly the case with the `Order` and `OrderDetail` classes, because each holds a reference to the other.</span></span> <span data-ttu-id="5fd17-143">Le module de formatage est suivent les références, l’écriture de chaque objet par valeur et vous accédez en cercles.</span><span class="sxs-lookup"><span data-stu-id="5fd17-143">The formatter will follow the references, writing each object by value, and go in circles.</span></span> <span data-ttu-id="5fd17-144">Par conséquent, nous devons modifier le comportement par défaut.</span><span class="sxs-lookup"><span data-stu-id="5fd17-144">Therefore, we need to change the default behavior.</span></span>

<span data-ttu-id="5fd17-145">Dans l’Explorateur de solutions, développez l’application\_dossier Démarrer et d’ouvrir le fichier nommé WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="5fd17-145">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="5fd17-146">Ajoutez le code suivant à la classe `WebApiConfig` :</span><span class="sxs-lookup"><span data-stu-id="5fd17-146">Add the following code to the `WebApiConfig` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

<span data-ttu-id="5fd17-147">Ce code définit le formateur JSON pour conserver les références d’objet et supprime le formateur XML provenant du pipeline entièrement.</span><span class="sxs-lookup"><span data-stu-id="5fd17-147">This code sets the JSON formatter to preserve object references, and removes the XML formatter from the pipeline entirely.</span></span> <span data-ttu-id="5fd17-148">(Vous pouvez configurer le formateur XML pour conserver les références d’objet, mais il est un peu plus de travail, et nous avons besoin uniquement de JSON pour cette application.</span><span class="sxs-lookup"><span data-stu-id="5fd17-148">(You can configure the XML formatter to preserve object references, but it's a little more work, and we only need JSON for this application.</span></span> <span data-ttu-id="5fd17-149">Pour plus d’informations, consultez [la gestion des références d’objet circulaires](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span><span class="sxs-lookup"><span data-stu-id="5fd17-149">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="5fd17-150">[Précédent](using-web-api-with-entity-framework-part-1.md)
[Suivant](using-web-api-with-entity-framework-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="5fd17-150">[Previous](using-web-api-with-entity-framework-part-1.md)
[Next](using-web-api-with-entity-framework-part-3.md)</span></span>
