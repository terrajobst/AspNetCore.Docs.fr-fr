---
title: "Informations de référence sur la syntaxe Razor pour ASP.NET Core"
author: rick-anderson
description: "Apprenez à utiliser la syntaxe de balisage Razor pour incorporer du code serveur dans des pages web."
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/razor
ms.openlocfilehash: 98021cc76555f0c1402764c845471a4730b01b20
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="razor-syntax-for-aspnet-core"></a>Syntaxe Razor pour ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen) et [Dan Vicarel](https://github.com/Rabadash8820)

Razor est une syntaxe de balisage qui permet d’incorporer du code serveur dans des pages web. La syntaxe Razor est constituée de balises Razor, ainsi que de code C# et HTML. Les fichiers contenant de la syntaxe Razor ont généralement l’extension de fichier *.cshtml*.

## <a name="rendering-html"></a>Rendu HTML

Razor utilise par défaut le langage HTML. Le rendu HTML d’un balisage Razor n’est pas différent du rendu HTML d’un fichier en HTML. Le balisage HTML dans les fichiers Razor *.cshtml* est affiché tel quel par le serveur.

## <a name="razor-syntax"></a>Syntaxe Razor

Razor prend en charge le langage C# et utilise le symbole `@` pour convertir du code HTML en C#. Razor évalue les expressions C# et les affiche dans la sortie HTML.

Quand un symbole `@` est suivi d’un [mot clé réservé Razor](#razor-reserved-keywords), il est converti en balise Razor. Sinon, il est converti en code C# brut.

Pour mettre un symbole `@` en échappement dans le balisage Razor, ajoutez un deuxième symbole `@` :

```cshtml
<p>@@Username</p>
```

Le code est affiché en HTML avec un seul symbole `@` :

```html
<p>@Username</p>
```

Les attributs et le code HTML contenant des adresses e-mail ne traitent pas le symbole `@` comme un caractère de conversion. Dans l’exemple suivant, les adresses e-mail ne sont pas modifiées par l’analyse Razor :

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a>Expressions Razor implicites

Les expressions Razor implicites commencent par `@` suivi de code C# :

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

À l’exception du mot clé `await` C#, les expressions implicites ne doivent pas contenir d’espaces. Si l’instruction C# se termine de façon non ambigüe, il est possible d’insérer des espaces n’importe où dans la chaîne :

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

Les expressions implicites **ne doivent pas** contenir de caractères génériques C#, car les caractères entre crochets (`<>`) sont interprétés comme une balise HTML. Le code suivant n’est **pas** valide :

```cshtml
<p>@GenericMethod<int>()</p>
```

Le code précédent génère l’un des types d’erreur de compilateur suivants :

 * L’élément « int » n’a pas été fermé. Tous les éléments doivent se fermer automatiquement ou contenir une balise de fin correspondante.
 *  Impossible de convertir le groupe de méthodes 'GenericMethod' en type non-délégué 'object'. Souhaitiez-vous appeler la méthode ? 
 
Les appels de méthode générique doivent être inclus dans un wrapper dans une [expression Razor explicite](#explicit-razor-expressions) ou dans un [bloc de code Razor](#razor-code-blocks).

## <a name="explicit-razor-expressions"></a>Expressions Razor explicites

Les expressions Razor explicites se composent d’un symbole `@` suivi d’un contenu entre parenthèses. Pour afficher l’heure de la semaine précédente, utilisez le balisage Razor suivant :

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

Le contenu situé entre les parenthèses `@()` est évalué et affiché dans la sortie.

Les expressions implicites, décrites dans la section précédente, ne doivent généralement pas contenir d’espaces. Dans le code suivant, une semaine n’est pas déduite de l’heure actuelle :

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

Le code s’affiche en HTML de la façon suivante :

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

Les expressions explicites peuvent servir à concaténer du texte avec un résultat d’expression :

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

Sans l’expression explicite, `<p>Age@joe.Age</p>` est traité comme une adresse e-mail, et `<p>Age@joe.Age</p>` est affiché. Avec une expression explicite, `<p>Age33</p>` est affiché.


Les expressions explicites peuvent être utilisées pour afficher la sortie de méthodes génériques dans les fichiers *.cshtml*. Dans une expression implicite, les caractères placés entre crochets (`<>`) sont interprétés comme une balise HTML. Le balisage suivant n’est **pas** une syntaxe Razor valide :

```cshtml
<p>@GenericMethod<int>()</p>
```

Le code précédent génère l’un des types d’erreur de compilateur suivants :

 * L’élément « int » n’a pas été fermé. Tous les éléments doivent se fermer automatiquement ou contenir une balise de fin correspondante.
 *  Impossible de convertir le groupe de méthodes 'GenericMethod' en type non-délégué 'object'. Souhaitiez-vous appeler la méthode ? 
 
 Le balisage suivant montre comment écrire ce code correctement. Le code est écrit sous forme d’expression explicite :

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a>Encodage des expressions

Les expressions C# évaluées qui correspondent à une chaîne sont encodées en HTML. Les expressions C# évaluées qui correspondent à `IHtmlContent` sont affichées directement par `IHtmlContent.WriteTo`. Les expressions C# évaluées qui ne correspondent pas à `IHtmlContent` sont converties en chaîne par `ToString` et sont encodées avant d’être affichées.

```cshtml
@("<span>Hello World</span>")
```

Le code s’affiche en HTML de la façon suivante :

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

Le code HTML s’affiche dans le navigateur de cette façon :

```
<span>Hello World</span>
```

La sortie `HtmlHelper.Raw` n’est pas encodée, mais elle est affichée sous forme de balisage HTML.

> [!WARNING]
> Utiliser `HtmlHelper.Raw` sur des entrées utilisateur non nettoyées présente un risque pour la sécurité. Les entrées utilisateur peuvent contenir du code malveillant JavaScript ou d’un autre type. Le nettoyage des entrées utilisateur est difficile. C’est pourquoi il est préférable de ne pas utiliser `HtmlHelper.Raw` sur des entrées utilisateur.

```cshtml
@Html.Raw("<span>Hello World</span>")
```

Le code s’affiche en HTML de la façon suivante :

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a>Blocs de code Razor

Les blocs de code Razor commencent par `@` et sont délimités par deux `{}`. Contrairement aux expressions, le code C# figurant dans des blocs de code n’est pas affiché. Les blocs de code et les expressions dans une vue ont la même étendue et sont définis dans l’ordre :

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

Le code s’affiche en HTML de la façon suivante :

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a>Transitions implicites

Un bloc de code utilise le langage C# par défaut, mais la page Razor peut le reconvertir en HTML :

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a>Conversion délimitée explicite

Pour définir une sous-section d’un bloc de code qui doit s’afficher en HTML, placez les caractères à afficher dans la balise **\<text>** Razor suivante :

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

Utilisez cette approche pour afficher du code HTML qui n’est pas entouré d’une balise HTML. Sans la balise HTML ou Razor, une erreur d’exécution Razor se produit.

La balise **\<text>** est utile pour contrôler les espaces blancs dans le contenu affiché :

* Seul le contenu situé dans la balise **\<text>** est affiché. 
* Aucun espace blanc avant ou après la balise **\<text>** ne s’affiche dans la sortie HTML.

### <a name="explicit-line-transition-with-"></a>Conversion de ligne explicite avec @ :

Pour afficher le reste d’une ligne entière en HTML à l’intérieur d’un bloc de code, utilisez la syntaxe `@:` :

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

Sans la balise `@:` dans le code, une erreur d’exécution Razor se produit.

Avertissement : La présence de caractères `@` en trop dans un fichier Razor risque de provoquer des erreurs du compilateur au niveau des instructions suivantes dans le bloc. Ces erreurs du compilateur peuvent être difficiles à comprendre, car elles se produisent en réalité avant les erreurs signalées. Ce type d’erreur est courant après la combinaison de plusieurs expressions implicites ou explicites dans un même bloc de code.

## <a name="control-structures"></a>Structures de contrôle

Les structures de contrôle sont une extension des blocs de code. Toutes les caractéristiques des blocs de code (conversion de balisage, Inline C#) valent aussi pour les structures suivantes :

### <a name="conditionals-if-else-if-else-and-switch"></a>Conditions @if, else if, else et @switch

`@if` contrôle l’exécution du code :

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

`else` et `else if` ne nécessitent pas le symbole `@` :

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

Le balisage suivant montre comment utiliser une instruction switch :

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

### <a name="looping-for-foreach-while-and-do-while"></a>Boucles @for, @foreach, @while et @do while

Le code HTML basé sur un modèle peut être affiché avec des instructions de contrôle de boucle. Pour afficher une liste de personnes :

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

Les instructions de boucle suivantes sont prises en charge :

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

### <a name="compound-using"></a>Instruction composée @using

En C#, une instruction `using` est utilisée pour garantir la dispose d’un objet. Dans Razor, le même mécanisme permet de créer des HTML Helpers avec du contenu supplémentaire. Dans le code suivant, les HTML Helpers affichent une balise Form à l’aide de l’instruction `@using` :


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

Des actions au niveau de l’étendue peuvent être effectuées avec des [Tag Helpers](xref:mvc/views/tag-helpers/intro).

### <a name="try-catch-finally"></a>@try, catch, finally

La gestion des exceptions est similaire à C# :

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

Razor permet de verrouiller les sections critiques par des instructions lock :

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a>Commentaires

Razor prend en charge les commentaires HTML et C# :

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

Le code s’affiche en HTML de la façon suivante :

```html
<!-- HTML comment -->
```

Le serveur supprime les commentaires Razor avant d’afficher la page web. Razor délimite les commentaires avec `@*  *@`. Le code suivant est commenté pour indiquer au serveur de ne pas afficher le balisage :

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

Les directives Razor sont représentées par des expressions implicites constituées du symbole `@` suivi de mots clés réservés. En règle générale, une directive change la manière dont une vue est analysée ou active une fonctionnalité différente.

Pour mieux comprendre le fonctionnement des directives, vous devez bien comprendre comment Razor génère le code pour une vue.

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

Le code génère une classe semblable à celle-ci :

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

La section [Affichage de la classe C# Razor générée pour une vue](#viewing-the-razor-c-class-generated-for-a-view), plus loin dans cet article, explique comment afficher cette classe générée.

### <a name="using"></a>@using

La directive `@using` ajoute la directive `using` C# à la vue générée :

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

La directive `@model` spécifie le type du modèle passé à une vue :

```cshtml
@model TypeNameOfModel
```

Dans une application ASP.NET Core MVC créée avec des comptes d’utilisateur individuels, la vue *Views/Account/Login.cshtml* contient la déclaration de modèle suivante :

```cshtml
@model LoginViewModel
```

La classe générée hérite de `RazorPage<dynamic>` :

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

Razor expose une propriété `Model` pour accéder au modèle passé à la vue :

```cshtml
<div>The Login Email: @Model.Email</div>
```

La directive `@model` spécifie le type de cette propriété. La directive spécifie le type `T` dans `RazorPage<T>` pour la classe générée dont dérive la vue. Si la directive `@model` n’est pas spécifiée, la propriété `Model` est de type `dynamic`. La valeur du modèle est passée à la vue par le contrôleur. Pour plus d’informations, consultez l’article sur les modèles fortement typés et le mot clé @model.

### <a name="inherits"></a>@inherits

La directive `@inherits` fournit un contrôle complet de la classe héritée par la vue :

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

Le code suivant est un type de page Razor personnalisé :

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

Le `CustomText` s’affiche dans une vue :

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

Le code s’affiche en HTML de la façon suivante :

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 `@model` et `@inherits` peuvent s’utiliser dans la même vue. `@inherits` peut être dans un fichier *_ViewImports.cshtml* importé par la vue :

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

Le code suivant est un exemple de vue fortement typée :

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

Si « rick@contoso.com » est passé au modèle, la vue génère le balisage HTML suivant :

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


La directive `@inject` permet à la page Razor d’injecter un service dans une vue à partir du [conteneur de services](xref:fundamentals/dependency-injection). Pour plus d’informations, consultez [Injection de dépendances dans les vues](xref:mvc/views/dependency-injection).

### <a name="functions"></a>@functions

La directive `@functions` permet à une page Razor d’ajouter du contenu au niveau des fonctions dans une vue :

```cshtml
@functions { // C# Code }
```

Exemple :

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

Le code génère le balisage HTML suivant :

```html
<div>From method: Hello</div>
```

Le code suivant correspond à la classe C# Razor générée :

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

La directive `@section` est utilisée conjointement avec une [disposition](xref:mvc/views/layout) pour permettre aux vues d’afficher le contenu dans différentes parties de la page HTML. Pour plus d’informations, consultez [Sections](xref:mvc/views/layout#layout-sections-label).

## <a name="tag-helpers"></a>Tag Helpers

Il existe trois directives spécifiques aux [Tag Helpers](xref:mvc/views/tag-helpers/intro).

| Directive | Fonction |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | Rend les Tag Helpers disponibles dans une vue. |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | Supprime les Tag Helpers précédemment ajoutés à une vue. |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | Spécifie un préfixe de balise pour activer la prise en charge des Tag Helpers et rendre leur usage explicite. |

## <a name="razor-reserved-keywords"></a>Mots clés réservés Razor

### <a name="razor-keywords"></a>Mots clés Razor

* page (nécessite ASP.NET Core 2.0 ou version ultérieure)
* functions
* inherits
* model
* section
* helper (non pris en charge par ASP.NET Core)

Les mots clés Razor sont précédés d’une séquence d’échappement `@(Razor Keyword)` (par exemple, `@(functions)`).

### <a name="c-razor-keywords"></a>Mots clés Razor C#

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

Les mots clés Razor C# doivent être précédés d’une double séquence d’échappement `@(@C# Razor Keyword)` (par exemple, `@(@case)`). La première séquence d’échappement `@` est pour l’analyseur Razor. La seconde séquence d’échappement `@` est pour l’analyseur C#.

### <a name="reserved-keywords-not-used-by-razor"></a>Mots clés réservés non utilisés par Razor

* namespace
* class

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a>Affichage de la classe C# Razor générée pour une vue

Ajoutez la classe suivante au projet ASP.NET Core MVC :

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

Remplacez la classe `RazorTemplateEngine` ajoutée par MVC par la classe `CustomTemplateEngine` :

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

Définissez un point d’arrêt sur l’instruction `return csharpDocument` de `CustomTemplateEngine`. Quand l’exécution du programme s’arrête au point d’arrêt, affichez la valeur de `generatedCode`.

![Vue Visualiseur de texte du code généré](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a>Recherches de vues et respect de la casse

Le moteur de vue Razor effectue des recherches de vues en respectant la casse. Toutefois, la recherche réellement effectuée est déterminée par le système de fichiers sous-jacent :

* Source basé sur un fichier : 
  * Sur les systèmes d’exploitation avec des systèmes de fichiers qui ne respectent pas la casse (par exemple, Windows), les recherches de fournisseurs de fichiers physiques ne respectent pas la casse. Par exemple, `return View("Test")` trouve les correspondances */Views/Home/Test.cshtml*, */Views/home/test.cshtml* et toutes les autres variantes de casse.
  * Sur des systèmes de fichiers respectant la casse (par exemple, Linux, OSX, et avec `EmbeddedFileProvider`), les recherches respectent la casse. Par exemple, `return View("Test")` trouve uniquement la correspondance */Views/Home/Test.cshtml*.
* Vues précompilées : Avec ASP.NET Core 2.0 et les versions ultérieures, les recherches de vues précompilées ne respectent pas la casse, quels que soient les systèmes d’exploitation. Le comportement est le même que celui du fournisseur de fichiers physiques sur Windows. Si deux vues précompilées diffèrent seulement par leur casse, le résultat de la recherche est non déterministe.

Les développeurs doivent s’efforcer d’utiliser la même casse pour les noms de fichiers et de répertoires que pour les noms des éléments suivants :

    * Zone, contrôleur et action 
    * Pages Razor
    
L’utilisation d’une casse identique garantit que les déploiements trouvent toujours les vues associées, indépendamment du système de fichiers sous-jacent.
