---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
title: Bien démarrer avec AJAX Control Toolkit (VB) | Microsoft Docs
author: microsoft
description: Apprendre tout ce que vous devez savoir pour commencer à utiliser les outils de contrôle AJAX.
ms.author: aspnetcontent
ms.date: 05/12/2009
ms.assetid: 9f8fa166-49a2-402c-b236-20caef0c658f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-vb
msc.type: authoredcontent
ms.openlocfilehash: 65743c9eb8eb1b02c6cf87da53222af1312f4445
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822077"
---
<a name="get-started-with-the-ajax-control-toolkit-vb"></a><span data-ttu-id="40092-103">Bien démarrer avec AJAX Control Toolkit (VB)</span><span class="sxs-lookup"><span data-stu-id="40092-103">Get Started with the AJAX Control Toolkit (VB)</span></span>
====================
<span data-ttu-id="40092-104">par [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="40092-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="40092-105">Apprendre tout ce que vous devez savoir pour commencer à utiliser les outils de contrôle AJAX.</span><span class="sxs-lookup"><span data-stu-id="40092-105">Learn all you need to know to get started using the AJAX Control Toolkit.</span></span>


<span data-ttu-id="40092-106">AJAX Control Toolkit contient plus de 30 contrôles gratuits que vous pouvez utiliser dans vos applications ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="40092-106">The AJAX Control Toolkit contains more than 30 free controls that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="40092-107">Dans ce didacticiel, vous allez apprendre à télécharger les outils de contrôle AJAX et ajoutez les contrôles de boîte à outils à votre boîte à outils Visual Studio/Visual Web Developer Express.</span><span class="sxs-lookup"><span data-stu-id="40092-107">In this tutorial, you learn how to download the AJAX Control Toolkit and add the toolkit controls to your Visual Studio/Visual Web Developer Express toolbox.</span></span>

## <a name="downloading-the-ajax-control-toolkit"></a><span data-ttu-id="40092-108">Télécharger les outils de contrôle AJAX</span><span class="sxs-lookup"><span data-stu-id="40092-108">Downloading the AJAX Control Toolkit</span></span>

<span data-ttu-id="40092-109">Le [AJAX Control Toolkit](http://devexpress.com/act) est un projet open source développé par les membres de la Communauté ASP.NET et de l’équipe ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="40092-109">The [AJAX Control Toolkit](http://devexpress.com/act) is an open source project developed by the members of the ASP.NET community and the ASP.NET team.</span></span>


<span data-ttu-id="40092-110">[![Télécharger les outils de contrôle AJAX](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="40092-110">[![Downloading the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image1.png)</span></span>

<span data-ttu-id="40092-111">**Figure 01**: téléchargement AJAX Control Toolkit ([cliquez pour afficher l’image en taille réelle](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="40092-111">**Figure 01**: Downloading the AJAX Control Toolkit([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image2.png))</span></span>


<span data-ttu-id="40092-112">Après avoir téléchargé le fichier, vous devez débloquer le fichier.</span><span class="sxs-lookup"><span data-stu-id="40092-112">After you download the file, you need to unblock the file.</span></span> <span data-ttu-id="40092-113">Cliquez sur le fichier, sélectionnez Propriétés, puis cliquez sur le **Unblock** bouton (voir Figure 2).</span><span class="sxs-lookup"><span data-stu-id="40092-113">Right-click the file, select Properties, and click the **Unblock** button (see Figure 2).</span></span>


<span data-ttu-id="40092-114">[![Débloquant le fichier ZIP de boîte à outils de contrôle AJAX](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="40092-114">[![Unblocking the AJAX Control Toolkit ZIP file](get-started-with-the-ajax-control-toolkit-vb/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image3.png)</span></span>

<span data-ttu-id="40092-115">**Figure 02**: débloquant le fichier ZIP de boîte à outils de contrôle AJAX ([cliquez pour afficher l’image en taille réelle](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="40092-115">**Figure 02**: Unblocking the AJAX Control Toolkit ZIP file([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image4.png))</span></span>


<span data-ttu-id="40092-116">Une fois que vous débloquez le fichier, vous pouvez décompresser le fichier : cliquez sur le fichier et sélectionnez le **extraire tout** option de menu.</span><span class="sxs-lookup"><span data-stu-id="40092-116">After you unblock the file, you can unzip the file: Right-click the file and select the **Extract All** menu option.</span></span> <span data-ttu-id="40092-117">Maintenant, nous sommes prêts à ajouter le Kit de ressources à la boîte à outils Visual Studio/Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="40092-117">Now, we are ready to add the toolkit to the Visual Studio/Visual Web Developer toolbox.</span></span>

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a><span data-ttu-id="40092-118">Ajout d’AJAX Control Toolkit à la boîte à outils</span><span class="sxs-lookup"><span data-stu-id="40092-118">Adding the AJAX Control Toolkit to the Toolbox</span></span>

<span data-ttu-id="40092-119">Pour utiliser les outils de contrôle AJAX le plus simple consiste à ajouter le Kit de ressources à votre boîte à outils Visual Studio/Visual Web Developer (voir Figure 3).</span><span class="sxs-lookup"><span data-stu-id="40092-119">The easiest way to use the AJAX Control Toolkit is to add the toolkit to your Visual Studio/Visual Web Developer toolbox (see Figure 3).</span></span> <span data-ttu-id="40092-120">De cette façon, vous pouvez simplement faire glisser un contrôle de boîte à outils sur une page lorsque vous souhaitez l’utiliser.</span><span class="sxs-lookup"><span data-stu-id="40092-120">That way, you can simply drag a toolkit control onto a page when you want to use it.</span></span>


<span data-ttu-id="40092-121">[![Outils de contrôle AJAX s’affiche dans la boîte à outils](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="40092-121">[![AJAX Control Toolkit appears in toolbox](get-started-with-the-ajax-control-toolkit-vb/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image5.png)</span></span>

<span data-ttu-id="40092-122">**Figure 03**: AJAX Control Toolkit apparaît dans la boîte à outils ([cliquez pour afficher l’image en taille réelle](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="40092-122">**Figure 03**: AJAX Control Toolkit appears in toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image6.png))</span></span>


<span data-ttu-id="40092-123">Tout d’abord, vous devez ajouter un onglet de boîte à outils de contrôle AJAX à la boîte à outils.</span><span class="sxs-lookup"><span data-stu-id="40092-123">First, you need to add an AJAX Control Toolkit tab to the toolbox.</span></span> <span data-ttu-id="40092-124">Suivez ces étapes.</span><span class="sxs-lookup"><span data-stu-id="40092-124">Follow these steps.</span></span>

1. <span data-ttu-id="40092-125">Créer un site Web ASP.NET en sélectionnant l’option de menu fichier, nouveau site Web.</span><span class="sxs-lookup"><span data-stu-id="40092-125">Create a new ASP.NET Website by selecting the menu option File, New Website.</span></span> <span data-ttu-id="40092-126">Double-cliquez sur la page Default.aspx dans la fenêtre Explorateur de solutions pour ouvrir le fichier dans l’éditeur.</span><span class="sxs-lookup"><span data-stu-id="40092-126">Double-click the Default.aspx in the Solution Explorer window to open the file in the editor.</span></span>
2. <span data-ttu-id="40092-127">Avec le bouton droit de la boîte à outils sous l’onglet Général et sélectionnez l’option de menu **ajouter un onglet** (voir Figure 4).</span><span class="sxs-lookup"><span data-stu-id="40092-127">Right-click the Toolbox beneath the General Tab and select the menu option **Add Tab** (see Figure 4).</span></span>
3. <span data-ttu-id="40092-128">Entrez un nouvel onglet appelé AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="40092-128">Enter a new tab named AJAX Control Toolkit.</span></span>


<span data-ttu-id="40092-129">[![Ajout d’un nouvel onglet](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="40092-129">[![Adding a new tab](get-started-with-the-ajax-control-toolkit-vb/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image7.png)</span></span>

<span data-ttu-id="40092-130">**Figure 04**: ajout d’un nouvel onglet ([cliquez pour afficher l’image en taille réelle](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="40092-130">**Figure 04**: Adding a new tab([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image8.png))</span></span>


<span data-ttu-id="40092-131">Ensuite, vous devez ajouter les contrôles AJAX Control Toolkit pour le nouvel onglet. Procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="40092-131">Next, you need to add the AJAX Control Toolkit controls to the new tab. Follow these steps:</span></span>

- <span data-ttu-id="40092-132">Avec le bouton droit sous l’onglet AJAX Control Toolkit et sélectionnez l’option de menu **choisir des éléments (voir Figure 5)**.</span><span class="sxs-lookup"><span data-stu-id="40092-132">Right-click beneath the AJAX Control Toolkit tab and select the menu option **Choose Items (see Figure 5)**.</span></span>
- <span data-ttu-id="40092-133">Accédez à l’emplacement où vous avez décompressé AJAX Control Toolkit et sélectionnez l’assembly AjaxControlToolkit.dll.</span><span class="sxs-lookup"><span data-stu-id="40092-133">Browse to the location where you unzipped the AJAX Control Toolkit and select the AjaxControlToolkit.dll assembly.</span></span>


<span data-ttu-id="40092-134">[![Choisissez les éléments à ajouter à la boîte à outils](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="40092-134">[![Choose items to add to the toolbox](get-started-with-the-ajax-control-toolkit-vb/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-vb/_static/image9.png)</span></span>

<span data-ttu-id="40092-135">**Figure 05**: choisissez les éléments à ajouter à la boîte à outils ([cliquez pour afficher l’image en taille réelle](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="40092-135">**Figure 05**: Choose items to add to the toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-vb/_static/image10.png))</span></span>


<span data-ttu-id="40092-136">Après avoir effectué ces étapes, tous les contrôles de boîte à outils seront affiche dans votre boîte à outils.</span><span class="sxs-lookup"><span data-stu-id="40092-136">After you complete these steps, all of the toolkit controls will appear in your toolbox.</span></span>

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a><span data-ttu-id="40092-137">La mise à niveau vers une nouvelle Version du Kit de ressources</span><span class="sxs-lookup"><span data-stu-id="40092-137">Upgrading to a New Version of the Toolkit</span></span>

<span data-ttu-id="40092-138">Si vous utilisiez une version antérieure du Kit de ressources et que vous devez maintenant déplacer vers une version ultérieure ici sont les étapes recommandées :</span><span class="sxs-lookup"><span data-stu-id="40092-138">If you were using an older release of the Toolkit and now need to move to a later version here are the recommended steps:</span></span>

- <span data-ttu-id="40092-139">Les fichiers binaires - supprimer l’ancienne version de l’assembly AjaxControlToolkit.dll à partir de votre dossier d’emplacement de site Web.</span><span class="sxs-lookup"><span data-stu-id="40092-139">Binaries - Delete the old version of the AjaxControlToolkit.dll assembly from your website Bin folder.</span></span>
- <span data-ttu-id="40092-140">Éléments de boîte à outils - supprimer l’onglet AJAX Control Toolkit et suivez les étapes ci-dessus pour créer de nouveau l’onglet avec la nouvelle version de l’assembly AjaxControlToolkit.dll.</span><span class="sxs-lookup"><span data-stu-id="40092-140">Toolbox Items - Delete the AJAX Control Toolkit tab and follow the steps above to re-create the tab with the new version of the AjaxControlToolkit.dll assembly.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="40092-141">[Précédent](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
> [Suivant](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)</span><span class="sxs-lookup"><span data-stu-id="40092-141">[Previous](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
[Next](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)</span></span>
