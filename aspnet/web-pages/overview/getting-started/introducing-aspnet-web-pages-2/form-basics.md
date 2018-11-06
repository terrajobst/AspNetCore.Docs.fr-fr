---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: Présentation des Pages Web ASP.NET - principes de base de formulaire HTML | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous montre les principes fondamentaux de la création d’un formulaire d’entrée et comment gérer l’entrée de l’utilisateur lorsque vous utilisez les Pages Web ASP.NET (Razor). Et maintenant que vous avez...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: 28385ac2244ab0bfb38ee5fcbc64e6e11804612b
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021675"
---
<a name="introducing-aspnet-web-pages---html-form-basics"></a>Présentation des Pages Web ASP.NET - principes de base de formulaire HTML
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Ce didacticiel vous montre les principes fondamentaux de la création d’un formulaire d’entrée et comment gérer l’entrée de l’utilisateur lorsque vous utilisez les Pages Web ASP.NET (Razor). Et maintenant que vous avez une base de données, vous allez utiliser vos compétences en matière de formulaire pour permettre aux utilisateurs de rechercher des films spécifiques dans la base de données. Il part du principe que vous avez terminé la série via [Introduction à affichage des données à l’aide de ASP.NET Web Pages](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data).
> 
> Ce que vous allez apprendre :
> 
> - Comment créer un formulaire à l’aide des éléments HTML standards.
> - Guide pratique pour lire l’utilisateur d’entrée dans un formulaire.
> - Comment créer une requête SQL que sélectivement Obtient des données à l’aide d’une recherche de terme que l’utilisateur fournit.
> - Comment faire en sorte de champs dans la page « mémoriser » l’entrée utilisateur.
>   
> 
> Fonctionnalités technologies abordées :
> 
> - Objet `Request`.
> - Le code SQL `Where` clause.


## <a name="what-youll-build"></a>Ce que vous allez générer

Dans le didacticiel précédent, vous créé une base de données, ajouté des données à ce dernier, puis utilisé la `WebGrid` helper pour afficher les données. Dans ce didacticiel, vous allez ajouter une zone de recherche qui vous permet de rechercher les films d’un genre spécifique ou dont le titre contient le mot. (Par exemple, vous serez en mesure de trouver tous les films dont le genre est « Action » ou dont le titre contient « Harry » ou « Adventure ».)

Lorsque vous avez terminé ce didacticiel, vous avez une page similaire à celle-ci :

![Page de films avec recherche de Genre et titre](form-basics/_static/image1.png)

La partie liste de la page est identique à celle du dernier didacticiel &mdash; une grille. La différence est que la grille affichera uniquement les films que vous avez recherchées.

## <a name="about-html-forms"></a>Sur les formulaires HTML

(Si vous avez une expérience avec la création de formulaires HTML et la différence entre `GET` et `POST`, vous pouvez ignorer cette section.)

Un formulaire a des éléments d’entrée utilisateur &mdash; zones de texte, boutons, boutons radio, les cases à cocher, listes déroulantes et ainsi de suite. Les utilisateurs renseignez ces contrôles ou effectuer des sélections et puis envoyez le formulaire en cliquant sur un bouton.

La syntaxe HTML de base d’un formulaire est illustrée dans cet exemple :

[!code-html[Main](form-basics/samples/sample1.html)]

Lorsque ce balisage s’exécute dans une page, il crée un formulaire simple qui ressemble à cette illustration :

![Formulaire HTML de base comme affiché dans le navigateur](form-basics/_static/image2.png)

