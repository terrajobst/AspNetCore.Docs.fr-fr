---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
title: Validation avec l’Interface IDataErrorInfo (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther vous montre comment afficher des messages d’erreur de validation personnalisée en implémentant l’interface IDataErrorInfo dans une classe de modèle.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 3a8a9d9f-82dd-4959-b7c6-960e9ce95df1
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: e9b989d0110c3947583fd70bd38b29dcb2bb5c31
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825549"
---
<a name="validating-with-the-idataerrorinfo-interface-vb"></a>Validation avec l’Interface IDataErrorInfo (VB)
====================
par [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther vous montre comment afficher des messages d’erreur de validation personnalisée en implémentant l’interface IDataErrorInfo dans une classe de modèle.


L’objectif de ce didacticiel est d’expliquer une approche pour la validation dans une application ASP.NET MVC. Vous allez apprendre à empêcher un utilisateur d’envoyer un formulaire HTML sans fournir de valeurs pour les champs obligatoires. Dans ce didacticiel, vous allez apprendre à effectuer la validation à l’aide de l’interface IErrorDataInfo.

## <a name="assumptions"></a>Assumptions (Hypothèses)

Dans ce didacticiel, je vais utiliser la base de données MoviesDB et la table de base de données de films. Cette table comporte les colonnes suivantes :

<a id="0.6_table01"></a>


| **Nom de colonne** | **Type de données** | **Autoriser les valeurs null** |
| --- | --- | --- |
| Id | Int | False |
| Titre | nvarchar (100) | False |
| Directeur | nvarchar (100) | False |
| DateReleased | DateTime | False |


Dans ce didacticiel, j’utilise Microsoft Entity Framework pour générer mes classes de modèle de base de données. La classe Movie générée par Entity Framework s’affiche dans la Figure 1.


[![L’entité de film](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)

**Figure 01**: entité de la vidéo ([cliquez pour afficher l’image en taille réelle](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))


> [!NOTE] 
> 
> Pour en savoir plus sur l’utilisation d’Entity Framework pour générer vos classes de modèle de base de données, consultez que mon didacticiel intitulé Création des Classes de modèle avec Entity Framework.


## <a name="the-controller-class"></a>La classe de contrôleur

Nous utilisons le contrôleur Home cinéma de liste et que vous créez de nouveaux films. Le code de cette classe est contenu dans le Listing 1.

**Liste 1 - Controllers\HomeController.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample1.vb)]

La classe de contrôleur d’accueil dans le Listing 1 contient deux actions Create(). La première action affiche le formulaire HTML pour la création d’un nouveau film. La deuxième action Create() effectue l’insertion de ce nouveau film dans la base de données. La deuxième action Create() est appelée lorsque le formulaire affiché par la première action Create() est envoyé au serveur.

Notez que la seconde action Create() contient les lignes de code suivantes :

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample2.vb)]

La propriété IsValid retourne false quand il existe une erreur de validation. Dans ce cas, la vue Create qui contient le formulaire HTML pour la création d’un film est réaffichée.

## <a name="creating-a-partial-class"></a>Création d’une classe partielle

La classe Movie est générée par Entity Framework. Vous pouvez voir le code de la classe Movie si vous développez le fichier MoviesDBModel.edmx dans la fenêtre Explorateur de solutions et ouvrez le fichier MoviesDBModel.Designer.vb dans l’éditeur de Code (voir Figure 2).


[![Le code de l’entité de film](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)

**Figure 02**: le code de l’entité de film ([cliquez pour afficher l’image en taille réelle](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))


La classe Movie est une classe partielle. Cela signifie que nous pouvons ajouter une autre classe partielle portant le même nom pour étendre les fonctionnalités de la classe Movie. Nous allons ajouter notre logique de validation à la nouvelle classe partielle.

Ajoutez la classe dans le Listing 2 dans le dossier Modèles.

**Listing 2 - Models\Movie.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample3.vb)]

Notez que la classe dans le Listing 2 inclut le *partielle* modificateur. Les méthodes ou les propriétés que vous ajoutez à cette classe font partie de la classe Movie générée par Entity Framework.

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>Ajout de OnChanging et méthodes de OnChanged partielle

Quand Entity Framework génère une classe d’entité, Entity Framework ajoute automatiquement les méthodes partielles à la classe. Entity Framework génère des méthodes partielles OnChanging et OnChanged qui correspondent à chaque propriété de la classe.

Dans le cas de la classe Movie, Entity Framework crée des méthodes suivantes :

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

La méthode OnChanging est appelée droite avant la modification de la propriété correspondante. La méthode OnChanged est appelée droit une fois que la propriété est modifiée.

