---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
title: Présentation des Pages Web ASP.NET - principes de base de programmation | Microsoft Docs
author: tfitzmac
description: 'Ce didacticiel vous donne une vue d’ensemble de la façon de programme dans ASP.NET Web Pages avec syntaxe Razor. Ce que vous allez apprendre : la syntaxe « Razor » de base que vous utilisez pour la demande de tirage...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/17/2015
ms.topic: article
ms.assetid: 7526ed45-a97d-4e8a-8301-01324ef0eff9
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
msc.type: authoredcontent
ms.openlocfilehash: 3824db99b3313876585fe284c254ac89b256bc8f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383243"
---
<a name="introducing-aspnet-web-pages---programming-basics"></a>Présentation des Pages Web ASP.NET - notions de programmation
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Ce didacticiel vous donne une vue d’ensemble de la façon de programme dans ASP.NET Web Pages avec syntaxe Razor.
> 
> Ce que vous allez apprendre :
> 
> - La syntaxe « Razor » de base que vous utilisez pour la programmation dans ASP.NET Web Pages.
> - Une base en c#, qui est le langage de programmation que vous allez utiliser.
> - Certains concepts de programmation fondamentales pour les Pages Web.
> - Comment installer des packages (composants qui contiennent du code prédéfini) à utiliser avec votre site.
> - Comment utiliser *helpers* pour effectuer des tâches de programmation courantes.
>   
> 
> Fonctionnalités technologies abordées :
> 
> - NuGet et le Gestionnaire de package.
> - Le `Gravatar` helper.


Ce didacticiel est principalement un exercice de présentation de la syntaxe de programmation que vous utiliserez pour ASP.NET Web Pages. Vous allez découvrir *syntaxe Razor* et langage de programmation de code qui est écrit en c#. Vous avez obtenu un aperçu de cette syntaxe dans le didacticiel précédent ; Dans ce didacticiel, nous expliquerons la syntaxe plus.

Nous vous assurons que ce didacticiel implique le meilleur parti de programmation que vous verrez dans un didacticiel unique et qu’il s’agit du didacticiel uniquement qui est *uniquement* sur la programmation. Dans les didacticiels restants dans ce jeu, vous créerez réellement des pages de faire des choses intéressantes.

Vous allez également découvrir *helpers*. Une application d’assistance est un composant, une partie des empaqueté de code, que vous pouvez ajouter à une page. L’application d’assistance effectue le travail pour vous qui sinon peut être fastidieuse ou difficile de le faire manuellement.

## <a name="creating-a-page-to-play-with-razor"></a>Création d’une Page de jouer avec Razor

Dans cette section vous allez jouer un peu avec Razor afin d’obtenir une idée de la syntaxe de base.

Démarrez WebMatrix si elle n’est pas déjà en cours d’exécution. Vous allez utiliser le site Web que vous avez créé dans le didacticiel précédent ([mise en route avec Web Pages](https://go.microsoft.com/fwlink/?LinkId=251578)). Pour la rouvrir, cliquez sur **Mes Sites** et choisissez **WebPageMovies**:

![Écran d’accueil de WebMatrix montrant les options d’ouvrir le Site et Mes Sites mis en surbrillance](intro-to-web-pages-programming/_static/image1.png)

Sélectionnez le **fichiers** espace de travail.

Dans le ruban, cliquez sur **New** pour créer une page. Sélectionnez **CSHTML** et nommez la nouvelle page *TestRazor.cshtml*.

Cliquez sur **OK**.

Copiez ce qui suit dans le fichier, remplacer complètement qu’il est déjà.

> [!NOTE]
> Lorsque vous copiez balisage ou code dans les exemples dans une page, la mise en retrait et l’alignement peut-être pas les mêmes que dans le didacticiel. Mise en retrait et l’alignement n’affectent pas l’exécution du code, cependant.


[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample1.cshtml)]

## <a name="examining-the-example-page"></a>Examen de l’exemple de Page

La majeure partie de ce que vous voyez est au format HTML ordinaire. Toutefois, en haut est ce bloc de code :

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample2.cshtml)]

Notez que les opérations suivantes sur ce bloc de code :

