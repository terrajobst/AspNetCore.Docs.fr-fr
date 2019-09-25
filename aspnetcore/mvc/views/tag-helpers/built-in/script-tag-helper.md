---
title: Tag Helper de script dans ASP.NET Core
author: rick-anderson
ms.author: riande
description: Découvrez les attributs d’assistance de balise de script ASP.NET Core et le rôle joué par chaque attribut lors de l’extension du comportement de la balise de script HTML.
ms.custom: mvc
ms.date: 12/18/2018
uid: mvc/views/tag-helpers/builtin-th/script-tag-helper
ms.openlocfilehash: 5f2fb8a45048804afa8aff2989cd53489e45a33b
ms.sourcegitcommit: fae6f0e253f9d62d8f39de5884d2ba2b4b2a6050
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71256497"
---
# <a name="script-tag-helper-in-aspnet-core"></a><span data-ttu-id="b68aa-103">Tag Helper de script dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b68aa-103">Script Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="b68aa-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b68aa-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b68aa-105">Le [tag Helper script](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) génère un lien vers un fichier de script principal ou de retour.</span><span class="sxs-lookup"><span data-stu-id="b68aa-105">The [Script Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) generates a link to a primary or fall back script file.</span></span> <span data-ttu-id="b68aa-106">En général, le fichier de script principal se trouve sur un [réseau de distribution de contenu](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).</span><span class="sxs-lookup"><span data-stu-id="b68aa-106">Typically the primary script file is on a [Content Delivery Network](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).</span></span>

[!INCLUDE[](~/includes/cdn.md)]

<span data-ttu-id="b68aa-107">Le tag Helper script vous permet de spécifier un CDN pour le fichier de script et un secours lorsque le CDN n’est pas disponible.</span><span class="sxs-lookup"><span data-stu-id="b68aa-107">The Script Tag Helper allows you to specify a CDN for the script file and a fallback when the CDN is not available.</span></span> <span data-ttu-id="b68aa-108">Le tag Helper script fournit l’avantage en termes de performances d’un CDN avec la robustesse de l’hébergement local.</span><span class="sxs-lookup"><span data-stu-id="b68aa-108">The Script Tag Helper provides the performance advantage of a CDN with the robustness of local hosting.</span></span>

<span data-ttu-id="b68aa-109">Le balisage Razor suivant montre `script` l’élément d’un fichier de disposition créé avec le modèle d’application Web ASP.net Core :</span><span class="sxs-lookup"><span data-stu-id="b68aa-109">The following Razor markup shows the `script` element of a layout file created with the ASP.NET Core web app template:</span></span>

[!code-html[](link-tag-helper/sample/_Layout.cshtml?name=snippet2)]

<span data-ttu-id="b68aa-110">Les éléments suivants sont similaires au code HTML rendu du code précédent (dans un environnement de non-développement) :</span><span class="sxs-lookup"><span data-stu-id="b68aa-110">The following is similar to the rendered HTML from the preceding code (in a non-Development environment):</span></span>

[!code-csharp[](link-tag-helper/sample/HtmlPage2.html)]

<span data-ttu-id="b68aa-111">Dans le code précédent, le tag Helper script générait le deuxième élément script `<script>  (window.jQuery || document.write(`(), qui `window.jQuery`teste.</span><span class="sxs-lookup"><span data-stu-id="b68aa-111">In the preceding code, the Script Tag Helper generated the second script ( `<script>  (window.jQuery || document.write(`) element, which tests for `window.jQuery`.</span></span> <span data-ttu-id="b68aa-112">Si `window.jQuery` est introuvable `document.write(` , exécute et crée un script</span><span class="sxs-lookup"><span data-stu-id="b68aa-112">If `window.jQuery` is not found, `document.write(` runs and creates a script</span></span> 

## <a name="commonly-used-script-tag-helper-attributes"></a><span data-ttu-id="b68aa-113">Attributs d’assistance de balise de script couramment utilisés</span><span class="sxs-lookup"><span data-stu-id="b68aa-113">Commonly used Script Tag Helper attributes</span></span>

