---
uid: web-api/overview/error-handling/exception-handling
title: Gestion des exceptions dans l’API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2012
ms.topic: article
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: ac01a4f35cde99a1f8ec699e6d31bf597f1d334e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400818"
---
<a name="exception-handling-in-aspnet-web-api"></a><span data-ttu-id="c7895-102">Gestion des exceptions dans l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c7895-102">Exception Handling in ASP.NET Web API</span></span>
====================
<span data-ttu-id="c7895-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c7895-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c7895-104">Cet article décrit l’erreur et gestion des exceptions dans l’API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c7895-104">This article describes error and exception handling in ASP.NET Web API.</span></span>

- [<span data-ttu-id="c7895-105">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="c7895-105">HttpResponseException</span></span>](#httpresponserexception)
- [<span data-ttu-id="c7895-106">Filtres d’exception</span><span class="sxs-lookup"><span data-stu-id="c7895-106">Exception Filters</span></span>](#exception_filters)
- [<span data-ttu-id="c7895-107">L’inscription des filtres d’Exception</span><span class="sxs-lookup"><span data-stu-id="c7895-107">Registering Exception Filters</span></span>](#registering_exception_filters)
- [<span data-ttu-id="c7895-108">Erreur HTTP</span><span class="sxs-lookup"><span data-stu-id="c7895-108">HttpError</span></span>](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a><span data-ttu-id="c7895-109">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="c7895-109">HttpResponseException</span></span>

<span data-ttu-id="c7895-110">Que se passe-t-il si un contrôleur Web API lève une exception non interceptée ?</span><span class="sxs-lookup"><span data-stu-id="c7895-110">What happens if a Web API controller throws an uncaught exception?</span></span> <span data-ttu-id="c7895-111">Par défaut, la plupart des exceptions sont traduites en une réponse HTTP avec le code d’état 500, erreur interne du serveur.</span><span class="sxs-lookup"><span data-stu-id="c7895-111">By default, most exceptions are translated into an HTTP response with status code 500, Internal Server Error.</span></span>

<span data-ttu-id="c7895-112">Le **HttpResponseException** type est un cas spécial.</span><span class="sxs-lookup"><span data-stu-id="c7895-112">The **HttpResponseException** type is a special case.</span></span> <span data-ttu-id="c7895-113">Cette exception retourne n’importe quel code d’état HTTP que vous spécifiez dans le constructeur d’exception.</span><span class="sxs-lookup"><span data-stu-id="c7895-113">This exception returns any HTTP status code that you specify in the exception constructor.</span></span> <span data-ttu-id="c7895-114">Par exemple, la méthode suivante retourne 404, non trouvé, si le *id* paramètre n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="c7895-114">For example, the following method returns 404, Not Found, if the *id* parameter is not valid.</span></span>

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

<span data-ttu-id="c7895-115">Pour mieux contrôler la réponse, vous pouvez également construire le message de réponse entière et l’inclure avec le **HttpResponseException :**</span><span class="sxs-lookup"><span data-stu-id="c7895-115">For more control over the response, you can also construct the entire response message and include it with the **HttpResponseException:**</span></span> 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a><span data-ttu-id="c7895-116">Filtres d’exception</span><span class="sxs-lookup"><span data-stu-id="c7895-116">Exception Filters</span></span>

<span data-ttu-id="c7895-117">Vous pouvez personnaliser la façon dont les API Web gère les exceptions en écrivant un *filtre d’exception*.</span><span class="sxs-lookup"><span data-stu-id="c7895-117">You can customize how Web API handles exceptions by writing an *exception filter*.</span></span> <span data-ttu-id="c7895-118">Un filtre d’exception est exécuté quand une méthode de contrôleur lève une exception non gérée qui est *pas* un **HttpResponseException** exception.</span><span class="sxs-lookup"><span data-stu-id="c7895-118">An exception filter is executed when a controller method throws any unhandled exception that is *not* an **HttpResponseException** exception.</span></span> <span data-ttu-id="c7895-119">Le **HttpResponseException** type est un cas spécial, car il est spécifiquement conçu pour renvoyer une réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="c7895-119">The **HttpResponseException** type is a special case, because it is designed specifically for returning an HTTP response.</span></span>

<span data-ttu-id="c7895-120">Filtres d’exception implémentent la **System.Web.Http.Filters.IExceptionFilter** interface.</span><span class="sxs-lookup"><span data-stu-id="c7895-120">Exception filters implement the **System.Web.Http.Filters.IExceptionFilter** interface.</span></span> <span data-ttu-id="c7895-121">La méthode la plus simple pour écrire un filtre d’exception consiste à dériver à partir de la **System.Web.Http.Filters.ExceptionFilterAttribute** classe et substituer les **OnException** (méthode).</span><span class="sxs-lookup"><span data-stu-id="c7895-121">The simplest way to write an exception filter is to derive from the **System.Web.Http.Filters.ExceptionFilterAttribute** class and override the **OnException** method.</span></span>

> [!NOTE]
> <span data-ttu-id="c7895-122">Filtres d’exception dans l’API Web ASP.NET sont semblables à celles dans ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c7895-122">Exception filters in ASP.NET Web API are similar to those in ASP.NET MVC.</span></span> <span data-ttu-id="c7895-123">Toutefois, ils sont déclarés dans un espace de noms distinct et une fonction séparément.</span><span class="sxs-lookup"><span data-stu-id="c7895-123">However, they are declared in a separate namespace and function separately.</span></span> <span data-ttu-id="c7895-124">En particulier, le **HandleErrorAttribute** classe utilisée dans MVC ne gère pas les exceptions levées par les contrôleurs d’API Web.</span><span class="sxs-lookup"><span data-stu-id="c7895-124">In particular, the **HandleErrorAttribute** class used in MVC does not handle exceptions thrown by Web API controllers.</span></span>


<span data-ttu-id="c7895-125">Voici un filtre qui convertit **NotImplementedException** exceptions dans l’état HTTP 501, non implémenté de code :</span><span class="sxs-lookup"><span data-stu-id="c7895-125">Here is a filter that converts **NotImplementedException** exceptions into HTTP status code 501, Not Implemented:</span></span>

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

<span data-ttu-id="c7895-126">Le **réponse** propriété de la **HttpActionExecutedContext** objet contient le message de réponse HTTP qui sera envoyé au client.</span><span class="sxs-lookup"><span data-stu-id="c7895-126">The **Response** property of the **HttpActionExecutedContext** object contains the HTTP response message that will be sent to the client.</span></span>

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a><span data-ttu-id="c7895-127">L’inscription des filtres d’Exception</span><span class="sxs-lookup"><span data-stu-id="c7895-127">Registering Exception Filters</span></span>

<span data-ttu-id="c7895-128">Il existe plusieurs façons d’inscrire un filtre d’exception API Web :</span><span class="sxs-lookup"><span data-stu-id="c7895-128">There are several ways to register a Web API exception filter:</span></span>

- <span data-ttu-id="c7895-129">Par action</span><span class="sxs-lookup"><span data-stu-id="c7895-129">By action</span></span>
- <span data-ttu-id="c7895-130">Par contrôleur</span><span class="sxs-lookup"><span data-stu-id="c7895-130">By controller</span></span>
- <span data-ttu-id="c7895-131">Dans le monde entier</span><span class="sxs-lookup"><span data-stu-id="c7895-131">Globally</span></span>

<span data-ttu-id="c7895-132">Pour appliquer le filtre à une action spécifique, ajoutez le filtre en tant qu’attribut à l’action :</span><span class="sxs-lookup"><span data-stu-id="c7895-132">To apply the filter to a specific action, add the filter as an attribute to the action:</span></span>

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

<span data-ttu-id="c7895-133">Pour appliquer le filtre à toutes les actions sur un contrôleur, ajoutez le filtre en tant qu’attribut à la classe de contrôleur :</span><span class="sxs-lookup"><span data-stu-id="c7895-133">To apply the filter to all of the actions on a controller, add the filter as an attribute to the controller class:</span></span>

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

<span data-ttu-id="c7895-134">Pour appliquer le filtre globalement à tous les contrôleurs d’API Web, ajoutez une instance du filtre à la **GlobalConfiguration.Configuration.Filters** collection.</span><span class="sxs-lookup"><span data-stu-id="c7895-134">To apply the filter globally to all Web API controllers, add an instance of the filter to the **GlobalConfiguration.Configuration.Filters** collection.</span></span> <span data-ttu-id="c7895-135">Filtres d’exception dans cette collection s’appliquent à une action de contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="c7895-135">Exeption filters in this collection apply to any Web API controller action.</span></span>

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

<span data-ttu-id="c7895-136">Si vous utilisez le modèle de projet « Application Web ASP.NET MVC 4 » pour créer votre projet, placez votre code de configuration d’API Web à l’intérieur de la `WebApiConfig` (classe), qui se trouve dans l’application\_dossier de démarrage :</span><span class="sxs-lookup"><span data-stu-id="c7895-136">If you use the "ASP.NET MVC 4 Web Application" project template to create your project, put your Web API configuration code inside the `WebApiConfig` class, which is located in the App\_Start folder:</span></span>

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a><span data-ttu-id="c7895-137">Erreur HTTP</span><span class="sxs-lookup"><span data-stu-id="c7895-137">HttpError</span></span>

<span data-ttu-id="c7895-138">Le **HttpError** objet fournit un moyen cohérent de renvoyer des informations d’erreur dans le corps de réponse.</span><span class="sxs-lookup"><span data-stu-id="c7895-138">The **HttpError** object provides a consistent way to return error information in the response body.</span></span> <span data-ttu-id="c7895-139">L’exemple suivant montre comment retourner le code d’état HTTP 404 (introuvable) avec un **HttpError** dans le corps de réponse.</span><span class="sxs-lookup"><span data-stu-id="c7895-139">The following example shows how to return HTTP status code 404 (Not Found) with an **HttpError** in the response body.</span></span>

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

<span data-ttu-id="c7895-140">**CreateErrorResponse** est une méthode d’extension définie dans le **System.Net.Http.HttpRequestMessageExtensions** classe.</span><span class="sxs-lookup"><span data-stu-id="c7895-140">**CreateErrorResponse** is an extension method defined in the **System.Net.Http.HttpRequestMessageExtensions** class.</span></span> <span data-ttu-id="c7895-141">En interne, **CreateErrorResponse** crée un **HttpError** de l’instance, puis crée un **HttpResponseMessage** qui contient le **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="c7895-141">Internally, **CreateErrorResponse** creates an **HttpError** instance and then creates an **HttpResponseMessage** that contains the **HttpError**.</span></span>

<span data-ttu-id="c7895-142">Dans cet exemple, si la méthode réussite, elle retourne le produit dans la réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="c7895-142">In this example, if the method is successful, it returns the product in the HTTP response.</span></span> <span data-ttu-id="c7895-143">Mais si le produit demandé est introuvable, la réponse HTTP contient un **HttpError** dans le corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="c7895-143">But if the requested product is not found, the HTTP response contains an **HttpError** in the request body.</span></span> <span data-ttu-id="c7895-144">La réponse peut se présenter comme suit :</span><span class="sxs-lookup"><span data-stu-id="c7895-144">The response might look like the following:</span></span>

[!code-console[Main](exception-handling/samples/sample9.cmd)]

<span data-ttu-id="c7895-145">Notez que le **HttpError** a été sérialisé au format JSON dans cet exemple.</span><span class="sxs-lookup"><span data-stu-id="c7895-145">Notice that the **HttpError** was serialized to JSON in this example.</span></span> <span data-ttu-id="c7895-146">L’un des avantages de l’utilisation de **HttpError** est qu’il traverse le même [négociation de contenu](../formats-and-model-binding/content-negotiation.md) et processus de sérialisation comme tout autre modèle fortement typé.</span><span class="sxs-lookup"><span data-stu-id="c7895-146">One advantage of using **HttpError** is that it goes through the same [content-negotiation](../formats-and-model-binding/content-negotiation.md) and serialization process as any other strongly-typed model.</span></span>

### <a name="httperror-and-model-validation"></a><span data-ttu-id="c7895-147">Erreur HTTP et la Validation du modèle</span><span class="sxs-lookup"><span data-stu-id="c7895-147">HttpError and Model Validation</span></span>

<span data-ttu-id="c7895-148">Pour la validation de modèle, vous pouvez passer à l’état du modèle **CreateErrorResponse**, pour inclure les erreurs de validation dans la réponse :</span><span class="sxs-lookup"><span data-stu-id="c7895-148">For model validation, you can pass the model state to **CreateErrorResponse**, to include the validation errors in the response:</span></span>

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

<span data-ttu-id="c7895-149">Cet exemple peut renvoyer la réponse suivante :</span><span class="sxs-lookup"><span data-stu-id="c7895-149">This example might return the following response:</span></span>

[!code-console[Main](exception-handling/samples/sample11.cmd)]

<span data-ttu-id="c7895-150">Pour plus d’informations sur la validation de modèle, consultez [Validation de modèle dans ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="c7895-150">For more information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

### <a name="using-httperror-with-httpresponseexception"></a><span data-ttu-id="c7895-151">À l’aide d’erreur HTTP avec HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="c7895-151">Using HttpError with HttpResponseException</span></span>

<span data-ttu-id="c7895-152">Les exemples précédents retournent un **HttpResponseMessage** message à partir de l’action du contrôleur, mais vous pouvez également utiliser **HttpResponseException** pour retourner un **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="c7895-152">The previous examples return an **HttpResponseMessage** message from the controller action, but you can also use **HttpResponseException** to return an **HttpError**.</span></span> <span data-ttu-id="c7895-153">Cela vous permet de renvoyer un modèle fortement typé dans le cas normal de réussite, tout en continuant à renvoyer **HttpError** si une erreur se produit :</span><span class="sxs-lookup"><span data-stu-id="c7895-153">This lets you return a strongly-typed model in the normal success case, while still returning **HttpError** if there is an error:</span></span>

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
