---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-vb
title: Création d’une couche d’accès aux données (VB) | Microsoft Docs
author: rick-anderson
description: Dans ce didacticiel, nous allons démarrer depuis le début et créer la couche DAL (Data Access), à l’aide de groupes de données typés, accéder aux informations dans une base de données.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/05/2010
ms.topic: article
ms.assetid: 6227233a-6254-4b6b-9a89-947efef22330
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: 34a107eac32f9a6d022612d47aa319b02c190070
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402965"
---
<a name="creating-a-data-access-layer-vb"></a>Création d’une couche d’accès aux données (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger l’exemple d’application](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_1_VB.exe) ou [télécharger le PDF](creating-a-data-access-layer-vb/_static/datatutorial01vb1.pdf)

> Dans ce didacticiel, nous allons démarrer depuis le début et créer la couche DAL (Data Access), à l’aide de groupes de données typés, accéder aux informations dans une base de données.


## <a name="introduction"></a>Introduction

En tant que développeurs web, nos vies tournent autour de l’utilisation des données. Nous allons créer des bases de données pour stocker les données, le code pour extraire et modifier et les pages web pour collecter et résumer. Il s’agit du premier didacticiel d’une longue série qui explorera les techniques permettant d’implémenter ces modèles courants dans ASP.NET 2.0. Nous allons commencer par créer un [architecture logicielle](http://en.wikipedia.org/wiki/Software_architecture) composé d’une couche DAL (Data Access) à l’aide de données typés, une couche BLL (Business Logic) qui applique des règles d’entreprise personnalisées, et une couche de présentation composée d’ASP.NET pages qui partager une mise en page courantes. Une fois que ce point de départ du serveur principal a été défini, nous passerons à la création de rapports, montrant comment afficher, synthétiser, collecter et valider les données à partir d’une application web. Ces didacticiels sont conçues pour être concis et fournissent des instructions pas à pas avec de nombreuses captures d’écran pour vous guident tout au long du processus visuellement. Chaque didacticiel est disponible dans les versions de Visual Basic et c# et inclut un téléchargement de l’intégralité du code utilisé. (Ce premier didacticiel est assez long, mais le reste sont présentées dans des segments digestes beaucoup plus).

Pour ces didacticiels nous allons utiliser une version de Microsoft SQL Server 2005 Express Edition de la base de données Northwind placé dans le `App_Data` directory. Outre le fichier de base de données, le `App_Data` dossier contient également les scripts SQL pour la création de la base de données, au cas où vous souhaitez utiliser une version de base de données différente. Ces scripts peuvent également être [téléchargé directement auprès de Microsoft](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en), si vous préférez. Si vous utilisez une autre version de SQL Server de la base de données Northwind, vous devez mettre à jour le `NORTHWNDConnectionString` définition dans l’application `Web.config` fichier. L’application web a été créée à l’aide de Visual Studio 2005 Professional Edition en tant qu’un projet de site Web basé sur le système de fichiers. Toutefois, tous les didacticiels fonctionnera aussi bien avec la version gratuite de Visual Studio 2005, [Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/).

Dans ce didacticiel, nous allons démarrer depuis le début et créer la couche DAL (Data Access), suivie de la création de la [couche BLL (Business Logic)](creating-a-business-logic-layer-vb.md) dans le deuxième didacticiel, travailler sur [mise en page et navigation](master-pages-and-site-navigation-vb.md) dans la troisième. Les didacticiels après que celui tiers s’appuiera sur la Fondation présentées dans les trois premiers. Nous avons beaucoup de choses à aborder dans ce premier didacticiel, par conséquent, démarrez Visual Studio et c’est parti !

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>Étape 1 : Création d’un projet Web et la connexion à la base de données

Avant de nous pouvons créer notre couche d’accès aux données (DAL), nous devons d’abord créer un site web et le programme d’installation de notre base de données. Commencez par créer un nouveau fichier basé sur un système web site ASP.NET. Pour ce faire, accédez au menu fichier et choisissez Nouveau Site Web, en affichant la boîte de dialogue Nouveau Site Web. Choisissez le modèle de Site Web ASP.NET, la valeur de la liste déroulante emplacement du système de fichiers, choisissez un dossier pour placer le site web et définir la langue de Visual Basic.


[![Créer un Site Web de système de nouveau fichier](creating-a-data-access-layer-vb/_static/image2.png)](creating-a-data-access-layer-vb/_static/image1.png)

**Figure 1**: créer un Site Web de New File System-Based ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image3.png))


Cela créera un nouveau site web avec un `Default.aspx` page ASP.NET, un `App_Data` dossier et un `Web.config` fichier.

Avec le site web créé, l’étape suivante consiste à ajouter une référence à la base de données dans l’Explorateur de serveurs de Visual Studio. En ajoutant une base de données à l’Explorateur de serveurs vous pouvez ajouter des tables, des procédures stockées, vues et ainsi de suite, le tout depuis Visual Studio. Vous pouvez également afficher les données de la table ou créer vos propres requêtes manuellement ou sous forme graphique via le Générateur de requêtes. En outre, lorsque nous générons les jeux de données typé pour la couche DAL nous allons devoir pointer Visual Studio à la base de données à partir duquel les jeux de données typé doit être construit. Bien que nous pouvons fournir ces informations de connexion à ce stade dans le temps, Visual Studio remplit automatiquement une liste déroulante des bases de données déjà inscrite dans l’Explorateur de serveurs.

Les étapes pour l’ajout de la base de données Northwind à l’Explorateur de serveurs varient selon que vous souhaitiez utiliser la base de données SQL Server 2005 Express Edition dans la `App_Data` dossier ou si vous avez un Microsoft SQL Server 2000 ou une configuration de serveur de base de données 2005 que vous souhaitez utiliser à la place.

## <a name="using-a-database-in-theappdatafolder"></a>À l’aide d’une base de données dans le`App_Data`dossier

Si vous n’avez pas d’un SQL Server 2000 ou 2005 server de base de données pour se connecter à, ou si vous souhaitez simplement éviter d’avoir à ajouter la base de données à un serveur de base de données, vous pouvez utiliser la version de SQL Server 2005 Express Edition de la base de données Northwind qui se trouve dans l’aucun téléchargé e's `App_Data` dossier (`NORTHWND.MDF`).

Une base de données est placé dans le `App_Data` dossier est automatiquement ajouté à l’Explorateur de serveurs. En supposant que vous disposez de SQL Server 2005 Express Edition installé sur votre ordinateur, vous devez voir un nœud nommé NORTHWND. MDF dans l’Explorateur de serveurs, que vous pouvez développer et explorez ses tables, vues, procédures stockées et ainsi de suite (voir Figure 2).

