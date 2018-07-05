---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[Comment faire] Utilisez la propriété Reponse.Filter pour remplacer le code HTML dans une Page ASP.NET | Microsoft Docs'
author: rick-anderson
description: Dans cette vidéo Chris Pels montre comment utiliser la propriété Reponse.Filter pour intercepter et de modifier le code HTML envoyé à une page. Tout d’abord, un exemple de page est créée w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/29/2009
ms.topic: article
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: f871a3969da21a22bfe10e3a85f43db3e0e522e8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382359"
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a>[Comment faire] Utilisez la propriété Reponse.Filter pour remplacer le code HTML dans une Page ASP.NET
====================
par [Chris Pels](https://twitter.com/chrispels)

Dans cette vidéo Chris Pels montre comment utiliser la propriété Reponse.Filter pour intercepter et de modifier le code HTML envoyé à une page. Tout d’abord, un exemple de page est créé avec du texte simple. Ensuite, une classe Stream personnalisée est créée qui sert le flux de remplacement pour le flux actuel qui est envoyé au navigateur de l’utilisateur. Dans cette classe de flux personnalisée, le contenu de la page est récupéré à partir du flux, modifié et puis écrites dans le flux de réponse. Dans cette classe Stream personnalisée, la méthode Write est personnalisée pour remplacer le code HTML dans le flux de réponse de base, altérant ainsi ce qui est envoyé au navigateur de l’utilisateur. Enfin, la nouvelle classe de flux de données est affectée à la propriété Response.Filter dans la Page\_charge l’événement, ce qui, en fournissant le mécanisme permettant de modifier le contenu de la page.

[&#9654;Regardez la vidéo (13 minutes)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
