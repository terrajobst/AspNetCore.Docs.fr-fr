---
title: Utilisation de SQL Server LocalDB et d’ASP.NET Core
author: rick-anderson
description: Explique l’utilisation de SQL Server LocalDB et d’ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 1fd86fb61b7f5ddb760992ac10f9bee43ab831cf
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272141"
---
# <a name="work-with-sql-server-localdb-and-aspnet-core"></a><span data-ttu-id="9d14d-103">Utilisation de SQL Server LocalDB et d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9d14d-103">Work with SQL Server LocalDB and ASP.NET Core</span></span>

<span data-ttu-id="9d14d-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="9d14d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="9d14d-105">L’objet `MovieContext` gère la tâche de connexion à la base de données et de mappage d’objets `Movie` à des enregistrements de la base de données.</span><span class="sxs-lookup"><span data-stu-id="9d14d-105">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="9d14d-106">Le contexte de base de données est inscrit auprès du conteneur [Injection de dépendances](xref:fundamentals/dependency-injection) dans la méthode `ConfigureServices` du fichier *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="9d14d-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="9d14d-107">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]</span><span class="sxs-lookup"><span data-stu-id="9d14d-107">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="9d14d-108">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]</span><span class="sxs-lookup"><span data-stu-id="9d14d-108">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]</span></span>

<span data-ttu-id="9d14d-109">Pour plus d’informations sur les méthodes utilisées dans `ConfigureServices`, consultez :</span><span class="sxs-lookup"><span data-stu-id="9d14d-109">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="9d14d-110">[Prise en charge du règlement général sur la protection des données (RGPD) de l’Union Européenne dans ASP.NET Core](xref:security/gdpr) pour `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="9d14d-110">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="9d14d-111">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="9d14d-111">SetCompatibilityVersion</span></span>](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc)

::: moniker-end

<span data-ttu-id="9d14d-112">Le système de [configuration](xref:fundamentals/configuration/index) d’ASP.NET Core lit `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="9d14d-112">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="9d14d-113">Pour un développement local, il obtient la chaîne de connexion à partir du fichier *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9d14d-113">For local development, it gets the connection string from the *appsettings.json* file.</span></span> <span data-ttu-id="9d14d-114">La valeur du nom de la base de données (`Database={Database name}`) est différent pour votre code généré.</span><span class="sxs-lookup"><span data-stu-id="9d14d-114">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="9d14d-115">La valeur du nom est arbitraire.</span><span class="sxs-lookup"><span data-stu-id="9d14d-115">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="9d14d-116">Quand vous déployez l’application sur un serveur de test ou de production, vous pouvez utiliser une variable d’environnement ou une autre approche pour définir un serveur SQL Server réel comme chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="9d14d-116">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="9d14d-117">Pour plus d’informations, consultez [Configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="9d14d-117">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="9d14d-118">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="9d14d-118">SQL Server Express LocalDB</span></span>

<span data-ttu-id="9d14d-119">LocalDB est une version allégée du moteur de base de données SQL Server Express qui est ciblée pour le développement de programmes.</span><span class="sxs-lookup"><span data-stu-id="9d14d-119">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="9d14d-120">LocalDB démarre à la demande et s’exécute en mode utilisateur, ce qui n’implique aucune configuration complexe.</span><span class="sxs-lookup"><span data-stu-id="9d14d-120">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="9d14d-121">Par défaut, la base de données LocalDB crée des fichiers « \*.mdf » dans le répertoire *C:/Users/\<utilisateur\>*.</span><span class="sxs-lookup"><span data-stu-id="9d14d-121">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="9d14d-122">Dans le menu **Affichage**, ouvrez **l’Explorateur d’objets SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="9d14d-122">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu View](sql/_static/ssox.png)

* <span data-ttu-id="9d14d-124">Cliquez avec le bouton droit sur la table `Movie` et sélectionnez **Concepteur de vues** :</span><span class="sxs-lookup"><span data-stu-id="9d14d-124">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Menu contextuel ouvert sur la table Movie](sql/_static/design.png)

  ![Table Movie ouverte dans le Concepteur](sql/_static/dv.png)