<span data-ttu-id="b68aa-114">Consultez [l’aide de balises de script](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) pour obtenir tous les attributs, propriétés et méthodes du programme d’assistance des balises de script.</span><span class="sxs-lookup"><span data-stu-id="b68aa-114">See [Script Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) for all the Script Tag Helper attributes, properties, and methods.</span></span>

### <a name="href"></a><span data-ttu-id="b68aa-115">href</span><span class="sxs-lookup"><span data-stu-id="b68aa-115">href</span></span>

<span data-ttu-id="b68aa-116">Adresse préférée de la ressource liée.</span><span class="sxs-lookup"><span data-stu-id="b68aa-116">Preferred address of the linked resource.</span></span> <span data-ttu-id="b68aa-117">Dans tous les cas, l’adresse est passée au code HTML généré.</span><span class="sxs-lookup"><span data-stu-id="b68aa-117">The address is passed thought to the generated HTML in all cases.</span></span>

### <a name="asp-fallback-href"></a><span data-ttu-id="b68aa-118">ASP-Fallback-href</span><span class="sxs-lookup"><span data-stu-id="b68aa-118">asp-fallback-href</span></span>

<span data-ttu-id="b68aa-119">URL d’une feuille de style CSS vers laquelle effectuer le secours dans le cas où l’URL principale échoue.</span><span class="sxs-lookup"><span data-stu-id="b68aa-119">The URL of a CSS stylesheet to fallback to in the case the primary URL fails.</span></span>

### <a name="asp-fallback-test-class"></a><span data-ttu-id="b68aa-120">ASP-Fallback-test-Class</span><span class="sxs-lookup"><span data-stu-id="b68aa-120">asp-fallback-test-class</span></span>

<span data-ttu-id="b68aa-121">Nom de classe défini dans la feuille de style à utiliser pour le test de secours.</span><span class="sxs-lookup"><span data-stu-id="b68aa-121">The class name defined in the stylesheet to use for the fallback test.</span></span> <span data-ttu-id="b68aa-122">Pour plus d'informations, consultez <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestClass>.</span><span class="sxs-lookup"><span data-stu-id="b68aa-122">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestClass>.</span></span>

### <a name="asp-fallback-test-property"></a><span data-ttu-id="b68aa-123">ASP-Fallback-test-propriété</span><span class="sxs-lookup"><span data-stu-id="b68aa-123">asp-fallback-test-property</span></span>

<span data-ttu-id="b68aa-124">Nom de la propriété CSS à utiliser pour le test de secours.</span><span class="sxs-lookup"><span data-stu-id="b68aa-124">The CSS property name to use for the fallback test.</span></span> <span data-ttu-id="b68aa-125">Pour plus d'informations, consultez <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestProperty>.</span><span class="sxs-lookup"><span data-stu-id="b68aa-125">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestProperty>.</span></span>

### <a name="asp-fallback-test-value"></a><span data-ttu-id="b68aa-126">ASP-Fallback-test-value</span><span class="sxs-lookup"><span data-stu-id="b68aa-126">asp-fallback-test-value</span></span>

<span data-ttu-id="b68aa-127">Valeur de propriété CSS à utiliser pour le test de secours.</span><span class="sxs-lookup"><span data-stu-id="b68aa-127">The CSS property value to use for the fallback test.</span></span> <span data-ttu-id="b68aa-128">Pour plus d'informations, consultez <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.</span><span class="sxs-lookup"><span data-stu-id="b68aa-128">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.</span></span>

### <a name="asp-fallback-test-value"></a><span data-ttu-id="b68aa-129">ASP-Fallback-test-value</span><span class="sxs-lookup"><span data-stu-id="b68aa-129">asp-fallback-test-value</span></span>

<span data-ttu-id="b68aa-130">Valeur de propriété CSS à utiliser pour le test de secours.</span><span class="sxs-lookup"><span data-stu-id="b68aa-130">The CSS property value to use for the fallback test.</span></span> <span data-ttu-id="b68aa-131">Pour plus d'informations, consultez <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue></span><span class="sxs-lookup"><span data-stu-id="b68aa-131">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue></span></span>

## <a name="additional-resources"></a><span data-ttu-id="b68aa-132">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="b68aa-132">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
