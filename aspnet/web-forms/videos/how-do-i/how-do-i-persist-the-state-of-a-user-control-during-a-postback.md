---
uid: web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
title: '[Comment] : rendre persistant l’état d’un contrôle utilisateur au cours d’une publication (postback) | Documents Microsoft'
author: rick-anderson
description: Dans cette Chris Pels vidéo montre comment rendre persistant l’état d’un ou plusieurs objets dans un contrôle utilisateur. Tout d’abord, un contrôle utilisateur est créé qui représente l’abilit...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/02/2009
ms.topic: article
ms.assetid: d1bca4c6-838c-40f7-87ec-80bb67e483e5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
msc.type: video
ms.openlocfilehash: 47d7d7a3f83586104ab2d2a3c288b4a51879ca06
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26525968"
---
<a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a><span data-ttu-id="930f1-104">[Comment] : rendre persistant l’état d’un contrôle utilisateur au cours d’une publication (postback)</span><span class="sxs-lookup"><span data-stu-id="930f1-104">[How Do I]: Persist the State of a User Control During a Postback</span></span>
====================
<span data-ttu-id="930f1-105">par [Chris PEL](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="930f1-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="930f1-106">Dans cette Chris Pels vidéo montre comment rendre persistant l’état d’un ou plusieurs objets dans un contrôle utilisateur.</span><span class="sxs-lookup"><span data-stu-id="930f1-106">In this video Chris Pels shows how to persist the state of one or more objects in a user control.</span></span> <span data-ttu-id="930f1-107">Tout d’abord, un contrôle utilisateur est créé qui représente la capacité pour un utilisateur de spécifier des critères de filtre pour la recherche.</span><span class="sxs-lookup"><span data-stu-id="930f1-107">First, a user control is created that represents the ability for a user to specify filter criteria for a search.</span></span> <span data-ttu-id="930f1-108">En outre, une classe de filtre d’accompagnement est créé pour stocker les informations de filtre.</span><span class="sxs-lookup"><span data-stu-id="930f1-108">In addition, a companion Filter class is created to store the filter information.</span></span> <span data-ttu-id="930f1-109">Plusieurs éléments d’interface utilisateur sont ajoutés au contrôle de filtre, ainsi que certaines méthodes et propriétés pour stocker les informations de filtre actuel dans l’instance de classe de filtre.</span><span class="sxs-lookup"><span data-stu-id="930f1-109">Several user interface elements are added to the filter control along with some methods and properties to store the current filter information in the Filter class instance.</span></span> <span data-ttu-id="930f1-110">Ensuite, la persistance du contrôle utilisateur est implémentée à l’aide de la RegisterRequiresControlState méthode et les méthodes associées de la sauvegarde/restauration.</span><span class="sxs-lookup"><span data-stu-id="930f1-110">Next, the user control persistence is implemented using the RegisterRequiresControlState method and associated Save/Restore methods.</span></span> <span data-ttu-id="930f1-111">Ces méthodes stockent l’instance de la classe de filtre et ses données au cours de la page publications (postback).</span><span class="sxs-lookup"><span data-stu-id="930f1-111">These methods store the instance of the filter class and its data during page postbacks.</span></span> <span data-ttu-id="930f1-112">Enfin, il est en savoir plus sur le stockage de plusieurs objets dans l’implémentation de l’état contrôle.</span><span class="sxs-lookup"><span data-stu-id="930f1-112">Finally, there is a discussion of how to store multiple objects in control state implementation.</span></span>

[<span data-ttu-id="930f1-113">&#9654; Regardez la vidéo (minutes 23)</span><span class="sxs-lookup"><span data-stu-id="930f1-113">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)
