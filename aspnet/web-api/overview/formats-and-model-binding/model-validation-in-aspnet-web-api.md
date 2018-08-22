---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Validation de modèle dans l’API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/20/2012
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 611a6466e160387592df678b3b8556625ff8e234
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834607"
---
<a name="model-validation-in-aspnet-web-api"></a><span data-ttu-id="1267c-102">Validation de modèle dans l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1267c-102">Model Validation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="1267c-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1267c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="1267c-104">Lorsqu’un client envoie des données à votre API web, souvent voulez-vous valider les données avant tout traitement.</span><span class="sxs-lookup"><span data-stu-id="1267c-104">When a client sends data to your web API, often you want to validate the data before doing any processing.</span></span> <span data-ttu-id="1267c-105">Cet article montre comment annoter vos modèles, utilisez les annotations pour la validation de données et gérer les erreurs de validation dans votre API web.</span><span class="sxs-lookup"><span data-stu-id="1267c-105">This article shows how to annotate your models, use the annotations for data validation, and handle validation errors in your web API.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="1267c-106">Annotations de données</span><span class="sxs-lookup"><span data-stu-id="1267c-106">Data Annotations</span></span>

<span data-ttu-id="1267c-107">Dans l’API Web ASP.NET, vous pouvez utiliser des attributs à partir de la [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) espace de noms pour définir des règles de validation pour les propriétés de votre modèle.</span><span class="sxs-lookup"><span data-stu-id="1267c-107">In ASP.NET Web API, you can use attributes from the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace to set validation rules for properties on your model.</span></span> <span data-ttu-id="1267c-108">Prenez en compte le modèle suivant :</span><span class="sxs-lookup"><span data-stu-id="1267c-108">Consider the following model:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="1267c-109">Si vous avez utilisé la validation de modèle dans ASP.NET MVC, cela doit sembler familière.</span><span class="sxs-lookup"><span data-stu-id="1267c-109">If you have used model validation in ASP.NET MVC, this should look familiar.</span></span> <span data-ttu-id="1267c-110">Le **requis** attribut indique que le `Name` propriété ne doit pas être null.</span><span class="sxs-lookup"><span data-stu-id="1267c-110">The **Required** attribute says that the `Name` property must not be null.</span></span> <span data-ttu-id="1267c-111">Le **plage** attribut indique que `Weight` doit être comprise entre 0 et 999.</span><span class="sxs-lookup"><span data-stu-id="1267c-111">The **Range** attribute says that `Weight` must be between zero and 999.</span></span>

<span data-ttu-id="1267c-112">Supposons qu’un client envoie une requête POST avec la représentation JSON suivante :</span><span class="sxs-lookup"><span data-stu-id="1267c-112">Suppose that a client sends a POST request with the following JSON representation:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

