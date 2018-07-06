---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: Réutilisation de l’interface utilisateur à l’aide de Pages maîtres et des vues partielles | Microsoft Docs
author: microsoft
description: Étape 7 examine les façons que nous pouvons appliquer le principe DRY au sein de nos modèles de vue pour éliminer les doublons de code, à l’aide de pages maîtres et des modèles de vue partielle.
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: 961378d6d710c678a0cd223a5c31544f014ace7f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839592"
---
<a name="re-use-ui-using-master-pages-and-partials"></a>Réutilisation de l’interface utilisateur à l’aide de Pages maîtres et des vues partielles
====================
par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit d’étape 7 a gratuit [« « l’application NerdDinner](introducing-the-nerddinner-tutorial.md) qui présente en détail comment créer un petit mais terminé, l’application web à l’aide d’ASP.NET MVC 1.
> 
> Étape 7 examine les façons que nous pouvons appliquer le « principe DRY » au sein de nos modèles de vue pour éliminer les doublons de code, à l’aide de pages maîtres et des modèles de vue partielle.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre le [mise en route avec MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de musique MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) didacticiels.


## <a name="nerddinner-step-7-partials-and-master-pages"></a>NerdDinner étape 7 : Vues partielles et les Pages maîtres

Une des philosophies de conception QU'ASP.NET MVC prend en compte est le principe « Faire pas se répéter » (communément appelé « Sec »). Une conception DRY permet d’éviter la duplication de code et de logique, ce qui rend finalement les applications plus rapidement à générer et plus faciles à gérer.

Nous avons déjà vu le principe DRY appliqué dans plusieurs de nos scénarios NerdDinner. Voici quelques exemples : notre logique de validation est implémentée au sein de notre couche de modèle, ce qui lui permet d’être appliquées au sein de ces deux modifier et créer des scénarios dans notre contrôleur ; Nous ré-utilisons le modèle de vue « NotFound » pour toutes les méthodes d’action Edit, détails et Delete ; Nous utilisons un convention d’affectation de noms modèle avec nos modèles de vue, ce qui vous évite de devoir spécifier explicitement le nom lorsque nous appelons la méthode d’assistance View() ; et nous ré-utilisez la classe DinnerFormViewModel pour à la fois modifier et créer des scénarios d’action.

Voyons maintenant les façons que nous pouvons appliquer le « principe DRY » au sein de nos modèles de vue pour éliminer les doublons de code il également.

### <a name="re-visiting-our-edit-and-create-view-templates"></a>Revisiter notre modifier et créer des modèles de vue

Actuellement, nous utilisons deux modèles de vue différents – « Edit.aspx » et « Create.aspx » – pour afficher notre formulaire dîner l’interface utilisateur. Une comparaison rapide visual d'entre eux met en évidence le degré de similitude qu’ils le sont. Voici à quoi ressemble le formulaire de création :

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

Et Voici à quoi ressemble notre formulaire « Modifier » :

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

Pas beaucoup de différence y a-t-il ? Autre que le texte de titre et l’en-tête, les contrôles de disposition et l’entrée de formulaire sont identiques.

Si nous ouvrons le « Edit.aspx » et « Create.aspx » les modèles de vue, vous trouverez qui ils contiennent code de contrôle de disposition et entrée forme identique. Cette duplication signifie que nous nous retrouvons avoir à apporter des modifications deux fois à chaque fois que nous introduire ou modifier une nouvelle propriété dîner - qui n’est pas bon.

### <a name="using-partial-view-templates"></a>À l’aide de modèles de vue partielle

ASP.NET MVC prend en charge la possibilité de définir des modèles de « vue partielle » qui peuvent être utilisées pour encapsuler la logique de rendu de vue pour une partie secondaire d’une page. « Partiels » permettent de définir une logique de rendu vue une seule fois, puis réutiliser dans plusieurs endroits dans une application.

Pour aider à « Sec-up », notre Edit.aspx et duplication de modèle de vue Create.aspx, nous pouvons créer un modèle de vue partielle nommé « DinnerForm.ascx » qui encapsule la disposition du formulaire et les éléments d’entrée communs aux deux. Nous aborderons ce point en cliquant sur notre annuaire dîners/vues/et en choisissant la « Add -&gt;vue « commande de menu :

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

