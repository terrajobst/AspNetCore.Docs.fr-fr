---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: Utiliser AJAX pour implémenter les scénarios de mappage | Microsoft Docs
author: microsoft
description: Étape 11 montre comment intégrer la prise en charge du mappage AJAX dans notre application NerdDinner, permettant aux utilisateurs qui créent, modification ou affichage dîners pour voir la lettre l...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: f8b9704e966c0211a690156555f4a272a823023a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825977"
---
<a name="use-ajax-to-implement-mapping-scenarios"></a>Utiliser AJAX pour implémenter les scénarios de mappage
====================
par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit d’étape 11 a gratuit [« « l’application NerdDinner](introducing-the-nerddinner-tutorial.md) qui présente en détail comment créer un petit mais terminé, l’application web à l’aide d’ASP.NET MVC 1.
> 
> Étape 11 montre comment intégrer la prise en charge du mappage AJAX dans notre application NerdDinner, permettant aux utilisateurs qui créent, modification ou affichage dîners pour voir l’emplacement du dîner graphiquement.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre le [mise en route avec MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de musique MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) didacticiels.


## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner étape 11 : L’intégration d’une carte d’AJAX

Nous allons maintenant rendre notre application un peu plus visuellement attrayante en intégrant la prise en charge du mappage AJAX. Cela permet aux utilisateurs qui créent, modification ou affichage dîners pour voir l’emplacement du dîner graphiquement.

### <a name="creating-a-map-partial-view"></a>Création d’une vue partielle de carte

Nous allons utiliser la fonctionnalité de mappage à plusieurs endroits au sein de notre application. Pour que notre code sec, nous allons encapsuler les fonctionnalités communes de la carte dans un seul modèle partielle qui nous pouvons réutiliser dans plusieurs actions de contrôleur et les vues. Nous nommez cette vue partielle « map.ascx » et le créer dans le répertoire \Views\Dinners.

Nous pouvons créer la map.ascx partielle en cliquant sur le répertoire \Views\Dinners et en choisissant Add -&gt;afficher la commande de menu. Nous allons nommer la vue « Map.ascx », archivez-le comme une vue partielle et indiquer que nous allons passer une classe de modèle fortement typé « dîner » :

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

Notre modèle partielle est créé lorsque nous cliquons sur le bouton « Ajouter ». Nous allons ensuite mettre à jour le fichier Map.ascx pour que le contenu suivant :

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

La première &lt;script&gt; points de référence pour la bibliothèque de mappage Microsoft Virtual Earth 6.2. La seconde &lt;script&gt; référence pointe vers un fichier map.js que nous allons créer peu de temps qui doit encapsuler notre logique de mappage Javascript commune. Le &lt;div id = « theMap »&gt; élément est le conteneur HTML que Virtual Earth utilisera pour héberger la carte.

Nous avons ensuite incorporé &lt;script&gt; bloc qui contient deux fonctions JavaScript spécifiques à cette vue. La première fonction utilise jQuery pour AutoEventWireup une fonction qui s’exécute lorsque la page est prête à exécuter un script côté client. Il appelle une fonction d’assistance de LoadMap() nous définirons dans notre fichier de script Map.js pour charger le contrôle de carte virtual earth. La deuxième fonction est un gestionnaire d’événements de rappel qui ajoute un code pin à la carte qui identifie un emplacement.

Notez la façon dont nous utilisons une côté serveur &lt;% = %&gt; bloc dans le bloc de script côté client pour incorporer la latitude et la longitude du dîner doivent intervenir dans le code JavaScript. Il s’agit d’une technique utile pour générer des valeurs dynamiques qui peuvent être utilisés par un script côté client (sans nécessiter un AJAX appel distinct sur le serveur pour récupérer les valeurs – ce qui le rend plus rapidement). Le &lt;% = %&gt; blocs doit être exécutée lorsque la vue est rendu sur le serveur, et par conséquent, la sortie du code HTML sera finir avec les valeurs de JavaScript incorporés (par exemple : var latitude = 47.64312 ;).