Vous pouvez tirer parti de ces méthodes partielles pour ajouter la logique de validation à la classe Movie. La classe Movie dans le Listing 3 de la mise à jour vérifie que les propriétés Title et directeur sont affectées des valeurs non vides.

> [!NOTE] 
> 
> Une méthode partielle est une méthode définie dans une classe que vous n’êtes pas obligé de mettre en œuvre. Si vous n’implémentez une méthode partielle ensuite le compilateur supprime la signature de méthode et tous les appels à la méthode, par conséquent, il n’existe aucun coût d’exécution associé à la méthode partielle. Dans l’éditeur de Code Visual Studio, vous pouvez ajouter une méthode partielle en tapant le mot clé *partielle* suivi d’un espace pour afficher une liste des vues partielles pour implémenter.


**Liste 3 - Models\Movie.vb**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample4.vb)]

Par exemple, si vous essayez d’assigner une chaîne vide à la propriété de titre, un message d’erreur est ensuite affecté à un dictionnaire nommé \_erreurs.

À ce stade, rien ne réellement se produit lorsque vous assignez une chaîne vide à la propriété de titre et une erreur est ajoutée à la private \_champ des erreurs. Nous avons besoin implémenter l’interface IDataErrorInfo pour exposer ces erreurs de validation pour l’infrastructure ASP.NET MVC.

## <a name="implementing-the-idataerrorinfo-interface"></a>Implémentation de l’Interface IDataErrorInfo

L’interface IDataErrorInfo a fait partie du .NET framework depuis la première version. Cette interface est une interface très simple :

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample5.vb)]

Si une classe implémente l’interface IDataErrorInfo, l’infrastructure ASP.NET MVC utilise cette interface lors de la création d’une instance de la classe. Par exemple, le contrôleur Home Create() action accepte une instance de la classe Movie :

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample6.vb)]

L’infrastructure ASP.NET MVC crée l’instance du film passé à l’action Create() à l’aide d’un classeur de modèles (le DefaultModelBinder). Le binder de modèle est responsable de la création d’une instance de l’objet de film en liant les champs de formulaire HTML à une instance de l’objet de film.

Le DefaultModelBinder détecte qu’une classe implémente l’interface IDataErrorInfo ou non. Si une classe implémente cette interface le binder de modèle appelle l’indexeur IDataErrorInfo.this pour chaque propriété de la classe. Si l’indexeur retourne un message d’erreur le binder de modèle ajoute ce message d’erreur pour modéliser état automatiquement.

Le DefaultModelBinder vérifie également la propriété IDataErrorInfo.Error. Cette propriété est destinée à représenter les erreurs de validation spécifique de propriétés de non associés à la classe. Par exemple, vous souhaiterez peut-être appliquer une règle de validation qui dépend des valeurs de plusieurs propriétés de la classe Movie. Dans ce cas, vous renvoie une erreur de validation à partir de la propriété de l’erreur.

La classe Movie mise à jour sur la liste 4 implémente l’interface IDataErrorInfo.

**Liste 4 - Models\Movie.vb (implémente l’interface IDataErrorInfo)**

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample7.vb)]

Dans la liste 4, la propriété d’indexeur vérifie le \_collection d’erreurs pour voir si elle contient une clé qui correspond au nom de propriété passé à l’indexeur. Si aucune erreur de validation associé à la propriété n’est une chaîne vide est retournée.

Vous n’avez pas besoin de modifier le contrôleur Home en aucune façon d’utiliser la classe Movie modifiée. La page affichée dans la Figure 3 illustre que se passe-t-il quand aucune valeur n’est entrée pour les champs de formulaire titre ou directeur.


[![Création automatique de méthodes d’action](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)

**Figure 03**: un formulaire avec des valeurs manquantes ([cliquez pour afficher l’image en taille réelle](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))


Notez que la valeur DateReleased est validée automatiquement. Étant donné que la propriété DateReleased n’accepte pas les valeurs NULL, le DefaultModelBinder génère automatiquement une erreur de validation pour cette propriété lorsqu’elle n’a pas une valeur. Si vous souhaitez modifier le message d’erreur pour la propriété DateReleased vous devez créer un classeur de modèles personnalisés.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez appris à utiliser l’interface IDataErrorInfo pour générer des messages d’erreur de validation. Tout d’abord, nous avons créé une classe partielle de film qui étend les fonctionnalités de la classe partielle de film générée par Entity Framework. Ensuite, nous avons ajouté la logique de validation pour les films classe OnTitleChanging() et OnDirectorChanging() méthodes partielles. Enfin, nous avons implémenté l’interface IDataErrorInfo afin d’exposer ces messages de validation à l’infrastructure ASP.NET MVC.

> [!div class="step-by-step"]
> [Précédent](performing-simple-validation-vb.md)
> [Suivant](validating-with-a-service-layer-vb.md)
