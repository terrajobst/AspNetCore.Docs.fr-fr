---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: Utiliser ViewData et implémenter des Classes ViewModel | Microsoft Docs
author: microsoft
description: Étape 6 montre comment activer la prise en charge des scénarios, d’édition de formulaires plus riche et aborde également les deux approches qui peuvent être utilisées pour passer des données à partir des contrôleurs aux vues :...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: edc62246cdc5e5df51c369a70b47dab92c9ecc1c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365121"
---
<a name="use-viewdata-and-implement-viewmodel-classes"></a>Utiliser ViewData et implémenter des Classes ViewModel
====================
par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit d’étape 6 a gratuit [« « l’application NerdDinner](introducing-the-nerddinner-tutorial.md) qui présente en détail comment créer un petit mais terminé, l’application web à l’aide d’ASP.NET MVC 1.
> 
> Étape 6 montre comment activer la prise en charge des scénarios, d’édition de formulaires plus riche et aborde également les deux approches qui peuvent être utilisées pour passer des données à partir des contrôleurs aux vues : ViewData et le ViewModel.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre le [mise en route avec MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de musique MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) didacticiels.


## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>NerdDinner étape 6 : ViewData et le ViewModel

Nous avons abordé plusieurs scénarios de publication de formulaire et vous avez appris à implémenter créer, mettre à jour et supprimer la prise en charge (CRUD). Nous allons maintenant aller plus loin dans notre implémentation DinnersController et activer la prise en charge des scénarios d’édition de formulaires plus riche. Ce faisant, nous allons aborder deux approches qui peuvent être utilisées pour passer des données à partir des contrôleurs aux vues : ViewData et le ViewModel.

### <a name="passing-data-from-controllers-to-view-templates"></a>Passage de données à partir des contrôleurs aux modèles de vue

Une des caractéristiques qui définissent le modèle MVC est la stricte « séparation des préoccupations » il permet d’appliquer entre les différents composants d’une application. Modèles, contrôleurs et vues chacun ont bien défini de rôles et responsabilités, et ils communiquent entre eux de manières bien défini. Cela permet de promouvoir la testabilité et la réutilisation du code.

Quand une classe de contrôleur décide de restituer une réponse HTML à un client, il est responsable de leur transmettant explicitement pour le modèle d’affichage de toutes les données nécessaires pour afficher la réponse. Afficher les modèles ne doivent jamais exécuter toute logique d’application ou récupération de données – et doivent se pour disposer uniquement le code de rendu qui est basée sur les modèle/des données transmises par le contrôleur de limiter à la place.

Dès maintenant les données du modèle passé par notre DinnersController classe nos modèles de vue est simple et directe : une liste d’objets dîner dans le cas Index() et un dîner unique de l’objet dans le cas Details(), Edit(), Create() et Delete(). Comme nous ajoutons des fonctionnalités d’interface utilisateur plus à notre application, nous allons souvent besoin de passer plus que ces données pour restituer des réponses HTML au sein de nos modèles de vue. Par exemple, nous pourrions modifier le champ « Pays » au sein de notre modifier et créer des vues d’en cours d’une zone de texte HTML à un contrôle dropdownlist. Au lieu de coder en dur la liste déroulante des noms de pays dans le modèle de vue, nous pourrions générer à partir d’une liste de pays pris en charge qui nous remplir dynamiquement. Nous aurons besoin d’un moyen de passer à la fois l’objet dîner *et* la liste des pays pris en charge à partir de notre contrôleur à nos modèles de vue.

Nous allons étudier deux façons, que nous pouvons effectuer cette opération.

### <a name="using-the-viewdata-dictionary"></a>À l’aide du dictionnaire ViewData

La classe de base du contrôleur expose une propriété de dictionnaire « ViewData » qui peut être utilisée pour transmettre des éléments de données à partir des contrôleurs aux vues.

Par exemple, pour prendre en charge le scénario où nous voulons modifier la zone de texte « Pays » au sein de notre vue d’édition ne soient pas d’une zone de texte HTML à un contrôle dropdownlist, nous pouvons mettre à jour notre méthode d’action Edit() pour transmettre (en plus d’un objet dîner) un objet SelectList qui peut être utilisé comme le m odèle de dropdownlist pays.

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

Le constructeur de la SelectList ci-dessus est acceptant une liste de comtés pour remplir la liste-downlist avec, ainsi que la valeur actuellement sélectionnée.

Nous pouvons ensuite mettre à jour notre modèle de vue Edit.aspx pour utiliser la méthode d’assistance Html.DropDownList() au lieu de la méthode d’assistance Html.TextBox() que nous avons utilisé précédemment :

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

La méthode d’assistance Html.DropDownList() ci-dessus prend deux paramètres. Le premier est le nom de l’élément de formulaire HTML en sortie. Le second est le modèle « SelectList » que nous avons transmis via le dictionnaire ViewData. Nous utilisons le C# mot clé « sous » un cast du type dans le dictionnaire en tant qu’un SelectList.

Et maintenant quand nous exécutons notre accès aux applications et le */Dinners/Edit/1* URL dans notre navigateur, nous allons voir que notre interface utilisateur de modification a été mis à jour pour afficher un contrôle dropdownlist de pays au lieu d’une zone de texte :

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

Étant donné que nous affichons également le modèle de vue de modification à partir de la méthode HTTP POST modifier (dans les scénarios de cas d’erreur), nous devrons pour vous assurer que nous avons également mettre à jour cette méthode pour ajouter le SelectList à ViewData quand le modèle de vue est affiché dans les scénarios d’erreur :

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

