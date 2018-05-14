## <a name="call-the-web-api-with-jquery"></a>Appeler l’API web avec jQuery

Dans cette section, une page HTML qui utilise jQuery pour appeler l’API web est ajoutée. jQuery lance la requête et met à jour la page avec les détails de la réponse de l’API.

Configurez le projet pour traiter les fichiers statiques et activer le mappage de fichier par défaut. Pour cela, appelez les méthodes d’extension [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) et [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) dans *Startup.Configure*. Pour plus d’informations, consultez [Utiliser des fichiers statiques dans ASP.NET Core](xref:fundamentals/static-files).

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup2.cs?name=snippet_Configure&highlight=3-4)]

Ajoutez un fichier HTML, nommé *index.html*, au répertoire *wwwroot* du projet. Remplacez son contenu par le balisage suivant :

[!code-html[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/index.html)]

Ajoutez un fichier JavaScript, nommé *site.js*, au répertoire *wwwroot* du projet. Remplacez son contenu par le code suivant :

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

Vous devrez peut-être changer les paramètres de lancement du projet ASP.NET Core pour tester la page HTML localement. Ouvrez *launchSettings.json* dans le répertoire *Propriétés* du projet. Supprimez la propriété `launchUrl` pour forcer l’utilisation du fichier *index.html* (fichier par défaut du projet) à l’ouverture de l’application.

Il existe plusieurs façons d’obtenir jQuery. Dans l’extrait précédent, la bibliothèque est chargée à partir d’un CDN. Cet exemple illustre une procédure CRUD complète qui appelle l’API avec jQuery. Cet exemple comprend des fonctionnalités supplémentaires qui permettent d’améliorer l’expérience. Les explications ci-dessous traitent des appels à l’API.

### <a name="get-a-list-of-to-do-items"></a>Obtenir une liste de tâches

Pour obtenir une liste de tâches, envoyez une requête HTTP GET à */api/todo*.

La fonction JQuery [ajax](https://api.jquery.com/jquery.ajax/) envoie une requête AJAX à l’API, qui retourne du code JSON représentant un objet ou un tableau. Cette fonction peut gérer toutes les formes de l’interaction HTTP en envoyant une requête HTTP à l’`url` spécifiée. `GET` est utilisé comme `type`. La fonction de rappel `success` est appelée si la requête réussit. Dans le rappel, le DOM est mis à jour avec les informations des tâches.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a>Ajouter une tâche

Pour ajouter une tâche, envoyez une requête HTTP POST à */api/todo/*. Le corps de la requête doit contenir un objet de tâche. La fonction [ajax](https://api.jquery.com/jquery.ajax/) utilise `POST` pour appeler l’API. Pour les requêtes `POST` et `PUT`, le corps de requête représente les données envoyées à l’API. L’API attend un corps de requête JSON. Les options `accepts` et `contentType` sont définies avec la valeur `application/json` pour classer le type de média qui est reçu et envoyé (respectivement). Les données sont converties en objet JSON avec [`JSON.stringify`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). Quand l’API retourne un code d’état de réussite, la fonction `getData` est appelée pour mettre à jour la table HTML.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a>Mettre à jour une tâche

Les opérations de mise à jour et d’ajout d’une tâche s’appuient toutes les deux sur un corps de requête et sont donc très similaires. Voici toutefois ce qui les distingue dans ce cas : l’`url` change pour ajouter l’identificateur unique de l’élément, et le `type` est `PUT`.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a>Supprimer une tâche

Pour supprimer une tâche, vous devez définir le `type` sur l’appel AJAX avec la valeur `DELETE` et spécifier l’identificateur unique de l’élément dans l’URL.

[!code-javascript[](../../tutorials/first-web-api/samples/2.0/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]