Le `<form>` élément englobe des éléments HTML à être envoyées. (Une erreur facile qui consiste à effectuer consiste à ajouter des éléments à la page, mais oublient de les placer à l’intérieur d’un `<form>` élément. Dans ce cas, rien n’est envoyé.) Le `method` attribut indique au navigateur comment soumettre l’entrée utilisateur. Vous affectez la valeur `post` si vous effectuez une mise à jour sur le serveur ou à `get` si vous êtes simplement extraire des données à partir du serveur.

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **GET, POST et sécurité du verbe HTTP**
> 
> HTTP, navigateurs et les serveurs utilisent pour échanger des informations, le protocole est remarquablement simple dans ses opérations de base. Navigateurs utilisent uniquement quelques verbes pour effectuer des demandes aux serveurs. Lorsque vous écrivez du code pour le web, il est utile de comprendre ces verbes et la façon dont le navigateur et le serveur les utiliser. Et éloignés les verbes couramment utilisés sont ces :
> 
> - `GET`. Le navigateur utilise ce verbe pour extraire un élément à partir du serveur. Par exemple, lorsque vous tapez une URL dans votre navigateur, le navigateur effectue une `GET` opération visant à demander la page qui vous intéresse. Si la page inclut des graphiques, le navigateur effectue supplémentaires `GET` opérations pour obtenir les images. Si le `GET` opération a passer des informations sur le serveur, les informations sont transmises en tant que partie de l’URL dans la chaîne de requête.
> - `POST`. Le navigateur envoie une `POST` demande afin d’envoyer des données pour être ajoutés ou modifiés sur le serveur. Par exemple, le `POST` verbe est utilisé pour créer des enregistrements dans une base de données ou modifier existants. La plupart du temps, lorsque vous remplissez un formulaire et cliquez sur le bouton Envoyer, le navigateur effectue une `POST` opération. Dans un `POST` opération, les données transmises sur le serveur sont dans le corps de la page.
> 
> Une différence importante entre ces verbes est qu’un `GET` opération n’est pas censé pour modifier quoi que ce soit sur le serveur, ou à le placer dans une méthode légèrement plus abstraite, un `GET` opération n’entraîne pas une modification d’état sur le serveur. Vous pouvez effectuer un `GET` opération sur les mêmes ressources autant de fois que vous voulez, et ne modifiez pas ces ressources. (Un `GET` opération dit souvent « sécurisée », ou pour utiliser un terme technique, est *idempotent*.) En revanche, bien sûr, un `POST` demande change quelque chose sur le serveur chaque fois que vous effectuez l’opération.
> 
> Deux exemples permet d’illustrer cette distinction. Lorsque vous effectuez une recherche à l’aide d’un moteur tel que Bing ou Google, vous remplissez un formulaire qui se compose d’une zone de texte, puis vous cliquez sur le bouton de recherche. Le navigateur effectue une `GET` opération, avec la valeur que vous avez entré dans la zone transmise en tant que partie de l’URL. À l’aide un `GET` opération pour ce type de formulaire est parfait, car une opération de recherche ne change pas toutes les ressources sur le serveur, il extrait uniquement les informations.
> 
> Examinons à présent le processus de classement quelque chose en ligne. Vous remplissez les détails de l’ordre et puis cliquez sur le bouton Envoyer. Cette opération doit être un `POST` demander, étant donné que l’opération entraîne des modifications sur le serveur, comme un nouvel enregistrement de commande, une modification dans les informations de votre compte et peut-être nombre d’autres modifications. Contrairement à la `GET` opération, vous ne pouvez pas répéter votre `POST` demande — si vous l’avez fait, chaque fois que vous soumis de nouveau la demande, vous génère une nouvelle commande sur le serveur. (Dans ce cas, sites Web souvent vous avertira ne pas à cliquez plusieurs fois sur un bouton d’envoi ou désactive le bouton Envoyer afin que vous ne renvoyez pas accidentellement le formulaire.)
> 
> Au cours de ce didacticiel, vous allez utiliser à la fois un `GET` opération et un `POST` opération pour travailler avec des formulaires HTML. Nous expliquerons dans chaque cas pourquoi le verbe que vous utilisez est celui qui convient.
> 
> (Pour en savoir plus sur les verbes HTTP, consultez le [définitions de méthode](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) article sur le site du W3C.)


La plupart des éléments d’entrée utilisateur sont HTML `<input>` éléments. Ils ressemblent `<input type="type" name="name">,` où *type* indique le type de contrôle d’entrée d’utilisateur souhaité. Ces éléments sont les plus courants :

- Zone de texte : `<input type="text">`
- Case à cocher : `<input type="check">`
- Case d’option : `<input type="radio">`
- Bouton : `<input type="button">`
- Bouton d’envoi : `<input type="submit">`

