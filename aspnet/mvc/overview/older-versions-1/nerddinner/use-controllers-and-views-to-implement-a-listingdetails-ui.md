---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: Utiliser des contrôleurs et des vues pour implémenter une interface utilisateur liste/détails | Microsoft Docs
author: microsoft
description: Étape 4 montre comment ajouter un contrôleur à l’application qui tire parti de notre modèle pour fournir aux utilisateurs une expérience de navigation de données liste/détails...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 4fe065e29950a076de07d73205a97399f82f07d6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377874"
---
<a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>Utiliser des contrôleurs et des vues pour implémenter une interface utilisateur liste/détails
====================
par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit d’étape 4 a gratuit [« « l’application NerdDinner](introducing-the-nerddinner-tutorial.md) qui présente en détail comment créer un petit mais terminé, l’application web à l’aide d’ASP.NET MVC 1.
> 
> Étape 4 montre comment ajouter un contrôleur à l’application qui tire parti de notre modèle pour fournir aux utilisateurs une expérience de navigation de la liste/détails de données pour dîners sur notre site NerdDinner.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre le [mise en route avec MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de musique MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) didacticiels.


## <a name="nerddinner-step-4-controllers-and-views"></a>NerdDinner étape 4 : Contrôleurs et les vues

Avec les infrastructures web traditionnelles (classic ASP, PHP, ASP.NET Web Forms, etc.), les URL entrantes sont généralement mappées aux fichiers sur le disque. Par exemple : une requête pour une URL semblable à « / Products.aspx » ou « /.PHP » peut être traitée par un fichier « Products.aspx » ou «.php ».

Les infrastructures MVC Web mappent des URL au code serveur d’une manière légèrement différente. Au lieu de mapper des URL entrantes aux fichiers, ils mappent à la place des URL aux méthodes sur les classes. Ces classes sont appelées « Contrôleurs » et ils sont responsables du traitement des requêtes HTTP entrantes, gestion des entrées utilisateur, de récupérer et de l’enregistrement des données et de déterminer la réponse à envoyer au client (afficher le code HTML, télécharger un fichier, rediriger vers un autre URL, etc.).

Maintenant que nous avons créé un modèle de base pour notre application NerdDinner, notre prochaine étape consistera à ajouter un contrôleur à l’application qui en profite pour fournir aux utilisateurs une expérience de navigation de la liste/détails de données pour dîners sur notre site.

### <a name="adding-a-dinnerscontroller-controller"></a>Ajout d’un contrôleur DinnersController

Nous allons commencer en cliquant sur le dossier « Contrôleurs » au sein de notre projet web, puis sélectionnez le **Add -&gt;contrôleur** commande de menu (vous pouvez également exécuter cette commande en tapant Ctrl-M, Ctrl-C) :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

Cela fera apparaître la boîte de dialogue « Ajouter un contrôleur » :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

Nous allons nommer le nouveau contrôleur de « DinnersController » et cliquez sur le bouton « Ajouter ». Visual Studio ajoutera ensuite un fichier DinnersController.cs sous notre répertoire \Controllers :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

Il ouvre également la nouvelle classe DinnersController dans l’éditeur de code.

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>Ajout de Index() et méthodes d’Action Details() à la classe DinnersController

Nous souhaitons activer les visiteurs à l’aide de notre application pour parcourir une liste de dîners à venir et leur permettre de cliquer sur n’importe quel dîner dans la liste pour afficher les détails spécifiques. Pour cela, nous allons les URL suivantes à partir de notre application de publication :

| **URL** | **Fonction** |
| --- | --- |
| */Dinners/* | Afficher une liste HTML de dîners à venir |
| */Dinners/détails / [id]* | Afficher des détails sur un dîner spécifique indiqué par un paramètre « id » incorporé dans l’URL qui correspond à la DinnerID du dîner dans la base de données. Par exemple : /Dinners/Details/2 afficherait une page HTML avec des détails sur le dîner DinnerID dont la valeur est 2. |

