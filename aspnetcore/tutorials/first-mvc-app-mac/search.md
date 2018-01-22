---
title: "Ajout d’une fonctionnalité de recherche à une application ASP.NET Core MVC"
author: rick-anderson
description: "Montre comment ajouter une fonction de recherche à une application ASP.NET MVC simple"
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/search
ms.openlocfilehash: 76739cd3805d9e5ac8b4b0e672b8e7da6596492e
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="8a1d5-103">Remarque : SQLite respecte la casse, vous devez donc rechercher « Ghost » et non pas « ghost ».</span><span class="sxs-lookup"><span data-stu-id="8a1d5-103">Note: SQLlite is case sensitive, so you'll need to search for "Ghost" and not "ghost".</span></span>

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="8a1d5-104">Changez la balise `<form>` dans la vue Razor *Views\movie\Index.cshtml* pour spécifier `method="get"` :</span><span class="sxs-lookup"><span data-stu-id="8a1d5-104">Change the `<form>` tag in the *Views\movie\Index.cshtml* Razor view to specify `method="get"`:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
<span data-ttu-id="8a1d5-105">[Précédent - Méthodes et vues du contrôleur](controller-methods-views.md)
[Suivant - Ajouter un champ](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="8a1d5-105">[Previous - Controller methods and views](controller-methods-views.md)
[Next - Add a field](new-field.md)</span></span>
