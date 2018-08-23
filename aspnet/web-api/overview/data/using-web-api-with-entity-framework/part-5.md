---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Créer des objets de transfert de données (DTO) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: 95075dd748f0fe4eb6d1c52d6bfe4a4576653b4c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833419"
---
<a name="create-data-transfer-objects-dtos"></a><span data-ttu-id="82612-102">Créer des objets de transfert de données (DTO)</span><span class="sxs-lookup"><span data-stu-id="82612-102">Create Data Transfer Objects (DTOs)</span></span>
====================
<span data-ttu-id="82612-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="82612-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="82612-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="82612-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="82612-105">Droit à présent, notre API web expose les entités de base de données au client.</span><span class="sxs-lookup"><span data-stu-id="82612-105">Right now, our web API exposes the database entities to the client.</span></span> <span data-ttu-id="82612-106">Le client reçoit des données est directement mappé à votre base de données.</span><span class="sxs-lookup"><span data-stu-id="82612-106">The client receives data that maps directly to your database tables.</span></span> <span data-ttu-id="82612-107">Toutefois, qui n’est pas toujours une bonne idée.</span><span class="sxs-lookup"><span data-stu-id="82612-107">However, that's not always a good idea.</span></span> <span data-ttu-id="82612-108">Parfois, vous souhaitez modifier la forme des données que vous envoyez au client.</span><span class="sxs-lookup"><span data-stu-id="82612-108">Sometimes you want to change the shape of the data that you send to client.</span></span> <span data-ttu-id="82612-109">Vous pouvez, par exemple, décider d'effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="82612-109">For example, you might want to:</span></span>

- <span data-ttu-id="82612-110">Supprimez les références circulaires (voir la section précédente).</span><span class="sxs-lookup"><span data-stu-id="82612-110">Remove circular references (see previous section).</span></span>
- <span data-ttu-id="82612-111">Masquer des propriétés particulières qui les clients ne sont pas censé pour afficher.</span><span class="sxs-lookup"><span data-stu-id="82612-111">Hide particular properties that clients are not supposed to view.</span></span>
- <span data-ttu-id="82612-112">Omettre certaines propriétés afin de réduire la taille de charge utile.</span><span class="sxs-lookup"><span data-stu-id="82612-112">Omit some properties in order to reduce payload size.</span></span>
- <span data-ttu-id="82612-113">Aplatir les graphiques d’objets qui contiennent des objets imbriqués, pour les rendre plus pratique pour les clients.</span><span class="sxs-lookup"><span data-stu-id="82612-113">Flatten object graphs that contain nested objects, to make them more convenient for clients.</span></span>
- <span data-ttu-id="82612-114">Évitez « les vulnérabilités de survalidation ».</span><span class="sxs-lookup"><span data-stu-id="82612-114">Avoid "over-posting" vulnerabilities.</span></span> <span data-ttu-id="82612-115">(Consultez [Validation du modèle](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) pour une présentation de survalidation.)</span><span class="sxs-lookup"><span data-stu-id="82612-115">(See [Model Validation](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) for a discussion of over-posting.)</span></span>
- <span data-ttu-id="82612-116">Découpler votre couche de service à partir de votre couche de base de données.</span><span class="sxs-lookup"><span data-stu-id="82612-116">Decouple your service layer from your database layer.</span></span>

<span data-ttu-id="82612-117">Pour ce faire, vous pouvez définir un *objet de transfert de données* (DTO).</span><span class="sxs-lookup"><span data-stu-id="82612-117">To accomplish this, you can define a *data transfer object* (DTO).</span></span> <span data-ttu-id="82612-118">Un objet DTO est un objet qui définit comment les données seront être envoyées sur le réseau.</span><span class="sxs-lookup"><span data-stu-id="82612-118">A DTO is an object that defines how the data will be sent over the network.</span></span> <span data-ttu-id="82612-119">Nous allons voir comment cela fonctionne avec l’entité de livre.</span><span class="sxs-lookup"><span data-stu-id="82612-119">Let's see how that works with the Book entity.</span></span> <span data-ttu-id="82612-120">Dans le dossier Models, ajoutez deux classes DTO :</span><span class="sxs-lookup"><span data-stu-id="82612-120">In the Models folder, add two DTO classes:</span></span>

[!code-csharp[Main](part-5/samples/sample1.cs)]

<span data-ttu-id="82612-121">Le `BookDetailDTO` classe inclut toutes les propriétés du modèle de livre, à ceci près que `AuthorName` est une chaîne qui contienne le nom de l’auteur.</span><span class="sxs-lookup"><span data-stu-id="82612-121">The `BookDetailDTO` class includes all of the properties from the Book model, except that `AuthorName` is a string that will hold the author name.</span></span> <span data-ttu-id="82612-122">Le `BookDTO` classe contient un sous-ensemble de propriétés à partir de `BookDetailDTO`.</span><span class="sxs-lookup"><span data-stu-id="82612-122">The `BookDTO` class contains a subset of properties from `BookDetailDTO`.</span></span>

<span data-ttu-id="82612-123">Ensuite, remplacez les deux méthodes GET dans le `BooksController` (classe), avec les versions qui retournent des objets DTO.</span><span class="sxs-lookup"><span data-stu-id="82612-123">Next, replace the two GET methods in the `BooksController` class, with versions that return DTOs.</span></span> <span data-ttu-id="82612-124">Nous allons utiliser LINQ **sélectionnez** instruction pour convertir les entités livre en objets DTO.</span><span class="sxs-lookup"><span data-stu-id="82612-124">We'll use the LINQ **Select** statement to convert from Book entities into DTOs.</span></span>

[!code-csharp[Main](part-5/samples/sample2.cs)]

<span data-ttu-id="82612-125">Voici le code SQL généré par le nouveau `GetBooks` (méthode).</span><span class="sxs-lookup"><span data-stu-id="82612-125">Here is the SQL generated by the new `GetBooks` method.</span></span> <span data-ttu-id="82612-126">Vous pouvez voir qu’Entity Framework traduit la LINQ **sélectionnez** dans une instruction SQL SELECT.</span><span class="sxs-lookup"><span data-stu-id="82612-126">You can see that EF translates the LINQ **Select** into a SQL SELECT statement.</span></span>

[!code-sql[Main](part-5/samples/sample3.sql)]

<span data-ttu-id="82612-127">Enfin, modifiez le `PostBook` méthode pour retourner un objet DTO.</span><span class="sxs-lookup"><span data-stu-id="82612-127">Finally, modify the `PostBook` method to return a DTO.</span></span>

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="82612-128">Dans ce didacticiel, nous convertissons à DTO manuellement dans le code.</span><span class="sxs-lookup"><span data-stu-id="82612-128">In this tutorial, we're converting to DTOs manually in code.</span></span> <span data-ttu-id="82612-129">Une autre option consiste à utiliser une bibliothèque comme [AutoMapper](http://automapper.org/) qui gère la conversion automatiquement.</span><span class="sxs-lookup"><span data-stu-id="82612-129">Another option is to use a library like [AutoMapper](http://automapper.org/) that handles the conversion automatically.</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="82612-130">[Précédent](part-4.md)
> [Suivant](part-6.md)</span><span class="sxs-lookup"><span data-stu-id="82612-130">[Previous](part-4.md)
[Next](part-6.md)</span></span>
