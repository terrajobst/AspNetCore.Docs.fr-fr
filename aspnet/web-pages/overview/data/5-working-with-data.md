---
uid: web-pages/overview/data/5-working-with-data
title: Introduction à l’utilisation avec une base de données dans ASP.NET Web Pages (Razor) Sites | Microsoft Docs
author: Rick-Anderson
description: Ce chapitre décrit comment accéder aux données à partir d’une base de données et les afficher à l’aide d’ASP.NET Web Pages.
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 673d502f-2c16-4a6f-bb63-dbfd9a77ef47
msc.legacyurl: /web-pages/overview/data/5-working-with-data
msc.type: authoredcontent
ms.openlocfilehash: a688761c87376aa93463c13eaa07858d3acb9dc2
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021688"
---
<a name="introduction-to-working-with-a-database-in-aspnet-web-pages-razor-sites"></a>Introduction à l’utilisation avec une base de données dans ASP.NET Web Pages (Razor) Sites
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article décrit comment utiliser les outils de Microsoft WebMatrix pour créer une base de données dans un site Web ASP.NET Web Pages (Razor) et comment créer des pages qui vous permettent d’afficher, ajouter, modifier et supprimer des données.
> 
> **Ce que vous allez apprendre :** 
> 
> - Comment créer une base de données.
> - Comment se connecter à une base de données.
> - Comment afficher des données dans une page web.
> - Guide pratique pour insérer, mettre à jour et supprimer des enregistrements de base de données.
> 
> Voici les fonctionnalités introduites dans l’article :
> 
> - Utilisez une base de données Microsoft SQL Server Compact Edition.
> - Utilisation de requêtes SQL.
> - La classe `Database`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> Ce didacticiel fonctionne également avec WebMatrix 3. Vous pouvez utiliser 3 des Pages Web ASP.NET et Visual Studio 2013 (ou Visual Studio Express 2013 pour le Web) ; Toutefois, l’interface utilisateur sera différente.


## <a name="introduction-to-databases"></a>Introduction aux bases de données

Imaginez un carnet d’adresses classiques. Pour chaque entrée dans le carnet d’adresses (autrement dit, pour chaque personne) vous avez plusieurs éléments d’informations telles que le prénom, nom, adresse, adresse de messagerie et numéro de téléphone.

Un moyen classique aux données d’image comme cela est comme une table avec des lignes et colonnes. En termes de base de données, chaque ligne est souvent appelé un enregistrement. Chaque colonne (parfois appelée champs) contient une valeur pour chaque type de données : prénom, nom du dernier et ainsi de suite.

| **ID** | **firstName** | **lastName** | **Adresse** | **E-mail** | **Téléphone** |
| --- | --- | --- | --- | --- | --- |
| 1 | Jim | Abrus | 210 100th St SE Orcas WA 98031 | jim@contoso.com | 555 0100 |
| 2 | Terry | Adams | 1234 Main St. Seattle WA 99011 | terry@cohowinery.com | 555 0101 |

Pour la plupart des tables de base de données, la table doit avoir une colonne qui contient un identificateur unique, comme un numéro de client, un numéro de compte, un périphérique. Il s’agit de la table *clé primaire*, et utilisez-la pour identifier chaque ligne dans la table. Dans l’exemple, la colonne ID est la clé primaire pour le carnet d’adresses.

Avec cette compréhension élémentaire des bases de données, vous êtes prêt à découvrir comment créer une base de données simple et effectuer des opérations telles que l’ajout, modification et suppression de données.

> [!TIP] 
> 
> **Bases de données relationnelles**
> 
> Vous pouvez stocker des données dans un grand nombre de façons, notamment les feuilles de calcul et des fichiers texte. Pour la plupart des utilisations professionnelles, cependant, les données sont stockées dans une base de données relationnelle.
> 
> Cet article n’accède pas très profondément dans les bases de données. Toutefois, vous pourrez s’avérer utile de comprendre un peu à leur sujet. Dans une base de données relationnelle, informations sont logiquement divisées en tables distinctes. Par exemple, une base de données pour une école peut contenir des tables distinctes pour les étudiants et des offres de la classe. Les base de données logiciels (tels que SQL Server) prend en charge des commandes puissantes qui vous permettent de manière dynamique établissent des relations entre les tables. Par exemple, vous pouvez utiliser la base de données relationnelle pour établir une relation logique entre les étudiants et les classes pour créer une planification. Stockage des données dans des tables distinctes réduit la complexité de la structure de table et réduit la nécessité de conserver les données redondantes dans les tables.


