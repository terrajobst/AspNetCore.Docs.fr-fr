<span data-ttu-id="2e854-101">Exécutez le Générateur de modèles automatique identité :</span><span class="sxs-lookup"><span data-stu-id="2e854-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2e854-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2e854-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2e854-103">À partir de **l’Explorateur de solutions**, avec le bouton droit sur le projet > **ajouter** > **nouvel élément structuré**.</span><span class="sxs-lookup"><span data-stu-id="2e854-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="2e854-104">Dans le volet gauche de la **ajouter une structure** boîte de dialogue, sélectionnez **identité** > **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="2e854-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="2e854-105">Dans le **identité ADD** boîte de dialogue, sélectionnez les options souhaitées.</span><span class="sxs-lookup"><span data-stu-id="2e854-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="2e854-106">Sélectionnez votre page de disposition existante, ou votre fichier de disposition est remplacée par balisage incorrect.</span><span class="sxs-lookup"><span data-stu-id="2e854-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="2e854-107">Lorsqu’un fichier _Layout.cshtml existant est sélectionné, il est **pas** remplacé.</span><span class="sxs-lookup"><span data-stu-id="2e854-107">When an existing _Layout.cshtml file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="2e854-108">Par exemple `~/Pages/Shared/_Layout.cshtml` pour les Pages Razor `~/Views/Shared/_Layout.cshtml` pour les projets MVC</span><span class="sxs-lookup"><span data-stu-id="2e854-108">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="2e854-109">Pour utiliser votre contexte de données existant, sélectionnez au moins un fichier à remplacer.</span><span class="sxs-lookup"><span data-stu-id="2e854-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="2e854-110">Vous devez sélectionner au moins un fichier pour ajouter votre contexte de données.</span><span class="sxs-lookup"><span data-stu-id="2e854-110">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="2e854-111">Sélectionnez votre classe de contexte de données.</span><span class="sxs-lookup"><span data-stu-id="2e854-111">Select your data context class.</span></span>
  * <span data-ttu-id="2e854-112">Sélectionnez **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="2e854-112">Select **ADD**.</span></span>
* <span data-ttu-id="2e854-113">Pour créer un nouveau contexte de l’utilisateur et éventuellement créer une classe d’utilisateur personnalisée pour l’identité :</span><span class="sxs-lookup"><span data-stu-id="2e854-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="2e854-114">Sélectionnez le **+** bouton pour créer un nouveau **classe de contexte de données**.</span><span class="sxs-lookup"><span data-stu-id="2e854-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="2e854-115">Sélectionnez **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="2e854-115">Select **ADD**.</span></span>

<span data-ttu-id="2e854-116">Remarque : Si vous créez un nouveau contexte utilisateur, il est inutile de sélectionner un fichier à remplacer.</span><span class="sxs-lookup"><span data-stu-id="2e854-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2e854-117">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="2e854-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="2e854-118">Si vous n’avez pas encore installé le Générateur de modèles automatique ASP.NET, vous pouvez l’installer maintenant :</span><span class="sxs-lookup"><span data-stu-id="2e854-118">If you have not previously installed the ASP.NET scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="2e854-119">Ajouter une référence de package à [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) au projet (\*.csproj) fichier.</span><span class="sxs-lookup"><span data-stu-id="2e854-119">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="2e854-120">Dans le répertoire du projet, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="2e854-120">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="2e854-121">Exécutez la commande suivante pour répertorier les options de génération de modèles automatique d’identité :</span><span class="sxs-lookup"><span data-stu-id="2e854-121">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="2e854-122">Dans le dossier du projet, exécutez le Générateur de modèles automatique identité avec les options souhaitées.</span><span class="sxs-lookup"><span data-stu-id="2e854-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="2e854-123">Par exemple, pour configurer l’identité avec l’interface utilisateur par défaut et le nombre minimal de fichiers, exécutez la commande suivante.</span><span class="sxs-lookup"><span data-stu-id="2e854-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="2e854-124">Utilisez le nom qualifié complet correct pour votre contexte de base de données :</span><span class="sxs-lookup"><span data-stu-id="2e854-124">Use the correct fully qualified name for your DB context:</span></span>

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

-------------
