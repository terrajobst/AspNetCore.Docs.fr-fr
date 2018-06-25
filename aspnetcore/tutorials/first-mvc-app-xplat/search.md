---
title: Ajout d’une fonction de recherche
author: rick-anderson
description: Montre comment ajouter une fonction de recherche à une application ASP.NET MVC simple
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-xplat/search
ms.openlocfilehash: dc84eb38c0487d90451979ec9572bf1641571357
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276004"
---
[!INCLUDE [adding-model](../../includes/mvc-intro/search1.md)]

Remarque : SQLite respecte la casse, vous devez donc rechercher « Ghost » et non pas « ghost ».

[!INCLUDE [adding-model](../../includes/mvc-intro/search2.md)]

Changez la balise `<form>` dans la vue Razor *Views\movie\Index.cshtml* pour spécifier `method="get"` :

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE [adding-model](../../includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> [Précédent - Méthodes et vues du contrôleur](controller-methods-views.md)
> [Suivant - Ajouter un champ](new-field.md)  