## <a name="creating-a-database"></a>Création d’une base de données

Cette procédure vous montre comment créer une base de données nommée SmallBakery à l’aide de l’outil de conception de base de données SQL Server Compact est inclus dans WebMatrix. Bien que vous pouvez créer une base de données à l’aide de code, il est plus courant pour créer la base de données et les tables de base de données à l’aide d’un outil de conception tels que WebMatrix.

1. Démarrez WebMatrix et dans la page de démarrage rapide, cliquez sur **Site à partir du modèle**.
2. Sélectionnez **Site vide**, puis, dans le **nom du Site** Entrez « SmallBakery » et cliquez sur **OK**. Le site est créé et affiché dans WebMatrix.
3. Dans le volet gauche, cliquez sur le **bases de données** espace de travail.
4. Dans le ruban, cliquez sur **nouvelle base de données**. Une base de données vide est créé avec le même nom que votre site.
5. Dans le volet gauche, développez le **SmallBakery.sdf** nœud, puis cliquez sur **Tables**.
6. Dans le ruban, cliquez sur **nouvelle Table**. WebMatrix s’ouvre le Concepteur de tables.

    ![[image]](5-working-with-data/_static/image1.jpg)
7. Cliquez dans le **nom** colonne et entrer &quot;Id&quot;.
8. Dans le **Type de données** colonne, sélectionnez **int**.
9. Définir le **est la clé primaire ?** et **est identifier ?** options **Oui**.

    Comme son nom l’indique, **est la clé primaire** indique à la base de données qu’il s’agit de la clé primaire de table. **Est l’identité** indique à la base de données pour créer automatiquement un numéro d’identification pour chaque nouvel enregistrement et attribuez-lui le numéro séquentiel suivant (commençant à 1).
