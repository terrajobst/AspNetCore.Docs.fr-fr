---
title: "Référence de la syntaxe Razor pour ASP.NET Core"
author: rick-anderson
description: "En savoir plus sur la syntaxe Razor balisage pour l’incorporation de code serveur dans les pages Web."
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: abdbb8112533d42f81180abad52f5ee86e3b280f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="razor-syntax-for-aspnet-core"></a>Syntaxe Razor pour ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), et [Dan Vicarel](https://github.com/Rabadash8820)

Razor est une syntaxe de balisage pour l’incorporation de code serveur dans les pages Web. La syntaxe Razor est constituée de basiles Razor, de c# et HTML. Les fichiers contenant Razor ont généralement *.cshtml* comme extension de fichier.

## <a name="rendering-html"></a>Rendu HTML

Le langage par défaut de Razor est le HTML. Le HTML généré à partir du balisage de Razor n’est pas différente du rendu HTML réalisé à partir d’un fichier HTML. Les balises HTML dans  les fichiers *.cshtml* Razor sont restituées par le serveur sans modification.

## <a name="razor-syntax"></a>Syntaxe Razor

Razor prend en charge de c# et utilise le `@` symbole pour effectuer la transition du code c# vers HTML. Razor évalue les expressions c# et les affiche dans la sortie HTML.

Lorsqu’un `@` symbole est suivi d’un [Razor de mot clé réservé](#razor-reserved-keywords),Razor interprete soit une balisage spécifique, soit comme du C#.

Pour utiliser le symbole `@` sans qu"il soit reconnu comme un symbole Razor, utiliser un deuxième `@` :

```cshtml
<p>@@Username</p>
```

Le code est rendu en HTML avec un seul `@` symbole :

```html
<p>@Username</p>
```

Les attributs et contenu HTML contenant les adresses de messagerie ne traitent pas les `@` symbole comme un caractère de la transition vers Razor. Dans l’exemple suivant, les adresses de messagerie sont conservées par Razor :

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a>Expressions implicites Razor

Les expressions implicites Razor commencent par `@` et sont suivies de code c# :

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

À l’exception de C# `await` (mot clé), les expressions implicites ne doivent pas contenir d'espace. Si l’instruction c# a une fin claire, des espaces peuvent être mélangés :

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

Les expressions implicites **ne peut pas** contenir des génériques c#: les caractères entre crochets (`<>`) sont interprétés comme une balise HTML. Le code suivant est, par exemple, **pas** valide :

```cshtml
<p>@GenericMethod<int>()</p>
```

Le code précédent génère une erreur de compilation semblable à un des éléments suivants :

 * L’élément « int » n’a pas été fermé. Tous les éléments doivent être à fermeture automatique ou concordent une balise de fin.
 *  Impossible de convertir le groupe de méthodes 'GenericMethod' au type 'object' non-délégué. Souhaitiez-vous appeler la méthode ? » 
 
Les appels de méthode générique doivent être encapsulées dans une [expression Razor explicite](#explicit-razor-expressions) ou un [bloc de code Razor](#razor-code-blocks).

## <a name="explicit-razor-expressions"></a>Expressions explicites Razor

Les expressions explicites Razor se composent d’un `@` symbole suivi d'une parenthèse ouvrante `(` et se terminent par une parenthèse fermante `)`. Pour afficher l’heure de la semaine dernière, le balisage de Razor suivant est utilisé :

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

Tout contenu dans le `@()` parenthèses est évaluée et rendu à la sortie.

En général les expressions implicites, décrites dans la section précédente, ne peut pas contenir d’espaces. Dans le code suivant, une semaine n’est pas soustraite de l’heure actuelle :

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

Le code restitue le code HTML suivant :

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

Les expressions explicites peuvent être utilisées pour concaténer du texte :

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

Sans l’expression explicite, `<p>Age@joe.Age</p>` est traité comme une adresse de messagerie, et `<p>Age@joe.Age</p>` est rendu. Lors de l’écriture en tant qu’expression explicite `<p>Age33</p>` est rendu.


Les expressions explicites peuvent être utilisées pour afficher la sortie à partir des méthodes génériques . Dans une expression implicite, les caractères entre crochets (`<>`) sont interprétés comme une balise HTML. Le balisage suivant est **pas** Razor valide :

```cshtml
<p>@GenericMethod<int>()</p>
```

Le code précédent génère une erreur du compilateur semblable à un des éléments suivants :

 * L’élément « int » n’a pas été fermé. Tous les éléments doivent être à fermeture automatique ou concordent une balise de fin.
 *  Impossible de convertir le groupe de méthodes 'GenericMethod' au type 'object' non-délégué. Souhaitiez-vous appeler la méthode ? » 
 
 Le balisage suivant illustre l’écriture de façon correcte ce code. Le code est écrit en tant qu’expression explicite :

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a>Encodage de l’expression

Les expressions c# qui correspondent à une chaîne sont encodées en HTML. Les expressions c# qui correspondent aux `IHtmlContent` sont rendus directement via `IHtmlContent.WriteTo`. Les expressions c# qui ne correspondent à `IHtmlContent` sont converties en une chaîne en `ToString` et codées avant leur rendus.

```cshtml
@("<span>Hello World</span>")
```

Le code restitue le code HTML suivant :

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

Le code HTML est indiqué dans le navigateur en tant que :

```
<span>Hello World</span>
```

`HtmlHelper.Raw`sortie n’est pas encodée mais sont rendue sous forme de balisage HTML.

> [!WARNING]
> L'utilisation de  `HtmlHelper.Raw` ou toute entrée utilisateur non vérifiée est un risque de sécurité. L’entrée d’utilisateur peut contenir du code JavaScript malveillant ou autres attaques. Il est difficile de totalement vérifier une entrée d’utilisateur. Évitez donc d’utiliser `HtmlHelper.Raw` à partir de données saisies par un utilisateur.

```cshtml
@Html.Raw("<span>Hello World</span>")
```

Le code restitue le code HTML suivant :

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a>Blocs de code Razor

Un bloc de code Razor commence par `@` et est placé entre `{}`. Contrairement aux expressions, le code c# à l’intérieur des blocs de code n’est pas rendu. Les blocs de code et des expressions dans une vue de partagent la même portée et sont définies dans l’ordre :

```cshtml
@{
    var quote = "The future depends on what you do today. - Mahatma Gandhi";
}

<p>@quote</p>

@{
    quote = "Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.";
}

<p>@quote</p>
```

Le code restitue le code HTML suivant :

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a>Transitions implicites

La langue par défaut dans un bloc de code est c#, mais la Page Razor peut effectuer la transition en HTML :

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a>Transition délimitée explicite

Pour définir une sous-section d’un bloc de code qui doit être rendu en HTML, entourer les caractères pour le rendu de Razor  **\<texte >** balise :

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

Utilisez cette approche pour le rendu HTML n’est pas entourée d’une autre balise HTML. Sans une balise HTML ou Razor, une erreur d’exécution Razor se produit.

Le  **\<texte >** balise est utile pour contrôler l’espace blanc lors du rendu du contenu :

* Seul le contenu entre le  **\<texte >** balise est affichée. 
* Aucun espace blanc avant ou après le  **\<texte >** étiquette s’affiche dans la sortie HTML.

### <a name="explicit-line-transition-with-"></a>Transition de ligne explicite avec @:

Pour rendre le reste d’une ligne entière à l’intérieur d’un bloc de code HTML, utilisez le `@:` syntaxe :

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

Sans le `@:` dans le code, une erreur d’exécution Razor est générée.

Avertissement : cette syntaxe dans un fichier Razor risque de provoquer des erreurs de compilation au niveau plus loin dans le bloc. Ces erreurs peuvent être difficiles à comprendre, car l’erreur se produit avant l’erreur signalée. Cette erreur est courante après la combinaison de plusieurs expressions implicite/explicite dans un bloc de code unique.

## <a name="control-structures"></a>Structures de contrôle

Les structures de contrôle sont une extension des blocs de code. Tous les aspects des blocs de code (transition au balisage, inline c#) également s’appliquent aux structures suivantes :

### <a name="conditionals-if-else-if-else-and-switch"></a>Instructions conditionnelles @if, sinon si, l’autre, et@switch

`@if`détermine à quel moment le code s’exécute :

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

`else`et `else if` ne nécessitent pas le `@` symbole :

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value is odd and small.</p>
}
```

Le balisage suivant montre comment utiliser une instruction switch :

```cshtml
@switch (value)
{
    case 1:
        <p>The value is 1!</p>
        break;
    case 1337:
        <p>Your number is 1337!</p>
        break;
    default:
        <p>Your number wasn't 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a>Bouclage @for, @foreach, @while, et @do tandis que

Le rendu HTML peut être réalisé à partir d'instructions de contrôle de boucle. Pour afficher une liste de personnes :

```cshtml
@{
    var people = new Person[]
    {
          new Person("Weston", 33),
          new Person("Johnathon", 41),
          ...
    };
}
```

Les instructions de boucles suivantes sont prises en charge :

`@for`

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@foreach`

```cshtml
@foreach (var person in people)
{
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@while`

```cshtml
@{ var i = 0; }
@while (i < people.Length)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
}
```

`@do while`

```cshtml
@{ var i = 0; }
@do
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
} while (i < people.Length);
```

### <a name="compound-using"></a>C@using

En c#, une instruction `using` est utilisée pour vérifier qu'un objet est bien libéré. Dans Razor, le même mécanisme est utilisé. Dans le code suivant, les balises Razor restituent un formulaire avec la `@using` instruction :


```cshtml
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

Les actions au niveau du formulaires peuvent être prise en charge par  [de l'aide au balisage](xref:mvc/views/tag-helpers/intro).

### <a name="try-catch-finally"></a>@try, catch, finally

La gestion des exceptions est similaire à c# :

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

Razor a la possibilité de protéger les sections critiques avec des instructions de verrouillage :

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a>Commentaires

Razor prend en charge les commentaires HTML et c# :

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

Le code restitue le code HTML suivant :

```html
<!-- HTML comment -->
```

Les commentaires dans les fichiers Razor sont supprimés par le serveur avant le rendu de la page Web. Razor utilise `@*  *@` pour délimiter les commentaires. Le code suivant est commenté pour le serveur n’effectue aucun rendu tout balisage :

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a>Directives

Les directives Razor sont représentées par des expressions implicites avec les éléments suivants de mots clés réservés du `@` symbole. Une directive modifie la manière dont une vue est analysée ou active des fonctionnalités différentes.

Comprendre comment Razor génère du code pour une vue rend plus facile à comprendre comment fonctionnent les directives.

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

Le code génère une classe semblable au suivant :

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Getting old ain't for wimps! - Anonymous";

        WriteLiteral("/r/n<div>Quote of the Day: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

Plus loin dans cet article, la section [affichage de la classe c# Razor générée pour une vue](#viewing-the-razor-c-class-generated-for-a-view) explique comment afficher cette classe générée.

### <a name="using"></a>@using

Le `@using` directive ajoute c# `using` directive à la vue générée :

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

Le `@model` directive spécifie le type du modèle passé à une vue :

```cshtml
@model TypeNameOfModel
```

Dans une application ASP.NET MVC de base créée avecle support de comptes d’utilisateur, le *Views/Account/Login.cshtml* affichage contient la déclaration de modèle suivante :

```cshtml
@model LoginViewModel
```

La classe générée hérite `RazorPage<dynamic>`:

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

Razor expose un `Model` propriété pour accéder au modèle passées à la vue :

```cshtml
<div>The Login Email: @Model.Email</div>
```

Le `@model` directive spécifie le type de cette propriété. La directive spécifie le `T` dans `RazorPage<T>` que généré classe que la vue dérive. Si le `@model` directive n’est pas spécifié, le `Model` propriété est de type `dynamic`. La valeur du modèle est passée à la vue à partir du contrôleur. Pour plus d’informations, consultez [Modèles fortement typéeset et @model](mot clé).

### <a name="inherits"></a>@inherits

La directive `@inherits` fournit un contrôle complet de la classe qui hérite de la vue :

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

Le code suivant est un type de page Razor personnalisé :

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

Le `CustomText` s’affiche dans une vue :

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

Le code restitue le code HTML suivant :

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 `@model`et `@inherits` peut être utilisé dans la même vue. `@inherits`peut être dans un *_ViewImports.cshtml* fichier importe de la vue :

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

Le code suivant est un exemple d’une vue fortement typée :

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

Si «rick@contoso.com» est passé dans le modèle, la vue génère le code HTML suivant :

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


La directive `@inject`  permet à la Page Razor injecter un service à partir du [conteneur de service](xref:fundamentals/dependency-injection). Pour plus d’informations, consultez la page [injection de dépendances dans les vues](xref:mvc/views/dependency-injection).

### <a name="functions"></a>@functions

La directive `@functions` dpermet à une Page Razor ajouter le contenu au niveau des fonctions à une vue :

```cshtml
@functions { // C# Code }
```

Exemple :

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

Le code génère le balisage HTML suivant :

```html
<div>From method: Hello</div>
```

Le code suivant est la classe générée Razor c# :

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

La directive `@section` dest utilisée conjointement avec la [disposition](xref:mvc/views/layout) pour activer des vues à restituer le contenu dans les différentes parties de la page HTML. Pour plus d’informations, consultez [Sections](xref:mvc/views/layout#layout-sections-label).

## <a name="tag-helpers"></a>Extensions de balises

Il existe trois directives qui se rapportent à [l'extension de balises](xref:mvc/views/tag-helpers/intro).

| Directive | Fonction |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | Rend les programmes d’assistance de balise disponibles à une vue. |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | Supprime les programmes d’assistance de balise précédemment ajoutés à partir d’une vue. |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | Spécifie un préfixe de balise pour activer la prise en charge de l’application d’assistance de balise et de rendre l’utilisation du programme d’assistance de balise explicite. |

## <a name="razor-reserved-keywords"></a>Mots clés de Razor réservé

### <a name="razor-keywords"></a>Mots clés de Razor

* page (nécessite d’ASP.NET core 2.0 ou versions ultérieures)
* fonctions
* inherits
* model
* section
* helper (non prises en charge par ASP.NET Core)

Mots clés de Razor sont précédés de `@(Razor Keyword)` (par exemple, `@(functions)`).

### <a name="c-razor-keywords"></a>Mots clés du langage c# Razor

* case
* do
* default
* for
* foreach
* if
* else
* lock
* switch
* try
* catch
* finally
* using
* while

Les mots clés du langage c# Razor doivent être échappées en double avec `@(@C# Razor Keyword)` (par exemple, `@(@case)`). Le premier `@` remplace l’analyseur Razor. La seconde `@` remplace l’analyseur c#.

### <a name="reserved-keywords-not-used-by-razor"></a>Mots clés réservés non utilisées par Razor

* namespace
* class

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a>Affichage de la classe c# Razor générée pour une vue

Ajoutez la classe suivante au projet ASP.NET MVC de base :

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

Remplacer la `RazorTemplateEngine` ajouté par MVC avec la `CustomTemplateEngine` classe :

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

Définir un point d’arrêt sur la `return csharpDocument` instruction de `CustomTemplateEngine`. Lorsque l’exécution du programme s’arrête au point d’arrêt, afficher la valeur de `generatedCode`.

![Vue de generatedCode visualiseur de texte](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a>Recherches de vue et de respect de la casse

Le moteur d’affichage Razor effectue des recherches respectant la casse pour les vues. Toutefois, la recherche réelle est déterminée par le système de fichiers sous-jacent :

* Source en fonction du fichier : 
  * Sur les systèmes d’exploitation avec les systèmes de fichiers insensibles à la casse (par exemple, Windows), les recherches de fournisseur de fichier physique sont insensible à la casse aussi. Par exemple, `return View("Test")` entraîne des correspondances pour */Views/Home/Test.cshtml*, */Views/home/test.cshtml*et n’importe quel autre variante de la casse.
  * Sur les systèmes de fichiers respectant la casse (par exemple, Linux, OS x et avec `EmbeddedFileProvider`), les recherches respectent la casse. Par exemple, `return View("Test")` spécifiquement les correspondances */Views/Home/Test.cshtml*.
* Vues précompilées : avec ASP.NET core 2.0 ou versions ultérieures, les recherches de vues précompilés sont insensibles à la casse  sur tous les systèmes d’exploitation. Le comportement est identique au comportement du fournisseur du fichier physique sous Windows. Si deux vues précompilés diffèrent uniquement par la casse, le résultat de recherche est non déterministe.

Les développeurs sont encouragés à correspondre à la casse des noms de fichiers et de répertoires à la casse de :

    * Noms de zone, le contrôleur et action. 
    * Pages Razor.
    
Le respect de la casse garantit que les déploiements retrouveront les vues, quelle que soit le système de fichiers sous-jacent.
