---
title: Gérer les erreurs dans les API Web ASP.NET Core
author: pranavkm
description: En savoir plus sur la gestion des erreurs avec ASP.NET Core API Web.
monikerRange: '>= aspnetcore-2.1'
ms.author: prkrishn
ms.custom: mvc
ms.date: 09/18/2019
uid: web-api/handle-errors
ms.openlocfilehash: bf8229d37ef15c42b5d7bb4a12737ac65d7b753c
ms.sourcegitcommit: a11f09c10ef3d4eeab7ae9ce993e7f30427741c1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150907"
---
# <a name="handle-errors-in-aspnet-core-web-apis"></a><span data-ttu-id="01ca0-103">Gérer les erreurs dans les API Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="01ca0-103">Handle errors in ASP.NET Core web APIs</span></span>

<span data-ttu-id="01ca0-104">Cet article explique comment gérer et personnaliser la gestion des erreurs avec ASP.NET Core API Web.</span><span class="sxs-lookup"><span data-stu-id="01ca0-104">This article describes how to handle and customize error handling with ASP.NET Core web APIs.</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="01ca0-105">Page d’exceptions du développeur</span><span class="sxs-lookup"><span data-stu-id="01ca0-105">Developer Exception Page</span></span>

<span data-ttu-id="01ca0-106">La [page exception du développeur](xref:fundamentals/error-handling) est un outil utile pour obtenir des traces de pile détaillées pour les erreurs de serveur.</span><span class="sxs-lookup"><span data-stu-id="01ca0-106">The [Developer Exception Page](xref:fundamentals/error-handling) is a useful tool to get detailed stack traces for server errors.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="01ca0-107">La page exception du développeur affiche une réponse en texte brut si le client n’accepte pas la sortie au format HTML :</span><span class="sxs-lookup"><span data-stu-id="01ca0-107">The Developer Exception Page displays a plain-text response if the client doesn't accept HTML-formatted output:</span></span>

```
> curl https://localhost:5001/weatherforecast
System.ArgumentException: count
   at errorhandling.Controllers.WeatherForecastController.Get(Int32 x) in D:\work\Samples\samples\aspnetcore\mvc\errorhandling\Controllers\WeatherForecastController.cs:line 35
   at lambda_method(Closure , Object , Object[] )
   at Microsoft.Extensions.Internal.ObjectMethodExecutor.Execute(Object target, Object[] parameters)
...
```

::: moniker-end

> [!WARNING]
> <span data-ttu-id="01ca0-108">Activez la page d’exception de développeur **uniquement quand l’application est en cours d’exécution dans l’environnement de développement**.</span><span class="sxs-lookup"><span data-stu-id="01ca0-108">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="01ca0-109">Il n’est pas souhaitable de partager publiquement des informations détaillées sur les exceptions quand l’application s’exécute en production.</span><span class="sxs-lookup"><span data-stu-id="01ca0-109">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="01ca0-110">Pour plus d’informations sur la configuration des environnements, consultez <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="01ca0-110">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

## <a name="exception-handler"></a><span data-ttu-id="01ca0-111">Gestionnaire d’exceptions</span><span class="sxs-lookup"><span data-stu-id="01ca0-111">Exception handler</span></span>

<span data-ttu-id="01ca0-112">Dans les environnements non-développement, l’intergiciel (middleware) de [gestion des exceptions](xref:fundamentals/error-handling) peut être utilisé pour générer une charge utile d’erreur :</span><span class="sxs-lookup"><span data-stu-id="01ca0-112">In non-development environments, [Exception Handling Middleware](xref:fundamentals/error-handling) can be used to produce an error payload:</span></span>

1. <span data-ttu-id="01ca0-113">Dans `Startup.Configure`, appelez <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> pour utiliser l’intergiciel (middleware) :</span><span class="sxs-lookup"><span data-stu-id="01ca0-113">In `Startup.Configure`, invoke <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> to use the middleware:</span></span>

    ```csharp
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }
        else
        {
            app.UseExceptionHandler("/error");
        }
    }
    ```

