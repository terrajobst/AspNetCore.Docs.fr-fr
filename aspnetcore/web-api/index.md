---
title: Créer des API web avec ASP.NET Core
author: scottaddie
description: Découvrez les fonctionnalités disponibles pour la création d’une API web dans ASP.NET Core et comment choisir la fonctionnalité la plus appropriée en fonction du besoin.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/11/2019
uid: web-api/index
---
# <a name="build-web-apis-with-aspnet-core"></a>Créer des API web avec ASP.NET Core

Par [Scott Addie](https://github.com/scottaddie)

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

Ce document explique comment créer une API web ASP.NET Core et comment choisir la fonctionnalité la plus appropriée en fonction du besoin.

## <a name="derive-class-from-controllerbase"></a>Dériver la classe de ControllerBase

Héritez de la classe <xref:Microsoft.AspNetCore.Mvc.ControllerBase> dans un contrôleur destiné à servir d’API web. Par exemple :

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

La classe `ControllerBase` fournit l’accès à plusieurs propriétés et méthodes. Dans le code précédent, les exemples incluent <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> et <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>. Ces méthodes sont appelées dans les méthodes d’action pour retourner les codes d’état HTTP 400 et 201, respectivement. La propriété <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState>, également fournie par `ControllerBase`, permet de gérer la validation des modèles de requêtes.

::: moniker range=">= aspnetcore-2.1"

## <a name="annotation-with-apicontroller-attribute"></a>Annotation avec attribut ApiController

ASP.NET Core 2.1 introduit l’attribut [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) pour désigner une classe de contrôleur d’API web. Par exemple :

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

Une version de compatibilité 2.1 ou ultérieure, définie par le biais de <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, est obligatoire pour utiliser cet attribut au niveau du contrôleur. Par exemple, le code en surbrillance dans `Startup.ConfigureServices` définit l’indicateur de compatibilité 2.1 :

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

Pour plus d'informations, consultez <xref:mvc/compatibility-version>.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Dans ASP.NET Core 2.2 ou ultérieur, l’attribut `[ApiController]` peut être appliqué à un assembly. De cette manière, l’annotation applique le comportement de l’API web à tous les contrôleurs de l’assembly. Notez que, dans ce cas, les contrôleurs individuels n’ont aucun moyen de faire en sorte que cet attribut ne s'applique pas. Il est recommandé d’appliquer des attributs de niveau assembly à la classe `Startup` :

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ApiControllerAttributeOnAssembly&highlight=1)]

Une version de compatibilité 2.2 ou ultérieure, définie par le biais de <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, est obligatoire pour utiliser cet attribut au niveau de l’assembly.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

L’attribut `[ApiController]` est généralement associé à `ControllerBase` pour donner aux contrôleurs un comportement spécifique à REST. `ControllerBase` permet d’accéder aux méthodes telles que <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> et <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.

Une autre approche consiste à créer une classe de contrôleur de base personnalisée annotée avec l’attribut `[ApiController]` :

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

Les sections suivantes décrivent les fonctionnalités pratiques ajoutées par l’attribut.

### <a name="automatic-http-400-responses"></a>Réponses HTTP 400 automatiques

Les erreurs de validation de modèles déclenchent automatiquement une réponse HTTP 400. Par conséquent, le code suivant devient inutile dans vos actions :

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

Utilisez <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> pour personnaliser la sortie de la réponse résultante.

La désactivation du comportement par défaut est utile lorsque votre action peut récupérer suite à une erreur de validation de modèle. Le comportement par défaut est désactivé quand la propriété <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> a la valeur `true`. Ajoutez le code suivant dans `Startup.ConfigureServices` après `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);` :

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=7)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Avec un indicateur de compatibilité 2.2 ou ultérieur, le type de réponse par défaut pour les réponses HTTP 400 est <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>. Le type `ValidationProblemDetails` est conforme à la [spécification RFC 7807](https://tools.ietf.org/html/rfc7807). Définissez la propriété `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` avec la valeur `true` pour retourner à la place le format d’erreur ASP.NET Core 2.1 de <xref:Microsoft.AspNetCore.Mvc.SerializableError>. Ajoutez le code suivant dans `Startup.ConfigureServices` :

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2)
    .ConfigureApiBehaviorOptions(options =>
    {
        options
          .SuppressUseValidationProblemDetailsForInvalidModelStateResponses = true;
    });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="binding-source-parameter-inference"></a>Inférence de paramètre de source de liaison

Un attribut de source de liaison définit l’emplacement auquel se trouve la valeur d’un paramètre d’action. Les attributs de source de liaison suivants existent :

|Attribut|Source de liaison |
|---------|---------|
|**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**     | Corps de la requête |
|**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**     | Données de formulaire dans le corps de la demande |
|**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)** | En-tête de demande |
|**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**   | Paramètre de la chaîne de requête de la demande |
|**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**   | Données d’itinéraire à partir de la demande actuelle |
|**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)** | Service de demande injecté comme paramètre d’action |

> [!WARNING]
> N’utilisez pas `[FromRoute]` lorsque les valeurs risquent de contenir `%2f` (c’est-à-dire `/`). `%2f` ne sera pas sans la séquence d’échappement `/`. Utilisez `[FromQuery]` si la valeur peut contenir `%2f`.

