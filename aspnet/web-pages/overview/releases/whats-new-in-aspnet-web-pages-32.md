---
uid: web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
title: Quelles sont les nouveautés dans ASP.NET Web Pages 3.2 | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 06/30/2014
ms.assetid: a652beff-8e6b-48ad-bfe4-3703f7ccf0a5
msc.legacyurl: /web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
msc.type: authoredcontent
ms.openlocfilehash: 31b462a3116b29770f534fb95b69a29ae665487c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827398"
---
<a name="whats-new-in-aspnet-web-pages-32"></a>Quelles sont les nouveautés dans ASP.NET Web Pages 3.2
====================
par [Microsoft](https://github.com/microsoft)

Cette rubrique explique quelles sont les nouveautés pour ASP.NET Web Pages 3.2, les Pages Web 3.2.2 et [Web Pages 3.2.3 beta1](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)

## <a name="aspnet-web-pages-32"></a>ASP.NET Web Pages 3.2

Cette version corrige un bogue et introduit une nouvelle fonctionnalité.

## <a name="download"></a>Téléchargement

Les fonctionnalités de runtime sont publiées sous forme de packages NuGet dans la galerie NuGet. Tous les packages de runtime suivent le [Semver](http://semver.org/) spécification. Le package ASP.NET Web Pages 3.2 a la version suivante : &ldquo;3.2.0&rdquo;. Vous pouvez installer ou mettre à jour ces packages via [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebPages/). La version inclut également des packages localisés correspondants sur NuGet.

Vous pouvez installer ou mettre à jour vers les packages NuGet publiées à l’aide de la Console du Gestionnaire de Package NuGet :

[!code-console[Main](whats-new-in-aspnet-web-pages-32/samples/sample1.cmd)]

## <a name="new-feature-and-bug-fix"></a>Nouvelle fonctionnalité et résolution de bogue

Nous avons corrigé un bogue et apportées une de ces améliorations mineures de fonctionnalité dans cette version. Vous pouvez trouver la requête pour le même [ici](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC|v5.2%20RTM&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=Id&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed).

## <a name="aspnet-web-pages-322"></a>ASP.NET Web Pages 3.2.2

Cette version restaure de la modification le [version bêta d’ASP.NET Web Pages 3.2.1](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) qui fournit une amélioration significative des performances lors du rendu des pages razor volumineux. Consultez[Codeplex problème 585](https://aspnetwebstack.codeplex.com/workitem/585). Cette version s’aligne avec le MVC 5.2.2 packages qui seront désormais dépendent de cette version.

Nous avons collaboré avec l’équipe MSN sur le rendu des pages de grande taille. Lorsque les pages afficher plus de 80 kilo-octets de données, nous nous retrouvons avec des objets sur le tas d’objets volumineux. Lorsque plusieurs couches de dispositions sont utilisés, cet effet peut être multiplié.

Le résultat sur le serveur est l’utilisation du processeur supplémentaire, la conservation à longue terme de mémoire et même longue s’interrompt pendant [le nettoyage Gen 2](https://msdn.microsoft.com/en-us/library/ms973837.aspx) dans le garbage collector.

Voici un tableau montrant les résultats d’analyse un [perfview](https://channel9.msdn.com/Series/PerfView-Tutorial) pour une exécution. Le processeur est maintenu constant à environ 68 %, tandis que les grandes pages sont restituées. Le tableau montre que le nombre de collections de génération 2 a été presque entièrement supprimé, et le résultat est le taux de demande plus élevé et une réduction considérable des pauses dus au garbage collection.

| **Zone** | **Avant (3.2)** | **Après (3.2.1)** | **% De delta** |
| --- | --- | --- | --- |
| Total de demandes (nombre) | 26,986 | 32,591 | <font style="background-color: #4bacc6">20.80%</font> |
| Durée de trace (secondes) | 196.20 | 198.60 | 1.20% |
| Requête/seconde | 137.53 | 164.10 | <font style="background-color: #4bacc6">19.30%</font> |
| Charge de l’UC | 68.80% | 68.50% |  -0.40% |
| Exemples d’UC GC | 124,323 | 17,543 | <font style="background-color: #4bacc6">-85.90%</font> |
| Allocations totales (nombre) | 55,357,146 | 57,222,949 | 3.40% |
| Nombre total de Pause du garbage collector (exemples) | 15,091 | 8,515 | <font style="background-color: #4bacc6">-43.60%</font> |
| GC de génération 0 (nombre) | 403 | 1,216 | 201.70% |
| GC de génération 1 (nombre) | 290 | 367 | 26.60% |
| GC de génération 2 (nombre) | 229 | 2 | <font style="background-color: #00ff00">-99.10%</font> |
| Processeur / de la demande (exemples/req) | 19.73 | 16.47 | -16.50% |

| Codage en couleur : | <font style="background-color: #00ff00">Amélioration de Core</font> | <font style="background-color: #4bacc6">Impact positif sur les performances</font> |
|---------------|-----------------------------------------------------------------|-------------------------------------------------------------------------------|
|               |                                                                 |                                                                               |

## <a name="aspnet-web-pages-323-beta1"></a>ASP.NET Web Pages 3.2.3 beta1

Cette version contient uniquement les correctifs de bogues. Vous pouvez utiliser [cette requête](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) pour afficher la liste des problèmes résolus dans cette version.
