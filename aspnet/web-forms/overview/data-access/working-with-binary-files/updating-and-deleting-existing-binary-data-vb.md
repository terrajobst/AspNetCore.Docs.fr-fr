---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
title: La mise à jour et la suppression des données binaires existantes (VB) | Microsoft Docs
author: rick-anderson
description: Dans les didacticiels précédents, nous avons vu comment le contrôle GridView permet de facilement modifier et supprimer des données de texte. Dans ce didacticiel, nous voyons comment le contrôle GridView rendent...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 3a052ced-9cf5-47b8-a400-934f0b687c26
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 9a0f9e5ebb8bc8227e869f2af085394262a31663
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832586"
---
<a name="updating-and-deleting-existing-binary-data-vb"></a>La mise à jour et la suppression des données binaires existantes (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_VB.exe) ou [télécharger le PDF](updating-and-deleting-existing-binary-data-vb/_static/datatutorial57vb1.pdf)

> Dans les didacticiels précédents, nous avons vu comment le contrôle GridView permet de facilement modifier et supprimer des données de texte. Dans ce didacticiel, nous expliquons comment le contrôle GridView rend également possible de modifier et supprimer des données binaires, si ces données binaires sont enregistrées dans la base de données ou stockées dans le système de fichiers.


## <a name="introduction"></a>Introduction

Sur les didacticiels de trois dernières nous ve ajouté un certain nombre de fonctionnalités de manipulation de données binaires. Nous avons commencé en ajoutant un `BrochurePath` colonne à la `Categories` de table et de mise à jour de l’architecture en conséquence. Nous avons également ajouté des méthodes de couche d’accès aux données et de la couche de logique métier pour travailler avec la table s catégories existant `Picture` colonne qui conserve le s contenu binaire d’un fichier image. Nous avons créé des pages web pour présenter les données binaires dans un GridView un lien de téléchargement pour la brochure, avec l’image de catégorie s affichée à un `<img>` élément et avez ajouté un contrôle DetailsView pour permettre aux utilisateurs d’ajouter une nouvelle catégorie et télécharger ses données brochure et image.

Il reste à implémenter que la possibilité de modifier et supprimer des catégories existantes, ce qui nous allons faire dans ce didacticiel à l’aide de la modification intégrés de GridView s et la suppression de fonctionnalités. Lorsque vous modifiez une catégorie, l’utilisateur pourra éventuellement télécharger une nouvelle image ou la catégorie de continuer à utiliser l’existant. Pour la brochure, ils peuvent soit choisir d’utiliser la brochure existante, pour télécharger une nouvelle brochure ou pour indiquer que la catégorie n’a plus une brochure associée. Laissez s commencer !

## <a name="step-1-updating-the-data-access-layer"></a>Étape 1 : La mise à jour de la couche d’accès aux données

La couche Data Access a généré automatiquement `Insert`, `Update`, et `Delete` méthodes, mais ces méthodes ont été générés selon le `CategoriesTableAdapter` requête principale s, qui n’inclut pas le `Picture` colonne. Par conséquent, le `Insert` et `Update` méthodes n’incluent pas de paramètres pour spécifier les données binaires pour l’image de catégorie s. Comme nous l’avons fait le [didacticiel précédent](including-a-file-upload-option-when-adding-a-new-record-vb.md), nous devons créer une nouvelle méthode du TableAdapter pour mettre à jour le `Categories` table lors de la spécification des données binaires.

Ouvrez le DataSet typé et, à partir du concepteur, cliquez sur le `CategoriesTableAdapter` en-tête de s et choisissez Ajouter une requête dans le menu contextuel pour launche l’Assistant Configuration de requêtes TableAdapter. Cet Assistant commence par nous demandent comment la requête TableAdapter doit-il accéder à la base de données. Choisissez d’utiliser des instructions SQL et cliquez sur Suivant. L’étape suivante vous invite à entrer pour le type de requête doit être généré. Dans la mesure où re création d’une requête pour ajouter un nouvel enregistrement à la `Categories` de table, choisissez la mise à jour et cliquez sur Suivant.


[![Sélectionnez l’Option de mise à jour](updating-and-deleting-existing-binary-data-vb/_static/image2.png)](updating-and-deleting-existing-binary-data-vb/_static/image1.png)

**Figure 1**: sélectionnez l’Option de mise à jour ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-vb/_static/image3.png))


Nous devons maintenant spécifier le `UPDATE` instruction SQL. L’Assistant suggère automatiquement un `UPDATE` instruction correspondant à la requête principale de s TableAdapter (celui qui met à jour le `CategoryName`, `Description`, et `BrochurePath` valeurs). Modifier l’instruction afin que le `Picture` colonne est incluse avec un `@Picture` paramètre, comme suit :


[!code-sql[Main](updating-and-deleting-existing-binary-data-vb/samples/sample1.sql)]

Le dernier écran de l’Assistant nous demande de nommer la nouvelle méthode du TableAdapter. Entrez `UpdateWithPicture` et cliquez sur Terminer.


[![Nom de la nouvelle UpdateWithPicture TableAdapter (méthode)](updating-and-deleting-existing-binary-data-vb/_static/image5.png)](updating-and-deleting-existing-binary-data-vb/_static/image4.png)

**Figure 2**: nom de la nouvelle méthode TableAdapter `UpdateWithPicture` ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-vb/_static/image6.png))


## <a name="step-2-adding-the-business-logic-layer-methods"></a>Étape 2 : Ajouter les méthodes de couche de logique métier

