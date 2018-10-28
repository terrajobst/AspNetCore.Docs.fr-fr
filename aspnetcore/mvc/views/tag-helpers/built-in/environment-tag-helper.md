---
title: Tag Helper Environnement dans ASP.NET Core
author: pkellner
description: Tag Helper Environnement ASP.NET Core défini avec toutes les propriétés
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 379f58ed37329f047d53adf1dcfdfd2ad6a6ca4e
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325235"
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="dc6bc-103">Tag Helper Environnement dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dc6bc-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="dc6bc-104">Article rédigé par [Peter Kellner](http://peterkellner.net), [Hisham Bin Ateya](https://twitter.com/hishambinateya) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="dc6bc-104">By [Peter Kellner](http://peterkellner.net), [Hisham Bin Ateya](https://twitter.com/hishambinateya), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="dc6bc-105">Le Tag Helper Environnement affiche de façon conditionnelle son contenu joint en fonction de [l’environnement d’hébergement](xref:fundamentals/environments) actuel.</span><span class="sxs-lookup"><span data-stu-id="dc6bc-105">The Environment Tag Helper conditionally renders its enclosed content based on the current [hosting environment](xref:fundamentals/environments).</span></span> <span data-ttu-id="dc6bc-106">L’attribut unique du Tag Helper Environnement, `names`, est une liste séparée par des virgules de noms d’environnement.</span><span class="sxs-lookup"><span data-stu-id="dc6bc-106">The Environment Tag Helper's single attribute, `names`, is a comma-separated list of environment names.</span></span> <span data-ttu-id="dc6bc-107">Si l’un des noms d’environnement fournis correspond à l’environnement actuel, le contenu joint est affiché.</span><span class="sxs-lookup"><span data-stu-id="dc6bc-107">If any of the provided environment names match the current environment, the enclosed content is rendered.</span></span>

<span data-ttu-id="dc6bc-108">Pour avoir une vue d’ensemble des Tag Helpers, consultez <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="dc6bc-108">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="dc6bc-109">Attributs de Tag Helper Environnement</span><span class="sxs-lookup"><span data-stu-id="dc6bc-109">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="dc6bc-110">noms</span><span class="sxs-lookup"><span data-stu-id="dc6bc-110">names</span></span>

<span data-ttu-id="dc6bc-111">`names` accepte un seul nom d’environnement d’hébergement ou une liste séparée par des virgules de noms d’environnement d’hébergement qui déclenchent l’affichage du contenu joint.</span><span class="sxs-lookup"><span data-stu-id="dc6bc-111">`names` accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="dc6bc-112">Les valeurs d’environnement sont comparées à la valeur actuelle retournée par [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="dc6bc-112">Environment values are compared to the current value returned by [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*).</span></span> <span data-ttu-id="dc6bc-113">La comparaison ignore la casse.</span><span class="sxs-lookup"><span data-stu-id="dc6bc-113">The comparison ignores case.</span></span>

<span data-ttu-id="dc6bc-114">L’exemple suivant utilise un Tag Helper Environnement.</span><span class="sxs-lookup"><span data-stu-id="dc6bc-114">The following example uses an Environment Tag Helper.</span></span> <span data-ttu-id="dc6bc-115">Le contenu est affiché si l’environnement d’hébergement est un environnement de préproduction (Staging) ou de production :</span><span class="sxs-lookup"><span data-stu-id="dc6bc-115">The content is rendered if the hosting environment is Staging or Production:</span></span>

```cshtml
<environment names="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

::: moniker range=">= aspnetcore-2.0"

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="dc6bc-116">Attributs include et exclude</span><span class="sxs-lookup"><span data-stu-id="dc6bc-116">include and exclude attributes</span></span>

<span data-ttu-id="dc6bc-117">Les attributs `include` & `exclude` contrôlent l’affichage du contenu joint en fonction des noms d’environnement d’hébergement inclus ou exclus.</span><span class="sxs-lookup"><span data-stu-id="dc6bc-117">`include` & `exclude` attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include"></a><span data-ttu-id="dc6bc-118">include</span><span class="sxs-lookup"><span data-stu-id="dc6bc-118">include</span></span>

<span data-ttu-id="dc6bc-119">La propriété `include` présente un comportement similaire à l’attribut `names`.</span><span class="sxs-lookup"><span data-stu-id="dc6bc-119">The `include` property exhibits similar behavior to the `names` attribute.</span></span> <span data-ttu-id="dc6bc-120">Un environnement listé dans la valeur d’attribut `include` doit correspondre à l’environnement d’hébergement de l’application ([IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*)) pour afficher le contenu de la balise `<environment>`.</span><span class="sxs-lookup"><span data-stu-id="dc6bc-120">An environment listed in the `include` attribute value must match the app's hosting environment ([IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*)) to render the content of the `<environment>` tag.</span></span>

```cshtml
<environment include="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude"></a><span data-ttu-id="dc6bc-121">exclude</span><span class="sxs-lookup"><span data-stu-id="dc6bc-121">exclude</span></span>

<span data-ttu-id="dc6bc-122">Contrairement à l’attribut `include`, le contenu de la balise `<environment>` est affiché quand l’environnement d’hébergement ne correspond pas à un environnement listé dans la valeur d’attribut `exclude`.</span><span class="sxs-lookup"><span data-stu-id="dc6bc-122">In contrast to the `include` attribute, the content of the `<environment>` tag is rendered when the hosting environment doesn't match an environment listed in the `exclude` attribute value.</span></span>

```cshtml
<environment exclude="Development">
    <strong>HostingEnvironment.EnvironmentName is not Development</strong>
</environment>
```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="dc6bc-123">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="dc6bc-123">Additional resources</span></span>

* <xref:fundamentals/environments>
