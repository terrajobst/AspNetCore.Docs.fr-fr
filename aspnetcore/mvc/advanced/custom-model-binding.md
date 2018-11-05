---
title: Liaison de données personnalisée dans ASP.NET Core
author: ardalis
description: Découvrez comment la liaison de données permet aux actions du contrôleur de fonctionner directement avec des types de modèle dans ASP.NET Core.
ms.author: riande
ms.date: 04/10/2017
uid: mvc/advanced/custom-model-binding
ms.openlocfilehash: dc901aea3c20e7f2e955f39d923216de70ef015b
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090405"
---
# <a name="custom-model-binding-in-aspnet-core"></a><span data-ttu-id="b63f7-103">Liaison de données personnalisée dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b63f7-103">Custom Model Binding in ASP.NET Core</span></span>

<span data-ttu-id="b63f7-104">Par [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="b63f7-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="b63f7-105">La liaison de données permet aux actions du contrôleur de fonctionner directement avec des types de modèle (passés en tant qu’arguments de méthode), plutôt qu’avec des requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="b63f7-105">Model binding allows controller actions to work directly with model types (passed in as method arguments), rather than HTTP requests.</span></span> <span data-ttu-id="b63f7-106">Le mappage entre les données de requête entrantes et les modèles d’application est pris en charge par les classeurs de modèles.</span><span class="sxs-lookup"><span data-stu-id="b63f7-106">Mapping between incoming request data and application models is handled by model binders.</span></span> <span data-ttu-id="b63f7-107">Les développeurs peuvent étendre la fonctionnalité de liaison de données intégrée en implémentant des classeurs de modèles personnalisés (même si, en règle générale, vous n’avez pas besoin d’écrire votre propre fournisseur).</span><span class="sxs-lookup"><span data-stu-id="b63f7-107">Developers can extend the built-in model binding functionality by implementing custom model binders (though typically, you don't need to write your own provider).</span></span>

[<span data-ttu-id="b63f7-108">Afficher ou télécharger un exemple depuis GitHub</span><span class="sxs-lookup"><span data-stu-id="b63f7-108">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-model-binding/)

## <a name="default-model-binder-limitations"></a><span data-ttu-id="b63f7-109">Limitations du classeur de modèles par défaut</span><span class="sxs-lookup"><span data-stu-id="b63f7-109">Default model binder limitations</span></span>

<span data-ttu-id="b63f7-110">Les classeurs de modèles par défaut prennent en charge la plupart des types de données .NET Core usuels et doivent répondre aux besoins de la majorité des développeurs.</span><span class="sxs-lookup"><span data-stu-id="b63f7-110">The default model binders support most of the common .NET Core data types and should meet most developers' needs.</span></span> <span data-ttu-id="b63f7-111">Ils sont censés lier directement les entrées textuelles de la requête aux types de modèle.</span><span class="sxs-lookup"><span data-stu-id="b63f7-111">They expect to bind text-based input from the request directly to model types.</span></span> <span data-ttu-id="b63f7-112">Vous devrez peut-être transformer l’entrée avant de la lier.</span><span class="sxs-lookup"><span data-stu-id="b63f7-112">You might need to transform the input prior to binding it.</span></span> <span data-ttu-id="b63f7-113">Par exemple, quand vous avez une clé qui peut être utilisée pour rechercher des données de modèle.</span><span class="sxs-lookup"><span data-stu-id="b63f7-113">For example, when you have a key that can be used to look up model data.</span></span> <span data-ttu-id="b63f7-114">Vous pouvez utiliser un classeur de modèles personnalisé pour récupérer (fetch) les données en fonction de la clé.</span><span class="sxs-lookup"><span data-stu-id="b63f7-114">You can use a custom model binder to fetch data based on the key.</span></span>

## <a name="model-binding-review"></a><span data-ttu-id="b63f7-115">Vérification de la liaison de données</span><span class="sxs-lookup"><span data-stu-id="b63f7-115">Model binding review</span></span>

<span data-ttu-id="b63f7-116">La liaison de données utilise des définitions spécifiques pour les types sur lesquels elle opère.</span><span class="sxs-lookup"><span data-stu-id="b63f7-116">Model binding uses specific definitions for the types it operates on.</span></span> <span data-ttu-id="b63f7-117">Un *type simple* est converti à partir d’une seule chaîne dans l’entrée.</span><span class="sxs-lookup"><span data-stu-id="b63f7-117">A *simple type* is converted from a single string in the input.</span></span> <span data-ttu-id="b63f7-118">Un *type complexe* est converti à partir de plusieurs valeurs d’entrée.</span><span class="sxs-lookup"><span data-stu-id="b63f7-118">A *complex type* is converted from multiple input values.</span></span> <span data-ttu-id="b63f7-119">Le framework détermine la différence en fonction de l’existence de `TypeConverter`.</span><span class="sxs-lookup"><span data-stu-id="b63f7-119">The framework determines the difference based on the existence of a `TypeConverter`.</span></span> <span data-ttu-id="b63f7-120">Nous vous recommandons de créer un convertisseur de type si vous disposez d’un mappage `string` -> `SomeType` simple qui ne nécessite pas de ressources externes.</span><span class="sxs-lookup"><span data-stu-id="b63f7-120">We recommended you create a type converter if you have a simple `string` -> `SomeType` mapping that doesn't require external resources.</span></span>

<span data-ttu-id="b63f7-121">Avant de créer votre propre classeur de modèles personnalisé, vérifiez la façon dont les classeurs de modèles existants sont implémentés.</span><span class="sxs-lookup"><span data-stu-id="b63f7-121">Before creating your own custom model binder, it's worth reviewing how existing model binders are implemented.</span></span> <span data-ttu-id="b63f7-122">Prenons l’exemple de [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder), qui permet de convertir des chaînes encodées au format base64 en tableaux d’octets.</span><span class="sxs-lookup"><span data-stu-id="b63f7-122">Consider the [ByteArrayModelBinder](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinder) which can be used to convert base64-encoded strings into byte arrays.</span></span> <span data-ttu-id="b63f7-123">Les tableaux d’octets sont souvent stockés sous forme de fichiers ou de champs BLOB de base de données.</span><span class="sxs-lookup"><span data-stu-id="b63f7-123">The byte arrays are often stored as files or database BLOB fields.</span></span>

### <a name="working-with-the-bytearraymodelbinder"></a><span data-ttu-id="b63f7-124">Utilisation de ByteArrayModelBinder</span><span class="sxs-lookup"><span data-stu-id="b63f7-124">Working with the ByteArrayModelBinder</span></span>

<span data-ttu-id="b63f7-125">Les chaînes encodées au format Base64 peuvent être utilisées pour représenter des données binaires.</span><span class="sxs-lookup"><span data-stu-id="b63f7-125">Base64-encoded strings can be used to represent binary data.</span></span> <span data-ttu-id="b63f7-126">Par exemple, l’image suivante peut être encodée sous forme de chaîne.</span><span class="sxs-lookup"><span data-stu-id="b63f7-126">For example, the following image can be encoded as a string.</span></span>

<span data-ttu-id="b63f7-127">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span><span class="sxs-lookup"><span data-stu-id="b63f7-127">![dotnet bot](custom-model-binding/images/bot.png "dotnet bot")</span></span>

<span data-ttu-id="b63f7-128">Une petite partie de la chaîne encodée est affichée dans l’image suivante :</span><span class="sxs-lookup"><span data-stu-id="b63f7-128">A small portion of the encoded string is shown in the following image:</span></span>

<span data-ttu-id="b63f7-129">![dotnet bot encodé](custom-model-binding/images/encoded-bot.png "dotnet bot encodé")</span><span class="sxs-lookup"><span data-stu-id="b63f7-129">![dotnet bot encoded](custom-model-binding/images/encoded-bot.png "dotnet bot encoded")</span></span>

<span data-ttu-id="b63f7-130">Suivez les instructions du [fichier README de l’exemple](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) pour convertir la chaîne encodée au format base64 en fichier.</span><span class="sxs-lookup"><span data-stu-id="b63f7-130">Follow the instructions in the [sample's README](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/advanced/custom-model-binding/sample/CustomModelBindingSample/README.md) to convert the base64-encoded string into a file.</span></span>

<span data-ttu-id="b63f7-131">ASP.NET Core MVC peut accepter une chaîne encodée au format base64 et utiliser `ByteArrayModelBinder` pour la convertir en tableau d’octets.</span><span class="sxs-lookup"><span data-stu-id="b63f7-131">ASP.NET Core MVC can take a base64-encoded string and use a `ByteArrayModelBinder` to convert it into a byte array.</span></span> <span data-ttu-id="b63f7-132">Le [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) qui implémente [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) mappe les arguments `byte[]` à `ByteArrayModelBinder` :</span><span class="sxs-lookup"><span data-stu-id="b63f7-132">The [ByteArrayModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.binders.bytearraymodelbinderprovider) which implements [IModelBinderProvider](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.imodelbinderprovider) maps `byte[]` arguments to `ByteArrayModelBinder`:</span></span>

```csharp
public IModelBinder GetBinder(ModelBinderProviderContext context)
{
    if (context == null)
    {
        throw new ArgumentNullException(nameof(context));
    }

    if (context.Metadata.ModelType == typeof(byte[]))
    {
        return new ByteArrayModelBinder();
    }

    return null;
}
```

<span data-ttu-id="b63f7-133">Quand vous créez votre propre classeur de modèles personnalisé, vous pouvez implémenter votre propre type `IModelBinderProvider`, ou utiliser [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span><span class="sxs-lookup"><span data-stu-id="b63f7-133">When creating your own custom model binder, you can implement your own `IModelBinderProvider` type, or use the [ModelBinderAttribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinderattribute).</span></span>

<span data-ttu-id="b63f7-134">L’exemple suivant montre comment utiliser `ByteArrayModelBinder` pour convertir une chaîne encodée au format base64 en `byte[]`, et comment enregistrer le résultat dans un fichier :</span><span class="sxs-lookup"><span data-stu-id="b63f7-134">The following example shows how to use `ByteArrayModelBinder` to convert a base64-encoded string to a `byte[]` and save the result to a file:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post1&highlight=3)]

<span data-ttu-id="b63f7-135">Vous pouvez envoyer (POST) une chaîne encodée au format base64 à cette méthode d’API à l’aide d’un outil tel que [Postman](https://www.getpostman.com/) :</span><span class="sxs-lookup"><span data-stu-id="b63f7-135">You can POST a base64-encoded string to this api method using a tool like [Postman](https://www.getpostman.com/):</span></span>

<span data-ttu-id="b63f7-136">![postman](custom-model-binding/images/postman.png "postman")</span><span class="sxs-lookup"><span data-stu-id="b63f7-136">![postman](custom-model-binding/images/postman.png "postman")</span></span>

<span data-ttu-id="b63f7-137">Tant que le classeur peut lier les données de requête à des propriétés ou des arguments nommés de manière appropriée, la liaison de données s’effectue correctement.</span><span class="sxs-lookup"><span data-stu-id="b63f7-137">As long as the binder can bind request data to appropriately named properties or arguments, model binding will succeed.</span></span> <span data-ttu-id="b63f7-138">L’exemple suivant montre comment utiliser `ByteArrayModelBinder` avec un modèle de vue :</span><span class="sxs-lookup"><span data-stu-id="b63f7-138">The following example shows how to use `ByteArrayModelBinder` with a view model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/ImageController.cs?name=post2&highlight=2)]

