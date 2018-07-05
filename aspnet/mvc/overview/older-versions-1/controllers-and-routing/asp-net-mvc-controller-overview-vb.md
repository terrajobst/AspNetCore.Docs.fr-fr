---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
title: Vue d’ensemble du contrôleur ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: Dans ce didacticiel, Stephen Walther vous présente les contrôleurs ASP.NET MVC. Vous allez apprendre à créer de nouveaux contrôleurs et de retourner différents types de res d’action...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: 94c3e5d9-a904-445e-a34e-d92fd1ca108a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: d1fa2e71ae37d9123b489511c57ae0f79c0802ff
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371776"
---
<a name="aspnet-mvc-controller-overview-vb"></a>Vue d’ensemble du contrôleur ASP.NET MVC (VB)
====================
par [Stephen Walther](https://github.com/StephenWalther)

> Dans ce didacticiel, Stephen Walther vous présente les contrôleurs ASP.NET MVC. Vous allez apprendre à créer de nouveaux contrôleurs et de retourner différents types de résultats d’action.


Ce didacticiel explore le sujet de contrôleurs ASP.NET MVC, les actions de contrôleur et les résultats d’action. Après avoir terminé ce didacticiel, vous comprendrez comment les contrôleurs sont utilisés pour contrôler la façon dont un visiteur interagit avec un site Web ASP.NET MVC.

## <a name="understanding-controllers"></a>Présentation des contrôleurs

Contrôleurs MVC sont chargés de répondre aux requêtes effectuées par rapport à un site Web ASP.NET MVC. Chaque demande de navigateur est mappée à un contrôleur spécifique. Par exemple, imaginez que vous entrez l’URL suivante dans la barre d’adresses de votre navigateur :

`http://localhost/Product/Index/3`

Dans ce cas, un contrôleur nommé ProductController est appelé. Le ProductController est responsable de la génération de la réponse à la demande de navigateur. Par exemple, le contrôleur peut retourner une vue particulière dans le navigateur ou le contrôleur peut rediriger l’utilisateur vers un autre contrôleur.

Listing 1 contient un simple contrôleur nommé ProductController.

**Listing1 - Controllers\ProductController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample1.vb)]

Comme vous pouvez le voir à partir de la liste 1, un contrôleur est simplement une classe (une classe Visual Basic .NET ou c#). Un contrôleur est une classe qui dérive de la classe de base System.Web.Mvc.Controller. Un contrôleur hérite de cette classe de base, un contrôleur hérite de plusieurs méthodes utiles gratuitement (nous y reviendrons ces méthodes dans un instant).

## <a name="understanding-controller-actions"></a>Présentation des Actions de contrôleur

Un contrôleur expose les actions de contrôleur. Une action est une méthode sur un contrôleur qui est appelée lorsque vous entrez une URL particulière dans la barre d’adresse de votre navigateur. Par exemple, imaginez que vous effectuez une requête pour l’URL suivante :

`http://localhost/Product/Index/3`

Dans ce cas, la méthode Index() est appelée sur la classe ProductController. La méthode Index() est un exemple d’une action de contrôleur.

Une action de contrôleur doit être une méthode publique d’une classe de contrôleur. Méthodes de Visual Basic.NET, par défaut, sont des méthodes publiques. Notez que toute méthode publique que vous ajoutez à une classe de contrôleur est exposée comme une action de contrôleur automatiquement (vous devez veiller à ce sujet dans la mesure où une action de contrôleur peut être appelée par n’importe qui dans l’univers en tapant l’URL de droite dans une barre d’adresses du navigateur).

Il existe certaines exigences supplémentaires qui doivent être satisfaites par une action de contrôleur. Une méthode utilisée comme une action de contrôleur ne peut pas être surchargée. En outre, une action de contrôleur ne peut pas être une méthode statique. Autrement, vous pouvez utiliser pratiquement n’importe quelle méthode comme une action de contrôleur.

## <a name="understanding-action-results"></a>Présentation des résultats d’Action

Une action de contrôleur retourne ce que l'on appelle un *résultat d’action*. Un résultat d’action est ce que retourne une action de contrôleur en réponse à une demande de navigateur.

L’infrastructure ASP.NET MVC prend en charge plusieurs types de résultats d’action, notamment :

1. ViewResult - représente HTML et le balisage.
2. EmptyResult - ne représente aucun résultat.
3. RedirectResult - représente une redirection vers une nouvelle URL.
4. JsonResult - représente un résultat de JavaScript Object Notation qui peut être utilisé dans une application AJAX.
5. JavaScriptResult - représente un script JavaScript.
6. ContentResult - représente un résultat de texte.
7. FileContentResult - représente un fichier téléchargeable (avec le contenu binaire).
8. FilePathResult - représente un fichier téléchargeable (avec un chemin d’accès).
9. FileStreamResult - représente un fichier téléchargeable (avec un flux de fichier).

Tous ces résultats d’action héritent de la classe ActionResult base.

Dans la plupart des cas, une action de contrôleur retourne un ViewResult. Par exemple, l’action de contrôleur d’Index dans la liste 2 Retourne un ViewResult.

**Listing 2 - Controllers\BookController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample2.vb)]

