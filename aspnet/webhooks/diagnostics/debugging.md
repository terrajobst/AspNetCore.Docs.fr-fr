---
uid: webhooks/diagnostics/debugging
title: WebHooks ASP.NET débogage | Microsoft Docs
author: rick-anderson
description: Guide pratique pour déboguer les WebHooks ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 517d282fc22703b5861b748aea51023fa0a12a26
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835629"
---
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="be651-103">WebHooks ASP.NET débogage</span><span class="sxs-lookup"><span data-stu-id="be651-103">ASP.NET WebHooks debugging</span></span>  

## <a name="debugging-in-azure"></a><span data-ttu-id="be651-104">Débogage dans Azure</span><span class="sxs-lookup"><span data-stu-id="be651-104">Debugging in Azure</span></span>

<span data-ttu-id="be651-105">Pour déboguer votre Application Web lors de l’exécution dans Azure, consultez le didacticiel [dépanner une application web dans Azure App Service à l’aide de Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="be651-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="be651-106">Avec la Source et les symboles de débogage</span><span class="sxs-lookup"><span data-stu-id="be651-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="be651-107">En plus du débogage de votre propre code, il est possible de déboguer directement dans Microsoft ASP.NET WebHooks et en fait tous de .NET.</span><span class="sxs-lookup"><span data-stu-id="be651-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="be651-108">Cela fonctionne indépendamment de si vous déboguez localement ou à distance.</span><span class="sxs-lookup"><span data-stu-id="be651-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="be651-109">Tout d’abord, configurer Visual Studio pour trouver la source et les symboles en accédant à **déboguer** , puis **Options et paramètres**.</span><span class="sxs-lookup"><span data-stu-id="be651-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="be651-110">Définir les options comme suit :</span><span class="sxs-lookup"><span data-stu-id="be651-110">Set the options like this:</span></span>

![Options et paramètres](_static/SourceSymbols.png)

<span data-ttu-id="be651-112">Puis ajoutez un lien vers [symbolsource.org](http://symbolsource.org) pour le téléchargement de la source et les symboles.</span><span class="sxs-lookup"><span data-stu-id="be651-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="be651-113">Accédez à la **symboles** onglet du menu ci-dessus et ajoutez le code suivant comme un emplacement de symboles :</span><span class="sxs-lookup"><span data-stu-id="be651-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="be651-114">En outre, assurez-vous que le répertoire de cache a un nom court ; sinon les noms de fichiers peuvent devenir trop longs, ce qui entraîne les symboles à ne pas se charger.</span><span class="sxs-lookup"><span data-stu-id="be651-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="be651-115">Un chemin d’accès de l’exemple est :</span><span class="sxs-lookup"><span data-stu-id="be651-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="be651-116">Les paramètres doivent ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="be651-116">The settings should look similar to this:</span></span>

![Exemple d’emplacement de fichier de symboles options](_static/SymSource.png)
