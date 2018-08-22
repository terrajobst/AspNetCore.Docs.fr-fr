---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-cs
title: Interaction avec la Page maître depuis la Page de contenu (c#) | Microsoft Docs
author: rick-anderson
description: Examine la manière d’appeler des méthodes, définissez des propriétés, etc. de la Page maître à partir du code dans la Page de contenu.
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 32d54638-71b2-491d-81f4-f7417a13a62f
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-cs
msc.type: authoredcontent
ms.openlocfilehash: afcec6cb7a6763d068301c7afdd28ff560a3065b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828090"
---
<a name="interacting-with-the-master-page-from-the-content-page-c"></a>Interaction avec la Page maître depuis la Page de contenu (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_06_CS.zip) ou [télécharger le PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_06_CS.pdf)

> Examine la manière d’appeler des méthodes, définissez des propriétés, etc. de la Page maître à partir du code dans la Page de contenu.


## <a name="introduction"></a>Introduction

Au cours des cinq derniers didacticiels, nous avons examiné comment créer une page maître, définir des zones de contenu, lier des pages ASP.NET à une page maître et définissent le contenu spécifique à la page. Lorsqu’un visiteur demande une page de contenu particulier, le contenu et le balisage des pages maîtres sont fusionnés lors de l’exécution, ce qui entraîne le rendu d’une hiérarchie de contrôle unifié. Par conséquent, nous avons déjà vu une manière qui la page maître et celle de ses pages de contenu peuvent interagir : la page de contenu énonce le balisage à transfuse dans les contrôles ContentPlaceHolder de la page maître.

Ce que nous avons encore à examiner est comment la page maître et la page de contenu peuvent interagir par programmation. En plus de définir le balisage pour les contrôles ContentPlaceHolder de la page maître, une page de contenu peut également affecter des valeurs aux propriétés publiques de sa page maître et appeler ses méthodes publiques. De même, une page maître peut interagir avec ses pages de contenu. Alors que les interactions par programme entre une page maître et le contenu est moins fréquentes que l’interaction entre les marques de leurs déclaratifs, il existe de nombreux scénarios où ces interactions par programme sont nécessaire.

Dans ce didacticiel, nous allons examiner comment une page de contenu peut interagir par programmation avec sa page maître ; dans le didacticiel suivant, nous allons examiner comment la page maître peut même interagir avec ses pages de contenu.

## <a name="examples-of-programmatic-interaction-between-a-content-page-and-its-master-page"></a>Exemples de l’Interaction entre une Page de contenu et sa Page maître par programmation

Lorsqu’une région particulière d’une page doit être configurée sur une base de page par page, nous utilisons un contrôle ContentPlaceHolder. Mais qu’en est-il des situations où la majorité des pages doivent émettre une sortie certains, mais un petit nombre de pages pour le personnaliser pour afficher autre chose ? Un exemple, que nous avons examiné dans le [ *ContentPlaceHolders multiples et par défaut contenu* ](multiple-contentplaceholders-and-default-content-cs.md) didacticiel, implique l’affichage d’une interface de connexion sur chaque page. Bien que la plupart des pages doivent inclure une interface de connexion, elle doit être supprimée pour un certain nombre de pages, tels que : la page de connexion principale (`Login.aspx`) ; la page Créer un compte ; et les autres pages qui sont uniquement accessibles aux utilisateurs authentifiés. Le [ *ContentPlaceHolders multiples et contenu par défaut* ](multiple-contentplaceholders-and-default-content-cs.md) didacticiel vous a montré comment définir le contenu par défaut pour un ContentPlaceHolder dans la page maître et puis comment substituer dans les pages où le contenu par défaut n’était pas voulu.

Une autre option consiste à créer une propriété publique ou une méthode dans la page maître qui indique s’il faut afficher ou masquer l’interface de connexion. Par exemple, la page maître peut inclure une propriété publique nommée `ShowLoginUI` dont la valeur a été utilisée pour définir le `Visible` propriété du contrôle de connexion dans la page maître. Ces pages de contenu où l’interface utilisateur de connexion doit être supprimée peut ensuite par programmation définir le `ShowLoginUI` propriété `false`.

Par exemple, l’exemple le plus courant de contenu et l’interaction de la page maître se produit lorsque les données affichées dans la page maître doit être actualisé après qu’une action a été dépassé dans la page de contenu. Imaginez une page maître qui inclut un GridView qui affiche les cinq plus récemment ajouté des enregistrements à partir d’une table de base de données particulière et que l’une des ses pages de contenu inclut une interface pour l’ajout de nouveaux enregistrements à cette table.

Lorsqu’un utilisateur visite la page pour ajouter un nouvel enregistrement, il voit que les cinq récemment ajoutées enregistrements affichés dans la page maître. Après avoir renseigné les valeurs pour les colonnes du nouvel enregistrement, elle envoie le formulaire. En supposant que le contrôle GridView dans la page maître a son `EnableViewState` propriété définie sur true (valeur par défaut), son contenu est rechargé à partir de l’état d’affichage et, par conséquent, les cinq mêmes enregistrements sont affichent, même si un enregistrement plus récent vient d’être ajouté à la base de données. Cela risque de perturber l’utilisateur.

> [!NOTE]
> Même si vous désactivez l’état d’affichage du contrôle GridView afin qu’il relie à sa source de données sous-jacente à chaque publication, il toujours n’indiquent pas l’enregistrement ajouté juste car les données sont liées au GridView plus tôt dans le cycle de vie de page que lorsque le nouvel enregistrement est ajouté à la datab ASE.


Pour résoudre ce problème afin que l’enregistrement juste-ajouté s’affiche dans la page maître du GridView sur la publication (postback), nous devons indiquer le contrôle GridView à relier à sa source de données *après* le nouvel enregistrement a été ajouté à la base de données. Cela requiert une interaction entre le contenu et les pages maîtres, car l’interface pour ajouter que le nouvel enregistrement (et ses gestionnaires d’événements) se trouvent dans la page de contenu, mais le contrôle GridView qui doit être actualisé est dans la page maître.

Étant donné que l’actualisation d’affichage de la page maître à partir d’un gestionnaire d’événements dans la page de contenu fait partie des besoins plus courants pour le contenu et l’interaction de la page maître, nous allons Explorer ce sujet plus en détail. Le téléchargement de ce didacticiel inclut une base de données Microsoft SQL Server 2005 Express Edition nommé `NORTHWIND.MDF` dans du site Web `App_Data` dossier. La base de données Northwind stocke produit, employé et informations sur les ventes d’une société fictive, Northwind Traders.

Étape 1 présente cinq affichage plus récemment ajouté des produits dans un GridView dans la page maître. Étape 2 crée une page de contenu pour l’ajout de nouveaux produits. Étape 3 examine comment créer des propriétés et méthodes publiques dans la page maître et étape 4 illustre comment interagir par programmation avec ces propriétés et méthodes à partir de la page de contenu.

> [!NOTE]
> Ce didacticiel n’aborde pas des spécificités de l’utilisation des données dans ASP.NET. Les étapes de configuration de la page maître afficher des données et la page de contenu pour l’insertion de données sont terminées, encore breezy. Pour obtenir une présentation plus approfondie à l’affichage et insertion de données et l’utilisation des contrôles SqlDataSource et GridView, consultez les ressources dans la section relevés supplémentaire à la fin de ce didacticiel.


## <a name="step-1-displaying-the-five-most-recently-added-products-in-the-master-page"></a>Étape 1 : Afficher les cinq plus récemment ajouté des produits dans la Page maître

Ouvrez le `Site.master` page maître et ajoutez une étiquette et un contrôle GridView à la `leftContent` `<div>`. Désactivez out l’étiquette `Text` propriété, la valeur son `EnableViewState` propriété sur false et son `ID` propriété `GridMessage`; définir le GridView `ID` propriété `RecentProducts`. Ensuite, à partir du concepteur, développez la balise active le contrôle GridView et choisissez de le lier à une source de données. Cette opération lance l’Assistant Configuration de Source de données. Étant donné que la base de données Northwind dans la `App_Data` dossier est une base de données Microsoft SQL Server, choisissez de créer un SqlDataSource en sélectionnant (voir Figure 1) ; nommez SqlDataSource `RecentProductsDataSource`.


[![Lier le contrôle GridView à un contrôle SqlDataSource nommé RecentProductsDataSource](interacting-with-the-master-page-from-the-content-page-cs/_static/image2.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image1.png)

**Figure 01**: lier le contrôle GridView à un contrôle SqlDataSource nommé `RecentProductsDataSource` ([cliquez pour afficher l’image en taille réelle](interacting-with-the-master-page-from-the-content-page-cs/_static/image3.png))


L’étape suivante nous demande de spécifier ce que la base de données pour se connecter à. Choisissez le `NORTHWIND.MDF` de base de données de fichier à partir de la liste déroulante et cliquez sur Suivant. Comme il s’agit de la première fois que nous avons utilisé cette base de données, l’Assistant propose stocker la chaîne de connexion dans `Web.config`. Il a à stocker la chaîne de connexion en utilisant le nom `NorthwindConnectionString`.


[![Se connecter à la base de données Northwind](interacting-with-the-master-page-from-the-content-page-cs/_static/image5.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image4.png)

**Figure 02**: se connecter à la base de données Northwind ([cliquez pour afficher l’image en taille réelle](interacting-with-the-master-page-from-the-content-page-cs/_static/image6.png))


L’Assistant Configurer la Source de données fournit deux moyens par lequel nous pouvons spécifier la requête utilisée pour récupérer des données :

- En spécifiant une instruction SQL personnalisée ou une procédure stockée, ou
- En choisissant une table ou vue, puis en spécifiant les colonnes à retourner

Étant donné que nous voulons renvoyer que simplement cinq récemment ajoutées des produits, nous devons spécifier une instruction SQL personnalisée. Utilisez la requête SELECT suivante :


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample1.sql)]

Le `TOP 5` mot clé retourne uniquement les cinq premiers enregistrements de la requête. Le `Products` clé primaire de la table, `ProductID`, est un `IDENTITY` colonne, ce qui nous permet de garantir que chaque produit de nouveau ajouté à la table a une valeur supérieure à l’entrée précédente. Par conséquent, trier les résultats par `ProductID` dans l’ordre décroissant renvoie les produits compter le plus récemment créées.


[![Retourner les cinq produits les plus récentes](interacting-with-the-master-page-from-the-content-page-cs/_static/image8.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image7.png)

**Figure 03**: retourner les cinq plus récemment ajouté produits ([cliquez pour afficher l’image en taille réelle](interacting-with-the-master-page-from-the-content-page-cs/_static/image9.png))


À l’issue de l’Assistant, Visual Studio génère deux BoundFields pour le contrôle GridView afficher le `ProductName` et `UnitPrice` champs renvoyés à partir de la base de données. À ce stade le balisage déclaratif de votre page maître doit inclure balisage similaire à ce qui suit :


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample2.aspx)]