### <a name="creating-a-mapjs-utility-library"></a>Création d’une bibliothèque d’utilitaire Map.js

Nous allons maintenant créer le fichier Map.js que nous pouvons utiliser pour encapsuler les fonctionnalités de JavaScript pour notre carte (et implémenter les méthodes LoadMap et LoadPin ci-dessus). Nous pouvons faire cela en cliquant sur le répertoire \Scripts au sein de notre projet, puis choisissez le « Add -&gt;un nouvel élément « commande de menu, sélectionnez l’élément de JScript et nommez-le « Map.js ».

Voici le code JavaScript que nous allons ajouter au fichier Map.js qui interagit avec Virtual Earth pour afficher notre carte et de lui ajouter des codes confidentiels d’emplacements pour notre dîners :

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>L’intégration de la carte à créer et les formulaires de modification

Vous intègrerez désormais la prise en charge de la carte avec nos scénarios Create et Edit existants. La bonne nouvelle est que cela est une tâche assez facile et ne nous amener à modifier l’un de nos code du contrôleur. Étant donné que notre vues Create et Edit partagent une vue partielle « DinnerForm » commune pour implémenter l’interface utilisateur du formulaire dîner, nous pouvons ajouter le mappage au même endroit et avez nos scénarios Create et Edit à utiliser.

Il nous suffit d’action consiste à ouvrir la vue partielle \Views\Dinners\DinnerForm.ascx et mettre à jour pour inclure notre nouveau mappage partielle. Voici l’aspect de la mise à jour DinnerForm une fois que la carte est ajoutée (Remarque : les éléments de formulaire HTML sont omis de l’extrait de code ci-dessous par souci de concision) :

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

Le DinnerForm partielle ci-dessus prend un objet de type « DinnerFormViewModel » comme type de modèle (car il a besoin à la fois un objet dîner, mais aussi un SelectList pour remplir l’objet dropdownlist de pays). Notre carte partielle doit simplement un objet de type « Dîner » comme type de modèle, et par conséquent, lorsque nous affichons la carte partielle nous allons simplement la dîner sous-propriété du DinnerFormViewModel en lui passant :

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

La fonction JavaScript que nous avons ajouté à la partielle utilise jQuery pour attacher un événement de « flou » dans la zone de texte « Address » HTML. Vous n’avez probablement entendu les onglets ou les événements « focus » qui se déclenchent lorsqu’un utilisateur clique dans une zone de texte. L’inverse est un événement de « flou » qui se déclenche lorsqu’un utilisateur quitte une zone de texte. Le Gestionnaire d’événements ci-dessus efface les valeurs de zone de texte de latitude et longitude lorsque cela se produit et représente le nouvel emplacement de l’adresse sur notre carte. Un gestionnaire d’événements de rappel que nous avons défini dans le fichier map.js met ensuite à jour les zones de texte sur notre formulaire à l’aide des valeurs retournées par virtual earth basé sur l’adresse que nous lui avons longitude et latitude.

Et maintenant quand nous pouvons exécuter notre application à nouveau et que vous cliquez sur l’onglet « Dîner hôte » que nous verrons une valeur par défaut de mapper affichés ainsi que nos éléments de formulaire standards Dinner :

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

Lorsque nous Entrez une adresse et appuyez sur tab de suite, la carte met à jour dynamiquement pour afficher l’emplacement et notre gestionnaire d’événements remplira les zones de texte de latitude/longitude avec les valeurs d’emplacement :

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

Si nous enregistrer le nouveau dîner et ouvrez de nouveau pour la modification, nous allons trouvé que l’emplacement de la carte s’affiche lorsque la page se charge :

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

Chaque fois que le champ d’adresse est modifié, la carte et les coordonnées de latitude/longitude met à jour.

Maintenant que la carte affiche l’emplacement dîner, nous pouvons également modifier les champs de formulaire de Latitude et Longitude ne soient pas de zones de texte visible pour être à la place les éléments masqués (étant donné que la carte est automatiquement les mettre à jour chaque fois que vous entrez une adresse). Action cela nous allons passer de l’utilisation du programme d’assistance Html.TextBox() HTML à l’aide de la méthode d’assistance Html.Hidden() :

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

