---
title: Utiliser une base de données et ASP.NET Core
author: rick-anderson
description: Explique l’utilisation d’une base de données et d’ASP.NET Core.
ms.author: riande
ms.date: 7/22/2019
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 197697f28e9faa45c1ac2b7f993bde15994957e5
ms.sourcegitcommit: 051f068c78931432e030b60094c38376d64d013e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68440392"
---
# <a name="work-with-a-database-and-aspnet-core"></a><span data-ttu-id="bc14a-103">Utiliser une base de données et ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bc14a-103">Work with a database and ASP.NET Core</span></span>

<span data-ttu-id="bc14a-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="bc14a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="bc14a-105">L’objet `RazorPagesMovieContext` gère la tâche de connexion à la base de données et de mappage d’objets `Movie` à des enregistrements de la base de données.</span><span class="sxs-lookup"><span data-stu-id="bc14a-105">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="bc14a-106">Le contexte de base de données est inscrit auprès du conteneur [Injection de dépendances](xref:fundamentals/dependency-injection) dans la méthode `ConfigureServices` de *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="bc14a-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bc14a-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bc14a-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="bc14a-108">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="bc14a-108">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---

<span data-ttu-id="bc14a-109">Le système de [configuration](xref:fundamentals/configuration/index) d’ASP.NET Core lit `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="bc14a-109">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="bc14a-110">Pour un développement local, il obtient la chaîne de connexion à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="bc14a-110">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bc14a-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bc14a-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bc14a-112">La valeur du nom de la base de données (`Database={Database name}`) est différent pour votre code généré.</span><span class="sxs-lookup"><span data-stu-id="bc14a-112">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="bc14a-113">La valeur du nom est arbitraire.</span><span class="sxs-lookup"><span data-stu-id="bc14a-113">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie30/appsettings.json)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="bc14a-114">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="bc14a-114">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---

<span data-ttu-id="bc14a-115">Quand l’application est déployée sur un serveur de test ou de production, une variable d’environnement peut être utilisée pour définir la chaîne de connexion à un serveur de base de données réel.</span><span class="sxs-lookup"><span data-stu-id="bc14a-115">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="bc14a-116">Pour plus d’informations, consultez [Configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="bc14a-116">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bc14a-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bc14a-117">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="bc14a-118">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="bc14a-118">SQL Server Express LocalDB</span></span>

<span data-ttu-id="bc14a-119">LocalDB est une version allégée du moteur de base de données SQL Server Express, qui est ciblée pour le développement de programmes.</span><span class="sxs-lookup"><span data-stu-id="bc14a-119">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="bc14a-120">LocalDB démarre à la demande et s’exécute en mode utilisateur, ce qui n’implique aucune configuration complexe.</span><span class="sxs-lookup"><span data-stu-id="bc14a-120">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="bc14a-121">Par défaut, la base de données LocalDB crée des fichiers `*.mdf` dans le répertoire `C:/Users/<user/>`.</span><span class="sxs-lookup"><span data-stu-id="bc14a-121">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="bc14a-122">Dans le menu **Affichage**, ouvrez **l’Explorateur d’objets SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="bc14a-122">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu View](sql/_static/ssox.png)

* <span data-ttu-id="bc14a-124">Cliquez avec le bouton droit sur la table `Movie` et sélectionnez **Concepteur de vues** :</span><span class="sxs-lookup"><span data-stu-id="bc14a-124">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Les menus contextuels s’ouvrent dans la table Movie](sql/_static/design.png)

  ![Les tables Movie s’ouvrent dans le concepteur](sql/_static/dv.png)

<span data-ttu-id="bc14a-127">Notez l’icône de clé en regard de `ID`.</span><span class="sxs-lookup"><span data-stu-id="bc14a-127">Note the key icon next to `ID`.</span></span> <span data-ttu-id="bc14a-128">Par défaut, EF crée une propriété nommée `ID` pour la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="bc14a-128">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="bc14a-129">Cliquez avec le bouton droit sur la table `Movie` et sélectionnez **Afficher les données** :</span><span class="sxs-lookup"><span data-stu-id="bc14a-129">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Table Movie ouverte, affichant des données de table](sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="bc14a-131">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="bc14a-131">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---

## <a name="seed-the-database"></a><span data-ttu-id="bc14a-132">Amorcer la base de données</span><span class="sxs-lookup"><span data-stu-id="bc14a-132">Seed the database</span></span>

<span data-ttu-id="bc14a-133">Créez une classe nommée `SeedData` dans le dossier *Modèles* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="bc14a-133">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="bc14a-134">Si la base de données contient des films, l’initialiseur de valeur initiale retourne une valeur et aucun film n’est ajouté.</span><span class="sxs-lookup"><span data-stu-id="bc14a-134">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="bc14a-135">Ajouter l’initialiseur de valeur initiale</span><span class="sxs-lookup"><span data-stu-id="bc14a-135">Add the seed initializer</span></span>

<span data-ttu-id="bc14a-136">Dans *Program.cs*, modifiez la méthode `Main` pour effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="bc14a-136">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="bc14a-137">Obtenir une instance de contexte de base de données à partir du conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="bc14a-137">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="bc14a-138">Appeler la méthode seed et la passer au contexte.</span><span class="sxs-lookup"><span data-stu-id="bc14a-138">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="bc14a-139">Supprimer le contexte une fois la méthode seed terminée.</span><span class="sxs-lookup"><span data-stu-id="bc14a-139">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="bc14a-140">Le code suivant montre le fichier *Program.cs* mis à jour.</span><span class="sxs-lookup"><span data-stu-id="bc14a-140">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Program.cs)]

<span data-ttu-id="bc14a-141">Une application de production n’appelle pas `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="bc14a-141">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="bc14a-142">Il est ajouté au code précédent afin d’éviter l’exception suivante quand `Update-Database` n’a pas été exécutée :</span><span class="sxs-lookup"><span data-stu-id="bc14a-142">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="bc14a-143">SqlException : Impossible d’ouvrir la base de données 'RazorPagesMovieContext-21' demandée par la connexion.</span><span class="sxs-lookup"><span data-stu-id="bc14a-143">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="bc14a-144">La connexion a échoué.</span><span class="sxs-lookup"><span data-stu-id="bc14a-144">The login failed.</span></span>
<span data-ttu-id="bc14a-145">Échec de la connexion de l’utilisateur 'nom utilisateur'.</span><span class="sxs-lookup"><span data-stu-id="bc14a-145">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="bc14a-146">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="bc14a-146">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bc14a-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bc14a-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bc14a-148">Supprimez tous les enregistrements de la base de données.</span><span class="sxs-lookup"><span data-stu-id="bc14a-148">Delete all the records in the DB.</span></span> <span data-ttu-id="bc14a-149">Pour ce faire, utilisez les liens de suppression disponibles dans le navigateur ou à partir de [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="bc14a-149">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="bc14a-150">Forcez l’application à s’initialiser (appelez les méthodes de la classe `Startup`) pour que la méthode seed s’exécute.</span><span class="sxs-lookup"><span data-stu-id="bc14a-150">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="bc14a-151">Pour forcer l’initialisation, IIS Express doit être arrêté et redémarré.</span><span class="sxs-lookup"><span data-stu-id="bc14a-151">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="bc14a-152">Pour cela, adoptez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="bc14a-152">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="bc14a-153">Cliquez avec le bouton droit sur l’icône de barre d’état système IIS Express dans la zone de notification, puis appuyez sur **Quitter** ou sur **Arrêter le site** :</span><span class="sxs-lookup"><span data-stu-id="bc14a-153">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Icône de la barre d’état système IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu contextuel](sql/_static/stopIIS.png)

    * <span data-ttu-id="bc14a-156">Si vous exécutiez Visual Studio en mode de non-débogage, appuyez sur F5 pour l’exécuter en mode de débogage.</span><span class="sxs-lookup"><span data-stu-id="bc14a-156">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="bc14a-157">Si vous exécutiez Visual Studio en mode de débogage, arrêtez le débogueur et appuyez sur F5.</span><span class="sxs-lookup"><span data-stu-id="bc14a-157">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="bc14a-158">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="bc14a-158">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="bc14a-159">Supprimez tous les enregistrements dans la base de données (pour que la méthode seed s’exécute).</span><span class="sxs-lookup"><span data-stu-id="bc14a-159">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="bc14a-160">Arrêtez et démarrez l’application pour amorcer la base de données.</span><span class="sxs-lookup"><span data-stu-id="bc14a-160">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="bc14a-161">L’application affiche les données de départ.</span><span class="sxs-lookup"><span data-stu-id="bc14a-161">The app shows the seeded data.</span></span>

