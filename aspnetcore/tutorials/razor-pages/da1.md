---
title: Mettre à jour les pages générées dans une application ASP.NET Core
author: rick-anderson
description: Découvrez comment mettre à jour les pages générées dans une application ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 5c188799b7a42bcd5e9d5eab8dfe8cdad8002fe5
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2018
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a>Mettre à jour les pages générées dans une application ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Nous avons une bonne ébauche de l’application de films, mais sa présentation n’est pas idéale. Nous ne voulons pas voir l’heure (« 12:00:00 AM » dans l’image ci-dessous) et **ReleaseDate** doit être **Release Date** (en deux mots).

![Application Movie ouverte dans Chrome, affichant les données relatives aux films](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Mettre à jour le code généré

Ouvrez le fichier *Models/Movie.cs*, puis ajoutez les lignes affichées en surbrillance dans le code suivant :

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

Cliquez avec le bouton droit sur une ligne ondulée rouge > ** Actions rapides et refactorisations**.

  ![Le menu contextuel affiche **> Actions rapides et refactorisations**.](da1/qa.png)

Sélectionnez `using System.ComponentModel.DataAnnotations;`.

  ![using System.ComponentModel.DataAnnotations en haut de la liste](da1/da.png)

  Visual studio ajoute `using System.ComponentModel.DataAnnotations;`.

[!INCLUDE [model1](../../includes/RP/da2.md)]

> [!div class="step-by-step"]
> [Précédent : Utilisation de SQL Server LocalDB](xref:tutorials/razor-pages/sql)
> [Ajouter une fonction de recherche](xref:tutorials/razor-pages/search)
