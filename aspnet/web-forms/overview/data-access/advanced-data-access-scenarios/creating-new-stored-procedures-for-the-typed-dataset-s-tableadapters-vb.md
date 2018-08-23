---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Création de procédures stockées pour les TableAdapters de DataSet typé (VB) | Microsoft Docs
author: rick-anderson
description: Dans les didacticiels précédents, nous avons créé des instructions SQL dans notre code et passé les instructions à la base de données doit être exécuté. Une autre approche consiste à utiliser s...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: a5a4a9ba-d18d-489a-a6b0-a3c26d6b0274
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: bc640564cfb67f0c1512bc7f4fae9ea7e6bc981f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838952"
---
<a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Création de procédures stockées pour les TableAdapters de DataSet typé (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_VB.zip) ou [télécharger le PDF](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial67vb1.pdf)

> Dans les didacticiels précédents, nous avons créé des instructions SQL dans notre code et passé les instructions à la base de données doit être exécuté. Une autre approche consiste à utiliser des procédures stockées, où les instructions SQL sont prédéfinies dans la base de données. Dans ce didacticiel, nous apprendre pour que l’Assistant TableAdapter à générer de nouvelles procédures stockées pour nous.


## <a name="introduction"></a>Introduction

La couche DAL (Data Access) pour ces didacticiels utilise des DataSet typés. Comme indiqué dans le [création d’une couche d’accès aux données](../introduction/creating-a-data-access-layer-vb.md) didacticiel, les DataSets typés sont constituées des TableAdapters et les tables de données fortement typées. Les tables de données représentent les entités logiques dans le système lors de l’interface de TableAdapters avec la base de données sous-jacent pour effectuer le travail d’accès aux données. Cela inclut le remplissage des tables de données avec des données, l’exécution de requêtes qui retournent des données scalaires, insertion, mise à jour et suppression d’enregistrements à partir de la base de données.

Les commandes SQL exécutées par les TableAdapters peuvent être soit des instructions SQL ad hoc, tel que `SELECT columnList FROM TableName`, ou des procédures stockées. Les TableAdapters dans notre architecture d’utiliser des instructions SQL ad hoc. Nombreux développeurs et administrateurs de base de données, préfèrent Toutefois, les procédures stockées sur les instructions SQL ad hoc pour des raisons de sécurité, la facilité de gestion et de mise à jour. D’autres préfèrent ardently des instructions SQL ad hoc pour leur flexibilité. Dans mon travail, je favoriser les procédures stockées sur les instructions SQL ad hoc, mais a choisi d’utiliser des instructions SQL ad hoc pour simplifier les didacticiels précédents.

Lorsque la définition d’un TableAdapter ou d’ajouter de nouvelles méthodes, l’Assistant TableAdapter s rend comme facile de créer des procédures stockées ou utiliser des procédures stockées existantes comme il le fait pour utiliser des instructions SQL ad hoc. Dans ce didacticiel, nous allons examiner comment l’Assistant TableAdapter s création automatique des procédures stockées. Dans le didacticiel suivant, nous allons examiner comment configurer les méthodes de s TableAdapter pour utiliser des procédures stockées existantes ou créé manuellement.

> [!NOTE]
> Consultez l’entrée de blog de Rob Howard [Don utilisez encore les procédures stockées t ?](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/) et [Frans Bouma](https://weblogs.asp.net/fbouma/) entrée de blog de s [de procédures stockées sont défectueux, M Kay ?](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx) pour un débat animé sur les avantages et inconvénients de procédures stockées et ad-hoc SQL.


## <a name="stored-procedure-basics"></a>Présentation des procédures stockées

Les fonctions sont une construction commune à tous les langages de programmation. Une fonction est une collection d’instructions qui sont exécutées lorsque la fonction est appelée. Fonctions peuvent accepter des paramètres d’entrée et peuvent éventuellement retourner une valeur. *[Procédures stockées](http://en.wikipedia.org/wiki/Stored_procedure)*  sont des constructions de base de données qui partagent de nombreuses similitudes avec les fonctions de langages de programmation. Une procédure stockée est constituée d’un ensemble d’instructions T-SQL qui sont exécutées lorsque la procédure stockée est appelée. Une procédure stockée peut accepter de zéro et de paramètres d’entrée et peut retourner des valeurs scalaires, des paramètres de sortie, ou, plus couramment, jeux de résultats à partir de `SELECT` requêtes.

> [!NOTE]
> Les procédures stockées sont souvent appelées sprocs ou SPs.


Procédures stockées sont créées à l’aide de la [ `CREATE PROCEDURE` ](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) instruction T-SQL. Par exemple, le script T-SQL suivant crée une procédure stockée nommée `GetProductsByCategoryID` qui prend un paramètre unique nommé `@CategoryID` et retourne le `ProductID`, `ProductName`, `UnitPrice`, et `Discontinued` champs de ces colonnes dans le `Products` table qui a une correspondance `CategoryID` valeur :


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Une fois cette procédure stockée a été créée, elle peut être appelée à l’aide de la syntaxe suivante :


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.sql)]

