---
uid: web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
title: Lot d’insertion (VB) | Microsoft Docs
author: rick-anderson
description: Découvrez comment insérer plusieurs enregistrements de base de données en une seule opération. Dans la couche d’Interface utilisateur, nous étendons le contrôle GridView pour autoriser l’utilisateur à entrer plusieurs n...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 48e2a4ae-77ca-4208-a204-c38c690ffb59
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-inserting-vb
msc.type: authoredcontent
ms.openlocfilehash: 17a077ed0124a0a9e06c90d0ac137958693fc30e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390085"
---
<a name="batch-inserting-vb"></a>Lot d’insertion (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_66_VB.zip) ou [télécharger le PDF](batch-inserting-vb/_static/datatutorial66vb1.pdf)

> Découvrez comment insérer plusieurs enregistrements de base de données en une seule opération. Dans la couche d’Interface utilisateur, nous étendons le contrôle GridView pour autoriser l’utilisateur à entrer plusieurs nouveaux enregistrements. Dans la couche d’accès aux données nous encapsuler plusieurs opérations d’insertion dans une transaction pour vous assurer que toutes les insertions de réussissent ou toutes les insertions sont restaurées.


## <a name="introduction"></a>Introduction

Dans le [la mise à jour par lots](batch-updating-vb.md) didacticiel, nous avons étudié la personnalisation du contrôle GridView pour présenter une interface où plusieurs enregistrements ont été modifiables. L’utilisateur accédant à la page peut effectuer une série de modifications et ensuite, avec un seul clic, effectuer une mise à jour par lots. Dans les situations où les utilisateurs souvent mettre à jour les nombre d’enregistrements d’un coup, une telle interface peut enregistrer un nombre incalculable de clics et les changements de contexte de clavier-souris par rapport à la valeur par défaut par ligne fonctionnalités de modification qui ont été tout d’abord examinées dans le [un Vue d’ensemble de l’insertion, mise à jour et suppression des données](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) didacticiel.

Ce concept peut également être appliqué lors de l’ajout d’enregistrements. Imaginez cela ici chez Northwind Traders nous reçoivent généralement les envois de fournisseurs qui contiennent un nombre de produits pour une catégorie particulière. Par exemple, nous pourrions réception de six différents thé et produits de café de Tokyo Traders. Si un utilisateur entre les six produits celui à la fois à travers un contrôle DetailsView, ils doivent choisir la plupart des mêmes valeurs indéfiniment : ils doivent choisir la même catégorie (boissons), le même fournisseur (Tokyo Traders), les mêmes abandonné (de valeur False) et les mêmes unités sur la valeur d’ordre (0). Cette entrée de données répétitives n’est pas uniquement beaucoup de temps, mais il est sujette aux erreurs.

Avec un peu de travail, nous pouvons créer un lot d’insertion d’interface qui permet à l’utilisateur de choisir le fournisseur et la catégorie une seule fois, entrez une série de noms de produits et des prix unitaires, puis cliquez sur un bouton pour ajouter les nouveaux produits à la base de données (voir Figure 1). Comme chaque produit est ajoutée, sa `ProductName` et `UnitPrice` des champs de données sont affectées les valeurs entrées dans les zones de texte, tandis que son `CategoryID` et `SupplierID` valeurs sont assignées les valeurs à partir de la DropDownList sur le haut de la forme. Le `Discontinued` et `UnitsOnOrder` valeurs sont définies sur les valeurs codées en dur des `False` et 0, respectivement.


[![L’Interface d’insertion de lot](batch-inserting-vb/_static/image2.png)](batch-inserting-vb/_static/image1.png)

**Figure 1**: l’Interface d’insertion de lot ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image3.png))


Dans ce didacticiel, nous allons créer une page qui implémente le lot d’insertion d’interface, comme indiqué dans la Figure 1. Comme avec deux didacticiels précédents, nous encapsule les insertions dans la portée d’une transaction pour garantir l’atomicité. Laissez s commencer !

## <a name="step-1-creating-the-display-interface"></a>Étape 1 : Création de l’Interface d’affichage

