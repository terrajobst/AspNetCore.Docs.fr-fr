---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
title: À l’aide de dépendances de Cache SQL (VB) | Microsoft Docs
author: rick-anderson
description: La stratégie de mise en cache la plus simple consiste à autoriser les données mises en cache expirent après une période spécifiée. Mais cette approche simple signifie que le données mises en cache maintai...
ms.author: aspnetcontent
ms.date: 05/30/2007
ms.assetid: bd347d93-4251-4532-801c-a36f2dfa7f96
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
msc.type: authoredcontent
ms.openlocfilehash: 7929966d9eb82994b9d427d0c3fd7f08c41212fc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821643"
---
<a name="using-sql-cache-dependencies-vb"></a>À l’aide de dépendances de Cache SQL (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_VB.zip) ou [télécharger le PDF](using-sql-cache-dependencies-vb/_static/datatutorial61vb1.pdf)

> La stratégie de mise en cache la plus simple consiste à autoriser les données mises en cache expirent après une période spécifiée. Mais cette approche simple signifie que les données mises en cache ne conservent aucune association avec sa source de données sous-jacente, ce qui entraîne des données obsolètes qui sont maintenues trop longues ou actuel qui est arrivé à expiration trop tôt. Une meilleure approche consiste à utiliser la classe SqlCacheDependency afin que les données restent en mémoire cache jusqu'à ce que ses données sous-jacentes a été modifiées dans la base de données SQL. Ce didacticiel vous montre comment.


## <a name="introduction"></a>Introduction

Les techniques de mise en cache est examiné dans le [la mise en cache des données avec ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) et [la mise en cache des données dans l’Architecture](caching-data-in-the-architecture-vb.md) didacticiels utilisé une expiration temporelle pour supprimer les données à partir du cache après une certaine période. Cette approche est la façon la plus simple pour équilibrer les gains de performance de mise en cache par rapport à l’obsolescence des données. En sélectionnant une expiration du temps de *x* secondes, un développeur de pages concedes pour profiter des avantages de performances de mise en cache uniquement pour *x* secondes, mais tranquille que ses données ne seront jamais à obsolètes plus longtemps que le maximum de *x* secondes. Bien entendu, pour les données statiques, *x* peut être étendu à la durée de vie de l’application web, comme l’a été examinée dans le [la mise en cache des données au démarrage de l’Application](caching-data-at-application-startup-vb.md) didacticiel.

Lors de la mise en cache de la base de données, une expiration temporelle est souvent choisie pour sa simplicité d’utilisation, mais est souvent une solution inadéquate. Dans l’idéal, la base de données reste en mémoire cache jusqu'à ce que les données sous-jacentes a été modifiées dans la base de données ; ensuite seulement, serait le cache de suppression. Cette approche optimise les avantages de performances de la mise en cache et réduit la durée des données périmées. Toutefois, pour bénéficier de ces avantages il doit être un système en place qui sait quand la base de données sous-jacente a été modifiée et exclure les éléments correspondants à partir du cache. Avant ASP.NET 2.0, les développeurs de pages étaient responsables de l’implémentation de ce système.