- Le caractère « @ » indique à ASP.NET que ce qui suit est code Razor, pas le code HTML. ASP.NET traite tout ce qui suit le caractère en tant que code jusqu'à ce qu’il s’exécute à nouveau en HTML « @ ». (Dans ce cas, c’est le &lt;! DOCTYPE&gt; élément.
- Les accolades ({et}) délimitent un bloc de code Razor si le code n’a plus d’une ligne. Les accolades indiquent à ASP.NET où le code pour ce bloc commence et se termine.
- Les / / caractères marquent un commentaire, autrement dit, une partie du code qui n’est pas exécutée.
- Chaque instruction doit se terminer par un point-virgule ( ;). (Pas de commentaires, cependant.)
- Vous pouvez stocker des valeurs dans *variables*, que vous créez (*déclarer*) avec le mot clé var. Lorsque vous créez une variable, lui donner un nom, qui peut inclure des lettres, des chiffres et des traits de soulignement (\_). Les noms de variable ne peut pas commencer par un chiffre et ne peut pas utiliser le nom d’un mot-clé de programmation (par exemple, var).
- Vous placez entre guillemets les chaînes de caractères (par exemple, « ASP.NET » et « Pages Web »). (Ils doivent être entre guillemets.) Nombres ne sont pas entre guillemets.
- Peu d’espace blanc en dehors des guillemets. Sauts de ligne principalement n’a pas d’importance ; l’exception est que vous ne pouvez fractionner une chaîne entre guillemets sur plusieurs lignes. Mise en retrait et l’alignement n’a pas d’importance.

Quelque chose qui n’est pas évident au vu de cet exemple est que tout le code respecte la casse. Cela signifie que la somme de variable est une variable différente que les variables qui peut être nommée somme ou la somme. De même, var est un mot clé, mais Var n’est pas.

### <a name="objects-and-properties-and-methods"></a>Objets et les propriétés et méthodes

Il est l’expression DateTime.Now. En termes simples, DateTime est un *objet*. Un objet est une chose que vous pouvez programmer avec — une page, une zone de texte, un fichier, une image, une requête web, un message électronique, un enregistrement de client, etc. Les objets ont une ou plusieurs *propriétés* qui décrivent leurs caractéristiques. Un objet de zone de texte a une propriété de texte (entre autres), un objet de requête a une propriété d’Url (et autres), un message électronique a une propriété From et une propriété To, et ainsi de suite. Objets possèdent également *méthodes* qui sont « verbes » qu’ils peuvent effectuer. Vous allez travailler avec des objets beaucoup.

Comme vous pouvez le voir à partir de l’exemple, DateTime est un objet qui vous permet de programme des dates et heures. Il a une propriété nommée maintenant qui retourne la date et heure actuelles.

### <a name="using-code-to-render-markup-in-the-page"></a>À l’aide de code pour restituer le balisage dans la page

Dans le corps de la page, notez les points suivants :

[!code-html[Main](intro-to-web-pages-programming/samples/sample3.html)]

Là encore, le caractère « @ » indique à ASP.NET que ce qui suit est le code, pas le code HTML. Dans le balisage vous pouvez ajouter suivie d’une expression de code, et ASP.NET restitue la valeur de ce droit de l’expression à ce stade. Dans l’exemple, @a rendra tout ce qui est la valeur de la variable nommée a, @product restitue tout ce qui se trouve dans le produit nommé variable et ainsi de suite.

Vous n’êtes pas limité à des variables, cependant. Dans certains cas, le caractère « @ » précède une expression :

- @(un\*b) rend le produit de tout ce qui est dans les variables d’un et b. (Le \* opérateur : multiplication.)
- @(technologie + "" + produit) affiche les valeurs de la technologie de variables et le produit après les concaténer et ajout d’un espace entre les deux. L’opérateur (+) pour concaténer des chaînes est identique à l’opérateur pour l’ajout de nombres. ASP.NET peuvent généralement indiquer si vous travaillez avec des numéros ou avec des chaînes et prend la bonne décision avec l’opérateur « + ».
- @Request.Url restitue la propriété Url de l’objet de requête. L’objet de requête contient des informations sur la requête actuelle à partir du navigateur, et bien sûr la propriété Url contient l’URL de la demande actuelle.

