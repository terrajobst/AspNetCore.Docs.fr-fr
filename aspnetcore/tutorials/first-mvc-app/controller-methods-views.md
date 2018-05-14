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
# <a name="controller-methods-and-views-in-aspnet-core"></a>Méthodes et vues de contrôleur dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Nous avons une bonne ébauche de l’application de films, mais sa présentation n’est pas idéale. Nous ne voulons pas voir l’heure (« 12:00:00 AM » dans l’image ci-dessous) et **ReleaseDate** doit être composé de deux mots.

![Vue d’index : Release Date apparaît en un seul mot (sans espace) et chaque date de sortie d’un film a comme heure « 12 AM »](working-with-sql/_static/m55.png)

Ouvrez le fichier *Models/Movie.cs*, puis ajoutez les lignes en surbrillance ci-dessous :

[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

Cliquez avec le bouton droit sur une ligne ondulée rouge **> Actions rapides et refactorisations**.

  ![Le menu contextuel affiche **> Actions rapides et refactorisations**.](controller-methods-views/_static/qa.png)


Appuyez sur `using System.ComponentModel.DataAnnotations;`

  ![using System.ComponentModel.DataAnnotations en haut de la liste](controller-methods-views/_static/da.png)

  Visual studio ajoute `using System.ComponentModel.DataAnnotations;`.

Supprimons les instructions `using` qui ne sont pas nécessaires. Elles s’affichent par défaut dans une police gris clair. Cliquez avec le bouton droit sur n’importe où dans le fichier *Movie.cs* **> Supprimer et trier les Usings**.

![Supprimer et trier les Usings](controller-methods-views/_static/rm.png)

Le code mis à jour :

[!code-csharp[](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> [Précédent](working-with-sql.md)
> [Suivant](search.md)  