Ce didacticiel se compose d’une seule page est divisée en deux zones : une zone d’affichage et une région d’insertion. L’interface d’affichage, nous allons créer dans cette étape, affiche les produits dans un GridView et inclut un bouton intitulé le processus de livraison du produit. Lorsque l’utilisateur clique sur ce bouton, l’interface d’affichage est remplacé par l’interface d’insertion, qui est indiqué dans la Figure 1. L’interface d’affichage retourne après l’ajout de produits à partir de l’expédition ou cliqué sur les boutons Annuler. Nous allons créer l’interface d’insertion à l’étape 2.

Lors de la création d’une page qui a deux interfaces, seul l’un d’eux est visible à la fois, chaque interface est généralement placé dans un [contrôle de panneau de configuration Web](http://www.w3schools.com/aspnet/control_panel.asp), qui sert de conteneur pour d’autres contrôles. Par conséquent, notre page aura deux contrôles de panneau un pour chaque interface.

Commencez par ouvrir le `BatchInsert.aspx` page dans le `BatchData` dossier et faites glisser un panneau à partir de la boîte à outils vers le concepteur (voir Figure 2). Définir le panneau s `ID` propriété `DisplayInterface`. Lorsque vous ajoutez le panneau vers le concepteur, son `Height` et `Width` propriétés sont définies à 50px et 125px, respectivement. Effacer les ces valeurs de propriété à partir de la fenêtre Propriétés.


[![Faites glisser un panneau à partir de la boîte à outils vers le Concepteur](batch-inserting-vb/_static/image5.png)](batch-inserting-vb/_static/image4.png)

**Figure 2**: faites glisser un panneau à partir de la boîte à outils vers le concepteur ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image6.png))


Ensuite, faites glisser un contrôle bouton et GridView dans le panneau de configuration. Définir le bouton s `ID` propriété `ProcessShipment` et son `Text` propriété pour le processus de livraison du produit. Définir les opérations de mappage GridView `ID` propriété `ProductsGrid` et, à partir de sa balise active, liez-le à une nouvelle ObjectDataSource nommé `ProductsDataSource`. Configurer l’ObjectDataSource afin d’extraire ses données à partir de la `ProductsBLL` classe s `GetProducts` (méthode). Dans la mesure où ce GridView est utilisé uniquement pour afficher des données, définir les listes déroulantes dans la mise à jour, insertion et supprimer des onglets à (None). Cliquez sur Terminer pour terminer l’Assistant Configurer la Source de données.


[![Afficher les données retournées à partir de la méthode GetProducts ProductsBLL classe s](batch-inserting-vb/_static/image8.png)](batch-inserting-vb/_static/image7.png)

**Figure 3**: afficher les données retournées à partir de la `ProductsBLL` classe s `GetProducts` (méthode) ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image9.png))


[![Définir les listes déroulantes dans la mise à jour, insertion et supprimer des onglets à (None)](batch-inserting-vb/_static/image11.png)](batch-inserting-vb/_static/image10.png)

**Figure 4**: la valeur est la liste déroulante répertorie dans la mise à jour, insertion et supprimer des onglets (aucun) ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image12.png))


À l’issue de l’Assistant ObjectDataSource, Visual Studio ajoutera BoundFields et un CheckBoxField pour les champs de données de produit. Supprimer tout sauf la `ProductName`, `CategoryName`, `SupplierName`, `UnitPrice`, et `Discontinued` champs. Vous pouvez apporter toutes les personnalisations esthétiques. J’ai décidé de mettre en forme le `UnitPrice` champ en tant que valeur monétaire, de réorganiser les champs et certains des champs renommés `HeaderText` valeurs. Également configurer le contrôle GridView pour inclure la pagination et tri de prise en charge en activant les cases à cocher Activer la pagination et activer le tri dans la balise active de s GridView.

Après avoir ajouté les contrôles du panneau, bouton, GridView et ObjectDataSource et personnaliser les champs de s GridView, votre balisage déclaratif s de page doit ressembler à ce qui suit :


[!code-aspx[Main](batch-inserting-vb/samples/sample1.aspx)]

Notez que le balisage pour le bouton et un GridView apparaissent au sein de l’ouverture et la fermeture `<asp:Panel>` balises. Étant donné que ces contrôles sont dans le `DisplayInterface` Panneau de configuration, nous pouvons les masquer en définissant simplement le panneau s `Visible` propriété `False`. Étape 3 examine la modification par programme le panneau s `Visible` propriété en réponse à un bouton Cliquez pour afficher une interface unique tout en masquant l’autre.

