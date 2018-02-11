---
title: Tag Helper Environnement dans ASP.NET Core
author: pkellner
description: "Tag Helper Environnement ASP.NET Core défini avec toutes les propriétés"
manager: wpickett
ms.author: riande
ms.date: 07/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 7a99ee0e59c7f49a3208d2c86c11cabce4294889
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="8af8b-103">Tag Helper Environnement dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8af8b-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="8af8b-104">Par [Peter Kellner](http://peterkellner.net) et [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span><span class="sxs-lookup"><span data-stu-id="8af8b-104">By [Peter Kellner](http://peterkellner.net) and [Hisham Bin Ateya](https://twitter.com/hishambinateya)</span></span>

<span data-ttu-id="8af8b-105">Le Tag Helper Environnement restitue de façon conditionnelle son contenu joint en fonction de l’environnement d’hébergement actuel.</span><span class="sxs-lookup"><span data-stu-id="8af8b-105">The Environment Tag Helper conditionally renders its enclosed content based on the current hosting environment.</span></span> <span data-ttu-id="8af8b-106">Son seul attribut `names` est une liste séparée par des virgules de noms d’environnement qui, s’ils correspondent à l’environnement actuel, déclenchent l’affichage du contenu joint.</span><span class="sxs-lookup"><span data-stu-id="8af8b-106">Its single attribute `names` is a comma separated list of environment names, that if any match to the current environment, will trigger the enclosed content to be rendered.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="8af8b-107">Attributs de Tag Helper Environnement</span><span class="sxs-lookup"><span data-stu-id="8af8b-107">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="8af8b-108">noms</span><span class="sxs-lookup"><span data-stu-id="8af8b-108">names</span></span>

<span data-ttu-id="8af8b-109">Accepte un seul nom d’environnement d’hébergement ou une liste séparée par des virgules de noms d’environnement d’hébergement qui déclenchent l’affichage du contenu joint.</span><span class="sxs-lookup"><span data-stu-id="8af8b-109">Accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="8af8b-110">Ces valeurs sont comparées à la valeur actuelle retournée par la propriété statique ASP.NET Core `HostingEnvironment.EnvironmentName`.</span><span class="sxs-lookup"><span data-stu-id="8af8b-110">These value(s) are compared to the current value returned from the ASP.NET Core static property `HostingEnvironment.EnvironmentName`.</span></span>  <span data-ttu-id="8af8b-111">Cette valeur est l’une des suivantes : **Préproduction**, **Développement** ou **Production**.</span><span class="sxs-lookup"><span data-stu-id="8af8b-111">This value is one of the following: **Staging**; **Development** or **Production**.</span></span> <span data-ttu-id="8af8b-112">La comparaison ignore la casse.</span><span class="sxs-lookup"><span data-stu-id="8af8b-112">The comparison ignores case.</span></span>

<span data-ttu-id="8af8b-113">Un exemple de Tag Helper `environment` valide est :</span><span class="sxs-lookup"><span data-stu-id="8af8b-113">An example of a valid `environment` tag helper is:</span></span>

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="8af8b-114">Attributs include et exclude</span><span class="sxs-lookup"><span data-stu-id="8af8b-114">include and exclude attributes</span></span>

<span data-ttu-id="8af8b-115">ASP.NET Core 2.x ajoute les attributs `include` & `exclude`.</span><span class="sxs-lookup"><span data-stu-id="8af8b-115">ASP.NET Core 2.x adds the `include` & `exclude` attributes.</span></span> <span data-ttu-id="8af8b-116">Ces attributs contrôlent le rendu du contenu joint en fonction des noms d’environnement d’hébergement inclus ou exclus.</span><span class="sxs-lookup"><span data-stu-id="8af8b-116">These attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include-aspnet-core-20-and-later"></a><span data-ttu-id="8af8b-117">Propriété include ASP.NET Core 2.0 et ultérieur</span><span class="sxs-lookup"><span data-stu-id="8af8b-117">include ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="8af8b-118">La propriété `include` a un comportement semblable à l’attribut `names` dans ASP.NET Core 1.0.</span><span class="sxs-lookup"><span data-stu-id="8af8b-118">The `include` property has a similar behavior of the `names` attribute in ASP.NET Core 1.0.</span></span>

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a><span data-ttu-id="8af8b-119">Propriété exclude ASP.NET Core 2.0 et ultérieur</span><span class="sxs-lookup"><span data-stu-id="8af8b-119">exclude ASP.NET Core 2.0 and later</span></span>

<span data-ttu-id="8af8b-120">En revanche, la propriété `exclude` permet à `EnvironmentTagHelper` de restituer le contenu joint pour tous les noms d’environnement d’hébergement à l’exception de ceux que vous avez spécifiés.</span><span class="sxs-lookup"><span data-stu-id="8af8b-120">In contrast, the `exclude` property lets the `EnvironmentTagHelper` render the enclosed content for all hosting environment names except the one(s) that you specified.</span></span>

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a><span data-ttu-id="8af8b-121">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="8af8b-121">Additional resources</span></span>

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