> [!NOTE]
> Dans le didacticiel suivant, nous allons examiner de création de procédures stockées via l’IDE Visual Studio. Pour ce didacticiel, toutefois, nous allons laisser l’Assistant TableAdapter générer automatiquement les procédures stockées pour nous.


En plus de retourner simplement les données, procédures stockées sont souvent utilisées pour exécuter plusieurs commandes de base de données dans l’étendue d’une transaction unique. Une procédure stockée nommée `DeleteCategory`, par exemple, peut prendre un `@CategoryID` paramètre et effectuer deux `DELETE` instructions : Premièrement, l’une pour supprimer les produits connexes et une seconde lors de la suppression de la catégorie spécifiée. Plusieurs instructions dans une procédure stockée sont *pas* automatiquement encapsulée dans une transaction. Les commandes T-SQL supplémentaires doivent être émis pour vous assurer de la procédure stockée s que plusieurs commandes sont traitées comme une opération atomique. Nous verrons comment encapsuler des commandes une procédure stockée dans l’étendue d’une transaction dans le didacticiel suivant.

Lorsque vous utilisez des procédures stockées au sein d’une architecture, les méthodes de s de couche d’accès aux données appellent une procédure stockée au lieu d’émettre une instruction SQL d’ad-hoc. Cela centralise l’emplacement des instructions SQL exécutées (sur la base de données) au lieu d’avoir défini au sein de l’architecture d’application s. Cette centralisation permet plus facilement rechercher, analyser et régler les requêtes sans doute et fournit une image beaucoup plus claire qu’où et comment la base de données est utilisé.

Pour plus d’informations sur les principes de base de procédure stockée, consultez les ressources dans la section obtenir des informations supplémentaires à la fin de ce didacticiel.

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>Étape 1 : Création de ces avancées couche scénarios Web Pages

