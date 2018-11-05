---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: Présentation des Pages Web ASP.NET - affichage des données | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous montre comment créer une base de données dans WebMatrix et comment afficher les données de la base de données dans une page lorsque vous utilisez les Pages Web ASP.NET (Razor). Il part du principe que vous y...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: 9158a1f53268daec6e6fbdf003dd73e1d62cc667
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021584"
---
<a name="introducing-aspnet-web-pages---displaying-data"></a>Présentation des Pages Web ASP.NET - affichage des données
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Ce didacticiel vous montre comment créer une base de données dans WebMatrix et comment afficher les données de la base de données dans une page lorsque vous utilisez les Pages Web ASP.NET (Razor). Il part du principe que vous avez terminé la série via [Introduction à la programmation de Pages Web ASP.NET](../introducing-razor-syntax-c.md).
> 
> Ce que vous allez apprendre :
> 
> - Comment utiliser les outils de WebMatrix pour créer une base de données et les tables de base de données.
> - L’utilisation des outils de WebMatrix pour ajouter des données à une base de données.
> - Comment afficher des données à partir de la base de données sur une page.
> - Comment exécuter des commandes SQL dans ASP.NET Web Pages.
> - Comment personnaliser le `WebGrid` helper pour modifier l’affichage de données et ajouter la pagination et le tri.
>   
> 
> Fonctionnalités technologies abordées :
> 
> - Outils de base de données de WebMatrix.
> - `WebGrid` programme d’assistance.


## <a name="what-youll-build"></a>Ce que vous allez générer

Dans le didacticiel précédent, vous ont été introduites pour ASP.NET Web Pages (*.cshtml* fichiers), les principes fondamentaux de la syntaxe Razor et aux helpers. Dans ce didacticiel, vous allez commencer à créer l’application web réelle que vous utiliserez pour le reste de la série. L’application est une application cinématographique simple qui vous permet d’afficher, ajouter, modifier et supprimer des informations sur les films.

Lorsque vous avez terminé avec ce didacticiel, vous serez en mesure d’afficher une liste de films qui ressemble à cette page :

![Affichage de WebGrid qui inclut les paramètres définis pour les noms de classe CSS](displaying-data/_static/image1.png)

Mais pour commencer, vous devez créer une base de données.

## <a name="a-very-brief-introduction-to-databases"></a>Très brièvement les bases de données

Ce didacticiel fournit uniquement l’introduction et aux bases de données. Si vous avez expérience de la base de données, vous pouvez ignorer cette section courte.

Une base de données contient une ou plusieurs tables qui contiennent des informations &mdash; tables, par exemple, pour les clients, les commandes et les fournisseurs, ou pour les étudiants, les enseignants, les classes et les notes. Point de vue structurel, une table de base de données ressemble à une feuille de calcul. Imaginez un carnet d’adresses classiques. Pour chaque entrée dans le carnet d’adresses (autrement dit, pour chaque personne) vous avez plusieurs éléments d’informations telles que le prénom, nom, adresse, adresse de messagerie et numéro de téléphone.

![Exemple de table de base de données dans une grille simple](displaying-data/_static/image2.png)

(Lignes sont parfois appelés *enregistrements*, et les colonnes sont parfois appelées *champs*.)

Pour la plupart des tables de base de données, la table doit avoir une colonne qui contient une valeur unique, comme un numéro de client, numéro de compte et ainsi de suite. Cette valeur est appelée la table *clé primaire*, et utilisez-la pour identifier chaque ligne dans la table. Dans l’exemple, la colonne ID est la clé primaire pour le carnet d’adresses indiqué dans l’exemple précédent.

Une grande partie des tâches que vous effectuez dans les applications web se compose de la lecture des informations de la base de données et de les afficher sur une page. Vous allez également souvent recueillir des informations à partir d’utilisateurs et l’ajouter à une base de données, ou que vous allez modifier les enregistrements qui se trouvent déjà dans la base de données. (Nous aborderons toutes ces opérations au cours de cet ensemble de didacticiels.)

