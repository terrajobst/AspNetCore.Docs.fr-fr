---
title: Empêcher les scripts entre sites (XSS) dans ASP.NET Core
author: rick-anderson
description: Découvrez les scripts inter-sites (XSS) et les techniques permettant de résoudre cette vulnérabilité dans une application ASP.NET Core.
ms.author: riande
ms.date: 10/02/2018
uid: security/cross-site-scripting
ms.openlocfilehash: 1d6f605dc336d8768b8a47e4995f119d198a61af
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667978"
---
# <a name="prevent-cross-site-scripting-xss-in-aspnet-core"></a>Empêcher les scripts entre sites (XSS) dans ASP.NET Core

De [Rick Anderson](https://twitter.com/RickAndMSFT)

L'écriture de scripts entre sites (XSS) est une faille de sécurité qui permet à une personne malveillante de placer les scripts côté client (généralement JavaScript) dans les pages web. Quand d’autres utilisateurs chargent des pages affectées, les scripts de l’attaquant s’exécutent, ce qui permet à l’attaquant de voler des cookies et des jetons de session, de modifier le contenu de la page Web par le biais de la manipulation DOM ou de rediriger le navigateur vers une autre page. Les vulnérabilités XSS se produisent généralement quand une application prend une entrée d’utilisateur et la renvoie à une page sans la valider, l’encoder ou la placer dans une séquence d’échappement.

## <a name="protecting-your-application-against-xss"></a>Protection de votre application contre XSS

À un niveau de base, XSS fonctionne en détrucnt votre application en insérant une balise `<script>` dans votre page rendue, ou en insérant un événement `On*` dans un élément. Les développeurs doivent utiliser les étapes de prévention suivantes pour éviter d’introduire des failles XSS dans leur application.

1. Ne placez jamais des données non fiables dans votre entrée HTML, sauf si vous suivez le reste des étapes ci-dessous. Les données non approuvées sont toutes les données qui peuvent être contrôlées par une personne malveillante, des entrées de formulaire HTML, chaînes de requête, les en-têtes HTTP, même les données provenant d’une base de données, car une personne malveillante peut être en mesure d'attaquer votre base de données même si elle ne cause pas des failles dans votre application.

2. Avant de placer des données non fiables à l’intérieur d’un élément HTML, assurez-vous qu’elles sont encodées en HTML. L’encodage HTML prend des caractères tels que &lt; et les modifie dans un format sécurisé comme &amp;lt ;

3. Avant de placer des données non fiables dans un attribut HTML, assurez-vous qu’elles sont encodées en HTML. L'Encodage d’attribut HTML est un sur-ensemble de l’encodage HTML et encode les caractères supplémentaires tels que « et ».

4. Avant de mettre des données non fiables en JavaScript placez les données dans un élément HTML dont le contenu sera récupéré lors de son exécution. Si ce n’est pas possible, assurez-vous que les données sont encodées en JavaScript. L’encodage JavaScript prend des caractères dangereux pour JavaScript et les remplace par leur Hex, par exemple &lt; est encodé comme `\u003C`.

5. Avant de placer des données non fiables dans une chaîne de requête URL assurez-vous qu'elles soient encodées URL.

## <a name="html-encoding-using-razor"></a>Encodage HTML à l’aide de Razor

Le moteur Razor automatiquement utilisé dans MVC encode toute sortie provenant de variables, sauf si vous travaillez dur pour empêcher cette opération. Elle utilise des règles d’encodage d’attribut HTML chaque fois que vous utilisez la directive *@* . L'encodage d’attribut HTML est un sur-ensemble de l'encodage HTML et cela signifie que vous n’êtes pas obligé de vous préoccuper si vous devez utiliser l’encodage HTML ou l'encodage d’attribut HTML. Vous devez vous assurer que vous utilisez @ uniquement dans un contexte HTML, et non pour tenter d’insérer des entrées non approuvées directement dans JavaScript. Les programmes d’assistance de balises encoderont également m’entrée que vous utilisez dans les paramètres de la balise.

Prenez la vue Razor suivante :

```cshtml
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

Cette vue renvoie le contenu de la variable *untrustedInput* . Cette variable comprend certains caractères utilisés dans les attaques XSS, à savoir &lt;» et &gt;. L'examen de la source présente la sortie rendue encodée sous la forme :

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> ASP.NET Core MVC fournit une classe `HtmlString` qui n’est pas automatiquement encodée au moment de la sortie. Cela ne doit jamais être utilisé en association avec une entrée non approuvée car cela pourrait exposer une vulnérabilité XSS.

## <a name="javascript-encoding-using-razor"></a>Encodage JavaScript à l’aide de Razor

Il peut arriver que vous souhaitiez insérer une valeur JavaScript à traiter dans votre affichage. Il existe deux façons d'effectuer cette opération. Le moyen le plus sûr d’insérer des valeurs consiste à placer la valeur dans un attribut de données d’une balise et à la récupérer dans votre code JavaScript. Par exemple :

```cshtml
@{
       var untrustedInput = "<\"123\">";
   }

   <div
       id="injectedData"
       data-untrustedinput="@untrustedInput" />

   <script>
     var injectedData = document.getElementById("injectedData");

     // All clients
     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     // HTML 5 clients only
     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

Le code HTML suivant est généré

```html
<div
     id="injectedData"
     data-untrustedinput="&lt;&quot;123&quot;&gt;" />

   <script>
     var injectedData = document.getElementById("injectedData");

     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

Qui, lorsqu’il s’exécute, affiche ce qui suit :

```
<"123">
   <"123">
```

Vous pouvez également appeler l’encodeur JavaScript directement :

```cshtml
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
```

Cela s’affiche dans le navigateur comme suit :

```html
<script>
    document.write("\u003C\u0022123\u0022\u003E");
</script>
```

>[!WARNING]
> Ne concaténez pas d'entrées non fiables dans le code JavaScript pour créer des éléments DOM. Vous devez utiliser `createElement()` et assigner des valeurs de propriété de manière appropriée, comme `node.TextContent=`, ou utiliser `element.SetAttribute()`/`element[attribute]=` dans le cas contraire, vous vous exposez à un XSS de type DOM.

## <a name="accessing-encoders-in-code"></a>Accès aux encodeurs dans le code

Les encodeurs HTML, JavaScript et d’URL sont disponibles pour votre code de deux manières, vous pouvez les injecter via l' [injection de dépendances](xref:fundamentals/dependency-injection) ou vous pouvez utiliser les encodeurs par défaut contenus dans l’espace de noms `System.Text.Encodings.Web`. Si vous utilisez les encodeurs par défaut, alors les encodeurs que vous avez appliqués aux plages de caractères considérés comme sécurisées ne prendront pas effet - les encodeurs par défaut utilisent les règles d'encodage les plus sûres possible.

Pour utiliser les encodeurs configurables via l’injection de code, les constructeurs doivent accepter un paramètre *HtmlEncoder*, *JavaScriptEncoder* et *UrlEncoder* , selon le cas. Par exemple :

```csharp
public class HomeController : Controller
   {
       HtmlEncoder _htmlEncoder;
       JavaScriptEncoder _javaScriptEncoder;
       UrlEncoder _urlEncoder;

       public HomeController(HtmlEncoder htmlEncoder,
                             JavaScriptEncoder javascriptEncoder,
                             UrlEncoder urlEncoder)
       {
           _htmlEncoder = htmlEncoder;
           _javaScriptEncoder = javascriptEncoder;
           _urlEncoder = urlEncoder;
       }
   }
   ```

## <a name="encoding-url-parameters"></a>Encodage des paramètres d’URL

Si vous souhaitez créer une chaîne de requête d’URL avec une entrée non approuvée comme valeur, utilisez l' `UrlEncoder` pour encoder la valeur. Par exemple,

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

Après l’encodage, la variable encodedValue contient `%22Quoted%20Value%20with%20spaces%20and%20%26%22`. Des espaces, des guillemets, des signes de ponctuation et autres caractères non sécurisés sont encodés en pourcentage dans leur valeur hexadécimale, par exemple, un caractère d’espace deviendra %20.

>[!WARNING]
> N’utilisez pas d’entrée non approuvée dans le cadre d’un chemin d’accès URL. Transmettez toujours les entrée non fiables en tant que valeurs de chaîne de requête.

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a>Personnalisation des encodeurs

Par défaut les encodeurs utilisent une liste sécurisée, limitée à la plage de base Unicode Latin, et encodent tous les caractères en dehors de cette plage selon leurs équivalents de code de caractère. Ce comportement affecte également le rendu Razor TagHelper et HtmlHelper car il utilise les encodeurs pour vos chaînes de sortie.

Le raisonnement sous-jacent vise à vous protéger des bogues de navigateur inconnus ou ultérieurs (les bogues de navigateur antérieurs ont gêné l’analyse basée sur le traitement de caractères non anglais). Si votre site web utilise de façon intensive les caractères non latins, comme le chinois, le cyrillique, ou autre, cela n'est probablement pas le comportement que vous souhaitez.

Vous pouvez personnaliser les listes sécurisées de l’encodeur pour inclure les plages Unicode appropriées à votre application au démarrage, dans `ConfigureServices()`.

Par exemple, à l’aide de la configuration par défaut, vous pouvez utiliser un HtmlHelper Razor comme ;

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

Lorsque vous affichez la source de la page web vous verrez quelle a été restituée comme suit, avec le texte chinois encodé ;

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

Pour élargir les caractères traités comme sécurisés par l’encodeur, insérez la ligne suivante dans la méthode `ConfigureServices()` dans `startup.cs`;

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

Cet exemple étend la liste sécurisée pour inclure la plage Unicode CjkUnifiedIdeographs. La sortie rendue deviendrait alors

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

Les plages de liste sécurisée sont spécifiées sous la forme de graphiques de code Unicode, et non de langues. La [norme Unicode](https://unicode.org/) contient une liste de [graphiques de code](https://www.unicode.org/charts/index.html) que vous pouvez utiliser pour rechercher le graphique contenant vos caractères. Chaque encodeur, html, JavaScript et URL doivent être configurés séparément.

> [!NOTE]
> La personnalisation de la liste n’affecte que les encodeurs provenant de DI. Si vous accédez directement à un encodeur via `System.Text.Encodings.Web.*Encoder.Default`, la valeur par défaut, latin de base uniquement, est utilisée.

## <a name="where-should-encoding-take-place"></a>Où l'encodage doit s'effectuer ?

Les pratiques générales acceptées est que l'encodage intervienne au point de rendu et les valeurs encodées ne doivent jamais être stockées dans une base de données. L'encodage dans la phase de rendu vous permet de modifier l’utilisation des données, par exemple, à partir de HTML à une valeur de chaîne de requête. Aussi, il vous permet de rechercher facilement vos données sans avoir à encoder les valeurs avant de les rechercher et vous permet de tirer parti des modifications ou des correctifs de bogues apportés aux encodeurs.

## <a name="validation-as-an-xss-prevention-technique"></a>Validation en tant que technique de prévention XSS

La validation peut être un outil utile dans limitation des attaques XSS. Par exemple, une chaîne numérique contenant uniquement les caractères 0-9 ne déclenchera pas d’attaque XSS. La validation devient plus complexe lors de l’acceptation du code HTML dans l’entrée utilisateur. L’analyse de l’entrée HTML est difficile, voire impossible. La démarque, couplée avec un analyseur qui supprime le code HTML incorporé, est une option plus sûre pour accepter une entrée enrichie. Ne vous fiez jamais à la validation. Encodez toujours les entrées non approuvées avant la sortie, quelle que soit la validation ou l’assainissement effectué.