En plus de la mise à jour de la couche DAL, nous devons mettre à jour de la couche BLL afin d’inclure des méthodes pour la mise à jour et suppression d’une catégorie. Ce sont les méthodes qui seront appelées à partir de la couche de présentation.

Pour supprimer une catégorie, nous pouvons utiliser le `CategoriesTableAdapter` s auto-généré `Delete` (méthode). Ajoutez la méthode suivante à la `CategoriesBLL` classe :


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample2.vb)]

Pour ce didacticiel, laissez s créer deux méthodes pour mettre à jour une catégorie - qui attend des données de l’image binaire et appelle le `UpdateWithPicture` méthode que nous venons d’ajouter à la `CategoriesTableAdapter` et une autre qui accepte uniquement le `CategoryName`, `Description`et `BrochurePath`valeurs et utilise `CategoriesTableAdapter` classe s auto-généré `Update` instruction. Le raisonnement derrière à l’aide de deux méthodes est que dans certaines circonstances, un utilisateur peut souhaiter mettre à jour l’image de s catégorie, ainsi que ses autres champs, auquel cas l’utilisateur a charger la nouvelle image. Les données binaires téléchargées image s peuvent ensuite être utilisées dans le `UPDATE` instruction. Dans d’autres cas, l’utilisateur peut-être uniquement être intéressées par la mise à jour, par exemple, le nom et la description. Mais si le `UPDATE` instruction attend les données binaires pour le `Picture` ainsi de colonne, puis nous d devez fournir ces informations. Cela nécessiterait un aller-retour supplémentaire vers la base de données pour ramener les données d’image pour l’enregistrement en cours de modification. Par conséquent, nous voulons deux `UPDATE` méthodes. La couche de logique métier déterminent celui que vous souhaitez utiliser selon si les données de l’image sont fournies lors de la mise à jour de la catégorie.

Pour ce faire, ajoutez deux méthodes à la `CategoriesBLL` classe nommées `UpdateCategory`. La première condition doit accepter trois `String` s, un `Byte` tableau et un `Integer` en tant que son entrée paramètres ; la seconde, trois seulement `String` s et un `Integer`. Le `String` sont des paramètres d’entrée pour le nom de catégorie s, la description et le chemin d’accès du fichier brochure, le `Byte` tableau est pour le contenu binaire de l’image de la catégorie s et le `Integer` identifie le `CategoryID` de l’enregistrement à mettre à jour. Notez que la première surcharge appelle le deuxième si le passé dans `Byte` tableau est `Nothing`:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample3.vb)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>Étape 3 : Copier sur l’insertion et les fonctionnalités d’une vue

Dans le [didacticiel précédent](including-a-file-upload-option-when-adding-a-new-record-vb.md) que nous avons créé une page nommée `UploadInDetailsView.aspx` qui liste toutes les catégories dans un GridView et fourni un contrôle DetailsView pour ajouter de nouvelles catégories pour le système. Dans ce didacticiel, nous étendons le contrôle GridView pour inclure la modification et suppression de prise en charge. Au lieu de continuer à travailler à partir de `UploadInDetailsView.aspx`, s permettent de placer à la place des modifications de ce didacticiel s dans le `UpdatingAndDeleting.aspx` page à partir du même dossier, `~/BinaryData`. Copiez et collez le balisage déclaratif et de code à partir de `UploadInDetailsView.aspx` à `UpdatingAndDeleting.aspx`.

Commencez par ouvrir le `UploadInDetailsView.aspx` page. Copiez toute la syntaxe déclarative dans le `<asp:Content>` élément, comme indiqué dans la Figure 3. Ensuite, ouvrez `UpdatingAndDeleting.aspx` et collez ce balisage dans son `<asp:Content>` élément. De même, copiez le code de la `UploadInDetailsView.aspx` page classe code-behind s `UpdatingAndDeleting.aspx`.


[![Copier le balisage déclaratif de UploadInDetailsView.aspx](updating-and-deleting-existing-binary-data-vb/_static/image8.png)](updating-and-deleting-existing-binary-data-vb/_static/image7.png)

**Figure 3**: copier le balisage déclaratif de `UploadInDetailsView.aspx` ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-vb/_static/image9.png))


Après avoir copié sur le balisage déclaratif et le code, visitez `UpdatingAndDeleting.aspx`. Vous devriez voir la même sortie et ont la même expérience utilisateur comme avec `UploadInDetailsView.aspx` page du didacticiel précédent.

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>Étape 4 : Ajout de la suppression de la prise en charge de l’ObjectDataSource et un GridView

Comme expliqué dans la [une vue d’ensemble d’insertion, mise à jour et suppression des données](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) didacticiel, le contrôle GridView fournit des fonctionnalités de suppression intégrées et ces fonctionnalités peuvent être activées au battement d’une case à cocher si la grille s sous-jacent source de données prend en charge la suppression. Actuellement ObjectDataSource GridView est lié à (`CategoriesDataSource`) ne prend pas en charge la suppression.

Pour résoudre ce problème, cliquez sur l’option de configurer la Source de données à partir de la balise active de s ObjectDataSource pour lancer l’Assistant. Le premier écran montre que ObjectDataSource est configuré pour fonctionner avec la `CategoriesBLL` classe. Appuyez sur Suivant. Actuellement, seuls l’ObjectDataSource s `InsertMethod` et `SelectMethod` propriétés sont spécifiées. Toutefois, l’Assistant et rempli automatiquement les listes déroulantes dans les onglets UPDATE et DELETE avec la `UpdateCategory` et `DeleteCategory` méthodes, respectivement. Il s’agit, car dans le `CategoriesBLL` classe nous marquée ces méthodes à l’aide de la `DataObjectMethodAttribute` en tant que les méthodes par défaut pour la mise à jour et la suppression.

