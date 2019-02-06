---
title: Créer votre première application Blazor
author: guardrex
description: Créez une application Blazor pas à pas et découvrez rapidement les fonctionnalités de base de l’infrastructure Composants Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: 99396e200eb121524abccc20a7d461062c94ecc7
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667995"
---
# <a name="build-your-first-blazor-app"></a>Créer votre première application Blazor

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Dans ce tutoriel, vous créez une application Blazor pas à pas et découvrez rapidement les fonctionnalités de base de l’infrastructure Composants Razor.

[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample)). Consultez la rubrique [Bien démarrer](xref:razor-components/get-started) pour connaître les prérequis.

Pour créer le projet dans Visual Studio :

1. Sélectionnez **Fichier** > **Nouveau** > **Projet**. Sélectionnez **Web** > **Application web ASP.NET Core**. Nommez le projet « BlazorApp1 » dans le champ **Nom**. Sélectionnez **OK**.

    ![Nouveau projet ASP.NET Core](build-your-first-blazor-app/_static/new-aspnet-core-project.png)

1. La boîte de dialogue **Nouvelle application web ASP.NET Core** s’affiche. Assurez-vous que **.NET Core** est sélectionné en haut. Sélectionnez **ASP.NET Core 2.1**. Choisissez le modèle **Blazor** et sélectionnez **OK**.

    ![Boîte de dialogue Nouvelle application Blazor](build-your-first-blazor-app/_static/new-blazor-app-dialog.png)

1. Une fois que le projet est créé, appuyez sur **Ctrl-F5** pour exécuter l’application *sans le débogueur*. L’exécution avec le débogueur (**F5**) n’est pas prise en charge pour l’instant.

> [!NOTE]
> Si vous n’utilisez pas Visual Studio, créez l’application Blazor à l’invite de commandes sur Windows, macOS ou Linux :
>
> ```console
> dotnet new --install "Microsoft.AspNetCore.Blazor.Templates"
> dotnet new blazor -o BlazorApp1
> cd BlazorApp1
> dotnet run
> ```
>
> Accédez à l’application à l’aide du port et de l’adresse localhost fournis dans la sortie de la fenêtre de console après l’exécution de `dotnet run`. Utilisez **Ctrl-C** dans la fenêtre de console pour arrêter l’application.

L’application Blazor s’exécute dans le navigateur :

