---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Partie 3 : Vues et ViewModels | Microsoft Docs'
author: jongalloway
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store. La partie 3 explique Views et ViewModels.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 828ff18abcc5932f82be71a45ebde589eeb051fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824896"
---
<a name="part-3-views-and-viewmodels"></a>Partie 3 : Vues et ViewModels
====================
par [Jon Galloway](https://github.com/jongalloway)

> Le Store de musique MVC est une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Studio pour le développement web.  
>   
> Le Store de musique MVC est une implémentation de magasin d’exemple léger qui vend des albums de musique en ligne et implémente l’administration de site de base, connexion de l’utilisateur et les fonctionnalités de panier d’achat.  
>   
> Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application ASP.NET MVC Music Store. La partie 3 explique Views et ViewModels.


Jusqu'à présent, nous avons simplement été retourner des chaînes à partir d’actions de contrôleur. Qui constitue un excellent moyen pour avoir une idée du fonctionnement des contrôleurs, mais il n’est pas comment vous souhaitez créer une application web réelle. Nous allons un meilleur moyen de générer du code HTML à visiter notre site de navigateurs : un où nous pouvons utiliser des fichiers de modèle pour personnaliser plus facilement le contenu HTML renvoyer. C’est exactement ce que font les affichages.

## <a name="adding-a-view-template"></a>Ajout d’un modèle de vue

Pour utiliser un modèle de vue, nous allons modifier la méthode Index du HomeController pour renvoyer un ActionResult et qu’il renvoie View(), comme ci-dessous :

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

Les modifications ci-dessus indique au lieu de retourné une chaîne, nous souhaitons à la place une « vue » permet de générer un résultat de retour.

Nous allons maintenant ajouter un modèle de vue approprié à notre projet. Pour ce faire, nous allons positionner le curseur de texte au sein de la méthode d’action Index, puis avec le bouton droit et sélectionnez « Ajouter une vue ». Cela fera apparaître la boîte de dialogue Ajouter une vue :

![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)

La boîte de dialogue « Ajouter une vue » permet de générer rapidement et facilement les fichiers de modèle de vue. Par défaut, la « vue d’ajouter » boîte de dialogue pré-remplit le nom du modèle de vue à créer afin qu’elle corresponde à la méthode d’action qui va l’utiliser. Étant donné que nous avons utilisé le menu contextuel « Ajouter une vue » au sein de la méthode d’action Index() de notre HomeController, la boîte de dialogue « Ajouter une vue » ci-dessus a « Index » comme nom de la vue prérempli par défaut. Nous n’avez pas besoin de modifier les options sur cette boîte de dialogue, cliquez sur le bouton Ajouter.

Lorsque nous cliquons sur le bouton Ajouter, Visual Web Developer crée une nouvelle Index.cshtml afficher le modèle pour nous dans le répertoire \Views\Home, si la création du dossier n’existe pas déjà.

![](mvc-music-store-part-3/_static/image2.png)

Le nom et dossier l’emplacement du fichier « Index.cshtml » est important et suit les conventions de nommage d’ASP.NET MVC par défaut. Le nom de répertoire, \Views\Home, correspond au contrôleur - ce qui est nommé HomeController. Le nom du modèle de vue, Index, correspond à la méthode d’action de contrôleur qui affichent la vue.

ASP.NET MVC vous permet de nous éviter d’avoir à spécifier explicitement le nom ou l’emplacement d’un modèle de vue lorsque nous utilisons cette convention de nommage pour retourner une vue. Il doit par défaut rendre le modèle de vue \Views\Home\Index.cshtml lorsque nous écrire le code ci-dessous au sein de notre HomeController :

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

Visual Web Developer créé et ouvert le modèle de vue « Index.cshtml » une fois que nous avons cliqué sur le bouton « Ajouter » dans la boîte de dialogue « Ajouter une vue ». Le contenu de Index.cshtml est présenté ci-dessous.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

Cette vue est à l’aide de la syntaxe Razor, qui est plus concise que le moteur d’affichage Web Forms utilisé dans ASP.NET Web Forms et des versions antérieures d’ASP.NET MVC. Le moteur d’affichage Web Forms est toujours disponible dans ASP.NET MVC 3, mais de nombreux développeurs trouvent que le moteur d’affichage Razor tient vraiment bien de développement ASP.NET MVC.

Les trois premières lignes définissent le titre de page à l’aide de ViewBag.Title. Nous allons examiner comment cela fonctionne plus en détail plus rapidement, mais tout d’abord nous allons mettre à jour le texte d’en-tête de texte et afficher la page. Mise à jour le &lt;h2&gt; balise à dire « This est la Page d’accueil » comme indiqué ci-dessous.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

L’application montre que notre nouveau texte est visible sur la page d’accueil en cours d’exécution.

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>À l’aide d’une disposition pour les éléments communs de site

La plupart des sites Web ont du contenu qui est partagé entre plusieurs pages : navigation, les pieds de page, les images de logo, les références de feuille de style, etc. Le moteur d’affichage Razor facilite la gestion à l’aide d’une page appelée \_Layout.cshtml qui a été créé automatiquement pour nous dans le dossier Shared/vues /.

![](mvc-music-store-part-3/_static/image4.png)

Double-cliquez sur ce dossier pour afficher le contenu, qui sont présentées ci-dessous.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

Le contenu à partir de notre vues individuelles est affiché par le @RenderBody() commande et n’importe quel contenu commun que nous souhaitons figurer en dehors de qui peuvent être ajoutés à la \_Layout.cshtml balisage. Nous devrons attraper notre Store de musique MVC pour avoir un en-tête commun avec des liens à notre zone de page d’accueil et Store sur toutes les pages du site, donc nous allons ajouter que pour le modèle directement au-dessus qui @RenderBody() instruction.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>La mise à jour de la feuille de style

Le modèle de projet vide inclut un fichier CSS très simplifié qui inclut uniquement les styles utilisés pour afficher les messages de validation. Notre concepteur a fourni du CSS et des images supplémentaires pour définir l’apparence de notre site, donc nous allons ajouter ceux maintenant.

Le fichier CSS et les Images mis à jour sont inclus dans le répertoire de contenu de Assets.zip MvcMusicStore qui est disponible à l’adresse [MVC-musique-Store](https://github.com/evilDave/MVC-Music-Store). Nous allons sélectionner les deux d'entre eux dans l’Explorateur Windows et les déposer dans le dossier de contenu de notre Solution dans Visual Web Developer, comme indiqué ci-dessous :

![](mvc-music-store-part-3/_static/image5.png)

Vous allez être invité à confirmer si vous souhaitez remplacer le fichier Site.css existant. Cliquez sur Oui.

![](mvc-music-store-part-3/_static/image6.png)

Le dossier de contenu de votre application s’affiche maintenant comme suit :

![](mvc-music-store-part-3/_static/image7.png)

Maintenant nous allons exécuter l’application et visualiser l’aspect de nos modifications sur la page d’accueil.

![](mvc-music-store-part-3/_static/image8.png)

- Passons en revue ce qui a changé : méthode d’action le HomeController Index trouvé et affiche le modèle \Views\Home\Index.cshtmlView, même si notre code appelé « retour View() », car la convention d’affectation de noms standard de suivi de notre modèle de vue.
- La Page d’accueil affiche un message d’accueil simple qui est défini dans le modèle de vue \Views\Home\Index.cshtml.
- À l’aide de la Page d’accueil est notre \_Layout.cshtml modèle, et par conséquent, le message d’accueil est contenu dans la mise en page de site standard HTML.

## <a name="using-a-model-to-pass-information-to-our-view"></a>À l’aide d’un modèle pour transmettre des informations à notre vue

Un modèle de vue qui affiche simplement codé en dur HTML ne va pas faire un site web très intéressant. Pour créer un site web dynamique, nous devrons au lieu de cela passer des informations à partir de nos actions de contrôleur à nos modèles de vue.

Dans le modèle Model-View-Controller, le terme À que modèle fait référence des objets qui représentent les données dans l’application. Souvent, les objets de modèle correspondent aux tables dans votre base de données, mais ils n’êtes pas obligé.

Méthodes d’action de contrôleur qui retournent un ActionResult peuvent passer un objet de modèle à la vue. Cela permet à un contrôleur proprement regrouper toutes les informations nécessaires pour générer une réponse, puis à transmettre ces informations à un modèle de vue à utiliser pour générer la réponse HTML appropriée. Il s’agit le plus simple à comprendre par voir en action, nous allons donc commencer.

Tout d’abord, nous allons créer des classes de modèle pour représenter les Genres et Albums dans notre magasin. Nous allons commencer en créant une classe de Genre. Cliquez sur le dossier « Models » au sein de votre projet, choisissez l’option « Ajouter une classe », nommez le fichier « Genre.cs ».

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

Ensuite, ajoutez une propriété de nom de chaîne publique à la classe qui a été créée :

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*Remarque : dans le cas, vous vous poseriez la question, le {get ; définir ;} effectue la notation utilisation de fonctionnalité de propriétés implémentées automatiquement de #. Cela nous donne les avantages d’une propriété sans nécessiter de déclarer un champ de stockage.*

Ensuite, suivez les mêmes étapes pour créer une classe d’Album (nommée Album.cs) qui a un titre et une propriété du Genre :

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

Nous pouvons maintenant modifier le StoreController pour utiliser des vues qui affichent des informations dynamiques à partir de notre modèle. Si le paramètre - à des fins de démonstration maintenant - nous avons nommé notre Albums basés sur l’ID de demande, nous pouvons afficher ces informations comme dans la vue ci-dessous.

![](mvc-music-store-part-3/_static/image10.png)

Nous allons commencer en modifiant l’action des détails de Store pour qu’il affiche les informations pour un seul album. Ajoutez une instruction « using » en haut de la **StoreControllers** classe pour inclure l’espace de noms MvcMusicStore.Models, nous n’avez pas besoin de taper MvcMusicStore.Models.Album chaque fois que nous souhaitons utiliser la classe album. La section instructions « using » de cette classe doit maintenant apparaître comme suit.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

Ensuite, nous mettrons à jour l’action du contrôleur détails afin qu’elle retourne un ActionResult plutôt qu’une chaîne, comme nous l’avons fait avec la méthode d’Index du HomeController.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

Nous pouvons maintenant modifier la logique pour retourner un objet d’Album à la vue. Plus loin dans ce didacticiel nous récupérons les données à partir d’une base de données – mais pour l’instant, nous allons utiliser « factice de données » pour commencer.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*Remarque : Si vous n’êtes pas familiarisé avec c#, vous pouvez supposer que var reprenant que notre variable album est à liaison tardive. Qui n’est pas correct : le compilateur c# à l’aide de-l’inférence de type en fonction de ce que nous allons affecter à la variable pour déterminer que cet album est de type Album et compilation de la variable locale album comme type d’Album, donc nous obtenons la vérification au moment de la compilation et l’éditeur de code Visual Studio prise en charge.*

Nous allons maintenant créer un modèle de vue qui utilise notre Album pour générer une réponse HTML. Avant cela, nous devons générer le projet afin que la boîte de dialogue Ajouter une vue connaît de notre classe Album nouvellement créé. Vous pouvez générer le projet en sélectionnant le Debug⇨Build MvcMusicStore élément de menu (pour plus de crédibilité, vous pouvez utiliser le raccourci Ctrl-Maj-B de numéros pour générer le projet).

![](mvc-music-store-part-3/_static/image11.png)

Maintenant que nous avons créé nos classes de prise en charge, nous sommes prêts à créer notre modèle de vue. Avec le bouton droit dans la méthode de détails et sélectionnez « Ajouter un affichage... » dans le menu contextuel.

![](mvc-music-store-part-3/_static/image12.png)

Nous allons créer un nouveau modèle de vue, comme nous l’avons fait avant avec le HomeController. Étant donné que nous allons la créer à partir de la StoreController générée ultérieurement utilisera par défaut dans un fichier \Views\Store\Index.cshtml.

Contrairement à avant, nous allons vérifier la case à cocher de la vue « Créer un fortement typée ». Ensuite, nous allons sélectionner notre classe « Album » dans la liste « Vue données-class »-downlist. Cela entraîne la boîte de dialogue « Ajouter une vue » créer un modèle de vue qui attend un Album objet sera passé à ce dernier à utiliser.

![](mvc-music-store-part-3/_static/image13.png)

Lorsque nous cliquons sur le bouton « Ajouter » notre modèle de vue \Views\Store\Details.cshtml sera créé, qui contient le code suivant.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

Notez que la première ligne, ce qui indique que cette vue est fortement typée à notre classe Album. Le moteur d’affichage Razor comprend qu’a été passée un objet Album, afin que nous puissions accéder facilement aux propriétés de modèle et même ont l’avantage d’IntelliSense dans l’éditeur de Visual Web Developer.

Mise à jour le &lt;h2&gt; balise afin qu’elle affiche la propriété de titre d’Album en modifiant cette ligne doit s’afficher comme suit.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

Notez qu’IntelliSense est déclenchée lorsque vous entrez la période après la @Model mot clé, en affichant les propriétés et méthodes qui prend en charge de la classe Album.

Nous allons maintenant exécuter à nouveau notre projet et accédez à l’URL de détails/Store/5. Nous allons voir les détails d’un Album comme ci-dessous.

![](mvc-music-store-part-3/_static/image14.png)

Nous allons maintenant rendre une mise à jour similaire pour la méthode d’action Parcourir Store. Mettre à jour de la méthode afin qu’elle retourne un ActionResult et modifiez la logique de la méthode afin qu’il crée un nouvel objet de Genre et renvoie à la vue.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

Avec le bouton droit dans la méthode de parcourir et sélectionnez « Ajouter un affichage... » dans le menu contextuel, puis ajouter une vue qui est fortement typée ajoutez fortement typé à la classe de Genre.

![](mvc-music-store-part-3/_static/image15.png)

Mise à jour le &lt;h2&gt; élément dans la vue de code (dans /Views/Store/Browse.cshtml) pour afficher les informations de Genre.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

Maintenant nous allons exécuter à nouveau notre projet et accédez à/Store/Parcourir ? Genre = URL Disco. Nous allons voir la page de navigation affichée comme ci-dessous.

![](mvc-music-store-part-3/_static/image16.png)

Enfin, nous allons effectuer une mise à jour légèrement plus complexe le **Index Store** méthode d’action et pour afficher une liste de tous les Genres dans notre magasin. Nous allons le faire à l’aide d’une liste de Genres comme notre objet de modèle, plutôt que simplement un Genre unique.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

Avec le bouton droit dans la méthode d’action Store Index et sélectionnez Ajouter une vue comme avant, sélectionnez Genre comme la classe de modèle, puis appuyez sur le bouton Ajouter.

![](mvc-music-store-part-3/_static/image17.png)

Tout d’abord, nous allons modifier le @model déclaration pour indiquer que la vue attend Genre plusieurs objets plutôt que simplement un. Modifiez la première ligne de /Store/Index.cshtml comme suit :

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

Cela indique le moteur d’affichage Razor vous travaillerez avec un objet de modèle qui peut contenir plusieurs objets de Genre. Nous utilisons un IEnumerable&lt;Genre&gt; au lieu d’une liste&lt;Genre&gt; dans la mesure où il est plus générique, ce qui nous permet de modifier ultérieurement le type de notre modèle pour tout type d’objet qui prend en charge l’interface IEnumerable.

Ensuite, nous allons parcourez en boucle les objets de Genre dans le modèle comme indiqué dans le code de vue terminée ci-dessous.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

Notez que nous avons une prise en charge IntelliSense complète comme nous Entrez ce code, afin que lorsque vous tapez «@Model. » Nous voyons toutes les méthodes et propriétés prises en charge par un IEnumerable de type Genre.

![](mvc-music-store-part-3/_static/image18.png)

Dans notre boucle « foreach », Visual Web Developer sait que chaque élément est de type Genre, nous voir IntelliSense pour chaque type de Genre.

![](mvc-music-store-part-3/_static/image19.png)

Ensuite, la fonctionnalité de génération de modèles automatique examiné l’objet de Genre et déterminé que chacune aura une propriété Name, afin qu’il parcourt en boucle et les écrit. Il génère également des liens à la modifier, les détails et les supprimer pour chaque élément individuel. Nous allons en tirer parti plus loin dans notre responsable de magasin, mais pour l’instant, nous souhaitons avoir une liste simple à la place.

Lorsque nous pouvons exécuter l’application et que vous accédez à /Store, nous voyons que le nombre et la liste de Genres est affiché.

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>Ajout de liens entre les pages

Notre URL /Store qui répertorie les Genres actuellement répertorie les noms de Genre simplement en tant que texte brut. Nous allons modifier cela afin qu’au lieu de texte brut à la place le lien de noms de Genre à l’URL du Store et parcourir appropriés, afin que le clic sur un genre de musique, comme « Disco » permet d’accéder à/Store/Parcourir ? genre = Disco URL. Nous pouvons mettre à jour notre modèle de vue \Views\Store\Index.cshtml à la sortie de ces liens à l’aide de code semblable au suivant **(ne tapez pas cela dans, nous allons améliorer dessus)**:

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

Cela fonctionne, mais elle risque de problèmes plus tard dans la mesure où elle s’appuie sur une chaîne codée en dur. Par exemple, si nous voulions renommer le contrôleur, nous devons effectuer des recherches dans notre code à la recherche pour les liens qui doivent être mis à jour.

Une autre approche que nous pouvons utiliser consiste à tirer parti d’une méthode d’assistance HTML. ASP.NET MVC inclut des méthodes d’assistance HTML qui sont disponibles à partir de notre code de modèle de vue pour effectuer diverses tâches courantes, comme cela. La méthode d’assistance Html.ActionLink() est particulièrement utile et permet de facilement générer HTML &lt;un&gt; établit un lien et s’occupe de détails ennuyeux tels que de s’assurer que les chemins d’URL sont correctement codées URL.

Html.ActionLink() a plusieurs surcharges différentes pour autoriser la spécification de toutes les informations que vous avez besoin pour vos liens. Dans le cas le plus simple, vous allez fournir le texte du lien et la méthode d’Action pour accéder à un clic sur le lien hypertexte sur le client. Par exemple, nous pouvons lier à « / Store / « méthode Index() sur la page de détails de Store avec le texte du lien « Aller vers le Store Index » à l’aide de l’appel suivant :

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*Remarque : dans ce cas, nous ne devons spécifier le nom du contrôleur, car nous allons simplement lier à une autre action au sein du même contrôleur qui est rendu l’affichage actuel.*

Notre liens vers la page Parcourir devez passer un paramètre, cependant, nous allons utiliser une autre surcharge de la méthode Html.ActionLink qui accepte trois paramètres :

- 1. Texte du lien, qui affiche le nom du Genre
- 2. Nom d’action de contrôleur (Parcourir)
- 3. Valeurs de paramètre d’itinéraire, en spécifiant le nom (Genre) et la valeur (nom du Genre)

Placement que tous ensemble, voici comment nous allons écrire ces liens à la vue Index de Store :

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

Lorsque nous pouvons exécuter notre projet à nouveau et que vous accédez à l’URL /Store/ nous voyez à présent une liste de genres. Chaque genre est un lien hypertexte : lorsque vous cliquez sur prendra nous notre/Store/Parcourir ? genre =*[genre]* URL.

![](mvc-music-store-part-3/_static/image3.jpg)

Le code HTML de la liste de genre ressemble à ceci :

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> [Précédent](mvc-music-store-part-2.md)
> [Suivant](mvc-music-store-part-4.md)
