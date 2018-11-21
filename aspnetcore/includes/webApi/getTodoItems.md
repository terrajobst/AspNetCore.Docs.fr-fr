::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="017ad-101">Le code précédent définit une classe de contrôleur d’API sans méthodes.</span><span class="sxs-lookup"><span data-stu-id="017ad-101">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="017ad-102">Dans les sections suivantes, des méthodes sont ajoutées pour implémenter l’API.</span><span class="sxs-lookup"><span data-stu-id="017ad-102">In the next sections, methods are added to implement the API.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="017ad-103">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="017ad-103">The preceding code:</span></span>

* <span data-ttu-id="017ad-104">Définit une classe de contrôleur d’API sans méthodes.</span><span class="sxs-lookup"><span data-stu-id="017ad-104">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="017ad-105">Crée un nouvel élément Todo lorsque `TodoItems` est vide.</span><span class="sxs-lookup"><span data-stu-id="017ad-105">Creates a new Todo item when `TodoItems` is empty.</span></span> <span data-ttu-id="017ad-106">Vous ne pourrez pas supprimer tous les éléments Todo, car le constructeur en crée un nouveau lorsque `TodoItems` est vide.</span><span class="sxs-lookup"><span data-stu-id="017ad-106">You won't be able to delete all the Todo items because the constructor creates a new one if `TodoItems` is empty.</span></span>