---

<span data-ttu-id="bc14a-162">Le tutoriel suivant sert à améliorer la présentation des données.</span><span class="sxs-lookup"><span data-stu-id="bc14a-162">The next tutorial will improve the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bc14a-163">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="bc14a-163">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bc14a-164">[Précédent : Pages Razor obtenues par génération de modèles automatique](xref:tutorials/razor-pages/page)
> [Suivant : Mise à jour des pages](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="bc14a-164">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="bc14a-165">L’objet `RazorPagesMovieContext` gère la tâche de connexion à la base de données et de mappage d’objets `Movie` à des enregistrements de la base de données.</span><span class="sxs-lookup"><span data-stu-id="bc14a-165">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="bc14a-166">Le contexte de base de données est inscrit auprès du conteneur [Injection de dépendances](xref:fundamentals/dependency-injection) dans la méthode `ConfigureServices` de *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="bc14a-166">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bc14a-167">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bc14a-167">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="bc14a-168">Visual Studio Code / Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="bc14a-168">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---

<span data-ttu-id="bc14a-169">Pour plus d’informations sur les méthodes utilisées dans `ConfigureServices`, consultez :</span><span class="sxs-lookup"><span data-stu-id="bc14a-169">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="bc14a-170">[Prise en charge du règlement général sur la protection des données (RGPD) de l’Union Européenne dans ASP.NET Core](xref:security/gdpr) pour `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="bc14a-170">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="bc14a-171">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="bc14a-171">SetCompatibilityVersion</span></span>](xref:mvc/compatibility-version)

<span data-ttu-id="bc14a-172">Le système de [configuration](xref:fundamentals/configuration/index) d’ASP.NET Core lit `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="bc14a-172">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="bc14a-173">Pour un développement local, il obtient la chaîne de connexion à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="bc14a-173">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bc14a-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bc14a-174">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bc14a-175">La valeur du nom de la base de données (`Database={Database name}`) est différent pour votre code généré.</span><span class="sxs-lookup"><span data-stu-id="bc14a-175">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="bc14a-176">La valeur du nom est arbitraire.</span><span class="sxs-lookup"><span data-stu-id="bc14a-176">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie22/appsettings.json)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bc14a-177">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bc14a-177">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bc14a-178">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="bc14a-178">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---

