---
title: Tag Helpers dans ASP.NET Core
author: rick-anderson
description: Découvrez ce que sont les Tag Helpers et comment les utiliser dans ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 03/18/2019
uid: mvc/views/tag-helpers/intro
ms.openlocfilehash: 11d2914b5797735fb6a262a31bdb49f58391579f
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64884054"
---
# <a name="tag-helpers-in-aspnet-core"></a>Tag Helpers dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-are-tag-helpers"></a>Que sont les Tag Helpers ?

Les Tag Helpers permettent au code côté serveur de participer à la création et au rendu des éléments HTML dans les fichiers Razor. Par exemple, le `ImageTagHelper` intégré peut ajouter un numéro de version au nom de l’image. Chaque fois que l’image change, le serveur en génère une nouvelle version unique, pour que les clients soient sûrs d’obtenir l’image actuelle (au lieu d’une image mise en cache obsolète). Il existe de nombreux Tag Helpers pour les tâches courantes (par exemple la création de formulaires ou de liens, le chargement de ressources, etc.) et bien d’autres encore, dans les dépôts GitHub publics et sous forme de packages NuGet. Les Tag Helpers sont créés en C# et ciblent les éléments HTML en fonction du nom de l’élément, du nom de l’attribut ou de la balise parente. Par exemple, le `LabelTagHelper` intégré peut cibler l’élément `<label>` HTML quand les attributs `LabelTagHelper` sont appliqués. Si vous connaissez déjà les [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), les Tag Helpers permettent de réduire les transitions explicites entre le code HTML et le code C# dans les affichages Razor. Dans de nombreux cas, les HTML Helpers offrent une autre approche par rapport à un Tag Helper spécifique. Toutefois, il est clair que les Tag Helpers ne remplacent pas les HTML Helpers, et qu’il n’existe pas toujours un Tag Helper pour chaque HTML Helper. [Comparaison des Tag Helpers aux HTML Helpers](#tag-helpers-compared-to-html-helpers) explique les différences de façon plus approfondie.

## <a name="what-tag-helpers-provide"></a>Ce que fournissent des Tag helpers

**Une expérience de développement HTML**  Dans leur majorité, les balises Razor qui utilisent des Tag Helpers ressemblent à du code HTML standard. Les concepteurs frontaux familiarisés avec HTML, CSS et JavaScript peuvent modifier Razor sans apprendre la syntaxe Razor C#.