Comme vous pouvez le voir, le balisage contient : le contrôle Web Label (`GridMessage`) ; le contrôle GridView `RecentProducts`, avec deux BoundFields ; et un contrôle SqlDataSource qui renvoie les cinq plus récemment ajouté des produits.

Avec ce GridView créé et son contrôle SqlDataSource configuré, visitez le site Web via un navigateur. Comme le montre la Figure 4, vous verrez une grille dans l’angle inférieur gauche qui répertorie les cinq plus récemment ajouté des produits.


[![Le contrôle GridView affiche cinq produits les plus récentes](interacting-with-the-master-page-from-the-content-page-cs/_static/image11.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image10.png)

**Figure 04**: le contrôle GridView affiche les cinq plus récemment ajouté produits ([cliquez pour afficher l’image en taille réelle](interacting-with-the-master-page-from-the-content-page-cs/_static/image12.png))


> [!NOTE]
> N’hésitez pas à nettoyer l’apparence du contrôle GridView. Quelques suggestions incluent la mise en forme affichés `UnitPrice` valeur comme une devise et à l’aide de couleurs d’arrière-plan et polices pour améliorer l’apparence de la grille.


## <a name="step-2-creating-a-content-page-to-add-new-products"></a>Étape 2 : Création d’une Page de contenu pour ajouter de nouveaux produits

La tâche suivante consiste à créer une page de contenu à partir de laquelle un utilisateur peut ajouter un nouveau produit à la `Products` table. Ajoutez une nouvelle page de contenu pour le `Admin` dossier nommé `AddProduct.aspx`, veillez à lier à la `Site.master` page maître. La figure 5 illustre l’Explorateur de solutions, une fois que cette page a été ajoutée au site Web.


[![Ajouter une nouvelle Page ASP.NET dans le dossier Admin](interacting-with-the-master-page-from-the-content-page-cs/_static/image14.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image13.png)

**Figure 05**: ajouter une nouvelle Page ASP.NET pour le `Admin` dossier ([cliquez pour afficher l’image en taille réelle](interacting-with-the-master-page-from-the-content-page-cs/_static/image15.png))


N’oubliez pas que dans le [ *spécifiant le titre, les balises Meta et les autres en-têtes HTML dans la Page maître* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) didacticiel, nous avons créé une classe de page de base personnalisée nommée `BasePage` qui a généré le titre de la page s’il s’agissait pas explicitement défini. Accédez à la `AddProduct.aspx` code-behind de la page de classe et de la dériver de `BasePage` (au lieu d’à partir de `System.Web.UI.Page`).

Enfin, mettez à jour le `Web.sitemap` fichier à inclure une entrée pour cette leçon. Ajoutez le balisage suivant sous le `<siteMapNode>` pour la leçon les problèmes de nommage ID de contrôle :


[!code-xml[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample3.xml)]

Comme indiqué dans la Figure 6, l’ajout de ce `<siteMapNode>` élément apparaît dans la liste des leçons.

Retour à `AddProduct.aspx`. Dans le contrôle de contenu pour le `MainContent` ContentPlaceHolder, ajoutez un contrôle DetailsView et nommez-le `NewProduct`. Lier le contrôle DetailsView à un nouveau contrôle SqlDataSource nommé `NewProductDataSource`. Comme avec SqlDataSource à l’étape 1, configurer l’Assistant afin qu’il utilise la base de données Northwind et choisir de spécifier une instruction SQL personnalisée. Étant donné que le contrôle DetailsView doit être utilisé pour ajouter des éléments à la base de données, nous devons spécifier à la fois un `SELECT` instruction et un `INSERT` instruction. Utilisez la commande suivante `SELECT` requête :


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample4.sql)]

