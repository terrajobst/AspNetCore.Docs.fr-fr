---
uid: web-forms/overview/moving-to-aspnet-20/data-bound-controls
title: Contrôles liés aux données | Microsoft Docs
author: microsoft
description: La plupart des applications ASP.NET s’appuient sur une certaine de présentation des données à partir d’une source de données back-end. Contrôles liés aux données ont été une partie pivotal d’interaction w...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 0e23ff32-646d-43f3-8bec-6b2313d3abd6
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-bound-controls
msc.type: authoredcontent
ms.openlocfilehash: b115109c7307d05dc9e620378a51a71407204740
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41839035"
---
<a name="data-bound-controls"></a>Contrôles liés aux données
====================
par [Microsoft](https://github.com/microsoft)

> La plupart des applications ASP.NET s’appuient sur une certaine de présentation des données à partir d’une source de données back-end. Contrôles liés aux données ont été une partie essentielle de manipulation des données dans des applications Web dynamiques. ASP.NET 2.0 introduit des améliorations importantes aux contrôles liés aux données, notamment une nouvelle classe BaseDataBoundControl et la syntaxe déclarative.


La plupart des applications ASP.NET s’appuient sur une certaine de présentation des données à partir d’une source de données back-end. Contrôles liés aux données ont été une partie essentielle de manipulation des données dans des applications Web dynamiques. ASP.NET 2.0 introduit des améliorations importantes aux contrôles liés aux données, notamment une nouvelle classe BaseDataBoundControl et la syntaxe déclarative.

Le BaseDataBoundControl agit comme classe de base pour la classe DataBoundControl et la classe HierarchicalDataBoundControl. Dans ce module, nous allons étudier les classes suivantes qui dérivent de DataBoundControl :

- AdRotator
- Contrôles de liste
- GridView
- FormView
- DetailsView

Nous aborderons également les classes suivantes dérivent de HierarchicalDataBoundControl classe :

- TreeView
- Menu
- SiteMapPath

## <a name="databoundcontrol-class"></a>Classe de DataBoundControl

La classe DataBoundControl est une classe abstraite (marquée MustInherit dans VB) utilisée pour interagir avec tabulaires ou des données de liste style. Les contrôles suivants sont quelques-uns des contrôles qui dérivent de DataBoundControl.

## <a name="adrotator"></a>AdRotator

Le contrôle AdRotator vous permet d’afficher une bannière graphique sur une page Web qui est liée à une URL spécifique. Le graphique qui s’affiche est pivoté à l’aide de propriétés pour le contrôle. La fréquence d’un affichage ad particulier sur une page peut être configurée à l’aide de la **expositions** propriété et des publicités peuvent être filtrées à l’aide du filtrage de mots-clés.

Contrôles AdRotator destiné à un fichier XML ou une table dans une base de données. Les attributs suivants sont utilisés dans des fichiers XML pour configurer le contrôle AdRotator.

### <a name="imageurl"></a>ImageUrl
L’URL d’une image à afficher pour la publicité.

### <a name="navigateurl"></a>NavigateUrl
URL vers laquelle l’utilisateur doit être dirigé vers la suite d’un clic sur la publicité. Cela doit être codée URL.

### <a name="alternatetext"></a>AlternateText
Le texte de remplacement qui s’affiche dans une info-bulle et est lu par les lecteurs d’écran. Affiche également lorsque l’image spécifiée par la propriété ImageUrl n’est pas disponible.

### <a name="keyword"></a>Mot clé
Définit un mot clé qui peut être utilisé lorsque vous utilisez le filtrage de mots-clés. Si spécifié, seuls ces publicités avec un mot clé correspondant au filtre de mot clé seront affichera.

### <a name="impressions"></a>Expositions
Nombre de pondération qui détermine la fréquence à laquelle une publicité est susceptible d’apparaître. Il est relatif à l’impression d’autres annonces dans le même fichier. La valeur maximale des impressions collectives de toutes les annonces dans un fichier XML est 2,048,000,000 1.

### <a name="height"></a>Hauteur 
La hauteur de la publicité en pixels.

### <a name="width"></a>Largeur
La largeur de la publicité en pixels.


> [!NOTE]
> Les attributs de la hauteur et largeur remplacent la hauteur et la largeur du contrôle AdRotator lui-même.


Un fichier XML standard peut se présenter comme suit :

[!code-xml[Main](data-bound-controls/samples/sample1.xml)]

Dans l’exemple ci-dessus, la publicité pour Contoso est deux fois plus que susceptible d’apparaître en tant que la publicité pour le site Web ASP.NET en raison de la valeur de l’attribut d’Impressions.

Pour afficher des publicités à partir du fichier XML ci-dessus, ajoutez un contrôle AdRotator à une page et définissez la **AdvertisementFile** propriété pour pointer vers le fichier XML, comme indiqué ci-dessous :

[!code-aspx[Main](data-bound-controls/samples/sample2.aspx)]

Si vous choisissez d’utiliser une table de base de données comme source de données pour votre contrôle AdRotator, vous devez d’abord configurer une base de données en utilisant le schéma suivant :

| **Nom de colonne** | **Type de données** | **Description** |
| --- | --- | --- |
| Id | int | Clé primaire. Cette colonne peut porter n’importe quel nom. |
| ImageUrl | nvarchar (*longueur*) | URL relative ou absolue de l’image à afficher pour la publicité. |
| NavigateUrl | nvarchar (*longueur*) | L’URL cible pour la publicité. Si vous ne fournissez pas une valeur, la publicité n’est pas un lien hypertexte. |
| AlternateText | nvarchar (*longueur*) | Texte affiché si l’image est introuvable. Dans certains navigateurs, le texte est affiché en tant qu’une info-bulle. Texte de remplacement est également utilisé pour l’accessibilité afin que les utilisateurs qui ne peuvent pas consulter le graphique peuvent écouter la lecture de sa description. |
| Mot clé | nvarchar (*longueur*) | Une catégorie pour la publicité sur lequel pouvez filtrer la page. |
| Expositions | int (4) | Nombre qui indique la probabilité de la fréquence à laquelle la publicité est affichée. Le plus grand nombre, le plus souvent la publicité doit être affichée. Le total de toutes les valeurs dans le fichier XML ne peut pas dépasser 2 048 000 000-1. |
| Largeur | int (4) | La largeur de l’image en pixels. |
| Hauteur  | int (4) | La hauteur de l’image en pixels. |

Dans les cas où vous disposez déjà d’une base de données avec un schéma différent, vous pouvez utiliser la **AlternateTextField**, **ImageUrlField**, et **NavigateUrlField** propriétés pour mapper le Attributs de AdRotator à votre base de données existante. Pour afficher les données à partir de la base de données dans le contrôle AdRotator, ajoutez un contrôle de source de données à la page, configurez la chaîne de connexion pour le contrôle de source de données pointer vers votre base de données, définir du contrôle **DataSourceID** ID à la propriété du contrôle de source de données. Dans les cas où vous avez besoin pour configurer les publicités AdRotator par programmation, utilisez l’événement AdCreated. L’événement AdCreated prend deux paramètres ; un objet et l’autre instance de AdCreatedEventArgs. Le AdCreatedEventArgs est une référence à la publicité est en cours de création.

L’extrait de code suivant définit la ImageUrl, NavigateUrl et AlternateText pour une publicité par programmation :

[!code-csharp[Main](data-bound-controls/samples/sample3.cs)]

## <a name="list-controls"></a>Contrôles de liste

Contrôles de liste incluent ListBox, DropDownList, CheckBoxList, RadioButtonList et BulletedList. Chacun de ces contrôles peut être lié aux données d’une source de données. Ils utilisent un champ dans la source de données en tant que le texte d’affichage et que vous peuvent éventuellement utiliser un deuxième champ comme valeur d’un élément. Les éléments peuvent également être ajoutés de manière statique au moment du design, et vous pouvez combiner des éléments statiques et dynamiques éléments ajoutés à partir d’une source de données.

Pour les données lier un contrôle de liste, ajoutez un contrôle de source de données à la page. Spécifier une commande SELECT pour le contrôle de source de données, puis définissez la propriété DataSourceID du contrôle de liste à l’ID du contrôle de source de données. Utilisez le **DataTextField** et **DataValueField** propriétés pour définir le texte d’affichage et la valeur pour le contrôle. En outre, vous pouvez utiliser la **DataTextFormatString** propriété pour contrôler l’apparence du texte d’affichage comme suit :

| **Expression** | **Description** |
| --- | --- |
| Prix : {0:C} | Pour les données numériques/décimales. Affiche le littéral « prix : » suivie de nombres au format monétaire. Le format monétaire dépend du paramètre de culture spécifié dans l’attribut de culture sur la **Page** directive ou dans le fichier Web.config. |
| {0:D4} | Pour les données de type integer. Ne peut pas être utilisé avec des nombres décimaux. Les entiers sont affichés dans un champ de zéros qui a une largeur de quatre caractères. |
| {0:N2}% | Pour les données numériques. Affiche le nombre avec 2 décimales précision suivie le littéral « % ». |
| {0:000.0} | Pour les données numériques/décimales. Nombres sont arrondis à une décimale. Les nombres de moins de trois chiffres sont remplis de zéros. |
| {0:D} | Pour les données de date/heure. Format de date longue affiche (« Jeudi 06 août 1996 »). Le format de date dépend des paramètres de culture de la page ou du fichier Web.config. |
| {0:d} | Pour les données de date/heure. Mettre en forme affiche la date courte (« 31/12/99 »). |
| {0:yy-MM-dd} | Pour les données de date/heure. Affiche la date au format numérique année-mois-jour (96-08-06) |

## <a name="gridview"></a>GridView

Le contrôle GridView permet l’affichage de données tabulaires et de modification à l’aide d’une approche déclarative et est le successeur du contrôle DataGrid. Les fonctionnalités suivantes sont disponibles dans le contrôle GridView.

- Liaison aux données des contrôles de source, tels que SqlDataSource.
- Fonctionnalités intégrées de tri.
- Intégrés, la mise à jour et suppression de fonctionnalités.
- Fonctionnalités intégrées de pagination.
- Fonctionnalités de sélection de ligne intégré.
- Accès par programme au modèle objet GridView pour définir dynamiquement les propriétés, gérer les événements et ainsi de suite.
- Plusieurs champs de clé.
- Plusieurs champs de données pour les colonnes de lien hypertexte.
- Apparence personnalisable grâce aux thèmes et styles.

**Champs de colonne**

Chaque colonne dans le contrôle GridView est représentée par un objet de DataControlField. Par défaut, la propriété AutoGenerateColumns est définie **true**, ce qui crée un objet AutoGeneratedField pour chaque champ dans la source de données. Chaque champ est ensuite rendu en tant que colonne dans le contrôle GridView dans l’ordre dans lequel chaque champ s’affiche dans la source de données. Vous pouvez également contrôler les colonnes de champs s’affichent dans le **GridView** contrôle en définissant le **AutoGenerateColumns** propriété **false** et en définissant votre propre collection de champs de colonne. Types de champs de colonne différents déterminent le comportement des colonnes dans le contrôle.

Le tableau suivant répertorie les types de champs de colonne différente qui peuvent être utilisés.

| **Type de champ de colonne** | **Description** |
| --- | --- |
| BoundField | Affiche la valeur d’un champ dans une source de données. Il s’agit du type de colonne par défaut du contrôle GridView. |
| ButtonField | Affiche un bouton de commande pour chaque élément dans le contrôle GridView. Cela vous permet de créer une colonne de contrôles boutons personnalisés, tels que le boutons Ajouter ou supprimer. |
| CheckBoxField | Affiche une case à cocher pour chaque élément dans le contrôle GridView. Ce type de champ de colonne est couramment utilisé pour afficher les champs avec une valeur booléenne. |
| CommandField | Affiche des boutons de commande pour effectuer la sélection, la modification ou la suppression des opérations prédéfinis. |
| HyperLinkField | Affiche la valeur d’un champ dans une source de données sous la forme d’un lien hypertexte. Ce type de champ de colonne vous permet de lier un deuxième champ vers les URL du lien hypertexte. |
| ImageField | Affiche une image pour chaque élément dans le contrôle GridView. |
| TemplateField | Affiche le contenu défini par l’utilisateur pour chaque élément dans le contrôle GridView selon un modèle spécifié. Ce type de champ de colonne vous permet de créer un champ de colonne personnalisé. |

Pour définir une collection de champs de colonne de façon déclarative, ajoutez d’abord l’ouvrantes et fermantes **&lt;colonnes&gt;** balises entre les balises d’ouverture et fermeture du contrôle GridView. Ensuite, répertoriez les champs de colonne que vous souhaitez inclure entre les balises **&lt;colonnes&gt;** balises. Les colonnes spécifiées sont ajoutées à la collection de colonnes dans l’ordre indiqué. Le **colonnes** collection stocke tous les champs dans le contrôle de la colonne et vous permet de gérer par programme les champs de colonne dans le contrôle GridView.

Les champs de colonnes déclarés explicitement peuvent figurer dans la combinaison de champs de colonne généré automatiquement. Lorsque les deux sont utilisés, les champs de colonnes déclarés explicitement sont rendus en premier, suivis des champs de colonne généré automatiquement.

## <a name="binding-to-data"></a>Liaison à des données

Le contrôle GridView peut être lié à un contrôle de source de données (tel que **SqlDataSource**, **ObjectDataSource**, et ainsi de suite), ainsi que n’importe quelle source de données qui implémente le System.Collections.IEnumerable interface (par exemple, System.Data.DataView, System.Collections.ArrayList ou System.Collections.Hashtable). Utilisez une des méthodes suivantes pour lier le contrôle GridView pour le type de source de données approprié :

- Pour lier à un contrôle de source de données, définissez la propriété DataSourceID du contrôle GridView à la valeur d’ID du contrôle de source de données. Le contrôle GridView est lié au contrôle de source de données spécifié automatiquement et peut tirer parti de la source de données des fonctionnalités du contrôle pour effectuer le tri, la mise à jour, la suppression et la fonctionnalité de pagination. Il s’agit de la méthode recommandée pour lier aux données.
- Pour lier à une source de données qui implémente l’interface System.Collections.IEnumerable, définir par programmation la propriété DataSource du contrôle GridView à la source de données, puis appelez la méthode DataBind. Lorsque vous utilisez cette méthode, le contrôle GridView ne fournit pas intégrées de tri, la mise à jour, suppression et d’échange. Vous devez fournir cette fonctionnalité vous-même.

## <a name="data-operations"></a>Opérations de données

Le contrôle GridView fournit plusieurs fonctionnalités intégrées qui permettent à l’utilisateur trier, mettre à jour, supprimer, sélectionner et parcourir les éléments dans le contrôle. Lorsque le contrôle GridView est lié à un contrôle de source de données, le contrôle GridView peut tirer parti de la source de données fonctionnalités du contrôle et fournir le tri automatique, la mise à jour et suppression de fonctionnalités.

> [!NOTE]
> Le contrôle GridView peut fournir la prise en charge pour le tri, la mise à jour et suppression avec d’autres types de sources de données ; Toutefois, vous devez fournir un gestionnaire d’événements approprié avec l’implémentation de ces opérations.


Tri permet à l’utilisateur de trier les éléments dans le contrôle GridView en ce qui concerne une colonne spécifique en cliquant sur l’en-tête de colonne. Pour activer le tri, la valeur est la propriété AllowSorting **true**.

Les fonctionnalités de la mise à jour, la suppression et la sélection automatique sont activées quand un bouton dans un **ButtonField** ou **TemplateField** champ de colonne, avec un nom de commande de « Modifier », « Delete » et « Select », respectivement, l’utilisateur clique. Le contrôle GridView peut ajouter automatiquement un **CommandField** champ de colonne avec un Edit, Delete ou sélectionnez bouton si la propriété AutoGenerateEditButton, AutoGenerateDeleteButton ou AutoGenerateSelectButton est définie à **true**, respectivement.

> [!NOTE]
> Insertion d’enregistrements dans la source de données n’est pas directement pris en charge par le contrôle GridView. Toutefois, il est possible d’insérer des enregistrements à l’aide du contrôle GridView conjointement avec le contrôle DetailsView ou FormView contrôle.


Au lieu d’afficher tous les enregistrements dans la source de données en même temps, le contrôle GridView peut scinder automatiquement les enregistrements en pages. Pour activer la pagination, définissez la propriété AllowPaging **true**.

## <a name="customizing-the-user-interface"></a>Personnalisation de l’Interface utilisateur

Vous pouvez personnaliser l’apparence du contrôle GridView en définissant les propriétés de style pour les différentes parties du contrôle. Le tableau suivant répertorie les différentes propriétés de style.

| **Propriété de style** | **Description** |
| --- | --- |
| AlternatingRowStyle | Les paramètres de style pour les lignes de données en alternance dans le contrôle GridView. Lorsque cette propriété est définie, les lignes de données sont affichées en alternance entre les paramètres RowStyle et **AlternatingRowStyle** paramètres. |
| EditRowStyle | Les paramètres de style pour la ligne en cours de modification dans le contrôle GridView. |
| EmptyDataRowStyle | Les paramètres de style pour la ligne de données vide affichée dans le contrôle GridView lorsque la source de données ne contient-elle pas d’enregistrements. |
| FooterStyle | Les paramètres de style pour la ligne de pied de page du contrôle GridView. |
| HeaderStyle | Les paramètres de style pour la ligne d’en-tête du contrôle GridView. |
| PagerStyle | Les paramètres de style pour la ligne de pagineur du contrôle GridView. |
| RowStyle | Les paramètres de style pour les lignes de données dans le contrôle GridView. Lorsque le **AlternatingRowStyle** est également définie, les lignes de données sont affichées en alternance entre le **RowStyle** paramètres et le **AlternatingRowStyle** paramètres. |
| SelectedRowStyle | Les paramètres de style pour la ligne sélectionnée dans le contrôle GridView. |

Vous pouvez également afficher ou masquer les différentes parties du contrôle. Le tableau suivant répertorie les propriétés qui contrôlent les parties sont affichés ou masqués.

| **Property** | **Description** |
| --- | --- |
| ShowFooter | Affiche ou masque la section de pied de page du contrôle GridView. |
| ShowHeader | Affiche ou masque la section d’en-tête du contrôle GridView. |

### <a name="events"></a>Événements

Le contrôle GridView fournit plusieurs événements que vous pouvez programmer. Cela permet d’exécuter une routine personnalisée lorsqu’un événement se produit. Le tableau suivant répertorie les événements pris en charge par le contrôle GridView.

| **Event** | **Description** |
| --- | --- |
| PageIndexChanged | Se produit lorsque l’utilisateur clique sur un des boutons du pagineur, mais une fois que le contrôle GridView gère l’opération de pagination. Cet événement est couramment utilisé lorsque vous avez besoin effectuer une tâche une fois que l’utilisateur navigue vers une autre page dans le contrôle. |
| PageIndexChanging en | Se produit lorsque l’utilisateur clique sur un des boutons du pagineur, mais avant que le contrôle GridView contrôle l’opération de pagination. Cet événement est souvent utilisé pour annuler l’opération de pagination. |
| RowCancelingEdit | Se produit lorsque l’utilisateur clique sur le bouton Annuler d’une ligne, mais avant que le contrôle GridView quitte le mode édition. Cet événement est souvent utilisé pour arrêter l’opération d’annulation. |
| RowCommand | Se produit lorsqu’un clic est effectué dans le contrôle GridView. Cet événement est souvent utilisé pour effectuer une tâche lorsqu’un clic est effectué dans le contrôle. |
| Syllabusgrid_rowcreated | Se produit lorsqu’une nouvelle ligne est créée dans le contrôle GridView. Cet événement est souvent utilisé pour modifier le contenu d’une ligne lorsque la ligne est créée. |
| RowDataBound | Se produit lorsqu’une ligne de données est liée aux données dans le contrôle GridView. Cet événement est souvent utilisé pour modifier le contenu d’une ligne lorsque la ligne est liée aux données. |
| RowDeleted | Se produit lorsque l’utilisateur clique sur le bouton Supprimer d’une ligne, mais une fois que le contrôle GridView supprime l’enregistrement de la source de données. Cet événement est souvent utilisé pour vérifier les résultats de l’opération de suppression. |
| RowDeleting | Se produit lorsque l’utilisateur clique sur le bouton Supprimer d’une ligne, mais avant le contrôle GridView contrôle supprime l’enregistrement à partir de la source de données. Cet événement est souvent utilisé pour annuler l’opération de suppression. |
| RowEditing | Se produit lorsque l’utilisateur clique sur le bouton de modification d’une ligne, mais avant que le contrôle GridView contrôle en mode édition. Cet événement est souvent utilisé pour annuler l’opération de modification. |
| RowUpdated | Se produit lorsque l’utilisateur clique sur le bouton de mise à jour d’une ligne, mais une fois que le contrôle GridView met à jour la ligne. Cet événement est souvent utilisé pour vérifier les résultats de l’opération de mise à jour. |
| RowUpdating | Se produit lorsque l’utilisateur clique sur le bouton de mise à jour d’une ligne, mais avant le contrôle GridView contrôle met à jour la ligne. Cet événement est souvent utilisé pour annuler l’opération de mise à jour. |
| SelectedIndexChanged | Se produit lorsque l’utilisateur clique sur le bouton de sélection d’une ligne, mais une fois que le contrôle GridView gère l’opération de sélection. Cet événement est souvent utilisé pour effectuer une tâche après avoir sélectionné une ligne dans le contrôle. |
| SelectedIndexChanging | Se produit lorsque l’utilisateur clique sur le bouton de sélection d’une ligne, mais avant que le contrôle GridView contrôle l’opération de sélection. Cet événement est souvent utilisé pour annuler l’opération de sélection. |
| Triés | Se produit lorsque l’utilisateur clique sur le lien hypertexte pour trier une colonne, mais une fois que le contrôle GridView gère l’opération de tri. Cet événement est couramment utilisé pour effectuer une tâche une fois que l’utilisateur clique sur un lien hypertexte pour trier une colonne. |
| Tri | Se produit lorsque l’utilisateur clique sur le lien hypertexte pour trier une colonne, mais avant que le contrôle GridView contrôle l’opération de tri. Cet événement est souvent utilisé pour annuler l’opération de tri ou exécuter une routine de tri personnalisée. |

## <a name="formview"></a>FormView

Le contrôle FormView est utilisé pour afficher un enregistrement unique d’une source de données. Il est similaire au contrôle DetailsView, mais il affiche des modèles définis par l’utilisateur au lieu de champs de ligne. Création de vos propres modèles offre davantage de souplesse pour contrôler comment les données sont affichées. Le contrôle FormView prend en charge les fonctionnalités suivantes :

- Liaison aux données des contrôles de source, tels que SqlDataSource et ObjectDataSource.
- Fonctionnalités intégrées d’insertion.
- Intégrés, la mise à jour et suppression de fonctionnalités.
- Fonctionnalités intégrées de pagination.
- Accès par programme au modèle objet FormView pour définir dynamiquement les propriétés, gérer les événements et ainsi de suite.
- Apparence personnalisable grâce aux modèles définis par l’utilisateur, les thèmes et styles.

## <a name="templates"></a>Modèles

Pour le contrôle FormView afficher le contenu, vous devez créer des modèles pour les différentes parties du contrôle. La plupart des modèles sont facultatifs ; Toutefois, vous devez créer un modèle pour le mode dans lequel le contrôle est configuré. Par exemple, un contrôle FormView qui prend en charge l’insertion d’enregistrements doit avoir un modèle d’élément insert défini. Le tableau suivant répertorie les différents modèles que vous pouvez créer.

| **Type de modèle** | **Description** |
| --- | --- |
| EditItemTemplate | Définit le contenu de la ligne de données lorsque le contrôle FormView est en mode édition. Ce modèle contient généralement des contrôles d’entrée et les boutons de commande avec laquelle l’utilisateur peut modifier un enregistrement existant. |
| EmptyDataTemplate | Définit le contenu de la ligne de données vide affichée lorsque le contrôle FormView est lié à une source de données qui ne contient-elle pas d’enregistrements. Ce modèle contient généralement pour avertir l’utilisateur que la source de données ne contient pas d’enregistrements. |
| FooterTemplate | Définit le contenu de la ligne de pied de page. Ce modèle contient généralement tout contenu supplémentaire que vous souhaitez afficher dans la ligne de pied de page. Comme alternative, vous pouvez simplement spécifier le texte à afficher dans la ligne de pied de page en définissant la propriété de texte pied page. |
| HeaderTemplate | Définit le contenu de la ligne d’en-tête. Ce modèle contient généralement tout contenu supplémentaire que vous souhaitez afficher dans la ligne d’en-tête. Comme alternative, vous pouvez simplement spécifier le texte à afficher dans la ligne d’en-tête en définissant la propriété HeaderText. |
| ItemTemplate | Définit le contenu de la ligne de données lorsque le contrôle FormView est en mode lecture seule. Ce modèle contient généralement du contenu pour afficher les valeurs d’un enregistrement existant. |
| InsertItemTemplate | Définit le contenu de la ligne de données lorsque le contrôle FormView est en mode insertion. Ce modèle contient généralement des contrôles d’entrée et les boutons de commande avec laquelle l’utilisateur peut ajouter un nouvel enregistrement. |
| PagerTemplate | Définit le contenu de la ligne de pagineur affichée lorsque la fonctionnalité de pagination est activée (lorsque la propriété AllowPaging a la valeur **true**). Ce modèle contient généralement des contrôles avec lequel l’utilisateur peut accéder à un autre enregistrement. |

Les contrôles d’entrée dans le modèle de modification d’élément et le modèle d’élément insert peuvent être liés aux champs d’une source de données à l’aide d’une expression de liaison bidirectionnelle. Ainsi, le contrôle FormView extraire automatiquement les valeurs du contrôle d’entrée pour une mise à jour ou l’opération d’insertion. Expressions de liaison bidirectionnelles permettent également des contrôles d’entrée dans un modèle d’élément de modification pour afficher automatiquement les valeurs de champ d’origine.

### <a name="binding-to-data"></a>Liaison à des données

Le contrôle FormView peut être lié à un contrôle de source de données (tel que **SqlDataSource**, AccessDataSource, **ObjectDataSource** et ainsi de suite), ou à n’importe quelle source de données qui implémente le Interface System.Collections.IEnumerable (par exemple, System.Data.DataView, System.Collections.ArrayList et System.Collections.Hashtable). Utilisez une des méthodes suivantes pour lier le contrôle FormView pour le type de source de données approprié :

- Pour lier à un contrôle de source de données, définissez la propriété DataSourceID du contrôle FormView pour la valeur d’ID du contrôle de source de données. Le contrôle FormView automatiquement lie au contrôle de source de données spécifié et peut tirer parti de la source de données des fonctionnalités du contrôle pour effectuer l’insertion, la mise à jour, suppression et la fonctionnalité de pagination. Il s’agit de la méthode recommandée pour lier aux données.
- Pour lier à une source de données qui implémente le **System.Collections.IEnumerable** interface et définir par programmation la propriété DataSource du contrôle FormView pour la source de données, puis appelez la méthode DataBind. Lorsque vous utilisez cette méthode, le contrôle FormView ne fournit pas intégrées d’insertion, la mise à jour, la suppression et la fonctionnalité de pagination. Vous devez fournir cette fonctionnalité à l’aide de l’événement approprié.

## <a name="data-operations"></a>Opérations de données

Le contrôle FormView fournit plusieurs fonctionnalités intégrées qui permettent à l’utilisateur à mettre à jour, supprimer, insérer et parcourir les éléments dans le contrôle. Lorsque le contrôle FormView est lié à un contrôle de source de données, le contrôle FormView permettre tirer parti de la source de données fonctionnalités du contrôle et fournir automatique la mise à jour, suppression, insertion et d’échange. Le contrôle FormView prennent en charge update, delete, insert et opérations de pagination avec d’autres types de sources de données ; Toutefois, vous devez fournir un gestionnaire d’événements approprié avec l’implémentation de ces opérations.

Étant donné que le contrôle FormView utilise des modèles, il ne fournit pas un moyen de générer automatiquement des boutons de commande pour effectuer la mise à jour, la suppression ou l’insertion d’opérations. Vous devez inclure manuellement ces boutons de commande dans le modèle approprié. Le contrôle FormView reconnaît certains boutons qui ont leur **CommandName** propriétés ont des valeurs spécifiques. Le tableau suivant répertorie les boutons de commande qui reconnaît le contrôle FormView.

| **Button** | **Valeur de CommandName** | **Description** |
| --- | --- | --- |
| Annuler | « Annuler » | Utilisé dans la mise à jour ou d’opérations d’insertion pour annuler l’opération et ignorer les valeurs entrées par l’utilisateur. Le contrôle FormView retourne ensuite au mode spécifié par la propriété DefaultMode. |
| Supprimer | "Delete" | Utilisé dans les opérations de suppression pour supprimer l’enregistrement affiché à partir de la source de données. Déclenche les événements ItemDeleting et ItemDeleted. |
| Modifier | « Modifier » | Utilisé dans les opérations de mise à jour pour que le contrôle FormView en mode édition. Le contenu spécifié dans le **EditItemTemplate** propriété s’affiche pour la ligne de données. |
| Insert | « Insert » | Utilisé dans les opérations d’insertion pour tenter d’insérer un nouvel enregistrement dans la source de données en utilisant les valeurs fournies par l’utilisateur. Déclenche les événements ItemInserting et ItemInserted. |
| Nouveau | « Nouveau » | Utilisé dans les opérations d’insertion pour placer le contrôle FormView en mode insertion. Le contenu spécifié dans le **InsertItemTemplate** propriété s’affiche pour la ligne de données. |
| Page | « Page » | Utilisé dans les opérations de pagination pour représenter un bouton dans la ligne de pagineur qui effectue la pagination. Pour spécifier l’opération de pagination, définissez la **CommandArgument** propriété du bouton « Suivant », « Précédent », « First », « Dernier » ou l’index de la page vers laquelle naviguer. Déclenche les événements PageIndexChanging en et PageIndexChanged. |
| Mise à jour | « Update » | Utilisé dans les opérations de mise à jour pour tenter de mettre à jour l’enregistrement affiché dans la source de données avec les valeurs fournies par l’utilisateur. Déclenche les événements ItemUpdating et ItemUpdated. |

Contrairement à la suppression bouton (ce qui supprime immédiatement l’enregistrement affiché), lorsque l’utilisateur clique sur le bouton Modifier ou nouveau, le contrôle FormView contrôle passe à modifier, ou en mode insertion, respectivement. En mode édition, le contenu contenus dans le **EditItemTemplate** propriété s’affiche pour l’élément de données actuel. En règle générale, le modèle d’élément de modification est défini tel que le bouton Modifier est remplacé par une mise à jour et un bouton Annuler. Les contrôles d’entrée qui sont appropriés pour le type de données du champ (par exemple, une zone de texte ou un contrôle de case à cocher) sont également généralement affichés avec la valeur d’un champ pour l’utilisateur à modifier. En cliquant sur le bouton de mise à jour met à jour l’enregistrement dans la source de données, tandis que le bouton Annuler abandonne toutes les modifications.

De même, le contenu de la **InsertItemTemplate** propriété s’affiche pour l’élément de données lorsque le contrôle est en mode insertion. Le modèle d’élément insert est défini en général, telles que le nouveau bouton est remplacé par une instruction Insert et un bouton Annuler, et les contrôles d’entrée vides sont affichés pour l’utilisateur à entrer les valeurs pour le nouvel enregistrement. En cliquant sur le bouton Insérer d’insère l’enregistrement dans la source de données, tandis que le bouton Annuler abandonne toutes les modifications.

Le contrôle FormView fournit une fonctionnalité de pagination qui permet à l’utilisateur accéder à d’autres enregistrements dans la source de données. Lorsque l’option est activée, une ligne de pagineur est affichée dans le contrôle FormView qui contient les contrôles de navigation de page. Pour activer la pagination, définissez la **AllowPaging** propriété **true**. Vous pouvez personnaliser la ligne de pagineur en définissant les propriétés des objets contenus dans le PagerStyle et la propriété PagerSettings. Au lieu d’utiliser l’interface utilisateur de ligne de pagineur intégrée, vous pouvez créer votre propre interface utilisateur à l’aide de la **PagerTemplate** propriété.

## <a name="customizing-the-user-interface"></a>Personnalisation de l’Interface utilisateur

Vous pouvez personnaliser l’apparence du contrôle FormView en définissant les propriétés de style pour les différentes parties du contrôle. Le tableau suivant répertorie les différentes propriétés de style.

| **Propriété de style** | **Description** |
| --- | --- |
| EditRowStyle | Les paramètres de style pour la ligne de données lorsque le contrôle FormView est en mode d’édition. |
| EmptyDataRowStyle | Les paramètres de style pour la ligne de données vide affichée dans le contrôle FormView lorsque la source de données ne contient-elle pas d’enregistrements. |
| FooterStyle | Les paramètres de style pour la ligne de pied de page du contrôle FormView. |
| HeaderStyle | Les paramètres de style pour la ligne d’en-tête du contrôle FormView. |
| InsertRowStyle | Les paramètres de style pour la ligne de données lorsque le contrôle FormView est en mode d’insertion. |
| PagerStyle | Les paramètres de style pour la ligne de pagineur affichée dans le contrôle FormView lorsque la fonctionnalité de pagination est activée. |
| RowStyle | Les paramètres de style pour la ligne de données lorsque le contrôle FormView est en mode lecture seule. |

## <a name="events"></a>Événements

Le contrôle FormView fournit plusieurs événements que vous pouvez programmer. Cela permet d’exécuter une routine personnalisée lorsqu’un événement se produit. Le tableau suivant répertorie les événements pris en charge par le contrôle FormView.

| **Event** | **Description** |
| --- | --- |
| ItemCommand | Se produit lorsque l’utilisateur clique sur un bouton dans un contrôle FormView. Cet événement est souvent utilisé pour effectuer une tâche lorsqu’un clic est effectué dans le contrôle. |
| ItemCreated | Se produit une fois que tous les objets FormViewRow sont créés dans le contrôle FormView. Cet événement est souvent utilisé pour modifier les valeurs d’un enregistrement avant son affichage. |
| ItemDeleted | Se produit lorsqu’un bouton Supprimer (un bouton avec son **CommandName** propriété la valeur « Delete ») est activé, mais une fois que le contrôle FormView supprime l’enregistrement de la source de données. Cet événement est souvent utilisé pour vérifier les résultats de l’opération de suppression. |
| ItemDeleting | Se produit lorsque l’utilisateur clique sur un bouton Supprimer, mais avant le contrôle FormView contrôle supprime l’enregistrement à partir de la source de données. Cet événement est souvent utilisé pour annuler l’opération de suppression. |
| ItemInserted | Se produit lorsqu’un bouton d’insertion (un bouton avec son **CommandName** propriété définie sur « Insert ») est activé, mais une fois que le contrôle FormView insère l’enregistrement. Cet événement est souvent utilisé pour vérifier les résultats de l’opération d’insertion. |
| ItemInserting | Se produit lorsque l’utilisateur clique sur un bouton d’insertion, mais avant le contrôle FormView contrôle insère l’enregistrement. Cet événement est souvent utilisé pour annuler l’opération d’insertion. |
| ItemUpdated | Se produit lorsqu’un bouton de mise à jour (un bouton avec son **CommandName** propriété définie sur « Update ») est activé, mais une fois que le contrôle FormView met à jour la ligne. Cet événement est souvent utilisé pour vérifier les résultats de l’opération de mise à jour. |
| ItemUpdating | Se produit lorsque l’utilisateur clique sur un bouton Mettre à jour, mais avant le contrôle FormView contrôle met à jour l’enregistrement. Cet événement est souvent utilisé pour annuler l’opération de mise à jour. |
| ModeChanged | Se produit après que le contrôle FormView modifie des modes (pour le mode édition, insertion ou en lecture seule). Cet événement est souvent utilisé pour effectuer une tâche lorsque le contrôle FormView modifie des modes. |
| ModeChanging | Se produit avant que le contrôle FormView modifie des modes (pour le mode édition, insertion ou en lecture seule). Cet événement est souvent utilisé pour annuler un changement de mode. |
| PageIndexChanged | Se produit lorsque l’utilisateur clique sur un des boutons du pagineur, mais une fois que le contrôle FormView gère l’opération de pagination. Cet événement est couramment utilisé lorsque vous avez besoin effectuer une tâche une fois que l’utilisateur navigue vers un autre enregistrement dans le contrôle. |
| PageIndexChanging en | Se produit lorsque l’utilisateur clique sur un des boutons du pagineur, mais avant que le contrôle FormView contrôle l’opération de pagination. Cet événement est souvent utilisé pour annuler l’opération de pagination. |

## <a name="detailsview"></a>DetailsView

Le contrôle DetailsView est utilisé pour afficher un enregistrement unique d’une source de données dans une table, où chaque champ de l’enregistrement s’affiche dans une ligne de la table. Il peut être utilisé en association avec un contrôle GridView pour les scénarios maître / détail. Le contrôle DetailsView prend en charge les fonctionnalités suivantes :

- Liaison aux données des contrôles de source, tels que SqlDataSource.
- Fonctionnalités intégrées d’insertion.
- Intégrés, la mise à jour et suppression de fonctionnalités.
- Fonctionnalités intégrées de pagination.
- Accès par programme au modèle d’objet DetailsView pour définir dynamiquement les propriétés, gérer les événements et ainsi de suite.
- Apparence personnalisable grâce aux thèmes et styles.

## <a name="row-fields"></a>Champs de ligne

Chaque ligne de données dans le contrôle DetailsView est créé en déclarant un contrôle de champ. Types de champs de ligne différentes déterminent le comportement des lignes dans le contrôle. Contrôles de champ dérivent de DataControlField. Le tableau suivant répertorie les types de champs de ligne différentes qui peuvent être utilisés.

| **Type de champ de colonne** | **Description** |
| --- | --- |
| BoundField | Affiche la valeur d’un champ dans une source de données sous forme de texte. |
| ButtonField | Affiche un bouton de commande dans le contrôle DetailsView. Cela vous permet d’afficher une ligne avec un contrôle bouton personnalisé, par exemple un bouton Ajouter ou supprimer. |
| CheckBoxField | Affiche une case à cocher dans le contrôle DetailsView. Ce type de champ de ligne est couramment utilisé pour afficher les champs avec une valeur booléenne. |
| CommandField | Affiche la commande intégrée boutons permettent d’effectuer les modifier, insérer ou supprimer des opérations dans le contrôle DetailsView. |
| HyperLinkField | Affiche la valeur d’un champ dans une source de données sous la forme d’un lien hypertexte. Ce type de champ de ligne vous permet de lier un deuxième champ vers les URL du lien hypertexte. |
| ImageField | Affiche une image dans le contrôle DetailsView. |
| TemplateField | Affiche le contenu défini par l’utilisateur pour une ligne dans le contrôle DetailsView selon un modèle spécifié. Ce type de champ de ligne vous permet de créer un champ de ligne personnalisé. |

Par défaut, la propriété AutoGenerateRows a **true**, ce qui génère automatiquement un objet de champ de ligne dépendant pour chaque champ d’un type pouvant être lié dans la source de données. Les types valides pouvant être liés sont String, DateTime, Decimal, Guid et l’ensemble des types primitifs. Chaque champ est ensuite affiché dans une ligne sous forme de texte, dans l’ordre dans lequel chaque champ s’affiche dans la source de données.

Génération automatique des lignes fournit un moyen simple et rapide pour afficher chaque champ dans l’enregistrement. Toutefois, pour utiliser le contrôle DetailsView contrôle des fonctionnalités avancées que vous devez déclarer explicitement les champs de ligne à inclure dans le contrôle DetailsView. Pour déclarer les champs de ligne, attribuez d’abord le **AutoGenerateRows** propriété **false**. Ensuite, ajoutez ouvrantes et fermantes **&lt;champs&gt;** balises entre les balises d’ouverture et fermeture du contrôle DetailsView. Enfin, répertoriez les champs de ligne que vous souhaitez inclure entre les balises **&lt;champs&gt;** balises. Les champs de ligne spécifiés sont ajoutés à la collection de champs dans l’ordre indiqué. Le **champs** collection vous permet de gérer par programme les champs de ligne dans le contrôle DetailsView.

> [!NOTE]
> Les champs ne sont pas ajoutés à la collection de champs de ligne générés automatiquement.


## <a name="binding-to-data"></a>Liaison à des données

Le contrôle DetailsView peut être lié à un contrôle de source de données, tel que **SqlDataSource** ou AccessDataSource, ou à n’importe quelle source de données qui implémente l’interface System.Collections.IEnumerable, telles que System.Data.DataView, System.Collections.ArrayList et System.Collections.Hashtable.

Utilisez une des méthodes suivantes pour lier le contrôle DetailsView pour le type de source de données approprié :

- Pour lier à un contrôle de source de données, définissez la propriété DataSourceID du contrôle DetailsView à la valeur d’ID du contrôle de source de données. Le contrôle DetailsView se lie automatiquement le contrôle de source de données spécifié. Il s’agit de la méthode recommandée pour lier aux données.
- Pour lier à une source de données qui implémente le **System.Collections.IEnumerable** interface, définir par programmation la propriété DataSource du contrôle DetailsView à la source de données et puis appelez la méthode DataBind.

## <a name="security"></a>Sécurité

Ce contrôle peut être utilisé pour afficher l’entrée d’utilisateur, ce qui peut inclure un script client malveillant. Vérifiez toutes les informations envoyées à partir d’un client pour le script exécutable, des instructions SQL ou autre code avant de les afficher dans votre application. ASP.NET fournit une fonctionnalité de validation de demande d’entrée pour bloquer les scripts et le HTML dans l’entrée d’utilisateur.

## <a name="data-operations"></a>Opérations de données

Le contrôle DetailsView fournit des fonctionnalités intégrées qui permettent à l’utilisateur à mettre à jour, supprimer, insérer et parcourir les éléments dans le contrôle. Lorsque le contrôle DetailsView est lié à un contrôle de source de données, le contrôle DetailsView permettre tirer parti de la source de données fonctionnalités du contrôle et fournir automatique la mise à jour, suppression, insertion et d’échange.

Le contrôle DetailsView prennent en charge update, delete, insert et opérations de pagination avec d’autres types de sources de données ; Toutefois, vous devez fournir l’implémentation pour ces opérations dans un gestionnaire d’événements approprié.

Le contrôle DetailsView peut ajouter automatiquement un **CommandField** champ de ligne avec un bouton Modifier, supprimer ou nouveau en définissant les propriétés AutoGenerateEditButton, AutoGenerateDeleteButton ou AutoGenerateInsertButton à **true**, respectivement. Contrairement à la suppression bouton (ce qui supprime immédiatement l’enregistrement sélectionné), lorsque l’utilisateur clique sur le bouton Modifier ou nouveau, le contrôle DetailsView contrôle passe à modifier ou mode insertion, respectivement. En mode édition, le bouton Modifier est remplacé par une mise à jour et un bouton Annuler. Les contrôles d’entrée qui sont appropriés pour le type de données du champ (par exemple, une zone de texte ou un contrôle de case à cocher) sont affichés avec la valeur d’un champ pour l’utilisateur à modifier. En cliquant sur le bouton de mise à jour met à jour l’enregistrement dans la source de données, tandis que le bouton Annuler abandonne toutes les modifications. De même, en mode d’insertion, le nouveau bouton est remplacé par une instruction Insert et un bouton Annuler, et les contrôles d’entrée vides sont affichés pour l’utilisateur à entrer les valeurs pour le nouvel enregistrement.

Le contrôle DetailsView fournit une fonctionnalité de pagination qui permet à l’utilisateur accéder à d’autres enregistrements dans la source de données. Lorsque l’option est activée, les contrôles de navigation de page sont affichés dans une ligne de pagineur. Pour activer la pagination, définissez la propriété AllowPaging **true**. La ligne de pagineur peut être personnalisée en utilisant les propriétés PagerStyle et PagerSettings.

## <a name="customizing-the-user-interface"></a>Personnalisation de l’Interface utilisateur

Vous pouvez personnaliser l’apparence du contrôle DetailsView en définissant les propriétés de style pour les différentes parties du contrôle. Le tableau suivant répertorie les différentes propriétés de style.

| **Propriété de style** | **Description** |
| --- | --- |
| AlternatingRowStyle | Les paramètres de style pour les lignes de données en alternance dans le contrôle DetailsView. Lorsque cette propriété est définie, les lignes de données sont affichées en alternance entre les paramètres RowStyle et **AlternatingRowStyle** paramètres. |
| CommandRowStyle | Les paramètres de style pour la ligne contenant les boutons de commande intégrée dans le contrôle DetailsView. |
| EditRowStyle | Les paramètres de style pour les lignes de données lorsque le contrôle DetailsView est en mode d’édition. |
| EmptyDataRowStyle | Les paramètres de style pour la ligne de données vide affichée dans le contrôle DetailsView lorsque la source de données ne contient-elle pas d’enregistrements. |
| FooterStyle | Les paramètres de style pour la ligne de pied de page du contrôle DetailsView. |
| HeaderStyle | Les paramètres de style pour la ligne d’en-tête du contrôle DetailsView. |
| InsertRowStyle | Les paramètres de style pour les lignes de données lorsque le contrôle DetailsView est en mode d’insertion. |
| PagerStyle | Les paramètres de style pour la ligne de pagineur du contrôle DetailsView. |
| RowStyle | Les paramètres de style pour les lignes de données dans le contrôle DetailsView. Lorsque le **AlternatingRowStyle** est également définie, les lignes de données sont affichées en alternance entre le **RowStyle** paramètres et le **AlternatingRowStyle** paramètres. |
| FieldHeaderStyle | Les paramètres de style pour la colonne d’en-tête du contrôle DetailsView. |

## <a name="events"></a>Événements

Le contrôle DetailsView fournit plusieurs événements que vous pouvez programmer. Cela permet d’exécuter une routine personnalisée lorsqu’un événement se produit. Le tableau suivant répertorie les événements pris en charge par le contrôle DetailsView. Le contrôle DetailsView hérite également ces événements de ses classes de base : liaison de données liée aux données, Disposed, Init, Load, PreRender et rendu.

| **Event** | **Description** |
| --- | --- |
| ItemCommand | Se produit lorsqu’un clic est effectué dans le contrôle DetailsView. |
| ItemCreated | Se produit une fois que tous les objets DetailsViewRow sont créés dans le contrôle DetailsView. Cet événement est souvent utilisé pour modifier les valeurs d’un enregistrement avant son affichage. |
| ItemDeleted | Se produit lorsque l’utilisateur clique sur un bouton Supprimer, mais une fois que le contrôle DetailsView supprime l’enregistrement de la source de données. Cet événement est souvent utilisé pour vérifier les résultats de l’opération de suppression. |
| ItemDeleting | Se produit lorsque l’utilisateur clique sur un bouton Supprimer, mais avant le contrôle DetailsView contrôle supprime l’enregistrement à partir de la source de données. Cet événement est souvent utilisé pour annuler l’opération de suppression. |
| ItemInserted | Se produit lorsque l’utilisateur clique sur un bouton d’insertion, mais une fois que le contrôle DetailsView insère l’enregistrement. Cet événement est souvent utilisé pour vérifier les résultats de l’opération d’insertion. |
| ItemInserting | Se produit lorsque l’utilisateur clique sur un bouton d’insertion, mais avant le contrôle DetailsView contrôle insère l’enregistrement. Cet événement est souvent utilisé pour annuler l’opération d’insertion. |
| ItemUpdated | Se produit lorsque l’utilisateur clique sur un bouton Mettre à jour, mais une fois que le contrôle DetailsView met à jour la ligne. Cet événement est souvent utilisé pour vérifier les résultats de l’opération de mise à jour. |
| ItemUpdating | Se produit lorsque l’utilisateur clique sur un bouton Mettre à jour, mais avant le contrôle DetailsView contrôle met à jour l’enregistrement. Cet événement est souvent utilisé pour annuler l’opération de mise à jour. |
| ModeChanged | Se produit une fois que le contrôle DetailsView modifie des modes (mode édition, insertion ou en lecture seule). Cet événement est souvent utilisé pour effectuer une tâche lorsque le contrôle DetailsView modifie des modes. |
| ModeChanging | Se produit avant le contrôle DetailsView modifie des modes (mode édition, insertion ou en lecture seule). Cet événement est souvent utilisé pour annuler un changement de mode. |
| PageIndexChanged | Se produit lorsque l’utilisateur clique sur un des boutons du pagineur, mais une fois que le contrôle DetailsView gère l’opération de pagination. Cet événement est couramment utilisé lorsque vous avez besoin effectuer une tâche une fois que l’utilisateur navigue vers un autre enregistrement dans le contrôle. |
| PageIndexChanging en | Se produit lorsque l’utilisateur clique sur un des boutons du pagineur, mais avant que le contrôle DetailsView contrôle l’opération de pagination. Cet événement est souvent utilisé pour annuler l’opération de pagination. |

## <a name="the-menu-control"></a>Le contrôle de Menu

Le contrôle de Menu dans ASP.NET 2.0 est conçu pour être un système complet de navigation. Il peut être lié aux données facilement à des sources de données hiérarchiques tels que SiteMapDataSource.

Une structure de contrôles de Menu peut être définie de manière déclarative ou dynamiquement et se compose d’un nœud racine unique et un nombre quelconque de sous-nœuds. Le code suivant définit de manière déclarative un menu pour le contrôle de Menu.

[!code-aspx[Main](data-bound-controls/samples/sample4.aspx)]

Dans l’exemple ci-dessus, le nœud Home.aspx est le nœud racine. Tous les autres nœuds sont imbriqués dans le nœud racine à différents niveaux.

Il existe deux types de menus du contrôle de Menu peut afficher ; les menus statiques et des menus dynamiques. Les menus statiques se composent des éléments de menu sont toujours visibles. Menus dynamiques se composent des éléments de menu qui sont visibles uniquement lorsque l’utilisateur pointe dessus avec la souris. Les clients peuvent souvent confondez pas avec les menus définis de façon déclarative les menus statiques et des menus dynamiques avec les menus qui sont liés lors de l’exécution. En fait, les menus dynamiques et statiques ne sont pas liées à la méthode de remplissage. Les termes du contrat *statique* et *dynamique* faire uniquement si le menu est statiquement affichée par défaut ou uniquement affichée lorsque l’utilisateur exécute une action.

Le **StaticDisplayLevels** propriété est utilisée pour configurer le nombre de niveaux du menu est statiques et par conséquent s’affichent par défaut. Dans l’exemple ci-dessus, en définissant le **StaticDisplayLevels** propriété une valeur de 2 entraînerait le menu à afficher statiquement l’accueil du nœud de la musique, nœud et le films. Tous les autres nœuds seraient dynamiquement affichés lorsque l’utilisateur pointe sur le nœud parent.

Le **MaximumDynamicDisplayLevels** propriété configure le nombre maximal de niveaux dynamiques le menu est capable d’afficher. Aucun menu dynamique à un niveau supérieur à la valeur spécifiée par le **MaximumDynamicDisplayLevels** propriété sont ignorées.

> [!NOTE]
> Il est presque certain que vous pouvez rencontrer des situations où menus n’apparaissent pas pour effectuer le rendu en raison de la propriété MaximumDynamicDisplayLevels. Dans ce cas, assurez-vous que la propriété est définie suffisamment pour permettre l’affichage des menus de clients.


## <a name="data-binding-the-menu-control"></a>Le contrôle de Menu de liaison de données

Le contrôle de Menu peut être lié à n’importe quelle source de données hiérarchiques tels que SiteMapDataSource ou XMLDataSource. SiteMapDataSource est le plus communément utilisée pour la liaison de données à un contrôle de Menu car il des flux sur le fichier Web.sitemap et son schéma fournit une API connue pour le contrôle de Menu. La liste ci-dessous montre un simple fichier Web.sitemap.

[!code-xml[Main](data-bound-controls/samples/sample5.xml)]

Notez qu’il n'existe qu’une seule racine siteMapNode élément, dans ce cas, l’élément d’accueil. Plusieurs attributs peuvent être configurés pour chaque noeud siteMapNode. Les attributs couramment utilisés sont :

- **URL** Spécifie l’URL à afficher lorsqu’un utilisateur clique sur l’élément de menu. Si cet attribut n’est pas présent, le nœud simplement valide lorsque vous cliquez sur.
- **titre** Spécifie le texte qui s’affiche sur l’élément de menu.
- **Description** utilisé en tant que documentation pour le nœud. Affiche également sous la forme d’une info-bulle lorsque la souris se trouve sur le nœud.
- **siteMapFile** permet sitemaps imbriquées. Cet attribut doit pointer vers un fichier de sitemap ASP.NET bien formé.
- **rôles** permet d’être contrôlé par l’ajustement de la sécurité ASP.NET pour l’apparence d’un nœud.

Notez que bien que ces attributs sont tous facultatifs, le comportement du menu peut-être pas ce qui est attendu si elles ne sont pas spécifiées. Par exemple, si le *url* attribut est spécifié mais le *description* attribut n’est pas, le nœud ne sera pas visible, et il n’y aura aucun moyen d’accéder à l’URL spécifiée.

## <a name="controlling-a-menus-operation"></a>Contrôle d’une opération de Menus

Il existe plusieurs propriétés qui affectent le fonctionnement d’un contrôle Menu ASP.NET ; le **Orientation** propriété, le **DisappearAfter** propriété, le **StaticItemFormatString** propriété et le **StaticPopoutImageUrl**propriété sont quelques-unes d'entre elles.

- Le **Orientation** peut être définie avec la valeur *horizontal* ou *vertical* et détermine si les éléments de menu statiques sont disposées horizontalement dans une ligne ou verticalement et empilées sur l’autre. Cette propriété n’affecte pas les menus dynamiques.
- Le **DisappearAfter** propriété configure la durée pendant laquelle un menu dynamique doit rester visible une fois que la souris a été déplacé en dehors de celui-ci. La valeur est spécifiée en millisecondes et la valeur par défaut est 500. Définition de cette propriété sur une valeur de -1 entraîne le menu jamais disparaître automatiquement. Dans ce cas, le menu disparaissent uniquement lorsque l’utilisateur clique en dehors du menu.
- Le **StaticItemFormatString** propriété rend facile à maintenir une formulation cohérente dans votre système de menus. Lorsque vous spécifiez cette propriété, *{0}* doit être entré à la place de la description qui apparaît dans la source de données. Par exemple, afin de bénéficier de l’élément de menu à partir de l’exercice 1 say visitez notre Page de produits, etc., vous devez spécifier visitez notre {0} Page pour le StaticItemFormatString. Lors de l’exécution, ASP.NET remplace toutes les occurrences de {0} avec la description correcte pour l’élément de menu.
- Le **StaticPopoutImageUrl** propriété spécifie l’image qui est utilisé pour indiquer qu’un nœud particulier de menu a des nœuds enfants qui sont accessibles en pointant dessus. Menus dynamiques continue à utiliser l’image par défaut.

## <a name="templated-menu-controls"></a>Contrôles de Menu basé sur un modèle

Le contrôle de Menu est un contrôle basé sur un modèle, tout en permettant à deux ItemTemplates différents ; le StaticItemTemplate et le DynamicItemTemplate. À l’aide de ces modèles, vous pouvez facilement ajouter des contrôles serveur ou des contrôles utilisateur à vos menus.

Pour modifier les modèles dans Visual Studio .NET, cliquez sur le bouton de la balise active dans le menu, puis choisissez Modifier les modèles. Vous pouvez ensuite choisir entre modifiant le StaticItemTemplate ou le DynamicItemTemplate.

Les contrôles ajoutés à la StaticItemTemplate apparaîtra dans le menu statique lorsque la page se charge. Les contrôles ajoutés à la DynamicItemTemplate apparaîtra dans tous les menus contextuels.

## <a name="menu-events"></a>Événements de menu

Le contrôle de Menu comporte deux événements qui sont uniques à cette dernière ; le **MenuItemClicked** et **MenuItemDatabound** événement.

L’événement MenuItemClicked est déclenché lorsque l’utilisateur clique sur un élément de menu. L’événement MenuItemDatabound est déclenché lorsqu’un élément de menu est lié aux données. Le **MenuEventArgs** qui lui est passé à l’événement Gestionnaire fournit l’accès à l’élément de menu par le biais de la propriété d’élément.

## <a name="controlling-a-menus-appearance"></a>Contrôler un aspect de Menus

Vous pouvez également affecter l’apparence d’un contrôle de menu à l’aide d’une ou plusieurs des nombreux styles disponibles dans les menus format. Parmi ceux-ci figurent **StaticMenuStyle**, **DynamicMenuStyle**, **DynamicMenuItemStyle**, **DynamicSelectedStyle**et **DynamicHoverStyle**. Ces propriétés sont configurées à l’aide d’une chaîne de HTML Style standard. Par exemple, le texte suivant affecte le style pour les menus dynamiques.

[!code-aspx[Main](data-bound-controls/samples/sample6.aspx)]

> [!NOTE]
> Si vous utilisez l’un des styles de pointage, vous devez ajouter un &lt;head&gt; élément dans la page avec le *runat* élément la valeur *server*.


Les contrôles menu prennent également en charge l’utilisation de thèmes d’ASP.NET 2.0.

## <a name="the-treeview-control"></a>Le contrôle TreeView

Le contrôle TreeView affiche des données dans une structure arborescente. Comme le contrôle de Menu, il peut être facilement lié aux données d’une source de données hiérarchiques SiteMapDataSource.

La première question que les clients sont susceptibles de poser à propos du contrôle TreeView d’ASP.NET 2.0 est ou non il concerne le contrôle TreeView IE WebControl qui était disponible pour ASP.NET 1.x. Il n’est pas le cas. Le contrôle TreeView d’ASP.NET 2.0 a été écrit à partir de zéro et offre une amélioration significative par rapport à la TreeView WebControl IE qui était précédemment disponible.

Je n’aborde en détail comment lier un contrôle TreeView à un plan de site tel qu’il est exécuté dans la même façon que le contrôle de Menu. Toutefois, le contrôle TreeView présente des différences distinctes dans la façon dont il fonctionne.

Par défaut, un contrôle TreeView apparaît complètement développé. Pour modifier le niveau d’expansion lors du chargement initial, modifiez le **ExpandDepth** propriété du contrôle. Cela est particulièrement important dans les cas où le contrôle TreeView est lié aux données lors de l’expansion des nœuds particuliers.

## <a name="databinding-the-treeview-control"></a>Liaison de données au contrôle TreeView

Contrairement au contrôle de Menu, TreeView se prête bien à gérer de grandes quantités de données. Par conséquent, en plus de la liaison de données à un SiteMapDataSource ou à XMLDataSource, l’arborescence est souvent lié aux données d’un jeu de données ou d’autres données relationnelles. Dans les cas où le contrôle TreeView est lié à de grandes quantités de données, il est préférable à lier à des données qui sont réellement visibles dans le contrôle. Vous pouvez ensuite lier des données à des données supplémentaires comme nœuds TreeView sont développées.

Dans ce cas, le **PopulateOnDemand** propriété du contrôle TreeView doit être définie sur *true*. Vous devrez ensuite fournir une implémentation pour le **TreeNodePopulate** (méthode).

## <a name="data-binding-without-postback"></a>Liaison sans publication (postback) de données

Notez que lorsque vous développez un nœud dans l’exemple précédent pour la première fois, la page publie et actualise. Vous avez un problème n’est pas dans cet exemple, mais vous pouvez l’imaginer qu’il peut être dans un environnement de production avec une grande quantité de données. Un meilleur scénario serait dans lequel le contrôle TreeView est toujours dynamiquement remplir ses nœuds, mais sans une publication au serveur.

En définissant le **PopulateNodesFromClient** et **PopulateOnDemand** propriétés sur true, le contrôle TreeView d’ASP.NET remplira dynamiquement des nœuds sans publication. Lorsque le nœud parent est développé, une requête XMLHttp est effectuée à partir du client et l’événement OnTreeNodePopulate est déclenché. Le serveur répond avec un îlot de données XML qui est ensuite utilisé pour les données lie les nœuds enfants.

ASP.NET crée dynamiquement le code client qui implémente cette fonctionnalité. Le &lt;script&gt; balises qui contient le script sont générés qui pointe vers un fichier AXD. Par exemple, la liste ci-dessous répertorie les liens de script pour le code de script qui génère la requête XMLHttp.

[!code-html[Main](data-bound-controls/samples/sample7.html)]

Si vous parcourez le fichier AXD ci-dessus dans votre navigateur et que vous l’ouvrez, vous verrez le code qui implémente la demande XMLHttp. Cette méthode évite aux clients de modifier le fichier de script.

## <a name="controlling-the-operation-of-the-treeview-control"></a>Contrôle du fonctionnement du contrôle TreeView

Le contrôle TreeView possède plusieurs propriétés qui affectent le fonctionnement du contrôle. Les propriétés plus évidentes sont la **ShowCheckBoxes**, **ShowExpandCollapse**, et **ShowLines**.

Le **ShowCheckBoxes** propriété affecte les nœuds affichent une case à cocher lors du rendu ou non. Les valeurs valides pour cette propriété sont **aucun**, **racine**, **Parent**, **feuille**, et **tous les**. Ces modifications concernent le contrôle TreeView comme suit :

| **Valeur de propriété** | **Effet** |
| --- | --- |
| Aucun. | Cases à cocher ne sont pas affichés sur tous les nœuds. Il s'agit du paramètre par défaut. |
| Racine | Une case à cocher est affichée uniquement sur le nœud racine. |
| Parent | Une case à cocher est affichée uniquement sur ces nœuds ayant des nœuds enfants. Ces nœuds enfants peuvent être nœuds parents ou les nœuds terminaux. |
| Feuille | Une case à cocher s’affiche uniquement sur les nœuds qui possèdent pas de nœuds enfants. |
| Tous | Une case à cocher est affichée sur tous les nœuds. |

Lorsque les cases à cocher sont utilisés, le **CheckedNodes** propriété retournera une collection de nœuds TreeView qui sont vérifiées lors de la publication.

Le **ShowExpandCollapse** propriété contrôle l’apparence de l’image de développer/réduire à côté des nœuds racine et parent. Si cette propriété est définie sur **false**, les nœuds TreeView sont rendus sous forme de liens hypertexte et sont développé/réduit en cliquant sur le lien.

Le **ShowLines** propriété contrôle si les lignes sont affichées connectant les nœuds parents aux nœuds enfants. Lorsque **false** (la valeur par défaut), aucune ligne n’est affichés. Lorsque **true**, le contrôle TreeView utilise des images de lignes dans le dossier spécifié par le **LineImagesFolder** propriété.

Pour personnaliser l’apparence des lignes de TreeView, Visual Studio .NET 2005 inclut un outil de Concepteur de ligne. Vous pouvez accéder à cet outil à l’aide du bouton de la balise active sur le contrôle TreeView comme ci-dessous.


![](data-bound-controls/_static/image1.jpg)

**Figure 1**


Sélectionnez quand le **personnaliser les Images de ligne** option de menu, l’outil Concepteur de ligne sera lancé et vous pouvez configurer l’apparence des lignes du contrôle TreeView.

## <a name="treeview-events"></a>Événements de TreeView

Le contrôle TreeView présente les événements uniques suivants :

- SelectedNodeChanged se produit lorsqu’un nœud est sélectionné en fonction de la **SelectAction** propriété.
- TreeNodeCheckChanged se produit lorsqu’un état checkboxs des nœuds est modifié.
- Événements TreeNodeExpanded se produit lorsqu’un nœud est développé en fonction de la **SelectAction** propriété.
- TreeNodeCollapsed se produit lorsqu’un nœud est réduit.
- TreeNodeDataBound se produit lorsqu’un nœud est données lié.
- TreeNodePopulate se produit lorsqu’un nœud est rempli.

Le **SelectAction** propriété vous permet de configurer l’événement qui est déclenché lorsqu’un nœud est sélectionné. La propriété SelectAction contient les actions suivantes :

- TreeNodeSelectAction.Expand déclenche événements TreeNodeExpanded lorsque le nœud est sélectionné.
- TreeNodeSelectAction.None ne déclenche aucun événement lorsque le nœud est sélectionné.
- TreeNodeSelectAction.Select déclenche l’événement SelectedNodeChanged lorsque le nœud est sélectionné.
- TreeNodeSelectAction.SelectExpand déclenche l’événement SelectedNodeChanged et l’événement événements TreeNodeExpanded lorsque le nœud est sélectionné.

## <a name="controlling-appearance-with-styles"></a>Contrôle de l’apparence avec des Styles

Le contrôle TreeView fournit de nombreuses propriétés pour contrôler l’apparence du contrôle avec des styles. Les propriétés suivantes sont disponibles.

| **Nom de propriété** | **Contrôles** |
| --- | --- |
| HoverNodeStyle | Contrôle le style des nœuds lorsque la souris se trouve au-dessus d’eux. |
| LeafNodeStyle | Contrôle le style des nœuds terminaux. |
| NodeStyle | Contrôle le style pour tous les nœuds. Styles de nœud spécifique (par exemple, LeafNodeStyle) remplacent ce style. |
| ParentNodeStyle | Contrôle le style pour tous les nœuds parents. |
| RootNodeStyle | Contrôle le style pour le nœud racine. |
| SelectedNodeStyle | Contrôle le style du nœud sélectionné. |

Chacune de ces propriétés est en lecture seule. Toutefois, il s’agit de chaque retour un **TreeNodeStyle** objet et les propriétés de cet objet peuvent être modifié à l’aide de la *sous-propriétés de la propriété* format. Par exemple, pour définir le **ForeColor** propriété de la **SelectedNodeStyle**, vous utiliseriez la syntaxe suivante :

[!code-aspx[Main](data-bound-controls/samples/sample8.aspx)]

Notez que la balise ci-dessus n’est pas fermée. C’est parce que lorsque vous utilisez la syntaxe déclarative illustrée ici, vous devez inclure les nœuds des arborescences dans le code HTML également.

Les propriétés de style peuvent également être spécifiées dans le code à l’aide du *property.subproperty* format. Par exemple, pour définir le **ForeColor** propriété de la **RootNodeStyle** dans le code, vous utiliseriez la syntaxe suivante :

[!code-csharp[Main](data-bound-controls/samples/sample9.cs)]

> [!NOTE]
> Pour obtenir la liste complète des propriétés de style différent, consultez la documentation MSDN sur l’objet TreeNodeStyle.


## <a name="the-sitemappath-control"></a>Le contrôle SiteMapPath

Le contrôle SiteMapPath fournit un contrôle de navigation de navigation de pain pour les développeurs ASP.NET. Comme les autres contrôles de navigation, il peut être facilement lié aux données à des données hiérarchiques sources telles que le SiteMapDataSource ou XmlDataSource.

Un contrôle SiteMapPath est composé d’objets de SiteMapNodeItem. Il existe trois types de nœuds ; le nœud racine, les nœuds parents et le nœud actuel. Le nœud racine est le nœud situé en haut de la structure hiérarchique. Le nœud actuel représente la page actuelle. Tous les autres nœuds sont des nœuds parents.

## <a name="controlling-the-operation-of-the-sitemappath-control"></a>Contrôle du fonctionnement du contrôle SiteMapPath

Les propriétés qui contrôlent le fonctionnement du contrôle SiteMapPath sont comme suit :

| **Property** | **Description de propriété** |
| --- | --- |
| ParentLevelsDisplayed | Contrôle le nombre de nœuds parents est affiché. La valeur par défaut est -1 n’impose aucune restriction sur le nombre de nœuds parents affichés. |
| PathDirection | Contrôle la direction du contrôle SiteMapPath. Les valeurs valides sont RootToCurrent (valeur par défaut) et CurrentToRoot. |
| PathSeparator | Chaîne qui contrôle le caractère qui sépare les nœuds dans un contrôle SiteMapPath. La valeur par défaut est :. |
| RenderCurrentNodeAsLink | Valeur booléenne qui contrôle si ou non le nœud actuel est rendu sous forme de lien. La valeur par défaut est False. |
| SkipLinkText | Aide à des fonctionnalités d’accessibilité lors de la page est affichée par les lecteurs d’écran. Cette propriété autorise les lecteurs d’écran d’ignorer le contrôle SiteMapPath. Pour désactiver cette fonctionnalité, définissez la propriété sur String.Empty. |

## <a name="templated-sitemappath-controls"></a>Contrôles SiteMapPath basé sur un modèle

Le SiteMapControl est un contrôle basé sur un modèle, et par conséquent, vous pouvez définir différents modèles pour une utilisation dans l’affichage du contrôle. Pour modifier les modèles dans un contrôle SiteMapPath, cliquez sur le bouton de la balise active sur le contrôle et choisissez Modifier les modèles dans le menu. Cela affiche le menu SiteMapTasks comme indiqué ci-dessous dans laquelle vous pouvez choisir entre les différents modèles disponibles.


![](data-bound-controls/_static/image2.jpg)

**Figure 2**


Le **NodeTemplate** modèle fait référence à n’importe quel nœud dans le contrôle SiteMapPath. Si le nœud est un nœud racine ou le nœud actuel et **RootNodeTemplate** ou **CurrentNodeTemplate** est configuré, le NodeTemplate est remplacée.

## <a name="sitemappath-events"></a>Événements SiteMapPath

Le contrôle SiteMapPath comporte deux événements qui ne sont pas dérivés de la classe du contrôle ; le **ItemCreated** événement et le **ItemDataBound** événement. L’événement ItemCreated est déclenché lorsqu’un élément SiteMapPath est créé. ItemDataBound est déclenché lorsque la méthode DataBind est appelée pendant la liaison de données d’un nœud SiteMapPath. Un **SiteMapNodeItemEventArgs** objet permet d’accéder à la SiteMapNodeItem spécifique par le biais de la propriété d’élément.

## <a name="controlling-appearance-with-styles"></a>Contrôle de l’apparence avec des Styles

Les styles suivants sont disponibles pour la mise en forme d’un contrôle SiteMapPath.

| **Nom de propriété** | **Contrôles** |
| --- | --- |
| CurrentNodeStyle | Contrôle le style du texte du nœud actuel. |
| RootNodeStyle | Contrôle le style du texte pour le nœud racine. |
| NodeStyle | Contrôle le style du texte pour tous les nœuds en supposant qu’un CurrentNodeStyle ou le RootNodeStyle ne s’applique pas. |

La propriété NodeStyle est remplacée par le CurrentNodeStyle ou le RootNodeStyle. Chacune de ces propriétés est en lecture seule et retourne un **Style** objet. Pour modifier l’apparence d’un nœud à l’aide d’une de ces propriétés, vous devez définir les propriétés de l’objet de Style qui est retourné. Par exemple, le code ci-dessous modifie la propriété de couleur de premier plan du nœud actuel.

[!code-aspx[Main](data-bound-controls/samples/sample10.aspx)]

La propriété peut également être appliquée par programmation comme suit :

[!code-csharp[Main](data-bound-controls/samples/sample11.cs)]

> [!NOTE]
> Si un modèle est appliqué, il se peut que le style ne sera pas appliqué.


## <a name="lab-1-configuring-an-aspnet-menu-control"></a>Atelier pratique 1 : Configuration d’un contrôle de Menu ASP.NET

1. Créer un nouveau site Web.
2. Ajoutez un fichier de plan de Site en sélectionnant le fichier, nouveau, fichier, plan de Site à partir de la liste des modèles de fichier.
3. Ouvrez le plan du site (Web.sitemap par défaut) et modifiez-le pour qu’il ressemble à la liste ci-dessous. Les pages à laquelle vous liez dans le fichier de mappage de site n’existent pas vraiment, mais qui ne sera pas un problème pour cet exercice.

    [!code-xml[Main](data-bound-controls/samples/sample12.xml)]
4. Ouvrez le formulaire Web par défaut en mode Design.
5. Dans la section de Navigation de la boîte à outils, ajoutez un nouveau contrôle de Menu à la page.
6. À partir de la section données de la boîte à outils, ajoutez un nouveau SiteMapDataSource. SiteMapDataSource utilise automatiquement le fichier Web.sitemap dans votre site. (Le fichier Web.sitemap *doit* être dans le dossier racine du site.)
7. Cliquez sur le contrôle de Menu, puis sur le bouton de balise active pour afficher la boîte de dialogue de tâches du Menu.
8. Dans la liste déroulante Choisir la Source de données, sélectionnez SiteMapDataSource1.
9. Cliquez sur le lien de mise en forme automatique et choisissez un format pour le Menu.
10. Dans le volet Propriétés, définissez la **StaticDisplayLevels** propriété à 2. Le contrôle de Menu apparaît alors le nœud d’accueil, produits et Services dans le concepteur.
11. Parcourir la page dans votre navigateur pour utiliser le menu. (Étant donné que les pages que vous avez déjà ajouté au plan du site n’existent pas, vous verrez une erreur lorsque vous essayez et vous y accédez.)

Essayer de changer la StaticDisplayLevels et les propriétés MaximumDynamicDisplayLevels et voir comment elles affectent la façon dont le menu est rendu.

## <a name="lab-2-dynamically-binding-a-treeview-control"></a>Atelier 2 : Liaison dynamique d’un contrôle TreeView

Cet exercice part du principe que vous disposez de SQL Server s’exécutant localement et que la base de données Northwind est présent sur l’instance de SQL Server. Si ces conditions ne sont pas remplies, modifiez la chaîne de connexion dans l’exemple. Notez que vous devez également spécifier l’authentification SQL Server au lieu d’une connexion approuvée.

1. Créer un nouveau site Web.
2. Basculez en mode Code pour Default.aspx et remplacez tout le code par le code répertorié ci-dessous. 

    [!code-aspx[Main](data-bound-controls/samples/sample13.aspx)]
3. Enregistrez la page en tant que treeview.aspx.
4. Parcourir la page.
5. Lorsque la page s’affiche tout d’abord, affichez la source de la page dans votre navigateur. Notez que seuls les nœuds visibles ont été envoyés au client.
6. Cliquez sur le signe plus en regard de n’importe quel nœud.
7. Afficher la source dans la page à nouveau. Notez que les nœuds qui vient d’être affichés sont désormais présentes.

## <a name="lab-3-details-view-and-editing-data-using-a-gridview-and-detailsview"></a>Labo 3 : Détails affichage et modification des données à l’aide d’un GridView et DetailsView un

1. Créer un nouveau site Web.
2. Ajouter un nouveau fichier web.config pour le site Web.
3. Ajouter une chaîne de connexion au fichier web.config comme indiqué ci-dessous : 

    [!code-xml[Main](data-bound-controls/samples/sample14.xml)]

    > [!NOTE]
    > Vous devrez peut-être modifier la chaîne de connexion en fonction de votre environnement.
4. Enregistrez et fermez le fichier web.config.
5. Ouvrez Default.aspx et ajouter un nouveau contrôle SqlDataSource.
6. Modifier l’ID du contrôle SqlDataSource à **produits**.
7. Dans le **Tâches SqlDataSource** menu, cliquez sur **configurer la Source de données**.
8. Sélectionnez **Northwind** dans la liste déroulante de connexion et cliquez sur Suivant.
9. Sélectionnez **produits** à partir de la **nom** liste déroulante et vérifiez la **ProductID**, **ProductName**, **UnitPrice**, et **UnitsInStock** cases à cocher comme illustré ci-dessous. 

![](data-bound-controls/_static/image3.jpg)

    **Figure 3**
10. Cliquez sur **Suivant**.
11. Cliquez sur **Terminer**.
12. Basculer en mode Source et examinez le code qui a été généré. Notez que le **SelectCommand**, **DeleteCommand**, **InsertCommand**, et **UpdateCommand** qui ont été ajoutés à SqlDataSource contrôle. Notez également les paramètres qui ont été ajoutés.
13. Basculez en mode Design et ajouter un nouveau contrôle GridView à la page.
14. Sélectionnez **produits** à partir de la **choisir la Source de données** liste déroulante.
15. Vérifiez **activer la pagination** et **activer la sélection** comme indiqué ci-dessous. 

![](data-bound-controls/_static/image4.jpg)

    **Figure 4**
16. Cliquez sur le **modifier les colonnes** lier et assurez-vous que l’option **générer automatiquement les champs** est activée.
17. Cliquez sur **OK**.
18. Le contrôle GridView est sélectionné, cliquez sur le bouton en regard du **DataKeyNames** propriété dans le volet Propriétés.
19. Sélectionnez **ProductID** à partir de la **champs de données disponibles** liste et cliquez sur le **&gt;** bouton pour l’ajouter.
20. Cliquez sur OK.
21. Ajouter un nouveau contrôle SqlDataSource à la page.
22. Modifier l’ID du contrôle SqlDataSource à **détails**.
23. Dans le menu Tâches SqlDataSource, choisissez **configurer la Source de données**.
24. Choisissez **Northwind** dans la liste déroulante et cliquez sur **suivant**.
25. Sélectionnez <strong>produits</strong> à partir de la <strong>nom</strong> liste déroulante et vérifiez la <strong> \</strong > * case à cocher dans la <strong>colonnes</strong> listbox.
26. Cliquez sur le **où** bouton.
27. Sélectionnez **ProductID** à partir de la **colonne** liste déroulante.
28. Sélectionnez **=** dans la liste déroulante opérateur.
29. Sélectionnez **contrôle** à partir de la **Source** liste déroulante.
30. Sélectionnez **GridView1** à partir de la **ID de contrôle** liste déroulante.
31. Cliquez sur le **ajouter** pour ajouter la clause WHERE.
32. Cliquez sur **OK**.
33. Cliquez sur le **avancé** bouton et vérifier le **instructions générer INSERT, UPDATE et DELETE** case à cocher.
34. Cliquez sur **OK**.
35. Cliquez sur **suivant** et cliquez sur **Terminer**.
36. Ajouter un contrôle DetailsView à la page.
37. Dans le **choisir la Source de données** liste déroulante, choisissez **détails**.
38. Vérifier le **activer la modification** case à cocher comme illustré ci-dessous. 

![](data-bound-controls/_static/image1.gif)

    **Figure 5**
39. Enregistrez la page et parcourir Default.aspx.
40. Cliquez sur le **sélectionnez** lien en regard des enregistrements différents pour afficher la mise à jour DetailsView automatiquement.
41. Cliquez sur le **modifier** lien dans le contrôle DetailsView.
42. Apportez une modification à l’enregistrement, puis cliquez sur **mise à jour**.
