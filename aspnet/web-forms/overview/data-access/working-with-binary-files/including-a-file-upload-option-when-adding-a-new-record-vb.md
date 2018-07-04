---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-vb
title: Option incluant un fichier de téléchargement lorsque vous ajoutez un nouvel enregistrement (VB) | Microsoft Docs
author: rick-anderson
description: Ce didacticiel montre comment créer une interface Web qui permet à l’utilisateur à entrer des données de texte et télécharger les fichiers binaires. Pour illustrer l’options disponibles de t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 5776281d-4637-4d1e-a65b-2621d2cade44
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-vb
msc.type: authoredcontent
ms.openlocfilehash: 30cfdcf8e9b65f92b267509b4289d828b2532e65
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363143"
---
<a name="including-a-file-upload-option-when-adding-a-new-record-vb"></a>Y compris une Option de chargement de fichier lors de l’ajout d’un nouvel enregistrement (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_VB.exe) ou [télécharger le PDF](including-a-file-upload-option-when-adding-a-new-record-vb/_static/datatutorial56vb1.pdf)

> Ce didacticiel montre comment créer une interface Web qui permet à l’utilisateur à entrer des données de texte et télécharger les fichiers binaires. Pour illustrer les options disponibles pour stocker des données binaires, un seul fichier sera enregistré dans la base de données, tandis que l’autre est stockée dans le système de fichiers.


## <a name="introduction"></a>Introduction

Dans les deux didacticiels précédents, nous avons exploré techniques pour le stockage des données binaires qui sont associées avec le modèle de données d’application s, étudié la façon d’utiliser le contrôle FileUpload pour envoyer des fichiers à partir du client au serveur web et l’avons vu comment présenter ces données binaires dans un W de données contrôle d’EB. Nous ve encore à aborder la manière d’associer des données chargées avec le modèle de données, cependant.

Dans ce didacticiel, nous allons créer une page web pour ajouter une nouvelle catégorie. En plus des zones de texte pour le nom de catégorie s et la description, cette page devez inclure deux contrôles FileUpload pour la nouvelle image de catégorie s et l’autre pour la brochure. L’image téléchargée est stockée directement dans le nouvel enregistrement s `Picture` colonne, tandis que la brochure sera enregistrée dans le `~/Brochures` dossier avec le chemin d’accès au fichier enregistré dans le nouvel enregistrement s `BrochurePath` colonne.

Avant de créer cette nouvelle page web, nous allons devoir mettre à jour de l’architecture. Le `CategoriesTableAdapter` requête principale s ne récupère pas les `Picture` colonne. Par conséquent, générée automatiquement `Insert` méthode dispose uniquement des entrées pour le `CategoryName`, `Description`, et `BrochurePath` champs. Par conséquent, nous devons créer une méthode supplémentaire dans le TableAdapter qui invite à entrer les quatre `Categories` champs. Le `CategoriesBLL` classe dans la couche de logique métier doit également être mis à jour.

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>Étape 1 : Ajout d’un`InsertWithPicture`méthode à la`CategoriesTableAdapter`

Lorsque nous avons créé le `CategoriesTableAdapter` dans le [création d’une couche d’accès aux données](../introduction/creating-a-data-access-layer-vb.md) didacticiel, nous avons configuré qu’il génère automatiquement `INSERT`, `UPDATE`, et `DELETE` instructions en fonction de la requête principale. En outre, nous lui avons demandé d’utiliser l’approche Direct de base de données, ce qui a créé les méthodes TableAdapter `Insert`, `Update`, et `Delete`. Ces méthodes sont exécutées générées automatiquement `INSERT`, `UPDATE`, et `DELETE` instructions et, par conséquent, acceptez les paramètres d’entrée selon les colonnes retournées par la requête principale. Dans le [télécharger des fichiers](uploading-files-vb.md) didacticiel, nous avons augmentés la `CategoriesTableAdapter` requête principale de s à utiliser le `BrochurePath` colonne.

