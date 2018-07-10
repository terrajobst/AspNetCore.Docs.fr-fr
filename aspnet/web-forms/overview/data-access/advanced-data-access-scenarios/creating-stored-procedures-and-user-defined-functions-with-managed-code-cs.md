---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
title: Création de procédures stockées et fonctions définies par l’utilisateur avec Managed Code (c#) | Microsoft Docs
author: rick-anderson
description: Microsoft SQL Server 2005 s’intègre avec le Common Language Runtime .NET pour permettre aux développeurs de créer des objets de base de données par le biais du code managé. Ce didacticiel...
ms.author: aspnetcontent
ms.date: 08/03/2007
ms.assetid: 213eea41-1ab4-4371-8b24-1a1a66c515de
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 33ec7cfbb029c1a0e5200eb61aa4e39b02d991f3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824414"
---
<a name="creating-stored-procedures-and-user-defined-functions-with-managed-code-c"></a>Création de procédures stockées et fonctions définies par l’utilisateur avec du Code managé (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_75_CS.zip) ou [télécharger le PDF](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/datatutorial75cs1.pdf)

> Microsoft SQL Server 2005 s’intègre avec le Common Language Runtime .NET pour permettre aux développeurs de créer des objets de base de données par le biais du code managé. Ce didacticiel montre comment créer des procédures stockées managées et managées des fonctions définies par l’utilisateur avec votre code Visual Basic ou c#. Nous avons également Découvrez comment ces éditions de Visual Studio vous permettent de déboguer ces objets de base de données managés.


## <a name="introduction"></a>Introduction

Utilisent des bases de données comme s Microsoft SQL Server 2005 la [Transact-Structured, langage de requête (T-SQL)](http://en.wikipedia.org/wiki/Transact-SQL) pour l’insertion, la modification et la récupération des données. La plupart des systèmes de base de données incluent des constructions de regroupement d’une série d’instructions SQL qui peuvent ensuite être exécutées comme une unité unique et réutilisable. Les procédures stockées sont un exemple. Un autre est *les fonctions définies par l’utilisateur*(UDF), une construction qui nous examinerons plus en détail à l’étape 9.

Fondamentalement, SQL est conçu pour travailler avec des jeux de données. Le `SELECT`, `UPDATE`, et `DELETE` instructions s’appliquent à tous les enregistrements dans la table correspondante par nature et sont limitées uniquement par leur `WHERE` clauses. Encore il existe de nombreuses fonctionnalités de langage conçues pour travailler avec un seul enregistrement à la fois et de manipulation des données scalaires. [`CURSOR` s](http://www.sqlteam.com/item.asp?ItemID=553) autoriser pour un jeu d’enregistrements à être bouclée via un à la fois. Comme les fonctions de manipulation de chaîne `LEFT`, `CHARINDEX`, et `PATINDEX` fonctionnent avec des données scalaires. SQL inclut également des instructions de flux de contrôle telles que `IF` et `WHILE`.

Antérieures à Microsoft SQL Server 2005, les procédures stockées et des UDF peuvent être définies uniquement comme une collection d’instructions T-SQL. SQL Server 2005, cependant, a été conçu pour offrir une intégration avec le [Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), qui est le runtime utilisé par tous les assemblys .NET. Par conséquent, les procédures stockées et les UDF dans une base de données SQL Server 2005 peut être créés à l’aide de code managé. Autrement dit, vous pouvez créer une procédure stockée ou UDF en tant que méthode dans une classe c#. Ainsi, ces procédures stockées et des UDF pour exploiter les fonctionnalités dans le .NET Framework et à partir de vos propres classes personnalisées.

Dans ce didacticiel, nous allons examiner comment créer géré procédures stockées et les fonctions définies par l’utilisateur et comment les intégrer dans notre base de données Northwind. Laissez s commencer !

> [!NOTE]
> Objets de base de données managés offrent des avantages par rapport à leurs équivalents SQL. Richesse du langage et une bonne connaissance et la possibilité de réutiliser la logique et le code existant sont les principaux avantages. Mais les objets de base de données managés sont susceptibles d’être moins efficace lorsque vous travaillez avec des jeux de données qui n’impliquent pas beaucoup de logique procédurale. Pour une discussion plus détaillée sur les avantages de l’utilisation de code managé et T-SQL, consultez le [avantages d’à l’aide du Code managé pour créer des objets de base de données](https://msdn.microsoft.com/library/k2e1fb36(VS.80).aspx).


## <a name="step-1-moving-the-northwind-database-out-ofappdata"></a>Étape 1 : Déplacer la base de données Northwind de`App_Data`

Tous les nos didacticiels jusqu'à présent ont utilisé un fichier de base de données Microsoft SQL Server 2005 Express Edition dans le s d’application web `App_Data` dossier. Placer la base de données `App_Data` simplifié la distribution et l’exécution de ces didacticiels ainsi tous les fichiers ont été trouvés dans un répertoire ne requis aucune étape de configuration supplémentaires pour tester le didacticiel.

Pour ce didacticiel, toutefois, s permettent de déplacer la base de données Northwind hors `App_Data` et l’inscrire explicitement avec l’instance de base de données SQL Server 2005 Express Edition. Bien que nous pouvons effectuer les étapes pour ce didacticiel avec la base de données dans le `App_Data` dossier, un certain nombre des étapes est apportées beaucoup plus simple en inscrivant explicitement la base de données avec l’instance de base de données SQL Server 2005 Express Edition.

Le téléchargement de ce didacticiel comporte les fichiers de base de données de deux - `NORTHWND.MDF` et `NORTHWND_log.LDF` - placée dans un dossier nommé `DataFiles`. Si vous poursuivez la procédure avec votre propre implémentation des didacticiels, fermez Visual Studio et déplacer le `NORTHWND.MDF` et `NORTHWND_log.LDF` fichiers depuis le site Web s `App_Data` vers un dossier en dehors du site Web. Une fois que les fichiers de base de données ont été déplacés vers un autre dossier que nous devons inscrire la base de données Northwind à l’instance de base de données SQL Server 2005 Express Edition. Cela est possible à partir de SQL Server Management Studio. Si vous avez un non - Express Edition de SQL Server 2005 installé sur votre ordinateur puis vous avez déjà installé de Management Studio. Si vous disposez de SQL Server 2005 Express Edition sur votre ordinateur uniquement, que vous prenez un moment pour télécharger et installer [Microsoft SQL Server Management Studio Express](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Lancez SQL Server Management Studio. Comme le montre la Figure 1, Management Studio démarre en demandant à quel serveur pour vous connecter à. Entrez localhost\SQLExpress pour le nom du serveur, choisissez l’authentification Windows dans la liste déroulante de l’authentification, cliquez sur se connecter.


![Connectez-vous à l’Instance de base de données appropriée](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image1.png)

**Figure 1**: connectez-vous à l’Instance de base de données appropriée


Une fois que vous avez déjà connecté, la fenêtre Explorateur d’objets affiche des informations sur l’instance de base de données SQL Server 2005 Express Edition, y compris ses bases de données, les informations de sécurité, les options de gestion et ainsi de suite.

Nous avons besoin d’attacher la base de données Northwind dans la `DataFiles` dossier (ou, là où vous avez déplacé il) à l’instance de base de données SQL Server 2005 Express Edition. Avec le bouton droit sur le dossier bases de données et choisissez l’option d’attachement dans le menu contextuel. Cela fera apparaître la boîte de dialogue Attacher les bases de données. Cliquez sur le bouton Ajouter, descendre dans approprié `NORTHWND.MDF` fichier, puis cliquez sur OK. À ce stade votre écran doit ressembler à la Figure 2.


[![Connectez-vous à l’Instance de base de données appropriée](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image3.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image2.png)

**Figure 2**: se connecter à l’Instance de base de données appropriée ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image4.png))


> [!NOTE]
> Lors de la connexion à l’instance de SQL Server 2005 Express Edition via Management Studio la boîte de dialogue Attacher les bases de données ne vous permet pas d’approfondir les répertoires de profil utilisateur, tels que Mes Documents. Par conséquent, veillez à placer le `NORTHWND.MDF` et `NORTHWND_log.LDF` fichiers dans un répertoire de profil non-utilisateur.


Cliquez sur le bouton OK pour attacher la base de données. La boîte de dialogue Attacher les bases de données se ferme et l’Explorateur d’objets doit répertorier maintenant la base de données juste-attaché. Il est probable qu’est la base de données a un nom tel que de Northwind `9FE54661B32FDD967F51D71D0D5145CC_LINE ARTICLES\DATATUTORIALS\VOLUME 3\CSHARP\73\ASPNET_DATA_TUTORIAL_75_CS\APP_DATA\NORTHWND.MDF`. Renommer la base de données à Northwind en cliquant sur la base de données et en sélectionnant Renommer.


![Renommez la base de données Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image5.png)

**Figure 3**: Renommez la base de données Northwind


## <a name="step-2-creating-a-new-solution-and-sql-server-project-in-visual-studio"></a>Étape 2 : Création d’une Solution et un projet SQL Server dans Visual Studio

Pour créer des procédures stockées managées ou UDF dans SQL Server 2005, nous allons écrire la procédure stockée et la logique de l’UDF en tant que code c# dans une classe. Une fois que le code a été écrit, nous devons compiler cette classe dans un assembly (un `.dll` fichier), inscrire l’assembly avec la base de données SQL Server, puis créer une procédure stockée ou un objet de fonction UDF dans la base de données qui pointe vers la méthode correspondante dans l’assembly. Ces étapes peuvent toutes être effectuées manuellement. Nous pouvons créer le code dans n’importe quel texte éditeur, le compiler depuis la ligne de commande à l’aide du compilateur c# ([`csc.exe`](https://msdn.microsoft.com/library/ms379563(vs.80).aspx)), inscrivez-le auprès de la base de données à l’aide de la [ `CREATE ASSEMBLY` ](https://msdn.microsoft.com/library/ms189524.aspx) commande ou de la gestion Studio et ajoutez la procédure stockée ou un objet de fonction UDF via un moyen similaire. Heureusement, les versions Professional et les systèmes de l’équipe de Visual Studio incluent un type de projet SQL Server qui automatise les tâches. Dans ce didacticiel nous étudierons utilisant le type de projet SQL Server pour créer une procédure stockée managée et UDF.

> [!NOTE]
> Si vous utilisez Visual Web Developer ou l’Édition Standard de Visual Studio, vous devrez utiliser l’approche manuelle à la place. Étape 13 fournit des instructions détaillées pour effectuer ces étapes manuellement. Je vous encourage à lire les étapes 2 à 12 avant de lire l’étape 13 dans la mesure où ces étapes comprennent les instructions de configuration SQL Server importantes qui doivent être appliquées quel que soit la version de Visual Studio que vous utilisez.


Commencez par ouvrir Visual Studio. Dans le menu fichier, choisissez Nouveau projet pour afficher la boîte de dialogue Nouveau projet zone (voir Figure 4). Explorer le type de projet de base de données et, à partir de modèles répertoriés sur la droite, puis créer un nouveau projet SQL Server. J’ai choisi de nommer ce projet `ManagedDatabaseConstructs` et placé dans une Solution nommée `Tutorial75`.


[![Créer un nouveau projet SQL Server](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image7.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image6.png)

**Figure 4**: créer un nouveau projet SQL Server ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image8.png))


Cliquez sur le bouton OK dans la boîte de dialogue Nouveau projet pour créer la Solution et projet SQL Server.

Un projet SQL Server est lié à une base de données. Par conséquent, après avoir créé le nouveau projet SQL Server nous sommes immédiatement invités à spécifier ces informations. La figure 5 illustre la boîte de dialogue Nouvelle référence de base de données qui a été remplie pour pointer vers la base de données Northwind que nous avons enregistrés dans l’instance de base de données SQL Server 2005 Express Edition à l’étape 1.


![Associer le projet SQL Server à la base de données Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image9.png)

**Figure 5**: associer le projet SQL Server à la base de données Northwind


Pour déboguer les procédures stockées managées et UDF, nous allons créer dans ce projet, nous devons activer la prise en charge pour la connexion de débogage SQL/CLR. Chaque fois que l’association d’un projet SQL Server à une base de données (comme nous l’avons fait dans la Figure 5), Visual Studio vous demande de nous si nous voulons activer le débogage SQL/CLR sur la connexion (voir Figure 6). Cliquez sur Oui.


![Activer le débogage SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image10.png)

**Figure 6**: activer le débogage SQL/CLR


À ce stade, le nouveau projet SQL Server a été ajouté à la Solution. Il contient un dossier nommé `Test Scripts` avec un fichier nommé `Test.sql`, qui est utilisé pour déboguer les objets de base de données managés créés dans le projet. Nous allons examiner de débogage à l’étape 12.

Nous pouvons maintenant ajouter des nouvelles procédures stockées managées et UDF à ce projet, mais avant que nous ne permettent pas aux s d’abord inclure notre application web existante dans la Solution. Dans le menu fichier, sélectionnez l’option Ajouter et choisir un Site Web existant. Accédez au dossier de site Web approprié, puis cliquez sur OK. Comme le montre la Figure 7, cela met à jour la Solution pour inclure les deux projets : le site Web et le `ManagedDatabaseConstructs` projet SQL Server.


![L’Explorateur de solutions inclut maintenant deux projets](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image11.png)

**Figure 7**: l’Explorateur de solutions inclut maintenant deux projets


Le `NORTHWNDConnectionString` valeur dans `Web.config` fait actuellement référence à la `NORTHWND.MDF` de fichiers dans le `App_Data` dossier. Étant donné que nous avons supprimé cette base de données à partir de `App_Data` et enregistrés de manière explicite dans l’instance de base de données SQL Server 2005 Express Edition, nous devons mettre à jour en conséquence le `NORTHWNDConnectionString` valeur. Ouvrez le `Web.config` fichier dans le site Web et la modification la `NORTHWNDConnectionString` valeur afin que la chaîne de connexion se présente : `Data Source=localhost\SQLExpress;Initial Catalog=Northwind;Integrated Security=True`. Après cette modification, votre `<connectionStrings>` section `Web.config` doit ressembler à ce qui suit :


[!code-xml[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample1.xml)]

> [!NOTE]
> Comme indiqué dans le [didacticiel précédent](debugging-stored-procedures-cs.md), lors du débogage d’un objet SQL Server à partir d’une application cliente, comme un site Web ASP.NET, nous devons désactiver le regroupement de connexions. La chaîne de connexion ci-dessus désactive le regroupement de connexions ( `Pooling=false` ). Si vous n’envisagez pas sur le débogage les procédures stockées managées et UDF depuis le site Web ASP.NET, activer le regroupement de connexions.


## <a name="step-3-creating-a-managed-stored-procedure"></a>Étape 3 : Création d’un élément géré une procédure stockée

Pour ajouter une procédure stockée managée à la base de données Northwind, nous devons tout d’abord créer la procédure stockée en tant que méthode dans le projet SQL Server. À partir de l’Explorateur de solutions, cliquez sur le `ManagedDatabaseConstructs` nom du projet et choisissez Ajouter un nouvel élément. Cela affiche la boîte de dialogue Ajouter un nouvel élément, qui répertorie les types d’objets de base de données managé qui peuvent être ajoutés au projet. Comme le montre la Figure 8, cela inclut les procédures stockées et fonctions définies par l’utilisateur, entre autres.

Laisser s démarrer en ajoutant une procédure stockée qui retourne simplement tous les produits qui ont été abandonnées. Nommez le nouveau fichier de la procédure stockée `GetDiscontinuedProducts.cs`.


[![Ajouter une nouvelle procédure stockée nommée GetDiscontinuedProducts.cs](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image13.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image12.png)

**Figure 8**: ajouter un nouveau stockées procédure nommée `GetDiscontinuedProducts.cs` ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image14.png))


Cela va créer un nouveau fichier de classe c# avec le contenu suivant :


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample2.cs)]

