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
ms.openlocfilehash: 347e5ddbc02866887d3f422ecc291e5e3dfacaaf
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207912"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a><span data-ttu-id="1fbad-104">Introduction à la programmation Web ASP.NET à l’aide de la syntaxe Razor (c#)</span><span class="sxs-lookup"><span data-stu-id="1fbad-104">Introduction to ASP.NET Web Programming Using the Razor Syntax (C#)</span></span>
====================
<span data-ttu-id="1fbad-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="1fbad-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="1fbad-106">Cet article vous donne une vue d’ensemble de la programmation avec les Pages Web ASP.NET à l’aide de la syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="1fbad-106">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax.</span></span> <span data-ttu-id="1fbad-107">ASP.NET est la technologie de Microsoft pour les pages web dynamiques en cours d’exécution sur les serveurs web.</span><span class="sxs-lookup"><span data-stu-id="1fbad-107">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span> <span data-ttu-id="1fbad-108">Cette articles est consacrée à l’aide du langage de programmation c#.</span><span class="sxs-lookup"><span data-stu-id="1fbad-108">This articles focuses on using the C# programming language.</span></span>
> 
> <span data-ttu-id="1fbad-109">**Ce que vous allez apprendre**:</span><span class="sxs-lookup"><span data-stu-id="1fbad-109">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="1fbad-110">Le 8 principaux conseils de mise en route avec ASP.NET Web Pages à l’aide de la syntaxe Razor de la programmation de programmation.</span><span class="sxs-lookup"><span data-stu-id="1fbad-110">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="1fbad-111">Concepts de programmation de base que vous avez besoin.</span><span class="sxs-lookup"><span data-stu-id="1fbad-111">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="1fbad-112">Le code de serveur ASP.NET et la syntaxe Razor concerne tous.</span><span class="sxs-lookup"><span data-stu-id="1fbad-112">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="1fbad-113">Versions des logiciels</span><span class="sxs-lookup"><span data-stu-id="1fbad-113">Software versions</span></span>
> 
> 
> - <span data-ttu-id="1fbad-114">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="1fbad-114">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="1fbad-115">Ce didacticiel fonctionne également avec ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="1fbad-115">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="the-top-8-programming-tips"></a><span data-ttu-id="1fbad-116">Les meilleurs conseils de programmation 8</span><span class="sxs-lookup"><span data-stu-id="1fbad-116">The Top 8 Programming Tips</span></span>

<span data-ttu-id="1fbad-117">Cette section répertorie quelques conseils que vous devez absolument connaître lorsque vous commencez à écrire le code de serveur ASP.NET à l’aide de la syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="1fbad-117">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="1fbad-118">La syntaxe Razor est basée sur le langage de programmation c#, et qui est la langue est utilisée plus souvent avec les Pages Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1fbad-118">The Razor syntax is based on the C# programming language, and that's the language that's used most often with ASP.NET Web Pages.</span></span> <span data-ttu-id="1fbad-119">Toutefois, la syntaxe Razor prend également en charge le langage Visual Basic et tout ce que vous voyez que vous pouvez également faire dans Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="1fbad-119">However, the Razor syntax also supports the Visual Basic language, and everything you see you can also do in Visual Basic.</span></span> <span data-ttu-id="1fbad-120">Pour plus d’informations, consultez l’annexe [langage Visual Basic et la syntaxe](https://go.microsoft.com/fwlink/?LinkId=202908).</span><span class="sxs-lookup"><span data-stu-id="1fbad-120">For details, see the appendix [Visual Basic Language and Syntax](https://go.microsoft.com/fwlink/?LinkId=202908).</span></span>


<span data-ttu-id="1fbad-121">Vous trouverez plus d’informations sur la plupart de ces techniques de programmation plus loin dans l’article.</span><span class="sxs-lookup"><span data-stu-id="1fbad-121">You can find more details about most of these programming techniques later in the article.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="1fbad-122">1. Ajouter du code à une page à l’aide du caractère « @ »</span><span class="sxs-lookup"><span data-stu-id="1fbad-122">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="1fbad-123">Le `@` caractère démarre les expressions inline, les blocs d’instruction unique et les blocs d’instructions multiples :</span><span class="sxs-lookup"><span data-stu-id="1fbad-123">The `@` character starts inline expressions, single statement blocks, and multi-statement blocks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

<span data-ttu-id="1fbad-124">Il s’agit d’aspect de ces instructions lorsque la page s’exécute dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="1fbad-124">This is what these statements look like when the page runs in a browser:</span></span>

