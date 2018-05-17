## <a name="register-the-database-context"></a><span data-ttu-id="b5d99-101">Inscrire le contexte de base de données</span><span class="sxs-lookup"><span data-stu-id="b5d99-101">Register the database context</span></span>

<span data-ttu-id="b5d99-102">À cette étape, le contexte de base de données est inscrit auprès du conteneur [Injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b5d99-102">In this step, the database context is registered with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="b5d99-103">Les services (comme le contexte de base de données) qui sont inscrits auprès du conteneur d’injection de dépendances sont accessibles aux contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="b5d99-103">Services (such as the DB context) that are registered with the dependency injection (DI) container are available to the controllers.</span></span>

<span data-ttu-id="b5d99-104">Inscrivez le contexte de base de données auprès du conteneur de service à l’aide de la prise en charge intégrée de [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b5d99-104">Register the DB context with the service container using the built-in support for [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="b5d99-105">Remplacez le contenu du fichier *Startup.cs* par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b5d99-105">Replace the contents of the *Startup.cs* file with the following code:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="b5d99-106">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]</span><span class="sxs-lookup"><span data-stu-id="b5d99-106">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="b5d99-107">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]</span><span class="sxs-lookup"><span data-stu-id="b5d99-107">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]</span></span>
::: moniker-end

<span data-ttu-id="b5d99-108">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="b5d99-108">The preceding code:</span></span>

* <span data-ttu-id="b5d99-109">Supprime le code non utilisé.</span><span class="sxs-lookup"><span data-stu-id="b5d99-109">Removes the unused code.</span></span>
* <span data-ttu-id="b5d99-110">Spécifie qu’une base de données en mémoire est injectée dans le conteneur de service.</span><span class="sxs-lookup"><span data-stu-id="b5d99-110">Specifies an in-memory database is injected into the service container.</span></span>