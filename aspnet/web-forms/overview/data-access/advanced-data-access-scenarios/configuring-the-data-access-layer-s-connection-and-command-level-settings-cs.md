---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-cs
title: Configuration des paramètres de niveau connexion et commande de la couche d’accès aux données (c#) | Microsoft Docs
author: rick-anderson
description: Les TableAdapters dans un DataSet typé prend automatiquement en charge de la connexion à la base de données, émettre des commandes et remplir un DataTable avec les résultats...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: cd330dd9-6254-4305-9351-dd727384c83b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-cs
msc.type: authoredcontent
ms.openlocfilehash: 0b9b1f1341416f92ab1df4f338bbdbcd753fbae4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389623"
---
<a name="configuring-the-data-access-layers-connection--and-command-level-settings-c"></a>Configuration des paramètres de niveau connexion et commande de la couche d’accès aux données (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_CS.zip) ou [télécharger le PDF](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/datatutorial72cs1.pdf)

> Les TableAdapters dans un DataSet typé prend automatiquement en charge de la connexion à la base de données, émettre des commandes et remplir un DataTable avec les résultats. Il existe des occasions Toutefois, lorsque vous souhaitez prendre en charge de ces détails nous-mêmes, dans ce didacticiel vous apprendre comment accéder aux paramètres de base de données au niveau de la connexion et de la commande dans le TableAdapter.


## <a name="introduction"></a>Introduction

Tout au long de la série de didacticiels, nous avons utilisé des DataSet typés pour implémenter la couche d’accès aux données et les objets métier de notre architecture en couches. Comme indiqué dans le [premier didacticiel](../introduction/creating-a-data-access-layer-cs.md), les opérations de mappage DataSet typée DataTables servent de référentiels de données alors que les TableAdapters agir en tant que wrappers pour communiquer avec la base de données pour extraire et modifier les données sous-jacentes. Les TableAdapters encapsulent la complexité inhérente à l’utilisation de la base de données et nous évite d’avoir à écrire du code pour vous connecter à la base de données, émettre une commande ou remplir les résultats dans un DataTable.

Il est parfois, cependant, lorsque nous avons besoin pour les procédures dans les profondeurs du TableAdapter et écrire du code qui fonctionne directement avec les objets ADO.NET. Dans le [encapsulant les Modifications de base de données dans une Transaction](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) (didacticiel), par exemple, nous avons ajouté méthodes au TableAdapter de début, la validation et annulation de transactions d’ADO.NET. Ces méthodes utilisées en interne, créé manuellement `SqlTransaction` objet qui a été assigné au TableAdapter s `SqlCommand` objets.

Dans ce didacticiel, nous allons examiner comment accéder aux paramètres de base de données au niveau de la connexion et de la commande dans le TableAdapter. En particulier, nous allons ajouter des fonctionnalités à la `ProductsTableAdapter` qui permet l’accès à la chaîne de connexion sous-jacent et les paramètres de délai d’expiration de la commande.

## <a name="working-with-data-using-adonet"></a>Utilisation des données à l’aide d’ADO.NET

Microsoft .NET Framework intègre une pléthore de classes conçues spécifiquement pour fonctionner avec des données. Ces classes, figurant dans le [ `System.Data` espace de noms](https://msdn.microsoft.com/library/system.data.aspx), sont appelés les *ADO.NET* classes. Certaines des classes dans le cadre ADO.NET sont liées à un particulier *fournisseur de données*. Vous pouvez considérer un fournisseur de données comme un canal de communication qui permet des informations entre les classes ADO.NET et le magasin de données sous-jacent. Il existe des fournisseurs généralisées, telles qu’OLE DB et ODBC, ainsi que les fournisseurs qui sont spécialement conçues pour un système de base de données particulière. Par exemple, bien qu’il soit possible de se connecter à une base de données Microsoft SQL Server à l’aide du fournisseur OleDb, le fournisseur SqlClient est beaucoup plus efficace car elle a été conçu et optimisé spécifiquement pour SQL Server.

Lors de l’accès par programme aux données, le modèle suivant est couramment utilisé :

- Établir une connexion à la base de données.
- Émettre une commande.
- Pour `SELECT` requêtes, fonctionnent avec les enregistrements résultant.

Il existe des classes ADO.NET distincts pour exécuter chacune de ces étapes. Pour vous connecter à une base de données via le fournisseur SqlClient, par exemple, utiliser le [ `SqlConnection` classe](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(VS.80).aspx). Pour émettre un `INSERT`, `UPDATE`, `DELETE`, ou `SELECT` commande à la base de données, utilisez le [ `SqlCommand` classe](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx).

À l’exception de la [encapsulant les Modifications de base de données dans une Transaction](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) didacticiel, nous n’avons pas reçu à écrire le ADO.NET de bas niveau code nous-mêmes, car le code généré automatiquement TableAdapters inclut les fonctionnalités nécessaires pour se connecter à la base de données, émettre des commandes, récupérer des données et remplir ces données dans les tables de données. Toutefois, il peut arriver lorsque nous avons besoin de personnaliser ces paramètres de bas niveau. Sur les étapes suivantes, nous allons examiner comment exploiter les objets ADO.NET utilisés en interne par les TableAdapters.

## <a name="step-1-examining-with-the-connection-property"></a>Étape 1 : Examen avec la propriété de connexion

Chaque classe de TableAdapter a un `Connection` propriété qui spécifie les informations de connexion de base de données. Ce type de données de propriété s et `ConnectionString` valeur sont déterminées par les sélections effectuées dans l’Assistant Configuration de TableAdapter. Rappelez-vous que lorsque nous ajoutons tout d’abord un TableAdapter à un DataSet typé cet Assistant nous demande pour la base de données source (voir Figure 1). La liste déroulante dans cette première étape inclut ces bases de données spécifiées dans le fichier de configuration, ainsi que toutes les bases de données dans l’Explorateur de serveurs s connexions de données. Si nous souhaitons utiliser la base de données n’existe pas dans la liste déroulante, vous pouvez spécifier une nouvelle connexion de base de données en cliquant sur le bouton Nouvelle connexion et en fournissant les informations de connexion nécessaires.


[![La première étape de l’Assistant Configuration de TableAdapter](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image1.png)

**Figure 1**: la première étape de l’Assistant Configuration de TableAdapter ([cliquez pour afficher l’image en taille réelle](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image3.png))


S permettent de prendre un moment pour inspecter le code pour les opérations de mappage TableAdapter `Connection` propriété. Comme indiqué dans le [création d’une couche d’accès aux données](../introduction/creating-a-data-access-layer-cs.md) didacticiel, nous pouvons afficher le code du TableAdapter généré automatiquement en accédant à la fenêtre d’affichage de classes, parcours de la classe appropriée, puis double-cliquez sur le nom du membre.

Accédez à la fenêtre d’affichage de classes en accédant au menu Affichage et en choisissant Affichage de classes (ou en tapant Ctrl + Maj + C). À partir de la moitié supérieure de la fenêtre Affichage de classes, le détail le `NorthwindTableAdapters` espace de noms et sélectionnez le `ProductsTableAdapter` classe. Ceci affichera le `ProductsTableAdapter` membres s dans la partie inférieure la moitié de l’affichage de classes, comme illustré dans la Figure 2. Double-cliquez sur le `Connection` propriété pour afficher son code.


![Double-cliquez sur la propriété de connexion dans l’affichage de classes pour afficher son Code généré automatiquement](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image4.png)

**Figure 2**: double-cliquez sur la propriété de connexion dans l’affichage de classes pour afficher son Code généré automatiquement


Le TableAdapter s `Connection` propriété et autres suit code liées à la connexion :


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample1.cs)]

Lorsque la classe de TableAdapter est instanciée, la variable membre `_connection` est égal à `null`. Lorsque le `Connection` propriété est accessible, il vérifie d’abord pour voir si le `_connection` variable membre a été instanciée. Si tel n’est pas, le `InitConnection` méthode est appelée, ce qui instancie `_connection` et définit ses `ConnectionString` propriété à la valeur de chaîne de connexion spécifiée à partir de l’étape de première Configuration de TableAdapter Assistant s.

Le `Connection` propriété peut également être affectée à un `SqlConnection` objet. Cela associe le nouvel `SqlConnection` objet avec chacun des s TableAdapter `SqlCommand` objets.

## <a name="step-2-exposing-connection-level-settings"></a>Étape 2 : Exposer les paramètres au niveau de la connexion

Les informations de connexion doivent rester encapsulées dans le TableAdapter et ne pas accessible à d’autres couches dans l’architecture d’application. Toutefois, il peut exister des scénarios lorsque les informations de connexion au niveau de s TableAdapter doivent être accessible ou personnalisables pour une requête, un utilisateur ou une page ASP.NET.

S permettent d’étendre le `ProductsTableAdapter` dans le `Northwind` jeu de données à inclure un `ConnectionString` propriété qui peut être utilisée par la couche de logique métier pour lire ou modifier la chaîne de connexion utilisée par le TableAdapter.

> [!NOTE]
> Un *chaîne de connexion* est une chaîne qui spécifie les informations de connexion de base de données, tels que le fournisseur à utiliser, l’emplacement de la base de données, les informations d’identification de l’authentification et les autres paramètres liés à la base de données. Pour obtenir la liste des modèles de chaîne de connexion utilisés par diverses banques de données et fournisseurs, consultez [ConnectionStrings.com](http://www.connectionstrings.com/).


Comme indiqué dans le [création d’une couche d’accès aux données](../introduction/creating-a-data-access-layer-cs.md) didacticiel, les classes générées automatiquement de DataSet typée s peuvent être étendus via l’utilisation des classes partielles. Tout d’abord, créez un nouveau sous-dossier dans le projet nommé `ConnectionAndCommandSettings` sous le `~/App_Code/DAL` dossier.


![Ajouter un sous-dossier nommé ConnectionAndCommandSettings](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image5.png)

**Figure 3**: ajouter un sous-dossier nommé `ConnectionAndCommandSettings`


Ajoutez un nouveau fichier de classe nommé `ProductsTableAdapter.ConnectionAndCommandSettings.cs` et entrez le code suivant :


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample2.cs)]