Notez que la procédure stockée est implémentée comme un `static` méthode au sein d’un `partial` fichier de classe nommé `StoredProcedures`. En outre, le `GetDiscontinuedProducts` méthode est décorée avec le `SqlProcedure attribute`, qui marque une méthode comme une procédure stockée.

Le code suivant crée un `SqlCommand` objet et les jeux de son `CommandText` à un `SELECT` requête qui retourne toutes les colonnes à partir de la `Products` de table pour les produits dont la propriété `Discontinued` champ est égal à 1. Il exécute la commande et renvoie les résultats à l’application cliente. Ajoutez ce code à la `GetDiscontinuedProducts` (méthode).


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample3.cs)]

Tous les objets de base de données managés ont accès à un [ `SqlContext` objet](https://msdn.microsoft.com/library/ms131108.aspx) qui représente le contexte de l’appelant. Le `SqlContext` fournit l’accès à un [ `SqlPipe` objet](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.aspx) via son [ `Pipe` propriété](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlcontext.pipe.aspx). Cela `SqlPipe` objet est utilisé pour transporter des informations entre la base de données SQL Server et l’application appelante. Comme son nom l’indique, le [ `ExecuteAndSend` méthode](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.executeandsend.aspx) exécute un passé dans `SqlCommand` objet et renvoie les résultats à l’application cliente.

> [!NOTE]
> Objets de base de données managés sont adaptées aux procédures stockées et des UDF qui utilisent la logique procédurale plutôt que logique basée sur un jeu. Logique procédurale implique l’utilisation de jeux de données sur une ligne par ligne de base ou utilisation avec des données scalaires. Le `GetDiscontinuedProducts` n’implique la méthode que nous venons de créer, toutefois, aucune logique procédurale. Par conséquent, elle serait dans l’idéal, être implémentée comme une procédure stockée T-SQL. Il est implémenté comme une procédure stockée managée pour illustrer les étapes nécessaires pour créer et déployer géré des procédures stockées.


## <a name="step-4-deploying-the-managed-stored-procedure"></a>Étape 4 : Déploiement managé d’une procédure stockée

Ce code terminé, nous sommes prêts à déployer dans la base de données Northwind. Déploiement d’un projet SQL Server compile le code dans un assembly, inscrit l’assembly avec la base de données et crée les objets correspondants dans la base de données, en les liant à des méthodes appropriées de l’assembly. Le jeu exact des tâches effectuées par l’option de déploiement est détaillée plus précisément à l’étape 13. Avec le bouton droit sur le `ManagedDatabaseConstructs` nom dans l’Explorateur de solutions du projet et choisissez l’option déployer. Toutefois, le déploiement échoue avec l’erreur suivante : syntaxe incorrecte près de « Externe ». Vous devrez peut-être définir le niveau de compatibilité de la base de données actuelle à une valeur plus élevée pour activer cette fonctionnalité. Consultez l’aide de la procédure stockée `sp_dbcmptlevel`.

Ce message d’erreur se produit lorsque vous tentez d’inscrire l’assembly avec la base de données Northwind. Pour inscrire un assembly avec une base de données SQL Server 2005, le niveau de compatibilité de base de données s doit être défini à 90. Par défaut, les nouvelles bases de données SQL Server 2005 ont un niveau de compatibilité 90. Toutefois, les bases de données créées à l’aide de Microsoft SQL Server 2000 ont un niveau de compatibilité par défaut 80. Dans la mesure où la base de données Northwind a été initialement une base de données Microsoft SQL Server 2000, son niveau de compatibilité est actuellement définie sur 80 et doit donc être augmentée à 90 pour enregistrer les objets de base de données managés.

Pour mettre à jour le niveau de compatibilité de base de données s, ouvrez une fenêtre de nouvelle requête dans Management Studio et entrez :


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample4.sql)]

