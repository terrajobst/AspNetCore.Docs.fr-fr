## <a name="implement-the-other-crud-operations"></a>Implémenter les autres opérations CRUD

Dans les sections suivantes, les méthodes `Create`, `Update` et `Delete` sont ajoutées au contrôleur.

### <a name="create"></a>Créer

Ajoutez la méthode `Create` suivante :

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Le code précédent est une méthode HTTP POST, comme indiqué par l’attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute). L’attribut [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) indique à MVC qu’il faut obtenir la valeur de la tâche à partir du corps de la requête HTTP.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Le code précédent est une méthode HTTP POST, comme indiqué par l’attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute). MVC obtient la valeur de la tâche à partir du corps de la requête HTTP.
::: moniker-end

La méthode `CreatedAtRoute` :

* Retourne une réponse 201. HTTP 201 est la réponse standard d’une méthode HTTP POST qui crée une ressource sur le serveur.
* Ajoute un en-tête Location à la réponse. L’en-tête Location spécifie l’URI de l’élément d’action qui vient d’être créé. Consultez [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* Utilise l’itinéraire nommé « GetTodo » pour créer l’URL. L’itinéraire nommé « GetTodo » est défini dans `GetById` :

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a>Utiliser Postman pour envoyer une requête Create

* Démarrez l'application.
* Ouvrez Postman.

![Console Postman](../../tutorials/first-web-api/_static/pmc.png)

* Mettez à jour le numéro de port dans l’URL localhost.
* Définissez la méthode HTTP avec la valeur *POST*.
* Cliquez sur l’onglet **Body** (Corps).
* Sélectionnez la case d’option **raw** (données brutes).
* Définissez le type avec la valeur *JSON (application/json)*.
* Entrez un corps de requête avec une tâche similaire au code JSON suivant :

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* Cliquez sur le bouton **Send** (Envoyer).

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> Si vous ne recevez pas de réponse après avoir cliqué sur **Send** (Envoyer), désactivez l’option **SSL certification verification** (Vérification de la certification SSL). Celle-ci se trouve sous **File** (Fichier) > **Settings** (Paramètres). Cliquez à nouveau sur le bouton **Send** (Envoyer) une fois le paramètre désactivé.
::: moniker-end

Cliquez sur l’onglet **Headers** (En-têtes) dans le volet **Response** (Réponse), puis copiez la valeur de l’en-tête **Location** (Emplacement) :

![Onglet Headers de la console Postman](../../tutorials/first-web-api/_static/pmc2.png)

L’URI d’en-tête d’emplacement permet d’accéder au nouvel élément.

### <a name="update"></a>Mise à jour

Ajoutez la méthode `Update` suivante :

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end

`Update` est similaire à `Create`, à la différence près qu’il utilise HTTP PUT. La réponse est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). D’après la spécification HTTP, une requête PUT nécessite que le client envoie toute l’entité mise à jour, et pas seulement les différences. Pour prendre en charge les mises à jour partielles, utilisez HTTP PATCH.

Utilisez Postman pour changer le nom de la tâche en « walk cat » :

![Console Postman montrant la réponse 204 (Pas de contenu)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Supprimer

Ajoutez la méthode `Delete` suivante :

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

La réponse `Delete` est [204 (Pas de contenu)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

Utilisez Postman pour supprimer la tâche :

![Console Postman montrant la réponse 204 (Pas de contenu)](../../tutorials/first-web-api/_static/pmd.png)