<span data-ttu-id="9d14d-127">Notez l’icône de clé en regard de `ID`.</span><span class="sxs-lookup"><span data-stu-id="9d14d-127">Note the key icon next to `ID`.</span></span> <span data-ttu-id="9d14d-128">Par défaut, EF crée une propriété nommée `ID` pour la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="9d14d-128">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="9d14d-129">Cliquez avec le bouton droit sur la table `Movie` et sélectionnez **Afficher les données** :</span><span class="sxs-lookup"><span data-stu-id="9d14d-129">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Table Movie ouverte, affichant des données de table](sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="9d14d-131">Amorcer la base de données</span><span class="sxs-lookup"><span data-stu-id="9d14d-131">Seed the database</span></span>

<span data-ttu-id="9d14d-132">Créez une classe nommée `SeedData` dans l’espace de noms *Modèles*.</span><span class="sxs-lookup"><span data-stu-id="9d14d-132">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="9d14d-133">Remplacez le code généré par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="9d14d-133">Replace the generated code with the following:</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="9d14d-134">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="9d14d-134">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9d14d-135">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="9d14d-135">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]</span></span>

::: moniker-end

<span data-ttu-id="9d14d-136">Si la base de données contient des films, l’initialiseur de valeur initiale retourne une valeur et aucun film n’est ajouté.</span><span class="sxs-lookup"><span data-stu-id="9d14d-136">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="9d14d-137">Ajouter l’initialiseur de valeur initiale</span><span class="sxs-lookup"><span data-stu-id="9d14d-137">Add the seed initializer</span></span>

<span data-ttu-id="9d14d-138">Dans *Program.cs*, modifiez la méthode `Main` pour effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="9d14d-138">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="9d14d-139">Obtenir une instance de contexte de base de données à partir du conteneur d’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="9d14d-139">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="9d14d-140">Appeler la méthode seed et la passer au contexte.</span><span class="sxs-lookup"><span data-stu-id="9d14d-140">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="9d14d-141">Supprimer le contexte une fois la méthode seed terminée.</span><span class="sxs-lookup"><span data-stu-id="9d14d-141">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="9d14d-142">Le code suivant montre le fichier *Program.cs* mis à jour.</span><span class="sxs-lookup"><span data-stu-id="9d14d-142">The following code shows the updated *Program.cs* file.</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="9d14d-143">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="9d14d-143">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9d14d-144">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="9d14d-144">[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]</span></span>

::: moniker-end

<span data-ttu-id="9d14d-145">Une application de production n’appelle pas `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="9d14d-145">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="9d14d-146">Il est ajouté au code précédent afin d’éviter l’exception suivante quand `Update-Database` n’a pas été exécutée :</span><span class="sxs-lookup"><span data-stu-id="9d14d-146">It's added to the preceeding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="9d14d-147">SqlException: impossible d’ouvrir la base de données 'RazorPagesMovieContext-21' demandée par la connexion.</span><span class="sxs-lookup"><span data-stu-id="9d14d-147">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="9d14d-148">La connexion a échoué.</span><span class="sxs-lookup"><span data-stu-id="9d14d-148">The login failed.</span></span>
<span data-ttu-id="9d14d-149">Échec de la connexion de l’utilisateur 'nom utilisateur'.</span><span class="sxs-lookup"><span data-stu-id="9d14d-149">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="9d14d-150">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="9d14d-150">Test the app</span></span>

* <span data-ttu-id="9d14d-151">Supprimez tous les enregistrements de la base de données.</span><span class="sxs-lookup"><span data-stu-id="9d14d-151">Delete all the records in the DB.</span></span> <span data-ttu-id="9d14d-152">Pour ce faire, utilisez les liens de suppression disponibles dans le navigateur ou à partir de [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="9d14d-152">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="9d14d-153">Forcez l’application à s’initialiser (appelez les méthodes de la classe `Startup`) pour que la méthode seed s’exécute.</span><span class="sxs-lookup"><span data-stu-id="9d14d-153">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="9d14d-154">Pour forcer l’initialisation, IIS Express doit être arrêté et redémarré.</span><span class="sxs-lookup"><span data-stu-id="9d14d-154">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="9d14d-155">Pour cela, adoptez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="9d14d-155">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="9d14d-156">Cliquez avec le bouton droit sur l’icône de barre d’état système IIS Express dans la zone de notification, puis appuyez sur **Quitter** ou sur **Arrêter le site** :</span><span class="sxs-lookup"><span data-stu-id="9d14d-156">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Icône de la barre d’état système IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu contextuel](sql/_static/stopIIS.png)

    * <span data-ttu-id="9d14d-159">Si vous exécutiez Visual Studio en mode de non-débogage, appuyez sur F5 pour l’exécuter en mode de débogage.</span><span class="sxs-lookup"><span data-stu-id="9d14d-159">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="9d14d-160">Si vous exécutiez Visual Studio en mode de débogage, arrêtez le débogueur et appuyez sur F5.</span><span class="sxs-lookup"><span data-stu-id="9d14d-160">If you were running VS in debug mode, stop the debugger and press F5.</span></span>
   
<span data-ttu-id="9d14d-161">L’application affiche les données de départ :</span><span class="sxs-lookup"><span data-stu-id="9d14d-161">The app shows the seeded data:</span></span>

![Application Movie ouverte dans Chrome, affichant les données relatives aux films](sql/_static/m55.png)

<span data-ttu-id="9d14d-163">Le didacticiel suivant nettoie la présentation des données.</span><span class="sxs-lookup"><span data-stu-id="9d14d-163">The next tutorial will clean up the presentation of the data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9d14d-164">[Précédent : Pages Razor obtenues par génération de modèles automatiques](xref:tutorials/razor-pages/page)
> [Suivant : Mises à jour des pages](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="9d14d-164">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
