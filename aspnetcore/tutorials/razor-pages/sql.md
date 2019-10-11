---
title: Utiliser une base de données et ASP.NET Core
author: rick-anderson
description: Explique l’utilisation d’une base de données et d’ASP.NET Core.
ms.author: riande
ms.date: 7/22/2019
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 87d27b60940826e21b060f2e07d344b30ff75b27
ms.sourcegitcommit: 7d3c6565dda6241eb13f9a8e1e1fd89b1cfe4d18
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72259784"
---
# <a name="work-with-a-database-and-aspnet-core"></a><span data-ttu-id="b7b15-103">Utiliser une base de données et ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b7b15-103">Work with a database and ASP.NET Core</span></span>

<span data-ttu-id="b7b15-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="b7b15-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="b7b15-105">L’objet `RazorPagesMovieContext` gère la tâche de connexion à la base de données et de mappage d’objets `Movie` à des enregistrements de la base de données.</span><span class="sxs-lookup"><span data-stu-id="b7b15-105">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="b7b15-106">Le contexte de base de données est inscrit auprès du conteneur [Injection de dépendances](xref:fundamentals/dependency-injection) dans la méthode `ConfigureServices` de *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="b7b15-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b7b15-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b7b15-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b7b15-108">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="b7b15-108">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---

