## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="5af61-101">Implémenter les autres opérations CRUD</span><span class="sxs-lookup"><span data-stu-id="5af61-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="5af61-102">Dans les sections suivantes, les méthodes `Create`, `Update` et `Delete` sont ajoutées au contrôleur.</span><span class="sxs-lookup"><span data-stu-id="5af61-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="5af61-103">Créer</span><span class="sxs-lookup"><span data-stu-id="5af61-103">Create</span></span>

<span data-ttu-id="5af61-104">Ajoutez la méthode `Create` suivante :</span><span class="sxs-lookup"><span data-stu-id="5af61-104">Add the following `Create` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="5af61-105">Le code précédent est une méthode HTTP POST, comme indiqué par l’attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="5af61-105">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="5af61-106">L’attribut [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) indique à MVC qu’il faut obtenir la valeur de la tâche à partir du corps de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="5af61-106">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="5af61-107">Le code précédent est une méthode HTTP POST, comme indiqué par l’attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="5af61-107">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="5af61-108">MVC obtient la valeur de la tâche dans le corps de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="5af61-108">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end

<span data-ttu-id="5af61-109">La méthode `CreatedAtRoute` :</span><span class="sxs-lookup"><span data-stu-id="5af61-109">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="5af61-110">Retourne une réponse 201.</span><span class="sxs-lookup"><span data-stu-id="5af61-110">Returns a 201 response.</span></span> <span data-ttu-id="5af61-111">HTTP 201 est la réponse standard d’une méthode HTTP POST qui crée une ressource sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="5af61-111">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="5af61-112">Ajoute un en-tête Location à la réponse.</span><span class="sxs-lookup"><span data-stu-id="5af61-112">Adds a Location header to the response.</span></span> <span data-ttu-id="5af61-113">L’en-tête Location spécifie l’URI de l’élément d’action qui vient d’être créé.</span><span class="sxs-lookup"><span data-stu-id="5af61-113">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="5af61-114">Consultez [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="5af61-114">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="5af61-115">Utilise l’itinéraire nommé « GetTodo » pour créer l’URL.</span><span class="sxs-lookup"><span data-stu-id="5af61-115">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="5af61-116">L’itinéraire nommé « GetTodo » est défini dans `GetById` :</span><span class="sxs-lookup"><span data-stu-id="5af61-116">The "GetTodo" named route is defined in `GetById`:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="5af61-117">Utiliser Postman pour envoyer une requête Create</span><span class="sxs-lookup"><span data-stu-id="5af61-117">Use Postman to send a Create request</span></span>

* <span data-ttu-id="5af61-118">Démarrez l'application.</span><span class="sxs-lookup"><span data-stu-id="5af61-118">Start the app.</span></span>
* <span data-ttu-id="5af61-119">Ouvrez Postman.</span><span class="sxs-lookup"><span data-stu-id="5af61-119">Open Postman.</span></span>

![Console Postman](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="5af61-121">Mettez à jour le numéro de port dans l’URL localhost.</span><span class="sxs-lookup"><span data-stu-id="5af61-121">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="5af61-122">Définissez la méthode HTTP sur *POST*.</span><span class="sxs-lookup"><span data-stu-id="5af61-122">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="5af61-123">Cliquez sur l’onglet **Body** (Corps).</span><span class="sxs-lookup"><span data-stu-id="5af61-123">Click the **Body** tab.</span></span>
* <span data-ttu-id="5af61-124">Sélectionnez la case d’option **raw** (données brutes).</span><span class="sxs-lookup"><span data-stu-id="5af61-124">Select the **raw** radio button.</span></span>
* <span data-ttu-id="5af61-125">Définissez le type sur *JSON (application/json)*.</span><span class="sxs-lookup"><span data-stu-id="5af61-125">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="5af61-126">Entrez un corps de requête avec une tâche similaire au code JSON suivant :</span><span class="sxs-lookup"><span data-stu-id="5af61-126">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="5af61-127">Cliquez sur le bouton **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="5af61-127">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> <span data-ttu-id="5af61-128">Si vous ne recevez pas de réponse après avoir cliqué sur **Send** (Envoyer), désactivez l’option **SSL certification verification** (Vérification de la certification SSL).</span><span class="sxs-lookup"><span data-stu-id="5af61-128">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="5af61-129">Celle-ci se trouve sous **File** (Fichier) > **Settings** (Paramètres).</span><span class="sxs-lookup"><span data-stu-id="5af61-129">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="5af61-130">Cliquez à nouveau sur le bouton **Send** (Envoyer) une fois le paramètre désactivé.</span><span class="sxs-lookup"><span data-stu-id="5af61-130">Click the **Send** button again after disabling the setting.</span></span>
::: moniker-end

<span data-ttu-id="5af61-131">Cliquez sur l’onglet **Headers** (En-têtes) dans le volet **Response** (Réponse), puis copiez la valeur de l’en-tête **Location** (Emplacement) :</span><span class="sxs-lookup"><span data-stu-id="5af61-131">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Onglet Headers de la console Postman](../../tutorials/first-web-api/_static/pmc2.png)

<span data-ttu-id="5af61-133">L’URI d’en-tête d’emplacement permet d’accéder au nouvel élément.</span><span class="sxs-lookup"><span data-stu-id="5af61-133">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="5af61-134">Mise à jour</span><span class="sxs-lookup"><span data-stu-id="5af61-134">Update</span></span>

<span data-ttu-id="5af61-135">Ajoutez la méthode `Update` suivante :</span><span class="sxs-lookup"><span data-stu-id="5af61-135">Add the following `Update` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end

<span data-ttu-id="5af61-136">`Update` est similaire à `Create`, à la différence près qu’il utilise HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="5af61-136">`Update` is similar to `Create`, except it uses HTTP PUT.</span></span> <span data-ttu-id="5af61-137">La réponse est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="5af61-137">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="5af61-138">D’après la spécification HTTP, une requête PUT nécessite que le client envoie toute l’entité mise à jour, et pas seulement les différences.</span><span class="sxs-lookup"><span data-stu-id="5af61-138">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="5af61-139">Pour prendre en charge les mises à jour partielles, utilisez HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="5af61-139">To support partial updates, use HTTP PATCH.</span></span>

<span data-ttu-id="5af61-140">Utilisez Postman pour changer le nom de la tâche en « walk cat » :</span><span class="sxs-lookup"><span data-stu-id="5af61-140">Use Postman to update the to-do item's name to "walk cat":</span></span>

![Console Postman montrant la réponse 204 (Pas de contenu)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="5af61-142">Supprimer</span><span class="sxs-lookup"><span data-stu-id="5af61-142">Delete</span></span>

<span data-ttu-id="5af61-143">Ajoutez la méthode `Delete` suivante :</span><span class="sxs-lookup"><span data-stu-id="5af61-143">Add the following `Delete` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="5af61-144">La réponse `Delete` est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="5af61-144">The `Delete` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="5af61-145">Utilisez Postman pour supprimer la tâche :</span><span class="sxs-lookup"><span data-stu-id="5af61-145">Use Postman to delete the to-do item:</span></span>

![Console Postman montrant la réponse 204 (Pas de contenu)](../../tutorials/first-web-api/_static/pmd.png)