Pour l’instant, définition de la liste de déroulante mise à jour onglet s (aucun), mais laissez la liste déroulante d’onglet s DELETE définie sur `DeleteCategory`. Nous allons revenir à cet Assistant à l’étape 6 pour ajouter la prise en charge la mise à jour.


[![Configurer pour utiliser la méthode DeleteCategory ObjectDataSource](updating-and-deleting-existing-binary-data-vb/_static/image11.png)](updating-and-deleting-existing-binary-data-vb/_static/image10.png)

**Figure 4**: configurer ObjectDataSource à utiliser le `DeleteCategory` (méthode) ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-vb/_static/image12.png))


> [!NOTE]
> À la fin de l’Assistant, Visual Studio peut vous demander si vous souhaitez actualiser les champs et les clés, qui sera régénérer les données Web contrôle les champs. Choisissez non, car vous sélectionnez Oui pour remplacer les personnalisations de champ que vous avez apportées.


ObjectDataSource inclut désormais une valeur pour son `DeleteMethod` propriété ainsi qu’un `DeleteParameter`. Rappelez-vous que lorsque vous utilisez l’Assistant pour spécifier les méthodes, Visual Studio définit les opérations de mappage ObjectDataSource `OldValuesParameterFormatString` propriété `original_{0}`, ce qui provoque des problèmes avec la mise à jour et supprimer des appels de méthode. Par conséquent, effacer entièrement cette propriété soit réaffectez-lui la valeur par défaut, `{0}`. Si vous avez besoin de rafraîchir la mémoire sur cette propriété ObjectDataSource, consultez le [une vue d’ensemble d’insertion, mise à jour et suppression des données](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) didacticiel.

Après la fin de l’Assistant et corriger la `OldValuesParameterFormatString`, le balisage déclaratif de s ObjectDataSource doit ressembler à ce qui suit :


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample4.aspx)]

Après avoir configuré l’ObjectDataSource, ajouter des fonctionnalités de suppression pour le contrôle GridView en cochant la case Activer la suppression de la balise active de GridView s. Cela ajoutera un CommandField au GridView dont `ShowDeleteButton` propriété est définie sur `True`.


[![Activer la prise en charge pour la suppression dans le contrôle GridView](updating-and-deleting-existing-binary-data-vb/_static/image14.png)](updating-and-deleting-existing-binary-data-vb/_static/image13.png)

**Figure 5**: activer la prise en charge pour suppression dans le contrôle GridView ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-vb/_static/image15.png))


Prenez un moment pour tester la fonctionnalité de suppression. Il existe une clé étrangère entre la `Products` table s `CategoryID` et `Categories` table s `CategoryID`, de sorte que vous obtenez une exception de violation de contrainte de clé étrangère si vous essayez de supprimer toutes les huit premières catégories. Pour tester cette fonctionnalité out, ajoutez une nouvelle catégorie, en fournissant une brochure et l’image. Catégorie de mon test, illustré Figure 6, inclut un fichier brochure de test nommé `Test.pdf` et d’une image de test. La figure 7 illustre le contrôle GridView après que la catégorie de test a été ajoutée.


[![Ajouter une catégorie de Test avec une Brochure et l’Image](updating-and-deleting-existing-binary-data-vb/_static/image17.png)](updating-and-deleting-existing-binary-data-vb/_static/image16.png)

**Figure 6**: ajouter une catégorie de Test avec une Image et la Brochure ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-vb/_static/image18.png))


[![Après avoir inséré la catégorie de Test, il est affiché dans le contrôle GridView](updating-and-deleting-existing-binary-data-vb/_static/image20.png)](updating-and-deleting-existing-binary-data-vb/_static/image19.png)

**Figure 7**: après avoir inséré la catégorie de Test, il est affiché dans le contrôle GridView ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-vb/_static/image21.png))


Dans Visual Studio, actualisez l’Explorateur de solutions. Vous devez maintenant voir un nouveau fichier dans le `~/Brochures` dossier, `Test.pdf` (voir Figure 8).

Ensuite, cliquez sur le lien Supprimer dans la ligne de la catégorie de Test, à l’origine de la page de publication (postback) et le `CategoriesBLL` classe s `DeleteCategory` méthode au feu. Cette action appelle la couche DAL s `Delete` méthode, à l’origine approprié `DELETE` instruction à transmettre à la base de données. Les données sont ensuite de nouveau liées au GridView et le balisage est envoyé au client avec la catégorie de Test n’est plus présent.

Bien que le flux de travail de suppression supprimé l’enregistrement de catégorie de Test à partir de la `Categories` table, il n’a pas supprimé de son fichier brochure du système de fichiers de serveur s web. Actualiser l’Explorateur de solutions et vous verrez que `Test.pdf` se trouveront dans le `~/Brochures` dossier.


![Le fichier Test.pdf n’a pas été supprimé du système de fichiers Web Server s](updating-and-deleting-existing-binary-data-vb/_static/image1.gif)

**Figure 8**: le `Test.pdf` fichier n’a pas été supprimé du système de fichiers de serveur s Web


## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>Étape 5 : Supprimer le fichier Brochure de catégorie supprimée

