---
title: Utiliser une base de données et ASP.NET Core
author: rick-anderson
description: Explique l’utilisation d’une base de données et d’ASP.NET Core.
ms.author: riande
ms.date: 12/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: e2e9be0aa25166e216d34419859cd907d0423f70
ms.sourcegitcommit: 34bf9fc6ea814c039401fca174642f0acb14be3c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57841564"
---
# <a name="work-with-a-database-and-aspnet-core"></a><span data-ttu-id="6d3d7-103">Utiliser une base de données et ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6d3d7-103">Work with a database and ASP.NET Core</span></span>

<span data-ttu-id="6d3d7-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="6d3d7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="6d3d7-105">L’objet `RazorPagesMovieContext` gère la tâche de connexion à la base de données et de mappage d’objets `Movie` à des enregistrements de la base de données.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-105">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="6d3d7-106">Le contexte de base de données est inscrit auprès du conteneur [Injection de dépendances](xref:fundamentals/dependency-injection) dans la méthode `ConfigureServices` de *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="6d3d7-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6d3d7-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6d3d7-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6d3d7-108">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6d3d7-108">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6d3d7-109">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="6d3d7-109">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---  
<!-- End of VS tabs -->

<span data-ttu-id="6d3d7-110">Pour plus d’informations sur les méthodes utilisées dans `ConfigureServices`, consultez :</span><span class="sxs-lookup"><span data-stu-id="6d3d7-110">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="6d3d7-111">[Prise en charge du règlement général sur la protection des données (RGPD) de l’Union Européenne dans ASP.NET Core](xref:security/gdpr) pour `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-111">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="6d3d7-112">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="6d3d7-112">SetCompatibilityVersion</span></span>](xref:mvc/compatibility-version)

<span data-ttu-id="6d3d7-113">Le système de [configuration](xref:fundamentals/configuration/index) d’ASP.NET Core lit `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-113">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="6d3d7-114">Pour un développement local, il obtient la chaîne de connexion à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-114">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6d3d7-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6d3d7-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6d3d7-116">La valeur du nom de la base de données (`Database={Database name}`) est différent pour votre code généré.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-116">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="6d3d7-117">La valeur du nom est arbitraire.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-117">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie22/appsettings.json)]

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6d3d7-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6d3d7-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6d3d7-119">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="6d3d7-119">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---  
<!-- End of VS tabs -->

<span data-ttu-id="6d3d7-120">Quand l’application est déployée sur un serveur de test ou de production, une variable d’environnement peut être utilisée pour définir la chaîne de connexion à un serveur de base de données réel.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-120">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="6d3d7-121">Pour plus d’informations, consultez [Configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="6d3d7-121">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6d3d7-122">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6d3d7-122">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="6d3d7-123">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="6d3d7-123">SQL Server Express LocalDB</span></span>

<span data-ttu-id="6d3d7-124">LocalDB est une version allégée du moteur de base de données SQL Server Express, qui est ciblée pour le développement de programmes.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-124">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="6d3d7-125">LocalDB démarre à la demande et s’exécute en mode utilisateur, ce qui n’implique aucune configuration complexe.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-125">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="6d3d7-126">Par défaut, la base de données LocalDB crée des fichiers `*.mdf` dans le répertoire `C:/Users/<user/>`.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-126">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="6d3d7-127">Dans le menu **Affichage**, ouvrez **l’Explorateur d’objets SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="6d3d7-127">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu View](sql/_static/ssox.png)

* <span data-ttu-id="6d3d7-129">Cliquez avec le bouton droit sur la table `Movie` et sélectionnez **Concepteur de vues** :</span><span class="sxs-lookup"><span data-stu-id="6d3d7-129">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Menu contextuel ouvert sur la table Movie](sql/_static/design.png)

  ![Table Movie ouverte dans le Concepteur](sql/_static/dv.png)

<span data-ttu-id="6d3d7-132">Notez l’icône de clé en regard de `ID`.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-132">Note the key icon next to `ID`.</span></span> <span data-ttu-id="6d3d7-133">Par défaut, EF crée une propriété nommée `ID` pour la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-133">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="6d3d7-134">Cliquez avec le bouton droit sur la table `Movie` et sélectionnez **Afficher les données** :</span><span class="sxs-lookup"><span data-stu-id="6d3d7-134">Right click on the `Movie` table and select **View Data**:</span></span>

  <span data-ttu-id="6d3d7-135">![Table Movie ouverte, montrant les données de la table](sql/_static/vd22.png)
</span><span class="sxs-lookup"><span data-stu-id="6d3d7-135">![Movie table open showing table data](sql/_static/vd22.png)
</span></span><!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6d3d7-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6d3d7-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6d3d7-137">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="6d3d7-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---  
<!-- End of VS tabs -->

## <a name="seed-the-database"></a><span data-ttu-id="6d3d7-138">Amorcer la base de données</span><span class="sxs-lookup"><span data-stu-id="6d3d7-138">Seed the database</span></span>