L’exemple est également conçu pour montrer que vous pouvez fonctionnent de différentes façons. Vous pouvez effectuer des calculs dans le bloc de code en haut, placer les résultats dans une variable et rendre ensuite la variable dans le balisage. Ou vous pouvez effectuer des calculs dans un droit de l’expression dans le balisage. L’approche que vous utilisez dépend de ce que vous faites et, dans une certaine mesure, de vos propres préférences.

### <a name="seeing-the-code-in-action"></a>Voir le code en action

Cliquez sur le nom du fichier, puis choisissez **lancer dans le navigateur**. Vous voyez la page dans le navigateur avec toutes les valeurs et les expressions résolues dans la page.

![Page 'TestRazor' en cours d’exécution dans le navigateur](intro-to-web-pages-programming/_static/image2.png)

Recherchez la source dans le navigateur.

![Source de la page « Test Razor » dans le navigateur](intro-to-web-pages-programming/_static/image3.png)

Comme prévu à partir de votre expérience dans le didacticiel précédent, le code Razor est dans la page. Vous ne voyez que sont les valeurs de l’affichage réel. Lorsque vous exécutez une page, vous établissez en fait une demande au serveur web intégré à WebMatrix. Lorsque la demande est reçue, ASP.NET résout toutes les valeurs et les expressions et affiche leurs valeurs dans la page. Il envoie ensuite la page au navigateur.

> [!TIP] 
> 
> **Razor et c#**
> 
> Jusqu'à présent, nous avons dit que vous travaillez avec la syntaxe Razor. C’est vrai, mais il n’est pas terminée. Le langage de programmation réel que vous utilisez est appelé *c#*. C# a été créé par Microsoft plus de dix ans plus tôt et est devenu l’un des langages de programmation principales pour la création d’applications de Windows. Toutes les règles que vous avez vu comment nommer une variable et comment créer des instructions et ainsi de suite sont réellement toutes les règles du langage c#.
> 
> Razor fait référence plus précisément à l’ensemble des conventions de la façon dont vous incorporez ce code dans une page. Par exemple, la convention de l’utilisation de pour marquer du code dans la page et à l’aide de @ {} pour incorporer un bloc de code est l’aspect de Razor d’une page. Programmes d’assistance sont également considérés comme partie de Razor. Syntaxe Razor est utilisée dans des emplacements plus élevée qu’uniquement dans ASP.NET Web Pages. (Par exemple, il est utilisé dans les affichages ASP.NET MVC.)
> 
> Nous y fait allusion car si vous recherchez des informations sur la programmation ASP.NET Web Pages, vous trouverez un grand nombre de références à Razor. Toutefois, un grand nombre de ces références ne s’appliquent pas à ce que vous êtes cela et pouvez donc être déroutant. Et en fait, la plupart de vos questions sur la programmation vraiment vont être sur l’utilisation de c# ou de travailler avec ASP.NET. Par conséquent, si vous recherchez spécifiquement les informations sur Razor, vous ne trouviez pas les réponses que vous avez besoin.


## <a name="adding-some-conditional-logic"></a>Ajout d’une logique conditionnelle

Une des fonctionnalités sur l’utilisation de code dans une page est que vous pouvez modifier ce qui se passe en fonction de différentes conditions. Dans cette partie du didacticiel, vous allez expérimenter certains moyens de modifier ce qui est affiché dans la page.

L’exemple sera simple et quelque peu inventé afin que nous pouvons nous concentrer sur la logique conditionnelle. La page que vous allez créer le fera :

- Afficher un texte différent sur la page selon s’il s’agit de la première fois que la page s’affiche, ou si vous avez cliqué un bouton pour envoyer la page. Il s’agit du premier test conditionnel.
- Afficher le message uniquement si une certaine valeur est passée dans la chaîne de requête de l’URL (http://...?show=true). Il s’agit du second test conditionnel.

Dans WebMatrix, créez une page et nommez-le *TestRazorPart2.cshtml*. (Dans le ruban, cliquez sur **New**, choisissez **CSHTML**, nommez le fichier, puis cliquez sur **OK**.)

Remplacez le contenu de cette page avec les éléments suivants :

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample4.cshtml)]