Le `App_Data` dossier peut contenir également Microsoft Access `.mdb` fichiers, qui, comme leurs homologues SQL Server, sont automatiquement ajoutés à l’Explorateur de serveurs. Si vous ne souhaitez pas utiliser une des options de SQL Server, vous pouvez toujours [télécharger une version de Microsoft Access du fichier de base de données Northwind](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN) et le déposer dans le `App_Data` directory. N’oubliez pas, toutefois, que les bases de données Access ne sont pas aussi riches en fonctionnalités que SQL Server et ne sont pas conçus pour être utilisés dans les scénarios de site web. En outre, deux des didacticiels 35 + vont utiliser certaines fonctionnalités de niveau de base de données qui ne sont pas pris en charge par l’accès.

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>Connexion à la base de données dans un serveur de base de données Microsoft SQL Server 2000 ou 2005

Ou bien, vous pouvez vous connecter à une base de données Northwind installé sur un serveur de base de données. Si le serveur de base de données n’a pas déjà installé la base de données Northwind, vous devez ajoutez-le d’abord au serveur de base de données en exécutant le script d’installation inclus dans le téléchargement de ce didacticiel ou par [télécharger la version de SQL Server 2000 de Northwind et le script d’installation](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en) directement à partir de site web de Microsoft.

Une fois que vous avez installé la base de données, accédez à l’Explorateur de serveurs dans Visual Studio, cliquez sur le nœud Connexions de données, puis choisissez Ajouter une connexion. Si vous ne voyez pas l’Explorateur de serveurs Accédez à la vue / Explorateur de serveurs, ou appuyez sur Ctrl + Alt + S. Cela fait apparaître la boîte de dialogue Ajouter une connexion, où vous pouvez spécifier le serveur pour vous connecter à, les informations d’authentification et le nom de la base de données. Une fois que vous avez correctement configuré les informations de connexion de base de données et cliqué sur le bouton OK, la base de données est ajouté en tant que nœud sous le nœud Connexions de données. Vous pouvez développer le nœud de base de données pour Explorer ses tables, vues, procédures stockées et ainsi de suite.


![Ajouter une connexion à la base de données de votre serveur de base de données Northwind](creating-a-data-access-layer-vb/_static/image4.png)

**Figure 2**: ajouter une connexion à la base de données de votre serveur de base de données Northwind


## <a name="step-2-creating-the-data-access-layer"></a>Étape 2 : Création de la couche d’accès aux données

Lorsque vous travaillez avec les données une option consiste à incorporer la logique spécifique de données directement dans la couche de présentation (dans une application web, la marque de pages ASP.NET de la couche de présentation). Cette opération peut prendre la forme d’écriture du code ADO.NET dans la partie du code de la page ASP.NET ou à l’aide du contrôle SqlDataSource à partir de la partie du balisage. Dans les deux cas, cette approche couple étroitement la logique d’accès aux données avec la couche de présentation. Il est toutefois, l’approche recommandée, pour séparer la logique d’accès aux données à partir de la couche de présentation. Cette couche distincte est appelée par la couche d’accès aux données, la couche DAL pour faire court et est généralement implémentée comme un projet de bibliothèque de classes distinct. Les avantages de cette architecture en couches sont bien documentées (voir la section « Autres lectures » à la fin de ce didacticiel pour plus d’informations sur ces avantages) et est l’approche que nous puissions appliquer dans cette série.

Tout le code qui est spécifique à la source de données sous-jacente telles que la création d’une connexion à la base de données émission `SELECT`, `INSERT`, `UPDATE`, et `DELETE` commandes et ainsi de suite doivent se trouver dans la couche DAL. La couche de présentation ne doit pas contenir toutes les références à ce code d’accès aux données, mais il doit effectuer à la place des appels dans la couche DAL pour les demandes de toutes les données. Couches d’accès aux données contiennent généralement des méthodes pour accéder à la base de données sous-jacente. La base de données Northwind, par exemple, a `Products` et `Categories` tables qui enregistrent les produits pour les catégories à laquelle ils appartiennent et de la vente. Dans notre DAL nous aura des méthodes telles que :

- `GetCategories(),` qui retourne des informations sur toutes les catégories
- `GetProducts()`, qui retourne des informations sur tous les produits
- `GetProductsByCategoryID(categoryID)`, qui retourne tous les produits qui appartiennent à une catégorie spécifiée
- `GetProductByProductID(productID)`, qui retourne des informations sur un produit particulier

Ces méthodes, lorsqu’elle est appelée, seront se connecter à la base de données, exécuter une requête appropriée et retourner les résultats. Comment nous retournons ces résultats est important. Ces méthodes peut simplement retourner un jeu de données ou d’un DataReader rempli par la requête de base de données, mais dans l’idéal, ces résultats doivent être retournés à l’aide de *objets fortement typés*. Un objet fortement typé est un dont le schéma est préconçus strictement défini au moment de la compilation, tandis que l’inverse, un objet faiblement typé, est un dont le schéma n’est pas connu jusqu'à l’exécution.

Par exemple, le DataReader et le jeu de données (par défaut) sont des objets faiblement typé, car leur schéma est défini par les colonnes retournées par la requête de base de données utilisée pour les remplir. Pour accéder à une colonne particulière d’un DataTable faiblement typé, nous devons utiliser une syntaxe comme : `DataTable.Rows(index)("columnName")`. Typage faible de la table de données dans cet exemple est exposée par le fait que nous avons besoin d’accéder au nom de colonne à l’aide d’une chaîne ou un index ordinal. Un DataTable fortement typée, quant à eux, aura chacune de ses colonnes implémentés en tant que propriétés, ce qui entraîne un code qui ressemble à : `DataTable.Rows(index).columnName`.

Pour retourner des objets fortement typés, les développeurs peuvent créer leurs propres objets métier personnalisés ou utiliser des DataSets typés. Un objet métier est implémenté par le développeur comme représente une classe dont les propriétés reflètent généralement les colonnes de la table sous-jacente de la base de données l’objet métier. Un DataSet typé est une classe générée pour vous par Visual Studio basé sur un schéma de base de données et dont les membres sont fortement typées en fonction de ce schéma. Le jeu de données typées elle-même se compose des classes qui étendent les classes ADO.NET DataSet, DataTable et DataRow. En plus des tables de données fortement typées, typés incluent maintenant des TableAdapters, qui sont des classes avec des méthodes pour remplir les tables de données du jeu de données et la propagation des modifications dans les tables de données dans la base de données.