Inconvénients du stockage des données binaires externes à la base de données est que les étapes supplémentaires doivent être prises pour nettoyer ces fichiers lors de l’enregistrement de base de données associée est supprimée. Contrôles GridView et ObjectDataSource fournissent des événements qui se déclenchent avant et après que la commande de suppression a été effectuée. En fait, nous devons créer des gestionnaires d’événements pour les événements préalables et post-action. Avant du `Categories` enregistrement est supprimé. nous devons déterminer son chemin d’accès du fichier s PDF, mais nous n t que vous souhaitez supprimer le fichier PDF avant la suppression de la catégorie dans le cas où il existe une exception et la catégorie n’est pas supprimée.

Les opérations de mappage GridView [ `RowDeleting` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx) se déclenche avant la commande de suppression ObjectDataSource s a été appelée, tandis que son [ `RowDeleted` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx) se déclenche après. Créer des gestionnaires d’événements pour ces deux événements utilisant le code suivant :


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample5.vb)]

Dans le `RowDeleting` Gestionnaire d’événements, le `CategoryID` de la ligne en cours de suppression est retiré de la s GridView `DataKeys` collection, qui est accessible dans ce gestionnaire d’événements via le `e.Keys` collection. Ensuite, le `CategoriesBLL` classe s `GetCategoryByCategoryID(categoryID)` est appelée pour retourner des informations sur l’enregistrement en cours de suppression. Si le texte retourné `CategoriesDataRow` a un objet non -`NULL``BrochurePath` valeur, il est stocké dans la variable de page `deletedCategorysPdfPath` afin que le fichier peut être supprimé dans le `RowDeleted` Gestionnaire d’événements.

> [!NOTE]
> Plutôt que de la récupération de la `BrochurePath` les détails de la `Categories` enregistrement en cours de suppression dans le `RowDeleting` Gestionnaire d’événements, nous aurions pu également ajouter le `BrochurePath` au GridView s `DataKeyNames` propriété et accéder à la valeur d’enregistrement s via le `e.Keys` collection. Cette opération serait légèrement augmenter la taille d’état GridView s vue, mais serait réduire la quantité de code nécessaire et enregistrer un trajet sur la base de données.


Après l’ObjectDataSource commande de suppression s sous-jacent a été appelée, les opérations de mappage GridView `RowDeleted` incendies de gestionnaire d’événements. Si aucune exception de la suppression des données et il existe une valeur pour `deletedCategorysPdfPath`, puis le fichier PDF est supprimé du système de fichiers. Notez que ce code supplémentaire n’est pas nécessaire pour nettoyer les données binaires de s catégorie associées à son image. S, car les données d’image sont stockées directement dans la base de données, si vous supprimez le `Categories` ligne supprime également ces données d’image de catégorie s.

Après avoir ajouté les deux gestionnaires d’événements, exécutez à nouveau ce cas de test. Lorsque vous supprimez la catégorie, son PDF associé est également supprimé.

La mise à jour une données binaires enregistrement s associé existant fournit des défis intéressants. Le reste de ce didacticiel explore en ajoutant des fonctionnalités de mise à jour à la brochure et l’image. Étape 6 explore des techniques de la mise à jour les informations de la brochure tandis que l’étape 7 examine la mise à jour de l’image.

## <a name="step-6-updating-a-category-s-brochure"></a>Étape 6 : Mise à jour une Brochure s de catégorie

Comme indiqué dans le didacticiel d’une vue d’ensemble d’insertion, mise à jour et suppression des données, le contrôle GridView offre au niveau des lignes édition prise en charge intégrée qui peut être implémentée par le cycle d’une case à cocher si sa source de données sous-jacente est correctement configuré. Actuellement, le `CategoriesDataSource` ObjectDataSource n’est pas encore configurée pour inclure la mise à jour de la prise en charge, donc let s ajouter que dans.

Cliquez sur le lien configurer la Source de données à partir de l’Assistant de s ObjectDataSource et passez à la deuxième étape. Raison de la `DataObjectMethodAttribute` utilisé dans `CategoriesBLL`, la liste déroulante de mise à jour doit être renseignée automatiquement avec le `UpdateCategory` surcharge qui accepte quatre paramètres d’entrée (pour toutes les colonnes mais `Picture`). Modifier cela afin qu’il utilise la surcharge avec cinq paramètres.


[![Configurer l’ObjectDataSource pour utiliser la méthode UpdateCategory ne qui inclut un paramètre pour l’image](updating-and-deleting-existing-binary-data-vb/_static/image23.png)](updating-and-deleting-existing-binary-data-vb/_static/image22.png)

**Figure 9**: configurer ObjectDataSource à utiliser le `UpdateCategory` méthode qui inclut un paramètre pour `Picture` ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-vb/_static/image24.png))


ObjectDataSource inclut désormais une valeur pour son `UpdateMethod` propriété mais aussi correspondant `UpdateParameter` s. Comme indiqué à l’étape 4, Visual Studio définit les opérations de mappage ObjectDataSource `OldValuesParameterFormatString` propriété `original_{0}` lors de l’utilisation de l’Assistant Configurer la Source de données. Cela entraîner des problèmes avec la mise à jour et supprimer des appels de méthode. Par conséquent, effacer entièrement cette propriété soit réaffectez-lui la valeur par défaut, `{0}`.

Après la fin de l’Assistant et corriger la `OldValuesParameterFormatString`, le balisage déclaratif de s ObjectDataSource doit ressembler à ce qui suit :


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample6.aspx)]

Pour activer les fonctionnalités d’édition intégrées s GridView, cochez la case Activer la modification de la balise active de s GridView. Cela définira le s CommandField `ShowEditButton` propriété `True`, se traduisant par l’ajout d’un bouton Modifier (et les boutons Annuler et de mise à jour pour la ligne en cours de modification).