## <a name="custom-model-binder-sample"></a><span data-ttu-id="b63f7-139">Exemple de classeur de modèles personnalisé</span><span class="sxs-lookup"><span data-stu-id="b63f7-139">Custom model binder sample</span></span>

<span data-ttu-id="b63f7-140">Dans cette section, nous allons implémenter un classeur de modèles personnalisé qui :</span><span class="sxs-lookup"><span data-stu-id="b63f7-140">In this section we'll implement a custom model binder that:</span></span>

- <span data-ttu-id="b63f7-141">Convertit les données de requête entrantes en arguments clés fortement typés.</span><span class="sxs-lookup"><span data-stu-id="b63f7-141">Converts incoming request data into strongly typed key arguments.</span></span>
- <span data-ttu-id="b63f7-142">Utilise Entity Framework Core pour récupérer (fetch) l’entité associée.</span><span class="sxs-lookup"><span data-stu-id="b63f7-142">Uses Entity Framework Core to fetch the associated entity.</span></span>
- <span data-ttu-id="b63f7-143">Passe l’entité associée en tant qu’argument à la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="b63f7-143">Passes the associated entity as an argument to the action method.</span></span>

<span data-ttu-id="b63f7-144">L’exemple suivant utilise l’attribut `ModelBinder` pour le modèle `Author` :</span><span class="sxs-lookup"><span data-stu-id="b63f7-144">The following sample uses the `ModelBinder` attribute on the `Author` model:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Data/Author.cs?highlight=10)]