Prenez un moment pour consulter notre progression via un navigateur. Comme le montre la Figure 5, vous devez voir un bouton de la livraison du produit processus au-dessus d’un GridView qui répertorie les dix produits à la fois.


[![Le contrôle GridView répertorie les produits et offre le tri et la pagination des capacités](batch-inserting-vb/_static/image14.png)](batch-inserting-vb/_static/image13.png)

**Figure 5**: le contrôle GridView répertorie les produits et offre le tri et les fonctionnalités de pagination ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image15.png))


## <a name="step-2-creating-the-inserting-interface"></a>Étape 2 : Création de l’Interface insertion

Avec l’interface d’affichage complet, nous vous êtes prêt à créer l’interface d’insertion. Pour ce didacticiel, permettent de créer une interface d’insertion qui invite à entrer une valeur de fournisseur et de catégorie unique et permet ensuite à l’utilisateur à entrer jusqu'à cinq noms de produits et les valeurs de prix d’unité s. Avec cette interface, l’utilisateur peut ajouter une à cinq nouveaux produits qui partagent tous la même catégorie et le même fournisseur, mais les prix et les noms de produit unique.

Commencez par faire glisser un panneau à partir de la boîte à outils vers le concepteur, en le plaçant sous existant `DisplayInterface` Panneau de configuration. Définir le `ID` propriété de ce récemment ajoutés Panneau de configuration pour `InsertingInterface` et définissez son `Visible` propriété `False`. Nous allons ajouter le code qui définit le `InsertingInterface` panneau s `Visible` propriété `True` à l’étape 3. Également effacer le volet s `Height` et `Width` les valeurs de propriété.

Ensuite, nous devons créer l’interface d’insertion qui a été indiqué dans la Figure 1. Cette interface peut être créée par le biais de diverses techniques HTML, mais nous allons utiliser une assez simple : une table de quatre colonnes et d’une ligne sept.

> [!NOTE]
> Lors de l’entrée du balisage HTML `<table>` éléments, je préfère utiliser la vue de Source. Lors de Visual Studio dispose des outils permettant d’ajouter `<table>` éléments via le concepteur, le concepteur semble tout trop disposé à injecter qui pour `style` paramètres dans le balisage. Une fois que j’ai créé le `<table>` balisage, j’ai généralement y revenir au concepteur pour ajouter les contrôles Web et définir leurs propriétés. Lors de la création de tables avec des lignes et colonnes prédéfinies, je préfère à l’aide de code HTML statique plutôt que la [contrôle Web Table](https://msdn.microsoft.com/library/system.web.ui.webcontrols.table.aspx) , car tous les contrôles Web placés dans un contrôle Web de la Table est accessible uniquement à l’aide de la `FindControl("controlID")` modèle. Je ne, cependant, utilise des contrôles Web de la Table pour les tables dimensionnées de manière dynamique (ceux dont la lignes ou colonnes est basés sur certains critères spécifiés par l’utilisateur ou de base), depuis le Web de la Table contrôle peut être construit par programme.


Entrez le balisage suivant dans le `<asp:Panel>` balises de la `InsertingInterface` panneau :


[!code-html[Main](batch-inserting-vb/samples/sample2.html)]

Cela `<table>` balisage n’inclut pas tous les contrôles Web encore, nous allons ajouter ceux momentanément. Notez que chaque `<tr>` élément contient un paramètre de classe CSS particulier : `BatchInsertHeaderRow` pour la ligne d’en-tête dans lequel le fournisseur et la catégorie DropDownList passera ; `BatchInsertFooterRow` pour la ligne de pied de page dans laquelle ajouter des produits à partir d’expédition et les boutons Annuler passera ; et remplacement `BatchInsertRow` et `BatchInsertAlternatingRow` valeurs pour les lignes contenant le produit et l’unité de prix des contrôles de zone de texte. Je ve créé des classes CSS correspondantes dans le `Styles.css` fichier pour donner à l’interface insertion une apparence semblable à des contrôles GridView et DetailsView nous contrôle ve utilisé tout au long de ces didacticiels. Ces classes CSS sont présentées ci-dessous.


[!code-css[Main](batch-inserting-vb/samples/sample3.css)]

Avec cette balise est entrée, revenir à la vue de conception. Cela `<table>` doit apparaître comme une table de quatre colonnes et d’une ligne sept dans le concepteur, comme le montre la Figure 6.


[![L’Interface d’insertion est composé d’une colonne de quatre, Table de sept lignes](batch-inserting-vb/_static/image17.png)](batch-inserting-vb/_static/image16.png)

**Figure 6**: l’Interface d’insertion est composé d’une colonne de quatre, Table de sept lignes ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image18.png))