Nous publierons des implémentations initiales de ces URL en ajoutant deux public « méthodes d’action » à notre classe DinnersController comme ci-dessous :

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

Nous allons exécuter l’application NerdDinner, puis utilisez notre navigateur pour appeler les. Saisie dans le *« / dîners / »* URL entraîne notre *Index()* méthode à exécuter et il renvoie ensuite la réponse suivante :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

Saisie dans le *« / dîners/détails/2 »* URL entraîne notre *Details()* méthode à exécuter et renvoyer la réponse suivante :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

Vous vous demandez peut-être - comment ASP.NET MVC savait pour créer notre classe DinnersController et appeler ces méthodes ? De comprendre que nous allons jeter un coup de œil sur le fonctionnement du routage.

### <a name="understanding-aspnet-mvc-routing"></a>Routage de présentation ASP.NET MVC

ASP.NET MVC inclut un puissant moteur de routage d’URL qui offre une grande souplesse pour contrôler la façon dont les URL sont mappés aux classes de contrôleur. Il nous permet de personnaliser entièrement la façon dont ASP.NET MVC choisit à quelle classe de contrôleur pour créer, de la méthode à appeler sur celle-ci, ainsi que configurer les différents moyens que les variables peuvent être automatiquement analysés à partir de la chaîne de requête/URL et passés à la méthode en tant que paramètre arguments. Il offre la flexibilité nécessaire pour totalement optimiser un site pour SEO (optimisation du moteur de recherche), ainsi que publier toute structure d’URL que nous voulons à partir d’une application.

Par défaut, les nouveaux projets ASP.NET MVC sont fournis avec un ensemble préconfiguré de règles de routage d’URL déjà inscrit. Cela nous permet facilement commencer sur une application sans devoir explicitement configurer quoi que ce soit. Les inscriptions de règle de routage par défaut peuvent se trouver dans la classe « Application » de nos projets – que nous pouvons ouvrir en double-cliquant sur le fichier « Global.asax » à la racine de notre projet :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

Les règles de routage ASP.NET MVC par défaut sont enregistrés dans la méthode « RegisterRoutes » de cette classe :

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

Les itinéraires ». MapRoute() « appel de la méthode ci-dessus inscrit une règle de routage par défaut qui mappe des URL entrantes aux classes de contrôleur à l’aide du format d’URL : « / {controller} / {action} / {id} », où « controller » est le nom de la classe de contrôleur pour instancier, « action » est le nom d’un méthode publique à appeler sur et « id » est un paramètre facultatif est incorporé dans l’URL qui peut être passé en tant qu’argument à la méthode. Le troisième paramètre transmis à l’appel de méthode « MapRoute() » est un ensemble de valeurs par défaut à utiliser pour les valeurs de contrôleur / / id d’action dans le cas où ils ne sont pas présents dans l’URL (contrôleur = « Home », Action = « Index », Id = » »).

Voici une table qui montre comment une variété d’URL sont mappés à l’aide de la valeur par défaut «<em>/ {contrôleurs} / {action} / {id} »</em>règle de routage :

