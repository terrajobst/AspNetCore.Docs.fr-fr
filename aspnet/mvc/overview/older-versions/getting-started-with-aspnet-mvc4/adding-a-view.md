---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: Ajout d’une vue | Microsoft Docs
author: Rick-Anderson
description: 'Remarque : Une version mise à jour de ce didacticiel est disponible ici qui utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sécurisé, beaucoup plus simple à suivre et de démonstration...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 7b55a55db6207b8ff18b2dd207e919cee45f6973
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577923"
---
<a name="adding-a-view"></a>Ajout d’une vue
====================
par [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > Une version mise à jour de ce didacticiel est disponible [ici](../../getting-started/introduction/getting-started.md) qui utilise ASP.NET MVC 5 et Visual Studio 2013. Il est plus sécurisé, beaucoup plus simple à suivre et illustre plusieurs fonctionnalités.


Dans cette section vous allez modifier la `HelloWorldController` classe à utiliser la vue fichiers de modèle proprement encapsulent le processus de génération des réponses HTML à un client.

Vous allez créer un fichier de modèle de vue à l’aide du [moteur d’affichage Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introduite avec ASP.NET MVC 3. Modèles de vue Razor ont une *.cshtml* extension de fichier et offrent un moyen élégant pour créer le HTML de sortie à l’aide de c#. Razor réduit le nombre de caractères et des touches du clavier nécessaires lors de l’écriture d’un modèle de vue et permet un rapide, fluide de flux de travail de codage.

Actuellement, la méthode `Index` retourne une chaîne avec un message qui est codé en dur dans la classe du contrôleur. Modifier le `Index` méthode pour retourner un `View` de l’objet, comme indiqué dans le code suivant :

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Le `Index` méthode ci-dessus utilise un modèle de vue pour générer une réponse HTML au navigateur. Méthodes de contrôleur (également appelé [méthodes d’action](http://rachelappel.com/asp.net-mvc-actionresults-explained)), comme le `Index` méthode ci-dessus, retournent généralement une [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (ou une classe dérivée de [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), types non primitifs comme chaîne.

Dans le projet, ajoutez un modèle de vue que vous pouvez utiliser avec le `Index` (méthode). Pour ce faire, le bouton droit dans le `Index` (méthode) et cliquez sur **ajouter une vue**.

![](adding-a-view/_static/image1.png)

Le **ajouter une vue** boîte de dialogue s’affiche. Laissez les valeurs par défaut de la façon dont ils sont et cliquez sur le **ajouter** bouton :

![](adding-a-view/_static/image2.png)

Le *MvcMovie\Views\HelloWorld* dossier et le *MvcMovie\Views\HelloWorld\Index.cshtml* fichier sont créés. Vous pouvez les voir dans **l’Explorateur de solutions**:

![](adding-a-view/_static/image3.png)

Le suivant le *Index.cshtml* fichier qui a été créé :

![HelloWorldIndex](adding-a-view/_static/image4.png)

Ajoutez le code HTML suivant sous le `<h2>` balise.

[!code-html[Main](adding-a-view/samples/sample2.html)]

L’ensemble *MvcMovie\Views\HelloWorld\Index.cshtml* fichier est présenté ci-dessous.

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

Si vous utilisez Visual Studio 2012, dans l’Explorateur de solutions, avec le bouton droit cliquez sur le *Index.cshtml* fichier et sélectionnez **afficher dans l’inspecteur de Page**.

![PI](adding-a-view/_static/image5.png)

Le [didacticiel de l’inspecteur de Page](../../views/using-page-inspector-in-aspnet-mvc.md) a plus d’informations sur ce nouvel outil.

Vous pouvez également exécuter l’application et accédez à la `HelloWorld` contrôleur (`http://localhost:xxxx/HelloWorld`). Le `Index` méthode dans votre contrôleur n’avez pas beaucoup de travail ; il a simplement exécuté l’instruction `return View()`, lequel spécifié que la méthode doit utiliser un fichier de modèle de vue pour restituer une réponse au navigateur. Étant donné que vous n’avez pas explicitement spécifier le nom du fichier de modèle de vue à utiliser, ASP.NET MVC par défaut la valeur à l’aide de la *Index.cshtml* afficher le fichier dans le *\Views\HelloWorld* dossier. L’image ci-dessous montre la chaîne &quot;Hello from our View Template !&quot; codées en dur dans la vue.

![](adding-a-view/_static/image6.png)

Il semble assez bonne. Toutefois, notez que la barre de titre affiche &quot;Index mon A ASP.NET&quot; et le lien big en haut de la page indique &quot;votre logo ici.&quot; Ci-dessous le &quot;votre logo ici.&quot; lien d’enregistrement et de journal dans les liens et en dessous qui accède à la page d’accueil, environ et coordonnées pages. Nous allons modifier certaines d'entre elles.

## <a name="changing-views-and-layout-pages"></a>Modification des vues et des Pages de disposition

Tout d’abord, vous souhaitez modifier le &quot;votre logo ici.&quot; titre en haut de la page. Ce texte est courant de chaque page. Il est effectivement implémenté dans un seul endroit dans le projet, même si elles apparaissent sur chaque page dans l’application. Accédez à la */Views/Shared* dossier **l’Explorateur de solutions** et ouvrez le  *\_Layout.cshtml* fichier. Ce fichier s’appelle un *page de disposition* et il est partagé &quot;shell&quot; qui utilisent toutes les autres pages.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Modèles de disposition permettent de spécifier la disposition du conteneur HTML de votre site au même endroit et de l’appliquer sur plusieurs pages de votre site. Recherchez la ligne `@RenderBody()`. `RenderBody` est un espace réservé dans lequel toutes les pages spécifiques aux vues que vous créez s’affichent, &quot;encapsulées&quot; dans la page de disposition. Par exemple, si vous sélectionnez le lien About, le *Views\Home\About.cshtml* vue est restituée à l’intérieur de la `RenderBody` (méthode).

Modifiez le titre du titre du site dans le modèle de disposition &quot;votre logo ici&quot; à &quot;MVC Movie&quot;.

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Remplacez le contenu de l’élément title par le balisage suivant :

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Exécuter l’application et notez qu’il indique à présent &quot;MVC Movie &quot;. Cliquez sur le **sur** lien et voir comment cette page affiche &quot;MVC Movie&quot;, trop. Nous avons pu apporter la modification une fois dans le modèle de disposition et avoir toutes les pages sur le site de refléter le nouveau titre.

![](adding-a-view/_static/image8.png)

Maintenant, nous allons modifier le titre de la vue Index.

Ouvrez *MvcMovie\Views\HelloWorld\Index.cshtml*. Il existe deux emplacements pour apporter une modification : tout d’abord, le texte qui apparaît dans le titre du navigateur, puis, dans l’en-tête secondaire (le `<h2>` élément). Vous allez les modifier légèrement pour voir quel morceau du code modifie quelle partie de l’application.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

Pour indiquer le titre HTML à afficher, le code ci-dessus affecte un `Title` propriété de la `ViewBag` objet (qui se trouve dans le *Index.cshtml* modèle de vue). Si vous revenez au code source du modèle de disposition, vous remarquerez que le modèle utilise cette valeur dans le `<title>` élément dans le cadre de la `<head>` section du contenu HTML que nous avons modifié précédemment. L’utilisation de cette `ViewBag` approche, vous pouvez facilement passer d’autres paramètres entre votre modèle de vue et de votre fichier de disposition.

Exécutez l’application et accédez à `http://localhost:xx/HelloWorld`. Notez que le titre du navigateur, l’en-tête principal et les en-têtes secondaires ont changé. (Si vous ne voyez pas les changements dans le navigateur, vous voyez peut-être le contenu mis en cache. Appuyez sur Ctrl+F5 dans votre navigateur pour forcer le chargement de la réponse du serveur.) Le titre du navigateur est créé avec le `ViewBag.Title` nous avons défini dans le *Index.cshtml* afficher le modèle et les autres &quot;-Movie App&quot; ajouté dans le fichier de disposition.

Notez également comment le contenu dans le *Index.cshtml* afficher le modèle a été fusionné avec la  *\_Layout.cshtml* afficher le modèle et une seule réponse HTML a été envoyée au navigateur. Les modèles de disposition permettent d’apporter facilement des modifications qui s’appliquent à toutes les pages de votre application.

![](adding-a-view/_static/image9.png)

Notre petit peu de &quot;données&quot; (dans ce cas le &quot;Hello from our View Template !&quot; message) est codé en dur, cependant. L’application MVC a un &quot;V&quot; (vue) et vous avez un &quot;C&quot; (contrôleur), mais aucun &quot;M&quot; (model) encore. Peu de temps, nous allons étudier la création d’une base de données et récupérer des données de modèle à partir de celui-ci.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passage de données du contrôleur vers la vue

Avant nous accédez à une base de données et que vous parler de modèles, cependant, nous allons tout d’abord parler de passer des informations à partir du contrôleur à une vue. Classes de contrôleur sont appelées en réponse à une demande d’URL entrante. Une classe de contrôleur est où vous écrivez le code qui gère le navigateur entrant demande, récupère les données à partir d’une base de données et finalement détermine le type de réponse à envoyer au navigateur. Vue modèles peuvent ensuite être utilisés à partir d’un contrôleur pour générer et mettre en forme une réponse HTML au navigateur.

Contrôleurs sont chargés de fournir les données ou les objets sont requis pour un modèle de vue restituer une réponse au navigateur. Une bonne pratique : **un modèle de vue ne doit jamais exécuter la logique métier ou interagir directement avec une base de données**. Au lieu de cela, un modèle de vue doit fonctionner uniquement avec les données qui sont qui lui sont fournies par le contrôleur. Leur gestion &quot;séparation des préoccupations&quot; vous aide à maintenir votre code concis, testable et plus facile à gérer.

Actuellement, le `Welcome` méthode d’action dans le `HelloWorldController` classe prend un `name` et un `numTimes` paramètre, puis sort les valeurs directement dans le navigateur. Au lieu que le contrôleur restitue cette réponse sous forme de chaîne, modifions-le pour utiliser un modèle de vue à la place. Le modèle de vue génère une réponse dynamique, ce qui signifie que vous devez passer les bits de données appropriés du contrôleur à la vue pour générer la réponse. Vous pouvez faire avec le contrôleur de placer les données dynamiques (paramètres) dont le modèle de vue a besoin dans un `ViewBag` objet auquel le modèle de vue peut ensuite accéder.

Retour à la *HelloWorldController.cs* du fichier et changer la `Welcome` méthode pour ajouter un `Message` et `NumTimes` valeur pour le `ViewBag` objet. `ViewBag` est un objet dynamique, ce qui signifie que vous pouvez placer tout ce que vous voulez le `ViewBag` objet ne possède aucune propriété définie jusqu'à ce que vous placez un élément qu’il contient. Le [système de liaison de modèle ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) mappe automatiquement les paramètres nommés (`name` et `numTimes`) à partir de la chaîne de requête dans la barre d’adresses aux paramètres de votre méthode. Le fichier *HelloWorldController.cs* complet ressemble à ceci :

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Maintenant le `ViewBag` objet contient des données qui seront passées à la vue automatiquement.

Ensuite, vous avez besoin d’un modèle de vue Bienvenue ! Dans le **Build** menu, sélectionnez **MvcMovie Build** s’assurer que le projet est compilé.

Puis avec le bouton droit à l’intérieur de la `Welcome` (méthode) et cliquez sur **ajouter une vue**.

![](adding-a-view/_static/image10.png)

Voici à quoi le **ajouter une vue** boîte de dialogue ressemble à :

![](adding-a-view/_static/image11.png)

Cliquez sur **ajouter**, puis ajoutez le code suivant sous le `<h2>` élément dans le nouveau *Welcome.cshtml* fichier. Vous allez créer une boucle qui déclare &quot;Hello&quot; aussi souvent que l’utilisateur indique qu’il doit. L’ensemble *Welcome.cshtml* fichier est présenté ci-dessous.

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

Exécutez l’application et accédez à l’URL suivante :

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Maintenant les données sont extraites de l’URL et passées au contrôleur en utilisant le [binder de modèle](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). Le contrôleur empaquette les données dans un `ViewBag` objet et passe cet objet à la vue. Puis, la vue affiche les données au format HTML à l’utilisateur.

![](adding-a-view/_static/image12.png)

Dans l’exemple ci-dessus, nous avons utilisé un `ViewBag` objet à passer des données à partir du contrôleur à une vue. Ce dernier dans le didacticiel, nous allons utiliser un modèle de vue pour passer des données à partir d’un contrôleur à une vue. L’approche de modèle de vue pour passer des données est généralement beaucoup préférée à l’approche de sac d’affichage. Consultez le billet de blog [V fortement typée des vues dynamiques](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) pour plus d’informations.

Hé bien, ce qui était un type d’un &quot;M&quot; pour le modèle, mais pas le type de base de données. Créons une base de données de films en utilisant ce que nous avons appris.

> [!div class="step-by-step"]
> [Précédent](adding-a-controller.md)
> [Suivant](adding-a-model.md)