> [!NOTE]
> Pour plus d’informations sur les avantages et inconvénients de l’utilisation de DataSets typés et des objets métier personnalisés, reportez-vous à [composants de conception de la couche données et en passant par le biais des couches de données](https://msdn.microsoft.com/library/ms978496.aspx).


Nous allons utiliser des jeux de données fortement typées pour l’architecture des ces didacticiels. La figure 3 illustre le flux de travail entre les différentes couches d’une application qui utilise des DataSet typés.


[![Tout Code d’accès aux données est relégués à la couche DAL](creating-a-data-access-layer-vb/_static/image6.png)](creating-a-data-access-layer-vb/_static/image5.png)

**Figure 3**: tous les Code d’accès aux données est relégués à la couche DAL ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image7.png))


## <a name="creating-a-typed-dataset-and-table-adapter"></a>Création d’un DataSet typé et un adaptateur de Table

Pour commencer à créer notre DAL, nous commençons par ajouter un DataSet typé à notre projet. Pour ce faire, avec le bouton droit sur le nœud de projet dans l’Explorateur de solutions et choisissez Ajouter un nouvel élément. Sélectionnez l’option de jeu de données à partir de la liste des modèles et nommez-le `Northwind.xsd`.


[![Choisissez d’ajouter un nouveau jeu de données à votre projet](creating-a-data-access-layer-vb/_static/image9.png)](creating-a-data-access-layer-vb/_static/image8.png)

**Figure 4**: choisissez d’ajouter un nouveau jeu de données à votre projet ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image10.png))


Après avoir cliqué sur Ajouter, lorsque vous êtes invité à ajouter le jeu de données pour le `App_Code` dossier, cliquez sur Oui. Le concepteur pour le DataSet typé s’affiche, et l’Assistant Configuration de TableAdapter démarre, ce qui vous permet d’ajouter votre premier TableAdapter au DataSet typé.

Un DataSet typé constitue une collection fortement typée des données ; Il est composé fortement typée d’instances de DataTable, chacun d’eux est à son tour composé d’instances de DataRow fortement typée. Nous allons créer une table de données fortement typées pour chacune des tables de base de données sous-jacente dont nous avons besoin pour travailler dans cette série de didacticiels. Nous allons commencer par la création d’un DataTable pour le `Products` table.

N’oubliez pas que les tables de données fortement typées n’incluent pas toutes les informations sur l’accès aux données à partir de leur table de base de données sous-jacente. Afin de récupérer les données pour remplir le DataTable, nous utilisons une classe TableAdapter, qui fonctionne comme notre couche d’accès aux données. Pour notre `Products` DataTable, le TableAdapter contiendra les méthodes `GetProducts()`, `GetProductByCategoryID(categoryID)`, et ainsi de suite que nous allons appeler à partir de la couche de présentation. Rôle du DataTable est de servir en tant que les objets fortement typés permettant de passer des données entre les couches.

L’Assistant Configuration de TableAdapter commence par vous invitant à sélectionner la base de données à utiliser. La liste déroulante affiche ces bases de données dans l’Explorateur de serveurs. Si vous n’avez pas ajouté à la base de données Northwind à l’Explorateur de serveurs, vous pouvez cliquer sur le bouton Nouvelle connexion pour l’instant à le faire.


[![Choisissez la base de données Northwind dans la liste déroulante](creating-a-data-access-layer-vb/_static/image12.png)](creating-a-data-access-layer-vb/_static/image11.png)

**Figure 5**: choisissez la base de données Northwind dans la liste déroulante ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image13.png))


Après avoir sélectionné la base de données et en cliquant sur Suivant, vous demandera si vous souhaitez enregistrer la chaîne de connexion dans le `Web.config` fichier. En enregistrant la chaîne de connexion, vous éviterez en fait, il dur codé dans les classes TableAdapter, ce qui simplifie les choses si les informations de chaîne de connexion changent à l’avenir. Si vous optez pour enregistrer la chaîne de connexion dans le fichier de configuration, il est placé dans le `<connectionStrings>` section, ce qui peut être [éventuellement chiffré](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx) pour l’amélioration de la sécurité ou modifiées ultérieurement via la Page de propriétés de 2.0 ASP.NET dans l’IIS Admin outil GUI, qui est plus idéale pour les administrateurs.


[![Enregistrer la chaîne de connexion dans Web.config](creating-a-data-access-layer-vb/_static/image15.png)](creating-a-data-access-layer-vb/_static/image14.png)

**Figure 6**: enregistrer la chaîne de connexion dans `Web.config` ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image16.png))


Ensuite, nous devons définir le schéma pour le premier DataTable fortement typée et fournir la première méthode pour notre TableAdapter à utiliser lors du remplissage du DataSet fortement typée. Ces deux étapes sont réalisées simultanément en créant une requête qui retourne les colonnes de la table que nous voulons reflétées dans notre DataTable. À la fin de l’Assistant, nous allons donner un nom de méthode pour cette requête. Une fois que qui a été accompli, cette méthode peut être appelée à partir de notre couche de présentation. La méthode exécute la requête définie et remplir un DataTable fortement typée.

Pour commencer la définition de la requête SQL, nous devons tout d’abord indiquer comment nous voulons le TableAdapter pour émettre la requête. Nous pouvons utiliser d’instruction SQL ad hoc, créez une nouvelle procédure stockée ou utiliser une procédure stockée existante. Pour ces didacticiels, nous utiliserons des instructions SQL ad hoc. Reportez-vous à [Brian Noyes](http://briannoyes.net/)de l’article, [créer une couche d’accès aux données avec le Concepteur de DataSet Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner) pour obtenir un exemple d’utilisation de procédures stockées.


[![Interroger les données à l’aide d’une instruction SQL de Ad-Hoc](creating-a-data-access-layer-vb/_static/image18.png)](creating-a-data-access-layer-vb/_static/image17.png)

**Figure 7**: interroger les données à l’aide d’une instruction SQL de Ad-Hoc ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image19.png))


À ce stade nous pouvons saisir dans la requête SQL manuellement. Lors de la création de la première méthode du TableAdapter vous souhaitez généralement que la requête retourne les colonnes qui doivent être exprimés dans le DataTable. Nous pouvons le faire en créant une requête qui retourne toutes les colonnes et toutes les lignes de la `Products` table :


[![Entrez la requête SQL dans la zone de texte](creating-a-data-access-layer-vb/_static/image21.png)](creating-a-data-access-layer-vb/_static/image20.png)

**Figure 8**: entrez la requête SQL dans le Textbox ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image22.png))