| **URL** | **Classe de contrôleur** | **Méthode d’action** | **Paramètres passés** |
| --- | --- | --- | --- |
| */ Dîners/détails/2* | DinnersController | Details(ID) | ID = 2 |
| */ Dîners/Edit/5* | DinnersController | Edit(ID) | ID = 5 |
| */Dinners/Create* | DinnersController | Create() | N/A |
| */ Dîners* | DinnersController | Index() | N/A |
| */ Édition familiale* | HomeController | Index() | N/A |
| */* | HomeController | Index() | N/A |

Les trois dernières lignes indiquent les valeurs par défaut (contrôleur = Home, Action = Index, Id = « ») qui est utilisé. Étant donné que la méthode « Index » est enregistrée en tant que le nom d’action par défaut si aucun n’est pas spécifié, le « / dîners » et « / Home » URL cause la méthode d’action Index() à appeler sur leurs classes de contrôleur. Étant donné que le contrôleur « Home » est inscrit en tant que le contrôleur par défaut s’il n’est pas spécifié, l’URL « / » entraîne le HomeController doit être créé et la méthode d’action Index() dessus à appeler.

Si vous n’aimez pas ces règles de routage d’URL par défaut, la bonne nouvelle est qu’ils sont faciles à modifier / simplement de les modifier dans la méthode RegisterRoutes ci-dessus. Pour notre application NerdDinner, cependant, nous n’allons pas modifier les règles de routage d’URL par défaut : au lieu de cela nous utilisons simplement en tant que-est.

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>À l’aide de la DinnerRepository à partir de notre DinnersController

Nous allons maintenant remplacer notre implémentation actuelle de la DinnersController Index() et Details() des méthodes d’action avec des implémentations qui utilisent notre modèle.

Nous allons utiliser la classe DinnerRepository que nous avons créé précédemment pour implémenter le comportement. Nous allons commencer en ajoutant une instruction « using » qui fait référence à l’espace de noms « NerdDinner.Models » et ensuite déclarer une instance de notre DinnerRepository en tant que champ dans notre classe DinnerController.

Plus loin dans ce chapitre, nous allons introduisent le concept de « Injection de dépendances » et afficher une autre façon afin d’obtenir une référence à un DinnerRepository qui permet une meilleure unité à nos contrôleurs de test – mais pour droite maintenant nous allons simplement créer une instance de notre DinnerRepository inline comme ci-dessous.

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

Nous sommes désormais prêts générer une réponse HTML à l’aide de nos objets de modèle de données récupérées.

### <a name="using-views-with-our-controller"></a>Utilisation des vues avec notre contrôleur

Bien qu’il soit possible d’écrire du code dans nos méthodes d’action pour assembler le HTML, puis utiliser le *Response.Write ()* méthode d’assistance pour l’envoyer au client, qu’approche devient rapidement assez difficile à gérer. Une approche bien meilleure consiste pour que nous puissions effectuer uniquement application et la logique de données à l’intérieur de nos méthodes d’action DinnersController et puis transférer les données nécessaires pour afficher une réponse HTML à un modèle distinct « vue » qui est responsable de la sortie de la représentation sous forme de HTML de celui-ci. Comme nous le verrons dans un moment, un modèle de « afficher » est un fichier texte qui contienne généralement une combinaison de balisage HTML et le code de rendu incorporé.

Séparation notre logique du contrôleur à partir de notre rendu de la vue offre plusieurs avantages big. En particulier il permet d’appliquer une séparation « claire des préoccupations » entre le code d’application et le code de mise en forme/rendu de l’interface utilisateur. Cela facilite grandement test d’unité logique d’application de manière isolée à partir de la logique de rendu de l’interface utilisateur. Elle facilite la modifier ultérieurement les modèles de rendu de l’interface utilisateur sans avoir à apporter des modifications de code d’application. Et il peut rendre plus facile pour les développeurs et concepteurs de collaborer sur des projets.

Nous pouvons mettre à jour notre classe DinnersController pour indiquer que nous souhaitons utiliser un modèle de vue pour renvoyer une réponse de l’interface utilisateur HTML en modifiant les signatures de méthode nos deux de méthodes d’action d’avoir un type de retour de « void » au lieu de cela ayant un type de retour de « ActionResult ». Nous pouvons appeler ensuite la *View()* méthode d’assistance sur la classe de base de contrôleur pour retourner un objet « ViewResult » comme ci-dessous :

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

La signature de la *View()* méthode d’assistance que nous utilisons ci-dessus se présente comme suit :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

Le premier paramètre de la *View()* méthode d’assistance est le nom du fichier de modèle de vue nous voulons à utiliser pour restituer la réponse HTML. Le deuxième paramètre est un objet de modèle qui contient les données dont le modèle de vue a besoin pour afficher la réponse HTML.

Dans notre méthode d’action Index(), nous appelons le *View()* méthode d’assistance et en indiquant que nous souhaitons afficher un listing de code HTML de dîners à l’aide d’un modèle d’affichage de « Index ». Nous passons une séquence d’objets dîner pour générer la liste à partir du modèle d’affichage :

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

Dans notre méthode d’action Details() nous tentons de récupérer un objet dîner à l’aide de l’id fourni dans l’URL. Si un dîner valid est trouvé, nous appelons le *View()* méthode d’assistance, indiquant nous souhaitons utiliser un modèle de vue « Details » pour restituer l’objet dîner récupéré. Si un dîner non valide est demandée, nous affichons un message d’erreur utiles qui indique que le dîner n’existe pas à l’aide d’un modèle de vue « NotFound » (et une version surchargée de la *View()* méthode d’assistance qui prend simplement le nom du modèle ):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

Nous allons maintenant implémenter les modèles de vue « NotFound », « Détails » et « Index ».

### <a name="implementing-the-notfound-view-template"></a>Implémenter le modèle de vue « NotFound »

Nous allons commencer en implémentant le modèle de vue « NotFound », qui affiche un message d’erreur convivial indiquant que le dîner demandé est introuvable.

Nous allons créer un nouveau modèle de vue en positionnant le curseur de notre texte au sein d’une méthode d’action de contrôleur, puis avec le bouton droit cliquez et choisissez la commande de menu « Ajouter une vue » (nous pouvons également exécuter cette commande en tapant Ctrl-M, Ctrl + V) :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

Cela fera apparaître une boîte de dialogue « Ajouter une vue », comme ci-dessous. Par défaut, que la boîte de dialogue préremplir le nom de la vue à créer pour correspondre au nom de la méthode d’action le curseur a été dans lorsque la boîte de dialogue a été lancée (dans ce cas, « Détails »). Parce que nous souhaitons tout d’abord implémenter le modèle « NotFound », nous allons remplacer ce nom de la vue et définissez-le à la place « NotFound » :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

Lorsque nous cliquons sur le bouton « Ajouter », Visual Studio crée un nouveau modèle de vue « NotFound.aspx » pour nous dans le répertoire « \Views\Dinners » (qui sera également créé si le répertoire n’existe pas déjà) :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

Il ouvre également notre nouveau modèle de vue de « NotFound.aspx » dans l’éditeur de code :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

Afficher les modèles par défaut ont deux « zones » où nous pouvons ajouter le contenu et le code. La première permet de personnaliser la « title » de la page HTML renvoyé. Le second permet de personnaliser le « contenu principal » de la page HTML renvoyé.

Pour implémenter notre modèle de vue « NotFound », nous allons ajouter du contenu de base :

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

Nous pouvons puis faites un essai dans le navigateur. Pour cela nous allons demande le *« / dîners/détails/9999 »* URL. Cela fait référence à un dîner qui n’existe pas actuellement dans la base de données et entraîne notre méthode d’action DinnersController.Details() restituer de notre modèle de vue « NotFound » :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

Vous remarquerez que dans la capture d’écran ci-dessus est que notre modèle de vue de base a hérité d’un ensemble de code HTML qui entoure le contenu sur l’écran principal. Il s’agit, car notre modèle de vue utilise un modèle « page maître » qui nous permet d’appliquer une disposition cohérente sur tous les affichages sur le site. Nous verrons comment des pages maîtres plus dans une partie ultérieure de ce didacticiel.

### <a name="implementing-the-details-view-template"></a>Implémenter le modèle de vue « Détails »

Nous allons maintenant implémenter le modèle de vue « Détails » – qui génère du code HTML pour un seul modèle dîner.

Nous allons cela en plaçant notre curseur de texte au sein de la méthode d’action plus d’informations, puis avec le bouton droit sur et choisissez la commande de menu « Ajouter une vue » (ou appuyez sur Ctrl-M, Ctrl + V) :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

Cela fera apparaître la boîte de dialogue « Ajouter une vue ». Nous allons conserver le nom de la vue par défaut (« détails »). Nous allons également sélectionner la case à cocher « Créer une vue fortement typée » dans la boîte de dialogue et sélectionnez (à l’aide de la liste déroulante de combobox) le nom du type de modèle que nous passons à partir du contrôleur à la vue. Pour cette vue, nous passons un objet dîner (le nom qualifié complet de ce type est : « NerdDinner.Models.Dinner ») :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

Contrairement au modèle précédent, où nous avons choisi de créer une « vue vide », cette fois que nous choisissons d’automatiquement « structurer » la vue à l’aide d’un modèle « Details ». Nous pouvons l’indiquer en modifiant la liste déroulante « Afficher le contenu » dans la boîte de dialogue ci-dessus.

« Génération de modèles automatique » génère une implémentation initiale de notre modèle de vue de détails basé sur l’objet dîner que nous passons à ce dernier. Cela fournit un moyen simple pour nous permettre de commencer rapidement sur notre implémentation de modèle de vue.

Lorsque nous cliquons sur le bouton « Ajouter », Visual Studio crée un nouveau fichier de modèle de vue « Details.aspx » pour nous dans notre annuaire de « \Views\Dinners » :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

Il ouvre également notre nouveau modèle de vue « Details.aspx » dans l’éditeur de code. Il contient une implémentation de la structure initiale d’une vue de détails basée sur un modèle de dîner. Le moteur de génération de modèles automatique utilise la réflexion .NET pour examiner les propriétés publiques exposées sur la classe transmise et ajoutera le contenu approprié en fonction de chaque type qu’il trouve :

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

Nous pouvons demander le *« / dîners/détails/1 »* URL pour voir à quoi ressemble cette implémentation d’une structure « details » dans le navigateur. À l’aide de cette URL affiche l’un de le dîners que nous avons ajouté manuellement à notre base de données lorsque nous avons tout d’abord créé il :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

Cela nous obtient rapidement opérationnel et nous fournit une implémentation initiale de notre fichier details.aspx. Nous pouvons ensuite accéder et le modifier pour personnaliser l’interface utilisateur à notre satisfaction.

Lorsque nous examinerons plus en détail le modèle Details.aspx, nous allons trouvé qu’il contient le code HTML statique ainsi que rendu code incorporé. &lt;%%&gt; pépites de code d’exécuter du code lorsque le modèle de vue est rendu, et &lt;% = %&gt; pépites de code exécute le code qu’ils contiennent, puis pour restituer le résultat dans le flux de sortie du modèle.

Nous pouvons écrire du code dans notre vue qui accède à l’objet de modèle « Dîner » qui a été passé à partir de notre contrôleur à l’aide d’une propriété fortement typée de « Modèle ». Visual Studio nous fournit avec code-fonctionnalités intellisense complètes lors de l’accès à cette propriété « Model » dans l’éditeur :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

Nous allons apporter quelques ajustements afin que la source de notre modèle de vue Détails finale se présente comme suit :

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

Lorsque nous accédez à la *« / dîners/détails/1 »* URL à nouveau qu’il va maintenant rendu comme ci-dessous :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>Implémentation du modèle d’affichage « Index »

Nous allons maintenant implémenter le modèle de vue « Index » – ce qui génèrera une liste de dîners à venir. Action, nous allons positionner notre curseur de texte au sein de la méthode d’action Index et ensuite avec le bouton droit cliquez sur et choisissez la commande de menu « Ajouter une vue » (ou appuyez sur Ctrl-M, Ctrl + V).

Dans la boîte de dialogue « Ajouter une vue » nous conserver le modèle de vue nommé « Index » et sélectionnez la case à cocher « Créer une vue fortement typée ». Cette fois, nous allons choisir pour automatiquement générer un modèle de vue « Liste », sélectionnez « NerdDinner.Models.Dinner » comme type de modèle passé à la vue (qui, car nous avons indiqué, nous allons créer une structure entraîne la boîte de dialogue Ajouter une vue Supposons que nous sommes « liste » en transmettant une séquence d’objets de dîner à partir de notre contrôleur à la vue) :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

Lorsque nous cliquons sur le bouton « Ajouter », Visual Studio crée un nouveau fichier de modèle de vue « Index.aspx » pour nous dans notre annuaire de « \Views\Dinners ». Il sera « générer automatiquement » une implémentation initiale qu’il contient qui fournit une liste de table HTML des dîners nous passons à la vue.

Lorsque nous exécutons l’accès aux applications et le *« / dîners / »* URL il affichera notre liste de dîners comme suit :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

La solution de la table ci-dessus nous donne une mise en page de grille de nos données dîner : qui n’est pas tout à fait ce que nous souhaitons pour notre consommateur accessible sur la liste Dinner. Nous pouvons mettre à jour le modèle de vue Index.aspx et modifiez-le pour répertorier le moins de colonnes de données et utiliser un &lt;ul&gt; élément afin de les restituer au lieu d’une table en utilisant le code ci-dessous :

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

Nous utilisons le mot clé « var » au sein de l’instruction foreach ci-dessus comme nous une boucle sur chaque dîner dans notre modèle. Ceux êtes pas familiarisé avec c# 3.0 peuvent penser qu’à l’aide de « var » signifie que l’objet dîner est à liaison tardive. Cela signifie plutôt que le compilateur utilise l’inférence de type par rapport à la propriété fortement typée « Model » (qui est de type « IEnumerable&lt;dîner&gt;») et la compilation de la variable locale « dîner » en tant que Dinner type – ce qui signifie que nous obtenons complètes IntelliSense et le moment de la compilation vérifiant dans les blocs de code :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

Lorsque nous actualisons le */Dinners* URL dans notre navigateur notre affichage mis à jour se présente comme ci-dessous :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

