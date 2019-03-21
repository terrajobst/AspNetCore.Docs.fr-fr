---
ms.openlocfilehash: 648ed8087c64eaf80fd055fa7dd08041ab9a8752
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320287"
---
<span data-ttu-id="4d2f5-101">Exécutez le Générateur de modèles automatique identité :</span><span class="sxs-lookup"><span data-stu-id="4d2f5-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4d2f5-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4d2f5-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4d2f5-103">À partir de **l’Explorateur de solutions**, avec le bouton droit sur le projet > **ajouter** > **nouvel élément structuré**.</span><span class="sxs-lookup"><span data-stu-id="4d2f5-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="4d2f5-104">Dans le volet gauche de la **ajouter une structure** boîte de dialogue, sélectionnez **identité** > **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="4d2f5-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="4d2f5-105">Dans le **identité ADD** boîte de dialogue, sélectionnez les options souhaitées.</span><span class="sxs-lookup"><span data-stu-id="4d2f5-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="4d2f5-106">Sélectionnez votre page de disposition existante, ou votre fichier de disposition est remplacée par balisage incorrect.</span><span class="sxs-lookup"><span data-stu-id="4d2f5-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="4d2f5-107">Par exemple `~/Pages/Shared/_Layout.cshtml` pour les Pages Razor `~/Views/Shared/_Layout.cshtml` pour les projets MVC</span><span class="sxs-lookup"><span data-stu-id="4d2f5-107">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
  * <span data-ttu-id="4d2f5-108">Sélectionnez le **+** bouton pour créer un nouveau **classe de contexte de données**.</span><span class="sxs-lookup"><span data-stu-id="4d2f5-108">Select the **+** button to create a new **Data context class**.</span></span>
* <span data-ttu-id="4d2f5-109">Sélectionnez **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="4d2f5-109">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4d2f5-110">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="4d2f5-110">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4d2f5-111">Si vous n’avez pas encore installé le Générateur de modèles automatique ASP.NET Core, vous pouvez l’installer maintenant :</span><span class="sxs-lookup"><span data-stu-id="4d2f5-111">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="4d2f5-112">Ajouter une référence de package à [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) au projet (\*.csproj) fichier.</span><span class="sxs-lookup"><span data-stu-id="4d2f5-112">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="4d2f5-113">Dans le répertoire du projet, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="4d2f5-113">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="4d2f5-114">Exécutez la commande suivante pour répertorier les options de génération de modèles automatique d’identité :</span><span class="sxs-lookup"><span data-stu-id="4d2f5-114">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="4d2f5-115">Dans le dossier du projet, exécutez le Générateur de modèles automatique identité avec les options souhaitées.</span><span class="sxs-lookup"><span data-stu-id="4d2f5-115">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="4d2f5-116">Par exemple, pour configurer l’identité avec l’interface utilisateur par défaut et le nombre minimal de fichiers, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="4d2f5-116">For example, to setup identity with the default UI and the minimum number of files, run the following command:</span></span>

```cli
dotnet aspnet-codegenerator identity --useDefaultUI
```

---
