---
title: Utiliser les conventions d’API web
author: pranavkm
description: Découvrez plus d’informations sur les conventions d’API web dans ASP.NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 11/13/2018
uid: web-api/advanced/conventions
ms.openlocfilehash: ede9a46c160cf6a49aa93da710af0bf0b8f59acc
ms.sourcegitcommit: c4572be5ebb301013a5698caf9b5572b76cb2e34
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52710073"
---
# <a name="use-web-api-conventions"></a><span data-ttu-id="d8c95-103">Utiliser les conventions d’API web</span><span class="sxs-lookup"><span data-stu-id="d8c95-103">Use web API conventions</span></span>

<span data-ttu-id="d8c95-104">ASP.NET Core 2.2 introduit un moyen d’extraire la [documentation des API](xref:tutorials/web-api-help-pages-using-swagger) courantes et de l’appliquer à plusieurs actions, à plusieurs contrôleurs ou à tous les contrôleurs au sein d’un assembly.</span><span class="sxs-lookup"><span data-stu-id="d8c95-104">ASP.NET Core 2.2 introduces a way to extract common [API documentation](xref:tutorials/web-api-help-pages-using-swagger) and apply it to multiple actions, controllers, or all controllers within an assembly.</span></span> <span data-ttu-id="d8c95-105">Les conventions des API web sont un substitut permettant de décorer des actions individuelles avec [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).</span><span class="sxs-lookup"><span data-stu-id="d8c95-105">Web API conventions are a substitute for decorating individual actions with [[ProducesResponseType]](xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute).</span></span> <span data-ttu-id="d8c95-106">Elles vous permettent de définir les types de retour « conventionnels » les plus courants et les codes d’état retournés depuis votre action avec un moyen de sélectionner la méthode de convention qui s’applique à une action.</span><span class="sxs-lookup"><span data-stu-id="d8c95-106">It allows you to define the most common "conventional" return types and status codes that you return from your action with a way to select the convention method that applies to an action.</span></span>

<span data-ttu-id="d8c95-107">Par défaut, ASP.NET Core MVC 2.2 est livré avec un ensemble de conventions par défaut, `Microsoft.AspNetCore.Mvc.DefaultApiConventions`.</span><span class="sxs-lookup"><span data-stu-id="d8c95-107">By default, ASP.NET Core MVC 2.2 ships with a set of default conventions, `Microsoft.AspNetCore.Mvc.DefaultApiConventions`.</span></span> <span data-ttu-id="d8c95-108">Les conventions sont basées sur le contrôleur généré automatiquement comme modèle par ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d8c95-108">The conventions are based on the controller that ASP.NET Core scaffolds.</span></span> <span data-ttu-id="d8c95-109">Si vos actions suivent le modèle produit par cette génération de modèles automatique, vous devez normalement réussir à utiliser les conventions par défaut.</span><span class="sxs-lookup"><span data-stu-id="d8c95-109">If your actions follow the pattern that scaffolding produces, you should be successful using the default conventions.</span></span>

