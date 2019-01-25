---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Tutoriel : Serveur de diffusion avec SignalR 2 | Microsoft Docs'
author: tdykstra
description: Ce didacticiel montre comment créer une application web qui utilise ASP.NET SignalR 2 pour fournir des fonctionnalités de diffusion de serveur.
ms.author: bradyg
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: a243c78c7d552f1c82a88c6083871fcd16538618
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837427"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a><span data-ttu-id="77c58-103">Tutoriel : Serveur de diffusion avec SignalR 2</span><span class="sxs-lookup"><span data-stu-id="77c58-103">Tutorial: Server broadcast with SignalR 2</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="77c58-104">Ce didacticiel montre comment créer une application web qui utilise ASP.NET SignalR 2 pour fournir des fonctionnalités de diffusion de serveur.</span><span class="sxs-lookup"><span data-stu-id="77c58-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span> <span data-ttu-id="77c58-105">Diffusion de serveur signifie que le serveur démarre les communications envoyées aux clients.</span><span class="sxs-lookup"><span data-stu-id="77c58-105">Server broadcast means that the server starts the communications sent to clients.</span></span>

<span data-ttu-id="77c58-106">L’application que vous allez créer dans ce didacticiel simule un téléscripteur, un scénario classique pour les fonctionnalités de diffusion de serveur.</span><span class="sxs-lookup"><span data-stu-id="77c58-106">The application that you'll create in this tutorial simulates a stock ticker, a typical scenario for server broadcast functionality.</span></span> <span data-ttu-id="77c58-107">Périodiquement, le serveur de façon aléatoire met à jour de bourse et diffuser les mises à jour pour tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="77c58-107">Periodically, the server randomly updates stock prices and broadcast the updates to all connected clients.</span></span> <span data-ttu-id="77c58-108">Dans le navigateur, des chiffres et des symboles dans le **modifier** et **%** colonnes changent dynamiquement en réponse aux notifications à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="77c58-108">In the browser, the numbers and symbols in the **Change** and **%** columns dynamically change in response to notifications from the server.</span></span> <span data-ttu-id="77c58-109">Si vous ouvrez des navigateurs supplémentaires à la même URL, elles affichent toutes les mêmes données et les mêmes modifications aux données simultanément.</span><span class="sxs-lookup"><span data-stu-id="77c58-109">If you open additional browsers to the same URL, they all show the same data and the same changes to the data simultaneously.</span></span>

![Créer le web](tutorial-server-broadcast-with-signalr/_static/image1.png)

<span data-ttu-id="77c58-111">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="77c58-111">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="77c58-112">Créer le projet</span><span class="sxs-lookup"><span data-stu-id="77c58-112">Create the project</span></span>
> * <span data-ttu-id="77c58-113">Configurer le code de serveur</span><span class="sxs-lookup"><span data-stu-id="77c58-113">Set up the server code</span></span>
> * <span data-ttu-id="77c58-114">Examinez le code de serveur</span><span class="sxs-lookup"><span data-stu-id="77c58-114">Examine the server code</span></span>
> * <span data-ttu-id="77c58-115">Définissez le code client</span><span class="sxs-lookup"><span data-stu-id="77c58-115">Set up the client code</span></span>
> * <span data-ttu-id="77c58-116">Examinez le code client</span><span class="sxs-lookup"><span data-stu-id="77c58-116">Examine the client code</span></span>
> * <span data-ttu-id="77c58-117">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="77c58-117">Test the application</span></span>
> * <span data-ttu-id="77c58-118">Activer la journalisation</span><span class="sxs-lookup"><span data-stu-id="77c58-118">Enable logging</span></span>

> [!IMPORTANT]
> <span data-ttu-id="77c58-119">Si vous ne souhaitez pas fonctionner à travers les étapes de génération de l’application, vous pouvez installer le package SignalR.Sample dans un nouveau projet d’Application Web ASP.NET vide.</span><span class="sxs-lookup"><span data-stu-id="77c58-119">If you don't want to work through the steps of building the application, you can install the SignalR.Sample package in a new Empty ASP.NET Web Application project.</span></span> <span data-ttu-id="77c58-120">Si vous installez le package NuGet sans effectuer les étapes décrites dans ce didacticiel, vous devez suivre les instructions fournies dans le *readme.txt* fichier.</span><span class="sxs-lookup"><span data-stu-id="77c58-120">If you install the NuGet package without performing the steps in this tutorial, you must follow the instructions in the *readme.txt* file.</span></span> <span data-ttu-id="77c58-121">Pour exécuter le package, vous devez ajouter un démarrage OWIN classe qui appelle le `ConfigureSignalR` méthode dans le package installé.</span><span class="sxs-lookup"><span data-stu-id="77c58-121">To run the package you need to add an OWIN startup class which calls the `ConfigureSignalR` method in the installed package.</span></span> <span data-ttu-id="77c58-122">Vous recevrez une erreur si vous n’ajoutez pas de la classe de démarrage OWIN.</span><span class="sxs-lookup"><span data-stu-id="77c58-122">You will receive an error if you do not add the OWIN startup class.</span></span> <span data-ttu-id="77c58-123">Consultez le [installer l’exemple StockTicker](#install-the-stockticker-sample) section de cet article.</span><span class="sxs-lookup"><span data-stu-id="77c58-123">See the [Install the StockTicker sample](#install-the-stockticker-sample) section of this article.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="77c58-124">Prérequis</span><span class="sxs-lookup"><span data-stu-id="77c58-124">Prerequisites</span></span>

 * <span data-ttu-id="77c58-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) avec la **ASP.NET et développement web** charge de travail.</span><span class="sxs-lookup"><span data-stu-id="77c58-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="77c58-126">Créer le projet</span><span class="sxs-lookup"><span data-stu-id="77c58-126">Create the project</span></span>

<span data-ttu-id="77c58-127">Cette section montre comment utiliser Visual Studio 2017 pour créer une Application de Web ASP.NET vide.</span><span class="sxs-lookup"><span data-stu-id="77c58-127">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application.</span></span>

