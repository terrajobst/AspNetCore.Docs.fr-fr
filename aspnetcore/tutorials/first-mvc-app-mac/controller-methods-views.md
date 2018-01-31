---
title: "Méthodes et vues de contrôleur dans une application ASP.NET Core MVC"
author: rick-anderson
description: "Utilisation des méthodes, vues et DataAnnotations de contrôleur"
manager: wpickett
ms.author: riande
ms.date: 04/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-mac/controller-methods-views
ms.openlocfilehash: 01c20e505bd9d1591e1921701f94d102822231c8
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="controller-methods-and-views-in-an-aspnet-core-mvc-app"></a><span data-ttu-id="4faa9-103">Méthodes et vues de contrôleur dans une application ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="4faa9-103">Controller methods and views in an ASP.NET Core MVC app</span></span>

<span data-ttu-id="4faa9-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4faa9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4faa9-105">Nous avons une bonne ébauche de l’application de films, mais sa présentation n’est pas idéale.</span><span class="sxs-lookup"><span data-stu-id="4faa9-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="4faa9-106">Nous ne voulons pas voir l’heure (« 12:00:00 AM » dans l’illustration suivante) et **ReleaseDate** doit être composé de deux mots.</span><span class="sxs-lookup"><span data-stu-id="4faa9-106">We don't want to see the time (12:00:00 AM in the following image) and **ReleaseDate** should be two words.</span></span>

![Vue d’index : Release Date apparaît en un seul mot (sans espace) et chaque date de sortie d’un film a comme heure « 12 AM »](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="4faa9-108">Ouvrez le fichier *Models/Movie.cs*, puis ajoutez les lignes en surbrillance ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="4faa9-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

<span data-ttu-id="4faa9-109">Générez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="4faa9-109">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="4faa9-110">[Précédent - Utilisation de SQLite](working-with-sql.md)
[Suivant - Ajouter une fonction de recherche](search.md)</span><span class="sxs-lookup"><span data-stu-id="4faa9-110">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>
