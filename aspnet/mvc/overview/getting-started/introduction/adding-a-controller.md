---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Ajout d’un contrôleur | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 77389bfa4774857eb2a607b0a70e982826312e03
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802290"
---
<a name="adding-a-controller"></a>Ajour d’un contrôleur
====================
par [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

MVC est l’acronyme *model-view-controller*. MVC est un modèle de développement d’applications sont bien conçue, testable et facile à gérer. Applications basées sur MVC contiennent :

- **M** odèles : les Classes qui représentent les données de l’application et qui utilisent une logique de validation pour appliquer des règles d’entreprise pour ces données.
- **V** ues : les fichiers de modèle que votre application utilise pour générer dynamiquement des réponses HTML.
- **C** ontrôleurs : Classes qui gèrent les demandes du navigateur entrantes, récupérer des données de modèle, puis spécifiez les modèles de vue qui retournent une réponse au navigateur.

Nous allons être couvrant tous ces concepts dans cette série de didacticiels et vous montrer comment les utiliser pour créer une application.

> [!NOTE]
> À l’étape précédente du MVC par défaut le modèle a été sélectionné. Cela crée la maison, compte et gérer les contrôleurs par défaut.

Nous allons commencer en créant une classe de contrôleur. Dans **l’Explorateur de solutions**, avec le bouton droit le *contrôleurs* dossier, puis cliquez sur **ajouter**, puis **contrôleur**.


![](adding-a-controller/_static/image1.png)

Dans le **ajouter une structure** boîte de dialogue, cliquez sur **contrôleur MVC 5 - vide**, puis cliquez sur **ajouter**.

![](adding-a-controller/_static/image2.png)  
 

Nommez votre contrôleur « HelloWorldController » et cliquez sur **ajouter**.

![Ajouter un contrôleur](adding-a-controller/_static/image3.png)

Notez que dans **l’Explorateur de solutions** un nouveau fichier a été créé nommé *HelloWorldController.cs* et un nouveau dossier *Views\HelloWorld*. Le contrôleur est ouvert dans l’IDE.

![](adding-a-controller/_static/image4.png)

Remplacez le contenu du fichier par le code suivant.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Les méthodes de contrôleur retourne une chaîne de code HTML par exemple. Le contrôleur est nommé `HelloWorldController` et la première méthode se nomme `Index`. Nous allons l’appeler à partir d’un navigateur. Exécutez l’application (appuyez sur F5 ou Ctrl + F5). Dans le navigateur, ajoutez &quot;HelloWorld&quot; pour le chemin d’accès dans la barre d’adresses. (Par exemple, dans l’illustration ci-dessous, ses `http://localhost:1234/HelloWorld.`) la page dans le navigateur ressemblera à la capture d’écran suivante. Dans la méthode ci-dessus, le code a renvoyé une chaîne directement. Vous avez demandé le système pour retourner uniquement des codes HTML, et c’était le cas !

![](adding-a-controller/_static/image5.png)

ASP.NET MVC appelle les classes de l’autre contrôleur (et différentes méthodes d’action au sein de celles-ci) en fonction de l’URL entrante. La logique de routage URL par défaut utilisée par ASP.NET MVC utilise un format comme celui-ci pour déterminer quel code pour appeler :

`/[Controller]/[ActionName]/[Parameters]`

Vous définissez le format pour le routage dans le *application\_Start/RouteConfig.cs* fichier.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

Lorsque vous exécutez l’application et que vous ne fournissez aucun segment d’URL, les valeurs par défaut pour le contrôleur « Home » et la méthode d’action « Index » spécifiée dans la section valeurs par défaut du code ci-dessus.

La première partie de l’URL détermine la classe de contrôleur à exécuter. Par conséquent, */HelloWorld* mappe à la `HelloWorldController` classe. La deuxième partie de l’URL détermine la méthode d’action sur la classe à exécuter. Par conséquent, */HelloWorld/Index* provoquerait la `Index` méthode de la `HelloWorldController` classe à exécuter. Notez que nous n’avions pour accéder à */HelloWorld* et `Index` méthode a été utilisée par défaut. Il s’agit, car une méthode nommée `Index` est la méthode par défaut qui sera appelée sur un contrôleur s’il n’est pas explicitement spécifié. La troisième partie du segment d’URL (`Parameters`) concerne les données de routage. Nous allons voir les données d’itinéraire par la suite dans ce didacticiel.

Accédez à `http://localhost:xxxx/HelloWorld/Welcome`. Le `Welcome` méthode s’exécute et retourne la chaîne &quot;c’est la méthode d’action Bienvenue... &quot;. Le mappage de MVC par défaut est `/[Controller]/[ActionName]/[Parameters]`. Pour cette URL, le contrôleur est `HelloWorld`, et `Welcome` est la méthode d’action. Vous n’avez pas encore utilisé la partie `[Parameters]` de l’URL.

![](adding-a-controller/_static/image6.png)

Nous allons modifier légèrement l’exemple afin que vous pouvez passer des informations de paramètre à partir de l’URL au contrôleur (par exemple, */HelloWorld/Welcome ? nom = Scott&amp;numtimes = 4*). Modifier votre `Welcome` méthode pour inclure les deux paramètres comme indiqué ci-dessous. Notez que le code utilise la fonctionnalité de paramètre facultatif de c# pour indiquer que le `numTimes` paramètre par défaut 1 si aucune valeur n’est passée pour ce paramètre.

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> Note de sécurité : Le code ci-dessus utilise [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) pour protéger l’application à partir des entrées malveillantes (à savoir JavaScript). Pour plus d’informations, consultez [Comment : protéger contre les attaques de Script dans une Application Web en utilisant l’encodage HTML en chaînes](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).


 Exécutez votre application et accédez à l’exemple d’URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`). Vous pouvez essayer différentes valeurs pour `name` et `numtimes` dans l’URL. Le [système de liaison de modèle ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) mappe automatiquement les paramètres nommés à partir de la chaîne de requête dans la barre d’adresses aux paramètres de votre méthode.

![](adding-a-controller/_static/image7.png)

Dans l’exemple ci-dessus, le segment d’URL ( `Parameters`) n’est pas utilisé, le `name` et `numTimes` paramètres sont passés en tant que [chaînes de requête](http://en.wikipedia.org/wiki/Query_string). Le caractère générique ? (point d’interrogation) dans l’URL ci-dessus est un séparateur, suivent les chaînes de requête. Le caractère &amp; sépare les chaînes de requête.

Remplacez la méthode de bienvenue avec le code suivant :

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

Exécutez l’application et entrez l’URL suivante : `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

Cette fois le troisième segment d’URL mise en correspondance le paramètre d’itinéraire `ID.` le `Welcome` méthode d’action contient un paramètre (`ID`) correspondant à la spécification d’URL dans le `RegisterRoutes` (méthode).

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

Dans les applications ASP.NET MVC, il est plus courant de passer des paramètres en tant que données d’itinéraire (comme nous l’avons fait avec l’ID ci-dessus) que de les transmettre sous forme de chaînes de requête. Vous pouvez également ajouter un itinéraire pour passer à la fois le `name` et `numtimes` dans les paramètres en tant que données d’itinéraire dans l’URL. Dans le *application\_start\routeconfig* , ajoutez l’itinéraire « Hello » :

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

Exécutez l’application et accédez à `/localhost:XXX/HelloWorld/Welcome/Scott/3`.

![](adding-a-controller/_static/image9.png)

Pour de nombreuses applications MVC, l’itinéraire par défaut fonctionne bien. Vous apprendrez plus loin dans ce didacticiel pour passer des données à l’aide du binder de modèle, et vous n’aurez pas à modifier l’itinéraire par défaut pour ce faire.

Dans ces exemples le contrôleur a fait le &quot;VC&quot; partie de MVC, autrement dit, le travail de la vue et contrôleur. Le contrôleur retourne directement du HTML. En règle générale, vous ne voulez contrôleurs retournent directement, du HTML dans la mesure où cela devient très difficile de revenir en code. Au lieu de cela, nous allons utiliser généralement un fichier de modèle de vue séparé afin de générer la réponse HTML. Penchons-nous maintenant à la façon dont nous pouvons le faire.

> [!div class="step-by-step"]
> [Précédent](getting-started.md)
> [Suivant](adding-a-view.md)
