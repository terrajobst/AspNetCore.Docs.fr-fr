---
title: "Méthodes et vues de contrôleur"
author: rick-anderson
description: "Utilisation des méthodes, vues et DataAnnotations de contrôleur"
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: e279b6c2852f1aea49685381ccaa5f7854f7c418
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="31738-103">Méthodes et vues de contrôleur</span><span class="sxs-lookup"><span data-stu-id="31738-103">Controller methods and views</span></span>

<span data-ttu-id="31738-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="31738-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="31738-105">Nous avons une bonne ébauche de l’application de films, mais sa présentation n’est pas idéale.</span><span class="sxs-lookup"><span data-stu-id="31738-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="31738-106">Nous ne voulons pas voir l’heure (« 12:00:00 AM » dans l’image ci-dessous) et **ReleaseDate** doit être composé de deux mots.</span><span class="sxs-lookup"><span data-stu-id="31738-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Vue d’index : Release Date apparaît en un seul mot (sans espace) et chaque date de sortie d’un film a comme heure « 12 AM »](working-with-sql/_static/m55.png)

<span data-ttu-id="31738-108">Ouvrez le fichier *Models/Movie.cs*, puis ajoutez les lignes en surbrillance ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="31738-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

<span data-ttu-id="31738-109">Cliquez avec le bouton droit sur une ligne ondulée rouge **> Actions rapides et refactorisations**.</span><span class="sxs-lookup"><span data-stu-id="31738-109">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![Le menu contextuel affiche **> Actions rapides et refactorisations**.](controller-methods-views/_static/qa.png)


<span data-ttu-id="31738-111">Appuyez sur `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="31738-111">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![using System.ComponentModel.DataAnnotations en haut de la liste](controller-methods-views/_static/da.png)

  <span data-ttu-id="31738-113">Visual studio ajoute `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="31738-113">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="31738-114">Supprimons les instructions `using` qui ne sont pas nécessaires.</span><span class="sxs-lookup"><span data-stu-id="31738-114">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="31738-115">Elles s’affichent par défaut dans une police gris clair.</span><span class="sxs-lookup"><span data-stu-id="31738-115">They show up by default in a light grey font.</span></span> <span data-ttu-id="31738-116">Cliquez avec le bouton droit sur n’importe où dans le fichier *Movie.cs* **> Supprimer et trier les Usings**.</span><span class="sxs-lookup"><span data-stu-id="31738-116">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![Supprimer et trier les Usings](controller-methods-views/_static/rm.png)

<span data-ttu-id="31738-118">Le code mis à jour :</span><span class="sxs-lookup"><span data-stu-id="31738-118">The updated code:</span></span>

[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="31738-119">[Précédent](working-with-sql.md)
[Suivant](search.md)</span><span class="sxs-lookup"><span data-stu-id="31738-119">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
