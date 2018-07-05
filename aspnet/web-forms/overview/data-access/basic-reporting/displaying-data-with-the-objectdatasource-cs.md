---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-cs
title: Affichage des données avec ObjectDataSource (c#) | Microsoft Docs
author: rick-anderson
description: Ce didacticiel examine le contrôle ObjectDataSource à l’aide de ce contrôle, que vous pouvez lier des données récupérées à partir de la couche BLL créée dans le didacticiel précédent sans havi...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: af882aef-56f5-4e9a-8f95-3977fde20e74
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: f10745eee9f6ac04e670d710a4ac999c9ddda50b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814923"
---
<a name="displaying-data-with-the-objectdatasource-c"></a>Affichage des données avec ObjectDataSource (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_4_CS.exe) ou [télécharger le PDF](displaying-data-with-the-objectdatasource-cs/_static/datatutorial04cs1.pdf)

> Ce didacticiel examine le contrôle ObjectDataSource à l’aide de ce contrôle, que vous pouvez lier des données récupérées à partir de la couche BLL créée dans le didacticiel précédent, sans avoir à écrire une ligne de code !


## <a name="introduction"></a>Introduction

Avec notre application architecture et le site Web de mise en page terminée, nous sommes prêts à commencer à découvrir comment effectuer diverses tâches courantes de données et création de rapports liés. Dans les didacticiels précédents, nous avons vu comment lier par programmation des données à partir de la couche DAL et la couche BLL à un contrôle Web dans une page ASP.NET de données. Cette syntaxe d’attribuer les données contrôle Web `DataSource` propriété aux données à afficher avant d’appeler le contrôle `DataBind()` méthode était le modèle utilisé dans les applications ASP.NET 1.x et pouvez continuer à utiliser dans vos 2.0 applications. Toutefois, les nouveaux contrôles de source de données d’ASP.NET 2.0 offrent fonctionnent avec des données de façon déclarative. À l’aide de ces contrôles, vous pouvez lier des données récupérées à partir de la couche BLL créés dans le [didacticiel précédent](../introduction/creating-a-business-logic-layer-cs.md) sans avoir à écrire une ligne de code !

ASP.NET 2.0 est livré avec cinq contrôles de source de données intégrés [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w(vs.80).aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a(en-US,VS.80).aspx), et [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x(en-US,VS.80).aspx) bien que vous pouvez créer votre propre [contrôles de source de données personnalisées](https://msdn.microsoft.com/library/default.asp?url=/library/dnvs05/html/DataSourceCon1.asp), si nécessaire. Étant donné que nous avons développé une architecture pour notre application du didacticiel, nous allons utiliser ObjectDataSource par rapport à nos classes de la couche BLL.


![ASP.NET 2.0 inclut cinq contrôles de Source de données intégrés](displaying-data-with-the-objectdatasource-cs/_static/image1.png)

**Figure 1**: ASP.NET 2.0 inclut cinq contrôles de Source de données intégrés


ObjectDataSource sert de proxy pour travailler avec un autre objet. Pour configurer l’ObjectDataSource nous spécifions cette sous-jacent d’objet et ses méthodes de mappent de l’ObjectDataSource `Select`, `Insert`, `Update`, et `Delete` méthodes. Une fois que cet objet sous-jacent a été spécifié et ses méthodes mappé à l’ObjectDataSource, nous pouvons ensuite lier ObjectDataSource à un contrôle Web de données. ASP.NET est livré avec des données de nombreux contrôles Web, y compris le GridView, DetailsView, RadioButtonList et le DropDownList, entre autres. Pendant le cycle de vie de page, les données de contrôle Web peut-être accéder aux données qu’il est lié, il s’exécutera en appelant son ObjectDataSource `Select` méthode ; si les données de contrôle Web prend en charge l’insertion, la mise à jour, ou de suppression, les appels peuvent être apportées à son ObjectDataSource `Insert`, `Update`, ou `Delete` méthodes. Ces appels sont ensuite dirigés par ObjectDataSource aux méthodes de l’objet sous-jacent approprié, comme l’illustre le diagramme suivant.


[![ObjectDataSource sert de Proxy](displaying-data-with-the-objectdatasource-cs/_static/image3.png)](displaying-data-with-the-objectdatasource-cs/_static/image2.png)

**Figure 2**: The ObjectDataSource sert de Proxy ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image4.png))


