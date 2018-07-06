---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
title: Affichage de données avec les contrôles DataList et Repeater (c#) | Microsoft Docs
author: rick-anderson
description: Dans les didacticiels précédents, nous avons utilisé le contrôle GridView pour afficher des données. À partir de ce didacticiel, nous intéresser à la construction des modèles de création de rapports courants avec...
ms.author: aspnetcontent
ms.date: 09/13/2006
ms.assetid: 0591cacc-b34b-4cf6-885e-2c9953bb0946
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 6368580d43e6216ea75d3013fb13aa59d9eeb6ba
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805939"
---
<a name="displaying-data-with-the-datalist-and-repeater-controls-c"></a>Affichage de données avec les contrôles DataList et Repeater (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_29_CS.exe) ou [télécharger le PDF](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/datatutorial29cs1.pdf)

> Dans les didacticiels précédents, nous avons utilisé le contrôle GridView pour afficher des données. À partir de ce didacticiel, nous intéresser à la construction des modèles de création de rapports courants avec les contrôles DataList et Repeater, en commençant par les principes fondamentaux de l’affichage des données avec ces contrôles.


## <a name="introduction"></a>Introduction

Dans tous les exemples dans le passé des 28 didacticiels, s’il est nécessaire pour afficher plusieurs enregistrements à partir d’une source de données que nous avons activés pour le contrôle GridView. Le contrôle GridView affiche une ligne pour chaque enregistrement dans la source de données, l’affichage des champs de données d’enregistrement s dans les colonnes. Bien que le contrôle GridView rend un composant logiciel enfichable pour afficher, Parcourir, trier, modifier et supprimer des données, son apparence est un peu bruts. En outre, le responsable du balisage pour la structure de s GridView est corrigée inclut HTML `<table>` avec une ligne de table (`<tr>`) pour chaque enregistrement et une cellule de tableau (`<td>`) pour chaque champ.

Pour fournir un degré de personnalisation de l’apparence et le balisage rendu supérieur lors de l’affichage de plusieurs enregistrements, ASP.NET 2.0 offre les contrôles DataList et Repeater (qui étaient également disponibles dans la version d’ASP.NET 1.x). Les contrôles DataList et Repeater restituer leur contenu à l’aide de modèles plutôt que de BoundFields, CheckBoxFields, ButtonFields et ainsi de suite. Comme le contrôle GridView, le contrôle DataList rendu sous la forme d’un élément HTML `<table>`, mais autorise les données de plusieurs enregistrements de la source à afficher par ligne de table. Le contrôle Repeater, ne restitue quant à eux, aucune balise supplémentaire à ce que vous explicitement spécifiez et êtes un candidat idéal lorsque vous avez besoin d’un contrôle précis sur le balisage émis.

Sur les didacticiels ensuite douzaines ou c’est le cas, nous examinerons la création de modèles de création de rapports courants avec les contrôles DataList et Repeater, en commençant par les principes fondamentaux de l’affichage des données avec ces modèles de contrôles. Nous verrons comment formater ces contrôles, comment modifier la disposition d’enregistrements de source de données dans DataList, les scénarios maître/détails courants, utiles pour modifier et supprimer des données, comment parcourir les enregistrements et ainsi de suite.

## <a name="step-1-adding-the-datalist-and-repeater-tutorial-web-pages"></a>Étape 1 : Ajout de contrôles DataList et Repeater didacticiel Web Pages

Avant de commencer ce didacticiel, laissez s tout d’abord prendre un moment pour ajouter les pages ASP.NET que nous avons besoin pour ce didacticiel et les didacticiels quelques suivants affaire de l’affichage des données à l’aide des contrôles DataList et Repeater. Commencez par créer un nouveau dossier dans le projet nommé `DataListRepeaterBasics`. Ensuite, ajoutez les pages ASP.NET cinq suivantes à ce dossier, présentant toutes les configurer pour utiliser la page maître `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `Formatting.aspx`
- `RepeatColumnAndDirection.aspx`
- `NestedControls.aspx`


![Créez un dossier DataListRepeaterBasics et ajouter les Pages ASP.NET didacticiel](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image1.png)

**Figure 1**: créer un `DataListRepeaterBasics` dossier et ajouter les Pages ASP.NET didacticiel


Ouvrir le `Default.aspx` page et faites glisser le `SectionLevelTutorialListing.ascx` contrôle utilisateur à partir de la `UserControls` dossier sur l’aire de conception. Ce contrôle utilisateur, que nous avons créée dans le [Pages maîtres et Navigation du Site](../introduction/master-pages-and-site-navigation-cs.md) didacticiel, énumère le plan du site et affiche les didacticiels à partir de la section en cours dans une liste à puces.


[![Ajouter le contrôle utilisateur de SectionLevelTutorialListing.ascx à Default.aspx](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image3.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image2.png)

**Figure 2**: ajouter la `SectionLevelTutorialListing.ascx` contrôle utilisateur à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image4.png))


Afin de disposer de l’affichage de liste à puces les didacticiels contrôles DataList et Repeater, que nous allons créer, nous avons besoin de les ajouter au plan du site. Ouvrez le `Web.sitemap` fichier, puis ajoutez le balisage suivant après la balise de nœud de plan de site Ajout de boutons personnalisés :


[!code-xml[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample1.xml)]


![Mettre à jour le plan du Site pour inclure les nouvelles Pages ASP.NET](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image5.png)

**Figure 3**: mettre à jour le plan du Site pour inclure les nouvelles Pages ASP.NET


## <a name="step-2-displaying-product-information-with-the-datalist"></a>Étape 2 : Affichage des informations de produit avec le contrôle DataList

Comme pour le contrôle FormView, le contrôle DataList s sortie rendue dépend modèles plutôt que BoundFields, CheckBoxFields et ainsi de suite. Contrairement à FormView, le contrôle DataList est conçu pour afficher un ensemble d’enregistrements au lieu d’un solitaires. Permettent de commencer ce didacticiel avec un aperçu des informations de produit de liaison dans un contrôle DataList s. Commencez par ouvrir le `Basics.aspx` page dans le `DataListRepeaterBasics` dossier. Ensuite, faites glisser un contrôle DataList à partir de la boîte à outils vers le concepteur. Comme le montre la Figure 4, avant de spécifier les modèles DataList s, le Concepteur de l’affiche sous la forme d’une zone grise.


[![Faites glisser le contrôle DataList à partir de la boîte à outils vers le Concepteur](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image7.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image6.png)

**Figure 4**: faites glisser la DataList à partir de la boîte à outils vers le concepteur ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image8.png))


À partir du contrôle DataList s balise active, ajouter un nouveau ObjectDataSource et configurez-le pour utiliser le `ProductsBLL` classe s `GetProducts` (méthode). Dans la mesure où nous re création d’un contrôle DataList en lecture seule dans ce didacticiel, définir la liste déroulante (aucun) dans le s Assistant INSERT, UPDATE et DELETE d’onglets.


[![Choisir de créer un nouveau ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image10.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image9.png)

**Figure 5**: choisir pour créer un nouveau ObjectDataSource ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image11.png))


[![Configurer pour utiliser la classe ProductsBLL ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image13.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image12.png)

**Figure 6**: configurer ObjectDataSource à utiliser le `ProductsBLL` classe ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image14.png))


[![Récupérer des informations sur tous les produits à l’aide de la méthode GetProducts](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image16.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image15.png)

**Figure 7**: récupérer des informations sur toutes les produits à l’aide du `GetProducts` (méthode) ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image17.png))


Après avoir configuré les ObjectDataSource et son association à la DataList via sa balise active, Visual Studio crée automatiquement un `ItemTemplate` dans le contrôle DataList qui affiche le nom et la valeur de chaque champ de données retourné par la source de données (consultez la balisage ci-dessous). Cette valeur par défaut `ItemTemplate` aspect est identique à celle des modèles créés automatiquement lors de la liaison d’une source de données pour le contrôle FormView via le concepteur.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample2.aspx)]

> [!NOTE]
> Rappelez-vous que, lors de la liaison d’une source de données à un contrôle FormView via la balise active de s FormView, Visual Studio a créé un `ItemTemplate`, `InsertItemTemplate`, et `EditItemTemplate`. Avec le contrôle DataList, toutefois, uniquement un `ItemTemplate` est créé. Il s’agit, car le contrôle DataList n’a pas intégrés même édition et insertion de prise en charge offerte par le contrôle FormView. Le contrôle DataList ne contient-elle pas edit et delete liés à des événements et la modification et suppression de prise en charge ne peuvent être ajoutés avec un peu de code, mais s’il y a aucune simple out-of-the-box prise en charge en tant qu’avec le contrôle FormView. Nous verrons comment inclure la modification et suppression dans un futur didacticiel de prise en charge avec le contrôle DataList.


Laissez s prenez un moment pour améliorer l’apparence de ce modèle. Au lieu d’afficher tous les champs de données, permettent d’afficher uniquement le-s nom de produit, le fournisseur, la catégorie, la quantité par unité et prix unitaire s. En outre, s permettent d’afficher le nom dans un `<h4>` titre et pour disposer les champs restants à l’aide un `<table>` sous le titre.

Pour effectuer ces modifications, que vous pouvez soit utiliser le fonctionnalités du concepteur à partir du contrôle DataList s smart tag cliquez sur le lien Modifier les modèles ou vous pouvez modifier le modèle manuellement à l’aide de la syntaxe déclarative s de page de modification de modèle. Si vous utilisez l’option Modifier les modèles dans le concepteur, votre balisage qui en résulte peut ne pas correspond exactement le balisage suivant, mais lorsqu’ils sont affichés via un navigateur doit ressembler très à l’écran illustré à la Figure 8.


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample3.aspx)]

> [!NOTE]
> L’exemple ci-dessus utilise étiquette de contrôles Web dont la propriété `Text` la valeur de la syntaxe de liaison de données est attribuée à la propriété. Vous pouvez également nous pourrions ont omis les étiquettes purement et simplement en tapant dans uniquement la syntaxe de liaison de données. Autrement dit, au lieu d’utiliser `<asp:Label ID="CategoryNameLabel" runat="server" Text='<%# Eval("CategoryName") %>' />` nous pourrions avoir utilisé à la place la syntaxe déclarative `<%# Eval("CategoryName") %>`.


En laissant dans les contrôles Web Label, toutefois, offre deux avantages. Tout d’abord, il fournit un moyen plus simple pour mettre en forme les données en fonction des données, comme nous allons le voir dans le didacticiel suivant. Deuxièmement, l’option Modifier les modèles dans la syntaxe de liaison de données déclarative concepteur n t complet qui apparaît en dehors de la partie du contrôle Web. Au lieu de cela, l’interface de modifier les modèles est conçu pour faciliter l’utilisation de balisage statique Web les contrôles et part du principe que toute liaison de données se fera via la boîte de dialogue Modifier les DataBindings, qui est accessible à partir de balises actives de contrôles Web.

Par conséquent, lorsque vous travaillez avec le contrôle DataList, qui offre la possibilité de modifier les modèles via le concepteur, je préfère utiliser des contrôles Web de l’étiquette afin que le contenu est accessible via l’interface de modifier les modèles. Comme nous le verrons bientôt, le Repeater nécessite que le contenu du modèle s être modifiées à partir de la vue de Source. Par conséquent, lors de l’élaboration les modèles de s Repeater je mentionnerai pas souvent le Web de l’étiquette de contrôle, sauf si je sais que j’ai besoin de mettre en forme l’apparence des données texte de la limite en fonction de la logique de programmation.


[![Chaque produit s sortie est restitué à l’aide de DataList s ItemTemplate](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image19.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image18.png)

**Figure 8**: chaque produit s sortie est restitué à l’aide de DataList s `ItemTemplate` ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image20.png))


