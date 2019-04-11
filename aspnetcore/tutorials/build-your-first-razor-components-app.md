---
title: Créer votre première application Composants Razor
author: guardrex
description: Créez une application Composants Razor pas à pas et découvrez les concepts de base des Composants Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
uid: tutorials/first-razor-components-app
ms.openlocfilehash: 697c4659bcc9952ffe9868fe9b3c0d28019bc369
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468774"
---
# <a name="build-your-first-razor-components-app"></a>Créer votre première application Composants Razor

Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Ce tutoriel vous montre comment créer une application avec les Composants Razor et illustre les concepts de base des Composants Razor. Vous pouvez profiter de ce tutoriel en utilisant soit un projet basé sur les Composants Razor (pris en charge dans .NET Core 3.0 ou version ultérieure) ou un projet Blazor (pris en charge dans une prochaine version de .NET Core).

Pour une expérience à l’aide de Composants Razor ASP.NET Core (*recommandé*) :

* Suivez les instructions dans <xref:razor-components/get-started> pour créer un projet basé sur les Composants Razor.
* Attribuez un nom au projet `RazorComponents`.

Pour une expérience à l’aide de Blazor :

* Suivez les instructions dans <xref:spa/blazor/get-started> pour créer un projet basé sur Blazor.
* Attribuez un nom au projet `Blazor`.

[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample)). Pour connaître les prérequis, consultez les rubriques suivantes :

## <a name="build-components"></a>Construire des composants

1. Accédez à chacune des trois pages de l’application dans le dossier *Components/Pages* (*Pages* dans Blazor) : Accueil, Compteur et Extraire des données. Ces pages sont implémentées par les fichiers du composant Razor : *Index.razor*, *Counter.razor* et *FetchData.razor*. (Blazor continue d’utiliser l’extension de fichier *.cshtml* : *Index.cshtml*, *Counter.cshtml* et *FetchData.cshtml*.)

1. Sur la page Counter, sélectionnez le bouton **Click me** pour incrémenter le compteur sans actualisation de la page. L’incrémentation d’un compteur dans une page web requiert normalement l’écriture JavaScript, mais le projet Composants Razor fournit une meilleure approche à l’aide C#.

1. Examinez l’implémentation du composant Counter dans le fichier *Counter.razor*.

   *Components/Pages/Counter.razor* (*Pages/Counter.cshtml* dans Blazor) :

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.razor)]

   L’interface utilisateur du composant Counter est définie à l’aide de HTML. La logique de rendu dynamique (par exemple, les boucles, conditions, expressions) est ajoutée à l’aide d’une syntaxe C# intégrée appelée [Razor](xref:mvc/views/razor). Le balisage HTML et la logique de rendu C# sont convertis en une classe de composants au moment de la génération. Le nom de la classe .NET générée correspond à celui du fichier.

   Les membres de la classe de composants sont définis dans un bloc `@functions`. Dans le bloc `@functions`, l’état du composant (propriétés, champs) et les méthodes sont spécifiés pour la gestion des événements ou pour définir une autre logique de composant. Ces membres sont ensuite utilisés dans le cadre de la logique de rendu du composant et la gestion des événements.

   Lorsque le bouton **Click me** est sélectionné :

   * Le gestionnaire `onclick` enregistré du composant Counter est appelé (méthode `IncrementCount`).
   * Le composant Counter regénère son arborescence de rendu.
   * La nouvelle arborescence de rendu est comparée à la précédente.
   * Seules les modifications apportées à Document Object Model (DOM) sont appliquées. Le compte affiché est mis à jour.

1. Modifiez la logique C# du composant Counter pour que l’incrément du compte passe de un à deux.

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. Régénérez et exécutez l’application pour voir les modifications. Sélectionnez le bouton **Click me** et le compteur passe à deux incréments.

## <a name="use-components"></a>Utiliser des composants

Incluez un composant dans un autre composant à l’aide d’une syntaxe de type HTML.

1. Ajoutez le composant Counter au composant Index (Home) de l’application en ajoutant un élément `<Counter />` au composant Index.

   Si vous utilisez Blazor pour cette expérience, un composant Survey Prompt (élément `<SurveyPrompt>`) se trouve dans le composant Index. Remplacez l’élément `<SurveyPrompt>` par l’élément `<Counter>`.

   *Components/Pages/Index.razor* (*Pages/Index.cshtml* dans Blazor) :

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.razor?highlight=7)]

1. Régénérez et exécutez l’application. La page d’accueil a son propre compteur.

## <a name="component-parameters"></a>Paramètres de composant

Les composants peuvent également avoir des paramètres. Les paramètres des composants sont définis à l’aide de propriétés non publiques sur la classe de composants décorée avec `[Parameter]`. Utilisez des attributs pour spécifier des arguments pour un composant dans le balisage.

1. Mettez à jour le code C# `@functions` du composant :

   * Ajoutez une propriété `IncrementAmount` décorée avec l’attribut `[Parameter]`.
   * Modifiez la méthode `IncrementCount` pour utiliser `IncrementAmount` lorsque vous augmentez la valeur de `currentCount`.

   *Components/Pages/Counter.razor* (*Pages/Counter.cshtml* dans Blazor) :

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Counter.razor?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. Spécifiez un paramètre `IncrementAmount` dans l’élément `<Counter>` du composant Home à l’aide d’un attribut. Définissez la valeur pour incrémenter le compteur par 10.

   *Components/Pages/Index.razor* (*Pages/Index.cshtml* dans Blazor) :

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Index.razor?highlight=7)]

1. Rechargez la page d’accueil. Le compteur s’incrémente de dix unités chaque fois que le bouton **Cliquer ici** est sélectionné. Le compteur sur la page Counter s’incrémente d’une unité.