<span data-ttu-id="b7b15-109">Le système de [configuration](xref:fundamentals/configuration/index) d’ASP.NET Core lit `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="b7b15-109">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="b7b15-110">Pour un développement local, il obtient la chaîne de connexion à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="b7b15-110">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b7b15-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b7b15-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b7b15-112">La valeur du nom de la base de données (`Database={Database name}`) est différent pour votre code généré.</span><span class="sxs-lookup"><span data-stu-id="b7b15-112">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="b7b15-113">La valeur du nom est arbitraire.</span><span class="sxs-lookup"><span data-stu-id="b7b15-113">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie30/appsettings.json)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b7b15-114">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="b7b15-114">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---

<span data-ttu-id="b7b15-115">Quand l’application est déployée sur un serveur de test ou de production, une variable d’environnement peut être utilisée pour définir la chaîne de connexion à un serveur de base de données réel.</span><span class="sxs-lookup"><span data-stu-id="b7b15-115">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="b7b15-116">Pour plus d’informations, consultez [Configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="b7b15-116">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b7b15-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b7b15-117">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="b7b15-118">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="b7b15-118">SQL Server Express LocalDB</span></span>

<span data-ttu-id="b7b15-119">LocalDB est une version allégée du moteur de base de données SQL Server Express, qui est ciblée pour le développement de programmes.</span><span class="sxs-lookup"><span data-stu-id="b7b15-119">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="b7b15-120">LocalDB démarre à la demande et s’exécute en mode utilisateur, ce qui n’implique aucune configuration complexe.</span><span class="sxs-lookup"><span data-stu-id="b7b15-120">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="b7b15-121">Par défaut, la base de données LocalDB crée des fichiers `*.mdf` dans le répertoire `C:/Users/<user/>`.</span><span class="sxs-lookup"><span data-stu-id="b7b15-121">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="b7b15-122">Dans le menu **Affichage**, ouvrez **l’Explorateur d’objets SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="b7b15-122">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu View](sql/_static/ssox.png)

* <span data-ttu-id="b7b15-124">Cliquez avec le bouton droit sur la table `Movie` et sélectionnez **Concepteur de vues** :</span><span class="sxs-lookup"><span data-stu-id="b7b15-124">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Les menus contextuels s’ouvrent dans la table Movie](sql/_static/design.png)

  ![Les tables Movie s’ouvrent dans le concepteur](sql/_static/dv.png)

<span data-ttu-id="b7b15-127">Notez l’icône de clé en regard de `ID`.</span><span class="sxs-lookup"><span data-stu-id="b7b15-127">Note the key icon next to `ID`.</span></span> <span data-ttu-id="b7b15-128">Par défaut, EF crée une propriété nommée `ID` pour la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="b7b15-128">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="b7b15-129">Cliquez avec le bouton droit sur la table `Movie` et sélectionnez **Afficher les données** :</span><span class="sxs-lookup"><span data-stu-id="b7b15-129">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Table Movie ouverte, affichant des données de table](sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b7b15-131">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="b7b15-131">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---

## <a name="seed-the-database"></a><span data-ttu-id="b7b15-132">Amorcer la base de données</span><span class="sxs-lookup"><span data-stu-id="b7b15-132">Seed the database</span></span>

<span data-ttu-id="b7b15-133">Créez une classe nommée `SeedData` dans le dossier *Modèles* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b7b15-133">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="b7b15-134">Si la base de données contient des films, l’initialiseur de valeur initiale retourne une valeur et aucun film n’est ajouté.</span><span class="sxs-lookup"><span data-stu-id="b7b15-134">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="b7b15-135">Ajouter l’initialiseur de valeur initiale</span><span class="sxs-lookup"><span data-stu-id="b7b15-135">Add the seed initializer</span></span>

<span data-ttu-id="b7b15-136">Dans *Program.cs*, modifiez la méthode `Main` pour effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="b7b15-136">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="b7b15-137">Obtenir une instance de contexte de base de données à partir du conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="b7b15-137">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="b7b15-138">Appeler la méthode seed et la passer au contexte.</span><span class="sxs-lookup"><span data-stu-id="b7b15-138">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="b7b15-139">Supprimer le contexte une fois la méthode seed terminée.</span><span class="sxs-lookup"><span data-stu-id="b7b15-139">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="b7b15-140">Le code suivant montre le fichier *Program.cs* mis à jour.</span><span class="sxs-lookup"><span data-stu-id="b7b15-140">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Program.cs)]

<span data-ttu-id="b7b15-141">L’exception suivante se produit lorsque `Update-Database` n’a pas été exécutée :</span><span class="sxs-lookup"><span data-stu-id="b7b15-141">The following exception occurs when `Update-Database` has not been run:</span></span>

`SqlException: Cannot open database "RazorPagesMovieContext-" requested by the login. The login failed.`
`Login failed for user 'user name'.`

### <a name="test-the-app"></a><span data-ttu-id="b7b15-142">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="b7b15-142">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b7b15-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b7b15-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b7b15-144">Supprimez tous les enregistrements de la base de données.</span><span class="sxs-lookup"><span data-stu-id="b7b15-144">Delete all the records in the DB.</span></span> <span data-ttu-id="b7b15-145">Pour ce faire, utilisez les liens de suppression disponibles dans le navigateur ou à partir de [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="b7b15-145">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="b7b15-146">Forcez l’application à s’initialiser (appelez les méthodes de la classe `Startup`) pour que la méthode seed s’exécute.</span><span class="sxs-lookup"><span data-stu-id="b7b15-146">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="b7b15-147">Pour forcer l’initialisation, IIS Express doit être arrêté et redémarré.</span><span class="sxs-lookup"><span data-stu-id="b7b15-147">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="b7b15-148">Pour cela, adoptez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="b7b15-148">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="b7b15-149">Cliquez avec le bouton droit sur l’icône de barre d’état système IIS Express dans la zone de notification, puis appuyez sur **Quitter** ou sur **Arrêter le site** :</span><span class="sxs-lookup"><span data-stu-id="b7b15-149">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Icône de la barre d’état système IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu contextuel](sql/_static/stopIIS.png)

    * <span data-ttu-id="b7b15-152">Si vous exécutiez Visual Studio en mode de non-débogage, appuyez sur F5 pour l’exécuter en mode de débogage.</span><span class="sxs-lookup"><span data-stu-id="b7b15-152">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="b7b15-153">Si vous exécutiez Visual Studio en mode de débogage, arrêtez le débogueur et appuyez sur F5.</span><span class="sxs-lookup"><span data-stu-id="b7b15-153">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b7b15-154">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="b7b15-154">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="b7b15-155">Supprimez tous les enregistrements dans la base de données (pour que la méthode seed s’exécute).</span><span class="sxs-lookup"><span data-stu-id="b7b15-155">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="b7b15-156">Arrêtez et démarrez l’application pour amorcer la base de données.</span><span class="sxs-lookup"><span data-stu-id="b7b15-156">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="b7b15-157">L’application affiche les données de départ.</span><span class="sxs-lookup"><span data-stu-id="b7b15-157">The app shows the seeded data.</span></span>

---

<span data-ttu-id="b7b15-158">Le tutoriel suivant sert à améliorer la présentation des données.</span><span class="sxs-lookup"><span data-stu-id="b7b15-158">The next tutorial will improve the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b7b15-159">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="b7b15-159">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b7b15-160">[Précédent : Pages Razor obtenues par génération de modèles automatique](xref:tutorials/razor-pages/page)
> [Suivant : Mise à jour des pages](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="b7b15-160">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="b7b15-161">L’objet `RazorPagesMovieContext` gère la tâche de connexion à la base de données et de mappage d’objets `Movie` à des enregistrements de la base de données.</span><span class="sxs-lookup"><span data-stu-id="b7b15-161">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="b7b15-162">Le contexte de base de données est inscrit auprès du conteneur [Injection de dépendances](xref:fundamentals/dependency-injection) dans la méthode `ConfigureServices` de *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="b7b15-162">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b7b15-163">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b7b15-163">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b7b15-164">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="b7b15-164">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---

<span data-ttu-id="b7b15-165">Pour plus d’informations sur les méthodes utilisées dans `ConfigureServices`, consultez :</span><span class="sxs-lookup"><span data-stu-id="b7b15-165">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="b7b15-166">[Prise en charge du règlement général sur la protection des données (RGPD) de l’Union Européenne dans ASP.NET Core](xref:security/gdpr) pour `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="b7b15-166">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="b7b15-167">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="b7b15-167">SetCompatibilityVersion</span></span>](xref:mvc/compatibility-version)