1. <span data-ttu-id="77c58-128">Dans Visual Studio, créez une Application Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="77c58-128">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Créer le web](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. <span data-ttu-id="77c58-130">Dans le **nouvelle Application Web ASP.NET - SignalR.StockTicker** fenêtre, laissez le champ **vide** sélectionné et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="77c58-130">In the **New ASP.NET Web Application - SignalR.StockTicker** window, leave **Empty** selected and select **OK**.</span></span>

## <a name="set-up-the-server-code"></a><span data-ttu-id="77c58-131">Configurer le code de serveur</span><span class="sxs-lookup"><span data-stu-id="77c58-131">Set up the server code</span></span>

<span data-ttu-id="77c58-132">Dans cette section, vous définissez le code qui s’exécute sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="77c58-132">In this section, you set up the code that runs on the server.</span></span>

### <a name="create-the-stock-class"></a><span data-ttu-id="77c58-133">Créer la classe de Stock</span><span class="sxs-lookup"><span data-stu-id="77c58-133">Create the Stock class</span></span>

<span data-ttu-id="77c58-134">Vous commencez par créer le *Stock* classe que vous utiliserez pour stocker et transmettre des informations concernant une action de modèle.</span><span class="sxs-lookup"><span data-stu-id="77c58-134">You begin by creating the *Stock* model class that you'll use to store and transmit information about a stock.</span></span>

1. <span data-ttu-id="77c58-135">Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **classe**.</span><span class="sxs-lookup"><span data-stu-id="77c58-135">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="77c58-136">Nommez la classe *Stock* et ajoutez-le au projet.</span><span class="sxs-lookup"><span data-stu-id="77c58-136">Name the class *Stock* and add it to the project.</span></span>

1. <span data-ttu-id="77c58-137">Remplacez le code dans le *Stock.cs* fichier avec ce code :</span><span class="sxs-lookup"><span data-stu-id="77c58-137">Replace the code in the *Stock.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    <span data-ttu-id="77c58-138">Les deux propriétés que vous définirez lorsque vous créez des stocks sont `Symbol` (par exemple, MSFT pour Microsoft) et `Price`.</span><span class="sxs-lookup"><span data-stu-id="77c58-138">The two properties that you'll set when you create stocks are `Symbol` (for example, MSFT for Microsoft) and `Price`.</span></span> <span data-ttu-id="77c58-139">Les autres propriétés dépendent comment et quand vous définissez `Price`.</span><span class="sxs-lookup"><span data-stu-id="77c58-139">The other properties depend on how and when you set `Price`.</span></span> <span data-ttu-id="77c58-140">La première fois que vous définissez `Price`, la valeur transmise à `DayOpen`.</span><span class="sxs-lookup"><span data-stu-id="77c58-140">The first time you set `Price`, the value gets propagated to `DayOpen`.</span></span> <span data-ttu-id="77c58-141">Après cela, lorsque vous définissez `Price`, l’application calcule le `Change` et `PercentChange` les valeurs de propriété basées sur la différence entre `Price` et `DayOpen`.</span><span class="sxs-lookup"><span data-stu-id="77c58-141">After that, when you set `Price`, the app calculates the `Change` and `PercentChange` property values based on the difference between `Price` and `DayOpen`.</span></span>

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a><span data-ttu-id="77c58-142">Créer les classes StockTickerHub et StockTicker</span><span class="sxs-lookup"><span data-stu-id="77c58-142">Create the StockTickerHub and StockTicker classes</span></span>

<span data-ttu-id="77c58-143">Vous allez utiliser l’API de concentrateur SignalR pour gérer les interactions client-serveur.</span><span class="sxs-lookup"><span data-stu-id="77c58-143">You'll use the SignalR Hub API to handle server-to-client interaction.</span></span> <span data-ttu-id="77c58-144">Un `StockTickerHub` classe qui dérive de la `SignalRHub` classe gérera reçoit des connexions et des appels de méthode à partir de clients.</span><span class="sxs-lookup"><span data-stu-id="77c58-144">A `StockTickerHub` class that derives from the `SignalRHub` class will handle receiving connections and method calls from clients.</span></span> <span data-ttu-id="77c58-145">Vous devez également mettre à jour les données de stock et exécuter un `Timer` objet.</span><span class="sxs-lookup"><span data-stu-id="77c58-145">You also need to maintain stock data and run a `Timer` object.</span></span> <span data-ttu-id="77c58-146">Le `Timer` objet déclenche périodiquement des mises à jour de prix indépendants des connexions clientes.</span><span class="sxs-lookup"><span data-stu-id="77c58-146">The `Timer` object will periodically trigger price updates independent of client connections.</span></span> <span data-ttu-id="77c58-147">Vous ne pouvez pas mettre ces fonctions un `Hub` classe, étant donné que les concentrateurs sont temporaires.</span><span class="sxs-lookup"><span data-stu-id="77c58-147">You can't put these functions in a `Hub` class, because Hubs are transient.</span></span> <span data-ttu-id="77c58-148">L’application crée un `Hub` instance de classe pour chaque tâche sur le concentrateur, telles que les connexions et les appels à partir du client au serveur.</span><span class="sxs-lookup"><span data-stu-id="77c58-148">The app creates a `Hub` class instance for each task on the hub, like connections and calls from the client to the server.</span></span> <span data-ttu-id="77c58-149">Par conséquent, le mécanisme qui conserve les données de stock, les prix des mises à jour et diffuse les mises à jour de prix doit s’exécuter dans une classe distincte.</span><span class="sxs-lookup"><span data-stu-id="77c58-149">So the mechanism that keeps stock data, updates prices, and broadcasts the price updates has to run in a separate class.</span></span> <span data-ttu-id="77c58-150">Vous allez nommer la classe `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="77c58-150">You'll name the class `StockTicker`.</span></span>

![Diffusion à partir de StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

<span data-ttu-id="77c58-152">Vous souhaitez uniquement une seule instance de la `StockTicker` classe exécutés sur le serveur, vous devez définir une référence à partir de chaque `StockTickerHub` instance au singleton `StockTicker` instance.</span><span class="sxs-lookup"><span data-stu-id="77c58-152">You only want one instance of the `StockTicker` class to run on the server, so you'll need to set up a reference from each `StockTickerHub` instance to the singleton `StockTicker` instance.</span></span> <span data-ttu-id="77c58-153">Le `StockTicker` classe a diffuser aux clients, car il possède les données de stock et déclenche les mises à jour, mais `StockTicker` n’est pas un `Hub` classe.</span><span class="sxs-lookup"><span data-stu-id="77c58-153">The `StockTicker` class has to broadcast to clients because it has the stock data and triggers updates, but `StockTicker` isn't a `Hub` class.</span></span> <span data-ttu-id="77c58-154">Le `StockTicker` classe a obtenir une référence à l’objet de contexte de connexion de concentrateur SignalR.</span><span class="sxs-lookup"><span data-stu-id="77c58-154">The `StockTicker` class has to get a reference to the SignalR Hub connection context object.</span></span> <span data-ttu-id="77c58-155">Il peut ensuite utiliser l’objet de contexte de connexion SignalR pour diffuser aux clients.</span><span class="sxs-lookup"><span data-stu-id="77c58-155">It can then use the SignalR connection context object to broadcast to clients.</span></span>

#### <a name="create-stocktickerhubcs"></a><span data-ttu-id="77c58-156">Créer StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="77c58-156">Create StockTickerHub.cs</span></span>

1. <span data-ttu-id="77c58-157">Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="77c58-157">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="77c58-158">Dans **ajouter un nouvel élément - SignalR.StockTicker**, sélectionnez **installé** > **Visual C#**   >  **Web**  >  **SignalR** , puis sélectionnez **classe de concentrateur SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="77c58-158">In **Add New Item - SignalR.StockTicker**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="77c58-159">Nommez la classe *StockTickerHub* et ajoutez-le au projet.</span><span class="sxs-lookup"><span data-stu-id="77c58-159">Name the class *StockTickerHub* and add it to the project.</span></span>

    <span data-ttu-id="77c58-160">Cette étape crée la *StockTickerHub.cs* fichier de classe.</span><span class="sxs-lookup"><span data-stu-id="77c58-160">This step creates the *StockTickerHub.cs* class file.</span></span> <span data-ttu-id="77c58-161">Simultanément, il ajoute un ensemble de fichiers de script et les références d’assembly qui prend en charge SignalR au projet.</span><span class="sxs-lookup"><span data-stu-id="77c58-161">Simultaneously, it adds a set of script files and assembly references that supports SignalR to the project.</span></span>

1. <span data-ttu-id="77c58-162">Remplacez le code dans le *StockTickerHub.cs* fichier avec ce code :</span><span class="sxs-lookup"><span data-stu-id="77c58-162">Replace the code in the *StockTickerHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. <span data-ttu-id="77c58-163">Enregistrez le fichier.</span><span class="sxs-lookup"><span data-stu-id="77c58-163">Save the file.</span></span>

<span data-ttu-id="77c58-164">L’application utilise le [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) classe pour définir des méthodes, les clients peuvent appeler sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="77c58-164">The app uses the [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) class to define methods the clients can call on the server.</span></span> <span data-ttu-id="77c58-165">Vous définissez une méthode : `GetAllStocks()`.</span><span class="sxs-lookup"><span data-stu-id="77c58-165">You're defining one method: `GetAllStocks()`.</span></span> <span data-ttu-id="77c58-166">Lorsqu’un client se connecte initialement au serveur, il appellera cette méthode pour obtenir une liste de tous les stocks avec leurs prix.</span><span class="sxs-lookup"><span data-stu-id="77c58-166">When a client initially connects to the server, it will call this method to get a list of all of the stocks with their current prices.</span></span> <span data-ttu-id="77c58-167">La méthode peut exécuter de façon synchrone et renvoyer `IEnumerable<Stock>` , car elle retourne des données à partir de la mémoire.</span><span class="sxs-lookup"><span data-stu-id="77c58-167">The method can run synchronously and return `IEnumerable<Stock>` because it's returning data from memory.</span></span>

<span data-ttu-id="77c58-168">Si la méthode devait obtenir les données par une opération qui nécessiterait en attente, comme une recherche de base de données ou un appel de service web, vous devez spécifier `Task<IEnumerable<Stock>>` en tant que la valeur de retour pour permettre un traitement asynchrone.</span><span class="sxs-lookup"><span data-stu-id="77c58-168">If the method had to get the data by doing something that would involve waiting, like a database lookup or a web service call, you would specify `Task<IEnumerable<Stock>>` as the return value to enable asynchronous processing.</span></span> <span data-ttu-id="77c58-169">Pour plus d’informations, consultez [Guide de l’API ASP.NET SignalR Hubs - Server - quand exécuter de façon asynchrone](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span><span class="sxs-lookup"><span data-stu-id="77c58-169">For more information, see [ASP.NET SignalR Hubs API Guide - Server - When to execute asynchronously](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span></span>

<span data-ttu-id="77c58-170">Le `HubName` attribut spécifie la façon dont l’application référencera le Hub dans le code JavaScript sur le client.</span><span class="sxs-lookup"><span data-stu-id="77c58-170">The `HubName` attribute specifies how the app will reference the Hub in JavaScript code on the client.</span></span> <span data-ttu-id="77c58-171">Le nom par défaut sur le client si vous n’utilisez pas cet attribut, est une version de la casse mixte du nom de classe, qui dans ce cas serait `stockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="77c58-171">The default name on the client if you don't use this attribute, is a camelCase version of the class name, which in this case would be `stockTickerHub`.</span></span>

<span data-ttu-id="77c58-172">Comme vous le verrez plus tard lorsque vous créez le `StockTicker` (classe), l’application crée une instance de singleton de cette classe dans son statique `Instance` propriété.</span><span class="sxs-lookup"><span data-stu-id="77c58-172">As you'll see later when you create the `StockTicker` class, the app creates a singleton instance of that class in its static `Instance` property.</span></span> <span data-ttu-id="77c58-173">Cette instance singleton de `StockTicker` est en mémoire, quel que soit le nombre de clients connecter ou déconnecter.</span><span class="sxs-lookup"><span data-stu-id="77c58-173">That singleton instance of `StockTicker` is in memory no matter how many clients connect or disconnect.</span></span> <span data-ttu-id="77c58-174">Cette instance est ce que fait le `GetAllStocks()` méthode utilise pour retourner des informations de stock en cours.</span><span class="sxs-lookup"><span data-stu-id="77c58-174">That instance is what the `GetAllStocks()` method uses to return current stock information.</span></span>

#### <a name="create-stocktickercs"></a><span data-ttu-id="77c58-175">Créer StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="77c58-175">Create StockTicker.cs</span></span>

1. <span data-ttu-id="77c58-176">Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **classe**.</span><span class="sxs-lookup"><span data-stu-id="77c58-176">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="77c58-177">Nommez la classe *StockTicker* et ajoutez-le au projet.</span><span class="sxs-lookup"><span data-stu-id="77c58-177">Name the class *StockTicker* and add it to the project.</span></span>

1. <span data-ttu-id="77c58-178">Remplacez le code dans le *StockTicker.cs* fichier avec ce code :</span><span class="sxs-lookup"><span data-stu-id="77c58-178">Replace the code in the *StockTicker.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

<span data-ttu-id="77c58-179">Étant donné que tous les threads exécutera la même instance de StockTicker code, la classe StockTicker doit être thread-safe.</span><span class="sxs-lookup"><span data-stu-id="77c58-179">Since all threads will be running the same instance of StockTicker code, the StockTicker class has to be thread-safe.</span></span>

### <a name="examine-the-server-code"></a><span data-ttu-id="77c58-180">Examinez le code de serveur</span><span class="sxs-lookup"><span data-stu-id="77c58-180">Examine the server code</span></span>

<span data-ttu-id="77c58-181">Si vous examinez le code du serveur, il sera vous aider à comprendre le fonctionne de l’application.</span><span class="sxs-lookup"><span data-stu-id="77c58-181">If you examine the server code, it will help you understand how the app works.</span></span>

#### <a name="storing-the-singleton-instance-in-a-static-field"></a><span data-ttu-id="77c58-182">Stockage de l’instance de singleton dans un champ statique</span><span class="sxs-lookup"><span data-stu-id="77c58-182">Storing the singleton instance in a static field</span></span>

<span data-ttu-id="77c58-183">Le code initialise statiques `_instance` champ qui stocke le `Instance` propriété avec une instance de la classe.</span><span class="sxs-lookup"><span data-stu-id="77c58-183">The code initializes the static `_instance` field that backs the `Instance` property with an instance of the class.</span></span> <span data-ttu-id="77c58-184">Étant donné que le constructeur est privé, il est la seule instance de la classe que l’application peut créer.</span><span class="sxs-lookup"><span data-stu-id="77c58-184">Because the constructor is private, it's the only instance of the class that the app can create.</span></span> <span data-ttu-id="77c58-185">L’application utilise [l’initialisation tardive](/dotnet/framework/performance/lazy-initialization) pour le `_instance` champ.</span><span class="sxs-lookup"><span data-stu-id="77c58-185">The app uses [Lazy initialization](/dotnet/framework/performance/lazy-initialization) for the `_instance` field.</span></span> <span data-ttu-id="77c58-186">Il n’est pas pour des raisons de performances.</span><span class="sxs-lookup"><span data-stu-id="77c58-186">It's not for performance reasons.</span></span> <span data-ttu-id="77c58-187">Il est de s’assurer que la création de l’instance est thread-safe.</span><span class="sxs-lookup"><span data-stu-id="77c58-187">It's to make sure the instance creation is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

<span data-ttu-id="77c58-188">Chaque fois un client se connecte au serveur, une nouvelle instance de la classe StockTickerHub en cours d’exécution dans un thread distinct Obtient l’instance de singleton StockTicker à partir de la `StockTicker.Instance` propriété statique, comme vous l’avez vu précédemment dans la `StockTickerHub` classe.</span><span class="sxs-lookup"><span data-stu-id="77c58-188">Each time a client connects to the server, a new instance of the StockTickerHub class running in a separate thread gets the StockTicker singleton instance from the `StockTicker.Instance` static property, as you saw earlier in the `StockTickerHub` class.</span></span>

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a><span data-ttu-id="77c58-189">Stockage des données de stock dans un ConcurrentDictionary</span><span class="sxs-lookup"><span data-stu-id="77c58-189">Storing stock data in a ConcurrentDictionary</span></span>

<span data-ttu-id="77c58-190">Le constructeur initialise la `_stocks` collection avec des exemples de données boursières, et `GetAllStocks` retourne les stocks.</span><span class="sxs-lookup"><span data-stu-id="77c58-190">The constructor initializes the `_stocks` collection with some sample stock data, and `GetAllStocks` returns the stocks.</span></span> <span data-ttu-id="77c58-191">Comme vous l’avez vu précédemment, cette collection de stocks est retournée par `StockTickerHub.GetAllStocks`, qui est une méthode de serveur dans la `Hub` classe les clients peuvent appeler.</span><span class="sxs-lookup"><span data-stu-id="77c58-191">As you saw earlier, this collection of stocks is returned by `StockTickerHub.GetAllStocks`, which is a server method in the `Hub` class that clients can call.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

<span data-ttu-id="77c58-192">La collection de stocks est définie comme un [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) type cohérence de thread.</span><span class="sxs-lookup"><span data-stu-id="77c58-192">The stocks collection is defined as a [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) type for thread safety.</span></span> <span data-ttu-id="77c58-193">Comme alternative, vous pouvez utiliser un [dictionnaire](https://msdn.microsoft.com/library/xfhwa508.aspx) de l’objet et de verrouiller explicitement le dictionnaire lorsque vous apportez des modifications à celui-ci.</span><span class="sxs-lookup"><span data-stu-id="77c58-193">As an alternative, you could use a [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) object and explicitly lock the dictionary when you make changes to it.</span></span>

<span data-ttu-id="77c58-194">Pour cet exemple d’application, il est OK pour stocker les données d’application en mémoire et de perdre les données lors de l’application supprime le `StockTicker` instance.</span><span class="sxs-lookup"><span data-stu-id="77c58-194">For this sample application, it's OK to store application data in memory and to lose the data when the app disposes of the  `StockTicker` instance.</span></span> <span data-ttu-id="77c58-195">Dans une application réelle, vous fonctionnerait avec un magasin de données back-end comme une base de données.</span><span class="sxs-lookup"><span data-stu-id="77c58-195">In a real application, you would work with a back-end data store like a database.</span></span>

#### <a name="periodically-updating-stock-prices"></a><span data-ttu-id="77c58-196">Mise à jour périodique des actions</span><span class="sxs-lookup"><span data-stu-id="77c58-196">Periodically updating stock prices</span></span>

<span data-ttu-id="77c58-197">Le constructeur démarre un `Timer` objet qui appelle régulièrement des méthodes qui mettent à jour de bourse sur une base aléatoire.</span><span class="sxs-lookup"><span data-stu-id="77c58-197">The constructor starts up a `Timer` object that periodically calls methods that update stock prices on a random basis.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 <span data-ttu-id="77c58-198">`Timer` appels `UpdateStockPrices`, qui passe dans la valeur null dans le paramètre d’état.</span><span class="sxs-lookup"><span data-stu-id="77c58-198">`Timer` calls `UpdateStockPrices`, which passes in null in the state parameter.</span></span> <span data-ttu-id="77c58-199">Avant la mise à jour de prix, l’application prend un verrou sur le `_updateStockPricesLock` objet.</span><span class="sxs-lookup"><span data-stu-id="77c58-199">Before updating prices, the app takes a lock on the `_updateStockPricesLock` object.</span></span> <span data-ttu-id="77c58-200">Le code vérifie si un autre thread est déjà la mise à jour les prix, et il appelle ensuite `TryUpdateStockPrice` sur chaque action dans la liste.</span><span class="sxs-lookup"><span data-stu-id="77c58-200">The code checks if another thread is already updating prices, and then it calls `TryUpdateStockPrice` on each stock in the list.</span></span> <span data-ttu-id="77c58-201">Le `TryUpdateStockPrice` méthode décide s’il faut modifier la cotation et la quantité pour la modifier.</span><span class="sxs-lookup"><span data-stu-id="77c58-201">The `TryUpdateStockPrice` method decides whether to change the stock price, and how much to change it.</span></span> <span data-ttu-id="77c58-202">Si la cotation change, l’application appelle `BroadcastStockPrice` pour diffuser la modification de la cotation à tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="77c58-202">If the stock price changes, the app calls `BroadcastStockPrice` to broadcast the stock price change to all connected clients.</span></span>

<span data-ttu-id="77c58-203">Le `_updatingStockPrices` indicateur désigné [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) pour vous assurer qu’il est thread-safe.</span><span class="sxs-lookup"><span data-stu-id="77c58-203">The `_updatingStockPrices` flag designated [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) to make sure it is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

<span data-ttu-id="77c58-204">Dans une application réelle, le `TryUpdateStockPrice` méthode appelez un service web pour rechercher le prix.</span><span class="sxs-lookup"><span data-stu-id="77c58-204">In a real application, the `TryUpdateStockPrice` method would call a web service to look up the price.</span></span> <span data-ttu-id="77c58-205">Dans ce code, l’application utilise un générateur de nombres aléatoires pour apporter des modifications de façon aléatoire.</span><span class="sxs-lookup"><span data-stu-id="77c58-205">In this code, the app uses a random number generator to make changes randomly.</span></span>

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a><span data-ttu-id="77c58-206">Obtention du contexte de SignalR afin que la classe StockTicker peut diffuser vers les clients</span><span class="sxs-lookup"><span data-stu-id="77c58-206">Getting the SignalR context so that the StockTicker class can broadcast to clients</span></span>

<span data-ttu-id="77c58-207">Étant donné que les modifications de prix sont originaires ici le `StockTicker` de l’objet, c’est l’objet qui doit appeler une `updateStockPrice` (méthode) sur tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="77c58-207">Because the price changes originate here in the `StockTicker` object, it's the object that needs to call an `updateStockPrice` method on all connected clients.</span></span> <span data-ttu-id="77c58-208">Dans un `Hub` (classe), vous avez une API pour appeler les méthodes du client, mais `StockTicker` ne dérive pas de la `Hub` classe et n’a pas une référence à tout `Hub` objet.</span><span class="sxs-lookup"><span data-stu-id="77c58-208">In a `Hub` class, you have an API for calling client methods, but `StockTicker` doesn't derive from the `Hub` class and doesn't have a reference to any `Hub` object.</span></span> <span data-ttu-id="77c58-209">Diffusion vers les clients connectés, le `StockTicker` classe a obtenir l’instance de contexte de SignalR pour la `StockTickerHub` classe et l’utiliser pour appeler des méthodes sur les clients.</span><span class="sxs-lookup"><span data-stu-id="77c58-209">To broadcast to connected clients, the `StockTicker` class has to get the SignalR context instance for the `StockTickerHub` class and use that to call methods on clients.</span></span>

<span data-ttu-id="77c58-210">Le code obtient une référence au contexte de SignalR lorsqu’il crée l’instance de la classe singleton passes qui font référence au constructeur, puis le constructeur de la `Clients` propriété.</span><span class="sxs-lookup"><span data-stu-id="77c58-210">The code gets a reference to the SignalR context when it creates the singleton class instance, passes that reference to the constructor, and the constructor puts it in the `Clients` property.</span></span>

<span data-ttu-id="77c58-211">Il existe deux raisons raison pour laquelle vous souhaitez obtenir le contexte qu’une seule fois : obtention du contexte est une tâche coûteuse et le faire une fois garantit que l’application conserve l’ordre prévu des messages envoyés aux clients.</span><span class="sxs-lookup"><span data-stu-id="77c58-211">There are two reasons why you want to get the context only once: getting the context is an expensive task, and getting it once makes sure the app preserves the intended order of messages sent to the clients.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

<span data-ttu-id="77c58-212">Obtention de la `Clients` propriété du contexte et de sa mise en le `StockTickerClient` propriété vous permet d’écrire du code pour appeler des méthodes de client qui a le même aspect comme il le ferait dans un `Hub` classe.</span><span class="sxs-lookup"><span data-stu-id="77c58-212">Getting the `Clients` property of the context and putting it in the `StockTickerClient` property lets you write code to call client methods that looks the same as it would in a `Hub` class.</span></span> <span data-ttu-id="77c58-213">Par exemple, pour une diffusion à tous les clients, vous pouvez écrire `Clients.All.updateStockPrice(stock)`.</span><span class="sxs-lookup"><span data-stu-id="77c58-213">For instance, to broadcast to all clients you can write `Clients.All.updateStockPrice(stock)`.</span></span>

<span data-ttu-id="77c58-214">Le `updateStockPrice` méthode que vous appelez dans `BroadcastStockPrice` n’existe pas encore.</span><span class="sxs-lookup"><span data-stu-id="77c58-214">The `updateStockPrice` method that you're calling in `BroadcastStockPrice` doesn't exist yet.</span></span> <span data-ttu-id="77c58-215">Vous allez l’ajouter ultérieurement lorsque vous écrivez du code qui s’exécute sur le client.</span><span class="sxs-lookup"><span data-stu-id="77c58-215">You'll add it later when you write code that runs on the client.</span></span> <span data-ttu-id="77c58-216">Vous pouvez faire référence à `updateStockPrice` ici, car `Clients.All` est dynamique, ce qui signifie que l’application évalue l’expression lors de l’exécution.</span><span class="sxs-lookup"><span data-stu-id="77c58-216">You can refer to `updateStockPrice` here because `Clients.All` is dynamic, which means the app will evaluate the expression at runtime.</span></span> <span data-ttu-id="77c58-217">Lorsque cet appel de méthode s’exécute, SignalR envoie le nom de méthode et la valeur du paramètre du client, et si le client possède une méthode nommée `updateStockPrice`, l’application appelle cette méthode et lui passer la valeur du paramètre.</span><span class="sxs-lookup"><span data-stu-id="77c58-217">When this method call executes, SignalR will send the method name and the parameter value to the client, and if the client has a method named `updateStockPrice`, the app will call that method and pass the parameter value to it.</span></span>

<span data-ttu-id="77c58-218">`Clients.All` signifie envoyer à tous les clients.</span><span class="sxs-lookup"><span data-stu-id="77c58-218">`Clients.All` means send to all clients.</span></span> <span data-ttu-id="77c58-219">SignalR vous donne d’autres options pour spécifier quels clients ou les groupes de clients à envoyer à.</span><span class="sxs-lookup"><span data-stu-id="77c58-219">SignalR gives you other options to specify which clients or groups of clients to send to.</span></span> <span data-ttu-id="77c58-220">Pour plus d’informations, consultez [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="77c58-220">For more information, see [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span></span>

### <a name="register-the-signalr-route"></a><span data-ttu-id="77c58-221">Inscrire l’itinéraire de SignalR</span><span class="sxs-lookup"><span data-stu-id="77c58-221">Register the SignalR route</span></span>

<span data-ttu-id="77c58-222">Le serveur doit connaître l’URL à intercepter et diriger vers SignalR.</span><span class="sxs-lookup"><span data-stu-id="77c58-222">The server needs to know which URL to intercept and direct to SignalR.</span></span> <span data-ttu-id="77c58-223">Pour ce faire, ajoutez une classe de démarrage OWIN :</span><span class="sxs-lookup"><span data-stu-id="77c58-223">To do that, add an OWIN startup class:</span></span>

1. <span data-ttu-id="77c58-224">Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="77c58-224">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="77c58-225">Dans **ajouter un nouvel élément - SignalR.StockTicker** sélectionnez **installé** > **Visual C#**   >  **Web** et puis sélectionnez **classe de démarrage OWIN**.</span><span class="sxs-lookup"><span data-stu-id="77c58-225">In **Add New Item - SignalR.StockTicker** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="77c58-226">Nommez la classe *démarrage* et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="77c58-226">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="77c58-227">Remplacez le code par défaut dans le *Startup.cs* fichier avec ce code :</span><span class="sxs-lookup"><span data-stu-id="77c58-227">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

<span data-ttu-id="77c58-228">Vous avez terminé de paramétrer le code du serveur.</span><span class="sxs-lookup"><span data-stu-id="77c58-228">You have now finished setting up the server code.</span></span> <span data-ttu-id="77c58-229">Dans la section suivante, vous allez configurer le client.</span><span class="sxs-lookup"><span data-stu-id="77c58-229">In the next section, you'll set up the client.</span></span>

## <a name="set-up-the-client-code"></a><span data-ttu-id="77c58-230">Définissez le code client</span><span class="sxs-lookup"><span data-stu-id="77c58-230">Set up the client code</span></span>

<span data-ttu-id="77c58-231">Dans cette section, vous définissez le code qui s’exécute sur le client.</span><span class="sxs-lookup"><span data-stu-id="77c58-231">In this section, you set up the code that runs on the client.</span></span>

### <a name="create-the-html-page-and-javascript-file"></a><span data-ttu-id="77c58-232">Créer la page HTML et un fichier JavaScript</span><span class="sxs-lookup"><span data-stu-id="77c58-232">Create the HTML page and JavaScript file</span></span>

<span data-ttu-id="77c58-233">La page HTML affiche les données et le fichier JavaScript s’organiser les données.</span><span class="sxs-lookup"><span data-stu-id="77c58-233">The HTML page will display the data and the JavaScript file will organize the data.</span></span>

#### <a name="create-stocktickerhtml"></a><span data-ttu-id="77c58-234">Créer StockTicker.html</span><span class="sxs-lookup"><span data-stu-id="77c58-234">Create StockTicker.html</span></span>

<span data-ttu-id="77c58-235">Tout d’abord, vous allez ajouter le client HTML.</span><span class="sxs-lookup"><span data-stu-id="77c58-235">First, you'll add the HTML client.</span></span>

1. <span data-ttu-id="77c58-236">Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **HTML Page**.</span><span class="sxs-lookup"><span data-stu-id="77c58-236">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="77c58-237">Nommez le fichier *StockTicker* et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="77c58-237">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="77c58-238">Remplacez le code par défaut dans le *StockTicker.html* fichier avec ce code :</span><span class="sxs-lookup"><span data-stu-id="77c58-238">Replace the default code in the *StockTicker.html* file with this code:</span></span>

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    <span data-ttu-id="77c58-239">Le code HTML crée une table avec cinq colonnes, une ligne d’en-tête et une ligne de données avec une seule cellule qui s’étend sur toutes les cinq colonnes.</span><span class="sxs-lookup"><span data-stu-id="77c58-239">The HTML creates a table with five columns, a header row, and a data row with a single cell that spans all five columns.</span></span> <span data-ttu-id="77c58-240">La ligne de données affiche « chargement... » momentanément lorsque l’application démarre.</span><span class="sxs-lookup"><span data-stu-id="77c58-240">The data row shows "loading..." momentarily when the app starts.</span></span> <span data-ttu-id="77c58-241">Code JavaScript sera supprimer cette ligne et ajoutez dans ses lignes sur place avec des données boursières extraites du serveur.</span><span class="sxs-lookup"><span data-stu-id="77c58-241">JavaScript code will remove that row and add in its place rows with stock data retrieved from the server.</span></span>

    <span data-ttu-id="77c58-242">Les balises de script spécifient :</span><span class="sxs-lookup"><span data-stu-id="77c58-242">The script tags specify:</span></span>

    * <span data-ttu-id="77c58-243">Le fichier de script jQuery.</span><span class="sxs-lookup"><span data-stu-id="77c58-243">The jQuery script file.</span></span>

    * <span data-ttu-id="77c58-244">Le fichier de script SignalR core.</span><span class="sxs-lookup"><span data-stu-id="77c58-244">The SignalR core script file.</span></span>

    * <span data-ttu-id="77c58-245">Le fichier de script de proxys de SignalR.</span><span class="sxs-lookup"><span data-stu-id="77c58-245">The SignalR proxies script file.</span></span>

    * <span data-ttu-id="77c58-246">Un fichier de script StockTicker que vous créerez ultérieurement.</span><span class="sxs-lookup"><span data-stu-id="77c58-246">A StockTicker script file that you'll create later.</span></span>

    <span data-ttu-id="77c58-247">L’application génère dynamiquement le fichier de script de proxys de SignalR.</span><span class="sxs-lookup"><span data-stu-id="77c58-247">The app dynamically generates the SignalR proxies script file.</span></span> <span data-ttu-id="77c58-248">Il spécifie l’URL « / signalr hubs » et définit les méthodes de proxy pour les méthodes sur la classe de concentrateur, dans ce cas, pour `StockTickerHub.GetAllStocks`.</span><span class="sxs-lookup"><span data-stu-id="77c58-248">It specifies the "/signalr/hubs" URL and defines proxy methods for the methods on the Hub class, in this case, for `StockTickerHub.GetAllStocks`.</span></span> <span data-ttu-id="77c58-249">Si vous préférez, vous pouvez générer manuellement ce fichier JavaScript à l’aide de [SignalR utilitaires](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span><span class="sxs-lookup"><span data-stu-id="77c58-249">If you prefer, you can generate this JavaScript file manually by using [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span></span> <span data-ttu-id="77c58-250">N’oubliez pas de désactiver la création de fichier dynamique dans le `MapHubs` appel de méthode.</span><span class="sxs-lookup"><span data-stu-id="77c58-250">Don't forget to disable dynamic file creation in the `MapHubs` method call.</span></span>

1. <span data-ttu-id="77c58-251">Dans **l’Explorateur de solutions**, développez **Scripts**.</span><span class="sxs-lookup"><span data-stu-id="77c58-251">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="77c58-252">Bibliothèques de scripts pour jQuery et SignalR sont visibles dans le projet.</span><span class="sxs-lookup"><span data-stu-id="77c58-252">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="77c58-253">Le Gestionnaire de package installe une version ultérieure des scripts SignalR.</span><span class="sxs-lookup"><span data-stu-id="77c58-253">The package manager will install a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="77c58-254">Mettre à jour les références de script dans le bloc de code pour qu’elles correspondent aux versions des fichiers de script dans le projet.</span><span class="sxs-lookup"><span data-stu-id="77c58-254">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

1. <span data-ttu-id="77c58-255">Dans **l’Explorateur de solutions**, avec le bouton droit *StockTicker.html*, puis sélectionnez **définir comme Page de démarrage**.</span><span class="sxs-lookup"><span data-stu-id="77c58-255">In **Solution Explorer**, right-click *StockTicker.html*, and then select **Set as Start Page**.</span></span>

#### <a name="create-stocktickerjs"></a><span data-ttu-id="77c58-256">Créer StockTicker.js</span><span class="sxs-lookup"><span data-stu-id="77c58-256">Create StockTicker.js</span></span>

<span data-ttu-id="77c58-257">Créez maintenant le fichier JavaScript.</span><span class="sxs-lookup"><span data-stu-id="77c58-257">Now create the JavaScript file.</span></span>

1. <span data-ttu-id="77c58-258">Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **fichier JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="77c58-258">In **Solution Explorer**, right-click the project and select **Add** > **JavaScript File**.</span></span>

1. <span data-ttu-id="77c58-259">Nommez le fichier *StockTicker* et sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="77c58-259">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="77c58-260">Ajoutez ce code à la *StockTicker.js* fichier :</span><span class="sxs-lookup"><span data-stu-id="77c58-260">Add this code to the *StockTicker.js* file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a><span data-ttu-id="77c58-261">Examinez le code client</span><span class="sxs-lookup"><span data-stu-id="77c58-261">Examine the client code</span></span>

<span data-ttu-id="77c58-262">Si vous examinez le code client, il vous permettra de savoir comment le code client interagit avec le code du serveur pour que l’application fonctionne.</span><span class="sxs-lookup"><span data-stu-id="77c58-262">If you examine the client code, it will help you learn how the client code interacts with the server code to make the app work.</span></span>

#### <a name="starting-the-connection"></a><span data-ttu-id="77c58-263">Démarrage de la connexion</span><span class="sxs-lookup"><span data-stu-id="77c58-263">Starting the connection</span></span>

<span data-ttu-id="77c58-264">`$.connection` fait référence aux proxys SignalR.</span><span class="sxs-lookup"><span data-stu-id="77c58-264">`$.connection` refers to the SignalR proxies.</span></span> <span data-ttu-id="77c58-265">Le code obtient une référence au proxy pour le `StockTickerHub` classe et le place dans le `ticker` variable.</span><span class="sxs-lookup"><span data-stu-id="77c58-265">The code gets a reference to the proxy for the `StockTickerHub` class and puts it in the `ticker` variable.</span></span> <span data-ttu-id="77c58-266">Le nom de proxy est celui qui a été défini par le `HubName` attribut :</span><span class="sxs-lookup"><span data-stu-id="77c58-266">The proxy name is the name that was set by the `HubName` attribute:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

<span data-ttu-id="77c58-267">Après avoir défini toutes les variables et fonctions, la dernière ligne de code dans le fichier initialise la connexion SignalR en appelant SignalR `start` (fonction).</span><span class="sxs-lookup"><span data-stu-id="77c58-267">After you define all the variables and functions, the last line of code in the file initializes the SignalR connection by calling the SignalR `start` function.</span></span> <span data-ttu-id="77c58-268">Le `start` fonction exécute de façon asynchrone et retourne un [jQuery différé objet](http://api.jquery.com/category/deferred-object/).</span><span class="sxs-lookup"><span data-stu-id="77c58-268">The `start` function executes asynchronously and returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/).</span></span> <span data-ttu-id="77c58-269">Vous pouvez appeler la fonction terminée pour spécifier la fonction à appeler lorsque l’application termine l’action asynchrone.</span><span class="sxs-lookup"><span data-stu-id="77c58-269">You can call the done function to specify the function to call when the app finishes the asynchronous action.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a><span data-ttu-id="77c58-270">Obtention de tous les stocks</span><span class="sxs-lookup"><span data-stu-id="77c58-270">Getting all the stocks</span></span>

<span data-ttu-id="77c58-271">Le `init` appels de fonction le `getAllStocks` fonction sur le serveur et utilise les informations retournés par le serveur pour mettre à jour la table de stock.</span><span class="sxs-lookup"><span data-stu-id="77c58-271">The `init` function calls the `getAllStocks` function on the server and uses the information that the server returns to update the stock table.</span></span> <span data-ttu-id="77c58-272">Notez que, par défaut, vous devez utiliser camelCasing sur le client, même si le nom de la méthode est sur le serveur de la casse pascal.</span><span class="sxs-lookup"><span data-stu-id="77c58-272">Notice that, by default, you have to use camelCasing on the client even though the method name is pascal-cased on the server.</span></span> <span data-ttu-id="77c58-273">La règle de casse mixte s’applique uniquement aux méthodes, pas les objets.</span><span class="sxs-lookup"><span data-stu-id="77c58-273">The camelCasing rule only applies to methods, not objects.</span></span> <span data-ttu-id="77c58-274">Par exemple, vous faites référence à `stock.Symbol` et `stock.Price`, et non `stock.symbol` ou `stock.price`.</span><span class="sxs-lookup"><span data-stu-id="77c58-274">For example, you refer to `stock.Symbol` and `stock.Price`, not `stock.symbol` or `stock.price`.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

<span data-ttu-id="77c58-275">Dans le `init` (méthode), l’application crée le HTML pour une ligne de table pour chaque objet de stock reçu à partir du serveur en appelant `formatStock` aux propriétés de format de la `stock` de l’objet, puis en appelant `supplant` pour remplacer les espaces réservés dans le `rowTemplate` variable avec le `stock` les valeurs de propriété de l’objet.</span><span class="sxs-lookup"><span data-stu-id="77c58-275">In the `init` method, the app creates HTML for a table row for each stock object received from the server by calling `formatStock` to format properties of the `stock` object, and then by calling `supplant` to replace placeholders in the `rowTemplate` variable with the `stock` object property values.</span></span> <span data-ttu-id="77c58-276">Le code HTML résultant est ensuite ajouté à la table de stock.</span><span class="sxs-lookup"><span data-stu-id="77c58-276">The resulting HTML is then appended to the stock table.</span></span>

> [!NOTE]
> <span data-ttu-id="77c58-277">Vous appelez `init` en la passant comme un `callback` fonction qui s’exécute après asynchrone `start` fonction se termine.</span><span class="sxs-lookup"><span data-stu-id="77c58-277">You call `init` by passing it in as a `callback` function that executes after the asynchronous `start` function finishes.</span></span> <span data-ttu-id="77c58-278">Si vous avez appelé `init` comme une instruction JavaScript séparée après avoir appelé `start`, la fonction échoue, car il est exécuté immédiatement sans attendre que la fonction de démarrage à la fin de l’établissement de la connexion.</span><span class="sxs-lookup"><span data-stu-id="77c58-278">If you called `init` as a separate JavaScript statement after calling `start`, the function would fail because it would run immediately without waiting for the start function to finish establishing the connection.</span></span> <span data-ttu-id="77c58-279">Dans ce cas, le `init` fonction essaie d’appeler le `getAllStocks` fonctionner avant que l’application établit une connexion au serveur.</span><span class="sxs-lookup"><span data-stu-id="77c58-279">In that case, the `init` function would try to call the `getAllStocks` function before the app establishes a server connection.</span></span>

#### <a name="getting-updated-stock-prices"></a><span data-ttu-id="77c58-280">Obtention des mises à jour de bourse</span><span class="sxs-lookup"><span data-stu-id="77c58-280">Getting updated stock prices</span></span>

<span data-ttu-id="77c58-281">Lorsque le serveur modifie les prix d’une action, il appelle le `updateStockPrice` sur les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="77c58-281">When the server changes a stock's price, it calls the `updateStockPrice` on connected clients.</span></span> <span data-ttu-id="77c58-282">L’application ajoute la fonction à la propriété de client de la `stockTicker` proxy pour le rendre disponible aux appels à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="77c58-282">The app adds the function to the client property of the `stockTicker` proxy to make it available to calls from the server.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

<span data-ttu-id="77c58-283">Le `updateStockPrice` un objet stock reçu à partir du serveur dans une table de ligne de la même façon que dans des formats de fonction le `init` (fonction).</span><span class="sxs-lookup"><span data-stu-id="77c58-283">The `updateStockPrice` function formats a stock object received from the server into a table row the same way as in the `init` function.</span></span> <span data-ttu-id="77c58-284">Au lieu de l’ajout de la ligne à la table, il recherche la ligne actuelle de l’action dans la table et remplace cette ligne par le nouveau.</span><span class="sxs-lookup"><span data-stu-id="77c58-284">Instead of appending the row to the table, it finds the stock's current row in the table and replaces that row with the new one.</span></span>

## <a name="test-the-application"></a><span data-ttu-id="77c58-285">Tester l’application</span><span class="sxs-lookup"><span data-stu-id="77c58-285">Test the application</span></span>

<span data-ttu-id="77c58-286">Vous pouvez tester l’application pour vous assurer qu’elle fonctionne.</span><span class="sxs-lookup"><span data-stu-id="77c58-286">You can test the app to make sure it's working.</span></span> <span data-ttu-id="77c58-287">Vous verrez toutes les fenêtres de navigateur et afficher la table de stock en direct avec des actions fluctue.</span><span class="sxs-lookup"><span data-stu-id="77c58-287">You'll see all browser windows display the live stock table with stock prices fluctuating.</span></span>

1. <span data-ttu-id="77c58-288">Dans la barre d’outils, activez **le débogage de Script** , puis sélectionnez le bouton lecture pour exécuter l’application en mode débogage.</span><span class="sxs-lookup"><span data-stu-id="77c58-288">In the toolbar, turn on **Script Debugging** and then select the play button to run the app in Debug mode.</span></span>

    ![Capture d’écran de l’utilisateur sous tension en sélectionnant play et de débogage.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    <span data-ttu-id="77c58-290">Une fenêtre de navigateur s’ouvre avec le **Live la Table de Stock**.</span><span class="sxs-lookup"><span data-stu-id="77c58-290">A browser window will open displaying the **Live Stock Table**.</span></span> <span data-ttu-id="77c58-291">La table stock affiche initialement la ligne « chargement... », puis, après une courte période, l’application affiche les données de stock initiales et commencez à modifier la bourse.</span><span class="sxs-lookup"><span data-stu-id="77c58-291">The stock table initially shows the "loading..." line, then, after a short time, the app shows the initial stock data, and then the stock prices start to change.</span></span>

1. <span data-ttu-id="77c58-292">Copiez l’URL à partir du navigateur, ouvrez les deux autres navigateurs et collez l’URL dans la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="77c58-292">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

    <span data-ttu-id="77c58-293">L’affichage initial de stock est le même que le premier navigateur et les modifications se produisent simultanément.</span><span class="sxs-lookup"><span data-stu-id="77c58-293">The initial stock display is the same as the first browser and changes happen simultaneously.</span></span>

1. <span data-ttu-id="77c58-294">Fermez tous les navigateurs, ouvrez une nouvelle fenêtre de navigateur et accédez à la même URL.</span><span class="sxs-lookup"><span data-stu-id="77c58-294">Close all browsers, open a new browser, and go to the same URL.</span></span>

    <span data-ttu-id="77c58-295">L’objet de singleton StockTicker continué à fonctionner dans le serveur.</span><span class="sxs-lookup"><span data-stu-id="77c58-295">The StockTicker singleton object continued to run in the server.</span></span> <span data-ttu-id="77c58-296">Le **Live la Table de Stock** montre que les stocks ont continué à modifier.</span><span class="sxs-lookup"><span data-stu-id="77c58-296">The **Live Stock Table** shows that the stocks have continued to change.</span></span> <span data-ttu-id="77c58-297">Vous ne voyez pas la table initiale avec zéro modifier des chiffres.</span><span class="sxs-lookup"><span data-stu-id="77c58-297">You don't see the initial table with zero change figures.</span></span>

1. <span data-ttu-id="77c58-298">Fermez le navigateur.</span><span class="sxs-lookup"><span data-stu-id="77c58-298">Close the browser.</span></span>

## <a name="enable-logging"></a><span data-ttu-id="77c58-299">Activer la journalisation</span><span class="sxs-lookup"><span data-stu-id="77c58-299">Enable logging</span></span>

<span data-ttu-id="77c58-300">SignalR possède une fonction de journalisation intégrés que vous pouvez activer sur le client pour faciliter le dépannage.</span><span class="sxs-lookup"><span data-stu-id="77c58-300">SignalR has a built-in logging function that you can enable on the client to aid in troubleshooting.</span></span> <span data-ttu-id="77c58-301">Dans cette section, vous activez la journalisation et voir des exemples qui montrent comment journaux vous indiquer parmi les méthodes de transport suivants à l’aide de SignalR :</span><span class="sxs-lookup"><span data-stu-id="77c58-301">In this section, you enable logging and see examples that show how logs tell you which of the following transport methods SignalR is using:</span></span>

* <span data-ttu-id="77c58-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), pris en charge par IIS 8 et les navigateurs actuels.</span><span class="sxs-lookup"><span data-stu-id="77c58-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), supported by IIS 8 and current browsers.</span></span>

* <span data-ttu-id="77c58-303">[Événements de serveur envoyé](http://en.wikipedia.org/wiki/Server-sent_events), pris en charge par les navigateurs autres qu’Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="77c58-303">[Server-sent events](http://en.wikipedia.org/wiki/Server-sent_events), supported by browsers other than Internet Explorer.</span></span>

* <span data-ttu-id="77c58-304">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), pris en charge par Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="77c58-304">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supported by Internet Explorer.</span></span>

* <span data-ttu-id="77c58-305">[AJAX interrogation longue](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), pris en charge par tous les navigateurs.</span><span class="sxs-lookup"><span data-stu-id="77c58-305">[Ajax long polling](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supported by all browsers.</span></span>

<span data-ttu-id="77c58-306">Pour une connexion donnée, SignalR choisit la meilleure méthode de transport qui prennent en charge par le serveur et le client.</span><span class="sxs-lookup"><span data-stu-id="77c58-306">For any given connection, SignalR chooses the best transport method that both the server and the client support.</span></span>

1. <span data-ttu-id="77c58-307">Ouvrez *StockTicker.js*.</span><span class="sxs-lookup"><span data-stu-id="77c58-307">Open *StockTicker.js*.</span></span>

1. <span data-ttu-id="77c58-308">Ajoutez la ligne en surbrillance du code pour activer la journalisation immédiatement avant le code qui initialise la connexion à la fin du fichier :</span><span class="sxs-lookup"><span data-stu-id="77c58-308">Add this highlighted line of code to enable logging immediately before the code that initializes the connection at the end of the file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. <span data-ttu-id="77c58-309">Appuyez sur **F5** pour exécuter le projet.</span><span class="sxs-lookup"><span data-stu-id="77c58-309">Press **F5** to run the project.</span></span>

1. <span data-ttu-id="77c58-310">Ouvrez la fenêtre Outils de développement de votre navigateur, puis sélectionnez la Console pour afficher les journaux.</span><span class="sxs-lookup"><span data-stu-id="77c58-310">Open your browser's developer tools window, and select the Console to see the logs.</span></span> <span data-ttu-id="77c58-311">Vous devrez peut-être actualiser la page pour afficher les journaux de SignalR négocier la méthode de transport pour une nouvelle connexion.</span><span class="sxs-lookup"><span data-stu-id="77c58-311">You might have to refresh the page to see the logs of SignalR negotiating the transport method for a new connection.</span></span>

    * <span data-ttu-id="77c58-312">Si vous exécutez Internet Explorer 10 sur Windows 8 (IIS 8), la méthode de transport est **WebSockets**.</span><span class="sxs-lookup"><span data-stu-id="77c58-312">If you're running Internet Explorer 10 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

    * <span data-ttu-id="77c58-313">Si vous exécutez Internet Explorer 10 sur Windows 7 (IIS 7.5), la méthode de transport est **iframe**.</span><span class="sxs-lookup"><span data-stu-id="77c58-313">If you're running Internet Explorer 10 on Windows 7 (IIS 7.5), the transport method is **iframe**.</span></span>

    * <span data-ttu-id="77c58-314">Si vous utilisez Firefox 19 sur Windows 8 (IIS 8), la méthode de transport est **WebSockets**.</span><span class="sxs-lookup"><span data-stu-id="77c58-314">If you're running Firefox 19 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

        > [!TIP]
        > <span data-ttu-id="77c58-315">Dans Firefox, installez le complément Firebug pour obtenir une fenêtre de Console.</span><span class="sxs-lookup"><span data-stu-id="77c58-315">In Firefox, install the Firebug add-in to get a Console window.</span></span>

    * <span data-ttu-id="77c58-316">Si vous utilisez Firefox 19 sur Windows 7 (IIS 7.5), la méthode de transport est **envoyées au serveur** événements.</span><span class="sxs-lookup"><span data-stu-id="77c58-316">If you're running Firefox 19 on Windows 7 (IIS 7.5), the transport method is **server-sent** events.</span></span>

## <a name="install-the-stockticker-sample"></a><span data-ttu-id="77c58-317">Installer l’exemple StockTicker</span><span class="sxs-lookup"><span data-stu-id="77c58-317">Install the StockTicker sample</span></span>

<span data-ttu-id="77c58-318">Le [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installe l’application StockTicker.</span><span class="sxs-lookup"><span data-stu-id="77c58-318">The [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installs the StockTicker application.</span></span> <span data-ttu-id="77c58-319">Le package NuGet inclut plus de fonctionnalités que la version simplifiée que vous avez créé à partir de zéro.</span><span class="sxs-lookup"><span data-stu-id="77c58-319">The NuGet package includes more features than the simplified version that you created from scratch.</span></span> <span data-ttu-id="77c58-320">Dans cette section du didacticiel, vous installez le package NuGet et passez en revue les nouvelles fonctionnalités et le code qui implémente les.</span><span class="sxs-lookup"><span data-stu-id="77c58-320">In this section of the tutorial, you install the NuGet package and review the new features and the code that implements them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="77c58-321">Si vous installez le package sans effectuer les étapes précédentes de ce didacticiel, vous devez ajouter une classe de démarrage OWIN à votre projet.</span><span class="sxs-lookup"><span data-stu-id="77c58-321">If you install the package without performing the earlier steps of this tutorial, you must add an OWIN startup class to your project.</span></span> <span data-ttu-id="77c58-322">Ce fichier readme.txt pour le package NuGet explique cette étape.</span><span class="sxs-lookup"><span data-stu-id="77c58-322">This readme.txt file for the NuGet package explains this step.</span></span>

### <a name="install-the-signalrsample-nuget-package"></a><span data-ttu-id="77c58-323">Installez le package NuGet de SignalR.Sample</span><span class="sxs-lookup"><span data-stu-id="77c58-323">Install the SignalR.Sample NuGet package</span></span>

1. <span data-ttu-id="77c58-324">Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Gérer les packages NuGet**.</span><span class="sxs-lookup"><span data-stu-id="77c58-324">In **Solution Explorer**, right-click the project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="77c58-325">Dans **le Gestionnaire de NuGet Package : SignalR.StockTicker**, sélectionnez **Parcourir**.</span><span class="sxs-lookup"><span data-stu-id="77c58-325">In **NuGet Package manager: SignalR.StockTicker**, select **Browse**.</span></span>

1. <span data-ttu-id="77c58-326">À partir de **source du Package**, sélectionnez **nuget.org**.</span><span class="sxs-lookup"><span data-stu-id="77c58-326">From **Package source**, select **nuget.org**.</span></span>

1. <span data-ttu-id="77c58-327">Entrez *SignalR.Sample* dans la zone de recherche, puis sélectionnez **Microsoft.AspNet.SignalR.Sample** > **installer**.</span><span class="sxs-lookup"><span data-stu-id="77c58-327">Enter *SignalR.Sample* in the search box and select **Microsoft.AspNet.SignalR.Sample** > **Install**.</span></span>

1. <span data-ttu-id="77c58-328">Dans **l’Explorateur de solutions**, développez le *SignalR.Sample* dossier.</span><span class="sxs-lookup"><span data-stu-id="77c58-328">In **Solution Explorer**, expand the *SignalR.Sample* folder.</span></span>

    <span data-ttu-id="77c58-329">Installation du package SignalR.Sample créé le dossier et son contenu.</span><span class="sxs-lookup"><span data-stu-id="77c58-329">Installing the SignalR.Sample package created the folder and its contents.</span></span>

1. <span data-ttu-id="77c58-330">Dans le *SignalR.Sample* dossier, avec le bouton droit *StockTicker.html*, puis sélectionnez **définir comme Page de démarrage**.</span><span class="sxs-lookup"><span data-stu-id="77c58-330">In the *SignalR.Sample* folder, right-click *StockTicker.html*, and then select **Set As Start Page**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="77c58-331">Installation de NuGet de SignalR.Sample le package peut changer la version de jQuery dont vous disposez dans votre *Scripts* dossier.</span><span class="sxs-lookup"><span data-stu-id="77c58-331">Installing The SignalR.Sample NuGet package might change the version of jQuery that you have in your *Scripts* folder.</span></span> <span data-ttu-id="77c58-332">La nouvelle *StockTicker.html* fichier que le package installe dans le *SignalR.Sample* dossier sera synchronisée avec la version de jQuery que le package installe, mais si vous souhaitez exécuter votre original *StockTicker.html* fichier à nouveau, vous devrez peut-être mettre à jour la référence de jQuery dans la balise de script tout d’abord.</span><span class="sxs-lookup"><span data-stu-id="77c58-332">The new *StockTicker.html* file that the package installs in the *SignalR.Sample* folder will be in sync with the jQuery version that the package installs, but if you want to run your original *StockTicker.html* file again, you might have to update the jQuery reference in the script tag first.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="77c58-333">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="77c58-333">Run the application</span></span>

 <span data-ttu-id="77c58-334">La table que vous avez vu dans la première application comportait des fonctionnalités utiles.</span><span class="sxs-lookup"><span data-stu-id="77c58-334">The table that you saw in the first app had useful features.</span></span> <span data-ttu-id="77c58-335">L’application de cotations boursières complète montre les nouvelles fonctionnalités : une fenêtre de défilement horizontal qui affiche les données de stock et les stocks qui changent de couleur lorsqu’ils augmentent et se situent.</span><span class="sxs-lookup"><span data-stu-id="77c58-335">The full stock ticker application shows new features: a horizontally scrolling window that shows the stock data and stocks that change color as they rise and fall.</span></span>

1. <span data-ttu-id="77c58-336">Appuyez sur **F5** pour exécuter l'application.</span><span class="sxs-lookup"><span data-stu-id="77c58-336">Press **F5** to run the app.</span></span>

     <span data-ttu-id="77c58-337">Lorsque vous exécutez l’application pour la première fois, le « marché » est « fermé » et vous voyez un tableau statique et une fenêtre de code qui n’est pas le défilement.</span><span class="sxs-lookup"><span data-stu-id="77c58-337">When you run the app for the first time, the "market" is "closed" and you see a static table and a ticker window that isn't scrolling.</span></span>

1. <span data-ttu-id="77c58-338">Sélectionnez **Open Market**.</span><span class="sxs-lookup"><span data-stu-id="77c58-338">Select **Open Market**.</span></span>

    ![Capture d’écran du téléscripteur en direct.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * <span data-ttu-id="77c58-340">Le **Live le Téléscripteur** boîte commence à faire défiler horizontalement, et le serveur commence à diffuser régulièrement les modifications de prix de l’action de manière aléatoire.</span><span class="sxs-lookup"><span data-stu-id="77c58-340">The **Live Stock Ticker** box starts to scroll horizontally, and the server starts to periodically broadcast stock price changes on a random basis.</span></span>

    * <span data-ttu-id="77c58-341">Chaque fois qu’une cotation change, l’application met à jour à la fois le **Live la Table de Stock** et **téléscripteur de Live**.</span><span class="sxs-lookup"><span data-stu-id="77c58-341">Each time a stock price changes, the app updates both the **Live Stock Table** and the **Live Stock Ticker**.</span></span>

    * <span data-ttu-id="77c58-342">Lorsque le changement de prix d’une action est un nombre positif, l’application affiche le stock avec un arrière-plan vert.</span><span class="sxs-lookup"><span data-stu-id="77c58-342">When a stock's price change is positive, the app shows the stock with a green background.</span></span>

    * <span data-ttu-id="77c58-343">Lorsque la modification est négative, l’application affiche le stock avec un arrière-plan rouge.</span><span class="sxs-lookup"><span data-stu-id="77c58-343">When the change is negative, the app shows the stock with a red background.</span></span>

1. <span data-ttu-id="77c58-344">Sélectionnez **fermer marché**.</span><span class="sxs-lookup"><span data-stu-id="77c58-344">Select **Close Market**.</span></span>

    * <span data-ttu-id="77c58-345">Le tableau met à jour les mots vides.</span><span class="sxs-lookup"><span data-stu-id="77c58-345">The table updates stop.</span></span>

    * <span data-ttu-id="77c58-346">Le téléscripteur arrête le défilement.</span><span class="sxs-lookup"><span data-stu-id="77c58-346">The ticker stops scrolling.</span></span>

1. <span data-ttu-id="77c58-347">Sélectionnez **réinitialiser**.</span><span class="sxs-lookup"><span data-stu-id="77c58-347">Select **Reset**.</span></span>

    * <span data-ttu-id="77c58-348">Toutes les données de stock est réinitialisé.</span><span class="sxs-lookup"><span data-stu-id="77c58-348">All stock data is reset.</span></span>

    * <span data-ttu-id="77c58-349">L’application restaure l’état initial avant de démarrer les modifications de prix.</span><span class="sxs-lookup"><span data-stu-id="77c58-349">The app restores the initial state before price changes started.</span></span>

1. <span data-ttu-id="77c58-350">Copiez l’URL à partir du navigateur, ouvrez les deux autres navigateurs et collez l’URL dans la barre d’adresse.</span><span class="sxs-lookup"><span data-stu-id="77c58-350">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="77c58-351">Vous voyez les mêmes données mis à jour dynamiquement en même temps dans chaque navigateur.</span><span class="sxs-lookup"><span data-stu-id="77c58-351">You see the same data dynamically updated at the same time in each browser.</span></span>

1. <span data-ttu-id="77c58-352">Lorsque vous sélectionnez un des contrôles, tous les navigateurs répondent de la même manière en même temps.</span><span class="sxs-lookup"><span data-stu-id="77c58-352">When you select any of the controls, all browsers respond the same way at the same time.</span></span>

### <a name="live-stock-ticker-display"></a><span data-ttu-id="77c58-353">Affichage de téléscripteur dynamique</span><span class="sxs-lookup"><span data-stu-id="77c58-353">Live Stock Ticker display</span></span>

<span data-ttu-id="77c58-354">Le **téléscripteur de Live** affichage est une liste non triée dans un `<div>` élément mis en forme en une seule ligne par les styles CSS.</span><span class="sxs-lookup"><span data-stu-id="77c58-354">The **Live Stock Ticker** display is an unordered list in a `<div>` element formatted into a single line by CSS styles.</span></span> <span data-ttu-id="77c58-355">L’application initialise et met à jour le téléscripteur de la même façon que la table : en remplaçant les espaces réservés dans une `<li>` chaîne de modèle et ajouter dynamiquement le `<li>` éléments à la `<ul>` élément.</span><span class="sxs-lookup"><span data-stu-id="77c58-355">The app initializes and updates the ticker the same way as the table: by replacing placeholders in an `<li>` template string and dynamically adding the `<li>` elements to the `<ul>` element.</span></span> <span data-ttu-id="77c58-356">L’application inclut le défilement à l’aide de le jQuery `animate` (fonction) pour faire varier la marge gauche de la liste non triée dans le `<div>`.</span><span class="sxs-lookup"><span data-stu-id="77c58-356">The app includes  scrolling by using the jQuery `animate` function to vary the margin-left of the unordered list within the `<div>`.</span></span>

#### <a name="signalrsample-stocktickerhtml"></a><span data-ttu-id="77c58-357">SignalR.Sample StockTicker.html</span><span class="sxs-lookup"><span data-stu-id="77c58-357">SignalR.Sample StockTicker.html</span></span>

<span data-ttu-id="77c58-358">La code HTML de cotations boursières :</span><span class="sxs-lookup"><span data-stu-id="77c58-358">The stock ticker HTML code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a><span data-ttu-id="77c58-359">SignalR.Sample StockTicker.css</span><span class="sxs-lookup"><span data-stu-id="77c58-359">SignalR.Sample StockTicker.css</span></span>

<span data-ttu-id="77c58-360">La code CSS de cotations boursières :</span><span class="sxs-lookup"><span data-stu-id="77c58-360">The stock ticker CSS code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a><span data-ttu-id="77c58-361">SignalR.Sample SignalR.StockTicker.js</span><span class="sxs-lookup"><span data-stu-id="77c58-361">SignalR.Sample SignalR.StockTicker.js</span></span>

<span data-ttu-id="77c58-362">Le code jQuery qui permet de faire défiler :</span><span class="sxs-lookup"><span data-stu-id="77c58-362">The jQuery code that makes it scroll:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a><span data-ttu-id="77c58-363">Sur le serveur que le client peut appeler des méthodes supplémentaires</span><span class="sxs-lookup"><span data-stu-id="77c58-363">Additional methods on the server that the client can call</span></span>

<span data-ttu-id="77c58-364">Pour ajouter de la flexibilité à l’application, il existe de l’application peut appeler des méthodes supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="77c58-364">To add flexibility to the app, there are additional methods the app can call.</span></span>

#### <a name="signalrsample-stocktickerhubcs"></a><span data-ttu-id="77c58-365">SignalR.Sample StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="77c58-365">SignalR.Sample StockTickerHub.cs</span></span>

<span data-ttu-id="77c58-366">Le `StockTickerHub` classe définit quatre méthodes supplémentaires que le client peut appeler :</span><span class="sxs-lookup"><span data-stu-id="77c58-366">The `StockTickerHub` class defines four additional methods that the client can call:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

<span data-ttu-id="77c58-367">Les appels de l’application `OpenMarket`, `CloseMarket`, et `Reset` en réponse aux boutons en haut de la page.</span><span class="sxs-lookup"><span data-stu-id="77c58-367">The app calls `OpenMarket`, `CloseMarket`, and `Reset` in response to the buttons at the top of the page.</span></span> <span data-ttu-id="77c58-368">Elles illustrent le modèle d’un seul client déclenchant un changement d’état immédiatement propagée à tous les clients.</span><span class="sxs-lookup"><span data-stu-id="77c58-368">They demonstrate the pattern of one client triggering a change in state immediately propagated to all clients.</span></span> <span data-ttu-id="77c58-369">Chacune de ces méthodes appelle une méthode la `StockTicker` classe qui provoque le changement d’état sur le marché et diffuse ensuite le nouvel état.</span><span class="sxs-lookup"><span data-stu-id="77c58-369">Each of these methods calls a method in the `StockTicker` class that causes the market state change and then broadcasts the new state.</span></span>

#### <a name="signalrsample-stocktickercs"></a><span data-ttu-id="77c58-370">SignalR.Sample StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="77c58-370">SignalR.Sample StockTicker.cs</span></span>

<span data-ttu-id="77c58-371">Dans le `StockTicker` (classe), l’application gère l’état du marché avec un `MarketState` propriété qui retourne un `MarketState` valeur enum :</span><span class="sxs-lookup"><span data-stu-id="77c58-371">In the `StockTicker` class, the app maintains the state of the market with a `MarketState` property that returns a `MarketState` enum value:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

<span data-ttu-id="77c58-372">Chacune des méthodes qui modifient l’état de mise sur le marché le faire à l’intérieur d’un bloc de verrous, car la `StockTicker` classe doit être thread-safe :</span><span class="sxs-lookup"><span data-stu-id="77c58-372">Each of the methods that change the market state do so inside a lock block because the `StockTicker` class has to be thread-safe:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

<span data-ttu-id="77c58-373">S’assurer que ce code est thread-safe, le `_marketState` champ qui stocke le `MarketState` propriété désignée `volatile`:</span><span class="sxs-lookup"><span data-stu-id="77c58-373">To make sure this code is thread-safe, the `_marketState` field that backs the `MarketState` property designated `volatile`:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

<span data-ttu-id="77c58-374">Le `BroadcastMarketStateChange` et `BroadcastMarketReset` méthodes sont similaires à la méthode BroadcastStockPrice que vous avez déjà vu, à ceci près qu’ils appellent des méthodes différentes définies au niveau du client :</span><span class="sxs-lookup"><span data-stu-id="77c58-374">The `BroadcastMarketStateChange` and `BroadcastMarketReset` methods are similar to the BroadcastStockPrice method that you already saw, except they call different methods defined at the client:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a><span data-ttu-id="77c58-375">Sur le client que le serveur peut appeler des fonctions supplémentaires</span><span class="sxs-lookup"><span data-stu-id="77c58-375">Additional functions on the client that the server can call</span></span>

<span data-ttu-id="77c58-376">Le `updateStockPrice` fonction gère désormais le tableau et l’affichage de téléscripteur, et il utilise `jQuery.Color` pour flasher couleurs rouges et vertes.</span><span class="sxs-lookup"><span data-stu-id="77c58-376">The `updateStockPrice` function now handles both the table and the ticker display, and it uses `jQuery.Color` to flash red and green colors.</span></span>

<span data-ttu-id="77c58-377">Nouvelles fonctions dans *SignalR.StockTicker.js* activer et désactiver les boutons en fonction de l’état de mise sur le marché.</span><span class="sxs-lookup"><span data-stu-id="77c58-377">New functions in *SignalR.StockTicker.js* enable and disable the buttons based on market state.</span></span> <span data-ttu-id="77c58-378">Ils également arrêter ou démarrer le **Live le Téléscripteur** un défilement horizontal.</span><span class="sxs-lookup"><span data-stu-id="77c58-378">They also stop or start the **Live Stock Ticker** horizontal scrolling.</span></span> <span data-ttu-id="77c58-379">Dans la mesure où de nombreuses fonctions sont ajoutées à `ticker.client`, l’application utilise le [jQuery étendre la fonction](http://api.jquery.com/jQuery.extend/) pour les ajouter.</span><span class="sxs-lookup"><span data-stu-id="77c58-379">Since many functions are being added to `ticker.client`, the app uses the [jQuery extend function](http://api.jquery.com/jQuery.extend/) to add them.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a><span data-ttu-id="77c58-380">Programme d’installation de client supplémentaires après avoir établi la connexion</span><span class="sxs-lookup"><span data-stu-id="77c58-380">Additional client setup after establishing the connection</span></span>

<span data-ttu-id="77c58-381">Une fois que le client établit la connexion, il a un travail supplémentaire à effectuer :</span><span class="sxs-lookup"><span data-stu-id="77c58-381">After the client establishes the connection, it has some additional work to do:</span></span>

* <span data-ttu-id="77c58-382">Déterminer si le marché est ouvert ou fermé à appeler approprié `marketOpened` ou `marketClosed` (fonction).</span><span class="sxs-lookup"><span data-stu-id="77c58-382">Find out if the market is open or closed to call the appropriate `marketOpened` or `marketClosed` function.</span></span>

* <span data-ttu-id="77c58-383">Attachez les appels de méthode de serveur pour les boutons.</span><span class="sxs-lookup"><span data-stu-id="77c58-383">Attach the server method calls to the buttons.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

<span data-ttu-id="77c58-384">Les méthodes de serveur liés ne sont pas les boutons jusqu'à une fois que l’application établit la connexion.</span><span class="sxs-lookup"><span data-stu-id="77c58-384">The server methods aren't wired up to the buttons until after the app establishes the connection.</span></span> <span data-ttu-id="77c58-385">Il est donc le code ne peut pas appeler les méthodes de serveur avant qu’ils sont disponibles.</span><span class="sxs-lookup"><span data-stu-id="77c58-385">It's so the code can't call the server methods before they're available.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="77c58-386">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="77c58-386">Additional resources</span></span>

<span data-ttu-id="77c58-387">Dans ce didacticiel, vous avez appris comment programmer une application SignalR qui diffuse des messages à partir du serveur à tous les clients connectés.</span><span class="sxs-lookup"><span data-stu-id="77c58-387">In this tutorial you've learned how to program a SignalR application that broadcasts messages from the server to all connected clients.</span></span> <span data-ttu-id="77c58-388">Maintenant vous pouvez diffuser des messages de manière périodique et en réponse aux notifications à partir de n’importe quel client.</span><span class="sxs-lookup"><span data-stu-id="77c58-388">Now you can broadcast messages on a periodic basis and in response to notifications from any client.</span></span> <span data-ttu-id="77c58-389">Vous pouvez utiliser le concept de l’instance de singleton multithread pour maintenir l’état de serveur dans des scénarios multijoueurs de jeux en ligne.</span><span class="sxs-lookup"><span data-stu-id="77c58-389">You can use the concept of multi-threaded singleton instance to maintain server state in multi-player online game scenarios.</span></span> <span data-ttu-id="77c58-390">Pour obtenir un exemple, consultez [le jeu ShootR selon SignalR](https://github.com/NTaylorMullen/ShootR).</span><span class="sxs-lookup"><span data-stu-id="77c58-390">For an example, see [the ShootR game based on SignalR](https://github.com/NTaylorMullen/ShootR).</span></span>

<span data-ttu-id="77c58-391">Pour des didacticiels qui montrent des scénarios de communication d’égal à égal, consultez [bien démarrer avec SignalR](introduction-to-signalr.md) et [la mise à jour d’en temps réel avec SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="77c58-391">For tutorials that show peer-to-peer communication scenarios, see [Getting Started with SignalR](introduction-to-signalr.md) and [Real-Time Updating with SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span></span>

<span data-ttu-id="77c58-392">Pour en savoir plus sur SignalR, consultez les ressources suivantes :</span><span class="sxs-lookup"><span data-stu-id="77c58-392">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="77c58-393">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="77c58-393">ASP.NET SignalR</span></span>](../../index.md)
* [<span data-ttu-id="77c58-394">Projet de SignalR</span><span class="sxs-lookup"><span data-stu-id="77c58-394">SignalR Project</span></span>](http://signalr.net/)
* [<span data-ttu-id="77c58-395">SignalR GitHub et exemples</span><span class="sxs-lookup"><span data-stu-id="77c58-395">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)
* [<span data-ttu-id="77c58-396">Wiki de SignalR</span><span class="sxs-lookup"><span data-stu-id="77c58-396">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="77c58-397">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="77c58-397">Next steps</span></span>

<span data-ttu-id="77c58-398">Dans ce didacticiel, vous avez effectué les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="77c58-398">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="77c58-399">Créé le projet</span><span class="sxs-lookup"><span data-stu-id="77c58-399">Created the project</span></span>
> * <span data-ttu-id="77c58-400">Configurer le code de serveur</span><span class="sxs-lookup"><span data-stu-id="77c58-400">Set up the server code</span></span>
> * <span data-ttu-id="77c58-401">Examiner le code du serveur</span><span class="sxs-lookup"><span data-stu-id="77c58-401">Examined the server code</span></span>
> * <span data-ttu-id="77c58-402">Définissez le code client</span><span class="sxs-lookup"><span data-stu-id="77c58-402">Set up the client code</span></span>
> * <span data-ttu-id="77c58-403">Examiner le code client</span><span class="sxs-lookup"><span data-stu-id="77c58-403">Examined the client code</span></span>
> * <span data-ttu-id="77c58-404">Test de l’application</span><span class="sxs-lookup"><span data-stu-id="77c58-404">Tested the application</span></span>
> * <span data-ttu-id="77c58-405">Journalisation est activée</span><span class="sxs-lookup"><span data-stu-id="77c58-405">Enabled logging</span></span>

<span data-ttu-id="77c58-406">Passez à l’article suivant pour apprendre à créer une application web en temps réel qui utilise ASP.NET SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="77c58-406">Advance to the next article to learn how to create a real-time web application that uses ASP.NET SignalR 2.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="77c58-407">Créer l’application web en temps réel avec SignalR</span><span class="sxs-lookup"><span data-stu-id="77c58-407">Create real-time web app with SignalR</span></span>](real-time-web-applications-with-signalr.md)