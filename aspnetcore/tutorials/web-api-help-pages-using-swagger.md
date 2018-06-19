---
title: Pages d’aide d’API web ASP.NET Core avec Swagger/Open API
author: rsuter
description: Ce tutoriel montre comment ajouter Swagger pour générer des pages d’aide et de documentation pour une application d’API web.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: e44b491fd5265e12646efa42f12eb0662e287f04
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
ms.locfileid: "30077334"
---
# <a name="aspnet-core-web-api-help-pages-with-swagger--open-api"></a><span data-ttu-id="94f57-103">Pages d’aide d’API web ASP.NET Core avec Swagger/Open API</span><span class="sxs-lookup"><span data-stu-id="94f57-103">ASP.NET Core Web API help pages with Swagger / Open API</span></span>

<span data-ttu-id="94f57-104">Par [Christoph Nienaber](https://twitter.com/zuckerthoben) et [Rico Suter](http://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="94f57-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](http://rsuter.com)</span></span>

<span data-ttu-id="94f57-105">Les différentes méthodes qui permettent d’utiliser une API web ne sont pas toujours simples à comprendre pour les développeurs.</span><span class="sxs-lookup"><span data-stu-id="94f57-105">When consuming a Web API, understanding its various methods can be challenging for a developer.</span></span> <span data-ttu-id="94f57-106">[Swagger](https://swagger.io/), également appelé Open API, résout le problème de la génération de pages d’aide et de documentation qu’utilisent les API web.</span><span class="sxs-lookup"><span data-stu-id="94f57-106">[Swagger](https://swagger.io/), also known as Open API, solves the problem of generating useful documentation and help pages for Web APIs.</span></span> <span data-ttu-id="94f57-107">Ses avantages sont, entre autres, la documentation interactive, la génération de SDK client et la découvertibilité des API.</span><span class="sxs-lookup"><span data-stu-id="94f57-107">It provides benefits such as interactive documentation, client SDK generation, and API discoverability.</span></span>

<span data-ttu-id="94f57-108">Cet article décrit les implémentations .NET Swagger suivantes [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) et [NSwag](https://github.com/RSuter/NSwag) :</span><span class="sxs-lookup"><span data-stu-id="94f57-108">In this article, the [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) and [NSwag](https://github.com/RSuter/NSwag) .NET Swagger implementations are showcased:</span></span>

* <span data-ttu-id="94f57-109">**Swashbuckle.AspNetCore** est un projet open source pour la génération de documents Swagger pour des API web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="94f57-109">**Swashbuckle.AspNetCore** is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="94f57-110">**NSwag** est un autre projet open source pour intégrer [l’IU Swagger](https://swagger.io/swagger-ui/) ou [ReDoc](https://github.com/Rebilly/ReDoc) aux API web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="94f57-110">**NSwag** is another open source project for integrating [Swagger UI](https://swagger.io/swagger-ui/) or [ReDoc](https://github.com/Rebilly/ReDoc) into ASP.NET Core Web APIs.</span></span> <span data-ttu-id="94f57-111">Il offre des méthodes pour générer du code client C# et TypeScript pour votre API.</span><span class="sxs-lookup"><span data-stu-id="94f57-111">It offers approaches to generate C# and TypeScript client code for your API.</span></span>

## <a name="what-is-swagger--open-api"></a><span data-ttu-id="94f57-112">Qu’est-ce que Swagger/Open API ?</span><span class="sxs-lookup"><span data-stu-id="94f57-112">What is Swagger / Open API?</span></span>

<span data-ttu-id="94f57-113">Swagger est une spécification indépendante du langage pour décrire les API [REST](https://en.wikipedia.org/wiki/Representational_state_transfer).</span><span class="sxs-lookup"><span data-stu-id="94f57-113">Swagger is a language-agnostic specification for describing [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) APIs.</span></span> <span data-ttu-id="94f57-114">Le projet Swagger a été donné au projet [OpenAPI Initiative](https://www.openapis.org/) et s’appelle maintenant Open API.</span><span class="sxs-lookup"><span data-stu-id="94f57-114">The Swagger project was donated to the [OpenAPI Initiative](https://www.openapis.org/), where it's now referred to as Open API.</span></span> <span data-ttu-id="94f57-115">Les deux noms sont utilisés indifféremment, mais Open API est préféré.</span><span class="sxs-lookup"><span data-stu-id="94f57-115">Both names are used interchangeably; however, Open API is preferred.</span></span> <span data-ttu-id="94f57-116">Il permet aux ordinateurs et aux utilisateurs de comprendre les fonctionnalités d’un service sans aucun accès direct à l’implémentation (code source, accès réseau, documentation).</span><span class="sxs-lookup"><span data-stu-id="94f57-116">It allows both computers and humans to understand the capabilities of a service without any direct access to the implementation (source code, network access, documentation).</span></span> <span data-ttu-id="94f57-117">L’un des objectifs est de limiter la quantité de travail nécessaire pour connecter des services dissociés.</span><span class="sxs-lookup"><span data-stu-id="94f57-117">One goal is to minimize the amount of work needed to connect disassociated services.</span></span> <span data-ttu-id="94f57-118">Un autre objectif est de réduire le temps nécessaire pour documenter un service avec précision.</span><span class="sxs-lookup"><span data-stu-id="94f57-118">Another goal is to reduce the amount of time needed to accurately document a service.</span></span>

## <a name="swagger-specification-swaggerjson"></a><span data-ttu-id="94f57-119">Spécification Swagger (swagger.json)</span><span class="sxs-lookup"><span data-stu-id="94f57-119">Swagger specification (swagger.json)</span></span>

<span data-ttu-id="94f57-120">Au cœur du flux Swagger se trouve la spécification Swagger, document nommé par défaut *swagger.json*.</span><span class="sxs-lookup"><span data-stu-id="94f57-120">The core to the Swagger flow is the Swagger specification&mdash;by default, a document named *swagger.json*.</span></span> <span data-ttu-id="94f57-121">Elle est générée par la chaîne d’outils Swagger (ou des implémentations tierces) en fonction de votre service.</span><span class="sxs-lookup"><span data-stu-id="94f57-121">It's generated by the Swagger tool chain (or third-party implementations of it) based on your service.</span></span> <span data-ttu-id="94f57-122">Elle décrit les fonctionnalités de votre API et comment y accéder avec HTTP.</span><span class="sxs-lookup"><span data-stu-id="94f57-122">It describes the capabilities of your API and how to access it with HTTP.</span></span> <span data-ttu-id="94f57-123">Elle gère l’IU Swagger et est utilisée par la chaîne d’outils pour activer la découverte et la génération de code client.</span><span class="sxs-lookup"><span data-stu-id="94f57-123">It drives the Swagger UI and is used by the tool chain to enable discovery and client code generation.</span></span> <span data-ttu-id="94f57-124">Voici l’exemple d’une spécification Swagger, réduite par souci de concision :</span><span class="sxs-lookup"><span data-stu-id="94f57-124">Here's an example of a Swagger specification, reduced for brevity:</span></span>

```json
{
   "swagger": "2.0",
   "info": {
       "version": "v1",
       "title": "API V1"
   },
   "basePath": "/",
   "paths": {
       "/api/Todo": {
           "get": {
               "tags": [
                   "Todo"
               ],
               "operationId": "ApiTodoGet",
               "consumes": [],
               "produces": [
                   "text/plain",
                   "application/json",
                   "text/json"
               ],
               "responses": {
                   "200": {
                       "description": "Success",
                       "schema": {
                           "type": "array",
                           "items": {
                               "$ref": "#/definitions/TodoItem"
                           }
                       }
                   }
                }
           },
           "post": {
               ...
           }
       },
       "/api/Todo/{id}": {
           "get": {
               ...
           },
           "put": {
               ...
           },
           "delete": {
               ...
   },
   "definitions": {
       "TodoItem": {
           "type": "object",
            "properties": {
                "id": {
                    "format": "int64",
                    "type": "integer"
                },
                "name": {
                    "type": "string"
                },
                "isComplete": {
                    "default": false,
                    "type": "boolean"
                }
            }
       }
   },
   "securityDefinitions": {}
}
```

## <a name="swagger-ui"></a><span data-ttu-id="94f57-125">Interface utilisateur de Swagger</span><span class="sxs-lookup"><span data-stu-id="94f57-125">Swagger UI</span></span>

<span data-ttu-id="94f57-126">[L’IU Swagger](https://swagger.io/swagger-ui/) est une interface utilisateur web qui fournit des informations sur le service, à l’aide de la spécification Swagger générée.</span><span class="sxs-lookup"><span data-stu-id="94f57-126">[Swagger UI](https://swagger.io/swagger-ui/) offers a web-based UI that provides information about the service, using the generated Swagger specification.</span></span> <span data-ttu-id="94f57-127">Swashbuckle et NSwag ont une version incorporée de l’IU Swagger pour que vous puissiez l’héberger dans votre application ASP.NET Core à l’aide d’un appel d’inscription d’intergiciel (middleware).</span><span class="sxs-lookup"><span data-stu-id="94f57-127">Both Swashbuckle and NSwag include an embedded version of Swagger UI, so that it can be hosted in your ASP.NET Core app using a middleware registration call.</span></span> <span data-ttu-id="94f57-128">L’IU web ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="94f57-128">The web UI looks like this:</span></span>

![Interface utilisateur de Swagger](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="94f57-130">Chaque méthode d’action publique dans vos contrôleurs peut être testée à partir de l’IU.</span><span class="sxs-lookup"><span data-stu-id="94f57-130">Each public action method in your controllers can be tested from the UI.</span></span> <span data-ttu-id="94f57-131">Cliquez sur un nom de méthode pour développer la section.</span><span class="sxs-lookup"><span data-stu-id="94f57-131">Click a method name to expand the section.</span></span> <span data-ttu-id="94f57-132">Ajoutez tous les paramètres nécessaires, puis cliquez sur **Try it out!**.</span><span class="sxs-lookup"><span data-stu-id="94f57-132">Add any necessary parameters, and click **Try it out!**.</span></span>

![Exemple de test GET Swagger](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

> [!NOTE]
> <span data-ttu-id="94f57-134">La version de l’IU Swagger utilisée pour les captures d’écran est la version 2.</span><span class="sxs-lookup"><span data-stu-id="94f57-134">The Swagger UI version used for the screenshots is version 2.</span></span> <span data-ttu-id="94f57-135">Pour obtenir un exemple de la version 3, consultez [l’exemple Petstore](http://petstore.swagger.io/).</span><span class="sxs-lookup"><span data-stu-id="94f57-135">For a version 3 example, see [Petstore example](http://petstore.swagger.io/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="94f57-136">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="94f57-136">Next steps</span></span>

* [<span data-ttu-id="94f57-137">Bien démarrer avec Swashbuckle</span><span class="sxs-lookup"><span data-stu-id="94f57-137">Get started with Swashbuckle</span></span>](xref:tutorials/get-started-with-swashbuckle)
* [<span data-ttu-id="94f57-138">Bien démarrer avec NSwag</span><span class="sxs-lookup"><span data-stu-id="94f57-138">Get started with NSwag</span></span>](xref:tutorials/get-started-with-nswag)