## <a name="step-3-improving-the-appearance-of-the-datalist"></a>Étape 3 : Améliorer l’apparence du contrôle DataList

Comme le contrôle GridView, le contrôle DataList offre un nombre de propriétés associées au style, tel que `Font`, `ForeColor`, `BackColor`, `CssClass`, `ItemStyle`, `AlternatingItemStyle`, `SelectedItemStyle`, et ainsi de suite. Lorsque vous travaillez avec les contrôles GridView et DetailsView, nous avons créé des fichiers d’apparence dans le `DataWebControls` thème prédéfinis le `CssClass` propriétés pour ces deux contrôles et la `CssClass` propriété pour plusieurs de ses sous-propriétés (`RowStyle` `HeaderStyle`, et ainsi de suite). Permettent de faire de même pour le contrôle DataList s.

Comme indiqué dans le [affichant les données avec ObjectDataSource le](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) (didacticiel), un fichier d’apparence spécifie les propriétés relatives à l’apparence par défaut pour un contrôle Web ; un thème est une collection de fichiers d’apparence, CSS, image et JavaScript qui définissent une présentation particulière pour un site Web. Dans le *affichant les données avec ObjectDataSource le* didacticiel, nous avons créé un `DataWebControls` thème (qui est implémenté en tant que dossier au sein de la `App_Themes` dossier) qui a actuellement, deux fichiers d’apparence - `GridView.skin` et `DetailsView.skin`. Permettent d’ajouter un troisième fichier d’apparence pour spécifier les paramètres de style prédéfini pour le contrôle DataList s.

