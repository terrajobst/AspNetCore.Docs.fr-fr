---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: Ajout d’un contrôleur (VB) | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est en cours...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 75451be20bd39f37bb692a06389ed7f04bc75a2a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367850"
---
<a name="adding-a-controller-vb"></a>Ajout d’un contrôleur (VB)
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

> Ce didacticiel vous apprend les notions de base de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio. Avant de commencer, assurez-vous que vous avez installé les composants requis listés ci-dessous. Vous pouvez installer tous les en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les conditions préalables à l’aide des liens suivants :
> 
> - [Prérequis pour le Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Mettre à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + outils prennent en charge)
> 
> Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les conditions préalables en cliquant sur le lien suivant : [configuration requise de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un projet de Visual Web Developer avec le code source VB.NET est disponible pour accompagner cette rubrique. [Téléchargez la version VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si vous préférez c#, basculez vers le [c# version](../cs/adding-a-controller.md) de ce didacticiel.


MVC est l’acronyme *model-view-controller*. MVC est un modèle pour le développement d’applications de telle sorte que chaque partie a une responsabilité distincte :

- Modèle : Les données de votre application.
- Vues : Les fichiers de modèle votre application utilisera pour générer dynamiquement des réponses HTML.
- Contrôleurs : Classes qui gèrent les demandes d’URL entrantes à l’application, récupérer des données de modèle, puis spécifiez les modèles de vue qui restitue une réponse au client.

Nous allons être couvrant tous ces concepts dans ce didacticiel et vous montrer comment les utiliser pour créer une application.

Créer un nouveau contrôleur en double-cliquant sur le *contrôleurs* dossier **l’Explorateur de solutions** , puis en sélectionnant **ajouter un contrôleur**.

[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)

Nommez votre contrôleur &quot;HelloWorldController&quot; et cliquez sur **ajouter**.

[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)

Notez que dans **l’Explorateur de solutions** sur la droite qu’un nouveau fichier a été créé pour vous nommé *HelloWorldController.cs* et que le fichier est ouvert dans l’IDE.

À l’intérieur de la nouvelle `public class HelloWorldController` bloc, créez deux nouvelles méthodes qui ressemblent le code suivant. Nous allons retourner une chaîne de code HTML directement à partir du contrôleur, par exemple.

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

Votre contrôleur est nommé `HelloWorldController` et votre nouvelle méthode est nommé `Index`. Exécutez l’application (appuyez sur F5 ou Ctrl + F5). Une fois que votre navigateur ait démarré, ajouter &quot;HelloWorld&quot; pour le chemin d’accès dans la barre d’adresses. (Sur mon ordinateur, il a `http://localhost:43246/HelloWorld`) votre navigateur doit ressembler à la capture d’écran ci-dessous. Dans la méthode ci-dessus, le code a renvoyé une chaîne directement. Nous dit le système pour retourner uniquement des codes HTML, et elle le faisait !

![](adding-a-controller/_static/image5.png)

ASP.NET MVC appelle les classes de l’autre contrôleur (et différentes méthodes d’action au sein de celles-ci) en fonction de l’URL entrante. La logique de mappage par défaut utilisée par ASP.NET MVC utilise un format comme celui-ci pour contrôler quel code est appelé :

`/[Controller]/[ActionName]/[Parameters]`

La première partie de l’URL détermine la classe de contrôleur à exécuter. Par conséquent, */HelloWorld* mappe à la `HelloWorldController` classe. La deuxième partie de l’URL détermine la méthode d’action sur la classe à exécuter. Par conséquent, */HelloWorld/Index* provoquerait la `Index` méthode de la `HelloWorldController` classe à exécuter. Notez que nous n’avions à visiter */HelloWorld* ci-dessus et le `Index` méthode a été utilisée par défaut. Il s’agit, car une méthode nommée `Index` est la méthode par défaut qui sera appelée sur un contrôleur s’il n’est pas explicitement spécifié.

Accédez à `http://localhost:xxxx/HelloWorld/Welcome`. Le `Welcome` méthode s’exécute et retourne la chaîne &quot;c’est la méthode d’action Bienvenue... &quot;. Le mappage de MVC par défaut est `/[Controller]/[ActionName]/[Parameters]`. Pour cette URL, le contrôleur est `HelloWorld` et `Welcome` est la méthode. Nous n’avons pas utilisé le `[Parameters]` dans le cadre de l’URL.

![](adding-a-controller/_static/image6.png)

Nous allons modifier légèrement l’exemple afin que nous pouvons passer des informations de paramètre dans à partir de l’URL au contrôleur (par exemple, */HelloWorld/Welcome ? nom = Scott&amp;numtimes = 4*). Modifier votre `Welcome` méthode pour inclure les deux paramètres comme indiqué ci-dessous. Notez que nous avons utilisé la fonctionnalité de paramètre facultatif VB pour indiquer que le `numTimes` paramètre par défaut 1 si aucune valeur n’est passée pour ce paramètre.

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

Exécutez votre application et accédez à `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **.** Vous pouvez essayer différentes valeurs pour `name` et `numtimes`. Le système mappe automatiquement les paramètres nommés à partir de votre chaîne de requête dans la barre d’adresses aux paramètres de votre méthode.

![](adding-a-controller/_static/image7.png)

Dans les deux exemples de ces le contrôleur a fait la partie VC de MVC, qui est le travail de la vue et contrôleur. Le contrôleur retourne directement du HTML. En règle générale, nous ne voulons contrôleurs retournent directement, du HTML dans la mesure où cela devient très difficile de revenir en code. Au lieu de cela, nous allons utiliser généralement un fichier de modèle de vue séparé afin de générer la réponse HTML. Examinons à présent comment nous pouvons le faire.

> [!div class="step-by-step"]
> [Précédent](intro-to-aspnet-mvc-3.md)
> [Suivant](adding-a-view.md)
