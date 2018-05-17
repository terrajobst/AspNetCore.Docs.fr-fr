## <a name="register-the-database-context"></a>Inscrire le contexte de base de données

À cette étape, le contexte de base de données est inscrit auprès du conteneur [Injection de dépendances](xref:fundamentals/dependency-injection). Les services (comme le contexte de base de données) qui sont inscrits auprès du conteneur d’injection de dépendances sont accessibles aux contrôleurs.

Inscrivez le contexte de base de données auprès du conteneur de service à l’aide de la prise en charge intégrée de [l’injection de dépendances](xref:fundamentals/dependency-injection). Remplacez le contenu du fichier *Startup.cs* par le code suivant :

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]
::: moniker-end

Le code précédent :

* Supprime le code non utilisé.
* Spécifie qu’une base de données en mémoire est injectée dans le conteneur de service.