[![Configurer le contrôle GridView à la modification de la prise en charge](updating-and-deleting-existing-binary-data-vb/_static/image26.png)](updating-and-deleting-existing-binary-data-vb/_static/image25.png)

**Figure 10**: configurer le contrôle GridView à la modification de la prise en charge ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-vb/_static/image27.png))


Visitez la page via un navigateur et cliquez sur un de la ligne s boutons d’édition. Le `CategoryName` et `Description` BoundFields sont rendus sous forme de zones de texte. Le `BrochurePath` TemplateField ne possède pas une `EditItemTemplate`, donc il continue d’afficher son `ItemTemplate` un lien vers la brochure. Le `Picture` ImageField rendu sous la forme d’une zone de texte dont la propriété `Text` propriété est affectée à la valeur de l’ImageField s `DataImageUrlField` valeur, dans ce cas `CategoryID`.


[![Le contrôle GridView ne dispose pas d’une Interface d’édition pour BrochurePath](updating-and-deleting-existing-binary-data-vb/_static/image29.png)](updating-and-deleting-existing-binary-data-vb/_static/image28.png)

**Figure 11**: le contrôle GridView ne dispose pas d’une Interface de modification pour `BrochurePath` ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-vb/_static/image30.png))


## <a name="customizing-thebrochurepaths-editing-interface"></a>Personnalisation de la`BrochurePath`Interface de modification s

Nous devons créer une interface de modification pour le `BrochurePath` TemplateField, ce qui permet à l’utilisateur soit :

- Laissez la brochure catégorie s en tant que-est,
- Mettre à jour de la brochure catégorie s en chargeant une nouvelle brochure, ou
- Supprimer la brochure catégorie s complètement (dans le cas, la catégorie n’est plus qu’une brochure associée).

Nous devons également mettre à jour le `Picture` interface de modification de s ImageField, mais nous reviendrons sur ce à l’étape 7.

À partir de la balise active de s GridView, cliquez sur le lien Modifier les modèles, puis sélectionnez le `BrochurePath` TemplateField s `EditItemTemplate` dans la liste déroulante. Ajouter un contrôle RadioButtonList Web à ce modèle, en définissant son `ID` propriété `BrochureOptions` et son `AutoPostBack` propriété `True`. À partir de la fenêtre Propriétés, cliquez sur le bouton de sélection dans le `Items` propriété, qui fait apparaître le `ListItem` éditeur de collections. Ajoutez les trois options suivantes avec `Value` s 1, 2 et 3, respectivement :

- Utilisez brochure actuel
- Supprimer la brochure en cours
- Télécharger la nouvelle brochure

Définir la première `ListItem` s `Selected` propriété `True`.


![Ajouter trois ListItems à RadioButtonList](updating-and-deleting-existing-binary-data-vb/_static/image2.gif)

**Figure 12**: ajoutez trois `ListItem` s à RadioButtonList


Sous RadioButtonList, ajoutez un contrôle FileUpload nommé `BrochureUpload`. Définissez ses `Visible` propriété `False`.


[![Ajouter un RadioButtonList et le contrôle FileUpload à EditItemTemplate](updating-and-deleting-existing-binary-data-vb/_static/image32.png)](updating-and-deleting-existing-binary-data-vb/_static/image31.png)

**Figure 13**: ajouter un RadioButtonList et du contrôle FileUpload à la `EditItemTemplate` ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-vb/_static/image33.png))


Cette RadioButtonList fournit les trois options pour l’utilisateur. L’idée est que le contrôle FileUpload s’affichera uniquement si la dernière option, le chargement de la nouvelle de la brochure, est sélectionnée. Pour ce faire, créez un gestionnaire d’événements pour les opérations de mappage RadioButtonList `SelectedIndexChanged` événement et ajoutez le code suivant :


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample7.vb)]

Étant donné que les contrôles RadioButtonList et FileUpload sont dans un modèle, nous devons écrire un peu de code pour ces contrôles d’accès par programmation. Le `SelectedIndexChanged` Gestionnaire d’événements reçoit une référence de RadioButtonList dans le `sender` paramètre d’entrée. Pour obtenir du contrôle FileUpload, nous devons obtenir le parent de s RadioButtonList contrôle et utilisez le `FindControl("controlID")` méthode à partir de là. Une fois que nous avons une référence aux contrôles RadioButtonList et de FileUpload, le FileUpload contrôler s `Visible` propriété est définie sur `True` uniquement si les opérations de mappage RadioButtonList `SelectedValue` égal à 3, qui est le `Value` pour la brochure nouveau téléchargement `ListItem`.

Avec ce code en place, prenez un moment pour tester l’interface de modification. Cliquez sur le bouton Modifier pour une ligne. Initialement, l’option utiliser les brochure actuelle doit être sélectionnée. Modification de l’index sélectionné provoque une publication (postback). Si la troisième option est sélectionnée, l’affichage du contrôle FileUpload, sinon il est masqué. La figure 14 illustre l’interface de modification, un clic sur le bouton Modifier est tout d’abord ; Figure 15 illustre l’interface après avoir sélectionné l’option de chargement de brochure.


[![Initialement, la brochure en cours d’utilisation qu'est sélectionnée](updating-and-deleting-existing-binary-data-vb/_static/image35.png)](updating-and-deleting-existing-binary-data-vb/_static/image34.png)

**Figure 14**: initialement, la brochure en cours d’utilisation est sélectionnée ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-vb/_static/image36.png))


[![En choisissant la brochure de nouveau téléchargement Option affiche le contrôle FileUpload](updating-and-deleting-existing-binary-data-vb/_static/image38.png)](updating-and-deleting-existing-binary-data-vb/_static/image37.png)

