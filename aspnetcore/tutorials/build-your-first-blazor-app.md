---
title: Créer votre première application Blazor
author: guardrex
description: Créez une application Blazor étape par étape.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/19/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: d48b891127f4db929b631c0ddf199c07658e604c
ms.sourcegitcommit: b4ef2b00f3e1eb287138f8b43c811cb35a100d3e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65970122"
---
# <a name="build-your-first-blazor-app"></a>Créer votre première application Blazor

Par [Daniel Roth](https://github.com/danroth27) et [Luke Latham](https://github.com/guardrex)

Ce tutoriel vous montre comment créer et modifier une application Blazor.

Suivez les instructions de l’article <xref:blazor/get-started> afin de créer un projet Blazor pour ce tutoriel.

## <a name="build-components"></a>Construire des composants

1. Accédez à chacune des trois pages de l’application dans le dossier *Pages* : Accueil, Compteur et Extraire des données. Ces pages sont implémentées par les fichiers de composants Razor *Index.razor*, *Counter.razor* et *FetchData.razor*.

1. Sur la page Counter, sélectionnez le bouton **Click me** pour incrémenter le compteur sans actualisation de la page. L’incrémentation d’un compteur dans une page web nécessite normalement l’écriture de code JavaScript. Toutefois, Blazor fournit une meilleure approche grâce à l’utilisation du langage C#.

1. Examinez l’implémentation du composant Counter dans le fichier *Counter.razor*.

   *Pages/Counter.razor* :

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   L’interface utilisateur du composant Counter est définie à l’aide de HTML. La logique de rendu dynamique (par exemple, les boucles, conditions, expressions) est ajoutée à l’aide d’une syntaxe C# intégrée appelée [Razor](xref:mvc/views/razor). Le balisage HTML et la logique de rendu C# sont convertis en une classe de composants au moment de la génération. Le nom de la classe .NET générée correspond à celui du fichier.

   Les membres de la classe de composants sont définis dans un bloc `@functions`. Dans le bloc `@functions`, l’état du composant (propriétés, champs) et les méthodes sont spécifiés pour la gestion des événements ou pour définir une autre logique de composant. Ces membres sont ensuite utilisés dans le cadre de la logique de rendu du composant et la gestion des événements.

   Lorsque le bouton **Click me** est sélectionné :

   * Le gestionnaire `onclick` enregistré du composant Counter est appelé (méthode `IncrementCount`).
   * Le composant Counter regénère son arborescence de rendu.
   * La nouvelle arborescence de rendu est comparée à la précédente.
   * Seules les modifications apportées à Document Object Model (DOM) sont appliquées. Le compte affiché est mis à jour.

1. Modifiez la logique C# du composant Counter pour que l’incrément du compte passe de un à deux.

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. Régénérez et exécutez l’application pour voir les modifications. Sélectionnez le bouton **Click me**. Le compteur est incrémenté de deux.

## <a name="use-components"></a>Utiliser des composants

Incluez un composant dans un autre composant utilisant une syntaxe HTML.

1. Ajoutez le composant Counter au composant Index de l’application en ajoutant un élément `<Counter />` au composant Index (*Index.razor*).

   Si vous utilisez Blazor côté client pour cette expérience, un composant Survey Prompt (élément `<SurveyPrompt>`) se trouve dans le composant Index. Remplacez l’élément `<SurveyPrompt>` par l’élément `<Counter>`. Si vous utilisez une application côté serveur Blazor pour cette expérience, ajoutez l’élément `<Counter>` au composant Index :

   *Pages/Index.razor* :

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. Régénérez et exécutez l’application. Le composant Index a son propre compteur.

## <a name="component-parameters"></a>Paramètres de composant

Les composants peuvent également avoir des paramètres. Les paramètres des composants sont définis à l’aide de propriétés non publiques sur la classe de composants décorée avec `[Parameter]`. Utilisez des attributs pour spécifier des arguments pour un composant dans le balisage.

1. Mettez à jour le code C# `@functions` du composant :

   * Ajoutez une propriété `IncrementAmount` décorée avec l’attribut `[Parameter]`.
   * Modifiez la méthode `IncrementCount` pour utiliser `IncrementAmount` lorsque vous augmentez la valeur de `currentCount`.

   *Pages/Counter.razor* :

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. Spécifiez un paramètre `IncrementAmount` dans l’élément `<Counter>` du composant Index à l’aide d’un attribut. Définissez la valeur pour incrémenter le compteur par 10.

   *Pages/Index.razor* :

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. Recharger le composant Index. Le compteur s’incrémente de dix unités chaque fois que le bouton **Cliquer ici** est sélectionné. Le compteur dans le composant Counter continue à être incrémenté d’un.

## <a name="route-to-components"></a>Acheminer vers les composants

La directive `@page` en haut du fichier *Counter.razor* spécifie que le composant Counter est un point de terminaison de routage. Le composant Counter gère les requêtes envoyées à `/counter`. Sans la directive `@page`, un composant ne gère pas les requêtes acheminées, mais le composant peut toujours être utilisé par d’autres composants.

## <a name="dependency-injection"></a>Injection de dépendances

Les services enregistrés dans le conteneur de services de l’application sont disponibles pour les composants via [l’injection de dépendance (DI)](xref:fundamentals/dependency-injection). Injectez des services dans un composant à l’aide de la directive `@inject`.

Examinez les directives du composant FetchData.

S’il fonctionne avec une application côté serveur Blazor, le service `WeatherForecastService` est inscrit en tant que [singleton](xref:fundamentals/dependency-injection#service-lifetimes), de sorte qu’une instance du service est disponible dans toute l’application. La directive `@inject` est utilisée pour injecter l’instance du service `WeatherForecastService` dans le composant.

*Pages/FetchData.razor* :

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

Le composant FetchData utilise le service injecté, comme `ForecastService`, pour récupérer un tableau d’objets `WeatherForecast` :

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

S’il fonctionne avec une application côté client Blazor, `HttpClient` est injecté pour obtenir des données de prévisions météorologiques à partir des fichiers *weather.json* du dossier *wwwroot/sample-data* :

*Pages/FetchData.razor* :

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

Une boucle [\@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) est utilisée pour restituer chaque instance de prévision sous forme de ligne dans la table des données météo :

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]


## <a name="build-a-todo-list"></a>Générer une liste de tâches

Ajoutez un nouveau composant à l’application qui implémente une liste de tâches simple.

1. Ajoutez un fichier vide nommé *Todo.razor* à l’application dans le dossier *Pages* :

1. Fournissez le balisage initial pour le composant :

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. Ajoutez le composant Todo à la barre de navigation.

   Le composant NavMenu (*Shared/NavMenu.razor*) est utilisé dans la disposition de l’application. Les dispositions sont des composants qui vous permettent d’éviter la duplication de contenu dans l’application. Pour plus d'informations, consultez <xref:blazor/layouts>.

   Ajoutez un `<NavLink>` pour le composant Todo en ajoutant le balisage d’élément de liste suivant sous les éléments de liste existants dans le fichier *Shared/NavMenu.razor* :

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. Régénérez et exécutez l’application. Consultez la nouvelle page Todo pour vérifier que le lien vers le composant Todo fonctionne.

1. Si vous créez une application Blazor côté serveur, ajoutez l’espace de noms de l’application au fichier *\_Imports.razor*. L’énoncé `@using` suivant suppose que l’espace de noms de l’application est `WebApplication` :

   ```cshtml
   @using WebApplication
   ```
   
   Les applications Blazor côté client incluent l’espace de noms de l’application par défaut dans le fichier *\_Imports.razor*.

1. Ajoutez un fichier *TodoItem.cs* à la racine du projet pour contenir une classe qui représente un élément Todo. Utilisez le code C# suivant pour la classe `TodoItem` :

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. Revenez au composant Todo (*Pages/Todo.razorl*) :

   * Ajoutez un champ pour les éléments Todo dans un bloc `@functions`. Le composant Todo utilise ce champ pour maintenir l’état de la liste de tâches.
   * Ajoutez un balisage de liste non triée et une boucle `foreach` pour effectuer le rendu de chaque élément todo en tant qu’élément de liste.

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. L’application nécessite des éléments d’interface utilisateur pour ajouter des éléments todo à la liste. Ajoutez une entrée de texte et un bouton sous la liste :

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. Régénérez et exécutez l’application. Lorsque le bouton **Add todo** est sélectionné, rien ne se produit car aucun gestionnaire d’événements n’est lié au bouton.

1. Ajoutez une méthode `AddTodo` au composant Todo et enregistrez-la pour les clics sur le bouton à l’aide de l’attribut `onclick` :

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   La méthode C# `AddTodo` est appelée lorsque le bouton est sélectionné.

1. Pour obtenir le titre du nouvel élément todo, ajoutez un champ de chaîne `newTodo` et liez-le à la valeur du texte d’entrée à l’aide de l’attribut `bind` :

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo" />
   ```

1. Mettez à jour la méthode `AddTodo` pour ajouter `TodoItem` avec le titre spécifié à la liste. Supprimez la valeur du texte d’entrée en définissant `newTodo` sur une chaîne vide :

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. Régénérez et exécutez l’application. Ajoutez quelques éléments todo à la liste Todo pour tester le nouveau code.

1. Le texte du titre pour chaque élément todo peut être rendu modifiable et une case à cocher peut aider l’utilisateur à effectuer le suivi des éléments terminés. Ajoutez une entrée de case à cocher pour chaque élément todo et liez sa valeur à la propriété `IsDone`. Remplacez `@todo.Title` par un élément `<input>` lié à `@todo.Title` :

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. Pour vérifier que ces valeurs sont liées, mettez à jour l’en-tête `<h1>` pour afficher le nombre d’éléments todo qui ne sont pas terminés (`IsDone` est `false`).

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. Le composant Todo (*Pages/Todo.razor*) terminé :

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. Régénérez et exécutez l’application. Ajoutez des éléments todo pour tester le nouveau code.

## <a name="publish-and-deploy-the-app"></a>Publier et déployer l’application

Pour publier l’application, consultez <xref:host-and-deploy/blazor/index>.