Nous re désormais prêt à ajouter les contrôles Web à l’interface d’insertion. Faites glisser deux DropDownList à partir de la boîte à outils dans les cellules appropriées dans la table une pour le fournisseur et l’autre pour la catégorie.

Définir le fournisseur DropDownList s `ID` propriété `Suppliers` et la lier à un nouveau ObjectDataSource nommé `SuppliersDataSource`. Configurer le nouveau ObjectDataSource pour récupérer ses données à partir de la `SuppliersBLL` classe s `GetSuppliers` (méthode) et définissez la mise à jour onglet liste de déroulante s (aucun). Cliquez sur Terminer pour terminer l’Assistant.


[![Configurer pour utiliser la méthode de GetSuppliers SuppliersBLL classe s ObjectDataSource](batch-inserting-vb/_static/image20.png)](batch-inserting-vb/_static/image19.png)

**Figure 7**: configurer ObjectDataSource à utiliser le `SuppliersBLL` classe s `GetSuppliers` (méthode) ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image21.png))


Ont le `Suppliers` DropDownList affichage le `CompanyName` champ de données et utilisez le `SupplierID` de champ de données en tant que son `ListItem` valeurs s.


[![Afficher le champ de données de CompanyName et utiliser SupplierID comme valeur](batch-inserting-vb/_static/image23.png)](batch-inserting-vb/_static/image22.png)

**Figure 8**: affichage du `CompanyName` champ de données et utilisez `SupplierID` comme valeur ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image24.png))


Nommez la liste DropDownList deuxième `Categories` et la lier à un nouveau ObjectDataSource nommé `CategoriesDataSource`. Configurer le `CategoriesDataSource` ObjectDataSource à utiliser le `CategoriesBLL` classe s `GetCategories` méthode ; définir la liste déroulante répertorie dans la mise à jour et supprimer des onglets à (None) cliquez sur termine pour terminer l’Assistant. Ont enfin, l’affichage DropDownList le `CategoryName` champ de données et utilisez le `CategoryID` comme valeur.

Une fois que ces deux DropDownList ont été ajoutés et lié à ObjectDataSources correctement configuré, votre écran doit ressembler à la Figure 9.


[![La ligne d’en-tête contient à présent les fournisseurs et les catégories DropDownList](batch-inserting-vb/_static/image26.png)](batch-inserting-vb/_static/image25.png)

**Figure 9**: l’en-tête de ligne contient maintenant le `Suppliers` et `Categories` DropDownList ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image27.png))


Nous devons maintenant créer les zones de texte pour collecter le nom et le prix pour chaque nouveau produit. Faites glisser un contrôle de zone de texte à partir de la boîte à outils vers le concepteur pour chacune des lignes de nom et le prix de cinq produit. Définir le `ID` propriétés des zones de texte à `ProductName1`, `UnitPrice1`, `ProductName2`, `UnitPrice2`, `ProductName3`, `UnitPrice3`, et ainsi de suite.

Ajouter un CompareValidator après chaque du prix unitaire zones de texte, définissant le `ControlToValidate` propriété appropriés `ID`. Également définir le `Operator` propriété `GreaterThanEqual`, `ValueToCompare` à 0, et `Type` à `Currency`. Ces paramètres indiquer CompareValidator pour vous assurer que le prix, si vous avez entré, est une valeur de devise valide qui est supérieur ou égal à zéro. Définir le `Text` propriété \*, et `ErrorMessage` au prix doit être supérieur ou égal à zéro. En outre, veuillez omettre tous les symboles monétaires.

> [!NOTE]
> L’interface d’insertion n’inclut pas tous les contrôles RequiredFieldValidator, même si le `ProductName` champ dans le `Products` table de base de données n’autorise pas `NULL` valeurs. Il s’agit, car nous voulons permettre à l’utilisateur d’entrer jusqu'à cinq produits. Par exemple, si l’utilisateur à fournir le produit nom et le prix unitaire pour les trois premières lignes, si vous laissez les deux dernières lignes vide, il d suffit d’ajouter trois nouveaux produits au système. Étant donné que `ProductName` est nécessaire, toutefois, nous devons vérifier par programmation pour garantir que si un prix unitaire est entré qu’une valeur de nom de produit correspondant est fournie. Nous les aborderons cette vérification à l’étape 4.


