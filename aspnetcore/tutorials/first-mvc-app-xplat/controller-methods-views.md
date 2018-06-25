---
title: Méthodes et vues de contrôleur dans ASP.NET Core
author: rick-anderson
description: Découvrez comment utiliser des méthodes et des vues de contrôleur, ainsi que DataAnnotations dans ASP.NET Core.
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-xplat/controller-methods-views
ms.openlocfilehash: 7cf42807eaba356cd090a08bba9357c3ec237087
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278438"
---
# <a name="controller-methods-and-views-in-aspnet-core"></a><span data-ttu-id="0dc00-103">Méthodes et vues de contrôleur dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0dc00-103">Controller methods and views in ASP.NET Core</span></span>

<span data-ttu-id="0dc00-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0dc00-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0dc00-105">Nous avons une bonne ébauche de l’application de films, mais sa présentation n’est pas idéale.</span><span class="sxs-lookup"><span data-stu-id="0dc00-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="0dc00-106">Nous ne voulons pas voir l’heure (« 12:00:00 AM » dans l’image ci-dessous) et **ReleaseDate** doit être composé de deux mots.</span><span class="sxs-lookup"><span data-stu-id="0dc00-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Vue d’index : Release Date apparaît en un seul mot (sans espace) et chaque date de sortie d’un film a comme heure « 12 AM »](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="0dc00-108">Ouvrez le fichier *Models/Movie.cs*, puis ajoutez les lignes en surbrillance ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="0dc00-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

<span data-ttu-id="0dc00-109">Générez et exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="0dc00-109">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="0dc00-110">[Précédent - Utilisation de SQLite](working-with-sql.md)
> [Suivant - Ajouter une fonction de recherche](search.md)</span><span class="sxs-lookup"><span data-stu-id="0dc00-110">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>  