Cliquez sur l’icône exécuter dans la barre d’outils pour exécuter la requête ci-dessus.


[![Mettre à jour le niveau de compatibilité de base de données Northwind s](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image16.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image15.png)

**Figure 9**: mettre à jour de la base de données Northwind s au niveau de compatibilité ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image17.png))


Après la mise à jour le niveau de compatibilité, redéployez le projet SQL Server. Cette fois le déploiement devrait se terminer sans erreur.

Revenir à SQL Server Management Studio, avec le bouton droit sur la base de données Northwind dans l’Explorateur d’objets et cliquez sur Actualiser. Ensuite, développez le dossier programmabilité et puis développez le dossier assemblys. Comme le montre la Figure 10, la base de données Northwind inclut désormais l’assembly généré par le `ManagedDatabaseConstructs` projet.


![Le ManagedDatabaseConstructs Assembly est maintenant inscrit auprès de la base de données Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image18.png)

**Figure 10**: le `ManagedDatabaseConstructs` Assembly est maintenant inscrit auprès de la base de données Northwind


Également développer le dossier Stored Procedures. Vous y trouverez une procédure stockée nommée `GetDiscontinuedProducts`. Cette procédure stockée a été créée par le processus de déploiement pointe vers le `GetDiscontinuedProducts` méthode dans le `ManagedDatabaseConstructs` assembly. Lorsque le `GetDiscontinuedProducts` procédure stockée est exécutée, elle, à son tour, exécute le `GetDiscontinuedProducts` (méthode). Dans la mesure où il s’agit d’une procédure stockée managée il ne peut pas être modifié via Management Studio (par conséquent, l’icône de verrou en regard du nom de la procédure stockée).


![La procédure stockée GetDiscontinuedProducts est répertoriée dans le dossier de procédures stockées](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image19.png)

**Figure 11**: le `GetDiscontinuedProducts` la procédure stockée est répertoriée dans le dossier de procédures stockées


Il existe toujours une entrave supplémentaire, nous avons à surmonter avant que nous pouvons appeler la procédure stockée managée : la base de données est configuré pour empêcher l’exécution du code managé. Le vérifier en ouvrant une nouvelle fenêtre de requête et de l’exécution de la `GetDiscontinuedProducts` procédure stockée. Vous recevrez le message d’erreur suivant : l’exécution du code utilisateur dans le .NET Framework est désactivée. Activez l’option de configuration de clr est activée.