## <a name="route-to-components"></a>Acheminer vers les composants

La directive `@page` en haut du fichier *Counter.razor* spécifie que ce composant est un point de terminaison de routage. Le composant Counter gère les requêtes envoyées à `/Counter`. Sans la directive `@page`, le composant ne gère pas les requêtes acheminées, mais le composant peut toujours être utilisé par d’autres composants.

## <a name="dependency-injection"></a>Injection de dépendances

Les services enregistrés dans le conteneur de services de l’application sont disponibles pour les composants via [l’injection de dépendance (DI)](xref:fundamentals/dependency-injection). Injectez des services dans un composant à l’aide de la directive `@inject`.

Examinez les directives du composant FetchData dans l’exemple d’application.

Dans l’exemple d’application Razor Components, le service `WeatherForecastService` est inscrit en tant que [singleton](xref:fundamentals/dependency-injection#service-lifetimes), de sorte qu’une instance du service est disponible dans toute l’application. La directive `@inject` est utilisée pour injecter l’instance du service `WeatherForecastService` dans le composant.

*Components/Pages/FetchData.razor* :

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

Le composant FetchData utilise le service injecté, comme `ForecastService`, pour récupérer un tableau d’objets `WeatherForecast` :

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

Dans la version Blazor de l’exemple d’application, `HttpClient` est injecté pour obtenir des données de prévisions météorologiques à partir des fichiers *weather.json* du dossier *wwwroot/sample-data* :

*Pages/FetchData.cshtml* :

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=7)]

Dans les deux exemples d’applications, une boucle [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) est utilisée pour restituer chaque instance de prévision sous forme de ligne dans la table des données météorologiques :

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a>Générer une liste de tâches

Ajoutez un nouveau composant à l’application qui implémente une liste de tâches simple.

1. Ajoutez un fichier vide à l’exemple d’application :

   * Pour l’expérience Razor Components, ajoutez un fichier *Todo.razor* au dossier *Components/Pages*.
   * Pour l’expérience Blazor, ajoutez un fichier *Todo.cshtml* au dossier *Pages*.

1. Fournissez le balisage initial pour le composant :

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. Ajoutez le composant Todo à la barre de navigation.

   Le composant NavMenu (*Components/Shared/NavMenu.razor* ou *Shared/NavMenu.cshtml* dans Blazor) est utilisé dans la disposition de l’application. Les dispositions sont des composants qui vous permettent d’éviter la duplication de contenu dans l’application. Pour plus d'informations, consultez <xref:razor-components/layouts>.

   Ajoutez un `<NavLink>` pour le composant Todo en ajoutant le balisage d’élément de liste suivant sous les éléments de liste existants dans le fichier *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* dans Blazor) :

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. Régénérez et exécutez l’application. Consultez la nouvelle page Todo pour vérifier que le lien vers le composant Todo fonctionne.

1. Ajoutez un fichier *TodoItem.cs* à la racine du projet pour contenir une classe qui représente un élément Todo. Utilisez le code C# suivant pour la classe `TodoItem` :

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/TodoItem.cs)]

1. Retournez au composant Todo (*Components/Pages/Todo.razor* ou *Pages/Todo.cshtml* dans Blazor) :

   * Ajoutez un champ pour les éléments Todo dans un bloc `@functions`. Le composant Todo utilise ce champ pour maintenir l’état de la liste de tâches.
   * Ajoutez un balisage de liste non triée et une boucle `foreach` pour effectuer le rendu de chaque élément todo en tant qu’élément de liste.

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. L’application nécessite des éléments d’interface utilisateur pour ajouter des éléments todo à la liste. Ajoutez une entrée de texte et un bouton sous la liste :

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. Régénérez et exécutez l’application. Lorsque le bouton **Add todo** est sélectionné, rien ne se produit car aucun gestionnaire d’événements n’est lié au bouton.

1. Ajoutez une méthode `AddTodo` au composant Todo et enregistrez-la pour les clics sur le bouton à l’aide de l’attribut `onclick` :

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   La méthode C# `AddTodo` est appelée lorsque le bouton est sélectionné.

1. Pour obtenir le titre du nouvel élément todo, ajoutez un champ de chaîne `newTodo` et liez-le à la valeur du texte d’entrée à l’aide de l’attribut `bind` :

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo">
   ```

1. Mettez à jour la méthode `AddTodo` pour ajouter `TodoItem` avec le titre spécifié à la liste. Supprimez la valeur du texte d’entrée en définissant `newTodo` sur une chaîne vide :

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. Régénérez et exécutez l’application. Ajoutez quelques éléments todo à la liste Todo pour tester le nouveau code.

1. Le texte du titre pour chaque élément todo peut être rendu modifiable et une case à cocher peut aider l’utilisateur à effectuer le suivi des éléments terminés. Ajoutez une entrée de case à cocher pour chaque élément todo et liez sa valeur à la propriété `IsDone`. Remplacez `@todo.Title` par un élément `<input>` lié à `@todo.Title` :

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. Pour vérifier que ces valeurs sont liées, mettez à jour l’en-tête `<h1>` pour afficher le nombre d’éléments todo qui ne sont pas terminés (`IsDone` est `false`).

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. Le composant Todo complet (*Components/Pages/Todo.razor* ou *Pages/Todo.cshtml* dans Blazor) :

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Todo.razor)]

1. Régénérez et exécutez l’application. Ajoutez des éléments todo pour tester le nouveau code.

## <a name="publish-and-deploy-the-app"></a>Publier et déployer l’application

Pour publier l’application, consultez <xref:host-and-deploy/razor-components-blazor/index>.
