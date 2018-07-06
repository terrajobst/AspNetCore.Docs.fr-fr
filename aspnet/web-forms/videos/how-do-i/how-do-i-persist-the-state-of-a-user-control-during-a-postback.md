---
uid: web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
title: '[How Do I]: Persist the State of a User Control During a Postback | Microsoft Docs'
author: rick-anderson
description: Dans cette vidéo Chris Pels montre comment rendre persistant l’état d’un ou plusieurs objets dans un contrôle utilisateur. Tout d’abord, un contrôle utilisateur est créé qui représente l’abilit...
ms.author: aspnetcontent
ms.date: 04/02/2009
ms.assetid: d1bca4c6-838c-40f7-87ec-80bb67e483e5
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
msc.type: video
ms.openlocfilehash: 7862d83d3df3ca5407b7d8fd465cf42da8e7228a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805176"
---
<a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a><span data-ttu-id="86824-103">[Comment] : rendre persistant l’état d’un contrôle utilisateur pendant une publication (postback)</span><span class="sxs-lookup"><span data-stu-id="86824-103">[How Do I]: Persist the State of a User Control During a Postback</span></span>
====================
<span data-ttu-id="86824-104">par [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="86824-104">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="86824-105">Dans cette vidéo Chris Pels montre comment rendre persistant l’état d’un ou plusieurs objets dans un contrôle utilisateur.</span><span class="sxs-lookup"><span data-stu-id="86824-105">In this video Chris Pels shows how to persist the state of one or more objects in a user control.</span></span> <span data-ttu-id="86824-106">Tout d’abord, un contrôle utilisateur est créé qui représente la possibilité pour un utilisateur de spécifier des critères de filtre pour une recherche.</span><span class="sxs-lookup"><span data-stu-id="86824-106">First, a user control is created that represents the ability for a user to specify filter criteria for a search.</span></span> <span data-ttu-id="86824-107">En outre, une classe de filtre associé est créé pour stocker les informations de filtre.</span><span class="sxs-lookup"><span data-stu-id="86824-107">In addition, a companion Filter class is created to store the filter information.</span></span> <span data-ttu-id="86824-108">Plusieurs éléments d’interface utilisateur sont ajoutés au contrôle de filtre, ainsi que certaines méthodes et propriétés pour stocker les informations de filtre actuel dans l’instance de classe de filtre.</span><span class="sxs-lookup"><span data-stu-id="86824-108">Several user interface elements are added to the filter control along with some methods and properties to store the current filter information in the Filter class instance.</span></span> <span data-ttu-id="86824-109">Ensuite, la persistance du contrôle utilisateur est implémentée à l’aide de la méthode de RegisterRequiresControlState et de des méthodes de sauvegarde/restauration associées.</span><span class="sxs-lookup"><span data-stu-id="86824-109">Next, the user control persistence is implemented using the RegisterRequiresControlState method and associated Save/Restore methods.</span></span> <span data-ttu-id="86824-110">Ces méthodes stocker l’instance de la classe de filtre et de ses données pendant les publications de page.</span><span class="sxs-lookup"><span data-stu-id="86824-110">These methods store the instance of the filter class and its data during page postbacks.</span></span> <span data-ttu-id="86824-111">Enfin, il existe une discussion sur le stockage de plusieurs objets dans l’implémentation d’état de contrôle.</span><span class="sxs-lookup"><span data-stu-id="86824-111">Finally, there is a discussion of how to store multiple objects in control state implementation.</span></span>

[<span data-ttu-id="86824-112">&#9654;Regardez la vidéo (23 minutes)</span><span class="sxs-lookup"><span data-stu-id="86824-112">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)