Vous pouvez également utiliser le Générateur de requêtes et de construire graphiquement la requête, comme illustré à la Figure 9.


[![Créer la requête sous forme de graphique, par le biais de l’éditeur de requête](creating-a-data-access-layer-vb/_static/image24.png)](creating-a-data-access-layer-vb/_static/image23.png)

**Figure 9**: créer la requête sous forme graphique, par le biais de l’éditeur de requête ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image25.png))


Après avoir créé la requête, mais avant de passer à l’écran suivant, cliquez sur le bouton Options avancées. Dans les projets de Site Web, « générer, mise à jour, instructions Insert et Delete » est la seule option activée par défaut ; avancée Si vous exécutez cet Assistant à partir d’une bibliothèque de classes ou d’un projet Windows l’option « Utiliser l’accès concurrentiel optimiste » sera également être sélectionnée. Laissez l’option « Utiliser l’accès concurrentiel optimiste » désactivée pour l’instant. Nous allons examiner l’accès concurrentiel optimiste dans les didacticiels futures.


[![Sélectionnez uniquement les instructions générer Insert, Update et Delete Option](creating-a-data-access-layer-vb/_static/image27.png)](creating-a-data-access-layer-vb/_static/image26.png)

**Figure 10**: sélectionnez uniquement les instructions générer Insert, Update et Delete Option ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image28.png))


Après avoir vérifié les options avancées, cliquez sur Suivant pour passer à l’écran final. Ici nous avons invités à sélectionner les méthodes à ajouter au TableAdapter. Il existe deux modèles de remplissage des données :

- **Remplir un DataTable** avec cette approche est de créer une méthode qui prend un DataTable en tant que paramètre et le remplit en fonction des résultats de la requête. La classe DataAdapter ADO.NET, par exemple, implémente ce modèle avec son `Fill()` (méthode).
- **Retourner un DataTable** avec cette approche de la méthode crée et remplit la table de données pour vous et le retourne comme valeur de retour de méthodes.

Vous pouvez avoir le TableAdapter à implémenter une ou les deux de ces modèles. Vous pouvez également renommer les méthodes fournies ici. Laissez les cases cochées, bien que nous utiliserons uniquement ce dernier modèle tout au long de ces didacticiels. Nous allons renommer également, au lieu de cela générique `GetData` méthode `GetProducts`.

Si elle est activée, la case à cocher finale, « GenerateDBDirectMethods, » crée `Insert()`, `Update()`, et `Delete()` méthodes du TableAdapter. Si vous laissez cette option désactivée, toutes les mises à jour doit être effectuée via la seule du TableAdapter `Update()` (méthode), qui accepte le jeu de données typé, un DataTable, un seul objet DataRow ou un tableau de DataRows. (Si vous avez désactivé les « générer Insert, Update et Delete instructions » option à partir des propriétés avancées dans la Figure 9, cette case à cocher paramètre n’a aucun effet.) Laissez cette case à cocher activée.


[![Diffère de nom de la méthode GetData GetProducts](creating-a-data-access-layer-vb/_static/image30.png)](creating-a-data-access-layer-vb/_static/image29.png)

**Figure 11**: modifier le nom de la méthode à partir de `GetData` à `GetProducts` ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image31.png))


Terminez l’Assistant en cliquant sur Terminer. Une fois que l’Assistant se ferme, nous revenons au Concepteur de DataSet qui affiche la table de données que nous venons de créer. Vous pouvez voir la liste des colonnes dans le `Products` DataTable (`ProductID`, `ProductName`, et ainsi de suite), ainsi que les méthodes de la `ProductsTableAdapter` (`Fill()` et `GetProducts()`).


[![Le DataTable de produits et ProductsTableAdapter ont été ajoutés au DataSet typé](creating-a-data-access-layer-vb/_static/image33.png)](creating-a-data-access-layer-vb/_static/image32.png)

**Figure 12**: le `Products` DataTable et `ProductsTableAdapter` ont été ajoutés au DataSet typé ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image34.png))


À ce stade, nous avons un DataSet typé avec un seul objet DataTable (`Northwind.Products`) et une classe fortement typée de DataAdapter (`NorthwindTableAdapters.ProductsTableAdapter`) avec un `GetProducts()` (méthode). Ces objets peuvent être utilisés pour accéder à une liste de tous les produits à partir de code tel que :

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample1.vb)]

Ce code ne nécessitait pas nous écrire un peu de code d’accès spécifique de données. Nous n’avait pas d’instancier des classes ADO.NET, nous n’avions pas à faire référence à des chaînes de connexion, les requêtes SQL, ou des procédures stockées. Au lieu de cela, le TableAdapter fournit le code d’accès aux données de bas niveau pour nous.

Chaque objet utilisé dans cet exemple est également fortement typées, ce qui permet de Visual Studio fournir IntelliSense et la vérification du type de compilation. Et le meilleur de toutes les tables de données retournés par le TableAdapter peut être lié aux données ASP.NET Web contrôles, tels que le contrôle GridView, DetailsView, DropDownList, CheckBoxList et plusieurs autres. L’exemple suivant illustre la liaison le DataTable retourné par la `GetProducts()` méthode sur un GridView dans un incomplètes trois lignes de code seulement dans le `Page_Load` Gestionnaire d’événements.

AllProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-vb/samples/sample2.aspx)]

AllProducts.aspx.vb

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample3.vb)]


[![La liste de produits s’affiche dans un GridView](creating-a-data-access-layer-vb/_static/image36.png)](creating-a-data-access-layer-vb/_static/image35.png)

**Figure 13**: la liste de produits s’affiche dans un GridView ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image37.png))


Bien que cet exemple obligatoire que nous écrivons les trois lignes de code dans notre page ASP.NET `Page_Load` Gestionnaire d’événements, dans un avenir didacticiels, nous allons examiner l’utilisation de l’ObjectDataSource de façon déclarative récupérer les données à partir de la couche DAL. Avec ObjectDataSource, nous n’aurez pas à écrire du code et obtiendra également en charge la pagination et tri !

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>Étape 3 : Ajout paramétré des méthodes pour la couche d’accès aux données

À ce stade notre `ProductsTableAdapter` classe ne possède qu’une seule méthode, `GetProducts()`, qui retourne tous les produits dans la base de données. S’il est sans aucun doute utile de pouvoir travailler avec tous les produits, voici les heures lorsque nous devrons pour récupérer des informations sur un produit spécifique ou tous les produits qui appartiennent à une catégorie particulière. Pour ajouter ces fonctionnalités à notre couche d’accès aux données, nous pouvons ajouter des méthodes paramétrables au TableAdapter.

