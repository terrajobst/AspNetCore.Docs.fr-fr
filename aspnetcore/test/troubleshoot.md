---
title: Résoudre les problèmes des projets ASP.NET Core
author: Rick-Anderson
description: Comprenez et résolvez les avertissements et les erreurs avec les projets ASP.NET Core.
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: ae4e6f191d8f856de60ecf21cb882b5ee9b02064
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274591"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="54e7d-103">Résoudre les problèmes des projets ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="54e7d-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="54e7d-104">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="54e7d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="54e7d-105">Les liens suivants fournissent des conseils de dépannage :</span><span class="sxs-lookup"><span data-stu-id="54e7d-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="54e7d-106">Résoudre les problèmes liés à ASP.NET Core sur Azure App Service</span><span class="sxs-lookup"><span data-stu-id="54e7d-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="54e7d-107">Résoudre les problèmes liés à ASP.NET Core sur IIS</span><span class="sxs-lookup"><span data-stu-id="54e7d-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="54e7d-108">Informations de référence sur les erreurs courantes pour Azure App Service et IIS avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="54e7d-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="54e7d-109">Conférence NDC (Londres, 2018) : Diagnostiquer les problèmes dans les Applications ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="54e7d-109">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="54e7d-110">Blog de l’ASP.NET : Résolution des problèmes de performances d’ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="54e7d-110">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="54e7d-111">Avertissements du Kit de développement .NET core</span><span class="sxs-lookup"><span data-stu-id="54e7d-111">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="54e7d-112">Les versions de 64 bits du Kit de développement .NET Core et 32 bits sont installées.</span><span class="sxs-lookup"><span data-stu-id="54e7d-112">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="54e7d-113">Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="54e7d-113">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="54e7d-114">32 et 64 bits des versions du Kit de développement .NET Core sont installées.</span><span class="sxs-lookup"><span data-stu-id="54e7d-114">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="54e7d-115">Seuls les modèles à partir de l’ou les versions 64 bits installée sur ' C:\\Program Files\\dotnet\\sdk\\» s’affichera.</span><span class="sxs-lookup"><span data-stu-id="54e7d-115">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Une capture d’écran de la boîte de dialogue OneASP.NET affichant le message d’avertissement](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="54e7d-117">Cet avertissement s’affiche lorsque les (x86) 32 bits et les versions 64 bits (x 64) de la [le Kit de développement .NET Core](https://www.microsoft.com/net/download/all) sont installés.</span><span class="sxs-lookup"><span data-stu-id="54e7d-117">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="54e7d-118">Les deux versions peuvent être installées causes courantes :</span><span class="sxs-lookup"><span data-stu-id="54e7d-118">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="54e7d-119">Vous initialement téléchargé le programme d’installation du Kit de développement .NET Core à l’aide d’un ordinateur 32 bits, mais puis copié sur et installez sur un ordinateur 64 bits.</span><span class="sxs-lookup"><span data-stu-id="54e7d-119">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="54e7d-120">Le Kit de développement 32 bits .NET Core a été installée par une autre application.</span><span class="sxs-lookup"><span data-stu-id="54e7d-120">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="54e7d-121">Version incorrecte a été téléchargée et installée.</span><span class="sxs-lookup"><span data-stu-id="54e7d-121">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="54e7d-122">Désinstallez le Kit de développement 32 bits .NET Core pour éviter cet avertissement.</span><span class="sxs-lookup"><span data-stu-id="54e7d-122">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="54e7d-123">Désinstallation à partir de **le panneau de configuration** > **programmes et fonctionnalités** > **désinstaller ou modifier un programme**.</span><span class="sxs-lookup"><span data-stu-id="54e7d-123">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="54e7d-124">Si vous comprenez la raison pour laquelle l’avertissement se produit et ses implications, vous pouvez ignorer l’avertissement.</span><span class="sxs-lookup"><span data-stu-id="54e7d-124">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="54e7d-125">Le Kit de développement .NET Core est installé dans plusieurs emplacements</span><span class="sxs-lookup"><span data-stu-id="54e7d-125">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="54e7d-126">Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="54e7d-126">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="54e7d-127">Le Kit de développement .NET Core est installé dans plusieurs emplacements.</span><span class="sxs-lookup"><span data-stu-id="54e7d-127">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="54e7d-128">Seuls les modèles à partir de la SDK(s) installé sur ' C:\\Program Files\\dotnet\\Kit de développement logiciel\\» s’affiche.</span><span class="sxs-lookup"><span data-stu-id="54e7d-128">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Une capture d’écran de la boîte de dialogue OneASP.NET affichant le message d’avertissement](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="54e7d-130">Ce message s’affiche lorsque vous avez au moins une installation du Kit de développement .NET Core dans un répertoire en dehors de *C:\\Program Files\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="54e7d-130">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="54e7d-131">Cela se produit généralement lorsque le Kit de développement .NET Core a été déployée sur un ordinateur à l’aide de copier/coller au lieu du programme d’installation MSI.</span><span class="sxs-lookup"><span data-stu-id="54e7d-131">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="54e7d-132">Désinstallez le Kit de développement 32 bits .NET Core pour éviter cet avertissement.</span><span class="sxs-lookup"><span data-stu-id="54e7d-132">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="54e7d-133">Désinstallation à partir de **le panneau de configuration** > **programmes et fonctionnalités** > **désinstaller ou modifier un programme**.</span><span class="sxs-lookup"><span data-stu-id="54e7d-133">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="54e7d-134">Si vous comprenez la raison pour laquelle l’avertissement se produit et ses implications, vous pouvez ignorer l’avertissement.</span><span class="sxs-lookup"><span data-stu-id="54e7d-134">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="54e7d-135">Aucun kits de développement logiciel .NET Core ont été détectés.</span><span class="sxs-lookup"><span data-stu-id="54e7d-135">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="54e7d-136">Dans le **nouveau projet** boîte de dialogue pour ASP.NET Core, vous pouvez voir l’avertissement suivant :</span><span class="sxs-lookup"><span data-stu-id="54e7d-136">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="54e7d-137">Aucun kits de développement logiciel .NET Core ont été détectés, assurez-vous qu’ils sont inclus dans la variable d’environnement « PATH ».</span><span class="sxs-lookup"><span data-stu-id="54e7d-137">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Une capture d’écran de la boîte de dialogue OneASP.NET affichant le message d’avertissement](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="54e7d-139">Cet avertissement s’affiche lorsque la variable d’environnement `PATH` ne pointe pas vers les kits de développement logiciel .NET Core sur l’ordinateur.</span><span class="sxs-lookup"><span data-stu-id="54e7d-139">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="54e7d-140">Pour résoudre ce problème :</span><span class="sxs-lookup"><span data-stu-id="54e7d-140">To resolve this problem:</span></span>