Lorsqu’une action retourne un ViewResult, HTML est retourné au navigateur. La méthode Index() dans le Listing 2 Retourne une vue nommée des Index dans le navigateur.

Notez que l’action Index() dans le Listing 2 ne retourne pas un ViewResult(). Au lieu de cela, la méthode View() de la classe de base de contrôleur est appelée. Normalement, vous ne retournez un résultat d’action directement. Au lieu de cela, vous appelez une des méthodes suivantes de la classe de base du contrôleur :

1. Vue - retourne un résultat d’action ViewResult.
2. Rediriger - retourne un résultat d’action RedirectResult.
3. RedirectToAction - retourne un résultat d’action RedirectToRouteResult.
4. RedirectToRoute - retourne un résultat d’action RedirectToRouteResult.
5. JSON - retourne un résultat d’action JsonResult.
6. JavaScriptResult - retourne un JavaScriptResult.
7. Contenu : retourne un résultat d’action ContentResult.
8. Fichier - retourne un FileContentResult, FilePathResult ou FileStreamResult selon les paramètres transmis à la méthode.

Par conséquent, si vous souhaitez renvoyer un affichage dans le navigateur, vous appelez la méthode View(). Si vous souhaitez rediriger l’utilisateur à partir de l’action d’un contrôleur à l’autre, vous appelez la méthode RedirectToAction(). Par exemple, l’action Details() dans le Listing 3 affiche une vue ou redirige l’utilisateur vers l’action Index() selon que le paramètre Id a une valeur.

**Liste 3 - CustomerController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample3.vb)]

Le résultat d’action ContentResult est spécial. Vous pouvez utiliser le résultat d’action ContentResult pour retourner un résultat d’action en tant que texte brut. Par exemple, la méthode Index() sur la liste 4 retourne un message en tant que texte brut et pas au format HTML.

**Liste 4 - Controllers\StatusController.vb**

> StatusController
> 
> 
> System.Web.Mvc.Controller


[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample4.vb)]

Lorsque l’action StatusController.Index() est appelée, une vue n’est pas renvoyée. Au lieu de cela, le texte brut « Hello World ! » est retourné au navigateur.

Si une action de contrôleur retourne un résultat qui n’est pas un résultat d’action - par exemple, une date ou un entier - le résultat est encapsulé dans un ContentResult automatiquement. Par exemple, lorsque l’action Index() de la WorkController dans la liste 5 est appelée, la date est retournée automatiquement comme un ContentResult.

**Liste 5 - WorkController.vb**

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample5.vb)]

L’action Index() dans la liste 5 retourne un objet DateTime. L’infrastructure ASP.NET MVC convertit l’objet DateTime en une chaîne et encapsule la valeur de date/heure dans un ContentResult automatiquement. Le navigateur reçoit la date et l’heure en tant que texte brut.

## <a name="summary"></a>Récapitulatif

L’objectif de ce didacticiel a été pour vous présenter les concepts de contrôleurs ASP.NET MVC, les actions de contrôleur et les résultats d’action de contrôleur. Dans la première section, vous avez appris à ajouter de nouveaux contrôleurs pour un projet ASP.NET MVC. Ensuite, vous avez appris les méthodes publics d’un contrôleur sont exposés à l’univers en tant qu’actions de contrôleur. Enfin, nous avons abordé les différents types de résultats d’action qui peuvent être retournés à partir d’une action de contrôleur. En particulier, nous avons vu comment renvoyer un ViewResult, RedirectToActionResult et ContentResult à partir d’une action de contrôleur.

> [!div class="step-by-step"]
> [Précédent](creating-a-custom-route-constraint-cs.md)
> [Suivant](creating-custom-routes-vb.md)