ASP.NET 2.0 fournit un [ `SqlCacheDependency` classe](https://msdn.microsoft.com/library/system.web.caching.sqlcachedependency.aspx) et l’infrastructure nécessaire pour déterminer quand une modification s’est produite dans la base de données afin que les éléments correspondants mis en cache peut être supprimée. Il existe deux techniques pour déterminer quand les données sous-jacentes a changé : notification et interrogation. Après avoir discuté les différences entre la notification et d’interrogation, nous allons créer l’infrastructure nécessaire pour prendre en charge l’interrogation et ensuite découvrir comment utiliser le `SqlCacheDependency` classe déclarative et par programme des scénarios.

## <a name="understanding-notification-and-polling"></a>L’interrogation et la Notification de présentation

Il existe deux techniques qui peuvent être utilisées pour déterminer quand les données dans une base de données a été modifiées : notification et interrogation. Avec notification, la base de données avertit automatiquement le runtime ASP.NET lorsque les résultats d’une requête particulière ont été modifiés depuis la dernière exécution de la requête, à quel point les éléments mis en cache associés à la requête sont supprimées. Avec l’interrogation, le serveur de base de données conserve les informations relatives lorsque des tables dernier mis à jour. Le runtime ASP.NET interroge régulièrement la base de données pour vérifier que les tables ont été modifiés dans la mesure où ils ont été entrés dans le cache. Ces tables dont les données ont été modifiées ont leurs éléments de cache associée supprimés.

L’option de notification requiert moins d’installation que l’interrogation et est plus granulaire dans la mesure où il suit les modifications apportées au niveau de la requête plutôt qu’au niveau de la table. Malheureusement, les notifications sont uniquement disponibles dans les éditions complètes de Microsoft SQL Server 2005 (par exemple, les éditions non Express). Toutefois, l’option d’interrogation peut être utilisée pour toutes les versions de Microsoft SQL Server à partir de 7.0 à 2005. Dans la mesure où ces didacticiels utilisent l’édition Express de SQL Server 2005, nous allons nous concentrer sur la configuration et à l’aide de l’option d’interrogation. Consultez la section de lecture supplémentaire à la fin de ce didacticiel pour davantage de ressources sur les fonctionnalités de notification de SQL Server 2005 s.

Avec l’interrogation, la base de données doit être configuré pour inclure une table nommée `AspNet_SqlCacheTablesForChangeNotification` qui a trois colonnes - `tableName`, `notificationCreated`, et `changeId`. Cette table contient une ligne pour chaque table qui comporte des données qui doivent être utilisées dans une dépendance de cache SQL dans l’application web. Le `tableName` colonne spécifie le nom de la table lors de la `notificationCreated` indique la date et l’heure de la ligne a été ajoutée à la table. Le `changeId` colonne est de type `int` et a une valeur initiale de 0. Sa valeur est incrémentée à chaque modification de la table.

Outre le `AspNet_SqlCacheTablesForChangeNotification` table, la base de données doit également inclure des déclencheurs sur chacune des tables qui peuvent apparaître dans une dépendance de cache SQL. Ces déclencheurs sont exécutées chaque fois qu’une ligne est insérée, mise à jour ou supprimée et incrémenter la table s `changeId` valeur dans `AspNet_SqlCacheTablesForChangeNotification`.

Le runtime ASP.NET effectue le suivi des cours `changeId` pour une table lors de la mise en cache de données à l’aide un `SqlCacheDependency` objet. La base de données est régulièrement vérifiée et n’importe quel `SqlCacheDependency` objets dont la propriété `changeId` diffère de la valeur dans la base de données sont supprimées depuis une qui se différencie `changeId` valeur indique qu’il a été une modification apportée à la table, dans la mesure où les données a été mis en cache.

## <a name="step-1-exploring-theaspnetregsqlexecommand-line-program"></a>Étape 1 : Explorer le`aspnet_regsql.exe`programme de ligne de commande

Avec l’approche d’interrogation de la base de données doit être configuré pour contenir l’infrastructure décrite ci-dessus : une table prédéfinie (`AspNet_SqlCacheTablesForChangeNotification`), un certain nombre de procédures stockées et des déclencheurs sur chacune des tables qui peuvent être utilisées dans les dépendances de cache SQL dans le site web application. Ces tables, les procédures stockées et les déclencheurs peuvent être créées via le programme de ligne de commande `aspnet_regsql.exe`, qui se trouve dans le `$WINDOWS$\Microsoft.NET\Framework\version` dossier. Pour créer le `AspNet_SqlCacheTablesForChangeNotification` table et des procédures stockées associées, exécutez la commande suivante à partir de la ligne de commande :


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample1.cmd)]

> [!NOTE]
> Pour exécuter ces commandes de la connexion de base de données spécifiée doit être dans le [ `db_securityadmin` ](https://msdn.microsoft.com/library/ms188685.aspx) et [ `db_ddladmin` ](https://msdn.microsoft.com/library/ms190667.aspx) rôles. Pour examiner le T-SQL envoyé à la base de données par le `aspnet_regsql.exe` programme de ligne de commande, reportez-vous à [cette entrée de blog](http://scottonwriting.net/sowblog/posts/10709.aspx).


Par exemple, pour ajouter l’infrastructure pour l’interrogation à une base de données Microsoft SQL Server nommé `pubs` sur un serveur de base de données nommé `ScottsServer` à l’aide de l’authentification Windows, accédez au répertoire approprié et, à partir de la ligne de commande, entrez :


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample2.cmd)]

Une fois que l’infrastructure de niveau de base de données a été ajoutée, nous devons ajouter les déclencheurs aux tables qui seront utilisées dans les dépendances de cache SQL. Utiliser le `aspnet_regsql.exe` ligne de commande du programme à nouveau, mais spécifiez le nom de table à l’aide de la `-t` basculer et au lieu d’utiliser le `-ed` basculer utilisation `-et`, comme suit :


[!code-html[Main](using-sql-cache-dependencies-vb/samples/sample3.html)]

Pour ajouter les déclencheurs à la `authors` et `titles` tables sur le `pubs` sur la base de données `ScottsServer`, utilisez :


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample4.cmd)]

Pour ce didacticiel, ajouter les déclencheurs à la `Products`, `Categories`, et `Suppliers` tables. Nous allons examiner la syntaxe de ligne de commande particulière à l’étape 3.

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inappdata"></a>Étape 2 : Faisant référence à un Microsoft SQL Server 2005 Express Edition de base de données dans`App_Data`