Cette classe partielle ajoute un `public` propriété nommée `ConnectionString` à la `ProductsTableAdapter` classe qui permet à n’importe quelle couche lire ou mettre à jour la chaîne de connexion pour la connexion sous-jacente TableAdapter s.

Avec cette classe partielle créée (et enregistré), ouvrez le `ProductsBLL` classe. Accédez à une des méthodes existantes et tapez dans `Adapter` , puis appuyez sur la touche point pour afficher IntelliSense. Vous devez voir le nouveau `ConnectionString` propriété disponible dans IntelliSense, ce qui signifie que vous pouvez par programmation lire ou modifier cette valeur à partir de la couche BLL.

## <a name="exposing-the-entire-connection-object"></a>Exposition de l’objet de connexion complète

Cette classe partielle expose qu’une des propriétés de l’objet de connexion sous-jacent : `ConnectionString`. Si vous souhaitez que l’objet de connexion complète disponible au-delà des limites du TableAdapter, vous pouvez également modifier le `Connection` niveau de protection de la propriété s. Le code généré automatiquement, nous avons examiné à l’étape 1 a montré que le TableAdapter s `Connection` propriété est marquée comme `internal`, ce qui signifie qu’il est uniquement accessible par les classes dans le même assembly. Cela peut être modifié, toutefois, via le TableAdapter s `ConnectionModifier` propriété.