Lors de la validation de l’entrée utilisateur s, CompareValidator publie les données non valides si la valeur contient un symbole monétaire. Ajouter un $ devant chacun des zones de texte comme un indice visuel qui indique à l’utilisateur d’omettre le symbole monétaire lorsque vous entrez le prix du prix unitaire.

Enfin, ajoutez un contrôle ValidationSummary dans le `InsertingInterface` Panneau de configuration, les paramètres de son `ShowMessageBox` propriété `True` et son `ShowSummary` propriété `False`. Avec ces paramètres, si l’utilisateur entre une valeur du prix unitaire non valide, un astérisque apparaît en regard les contrôles de zone de texte incriminées et le contrôle ValidationSummary affiche un messagebox côté client qui affiche le message d’erreur que nous avons spécifié précédemment.

À ce stade, votre écran doit ressembler à la Figure 10.


[![L’Interface insertion inclut désormais les zones de texte pour les produits nom et le prix](batch-inserting-vb/_static/image29.png)](batch-inserting-vb/_static/image28.png)

**Figure 10**: l’insertion Interface maintenant inclut des zones de texte pour les noms de produits et les prix ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image30.png))


Nous devons ensuite ajouter les produits à ajouter à partir des boutons d’expédition et annuler à la ligne de pied de page. Faites glisser deux contrôles bouton à partir de la boîte à outils dans le pied de page de l’interface d’insertion, définissant les boutons `ID` propriétés à `AddProducts` et `CancelButton` et `Text` propriétés pour ajouter des produits à partir d’expédition et Annuler, respectivement. En outre, définissez le `CancelButton` contrôle s `CausesValidation` propriété `false`.

Enfin, nous devons ajouter un contrôle étiquette qui affiche des messages d’état pour les deux interfaces. Par exemple, lorsqu’un utilisateur ajoute correctement une nouvelle livraison de produits, nous souhaitons retourner à l’interface d’affichage et afficher un message de confirmation. Si, toutefois, l’utilisateur fournit un prix pour un nouveau produit mais s’est arrêté le nom du produit, nous devons afficher un message d’avertissement depuis le `ProductName` champ est obligatoire. Étant donné que nous avons besoin de ce message à afficher pour les deux interfaces, placez-le en haut de la page en dehors de panneaux.

Faites glisser un contrôle étiquette Web à partir de la boîte à outils vers le haut de la page dans le concepteur. Définir le `ID` propriété `StatusLabel`, désactivez out le `Text` propriété et définissez la `Visible` et `EnableViewState` propriétés à `False`. Comme nous l’avons vu dans les didacticiels précédents, définissant le `EnableViewState` propriété `False` nous permet par programmation modifier les valeurs de propriété d’étiquette s et demandez-lui de revenir automatiquement à leurs valeurs par défaut sur la publication (postback) suivante. Cela simplifie le code pour afficher un message d’état en réponse à une action de l’utilisateur qui disparaît lors de la publication suivante. Enfin, définissez la `StatusLabel` contrôle s `CssClass` propriété à avertissement, qui est le nom d’une classe CSS définie dans `Styles.css` qui affiche le texte dans une police de grande taille, en italique, gras, rouge.

La figure 11 illustre le concepteur Visual Studio après que l’étiquette a été ajouté et configuré.


[![Placer le contrôle StatusLabel ci-dessus les deux contrôles de panneau](batch-inserting-vb/_static/image32.png)](batch-inserting-vb/_static/image31.png)

**Figure 11**: Place le `StatusLabel` contrôle ci-dessus les deux contrôles de panneau ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image33.png))


## <a name="step-3-switching-between-the-display-and-inserting-interfaces"></a>Étape 3 : Basculer entre l’affichage et en insérant des Interfaces

À ce stade, nous avons terminé le balisage pour notre affichage et en insérant des interfaces, mais nous re laissait toujours avec deux tâches :

- Basculer entre l’affichage et en insérant des interfaces
- Ajout de produits dans l’expédition de la base de données

