::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="e6570-101">Le code précédent définit une classe de contrôleur d’API sans méthodes.</span><span class="sxs-lookup"><span data-stu-id="e6570-101">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="e6570-102">Dans les sections suivantes, des méthodes sont ajoutées pour implémenter l’API.</span><span class="sxs-lookup"><span data-stu-id="e6570-102">In the next sections, methods are added to implement the API.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="e6570-103">Le code précédent définit une classe de contrôleur d’API sans méthodes.</span><span class="sxs-lookup"><span data-stu-id="e6570-103">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="e6570-104">Dans les sections suivantes, des méthodes sont ajoutées pour implémenter l’API.</span><span class="sxs-lookup"><span data-stu-id="e6570-104">In the next sections, methods are added to implement the API.</span></span> <span data-ttu-id="e6570-105">La classe est annotée avec un attribut `[ApiController]` pour activer certaines fonctionnalités utiles.</span><span class="sxs-lookup"><span data-stu-id="e6570-105">The class is annotated with an `[ApiController]` attribute to enable some convenient features.</span></span> <span data-ttu-id="e6570-106">Pour plus d’informations sur les fonctionnalités activées par l’attribut, consultez [Annoter la classe avec ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="e6570-106">For information on features enabled by the attribute, see [Annotate class with ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span></span>
::: moniker-end

<span data-ttu-id="e6570-107">Le constructeur du contrôleur utilise une [injection de dépendances](xref:fundamentals/dependency-injection) pour injecter le contexte de base de données (`TodoContext`) dans le contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e6570-107">The controller's constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="e6570-108">Le contexte de base de données est utilisé dans chacune des méthodes la [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) du contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e6570-108">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span> <span data-ttu-id="e6570-109">Le constructeur ajoute un élément à la base de données en mémoire s’il n’existe pas.</span><span class="sxs-lookup"><span data-stu-id="e6570-109">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="get-to-do-items"></a><span data-ttu-id="e6570-110">Obtenir les tâches</span><span class="sxs-lookup"><span data-stu-id="e6570-110">Get to-do items</span></span>

<span data-ttu-id="e6570-111">Pour obtenir les tâches, ajoutez les méthodes suivantes à la classe `TodoController` :</span><span class="sxs-lookup"><span data-stu-id="e6570-111">To get to-do items, add the following methods to the `TodoController` class:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end

<span data-ttu-id="e6570-112">Ces méthodes implémentent les deux méthodes GET :</span><span class="sxs-lookup"><span data-stu-id="e6570-112">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="e6570-113">Voici un exemple de réponse HTTP pour la méthode `GetAll` :</span><span class="sxs-lookup"><span data-stu-id="e6570-113">Here's a sample HTTP response for the `GetAll` method:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

<span data-ttu-id="e6570-114">Dans la suite de ce tutoriel, je vous montrerai comment afficher la réponse HTTP avec [Postman](https://www.getpostman.com/) ou [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span><span class="sxs-lookup"><span data-stu-id="e6570-114">Later in the tutorial, I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="e6570-115">Routage et chemins d’URL</span><span class="sxs-lookup"><span data-stu-id="e6570-115">Routing and URL paths</span></span>

<span data-ttu-id="e6570-116">L’attribut `[HttpGet]` désigne une méthode qui répond à une requête HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="e6570-116">The `[HttpGet]` attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="e6570-117">Le chemin d’URL pour chaque méthode est construit comme suit :</span><span class="sxs-lookup"><span data-stu-id="e6570-117">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="e6570-118">Prenez le modèle de chaîne dans l’attribut `Route` du contrôleur :</span><span class="sxs-lookup"><span data-stu-id="e6570-118">Take the template string in the controller's `Route` attribute:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end

* <span data-ttu-id="e6570-119">Remplacez `[controller]` par le nom du contrôleur, qui est le nom de la classe du contrôleur sans le suffixe « Controller ».</span><span class="sxs-lookup"><span data-stu-id="e6570-119">Replace `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="e6570-120">Pour cet exemple, le nom de classe du contrôleur est **Todo**Controller et le nom racine est « todo ».</span><span class="sxs-lookup"><span data-stu-id="e6570-120">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="e6570-121">Le [routage](xref:mvc/controllers/routing) d’ASP.NET Core ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="e6570-121">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="e6570-122">Si l’attribut `[HttpGet]` a un modèle de route (comme `[HttpGet("/products")]`, ajoutez-le au chemin.</span><span class="sxs-lookup"><span data-stu-id="e6570-122">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="e6570-123">Cet exemple n’utilise pas de modèle.</span><span class="sxs-lookup"><span data-stu-id="e6570-123">This sample doesn't use a template.</span></span> <span data-ttu-id="e6570-124">Pour plus d’informations, consultez [Routage par attributs avec des attributs Http[Verbe]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="e6570-124">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="e6570-125">Dans la méthode `GetById` suivante, `"{id}"` est une variable d’espace réservé pour l’identificateur unique de la tâche.</span><span class="sxs-lookup"><span data-stu-id="e6570-125">In the following `GetById` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="e6570-126">Quand `GetById` est appelée, elle affecte la valeur de `"{id}"` dans l’URL au paramètre `id` de la méthode.</span><span class="sxs-lookup"><span data-stu-id="e6570-126">When `GetById` is invoked, it assigns the value of `"{id}"` in the URL to the method's `id` parameter.</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

<span data-ttu-id="e6570-127">`Name = "GetTodo"` crée un itinéraire nommé.</span><span class="sxs-lookup"><span data-stu-id="e6570-127">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="e6570-128">Les itinéraires nommés :</span><span class="sxs-lookup"><span data-stu-id="e6570-128">Named routes:</span></span>

* <span data-ttu-id="e6570-129">permettent à l’application créer une liaison HTTP à l’aide du nom de l’itinéraire ;</span><span class="sxs-lookup"><span data-stu-id="e6570-129">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="e6570-130">sont expliqués dans la suite du didacticiel.</span><span class="sxs-lookup"><span data-stu-id="e6570-130">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="e6570-131">Valeurs de retour</span><span class="sxs-lookup"><span data-stu-id="e6570-131">Return values</span></span>

<span data-ttu-id="e6570-132">La méthode `GetAll` retourne une collection d’objets `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="e6570-132">The `GetAll` method returns a collection of `TodoItem` objects.</span></span> <span data-ttu-id="e6570-133">MVC sérialise automatiquement l’objet en [JSON](https://www.json.org/) et écrit le JSON dans le corps du message de réponse.</span><span class="sxs-lookup"><span data-stu-id="e6570-133">MVC automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="e6570-134">Le code de réponse pour cette méthode est 200, en supposant qu’il n’existe pas d’exception non gérée.</span><span class="sxs-lookup"><span data-stu-id="e6570-134">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="e6570-135">Les exceptions non gérées sont converties en erreurs 5xx.</span><span class="sxs-lookup"><span data-stu-id="e6570-135">Unhandled exceptions are translated into 5xx errors.</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="e6570-136">En revanche, la méthode `GetById` retourne le [type IActionResult](xref:web-api/action-return-types#iactionresult-type) plus général qui représente une grande variété de types de retour.</span><span class="sxs-lookup"><span data-stu-id="e6570-136">In contrast, the `GetById` method returns the more general [IActionResult type](xref:web-api/action-return-types#iactionresult-type), which represents a wide range of return types.</span></span> <span data-ttu-id="e6570-137">`GetById` a deux types de retour différents :</span><span class="sxs-lookup"><span data-stu-id="e6570-137">`GetById` has two different return types:</span></span>

* <span data-ttu-id="e6570-138">Si aucun élément ne correspond à l’ID demandé, la méthode retourne une erreur 404.</span><span class="sxs-lookup"><span data-stu-id="e6570-138">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="e6570-139">Le retour de [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) entraîne une réponse HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="e6570-139">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="e6570-140">Sinon, la méthode retourne 200 avec un corps de réponse JSON.</span><span class="sxs-lookup"><span data-stu-id="e6570-140">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="e6570-141">Le retour de [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) entraîne une réponse HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="e6570-141">Returning [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) results in an HTTP 200 response.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="e6570-142">En revanche, la méthode `GetById` retourne le [type ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type) qui représente une grande variété de types de retour.</span><span class="sxs-lookup"><span data-stu-id="e6570-142">In contrast, the `GetById` method returns the [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type), which represents a wide range of return types.</span></span> <span data-ttu-id="e6570-143">`GetById` a deux types de retour différents :</span><span class="sxs-lookup"><span data-stu-id="e6570-143">`GetById` has two different return types:</span></span>

* <span data-ttu-id="e6570-144">Si aucun élément ne correspond à l’ID demandé, la méthode retourne une erreur 404.</span><span class="sxs-lookup"><span data-stu-id="e6570-144">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="e6570-145">Le retour de [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) entraîne une réponse HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="e6570-145">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="e6570-146">Sinon, la méthode retourne 200 avec un corps de réponse JSON.</span><span class="sxs-lookup"><span data-stu-id="e6570-146">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="e6570-147">Le retour de `item` entraîne une réponse HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="e6570-147">Returning `item` results in an HTTP 200 response.</span></span>
::: moniker-end