Dans la mesure où le `CategoriesTableAdapter` requête principale s ne fait pas référence à la `Picture` colonne, nous pouvons ni ajouter un nouvel enregistrement ni mettre à jour un enregistrement existant avec une valeur pour le `Picture` colonne. Pour capturer ces informations, nous pouvons soit créer une nouvelle méthode du TableAdapter est utilisée spécifiquement pour insérer un enregistrement avec des données binaires ou nous pouvons personnaliser générées automatiquement `INSERT` instruction. Le problème avec la personnalisation générées automatiquement `INSERT` instruction est que nous contraint notre personnalisations remplacées par l’Assistant. Par exemple, imaginez que nous avons personnalisé les `INSERT` instruction d’inclure l’utilisation de la `Picture` colonne. Cela est mise à jour du TableAdapter s `Insert` méthode pour inclure un paramètre d’entrée supplémentaire pour les données binaires de catégorie s image s. Nous pouvons ensuite créer une méthode dans la couche de logique métier pour utiliser cette méthode de la couche DAL et appeler cette méthode de la couche BLL via la couche de présentation, et tout fonctionnera parfaitement. Autrement dit, jusqu'à ce que la prochaine fois, nous avons configuré le TableAdapter via l’Assistant Configuration de TableAdapter. Dès que l’Assistant terminée, notre personnalisations à la `INSERT` instruction serait remplacée, le `Insert` méthode est rétablie à sa forme ancien, et notre code compilera n’est plus !

> [!NOTE]
> Ce désagrément est un problème lors de l’utilisation de procédures stockées au lieu d’instructions SQL ad hoc. Un futur didacticiel explorera à être à l’aide de procédures stockées à la place d’instructions SQL ad hoc dans la couche d’accès aux données.


Pour éviter de vous soucier, plutôt que de personnaliser les instructions SQL générées automatiquement permettre de s au lieu de cela créer une nouvelle méthode du TableAdapter. Cette méthode, nommée `InsertWithPicture`, acceptent des valeurs pour le `CategoryName`, `Description`, `BrochurePath`, et `Picture` colonnes et exécuter un `INSERT` instruction qui stocke toutes les quatre valeurs dans un nouvel enregistrement.

Ouvrez le DataSet typé et, à partir du concepteur, cliquez sur le `CategoriesTableAdapter` en-tête de s et choisissez Ajouter une requête dans le menu contextuel. Cette opération lance l’Assistant de Configuration de requête TableAdapter, qui commence par vous demander de nous comment la requête TableAdapter doit-il accéder à la base de données. Choisissez d’utiliser des instructions SQL et cliquez sur Suivant. L’étape suivante vous invite à entrer pour le type de requête doit être généré. Dans la mesure où re création d’une requête pour ajouter un nouvel enregistrement à la `Categories` de table, cliquez sur Insérer et cliquez sur Suivant.


[![Sélectionnez l’Option d’insertion](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image1.png)

**Figure 1**: sélectionnez l’Option Insérer ([cliquez pour afficher l’image en taille réelle](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image2.png))


Nous devons maintenant spécifier le `INSERT` instruction SQL. L’Assistant suggère automatiquement un `INSERT` instruction correspondant à la requête principale de TableAdapter s. Dans ce cas, il s un `INSERT` instruction insère le `CategoryName`, `Description`, et `BrochurePath` valeurs. L’instruction de mise à jour afin que le `Picture` colonne est incluse avec un `@Picture` paramètre, comme suit :


[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample1.sql)]

Le dernier écran de l’Assistant nous demande de nommer la nouvelle méthode du TableAdapter. Entrez `InsertWithPicture` et cliquez sur Terminer.


[![Nom de la nouvelle InsertWithPicture TableAdapter (méthode)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image3.png)

**Figure 2**: nom de la nouvelle méthode TableAdapter `InsertWithPicture` ([cliquez pour afficher l’image en taille réelle](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image4.png))


## <a name="step-2-updating-the-business-logic-layer"></a>Étape 2 : Mise à jour de la couche de logique métier

Dans la mesure où la couche de présentation doit uniquement interagir avec la couche de logique métier au lieu de contournement pour accéder directement à la couche d’accès aux données, nous devons créer une méthode de la couche de logique métier qui appelle la méthode de la couche DAL que nous venons de créer (`InsertWithPicture`). Pour ce didacticiel, créez une méthode dans le `CategoriesBLL` classe nommée `InsertWithPicture` qui accepte comme entrée trois `String` s et un `Byte` tableau. Le `String` sont des paramètres d’entrée pour le nom de catégorie s, la description et le chemin d’accès du fichier brochure, tandis que le `Byte` tableau est pour le contenu binaire de l’image de catégorie s. Comme le montre le code suivant, cette méthode de la couche BLL appelle la méthode correspondante de la couche DAL :


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample2.vb)]