Pour ajouter un fichier d’apparence, cliquez sur le `App_Themes/DataWebControls` dossier, choisissez Ajouter un nouvel élément et sélectionnez l’option de fichier d’apparence dans la liste. Nommez le fichier `DataList.skin`.


[![Créer un nouveau fichier d’apparence nommé DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image22.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image21.png)

**Figure 9**: créer un nouveau fichier apparence nommé `DataList.skin` ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image23.png))


Utiliser le balisage suivant pour le `DataList.skin` fichier :


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample4.aspx)]

Ces paramètres attribuer les mêmes classes CSS pour les propriétés de DataList appropriées comme ont été utilisés avec les contrôles GridView et DetailsView. Les classes CSS utilisées ici `DataWebControlStyle`, `AlternatingRowStyle`, `RowStyle`, et ainsi de suite sont définies dans le `Styles.css` de fichiers et ont été ajoutées dans les didacticiels précédents.

Avec l’ajout de ce fichier d’apparence, l’apparence de s DataList est mis à jour dans le concepteur (vous devrez peut-être actualiser la vue de concepteur pour voir les effets du nouveau fichier d’apparence ; dans le menu Affichage, cliquez sur Actualiser). Comme le montre la Figure 10, chaque produit en alternance a une couleur d’arrière-plan rose.


