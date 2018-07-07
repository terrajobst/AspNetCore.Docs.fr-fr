---
title: Résoudre les problèmes des projets ASP.NET Core
author: Rick-Anderson
description: Comprenez et résolvez les avertissements et les erreurs avec les projets ASP.NET Core.
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: b4d90f541a4cda2d41b49101b7ea39af87a4dcb4
ms.sourcegitcommit: a09820f91e71a7d98b7347bf93210abb9e995e22
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889010"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="0b867-103">Résoudre les problèmes des projets ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b867-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="0b867-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0b867-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0b867-105">Les liens suivants fournissent des conseils de dépannage :</span><span class="sxs-lookup"><span data-stu-id="0b867-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="0b867-106">Résoudre les problèmes liés à ASP.NET Core sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="0b867-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="0b867-107">Résoudre les problèmes liés à ASP.NET Core sur IIS</span><span class="sxs-lookup"><span data-stu-id="0b867-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="0b867-108">Informations de référence sur les erreurs courantes pour Azure App Service et IIS avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b867-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="0b867-109">Conférence NDC (Londres, 2018) : Diagnostic des problèmes dans les Applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b867-109">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="0b867-110">Blog de l’ASP.NET : Résolution des problèmes de performances d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b867-110">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="0b867-111">Avertissements du SDK .NET core</span><span class="sxs-lookup"><span data-stu-id="0b867-111">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="0b867-112">32 bits et 64 bits des versions du SDK .NET Core sont installées.</span><span class="sxs-lookup"><span data-stu-id="0b867-112">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="0b867-113">Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="0b867-113">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="0b867-114">32 et 64 bits des versions du SDK .NET Core sont installées.</span><span class="sxs-lookup"><span data-stu-id="0b867-114">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="0b867-115">Seuls les modèles à partir de l’ou les versions 64 bits installée sur ' C:\\Program Files\\dotnet\\sdk\\» s’affiche.</span><span class="sxs-lookup"><span data-stu-id="0b867-115">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Une capture d’écran de la boîte de dialogue OneASP.NET affichant le message d’avertissement](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="0b867-117">Cet avertissement s’affiche lorsque les (x86) 32 bits et les versions 64 bits (x 64) de la [du SDK .NET Core](https://www.microsoft.com/net/download/all) sont installés.</span><span class="sxs-lookup"><span data-stu-id="0b867-117">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="0b867-118">Raisons courantes, les deux versions peuvent être installées sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="0b867-118">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="0b867-119">Vous initialement téléchargé le programme d’installation du SDK .NET Core à l’aide d’un ordinateur 32 bits, mais ensuite copié sur et installé sur un ordinateur 64 bits.</span><span class="sxs-lookup"><span data-stu-id="0b867-119">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="0b867-120">Le SDK .NET Core de 32 bits a été installé par une autre application.</span><span class="sxs-lookup"><span data-stu-id="0b867-120">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="0b867-121">Une version incorrecte a été téléchargée et installée.</span><span class="sxs-lookup"><span data-stu-id="0b867-121">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="0b867-122">Désinstaller le SDK 32 bits de .NET Core pour éviter cet avertissement.</span><span class="sxs-lookup"><span data-stu-id="0b867-122">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="0b867-123">Désinstaller à partir de **le panneau de configuration** > **programmes et fonctionnalités** > **désinstaller ou modifier un programme**.</span><span class="sxs-lookup"><span data-stu-id="0b867-123">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="0b867-124">Si vous comprenez pourquoi l’avertissement se produit et ses implications, vous pouvez ignorer l’avertissement.</span><span class="sxs-lookup"><span data-stu-id="0b867-124">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="0b867-125">Le SDK .NET Core est installé dans plusieurs emplacements</span><span class="sxs-lookup"><span data-stu-id="0b867-125">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="0b867-126">Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="0b867-126">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="0b867-127">Le SDK .NET Core est installé dans plusieurs emplacements.</span><span class="sxs-lookup"><span data-stu-id="0b867-127">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="0b867-128">Seuls les modèles des SDK installés à ' C:\\Program Files\\dotnet\\sdk\\» s’affiche.</span><span class="sxs-lookup"><span data-stu-id="0b867-128">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Une capture d’écran de la boîte de dialogue OneASP.NET affichant le message d’avertissement](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="0b867-130">Ce message s’affiche lorsque vous avez au moins une installation du SDK .NET Core dans un répertoire en dehors de *C:\\Program Files\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="0b867-130">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="0b867-131">Cela se produit généralement lorsque le SDK .NET Core a été déployé sur un ordinateur à l’aide de copier/coller au lieu du programme d’installation MSI.</span><span class="sxs-lookup"><span data-stu-id="0b867-131">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="0b867-132">Désinstaller le SDK 32 bits de .NET Core pour éviter cet avertissement.</span><span class="sxs-lookup"><span data-stu-id="0b867-132">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="0b867-133">Désinstaller à partir de **le panneau de configuration** > **programmes et fonctionnalités** > **désinstaller ou modifier un programme**.</span><span class="sxs-lookup"><span data-stu-id="0b867-133">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="0b867-134">Si vous comprenez pourquoi l’avertissement se produit et ses implications, vous pouvez ignorer l’avertissement.</span><span class="sxs-lookup"><span data-stu-id="0b867-134">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="0b867-135">Aucun SDK .NET Core ont été détectées.</span><span class="sxs-lookup"><span data-stu-id="0b867-135">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="0b867-136">Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="0b867-136">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="0b867-137">Aucun SDK .NET Core ont été détectées, assurez-vous qu’ils sont inclus dans la variable d’environnement « PATH ».</span><span class="sxs-lookup"><span data-stu-id="0b867-137">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Une capture d’écran de la boîte de dialogue OneASP.NET affichant le message d’avertissement](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="0b867-139">Cet avertissement apparaît lorsque la variable d’environnement `PATH` ne pointe pas vers n’importe quel SDK .NET Core sur l’ordinateur.</span><span class="sxs-lookup"><span data-stu-id="0b867-139">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="0b867-140">Pour résoudre ce problème :</span><span class="sxs-lookup"><span data-stu-id="0b867-140">To resolve this problem:</span></span>

* <span data-ttu-id="0b867-141">Installer ou vérifiez que le SDK .NET Core est installé.</span><span class="sxs-lookup"><span data-stu-id="0b867-141">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="0b867-142">Vérifiez que le `PATH` variable d’environnement pointe vers l’emplacement dans lequel le SDK est installé.</span><span class="sxs-lookup"><span data-stu-id="0b867-142">Verify that the `PATH` environment variable points to the location in which the SDK is installed.</span></span> <span data-ttu-id="0b867-143">Le programme d’installation définit normalement le `PATH`.</span><span class="sxs-lookup"><span data-stu-id="0b867-143">The installer normally sets the `PATH`.</span></span>