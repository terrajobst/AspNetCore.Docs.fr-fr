<span data-ttu-id="d2e17-101">Exécutez l’échafaudage d’identité :</span><span class="sxs-lookup"><span data-stu-id="d2e17-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="d2e17-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d2e17-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d2e17-103">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet > **Ajouter** > **nouvel élément de génération de modèles**automatique.</span><span class="sxs-lookup"><span data-stu-id="d2e17-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="d2e17-104">Dans le volet gauche de la boîte de dialogue **Ajouter une structure** , sélectionnez **identité** > **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="d2e17-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="d2e17-105">Dans la boîte de dialogue **Ajouter une identité** , sélectionnez les options souhaitées.</span><span class="sxs-lookup"><span data-stu-id="d2e17-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="d2e17-106">Sélectionnez votre page de disposition existante, ou votre fichier de disposition sera remplacé par un balisage incorrect.</span><span class="sxs-lookup"><span data-stu-id="d2e17-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="d2e17-107">Par exemple `~/Pages/Shared/_Layout.cshtml` pour Razor Pages `~/Views/Shared/_Layout.cshtml` pour les projets MVC</span><span class="sxs-lookup"><span data-stu-id="d2e17-107">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
  * <span data-ttu-id="d2e17-108">Sélectionnez le bouton **+** pour créer une **classe de contexte de données**.</span><span class="sxs-lookup"><span data-stu-id="d2e17-108">Select the **+** button to create a new **Data context class**.</span></span>
* <span data-ttu-id="d2e17-109">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="d2e17-109">Select **ADD**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="d2e17-110">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="d2e17-110">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d2e17-111">Si vous n’avez pas encore installé le Générateur de modèles automatique ASP.NET Core, vous pouvez l’installer maintenant :</span><span class="sxs-lookup"><span data-stu-id="d2e17-111">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="d2e17-112">Ajoutez les références de package NuGet nécessaires au fichier projet (\*. csproj).</span><span class="sxs-lookup"><span data-stu-id="d2e17-112">Add required NuGet package references to the project (\*.csproj) file.</span></span> <span data-ttu-id="d2e17-113">Dans le répertoire du projet, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="d2e17-113">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="d2e17-114">Exécutez la commande suivante pour répertorier les options de génération de modèles automatique d’identité :</span><span class="sxs-lookup"><span data-stu-id="d2e17-114">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

[!INCLUDE[](~/includes/scaffoldTFM.md)]

<span data-ttu-id="d2e17-115">Dans le dossier du projet, exécutez l’échafaudage d’identité avec les options souhaitées.</span><span class="sxs-lookup"><span data-stu-id="d2e17-115">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="d2e17-116">Par exemple, pour configurer l’identité avec l’interface utilisateur par défaut et le nombre minimal de fichiers, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="d2e17-116">For example, to setup identity with the default UI and the minimum number of files, run the following command:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity --useDefaultUI
```

---
