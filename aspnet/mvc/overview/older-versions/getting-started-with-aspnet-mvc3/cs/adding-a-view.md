---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: Ajout d’une vue (c#) | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les bases de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est en cours...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 9fc8c6cad44016511c462b4206c28ea3449ff97e
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576610"
---
<a name="adding-a-view-c"></a>Ajout d’une vue (c#)
====================
par [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > Une version mise à jour de ce didacticiel est disponible [ici](../../../getting-started/introduction/getting-started.md) qui utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sécurisé, beaucoup plus simple à suivre et illustre plusieurs fonctionnalités.
> 
> 
> Ce didacticiel vous apprend les notions de base de la création d’une application Web ASP.NET MVC à l’aide de Microsoft Visual Web Developer 2010 Express Service Pack 1, qui est une version gratuite de Microsoft Visual Studio. Avant de commencer, assurez-vous que vous avez installé les composants requis listés ci-dessous. Vous pouvez installer tous les en cliquant sur le lien suivant : [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Vous pouvez également installer individuellement les conditions préalables à l’aide des liens suivants :
> 
> - [Prérequis pour le Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Mettre à jour des outils ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + outils prennent en charge)
> 
> Si vous utilisez Visual Studio 2010 au lieu de Visual Web Developer 2010, installez les conditions préalables en cliquant sur le lien suivant : [configuration requise de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un projet de Visual Web Developer avec code source c# est disponible pour accompagner cette rubrique. [Téléchargez la version c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si vous préférez Visual Basic, basculez vers le [version Visual Basic](../vb/intro-to-aspnet-mvc-3.md) de ce didacticiel.


Dans cette section vous allez modifier la `HelloWorldController` classe à utiliser la vue fichiers de modèle proprement encapsulent le processus de génération des réponses HTML à un client.

Vous allez créer un fichier de modèle de vue à l’aide de la nouvelle [moteur d’affichage Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introduite avec ASP.NET MVC 3. Modèles de vue Razor ont une *.cshtml* extension de fichier et offrent un moyen élégant pour créer le HTML de sortie à l’aide de c#. Razor réduit le nombre de caractères et des touches du clavier nécessaires lors de l’écriture d’un modèle de vue et permet un rapide, fluide de flux de travail de codage.

Démarrer à l’aide d’un modèle de vue avec le `Index` méthode dans la `HelloWorldController` classe. Actuellement, la méthode `Index` retourne une chaîne avec un message qui est codé en dur dans la classe du contrôleur. Modifier le `Index` méthode pour retourner un `View` de l’objet, comme indiqué dans l’exemple suivant :

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Ce code utilise un modèle de vue pour générer une réponse HTML au navigateur. Dans le projet, ajoutez un modèle de vue que vous pouvez utiliser avec le `Index` (méthode). Pour ce faire, le bouton droit dans le `Index` (méthode) et cliquez sur **ajouter une vue**.

![](adding-a-view/_static/image1.png)

Le **ajouter une vue** boîte de dialogue s’affiche. Laissez les valeurs par défaut de la façon dont ils sont et cliquez sur le **ajouter** bouton :

![](adding-a-view/_static/image2.png)

Le *MvcMovie\Views\HelloWorld* dossier et le *MvcMovie\Views\HelloWorld\Index.cshtml* fichier sont créés. Vous pouvez les voir dans **l’Explorateur de solutions**:

![](adding-a-view/_static/image3.png)

Le suivant le *Index.cshtml* fichier qui a été créé :

[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

Ajouter du code HTML sous la `<h2>` balise. Modifié *MvcMovie\Views\HelloWorld\Index.cshtml* fichier est présenté ci-dessous.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

Exécutez l’application et accédez à la `HelloWorld` contrôleur (`http://localhost:xxxx/HelloWorld`). Le `Index` méthode dans votre contrôleur n’avez pas beaucoup de travail ; il a simplement exécuté l’instruction `return View()`, lequel spécifié que la méthode doit utiliser un fichier de modèle de vue pour restituer une réponse au navigateur. Étant donné que vous n’avez pas explicitement spécifier le nom du fichier de modèle de vue à utiliser, ASP.NET MVC par défaut la valeur à l’aide de la *Index.cshtml* afficher le fichier dans le *\Views\HelloWorld* dossier. L’image ci-dessous montre la chaîne codées en dur dans la vue.

![](adding-a-view/_static/image6.png)

Il semble assez bonne. Toutefois, notez que la barre de titre indique « Index » et le titre big sur la page indique « Mon Application MVC ». Nous allons modifier ceux.

## <a name="changing-views-and-layout-pages"></a>Modification des vues et des Pages de disposition

Tout d’abord, vous souhaitez modifier le titre « My Application MVC » en haut de la page. Ce texte est courant de chaque page. Il est effectivement implémenté dans un seul endroit dans le projet, même si elles apparaissent sur chaque page dans l’application. Accédez à la */Views/Shared* dossier **l’Explorateur de solutions** et ouvrez le  *\_Layout.cshtml* fichier. Ce fichier s’appelle un *page de disposition* et il est le partagé « shell » qui utilisent toutes les autres pages.

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

Modèles de disposition permettent de spécifier la disposition du conteneur HTML de votre site au même endroit et de l’appliquer sur plusieurs pages de votre site. Remarque la `@RenderBody()` ligne au bas du fichier. `RenderBody` est un espace réservé dans lequel toutes les pages spécifiques à la vue que vous créez apparaissent, « encapsulées » dans la page de disposition. Modifier l’en-tête du titre dans le modèle de disposition à partir de « My MVC Application » à « MVC Movie App ».

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

Exécutez l’application et notez qu’il indique à présent « MVC Movie App ». Cliquez sur le **sur** lien et que vous consultez Comment cette page affiche « MVC Movie App », trop. Nous avons pu apporter la modification une fois dans le modèle de disposition et avoir toutes les pages sur le site de refléter le nouveau titre.

![](adding-a-view/_static/image9.png)

L’ensemble  *\_Layout.cshtml* fichier est présenté ci-dessous :

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Maintenant, nous allons modifier le titre de la page d’Index (vue).

Ouvrez *MvcMovie\Views\HelloWorld\Index.cshtml*. Il existe deux emplacements pour apporter une modification : tout d’abord, le texte qui apparaît dans le titre du navigateur, puis, dans l’en-tête secondaire (le `<h2>` élément). Vous allez les modifier légèrement pour voir quel morceau du code modifie quelle partie de l’application.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Pour indiquer le titre HTML à afficher, le code ci-dessus affecte un `Title` propriété de la `ViewBag` objet (qui se trouve dans le *Index.cshtml* modèle de vue). Si vous revenez au code source du modèle de disposition, vous remarquerez que le modèle utilise cette valeur dans le `<title>` élément dans le cadre de la `<head>` section du code HTML. À l’aide de cette approche, vous pouvez facilement passer d’autres paramètres entre votre modèle de vue et de votre fichier de disposition.

Exécutez l’application et accédez à `http://localhost:xx/HelloWorld`. Notez que le titre du navigateur, l’en-tête principal et les en-têtes secondaires ont changé. (Si vous ne voyez pas les changements dans le navigateur, vous voyez peut-être le contenu mis en cache. Appuyez sur Ctrl+F5 dans votre navigateur pour forcer le chargement de la réponse du serveur.)

Notez également comment le contenu dans le *Index.cshtml* afficher le modèle a été fusionné avec la  *\_Layout.cshtml* afficher le modèle et une seule réponse HTML a été envoyée au navigateur. Les modèles de disposition permettent d’apporter facilement des modifications qui s’appliquent à toutes les pages de votre application.

![](adding-a-view/_static/image10.png)

Nos quelques « données » (dans le cas présent, le message « Hello from our View Template! » ) sont toutefois codées en dur. L’application MVC a une vue (« V») et vous avez un contrôleur (« C »), mais pas encore de modèle (« M »). Peu de temps, nous allons étudier la création d’une base de données et récupérer des données de modèle à partir de celui-ci.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passage de données du contrôleur vers la vue

Avant nous accédez à une base de données et que vous parler de modèles, cependant, nous allons tout d’abord parler de passer des informations à partir du contrôleur à une vue. Classes de contrôleur sont appelées en réponse à une demande d’URL entrante. Une classe de contrôleur est où vous écrivez le code qui gère les paramètres entrants, récupère les données à partir d’une base de données et finalement détermine le type de réponse à envoyer au navigateur. Vue modèles peuvent ensuite être utilisés à partir d’un contrôleur pour générer et mettre en forme une réponse HTML au navigateur.

Contrôleurs sont chargés de fournir les données ou les objets sont requis pour un modèle de vue restituer une réponse au navigateur. Un modèle de vue ne doit jamais exécuter la logique métier ou interagir directement avec une base de données. Au lieu de cela, il doit fonctionner uniquement avec les données qui sont qui lui sont fournies par le contrôleur. Préserver cette « séparation des préoccupations » permet de maintenir votre code propre et plus facile à gérer.

Actuellement, le `Welcome` méthode d’action dans le `HelloWorldController` classe prend un `name` et un `numTimes` paramètre, puis sort les valeurs directement dans le navigateur. Au lieu que le contrôleur restitue cette réponse sous forme de chaîne, modifions-le pour utiliser un modèle de vue à la place. Le modèle de vue génère une réponse dynamique, ce qui signifie que vous devez passer les bits de données appropriés du contrôleur à la vue pour générer la réponse. Vous pouvez faire avec le contrôleur de placer les données dynamiques dont le modèle de vue a besoin dans un `ViewBag` objet auquel le modèle de vue peut ensuite accéder.

Retour à la *HelloWorldController.cs* du fichier et changer la `Welcome` méthode pour ajouter un `Message` et `NumTimes` valeur pour le `ViewBag` objet. `ViewBag` est un objet dynamique, ce qui signifie que vous pouvez placer tout ce que vous voulez le `ViewBag` objet ne possède aucune propriété définie jusqu'à ce que vous placez un élément qu’il contient. Le fichier *HelloWorldController.cs* complet ressemble à ceci :

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

Maintenant le `ViewBag` objet contient des données qui seront passées à la vue automatiquement.

Ensuite, vous avez besoin d’un modèle de vue Bienvenue ! Dans le **déboguer** menu, sélectionnez **MvcMovie Build** s’assurer que le projet est compilé.

[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

Puis avec le bouton droit à l’intérieur de la `Welcome` (méthode) et cliquez sur **ajouter une vue**. Voici à quoi le **ajouter une vue** boîte de dialogue ressemble à :

![](adding-a-view/_static/image13.png)

Cliquez sur **ajouter**, puis ajoutez le code suivant sous le `<h2>` élément dans le nouveau *Welcome.cshtml* fichier. Vous allez créer une boucle qui dit « Hello » comme autant de fois que l’utilisateur indique qu’il doit. L’ensemble *Welcome.cshtml* fichier est présenté ci-dessous.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

Exécutez l’application et accédez à l’URL suivante :

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Maintenant les données sont extraites de l’URL et passées automatiquement au contrôleur. Le contrôleur empaquette les données dans un `ViewBag` objet et passe cet objet à la vue. Puis, la vue affiche les données au format HTML à l’utilisateur.

![](adding-a-view/_static/image14.png)

Il s’agissait d’une sorte de « M » pour le modèle, mais pas d’une base de données en soi. Créons une base de données de films en utilisant ce que nous avons appris.

> [!div class="step-by-step"]
> [Précédent](adding-a-controller.md)
> [Suivant](adding-a-model.md)