Nous allons ajouter le `GetProductsByCategoryID(categoryID)` (méthode). Pour ajouter une nouvelle méthode à la couche DAL, retournez dans le Concepteur de DataSet, cliquez sur le `ProductsTableAdapter` section, puis choisissez Ajouter une requête.


![Avec le bouton droit sur le TableAdapter et choisissez Ajouter une requête](creating-a-data-access-layer-vb/_static/image38.png)

**Figure 14**: avec le bouton droit sur le TableAdapter et choisissez Ajouter une requête


Nous allons tout d’abord invité à indiquer si nous souhaitons accéder à la base de données à l’aide d’une instruction de SQL ad hoc ou d’une procédure stockée nouveau ou existante. Nous allons choisir d’utiliser une instruction SQL ad-hoc. Ensuite, nous avons demandés quel type de requête SQL que nous souhaitons utiliser. Dans la mesure où nous voulons renvoyer tous les produits qui appartiennent à une catégorie spécifiée, nous souhaitons écrire un `SELECT` instruction qui retourne des lignes.


[![Choisissez de créer une instruction SELECT qui retourne des lignes](creating-a-data-access-layer-vb/_static/image40.png)](creating-a-data-access-layer-vb/_static/image39.png)

**Figure 15**: choisissez de créer un `SELECT` instruction qui retourne les lignes ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image41.png))


L’étape suivante consiste à définir la requête SQL utilisée pour accéder aux données. Dans la mesure où nous voulons renvoyer uniquement les produits qui appartiennent à une catégorie particulière, j’utilise le même `SELECT` instruction à partir de `GetProducts()`, mais ajoutez le code suivant `WHERE` clause : `WHERE CategoryID = @CategoryID`. Le `@CategoryID` paramètre indique à l’Assistant TableAdapter que la méthode que nous créons nécessite un paramètre d’entrée du type correspondant (à savoir, un entier nullable).


[![Entrez une requête pour retourner uniquement les produits dans une catégorie spécifiée](creating-a-data-access-layer-vb/_static/image43.png)](creating-a-data-access-layer-vb/_static/image42.png)

**Figure 16**: entrez une requête pour retourner uniquement les produits dans une catégorie spécifiée ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image44.png))


Dans la dernière étape que nous pouvons choisir les modèles à utiliser, ainsi que de personnaliser les noms des méthodes générées d’accès aux données. Pour le motif de remplissage, nous allons modifier le nom à `FillByCategoryID` et pour la valeur de retour un DataTable retourner le modèle (le `GetX` méthodes), nous allons utiliser `GetProductsByCategoryID`.


[![Choisissez les noms pour les méthodes TableAdapter](creating-a-data-access-layer-vb/_static/image46.png)](creating-a-data-access-layer-vb/_static/image45.png)

**Figure 17**: choisissez les noms pour les méthodes TableAdapter ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image47.png))


À l’issue de l’Assistant, le Concepteur de DataSet inclut les nouvelles méthodes TableAdapter.


![Les produits peuvent maintenant être interrogées par catégorie](creating-a-data-access-layer-vb/_static/image48.png)

**Figure 18**: The le produits peut maintenant être interrogées par catégorie


Prenez un moment pour ajouter un `GetProductByProductID(productID)` méthode à l’aide de la même technique.

Ces requêtes paramétrables peuvent être testées directement depuis le Concepteur de DataSet. Avec le bouton droit sur la méthode du TableAdapter et choisissez Aperçu des données. Ensuite, entrez les valeurs à utiliser pour les paramètres et cliquez sur Aperçu.


[![Ces appartenant produits à la catégorie boissons sont affichés.](creating-a-data-access-layer-vb/_static/image50.png)](creating-a-data-access-layer-vb/_static/image49.png)

**Figure 19**: les produits appartenant à la catégorie boissons sont affichés ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image51.png))


Avec la `GetProductsByCategoryID(categoryID)` méthode dans notre DAL, nous pouvons maintenant créer une page ASP.NET qui affiche uniquement les produits dans une catégorie spécifiée. L’exemple suivant affiche tous les produits qui se trouvent dans la catégorie des boissons, et qui ont un `CategoryID` de 1.

Beverages.aspx

[!code-aspx[Main](creating-a-data-access-layer-vb/samples/sample4.aspx)]

Beverages.aspx.vb

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample5.vb)]


[![Ces produits de la catégorie boissons sont affichés.](creating-a-data-access-layer-vb/_static/image53.png)](creating-a-data-access-layer-vb/_static/image52.png)

**Figure 20**: ceux produits dans la catégorie boissons sont affichés ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image54.png))


## <a name="step-4-inserting-updating-and-deleting-data"></a>Étape 4 : Insertion, la mise à jour et suppression de données

Il existe deux modèles couramment utilisés pour l’insertion, la mise à jour et suppression de données. Le premier modèle, que j’appellerai le modèle direct de base de données, implique la création de méthodes qui, lorsqu’elle est appelée, problème un `INSERT`, `UPDATE`, ou `DELETE` commande à la base de données qui fonctionne sur un enregistrement de base de données unique. Ces méthodes sont généralement transmis dans une série de valeurs scalaires (entiers, chaînes, valeurs booléennes, dates/heures et ainsi de suite) qui correspondent aux valeurs à insérer, mettre à jour ou supprimer. Par exemple, avec ce modèle pour le `Products` table, la méthode delete prendrait dans un paramètre de type entier indiquant le `ProductID` de l’enregistrement à supprimer, tandis que la méthode d’insertion prendrait dans une chaîne pour le `ProductName`, une valeur décimale pour la `UnitPrice`, un entier pour le `UnitsOnStock`, et ainsi de suite.


[![Chaque insertion, mise à jour et supprimer la demande sont envoyée à la base de données immédiatement](creating-a-data-access-layer-vb/_static/image56.png)](creating-a-data-access-layer-vb/_static/image55.png)

**Figure 21**: insérer chaque mise à jour et supprimer la demande est envoyée à la base de données immédiatement ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image57.png))


L’autre modèle, je me référerai à dans le lot de mises à jour de modèle, consiste à mettre à jour d’un DataSet, DataTable ou collection de DataRows dans un appel de méthode ensemble. Avec ce modèle un développeur supprime, insère et modifie les DataRows dans un DataTable, puis transmet ces objets DataRow ou un DataTable dans une méthode de mise à jour. Puis cette méthode énumère les DataRows transmis, détermine s’ils ont été modifiés, ajoutés ou supprimés (via le DataRow [propriété RowState](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) valeur) et émet la demande de base de données appropriée pour chaque enregistrement.