Pour examiner les informations de configuration de base de données Northwind, entrez et exécutez la commande `exec sp_configure` dans la fenêtre de requête. Cela indique que le clr est activé en définissant est actuellement défini sur 0.


[![Le clr activé paramètre est actuellement défini sur 0](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image21.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image20.png)

**Figure 12**: le clr activé paramètre est actuellement défini sur 0 ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image22.png))


Notez que chaque paramètre de configuration dans la Figure 12 a quatre valeurs répertoriées avec celle-ci : la valeur minimale et valeurs maximales et la config et les valeurs d’exécution. Pour mettre à jour la valeur de configuration pour le paramètre clr activé, exécutez la commande suivante :


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample5.sql)]

Si vous exécutez de nouveau le `exec sp_configure` vous verrez que l’instruction ci-dessus mis à jour la valeur de configuration de paramètre clr activé s à 1, mais que la valeur d’exécution est toujours définie sur 0. Pour que cette modification de la configuration prenne effet, nous avons besoin exécuter le [ `RECONFIGURE` commande](https://msdn.microsoft.com/library/ms176069.aspx), qui définit la valeur d’exécution à la valeur actuelle de la configuration. Entrez simplement `RECONFIGURE` dans la fenêtre de requête et cliquez sur l’icône exécuter dans la barre d’outils. Si vous exécutez `exec sp_configure` maintenant vous devez voir une valeur de 1 pour la configuration de paramètre clr activé s et exécuter des valeurs.

La configuration de clr activé terminée, nous sommes prêts à exécuter managé `GetDiscontinuedProducts` procédure stockée. Dans la fenêtre de requête, entrez et exécutez la commande `exec` `GetDiscontinuedProducts`. Le code managé correspondant dans l’appel à la procédure stockée entraîne le `GetDiscontinuedProducts` méthode à exécuter. Ce code émet une `SELECT` requête pour retourner tous les produits qui sont abandonnés et retourne ces données à l’application appelante, qui est SQL Server Management Studio dans cette instance. Management Studio reçoit ces résultats et les affiche dans la fenêtre des résultats.


[![Le GetDiscontinuedProducts procédure stockée retourne tous les produits interrompus](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image24.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image23.png)

**Figure 13**: le `GetDiscontinuedProducts` stockées procédure retourne tous les abandonné produits ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image25.png))


## <a name="step-5-creating-managed-stored-procedures-that-accept-input-parameters"></a>Étape 5 : Création managé des procédures stockées qui acceptent des paramètres d’entrée

La plupart des requêtes et des procédures stockées que nous avons créé dans l’ensemble de ces didacticiels ont utilisé *paramètres*. Par exemple, dans le [création de nouvelles procédures stockées pour s DataSet typée TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) didacticiel, nous avons créé une procédure stockée nommée `GetProductsByCategoryID` qui accepté un paramètre d’entrée nommé `@CategoryID`. La procédure stockée, retournés tous les produits dont `CategoryID` champ correspondait à la valeur de l’élément `@CategoryID` paramètre.

Pour créer une procédure stockée managée qui accepte des paramètres d’entrée, vous devez simplement spécifier ces paramètres dans la définition de méthode s. Pour illustrer ceci, s permettent d’ajouter une autre procédure stockée managée pour le `ManagedDatabaseConstructs` projet nommé `GetProductsWithPriceLessThan`. Cette procédure stockée managée accepte un paramètre d’entrée, en spécifiant un prix et renverra tous les produits dont `UnitPrice` champ est inférieure à la valeur du paramètre s.

Pour ajouter une nouvelle procédure stockée pour le projet, cliquez sur le `ManagedDatabaseConstructs` nom du projet et choisissez d’ajouter une nouvelle procédure stockée. Nommez le fichier `GetProductsWithPriceLessThan.cs`. Comme nous l’avons vu à l’étape 3, cela créera un nouveau fichier de classe c# avec une méthode nommée `GetProductsWithPriceLessThan` placé dans le `partial` classe `StoredProcedures`.

Mise à jour le `GetProductsWithPriceLessThan` définition de méthode s afin qu’il accepte un [ `SqlMoney` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.aspx) paramètre d’entrée nommé `price` et écrire le code pour exécuter et retourner des résultats de la requête :


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample6.cs)]

Le `GetProductsWithPriceLessThan` définition de méthode s et le code ressemble étroitement à la définition et le code de la `GetDiscontinuedProducts` méthode créée à l’étape 3. Les seules différences sont que le `GetProductsWithPriceLessThan` méthode accepte comme paramètre d’entrée (`price`), la `SqlCommand` s requête contient un paramètre (`@MaxPrice`), et un paramètre est ajouté à la `SqlCommand` s `Parameters` est de la collection et la valeur affectée à la `price` variable.

Après avoir ajouté ce code, redéployer le projet SQL Server. Ensuite, revenez à SQL Server Management Studio et actualiser le dossier Stored Procedures. Vous devez voir une nouvelle entrée `GetProductsWithPriceLessThan`. À partir d’une fenêtre de requête, entrez et exécutez la commande `exec GetProductsWithPriceLessThan 25`, qui sera liste tous les produits inférieur à 25 $, comme le montre la Figure 14.


[![Produits sous 25 $ sont affichés.](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image27.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image26.png)

**Figure 14**: produits sous 25 $ sont affichés ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image28.png))


## <a name="step-6-calling-the-managed-stored-procedure-from-the-data-access-layer"></a>Étape 6 : Appel de la procédure stockée managée à partir de la couche d’accès aux données

À ce stade, nous avons ajouté la `GetDiscontinuedProducts` et `GetProductsWithPriceLessThan` gérés des procédures stockées pour le `ManagedDatabaseConstructs` de projet et ont les inscrits auprès de la base de données Northwind SQL Server. Nous avons appelé également ces procédures stockées managées à partir de SQL Server Management Studio (voir Figure s 13 et 14). Afin que notre ASP.NET gérées l’application pour utiliser ces procédures stockées, toutefois, nous avons besoin de les ajouter à l’accès aux données et les couches de logique métier dans l’architecture. Dans cette étape, nous allons ajouter deux nouvelles méthodes pour le `ProductsTableAdapter` dans le `NorthwindWithSprocs` DataSet typée, qui a été créé initialement dans le [création de nouvelles procédures stockées pour s DataSet typée TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) didacticiel. À l’étape 7, nous allons ajouter des méthodes correspondantes à la couche BLL.

Ouvrez le `NorthwindWithSprocs` DataSet typée dans Visual Studio, puis en ajoutant une nouvelle méthode à la `ProductsTableAdapter` nommé `GetDiscontinuedProducts`. Pour ajouter une nouvelle méthode à un TableAdapter, avec le bouton droit sur le nom de s TableAdapter dans le concepteur et choisissez l’option Ajouter une requête dans le menu contextuel.

> [!NOTE]
> Étant donné que nous avons déplacé la base de données Northwind à partir de la `App_Data` dossier à l’instance de base de données SQL Server 2005 Express Edition, il est impératif que la chaîne de connexion correspondante dans le fichier Web.config être mis à jour pour refléter cette modification. À l’étape 2, nous avons abordé la mise à jour le `NORTHWNDConnectionString` valeur dans `Web.config`. Si vous avez oublié de rendre cette mise à jour, vous verrez le message d’erreur Échec pour ajouter une requête. Impossible de trouver la connexion `NORTHWNDConnectionString` pour objet `Web.config` dans une boîte de dialogue lorsque vous tentez d’ajouter une nouvelle méthode au TableAdapter. Pour résoudre cette erreur, cliquez sur OK, puis accédez à `Web.config` et mettre à jour le `NORTHWNDConnectionString` valeur comme indiqué à l’étape 2. Puis essayez de rajouter la méthode au TableAdapter. Cette fois, cela devrait fonctionner sans erreur.