Tandis que ObjectDataSource peut être utilisé pour appeler des méthodes d’insertion, la mise à jour ou supprimer des données, nous allons simplement nous concentrer sur retour de données ; didacticiels futures explorerons à l’aide de l’ObjectDataSource et les données des contrôles Web qui modifient les données.

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>Étape 1 : Ajout et configuration du contrôle ObjectDataSource

Commencez par ouvrir le `SimpleDisplay.aspx` page dans le `BasicReporting` dossier, basculez en mode Design, puis faites glisser un contrôle ObjectDataSource à partir de la boîte à outils vers l’aire de conception de la page. ObjectDataSource apparaît sous la forme d’une zone grise sur l’aire de conception, car il ne produit pas de tout balisage ; il accède simplement à des données en appelant une méthode à partir d’un objet spécifié. Les données retournées par un ObjectDataSource peuvent être affichées par un contrôle Web, comme le GridView, DetailsView, FormView et ainsi de suite de données.

> [!NOTE]
> Ou bien, vous devrez peut-être ajouter les données de contrôle Web à la page et, à partir de sa balise active, puis le &lt;nouvelle source de données&gt; option dans la liste déroulante.


Pour spécifier l’objet sous-jacent de l’ObjectDataSource et comment mappent les méthodes de l’objet de l’ObjectDataSource, cliquez sur le lien configurer la Source de données à partir de la balise active de l’ObjectDataSource.


[![Cliquez sur le lien de Source de données à partir de la balise active de configuration](displaying-data-with-the-objectdatasource-cs/_static/image6.png)](displaying-data-with-the-objectdatasource-cs/_static/image5.png)

**Figure 3**: cliquez sur le lien de Source de données de configuration à partir de la balise active ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image7.png))


Ceci fait apparaître l’Assistant Configurer la Source de données. Tout d’abord, nous devons spécifier l’objet avec qu'objectdatasource consiste à travailler. Si la case à cocher « Afficher uniquement les composants de données » est cochée, la liste déroulante sur cet écran répertorie uniquement les objets qui ont été décorées avec le `DataObject` attribut. Notre liste inclut actuellement les TableAdapters dans le DataSet typé et les classes de couche de logique métier que nous avons créé dans le didacticiel précédent. Si vous avez oublié d’ajouter le `DataObject` attribut aux classes de couche de logique métier vous ne les voyez dans cette liste. Dans ce cas, désactivez la case à cocher « Afficher uniquement les composants de données » pour afficher tous les objets, qui doivent inclure les classes de la couche BLL (ainsi que les autres classes dans le DataSet typée DataTables, DataRows et ainsi de suite).

Dans le premier écran, choisissez la `ProductsBLL` classe dans la liste déroulante et cliquez sur Suivant.


[![Spécifier l’objet à utiliser avec le contrôle ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image9.png)](displaying-data-with-the-objectdatasource-cs/_static/image8.png)

**Figure 4**: spécifiez l’objet à utiliser avec le contrôle ObjectDataSource ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image10.png))


L’écran suivant de l’Assistant vous invite à sélectionner quelle méthode ObjectDataSource doit appeler. La liste déroulante répertorie les méthodes qui retournent des données dans l’objet sélectionné à partir de l’écran précédent. Nous voyons ici `GetProductByProductID`, `GetProducts`, `GetProductsByCategoryID`, et `GetProductsBySupplierID`. Sélectionnez le `GetProducts` méthode dans la liste déroulante, cliquez sur Terminer (si vous avez ajouté le `DataObjectMethodAttribute` à la `ProductBLL`de méthodes comme indiqué dans le didacticiel précédent, cette option seront sélectionnées par défaut).