Avant de commencer notre discussion sur la création d’une DAL à l’aide de procédures stockées, permettent de s tout d’abord prendre un moment pour créer les pages ASP.NET dans notre projet de site Web que nous aurons besoin pour cela et les didacticiels plusieurs suivants. Commencez par ajouter un nouveau dossier nommé `AdvancedDAL`. Ensuite, ajoutez les pages ASP.NET suivantes à ce dossier, en veillant à associer chaque page avec le `Site.master` page maître :

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`


![Ajouter les Pages ASP.NET pour les didacticiels de scénarios de couche données avancés accès](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Figure 1**: ajoutez les Pages ASP.NET pour les didacticiels de scénarios de couche données avancés accès


Comme dans les autres dossiers, `Default.aspx` dans le `AdvancedDAL` dossier répertorie les didacticiels dans sa section. N’oubliez pas que le `SectionLevelTutorialListing.ascx` contrôle utilisateur fournit cette fonctionnalité. Par conséquent, ajoutez ce contrôle utilisateur à `Default.aspx` en le faisant glisser à partir de l’Explorateur de solutions sur la page s en mode Création.


[![Ajouter le contrôle utilisateur de SectionLevelTutorialListing.ascx à Default.aspx](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)

**Figure 2**: ajouter la `SectionLevelTutorialListing.ascx` contrôle utilisateur à `Default.aspx` ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png))


Enfin, ajoutez ces pages en tant qu’entrées pour le `Web.sitemap` fichier. Plus précisément, ajoutez le balisage suivant après l’utilisation de données traités par lot `<siteMapNode>`:


[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.xml)]

Après la mise à jour `Web.sitemap`, prenez un moment pour afficher le site Web de didacticiels via un navigateur. Le menu de gauche inclut désormais les éléments pour les didacticiels de scénarios de couche DAL avancées.


![Le plan de Site inclut maintenant des entrées pour les didacticiels de scénarios de couche DAL avancées](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)

**Figure 3**: le plan de Site inclut maintenant des entrées pour les didacticiels de scénarios de couche DAL avancées


## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>Étape 2 : Configuration d’un TableAdapter à créer des procédures stockées

Pour illustrer la création d’une couche d’accès aux données qui utilise des procédures stockées au lieu d’instructions SQL ad hoc, s permettent de créer un nouveau DataSet typé dans les `~/App_Code/DAL` dossier nommé `NorthwindWithSprocs.xsd`. Étant donné que nous avons étudié en détail ce processus en détail dans les didacticiels précédents, nous choisissons rapidement à travers les étapes ici. Si vous êtes bloqué ou que vous avez besoin de davantage des instructions pas à pas dans la création et configuration d’un DataSet typé, vous référer à la [création d’une couche d’accès aux données](../introduction/creating-a-data-access-layer-vb.md) didacticiel.

Ajouter un nouveau DataSet au projet en cliquant sur le `DAL` dossier, en choisissant Ajouter un nouvel élément, puis en sélectionnant le modèle de jeu de données comme indiqué dans la Figure 4.


[![Ajouter un nouveau jeu de données typé au projet nommé NorthwindWithSprocs.xsd](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png)

**Figure 4**: ajouter un nouveau DataSet typé pour le projet nommé `NorthwindWithSprocs.xsd` ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png))


Cela sera créer le nouveau DataSet typé, ouvrez son concepteur, créez un TableAdapter et lancer l’Assistant Configuration de TableAdapter. L’étape de première s Assistant Configuration de TableAdapter nous invite à sélectionner la base de données pour travailler avec. La chaîne de connexion à la base de données Northwind doit être répertoriée dans la liste déroulante. Sélectionnez cette option et cliquez sur Suivant.

À partir de cet écran suivant, nous pouvons choisir comment le TableAdapter doit-il accéder à la base de données. Dans les didacticiels précédents, nous avons sélectionné la première option, utiliser des instructions SQL. Pour ce didacticiel, sélectionnez la deuxième option, créer des procédures stockées et cliquez sur Suivant.


[![Indiquer le TableAdpater pour créer des procédures stockées](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png)

**Figure 5**: indiquer le TableAdpater à créer de nouvelles procédures stockées ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png))


Tout comme avec à l’aide des instructions SQL ad hoc, à l’étape suivante on nous demande de fournir la `SELECT` instruction pour la requête principale de TableAdapter s. Mais au lieu d’utiliser le `SELECT` instruction entrée ici pour effectuer une requête ad hoc directement, l’Assistant TableAdapter s créera une procédure stockée qui contient ce `SELECT` requête.

Utilisez la commande suivante `SELECT` requête pour ce TableAdapter :


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]


[![Entrez la requête SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png)

**Figure 6**: entrez le `SELECT` requête ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png))


> [!NOTE]
> La requête ci-dessus diffère légèrement de la requête principale de la `ProductsTableAdapter` dans le `Northwind` DataSet typée. N’oubliez pas que le `ProductsTableAdapter` dans le `Northwind` DataSet typée inclut deux sous-requêtes en corrélation afin de rétablir le nom de la catégorie et le nom de société pour chaque catégorie de produit s et le fournisseur. Dans le prochain [mise à jour du TableAdapter pour l’utilisation de jointures](updating-the-tableadapter-to-use-joins-vb.md) didacticiel, nous allons examiner cela Ajout des données liées à ce TableAdapter.


Prenez un moment cliquer sur le bouton Options avancées. À partir de là, nous pouvons spécifier si l’Assistant doit générer également insert, update et delete instructions pour le TableAdapter, s’il faut utiliser l’accès concurrentiel optimiste et indique si la table de données doit être actualisée après les insertions et mises à jour. L’option d’instructions générer Insert, Update et Delete est activée par défaut. Laissez-la activée. Pour ce didacticiel, laissez les options de l’accès concurrentiel optimiste utilisation désactivée.

Lorsque having les procédures stockées créées automatiquement par l’Assistant TableAdapter, il apparaît que l’actualisation de l’option de table de données est ignorée. Indépendamment de si cette case à cocher est activée, qui en résulte insertion et mise à jour des procédures stockées récupérer l’enregistrement inséré juste ou mis à jour juste, comme nous le verrons à l’étape 3.


![Laissez les instructions générer Insert, Update et Delete, Option activée](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png)

**Figure 7**: laisser les instructions générer Insert, Update et Delete, Option activée


> [!NOTE]
> Si l’option de l’accès concurrentiel optimiste utiliser est activée, l’Assistant ajoutera des conditions supplémentaires pour le `WHERE` clause qui empêchent les données à partir de la mise à jour si des modifications ont été dans d’autres champs. Faire référence à la [implémentation de l’accès concurrentiel optimiste](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) didacticiel pour plus d’informations sur l’utilisation de la fonctionnalité de contrôle d’accès concurrentiel optimiste intégrés s TableAdapter.


Après avoir entré la `SELECT` interroger et confirmé que l’option d’instructions générer Insert, Update et Delete est cochée, cliquez sur Suivant. Cet écran suivant, illustré à la Figure 8, invite à entrer les noms des procédures stockées, que l’Assistant va créer pour la sélection, insertion, la mise à jour et suppression de données. Modifier ces noms de procédures à de stockage `Products_Select`, `Products_Insert`, `Products_Update`, et `Products_Delete`.


[![Renommer les procédures stockées](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Figure 8**: renommer des procédures stockées ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))


Pour afficher le code T-SQL, l’Assistant TableAdapter utilisera pour créer les quatre procédures stockées, cliquez sur le bouton Aperçu le Script SQL. À partir de la boîte de dialogue de Script SQL de version préliminaire, vous pouvez enregistrer le script dans un fichier ou copiez-le dans le Presse-papiers.


![Afficher un aperçu du Script SQL utilisé pour générer les procédures stockées](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Figure 9**: aperçu du Script SQL utilisé pour générer les procédures stockées


Après la désignation des procédures stockées, cliquez sur Suivant pour les méthodes correspondantes nom le TableAdapter s. Comme lors de l’utilisation des instructions SQL ad hoc, nous pouvons créer des méthodes pour remplir un DataTable existant ou retournent un nouveau. Nous pouvons également spécifier si le TableAdapter doit inclure le modèle de Direct à la base de données pour insérer, mettre à jour et suppression d’enregistrements. Laissez toutes les cases à trois cocher vérifiées, mais renommer le retour à une méthode de DataTable à `GetProducts` (comme indiqué dans la Figure 10).


[![Nommez les méthodes Fill et GetProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)

**Figure 10**: nommez les méthodes `Fill` et `GetProducts` ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png))


Cliquez sur Suivant pour afficher un résumé des étapes de que l’Assistant va effectuer. Terminez l’Assistant en cliquant sur le bouton Terminer. Une fois l’Assistant terminé, vous serez renvoyé au jeu de données s concepteur, qui doit inclure désormais le `ProductsDataTable`.


[![Le Concepteur de s DataSet montre le ProductsDataTable nouvellement ajoutée](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)

**Figure 11**: le jeu de données s Concepteur montre nouvellement ajouté `ProductsDataTable` ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png))


## <a name="step-3-examining-the-newly-created-stored-procedures"></a>Étape 3 : Examiner les procédures stockées qui vient d’être créés

L’Assistant TableAdapter utilisé automatiquement à l’étape 2 a créé les procédures stockées pour la sélection, insertion, la mise à jour et suppression de données. Ces procédures stockées peuvent être visualisées ou modifiées via Visual Studio en accédant à l’Explorateur de serveurs et de descente dans le dossier de procédures stockées de base de données s. Comme le montre la Figure 12, la base de données Northwind contient quatre nouvelles procédures stockées : `Products_Delete`, `Products_Insert`, `Products_Select`, et `Products_Update`.


![Vous trouverez les quatre procédures stockées créées à l’étape 2 dans le dossier de procédures stockées de base de données s](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)

**Figure 12**: les quatre procédures stockées créées à l’étape 2 peuvent être trouvées dans le dossier de procédures stockées de base de données s


> [!NOTE]
> Si vous ne voyez pas l’Explorateur de serveurs, accédez au menu Affichage et choisissez l’option de l’Explorateur de serveurs. Si vous ne voyez pas les procédures stockées de produit ajoutés à l’étape 2, essayez de clic droit sur le dossier de procédures stockées et en choisissant actualise.


Pour afficher ou modifier une procédure stockée, double-cliquez sur son nom dans l’Explorateur de serveurs ou bien, vous pouvez également, avec le bouton droit sur la procédure stockée et choisissez Ouvrir. La figure 13 montre la `Products_Delete` procédure stockée, l’ouverture.


[![Les procédures stockées peuvent être ouvert et modifiés dans Visual Studio](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png)

**Figure 13**: stockées procédures peuvent être ouverts et modifiés à partir de Visual Studio ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png))


Le contenu des deux le `Products_Delete` et `Products_Select` les procédures stockées sont relativement simples. Le `Products_Insert` et `Products_Update` des procédures stockées, justifient quant à eux, un examen approfondi pendant qu’ils les deux effectuent un `SELECT` instruction après leur `INSERT` et `UPDATE` instructions. Par exemple, le code SQL suivant constitue le `Products_Insert` procédure stockée :


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

La procédure stockée accepte comme paramètres d’entrée le `Products` colonnes qui ont été retournées par la `SELECT` requête spécifiée dans l’Assistant de s TableAdapter et ces valeurs sont utilisées dans un `INSERT` instruction. Suivant le `INSERT` instruction, un `SELECT` requête est utilisée pour retourner le `Products` les valeurs de colonne (y compris le `ProductID`) de l’enregistrement qui vient d’être ajouté. Cette fonctionnalité d’actualisation est utile lors de l’ajout d’un nouvel enregistrement à l’aide du modèle de mise à jour par lots tel qu’il automatiquement des mises à jour récemment ajouté `ProductRow` instances `ProductID` propriétés avec les valeurs incrémentées automatiquement affectées par la base de données.

Le code suivant illustre cette fonctionnalité. Il contient un `ProductsTableAdapter` et `ProductsDataTable` créé pour le `NorthwindWithSprocs` DataSet typée. Un nouveau produit est ajouté à la base de données en créant un `ProductsRow` instance, qui fournit ses valeurs et en appelant le TableAdapter s `Update` méthode, en passant le `ProductsDataTable`. En interne, le TableAdapter s `Update` méthode énumère la `ProductsRow` instances dans le DataTable dans le passé (dans cet exemple il existe un seul - celui que nous venons d’ajouter) et effectue approprié insert, update ou supprimer la commande. Dans ce cas, le `Products_Insert` procédure stockée est exécutée, qui ajoute un nouvel enregistrement à la `Products` de table et retourne les détails de l’enregistrement qui vient d’être ajouté. Le `ProductsRow` instance s `ProductID` la mise à jour puis de valeur. Après le `Update` méthode est terminée, nous pouvons accéder à l’enregistrement qui vient d’être ajouté s `ProductID` valeur via la `ProductsRow` s `ProductID` propriété.


[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.vb)]

Le `Products_Update` procédure stockée inclut même un `SELECT` instruction après son `UPDATE` instruction.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample7.sql)]

Notez que cette procédure stockée comprend deux paramètres d’entrée pour `ProductID`: `@Original_ProductID` et `@ProductID`. Cette fonctionnalité autorise des scénarios où la clé primaire peut être modifiée. Par exemple, dans une base de données employés, chaque enregistrement d’employé peut utiliser le numéro de sécurité sociale s employé en tant que leur clé primaire. Pour modifier un numéro de sécurité sociale de s employé existant, le nouveau numéro de sécurité sociale et celui d’origine doivent être fournis. Pour le `Products` table, cette fonctionnalité n’est pas nécessaire car le `ProductID` colonne est un `IDENTITY` colonne et ne doit jamais être modifiée. En fait, le `UPDATE` instruction dans le `Products_Update` procédure stockée ne t incluent la `ProductID` colonne dans sa liste de colonnes. Par conséquent, tandis que `@Original_ProductID` est utilisé dans le `UPDATE` instruction s `WHERE` clause, c’est superflu pour le `Products` table et pourrait être remplacé par le `@ProductID` paramètre. Lorsque vous modifiez des paramètres de procédure stockée s’il est important que l’ou les méthodes TableAdapter qui utilisent cette procédure stockée sont également mis à jour.

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>Étape 4 : Modification de des paramètres de procédure stockée s et la mise à jour du TableAdapter

Dans la mesure où le `@Original_ProductID` paramètre est superflu, s permettent de supprimer de la `Products_Update` procédure stockée complètement. Ouvrir le `Products_Update` procédure stockée, supprimer le `@Original_ProductID` paramètre et, dans le `WHERE` clause de la `UPDATE` instruction, modifiez le nom de paramètre utilisé à partir de `@Original_ProductID` à `@ProductID`. Après avoir apporté ces modifications, le code T-SQL dans la procédure stockée doit se présenter comme suit :


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample8.sql)]

Pour enregistrer ces modifications dans la base de données, cliquez sur l’icône Enregistrer dans la barre d’outils ou appuyez sur Ctrl + S. À ce stade, le `Products_Update` n’attend pas de procédure stockée une `@Original_ProductID` paramètre d’entrée, mais le TableAdapter est configuré pour passer un paramètre de ce type. Vous pouvez voir les paramètres TableAdapter enverra à la `Products_Update` procédure stockée en sélectionnant le TableAdapter dans le Concepteur de DataSet, accédant à la fenêtre Propriétés et en cliquant sur le bouton de sélection dans le `UpdateCommand` s `Parameters` collection. Ceci fait apparaître la boîte de dialogue Éditeur de collections Parameters indiquée dans la Figure 14.


![Les listes d’éditeur de collections de paramètres les paramètres utilisés passé à la Products_Update procédure stockée](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png)

**Figure 14**: les listes d’éditeur de collections de paramètres les paramètres utilisés passé à la `Products_Update` procédure stockée


Vous pouvez supprimer ce paramètre à partir d’ici en sélectionnant simplement le `@Original_ProductID` paramètre dans la liste des membres et en cliquant sur le bouton Supprimer.

Ou bien, vous pouvez actualiser les paramètres utilisés pour toutes les méthodes en effectuant un clic droit sur le TableAdapter dans le concepteur et en choisissant de configurer. Cela fera apparaître l’Assistant Configuration de TableAdapter, répertoriant les procédures stockées utilisées pour la sélection, insertion, mise à jour, et la suppression, ainsi que les paramètres des procédures stockées s’attendre à recevoir. Si vous cliquez sur la liste déroulante de mise à jour, vous pouvez voir le `Products_Update` des procédures stockées attendu de paramètres d’entrée, qui inclut maintenant n’est plus `@Original_ProductID` (voir Figure 15). Cliquez simplement sur Terminer pour automatiquement mettre à jour la collection de paramètres utilisée par le TableAdapter.


[![Vous pouvez également utiliser l’Assistant de Configuration de TableAdapter s pour actualiser ses Collections de paramètres de méthodes](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Figure 15**: vous pouvez également utiliser l’Assistant de Configuration pour actualiser une ses Collections de paramètres de méthodes du TableAdapter s ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))


## <a name="step-5-adding-additional-tableadapter-methods"></a>Étape 5 : Ajout de méthodes TableAdapter supplémentaires

En tant qu’étape 2 illustré, lors de la création d’un nouveau TableAdapter, il est facile d’avoir des procédures stockées correspondantes générés automatiquement. Il en est de même lorsque vous ajoutez des méthodes supplémentaires à un TableAdapter. Pour illustrer ceci, permettent s d’ajouter un `GetProductByProductID(productID)` méthode à la `ProductsTableAdapter` créé à l’étape 2. Cette méthode prend comme entrée un `ProductID` valeur et retourner des détails sur le produit spécifié.

Démarrer en cliquant sur le TableAdapter et en choisissant Ajouter une requête dans le menu contextuel.


![Ajouter une nouvelle requête au TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Figure 16**: ajouter une nouvelle requête au TableAdapter


Ceci démarrera l’Assistant Configuration de requêtes TableAdapter, qui demande tout d’abord comment le TableAdapter doit-il accéder à la base de données. Pour qu’une nouvelle procédure stockée créée, sélectionnez Créer une nouvelle option de procédure stockée et cliquez sur Suivant.


[![Choisissez de créer une nouvelle Option de procédure stockée](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)

**Figure 17**: choisissez de créer une nouvelle Option de procédure stockée ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png))


L’écran suivant nous demande pour identifier le type de requête à exécuter, si elle sera retourner un ensemble de lignes ou une valeur scalaire unique, ou effectuer une `UPDATE`, `INSERT`, ou `DELETE` instruction. Dans la mesure où le `GetProductByProductID(productID)` méthode sera retourner une ligne, laissez l’instruction SELECT qui retourne l’option de la ligne sélectionnée et appuyez sur Suivant.


[![Choisissez l’instruction SELECT qui retourne l’Option de ligne](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)

**Figure 18**: choisissez l’instruction SELECT qui retourne l’Option de ligne ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png))


L’écran suivant affiche la requête principale s du TableAdapter, qui répertorie simplement le nom de la procédure stockée (`dbo.Products_Select`). Remplacez le nom de la procédure stockée par le code suivant `SELECT` instruction, qui retourne tous les champs de produit pour un produit donné :


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample9.sql)]


[![Remplacez le nom de la procédure stockée par une requête SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)

**Figure 19**: Remplacez le nom de la procédure stockée avec un `SELECT` requête ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png))


L’écran suivant vous invite à nommer la procédure stockée qui sera créée. Entrez le nom `Products_SelectByProductID` et cliquez sur Suivant.


[![Nom de la nouvelle Products_SelectByProductID de procédure stockée](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Figure 20**: nom de la nouvelle procédure stockée `Products_SelectByProductID` ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image46.png))


L’étape finale de l’Assistant permet de modifier la méthode de noms générant ainsi que pour indiquent s’il faut utiliser le remplissage un modèle de DataTable, retourner un DataTable de modèle, ou les deux. Pour cette méthode, laissez les deux options activées, mais les méthodes à le renommer `FillByProductID` et `GetProductByProductID`. Cliquez sur Suivant pour afficher un résumé des étapes de l’Assistant va effectuer et puis cliquez sur Terminer pour terminer l’Assistant.


[![Renommez les méthodes de s TableAdapter FillByProductID et GetProductByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image47.png)

**Figure 21**: Renommez les méthodes de s TableAdapter à `FillByProductID` et `GetProductByProductID` ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image49.png))


À l’issue de l’Assistant, le TableAdapter ne dispose d’une nouvelle méthode disponible, `GetProductByProductID(productID)` qui, lorsqu’elle est appelée, exécutera le `Products_SelectByProductID` procédure stockée qui vient d’être créé. Prenez un moment pour afficher cette nouvelle procédure stockée à partir de l’Explorateur de serveurs en extraction vers le dossier de procédures stockées et en ouvrant `Products_SelectByProductID` (si vous ne le voyez pas, avec le bouton droit sur le dossier Stored Procedures, cliquez sur Actualiser).

Notez que le `SelectByProductID` procédure stockée accepte `@ProductID` comme paramètre d’entrée et exécute la `SELECT` instruction que nous avions saisie dans l’Assistant.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>Étape 6 : Création d’une classe de couche de logique métier

Tout au long de la série de didacticiels, nous avons cherché à mettre à jour d’une architecture en couches dans lequel la couche de présentation mis tous ses appels à la couche BLL (Business Logic). Afin de respecter cette décision de conception, nous devons d’abord créer une classe de la couche de logique métier pour le nouveau DataSet typé avant que nous pouvons accéder à des données de produit à partir de la couche de présentation.

Créer un nouveau fichier de classe nommé `ProductsBLLWithSprocs.vb` dans le `~/App_Code/BLL` dossier et lui ajouter le code suivant :


[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample11.vb)]

Reproduit cette classe le `ProductsBLL` classe sémantique à partir des didacticiels précédents, mais utilise le `ProductsTableAdapter` et `ProductsDataTable` objets à partir de la `NorthwindWithSprocs` jeu de données. Par exemple, au lieu d’avoir un `Imports NorthwindTableAdapters` instruction au début du fichier de classe en tant que `ProductsBLL` est le cas, le `ProductsBLLWithSprocs` classe utilise `Imports NorthwindWithSprocsTableAdapters`. De même, le `ProductsDataTable` et `ProductsRow` objets utilisés dans cette classe sont précédés de la `NorthwindWithSprocs` espace de noms. Le `ProductsBLLWithSprocs` classe fournit des méthodes, d’accès aux données de deux `GetProducts` et `GetProductByProductID`, et des méthodes pour ajouter, mettre à jour et supprimer une instance de produit unique.

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>Étape 7 : Utilisation de la`NorthwindWithSprocs`jeu de données à partir de la couche de présentation

À ce stade, nous avons créé une DAL qui utilise des procédures stockées pour accéder et modifier les données de base de données sous-jacente. Nous avons également créé une couche de logique métier rudimentaire avec des méthodes pour récupérer tous les produits ou un produit particulier, ainsi que des méthodes pour l’ajout, la mise à jour et suppression de produits. Pour arrondir ce didacticiel, s permettent de créer une page ASP.NET qui utilise la couche BLL s `ProductsBLLWithSprocs` classe pour l’affichage, la mise à jour et suppression d’enregistrements.

Ouvrez le `NewSprocs.aspx` page dans le `AdvancedDAL` dossier et faites glisser un GridView à partir de la boîte à outils vers le concepteur, nommez-le `Products`. Dans le contrôle GridView balise active s choisissez de le lier à un nouveau ObjectDataSource nommé `ProductsDataSource`. Configurer l’ObjectDataSource à utiliser le `ProductsBLLWithSprocs` classe, comme indiqué dans la Figure 22.


[![Configurer pour utiliser la classe ProductsBLLWithSprocs ObjectDataSource](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image50.png)

**Figure 22**: configurer ObjectDataSource à utiliser le `ProductsBLLWithSprocs` classe ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image52.png))


La liste déroulante dans l’onglet Sélection propose deux options, `GetProducts` et `GetProductByProductID`. Étant donné que nous souhaitons afficher tous les produits dans le contrôle GridView, choisir le `GetProducts` (méthode). Les listes déroulantes dans les onglets UPDATE, INSERT et DELETE ont uniquement une méthode. Assurez-vous que chacune de ces listes déroulantes dispose de sa méthode approprié sélectionné, puis sur Terminer.

Une fois l’Assistant ObjectDataSource terminée, Visual Studio ajoute BoundFields et un CheckBoxField au GridView pour les champs de données de produit. Activer les fonctionnalités de suppression et de modification intégrés de GridView s en vérifiant les options Activer la suppression qui sont présentes dans la balise active et activer la modification.


[![La Page contient un GridView avec la modification et suppression prenant en charge](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image53.png)

**Figure 23**: la Page contient un GridView avec la modification et suppression de la prise en charge activée ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image55.png))


Comme nous ve abordés dans les didacticiels précédents, à l’issue de l’Assistant de s ObjectDataSource, jeux de Visual Studio le `OldValuesParameterFormatString` propriété original\_{0}. Cette opération doit être rétablie à sa valeur par défaut {0} afin que les fonctionnalités de modification de données fonctionne correctement, étant donné les paramètres attendus par les méthodes dans notre BLL. Par conséquent, veillez à définir le `OldValuesParameterFormatString` propriété {0} ou supprimer complètement de la propriété à partir de la syntaxe déclarative.

Après la fin de l’Assistant Configurer la Source de données, modification et suppression de prise en charge dans le contrôle GridView et retourner le s ObjectDataSource sous tension `OldValuesParameterFormatString` propriété à sa valeur par défaut, votre balisage déclaratif s de page doit ressembler à ce qui suit :


[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample12.aspx)]

À ce stade nous aurions pu nettoyer le GridView en personnalisant l’interface de modification pour inclure la validation, ayant le `CategoryID` et `SupplierID` colonnes restituer comme DropDownList et ainsi de suite. Nous pourrions également ajouter une confirmation côté client pour le bouton Supprimer, et je vous encourage à prendre le temps de mettre en œuvre ces améliorations. Dans la mesure où ces rubriques ont été traités dans les didacticiels précédents, toutefois, nous n'allons pas aborder les à nouveau ici.

Quel que soit l’amélioration que le contrôle GridView ou non, testez les fonctionnalités principales de page s dans un navigateur. Comme le montre la Figure 24, la page répertorie les produits dans un GridView qui fournit par ligne modification et suppression de fonctionnalités.


[![Les produits peuvent être affichés, modifiés et supprimés à partir de la GridView](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image56.png)

**Figure 24**: les produits peuvent être affichées, modifiée et supprimés à partir de la GridView ([cliquez pour afficher l’image en taille réelle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image58.png))


## <a name="summary"></a>Récapitulatif

Les TableAdapters dans un DataSet typé peut accéder aux données à partir de la base de données à l’aide d’instructions SQL ad hoc ou via des procédures stockées. Lorsque utilisation des procédures stockées, soit des procédures stockées existantes peuvent être utilisées ou l’Assistant TableAdapter peut être paramétrée pour créer de nouvelles procédures stockées basées sur un `SELECT` requête. Dans ce didacticiel, nous avons exploré comment procéder pour que les procédures stockées créées automatiquement pour nous.

Bien que les procédures stockées générées automatiquement une vous aide à gagner du temps, il existe certains cas où la procédure stockée créée par l’Assistant ne t s’alignent sur ce que nous aurait créé sur notre propre. Par exemple, le `Products_Update` procédure stockée, ce qui est attendu à la fois `@Original_ProductID` et `@ProductID` même si les paramètres d’entrée le `@Original_ProductID` paramètre était superflu.

Dans de nombreux scénarios, les procédures stockées peuvent avoir été créés, ou nous pouvons choisir de les générer manuellement afin d’avoir un meilleur niveau de contrôle sur les commandes de s de procédure stockée. Dans les deux cas, nous voulons indiquer le TableAdapter à utiliser des procédures stockées existantes pour ses méthodes. Nous verrons comment accomplir cette tâche dans le didacticiel suivant.

Bonne programmation !

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Création et la gestion des procédures stockées](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [Récupération de données scalaire à partir d’une procédure stockée](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [Présentation des procédures stockées de SQL Server](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [Procédures stockées : Une vue d’ensemble](http://www.sqlteam.com/item.asp?ItemID=563)
- [Écriture d’une procédure stockée](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Hilton Geisenow. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
> [Suivant](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
