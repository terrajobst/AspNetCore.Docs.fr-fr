<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="8d395-101">Effectuer la génération de modèles automatique pour le modèle Movie</span><span class="sxs-lookup"><span data-stu-id="8d395-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="8d395-102">Exécutez la commande suivante dans une ligne de commande (dans le répertoire du projet qui contient les fichiers *Program.cs*, *Startup.cs* et *.csproj*) :</span><span class="sxs-lookup"><span data-stu-id="8d395-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet restore
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

<span data-ttu-id="8d395-103">Si vous obtenez l’erreur :</span><span class="sxs-lookup"><span data-stu-id="8d395-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="8d395-104">L’erreur précédente se produit quand vous êtes dans le mauvais répertoire.</span><span class="sxs-lookup"><span data-stu-id="8d395-104">The preceeding error happens when you are in the wrong directory.</span></span> <span data-ttu-id="8d395-105">Ouvrez une interface de commande dans le répertoire du projet (répertoire contenant les fichiers *Program.cs*, *Startup.cs* et *.csproj*), puis exécutez la commande précédente.</span><span class="sxs-lookup"><span data-stu-id="8d395-105">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files), and then run the preceeding command.</span></span>

<span data-ttu-id="8d395-106">Si vous obtenez l’erreur :</span><span class="sxs-lookup"><span data-stu-id="8d395-106">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="8d395-107">Quittez Visual Studio et réexécutez la commande.</span><span class="sxs-lookup"><span data-stu-id="8d395-107">Exit Visual Studio and run the command again.</span></span>