1. <span data-ttu-id="01ca0-114">Configurez une action de contrôleur pour `/error` répondre à l’itinéraire :</span><span class="sxs-lookup"><span data-stu-id="01ca0-114">Configure a controller action to respond to the `/error` route:</span></span>

    ```csharp
    [ApiController]
    public class ErrorController : ControllerBase
    {
        [Route("/error")]
        public IActionResult Error() => Problem();
    }
    ```

<span data-ttu-id="01ca0-115">L’action `Error` précédente envoie une charge utile conforme à [RFC7807](https://tools.ietf.org/html/rfc7807)au client.</span><span class="sxs-lookup"><span data-stu-id="01ca0-115">The preceding `Error` action sends an [RFC7807](https://tools.ietf.org/html/rfc7807)-compliant payload to the client.</span></span>

<span data-ttu-id="01ca0-116">L’intergiciel (middleware) de gestion des exceptions peut également fournir une sortie de négociation de contenu plus détaillée dans l’environnement de développement local.</span><span class="sxs-lookup"><span data-stu-id="01ca0-116">Exception Handling Middleware can also provide more detailed content-negotiated output in the local development environment.</span></span> <span data-ttu-id="01ca0-117">Utilisez les étapes suivantes pour créer un format de charge utile cohérent dans les environnements de développement et de production :</span><span class="sxs-lookup"><span data-stu-id="01ca0-117">Use the following steps to produce a consistent payload format across development and production environments:</span></span>

1. <span data-ttu-id="01ca0-118">Dans `Startup.Configure`, inscrivez les instances d’intergiciels (middleware) de gestion des exceptions spécifiques à l’environnement :</span><span class="sxs-lookup"><span data-stu-id="01ca0-118">In `Startup.Configure`, register environment-specific Exception Handling Middleware instances:</span></span>

    ```csharp
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseExceptionHandler("/error-local-development");
        }
        else
        {
            app.UseExceptionHandler("/error");
        }
    }
    ```

    <span data-ttu-id="01ca0-119">Dans le code précédent, l’intergiciel est inscrit avec :</span><span class="sxs-lookup"><span data-stu-id="01ca0-119">In the preceding code, the middleware is registered with:</span></span>

    * <span data-ttu-id="01ca0-120">Itinéraire de dans `/error-local-development` l’environnement de développement.</span><span class="sxs-lookup"><span data-stu-id="01ca0-120">A route of `/error-local-development` in the Development environment.</span></span>
    * <span data-ttu-id="01ca0-121">Un itinéraire de `/error` dans des environnements qui ne sont pas des développements.</span><span class="sxs-lookup"><span data-stu-id="01ca0-121">A route of `/error` in environments that aren't Development.</span></span>
    
1. <span data-ttu-id="01ca0-122">Appliquer le routage d’attribut aux actions du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="01ca0-122">Apply attribute routing to controller actions:</span></span>

    ```csharp
    [ApiController]
    public class ErrorController : ControllerBase
    {
        [Route("/error-local-development")]
        public IActionResult ErrorLocalDevelopment(
            [FromServices] IWebHostEnvironment webHostEnvironment)
        {
            if (!webHostEnvironment.IsDevelopment())
            {
                throw new InvalidOperationException(
                    "This shouldn't be invoked in non-development environments.");
            }
    
            var context = HttpContext.Features.Get<IExceptionHandlerFeature>();

            return Problem(
                detail: context.Error.StackTrace,
                title: context.Error.Message);
        }
    
        [Route("/error")]
        public IActionResult Error() => Problem();
    }
    ```

## <a name="use-exceptions-to-modify-the-response"></a><span data-ttu-id="01ca0-123">Utiliser des exceptions pour modifier la réponse</span><span class="sxs-lookup"><span data-stu-id="01ca0-123">Use exceptions to modify the response</span></span>

<span data-ttu-id="01ca0-124">Le contenu de la réponse peut être modifié à partir de l’extérieur du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="01ca0-124">The contents of the response can be modified from outside of the controller.</span></span> <span data-ttu-id="01ca0-125">Dans l’API Web ASP.net 4. x, l’une des méthodes permettant d’effectuer <xref:System.Web.Http.HttpResponseException> cette opération était d’utiliser le type.</span><span class="sxs-lookup"><span data-stu-id="01ca0-125">In ASP.NET 4.x Web API, one way to do this was using the <xref:System.Web.Http.HttpResponseException> type.</span></span> <span data-ttu-id="01ca0-126">ASP.NET Core n’inclut pas de type équivalent.</span><span class="sxs-lookup"><span data-stu-id="01ca0-126">ASP.NET Core doesn't include an equivalent type.</span></span> <span data-ttu-id="01ca0-127">La prise `HttpResponseException` en charge de peut être ajoutée avec les étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="01ca0-127">Support for `HttpResponseException` can be added with the following steps:</span></span>

1. <span data-ttu-id="01ca0-128">Créez un type d’exception connu nommé `HttpResponseException`:</span><span class="sxs-lookup"><span data-stu-id="01ca0-128">Create a well-known exception type named `HttpResponseException`:</span></span>

    ```csharp
    public class HttpResponseException : Exception
    {
        public int Status { get; set; } = 500;
    
        public object Value { get; set; }
    }
    ```

1. <span data-ttu-id="01ca0-129">Créez un filtre d’action `HttpResponseExceptionFilter`nommé :</span><span class="sxs-lookup"><span data-stu-id="01ca0-129">Create an action filter named `HttpResponseExceptionFilter`:</span></span>

    ```csharp
    public class HttpResponseExceptionFilter : IActionFilter, IOrderedFilter
    {
        public int Order { get; set; } = int.MaxValue - 10;
    
        public void OnActionExecuting(ActionExecutingContext context) {}
    
        public void OnActionExecuted(ActionExecutedContext context)
        {
            if (context.Exception is HttpResponseException exception)
            {
                context.Result = new ObjectResult(exception.Value)
                {
                    Status = exception.Status,
                };
                context.ExceptionHandled = true;
            }
        }
    }
    ```

1. <span data-ttu-id="01ca0-130">Dans `Startup.ConfigureServices`, ajoutez le filtre d’action à la collection de filtres :</span><span class="sxs-lookup"><span data-stu-id="01ca0-130">In `Startup.ConfigureServices`, add the action filter to the filters collection:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddControllers(options => 
            options.Filters.Add(new HttpResponseExceptionFilter()));
    }
    ```

## <a name="validation-failure-error-response"></a><span data-ttu-id="01ca0-131">Réponse d’erreur d’échec de validation</span><span class="sxs-lookup"><span data-stu-id="01ca0-131">Validation failure error response</span></span>

<span data-ttu-id="01ca0-132">Pour les contrôleurs d’API Web, MVC <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> répond avec un type de réponse lors de l’échec de la validation du modèle.</span><span class="sxs-lookup"><span data-stu-id="01ca0-132">For web API controllers, MVC responds with a <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails> response type when model validation fails.</span></span> <span data-ttu-id="01ca0-133">MVC utilise les résultats de <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> pour construire la réponse d’erreur pour un échec de validation.</span><span class="sxs-lookup"><span data-stu-id="01ca0-133">MVC uses the results of <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to construct the error response for a validation failure.</span></span> <span data-ttu-id="01ca0-134">L’exemple suivant utilise la fabrique pour modifier le type <xref:Microsoft.AspNetCore.Mvc.SerializableError> de réponse par défaut en in : `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="01ca0-134">The following example uses the factory to change the default response type to <xref:Microsoft.AspNetCore.Mvc.SerializableError> in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=4-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_DisableProblemDetailsInvalidModelStateResponseFactory&highlight=5-14)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);