Le bloc de code en haut Initialise une variable nommée message contenant du texte. Dans le corps de la page, le contenu de la variable de message s’affiche à l’intérieur d’un &lt;p&gt; élément. Le balisage contient également un &lt;d’entrée&gt; élément pour créer un **envoyer** bouton.

Exécutez la page pour voir comment il fonctionne maintenant. Pour l’instant, il est en fait une page statique, même si vous cliquez sur le **envoyer** bouton.

Revenez dans WebMatrix. À l’intérieur du bloc de code, ajoutez le code en surbrillance suivant *après* la ligne qui initialise le message :

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample5.cshtml?highlight=4-6)]

### <a name="the-if---block"></a>If {} bloc

Ce que vous venez d’ajouter a été un if condition. Dans le code, le si la condition a une structure similaire à ceci :

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample6.cs)]

La condition à tester est entre parenthèses. Il doit être une valeur ou une expression qui retourne true ou false. Si la condition est true, ASP.NET exécute l’instruction ou les instructions qui se trouvent dans les accolades. (Ce sont les *puis* dans le cadre de la *if-then* logique.) Si la condition est false, le bloc de code est ignoré.

Voici quelques exemples de conditions que vous pouvez tester dans une instruction if instruction :

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample7.cs)]

Vous pouvez tester les variables par rapport aux valeurs ou par rapport aux expressions en utilisant un <em>opérateur logique</em> ou <em>opérateur de comparaison</em>: égal à (==), supérieur à (&gt;), inférieur à (&lt;), supérieur ou égal à (&gt;=) et inférieur ou égal à (&lt;=). Le ! = signifie d’opérateur non égal à, par exemple, si (un ! = 0) signifie <em>si</em> <em>un</em><em>n’est pas égal à 0</em>.

> [!NOTE]
> Assurez-vous que vous remarquerez que l’opérateur de comparaison égal à (==) n’est pas le même que =. Le = opérateur est utilisé uniquement pour affecter des valeurs (var un = 2). Si vous mélangez ces opérateurs, vous obtenez une erreur ou vous obtiendrez des résultats inattendus.


Pour vérifier si quelque chose a la valeur true, la syntaxe complète est if(IsDone == true). Mais vous pouvez également utiliser le raccourci if(IsDone). S’il n’existe aucun opérateur de comparaison, ASP.NET suppose que vous testez la valeur true.

Le ! opérateur en soi signifie un NOT logique. Par exemple, la condition if ( ! IsPost) signifie *si IsPost n’est pas remplie*.

Vous pouvez combiner des conditions à l’aide d’une opération AND logique (&amp; &amp; opérateur) ou OR logique (|| opérateur). Par exemple, le dernier if conditions dans les moyens d’obtenir des exemples précédents *si FileProcessingIsDone n’est pas définie sur true AND displayMessage est définie sur false*.

### <a name="the-else-block"></a>Le bloc « else »

Une dernière chose sur if blocs : un si le bloc peut être suivi d’un bloc « else ». Un bloc else est utile est d’avoir à exécuter un code différent lorsque la condition est false. Voici un exemple simple :

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample8.cs)]

Vous verrez des exemples dans d’autres didacticiels de cette série dans lequel l’à l’aide d’un bloc else est utile.

### <a name="testing-whether-the-request-is-a-submit-post"></a>Test d’existence de la demande est un envoi (post)

Il existe plus, mais nous allons revenir à l’exemple, ce qui a le if(IsPost) condition {...}. IsPost est en réalité une propriété de la page actuelle. La première fois la page est demandée, IsPost retourne false. Toutefois, si vous cliquez sur un bouton ou que vous envoyez la page, autrement dit, vous le validez — IsPost retourne la valeur true. Par conséquent, IsPost vous permet de déterminer si vous avez affaire à une soumission de formulaire. (En termes de verbes HTTP, si la demande est une opération GET, IsPost retourne false. Si la demande est une opération POST, IsPost retourne la valeur true.) Dans un prochain didacticiel, vous allez utiliser des formulaires d’entrée, où ce test s’avère particulièrement utile.

