---
title: Mettre à jour les pages générées dans une application ASP.NET Core
author: rick-anderson
description: Découvrez comment mettre à jour les pages générées dans une application ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: 78490be1cfa3018c465cb1e8125918404a7e4525
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011603"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="7e8d2-103">Mettre à jour les pages générées dans une application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7e8d2-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="7e8d2-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7e8d2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7e8d2-105">Nous avons une bonne ébauche de l’application de films, mais sa présentation n’est pas idéale.</span><span class="sxs-lookup"><span data-stu-id="7e8d2-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="7e8d2-106">Nous ne voulons pas voir l’heure (« 12:00:00 AM » dans l’image ci-dessous) et **ReleaseDate** doit être **Release Date** (en deux mots).</span><span class="sxs-lookup"><span data-stu-id="7e8d2-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Application Movie ouverte dans Chrome, affichant les données relatives aux films](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="7e8d2-108">Mettre à jour le code généré</span><span class="sxs-lookup"><span data-stu-id="7e8d2-108">Update the generated code</span></span>

<span data-ttu-id="7e8d2-109">Ouvrez le fichier *Models/Movie.cs*, puis ajoutez les lignes affichées en surbrillance dans le code suivant :</span><span class="sxs-lookup"><span data-stu-id="7e8d2-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]

::: moniker-end

<span data-ttu-id="7e8d2-110">Cliquez avec le bouton droit sur une ligne ondulée rouge > **Actions rapides et refactorisations**.</span><span class="sxs-lookup"><span data-stu-id="7e8d2-110">Right click on a red squiggly line > **Quick Actions and Refactorings**.</span></span>

  ![Le menu contextuel affiche **> Actions rapides et refactorisations**.](da1/qa.png)

<span data-ttu-id="7e8d2-112">Sélectionnez `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="7e8d2-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![using System.ComponentModel.DataAnnotations en haut de la liste](da1/da.png)

  <span data-ttu-id="7e8d2-114">Visual studio ajoute `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="7e8d2-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="7e8d2-115">[Précédent : Utilisation de SQL Server LocalDB](xref:tutorials/razor-pages/sql)
> [Ajouter une fonction de recherche](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="7e8d2-115">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Add search](xref:tutorials/razor-pages/search)</span></span>
