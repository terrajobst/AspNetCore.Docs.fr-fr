---
title: Utilisation de SQL Server LocalDB
author: rick-anderson
description: Utilisation de SQL Server LocalDB avec une application MVC simple
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: a0aa6fdfa51650628021a4ba6d0533e7e0e39200
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2018
---
# <a name="working-with-sql-server-localdb"></a><span data-ttu-id="45cb5-103">Utilisation de SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="45cb5-103">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="45cb5-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="45cb5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="45cb5-105">L’objet `MvcMovieContext` gère la tâche de connexion à la base de données et de mappage d’objets `Movie` à des enregistrements de la base de données.</span><span class="sxs-lookup"><span data-stu-id="45cb5-105">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="45cb5-106">Le contexte de base de données est inscrit auprès du conteneur [Injection de dépendances](xref:fundamentals/dependency-injection) dans la méthode `ConfigureServices` du fichier *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="45cb5-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

<span data-ttu-id="45cb5-107">Le système de [configuration](xref:fundamentals/configuration/index) d’ASP.NET Core lit `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="45cb5-107">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="45cb5-108">Pour un développement local, il obtient la chaîne de connexion à partir du fichier *appsettings.json* :</span><span class="sxs-lookup"><span data-stu-id="45cb5-108">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[Main](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="45cb5-109">Quand vous déployez l’application sur un serveur de test ou de production, vous pouvez utiliser une variable d’environnement ou une autre approche pour définir un serveur SQL Server réel comme chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="45cb5-109">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="45cb5-110">Pour plus d’informations, consultez [Configuration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="45cb5-110">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="45cb5-111">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="45cb5-111">SQL Server Express LocalDB</span></span>

<span data-ttu-id="45cb5-112">LocalDB est une version allégée du moteur de base de données SQL Server Express qui est ciblée pour le développement de programmes.</span><span class="sxs-lookup"><span data-stu-id="45cb5-112">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="45cb5-113">LocalDB démarre à la demande et s’exécute en mode utilisateur, ce qui n’implique aucune configuration complexe.</span><span class="sxs-lookup"><span data-stu-id="45cb5-113">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="45cb5-114">Par défaut, la base de données LocalDB crée des fichiers « \*.mdf » dans le répertoire *C:/Users/\<utilisateur\>*.</span><span class="sxs-lookup"><span data-stu-id="45cb5-114">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

* <span data-ttu-id="45cb5-115">Dans le menu **Affichage**, ouvrez **l’Explorateur d’objets SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="45cb5-115">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu View](working-with-sql/_static/ssox.png)

* <span data-ttu-id="45cb5-117">Cliquez avec le bouton droit sur la table `Movie` **> Concepteur de vue**.</span><span class="sxs-lookup"><span data-stu-id="45cb5-117">Right click on the `Movie` table **> View Designer**</span></span>

  ![Menu contextuel ouvert sur la table Movie](working-with-sql/_static/design.png)

  ![Table Movie ouverte dans le Concepteur](working-with-sql/_static/dv.png)

<span data-ttu-id="45cb5-120">Notez l’icône de clé en regard de `ID`.</span><span class="sxs-lookup"><span data-stu-id="45cb5-120">Note the key icon next to `ID`.</span></span> <span data-ttu-id="45cb5-121">Par défaut, EF fait d’une propriété nommée `ID` la clé primaire.</span><span class="sxs-lookup"><span data-stu-id="45cb5-121">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="45cb5-122">Cliquez avec le bouton droit sur la table `Movie` **> Afficher les données**.</span><span class="sxs-lookup"><span data-stu-id="45cb5-122">Right click on the `Movie` table **> View Data**</span></span>

  ![Menu contextuel ouvert sur la table Movie](working-with-sql/_static/ssox2.png)

  ![Table Movie ouverte, affichant des données de table](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="45cb5-125">Amorcer la base de données</span><span class="sxs-lookup"><span data-stu-id="45cb5-125">Seed the database</span></span>

<span data-ttu-id="45cb5-126">Créez une classe nommée `SeedData` dans l’espace de noms *Modèles*.</span><span class="sxs-lookup"><span data-stu-id="45cb5-126">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="45cb5-127">Remplacez le code généré par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="45cb5-127">Replace the generated code with the following:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="45cb5-128">Si la base de données contient des films, l’initialiseur de valeur initiale retourne une valeur et aucun film n’est ajouté.</span><span class="sxs-lookup"><span data-stu-id="45cb5-128">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="45cb5-129">Ajouter l’initialiseur de valeur initiale</span><span class="sxs-lookup"><span data-stu-id="45cb5-129">Add the seed initializer</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="45cb5-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="45cb5-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="45cb5-131">Ajoutez l’initialiseur de valeur initiale à la méthode `Main` dans le fichier *Program.cs* :</span><span class="sxs-lookup"><span data-stu-id="45cb5-131">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="45cb5-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="45cb5-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="45cb5-133">Ajoutez l’initialiseur de valeur initiale à la fin de la méthode `Configure` dans le fichier *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="45cb5-133">Add the seed initializer to the end of the `Configure` method in the *Startup.cs* file.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]

---

<span data-ttu-id="45cb5-134">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="45cb5-134">Test the app</span></span>

* <span data-ttu-id="45cb5-135">Supprimez tous les enregistrements de la base de données.</span><span class="sxs-lookup"><span data-stu-id="45cb5-135">Delete all the records in the DB.</span></span> <span data-ttu-id="45cb5-136">Pour ce faire, utilisez les liens de suppression disponibles dans le navigateur ou à partir de SSOX.</span><span class="sxs-lookup"><span data-stu-id="45cb5-136">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="45cb5-137">Forcez l’application à s’initialiser (appelez les méthodes de la classe `Startup`) pour que la méthode seed s’exécute.</span><span class="sxs-lookup"><span data-stu-id="45cb5-137">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="45cb5-138">Pour forcer l’initialisation, IIS Express doit être arrêté et redémarré.</span><span class="sxs-lookup"><span data-stu-id="45cb5-138">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="45cb5-139">Pour cela, adoptez l’une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="45cb5-139">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="45cb5-140">Cliquez avec le bouton droit sur l’icône de barre d’état système IIS Express dans la zone de notification, puis appuyez sur **Quitter** ou sur **Arrêter le site**.</span><span class="sxs-lookup"><span data-stu-id="45cb5-140">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![Icône de la barre d’état système IIS Express](working-with-sql/_static/iisExIcon.png)

    ![Menu contextuel](working-with-sql/_static/stopIIS.png)

   * <span data-ttu-id="45cb5-143">Si vous exécutiez Visual Studio en mode non-débogage, appuyez sur F5 pour l’exécuter en mode débogage.</span><span class="sxs-lookup"><span data-stu-id="45cb5-143">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
   * <span data-ttu-id="45cb5-144">Si vous exécutiez Visual Studio en mode débogage, arrêtez le débogueur et appuyez sur F5.</span><span class="sxs-lookup"><span data-stu-id="45cb5-144">If you were running VS in debug mode, stop the debugger and press F5</span></span>
   
<span data-ttu-id="45cb5-145">L’application affiche les données de départ.</span><span class="sxs-lookup"><span data-stu-id="45cb5-145">The app shows the seeded data.</span></span>

![Application MVC Movie ouverte dans Microsoft Edge, affichant les données relatives aux films](working-with-sql/_static/m55.png)

>[!div class="step-by-step"]
<span data-ttu-id="45cb5-147">[Précédent](adding-model.md)
[Suivant](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="45cb5-147">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>  