<span data-ttu-id="bc14a-179">Quand l’application est déployée sur un serveur de test ou de production, une variable d’environnement peut être utilisée pour définir la chaîne de connexion à un serveur de base de données réel.</span><span class="sxs-lookup"><span data-stu-id="bc14a-179">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="bc14a-180">Pour plus d’informations, consultez [Configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="bc14a-180">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bc14a-181">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bc14a-181">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="bc14a-182">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="bc14a-182">SQL Server Express LocalDB</span></span>

<span data-ttu-id="bc14a-183">LocalDB est une version allégée du moteur de base de données SQL Server Express, qui est ciblée pour le développement de programmes.</span><span class="sxs-lookup"><span data-stu-id="bc14a-183">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="bc14a-184">LocalDB démarre à la demande et s’exécute en mode utilisateur, ce qui n’implique aucune configuration complexe.</span><span class="sxs-lookup"><span data-stu-id="bc14a-184">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="bc14a-185">Par défaut, la base de données LocalDB crée des fichiers `*.mdf` dans le répertoire `C:/Users/<user/>`.</span><span class="sxs-lookup"><span data-stu-id="bc14a-185">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="bc14a-186">Dans le menu **Affichage**, ouvrez **l’Explorateur d’objets SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="bc14a-186">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu View](sql/_static/ssox.png)