![Razor-Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="1fbad-126">**Codage HTML**</span><span class="sxs-lookup"><span data-stu-id="1fbad-126">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="1fbad-127">Lorsque vous affichez le contenu dans une page à l’aide de la `@` de caractères, comme dans les exemples précédents, ASP.NET encode au format HTML la sortie.</span><span class="sxs-lookup"><span data-stu-id="1fbad-127">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="1fbad-128">Cela remplace les caractères HTML réservés (tels que `<` et `>` et `&`) avec des codes qui permettent les caractères à afficher en tant que caractères dans une page web au lieu d’être interprétée comme des balises HTML ou des entités.</span><span class="sxs-lookup"><span data-stu-id="1fbad-128">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="1fbad-129">Sans codage HTML, la sortie à partir de votre code de serveur peuvent ne pas s’afficher correctement et peut exposer une page à des risques de sécurité.</span><span class="sxs-lookup"><span data-stu-id="1fbad-129">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="1fbad-130">Si votre objectif est de sortie de balisage HTML qui rend les balises en tant que balisage (par exemple `<p></p>` pour un paragraphe ou `<em></em>` pour mettre en évidence de texte), consultez la section [combinant le texte, le balisage et Code dans les blocs de Code](#BM_CombiningTextMarkupAndCode) plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="1fbad-130">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="1fbad-131">Vous trouverez plus d’informations sur le codage HTML dans [utilisation des formulaires](https://go.microsoft.com/fwlink/?LinkId=202892).</span><span class="sxs-lookup"><span data-stu-id="1fbad-131">You can read more about HTML encoding in [Working with Forms](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>


### <a name="2-you-enclose-code-blocks-in-braces"></a><span data-ttu-id="1fbad-132">2. Vous placez les blocs de code entre accolades</span><span class="sxs-lookup"><span data-stu-id="1fbad-132">2. You enclose code blocks in braces</span></span>

<span data-ttu-id="1fbad-133">Un *bloc de code* inclut une ou plusieurs instructions de code et est placé entre accolades.</span><span class="sxs-lookup"><span data-stu-id="1fbad-133">A *code block* includes one or more code statements and is enclosed in braces.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

<span data-ttu-id="1fbad-134">Le résultat est affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="1fbad-134">The result displayed in a browser:</span></span>

![Razor-Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a><span data-ttu-id="1fbad-136">3. À l’intérieur d’un bloc, vous obtenez chaque instruction du code par un point-virgule</span><span class="sxs-lookup"><span data-stu-id="1fbad-136">3. Inside a block, you end each code statement with a semicolon</span></span>

<span data-ttu-id="1fbad-137">À l’intérieur d’un bloc de code, chaque instruction de code complet doit se terminer par un point-virgule.</span><span class="sxs-lookup"><span data-stu-id="1fbad-137">Inside a code block, each complete code statement must end with a semicolon.</span></span> <span data-ttu-id="1fbad-138">Expressions inline ne se terminent par un point-virgule.</span><span class="sxs-lookup"><span data-stu-id="1fbad-138">Inline expressions don't end with a semicolon.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="1fbad-139">4. Vous utilisez des variables pour stocker des valeurs</span><span class="sxs-lookup"><span data-stu-id="1fbad-139">4. You use variables to store values</span></span>

<span data-ttu-id="1fbad-140">Vous pouvez stocker des valeurs dans un *variable*, y compris les chaînes, des chiffres et des dates, etc. Vous créez une variable en utilisant le `var` mot clé.</span><span class="sxs-lookup"><span data-stu-id="1fbad-140">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `var` keyword.</span></span> <span data-ttu-id="1fbad-141">Vous pouvez insérer des valeurs des variables directement dans une page à l’aide `@`.</span><span class="sxs-lookup"><span data-stu-id="1fbad-141">You can insert variable values directly in a page using `@`.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

<span data-ttu-id="1fbad-142">Le résultat est affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="1fbad-142">The result displayed in a browser:</span></span>

![Razor-Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="1fbad-144">5. Vous placez les valeurs de chaîne littérale entre guillemets doubles</span><span class="sxs-lookup"><span data-stu-id="1fbad-144">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="1fbad-145">Un *chaîne* est une séquence de caractères qui sont traitées en tant que texte.</span><span class="sxs-lookup"><span data-stu-id="1fbad-145">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="1fbad-146">Pour spécifier une chaîne, mettez-le entre guillemets doubles :</span><span class="sxs-lookup"><span data-stu-id="1fbad-146">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

<span data-ttu-id="1fbad-147">Si la chaîne que vous souhaitez afficher contient un caractère de barre oblique inverse ( `\` ) ou des guillemets doubles ( `"` ), utilisez un *littéral de chaîne textuelle* qui est préfixé avec le `@` opérateur.</span><span class="sxs-lookup"><span data-stu-id="1fbad-147">If the string that you want to display contains a backslash character ( `\` ) or double quotation marks ( `"` ), use a *verbatim string literal* that's prefixed with the `@` operator.</span></span> <span data-ttu-id="1fbad-148">(En c#, le \ caractère a une signification spéciale, sauf si vous utilisez un littéral de chaîne textuelle.)</span><span class="sxs-lookup"><span data-stu-id="1fbad-148">(In C#, the \ character has special meaning unless you use a verbatim string literal.)</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

<span data-ttu-id="1fbad-149">Pour incorporer des guillemets doubles, utiliser un littéral de chaîne textuelle et répétez les guillemets :</span><span class="sxs-lookup"><span data-stu-id="1fbad-149">To embed double quotation marks, use a verbatim string literal and repeat the quotation marks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

<span data-ttu-id="1fbad-150">Voici le résultat de l’utilisation de ces deux exemples dans une page :</span><span class="sxs-lookup"><span data-stu-id="1fbad-150">Here's the result of using both of these examples in a page:</span></span>

![Razor-Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> <span data-ttu-id="1fbad-152">Notez que le `@` caractère est utilisé pour marquer des littéraux de chaîne textuelle dans c# et à marquer du code dans les pages ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1fbad-152">Notice that the `@` character is used both to mark verbatim string literals in C# and to mark code in ASP.NET pages.</span></span>


### <a name="6-code-is-case-sensitive"></a><span data-ttu-id="1fbad-153">6. Code respecte la casse</span><span class="sxs-lookup"><span data-stu-id="1fbad-153">6. Code is case sensitive</span></span>

<span data-ttu-id="1fbad-154">En c#, mots clés (telles que `var`, `true`, et `if`) et les noms de variables respectent la casse.</span><span class="sxs-lookup"><span data-stu-id="1fbad-154">In C#, keywords (like `var`, `true`, and `if`) and variable names are case sensitive.</span></span> <span data-ttu-id="1fbad-155">Les lignes suivantes de code créent deux variables différentes, `lastName` et `LastName.`</span><span class="sxs-lookup"><span data-stu-id="1fbad-155">The following lines of code create two different variables, `lastName` and `LastName.`</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

<span data-ttu-id="1fbad-156">Si vous déclarez une variable en tant que `var lastName = "Smith";` et si vous tentez de référencer cette variable dans votre page en tant que `@LastName`, une erreur se produit, car `LastName` ne sera pas reconnue.</span><span class="sxs-lookup"><span data-stu-id="1fbad-156">If you declare a variable as `var lastName = "Smith";` and if you try to reference that variable in your page as `@LastName`, an error results because `LastName` won't be recognized.</span></span>

> [!NOTE]
> <span data-ttu-id="1fbad-157">En Visual Basic, mots clés et les variables sont *pas* respecte la casse.</span><span class="sxs-lookup"><span data-stu-id="1fbad-157">In Visual Basic, keywords and variables are *not* case sensitive.</span></span>


### <a name="7-much-of-your-coding-involves-objects"></a><span data-ttu-id="1fbad-158">7. Une grande partie de votre codage implique des objets</span><span class="sxs-lookup"><span data-stu-id="1fbad-158">7. Much of your coding involves objects</span></span>

<span data-ttu-id="1fbad-159">Un *objet* représente une chose que vous pouvez programmer avec &#8212; une page, une zone de texte, un fichier, une image, une requête web, un message électronique, un enregistrement de client (ligne de base de données), etc. Objets ont des propriétés qui décrivent leurs caractéristiques, et que vous pouvez lire ou modifier &#8212; un objet de zone de texte a un `Text` propriété (entre autres), un objet de requête a un `Url` propriété, un message électronique a un `From` propriété, et un objet customer possède une `FirstName` propriété.</span><span class="sxs-lookup"><span data-stu-id="1fbad-159">An *object* represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics and that you can read or change &#8212; a text box object has a `Text` property (among others), a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="1fbad-160">Objets possèdent également des méthodes qui sont le &quot;verbes&quot; qu’ils peuvent effectuer.</span><span class="sxs-lookup"><span data-stu-id="1fbad-160">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="1fbad-161">Exemples incluent un objet fichier `Save` (méthode), un objet image `Rotate` (méthode) et un objet de messagerie `Send` (méthode).</span><span class="sxs-lookup"><span data-stu-id="1fbad-161">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="1fbad-162">Vous utiliserez souvent le `Request` de l’objet, ce qui vous donne des informations telles que les valeurs des zones de texte (champs de formulaire) dans la page, le type de navigateur fait la demande, l’URL de la page, de l’identité de l’utilisateur, etc. L’exemple suivant montre comment accéder aux propriétés de la `Request` objet et comment appeler le `MapPath` méthode de la `Request` objet, ce qui vous donne le chemin d’accès absolu de la page sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="1fbad-162">You'll often work with the `Request` object, which gives you information like the values of text boxes (form fields) on the page, what type of browser made the request, the URL of the page, the user identity, etc. The following example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

<span data-ttu-id="1fbad-163">Le résultat est affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="1fbad-163">The result displayed in a browser:</span></span>

![Razor-Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="1fbad-165">8. Vous pouvez écrire du code qui prend des décisions</span><span class="sxs-lookup"><span data-stu-id="1fbad-165">8. You can write code that makes decisions</span></span>

<span data-ttu-id="1fbad-166">Une fonctionnalité clé des pages web dynamiques est que vous pouvez déterminer ce qu’il faut faire en fonction des conditions.</span><span class="sxs-lookup"><span data-stu-id="1fbad-166">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="1fbad-167">L’approche la plus courante pour ce faire est avec la `if` instruction (et éventuellement `else` instruction).</span><span class="sxs-lookup"><span data-stu-id="1fbad-167">The most common way to do this is with the `if` statement (and optional `else` statement).</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

<span data-ttu-id="1fbad-168">L’instruction `if(IsPost)` est un moyen rapide de la rédaction de `if(IsPost == true)`.</span><span class="sxs-lookup"><span data-stu-id="1fbad-168">The statement `if(IsPost)` is a shorthand way of writing `if(IsPost == true)`.</span></span> <span data-ttu-id="1fbad-169">Avec `if` instructions, il existe plusieurs façons pour tester les conditions, la répétition des blocs de code, et ainsi de suite, qui sont décrits plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="1fbad-169">Along with `if` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="1fbad-170">Le résultat affiché dans un navigateur (après avoir cliqué sur **envoyer**) :</span><span class="sxs-lookup"><span data-stu-id="1fbad-170">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor-Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a><span data-ttu-id="1fbad-172">HTTP GET et POST méthodes et la propriété IsPost</span><span class="sxs-lookup"><span data-stu-id="1fbad-172">HTTP GET and POST Methods and the IsPost Property</span></span>
> 
> <span data-ttu-id="1fbad-173">Le protocole utilisé pour les pages web (HTTP) prend en charge un nombre très limité de méthodes (verbes) qui sont utilisés pour effectuer des demandes au serveur.</span><span class="sxs-lookup"><span data-stu-id="1fbad-173">The protocol used for web pages (HTTP) supports a very limited number of methods (verbs) that are used to make requests to the server.</span></span> <span data-ttu-id="1fbad-174">Les deux plus courantes sont GET, qui est utilisé pour lire une page, et POST, ce qui est utilisé pour envoyer une page.</span><span class="sxs-lookup"><span data-stu-id="1fbad-174">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="1fbad-175">En règle générale, la première fois qu’un utilisateur demande une page, la page est demandée à l’aide de GET.</span><span class="sxs-lookup"><span data-stu-id="1fbad-175">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="1fbad-176">Si l’utilisateur remplit un formulaire, puis clique sur un bouton d’envoi, le navigateur envoie une demande POST vers le serveur.</span><span class="sxs-lookup"><span data-stu-id="1fbad-176">If the user fills in a form and then clicks a submit button, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="1fbad-177">Dans la programmation web, il est souvent utile de savoir si une page est demandée sous la forme d’une opération GET ou un billet afin que vous sachiez comment traiter la page.</span><span class="sxs-lookup"><span data-stu-id="1fbad-177">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="1fbad-178">Dans ASP.NET Web Pages, vous pouvez utiliser le `IsPost` propriété pour déterminer si une requête est une opération GET ou POST.</span><span class="sxs-lookup"><span data-stu-id="1fbad-178">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="1fbad-179">Si la demande est une publication, le `IsPost` propriété retournera la valeur true, et vous pouvez effectuer les opérations en lecture les valeurs des zones de texte sur un formulaire.</span><span class="sxs-lookup"><span data-stu-id="1fbad-179">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="1fbad-180">Vous verrez de nombreux exemples vous montrent comment traiter la page différemment selon la valeur de `IsPost`.</span><span class="sxs-lookup"><span data-stu-id="1fbad-180">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>


## <a name="a-simple-code-example"></a><span data-ttu-id="1fbad-181">Un exemple de Code Simple</span><span class="sxs-lookup"><span data-stu-id="1fbad-181">A Simple Code Example</span></span>

<span data-ttu-id="1fbad-182">Cette procédure vous montre comment créer une page qui illustre des techniques de programmation de base.</span><span class="sxs-lookup"><span data-stu-id="1fbad-182">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="1fbad-183">Dans l’exemple, vous créez une page qui permet aux utilisateurs d’entrer des deux nombres, puis il les ajoute et affiche le résultat.</span><span class="sxs-lookup"><span data-stu-id="1fbad-183">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="1fbad-184">Dans votre éditeur, créez un nouveau fichier et nommez-le *AddNumbers.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1fbad-184">In your editor, create a new file and name it *AddNumbers.cshtml*.</span></span>
2. <span data-ttu-id="1fbad-185">Copiez le code et le balisage suivant dans la page, en remplaçant tout élément déjà dans la page.</span><span class="sxs-lookup"><span data-stu-id="1fbad-185">Copy the following code and markup into the page, replacing anything already in the page.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    <span data-ttu-id="1fbad-186">Voici quelques éléments de noter :</span><span class="sxs-lookup"><span data-stu-id="1fbad-186">Here are some things for you to note:</span></span>

    - <span data-ttu-id="1fbad-187">Le `@` caractère démarre le premier bloc de code dans la page, et il précède le `totalMessage` variable qui est incorporé au bas de la page.</span><span class="sxs-lookup"><span data-stu-id="1fbad-187">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable that's embedded near the bottom of the page.</span></span>
    - <span data-ttu-id="1fbad-188">Le bloc en haut de la page est placé entre accolades.</span><span class="sxs-lookup"><span data-stu-id="1fbad-188">The block at the top of the page is enclosed in braces.</span></span>
    - <span data-ttu-id="1fbad-189">Dans le bloc en haut, toutes les lignes se terminant par un point-virgule.</span><span class="sxs-lookup"><span data-stu-id="1fbad-189">In the block at the top, all lines end with a semicolon.</span></span>
    - <span data-ttu-id="1fbad-190">Les variables `total`, `num1`, `num2`, et `totalMessage` stocker plusieurs nombres et une chaîne.</span><span class="sxs-lookup"><span data-stu-id="1fbad-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="1fbad-191">La valeur de littéral de chaîne assignée à la `totalMessage` variable est entre guillemets doubles.</span><span class="sxs-lookup"><span data-stu-id="1fbad-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="1fbad-192">Étant donné que le code respecte la casse, lorsque le `totalMessage` variable est utilisée au bas de la page, son nom doit correspondre exactement à la variable en haut.</span><span class="sxs-lookup"><span data-stu-id="1fbad-192">Because the code is case-sensitive, when the `totalMessage` variable is used near the bottom of the page, its name must match the variable at the top exactly.</span></span>
    - <span data-ttu-id="1fbad-193">L’expression `num1.AsInt() + num2.AsInt()` montre comment utiliser des méthodes et des objets.</span><span class="sxs-lookup"><span data-stu-id="1fbad-193">The expression `num1.AsInt() + num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="1fbad-194">Le `AsInt` méthode sur chaque variable convertit la chaîne entrée par un utilisateur à un nombre (entier) afin que vous pouvez effectuer des opérations arithmétiques sur celui-ci.</span><span class="sxs-lookup"><span data-stu-id="1fbad-194">The `AsInt` method on each variable converts the string entered by a user to a number (an integer) so that you can perform arithmetic on it.</span></span>
    - <span data-ttu-id="1fbad-195">Le `<form>` balise inclut un `method="post"` attribut.</span><span class="sxs-lookup"><span data-stu-id="1fbad-195">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="1fbad-196">Spécifie que lorsque l’utilisateur clique sur **ajouter**, la page sera être envoyée au serveur à l’aide de la méthode HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="1fbad-196">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="1fbad-197">Lorsque la page est envoyée, le `if(IsPost)` test a la valeur true et l’instruction conditionnelle de code s’exécute, affiche le résultat de l’ajout de nombres.</span><span class="sxs-lookup"><span data-stu-id="1fbad-197">When the page is submitted, the `if(IsPost)` test evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="1fbad-198">Enregistrez la page et l’exécuter dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="1fbad-198">Save the page and run it in a browser.</span></span> <span data-ttu-id="1fbad-199">(Assurez-vous que la page est sélectionnée dans le **fichiers** espace de travail avant de l’exécuter.) Entrez deux nombres entiers, puis activez la **ajouter** bouton.</span><span class="sxs-lookup"><span data-stu-id="1fbad-199">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span> 

    ![Razor-Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a><span data-ttu-id="1fbad-201">Concepts de programmation de base</span><span class="sxs-lookup"><span data-stu-id="1fbad-201">Basic Programming Concepts</span></span>

<span data-ttu-id="1fbad-202">Cet article vous offre une vue d’ensemble de la programmation web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1fbad-202">This article provides you with an overview of ASP.NET web programming.</span></span> <span data-ttu-id="1fbad-203">Il n’est pas un examen exhaustif, simplement une rapide visite guidée les concepts de programmation que vous utiliserez le plus souvent.</span><span class="sxs-lookup"><span data-stu-id="1fbad-203">It isn't an exhaustive examination, just a quick tour through the programming concepts you'll use most often.</span></span> <span data-ttu-id="1fbad-204">Même dans ce cas, il couvre presque tout ce dont vous aurez besoin pour bien démarrer avec ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="1fbad-204">Even so, it covers almost everything you'll need to get started with ASP.NET Web Pages.</span></span>

<span data-ttu-id="1fbad-205">Mais tout d’abord, un peu bagage technique.</span><span class="sxs-lookup"><span data-stu-id="1fbad-205">But first, a little technical background.</span></span>

### <a name="the-razor-syntax-server-code-and-aspnet"></a><span data-ttu-id="1fbad-206">La syntaxe Razor, le Code serveur et ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1fbad-206">The Razor Syntax, Server Code, and ASP.NET</span></span>

<span data-ttu-id="1fbad-207">Syntaxe Razor est une syntaxe de programmation simple pour l’incorporation de code basé sur le serveur dans une page web.</span><span class="sxs-lookup"><span data-stu-id="1fbad-207">Razor syntax is a simple programming syntax for embedding server-based code in a web page.</span></span> <span data-ttu-id="1fbad-208">Dans une page web qui utilise la syntaxe Razor, il existe deux types de contenu : code de contenu et du serveur du client.</span><span class="sxs-lookup"><span data-stu-id="1fbad-208">In a web page that uses the Razor syntax, there are two kinds of content: client content and server code.</span></span> <span data-ttu-id="1fbad-209">Contenu du client est les choses que vous êtes habitué dans les pages web : le balisage HTML (éléments), des informations de style tels que CSS, peut-être script côté client telles que JavaScript et texte brut.</span><span class="sxs-lookup"><span data-stu-id="1fbad-209">Client content is the stuff you're used to in web pages: HTML markup (elements), style information such as CSS, maybe some client script such as JavaScript, and plain text.</span></span>

<span data-ttu-id="1fbad-210">Syntaxe Razor vous permet d’ajouter le code de serveur à ce contenu client.</span><span class="sxs-lookup"><span data-stu-id="1fbad-210">Razor syntax lets you add server code to this client content.</span></span> <span data-ttu-id="1fbad-211">S’il existe le code serveur dans la page, le serveur exécute tout d’abord, ce code avant d’envoyer la page au navigateur.</span><span class="sxs-lookup"><span data-stu-id="1fbad-211">If there's server code in the page, the server runs that code first, before it sends the page to the browser.</span></span> <span data-ttu-id="1fbad-212">S’exécute sur le serveur, le code peut effectuer des tâches qui peuvent être beaucoup plus difficile de le faire à l’aide du client à du contenu uniquement, comme l’accès à des bases de données basées sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="1fbad-212">By running on the server, the code can perform tasks that can be a lot more complex to do using client content alone, like accessing server-based databases.</span></span> <span data-ttu-id="1fbad-213">Plus important encore, le code serveur peut créer dynamiquement du contenu de client &#8212; il peut générer le balisage HTML ou autres contenus à la volée et puis l’envoyer au navigateur, ainsi que toutes les données HTML statique que la page peut contenir.</span><span class="sxs-lookup"><span data-stu-id="1fbad-213">Most importantly, server code can dynamically create client content &#8212; it can generate HTML markup or other content on the fly and then send it to the browser along with any static HTML that the page might contain.</span></span> <span data-ttu-id="1fbad-214">Du point de vue du navigateur, client à du contenu généré par le code de votre serveur n’est pas différent de celui de tout autre contenu client.</span><span class="sxs-lookup"><span data-stu-id="1fbad-214">From the browser's perspective, client content that's generated by your server code is no different than any other client content.</span></span> <span data-ttu-id="1fbad-215">Comme vous l’avez déjà vu, le code du serveur qui a requis est assez simple.</span><span class="sxs-lookup"><span data-stu-id="1fbad-215">As you've already seen, the server code that's required is quite simple.</span></span>

<span data-ttu-id="1fbad-216">Les pages web ASP.NET qui incluent la syntaxe Razor ont une extension de fichier spécial (*.cshtml* ou *.vbhtml*).</span><span class="sxs-lookup"><span data-stu-id="1fbad-216">ASP.NET web pages that include the Razor syntax have a special file extension (*.cshtml* or *.vbhtml*).</span></span> <span data-ttu-id="1fbad-217">Le serveur reconnaît ces extensions, exécute le code qui est marqué avec la syntaxe Razor, puis envoie la page au navigateur.</span><span class="sxs-lookup"><span data-stu-id="1fbad-217">The server recognizes these extensions, runs the code that's marked with Razor syntax, and then sends the page to the browser.</span></span>

### <a name="where-does-aspnet-fit-in"></a><span data-ttu-id="1fbad-218">Quand ASP.NET convient-elle ?</span><span class="sxs-lookup"><span data-stu-id="1fbad-218">Where does ASP.NET fit in?</span></span>

<span data-ttu-id="1fbad-219">Syntaxe Razor est basée sur une technologie de Microsoft appelé ASP.NET, qui à son tour est basé sur Microsoft .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="1fbad-219">Razor syntax is based on a technology from Microsoft called ASP.NET, which in turn is based on the Microsoft .NET Framework.</span></span> <span data-ttu-id="1fbad-220">Le.NET Framework est une infrastructure de programmation big et complet de Microsoft pour le développement de pratiquement n’importe quel type d’application de l’ordinateur.</span><span class="sxs-lookup"><span data-stu-id="1fbad-220">The.NET Framework is a big, comprehensive programming framework from Microsoft for developing virtually any type of computer application.</span></span> <span data-ttu-id="1fbad-221">ASP.NET est la partie du .NET Framework qui est spécialement conçu pour la création d’applications web.</span><span class="sxs-lookup"><span data-stu-id="1fbad-221">ASP.NET is the part of the .NET Framework that's specifically designed for creating web applications.</span></span> <span data-ttu-id="1fbad-222">Les développeurs ont utilisé ASP.NET pour créer la majorité des sites Web plus grands et le plus élevé de trafic dans le monde.</span><span class="sxs-lookup"><span data-stu-id="1fbad-222">Developers have used ASP.NET to create many of the largest and highest-traffic websites in the world.</span></span> <span data-ttu-id="1fbad-223">(Chaque fois que vous voyez l’extension de nom de fichier *.aspx* dans le cadre de l’URL dans un site, vous saurez que le site a été écrit à l’aide d’ASP.NET.)</span><span class="sxs-lookup"><span data-stu-id="1fbad-223">(Any time you see the file-name extension *.aspx* as part of the URL in a site, you'll know that the site was written using ASP.NET.)</span></span>

<span data-ttu-id="1fbad-224">La syntaxe Razor offre toute la puissance d’ASP.NET, mais en utilisant une syntaxe simplifiée est plus facile de savoir si vous êtes débutant et qui vous rend plus productif si vous êtes un expert.</span><span class="sxs-lookup"><span data-stu-id="1fbad-224">The Razor syntax gives you all the power of ASP.NET, but using a simplified syntax that's easier to learn if you're a beginner and that makes you more productive if you're an expert.</span></span> <span data-ttu-id="1fbad-225">Bien que cette syntaxe est simple à utiliser, sa relation famille avec ASP.NET et .NET Framework signifie que comme vos sites Web est sophistiquées, plus vous disposez de la puissance des infrastructures de plus grande à votre disposition.</span><span class="sxs-lookup"><span data-stu-id="1fbad-225">Even though this syntax is simple to use, its family relationship to ASP.NET and the .NET Framework means that as your websites become more sophisticated, you have the power of the larger frameworks available to you.</span></span>

![Razor-Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> <span data-ttu-id="1fbad-227">**Classes et Instances**</span><span class="sxs-lookup"><span data-stu-id="1fbad-227">**Classes and Instances**</span></span>
> 
> <span data-ttu-id="1fbad-228">Code de serveur ASP.NET utilise des objets, qui à son tour reposent sur l’idée des classes.</span><span class="sxs-lookup"><span data-stu-id="1fbad-228">ASP.NET server code uses objects, which are in turn built on the idea of classes.</span></span> <span data-ttu-id="1fbad-229">La classe est la définition ou un modèle pour un objet.</span><span class="sxs-lookup"><span data-stu-id="1fbad-229">The class is the definition or template for an object.</span></span> <span data-ttu-id="1fbad-230">Par exemple, une application peut contenir un `Customer` classe qui définit les propriétés et méthodes n’importe quel objet de client a besoin.</span><span class="sxs-lookup"><span data-stu-id="1fbad-230">For example, an application might contain a `Customer` class that defines the properties and methods that any customer object needs.</span></span>
> 
> <span data-ttu-id="1fbad-231">Lorsque l’application doit fonctionner avec les informations client réel, il crée une instance de (ou *instancie*) un objet customer.</span><span class="sxs-lookup"><span data-stu-id="1fbad-231">When the application needs to work with actual customer information, it creates an instance of (or *instantiates*) a customer object.</span></span> <span data-ttu-id="1fbad-232">Chaque client est une instance distincte de la `Customer` classe.</span><span class="sxs-lookup"><span data-stu-id="1fbad-232">Each individual customer is a separate instance of the `Customer` class.</span></span> <span data-ttu-id="1fbad-233">Chaque instance prend en charge les mêmes propriétés et méthodes, mais les valeurs de propriété pour chaque instance sont généralement différentes, car chaque objet customer est unique.</span><span class="sxs-lookup"><span data-stu-id="1fbad-233">Every instance supports the same properties and methods, but the property values for each instance are typically different, because each customer object is unique.</span></span> <span data-ttu-id="1fbad-234">Dans l’objet d’un client, le `LastName` propriété peut être « Smith » ; dans un autre objet client, le `LastName` propriété peut être « Jones ».</span><span class="sxs-lookup"><span data-stu-id="1fbad-234">In one customer object, the `LastName` property might be "Smith"; in another customer object, the `LastName` property might be "Jones."</span></span>
> 
> <span data-ttu-id="1fbad-235">De même, n’importe quelle page web individuelles dans votre site est un `Page` objet qui est une instance de la `Page` classe.</span><span class="sxs-lookup"><span data-stu-id="1fbad-235">Similarly, any individual web page in your site is a `Page` object that's an instance of the `Page` class.</span></span> <span data-ttu-id="1fbad-236">Un bouton sur la page est un `Button` objet qui est une instance de la `Button` classe et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="1fbad-236">A button on the page is a `Button` object that is an instance of the `Button` class, and so on.</span></span> <span data-ttu-id="1fbad-237">Chaque instance possède ses propres caractéristiques, mais elles sont basées sur ce qui est spécifié dans la définition de classe de l’objet.</span><span class="sxs-lookup"><span data-stu-id="1fbad-237">Each instance has its own characteristics, but they all are based on what's specified in the object's class definition.</span></span>


## <a name="basic-syntax"></a><span data-ttu-id="1fbad-238">Syntaxe de base</span><span class="sxs-lookup"><span data-stu-id="1fbad-238">Basic Syntax</span></span>

<span data-ttu-id="1fbad-239">Vous avez vu précédemment un exemple de base de la création d’une page ASP.NET Web Pages, et comment vous pouvez ajouter du code de serveur à la balise HTML.</span><span class="sxs-lookup"><span data-stu-id="1fbad-239">Earlier you saw a basic example of how to create an ASP.NET Web Pages page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="1fbad-240">Ici, vous allez apprendre les principes fondamentaux de l’écriture de code de serveur ASP.NET à l’aide de la syntaxe Razor &#8212; , autrement dit, les règles langage de programmation.</span><span class="sxs-lookup"><span data-stu-id="1fbad-240">Here you'll learn the basics of writing ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="1fbad-241">Si vous êtes familiarisé avec la programmation (surtout si vous avez utilisé le C, C++, c#, Visual Basic ou JavaScript), une grande partie de ce que vous lire ici sera familier.</span><span class="sxs-lookup"><span data-stu-id="1fbad-241">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="1fbad-242">Vous devrez probablement vous familiariser uniquement avec le code de serveur est ajouté à balisage dans *.cshtml* fichiers.</span><span class="sxs-lookup"><span data-stu-id="1fbad-242">You'll probably need to familiarize yourself only with how server code is added to markup in *.cshtml* files.</span></span>

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a><span data-ttu-id="1fbad-243">Combinaison de texte, balisage et Code dans les blocs de Code</span><span class="sxs-lookup"><span data-stu-id="1fbad-243">Combining Text, Markup, and Code in Code Blocks</span></span>

<span data-ttu-id="1fbad-244">Dans les blocs de code serveur, vous souhaitez souvent sortie texte ou balisage (ou les deux) à la page.</span><span class="sxs-lookup"><span data-stu-id="1fbad-244">In server code blocks, you often want to output text or markup (or both) to the page.</span></span> <span data-ttu-id="1fbad-245">Si un bloc de code de serveur contient du texte non code, mais qui au lieu de cela doit être restitué en l’état, ASP.NET doit être en mesure de distinguer ce texte à partir du code.</span><span class="sxs-lookup"><span data-stu-id="1fbad-245">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="1fbad-246">Pour ce faire, plusieurs méthodes sont possibles.</span><span class="sxs-lookup"><span data-stu-id="1fbad-246">There are several ways to do this.</span></span>

- <span data-ttu-id="1fbad-247">Placez le texte dans un élément HTML comme `<p></p>` ou `<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="1fbad-247">Enclose the text in an HTML element like `<p></p>` or `<em></em>`:</span></span>   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    <span data-ttu-id="1fbad-248">L’élément HTML peut inclure de texte, des éléments HTML supplémentaires et des expressions de code serveur.</span><span class="sxs-lookup"><span data-stu-id="1fbad-248">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="1fbad-249">Lorsque ASP.NET voit la balise HTML d’ouverture (par exemple, `<p>`), elle affiche tous les éléments, y compris l’élément et son contenu sous la forme est dans le navigateur, résolution des expressions de code serveur tandis qu’elle passe.</span><span class="sxs-lookup"><span data-stu-id="1fbad-249">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything including the element and its content as is to the browser, resolving server-code expressions as it goes.</span></span>
- <span data-ttu-id="1fbad-250">Utilisez le `@:` opérateur ou la `<text>` élément.</span><span class="sxs-lookup"><span data-stu-id="1fbad-250">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="1fbad-251">Le `@:` génère une ligne unique de contenu contenant du texte brut ou des balises HTML sans correspondance ; le `<text>` élément englobe plusieurs lignes de sortie.</span><span class="sxs-lookup"><span data-stu-id="1fbad-251">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="1fbad-252">Ces options sont utiles lorsque vous ne souhaitez pas restituer un élément HTML dans le cadre de la sortie.</span><span class="sxs-lookup"><span data-stu-id="1fbad-252">These options are useful when you don't want to render an HTML element as part of the output.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    <span data-ttu-id="1fbad-253">Si vous souhaitez plusieurs lignes de texte ou des balises HTML sans correspondance de sortie, vous pouvez le faire précéder chaque ligne avec `@:`, ou vous pouvez placer la ligne dans un `<text>` élément.</span><span class="sxs-lookup"><span data-stu-id="1fbad-253">If you want to output multiple lines of text or unmatched HTML tags, you can precede each line with `@:`, or you can enclose the line in a `<text>` element.</span></span> <span data-ttu-id="1fbad-254">Comme le `@:` opérateur,`<text>` balises sont utilisées par ASP.NET pour identifier le contenu de texte et ne sont jamais restitués dans la sortie de page.</span><span class="sxs-lookup"><span data-stu-id="1fbad-254">Like the `@:` operator,`<text>` tags are used by ASP.NET to identify text content and are never rendered in the page output.</span></span>

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    <span data-ttu-id="1fbad-255">Le premier exemple répète l’exemple précédent, mais utilise une seule paire de `<text>` balises pour encadrer le texte à afficher.</span><span class="sxs-lookup"><span data-stu-id="1fbad-255">The first example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span> <span data-ttu-id="1fbad-256">Dans le deuxième exemple, le `<text>` et `</text>` balises placer trois lignes, disposant de certaines relation contenant-contenu de texte et de des balises HTML sans correspondance (`<br />`), ainsi que le code serveur et de mise en correspondance des balises HTML.</span><span class="sxs-lookup"><span data-stu-id="1fbad-256">In the second example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="1fbad-257">Là encore, peut également faire précéder chaque ligne individuellement avec la `@:` opérateur ; les deux fonctionnent de façon.</span><span class="sxs-lookup"><span data-stu-id="1fbad-257">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1fbad-258">Sortie de texte comme indiqué dans cette section &#8212; à l’aide d’un élément HTML, le `@:` opérateur, ou la `<text>` élément &#8212; ASP.NET n’encoder en HTML la sortie.</span><span class="sxs-lookup"><span data-stu-id="1fbad-258">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="1fbad-259">(Comme indiqué précédemment, ASP.NET n’encode pas la sortie des expressions de code serveur et les blocs de code de serveur qui sont précédés `@`, sauf dans les cas spéciales indiquées dans cette section.)</span><span class="sxs-lookup"><span data-stu-id="1fbad-259">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="1fbad-260">Whitespace</span><span class="sxs-lookup"><span data-stu-id="1fbad-260">Whitespace</span></span>

<span data-ttu-id="1fbad-261">L’instruction n’affectent pas les espaces supplémentaires dans une instruction (et en dehors d’un littéral de chaîne) :</span><span class="sxs-lookup"><span data-stu-id="1fbad-261">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

<span data-ttu-id="1fbad-262">Un saut de ligne dans une instruction n’a aucun effet sur l’instruction, et vous pouvez encapsuler des instructions pour une meilleure lisibilité.</span><span class="sxs-lookup"><span data-stu-id="1fbad-262">A line break in a statement has no effect on the statement, and you can wrap statements for readability.</span></span> <span data-ttu-id="1fbad-263">Les instructions suivantes sont les mêmes :</span><span class="sxs-lookup"><span data-stu-id="1fbad-263">The following statements are the same:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

<span data-ttu-id="1fbad-264">Toutefois, vous ne pouvez pas encapsuler une ligne au milieu d’un littéral de chaîne.</span><span class="sxs-lookup"><span data-stu-id="1fbad-264">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="1fbad-265">L’exemple suivant ne fonctionne pas :</span><span class="sxs-lookup"><span data-stu-id="1fbad-265">The following example doesn't work:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

<span data-ttu-id="1fbad-266">Pour combiner une longue chaîne répartie sur plusieurs lignes, comme le code ci-dessus, il existe deux options.</span><span class="sxs-lookup"><span data-stu-id="1fbad-266">To combine a long string that wraps to multiple lines like the above code, there are two options.</span></span> <span data-ttu-id="1fbad-267">Vous pouvez utiliser l’opérateur de concaténation (`+`), comme vous le verrez plus loin dans cet article.</span><span class="sxs-lookup"><span data-stu-id="1fbad-267">You can use the concatenation operator (`+`), which you'll see later in this article.</span></span> <span data-ttu-id="1fbad-268">Vous pouvez également utiliser le `@` caractère pour créer une chaîne textuelle literal, comme vous l’avez vu précédemment dans cet article.</span><span class="sxs-lookup"><span data-stu-id="1fbad-268">You can also use the `@` character to create a verbatim string literal, as you saw earlier in this article.</span></span> <span data-ttu-id="1fbad-269">Vous pouvez rompre les littéraux de chaîne textuelle sur plusieurs lignes :</span><span class="sxs-lookup"><span data-stu-id="1fbad-269">You can break verbatim string literals across lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a><span data-ttu-id="1fbad-270">Code (et le balisage) commentaires</span><span class="sxs-lookup"><span data-stu-id="1fbad-270">Code (and Markup) Comments</span></span>

<span data-ttu-id="1fbad-271">Commentaires vous permettent de rédiger des notes pour vous-même ou d’autres personnes.</span><span class="sxs-lookup"><span data-stu-id="1fbad-271">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="1fbad-272">Ils vous permettent également de désactiver (*commentez*) une section de code ou balisage que vous ne souhaitez pas exécuter mais que vous souhaitez conserver dans votre page pour l’instant.</span><span class="sxs-lookup"><span data-stu-id="1fbad-272">They also allow you to disable (*comment out*) a section of code or markup that you don't want to run but want to keep in your page for the time being.</span></span>

<span data-ttu-id="1fbad-273">Il est différent de commentaires de syntaxe pour le code Razor et pour le balisage HTML.</span><span class="sxs-lookup"><span data-stu-id="1fbad-273">There's different commenting syntax for Razor code and for HTML markup.</span></span> <span data-ttu-id="1fbad-274">Comme avec tout le code Razor, Razor commentaires sont traités (et puis supprimés) sur le serveur avant que la page est envoyée au navigateur.</span><span class="sxs-lookup"><span data-stu-id="1fbad-274">As with all Razor code, Razor comments are processed (and then removed) on the server before the page is sent to the browser.</span></span> <span data-ttu-id="1fbad-275">Par conséquent, la syntaxe de commentaire Razor vous permet de placer des commentaires dans le code (ou même dans le balisage), vous pouvez voir quand vous modifiez le fichier, mais que les utilisateurs ne voient, même dans la page source.</span><span class="sxs-lookup"><span data-stu-id="1fbad-275">Therefore, the Razor commenting syntax lets you put comments into the code (or even into the markup) that you can see when you edit the file, but that users don't see, even in the page source.</span></span>

<span data-ttu-id="1fbad-276">Pour les commentaires ASP.NET Razor, vous commencez le commentaire avec `@*` et terminez-le avec `*@`.</span><span class="sxs-lookup"><span data-stu-id="1fbad-276">For ASP.NET Razor comments, you start the comment with `@*` and end it with `*@`.</span></span> <span data-ttu-id="1fbad-277">Ce commentaire peut être sur une seule ligne ou plusieurs lignes :</span><span class="sxs-lookup"><span data-stu-id="1fbad-277">The comment can be on one line or multiple lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

<span data-ttu-id="1fbad-278">Voici un commentaire dans un bloc de code :</span><span class="sxs-lookup"><span data-stu-id="1fbad-278">Here is a comment within a code block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

<span data-ttu-id="1fbad-279">Ici est le même bloc de code, avec la ligne de code commentée afin qu’il ne s’exécutera :</span><span class="sxs-lookup"><span data-stu-id="1fbad-279">Here is the same block of code, with the line of code commented out so that it won't run:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

<span data-ttu-id="1fbad-280">À l’intérieur d’un bloc de code, comme alternative à l’aide de syntaxe de commentaire Razor, vous pouvez utiliser la syntaxe de commentaire du langage de programmation que vous utilisez, comme c# :</span><span class="sxs-lookup"><span data-stu-id="1fbad-280">Inside a code block, as an alternative to using Razor comment syntax, you can use the commenting syntax of the programming language you're using, such as C#:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

<span data-ttu-id="1fbad-281">En c#, les commentaires à ligne unique sont précédées par le `//` caractères et commentaires à plusieurs lignes commencent par `/*` et se terminer par `*/`.</span><span class="sxs-lookup"><span data-stu-id="1fbad-281">In C#, single-line comments are preceded by the `//` characters, and multi-line comments begin with `/*` and end with `*/`.</span></span> <span data-ttu-id="1fbad-282">(Comme avec les commentaires Razor, les commentaires c# ne sont pas rendus dans le navigateur.)</span><span class="sxs-lookup"><span data-stu-id="1fbad-282">(As with Razor comments, C# comments are not rendered to the browser.)</span></span>

<span data-ttu-id="1fbad-283">Pour le balisage, comme vous le savez probablement, vous pouvez créer un commentaire HTML :</span><span class="sxs-lookup"><span data-stu-id="1fbad-283">For markup, as you probably know, you can create an HTML comment:</span></span>

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

<span data-ttu-id="1fbad-284">Les commentaires HTML commencent par `<!--` caractères et se terminent par `-->`.</span><span class="sxs-lookup"><span data-stu-id="1fbad-284">HTML comments start with `<!--` characters and end with `-->`.</span></span> <span data-ttu-id="1fbad-285">Vous pouvez utiliser les commentaires HTML pour entourer le texte non seulement, mais également toutes les balises HTML que vous pouvez souhaiter conserver dans la page, mais ne souhaitez pas effectuer le rendu.</span><span class="sxs-lookup"><span data-stu-id="1fbad-285">You can use HTML comments to surround not only text, but also any HTML markup that you may want to keep in the page but don't want to render.</span></span> <span data-ttu-id="1fbad-286">Ce commentaire HTML masquera l’intégralité du contenu des balises et le texte qu’ils contiennent :</span><span class="sxs-lookup"><span data-stu-id="1fbad-286">This HTML comment will hide the entire content of the tags and the text they contain:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

<span data-ttu-id="1fbad-287">Contrairement aux commentaires HTML, les commentaires Razor *sont* restitué dans la page et l’utilisateur puisse les voir en affichant la page source.</span><span class="sxs-lookup"><span data-stu-id="1fbad-287">Unlike Razor comments, HTML comments *are* rendered to the page and the user can see them by viewing the page source.</span></span>

<span data-ttu-id="1fbad-288">Razor présente des limitations sur les blocs imbriqués de c#.</span><span class="sxs-lookup"><span data-stu-id="1fbad-288">Razor has limitations on nested blocks of C#.</span></span> <span data-ttu-id="1fbad-289">Pour plus d’informations, consultez [Variables c# nommé et imbriqués blocs générer divisé le Code](http://aspnetwebstack.codeplex.com/workitem/1914)</span><span class="sxs-lookup"><span data-stu-id="1fbad-289">For more information see [Named C# Variables and Nested Blocks Generate Broken Code](http://aspnetwebstack.codeplex.com/workitem/1914)</span></span>

## <a name="variables"></a><span data-ttu-id="1fbad-290">Variables</span><span class="sxs-lookup"><span data-stu-id="1fbad-290">Variables</span></span>

<span data-ttu-id="1fbad-291">Une variable est un objet nommé qui vous permet de stocker des données.</span><span class="sxs-lookup"><span data-stu-id="1fbad-291">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="1fbad-292">Vous pouvez nommer les variables, mais le nom doit commencer par un caractère alphabétique et il ne peut pas contenir d’espaces blancs ou des caractères réservés.</span><span class="sxs-lookup"><span data-stu-id="1fbad-292">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="1fbad-293">Variables et Types de données</span><span class="sxs-lookup"><span data-stu-id="1fbad-293">Variables and Data Types</span></span>

<span data-ttu-id="1fbad-294">Une variable peut avoir un type de données spécifique, ce qui indique le type de données est stocké dans la variable.</span><span class="sxs-lookup"><span data-stu-id="1fbad-294">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="1fbad-295">Vous pouvez avoir des variables de chaîne qui stockent des valeurs de chaîne (comme &quot;Hello world&quot;), les variables de type entier qui stockent des valeurs de nombre entier (par exemple, 3 ou 79) et les variables de date qui stockent des valeurs de date dans une variété de formats (par exemple, 4/12/2012 ou mars 2009 ).</span><span class="sxs-lookup"><span data-stu-id="1fbad-295">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="1fbad-296">Et il existe de nombreux autres types de données que vous pouvez utiliser.</span><span class="sxs-lookup"><span data-stu-id="1fbad-296">And there are many other data types you can use.</span></span>

<span data-ttu-id="1fbad-297">Toutefois, en général inutile de spécifier un type d’une variable.</span><span class="sxs-lookup"><span data-stu-id="1fbad-297">However, you generally don't have to specify a type for a variable.</span></span> <span data-ttu-id="1fbad-298">La plupart du temps, ASP.NET peut déterminer le type en fonction de la façon dont les données dans la variable sont utilisées.</span><span class="sxs-lookup"><span data-stu-id="1fbad-298">Most of the time, ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="1fbad-299">(Il peut arriver que vous devez spécifier un type ; vous verrez d’exemples où cela est vrai.)</span><span class="sxs-lookup"><span data-stu-id="1fbad-299">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="1fbad-300">Vous déclarez une variable à l’aide du `var` mot clé (si vous ne souhaitez pas spécifier un type) ou en utilisant le nom du type :</span><span class="sxs-lookup"><span data-stu-id="1fbad-300">You declare a variable using the `var` keyword (if you don't want to specify a type) or by using the name of the type:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

<span data-ttu-id="1fbad-301">L’exemple suivant montre certaines utilisations courantes des variables dans une page web :</span><span class="sxs-lookup"><span data-stu-id="1fbad-301">The following example shows some typical uses of variables in a web page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

<span data-ttu-id="1fbad-302">Si vous combinez les exemples précédents dans une page, vous consultez ces informations apparaissent dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="1fbad-302">If you combine the previous examples in a page, you see this displayed in a browser:</span></span>

![Razor-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="1fbad-304">Conversion et le test des Types de données</span><span class="sxs-lookup"><span data-stu-id="1fbad-304">Converting and Testing Data Types</span></span>

<span data-ttu-id="1fbad-305">Bien qu’ASP.NET peut généralement déterminer automatiquement un type de données, parfois, il ne peut pas.</span><span class="sxs-lookup"><span data-stu-id="1fbad-305">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="1fbad-306">Par conséquent, vous devrez peut-être aider ASP.NET en effectuant une conversion explicite.</span><span class="sxs-lookup"><span data-stu-id="1fbad-306">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="1fbad-307">Même si vous n’êtes pas obligé de convertir les types, il est parfois utile tester et pour voir quel type de données vous travaillez peut-être avec.</span><span class="sxs-lookup"><span data-stu-id="1fbad-307">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="1fbad-308">Le cas le plus courant est que vous devez convertir une chaîne en un autre type, comme un entier ou une date.</span><span class="sxs-lookup"><span data-stu-id="1fbad-308">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="1fbad-309">L’exemple suivant illustre un cas classique où vous devez convertir une chaîne en nombre.</span><span class="sxs-lookup"><span data-stu-id="1fbad-309">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

<span data-ttu-id="1fbad-310">En règle générale, l’entrée d’utilisateur vous parviennent sous forme de chaînes.</span><span class="sxs-lookup"><span data-stu-id="1fbad-310">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="1fbad-311">Même si vous avez invité des utilisateurs à entrer un numéro, et même s’ils ont entré un chiffre, lorsque l’entrée d’utilisateur est soumise et lisez-le dans le code, les données sont au format de chaîne.</span><span class="sxs-lookup"><span data-stu-id="1fbad-311">Even if you've prompted users to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="1fbad-312">Par conséquent, vous devez convertir la chaîne en nombre.</span><span class="sxs-lookup"><span data-stu-id="1fbad-312">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="1fbad-313">Dans l’exemple, si vous essayez d’effectuer des opérations arithmétiques sur les valeurs sans les convertir, l’erreur suivante se produit, car ASP.NET ne peut pas ajouter deux chaînes :</span><span class="sxs-lookup"><span data-stu-id="1fbad-313">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

<span data-ttu-id="1fbad-314">*Impossible de convertir implicitement le type 'string' en 'int'.*</span><span class="sxs-lookup"><span data-stu-id="1fbad-314">*Cannot implicitly convert type 'string' to 'int'.*</span></span>

<span data-ttu-id="1fbad-315">Pour convertir les valeurs à des entiers, vous appelez le `AsInt` (méthode).</span><span class="sxs-lookup"><span data-stu-id="1fbad-315">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="1fbad-316">Si la conversion est réussie, vous pouvez ensuite ajouter les numéros.</span><span class="sxs-lookup"><span data-stu-id="1fbad-316">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="1fbad-317">Le tableau suivant répertorie les méthodes de conversion et de test habituellement pour les variables.</span><span class="sxs-lookup"><span data-stu-id="1fbad-317">The following table lists some common conversion and test methods for variables.</span></span>

:::row:::
    :::column:::
        <strong>Method</strong>
    :::column-end:::
    :::column:::
        <strong>Description</strong>
    :::column-end:::
    :::column:::
        <strong>Example</strong>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
        Converts a string that represents a whole number (like "593") to an integer.
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
        Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.
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
        Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.
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
        Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number. (In ASP.NET, a decimal number is more precise than a floating-point number.)
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
        Converts a string that represents a date and time value to the ASP.NET `DateTime` type.
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
        Converts any other data type to a string.
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a><span data-ttu-id="1fbad-318">Opérateurs</span><span class="sxs-lookup"><span data-stu-id="1fbad-318">Operators</span></span>

<span data-ttu-id="1fbad-319">Un opérateur est un mot clé ou un caractère qui indique à ASP.NET quel type de commande à effectuer dans une expression.</span><span class="sxs-lookup"><span data-stu-id="1fbad-319">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="1fbad-320">Le langage c# (et la syntaxe Razor qui repose sur ce dernier) prend en charge de nombreux opérateurs, mais il vous suffit de reconnaître les quelques pour commencer.</span><span class="sxs-lookup"><span data-stu-id="1fbad-320">The C# language (and the Razor syntax that's based on it) supports many operators, but you only need to recognize a few to get started.</span></span> <span data-ttu-id="1fbad-321">Le tableau suivant récapitule les opérateurs courants.</span><span class="sxs-lookup"><span data-stu-id="1fbad-321">The following table summarizes the most common operators.</span></span>


:::row:::
    :::column:::
        <strong>Operator</strong>
    :::column-end:::
    :::column:::
        <strong>Description</strong>
    :::column-end:::
    :::column:::
        <strong>Examples</strong>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+` `-` `*` `/`
    :::column-end:::
    :::column:::
        Math operators used in numerical expressions.
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
        Assignment. Assigns the value on the right side of a statement to the object on the left side.
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
        Equality. Returns `true` if the values are equal. (Notice the distinction between the `=` operator and the `==` operator.)
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
        Inequality. Returns `true` if the values are not equal.
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
        Less-than, greater-than, less-than-or-equal, and greater-than-or-equal.
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
        Concatenation, which is used to join strings. ASP.NET knows the difference between this operator and the addition operator based on the data type of the expression.
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
        The increment and decrement operators, which add and subtract 1 (respectively) from a variable.
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
        Dot. Used to distinguish objects and their properties and methods.
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
        Parentheses. Used to group expressions and to pass parameters to methods.
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
        Brackets. Used for accessing values in arrays or collections.
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
        Not. Reverses a `true` value to `false` and vice versa. Typically used as a shorthand way to test for `false` (that is, for not `true`).
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
        Logical AND and OR, which are used to link conditions together.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    :::column-end:::
:::row-end:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="1fbad-322">Utilisation de fichiers et chemins d’accès de dossier dans le Code</span><span class="sxs-lookup"><span data-stu-id="1fbad-322">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="1fbad-323">Souvent, vous allez travailler avec les chemins d’accès de fichier et dossier dans votre code.</span><span class="sxs-lookup"><span data-stu-id="1fbad-323">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="1fbad-324">Voici un exemple de structure de dossiers physiques pour un site Web tel qu’il peut apparaître sur votre ordinateur de développement :</span><span class="sxs-lookup"><span data-stu-id="1fbad-324">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="1fbad-325">Voici quelques détails essentiels sur les URL et les chemins d’accès :</span><span class="sxs-lookup"><span data-stu-id="1fbad-325">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="1fbad-326">Une URL commence par un nom de domaine (`http://www.example.com`) ou un nom de serveur (`http://localhost`, `http://mycomputer`).</span><span class="sxs-lookup"><span data-stu-id="1fbad-326">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="1fbad-327">Une URL correspond à un chemin d’accès physique sur un ordinateur hôte.</span><span class="sxs-lookup"><span data-stu-id="1fbad-327">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="1fbad-328">Par exemple, `http://myserver` peuvent correspondre au dossier *C:\websites\mywebsite* sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="1fbad-328">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="1fbad-329">Un chemin d’accès virtuel est un raccourci pour représenter les chemins d’accès dans le code sans avoir à spécifier le chemin d’accès complet.</span><span class="sxs-lookup"><span data-stu-id="1fbad-329">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="1fbad-330">Il inclut la partie d’une URL qui suit le nom de domaine ou du serveur.</span><span class="sxs-lookup"><span data-stu-id="1fbad-330">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="1fbad-331">Lorsque vous utilisez des chemins d’accès virtuels, vous pouvez déplacer votre code vers un autre domaine ou un serveur sans avoir à mettre à jour les chemins d’accès.</span><span class="sxs-lookup"><span data-stu-id="1fbad-331">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="1fbad-332">Voici un exemple pour vous aider à comprendre les différences :</span><span class="sxs-lookup"><span data-stu-id="1fbad-332">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="1fbad-333">URL complète</span><span class="sxs-lookup"><span data-stu-id="1fbad-333">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="1fbad-334">Nom du serveur</span><span class="sxs-lookup"><span data-stu-id="1fbad-334">Server name</span></span> | <span data-ttu-id="1fbad-335">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="1fbad-335">*mycompanyserver*</span></span> |
| <span data-ttu-id="1fbad-336">Chemin d'accès virtuel</span><span class="sxs-lookup"><span data-stu-id="1fbad-336">Virtual path</span></span> | <span data-ttu-id="1fbad-337">*/humanresources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="1fbad-337">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="1fbad-338">Chemin d’accès physique</span><span class="sxs-lookup"><span data-stu-id="1fbad-338">Physical path</span></span> | <span data-ttu-id="1fbad-339">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="1fbad-339">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="1fbad-340">Est la racine virtuelle /, tout comme la racine de votre lecteur est \.</span><span class="sxs-lookup"><span data-stu-id="1fbad-340">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="1fbad-341">(Chemins d’accès du dossier virtuel utilisent toujours des barres obliques.) Le chemin d’accès virtuel d’un dossier ne doit pas avoir le même nom que le dossier physique ; Il peut être un alias.</span><span class="sxs-lookup"><span data-stu-id="1fbad-341">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="1fbad-342">(Sur les serveurs de production, le chemin d’accès virtuel rarement correspond à un chemin d’accès physique exact.)</span><span class="sxs-lookup"><span data-stu-id="1fbad-342">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="1fbad-343">Lorsque vous travaillez avec des fichiers et dossiers dans le code, parfois, vous devez référencer le chemin d’accès physique et parfois selon les objets que vous travaillez avec un chemin d’accès virtuel.</span><span class="sxs-lookup"><span data-stu-id="1fbad-343">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="1fbad-344">ASP.NET vous fournit ces outils pour l’utilisation des chemins d’accès de fichier et dossier dans le code : le `Server.MapPath` (méthode) et le `~` opérateur et `Href` (méthode).</span><span class="sxs-lookup"><span data-stu-id="1fbad-344">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="1fbad-345">Conversion des chemins d’accès virtuels physiques : la méthode Server.MapPath</span><span class="sxs-lookup"><span data-stu-id="1fbad-345">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="1fbad-346">Le `Server.MapPath` méthode convertit un chemin d’accès virtuel (comme */default.cshtml*) à un chemin d’accès physique absolu (comme *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="1fbad-346">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="1fbad-347">Vous utilisez cette méthode chaque fois que vous avez besoin d’un chemin d’accès physique complet.</span><span class="sxs-lookup"><span data-stu-id="1fbad-347">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="1fbad-348">Un exemple typique est lors de la lecture ou écriture d’un fichier texte ou un fichier image sur le serveur web.</span><span class="sxs-lookup"><span data-stu-id="1fbad-348">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="1fbad-349">En général, vous ne connaissez pas de chemin d’accès physique absolu de votre site sur le serveur d’hébergement d’un site, donc cette méthode peut convertir le chemin d’accès, vous connaissez — le chemin d’accès virtuel, le chemin d’accès correspondant sur le serveur pour vous.</span><span class="sxs-lookup"><span data-stu-id="1fbad-349">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="1fbad-350">Vous passez le chemin d’accès virtuel à un fichier ou dossier à la méthode et retourne le chemin d’accès physique :</span><span class="sxs-lookup"><span data-stu-id="1fbad-350">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="1fbad-351">Faisant référence à la racine virtuelle : le ~ opérateur et Href (méthode)</span><span class="sxs-lookup"><span data-stu-id="1fbad-351">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="1fbad-352">Dans un *.cshtml* ou *.vbhtml* fichier, vous pouvez référencer le chemin d’accès virtuel racine à l’aide du `~` opérateur.</span><span class="sxs-lookup"><span data-stu-id="1fbad-352">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="1fbad-353">Cela est très pratique, car vous pouvez déplacer les pages dans un site, et des liens qu’ils contiennent vers d’autres pages ne sera pas interrompues.</span><span class="sxs-lookup"><span data-stu-id="1fbad-353">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="1fbad-354">Il est également pratique au cas où vous décidez de déplacer votre site Web vers un autre emplacement.</span><span class="sxs-lookup"><span data-stu-id="1fbad-354">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="1fbad-355">Voici quelques exemples :</span><span class="sxs-lookup"><span data-stu-id="1fbad-355">Here are some examples:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

<span data-ttu-id="1fbad-356">Si le site Web est `http://myserver/myapp`, voici la façon dont ASP.NET traite ces chemins d’accès lors de l’exécution de la page :</span><span class="sxs-lookup"><span data-stu-id="1fbad-356">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="1fbad-357">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="1fbad-357">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="1fbad-358">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="1fbad-358">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="1fbad-359">(Vous ne verrez pas ces chemins d’accès en tant que les valeurs de la variable, mais ASP.NET traite les chemins d’accès comme si c’est qu’ils étaient).</span><span class="sxs-lookup"><span data-stu-id="1fbad-359">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="1fbad-360">Vous pouvez utiliser le `~` opérateur à la fois dans le code serveur (comme ci-dessus) et dans le balisage, comme suit :</span><span class="sxs-lookup"><span data-stu-id="1fbad-360">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

<span data-ttu-id="1fbad-361">Dans le balisage, vous utilisez le `~` opérateur pour créer des chemins d’accès aux ressources telles que des fichiers image, d’autres pages web et de fichiers CSS.</span><span class="sxs-lookup"><span data-stu-id="1fbad-361">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="1fbad-362">Lorsque la page s’exécute, ASP.NET recherche via la page (de code et de balisage) et résout tous les `~` références pour le chemin d’accès approprié.</span><span class="sxs-lookup"><span data-stu-id="1fbad-362">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="1fbad-363">Boucles et la logique conditionnelle</span><span class="sxs-lookup"><span data-stu-id="1fbad-363">Conditional Logic and Loops</span></span>

<span data-ttu-id="1fbad-364">Code de serveur ASP.NET vous permet d’effectuer des tâches en fonction des conditions et écrire du code qui se répète les instructions un certain nombre de fois (autrement dit, code qui exécute une boucle).</span><span class="sxs-lookup"><span data-stu-id="1fbad-364">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times (that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="1fbad-365">Conditions de tests</span><span class="sxs-lookup"><span data-stu-id="1fbad-365">Testing Conditions</span></span>

<span data-ttu-id="1fbad-366">Pour tester une condition simple que vous utilisez la `if` instruction, qui retourne true ou false selon un test que vous spécifiez :</span><span class="sxs-lookup"><span data-stu-id="1fbad-366">To test a simple condition you use the `if` statement, which returns true or false based on a test you specify:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

<span data-ttu-id="1fbad-367">Le `if` mot clé commence un bloc.</span><span class="sxs-lookup"><span data-stu-id="1fbad-367">The `if` keyword starts a block.</span></span> <span data-ttu-id="1fbad-368">Le test (condition) figure entre parenthèses et retourne true ou false.</span><span class="sxs-lookup"><span data-stu-id="1fbad-368">The actual test (condition) is in parentheses and returns true or false.</span></span> <span data-ttu-id="1fbad-369">Les instructions qui s’exécutent si le test a la valeur true sont placées entre accolades.</span><span class="sxs-lookup"><span data-stu-id="1fbad-369">The statements that run if the test is true are enclosed in braces.</span></span> <span data-ttu-id="1fbad-370">Un `if` instruction peut inclure un `else` bloc qui spécifie les instructions à exécuter si la condition a la valeur false :</span><span class="sxs-lookup"><span data-stu-id="1fbad-370">An `if` statement can include an `else` block that specifies statements to run if the condition is false:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

<span data-ttu-id="1fbad-371">Vous pouvez ajouter plusieurs conditions à l’aide un `else if` bloc :</span><span class="sxs-lookup"><span data-stu-id="1fbad-371">You can add multiple conditions using an `else if` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

<span data-ttu-id="1fbad-372">Dans cet exemple, si la première condition dans if bloc n’est pas la valeur est true, le `else if` condition est vérifiée.</span><span class="sxs-lookup"><span data-stu-id="1fbad-372">In this example, if the first condition in the if block is not true, the `else if` condition is checked.</span></span> <span data-ttu-id="1fbad-373">Si cette condition est remplie, les instructions dans le `else if` bloc sont exécutées.</span><span class="sxs-lookup"><span data-stu-id="1fbad-373">If that condition is met, the statements in the `else if` block are executed.</span></span> <span data-ttu-id="1fbad-374">Si aucune des conditions sont remplies, les instructions dans le `else` bloc sont exécutées.</span><span class="sxs-lookup"><span data-stu-id="1fbad-374">If none of the conditions are met, the statements in the `else` block are executed.</span></span> <span data-ttu-id="1fbad-375">Vous pouvez ajouter un nombre quelconque d’if else bloque et puis fermer avec une `else` bloquer comme le &quot;tout le reste&quot; condition.</span><span class="sxs-lookup"><span data-stu-id="1fbad-375">You can add any number of else if blocks, and then close with an `else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="1fbad-376">Pour tester un grand nombre de conditions, utilisez un `switch` bloc :</span><span class="sxs-lookup"><span data-stu-id="1fbad-376">To test a large number of conditions, use a `switch` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

<span data-ttu-id="1fbad-377">La valeur à tester est entre parenthèses (dans l’exemple, le `weekday` variable).</span><span class="sxs-lookup"><span data-stu-id="1fbad-377">The value to test is in parentheses (in the example, the `weekday` variable).</span></span> <span data-ttu-id="1fbad-378">Chaque test individuel utilise un `case` instruction se termine par un signe deux-points ( :)).</span><span class="sxs-lookup"><span data-stu-id="1fbad-378">Each individual test uses a `case` statement that ends with a colon (:).</span></span> <span data-ttu-id="1fbad-379">Si la valeur d’un `case` instruction correspond à la valeur de test, le code dans ce bloc case est exécuté.</span><span class="sxs-lookup"><span data-stu-id="1fbad-379">If the value of a `case` statement matches the test value, the code in that case block is executed.</span></span> <span data-ttu-id="1fbad-380">Vous fermez chaque instruction case avec un `break` instruction.</span><span class="sxs-lookup"><span data-stu-id="1fbad-380">You close each case statement with a `break` statement.</span></span> <span data-ttu-id="1fbad-381">(Si vous oubliez d’inclure un saut dans chaque `case` bloquer, le code de l’élément suivant `case` instruction s’exécute également.) Un `switch` bloc a souvent un `default` instruction en tant que le dernier cas pour un &quot;tout le reste&quot; option qui s’exécute si aucun des autres cas sont remplies.</span><span class="sxs-lookup"><span data-stu-id="1fbad-381">(If you forget to include break in each `case` block, the code from the next `case` statement will run also.) A `switch` block often has a `default` statement as the last case for an &quot;everything else&quot; option that runs if none of the other cases are true.</span></span>

<span data-ttu-id="1fbad-382">Le résultat des deux derniers blocs conditionnels affiché dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="1fbad-382">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="1fbad-384">Code de boucle</span><span class="sxs-lookup"><span data-stu-id="1fbad-384">Looping Code</span></span>

<span data-ttu-id="1fbad-385">Souvent, vous devez exécuter les mêmes instructions à plusieurs reprises.</span><span class="sxs-lookup"><span data-stu-id="1fbad-385">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="1fbad-386">Pour cela, en effectuant une boucle.</span><span class="sxs-lookup"><span data-stu-id="1fbad-386">You do this by looping.</span></span> <span data-ttu-id="1fbad-387">Par exemple, vous exécutez souvent les mêmes instructions pour chaque élément dans une collection de données.</span><span class="sxs-lookup"><span data-stu-id="1fbad-387">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="1fbad-388">Si vous savez exactement combien de fois que vous souhaitez effectuer une boucle, vous pouvez utiliser un `for` boucle.</span><span class="sxs-lookup"><span data-stu-id="1fbad-388">If you know exactly how many times you want to loop, you can use a `for` loop.</span></span> <span data-ttu-id="1fbad-389">Ce type de boucle est particulièrement utile pour allant ou le compte à rebours :</span><span class="sxs-lookup"><span data-stu-id="1fbad-389">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

<span data-ttu-id="1fbad-390">La boucle commence par la `for` mot clé, suivie de trois instructions entre parenthèses, chacun termine par un point-virgule.</span><span class="sxs-lookup"><span data-stu-id="1fbad-390">The loop begins with the `for` keyword, followed by three statements in parentheses, each terminated with a semicolon.</span></span>

- <span data-ttu-id="1fbad-391">Dans les parenthèses, la première instruction (`var i=10;`) crée un compteur et l’initialise à 10.</span><span class="sxs-lookup"><span data-stu-id="1fbad-391">Inside the parentheses, the first statement (`var i=10;`) creates a counter and initializes it to 10.</span></span> <span data-ttu-id="1fbad-392">Vous n’êtes pas obligé de nommer le compteur `i` &#8212; vous pouvez utiliser n’importe quelle variable.</span><span class="sxs-lookup"><span data-stu-id="1fbad-392">You don't have to name the counter `i` &#8212; you can use any variable.</span></span> <span data-ttu-id="1fbad-393">Lorsque le `for` boucle s’exécute, le compteur est incrémenté automatiquement.</span><span class="sxs-lookup"><span data-stu-id="1fbad-393">When the `for` loop runs, the counter is automatically incremented.</span></span>
- <span data-ttu-id="1fbad-394">La deuxième instruction (`i < 21;`) définit la condition pour jusqu’où vous souhaitez compter.</span><span class="sxs-lookup"><span data-stu-id="1fbad-394">The second statement (`i < 21;`) sets the condition for how far you want to count.</span></span> <span data-ttu-id="1fbad-395">Dans ce cas, vous le souhaitez pour accéder à un maximum de 20 (autrement dit, continuez tandis que le compteur est inférieure à 21).</span><span class="sxs-lookup"><span data-stu-id="1fbad-395">In this case, you want it to go to a maximum of 20 (that is, keep going while the counter is less than 21).</span></span>
- <span data-ttu-id="1fbad-396">La troisième instruction (`i++` ) utilise un opérateur d’incrémentation, laquelle spécifie simplement que le compteur doit avoir 1 ajouté chaque fois que la boucle s’exécute.</span><span class="sxs-lookup"><span data-stu-id="1fbad-396">The third statement (`i++` ) uses an increment operator, which simply specifies that the counter should have 1 added to it each time the loop runs.</span></span>

<span data-ttu-id="1fbad-397">Les accolades est le code qui s’exécute pour chaque itération de la boucle.</span><span class="sxs-lookup"><span data-stu-id="1fbad-397">Inside the braces is the code that will run for each iteration of the loop.</span></span> <span data-ttu-id="1fbad-398">Le balisage crée un nouveau paragraphe (`<p>` élément) chaque fois, ajoute une ligne à la sortie, en affichant la valeur de `i` (le compteur).</span><span class="sxs-lookup"><span data-stu-id="1fbad-398">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of `i` (the counter).</span></span> <span data-ttu-id="1fbad-399">Lorsque vous exécutez cette page, l’exemple crée 11 lignes affichant la sortie, avec le texte dans chaque ligne indiquant le numéro d’élément.</span><span class="sxs-lookup"><span data-stu-id="1fbad-399">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

<span data-ttu-id="1fbad-401">Si vous travaillez avec une collection ou un tableau, vous utilisez souvent une `foreach` boucle.</span><span class="sxs-lookup"><span data-stu-id="1fbad-401">If you're working with a collection or array, you often use a `foreach` loop.</span></span> <span data-ttu-id="1fbad-402">Une collection est un groupe d’objets similaires et le `foreach` boucle vous permet de transporter une tâche sur chaque élément dans la collection.</span><span class="sxs-lookup"><span data-stu-id="1fbad-402">A collection is a group of similar objects, and the `foreach` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="1fbad-403">Ce type de boucle est pratique pour les collections, car contrairement à un `for` boucle, vous n’êtes pas obligé incrément du compteur ou de définir une limite.</span><span class="sxs-lookup"><span data-stu-id="1fbad-403">This type of loop is convenient for collections, because unlike a `for` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="1fbad-404">Au lieu de cela, le `foreach` code boucle passe simplement via la collection jusqu'à ce qu’elle est terminée.</span><span class="sxs-lookup"><span data-stu-id="1fbad-404">Instead, the `foreach` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="1fbad-405">Par exemple, le code suivant retourne les éléments dans le `Request.ServerVariables` collection, qui est un objet qui contient des informations sur votre serveur web.</span><span class="sxs-lookup"><span data-stu-id="1fbad-405">For example, the following code returns the items in the `Request.ServerVariables` collection, which is an object that contains information about your web server.</span></span> <span data-ttu-id="1fbad-406">Il utilise un `foreac` boucle h pour afficher le nom de chaque élément en créant un nouveau `<li>` élément dans une liste à puces HTML.</span><span class="sxs-lookup"><span data-stu-id="1fbad-406">It uses a `foreac` h loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

<span data-ttu-id="1fbad-407">Le `foreach` mot clé est suivi de parenthèses où vous déclarez une variable qui représente un élément unique dans la collection (dans l’exemple, `var item`), suivi par le `in` mot clé, suivie de la collection que vous souhaitez parcourir.</span><span class="sxs-lookup"><span data-stu-id="1fbad-407">The `foreach` keyword is followed by parentheses where you declare a variable that represents a single item in the collection (in the example, `var item`), followed by the `in` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="1fbad-408">Dans le corps de la `foreach` boucle, vous pouvez accéder à l’élément actuel à l’aide de la variable que vous avez déclaré précédemment.</span><span class="sxs-lookup"><span data-stu-id="1fbad-408">In the body of the `foreach` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor-Img12](introducing-razor-syntax-c/_static/image12.jpg)

<span data-ttu-id="1fbad-410">Pour créer une boucle plus généraliste, utilisez la `while` instruction :</span><span class="sxs-lookup"><span data-stu-id="1fbad-410">To create a more general-purpose loop, use the `while` statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

<span data-ttu-id="1fbad-411">Un `while` boucle commence par la `while` mot clé, suivi de parenthèses, où vous spécifiez la durée pendant laquelle la boucle continue (ici, pour tant que `countNum` est inférieur à 50), puis le bloc à répéter.</span><span class="sxs-lookup"><span data-stu-id="1fbad-411">A `while` loop begins with the `while` keyword, followed by parentheses where you specify how long the loop continues (here, for as long as `countNum` is less than 50), then the block to repeat.</span></span> <span data-ttu-id="1fbad-412">Incrémentent généralement des boucles (ajouter à) ou de décrémentation (Soustraire) une variable ou un objet utilisé pour le comptage.</span><span class="sxs-lookup"><span data-stu-id="1fbad-412">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="1fbad-413">Dans l’exemple, le `+=` opérateur ajoute 1 à `countNum` chaque fois que la boucle s’exécute.</span><span class="sxs-lookup"><span data-stu-id="1fbad-413">In the example, the `+=` operator adds 1 to `countNum` each time the loop runs.</span></span> <span data-ttu-id="1fbad-414">(Pour décrémenter une variable dans une boucle qui compte à rebours, vous utiliseriez l’opérateur de décrémentation `-=`).</span><span class="sxs-lookup"><span data-stu-id="1fbad-414">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`).</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="1fbad-415">Objets et Collections</span><span class="sxs-lookup"><span data-stu-id="1fbad-415">Objects and Collections</span></span>

<span data-ttu-id="1fbad-416">Presque tous les éléments dans un site Web ASP.NET sont un objet, y compris la page web elle-même.</span><span class="sxs-lookup"><span data-stu-id="1fbad-416">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="1fbad-417">Cette section décrit certains objets importants que vous utiliserez fréquemment dans votre code.</span><span class="sxs-lookup"><span data-stu-id="1fbad-417">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="1fbad-418">Objets de page</span><span class="sxs-lookup"><span data-stu-id="1fbad-418">Page Objects</span></span>

<span data-ttu-id="1fbad-419">L’objet de base dans ASP.NET est la page.</span><span class="sxs-lookup"><span data-stu-id="1fbad-419">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="1fbad-420">Vous pouvez accéder à propriétés de l’objet page directement sans aucun objet éligible.</span><span class="sxs-lookup"><span data-stu-id="1fbad-420">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="1fbad-421">Le code suivant obtient le chemin d’accès du fichier de la page, à l’aide de la `Request` objet de la page :</span><span class="sxs-lookup"><span data-stu-id="1fbad-421">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

<span data-ttu-id="1fbad-422">Pour le rendre claire que vous référencez propriétés et des méthodes sur l’objet actuel de la page, vous pouvez éventuellement utiliser le mot clé `this` pour représenter l’objet de page dans votre code.</span><span class="sxs-lookup"><span data-stu-id="1fbad-422">To make it clear that you're referencing properties and methods on the current page object, you can optionally use the keyword `this` to represent the page object in your code.</span></span> <span data-ttu-id="1fbad-423">Voici l’exemple de code précédent, avec `this` ajoutée pour représenter la page :</span><span class="sxs-lookup"><span data-stu-id="1fbad-423">Here is the previous code example, with `this` added to represent the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

<span data-ttu-id="1fbad-424">Vous pouvez utiliser les propriétés de la `Page` objet pour obtenir un grand nombre d’informations, telles que :</span><span class="sxs-lookup"><span data-stu-id="1fbad-424">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="1fbad-425">`Request`.</span><span class="sxs-lookup"><span data-stu-id="1fbad-425">`Request`.</span></span> <span data-ttu-id="1fbad-426">Comme vous l’avez déjà vu, il s’agit d’une collection d’informations sur la requête actuelle, y compris le type de navigateur fait la demande, l’URL de la page, de l’identité de l’utilisateur, etc.</span><span class="sxs-lookup"><span data-stu-id="1fbad-426">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="1fbad-427">`Response`.</span><span class="sxs-lookup"><span data-stu-id="1fbad-427">`Response`.</span></span> <span data-ttu-id="1fbad-428">Il s’agit d’une collection d’informations sur la réponse (page) qui sera envoyée au navigateur une fois le code du serveur en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="1fbad-428">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="1fbad-429">Par exemple, vous pouvez utiliser cette propriété pour écrire des informations dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="1fbad-429">For example, you can use this property to write information into the response.</span></span> 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="1fbad-430">Objets de collection (tableaux et les dictionnaires)</span><span class="sxs-lookup"><span data-stu-id="1fbad-430">Collection Objects (Arrays and Dictionaries)</span></span>

<span data-ttu-id="1fbad-431">Un *collection* est un groupe d’objets du même type, telle qu’une collection de `Customer` objets à partir d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="1fbad-431">A *collection* is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="1fbad-432">ASP.NET contient de nombreux regroupements intégrés, tels que le `Request.Files` collection.</span><span class="sxs-lookup"><span data-stu-id="1fbad-432">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="1fbad-433">Souvent, vous allez travailler avec des données dans des collections.</span><span class="sxs-lookup"><span data-stu-id="1fbad-433">You'll often work with data in collections.</span></span> <span data-ttu-id="1fbad-434">Deux types de collections courants sont le *tableau* et *dictionnaire*.</span><span class="sxs-lookup"><span data-stu-id="1fbad-434">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="1fbad-435">Un tableau est utile lorsque vous souhaitez stocker une collection d’éléments similaires mais ne souhaitez pas créer une variable distincte pour contenir chaque élément :</span><span class="sxs-lookup"><span data-stu-id="1fbad-435">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

<span data-ttu-id="1fbad-436">Les tableaux, vous déclarez un type de données spécifique, tel que `string`, `int`, ou `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="1fbad-436">With arrays, you declare a specific data type, such as `string`, `int`, or `DateTime`.</span></span> <span data-ttu-id="1fbad-437">Pour indiquer que la variable peut contenir un tableau, vous ajoutez des crochets à la déclaration (tel que `string[]` ou `int[]`).</span><span class="sxs-lookup"><span data-stu-id="1fbad-437">To indicate that the variable can contain an array, you add brackets to the declaration (such as `string[]` or `int[]`).</span></span> <span data-ttu-id="1fbad-438">Vous pouvez accéder à des éléments dans un tableau à l’aide de leur position (index) ou en utilisant la `foreach` instruction.</span><span class="sxs-lookup"><span data-stu-id="1fbad-438">You can access items in an array using their position (index) or by using the `foreach` statement.</span></span> <span data-ttu-id="1fbad-439">Index de tableau sont de base zéro &#8212; autrement dit, le premier élément est en position 0, le deuxième élément est à la position 1 et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="1fbad-439">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

<span data-ttu-id="1fbad-440">Vous pouvez déterminer le nombre d’éléments dans un tableau en obtenant son `Length` propriété.</span><span class="sxs-lookup"><span data-stu-id="1fbad-440">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="1fbad-441">Pour obtenir la position d’un élément spécifique dans le tableau (à la recherche dans le tableau), utilisez le `Array.IndexOf` (méthode).</span><span class="sxs-lookup"><span data-stu-id="1fbad-441">To get the position of a specific item in the array (to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="1fbad-442">Vous pouvez également effectuer les opérations comme inverse le contenu d’un tableau (la `Array.Reverse` méthode) ou trier le contenu (la `Array.Sort` méthode).</span><span class="sxs-lookup"><span data-stu-id="1fbad-442">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="1fbad-443">La sortie du code de tableau de chaîne affichée dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="1fbad-443">The output of the string array code displayed in a browser:</span></span>

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

<span data-ttu-id="1fbad-445">Un dictionnaire est une collection de paires clé/valeur, où vous fournissez la clé (ou nom) pour définir ou récupérer la valeur correspondante :</span><span class="sxs-lookup"><span data-stu-id="1fbad-445">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

<span data-ttu-id="1fbad-446">Pour créer un dictionnaire, vous utilisez le `new` mot clé pour indiquer que vous créez un nouvel objet de dictionnaire.</span><span class="sxs-lookup"><span data-stu-id="1fbad-446">To create a dictionary, you use the `new` keyword to indicate that you're creating a new dictionary object.</span></span> <span data-ttu-id="1fbad-447">Vous pouvez affecter un dictionnaire à une variable à l’aide du `var` mot clé.</span><span class="sxs-lookup"><span data-stu-id="1fbad-447">You can assign a dictionary to a variable using the `var` keyword.</span></span> <span data-ttu-id="1fbad-448">Vous indiquez les types de données des éléments dans le dictionnaire à l’aide de crochets pointus ( `< >` ).</span><span class="sxs-lookup"><span data-stu-id="1fbad-448">You indicate the data types of the items in the dictionary using angle brackets ( `< >` ).</span></span> <span data-ttu-id="1fbad-449">À la fin de la déclaration, vous devez ajouter une paire de parenthèses, car il s’agit en fait une méthode qui crée un dictionnaire.</span><span class="sxs-lookup"><span data-stu-id="1fbad-449">At the end of the declaration, you must add a pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="1fbad-450">Pour ajouter des éléments au dictionnaire, vous pouvez appeler la `Add` méthode de la variable de dictionnaire (`myScores` dans ce cas), puis spécifiez une clé et une valeur.</span><span class="sxs-lookup"><span data-stu-id="1fbad-450">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="1fbad-451">Ou bien, vous pouvez utiliser des crochets pour indiquer la clé et d’effectuer l’affectation simple, comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="1fbad-451">Alternatively, you can use square brackets to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

<span data-ttu-id="1fbad-452">Pour obtenir une valeur à partir du dictionnaire, vous spécifiez la clé entre crochets :</span><span class="sxs-lookup"><span data-stu-id="1fbad-452">To get a value from the dictionary, you specify the key in brackets:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="1fbad-453">Appel de méthodes avec des paramètres</span><span class="sxs-lookup"><span data-stu-id="1fbad-453">Calling Methods with Parameters</span></span>

<span data-ttu-id="1fbad-454">Que vous lire plus haut dans cet article, les objets que vous programmez avec peuvent avoir des méthodes.</span><span class="sxs-lookup"><span data-stu-id="1fbad-454">As you read earlier in this article, the objects that you program with can have methods.</span></span> <span data-ttu-id="1fbad-455">Par exemple, un `Database` objet peut avoir un `Database.Connect` (méthode).</span><span class="sxs-lookup"><span data-stu-id="1fbad-455">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="1fbad-456">De nombreuses méthodes ont également un ou plusieurs paramètres.</span><span class="sxs-lookup"><span data-stu-id="1fbad-456">Many methods also have one or more parameters.</span></span> <span data-ttu-id="1fbad-457">Un *paramètre* est une valeur que vous passez à une méthode pour activer la méthode effectuer sa tâche.</span><span class="sxs-lookup"><span data-stu-id="1fbad-457">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="1fbad-458">Par exemple, examinez une déclaration pour le `Request.MapPath` (méthode), qui prend trois paramètres :</span><span class="sxs-lookup"><span data-stu-id="1fbad-458">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

<span data-ttu-id="1fbad-459">(La ligne a été encapsulée pour le rendre plus lisible.</span><span class="sxs-lookup"><span data-stu-id="1fbad-459">(The line has been wrapped to make it more readable.</span></span> <span data-ttu-id="1fbad-460">N’oubliez pas que vous pouvez insérer des sauts de ligne presque partout, sauf à l’intérieur des chaînes qui sont placés entre guillemets.)</span><span class="sxs-lookup"><span data-stu-id="1fbad-460">Remember that you can put line breaks almost any place except inside strings that are enclosed in quotation marks.)</span></span>

<span data-ttu-id="1fbad-461">Cette méthode retourne le chemin d’accès physique sur le serveur qui correspond à un chemin d’accès virtuel spécifié.</span><span class="sxs-lookup"><span data-stu-id="1fbad-461">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="1fbad-462">Les trois paramètres pour la méthode sont `virtualPath`, `baseVirtualDir`, et `allowCrossAppMapping`.</span><span class="sxs-lookup"><span data-stu-id="1fbad-462">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="1fbad-463">(Notez que dans la déclaration, les paramètres sont répertoriés avec les types de données des données qu’ils accepte). Lorsque vous appelez cette méthode, vous devez fournir des valeurs pour tous les trois paramètres.</span><span class="sxs-lookup"><span data-stu-id="1fbad-463">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="1fbad-464">La syntaxe Razor vous propose deux options pour passer des paramètres à une méthode : *paramètres positionnels* et *des paramètres nommés*.</span><span class="sxs-lookup"><span data-stu-id="1fbad-464">The Razor syntax gives you two options for passing parameters to a method: *positional parameters* and *named parameters*.</span></span> <span data-ttu-id="1fbad-465">Pour appeler une méthode à l’aide des paramètres positionnels, vous transmettez les paramètres dans un ordre strict est spécifié dans la déclaration de méthode.</span><span class="sxs-lookup"><span data-stu-id="1fbad-465">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="1fbad-466">(Vous généralement sauriez cet ordre en lisant la documentation de la méthode.) Vous devez suivre l’ordre, et vous ne pouvez pas ignorer les paramètres &#8212; si nécessaire, vous passez une chaîne vide (`""`) ou `null` pour que vous n’avez pas une valeur pour un paramètre de positionnement.</span><span class="sxs-lookup"><span data-stu-id="1fbad-466">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or `null` for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="1fbad-467">L’exemple suivant suppose que vous avez un dossier nommé *scripts* sur votre site Web.</span><span class="sxs-lookup"><span data-stu-id="1fbad-467">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="1fbad-468">Le code appelle la `Request.MapPath` et transmet des valeurs pour les trois paramètres dans l’ordre approprié.</span><span class="sxs-lookup"><span data-stu-id="1fbad-468">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="1fbad-469">Il affiche ensuite le chemin d’accès mappé qui en résulte.</span><span class="sxs-lookup"><span data-stu-id="1fbad-469">It then displays the resulting mapped path.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

<span data-ttu-id="1fbad-470">Quand une méthode possède de nombreux paramètres, vous pouvez conserver votre code plus lisible à l’aide des paramètres nommés.</span><span class="sxs-lookup"><span data-stu-id="1fbad-470">When a method has many parameters, you can keep your code more readable by using named parameters.</span></span> <span data-ttu-id="1fbad-471">Pour appeler une méthode à l’aide de paramètres nommés, spécifiez le nom du paramètre suivi de deux-points ( :)), puis la valeur.</span><span class="sxs-lookup"><span data-stu-id="1fbad-471">To call a method using named parameters, you specify the parameter name followed by a colon (:), and then the value.</span></span> <span data-ttu-id="1fbad-472">L’avantage des paramètres nommés, que vous pouvez les transmettre dans l’ordre que vous souhaitez.</span><span class="sxs-lookup"><span data-stu-id="1fbad-472">The advantage of named parameters is that you can pass them in any order you want.</span></span> <span data-ttu-id="1fbad-473">(Un inconvénient est que l’appel de méthode n’est pas aussi compact.)</span><span class="sxs-lookup"><span data-stu-id="1fbad-473">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="1fbad-474">L’exemple suivant appelle la méthode décrite ci-dessus, mais utilise des paramètres nommés pour fournir les valeurs :</span><span class="sxs-lookup"><span data-stu-id="1fbad-474">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

<span data-ttu-id="1fbad-475">Comme vous pouvez le voir, les paramètres sont passés dans un ordre différent.</span><span class="sxs-lookup"><span data-stu-id="1fbad-475">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="1fbad-476">Toutefois, si vous exécutez l’exemple précédent et cet exemple, ils allons retourner la même valeur.</span><span class="sxs-lookup"><span data-stu-id="1fbad-476">However, if you run the previous example and this example, they'll return the same value.</span></span>

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a><span data-ttu-id="1fbad-477">Gestion des erreurs</span><span class="sxs-lookup"><span data-stu-id="1fbad-477">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="1fbad-478">Instructions Try-Catch</span><span class="sxs-lookup"><span data-stu-id="1fbad-478">Try-Catch Statements</span></span>

<span data-ttu-id="1fbad-479">Vous aurez souvent des instructions dans votre code peut échouer pour des raisons en dehors de votre contrôle.</span><span class="sxs-lookup"><span data-stu-id="1fbad-479">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="1fbad-480">Exemple :</span><span class="sxs-lookup"><span data-stu-id="1fbad-480">For example:</span></span>

- <span data-ttu-id="1fbad-481">Si votre code essaie de créer ou d’accéder à un fichier, toutes sortes d’erreurs peuvent se produire.</span><span class="sxs-lookup"><span data-stu-id="1fbad-481">If your code tries to create or access a file, all sorts of errors might occur.</span></span> <span data-ttu-id="1fbad-482">Le fichier n’existe ne peut-être pas, il est peut-être verrouillé, le code ne peut pas disposer des autorisations et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="1fbad-482">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="1fbad-483">De même, si votre code essaie de mettre à jour des enregistrements dans une base de données, il peut y avoir des problèmes d’autorisations, la connexion à la base de données peut-être être supprimée, les données à enregistrer est peut-être non valide et ainsi de suite.</span><span class="sxs-lookup"><span data-stu-id="1fbad-483">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="1fbad-484">En termes de programmation, ces situations sont appelées *exceptions*.</span><span class="sxs-lookup"><span data-stu-id="1fbad-484">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="1fbad-485">Si votre code rencontre une exception, il génère (lève) un message d’erreur qui est, au mieux, ennuyeux pour les utilisateurs :</span><span class="sxs-lookup"><span data-stu-id="1fbad-485">If your code encounters an exception, it generates (throws) an error message that's, at best, annoying to users:</span></span>

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

<span data-ttu-id="1fbad-487">Dans les situations où votre code peut rencontrer des exceptions et afin d’éviter les messages d’erreur de ce type, vous pouvez utiliser `try/catch` instructions.</span><span class="sxs-lookup"><span data-stu-id="1fbad-487">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `try/catch` statements.</span></span> <span data-ttu-id="1fbad-488">Dans la `try` instruction, vous exécutez le code que vous êtes en train de.</span><span class="sxs-lookup"><span data-stu-id="1fbad-488">In the `try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="1fbad-489">Dans un ou plusieurs `catch` instructions, vous pouvez rechercher propres erreurs (types d’exceptions spécifiques) qui se sont produites.</span><span class="sxs-lookup"><span data-stu-id="1fbad-489">In one or more `catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="1fbad-490">Vous pouvez inclure autant `catch` instructions que vous avez besoin rechercher des erreurs sont prévues.</span><span class="sxs-lookup"><span data-stu-id="1fbad-490">You can include as many `catch` statements as you need to look for errors that you are anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="1fbad-491">Nous vous recommandons d’éviter à l’aide de la `Response.Redirect` méthode dans `try/catch` instructions, car il peut provoquer une exception dans votre page.</span><span class="sxs-lookup"><span data-stu-id="1fbad-491">We recommend that you avoid using the `Response.Redirect` method in `try/catch` statements, because it can cause an exception in your page.</span></span>


<span data-ttu-id="1fbad-492">L’exemple suivant montre une page qui crée un fichier texte à la première demande, puis affiche un bouton qui permet à l’utilisateur d’ouvrir le fichier.</span><span class="sxs-lookup"><span data-stu-id="1fbad-492">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="1fbad-493">L’exemple utilise délibérément un nom de fichier incorrect afin qu’elle entraîne une exception.</span><span class="sxs-lookup"><span data-stu-id="1fbad-493">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="1fbad-494">Le code inclut `catch` instructions pour les deux exceptions possibles : `FileNotFoundException`, ce qui se produit si le nom de fichier est incorrect, et `DirectoryNotFoundException`, ce qui se produit si ASP.NET même Impossible de trouver le dossier.</span><span class="sxs-lookup"><span data-stu-id="1fbad-494">The code includes `catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="1fbad-495">(Vous pouvez ne pas commenter une instruction dans l’exemple pour voir comment elle s’exécute lorsque tout fonctionne correctement.)</span><span class="sxs-lookup"><span data-stu-id="1fbad-495">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="1fbad-496">Si votre code n’a pas gérer l’exception, vous voyez une page d’erreur comme la capture d’écran précédente.</span><span class="sxs-lookup"><span data-stu-id="1fbad-496">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="1fbad-497">Toutefois, le `try/catch` section vous aide à empêcher l’utilisateur de voir ces types d’erreurs.</span><span class="sxs-lookup"><span data-stu-id="1fbad-497">However, the `try/catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="1fbad-498">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="1fbad-498">Additional Resources</span></span>

<span data-ttu-id="1fbad-499">**Programmation avec Visual Basic**</span><span class="sxs-lookup"><span data-stu-id="1fbad-499">**Programming with Visual Basic**</span></span>


[<span data-ttu-id="1fbad-500">Annexe : langage Visual Basic et syntaxe</span><span class="sxs-lookup"><span data-stu-id="1fbad-500">Appendix: Visual Basic Language and Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkId=202908)


<span data-ttu-id="1fbad-501">**Documentation de référence**</span><span class="sxs-lookup"><span data-stu-id="1fbad-501">**Reference Documentation**</span></span>


[<span data-ttu-id="1fbad-502">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1fbad-502">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)

[<span data-ttu-id="1fbad-503">Langage c#</span><span class="sxs-lookup"><span data-stu-id="1fbad-503">C# Language</span></span>](https://msdn.microsoft.com/library/kx37x362.aspx)
