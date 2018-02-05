---
title: "Tag helpers dans ASP.NET Core"
author: rick-anderson
description: "Découvrez ce que sont les Tag helpers et comment les utiliser dans ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/intro
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3c198ccc3e3e2c11f3e2b9379bc63bd6428dbf69
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-tag-helpers-in-aspnet-core"></a>Introduction aux Tag helpers dans ASP.NET Core 

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-are-tag-helpers"></a>Que sont les Tag helpers ?

Les Tag helpers permettent au code côté serveur de participer à la création et au rendu des éléments HTML dans les fichiers Razor. Par exemple, la fonction intégrée `ImageTagHelper` peut ajouter un numéro de version pour le nom de l’image. A chaque modification de l’image, le serveur génère une nouvelle version unique pour l’image, pour que les clients soient assurés de bénéficier de l’image actuelle (au lieu d’une image  obsolète mise en cache). Il existe de nombreux Tag helpers intégrés pour les tâches courantes - telles que la création de formulaires, des liens, le chargement de ressources et plus - et encore plus disponibles dans les référentiels GitHub publics et en tant que NuGet packages. Les Tag helpers sont créés en c#, et ils ciblent des éléments HTML en fonction de la balise parente, le nom d’attribut ou le nom de l’élément. Par exemple, le `LabelTagHelper` intégré peut cibler un élément HTML `<label>` lorsque les attributs du `LabelTagHelper` sont appliqués. Si vous êtes familiarisé avec des [HTML helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), les Tag helpers réduisent les transitions explicites entre HTML et c# dans les vues Razor. Dans de nombreux cas, les HTML helpers fournissent une approche alternative à un Tag helpers spécifique, mais il est important de reconnaître que les Tag helpers ne remplacent pas les HTML helpers et il n'y a pas de Tag helpers pour chaque HTML helper. [Tag helpers comparés aux HTML helpers](#tag-helpers-compared-to-html-helpers) explique les différences plus en détail.

## <a name="what-tag-helpers-provide"></a>Ce que fournissent des Tag helpers

**Une expérience de développement HTML convivial**. Pour la plupart des cas, le balisage Razor à l’aide de Tag helpers ressemble à du HTML standard. Les designers front-end qui sont familiarisés avec HTML, CSS et JavaScript peuvent modifier Razor sans apprendre la syntaxe Razor c#.

