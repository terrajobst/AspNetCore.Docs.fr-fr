---
title: Bien démarrer avec ASP.NET Core 1.1
author: rick-anderson
description: Suivez ce tutoriel rapide pour créer et exécuter une application Hello World simple à l’aide d’ASP.NET Core 1.1.
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started-1.1
ms.openlocfilehash: c61a9a918e51bbd6c1f1142a04473393c8fc54ca
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="get-started-with-aspnet-core-11"></a>Bien démarrer avec ASP.NET Core 1.1

> [!NOTE]
> Les instructions suivantes concernent ASP.NET Core 1.1. Si vous recherchez la dernière version, consultez [la version actuelle de ce tutoriel](xref:getting-started).

1. Installez le **programme d’installation du SDK** .NET Core pour le SDK 1.0.4 à partir de la [page de tous les téléchargements .NET Core](https://www.microsoft.com/net/download/all).

2. Créez un dossier pour un nouveau projet .NET Core.

   Sur macOS et Linux, ouvrez une fenêtre de terminal. Sur Windows, ouvrez une invite de commandes.

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. Si vous avez installé une version ultérieure du SDK sur votre ordinateur, créez un fichier *global.json* pour sélectionner le SDK 1.0.4.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. Créez un projet .NET Core.

   ```terminal
   dotnet new web
   ```
   
3.  Restaurez les packages.

    ```terminal
    dotnet restore
    ```

4. Exécutez l’application.

   La commande [dotnet run](/dotnet/core/tools/dotnet-run) commence par créer l’application, si nécessaire.

   ```terminal
   dotnet run
   ```

5. Accédez à `http://localhost:5000`.

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a>Étapes suivantes

Pour consulter des didacticiels permettant de bien démarrer, consultez [Didacticiels ASP.NET Core](tutorials/index.md).

Pour obtenir une introduction aux concepts et à l’architecture d’ASP.NET Core, consultez [Introduction à ASP.NET Core](index.md) et [Notions de base d’ASP.NET Core](fundamentals/index.md).

Une application ASP.NET Core peut utiliser la bibliothèque de classes de base et le runtime .NET Core ou .NET Framework. Pour plus d’informations, consultez [Choix entre .NET Core et .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