Actuellement, l’interface d’affichage est visible mais l’interface d’insertion est masquée. Il s’agit, car le `DisplayInterface` panneau s `Visible` propriété est définie sur `True` (la valeur par défaut), tandis que le `InsertingInterface` panneau s `Visible` propriété est définie sur `False`. Pour basculer entre les deux interfaces, nous devons simplement chaque contrôle s toggle `Visible` valeur de propriété.

Nous souhaitons déplacer à partir de l’interface d’affichage à l’interface d’insertion lorsque l’utilisateur clique sur le bouton de la livraison du produit. Par conséquent, créer un gestionnaire d’événements pour ce bouton s `Click` événement qui contient le code suivant :


[!code-vb[Main](batch-inserting-vb/samples/sample4.vb)]

Ce code masque simplement la `DisplayInterface` volet et affiche le `InsertingInterface` Panneau de configuration.

Ensuite, créez les gestionnaires d’événements pour les produits d’ajouter des contrôles d’expédition et un bouton Annuler dans l’interface d’insertion. Un clic sur un de ces boutons, nous devons revenir à l’interface d’affichage. Créer `Click` gestionnaires d’événements pour les deux contrôles bouton afin qu’ils appellent `ReturnToDisplayInterface`, une méthode, nous allons ajouter momentanément. En plus de masquage le `InsertingInterface` panneau et affichant le `DisplayInterface` Panneau de configuration, le `ReturnToDisplayInterface` méthode doit retourner les contrôles Web à leur état de modification préalable. Cela implique la définition de la DropDownList `SelectedIndex` propriétés sur 0 et éliminant la `Text` propriétés des contrôles de zone de texte.

> [!NOTE]
> Envisagez ce qui peut se produire si nous fonctionné t revenir aux contrôles à leur état de modification préalable avant de retourner à l’interface d’affichage. Un utilisateur peut cliquer sur le bouton de la livraison du produit, entrez les produits à partir de l’expédition, puis cliquez sur Ajouter des produits à partir de l’expédition. Cela aurait ajouté les produits et renvoie l’utilisateur à l’interface d’affichage. À ce stade, l’utilisateur souhaite ajouter une autre expédition. Lorsque vous cliquez sur le bouton de la livraison du produit processus qu'elles restaurent à l’interface d’insertion, mais la liste DropDownList sélections et les valeurs de la zone de texte seraient toujours remplies de leurs valeurs précédentes.


[!code-vb[Main](batch-inserting-vb/samples/sample5.vb)]

Les deux `Click` gestionnaires d’événements simplement appellent le `ReturnToDisplayInterface` (méthode), même si nous revenons sur les produits à ajouter à partir de l’expédition `Click` Gestionnaire d’événements dans l’étape 4 et ajoutez le code pour enregistrer les produits. `ReturnToDisplayInterface` démarre en renvoyant le `Suppliers` et `Categories` DropDownList pour leurs premières options. Les deux constantes `firstControlID` et `lastControlID` marquer le début et fin des valeurs d’index de contrôle utilisés pour nommer le nom et l’unité de prix du produit zones de texte de l’insertion de l’interface et sont utilisés dans les limites de la `For` boucle qui définit le `Text`sauvegarder des propriétés des contrôles de zone de texte à une chaîne vide. Enfin, les panneaux `Visible` des propriétés sont rétablies afin que l’interface d’insertion est masquée et l’interface de l’affichage indiqué.

Prenez un moment pour tester cette page dans un navigateur. Lors de la visite de tout d’abord la page, vous devez voir l’interface d’affichage comme indiqué dans la Figure 5. Cliquez sur le bouton de la livraison du produit. La page est publiée et que vous devriez maintenant voir l’interface insertion comme indiqué dans la Figure 12. Cliquez sur soit l’ajout de produits à partir des boutons d’expédition ou sur Annuler pour revenir à l’interface d’affichage.

> [!NOTE]
> Lorsque vous affichez l’interface d’insertion, prenez un moment pour tester le CompareValidators sur le prix unitaire zones de texte. Vous devez voir une messagebox côté client d’avertissement lorsque vous cliquez sur Ajouter des produits bouton expédition avec les valeurs de devise non valide ou des prix avec une valeur inférieure à zéro.


[![L’Interface d’insertion s’affiche après avoir cliqué sur le bouton de livraison de produit de processus](batch-inserting-vb/_static/image35.png)](batch-inserting-vb/_static/image34.png)

