---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: Ajout d’une vue (VB) | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est en cours...
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 140dc8a7302c52112c2d8873325b2567e3ba66a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829978"
---
<a name="adding-a-view-vb"></a>Ajout d’une vue (VB)
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
> Un projet de Visual Web Developer avec le code source VB.NET est disponible pour accompagner cette rubrique. [Téléchargez la version VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si vous préférez c#, basculez vers le [c# version](../cs/adding-a-view.md) de ce didacticiel.


Dans cette section, nous devons modifier la `HelloWorldController` classe à utiliser un fichier de modèle de vue à proprement encapsule le processus de génération des réponses HTML à un client.

Nous allons commencer en utilisant un modèle de vue avec le `Index` méthode dans la `HelloWorldController` classe. Actuellement le `Index` méthode retourne une chaîne avec un message qui est codé en dur dans la classe de contrôleur. Modifier le `Index` méthode pour retourner un `View` de l’objet, comme indiqué dans l’exemple suivant :

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

Nous allons maintenant ajouter un modèle de vue à notre projet que nous pouvons appeler avec la `Index` (méthode). Pour ce faire, le bouton droit dans le `Index` (méthode) et cliquez sur **ajouter une vue**.

[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)

Le **ajouter une vue** boîte de dialogue s’affiche. Laissez les entrées par défaut et cliquez sur le **ajouter** bouton.

[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)

Le *MvcMovie\Views\HelloWorld* dossier et le *MvcMovie\Views\HelloWorld\Index.vbhtml* fichier sont créés. Vous pouvez les voir dans **l’Explorateur de solutions**:

[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)

Ajouter du code HTML sous la `<h2>` balise. Modifié *MvcMovie\Views\HelloWorld\Index.vbhtml* fichier est présenté ci-dessous.

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

Exécutez l’application et accédez à la &quot;Bonjour&quot; contrôleur (`http://localhost:xxxx/HelloWorld`). Le `Index` méthode dans votre contrôleur n’avez pas beaucoup de travail ; il a simplement exécuté l’instruction `return View()`, qui indiquait que nous souhaitons utiliser un fichier de modèle de vue pour restituer une réponse au client. Car nous ne spécifie pas explicitement le nom du fichier de modèle de vue à utiliser, ASP.NET MVC par défaut la valeur à l’aide de la *Index.vbhtml* afficher le fichier dans le *\Views\HelloWorld* dossier. L’image ci-dessous montre la chaîne codées en dur dans la vue.

[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)

Il semble assez bonne. Toutefois, notez que la barre de titre indique &quot;Index&quot; et indique que le titre big sur la page &quot;mon Application MVC.&quot; Nous allons modifier ceux.

## <a name="changing-views-and-layout-pages"></a>Changement de vues et de pages de disposition

Tout d’abord, nous allons modifier le texte &quot;mon Application MVC.&quot; Ce texte est partagé et apparaît sur chaque page. Il apparaît réellement dans le seul endroit dans notre projet, même s’il est sur chaque page dans notre application. Accédez à la */Views/Shared* dossier **l’Explorateur de solutions** et ouvrez le  *\_Layout.vbhtml* fichier. Ce fichier s’appelle une page de disposition et il est partagé &quot;shell&quot; qui utilisent toutes les autres pages.

Remarque la `@RenderBody()` ligne de code vers le bas du fichier. `RenderBody` est un espace réservé dans lequel toutes les pages que vous créez apparaissent, &quot;encapsulé&quot; dans la page de disposition. Modifier le `<h1>` en-tête à partir de **&quot;** mon Application MVC&quot; à &quot;application MVC Movie&quot;.

[!code-html[Main](adding-a-view/samples/sample3.html)]

Exécutez l’application et notez qu’il indique à présent &quot;application MVC Movie&quot;. Cliquez sur le **sur** lien et qui affiche la page &quot;application MVC Movie&quot;, trop.

L’ensemble  *\_Layout.vbhtml* fichier est présenté ci-dessous :

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Maintenant, nous allons modifier le titre de la page d’Index (vue).

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

Ouvrez *MvcMovie\Views\HelloWorld\Index.vbhtml*. Il existe deux emplacements pour apporter une modification : tout d’abord, le texte qui apparaît dans le titre du navigateur, puis, dans l’en-tête secondaire (le `<h2>` élément). Nous allons rendre légèrement différent afin que vous puissiez voir quel morceau de code modifie quelle partie de l’application.

Exécutez l’application et accédez à`http://localhost:xx/HelloWorld`. Notez que le titre du navigateur, l’en-tête principal et les en-têtes secondaires ont changé. Il est facile d’effectuer des modifications importantes dans votre application avec petites modifications à une vue. (Si vous ne voyez pas les changements dans le navigateur, vous voyez peut-être le contenu mis en cache. Appuyez sur Ctrl+F5 dans votre navigateur pour forcer le chargement de la réponse du serveur.)

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

Notre petit peu de &quot;données&quot; (dans ce cas le &quot;Hello World !&quot; message) est codé en dur, cependant. Notre application MVC a V (vues) et nous avons C (contrôleurs), mais encore aucune M (modèle). Peu de temps, nous allons étudier la création d’une base de données et récupérer des données de modèle à partir de celui-ci.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passage de données du contrôleur vers la vue

Avant nous accédez à une base de données et que vous parler de modèles, cependant, nous allons tout d’abord parler de passer des informations à partir du contrôleur à une vue. Nous souhaitons passer ce qui nécessite un modèle de vue pour restituer une réponse HTML à un client. Ces objets sont généralement créés et passées par une classe de contrôleur à un modèle de vue, et ils doivent contenir uniquement les données requises par le modèle de vue et pas plus.

Précédemment avec le `HelloWorldController` (classe), le `Welcome` méthode d’action a eu un `name` et un `numTimes` paramètre et sortie puis les valeurs de paramètre dans le navigateur. Plutôt que le contrôleur continuent à être restitués directement de cette réponse, nous allons plutôt nous mettrons ces données dans un conteneur pour la vue. Contrôleurs et vues peuvent utiliser un `ViewBag` objet pour contenir ces données. Qui est transmis à un modèle de vue automatiquement et utilisé pour restituer la réponse HTML en utilisant le contenu du conteneur en tant que données. De cette façon, le contrôleur s’attache à une chose et le modèle de vue avec une autre, nous permet de maintenir propre &quot;séparation des préoccupations&quot; au sein de l’application.

Ou bien, nous pourrions définir une classe personnalisée, puis créer une instance de cet objet sur notre propre, remplir avec des données et le passer à la vue. Qui est souvent appelé un ViewModel, car c’est un modèle personnalisé pour la vue. Pour les petites quantités de données, cependant, ViewBag fonctionne très bien.

Retour à la *HelloWorldController.vb* modification du fichier le `Welcome` méthode à l’intérieur du contrôleur pour placer le Message et NumTimes dans ViewBag. ViewBag est un objet dynamique. Cela signifie que vous pouvez y placer tout ce qui vous voulez lui. ViewBag n’a pas de propriétés définis jusqu'à ce que vous placez un élément qu’il contient.

L’ensemble `HelloWorldController.vb` avec la nouvelle classe dans le même fichier.

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

Notre ViewBag contient maintenant des données qui seront passées au fil de la vue automatiquement. Là encore, vous pouvez également nous pourrions écoulées dans notre propre objet comme suit si nous plaisait :

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Maintenant nous avons besoin d’un `WelcomeView` modèle ! Exécutez l’application pour le nouveau code est compilé. Fermez le navigateur, avec le bouton droit à l’intérieur de la `Welcome` (méthode), puis cliquez sur **ajouter une vue**.

Voici à quoi votre **ajouter une vue** ressemble à la boîte de dialogue.

[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)

Ajoutez le code suivant sous le `<h2>` élément dans le nouveau <em>Bienvenue.</em> fichier de vbhtml. Nous allons effectuer une boucle et dire &quot;Hello&quot; aussi souvent que l’utilisateur dit que nous devrions !

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

Exécutez l’application et accédez à `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Maintenant les données sont extraites de l’URL et passées automatiquement au contrôleur. Le contrôleur empaquette les données en un `Model` objet et passe cet objet à la vue. La vue qu’affiche les données au format HTML à l’utilisateur.

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

Hé bien, ce qui était un type d’un &quot;M&quot; pour le modèle, mais pas le type de base de données. Créons une base de données de films en utilisant ce que nous avons appris.

> [!div class="step-by-step"]
> [Précédent](adding-a-controller.md)
> [Suivant](adding-a-model.md)