Ensuite, ajoutez le code suivant à partir de l’onglet Insérer, `INSERT` instruction :


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample5.sql)]

Après la fin de l’Assistant accédez à la balise active de DetailsView et cochez la case « Activer l’insertion ». Cette opération ajoute une CommandField pour le contrôle DetailsView avec son `ShowInsertButton` propriété définie sur true. Étant donné que cette DetailsView sera utilisé uniquement pour l’insertion de données, définissez le DetailsView `DefaultMode` propriété `Insert`.

C’est tout cela ! Nous allons tester cette page. Visitez `AddProduct.aspx` via un navigateur, entrez un nom et le prix (voir Figure 6).


[![Ajouter un nouveau produit à la base de données](interacting-with-the-master-page-from-the-content-page-cs/_static/image17.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image16.png)

**Figure 06**: ajouter un nouveau produit à la base de données ([cliquez pour afficher l’image en taille réelle](interacting-with-the-master-page-from-the-content-page-cs/_static/image18.png))


Après avoir tapé le nom et le prix de votre nouveau produit, cliquez sur le bouton d’insertion. Ainsi, le formulaire à la publication (postback). Lors de la de publication, le contrôle SqlDataSource `INSERT` instruction est exécutée ; ses deux paramètres sont remplis avec les valeurs entrées par l’utilisateur dans deux contrôles de zone de texte de DetailsView. Malheureusement, il n’existe aucun retour visuel une insertion a eu lieu. Il serait intéressant d’avoir un message affiché, confirmant qu’un nouvel enregistrement a été ajouté. Je laisse cela en guise d’exercice pour le lecteur. En outre, après avoir ajouté un nouvel enregistrement dans le contrôle DetailsView le contrôle GridView dans la page maître affiche toujours les cinq mêmes enregistrements qu’avant ; Il n’inclut pas l’enregistrement juste-ajouté. Nous allons examiner comment résoudre ce problème dans les étapes à venir.

> [!NOTE]
> Outre l’ajout d’une forme d’une rétroaction visuelle qui l’insertion a réussi, je vous encourage également mettre à jour d’interface d’insertion de DetailsView pour inclure la validation. Actuellement, il n’existe aucune validation. Si un utilisateur entre une valeur non valide pour le `UnitPrice` champ, tels que « trop coûteux, « une exception sera levée lors de la publication lorsque le système tente de convertir cette chaîne en un nombre décimal. Pour plus d’informations sur la personnalisation de l’insertion de l’interface, reportez-vous à la [ *personnalisation de l’Interface de Modification de données* didacticiel](../../data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) à partir de mon [utilisation de la série de didacticiels données](../../data-access/index.md).


## <a name="step-3-creating-public-properties-and-methods-in-the-master-page"></a>Étape 3 : Création de propriétés et méthodes publiques dans la Page maître

À l’étape 1, nous avons ajouté un contrôle Web Label nommé `GridMessage` ci-dessus le contrôle GridView dans la page maître. Cette étiquette est destinée à afficher éventuellement un message. Par exemple, après l’ajout d’un nouvel enregistrement à la `Products` table, que nous voulons afficher un message qui lit : «*ProductName* a été ajouté à la base de données. » Au lieu de coder en dur le texte de cette étiquette dans la page maître, nous pourrions le message à être personnalisés par la page de contenu.

Étant donné que le contrôle d’étiquette est implémenté comme une variable membre protégée au sein de la page maître que n’est pas accessible directement à partir de pages de contenu. Pour pouvoir fonctionner avec l’étiquette au sein d’une page maître à partir de la page de contenu (ou, d’ailleurs, n’importe quel contrôle Web dans la page maître) nous devons créer une propriété publique dans la page maître qui expose le contrôle Web ou sert de proxy par lequel une de ses propriétés peut être  accessible. Ajoutez la syntaxe suivante à la classe de code-behind de la page maître pour exposer l’étiquette `Text` propriété :


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample6.cs)]