Ouvrez le `Northwind` jeu de données, cliquez sur le `ProductsTableAdatper` dans le concepteur et accédez à la fenêtre Propriétés. Vous y trouverez le `ConnectionModifier` définie sur sa valeur par défaut, `Assembly`. Pour rendre le `Connection` disponible en dehors de l’assembly de s DataSet typée, modification de la propriété le `ConnectionModifier` propriété `Public`.


[![Le niveau d’accessibilité de connexion propriété s peut être configuré via la propriété ConnectionModifier](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image6.png)

**Figure 4**: le `Connection` propriété s accessibilité niveau peut être configuré via le `ConnectionModifier` propriété ([cliquez pour afficher l’image en taille réelle](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image8.png))


Enregistrer le jeu de données, puis revenez à la `ProductsBLL` classe. Comme avant, accédez à une des méthodes existantes et tapez `Adapter` , puis appuyez sur la touche point pour afficher IntelliSense. La liste doit inclure un `Connection` propriété, ce qui signifie que vous pouvez désormais par programmation lire ou affecter tous les paramètres au niveau de la connexion à partir de la couche BLL.

## <a name="step-3-examining-the-command-related-properties"></a>Étape 3 : Examiner les propriétés liées à la commande

Un TableAdapter se compose d’une requête principale qui, par défaut, a généré automatiquement `INSERT`, `UPDATE`, et `DELETE` instructions. Cette requête principale s `INSERT`, `UPDATE`, et `DELETE` instructions sont implémentées dans le code du TableAdapter s comme un objet d’adaptateur de données ADO.NET via la `Adapter` propriété. Comme avec ses `Connection` propriété, le `Adapter` type de données de propriété s est déterminé par le fournisseur de données utilisé. Dans la mesure où ces didacticiels utilisent le fournisseur SqlClient, le `Adapter` propriété est de type [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter(VS.80).aspx).

Le TableAdapter s `Adapter` propriété possède trois propriétés de type `SqlCommand` qu’il utilise pour problème la `INSERT`, `UPDATE`, et `DELETE` instructions :

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

Un `SqlCommand` objet est chargé d’envoyer une requête spécifique à la base de données et a des propriétés telles que : [ `CommandText` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtext.aspx), qui contient l’instruction SQL d’ad hoc ou procédure stockée à exécuter ; et [ `Parameters` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.parameters.aspx), qui est une collection de `SqlParameter` objets. Comme nous l’avons vu dans la [création d’une couche d’accès aux données](../introduction/creating-a-data-access-layer-cs.md) didacticiel, ces commandes objets pouvant être personnalisés via la fenêtre Propriétés.

En plus de sa requête principale, le TableAdapter peut inclure un nombre variable de méthodes qui, lorsqu’elle est appelée, distribuer une commande spécifiée à la base de données. L’objet de commande de requête principale s et les objets de commande pour toutes les méthodes supplémentaires sont stockés dans le TableAdapter s `CommandCollection` propriété.

Let s prenez un moment pour examiner le code généré par le `ProductsTableAdapter` dans le `Northwind` jeu de données pour ces deux propriétés et leur prise en charge les variables membres et les méthodes d’assistance :


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample3.cs)]