Travail de base de données peut être extrêmement complexe et peut nécessiter des connaissances spécialisées. Cependant, pour cet ensemble de didacticiels, vous devez comprendre uniquement concepts de base, qui seront tous expliquées comme vous accédez.

## <a name="creating-a-database"></a>Création d’une base de données

WebMatrix inclut des outils qui facilitent la création d’une base de données et créer des tables dans la base de données. (La structure d’une base de données est appelée à la base de données *schéma*.) Pour cet ensemble de didacticiels, vous allez créer une base de données qui compte une seule table &mdash; films.

Ouvrez WebMatrix si vous n’avez pas déjà fait, puis ouvrez le site WebPagesMovies que vous avez créé dans le didacticiel précédent.

Dans le volet gauche, cliquez sur le **base de données** espace de travail.

![Onglet espace de travail de base de données de WebMatrix](displaying-data/_static/image3.png)

Les modifications de ruban pour afficher les tâches liées à la base de données. Dans le ruban, cliquez sur **nouvelle base de données**.

![« Nouvelle base de données » situé dans le ruban de WebMatrix](displaying-data/_static/image4.png)

WebMatrix crée une base de données SQL Server CE (un *.sdf* fichier) qui porte le même nom que votre site &mdash; *WebPagesMovies.sdf*. (Vous ne le faire ici, mais vous pouvez renommer le fichier à votre convenance, pourvu qu’il ait un *.sdf* extension.)

## <a name="creating-a-table"></a>Création d’une Table

Dans le ruban, cliquez sur **nouvelle Table**. WebMatrix s’ouvre le Concepteur de tables dans un nouvel onglet. (Si l’option de nouvelle Table n’est pas disponible, assurez-vous que la nouvelle base de données est sélectionné dans l’arborescence sur la gauche.)

![« Nouvelle Table » situé dans le ruban de WebMatrix](displaying-data/_static/image5.png)

Dans la zone de texte en haut (où le filigrane indique « Entrez le nom de table »), entrez « Movies ».

![Entrer un nom de table dans le Concepteur de base de données de WebMatrix](displaying-data/_static/image6.png)

Le volet situé sous le nom de table est où vous définissez des colonnes individuelles. Pour le *films* table dans ce didacticiel, vous allez créer uniquement certaines colonnes : *ID*, *titre*, *Genre*, et *année*.

Dans le **nom** , entrez « ID ». Entrer une valeur ici active tous les contrôles pour la nouvelle colonne.

TAB pour accéder à la **Type de données** liste et choisissez **int**. Cette valeur spécifie que la colonne ID contiendra les données de type integer (nombre).

> [!NOTE]
> Nous ne l’appelons les plus ici (beaucoup), mais vous pouvez utiliser des gestes de clavier Windows standards pour naviguer dans cette grille. Par exemple, utilisez la touche tab entre les champs, vous pouvez simplement commencez à taper afin de sélectionner un élément dans une liste et ainsi de suite.


Onglet au-delà de la **la valeur par défaut** boîte (autrement dit, laissez ce champ vide). TAB pour accéder à la **est la clé primaire** case à cocher et sélectionnez-le. Cette option indique à la base de données qui le *ID* colonne contiendra les données qui identifie les lignes individuelles. (Autrement dit, chaque ligne aura une valeur unique dans la colonne d’ID que vous pouvez utiliser pour rechercher cette ligne.)

Choisissez le **est d’identité** option. Cette option indique à la base de données qu’il doit générer automatiquement le numéro séquentiel suivant pour chaque nouvelle ligne. (Le **est d’identité** option fonctionne uniquement si vous avez aussi sélectionné le **est la clé primaire** option.)

Cliquez sur la ligne de grille suivante, ou appuyez sur Tab à deux reprises pour terminer la ligne actuelle. Un mouvement enregistre la ligne actuelle et commence celle qui suit. Notez que le **la valeur par défaut** colonne indique désormais **Null**. (Null est la valeur par défaut pour la valeur par défaut, pour ainsi dire.)

Lorsque vous avez terminé la définition de la nouvelle **ID** colonne, le concepteur doit ressembler à cette illustration :