Exécutez la page. Étant donné que c’est la première fois vous êtes demandé la page, vous voyez « Il s’agit de la première fois que vous avez demandé la page ». Cette chaîne est la valeur que vous avez initialisé à la variable de message. Il existe un test if(IsPost), mais qui retourne la valeur false pour le moment, afin de bloquer le code à l’intérieur d’if ne s’exécute pas.

Cliquez sur le **envoyer** bouton. La page est demandée à nouveau. Comme avant, la variable de message est définie à « C’est la première fois... ». Mais cette fois-ci, le test if(IsPost) retourne la valeur true pour que le code à l’intérieur d’if bloque s’exécute. Le code modifie la valeur de la variable de message à une valeur différente, qui est ce qui est rendu dans le balisage.

Ajoutez maintenant un if condition dans le balisage. Ci-dessous le &lt;p&gt; élément qui contient le **envoyer** bouton, ajoutez le balisage suivant :

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample9.cshtml)]

Vous ajoutez le code au sein du balisage, donc vous commencez par @. Il y a un if test similaire à celle que vous ajoutées précédemment dans le bloc de code. Les accolades, cependant, vous ajoutez HTML ordinaire, au moins, il est ordinaire jusqu'à ce qu’elle arrive au @DateTime.Now. Il s’agit d’un autre peu de code Razor, donc vous devez ajouter placés devant lui.

L’idée ici est que vous pouvez ajouter si les conditions à la fois dans le bloc de code en haut et dans le balisage. Si vous utilisez une instruction if condition dans le corps de la page, les lignes à l’intérieur du bloc peut être balisage ou code. Dans ce cas, et comme chaque fois que vous mélanger le balisage et le code, vous devez utiliser pour indiquer clairement à ASP.NET où le code est.

Exécutez la page et cliquez sur **envoyer**. Cette fois-ci vous permet de voir un message différent lorsque vous envoyez (« maintenant vous avez envoyé... »), mais vous voyez un nouveau message qui indique la date et l’heure.

![Page « Test Razor 2 » en cours d’exécution dans le navigateur avec un horodateur affichant après envoi](intro-to-web-pages-programming/_static/image4.png)

### <a name="testing-the-value-of-a-query-string"></a>Tester la valeur d’une chaîne de requête

Un test supplémentaire. Cette fois-ci, vous allez ajouter une instruction if bloc qui teste une valeur nommée show qui peut-être être transmis dans la chaîne de requête. (Comme ceci : `http://localhost:43097/TestRazorPart2.cshtml?show=true`) vous modifierez la page afin que le message vous avez été affichage (« C’est la première fois... », etc.) s’affiche uniquement si la valeur d’émission est true.

Le bas (mais à l’intérieur) sur le bloc de code en haut de la page, ajoutez ce qui suit :

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample10.cs)]

L’apparence de désormais de bloc de code complet comme dans l’exemple suivant. (N’oubliez pas que lorsque vous copiez le code dans votre page, la mise en retrait peut différer. Mais qui n’affecte pas l’exécution du code.)

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample11.cshtml)]

Le nouveau code dans le bloc Initialise une variable nommée showMessage sur false. Il effectue ensuite une instruction if test pour rechercher une valeur dans la chaîne de requête. Lorsque vous demandez tout d’abord la page, il possède une URL similaire à celle-ci :

`http://localhost:43097/TestRazorPart2.cshtml`

Le code détermine si l’URL contient une variable nommée s’affichent dans la chaîne de requête, telles que cette version de l’URL :

`http://localhost:43097/TestRazorPart2.cshtml`? Afficher = true

Le test lui-même examine la propriété de chaîne de requête de l’objet de demande. Si la chaîne de requête contient un élément nommé show, et si cet élément est défini sur true, if bloc s’exécute et définit la variable showMessage sur true.

Il existe une astuce ici, comme vous pouvez le voir. Comme son nom l’indique, la chaîne de requête est une chaîne. Toutefois, vous pouvez uniquement tester true et false si la valeur que vous testez est la valeur booléenne (true/false). Avant de pouvoir tester la valeur de la variable de l’afficher dans la chaîne de requête, vous devez convertir en une valeur booléenne. C’est ce que fait la méthode AsBool, elle prend une chaîne comme entrée et le convertit en une valeur booléenne. Clairement, si la chaîne est « true », la méthode AsBool convertit cette valeur sur true. Si la valeur de la chaîne n’est rien d’autre, AsBool retourne la valeur false.