Vous pouvez également utiliser le `<textarea>` élément pour créer une zone de texte multiligne et `<select>` élément pour créer une liste déroulante ou une liste déroulante. (Pour plus d’informations sur HTML forment des éléments, consultez [formulaires HTML et entrée](http://www.w3schools.com/html/html_forms.asp) sur le site W3Schools.)

Le `name` attribut est très important, car le nom est comment vous allez obtenir la valeur de l’élément dans une version ultérieure, comme vous le verrez bientôt.

La partie intéressante est ce que vous, le développeur de pages, faire avec l’entrée d’utilisateur. Il n’existe aucun comportement intégré associé à ces éléments. Au lieu de cela, vous devez obtenir les valeurs que l’utilisateur a entré ou sélectionné et faire quelque chose avec eux. C’est ce que vous allez apprendre dans ce didacticiel.

> [!TIP] 
> 
> **HTML5 et les formulaires de saisie**
> 
> Comme vous le savez peut-être, HTML est en transition et la dernière version (HTML5) inclut la prise en charge de manière plus intuitive pour les utilisateurs à entrer des informations. Par exemple, en HTML5, vous (le développeur de pages) pouvez indiquer la page que vous souhaitez que l’utilisateur à entrer une date. Le navigateur peut afficher automatiquement puis d’un calendrier au lieu d’obliger l’utilisateur à entrer une date manuellement. Toutefois, HTML5 est nouveau et ne prend pas en charge tous les navigateurs encore.
> 
> Les Pages Web ASP.NET prend en charge HTML5 d’entrée dans la mesure où le navigateur de l’utilisateur est. Pour avoir une idée des nouveaux attributs pour le `<input>` élément en HTML5, consultez [HTML &lt;d’entrée&gt; attribut type](http://www.w3schools.com/html/html_form_input_types.asp) sur le site W3Schools.


## <a name="creating-the-form"></a>Création du formulaire

Dans WebMatrix, dans le **fichiers** espace de travail, ouvrez le *Movies.cshtml* page.

Après la fermeture `</h1>` balise et avant l’ouverture `<div>` balise de le `grid.GetHtml` appeler, ajoutez le balisage suivant :

[!code-html[Main](form-basics/samples/sample2.html)]

Ce balisage crée un formulaire qui a une zone de texte nommée `searchGenre` et un bouton Envoyer. Le texte de zone et d’envoyer le bouton sont placés entre un `<form>` élément dont `method` attribut a la valeur `get`. (N’oubliez pas que si vous ne placez la zone de texte et bouton à l’intérieur d’envoyer un `<form>` élément, rien n’est envoyée lorsque vous cliquez sur le bouton.) Vous utilisez le `GET` verbe ici, car vous créez un formulaire qui n’apporte aucune modification sur le serveur, il en résulte simplement une recherche. (Dans le didacticiel précédent, vous avez utilisé un `post` (méthode), qui est la manière dont vous soumettez des modifications sur le serveur. Vous verrez que dans le didacticiel suivant à nouveau.)

Exécutez la page. Bien que vous n’avez pas défini de comportement pour le formulaire, vous pouvez voir à quoi elle ressemble :

![Page de films avec la zone de recherche pour Genre](form-basics/_static/image3.png)

Entrez une valeur dans la zone de texte, tels que « Comédie ». Puis cliquez sur **recherche Genre**.

Prenez note de l’URL de la page. Étant donné que vous définissez le `<form>` l’élément `method` attribut `get`, la valeur que vous avez entré fait désormais partie de la chaîne de requête dans l’URL, comme ceci :

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>Valeurs de formulaire de lecture

La page contient déjà du code qui obtient des données de base de données et affiche les résultats dans une grille. Vous devez maintenant ajouter du code qui lit la valeur de la zone de texte afin de pouvoir exécuter une requête SQL qui inclut le terme de recherche.

Étant donné que vous définissez la méthode du formulaire sur `get`, vous pouvez lire la valeur qui a été entrée dans la zone de texte à l’aide de code comme suit :

`var searchTerm = Request.QueryString["searchGenre"];`

Le `Request.QueryString` objet (la `QueryString` propriété de la `Request` objet) inclut les valeurs des éléments qui ont été soumises dans le cadre de la `GET` opération. Le `Request.QueryString` propriété contient un *collection* (liste) des valeurs qui sont envoyées dans le formulaire. Pour obtenir toute valeur individuelle, vous spécifiez le nom de l’élément que vous souhaitez. C’est pourquoi vous devez avoir un `name` d’attribut sur le `<input>` élément (`searchTerm`) qui crée la zone de texte. (Pour plus d’informations sur la `Request` d’objets, consultez le [encadré](#BKMK_TheRequestObject) ultérieurement.)

Il est suffisamment simple pour lire la valeur de la zone de texte. Toutefois, si l’utilisateur n’entrez rien du tout dans la zone de texte, qu’il a cliqué sur **recherche** tout de même, vous pouvez ignorer ce clic, dans la mesure où il n’y a rien à rechercher.

Le code suivant est un exemple qui montre comment implémenter ces conditions. (Vous n’êtes pas obligé d’ajouter ce code encore ; vous pouvez utiliser dans un instant).

[!code-csharp[Main](form-basics/samples/sample3.cs)]

Le test se décompose ainsi :

- Obtenir la valeur de `Request.QueryString["searchGenre"]`, à savoir la valeur qui a été entrée dans le `<input>` élément nommé `searchGenre`.
- Découvrez si elle est vide à l’aide de la `IsEmpty` (méthode). Cette méthode est la méthode standard pour déterminer si quelque chose (par exemple, un élément de formulaire) contient une valeur. Mais, vous fiche uniquement si elle a *pas* vide, par conséquent...
- Ajouter le `!` opérateur devant le `IsEmpty` de test. (Le `!` opérateur signifie NOT logique).

En clair, l’intégralité de `if` condition se traduit par les éléments suivants : *si l’élément de searchGenre du formulaire n’est pas vide, puis...*

Ce bloc définit l’étape de création d’une requête qui utilise le terme de recherche. Vous pouvez utiliser dans la section suivante.

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **L’objet de requête**
> 
> Le `Request` objet contient toutes les informations que le navigateur envoie à votre application lorsqu’une page est demandée ou soumise. Cet objet inclut toutes les informations fournies par l’utilisateur, telles que les valeurs de zone de texte ou un fichier à charger. Il inclut également toutes sortes d’informations supplémentaires, telles que les cookies, les valeurs dans la chaîne de requête URL (le cas échéant), le chemin d’accès de fichier de la page qui est en cours d’exécution, le type de navigateur à l’aide de l’utilisateur, la liste des langues qui sont définies dans le navigateur et bien plus encore.
> 
> Le `Request` objet est un *collection* (liste) de valeurs. Vous obtenez une valeur individuelle dans la collection en spécifiant son nom :
> 
> `var someValue = Request["name"];`
> 
> Le `Request` objet expose en fait plusieurs sous-ensembles. Exemple :
> 
> - `Request.Form` vous donne les valeurs à partir des éléments à l’intérieur de l’élément soumis `<form>` élément si la demande est un `POST` demande.
> - `Request.QueryString` permet que les valeurs dans la chaîne de requête URL. (Dans une URL telle que `http://mysite/myapp/page?searchGenre=action&page=2`, le `?searchGenre=action&page=2` section de l’URL est la chaîne de requête.)
> - `Request.Cookies` collection vous permet d’accéder aux cookies du navigateur a envoyé.
> 
> Pour obtenir une valeur dont vous savez qu’il est dans le formulaire envoyé, vous pouvez utiliser `Request["name"]`. Vous pouvez également utiliser les versions plus spécifiques `Request.Form["name"]` (pour `POST` demandes) ou `Request.QueryString["name"]` (pour `GET` demandes). Bien sûr, *nom* est le nom de l’élément à obtenir.
> 
> Le nom de l’élément que vous souhaitez obtenir doit être unique au sein de la collection que vous utilisez. C’est pourquoi le `Request` objet fournit des sous-ensembles telles que `Request.Form` et `Request.QueryString`. Supposons que votre page contient un élément de formulaire nommé `userName` et *également* contient un cookie nommé `userName`. Si vous obtenez `Request["userName"]`, si vous souhaitez la valeur de formulaire ou le cookie est ambigu. Toutefois, si vous obtenez `Request.Form["userName"]` ou `Request.Cookie["userName"]`, vous êtes soient explicites sur la valeur à obtenir.
> 
> Il est conseillé d’être précis et utiliser le sous-ensemble de `Request` qui vous intéresse, comme `Request.Form` ou `Request.QueryString`. Pour les pages simples que vous créez dans ce didacticiel, il probablement ne vraiment faire la différence. Toutefois, lorsque vous créez des pages plus complexes, à l’aide de la version explicite `Request.Form` ou `Request.QueryString` peut vous aider à éviter les problèmes qui peuvent survenir lors de la page contient un formulaire (ou plusieurs formulaires), les cookies, les valeurs de chaîne de requête et ainsi de suite.


## <a name="creating-a-query-by-using-a-search-term"></a>Création d’une requête à l’aide d’un terme de recherche

Maintenant que vous savez comment obtenir le terme de recherche entré par l’utilisateur, vous pouvez créer une requête qui l’utilise. N’oubliez pas que pour obtenir tous les éléments film de la base de données, vous utilisez une requête SQL qui ressemble à cette instruction :

`SELECT * FROM Movies`

Pour obtenir uniquement certains films, vous devez utiliser une requête qui inclut un `Where` clause. Cette clause vous permet de définir une condition sur laquelle les lignes sont retournées par la requête. Voici un exemple :

`SELECT * FROM Movies WHERE Genre = 'Action'`

Le format de base est `WHERE column = value`. Vous pouvez utiliser différents opérateurs outre juste `=`, comme `>` (supérieur à), `<` (inférieur à), `<>` (différent de), `<=` (inférieur ou égal à), etc., selon ce que vous cherchez.

Au cas où vous vous poseriez la question, les instructions SQL ne sont pas sensible à la casse &mdash; `SELECT` est identique à `Select` (voire `select`). Toutefois, personnes souvent Tirez profit des mots clés dans une instruction SQL, comme `SELECT` et `WHERE`, pour le rendre plus facile à lire.

### <a name="passing-the-search-term-as-a-parameter"></a>En passant le terme de recherche en tant que paramètre

Si vous recherchez un genre spécifique est assez facile (`WHERE Genre = 'Action'`), mais que vous souhaitez être en mesure de rechercher n’importe quel genre de l’utilisateur entre. Pour ce faire, vous créez en tant que requête SQL qui inclut un espace réservé pour la valeur à rechercher. Il ressemblera à cette commande :

`SELECT * FROM Movies WHERE Genre = @0`

L’espace réservé est le `@` caractères suivis de zéro. Comme vous pouvez le deviner, une requête peut contenir des espaces réservés, et ils se nommerait `@0`, `@1`, `@2`, etc.

Pour définir la requête et transmet en réalité la valeur, vous utilisez le code comme suit :

[!code-sql[Main](form-basics/samples/sample4.sql)]

Ce code est similaire à ce que vous avez déjà fait pour afficher des données dans la grille. Les seules différences sont :

- La requête contient un espace réservé (`WHERE Genre = @0"`).
- La requête est placée dans une variable (`selectCommand`) ; avant, vous avez passé la requête directement à la `db.Query` (méthode).
- Lorsque vous appelez le `db.Query` (méthode), vous passez la requête et la valeur à utiliser pour l’espace réservé. (Si la requête a dû à plusieurs espaces réservés, vous pouvez transmettre les tout en tant que valeurs distincts à la méthode.)

Si vous placez tous ces éléments ensemble, vous obtenez le code suivant :

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **Important !** À l’aide des espaces réservés (tels que `@0`) pour transmettre des valeurs à une commande SQL est *extrêmement important* pour la sécurité. La même façon qu’ici, avec des espaces réservés pour les données de variable, est la seule façon, vous devez construire des commandes SQL.
> 
> Jamais construire une instruction SQL en rassemblant des texte littéral (concaténer) et les valeurs que vous obtenez à partir de l’utilisateur. Concaténation de l’entrée utilisateur dans une instruction SQL s’ouvre votre site vers un *attaque par injection SQL* où un utilisateur malveillant soumet les valeurs à votre page de pirater votre base de données. (Vous trouverez plus d’informations dans l’article [Injection SQL](https://msdn.microsoft.com/library/ms161953.aspx) le site Web MSDN.)


## <a name="updating-the-movies-page-with-search-code"></a>Mise à jour de la Page de films avec rechercher du Code

Maintenant vous pouvez mettre à jour le code dans le *Movies.cshtml* fichier. Pour commencer, remplacez le code dans le bloc de code en haut de la page avec ce code :

[!code-csharp[Main](form-basics/samples/sample6.cs)]

La différence ici est que vous avez créé la requête dans le `selectCommand` variable, ce qui vous passerez à `db.Query` plus tard. Placement de l’instruction SQL dans une variable vous permet de modifier l’instruction, ce qui est ce que vous allez faire pour effectuer la recherche.

Vous avez également supprimé ces deux lignes, vous devez remettre dans ultérieurement :

[!code-csharp[Main](form-basics/samples/sample7.cs)]

Vous ne souhaitez pas encore exécuté la requête (autrement dit, appelez `db.Query`) et vous ne souhaitez pas initialiser le `WebGrid` helper encore soit. Vous pouvez utiliser ces éléments une fois que vous avez compris comment fonctionne l’instruction SQL doit s’exécuter.

Après ce bloc réécrit, vous pouvez ajouter la nouvelle logique pour la gestion de la recherche. Le code complet se présente comme suit. Mettre à jour le code de votre page afin qu’il corresponde à cet exemple :

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

La page fonctionne désormais comme suit. Chaque fois que la page s’exécute, le code s’ouvre à la base de données et la `selectCommand` variable est définie à l’instruction SQL qui obtient tous les enregistrements à partir de la `Movies` table. Le code initialise également le `searchTerm` variable.

Toutefois, si la requête actuelle inclut une valeur pour le `searchGenre` élément, le code définit `selectCommand` à une autre requête, à savoir, pour qu’il inclut le `Where` clause pour rechercher un genre. Il définit également `searchTerm` à tout ce qui a été transmis pour la zone de recherche (qui peut être nothing).

Quel que soit l’élément SQL instruction se trouve dans `selectCommand`, le code appelle ensuite `db.Query` pour exécuter la requête, en lui passant l’est dans `searchTerm`. Si aucun élément ne `searchTerm`, peu importe, car dans ce cas il n’existe aucun paramètre à passer la valeur à `selectCommand` quand même.

Enfin, le code initialise le `WebGrid` application d’assistance à l’aide des résultats de la requête, comme avant.

Vous pouvez voir qu’en plaçant l’instruction SQL et le terme de recherche dans des variables, vous avez ajouté de la flexibilité au code. Comme vous le verrez plus loin dans ce didacticiel, vous pouvez utiliser cette infrastructure de base et continuez d’ajouter une logique pour différents types de recherche.

## <a name="testing-the-search-by-genre-feature"></a>La fonctionnalité de recherche par Genre de test

Dans WebMatrix, exécutez le *Movies.cshtml* page. Vous voyez la page avec la zone de texte pour le genre.

Entrez un genre que vous avez entré pour l’une de vos enregistrements de test, puis cliquez sur **recherche**. Cette fois-ci vous obtenir la liste de simplement les films qui correspondent à ce genre s’affiche :

![Page de films répertoriant après une recherche de genre 'Comedies'](form-basics/_static/image4.png)

Entrez un autre genre et relancez la recherche. Essayez d’entrer le genre à l’aide de toutes les lettres minuscules ou majuscules afin que vous puissiez voir que la recherche ne respecte pas la casse.

## <a name="remembering-what-the-user-entered"></a>« Mémoriser » l’entrée utilisateur

Vous avez peut-être remarqué qu’une fois que vous avez entré un genre et que vous avez cliqué sur **recherche Genre**, vous avez vu une liste pour ce genre s’affiche. Toutefois, la zone de texte de recherche était vide &mdash; en d’autres termes, il n’a pas n’oubliez pas ce que vous aviez entré.

Il est important de comprendre pourquoi ce comportement se produit. Lorsque vous envoyez une page, le navigateur envoie une demande au serveur web. Lorsqu’ASP.NET reçoit la demande, il crée une toute nouvelle instance de la page, exécute le code qu’il contient, puis affiche de nouveau la page au navigateur. En effet, cependant, la page ne sait pas que vous travailliez avec une version précédente de lui-même. Les qu'il sait qu’il est obtenu d’une demande ayant certaines données de formulaire qu’il contient.

Chaque fois que vous demandez une page &mdash; s’il faut pour la première fois ou en le soumettant &mdash; vous obtenez une nouvelle page. Le serveur web n’a aucun mémoire de votre dernière demande. Pas plus qu’ASP.NET, et pas plus que le navigateur. La seule connexion entre ces instances distinctes de la page est toutes les données que vous transmettez entre eux. Par exemple, si vous envoyez une page, la nouvelle instance de page permettre obtenir les données de formulaire qui a été envoyées par l’instance antérieure. (Un autre pour passer des données entre les pages consiste à utiliser des cookies.)

Une méthode formelle pour décrire cette situation est à dire que les pages web sont *sans état*. Serveurs Web et les pages elles-mêmes et les éléments dans la page ne conservent pas toutes les informations concernant l’état précédent d’une page. Le web a été conçu de cette façon, car la gestion de l’état des requêtes individuelles est rapidement épuiser les ressources de serveurs web, ce qui est souvent gérer des milliers, peut-être même des centaines de milliers de demandes par seconde.

C’est pourquoi la zone de texte était vide. Une fois que vous avez envoyé la page, ASP.NET créé une nouvelle instance de la page et s’est exécutée dans le code et le balisage. Lors de rien que du code qui a dit ASP.NET pour placer une valeur dans la zone de texte. Par conséquent, ASP.NET n’ai rien fait, et la zone de texte a été restituée sans une valeur qu’il contient.

Il est en fait un moyen simple de contourner ce problème. Le genre que vous avez entré dans la zone de texte *est* à votre disposition dans le code &mdash; il se trouve dans `Request.QueryString["searchGenre"]`.

Mettre à jour le balisage de la zone de texte afin que le `value` attribut obtient sa valeur de `searchTerm`, comme dans cet exemple :

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

Dans cette page, vous pouvez aussi définir également la `value` attribut le `searchTerm` variable, étant donné que cette variable contient également le genre que vous avez entré. Mais l’utilisation du `Request` objet à définir le `value` attribut comme indiqué ici est la méthode standard pour accomplir cette tâche. (En supposant que vous souhaitez encore cela &mdash; dans certaines situations, vous souhaiterez restituer la page *sans* valeurs dans les champs. Tout dépend de ce qui se passait avec votre application.)

> [!NOTE]
> Vous ne pouvez pas de « mémoriser » la valeur d’une zone de texte qui est utilisée pour les mots de passe. Il serait une faille de sécurité pour permettre aux utilisateurs de remplir un champ de mot de passe à l’aide de code.


Réexécutez la page, entrez un genre, puis cliquez sur **recherche Genre**. Cette fois mais non seulement voir les résultats de la recherche, mais souvient de la zone de texte que vous avez entré la dernière fois :

![Page montrant que la zone de texte a « mémorisé » l’entrée précédente](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>Recherche un mot dans le titre

Vous pouvez maintenant rechercher n’importe quel genre, mais vous souhaiterez également rechercher un titre. Il est difficile d’obtenir un titre brancher correctement lorsque vous recherchez, par conséquent, au lieu de cela que vous pouvez rechercher un mot qui apparaît n’importe où à l’intérieur d’un titre. Pour ce faire dans SQL, vous utilisez le `LIKE` opérateur et la syntaxe comme suit :

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

Cette commande Obtient tous les films dont le titre contient « adventure ». Lorsque vous utilisez le `LIKE` opérateur, vous incluez le caractère générique `%` en tant que partie du terme recherché. La recherche `LIKE 'adventure%'` signifie « commençant par « adventure » ». (Techniquement, cela signifie que « La chaîne « adventure », suivi par aucun élément ».) De même, le terme de recherche `LIKE '%adventure'` signifie « quoi que ce soit suivi de la chaîne « adventure » », qui est une autre façon de dire « terminant par « adventure » ».

Le terme de recherche `LIKE '%adventure%'` signifie par conséquent « avec « adventure » n’importe où dans le titre. » (Techniquement, « quoi que ce soit dans le titre, suivi de « adventure », suivie de quoi que ce soit. »)

À l’intérieur de la `<form>` élément, ajoutez le balisage suivant juste en dessous la fermeture `</div>` balise pour la recherche de genre (juste avant la fermeture `</form>` élément) :

[!code-html[Main](form-basics/samples/sample10.html)]

Le code pour gérer cette recherche est similaire au code de la recherche de genre, à ceci près que vous ayez à assembler le `LIKE` recherche. Dans le bloc de code en haut de la page, ajoutez ceci `if` bloquer juste après le `if` bloc pour la recherche du genre :

[!code-csharp[Main](form-basics/samples/sample11.cs)]

Ce code utilise la même logique que vous avez vu précédemment, à ceci près que la recherche utilise un `LIKE` opérateur et la place de code «`%`» avant et après le terme de recherche.

Remarquez comment il était facile d’ajouter une autre recherche à la page. Il vous suffisait à faire était :

- Créer un `if` bloc testé pour voir si la zone de recherche pertinentes avait une valeur.
- Définir le `selectCommand` variable à une nouvelle instruction SQL.
- Définir le `searchTerm` variable à la valeur à passer à la requête.

Voici le bloc de code complet, qui contient la nouvelle logique d’une recherche de titre :

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

Voici un résumé de ce que fait ce code :

- Les variables `searchTerm` et `selectCommand` sont initialisés en haut. Vous allez définir ces variables au terme de recherche approprié (le cas échéant) et la commande SQL appropriée selon ce que fait l’utilisateur dans la page. La recherche par défaut est le cas simple d’obtenir tous les films à partir de la base de données.
- Dans les tests pour `searchGenre` et `searchTitle`, le code définit `searchTerm` à la valeur que vous souhaitez rechercher. Ces blocs de code est également défini `selectCommand` à une commande SQL appropriée pour cette recherche.
- Le `db.Query` méthode est appelée une seule fois, à l’aide de toute autre commande SQL dans `selectedCommand` et quelle que soit la valeur se trouve dans `searchTerm`. S’il n’existe aucun terme de recherche (aucun genre et aucun mot de titre), la valeur de `searchTerm` est une chaîne vide. Toutefois, qui importe peu, car dans ce cas la requête ne nécessite pas un paramètre.

## <a name="testing-the-title-search-feature"></a>Test de la fonctionnalité de recherche de titre

Vous pouvez maintenant tester votre page de recherche terminée. Exécutez *Movies.cshtml*.

Entrez un genre et cliquez sur **recherche Genre**. La grille affiche les films de ce genre s’affiche, comme avant.

Entrez un mot de titre et cliquez sur **titre de la recherche**. La grille affiche les films qui ont ce mot dans le titre.

![Page de films répertoriant après une recherche pour « Le » dans le titre](form-basics/_static/image6.png)

Ne renseignez pas les deux zones de texte et cliquez sur l’un des boutons. La grille affiche tous les films.

## <a name="combining-the-queries"></a>Combinant les requêtes

Vous pouvez remarquer que les recherches, que vous pouvez effectuer sont exclusifs. Vous ne pouvez pas rechercher le titre et le genre en même temps, même si les deux zones de recherche contiennent des valeurs. Par exemple, vous ne pouvez pas rechercher pour tous les films d’action dont le titre contient « Adventure ». (La page est codé à présent, si vous entrez des valeurs pour le genre et le titre, la recherche de titre obtient priorité). Pour créer une recherche qui combine les conditions, vous devrez créer une requête SQL qui a une syntaxe similaire à celle-ci :

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

Et vous seriez obligé d’exécuter la requête en utilisant une instruction comme suit (les grandes lignes) :

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

Création d’une logique permettant de nombreuses permutations de critères de recherche peut obtenir un peu impliqué, comme vous pouvez le voir. Par conséquent, nous arrêtons ici.

## <a name="coming-up-next"></a>Prochaine

Dans le didacticiel suivant, vous allez créer une page qui utilise un formulaire pour permettre aux utilisateurs d’ajouter des films à la base de données.

## <a name="complete-listing-for-movie-page-updated-with-search"></a>Intégralité de la Page de film (mis à jour avec recherche)

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>Ressources supplémentaires

- [Introduction à la programmation Web ASP.NET à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Une Clause SQL WHERE](http://www.w3schools.com/sql/sql_where.asp) sur le site W3Schools
- [Définitions de méthode](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) article sur le site du W3C

> [!div class="step-by-step"]
> [Précédent](displaying-data.md)
> [Suivant](entering-data.md)