<span data-ttu-id="b63f7-145">Dans le code précédent, l’attribut `ModelBinder` spécifie le type de `IModelBinder` à utiliser pour lier les paramètres d’action de `Author`.</span><span class="sxs-lookup"><span data-stu-id="b63f7-145">In the preceding code, the `ModelBinder` attribute specifies the type of `IModelBinder` that should be used to bind `Author` action parameters.</span></span> 

<span data-ttu-id="b63f7-146">`AuthorEntityBinder` est utilisé pour lier un paramètre `Author` en récupérant (fetch) l’entité à partir d’une source de données via Entity Framework Core et `authorId` :</span><span class="sxs-lookup"><span data-stu-id="b63f7-146">The `AuthorEntityBinder` is used to bind an `Author` parameter by fetching the entity from a data source using Entity Framework Core and an `authorId`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinder.cs?name=demo)]

<span data-ttu-id="b63f7-147">Le code suivant montre comment utiliser `AuthorEntityBinder` dans une méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="b63f7-147">The following code shows how to use the `AuthorEntityBinder` in an action method:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo2&highlight=2)]

<span data-ttu-id="b63f7-148">L’attribut `ModelBinder` permet d’appliquer `AuthorEntityBinder` aux paramètres qui n’utilisent pas les conventions par défaut :</span><span class="sxs-lookup"><span data-stu-id="b63f7-148">The `ModelBinder` attribute can be used to apply the `AuthorEntityBinder` to parameters that don't use default conventions:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Controllers/BoundAuthorsController.cs?name=demo1&highlight=2)]