Ajout d’une nouvelle méthode lance l’Assistant Configuration de requêtes TableAdapter, ce qui nous avons utilisé plusieurs fois dans les didacticiels passées. La première étape nous demande pour spécifier comment le TableAdapter doit-il accéder à la base de données : via une instruction de SQL ad hoc ou une procédure stockée nouveau ou existante. Étant donné que nous avons déjà créé et inscrit le `GetDiscontinuedProducts` une procédure stockée managée avec la base de données, choisissez l’utiliser l’existant stocké l’option de procédure et appuyez sur Suivant.


[![Choisissez l’utilisation existante d’Option de procédure stockée](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image30.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image29.png)

**Figure 15**: choisissez l’utiliser l’existant de la procédure Option stockée ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image31.png))


L’écran suivant nous demande la procédure stockée qu'appelle la méthode. Choisissez le `GetDiscontinuedProducts` procédure stockée managée à partir de la liste déroulante et appuyez sur Suivant.


[![Sélectionnez le GetDiscontinuedProducts de procédure stockée managée](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image33.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image32.png)

**Figure 16**: sélectionnez le `GetDiscontinuedProducts` géré la procédure stockée ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image34.png))


Nous allons ensuite invités à spécifier si la procédure stockée retourne des lignes, une seule valeur ou rien. Dans la mesure où `GetDiscontinuedProducts` retourne l’ensemble de lignes supprimées de produit, choisissez la première option (données tabulaires), puis cliquez sur Suivant.


[![Sélectionnez l’Option de données tabulaires](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image36.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image35.png)

**Figure 17**: sélectionnez l’Option de données tabulaires ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image37.png))


L’écran finale de l’Assistant permet de spécifier les modèles d’accès aux données utilisées et les noms des méthodes qui en résulte. Laissez les cases à cocher activée et nom les méthodes `FillByDiscontinued` et `GetDiscontinuedProducts`. Cliquez sur Terminer pour terminer l’Assistant.


[![Nom de la FillByDiscontinued de méthodes et GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image39.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image38.png)

**Figure 18**: nommez les méthodes `FillByDiscontinued` et `GetDiscontinuedProducts` ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image40.png))


Répétez ces étapes pour créer des méthodes nommées `FillByPriceLessThan` et `GetProductsWithPriceLessThan` dans le `ProductsTableAdapter` pour le `GetProductsWithPriceLessThan` procédure stockée managée.

Figure 19 montre une capture d’écran du Concepteur de DataSet après avoir ajouté les méthodes à la `ProductsTableAdapter` pour le `GetDiscontinuedProducts` et `GetProductsWithPriceLessThan` gérés des procédures stockées.


[![Le ProductsTableAdapter inclut les nouvelles méthodes ajoutées à cette étape](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image42.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image41.png)

**Figure 19**: le `ProductsTableAdapter` inclut les méthodes ajoutées dans cette étape ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image43.png))


## <a name="step-7-adding-corresponding-methods-to-the-business-logic-layer"></a>Étape 7 : Ajout de méthodes correspondants à la couche de logique métier

Maintenant que nous avons mis à jour de la couche d’accès aux données pour inclure des méthodes pour appeler les procédures stockées managées ajoutés aux étapes 4 et 5, nous devons ajouter des méthodes correspondants à la couche de logique métier. Ajoutez les deux méthodes suivantes pour la `ProductsBLLWithSprocs` classe :


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample7.cs)]

Les deux méthodes simplement appeler la méthode correspondante de la couche DAL et retourner le `ProductsDataTable` instance. Le `DataObjectMethodAttribute` balisage au-dessus de chaque méthode provoque ces méthodes à inclure dans la liste déroulante dans l’onglet de sélection de l’Assistant Configurer la Source de données de s ObjectDataSource.

## <a name="step-8-invoking-the-managed-stored-procedures-from-the-presentation-layer"></a>Étape 8 : Appel managé procédures stockées à partir la couche de présentation

Avec la logique métier et les couches d’accès aux données augmenté pour inclure la prise en charge pour appeler le `GetDiscontinuedProducts` et `GetProductsWithPriceLessThan` gérés des procédures stockées, nous pouvons maintenant afficher ces stockés les résultats de procédures via une page ASP.NET.

Ouvrez le `ManagedFunctionsAndSprocs.aspx` page dans le `AdvancedDAL` dossier et, dans la boîte à outils, faites glisser un GridView sur le concepteur. Définir les opérations de mappage GridView `ID` propriété `DiscontinuedProducts` et, à partir de sa balise active, liez-le à une nouvelle ObjectDataSource nommé `DiscontinuedProductsDataSource`. Configurer l’ObjectDataSource afin d’extraire ses données à partir de la `ProductsBLLWithSprocs` classe s `GetDiscontinuedProducts` (méthode).


[![Configurer pour utiliser la classe ProductsBLLWithSprocs ObjectDataSource](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image45.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image44.png)

**Figure 20**: configurer ObjectDataSource à utiliser le `ProductsBLLWithSprocs` classe ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image46.png))


[![Choisissez la méthode GetDiscontinuedProducts dans la liste déroulante dans l’onglet Sélection](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image48.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image47.png)

**Figure 21**: choisissez le `GetDiscontinuedProducts` méthode dans la liste déroulante dans l’onglet à sélectionner ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image49.png))


Dans la mesure où cette grille servira d’afficher simplement des informations de produit, définir les listes déroulantes dans la mise à jour, insertion et supprimer des onglets à (None) et puis cliquez sur Terminer.

À la fin de l’Assistant, Visual Studio ajoute automatiquement un BoundField ou du CheckBoxField pour chaque champ de données dans le `ProductsDataTable`. Prenez un moment pour supprimer tous ces champs à l’exception de `ProductName` et `Discontinued`, l’endroit où votre GridView et balisage déclaratif de s ObjectDataSource doit ressembler à ce qui suit :


[!code-aspx[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample8.aspx)]

Prenez un moment pour afficher cette page via un navigateur. Lorsque la page est visitée, les appels de l’ObjectDataSource le `ProductsBLLWithSprocs` classe s `GetDiscontinuedProducts` (méthode). Comme nous l’avons vu à l’étape 7, cette méthode appelle vers le bas de la couche DAL s `ProductsDataTable` classe s `GetDiscontinuedProducts` (méthode), qui appelle le `GetDiscontinuedProducts` procédure stockée. Cette procédure stockée est une procédure stockée et non managées s’exécute le code que nous avons créé à l’étape 3, en retournant les produits interrompus.

Les résultats retournés par la procédure stockée managée sont regroupés dans un `ProductsDataTable` par la couche DAL et retournées à la couche BLL, qui puis les retourne à la couche de présentation, où elles sont liées au GridView et affichées. Comme prévu, la grille répertorie les produits qui ont été abandonnées.


[![Les produits supprimées sont répertoriées.](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image51.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image50.png)

**Figure 22**: les produits supprimées sont répertoriés ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image52.png))


