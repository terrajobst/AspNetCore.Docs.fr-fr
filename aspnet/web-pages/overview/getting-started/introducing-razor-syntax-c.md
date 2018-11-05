---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: Introduction à la programmation Web ASP.NET à l’aide de la syntaxe Razor (c#) | Microsoft Docs
author: tfitzmac
description: Ce chapitre vous donne une vue d’ensemble de la programmation avec les Pages Web ASP.NET à l’aide de la syntaxe Razor. ASP.NET est la technologie de Microsoft pour les pa web dynamique en cours d’exécution...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: b242bf52bbd63d726e6ce6ab7be01a1b81c5bf1b
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758256"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a>Introduction à la programmation Web ASP.NET à l’aide de la syntaxe Razor (c#)
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article vous donne une vue d’ensemble de la programmation avec les Pages Web ASP.NET à l’aide de la syntaxe Razor. ASP.NET est la technologie de Microsoft pour les pages web dynamiques en cours d’exécution sur les serveurs web. Cette articles est consacrée à l’aide du langage de programmation c#.
> 
> **Ce que vous allez apprendre**:
> 
> - Le 8 principaux conseils de mise en route avec ASP.NET Web Pages à l’aide de la syntaxe Razor de la programmation de programmation.
> - Concepts de programmation de base que vous avez besoin.
> - Le code de serveur ASP.NET et la syntaxe Razor concerne tous.
>   
> 
> ## <a name="software-versions"></a>Versions des logiciels
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Ce didacticiel fonctionne également avec ASP.NET Web Pages 2.


## <a name="the-top-8-programming-tips"></a>Les meilleurs conseils de programmation 8

Cette section répertorie quelques conseils que vous devez absolument connaître lorsque vous commencez à écrire le code de serveur ASP.NET à l’aide de la syntaxe Razor.

> [!NOTE]
> La syntaxe Razor est basée sur le langage de programmation c#, et qui est la langue est utilisée plus souvent avec les Pages Web ASP.NET. Toutefois, la syntaxe Razor prend également en charge le langage Visual Basic et tout ce que vous voyez que vous pouvez également faire dans Visual Basic. Pour plus d’informations, consultez l’annexe [langage Visual Basic et la syntaxe](https://go.microsoft.com/fwlink/?LinkId=202908).


Vous trouverez plus d’informations sur la plupart de ces techniques de programmation plus loin dans l’article.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Ajouter du code à une page à l’aide du caractère « @ »

Le `@` caractère démarre les expressions inline, les blocs d’instruction unique et les blocs d’instructions multiples :

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

Il s’agit d’aspect de ces instructions lorsque la page s’exécute dans un navigateur :

![Razor-Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> **Codage HTML**
> 
> Lorsque vous affichez le contenu dans une page à l’aide de la `@` de caractères, comme dans les exemples précédents, ASP.NET encode au format HTML la sortie. Cela remplace les caractères HTML réservés (tels que `<` et `>` et `&`) avec des codes qui permettent les caractères à afficher en tant que caractères dans une page web au lieu d’être interprétée comme des balises HTML ou des entités. Sans codage HTML, la sortie à partir de votre code de serveur peuvent ne pas s’afficher correctement et peut exposer une page à des risques de sécurité.
> 
> Si votre objectif est de sortie de balisage HTML qui rend les balises en tant que balisage (par exemple `<p></p>` pour un paragraphe ou `<em></em>` pour mettre en évidence de texte), consultez la section [combinant le texte, le balisage et Code dans les blocs de Code](#BM_CombiningTextMarkupAndCode) plus loin dans cet article.
> 
> Vous trouverez plus d’informations sur le codage HTML dans [utilisation des formulaires](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-in-braces"></a>2. Vous placez les blocs de code entre accolades

Un *bloc de code* inclut une ou plusieurs instructions de code et est placé entre accolades.

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

Le résultat est affiché dans un navigateur :

![Razor-Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a>3. À l’intérieur d’un bloc, vous obtenez chaque instruction du code par un point-virgule

À l’intérieur d’un bloc de code, chaque instruction de code complet doit se terminer par un point-virgule. Expressions inline ne se terminent par un point-virgule.

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a>4. Vous utilisez des variables pour stocker des valeurs

Vous pouvez stocker des valeurs dans un *variable*, y compris les chaînes, des chiffres et des dates, etc. Vous créez une variable en utilisant le `var` mot clé. Vous pouvez insérer des valeurs des variables directement dans une page à l’aide `@`.

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

Le résultat est affiché dans un navigateur :

![Razor-Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Vous placez les valeurs de chaîne littérale entre guillemets doubles

Un *chaîne* est une séquence de caractères qui sont traitées en tant que texte. Pour spécifier une chaîne, mettez-le entre guillemets doubles :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

Si la chaîne que vous souhaitez afficher contient un caractère de barre oblique inverse ( `\` ) ou des guillemets doubles ( `"` ), utilisez un *littéral de chaîne textuelle* qui est préfixé avec le `@` opérateur. (En c#, le \ caractère a une signification spéciale, sauf si vous utilisez un littéral de chaîne textuelle.)

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

Pour incorporer des guillemets doubles, utiliser un littéral de chaîne textuelle et répétez les guillemets :

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

Voici le résultat de l’utilisation de ces deux exemples dans une page :

![Razor-Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> Notez que le `@` caractère est utilisé pour marquer des littéraux de chaîne textuelle dans c# et à marquer du code dans les pages ASP.NET.


### <a name="6-code-is-case-sensitive"></a>6. Code respecte la casse

En c#, mots clés (telles que `var`, `true`, et `if`) et les noms de variables respectent la casse. Les lignes suivantes de code créent deux variables différentes, `lastName` et `LastName.`

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

Si vous déclarez une variable en tant que `var lastName = "Smith";` et si vous tentez de référencer cette variable dans votre page en tant que `@LastName`, une erreur se produit, car `LastName` ne sera pas reconnue.

> [!NOTE]
> En Visual Basic, mots clés et les variables sont *pas* respecte la casse.


### <a name="7-much-of-your-coding-involves-objects"></a>7. Une grande partie de votre codage implique des objets

Un *objet* représente une chose que vous pouvez programmer avec &#8212; une page, une zone de texte, un fichier, une image, une requête web, un message électronique, un enregistrement de client (ligne de base de données), etc. Objets ont des propriétés qui décrivent leurs caractéristiques, et que vous pouvez lire ou modifier &#8212; un objet de zone de texte a un `Text` propriété (entre autres), un objet de requête a un `Url` propriété, un message électronique a un `From` propriété, et un objet customer possède une `FirstName` propriété. Objets possèdent également des méthodes qui sont le &quot;verbes&quot; qu’ils peuvent effectuer. Exemples incluent un objet fichier `Save` (méthode), un objet image `Rotate` (méthode) et un objet de messagerie `Send` (méthode).

Vous utiliserez souvent le `Request` de l’objet, ce qui vous donne des informations telles que les valeurs des zones de texte (champs de formulaire) dans la page, le type de navigateur fait la demande, l’URL de la page, de l’identité de l’utilisateur, etc. L’exemple suivant montre comment accéder aux propriétés de la `Request` objet et comment appeler le `MapPath` méthode de la `Request` objet, ce qui vous donne le chemin d’accès absolu de la page sur le serveur :

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

Le résultat est affiché dans un navigateur :

![Razor-Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. Vous pouvez écrire du code qui prend des décisions

Une fonctionnalité clé des pages web dynamiques est que vous pouvez déterminer ce qu’il faut faire en fonction des conditions. L’approche la plus courante pour ce faire est avec la `if` instruction (et éventuellement `else` instruction).

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

L’instruction `if(IsPost)` est un moyen rapide de la rédaction de `if(IsPost == true)`. Avec `if` instructions, il existe plusieurs façons pour tester les conditions, la répétition des blocs de code, et ainsi de suite, qui sont décrits plus loin dans cet article.

Le résultat affiché dans un navigateur (après avoir cliqué sur **envoyer**) :

![Razor-Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a>HTTP GET et POST méthodes et la propriété IsPost
> 
> Le protocole utilisé pour les pages web (HTTP) prend en charge un nombre très limité de méthodes (verbes) qui sont utilisés pour effectuer des demandes au serveur. Les deux plus courantes sont GET, qui est utilisé pour lire une page, et POST, ce qui est utilisé pour envoyer une page. En règle générale, la première fois qu’un utilisateur demande une page, la page est demandée à l’aide de GET. Si l’utilisateur remplit un formulaire, puis clique sur un bouton d’envoi, le navigateur envoie une demande POST vers le serveur.
> 
> Dans la programmation web, il est souvent utile de savoir si une page est demandée sous la forme d’une opération GET ou un billet afin que vous sachiez comment traiter la page. Dans ASP.NET Web Pages, vous pouvez utiliser le `IsPost` propriété pour déterminer si une requête est une opération GET ou POST. Si la demande est une publication, le `IsPost` propriété retournera la valeur true, et vous pouvez effectuer les opérations en lecture les valeurs des zones de texte sur un formulaire. Vous verrez de nombreux exemples vous montrent comment traiter la page différemment selon la valeur de `IsPost`.


## <a name="a-simple-code-example"></a>Un exemple de Code Simple

Cette procédure vous montre comment créer une page qui illustre des techniques de programmation de base. Dans l’exemple, vous créez une page qui permet aux utilisateurs d’entrer des deux nombres, puis il les ajoute et affiche le résultat.

1. Dans votre éditeur, créez un nouveau fichier et nommez-le *AddNumbers.cshtml*.
2. Copiez le code et le balisage suivant dans la page, en remplaçant tout élément déjà dans la page.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    Voici quelques éléments de noter :

    - Le `@` caractère démarre le premier bloc de code dans la page, et il précède le `totalMessage` variable qui est incorporé au bas de la page.
    - Le bloc en haut de la page est placé entre accolades.
    - Dans le bloc en haut, toutes les lignes se terminant par un point-virgule.
    - Les variables `total`, `num1`, `num2`, et `totalMessage` stocker plusieurs nombres et une chaîne.
    - La valeur de littéral de chaîne assignée à la `totalMessage` variable est entre guillemets doubles.
    - Étant donné que le code respecte la casse, lorsque le `totalMessage` variable est utilisée au bas de la page, son nom doit correspondre exactement à la variable en haut.
    - L’expression `num1.AsInt() + num2.AsInt()` montre comment utiliser des méthodes et des objets. Le `AsInt` méthode sur chaque variable convertit la chaîne entrée par un utilisateur à un nombre (entier) afin que vous pouvez effectuer des opérations arithmétiques sur celui-ci.
    - Le `<form>` balise inclut un `method="post"` attribut. Spécifie que lorsque l’utilisateur clique sur **ajouter**, la page sera être envoyée au serveur à l’aide de la méthode HTTP POST. Lorsque la page est envoyée, le `if(IsPost)` test a la valeur true et l’instruction conditionnelle de code s’exécute, affiche le résultat de l’ajout de nombres.
3. Enregistrez la page et l’exécuter dans un navigateur. (Assurez-vous que la page est sélectionnée dans le **fichiers** espace de travail avant de l’exécuter.) Entrez deux nombres entiers, puis activez la **ajouter** bouton. 

    ![Razor-Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a>Concepts de programmation de base

Cet article vous offre une vue d’ensemble de la programmation web ASP.NET. Il n’est pas un examen exhaustif, simplement une rapide visite guidée les concepts de programmation que vous utiliserez le plus souvent. Même dans ce cas, il couvre presque tout ce dont vous aurez besoin pour bien démarrer avec ASP.NET Web Pages.

Mais tout d’abord, un peu bagage technique.

### <a name="the-razor-syntax-server-code-and-aspnet"></a>La syntaxe Razor, le Code serveur et ASP.NET

Syntaxe Razor est une syntaxe de programmation simple pour l’incorporation de code basé sur le serveur dans une page web. Dans une page web qui utilise la syntaxe Razor, il existe deux types de contenu : code de contenu et du serveur du client. Contenu du client est les choses que vous êtes habitué dans les pages web : le balisage HTML (éléments), des informations de style tels que CSS, peut-être script côté client telles que JavaScript et texte brut.

Syntaxe Razor vous permet d’ajouter le code de serveur à ce contenu client. S’il existe le code serveur dans la page, le serveur exécute tout d’abord, ce code avant d’envoyer la page au navigateur. S’exécute sur le serveur, le code peut effectuer des tâches qui peuvent être beaucoup plus difficile de le faire à l’aide du client à du contenu uniquement, comme l’accès à des bases de données basées sur le serveur. Plus important encore, le code serveur peut créer dynamiquement du contenu de client &#8212; il peut générer le balisage HTML ou autres contenus à la volée et puis l’envoyer au navigateur, ainsi que toutes les données HTML statique que la page peut contenir. Du point de vue du navigateur, client à du contenu généré par le code de votre serveur n’est pas différent de celui de tout autre contenu client. Comme vous l’avez déjà vu, le code du serveur qui a requis est assez simple.

Les pages web ASP.NET qui incluent la syntaxe Razor ont une extension de fichier spécial (*.cshtml* ou *.vbhtml*). Le serveur reconnaît ces extensions, exécute le code qui est marqué avec la syntaxe Razor, puis envoie la page au navigateur.

### <a name="where-does-aspnet-fit-in"></a>Quand ASP.NET convient-elle ?

Syntaxe Razor est basée sur une technologie de Microsoft appelé ASP.NET, qui à son tour est basé sur Microsoft .NET Framework. Le.NET Framework est une infrastructure de programmation big et complet de Microsoft pour le développement de pratiquement n’importe quel type d’application de l’ordinateur. ASP.NET est la partie du .NET Framework qui est spécialement conçu pour la création d’applications web. Les développeurs ont utilisé ASP.NET pour créer la majorité des sites Web plus grands et le plus élevé de trafic dans le monde. (Chaque fois que vous voyez l’extension de nom de fichier *.aspx* dans le cadre de l’URL dans un site, vous saurez que le site a été écrit à l’aide d’ASP.NET.)

La syntaxe Razor offre toute la puissance d’ASP.NET, mais en utilisant une syntaxe simplifiée est plus facile de savoir si vous êtes débutant et qui vous rend plus productif si vous êtes un expert. Bien que cette syntaxe est simple à utiliser, sa relation famille avec ASP.NET et .NET Framework signifie que comme vos sites Web est sophistiquées, plus vous disposez de la puissance des infrastructures de plus grande à votre disposition.

![Razor-Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> **Classes et Instances**
> 
> Code de serveur ASP.NET utilise des objets, qui à son tour reposent sur l’idée des classes. La classe est la définition ou un modèle pour un objet. Par exemple, une application peut contenir un `Customer` classe qui définit les propriétés et méthodes n’importe quel objet de client a besoin.
> 
> Lorsque l’application doit fonctionner avec les informations client réel, il crée une instance de (ou *instancie*) un objet customer. Chaque client est une instance distincte de la `Customer` classe. Chaque instance prend en charge les mêmes propriétés et méthodes, mais les valeurs de propriété pour chaque instance sont généralement différentes, car chaque objet customer est unique. Dans l’objet d’un client, le `LastName` propriété peut être « Smith » ; dans un autre objet client, le `LastName` propriété peut être « Jones ».
> 
> De même, n’importe quelle page web individuelles dans votre site est un `Page` objet qui est une instance de la `Page` classe. Un bouton sur la page est un `Button` objet qui est une instance de la `Button` classe et ainsi de suite. Chaque instance possède ses propres caractéristiques, mais elles sont basées sur ce qui est spécifié dans la définition de classe de l’objet.


## <a name="basic-syntax"></a>Syntaxe de base

Vous avez vu précédemment un exemple de base de la création d’une page ASP.NET Web Pages, et comment vous pouvez ajouter du code de serveur à la balise HTML. Ici, vous allez apprendre les principes fondamentaux de l’écriture de code de serveur ASP.NET à l’aide de la syntaxe Razor &#8212; , autrement dit, les règles langage de programmation.

Si vous êtes familiarisé avec la programmation (surtout si vous avez utilisé le C, C++, c#, Visual Basic ou JavaScript), une grande partie de ce que vous lire ici sera familier. Vous devrez probablement vous familiariser uniquement avec le code de serveur est ajouté à balisage dans *.cshtml* fichiers.

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a>Combinaison de texte, balisage et Code dans les blocs de Code

Dans les blocs de code serveur, vous souhaitez souvent sortie texte ou balisage (ou les deux) à la page. Si un bloc de code de serveur contient du texte non code, mais qui au lieu de cela doit être restitué en l’état, ASP.NET doit être en mesure de distinguer ce texte à partir du code. Pour ce faire, plusieurs méthodes sont possibles.

- Placez le texte dans un élément HTML comme `<p></p>` ou `<em></em>`:   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    L’élément HTML peut inclure de texte, des éléments HTML supplémentaires et des expressions de code serveur. Lorsque ASP.NET voit la balise HTML d’ouverture (par exemple, `<p>`), elle affiche tous les éléments, y compris l’élément et son contenu sous la forme est dans le navigateur, résolution des expressions de code serveur tandis qu’elle passe.
- Utilisez le `@:` opérateur ou la `<text>` élément. Le `@:` génère une ligne unique de contenu contenant du texte brut ou des balises HTML sans correspondance ; le `<text>` élément englobe plusieurs lignes de sortie. Ces options sont utiles lorsque vous ne souhaitez pas restituer un élément HTML dans le cadre de la sortie.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    Si vous souhaitez plusieurs lignes de texte ou des balises HTML sans correspondance de sortie, vous pouvez le faire précéder chaque ligne avec `@:`, ou vous pouvez placer la ligne dans un `<text>` élément. Comme le `@:` opérateur,`<text>` balises sont utilisées par ASP.NET pour identifier le contenu de texte et ne sont jamais restitués dans la sortie de page.

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    Le premier exemple répète l’exemple précédent, mais utilise une seule paire de `<text>` balises pour encadrer le texte à afficher. Dans le deuxième exemple, le `<text>` et `</text>` balises placer trois lignes, disposant de certaines relation contenant-contenu de texte et de des balises HTML sans correspondance (`<br />`), ainsi que le code serveur et de mise en correspondance des balises HTML. Là encore, peut également faire précéder chaque ligne individuellement avec la `@:` opérateur ; les deux fonctionnent de façon.

    > [!NOTE]
    > Sortie de texte comme indiqué dans cette section &#8212; à l’aide d’un élément HTML, le `@:` opérateur, ou la `<text>` élément &#8212; ASP.NET n’encoder en HTML la sortie. (Comme indiqué précédemment, ASP.NET n’encode pas la sortie des expressions de code serveur et les blocs de code de serveur qui sont précédés `@`, sauf dans les cas spéciales indiquées dans cette section.)

### <a name="whitespace"></a>Whitespace

L’instruction n’affectent pas les espaces supplémentaires dans une instruction (et en dehors d’un littéral de chaîne) :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

Un saut de ligne dans une instruction n’a aucun effet sur l’instruction, et vous pouvez encapsuler des instructions pour une meilleure lisibilité. Les instructions suivantes sont les mêmes :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

Toutefois, vous ne pouvez pas encapsuler une ligne au milieu d’un littéral de chaîne. L’exemple suivant ne fonctionne pas :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

Pour combiner une longue chaîne répartie sur plusieurs lignes, comme le code ci-dessus, il existe deux options. Vous pouvez utiliser l’opérateur de concaténation (`+`), comme vous le verrez plus loin dans cet article. Vous pouvez également utiliser le `@` caractère pour créer une chaîne textuelle literal, comme vous l’avez vu précédemment dans cet article. Vous pouvez rompre les littéraux de chaîne textuelle sur plusieurs lignes :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a>Code (et le balisage) commentaires

Commentaires vous permettent de rédiger des notes pour vous-même ou d’autres personnes. Ils vous permettent également de désactiver (*commentez*) une section de code ou balisage que vous ne souhaitez pas exécuter mais que vous souhaitez conserver dans votre page pour l’instant.

Il est différent de commentaires de syntaxe pour le code Razor et pour le balisage HTML. Comme avec tout le code Razor, Razor commentaires sont traités (et puis supprimés) sur le serveur avant que la page est envoyée au navigateur. Par conséquent, la syntaxe de commentaire Razor vous permet de placer des commentaires dans le code (ou même dans le balisage), vous pouvez voir quand vous modifiez le fichier, mais que les utilisateurs ne voient, même dans la page source.

Pour les commentaires ASP.NET Razor, vous commencez le commentaire avec `@*` et terminez-le avec `*@`. Ce commentaire peut être sur une seule ligne ou plusieurs lignes :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

Voici un commentaire dans un bloc de code :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

Ici est le même bloc de code, avec la ligne de code commentée afin qu’il ne s’exécutera :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

À l’intérieur d’un bloc de code, comme alternative à l’aide de syntaxe de commentaire Razor, vous pouvez utiliser la syntaxe de commentaire du langage de programmation que vous utilisez, comme c# :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

En c#, les commentaires à ligne unique sont précédées par le `//` caractères et commentaires à plusieurs lignes commencent par `/*` et se terminer par `*/`. (Comme avec les commentaires Razor, les commentaires c# ne sont pas rendus dans le navigateur.)

Pour le balisage, comme vous le savez probablement, vous pouvez créer un commentaire HTML :

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

Les commentaires HTML commencent par `<!--` caractères et se terminent par `-->`. Vous pouvez utiliser les commentaires HTML pour entourer le texte non seulement, mais également toutes les balises HTML que vous pouvez souhaiter conserver dans la page, mais ne souhaitez pas effectuer le rendu. Ce commentaire HTML masquera l’intégralité du contenu des balises et le texte qu’ils contiennent :

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

Contrairement aux commentaires HTML, les commentaires Razor *sont* restitué dans la page et l’utilisateur puisse les voir en affichant la page source.

Razor présente des limitations sur les blocs imbriqués de c#. Pour plus d’informations, consultez [Variables c# nommé et imbriqués blocs générer divisé le Code](http://aspnetwebstack.codeplex.com/workitem/1914)

## <a name="variables"></a>Variables

Une variable est un objet nommé qui vous permet de stocker des données. Vous pouvez nommer les variables, mais le nom doit commencer par un caractère alphabétique et il ne peut pas contenir d’espaces blancs ou des caractères réservés.

### <a name="variables-and-data-types"></a>Variables et Types de données

Une variable peut avoir un type de données spécifique, ce qui indique le type de données est stocké dans la variable. Vous pouvez avoir des variables de chaîne qui stockent des valeurs de chaîne (comme &quot;Hello world&quot;), les variables de type entier qui stockent des valeurs de nombre entier (par exemple, 3 ou 79) et les variables de date qui stockent des valeurs de date dans une variété de formats (par exemple, 4/12/2012 ou mars 2009 ). Et il existe de nombreux autres types de données que vous pouvez utiliser.

Toutefois, en général inutile de spécifier un type d’une variable. La plupart du temps, ASP.NET peut déterminer le type en fonction de la façon dont les données dans la variable sont utilisées. (Il peut arriver que vous devez spécifier un type ; vous verrez d’exemples où cela est vrai.)

Vous déclarez une variable à l’aide du `var` mot clé (si vous ne souhaitez pas spécifier un type) ou en utilisant le nom du type :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

L’exemple suivant montre certaines utilisations courantes des variables dans une page web :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

Si vous combinez les exemples précédents dans une page, vous consultez ces informations apparaissent dans un navigateur :

![Razor-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Conversion et le test des Types de données

Bien qu’ASP.NET peut généralement déterminer automatiquement un type de données, parfois, il ne peut pas. Par conséquent, vous devrez peut-être aider ASP.NET en effectuant une conversion explicite. Même si vous n’êtes pas obligé de convertir les types, il est parfois utile tester et pour voir quel type de données vous travaillez peut-être avec.

Le cas le plus courant est que vous devez convertir une chaîne en un autre type, comme un entier ou une date. L’exemple suivant illustre un cas classique où vous devez convertir une chaîne en nombre.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

En règle générale, l’entrée d’utilisateur vous parviennent sous forme de chaînes. Même si vous avez invité des utilisateurs à entrer un numéro, et même s’ils ont entré un chiffre, lorsque l’entrée d’utilisateur est soumise et lisez-le dans le code, les données sont au format de chaîne. Par conséquent, vous devez convertir la chaîne en nombre. Dans l’exemple, si vous essayez d’effectuer des opérations arithmétiques sur les valeurs sans les convertir, l’erreur suivante se produit, car ASP.NET ne peut pas ajouter deux chaînes :

*Impossible de convertir implicitement le type 'string' en 'int'.*

Pour convertir les valeurs à des entiers, vous appelez le `AsInt` (méthode). Si la conversion est réussie, vous pouvez ensuite ajouter les numéros.

Le tableau suivant répertorie les méthodes de conversion et de test habituellement pour les variables.

:::row:::
    :::column:::
    <strong>Méthode</strong>
    :::column-end:::
    :::column:::
    <strong>Description</strong>
    :::column-end:::
    :::column:::
    <strong>Exemple</strong>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
    Convertit une chaîne qui représente un nombre entier (par exemple, « 593 ») vers un entier.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
    Convertit une chaîne telle que &quot;true&quot; ou &quot;false&quot; à un type booléen.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
    Convertit une chaîne qui a une valeur décimale comme &quot;1.3&quot; ou &quot;7.439&quot; un nombre à virgule flottante.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
    Convertit une chaîne qui a une valeur décimale comme &quot;1.3&quot; ou &quot;7.439&quot; un nombre décimal. (Dans ASP.NET, un nombre décimal est plus précis qu’un nombre à virgule flottante.)
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
    Convertit une chaîne qui représente une valeur de date et d’heure pour ASP.NET `DateTime` type.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
    Convertit n’importe quel autre type de données en une chaîne.
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a>Opérateurs

Un opérateur est un mot clé ou un caractère qui indique à ASP.NET quel type de commande à effectuer dans une expression. Le langage c# (et la syntaxe Razor qui repose sur ce dernier) prend en charge de nombreux opérateurs, mais il vous suffit de reconnaître les quelques pour commencer. Le tableau suivant récapitule les opérateurs courants.


:::row:::
    :::column:::
    <strong>Operator</strong>
    :::column-end:::
    :::column:::
    <strong>Description</strong>
    :::column-end:::
    :::column:::
    <strong>Exemples</strong>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+` `-` `*` `/`
    :::column-end:::
    :::column:::
    Opérateurs mathématiques utilisés dans les expressions numériques.
    :::column-end:::
    :::column:::
        [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
    Assignation. Assigne la valeur située à droite d’une instruction à l’objet sur le côté gauche.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `==`
    :::column-end:::
    :::column:::
    Égalité Retourne `true` si les valeurs sont égales. (Notez la distinction entre les `=` opérateur et la `==` opérateur.)
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `!=`
    :::column-end:::
    :::column:::
    Inégalité Retourne `true` si les valeurs ne sont pas égales.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
    Moins-à, supérieur-à, inférieur ou égal et supérieure ou égale.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+`
    :::column-end:::
    :::column:::
    Concaténation, qui est utilisée pour joindre des chaînes. ASP.NET sait que la différence entre cet opérateur et l’opérateur d’addition en fonction du type de données de l’expression.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+=` `-=`
    :::column-end:::
    :::column:::
    Les opérateurs incrémentation et de décrémentation, qui l’addition et de soustraction (respectivement) de 1 à partir d’une variable.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
    Point. Utilisé pour distinguer les objets et leurs propriétés et les méthodes.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
    Parenthèses. Utilisé pour les expressions de groupe et pour passer des paramètres aux méthodes.
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `[]`
    :::column-end:::
    :::column:::
    Des crochets. Utilisé pour accéder aux valeurs dans les tableaux ou collections.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `!`
    :::column-end:::
    :::column:::
    Non. Inverse un `true` valeur `false` et vice versa. Généralement utilisé comme un moyen rapide pour tester `false` (autrement dit, pour pas `true`).
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `&&` <code>&#124;&#124;</code>
    :::column-end:::
    :::column:::
    AND logique et ou des conditions qui servent à lier.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    :::column-end:::
:::row-end:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a>Utilisation de fichiers et chemins d’accès de dossier dans le Code

Souvent, vous allez travailler avec les chemins d’accès de fichier et dossier dans votre code. Voici un exemple de structure de dossiers physiques pour un site Web tel qu’il peut apparaître sur votre ordinateur de développement :

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

Voici quelques détails essentiels sur les URL et les chemins d’accès :

- Une URL commence par un nom de domaine (`http://www.example.com`) ou un nom de serveur (`http://localhost`, `http://mycomputer`).
- Une URL correspond à un chemin d’accès physique sur un ordinateur hôte. Par exemple, `http://myserver` peuvent correspondre au dossier *C:\websites\mywebsite* sur le serveur.
- Un chemin d’accès virtuel est un raccourci pour représenter les chemins d’accès dans le code sans avoir à spécifier le chemin d’accès complet. Il inclut la partie d’une URL qui suit le nom de domaine ou du serveur. Lorsque vous utilisez des chemins d’accès virtuels, vous pouvez déplacer votre code vers un autre domaine ou un serveur sans avoir à mettre à jour les chemins d’accès.

Voici un exemple pour vous aider à comprendre les différences :

| URL complète | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| Nom du serveur | *mycompanyserver* |
| Chemin d'accès virtuel | */humanresources/CompanyPolicy.htm* |
| Chemin d’accès physique | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

Est la racine virtuelle /, tout comme la racine de votre lecteur est \. (Chemins d’accès du dossier virtuel utilisent toujours des barres obliques.) Le chemin d’accès virtuel d’un dossier ne doit pas avoir le même nom que le dossier physique ; Il peut être un alias. (Sur les serveurs de production, le chemin d’accès virtuel rarement correspond à un chemin d’accès physique exact.)

Lorsque vous travaillez avec des fichiers et dossiers dans le code, parfois, vous devez référencer le chemin d’accès physique et parfois selon les objets que vous travaillez avec un chemin d’accès virtuel. ASP.NET vous fournit ces outils pour l’utilisation des chemins d’accès de fichier et dossier dans le code : le `Server.MapPath` (méthode) et le `~` opérateur et `Href` (méthode).

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>Conversion des chemins d’accès virtuels physiques : la méthode Server.MapPath

Le `Server.MapPath` méthode convertit un chemin d’accès virtuel (comme */default.cshtml*) à un chemin d’accès physique absolu (comme *C:\WebSites\MyWebSiteFolder\default.cshtml*). Vous utilisez cette méthode chaque fois que vous avez besoin d’un chemin d’accès physique complet. Un exemple typique est lors de la lecture ou écriture d’un fichier texte ou un fichier image sur le serveur web.

En général, vous ne connaissez pas de chemin d’accès physique absolu de votre site sur le serveur d’hébergement d’un site, donc cette méthode peut convertir le chemin d’accès, vous connaissez — le chemin d’accès virtuel, le chemin d’accès correspondant sur le serveur pour vous. Vous passez le chemin d’accès virtuel à un fichier ou dossier à la méthode et retourne le chemin d’accès physique :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Faisant référence à la racine virtuelle : le ~ opérateur et Href (méthode)

Dans un *.cshtml* ou *.vbhtml* fichier, vous pouvez référencer le chemin d’accès virtuel racine à l’aide du `~` opérateur. Cela est très pratique, car vous pouvez déplacer les pages dans un site, et des liens qu’ils contiennent vers d’autres pages ne sera pas interrompues. Il est également pratique au cas où vous décidez de déplacer votre site Web vers un autre emplacement. Voici quelques exemples :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

Si le site Web est `http://myserver/myapp`, voici la façon dont ASP.NET traite ces chemins d’accès lors de l’exécution de la page :

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Vous ne verrez pas ces chemins d’accès en tant que les valeurs de la variable, mais ASP.NET traite les chemins d’accès comme si c’est qu’ils étaient).

Vous pouvez utiliser le `~` opérateur à la fois dans le code serveur (comme ci-dessus) et dans le balisage, comme suit :

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

Dans le balisage, vous utilisez le `~` opérateur pour créer des chemins d’accès aux ressources telles que des fichiers image, d’autres pages web et de fichiers CSS. Lorsque la page s’exécute, ASP.NET recherche via la page (de code et de balisage) et résout tous les `~` références pour le chemin d’accès approprié.

## <a name="conditional-logic-and-loops"></a>Boucles et la logique conditionnelle

Code de serveur ASP.NET vous permet d’effectuer des tâches en fonction des conditions et écrire du code qui se répète les instructions un certain nombre de fois (autrement dit, code qui exécute une boucle).

### <a name="testing-conditions"></a>Conditions de tests

Pour tester une condition simple que vous utilisez la `if` instruction, qui retourne true ou false selon un test que vous spécifiez :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

Le `if` mot clé commence un bloc. Le test (condition) figure entre parenthèses et retourne true ou false. Les instructions qui s’exécutent si le test a la valeur true sont placées entre accolades. Un `if` instruction peut inclure un `else` bloc qui spécifie les instructions à exécuter si la condition a la valeur false :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

Vous pouvez ajouter plusieurs conditions à l’aide un `else if` bloc :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

Dans cet exemple, si la première condition dans if bloc n’est pas la valeur est true, le `else if` condition est vérifiée. Si cette condition est remplie, les instructions dans le `else if` bloc sont exécutées. Si aucune des conditions sont remplies, les instructions dans le `else` bloc sont exécutées. Vous pouvez ajouter un nombre quelconque d’if else bloque et puis fermer avec une `else` bloquer comme le &quot;tout le reste&quot; condition.

Pour tester un grand nombre de conditions, utilisez un `switch` bloc :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

La valeur à tester est entre parenthèses (dans l’exemple, le `weekday` variable). Chaque test individuel utilise un `case` instruction se termine par un signe deux-points ( :)). Si la valeur d’un `case` instruction correspond à la valeur de test, le code dans ce bloc case est exécuté. Vous fermez chaque instruction case avec un `break` instruction. (Si vous oubliez d’inclure un saut dans chaque `case` bloquer, le code de l’élément suivant `case` instruction s’exécute également.) Un `switch` bloc a souvent un `default` instruction en tant que le dernier cas pour un &quot;tout le reste&quot; option qui s’exécute si aucun des autres cas sont remplies.

Le résultat des deux derniers blocs conditionnels affiché dans un navigateur :

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a>Code de boucle

Souvent, vous devez exécuter les mêmes instructions à plusieurs reprises. Pour cela, en effectuant une boucle. Par exemple, vous exécutez souvent les mêmes instructions pour chaque élément dans une collection de données. Si vous savez exactement combien de fois que vous souhaitez effectuer une boucle, vous pouvez utiliser un `for` boucle. Ce type de boucle est particulièrement utile pour allant ou le compte à rebours :

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

La boucle commence par la `for` mot clé, suivie de trois instructions entre parenthèses, chacun termine par un point-virgule.

- Dans les parenthèses, la première instruction (`var i=10;`) crée un compteur et l’initialise à 10. Vous n’êtes pas obligé de nommer le compteur `i` &#8212; vous pouvez utiliser n’importe quelle variable. Lorsque le `for` boucle s’exécute, le compteur est incrémenté automatiquement.
- La deuxième instruction (`i < 21;`) définit la condition pour jusqu’où vous souhaitez compter. Dans ce cas, vous le souhaitez pour accéder à un maximum de 20 (autrement dit, continuez tandis que le compteur est inférieure à 21).
- La troisième instruction (`i++` ) utilise un opérateur d’incrémentation, laquelle spécifie simplement que le compteur doit avoir 1 ajouté chaque fois que la boucle s’exécute.

Les accolades est le code qui s’exécute pour chaque itération de la boucle. Le balisage crée un nouveau paragraphe (`<p>` élément) chaque fois, ajoute une ligne à la sortie, en affichant la valeur de `i` (le compteur). Lorsque vous exécutez cette page, l’exemple crée 11 lignes affichant la sortie, avec le texte dans chaque ligne indiquant le numéro d’élément.

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

Si vous travaillez avec une collection ou un tableau, vous utilisez souvent une `foreach` boucle. Une collection est un groupe d’objets similaires et le `foreach` boucle vous permet de transporter une tâche sur chaque élément dans la collection. Ce type de boucle est pratique pour les collections, car contrairement à un `for` boucle, vous n’êtes pas obligé incrément du compteur ou de définir une limite. Au lieu de cela, le `foreach` code boucle passe simplement via la collection jusqu'à ce qu’elle est terminée.

Par exemple, le code suivant retourne les éléments dans le `Request.ServerVariables` collection, qui est un objet qui contient des informations sur votre serveur web. Il utilise un `foreac` boucle h pour afficher le nom de chaque élément en créant un nouveau `<li>` élément dans une liste à puces HTML.

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

Le `foreach` mot clé est suivi de parenthèses où vous déclarez une variable qui représente un élément unique dans la collection (dans l’exemple, `var item`), suivi par le `in` mot clé, suivie de la collection que vous souhaitez parcourir. Dans le corps de la `foreach` boucle, vous pouvez accéder à l’élément actuel à l’aide de la variable que vous avez déclaré précédemment.

![Razor-Img12](introducing-razor-syntax-c/_static/image12.jpg)

Pour créer une boucle plus généraliste, utilisez la `while` instruction :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

Un `while` boucle commence par la `while` mot clé, suivi de parenthèses, où vous spécifiez la durée pendant laquelle la boucle continue (ici, pour tant que `countNum` est inférieur à 50), puis le bloc à répéter. Incrémentent généralement des boucles (ajouter à) ou de décrémentation (Soustraire) une variable ou un objet utilisé pour le comptage. Dans l’exemple, le `+=` opérateur ajoute 1 à `countNum` chaque fois que la boucle s’exécute. (Pour décrémenter une variable dans une boucle qui compte à rebours, vous utiliseriez l’opérateur de décrémentation `-=`).

## <a name="objects-and-collections"></a>Objets et Collections

Presque tous les éléments dans un site Web ASP.NET sont un objet, y compris la page web elle-même. Cette section décrit certains objets importants que vous utiliserez fréquemment dans votre code.

### <a name="page-objects"></a>Objets de page

L’objet de base dans ASP.NET est la page. Vous pouvez accéder à propriétés de l’objet page directement sans aucun objet éligible. Le code suivant obtient le chemin d’accès du fichier de la page, à l’aide de la `Request` objet de la page :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

Pour le rendre claire que vous référencez propriétés et des méthodes sur l’objet actuel de la page, vous pouvez éventuellement utiliser le mot clé `this` pour représenter l’objet de page dans votre code. Voici l’exemple de code précédent, avec `this` ajoutée pour représenter la page :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

Vous pouvez utiliser les propriétés de la `Page` objet pour obtenir un grand nombre d’informations, telles que :

- `Request`. Comme vous l’avez déjà vu, il s’agit d’une collection d’informations sur la requête actuelle, y compris le type de navigateur fait la demande, l’URL de la page, de l’identité de l’utilisateur, etc.
- `Response`. Il s’agit d’une collection d’informations sur la réponse (page) qui sera envoyée au navigateur une fois le code du serveur en cours d’exécution. Par exemple, vous pouvez utiliser cette propriété pour écrire des informations dans la réponse. 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a>Objets de collection (tableaux et les dictionnaires)

Un *collection* est un groupe d’objets du même type, telle qu’une collection de `Customer` objets à partir d’une base de données. ASP.NET contient de nombreux regroupements intégrés, tels que le `Request.Files` collection.

Souvent, vous allez travailler avec des données dans des collections. Deux types de collections courants sont le *tableau* et *dictionnaire*. Un tableau est utile lorsque vous souhaitez stocker une collection d’éléments similaires mais ne souhaitez pas créer une variable distincte pour contenir chaque élément :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

Les tableaux, vous déclarez un type de données spécifique, tel que `string`, `int`, ou `DateTime`. Pour indiquer que la variable peut contenir un tableau, vous ajoutez des crochets à la déclaration (tel que `string[]` ou `int[]`). Vous pouvez accéder à des éléments dans un tableau à l’aide de leur position (index) ou en utilisant la `foreach` instruction. Index de tableau sont de base zéro &#8212; autrement dit, le premier élément est en position 0, le deuxième élément est à la position 1 et ainsi de suite.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

Vous pouvez déterminer le nombre d’éléments dans un tableau en obtenant son `Length` propriété. Pour obtenir la position d’un élément spécifique dans le tableau (à la recherche dans le tableau), utilisez le `Array.IndexOf` (méthode). Vous pouvez également effectuer les opérations comme inverse le contenu d’un tableau (la `Array.Reverse` méthode) ou trier le contenu (la `Array.Sort` méthode).

La sortie du code de tableau de chaîne affichée dans un navigateur :

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

Un dictionnaire est une collection de paires clé/valeur, où vous fournissez la clé (ou nom) pour définir ou récupérer la valeur correspondante :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

Pour créer un dictionnaire, vous utilisez le `new` mot clé pour indiquer que vous créez un nouvel objet de dictionnaire. Vous pouvez affecter un dictionnaire à une variable à l’aide du `var` mot clé. Vous indiquez les types de données des éléments dans le dictionnaire à l’aide de crochets pointus ( `< >` ). À la fin de la déclaration, vous devez ajouter une paire de parenthèses, car il s’agit en fait une méthode qui crée un dictionnaire.

Pour ajouter des éléments au dictionnaire, vous pouvez appeler la `Add` méthode de la variable de dictionnaire (`myScores` dans ce cas), puis spécifiez une clé et une valeur. Ou bien, vous pouvez utiliser des crochets pour indiquer la clé et d’effectuer l’affectation simple, comme dans l’exemple suivant :

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

Pour obtenir une valeur à partir du dictionnaire, vous spécifiez la clé entre crochets :

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a>Appel de méthodes avec des paramètres

Que vous lire plus haut dans cet article, les objets que vous programmez avec peuvent avoir des méthodes. Par exemple, un `Database` objet peut avoir un `Database.Connect` (méthode). De nombreuses méthodes ont également un ou plusieurs paramètres. Un *paramètre* est une valeur que vous passez à une méthode pour activer la méthode effectuer sa tâche. Par exemple, examinez une déclaration pour le `Request.MapPath` (méthode), qui prend trois paramètres :

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

(La ligne a été encapsulée pour le rendre plus lisible. N’oubliez pas que vous pouvez insérer des sauts de ligne presque partout, sauf à l’intérieur des chaînes qui sont placés entre guillemets.)

Cette méthode retourne le chemin d’accès physique sur le serveur qui correspond à un chemin d’accès virtuel spécifié. Les trois paramètres pour la méthode sont `virtualPath`, `baseVirtualDir`, et `allowCrossAppMapping`. (Notez que dans la déclaration, les paramètres sont répertoriés avec les types de données des données qu’ils accepte). Lorsque vous appelez cette méthode, vous devez fournir des valeurs pour tous les trois paramètres.

La syntaxe Razor vous propose deux options pour passer des paramètres à une méthode : *paramètres positionnels* et *des paramètres nommés*. Pour appeler une méthode à l’aide des paramètres positionnels, vous transmettez les paramètres dans un ordre strict est spécifié dans la déclaration de méthode. (Vous généralement sauriez cet ordre en lisant la documentation de la méthode.) Vous devez suivre l’ordre, et vous ne pouvez pas ignorer les paramètres &#8212; si nécessaire, vous passez une chaîne vide (`""`) ou `null` pour que vous n’avez pas une valeur pour un paramètre de positionnement.

L’exemple suivant suppose que vous avez un dossier nommé *scripts* sur votre site Web. Le code appelle la `Request.MapPath` et transmet des valeurs pour les trois paramètres dans l’ordre approprié. Il affiche ensuite le chemin d’accès mappé qui en résulte.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

Quand une méthode possède de nombreux paramètres, vous pouvez conserver votre code plus lisible à l’aide des paramètres nommés. Pour appeler une méthode à l’aide de paramètres nommés, spécifiez le nom du paramètre suivi de deux-points ( :)), puis la valeur. L’avantage des paramètres nommés, que vous pouvez les transmettre dans l’ordre que vous souhaitez. (Un inconvénient est que l’appel de méthode n’est pas aussi compact.)

L’exemple suivant appelle la méthode décrite ci-dessus, mais utilise des paramètres nommés pour fournir les valeurs :

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

Comme vous pouvez le voir, les paramètres sont passés dans un ordre différent. Toutefois, si vous exécutez l’exemple précédent et cet exemple, ils allons retourner la même valeur.

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a>Gestion des erreurs

### <a name="try-catch-statements"></a>Instructions Try-Catch

Vous aurez souvent des instructions dans votre code peut échouer pour des raisons en dehors de votre contrôle. Exemple :

- Si votre code essaie de créer ou d’accéder à un fichier, toutes sortes d’erreurs peuvent se produire. Le fichier n’existe ne peut-être pas, il est peut-être verrouillé, le code ne peut pas disposer des autorisations et ainsi de suite.
- De même, si votre code essaie de mettre à jour des enregistrements dans une base de données, il peut y avoir des problèmes d’autorisations, la connexion à la base de données peut-être être supprimée, les données à enregistrer est peut-être non valide et ainsi de suite.

En termes de programmation, ces situations sont appelées *exceptions*. Si votre code rencontre une exception, il génère (lève) un message d’erreur qui est, au mieux, ennuyeux pour les utilisateurs :

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

Dans les situations où votre code peut rencontrer des exceptions et afin d’éviter les messages d’erreur de ce type, vous pouvez utiliser `try/catch` instructions. Dans la `try` instruction, vous exécutez le code que vous êtes en train de. Dans un ou plusieurs `catch` instructions, vous pouvez rechercher propres erreurs (types d’exceptions spécifiques) qui se sont produites. Vous pouvez inclure autant `catch` instructions que vous avez besoin rechercher des erreurs sont prévues.

> [!NOTE]
> Nous vous recommandons d’éviter à l’aide de la `Response.Redirect` méthode dans `try/catch` instructions, car il peut provoquer une exception dans votre page.


L’exemple suivant montre une page qui crée un fichier texte à la première demande, puis affiche un bouton qui permet à l’utilisateur d’ouvrir le fichier. L’exemple utilise délibérément un nom de fichier incorrect afin qu’elle entraîne une exception. Le code inclut `catch` instructions pour les deux exceptions possibles : `FileNotFoundException`, ce qui se produit si le nom de fichier est incorrect, et `DirectoryNotFoundException`, ce qui se produit si ASP.NET même Impossible de trouver le dossier. (Vous pouvez ne pas commenter une instruction dans l’exemple pour voir comment elle s’exécute lorsque tout fonctionne correctement.)

Si votre code n’a pas gérer l’exception, vous voyez une page d’erreur comme la capture d’écran précédente. Toutefois, le `try/catch` section vous aide à empêcher l’utilisateur de voir ces types d’erreurs.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a>Ressources supplémentaires

**Programmation avec Visual Basic**


[Annexe : langage Visual Basic et syntaxe](https://go.microsoft.com/fwlink/?LinkId=202908)


**Documentation de référence**


[ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)

[Langage c#](https://msdn.microsoft.com/library/kx37x362.aspx)
