---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: "L’activation de l’authentification Windows dans Katana | Documents Microsoft"
author: MikeWasson
description: "Cet article explique comment activer l’authentification Windows dans Katana. Elle couvre les deux scénarios : à l’aide d’IIS à l’hôte Katana et HttpListener auto-hébergement Kat..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 8a26d356f7abafba021199761f9a49dcb81765c5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
<a name="enabling-windows-authentication-in-katana"></a><span data-ttu-id="82e45-104">L’activation de l’authentification Windows dans Katana</span><span class="sxs-lookup"><span data-stu-id="82e45-104">Enabling Windows Authentication in Katana</span></span>
====================
<span data-ttu-id="82e45-105">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="82e45-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="82e45-106">Cet article explique comment activer l’authentification Windows dans Katana.</span><span class="sxs-lookup"><span data-stu-id="82e45-106">This article shows how to enable Windows Authentication in Katana.</span></span> <span data-ttu-id="82e45-107">Elle couvre les deux scénarios : à l’aide d’IIS à l’hôte Katana et HttpListener auto-hébergement Katana dans un processus personnalisé.</span><span class="sxs-lookup"><span data-stu-id="82e45-107">It covers two scenarios: Using IIS to host Katana, and using HttpListener to self-host Katana in a custom process.</span></span> <span data-ttu-id="82e45-108">Barry Dorrans, David Matson et Chris Ross Merci d’avoir relu cet article.</span><span class="sxs-lookup"><span data-stu-id="82e45-108">Thanks to Barry Dorrans, David Matson, and Chris Ross for reviewing this article.</span></span>


