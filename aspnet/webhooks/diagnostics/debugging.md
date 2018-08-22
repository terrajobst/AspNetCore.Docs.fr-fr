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
# <a name="aspnet-webhooks-debugging"></a>WebHooks ASP.NET débogage  

## <a name="debugging-in-azure"></a>Débogage dans Azure

Pour déboguer votre Application Web lors de l’exécution dans Azure, consultez le didacticiel [dépanner une application web dans Azure App Service à l’aide de Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Avec la Source et les symboles de débogage

En plus du débogage de votre propre code, il est possible de déboguer directement dans Microsoft ASP.NET WebHooks et en fait tous de .NET. Cela fonctionne indépendamment de si vous déboguez localement ou à distance. Tout d’abord, configurer Visual Studio pour trouver la source et les symboles en accédant à **déboguer** , puis **Options et paramètres**. Définir les options comme suit :

![Options et paramètres](_static/SourceSymbols.png)

Puis ajoutez un lien vers [symbolsource.org](http://symbolsource.org) pour le téléchargement de la source et les symboles. Accédez à la **symboles** onglet du menu ci-dessus et ajoutez le code suivant comme un emplacement de symboles :

```
http://srv.symbolsource.org/pdb/Public
```

En outre, assurez-vous que le répertoire de cache a un nom court ; sinon les noms de fichiers peuvent devenir trop longs, ce qui entraîne les symboles à ne pas se charger. Un chemin d’accès de l’exemple est :

```
C:\SymCache
```

Les paramètres doivent ressembler à ceci :

![Exemple d’emplacement de fichier de symboles options](_static/SymSource.png)