Lorsqu’un nouvel enregistrement est ajouté à la `Products` table à partir d’une page de contenu le `RecentProducts` GridView dans la page maître a besoin relier à sa source de données sous-jacente. Pour relier l’appel de GridView son `DataBind` (méthode). Étant donné que le contrôle GridView dans la page maître n’est pas accessible par programme pour les pages de contenu, nous devez créer une méthode publique dans la page maître, lorsqu’elle est appelée, relie les données au GridView. Ajoutez la méthode suivante à la classe de code-behind de la page maître :


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample7.cs)]

Avec le `GridMessageText` propriété et `RefreshRecentProductsGrid` méthode en place, n’importe quelle page de contenu peut par programmation définir ou lire la valeur de la `GridMessage` l’étiquette `Text` propriété ou lier de nouveau les données à le `RecentProducts` GridView. Étape 4 examine comment accéder aux propriétés publiques et les méthodes de la page maître à partir d’une page de contenu.

> [!NOTE]
> N’oubliez pas de marquer des propriétés et des méthodes en tant que la page maître `public`. Si vous ne désignez pas des explicitement ces propriétés et méthodes en tant que `public`, ils ne seront plus accessibles à partir de la page de contenu.


## <a name="step-4-calling-the-master-pages-public-members-from-a-content-page"></a>Étape 4 : Appel des membres publics de la Page maître à partir d’une Page de contenu

