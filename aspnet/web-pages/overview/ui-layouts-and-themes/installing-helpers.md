---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Installation d’un Helper dans une application Web Pages (Razor) Site | Microsoft Docs
author: tfitzmac
description: Cet article décrit comment installer une application auxiliaire dans un site Web ASP.NET Web Pages (Razor). Une application d’assistance est un composant réutilisable qui inclut le code et la balise par...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 38290fd47355e7893eddd1f867f47b113b54ca7e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37361804"
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="48fb7-104">Installation d’un Helper dans un Site ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="48fb7-104">Installing a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="48fb7-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="48fb7-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="48fb7-106">Cet article décrit comment installer une application auxiliaire dans un site Web ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="48fb7-106">This article describes how to install a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="48fb7-107">Un *helper* est un composant réutilisable qui inclut le code et le balisage pour effectuer une tâche qui peut être fastidieux ou complexes.</span><span class="sxs-lookup"><span data-stu-id="48fb7-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="48fb7-108">Ce que vous allez apprendre :</span><span class="sxs-lookup"><span data-stu-id="48fb7-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="48fb7-109">Comment installer une application auxiliaire dans un site Web créé à l’aide de WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="48fb7-109">How to install a helper in a website created using WebMatrix 3.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="48fb7-110">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="48fb7-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="48fb7-111">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="48fb7-111">WebMatrix 3</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="48fb7-112">Vue d’ensemble des programmes d’assistance</span><span class="sxs-lookup"><span data-stu-id="48fb7-112">Overview of Helpers</span></span>

<span data-ttu-id="48fb7-113">Certaines tâches que les gens souhaitent souvent à faire sur les pages web ont besoin de beaucoup de code ou requièrent des connaissances supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="48fb7-113">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="48fb7-114">Exemples incluent l’affichage d’un graphique pour les données ; placer un bouton « Suivre » de Twitter sur une page ; envoi d’e-mails à partir de votre site Web ; rognage ou redimensionnement d’images ; à l’aide de PayPal pour votre site.</span><span class="sxs-lookup"><span data-stu-id="48fb7-114">Examples include displaying a chart for data; putting a Twitter "Follow" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="48fb7-115">Pour le rendre facile à réaliser ce types d’éléments différents, les Pages Web ASP.NET vous permet d’utiliser *helpers*.</span><span class="sxs-lookup"><span data-stu-id="48fb7-115">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="48fb7-116">Programmes d’assistance sont des composants que vous installez pour un site et qui permettent de vous effectuent les tâches standard en utilisant simplement une ou deux lignes de code Razor.</span><span class="sxs-lookup"><span data-stu-id="48fb7-116">Helpers are components that you install for a site and that let you perform typical tasks by using just a line or two of Razor code.</span></span>

<span data-ttu-id="48fb7-117">Les Pages Web ASP.NET a quelques helpers intégrés.</span><span class="sxs-lookup"><span data-stu-id="48fb7-117">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="48fb7-118">Toutefois, nombreux helpers sont disponibles dans les packages (compléments) qui sont fournies à l’aide du Gestionnaire de package NuGet.</span><span class="sxs-lookup"><span data-stu-id="48fb7-118">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="48fb7-119">NuGet vous permet de sélectionner un package à installer, et il s’occupe ensuite de tous les détails de l’installation.</span><span class="sxs-lookup"><span data-stu-id="48fb7-119">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

## <a name="installing-a-helper-in-webmatrix-3"></a><span data-ttu-id="48fb7-120">Installation d’un Helper dans WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="48fb7-120">Installing a Helper in WebMatrix 3</span></span>

1. <span data-ttu-id="48fb7-121">Dans WebMatrix 3, cliquez sur le **NuGet** bouton.</span><span class="sxs-lookup"><span data-stu-id="48fb7-121">In WebMatrix 3, click the **NuGet** button.</span></span>

    ![Boîte de dialogue galerie NuGet dans WebMatrix](installing-helpers/_static/image1.png)
2. <span data-ttu-id="48fb7-123">Cela lance le Gestionnaire de package NuGet et affiche les packages disponibles.</span><span class="sxs-lookup"><span data-stu-id="48fb7-123">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="48fb7-124">Dans la zone de recherche, entrez un mot clé pour l’application d’assistance que vous souhaitez installer.</span><span class="sxs-lookup"><span data-stu-id="48fb7-124">In the search box, enter a keyword for the helper you want to install.</span></span>

    ![Boîte de dialogue galerie NuGet dans WebMatrix](installing-helpers/_static/image2.png)
3. <span data-ttu-id="48fb7-126">Sélectionnez le package, puis cliquez **installer**.</span><span class="sxs-lookup"><span data-stu-id="48fb7-126">Select the package and then click **Install**.</span></span> <span data-ttu-id="48fb7-127">Cliquez sur **Oui** lorsque vous êtes invité à installer le package et d’indiquer que vous acceptez les termes du contrat.</span><span class="sxs-lookup"><span data-stu-id="48fb7-127">Click **Yes** when asked if you want to install the package and indicate that you accept the terms.</span></span>

     <span data-ttu-id="48fb7-128">S’il s’agit de la première fois que vous avez installé une application d’assistance, NuGet crée des dossiers dans votre site Web pour le code de l’application d’assistance.</span><span class="sxs-lookup"><span data-stu-id="48fb7-128">If this is the first time you've installed a helper, NuGet creates folders in your website for the code that makes up the helper.</span></span>
4. <span data-ttu-id="48fb7-129">Pour désinstaller un programme d’assistance, cliquez sur le **galerie** bouton, cliquez sur le **installé** onglet et sélectionnez le package que vous souhaitez désinstaller.</span><span class="sxs-lookup"><span data-stu-id="48fb7-129">To uninstall a helper, click the **Gallery** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="installing-the-twitter-helper"></a><span data-ttu-id="48fb7-130">L’installation du programme d’assistance de Twitter</span><span class="sxs-lookup"><span data-stu-id="48fb7-130">Installing the Twitter helper</span></span>

<span data-ttu-id="48fb7-131">La dernière version de l’API Twitter n’est pas compatible avec l’application d’assistance Twitter que vous installez via NuGet.</span><span class="sxs-lookup"><span data-stu-id="48fb7-131">The latest version of the Twitter API is not compatible with the Twitter helper you install through NuGet.</span></span> <span data-ttu-id="48fb7-132">Au lieu de cela, consultez le [application auxiliaire Twitter avec WebMatrix](twitter-helper.md) rubrique pour plus d’informations sur la configuration du programme d’assistance de Twitter dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="48fb7-132">Instead, see the [Twitter Helper with WebMatrix](twitter-helper.md) topic for information about how to set up the Twitter helper in your project.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="48fb7-133">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="48fb7-133">Additional Resources</span></span>


[<span data-ttu-id="48fb7-134">Présentation d’ASP.NET Web Pages 2 - notions de programmation</span><span class="sxs-lookup"><span data-stu-id="48fb7-134">Introducing ASP.NET Web Pages 2 - Programming Basics</span></span>](../getting-started/introducing-razor-syntax-c.md)

[<span data-ttu-id="48fb7-135">Helper Twitter avec WebMatrix</span><span class="sxs-lookup"><span data-stu-id="48fb7-135">Twitter Helper with WebMatrix</span></span>](twitter-helper.md)