[![Toutes les modifications sont synchronisées avec la base de données lorsque la méthode de mise à jour est appelée.](creating-a-data-access-layer-vb/_static/image59.png)](creating-a-data-access-layer-vb/_static/image58.png)

**Figure 22**: toutes les modifications sont synchronisées avec la base de données lorsque la méthode de mise à jour est appelée ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image60.png))


Le TableAdapter utilise le modèle de mise à jour de lot par défaut, mais prend également en charge le modèle de base de données directe. Étant donné que nous avons sélectionné l’option « Générer, mise à jour, instructions Insert et Delete » à partir des propriétés avancées lors de la création de notre TableAdapter, le `ProductsTableAdapter` contient un `Update()` (méthode), qui implémente le modèle de mise à jour par lots. Plus précisément, le TableAdapter contient un `Update()` méthode qui peut être passée pour le jeu de données typé, un DataTable fortement typée ou DataRows un ou plusieurs. Si vous avez laissé la case à cocher « GenerateDBDirectMethods » cochée lorsque créant d’abord le TableAdapter le modèle direct de base de données sera également implémentée par le biais de `Insert()`, `Update()`, et `Delete()` méthodes.

Les deux modèles de modification de données utilisent le TableAdapter `InsertCommand`, `UpdateCommand`, et `DeleteCommand` propriétés émettre leurs `INSERT`, `UPDATE`, et `DELETE` commandes pour la base de données. Vous pouvez inspecter et modifier le `InsertCommand`, `UpdateCommand`, et `DeleteCommand` propriétés en cliquant sur le TableAdapter dans le Concepteur de DataSet, puis en accédant à la fenêtre Propriétés. (Vérifiez que vous avez sélectionné le TableAdapter et qui le `ProductsTableAdapter` objet est celui sélectionné dans la liste déroulante dans la fenêtre Propriétés.)


[![Le TableAdapter a InsertCommand, UpdateCommand et DeleteCommand propriétés](creating-a-data-access-layer-vb/_static/image62.png)](creating-a-data-access-layer-vb/_static/image61.png)

**Figure 23**: le TableAdapter a `InsertCommand`, `UpdateCommand`, et `DeleteCommand` propriétés ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image63.png))


Pour examiner ou modifier ces propriétés de commande de base de données, cliquez sur le `CommandText` sous-propriété, qui ouvre le Générateur de requêtes.


[![Configurer le INSERT, UPDATE et instructions DELETE dans le Générateur de requêtes](creating-a-data-access-layer-vb/_static/image65.png)](creating-a-data-access-layer-vb/_static/image64.png)

**Figure 24**: configurer le `INSERT`, `UPDATE`, et `DELETE` instructions dans le Générateur de requêtes ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image66.png))


L’exemple de code suivant montre comment utiliser le modèle de mise à jour par lots à doubler le prix de tous les produits qui ne sont pas supprimées et qui ont des 25 unités en stock ou moins :

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample6.vb)]

Le code ci-dessous illustre comment utiliser le modèle direct de base de données par programmation supprimer un produit particulier, puis mettre à jour un et ajouter un nouveau :

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample7.vb)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>Création personnalisée Insert, Update, méthodes et Delete

Le `Insert()`, `Update()`, et `Delete()` méthodes créées par la méthode directe de base de données peuvent être un peu délicate, en particulier pour les tables avec plusieurs colonnes. Examinez l’exemple de code précédent, sans IntelliSense aide n’est pas particulièrement claire que `Products` colonne de table est mappé à chaque paramètre d’entrée pour le `Update()` et `Insert()` méthodes. Il peut arriver lorsque nous ne souhaitons mettre à jour une seule colonne ou les deux, ou un texte personnalisé `Insert()` méthode qui sera, par exemple, retourner la valeur de l’enregistrement inséré `IDENTITY` les champ (incrémentation automatique).

Pour créer une telle méthode personnalisée, revenez dans le Concepteur de DataSet. Avec le bouton droit sur le TableAdapter et choisissez Ajouter une requête, retour à l’Assistant TableAdapter. Sur le deuxième écran, nous pouvons indiquer le type de requête permettant de créer. Nous allons créer une méthode qui ajoute un nouveau produit, puis retourne la valeur de l’enregistrement qui vient d’être ajouté `ProductID`. Par conséquent, choisir de créer un `INSERT` requête.


[![Créez une méthode pour ajouter une nouvelle ligne à la Table Products](creating-a-data-access-layer-vb/_static/image68.png)](creating-a-data-access-layer-vb/_static/image67.png)

**Figure 25**: créer une méthode pour ajouter une nouvelle ligne à la `Products` Table ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image69.png))


Dans l’écran suivant le `InsertCommand`de `CommandText` s’affiche. Augmenter cette requête en ajoutant des `SELECT SCOPE_IDENTITY()` à la fin de la requête, qui retourne la dernière valeur identity insérée dans une `IDENTITY` colonne dans la même portée. (Consultez le [documentation technique](https://msdn.microsoft.com/library/ms190315.aspx) pour plus d’informations sur `SCOPE_IDENTITY()` et pourquoi vous souhaiterez probablement [utiliser étendue\_Identity() n’à la place de @@IDENTITY](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx).) Assurez-vous que vous obtenez le `INSERT` instruction par un point-virgule avant d’ajouter le `SELECT` instruction.


[![Augmenter la requête pour retourner la valeur SCOPE_IDENTITY()](creating-a-data-access-layer-vb/_static/image71.png)](creating-a-data-access-layer-vb/_static/image70.png)

**Figure 26**: augmenter la requête pour retourner le `SCOPE_IDENTITY()` valeur ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image72.png))


Enfin, nommez la nouvelle méthode `InsertProduct`.


[![Définir le nouveau nom de méthode à InsertProduct](creating-a-data-access-layer-vb/_static/image74.png)](creating-a-data-access-layer-vb/_static/image73.png)

**Figure 27**: la valeur est le nouveau nom de méthode `InsertProduct` ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image75.png))


Lorsque vous revenez sur le Concepteur de DataSet vous verrez que le `ProductsTableAdapter` contient une nouvelle méthode, `InsertProduct`. Si cette nouvelle méthode n’a pas un paramètre pour chaque colonne dans le `Products` table, il est probable qu’est vous avez oublié de mettre fin à la `INSERT` instruction par un point-virgule. Configurer le `InsertProduct` (méthode) et assurez-vous d’avoir un point-virgule qui entourent le `INSERT` et `SELECT` instructions.

