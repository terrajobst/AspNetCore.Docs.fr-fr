---
title: Tag Helpers intégrés d’ASP.NET Core
author: pkellner
description: Découvrez comment les Tag Helpers intégrés d’ASP.NET Core augmentent votre productivité.
ms.author: riande
ms.date: 09/18/2018
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: 5d9425e0b68578c86a6f9a691322e0bb63a860fb
ms.sourcegitcommit: c684eb6c0999d11d19e15e65939e5c7f99ba47df
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46292308"
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="76e89-103">Tag Helpers intégrés d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="76e89-103">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="76e89-104">Par [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="76e89-104">By [Peter Kellner](http://peterkellner.net)</span></span>

<span data-ttu-id="76e89-105">ASP.NET Core inclut de nombreux Tag Helpers intégrés permettant d’augmenter votre productivité.</span><span class="sxs-lookup"><span data-stu-id="76e89-105">ASP.NET Core includes many built-in Tag Helpers to boost your productivity.</span></span> <span data-ttu-id="76e89-106">Cette section donne une vue d’ensemble de ces Tag Helpers.</span><span class="sxs-lookup"><span data-stu-id="76e89-106">This section provides an overview of the built-in Tag Helpers.</span></span>

> [!NOTE]
> <span data-ttu-id="76e89-107">Certains Tag Helpers intégrés ne sont pas abordés, car ils sont utilisés en interne par le moteur d’affichage [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="76e89-107">There are built-in Tag Helpers which aren't discussed, since they're used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="76e89-108">Cela inclut notamment un Tag Helper pour le caractère ~ qui développe le chemin racine du site web.</span><span class="sxs-lookup"><span data-stu-id="76e89-108">This includes a Tag Helper for the ~ character, which expands to the root path of the website.</span></span>

## <a name="built-in-aspnet-core-tag-helpers"></a><span data-ttu-id="76e89-109">Tag Helpers intégrés d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="76e89-109">Built-in ASP.NET Core Tag Helpers</span></span>

<span data-ttu-id="76e89-110">**[Tag Helper d’ancre](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="76e89-110">**[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)**</span></span>

<span data-ttu-id="76e89-111">**[Tag Helper de cache](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="76e89-111">**[Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)**</span></span>

<span data-ttu-id="76e89-112">**[Tag Helper de cache distribué](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="76e89-112">**[Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)**</span></span>

<span data-ttu-id="76e89-113">**[Tag Helper d’environnement](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="76e89-113">**[Environment Tag Helper](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)**</span></span>

[comment]: **[FormActionTagHelper](xref:mvc/views/tag-helpers/builtin-th/form-action-tag-helper)**

<span data-ttu-id="76e89-114">**[Tag Helper Form](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="76e89-114">**[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper)**</span></span>

<span data-ttu-id="76e89-115">**[Tag Helper d’image](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="76e89-115">**[Image Tag Helper](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)**</span></span>

<span data-ttu-id="76e89-116">**[Tag Helper Input](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="76e89-116">**[Input Tag Helper](xref:mvc/views/working-with-forms#the-input-tag-helper)**</span></span>

<span data-ttu-id="76e89-117">**[Tag Helper Label](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="76e89-117">**[Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper)**</span></span>

[comment]: **[LinkTagHelper](xref:mvc/views/tag-helpers/builtin-th/link-tag-helper)**

[comment]: **[OptionTagHelper](xref:mvc/views/tag-helpers/builtin-th/option-tag-helper)**

[comment]: **[ScriptTagHelper](xref:mvc/views/tag-helpers/builtin-th/script-tag-helper)**

<span data-ttu-id="76e89-118">**[Tag Helper Partial](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="76e89-118">**[Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)**</span></span>

<span data-ttu-id="76e89-119">**[Tag Helper Select](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="76e89-119">**[Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper)**</span></span>

<span data-ttu-id="76e89-120">**[Tag Helper Textarea](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="76e89-120">**[Textarea Tag Helper](xref:mvc/views/working-with-forms#the-textarea-tag-helper)**</span></span>

<span data-ttu-id="76e89-121">**[Tag Helper de message de validation](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="76e89-121">**[Validation Message Tag Helper](xref:mvc/views/working-with-forms#the-validation-message-tag-helper)**</span></span>

<span data-ttu-id="76e89-122">**[Tag Helper de résumé de validation](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span><span class="sxs-lookup"><span data-stu-id="76e89-122">**[Validation Summary Tag Helper](xref:mvc/views/working-with-forms#the-validation-summary-tag-helper)**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="76e89-123">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="76e89-123">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/th-components>
