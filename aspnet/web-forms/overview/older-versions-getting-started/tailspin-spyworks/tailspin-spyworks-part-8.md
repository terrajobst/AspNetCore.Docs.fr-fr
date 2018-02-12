---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: "Partie 8 : Pages Final, la gestion des exceptions et Conclusion | Documents Microsoft"
author: JoeStagner
description: "Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application Tailspin Spyworks. Partie 8 ajoute une page de contact, sur la page et l’exception en cours..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: fce1a20f9d1093b6c60542d8a786ddf54fdc922c
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2018
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a><span data-ttu-id="9813b-104">Partie 8 : Pages Final, la gestion des exceptions et Conclusion</span><span class="sxs-lookup"><span data-stu-id="9813b-104">Part 8: Final Pages, Exception Handling, and Conclusion</span></span>
====================
<span data-ttu-id="9813b-105">par [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="9813b-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="9813b-106">Tailspin Spyworks montre comment extrêmement simple est de créer des applications puissantes et évolutives pour la plateforme .NET.</span><span class="sxs-lookup"><span data-stu-id="9813b-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="9813b-107">Il illustre comment utiliser les nouvelles fonctionnalités dans ASP.NET 4 pour créer un magasin en ligne, y compris les achats, l’extraction et l’administration.</span><span class="sxs-lookup"><span data-stu-id="9813b-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="9813b-108">Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="9813b-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="9813b-109">Partie 8 ajoute une page de contact, sur la page et la gestion des exceptions.</span><span class="sxs-lookup"><span data-stu-id="9813b-109">Part 8 adds a contact page, about page, and exception handling.</span></span> <span data-ttu-id="9813b-110">Il s’agit de la fin de la série.</span><span class="sxs-lookup"><span data-stu-id="9813b-110">This is the conclusion of the series.</span></span>


## <a id="_Toc260221680"></a><span data-ttu-id="9813b-111">Contactez Page (envoi par courrier électronique à partir d’ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="9813b-111">Contact Page (Sending email from ASP.NET)</span></span>

<span data-ttu-id="9813b-112">Créer une nouvelle page nommée ContactUs.aspx</span><span class="sxs-lookup"><span data-stu-id="9813b-112">Create a new page named ContactUs.aspx</span></span>

<span data-ttu-id="9813b-113">Le Concepteur de créer la forme suivante pris note spécial pour inclure le ToolkitScriptManager et le contrôle de l’éditeur à partir de la AjaxdControlToolkit.</span><span class="sxs-lookup"><span data-stu-id="9813b-113">Using the designer, create the following form taking special note to include the ToolkitScriptManager and the Editor control from the AjaxdControlToolkit.</span></span> <span data-ttu-id="9813b-114">.</span><span class="sxs-lookup"><span data-stu-id="9813b-114">.</span></span>

![](tailspin-spyworks-part-8/_static/image1.jpg)

<span data-ttu-id="9813b-115">Double-cliquez sur le bouton « Submit » pour générer un gestionnaire d’événements dans le fichier code-behind et implémentez une méthode pour envoyer les informations de contact en tant qu’un message électronique.</span><span class="sxs-lookup"><span data-stu-id="9813b-115">Double click on the "Submit" button to generate a click event handler in the code behind file and implement a method to send the contact information as an email.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

<span data-ttu-id="9813b-116">Ce code nécessite que votre fichier web.config contient une entrée dans la section de configuration qui spécifie le serveur SMTP à utiliser pour l’envoi du message électronique.</span><span class="sxs-lookup"><span data-stu-id="9813b-116">This code requires that your web.config file contain an entry in the configuration section that specifies the SMTP server to use for sending mail.</span></span>

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a><span data-ttu-id="9813b-117">Sur la Page</span><span class="sxs-lookup"><span data-stu-id="9813b-117">About Page</span></span>

<span data-ttu-id="9813b-118">Créer une page nommée AboutUs.aspx et ajoutez le contenu que vous le souhaitez.</span><span class="sxs-lookup"><span data-stu-id="9813b-118">Create a page named AboutUs.aspx and add whatever content you like.</span></span>

## <a id="_Toc260221682"></a><span data-ttu-id="9813b-119">Gestionnaire d’exceptions global</span><span class="sxs-lookup"><span data-stu-id="9813b-119">Global Exception Handler</span></span>

<span data-ttu-id="9813b-120">Enfin, tout au long de l’application nous avons levée des exceptions et il existe des circonstances imprévues autrement à froid également cause non gérée des exceptions dans l’application web.</span><span class="sxs-lookup"><span data-stu-id="9813b-120">Lastly, throughout the application we have thrown exceptions and there are unforeseen circumstances that cold also cause unhandled exceptions in our web application.</span></span>

<span data-ttu-id="9813b-121">Nous ne souhaitez jamais une exception non gérée à afficher pour le visiteur du site web.</span><span class="sxs-lookup"><span data-stu-id="9813b-121">We never want an unhandled exception to be displayed to a web site visitor.</span></span>

![](tailspin-spyworks-part-8/_static/image2.jpg)

<span data-ttu-id="9813b-122">Outre une expérience utilisateur horribles les exceptions non gérées peuvent également être un problème de sécurité.</span><span class="sxs-lookup"><span data-stu-id="9813b-122">Apart from being a terrible user experience unhandled exceptions can also be a security problem.</span></span>

<span data-ttu-id="9813b-123">Pour résoudre ce problème, nous implémenterons un gestionnaire d’exceptions global.</span><span class="sxs-lookup"><span data-stu-id="9813b-123">To solve this problem we will implement a global exception handler.</span></span>

<span data-ttu-id="9813b-124">Pour ce faire, ouvrez le fichier Global.asax et notez le Gestionnaire d’événements prégénéré suivant.</span><span class="sxs-lookup"><span data-stu-id="9813b-124">To do this, open the Global.asax file and note the following pre-generated event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

<span data-ttu-id="9813b-125">Ajoutez du code pour implémenter l’Application\_Gestionnaire d’erreurs comme suit.</span><span class="sxs-lookup"><span data-stu-id="9813b-125">Add code to implement the Application\_Error handler as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

<span data-ttu-id="9813b-126">Ensuite, ajoutez une page nommée Error.aspx à la solution et ajoutez cet extrait de balisage.</span><span class="sxs-lookup"><span data-stu-id="9813b-126">Then add a page named Error.aspx to the solution and add this markup snippet.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

<span data-ttu-id="9813b-127">Désormais, dans la Page\_charger extrait de gestionnaire d’événements les messages d’erreur de l’objet Request.</span><span class="sxs-lookup"><span data-stu-id="9813b-127">Now in the Page\_Load event handler extract the error messages from the Request Object.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  <span data-ttu-id="9813b-128">Conclusion</span><span class="sxs-lookup"><span data-stu-id="9813b-128">Conclusion</span></span>

<span data-ttu-id="9813b-129">Nous avons vu que ASP.NET WebForms facilite la création d’un site Web sophistiqué avec un accès de base de données, l’appartenance, AJAX, etc.</span><span class="sxs-lookup"><span data-stu-id="9813b-129">We've seen that ASP.NET WebForms makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="9813b-130">assez rapidement.</span><span class="sxs-lookup"><span data-stu-id="9813b-130">pretty quickly.</span></span>

<span data-ttu-id="9813b-131">Nous espérons que ce didacticiel vous a accordé les outils que vous avez besoin pour commencer à créer votre propre Web Forms ASP.NET applications !</span><span class="sxs-lookup"><span data-stu-id="9813b-131">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET WebForms applications!</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="9813b-132">Précédent</span><span class="sxs-lookup"><span data-stu-id="9813b-132">Previous</span></span>](tailspin-spyworks-part-7.md)