![Page d’accueil d’application Blazor](https://user-images.githubusercontent.com/1874516/39509497-5515c3ea-4d9b-11e8-887f-019ea4fdb3ee.png)

## <a name="build-components"></a>Construire des composants

1. Accédez à chacune des trois pages de l’application : Accueil, Compteur et Extraire des données.

    Ces trois pages sont implémentées par les trois fichiers Razor dans le dossier *Pages* : *Index.cshtml*, *Counter.cshtml* et *FetchData.cshtml*. Chacun de ces fichiers implémente un composant qui est compilé et exécuté côté client dans le navigateur.

1. Sélectionnez le bouton sur la page Compteur.

    ![Page d’accueil d’application Blazor](https://user-images.githubusercontent.com/1874516/39509525-6e367c66-4d9b-11e8-9978-e52a9750c34b.png)

    À chaque fois que le bouton est sélectionné, le compteur est incrémenté sans actualisation de la page. En règle générale, ce type de comportement côté client est contrôlé dans JavaScript ; mais ici, il est implémenté dans C# et .NET par le composant `Counter`.

1. Examinons l’implémentation du composant `Counter` dans le fichier *Counter.cshtml* :

    ```cshtml
    @page "/counter"

    <h1>Counter</h1>

    <p>Current count: @currentCount</p>

    <button class="btn btn-primary" onclick="@IncrementCount">Click me</button>

    @functions {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount++;
        }
    }
    ```

    L’interface utilisateur pour le composant `Counter` est définie à l’aide de code HTML normal. La logique de rendu dynamique (par exemple, les boucles, conditions, expressions) est ajoutée à l’aide d’une syntaxe C# intégrée appelée [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor). Le balisage HTML et la logique de rendu C# sont convertis en une classe de composants au moment de la génération. Le nom de la classe .NET générée correspond à celui du fichier.

    Les membres de la classe de composants sont définis dans un bloc `@functions`. Dans le bloc `@functions`, l’état du composant (propriétés, champs) et des méthodes peuvent être spécifiés pour la gestion des événements ou pour définir une autre logique de composant. Ces membres peuvent ensuite servir dans le cadre de la logique de rendu du composant et la gestion des événements.

    Lorsque le bouton est sélectionné, le gestionnaire `onclick` du composant `Counter` est appelé (méthode `IncrementCount`) et le composant `Counter` régénère son arborescence de rendu. Blazor compare la nouvelle arborescence de rendu avec la précédente et applique les modifications au modèle DOM (Document Object Model) du navigateur. Le compte affiché est mis à jour.

1. Mettez à jour le balisage pour le composant `Counter` pour rendre l’en-tête de niveau supérieur plus *passionnant*.

    ```cshtml
    @page "/counter"
    <h1><em>Counter!!</em></h1>
    ```

1. Modifiez aussi la logique C# du composant `Counter` pour que l’incrément du compte passe de un à deux.

    ```cshtml
    @functions {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount += 2;
        }
    }
    ```

1. Actualisez la page du compteur dans le navigateur pour voir les modifications.

    ![Compteur passionnant](https://user-images.githubusercontent.com/1874516/39509668-e8949a92-4d9b-11e8-91e9-b6a494695d92.png)

## <a name="use-components"></a>Utiliser des composants

Une fois un composant défini, il peut être utilisé pour implémenter d’autres composants. Le balisage pour l’utilisation d’un composant ressemble à une balise HTML où le nom de la balise est le type du composant.

1. Ajoutez un composant `Counter` à la page Accueil de l’application (*Index.cshtml*).

    ```cshtml
    @page "/"

    <h1>Hello, world!</h1>

    Welcome to your new app.

    <SurveyPrompt Title="How is Blazor working for you?" />

    <Counter />
    ```

1. Actualisez la page d’accueil dans le navigateur. Notez l’instance distincte du composant `Counter` sur la page d’accueil.

    ![Page d’accueil Blazor avec compteur](https://user-images.githubusercontent.com/1874516/39509718-224483f6-4d9c-11e8-9030-b4c7228d669d.png)

## <a name="component-parameters"></a>Paramètres de composant

Les composants peuvent également avoir des paramètres qui sont définies à l’aide de propriétés privées sur la classe de composants décorée avec `[Parameter]`. Utilisez des attributs pour spécifier des arguments pour un composant dans le balisage. 

1. Mettez à jour le composant `Counter` pour avoir un paramètre `IncrementAmount` dont la valeur par défaut est 1.

    ```cshtml
    @functions {
        int currentCount = 0;

        [Parameter]
        private int IncrementAmount { get; set; } = 1;

        void IncrementCount()
        {
            currentCount += IncrementAmount;
        }
    }
    ```

    > [!NOTE]
    > À partir de Visual Studio, vous pouvez rapidement ajouter un paramètre de composant à l’aide de l’extrait de code `para`. Tapez `para`, puis appuyez sur la touche `Tab` deux fois.

1. Sur la page d’accueil (*Index.cshtml*), modifiez la quantité d’incrémentation pour `Counter` sur 10 en définissant un attribut qui correspond au nom de la propriété du composant pour `IncrementCount`.

    ```cshtml
    <Counter IncrementAmount="10" />
    ```

1. Rechargez la page.

    L’incrémentation du compteur sur la page d’accueil se fait maintenant par 10, tandis que le compteur sur la page Compteur incrémente toujours par 1.

    ![Compteur Blazor par 10](https://user-images.githubusercontent.com/1874516/39509798-618f0720-4d9c-11e8-9125-3d4c634dff46.png)

## <a name="route-to-components"></a>Acheminer vers les composants

La directive `@page` en haut du fichier *Counter.cshtml* spécifie que ce composant est une page vers laquelle les requêtes peuvent être acheminées. Plus précisément, le composant `Counter` gère les requêtes envoyées à `/Counter`. Sans la directive `@page`, le composant ne gèrerait pas les requêtes acheminées, mais le composant pourrait toujours être utilisé par d’autres composants.

## <a name="dependency-injection"></a>Injection de dépendances

Les services enregistrés auprès du fournisseur de services de l’application sont disponibles pour les composants via [l’injection de dépendance (DI)](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection). Les services peuvent être injectés dans un composant à l’aide de la directive `@inject`.

Examinons l’implémentation du composant `FetchData` dans *FetchData.cshtml*. La directive `@inject` est utilisée pour injecter une instance [HttpClient](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient) dans le composant.

```cshtml
@page "/fetchdata"
@inject HttpClient Http
```

Le composant `FetchData` utilise le `HttpClient` injecté pour récupérer des données JSON à partir du serveur lorsque le composant est initialisé. En coulisses, le `HttpClient` fourni par le runtime Blazor est implémenté à l’aide d’une interopération JavaScript pour appeler l’API Fetch sous-jacente du navigateur afin d’envoyer la requête (à partir de C#, il est possible d’appeler n’importe quelle bibliothèque JavaScript ou API du navigateur). Les données récupérées sont désérialisées dans la variable C# `forecasts` en tant que tableau d’objets `WeatherForecast`.

```cshtml
@functions {
    WeatherForecast[] forecasts;

    protected override async Task OnInitAsync()
    {
        forecasts = await Http.GetJsonAsync<WeatherForecast[]>("/sample-data/weather.json");
    }

    class WeatherForecast
    {
        public DateTime Date { get; set; }
        public int TemperatureC { get; set; }
        public int TemperatureF { get; set; }
        public string Summary { get; set; }
    }
}
```

Une boucle `@foreach` est utilisée pour restituer chaque instance de prévision sous forme de ligne dans la table sur la météo.

```cshtml
<table class="table">
    <thead>
        <tr>
            <th>Date</th>
            <th>Temp. (C)</th>
            <th>Temp. (F)</th>
            <th>Summary</th>
        </tr>
    </thead>
    <tbody>
        @foreach (var forecast in forecasts)
        {
            <tr>
                <td>@forecast.Date.ToShortDateString()</td>
                <td>@forecast.TemperatureC</td>
                <td>@forecast.TemperatureF</td>
                <td>@forecast.Summary</td>
            </tr>
        }
    </tbody>
</table>
```

## <a name="build-a-todo-list"></a>Générer une liste de tâches

Ajoutez une nouvelle page à l’application qui implémente une liste de tâches simple.

1. Ajoutez un fichier texte vide au dossier *Pages* nommé *Todo.cshtml*.

1. Fournissez le balisage initial pour la page.

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>
    ```

1. Ajouter la page Todo de la barre de navigation en mettant à jour *Shared/NavMenu.cshtml*. Ajoutez un `NavLink` pour la page Todo en ajoutant le balisage d’élément de liste suivant sous les éléments de liste existants.

    ```cshtml
    <li class="nav-item px-3">
        <NavLink class="nav-link" href="todo">
            <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
        </NavLink>
    </li>
    ```

1. Actualisez l’application dans le navigateur. Consultez la nouvelle page Todo.

    ![Démarrer la page Todo Blazor](https://user-images.githubusercontent.com/1874516/39509907-bb27e77a-4d9c-11e8-91e7-ea1e7c01729e.png)

1. Ajoutez un fichier *TodoItem.cs* à la racine du projet pour contenir une classe afin de représenter les éléments des tâches.

1. Utilisez le code C# suivant pour la classe `TodoItem`.

    ```csharp
    public class TodoItem
    {
        public string Title { get; set; }
        public bool IsDone { get; set; }
    }
    ```

1. Revenez au composant `Todo` dans *Todo.cshtml* et ajoutez un champ pour les tâches Todo dans un bloc `@functions`. Le composant `Todo` utilise ce champ pour maintenir l’état de la liste de tâches.

    ```cshtml
    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
    }
    ```

1. Ajoutez un balisage de liste non triée et une boucle `foreach` pour effectuer le rendu de chaque élément todo en tant qu’élément de liste.

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>
    ```

1. L’application nécessite des éléments d’interface utilisateur pour ajouter des éléments todo à la liste. Ajoutez une entrée de texte et un bouton sous la liste.

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>

    <input placeholder="Something todo" />
    <button>Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
    }
    ```

1. Actualisez le navigateur.

    ![Ajouter un bouton todo](https://user-images.githubusercontent.com/1874516/39512402-bc88ab46-4da5-11e8-9e3f-87b875b56383.png)

    Rien ne se produit lorsque le bouton **Add todo** est sélectionné, car aucun gestionnaire d’événements n’est lié au bouton.

1. Ajoutez une méthode `AddTodo` au composant `Todo` et enregistrez-la pour les clics sur le bouton à l’aide de l’attribut `onclick`.

    ```cshtml
    <input placeholder="Something todo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();

        void AddTodo()
        {
            // Todo: Add the todo
        }
    }
    ```

    La méthode C# `AddTodo` est appelée à chaque fois que le bouton est sélectionné.

1. Pour obtenir le titre du nouvel élément todo, ajoutez un champ de chaîne `newTodo` et liez-le à la valeur du texte d’entrée à l’aide de l’attribut `bind`.

    ```csharp
    IList<TodoItem> todos = new List<TodoItem>();
    string newTodo;
    ```

    ```cshtml
    <input placeholder="Something todo" bind="@newTodo" />
    ```

1. Mettez à jour la méthode `AddTodo` pour ajouter `TodoItem` avec le titre spécifié à la liste. N’oubliez pas de supprimer la valeur du texte d’entrée en définissant `newTodo` sur une chaîne vide.

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>

    <input placeholder="Something todo" bind="@newTodo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
        string newTodo;

        void AddTodo()
        {
            if (!string.IsNullOrWhiteSpace(newTodo))
            {
                todos.Add(new TodoItem { Title = newTodo });
                newTodo = string.Empty;
            }
        }
    }
    ```

1. Actualisez le navigateur. Ajoutez quelques éléments todo à la liste de tâches.

    ![Ajouter des éléments todo](https://user-images.githubusercontent.com/1874516/39512531-2d2ff62e-4da6-11e8-8b83-291b0efc821b.png)

1. Enfin, à quoi sert une liste de tâches sans case à cocher ? Le texte du titre pour chaque élément todo peut être modifié également. Ajoutez une entrée de case à cocher et une entrée de texte pour chaque élément todo et liez leurs valeurs aux propriétés `Title` et `IsDone`, respectivement.

    ```cshtml
    <ul>
        @foreach (var todo in todos)
        {
            <li>
                <input type="checkbox" bind="@todo.IsDone" />
                <input bind="@todo.Title" />
            </li>
        }
    </ul>
    ```

1. Pour vérifier que ces valeurs sont liées, mettez à jour l’en-tête `h1` pour afficher le nombre d’éléments todo qui ne sont pas encore effectués (`IsDone` est `false`).

    ```cshtml
    <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
    ```

1. Le composant `Todo` terminé doit ressembler à ce qui suit :

    ```cshtml
    @page "/todo"

    <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>
                <input type="checkbox" bind="@todo.IsDone" />
                <input bind="@todo.Title" />
            </li>
        }
    </ul>

    <input placeholder="Something todo" bind="@newTodo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
        string newTodo;

        void AddTodo()
        {
            if (!string.IsNullOrWhiteSpace(newTodo))
            {
                todos.Add(new TodoItem { Title = newTodo });
                newTodo = string.Empty;
            }
        }
    }
    ```

Actualisez l’application dans le navigateur. Essayez d’ajouter quelques éléments todo.

![Liste de tâches Blazor terminée](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

## <a name="publish-and-deploy"></a>Publier et déployer

Lorsque vous utilisez Visual Studio, procédez comme suit pour publier l’application Todo Blazor sur Azure :

1. Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions**, puis sélectionnez **Publier**.

1. Dans la boîte de dialogue **Choisir une cible de publication**, sélectionnez **App Service** et **Créer**. Sélectionnez **Publier**.

    ![Choisir une cible de publication](build-your-first-blazor-app/_static/blazor-publish-pick-target.png)

1. Dans la boîte de dialogue **Créer App Service**, choisissez un nom pour l’application et sélectionnez l’abonnement, le groupe de ressources et le plan d’hébergement. Sélectionnez **Créer** pour créer le service d’application et publier l’application.

    ![Créer App Service](build-your-first-blazor-app/_static/blazor-publish-create-appservice2.png)

Patientez environ une minute pour que l’application soit déployée.

L’application doit maintenant s’exécuter dans Azure. Marquez l’élément todo pour créer votre première application Blazor comme *terminé*. Bravo !

![Blazor sur Azure](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

> [!NOTE]
> Si vous n’utilisez pas Visual Studio, publiez l’application Blazor à l’invite de commandes sur Windows, macOS ou Linux :
>
> ```console
> dotnet publish -c Release
> ```
>
> Le déploiement est créé dans le dossier */bin/Release/\<target-framework>/publish*. Déplacez le contenu du dossier *publish* sur le serveur ou le service d’hébergement.
>
> Pour plus d’informations, consultez la rubrique [Héberger et déployer](xref:host-and-deploy/razor-components/index#publish-the-app).

## <a name="additional-resources"></a>Ressources supplémentaires

Pour un exemple d’application Blazor plus impliqué, consultez [l’exemple Flight Finder](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor) sur GitHub.

![Blazor Flight Finder](https://msdnshared.blob.core.windows.net/media/2018/03/blazor-flight-finder.png)
