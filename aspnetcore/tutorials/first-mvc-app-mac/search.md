---
title: Ajout d’une fonctionnalité de recherche à une application ASP.NET Core MVC
author: rick-anderson
description: Montre comment ajouter une fonction de recherche à une application ASP.NET MVC simple
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-mac/search
ms.openlocfilehash: 4175d4dfd03d173f7025aff3b51d255bb1c213ee
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274480"
---
[!INCLUDE [adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="3db10-103">Remarque : SQLite respecte la casse, vous devez donc rechercher « Ghost » et non pas « ghost ».</span><span class="sxs-lookup"><span data-stu-id="3db10-103">Note: SQLlite is case sensitive, so you'll need to search for "Ghost" and not "ghost".</span></span>

[!INCLUDE [adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="3db10-104">Changez la balise `<form>` dans la vue Razor *Views\movie\Index.cshtml* pour spécifier `method="get"` :</span><span class="sxs-lookup"><span data-stu-id="3db10-104">Change the `<form>` tag in the *Views\movie\Index.cshtml* Razor view to specify `method="get"`:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE [adding-model](../../includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="3db10-105">[Précédent - Méthodes et vues du contrôleur](controller-methods-views.md)
> [Suivant - Ajouter un champ](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="3db10-105">[Previous - Controller methods and views](controller-methods-views.md)
[Next - Add a field](new-field.md)</span></span>