> [!TIP] 
> 
> **Types de données et les méthodes As()**
> 
> Nous avons seulement dit que jusqu'à présent, lorsque vous créez une variable, utiliser le mot clé var. N’est pas tout l’article, cependant. Afin de manipuler les valeurs, pour ajouter des numéros, ou concaténer des chaînes, ou comparer les dates ou tester pour la valeur true/false, c# doit fonctionner avec une représentation interne appropriée de la valeur. C# peut *généralement* déterminer ce que cette représentation doit être (autrement dit, ce qui *type* les données sont) en fonction de ce que vous faites avec les valeurs. Maintenant, puis, cependant, il ne peut pas le faire. Si ce n’est pas le cas, vous devez aider en explicitement indiquant comment c# doit représenter des données. La méthode AsBool fait, il indique à c# qu’une valeur de chaîne « true » ou « false » doit être traitée comme une valeur booléenne. Il existe des méthodes similaires pour représenter des chaînes comme d’autres types, tels que AsInt (considérer comme un entier), AsDateTime (considérer comme une date/heure), AsFloat (considérer comme un nombre à virgule flottante) et ainsi de suite. Lorsque vous utilisez en tant que méthodes (), si c# ne peut pas représenter la valeur de chaîne comme demandé, vous verrez une erreur.


Dans le balisage de la page, supprimez ou commentez cet élément (ici il est indiqué comme commenté) :

[!code-html[Main](intro-to-web-pages-programming/samples/sample12.html)]

Right où vous supprimées ou commentée de ce texte, ajoutez le code suivant :

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample13.cshtml)]

Le test que si la variable showMessage est true, le rendu en revanche, si un &lt;p&gt; élément avec la valeur de la variable de message.

### <a name="summary-of-your-conditional-logic"></a>Résumé de votre logique conditionnelle

Si vous n’êtes pas tout à fait certain de ce que vous venez de faire, Voici un résumé.

- La variable de message est initialisée à une chaîne par défaut (« Ceci est la première fois... »).
- Si la demande de page est le résultat d’un envoi (post), la valeur du message est modifiée pour « maintenant vous avez envoyé... »
- La variable showMessage est initialisée sur false.
- Si la chaîne de requête contient ? afficher = true, la variable showMessage est définie sur true.
- Dans le balisage, si showMessage est true, un &lt;p&gt; élément est rendu qui affiche la valeur du message. (Si showMessage a la valeur false, rien n’est restitué à ce stade dans le balisage.)
- Dans le balisage, si la demande est une publication, un &lt;p&gt; élément est rendu qui affiche la date et l’heure.

Exécutez la page. Il n’est aucun message, car showMessage est false, par conséquent, dans le balisage, le test if(showMessage) retourne false.

Cliquez sur **Envoyer**. Affiche la date et heure, mais toujours aucun message.

Dans votre navigateur, accédez à la zone URL et ajoutez le code suivant à la fin de l’URL : ? afficher = true et appuyez sur ENTRÉE.

![Page de « Test Razor 2 » dans le navigateur affichant la chaîne de requête](intro-to-web-pages-programming/_static/image5.png)

La page s’affiche à nouveau. (Étant donné que vous avez modifié l’URL, c’est une nouvelle demande, pas un envoi.) Cliquez sur **envoyer** à nouveau. Le message s’affiche à nouveau, en l’état de la date et l’heure.

![La page « Test Razor 2 » après envoyer lorsqu’il est une chaîne de requête](intro-to-web-pages-programming/_static/image6.png)

Dans l’URL, remplacez ? afficher = true pour ? afficher = false et appuyez sur ENTRÉE. Soumettre à nouveau la page. La page est à la façon dont vous avez démarré, aucun message.

Comme indiqué précédemment, la logique de cet exemple est un peu inventé. Toutefois, s’il va s’afficher dans la plupart de vos pages, et il vous faudra un ou plusieurs des formes que vous avez vu ici.

## <a name="installing-a-helper-displaying-a-gravatar-image"></a>Installation d’un Helper (affichage d’une Image Gravatar)

