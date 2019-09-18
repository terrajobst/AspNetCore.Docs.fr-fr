<span data-ttu-id="9865e-101">Exécutez l’échafaudage d’identité:</span><span class="sxs-lookup"><span data-stu-id="9865e-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9865e-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9865e-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9865e-103">À partir de **l’Explorateur de solutions**, avec le bouton droit sur le projet > **ajouter** > **nouvel élément structuré**.</span><span class="sxs-lookup"><span data-stu-id="9865e-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="9865e-104">Dans le volet gauche de la boîte de dialogue **Ajouter une structure** , sélectionnez **identité** > **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="9865e-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="9865e-105">Dans la boîte de dialogue **Ajouter une identité** , sélectionnez les options souhaitées.</span><span class="sxs-lookup"><span data-stu-id="9865e-105">In the **Add Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="9865e-106">Sélectionnez votre page de disposition existante, ou votre fichier de disposition sera remplacé par un balisage incorrect.</span><span class="sxs-lookup"><span data-stu-id="9865e-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="9865e-107">Lorsqu’un fichier  *\_Layout. cshtml* existant est sélectionné, il n’est **pas** remplacé.</span><span class="sxs-lookup"><span data-stu-id="9865e-107">When an existing *\_Layout.cshtml* file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="9865e-108">Par exemple: `~/Pages/Shared/_Layout.cshtml` for Razor pages `~/Views/Shared/_Layout.cshtml` pour les projets MVC</span><span class="sxs-lookup"><span data-stu-id="9865e-108">For example: `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="9865e-109">Pour utiliser le contexte de données existant, sélectionnez au moins un fichier à substituer.</span><span class="sxs-lookup"><span data-stu-id="9865e-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="9865e-110">Vous devez sélectionner au moins un fichier pour ajouter votre contexte de données.</span><span class="sxs-lookup"><span data-stu-id="9865e-110">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="9865e-111">Sélectionnez votre classe de contexte de données.</span><span class="sxs-lookup"><span data-stu-id="9865e-111">Select your data context class.</span></span>
  * <span data-ttu-id="9865e-112">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="9865e-112">Select **Add**.</span></span>
* <span data-ttu-id="9865e-113">Pour créer un nouveau contexte utilisateur et éventuellement créer une classe d’utilisateur personnalisée pour l’identité:</span><span class="sxs-lookup"><span data-stu-id="9865e-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="9865e-114">Sélectionnez le **+** bouton pour créer un nouveau **classe de contexte de données**.</span><span class="sxs-lookup"><span data-stu-id="9865e-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="9865e-115">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="9865e-115">Select **Add**.</span></span>

<span data-ttu-id="9865e-116">Remarque : Si vous créez un nouveau contexte utilisateur, vous n’êtes pas obligé de sélectionner un fichier à remplacer.</span><span class="sxs-lookup"><span data-stu-id="9865e-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="9865e-117">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="9865e-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="9865e-118">Si vous n’avez pas encore installé le Générateur de modèles automatique ASP.NET Core, vous pouvez l’installer maintenant :</span><span class="sxs-lookup"><span data-stu-id="9865e-118">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="9865e-119">Ajoutez une référence de package à [Microsoft. VisualStudio. Web. CodeGeneration. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) dans le fichier\*projet (. csproj).</span><span class="sxs-lookup"><span data-stu-id="9865e-119">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="9865e-120">Dans le répertoire du projet, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="9865e-120">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="9865e-121">Exécutez la commande suivante pour répertorier les options de génération de modèles automatique d’identité :</span><span class="sxs-lookup"><span data-stu-id="9865e-121">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="9865e-122">Dans le dossier du projet, exécutez l’échafaudage d’identité avec les options souhaitées.</span><span class="sxs-lookup"><span data-stu-id="9865e-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="9865e-123">Par exemple, pour configurer l’identité avec l’interface utilisateur par défaut et le nombre minimal de fichiers, exécutez la commande suivante.</span><span class="sxs-lookup"><span data-stu-id="9865e-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="9865e-124">Utilisez le nom complet correct pour votre contexte de base de connaissances:</span><span class="sxs-lookup"><span data-stu-id="9865e-124">Use the correct fully qualified name for your DB context:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

<span data-ttu-id="9865e-125">PowerShell utilise un point-virgule comme séparateur de commande.</span><span class="sxs-lookup"><span data-stu-id="9865e-125">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="9865e-126">Quand vous utilisez PowerShell, échapper les points-virgules dans la liste de fichiers ou placer la liste de fichiers entre guillemets doubles.</span><span class="sxs-lookup"><span data-stu-id="9865e-126">When using PowerShell, escape the semi-colons in the file list or put the file list in double quotes.</span></span> <span data-ttu-id="9865e-127">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="9865e-127">For example:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="9865e-128">Si vous exécutez l’échafaudage d’identité sans spécifier `--files` l’indicateur ou `--useDefaultUI` l’indicateur, toutes les pages d’interface utilisateur d’identité disponibles seront créées dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="9865e-128">If you run the Identity scaffolder without specifying the `--files` flag or the `--useDefaultUI` flag, all the available Identity UI pages will be created in your project.</span></span>

---
