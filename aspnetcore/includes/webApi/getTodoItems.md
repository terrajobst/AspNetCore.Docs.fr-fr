::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Le code précédent définit une classe de contrôleur d’API sans méthodes. Dans les sections suivantes, des méthodes sont ajoutées pour implémenter l’API.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Le code précédent définit une classe de contrôleur d’API sans méthodes. Dans les sections suivantes, des méthodes sont ajoutées pour implémenter l’API. La classe est annotée avec un attribut `[ApiController]` pour activer certaines fonctionnalités utiles. Pour plus d’informations sur les fonctionnalités activées par l’attribut, consultez [Annoter la classe avec ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).
::: moniker-end

Le constructeur du contrôleur utilise une [injection de dépendances](xref:fundamentals/dependency-injection) pour injecter le contexte de base de données (`TodoContext`) dans le contrôleur. Le contexte de base de données est utilisé dans chacune des méthodes la [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) du contrôleur. Le constructeur ajoute un élément à la base de données en mémoire s’il n’existe pas.

## <a name="get-to-do-items"></a>Obtenir les tâches

Pour obtenir les tâches, ajoutez les méthodes suivantes à la classe `TodoController` :

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end

Ces méthodes implémentent les deux méthodes GET :

* `GET /api/todo`
* `GET /api/todo/{id}`

Voici un exemple de réponse HTTP pour la méthode `GetAll` :

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

Dans la suite de ce tutoriel, je vous montrerai comment afficher la réponse HTTP avec [Postman](https://www.getpostman.com/) ou [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).

### <a name="routing-and-url-paths"></a>Routage et chemins d’URL

L’attribut `[HttpGet]` désigne une méthode qui répond à une requête HTTP GET. Le chemin d’URL pour chaque méthode est construit comme suit :

* Prenez le modèle de chaîne dans l’attribut `Route` du contrôleur :

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end

* Remplacez `[controller]` par le nom du contrôleur, qui est le nom de la classe du contrôleur sans le suffixe « Controller ». Pour cet exemple, le nom de classe du contrôleur est **Todo**Controller et le nom racine est « todo ». Le [routage](xref:mvc/controllers/routing) d’ASP.NET Core ne respecte pas la casse.
* Si l’attribut `[HttpGet]` a un modèle de route (comme `[HttpGet("/products")]`, ajoutez-le au chemin. Cet exemple n’utilise pas de modèle. Pour plus d’informations, consultez [Routage par attributs avec des attributs Http[Verbe]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

Dans la méthode `GetById` suivante, `"{id}"` est une variable d’espace réservé pour l’identificateur unique de la tâche. Quand `GetById` est appelée, elle affecte la valeur de `"{id}"` dans l’URL au paramètre `id` de la méthode.

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

`Name = "GetTodo"` crée un itinéraire nommé. Les itinéraires nommés :

* permettent à l’application créer une liaison HTTP à l’aide du nom de l’itinéraire ;
* sont expliqués dans la suite du didacticiel.

### <a name="return-values"></a>Valeurs de retour

La méthode `GetAll` retourne une collection d’objets `TodoItem`. MVC sérialise automatiquement l’objet en [JSON](https://www.json.org/) et écrit le JSON dans le corps du message de réponse. Le code de réponse pour cette méthode est 200, en supposant qu’il n’existe pas d’exception non gérée. Les exceptions non gérées sont converties en erreurs 5xx.

::: moniker range="<= aspnetcore-2.0"
En revanche, la méthode `GetById` retourne le [type IActionResult](xref:web-api/action-return-types#iactionresult-type) plus général qui représente une grande variété de types de retour. `GetById` a deux types de retour différents :

* Si aucun élément ne correspond à l’ID demandé, la méthode retourne une erreur 404. Le retour de [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) entraîne une réponse HTTP 404.
* Sinon, la méthode retourne 200 avec un corps de réponse JSON. Le retour de [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) entraîne une réponse HTTP 200.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
En revanche, la méthode `GetById` retourne le [type ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type) qui représente une grande variété de types de retour. `GetById` a deux types de retour différents :

* Si aucun élément ne correspond à l’ID demandé, la méthode retourne une erreur 404. Le retour de [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) entraîne une réponse HTTP 404.
* Sinon, la méthode retourne 200 avec un corps de réponse JSON. Le retour de `item` entraîne une réponse HTTP 200.
::: moniker-end
