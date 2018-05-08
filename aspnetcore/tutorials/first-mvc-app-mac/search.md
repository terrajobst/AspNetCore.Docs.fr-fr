---
title: Ajout d’une fonctionnalité de recherche à une application ASP.NET Core MVC
author: rick-anderson
description: Montre comment ajouter une fonction de recherche à une application ASP.NET MVC simple
manager: wpickett
ms.author: riande
ms.date: 04/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-mac/search
ms.openlocfilehash: 3f648bc6c6d095b9fe8b6ac65bf5f51741938ed1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
[!INCLUDE [adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="724e9-103">Remarque : SQLite respecte la casse, vous devez donc rechercher « Ghost » et non pas « ghost ».</span><span class="sxs-lookup"><span data-stu-id="724e9-103">Note: SQLlite is case sensitive, so you'll need to search for "Ghost" and not "ghost".</span></span>

[!INCLUDE [adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="724e9-104">Changez la balise `<form>` dans la vue Razor *Views\movie\Index.cshtml* pour spécifier `method="get"` :</span><span class="sxs-lookup"><span data-stu-id="724e9-104">Change the `<form>` tag in the *Views\movie\Index.cshtml* Razor view to specify `method="get"`:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE [adding-model](../../includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="724e9-105">[Précédent - Méthodes et vues du contrôleur](controller-methods-views.md)
> [Suivant - Ajouter un champ](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="724e9-105">[Previous - Controller methods and views](controller-methods-views.md)
[Next - Add a field](new-field.md)</span></span>