Le `aspnet_regsql.exe` programme de ligne de commande nécessite le nom du serveur et de base de données afin d’ajouter l’infrastructure nécessaire d’interrogation. Mais quel est le nom du serveur et de base de données pour une base de données Microsoft SQL Server 2005 Express qui réside dans le `App_Data` dossier ? Au lieu de devoir découvrir quelles sont les noms de base de données et serveur, je ve trouvé que l’approche la plus simple consiste à attacher la base de données pour le `localhost\SQLExpress` instance de base de données et de renommer les données à l’aide [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx). Si vous avez une des versions complètes de SQL Server 2005 est installé sur votre ordinateur, puis vous avez déjà installé sur votre ordinateur SQL Server Management Studio. Si vous avez uniquement l’édition Express, vous pouvez télécharger la version gratuite [Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Commencez par fermer Visual Studio. Ensuite, ouvrez SQL Server Management Studio et choisissez se connecter à la `localhost\SQLExpress` serveur à l’aide de l’authentification Windows.


![Attacher à la localhost\SQLExpress Server](using-sql-cache-dependencies-vb/_static/image1.gif)

**Figure 1**: attacher à la `localhost\SQLExpress` Server


Une fois connecté au serveur, Management Studio affiche le serveur et contenir des sous-dossiers pour les bases de données, sécurité et ainsi de suite. Avec le bouton droit sur le dossier bases de données et choisissez l’option d’attachement. Cela fera apparaître la boîte de dialogue Attacher les bases de données boîte (voir Figure 2). Cliquez sur le bouton Ajouter, puis sélectionnez le `NORTHWND.MDF` dossier de base de données dans votre s d’application web `App_Data` dossier.


[![Attacher le fichier NORTHWND. Fichiers MDF de base de données à partir du dossier App_Data](using-sql-cache-dependencies-vb/_static/image2.gif)](using-sql-cache-dependencies-vb/_static/image1.png)

**Figure 2**: attacher le `NORTHWND.MDF` de base de données à partir de la `App_Data` dossier ([cliquez pour afficher l’image en taille réelle](using-sql-cache-dependencies-vb/_static/image2.png))


Cette opération ajoute la base de données dans le dossier de bases de données. Le nom de la base de données peut être le chemin d’accès complet au fichier de base de données ou le chemin d’accès complet avec préfixe un [GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier). Pour éviter d’avoir à taper ce nom long de la base de données lorsque vous utilisez le compte aspnet\_outil de ligne de commande regsql.exe, de renommer la base de données à un nom plus convivial en cliquant simplement sur la base de données attaché et en choisissant de renommer. Je ve renommé DataTutorials ma base de données.


![Renommer la base de données attachée à un nom plus convivial](using-sql-cache-dependencies-vb/_static/image3.gif)

**Figure 3**: renommer la base de données attachée à un nom plus convivial


## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>Étape 3 : Ajout de l’Infrastructure d’interrogation à la base de données Northwind

Maintenant que nous avons joint le `NORTHWND.MDF` de base de données à partir de la `App_Data` dossier, nous vous êtes prêt à ajouter l’infrastructure d’interrogation. En supposant que vous avez déjà renommé la base de données DataTutorials, exécutez les quatre commandes suivantes :


[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample5.cmd)]

Après avoir exécuté ces quatre commandes, avec le bouton droit sur le nom de la base de données dans Management Studio, accédez au sous-menu tâches et choisissez de détachement. Ensuite, fermez Management Studio et rouvrez Visual Studio.

Une fois que Visual Studio a rouvert, explorez la base de données via l’Explorateur de serveurs. Notez la nouvelle table (`AspNet_SqlCacheTablesForChangeNotification`), les nouvelles procédures stockées et les déclencheurs sur la `Products`, `Categories`, et `Suppliers` tables.


![La base de données inclut désormais l’Infrastructure nécessaire d’interrogation](using-sql-cache-dependencies-vb/_static/image4.gif)

**Figure 4**: la base de données inclut désormais l’Infrastructure nécessaire d’interrogation


## <a name="step-4-configuring-the-polling-service"></a>Étape 4 : Configuration du Service d’interrogation

Après avoir créé les tables nécessaires, les déclencheurs et les procédures stockées dans la base de données, l’étape finale consiste à configurer le service d’interrogation, ce qui est effectué via `Web.config` en spécifiant les bases de données à utiliser et la fréquence d’interrogation en millisecondes. Le balisage suivant interroge la base de données Northwind qu’une seule fois chaque seconde.


[!code-xml[Main](using-sql-cache-dependencies-vb/samples/sample6.xml)]

Le `name` valeur dans le `<add>` élément (NorthwindDB) associe un nom explicite à une base de données. Lorsque vous travaillez avec des dépendances de cache SQL, nous allons devoir faire référence au nom de la base de données défini ici, ainsi que la table basée sur les données en cache. Nous verrons comment utiliser le `SqlCacheDependency` classe à associer par programme les dépendances de cache SQL avec mise en cache des données à l’étape 6.