Le code pour le `Adapter` et `CommandCollection` propriétés imite celui de la `Connection` propriété. Il existe des variables de membre qui contiennent les objets utilisés par les propriétés. Les propriétés `get` accesseurs commencez par vérifier si la variable de membre correspondant est `null`. Dans ce cas, une méthode d’initialisation est appelée, ce qui crée une instance de la variable de membre et attribue les principales propriétés liées à la commande.

## <a name="step-4-exposing-command-level-settings"></a>Étape 4 : Exposer les paramètres au niveau de la commande

Dans l’idéal, les informations au niveau de la commande doivent rester encapsulées au sein de la couche d’accès aux données. Ces informations doivent être nécessaires dans les autres couches de l’architecture, toutefois, il peut être exposé via une classe partielle, tout comme avec les paramètres au niveau de la connexion.

Dans la mesure où le TableAdapter a uniquement un seul `Connection` propriété, le code pour l’exposition des paramètres au niveau de la connexion est assez simple. Choses sont un peu plus compliquées lorsque vous modifiez les paramètres au niveau de la commande, car le TableAdapter peut avoir plusieurs objets de commande - une `InsertCommand`, `UpdateCommand`, et `DeleteCommand`, ainsi que d’un nombre variable d’objets de commande dans le `CommandCollection` propriété. Lors de la mise à jour des paramètres au niveau de la commande, ces paramètres seront doivent être propagées à tous les objets de commande.

Par exemple, imaginez qu’il y avait certaines requêtes du TableAdapter qui ont eu une extraordinaire beaucoup de temps à exécuter. Lorsque vous utilisez le TableAdapter pour exécuter l’une de ces requêtes, nous pourrions augmenter l’objet de commande s [ `CommandTimeout` propriété](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx). Cette propriété spécifie le nombre de secondes d’attente de la commande à exécuter et la valeur par défaut est 30.

Pour permettre la `CommandTimeout` propriété être ajustée par la couche BLL, ajoutez le code suivant `public` méthode à la `ProductsDataTable` à l’aide du fichier de classe partielle créée à l’étape 2 (`ProductsTableAdapter.ConnectionAndCommandSettings.cs`) :


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample4.cs)]

Cette méthode peut être appelée à partir de la couche de logique métier ou une couche de présentation pour définir le délai d’attente de commande pour tous les problèmes de commandes par cette instance de TableAdapter.

> [!NOTE]
> Le `Adapter` et `CommandCollection` propriétés sont marquées comme étant `private`, ce qui signifie qu’ils soient uniquement accessibles à partir du code dans le TableAdapter. Contrairement à la `Connection` propriété, ces modificateurs d’accès ne sont pas configurables. Par conséquent, si vous avez besoin exposer les propriétés de niveau de la commande à d’autres couches dans l’architecture, vous devez utiliser l’approche de classe partielle décrite ci-dessus pour fournir un `public` méthode ou une propriété qui lit ou écrit dans le `private` objets de commande.


## <a name="summary"></a>Récapitulatif

Les TableAdapters dans un DataSet typé servent à encapsuler les détails d’accès aux données et la complexité. À l’aide de TableAdapters, il est inutile à vous soucier de l’écriture du code ADO.NET pour se connecter à la base de données, exécutez une commande d’ou remplir les résultats dans un DataTable. Tout est traité automatiquement pour nous.

Toutefois, il peut arriver lorsque nous avons besoin pour personnaliser les spécificités de ADO.NET de bas niveau, telles que la modification de la chaîne de connexion ou les valeurs de délai d’expiration de connexion ou de la commande par défaut. Le TableAdapter a généré automatiquement `Connection`, `Adapter`, et `CommandCollection` propriétés, mais elles sont soit `internal` ou `private`, par défaut. Ces informations internes peuvent être exposées en étendant le TableAdapter à l’aide des classes partielles pour inclure `public` méthodes ou propriétés. Vous pouvez également le TableAdapter s `Connection` modificateur d’accès de propriété peut être configuré via le TableAdapter s `ConnectionModifier` propriété.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel ont été Burnadette Leigh, S ren Jacob Lauritsen, Teresa Murphy et Hilton Geisenow. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](working-with-computed-columns-cs.md)
> [Suivant](protecting-connection-strings-and-other-configuration-information-cs.md)
