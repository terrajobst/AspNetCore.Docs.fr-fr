---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
title: 'Comment faire : créer une application d’assistance HTML personnalisée pour une Application MVC ? | Microsoft Docs'
author: rick-anderson
description: Dans cette vidéo, Chris Pels montre comment créer un HtmlHelper personnalisé qui n’est pas disponible dans le jeu standard dans une application MVC. Tout d’abord, une application MVC d’exemple...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/11/2009
ms.topic: article
ms.assetid: 58b5eb15-4160-4ce2-ae70-6ba94262ea73
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
msc.type: video
ms.openlocfilehash: 87e74ade0182589d22aeaf66a608165df7ea2ee6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380139"
---
<a name="how-do-i-create-a-custom-html-helper-for-an-mvc-application"></a><span data-ttu-id="ef4c7-105">Comment faire : créer une application d’assistance HTML personnalisée pour une Application MVC ?</span><span class="sxs-lookup"><span data-stu-id="ef4c7-105">How Do I: Create a Custom HTML Helper for an MVC Application?</span></span>
====================
<span data-ttu-id="ef4c7-106">par [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="ef4c7-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="ef4c7-107">Dans cette vidéo, Chris Pels montre comment créer un HtmlHelper personnalisé qui n’est pas disponible dans le jeu standard dans une application MVC.</span><span class="sxs-lookup"><span data-stu-id="ef4c7-107">In this video Chris Pels shows how to create a custom HtmlHelper that is not available in the standard set in an MVC application.</span></span> <span data-ttu-id="ef4c7-108">Tout d’abord, un exemple d’application MVC est créé avec un contrôleur de démonstration et de la vue pour tester le HtmlHelper personnalisé.</span><span class="sxs-lookup"><span data-stu-id="ef4c7-108">First, a sample MVC application is created with a demo controller and view to test the custom HtmlHelper.</span></span> <span data-ttu-id="ef4c7-109">Ensuite, un module est créé avec une fonction publique qui est une méthode d’extension qui représente l’implémentation de l’objet HtmlHelper personnalisé.</span><span class="sxs-lookup"><span data-stu-id="ef4c7-109">Next, a module is created with a public function that is an extension method which represents the implementation of the custom HtmlHelper.</span></span> <span data-ttu-id="ef4c7-110">L’application d’assistance personnalisée concerne la création `<img>` balises dans une page et reçoit plusieurs paramètres entrants, notamment l’id, une url et un texte de remplacement pour la balise d’image.</span><span class="sxs-lookup"><span data-stu-id="ef4c7-110">The custom helper is for creating `<img>` tags in a page and receives several inbound parameters including the id, url, and alt text for the image tag.</span></span> <span data-ttu-id="ef4c7-111">La logique est ensuite ajoutée à la fonction pour retourner le terminé `<img>` balise avec les informations spécifiées.</span><span class="sxs-lookup"><span data-stu-id="ef4c7-111">The logic is then added to the function for returning the completed `<img>` tag with the specified information.</span></span> <span data-ttu-id="ef4c7-112">Objet HtmlHelper personnalisé est utilisé dans la page de démonstration pour afficher une image.</span><span class="sxs-lookup"><span data-stu-id="ef4c7-112">Then the custom HtmlHelper is used on the demo page to display an image.</span></span> <span data-ttu-id="ef4c7-113">Enfin, objet HtmlHelper personnalisé est développé pour inclure plusieurs remplacements constructeur permettant en toute flexibilité pour en savoir plus facilement créer différentes `<img>` balises.</span><span class="sxs-lookup"><span data-stu-id="ef4c7-113">Finally, the custom HtmlHelper is expanded to include multiple constructor overrides which provide flexibility for more easily creating different `<img>` tags.</span></span>

[<span data-ttu-id="ef4c7-114">&#9654;Regardez la vidéo (18 minutes)</span><span class="sxs-lookup"><span data-stu-id="ef4c7-114">&#9654; Watch video (18 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-custom-html-helper-for-an-mvc-application)

> [!div class="step-by-step"]
> <span data-ttu-id="ef4c7-115">[Précédent](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
> [Suivant](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="ef4c7-115">[Previous](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
[Next](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span></span>
