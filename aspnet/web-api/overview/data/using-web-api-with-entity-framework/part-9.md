---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Ajouter un nouvel élément à la base de données | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 01586ef6eecdbe1acfadfcfcc0c9ca8b442de2d3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837426"
---
<a name="add-a-new-item-to-the-database"></a><span data-ttu-id="51abe-102">Ajouter un nouvel élément à la base de données</span><span class="sxs-lookup"><span data-stu-id="51abe-102">Add a New Item to the Database</span></span>
====================
<span data-ttu-id="51abe-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="51abe-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="51abe-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="51abe-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="51abe-105">Dans cette section, vous allez ajouter la possibilité aux utilisateurs de créer un nouveau livre.</span><span class="sxs-lookup"><span data-stu-id="51abe-105">In this section, you will add the ability for users to create a new book.</span></span> <span data-ttu-id="51abe-106">Dans app.js, ajoutez le code suivant au modèle de vue :</span><span class="sxs-lookup"><span data-stu-id="51abe-106">In app.js, add the following code to the view model:</span></span>

[!code-javascript[Main](part-9/samples/sample1.js)]

<span data-ttu-id="51abe-107">Dans Index.cshtml, remplacez le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="51abe-107">In Index.cshtml, replace the following markup:</span></span>

[!code-html[Main](part-9/samples/sample2.html)]

<span data-ttu-id="51abe-108">Par :</span><span class="sxs-lookup"><span data-stu-id="51abe-108">With:</span></span>

[!code-html[Main](part-9/samples/sample3.html)]

<span data-ttu-id="51abe-109">Ce balisage crée un formulaire pour l’envoi d’un auteur de nouveau.</span><span class="sxs-lookup"><span data-stu-id="51abe-109">This markup creates a form for submitting a new author.</span></span> <span data-ttu-id="51abe-110">Les valeurs de la liste déroulante auteur sont liés aux données à le `authors` observable dans le modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="51abe-110">The values for the author drop-down list are data-bound to the `authors` observable in the view model.</span></span> <span data-ttu-id="51abe-111">Pour les autres entrées de formulaire, les valeurs sont liés aux données à le `newBook` propriété du modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="51abe-111">For the other form inputs, the values are data-bound to the `newBook` property of the view model.</span></span>

<span data-ttu-id="51abe-112">Le Gestionnaire d’envoi du formulaire est lié à la `addBook` (fonction) :</span><span class="sxs-lookup"><span data-stu-id="51abe-112">The submit handler on the form is bound to the `addBook` function:</span></span>

[!code-html[Main](part-9/samples/sample4.html)]

<span data-ttu-id="51abe-113">Le `addBook` fonction lit les valeurs actuelles des entrées de formulaire lié aux données pour créer un objet JSON.</span><span class="sxs-lookup"><span data-stu-id="51abe-113">The `addBook` function reads the current values of the data-bound form inputs to create a JSON object.</span></span> <span data-ttu-id="51abe-114">Puis il publie l’objet JSON à `/api/books`.</span><span class="sxs-lookup"><span data-stu-id="51abe-114">Then it POSTs the JSON object to `/api/books`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="51abe-115">[Précédent](part-8.md)
> [Suivant](part-10.md)</span><span class="sxs-lookup"><span data-stu-id="51abe-115">[Previous](part-8.md)
[Next](part-10.md)</span></span>
