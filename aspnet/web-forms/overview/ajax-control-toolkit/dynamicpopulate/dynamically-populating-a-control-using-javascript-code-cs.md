---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: Remplissage dynamique d’un contrôle à l’aide de Code JavaScript (c#) | Microsoft Docs
author: wenz
description: Le contrôle de DynamicPopulate dans ASP.NET AJAX Control Toolkit appelle un service web (ou une méthode de page) et remplit la valeur obtenue dans un contrôle cible sur t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 6999dee71966c4362254d9f601e55fdb75d0b292
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394072"
---
<a name="dynamically-populating-a-control-using-javascript-code-c"></a><span data-ttu-id="1b806-103">Remplissage dynamique d’un contrôle à l’aide de Code JavaScript (c#)</span><span class="sxs-lookup"><span data-stu-id="1b806-103">Dynamically Populating a Control Using JavaScript Code (C#)</span></span>
====================
<span data-ttu-id="1b806-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1b806-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1b806-105">[Télécharger le Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="1b806-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)</span></span>

> <span data-ttu-id="1b806-106">Le contrôle DynamicPopulate dans ASP.NET AJAX Control Toolkit appelle un service web (ou une méthode de page) et remplit la valeur obtenue dans un contrôle cible dans la page, sans une actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="1b806-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="1b806-107">Il est également possible de déclencher le remplissage à l’aide d’un code JavaScript côté client personnalisé.</span><span class="sxs-lookup"><span data-stu-id="1b806-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="1b806-108">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="1b806-108">Overview</span></span>

<span data-ttu-id="1b806-109">Le `DynamicPopulate` contrôle dans ASP.NET AJAX Control Toolkit appelle un service web (ou une méthode de page) et remplit la valeur obtenue dans un contrôle cible dans la page, sans une actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="1b806-109">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="1b806-110">Il est également possible de déclencher le remplissage à l’aide d’un code JavaScript côté client personnalisé.</span><span class="sxs-lookup"><span data-stu-id="1b806-110">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="1b806-111">Étapes</span><span class="sxs-lookup"><span data-stu-id="1b806-111">Steps</span></span>

<span data-ttu-id="1b806-112">Tout d’abord, vous avez besoin d’un Service Web de ASP.NET qui implémente la méthode doit être appelée par le `DynamicPopulateExtender` contrôle.</span><span class="sxs-lookup"><span data-stu-id="1b806-112">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="1b806-113">Le service web implémente la méthode `getDate()` qui attend un argument de type chaîne, appelée `contextKey`, dans la mesure où le `DynamicPopulate` contrôle envoie une information de contexte à chaque appel de service web.</span><span class="sxs-lookup"><span data-stu-id="1b806-113">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="1b806-114">Voici le code (fichier `DynamicPopulate.cs.asmx`) qui Récupère la date actuelle dans un des trois formats :</span><span class="sxs-lookup"><span data-stu-id="1b806-114">Here is the code (file `DynamicPopulate.cs.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

<span data-ttu-id="1b806-115">Dans l’étape suivante, créer un nouveau site ASP.NET et démarrer avec le contrôle ASP.NET AJAX ScriptManager :</span><span class="sxs-lookup"><span data-stu-id="1b806-115">In the next step, create a new ASP.NET site and start with the ASP.NET AJAX ScriptManager control:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

<span data-ttu-id="1b806-116">Ensuite, ajoutez un contrôle d’étiquette (par exemple en utilisant le contrôle HTML du même nom, ou le `<asp:Label />` contrôle web) qui affiche plus tard le résultat de l’appel de service web.</span><span class="sxs-lookup"><span data-stu-id="1b806-116">Then, add a label control (for instance using the HTML control of the same name, or the `<asp:Label />` web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

<span data-ttu-id="1b806-117">Ensuite, incluez un `DynamicPopulateExtender` contrôlent et fournissent des informations sur le service web, contrôle cible, mais pas le nom du contrôle qui déclenche le remplissage de cette opération est effectuée par la suite à l’aide de code JavaScript personnalisé !</span><span class="sxs-lookup"><span data-stu-id="1b806-117">Next, include a `DynamicPopulateExtender` control and provide web service information, target control, but not the name of the control which triggers the population this will be done later on, using custom JavaScript!</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

<span data-ttu-id="1b806-118">Maintenant à la partie de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1b806-118">Now to the JavaScript part.</span></span> <span data-ttu-id="1b806-119">Le `$find()` fonction, définie par la bibliothèque ASP.NET AJAX, retourne une référence aux objets côté serveur d’ASP.NET AJAX Control Toolkit comme `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="1b806-119">The `$find()` function, defined by the ASP.NET AJAX library, returns a reference to server-side objects of the ASP.NET AJAX Control Toolkit such as `DynamicPopulateExtender`.</span></span> <span data-ttu-id="1b806-120">Dans le fichier actuel, `$find("dpe")` retourne une référence à le `DynamicPopulateExtender` contrôle dans la page.</span><span class="sxs-lookup"><span data-stu-id="1b806-120">In the current file, `$find("dpe")` returns a reference to the one `DynamicPopulateExtender` control in the page.</span></span> <span data-ttu-id="1b806-121">Il expose une méthode appelée `populate()` qui déclenche le processus de remplissage dynamique.</span><span class="sxs-lookup"><span data-stu-id="1b806-121">It exposes a method called `populate()` which triggers the dynamic population process.</span></span> <span data-ttu-id="1b806-122">Le `populate()` méthode nécessite un seul argument : la clé de contexte qui servira d’en tant qu’argument à la `getDate()` méthode web.</span><span class="sxs-lookup"><span data-stu-id="1b806-122">The `populate()` method requires one argument: the context key which will serve as argument to the `getDate()` web method.</span></span> <span data-ttu-id="1b806-123">Ainsi, par exemple, `$find("dpe").populate("format1")` peut remplir l’étiquette avec la date actuelle au format mois-jour-année.</span><span class="sxs-lookup"><span data-stu-id="1b806-123">So for instance, `$find("dpe").populate("format1")` would populate the label with the current date in month-day-year format.</span></span>

<span data-ttu-id="1b806-124">Afin de rendre l’exemple un peu plus souple, l’utilisateur peut désormais choisir entre plusieurs formats de date.</span><span class="sxs-lookup"><span data-stu-id="1b806-124">In order to make the sample a bit more flexible, the user may now choose between several date formats.</span></span> <span data-ttu-id="1b806-125">Pour chacun d'entre eux, une case s’affiche.</span><span class="sxs-lookup"><span data-stu-id="1b806-125">For each one of them, a radio button is displayed.</span></span> <span data-ttu-id="1b806-126">Une fois l’utilisateur clique sur un bouton radio, code JavaScript remplit dynamiquement l’étiquette avec le format de date sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="1b806-126">Once the user clicks on a radio button, JavaScript code dynamically populates the label with the selected date format.</span></span> <span data-ttu-id="1b806-127">Voici ces cases :</span><span class="sxs-lookup"><span data-stu-id="1b806-127">Here are those radio buttons:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

<span data-ttu-id="1b806-128">Notez que dans le contexte d’un bouton radio, l’expression JavaScript `this.value` fait référence à la valeur du bouton actuel, qui se trouve être exactement les mêmes informations le `getDate()` méthode peut fonctionner avec.</span><span class="sxs-lookup"><span data-stu-id="1b806-128">Note that within the context of a radio button, the JavaScript expression `this.value` refers to the value of the current button, which happens to be exactly the same information the `getDate()` method can work with.</span></span>


<span data-ttu-id="1b806-129">[![Un clic sur le bouton récupère la date à partir du serveur, dans le format spécifié](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1b806-129">[![A click on the button retrieves the date from the server, in the format specified](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="1b806-130">Un clic sur le bouton récupère la date à partir du serveur, dans le format spécifié ([cliquez pour afficher l’image en taille réelle](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1b806-130">A click on the button retrieves the date from the server, in the format specified ([Click to view full-size image](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1b806-131">[Précédent](dynamically-populating-a-control-cs.md)
> [Suivant](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)</span><span class="sxs-lookup"><span data-stu-id="1b806-131">[Previous](dynamically-populating-a-control-cs.md)
[Next](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)</span></span>