**Un environnement IntelliSense riche pour la création des balises HTML et Razor**. Il y a un certian contraste avec les HTML helpers, l’approche précédente côté serveur pour créer des balises dans les vues Razor. [Tag helpers comparés aux HTML helpers](#tag-helpers-compared-to-html-helpers) explique les différences plus en détail. [La prise en charge de l'IntelliSense pour les tag helpers](#intellisense-support-for-tag-helpers) explique l’environnement d’IntelliSense. Même les développeurs expérimentés avec la syntaxe Razor c# sont plus productifs, à l’aide de Tag helpers pour écrire des balises C# Razor.

**Un moyen de vous rendre plus productif et capable de produire du code plus robuste et fiable à l’aide des informations disponibles uniquement sur le serveur** , par exemple, historiquement la règle de mise à jour des images était de modifier le nom de l’image lorsque vous modifiez l’image. Les images doivent être mises en cache de manière agressive pour des raisons de performances, et sauf si vous modifiez le nom d’une image, vous risquez que les clients obtiennent une copie obsolète. Historiquement, après qu’une image a été modifiée, le nom devait être modifié et chaque référence à l’image dans l’application web devait être mise à jour. Non seulement c’est un travail très laborieux, mais c'est également sujet aux erreurs (vous pouvez oublier une référence, entrez accidentellement une chaîne incorrecte, etc.) Le `ImageTagHelper` intégré peut faire cela pour vous automatiquement. Le `ImageTagHelper` peut ajouter un numéro de version pour le nom de l’image, afin qu'à chaque modification de l’image, le serveur génère automatiquement une nouvelle version unique pour l’image. Les clients sont garantis d'obtenir l’image actuelle. Cette robustesse et cette économie de travail sont essentiellement gratuites à l’aide de l'`ImageTagHelper`.

La plupart des Tag helpers intégrés ciblent des éléments HTML existants et fournit des attributs côté serveur pour l’élément. Par exemple, l'élément `<input>` utilisé dans la plupart des vues dans le dossier *Views/Account* contient l'attribut `asp-for`, qui extrait le nom de la propriété de modèle spécifié dans le code HTML restitué. Le balisage de Razor suivant :

```cshtml
<label asp-for="Email"></label>
```

Génère le code HTML suivant :

```cshtml
<label for="Email">Email</label>
```

L'attribut `asp-for` est rendu disponible par la propriété `For` dans le `LabelTagHelper`. Consultez [la création de tag helpers](authoring.md) pour plus d’informations.

## <a name="managing-tag-helper-scope"></a>Gestion de la portée des tag helpers

La portée des Tag helpers est contrôlée par une combinaison de `@addTagHelper`, `@removeTagHelper`et de caractère "!".

<a name="add-helper-label"></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a>`@addTagHelper` rend les Tag helpers disponibles

Si vous créez une application web ASP.NET Core nommée *AuthoringTagHelpers* (avec aucune authentification), le fichier qui suit *Views/_ViewImports.cshtml* sera ajouté à votre projet :

[!code-cshtml[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

La directive `@addTagHelper` rend les Tag helpers disponibles à la vue. Dans ce cas, le fichier de vue est *Views/_ViewImports.cshtml*, qui par défaut est hérité par tous les fichiers de vue dans le dossier *views* et les sous-répertoires ; en rendant les Tag helpers disponibles. Le code ci-dessus utilise la syntaxe des caractères génériques ("\*") pour spécifier que tous les programmes d’assistance de balise dans l’assembly spécifié (*Microsoft.AspNetCore.Mvc.TagHelpers*) est disponible pour tous les fichiers de vue du répertoire *viewss* ou des sous-répertoire. Le premier paramètre après `@addTagHelper` spécifie les Tag helpers à charger (nous utilisons «\*» pour tous les Tag helpers), et le deuxième paramètre "Microsoft.AspNetCore.Mvc.TagHelpers" spécifie l’assembly qui contient les Tag helpers. *Microsoft.AspNetCore.Mvc.TagHelpers* est l’assembly pour les Tag helpers ASP.NET intégrés de base.

Pour exposer tous les Tag helpers dans ce projet (ce qui crée un assembly nommé *AuthoringTagHelpers*), utilisez ce qui suit :

[!code-cshtml[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

Si votre projet contient un `EmailTagHelper` avec l’espace de noms par défaut (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), vous pouvez fournir le nom qualifié complet (FQN) des Tag helpers :

```cshtml
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

Pour ajouter un Tag helpers à une vue à l’aide d’un FQN, vous ajoutez d’abord le FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) et le nom d’assembly (*AuthoringTagHelpers*). La plupart des développeurs préfèrent utiliser la syntaxe de caractère générique "\*" . La syntaxe de caractère générique vous permet d’insérer le caractère générique "\*" en guise de suffixe dans une FQN. Par exemple, une des directives suivantes affiche le `EmailTagHelper`:

```cshtml
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

Comme mentionné précédemment, l’ajout la directive `@addTagHelper` pour le fichier *Views/_ViewImports.cshtml* rend le Tag helper disponible pour afficher tous les fichiers dans les répertoires *views* et les sous-répertoires. Vous pouvez utiliser la directive `@addTagHelper` dans les fichiers de vue spécifiques si vous souhaitez participer à l’exposition de Tag helpers pour uniquement ces vues.

<a name="remove-razor-directives-label"></a>

### <a name="removetaghelper-removes-tag-helpers"></a>`@removeTagHelper`Supprime les Tag helpers

Le `@removeTagHelper` a les deux mêmes paramètres que `@addTagHelper`, et il supprime un Tag helper ajoutée précédemment. Par exemple, `@removeTagHelper` appliqué à une vue supprime le Tag helper spécifié de la vue. Utiliser `@removeTagHelper` dans un fichier *Views/Folder/_ViewImports.cshtml* supprime le Tag helper à partir de toutes les vues du *dossier*.

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a>Contrôle de la portée de Tag helper avec le fichier *_ViewImports.cshtml*

Vous pouvez ajouter un *_ViewImports.cshtml* à n’importe quel dossier de la vue et le moteur de vue applique les directives du fichier et du fichier *Views/_ViewImports.cshtml*. Si vous avez ajouté un fichier vide *Views/Home/_ViewImports.cshtml* pour les vues *Home*, il n'y aurait aucune modification car le fichier *_ViewImports.cshtml* est additif. N’importe quelle directive `@addTagHelper` vous ajoutez à la *Views/Home/_ViewImports.cshtml* fichier (qui ne sont pas dans la valeur par défaut *Views/_ViewImports.cshtml* fichier) exposerait ces Tag helpers aux vues uniquement dans le dossier *Home*.

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a>Désactivation des éléments individuels

Vous pouvez désactiver un Tag helper au niveau de l’élément avec le caractère d’annulations de Tag helper ("!"). Par exemple, `Email` validation est désactivée dans le `<span>` avec le caractère d’annulations de Tag helper :

```cshtml
<!span asp-validation-for="Email" class="text-danger"></!span>
```

Vous devez appliquer le caractère d’annulations de Tag helper à l’ouverture et la fermeture de la balise. (L’éditeur Visual Studio ajoute automatiquement le caractère d’exclusion à la balise de fermeture lorsque vous ajoutez une balise d’ouverture). Après avoir ajouté le caractère de désactivation, l’élément et les attributs de Tag helper ne s’affichent plus dans une police unique.

<a name="prefix-razor-directives-label"></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a>À l’aide de `@tagHelperPrefix` pour rendre l’utilisation du Tag helper explicite

La directive `@tagHelperPrefix` vous permet de spécifier une chaîne de préfixe de balise pour activer la prise en charge du Tag helper et de rendre l’utilisation du Tag helper explicite. Par exemple, vous pouvez ajouter le balisage suivant au fichier *Views/_ViewImports.cshtml* :

```cshtml
@tagHelperPrefix th:
```
Dans l’image du code ci-dessous, le préfixe de Tag helper est défini sur `th:`, ainsi que les éléments à l’aide du préfixe `th:` prend en charge les Tag helpers (les éléments de Tag helpers activés ont une police unique). Les éléments `<label>` et `<input>` ont le préfixe de Tag helper et sont activés, alors que l’élément `<span>` ne l'est pas.

![image](intro/_static/thp.png)

Les mêmes règles de hiérarchie qui s’appliquent à `@addTagHelper` s’appliquent également aux `@tagHelperPrefix`.

## <a name="intellisense-support-for-tag-helpers"></a>Prise en charge IntelliSense pour les Tag helpers

Lorsque vous créez une application web ASP.NET dans Visual Studio, elle ajoute le package NuGet "Microsoft.AspNetCore.Razor.Tools". C’est le package qui ajoute les outils de Tag helpers.

Envisagez d’écrire un élément HTML `<label>`. Dès que vous entrez `<l` dans l’éditeur Visual Studio, IntelliSense affiche les éléments correspondants :

![image](intro/_static/label.png)

Non seulement vous obtenezde l'aide HTML, mais aussi l’icône (le symbole "@" avec "<>" en-dessous).

![image](intro/_static/tagSym.png)

identifie l’élément ciblé par les programmes d’assistance de balise. Les éléments HTML purs (tels que le `fieldset`) affichent l’icône « <> ».

Un code HTML pur `<label>` affiche la balise HTML (avec le thème de couleur Visual Studio par défaut) dans une police marron, les attributs en rouge, et les valeurs de l’attribut en bleu.

![image](intro/_static/LableHtmlTag.png)

Une fois que vous entrez `<label`, IntelliSense répertorie les attributs HTML/CSS disponibles et les attributs ciblés de tag helper :

![image](intro/_static/labelattr.png)

La saisie semi-automatique des instructions IntelliSense vous permet d’appuyer sur la touche tab pour compléter l’instruction avec la valeur sélectionnée :

![image](intro/_static/stmtcomplete.png)

Dès qu’un attribut de Tag helper est entré, les polices de balise et d’attribut changent. En utilisant ke thème de couleur "Light" ou  "Blue" par défaut de Visual Studio, la police est en gras violet. Si vous utilisez le thème "Dark" de la police est bleu-vert en gras. Les images dans ce document ont été effectuées à l’aide de thème par défaut.

![image](intro/_static/labelaspfor2.png)

Vous pouvez entrer le raccourci Visual Studio *CompleteWord* (Ctrl + espace est le raccourci [par défaut](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) à l’intérieur des guillemets doubles (""), et vous êtes maintenant en c#, tout comme vous seriez dans une classe c#. L'IntelliSense affiche toutes les méthodes et propriétés sur le modèle de page. Les méthodes et propriétés sont disponibles, car le type de propriété est `ModelExpression`. Dans l’image ci-dessous, je vais modifier la vue `Register`, donc le `RegisterViewModel` est disponible.

![image](intro/_static/intellemail.png)

IntelliSense répertorie les propriétés et méthodes disponibles pour le modèle dans la page. L’environnement riche IntelliSense vous permet de sélectionner la classe CSS :

![image](intro/_static/iclass.png)

![image](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a>Programmes d’assistance de balise par rapport aux programmes d’assistance HTML

Programmes d’assistance de balise attachement à des éléments HTML dans les vues Razor, tandis que [programmes d’assistance HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) sont appelés comme Méthodes entrecoupées avec du code HTML dans les vues Razor. Prenez en compte le balisage suivant Razor, qui crée un nom HTML avec la classe CSS « caption » :

```cshtml
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

Le symbole At (`@`) indique à Razor qu'il s'agit du début du code. Les deux paramètres ("FirstName" et "First Name") sont des chaînes, par conséquent, [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense) ne peut aider. Le dernier argument :

```cshtml
new {@class="caption"}
```

est un objet anonyme est utilisé pour représenter des attributs. Étant donné que **class** est un mot clé réservé en c#, vous utilisez le symbole `@` pour forcer c# à interpréter "@class= " comme un symbole (nom de propriété). Pour un designer front-end (quelqu'un familiarisé avec HTML, CSS et JavaScript et d’autres technologies client, mais pas familiarisé avec c# et Razor), la plupart de la ligne est étrangère. La ligne entière doit être créée sans l’aide d’IntelliSense.

En utilisant le `LabelTagHelper`, la même balise peut être écrite comme ceci :

![image](intro/_static/label2.png)

Avec la version Tag helper, dès que vous entrez `<l` dans l’éditeur Visual Studio, IntelliSense affiche les éléments correspondants :

![image](intro/_static/label.png)

IntelliSense vous permet d’écrire la ligne entière. Le `LabelTagHelper` également par défaut, le contenu de la valeur de l’attribut `asp-for` ("FirstName") en "First Name" ; Il convertit des propriétés de casse mixte (Camel case) en une phrase composée du nom de propriété avec un espace pour chaque nouvelle lettre majuscule. Dans le balisage suivant :

![image](intro/_static/label2.png)

génère :

```cshtml
<label class="caption" for="FirstName">First Name</label>
```

La casse mixte qui utilise la phrase de contenu n’est pas utilisée si vous ajoutez du contenu au `<label>`. Exemple :

![image](intro/_static/1stName.png)

génère :

```cshtml
<label class="caption" for="FirstName">Name First</label>
```

L’image du code suivant montre la partie de l’écran de la vue Razor *Views/Account/Register.cshtml* générée à partir du modèle ASP.NET MVC hérité 4.5.x inclus avec Visual Studio 2015.

![image](intro/_static/regCS.png)

L’éditeur Visual Studio affiche un code c# avec un fond gris. Par exemple, le Tag helper `AntiForgeryToken` :

```cshtml
@Html.AntiForgeryToken()
```

s’affiche avec un fond gris. La plupart du balisage dans la vue de Registre est c#. Comparer que l’approche équivalente à l’aide de programmes d’assistance de balise :

![image](intro/_static/regTH.png)

Le balisage est beaucoup plus claire et plus facile à lire, modifier et mettre à jour que l’approche HTML helper. Le code c# est réduit à la valeur minimale que le serveur doit savoir. L’éditeur Visual Studio affiche un balisage ciblé par un tag helper dans une police unique.

Considérez le groupe *email* :

[!code-csharp[Main](intro/sample/Register.cshtml?range=12-18)]

Chacun des attributs "asp-" a la valeur "Email", mais "Email" n’est pas une chaîne. Dans ce contexte, "Email" est la propriété d’expression de modèle c# pour le `RegisterViewModel`.

L’éditeur Visual Studio vous aide à écrire **toutes les** balises avec l’approche Tag helper du formulaire d'inscription, alors que Visual Studio ne fournit aucune aide pour la plupart du code avec l’approche HTML helper. [la prise en charge d'IntelliSense pour les Tag helpers](#intellisense-support-for-tag-helpers) fournit de détails sur l’utilisation des Tag helpers dans l’éditeur Visual Studio.

## <a name="tag-helpers-compared-to-web-server-controls"></a>Tag helpers par rapport aux contrôles serveur Web

* Les Tag helpers ne possèdent pas l’élément auxquels ils sont associés ; ils participent simplement au rendu de l’élément et au contenu. [Les contrôles serveur Web](https://msdn.microsoft.com/library/7698y1f0.aspx) ASP.NET sont déclarés et appelés sur une page.

* [Les contrôles serveur Web](https://msdn.microsoft.com/library/zsyt68f1.aspx) ont un cycle de vie non trivial qui facilitent le développement et le débogage difficile.

* Les contrôles serveur Web permettent d’ajouter des fonctionnalités aux éléments de modèle DOM (Document Object) client à l’aide d’un contrôle client. Les Tag helpers n’ont aucun modèle DOM.

* Les contrôles serveur Web incluent la détection automatique du navigateur. Les Tag helpers n'ont aucune connaissance du navigateur.

* Plusieurs Tag helpers peuvent agir sur le même élément (consultez [éviter les conflits entre Tag helpers](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts) ) alors que vous ne pouvez pas généralement composer des contrôles serveur Web.

* Les Tag helpers peuvent modifier la balise et le contenu des éléments HTML auxquels ils sont étendus, mais ne modifier directement rien d’autre sur une page. Les contrôles serveur Web ont une portée moins spécifique et peuvent effectuer des actions qui affectent les autres parties de votre page; activant des effets secondaires involontaires.

* Les contrôles serveur Web utilisent des convertisseurs de type pour convertir des chaînes en objets. Avec les Tag helpers, vous travaillez en mode natif en c#, vous n’avez pas besoin d'effectuer une conversion de type.

* Les contrôles serveur Web utilisent [System.ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel) pour implémenter le comportement d’exécution et au moment du design des composants et des contrôles. `System.ComponentModel` inclut les classes de base et les interfaces pour l’implémentation des attributs et des convertisseurs de type, les liaisons de sources de données et les licences des composants. Comparez cela à des Tags helpers, généralement dérivés de `TagHelper`et la classe de base `TagHelper` expose uniquement deux méthodes `Process` et `ProcessAsync`.

## <a name="customizing-the-tag-helper-element-font"></a>Personnalisation de la police d’élément d’assistance de balise

Vous pouvez personnaliser la police et la colorisation dans **Outils** > **Options** > **Environnement** > **Polices et couleurs**:

![image](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a>Ressources supplémentaires

* [Création de Tag Helpers](authoring.md)
* [Utilisation des formulaires](../working-with-forms.md)
* [TagHelperSamples sur GitHub](https://github.com/dpaquette/TagHelperSamples) contient des exemples de tag helpers pour l’utilisation de [Bootstrap](http://getbootstrap.com/).

<!--
* [Working with Forms ](xref:mvc/views/working-with-forms)
-->