Une fois qu’une dépendance de cache SQL a été établie, le système d’interrogation se connectera aux bases de données définies dans le `<databases>` éléments chaque `pollTime` millisecondes et exécuter le `AspNet_SqlCachePollingStoredProcedure` procédure stockée. Cette procédure stockée - qui a été ajoutée de nouveau à l’aide de l’étape 3 la `aspnet_regsql.exe` outil de ligne de commande - renvoie le `tableName` et `changeId` valeurs pour chaque enregistrement dans `AspNet_SqlCacheTablesForChangeNotification`. Les dépendances de cache SQL obsolètes sont supprimés du cache.

Le `pollTime` paramètre introduit un compromis entre performances et d’obsolescence des données. Un petit `pollTime` valeur augmente le nombre de demandes à la base de données, mais plus rapidement évince des données obsolètes à partir du cache. Une valeur plus élevée `pollTime` valeur réduit le nombre de demandes de base de données, mais augmente le délai entre la modification des données back-end et lorsque les éléments connexes du cache sont supprimées. Heureusement, la requête de base de données s’exécute une procédure stockée simple s retournant uniquement quelques lignes d’une table simple et léger. Mais faire des essais avec différents `pollTime` pour trouver un équilibre idéal entre les valeurs de base de données access et les données de l’obsolescence pour votre application. Le plus petit `pollTime` valeur autorisée est de 500.

> [!NOTE]
> L’exemple ci-dessus fournit un seul `pollTime` valeur dans le `<sqlCacheDependency>` élément, mais vous pouvez éventuellement spécifier le `pollTime` valeur dans le `<add>` élément. Cela est utile si vous disposez de plusieurs bases de données spécifiés et que vous souhaitez personnaliser la fréquence d’interrogation par base de données.


## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>Étape 5 : Utilisation de façon déclarative les dépendances de Cache SQL

Dans les étapes 1 à 4, nous avons vu comment configurer l’infrastructure de base de données nécessaires et de configurer le système d’interrogation. Avec cette infrastructure en place, nous pouvons maintenant ajouter des éléments au cache de données avec une dépendance de cache SQL associée à l’aide de techniques de programmation ou déclaratives. Dans cette étape, nous allons examiner l’utilisation de façon déclarative des dépendances de cache SQL. À l’étape 6, nous allons examiner l’approche par programmation.

Le [la mise en cache des données avec ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) didacticiel exploré les fonctionnalités de mise en cache déclaratives de l’ObjectDataSource. En définissant simplement le `EnableCaching` propriété `True` et le `CacheDuration` propriété à un intervalle de temps, ObjectDataSource met automatiquement en cache les données retournées à partir de son objet sous-jacent pour l’intervalle spécifié. L’ObjectDataSource permettre également utiliser une ou plusieurs dépendances de cache SQL.

Pour illustrer l’utilisation déclarative de dépendances de cache SQL, ouvrez le `SqlCacheDependencies.aspx` page dans le `Caching` dossier et faites glisser un GridView à partir de la boîte à outils vers le concepteur. Définir les opérations de mappage GridView `ID` à `ProductsDeclarative` et, à partir de sa balise active, choisir de lier à un nouveau ObjectDataSource nommé `ProductsDataSourceDeclarative`.


[![Créer un nouveau ObjectDataSource nommé ProductsDataSourceDeclarative](using-sql-cache-dependencies-vb/_static/image5.gif)](using-sql-cache-dependencies-vb/_static/image3.png)

**Figure 5**: créer une nouvelle nommée de ObjectDataSource `ProductsDataSourceDeclarative` ([cliquez pour afficher l’image en taille réelle](using-sql-cache-dependencies-vb/_static/image4.png))


Configurer l’ObjectDataSource à utiliser le `ProductsBLL` classe et définissez la liste déroulante dans l’onglet Sélection à `GetProducts()`. Dans l’onglet mise à jour, choisissez le `UpdateProduct` surcharge avec trois paramètres d’entrée - `productName`, `unitPrice`, et `productID`. Définir les listes déroulantes (None) dans les onglets INSERT et DELETE.


[![Utilisez la surcharge de UpdateProduct avec trois paramètres d’entrée](using-sql-cache-dependencies-vb/_static/image6.gif)](using-sql-cache-dependencies-vb/_static/image5.png)

**Figure 6**: utiliser la surcharge de UpdateProduct avec trois paramètres d’entrée ([cliquez pour afficher l’image en taille réelle](using-sql-cache-dependencies-vb/_static/image6.png))


[![Définition de la liste déroulante (None) pour l’insertion et supprimer les tabulations](using-sql-cache-dependencies-vb/_static/image7.gif)](using-sql-cache-dependencies-vb/_static/image7.png)

**Figure 7**: définir la liste déroulante (None) pour l’insérer et supprimer des onglets ([cliquez pour afficher l’image en taille réelle](using-sql-cache-dependencies-vb/_static/image8.png))


