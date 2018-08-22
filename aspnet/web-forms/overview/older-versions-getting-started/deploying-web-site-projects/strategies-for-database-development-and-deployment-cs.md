---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-cs
title: Stratégies de développement de base de données et de déploiement (c#) | Microsoft Docs
author: rick-anderson
description: Lorsque vous déployez une application orientée données pour la première fois que vous pouvez aveuglément copier la base de données dans l’environnement de développement à l’environnement de production. B....
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 3e8b0627-3eb7-488e-807e-067cba7cec05
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-cs
msc.type: authoredcontent
ms.openlocfilehash: 029c17d5f996f595aef1af68c3ddb9b21f99f5cf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835010"
---
<a name="strategies-for-database-development-and-deployment-c"></a>Stratégies de développement de base de données et de déploiement (c#)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial10_DBDevel_cs.pdf)

> Lorsque vous déployez une application orientée données pour la première fois que vous pouvez aveuglément copier la base de données dans l’environnement de développement à l’environnement de production. Mais effectuant un blind copie dans les déploiements suivants remplacera toutes les données entrées dans la base de données de production. Au lieu de cela, le déploiement d’une base de données implique appliquer les modifications apportées à la base de données de développement depuis le dernier déploiement sur la base de données de production. Ce didacticiel examine ces défis et propose des stratégies différentes pour vous aider à chronicling et appliquer les modifications apportées à la base de données depuis le dernier déploiement.


## <a name="introduction"></a>Introduction

Comme indiqué dans les didacticiels précédents, déploiement d’une application ASP.NET implique la copie le contenu pertinent à partir de l’environnement de développement à l’environnement de production. Déploiement n’est pas un événement ponctuel, mais plutôt quelque chose qui se produit chaque fois qu’une nouvelle version du logiciel est publiée ou quand les bogues ou problèmes de sécurité ont été identifiés et résolus. Lors de la copie des pages ASP.NET, images, fichiers JavaScript et autres fichiers de ce type à l’environnement de production que vous n’avez pas besoin de vous occuper de la façon dont ces fichiers ont été modifiés depuis le dernier déploiement. Vous pouvez aveuglément copier le fichier à la production, en remplaçant le contenu existant. Malheureusement, cette simplicité n’étend pas au déploiement de la base de données.

Lorsque vous déployez une application orientée données pour la première fois que vous pouvez aveuglément copier la base de données dans l’environnement de développement à l’environnement de production. Mais effectuant un blind copie dans les déploiements suivants remplacera toutes les données entrées dans la base de données de production. Au lieu de cela, déploiement d’une base de données implique d’appliquer le *modifications* apportées à la base de données de développement depuis le dernier déploiement sur la base de données de production. Ce didacticiel examine ces défis et propose des stratégies différentes pour vous aider à chronicling et appliquer les modifications apportées à la base de données depuis le dernier déploiement.

## <a name="the-challenges-of-deploying-a-database"></a>Les défis du déploiement d’une base de données

Avant d’une application orientée données a été déployée pour la première fois, il n'est qu’une seule base de données, à savoir la base de données dans l’environnement de développement, c’est pourquoi lorsque vous déployez une application orientée données pour la première fois que vous pouvez aveuglément copier la base de données dans le environnement de développement à l’environnement de production. Mais une fois que l’application a été déployée. il existe deux copies de la base de données : un dans le développement et l’autre en production.

Entre les déploiements, les bases de données de développement et de production peuvent devenir désynchronisés. Alors que le schéma de s de base de données de production reste inchangé, le schéma de s de base de données de développement peut changer lors de l’ajout de nouvelles fonctionnalités. Vous pouvez ajouter ou supprimer des colonnes, tables, vues ou procédures stockées. Il peut également être des données importantes qui sont ajoutées à la base de données de développement. De nombreuses applications pilotées par les données contiennent des tables de recherche remplis avec les données codées en dur, spécifiques à l’application qui ne sont pas modifiables par l’utilisateur. Par exemple, un site Web de vente aux enchères peut avoir une liste déroulante avec les choix qui décrivent la condition de l’élément en cours d’enchère : New, telles que la nouvelle, bonne et équitable. Au lieu de coder en dur ces options directement dans la liste déroulante, il est généralement préférable de les placer dans une table de base de données. Si, pendant le développement, une nouvelle condition nommée médiocre est ajoutée à la table puis lors du déploiement de l’application ce même enregistrement doit être ajouté à la table de recherche dans la base de données de production.

Dans l’idéal, déploiement de la base de données implique la copie de la base de données de développement en production. Mais n’oubliez pas qu’une fois que vous avez déployé l’application et la reprise de développement, la base de données de production sont ajouté à des données réelles provenant des utilisateurs réels. Par conséquent, si vous deviez simplement copier la base de données à partir de développement pour le prochain déploiement de production vous remplacer la base de données de production et perdre les données existantes. Le résultat net est que le déploiement de la base de données se résume à appliquer les modifications apportées à la base de données de développement depuis le dernier déploiement.

Étant donné que le déploiement d’une base de données implique d’appliquer les modifications dans le schéma et, éventuellement, les données depuis le dernier déploiement, un historique des modifications doit être maintenu (ou déterminé au moment du déploiement) afin que ces modifications peuvent être appliquées sur la production. Il existe une variété de techniques pour la gestion et appliquer les modifications au modèle de données.

### <a name="defining-the-baseline"></a>Définition de la ligne de base

Pour conserver les modifications apportées à votre base de données s application, vous devez avoir un état de départ, une ligne de base à laquelle les modifications sont appliquées à. D’un côté, l’état de départ peut être une base de données vide sans tables, vues, ou des procédures stockées. Une ligne de base des résultats dans un journal de modification importante, car il doit inclure la création de toutes les tables de base de données s, les vues et les procédures stockées, ainsi que les modifications apportées après le déploiement initial. À l’autre extrémité du spectre, vous pouvez définir la ligne de base en tant que la version de la base de données est d’abord déployée sur l’environnement de production. Ce choix entraîne un journal des modifications beaucoup plus petits, car elle inclut uniquement les modifications apportées à la base de données suivant le premier déploiement. Il s’agit de l’approche que je préfère. Et bien sûr vous pouvez choisir un milieu plus de l’approche de la route, définition de la ligne de base que certaines point entre la création initiale de la base de données et lorsque la base de données est tout d’abord déployée.

Une fois que vous avez choisi une ligne de base envisagez de générer un script SQL qui peut être exécuté pour recréer la version de référence. Un tel script rend possible recréer rapidement la version de ligne de base de la base de données. Cette fonctionnalité est particulièrement utile dans les grands projets, où il peut y avoir plusieurs développeurs travaillent sur le projet ou des environnements supplémentaires, tels que les tests ou intermédiaire, que chaque besoin de sa propre copie de la base de données.

Il existe un large éventail d’outils à votre disposition pour générer un script SQL de la version de ligne de base. À partir de SQL Server Management Studio (SSMS) vous pouvez avec le bouton droit sur la base de données, accédez au sous-menu de tâches, choisissez l’option Générer des Scripts. Cette opération lance l’Assistant de Script, vous pouvez demander à générer un fichier qui contient les commandes SQL pour créer des objets de s de votre base de données. Une autre option consiste à l’Assistant Publication de base de données, qui peut générer les commandes SQL pour non seulement de créer le schéma de base de données, mais également les données dans les tables de base de données. L’Assistant Publication de base de données a été examiné en détail dans le *déploiement d’une base de données* didacticiel. Quel que soit les outils que vous utilisez, au final, vous devez avoir un fichier de script que vous pouvez utiliser pour recréer la version de ligne de base de votre base de données, en cas de besoin surviennent.

## <a name="documenting-the-database-changes-in-prose"></a>Documenter les modifications de base de données dans une Prose

Pour conserver un journal des modifications au modèle de données pendant la phase de développement, le plus simple consiste à enregistrer les modifications dans une prose. Par exemple, si pendant le développement d’une application déjà déployée, vous ajoutez une nouvelle colonne à la `Employees` de table, de supprimer une colonne à partir de la `Orders` table et ajouter une nouvelle table (`ProductCategories`), vous maintiendrait un fichier texte ou un document Microsoft Word l’historique des suivantes :

<a id="0.4_table01"></a>


| **Date de modification** | **Modifier les détails** |
| --- | --- |
| 2009-02-03: | Ajout d’une colonne `DepartmentID` (`int`, non NULL) pour le `Employees` table. Ajouter une contrainte de clé étrangère à partir de `Departments.DepartmentID` à `Employees.DepartmentID`. |
| 2009-02-05: | Colonne supprimé `TotalWeight` à partir de la `Orders` table. Données déjà capturées dans associés `OrderDetails` enregistrements. |
| 2009-02-12: | Créé le `ProductCategories` table. Il y a trois colonnes : `ProductCategoryID` (`int`, `IDENTITY`, `NOT NULL`), `CategoryName` (`nvarchar(50)`, `NOT NULL`), et `Active` (`bit`, `NOT NULL`). Ajouter une contrainte de clé primaire à `ProductCategoryID`et une valeur par défaut de 1 à `Active`. |


Il existe un certain nombre d’inconvénients à cette approche. Pour commencer, il est sans espoir pour l’automatisation. À tout moment ces modifications doivent être appliquées à une base de données - telles que lorsque l’application est déployée, un développeur doit implémenter manuellement chacune, une à la fois. En outre, si vous avez besoin pour reconstruire une version particulière de la base de données à partir de la ligne de base à l’aide du journal des modifications, cela donc prendra plus de temps que la taille du journal augmente. Un autre inconvénient à cette méthode est que la clarté et le niveau de détail de chaque entrée de journal est de gauche à la personne que l’enregistrement de la modification. Dans une équipe avec plusieurs développeurs certaines peuvent rendre plus détaillées, plus lisibles et plus précisément les entrées que d’autres. En outre, les fautes de frappe et autres erreurs de saisie de données liées aux humaines sont possibles.

Le principal avantage de documenter les modifications de base de données dans une prose est sa simplicité. Vous n’avez pas une bonne connaissance de la nécessité t avec la syntaxe SQL pour la création et modification d’objets de base de données. Au lieu de cela, vous pouvez enregistrer les modifications dans une prose et leur implémentation via l’interface utilisateur graphique de s SQL Server Management Studio.

Maintenir votre journal des modifications dans une prose est, certes, pas très sophistiqué et gagné / fonctionne correctement avec certains projets, telles que celles qui sont de grande taille dans la portée, ont des modifications fréquentes au modèle de données, ou impliquant plusieurs développeurs. Mais j’ai vu cette approche fonctionne très bien dans les petites, adeptes des projets qui ont uniquement des modifications au modèle de données occasionnelles et où le développeur Freelance n’a pas une expérience préalable conséquente dans la syntaxe SQL pour la création et modification d’objets de base de données.

> [!NOTE]
> Alors que les informations contenues dans le journal des modifications sont techniquement, nécessaire uniquement jusqu’au moment du déploiement, je vous recommande de conserver un historique des modifications. Mais au lieu de conserver un seul, toujours le fichier journal des modifications, envisagez de disposer d’un fichier journal de modification différente pour chaque version de base de données. En général, vous souhaitez version de la base de données chaque fois qu’elle est déployée. En conservant un journal des journaux des modifications n’importe quelle version de base de données en exécutant les scripts de journal de modification depuis la version 1 peut, à partir de la ligne de base, recréer et continuant jusqu'à ce que vous atteigniez la version que vous devrez recréer.


## <a name="recording-the-sql-change-statements"></a>Enregistrer les instructions de modification SQL

L’inconvénient principal de gestion du journal des modifications dans une prose est le manque d’automatisation. Dans l’idéal, implémenter les modifications de base de données à la base de données de production au moment du déploiement est aussi simple que cliquer sur un bouton pour exécuter un script au lieu de devoir effectuer manuellement une liste d’instructions. Cette automatisation est possible en conservant un journal des modifications qui contient les commandes SQL permettent de modifier le modèle de données.

La syntaxe SQL comprend des instructions pour créer et modifier divers objets de base de données. Par exemple, le [ *instruction CREATE TABLE*](https://msdn.microsoft.com/library/ms174979.aspx)est exécuté, crée une table avec les colonnes spécifiées et les contraintes. Le [ *instruction ALTER TABLE* ](https://msdn.microsoft.com/library/ms190273.aspx) modifie une table existante, ajout, suppression ou modification de ses contraintes ou les colonnes. Il existe également des instructions pour créer, modifier et supprimer des index, vues, fonctions définies par l’utilisateur, procédures stockées, déclencheurs et autres objets de base de données.

Retour à notre exemple précédent, l’image que pendant le développement d’une application déjà déployée, vous ajoutez une nouvelle colonne à la `Employees` de table, de supprimer une colonne à partir de la `Orders` table et ajouter une nouvelle table (`ProductCategories`). Ces actions génère un fichier de journal des modifications avec les commandes SQL suivantes :

[!code-sql[Main](strategies-for-database-development-and-deployment-cs/samples/sample1.sql)]

Distribution de ces modifications vers la base de données de production au moment du déploiement est une opération de clic : ouvrez SQL Server Management Studio, vous connecter à votre base de données de production, ouvrez une fenêtre de nouvelle requête, collez le contenu du journal des modifications, cliquez sur Exécuter pour exécuter le script.

## <a name="using-a-comparison-tool-to-synchronize-the-data-models"></a>À l’aide d’un outil de comparaison pour synchroniser les modèles de données

Il est facile de documenter les modifications de base de données dans une prose, mais les modifications de mise en œuvre nécessite un développeur d’apporter de chaque modification sur la base de données de production une à la fois. documenter les commandes SQL de modification facilite l’implémentation de ces modifications sur la base de données de production en tant que simple et rapide qu’un clic sur un bouton, mais nécessite d’apprentissage et de maîtrise les instructions SQL et la syntaxe pour la création et modification d’objets de base de données. Outils de comparaison de base de données tirer le meilleur de ces deux approches et ignorer le pire.

Un outil de comparaison de base de données compare le schéma ou les données de deux bases de données et affiche un rapport de synthèse vous montrant comment les bases de données diffèrent. Ensuite, en cliquant sur un bouton, vous pouvez générer les commandes SQL pour la synchronisation d’un ou plusieurs objets de base de données. En bref, vous pouvez utiliser un outil de comparaison de base de données à comparer le développement et les bases de données de production au moment du déploiement, génération d’un fichier qui contient le code SQL de commandes, lorsque exécutée, s’applique les modifications au schéma de base de données s production donc qu’il reflète le schéma de base de données de développement.

Il existe une variété d’outils de comparaison de base de données tierce proposés par de nombreux fournisseurs différents. Par exemple, une est [ *SQL Compare*](http://www.red-gate.com/products/SQL_Compare/), par [ *Red Gate Software*](http://www.red-gate.com/). Laissez s décrivent le processus de l’utilisation de SQL Compare pour comparer et synchroniser les schémas de bases de données de développement et de production.

> [!NOTE]
> Au moment de la rédaction la version actuelle de SQL Compare était la version 7.1, avec l’Édition Standard d’évaluation des coûts à partir de 395 $. Vous pouvez suivre la procédure en téléchargeant une version d’évaluation gratuite de 14 jours.


Démarrage de SQL Compare la boîte de dialogue de projets de comparaison s’ouvre, affichant les projets enregistrés SQL Compare. Créer un nouveau projet. Cette action lance l’Assistant Configuration de projet, qui vous invite à entrer pour plus d’informations sur les bases de données à comparer (voir Figure 1). Entrez les informations pour les bases de données d’environnement de développement et de production.


[![Comparer le développement et les bases de données de Production](strategies-for-database-development-and-deployment-cs/_static/image2.jpg)](strategies-for-database-development-and-deployment-cs/_static/image1.jpg)

**Figure 1**: comparer le développement et les bases de données de Production ([cliquez pour afficher l’image en taille réelle](strategies-for-database-development-and-deployment-cs/_static/image3.jpg))


> [!NOTE]
> Si votre base de données des environnement de développement est un fichier de base de données SQL Express Edition dans la `App_Data` dossier de votre site Web que vous devez inscrire la base de données dans le serveur de base de données SQL Server Express afin de le sélectionner dans la boîte de dialogue illustrée dans la Figure 1. Le moyen le plus simple pour y parvenir consiste à ouvrir SQL Server Management Studio (SSMS), connectez-vous au serveur de base de données SQL Server Express et attacher la base de données. Si vous n’avez pas installé sur votre ordinateur SSMS vous pouvez télécharger et installer la version gratuite [ *base SQL Server 2008 Management Studio version*](https://www.microsoft.com/downloads/details.aspx?FamilyId=7522A683-4CB2-454E-B908-E805E9BD4E28&amp;displaylang=en).


Après avoir sélectionné les bases de données à comparer, vous pouvez également spécifier divers paramètres de comparaison à partir de l’onglet Options. Une option, que vous pouvez souhaiter activer est « Ignorer contrainte et l’index noms. » Rappelez-vous que dans le didacticiel précédent nous avons ajouté que des objets de base de données aux bases de données de développement et de production de services de l’application. Si vous avez utilisé le `aspnet_regsql.exe` outil pour créer ces objets sur la base de données de production se trouve que la clé primaire et les noms de contrainte unique diffèrent entre les bases de données de développement et de production. Par conséquent, SQL Compare marquera toutes les tables de services d’application comme des différentes. Vous pouvez soit laisser les « ignorer contrainte et l’index noms » désactivée et synchroniser les noms de contrainte, ou demandez au SQL Compare pour ignorer ces différences.

Après avoir sélectionné les bases de données à comparer (et examiner les options de comparaison), cliquez sur le bouton comparer maintenant pour commencer la comparaison. Sur les prochaines secondes, SQL Compare examine les schémas des deux bases de données et génère un rapport de la façon dont elles diffèrent. J’ai sauter délibérément des modifications à la base de données de développement pour montrer comment ces différences sont indiquées dans l’interface SQL Compare. Comme le montre la Figure 2, je ve ajouté un `BirthDate` colonne à la `Authors` table, supprimés le `ISBN` colonne à partir de la `Books` table, et ajouté une nouvelle table, `Ratings`, qui vise à permettre aux utilisateurs visitant le taux de site les livres de révision.

> [!NOTE]
> Les modifications de modèle de données effectuées dans ce didacticiel ont été résolues pour illustrer l’utilisation d’un outil de comparaison de base de données. Vous trouverez pas ces modifications dans la base de données dans les didacticiels futures.


[![SQL Compare répertorie les différences entre le développement et les bases de données de Production](strategies-for-database-development-and-deployment-cs/_static/image5.jpg)](strategies-for-database-development-and-deployment-cs/_static/image4.jpg)

**Figure 2**: SQL Compare répertorie les différences entre le développement et les bases de données de Production ([cliquez pour afficher l’image en taille réelle](strategies-for-database-development-and-deployment-cs/_static/image6.jpg))


SQL Compare décompose les objets de base de données en groupes, rapidement vous montrant quels sont les objets existent dans les deux bases de données, mais sont différent, qui objets existent dans une base de données, mais pas dans l’autre, et quels objets sont identiques. Comme vous pouvez le voir, il existe deux objets qui existent dans les deux bases de données mais sont différents : le `Authors` table dont une colonne ajoutée, et le `Books` table, qui contenait un supprimé. Il existe un objet qui existe uniquement dans le développement de base de données, à savoir nouvellement créé `Ratings` table. Et il existe des 117 objets sont identiques dans les deux bases de données.

Sélection d’un objet de base de données affiche la fenêtre différences SQL, qui montre comment ces objets sont différents. Met en surbrillance de la fenêtre différences SQL, affichée en bas dans la Figure 2, qui les `Authors` table dans la base de données de développement a la `BirthDate` colonne, qui est introuvable dans le `Authors` table sur la base de données de production.

Après avoir examiné les différences et en sélectionnant les objets que vous souhaitez synchroniser, l’étape suivante consiste à générer les commandes SQL nécessaires pour mettre à jour le schéma de s de base de données de production pour correspondre à la base de données de développement. Cela s’effectue via l’Assistant synchronisation. L’Assistant synchronisation confirme quels objets pour synchroniser et résume l’action planifier (voir Figure 3). Vous pouvez synchroniser les bases de données immédiatement ou générer un script avec les commandes SQL qui peut être exécuté à tout moment.


[![Utilisez l’Assistant de synchronisation pour synchroniser vos schémas de bases de données](strategies-for-database-development-and-deployment-cs/_static/image8.jpg)](strategies-for-database-development-and-deployment-cs/_static/image7.jpg)

**Figure 3**: utilisez l’Assistant de synchronisation pour synchroniser vos schémas de bases de données ([cliquez pour afficher l’image en taille réelle](strategies-for-database-development-and-deployment-cs/_static/image9.jpg))


Outils de comparaison de base de données telles que Red Gate Software s SQL Compare rendre l’application des modifications au schéma de base de données de développement pour la base de données de production aussi simple que de pointer et cliquez sur.

> [!NOTE]
> Comparer SQL Compare et synchronise deux bases de données *schémas*. Malheureusement, il ne comparer et synchroniser les données dans les tables de deux bases de données. Red Gate Software offre un produit nommé [ *comparaison de données SQL* ](http://www.red-gate.com/products/SQL_Data_Compare/) qui compare et synchronise les données entre deux bases de données, mais il est un produit distinct à partir de SQL Compare et les coûts d’un autre 395 $.


## <a name="taking-the-application-offline-during-deployment"></a>En prenant l’Application en mode hors connexion pendant le déploiement

Comme nous ve vu tout au long de ces didacticiels, le déploiement est un processus qui implique plusieurs étapes : copie les pages ASP.NET, les pages maîtres, CSS fichiers, JavaScript fichiers, images et autre contenu nécessaire à partir de l’environnement de développement à la production environnement ; copie les informations de configuration spécifiques à l’environnement de production, si nécessaire ; et en appliquant les modifications apportées au modèle de données depuis le dernier déploiement. Selon le nombre de fichiers et la complexité de vos modifications de base de données, ces étapes peuvent prendre de quelques secondes à plusieurs minutes pour terminer. Pendant cette fenêtre de l’application web est en mouvance constante et les utilisateurs qui visitent le site peuvent rencontrer des erreurs ou un comportement inattendu.

Lors du déploiement d’un site Web, il est préférable d’adopter l’application web « hors connexion » jusqu'à ce que le déploiement est terminé. Mise hors connexion de l’application (et les mettre sauvegarder une fois terminé le processus de déploiement) est aussi simple que le téléchargement d’un fichier, puis leur suppression. Démarrer avec ASP.NET 2.0, la simple présence d’un fichier nommé `app_offline.htm` dans l’application s répertoire racine prend tout le site Web « hors connexion ». Toute demande d’une page ASP.NET sur ce site est renseigné automatiquement avec le contenu de la `app_offline.htm` fichier. Une fois que ce fichier est supprimé, l’application revient en ligne.

Prendre une application en mode hors connexion pendant le déploiement, puis, est aussi simple que de charger un `app_offline.htm` fichier à l’environnement de production s répertoire avant de commencer le processus de déploiement et puis en le supprimant (ou de le renommer à quelque chose d’autre) de racine déploiement une fois est terminée. Pour plus d’informations sur cette technique, reportez-vous à l’article de s John Peterson, en prenant un [ *ASP.NET Application en mode hors connexion*](http://www.15seconds.com/issue/061207.htm).

## <a name="summary"></a>Récapitulatif

Le principal défi de déploiement d’une application orientée données se concentre sur le déploiement de la base de données. Qu’il existe deux versions de la base de données - un dans l’environnement de développement et l’autre dans l’environnement de production ces schémas de deux bases de données peuvent devenir désynchronisés comme l’ajout de nouvelles fonctionnalités dans le développement. Nouveautés plus, étant donné que la base de données de production comme rempli avec des données réelles provenant des utilisateurs réels, Impossible de remplacer la base de données de production avec la base de données de développement modifié comme vous pouvez le faire lorsque vous déployez les fichiers qui composent l’application (les pages ASP.NET, fichiers image et ainsi de suite). Au lieu de cela, déploiement d’une base de données implique la mise en œuvre de l’ensemble précis des modifications apportées à la base de données de développement sur la base de données de production depuis le dernier déploiement.

Ce didacticiel examiné trois techniques de la gestion et appliquer un journal des modifications de base de données. L’approche la plus simple consiste à enregistrer les modifications dans une prose. Bien que cette tactique facilite l’implémentation de ces modifications sur la base de données de production un processus manuel, il ne nécessite pas de connaissances des commandes SQL pour la création et modification d’objets de base de données. Une approche plus sophistiquée et l’autre qui est bien plus attractif dans des projets ou des projets plus volumineux avec plusieurs développeurs, consiste à enregistrer les modifications en tant que série de commandes SQL. Cela accélère considérablement grandement déployer ces modifications à la base de données cible. Le meilleur des deux approches peut être obtenu à l’aide d’un outil de comparaison de base de données, comme s Red Gate Software SQL Compare.

Ce didacticiel conclut notre focus sur le déploiement d’une application orientée données. L’ensemble suivant de didacticiels examine comment répondre aux erreurs dans l’environnement de production. Nous examinerons comment afficher une page d’erreur convivial plutôt au lieu de l’écran de décès jaune. Et nous verrons comment consigner les détails de s erreur et pour vous avertir lorsque ces erreurs se produisent.

Bonne programmation !

> [!div class="step-by-step"]
> [Précédent](configuring-a-website-that-uses-application-services-cs.md)
> [Suivant](displaying-a-custom-error-page-cs.md)