[![Créer un nouveau fichier d’apparence nommé DataList.skin](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image25.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image24.png)

**Figure 10**: créer un nouveau fichier apparence nommé `DataList.skin` ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image26.png))


## <a name="step-4-exploring-the-datalist-s-other-templates"></a>Étape 4 : Exploration s autres modèles DataList

Outre le `ItemTemplate`, le contrôle DataList prend en charge les six autres modèles facultatifs :

- `HeaderTemplate` s’il est fourni, ajoute une ligne d’en-tête à la sortie et est utilisé pour rendre cette ligne
- `AlternatingItemTemplate` utilisé pour restituer les éléments de remplacement
- `SelectedItemTemplate` utilisé pour restituer l’élément sélectionné ; l’élément sélectionné est l’élément dont l’index correspond à la DataList s [ `SelectedIndex` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.selectedindex.aspx)
- `EditItemTemplate` utilisé pour restituer l’élément en cours de modification
- `SeparatorTemplate` s’il est fourni, ajoute un séparateur entre chaque élément et est utilisé pour restituer ce séparateur.
- `FooterTemplate` -Si fourni, ajoute une ligne de pied de page à la sortie et est utilisé pour rendre cette ligne

Lorsque vous spécifiez le `HeaderTemplate` ou `FooterTemplate`, le contrôle DataList ajoute une ligne d’en-tête ou pied de page supplémentaire à la sortie rendue. Comme avec l’en-tête de s GridView et le pied de page lignes, l’en-tête et le pied de page dans un contrôle DataList ne sont pas liés aux données. Par conséquent, toute syntaxe de liaison de données dans le `HeaderTemplate` ou `FooterTemplate` que les tentatives d’accès aux données liées retournera une chaîne vide.

> [!NOTE]
> Comme nous l’avons vu dans la [affichant des informations de résumé dans le pied de page/s GridView](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) (didacticiel), tandis que les lignes d’en-tête et pied de page n la syntaxe de liaison de données de la prise en charge t, les informations spécifiques aux données peut être injecté directement dans ces lignes à partir de la GridView s `RowDataBound` Gestionnaire d’événements. Cette technique peut être utilisée pour les deux calculent des totaux cumulés ou autres informations à partir des données liées au contrôle ainsi qu’affecter ces informations pour le pied de page. Ce même concept peut être appliqué pour les contrôles DataList et Repeater ; la seule différence est que pour les contrôles DataList et Repeater de création d’un gestionnaire d’événements pour le `ItemDataBound` événement (au lieu de pour le `RowDataBound` événement).