<span data-ttu-id="017ad-107">Dans les sections suivantes, des méthodes sont ajoutées pour implémenter l’API.</span><span class="sxs-lookup"><span data-stu-id="017ad-107">In the next sections, methods are added to implement the API.</span></span> <span data-ttu-id="017ad-108">La classe est annotée avec un attribut `[ApiController]` pour activer certaines fonctionnalités utiles.</span><span class="sxs-lookup"><span data-stu-id="017ad-108">The class is annotated with an `[ApiController]` attribute to enable some convenient features.</span></span> <span data-ttu-id="017ad-109">Pour plus d’informations sur les fonctionnalités activées par l’attribut, consultez [Annotation avec l’attribut ApiController](xref:web-api/index#annotation-with-apicontroller-attribute).</span><span class="sxs-lookup"><span data-stu-id="017ad-109">For information on features enabled by the attribute, see [Annotation with ApiController attribute](xref:web-api/index#annotation-with-apicontroller-attribute).</span></span>

::: moniker-end

<span data-ttu-id="017ad-110">Le constructeur du contrôleur utilise une [injection de dépendances](xref:fundamentals/dependency-injection) pour injecter le contexte de base de données (`TodoContext`) dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="017ad-110">The controller's constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="017ad-111">Le contexte de base de données est utilisé dans chacune des méthodes la [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="017ad-111">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span> <span data-ttu-id="017ad-112">Le constructeur ajoute un élément à la base de données en mémoire s’il n’existe pas.</span><span class="sxs-lookup"><span data-stu-id="017ad-112">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="get-to-do-items"></a><span data-ttu-id="017ad-113">Obtenir les tâches</span><span class="sxs-lookup"><span data-stu-id="017ad-113">Get to-do items</span></span>

<span data-ttu-id="017ad-114">Pour obtenir les tâches, ajoutez les méthodes suivantes à la classe `TodoController` :</span><span class="sxs-lookup"><span data-stu-id="017ad-114">To get to-do items, add the following methods to the `TodoController` class:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

::: moniker-end

<span data-ttu-id="017ad-115">Ces méthodes implémentent les deux méthodes GET :</span><span class="sxs-lookup"><span data-stu-id="017ad-115">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="017ad-116">Voici un exemple de réponse HTTP pour la méthode `GetAll` :</span><span class="sxs-lookup"><span data-stu-id="017ad-116">Here's a sample HTTP response for the `GetAll` method:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

<span data-ttu-id="017ad-117">Dans la suite de ce tutoriel, je vous montrerai comment afficher la réponse HTTP avec [Postman](https://www.getpostman.com/) ou [curl](https://curl.haxx.se/docs/manpage.html).</span><span class="sxs-lookup"><span data-stu-id="017ad-117">Later in the tutorial, I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://curl.haxx.se/docs/manpage.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="017ad-118">Routage et chemins d’URL</span><span class="sxs-lookup"><span data-stu-id="017ad-118">Routing and URL paths</span></span>

<span data-ttu-id="017ad-119">L’attribut `[HttpGet]` désigne une méthode qui répond à une requête HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="017ad-119">The `[HttpGet]` attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="017ad-120">Le chemin d’URL pour chaque méthode est construit comme suit :</span><span class="sxs-lookup"><span data-stu-id="017ad-120">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="017ad-121">Prenez le modèle de chaîne dans l’attribut `Route` du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="017ad-121">Take the template string in the controller's `Route` attribute:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

::: moniker-end

* <span data-ttu-id="017ad-122">Remplacez `[controller]` par le nom du contrôleur qui, par convention, est le nom de la classe du contrôleur sans le suffixe « Controller ».</span><span class="sxs-lookup"><span data-stu-id="017ad-122">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="017ad-123">Pour cet exemple, le nom de classe du contrôleur est **Todo**Controller et le nom racine est « todo ».</span><span class="sxs-lookup"><span data-stu-id="017ad-123">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="017ad-124">Le [routage](xref:mvc/controllers/routing) d’ASP.NET Core ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="017ad-124">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="017ad-125">Si l’attribut `[HttpGet]` a un modèle de route (comme `[HttpGet("/products")]`, ajoutez-le au chemin.</span><span class="sxs-lookup"><span data-stu-id="017ad-125">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="017ad-126">Cet exemple n’utilise pas de modèle.</span><span class="sxs-lookup"><span data-stu-id="017ad-126">This sample doesn't use a template.</span></span> <span data-ttu-id="017ad-127">Pour plus d’informations, consultez [Routage par attributs avec des attributs Http[Verbe]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="017ad-127">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="017ad-128">Dans la méthode `GetById` suivante, `"{id}"` est une variable d’espace réservé pour l’identificateur unique de la tâche.</span><span class="sxs-lookup"><span data-stu-id="017ad-128">In the following `GetById` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="017ad-129">Quand `GetById` est appelée, elle affecte la valeur de `"{id}"` dans l’URL au paramètre `id` de la méthode.</span><span class="sxs-lookup"><span data-stu-id="017ad-129">When `GetById` is invoked, it assigns the value of `"{id}"` in the URL to the method's `id` parameter.</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

::: moniker-end

<span data-ttu-id="017ad-130">`Name = "GetTodo"` crée un itinéraire nommé.</span><span class="sxs-lookup"><span data-stu-id="017ad-130">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="017ad-131">Les itinéraires nommés :</span><span class="sxs-lookup"><span data-stu-id="017ad-131">Named routes:</span></span>

* <span data-ttu-id="017ad-132">permettent à l’application créer une liaison HTTP à l’aide du nom de l’itinéraire ;</span><span class="sxs-lookup"><span data-stu-id="017ad-132">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="017ad-133">sont expliqués dans la suite du didacticiel.</span><span class="sxs-lookup"><span data-stu-id="017ad-133">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="017ad-134">Valeurs de retour</span><span class="sxs-lookup"><span data-stu-id="017ad-134">Return values</span></span>

<span data-ttu-id="017ad-135">La méthode `GetAll` retourne une collection d’objets `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="017ad-135">The `GetAll` method returns a collection of `TodoItem` objects.</span></span> <span data-ttu-id="017ad-136">MVC sérialise automatiquement l’objet en [JSON](https://www.json.org/) et écrit le JSON dans le corps du message de réponse.</span><span class="sxs-lookup"><span data-stu-id="017ad-136">MVC automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="017ad-137">Le code de réponse pour cette méthode est 200, en supposant qu’il n’existe pas d’exception non gérée.</span><span class="sxs-lookup"><span data-stu-id="017ad-137">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="017ad-138">Les exceptions non gérées sont converties en erreurs 5xx.</span><span class="sxs-lookup"><span data-stu-id="017ad-138">Unhandled exceptions are translated into 5xx errors.</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="017ad-139">En revanche, la méthode `GetById` retourne le [type IActionResult](xref:web-api/action-return-types#iactionresult-type) plus général qui représente une grande variété de types de retour.</span><span class="sxs-lookup"><span data-stu-id="017ad-139">In contrast, the `GetById` method returns the more general [IActionResult type](xref:web-api/action-return-types#iactionresult-type), which represents a wide range of return types.</span></span> <span data-ttu-id="017ad-140">`GetById` a deux types de retour différents :</span><span class="sxs-lookup"><span data-stu-id="017ad-140">`GetById` has two different return types:</span></span>

* <span data-ttu-id="017ad-141">Si aucun élément ne correspond à l’ID demandé, la méthode retourne une erreur 404.</span><span class="sxs-lookup"><span data-stu-id="017ad-141">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="017ad-142">Le retour de [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) entraîne une réponse HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="017ad-142">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="017ad-143">Sinon, la méthode retourne 200 avec un corps de réponse JSON.</span><span class="sxs-lookup"><span data-stu-id="017ad-143">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="017ad-144">Le retour de [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) entraîne une réponse HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="017ad-144">Returning [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) results in an HTTP 200 response.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="017ad-145">En revanche, la méthode `GetById` retourne le [type ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type) qui représente une grande variété de types de retour.</span><span class="sxs-lookup"><span data-stu-id="017ad-145">In contrast, the `GetById` method returns the [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type), which represents a wide range of return types.</span></span> <span data-ttu-id="017ad-146">`GetById` a deux types de retour différents :</span><span class="sxs-lookup"><span data-stu-id="017ad-146">`GetById` has two different return types:</span></span>

* <span data-ttu-id="017ad-147">Si aucun élément ne correspond à l’ID demandé, la méthode retourne une erreur 404.</span><span class="sxs-lookup"><span data-stu-id="017ad-147">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="017ad-148">Le retour de [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) entraîne une réponse HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="017ad-148">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="017ad-149">Sinon, la méthode retourne 200 avec un corps de réponse JSON.</span><span class="sxs-lookup"><span data-stu-id="017ad-149">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="017ad-150">Le retour de `item` entraîne une réponse HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="017ad-150">Returning `item` results in an HTTP 200 response.</span></span>

::: moniker-end