Maintenant que la page maître a les propriétés publiques nécessaires et les méthodes, nous sommes prêts à appeler ces propriétés et méthodes à partir de la `AddProduct.aspx` page de contenu. Plus précisément, nous devons définir la page maître `GridMessageText` propriété et appelez ses `RefreshRecentProductsGrid` méthode une fois que le nouveau produit a été ajouté à la base de données. Tous les contrôles de Web ASP.NET données déclenchent des événements juste avant et après l’exécution de différentes tâches, qui permettent de facilement pour les développeurs de pages de prendre des mesures par programmation avant ou après la tâche. Par exemple, lorsque l’utilisateur final clique sur un bouton Insérer de DetailsView, de publication (postback) du contrôle DetailsView déclenche son `ItemInserting` événement avant de commencer l’insertion du flux de travail. Il insère ensuite l’enregistrement dans la base de données. Ensuite, le contrôle DetailsView déclenche son `ItemInserted` événement. Par conséquent, pour utiliser la page maître une fois que le nouveau produit a été ajouté, créer un gestionnaire d’événements pour le DetailsView `ItemInserted` événement.

Il existe une page de contenu peut interagir par programmation avec sa page maître de deux manières :

- À l’aide de la `Page.Master` propriété, qui retourne une référence faiblement typée à la page maître, ou
- Spécifiez le chemin de type ou le fichier de page maître par le biais de la page un `@MasterType` directive ; cela ajoute automatiquement une propriété fortement typée vers la page nommée `Master`.

Commençons par examiner les deux approches.

