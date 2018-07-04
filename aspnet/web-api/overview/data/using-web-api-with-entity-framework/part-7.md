---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Créer la vue (IU) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: e9ebe60f88ecbf65a6f8d04de9a23d72a72fda83
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364979"
---
<a name="create-the-view-ui"></a><span data-ttu-id="3b247-102">Créer la vue (interface utilisateur)</span><span class="sxs-lookup"><span data-stu-id="3b247-102">Create the View (UI)</span></span>
====================
<span data-ttu-id="3b247-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3b247-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="3b247-104">Télécharger le projet terminé</span><span class="sxs-lookup"><span data-stu-id="3b247-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="3b247-105">Dans cette section, vous allez démarrer définir le code HTML de l’application et ajouter une liaison de données entre le code HTML et le modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="3b247-105">In this section, you will start to define the HTML for the app, and add data binding between the HTML and the view model.</span></span>

<span data-ttu-id="3b247-106">Ouvrez le fichier Views/Home/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="3b247-106">Open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="3b247-107">Remplacez tout le contenu de ce fichier avec les éléments suivants.</span><span class="sxs-lookup"><span data-stu-id="3b247-107">Replace the entire contents of that file with the following.</span></span>

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

<span data-ttu-id="3b247-108">La plupart de la `div` éléments existe-t-il pour [Bootstrap](http://getbootstrap.com/) styles.</span><span class="sxs-lookup"><span data-stu-id="3b247-108">Most of the `div` elements are there for [Bootstrap](http://getbootstrap.com/) styling.</span></span> <span data-ttu-id="3b247-109">Les éléments importants sont ceux avec `data-bind` attributs.</span><span class="sxs-lookup"><span data-stu-id="3b247-109">The important elements are the ones with `data-bind` attributes.</span></span> <span data-ttu-id="3b247-110">Cet attribut lie le code HTML au modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="3b247-110">This attribute links the HTML to the view model.</span></span>

<span data-ttu-id="3b247-111">Exemple :</span><span class="sxs-lookup"><span data-stu-id="3b247-111">For example:</span></span>

[!code-html[Main](part-7/samples/sample2.html)]

<span data-ttu-id="3b247-112">Dans cet exemple, le &quot; `text` &quot; liaison provoque la `<p>` élément pour afficher la valeur de la `error` propriété à partir du modèle de vue.</span><span class="sxs-lookup"><span data-stu-id="3b247-112">In this example, the &quot;`text`&quot; binding causes the `<p>` element to show the value of the `error` property from the view model.</span></span> <span data-ttu-id="3b247-113">N’oubliez pas que `error` a été déclaré comme un `ko.observable`:</span><span class="sxs-lookup"><span data-stu-id="3b247-113">Recall that `error` was declared as a `ko.observable`:</span></span>

[!code-javascript[Main](part-7/samples/sample3.js)]

<span data-ttu-id="3b247-114">Chaque fois qu’une nouvelle valeur est affectée à `error`, Knockout met à jour le texte dans le `<p>` élément.</span><span class="sxs-lookup"><span data-stu-id="3b247-114">Whenever a new value is assigned to `error`, Knockout updates the text in the `<p>` element.</span></span>

<span data-ttu-id="3b247-115">Le `foreach` liaison indique à Knockout pour itérer sur le contenu de la `books` tableau.</span><span class="sxs-lookup"><span data-stu-id="3b247-115">The `foreach` binding tells Knockout to loop through the contents of the `books` array.</span></span> <span data-ttu-id="3b247-116">Pour chaque élément du tableau, Knockout crée un &lt;li&gt; élément.</span><span class="sxs-lookup"><span data-stu-id="3b247-116">For each item in the array, Knockout creates a new &lt;li&gt; element.</span></span> <span data-ttu-id="3b247-117">Liaisons dans le contexte de la `foreach` font référence aux propriétés sur l’élément du tableau.</span><span class="sxs-lookup"><span data-stu-id="3b247-117">Bindings inside the context of the `foreach` refer to properties on the array item.</span></span> <span data-ttu-id="3b247-118">Exemple :</span><span class="sxs-lookup"><span data-stu-id="3b247-118">For example:</span></span>

[!code-html[Main](part-7/samples/sample4.html)]

<span data-ttu-id="3b247-119">Ici le `text` liaison lit la propriété de l’auteur de chaque ouvrage.</span><span class="sxs-lookup"><span data-stu-id="3b247-119">Here the `text` binding reads the Author property of each book.</span></span>

<span data-ttu-id="3b247-120">Si vous exécutez l’application maintenant, il doit ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="3b247-120">If you run the application now, it should look like this:</span></span>

![](part-7/_static/image1.png)

<span data-ttu-id="3b247-121">La liste des livres charge en mode asynchrone, une fois que la page se charge.</span><span class="sxs-lookup"><span data-stu-id="3b247-121">The list of books loads asynchronously, after the page loads.</span></span> <span data-ttu-id="3b247-122">Maintenant, le &quot;détails&quot; liens ne sont pas fonctionnels.</span><span class="sxs-lookup"><span data-stu-id="3b247-122">Right now, the &quot;Details&quot; links are not functional.</span></span> <span data-ttu-id="3b247-123">Nous allons ajouter cette fonctionnalité dans la section suivante.</span><span class="sxs-lookup"><span data-stu-id="3b247-123">We'll add this functionality in the next section.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3b247-124">[Précédent](part-6.md)
> [Suivant](part-8.md)</span><span class="sxs-lookup"><span data-stu-id="3b247-124">[Previous](part-6.md)
[Next](part-8.md)</span></span>