Et maintenant notre scénario de modification DinnersController prend en charge un contrôle DropDownList.

### <a name="using-a-viewmodel-pattern"></a>À l’aide d’un modèle ViewModel

L’approche de dictionnaire ViewData présente l’avantage d’être relativement rapide et facile à implémenter. Certains développeurs n’aiment pas à l’aide de dictionnaires basés sur chaîne, cependant, dans la mesure où les fautes de frappe peuvent entraîner des erreurs qui ne sont pas interceptées au moment de la compilation. Le dictionnaire ViewData non typé nécessite également à l’aide de l’opérateur « as » ou de conversion lors de l’utilisation d’un langage fortement typé tel que c# dans un modèle de vue.

Une autre approche que nous pourrions utiliser est un est communément appelée le modèle « ViewModel ». Lorsque vous utilisez ce modèle, que nous créons des classes fortement typées qui sont optimisés pour nos scénarios vue spécifique, et qui exposent des propriétés pour le valeurs/le contenu dynamique requises par nos modèles de vue. Nos classes de contrôleur peuvent remplir, puis transmettre ces classes d’affichage optimisé à notre modèle de vue à utiliser. Cela permet la sécurité de type, la vérification au moment de la compilation et intellisense de l’éditeur au sein des modèles de vue.

Par exemple, pour activer le formulaire dîner éditions scénarios que nous pouvons créer un « DinnerFormViewModel » classe comme ci-dessous qui expose deux propriétés fortement typées : un objet dîner et le modèle de SelectList requis pour remplir la liste dropdownlist pays :

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

Nous pouvons puis mettez à jour notre méthode d’action Edit() pour créer le DinnerFormViewModel à l’aide de l’objet dîner que nous récupérons à partir de notre référentiel et puis la transmettons à notre modèle de vue :

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

Nous allons ensuite mise à jour de notre modèle de vue afin qu’elle attend un « DinnerFormViewModel » au lieu d’un « dîner » de l’objet en modifiant l’attribut « hérite » en haut de la page edit.aspx comme suit :

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

Une fois que nous faisons cela, l’intellisense de la propriété « Modèle » au sein de notre modèle de vue sera mis à jour pour refléter le modèle objet du type DinnerFormViewModel que nous passons il :

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

Nous pouvons ensuite mettre à jour notre code de vue pour travailler dessus. Comme indiqué ci-dessous, comment nous modifions pas les noms des éléments d’entrée, nous allons créer (les éléments de formulaire seront toujours être nommés « Title », « Pays ») – mais nous mettons à jour les méthodes d’assistance HTML pour récupérer les valeurs à l’aide de la classe DinnerFormViewModel :

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

Nous allons également mettre à jour notre méthode post de modification pour utiliser la classe DinnerFormViewModel lors du rendu des erreurs :

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

Nous pouvons également mettre à jour nos méthodes d’action Create() à nouveau utiliser exactement le même *DinnerFormViewModel* classe pour activer les pays de DropDownList dans ces derniers. Voici l’implémentation HTTP-GET :

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

Voici l’implémentation de la méthode HTTP POST créer :

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

Et maintenant notre écrans de modification et de création prend en charge des listes déroulantes pour sélectionner le pays.

### <a name="custom-shaped-viewmodel-classes"></a>Classes de ViewModel en forme personnalisée

Dans le scénario ci-dessus, notre classe DinnerFormViewModel expose directement l’objet de modèle dîner en tant que propriété, ainsi que d’une propriété de modèle SelectList prise en charge. Cette approche fonctionne bien pour les scénarios où l’interface utilisateur HTML que nous souhaitons créer au sein de notre modèle de vue correspond relativement à nos objets de modèle de domaine.

Pour les scénarios où cela n’est pas le cas, une option que vous pouvez utiliser est de créer une classe ViewModel en forme personnalisée dont le modèle objet est plus optimisé pour la consommation par la vue – et ce qui pourrait donner complètement différent de l’objet de modèle de domaine sous-jacent. Par exemple, elle peut potentiellement exposer différents noms de propriétés et/ou les propriétés d’agrégat collectées à partir de plusieurs objets de modèle.

Classes de ViewModel en forme personnalisée peuvent être utilisé à la fois pour passer des données à partir des contrôleurs aux vues à restituer, ainsi que pour aider à gérer les données de formulaire publiées sur la méthode d’action d’un contrôleur. Pour ce scénario ultérieur, vous pouvez avoir la méthode d’action mettre à jour un objet ViewModel avec les données de formulaire publié et ensuite utiliser l’instance ViewModel pour mapper ou récupérer un objet de modèle de domaine réel.

En forme personnalisée des classes ViewModel peuvent fournir une très grande souplesse et sont quelque chose à examiner n’importe quel moment vous trouverez le code de rendu dans vos modèles de vue ou le code de validation de formulaire à l’intérieur de vos méthodes d’action obtiennent trop compliqué. C’est souvent le signe que vos modèles de domaine ne correspondent pas correctement à l’interface utilisateur que vous générez et qu’une classe de ViewModel en forme personnalisée intermédiaire peut aider.

### <a name="next-step"></a>Étape suivante

Voyons maintenant comment nous pouvons utiliser des vues partielles et les pages maîtres pour réutiliser et partager l’interface utilisateur dans notre application.

> [!div class="step-by-step"]
> [Précédent](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [Suivant](re-use-ui-using-master-pages-and-partials.md)
