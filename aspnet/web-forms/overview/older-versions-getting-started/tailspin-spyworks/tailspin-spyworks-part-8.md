---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Partie 8 : Pages finales, gestion des exceptions et Conclusion | Microsoft Docs'
author: JoeStagner
description: Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks. Partie 8 ajoute une page de contact, sur la page et l’exception...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 3b49ee53e82933de9b50960779c28ca6ab7441e5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825507"
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a><span data-ttu-id="d4782-104">Partie 8 : Pages finales, gestion des exceptions et Conclusion</span><span class="sxs-lookup"><span data-stu-id="d4782-104">Part 8: Final Pages, Exception Handling, and Conclusion</span></span>
====================
<span data-ttu-id="d4782-105">par [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="d4782-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="d4782-106">Tailspin Spyworks montre comment extrêmement simple est de créer des applications puissantes et évolutives pour la plate-forme .NET.</span><span class="sxs-lookup"><span data-stu-id="d4782-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="d4782-107">Il montre comment utiliser les nouvelles fonctionnalités dans ASP.NET 4 pour créer un magasin en ligne, y compris les achats, extraction et administration.</span><span class="sxs-lookup"><span data-stu-id="d4782-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="d4782-108">Cette série de didacticiels décrit en détail les étapes prises pour générer l’exemple d’application Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="d4782-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="d4782-109">Partie 8 ajoute une page de contact, sur la page et la gestion des exceptions.</span><span class="sxs-lookup"><span data-stu-id="d4782-109">Part 8 adds a contact page, about page, and exception handling.</span></span> <span data-ttu-id="d4782-110">Il s’agit de la conclusion de la série.</span><span class="sxs-lookup"><span data-stu-id="d4782-110">This is the conclusion of the series.</span></span>


## <a id="_Toc260221680"></a>  <span data-ttu-id="d4782-111">Contactez Page (envoi de message électronique à partir d’ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="d4782-111">Contact Page (Sending email from ASP.NET)</span></span>

<span data-ttu-id="d4782-112">Créer une page nommée ContactUs.aspx</span><span class="sxs-lookup"><span data-stu-id="d4782-112">Create a new page named ContactUs.aspx</span></span>

<span data-ttu-id="d4782-113">À l’aide du concepteur, créez le formulaire suivant, en notant spécial pour inclure le ToolkitScriptManager et le contrôle d’édition à partir de la AjaxdControlToolkit.</span><span class="sxs-lookup"><span data-stu-id="d4782-113">Using the designer, create the following form taking special note to include the ToolkitScriptManager and the Editor control from the AjaxdControlToolkit.</span></span> <span data-ttu-id="d4782-114">.</span><span class="sxs-lookup"><span data-stu-id="d4782-114">.</span></span>

![](tailspin-spyworks-part-8/_static/image1.jpg)

<span data-ttu-id="d4782-115">Double-cliquez sur le bouton « Submit » pour générer un gestionnaire d’événements dans le fichier code-behind et implémenter une méthode pour envoyer les informations de contact sous forme d’e-mail.</span><span class="sxs-lookup"><span data-stu-id="d4782-115">Double click on the "Submit" button to generate a click event handler in the code behind file and implement a method to send the contact information as an email.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

<span data-ttu-id="d4782-116">Ce code nécessite que votre fichier web.config contient une entrée dans la section de configuration qui spécifie le serveur SMTP à utiliser pour l’envoi du message électronique.</span><span class="sxs-lookup"><span data-stu-id="d4782-116">This code requires that your web.config file contain an entry in the configuration section that specifies the SMTP server to use for sending mail.</span></span>

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>  <span data-ttu-id="d4782-117">Sur la Page</span><span class="sxs-lookup"><span data-stu-id="d4782-117">About Page</span></span>

<span data-ttu-id="d4782-118">Créez une page nommée AboutUs.aspx et ajoutez le contenu que vous le souhaitez.</span><span class="sxs-lookup"><span data-stu-id="d4782-118">Create a page named AboutUs.aspx and add whatever content you like.</span></span>

## <a id="_Toc260221682"></a>  <span data-ttu-id="d4782-119">Gestionnaire d’exceptions global</span><span class="sxs-lookup"><span data-stu-id="d4782-119">Global Exception Handler</span></span>

<span data-ttu-id="d4782-120">Enfin, tout au long de l’application nous avons levée des exceptions et des circonstances imprévues autrement à froid également cause non prise en charge des exceptions dans notre application web.</span><span class="sxs-lookup"><span data-stu-id="d4782-120">Lastly, throughout the application we have thrown exceptions and there are unforeseen circumstances that cold also cause unhandled exceptions in our web application.</span></span>

<span data-ttu-id="d4782-121">Nous voulons jamais une exception non gérée à afficher pour un visiteur du site web.</span><span class="sxs-lookup"><span data-stu-id="d4782-121">We never want an unhandled exception to be displayed to a web site visitor.</span></span>

![](tailspin-spyworks-part-8/_static/image2.jpg)

<span data-ttu-id="d4782-122">Outre une expérience utilisateur terrible les exceptions non gérées peuvent également être un problème de sécurité.</span><span class="sxs-lookup"><span data-stu-id="d4782-122">Apart from being a terrible user experience unhandled exceptions can also be a security problem.</span></span>

<span data-ttu-id="d4782-123">Pour résoudre ce problème, nous implémenterons un gestionnaire d’exceptions global.</span><span class="sxs-lookup"><span data-stu-id="d4782-123">To solve this problem we will implement a global exception handler.</span></span>

<span data-ttu-id="d4782-124">Pour ce faire, ouvrez le fichier Global.asax et notez le Gestionnaire d’événements prégénérées.</span><span class="sxs-lookup"><span data-stu-id="d4782-124">To do this, open the Global.asax file and note the following pre-generated event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

<span data-ttu-id="d4782-125">Ajoutez du code pour implémenter l’Application\_Gestionnaire d’erreurs comme suit.</span><span class="sxs-lookup"><span data-stu-id="d4782-125">Add code to implement the Application\_Error handler as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

<span data-ttu-id="d4782-126">Ensuite, ajoutez une page nommée Error.aspx à la solution et ajoutez cet extrait de balisage.</span><span class="sxs-lookup"><span data-stu-id="d4782-126">Then add a page named Error.aspx to the solution and add this markup snippet.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

<span data-ttu-id="d4782-127">Maintenant, dans la Page\_charger d’extraction de gestionnaire d’événements les messages d’erreur de l’objet Request.</span><span class="sxs-lookup"><span data-stu-id="d4782-127">Now in the Page\_Load event handler extract the error messages from the Request Object.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  <span data-ttu-id="d4782-128">Conclusion</span><span class="sxs-lookup"><span data-stu-id="d4782-128">Conclusion</span></span>

<span data-ttu-id="d4782-129">Nous avons vu que Web Forms ASP.NET facilite la création d’un site Web sophistiqué avec un accès de base de données, l’appartenance, AJAX, etc.</span><span class="sxs-lookup"><span data-stu-id="d4782-129">We've seen that ASP.NET WebForms makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="d4782-130">assez rapidement.</span><span class="sxs-lookup"><span data-stu-id="d4782-130">pretty quickly.</span></span>

<span data-ttu-id="d4782-131">J’espère que ce didacticiel vous a donné les outils que vous avez besoin pour commencer à créer votre propre WebForms ASP.NET applications !</span><span class="sxs-lookup"><span data-stu-id="d4782-131">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET WebForms applications!</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d4782-132">Précédent</span><span class="sxs-lookup"><span data-stu-id="d4782-132">Previous</span></span>](tailspin-spyworks-part-7.md)