Ceci affichera la boîte de dialogue « Ajouter une vue ». Nous allons nommer la nouvelle vue créer des « DinnerForm », sélectionnez la case à cocher « Créer une vue partielle » dans la boîte de dialogue et nous indiquer que nous transmettons il une classe DinnerFormViewModel :

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

Lorsque nous cliquons sur le bouton « Ajouter », Visual Studio crée un nouveau modèle de vue « DinnerForm.ascx » pour nous dans le répertoire « \Views\Dinners ».

Nous pouvons ensuite copier/coller la disposition du formulaire en double d’entrée de code de contrôle à partir de nos modèles de vue Edit.aspx/ Create.aspx dans notre nouveau modèle de vue partielle « DinnerForm.ascx » :

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

Nous pouvons ensuite mettre à jour nos modèles de vue de modification et de création pour appeler le modèle partiels DinnerForm et éviter la duplication du formulaire. Nous pouvons faire cela en appelant Html.RenderPartial("DinnerForm") nos modèles de vue :

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

Vous pouvez qualifier explicitement le chemin d’accès du modèle partiels lors de l’appel Html.RenderPartial (par exemple : ~ Views/Dinners/DinnerForm.ascx »). Dans notre code ci-dessus, cependant, nous sommes en tirant parti du modèle d’affectation de noms basé sur une convention dans ASP.NET MVC et simplement spécifier « DinnerForm » comme nom de la partielle à restituer. Lorsque nous faisons cela ASP.NET MVC recherche premier dans le répertoire views conventionnelle (DinnersController cela serait dîners/vues /). S’il ne trouve pas le modèle partiels il il sera recherchez-le dans le répertoire /Views/Shared.

Lorsque Html.RenderPartial() est appelée uniquement avec le nom de la vue partielle, ASP.NET MVC passe à la vue partielle même modèle et ViewData dictionnaire objets utilisés par le modèle de vue appelant. Vous pouvez également, il existe des versions surchargées de Html.RenderPartial() qui vous permettent de passer un autre objet de modèle et/ou de dictionnaire ViewData de la vue partielle à utiliser. Cela est utile pour les scénarios où vous souhaitez uniquement passer un sous-ensemble du modèle complet/ViewModel.