Dans notre exemple, let s ont le titre d’informations produit affiché en haut du contrôle DataList s provoque un `<h3>` titre. Pour ce faire, ajoutez un `HeaderTemplate` avec le balisage approprié. À partir du concepteur, il est possible en cliquant sur le lien Modifier les modèles dans la balise active DataList s, en choisissant le modèle d’en-tête dans la liste déroulante et en tapant dans le texte après avoir sélectionné l’option de titre 3 à partir du menu déroulant de style liste (voir Figure 11).


[![Ajouter un HeaderTemplate avec les informations de produit du texte](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image28.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image27.png)

**Figure 11**: ajouter un `HeaderTemplate` avec les informations de produit du texte ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image29.png))


Ou bien, il peut être ajouté manière déclarative en entrant le balisage suivant dans le `<asp:DataList>` balises :


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample5.html)]

Pour ajouter un peu d’espace entre chaque liste de produits, permettent s d’ajouter un `SeparatorTemplate` qui inclut une ligne entre chaque section. La balise de règle horizontale (`<hr>`), ajoute un diviseur de ce type. Créer le `SeparatorTemplate` afin qu’il dispose le balisage suivant :


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample6.html)]

> [!NOTE]
> Comme le `HeaderTemplate` et `FooterTemplates`, le `SeparatorTemplate` n’est pas lié à un enregistrement à partir de la source de données et par conséquent ne peut pas directement la source de données liées à la DataList des enregistrements des accès.


Après avoir établi la cet ajout, lorsque vous affichez la page via un navigateur qu’il doit ressembler à la Figure 12. Notez la ligne d’en-tête et la ligne entre chaque liste de produits.


[![Le contrôle DataList inclut une ligne d’en-tête et une règle horizontale entre chaque liste de produits](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image31.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image30.png)

**Figure 12**: le contrôle DataList inclut une ligne d’en-tête et un Horizontal règle entre chaque liste de produits ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image32.png))


## <a name="step-5-rendering-specific-markup-with-the-repeater-control"></a>Étape 5 : Rendu du balisage spécifique avec le contrôle Repeater

Si vous le faites une afficher la Source à partir de votre navigateur lors de la visite de l’exemple de DataList à partir de la Figure 12, vous verrez que le contrôle DataList émet HTML `<table>` qui contient une ligne de table (`<tr>`) avec une cellule de tableau (`<td>`) pour chaque élément lié à la DataList. Cette sortie, en fait, est identique à ce qui est alors émis à partir d’un GridView avec TemplateField unique. Comme nous le verrons dans un futur didacticiel, le contrôle DataList permet une personnalisation plus poussée de la sortie, ce qui nous permet d’afficher plusieurs enregistrements de source de données par ligne de table.

Que se passe-t-il si vous ne souhaitez t pour émettre un élément HTML `<table>`, bien que ? Pour le contrôle total et complet sur le balisage généré par un contrôle Web de données, nous devons utiliser le contrôle du répéteur. Comme le contrôle DataList, Repeater est construit en fonction des modèles. Toutefois, le contrôle Repeater, offre uniquement cinq modèles suivants :

- `HeaderTemplate` s’il est fourni, ajoute du balisage spécifié avant les éléments
- `ItemTemplate` utilisé pour restituer les éléments
- `AlternatingItemTemplate` s’il est fourni, utilisé pour restituer les éléments de remplacement
- `SeparatorTemplate` s’il est fourni, ajoute du balisage spécifié entre chaque élément.
- `FooterTemplate` -Si fourni, ajoute du balisage spécifié après les éléments

Dans ASP.NET 1.x, le Repeater contrôle était couramment utilisé pour afficher une liste à puces dont les données proviennent d’une source de données. Dans ce cas, le `HeaderTemplate` et `FooterTemplates` contiendrait l’ouverture et la fermeture `<ul>` balises, tandis que respectivement, le `ItemTemplate` contiendrait `<li>` éléments avec la syntaxe de liaison de données. Cette approche peut toujours être utilisée dans ASP.NET 2.0 comme nous l’avons vu dans deux exemples de la [Pages maîtres et Navigation du Site](../introduction/master-pages-and-site-navigation-cs.md) didacticiel :

