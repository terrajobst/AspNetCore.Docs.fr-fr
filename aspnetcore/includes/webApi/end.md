## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="e4073-101">Implémenter les autres opérations CRUD</span><span class="sxs-lookup"><span data-stu-id="e4073-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="e4073-102">Dans les sections suivantes, les méthodes `Create`, `Update` et `Delete` sont ajoutées au contrôleur.</span><span class="sxs-lookup"><span data-stu-id="e4073-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="e4073-103">Créer</span><span class="sxs-lookup"><span data-stu-id="e4073-103">Create</span></span>

<span data-ttu-id="e4073-104">Ajoutez la méthode `Create` suivante :</span><span class="sxs-lookup"><span data-stu-id="e4073-104">Add the following `Create` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="e4073-105">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="e4073-105">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="e4073-106">Le code précédent est une méthode HTTP POST, comme indiqué par l’attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="e4073-106">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="e4073-107">L’attribut [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) indique à MVC qu’il faut obtenir la valeur de la tâche à partir du corps de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="e4073-107">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="e4073-108">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="e4073-108">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="e4073-109">Le code précédent est une méthode HTTP POST, comme indiqué par l’attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="e4073-109">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="e4073-110">MVC obtient la valeur de la tâche à partir du corps de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="e4073-110">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end

