---
title: Créer des API web avec ASP.NET Core
author: scottaddie
description: Découvrez les fonctionnalités disponibles pour la création d’une API web dans ASP.NET Core et quand il convient d’utiliser chaque fonctionnalité.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/15/2018
uid: web-api/index
ms.openlocfilehash: d410f28ff7fda3bf33f73c06b3e626dfd4ee7dd8
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/22/2018
ms.locfileid: "41822138"
---
# <a name="build-web-apis-with-aspnet-core"></a>Créer des API web avec ASP.NET Core

Par [Scott Addie](https://github.com/scottaddie)

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([procédure de téléchargement](xref:tutorials/index#how-to-download-a-sample))

Ce document explique comment créer une API web ASP.NET Core et quand il convient le plus d’utiliser chaque fonctionnalité.

## <a name="derive-class-from-controllerbase"></a>Dériver la classe de ControllerBase

Héritez de la classe <xref:Microsoft.AspNetCore.Mvc.ControllerBase> dans un contrôleur destiné à servir d’API web. Exemple :

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

La classe `ControllerBase` fournit l’accès à de plusieurs propriétés et méthodes. Dans le code précédent, les exemples incluent <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> et <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>. Ces méthodes sont appelées dans les méthodes d’action pour retourner les codes d’état HTTP 400 et 201, respectivement. La propriété <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState>, également fournie par `ControllerBase`, permet de gérer la validation des modèles de requêtes.

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a>Annoter la classe avec ApiControllerAttribute

ASP.NET Core 2.1 introduit l’attribut [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) pour désigner une classe de contrôleur d’API web. Exemple :

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

Une version de compatibilité 2.1 ou ultérieure, définie via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, est obligatoire pour utiliser cet attribut. Par exemple, le code en surbrillance dans *Startup.ConfigureServices* définit l’indicateur de compatibilité 2.1 :

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

Pour plus d'informations, consultez <xref:mvc/compatibility-version>.

L’attribut `[ApiController]` est généralement associé à `ControllerBase` pour donner aux contrôleurs un comportement spécifique à REST. `ControllerBase` permet d’accéder aux méthodes telles que <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> et <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.

Une autre approche consiste à créer une classe de contrôleur de base personnalisée annotée avec l’attribut `[ApiController]` :

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

Les sections suivantes décrivent les fonctionnalités pratiques ajoutées par l’attribut.

### <a name="automatic-http-400-responses"></a>Réponses HTTP 400 automatiques

Les erreurs de validation déclenchent automatiquement une réponse HTTP 400. Le code suivant devient inutile dans vos actions :

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

Le comportement par défaut est désactivé quand la propriété <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> a la valeur `true`. Ajoutez le code suivant dans *Startup.ConfigureServices* après `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

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

Sans l’attribut `[ApiController]`, les attributs de source de liaison sont définis explicitement. Dans l’exemple suivant, l’attribut `[FromQuery]` indique que la valeur du paramètre `discontinuedOnly` est fournie dans la chaîne de requête de l’URL de demande :

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

Des règles d’inférence sont appliquées pour les sources de données par défaut des paramètres d’action. Ces règles configurent les sources de liaison que vous seriez susceptible d’appliquer manuellement aux paramètres d’action. Les attributs de source de liaison se comportent comme suit :

* **[FromBody]** est déduit des paramètres de type complexe. Une exception à cette règle est tout type complexe intégré ayant une signification spéciale, comme <xref:Microsoft.AspNetCore.Http.IFormCollection> et <xref:System.Threading.CancellationToken>. Le code de l’inférence de la source de liaison ignore ces types spéciaux. `[FromBody]` n’est pas déduit pour les types simples tels que `string` ou `int`. Par conséquent, vous devez utiliser l’attribut `[FromBody]` pour les types simples quand cette fonctionnalité est souhaitée. Quand une action a plusieurs paramètres explicitement spécifiés (par le biais de `[FromBody]`) ou déduits comme étant liés à partir du corps de la demande, une exception est levée. Par exemple, les signatures d’action suivantes génèrent une exception :

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* **[FromForm]** est déduit pour les paramètres d’action de type <xref:Microsoft.AspNetCore.Http.IFormFile> et <xref:Microsoft.AspNetCore.Http.IFormFileCollection>. Il n’est pas déduit pour les types simples ou définis par l’utilisateur.
* **[FromRoute]** est déduit pour tout nom de paramètre d’action correspondant à un paramètre dans le modèle d’itinéraire. Quand plus d’un itinéraire correspond à un paramètre d’action, toute valeur d’itinéraire est considérée comme `[FromRoute]`.
* **[FromQuery]** est déduit pour tous les autres paramètres d’action.

Les règles d’inférence par défaut sont désactivées quand la propriété <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> a la valeur `true`. Ajoutez le code suivant dans *Startup.ConfigureServices* après `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a>Inférence de demande multipart/form-data

Quand un paramètre d’action est annoté avec l’attribut [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute), le type de contenu de demande `multipart/form-data` est déduit.

Le comportement par défaut est désactivé quand la propriété <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> a la valeur `true`. Ajoutez le code suivant dans *Startup.ConfigureServices* après `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a>Exigence du routage d’attribut

Le routage d’attribut devient une exigence. Exemple :

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

Les actions sont inaccessibles par le biais des [itinéraires conventionnels](xref:mvc/controllers/routing#conventional-routing) définis dans <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> ou par <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> dans *Startup.Configure*.

::: moniker-end

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
