---
uid: web-forms/videos/aspnet-ajax/how-do-i-use-the-conditional-updatemode-of-the-updatepanel
title: '[Comment faire] Utilisez l’UpdateMode conditionnel du contrôle UpdatePanel ? | Microsoft Docs'
author: JoeStagner
description: Le contrôle UpdatePanel d’ASP.NET AJAX inclut une propriété UpdateMode qui peut être définie sur « Toujours » ou « Conditionnel ». La valeur par défaut est toujours, auquel cas la UpdatePan...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/01/2007
ms.topic: article
ms.assetid: 10b5bad3-4c18-464f-9454-0b3e60b7b8be
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-use-the-conditional-updatemode-of-the-updatepanel
msc.type: video
ms.openlocfilehash: a63c5687e52dd89e1a967bdd7d0dd0539e32ff37
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397381"
---
<a name="how-do-i-use-the-conditional-updatemode-of-the-updatepanel"></a><span data-ttu-id="562b9-105">[Comment faire] Utilisez l’UpdateMode conditionnel du contrôle UpdatePanel ?</span><span class="sxs-lookup"><span data-stu-id="562b9-105">[How Do I:] Use the Conditional UpdateMode of the UpdatePanel?</span></span>
====================
<span data-ttu-id="562b9-106">par [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="562b9-106">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="562b9-107">Le contrôle UpdatePanel d’ASP.NET AJAX inclut une propriété UpdateMode qui peut être définie sur « Toujours » ou « Conditionnel ».</span><span class="sxs-lookup"><span data-stu-id="562b9-107">The ASP.NET AJAX UpdatePanel includes an UpdateMode property that may be set to 'Always' or 'Conditional'.</span></span> <span data-ttu-id="562b9-108">La valeur par défaut est toujours, auquel cas le contrôle UpdatePanel toujours à jour son contenu pendant une publication (postback) asynchrone.</span><span class="sxs-lookup"><span data-stu-id="562b9-108">The default is Always, in which case the UpdatePanel will always update its content during an asychronous postback.</span></span> <span data-ttu-id="562b9-109">Dans cette vidéo, nous apprendre comment nous pouvons définir l’UpdateMode sur Conditional, dans lequel le contrôle UpdatePanel de cas met à jour uniquement son contenu lors de notre code côté serveur appelle sa méthode de mise à jour.</span><span class="sxs-lookup"><span data-stu-id="562b9-109">In this video we learn how we can set the UpdateMode to Conditional, in which case the UpdatePanel will only update its content when our server-side code calls its Update method.</span></span> <span data-ttu-id="562b9-110">Cela vous permet d’utiliser une logique conditionnelle dans votre code c# ou Visual Basic pour déterminer si le contrôle UpdatePanel met à jour son contenu lors de la publication asynchrone actuelle.</span><span class="sxs-lookup"><span data-stu-id="562b9-110">This allows you to use conditional logic in your C# or Visual Basic code to determine whether the UpdatePanel will update its content during the current asynchronous postback.</span></span>

[<span data-ttu-id="562b9-111">&#9654;Regardez la vidéo (13 minutes)</span><span class="sxs-lookup"><span data-stu-id="562b9-111">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-conditional-updatemode-of-the-updatepanel)

> [!div class="step-by-step"]
> <span data-ttu-id="562b9-112">[Précédent](how-do-i-determine-whether-an-asynchronous-postback-has-occurred.md)
> [Suivant](how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel.md)</span><span class="sxs-lookup"><span data-stu-id="562b9-112">[Previous](how-do-i-determine-whether-an-asynchronous-postback-has-occurred.md)
[Next](how-do-i-implement-the-persistent-communications-pattern-with-the-updatepanel.md)</span></span>
