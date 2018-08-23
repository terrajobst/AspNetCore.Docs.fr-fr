---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
title: Débogage des procédures stockées (c#) | Microsoft Docs
author: rick-anderson
description: Les éditions Visual Studio Professional et Team System permettent de définir des points d’arrêt et de participer à des procédures stockées dans SQL Server, ainsi tout débogage stockées...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: c655c324-2ffa-4c21-8265-a254d79a693d
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
msc.type: authoredcontent
ms.openlocfilehash: a377dde0c4ed25aed549ca1f3b8eeeecece79517
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826181"
---
<a name="debugging-stored-procedures-c"></a>Débogage des procédures stockées (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_74_CS.zip) ou [télécharger le PDF](debugging-stored-procedures-cs/_static/datatutorial74cs1.pdf)

> Les éditions Visual Studio Professional et Team System vous permettent de définir des points d’arrêt et de participer à des procédures stockées dans SQL Server, permettent de faciliter le débogage des procédures stockées aussi simples que le débogage de code d’application. Ce didacticiel illustre le débogage direct de la base de données et débogage de l’application des procédures stockées.


## <a name="introduction"></a>Introduction

Visual Studio fournit une riche expérience de débogage. Avec quelques séquences de touches ou des clics de souris, il s possible d’utiliser des points d’arrêt pour arrêter l’exécution d’un programme et examiner son état et contrôle de flux. En même temps que le débogage du code d’application, Visual Studio propose la prise en charge pour le débogage des procédures stockées à partir de SQL Server. Tout comme la peuvent de définir des points d’arrêt dans le code d’une classe code-behind d’ASP.NET ou d’une classe de la couche de logique métier, donc trop peuvent leur être placés dans des procédures stockées.

Dans ce didacticiel, nous allons examiner pas à pas détaillé dans les procédures stockées à partir de l’Explorateur de serveurs dans Visual Studio, ainsi que la manière dont pour définir des points d’arrêt qui sont atteints lorsque la procédure stockée est appelée à partir de l’application ASP.NET en cours d’exécution.

> [!NOTE]
> Malheureusement, les procédures stockées peuvent uniquement être détaillés et de débogage via les versions Professional et les systèmes de l’équipe de Visual Studio. Si vous utilisez Visual Web Developer ou la version standard de Visual Studio, vous avez la possibilité de lire le long que nous vous guidons à travers les étapes nécessaires pour déboguer les procédures stockées, mais vous ne serez pas en mesure de répliquer ces étapes sur votre ordinateur.


## <a name="sql-server-debugging-concepts"></a>Concepts de débogage SQL Server

Microsoft SQL Server 2005 a été conçu pour offrir une intégration avec le [Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), qui est le runtime utilisé par tous les assemblys .NET. Par conséquent, SQL Server 2005 prend en charge les objets de base de données managés. Autrement dit, vous pouvez créer des objets de base de données tels que des procédures stockées et fonctions définies par l’utilisateur (UDF) en tant que méthodes dans une classe c#. Ainsi, ces procédures stockées et des UDF pour exploiter les fonctionnalités dans le .NET Framework et à partir de vos propres classes personnalisées. Bien entendu, SQL Server 2005 prend également en charge pour les objets de base de données de T-SQL.

SQL Server 2005 offre la prise en charge du débogage pour T-SQL et les objets de base de données managés. Toutefois, ces objets peuvent uniquement être débogués via les éditions de Visual Studio 2005 Professional et les systèmes de l’équipe. Dans ce didacticiel, nous allons examiner les objets de base de données débogage T-SQL. Le didacticiel suivant ressemble au débogage des objets de base de données managés.

Le [vue d’ensemble de T-SQL et de débogage CLR dans SQL Server 2005](https://blogs.msdn.com/sqlclr/archive/2006/06/29/651644.aspx) entrée de blog à partir de la [équipe d’intégration CLR de SQL Server 2005](https://blogs.msdn.com/sqlclr/default.aspx) met en surbrillance les trois façons de déboguer des objets de SQL Server 2005 à partir de Visual Studio :

- **Diriger le débogage de base de données (DDD)** : dans l’Explorateur de serveurs que nous pouvons détaillé dans n’importe quel objet de base de données T-SQL, telles que des procédures stockées et des UDF. Nous allons examiner la conception pilotée par domaine à l’étape 1.
- **Le débogage d’application** -nous pouvons définir des points d’arrêt dans un objet de base de données et ensuite exécuter notre application ASP.NET. Lorsque l’objet de base de données est exécuté, le point d’arrêt est atteint et le contrôle est activé le débogueur. Notez qu’avec le débogage de l’application nous ne pouvons pas l’étape dans un objet de base de données à partir du code d’application. Nous devons définir explicitement des points d’arrêt dans ces procédures stockées ou des UDF où nous voulons le débogueur s’arrête. Débogage de l’application est examiné en commençant à l’étape 2.
- **Le débogage à partir d’un projet SQL Server** -éditions Visual Studio Professional et les systèmes de l’équipe incluent un type de projet SQL Server qui est couramment utilisé pour créer des objets de base de données managés. Nous allons examiner à l’aide de projets SQL Server et le débogage de leur contenu dans le didacticiel suivant.

Visual Studio peut déboguer les procédures stockées sur des instances de SQL Server locaux et distants. Une instance de SQL Server locale est est installé sur le même ordinateur que Visual Studio. Si la base de données SQL Server que vous utilisez n’est pas situé sur votre ordinateur de développement, il est considéré comme une instance distante. Pour ces didacticiels nous avons été à l’aide des instances de SQL Server locales. Débogage des procédures stockées sur une instance distante de SQL server nécessite des étapes de configuration supplémentaires que lorsque le débogage des procédures stockées sur une instance locale.

Si vous utilisez une instance de SQL Server locale, vous pouvez commencer à l’étape 1 et parcourez ce didacticiel à la fin. Si vous utilisez une instance distante de SQL Server, toutefois, vous ne serez devez d’abord pour vous assurer que lorsque vous déboguez votre sont enregistrés dans votre ordinateur de développement avec un compte d’utilisateur Windows qui dispose d’une connexion de SQL Server sur l’instance distante. Moveover, cette connexion de base de données et la connexion de base de données utilisée pour se connecter à la base de données à partir de l’application ASP.NET en cours d’exécution doit être membres de la `sysadmin` rôle. Consultez le débogage T-SQL de base de données d’objets dans la section des Instances distantes à la fin de ce didacticiel pour plus d’informations sur la configuration de Visual Studio et SQL Server pour déboguer une instance distante.

Enfin, comprendre que la prise en charge pour les objets de base de données de T-SQL de débogage n’est pas en tant que fonctionnalité riche que le débogage de la prise en charge pour les applications .NET. Par exemple, les conditions de point d’arrêt et les filtres ne sont pas pris en charge, uniquement un sous-ensemble des fenêtres de débogage sont disponibles, vous ne pouvez pas utiliser Modifier & Continuer, la fenêtre exécution est rendue inutilisable et ainsi de suite. Consultez [Limitations sur les fonctionnalités et les commandes de débogueur](https://msdn.microsoft.com/library/ms165035(VS.80).aspx) pour plus d’informations.

## <a name="step-1-directly-stepping-into-a-stored-procedure"></a>Étape 1 : Directement pas à pas détaillé dans une procédure stockée

Visual Studio rend plus facile à déboguer directement un objet de base de données. Laisser s étudier comment utiliser la fonctionnalité de débogage de base de données Direct (DDD) à parcourir le `Products_SelectByCategoryID` procédure stockée dans la base de données Northwind. Comme son nom l’indique, `Products_SelectByCategoryID` retourne des informations de produit pour une catégorie particulière ; il a été créé dans le [à l’aide des procédures stockées existantes pour s DataSet typée TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) didacticiel. Démarrez en accédant à l’Explorateur de serveurs et développez le nœud de base de données Northwind. Ensuite, développez le dossier procédures stockées, avec le bouton droit sur le `Products_SelectByCategoryID` procédure stockée, puis choisissez l’option étape dans une procédure stockée dans le menu contextuel. Ceci démarrera le débogueur.

Dans la mesure où le `Products_SelectByCategoryID` procédure stockée attend un `@CategoryID` paramètre d’entrée, nous devons fournir cette valeur. Entrez 1, ce qui retourne des informations sur les boissons.


![Utilisez la valeur 1 pour le @CategoryID paramètre](debugging-stored-procedures-cs/_static/image1.png)

**Figure 1**: utilisez la valeur 1 pour le `@CategoryID` paramètre


Après avoir fourni la valeur pour le `@CategoryID` paramètre, la procédure stockée est exécutée. Au lieu d’exécuter jusqu'à la fin, cependant, le débogueur interrompt l’exécution à la première instruction. Notez la flèche jaune dans la marge, indiquant l’emplacement actuel dans la procédure stockée. Vous pouvez afficher et modifier les valeurs de paramètre via la fenêtre Espion ou en pointant sur le nom du paramètre dans la procédure stockée.


[![Le débogueur a interrompu sur la première instruction de la procédure stockée](debugging-stored-procedures-cs/_static/image3.png)](debugging-stored-procedures-cs/_static/image2.png)

**Figure 2**: le débogueur s’est arrêté à la première instruction de la procédure stockée ([cliquez pour afficher l’image en taille réelle](debugging-stored-procedures-cs/_static/image4.png))


Pour parcourir l’instruction d’une procédure stockée à la fois, cliquez sur le bouton pas à pas principal dans la barre d’outils ou appuyez sur la touche F10. Le `Products_SelectByCategoryID` procédure stockée contienne un seul `SELECT` instruction, afin d’appuyer sur F10 sera survol de l’instruction unique et terminer l’exécution de la procédure stockée. À l’issue de la procédure stockée, sa sortie s’affiche dans la fenêtre Sortie et le débogueur s’arrête.

> [!NOTE]
> Le débogage T-SQL se produit au niveau de l’instruction ; Vous ne pouvez pas entrer dans un `SELECT` instruction.


## <a name="step-2-configuring-the-website-for-application-debugging"></a>Étape 2 : Configuration du site Web pour le débogage de l’Application

Pendant le débogage d’une procédure stockée directement à partir de l’Explorateur de serveurs est pratique, dans de nombreux scénarios, nous sommes plus intéressés par le débogage de la procédure stockée lorsqu’elle est appelée à partir de notre application ASP.NET. Nous pouvons ajouter des points d’arrêt à une procédure stockée à partir de Visual Studio, puis démarrez le débogage de l’application ASP.NET. Lorsqu’une procédure stockée avec des points d’arrêt est appelée à partir de l’application, l’exécution s’arrêtera au point d’arrêt et nous pouvons afficher et modifier les valeurs de paramètre de procédure stockée s et parcourir ses États, comme nous l’avons fait à l’étape 1.

Avant que nous pouvons commencer le débogage des procédures stockées appelées à partir de l’application, nous devons indiquer à l’application web ASP.NET à intégrer avec le débogueur SQL Server. Démarrer en cliquant sur le nom du site Web dans l’Explorateur de solutions (`ASPNET_Data_Tutorial_74_CS`). Choisissez l’option de Pages de propriétés dans le menu contextuel, sélectionnez l’élément Options de démarrage sur la gauche et cochez la case de SQL Server dans la section débogueurs (voir Figure 3).


[![Cochez la case à cocher du serveur SQL dans les Pages de propriétés d’Application s](debugging-stored-procedures-cs/_static/image6.png)](debugging-stored-procedures-cs/_static/image5.png)

**Figure 3**: cochez la case à cocher du serveur SQL dans les Pages de propriétés de seconde Application ([cliquez pour afficher l’image en taille réelle](debugging-stored-procedures-cs/_static/image7.png))


En outre, nous devons mettre à jour la chaîne de connexion de base de données utilisée par l’application afin que le regroupement de connexions est désactivé. Lorsqu’une connexion à une base de données est fermée, le correspondantes `SqlConnection` objet est placé dans un pool de connexions disponibles. Lorsque vous établissez une connexion à une base de données, un objet de connexion disponibles peuvent être récupérées à partir de ce pool plutôt que devoir créer et établir une nouvelle connexion. Cette mise en pool d’objets de connexion est une amélioration des performances et est activé par défaut. Toutefois, lors du débogage nous souhaitons désactiver le regroupement de connexions, car l’infrastructure de débogage n’est pas correctement rétablie lorsque vous travaillez avec une connexion qui a été extraite du pool.

Pour le regroupement de connexion désactivée, mise à jour le `NORTHWNDConnectionString` dans `Web.config` afin qu’il inclue le paramètre `Pooling=false` .


[!code-xml[Main](debugging-stored-procedures-cs/samples/sample1.xml)]

> [!NOTE]
> Une fois que vous avez terminé le débogage SQL Server via l’application ASP.NET veillez à rétablir le regroupement de connexions en supprimant le `Pooling` définition à partir de la chaîne de connexion (ou en lui affectant `Pooling=true` ).


À ce stade, l’application ASP.NET a été configurée pour autoriser Visual Studio déboguer des objets de base de données SQL Server lorsqu’elle est appelée via l’application web. Il ne reste plus maintenant consiste à ajouter un point d’arrêt à une procédure stockée et de démarrer le débogage !

## <a name="step-3-adding-a-breakpoint-and-debugging"></a>Étape 3 : Ajout d’un point d’arrêt et débogage

Ouvrez le `Products_SelectByCategoryID` procédure stockée et définissez un point d’arrêt au début de la `SELECT` instruction en cliquant dans la marge à l’emplacement approprié ou en plaçant votre curseur au début de la `SELECT` instruction et en appuyant sur F9. Comme l’illustre la Figure 4, le point d’arrêt apparaît sous la forme d’un cercle rouge dans la marge.


[![Définir un point d’arrêt dans le Products_SelectByCategoryID procédure stockée](debugging-stored-procedures-cs/_static/image9.png)](debugging-stored-procedures-cs/_static/image8.png)

**Figure 4**: définir un point d’arrêt dans le `Products_SelectByCategoryID` la procédure stockée ([cliquez pour afficher l’image en taille réelle](debugging-stored-procedures-cs/_static/image10.png))


Dans l’ordre pour un objet de base de données SQL à déboguer une application cliente, il est impératif que la base de données configurée pour prendre en charge le débogage de l’application. Lorsque vous définissez tout d’abord un point d’arrêt, ce paramètre doit être automatiquement activé, mais il est préférable de procéder à une. Avec le bouton droit sur le `NORTHWND.MDF` nœud dans l’Explorateur de serveurs. Le menu contextuel doit inclure un menu activé élément nommé de débogage de l’Application.


![Vérifiez que l’Option de débogage d’Application est activée.](debugging-stored-procedures-cs/_static/image11.png)

**Figure 5**: Vérifiez que l’Option de débogage d’Application est activée.


Le jeu de point d’arrêt et l’option de débogage de l’Application est activée, nous sommes prêts à déboguer la procédure stockée lorsqu’elle est appelée à partir de l’application ASP.NET. Démarrez le débogueur en accédant au menu Déboguer et icône choix de démarrer le débogage, en appuyant sur F5 ou en cliquant sur le vert de lecture dans la barre d’outils. Cela démarre le débogueur et lancer le site Web.

Le `Products_SelectByCategoryID` procédure stockée a été créée dans le [à l’aide des procédures stockées existantes pour s DataSet typée TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) didacticiel. La page web correspondante (`~/AdvancedDAL/ExistingSprocs.aspx`) contient un GridView qui affiche les résultats retournés par cette procédure stockée. Visitez cette page via le navigateur. Lorsque vous atteignez la page, le point d’arrêt dans le `Products_SelectByCategoryID` procédure stockée est atteint et que le contrôle retourné à Visual Studio. Comme à l’étape 1, vous pouvez parcourir les instructions de procédure stockée s et la vue et modifier les valeurs de paramètre.


[![La Page ExistingSprocs.aspx affiche initialement les boissons](debugging-stored-procedures-cs/_static/image13.png)](debugging-stored-procedures-cs/_static/image12.png)

**Figure 6**: le `ExistingSprocs.aspx` Page affiche initialement les boissons ([cliquez pour afficher l’image en taille réelle](debugging-stored-procedures-cs/_static/image14.png))


[![La procédure stockée s point d’arrêt a été atteint.](debugging-stored-procedures-cs/_static/image16.png)](debugging-stored-procedures-cs/_static/image15.png)

**Figure 7**: s de la procédure stockée le point d’arrêt a été atteint ([cliquez pour afficher l’image en taille réelle](debugging-stored-procedures-cs/_static/image17.png))


En tant que la fenêtre Espion dans la Figure 7 montre, la valeur de la `@CategoryID` paramètre est 1. Il s’agit, car le `ExistingSprocs.aspx` page affiche initialement les produits dans la catégorie boissons, qui a un `CategoryID` la valeur 1. Choisissez une catégorie différente dans la liste déroulante. Cette opération entraîne une publication et réexécute la `Products_SelectByCategoryID` procédure stockée. Le point d’arrêt est atteint, mais cette fois le `@CategoryID` valeur du paramètre s reflète l’élément de liste déroulante sélectionnée s `CategoryID`.


[![Choisissez une catégorie différente dans la liste déroulante](debugging-stored-procedures-cs/_static/image19.png)](debugging-stored-procedures-cs/_static/image18.png)

**Figure 8**: choisissez une catégorie différente dans la liste déroulante ([cliquez pour afficher l’image en taille réelle](debugging-stored-procedures-cs/_static/image20.png))


[![Le @CategoryID paramètre reflète la catégorie sélectionnée à partir de la Page Web](debugging-stored-procedures-cs/_static/image22.png)](debugging-stored-procedures-cs/_static/image21.png)

**Figure 9**: le `@CategoryID` paramètre reflète la catégorie sélectionnée dans la Page Web ([cliquez pour afficher l’image en taille réelle](debugging-stored-procedures-cs/_static/image23.png))


> [!NOTE]
> Si le point d’arrêt dans le `Products_SelectByCategoryID` procédure stockée n’est pas atteint lors de la visite le `ExistingSprocs.aspx` , assurez-vous que la case à cocher de SQL Server a été cochée dans la section débogueurs de l’application ASP.NET s Page de propriétés, que le regroupement de connexions a été désactivé, et que la base de données s option de débogage de l’Application est activée. Si vous re toujours des difficultés, redémarrez Visual Studio et réessayez.


## <a name="debugging-t-sql-database-objects-on-remote-instances"></a>Débogage des objets de base de données de T-SQL sur des Instances distantes

Débogage des objets de base de données via Visual Studio est assez simple lorsque l’instance de base de données SQL Server est sur le même ordinateur que Visual Studio. Toutefois, si SQL Server et Visual Studio se trouvent sur des ordinateurs différents certains une configuration soigneuse est nécessaire pour que tout fonctionne correctement. Il existe deux tâches principales que se posent :

- Assurez-vous que la connexion utilisée pour se connecter à la base de données via ADO.NET appartienne à la `sysadmin` rôle.
- Vérifiez que le compte d’utilisateur Windows utilisé par Visual Studio sur l’ordinateur de développement est un compte de connexion SQL Server valid qui appartient à la `sysadmin` rôle.

La première étape est relativement simple. Tout d’abord, identifiez le compte d’utilisateur utilisé pour se connecter à la base de données à partir de l’application ASP.NET et puis, à partir de SQL Server Management Studio, ajoutez ce compte de connexion à la `sysadmin` rôle.

La seconde tâche nécessite que le compte d’utilisateur Windows utilisé pour déboguer l’application soit un compte de connexion valide sur la base de données distante. Toutefois, il est probable qu’est le compte Windows que vous ouvrez une session sur votre station de travail avec n’est pas une connexion valide sur SQL Server. Au lieu d’ajouter votre compte de connexion spécifique à SQL Server, un meilleur choix serait pour désigner un compte d’utilisateur Windows en tant que le compte de débogage de SQL Server. Ensuite, pour déboguer les objets de base de données d’une instance de SQL Server distante, vous exécuteriez Visual Studio à l’aide de cette identification de compte s de Windows.

Un exemple devrait permettre de clarifier les choses. Imaginez qu’il existe un compte Windows nommé `SQLDebug` au sein du domaine Windows. Ce compte devra être ajouté à l’instance distante de SQL Server sous la forme d’une connexion valide et un membre de la `sysadmin` rôle. Ensuite, pour déboguer l’instance distante de SQL Server à partir de Visual Studio, nous devons exécuter Visual Studio en tant que le `SQLDebug` utilisateur. Cela peut être effectuée en vous connectant en dehors de notre station de travail, se reconnectent en tant que `SQLDebug`, et puis en lançant Visual Studio, mais une approche plus simple consisterait à se connecter à notre station de travail à l’aide de nos propres informations d’identification, puis utilisez `runas.exe` pour lancer Visual Studio en tant que le `SQLDebug` utilisateur. `runas.exe` permet à une application particulière doit être exécuté sous la forme d’un autre compte d’utilisateur. Pour lancer Visual Studio en tant que `SQLDebug`, vous pouvez entrer l’instruction suivante à la ligne de commande :


[!code-console[Main](debugging-stored-procedures-cs/samples/sample2.cmd)]

Pour obtenir une explication plus détaillée sur ce processus, consultez [William R. Vaughn](http://betav.com/BLOG/billva/) s *Hitchhiker ' s s Guide Visual Studio et SQL Server, septième édition* ainsi que [Comment : définir des autorisations SQL Server pour le débogage](https://msdn.microsoft.com/library/w1bhybwz(VS.80).aspx).

> [!NOTE]
> Si votre ordinateur de développement s’exécute Windows XP Service Pack 2, vous devez configurer le pare-feu de connexion Internet pour autoriser le débogage distant. [La procédure pour : activer le débogage de SQL Server 2005](https://msdn.microsoft.com/library/s0fk6z6e(VS.80).aspx) article vous indique que cela implique deux étapes : (a) sur l’ordinateur hôte Visual Studio, vous devez ajouter `Devenv.exe` à la liste des Exceptions et ouvrir le port TCP 135 ; et (b) sur l’ordinateur distant (SQL), vous devez ouvrir TCP 135 de port et ajoutez `sqlservr.exe` à la liste des Exceptions. Si votre stratégie de domaine nécessite la communication réseau via IPSec, vous devez ouvrir les ports UDP 4500 et UDP 500.


## <a name="summary"></a>Récapitulatif

En plus de fournir la prise en charge de débogage pour le code d’application .NET, Visual Studio propose également un ensemble d’options de débogage pour SQL Server 2005. Dans ce didacticiel nous avons étudié deux de ces options : débogage Direct de base de données et de débogage de l’application. Pour déboguer directement un objet de base de données de T-SQL, recherchez l’objet via l’Explorateur de serveurs puis avec le bouton droit dessus et choisissez pas à pas détaillé. Cela démarre le débogueur et s’arrête à la première instruction dans l’objet de base de données, à quel point vous pouvez parcourir les instructions de s objet et les afficher et modifier les valeurs de paramètre. À l’étape 1, nous avons utilisé cette approche à parcourir le `Products_SelectByCategoryID` procédure stockée.

Débogage de l’application permet à des points d’arrêt à définir directement dans les objets de base de données. Lorsqu’un objet de base de données avec des points d’arrêt est appelé à partir d’une application cliente (par exemple, une application web ASP.NET), le programme s’arrête car le débogueur adopte. Débogage de l’application est utile, car il montre plus clairement à quelle action de l’application provoque un objet de base de données particulier à appeler. Toutefois, elle nécessite un peu plus de configuration et du programme d’installation que débogage Direct de base de données.

Objets de base de données peuvent également être débogués via des projets SQL Server. Nous allons examiner à l’aide de projets SQL Server et comment les utiliser pour créer et déboguer gérés des objets de base de données dans le didacticiel suivant.

Bonne programmation !

## <a name="about-the-author"></a>À propos de l’auteur

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), auteur de sept les livres sur ASP/ASP.NET et fondateur de [4GuysFromRolla.com](http://www.4guysfromrolla.com), travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est [*SAM animer vous-même ASP.NET 2.0 des dernières 24 heures*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Il peut être contacté à [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou via son blog, qui se trouve à [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Précédent](protecting-connection-strings-and-other-configuration-information-cs.md)
> [Suivant](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