### <a name="using-the-loosely-typedpagemasterproperty"></a>À l’aide de la faiblement typé`Page.Master`propriété

Toutes les pages de web ASP.NET doit dériver de la `Page` (classe), qui se trouve dans le `System.Web.UI` espace de noms. Le `Page` classe inclut un [ `Master` propriété](https://msdn.microsoft.com/library/system.web.ui.page.master.aspx) qui retourne une référence à la page maître de la page. Si la page n’a pas d’une page maître `Master` retourne `null`.

Le `Master` propriété retourne un objet de type [ `MasterPage` ](https://msdn.microsoft.com/library/system.web.ui.masterpage.aspx) (également située dans le `System.Web.UI` espace de noms) qui est le type de base dont dérivent toutes les pages maîtres à partir de. Par conséquent, à l’utilisation des propriétés publiques ou les méthodes définies dans la page maître de notre site Web nous devons effectuer un cast du `MasterPage` objet retourné par la `Master` propriété vers le type approprié. Étant donné que nous avons nommé notre fichier de page maître `Site.master`, la classe code-behind s’appelait `Site`. Par conséquent, le code suivant convertit le `Page.Master` propriété à une instance de la classe de Site.


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample8.cs)]

Maintenant que nous avons converti la faiblement typé `Page.Master` propriété à la `Site` type, nous pouvons référencer les propriétés et méthodes spécifiques au Site. Comme le montre la Figure 7, la propriété publique `GridMessageText` apparaît dans la liste déroulante IntelliSense.


[![IntelliSense affiche notre Page principale propriétés et méthodes publiques](interacting-with-the-master-page-from-the-content-page-cs/_static/image20.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image19.png)

**Figure 07**: IntelliSense affiche notre Page principale propriétés et méthodes publiques ([cliquez pour afficher l’image en taille réelle](interacting-with-the-master-page-from-the-content-page-cs/_static/image21.png))


> [!NOTE]
> Si vous avez nommé votre fichier de page maître `MasterPage.master` , nom de la classe code-behind de la page maître est `MasterPage`. Cela peut entraîner des code ambiguë lors de la conversion du type `System.Web.UI.MasterPage` à votre `MasterPage` classe. En bref, vous devez qualifier complètement le type que vous effectuez un cast, ce qui peut être un peu difficile lorsque vous utilisez le modèle de projet de Site Web. Je vous suggère consisterait à faire en sorte que lorsque vous créez votre page maître vous il donnez un nom autre que `MasterPage.master` ou, mieux encore, créez une référence fortement typée à la page maître.


### <a name="creating-a-strongly-typed-reference-with-themastertypedirective"></a>Création d’une référence fortement typée avec la`@MasterType`(Directive)

Si vous examinez attentivement, vous pouvez voir que classe de code-behind d’une page ASP.NET est une classe partielle (Notez le `partial` mot clé dans la définition de classe). Classes partielles ont été introduits dans c# et Visual Basic avec.NET Framework 2.0 et, en bref, permettant aux membres d’une classe à être définie dans plusieurs fichiers. Le fichier de classe code-behind - `AddProduct.aspx.cs`, par exemple - contient le code que nous, le développeur de pages, créez. En plus de notre code, le moteur ASP.NET crée automatiquement un fichier de classe distincte avec des propriétés et gestionnaires d’événements qui traduisent par le balisage déclaratif hiérarchie de classe de la page.

La génération de code automatique qui se produit chaque fois qu’une page ASP.NET est visitée la voie pour certaines possibilités plutôt intéressant et utiles. Dans le cas des pages maîtres, si nous indiquent au moteur de ASP.NET quelle page maître est utilisé par notre page de contenu, cela génère un fortement typée `Master` propriété pour nous.

Utilisez le [ `@MasterType` directive](https://msdn.microsoft.com/library/ms228274.aspx) pour informer le moteur ASP.NET de type de page maître de la page de contenu. Le `@MasterType` directive peut accepter le nom de type de la page maître ou son chemin d’accès de fichier. Pour spécifier que le `AddProduct.aspx` page utilise `Site.master` comme sa page maître, ajoutez la directive suivante en haut du `AddProduct.aspx`:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample9.aspx)]

Cette directive indique au moteur d’ASP.NET pour ajouter une référence fortement typée à la page maître via une propriété nommée `Master`. Avec le `@MasterType` directive en place, nous pouvons appeler le `Site.master` maître de la page Propriétés et méthodes publiques directement via le `Master` propriété sans toutes les conversions.

> [!NOTE]
> Si vous omettez le `@MasterType` directive, la syntaxe `Page.Master` et `Master` retournent la même chose : un objet faiblement typé à la page maître de la page. Si vous incluez le `@MasterType` directive puis `Master` retourne une référence fortement typée à la page maître spécifiée. `Page.Master`, cependant, retourne toujours une référence faiblement typée. Pour examiner plus poussée pourquoi c’est le cas et comment la `Master` propriété est construite lors de la `@MasterType` directive est incluse, consultez [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx)d’entrée de blog [ `@MasterType` dans ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx).


### <a name="updating-the-master-page-after-adding-a-new-product"></a>La mise à jour de la Page maître après l’ajout d’un nouveau produit

Maintenant que nous savons comment appeler des propriétés publiques et les méthodes à partir d’une page de contenu d’une page maître, nous sommes prêts à mettre à jour le `AddProduct.aspx` page afin que la page maître soit actualisée après avoir ajouté un nouveau produit. Au début de l’étape 4, nous avons créé un gestionnaire d’événements pour le contrôle DetailsView `ItemInserting` événement, qui s’exécute immédiatement après que le nouveau produit a été ajouté à la base de données. Ajoutez le code suivant à ce gestionnaire d’événements :


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample10.cs)]

