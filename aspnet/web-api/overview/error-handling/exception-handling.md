---
uid: web-api/overview/error-handling/exception-handling
title: Gestion des exceptions dans l’API Web ASP.NET | Documents Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2012
ms.topic: article
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: c65ddcca012840d70ab5a33af92edb30041be971
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506958"
---
<a name="exception-handling-in-aspnet-web-api"></a><span data-ttu-id="b37b7-102">Gestion des exceptions dans l’API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b37b7-102">Exception Handling in ASP.NET Web API</span></span>
====================
<span data-ttu-id="b37b7-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b37b7-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="b37b7-104">Cet article décrit l’erreur et gestion des exceptions dans l’API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b37b7-104">This article describes error and exception handling in ASP.NET Web API.</span></span>

- [<span data-ttu-id="b37b7-105">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="b37b7-105">HttpResponseException</span></span>](#httpresponserexception)
- [<span data-ttu-id="b37b7-106">Filtres d’exception</span><span class="sxs-lookup"><span data-stu-id="b37b7-106">Exception Filters</span></span>](#exception_filters)
- [<span data-ttu-id="b37b7-107">L’inscription des filtres d’Exception</span><span class="sxs-lookup"><span data-stu-id="b37b7-107">Registering Exception Filters</span></span>](#registering_exception_filters)
- [<span data-ttu-id="b37b7-108">Erreur HTTP</span><span class="sxs-lookup"><span data-stu-id="b37b7-108">HttpError</span></span>](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a><span data-ttu-id="b37b7-109">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="b37b7-109">HttpResponseException</span></span>

<span data-ttu-id="b37b7-110">Que se passe-t-il si un contrôleur d’API Web lève une exception non interceptée ?</span><span class="sxs-lookup"><span data-stu-id="b37b7-110">What happens if a Web API controller throws an uncaught exception?</span></span> <span data-ttu-id="b37b7-111">Par défaut, la plupart des exceptions sont traduites en une réponse HTTP avec le code d’état 500 Erreur interne au serveur.</span><span class="sxs-lookup"><span data-stu-id="b37b7-111">By default, most exceptions are translated into an HTTP response with status code 500, Internal Server Error.</span></span>

<span data-ttu-id="b37b7-112">Le **HttpResponseException** type est un cas spécial.</span><span class="sxs-lookup"><span data-stu-id="b37b7-112">The **HttpResponseException** type is a special case.</span></span> <span data-ttu-id="b37b7-113">Cette exception retourne un code d’état HTTP que vous spécifiez dans le constructeur de l’exception.</span><span class="sxs-lookup"><span data-stu-id="b37b7-113">This exception returns any HTTP status code that you specify in the exception constructor.</span></span> <span data-ttu-id="b37b7-114">Par exemple, la méthode suivante retourne 404 introuvable, si la *id* paramètre n’est pas valide.</span><span class="sxs-lookup"><span data-stu-id="b37b7-114">For example, the following method returns 404, Not Found, if the *id* parameter is not valid.</span></span>

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

<span data-ttu-id="b37b7-115">Pour mieux contrôler la réponse, vous pouvez également construire le message de réponse entier et l’inclure avec la **HttpResponseException :**</span><span class="sxs-lookup"><span data-stu-id="b37b7-115">For more control over the response, you can also construct the entire response message and include it with the **HttpResponseException:**</span></span> 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a><span data-ttu-id="b37b7-116">Filtres d’exception</span><span class="sxs-lookup"><span data-stu-id="b37b7-116">Exception Filters</span></span>

<span data-ttu-id="b37b7-117">Vous pouvez personnaliser comment API Web gère les exceptions en écrivant un *filtre d’exception*.</span><span class="sxs-lookup"><span data-stu-id="b37b7-117">You can customize how Web API handles exceptions by writing an *exception filter*.</span></span> <span data-ttu-id="b37b7-118">Un filtre d’exception est exécuté lorsqu’une méthode de contrôleur lève une exception non gérée qui est *pas* un **HttpResponseException** exception.</span><span class="sxs-lookup"><span data-stu-id="b37b7-118">An exception filter is executed when a controller method throws any unhandled exception that is *not* an **HttpResponseException** exception.</span></span> <span data-ttu-id="b37b7-119">Le **HttpResponseException** type est un cas spécial, parce qu’il est conçu spécifiquement pour renvoyer une réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="b37b7-119">The **HttpResponseException** type is a special case, because it is designed specifically for returning an HTTP response.</span></span>

<span data-ttu-id="b37b7-120">Implémentent des filtres d’exception le **System.Web.Http.Filters.IExceptionFilter** interface.</span><span class="sxs-lookup"><span data-stu-id="b37b7-120">Exception filters implement the **System.Web.Http.Filters.IExceptionFilter** interface.</span></span> <span data-ttu-id="b37b7-121">La façon la plus simple pour écrire un filtre d’exception doit dériver de la **System.Web.Http.Filters.ExceptionFilterAttribute** classe et substituer les **OnException** (méthode).</span><span class="sxs-lookup"><span data-stu-id="b37b7-121">The simplest way to write an exception filter is to derive from the **System.Web.Http.Filters.ExceptionFilterAttribute** class and override the **OnException** method.</span></span>

> [!NOTE]
> <span data-ttu-id="b37b7-122">Filtres d’exception dans l’API Web ASP.NET sont similaires à celles utilisées dans ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b37b7-122">Exception filters in ASP.NET Web API are similar to those in ASP.NET MVC.</span></span> <span data-ttu-id="b37b7-123">Toutefois, ils sont déclarés dans un espace de noms distinct et une fonction séparément.</span><span class="sxs-lookup"><span data-stu-id="b37b7-123">However, they are declared in a separate namespace and function separately.</span></span> <span data-ttu-id="b37b7-124">En particulier, la **HandleErrorAttribute** classe utilisée dans MVC ne gère pas les exceptions levées par les contrôleurs de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="b37b7-124">In particular, the **HandleErrorAttribute** class used in MVC does not handle exceptions thrown by Web API controllers.</span></span>


<span data-ttu-id="b37b7-125">Voici un filtre qui convertit **NotImplementedException** 501, non implémenté de code des exceptions dans l’état HTTP :</span><span class="sxs-lookup"><span data-stu-id="b37b7-125">Here is a filter that converts **NotImplementedException** exceptions into HTTP status code 501, Not Implemented:</span></span>

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

<span data-ttu-id="b37b7-126">Le **réponse** propriété de la **HttpActionExecutedContext** objet contient le message de réponse HTTP qui sera envoyé au client.</span><span class="sxs-lookup"><span data-stu-id="b37b7-126">The **Response** property of the **HttpActionExecutedContext** object contains the HTTP response message that will be sent to the client.</span></span>

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a><span data-ttu-id="b37b7-127">L’inscription des filtres d’Exception</span><span class="sxs-lookup"><span data-stu-id="b37b7-127">Registering Exception Filters</span></span>

<span data-ttu-id="b37b7-128">Il existe plusieurs façons d’enregistrer un filtre d’exception API Web :</span><span class="sxs-lookup"><span data-stu-id="b37b7-128">There are several ways to register a Web API exception filter:</span></span>

- <span data-ttu-id="b37b7-129">Par action</span><span class="sxs-lookup"><span data-stu-id="b37b7-129">By action</span></span>
- <span data-ttu-id="b37b7-130">Par le contrôleur</span><span class="sxs-lookup"><span data-stu-id="b37b7-130">By controller</span></span>
- <span data-ttu-id="b37b7-131">Global</span><span class="sxs-lookup"><span data-stu-id="b37b7-131">Globally</span></span>

<span data-ttu-id="b37b7-132">Pour appliquer le filtre à une action spécifique, ajoutez le filtre en tant qu’attribut de l’action :</span><span class="sxs-lookup"><span data-stu-id="b37b7-132">To apply the filter to a specific action, add the filter as an attribute to the action:</span></span>

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

<span data-ttu-id="b37b7-133">Pour appliquer le filtre à toutes les actions sur un contrôleur, ajoutez le filtre en tant qu’attribut à la classe de contrôleur :</span><span class="sxs-lookup"><span data-stu-id="b37b7-133">To apply the filter to all of the actions on a controller, add the filter as an attribute to the controller class:</span></span>

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

<span data-ttu-id="b37b7-134">Pour appliquer le filtre globalement à tous les contrôleurs de l’API Web, ajoutez une instance du filtre à la **GlobalConfiguration.Configuration.Filters** collection.</span><span class="sxs-lookup"><span data-stu-id="b37b7-134">To apply the filter globally to all Web API controllers, add an instance of the filter to the **GlobalConfiguration.Configuration.Filters** collection.</span></span> <span data-ttu-id="b37b7-135">Filtres d’exception dans cette collection s’appliquent à toute action de contrôleur d’API Web.</span><span class="sxs-lookup"><span data-stu-id="b37b7-135">Exeption filters in this collection apply to any Web API controller action.</span></span>

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

<span data-ttu-id="b37b7-136">Si vous utilisez le modèle de projet « Application Web ASP.NET MVC 4 » pour créer votre projet, placez votre code de configuration de API Web à l’intérieur de la `WebApiConfig` (classe), qui se trouve dans l’application\_dossier de démarrage :</span><span class="sxs-lookup"><span data-stu-id="b37b7-136">If you use the "ASP.NET MVC 4 Web Application" project template to create your project, put your Web API configuration code inside the `WebApiConfig` class, which is located in the App\_Start folder:</span></span>

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a><span data-ttu-id="b37b7-137">Erreur HTTP</span><span class="sxs-lookup"><span data-stu-id="b37b7-137">HttpError</span></span>

<span data-ttu-id="b37b7-138">Le **HttpError** objet fournit un moyen cohérent de retourner des informations d’erreur dans le corps de réponse.</span><span class="sxs-lookup"><span data-stu-id="b37b7-138">The **HttpError** object provides a consistent way to return error information in the response body.</span></span> <span data-ttu-id="b37b7-139">L’exemple suivant montre comment retourner le code d’état HTTP 404 (introuvable) avec une **HttpError** dans le corps de réponse.</span><span class="sxs-lookup"><span data-stu-id="b37b7-139">The following example shows how to return HTTP status code 404 (Not Found) with an **HttpError** in the response body.</span></span>

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

<span data-ttu-id="b37b7-140">**CreateErrorResponse** est une méthode d’extension définie dans le **System.Net.Http.HttpRequestMessageExtensions** classe.</span><span class="sxs-lookup"><span data-stu-id="b37b7-140">**CreateErrorResponse** is an extension method defined in the **System.Net.Http.HttpRequestMessageExtensions** class.</span></span> <span data-ttu-id="b37b7-141">En interne, **CreateErrorResponse** crée un **HttpError** de l’instance, puis crée un **HttpResponseMessage** qui contient le **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="b37b7-141">Internally, **CreateErrorResponse** creates an **HttpError** instance and then creates an **HttpResponseMessage** that contains the **HttpError**.</span></span>

<span data-ttu-id="b37b7-142">Dans cet exemple, si la méthode réussit, elle retourne le produit dans la réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="b37b7-142">In this example, if the method is successful, it returns the product in the HTTP response.</span></span> <span data-ttu-id="b37b7-143">Mais si le produit demandé est introuvable, la réponse HTTP contient un **HttpError** dans le corps de la demande.</span><span class="sxs-lookup"><span data-stu-id="b37b7-143">But if the requested product is not found, the HTTP response contains an **HttpError** in the request body.</span></span> <span data-ttu-id="b37b7-144">La réponse peut se présenter comme suit :</span><span class="sxs-lookup"><span data-stu-id="b37b7-144">The response might look like the following:</span></span>

[!code-console[Main](exception-handling/samples/sample9.cmd)]

<span data-ttu-id="b37b7-145">Notez que la **HttpError** a été sérialisé en JSON dans cet exemple.</span><span class="sxs-lookup"><span data-stu-id="b37b7-145">Notice that the **HttpError** was serialized to JSON in this example.</span></span> <span data-ttu-id="b37b7-146">L’un des avantages de l’utilisation de **HttpError** est qu’il passe par le même [négociation de contenu](../formats-and-model-binding/content-negotiation.md) et processus de sérialisation que tout autre modèle fortement typé.</span><span class="sxs-lookup"><span data-stu-id="b37b7-146">One advantage of using **HttpError** is that it goes through the same [content-negotiation](../formats-and-model-binding/content-negotiation.md) and serialization process as any other strongly-typed model.</span></span>

### <a name="httperror-and-model-validation"></a><span data-ttu-id="b37b7-147">Erreur HTTP et la Validation du modèle</span><span class="sxs-lookup"><span data-stu-id="b37b7-147">HttpError and Model Validation</span></span>

<span data-ttu-id="b37b7-148">Pour la validation de modèle, vous pouvez passer à l’état du modèle **CreateErrorResponse**, pour inclure les erreurs de validation dans la réponse :</span><span class="sxs-lookup"><span data-stu-id="b37b7-148">For model validation, you can pass the model state to **CreateErrorResponse**, to include the validation errors in the response:</span></span>

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

<span data-ttu-id="b37b7-149">Cet exemple peut retourner la réponse suivante :</span><span class="sxs-lookup"><span data-stu-id="b37b7-149">This example might return the following response:</span></span>

[!code-console[Main](exception-handling/samples/sample11.cmd)]

<span data-ttu-id="b37b7-150">Pour plus d’informations sur la validation des modèles, consultez [Validation de modèle dans ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="b37b7-150">For more information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

### <a name="using-httperror-with-httpresponseexception"></a><span data-ttu-id="b37b7-151">À l’aide d’erreur HTTP avec HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="b37b7-151">Using HttpError with HttpResponseException</span></span>

<span data-ttu-id="b37b7-152">Les exemples précédents retournent un **HttpResponseMessage** message à partir de l’action du contrôleur, mais vous pouvez également utiliser **HttpResponseException** pour retourner une **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="b37b7-152">The previous examples return an **HttpResponseMessage** message from the controller action, but you can also use **HttpResponseException** to return an **HttpError**.</span></span> <span data-ttu-id="b37b7-153">Cela vous permet de retourner un modèle fortement typé dans le cas de réussite normal, tout en continuant à renvoyer **HttpError** s’il existe une erreur :</span><span class="sxs-lookup"><span data-stu-id="b37b7-153">This lets you return a strongly-typed model in the normal success case, while still returning **HttpError** if there is an error:</span></span>

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
