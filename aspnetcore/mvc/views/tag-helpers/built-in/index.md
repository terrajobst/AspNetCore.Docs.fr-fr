---
title: Tag Helpers intégrés d’ASP.NET Core
author: pkellner
description: Découvrez comment les Tag Helpers intégrés d’ASP.NET Core augmentent votre productivité.
ms.author: riande
ms.date: 09/13/2017
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: 4f2ebf1600f42847db1c1f9517787b020d2e86c9
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279166"
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="fc3b9-103">Tag Helpers intégrés d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fc3b9-103">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="fc3b9-104">Par [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="fc3b9-104">By [Peter Kellner](http://peterkellner.net)</span></span>

<span data-ttu-id="fc3b9-105">ASP.NET Core inclut de nombreux Tag Helpers intégrés permettant d’augmenter votre productivité.</span><span class="sxs-lookup"><span data-stu-id="fc3b9-105">ASP.NET Core includes many built-in Tag Helpers to boost your productivity.</span></span> <span data-ttu-id="fc3b9-106">Cette section donne une vue d’ensemble de ces Tag Helpers.</span><span class="sxs-lookup"><span data-stu-id="fc3b9-106">This section provides an overview of the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="fc3b9-107">Certains Tag Helpers intégrés ne sont pas abordés, car ils sont utilisés en interne par le moteur d’affichage [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="fc3b9-107">There are built-in Tag Helpers which aren't discussed, since they're used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="fc3b9-108">Cela inclut notamment un Tag Helper pour le caractère ~ qui développe le chemin racine du site web.</span><span class="sxs-lookup"><span data-stu-id="fc3b9-108">This includes a Tag Helper for the ~ character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="fc3b9-109">Tag Helpers intégrés d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fc3b9-109">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="fc3b9-110">**[Tag Helper d’ancre](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="fc3b9-110">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="fc3b9-111">**[Tag Helper de cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="fc3b9-111">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="fc3b9-112">**[Tag Helper de cache distribué](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="fc3b9-112">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="fc3b9-113">**[Tag Helper d’environnement](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="fc3b9-113">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="fc3b9-114">**[Tag Helper Form](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="fc3b9-114">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="fc3b9-115">**[Tag Helper d’image](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="fc3b9-115">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="fc3b9-116">**[Tag Helper Input](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="fc3b9-116">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="fc3b9-117">**[Tag Helper Label](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="fc3b9-117">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="fc3b9-118">**[Tag Helper Partial](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="fc3b9-118">**[Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span></span>

<span data-ttu-id="fc3b9-119">**[Tag Helper Select](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="fc3b9-119">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="fc3b9-120">**[Tag Helper Textarea](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="fc3b9-120">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="fc3b9-121">**[Tag Helper de message de validation](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="fc3b9-121">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="fc3b9-122">**[Tag Helper de résumé de validation](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="fc3b9-122">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fc3b9-123">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="fc3b9-123">Additional resources</span></span>

* [<span data-ttu-id="fc3b9-124">Développement côté client</span><span class="sxs-lookup"><span data-stu-id="fc3b9-124">Client-side development</span></span>](xref:client-side/index)
* [<span data-ttu-id="fc3b9-125">Les Tag Helpers</span><span class="sxs-lookup"><span data-stu-id="fc3b9-125">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