Pour plus de vous entraîner, ajoutez un contrôle TextBox et GridView une autre à la page. Avoir ce GridView affiche les produits inférieure à la quantité d’entrées dans la zone de texte en appelant le `ProductsBLLWithSprocs` classe s `GetProductsWithPriceLessThan` (méthode).

## <a name="step-9-creating-and-calling-t-sql-udfs"></a>Étape 9 : Création et appel de fonctions UDF T-SQL

Fonctions définies par l’utilisateur ou UDF, sont la base de données objets étroitement imiter la sémantique des fonctions dans les langages de programmation. Comme une fonction dans c#, UDF peuvent inclure un nombre variable de paramètres d’entrée et retourner une valeur d’un type particulier. Une fonction UDF peut retourner des données scalaires - une chaîne, un entier et ainsi de suite - ou données tabulaires. Laissez s jeter un coup de œil sur les deux types de fichiers UDF, en commençant par une fonction UDF qui retourne un type de données scalaire.

La fonction UDF suivante calcule la valeur estimée de l’inventaire d’un produit spécifique. Pour ce faire, la prise de trois paramètres d’entrée - le `UnitPrice`, `UnitsInStock`, et `Discontinued` valeurs pour un produit particulier - et retourne une valeur de type `money`. Il calcule la valeur estimée de l’inventaire en multipliant le `UnitPrice` par le `UnitsInStock`. Pour les articles abandonnés, cette valeur est réduit de moitié.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample9.sql)]

Une fois cette UDF a été ajoutée à la base de données, il est accessible via Management Studio, développez le dossier programmabilité, puis les fonctions et les fonctions de valeur scalaire. Il peut être utilisé dans un `SELECT` requête comme suit :


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample10.sql)]

J’ai ajouté le `udf_ComputeInventoryValue` UDF à la base de données Northwind ; Figure 23 montre la sortie de la méthode ci-dessus `SELECT` interroger lorsqu’ils sont affichés via Management Studio. Notez également que la fonction UDF est répertoriée sous le dossier de fonctions de valeur scalaire dans l’Explorateur d’objets.


[![Chaque produit s valeurs de l’inventaire est répertorié.](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image54.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image53.png)

**Figure 23**: s de chaque produit des valeurs de stock est répertorié ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image55.png))


UDF peuvent également retourner des données tabulaires. Par exemple, nous pouvons créer une fonction UDF qui retourne des produits qui appartiennent à une catégorie particulière :


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample11.sql)]

Le `udf_GetProductsByCategoryID` UDF accepte un `@CategoryID` paramètre d’entrée et retourne les résultats de l’objet `SELECT` requête. Une fois créé, cette fonction UDF peut être référencée dans le `FROM` (ou `JOIN`) clause d’une `SELECT` requête. L’exemple suivant retourne le `ProductID`, `ProductName`, et `CategoryID` valeurs pour chacun des boissons.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample12.sql)]

J’ai ajouté le `udf_GetProductsByCategoryID` UDF à la base de données Northwind ; Figure 24 montre la sortie de la méthode ci-dessus `SELECT` interroger lorsqu’ils sont affichés via Management Studio. Vous trouverez des UDF qui retourne des données tabulaires dans le dossier de fonctions de valeur de la Table s Explorateur d’objets.


[![Le ProductID, ProductName et CategoryID sont répertoriés pour chaque boissons](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image57.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image56.png)

**Figure 24**: le `ProductID`, `ProductName`, et `CategoryID` sont répertoriés pour chaque boisson ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image58.png))


> [!NOTE]
> Pour plus d’informations sur la création et l’utilisation des UDF, consultez [introduction sur les fonctions définies par l’utilisateur](http://www.sqlteam.com/item.asp?ItemID=1955). Découvrez également [avantages et les fonctions de Drawbacks of User-Defined](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1).


## <a name="step-10-creating-a-managed-udf"></a>Étape 10 : Création d’un code UDF géré

Le `udf_ComputeInventoryValue` et `udf_GetProductsByCategoryID` UDF créées dans les exemples ci-dessus est des objets de base de données de T-SQL. SQL Server 2005 prend également en charge UDF gérés, ce qui peuvent être ajoutés à la `ManagedDatabaseConstructs` projet comme managé des procédures stockées à partir des étapes 3 et 5. Pour cette étape, laisser s implémenter le `udf_ComputeInventoryValue` UDF dans le code managé.

Pour ajouter un code UDF géré pour le `ManagedDatabaseConstructs` de projet, avec le bouton droit sur le nom du projet dans l’Explorateur de solutions et choisissez d’ajouter un nouvel élément. Sélectionnez le modèle défini par l’utilisateur à partir de la boîte de dialogue Ajouter un nouvel élément et nommez le nouveau fichier UDF `udf_ComputeInventoryValue_Managed.cs`.


[![Ajouter un nouveau code UDF géré pour le projet ManagedDatabaseConstructs](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image60.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image59.png)

**Figure 25**: ajouter un nouveau UDF géré pour le `ManagedDatabaseConstructs` projet ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image61.png))


Le modèle de fonction de définis par l’utilisateur crée un `partial` classe nommée `UserDefinedFunctions` avec une méthode dont le nom est le même que le nom de fichier s classe (`udf_ComputeInventoryValue_Managed`, dans cette instance). Cette méthode est décorée à l’aide de la [ `SqlFunction` attribut](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlfunctionattribute.aspx), qui marque la méthode comme un code UDF géré.


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample13.cs)]

Le `udf_ComputeInventoryValue` méthode retourne actuellement un [ `SqlString` objet](https://msdn.microsoft.com/library/system.data.sqltypes.sqlstring.aspx) et n’accepte pas les paramètres d’entrée. Nous devons mettre à jour la définition de méthode afin qu’elle accepte trois paramètres - d’entrée `UnitPrice`, `UnitsInStock`, et `Discontinued` - et retourne un `SqlMoney` objet. La logique pour calculer la valeur d’inventaire est identique à celui dans le code T-SQL `udf_ComputeInventoryValue` UDF.


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample14.cs)]

Notez que les paramètres d’entrée de méthode s UDF sont de leurs types SQL correspondants : `SqlMoney` pour le `UnitPrice` champ, [ `SqlInt16` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlint16.aspx) pour `UnitsInStock`, et [ `SqlBoolean` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlboolean.aspx) pour `Discontinued`. Ces types de données reflètent les types définis dans le `Products` table : le `UnitPrice` colonne est de type `money`, le `UnitsInStock` colonne de type `smallint`et le `Discontinued` colonne de type `bit`.

Le code commence par créer un `SqlMoney` instance nommée `inventoryValue` qui est assigné à la valeur 0. Le `Products` permet de table de base de données `NULL` des valeurs dans le `UnitsInPrice` et `UnitsInStock` colonnes. Par conséquent, nous devons vérifier pour voir si ces valeurs contiennent `NULL` s, ce que nous faisons via la `SqlMoney` objet s [ `IsNull` propriété](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.isnull.aspx). Si les deux `UnitPrice` et `UnitsInStock` contiennent non -`NULL` des valeurs, puis nous calculons la `inventoryValue` le produit des deux. Ensuite, si `Discontinued` a la valeur true, nous réduire de moitié la valeur.

> [!NOTE]
> Le `SqlMoney` objet autorise uniquement deux `SqlMoney` instances pour être multipliés. Il n’autorise pas un `SqlMoney` instance doit être multipliée par un nombre à virgule flottante littéral. Par conséquent, pour réduire de moitié `inventoryValue` nous multipliez-le par un nouvel `SqlMoney` instance qui a la valeur 0,5.