**Figure 15**: choix de la brochure de nouveau téléchargement Option affiche le contrôle FileUpload ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-vb/_static/image39.png))


## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>L’enregistrement de la Brochure de fichiers et de la mise à jour le`BrochurePath`colonne

Lorsque l’utilisateur clique sur le bouton de mise à jour de s GridView, son `RowUpdating` événement est déclenché. ObjectDataSource commande de mise à jour de s est appelé, puis le GridView s `RowUpdated` événement est déclenché. Comme avec le workflow de suppression, nous devons créer des gestionnaires d’événements pour ces deux événements. Dans le `RowUpdating` Gestionnaire d’événements, nous devons déterminer l’action à entreprendre en fonction de la `SelectedValue` de la `BrochureOptions` RadioButtonList :

- Si le `SelectedValue` est 1, nous souhaitons continuer à utiliser le même `BrochurePath` paramètre. Par conséquent, nous devons définir le s ObjectDataSource `brochurePath` paramètre à l’objet de `BrochurePath` valeur de l’enregistrement mis à jour. Les opérations de mappage ObjectDataSource `brochurePath` paramètre peut être défini à l’aide de `e.NewValues["brochurePath"] = value`.
- Si le `SelectedValue` est 2, nous souhaitons définir l’enregistrement s `BrochurePath` valeur `NULL`. Cela est possible en définissant le s ObjectDataSource `brochurePath` paramètre `Nothing`, ce qui aboutit à une base de données `NULL` utilisé dans le `UPDATE` instruction. S’il existe un fichier brochure existant en cours de suppression, nous devons supprimer le fichier existant. Toutefois, nous voulons uniquement effectuer cette opération si la mise à jour se termine sans lever une exception.
- Si le `SelectedValue` est 3, puis nous souhaitons vous assurer que l’utilisateur a chargé un fichier PDF et enregistrez-le dans le système de fichiers, puis mettre à jour l’enregistrement s `BrochurePath` valeur de colonne. En outre, s’il existe un fichier brochure existant qui est remplacé, nous devons supprimer le fichier précédent. Toutefois, nous voulons uniquement effectuer cette opération si la mise à jour se termine sans lever une exception.

Les étapes nécessaires pour être terminée lorsque les opérations de mappage RadioButtonList `SelectedValue` est 3 sont virtuellement identiques à ceux utilisés par les opérations de mappage DetailsView `ItemInserting` Gestionnaire d’événements. Ce gestionnaire d’événements est exécuté lorsqu’un nouvel enregistrement de catégorie est ajouté à partir du contrôle DetailsView que nous avons ajouté dans le [didacticiel précédent](including-a-file-upload-option-when-adding-a-new-record-vb.md). Par conséquent, il appartient à refactoriser cette fonctionnalité out dans des méthodes distinctes. Plus précisément, j’ai déplacé les fonctionnalités courantes en deux méthodes :

- `ProcessBrochureUpload(FileUpload, out bool)` accepte comme entrée une instance du contrôle FileUpload et une valeur booléenne de sortie qui spécifie si l’opération de suppression ou de modification doit continuer ou si elle doit être annulée en raison d’une erreur de validation. Cette méthode retourne le chemin d’accès vers le fichier enregistré ou `null` si aucun fichier n’a été enregistré.
- `DeleteRememberedBrochurePath` Supprime le fichier spécifié par le chemin d’accès dans la variable de page `deletedCategorysPdfPath` si `deletedCategorysPdfPath` n’est pas `null`.

Le code pour ces deux méthodes suit. Notez la similarité entre `ProcessBrochureUpload` et les opérations de mappage DetailsView `ItemInserting` Gestionnaire d’événements à partir du didacticiel précédent. Dans ce didacticiel, j’ai mis à jour les gestionnaires d’événements DetailsView s pour utiliser ces nouvelles méthodes. Téléchargez le code associé à ce didacticiel pour voir les modifications aux gestionnaires d’événements s DetailsView.


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample8.vb)]

Les opérations de mappage GridView `RowUpdating` et `RowUpdated` gestionnaires d’événements utilisent le `ProcessBrochureUpload` et `DeleteRememberedBrochurePath` méthodes, comme le montre le code suivant :


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample9.vb)]

Notez comment la `RowUpdating` Gestionnaire d’événements utilise une série d’instructions conditionnelles pour exécuter l’action appropriée selon la `BrochureOptions` RadioButtonList s `SelectedValue` valeur de propriété.

Avec ce code en place, vous pouvez modifier une catégorie et le laisser utiliser son brochure actuel, n’utilisez aucun brochure ou télécharger un nouveau. Continuez et essayez-le. Définissez des points d’arrêt le `RowUpdating` et `RowUpdated` gestionnaires d’événements pour vous faire une idée du flux de travail.

## <a name="step-7-uploading-a-new-picture"></a>Étape 7 : Charger une nouvelle image

Le `Picture` s ImageField modification interface rendus comme une zone de texte renseignée avec la valeur à partir de son `DataImageUrlField` propriété. Pendant le flux de travail, le contrôle GridView transmet un paramètre à ObjectDataSource avec le nom du paramètre s la valeur de l’ImageField s `DataImageUrlField` la valeur entrée dans la zone de texte dans l’interface de modification de valeur de propriété et le paramètre s. Ce comportement est approprié lorsque l’image est enregistrée en tant que fichier sur le système de fichiers et le `DataImageUrlField` contient l’URL complète de l’image. Avec telles circonstances, l’interface de modification affiche l’URL d’image s dans la zone de texte, à laquelle l’utilisateur peut modifier et avez enregistré dans la base de données. Certes, ce t de n interface par défaut autoriser l’utilisateur à télécharger une nouvelle image, mais il leur permet de modifier l’URL de l’image à partir de la valeur actuelle à un autre. Pour ce didacticiel, toutefois, la valeur par défaut de s ImageField modification d’interface ne suffit pas, car le `Picture` données binaires sont stockées directement dans la base de données et la `DataImageUrlField` propriété contient uniquement le `CategoryID`.