- Dans le `Site.master` page maître, un répéteur a été utilisé pour afficher une liste à puces du contenu du mappage de site de niveau supérieur (rapports de base, filtrage des rapports, de mise en forme personnalisée et ainsi de suite) ; un autre, imbriquée Repeater a été utilisé pour afficher les sections enfants de la sections de niveau supérieur
- Dans `SectionLevelTutorialListing.ascx`, un répéteur a été utilisé pour afficher une liste à puces des sections enfants de la section de mappage de site en cours

> [!NOTE]
> ASP.NET 2.0 introduit la nouvelle [contrôle BulletedList](https://msdn.microsoft.com/library/ms228101.aspx), qui peut être lié à un contrôle de source de données afin d’afficher une liste à puces simple. Avec le contrôle BulletedList, nous n’avez pas besoin de spécifier le code HTML liés à la liste ; au lieu de cela, nous indiquons simplement le champ de données à afficher en tant que le texte pour chaque élément de liste.


Le Repeater sert un bloc catch de toutes les données de contrôle Web. Si il n’est pas un contrôle existant qui génère le balisage nécessaire, le contrôle Repeater peut être utilisé. Pour illustrer l’utilisation de répéteur, permettent de s disposent de la liste des catégories affiché au-dessus du contrôle DataList informations de produit créé à l’étape 2. En particulier, let s ont les catégories affichées dans une seule ligne HTML `<table>` avec chaque catégorie affiché sous la forme d’une colonne dans la table.

Pour ce faire, nous allons faire glisser un contrôle Repeater à partir de la boîte à outils vers le concepteur, au-dessus du contrôle DataList informations de produit. Comme avec le contrôle DataList, Repeater affiche initialement comme une zone grisée jusqu'à ce que ses modèles ont été définis.


[![Ajouter un répéteur au concepteur](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image34.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image33.png)

**Figure 13**: ajouter un répéteur vers le concepteur ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image35.png))


Cet emplacement s uniquement une des options dans la balise active Repeater s : choisir une Source de données. Choisir de créer un nouveau ObjectDataSource et configurez-le pour utiliser le `CategoriesBLL` classe s `GetCategories` (méthode).


[![Créer un nouveau ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image37.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image36.png)

**Figure 14**: créer un nouveau ObjectDataSource ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image38.png))


[![Configurer pour utiliser la classe CategoriesBLL ObjectDataSource](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image40.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image39.png)

**Figure 15**: configurer ObjectDataSource à utiliser le `CategoriesBLL` classe ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image41.png))


[![Récupérer des informations sur toutes les catégories à l’aide de la méthode GetCategories](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image43.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image42.png)

**Figure 16**: récupérer des informations sur toutes les catégories à l’aide du `GetCategories` (méthode) ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image44.png))


Contrairement à la DataList, Visual Studio ne crée pas automatiquement un modèle ItemTemplate pour le contrôle Repeater après sa liaison à une source de données. En outre, les modèles de s Repeater ne peut pas être configurés via le concepteur et doivent être spécifiés de façon déclarative.

Pour afficher les catégories en tant qu’une seule ligne `<table>` avec une colonne pour chaque catégorie, nous avons besoin de la répétition d’émettre un balisage semblable au suivant :


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample7.html)]

Dans la mesure où la `<td>Category X</td>` texte correspond à la partie qui se répète, cela apparaît dans le répéteur s ItemTemplate. Le balisage qui s’affiche avant - `<table><tr>` -seront placés dans le `HeaderTemplate` lors de la balise de fin - `</tr></table>` -sera placé dans le `FooterTemplate`. Pour entrer ces paramètres de modèle, accédez à la partie déclarative de la page ASP.NET en cliquant sur le bouton de la Source dans l’angle inférieur gauche et tapez la syntaxe suivante :


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample8.aspx)]

Le Repeater émet le balisage précis comme spécifié par ses modèles, rien de plus, rien de moins. Figure 17 montre la sortie de s Repeater lorsqu’ils sont affichés via un navigateur.


