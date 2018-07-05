---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
title: Ajout d’une colonne GridView de cases d’option (c#) | Microsoft Docs
author: rick-anderson
description: Ce didacticiel explique comment ajouter une colonne de cases d’option à un contrôle GridView à permettre aux utilisateurs de la sélection d’une ligne unique de façon plus intuitive...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 32377145-ec25-4715-8370-a1c590a331d5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
msc.type: authoredcontent
ms.openlocfilehash: 1e1691b3c0c5fb576f25b84e8f4d7125a8d0c698
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366914"
---
<a name="adding-a-gridview-column-of-radio-buttons-c"></a>Ajout d’une colonne GridView de cases d’option (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_CS.exe) ou [télécharger le PDF](adding-a-gridview-column-of-radio-buttons-cs/_static/datatutorial51cs1.pdf)

> Ce didacticiel explique comment ajouter une colonne de cases d’option à un contrôle GridView à permettre aux utilisateurs un moyen plus intuitif de la sélection d’une seule ligne du contrôle GridView.


## <a name="introduction"></a>Introduction

Le contrôle GridView offre un large éventail de fonctionnalités intégrées. Il inclut un nombre de champs différents pour l’affichage de texte, des images, des liens hypertexte et des boutons. Il prend en charge les modèles pour davantage de personnalisation. En quelques clics de souris, il s possible pour rendre un GridView où chaque ligne peut être sélectionnée via un bouton, ou pour activer la modification ou la suppression de fonctionnalités. En dépit de l’abondance des fonctionnalités fournies, il arrive souvent que les situations dans lesquelles supplémentaires, non pris en charge les fonctionnalités doivent être ajoutés. Dans ce didacticiel et les deux suivants, nous allons examiner comment améliorer les fonctionnalités de s GridView pour inclure des fonctionnalités supplémentaires.

Ce didacticiel et celle qui suit vous concentrer sur l’amélioration du processus de sélection de ligne. Comme examiné dans le [maître/détail utilisant un GridView maître avec un DetailView détail](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md), nous pouvons ajouter un CommandField au GridView qui inclut un bouton Sélectionner. Lorsque vous cliquez dessus, une publication (postback) s’ensuit et les opérations de mappage GridView `SelectedIndex` propriété est mise à jour à l’index de la ligne dont bouton Sélectionner l’utilisateur a cliqué. Dans le [maître/détail utilisant un GridView maître avec un DetailView détail](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) didacticiel, nous avons vu comment utiliser cette fonctionnalité pour afficher les détails de la ligne sélectionnée de la GridView.

Bien que le bouton de sélection fonctionne dans de nombreuses situations, il peut ne pas fonctionne aussi pour d’autres. Au lieu d’utiliser un bouton, les deux autres éléments d’interface utilisateur sont couramment utilisés pour la sélection : la case d’option et la case à cocher. Nous pouvons augmenter le contrôle GridView afin qu’au lieu d’un bouton Sélectionner, chaque ligne contient une case d’option ou une case à cocher. Dans les scénarios où l’utilisateur peut sélectionner uniquement un des enregistrements de GridView, la case d’option peut être préférée sur le bouton de sélection. Dans les situations où l’utilisateur peut potentiellement sélectionner plusieurs enregistrements, comme dans une application de messagerie web, où un utilisateur peut souhaiter sélectionner plusieurs messages à supprimer de la case à cocher offre des fonctionnalités qui ne sont pas disponible à partir du bouton Sélectionner ou bouton radio interfaces utilisateur.

Ce didacticiel explique comment ajouter une colonne de cases d’option dans le contrôle GridView. Le didacticiel de procédure décrit l’utilisation de cases à cocher.

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>Étape 1 : Création de l’amélioration de Pages Web de GridView

Avant de commencer amélioration du contrôle GridView pour inclure une colonne de cases d’option, laissez s tout d’abord prendre un moment pour créer les pages ASP.NET dans notre projet de site Web que nous avons besoin pour ce didacticiel et les deux suivants. Commencez par ajouter un nouveau dossier nommé `EnhancedGridView`. Ensuite, ajoutez les pages ASP.NET suivantes à ce dossier, en veillant à associer chaque page avec le `Site.master` page maître :

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`


![Ajouter les Pages ASP.NET pour les didacticiels de SqlDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.gif)

**Figure 1**: ajoutez les Pages ASP.NET pour les didacticiels de SqlDataSource


Comme dans les autres dossiers, `Default.aspx` dans le `EnhancedGridView` dossier répertorie les didacticiels dans sa section. N’oubliez pas que le `SectionLevelTutorialListing.ascx` contrôle utilisateur fournit cette fonctionnalité. Par conséquent, ajoutez ce contrôle utilisateur à `Default.aspx` en le faisant glisser à partir de l’Explorateur de solutions sur la page s en mode Création.


[![Ajouter le contrôle utilisateur de SectionLevelTutorialListing.ascx à Default.aspx](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.png)

**Figure 2**: ajouter la `SectionLevelTutorialListing.ascx` contrôle utilisateur à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.png))


Enfin, ajoutez ces quatre pages sous forme d’entrées pour le `Web.sitemap` fichier. Plus précisément, ajoutez le balisage suivant après l’utilisation du contrôle SqlDataSource `<siteMapNode>`:


[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample1.xml)]

Après la mise à jour `Web.sitemap`, prenez un moment pour afficher le site Web de didacticiels via un navigateur. Le menu de gauche inclut désormais les éléments de la modification, insertion et suppression des didacticiels.


![Le plan de Site inclut maintenant des entrées permettant d’améliorer la les didacticiels de GridView](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.gif)

**Figure 3**: le plan de Site inclut maintenant des entrées permettant d’améliorer la les didacticiels de GridView


## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>Étape 2 : Afficher les fournisseurs dans un GridView

Pour ce didacticiel s permettre de générer un GridView qui répertorie les fournisseurs à partir des États-Unis, avec chaque ligne GridView fournissant une case d’option. Après avoir sélectionné un fournisseur via le bouton radio, l’utilisateur peut afficher les produits de s fournisseur en cliquant sur un bouton. Bien que cette tâche peut sembler triviale, il existe un nombre de subtilités qui le rendent particulièrement délicat. Avant de nous plonger dans ces subtilités, permettent de s tout d’abord obtenir une liste des fournisseurs de GridView.

Commencez par ouvrir le `RadioButtonField.aspx` page dans le `EnhancedGridView` dossier en faisant glisser un GridView à partir de la boîte à outils vers le concepteur. Définir les opérations de mappage GridView `ID` à `Suppliers` et, à partir de sa balise active, choisissez de créer une nouvelle source de données. En particulier, créer un ObjectDataSource nommé `SuppliersDataSource` qui extrait ses données à partir de la `SuppliersBLL` objet.


[![Créer un nouveau ObjectDataSource nommé SuppliersDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.png)

**Figure 4**: créer une nouvelle nommée de ObjectDataSource `SuppliersDataSource` ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.png))


[![Configurer pour utiliser la classe SuppliersBLL ObjectDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.png)

**Figure 5**: configurer ObjectDataSource à utiliser le `SuppliersBLL` classe ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.png))


Étant donné que nous ne souhaitons pas répertorier les fournisseurs aux États-Unis d’Amérique, choisir la `GetSuppliersByCountry(country)` méthode dans la liste déroulante dans l’onglet sélection.


[![Configurer pour utiliser la classe SuppliersBLL ObjectDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.png)

**Figure 6**: configurer ObjectDataSource à utiliser le `SuppliersBLL` classe ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.png))


À partir de l’onglet mise à jour, sélectionnez (aucun), cliquez sur Suivant.


[![Configurer pour utiliser la classe SuppliersBLL ObjectDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.png)

**Figure 7**: configurer ObjectDataSource à utiliser le `SuppliersBLL` classe ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.png))


Dans la mesure où le `GetSuppliersByCountry(country)` méthode accepte un paramètre, l’Assistant Configurer la Source de données nous demande la source de ce paramètre. Pour spécifier une valeur codée en dur (États-Unis, dans cet exemple), laissez le paramètre de liste déroulante de la source de la valeur None et entrez la valeur par défaut dans la zone de texte. Cliquez sur Terminer pour terminer l’Assistant.


[![Utiliser des États-Unis comme valeur par défaut pour le paramètre de pays](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.png)

**Figure 8**: USA utiliser comme valeur par défaut pour le `country` paramètre ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.png))


À l’issue de l’Assistant, le contrôle GridView inclura un BoundField pour chacun des champs de données de fournisseur. Supprimer tout sauf la `CompanyName`, `City`, et `Country` BoundFields et renommez le `CompanyName` BoundFields `HeaderText` propriété au fournisseur. Après cela, la syntaxe déclarative GridView et ObjectDataSource doit ressembler à ce qui suit.


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample2.aspx)]

Pour ce didacticiel, permettent d’autoriser l’utilisateur d’afficher le fournisseur sélectionné des produits de s sur la même page que la liste des fournisseurs, ou sur une autre page s. Pour ce faire, ajoutez deux contrôles bouton à la page. Je ve ensemble la `ID` s de ces deux boutons à `ListProducts` et `SendToProducts`, avec l’idée que quand `ListProducts` clic est effectué sur une publication (postback) se produit et les produits du fournisseur sélectionné s seront répertoriés sur la même page, mais lorsque `SendToProducts` est activé, l’utilisateur sera whisked vers une autre page qui répertorie les produits.

La figure 9 illustre la `Suppliers` GridView et les deux serveurs Web bouton contrôle lorsqu’ils sont affichés via un navigateur.


[![Ces fournisseurs à partir des États-Unis ont leur nom, ville et les informations de pays répertoriés](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.png)

**Figure 9**: les fournisseurs à partir des États-Unis ont leur nom, ville et pays dans la liste d’informations ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.png))


## <a name="step-3-adding-a-column-of-radio-buttons"></a>Étape 3 : Ajout d’une colonne de cases d’option

À ce stade le `Suppliers` GridView a trois BoundFields affichant le nom de la société, la ville et le pays de chaque fournisseur aux États-Unis d’Amérique. Il manque encore une colonne de cases d’option, toutefois. Malheureusement, GridView ne t incluent un intégrés RadioButtonField, sinon nous pourrait simplement ajouter à la grille et le faire. Au lieu de cela, nous pouvons ajouter un TemplateField et configurer ses `ItemTemplate` pour afficher un bouton radio, ce qui entraîne un bouton radio pour chaque ligne GridView.

Au départ, nous pourrions partons du principe que l’interface utilisateur de votre choix peut être implémentée en ajoutant un contrôle de case d’option Web pour le `ItemTemplate` de TemplateField. Bien que cela ajoutera effectivement une case d’option unique à chaque ligne du contrôle GridView, les boutons radio ne peuvent pas être regroupées et par conséquent, ne sont pas mutuellement exclusives. Autrement dit, un utilisateur final est en mesure de sélectionner plusieurs cases d’option simultanément à partir de la GridView.

Même si à l’aide d’un TemplateField de contrôles Web RadioButton n’offre pas les fonctionnalités que nous avons besoin, s permettent d’implémenter cette approche, car il s judicieux d’examiner la raison pour laquelle les cases qui en résulte ne sont pas regroupées. Commencez par ajouter un TemplateField au GridView fournisseurs, rendant le champ à l’extrême gauche. Ensuite, à partir de la balise active de s GridView, cliquez sur le lien Modifier les modèles et faites glisser un contrôle de case d’option Web à partir de la boîte à outils dans le s TemplateField `ItemTemplate` (voir Figure 10). Définir la case d’option s `ID` propriété `RowSelector` et `GroupName` propriété `SuppliersGroup`.


[![Ajouter un contrôle RadioButton à ItemTemplate](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.png)

**Figure 10**: ajouter un contrôle RadioButton à le `ItemTemplate` ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.png))


Après avoir apporté ces ajouts via le concepteur, votre balisage de s GridView doit ressembler à ce qui suit :


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample3.aspx)]

La case d’option s [ `GroupName` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx) est celui qui est utilisé pour regrouper une série de cases d’option. Tous les contrôles de case d’option avec le même `GroupName` valeur sont considérées comme groupées ; qu’une seule case d’option peut être sélectionnée à partir d’un groupe à la fois. Le `GroupName` propriété spécifie la valeur de la case de rendu s `name` attribut. Le navigateur examine les boutons radio `name` attributs pour déterminer la case d’option bouton regroupements.

Avec le contrôle de case d’option Web ajouté à la `ItemTemplate`, visitez cette page via un navigateur, puis cliquez sur les boutons de case d’option dans les lignes de grille s. Notez comment les boutons radio ne sont pas regroupés, ce qui permet de sélectionner toutes les lignes, comme la Figure 11 montre.


[![Les boutons de Radio de s GridView ne sont pas regroupées](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.png)

**Figure 11**: GridView s elles ne sont pas regroupées ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.png))


Les boutons radio ne sont pas regroupés est parce que leur rendu `name` attributs sont différents, en dépit de la même `GroupName` paramètre de propriété. Pour afficher ces différences, faire une afficher la Source à partir du navigateur et examiner le balisage du bouton radio :


[!code-html[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample4.html)]

Notez comment les deux le `name` et `id` attributs ne sont pas les valeurs exactes, comme spécifié dans la fenêtre Propriétés, mais sont précédés d’un nombre d’autres `ID` valeurs. Supplémentaires `ID` valeurs ajoutées au premier rang de rendu `id` et `name` attributs sont le `ID` s de la case d’option boutons contrôles parents la `GridViewRow` s `ID` s, les opérations de mappage GridView `ID`, le Contrôle s de contenu `ID`et le Web Form s `ID`. Ces `ID` s sont ajoutées afin que les rendus contrôle Web dans le contrôle GridView a une valeur unique `id` et `name` valeurs.

Rendus les besoins de contrôle un autre `name` et `id` , car il s’agit comment le navigateur identifie de façon unique chaque contrôle sur la côté client et comment il identifie le serveur web quelle action ou de modification s’est produite lors de la publication. Par exemple, imaginez que nous souhaitons exécuter du code côté serveur chaque fois qu’un s RadioButton vérifiée état a été modifié. Nous pourrions le faire en définissant le s RadioButton `AutoPostBack` propriété `true` et la création d’un gestionnaire d’événements pour le `CheckChanged` événement. Toutefois, si le rendu `name` et `id` valeurs pour tous les boutons radio étaient les mêmes, de publication (postback) Impossible de déterminer sur quel utilisateur a cliqué sur RadioButton.

Court de celui-ci est que nous ne pouvons pas créer une colonne de cases d’option dans un GridView utilisant le contrôle de case d’option Web. Au lieu de cela, nous devons utiliser plutôt archaïques techniques pour vous assurer que le balisage approprié est injecté dans chaque ligne GridView.

> [!NOTE]
> Comme le contrôle de case d’option Web, le bouton radio lorsqu’il est ajouté à un modèle, contrôle HTML inclura unique `name` attribut, ce qui les cases dans la grille non groupée. Si vous n’êtes pas familiarisé avec les contrôles HTML, n’hésitez pas à ignorer cette remarque, comme les contrôles HTML sont rarement utilisées, en particulier dans ASP.NET 2.0. Mais si vous souhaitez en savoir plus, consultez [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx) entrée de blog de s [les contrôles Web et les contrôles HTML](http://www.odetocode.com/Articles/348.aspx).


## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>À l’aide d’un contrôle Literal pour injecter le balisage du bouton Radio

Pour correctement regrouper tous les boutons de case d’option dans le GridView, nous devons injecter manuellement le balisage de boutons de case d’option dans le `ItemTemplate`. Chaque bouton radio a besoin de la même `name` d’attribut, mais doivent avoir une valeur unique `id` attribut (au cas où nous souhaitons accéder à un bouton radio via un script côté client). Une fois un utilisateur sélectionne un bouton radio et la page effectue une publication, le navigateur renverra la valeur de la case d’option sélectionnée s `value` attribut. Par conséquent, chaque case d’option devez unique `value` attribut. Publication (postback) pour finir, nous devons veillez à ajouter le `checked` attribut à l’une case d’option qui est sélectionnée, sinon une fois que l’utilisateur effectue une sélection et billets de retour, les boutons radio retournera à leur état par défaut (tous les non sélectionnés).

Il existe deux approches qui peuvent être effectuées pour injecter du balisage de bas niveau dans un modèle. Une consiste à effectuer un mélange de balisage et les appels aux méthodes définies dans la classe code-behind de la mise en forme. Cette technique est abordée dans le [à l’aide de TemplateFields dans le contrôle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) didacticiel. Dans notre cas, il peut se présenter quelque chose comme :


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample5.aspx)]

Ici, `GetUniqueRadioButton` et `GetRadioButtonValue` seraient des méthodes définies dans la classe code-behind qui retourné approprié `id` et `value` des valeurs pour chaque case d’option d’attribut. Cette approche fonctionne bien pour l’affectation de la `id` et `value` des attributs, mais s’avère insuffisant lorsque vous avez besoin spécifier le `checked` valeur d’attribut, car la syntaxe de liaison de données est exécutée uniquement lorsque des données sont tout d’abord liées au GridView. Par conséquent, si le contrôle GridView présente un état d’affichage est activé, les méthodes de mise en forme ne se déclenche lorsque la page est chargée (ou lorsque le contrôle GridView est explicitement de nouveau lié à la source de données) et par conséquent, la fonction qui définit le `checked` attribut a gagné t être appelée sur publication (postback). Il s un problème subtil au lieu de cela et un peu au-delà de la portée de cet article, donc je vais laisser à cela. Toutefois, j’ai faire vous encourage à essayer d’utiliser l’approche ci-dessus et par le biais d’utiliser au point où vous allez rester bloqué. Tel un t exercice a gagné obtenir n’importe quel plus proche vers une version opérationnelle, mais elle permet favoriser une meilleure compréhension de la GridView et le cycle de vie de liaison de données.

L’autre approche pour injecter personnalisé, balisage de bas niveau dans un modèle et l’approche que nous allons utiliser pour ce didacticiel consiste à ajouter un [contrôle Literal](https://msdn.microsoft.com/library/sz4949ks(VS.80).aspx) au modèle. Ensuite, dans le s GridView `RowCreated` ou `RowDataBound` Gestionnaire d’événements, le contrôle Literal est accessible par programmation et son `Text` propriété définie sur le balisage à émettre.

Commencez par supprimer le contrôle RadioButton à partir de la s TemplateField `ItemTemplate`, en la remplaçant par un contrôle littéral. Définir le contrôle littéral s `ID` à `RadioButtonMarkup`.


[![Ajouter un contrôle littéral à ItemTemplate](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.png)

**Figure 12**: ajouter un contrôle littéral pour le `ItemTemplate` ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.png))


Ensuite, créez un gestionnaire d’événements pour les opérations de mappage GridView `RowCreated` événement. Le `RowCreated` événement est déclenché une fois pour chaque ligne ajoutée, ou non les données sont en cours de nouveau liées au GridView. Cela signifie que même sur une publication (postback) lorsque les données sont rechargées à partir de l’état d’affichage, le `RowCreated` événement est toujours déclenché, et c’est pourquoi nous utilisons à la place de `RowDataBound` (qui déclenche uniquement lorsque les données sont liées explicitement pour les données de contrôle Web).

Dans ce gestionnaire d’événements, nous ne souhaitons continuer si nous re traiter une ligne de données. Pour chaque ligne de données que vous souhaitez référencer par programme le `RadioButtonMarkup` contrôle Literal et définissez son `Text` propriété au balisage à émettre. Comme le montre le code suivant, le balisage émis crée une case d’option bouton dont la propriété `name` attribut a la valeur `SuppliersGroup`, dont `id` attribut a la valeur `RowSelectorX`, où *X* est l’index de la ligne de GridView, et dont `value` attribut est défini sur l’index de la ligne GridView.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample6.cs)]

Lorsqu’une ligne GridView est sélectionnée et une publication (postback) se produit, nous nous intéressons à la `SupplierID` du fournisseur sélectionné. Par conséquent, on pourrait penser que la valeur de chaque case doit être réelle `SupplierID` (plutôt que l’index de la ligne GridView). Cela peut fonctionner dans certaines circonstances, il serait un risque de sécurité pour aveuglément accepter et traiter un `SupplierID`. Notre GridView, répertorie, par exemple, seuls les fournisseurs aux États-Unis d’Amérique. Toutefois, si le `SupplierID` est passée directement à partir de la case, Nouveautés pour tout utilisateur espiègle de manipuler le `SupplierID` valeur envoyée dans la publication (postback) ? À l’aide de l’index de ligne en tant que le `value`et l’obtention de la `SupplierID` lors de la publication à partir de la `DataKeys` collection, nous pouvons garantir que l’utilisateur est uniquement à l’aide d’une de la `SupplierID` valeurs associées à une des lignes GridView.

Après avoir ajouté ce code de gestionnaire d’événements, prenez une minute pour tester la page dans un navigateur. Notez tout d’abord, cette case d’option qu’un seul bouton dans la grille peut être sélectionné à la fois. Toutefois, quand en sélectionnant une case d’option et en cliquant sur un des boutons, une publication (postback) et les boutons de case d’option tous les restaurer à leur état initial (autrement dit, sur la publication (postback), la case d’option sélectionnée n’est plus sélectionnée). Pour résoudre ce problème, nous avons besoin d’augmenter la `RowCreated` Gestionnaire d’événements pour qu’elle inspecte l’index du bouton de case d’option sélectionnée envoyé à partir de la publication (postback) et ajoute le `checked="checked"` le balisage émis de l’index de ligne correspond à l’attribut.

Lorsqu’une publication (postback) se produit, le navigateur envoie la `name` et `value` du bouton de case d’option sélectionnée. La valeur peut être récupérée par programmation à l’aide de `Request.Form["name"]`. Le [ `Request.Form` propriété](https://msdn.microsoft.com/library/system.web.httprequest.form.aspx) fournit un [ `NameValueCollection` ](https://msdn.microsoft.com/library/system.collections.specialized.namevaluecollection.aspx) représentant les variables de formulaire. Les variables de formulaire sont les noms et valeurs des champs de formulaire dans la page web et renvoyés par le navigateur web chaque fois qu’une publication (postback) s’ensuit. Étant donné que le rendu `name` attribut des cases d’option dans le contrôle GridView est `SuppliersGroup`, lorsque la page web est publié, le navigateur envoie `SuppliersGroup=valueOfSelectedRadioButton` sur le serveur web (ainsi que les autres champs de formulaire). Ces informations peuvent ensuite être accessible à partir du `Request.Form` à l’aide de la propriété : `Request.Form["SuppliersGroup"]`.

Depuis que nous allons besoin pour déterminer le bouton de case d’option sélectionnée d’index pas uniquement dans le `RowCreated` Gestionnaire d’événements, mais dans le `Click` ajouter des gestionnaires d’événements pour les contrôles bouton Web, permettent de s un `SuppliersSelectedIndex` propriété à la classe code-behind qui retourne `-1`si aucune case d’option a été sélectionnée et l’index sélectionné si un des boutons radio est sélectionné.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample7.cs)]

Avec cette propriété est ajoutée, nous savons à ajouter le `checked="checked"` balisage dans le `RowCreated` Gestionnaire d’événements lorsque `SuppliersSelectedIndex` est égal à `e.Row.RowIndex`. Mettre à jour le Gestionnaire d’événements pour inclure cette logique :


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample8.cs)]

Avec cette modification, la case d’option sélectionnée reste sélectionnée après une publication (postback). Maintenant que nous avons la possibilité de spécifier quel bouton de case d’option est sélectionné, nous pouvons modifier le comportement afin que lorsque la page a été visitée tout d’abord, le premier bouton de case d’option de ligne s GridView a été sélectionné (plutôt que n’avoir aucune cases d’option sélectionnée par défaut, qui est en cours comportement). Pour que le premier bouton de case d’option sélectionné par défaut, il suffit de changer le `if (SuppliersSelectedIndex == e.Row.RowIndex)` instruction à la suivante : `if (SuppliersSelectedIndex == e.Row.RowIndex || (!Page.IsPostBack && e.Row.RowIndex == 0))`.

À ce stade, nous avons ajouté une colonne de cases d’option groupées à GridView qui autorise une seule ligne GridView d’être sélectionnées et conservés entre les postbacks. Les étapes suivantes consistent à afficher les produits fournis par le fournisseur sélectionné. À l’étape 4 nous verrons comment rediriger l’utilisateur vers une autre page, envoyer le long de l’élément sélectionné `SupplierID`. À l’étape 5, nous verrons comment afficher les produits de s fournisseur sélectionné dans un GridView sur la même page.

> [!NOTE]
> Au lieu d’utiliser TemplateField (le focus de cette longue étape 3), nous pourrions créer un personnalisé `DataControlField` classe qui restitue l’interface utilisateur appropriée et la fonctionnalité. Le [ `DataControlField` classe](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.aspx) est la classe de base à partir de laquelle dérivent le BoundField, CheckBoxField, TemplateField et autres champs prédéfinis de GridView et DetailsView. Créer une personnalisée `DataControlField` classe signifie que la colonne de cases d’option peut être ajoutée uniquement à l’aide de la syntaxe déclarative et permettrait également répliquent la fonctionnalité sur les autres pages web et d’autres applications web beaucoup plus faciles.


Si vous avez déjà créé personnalisé, compilé contrôles dans ASP.NET, cependant, vous savez que cela nécessite une grande quantité de gros du travail et s’accompagne d’un hôte de subtilités et les cas ambigus qui doivent être gérées soigneusement. Par conséquent, nous ne prend pas mise en œuvre d’une colonne de cases d’option comme un personnalisé `DataControlField` classe pour l’instant et gardez-le avec l’option TemplateField. Par exemple, nous aurons l’occasion d’Explorer la création, à l’aide et déploiement personnalisé `DataControlField` classes dans un futur didacticiel !

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>Étape 4 : Afficher les produits de s fournisseur sélectionné dans une Page distincte

Une fois que l’utilisateur a sélectionné une ligne GridView, nous devons afficher les produits de s le fournisseur sélectionné. Dans certains cas, nous souhaiterons peut-être à afficher ces produits dans une page distincte, dans d’autres que nous pourrions préférez le faire dans la même page. Laisser s tout d’abord examiner comment afficher les produits dans une page distincte ; à l’étape 5, nous examinerons Ajout d’un contrôle GridView à `RadioButtonField.aspx` pour afficher les produits du fournisseur sélectionné s.

Actuellement, il existe deux contrôles bouton Web sur la page `ListProducts` et `SendToProducts`. Lorsque le `SendToProducts` bouton, nous souhaitons envoyer l’utilisateur à `~/Filtering/ProductsForSupplierDetails.aspx`. Cette page a été créée dans le [filtrage de maître/détail sur deux Pages](../masterdetail/master-detail-filtering-across-two-pages-cs.md) didacticiel et affiche les produits pour le fournisseur dont `SupplierID` sont transmises via le champ de chaîne de requête nommé `SupplierID`.

Pour fournir cette fonctionnalité, créez un gestionnaire d’événements pour le `SendToProducts` bouton s `Click` événement. À l’étape 3, nous avons ajouté la `SuppliersSelectedIndex` propriété qui retourne l’index de la ligne dont case d’option est sélectionnée. Correspondants `SupplierID` peuvent être récupérées à partir de la s GridView `DataKeys` collecte et l’utilisateur peuvent ensuite être envoyées à `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` à l’aide de `Response.Redirect("url")`.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample9.cs)]

Ce code fonctionne parfaitement tant qu’un des boutons radio est sélectionné dans le contrôle GridView. Si, au départ, le contrôle GridView ne dispose pas des boutons de case d’option sélectionnées, l’utilisateur clique sur le `SendToProducts` bouton, `SuppliersSelectedIndex` sera `-1`, ce qui provoquera une exception levée depuis `-1` est hors de la plage d’index de la `DataKeys`collection. Il s’agit pas d’un problème, toutefois, si vous avez décidé de mettre à jour le `RowCreated` Gestionnaire d’événements comme indiqué à l’étape 3 afin de disposer de la première case dans le contrôle GridView initialement sélectionné.

Pour prendre en charge un `SuppliersSelectedIndex` valeur `-1`, ajoutez un contrôle Web Label à la page au-dessus de la GridView. Définir son `ID` propriété `ChooseSupplierMsg`, ses `CssClass` propriété `Warning`, ses `EnableViewState` et `Visible` propriétés à `false`et son `Text` propriété Veuillez choisir un fournisseur à partir de la grille. La classe CSS `Warning` affiche du texte dans une police rouge, italique, gras, grand et est défini dans `Styles.css`. En définissant le `EnableViewState` et `Visible` propriétés à `false`, l’étiquette n’est pas restitué à l’exception pour uniquement les publications où le contrôle s `Visible` propriété est définie par programmation sur `true`.


[![Ajouter un contrôle étiquette au-dessus de GridView](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image21.png)

**Figure 13**: ajouter une étiquette Web contrôle ci-dessus GridView ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image22.png))


Ensuite, augmenter la `Click` Gestionnaire d’événements pour afficher le `ChooseSupplierMsg` si l’étiquette `SuppliersSelectedIndex` est inférieure à zéro et rediriger l’utilisateur vers `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` dans le cas contraire.


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample10.cs)]

Visitez la page dans un navigateur et d’un clic le `SendToProducts` bouton avant de sélectionner un fournisseur de GridView. Comme le montre la Figure 14, cela affiche le `ChooseSupplierMsg` étiquette. Ensuite, sélectionnez un fournisseur et cliquez sur le `SendToProducts` bouton. Ce sera vous tous à une page qui répertorie les produits fournis par le fournisseur sélectionné. La figure 15 illustre la `ProductsForSupplierDetails.aspx` page lorsque le fournisseur Bigfoot brasseries a été sélectionné.


[![L’étiquette de ChooseSupplierMsg est affichée si aucun fournisseur n’est sélectionnée](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image23.png)

**Figure 14**: le `ChooseSupplierMsg` étiquette est affichée si aucun fournisseur n’est sélectionnée ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image24.png))


[![Les produits de s fournisseur sélectionné sont affichés dans ProductsForSupplierDetails.aspx](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image25.png)

**Figure 15**: le fournisseur sélectionné s produits sont affichés dans `ProductsForSupplierDetails.aspx` ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image26.png))


## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>Étape 5 : Afficher les produits de s fournisseur sélectionné dans la même Page

À l’étape 4, nous avons vu comment envoyer l’utilisateur vers une autre page web pour afficher le fournisseur sélectionné des produits de s. Vous pouvez également les produits de s fournisseur sélectionné peuvent figurer sur la même page. Pour illustrer cela, nous allons ajouter un autre contrôle GridView à `RadioButtonField.aspx` pour afficher les produits du fournisseur sélectionné s.

Dans la mesure où nous voulons uniquement ce GridView de produits pour afficher une fois qu’un fournisseur a été sélectionné, ajoutez un contrôle de panneau de configuration Web sous le `Suppliers` GridView, en définissant son `ID` à `ProductsBySupplierPanel` et son `Visible` propriété `false`. Dans le panneau de configuration, ajoutez le texte de produits pour le fournisseur sélectionné, suivie d’un GridView nommé `ProductsBySupplier`. À partir de la balise active de s GridView, choisir de lier à un nouveau ObjectDataSource nommé `ProductsBySupplierDataSource`.


[![Lier le contrôle ProductsBySupplier GridView à un nouveau ObjectDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image27.png)

**Figure 16**: lier le `ProductsBySupplier` contrôle GridView à une nouvelle ObjectDataSource ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image28.png))


Ensuite, configurez l’ObjectDataSource à utiliser le `ProductsBLL` classe. Dans la mesure où nous voulons uniquement récupérer ces produits fournis par le fournisseur sélectionné, spécifiez que ObjectDataSource doit appeler la `GetProductsBySupplierID(supplierID)` méthode pour récupérer ses données. Sélectionnez (aucun) dans les listes déroulantes dans la mise à jour, insertion et supprimer des onglets.


[![Configurer pour utiliser la méthode GetProductsBySupplierID(supplierID) ObjectDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image29.png)

**Figure 17**: configurer ObjectDataSource à utiliser le `GetProductsBySupplierID(supplierID)` (méthode) ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image30.png))


[![Définir les listes déroulantes (None) dans la mise à jour, insertion et supprimer des onglets](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image31.png)

**Figure 18**: définir les listes de liste déroulante (aucun) dans la mise à jour, insertion et supprimer des onglets ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image32.png))


Après avoir configuré l’instruction SELECT, mettre à jour, insérer et supprimer des onglets, cliquez sur Suivant. Dans la mesure où le `GetProductsBySupplierID(supplierID)` méthode attend un paramètre d’entrée, l’Assistant créer une Source de données nous invite à spécifier la source pour la valeur du paramètre s.

Nous avons quelques options ici, en spécifiant la source de la valeur du paramètre s. Nous aurions pu utiliser l’objet de paramètre par défaut et affectez par programme la valeur de la `SuppliersSelectedIndex` propriété pour le paramètre s `DefaultValue` propriété dans le s ObjectDataSource `Selecting` Gestionnaire d’événements. Faire référence à la [définition par programmation les valeurs des paramètres de l’ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md) didacticiel pour revoir assigner par programmation des valeurs aux paramètres s ObjectDataSource.

Vous pouvez également utiliser un ControlParameter et nous faire référence à la `Suppliers` GridView s [ `SelectedValue` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx) (voir Figure 19). Les opérations de mappage GridView `SelectedValue` propriété retourne le `DataKey` valeur correspondant à la [ `SelectedIndex` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex.aspx). Afin que cette option fonctionne, nous devons définir par programmation le s GridView `SelectedIndex` sélectionné à la propriété de ligne lorsque le `ListProducts` clic sur le bouton. Autre avantage, en définissant le `SelectedIndex`, l’enregistrement sélectionné effectuera sur le `SelectedRowStyle` définis dans le `DataWebControls` thème (un arrière-plan jaune).


[![Utiliser un ControlParameter pour spécifier le SelectedValue s GridView comme Source du paramètre](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image33.png)

**Figure 19**: un ControlParameter permet de spécifier le s GridView SelectedValue comme Source de paramètre ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image34.png))


À la fin de l’Assistant, Visual Studio ajoute automatiquement les champs pour les champs de données de produit s. Supprimer tout sauf la `ProductName`, `CategoryName`, et `UnitPrice` BoundFields et modifier le `HeaderText` propriétés à catégorie, les produits et les prix. Configurer le `UnitPrice` BoundField afin que sa valeur est mise en forme comme une devise. Après avoir apporté ces modifications, le balisage déclaratif s panneau, GridView et ObjectDataSource doit ressembler à ce qui suit :


[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample11.aspx)]

Pour effectuer cet exercice, nous devons définir le s GridView `SelectedIndex` propriété le `SelectedSuppliersIndex` et le `ProductsBySupplierPanel` panneau s `Visible` propriété `true` lorsque le `ListProducts` bouton. Pour ce faire, créez un gestionnaire d’événements pour le `ListProducts` contrôle Web Button s `Click` événement et ajoutez le code suivant :


[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample12.cs)]

Si un fournisseur n’a pas été sélectionné à partir de la GridView, le `ChooseSupplierMsg` étiquette est affichée et la `ProductsBySupplierPanel` panneau masqué. Sinon, si un fournisseur a été sélectionné, le `ProductsBySupplierPanel` s’affiche et les opérations de mappage GridView `SelectedIndex` propriété est mise à jour.

La figure 20 montre les résultats une fois que le fournisseur Bigfoot brasseries a été sélectionné et les produits à afficher sur le bouton de Page a été cliqué.


[![Les produits fournis par Bigfoot brasseries sont répertoriés sur la même Page](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image35.png)

**Figure 20**: les produits fournis par Bigfoot brasseries sont répertoriés sur la même Page ([cliquez pour afficher l’image en taille réelle](adding-a-gridview-column-of-radio-buttons-cs/_static/image36.png))


## <a name="summary"></a>Récapitulatif

Comme indiqué dans le [maître/détail utilisant un GridView maître avec un DetailView détail](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) didacticiel, les enregistrements peuvent être sélectionnés à partir d’un GridView utilisant un CommandField dont `ShowSelectButton` propriété est définie sur `true`. Mais le CommandField affiche ses boutons sous la forme régulière des boutons de commande, des liens ou des images. Une interface utilisateur de sélection de ligne autre consiste à fournir une case d’option ou une case à cocher dans chaque ligne GridView. Dans ce didacticiel, nous avons examiné comment ajouter une colonne de cases d’option.

Malheureusement, l’ajout d’une colonne de cases d’option boutons n’est pas en tant que simple, ni comme prévu. Il n’existe aucun RadioButtonField intégré qui peut être ajouté à un clic de souris, et l’utilisation du contrôle de case d’option Web au sein d’un TemplateField présente son propre ensemble de problèmes. Au final, pour fournir une telle interface nous soit obligé de créer un personnalisé `DataControlField` classe ou le recours à l’injection de code HTML approprié en TemplateField pendant la `RowCreated` événement.

Avoir exploré comment ajouter une colonne de cases d’option, nous le faire porter notre attention vers l’ajout d’une colonne de cases à cocher. Avec une colonne de cases à cocher, un utilisateur peut sélectionner une ou plusieurs lignes de GridView et puis effectuer une opération sur toutes les lignes sélectionnées (par exemple, en sélectionnant un ensemble d’e-mails à partir d’un client de messagerie basée sur le web et puis en choisissant de supprimer tous les e-mails sélectionnés). Dans le didacticiel suivant, nous allons voir comment ajouter une colonne de ce type.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été David Suru. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](adding-a-gridview-column-of-checkboxes-cs.md)
