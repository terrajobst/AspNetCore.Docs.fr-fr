---
title: "Tag Helpers intégrés d’ASP.NET Core"
author: pkellner
description: "Tag Helpers intégrés d’ASP.NET Core"
manager: wpickett
ms.author: riande
ms.date: 09/13/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: 09024bf0d6c87fce9eba9b70bebefa11d2ff0a44
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="aecb6-103">Tag Helpers intégrés d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aecb6-103">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="aecb6-104">Par [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="aecb6-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="aecb6-105">ASP.NET Core inclut de nombreux Tag Helpers intégrés permettant d’augmenter votre productivité.</span><span class="sxs-lookup"><span data-stu-id="aecb6-105">ASP.NET Core includes many built-in Tag Helpers to boost your productivity.</span></span> <span data-ttu-id="aecb6-106">Cette section donne une vue d’ensemble de ces Tag Helpers.</span><span class="sxs-lookup"><span data-stu-id="aecb6-106">This section provides an overview of the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="aecb6-107">Certains Tag Helpers intégrés ne sont pas abordés, car ils sont utilisés en interne par le moteur d’affichage [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="aecb6-107">There are built-in Tag Helpers which aren't discussed, since they're used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="aecb6-108">Cela inclut notamment un Tag Helper pour le caractère ~ qui développe le chemin racine du site web.</span><span class="sxs-lookup"><span data-stu-id="aecb6-108">This includes a Tag Helper for the ~ character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="aecb6-109">Tag Helpers intégrés d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aecb6-109">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="aecb6-110">**[Tag Helper d’ancre](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="aecb6-110">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="aecb6-111">**[Tag Helper de cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="aecb6-111">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="aecb6-112">**[Tag Helper de cache distribué](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="aecb6-112">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="aecb6-113">**[Tag Helper d’environnement](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="aecb6-113">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="aecb6-114">**[Tag Helper Form](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="aecb6-114">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="aecb6-115">**[Tag Helper d’image](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="aecb6-115">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="aecb6-116">**[Tag Helper Input](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="aecb6-116">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="aecb6-117">**[Tag Helper Label](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="aecb6-117">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="aecb6-118">**[Tag Helper Select](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="aecb6-118">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="aecb6-119">**[Tag Helper Textarea](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="aecb6-119">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="aecb6-120">**[Tag Helper de message de validation](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="aecb6-120">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="aecb6-121">**[Tag Helper de résumé de validation](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="aecb6-121">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="aecb6-122">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="aecb6-122">Additional resources</span></span>

* [<span data-ttu-id="aecb6-123">Développement côté client</span><span class="sxs-lookup"><span data-stu-id="aecb6-123">Client-Side Development</span></span>](xref:client-side/index)
* [<span data-ttu-id="aecb6-124">Tag Helpers</span><span class="sxs-lookup"><span data-stu-id="aecb6-124">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