[![Choisissez la méthode pour retourner des données à partir de l’onglet Sélection](displaying-data-with-the-objectdatasource-cs/_static/image12.png)](displaying-data-with-the-objectdatasource-cs/_static/image11.png)

**Figure 5**: choisissez la méthode pour retourner des données à partir de l’onglet à sélectionner ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image13.png))


## <a name="configure-the-objectdatasource-manually"></a>Configurer manuellement les ObjectDataSource

Assistant Configurer la Source de données de l’ObjectDataSource offre un moyen rapide pour spécifier l’objet qu’il utilise et associer les méthodes de l’objet sont appelés. Vous pouvez toutefois configurer ObjectDataSource via ses propriétés, soit via la fenêtre Propriétés, soit directement dans le balisage déclaratif. Il suffit de définir la `TypeName` propriété au type de l’objet sous-jacent à utiliser et le `SelectMethod` à la méthode à appeler lors de la récupération des données.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample1.aspx)]

Même si vous préférez l’Assistant Configurer la Source de données il peut arriver que vous deviez configurer manuellement l’ObjectDataSource, comme l’Assistant répertorie uniquement les classes créées par le développeur. Si vous souhaitez lier l’ObjectDataSource à une classe dans le .NET Framework, tels que le [classe d’appartenance](https://msdn.microsoft.com/library/system.web.security.membership.aspx), pour accéder aux informations de compte d’utilisateur, ou le [classe Directory](https://msdn.microsoft.com/library/system.io.directory.aspx) pour travailler avec les informations de système de fichiers Vous devrez définir manuellement les propriétés de l’ObjectDataSource.

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>Étape 2 : Ajout d’un contrôle Web de données et en la liant à ObjectDataSource

Une fois que ObjectDataSource a été ajouté à la page et configuré, nous sommes prêts à ajouter des contrôles Web de données à la page pour afficher les données retournées par l’ObjectDataSource `Select` (méthode). N’importe quel contrôle Web de données peuvent être liées à ObjectDataSource ; Penchons-nous sur l’affichage des données de l’ObjectDataSource dans un GridView, DetailsView et FormView.

## <a name="binding-a-gridview-to-the-objectdatasource"></a>Liaison d’un GridView à ObjectDataSource

Ajouter un contrôle GridView à partir de la boîte à outils à `SimpleDisplay.aspx`d’aire de conception. À partir de la balise active le contrôle GridView, choisissez le contrôle ObjectDataSource que nous avons ajouté à l’étape 1. Cela créera automatiquement un BoundField dans le contrôle GridView pour chaque propriété retournée par les données à partir de l’ObjectDataSource `Select` (méthode) (à savoir, les propriétés définies par le DataTable de produits).


[![Un GridView a été ajouté à la Page et lié à ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image15.png)](displaying-data-with-the-objectdatasource-cs/_static/image14.png)

**Figure 6**: un GridView a été ajouté à la Page et lié à ObjectDataSource ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image16.png))


Vous pouvez ensuite personnaliser, réorganiser ou supprimer le contrôle GridView BoundFields en cliquant sur l’option Modifier les colonnes à partir de la balise active.


[![Gérer BoundFields le contrôle GridView via la boîte de dialogue Modifier les colonnes](displaying-data-with-the-objectdatasource-cs/_static/image18.png)](displaying-data-with-the-objectdatasource-cs/_static/image17.png)

**Figure 7**: gérer BoundFields via le modifier boîte le contrôle GridView de dialogue colonnes ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image19.png))