## <a name="step-11-deploying-the-managed-udf"></a>Étape 11 : Déploiement de l’UDF géré

Maintenant que le code UDF géré a été créé, nous sommes prêts à déployer dans la base de données Northwind. Comme nous l’avons vu à l’étape 4, les objets gérés dans un projet SQL Server sont déployés en cliquant sur le nom du projet dans l’Explorateur de solutions et en choisissant l’option déployer dans le menu contextuel.

Une fois que vous avez déployé le projet, revenez à SQL Server Management Studio et actualisez le dossier de fonctions scalaires. Vous devez maintenant voir deux entrées :

- `dbo.udf_ComputeInventoryValue` -l’UDF T-SQL créé à l’étape 9, et
- `dbo.udf ComputeInventoryValue_Managed` -le code UDF géré créé à l’étape 10 que vous venez de déployer.

Pour tester ce code UDF géré, exécutez la requête suivante à partir de Management Studio :


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample15.sql)]

Cette commande utilise le managé `udf ComputeInventoryValue_Managed` UDF au lieu du T-SQL `udf_ComputeInventoryValue` UDF, mais la sortie est la même. Reportez-vous à la Figure 23 pour voir une capture d’écran de la sortie de s UDF.

## <a name="step-12-debugging-the-managed-database-objects"></a>Étape 12 : Déboguer les objets de base de données managés

Dans le [le débogage des procédures stockées](debugging-stored-procedures-cs.md) didacticiel, nous avons abordé les trois options pour le débogage SQL Server via Visual Studio : débogage Direct de base de données, le débogage de l’Application et le débogage à partir d’un projet SQL Server. Géré de base de données objets ne peut pas être débogués via débogage Direct de base de données, mais ils peuvent être débogués à partir d’une application cliente et directement depuis le projet SQL Server. Pour le débogage fonctionne, toutefois, la base de données SQL Server 2005 doit autoriser le débogage SQL/CLR. N’oubliez pas que, lorsque nous avons tout d’abord créé le `ManagedDatabaseConstructs` projet Visual Studio nous avez demandé si nous souhaitons activer le débogage (voir Figure 6 à l’étape 2) SQL/CLR. Ce paramètre peut être modifié en cliquant sur la base de données à partir de la fenêtre Explorateur de serveurs.


![Vérifiez que la base de données autorise le débogage SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image62.png)

**Figure 26**: Vérifiez que la base de données autorise le débogage SQL/CLR


Supposons que nous voulons déboguer le `GetProductsWithPriceLessThan` procédure stockée managée. Nous commencerions en définissant un point d’arrêt dans le code de la `GetProductsWithPriceLessThan` (méthode).


[![Définir un point d’arrêt dans la méthode GetProductsWithPriceLessThan](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image64.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image63.png)

**Figure 27**: définir un point d’arrêt dans le `GetProductsWithPriceLessThan` (méthode) ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image65.png))


Laissez s Examinez d’abord les objets de base de données géré à partir du projet de serveur SQL de débogage. Étant donné que notre Solution comprend deux projets : le `ManagedDatabaseConstructs` projet SQL Server, ainsi que notre site Web Azure, afin de déboguer à partir du projet SQL Server, nous devons indiquer à Visual Studio pour lancer le `ManagedDatabaseConstructs` projet lorsque nous commencerons le débogage de SQL Server. Cliquez sur le `ManagedDatabaseConstructs` projet dans l’Explorateur de solutions, puis sélectionnez le jeu en tant qu’option de projet de démarrage dans le menu contextuel.

Lorsque le `ManagedDatabaseConstructs` projet est lancé à partir du débogueur, il exécute les instructions SQL dans le `Test.sql` fichier, ce qui se trouve dans le `Test Scripts` dossier. Par exemple, pour tester le `GetProductsWithPriceLessThan` gérés d’une procédure stockée, remplacer la `Test.sql` fichier de contenu avec l’instruction suivante, qui appelle le `GetProductsWithPriceLessThan` en passant de procédure stockée managée la `@CategoryID` valeur 14,95 :


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample16.sql)]

Une fois que vous avez déjà entré le script ci-dessus dans `Test.sql`, démarrez le débogage en accédant au menu Déboguer et en choisissant de démarrer le débogage ou en appuyant sur F5 ou icône de lecture vert dans la barre d’outils. Cela sera générer les projets dans la Solution, déployer les objets de base de données managé pour la base de données Northwind, puis exécutez le `Test.sql` script. À ce stade, le point d’arrêt est atteint et que nous pouvons examiner la `GetProductsWithPriceLessThan` méthode, examinez les valeurs des paramètres d’entrée et ainsi de suite.


[![Le point d’arrêt dans la méthode GetProductsWithPriceLessThan a été atteint.](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image67.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image66.png)

**Figure 28**: le point d’arrêt dans le `GetProductsWithPriceLessThan` méthode a été atteint ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image68.png))


Dans l’ordre pour un objet de base de données SQL à déboguer une application cliente, il est impératif que la base de données configurée pour prendre en charge le débogage de l’application. Avec le bouton droit sur la base de données dans l’Explorateur de serveurs et assurez-vous que l’option de débogage de l’Application est activée. En outre, nous devons configurer l’application ASP.NET d’intégrer avec le débogueur SQL et de désactiver le regroupement de connexions. Ces étapes ont été abordés en détail à l’étape 2 de la [le débogage des procédures stockées](debugging-stored-procedures-cs.md) didacticiel.

Une fois que vous avez configuré l’application ASP.NET et la base de données, définir le site Web ASP.NET en tant que projet de démarrage et démarrer le débogage. Si vous visitez une page qui appelle l’une de ces objets ayant un point d’arrêt, l’application s’arrête et le contrôle est activé le débogueur, où vous pouvez parcourir le code comme indiqué dans la Figure 28.

## <a name="step-13-manually-compiling-and-deploying-managed-database-objects"></a>Étape 13 : Compilation et déploiement gérés manuellement les objets de base de données

Projets SQL Server facilitent la création, compiler et déployer des objets de base de données managés. Malheureusement, les projets SQL Server sont uniquement disponibles dans les éditions Professional et les systèmes de l’équipe de Visual Studio. Si vous utilisez Visual Web Developer ou l’Édition Standard de Visual Studio et à utiliser des objets de base de données managés, vous devez manuellement créer et de les déployer. Cela implique quatre étapes :

1. Créer un fichier qui contient le code source pour l’objet de base de données managés,
2. Compilez l’objet dans un assembly,
3. Inscrire l’assembly avec la base de données SQL Server 2005, et
4. Créez un objet de base de données dans SQL Server qui pointe vers la méthode appropriée dans l’assembly.

Pour illustrer ces tâches, permettent de créer un nouveau s gérés procédure stockée qui retourne les produits dont `UnitPrice` est supérieure à une valeur spécifiée. Créer un nouveau fichier sur votre ordinateur nommé `GetProductsWithPriceGreaterThan.cs` et entrez le code suivant dans le fichier (vous pouvez utiliser Visual Studio, le bloc-notes ou un éditeur de texte pour effectuer cette opération) :


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample17.cs)]

Ce code est presque identique à celle de la `GetProductsWithPriceLessThan` méthode créée à l’étape 5. Les seules différences sont les noms de méthode, le `WHERE` clause et le nom du paramètre utilisé dans la requête. Dans le `GetProductsWithPriceLessThan` (méthode), le `WHERE` clause lire : `WHERE UnitPrice < @MaxPrice`. Ici, dans `GetProductsWithPriceGreaterThan`, nous utilisons : `WHERE UnitPrice > @MinPrice` .