> [!NOTE]
> Assurez-vous que vous avez enregistré le DataSet typée avant d’ajouter le `InsertWithPicture` méthode à la couche BLL. Dans la mesure où le `CategoriesTableAdapter` code de la classe est généré automatiquement selon le jeu de données typé, si ne pas enregistrer tout d’abord vos modifications dans le DataSet typé la `Adapter` propriété a gagné t connaître le `InsertWithPicture` (méthode).


## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>Étape 3 : Liste des catégories existantes et leurs données binaires

Dans ce didacticiel, nous allons créer une page qui permet à un utilisateur final d’ajouter une nouvelle catégorie au système, en fournissant une image et la brochure pour la nouvelle catégorie. Dans le [didacticiel précédent](displaying-binary-data-in-the-data-web-controls-vb.md) nous avons utilisé un GridView avec ImageField et TemplateField pour afficher chaque nom de catégorie s, description, image et un lien pour télécharger son brochure. Laissez s répliquer cette fonctionnalité pour ce didacticiel, la création d’une page qui répertorie toutes les catégories existantes et permet aux nouvelles sauvegardes doit être créé.

Commencez par ouvrir le `DisplayOrDownload.aspx` page à partir de la `BinaryData` dossier. Accédez à la vue de Source et copiez la GridView et ObjectDataSource s syntaxe déclarative, son collage dans le `<asp:Content>` élément `UploadInDetailsView.aspx`. En outre, n’oubliez pas que copier sur le `GenerateBrochureLink` méthode à partir de la classe code-behind de `DisplayOrDownload.aspx` à `UploadInDetailsView.aspx`.


