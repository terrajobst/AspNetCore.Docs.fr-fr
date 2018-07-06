---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
title: Interrogation des données avec le contrôle SqlDataSource (c#) | Microsoft Docs
author: rick-anderson
description: Dans les didacticiels précédents, nous avons utilisé le contrôle ObjectDataSource pour séparer complètement la couche de présentation à partir de la couche d’accès aux données. À compter de cette terminologique...
ms.author: aspnetcontent
ms.date: 02/20/2007
ms.assetid: 60512d6a-b572-4b7a-beb3-3e44b4d2020c
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 6b9ca474b7a085302d287a223c08abe2fa5336b0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815289"
---
<a name="querying-data-with-the-sqldatasource-control-c"></a>Interrogation des données avec le contrôle SqlDataSource (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_CS.exe) ou [télécharger le PDF](querying-data-with-the-sqldatasource-control-cs/_static/datatutorial47cs1.pdf)

> Dans les didacticiels précédents, nous avons utilisé le contrôle ObjectDataSource pour séparer complètement la couche de présentation à partir de la couche d’accès aux données. À partir de ce didacticiel, nous Découvrez comment le contrôle SqlDataSource peut être utilisé pour les applications simples qui ne nécessitent pas de ce type d’une séparation stricte de présentation et d’accès aux données.


## <a name="introduction"></a>Introduction

Tous les didacticiels nous ve analysée jusqu'à présent ont utilisé une architecture à plusieurs niveaux constituée de présentation, logique métier et couches d’accès aux données. La couche DAL (Data Access) a été conçu dans le premier didacticiel ([création d’une couche d’accès aux données](../introduction/creating-a-data-access-layer-cs.md)) et la couche de logique métier dans le deuxième ([création d’une couche de logique métier](../introduction/creating-a-business-logic-layer-cs.md)). En commençant par le [affichant les données avec ObjectDataSource the](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) didacticiel, nous avons vu comment utiliser le contrôle ObjectDataSource ASP.NET 2.0 s de manière déclarative l’interface avec l’architecture de la couche présentation.

Tandis que tous les didacticiels jusqu'à présent ont utilisé l’architecture pour exploiter les données, il est également possible d’accéder à insérer, mettre à jour et supprimer des données de base de données directement à partir d’une page ASP.NET, en ignorant l’architecture. Cela place les requêtes de base de données spécifique et une logique métier directement dans la page web. Pour les applications suffisamment volumineux ou complexes, concevoir, implémenter et à l’aide d’une architecture à plusieurs niveaux sont essentiel pour la réussite, les mises à jour et maintenance de l’application. Développement d’une architecture robuste, toutefois, peut s’avérer inutile lors de la création d’applications extrêmement simples et ponctuelles.

ASP.NET 2.0 fournit des contrôles de source de données intégrés cinq [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx), et [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx). SqlDataSource peut être utilisé pour accéder et modifier les données directement à partir d’une base de données relationnel, y compris Microsoft SQL Server, Microsoft Access, Oracle, MySQL et autres utilisateurs. Dans ce didacticiel et les trois suivants, nous allons examiner comment travailler avec le contrôle SqlDataSource, explorant comment interroger et filtrer les données de base de données, ainsi que comment utiliser SqlDataSource pour insérer, mettre à jour et supprimer des données.


![ASP.NET 2.0 inclut cinq contrôles de Source de données intégrés](querying-data-with-the-sqldatasource-control-cs/_static/image1.gif)

**Figure 1**: ASP.NET 2.0 inclut cinq contrôles de Source de données intégrés


## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>Comparaison de l’ObjectDataSource et SqlDataSource

Conceptuellement, les contrôles de l’ObjectDataSource et SqlDataSource sont simplement des proxys aux données. Comme indiqué dans le [affichant les données avec ObjectDataSource the](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) didacticiel, ObjectDataSource a des propriétés qui indiquent le type d’objet qui fournit les données et les méthodes à appeler pour sélectionner, insérer, mettre à jour et supprimer des données à partir du type d’objet sous-jacent. Après ont configuré les propriétés de s ObjectDataSource, un contrôle Web tel qu’un GridView, DetailsView ou DataList peuvent être lié aux données au contrôle, à l’aide de l’ObjectDataSource s `Select()`, `Insert()`, `Delete()`, et `Update()` méthodes à interagir avec l’architecture sous-jacente.

SqlDataSource fournit les mêmes fonctionnalités, mais s’applique à une base de données relationnelle plutôt que dans une bibliothèque d’objets. Avec SqlDataSource, nous devez spécifier la chaîne de connexion de base de données et les requêtes SQL ad hoc ou des procédures stockées pour exécuter pour insérer, mettre à jour, supprimer et récupérer des données. Les opérations de mappage SqlDataSource `Select()`, `Insert()`, `Update()`, et `Delete()` méthodes, lorsqu’elle est appelée, se connecter à la base de données spécifiée et exécuter une requête SQL appropriée. Comme le montre le diagramme suivant, ces méthodes effectuent les tâches rébarbatives de connexion à une base de données, émettre une requête et retourner les résultats.


![SqlDataSource sert de Proxy pour la base de données](querying-data-with-the-sqldatasource-control-cs/_static/image2.gif)

**Figure 2**: SqlDataSource sert de Proxy pour la base de données


> [!NOTE]
> Dans ce didacticiel, nous nous concentrerons sur la récupération de données à partir de la base de données. Dans le [insertion, mise à jour et suppression des données avec le contrôle SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md) didacticiel, nous verrons comment configurer SqlDataSource pour prendre en charge d’insertion, de la mise à jour et de suppression.


## <a name="the-sqldatasource-and-accessdatasource-controls"></a>Les AccessDataSource contrôles SqlDataSource

Outre le contrôle SqlDataSource, ASP.NET 2.0 inclut également un contrôle AccessDataSource. Ces deux contrôles différents entraîner de nombreux développeurs nouvelle sur ASP.NET 2.0 de suspecter que le contrôle AccessDataSource est conçu pour fonctionner exclusivement avec Microsoft Access avec le contrôle SqlDataSource conçu pour fonctionner exclusivement avec Microsoft SQL Server. Le AccessDataSource est conçu pour fonctionner spécialement avec Microsoft Access, le contrôle SqlDataSource fonctionne avec *n’importe quel* base de données relationnelle qui sont accessibles via .NET. Cela inclut tout OleDb ou ODBC compatible magasins de données, telles que Microsoft SQL Server, Microsoft Access, Oracle, Informix, MySQL et PostgreSQL, entre autres.

La seule différence entre les contrôles AccessDataSource et SqlDataSource est la façon dont les informations de connexion de base de données sont spécifiées. Le contrôle AccessDataSource doit simplement le chemin d’accès du fichier de base de données Access. SqlDataSource, nécessite quant à eux, une chaîne de connexion complète.

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>Étape 1 : Création de Pages Web de SqlDataSource

Avant de commencer la manière de travailler directement avec les données de base de données à l’aide du contrôle SqlDataSource, permettent de s tout d’abord prendre un moment pour créer les pages ASP.NET dans notre projet de site Web que nous avons besoin pour ce didacticiel et les trois suivants. Commencez par ajouter un nouveau dossier nommé `SqlDataSource`. Ensuite, ajoutez les pages ASP.NET suivantes à ce dossier, en veillant à associer chaque page avec le `Site.master` page maître :

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`


![Ajouter les Pages ASP.NET pour les didacticiels de SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image3.gif)

**Figure 3**: ajoutez les Pages ASP.NET pour les didacticiels de SqlDataSource


Comme dans les autres dossiers, `Default.aspx` dans le `SqlDataSource` dossier répertorie les didacticiels dans sa section. N’oubliez pas que le `SectionLevelTutorialListing.ascx` contrôle utilisateur fournit cette fonctionnalité. Par conséquent, ajoutez ce contrôle utilisateur à `Default.aspx` en le faisant glisser à partir de l’Explorateur de solutions sur la page s en mode Création.


[![Ajouter le contrôle utilisateur de SectionLevelTutorialListing.ascx à Default.aspx](querying-data-with-the-sqldatasource-control-cs/_static/image5.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image4.gif)

**Figure 4**: ajouter la `SectionLevelTutorialListing.ascx` contrôle utilisateur à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](querying-data-with-the-sqldatasource-control-cs/_static/image6.gif))


Enfin, ajoutez ces quatre pages sous forme d’entrées pour le `Web.sitemap` fichier. Plus précisément, ajoutez le balisage suivant après l’ajout de boutons personnalisés pour les contrôles DataList et Repeater `<siteMapNode>`:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample1.sql)]

Après la mise à jour `Web.sitemap`, prenez un moment pour afficher le site Web de didacticiels via un navigateur. Le menu de gauche inclut désormais les éléments de la modification, insertion et suppression des didacticiels.


![Le plan de Site inclut maintenant des entrées pour les didacticiels de SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image7.gif)

**Figure 5**: le plan de Site inclut maintenant des entrées pour les didacticiels de SqlDataSource


## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>Étape 2 : Ajout et configuration du contrôle SqlDataSource

Commencez par ouvrir le `Querying.aspx` page dans le `SqlDataSource` dossier et basculez en mode Design. Faites glisser un contrôle SqlDataSource à partir de la boîte à outils vers le concepteur et le jeu de son `ID` à `ProductsDataSource`. À l’instar de l’ObjectDataSource, SqlDataSource ne génère aucune sortie rendue et par conséquent apparaît sous la forme d’une zone grise sur l’aire de conception. Pour configurer SqlDataSource, cliquez sur le lien configurer la Source de données à partir de la balise active de SqlDataSource s.


![Cliquez sur la configuration de lien de Source de données à partir de la balise active de s SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image8.gif)

**Figure 6**: cliquez sur la configuration de lien de Source de données à partir de la balise active de s SqlDataSource


Ceci fait apparaître l’Assistant Configurer la Source de données de SqlDataSource contrôle s. Les étapes de l’Assistant s différent à partir du contrôle ObjectDataSource s, l’objectif final est le même pour fournir les détails sur la façon de récupérer, insérer, mettre à jour et supprimer des données via la source de données. Pour SqlDataSource, cela implique la base de données sous-jacent à utiliser et de fournir les instructions SQL ad hoc ou les procédures stockées.

La première étape de l’Assistant nous invite pour la base de données. La liste déroulante inclut les bases de données trouvées dans le s d’application web `App_Data` dossier et ceux qui ont été ajoutés au nœud Connexions de données dans l’Explorateur de serveurs. Depuis que nous avons ajouté une déjà une chaîne de connexion pour le `NORTHWIND.MDF` de base de données dans le `App_Data` dossier à notre projet s `Web.config` fichier, la liste déroulante inclut une référence à cette chaîne de connexion, `NORTHWINDConnectionString`. Choisissez cet élément dans la liste déroulante et cliquez sur Suivant.


![Choisissez le NORTHWINDConnectionString dans la liste déroulante](querying-data-with-the-sqldatasource-control-cs/_static/image9.gif)

**Figure 7**: choisissez la `NORTHWINDConnectionString` dans la liste déroulante


Après avoir choisi la base de données, l’Assistant vous demande de la requête pour retourner des données. Nous pouvons spécifier les colonnes d’une table ou vue à retourner ou peut entrer une instruction SQL personnalisée ou spécifier une procédure stockée. Vous pouvez basculer entre ce choix via la spécifier une instruction SQL personnalisée ou une procédure stockée et spécifiez les colonnes d’une table ou afficher des cases d’option.

> [!NOTE]
> Pour ce premier exemple permettent s d’utiliser les spécifiez les colonnes d’une option de table ou vue. Nous revenir à l’Assistant, plus loin dans ce didacticiel et explorez la spécifier une instruction SQL personnalisée ou une option de procédure stockée.


La figure 8 illustre la configurer à l’écran de l’instruction Select lorsque vous spécifiez les colonnes à partir d’un bouton de case d’option de table ou la vue est sélectionnée. La liste déroulante contienne l’ensemble des tables et des vues dans la base de données Northwind, avec la table sélectionnée ou des colonnes de s vue affichées dans la liste de case à cocher ci-dessous. Pour cet exemple, s permettent de retourner le `ProductID`, `ProductName`, et `UnitPrice` colonnes à partir de la `Products` table. Comme le montre la Figure 8, une fois vos ces sélections de l’Assistant affiche l’instruction SQL obtenue `SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`.


![Retourner des données à partir de la Table Products](querying-data-with-the-sqldatasource-control-cs/_static/image10.gif)

**Figure 8**: renvoyer des données depuis le `Products` Table


Une fois que vous avez configuré l’Assistant pour retourner le `ProductID`, `ProductName`, et `UnitPrice` colonnes à partir de la `Products` , cliquez sur le bouton suivant. Cet écran final permet d’examiner les résultats de la requête configurée à partir de l’étape précédente. En cliquant sur le bouton Tester la requête s’exécute configuré `SELECT` instruction et affiche les résultats dans une grille.


![Cliquez sur le bouton de requête de Test pour passer en revue votre requête SELECT](querying-data-with-the-sqldatasource-control-cs/_static/image11.gif)

**Figure 9**: cliquez sur le bouton de la requête de Test pour vérifier votre `SELECT` requête


Pour terminer l’Assistant, cliquez sur Terminer.

Comme avec ObjectDataSource, l’Assistant de s SqlDataSource simplement assigne des valeurs aux propriétés du contrôle s, à savoir le [ `ConnectionString` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx) et [ `SelectCommand` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx) propriétés. À l’issue de l’Assistant, votre balisage déclaratif s du contrôle SqlDataSource doit ressembler à ce qui suit :


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample2.aspx)]

Le `ConnectionString` propriété fournit des informations sur la façon de se connecter à la base de données. Cette propriété peut avoir une valeur de chaîne de connexion complète, codée en dur ou peut pointer vers une chaîne de connexion dans `Web.config`. Pour faire référence à une valeur de chaîne de connexion dans le fichier Web.config, utilisez la syntaxe `<%$ expressionPrefix:expressionValue %>`. En règle générale, *expressionPrefix* est ConnectionStrings et *expressionValue* est le nom de la chaîne de connexion dans le `Web.config` [ `<connectionStrings>` section](https://msdn.microsoft.com/library/bf7sd233.aspx). Toutefois, la syntaxe peut être utilisée pour référencer `<appSettings>` éléments ou du contenu à partir de fichiers de ressources. Consultez [vue d’ensemble des Expressions ASP.NET](https://msdn.microsoft.com/library/d5bd1tad.aspx) pour plus d’informations sur cette syntaxe.

Le `SelectCommand` propriété spécifie l’instruction de SQL ad hoc ou procédure stockée à exécuter pour retourner les données.

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>Étape 3 : Ajout d’un contrôle Web de données et en la liant à SqlDataSource

Une fois que SqlDataSource a été configuré, il peut être lié à un contrôle Web, tel qu’un GridView ou d’un contrôle DetailsView de données. Pour ce didacticiel, permettent d’afficher les données dans un GridView s. Dans la boîte à outils, faites glisser un GridView sur la page, puis liez-le à le `ProductsDataSource` SqlDataSource en choisissant la source de données à partir de la liste déroulante dans la balise active de s GridView.


[![Ajouter un GridView et le lier au contrôle SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image13.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image12.gif)

**Figure 10**: ajouter un GridView et le lier au contrôle SqlDataSource ([cliquez pour afficher l’image en taille réelle](querying-data-with-the-sqldatasource-control-cs/_static/image14.gif))


Une fois que vous avez déjà sélectionné le contrôle SqlDataSource à partir de la liste déroulante dans la balise active de s GridView, Visual Studio ajoute automatiquement un BoundField ou du CheckBoxField au GridView pour chacune des colonnes retournées par le contrôle de source de données. Étant donné que SqlDataSource retourne trois colonnes de base de données `ProductID`, `ProductName`, et `UnitPrice` il existe trois champs dans le contrôle GridView.

Prenez un moment pour configurer les trois opérations de mappage GridView BoundFields. Modifier le `ProductName` champ s `HeaderText` propriété nom de produit et le `UnitPrice` s champ prix. Également mettre en forme le `UnitPrice` champ sous forme de devise. Après avoir apporté ces modifications, votre balisage déclaratif de GridView s doit ressembler à ce qui suit :


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample3.aspx)]

Visitez cette page via un navigateur. Comme le montre la Figure 11, le contrôle GridView répertorie chaque produit s `ProductID`, `ProductName`, et `UnitPrice` valeurs.


[![Le contrôle GridView affiche chaque produit s ProductID, ProductName et UnitPrice valeurs](querying-data-with-the-sqldatasource-control-cs/_static/image16.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image15.gif)

**Figure 11**: le GridView affiche chaque produit s `ProductID`, `ProductName`, et `UnitPrice` valeurs ([cliquez pour afficher l’image en taille réelle](querying-data-with-the-sqldatasource-control-cs/_static/image17.gif))


Lorsque la page est visitée le GridView appelle son contrôle de source de données s `Select()` (méthode). Lorsque nous utilisions le contrôle ObjectDataSource, on le `ProductsBLL` classe s `GetProducts()` (méthode). Avec SqlDataSource, toutefois, le `Select()` méthode établit une connexion à la base de données spécifiée et les problèmes le `SelectCommand` (`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`, dans cet exemple). SqlDataSource retourne ses résultats dans lequel le contrôle GridView énumère ensuite, création d’une ligne dans le contrôle GridView pour chaque enregistrement de base de données retourné.

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>Les fonctionnalités de contrôle de données intégrés Web et le contrôle SqlDataSource

En général, les fonctionnalités inhérentes aux données de contrôles Web d’échange, le tri, modification, suppression, insertion et ainsi de suite sont spécifiques au contrôle Web de données et ne sont pas dépendantes sur le contrôle de source de données utilisé. Autrement dit, le contrôle GridView peut utiliser son intégré la pagination, de tri, de modification et de suppression si elle est liée à un ObjectDataSource ou un SqlDataSource. Toutefois, certaines fonctionnalités de contrôle Web de données sont sensibles au contrôle de source de données utilisé ou à la configuration de contrôle s de source de données.

Par exemple, dans le [efficacement la pagination par le biais d’importants volumes de données](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) didacticiel nous l’avons vu comment procéder, par défaut, la logique de pagination pour les données Web contrôles naïvement retourne *tous les* enregistrements sous-jacent source de données et affiche alors uniquement un sous-ensemble d’enregistrements à l’index de page actuel et du nombre d’enregistrements à afficher par page approprié. Ce modèle est très peu efficace lors de la pagination via suffisamment grands jeux de résultats. Heureusement, ObjectDataSource peut être configuré pour prendre en charge la pagination personnalisée, qui retourne uniquement le sous-ensemble précis d’enregistrements à afficher. Le contrôle SqlDataSource, toutefois, ne dispose pas des propriétés pour l’implémentation de la pagination personnalisée.

Une autre subtilité avec pagination et tri survient avec SqlDataSource. Par défaut, les données retournées à partir d’un SqlDataSource peuvent être paginées ou triées via le contrôle GridView. Pour illustrer cela, vérifiez les options Activer la pagination et activer le tri dans la balise active de s GridView dans `Querying.aspx` et vérifiez que cela fonctionne comme prévu.

Tri et la pagination fonctionnent parce que SqlDataSource récupère les données de base de données dans un DataSet faiblement typé. Le nombre total d’enregistrements renvoyés par la requête un aspect essentiel pour implémenter la pagination peut être déterminée par le jeu de données. En outre, les résultats de s de jeu de données peuvent être triés via un DataView. Ces fonctionnalités sont utilisées automatiquement par SqlDataSource lorsque les demandes de GridView paginés ou les données triées.

SqlDataSource peut être configuré pour retourner un objet DataReader au lieu d’un jeu de données en modifiant son [ `DataSourceMode` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx) de `DataSet` (la valeur par défaut) à `DataReader`. À l’aide d’un DataReader peut être recommandé dans les situations lors du passage des résultats s SqlDataSource au code existant qui attend un DataReader. En outre, étant donné que DataReaders sont des objets beaucoup plus simples que de jeux de données, ils offrent de meilleures performances. Si vous apportez cette modification, toutefois, le contrôle Web de données ne peut ni trier ni page depuis SqlDataSource ne peut pas déterminer le nombre d’enregistrements retourné par la requête, pas plus qu’il ne le DataReader offre des techniques pour trier les données retournées.

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>Étape 4 : Utilisation d’une instruction SQL personnalisée ou une procédure stockée

Lorsque vous configurez le contrôle SqlDataSource, la requête utilisée pour retourner des données peut être spécifiée dans une des deux approches comme une procédure stockée ou une instruction SQL personnalisée ou en tant que colonnes à partir d’une table ou une vue. À l’étape 2, nous avons examiné en sélectionnant les colonnes à partir de la `Products` table. Permettent d’examiner l’utilisation d’une instruction SQL personnalisée s.

Ajouter un autre contrôle GridView à la `Querying.aspx` page et choisir de créer une nouvelle source de données à partir de la liste déroulante dans la balise active. Ensuite, indiquent que les données se fera à partir d’une base de données cela créera un nouveau contrôle SqlDataSource. Nommez le contrôle `ProductsWithCategoryInfoDataSource`.


![Créer un contrôle SqlDataSource nommé ProductsWithCategoryInfoDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image18.gif)

**Figure 12**: créer un contrôle SqlDataSource nommé `ProductsWithCategoryInfoDataSource`


L’écran suivant nous demande de spécifier la base de données. Comme nous l’avons fait dans la Figure 7, sélectionnez le `NORTHWINDConnectionString` à partir de la liste déroulante liste et cliquez sur Suivant. Dans la configuration de l’écran de l’instruction Select, choisissez la spécifier une instruction SQL personnalisée ou une procédure stockée case et cliquez sur Suivant. Cela fera apparaître l’écran de définir des instructions personnalisées ou des procédures stockées, qui offre des onglets : SELECT, UPDATE, INSERT et DELETE. Dans chaque onglet, vous pouvez entrer une instruction SQL personnalisée dans la zone de texte ou choisissez une procédure stockée dans la liste déroulante. Dans ce didacticiel, nous examinerons entrer une instruction SQL personnalisée ; le didacticiel suivant inclut un exemple qui utilise une procédure stockée.


![Entrez une instruction SQL personnalisée ou sélectionner une procédure stockée](querying-data-with-the-sqldatasource-control-cs/_static/image19.gif)

**Figure 13**: entrez une instruction SQL personnalisée ou sélectionner une procédure stockée


L’instruction SQL personnalisée peut être entrée manuellement dans la zone de texte ou peut être construite sous forme graphique en cliquant sur le bouton Générateur de requêtes. Dans le Générateur de requêtes ou la zone de texte, utilisez la requête suivante pour renvoyer le `ProductID` et `ProductName` champs à partir de la `Products` à l’aide de la table un `JOIN` pour récupérer le produit s `CategoryName` à partir de la `Categories` table :


[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample4.sql)]


![Vous pouvez graphiquement construire la requête à l’aide du Générateur de requêtes](querying-data-with-the-sqldatasource-control-cs/_static/image20.gif)

**Figure 14**: vous pouvez construire graphiquement la requête en utilisant le Générateur de requêtes


Après avoir spécifié la requête, cliquez sur Suivant pour passer à l’écran de requête de Test. Cliquez sur Terminer pour terminer l’Assistant de SqlDataSource.

À l’issue de l’Assistant, le contrôle GridView aura trois BoundFields ajoutés à ce dernier affichage de la `ProductID`, `ProductName`, et `CategoryName` colonnes retournées par la requête et résultant dans le balisage déclaratif suivant :


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample5.aspx)]


[![Le contrôle GridView affiche chaque ID de produit s, le nom de la catégorie de nom et associé](querying-data-with-the-sqldatasource-control-cs/_static/image22.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image21.gif)

**Figure 15**: le GridView affiche chaque produit s ID, nom et nom de catégorie associés ([cliquez pour afficher l’image en taille réelle](querying-data-with-the-sqldatasource-control-cs/_static/image23.gif))


## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons vu comment interroger et afficher des données à l’aide du contrôle SqlDataSource. Comme l’ObjectDataSource, SqlDataSource sert de proxy, en fournissant une approche déclarative à l’accès aux données. Ses propriétés spécifient pour se connecter à la base de données et SQL `SELECT` pour exécuter de requête ; elles peuvent être spécifiées via la fenêtre Propriétés ou à l’aide de l’Assistant Configurer la source de données.

Le `SELECT` exemples de requête que nous avons examiné dans ce didacticiel tous les enregistrements renvoyés par la requête spécifiée. Toutefois, le contrôle SqlDataSource, peut inclure un `WHERE` clause avec des paramètres dont les valeurs sont affectées par programmation ou sont automatiquement extraites d’une source spécifiée. Nous allons examiner comment créer et utiliser des requêtes paramétrables dans le didacticiel suivant !

Bonne programmation !

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [L’accès aux données de base de données relationnelle](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [Vue d’ensemble du contrôle SqlDataSource](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [Didacticiels de démarrage rapide ASP.NET : Le contrôle SqlDataSource](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [Le fichier Web.config `<connectionStrings>` élément](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [Référence de chaîne de connexion de base de données](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Susan Connery Bernadette Leigh et David Suru. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](using-parameterized-queries-with-the-sqldatasource-cs.md)