<span data-ttu-id="b7b15-168">Le système de [configuration](xref:fundamentals/configuration/index) d’ASP.NET Core lit `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="b7b15-168">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="b7b15-169">Pour un développement local, il obtient la chaîne de connexion à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="b7b15-169">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b7b15-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b7b15-170">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b7b15-171">La valeur du nom de la base de données (`Database={Database name}`) est différent pour votre code généré.</span><span class="sxs-lookup"><span data-stu-id="b7b15-171">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="b7b15-172">La valeur du nom est arbitraire.</span><span class="sxs-lookup"><span data-stu-id="b7b15-172">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie22/appsettings.json)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b7b15-173">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b7b15-173">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b7b15-174">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="b7b15-174">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---

<span data-ttu-id="b7b15-175">Quand l’application est déployée sur un serveur de test ou de production, une variable d’environnement peut être utilisée pour définir la chaîne de connexion à un serveur de base de données réel.</span><span class="sxs-lookup"><span data-stu-id="b7b15-175">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="b7b15-176">Pour plus d’informations, consultez [Configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="b7b15-176">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b7b15-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b7b15-177">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="b7b15-178">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="b7b15-178">SQL Server Express LocalDB</span></span>

<span data-ttu-id="b7b15-179">LocalDB est une version allégée du moteur de base de données SQL Server Express, qui est ciblée pour le développement de programmes.</span><span class="sxs-lookup"><span data-stu-id="b7b15-179">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="b7b15-180">LocalDB démarre à la demande et s’exécute en mode utilisateur, ce qui n’implique aucune configuration complexe.</span><span class="sxs-lookup"><span data-stu-id="b7b15-180">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="b7b15-181">Par défaut, la base de données LocalDB crée des fichiers `*.mdf` dans le répertoire `C:/Users/<user/>`.</span><span class="sxs-lookup"><span data-stu-id="b7b15-181">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="b7b15-182">Dans le menu **Affichage**, ouvrez **l’Explorateur d’objets SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="b7b15-182">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu View](sql/_static/ssox.png)

* <span data-ttu-id="b7b15-184">Cliquez avec le bouton droit sur la table `Movie` et sélectionnez **Concepteur de vues** :</span><span class="sxs-lookup"><span data-stu-id="b7b15-184">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Menu contextuel ouvert sur la table Movie](sql/_static/design.png)

  ![Table Movie ouverte dans le Concepteur](sql/_static/dv.png)

<span data-ttu-id="b7b15-187">Notez l’icône de clé en regard de `ID`.</span><span class="sxs-lookup"><span data-stu-id="b7b15-187">Note the key icon next to `ID`.</span></span> <span data-ttu-id="b7b15-188">Par défaut, EF crée une propriété nommée `ID` pour la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="b7b15-188">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="b7b15-189">Cliquez avec le bouton droit sur la table `Movie` et sélectionnez **Afficher les données** :</span><span class="sxs-lookup"><span data-stu-id="b7b15-189">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Table Movie ouverte, affichant des données de table](sql/_static/vd22.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b7b15-191">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b7b15-191">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b7b15-192">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="b7b15-192">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---

## <a name="seed-the-database"></a><span data-ttu-id="b7b15-193">Amorcer la base de données</span><span class="sxs-lookup"><span data-stu-id="b7b15-193">Seed the database</span></span>

<span data-ttu-id="b7b15-194">Créez une classe nommée `SeedData` dans le dossier *Modèles* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b7b15-194">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="b7b15-195">Si la base de données contient des films, l’initialiseur de valeur initiale retourne une valeur et aucun film n’est ajouté.</span><span class="sxs-lookup"><span data-stu-id="b7b15-195">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="b7b15-196">Ajouter l’initialiseur de valeur initiale</span><span class="sxs-lookup"><span data-stu-id="b7b15-196">Add the seed initializer</span></span>

<span data-ttu-id="b7b15-197">Dans *Program.cs*, modifiez la méthode `Main` pour effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="b7b15-197">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="b7b15-198">Obtenir une instance de contexte de base de données à partir du conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="b7b15-198">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="b7b15-199">Appeler la méthode seed et la passer au contexte.</span><span class="sxs-lookup"><span data-stu-id="b7b15-199">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="b7b15-200">Supprimer le contexte une fois la méthode seed terminée.</span><span class="sxs-lookup"><span data-stu-id="b7b15-200">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="b7b15-201">Le code suivant montre le fichier *Program.cs* mis à jour.</span><span class="sxs-lookup"><span data-stu-id="b7b15-201">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Program.cs)]

<span data-ttu-id="b7b15-202">Une application de production n’appelle pas `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="b7b15-202">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="b7b15-203">Il est ajouté au code précédent afin d’éviter l’exception suivante quand `Update-Database` n’a pas été exécutée :</span><span class="sxs-lookup"><span data-stu-id="b7b15-203">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="b7b15-204">SqlException : Impossible d’ouvrir la base de données 'RazorPagesMovieContext-21' demandée par la connexion.</span><span class="sxs-lookup"><span data-stu-id="b7b15-204">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="b7b15-205">La connexion a échoué.</span><span class="sxs-lookup"><span data-stu-id="b7b15-205">The login failed.</span></span>
<span data-ttu-id="b7b15-206">Échec de la connexion de l’utilisateur 'nom utilisateur'.</span><span class="sxs-lookup"><span data-stu-id="b7b15-206">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="b7b15-207">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="b7b15-207">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b7b15-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b7b15-208">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b7b15-209">Supprimez tous les enregistrements de la base de données.</span><span class="sxs-lookup"><span data-stu-id="b7b15-209">Delete all the records in the DB.</span></span> <span data-ttu-id="b7b15-210">Pour ce faire, utilisez les liens de suppression disponibles dans le navigateur ou à partir de [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="b7b15-210">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="b7b15-211">Forcez l’application à s’initialiser (appelez les méthodes de la classe `Startup`) pour que la méthode seed s’exécute.</span><span class="sxs-lookup"><span data-stu-id="b7b15-211">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="b7b15-212">Pour forcer l’initialisation, IIS Express doit être arrêté et redémarré.</span><span class="sxs-lookup"><span data-stu-id="b7b15-212">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="b7b15-213">Pour cela, adoptez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="b7b15-213">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="b7b15-214">Cliquez avec le bouton droit sur l’icône de barre d’état système IIS Express dans la zone de notification, puis appuyez sur **Quitter** ou sur **Arrêter le site** :</span><span class="sxs-lookup"><span data-stu-id="b7b15-214">Right-click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Icône de la barre d’état système IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu contextuel](sql/_static/stopIIS.png)

    * <span data-ttu-id="b7b15-217">Si vous exécutiez Visual Studio en mode de non-débogage, appuyez sur F5 pour l’exécuter en mode de débogage.</span><span class="sxs-lookup"><span data-stu-id="b7b15-217">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="b7b15-218">Si vous exécutiez Visual Studio en mode de débogage, arrêtez le débogueur et appuyez sur F5.</span><span class="sxs-lookup"><span data-stu-id="b7b15-218">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b7b15-219">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b7b15-219">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="b7b15-220">Supprimez tous les enregistrements dans la base de données (pour que la méthode seed s’exécute).</span><span class="sxs-lookup"><span data-stu-id="b7b15-220">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="b7b15-221">Arrêtez et démarrez l’application pour amorcer la base de données.</span><span class="sxs-lookup"><span data-stu-id="b7b15-221">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="b7b15-222">L’application affiche les données de départ.</span><span class="sxs-lookup"><span data-stu-id="b7b15-222">The app shows the seeded data.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b7b15-223">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="b7b15-223">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="b7b15-224">Supprimez tous les enregistrements dans la base de données (pour que la méthode seed s’exécute).</span><span class="sxs-lookup"><span data-stu-id="b7b15-224">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="b7b15-225">Arrêtez et démarrez l’application pour amorcer la base de données.</span><span class="sxs-lookup"><span data-stu-id="b7b15-225">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="b7b15-226">L’application affiche les données de départ.</span><span class="sxs-lookup"><span data-stu-id="b7b15-226">The app shows the seeded data.</span></span>

---

<span data-ttu-id="b7b15-227">L’application affiche les données de départ :</span><span class="sxs-lookup"><span data-stu-id="b7b15-227">The app shows the seeded data:</span></span>

![Application Movie ouverte dans Chrome, affichant les données relatives aux films](sql/_static/m55.png)

<span data-ttu-id="b7b15-229">Le didacticiel suivant nettoie la présentation des données.</span><span class="sxs-lookup"><span data-stu-id="b7b15-229">The next tutorial will clean up the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b7b15-230">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="b7b15-230">Additional resources</span></span>

* [<span data-ttu-id="b7b15-231">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="b7b15-231">YouTube version of this tutorial</span></span>](https://youtu.be/A_5ff11sDHY)

> [!div class="step-by-step"]
> <span data-ttu-id="b7b15-232">[Précédent : Pages Razor obtenues par génération de modèles automatique](xref:tutorials/razor-pages/page)
> [Suivant : Mise à jour des pages](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="b7b15-232">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>

::: moniker-end