**Figure 12**: l’Interface d’insertion s’affiche après avoir cliqué sur le bouton de livraison de produit de processus ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image36.png))


## <a name="step-4-adding-the-products"></a>Étape 4 : Ajout de produits

Tout ce qui reste pour ce didacticiel consiste à enregistrer les produits à la base de données dans les produits à ajouter à partir du bouton d’expédition s `Click` Gestionnaire d’événements. Cela est possible en créant un `ProductsDataTable` et en ajoutant un `ProductsRow` instance pour chacun de ces noms de produit fournies. Une fois ces `ProductsRow` s ont été ajoutées, nous apporterons un appel à la `ProductsBLL` classe s `UpdateWithTransaction` méthode en passant le `ProductsDataTable`. N’oubliez pas que le `UpdateWithTransaction` (méthode), qui a été créé dans le [encapsulant les Modifications de base de données dans une Transaction](wrapping-database-modifications-within-a-transaction-vb.md) passes (didacticiels), le `ProductsDataTable` à la `ProductsTableAdapter` s `UpdateWithTransaction` (méthode). À partir de là, une transaction ADO.NET est démarrée et les problèmes de TableAdatper un `INSERT` instruction à la base de données pour chaque ajouté `ProductsRow` dans le DataTable. En supposant que tous les produits sont ajoutés sans erreur, que la transaction est validée, sinon elle est restaurée.

Le code pour ajouter des produits à partir du bouton d’expédition s `Click` Gestionnaire d’événements doit également effectuer un peu de vérification des erreurs. Dans la mesure où il n’y a aucune RequiredFieldValidators utilisé dans l’interface d’insertion, un utilisateur peut entrer un prix pour un produit tout en omettant son nom. Étant donné que le nom de produit s est obligatoire, si une telle condition qui se déroule nous devons avertir l’utilisateur et de poursuivre les insertions. L’ensemble `Click` code gestionnaire d’événements suit :


[!code-vb[Main](batch-inserting-vb/samples/sample6.vb)]

Le Gestionnaire d’événements démarre en s’assurant que le `Page.IsValid` propriété retourne une valeur de `True`. Si elle retourne `False`, puis qui peut être l’une ou plusieurs de la CompareValidators rapportent des données non valides ; dans ce cas nous ne souhaitons pas tenter d’insérer les produits entrés ou nous allons vous retrouver avec une exception lorsque vous tentez d’affecter le prix unitaire entré par l’utilisateur valeur à la `ProductsRow` s `UnitPrice` propriété.

Ensuite, une nouvelle `ProductsDataTable` instance est créée (`products`). Un `For` boucle est utilisée pour itérer au sein du produit nom et le prix unitaire zones de texte et la `Text` propriétés sont lues dans les variables locales `productName` et `unitPrice`. Si l’utilisateur a entré une valeur pour le prix unitaire, mais pas pour le nom de produit correspondant, la `StatusLabel` affiche le message si vous fournissez une unité de prix que vous devez également inclure le nom du produit et le Gestionnaire d’événements est quitté.

Si un nom de produit a été fourni, une nouvelle `ProductsRow` instance est créée à l’aide de la `ProductsDataTable` s `NewProductsRow` (méthode). Cette nouvelle `ProductsRow` instance s `ProductName` propriété est définie sur le produit actuel nommez la zone de texte lors de la `SupplierID` et `CategoryID` sont les propriétés assignées à le `SelectedValue` propriétés de la DropDownList dans l’en-tête de l’interface s insertion. Si l’utilisateur a entré une valeur pour le prix du produit s, elle est affectée à la `ProductsRow` instance s `UnitPrice` propriété ; sinon, la propriété est gauche non affecté, ce qui entraîne un `NULL` valeur pour `UnitPrice` dans la base de données. Enfin, le `Discontinued` et `UnitsOnOrder` propriétés sont attribuées aux valeurs codées en dur `False` et 0, respectivement.

Une fois que les propriétés ont été attribuées à la `ProductsRow` instance, il est ajouté à la `ProductsDataTable`.