<span data-ttu-id="b63f7-149">Dans cet exemple, comme le nom de l’argument n’est pas le `authorId` par défaut, il est spécifié dans le paramètre à l’aide de l’attribut `ModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="b63f7-149">In this example, since the name of the argument isn't the default `authorId`, it's specified on the parameter using the `ModelBinder` attribute.</span></span> <span data-ttu-id="b63f7-150">Notez que le contrôleur et la méthode d’action sont simplifiés par rapport à la recherche de l’entité dans la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="b63f7-150">Note that both the controller and action method are simplified compared to looking up the entity in the action method.</span></span> <span data-ttu-id="b63f7-151">La logique permettant de récupérer (fetch) l’auteur à l’aide d’Entity Framework Core est déplacée vers le classeur de modèles.</span><span class="sxs-lookup"><span data-stu-id="b63f7-151">The logic to fetch the author using Entity Framework Core is moved to the model binder.</span></span> <span data-ttu-id="b63f7-152">Cela peut représenter une simplification considérable quand vous avez plusieurs méthodes qui sont liées au modèle `Author`. Cela peut également vous aider à suivre le [principe DRY](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="b63f7-152">This can be a considerable simplification when you have several methods that bind to the `Author` model, and can help you to follow the [DRY principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="b63f7-153">Vous pouvez appliquer l’attribut `ModelBinder` à des propriétés de modèle individuelles (par exemple viewmodel) ou à des paramètres de méthode d’action afin de spécifier un classeur de modèles ou un nom de modèle particulier pour ce type ou cette action uniquement.</span><span class="sxs-lookup"><span data-stu-id="b63f7-153">You can apply the `ModelBinder` attribute to individual model properties (such as on a viewmodel) or to action method parameters to specify a certain model binder or model name for just that type or action.</span></span>

### <a name="implementing-a-modelbinderprovider"></a><span data-ttu-id="b63f7-154">Implémentation de ModelBinderProvider</span><span class="sxs-lookup"><span data-stu-id="b63f7-154">Implementing a ModelBinderProvider</span></span>

<span data-ttu-id="b63f7-155">Au lieu d’appliquer un attribut, vous pouvez implémenter `IModelBinderProvider`.</span><span class="sxs-lookup"><span data-stu-id="b63f7-155">Instead of applying an attribute, you can implement `IModelBinderProvider`.</span></span> <span data-ttu-id="b63f7-156">C’est ainsi que les classeurs de framework intégrés sont implémentés.</span><span class="sxs-lookup"><span data-stu-id="b63f7-156">This is how the built-in framework binders are implemented.</span></span> <span data-ttu-id="b63f7-157">Quand vous spécifiez le type sur lequel votre classeur opère, vous spécifiez le type d’argument qu’il produit, et **non** l’entrée que votre classeur accepte.</span><span class="sxs-lookup"><span data-stu-id="b63f7-157">When you specify the type your binder operates on, you specify the type of argument it produces, **not** the input your binder accepts.</span></span> <span data-ttu-id="b63f7-158">Le fournisseur de classeurs suivant fonctionne avec `AuthorEntityBinder`.</span><span class="sxs-lookup"><span data-stu-id="b63f7-158">The following binder provider works with the `AuthorEntityBinder`.</span></span> <span data-ttu-id="b63f7-159">Quand il est ajouté à la collection de fournisseurs de MVC, vous n’avez pas besoin d’utiliser l’attribut `ModelBinder` sur `Author` ou les paramètres typés de `Author`.</span><span class="sxs-lookup"><span data-stu-id="b63f7-159">When it's added to MVC's collection of providers, you don't need to use the `ModelBinder` attribute on `Author` or `Author` typed parameters.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Binders/AuthorEntityBinderProvider.cs?highlight=17-20)]

> <span data-ttu-id="b63f7-160">Remarque : Le code précédent retourne `BinderTypeModelBinder`.</span><span class="sxs-lookup"><span data-stu-id="b63f7-160">Note: The preceding code returns a `BinderTypeModelBinder`.</span></span> <span data-ttu-id="b63f7-161">`BinderTypeModelBinder` sert de fabrique pour les classeurs de modèles et permet l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="b63f7-161">`BinderTypeModelBinder` acts as a factory for model binders and provides dependency injection (DI).</span></span> <span data-ttu-id="b63f7-162">`AuthorEntityBinder` a besoin de l’injection de dépendances pour accéder à EF Core.</span><span class="sxs-lookup"><span data-stu-id="b63f7-162">The `AuthorEntityBinder` requires DI to access EF Core.</span></span> <span data-ttu-id="b63f7-163">Utilisez `BinderTypeModelBinder`, si votre classeur de modèles nécessite des services liés à l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="b63f7-163">Use `BinderTypeModelBinder` if your model binder requires services from DI.</span></span>

<span data-ttu-id="b63f7-164">Pour utiliser un fournisseur de classeurs de modèles personnalisé, ajoutez-le à `ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="b63f7-164">To use a custom model binder provider, add it in `ConfigureServices`:</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