<span data-ttu-id="1267c-113">Vous pouvez voir que le client n’inclut pas le `Name` propriété, qui est marquée comme requis.</span><span class="sxs-lookup"><span data-stu-id="1267c-113">You can see that the client did not include the `Name` property, which is marked as required.</span></span> <span data-ttu-id="1267c-114">Lorsque les API Web convertit du JSON en un `Product` instance, il valide le `Product` sur les attributs de validation.</span><span class="sxs-lookup"><span data-stu-id="1267c-114">When Web API converts the JSON into a `Product` instance, it validates the `Product` against the validation attributes.</span></span> <span data-ttu-id="1267c-115">Dans l’action de contrôleur, vous pouvez vérifier si le modèle est valid :</span><span class="sxs-lookup"><span data-stu-id="1267c-115">In your controller action, you can check whether the model is valid:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="1267c-116">Validation du modèle ne garantit pas que les données client sont sécurisées.</span><span class="sxs-lookup"><span data-stu-id="1267c-116">Model validation does not guarantee that client data is safe.</span></span> <span data-ttu-id="1267c-117">Une validation supplémentaire peut être nécessaire dans les autres couches de l’application.</span><span class="sxs-lookup"><span data-stu-id="1267c-117">Additional validation might be needed in other layers of the application.</span></span> <span data-ttu-id="1267c-118">(Par exemple, la couche de données peut appliquer des contraintes de clé étrangère.) Le didacticiel [à l’aide des API Web avec Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explore certains de ces problèmes.</span><span class="sxs-lookup"><span data-stu-id="1267c-118">(For example, the data layer might enforce foreign key constraints.) The tutorial [Using Web API with Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explores some of these issues.</span></span>

<span data-ttu-id="1267c-119">**« Validation incomplète »**: incomplète de validation se produit lorsque le client omet certaines propriétés.</span><span class="sxs-lookup"><span data-stu-id="1267c-119">**"Under-Posting"**: Under-posting happens when the client leaves out some properties.</span></span> <span data-ttu-id="1267c-120">Par exemple, supposons que le client envoie les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="1267c-120">For example, suppose the client sends the following:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

<span data-ttu-id="1267c-121">Ici, le client n’a pas spécifié les valeurs pour `Price` ou `Weight`.</span><span class="sxs-lookup"><span data-stu-id="1267c-121">Here, the client did not specify values for `Price` or `Weight`.</span></span> <span data-ttu-id="1267c-122">Le formateur JSON affecte la valeur par défaut de zéro pour les propriétés manquantes.</span><span class="sxs-lookup"><span data-stu-id="1267c-122">The JSON formatter assigns a default value of zero to the missing properties.</span></span>

![](model-validation-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="1267c-123">L’état du modèle est valide, car zéro est une valeur valide pour ces propriétés.</span><span class="sxs-lookup"><span data-stu-id="1267c-123">The model state is valid, because zero is a valid value for these properties.</span></span> <span data-ttu-id="1267c-124">S’il s’agit d’un problème dépend de votre scénario.</span><span class="sxs-lookup"><span data-stu-id="1267c-124">Whether this is a problem depends on your scenario.</span></span> <span data-ttu-id="1267c-125">Par exemple, dans une opération de mise à jour, vous souhaiterez peut-être faire la distinction entre « zéro » et « non définie ».</span><span class="sxs-lookup"><span data-stu-id="1267c-125">For example, in an update operation, you might want to distinguish between "zero" and "not set."</span></span> <span data-ttu-id="1267c-126">Pour forcer les clients à définir une valeur, vérifiez la propriété nullable et définissez le **requis** attribut :</span><span class="sxs-lookup"><span data-stu-id="1267c-126">To force clients to set a value, make the property nullable and set the **Required** attribute:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

<span data-ttu-id="1267c-127">**« Sur-publication »**: un client peut également envoyer *plus* données que prévu.</span><span class="sxs-lookup"><span data-stu-id="1267c-127">**"Over-Posting"**: A client can also send *more* data than you expected.</span></span> <span data-ttu-id="1267c-128">Exemple :</span><span class="sxs-lookup"><span data-stu-id="1267c-128">For example:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

<span data-ttu-id="1267c-129">Ici, le code JSON inclut une propriété (« Color ») qui n’existe pas dans le `Product` modèle.</span><span class="sxs-lookup"><span data-stu-id="1267c-129">Here, the JSON includes a property ("Color") that does not exist in the `Product` model.</span></span> <span data-ttu-id="1267c-130">Dans ce cas, le formateur JSON ignore simplement cette valeur.</span><span class="sxs-lookup"><span data-stu-id="1267c-130">In this case, the JSON formatter simply ignores this value.</span></span> <span data-ttu-id="1267c-131">(Le formateur XML fait de même.) Survalidation pose des problèmes si votre modèle comporte des propriétés qui vous destinées à être en lecture seule.</span><span class="sxs-lookup"><span data-stu-id="1267c-131">(The XML formatter does the same.) Over-posting causes problems if your model has properties that you intended to be read-only.</span></span> <span data-ttu-id="1267c-132">Exemple :</span><span class="sxs-lookup"><span data-stu-id="1267c-132">For example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="1267c-133">Les utilisateurs à mettre à jour le `IsAdmin` propriété et aux administrateurs de s’élever lui-même !</span><span class="sxs-lookup"><span data-stu-id="1267c-133">You don't want users to update the `IsAdmin` property and elevate themselves to administrators!</span></span> <span data-ttu-id="1267c-134">La stratégie la plus sûre consiste à utiliser une classe de modèle qui correspond exactement à ce que le client est autorisé à envoyer :</span><span class="sxs-lookup"><span data-stu-id="1267c-134">The safest strategy is to use a model class that exactly matches what the client is allowed to send:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="1267c-135">Billet de blog de Brad Wilson »[vs de Validation des entrées. Validation de modèle dans ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)« a des informations intéressantes de validation incomplète et survalidation.</span><span class="sxs-lookup"><span data-stu-id="1267c-135">Brad Wilson's blog post "[Input Validation vs. Model Validation in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" has a good discussion of under-posting and over-posting.</span></span> <span data-ttu-id="1267c-136">Bien que la publication est sur ASP.NET MVC 2, les problèmes sont toujours applicables à l’API Web.</span><span class="sxs-lookup"><span data-stu-id="1267c-136">Although the post is about ASP.NET MVC 2, the issues are still relevant to Web API.</span></span>


## <a name="handling-validation-errors"></a><span data-ttu-id="1267c-137">Gestion des erreurs de Validation</span><span class="sxs-lookup"><span data-stu-id="1267c-137">Handling Validation Errors</span></span>

<span data-ttu-id="1267c-138">API Web ne renvoie pas automatiquement de d’erreur au client lorsque la validation échoue.</span><span class="sxs-lookup"><span data-stu-id="1267c-138">Web API does not automatically return an error to the client when validation fails.</span></span> <span data-ttu-id="1267c-139">C’est à l’action du contrôleur pour vérifier l’état du modèle et de réagir de façon appropriée.</span><span class="sxs-lookup"><span data-stu-id="1267c-139">It is up to the controller action to check the model state and respond appropriately.</span></span>

<span data-ttu-id="1267c-140">Vous pouvez également créer un filtre d’action pour vérifier l’état du modèle avant l’appel de l’action du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="1267c-140">You can also create an action filter to check the model state before the controller action is invoked.</span></span> <span data-ttu-id="1267c-141">Le code suivant montre un exemple :</span><span class="sxs-lookup"><span data-stu-id="1267c-141">The following code shows an example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="1267c-142">Si l’échec de la validation du modèle, ce filtre retourne une réponse HTTP qui contient les erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="1267c-142">If model validation fails, this filter returns an HTTP response that contains the validation errors.</span></span> <span data-ttu-id="1267c-143">Dans ce cas, l’action du contrôleur n’est pas appelée.</span><span class="sxs-lookup"><span data-stu-id="1267c-143">In that case, the controller action is not invoked.</span></span>

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

<span data-ttu-id="1267c-144">Pour appliquer ce filtre à tous les contrôleurs d’API Web, ajoutez une instance du filtre à la **HttpConfiguration.Filters** collection lors de la configuration :</span><span class="sxs-lookup"><span data-stu-id="1267c-144">To apply this filter to all Web API controllers, add an instance of the filter to the **HttpConfiguration.Filters** collection during configuration:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="1267c-145">Une autre option consiste à définir le filtre en tant qu’attribut sur des contrôleurs ou des actions de contrôleur :</span><span class="sxs-lookup"><span data-stu-id="1267c-145">Another option is to set the filter as an attribute on individual controllers or controller actions:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
