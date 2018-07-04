---
uid: web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
title: '[Comment faire] Choisir parmi les méthodes d’AJAX Page mises à jour ? | Microsoft Docs'
author: JoeStagner
description: Dans cette vidéo, Joe Stagner compare les deux principales méthodes de mise à jour de page de style AJAX dans une application ASP.NET. La première méthode consiste à utiliser un Upd...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2007
ms.topic: article
ms.assetid: a5e33a7d-ccb2-483f-a955-3d39f72ba4ec
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
msc.type: video
ms.openlocfilehash: c092a7dcaeeeec53e899bb1cdf12f9bb02a99597
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37392050"
---
<a name="how-do-i-choose-between-methods-of-ajax-page-updates"></a>[Comment faire] Choisir parmi les méthodes d’AJAX Page mises à jour ?
====================
par [Joe Stagner](https://github.com/JoeStagner)

Dans cette vidéo, Joe Stagner compare les deux principales méthodes de mise à jour de page de style AJAX dans une application ASP.NET. La première méthode consiste à utiliser un UpdatePanel, où aucun code supplémentaire ne doit être écrit sur le côté client ou côté serveur. L’avantage d’utiliser le contrôle UpdatePanel est que tout fonctionne automatiquement. La pénalité est que, au niveau du client, qu'elle nécessite une grande quantité de données à inclure dans la requête AJAX et la réponse et au niveau du serveur, qu'elle nécessite un cycle de vie de page entière doit être exécuté. La deuxième méthode consiste à utiliser des rappels de réseau, où le code supplémentaire doit être écrit sur le côté client et côté serveur. L’avantage d’utiliser des rappels réseau est qu’au niveau du client, elle nécessite très peu de données à inclure dans la requête AJAX et la réponse et au niveau du serveur nécessite uniquement la méthode de service appelé doit être exécuté. La dégradation significative des est le temps et l’effort que nécessaire pour écrire le code nécessaire. Joe conclut la vidéo en expliquant que vous devez prendre en compte lors du choix entre les deux principales méthodes de mises à jour de page de style AJAX. (Cette vidéo utilise le code à partir de la [comment pour démarrer avec ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) vidéo et le [comment faire pour effectuer des rappels réseau côté Client avec ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) vidéo.)

[&#9654;Regardez la vidéo (11 minutes)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-choose-between-methods-of-ajax-page-updates)

> [!div class="step-by-step"]
> [Précédent](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
> [Suivant](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)
