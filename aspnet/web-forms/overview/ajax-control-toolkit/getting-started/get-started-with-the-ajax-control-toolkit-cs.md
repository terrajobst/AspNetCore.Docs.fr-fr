---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: Prise en main la boîte à outils de contrôle AJAX (c#) | Documents Microsoft
author: microsoft
description: En savoir plus vous devez connaître pour commencer à utiliser les outils de contrôle AJAX.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: e6a7a8d45f32a33eaacf3c42b52a02d2ada1aab6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879177"
---
<a name="get-started-with-the-ajax-control-toolkit-c"></a><span data-ttu-id="a5ee6-103">Prise en main la boîte à outils de contrôle AJAX (c#)</span><span class="sxs-lookup"><span data-stu-id="a5ee6-103">Get Started with the AJAX Control Toolkit (C#)</span></span>
====================
<span data-ttu-id="a5ee6-104">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a5ee6-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="a5ee6-105">En savoir plus vous devez connaître pour commencer à utiliser les outils de contrôle AJAX.</span><span class="sxs-lookup"><span data-stu-id="a5ee6-105">Learn all you need to know to get started using the AJAX Control Toolkit.</span></span>


<span data-ttu-id="a5ee6-106">La boîte à outils de contrôle AJAX contient plus de 30 contrôles gratuits que vous pouvez utiliser dans vos applications ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a5ee6-106">The AJAX Control Toolkit contains more than 30 free controls that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="a5ee6-107">Dans ce didacticiel, vous allez apprendre à télécharger les outils de contrôle AJAX et ajouter les contrôles de boîte à outils à votre boîte à outils Visual Studio/Visual Web Developer Express.</span><span class="sxs-lookup"><span data-stu-id="a5ee6-107">In this tutorial, you learn how to download the AJAX Control Toolkit and add the toolkit controls to your Visual Studio/Visual Web Developer Express toolbox.</span></span>

## <a name="downloading-the-ajax-control-toolkit"></a><span data-ttu-id="a5ee6-108">Téléchargement de la boîte à outils de contrôle AJAX</span><span class="sxs-lookup"><span data-stu-id="a5ee6-108">Downloading the AJAX Control Toolkit</span></span>

<span data-ttu-id="a5ee6-109">Le [outils de contrôle AJAX](http://devexpress.com/act) est un projet open source développé par les membres de la Communauté ASP.NET et l’équipe d’ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a5ee6-109">The [AJAX Control Toolkit](http://devexpress.com/act) is an open source project developed by the members of the ASP.NET community and the ASP.NET team.</span></span> 


<span data-ttu-id="a5ee6-110">[![Téléchargement de la boîte à outils de contrôle AJAX](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a5ee6-110">[![Downloading the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)</span></span>

<span data-ttu-id="a5ee6-111">**Figure 01**: téléchargement de la boîte à outils de contrôle AJAX ([cliquez pour afficher l’image en taille réelle](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="a5ee6-111">**Figure 01**: Downloading the AJAX Control Toolkit([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))</span></span>


<span data-ttu-id="a5ee6-112">Après avoir téléchargé le fichier, vous devez débloquer le fichier.</span><span class="sxs-lookup"><span data-stu-id="a5ee6-112">After you download the file, you need to unblock the file.</span></span> <span data-ttu-id="a5ee6-113">Cliquez sur le fichier, sélectionnez Propriétés, puis cliquez sur le **Unblock** bouton (voir Figure 2).</span><span class="sxs-lookup"><span data-stu-id="a5ee6-113">Right-click the file, select Properties, and click the **Unblock** button (see Figure 2).</span></span>


<span data-ttu-id="a5ee6-114">[![Débloquer le fichier ZIP de boîte à outils de contrôle AJAX](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="a5ee6-114">[![Unblocking the AJAX Control Toolkit ZIP file](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)</span></span>

<span data-ttu-id="a5ee6-115">**Figure 02**: débloquer le fichier ZIP de boîte à outils de contrôle AJAX ([cliquez pour afficher l’image en taille réelle](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="a5ee6-115">**Figure 02**: Unblocking the AJAX Control Toolkit ZIP file([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))</span></span>


<span data-ttu-id="a5ee6-116">Après avoir débloqué le fichier, vous pouvez décompresser le fichier : cliquez sur le fichier et sélectionnez le **extraire tout** option de menu.</span><span class="sxs-lookup"><span data-stu-id="a5ee6-116">After you unblock the file, you can unzip the file: Right-click the file and select the **Extract All** menu option.</span></span> <span data-ttu-id="a5ee6-117">Maintenant, nous sommes prêts à ajouter de la boîte à outils à la boîte à outils de Visual Studio/Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="a5ee6-117">Now, we are ready to add the toolkit to the Visual Studio/Visual Web Developer toolbox.</span></span>

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a><span data-ttu-id="a5ee6-118">Ajout de la boîte à outils de contrôle AJAX à la boîte à outils</span><span class="sxs-lookup"><span data-stu-id="a5ee6-118">Adding the AJAX Control Toolkit to the Toolbox</span></span>

<span data-ttu-id="a5ee6-119">Le moyen le plus simple d’utiliser les outils de contrôle AJAX consiste à ajouter de la boîte à outils à votre boîte à outils de Visual Studio/Visual Web Developer (voir Figure 3).</span><span class="sxs-lookup"><span data-stu-id="a5ee6-119">The easiest way to use the AJAX Control Toolkit is to add the toolkit to your Visual Studio/Visual Web Developer toolbox (see Figure 3).</span></span> <span data-ttu-id="a5ee6-120">De cette façon, vous pouvez simplement faire glisser un contrôle de boîte à outils sur une page lorsque vous souhaitez utiliser.</span><span class="sxs-lookup"><span data-stu-id="a5ee6-120">That way, you can simply drag a toolkit control onto a page when you want to use it.</span></span>


<span data-ttu-id="a5ee6-121">[![Outils de contrôle AJAX s’affiche dans la boîte à outils](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="a5ee6-121">[![AJAX Control Toolkit appears in toolbox](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)</span></span>

<span data-ttu-id="a5ee6-122">**Figure 03**: outils de contrôle AJAX s’affiche dans la boîte à outils ([cliquez pour afficher l’image en taille réelle](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="a5ee6-122">**Figure 03**: AJAX Control Toolkit appears in toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))</span></span>


<span data-ttu-id="a5ee6-123">Tout d’abord, vous devez ajouter un onglet de boîte à outils de contrôle AJAX à la boîte à outils.</span><span class="sxs-lookup"><span data-stu-id="a5ee6-123">First, you need to add an AJAX Control Toolkit tab to the toolbox.</span></span> <span data-ttu-id="a5ee6-124">Procédez comme suit.</span><span class="sxs-lookup"><span data-stu-id="a5ee6-124">Follow these steps.</span></span>

1. <span data-ttu-id="a5ee6-125">Créer un site Web ASP.NET en sélectionnant l’option de menu fichier, nouveau site Web.</span><span class="sxs-lookup"><span data-stu-id="a5ee6-125">Create a new ASP.NET Website by selecting the menu option File, New Website.</span></span> <span data-ttu-id="a5ee6-126">Double-cliquez sur la page Default.aspx dans la fenêtre de l’Explorateur de solutions pour ouvrir le fichier dans l’éditeur.</span><span class="sxs-lookup"><span data-stu-id="a5ee6-126">Double-click the Default.aspx in the Solution Explorer window to open the file in the editor.</span></span>
2. <span data-ttu-id="a5ee6-127">Avec le bouton droit de la boîte à outils sous l’onglet Général et sélectionnez l’option de menu **ajouter un onglet** (voir Figure 4).</span><span class="sxs-lookup"><span data-stu-id="a5ee6-127">Right-click the Toolbox beneath the General Tab and select the menu option **Add Tab** (see Figure 4).</span></span>
3. <span data-ttu-id="a5ee6-128">Entrez un nouvel onglet Outils de contrôle AJAX.</span><span class="sxs-lookup"><span data-stu-id="a5ee6-128">Enter a new tab named AJAX Control Toolkit.</span></span>


<span data-ttu-id="a5ee6-129">[![Ajout d’un nouvel onglet](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="a5ee6-129">[![Adding a new tab](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)</span></span>

<span data-ttu-id="a5ee6-130">**Figure 04**: ajout d’un nouvel onglet ([cliquez pour afficher l’image en taille réelle](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="a5ee6-130">**Figure 04**: Adding a new tab([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))</span></span>


<span data-ttu-id="a5ee6-131">Ensuite, vous devez ajouter les contrôles de boîte à outils de contrôle AJAX pour le nouvel onglet. Procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="a5ee6-131">Next, you need to add the AJAX Control Toolkit controls to the new tab. Follow these steps:</span></span>

- <span data-ttu-id="a5ee6-132">Avec le bouton droit sous l’onglet Outils de contrôle AJAX et sélectionnez l’option de menu **choisir des éléments (voir Figure 5)**.</span><span class="sxs-lookup"><span data-stu-id="a5ee6-132">Right-click beneath the AJAX Control Toolkit tab and select the menu option **Choose Items (see Figure 5)**.</span></span>
- <span data-ttu-id="a5ee6-133">Accédez à l’emplacement où vous avez décompressé les outils de contrôle AJAX et sélectionnez l’assembly AjaxControlToolkit.dll.</span><span class="sxs-lookup"><span data-stu-id="a5ee6-133">Browse to the location where you unzipped the AJAX Control Toolkit and select the AjaxControlToolkit.dll assembly.</span></span>


<span data-ttu-id="a5ee6-134">[![Choisissez les éléments à ajouter à la boîte à outils](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="a5ee6-134">[![Choose items to add to the toolbox](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)</span></span>

<span data-ttu-id="a5ee6-135">**Figure 05**: choisissez les éléments à ajouter à la boîte à outils ([cliquez pour afficher l’image en taille réelle](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="a5ee6-135">**Figure 05**: Choose items to add to the toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))</span></span>


<span data-ttu-id="a5ee6-136">Après avoir effectué ces étapes, tous les contrôles de boîte à outils seront affiche dans votre boîte à outils.</span><span class="sxs-lookup"><span data-stu-id="a5ee6-136">After you complete these steps, all of the toolkit controls will appear in your toolbox.</span></span>

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a><span data-ttu-id="a5ee6-137">La mise à niveau vers une nouvelle Version de la boîte à outils</span><span class="sxs-lookup"><span data-stu-id="a5ee6-137">Upgrading to a New Version of the Toolkit</span></span>

<span data-ttu-id="a5ee6-138">Si vous utilisiez une version antérieure du Kit de ressources et que vous devez maintenant déplacer vers une version ultérieure ici sont les étapes recommandées :</span><span class="sxs-lookup"><span data-stu-id="a5ee6-138">If you were using an older release of the Toolkit and now need to move to a later version here are the recommended steps:</span></span>

- <span data-ttu-id="a5ee6-139">Fichiers binaires - supprimer l’ancienne version de l’assembly AjaxControlToolkit.dll dans votre dossier Bin du site Web.</span><span class="sxs-lookup"><span data-stu-id="a5ee6-139">Binaries - Delete the old version of the AjaxControlToolkit.dll assembly from your website Bin folder.</span></span>
- <span data-ttu-id="a5ee6-140">Les éléments de boîte à outils - supprimer de l’onglet Outils de contrôle AJAX et suivez les étapes ci-dessus pour créer de nouveau l’onglet avec la nouvelle version de l’assembly AjaxControlToolkit.dll.</span><span class="sxs-lookup"><span data-stu-id="a5ee6-140">Toolbox Items - Delete the AJAX Control Toolkit tab and follow the steps above to re-create the tab with the new version of the AjaxControlToolkit.dll assembly.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a5ee6-141">Next</span><span class="sxs-lookup"><span data-stu-id="a5ee6-141">Next</span></span>](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
