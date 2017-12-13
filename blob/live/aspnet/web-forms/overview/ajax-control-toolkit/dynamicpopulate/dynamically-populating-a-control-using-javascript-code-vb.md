---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
title: "Remplissage dynamique d’un contrôle à l’aide du Code JavaScript (VB) | Documents Microsoft"
author: wenz
description: "Le contrôle DynamicPopulate dans les outils de contrôle ASP.NET AJAX appelle un service web (ou une méthode de page) et remplit la valeur obtenue dans un contrôle cible t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 90582e54-3e90-432a-9da5-689fb39ed56b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-vb
msc.type: authoredcontent
ms.openlocfilehash: b4090b3a785059c8f09de266df79eba0914e9f13
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-populating-a-control-using-javascript-code-vb"></a><span data-ttu-id="416b7-103">Remplissage dynamique d’un contrôle à l’aide du Code JavaScript (VB)</span><span class="sxs-lookup"><span data-stu-id="416b7-103">Dynamically Populating a Control Using JavaScript Code (VB)</span></span>
====================
<span data-ttu-id="416b7-104">par [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="416b7-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="416b7-105">[Télécharger le Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="416b7-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1VB.pdf)</span></span>

> <span data-ttu-id="416b7-106">Le contrôle DynamicPopulate dans les outils de contrôle ASP.NET AJAX appelle un service web (ou une méthode de page) et remplit la valeur obtenue dans un contrôle cible dans la page, sans une actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="416b7-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="416b7-107">Il est également possible de déclencher le remplissage à l’aide du code JavaScript côté client personnalisé.</span><span class="sxs-lookup"><span data-stu-id="416b7-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="416b7-108">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="416b7-108">Overview</span></span>

<span data-ttu-id="416b7-109">Le `DynamicPopulate` contrôle dans la boîte à outils de contrôle ASP.NET AJAX appelle un service web (ou une méthode de page) et remplit la valeur obtenue dans un contrôle cible dans la page, sans une actualisation de la page.</span><span class="sxs-lookup"><span data-stu-id="416b7-109">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="416b7-110">Il est également possible de déclencher le remplissage à l’aide du code JavaScript côté client personnalisé.</span><span class="sxs-lookup"><span data-stu-id="416b7-110">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="416b7-111">Étapes</span><span class="sxs-lookup"><span data-stu-id="416b7-111">Steps</span></span>

