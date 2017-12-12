---
uid: webhooks/diagnostics/debugging
title: "WebHooks ASP.NET débogage | Documents Microsoft"
author: rick-anderson
description: "Explique comment déboguer des WebHooks de ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 566ee353f6a947e3ef0efdfd0af3a81dff2147c7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="410e3-103">WebHooks ASP.NET de débogage</span><span class="sxs-lookup"><span data-stu-id="410e3-103">ASP.NET WebHooks debugging</span></span>  

## <a name="debugging-in-azure"></a><span data-ttu-id="410e3-104">Débogage dans Azure</span><span class="sxs-lookup"><span data-stu-id="410e3-104">Debugging in Azure</span></span>

<span data-ttu-id="410e3-105">Pour déboguer votre Application Web lors de l’exécution dans Azure, consultez le didacticiel [dépanner une application web dans Azure App Service à l’aide de Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="410e3-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="410e3-106">Débogage avec Source et des symboles</span><span class="sxs-lookup"><span data-stu-id="410e3-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="410e3-107">En plus du débogage de votre propre code, il est possible de déboguer directement dans Microsoft ASP.NET WebHooks et en fait partie de .NET.</span><span class="sxs-lookup"><span data-stu-id="410e3-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="410e3-108">Cela fonctionne indépendamment de si vous déboguez localement ou à distance.</span><span class="sxs-lookup"><span data-stu-id="410e3-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="410e3-109">Commencez par configurer Visual Studio pour rechercher la source et les symboles en accédant à **déboguer** , puis **Options et paramètres**.</span><span class="sxs-lookup"><span data-stu-id="410e3-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="410e3-110">Définissez les options comme suit :</span><span class="sxs-lookup"><span data-stu-id="410e3-110">Set the options like this:</span></span>

![Options et paramètres](_static/SourceSymbols.png)

<span data-ttu-id="410e3-112">Ajoutez ensuite un lien vers [symbolsource.org](http://symbolsource.org) pour le téléchargement de la source et les symboles.</span><span class="sxs-lookup"><span data-stu-id="410e3-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="410e3-113">Accédez à la **symboles** onglet du menu ci-dessus et ajoutez le code suivant comme un emplacement de symboles :</span><span class="sxs-lookup"><span data-stu-id="410e3-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="410e3-114">En outre, assurez-vous que le répertoire de cache a un nom court ; dans le cas contraire les noms de fichiers peuvent être trop longs, ce qui entraîne les symboles à ne pas se charger.</span><span class="sxs-lookup"><span data-stu-id="410e3-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="410e3-115">Un chemin d’accès de l’exemple est la suivante :</span><span class="sxs-lookup"><span data-stu-id="410e3-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="410e3-116">Les paramètres doivent ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="410e3-116">The settings should look similar to this:</span></span>

![Exemple d’emplacement de fichier de symboles options](_static/SymSource.png)
