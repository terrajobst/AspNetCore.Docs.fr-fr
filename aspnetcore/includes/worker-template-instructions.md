# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e51a4-101">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e51a4-101">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="e51a4-102">Créer un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="e51a4-102">Create a new project.</span></span>
1. <span data-ttu-id="e51a4-103">Sélectionnez **Worker service**.</span><span class="sxs-lookup"><span data-stu-id="e51a4-103">Select **Worker Service**.</span></span> <span data-ttu-id="e51a4-104">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="e51a4-104">Select **Next**.</span></span>
1. <span data-ttu-id="e51a4-105">Indiquez un nom de projet dans le champ **Nom du projet**, ou acceptez le nom de projet par défaut.</span><span class="sxs-lookup"><span data-stu-id="e51a4-105">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="e51a4-106">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="e51a4-106">Select **Create**.</span></span>
1. <span data-ttu-id="e51a4-107">Dans la boîte de dialogue **créer un service de travail** , sélectionnez **créer**.</span><span class="sxs-lookup"><span data-stu-id="e51a4-107">In the **Create a new Worker service** dialog, select **Create**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e51a4-108">Visual Studio pour Mac</span><span class="sxs-lookup"><span data-stu-id="e51a4-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="e51a4-109">Créer un nouveau projet.</span><span class="sxs-lookup"><span data-stu-id="e51a4-109">Create a new project.</span></span>
1. <span data-ttu-id="e51a4-110">Sélectionnez **application** sous **.net Core** dans la barre latérale.</span><span class="sxs-lookup"><span data-stu-id="e51a4-110">Select **App** under **.NET Core** in the sidebar.</span></span>
1. <span data-ttu-id="e51a4-111">Sélectionnez **Worker** sous **ASP.net Core**.</span><span class="sxs-lookup"><span data-stu-id="e51a4-111">Select **Worker** under **ASP.NET Core**.</span></span> <span data-ttu-id="e51a4-112">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="e51a4-112">Select **Next**.</span></span>
1. <span data-ttu-id="e51a4-113">Sélectionnez **.net Core 3,0** ou une version ultérieure pour le **Framework cible**.</span><span class="sxs-lookup"><span data-stu-id="e51a4-113">Select **.NET Core 3.0** or later for the **Target Framework**.</span></span> <span data-ttu-id="e51a4-114">Sélectionnez **Suivant**.</span><span class="sxs-lookup"><span data-stu-id="e51a4-114">Select **Next**.</span></span>
1. <span data-ttu-id="e51a4-115">Indiquez un nom dans le champ **nom du projet** .</span><span class="sxs-lookup"><span data-stu-id="e51a4-115">Provide a name in the **Project Name** field.</span></span> <span data-ttu-id="e51a4-116">Sélectionnez **Créer**.</span><span class="sxs-lookup"><span data-stu-id="e51a4-116">Select **Create**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e51a4-117">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="e51a4-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e51a4-118">Utilisez le modèle Service Worker (`worker`) avec la commande [dotnet new](/dotnet/core/tools/dotnet-new) à partir d’un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="e51a4-118">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="e51a4-119">Dans l’exemple suivant, une application Service Worker est créée et se nomme `ContosoWorker`.</span><span class="sxs-lookup"><span data-stu-id="e51a4-119">In the following example, a Worker Service app is created named `ContosoWorker`.</span></span> <span data-ttu-id="e51a4-120">Un dossier pour l’application `ContosoWorker` est créé automatiquement durant l’exécution de la commande.</span><span class="sxs-lookup"><span data-stu-id="e51a4-120">A folder for the `ContosoWorker` app is created automatically when the command is executed.</span></span>

```dotnetcli
dotnet new worker -o ContosoWorker
```

---