Certaines tâches que les gens souhaitent souvent à faire sur les pages web ont besoin de beaucoup de code ou requièrent des connaissances supplémentaires. Exemples : affichage d’un graphique pour les données ; placer un Facebook bouton « Like » sur une page ; envoi d’e-mails à partir de votre site Web ; rognage ou redimensionnement d’images ; à l’aide de PayPal pour votre site. Pour le rendre facile à réaliser ce types d’éléments différents, les Pages Web ASP.NET vous permet d’utiliser *helpers*. Programmes d’assistance sont des composants que vous installez pour un site et qui vous permettent d’effectuer des tâches courantes à l’aide de quelques lignes de code Razor.

Les Pages Web ASP.NET a quelques helpers intégrés. Toutefois, nombreux helpers sont disponibles dans les packages (compléments) qui sont fournies à l’aide du Gestionnaire de package NuGet. NuGet vous permet de sélectionner un package à installer, et il s’occupe ensuite de tous les détails de l’installation.

Dans cette partie du didacticiel, vous allez installer un programme d’assistance qui vous permet d’afficher une image Gravatar (« avatar mondialement reconnue »). Vous allez découvrir deux choses. Une consiste à rechercher et installer un programme d’assistance. Vous apprendrez également comment une application auxiliaire permet de facilement faire quelque chose que vous devriez sinon faire utiliser beaucoup de code, que vous seriez obligé d’écrire vous-même.