À l’issue de l’Assistant Configurer la Source de données, Visual Studio créera BoundFields et CheckBoxFields dans le contrôle GridView pour chacun des champs de données. Supprimer tous les champs mais `ProductName`, `CategoryName`, et `UnitPrice`et mettre en forme ces champs comme vous le souhaitez. Dans la balise active de s GridView, cochez les cases à cocher Activer la pagination, activer le tri et activer la modification. Visual Studio définira le s ObjectDataSource `OldValuesParameterFormatString` propriété `original_{0}`. Dans l’ordre pour la fonctionnalité de modification GridView s fonctionne correctement, supprimez cette propriété entièrement à partir de la syntaxe déclarative ou réaffectez-lui la valeur par défaut, `{0}`.

Enfin, ajoutez un contrôle Web Label au-dessus de la GridView ensemble son `ID` propriété `ODSEvents` et son `EnableViewState` propriété `False`. Après avoir apporté ces modifications, votre balisage déclaratif s de page doit ressembler à ce qui suit. Notez que je ve apporté plusieurs personnalisations esthétiques aux champs GridView qui ne sont pas nécessaires pour illustrer la fonctionnalité de dépendance de cache SQL.


[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample7.aspx)]

Ensuite, créez un gestionnaire d’événements pour les opérations de mappage ObjectDataSource `Selecting` événements et dans le code suivant :


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample8.vb)]

N’oubliez pas que les opérations de mappage ObjectDataSource `Selecting` événement est déclenché uniquement lorsque vous récupérez des données à partir de son objet sous-jacent. Si l’ObjectDataSource accède aux données à partir de son propre cache, cet événement n’est pas déclenché.

Maintenant, visitez cette page via un navigateur. Dans la mesure où ve encore à implémenter toute la mise en cache, chaque fois que vous page, triez ou modifiez la grille de la page doit afficher le texte, l’événement de sélection de l’option déclenché, comme le montre la Figure 8.


[![Le s ObjectDataSource événement Selecting déclenche chaque fois que le contrôle GridView est par radiomessagerie, modifié, ou les trié](using-sql-cache-dependencies-vb/_static/image8.gif)](using-sql-cache-dependencies-vb/_static/image9.png)

**Figure 8**: The ObjectDataSource s `Selecting` événement se déclenche à chaque fois le contrôle GridView est paginé, modifiée ou Sorted ([cliquez pour afficher l’image en taille réelle](using-sql-cache-dependencies-vb/_static/image10.png))


Comme nous l’avons vu dans la [la mise en cache des données avec ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) (didacticiel), définissant le `EnableCaching` propriété `True` provoque l’ObjectDataSource pour mettre en cache ses données pour la durée spécifiée par son `CacheDuration` propriété. ObjectDataSource a également un [ `SqlCacheDependency` propriété](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx), qui ajoute une ou plusieurs dépendances de cache SQL pour les données mises en cache à l’aide du modèle :


[!code-css[Main](using-sql-cache-dependencies-vb/samples/sample9.css)]

Où *databaseName* est le nom de la base de données comme spécifié dans le `name` attribut de la `<add>` élément `Web.config`, et *tableName* est le nom de la table de base de données. Par exemple, pour créer un ObjectDataSource qui met en cache indéfiniment les données selon une dépendance de cache SQL sur les opérations de mappage Northwind `Products` table, définissez le s ObjectDataSource `EnableCaching` propriété `True` et son `SqlCacheDependency` propriété NorthwindDB:Products.

> [!NOTE]
> Vous pouvez utiliser une dépendance de cache SQL *et* une expiration temporelle en définissant `EnableCaching` à `True`, `CacheDuration` à l’intervalle de temps, et `SqlCacheDependency` aux noms de base de données et de table. ObjectDataSource supprimez ses données lors de l’expiration temporelle est atteinte ou lorsque le système d’interrogation de commentaires que la base de données sous-jacente a changé, selon ce qui se produit en premier.


Le contrôle GridView dans `SqlCacheDependencies.aspx` affiche les données des deux tables - `Products` et `Categories` (le produit s `CategoryName` champ est récupéré un `JOIN` sur `Categories`). Par conséquent, nous voulons spécifier deux dépendances de cache SQL : NorthwindDB:Products ; NorthwindDB:Categories.


[![Configurer l’ObjectDataSource pour prendre en charge la mise en cache à l’aide de dépendances de Cache SQL sur les produits et les catégories](using-sql-cache-dependencies-vb/_static/image9.gif)](using-sql-cache-dependencies-vb/_static/image11.png)

**Figure 9**: configurer l’ObjectDataSource pour la prise en charge la mise en cache à l’aide de dépendances de Cache SQL `Products` et `Categories` ([cliquez pour afficher l’image en taille réelle](using-sql-cache-dependencies-vb/_static/image12.png))


Après avoir configuré l’ObjectDataSource pour prendre en charge la mise en cache, visitez la page via un navigateur. Là encore, l’événement de sélection de texte déclenché doit apparaître sur la première visite de page, mais devrait disparaître lorsque la pagination, de tri ou en cliquant sur les boutons de modification ou sur Annuler. Il s’agit, car une fois que les données sont chargées dans le cache de s ObjectDataSource, elle y reste jusqu'à ce que le `Products` ou `Categories` les tables sont modifiées ou les données sont mis à jour via le contrôle GridView.

