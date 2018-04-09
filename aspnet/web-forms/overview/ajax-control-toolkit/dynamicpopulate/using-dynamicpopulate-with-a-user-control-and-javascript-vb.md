---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
title: À l’aide de DynamicPopulate avec un contrôle utilisateur et le JavaScript (VB) | Documents Microsoft
author: wenz
description: Le contrôle DynamicPopulate dans les outils de contrôle ASP.NET AJAX appelle un service web (ou une méthode de page) et remplit la valeur obtenue dans un contrôle cible t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 778b9009-76f2-4665-940e-afc0e35bc917
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 715973ef4923e635ec2a860d00d55f13a5f8b2d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-vb"></a><span data-ttu-id="f20cc-103">À l’aide de DynamicPopulate avec un contrôle utilisateur et le JavaScript (VB)</span><span class="sxs-lookup"><span data-stu-id="f20cc-103">Using DynamicPopulate with a User Control And JavaScript (VB)</span></span>
====================
<span data-ttu-id="f20cc-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f20cc-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f20cc-105">[Télécharger le Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f20cc-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)</span></span>

> <span data-ttu-id="f20cc-106">Le contrôle DynamicPopulate dans les outils de contrôle ASP.NET AJAX appelle un service web (ou une méthode de page) et remplit la valeur obtenue dans un contrôle cible dans la page, sans une actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="f20cc-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="f20cc-107">Il est également possible de déclencher le remplissage à l’aide du code JavaScript côté client personnalisé.</span><span class="sxs-lookup"><span data-stu-id="f20cc-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="f20cc-108">Toutefois, une attention particulière doit entreprendre lorsque l’extendeur se trouve dans un contrôle utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f20cc-108">However special care has to be taken when the extender resides in a user control.</span></span>


## <a name="overview"></a><span data-ttu-id="f20cc-109">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="f20cc-109">Overview</span></span>

<span data-ttu-id="f20cc-110">Le `DynamicPopulate` contrôle dans la boîte à outils de contrôle ASP.NET AJAX appelle un service web (ou une méthode de page) et remplit la valeur obtenue dans un contrôle cible dans la page, sans une actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="f20cc-110">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="f20cc-111">Il est également possible de déclencher le remplissage à l’aide du code JavaScript côté client personnalisé.</span><span class="sxs-lookup"><span data-stu-id="f20cc-111">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="f20cc-112">Toutefois, une attention particulière doit entreprendre lorsque l’extendeur se trouve dans un contrôle utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f20cc-112">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="steps"></a><span data-ttu-id="f20cc-113">Étapes</span><span class="sxs-lookup"><span data-stu-id="f20cc-113">Steps</span></span>