![Concepteur de base de données de WebMatrix après la définition de la colonne ID pour la table de films](displaying-data/_static/image7.png)

Pour créer la colonne suivante, cliquez dans la zone dans le **nom** colonne. Entrez « Title » pour la colonne, puis sélectionnez **nvarchar** pour le **Type de données** valeur. La partie « var » de **nvarchar** indique à la base de données que les données de cette colonne sera une chaîne dont la taille peut varier d’un enregistrement à l’autre. (Le préfixe « n » représente « national », ce qui indique que le champ peut contenir des données de type caractère pour n’importe quel ordre alphabétique ou le système d’écriture, autrement dit, le champ contient des données Unicode.)

Lorsque vous choisissez **nvarchar**, une autre zone s’affiche dans lequel vous pouvez entrer la longueur maximale pour le champ. Entrez 50, sur l’hypothèse qu’aucun titre du film que vous allez utiliser dans ce didacticiel n’y a plus de 50 caractères.

Skip **la valeur par défaut** et désactivez le **autoriser les valeurs null** option. Vous ne voulez pas autoriser des films à entrer dans la base de données qui n’ont pas un titre de la base de données.

Lorsque vous avez terminé et que vous accédez à la ligne suivante, le concepteur se présente comme dans cette illustration :

![Concepteur de base de données de WebMatrix après la définition de la colonne de titre pour la table de films](displaying-data/_static/image8.png)

Répétez ces étapes pour créer une colonne nommée « Genre », à l’exception de la longueur, affectez-lui la valeur 30 simplement.

Créer une autre colonne nommée « Année ». Pour le type de données, choisissez **nchar** (pas **nvarchar**) et la longueur de la valeur 4. Pour l’année, vous allez utiliser un nombre à 4 chiffres comme « 1995 » ou « 2010 », donc vous ne nécessitent pas une colonne de taille variable.

Voici à quoi ressemble la conception :

![Concepteur de base de données de WebMatrix une fois que tous les champs sont définis pour la table de films](displaying-data/_static/image9.png)

Appuyez sur Ctrl + S ou cliquez sur le **enregistrer** bouton dans la barre d’outils Accès rapide. Fermez le Concepteur de base de données en fermant l’onglet.

## <a name="adding-some-example-data"></a>Ajout des données d’exemple

Plus loin dans cette série de didacticiels, vous allez créer une page où vous pouvez entrer des nouveaux films dans un formulaire. Toutefois, pour l’instant, vous pouvez ajouter des données d’exemple que vous pouvez ensuite afficher sur une page.

Dans le **base de données** espace de travail dans WebMatrix, notez qu’il existe une arborescence qui vous montre les *.sdf* fichier que vous avez créé précédemment. Ouvrez le nœud de votre nouveau *.sdf* de fichiers, puis ouvrez le **Tables** nœud.

![Espace de travail de base de données de WebMatrix avec arborescence ouverte pour la table de films](displaying-data/_static/image10.png)

Cliquez sur le **films** nœud, puis choisissez **données**. WebMatrix ouvre une grille où vous pouvez entrer des données pour le *films* table :

![Grille d’entrée de base de données dans WebMatrix (vide)](displaying-data/_static/image11.png)

Cliquez sur le **titre** colonne et entrer « Lorsque Harry remplies Catherine ». Déplacer vers le **Genre** colonne (vous pouvez utiliser la touche Tab) et entrez « Comédie scènes romantiques ». Déplacer vers le **année** colonne et entrez « 1989 » :

![Grille d’entrée de base de données dans WebMatrix avec un seul enregistrement](displaying-data/_static/image12.png)

Appuyez sur entrée et WebMatrix enregistre le nouveau film. Notez que le **ID** colonne a été remplie.

![Grille d’entrée de base de données dans WebMatrix avec un seul enregistrement et l’ID généré automatiquement](displaying-data/_static/image13.png)

Entrez un autre film (par exemple, « disparu avec le vent », « Drama », « 1939 »). La colonne ID est de nouveau remplie :

