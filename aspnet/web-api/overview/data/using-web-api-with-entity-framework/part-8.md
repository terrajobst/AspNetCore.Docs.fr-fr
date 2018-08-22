---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Afficher les détails de l’élément | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 83f291326307a4964afdd5e8b50f2c375348ed0e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824500"
---
<a name="display-item-details"></a><span data-ttu-id="bf7a1-102">Affichage des détails d’élément</span><span class="sxs-lookup"><span data-stu-id="bf7a1-102">Display Item Details</span></span>
====================
<span data-ttu-id="bf7a1-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bf7a1-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="bf7a1-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="bf7a1-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="bf7a1-105">Dans cette section, vous allez ajouter la possibilité d’afficher les détails de chaque ouvrage.</span><span class="sxs-lookup"><span data-stu-id="bf7a1-105">In this section, you will add the ability to view details for each book.</span></span> <span data-ttu-id="bf7a1-106">Dans app.js, ajoutez le code suivant au modèle de vue :</span><span class="sxs-lookup"><span data-stu-id="bf7a1-106">In app.js, add to the following code to the view model:</span></span>

[!code-javascript[Main](part-8/samples/sample1.js)]

<span data-ttu-id="bf7a1-107">Dans Views/Home/Index.cshtml, ajoutez un élément de liaison de données pour le lien Détails :</span><span class="sxs-lookup"><span data-stu-id="bf7a1-107">In Views/Home/Index.cshtml, add a data-bind element to the Details link:</span></span>

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

<span data-ttu-id="bf7a1-108">Cette opération lie au Gestionnaire de clic le &lt;un&gt; élément à la `getBookDetail` fonction sur le modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="bf7a1-108">This binds the click handler for the &lt;a&gt; element to the `getBookDetail` function on the view model.</span></span>

<span data-ttu-id="bf7a1-109">Dans le même fichier, remplacez la majoration suivante :</span><span class="sxs-lookup"><span data-stu-id="bf7a1-109">In the same file, replace the following mark-up:</span></span>

[!code-html[Main](part-8/samples/sample3.html)]

<span data-ttu-id="bf7a1-110">par le code :</span><span class="sxs-lookup"><span data-stu-id="bf7a1-110">with this:</span></span>

[!code-html[Main](part-8/samples/sample4.html)]

<span data-ttu-id="bf7a1-111">Ce balisage crée une table qui est lié aux données aux propriétés de la `detail` observable dans le modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="bf7a1-111">This markup creates a table that is data-bound to the properties of the `detail` observable in the view model.</span></span>

<span data-ttu-id="bf7a1-112">Le «&lt;!--ko--&gt; &quot; syntaxe vous permet d’inclure une liaison de Knockout en dehors d’un élément DOM.</span><span class="sxs-lookup"><span data-stu-id="bf7a1-112">The "&lt;!-- ko --&gt;&quot; syntax lets you include a Knockout binding outside of a DOM element.</span></span> <span data-ttu-id="bf7a1-113">Dans ce cas, le `if` liaison provoque cette section de balisage à afficher uniquement lorsque `details` n’est pas null.</span><span class="sxs-lookup"><span data-stu-id="bf7a1-113">In this case, the `if` binding causes this section of markup to be displayed only when `details` is non-null.</span></span>

[!code-html[Main](part-8/samples/sample5.html)]

<span data-ttu-id="bf7a1-114">Maintenant, si vous exécutez l’application et cliquez sur une de le &quot;détail&quot; liens, l’application affiche les détails du livre.</span><span class="sxs-lookup"><span data-stu-id="bf7a1-114">Now if you run the app and click one of the &quot;Detail&quot; links, the app will display the book details.</span></span>

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="bf7a1-115">[Précédent](part-7.md)
> [Suivant](part-9.md)</span><span class="sxs-lookup"><span data-stu-id="bf7a1-115">[Previous](part-7.md)
[Next](part-9.md)</span></span>
