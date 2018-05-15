---
title: Résoudre les problèmes pour ASP.NET Core
author: Rick-Anderson
description: Comprendre et résoudre les avertissements et erreurs avec les projets ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: testing/troubleshoot
ms.openlocfilehash: f2c785bfe27ddd67db0313b8ee1c077a8cc06e05
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2018
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="4f4c7-103">Résoudre les problèmes des projets ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f4c7-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="4f4c7-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4f4c7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4f4c7-105">Les liens suivants fournissent des conseils de dépannage :</span><span class="sxs-lookup"><span data-stu-id="4f4c7-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="4f4c7-106">Résoudre les problèmes liés à ASP.NET Core sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="4f4c7-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="4f4c7-107">Résoudre les problèmes liés à ASP.NET Core sur IIS</span><span class="sxs-lookup"><span data-stu-id="4f4c7-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="4f4c7-108">Informations de référence sur les erreurs courantes pour Azure App Service et IIS avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f4c7-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="4f4c7-109">YouTube : diagnostic des problèmes dans les applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f4c7-109">YouTube: Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a><span data-ttu-id="4f4c7-110">Avertissements du Kit de développement .NET core</span><span class="sxs-lookup"><span data-stu-id="4f4c7-110">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="4f4c7-111">Les versions de 64 bits du Kit de développement .NET Core et 32 bits sont installées.</span><span class="sxs-lookup"><span data-stu-id="4f4c7-111">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>
<span data-ttu-id="4f4c7-112">Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="4f4c7-112">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span> 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Une capture d’écran de la boîte de dialogue OneASP.NET affichant le message d’avertissement](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="4f4c7-114">Cet avertissement s’affiche lorsque les (x86) 32 bits et les versions 64 bits (x 64) de la [le Kit de développement .NET Core](https://www.microsoft.com/net/download/all) sont installés.</span><span class="sxs-lookup"><span data-stu-id="4f4c7-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="4f4c7-115">Les deux versions peuvent être installées causes courantes :</span><span class="sxs-lookup"><span data-stu-id="4f4c7-115">Common reasons both versions can be installed include:</span></span>

* <span data-ttu-id="4f4c7-116">Vous initialement téléchargé le programme d’installation du Kit de développement .NET Core à l’aide d’un ordinateur 32 bits, mais puis copié sur et installez sur un ordinateur 64 bits.</span><span class="sxs-lookup"><span data-stu-id="4f4c7-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine, but then copied it across and installed it on a 64-bit machine.</span></span> 
* <span data-ttu-id="4f4c7-117">Le Kit de développement 32 bits .NET Core a été installée par une autre application.</span><span class="sxs-lookup"><span data-stu-id="4f4c7-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="4f4c7-118">Version incorrecte a été téléchargée et installée.</span><span class="sxs-lookup"><span data-stu-id="4f4c7-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="4f4c7-119">Désinstallez le Kit de développement 32 bits .NET Core pour éviter cet avertissement.</span><span class="sxs-lookup"><span data-stu-id="4f4c7-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="4f4c7-120">Désinstallation à partir de **le panneau de configuration** > **programmes et fonctionnalités** > **désinstaller ou modifier un programme**.</span><span class="sxs-lookup"><span data-stu-id="4f4c7-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="4f4c7-121">Si vous comprenez la raison pour laquelle l’avertissement se produit et ses implications, vous pouvez ignorer l’avertissement.</span><span class="sxs-lookup"><span data-stu-id="4f4c7-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="4f4c7-122">Le Kit de développement .NET Core est installé dans plusieurs emplacements</span><span class="sxs-lookup"><span data-stu-id="4f4c7-122">The .NET Core SDK is installed in multiple locations</span></span>
<span data-ttu-id="4f4c7-123">Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="4f4c7-123">In the **New Project** dialog for ASP.NET Core you may see the following warning:</span></span> 

 <span data-ttu-id="4f4c7-124">Le Kit de développement .NET Core est installé dans plusieurs emplacements.</span><span class="sxs-lookup"><span data-stu-id="4f4c7-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="4f4c7-125">Seuls les modèles à partir de la SDK(s) installé sur « C:\Program Files\dotnet\sdk\' s’affiche.</span><span class="sxs-lookup"><span data-stu-id="4f4c7-125">Only templates from the SDK(s) installed at 'C:\Program Files\dotnet\sdk\' will be displayed.</span></span>

![Une capture d’écran de la boîte de dialogue OneASP.NET affichant le message d’avertissement](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="4f4c7-127">Ce message s’affiche lorsque vous avez au moins une installation du Kit de développement .NET Core dans un répertoire en dehors de * C:\Program Files\dotnet\sdk\*.</span><span class="sxs-lookup"><span data-stu-id="4f4c7-127">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\Program Files\dotnet\sdk\*.</span></span> <span data-ttu-id="4f4c7-128">Cela se produit généralement lorsque le Kit de développement .NET Core a été déployée sur un ordinateur à l’aide de copier/coller au lieu du programme d’installation MSI.</span><span class="sxs-lookup"><span data-stu-id="4f4c7-128">Usually that happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="4f4c7-129">Désinstallez le Kit de développement 32 bits .NET Core pour éviter cet avertissement.</span><span class="sxs-lookup"><span data-stu-id="4f4c7-129">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="4f4c7-130">Désinstallation à partir de **le panneau de configuration** > **programmes et fonctionnalités** > **désinstaller ou modifier un programme**.</span><span class="sxs-lookup"><span data-stu-id="4f4c7-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="4f4c7-131">Si vous comprenez la raison pour laquelle l’avertissement se produit et ses implications, vous pouvez ignorer l’avertissement.</span><span class="sxs-lookup"><span data-stu-id="4f4c7-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="4f4c7-132">Aucun kits de développement logiciel .NET Core ont été détectés.</span><span class="sxs-lookup"><span data-stu-id="4f4c7-132">No .NET Core SDKs were detected</span></span>
<span data-ttu-id="4f4c7-133">Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="4f4c7-133">In the **New Project** dialog for ASP.NET Core you may see the following warning:</span></span> 

<span data-ttu-id="4f4c7-134">**Aucun kits de développement logiciel .NET Core ont été détectés, assurez-vous qu’ils sont inclus dans la variable d’environnement « PATH »**</span><span class="sxs-lookup"><span data-stu-id="4f4c7-134">**No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'**</span></span>

![Une capture d’écran de la boîte de dialogue OneASP.NET affichant le message d’avertissement](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="4f4c7-136">Cet avertissement s’affiche lorsque la variable d’environnement `PATH` ne pointe pas vers les kits de développement logiciel .NET Core sur l’ordinateur.</span><span class="sxs-lookup"><span data-stu-id="4f4c7-136">This warning appears when the environment variable `PATH` doesn’t point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="4f4c7-137">Pour résoudre ce problème :</span><span class="sxs-lookup"><span data-stu-id="4f4c7-137">To resolve this problem:</span></span>

* <span data-ttu-id="4f4c7-138">Installer ou vérifiez que le Kit de développement .NET Core est installé.</span><span class="sxs-lookup"><span data-stu-id="4f4c7-138">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="4f4c7-139">Vérifiez le `PATH` variable d’environnement pointe vers l’emplacement du Kit de développement logiciel est installé.</span><span class="sxs-lookup"><span data-stu-id="4f4c7-139">Verify the `PATH` environment variable points to the location the SDK is installed.</span></span> <span data-ttu-id="4f4c7-140">Le programme d’installation définit normalement le `PATH`.</span><span class="sxs-lookup"><span data-stu-id="4f4c7-140">The installer normally sets the `PATH`.</span></span>