Après le déclenchement de la pagination dans la grille et noter l’absence de l’événement de sélection de l’option texte, ouvrez une nouvelle fenêtre de navigateur et accédez au didacticiel principes de base dans l’édition, insertion et suppression de section (`~/EditInsertDelete/Basics.aspx`). Mettre à jour le nom ou le prix d’un produit. À partir de la première fenêtre de navigateur, affichez ensuite une autre page de données, trier la grille ou cliquez sur un bouton de modification de ligne s. Cette fois-ci, l’événement de sélection de l’option déclenché doit réapparaître, car la base de données sous-jacente données a été modifié (voir Figure 10). Si le texte n’apparaît pas, attendez quelques instants et réessayez. N’oubliez pas que le service d’interrogation vérifie les modifications apportées à la `Products` table chaque `pollTime` millisecondes, donc il existe un délai entre lorsque les données sous-jacentes sont mis à jour et lorsque les données mises en cache soit supprimées.


[![Modification de la Table de produits d’exclure les données de produit mis en cache](using-sql-cache-dependencies-vb/_static/image10.gif)](using-sql-cache-dependencies-vb/_static/image13.png)

**Figure 10**: modification de la Table de produits d’exclure les données mises en cache de produit ([cliquez pour afficher l’image en taille réelle](using-sql-cache-dependencies-vb/_static/image14.png))


## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>Étape 6 : Utilisation par programmation de la`SqlCacheDependency`classe

Le [la mise en cache des données dans l’Architecture](caching-data-in-the-architecture-vb.md) didacticiel examiné les avantages de l’utilisation d’une couche distincte de la mise en cache dans l’architecture par opposition à associant étroitement à la mise en cache avec ObjectDataSource. Dans ce didacticiel, nous avons créé un `ProductsCL` classe afin d’illustrer l’utilisation par programmation le cache de données. Pour utiliser les dépendances de cache SQL dans la couche de mise en cache, utilisez la `SqlCacheDependency` classe.

Avec le système d’interrogation, un `SqlCacheDependency` objet doit être associé à une paire de base de données et de table particulier. Le code suivant, par exemple, crée un `SqlCacheDependency` objet basé sur la base de données Northwind s `Products` table :


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample10.vb)]

Les deux paramètres d’entrée de la `SqlCacheDependency` constructeur de s sont les noms de base de données et de table, respectivement. Comme avec les opérations de mappage ObjectDataSource `SqlCacheDependency` propriété, le nom de base de données utilisé est identique à la valeur spécifiée dans le `name` attribut de la `<add>` élément `Web.config`. Le nom de table est le nom réel de la table de base de données.

Pour associer un `SqlCacheDependency` avec un élément ajouté au cache de données, utilisez une de la `Insert` surcharges de méthode qui accepte une dépendance. Le code suivant ajoute *valeur* au cache de données pour une durée indéterminée, mais associe un `SqlCacheDependency` sur la `Products` table. En bref, *valeur* resteront dans le cache jusqu'à ce qu’il soit supprimé en raison des contraintes de mémoire ou parce que le système d’interrogation a détecté que le `Products` table a été modifiée dans la mesure où il a été mis en cache.


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample11.vb)]

La couche de mise en cache s `ProductsCL` classe met actuellement en cache les données à partir de la `Products` table à l’aide d’une expiration temporelle de 60 secondes. Permettent de mettre à jour de cette classe afin qu’il utilise à la place des dépendances de cache SQL s. Le `ProductsCL` classe s `AddCacheItem` (méthode), qui est chargé d’ajouter les données dans le cache, contient actuellement le code suivant :


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample12.vb)]

Mettre à jour de ce code pour utiliser un `SqlCacheDependency` de l’objet au lieu du `MasterCacheKeyArray` la dépendance de cache :


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample13.vb)]

Pour tester cette fonctionnalité, ajoutez un GridView à la page sous existant `ProductsDeclarative` GridView. Définir cette nouvelle s GridView `ID` à `ProductsProgrammatic` et via sa balise active, liez-le à une nouvelle ObjectDataSource nommé `ProductsDataSourceProgrammatic`. Configurer l’ObjectDataSource à utiliser le `ProductsCL` (classe), définissant les listes déroulantes dans l’instruction SELECT et les onglets de mise à jour à `GetProducts` et `UpdateProduct`, respectivement.


[![Configurer pour utiliser la classe ProductsCL ObjectDataSource](using-sql-cache-dependencies-vb/_static/image11.gif)](using-sql-cache-dependencies-vb/_static/image15.png)

**Figure 11**: configurer ObjectDataSource à utiliser le `ProductsCL` classe ([cliquez pour afficher l’image en taille réelle](using-sql-cache-dependencies-vb/_static/image16.png))


