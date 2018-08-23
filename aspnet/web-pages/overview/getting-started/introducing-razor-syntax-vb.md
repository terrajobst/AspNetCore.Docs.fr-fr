---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Introduction à la programmation Web ASP.NET à l’aide de la syntaxe Razor (Visual Basic) | Microsoft Docs
author: tfitzmac
description: Cette annexe vous donne une vue d’ensemble de la programmation avec les pages Web ASP.NET dans Visual Basic, à l’aide de la syntaxe Razor.
ms.author: riande
ms.date: 02/07/2014
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: cbec035533c37723afcd5bf4aa0c6e1c83dbae23
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834216"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a>Introduction à la programmation Web ASP.NET à l’aide de la syntaxe Razor (Visual Basic)
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article vous donne une vue d’ensemble de la programmation avec les Pages Web ASP.NET à l’aide de la syntaxe Razor et Visual Basic. ASP.NET est la technologie de Microsoft pour les pages web dynamiques en cours d’exécution sur les serveurs web.
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


Utilisation de la plupart des exemples de l’utilisation d’ASP.NET Web Pages avec syntaxe Razor c#. Mais la syntaxe Razor prend également en charge Visual Basic. Pour programmer une page de web ASP.NET dans Visual Basic, vous créez une page web avec un *.vbhtml* extension de nom de fichier, puis ajoutez le code Visual Basic. Cet article vous donne une vue d’ensemble de l’utilisation avec le langage Visual Basic et de la syntaxe pour créer des pages Web ASP.NET.

> [!NOTE]
> Les modèles de site Web par défaut de Microsoft WebMatrix (**boulangerie**, **galerie de photos**, et **Starter Site**, etc.) sont disponibles dans les versions de c# et Visual Basic. Vous pouvez installer les modèles Visual Basic par comme packages NuGet. Les modèles de site Web sont installés dans le dossier racine de votre site dans un dossier nommé *Templates Microsoft*.


## <a name="the-top-8-programming-tips"></a>Les meilleurs conseils de programmation 8

Cette section répertorie quelques conseils que vous devez absolument connaître lorsque vous commencez à écrire le code de serveur ASP.NET à l’aide de la syntaxe Razor.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Ajouter du code à une page à l’aide du caractère « @ »

Le `@` caractère démarre les expressions inline, les blocs d’instruction unique et les blocs d’instructions multiples :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

Le résultat est affiché dans un navigateur :

