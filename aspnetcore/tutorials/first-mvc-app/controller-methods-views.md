---
title: Méthodes et vues de contrôleur dans ASP.NET Core
author: rick-anderson
description: Découvrez comment utiliser des méthodes et des vues de contrôleur ainsi que DataAnnotations dans ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 6fe0a0e71079bebcbd3a76abee0f2917f562e766
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
# <a name="controller-methods-and-views-in-aspnet-core"></a><span data-ttu-id="830b3-103">Méthodes et vues de contrôleur dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="830b3-103">Controller methods and views in ASP.NET Core</span></span>

<span data-ttu-id="830b3-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="830b3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="830b3-105">Nous avons une bonne ébauche de l’application de films, mais sa présentation n’est pas idéale.</span><span class="sxs-lookup"><span data-stu-id="830b3-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="830b3-106">Nous ne voulons pas voir l’heure (« 12:00:00 AM » dans l’image ci-dessous) et **ReleaseDate** doit être composé de deux mots.</span><span class="sxs-lookup"><span data-stu-id="830b3-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Vue d’index : Release Date apparaît en un seul mot (sans espace) et chaque date de sortie d’un film a comme heure « 12 AM »](working-with-sql/_static/m55.png)

<span data-ttu-id="830b3-108">Ouvrez le fichier *Models/Movie.cs*, puis ajoutez les lignes en surbrillance ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="830b3-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

<span data-ttu-id="830b3-109">Cliquez avec le bouton droit sur une ligne ondulée rouge **> Actions rapides et refactorisations**.</span><span class="sxs-lookup"><span data-stu-id="830b3-109">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![Le menu contextuel affiche **> Actions rapides et refactorisations**.](controller-methods-views/_static/qa.png)


<span data-ttu-id="830b3-111">Appuyez sur `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="830b3-111">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![using System.ComponentModel.DataAnnotations en haut de la liste](controller-methods-views/_static/da.png)

  <span data-ttu-id="830b3-113">Visual studio ajoute `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="830b3-113">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="830b3-114">Supprimons les instructions `using` qui ne sont pas nécessaires.</span><span class="sxs-lookup"><span data-stu-id="830b3-114">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="830b3-115">Elles s’affichent par défaut dans une police gris clair.</span><span class="sxs-lookup"><span data-stu-id="830b3-115">They show up by default in a light grey font.</span></span> <span data-ttu-id="830b3-116">Cliquez avec le bouton droit sur n’importe où dans le fichier *Movie.cs* **> Supprimer et trier les Usings**.</span><span class="sxs-lookup"><span data-stu-id="830b3-116">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![Supprimer et trier les Usings](controller-methods-views/_static/rm.png)

<span data-ttu-id="830b3-118">Le code mis à jour :</span><span class="sxs-lookup"><span data-stu-id="830b3-118">The updated code:</span></span>

[!code-csharp[](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="830b3-119">[Précédent](working-with-sql.md)
> [Suivant](search.md)</span><span class="sxs-lookup"><span data-stu-id="830b3-119">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