[![Sélectionnez la méthode GetProducts dans la liste déroulante de s onglet Sélection](using-sql-cache-dependencies-vb/_static/image12.gif)](using-sql-cache-dependencies-vb/_static/image17.png)

**Figure 12**: sélectionnez le `GetProducts` méthode à partir de la liste déroulante de s Sélectionnez un onglet ([cliquez pour afficher l’image en taille réelle](using-sql-cache-dependencies-vb/_static/image18.png))


[![Choisissez la méthode UpdateProduct dans la liste déroulante de mise à jour onglet s](using-sql-cache-dependencies-vb/_static/image13.gif)](using-sql-cache-dependencies-vb/_static/image19.png)

**Figure 13**: choisissez la méthode UpdateProduct à partir de la liste déroulante de s onglet de mise à jour ([cliquez pour afficher l’image en taille réelle](using-sql-cache-dependencies-vb/_static/image20.png))


À l’issue de l’Assistant Configurer la Source de données, Visual Studio créera BoundFields et CheckBoxFields dans le contrôle GridView pour chacun des champs de données. Comme avec la première GridView ajouté à cette page, supprimez tous les champs mais `ProductName`, `CategoryName`, et `UnitPrice`et mettre en forme ces champs comme vous le souhaitez. Dans la balise active de s GridView, cochez les cases à cocher Activer la pagination, activer le tri et activer la modification. Comme avec la `ProductsDataSourceDeclarative` ObjectDataSource, Visual Studio définira le `ProductsDataSourceProgrammatic` ObjectDataSource s `OldValuesParameterFormatString` propriété `original_{0}`. Dans l’ordre pour la fonctionnalité de modification GridView s fonctionne correctement, définissez cette propriété retour au `{0}` (ou supprimer complètement de l’assignation de propriété à partir de la syntaxe déclarative).

Après avoir effectué ces tâches, le balisage déclaratif GridView et ObjectDataSource résultant doit ressembler à ce qui suit :


[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample14.aspx)]

Pour tester le code SQL dépendance de cache dans la couche de mise en cache définie un point d’arrêt dans le `ProductCL` classe s `AddCacheItem` méthode et puis démarrer le débogage. Lorsque vous visitez `SqlCacheDependencies.aspx`, le point d’arrêt doit être atteint pour que les données sont demandées pour la première fois et placées dans le cache. Ensuite, déplacez vers une autre page dans le contrôle GridView ou l’une des colonnes de tri. Cela entraîne le contrôle GridView à actualiser ses données, mais les données doivent être trouvées dans le cache depuis le `Products` table de base de données n’a pas été modifié. Si les données sont à plusieurs reprises introuvable dans le cache, assurez-vous que suffisamment de mémoire est disponible sur votre ordinateur et réessayez.

Après la pagination par le biais de quelques pages du contrôle GridView, ouvrez une deuxième fenêtre de navigateur et accédez au didacticiel principes de base dans l’édition, insertion et suppression de section (`~/EditInsertDelete/Basics.aspx`). Mettre à jour un enregistrement de la table Products et puis, à partir de la première fenêtre de navigateur, afficher une nouvelle page ou cliquez sur l’un des en-têtes de tri.

Dans ce scénario vous verrez deux choses : le point d’arrêt est atteint, indiquant que les données mises en cache a été supprimées en raison de la modification dans la base de données ; ou, le point d’arrêt n’est pas atteints, ce qui signifie que `SqlCacheDependencies.aspx` affiche maintenant des données périmées. Si le point d’arrêt n’est pas atteint, il est probable, car le service d’interrogation n’a pas encore déclenché dans la mesure où les données ont été modifiées. N’oubliez pas que le service d’interrogation vérifie les modifications apportées à la `Products` table chaque `pollTime` millisecondes, donc il existe un délai entre lorsque les données sous-jacentes sont mis à jour et lorsque les données mises en cache soit supprimées.

> [!NOTE]
> Ce délai est plus susceptible d’apparaître lors de la modification d’un de ces produits via le contrôle GridView dans `SqlCacheDependencies.aspx`. Dans le [la mise en cache des données dans l’Architecture](caching-data-in-the-architecture-vb.md) didacticiel, nous avons ajouté la `MasterCacheKeyArray` dépendance pour vous assurer que les données en cours de modification par le biais de cache la `ProductsCL` classe s `UpdateProduct` méthode a été supprimée du cache. Toutefois, nous avons remplacé cette dépendance de cache lorsque vous modifiez le `AddCacheItem` méthode plus haut dans cette étape et par conséquent le `ProductsCL` classe continue d’afficher les données mises en cache jusqu'à ce que le système de d’interrogation indique la modification apportée à la `Products` table. Nous verrons comment réintroduire le `MasterCacheKeyArray` la dépendance à l’étape 7 de cache.


## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>Étape 7 : Association de plusieurs dépendances avec un élément mis en cache

