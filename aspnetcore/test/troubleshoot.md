---
title: Résoudre les problèmes des projets ASP.NET Core
author: Rick-Anderson
description: Comprenez et résolvez les avertissements et les erreurs avec les projets ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: test/troubleshoot
ms.openlocfilehash: 150f2192bb4b6dd0d330fd678d9c5fa0bf31673e
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090109"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="efc89-103">Résoudre les problèmes des projets ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="efc89-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="efc89-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="efc89-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="efc89-105">Les liens suivants fournissent des conseils de dépannage :</span><span class="sxs-lookup"><span data-stu-id="efc89-105">The following links provide troubleshooting guidance:</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="efc89-106">Conférence NDC (Londres, 2018) : Diagnostic des problèmes dans les Applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="efc89-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="efc89-107">Blog de l’ASP.NET : Résolution des problèmes de performances d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="efc89-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="efc89-108">Avertissements du SDK .NET core</span><span class="sxs-lookup"><span data-stu-id="efc89-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="efc89-109">32 bits et 64 bits des versions du SDK .NET Core sont installées.</span><span class="sxs-lookup"><span data-stu-id="efc89-109">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="efc89-110">Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="efc89-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="efc89-111">32 et 64 bits des versions du SDK .NET Core sont installées.</span><span class="sxs-lookup"><span data-stu-id="efc89-111">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="efc89-112">Seuls les modèles à partir de l’ou les versions 64 bits installée sur ' C:\\Program Files\\dotnet\\sdk\\» s’affiche.</span><span class="sxs-lookup"><span data-stu-id="efc89-112">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Une capture d’écran de la boîte de dialogue OneASP.NET affichant le message d’avertissement](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="efc89-114">Cet avertissement s’affiche lorsque les (x86) 32 bits et les versions 64 bits (x 64) de la [du SDK .NET Core](https://www.microsoft.com/net/download/all) sont installés.</span><span class="sxs-lookup"><span data-stu-id="efc89-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="efc89-115">Raisons courantes, les deux versions peuvent être installées sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="efc89-115">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="efc89-116">Vous initialement téléchargé le programme d’installation du SDK .NET Core à l’aide d’un ordinateur 32 bits, mais ensuite copié sur et installé sur un ordinateur 64 bits.</span><span class="sxs-lookup"><span data-stu-id="efc89-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="efc89-117">Le SDK .NET Core de 32 bits a été installé par une autre application.</span><span class="sxs-lookup"><span data-stu-id="efc89-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="efc89-118">Une version incorrecte a été téléchargée et installée.</span><span class="sxs-lookup"><span data-stu-id="efc89-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="efc89-119">Désinstaller le SDK 32 bits de .NET Core pour éviter cet avertissement.</span><span class="sxs-lookup"><span data-stu-id="efc89-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="efc89-120">Désinstaller à partir de **le panneau de configuration** > **programmes et fonctionnalités** > **désinstaller ou modifier un programme**.</span><span class="sxs-lookup"><span data-stu-id="efc89-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="efc89-121">Si vous comprenez pourquoi l’avertissement se produit et ses implications, vous pouvez ignorer l’avertissement.</span><span class="sxs-lookup"><span data-stu-id="efc89-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="efc89-122">Le SDK .NET Core est installé dans plusieurs emplacements</span><span class="sxs-lookup"><span data-stu-id="efc89-122">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="efc89-123">Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="efc89-123">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="efc89-124">Le SDK .NET Core est installé dans plusieurs emplacements.</span><span class="sxs-lookup"><span data-stu-id="efc89-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="efc89-125">Seuls les modèles des SDK installés à ' C:\\Program Files\\dotnet\\sdk\\» s’affiche.</span><span class="sxs-lookup"><span data-stu-id="efc89-125">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Une capture d’écran de la boîte de dialogue OneASP.NET affichant le message d’avertissement](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="efc89-127">Ce message s’affiche lorsque vous avez au moins une installation du SDK .NET Core dans un répertoire en dehors de *C:\\Program Files\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="efc89-127">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="efc89-128">Cela se produit généralement lorsque le SDK .NET Core a été déployé sur un ordinateur à l’aide de copier/coller au lieu du programme d’installation MSI.</span><span class="sxs-lookup"><span data-stu-id="efc89-128">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="efc89-129">Désinstaller le SDK 32 bits de .NET Core pour éviter cet avertissement.</span><span class="sxs-lookup"><span data-stu-id="efc89-129">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="efc89-130">Désinstaller à partir de **le panneau de configuration** > **programmes et fonctionnalités** > **désinstaller ou modifier un programme**.</span><span class="sxs-lookup"><span data-stu-id="efc89-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="efc89-131">Si vous comprenez pourquoi l’avertissement se produit et ses implications, vous pouvez ignorer l’avertissement.</span><span class="sxs-lookup"><span data-stu-id="efc89-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="efc89-132">Aucun SDK .NET Core ont été détectées.</span><span class="sxs-lookup"><span data-stu-id="efc89-132">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="efc89-133">Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="efc89-133">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="efc89-134">Aucun SDK .NET Core ont été détectées, assurez-vous qu’ils sont inclus dans la variable d’environnement « PATH ».</span><span class="sxs-lookup"><span data-stu-id="efc89-134">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Une capture d’écran de la boîte de dialogue OneASP.NET affichant le message d’avertissement](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="efc89-136">Cet avertissement apparaît lorsque la variable d’environnement `PATH` ne pointe pas vers n’importe quel SDK .NET Core sur l’ordinateur.</span><span class="sxs-lookup"><span data-stu-id="efc89-136">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="efc89-137">Pour résoudre ce problème :</span><span class="sxs-lookup"><span data-stu-id="efc89-137">To resolve this problem:</span></span>

* <span data-ttu-id="efc89-138">Installer ou vérifiez que le SDK .NET Core est installé.</span><span class="sxs-lookup"><span data-stu-id="efc89-138">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="efc89-139">Vérifiez que le `PATH` variable d’environnement pointe vers l’emplacement dans lequel le SDK est installé.</span><span class="sxs-lookup"><span data-stu-id="efc89-139">Verify that the `PATH` environment variable points to the location in which the SDK is installed.</span></span> <span data-ttu-id="efc89-140">Le programme d’installation définit normalement le `PATH`.</span><span class="sxs-lookup"><span data-stu-id="efc89-140">The installer normally sets the `PATH`.</span></span>
