---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
title: Lutte contre robots (c#) | Documents Microsoft
author: wenz
description: Robots automatisés coller les blogs et autres sites Web avec courrier indésirable, l’envoi de formulaires de commentaire sans intervention de l’utilisateur. Le contrôle NoBot dans la Con AJAX ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0a1917e0-884a-4576-8e93-9ed660faae51
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
msc.type: authoredcontent
ms.openlocfilehash: 1ea3aaa5461c2f58a927ae975568f18a34a4729b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872011"
---
<a name="fighting-bots-c"></a><span data-ttu-id="92070-104">Lutte contre robots (c#)</span><span class="sxs-lookup"><span data-stu-id="92070-104">Fighting Bots (C#)</span></span>
====================
<span data-ttu-id="92070-105">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="92070-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="92070-106">[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="92070-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)</span></span>

> <span data-ttu-id="92070-107">Robots automatisés coller les blogs et autres sites Web avec courrier indésirable, l’envoi de formulaires de commentaire sans intervention de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="92070-107">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="92070-108">Le contrôle NoBot dans les outils de contrôle ASP.NET AJAX permettent de combattre les robots.</span><span class="sxs-lookup"><span data-stu-id="92070-108">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>


## <a name="overview"></a><span data-ttu-id="92070-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="92070-109">Overview</span></span>

<span data-ttu-id="92070-110">Robots automatisés coller les blogs et autres sites Web avec courrier indésirable, l’envoi de formulaires de commentaire sans intervention de l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="92070-110">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="92070-111">Le contrôle NoBot dans les outils de contrôle ASP.NET AJAX permettent de combattre les robots.</span><span class="sxs-lookup"><span data-stu-id="92070-111">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="steps"></a><span data-ttu-id="92070-112">Étapes</span><span class="sxs-lookup"><span data-stu-id="92070-112">Steps</span></span>

<span data-ttu-id="92070-113">Une approche commune pour lutter robots est CAPTCHAs entièrement automatisée publique Turing test permet d’indiquer les uns des autres utilisateurs et ordinateurs.</span><span class="sxs-lookup"><span data-stu-id="92070-113">One common approach to defeat bots is to use CAPTCHAs Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="92070-114">Un test Turing a été initialement un test sur lequel un utilisateur nécessaires pour déterminer si un partenaire de communication est un être humain ou un ordinateur.</span><span class="sxs-lookup"><span data-stu-id="92070-114">A Turing test was originally a test where someone needed to decide whether a communication partner is a human or a machine.</span></span> <span data-ttu-id="92070-115">Dans le site web, un test CAPTCHA se compose généralement d’une image avec certaines lettres déformées sur celui-ci.</span><span class="sxs-lookup"><span data-stu-id="92070-115">In the web, a CAPTCHA usually consists of an image with some distorted letters on it.</span></span> <span data-ttu-id="92070-116">L’idée est que seulement une personne peut lire les lettres de l’image, tandis que les algorithmes OCR échouera.</span><span class="sxs-lookup"><span data-stu-id="92070-116">The idea is that only a human can read the letters on the image, whereas OCR algorithms will fail.</span></span>

<span data-ttu-id="92070-117">Il existe plusieurs avantages et inconvénients de cette approche, mais une discussion de ce est dépasse le cadre de ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="92070-117">There are several advantages and disadvantages to this approach, but a discussion of this is beyond the scope of this tutorial.</span></span> <span data-ttu-id="92070-118">Il existe toutefois un contrôle dans les outils de contrôle ASP.NET AJAX qui fournit une approche similaire : `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="92070-118">There is however a control in the ASP.NET AJAX Control Toolkit which provides a similar approach: `NoBot`.</span></span> <span data-ttu-id="92070-119">Il est plus facile de résoudre à un test CAPTCHA, mais est très facile à utiliser et tarifs parfaitement sur les sites Web tels que des blogs, où il est considéré comme un succès si la plupart des tentatives de spam invalidées, ce qui le `NoBot` peut faire.</span><span class="sxs-lookup"><span data-stu-id="92070-119">It is easier to overcome than a CAPTCHA, but is very easy to use and fares extremely well on websites like blogs where it is considered a success if most spam attempts are defeated, which the `NoBot` control can do.</span></span>

<span data-ttu-id="92070-120">`NoBot` intercepte la publication du formulaire web ASP.NET en cours si au moins une des conditions suivantes est remplie :</span><span class="sxs-lookup"><span data-stu-id="92070-120">`NoBot` intercepts the postback of the current ASP.NET web form if at least one of these conditions is met:</span></span>

- <span data-ttu-id="92070-121">Le navigateur ne parvient pas à résoudre un puzzle JavaScript (par exemple lorsque JavaScript est désactivé)</span><span class="sxs-lookup"><span data-stu-id="92070-121">The browser fails to solve a JavaScript puzzle (for instance when JavaScript is deactivated)</span></span>
- <span data-ttu-id="92070-122">L’utilisateur a envoyé le formulaire à rapide</span><span class="sxs-lookup"><span data-stu-id="92070-122">The user submitted the form to fast</span></span>
- <span data-ttu-id="92070-123">L’adresse IP du client a envoyé le formulaire trop souvent dans un certain temps.</span><span class="sxs-lookup"><span data-stu-id="92070-123">The client IP address submitted the form too often in a certain period of time.</span></span>

<span data-ttu-id="92070-124">Pour vérifier ces conditions, la `NoBot` contrôle requiert ces attributs (tous les facultatif) :</span><span class="sxs-lookup"><span data-stu-id="92070-124">In order to check for these conditions, the `NoBot` control requires these attributes (all of them optional):</span></span>

- <span data-ttu-id="92070-125">`ResponseMinimumDelaySeconds` quantité minimale de secondes entre les publications (postback)</span><span class="sxs-lookup"><span data-stu-id="92070-125">`ResponseMinimumDelaySeconds` minimum amount of seconds between postbacks</span></span>
- <span data-ttu-id="92070-126">`CutoffWindowSeconds` longueur d’intervalle de temps dans laquelle les publications (postback) à partir d’une adresse IP est des mesures</span><span class="sxs-lookup"><span data-stu-id="92070-126">`CutoffWindowSeconds` length of time interval in which postbacks from one IP are measures</span></span>
- <span data-ttu-id="92070-127">`CutoffMaximumInstances` quantité maximale de l’intervalle de temps, en secondes</span><span class="sxs-lookup"><span data-stu-id="92070-127">`CutoffMaximumInstances` maximum amount of seconds per time interval</span></span>

<span data-ttu-id="92070-128">Les demandes de balisage suivant au moins deux secondes s’écoulent entre publications (postback) et qu’il existe cinq publications (postback) ou moins dans un intervalle de 30 secondes :</span><span class="sxs-lookup"><span data-stu-id="92070-128">The following markup demands that at least two seconds elapse between postbacks and that there are only five postbacks or less within a 30 seconds interval:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample1.aspx)]

<span data-ttu-id="92070-129">Comme d’habitude veillez à inclure le `ScriptManager` dans la page afin que la bibliothèque ASP.NET AJAX est chargée et les outils de contrôle peut être utilisé :</span><span class="sxs-lookup"><span data-stu-id="92070-129">Then as usual make sure to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample2.aspx)]

<span data-ttu-id="92070-130">Puisque la majorité des contrôles `NoBot` fait se produisent sur le côté serveur, vous devez vérifier le résultat de ces validations.</span><span class="sxs-lookup"><span data-stu-id="92070-130">Since most of the checks `NoBot` is doing occur on the server side, you need to check the result of these validations.</span></span> <span data-ttu-id="92070-131">Cela peut être effectué en appelant `NoBot`de `IsValid()` (méthode).</span><span class="sxs-lookup"><span data-stu-id="92070-131">This can be done by calling `NoBot`'s `IsValid()` method.</span></span> <span data-ttu-id="92070-132">Il possède un seul argument (comme un `out` paramètre /`ByRef` paramètre) qui est de type `NoBotState`.</span><span class="sxs-lookup"><span data-stu-id="92070-132">It has one argument (as an `out` parameter/`ByRef` parameter) which is of type `NoBotState`.</span></span> <span data-ttu-id="92070-133">Représentation sous forme de chaîne contient la raison lorsque la vérification échoue et `Valid` dans le cas contraire.</span><span class="sxs-lookup"><span data-stu-id="92070-133">Its string representation contains the reason when the check fails and `Valid` otherwise.</span></span> <span data-ttu-id="92070-134">Le code suivant génère un message en fonction de `NoBot`du résultat :</span><span class="sxs-lookup"><span data-stu-id="92070-134">The following code outputs a message according to `NoBot`'s result:</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample3.aspx)]

<span data-ttu-id="92070-135">Enfin, vous avez besoin d’un formulaire à envoyer et un élément de l’étiquette pour le message de sortie, et que vous avez terminé !</span><span class="sxs-lookup"><span data-stu-id="92070-135">Finally, you need a form to submit and a label element to output the message, and you are done!</span></span>

[!code-aspx[Main](fighting-bots-cs/samples/sample4.aspx)]

<span data-ttu-id="92070-136">Lorsque vous exécutez ce script et désactivez JavaScript ou envoyez le formulaire dans les deux premières secondes ou envoyez le formulaire sept fois dans les 30 secondes, vous obtiendrez un message d’erreur.</span><span class="sxs-lookup"><span data-stu-id="92070-136">When you run this script and deactivate JavaScript or submit the form within the first two seconds or submit the form seven times within thirty seconds, you will get an error message.</span></span> <span data-ttu-id="92070-137">Toutefois utiliser judicieusement ce contrôle, environ 90-95 % des utilisateurs ayant JavaScript activé, par conséquent, 5 et 10 % des utilisateurs ne sera pas `NoBot`du test.</span><span class="sxs-lookup"><span data-stu-id="92070-137">However use this control wisely, since only about 90-95% of users have JavaScript activated, therefore 5-10% of users will fail `NoBot`'s test.</span></span>


<span data-ttu-id="92070-138">[![Ce message d’erreur peut être dû à un bot](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="92070-138">[![This error message could have been caused by a bot](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)</span></span>

<span data-ttu-id="92070-139">Ce message d’erreur peut être dû à un bot ([cliquez pour afficher l’image en taille réelle](fighting-bots-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="92070-139">This error message could have been caused by a bot ([Click to view full-size image](fighting-bots-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="92070-140">Next</span><span class="sxs-lookup"><span data-stu-id="92070-140">Next</span></span>](fighting-bots-vb.md)