<span data-ttu-id="d8c95-110">À l’exécution, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> comprend les conventions.</span><span class="sxs-lookup"><span data-stu-id="d8c95-110">At runtime, <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> understands conventions.</span></span> <span data-ttu-id="d8c95-111">`ApiExplorer` est l’abstraction de MVC pour communiquer avec des générateurs de documents d’API ouvertes.</span><span class="sxs-lookup"><span data-stu-id="d8c95-111">`ApiExplorer` is MVC's abstraction to communicate with Open API document generators.</span></span> <span data-ttu-id="d8c95-112">Les attributs provenant de la convention appliquée sont associés à une action et sont inclus dans la documentation Swagger de l’action.</span><span class="sxs-lookup"><span data-stu-id="d8c95-112">Attributes from the applied convention get associated with an action and are included in the action's Swagger documentation.</span></span> <span data-ttu-id="d8c95-113">Les analyseurs d’API comprennent aussi les conventions.</span><span class="sxs-lookup"><span data-stu-id="d8c95-113">API analyzers also understand conventions.</span></span> <span data-ttu-id="d8c95-114">Si votre action n’est pas conventionnelle (par exemple, elle retourne un code d’état qui n’est pas documenté par la convention appliquée), elle produit un avertissement, qui vous encourage à le documenter.</span><span class="sxs-lookup"><span data-stu-id="d8c95-114">If your action is unconventional (for example, it returns a status code that isn't documented by the applied convention), it produces a warning, encouraging you to document it.</span></span>

<span data-ttu-id="d8c95-115">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d8c95-115">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/conventions/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="apply-web-api-conventions"></a><span data-ttu-id="d8c95-116">Appliquer des conventions d’API web</span><span class="sxs-lookup"><span data-stu-id="d8c95-116">Apply web API conventions</span></span>

<span data-ttu-id="d8c95-117">Il existe trois façons d’appliquer une convention.</span><span class="sxs-lookup"><span data-stu-id="d8c95-117">There are three ways to apply a convention.</span></span> <span data-ttu-id="d8c95-118">Les conventions ne se combinent pas :</span><span class="sxs-lookup"><span data-stu-id="d8c95-118">Conventions don't compose.</span></span> <span data-ttu-id="d8c95-119">chaque action peut être associée à une seule convention.</span><span class="sxs-lookup"><span data-stu-id="d8c95-119">Each action may be associated with exactly one convention.</span></span> <span data-ttu-id="d8c95-120">Les conventions plus spécifiques (détaillées ci-dessous) sont prioritaires par rapport à celles qui sont moins spécifiques.</span><span class="sxs-lookup"><span data-stu-id="d8c95-120">More specific conventions (detailed below) take precedence over less specific ones.</span></span> <span data-ttu-id="d8c95-121">La sélection est non déterministe quand plusieurs conventions de même priorité s’appliquent à une action.</span><span class="sxs-lookup"><span data-stu-id="d8c95-121">The selection is non-deterministic when two or more conventions of the same priority apply to an action.</span></span> <span data-ttu-id="d8c95-122">Les options suivantes sont disponibles pour appliquer une convention à une action, de la plus spécifique à la moins spécifique :</span><span class="sxs-lookup"><span data-stu-id="d8c95-122">The following options exist to apply a convention to an action, from the most specific to the least specific:</span></span>

1. <span data-ttu-id="d8c95-123">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; s’applique à des actions individuelles, et spécifie le type de convention et la méthode de convention qui s’applique.</span><span class="sxs-lookup"><span data-stu-id="d8c95-123">`Microsoft.AspNetCore.Mvc.ApiConventionMethodAttribute` &mdash; Applies to individual actions and specifies the convention type and the convention method that applies.</span></span> <span data-ttu-id="d8c95-124">Dans l’exemple suivant, la méthode de convention `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` est appliquée à l’action `Update` :</span><span class="sxs-lookup"><span data-stu-id="d8c95-124">In the following sample, the convention method `Microsoft.AspNetCore.Mvc.DefaultApiConventions.Put` is applied to the `Update` action:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventionmethod&highlight=2-3)]

1. <span data-ttu-id="d8c95-125">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` appliqué à un contrôleur &mdash; applique le type de convention à toutes les actions sur le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="d8c95-125">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to a controller &mdash; Applies the convention type to all actions on the controller.</span></span> <span data-ttu-id="d8c95-126">Les méthodes de convention sont décorées avec des indicateurs qui déterminent les actions auxquelles elles s’appliquent (détails dans le cadre des conventions de création).</span><span class="sxs-lookup"><span data-stu-id="d8c95-126">Convention methods are decorated with hints that determine which actions it would apply to (details as part of authoring conventions).</span></span> <span data-ttu-id="d8c95-127">Exemple :</span><span class="sxs-lookup"><span data-stu-id="d8c95-127">For example:</span></span>

    [!code-csharp[](conventions/sample/Controllers/ContactsConventionController.cs?name=apiconventiontypeattribute)]

1. <span data-ttu-id="d8c95-128">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` appliqué à un assembly &mdash; applique le type de convention à tous les contrôleurs dans l’assembly actif.</span><span class="sxs-lookup"><span data-stu-id="d8c95-128">`Microsoft.AspNetCore.Mvc.ApiConventionTypeAttribute` applied to an assembly &mdash; Applies the convention type to all controllers in the current assembly.</span></span> <span data-ttu-id="d8c95-129">Exemple :</span><span class="sxs-lookup"><span data-stu-id="d8c95-129">For example:</span></span>

    [!code-csharp[](conventions/sample/Startup.cs?name=apiconventiontypeattribute)]

## <a name="create-web-api-conventions"></a><span data-ttu-id="d8c95-130">Créer des conventions d’API web</span><span class="sxs-lookup"><span data-stu-id="d8c95-130">Create web API conventions</span></span>