Vous pouvez inscrire votre propre Gravatar sur le site Web Gravatar à [ http://www.gravatar.com/ ](http://www.gravatar.com/), mais il n’est pas essentiel pour créer un compte Gravatar pour effectuer cette partie du didacticiel.

Dans WebMatrix, cliquez sur le **NuGet** bouton.

![Boîte de dialogue galerie NuGet dans WebMatrix](intro-to-web-pages-programming/_static/image7.png)

Cela lance le Gestionnaire de package NuGet et affiche les packages disponibles. (Tous les packages sont des programmes d’assistance ; certaines ajoutent des fonctionnalités à WebMatrix, certaines sont des modèles supplémentaires et ainsi de suite.) Vous pouvez obtenir un message d’erreur sur l’incompatibilité de version. Vous pouvez ignorer ce message d’erreur en cliquant sur **OK** et poursuivre ce didacticiel.

![Boîte de dialogue galerie NuGet dans WebMatrix](intro-to-web-pages-programming/_static/image8.png)

Dans la zone de recherche, entrez « helpers asp.net ». NuGet affiche les packages correspondant aux termes de recherche.

![Galerie NuGet dans WebMatrix montrant les packages](intro-to-web-pages-programming/_static/image9.png)

La bibliothèque de programmes d’assistance ASP.NET Web contient le code pour simplifier de nombreuses tâches courantes, notamment l’utilisation d’images Gravatar. Sélectionnez le **ASP.NET Web Helpers Library** du package, puis cliquez sur **installer** pour lancer le programme d’installation. Sélectionnez **Oui** lorsque vous êtes invité à installer le package, puis acceptez les conditions pour terminer l’installation.

C'est tout ! NuGet télécharge et installe tout, y compris tous les autres composants qui peuvent être requises (*dépendances*).

Si vous devez désinstaller un programme d’assistance pour une raison quelconque, le processus est très similaire. Cliquez sur le **NuGet** bouton, cliquez sur le **installé** onglet et sélectionnez le package que vous souhaitez désinstaller.

## <a name="using-a-helper-in-a-page"></a>À l’aide d’un programme d’assistance dans une Page

Maintenant, vous allez utiliser l’application d’assistance que vous venez d’installer. Le processus d’ajout d’une assistance à une page est similaire pour la plupart des programmes d’assistance.

Dans WebMatrix, créez une page et nommez-le *GravatarTest.cshml*. (Vous créez une page spéciale pour tester l’application d’assistance, mais vous pouvez utiliser des programmes d’assistance dans n’importe quelle page de votre site.)

À l’intérieur de la &lt;corps&gt; élément, ajoutez un &lt;div&gt; élément. À l’intérieur de la &lt;div&gt; élément, tapez ceci :

@Gravatar.

Le caractère « @ » est le même caractère que vous avez utilisé pour marquer le code Razor. **Gravatar** est l’objet d’assistance qui vous travaillez.

Dès que vous tapez le point (.), WebMatrix affiche une liste de *méthodes* (fonctions) que l’application d’assistance Gravatar rend disponible :

![Liste d’assistance déroulante IntelliSense de Gravatar](intro-to-web-pages-programming/_static/image10.png)

Cette fonctionnalité est appelée *IntelliSense*. Il vous aide à vous codez en fournissant des choix appropriés au contexte. IntelliSense fonctionne avec HTML, CSS, code ASP.NET, JavaScript et autres langages sont pris en charge dans WebMatrix. C’est une autre fonctionnalité qui facilite le développement de pages web dans WebMatrix.

Appuyez sur G sur le clavier et que vous consultez que IntelliSense détecte que la méthode GetHtml. Appuyez sur Tab. IntelliSense insère la méthode sélectionnée (GetHtml) pour vous. Tapez une parenthèse ouvrante et notez que la parenthèse fermante est ajoutée automatiquement. Guillemets entre les deux parenthèses, tapez votre adresse de messagerie. Si vous avez un compte Gravatar, votre photo de profil s’affichera. Si vous n’avez pas un compte Gravatar, une image par défaut est retournée. Lorsque vous avez terminé, la ligne se présente comme suit :

[!code-css[Main](intro-to-web-pages-programming/samples/sample14.css)]

Maintenant afficher la page dans un navigateur. Votre image ou l’image par défaut s’affiche, selon que vous avez un compte Gravatar.

![Gravatar](intro-to-web-pages-programming/_static/image11.png) ![image par défaut](intro-to-web-pages-programming/_static/image12.png)

Pour obtenir une idée de ce que fait l’application d’assistance pour vous, afficher la source de la page dans le navigateur. En même temps que le code HTML que vous aviez dans votre page, vous voyez un élément d’image qui inclut un identificateur. Voici le code que l’application d’assistance rendue dans la page à l’endroit où vous aviez @Gravatar.GetHtml. Le programme d’assistance a pris les informations fournies et a généré le code qui communique directement avec les Gravatar afin de récupérer l’image correcte pour le compte fourni.

La méthode GetHtml vous permet également de personnaliser l’image en fournissant des autres paramètres. Le code suivant montre comment demander une image a une largeur et la hauteur de 40 pixels et utilise une image par défaut spécifiée nommée **wavatar** si le compte spécifié n’existe pas.

[!code-javascript[Main](intro-to-web-pages-programming/samples/sample15.js)]

Ce code produit quelque chose comme le résultat suivant (l’image par défaut varient au hasard).

![](intro-to-web-pages-programming/_static/image13.png)

## <a name="coming-up-next"></a>Prochaine

Pour conserver ce court didacticiel, nous avons dû vous concentrer sur quelques notions de base uniquement. Naturellement, il existe un *beaucoup* plus Razor et c#. Vous allez découvrir plus en tant que vous parcourez ces didacticiels. Si vous souhaitez en savoir plus sur les aspects de programmation de Razor et c# dès maintenant, vous pouvez lire une présentation plus poussée ici : [Introduction à ASP.NET Web Programming à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkID=202890).

Le didacticiel suivant vous présente à travailler avec une base de données. Dans ce didacticiel, vous allez commencer à créer l’exemple d’application qui permet de répertorier vos films préférés.

## <a name="complete-listing-for-testrazor-page"></a>Liste complète de la Page de TestRazor

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample16.cshtml)]

## <a name="complete-listing-for-testrazorpart2-page"></a>Liste complète de la Page de TestRazorPart2

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample17.cshtml)]

## <a name="complete-listing-for-gravatartest-page"></a>Liste complète de la Page de GravatarTest

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample18.cshtml)]

## <a name="additional-resources"></a>Ressources supplémentaires

- [Introduction à la programmation Web ASP.NET à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Application auxiliaire Twitter](../../ui-layouts-and-themes/twitter-helper.md)

> [!div class="step-by-step"]
> [Précédent](getting-started.md)
> [Suivant](displaying-data.md)