[![Copiez et collez la syntaxe déclarative à partir de DisplayOrDownload.aspx à UploadInDetailsView.aspx](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image5.png)

**Figure 3**: copiez et collez la syntaxe déclarative à partir de `DisplayOrDownload.aspx` à `UploadInDetailsView.aspx` ([cliquez pour afficher l’image en taille réelle](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image6.png))


Après avoir copié la syntaxe déclarative et `GenerateBrochureLink` méthode sur le `UploadInDetailsView.aspx` page, affichez la page via un navigateur pour vous assurer que tout a été correctement copiée. Vous devriez voir un GridView les huit catégories d’annonces qui inclut un lien pour télécharger la brochure, ainsi que l’image de catégorie s.


[![Vous devriez maintenant voir chaque catégorie, ainsi que ses données binaires](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image7.png)

**Figure 4**: vous doit maintenant voir chaque catégorie ainsi que ses données binaires ([cliquez pour afficher l’image en taille réelle](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image8.png))


## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>Étape 4 : Configuration du`CategoriesDataSource`à l’insertion de la prise en charge

Le `CategoriesDataSource` ObjectDataSource utilisé par le `Categories` actuellement GridView ne fournit pas la possibilité d’insérer des données. Pour prendre en charge d’insertion de ce contrôle de source de données, nous devons mapper son `Insert` méthode à une méthode dans son objet sous-jacent, `CategoriesBLL`. En particulier, nous souhaitons pouvoir mapper le `CategoriesBLL` méthode que nous avons ajouté dans l’étape 2, `InsertWithPicture`.

Démarrez en cliquant sur le lien configurer la Source de données à partir de la balise active de s ObjectDataSource. Le premier écran affiche l’objet que la source de données est configurée pour fonctionner avec, `CategoriesBLL`. Laissez ce paramètre en tant que- et cliquez sur Suivant pour passer à l’écran de définir des méthodes de données. Déplacer vers l’onglet Insertion et choisissez le `InsertWithPicture` méthode dans la liste déroulante. Cliquez sur Terminer pour terminer l’Assistant.


[![Configurer pour utiliser la méthode InsertWithPicture ObjectDataSource](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image9.png)

**Figure 5**: configurer ObjectDataSource à utiliser le `InsertWithPicture` (méthode) ([cliquez pour afficher l’image en taille réelle](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image10.png))


> [!NOTE]
> À la fin de l’Assistant, Visual Studio peut vous demander si vous souhaitez actualiser les champs et les clés, qui sera régénérer les données Web contrôle les champs. Choisissez non, car vous sélectionnez Oui pour remplacer les personnalisations de champ que vous avez apportées.


Après la fin de l’Assistant ObjectDataSource inclut désormais une valeur pour son `InsertMethod` propriété ainsi que `InsertParameters` pour les colonnes de la catégorie de quatre, en tant que le balisage déclaratif suivant illustre la :


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>Étape 5 : Création de l’Interface insertion

Comme tout d’abord traitées dans le [une vue d’ensemble d’insertion, mise à jour et suppression des données](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md), le contrôle DetailsView fournit une interface d’insertion intégrée qui peut être utilisée lorsque vous travaillez avec un contrôle de source de données qui prend en charge l’insertion. Permettent d’ajouter un contrôle DetailsView sur cette page au-dessus de la GridView qui restituera définitivement son interface d’insertion, qui permet à un utilisateur ajouter rapidement une nouvelle catégorie s. Lors de l’ajout d’une nouvelle catégorie dans le contrôle DetailsView, GridView sous celui-ci sera automatiquement actualiser et afficher la nouvelle catégorie.

Démarrage en faisant glisser un contrôle DetailsView à partir de la boîte à outils vers le concepteur au-dessus de la GridView, en définissant son `ID` propriété `NewCategory` et éliminant la `Height` et `Width` les valeurs de propriété. À partir de la balise active de s DetailsView, liez-le à l’objet existant `CategoriesDataSource` puis cochez la case à cocher Activer l’insertion.


[![Lier le contrôle DetailsView à la CategoriesDataSource et activer l’insertion](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image11.png)

**Figure 6**: lier le contrôle DetailsView à la `CategoriesDataSource` et activer l’insertion ([cliquez pour afficher l’image en taille réelle](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image12.png))


Pour définitivement restituer le contrôle DetailsView dans son interface d’insertion, définissez son `DefaultMode` propriété `Insert`.

Notez que le contrôle DetailsView a cinq BoundFields `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, et `BrochurePath` bien que le `CategoryID` BoundField n’est pas restitué dans l’interface d’insertion, car son `InsertVisible` propriété est définie sur `False`. Ces BoundFields existe, car ils sont les colonnes retournées par la `GetCategories()` (méthode), qui est ce que ObjectDataSource appelle pour récupérer ses données. Pour l’insertion, toutefois, nous n t que vous souhaitez pour permettre à l’utilisateur de spécifier une valeur pour `NumberOfProducts`. En outre, nous avons besoin de les autoriser à télécharger une image pour la nouvelle catégorie ainsi que de télécharger un fichier PDF pour la brochure.

Supprimer le `NumberOfProducts` BoundField à partir du DetailsView complètement et de la mise à jour puis la `HeaderText` propriétés de la `CategoryName` et `BrochurePath` BoundFields à la catégorie et de la Brochure, respectivement. Ensuite, convertissez le `BrochurePath` BoundField en TemplateField et ajouter un nouveau TemplateField pour l’image, en donnant à cette nouvelle TemplateField un `HeaderText` valeur de l’image. Déplacer le `Picture` TemplateField afin qu’il soit entre le `BrochurePath` TemplateField et CommandField.


![Lier le contrôle DetailsView à la CategoriesDataSource et activer l’insertion](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image7.gif)

**Figure 7**: lier le contrôle DetailsView à la `CategoriesDataSource` et activer l’insertion


Si vous avez converti le `BrochurePath` BoundField en TemplateField via la boîte de dialogue Modifier les champs, le TemplateField contenu inclut un `ItemTemplate`, `EditItemTemplate`, et `InsertItemTemplate`. Uniquement les `InsertItemTemplate` est nécessaire, toutefois, par conséquent, n’hésitez pas à supprimer les deux autres modèles. À ce stade, votre syntaxe déclarative du contrôle DetailsView s doit se présenter comme suit :


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>Ajout de contrôles FileUpload pour la Brochure et les champs d’image

Actuellement, le `BrochurePath` TemplateField s `InsertItemTemplate` contient une zone de texte, tandis que le `Picture` TemplateField ne contient-elle pas tous les modèles. Nous devons mettre à jour de ces deux s TemplateField `InsertItemTemplate` s à utiliser des contrôles FileUpload.

À partir de la balise active de s DetailsView, choisissez l’option Modifier les modèles, puis sélectionnez le `BrochurePath` TemplateField s `InsertItemTemplate` dans la liste déroulante. Supprimer la zone de texte et faites glisser un contrôle FileUpload à partir de la boîte à outils, dans le modèle. Définir le contrôle FileUpload s `ID` à `BrochureUpload`. De même, ajoutez un contrôle FileUpload pour le `Picture` TemplateField s `InsertItemTemplate`. Définissez ce contrôle FileUpload s `ID` à `PictureUpload`.


[![Ajouter un contrôle FileUpload à l’élément InsertItemTemplate](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image13.png)

**Figure 8**: ajouter un contrôle FileUpload à la `InsertItemTemplate` ([cliquez pour afficher l’image en taille réelle](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image14.png))


Après avoir apporté ces ajouts, la syntaxe déclarative de deux TemplateField s sera :


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample5.aspx)]

Lorsqu’un utilisateur ajoute une nouvelle catégorie, nous voulons vérifier que la brochure et image sont du type de fichier correct. Pour la brochure, l’utilisateur doit fournir un fichier PDF. Pour l’image, nous avons besoin de l’utilisateur de télécharger un fichier image, mais nous permettent *n’importe quel* fichier ou uniquement les fichiers d’image d’un type particulier, tels que le format GIF ou jpg de l’image ? Pour autoriser d’autres types de fichiers, nous avons d besoin étendre le `Categories` schéma afin d’inclure une colonne qui capture le type de fichier afin que ce type peut être envoyé au client via `Response.ContentType` dans `DisplayCategoryPicture.aspx`. Étant donné que nous ne pas avoir une telle colonne, il serait préférable de restreindre les utilisateurs à fournir uniquement un type de fichier image spécifique. Le `Categories` images existantes de la table s sont des bitmaps, mais jpg est un format de fichier le plus approprié pour les images pris en charge sur le web.

Si un utilisateur charge un type de fichier incorrect, nous devons annuler l’insertion et affiche un message indiquant le problème. Ajoutez un contrôle Web Label sous le contrôle DetailsView. Définir son `ID` propriété `UploadWarning`, désactivez les son `Text` propriété, la valeur la `CssClass` propriété à avertissement et le `Visible` et `EnableViewState` propriétés à `False`. Le `Warning` classe CSS est définie dans `Styles.css` et restitue le texte dans une police de grande taille, rouge, en italique, gras.

> [!NOTE]
> Dans l’idéal, le `CategoryName` et `Description` BoundFields est convertie en TemplateField et leurs interfaces insertion personnalisées. Le `Description` insertion interface, par exemple, serait probablement mieux adapté à une zone de texte multiligne. Et puisque le `CategoryName` colonne n’accepte pas `NULL` valeurs, un contrôle RequiredFieldValidator doit être ajouté pour vous assurer de l’utilisateur fournit une valeur pour le nouveau nom de catégorie s. Ces étapes sont laissées en guise d’exercice pour le lecteur. Faire référence à [personnalisation de l’Interface de Modification de données](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) pour un examen approfondi de compléter les interfaces de modification de données.


## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>Étape 6 : L’enregistrement de la Brochure chargée dans le système de fichiers Web Server s

Lorsque l’utilisateur entre les valeurs pour une nouvelle catégorie et clique sur le bouton d’insertion, une publication (postback) se produit et le flux de travail insertion qui se déroule. Tout d’abord, le contrôle DetailsView s [ `ItemInserting` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx) se déclenche. Ensuite, les opérations de mappage ObjectDataSource `Insert()` méthode est appelée, ce qui aboutit à un nouvel enregistrement est ajouté à la `Categories` table. Après cela, le contrôle DetailsView s [ `ItemInserted` événement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx) se déclenche.

Avant de l’ObjectDataSource s `Insert()` méthode est appelée, nous devons tout d’abord vous assurer que les types de fichiers appropriées ont été téléchargés par l’utilisateur et puis enregistrez la brochure PDF dans le système de fichiers du serveur s web. Créer un gestionnaire d’événements pour les opérations de mappage DetailsView `ItemInserting` événement et ajoutez le code suivant :


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample6.vb)]

Le Gestionnaire d’événements démarre en référençant le `BrochureUpload` contrôle FileUpload parmi les modèles de s DetailsView. Ensuite, si une brochure a été chargée, l’extension de s du fichier chargé est examinée. Si l’extension n’est pas. PDF, puis un message d’avertissement s’affiche, l’insertion est annulée et l’exécution du Gestionnaire d’événements se termine.

> [!NOTE]
> S’appuyer sur l’extension de s du fichier chargé n’est pas une technique incontournable pour s’assurer que le fichier chargé est un document PDF. L’utilisateur peut avoir un document PDF valide avec l’extension `.Brochure`, ou pourrait a pris un document non PDF et attribué une `.pdf` extension. Le contenu binaire du fichier s doit être examinée par programmation afin de conclure le plus le type de fichier. Ces approches approfondies, cependant, sont souvent excessifs ; la vérification de l’extension est suffisante pour la plupart des scénarios.


Comme indiqué dans le [télécharger des fichiers](uploading-files-vb.md) être vigilant lors de l’enregistrement de fichiers au système de fichiers pour ce téléchargement d’un utilisateur s ne sont pas remplacés s un autre didacticiel. Pour ce didacticiel, nous tenterons d’utiliser le même nom que le fichier téléchargé. S’il existe déjà un fichier dans le `~/Brochures` répertoire avec ce même nom de fichier, cependant, nous allons ajouter un numéro à la fin jusqu'à ce qu’un nom unique est trouvé. Par exemple, si l’utilisateur télécharge un fichier brochure nommé `Meats.pdf`, mais il existe déjà un fichier nommé `Meats.pdf` dans le `~/Brochures` dossier, nous allons modifier le nom de fichier enregistré à `Meats-1.pdf`. Si qui existe, nous essaierons `Meats-2.pdf`, et ainsi de suite, jusqu'à ce qu’un nom de fichier unique est trouvé.

Le code suivant utilise la [ `File.Exists(path)` méthode](https://msdn.microsoft.com/library/system.io.file.exists.aspx) pour déterminer si un fichier existe déjà avec le nom de fichier spécifié. Dans ce cas, il continue d’essayer les nouveaux noms de fichiers pour la brochure jusqu'à ce qu’aucun conflit n’est trouvée.


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample7.vb)]

Une fois qu’un nom de fichier valide a été trouvé, le fichier doit être enregistré dans le système de fichiers et les opérations de mappage ObjectDataSource `brochurePath``InsertParameter` la valeur doit être mis à jour afin que ce nom de fichier est écrit dans la base de données. Comme nous l’avons vu dans la *télécharger des fichiers* didacticiel, le fichier peut être enregistré à l’aide du contrôle FileUpload s `SaveAs(path)` (méthode). Pour mettre à jour de l’ObjectDataSource s `brochurePath` paramètre, utilisez le `e.Values` collection.


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample8.vb)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>Étape 7 : L’enregistrement de l’image téléchargée dans la base de données

Pour stocker l’image téléchargée dans le nouveau `Categories` enregistrement, vous devez affecter le contenu binaire chargé pour les opérations de mappage ObjectDataSource `picture` paramètre dans le s DetailsView `ItemInserting` événement. Toutefois, avant que nous apportons cette affectation, nous devons tout d’abord vous assurer que l’image téléchargé est un fichier JPG et pas un autre type d’image. Comme dans l’étape 6, permettent d’utiliser l’extension de fichier image téléchargé s afin de déterminer son type s.

Alors que le `Categories` table autorise `NULL` valeurs pour la `Picture` colonne, toutes les catégories actuellement avoir une image. Laissez s forcer l’utilisateur à fournir une image lors de l’ajout d’une nouvelle catégorie sur cette page. Le code suivant vérifie qu’une image a été chargée et qu’il a une extension appropriée.


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample9.vb)]

Ce code doit être placé *avant* le code de l’étape 6 afin que s’il existe un problème avec le chargement d’image, le Gestionnaire d’événements s’arrête avant que le fichier de la brochure est enregistré dans le système de fichiers.

En supposant qu’un fichier approprié a été chargé, affecter le contenu binaire chargé pour la valeur du paramètre s image avec la ligne de code suivante :


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample10.vb)]

## <a name="the-completeiteminsertingevent-handler"></a>L’ensemble`ItemInserting`Gestionnaire d’événements

Par souci d’exhaustivité, voici le `ItemInserting` Gestionnaire d’événements dans son intégralité :


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample11.vb)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>Étape 8 : Résolution du`DisplayCategoryPicture.aspx`Page

Let s prenez un moment pour tester l’interface d’insertion et `ItemInserting` Gestionnaire d’événements qui a été créé au cours des étapes. Visitez le `UploadInDetailsView.aspx` page via un navigateur et de la tentative d’ajouter une catégorie, mais omettez l’image, ou spécifier une image non JPG ou une brochure non PDF. Dans tous ces cas, un message d’erreur s’affichera et le flux de travail d’insertion est annulée.


[![Un Message d’avertissement est affiché si un Type de fichier non valide est téléchargé](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image15.png)

**Figure 9**: un Message d’avertissement est affiché si un Type de fichier non valide est téléchargé ([cliquez pour afficher l’image en taille réelle](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image16.png))


Une fois avoir vérifié que la page nécessite une image à télécharger et gagné / t accepter des fichiers non PDF ou non JPG, ajouter une nouvelle catégorie avec une image JPG valide, si vous laissez le champ de la Brochure vide. Après avoir cliqué sur le bouton d’insertion, la page sera publication (postback) et un nouvel enregistrement sera ajouté à la `Categories` table avec le contenu binaire de l’image chargée s stockées directement dans la base de données. Le contrôle GridView est mis à jour et affiche une ligne pour la catégorie nouvellement ajoutée, mais, comme le montre la Figure 10, la nouvelle image de s catégorie s’affiche pas correctement.


[![La nouvelle catégorie s Qu'image n’est pas affichée.](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image17.png)

**Figure 10**: s de la nouvelle catégorie image n’est pas affichée ([cliquez pour afficher l’image en taille réelle](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image18.png))


La nouvelle image n’est pas affichée est parce que le `DisplayCategoryPicture.aspx` page qui retourne une image de la catégorie spécifiée s est configurée pour traiter les bitmaps qui ont un en-tête OLE. Cet en-tête 78 octets est supprimé de la `Picture` contenu binaire de la colonne s avant leur envoi au client. Mais le fichier JPG, nous avons simplement chargé pour la nouvelle catégorie n’a pas cet en-tête OLE ; Par conséquent, sont en cours de suppression des octets valides, qui est nécessaires à partir des données binaires image s.

Dans la mesure où il existe désormais deux bitmaps avec en-têtes OLE et jpg dans le `Categories` table, nous devons mettre à jour `DisplayCategoryPicture.aspx` afin qu’il ne l’en-tête OLE jointes pour les catégories de huit d’origine et que vous ignore cette suppression pour les enregistrements plus récents de la catégorie. Dans notre prochain didacticiel, nous allons examiner comment mettre à jour une image de l’enregistrement s existante, et nous allons mettre à jour toutes les anciennes images catégorie afin qu’ils soient jpg. Pour l’instant, cependant, utiliser le code suivant dans `DisplayCategoryPicture.aspx` pour supprimer les en-têtes OLE uniquement pour ces huit catégories d’origine :


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample12.vb)]

Avec cette modification, l’image JPG s’affiche désormais correctement dans le contrôle GridView.


[![Les Images JPG de nouvelles catégories sont correctement rendus](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image19.png)

**Figure 11**: les Images JPG de nouvelles catégories sont correctement rendus ([cliquez pour afficher l’image en taille réelle](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image20.png))


## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>Étape 9 : Suppression de la Brochure face à une Exception

Un des défis du stockage des données binaires sur le système de fichiers web server s est qu’elle entraîne une déconnexion entre le modèle de données et ses données binaires. Par conséquent, chaque fois qu’un enregistrement est supprimé, les données binaires correspondantes sur le système de fichiers doivent également être supprimées. Cela peut entrent en jeu lors de l’insertion, également. Considérez le scénario suivant : un utilisateur ajoute une nouvelle catégorie, en spécifiant une image valide et la brochure. Sur le bouton Insert, une publication (postback) et les opérations de mappage DetailsView `ItemInserting` se déclenche l’événement, l’enregistrement de la brochure dans le système de fichiers du serveur s web. Ensuite, les opérations de mappage ObjectDataSource `Insert()` méthode est appelée, qui appelle le `CategoriesBLL` classe s `InsertWithPicture` (méthode), qui appelle le `CategoriesTableAdapter` s `InsertWithPicture` (méthode).

Maintenant, que se passe-t-il si la base de données est hors connexion, ou s’il existe une erreur dans le `INSERT` instruction SQL ? Clairement l’insertion échoue, et aucune nouvelle ligne catégorie ne sera être ajouté à la base de données. Mais nous conservons le fichier téléchargé brochure posé sur le système de fichiers web server s ! Ce fichier doit être supprimé en cas d’une exception pendant le flux de travail insertion.

Comme mentionné précédemment dans le [BLL - gestion et les Exceptions au niveau de la couche DAL dans une Page ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) (didacticiel), lorsqu’une exception est levée à partir dans les profondeurs de l’architecture, elle est propagée à travers les différentes couches. Dans la couche de présentation, nous pouvons déterminer si une exception s’est produite à partir de la s DetailsView `ItemInserted` événement. Ce gestionnaire d’événements fournit également les valeurs de l’ObjectDataSource s `InsertParameters`. Par conséquent, nous pouvons créer un gestionnaire d’événements pour le `ItemInserted` événement qui vérifie si une exception s’est produite et, le cas échéant, supprime le fichier spécifié par le s ObjectDataSource `brochurePath` paramètre :


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample13.vb)]

## <a name="summary"></a>Récapitulatif

Il existe un nombre d’étapes qui doivent être effectuées afin de fournir une interface basée sur le web pour ajouter des enregistrements qui incluent des données binaires. Si les données binaires sont stockées directement dans la base de données, sans doute que vous devez mettre à jour de l’architecture, ajout de méthodes spécifiques pour gérer le cas où les données binaires sont insérées. Une fois que l’architecture a été mis à jour, l’étape suivant consiste à créer l’interface d’insertion, ce qui peut être effectué à l’aide d’un contrôle DetailsView qui a été personnalisé pour inclure un contrôle FileUpload pour chaque champ de données binaires. Les données chargées peuvent être enregistrées dans le système de fichiers du serveur s web ou affectées à un paramètre de source de données dans le contrôle DetailsView s `ItemInserting` Gestionnaire d’événements.

L’enregistrement des données binaires dans le système de fichiers requiert une planification plus que l’enregistrement des données directement dans la base de données. Un schéma d’affectation de noms doit être choisi afin d’éviter un chargement utilisateur s en remplaçant s un autre. En outre, des étapes supplémentaires sont nécessaires pour supprimer le fichier chargé si l’insertion de la base de données échoue.

Nous avons maintenant la possibilité d’ajouter de nouvelles catégories pour le système avec une brochure et image, mais nous ve encore pour examiner comment mettre à jour de la catégorie s binaire données existantes ou de la façon de supprimer correctement les données binaires pour une catégorie supprimée. Nous allons explorer ces deux rubriques dans le didacticiel suivant.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Dave Gardner, Teresa Murphy et Bernadette Leigh. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](displaying-binary-data-in-the-data-web-controls-vb.md)
> [Suivant](updating-and-deleting-existing-binary-data-vb.md)
