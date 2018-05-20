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
# <a name="get-started-with-aspnet-core"></a>Bien démarrer avec ASP.NET Core

::: moniker range=">= aspnetcore-2.0"

1. Installez le [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].

2. Créez un projet .NET Core.

   Sur macOS et Linux, ouvrez une fenêtre de terminal. Sur Windows, ouvrez une invite de commandes. Entrez la commande suivante :

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```

3. Exécutez l’application avec les commandes suivantes :

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. Accédez à [http://localhost:5000](http://localhost:5000).

5. Ouvrez *Pages/About.cshtml*, puis modifiez la page de façon à afficher le message « Hello, world! The time on the server is @DateTime.Now » :

    [!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Accédez à [http://localhost:5000/About](http://localhost:5000/About) et vérifiez les changements.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. Installez le **programme d’installation du SDK** .NET Core pour le SDK 1.0.4 à partir de la [page de tous les téléchargements .NET Core](https://www.microsoft.com/net/download/all).

2. Créez un dossier pour un nouveau projet .NET Core.

   Sur macOS et Linux, ouvrez une fenêtre de terminal. Sur Windows, ouvrez une invite de commandes.

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. Si vous avez installé une version ultérieure du SDK sur votre ordinateur, créez un fichier *global.json* pour sélectionner le SDK 1.0.4.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. Créez un projet .NET Core.

   ```terminal
   dotnet new web
   ```

5. Restaurez les packages.

    ```terminal
    dotnet restore
    ```

6. Exécutez l’application.

   ```terminal
   dotnet run
   ```

   La commande [dotnet run](/dotnet/core/tools/dotnet-run) commence par créer l’application, si nécessaire.

7. Accédez à `http://localhost:5000`.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end