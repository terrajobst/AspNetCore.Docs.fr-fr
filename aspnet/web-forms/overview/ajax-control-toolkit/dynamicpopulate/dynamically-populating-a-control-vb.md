---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: Remplissage dynamique d’un contrôle (VB) | Microsoft Docs
author: wenz
description: Le contrôle de DynamicPopulate dans ASP.NET AJAX Control Toolkit appelle un service web (ou une méthode de page) et remplit la valeur obtenue dans un contrôle cible sur t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: fd55b59f9375eb320711ffea8d971a8d86179c43
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368658"
---
<a name="dynamically-populating-a-control-vb"></a><span data-ttu-id="a3cae-103">Remplissage dynamique d’un contrôle (VB)</span><span class="sxs-lookup"><span data-stu-id="a3cae-103">Dynamically Populating a Control (VB)</span></span>
====================
<span data-ttu-id="a3cae-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a3cae-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a3cae-105">[Télécharger le Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="a3cae-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)</span></span>

> <span data-ttu-id="a3cae-106">Le contrôle DynamicPopulate dans ASP.NET AJAX Control Toolkit appelle un service web (ou une méthode de page) et remplit la valeur obtenue dans un contrôle cible dans la page, sans une actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="a3cae-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span>


## <a name="overview"></a><span data-ttu-id="a3cae-107">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="a3cae-107">Overview</span></span>

<span data-ttu-id="a3cae-108">Le `DynamicPopulate` contrôle dans ASP.NET AJAX Control Toolkit appelle un service web (ou une méthode de page) et remplit la valeur obtenue dans un contrôle cible dans la page, sans une actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="a3cae-108">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="a3cae-109">Ce didacticiel montre comment configurer ce paramètre.</span><span class="sxs-lookup"><span data-stu-id="a3cae-109">This tutorial shows how to set this up.</span></span>

## <a name="steps"></a><span data-ttu-id="a3cae-110">Étapes</span><span class="sxs-lookup"><span data-stu-id="a3cae-110">Steps</span></span>

<span data-ttu-id="a3cae-111">Tout d’abord, vous avez besoin d’un Service Web de ASP.NET qui implémente la méthode doit être appelée par `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="a3cae-111">First of all, you need an ASP.NET Web Service which implements the method to be called by `DynamicPopulate`.</span></span> <span data-ttu-id="a3cae-112">La classe de service web requiert le `ScriptService` attribut qui est défini dans `Microsoft.Web.Script.Services`; sinon ASP.NET AJAX ne peut pas créer le proxy JavaScript côté client pour le service web, ce qui à son tour, est requis par `DynamicPopulate`.</span><span class="sxs-lookup"><span data-stu-id="a3cae-112">The web service class requires the `ScriptService` attribute which is defined within `Microsoft.Web.Script.Services`; otherwise ASP.NET AJAX cannot create the client-side JavaScript proxy for the web service which in turn is required by `DynamicPopulate`.</span></span>

<span data-ttu-id="a3cae-113">La méthode web doit attendre un argument de type chaîne, appelée `contextKey`, dans la mesure où le `DynamicPopulate` contrôle envoie une information de contexte à chaque appel de service web.</span><span class="sxs-lookup"><span data-stu-id="a3cae-113">The web method must expect one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="a3cae-114">Le service web suivant retourne la date actuelle dans un format représenté par le `contextKey` argument :</span><span class="sxs-lookup"><span data-stu-id="a3cae-114">The following web service returns the current date in a format represented by the `contextKey` argument:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

<span data-ttu-id="a3cae-115">Le service web est ensuite enregistré en tant que `DynamicPopulate.vb.asmx`.</span><span class="sxs-lookup"><span data-stu-id="a3cae-115">The web service is then saved as `DynamicPopulate.vb.asmx`.</span></span> <span data-ttu-id="a3cae-116">Vous pouvez également implémenter le `getDate()` méthode comme une méthode de page au sein de la page ASP.NET avec le `DynamicPopulate` contrôle.</span><span class="sxs-lookup"><span data-stu-id="a3cae-116">Alternatively, you could implement the `getDate()` method as a page method within the actual ASP.NET page with the `DynamicPopulate` control.</span></span>

<span data-ttu-id="a3cae-117">Dans l’étape suivante, créez un nouveau fichier ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a3cae-117">In the next step, create a new ASP.NET file.</span></span> <span data-ttu-id="a3cae-118">Comme toujours, la première étape consiste à inclure le `ScriptManager` dans la page actuelle pour charger la bibliothèque AJAX ASP.NET et pour rendre le travail de la boîte à outils de contrôle :</span><span class="sxs-lookup"><span data-stu-id="a3cae-118">As always, the first step is to include the `ScriptManager` in the current page to load the ASP.NET AJAX library and to make the Control Toolkit work:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

<span data-ttu-id="a3cae-119">Ensuite, ajoutez un contrôle d’étiquette (par exemple en utilisant le contrôle HTML du même nom, ou le &lt; `asp:Label`  / &gt; contrôle web) qui affiche plus tard le résultat de l’appel de service web.</span><span class="sxs-lookup"><span data-stu-id="a3cae-119">Then, add a label control (for instance using the HTML control of the same name, or the &lt;`asp:Label` /&gt; web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

<span data-ttu-id="a3cae-120">Un bouton HTML (comme un contrôle HTML, étant donné que nous ne nécessitent pas une publication (postback) sur le serveur) sera ensuite servir à déclencher le remplissage dynamique :</span><span class="sxs-lookup"><span data-stu-id="a3cae-120">An HTML button (as an HTML control, since we do not require a postback to the server) will then be used to trigger the dynamic population:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

<span data-ttu-id="a3cae-121">Enfin, nous devons le `DynamicPopulateExtender` contrôle pour associer les choses.</span><span class="sxs-lookup"><span data-stu-id="a3cae-121">Finally, we need the `DynamicPopulateExtender` control to wire things up.</span></span> <span data-ttu-id="a3cae-122">Les attributs suivants seront définis (en dehors de celles évident, `ID` et `runat` = `"server"`) :</span><span class="sxs-lookup"><span data-stu-id="a3cae-122">The following attributes will be set (apart from the obvious ones, `ID` and `runat`=`"server"`):</span></span>

- <span data-ttu-id="a3cae-123">`TargetControlID` où placer le résultat de l’appel de service web</span><span class="sxs-lookup"><span data-stu-id="a3cae-123">`TargetControlID` where to put the result from the web service call</span></span>
- <span data-ttu-id="a3cae-124">`ServicePath` chemin d’accès au service web (omettre si vous souhaitez utiliser une méthode de page)</span><span class="sxs-lookup"><span data-stu-id="a3cae-124">`ServicePath` path to the web service (omit if you want to use a page method)</span></span>
- <span data-ttu-id="a3cae-125">`ServiceMethod` nom de la méthode web ou d’une méthode de page</span><span class="sxs-lookup"><span data-stu-id="a3cae-125">`ServiceMethod` name of the web method or page method</span></span>
- <span data-ttu-id="a3cae-126">`ContextKey` informations de contexte à envoyer au service web</span><span class="sxs-lookup"><span data-stu-id="a3cae-126">`ContextKey` context information to be sent to the web service</span></span>
- <span data-ttu-id="a3cae-127">`PopulateTriggerControlID` élément qui déclenche l’appel de service web</span><span class="sxs-lookup"><span data-stu-id="a3cae-127">`PopulateTriggerControlID` element which triggers the web service call</span></span>
- <span data-ttu-id="a3cae-128">`ClearContentsDuringUpdate` s’il faut vide de l’élément cible lors de l’appel de service web</span><span class="sxs-lookup"><span data-stu-id="a3cae-128">`ClearContentsDuringUpdate` whether to empty the target element during the web service call</span></span>

<span data-ttu-id="a3cae-129">Comme vous pouvez le voir, le contrôle nécessite des informations mais tout ce que la mise en place est très simple.</span><span class="sxs-lookup"><span data-stu-id="a3cae-129">As you can see, the control requires some information but putting everything into place is quite straight-forward.</span></span> <span data-ttu-id="a3cae-130">Voici le balisage pour le `DynamicPopulateExtender` contrôle dans le scénario en cours :</span><span class="sxs-lookup"><span data-stu-id="a3cae-130">Here is the markup for the `DynamicPopulateExtender` control in the current scenario:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

<span data-ttu-id="a3cae-131">Exécuter la page ASP.NET dans le navigateur, puis cliquez sur le bouton ; Vous recevrez la date actuelle au format de mois-jour-année.</span><span class="sxs-lookup"><span data-stu-id="a3cae-131">Run the ASP.NET page in the browser and click on the button; you will receive the current date in month-day-year format.</span></span>


<span data-ttu-id="a3cae-132">[![Un clic sur le bouton récupère la date à partir du serveur](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a3cae-132">[![A click on the button retrieves the date from the server](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="a3cae-133">Un clic sur le bouton récupère la date à partir du serveur ([cliquez pour afficher l’image en taille réelle](dynamically-populating-a-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a3cae-133">A click on the button retrieves the date from the server ([Click to view full-size image](dynamically-populating-a-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a3cae-134">[Précédent](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
> [Suivant](dynamically-populating-a-control-using-javascript-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a3cae-134">[Previous](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
[Next](dynamically-populating-a-control-using-javascript-code-vb.md)</span></span>