Par défaut, insérer les méthodes de requête non de méthodes problème, ce qui signifie qu’elles retournent le nombre de lignes affectées. Toutefois, nous voulons le `InsertProduct` méthode pour retourner la valeur retournée par la requête, et non le nombre de lignes affectées. Pour ce faire, vous devez ajuster le `InsertProduct` la méthode `ExecuteMode` propriété `Scalar`.


[![Remplacez la propriété ExecuteMode scalaire](creating-a-data-access-layer-vb/_static/image77.png)](creating-a-data-access-layer-vb/_static/image76.png)

**Figure 28**: modification du `ExecuteMode` propriété `Scalar` ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image78.png))


Le code suivant illustre cette nouvelle `InsertProduct` méthode en action :

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample8.vb)]

## <a name="step-5-completing-the-data-access-layer"></a>Étape 5 : Fin de la couche d’accès aux données

Notez que le `ProductsTableAdapters` classe retourne la `CategoryID` et `SupplierID` des valeurs du `Products` de table, mais n’inclut pas le `CategoryName` colonne à partir de la `Categories` table ou la `CompanyName` colonne à partir de la `Suppliers` la table, bien qu’il s’agit probablement les colonnes que nous souhaitons afficher lors de l’affichage des informations sur le produit. Nous pouvons augmenter la méthode du TableAdapter initiale, `GetProducts()`, à inclure à la fois le `CategoryName` et `CompanyName` les valeurs de colonne, qui met à jour la table de données fortement typées pour inclure également ces nouvelles colonnes.

Cela peut poser un problème, cependant, comme les méthodes du TableAdapter pour l’insertion, la mise à jour, et suppression des données sont basées sur cette méthode initiale. Heureusement, les méthodes générées automatiquement pour l’insertion, de la mise à jour et de suppression ne sont pas affectées par les sous-requêtes dans les `SELECT` clause. En veillant à ajouter nos requêtes à `Categories` et `Suppliers` comme des sous-requêtes, plutôt que `JOIN` s, nous allons éviter d’avoir à retravailler ces méthodes de modification des données. Avec le bouton droit sur le `GetProducts()` méthode dans le `ProductsTableAdapter` et choisissez configurer. Ensuite, ajustez le `SELECT` clause afin qu’il se présente comme :

[!code-sql[Main](creating-a-data-access-layer-vb/samples/sample9.sql)]


[![Mettre à jour de l’instruction SELECT de la méthode GetProducts()](creating-a-data-access-layer-vb/_static/image80.png)](creating-a-data-access-layer-vb/_static/image79.png)

**Figure 29**: mise à jour le `SELECT` instruction pour la `GetProducts()` (méthode) ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image81.png))


Après la mise à jour le `GetProducts()` méthode à utiliser cette nouvelle requête de la table de données inclut deux nouvelles colonnes : `CategoryName` et `SupplierName`.


![Le DataTable produits présente deux nouvelles colonnes](creating-a-data-access-layer-vb/_static/image82.png)

**Figure 30**: le `Products` DataTable présente deux nouvelles colonnes


Prenez un moment pour mettre à jour le `SELECT` clause dans la `GetProductsByCategoryID(categoryID)` méthode également.

Si vous mettez à jour le `GetProducts()` `SELECT` à l’aide de `JOIN` syntaxe le Concepteur de DataSet ne pourrez pas générer automatiquement les méthodes d’insertion, la mise à jour, et suppression des données de base de données à l’aide de la base de données directe de modèle. Au lieu de cela, vous devrez créer manuellement les beaucoup, comme nous l’avons fait avec la `InsertProduct` méthode précédemment dans ce didacticiel. En outre, vous devrez manuellement fournissent la `InsertCommand`, `UpdateCommand`, et `DeleteCommand` les valeurs de propriété si vous souhaitez utiliser le lot de mise à jour du modèle.

## <a name="adding-the-remaining-tableadapters"></a>Ajouter les TableAdapters restants

Jusqu'à présent, nous avons examiné uniquement travailler avec un TableAdapter unique pour une table de base de données unique. Toutefois, la base de données Northwind contient plusieurs tables associées que nous allons devoir travailler avec dans notre application web. Un DataSet typé peut contenir plusieurs, liées DataTables. Par conséquent, pour terminer notre DAL nous devons ajouter des tables de données pour les autres tables, que nous allons utiliser dans ces didacticiels. Pour ajouter un nouveau TableAdapter à un DataSet typé, ouvrez le Concepteur de DataSet, avec le bouton droit dans le concepteur et choisissez Ajouter / TableAdapter. Cela crée un nouveau DataTable et TableAdapter et vous guident tout au long de l’Assistant que nous avons examiné précédemment dans ce didacticiel.

Prenez quelques minutes pour créer des TableAdapters et les requêtes suivantes d’à l’aide des méthodes suivantes. Notez que les requêtes dans le `ProductsTableAdapter` inclure de sous-requêtes pour récupérer les noms de catégorie et le fournisseur de chaque produit. En outre, si vous avez suivi le long, vous avez déjà ajouté le `ProductsTableAdapter` la classe `GetProducts()` et `GetProductsByCategoryID(categoryID)` méthodes.

- **ProductsTableAdapter**

  - **GetProducts**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample10.sql)]
  - **GetProductsByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample11.sql)]
  - **GetProductsBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample12.sql)]
  - **GetProductByProductID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample13.sql)]
- **CategoriesTableAdapter**

  - **GetCategories**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample14.sql)]
  - **GetCategoryByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample15.sql)]
- **SuppliersTableAdapter**

  - **GetSuppliers**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample16.sql)]
  - **GetSuppliersByCountry**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample17.sql)]
  - **GetSupplierBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample18.sql)]
- **EmployeesTableAdapter**

  - **GetEmployees**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample19.sql)]
  - **GetEmployeesByManager**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample20.sql)]
  - **GetEmployeeByEmployeeID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample21.sql)]


[![Le Concepteur de DataSet une fois que les quatre TableAdapters ont été ajoutés.](creating-a-data-access-layer-vb/_static/image84.png)](creating-a-data-access-layer-vb/_static/image83.png)

**Figure 31**: le jeu de données concepteur après le quatre TableAdapters ont été ajoutés ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image85.png))


## <a name="adding-custom-code-to-the-dal"></a>Ajouter du Code personnalisé à la couche DAL

Les TableAdapters et les DataTables ajouté au jeu de données typé sont exprimées en tant qu’un fichier de définition de schéma XML (`Northwind.xsd`). Vous pouvez afficher ces informations de schéma en cliquant sur le `Northwind.xsd` de fichiers dans l’Explorateur de solutions et en choisissant Afficher le Code.