Et maintenant nos formulaires sont un peu plus conviviales et éviter l’affichage de la latitude/longitude brute (tout en stockant toujours avec chaque dîner dans la base de données) :

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>L’intégration de la carte dans la vue Détails

Maintenant que nous avons le mappage intégré à nos scénarios Create et Edit, nous allons également l’intégrer avec notre scénario de détails. Il nous suffit d’action consiste à appeler &lt;% Html.RenderPartial("map") ; %&gt; au sein de l’affichage des détails.

Voici à quoi ressemble le code source à la vue Détails complète (avec l’intégration de la carte) :

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

Et maintenant lorsqu’un utilisateur accède à une URL de /Dinners/détails / [id] ils voient plus d’informations sur le dîner, l’emplacement du dîner sur la carte (complet avec une punaise qu’au moment où dessus affiche le titre du dîner et l’adresse de celui-ci), et dispose d’un lien AJAX RSVP fo r il :

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>L’implémentation de la recherche de l’emplacement dans notre base de données et le référentiel

Pour terminer notre implémentation AJAX, nous allons ajouter un mappage à la page d’accueil de l’application qui permet aux utilisateurs de rechercher graphiquement dîners près d’eux.

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

Nous allons commencer en implémentant la prise en charge au sein de notre couche de référentiels et bases de données pour effectuer efficacement une recherche en fonction des emplacements de radius pour dîners. Nous pourrions utiliser le nouveau [fonctionnalités géospatiales de SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) pour implémenter cela, ou vous pouvez également nous pouvons utiliser une approche de fonction SQL Gary Dryden abordé dans l’article ici : [ http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx ](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) et Rob Conery créé un blog sur l’utilisation de LINQ to SQL ici : [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

Pour implémenter cette technique, nous sera ouvrir l’Explorateur de serveurs » » dans Visual Studio, sélectionnez la base de données NerdDinner, puis avec le bouton droit sur le sous-nœud « fonctions » dans cette section et choisir de créer une nouveau « fonction scalaire » :

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

Nous allons coller ensuite dans la fonction DistanceBetween suivante :

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

Nous allons ensuite créer une nouvelle fonction table incluse dans SQL Server, que nous appellerons « NearestDinners » :

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

Cette fonction de la table « NearestDinners » utilise la fonction d’assistance DistanceBetween pour retourner tous les dîners dans 100 miles de la latitude et longitude nous lui fournir :

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

Pour appeler cette fonction, nous allons tout d’abord ouvrir le concepteur LINQ to SQL en double-cliquant sur le fichier NerdDinner.dbml au sein de notre annuaire \Models :

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

Ensuite, nous allons faire glisser les fonctions NearestDinners et DistanceBetween sur LINQ pour concepteur SQL, ce qui entraîne à ajouter en tant que méthodes sur notre LINQ à SQL NerdDinnerDataContext classe :

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

Nous pouvons ensuite exposer une méthode de requête « FindByLocation » dans notre classe DinnerRepository qui utilise la fonction NearestDinner pour retourner à venir dîners qui se trouvent dans 100 miles de l’emplacement spécifié :

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>Implémentation d’une méthode d’Action de recherche AJAX basés sur JSON

Nous allons maintenant implémenter une méthode d’action de contrôleur qui tire parti de la nouvelle méthode de référentiel FindByLocation() pour retourner une liste de données dîner qui peuvent être utilisées pour remplir une carte. Nous aurons cette méthode d’action retournent les données Dinner au format JSON (JavaScript Object Notation) afin qu’il peut être facilement manipulé à l’aide de JavaScript sur le client.

Pour implémenter cela, nous allons créer une nouvelle classe « SearchController » en cliquant sur le répertoire \Controllers et en choisissant Add -&gt;commande de menu de contrôleur. Nous allons ensuite implémenter une méthode d’action « SearchByLocation » au sein de la nouvelle classe SearchController comme ci-dessous :

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

Méthode d’action de la SearchController SearchByLocation appelle en interne la méthode FindByLocation sur DinnerRespository pour obtenir une liste de dîners à proximité. Au lieu de renvoyer les objets dîner directement au client, cependant, il retourne à la place les objets JsonDinner. La classe JsonDinner expose un sous-ensemble des propriétés de dîner (par exemple : pour des raisons de sécurité qu’il ne divulguer les noms des personnes qui ont répondu pour un dîner). Il inclut également une propriété RSVPCount qui n’existe pas sur dîner – et qui est calculé dynamiquement en comptant le nombre d’objets RSVP associé à un dîner particulier.

Nous utilisons ensuite la méthode d’assistance Json() sur la classe de base du contrôleur retourne la séquence de dîners à l’aide d’un format de transmission basé sur JSON. JSON est un format de texte standard pour représenter des structures de données simples. Voici un exemple d’une liste au format JSON de deux objets JsonDinner aspect lorsque renvoyé à partir de notre méthode d’action :

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>Appel de la méthode AJAX basés sur JSON à l’aide de jQuery

Nous sommes maintenant prêts à mettre à jour de la page d’accueil de l’application NerdDinner à utiliser la méthode d’action de la SearchController SearchByLocation. Action, nous allons ouvrir le modèle de vue /Views/Home/Index.aspx et mettre à jour pour qu’il dispose d’une zone de texte, bouton de recherche, notre carte et un &lt;div&gt; élément nommé dinnerList :

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

Nous pouvons ensuite ajouter deux fonctions JavaScript à la page :

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

La première fonction JavaScript charge la carte lors du premier charge de la page. Les fils de fonction JavaScript deuxième d’un code JavaScript cliquez sur Gestionnaire d’événements sur le bouton de recherche. Lorsque le bouton est enfoncé, il appelle la fonction FindDinnersGivenLocation() JavaScript qui nous ajouterons à notre fichier Map.js :

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

Cette fonction FindDinnersGivenLocation() appelle la carte. Find() sur le contrôle de la terre virtuel pour centrer sur l’emplacement spécifié. Lorsque le service de carte virtual earth est retournée, la carte. Méthode Find() appelle la méthode de rappel callbackUpdateMapDinners que nous le passé comme argument final.

La méthode callbackUpdateMapDinners() est où le vrai travail est terminé. Il utilise la méthode d’assistance de $.post() de jQuery pour effectuer un appel AJAX de méthode d’action de notre SearchController SearchByLocation() – en lui passant la latitude et la longitude de la carte qui vient d’être centrée. Il définit une fonction inline qui sera appelée lorsque la méthode d’assistance $.post() est terminée et les résultats de dîner au format JSON renvoyés par la méthode d’action sera passée à l’aide d’une variable appelée « dîners » de SearchByLocation(). Il effectue une opération foreach via chaque dîner retourné et utilise la longitude et latitude du dîner et autres propriétés pour ajouter un nouveau code pin sur la carte. Il ajoute également une entrée de dîner à la liste HTML de dîners à droite de la carte. Il puis fils-up un événement de pointage pour les repères et la liste HTML afin que les détails concernant le dîner s’affichent lorsqu’un utilisateur pointe dessus :

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

Et maintenant quand nous pouvons exécuter l’application et que vous visitez la page d’accueil que nous sont proposées avec une carte. Lorsque nous Entrez le nom d’une ville la carte affiche les dîners à venir près d’elle :

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

Survol d’un dîner affichera les détails à ce sujet.

En cliquant sur le titre de dîner dans la bulle ou sur le côté droit dans la liste HTML permet d’accéder nous pour le dîner – ce qui nous pouvons ensuite éventuellement Inscrivez-vous au :

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>Étape suivante

Nous avons maintenant implémenté toutes les fonctionnalités d’application de notre application NerdDinner. Nous allons maintenant examiner comment nous pouvons activer automatisée unit test de celui-ci.

> [!div class="step-by-step"]
> [Précédent](use-ajax-to-deliver-dynamic-updates.md)
> [Suivant](enable-automated-unit-testing.md)
