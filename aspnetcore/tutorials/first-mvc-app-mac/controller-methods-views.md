---
title: "Méthodes et vues de contrôleur dans une application ASP.NET Core MVC"
author: rick-anderson
description: "Utilisation des méthodes, vues et DataAnnotations de contrôleur"
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/controller-methods-views
ms.openlocfilehash: 1c58a3bbac227e06a36df0b0d3ed6c2455eb46c4
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/19/2018
---
# <a name="controller-methods-and-views-in-an-aspnet-core-mvc-app"></a><span data-ttu-id="f1c66-103">Méthodes et vues de contrôleur dans une application ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="f1c66-103">Controller methods and views in an ASP.NET Core MVC app</span></span>

<span data-ttu-id="f1c66-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f1c66-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f1c66-105">Nous avons une bonne ébauche de l’application de films, mais sa présentation n’est pas idéale.</span><span class="sxs-lookup"><span data-stu-id="f1c66-105">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="f1c66-106">Nous ne voulons pas voir l’heure (« 12:00:00 AM » dans l’illustration suivante) et **ReleaseDate** doit être composé de deux mots.</span><span class="sxs-lookup"><span data-stu-id="f1c66-106">We don't want to see the time (12:00:00 AM in the following image) and **ReleaseDate** should be two words.</span></span>

![Vue d’index : Release Date apparaît en un seul mot (sans espace) et chaque date de sortie d’un film a comme heure « 12 AM »](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="f1c66-108">Ouvrez le fichier *Models/Movie.cs*, puis ajoutez les lignes en surbrillance ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="f1c66-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

<span data-ttu-id="f1c66-109">Générez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="f1c66-109">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="f1c66-110">[Précédent - Utilisation de SQLite](working-with-sql.md)
[Suivant - Ajouter une fonction de recherche](search.md)</span><span class="sxs-lookup"><span data-stu-id="f1c66-110">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>
