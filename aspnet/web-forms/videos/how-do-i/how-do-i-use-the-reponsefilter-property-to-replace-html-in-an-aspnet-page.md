---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[Comment faire] La propriété Reponse.Filter permet de remplacer le code HTML dans une Page ASP.NET | Documents Microsoft'
author: rick-anderson
description: Dans cette Chris Pels vidéo montre comment utiliser la propriété Reponse.Filter pour intercepter et modifier le code HTML envoyé à une page. Un exemple de page est d’abord créé w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/29/2009
ms.topic: article
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 3e7c1f2a947d185d7c2a01deb75e42c92ba914c3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26525528"
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a><span data-ttu-id="8873a-104">[Comment faire] La propriété Reponse.Filter permet de remplacer le code HTML dans une Page ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8873a-104">[How Do I:] Use the Reponse.Filter Property to Replace HTML in an ASP.NET Page</span></span>
====================
<span data-ttu-id="8873a-105">par [Chris PEL](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="8873a-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="8873a-106">Dans cette Chris Pels vidéo montre comment utiliser la propriété Reponse.Filter pour intercepter et modifier le code HTML envoyé à une page.</span><span class="sxs-lookup"><span data-stu-id="8873a-106">In this video Chris Pels shows how to use the Reponse.Filter property to intercept and alter the HTML being sent to a page.</span></span> <span data-ttu-id="8873a-107">Tout d’abord, un exemple de page est créé avec du texte simple.</span><span class="sxs-lookup"><span data-stu-id="8873a-107">First, a sample page is created with some simple text.</span></span> <span data-ttu-id="8873a-108">Ensuite, une classe de flux personnalisée est créée qui sert le flux de remplacement pour le flux actuel est envoyé au navigateur de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8873a-108">Then, a custom Stream class is created which serves as the replacement stream for the current stream being sent to the user's browser.</span></span> <span data-ttu-id="8873a-109">Dans cette classe de flux de données personnalisé, le contenu de la page est récupéré à partir du flux, modifiées et puis écrits dans le flux de réponse.</span><span class="sxs-lookup"><span data-stu-id="8873a-109">In that custom stream class the contents of the page are retrieved from the stream, altered, and then written out to the response stream.</span></span> <span data-ttu-id="8873a-110">Dans cette classe de flux personnalisée, la méthode Write est personnalisée pour remplacer le code HTML dans le flux de réponse de base, altérant ainsi ce qui est envoyé au navigateur de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="8873a-110">In this custom Stream class the Write method is customized to replace the HTML in the base Response stream, thereby altering what is sent to the user's browser.</span></span> <span data-ttu-id="8873a-111">Enfin, la nouvelle classe de flux de données est attribuée à la propriété Response.Filter dans la Page\_charge l’événement, ce qui, en fournissant le mécanisme permettant de modifier le contenu de la page.</span><span class="sxs-lookup"><span data-stu-id="8873a-111">Finally, the new stream class is assigned to the Response.Filter property in the Page\_Load event, thereby, providing the mechanism for altering the page content.</span></span>

[<span data-ttu-id="8873a-112">&#9654; Regardez la vidéo (13 minutes)</span><span class="sxs-lookup"><span data-stu-id="8873a-112">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
