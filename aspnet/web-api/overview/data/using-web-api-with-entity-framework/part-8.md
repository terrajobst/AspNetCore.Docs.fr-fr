---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: "Afficher les détails de l’élément | Documents Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 0b6ae9384843712cae824ea662b984a40f021e57
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="display-item-details"></a><span data-ttu-id="3375a-102">Affichage des détails d’élément</span><span class="sxs-lookup"><span data-stu-id="3375a-102">Display Item Details</span></span>
====================
<span data-ttu-id="3375a-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3375a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="3375a-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="3375a-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="3375a-105">Dans cette section, vous allez ajouter la possibilité d’afficher les détails de chaque livre.</span><span class="sxs-lookup"><span data-stu-id="3375a-105">In this section, you will add the ability to view details for each book.</span></span> <span data-ttu-id="3375a-106">Dans app.js, ajoutez le code suivant pour le modèle d’affichage :</span><span class="sxs-lookup"><span data-stu-id="3375a-106">In app.js, add to the following code to the view model:</span></span>

[!code-javascript[Main](part-8/samples/sample1.js)]

<span data-ttu-id="3375a-107">Dans Views/Home/Index.cshtml, ajoutez un élément de liaison de données pour le lien Détails :</span><span class="sxs-lookup"><span data-stu-id="3375a-107">In Views/Home/Index.cshtml, add a data-bind element to the Details link:</span></span>

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

<span data-ttu-id="3375a-108">Ceci lie le gestionnaire click pour le &lt;un&gt; élément à la `getBookDetail` fonction sur le modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="3375a-108">This binds the click handler for the &lt;a&gt; element to the `getBookDetail` function on the view model.</span></span>

<span data-ttu-id="3375a-109">Dans le même fichier, remplacez le marquer des suivantes :</span><span class="sxs-lookup"><span data-stu-id="3375a-109">In the same file, replace the following mark-up:</span></span>

[!code-html[Main](part-8/samples/sample3.html)]

<span data-ttu-id="3375a-110">par le code :</span><span class="sxs-lookup"><span data-stu-id="3375a-110">with this:</span></span>

[!code-html[Main](part-8/samples/sample4.html)]

<span data-ttu-id="3375a-111">Cette balise crée une table qui est lié aux données pour les propriétés de la `detail` observable dans le modèle d’affichage.</span><span class="sxs-lookup"><span data-stu-id="3375a-111">This markup creates a table that is data-bound to the properties of the `detail` observable in the view model.</span></span>

<span data-ttu-id="3375a-112">Le «&lt;!--ko--&gt; &quot; syntaxe vous permet d’inclure une liaison Knockout en dehors d’un élément DOM.</span><span class="sxs-lookup"><span data-stu-id="3375a-112">The "&lt;!-- ko --&gt;&quot; syntax lets you include a Knockout binding outside of a DOM element.</span></span> <span data-ttu-id="3375a-113">Dans ce cas, le `if` liaison provoque cette section du balisage s’affiche uniquement lorsque `details` n’est pas null.</span><span class="sxs-lookup"><span data-stu-id="3375a-113">In this case, the `if` binding causes this section of markup to be displayed only when `details` is non-null.</span></span>

[!code-html[Main](part-8/samples/sample5.html)]

<span data-ttu-id="3375a-114">Maintenant, si vous exécutez l’application et cliquez sur un de la &quot;détail&quot; liens, l’application affiche les détails de l’annuaire.</span><span class="sxs-lookup"><span data-stu-id="3375a-114">Now if you run the app and click one of the &quot;Detail&quot; links, the app will display the book details.</span></span>

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

>[!div class="step-by-step"]
<span data-ttu-id="3375a-115">[Précédent](part-7.md)
[Suivant](part-9.md)</span><span class="sxs-lookup"><span data-stu-id="3375a-115">[Previous](part-7.md)
[Next](part-9.md)</span></span>