<span data-ttu-id="d8c95-131">Une convention est un type statique avec des méthodes.</span><span class="sxs-lookup"><span data-stu-id="d8c95-131">A convention is a static type with methods.</span></span> <span data-ttu-id="d8c95-132">Ces méthodes sont annotées avec des attributs `[ProducesResponseType]` ou `[ProducesDefaultResponseType]`.</span><span class="sxs-lookup"><span data-stu-id="d8c95-132">These methods are annotated with `[ProducesResponseType]` or `[ProducesDefaultResponseType]` attributes.</span></span>

```csharp
public static class MyAppConventions
{
    [ProducesResponseType(200)]
    [ProducesResponseType(404)]
    public static void Find(int id)
    {
    }
}
```

<span data-ttu-id="d8c95-133">L’application de cette convention à un assembly fait que la méthode de convention est appliquée aux actions ayant le nom `Find` et avec un seul paramètre nommé `id`, dès lors qu’elles n’ont pas d’autres attributs de métadonnées plus spécifiques.</span><span class="sxs-lookup"><span data-stu-id="d8c95-133">Applying this convention to an assembly results in the convention method applying to any action with the name `Find` and having exactly one parameter named `id`, as long as they don't have other more specific metadata attributes.</span></span>

<span data-ttu-id="d8c95-134">En plus de `[ProducesResponseType]` et de `[ProducesDefaultResponseType]`, `[ApiConventionNameMatch]` et `[ApiConventionTypeMatch]` peuvent être appliquées à la méthode de convention qui détermine les méthodes auxquelles elles s’appliquent.</span><span class="sxs-lookup"><span data-stu-id="d8c95-134">In addition to `[ProducesResponseType]` and `[ProducesDefaultResponseType]`, `[ApiConventionNameMatch]` and `[ApiConventionTypeMatch]` can be applied to the convention method that determines the methods they apply to.</span></span> <span data-ttu-id="d8c95-135">Exemple :</span><span class="sxs-lookup"><span data-stu-id="d8c95-135">For example:</span></span>

```csharp
[ProducesResponseType(200)]
[ProducesResponseType(404)]
[ApiConventionNameMatch(ApiConventionNameMatchBehavior.Prefix)]
public static void Find(
    [ApiConventionNameMatch(ApiConventionNameMatchBehavior.Suffix)]
    int id)
{ }
```

* <span data-ttu-id="d8c95-136">L’option `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` appliquée à la méthode indique que la convention peut correspondre à n’importe action dès lors qu’elle est préfixée par « Find ».</span><span class="sxs-lookup"><span data-stu-id="d8c95-136">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Prefix` option applied to the method, indicates that the convention can match any action as long as it's prefixed with "Find".</span></span> <span data-ttu-id="d8c95-137">Ceci inclut des méthodes comme `Find`, `FindPet` ou `FindById`.</span><span class="sxs-lookup"><span data-stu-id="d8c95-137">This includes methods such as `Find`, `FindPet`, or `FindById`.</span></span>
* <span data-ttu-id="d8c95-138">`Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` appliqué au paramètre indique que la convention peut correspondre aux méthodes avec un seul paramètre se terminant par l’identificateur du suffixe.</span><span class="sxs-lookup"><span data-stu-id="d8c95-138">The `Microsoft.AspNetCore.Mvc.ApiExplorer.ApiConventionNameMatchBehavior.Suffix` applied to the parameter indicates that the convention can match methods with exactly one parameter ending in the suffix identifier.</span></span> <span data-ttu-id="d8c95-139">Ceci inclut des paramètres comme `id` ou `petId`.</span><span class="sxs-lookup"><span data-stu-id="d8c95-139">This includes parameters such as `id` or `petId`.</span></span> <span data-ttu-id="d8c95-140">`ApiConventionTypeMatch` peut être appliqué de façon similaire à des types pour contraindre le type du paramètre.</span><span class="sxs-lookup"><span data-stu-id="d8c95-140">`ApiConventionTypeMatch` can be similarly applied to types to constrain the type of the parameter.</span></span> <span data-ttu-id="d8c95-141">Un argument `params[]` peut être utilisé afin d’indiquer les paramètres restants pour lesquels une correspondance explicite n’est pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="d8c95-141">A `params[]` argument can be used to indicate remaining parameters that don't need to be explicitly matched.</span></span>