![Grille d’entrée de base de données dans WebMatrix avec deux enregistrements et l’ID généré automatiquement](displaying-data/_static/image14.png)

Entrez un film tiers (par exemple, « Ghostbusters », « Comédie »). L’expérience, laissez le **année** colonne vide, puis appuyez sur ENTRÉE. Étant donné que vous désélectionnez le **autoriser les valeurs null** option, la base de données affiche une erreur :

![Erreur de « Données non valides » affiche si une valeur de colonne requise est vide.](displaying-data/_static/image15.png)

Cliquez sur **OK** à revenir en arrière et corriger l’entrée (l’année pour « Ghostbusters » est 1984) et appuyez sur ENTRÉE.

Renseignez plusieurs films jusqu'à ce que vous ayez 8 ou donc. (Entrer 8 rend plus facile de travailler avec pagination plus tard. Mais si c’est trop grand nombre, entrez quelques exemples pour l’instant.) Peu importe les données réelles.

![Grille d’entrée de base de données dans WebMatrix avec deux enregistrements](displaying-data/_static/image16.png)

Si vous avez entré tous les films sans erreurs, les valeurs d’ID sont séquentielles. Si vous avez essayé d’enregistrer un enregistrement vidéo incomplet, les numéros d’identification n’est peut-être pas séquentiels. Dans ce cas, c’est OK. Ces nombres n’ont aucune signification intrinsèque, et la seule chose qui est importante est qu’ils sont uniques dans le *films* table.

Fermez l’onglet qui contient le Concepteur de base de données.

Vous pouvez maintenant activer à l’affichage de ces données sur une page web.

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a>Affichage des données dans une Page à l’aide du programme d’assistance WebGrid

Pour afficher des données dans une page, vous allez utiliser le `WebGrid` helper. Ce programme d’assistance produit un affichage dans une grille ou une table (lignes et colonnes). Comme vous le verrez, vous serez en mesure de redéfinir la grille avec mise en forme et d’autres fonctionnalités.

Pour exécuter la grille, vous devrez écrire quelques lignes de code. Ces quelques lignes servira à un type de modèle pour presque tous les accès aux données que vous effectuez dans ce didacticiel.

> [!NOTE]
> Vous disposez effectivement de nombreuses options pour afficher les données sur une page ; le `WebGrid` n’est qu’un programme d’assistance. A été choisi pour ce didacticiel, car c’est le moyen le plus simple pour afficher des données et où il est relativement flexible. Dans le didacticiel suivantes, vous verrez comment utiliser une méthode plus « manual » pour travailler avec des données dans la page, ce qui vous donne un contrôle plus direct sur comment afficher les données.


Dans le volet gauche de WebMatrix, cliquez sur le **fichiers** espace de travail.

La nouvelle base de données que vous avez créé est dans le *application\_données* dossier. Si le dossier n’existe déjà, WebMatrix créée pour votre nouvelle base de données. (Le dossier ont pu exister si vous aviez précédemment installé des programmes d’assistance.)

Dans l’arborescence, sélectionnez la racine du site Web. Vous devez sélectionner la racine du site Web ; Sinon, le nouveau fichier peut être ajouté à l’application\_dossier de données.

Dans le ruban, cliquez sur **New**. Dans le **choisir un Type de fichier** , sélectionnez **CSHTML**.

Dans le **nom** boîte, nommez la nouvelle page « Movies.cshtml » :

!['Choisir un Type de fichier' de la boîte de dialogue affichant la page « Movies »](displaying-data/_static/image17.png)

Cliquez sur le **OK** bouton. WebMatrix ouvre un nouveau fichier avec certains éléments squelettes qu’il contient. Tout d’abord, vous allez écrire du code pour obtenir les données à partir de la base de données. Puis vous allez ajouter le balisage à la page pour afficher les données.

### <a name="writing-the-data-query-code"></a>Écriture du Code de requête de données

En haut de la page, entre le `@{` et `}` caractères, entrez le code suivant. (Assurez-vous que vous entrez ce code entre les accolades ouvrantes et fermantes.)

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

