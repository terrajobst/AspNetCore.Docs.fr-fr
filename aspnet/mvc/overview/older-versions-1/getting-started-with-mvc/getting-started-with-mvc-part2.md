---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: Ajout d’un contrôleur | Microsoft Docs
author: shanselman
description: Une version mise à jour si ce didacticiel est disponible ici à l’aide de Visual Studio 2013. Le nouveau didacticiel utilise ASP.NET MVC 5, qui fournit de nombreuses améliorations de t...
ms.author: riande
ms.date: 08/14/2010
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: 856b7d98ac8bd30982d81b0609bb9c1288e07e49
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830148"
---
<a name="adding-a-controller"></a>Ajour d’un contrôleur
====================
par [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Une version mise à jour si ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) à l’aide de [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Le nouveau didacticiel utilise ASP.NET MVC 5, qui fournit de nombreuses améliorations de ce didacticiel.
> 
> 
> Il s’agit d’un didacticiel de débutant qui présente les principes de base d’ASP.NET MVC. Vous allez créer une application web simple qui lit et écrit à partir d’une base de données. Visitez le [centre d’apprentissage ASP.NET MVC](../../../index.md) pour rechercher d’autres ASP.NET MVC didacticiels et exemples.


Représente le MVC de modèle, vue, le contrôleur. MVC est un modèle pour le développement d’applications de telle sorte que chaque partie a une responsabilité est différente d’un autre.

- Modèle : Les données de votre application
- Vues : Les fichiers de modèle votre application utilisera pour générer dynamiquement des réponses HTML.
- Contrôleurs : Classes qui gèrent les demandes d’URL entrantes à l’application, récupérer des données de modèle, puis spécifiez les modèles de vue qui restitue une réponse au client

Nous allons être couvrant tous ces concepts dans ce didacticiel et vous montrer comment les utiliser pour créer une application.

Nous allons créer un nouveau contrôleur en double-cliquant sur le dossier contrôleurs dans l’Explorateur de solutions et en sélectionnant Ajouter un contrôleur.

[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

Nommez votre contrôleur « HelloWorldController » et cliquez sur Ajouter.

[![Ajouter la boîte de dialogue contrôleur](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

Notez que, dans l’Explorateur de solutions sur la droite qui a un nouveau fichier a été créé pour vous avez appelé HelloWorldController.cs et ce fichier est maintenant ouvert dans le **IDE**.

[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

Créez deux nouvelles méthodes qui ressemblent à ceci à l’intérieur de votre nouvelle classe publique HelloWorldController. Nous allons retourner une chaîne de code HTML directement à partir de notre contrôleur, par exemple.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

Votre contrôleur est nommé HelloWorldController et votre nouvelle méthode est appelée l’Index. Réexécuter votre application, comme auparavant (cliquez sur le bouton de lecture ou appuyez sur F5 pour cela). Une fois que votre navigateur ait démarré, modifiez le chemin d’accès dans la barre d’adresse pour `http://localhost:xx/HelloWorld` où xx est le numéro de votre ordinateur a choisi. Votre navigateur doit maintenant ressembler à la capture d’écran ci-dessous. Dans notre méthode ci-dessus nous retourné une chaîne passée à une méthode appelée « Contenu ». Nous avons dit que le système retourne simplement du code HTML, et c’était le cas !

ASP.NET MVC appelle les différentes classes de contrôleur (et les différentes méthodes d’Action au sein de celles-ci) en fonction de l’URL entrante. La logique de mappage par défaut utilisée par ASP.NET MVC utilise un format comme celui-ci pour contrôler ce que le code est exécuté :

/ [Controller] / [ActionName] / [paramètres]

La première partie de l’URL détermine la classe de contrôleur à exécuter. Par conséquent, /HelloWorld mappe à la classe HelloWorldController. La deuxième partie de l’URL détermine la méthode d’Action sur la classe à exécuter. Par conséquent, /HelloWorld/Index provoquerait la méthode Index() de la classe HelloWorldcontroller à exécuter. Notez que nous n’avions à visiter /HelloWorld ci-dessus et la méthode Qu'index a été impliquée. Il s’agit d’une méthode nommée « Index » étant la méthode par défaut qui sera appelée sur un contrôleur s’il n’est pas explicitement spécifié.

[![Mon action par défaut](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

À présent, voyons `http://localhost:xx/HelloWorld/Welcome.` maintenant notre méthode Bienvenue a exécuté et a retourné de sa chaîne au format HTML.

Là encore, / [Controller] / [ActionName] / [paramètres] contrôleur est donc HelloWorld et bienvenue est la méthode dans ce cas. Nous n’avons pas encore fini les paramètres.

[![C’est la méthode d’action de bienvenue](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

Modifions notre exemple légèrement afin que nous pouvons transmettre certaines informations à partir de l’URL vers notre contrôleur, par exemple comme suit : / HelloWorld/Welcome ? nom = Scott&amp;numtimes = 4. Modifier votre méthode de bienvenue pour inclure les deux paramètres et mise à jour, tel que ci-dessous. Notez que nous avons utilisé la fonctionnalité de paramètre facultatif de c# pour indiquer que le paramètre numTimes doivent par défaut 1 si elle n’est transmise.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

Exécutez votre application et visitez `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` modifiant la valeur de nom et numtimes que vous le souhaitez. Le système mappées automatiquement les paramètres nommés à partir de votre chaîne de requête dans la barre d’adresses aux paramètres de votre méthode.

Dans ces deux exemples, le contrôleur a été fait tout le travail et a été retournent directement du HTML. En règle générale, nous ne voulons nos contrôleurs retournent directement - du HTML dans la mesure où qui finit par être très difficile de revenir en code. Au lieu de cela, nous allons utiliser généralement un fichier de modèle de vue séparé afin de générer la réponse HTML. Examinons à présent comment nous pouvons le faire. Fermez votre navigateur et revenir à l’IDE.

> [!div class="step-by-step"]
> [Précédent](getting-started-with-mvc-part1.md)
> [Suivant](getting-started-with-mvc-part3.md)