N’oubliez pas que le `MasterCacheKeyArray` dépendance de cache est utilisée pour vous assurer que *toutes les* données relatives au produit sont supprimées du cache lorsque la mise à jour de n’importe quel élément unique associé qu’il contient. Par exemple, le `GetProductsByCategoryID(categoryID)` méthode caches `ProductsDataTables` instances pour chaque unique *categoryID* valeur. Si un de ces objets est supprimé, le `MasterCacheKeyArray` dépendance de cache permet de s’assurer que les autres sont également supprimés. Sans cette dépendance de cache, quand les données mises en cache sont modifiées. il est possible que d’autres données de produit mis en cache est peut-être obsolètes. Par conséquent, il s important que nous gérons la `MasterCacheKeyArray` dépendance de cache lors de l’utilisation de dépendances de cache SQL. Toutefois, les données en cache s `Insert` méthode permet uniquement d’un objet de dépendance unique.

En outre, lorsque vous travaillez avec des dépendances de cache SQL nous devons associer plusieurs tables de base de données en tant que dépendances. Par exemple, le `ProductsDataTable` mis en cache dans le `ProductsCL` classe contient les noms de catégorie et le fournisseur pour chaque produit, mais le `AddCacheItem` méthode utilise uniquement une dépendance sur `Products`. Dans ce cas, si l’utilisateur met à jour le nom d’une catégorie ou le fournisseur, les données de produit mis en cache resteront dans le cache et être obsolètes. Par conséquent, nous voulons que les données de produit mis en cache dépend non seulement le `Products` de table, mais sur le `Categories` et `Suppliers` également à des tables.

Le [ `AggregateCacheDependency` classe](https://msdn.microsoft.com/library/system.web.caching.aggregatecachedependency.aspx) fournit un moyen d’associer plusieurs dépendances avec un élément de cache. Commencez par créer un `AggregateCacheDependency` instance. Ensuite, ajoutez l’ensemble de dépendances à l’aide de la `AggregateCacheDependency` s `Add` (méthode). Quand vous insérez l’élément dans le cache de données par la suite, transmettez le `AggregateCacheDependency` instance. Lorsque *n’importe quel* de la `AggregateCacheDependency` modifier les dépendances de l’instance s, de suppression de l’élément mis en cache.

L’exemple suivant montre le code mis à jour pour le `ProductsCL` classe s `AddCacheItem` (méthode). La méthode crée le `MasterCacheKeyArray` cache dépendance avec `SqlCacheDependency` des objets pour le `Products`, `Categories`, et `Suppliers` tables. Ils sont combinés en une seule `AggregateCacheDependency` objet nommé `aggregateDependencies`, qui est ensuite passé à la `Insert` (méthode).


[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample15.vb)]

Tester ce nouveau code out. Désormais le `Products`, `Categories`, ou `Suppliers` tables provoquent l’éviction de données mises en cache. En outre, le `ProductsCL` classe s `UpdateProduct` (méthode), qui est appelée lors de la modification d’un produit via le contrôle GridView, exclure les `MasterCacheKeyArray` la dépendance, ce qui entraîne la mise en cache de cache `ProductsDataTable` éviction et les données à récupérer de nouveau sur Suivant demande.

> [!NOTE]
> Dépendances de cache SQL peuvent également être utilisées avec [la mise en cache de sortie](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx). Pour une démonstration de cette fonctionnalité, consultez : [à l’aide du cache de sortie ASP.NET avec SQL Server](https://msdn.microsoft.com/library/e3w8402y(VS.80).aspx).


## <a name="summary"></a>Récapitulatif

Lors de la mise en cache de la base de données, les données resteront dans l’idéal, dans le cache jusqu'à ce qu’il est modifié dans la base de données. Avec ASP.NET 2.0, les dépendances de cache SQL peuvent être créés et utilisés dans les scénarios déclaratives et par programme. L’une des difficultés avec cette approche est dans la découverte lorsque les données ont été modifiées. Les versions complètes de Microsoft SQL Server 2005 fournissent des fonctionnalités de notification qui peuvent signaler une application lorsqu’un résultat de la requête a été modifiée. Pour les Express Edition de SQL Server 2005 et les versions antérieures de SQL Server, un système d’interrogation doit être utilisé à la place. Heureusement, la configuration de l’infrastructure nécessaire d’interrogation est assez simple.

Bonne programmation !

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [À l’aide de Notifications de requête dans Microsoft SQL Server 2005](https://msdn.microsoft.com/library/ms175110.aspx)
- [Création d’une Notification de requête](https://msdn.microsoft.com/library/ms188669.aspx)
- [La mise en cache dans ASP.NET avec la `SqlCacheDependency` classe](https://msdn.microsoft.com/library/ms178604(VS.80).aspx)
- [ASP.NET SQL Server Registration Tool (`aspnet_regsql.exe`)](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)
- [Vue d’ensemble de `SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Marko Rangel Teresa Murphy et Hilton Giesenow. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](caching-data-at-application-startup-vb.md)
