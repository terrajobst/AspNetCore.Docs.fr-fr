---
title: Empêcher les scripts entre sites (XSS) dans ASP.NET Core
author: rick-anderson
description: Découvrez les scripts inter-sites (XSS) et les techniques permettant de résoudre cette vulnérabilité dans une application ASP.NET Core.
ms.author: riande
ms.date: 10/02/2018
uid: security/cross-site-scripting
ms.openlocfilehash: 1d6f605dc336d8768b8a47e4995f119d198a61af
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172632"
---
# <a name="prevent-cross-site-scripting-xss-in-aspnet-core"></a><span data-ttu-id="ab371-103">Empêcher les scripts entre sites (XSS) dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ab371-103">Prevent Cross-Site Scripting (XSS) in ASP.NET Core</span></span>

<span data-ttu-id="ab371-104">De [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ab371-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ab371-105">L'écriture de scripts entre sites (XSS) est une faille de sécurité qui permet à une personne malveillante de placer les scripts côté client (généralement JavaScript) dans les pages web.</span><span class="sxs-lookup"><span data-stu-id="ab371-105">Cross-Site Scripting (XSS) is a security vulnerability which enables an attacker to place client side scripts (usually JavaScript) into web pages.</span></span> <span data-ttu-id="ab371-106">Quand d’autres utilisateurs chargent des pages affectées, les scripts de l’attaquant s’exécutent, ce qui permet à l’attaquant de voler des cookies et des jetons de session, de modifier le contenu de la page Web par le biais de la manipulation DOM ou de rediriger le navigateur vers une autre page.</span><span class="sxs-lookup"><span data-stu-id="ab371-106">When other users load affected pages the attacker's scripts will run, enabling the attacker to steal cookies and session tokens, change the contents of the web page through DOM manipulation or redirect the browser to another page.</span></span> <span data-ttu-id="ab371-107">Les vulnérabilités XSS se produisent généralement quand une application prend une entrée d’utilisateur et la renvoie à une page sans la valider, l’encoder ou la placer dans une séquence d’échappement.</span><span class="sxs-lookup"><span data-stu-id="ab371-107">XSS vulnerabilities generally occur when an application takes user input and outputs it to a page without validating, encoding or escaping it.</span></span>

## <a name="protecting-your-application-against-xss"></a><span data-ttu-id="ab371-108">Protection de votre application contre XSS</span><span class="sxs-lookup"><span data-stu-id="ab371-108">Protecting your application against XSS</span></span>

<span data-ttu-id="ab371-109">À un niveau de base, XSS fonctionne en détrucnt votre application en insérant une balise `<script>` dans votre page rendue, ou en insérant un événement `On*` dans un élément.</span><span class="sxs-lookup"><span data-stu-id="ab371-109">At a basic level XSS works by tricking your application into inserting a `<script>` tag into your rendered page, or by inserting an `On*` event into an element.</span></span> <span data-ttu-id="ab371-110">Les développeurs doivent utiliser les étapes de prévention suivantes pour éviter d’introduire des failles XSS dans leur application.</span><span class="sxs-lookup"><span data-stu-id="ab371-110">Developers should use the following prevention steps to avoid introducing XSS into their application.</span></span>

1. <span data-ttu-id="ab371-111">Ne placez jamais des données non fiables dans votre entrée HTML, sauf si vous suivez le reste des étapes ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="ab371-111">Never put untrusted data into your HTML input, unless you follow the rest of the steps below.</span></span> <span data-ttu-id="ab371-112">Les données non approuvées sont toutes les données qui peuvent être contrôlées par une personne malveillante, des entrées de formulaire HTML, chaînes de requête, les en-têtes HTTP, même les données provenant d’une base de données, car une personne malveillante peut être en mesure d'attaquer votre base de données même si elle ne cause pas des failles dans votre application.</span><span class="sxs-lookup"><span data-stu-id="ab371-112">Untrusted data is any data that may be controlled by an attacker, HTML form inputs, query strings, HTTP headers, even data sourced from a database as an attacker may be able to breach your database even if they cannot breach your application.</span></span>

2. <span data-ttu-id="ab371-113">Avant de placer des données non fiables à l’intérieur d’un élément HTML, assurez-vous qu’elles sont encodées en HTML.</span><span class="sxs-lookup"><span data-stu-id="ab371-113">Before putting untrusted data inside an HTML element ensure it's HTML encoded.</span></span> <span data-ttu-id="ab371-114">L’encodage HTML prend des caractères tels que &lt; et les modifie dans un format sécurisé comme &amp;lt ;</span><span class="sxs-lookup"><span data-stu-id="ab371-114">HTML encoding takes characters such as &lt; and changes them into a safe form like &amp;lt;</span></span>

3. <span data-ttu-id="ab371-115">Avant de placer des données non fiables dans un attribut HTML, assurez-vous qu’elles sont encodées en HTML.</span><span class="sxs-lookup"><span data-stu-id="ab371-115">Before putting untrusted data into an HTML attribute ensure it's HTML encoded.</span></span> <span data-ttu-id="ab371-116">L'Encodage d’attribut HTML est un sur-ensemble de l’encodage HTML et encode les caractères supplémentaires tels que « et ».</span><span class="sxs-lookup"><span data-stu-id="ab371-116">HTML attribute encoding is a superset of HTML encoding and encodes additional characters such as " and '.</span></span>

4. <span data-ttu-id="ab371-117">Avant de mettre des données non fiables en JavaScript placez les données dans un élément HTML dont le contenu sera récupéré lors de son exécution.</span><span class="sxs-lookup"><span data-stu-id="ab371-117">Before putting untrusted data into JavaScript place the data in an HTML element whose contents you retrieve at runtime.</span></span> <span data-ttu-id="ab371-118">Si ce n’est pas possible, assurez-vous que les données sont encodées en JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ab371-118">If this isn't possible, then ensure the data is JavaScript encoded.</span></span> <span data-ttu-id="ab371-119">L’encodage JavaScript prend des caractères dangereux pour JavaScript et les remplace par leur Hex, par exemple &lt; est encodé comme `\u003C`.</span><span class="sxs-lookup"><span data-stu-id="ab371-119">JavaScript encoding takes dangerous characters for JavaScript and replaces them with their hex, for example &lt; would be encoded as `\u003C`.</span></span>

5. <span data-ttu-id="ab371-120">Avant de placer des données non fiables dans une chaîne de requête URL assurez-vous qu'elles soient encodées URL.</span><span class="sxs-lookup"><span data-stu-id="ab371-120">Before putting untrusted data into a URL query string ensure it's URL encoded.</span></span>

## <a name="html-encoding-using-razor"></a><span data-ttu-id="ab371-121">Encodage HTML à l’aide de Razor</span><span class="sxs-lookup"><span data-stu-id="ab371-121">HTML Encoding using Razor</span></span>

<span data-ttu-id="ab371-122">Le moteur Razor automatiquement utilisé dans MVC encode toute sortie provenant de variables, sauf si vous travaillez dur pour empêcher cette opération.</span><span class="sxs-lookup"><span data-stu-id="ab371-122">The Razor engine used in MVC automatically encodes all output sourced from variables, unless you work really hard to prevent it doing so.</span></span> <span data-ttu-id="ab371-123">Elle utilise des règles d’encodage d’attribut HTML chaque fois que vous utilisez la directive *@* .</span><span class="sxs-lookup"><span data-stu-id="ab371-123">It uses HTML attribute encoding rules whenever you use the *@* directive.</span></span> <span data-ttu-id="ab371-124">L'encodage d’attribut HTML est un sur-ensemble de l'encodage HTML et cela signifie que vous n’êtes pas obligé de vous préoccuper si vous devez utiliser l’encodage HTML ou l'encodage d’attribut HTML.</span><span class="sxs-lookup"><span data-stu-id="ab371-124">As HTML attribute encoding is a superset of HTML encoding this means you don't have to concern yourself with whether you should use HTML encoding or HTML attribute encoding.</span></span> <span data-ttu-id="ab371-125">Vous devez vous assurer que vous utilisez @ uniquement dans un contexte HTML, et non pour tenter d’insérer des entrées non approuvées directement dans JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ab371-125">You must ensure that you only use @ in an HTML context, not when attempting to insert untrusted input directly into JavaScript.</span></span> <span data-ttu-id="ab371-126">Les programmes d’assistance de balises encoderont également m’entrée que vous utilisez dans les paramètres de la balise.</span><span class="sxs-lookup"><span data-stu-id="ab371-126">Tag helpers will also encode input you use in tag parameters.</span></span>

<span data-ttu-id="ab371-127">Prenez la vue Razor suivante :</span><span class="sxs-lookup"><span data-stu-id="ab371-127">Take the following Razor view:</span></span>

```cshtml
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

<span data-ttu-id="ab371-128">Cette vue renvoie le contenu de la variable *untrustedInput* .</span><span class="sxs-lookup"><span data-stu-id="ab371-128">This view outputs the contents of the *untrustedInput* variable.</span></span> <span data-ttu-id="ab371-129">Cette variable comprend certains caractères utilisés dans les attaques XSS, à savoir &lt;» et &gt;.</span><span class="sxs-lookup"><span data-stu-id="ab371-129">This variable includes some characters which are used in XSS attacks, namely &lt;, " and &gt;.</span></span> <span data-ttu-id="ab371-130">L'examen de la source présente la sortie rendue encodée sous la forme :</span><span class="sxs-lookup"><span data-stu-id="ab371-130">Examining the source shows the rendered output encoded as:</span></span>

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> <span data-ttu-id="ab371-131">ASP.NET Core MVC fournit une classe `HtmlString` qui n’est pas automatiquement encodée au moment de la sortie.</span><span class="sxs-lookup"><span data-stu-id="ab371-131">ASP.NET Core MVC provides an `HtmlString` class which isn't automatically encoded upon output.</span></span> <span data-ttu-id="ab371-132">Cela ne doit jamais être utilisé en association avec une entrée non approuvée car cela pourrait exposer une vulnérabilité XSS.</span><span class="sxs-lookup"><span data-stu-id="ab371-132">This should never be used in combination with untrusted input as this will expose an XSS vulnerability.</span></span>

## <a name="javascript-encoding-using-razor"></a><span data-ttu-id="ab371-133">Encodage JavaScript à l’aide de Razor</span><span class="sxs-lookup"><span data-stu-id="ab371-133">JavaScript Encoding using Razor</span></span>

<span data-ttu-id="ab371-134">Il peut arriver que vous souhaitiez insérer une valeur JavaScript à traiter dans votre affichage.</span><span class="sxs-lookup"><span data-stu-id="ab371-134">There may be times you want to insert a value into JavaScript to process in your view.</span></span> <span data-ttu-id="ab371-135">Il existe deux façons d'effectuer cette opération.</span><span class="sxs-lookup"><span data-stu-id="ab371-135">There are two ways to do this.</span></span> <span data-ttu-id="ab371-136">Le moyen le plus sûr d’insérer des valeurs consiste à placer la valeur dans un attribut de données d’une balise et à la récupérer dans votre code JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ab371-136">The safest way to insert values is to place the value in a data attribute of a tag and retrieve it in your JavaScript.</span></span> <span data-ttu-id="ab371-137">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="ab371-137">For example:</span></span>

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

<span data-ttu-id="ab371-138">Le code HTML suivant est généré</span><span class="sxs-lookup"><span data-stu-id="ab371-138">This will produce the following HTML</span></span>

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

<span data-ttu-id="ab371-139">Qui, lorsqu’il s’exécute, affiche ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="ab371-139">Which, when it runs, will render the following:</span></span>

```
<"123">
   <"123">
```

<span data-ttu-id="ab371-140">Vous pouvez également appeler l’encodeur JavaScript directement :</span><span class="sxs-lookup"><span data-stu-id="ab371-140">You can also call the JavaScript encoder directly:</span></span>

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

<span data-ttu-id="ab371-141">Cela s’affiche dans le navigateur comme suit :</span><span class="sxs-lookup"><span data-stu-id="ab371-141">This will render in the browser as follows:</span></span>

```html
<script>
    document.write("\u003C\u0022123\u0022\u003E");
</script>
```

>[!WARNING]
> <span data-ttu-id="ab371-142">Ne concaténez pas d'entrées non fiables dans le code JavaScript pour créer des éléments DOM.</span><span class="sxs-lookup"><span data-stu-id="ab371-142">Don't concatenate untrusted input in JavaScript to create DOM elements.</span></span> <span data-ttu-id="ab371-143">Vous devez utiliser `createElement()` et assigner des valeurs de propriété de manière appropriée, comme `node.TextContent=`, ou utiliser `element.SetAttribute()`/`element[attribute]=` dans le cas contraire, vous vous exposez à un XSS de type DOM.</span><span class="sxs-lookup"><span data-stu-id="ab371-143">You should use `createElement()` and assign property values appropriately such as `node.TextContent=`, or use `element.SetAttribute()`/`element[attribute]=` otherwise you expose yourself to DOM-based XSS.</span></span>

## <a name="accessing-encoders-in-code"></a><span data-ttu-id="ab371-144">Accès aux encodeurs dans le code</span><span class="sxs-lookup"><span data-stu-id="ab371-144">Accessing encoders in code</span></span>

<span data-ttu-id="ab371-145">Les encodeurs HTML, JavaScript et d’URL sont disponibles pour votre code de deux manières, vous pouvez les injecter via l' [injection de dépendances](xref:fundamentals/dependency-injection) ou vous pouvez utiliser les encodeurs par défaut contenus dans l’espace de noms `System.Text.Encodings.Web`.</span><span class="sxs-lookup"><span data-stu-id="ab371-145">The HTML, JavaScript and URL encoders are available to your code in two ways, you can inject them via [dependency injection](xref:fundamentals/dependency-injection) or you can use the default encoders contained in the `System.Text.Encodings.Web` namespace.</span></span> <span data-ttu-id="ab371-146">Si vous utilisez les encodeurs par défaut, alors les encodeurs que vous avez appliqués aux plages de caractères considérés comme sécurisées ne prendront pas effet - les encodeurs par défaut utilisent les règles d'encodage les plus sûres possible.</span><span class="sxs-lookup"><span data-stu-id="ab371-146">If you use the default encoders then any  you applied to character ranges to be treated as safe won't take effect - the default encoders use the safest encoding rules possible.</span></span>

<span data-ttu-id="ab371-147">Pour utiliser les encodeurs configurables via l’injection de code, les constructeurs doivent accepter un paramètre *HtmlEncoder*, *JavaScriptEncoder* et *UrlEncoder* , selon le cas.</span><span class="sxs-lookup"><span data-stu-id="ab371-147">To use the configurable encoders via DI your constructors should take an *HtmlEncoder*, *JavaScriptEncoder* and *UrlEncoder* parameter as appropriate.</span></span> <span data-ttu-id="ab371-148">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="ab371-148">For example;</span></span>

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

## <a name="encoding-url-parameters"></a><span data-ttu-id="ab371-149">Encodage des paramètres d’URL</span><span class="sxs-lookup"><span data-stu-id="ab371-149">Encoding URL Parameters</span></span>

<span data-ttu-id="ab371-150">Si vous souhaitez créer une chaîne de requête d’URL avec une entrée non approuvée comme valeur, utilisez l' `UrlEncoder` pour encoder la valeur.</span><span class="sxs-lookup"><span data-stu-id="ab371-150">If you want to build a URL query string with untrusted input as a value use the `UrlEncoder` to encode the value.</span></span> <span data-ttu-id="ab371-151">Par exemple,</span><span class="sxs-lookup"><span data-stu-id="ab371-151">For example,</span></span>

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

<span data-ttu-id="ab371-152">Après l’encodage, la variable encodedValue contient `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span><span class="sxs-lookup"><span data-stu-id="ab371-152">After encoding the encodedValue variable will contain `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span></span> <span data-ttu-id="ab371-153">Des espaces, des guillemets, des signes de ponctuation et autres caractères non sécurisés sont encodés en pourcentage dans leur valeur hexadécimale, par exemple, un caractère d’espace deviendra %20.</span><span class="sxs-lookup"><span data-stu-id="ab371-153">Spaces, quotes, punctuation and other unsafe characters will be percent encoded to their hexadecimal value, for example a space character will become %20.</span></span>

>[!WARNING]
> <span data-ttu-id="ab371-154">N’utilisez pas d’entrée non approuvée dans le cadre d’un chemin d’accès URL.</span><span class="sxs-lookup"><span data-stu-id="ab371-154">Don't use untrusted input as part of a URL path.</span></span> <span data-ttu-id="ab371-155">Transmettez toujours les entrée non fiables en tant que valeurs de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="ab371-155">Always pass untrusted input as a query string value.</span></span>

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a><span data-ttu-id="ab371-156">Personnalisation des encodeurs</span><span class="sxs-lookup"><span data-stu-id="ab371-156">Customizing the Encoders</span></span>

<span data-ttu-id="ab371-157">Par défaut les encodeurs utilisent une liste sécurisée, limitée à la plage de base Unicode Latin, et encodent tous les caractères en dehors de cette plage selon leurs équivalents de code de caractère.</span><span class="sxs-lookup"><span data-stu-id="ab371-157">By default encoders use a safe list limited to the Basic Latin Unicode range and encode all characters outside of that range as their character code equivalents.</span></span> <span data-ttu-id="ab371-158">Ce comportement affecte également le rendu Razor TagHelper et HtmlHelper car il utilise les encodeurs pour vos chaînes de sortie.</span><span class="sxs-lookup"><span data-stu-id="ab371-158">This behavior also affects Razor TagHelper and HtmlHelper rendering as it will use the encoders to output your strings.</span></span>

<span data-ttu-id="ab371-159">Le raisonnement sous-jacent vise à vous protéger des bogues de navigateur inconnus ou ultérieurs (les bogues de navigateur antérieurs ont gêné l’analyse basée sur le traitement de caractères non anglais).</span><span class="sxs-lookup"><span data-stu-id="ab371-159">The reasoning behind this is to protect against unknown or future browser bugs (previous browser bugs have tripped up parsing based on the processing of non-English characters).</span></span> <span data-ttu-id="ab371-160">Si votre site web utilise de façon intensive les caractères non latins, comme le chinois, le cyrillique, ou autre, cela n'est probablement pas le comportement que vous souhaitez.</span><span class="sxs-lookup"><span data-stu-id="ab371-160">If your web site makes heavy use of non-Latin characters, such as Chinese, Cyrillic or others this is probably not the behavior you want.</span></span>

<span data-ttu-id="ab371-161">Vous pouvez personnaliser les listes sécurisées de l’encodeur pour inclure les plages Unicode appropriées à votre application au démarrage, dans `ConfigureServices()`.</span><span class="sxs-lookup"><span data-stu-id="ab371-161">You can customize the encoder safe lists to include Unicode ranges appropriate to your application during startup, in `ConfigureServices()`.</span></span>

<span data-ttu-id="ab371-162">Par exemple, à l’aide de la configuration par défaut, vous pouvez utiliser un HtmlHelper Razor comme ;</span><span class="sxs-lookup"><span data-stu-id="ab371-162">For example, using the default configuration you might use a Razor HtmlHelper like so;</span></span>

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

<span data-ttu-id="ab371-163">Lorsque vous affichez la source de la page web vous verrez quelle a été restituée comme suit, avec le texte chinois encodé ;</span><span class="sxs-lookup"><span data-stu-id="ab371-163">When you view the source of the web page you will see it has been rendered as follows, with the Chinese text encoded;</span></span>

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

<span data-ttu-id="ab371-164">Pour élargir les caractères traités comme sécurisés par l’encodeur, insérez la ligne suivante dans la méthode `ConfigureServices()` dans `startup.cs`;</span><span class="sxs-lookup"><span data-stu-id="ab371-164">To widen the characters treated as safe by the encoder you would insert the following line into the `ConfigureServices()` method in `startup.cs`;</span></span>

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

<span data-ttu-id="ab371-165">Cet exemple étend la liste sécurisée pour inclure la plage Unicode CjkUnifiedIdeographs.</span><span class="sxs-lookup"><span data-stu-id="ab371-165">This example widens the safe list to include the Unicode Range CjkUnifiedIdeographs.</span></span> <span data-ttu-id="ab371-166">La sortie rendue deviendrait alors</span><span class="sxs-lookup"><span data-stu-id="ab371-166">The rendered output would now become</span></span>

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

<span data-ttu-id="ab371-167">Les plages de liste sécurisée sont spécifiées sous la forme de graphiques de code Unicode, et non de langues.</span><span class="sxs-lookup"><span data-stu-id="ab371-167">Safe list ranges are specified as Unicode code charts, not languages.</span></span> <span data-ttu-id="ab371-168">La [norme Unicode](https://unicode.org/) contient une liste de [graphiques de code](https://www.unicode.org/charts/index.html) que vous pouvez utiliser pour rechercher le graphique contenant vos caractères.</span><span class="sxs-lookup"><span data-stu-id="ab371-168">The [Unicode standard](https://unicode.org/) has a list of [code charts](https://www.unicode.org/charts/index.html) you can use to find the chart containing your characters.</span></span> <span data-ttu-id="ab371-169">Chaque encodeur, html, JavaScript et URL doivent être configurés séparément.</span><span class="sxs-lookup"><span data-stu-id="ab371-169">Each encoder, Html, JavaScript and Url, must be configured separately.</span></span>

> [!NOTE]
> <span data-ttu-id="ab371-170">La personnalisation de la liste n’affecte que les encodeurs provenant de DI.</span><span class="sxs-lookup"><span data-stu-id="ab371-170">Customization of the safe list only affects encoders sourced via DI.</span></span> <span data-ttu-id="ab371-171">Si vous accédez directement à un encodeur via `System.Text.Encodings.Web.*Encoder.Default`, la valeur par défaut, latin de base uniquement, est utilisée.</span><span class="sxs-lookup"><span data-stu-id="ab371-171">If you directly access an encoder via `System.Text.Encodings.Web.*Encoder.Default` then the default, Basic Latin only safelist will be used.</span></span>

## <a name="where-should-encoding-take-place"></a><span data-ttu-id="ab371-172">Où l'encodage doit s'effectuer ?</span><span class="sxs-lookup"><span data-stu-id="ab371-172">Where should encoding take place?</span></span>

<span data-ttu-id="ab371-173">Les pratiques générales acceptées est que l'encodage intervienne au point de rendu et les valeurs encodées ne doivent jamais être stockées dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="ab371-173">The general accepted practice is that encoding takes place at the point of output and encoded values should never be stored in a database.</span></span> <span data-ttu-id="ab371-174">L'encodage dans la phase de rendu vous permet de modifier l’utilisation des données, par exemple, à partir de HTML à une valeur de chaîne de requête.</span><span class="sxs-lookup"><span data-stu-id="ab371-174">Encoding at the point of output allows you to change the use of data, for example, from HTML to a query string value.</span></span> <span data-ttu-id="ab371-175">Aussi, il vous permet de rechercher facilement vos données sans avoir à encoder les valeurs avant de les rechercher et vous permet de tirer parti des modifications ou des correctifs de bogues apportés aux encodeurs.</span><span class="sxs-lookup"><span data-stu-id="ab371-175">It also enables you to easily search your data without having to encode values before searching and allows you to take advantage of any changes or bug fixes made to encoders.</span></span>

## <a name="validation-as-an-xss-prevention-technique"></a><span data-ttu-id="ab371-176">Validation en tant que technique de prévention XSS</span><span class="sxs-lookup"><span data-stu-id="ab371-176">Validation as an XSS prevention technique</span></span>

<span data-ttu-id="ab371-177">La validation peut être un outil utile dans limitation des attaques XSS.</span><span class="sxs-lookup"><span data-stu-id="ab371-177">Validation can be a useful tool in limiting XSS attacks.</span></span> <span data-ttu-id="ab371-178">Par exemple, une chaîne numérique contenant uniquement les caractères 0-9 ne déclenchera pas d’attaque XSS.</span><span class="sxs-lookup"><span data-stu-id="ab371-178">For example, a numeric string containing only the characters 0-9 won't trigger an XSS attack.</span></span> <span data-ttu-id="ab371-179">La validation devient plus complexe lors de l’acceptation du code HTML dans l’entrée utilisateur.</span><span class="sxs-lookup"><span data-stu-id="ab371-179">Validation becomes more complicated when accepting HTML in user input.</span></span> <span data-ttu-id="ab371-180">L’analyse de l’entrée HTML est difficile, voire impossible.</span><span class="sxs-lookup"><span data-stu-id="ab371-180">Parsing HTML input is difficult, if not impossible.</span></span> <span data-ttu-id="ab371-181">La démarque, couplée avec un analyseur qui supprime le code HTML incorporé, est une option plus sûre pour accepter une entrée enrichie.</span><span class="sxs-lookup"><span data-stu-id="ab371-181">Markdown, coupled with a parser that strips embedded HTML, is a safer option for accepting rich input.</span></span> <span data-ttu-id="ab371-182">Ne vous fiez jamais à la validation.</span><span class="sxs-lookup"><span data-stu-id="ab371-182">Never rely on validation alone.</span></span> <span data-ttu-id="ab371-183">Encodez toujours les entrées non approuvées avant la sortie, quelle que soit la validation ou l’assainissement effectué.</span><span class="sxs-lookup"><span data-stu-id="ab371-183">Always encode untrusted input before output, no matter what validation or sanitization has been performed.</span></span>