[![Une seule ligne de HTML &lt;table&gt; répertorie chaque catégorie dans une colonne distincte](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image46.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image45.png)

**Figure 17**: une seule ligne HTML `<table>` répertorie chaque catégorie dans une colonne distincte ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image47.png))


## <a name="step-6-improving-the-appearance-of-the-repeater"></a>Étape 6 : Améliorer l’apparence du répéteur

Dans la mesure où le contrôle Repeater émet précisément le balisage spécifié par ses modèles, celle-ci doit provenir pas étonnant qu’il n’y a pas de propriétés associées au style pour le contrôle Repeater. Pour modifier l’apparence du contenu généré par le contrôle Repeater, nous devons ajouter manuellement le contenu HTML ou CSS nécessaire directement aux modèles Repeater s.

Dans notre exemple, laisser s ont les colonnes de catégorie alterner les couleurs d’arrière-plan, comme avec les lignes en alternance dans le contrôle DataList. Pour ce faire, vous devez affecter la `RowStyle` classe CSS à chaque élément du Repeater et le `AlternatingRowStyle` classe CSS à chaque élément du Repeater en alternance via le `ItemTemplate` et `AlternatingItemTemplate` modèles, comme suit :


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample9.aspx)]

Permettent de s également ajouter une ligne d’en-tête à la sortie avec le texte de catégories de produits. Dans la mesure où nous n t savoir combien de colonnes notre résultant `<table>` sera constitué de, la façon la plus simple de générer une ligne d’en-tête est garantie pour couvrir toutes les colonnes consiste à utiliser *deux* `<table>` s. La première `<table>` contiendra deux lignes de la ligne d’en-tête et une ligne qui contient la seule ligne deuxième, `<table>` qui a une colonne pour chaque catégorie dans le système. Autrement dit, nous voulons émettre le balisage suivant :


[!code-html[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample10.html)]

Ce qui suit `HeaderTemplate` et `FooterTemplate` entraîner dans le balisage de votre choix :


[!code-aspx[Main](displaying-data-with-the-datalist-and-repeater-controls-cs/samples/sample11.aspx)]

Figure 18 montre le Repeater après ont apporté ces modifications.


[![Les colonnes de la catégorie autre couleur d’arrière-plan et inclut une ligne d’en-tête](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image49.png)](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image48.png)

**Figure 18**: la catégorie colonnes autre dans la couleur d’arrière-plan et inclut une ligne d’en-tête ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-datalist-and-repeater-controls-cs/_static/image50.png))


## <a name="summary"></a>Récapitulatif

Tandis que le contrôle GridView rend plus facile d’afficher, modifier, supprimer, trier et parcourir les données, l’apparence est très bruts et grille. Pour mieux contrôler l’apparence, nous devons nous tourner vers les contrôles le contrôle DataList ou Repeater. Les deux de ces contrôles affichent un ensemble d’enregistrements à l’aide de modèles au lieu de BoundFields, CheckBoxFields et ainsi de suite.

Le contrôle DataList rendu sous la forme d’un élément HTML `<table>` qui, par défaut, affiche chaque enregistrement de source de données dans une seule ligne du tableau, à l’instar d’un GridView avec TemplateField unique. Comme nous le verrons dans un futur didacticiel, toutefois, le contrôle DataList peut modifier plusieurs enregistrements à afficher par ligne de table. Le contrôle Repeater, quant à eux, strictement émet le balisage spécifié dans ses modèles ; Il n’ajoute pas de tout balisage supplémentaire et par conséquent est généralement utilisée pour afficher des données dans les éléments HTML autre qu’un `<table>` (comme dans une liste à puces).

Bien que les contrôles DataList et Repeater offrent davantage de flexibilité dans leur sortie rendue, ils ne présentent pas tous les fonctionnalités intégrées dans le contrôle GridView. Comme nous allons examiner dans les didacticiels à venir, certaines de ces fonctionnalités peuvent être branché précédent sans trop d’efforts, mais faire continuer à l’esprit qu’à utiliser le contrôle DataList ou Repeater à la place le contrôle GridView ne limite pas les fonctionnalités que vous pouvez utiliser sans avoir à implémenter ces fonctionnalités vous-même.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Yaakov Ellis, Liz Shulok, Randy Schmidt et Stacy Park. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