Prenez un moment pour modifier le contrôle GridView BoundFields, suppression de la `ProductID`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitsInStock`, `UnitsOnOrder`, et `ReorderLevel` BoundFields. Sélectionnez simplement le BoundField dans la liste en bas à gauche et cliquez sur le bouton Supprimer (X rouge) pour les supprimer. Ensuite, réorganiser les BoundFields afin que le `CategoryName` et `SupplierName` BoundFields précéder le `UnitPrice` BoundField en sélectionnant ces BoundFields et en cliquant sur la flèche vers le haut. Définir le `HeaderText` propriétés de la BoundFields restants à `Products`, `Category`, `Supplier`, et `Price`, respectivement. Ensuite, demandez à le `Price` BoundField mis en forme comme une devise en définissant le BoundField `HtmlEncode` False à la propriété et sa `DataFormatString` propriété `{0:c}`. Enfin, aligner horizontalement la `Price` vers la droite et la `Discontinued` case à cocher dans le centre par le `ItemStyle` / `HorizontalAlign` propriété.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample2.aspx)]


[![Le contrôle GridView BoundFields ont été personnalisées.](displaying-data-with-the-objectdatasource-cs/_static/image21.png)](displaying-data-with-the-objectdatasource-cs/_static/image20.png)

**Figure 8**: BoundFields ont été personnalisées le contrôle GridView ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image22.png))


## <a name="using-themes-for-a-consistent-look"></a>Utilisation de thèmes pour une apparence cohérente

Ces didacticiels vous efforcer de supprimer des paramètres de style de niveau de contrôle, au lieu d’utiliser les feuilles de style en cascade définies dans un fichier externe chaque fois que possible. Le `Styles.css` fichier contient `DataWebControlStyle`, `HeaderStyle`, `RowStyle`, et `AlternatingRowStyle` classes CSS qui doivent être utilisés pour dicter l’apparence des données Web contrôles utilisés dans ces didacticiels. Pour ce faire, nous avons défini le GridView `CssClass` propriété `DataWebControlStyle`et son `HeaderStyle`, `RowStyle`, et `AlternatingRowStyle` propriétés `CssClass` propriétés en conséquence.

Si vous définissez ces `CssClass` propriétés au contrôle Web, nous devons penser à définir explicitement ces valeurs de propriété pour les données de chaque Web contrôle ajouté à nos didacticiels. Une approche plus gérable consiste à définir les propriétés CSS relatives à par défaut pour le contrôle GridView, DetailsView, FormView contrôles et à l’aide d’un thème. Un thème est une collection de paramètres de propriété de niveau de contrôle, des images et des classes CSS qui peuvent être appliqués aux pages sur un site pour appliquer une apparence commune.

Notre thème n’inclut pas des images ou des fichiers CSS (nous allons laisser la feuille de style `Styles.css` en tant que-, défini dans le dossier racine de l’application web), mais inclut deux apparences. Une apparence est un fichier qui définit les propriétés par défaut pour un contrôle Web. Plus précisément, nous allons avoir un fichier d’apparence pour les contrôles GridView et DetailsView, indiquant la valeur par défaut `CssClass`-propriétés associées.

Commencez par ajouter un nouveau fichier d’apparence à votre projet nommé `GridView.skin` en cliquant sur le nom du projet dans l’Explorateur de solutions et en choisissant Ajouter un nouvel élément.


[![Ajouter un fichier d’apparence nommé GridView.skin](displaying-data-with-the-objectdatasource-cs/_static/image24.png)](displaying-data-with-the-objectdatasource-cs/_static/image23.png)

**Figure 9**: ajouter un fichier d’apparence nommé `GridView.skin` ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image25.png))


Fichiers d’apparence doivent être placées dans un thème, qui se trouvent dans le `App_Themes` dossier. Étant donné que nous n’avez pas encore un tel dossier, Visual Studio bien vouloir propose de créer un pour nous lors de l’ajout de l’apparence de notre premier. Cliquez sur Oui pour créer le `App_Theme` dossier et placer le nouveau `GridView.skin` fichier il.


[![Laissez Visual Studio de créer le dossier App_Theme](displaying-data-with-the-objectdatasource-cs/_static/image27.png)](displaying-data-with-the-objectdatasource-cs/_static/image26.png)

**Figure 10**: laisser Visual Studio de créer le `App_Theme` dossier ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image28.png))


Cela créera un nouveau thème dans le `App_Themes` dossier nommé GridView avec le fichier d’apparence `GridView.skin`.


![Le thème de GridView a été ajouté au dossier App_Theme](displaying-data-with-the-objectdatasource-cs/_static/image29.png)

**Figure 11**: le thème de GridView a été ajouté à la `App_Theme` dossier


Renommez le thème de GridView DataWebControls (avec le bouton droit sur le dossier GridView dans le `App_Theme` dossier et choisissez Renommer). Ensuite, entrez le balisage suivant dans le `GridView.skin` fichier :


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample3.aspx)]

Définit les propriétés par défaut pour le `CssClass`-propriétés associées de n’importe quel GridView dans n’importe quelle page qui utilise le DataWebControls thème. Nous allons ajouter une autre apparence pour le contrôle DetailsView, un contrôle Web que nous allons utiliser peu de données. Ajouter une nouvelle apparence au thème DataWebControls nommé `DetailsView.skin` et ajoutez le balisage suivant :


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample4.aspx)]

Avec notre thème défini, la dernière étape consiste à appliquer le thème à notre page ASP.NET. Un thème peut être appliqué sur une base de page par page ou pour toutes les pages dans un site Web. Nous allons utiliser ce thème pour toutes les pages dans le site Web. Pour ce faire, ajoutez le balisage suivant à `Web.config`de `<system.web>` section :


[!code-xml[Main](displaying-data-with-the-objectdatasource-cs/samples/sample5.xml)]

C’est tout cela ! Le `styleSheetTheme` paramètre indique que les propriétés spécifiées dans le thème doivent *pas* remplacer les propriétés spécifiées au niveau du contrôle. Pour spécifier que les paramètres de thème doivent l’emporte pas sur les paramètres de contrôle, utilisez la `theme` d’attribut à la place de `styleSheetTheme`; Malheureusement, les paramètres de thème spécifiés via le `theme` attribut n’apparaissent pas dans la vue de conception de Visual Studio. Reportez-vous à [thèmes ASP.NET et présentation des Skins](https://msdn.microsoft.com/library/ykzx33wh.aspx) et [côté serveur Styles à l’aide de thèmes](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx) pour plus d’informations sur les thèmes et apparences ; consultez [Comment : appliquer des thèmes ASP.NET](https://msdn.microsoft.com/library/0yy5hxdk(VS.80).aspx) pour en savoir plus sur configuration d’une page pour utiliser un thème.


[![Le contrôle GridView affiche le nom du produit, catégorie, fournisseur, prix et informations supprimées](displaying-data-with-the-objectdatasource-cs/_static/image31.png)](displaying-data-with-the-objectdatasource-cs/_static/image30.png)

**Figure 12**: le contrôle GridView affiche le nom du produit, catégorie, fournisseur, prix et informations supprimées ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image32.png))


## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>Affichage d’un enregistrement à la fois dans le contrôle DetailsView

Le contrôle GridView affiche une ligne pour chaque enregistrement retourné par le contrôle de source de données à laquelle elle est liée. Il est parfois, cependant, lorsque nous souhaiterons peut-être afficher un enregistrement unique ou qu’un seul enregistrement à la fois. Le [contrôle DetailsView](https://msdn.microsoft.com/library/s3w1w7t4.aspx) offre cette fonctionnalité, rendu sous la forme d’un élément HTML `<table>` avec deux colonnes et une ligne pour chaque colonne ou propriété liée au contrôle. Vous pouvez considérer le contrôle DetailsView comme un GridView avec un seul enregistrement pivotée de 90 degrés.

Commencez par ajouter un contrôle DetailsView *ci-dessus* le contrôle GridView dans `SimpleDisplay.aspx`. Ensuite, le lier au contrôle ObjectDataSource même en tant que le contrôle GridView. Comme avec le contrôle GridView, un BoundField est ajouté au contrôle DetailsView pour chaque propriété de l’objet retourné par l’ObjectDataSource `Select` (méthode). La seule différence est que BoundFields de DetailsView sont disposées horizontalement plutôt que verticalement.


[![Ajouter un contrôle DetailsView à la Page et la lier à ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image34.png)](displaying-data-with-the-objectdatasource-cs/_static/image33.png)

**Figure 13**: ajouter un contrôle DetailsView à la Page et la lier à ObjectDataSource ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image35.png))


Comme le contrôle GridView, BoundFields de DetailsView peut être ajustée pour fournir un affichage plus personnalisé des données retournées par l’ObjectDataSource. La figure 14 illustre le contrôle DetailsView après son BoundFields et `CssClass` propriétés ont été configurées pour rendre son apparence similaire à l’exemple de GridView.


[![Le contrôle DetailsView montre un seul enregistrement](displaying-data-with-the-objectdatasource-cs/_static/image37.png)](displaying-data-with-the-objectdatasource-cs/_static/image36.png)

**Figure 14**: le contrôle DetailsView affiche un enregistrement unique ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image38.png))


Notez que le contrôle DetailsView affiche uniquement le premier enregistrement retourné par sa source de données. Pour autoriser l’utilisateur à parcourir tous les enregistrements, un à la fois, nous devons activer la pagination pour le contrôle DetailsView. Pour ce faire, revenez à Visual Studio, puis cochez la case Activer la pagination dans la balise active de DetailsView.


[![Activer la pagination dans le contrôle DetailsView](displaying-data-with-the-objectdatasource-cs/_static/image40.png)](displaying-data-with-the-objectdatasource-cs/_static/image39.png)

**Figure 15**: activer la pagination dans le contrôle DetailsView ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image41.png))


[![Avec la pagination est activée, le contrôle DetailsView permet à l’utilisateur afficher tous les produits](displaying-data-with-the-objectdatasource-cs/_static/image43.png)](displaying-data-with-the-objectdatasource-cs/_static/image42.png)

**Figure 16**: avec la pagination activé, le contrôle DetailsView permet à l’utilisateur afficher tous les produits ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image44.png))


Nous nous pencherons sur la pagination dans les futures didacticiels.

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>Une présentation plus souple pour afficher un enregistrement à la fois

Le contrôle DetailsView est assez rigid dans comment il s’affiche chaque enregistrement retourné à partir de l’ObjectDataSource. Nous pouvons choisir une vue plus flexible des données. Par exemple, au lieu d’afficher le nom du produit, catégorie, fournisseur, prix et chaque supprimées d’informations sur une ligne distincte, nous souhaiterons peut-être afficher le nom du produit et de prix dans une `<h4>` titre, avec les informations de catégorie et le fournisseur apparaissant sous le nom et le prix de la taille de police. Et nous ne pouvons pas en charge afficher les noms de propriété (produit, catégorie et ainsi de suite) en regard des valeurs.

Le [contrôle FormView](https://msdn.microsoft.com/library/fyf1dk77.aspx) fournit ce niveau de personnalisation. Au lieu d’utiliser les champs (tels que GridView et DetailsView font), le contrôle FormView utilise des modèles, qui offrent une combinaison de contrôles Web, HTML statique, et [syntaxe de liaison de données](http://www.15seconds.com/issue/040630.htm). Si vous êtes familiarisé avec le contrôle Repeater à partir d’ASP.NET 1.x, vous pouvez considérer le FormView comme le Repeater pour afficher un seul enregistrement.

Ajouter un contrôle FormView pour le `SimpleDisplay.aspx` aire de conception de la page. Le contrôle FormView affiche initialement comme un bloc gris pour nous informer que nous devons fournir, au minimum, le contrôle `ItemTemplate`.


[![FormView doivent inclure un modèle ItemTemplate](displaying-data-with-the-objectdatasource-cs/_static/image46.png)](displaying-data-with-the-objectdatasource-cs/_static/image45.png)

**Figure 17**: The FormView doit inclure un `ItemTemplate` ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image47.png))


Vous pouvez lier le contrôle FormView directement à un contrôle de source de données par le biais SmartTag de FormView, ce qui crée une valeur par défaut `ItemTemplate` automatiquement (avec un `EditItemTemplate` et `InsertItemTemplate`, si le contrôle ObjectDatatSource `InsertMethod` et `UpdateMethod` propriétés sont définies). Toutefois, pour cet exemple nous allons lier les données pour le contrôle FormView et spécifier ses `ItemTemplate` manuellement. Commencez par définir le FormView `DataSourceID` propriété le `ID` du contrôle ObjectDataSource, `ObjectDataSource1`. Ensuite, créez le `ItemTemplate` afin qu’il affiche le nom du produit et des prix dans un `<h4>` élément et les noms de catégorie et expéditeur sous que dans une plus petite taille de police.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample6.aspx)]


[![Le premier produit (Chai) s’affiche dans un Format personnalisé](displaying-data-with-the-objectdatasource-cs/_static/image49.png)](displaying-data-with-the-objectdatasource-cs/_static/image48.png)

**Figure 18**: le premier produit (Chai) s’affiche dans un Format personnalisé ([cliquez pour afficher l’image en taille réelle](displaying-data-with-the-objectdatasource-cs/_static/image50.png))


Le `<%# Eval(propertyName) %>` est la syntaxe de liaison de données. Le `Eval` méthode retourne la valeur de la propriété spécifiée pour l’objet actuel qui est liée au contrôle FormView. Consultez l’article d’Alex Homer [simplifié et étendus syntaxe liaison de données dans ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm) pour plus d’informations sur les tenants et aboutissants de la liaison de données.