<span data-ttu-id="6d3d7-139">Créez une classe nommée `SeedData` dans le dossier *Modèles* avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="6d3d7-139">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="6d3d7-140">Si la base de données contient des films, l’initialiseur de valeur initiale retourne une valeur et aucun film n’est ajouté.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-140">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="6d3d7-141">Ajouter l’initialiseur de valeur initiale</span><span class="sxs-lookup"><span data-stu-id="6d3d7-141">Add the seed initializer</span></span>

<span data-ttu-id="6d3d7-142">Dans *Program.cs*, modifiez la méthode `Main` pour effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="6d3d7-142">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="6d3d7-143">Obtenir une instance de contexte de base de données à partir du conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-143">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="6d3d7-144">Appeler la méthode seed et la passer au contexte.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-144">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="6d3d7-145">Supprimer le contexte une fois la méthode seed terminée.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-145">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="6d3d7-146">Le code suivant montre le fichier *Program.cs* mis à jour.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-146">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Program.cs)]

<span data-ttu-id="6d3d7-147">Une application de production n’appelle pas `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-147">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="6d3d7-148">Il est ajouté au code précédent afin d’éviter l’exception suivante quand `Update-Database` n’a pas été exécutée :</span><span class="sxs-lookup"><span data-stu-id="6d3d7-148">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="6d3d7-149">SqlException : Impossible d’ouvrir la base de données 'RazorPagesMovieContext-21' demandée par la connexion.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-149">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="6d3d7-150">La connexion a échoué.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-150">The login failed.</span></span>
<span data-ttu-id="6d3d7-151">Échec de la connexion de l’utilisateur 'nom utilisateur'.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-151">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="6d3d7-152">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="6d3d7-152">Test the app</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6d3d7-153">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6d3d7-153">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6d3d7-154">Supprimez tous les enregistrements de la base de données.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-154">Delete all the records in the DB.</span></span> <span data-ttu-id="6d3d7-155">Pour ce faire, utilisez les liens de suppression disponibles dans le navigateur ou à partir de [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="6d3d7-155">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="6d3d7-156">Forcez l’application à s’initialiser (appelez les méthodes de la classe `Startup`) pour que la méthode seed s’exécute.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-156">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="6d3d7-157">Pour forcer l’initialisation, IIS Express doit être arrêté et redémarré.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-157">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="6d3d7-158">Pour cela, adoptez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="6d3d7-158">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="6d3d7-159">Cliquez avec le bouton droit sur l’icône de barre d’état système IIS Express dans la zone de notification, puis appuyez sur **Quitter** ou sur **Arrêter le site** :</span><span class="sxs-lookup"><span data-stu-id="6d3d7-159">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Icône de la barre d’état système IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu contextuel](sql/_static/stopIIS.png)

    * <span data-ttu-id="6d3d7-162">Si vous exécutiez Visual Studio en mode de non-débogage, appuyez sur F5 pour l’exécuter en mode de débogage.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-162">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="6d3d7-163">Si vous exécutiez Visual Studio en mode de débogage, arrêtez le débogueur et appuyez sur F5.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-163">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6d3d7-164">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6d3d7-164">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6d3d7-165">Supprimez tous les enregistrements dans la base de données (pour que la méthode seed s’exécute).</span><span class="sxs-lookup"><span data-stu-id="6d3d7-165">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="6d3d7-166">Arrêtez et démarrez l’application pour amorcer la base de données.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-166">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="6d3d7-167">L’application affiche les données de départ.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-167">The app shows the seeded data.</span></span>

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6d3d7-168">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="6d3d7-168">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="6d3d7-169">Supprimez tous les enregistrements dans la base de données (pour que la méthode seed s’exécute).</span><span class="sxs-lookup"><span data-stu-id="6d3d7-169">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="6d3d7-170">Arrêtez et démarrez l’application pour amorcer la base de données.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-170">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="6d3d7-171">L’application affiche les données de départ.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-171">The app shows the seeded data.</span></span>

---  
<!-- End of VS tabs -->


   
<span data-ttu-id="6d3d7-172">L’application affiche les données de départ :</span><span class="sxs-lookup"><span data-stu-id="6d3d7-172">The app shows the seeded data:</span></span>

![Application Movie ouverte dans Chrome, affichant les données relatives aux films](sql/_static/m55.png)

<span data-ttu-id="6d3d7-174">Le didacticiel suivant nettoie la présentation des données.</span><span class="sxs-lookup"><span data-stu-id="6d3d7-174">The next tutorial will clean up the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6d3d7-175">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="6d3d7-175">Additional resources</span></span>

* [<span data-ttu-id="6d3d7-176">Version YouTube de ce tutoriel</span><span class="sxs-lookup"><span data-stu-id="6d3d7-176">YouTube version of this tutorial</span></span>](https://youtu.be/A_5ff11sDHY)

> [!div class="step-by-step"]
> <span data-ttu-id="6d3d7-177">[Précédent : Pages Razor obtenues par génération de modèles automatique](xref:tutorials/razor-pages/page)
> [Suivant : Mise à jour des pages](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="6d3d7-177">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