<span data-ttu-id="82e45-109">Katana est l’implémentation Microsoft de [OWIN](http://owin.org/), l’Interface Web ouverte pour .NET.</span><span class="sxs-lookup"><span data-stu-id="82e45-109">Katana is Microsoft's implementation of [OWIN](http://owin.org/), the Open Web Interface for .NET.</span></span> <span data-ttu-id="82e45-110">Vous trouverez une présentation de OWIN et Katana [ici](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="82e45-110">You can read an introduction to OWIN and Katana [here](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="82e45-111">L’architecture OWIN a plusieurs couches :</span><span class="sxs-lookup"><span data-stu-id="82e45-111">The OWIN architecture has several layers:</span></span>

- <span data-ttu-id="82e45-112">Hôte : Gère le processus dans lequel s’exécute le pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="82e45-112">Host: Manages the process in which the OWIN pipeline runs.</span></span>
- <span data-ttu-id="82e45-113">Serveur : Ouvre un socket réseau et écoute les demandes.</span><span class="sxs-lookup"><span data-stu-id="82e45-113">Server: Opens a network socket and listens for requests.</span></span>
- <span data-ttu-id="82e45-114">Intergiciel (middleware) : Traite la demande et réponse HTTP.</span><span class="sxs-lookup"><span data-stu-id="82e45-114">Middleware: Processes the HTTP request and response.</span></span>

<span data-ttu-id="82e45-115">Katana fournit actuellement les deux serveurs, qui prennent en charge l’authentification intégrée de Windows :</span><span class="sxs-lookup"><span data-stu-id="82e45-115">Katana currently provides two servers, both of which support Windows Integrated Authentication:</span></span>

- <span data-ttu-id="82e45-116">**Microsoft.Owin.Host.SystemWeb**.</span><span class="sxs-lookup"><span data-stu-id="82e45-116">**Microsoft.Owin.Host.SystemWeb**.</span></span> <span data-ttu-id="82e45-117">Utilise IIS avec le pipeline ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="82e45-117">Uses IIS with the ASP.NET pipeline.</span></span>
- <span data-ttu-id="82e45-118">**Microsoft.Owin.Host.HttpListener**.</span><span class="sxs-lookup"><span data-stu-id="82e45-118">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="82e45-119">Utilise [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span><span class="sxs-lookup"><span data-stu-id="82e45-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span></span> <span data-ttu-id="82e45-120">Ce serveur est actuellement l’option par défaut lors de l’hébergement des interconnexions.</span><span class="sxs-lookup"><span data-stu-id="82e45-120">This server is currently the default option when self-hosting Katana.</span></span>

> [!NOTE]
> <span data-ttu-id="82e45-121">Katana ne fournit pas actuellement d’intergiciel (middleware) OWIN pour l’authentification Windows, car cette fonctionnalité est déjà disponible dans les serveurs.</span><span class="sxs-lookup"><span data-stu-id="82e45-121">Katana does not currently provide OWIN middleware for Windows Authentication, because this functionality is already available in the servers.</span></span>


## <a name="windows-authentication-in-iis"></a><span data-ttu-id="82e45-122">Authentification Windows dans IIS</span><span class="sxs-lookup"><span data-stu-id="82e45-122">Windows Authentication in IIS</span></span>

<span data-ttu-id="82e45-123">Vous pouvez simplement à l’aide de Microsoft.Owin.Host.SystemWeb, pour activer l’authentification Windows dans IIS.</span><span class="sxs-lookup"><span data-stu-id="82e45-123">Using Microsoft.Owin.Host.SystemWeb, you can simply enable Windows Authentication in IIS.</span></span>

<span data-ttu-id="82e45-124">Commençons par la création d’une application ASP.NET à l’aide du modèle de projet « Application Web ASP.NET vide ».</span><span class="sxs-lookup"><span data-stu-id="82e45-124">Let's start by creating a new ASP.NET application, using the "ASP.NET Empty Web Application" project template.</span></span>

![](enabling-windows-authentication-in-katana/_static/image1.png)

<span data-ttu-id="82e45-125">Ensuite, ajoutez les packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="82e45-125">Next, add NuGet packages.</span></span> <span data-ttu-id="82e45-126">À partir de la **outils** menu, sélectionnez **Gestionnaire de Package de bibliothèque**, puis sélectionnez **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="82e45-126">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="82e45-127">Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="82e45-127">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

<span data-ttu-id="82e45-128">Ajoutez ensuite une classe nommée `Startup` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="82e45-128">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

<span data-ttu-id="82e45-129">Qui est que nécessaire pour créer une application « Hello world » pour OWIN, s’exécutant sur IIS.</span><span class="sxs-lookup"><span data-stu-id="82e45-129">That's all you need to create a "Hello world" application for OWIN, running on IIS.</span></span> <span data-ttu-id="82e45-130">Appuyez sur F5 pour déboguer l'application.</span><span class="sxs-lookup"><span data-stu-id="82e45-130">Press F5 to debug the application.</span></span> <span data-ttu-id="82e45-131">Vous devez voir « Hello World ! »</span><span class="sxs-lookup"><span data-stu-id="82e45-131">You should see "Hello World!"</span></span> <span data-ttu-id="82e45-132">dans la fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="82e45-132">in the browser window.</span></span>

![](enabling-windows-authentication-in-katana/_static/image2.png)

<span data-ttu-id="82e45-133">Ensuite, nous allons activer l’authentification Windows dans IIS Express.</span><span class="sxs-lookup"><span data-stu-id="82e45-133">Next, we'll enable Windows Authentication in IIS Express.</span></span> <span data-ttu-id="82e45-134">À partir de la **vue** menu, sélectionnez **propriétés**.</span><span class="sxs-lookup"><span data-stu-id="82e45-134">From the **View** menu, select **Properties**.</span></span> <span data-ttu-id="82e45-135">Cliquez sur le nom du projet dans l’Explorateur de solutions pour afficher les propriétés du projet.</span><span class="sxs-lookup"><span data-stu-id="82e45-135">Click on the project name in Solution Explorer to view the project properties.</span></span>

<span data-ttu-id="82e45-136">Dans le **propriétés** , configurez **l’authentification anonyme** à **désactivé** et **l’authentification Windows** à  **Activé**.</span><span class="sxs-lookup"><span data-stu-id="82e45-136">In the **Properties** window, set **Anonymous Authentication** to **Disabled** and set **Windows Authentication** to **Enabled**.</span></span>

![](enabling-windows-authentication-in-katana/_static/image3.png)

<span data-ttu-id="82e45-137">Lorsque vous exécutez l’application à partir de Visual Studio, IIS Express requiert les informations d’identification Windows.</span><span class="sxs-lookup"><span data-stu-id="82e45-137">When you run the application from Visual Studio, IIS Express will require the user's Windows credentials.</span></span> <span data-ttu-id="82e45-138">Vous pouvez le voir à l’aide de [Fiddler](http://fiddler2.com/home) ou HTTP un autre outil de débogage.</span><span class="sxs-lookup"><span data-stu-id="82e45-138">You can see this by using [Fiddler](http://fiddler2.com/home) or another HTTP debugging tool.</span></span> <span data-ttu-id="82e45-139">Voici un exemple de réponse HTTP :</span><span class="sxs-lookup"><span data-stu-id="82e45-139">Here is an example HTTP response:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

<span data-ttu-id="82e45-140">Les en-têtes WWW-Authenticate dans cette réponse indiquent que le serveur prend en charge la [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocole, qui utilise l’authentification Kerberos ou NTLM.</span><span class="sxs-lookup"><span data-stu-id="82e45-140">The WWW-Authenticate headers in this response indicate that the server supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocol, which uses either Kerberos or NTLM.</span></span>

<span data-ttu-id="82e45-141">Plus tard, lorsque vous déployez l’application sur un serveur, suivez [suit](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) pour activer l’authentification Windows dans IIS sur ce serveur.</span><span class="sxs-lookup"><span data-stu-id="82e45-141">Later, when you deploy the application to a server, follow [these steps](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) to enable Windows Authentication in IIS on that server.</span></span>

## <a name="windows-authentication-in-httplistener"></a><span data-ttu-id="82e45-142">Authentification Windows dans HttpListener</span><span class="sxs-lookup"><span data-stu-id="82e45-142">Windows Authentication in HttpListener</span></span>

<span data-ttu-id="82e45-143">Si vous utilisez Microsoft.Owin.Host.HttpListener pour Katana l’auto-hébergement, vous pouvez activer l’authentification Windows directement sur le **HttpListener** instance.</span><span class="sxs-lookup"><span data-stu-id="82e45-143">If you are using Microsoft.Owin.Host.HttpListener to self-host Katana, you can enable Windows Authentication directly on the **HttpListener** instance.</span></span>

<span data-ttu-id="82e45-144">Tout d’abord, créez une nouvelle application console.</span><span class="sxs-lookup"><span data-stu-id="82e45-144">First, create a new console application.</span></span> <span data-ttu-id="82e45-145">Ensuite, ajoutez les packages NuGet.</span><span class="sxs-lookup"><span data-stu-id="82e45-145">Next, add NuGet packages.</span></span> <span data-ttu-id="82e45-146">À partir de la **outils** menu, sélectionnez **Gestionnaire de Package de bibliothèque**, puis sélectionnez **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="82e45-146">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="82e45-147">Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="82e45-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

<span data-ttu-id="82e45-148">Ajoutez ensuite une classe nommée `Startup` avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="82e45-148">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

<span data-ttu-id="82e45-149">Cette classe implémente l’exemple « Hello world » avant, mais il définit également l’authentification Windows en tant que le schéma d’authentification.</span><span class="sxs-lookup"><span data-stu-id="82e45-149">This class implements the same "Hello world" example from before, but it also sets Windows Authentication as the authentication scheme.</span></span>

<span data-ttu-id="82e45-150">À l’intérieur de la `Main` de fonction, le pipeline OWIN de début :</span><span class="sxs-lookup"><span data-stu-id="82e45-150">Inside the `Main` function, start the OWIN pipeline:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

<span data-ttu-id="82e45-151">Vous pouvez envoyer une demande de Fiddler pour confirmer que l’application utilise l’authentification Windows :</span><span class="sxs-lookup"><span data-stu-id="82e45-151">You can send a request in Fiddler to confirm that the application is using Windows Authentication:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a><span data-ttu-id="82e45-152">Rubriques connexes</span><span class="sxs-lookup"><span data-stu-id="82e45-152">Related Topics</span></span>

[<span data-ttu-id="82e45-153">Vue d’ensemble du projet Katana</span><span class="sxs-lookup"><span data-stu-id="82e45-153">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)

[<span data-ttu-id="82e45-154">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="82e45-154">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[<span data-ttu-id="82e45-155">Présentation de l’authentification de formulaires OWIN dans MVC 5</span><span class="sxs-lookup"><span data-stu-id="82e45-155">Understanding OWIN Forms Authentication in MVC 5</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