La première ligne ouvre la base de données que vous avez créé précédemment, qui est toujours la première étape avant de procéder à un élément avec la base de données. Vous indiquez le `Database.Open` nom de la méthode de la base de données à ouvrir. Notez que vous n’incluez pas *.sdf* dans le nom. Le `Open` méthode suppose qu’il recherche un *.sdf* fichier (autrement dit, *WebPagesMovies.sdf*) et que le *.sdf* fichier se trouve dans le *application\_ Données* dossier. (Précédemment, nous l’avons dit que le *application\_données* dossier est réservé ; ce scénario est un des emplacements où ASP.NET sur les hypothèses sur ce nom.)

Lorsque la base de données est ouverte, une référence à celle-ci est placée dans la variable nommée `db`. (Ce qui peut être n’importe quel nom.) Le `db` variable est que vous obtiendrez comment interagir avec la base de données.

La deuxième ligne extrait réellement la base de données à l’aide de la `Query` (méthode). Notez le fonctionne de ce code : le `db` variable a une référence à la base de données ouvert et que vous appelez le `Query` (méthode) à l’aide de la `db` variable (`db.Query`).

La requête elle-même est une instance SQL `Select` instruction. (Pour quelques notions de base sur SQL, consultez l’explication ultérieurement.) Dans l’instruction, `Movies` identifie la table à la requête. Le `*` caractère spécifie que la requête doit retourner toutes les colonnes de la table. (Vous pouvez également répertorier les colonnes individuellement, séparés par des virgules.)

Les résultats de la requête, le cas échéant, sont renvoyés et mis à disposition dans le `selectedData` variable. Là encore, la variable peut être n’importe quel nom.

Enfin, la troisième ligne indique à ASP.NET que vous souhaitez utiliser une instance de la `WebGrid` helper. Vous créez (*instancier*) l’objet d’assistance à l’aide de la `new` mot clé et le passer les résultats de requête par le biais de la `selectedData` variable. La nouvelle `WebGrid` objet, ainsi que les résultats de la requête de base de données, sont accessibles dans le `grid` variable. Vous aurez besoin de ce résultat dans un instant pour afficher les données dans la page.

À ce stade, la base de données a été ouverte, vous avez obtenu les données souhaitées, et que vous avez préparé la `WebGrid` helper avec ces données. Est ensuite pour créer le balisage dans la page.