<span data-ttu-id="f20cc-114">Vous devez tout d’abord, Service Web ASP.NET qui implémente la méthode à être appelée par le `DynamicPopulateExtender` contrôle.</span><span class="sxs-lookup"><span data-stu-id="f20cc-114">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="f20cc-115">Le service web implémente la méthode `getDate()` qui attend un argument de type chaîne, appelée `contextKey`, étant donné que le `DynamicPopulate` contrôle envoie une partie des informations de contexte à chaque appel de service web.</span><span class="sxs-lookup"><span data-stu-id="f20cc-115">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="f20cc-116">Voici le code (fichiers `DynamicPopulate.vb.asmx`) qui Récupère la date actuelle dans un des trois formats :</span><span class="sxs-lookup"><span data-stu-id="f20cc-116">Here is the code (files `DynamicPopulate.vb.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample1.aspx)]

<span data-ttu-id="f20cc-117">Dans l’étape suivante, créez un nouveau contrôle utilisateur (`.ascx` fichier), il est signalé par la déclaration suivante dans la première ligne :</span><span class="sxs-lookup"><span data-stu-id="f20cc-117">In the next step, create a new user control (`.ascx` file), denoted by the following declaration in its first line:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample2.aspx)]

<span data-ttu-id="f20cc-118">A &lt; `label` &gt; élément servira pour afficher les données provenant du serveur.</span><span class="sxs-lookup"><span data-stu-id="f20cc-118">A &lt;`label`&gt; element will be used to display the data coming from the server.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample3.aspx)]

<span data-ttu-id="f20cc-119">Également dans le fichier de contrôle utilisateur, nous utiliserons des trois cases d’option, chacune représentant un des trois formats de date possibles prises en charge par le service web.</span><span class="sxs-lookup"><span data-stu-id="f20cc-119">Also in the user control file, we will use three radio buttons, each one representing one of the three possible date formats supported by the web service.</span></span> <span data-ttu-id="f20cc-120">Lorsque l’utilisateur clique sur l’un des boutons radio, le navigateur exécute le code JavaScript qui ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="f20cc-120">When the user clicks on one of the radio buttons, the browser will execute JavaScript code which looks like this:</span></span>

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample4.ps1)]

<span data-ttu-id="f20cc-121">Ce code accède à la `DynamicPopulateExtender` (ne vous inquiétez pas sur l’ID d’étrange encore, ce point sera abordé plus tard) et déclenche le remplissage dynamique avec des données.</span><span class="sxs-lookup"><span data-stu-id="f20cc-121">This code accesses the `DynamicPopulateExtender` (do not worry about the strange ID yet, this will be covered later on) and triggers the dynamic population with data.</span></span> <span data-ttu-id="f20cc-122">Dans le contexte de la case en cours, `this.value` fait référence à sa valeur, qui est `format1`, `format2` ou `format3` exactement ce qu’attend la méthode web.</span><span class="sxs-lookup"><span data-stu-id="f20cc-122">In the context of the current radio button, `this.value` refers to its value which is `format1`, `format2` or `format3` exactly what the web method expects.</span></span>

<span data-ttu-id="f20cc-123">La seule chose que manquant encore dans le contrôle utilisateur est le `DynamicPopulateExtender` contrôle qui lie les cases au service web.</span><span class="sxs-lookup"><span data-stu-id="f20cc-123">The only thing missing in the user control yet is the `DynamicPopulateExtender` control which links the radio buttons to the web service.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample5.aspx)]

<span data-ttu-id="f20cc-124">À nouveau, vous pouvez noter l’ID étrange utilisé dans le contrôle : `mcd1$myDate` au lieu de `myDate`.</span><span class="sxs-lookup"><span data-stu-id="f20cc-124">Again you may note the strange ID used in the control: `mcd1$myDate` instead of `myDate`.</span></span> <span data-ttu-id="f20cc-125">Auparavant, le code JavaScript utilisé `mcd1_dpe1` pour accéder à la `DynamicPopulateExtender` au lieu de `dpe1`. Cette stratégie d’affectation de noms est un besoin particulier lorsque vous utilisez `DynamicPopulateExtender` au sein d’un contrôle utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f20cc-125">Previously, the JavaScript code used `mcd1_dpe1` to access the `DynamicPopulateExtender` instead of `dpe1`.This naming strategy is a special requirement when using `DynamicPopulateExtender` within a user control.</span></span> <span data-ttu-id="f20cc-126">En outre, vous devez incorporer le contrôle utilisateur de manière spécifique pour que tout fonctionne.</span><span class="sxs-lookup"><span data-stu-id="f20cc-126">Furthermore, you have to embed the user contol in a specific way to make it all work.</span></span> <span data-ttu-id="f20cc-127">Créer une page ASP.NET et inscrire un préfixe de balise pour le contrôle utilisateur que vous venez d’implémenter :</span><span class="sxs-lookup"><span data-stu-id="f20cc-127">Create a new ASP.NET page and register a tag prefix for the user control you have just implemented:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample6.aspx)]

<span data-ttu-id="f20cc-128">Ensuite, inclure ASP.NET AJAX `ScriptManager` contrôle sur la nouvelle page :</span><span class="sxs-lookup"><span data-stu-id="f20cc-128">Then, include the ASP.NET AJAX `ScriptManager` control on the new page:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample7.aspx)]

<span data-ttu-id="f20cc-129">Enfin, ajoutez le contrôle utilisateur à la page.</span><span class="sxs-lookup"><span data-stu-id="f20cc-129">Finally, add the user control to the page.</span></span> <span data-ttu-id="f20cc-130">Il vous suffit de définir son `ID` attribut (et `runat="server"`, bien sûr), mais vous devez également lui affecter un nom spécifique : `mcd1` puisque c’est le préfixe utilisé dans le contrôle utilisateur pour accéder à l’aide de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f20cc-130">You only have to set its `ID` attribute (and `runat="server"`, of course), but you also have to set it to a specific name: `mcd1` since this is the prefix used within the user control to access it using JavaScript.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample8.aspx)]

<span data-ttu-id="f20cc-131">Et voilà !</span><span class="sxs-lookup"><span data-stu-id="f20cc-131">And that's it!</span></span> <span data-ttu-id="f20cc-132">La page se comporte comme prévu : un utilisateur clique sur l’un des boutons radio, le contrôle de la boîte à outils appelle le service web et affiche la date actuelle au format souhaité.</span><span class="sxs-lookup"><span data-stu-id="f20cc-132">The page behaves as expected: A user clicks on one of the radio buttons, the control in the Toolkit calls the web service and displays the current date in the desired format.</span></span>


<span data-ttu-id="f20cc-133">[![Les boutons de case d’option se trouvent dans un contrôle utilisateur](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f20cc-133">[![The radio buttons reside in a user control](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)</span></span>

<span data-ttu-id="f20cc-134">Les boutons de case d’option se trouvent dans un contrôle utilisateur ([cliquez pour afficher l’image en taille réelle](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f20cc-134">The radio buttons reside in a user control ([Click to view full-size image](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f20cc-135">Précédent</span><span class="sxs-lookup"><span data-stu-id="f20cc-135">Previous</span></span>](dynamically-populating-a-control-using-javascript-code-vb.md)