Nous devons maintenant compiler cette classe dans un assembly. À partir de la ligne de commande, accédez au répertoire où vous avez enregistré le `GetProductsWithPriceGreaterThan.cs` de fichiers et d’utiliser le compilateur c# (`csc.exe`) pour compiler le fichier de classe dans un assembly :


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample18.cmd)]

Si le dossier contenant `csc.exe` dans pas dans le système s `PATH`, vous devrez référencer entièrement son chemin d’accès, `%WINDOWS%\Microsoft.NET\Framework\version\`, comme suit :


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample19.cmd)]


[![Compiler GetProductsWithPriceGreaterThan.cs dans un Assembly](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image70.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image69.png)

**Figure 29**: compiler `GetProductsWithPriceGreaterThan.cs` dans un Assembly ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image71.png))


Le `/t` indicateur spécifie que le fichier de classe c# doit être compilé dans une DLL (au lieu d’un fichier exécutable). Le `/out` indicateur spécifie le nom de l’assembly résultant.

> [!NOTE]
> Au lieu de compiler le `GetProductsWithPriceGreaterThan.cs` fichier de classe à partir de la ligne de commande que vous pouvez également utiliser [Visual c# Express Edition](https://msdn.microsoft.com/vstudio/express/visualcsharp/) ou créer un projet de bibliothèque de classes distinct dans Visual Studio Standard Edition. S ren Jacob Lauritsen a fourni bien vouloir tel un projet Visual c# Express Edition avec le code pour le `GetProductsWithPriceGreaterThan` de procédures stockées et les deux gérés des procédures stockées et UDF créé lors des étapes 3, 5 et 10. Projet de s S ren inclut également les commandes T-SQL nécessaires pour ajouter les objets de base de données correspondante.


Le code compilé dans un assembly, nous sommes prêts à inscrire l’assembly dans la base de données SQL Server 2005. Cela peut être effectué via T-SQL, à l’aide de la commande `CREATE ASSEMBLY`, ou via SQL Server Management Studio. Laissez le focus s à l’aide de Management Studio.

À partir de Management Studio, développez le dossier programmabilité dans la base de données Northwind. Un de ses sous-dossier est assemblys. Pour ajouter manuellement un nouvel Assembly à la base de données, avec le bouton droit sur le dossier Assemblies et choisissez nouvel Assembly dans le menu contextuel. Cette affiche la boîte de dialogue Assembly nouvelle zone (voir la Figure 30). Cliquez sur le bouton Parcourir, sélectionnez le `ManuallyCreatedDBObjects.dll` assembly nous vient de compiler, puis cliquez sur OK pour ajouter l’Assembly à la base de données. Vous ne verrez pas le `ManuallyCreatedDBObjects.dll` assembly dans l’Explorateur d’objets.


[![Ajouter l’Assembly ManuallyCreatedDBObjects.dll à la base de données](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image73.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image72.png)

**Figure 30**: ajouter la `ManuallyCreatedDBObjects.dll` Assembly à la base de données ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image74.png))


![Le ManuallyCreatedDBObjects.dll est répertorié dans l’Explorateur d’objets](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image75.png)

**Figure 31**: le `ManuallyCreatedDBObjects.dll` est répertorié dans l’Explorateur d’objets


Bien que nous avons ajouté l’assembly à la base de données Northwind, nous devons encore associer une procédure stockée avec la `GetProductsWithPriceGreaterThan` méthode dans l’assembly. Pour ce faire, ouvrez une nouvelle fenêtre de requête et exécutez le script suivant :


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample20.sql)]

Cela crée une nouvelle procédure stockée dans la base de données Northwind nommé `GetProductsWithPriceGreaterThan` et l’associe à la méthode managée `GetProductsWithPriceGreaterThan` (qui se trouve dans la classe `StoredProcedures`, qui se trouve dans l’assembly `ManuallyCreatedDBObjects`).

Après avoir exécuté le script ci-dessus, actualisez le dossier de procédures stockées dans l’Explorateur d’objets. Vous devez voir une nouvelle entrée de la procédure stockée - `GetProductsWithPriceGreaterThan` -qui a une icône de verrou en regard de celle-ci. Pour tester cette procédure stockée, entrez et exécutez le script suivant dans la fenêtre de requête :


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample21.sql)]

Comme le montre la Figure 32, la commande ci-dessus affiche des informations pour les produits avec une `UnitPrice` supérieur 24,95.


[![Le ManuallyCreatedDBObjects.dll est répertorié dans l’Explorateur d’objets](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image77.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image76.png)

**Figure 32**: le `ManuallyCreatedDBObjects.dll` est répertorié dans l’Explorateur d’objets ([cliquez pour afficher l’image en taille réelle](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image78.png))


## <a name="summary"></a>Récapitulatif

Microsoft SQL Server 2005 s’intègre avec le Common Language Runtime (CLR), ce qui permet aux objets de base de données d’être créé à l’aide de code managé. Auparavant, ces objets de base de données peuvent uniquement être créés à l’aide de T-SQL, mais nous pouvons maintenant créer ces objets à l’aide de la programmation des langages tels que c# .NET. Dans ce didacticiel, que nous avons créé deux gérés des procédures stockées et une fonction définie par l’utilisateur géré.

Visual Studio s type de projet SQL Server facilite la création, la compilation et déploiement d’objets de base de données managés. En outre, il offre la prise en charge du débogage enrichi. Toutefois, les types de projet SQL Server sont uniquement disponibles dans les éditions Professional et les systèmes de l’équipe de Visual Studio. Pour ceux à l’aide de Visual Web Developer ou l’Édition Standard de Visual Studio, la création, compilation et étapes de déploiement doit être effectuée manuellement, comme nous l’avons vu à l’étape 13.

Bonne programmation !

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Avantages et inconvénients des fonctions définies par l’utilisateur](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)
- [Création d’objets SQL Server 2005 dans le Code managé](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Création de déclencheurs à l’aide de Code managé dans SQL Server 2005](http://www.15seconds.com/issue/041006.htm)
- [Comment : Créer et exécuter un CLR SQL Server de procédure stockée](https://msdn.microsoft.com/library/5czye81z(VS.80).aspx)
- [Comment : Créer et exécuter une fonction définie par l’utilisateur de CLR SQL Server](https://msdn.microsoft.com/library/w2kae45k(VS.80).aspx)
- [Comment : Modifier le `Test.sql` Script à exécuter des objets SQL](https://msdn.microsoft.com/library/ms233682(VS.80).aspx)
- [Fonctions définies par l’introduction à l’utilisateur](http://www.sqlteam.com/item.asp?ItemID=1955)
- [Le Code managé et SQL Server 2005 (vidéo)](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Référence Transact-SQL](https://msdn.microsoft.com/library/aa299742(SQL.80).aspx)
- [Procédure pas à pas : Création d’une procédure stockée dans le Code managé](https://msdn.microsoft.com/library/zxsa8hkf(VS.80).aspx)

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Remerciements

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été S ren Jacob Lauritsen. En plus d’étudier cet article, le S ren également créé le projet Visual c# Express Edition inclus dans ce téléchargement de l’article %s pour la compilation manuellement les objets de base de données managés. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](debugging-stored-procedures-cs.md)
> [Suivant](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