> [!TIP] 
> 
> **Structured Query Language (SQL)**
> 
> SQL est un langage qui est utilisé dans la plupart des bases de données relationnelles pour la gestion des données dans une base de données. Il inclut des commandes qui permettent de récupérer des données et mettre à jour, et qui vous permettent de créer, modifier et gérer des données dans les tables de base de données. SQL est différent de celui d’un langage de programmation (tel que c#). Avec SQL, vous indiquer la base de données à ce que vous voulez, et travail de la base de données consiste à déterminer comment obtenir les données ou d’effectuer la tâche. Voici des exemples de certaines commandes SQL et ce qu’ils font :
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> La première `Select` instruction Obtient toutes les colonnes (spécifié par `*`) à partir de la *films* table.
> 
> La seconde `Select` instruction extrait les colonnes ID, nom et prix à partir d’enregistrements dans le *produit* table dont valeur de colonne de prix est supérieur à 10. La commande retourne les résultats par ordre alphabétique en fonction des valeurs de la colonne nom. Si aucun enregistrement correspondent aux critères de prix, la commande retourne un jeu vide.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> Cette commande permet d’insérer un nouvel enregistrement dans le *produit* table, la définition de la colonne de nom pour le « Croissant », la colonne de Description pour « Plaisir de non fiable A » et le prix à 1,99.
> 
> Notez que lorsque vous spécifiez une valeur non numérique, la valeur est placée entre guillemets simples (pas de doubles guillemets, comme dans c#). Vous utilisez ces guillemets autour des valeurs de texte ou de date, mais pas autour de nombres.
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> Cette commande supprime les enregistrements dans le *produit* table dont colonne de date d’expiration est antérieure au 1er janvier 2008. (La commande suppose que le *produit* table a une telle colonne, bien sûr.) La date est entrée ici au format MM/jj/aaaa, mais il doit être entré au format qui est utilisé pour vos paramètres régionaux.
> 
> Le `Insert` et `Delete` commandes ne retournent des jeux de résultats. Au lieu de cela, elles retournent un nombre qui indique le nombre d’enregistrements ont été affecté par la commande.
> 
> Pour certaines de ces opérations (par exemple, insertion et suppression d’enregistrements), le processus qui demande l’opération doit disposer des autorisations appropriées dans la base de données. C’est pourquoi des bases de données de production vous avez généralement à fournir un nom d’utilisateur et le mot de passe lorsque vous vous connectez à la base de données.
> 
> Il existe des douzaines de commandes SQL, mais elles suivent un modèle, comme les commandes que vous voyez ici. Vous pouvez utiliser des commandes SQL pour créer des tables de base de données, compter le nombre d’enregistrements dans une table, calculer les prix et effectuer de nombreuses opérations plus.


### <a name="adding-markup-to-display-the-data"></a>Ajout de balisage pour afficher les données

À l’intérieur de la `<head>` élément, le contenu de l’ensemble de la `<title>` élément à « Movies » :

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

À l’intérieur du `<body>` élément de la page, ajoutez le code suivant :

[!code-html[Main](displaying-data/samples/sample3.html)]

C'est tout ! Le `grid` variable est la valeur que vous avez créé lorsque vous avez créé le `WebGrid` objet dans le code précédemment.

Dans l’arborescence de WebMatrix, cliquez sur la page et sélectionnez **lancer dans le navigateur**. Vous verrez quelque chose comme cette page :

![Sortie de d’assistance WebGrid par défaut de la table de films](displaying-data/_static/image18.png)

Cliquez sur un lien d’en-tête de colonne pour trier selon cette colonne. Pour trier en cliquant sur un en-tête est une fonctionnalité qui est intégrée à la **WebGrid** helper.

Le `GetHtml` méthode, comme son nom l’indique, génère un balisage qui affiche les données. Par défaut, le `GetHtml` méthode génère un élément HTML `<table>` élément. (Si vous le souhaitez, vous pouvez vérifier le rendu en examinant la source de la page dans le navigateur.)

## <a name="modifying-the-look-of-the-grid"></a>Modifier l’apparence de la grille

À l’aide de la `WebGrid` helper comme vous venez de faire est facile, mais l’affichage qui en résulte est simple. Le `WebGrid` helper a toutes sortes d’options qui vous permettent de contrôler comment les données sont affichées. Il existe beaucoup trop nombreux à découvrir dans ce didacticiel, mais cette section vous donnera une idée de certaines de ces options. Quelques options supplémentaires seront abordées dans d’autres didacticiels de cette série.

### <a name="specifying-individual-columns-to-display"></a>Spécification des colonnes individuelles à afficher

Pour commencer, vous pouvez spécifier que vous souhaitez afficher uniquement certaines colonnes. Par défaut, comme vous l’avez vu, la grille affiche les quatre colonnes de la *films* table.

Dans le *Movies.cshtml* fichier, remplacez le `@grid.GetHtml()` balisage que vous venez d’ajouter par le code suivant :

[!code-css[Main](displaying-data/samples/sample4.css)]

Pour indiquer les colonnes à afficher à l’application d’assistance, vous incluez un `columns` paramètre pour le `GetHtml` (méthode) et passez une collection de colonnes. Dans la collection, vous spécifiez chaque colonne à inclure. Vous spécifiez une colonne individuelle à afficher en incluant un `grid.Column` l’objet et de transmettre le nom de la colonne de données que vous souhaitez. (Ces colonnes doivent être incluses dans les résultats de requête SQL, le `WebGrid` helper ne peut pas afficher les colonnes qui n’ont pas été renvoyés par la requête.)

Lancer le *Movies.cshtml* page dans le navigateur et cette fois, vous obtenez un affichage semblable à celle qui suit (Notez qu’aucune colonne d’ID ne s’affiche) :

![Affichage de WebGrid affiche uniquement les colonnes sélectionnées](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a>Changer l’aspect de la grille

Il existe un certain davantage d’options pour afficher des colonnes, certains d'entre eux sont présentés dans les didacticiels suivants dans cet ensemble. Pour l’instant, cette section vous présentera façons dans lequel vous pouvez appliquer un style de la grille dans son ensemble.

À l’intérieur de la `<head>` section de la page, juste avant la fermeture `</head>` balise, ajoutez le code suivant `<style>` élément :

[!code-css[Main](displaying-data/samples/sample5.css)]

Ce balisage CSS définit des classes nommées `grid`, `head`, et ainsi de suite. Vous pouvez également placer ces définitions de style dans une fonction *.css* de fichiers et lier ceci à la page. (En fait, vous le ferez plus loin dans cet ensemble de didacticiels.) Mais pour simplifier les choses pour ce didacticiel, elles sont à l’intérieur de la même page qui affiche les données.

Vous pouvez désormais le `WebGrid` helper pour utiliser ces classes de style. Le programme d’assistance a un nombre de propriétés (par exemple, `tableStyle`) dans ce but, vous leur attribuez un nom de classe de style CSS, et ce nom de classe est rendu en tant que partie du balisage restitué par l’application d’assistance.

Modifier le `grid.GetHtml` balisage afin qu’il ressemble à ce code :

[!code-css[Main](displaying-data/samples/sample6.css)]

Quelles sont les nouveautés ici est que vous avez ajouté `tableStyle`, `headerStyle`, et `alternatingRowStyle` paramètres pour le `GetHtml` (méthode). Ces paramètres ont été définis pour les noms des styles CSS que vous avez ajouté un moment il y a.

Exécutez la page, et cette fois, vous voyez une grille qui ressemble beaucoup moins simple qu’avant :

![Affichage de WebGrid qui inclut les paramètres définis pour les noms de classe CSS](displaying-data/_static/image20.png)

Pour connaître la `GetHtml` méthode générée, vous pouvez consulter la source de la page dans le navigateur. Nous n’entrerons pas dans les détails ici, mais le point important est que, en spécifiant des paramètres tels que `tableStyle`, vous a provoqué la grille générer des balises HTML comme suit :

`<table class="grid">`

Le `<table>` tag a eu un `class` attribut ajouté à ce dernier qui fait référence à une des règles CSS que vous avez ajouté précédemment. Ce code vous montre le modèle de base &mdash; des paramètres différents pour le `GetHtml` méthode let vous référence CSS classes que la méthode génère ensuite avec le balisage. Les opérations avec les classes CSS vous revient.

## <a name="adding-paging"></a>Ajout de pagination

En tant que la dernière tâche pour ce didacticiel, vous allez ajouter la pagination à la grille. Pour l’instant ne pose aucun problème pour afficher tous vos films à la fois. Mais si vous avez ajouté des centaines de films, l’affichage de page long.

Dans le code de page, remplacez la ligne qui crée le `WebGrid` objet pour le code suivant :

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

La seule différence avec avant est que vous avez ajouté un `rowsPerPage` paramètre est défini sur 3.

Exécutez la page. La grille affiche les 3 lignes à une heure, ainsi que des liens de navigation qui permettent de parcourir sur les films dans votre base de données :

![Affichage de WebGrid avec pagination](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a>Prochaine

Dans le didacticiel suivant, vous allez apprendre à utiliser le code Razor et c# pour obtenir une entrée d’utilisateur dans un formulaire. Vous allez ajouter une zone de recherche à la page Movies afin que vous pouvez rechercher les films par titre ou le genre.

## <a name="complete-listing-for-movies-page"></a>Liste complète de la Page de films

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a>Ressources supplémentaires

- [Introduction à la programmation Web ASP.NET à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkID=202890)

> [!div class="step-by-step"]
> [Précédent](intro-to-web-pages-programming.md)
> [Suivant](form-basics.md)