Comme le contrôle DetailsView, FormView affiche uniquement le premier enregistrement retourné à partir de l’ObjectDataSource. Vous pouvez activer la pagination dans le contrôle FormView pour autoriser les visiteurs à parcourir les produits d’un à la fois.

## <a name="summary"></a>Récapitulatif

Accès et affichage des données à partir d’une couche de logique métier peuvent être accomplis sans écrire une ligne de code contrôle ObjectDataSource d’ASP.NET 2.0. ObjectDataSource appelle une méthode spécifiée d’une classe et retourne les résultats. Ces résultats peuvent être affichés dans un contrôle Web qui est lié à ObjectDataSource de données. Dans ce didacticiel, nous avons vu la liaison des contrôles GridView, DetailsView et FormView à ObjectDataSource.

Jusqu'à présent, nous avons vu seulement l’utilisation de l’ObjectDataSource pour appeler une méthode sans paramètre, mais que se passe-t-il si nous voulons appeler une méthode qui attend des paramètres d’entrée, tels que le `ProductBLL` la classe `GetProductsByCategoryID(categoryID)`? Pour pouvoir appeler une méthode qui attend un ou plusieurs paramètres, nous devons configurer ObjectDataSource pour spécifier les valeurs de ces paramètres. Nous verrons comment procéder dans notre [didacticiel suivant](declarative-parameters-cs.md).

Bonne programmation !

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Créer vos propres contrôles de Source de données](https://msdn.microsoft.com/library/ms364049.aspx)
- [Exemples de GridView d’ASP.NET 2.0](https://msdn.microsoft.com/library/aa479339.aspx)
- [Simplifié et étendu la liaison de données syntaxe dans ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm)
- [Thèmes dans ASP.NET 2.0](http://www.odetocode.com/Articles/423.aspx)
- [Styles de côté serveur à l’aide de thèmes](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [Comment : Appliquer des thèmes ASP.NET par programmation](https://msdn.microsoft.com/library/tx35bd89.aspx)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Hilton Giesenow. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](declarative-parameters-cs.md)