<span data-ttu-id="b63f7-165">Durant l’évaluation des classeurs de modèles, la collection de fournisseurs est examinée dans un certain ordre.</span><span class="sxs-lookup"><span data-stu-id="b63f7-165">When evaluating model binders, the collection of providers is examined in order.</span></span> <span data-ttu-id="b63f7-166">Le premier fournisseur qui retourne un classeur est utilisé.</span><span class="sxs-lookup"><span data-stu-id="b63f7-166">The first provider that returns a binder is used.</span></span>

<span data-ttu-id="b63f7-167">L’image suivante illustre les classeurs de modèles par défaut du débogueur.</span><span class="sxs-lookup"><span data-stu-id="b63f7-167">The following image shows the default model binders from the debugger.</span></span>

<span data-ttu-id="b63f7-168">![classeurs de modèles par défaut](custom-model-binding/images/default-model-binders.png "classeurs de modèles par défaut")</span><span class="sxs-lookup"><span data-stu-id="b63f7-168">![default model binders](custom-model-binding/images/default-model-binders.png "default model binders")</span></span>

<span data-ttu-id="b63f7-169">L’ajout de votre fournisseur à la fin de la collection peut entraîner l’appel d’un classeur de modèles intégré avant votre classeur personnalisé.</span><span class="sxs-lookup"><span data-stu-id="b63f7-169">Adding your provider to the end of the collection may result in a built-in model binder being called before your custom binder has a chance.</span></span> <span data-ttu-id="b63f7-170">Dans cet exemple, le fournisseur personnalisé est ajouté au début de la collection afin qu’il soit utilisé pour les arguments d’action `Author`.</span><span class="sxs-lookup"><span data-stu-id="b63f7-170">In this example, the custom provider is added to the beginning of the collection to ensure it's used for `Author` action arguments.</span></span>