* <span data-ttu-id="bc14a-188">Cliquez avec le bouton droit sur la table `Movie` et sélectionnez **Concepteur de vues** :</span><span class="sxs-lookup"><span data-stu-id="bc14a-188">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Menu contextuel ouvert sur la table Movie](sql/_static/design.png)

  ![Table Movie ouverte dans le Concepteur](sql/_static/dv.png)

<span data-ttu-id="bc14a-191">Notez l’icône de clé en regard de `ID`.</span><span class="sxs-lookup"><span data-stu-id="bc14a-191">Note the key icon next to `ID`.</span></span> <span data-ttu-id="bc14a-192">Par défaut, EF crée une propriété nommée `ID` pour la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="bc14a-192">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="bc14a-193">Cliquez avec le bouton droit sur la table `Movie` et sélectionnez **Afficher les données** :</span><span class="sxs-lookup"><span data-stu-id="bc14a-193">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Table Movie ouverte, affichant des données de table](sql/_static/vd22.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bc14a-195">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bc14a-195">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bc14a-196">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="bc14a-196">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---

## <a name="seed-the-database"></a><span data-ttu-id="bc14a-197">Amorcer la base de données</span><span class="sxs-lookup"><span data-stu-id="bc14a-197">Seed the database</span></span>

<span data-ttu-id="bc14a-198">Créez une classe nommée `SeedData` dans le dossier *Modèles* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="bc14a-198">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="bc14a-199">Si la base de données contient des films, l’initialiseur de valeur initiale retourne une valeur et aucun film n’est ajouté.</span><span class="sxs-lookup"><span data-stu-id="bc14a-199">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="bc14a-200">Ajouter l’initialiseur de valeur initiale</span><span class="sxs-lookup"><span data-stu-id="bc14a-200">Add the seed initializer</span></span>

<span data-ttu-id="bc14a-201">Dans *Program.cs*, modifiez la méthode `Main` pour effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="bc14a-201">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="bc14a-202">Obtenir une instance de contexte de base de données à partir du conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="bc14a-202">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="bc14a-203">Appeler la méthode seed et la passer au contexte.</span><span class="sxs-lookup"><span data-stu-id="bc14a-203">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="bc14a-204">Supprimer le contexte une fois la méthode seed terminée.</span><span class="sxs-lookup"><span data-stu-id="bc14a-204">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="bc14a-205">Le code suivant montre le fichier *Program.cs* mis à jour.</span><span class="sxs-lookup"><span data-stu-id="bc14a-205">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Program.cs)]

