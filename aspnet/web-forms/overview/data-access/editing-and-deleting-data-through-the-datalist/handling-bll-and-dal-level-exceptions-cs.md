---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-cs
title: Gestion des Exceptions de niveau BLL et DAL (c#) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons voir comment gérer les exceptions levées au cours de flux de travail mis à jour d’une DataList modifiable tact.
ms.author: riande
ms.date: 10/30/2006
ms.assetid: f8fd58e2-f932-4f08-ab3d-fbf8ff3295d2
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-cs
msc.type: authoredcontent
ms.openlocfilehash: ebaca5ea34fabe3fcd4979eab2e3f684e8e221be
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828941"
---
<a name="handling-bll--and-dal-level-exceptions-c"></a>Gestion des Exceptions de niveau BLL et DAL (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_CS.exe) ou [télécharger le PDF](handling-bll-and-dal-level-exceptions-cs/_static/datatutorial38cs1.pdf)

> Dans ce didacticiel, nous allons voir comment gérer les exceptions levées au cours de flux de travail mis à jour d’une DataList modifiable tact.


## <a name="introduction"></a>Introduction

Dans le [vue d’ensemble de la modification et suppression des données dans le contrôle DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) didacticiel, nous avons créé un contrôle DataList qui proposés simple de modification et de suppression de fonctionnalités. Bien qu’entièrement fonctionnel, il était tout à fait convivial, comme une erreur s’est produite pendant la modification ou suppression des processus a généré une exception non gérée. Par exemple, en omettant le nom du produit s ou lorsque vous modifiez un produit, entrez une valeur du prix de très abordable !, lève une exception. Étant donné que cette exception n’est pas interceptée dans le code, il se propage au runtime d’ASP.NET, qui affiche ensuite les détails d’exception s dans la page web.

Comme nous l’avons vu dans la [BLL - gestion et les Exceptions au niveau de la couche DAL dans une Page ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) didacticiel, si une exception est levée à partir de la profondeur de la logique métier ou les couches d’accès aux données, les détails d’exception sont retournés à ObjectDataSource et puis au GridView. Nous avons vu comment gérer correctement ces exceptions en créant `Updated` ou `RowUpdated` gestionnaires d’événements pour l’ObjectDataSource ou GridView, la vérification pour une exception et puis indiquant que l’exception a été gérée.

Nos DataList didacticiels, toutefois, ne les t à l’aide d’ObjectDataSource pour la mise à jour et suppression de données. Au lieu de cela, nous travaillons directement par rapport à la couche BLL. Afin de détecter les exceptions provenant de la couche BLL ou DAL, nous devons implémenter le code dans le code-behind de notre page ASP.NET de gestion des exceptions. Dans ce didacticiel, nous allons voir comment gérer les exceptions levées au cours d’un s DataList modifiable, la mise à jour de flux de travail plus tact.

> [!NOTE]
> Dans le *une vue d’ensemble de modification et suppression des données dans le contrôle DataList* didacticiel nous avons abordé les différentes techniques pour la modification et suppression de données dans le contrôle DataList, certaines techniques impliquées à l’aide d’ObjectDataSource pour la mise à jour et la suppression. Si vous employez ces techniques, vous pouvez gérer les exceptions à partir de la couche BLL ou DAL via le s ObjectDataSource `Updated` ou `Deleted` gestionnaires d’événements.


## <a name="step-1-creating-an-editable-datalist"></a>Étape 1 : Création d’un contrôle DataList modifiable

Avant de nous soucier de la gestion des exceptions qui se produisent pendant le flux de travail de mise à jour, permettent de s tout d’abord créer un contrôle DataList modifiable. Ouvrir le `ErrorHandling.aspx` page dans le `EditDeleteDataList` ajouter un contrôle DataList vers le concepteur, un ensemble de dossier, son `ID` propriété `Products`, et ajoutez un nouveau ObjectDataSource nommé `ProductsDataSource`. Configurer l’ObjectDataSource à utiliser le `ProductsBLL` classe s `GetProducts()` méthode de sélection des enregistrements, de définir les listes déroulantes dans l’instruction INSERT, UPDATE et de supprimer des onglets à (None).


[![Retourner les informations de produit à l’aide de la méthode GetProducts()](handling-bll-and-dal-level-exceptions-cs/_static/image2.png)](handling-bll-and-dal-level-exceptions-cs/_static/image1.png)

**Figure 1**: retourner les informations de produit en utilisant le `GetProducts()` (méthode) ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-cs/_static/image3.png))


À l’issue de l’Assistant ObjectDataSource, Visual Studio crée automatiquement un `ItemTemplate` pour le contrôle DataList. Remplacez ceci avec un `ItemTemplate` qui affiche chaque nom de produit s et le prix et inclut un bouton Modifier. Ensuite, créez un `EditItemTemplate` avec un contrôle TextBox Web pour le nom et les prix et les boutons mise à jour et Annuler. Enfin, définissez le contrôle DataList s `RepeatColumns` propriété à 2.