[!code-csharp[](custom-model-binding/sample/CustomModelBindingSample/Startup.cs?name=callout&highlight=5-9)]

## <a name="recommendations-and-best-practices"></a><span data-ttu-id="b63f7-171">Recommandations et bonnes pratiques</span><span class="sxs-lookup"><span data-stu-id="b63f7-171">Recommendations and best practices</span></span>

<span data-ttu-id="b63f7-172">Les classeurs de modèles personnalisés :</span><span class="sxs-lookup"><span data-stu-id="b63f7-172">Custom model binders:</span></span>
- <span data-ttu-id="b63f7-173">Ne doivent pas tenter de définir des codes d’état ou de retourner des résultats (par exemple, 404 Introuvable).</span><span class="sxs-lookup"><span data-stu-id="b63f7-173">Shouldn't attempt to set status codes or return results (for example, 404 Not Found).</span></span> <span data-ttu-id="b63f7-174">En cas d’échec de la liaison de données, un [filtre d’action](xref:mvc/controllers/filters) ou une logique située dans la méthode d’action elle-même doit prendre en charge l’erreur.</span><span class="sxs-lookup"><span data-stu-id="b63f7-174">If model binding fails, an [action filter](xref:mvc/controllers/filters) or logic within the action method itself should handle the failure.</span></span>
- <span data-ttu-id="b63f7-175">Sont surtout utiles pour éliminer le code répétitif et les problèmes transversaux des méthodes d’action.</span><span class="sxs-lookup"><span data-stu-id="b63f7-175">Are most useful for eliminating repetitive code and cross-cutting concerns from action methods.</span></span>
- <span data-ttu-id="b63f7-176">Ne doivent pas être utilisés pour convertir une chaîne en type personnalisé. En règle générale, [`TypeConverter`](/dotnet/api/system.componentmodel.typeconverter) est une meilleure option.</span><span class="sxs-lookup"><span data-stu-id="b63f7-176">Typically shouldn't be used to convert a string into a custom type, a [`TypeConverter`](/dotnet/api/system.componentmodel.typeconverter) is usually a better option.</span></span>