services.Configure<ApiBehaviorOptions>(options =>
{
    options.InvalidModelStateResponseFactory = context =>
    {
        var result = new BadRequestObjectResult(context.ModelState);

        // TODO: add `using using System.Net.Mime;` to resolve MediaTypeNames
        result.ContentTypes.Add(MediaTypeNames.Application.Json);
        result.ContentTypes.Add(MediaTypeNames.Application.Xml);

        return result;
    };
});
```

::: moniker-end

## <a name="client-error-response"></a><span data-ttu-id="01ca0-135">Réponse d’erreur du client</span><span class="sxs-lookup"><span data-stu-id="01ca0-135">Client error response</span></span>

<span data-ttu-id="01ca0-136">Un *résultat d’erreur* est défini en tant que résultat avec le code d’état HTTP 400 ou une version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="01ca0-136">An *error result* is defined as a result with an HTTP status code of 400 or higher.</span></span> <span data-ttu-id="01ca0-137">Pour les contrôleurs d’API Web, MVC transforme un résultat d’erreur en <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>un résultat avec.</span><span class="sxs-lookup"><span data-stu-id="01ca0-137">For web API controllers, MVC transforms an error result to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="01ca0-138">La réponse d’erreur peut être configurée de l’une des manières suivantes :</span><span class="sxs-lookup"><span data-stu-id="01ca0-138">The error response can be configured in one of the following ways:</span></span>

1. [<span data-ttu-id="01ca0-139">Implémenter ProblemDetailsFactory</span><span class="sxs-lookup"><span data-stu-id="01ca0-139">Implement ProblemDetailsFactory</span></span>](#implement-problemdetailsfactory)
1. [<span data-ttu-id="01ca0-140">Utilisez ApiBehaviorOptions. ClientErrorMapping</span><span class="sxs-lookup"><span data-stu-id="01ca0-140">Use ApiBehaviorOptions.ClientErrorMapping</span></span>](#use-apibehavioroptionsclienterrormapping)

### <a name="implement-problemdetailsfactory"></a><span data-ttu-id="01ca0-141">Implémenter ProblemDetailsFactory</span><span class="sxs-lookup"><span data-stu-id="01ca0-141">Implement ProblemDetailsFactory</span></span>

<span data-ttu-id="01ca0-142">MVC utilise `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory` pour générer toutes les instances <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> de <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>et.</span><span class="sxs-lookup"><span data-stu-id="01ca0-142">MVC uses `Microsoft.AspNetCore.Mvc.ProblemDetailsFactory` to produce all instances of <xref:Microsoft.AspNetCore.Mvc.ProblemDetails> and <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="01ca0-143">Cela comprend les réponses d’erreur du client, les réponses d’erreur `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` de <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> validation et les méthodes d’assistance et.</span><span class="sxs-lookup"><span data-stu-id="01ca0-143">This includes client error responses, validation failure error responses, and the `Microsoft.AspNetCore.Mvc.ControllerBase.Problem` and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ValidationProblem> helper methods.</span></span>

<span data-ttu-id="01ca0-144">Pour personnaliser la réponse des détails du problème, inscrivez une implémentation `ProblemDetailsFactory` personnalisée `Startup.ConfigureServices`de dans :</span><span class="sxs-lookup"><span data-stu-id="01ca0-144">To customize the problem details response, register a custom implementation of `ProblemDetailsFactory` in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection serviceCollection)
{
    services.AddControllers();
    services.AddTransient<ProblemDetailsFactory, CustomProblemDetailsFactory>();
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="01ca0-145">La réponse d’erreur peut être configurée comme indiqué dans la section [use ApiBehaviorOptions. ClientErrorMapping](#use-apibehavioroptionsclienterrormapping) .</span><span class="sxs-lookup"><span data-stu-id="01ca0-145">The error response can be configured as outlined in the [Use ApiBehaviorOptions.ClientErrorMapping](#use-apibehavioroptionsclienterrormapping) section.</span></span>

::: moniker-end

### <a name="use-apibehavioroptionsclienterrormapping"></a><span data-ttu-id="01ca0-146">Utilisez ApiBehaviorOptions. ClientErrorMapping</span><span class="sxs-lookup"><span data-stu-id="01ca0-146">Use ApiBehaviorOptions.ClientErrorMapping</span></span>

<span data-ttu-id="01ca0-147">Utilisez la propriété <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> pour configurer le contenu de la réponse `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="01ca0-147">Use the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping*> property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="01ca0-148">Par exemple, le code suivant met à jour la propriété `type` pour les réponses 404 :</span><span class="sxs-lookup"><span data-stu-id="01ca0-148">For example, the following code updates the `type` property for 404 responses:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8-9)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=9-10)]

::: moniker-end