<span data-ttu-id="e4073-111">La méthode `CreatedAtRoute` :</span><span class="sxs-lookup"><span data-stu-id="e4073-111">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="e4073-112">Retourne une réponse 201.</span><span class="sxs-lookup"><span data-stu-id="e4073-112">Returns a 201 response.</span></span> <span data-ttu-id="e4073-113">HTTP 201 est la réponse standard d’une méthode HTTP POST qui crée une ressource sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="e4073-113">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="e4073-114">Ajoute un en-tête Location à la réponse.</span><span class="sxs-lookup"><span data-stu-id="e4073-114">Adds a Location header to the response.</span></span> <span data-ttu-id="e4073-115">L’en-tête Location spécifie l’URI de l’élément d’action qui vient d’être créé.</span><span class="sxs-lookup"><span data-stu-id="e4073-115">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="e4073-116">Consultez [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="e4073-116">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="e4073-117">Utilise l’itinéraire nommé « GetTodo » pour créer l’URL.</span><span class="sxs-lookup"><span data-stu-id="e4073-117">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="e4073-118">L’itinéraire nommé « GetTodo » est défini dans `GetById` :</span><span class="sxs-lookup"><span data-stu-id="e4073-118">The "GetTodo" named route is defined in `GetById`:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="e4073-119">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="e4073-119">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="e4073-120">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="e4073-120">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="e4073-121">Utiliser Postman pour envoyer une requête Create</span><span class="sxs-lookup"><span data-stu-id="e4073-121">Use Postman to send a Create request</span></span>

* <span data-ttu-id="e4073-122">Démarrez l'application.</span><span class="sxs-lookup"><span data-stu-id="e4073-122">Start the app.</span></span>
* <span data-ttu-id="e4073-123">Ouvrez Postman.</span><span class="sxs-lookup"><span data-stu-id="e4073-123">Open Postman.</span></span>

![Console Postman](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="e4073-125">Mettez à jour le numéro de port dans l’URL localhost.</span><span class="sxs-lookup"><span data-stu-id="e4073-125">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="e4073-126">Définissez la méthode HTTP avec la valeur *POST*.</span><span class="sxs-lookup"><span data-stu-id="e4073-126">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="e4073-127">Cliquez sur l’onglet **Body** (Corps).</span><span class="sxs-lookup"><span data-stu-id="e4073-127">Click the **Body** tab.</span></span>
* <span data-ttu-id="e4073-128">Sélectionnez la case d’option **raw** (données brutes).</span><span class="sxs-lookup"><span data-stu-id="e4073-128">Select the **raw** radio button.</span></span>
* <span data-ttu-id="e4073-129">Définissez le type avec la valeur *JSON (application/json)*.</span><span class="sxs-lookup"><span data-stu-id="e4073-129">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="e4073-130">Entrez un corps de requête avec une tâche similaire au code JSON suivant :</span><span class="sxs-lookup"><span data-stu-id="e4073-130">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="e4073-131">Cliquez sur le bouton **Send** (Envoyer).</span><span class="sxs-lookup"><span data-stu-id="e4073-131">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> <span data-ttu-id="e4073-132">Si vous ne recevez pas de réponse après avoir cliqué sur **Send** (Envoyer), désactivez l’option **SSL certification verification** (Vérification de la certification SSL).</span><span class="sxs-lookup"><span data-stu-id="e4073-132">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="e4073-133">Celle-ci se trouve sous **File** (Fichier) > **Settings** (Paramètres).</span><span class="sxs-lookup"><span data-stu-id="e4073-133">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="e4073-134">Cliquez à nouveau sur le bouton **Send** (Envoyer) une fois le paramètre désactivé.</span><span class="sxs-lookup"><span data-stu-id="e4073-134">Click the **Send** button again after disabling the setting.</span></span>
::: moniker-end

<span data-ttu-id="e4073-135">Cliquez sur l’onglet **Headers** (En-têtes) dans le volet **Response** (Réponse), puis copiez la valeur de l’en-tête **Location** (Emplacement) :</span><span class="sxs-lookup"><span data-stu-id="e4073-135">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Onglet Headers de la console Postman](../../tutorials/first-web-api/_static/pmc2.png)

<span data-ttu-id="e4073-137">L’URI d’en-tête d’emplacement permet d’accéder au nouvel élément.</span><span class="sxs-lookup"><span data-stu-id="e4073-137">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="e4073-138">Mise à jour</span><span class="sxs-lookup"><span data-stu-id="e4073-138">Update</span></span>

<span data-ttu-id="e4073-139">Ajoutez la méthode `Update` suivante :</span><span class="sxs-lookup"><span data-stu-id="e4073-139">Add the following `Update` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="e4073-140">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="e4073-140">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="e4073-141">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="e4073-141">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end

<span data-ttu-id="e4073-142">`Update` est similaire à `Create`, à la différence près qu’il utilise HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="e4073-142">`Update` is similar to `Create`, except it uses HTTP PUT.</span></span> <span data-ttu-id="e4073-143">La réponse est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="e4073-143">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="e4073-144">D’après la spécification HTTP, une requête PUT nécessite que le client envoie toute l’entité mise à jour, et pas seulement les différences.</span><span class="sxs-lookup"><span data-stu-id="e4073-144">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="e4073-145">Pour prendre en charge les mises à jour partielles, utilisez HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="e4073-145">To support partial updates, use HTTP PATCH.</span></span>

<span data-ttu-id="e4073-146">Utilisez Postman pour changer le nom de la tâche en « walk cat » :</span><span class="sxs-lookup"><span data-stu-id="e4073-146">Use Postman to update the to-do item's name to "walk cat":</span></span>

![Console Postman montrant la réponse 204 (Pas de contenu)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="e4073-148">Supprimer</span><span class="sxs-lookup"><span data-stu-id="e4073-148">Delete</span></span>

<span data-ttu-id="e4073-149">Ajoutez la méthode `Delete` suivante :</span><span class="sxs-lookup"><span data-stu-id="e4073-149">Add the following `Delete` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="e4073-150">La réponse `Delete` est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="e4073-150">The `Delete` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="e4073-151">Utilisez Postman pour supprimer la tâche :</span><span class="sxs-lookup"><span data-stu-id="e4073-151">Use Postman to delete the to-do item:</span></span>

![Console Postman montrant la réponse 204 (Pas de contenu)](../../tutorials/first-web-api/_static/pmd.png)