Cela vise à mieux – mais entièrement n’y figure pas encore. Notre dernière étape consiste à permettre aux utilisateurs de cliquer sur dîners individuels dans la liste et d’afficher les détails les concernant. Nous allons implémenter cela par le rendu des éléments de lien hypertexte HTML qui sont liées à la méthode d’action de détails sur notre DinnersController.

Nous pouvons générer ces liens hypertexte dans notre vue Index de deux manières. La première consiste à créer manuellement un fichier HTML &lt;un&gt; éléments tels que ci-dessous, où nous incorporons &lt;%%&gt; bloque dans les &lt;un&gt; élément HTML :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

Une autre approche que nous pouvons utiliser consiste à tirer parti de la méthode d’assistance de « Html.ActionLink() » intégrée dans ASP.NET MVC qui prend en charge la création d’un élément HTML par programmation &lt;un&gt; élément qui établit un lien vers une autre méthode d’action sur un Contrôleur :

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

Le premier paramètre à la méthode d’assistance Html.ActionLink() est le texte du lien pour afficher (dans ce cas le titre du dîner), le deuxième paramètre est le nom d’action de contrôleur que nous voulons générer le lien pour (dans ce cas, la méthode de détails), et le troisième paramètre est un ensemble de paramètres à envoyer à l’action (implémentée comme un type anonyme avec nom/valeurs de propriété). Dans ce cas, nous allons spécifier le paramètre « id » du dîner nous à lier à et étant donné que le routage d’URL par défaut de règle dans ASP.NET MVC est « {Controller} / {Action} / {id} » la méthode d’assistance Html.ActionLink() génère la sortie suivante :

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