[![Le fichier XML Schema Definition (XSD) pour le Northwind de DataSet typé](creating-a-data-access-layer-vb/_static/image87.png)](creating-a-data-access-layer-vb/_static/image86.png)

**Figure 32**: le XML (fichier XSD Schema Definition) pour le DataSet typé de Northwind ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image88.png))


Ces informations de schéma sont traduites en code c# ou Visual Basic au moment du design lorsque compilé ou lors de l’exécution (si nécessaire), après quoi vous pouvez détaillé avec le débogueur. Pour afficher ce code généré automatiquement accédez à l’affichage de classes et l’extraction vers le bas pour les classes TableAdapter ou DataSet typée. Si vous ne voyez pas l’affichage de classes sur votre écran, accédez au menu Affichage et sélectionnez-la à partir de là ou appuyez sur Ctrl + Maj + C. À partir de l’affichage de classes, vous pouvez voir les propriétés, les méthodes et les événements des classes Typed DataSet et TableAdapter. Pour afficher le code pour une méthode particulière, double-cliquez sur le nom de méthode dans l’affichage de classes ou avec le bouton droit dessus et choisissez Atteindre la définition.


![Inspecter le Code généré automatiquement en sélectionnant Aller à la définition de l’affichage de classes](creating-a-data-access-layer-vb/_static/image89.png)

**Figure 33**: inspecter le Code généré automatiquement en sélectionnant Aller à la définition de l’affichage de classes


Si le code généré automatiquement peut être économiser beaucoup de temps, le code est souvent très générique et doit être personnalisé pour répondre aux besoins uniques d’une application. Le risque d’étendre le code généré automatiquement, cependant, est que l’outil qui a généré le code peut décider, qu'il est temps de « régénérer » et d’écraser vos personnalisations. Avec le nouveau concept d’une classe partielle de .NET 2.0, il est facile fractionner une classe dans plusieurs fichiers. Cela permet d’ajouter nos propres méthodes, les propriétés et les événements pour les classes générées automatiquement sans avoir à vous soucier de remplacer notre personnalisations de Visual Studio.

Pour illustrer comment personnaliser la couche DAL, nous allons ajouter un `GetProducts()` méthode à la `SuppliersRow` classe. Le `SuppliersRow` classe représente un enregistrement unique dans le `Suppliers` table ; chaque fournisseur de peut fournisseur zéro pour nombreux produits, donc `GetProducts()` renverra ces produits du fournisseur spécifié. Pour accomplir cela créer un nouveau fichier de classe dans le `App_Code` dossier nommé `SuppliersRow.vb` et ajoutez le code suivant :

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample22.vb)]

Cette classe partielle indique au compilateur qu’au moment où créer le `Northwind.SuppliersRow` classe pour inclure la `GetProducts()` méthode que nous venons de définir. Si vous générez votre projet et puis revenez à l’affichage de classes que vous verrez `GetProducts()` maintenant répertorié en tant que méthode de `Northwind.SuppliersRow`.


![La méthode GetProducts() est désormais une partie de la classe Northwind.SuppliersRow](creating-a-data-access-layer-vb/_static/image90.png)

**Figure 34**: le `GetProducts()` méthode fait désormais partie de la `Northwind.SuppliersRow` classe


Le `GetProducts()` méthode peut maintenant être utilisée pour énumérer l’ensemble des produits pour un fournisseur particulier, comme le montre le code suivant :

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample23.vb)]

Ces données peuvent également être affichées dans les ASP. Données de NET contrôles Web. La page suivante utilise un contrôle GridView avec deux champs :

- Un BoundField qui affiche le nom de chaque fournisseur, et
- TemplateField qui contient un contrôle BulletedList qui est lié aux résultats retournés par la `GetProducts()` méthode pour chaque fournisseur.

Nous allons examiner comment afficher ces rapports maître / détail dans les didacticiels futures. Pour l’instant, cet exemple est conçu pour illustrer l’utilisation de la méthode personnalisée ajoutée à la `Northwind.SuppliersRow` classe.

SuppliersAndProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-vb/samples/sample24.aspx)]

SuppliersAndProducts.aspx.vb

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample25.vb)]


[![Nom de la société du fournisseur est répertorié dans la colonne de gauche, leurs produits dans la droite](creating-a-data-access-layer-vb/_static/image92.png)](creating-a-data-access-layer-vb/_static/image91.png)

**Figure 35**: nom de la société du fournisseur est répertorié dans la colonne de gauche, leurs produits dans la droite ([cliquez pour afficher l’image en taille réelle](creating-a-data-access-layer-vb/_static/image93.png))


## <a name="summary"></a>Récapitulatif

Lorsque la création d’une application web de création de la DAL doit être une des premières étapes à suivre avant de commencer à créer votre couche de présentation. Avec Visual Studio, la création d’une DAL basée sur des DataSet typés est une tâche qui peut être accomplie en 10 à 15 minutes sans écrire une ligne de code. Les didacticiels en progressant seront appuie sur cette couche DAL. Dans le [didacticiel suivant](creating-a-business-logic-layer-vb.md) nous allons définir un nombre de règles d’entreprise et voir comment les implémenter dans une couche de logique métier distincts.

Bonne programmation !

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Création d’une DAL à l’aide de fortement typée des TableAdapters et les tables de données dans Visual Studio 2005 et ASP.NET 2.0](https://weblogs.asp.net/scottgu/435498)
- [Conception des composants de la couche données et passer des données via des niveaux](https://msdn.microsoft.com/library/ms978496.aspx)
- [Créer une couche d’accès aux données avec le Concepteur de DataSet Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [Chiffrement des informations de Configuration dans ASP.NET 2.0 Applications](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Vue d’ensemble de TableAdapter](https://msdn.microsoft.com/library/bz9tthwx.aspx)
- [Utilisation d’un DataSet typé](https://msdn.microsoft.com/library/esbykkzb.aspx)
- [Utilisation de l’accès des données fortement typées dans Visual Studio 2005 et ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [Comment étendre les méthodes TableAdapter](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [Récupération de données scalaire à partir d’une procédure stockée](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Vidéos de formation sur les rubriques contenues dans ce didacticiel

- [Couches d’accès aux données dans les applications ASP.NET](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [Comment lier manuellement un jeu de données à un contrôle Datagrid](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [Utilisation des jeux de données et les filtres à partir d’une Application ASP](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Ron Green, Hilton Giesenow, Dennis Patterson, Liz Shulok, Abel Gomez et Carlos Santos. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](master-pages-and-site-navigation-cs.md)
> [Suivant](creating-a-business-logic-layer-vb.md)
