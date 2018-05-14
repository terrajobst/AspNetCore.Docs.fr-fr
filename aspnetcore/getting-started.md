---
title: Bien démarrer avec ASP.NET Core
author: rick-anderson
description: Didacticiel rapide qui crée et exécute une application Hello World simple à l’aide d’ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: c2f18c69901a5a6503314d508a776e6985872681
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core"></a>Bien démarrer avec ASP.NET Core

> [!NOTE]
> Les instructions suivantes concernent la dernière version d’ASP.NET Core. Consultez [Bien démarrer avec ASP.NET Core 1.1](xref:getting-started-1.1) pour la version 1.1 de ce document.

1. Installez le [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].

2. Créez un projet .NET Core.

   Sur macOS et Linux, ouvrez une fenêtre de terminal. Sur Windows, ouvrez une invite de commandes. Entrez la commande suivante :

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
3. Exécutez l’application.

    Exécutez les commandes suivantes pour exécuter l’application :

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. Accédez à [http://localhost:5000](http://localhost:5000).

5. Ouvrez <em>Pages/About.cshtml</em>, puis modifiez la page de façon à afficher le message « Hello, world! The time on the server is @DateTime.Now » :

    [!code-html[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Accédez à [http://localhost:5000/About](http://localhost:5000/About) et vérifiez les changements.

### <a name="next-steps"></a>Étapes suivantes

Pour consulter des didacticiels permettant de bien démarrer, consultez [Didacticiels ASP.NET Core](tutorials/index.md).

Pour obtenir une introduction aux concepts et à l’architecture d’ASP.NET Core, consultez [Introduction à ASP.NET Core](index.md) et [Notions de base d’ASP.NET Core](fundamentals/index.md).

Une application ASP.NET Core peut utiliser la bibliothèque de classes de base et le runtime .NET Core ou .NET Framework. Pour plus d’informations, consultez [Choix entre .NET Core et .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