Pour notre vue Index.aspx, nous allons utiliser l’approche de méthode d’assistance Html.ActionLink() et avoir chaque dîner dans le lien de la liste à l’URL de détails appropriés :

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

Et maintenant, lorsque nous atteignons les */Dinners* URL notre liste dîner se présente comme suit :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

Lorsque nous cliquons sur un de le dîners dans la liste, nous allons naviguer pour afficher les détails :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>Basé sur une convention d’affectation de noms et la structure de répertoires \Views

Les applications ASP.NET MVC par défaut utilisent un répertoire basée sur une convention d’affectation de noms structure lors de la résolution des modèles de vue. Cela permet aux développeurs d’éviter de devoir qualifier complètement un chemin de localisation lors du référencement des vues à partir d’une classe de contrôleur. Par défaut, ASP.NET MVC recherchera le fichier de modèle de vue dans le * \Views\[Nom_contrôleur]\* répertoire en dessous de l’application.

Par exemple, nous avons travaillé sur la classe DinnersController – qui fait explicitement référence à trois modèles de vue : « Index », « Détails » et « NotFound ». ASP.NET MVC par défaut recherchera ces vues dans le *\Views\Dinners* répertoire sous le répertoire racine de notre application :

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

Notez que ci-dessus comment il existe actuellement trois classes de contrôleur dans le projet (DinnersController, HomeController et AccountController – les deux dernières ont été ajoutées par défaut lorsque nous avons créé le projet), et il existe trois sous-répertoires (un pour chaque contrôleur) dans le répertoire \Views.