**Un riche environnement IntelliSense pour la création de balises HTML et Razor** Cet environnement se démarque fortement des HTML Helpers, correspondant à la précédente approche de la création côté serveur de balises dans les affichages Razor. [Comparaison des Tag Helpers aux HTML Helpers](#tag-helpers-compared-to-html-helpers) explique les différences de façon plus approfondie. [Prise en charge IntelliSense des Tag Helpers](#intellisense-support-for-tag-helpers) explique l’environnement IntelliSense. Même les développeurs habitués à la syntaxe Razor C# sont plus productifs en utilisant des Tag Helpers qu’en écrivant des balises Razor C#.

**Un moyen d’améliorer votre productivité et votre capacité à produire du code plus robuste, fiable et facile à gérer en utilisant des informations uniquement disponibles sur le serveur** Par exemple, l’ancien usage pour la mise à jour des images consistait à modifier le nom de l’image quand vous modifiiez l’image. Les images doivent être efficacement mises en cache pour des raisons de performance, et à moins de modifier le nom d’une image, les clients risquent d’obtenir une copie obsolète. Auparavant, une fois qu’une image avait été modifiée, le nom devait être modifié et chaque référence à l’image dans l’application web avait besoin d’être mise à jour. Non seulement ce travail était fastidieux, mais il engendrait facilement des erreurs (vous pouviez oublier une référence, entrer la mauvaise chaîne par inadvertance, etc.). Le `ImageTagHelper` intégré peut s’en charger automatiquement à votre place. Le `ImageTagHelper` peut ajouter un numéro de version au nom de l’image, si bien que dès que l’image change, le serveur génère automatiquement une nouvelle version unique de l’image. Les clients sont sûrs d’obtenir l’image actuelle. Cette robustesse et cette économie de travail sont inhérentes à l’utilisation du `ImageTagHelper`.

La plupart des Tag Helpers intégrés ciblent les éléments HTML standard et fournissent des attributs côté serveur pour l’élément. Par exemple, l’élément `<input>` utilisé dans de nombreuses vues dans le dossier *Views/Account* contient l’attribut `asp-for`. Cet attribut extrait le nom de la propriété de modèle spécifiée dans le code HTML restitué. Examinons une vue Razor avec le modèle suivant :

```csharp
public class Movie
{
    public int ID { get; set; }
    public string Title { get; set; }
    public DateTime ReleaseDate { get; set; }
    public string Genre { get; set; }
    public decimal Price { get; set; }
}
```

Le balisage Razor suivant :

```cshtml
<label asp-for="Movie.Title"></label>
```

Génère le code HTML suivant :

```html
<label for="Movie_Title">Title</label>
```

L’attribut `asp-for` est rendu disponible par la propriété `For` dans le [LabelTagHelper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.labeltaghelper?view=aspnetcore-2.0). Pour plus d’informations, consultez [Créer des Tag Helpers](xref:mvc/views/tag-helpers/authoring).

## <a name="managing-tag-helper-scope"></a>Gestion de l’étendue des Tag Helpers

L’étendue des Tag Helpers est contrôlée par une combinaison de `@addTagHelper`, `@removeTagHelper` et du caractère d’annulation « ! ».

<a name="add-helper-label"></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a>`@addTagHelper` rend les Tag Helpers disponibles

Si vous créez une application web ASP.NET Core nommée *AuthoringTagHelpers*, le fichier qui suit *Views/_ViewImports.cshtml* est ajouté à votre projet :

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

La directive `@addTagHelper` rend les Tag Helpers disponibles dans l’affichage. Dans cet exemple, le fichier d’affichage est *Pages/_ViewImports.cshtml*, qui est hérité par défaut par tous les fichiers dans le dossier et les sous-dossiers *Pages* ; les Tag Helpers sont ainsi disponibles. Le code ci-dessus utilise la syntaxe d’expressions génériques (« \* ») pour spécifier que tous les Tag Helpers dans l’assembly spécifié (*Microsoft.AspNetCore.Mvc.TagHelpers*) sont disponibles pour chaque fichier d’affichage du répertoire ou sous-répertoire *Views*. Le premier paramètre après `@addTagHelper` spécifie les Tag Helpers à charger (nous utilisons « \* » pour tous les Tag Helpers), et le deuxième paramètre « Microsoft.AspNetCore.Mvc.TagHelpers » spécifie l’assembly qui contient les Tag Helpers. *Microsoft.AspNetCore.Mvc.TagHelpers* est l’assembly des Tag Helpers ASP.NET Core intégrés.

Pour exposer tous les Tag Helpers inclus dans ce projet (ce qui crée un assembly nommé *AuthoringTagHelpers*), utilisez ce qui suit :

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

Si votre projet contient un `EmailTagHelper` avec l’espace de noms par défaut (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), vous pouvez fournir le nom qualifié complet (FQN) des Tag helpers :

```cshtml
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

Pour ajouter un Tag Helper à un affichage à l’aide d’un FQN, vous ajoutez d’abord ce FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), puis le nom de l’assembly (*AuthoringTagHelpers*). La plupart des développeurs préfèrent utiliser la syntaxe d’expressions génériques « \* ». Celle-ci permet d’insérer le caractère générique « \* » en guise de suffixe dans un FQN. Par exemple, chacune des directives suivantes affiche le `EmailTagHelper` :

```cshtml
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

Comme mentionné précédemment, l’ajout de la directive `@addTagHelper` au fichier *Views/_ViewImports.cshtml* met le Tag Helper à la disposition de tous les fichiers d’affichage inclus dans le répertoire et les sous-répertoires *Views*. Vous pouvez utiliser la directive `@addTagHelper` dans des fichiers d’affichage spécifiques si vous choisissez d’exposer le Tag Helper uniquement à ces affichages.

<a name="remove-razor-directives-label"></a>

### <a name="removetaghelper-removes-tag-helpers"></a>`@removeTagHelper` supprime les Tag Helpers

Le`@removeTagHelper` a les deux mêmes paramètres que `@addTagHelper`, et il supprime un Tag helper ajoutée précédemment. Par exemple, `@removeTagHelper` appliqué à une vue supprime le Tag helper spécifié de la vue. Utiliser `@removeTagHelper` dans un fichier *Views/Folder/_ViewImports.cshtml* supprime le Tag helper à partir de toutes les vues du *dossier*.

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a>Contrôle de l’étendue des Tag Helpers à l’aide du fichier *_ViewImports.cshtml*

Vous pouvez ajouter un fichier *_ViewImports.cshtml* à tout dossier d’affichage. Le moteur d’affichage applique les directives de ce fichier et du fichier *Views/_ViewImports.cshtml*. Si vous avez ajouté un fichier *Views/Home/_ViewImports.cshtml* vide pour les affichages *Home*, rien n’est modifié car le fichier *_ViewImports.cshtml* est additif. Toute directive `@addTagHelper` que vous ajoutez au fichier *Views/Home/_ViewImports.cshtml* (qui n’est pas dans le fichier *Views/_ViewImports.cshtml* par défaut) expose ces Tag Helpers uniquement aux affichages inclus dans le dossier *Home*.

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a>Refus d’éléments individuels

Vous pouvez désactiver un Tag Helper au niveau de l’élément à l’aide du caractère d’annulation de Tag Helper (« ! »). Par exemple, la validation `Email` est désactivée dans `<span>` à l’aide du caractère d’annulation de Tag Helper :

```cshtml
<!span asp-validation-for="Email" class="text-danger"></!span>
```

Vous devez appliquer le caractère d’annulation de Tag Helper à la balise d’ouverture et de fermeture. (L’éditeur Visual Studio ajoute automatiquement le caractère d’annulation à la balise de fermeture quand vous en ajoutez un à la balise d’ouverture). Après avoir ajouté le caractère d’annulation, l’élément et les attributs du Tag Helper ne s’affichent plus dans une police caractéristique.

<a name="prefix-razor-directives-label"></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a>Utilisation de `@tagHelperPrefix` pour rendre l’utilisation du Tag Helper explicite

La directive `@tagHelperPrefix` vous permet de spécifier une chaîne de préfixe de balise pour activer la prise en charge des Tag Helpers et rendre leur utilisation explicite. Par exemple, vous pouvez ajouter le balisage suivant au fichier *Views/_ViewImports.cshtml* :

```cshtml
@tagHelperPrefix th:
```

Dans l’image de code ci-dessous, le préfixe du Tag Helper a la valeur `th:`, si bien que seuls les éléments qui utilisent le préfixe `th:` prennent en charge les Tag Helpers (les éléments activés pour les Tag Helpers ont une police caractéristique). Les éléments `<label>` et `<input>` ont le préfixe du Tag Helper et sont activés, à la différence de l’élément `<span>`.

![image](intro/_static/thp.png)

Les mêmes règles de hiérarchie qui s’appliquent à `@addTagHelper` s’appliquent aussi à `@tagHelperPrefix`.

## <a name="self-closing-tag-helpers"></a>Tag Helpers à fermeture automatique

De nombreux Tag Helpers ne peuvent pas être utilisés en tant que balises à fermeture automatique. Certains Tag Helpers sont conçus en tant que balises à fermeture automatique. L’utilisation d’un Tag Helper qui n’a pas été conçu pour être à fermeture automatique supprime la sortie rendue. La fermeture automatique d’un Tag Helper aboutit à une balise à fermeture automatique dans la sortie rendue. Pour plus d’informations, consultez [cette remarque](xref:mvc/views/tag-helpers/authoring#self-closing) dans [Création de Tag Helpers](xref:mvc/views/tag-helpers/authoring).

## <a name="intellisense-support-for-tag-helpers"></a>Prise en charge IntelliSense des Tag Helpers

Quand vous créez une application web ASP.NET Core dans Visual Studio, le package NuGet « Microsoft.AspNetCore.Razor.Tools » est ajouté. Il s’agit du package qui ajoute les outils de Tag Helper.

Envisagez d’écrire un élément `<label>` HTML. Dès que vous entrez `<l` dans l’éditeur Visual Studio, IntelliSense affiche les éléments correspondants :

![image](intro/_static/label.png)

Non seulement vous obtenez l’aide HTML, mais aussi l’icône (le « @" symbol with "<> » en dessous).

![image](intro/_static/tagSym.png)

Identifie l’élément comme étant ciblé par les Tag Helpers. Les éléments HTML purs (comme `fieldset`) présentent l’icône « <> ».

Une balise `<label>` HTML pure affiche la balise HTML (avec le thème de couleur Visual Studio par défaut) dans une police marron, les attributs en rouge et les valeurs d’attribut en bleu.

![image](intro/_static/LableHtmlTag.png)

Après avoir entré `<label`, IntelliSense répertorie les attributs HTML/CSS disponibles et les attributs ciblés par les Tag Helpers :

![image](intro/_static/labelattr.png)

La saisie semi-automatique des instructions par IntelliSense vous permet d’appuyer sur la touche Tab pour compléter automatiquement l’instruction avec la valeur sélectionnée :

![image](intro/_static/stmtcomplete.png)

Dès qu’un attribut de Tag Helper est entré, les polices de la balise et de l’attribut changent. En utilisant le thème de couleur « Bleu » ou « Clair » par défaut de Visual Studio, la police est en violet gras. Si vous utilisez le thème « Foncé », la police est en bleu-vert gras. Les images de ce document ont été prises à l’aide du thème par défaut.

![image](intro/_static/labelaspfor2.png)

Vous pouvez entrer le raccourci *CompleteWord* Visual Studio (Ctrl+Espace [par défaut](/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio)) à l’intérieur des guillemets doubles ("") pour passer en C#, de la même façon que dans une classe C#. IntelliSense affiche toutes les méthodes et propriétés dans le modèle de page. Les méthodes et propriétés sont disponibles car le type de propriété est `ModelExpression`. Dans l’image ci-dessous, je modifie l’affichage `Register`, donc `RegisterViewModel` est disponible.

![image](intro/_static/intellemail.png)

IntelliSense répertorie les propriétés et méthodes disponibles pour le modèle dans la page. Le riche environnement IntelliSense vous aide à sélectionner la classe CSS :

![image](intro/_static/iclass.png)

![image](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a>Comparaison des Tag Helpers aux HTML Helpers

Les Tag Helpers s’attachent aux éléments HTML dans les affichages Razor, tandis que les [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) sont appelés comme des méthodes parsemées dans le code HTML dans les affichages Razor. Examinez le balisage Razor suivant, qui crée une étiquette HTML avec la classe CSS « caption » :

```cshtml
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

L’arobase (`@`) signale le début du code à Razor. Les deux paramètres suivants (« FirstName » et « First Name: ») sont des chaînes, par conséquent, [IntelliSense](/visualstudio/ide/using-intellisense) ne sert à rien. Le dernier argument :

```cshtml
new {@class="caption"}
```

Est un objet anonyme utilisé pour représenter des attributs. Étant donné que `class` est un mot clé réservé en C#, vous utilisez le symbole `@` pour forcer le code C# à interpréter `@class=` comme un symbole (nom de propriété). Pour un concepteur frontal (une personne qui connaît bien le code HTML, CSS et JavaScript et d’autres technologies clientes, mais qui ne connaît pas C# et Razor), la majorité de la ligne est étrangère. La ligne entière doit être créée sans l’aide d’IntelliSense.

Avec `LabelTagHelper`, le même balisage peut s’écrire ainsi :

```cshtml
<label class="caption" asp-for="FirstName"></label>
```

Avec la version Tag Helper, dès que vous entrez `<l` dans l’éditeur Visual Studio, IntelliSense affiche les éléments correspondants :

![image](intro/_static/label.png)

IntelliSense vous aide à écrire la ligne entière.

L’image de code suivante montre la partie formulaire de l’affichage Razor *Views/Account/Register.cshtml* généré à partir du modèle ASP.NET 4.5.x MVC inclus dans Visual Studio.

![image](intro/_static/regCS.png)

L’éditeur Visual Studio présente le code C# avec un arrière-plan gris. Par exemple, le HTML Helper `AntiForgeryToken` :

```cshtml
@Html.AntiForgeryToken()
```

est présenté avec un arrière-plan gris. La plupart du balisage dans l’affichage Register est en C#. Comparez-le à l’approche équivalente qui utilise des Tag Helpers :

![image](intro/_static/regTH.png)

Le balisage est beaucoup plus claire et facile à lire, modifier et gérer qu’avec l’approche des HTML Helpers. Le code C# est réduit au minimum que le serveur doit savoir. L’éditeur Visual Studio présente le balisage ciblé par un Tag Helper dans une police caractéristique.

Examinez le groupe *Email* :

[!code-csharp[](intro/sample/Register.cshtml?range=12-18)]

Chacun des attributs « asp- » a la valeur « Email », mais « Email » n’est pas une chaîne. Dans ce contexte, « Email » est la propriété de l’expression du modèle C# pour `RegisterViewModel`.

L’éditeur Visual Studio vous aide à écrire **tout** le balisage dans l’approche Tag Helpers du formulaire d’inscription, tandis que Visual Studio ne fournit aucune aide pour la plupart du code dans l’approche HTML Helpers. [Prise en charge IntelliSense des Tag Helpers](#intellisense-support-for-tag-helpers) décrit précisément l’utilisation de Tag Helpers dans l’éditeur Visual Studio.

## <a name="tag-helpers-compared-to-web-server-controls"></a>Comparaison des Tag Helpers aux contrôles de serveur web

* Les Tag Helpers ne sont pas propriétaires de l’élément auquel ils sont associés ; ils participent simplement au rendu de l’élément et du contenu. Les [contrôles de serveur web](https://msdn.microsoft.com/library/7698y1f0.aspx) ASP.NET sont déclarés et appelés dans une page.

* Les [contrôles de serveur web](https://msdn.microsoft.com/library/zsyt68f1.aspx) ont un cycle de vie non trivial qui peut rendre le développement et le débogage difficiles.

* Les contrôles de serveur web vous permettent d’ajouter des fonctionnalités aux éléments DOM (Document Object Model) à l’aide d’un contrôle client. Les Tag Helpers n’ont pas de DOM.

* Les contrôles de serveur web incluent une détection automatique du navigateur. Les Tag Helpers n’ont pas connaissance du navigateur.

* Plusieurs Tag Helpers peuvent agir sur le même élément (consultez [Éviter les conflits de Tag Helpers](xref:mvc/views/tag-helpers/authoring#avoid-tag-helper-conflicts)) alors que généralement vous ne pouvez pas composer des contrôles de serveur web.

* Les Tag Helpers peuvent modifier la balise et le contenu des éléments HTML auxquels ils s’étendent, mais pas modifier directement quoi que ce soit d’autre dans une page. Les contrôles de serveur web ont une étendue moins spécifique et peuvent effectuer des actions qui affectent d’autres parties de votre page, ce qui peut entraîner des effets secondaires involontaires.

* Les contrôles de serveur web utilisent des convertisseurs de type pour convertir les chaînes en objets. Avec les Tag Helpers, vous travaillez en mode natif en C#, donc vous n’avez pas besoin d’effectuer de conversions de type.

* Les contrôles de serveur web utilisent [System.ComponentModel](/dotnet/api/system.componentmodel) pour implémenter le comportement au moment de l’exécution et au moment du design des composants et des contrôles. `System.ComponentModel` inclut les classes et les interfaces de base servant à l’implémentation des attributs et des convertisseurs de type, à la liaison à des sources de données et à la gestion des licences des composants. Comparez-les aux Tag Helpers, qui sont généralement dérivés de `TagHelper` et à la classe de base `TagHelper` qui expose uniquement deux méthodes, `Process` et `ProcessAsync`.

## <a name="customizing-the-tag-helper-element-font"></a>Personnalisation de la police des éléments Tag Helper

Vous pouvez personnaliser la police et les couleurs depuis **Outils** > **Options** > **Environnement** > **Polices et couleurs** :

![image](intro/_static/fontoptions2.png)

[!INCLUDE[](~/includes/built-in-TH.md)]

## <a name="additional-resources"></a>Ressources supplémentaires

* [Créer des Tag Helpers](xref:mvc/views/tag-helpers/authoring)
* [Utilisation des formulaires](xref:mvc/views/working-with-forms)
* [TagHelperSamples sur GitHub](https://github.com/dpaquette/TagHelperSamples) contient des exemples de Tag Helpers à utiliser avec [Bootstrap](http://getbootstrap.com/).
