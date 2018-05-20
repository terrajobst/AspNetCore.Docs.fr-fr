---
title: Bien démarrer avec ASP.NET Core
author: rick-anderson
description: Didacticiel rapide qui crée et exécute une application Hello World simple à l’aide d’ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: e814277663ff5a964171a71ebb6e0f094e0ddc60
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="55d4e-103">Bien démarrer avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="55d4e-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.0"

1. <span data-ttu-id="55d4e-104">Installez le [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="55d4e-104">Install the [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="55d4e-105">Créez un projet .NET Core.</span><span class="sxs-lookup"><span data-stu-id="55d4e-105">Create a new .NET Core project.</span></span>

   <span data-ttu-id="55d4e-106">Sur macOS et Linux, ouvrez une fenêtre de terminal.</span><span class="sxs-lookup"><span data-stu-id="55d4e-106">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="55d4e-107">Sur Windows, ouvrez une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="55d4e-107">On Windows, open a command prompt.</span></span> <span data-ttu-id="55d4e-108">Entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="55d4e-108">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="55d4e-109">Exécutez l’application avec les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="55d4e-109">Run the app with the following commands:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="55d4e-110">Accédez à [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="55d4e-110">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="55d4e-111">Ouvrez *Pages/About.cshtml*, puis modifiez la page de façon à afficher le message « Hello, world!</span><span class="sxs-lookup"><span data-stu-id="55d4e-111">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="55d4e-112">The time on the server is @DateTime.Now » :</span><span class="sxs-lookup"><span data-stu-id="55d4e-112">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="55d4e-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="55d4e-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

6. <span data-ttu-id="55d4e-114">Accédez à [http://localhost:5000/About](http://localhost:5000/About) et vérifiez les changements.</span><span class="sxs-lookup"><span data-stu-id="55d4e-114">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="55d4e-115">Installez le **programme d’installation du SDK** .NET Core pour le SDK 1.0.4 à partir de la [page de tous les téléchargements .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="55d4e-115">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="55d4e-116">Créez un dossier pour un nouveau projet .NET Core.</span><span class="sxs-lookup"><span data-stu-id="55d4e-116">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="55d4e-117">Sur macOS et Linux, ouvrez une fenêtre de terminal.</span><span class="sxs-lookup"><span data-stu-id="55d4e-117">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="55d4e-118">Sur Windows, ouvrez une invite de commandes.</span><span class="sxs-lookup"><span data-stu-id="55d4e-118">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="55d4e-119">Si vous avez installé une version ultérieure du SDK sur votre ordinateur, créez un fichier *global.json* pour sélectionner le SDK 1.0.4.</span><span class="sxs-lookup"><span data-stu-id="55d4e-119">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="55d4e-120">Créez un projet .NET Core.</span><span class="sxs-lookup"><span data-stu-id="55d4e-120">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```

5. <span data-ttu-id="55d4e-121">Restaurez les packages.</span><span class="sxs-lookup"><span data-stu-id="55d4e-121">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

6. <span data-ttu-id="55d4e-122">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="55d4e-122">Run the app.</span></span>

   ```terminal
   dotnet run
   ```

   <span data-ttu-id="55d4e-123">La commande [dotnet run](/dotnet/core/tools/dotnet-run) commence par créer l’application, si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="55d4e-123">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="55d4e-124">Accédez à `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="55d4e-124">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end