À la fin de la `For` boucle, nous vérifions si tous les produits ont été ajoutés. L’utilisateur peut, après tout, avez cliqué sur Ajouter des produits à partir d’expédition avant d’entrer les noms de produits ou de prix. S’il existe au moins un produit dans le `ProductsDataTable`, le `ProductsBLL` classe s `UpdateWithTransaction` méthode est appelée. Ensuite, les données sont de nouveau liées à la `ProductsGrid` GridView afin que les produits nouvellement ajoutées seront affiche dans l’interface d’affichage. Le `StatusLabel` est mis à jour pour afficher un message de confirmation et le `ReturnToDisplayInterface` est appelé, en masquant l’insertion d’interface et affichant l’interface d’affichage.

Si aucun produit ont été entrés, l’interface d’insertion reste affichée mais le message Qu'aucun produit ont été ajoutés. Entrez les noms de produits et le prix unitaire dans les zones de texte s’affiche.

Figure 13, 14 et 15 afficher l’insertion et affichent des interfaces en action. Dans la Figure 13, l’utilisateur a entré une valeur du prix unitaire sans un nom de produit correspondant. Figure 14 illustre l’interface d’affichage après trois nouveaux produits ont été ajoutés avec succès, tandis que la Figure 15 illustre deux des produits qui vient d’être ajoutés dans le contrôle GridView (le troisième est dans la page précédente).


[![Un nom de produit est requis lors de la saisie de prix unitaire](batch-inserting-vb/_static/image38.png)](batch-inserting-vb/_static/image37.png)

**Figure 13**: nom du produit A est requis lorsque vous entrez vous un prix unitaire ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image39.png))


[![Trois jardins nouvelles ont été ajoutées pour le fournisseur Mayumi s](batch-inserting-vb/_static/image41.png)](batch-inserting-vb/_static/image40.png)

**Figure 14**: trois nouveaux jardins ont été ajoutées pour s fournisseur Mayumi ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image42.png))


[![Vous trouverez les nouveaux produits dans la dernière Page du contrôle GridView](batch-inserting-vb/_static/image44.png)](batch-inserting-vb/_static/image43.png)

**Figure 15**: les nouveaux produits se trouvent dans la dernière Page du GridView ([cliquez pour afficher l’image en taille réelle](batch-inserting-vb/_static/image45.png))


> [!NOTE]
> Le lot d’insertion de logique utilisée dans ce didacticiel, les insertions est encapsulé dans l’étendue de transaction. Pour vérifier ceci, volontairement introduire une erreur au niveau de la base de données. Par exemple, au lieu d’affecter le nouveau `ProductsRow` instance s `CategoryID` valeur à la propriété sélectionnée dans le `Categories` DropDownList, affecter une valeur, tel que `i * 5`. Ici `i` est l’indexeur de boucle et a des valeurs comprises entre 1 et 5. Par conséquent, lorsque l’ajout de deux ou plusieurs produits dans le lot insérer le premier produit aura valide `CategoryID` valeur (5), mais les produits suivants aura `CategoryID` valeurs qui ne correspondent pas jusqu'à `CategoryID` des valeurs dans le `Categories` table. L’effet net est que lors de la première `INSERT` réussit, les conditions suivantes échouent avec une violation de contrainte de clé étrangère. Étant donné que l’insertion de lot est atomique, le premier `INSERT` sera restaurée, retour de la base de données à son état avant le processus d’insertion de lot a commencé.


## <a name="summary"></a>Récapitulatif

Cet aspect ainsi deux didacticiels précédents, nous avons créé les interfaces qui permettent la mise à jour, suppression, et insertion de lots de données, chacun utilisé la prise en charge de transactions que nous avons ajouté à la couche d’accès aux données dans le [encapsulant les Modifications de base de données dans une Transaction](wrapping-database-modifications-within-a-transaction-vb.md) didacticiel. Pour certains scénarios, ces interfaces utilisateur de traitement par lots améliorent considérablement l’efficacité d’utilisateur final en réduisant le nombre de clics, les publications et les changements de contexte de clavier-souris, tout en conservant l’intégrité des données sous-jacentes.

Ce didacticiel termine notre comment utiliser les données par lot. L’ensemble suivant de didacticiels explore une variété de scénarios avancés de couche d’accès aux données, y compris à l’aide de procédures stockées dans les méthodes de s TableAdapter, configuration des paramètres au niveau de la connexion et de la commande dans la couche DAL, le chiffrement des chaînes de connexion et bien plus encore !

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Des prospects réviseurs pour ce didacticiel ont été Hilton Giesenow et S ren Jacob Lauritsen. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](batch-deleting-vb.md)
