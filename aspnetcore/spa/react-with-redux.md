---
title: Utiliser le modèle de projet React avec réédition avec ASP.NET Core
author: SteveSandersonMS
description: Découvrez comment démarrer avec le modèle de projet Application Page unique (SPA) de ASP.NET Core pour réagir avec Redux et application de réagir créer.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/react-with-redux
ms.openlocfilehash: 9abfbfe5be69d3145de453d9d9e56ea35eec64ed
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a><span data-ttu-id="d13d7-103">Utiliser le modèle de projet React avec réédition avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d13d7-103">Use the React-with-Redux project template with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="d13d7-104">Cette documentation n’est pas sur le modèle de projet React avec réédition incluse dans ASP.NET 2.0 de base.</span><span class="sxs-lookup"><span data-stu-id="d13d7-104">This documentation isn't about the React-with-Redux project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="d13d7-105">Il est sur le modèle de réagir avec réédition plus récente à laquelle vous pouvez mettre à jour manuellement.</span><span class="sxs-lookup"><span data-stu-id="d13d7-105">It's about the newer React-with-Redux template to which you can update manually.</span></span> <span data-ttu-id="d13d7-106">Par défaut, le modèle est inclus dans ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="d13d7-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

<span data-ttu-id="d13d7-107">Le modèle de projet React avec Redux mise à jour fournit un point de départ pratique pour les applications ASP.NET Core à l’aide de réagissent, Redux, et [application react créer](https://github.com/facebookincubator/create-react-app) conventions (ARC) pour implémenter une interface utilisateur côté client riche (IU).</span><span class="sxs-lookup"><span data-stu-id="d13d7-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="d13d7-108">À l’exception de la commande de création de projet, toutes les informations sur le modèle React avec réédition sont le même que le modèle de réagir.</span><span class="sxs-lookup"><span data-stu-id="d13d7-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="d13d7-109">Pour créer ce type de projet, exécutez `dotnet new reactredux` au lieu de `dotnet new react`.</span><span class="sxs-lookup"><span data-stu-id="d13d7-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="d13d7-110">Pour plus d’informations sur les fonctionnalités communes aux deux modèles React, consultez [réagir documentation relative au modèle](xref:spa/react).</span><span class="sxs-lookup"><span data-stu-id="d13d7-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>