| **Rubrique de côté : Pourquoi &lt;%%&gt; au lieu de &lt;% = %&gt;?** |
| --- |
| L’une des choses subtiles que vous avez peut-être remarqué avec le code ci-dessus est que nous utilisons un &lt;%%&gt; bloquer au lieu d’un &lt;% = %&gt; bloquer lors de l’appel Html.RenderPartial(). &lt;% = %&gt; blocs dans ASP.NET indiquent qu’un développeur veut afficher une valeur spécifiée (par exemple : &lt;% = « Hello » %&gt; affichant « Hello »). &lt;%%&gt; blocs indiquent plutôt que le développeur souhaite exécuter du code, et que tout restitué de sortie au sein de celles-ci doit être effectué explicitement (par exemple : &lt;Response.Write("Hello") %&gt;. La raison pour laquelle nous utilisons un &lt;%%&gt; bloc avec notre code Html.RenderPartial ci-dessus est, car la méthode Html.RenderPartial() ne retourne pas une chaîne et qu’il génère à la place le contenu directement à l’appelant afficher le modèle de flux de sortie. Il procède ainsi pour des raisons de l’efficacité de performances et en procédant ainsi, elle évite d’avoir à créer un objet chaîne temporaire (potentiellement très grande). Cela réduit l’utilisation de la mémoire et améliore le débit global d’applications. Une erreur courante lors de l’utilisation de Html.RenderPartial() est d’oublier d’ajouter un point-virgule à la fin de l’appel quand il est dans un &lt;%%&gt; bloc. Par exemple, ce code entraîne une erreur du compilateur : &lt;Html.RenderPartial("DinnerForm") %&gt; vous devez plutôt écrire : &lt;% Html.RenderPartial("DinnerForm") ; %&gt; effet, &lt;%%&gt; blocs sont des instructions de code autonome et lors de l’utilisation de code c# instructions doivent se terminer par un point-virgule. |

### <a name="using-partial-view-templates-to-clarify-code"></a>À l’aide de modèles de vue partielle à clarifier le Code

Nous avons créé le modèle de vue partielle « DinnerForm » pour éviter de dupliquer la logique de rendu d’affichage à plusieurs endroits. Il s’agit de la raison la plus courante pour créer des modèles de vue partielle.

Parfois, il est toujours judicieux pour créer des vues partielles même lorsqu’elles sont appelées uniquement dans un emplacement unique. Modèles de vue très compliqué peuvent devenir beaucoup plus faciles à lire lorsque leur logique de rendu de vue est extrait et partitionnée en une seule ou plus bien nommé modèles partielles.

Par exemple, considérez le ci-dessous extrait de code à partir du fichier Site.master dans notre projet (ce qui nous nous allons examiner sous peu). Le code est relativement simple à lire : en partie parce que la logique pour afficher une connexion/déconnexion situé en haut à droite de l’écran est encapsulée dans partielle « LogOnUserControl » :

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

Chaque fois que vous êtes amené à confondre essayez de comprendre le balisage html/code au sein d’un modèle de vue, envisagez de si il ne serait plus clair si certaines de ces a été extraite et refactorisés dans les vues partielles bien nommés.

### <a name="master-pages"></a>Pages maîtres

La prise en charge des vues partielles, ASP.NET MVC prend également en charge la possibilité de créer des modèles de « page maître » qui peuvent être utilisées pour définir la mise en page commune et le html de niveau supérieur d’un site. Espace réservé de contrôles peuvent ensuite être ajoutés à la page maître pour identifier les zones remplaçables qui peuvent être remplacées ou « remplis » par les vues de contenu. Cela fournit un moyen très efficace (et sec) pour appliquer une disposition courante sur une application.

Par défaut, les nouveaux projets ASP.NET MVC ont un modèle de page maître automatiquement ajouté. Cette page maître est nommée « Site.master » et se trouve dans le dossier \Views\Shared\ :

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

Le fichier Site.master par défaut se présente comme suit. Il définit le code html externe du site, ainsi que d’un menu de navigation en haut. Il contient deux contrôles remplaçable par l’espace réservé de contenu : un pour le titre et l’autre pour où le contenu principal d’une page doit être remplacé :

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

Tous les modèles de vue que nous avons créé pour notre application NerdDinner (« List », « Details », « Modifier », « Créer », « NotFound », etc.) ont été basés sur ce modèle de Site.master. Cela est indiqué par le biais de l’attribut « MasterPageFile » qui a été ajouté par défaut vers le haut &lt;% @ Page %&gt; directive lorsque nous avons créé notre vues à l’aide de la boîte de dialogue « Ajouter une vue » :

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

Cela signifie que nous pouvons modifier le contenu Site.master et ont les modifications automatiquement appliquées et utilisé lorsque nous affichons un de nos modèles de vue.

Nous allons mettre à jour section d’en-tête de notre Site.master afin que l’en-tête de notre application est « NerdDinner » au lieu de « My Application MVC ». Nous allons également mettre à jour notre menu de navigation afin que le premier onglet est « Rechercher un dîner » (gérées par la méthode d’action du HomeController Index()) et nous allons ajouter un nouvel onglet appelé « Hôte un dîner » (gérées par la méthode d’action de la DinnersController Create()) :

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Lorsque nous enregistrons le fichier Site.master et actualiser notre navigateur, nous allons voir notre en-tête changements soient répercutés à toutes les vues au sein de notre application. Exemple :

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

Et avec le */Dinners/modification / [id]* URL :

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>Étape suivante

Vues partielles et les pages maîtres fournissent des options très flexibles qui vous permettent d’organiser correctement les vues. Vous trouverez qu’ils permettent de vous éviter de dupliquer la vue de contenu / code et facilitent la lecture et la maintenance de vos modèles de vue.

Nous allons maintenant revisiter l’annonce scénario que nous avons créé précédemment et activer la prise en charge de la pagination évolutive.

> [!div class="step-by-step"]
> [Précédent](use-viewdata-and-implement-viewmodel-classes.md)
> [Suivant](implement-efficient-data-paging.md)