<span data-ttu-id="bc14a-206">Une application de production n’appelle pas `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="bc14a-206">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="bc14a-207">Il est ajouté au code précédent afin d’éviter l’exception suivante quand `Update-Database` n’a pas été exécutée :</span><span class="sxs-lookup"><span data-stu-id="bc14a-207">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="bc14a-208">SqlException : Impossible d’ouvrir la base de données 'RazorPagesMovieContext-21' demandée par la connexion.</span><span class="sxs-lookup"><span data-stu-id="bc14a-208">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="bc14a-209">La connexion a échoué.</span><span class="sxs-lookup"><span data-stu-id="bc14a-209">The login failed.</span></span>
<span data-ttu-id="bc14a-210">Échec de la connexion de l’utilisateur 'nom utilisateur'.</span><span class="sxs-lookup"><span data-stu-id="bc14a-210">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="bc14a-211">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="bc14a-211">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bc14a-212">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bc14a-212">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="bc14a-213">Supprimez tous les enregistrements de la base de données.</span><span class="sxs-lookup"><span data-stu-id="bc14a-213">Delete all the records in the DB.</span></span> <span data-ttu-id="bc14a-214">Pour ce faire, utilisez les liens de suppression disponibles dans le navigateur ou à partir de [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="bc14a-214">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="bc14a-215">Forcez l’application à s’initialiser (appelez les méthodes de la classe `Startup`) pour que la méthode seed s’exécute.</span><span class="sxs-lookup"><span data-stu-id="bc14a-215">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="bc14a-216">Pour forcer l’initialisation, IIS Express doit être arrêté et redémarré.</span><span class="sxs-lookup"><span data-stu-id="bc14a-216">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="bc14a-217">Pour cela, adoptez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="bc14a-217">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="bc14a-218">Cliquez avec le bouton droit sur l’icône de barre d’état système IIS Express dans la zone de notification, puis appuyez sur **Quitter** ou sur **Arrêter le site** :</span><span class="sxs-lookup"><span data-stu-id="bc14a-218">Right-click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Icône de la barre d’état système IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu contextuel](sql/_static/stopIIS.png)

    * <span data-ttu-id="bc14a-221">Si vous exécutiez Visual Studio en mode de non-débogage, appuyez sur F5 pour l’exécuter en mode de débogage.</span><span class="sxs-lookup"><span data-stu-id="bc14a-221">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="bc14a-222">Si vous exécutiez Visual Studio en mode de débogage, arrêtez le débogueur et appuyez sur F5.</span><span class="sxs-lookup"><span data-stu-id="bc14a-222">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bc14a-223">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bc14a-223">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="bc14a-224">Supprimez tous les enregistrements dans la base de données (pour que la méthode seed s’exécute).</span><span class="sxs-lookup"><span data-stu-id="bc14a-224">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="bc14a-225">Arrêtez et démarrez l’application pour amorcer la base de données.</span><span class="sxs-lookup"><span data-stu-id="bc14a-225">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="bc14a-226">L’application affiche les données de départ.</span><span class="sxs-lookup"><span data-stu-id="bc14a-226">The app shows the seeded data.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bc14a-227">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="bc14a-227">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="bc14a-228">Supprimez tous les enregistrements dans la base de données (pour que la méthode seed s’exécute).</span><span class="sxs-lookup"><span data-stu-id="bc14a-228">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="bc14a-229">Arrêtez et démarrez l’application pour amorcer la base de données.</span><span class="sxs-lookup"><span data-stu-id="bc14a-229">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="bc14a-230">L’application affiche les données de départ.</span><span class="sxs-lookup"><span data-stu-id="bc14a-230">The app shows the seeded data.</span></span>

---

<span data-ttu-id="bc14a-231">L’application affiche les données de départ :</span><span class="sxs-lookup"><span data-stu-id="bc14a-231">The app shows the seeded data:</span></span>

![Application Movie ouverte dans Chrome, affichant les données relatives aux films](sql/_static/m55.png)

<span data-ttu-id="bc14a-233">Le didacticiel suivant nettoie la présentation des données.</span><span class="sxs-lookup"><span data-stu-id="bc14a-233">The next tutorial will clean up the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bc14a-234">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="bc14a-234">Additional resources</span></span>

* [<span data-ttu-id="bc14a-235">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="bc14a-235">YouTube version of this tutorial</span></span>](https://youtu.be/A_5ff11sDHY)

> [!div class="step-by-step"]
> <span data-ttu-id="bc14a-236">[Précédent : Pages Razor obtenues par génération de modèles automatique](xref:tutorials/razor-pages/page)
> [Suivant : Mise à jour des pages](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="bc14a-236">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>

::: moniker-end