Après ces modifications, votre balisage déclaratif s de page doit ressembler à ce qui suit. Assurez-vous que la modification, Annuler, et les boutons de mise à jour ont leur `CommandName` propriétés définies pour modifier, Annuler et mettre à jour, respectivement.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample1.aspx)]

> [!NOTE]
> Pour ce didacticiel, le contrôle DataList, état d’affichage s doit être activée.


Prenez un moment pour consulter notre progression via un navigateur (voir Figure 2).


[![Chaque produit inclut un bouton Modifier](handling-bll-and-dal-level-exceptions-cs/_static/image5.png)](handling-bll-and-dal-level-exceptions-cs/_static/image4.png)

**Figure 2**: chaque produit inclut un bouton Modifier ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-cs/_static/image6.png))


Actuellement, le bouton Modifier entraîne uniquement une publication (postback) qu’il ne rendent encore le produit modifiable. Pour activer la modification, nous devons créer des gestionnaires d’événements pour le contrôle DataList s `EditCommand`, `CancelCommand`, et `UpdateCommand` événements. Le `EditCommand` et `CancelCommand` événements simplement mettre à jour pour le contrôle DataList s `EditItemIndex` propriété et les données pour le contrôle DataList la reliaison :


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample2.cs)]

Le `UpdateCommand` Gestionnaire d’événements est un peu plus complexe. Il doit lire dans le produit modifié s `ProductID` à partir de la `DataKeys` collection ainsi que le nom de produit s et le prix de zones de texte dans le `EditItemTemplate`, puis appelez le `ProductsBLL` classe s `UpdateProduct` avant de retourner le contrôle DataList (méthode) à son état avant modification.

Pour l’instant, s permettent d’utiliser simplement le même code à partir de la `UpdateCommand` Gestionnaire d’événements dans le *vue d’ensemble de la modification et suppression des données dans le contrôle DataList* didacticiel. Nous allons ajouter le code pour gérer correctement les exceptions à l’étape 2.


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample3.cs)]

En cas d’entrée non valide qui peut être sous la forme d’un prix unitaire incorrectement mis en forme, une valeur du prix unitaire non conforme, telles que-$5.00 ou l’omission du nom de s produit qu'une exception sera levée. Dans la mesure où le `UpdateCommand` Gestionnaire d’événements n’inclut pas les exceptions code à ce stade, l’exception se propage jusqu'à l’exécution d’ASP.NET, où il sera affiché à l’utilisateur final (voir Figure 3).


![Lorsqu’une Exception non gérée se produit, l’utilisateur final voit une Page d’erreur](handling-bll-and-dal-level-exceptions-cs/_static/image7.png)

**Figure 3**: lorsqu’une Exception non gérée se produit, l’utilisateur final voit une Page d’erreur


## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>Étape 2 : Gestion normalement des Exceptions dans le Gestionnaire d’événements de UpdateCommand

Pendant le flux de travail mis à jour, les exceptions peuvent se produire dans le `UpdateCommand` Gestionnaire d’événements, la couche BLL ou la couche DAL. Par exemple, si un utilisateur entre un prix de trop coûteux, la `Decimal.Parse` instruction dans le `UpdateCommand` Gestionnaire d’événements lève une `FormatException` exception. Si l’utilisateur l’omet le nom du s produit ou si le prix a une valeur négative, la couche DAL lève une exception.

Lorsqu’une exception se produit, nous souhaitons afficher un message d’information au sein de la page elle-même. Ajouter un service Web étiquette le contrôle à la page dont la propriété `ID` est défini sur `ExceptionDetails`. Configurer le texte d’étiquette s pour afficher en rouge, très grande taille, police en gras et italique en attribuant son `CssClass` propriété le `Warning` classe CSS, qui est défini dans le `Styles.css` fichier.

Lorsqu’une erreur se produit, nous voulons uniquement l’étiquette à afficher une seule fois. Autrement dit, sur les publications ultérieures, le message d’avertissement étiquette s doit disparaître. Cela est possible en supprimant soit l’étiquette s `Text` propriété ou des paramètres de son `Visible` propriété `False` dans le `Page_Load` Gestionnaire d’événements (comme nous l’avons fait dans le [BLL - gestion et les Exceptions au niveau de la couche DAL dans une page ASP Page .NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) didacticiel) ou en désactivant la prise en charge de l’étiquette s vue état. Permettent d’utiliser cette dernière option s.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample4.aspx)]

Lorsqu’une exception est levée, nous attribuons les détails de l’exception pour le `ExceptionDetails` Label, contrôle s `Text` propriété. Étant donné que son état d’affichage est désactivé, sur les publications ultérieures le `Text` modifications par programmation la propriété s seront perdues, revenez au texte par défaut (une chaîne vide), masquant ainsi le message d’avertissement.