![Razor-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> **Codage HTML**
> 
> Lorsque vous affichez le contenu dans une page à l’aide de la `@` de caractères, comme dans les exemples précédents, ASP.NET encode au format HTML la sortie. Cela remplace les caractères HTML réservés (tels que `<` et `>` et `&`) avec des codes qui permettent les caractères à afficher en tant que caractères dans une page web au lieu d’être interprétée comme des balises HTML ou des entités. Sans codage HTML, la sortie à partir de votre code de serveur peuvent ne pas s’afficher correctement et peut exposer une page à des risques de sécurité.
> 
> Si votre objectif est de sortie de balisage HTML qui rend les balises en tant que balisage (par exemple `<p></p>` pour un paragraphe ou `<em></em>` pour mettre en évidence de texte), consultez la section [combinant le texte, le balisage et Code dans les blocs de Code](#BM_CombiningTextMarkupAndCode) plus loin dans cet article.
> 
> Vous trouverez plus d’informations sur le codage HTML dans [utilisation des formulaires HTML dans les Sites ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a>2. Vous placez des blocs de code avec le Code... Code de fin

Un bloc de code inclut une ou plusieurs instructions de code et est placé avec les mots clés `Code` et `End Code`. Placez l’ouverture `Code` mot clé immédiatement après le `@` caractère &#8212; il ne peut pas être d’un espace blanc entre eux.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

Le résultat est affiché dans un navigateur :

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a>3. À l’intérieur d’un bloc, vous obtenez chaque instruction du code avec un saut de ligne

Dans un bloc de code Visual Basic, chaque instruction se termine par un saut de ligne. (Plus loin dans cet article vous verrez un moyen d’encapsuler une instruction de code long en plusieurs lignes si nécessaire.)

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a>4. Vous utilisez des variables pour stocker des valeurs

Vous pouvez stocker des valeurs dans un *variable*, y compris les chaînes, des chiffres et des dates, etc. Vous créez une variable en utilisant le `Dim` mot clé. Vous pouvez insérer des valeurs des variables directement dans une page à l’aide `@`.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

Le résultat est affiché dans un navigateur :

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Vous placez les valeurs de chaîne littérale entre guillemets doubles

Un *chaîne* est une séquence de caractères qui sont traitées en tant que texte. Pour spécifier une chaîne, mettez-le entre guillemets doubles :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

Pour incorporer des guillemets doubles dans une valeur de chaîne, insérez deux caractères de guillemet double. Si vous souhaitez que le caractère guillemet double d’apparaître une fois dans la sortie de page, entrez-le en tant que `""` au sein de la proposition de prix de chaîne et si vous souhaitez qu’il apparaisse à deux reprises, entrez-le en tant que `""""` dans la chaîne entre guillemets.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

Le résultat est affiché dans un navigateur :

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a>6. Code Visual Basic n’est pas sensible à la casse

Le langage Visual Basic n’est pas sensible à la casse. Mots clés de programmation (telles que `Dim`, `If`, et `True`) et les noms de variable (comme `myString`, ou `subTotal`) peuvent être écrites dans tous les cas.

Les lignes de code suivantes assigner une valeur à la variable `lastname` à l’aide d’une minuscule nommez, puis affichez la valeur de la variable à la page à l’aide d’un nom en majuscules.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

Le résultat est affiché dans un navigateur :

![syntaxe-VB-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a>7. Une grande partie de votre codage implique l’utilisation des objets

Un objet représente une chose que vous pouvez programmer avec &#8212; une page, une zone de texte, un fichier, une image, une requête web, un message électronique, un enregistrement de client (ligne de base de données), etc. Objets ont des propriétés qui décrivent leurs caractéristiques &#8212; un objet de zone de texte a un `Text` propriété, un objet de requête a un `Url` propriété, un message électronique a un `From` propriété et un objet customer a une `FirstName` propriété. Objets possèdent également des méthodes qui sont le &quot;verbes&quot; qu’ils peuvent effectuer. Exemples incluent un objet fichier `Save` (méthode), un objet image `Rotate` (méthode) et un objet de messagerie `Send` (méthode).

Vous utiliserez souvent le `Request` champs d’objet, ce qui vous donne des informations telles que les valeurs du formulaire sur la page (zones de texte, etc.), le type de navigateur fait la demande, l’URL de la page, de l’identité de l’utilisateur, etc. Cet exemple montre comment accéder aux propriétés de la `Request` objet et comment appeler le `MapPath` méthode de la `Request` objet, ce qui vous donne le chemin d’accès absolu de la page sur le serveur :

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

Le résultat est affiché dans un navigateur :

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. Vous pouvez écrire du code qui prend des décisions

Une fonctionnalité clé des pages web dynamiques est que vous pouvez déterminer ce qu’il faut faire en fonction des conditions. L’approche la plus courante pour ce faire est avec la `If` instruction (et éventuellement `Else` instruction).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

L’instruction `If IsPost` est un moyen rapide de la rédaction de `If IsPost = True`. Avec `If` instructions, il existe plusieurs façons pour tester les conditions, la répétition des blocs de code, et ainsi de suite, qui sont décrits plus loin dans cet article.

Le résultat affiché dans un navigateur (après avoir cliqué sur **envoyer**) :

![Razor-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> **HTTP GET et POST méthodes et la propriété IsPost**
> 
> Le protocole utilisé pour les pages web (HTTP) prend en charge un nombre très limité de méthodes (&quot;verbes&quot;) qui sont utilisés pour effectuer des demandes au serveur. Les deux plus courantes sont GET, qui est utilisé pour lire une page, et POST, ce qui est utilisé pour envoyer une page. En règle générale, la première fois qu’un utilisateur demande une page, la page est demandée à l’aide de GET. Si l’utilisateur remplit un formulaire, puis sur **envoyer**, le navigateur envoie une demande POST vers le serveur.
> 
> Dans la programmation web, il est souvent utile de savoir si une page est demandée sous la forme d’une opération GET ou un billet afin que vous sachiez comment traiter la page. Dans ASP.NET Web Pages, vous pouvez utiliser le `IsPost` propriété pour déterminer si une requête est une opération GET ou POST. Si la demande est une publication, le `IsPost` propriété retournera la valeur true, et vous pouvez effectuer les opérations en lecture les valeurs des zones de texte sur un formulaire. Vous verrez de nombreux exemples vous montrent comment traiter la page différemment selon la valeur de `IsPost`.


## <a name="a-simple-code-example"></a>Un exemple de Code Simple

Cette procédure vous montre comment créer une page qui illustre des techniques de programmation de base. Dans l’exemple, vous créez une page qui permet aux utilisateurs d’entrer des deux nombres, puis il les ajoute et affiche le résultat.

1. Dans votre éditeur, créez un nouveau fichier et nommez-le *AddNumbers.vbhtml*.
2. Copiez le code et le balisage suivant dans la page, en remplaçant tout élément déjà dans la page.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    Voici quelques éléments de noter :

    - Le `@` caractère démarre le premier bloc de code dans la page, et il précède le `totalMessage` variable incorporé vers le bas.
    - Le bloc en haut de la page est placé dans `Code...End Code`.
    - Les variables `total`, `num1`, `num2`, et `totalMessage` stocker plusieurs nombres et une chaîne.
    - La valeur de littéral de chaîne assignée à la `totalMessage` variable est entre guillemets doubles.
    - Étant donné que code Visual Basic n’est pas la casse, lorsque le `totalMessage` variable est utilisée au bas de la page, son nom ne doit correspondre à l’orthographe de la déclaration de variable en haut de la page. La casse n’a pas d’importance.
    - L’expression `num1.AsInt()`  +  `num2.AsInt()` montre comment utiliser des méthodes et des objets. Le `AsInt` méthode sur chaque variable convertit la chaîne entrée par un utilisateur à un nombre entier (entier) qui peut être ajouté.
    - Le `<form>` balise inclut un `method="post"` attribut. Spécifie que lorsque l’utilisateur clique sur **ajouter**, la page sera être envoyée au serveur à l’aide de la méthode HTTP POST. Lorsque la page est envoyée, le code `If IsPost` prend la valeur true et l’instruction conditionnelle de code s’exécute, affiche le résultat de l’ajout de nombres.
3. Enregistrez la page et l’exécuter dans un navigateur. (Assurez-vous que la page est sélectionnée dans le **fichiers** espace de travail avant de l’exécuter.) Entrez deux nombres entiers, puis activez la **ajouter** bouton.

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a>Syntaxe et le langage Visual Basic

Vous avez vu précédemment un exemple de base de la création d’une page web ASP.NET et comment vous pouvez ajouter du code de serveur à la balise HTML. Ici, vous allez apprendre les principes fondamentaux de l’utilisation de Visual Basic pour écrire du code de serveur ASP.NET à l’aide de la syntaxe Razor &#8212; , autrement dit, les règles langage de programmation.

Si vous êtes familiarisé avec la programmation (surtout si vous avez utilisé le C, C++, c#, Visual Basic ou JavaScript), une grande partie de ce que vous lire ici sera familier. Vous devrez probablement vous familiariser uniquement avec le code de WebMatrix est ajouté à balisage dans *.vbhtml* fichiers.

### <a id="BM_CombiningTextMarkupAndCode"></a>  Combinaison de texte, balisage et code dans les blocs de code

Dans les blocs de code serveur, vous souhaiterez souvent de sortie texte et des balises à la page. Si un bloc de code de serveur contient du texte non code, mais qui au lieu de cela doit être restitué en l’état, ASP.NET doit être en mesure de distinguer ce texte à partir du code. Pour ce faire, plusieurs méthodes sont possibles.

- Placez le texte dans un élément de bloc HTML comme `<p></p>` ou `<em></em>`:

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    L’élément HTML peut inclure de texte, des éléments HTML supplémentaires et des expressions de code serveur. Lorsque ASP.NET voit la balise HTML d’ouverture (par exemple, `<p>`), il restitue tout ce que l’élément et son contenu sous la forme est dans le navigateur (et résout les expressions de code de serveur).

- Utilisez le `@:` opérateur ou la `<text>` élément. Le `@:` génère une ligne unique de contenu contenant du texte brut ou des balises HTML sans correspondance ; le `<text>` élément englobe plusieurs lignes de sortie. Ces options sont utiles lorsque vous ne souhaitez pas restituer un élément HTML dans le cadre de la sortie.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    L’exemple suivant répète l’exemple précédent, mais utilise une seule paire de `<text>` balises pour encadrer le texte à afficher.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    Dans l’exemple suivant, le `<text>` et `</text>` balises placer trois lignes, disposant de certaines relation contenant-contenu de texte et de des balises HTML sans correspondance (`<br />`), ainsi que le code serveur et de mise en correspondance des balises HTML. Là encore, peut également faire précéder chaque ligne individuellement avec la `@:` opérateur ; les deux fonctionnent de façon.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > Sortie de texte comme indiqué dans cette section &#8212; à l’aide d’un élément HTML, le `@:` opérateur, ou la `<text>` élément &#8212; ASP.NET n’encoder en HTML la sortie. (Comme indiqué précédemment, ASP.NET n’encode pas la sortie des expressions de code serveur et les blocs de code de serveur qui sont précédés `@`, sauf dans les cas spéciales indiquées dans cette section.)

### <a name="whitespace"></a>Whitespace

L’instruction n’affectent pas les espaces supplémentaires dans une instruction (et en dehors d’un littéral de chaîne) :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a>Dernières instructions longues sur plusieurs lignes

Vous pouvez interrompre une instruction de code long en plusieurs lignes en utilisant le caractère de soulignement `_` (qui, dans Visual Basic est appelé le *caractère de continuation*) après chaque ligne de code. Pour arrêter une instruction sur la ligne suivante, ajoutez un espace, puis le caractère de continuation à la fin de la ligne. L’instruction continue sur la ligne suivante. Vous pouvez encapsuler des instructions sur autant de lignes que vous avez besoin améliorer la lisibilité. Les instructions suivantes sont les mêmes :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

Toutefois, vous ne pouvez pas encapsuler une ligne au milieu d’un littéral de chaîne. L’exemple suivant ne fonctionne pas :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

Pour combiner une longue chaîne répartie sur plusieurs lignes, comme le code ci-dessus, vous devez utiliser le *opérateur de concaténation* (`&`), comme vous le verrez plus loin dans cet article.

### <a name="code-comments"></a>Commentaires de code

Commentaires vous permettent de rédiger des notes pour vous-même ou d’autres personnes. Les commentaires de syntaxe Razor sont précédés de `@*` et se terminer par `*@`.

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

Dans les blocs de code, vous pouvez utiliser les commentaires de la syntaxe Razor, ou vous pouvez utiliser des caractères de commentaire Visual Basic ordinaires, qui est un guillemet (`'`) ajouté devant chaque ligne.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a>Variables

Une variable est un objet nommé qui vous permet de stocker des données. Vous pouvez nommer les variables, mais le nom doit commencer par un caractère alphabétique et il ne peut pas contenir d’espaces blancs ou des caractères réservés. En Visual Basic, comme vous l’avez vu précédemment, la casse des lettres dans un nom de variable importe peu.

### <a name="variables-and-data-types"></a>Variables et types de données

Une variable peut avoir un type de données spécifique, ce qui indique le type de données est stocké dans la variable. Vous pouvez avoir des variables de chaîne qui stockent des valeurs de chaîne (comme &quot;Hello world&quot;), les variables de type entier qui stockent des valeurs de nombre entier (par exemple, 3 ou 79) et les variables de date qui stockent des valeurs de date dans une variété de formats (par exemple, 4/12/2012 ou mars 2009 ). Et il existe de nombreux autres types de données que vous pouvez utiliser.

Toutefois, vous n’êtes pas obligé de spécifier un type pour une variable. Dans la plupart des cas ASP.NET peut déterminer le type en fonction de la façon dont les données dans la variable sont utilisées. (Il peut arriver que vous devez spécifier un type ; vous verrez d’exemples où cela est vrai.)

Pour déclarer une variable sans spécification d’un type, utilisez `Dim` ainsi que le nom de variable (par exemple, `Dim myVar`). Pour déclarer une variable avec un type, utilisez `Dim` ainsi que le nom de variable, suivi par `As` , puis du nom de type (par exemple, `Dim myVar As String`).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

L’exemple suivant illustre certaines expressions inline qui utilisent les variables dans une page web.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

Le résultat est affiché dans un navigateur :

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Conversion et le test des types de données

Bien qu’ASP.NET peut généralement déterminer automatiquement un type de données, parfois, il ne peut pas. Par conséquent, vous devrez peut-être aider ASP.NET en effectuant une conversion explicite. Même si vous n’êtes pas obligé de convertir les types, il est parfois utile tester et pour voir quel type de données vous travaillez peut-être avec.

Le cas le plus courant est que vous devez convertir une chaîne en un autre type, comme un entier ou une date. L’exemple suivant illustre un cas classique où vous devez convertir une chaîne en nombre.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

En règle générale, l’entrée d’utilisateur vous parviennent sous forme de chaînes. Même si vous avez invité de l’utilisateur à entrer un numéro, et même s’ils ont entré un chiffre, lorsque l’entrée d’utilisateur est soumise et lisez-le dans le code, les données sont au format de chaîne. Par conséquent, vous devez convertir la chaîne en nombre. Dans l’exemple, si vous essayez d’effectuer des opérations arithmétiques sur les valeurs sans les convertir, l’erreur suivante se produit, car ASP.NET ne peut pas ajouter deux chaînes :

`Cannot implicitly convert type 'string' to 'int'.`

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
        Convertit une chaîne qui représente un nombre entier (comme &quot;593&quot;) vers un entier.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
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
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
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
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
        Convertit une chaîne qui a une valeur décimale comme &quot;1.3&quot; ou &quot;7.439&quot; un nombre décimal. (Dans ASP.NET, un nombre décimal est plus précis qu’un nombre à virgule flottante.) :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
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
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
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
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    :::column-end:::
:::row-end:::


## <a name="operators"></a>Opérateurs

Un opérateur est un mot clé ou un caractère qui indique à ASP.NET quel type de commande à effectuer dans une expression. Visual Basic prend en charge de nombreux opérateurs, mais il vous suffit de reconnaître les quelques à commencer à développer des pages web ASP.NET. Le tableau suivant récapitule les opérateurs courants.


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
        `+ - * /`
    :::column-end:::
    :::column:::
        Opérateurs mathématiques utilisés dans les expressions numériques.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
        Assignation et égalité. Selon le contexte, soit assigne la valeur située à droite d’une instruction à l’objet sur le côté gauche ou vérifie les valeurs pour l’égalité.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `<>`
    :::column-end:::
    :::column:::
        Inégalité Retourne `True` si les valeurs ne sont pas égales.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
        Inférieur à, supérieur à, inférieur ou égal et supérieur ou égal.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `&`
    :::column-end:::
    :::column:::
        Concaténation, qui est utilisée pour joindre des chaînes.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+= -=`
    :::column-end:::
    :::column:::
        Les opérateurs incrémentation et de décrémentation, qui l’addition et de soustraction (respectivement) de 1 à partir d’une variable.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
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
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
        Parenthèses. Utilisé pour les expressions de groupe, pour passer des paramètres aux méthodes et pour accéder aux membres de tableaux et collections.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `Not`
    :::column-end:::
    :::column:::
        Non. Inverse la valeur true à false et inversement. Généralement utilisé comme un moyen rapide pour tester `False` (autrement dit, pour pas `True`).
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AndAlso OrElse`
    :::column-end:::
    :::column:::
        AND logique et ou des conditions qui servent à lier.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    :::column-end:::
:::row-end:::

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

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Faisant référence à la racine virtuelle : le ~ opérateur et Href (méthode)

Dans un *.cshtml* ou *.vbhtml* fichier, vous pouvez référencer le chemin d’accès virtuel racine à l’aide du `~` opérateur. Cela est très pratique, car vous pouvez déplacer les pages dans un site, et des liens qu’ils contiennent vers d’autres pages ne sera pas interrompues. Il est également pratique au cas où vous décidez de déplacer votre site Web vers un autre emplacement. Voici quelques exemples :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

Si le site Web est `http://myserver/myapp`, voici la façon dont ASP.NET traite ces chemins d’accès lors de l’exécution de la page :

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Vous ne verrez pas ces chemins d’accès en tant que les valeurs de la variable, mais ASP.NET traite les chemins d’accès comme si c’est qu’ils étaient).

Vous pouvez utiliser le `~` opérateur à la fois dans le code serveur (comme ci-dessus) et dans le balisage, comme suit :

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

Dans le balisage, vous utilisez le `~` opérateur pour créer des chemins d’accès aux ressources telles que des fichiers image, d’autres pages web et de fichiers CSS. Lorsque la page s’exécute, ASP.NET recherche via la page (de code et de balisage) et résout tous les `~` références pour le chemin d’accès approprié.

## <a name="conditional-logic-and-loops"></a>Boucles et la logique conditionnelle

Code de serveur ASP.NET vous permet d’effectuer les tâches en fonction de conditions et d’écrire du code qui se répète les instructions de code, autrement dit, un certain nombre de fois qui s’exécute une boucle).

### <a name="testing-conditions"></a>Conditions de tests

Pour tester une condition simple que vous utilisez le `If...Then` instruction, qui retourne `True` ou `False` basé sur un test que vous spécifiez :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

Le `If` mot clé commence un bloc. Le test (condition) suit le `If` mot clé et retourne true ou false. Le `If` instruction se termine par `Then`. Les instructions qui s’exécutera si le test a la valeur true sont encadrées par des `If` et `End If`. Un `If` instruction peut inclure un `Else` bloc qui spécifie les instructions à exécuter si la condition a la valeur false :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

Si un `If` instruction commence un bloc de code, vous n’êtes pas obligé d’utiliser la normale `Code...End Code` instructions pour inclure les blocs. Vous pouvez simplement ajouter `@` au bloc, et il ne fonctionnera pas. Cette approche fonctionne avec `If` ainsi que d’autres mots clés qui sont suivis par les blocs de code, y compris de programmation de Visual Basic `For`, `For Each`, `Do While`, etc.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

Vous pouvez ajouter plusieurs conditions à l’aide d’un ou plusieurs `ElseIf` blocs :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

Dans cet exemple, si la première condition dans la `If` bloc n’est pas la valeur est true, le `ElseIf` condition est vérifiée. Si cette condition est remplie, les instructions dans le `ElseIf` bloc sont exécutées. Si aucune des conditions sont remplies, les instructions dans le `Else` bloc sont exécutées. Vous pouvez ajouter un nombre quelconque de `ElseIf` bloque et puis fermer avec une `Else` bloquer comme le &quot;tout le reste&quot; condition.

Pour tester un grand nombre de conditions, utilisez un `Select Case` bloc :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

La valeur à tester est entre parenthèses (dans l’exemple, la variable de jour de la semaine). Chaque test individuel utilise un `Case` instruction qui indique une valeur. Si la valeur d’un `Case` instruction correspond à la valeur de test, le code qui `Case` bloc est exécuté.

Le résultat des deux derniers blocs conditionnels affiché dans un navigateur :

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a>Code de boucle

Souvent, vous devez exécuter les mêmes instructions à plusieurs reprises. Pour cela, en effectuant une boucle. Par exemple, vous exécutez souvent les mêmes instructions pour chaque élément dans une collection de données. Si vous savez exactement combien de fois que vous souhaitez effectuer une boucle, vous pouvez utiliser un `For` boucle. Ce type de boucle est particulièrement utile pour allant ou le compte à rebours :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

La boucle commence par la `For` mot clé, suivie de trois éléments :

- Immédiatement après le `For` instruction, vous déclarez une variable de compteur (vous n’êtes pas obligé d’utiliser `Dim`), puis indiquez la plage, comme dans `i = 10 to 20`. Cela signifie que la variable `i` démarre à compter à 10 et continuer jusqu'à ce qu’il atteigne 20 (inclus).
- Entre le `For` et `Next` instructions est le contenu du bloc. Il peut contenir un ou plusieurs instructions de code qui s’exécutent avec chaque boucle.
- La `Next i` instruction termine la boucle. Il incrémente le compteur et démarre l’itération suivante de la boucle.

La ligne de code entre la `For` et `Next` lignes contient le code qui s’exécute pour chaque itération de la boucle. Le balisage crée un nouveau paragraphe (`<p>` élément) chaque fois, ajoute une ligne à la sortie, en affichant la valeur de i (le compteur). Lorsque vous exécutez cette page, l’exemple crée 11 lignes affichant la sortie, avec le texte dans chaque ligne indiquant le numéro d’élément.

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

Si vous travaillez avec une collection ou un tableau, vous utilisez souvent une `For Each` boucle. Une collection est un groupe d’objets similaires et le `For Each` boucle vous permet de transporter une tâche sur chaque élément dans la collection. Ce type de boucle est pratique pour les collections, car contrairement à un `For` boucle, vous n’êtes pas obligé incrément du compteur ou de définir une limite. Au lieu de cela, le `For Each` code boucle passe simplement via la collection jusqu'à ce qu’elle est terminée.

Cet exemple retourne les éléments dans le `Request.ServerVariables` collection (qui contient des informations sur votre serveur web). Il utilise un `For Each` boucle pour afficher le nom de chaque élément en créant un nouveau `<li>` élément dans une liste à puces HTML.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

Le `For Each` mot clé est suivi par une variable qui représente un élément unique dans la collection (dans l’exemple, `myItem`), suivi par le `In` mot clé, suivie de la collection que vous souhaitez parcourir. Dans le corps de la `For Each` boucle, vous pouvez accéder à l’élément actuel à l’aide de la variable que vous avez déclaré précédemment.

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

Pour créer une boucle plus généraliste, utilisez la `Do While` instruction :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

Cette boucle commence par la `Do While` mot clé, suivie d’une condition, suivi par le bloc à répéter. Incrémentent généralement des boucles (ajouter à) ou de décrémentation (Soustraire) une variable ou un objet utilisé pour le comptage. Dans l’exemple, le `+=` opérateur ajoute 1 à la valeur d’une variable chaque fois que la boucle s’exécute. (Pour décrémenter une variable dans une boucle qui compte à rebours, vous utiliseriez l’opérateur de décrémentation `-=`.)

## <a name="objects-and-collections"></a>Objets et Collections

Presque tous les éléments dans un site Web ASP.NET sont un objet, y compris la page web elle-même. Cette section décrit certains objets importants que vous utiliserez fréquemment dans votre code.

### <a name="page-objects"></a>Objets de page

L’objet de base dans ASP.NET est la page. Vous pouvez accéder à propriétés de l’objet page directement sans aucun objet éligible. Le code suivant obtient le chemin d’accès du fichier de la page, à l’aide de la `Request` objet de la page :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

Vous pouvez utiliser les propriétés de la `Page` objet pour obtenir un grand nombre d’informations, telles que :

- `Request`. Comme vous l’avez déjà vu, il s’agit d’une collection d’informations sur la requête actuelle, y compris le type de navigateur fait la demande, l’URL de la page, de l’identité de l’utilisateur, etc.
- `Response`. Il s’agit d’une collection d’informations sur la réponse (page) qui sera envoyée au navigateur une fois le code du serveur en cours d’exécution. Par exemple, vous pouvez utiliser cette propriété pour écrire des informations dans la réponse.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a>Objets de collection (tableaux et les dictionnaires)

Une collection est un groupe d’objets du même type, telle qu’une collection de `Customer` objets à partir d’une base de données. ASP.NET contient de nombreux regroupements intégrés, tels que le `Request.Files` collection.

Souvent, vous allez travailler avec des données dans des collections. Deux types de collections courants sont le *tableau* et *dictionnaire*. Un tableau est utile lorsque vous souhaitez stocker une collection d’éléments similaires mais ne souhaitez pas créer une variable distincte pour contenir chaque élément :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

Les tableaux, vous déclarez un type de données spécifique, tel que `String`, `Integer`, ou `DateTime`. Pour indiquer que la variable peut contenir un tableau, vous ajoutez des parenthèses au nom de variable dans la déclaration (tel que `Dim myVar() As String`). Vous pouvez accéder à des éléments dans un tableau à l’aide de leur position (index) ou en utilisant la `For Each` instruction. Index de tableau sont de base zéro &#8212; autrement dit, le premier élément est en position 0, le deuxième élément est à la position 1 et ainsi de suite.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

Vous pouvez déterminer le nombre d’éléments dans un tableau en obtenant son `Length` propriété. Pour obtenir la position d’un élément spécifique dans le tableau (autrement dit, pour rechercher le tableau), utilisez le `Array.IndexOf` (méthode). Vous pouvez également effectuer les opérations comme inverse le contenu d’un tableau (la `Array.Reverse` méthode) ou trier le contenu (la `Array.Sort` méthode).

La sortie du code de tableau de chaîne affichée dans un navigateur :

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

Un dictionnaire est une collection de paires clé/valeur, où vous fournissez la clé (ou nom) pour définir ou récupérer la valeur correspondante :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

Pour créer un dictionnaire, vous utilisez le `New` mot clé pour indiquer que vous créez un nouveau `Dictionary` objet. Vous pouvez affecter un dictionnaire à une variable à l’aide du `Dim` mot clé. Vous indiquez les types de données des éléments dans le dictionnaire à l’aide de parenthèses ( `( )` ). À la fin de la déclaration, vous devez ajouter une autre paire de parenthèses, car il s’agit en fait une méthode qui crée un dictionnaire.

Pour ajouter des éléments au dictionnaire, vous pouvez appeler la `Add` méthode de la variable de dictionnaire (`myScores` dans ce cas), puis spécifiez une clé et une valeur. Ou bien, vous pouvez utiliser des parenthèses pour indiquer la clé et d’effectuer l’affectation simple, comme dans l’exemple suivant :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

Pour obtenir une valeur à partir du dictionnaire, vous spécifiez la clé entre parenthèses :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a>Appel de méthodes avec des paramètres

Comme vous l’avez vu précédemment dans cet article, les objets que vous programmez avec ont des méthodes. Par exemple, un `Database` objet peut avoir un `Database.Connect` (méthode). De nombreuses méthodes ont également un ou plusieurs paramètres. Un *paramètre* est une valeur que vous passez à une méthode pour activer la méthode effectuer sa tâche. Par exemple, examinez une déclaration pour le `Request.MapPath` (méthode), qui prend trois paramètres :

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

Cette méthode retourne le chemin d’accès physique sur le serveur qui correspond à un chemin d’accès virtuel spécifié. Les trois paramètres pour la méthode sont `virtualPath`, `baseVirtualDir`, et `allowCrossAppMapping`. (Notez que dans la déclaration, les paramètres sont répertoriés avec les types de données des données qu’ils accepte). Lorsque vous appelez cette méthode, vous devez fournir des valeurs pour tous les trois paramètres.

Lorsque vous utilisez Visual Basic avec la syntaxe Razor, vous avez deux options pour passer des paramètres à une méthode : *paramètres positionnels* ou *des paramètres nommés*. Pour appeler une méthode à l’aide des paramètres positionnels, vous transmettez les paramètres dans un ordre strict est spécifié dans la déclaration de méthode. (Vous généralement sauriez cet ordre en lisant la documentation de la méthode.) Vous devez suivre l’ordre, et vous ne pouvez pas ignorer les paramètres &#8212; si nécessaire, vous passez une chaîne vide (`""`) ou null pour que vous n’avez pas une valeur pour un paramètre de positionnement.

L’exemple suivant suppose que vous avez un dossier nommé *scripts* sur votre site Web. Le code appelle la `Request.MapPath` et transmet des valeurs pour les trois paramètres dans l’ordre approprié. Il affiche ensuite le chemin d’accès mappé qui en résulte.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

Lorsqu’il existe de nombreux paramètres pour une méthode, vous pouvez conserver votre code plus propre et plus lisible à l’aide des paramètres nommés. Pour appeler une méthode à l’aide de paramètres nommés, spécifiez le nom du paramètre suivi `:=` , puis indiquez la valeur. L’avantage des paramètres nommés est que vous pouvez les ajouter dans l’ordre que vous souhaitez. (Un inconvénient est que l’appel de méthode n’est pas aussi compact.)

L’exemple suivant appelle la méthode décrite ci-dessus, mais utilise des paramètres nommés pour fournir les valeurs :

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

Comme vous pouvez le voir, les paramètres sont passés dans un ordre différent. Toutefois, si vous exécutez l’exemple précédent et cet exemple, ils allons retourner la même valeur.

## <a name="handling-errors"></a>Gestion des erreurs

### <a name="try-catch-statements"></a>Instructions Try-Catch

Vous aurez souvent des instructions dans votre code peut échouer pour des raisons en dehors de votre contrôle. Exemple :

- Si votre code tente d’ouvrir, créer, lire ou écrire un fichier, que toutes sortes d’erreurs peuvent se produire. Le fichier n’existe ne peut-être pas, il est peut-être verrouillé, le code ne peut pas disposer des autorisations et ainsi de suite.
- De même, si votre code essaie de mettre à jour des enregistrements dans une base de données, il peut y avoir des problèmes d’autorisations, la connexion à la base de données peut-être être supprimée, les données à enregistrer est peut-être non valide et ainsi de suite.

En termes de programmation, ces situations sont appelées *exceptions*. Si votre code rencontre une exception, il génère (lève) un message d’erreur qui est, au mieux, ennuyeux pour les utilisateurs.

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

Dans les situations où votre code peut rencontrer des exceptions et afin d’éviter les messages d’erreur de ce type, vous pouvez utiliser `Try/Catch` instructions. Dans la `Try` instruction, vous exécutez le code que vous êtes en train de. Dans un ou plusieurs `Catch` instructions, vous pouvez rechercher propres erreurs (types d’exceptions spécifiques) qui se sont produites. Vous pouvez inclure autant `Catch` instructions que vous avez besoin rechercher des erreurs que vous êtes anticipation.

> [!NOTE]
> Nous vous recommandons d’éviter à l’aide de la `Response.Redirect` méthode dans `Try/Catch` instructions, car il peut provoquer une exception dans votre page.


L’exemple suivant montre une page qui crée un fichier texte à la première demande, puis affiche un bouton qui permet à l’utilisateur d’ouvrir le fichier. L’exemple utilise délibérément un nom de fichier incorrect afin qu’elle entraîne une exception. Le code inclut `Catch` instructions pour les deux exceptions possibles : `FileNotFoundException`, ce qui se produit si le nom de fichier est incorrect, et `DirectoryNotFoundException`, ce qui se produit si ASP.NET même Impossible de trouver le dossier. (Vous pouvez ne pas commenter une instruction dans l’exemple pour voir comment elle s’exécute lorsque tout fonctionne correctement.)

Si votre code n’a pas gérer l’exception, vous voyez une page d’erreur comme la capture d’écran précédente. Toutefois, le `Try/Catch` section vous aide à empêcher l’utilisateur de voir ces types d’erreurs.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a>Ressources supplémentaires

### <a name="reference-documentation"></a>Documentation de référence

- [ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)
- [Langage Visual Basic](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