Pour mieux comprendre ce qui se passe dans notre didacticiel lorsqu’un utilisateur modifie une ligne avec un ImageField, prenons l’exemple suivant : un utilisateur modifie une ligne avec `CategoryID` 10, à l’origine le `Picture` ImageField pour restituer comme une zone de texte avec la valeur 10. Imaginez que l’utilisateur modifie la valeur dans cette zone de texte à 50 et clique sur le bouton de mise à jour. Une publication (postback) se produit et le contrôle GridView crée initialement un paramètre nommé `CategoryID` avec la valeur 50. Toutefois, avant que le contrôle GridView envoie ce paramètre (et le `CategoryName` et `Description` paramètres), il ajoute les valeurs à partir de la `DataKeys` collection. Par conséquent, il remplace le `CategoryID` paramètre avec la ligne s sous-jacent actuel `CategoryID` valeur 10. En bref, les opérations de mappage ImageField modification d’interface n’a aucun effet sur le flux de travail pour ce didacticiel, car les noms des opérations de mappage ImageField `DataImageUrlField` propriété et la grille s `DataKey` valeur sont identiques.

Bien que l’ImageField rend plus facile d’afficher une image basée sur la base de données, nous ne souhaitez pour fournir une zone de texte dans l’interface de modification. Au lieu de cela, nous voulons offrir un contrôle FileUpload que l’utilisateur final peut utiliser pour modifier l’image de s catégorie. Contrairement à la `BrochurePath` valeur pour ces didacticiels nous ve décidé d’exiger que chaque catégorie doit être une image. Par conséquent, nous n’avez besoin de t à indiquer qu’il y a aucune image associé à l’utilisateur ne peut soit charger une nouvelle image ou laissez l’image actuelle en tant que l’utilisateur-est.

Pour personnaliser l’interface de modification s ImageField, nous devons convertir en TemplateField. À partir de la balise active de s GridView, cliquez sur le lien Modifier les colonnes, sélectionnez l’ImageField, cliquez sur la convertir ce champ en TemplateField lien.


![Convertir l’ImageField en TemplateField](updating-and-deleting-existing-binary-data-vb/_static/image3.gif)

**Figure 16**: convertir l’ImageField en TemplateField


Conversion de l’ImageField en TemplateField de cette manière génère un TemplateField avec deux modèles. Comme le montre la syntaxe déclarative suivante, le `ItemTemplate` contient un serveur Web Image contrôle dont `ImageUrl` propriété est affectée à l’aide de la syntaxe de liaison de données basée sur les opérations de mappage ImageField `DataImageUrlField` et `DataImageUrlFormatString` propriétés. Le `EditItemTemplate` contient une zone de texte dont `Text` propriété est liée à la valeur spécifiée par le `DataImageUrlField` propriété.


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample10.aspx)]

Nous devons mettre à jour le `EditItemTemplate` pour utiliser un contrôle FileUpload. À partir de la GridView balise active s cliquez sur Modifier les modèles un lien, puis sélectionnez le `Picture` TemplateField s `EditItemTemplate` dans la liste déroulante. Dans le modèle, vous devez voir une zone de texte supprimer cela. Ensuite, faites glisser un contrôle FileUpload à partir de la boîte à outils dans le paramètre de modèle, son `ID` à `PictureUpload`. Ajoutez également le texte à modifier l’image de catégorie s, spécifiez une nouvelle image. Pour conserver l’image de catégorie s, laissez le champ vide pour le modèle.


[![Ajouter un contrôle FileUpload à EditItemTemplate](updating-and-deleting-existing-binary-data-vb/_static/image41.png)](updating-and-deleting-existing-binary-data-vb/_static/image40.png)

**Figure 17**: ajouter un contrôle FileUpload à la `EditItemTemplate` ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-vb/_static/image42.png))


Après la personnalisation de l’interface de modification, consulter votre progression dans un navigateur. Lorsque vous affichez une ligne en mode lecture seule, l’image de s catégorie est indiqué comme avant, mais en cliquant sur le bouton Modifier restitue la colonne d’image sous forme de texte avec un contrôle FileUpload.


[![L’Interface de modification inclut un contrôle FileUpload](updating-and-deleting-existing-binary-data-vb/_static/image44.png)](updating-and-deleting-existing-binary-data-vb/_static/image43.png)

**Figure 18**: l’Interface de modification inclut un contrôle FileUpload ([cliquez pour afficher l’image en taille réelle](updating-and-deleting-existing-binary-data-vb/_static/image45.png))