10. Cliquez sur la ligne suivante. L’éditeur démarre une nouvelle définition de colonne.
11. Pour la valeur de nom, entrez &quot;nom&quot;.
12. Pour **Type de données**, choisissez &quot;nvarchar&quot; et définir la longueur à 50. Le *var* dans le cadre de `nvarchar` indique à la base de données que les données de cette colonne sera une chaîne dont la taille peut varier d’un enregistrement à l’autre. (Le *n* préfixe représente *national*, indiquant que le champ peut contenir des données de caractères qui représente n’importe quel ordre alphabétique ou système d’écriture &#8212; , autrement dit, que le champ contient des données Unicode.)
13. Définir le **autoriser les valeurs null** option **non**. Cela qui applique le *nom* colonne n’est pas vide.
14. À l’aide de ce même processus, créez une colonne nommée *Description*. Définissez **Type de données** à « nvarchar » et 50 pour la longueur, puis définissez **autoriser les valeurs null** avec la valeur false.
15. Créez une colonne nommée *prix*. Définissez **Type de données « argent »** et définissez **autoriser les valeurs null** avec la valeur false.
16. Dans la zone en haut, nommez la table &quot;produit&quot;.

    Lorsque vous avez terminé, la définition ressemblera à ceci :

    ![[image]](5-working-with-data/_static/image2.jpg)
17. Appuyez sur Ctrl + S pour enregistrer la table.

## <a name="adding-data-to-the-database"></a>Ajout de données à la base de données

Vous pouvez maintenant ajouter des exemples de données à votre base de données que vous allez utiliser plus loin dans l’article.

1. Dans le volet gauche, développez le **SmallBakery.sdf** nœud, puis cliquez sur **Tables**.
2. Avec le bouton droit de la table Product, puis **données**.
3. Dans le volet d’édition, entrez les enregistrements suivants :

    | **Name** | **Description** | **Prix** |
    | --- | --- | --- |
    | Pain | Nouvelle cuite tous les jours. | 2.99 |
    | Strawberry Shortcake | Effectué avec fraises organiques à partir de notre domaine privé. | 9.99 |
    | Tarte | Second uniquement pour les secteurs de votre mère. | 12.99 |
    | Graphique à secteurs pecan | Si vous aimez du Queensland, voici pour vous. | 10.99 |
    | Graphique à secteurs citron | Effectuées avec les meilleure citrons dans le monde. | 11.99 |
    | Cupcakes | Vos enfants et l’élément kid dans vous adorerez ces. | 7.99 |

    N’oubliez pas que vous n’êtes pas obligé d’entrer des informations pour le *Id* colonne. Lorsque vous avez créé le *Id* colonne, vous définissez son **est d’identité** true, ce qui entraîne sa automatiquement à la propriété.

    Lorsque vous avez terminé la saisie de données, le Concepteur de tables ressemblera à ceci :

    ![[image]](5-working-with-data/_static/image3.jpg)
4. Fermez l’onglet qui contient les données de base de données.

## <a name="displaying-data-from-a-database"></a>Affichage des données à partir d’une base de données

Une fois que vous avez une base de données avec des données qu’il contient, vous pouvez afficher les données dans une page web ASP.NET. Pour sélectionner les lignes de table à afficher, vous utilisez une instruction SQL, qui est une commande que vous passez à la base de données.

1. Dans le volet gauche, cliquez sur le **fichiers** espace de travail.
2. À la racine du site Web, créez une nouvelle page CSHTML nommée *ListProducts.cshtml*.
3. Remplacez le balisage existant par le code suivant :

    [!code-cshtml[Main](5-working-with-data/samples/sample1.cshtml)]

    Dans le premier bloc de code, vous ouvrez le *SmallBakery.sdf* fichier (base de données) que vous avez créé précédemment. Le `Database.Open` méthode suppose que le *.sdf* fichier est en cours de votre site Web *application\_données* dossier. (Notez que vous n’avez pas besoin de spécifier le *.sdf* extension &#8212; en fait, si vous le faites, les `Open` méthode ne fonctionne pas.)

    > [!NOTE]
    > Le *application\_données* dossier est un dossier spécial qui est utilisé pour stocker les fichiers de données dans ASP.NET. Pour plus d’informations, consultez [connexion à une base de données](#SB_ConnectingToADatabase) plus loin dans cet article.

    Vous effectuez ensuite une requête pour interroger la base de données à l’aide de la commande SQL suivante `Select` instruction :

    [!code-sql[Main](5-working-with-data/samples/sample2.sql)]

    Dans l’instruction, `Product` identifie la table à la requête. Le `*` caractère spécifie que la requête doit retourner toutes les colonnes de la table. (Vous pouvez également répertorier les colonnes individuellement, séparés par des virgules, si vous souhaitez consulter uniquement certaines colonnes.) Le `Order By` clause indique comment les données doivent être triées &#8212; dans ce cas, par le *nom* colonne. Cela signifie que les données sont triées par ordre alphabétique en fonction de la valeur de la *nom* colonne pour chaque ligne.

    Dans le corps de la page, le balisage crée une table HTML qui doit être utilisée pour afficher les données. À l’intérieur de la `<tbody>` élément, vous utilisez un `foreach` boucle s’individuellement chaque ligne de données qui est retourné par la requête. Pour chaque ligne de données, vous créez une ligne de table HTML (`<tr>` élément). Puis vous créez des cellules de tableau HTML (`<td>` éléments) pour chaque colonne. Chaque fois que vous parcourez la boucle, la ligne suivante à partir de la base de données est dans le `row` variable (vous le configurer dans le `foreach` instruction). Pour obtenir une colonne individuelle à partir de la ligne, vous pouvez utiliser `row.Name` ou `row.Description` ou le nom de votre choix est de la colonne que vous souhaitez.
4. Exécutez la page dans un navigateur. (Assurez-vous que la page est sélectionnée dans le **fichiers** espace de travail avant de l’exécuter.) La page affiche une liste comme suit :

    ![[image]](5-working-with-data/_static/image4.jpg)

> [!TIP] 
> 
> **Structured Query Language (SQL)**
> 
> SQL est un langage qui est utilisé dans la plupart des bases de données relationnelles pour la gestion des données dans une base de données. Il inclut des commandes qui permettent de récupérer des données et mettre à jour, et qui vous permettent de créer, modifier et gérer des tables de base de données. SQL est différent de celui d’un langage de programmation (comme celui que vous utilisez dans WebMatrix), car avec SQL, l’idée est que vous indiquer la base de données à ce que vous voulez, et travail de la base de données consiste à déterminer comment obtenir les données ou d’effectuer la tâche. Voici des exemples de certaines commandes SQL et ce qu’ils font :
> 
> `SELECT Id, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> Cela extrait les *Id*, *nom*, et *prix* colonnes à partir des enregistrements dans la *produit* table si la valeur de *prix* est supérieur à 10 et retourne les résultats par ordre alphabétique en fonction des valeurs de la *nom* colonne. Cette commande renvoie un jeu de résultats qui contient les enregistrements qui répondent aux critères, ou un jeu vide si aucun enregistrement ne correspond.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ("Croissant", "A flaky delight", 1.99)`
> 
> Cela permet d’insérer un nouvel enregistrement dans le *produit* table, la définition la *nom* colonne à &quot;Croissant&quot;, le *Description* colonne à &quot; Une satisfaction friables&quot;et le prix à 1,99.
> 
> `DELETE FROM Product WHERE ExpirationDate < "01/01/2008"`
> 
> Cette commande supprime les enregistrements dans le *produit* table dont colonne de date d’expiration est antérieure au 1er janvier 2008. (Cela suppose que le *produit* table a une telle colonne, bien sûr.) La date est entrée ici au format MM/jj/aaaa, mais il doit être entré au format qui est utilisé pour vos paramètres régionaux.
> 
> Le `Insert Into` et `Delete` commandes ne retournent des jeux de résultats. Au lieu de cela, elles retournent un nombre qui indique le nombre d’enregistrements ont été affecté par la commande.
> 
> Pour certaines de ces opérations (par exemple, insertion et suppression d’enregistrements), le processus qui demande l’opération doit disposer des autorisations appropriées dans la base de données. C’est pourquoi des bases de données de production vous avez généralement à fournir un nom d’utilisateur et le mot de passe lorsque vous vous connectez à la base de données.
> 
> Il existe des douzaines de commandes SQL, mais elles suivent un modèle comme celui-ci. Vous pouvez utiliser des commandes SQL pour créer des tables de base de données, compter le nombre d’enregistrements dans une table, calculer les prix et effectuer de nombreuses opérations plus.


## <a name="inserting-data-in-a-database"></a>Insertion de données dans une base de données

Cette section montre comment créer une page qui permet aux utilisateurs d’ajouter un nouveau produit à la *produit* table de base de données. Après l’insertion d’un nouvel enregistrement de produit, la page affiche la table mise à jour à l’aide de la *ListProducts.cshtml* page que vous avez créé dans la section précédente.

La page inclut la validation pour vous assurer que les données entrées par l’utilisateur sont valides pour la base de données. Par exemple, le code dans la page permet de s’assurer qu’une valeur a été entrée pour toutes les colonnes requises.

1. Dans le site Web, créez un nouveau fichier CSHTML nommé *InsertProducts.cshtml*.
2. Remplacez le balisage existant par le code suivant :

    [!code-cshtml[Main](5-working-with-data/samples/sample3.cshtml)]

    Le corps de la page contient un formulaire HTML avec trois zones de texte qui permettent aux utilisateurs d’entrer un nom, la description et le prix. Lorsque les utilisateurs cliquent sur le **insérer** bouton, le code en haut de la page ouvre une connexion à la *SmallBakery.sdf* base de données. Vous obtenez ensuite les valeurs que l’utilisateur a soumis à l’aide de la `Request` de l’objet et affecter ces valeurs aux variables locales.

    Pour valider que l’utilisateur a entré une valeur pour chaque colonne requise, vous inscrivez chaque `<input>` élément que vous souhaitez valider :

    [!code-csharp[Main](5-working-with-data/samples/sample4.cs)]

    Le `Validation` helper vérifie qu’il existe une valeur dans chacun des champs que vous avez enregistré. Vous pouvez tester si tous les champs passé la validation en vérifiant `Validation.IsValid()`, qui vous le faites habituellement avant de traiter les informations que vous obtenez à partir de l’utilisateur :

    [!code-csharp[Main](5-working-with-data/samples/sample5.cs)]

    (Le `&&` moyen de l’opérateur AND : ce test est *s’il s’agit d’un envoi d’un formulaire et tous les champs ont réussi la validation*.)

    Si toutes les colonnes à valider (aucun n’était vide), vous continuez et créez une instruction SQL pour insérer les données, puis l’exécuter comme indiqué ci-après :

    [!code-csharp[Main](5-working-with-data/samples/sample6.cs)]

    Pour les valeurs à insérer, vous incluez les espaces réservés de paramètre (`@0`, `@1`, `@2`).

    > [!NOTE]
    > Par mesure de sécurité, toujours passer des valeurs à une instruction SQL à l’aide des paramètres, comme vous le voir dans l’exemple précédent. Cela vous permet de valider les données de l’utilisateur, ainsi que permet de se prémunir contre les tentatives d’envoi des commandes nuisibles à votre base de données (parfois appelée attaques par injection SQL).

    Pour exécuter la requête, vous utilisez cette instruction, en lui transmettant les variables qui contiennent les valeurs pour remplacer les espaces réservés :

    [!code-csharp[Main](5-working-with-data/samples/sample7.cs)]

    Après le `Insert Into` instruction est exécutée, vous envoyez à l’utilisateur à la page qui répertorie les produits à l’aide de cette ligne :

    [!code-javascript[Main](5-working-with-data/samples/sample8.js)]

    Si la validation a échoué, vous ignorez l’insertion. Au lieu de cela, vous avez une application d’assistance dans la page qui peut afficher les messages d’erreur cumulée (le cas échéant) :

    [!code-cshtml[Main](5-working-with-data/samples/sample9.cshtml)]

    Notez que le bloc de style dans le balisage inclut une définition de classe CSS nommée `.validation-summary-errors`. Il s’agit du nom de la classe CSS qui est utilisé par défaut pour le `<div>` élément qui contient des erreurs de validation. Dans ce cas, la classe CSS spécifie que les erreurs de résumé de validation sont affichés en rouge et en gras, mais vous pouvez définir la `.validation-summary-errors` classe afin d’afficher toute mise en forme que vous le souhaitez.

### <a name="testing-the-insert-page"></a>Test de la Page d’insertion

1. Afficher la page dans un navigateur. La page affiche un formulaire qui est similaire à celui qui est indiqué dans l’illustration suivante.

    ![[image]](5-working-with-data/_static/image5.jpg)
2. Entrez des valeurs pour toutes les colonnes, mais veillez à ce que vous laissez le *prix* colonne vide.
3. Cliquez sur **Insérer**. La page affiche un message d’erreur, comme indiqué dans l’illustration suivante. (Aucun nouvel enregistrement n’est créé).

    ![[image]](5-working-with-data/_static/image6.jpg)
4. Remplissez le formulaire complètement, puis cliquez sur **insérer**. Cette fois-ci, le *ListProducts.cshtml* page s’affiche et présente le nouvel enregistrement.

## <a name="updating-data-in-a-database"></a>La mise à jour des données dans une base de données

Une fois que les données ont été entrées dans une table, vous devrez peut-être mettre à jour. Cette procédure vous montre comment créer deux pages sont similaires à ceux que vous avez créé pour l’insertion de données précédemment. La première page affiche les produits et permet aux utilisateurs de sélectionner un à modifier. La deuxième page permet des utilisateurs réellement apporter les modifications et les enregistrer.

> [!NOTE] 
> 
> **Important** dans un site Web de production, vous généralement Limitez les personnes autorisées à apporter des modifications aux données. Pour plus d’informations sur comment configurer l’appartenance et sur les façons d’autoriser les utilisateurs à effectuer des tâches sur le site, consultez [Ajout de la sécurité et l’appartenance à un Site ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202904).


1. Dans le site Web, créez un nouveau fichier CSHTML nommé *EditProducts.cshtml*.
2. Remplacez le balisage existant dans le fichier avec les éléments suivants :

    [!code-cshtml[Main](5-working-with-data/samples/sample10.cshtml)]

    La seule différence entre cette page et le *ListProducts.cshtml* page à partir de versions antérieures est que la table HTML dans cette page inclut une colonne supplémentaire qui affiche un **modifier** lien. Lorsque vous cliquez sur ce lien, il vous amène directement à la *UpdateProducts.cshtml* page (que vous créerez ensuite) dans laquelle vous pouvez modifier l’enregistrement sélectionné.

    Examinez le code qui crée le **modifier** lien :

    [!code-cshtml[Main](5-working-with-data/samples/sample11.cshtml)]

    Cette opération crée un élément HTML `<a>` élément dont `href` attribut est défini de manière dynamique. Le `href` attribut spécifie la page à afficher lorsque l’utilisateur clique sur le lien. Il passe également le `Id` valeur de la ligne actuelle pour le lien. Lorsque la page s’exécute, la page source peut contenir des liens comme celles-ci :

    [!code-html[Main](5-working-with-data/samples/sample12.html)]

    Notez que le `href` attribut a la valeur `UpdateProducts/n`, où *n* est un numéro de produit. Lorsqu’un utilisateur clique sur un de ces liens, l’URL qui en résulte se présente comme suit :

    `http://localhost:18816/UpdateProducts/6`

    En d’autres termes, la modification du numéro de produit est passé dans l’URL.
3. Afficher la page dans un navigateur. La page affiche les données dans un format comme suit :

    ![[image]](5-working-with-data/_static/image7.jpg)

    Ensuite, vous allez créer la page qui permet aux utilisateurs de mettre à jour les données. La page de mise à jour inclut la validation pour valider les données entrées par l’utilisateur. Par exemple, le code dans la page permet de s’assurer qu’une valeur a été entrée pour toutes les colonnes requises.
4. Dans le site Web, créez un nouveau fichier CSHTML nommé *UpdateProducts.cshtml*.
5. Remplacez le balisage existant dans le fichier avec les éléments suivants.

    [!code-cshtml[Main](5-working-with-data/samples/sample13.cshtml)]

    Le corps de la page contient un formulaire HTML dans lequel un produit est affiché et où les utilisateurs peuvent la modifier. Pour obtenir le produit à afficher, vous utilisez cette instruction SQL :

    [!code-sql[Main](5-working-with-data/samples/sample14.sql)]

    Il sélectionne le produit dont l’ID correspond à la valeur passée dans le `@0` paramètre. (Étant donné que *Id* est la clé primaire et par conséquent doivent être uniques, produit qu’un seul enregistrement peut jamais être sélectionné ainsi.) Pour obtenir la valeur d’ID pour passer à ce `Select` instruction, vous pouvez lire la valeur qui est passée à la page dans le cadre de l’URL, à l’aide de la syntaxe suivante :

    [!code-csharp[Main](5-working-with-data/samples/sample15.cs)]

    Pour extraire l’enregistrement du produit, vous utilisez le `QuerySingle` (méthode), qui renvoie simplement un enregistrement :

    [!code-csharp[Main](5-working-with-data/samples/sample16.cs)]

    La ligne est retournée dans le `row` variable. Vous pouvez obtenir des données de chaque colonne et affectez-le à des variables locales comme suit :

    [!code-csharp[Main](5-working-with-data/samples/sample17.cs)]

    Dans le balisage pour le formulaire, ces valeurs sont affichent automatiquement dans les zones de texte individuelles à l’aide de code incorporé comme suit :

    [!code-html[Main](5-working-with-data/samples/sample18.html)]

    Cette partie du code affiche l’enregistrement du produit à mettre à jour. Une fois que l’enregistrement a été affiché, l’utilisateur peut modifier des colonnes individuelles.

    Lorsque l’utilisateur envoie le formulaire en cliquant sur le **mise à jour** bouton, le code dans le `if(IsPost)` bloquer s’exécute. Ceci permet d’obtenir les valeurs de l’utilisateur à partir de la `Request` objet, stocke les valeurs dans des variables et valide le fait que chaque colonne a été rempli. Si la validation réussit, le code crée l’instruction SQL Update suivante :

    [!code-sql[Main](5-working-with-data/samples/sample19.sql)]

    Dans SQL `Update` instruction, vous spécifiez chaque colonne mise à jour et la valeur pour le définir. Dans ce code, les valeurs sont spécifiées à l’aide des espaces réservés de paramètre `@0`, `@1`, `@2`, et ainsi de suite. (Comme indiqué précédemment, pour la sécurité, vous devez toujours passer les valeurs à une instruction SQL à l’aide de paramètres.)

    Lorsque vous appelez le `db.Execute` (méthode), vous passez les variables qui contiennent les valeurs dans l’ordre qui correspond aux paramètres dans l’instruction SQL :

    [!code-csharp[Main](5-working-with-data/samples/sample20.cs)]

    Après le `Update` instruction a été exécutée, vous appelez la méthode suivante afin de rediriger l’utilisateur vers la page de modification :

    [!code-cshtml[Main](5-working-with-data/samples/sample21.cshtml)]

    L’effet est que l’utilisateur voit un listing de mise à jour des données dans la base de données et permet de modifier un autre produit.
6. Enregistrez la page.
7. Exécutez le *EditProducts.cshtml* page (et non la page de mise à jour), puis sur **modifier** sélectionner un produit à modifier. Le *UpdateProducts.cshtml* page s’affiche, montrant l’enregistrement que vous avez sélectionné.

    ![[image]](5-working-with-data/_static/image8.jpg)
8. Apportez une modification, puis cliquez sur **mise à jour**. La liste de produits s’affiche à nouveau avec vos données mises à jour.

## <a name="deleting-data-in-a-database"></a>Suppression de données dans une base de données

Cette section montre comment permettre aux utilisateurs de supprimer un produit à partir de la *produit* table de base de données. L’exemple se compose de deux pages. Dans la première page, les utilisateurs sélectionner un enregistrement à supprimer. L’enregistrement à supprimer est ensuite affiché dans une deuxième page qui leur permet de confirmer qu’ils souhaitent supprimer l’enregistrement.

> [!NOTE] 
> 
> **Important** dans un site Web de production, vous généralement Limitez les personnes autorisées à apporter des modifications aux données. Pour plus d’informations sur la façon de configurer l’appartenance et méthodes permettent d’autoriser l’utilisateur d’effectuer des tâches sur le site, consultez [Ajout de la sécurité et l’appartenance à un Site ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202904).


1. Dans le site Web, créez un nouveau fichier CSHTML nommé *ListProductsForDelete.cshtml*.
2. Remplacez le balisage existant par le code suivant :

    [!code-cshtml[Main](5-working-with-data/samples/sample22.cshtml)]

    Cette page est similaire à la *EditProducts.cshtml* page précédemment. Toutefois, au lieu d’afficher un **modifier** lien pour chaque produit, il affiche un **supprimer** lien. Le **supprimer** lien est créé à l’aide de code incorporé suivant dans le balisage :

    [!code-cshtml[Main](5-working-with-data/samples/sample23.cshtml)]

    Cela crée une URL qui ressemble à ceci lorsque les utilisateurs cliquent sur le lien :

    `http://<server>/DeleteProduct/4`

    L’URL appelle une page nommée *DeleteProduct.cshtml* (que vous créerez ensuite) et transmet l’ID du produit à supprimer (ici, 4).
3. Enregistrez le fichier, mais laissez ouverte.
4. Créez un autre fichier CHTML nommé *DeleteProduct.cshtml*. Remplacez le contenu existant par le suivant :

    [!code-cshtml[Main](5-working-with-data/samples/sample24.cshtml)]

    Cette page est appelée par *ListProductsForDelete.cshtml* et permet aux utilisateurs de confirmer qu’ils souhaitent supprimer un produit. Pour répertorier le produit doit être supprimé, vous obtenez l’ID du produit à supprimer de l’URL en utilisant le code suivant :

    [!code-csharp[Main](5-working-with-data/samples/sample25.cs)]

    La page demande ensuite à l’utilisateur clique sur un bouton pour supprimer l’enregistrement. Il s’agit d’une mesure de sécurité importante : lorsque vous effectuez des opérations sensibles dans votre site Web, comme la mise à jour ou supprimer des données, ces opérations doivent toujours être effectuées à l’aide d’une opération POST, pas une opération GET. Si votre site est configuré de sorte qu’une opération de suppression peut être effectuée à l’aide d’une opération GET, toute personne peut passer une URL telle que `http://<server>/DeleteProduct/4` et ce qu’ils veulent supprimer à partir de votre base de données. En ajoutant de la confirmation et codage de la page afin que la suppression peut être effectuée qu’à l’aide d’un billet, vous ajoutez une mesure de sécurité à votre site.

    L’opération de suppression est effectuée à l’aide du code suivant, qui vérifie tout d’abord qu’il s’agit d’une opération post et que l’ID n’est pas vide :

    [!code-csharp[Main](5-working-with-data/samples/sample26.cs)]

    Le code exécute une instruction SQL qui supprime l’enregistrement spécifié, il redirige l’utilisateur vers la page de liste.
5. Exécutez *ListProductsForDelete.cshtml* dans un navigateur.

    ![[image]](5-working-with-data/_static/image9.jpg)
6. Cliquez sur le **supprimer** lien de l’un des produits. Le *DeleteProduct.cshtml* page s’affiche pour confirmer que vous souhaitez supprimer cet enregistrement.
7. Cliquez sur le **supprimer** bouton. L’enregistrement de produit est supprimé et la page est actualisée avec une liste de produits mis à jour.

> [!TIP]
> 
> <a id="SB_ConnectingToADatabase"></a>
> ### <a name="connecting-to-a-database"></a>Connexion à une base de données
> 
> Vous pouvez vous connecter à une base de données de deux manières. La première consiste à utiliser le `Database.Open` (méthode) et pour spécifier le nom du fichier de base de données (moins les *.sdf* extension) :
> 
> `var db = Database.Open("SmallBakery");`
> 
> Le `Open` méthode suppose que le. *sdf* fichier est en cours du site Web *application\_données* dossier. Ce dossier est conçu spécifiquement pour contenant les données. Par exemple, il dispose des autorisations appropriées pour autoriser le site Web lire et écrire des données, et par mesure de sécurité, WebMatrix ne pas autorise l’accès aux fichiers à partir de ce dossier.
> 
> La seconde consiste à utiliser une chaîne de connexion. Une chaîne de connexion contient des informations sur comment se connecter à une base de données. Cela peut inclure un chemin d’accès de fichier, ou il peut inclure le nom d’une base de données SQL Server sur un serveur local ou distant, ainsi que le nom d’utilisateur et mot de passe pour se connecter à ce serveur. (Si vous conservez les données dans une version gérée de manière centralisée de SQL Server, tels que sur le site du fournisseur d’hébergement, vous toujours utiliser une chaîne de connexion pour spécifier les informations de connexion de base de données.)
> 
> Dans WebMatrix, chaînes de connexion sont généralement stockés dans un fichier XML nommé *Web.config*. Comme son nom l’indique, vous pouvez utiliser un *Web.config* fichier à la racine de votre site Web pour stocker les informations de configuration du site, y compris toutes les chaînes de connexion que votre site peut nécessiter. Un exemple de chaîne de connexion dans un *Web.config* fichier peut se présenter comme suit :
> 
> [!code-xml[Main](5-working-with-data/samples/sample27.xml)]
> 
> Dans l’exemple, la chaîne de connexion pointe vers une base de données dans une instance de SQL Server qui s’exécute sur un serveur quelque part (par opposition à une variable locale *.sdf* fichier). Vous devez substituer les noms appropriés pour `myServer` et `myDatabase`et spécifier des valeurs de connexion SQL Server pour `username` et `password`. (Les valeurs de nom d’utilisateur et mot de passe ne sont pas nécessairement les mêmes en tant que vos informations d’identification Windows ou les valeurs qui vous a être donné par votre fournisseur d’hébergement pour la connexion à leurs serveurs. Contactez l’administrateur pour les valeurs exactes que vous avez besoin.)
> 
> Le `Database.Open` méthode est flexible, car il vous permet de passer le nom d’une base de données *.sdf* fichier ou le nom d’une chaîne de connexion qui est stocké dans le *Web.config* fichier. L’exemple suivant montre comment se connecter à la base de données à l’aide de la chaîne de connexion illustrée dans l’exemple précédent :
> 
> [!code-cshtml[Main](5-working-with-data/samples/sample28.cshtml)]
> 
> Comme indiqué, le `Database.Open` méthode vous permet de passer un nom de base de données ou une chaîne de connexion, et il pourrez déterminer lequel utiliser. Cela est très utile lorsque vous déployez (publier) votre site Web. Vous pouvez utiliser un *.sdf* de fichiers dans le *application\_données* dossier lorsque vous développez et testez votre site. Lorsque vous déplacez votre site vers un serveur de production, vous pouvez ensuite utiliser une chaîne de connexion dans le *Web.config* fichier ayant le même nom que votre *.sdf* fichier mais qui pointe vers le fournisseur d’hébergement de base de données &#8212;tout cela sans avoir à modifier votre code.
> 
> Enfin, si vous souhaitez travailler directement avec une chaîne de connexion, vous pouvez appeler la `Database.OpenConnectionString` méthode et passe il la connexion réelle de chaîne au lieu de simplement le nom d’un objet dans le *Web.config* fichier. Cela peut être utile dans les situations où pour une raison quelconque vous n’avez pas accès à la chaîne de connexion (ou les valeurs qu’il contient, telles que la *.sdf* nom de fichier) jusqu'à ce que la page est en cours d’exécution. Toutefois, la plupart des scénarios, vous pouvez utiliser `Database.Open` comme décrit dans cet article.


## <a name="additional-resources"></a>Ressources supplémentaires

- [SQL Server Compact](https://www.microsoft.com/sqlserver/2008/en/us/compact.aspx)
- [Connexion à un SQL Server ou de la base de données MySQL dans WebMatrix](https://go.microsoft.com/fwlink/?LinkId=208661)
- [Validation des entrées utilisateur dans les sites ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=253002)