* <span data-ttu-id="54e7d-141">Installer ou vérifiez que le Kit de développement .NET Core est installé.</span><span class="sxs-lookup"><span data-stu-id="54e7d-141">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="54e7d-142">Vérifiez le `PATH` variable d’environnement pointe vers l’emplacement du Kit de développement logiciel est installé.</span><span class="sxs-lookup"><span data-stu-id="54e7d-142">Verify the `PATH` environment variable points to the location the SDK is installed.</span></span> <span data-ttu-id="54e7d-143">Le programme d’installation définit normalement le `PATH`.</span><span class="sxs-lookup"><span data-stu-id="54e7d-143">The installer normally sets the `PATH`.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-app-deadlocks"></a><span data-ttu-id="54e7d-144">Utilisation de IHtmlHelper.Partial peut provoquer des blocages d’application</span><span class="sxs-lookup"><span data-stu-id="54e7d-144">Use of IHtmlHelper.Partial may result in app deadlocks</span></span>

<span data-ttu-id="54e7d-145">Dans ASP.NET Core 2.1 et versions ultérieures, l’appel `Html.Partial` génère un avertissement de l’analyseur en raison du risque de blocages.</span><span class="sxs-lookup"><span data-stu-id="54e7d-145">In ASP.NET Core 2.1 and later, calling `Html.Partial` results in an analyzer warning due to the potential for deadlocks.</span></span> <span data-ttu-id="54e7d-146">Le message d’avertissement est :</span><span class="sxs-lookup"><span data-stu-id="54e7d-146">The warning message is:</span></span>

> <span data-ttu-id="54e7d-147">Utilisation de IHtmlHelper.Partial peut entraîner des blocages d’application.</span><span class="sxs-lookup"><span data-stu-id="54e7d-147">Use of IHtmlHelper.Partial may result in application deadlocks.</span></span> <span data-ttu-id="54e7d-148">Envisagez d’utiliser `<partial>` application d’assistance de balise ou `IHtmlHelper.PartialAsync`.</span><span class="sxs-lookup"><span data-stu-id="54e7d-148">Consider using `<partial>` Tag Helper or `IHtmlHelper.PartialAsync`.</span></span>

<span data-ttu-id="54e7d-149">Les appels à `@Html.Partial` doit être remplacé par `@await Html.PartialAsync` ou l’application d’assistance d’étiquette partielle `<partial name="_Partial" />`.</span><span class="sxs-lookup"><span data-stu-id="54e7d-149">Calls to `@Html.Partial` should be replaced by `@await Html.PartialAsync` or the partial tag helper `<partial name="_Partial" />`.</span></span>

::: moniker-end