Le code ci-dessus utilise les deux le faiblement typé `Page.Master` propriété et fortement typées `Master` propriété. Notez que le `GridMessageText` propriété est définie sur «*ProductName* ajouté à la grille... » Les valeurs du produit ajouté juste sont accessibles via le `e.Values` collection, comme vous pouvez le voir, le juste ajouté `ProductName` valeur est accessible via `e.Values["ProductName"]`.

La figure 8 illustre la `AddProduct.aspx` page immédiatement après un nouveau produit - de Scott Soda - a été ajoutée à la base de données. Notez que le nom du produit juste-ajouté est indiqué dans l’étiquette de la page maître et que le contrôle GridView a été actualisé pour inclure le produit et son prix.


[![Affichent le produit ajouté juste de l’étiquette et GridView de la Page maître](interacting-with-the-master-page-from-the-content-page-cs/_static/image23.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image22.png)

**Figure 08**: la Page maître étiquette et GridView affichent le produit Just-Added ([cliquez pour afficher l’image en taille réelle](interacting-with-the-master-page-from-the-content-page-cs/_static/image24.png))


## <a name="summary"></a>Récapitulatif

Dans l’idéal, une page maître et ses pages de contenu sont complètement séparées les unes des autres et ne nécessitent aucun niveau d’interaction. Tandis que les pages maîtres et les pages de contenu doivent être conçus avec cet objectif à l’esprit, il existe un nombre de scénarios courants dans lesquels une page de contenu doit interagir avec sa page maître. Une des raisons plus courantes se concentre sur la mise à jour une partie spécifique de l’affichage de page maître basé sur une action qui se sont produites dans la page de contenu.

La bonne nouvelle est qu’il est relativement simple d’avoir une page de contenu interagir par programmation avec sa page maître. Commencez par créer des propriétés ou méthodes publiques dans la page maître qui encapsulent les fonctionnalités qui devant être appelé par une page de contenu. Puis, dans la page de contenu, accéder aux propriétés et méthodes de la page maître via les faiblement typé `Page.Master` propriété ou utilisez le `@MasterType` directive permettant de créer une référence fortement typée à la page maître.

Dans le didacticiel suivant, nous examinons comment la page maître interagir par programmation avec une de ses pages de contenu.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [L’accès et la mise à jour des données dans ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Pages maître ASP.NET : Conseils, astuces et pièges](http://www.odetocode.com/articles/450.aspx)
- [`@MasterType` dans ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)
- [Informations de transmission entre le contenu et les Pages maîtres](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Utilisation des données dans les didacticiels ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de plusieurs livres de sur ASP/ASP.NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier livre s’intitule [ *Sams Teach vous-même ASP.NET 3.5 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott peut être atteint à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou via son blog à [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Zack Jones. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](control-id-naming-in-content-pages-cs.md)
> [Suivant](interacting-with-the-content-page-from-the-master-page-cs.md)