Sans l’attribut `[ApiController]`, les attributs de source de liaison sont définis explicitement. Sans `[ApiController]` ou autres attributs de source de liaison comme `[FromQuery]`, le runtime ASP.NET Core tente d’utiliser le classeur de modèles objet complexe. Le classeur de modèles objet complexe extrait des données à partir de fournisseurs de valeurs (qui ont un ordre défini). Par exemple,le « classeur de modèles body » est toujours activé.

Dans l’exemple suivant, l’attribut `[FromQuery]` indique que la valeur du paramètre `discontinuedOnly` est fournie dans la chaîne de requête de l’URL de demande :

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

Des règles d’inférence sont appliquées pour les sources de données par défaut des paramètres d’action. Ces règles configurent les sources de liaison que vous seriez susceptible d’appliquer manuellement aux paramètres d’action. Les attributs de source de liaison se comportent comme suit :

* **[FromBody]** est déduit des paramètres de type complexe. Une exception à cette règle est tout type complexe intégré ayant une signification spéciale, comme <xref:Microsoft.AspNetCore.Http.IFormCollection> et <xref:System.Threading.CancellationToken>. Le code de l’inférence de la source de liaison ignore ces types spéciaux. `[FromBody]` n’est pas déduit pour les types simples tels que `string` ou `int`. Vous devez donc utiliser l’attribut `[FromBody]` pour les types simples quand cette fonctionnalité est nécessaire. Quand une action a plusieurs paramètres explicitement spécifiés (par le biais de `[FromBody]`) ou déduits comme étant liés à partir du corps de la demande, une exception est levée. Par exemple, les signatures d’action suivantes génèrent une exception :

    [!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

    > [!NOTE]
    > Dans ASP.NET Core 2.1, les paramètres de type collection comme les listes et les tableaux sont déduits de manière incorrecte en tant que [[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute). [[FromBody] ](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) doit être utilisé pour ces paramètres s’ils doivent être liés à partir du corps de la requête. Ce problème est résolu dans ASP.NET Core 2.2 ou version ultérieure, où les paramètres de type collection sont déduits pour être liés à partir du corps par défaut.

* **[FromForm]** est déduit pour les paramètres d’action de type <xref:Microsoft.AspNetCore.Http.IFormFile> et <xref:Microsoft.AspNetCore.Http.IFormFileCollection>. Il n’est pas déduit pour les types simples ou définis par l’utilisateur.
* **[FromRoute]** est déduit pour tout nom de paramètre d’action correspondant à un paramètre dans le modèle d’itinéraire. Quand plus d’un itinéraire correspond à un paramètre d’action, toute valeur d’itinéraire est considérée comme `[FromRoute]`.
* **[FromQuery]** est déduit pour tous les autres paramètres d’action.

Les règles d’inférence par défaut sont désactivées quand la propriété <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> a la valeur `true`. Ajoutez le code suivant dans `Startup.ConfigureServices` après `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);` :

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=6)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="multipartform-data-request-inference"></a>Inférence de demande multipart/form-data

Quand un paramètre d’action est annoté avec l’attribut [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute), le type de contenu de demande `multipart/form-data` est déduit.

Le comportement par défaut est désactivé quand la propriété <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> a la valeur `true`.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Ajoutez le code suivant dans `Startup.ConfigureServices` :

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

Ajoutez le code suivant dans `Startup.ConfigureServices` après `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` :

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="attribute-routing-requirement"></a>Nécessité du routage d’attribut

Le routage d’attribut devient une nécessité. Par exemple :

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

Les actions sont inaccessibles par le biais de [routes conventionnelles](xref:mvc/controllers/routing#conventional-routing) définies dans <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> ou par <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> dans `Startup.Configure`.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="problem-details-responses-for-error-status-codes"></a>Réponses des détails des problèmes pour les codes d’état erreur

Dans ASP.NET Core 2.2 ou ultérieur, MVC transforme un résultat d’erreur (résultat avec un code d’état égal ou supérieur à 400) en résultat avec <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>. `ProblemDetails` est :

* Un type basé sur la [spécification RFC 7807](https://tools.ietf.org/html/rfc7807).
* Un format standardisé pour spécifier les détails des erreurs lisibles par machine dans une réponse HTTP.

Prenons le code suivant dans une action de contrôleur :

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Controllers/ProductsController.cs?name=snippet_ProblemDetailsStatusCode)]

La réponse HTTP pour `NotFound` a le code d’état 404 avec le corps `ProblemDetails`. Par exemple :

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

La fonctionnalité Détails du problème nécessite un indicateur de compatibilité de 2.2 ou ultérieur. Le comportement par défaut est désactivé quand la propriété `SuppressMapClientErrors` a la valeur `true`. Ajoutez le code suivant dans `Startup.ConfigureServices` :

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8)]

Utilisez la propriété `ClientErrorMapping` pour configurer le contenu de la réponse `ProblemDetails`. Par exemple, le code suivant met à jour la propriété `type` pour les réponses 404 :

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

::: moniker-end

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