Rappelez-vous que ObjectDataSource est configurée pour appeler le `CategoriesBLL` classe s `UpdateCategory` méthode qui accepte en entrée les données binaires pour l’image en tant qu’un `Byte` tableau. Si ce tableau est `Nothing`, toutefois, l’autre `UpdateCategory` surcharge est appelée, les problèmes qui la `UPDATE` instruction SQL qui ne modifie pas la `Picture` colonne, laissant ainsi la catégorie s en cours d’image intactes. Par conséquent, dans le s GridView `RowUpdating` Gestionnaire d’événements nous devons référencer par programme le `PictureUpload` FileUpload contrôler et de déterminer si un fichier a été chargé. Si un téléchargement a, nous faisons *pas* pour spécifier une valeur pour le `picture` paramètre. En revanche, si un fichier a été chargé dans le `PictureUpload` contrôle FileUpload, nous voulons vérifier que c’est un fichier JPG. Si elle est, nous pouvons envoyer son contenu binaire à ObjectDataSource via le `picture` paramètre.

Comme avec le code utilisé dans l’étape 6, une grande partie du code nécessaire ici déjà existe dans le s DetailsView `ItemInserting` Gestionnaire d’événements. Par conséquent, je ve a refactorisé les fonctionnalités courantes dans une nouvelle méthode, `ValidPictureUpload`et mis à jour le `ItemInserting` Gestionnaire d’événements pour utiliser cette méthode.

Ajoutez le code suivant au début de la s GridView `RowUpdating` Gestionnaire d’événements. Il s important que ce code précéder le code qui enregistre le fichier brochure dans la mesure où nous ne pas vouloir enregistrer la brochure dans le système de fichiers du serveur s web si un fichier image non valide est téléchargé.


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample11.vb)]

Le `ValidPictureUpload(FileUpload)` méthode prend un contrôle FileUpload comme seul paramètre d’entrée et vérifie l’extension du fichier chargé s pour vous assurer que le fichier chargé est un fichier JPG ; elle est appelée uniquement si un fichier image est chargé. Si aucun fichier n’est chargé, le paramètre de l’image n’est ne pas définie et par conséquent utilise sa valeur par défaut `Nothing`. Si une image a été chargée et `ValidPictureUpload` retourne `True`, le `picture` paramètre reçoit les données binaires de l’image chargée ; si la méthode retourne `False`, le flux de travail de mise à jour est annulé et le Gestionnaire d’événements s’est arrêté.

Le `ValidPictureUpload(FileUpload)` code de la méthode a été refactorisé à partir de la s DetailsView `ItemInserting` Gestionnaire d’événements, suit :


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample12.vb)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>Étape 8 : En remplaçant les images de catégories d’origine par jpg

Rappelez-vous que les images de huit catégories d’origine sont des fichiers bitmap encapsulés dans un en-tête OLE. Maintenant que nous avons ajouté la possibilité de modifier une vue d’enregistrement s existant, prenez un moment pour remplacer ces bitmaps par jpg. Si vous souhaitez continuer à utiliser les images de catégorie en cours, vous pouvez convertir les jpg en effectuant les étapes suivantes :

1. Enregistrer les images bitmap sur votre disque dur. Visitez le `UpdatingAndDeleting.aspx` page dans votre navigateur et pour chacune des huit premières catégories, avec le bouton droit sur l’image et choisir d’enregistrer l’image.
2. Ouvrez l’image dans votre éditeur d’images de choix. Vous pouvez utiliser Microsoft Paint, par exemple.
3. Enregistrer l’image bitmap en tant qu’une image JPG.
4. Mettre à jour l’image de catégorie s via l’interface de modification, à l’aide du fichier JPG.

Après avoir modifié une catégorie et chargement de l’image JPG, l’image n’est pas restitué dans le navigateur, car le `DisplayCategoryPicture.aspx` page supprime les premiers octets de 78 à partir d’images des huit premières catégories. Résoudre ce problème en supprimant le code qui effectue l’enlèvement d’en-tête OLE. Après cela, le `DisplayCategoryPicture.aspx``Page_Load` Gestionnaire d’événements doit avoir simplement le code suivant :


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample13.vb)]

> [!NOTE]
> Le `UpdatingAndDeleting.aspx` page s insertion et la modification des interfaces pourrait utiliser un peu plus de travail. Le `CategoryName` et `Description` BoundFields dans le contrôle DetailsView et GridView doit être converti en TemplateField. Dans la mesure où `CategoryName` n’autorise pas `NULL` valeurs, un contrôle RequiredFieldValidator doit être ajouté. Et le `Description` zone de texte doit probablement être converti en une zone de texte multiligne. Je laisse ces touches finales en guise d’exercice pour vous.


## <a name="summary"></a>Récapitulatif

Ce didacticiel termine notre comment utiliser les données binaires. Dans ce didacticiel et les trois précédents, nous avons vu comment binaires données peuvent être stockées sur le système de fichiers ou directement dans la base de données. Un utilisateur fournit des données binaires au système en sélectionnant un fichier à partir de leur disque dur et de télécharger vers le serveur web, où il peut être stocké sur le système de fichiers, ou inséré dans la base de données. ASP.NET 2.0 inclut un contrôle FileUpload qui permet de fournir une telle interface aussi simple glisser -déplacer. Toutefois, comme indiqué dans le [télécharger des fichiers](uploading-files-vb.md) (didacticiel), du contrôle FileUpload est uniquement adapté aux chargements de fichiers relativement faible, dans l’idéal, n’excédant ne pas un mégaoctet. Nous avons exploré également comment associer les données chargées avec le modèle de données sous-jacent, ainsi que comment modifier et supprimer les données binaires à partir d’enregistrements existants.

Notre prochain ensemble de didacticiels explore les différentes techniques de mise en cache. La mise en cache fournit un moyen d’améliorer un application s les performances globales, extraire les résultats d’opérations coûteuses et les stocke dans un emplacement qui sont plus rapidement accessibles.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Teresa Murphy. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](including-a-file-upload-option-when-adding-a-new-record-vb.md)