<span data-ttu-id="416b7-112">Vous devez tout d’abord, Service Web ASP.NET qui implémente la méthode à être appelée par le `DynamicPopulateExtender` contrôle.</span><span class="sxs-lookup"><span data-stu-id="416b7-112">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="416b7-113">Le service web implémente la méthode `getDate()` qui attend un argument de type chaîne, appelée `contextKey`, étant donné que le `DynamicPopulate` contrôle envoie une partie des informations de contexte à chaque appel de service web.</span><span class="sxs-lookup"><span data-stu-id="416b7-113">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="416b7-114">Voici le code (fichier `DynamicPopulate.vb.asmx`) qui Récupère la date actuelle dans un des trois formats :</span><span class="sxs-lookup"><span data-stu-id="416b7-114">Here is the code (file `DynamicPopulate.vb.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample1.aspx)]

<span data-ttu-id="416b7-115">Dans l’étape suivante, créer un nouveau site ASP.NET et démarrer avec le contrôle ASP.NET AJAX ScriptManager :</span><span class="sxs-lookup"><span data-stu-id="416b7-115">In the next step, create a new ASP.NET site and start with the ASP.NET AJAX ScriptManager control:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample2.aspx)]

<span data-ttu-id="416b7-116">Ensuite, ajoutez un contrôle label (par exemple à l’aide du contrôle HTML portant le même nom, ou le `<asp:Label />` contrôle web) qui affichera ultérieurement le résultat de l’appel de service web.</span><span class="sxs-lookup"><span data-stu-id="416b7-116">Then, add a label control (for instance using the HTML control of the same name, or the `<asp:Label />` web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample3.aspx)]

<span data-ttu-id="416b7-117">Ensuite, inclure un `DynamicPopulateExtender` contrôler et de fournir des informations sur le service web, contrôle de la cible, mais pas le nom du contrôle qui déclenche la population de cette opération est effectuée ultérieurement à l’aide de code JavaScript personnalisé !</span><span class="sxs-lookup"><span data-stu-id="416b7-117">Next, include a `DynamicPopulateExtender` control and provide web service information, target control, but not the name of the control which triggers the population this will be done later on, using custom JavaScript!</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample4.aspx)]

<span data-ttu-id="416b7-118">Pour la partie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="416b7-118">Now to the JavaScript part.</span></span> <span data-ttu-id="416b7-119">Le `$find()` fonction, définie par la bibliothèque ASP.NET AJAX, retourne une référence aux objets côté serveur de la boîte à outils de contrôle ASP.NET AJAX comme `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="416b7-119">The `$find()` function, defined by the ASP.NET AJAX library, returns a reference to server-side objects of the ASP.NET AJAX Control Toolkit such as `DynamicPopulateExtender`.</span></span> <span data-ttu-id="416b7-120">Dans le fichier actuel, `$find("dpe")` retourne une référence à le `DynamicPopulateExtender` contrôle dans la page.</span><span class="sxs-lookup"><span data-stu-id="416b7-120">In the current file, `$find("dpe")` returns a reference to the one `DynamicPopulateExtender` control in the page.</span></span> <span data-ttu-id="416b7-121">Il expose une méthode appelée `populate()` qui déclenche le processus de remplissage dynamique.</span><span class="sxs-lookup"><span data-stu-id="416b7-121">It exposes a method called `populate()` which triggers the dynamic population process.</span></span> <span data-ttu-id="416b7-122">Le `populate()` méthode nécessite un seul argument : la clé de contexte qui servira à l’argument de la `getDate()` méthode web.</span><span class="sxs-lookup"><span data-stu-id="416b7-122">The `populate()` method requires one argument: the context key which will serve as argument to the `getDate()` web method.</span></span> <span data-ttu-id="416b7-123">Par exemple, `$find("dpe").populate("format1")` remplirait l’étiquette avec la date actuelle au format de mois-jour-année.</span><span class="sxs-lookup"><span data-stu-id="416b7-123">So for instance, `$find("dpe").populate("format1")` would populate the label with the current date in month-day-year format.</span></span>

<span data-ttu-id="416b7-124">Pour rendre l’exemple un peu plus souple, l’utilisateur peut désormais le choix entre plusieurs formats de date.</span><span class="sxs-lookup"><span data-stu-id="416b7-124">In order to make the sample a bit more flexible, the user may now choose between several date formats.</span></span> <span data-ttu-id="416b7-125">Pour chacune d'entre elles, une case s’affiche.</span><span class="sxs-lookup"><span data-stu-id="416b7-125">For each one of them, a radio button is displayed.</span></span> <span data-ttu-id="416b7-126">Une fois l’utilisateur clique sur un bouton radio, du code JavaScript renseigne dynamiquement l’étiquette avec le format de date sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="416b7-126">Once the user clicks on a radio button, JavaScript code dynamically populates the label with the selected date format.</span></span> <span data-ttu-id="416b7-127">Voici les cases d’option :</span><span class="sxs-lookup"><span data-stu-id="416b7-127">Here are those radio buttons:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-vb/samples/sample5.aspx)]

<span data-ttu-id="416b7-128">Notez que dans le contexte d’une case d’option, l’expression JavaScript `this.value` fait référence à la valeur du bouton actuel, qui se trouve être exactement les mêmes informations la `getDate()` méthode peut fonctionner avec.</span><span class="sxs-lookup"><span data-stu-id="416b7-128">Note that within the context of a radio button, the JavaScript expression `this.value` refers to the value of the current button, which happens to be exactly the same information the `getDate()` method can work with.</span></span>


<span data-ttu-id="416b7-129">[![Un clic sur le bouton récupère la date à partir du serveur, dans le format spécifié](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="416b7-129">[![A click on the button retrieves the date from the server, in the format specified](dynamically-populating-a-control-using-javascript-code-vb/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="416b7-130">Un clic sur le bouton récupère la date à partir du serveur, dans le format spécifié ([cliquez pour afficher l’image en taille réelle](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="416b7-130">A click on the button retrieves the date from the server, in the format specified ([Click to view full-size image](dynamically-populating-a-control-using-javascript-code-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="416b7-131">[Précédent](dynamically-populating-a-control-vb.md)
[Suivant](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span><span class="sxs-lookup"><span data-stu-id="416b7-131">[Previous](dynamically-populating-a-control-vb.md)
[Next](using-dynamicpopulate-with-a-user-control-and-javascript-vb.md)</span></span>
