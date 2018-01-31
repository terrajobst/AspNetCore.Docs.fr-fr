---
title: "Méthodes et vues de contrôleur"
author: rick-anderson
description: "Utilisation des méthodes, vues et DataAnnotations de contrôleur"
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 200f02f9815966653b3b46918737c60d11f11d5a
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="65933-103">Méthodes et vues de contrôleur</span><span class="sxs-lookup"><span data-stu-id="65933-103">Controller methods and views</span></span>

<span data-ttu-id="65933-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="65933-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="65933-105">Nous avons une bonne ébauche de l’application de films, mais sa présentation n’est pas idéale.</span><span class="sxs-lookup"><span data-stu-id="65933-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="65933-106">Nous ne voulons pas voir l’heure (« 12:00:00 AM » dans l’image ci-dessous) et **ReleaseDate** doit être composé de deux mots.</span><span class="sxs-lookup"><span data-stu-id="65933-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Vue d’index : Release Date apparaît en un seul mot (sans espace) et chaque date de sortie d’un film a comme heure « 12 AM »](working-with-sql/_static/m55.png)

<span data-ttu-id="65933-108">Ouvrez le fichier *Models/Movie.cs*, puis ajoutez les lignes en surbrillance ci-dessous :</span><span class="sxs-lookup"><span data-stu-id="65933-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

<span data-ttu-id="65933-109">Cliquez avec le bouton droit sur une ligne ondulée rouge **> Actions rapides et refactorisations**.</span><span class="sxs-lookup"><span data-stu-id="65933-109">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![Le menu contextuel affiche **> Actions rapides et refactorisations**.](controller-methods-views/_static/qa.png)


<span data-ttu-id="65933-111">Appuyez sur `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="65933-111">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![using System.ComponentModel.DataAnnotations en haut de la liste](controller-methods-views/_static/da.png)

  <span data-ttu-id="65933-113">Visual studio ajoute `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="65933-113">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="65933-114">Supprimons les instructions `using` qui ne sont pas nécessaires.</span><span class="sxs-lookup"><span data-stu-id="65933-114">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="65933-115">Elles s’affichent par défaut dans une police gris clair.</span><span class="sxs-lookup"><span data-stu-id="65933-115">They show up by default in a light grey font.</span></span> <span data-ttu-id="65933-116">Cliquez avec le bouton droit sur n’importe où dans le fichier *Movie.cs* **> Supprimer et trier les Usings**.</span><span class="sxs-lookup"><span data-stu-id="65933-116">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![Supprimer et trier les Usings](controller-methods-views/_static/rm.png)

<span data-ttu-id="65933-118">Le code mis à jour :</span><span class="sxs-lookup"><span data-stu-id="65933-118">The updated code:</span></span>

[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="65933-119">[Précédent](working-with-sql.md)
[Suivant](search.md)</span><span class="sxs-lookup"><span data-stu-id="65933-119">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