Pour déterminer quand une erreur a été déclenchée pour afficher un message utile sur la page, nous devons ajouter un `Try ... Catch` bloquer à la `UpdateCommand` Gestionnaire d’événements. Le `Try` partie contient le code qui peut provoquer une exception, tandis que le `Catch` bloc contient du code qui est exécuté en cas d’une exception. Découvrez le [notions de base de gestion des exceptions](https://msdn.microsoft.com/library/2w8f0bss.aspx) section dans la documentation .NET Framework pour plus d’informations sur la `Try ... Catch` bloc.


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample5.cs)]

Quand une exception de n’importe quel type est levée par le code dans le `Try` bloc, le `Catch` code de bloc s commence l’exécution. Le type d’exception qui est levée `DbException`, `NoNullAllowedException`, `ArgumentException`et ainsi de suite dépend, exactement, a déclenché l’erreur en premier lieu. Si cet emplacement s un problème au niveau de la base de données, un `DbException` sera levée. Si une valeur non conforme est entrée pour le `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, ou `ReorderLevel` champs, un `ArgumentException` est levée, comme nous avons ajouté du code pour valider ces valeurs de champ dans le `ProductsDataTable` classe (voir la [ Création d’une couche de logique métier](../introduction/creating-a-business-logic-layer-cs.md) didacticiel).

Nous pouvons fournir une explication plus utile à l’utilisateur final en le basant le texte du message sur le type d’exception interceptée. Le code suivant qui a été utilisé dans un formulaire quasiment identique dans le [BLL - gestion et les Exceptions au niveau de la couche DAL dans une Page ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) didacticiel fournit ce niveau de détail :


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample6.cs)]

Pour suivre ce didacticiel, appelez simplement la `DisplayExceptionDetails` méthode à partir de la `Catch` bloc en passant l’interceptée `Exception` instance (`ex`).

Avec le `Try ... Catch` bloquer en place, les utilisateurs sont présentées avec un message d’erreur plus explicite, comme les Figures 4 et 5 show. Notez que, face à une exception, le contrôle DataList, reste dans le mode édition. Il s’agit, car une fois l’exception se produit, le flux de contrôle est immédiatement redirigé vers le `Catch` bloc, en ignorant le code qui retourne le contrôle DataList à son état avant modification.


[![Un Message d’erreur s’affiche si un utilisateur omet un champ requis](handling-bll-and-dal-level-exceptions-cs/_static/image9.png)](handling-bll-and-dal-level-exceptions-cs/_static/image8.png)

**Figure 4**: un Message d’erreur s’affiche si un utilisateur omet un champ requis ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-cs/_static/image10.png))


[![Un Message d’erreur est affiché lorsque vous entrez vous un prix négatif](handling-bll-and-dal-level-exceptions-cs/_static/image12.png)](handling-bll-and-dal-level-exceptions-cs/_static/image11.png)

**Figure 5**: un Message d’erreur est affiché lorsque vous entrez vous un prix négatif ([cliquez pour afficher l’image en taille réelle](handling-bll-and-dal-level-exceptions-cs/_static/image13.png))


## <a name="summary"></a>Récapitulatif

Contrôles GridView et ObjectDataSource fournissent des gestionnaires d’événements post-niveau qui incluent des informations sur les exceptions qui ont été générées pendant le flux de travail de mise à jour et suppression, ainsi que les propriétés qui peuvent être définies pour indiquer si l’exception a été gérée. Ces fonctionnalités, cependant, ne sont pas disponibles quand fonctionne avec le contrôle DataList et à l’aide de la couche BLL directement. Au lieu de cela, nous sommes responsables de l’implémentation de la gestion des exceptions.

Dans ce didacticiel, nous avons vu comment ajouter la gestion des exceptions à un s DataList modifiable, la mise à jour de flux de travail en ajoutant un `Try ... Catch` bloquer à la `UpdateCommand` Gestionnaire d’événements. Si une exception est levée pendant le flux de travail mis à jour, le `Catch` bloc s code s’exécute, affichage des informations utiles dans le `ExceptionDetails` étiquette.

À ce stade, le contrôle DataList ne fait rien pour éviter les exceptions de se produire en premier lieu. Même si nous savons qu’un prix négatif entraîne une exception, nous réalisées t encore ajouté toutes les fonctionnalités pour empêcher proactivement un utilisateur d’entrer ce type d’entrée non valide. Dans notre didacticiel suivant, nous allons voir comment aider à réduire les exceptions provoquées par l’entrée utilisateur non valide en ajoutant des contrôles de validation dans le `EditItemTemplate`.

Bonne programmation !

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Instructions de conception pour les exceptions](https://msdn.microsoft.com/library/ms298399.aspx)
- [Modules de journalisation des erreurs et des gestionnaires (ELMAH)](http://workspaces.gotdotnet.com/elmah) (une bibliothèque open source pour la journalisation des erreurs)
- [Enterprise Library pour .NET Framework 2.0](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) (inclut le bloc d’Application de gestion des exceptions)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Ken Pespisa. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](performing-batch-updates-cs.md)
> [Suivant](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md)