Vues référencées à partir des contrôleurs accueil et comptes seront résolu automatiquement leurs modèles de vue de respectifs *\Views\Home* et *\Views\Account* répertoires. Le *\Views\Shared* sous-répertoire offre un moyen de stocker les modèles de vue qui sont réutilisées sur plusieurs contrôleurs au sein de l’application. Lorsque ASP.NET MVC tente de résoudre un modèle de vue, il vérifie d’abord dans le *\Views\[contrôleur]* répertoire spécifique, et s’il ne trouve pas le modèle de vue il il examinera le *\Views\ Partagé* directory.

Lorsqu’il s’agit de modèles de vue individuelle d’affectation de noms, la recommandation recommandée consiste à avoir le modèle de vue de partager le même nom que la méthode d’action qui a entraîné son rendu. Par exemple, notre « Index » au-dessus de méthode d’action est à l’aide de la vue « Index » pour afficher le résultat de la vue, et la méthode d’action « Détails » est à l’aide de la vue « Détails » pour afficher ses résultats. Cela facilite la rapidement déterminer quel modèle est associé à chaque action.

Les développeurs n’avez pas besoin de spécifier explicitement le nom de modèle de vue lorsque le modèle de vue a le même nom que la méthode d’action invoquée sur le contrôleur. Nous pouvons à la place simplement transmettre l’objet de modèle à la méthode d’assistance « View() » (sans spécifier le nom de la vue) et ASP.NET MVC déduira automatiquement que nous souhaitons utiliser la *\Views\[Nom_contrôleur]\[ActionName]* afficher le modèle sur le disque pour l’afficher.

Cela nous permet de nettoyer un peu de notre code de contrôleur, et d’éviter de dupliquer le nom à deux reprises dans notre code :

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

Le code ci-dessus est que tout ce qui est nécessaire pour implémenter une agréable dîner/détails de l’annonce une expérience pour le site.

#### <a name="next-step"></a>Étape suivante

Nous disposons désormais d’un dîner agréable expérience intégrée de navigation.

Nous allons maintenant activer la prise en charge d’édition de formulaires de données CRUD (Create, Read, Update, Delete).

> [!div class="step-by-step"]
> [Précédent](build-a-model-with-business-rule-validations.md)
> [Suivant](provide-crud-create-read-update-delete-data-form-entry-support.md)
