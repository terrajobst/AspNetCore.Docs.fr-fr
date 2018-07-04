---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
title: Utilisation de DynamicPopulate avec un contrôle utilisateur et le JavaScript (VB) | Microsoft Docs
author: wenz
description: Le contrôle de DynamicPopulate dans ASP.NET AJAX Control Toolkit appelle un service web (ou une méthode de page) et remplit la valeur obtenue dans un contrôle cible sur t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 778b9009-76f2-4665-940e-afc0e35bc917
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 6ab979347a3f412e3225a58a133ae63fcae0a11a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365592"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-vb"></a><span data-ttu-id="144d6-103">Utilisation de DynamicPopulate avec un contrôle utilisateur et le JavaScript (VB)</span><span class="sxs-lookup"><span data-stu-id="144d6-103">Using DynamicPopulate with a User Control And JavaScript (VB)</span></span>
====================
<span data-ttu-id="144d6-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="144d6-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="144d6-105">[Télécharger le Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="144d6-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)</span></span>

> <span data-ttu-id="144d6-106">Le contrôle DynamicPopulate dans ASP.NET AJAX Control Toolkit appelle un service web (ou une méthode de page) et remplit la valeur obtenue dans un contrôle cible dans la page, sans une actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="144d6-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="144d6-107">Il est également possible de déclencher le remplissage à l’aide d’un code JavaScript côté client personnalisé.</span><span class="sxs-lookup"><span data-stu-id="144d6-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="144d6-108">Toutefois, une attention particulière doit être effectuée lorsque l’extendeur se trouve dans un contrôle utilisateur.</span><span class="sxs-lookup"><span data-stu-id="144d6-108">However special care has to be taken when the extender resides in a user control.</span></span>


## <a name="overview"></a><span data-ttu-id="144d6-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="144d6-109">Overview</span></span>

<span data-ttu-id="144d6-110">Le `DynamicPopulate` contrôle dans ASP.NET AJAX Control Toolkit appelle un service web (ou une méthode de page) et remplit la valeur obtenue dans un contrôle cible dans la page, sans une actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="144d6-110">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="144d6-111">Il est également possible de déclencher le remplissage à l’aide d’un code JavaScript côté client personnalisé.</span><span class="sxs-lookup"><span data-stu-id="144d6-111">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="144d6-112">Toutefois, une attention particulière doit être effectuée lorsque l’extendeur se trouve dans un contrôle utilisateur.</span><span class="sxs-lookup"><span data-stu-id="144d6-112">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="steps"></a><span data-ttu-id="144d6-113">Étapes</span><span class="sxs-lookup"><span data-stu-id="144d6-113">Steps</span></span>

<span data-ttu-id="144d6-114">Tout d’abord, vous avez besoin d’un Service Web de ASP.NET qui implémente la méthode doit être appelée par le `DynamicPopulateExtender` contrôle.</span><span class="sxs-lookup"><span data-stu-id="144d6-114">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="144d6-115">Le service web implémente la méthode `getDate()` qui attend un argument de type chaîne, appelée `contextKey`, dans la mesure où le `DynamicPopulate` contrôle envoie une information de contexte à chaque appel de service web.</span><span class="sxs-lookup"><span data-stu-id="144d6-115">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="144d6-116">Voici le code (fichiers `DynamicPopulate.vb.asmx`) qui Récupère la date actuelle dans un des trois formats :</span><span class="sxs-lookup"><span data-stu-id="144d6-116">Here is the code (files `DynamicPopulate.vb.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample1.aspx)]

<span data-ttu-id="144d6-117">Dans l’étape suivante, créez un nouveau contrôle utilisateur (`.ascx` fichier), il est signalé par la déclaration suivante dans sa première ligne :</span><span class="sxs-lookup"><span data-stu-id="144d6-117">In the next step, create a new user control (`.ascx` file), denoted by the following declaration in its first line:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample2.aspx)]

<span data-ttu-id="144d6-118">Un &lt; `label` &gt; élément doit être utilisé pour afficher les données provenant du serveur.</span><span class="sxs-lookup"><span data-stu-id="144d6-118">A &lt;`label`&gt; element will be used to display the data coming from the server.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample3.aspx)]

<span data-ttu-id="144d6-119">Également dans le fichier de contrôle utilisateur, nous allons utiliser trois boutons radio, chacun d’eux représentant une des trois formats de date possibles prises en charge par le service web.</span><span class="sxs-lookup"><span data-stu-id="144d6-119">Also in the user control file, we will use three radio buttons, each one representing one of the three possible date formats supported by the web service.</span></span> <span data-ttu-id="144d6-120">Lorsque l’utilisateur clique sur l’un des boutons radio, le navigateur exécute le code JavaScript qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="144d6-120">When the user clicks on one of the radio buttons, the browser will execute JavaScript code which looks like this:</span></span>

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample4.ps1)]

<span data-ttu-id="144d6-121">Ce code accède à la `DynamicPopulateExtender` (ne vous inquiétez pas sur l’ID étrange encore, ce point sera abordé plus tard) et déclenche le remplissage dynamique des données.</span><span class="sxs-lookup"><span data-stu-id="144d6-121">This code accesses the `DynamicPopulateExtender` (do not worry about the strange ID yet, this will be covered later on) and triggers the dynamic population with data.</span></span> <span data-ttu-id="144d6-122">Dans le contexte de la case actuel, `this.value` fait référence à sa valeur soit `format1`, `format2` ou `format3` exactement ce qu’attend la méthode web.</span><span class="sxs-lookup"><span data-stu-id="144d6-122">In the context of the current radio button, `this.value` refers to its value which is `format1`, `format2` or `format3` exactly what the web method expects.</span></span>

<span data-ttu-id="144d6-123">La seule chose que manquant encore dans le contrôle utilisateur est le `DynamicPopulateExtender` contrôle qui lie les boutons radio au service web.</span><span class="sxs-lookup"><span data-stu-id="144d6-123">The only thing missing in the user control yet is the `DynamicPopulateExtender` control which links the radio buttons to the web service.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample5.aspx)]

<span data-ttu-id="144d6-124">Là encore, vous pouvez noter l’ID étrange utilisé dans le contrôle : `mcd1$myDate` au lieu de `myDate`.</span><span class="sxs-lookup"><span data-stu-id="144d6-124">Again you may note the strange ID used in the control: `mcd1$myDate` instead of `myDate`.</span></span> <span data-ttu-id="144d6-125">Auparavant, le code JavaScript utilisé `mcd1_dpe1` pour accéder à la `DynamicPopulateExtender` au lieu de `dpe1`. Cette stratégie d’affectation de noms est un besoin particulier lorsque vous utilisez `DynamicPopulateExtender` au sein d’un contrôle utilisateur.</span><span class="sxs-lookup"><span data-stu-id="144d6-125">Previously, the JavaScript code used `mcd1_dpe1` to access the `DynamicPopulateExtender` instead of `dpe1`.This naming strategy is a special requirement when using `DynamicPopulateExtender` within a user control.</span></span> <span data-ttu-id="144d6-126">En outre, vous devez incorporer le contrôle utilisateur de manière spécifique pour que tout fonctionne.</span><span class="sxs-lookup"><span data-stu-id="144d6-126">Furthermore, you have to embed the user contol in a specific way to make it all work.</span></span> <span data-ttu-id="144d6-127">Créez une page ASP.NET et inscrire un préfixe de balise pour le contrôle utilisateur que vous venez d’implémenter :</span><span class="sxs-lookup"><span data-stu-id="144d6-127">Create a new ASP.NET page and register a tag prefix for the user control you have just implemented:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample6.aspx)]

<span data-ttu-id="144d6-128">Incluez ensuite ASP.NET AJAX `ScriptManager` contrôle sur la nouvelle page :</span><span class="sxs-lookup"><span data-stu-id="144d6-128">Then, include the ASP.NET AJAX `ScriptManager` control on the new page:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample7.aspx)]

<span data-ttu-id="144d6-129">Enfin, ajoutez le contrôle utilisateur à la page.</span><span class="sxs-lookup"><span data-stu-id="144d6-129">Finally, add the user control to the page.</span></span> <span data-ttu-id="144d6-130">Vous devez uniquement définir son `ID` attribut (et `runat="server"`, bien sûr), mais vous devez également lui attribuer un nom spécifique : `mcd1` puisqu’il s’agit le préfixe utilisé dans le contrôle utilisateur pour accéder à l’aide de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="144d6-130">You only have to set its `ID` attribute (and `runat="server"`, of course), but you also have to set it to a specific name: `mcd1` since this is the prefix used within the user control to access it using JavaScript.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample8.aspx)]

<span data-ttu-id="144d6-131">Et voilà !</span><span class="sxs-lookup"><span data-stu-id="144d6-131">And that's it!</span></span> <span data-ttu-id="144d6-132">La page se comporte comme prévu : un utilisateur clique sur un des boutons radio, le contrôle dans la boîte à outils appelle le service web et affiche la date actuelle au format souhaité.</span><span class="sxs-lookup"><span data-stu-id="144d6-132">The page behaves as expected: A user clicks on one of the radio buttons, the control in the Toolkit calls the web service and displays the current date in the desired format.</span></span>


<span data-ttu-id="144d6-133">[![Les boutons radio résident dans un contrôle utilisateur](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="144d6-133">[![The radio buttons reside in a user control](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)</span></span>

<span data-ttu-id="144d6-134">Les boutons radio résident dans un contrôle utilisateur ([cliquez pour afficher l’image en taille réelle](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="144d6-134">The radio buttons reside in a user control ([Click to view full-size image](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="144d6-135">Précédent</span><span class="sxs-lookup"><span data-stu-id="144d6-135">Previous</span></span>](dynamically-populating-a-control-using-javascript-code-vb.md)
