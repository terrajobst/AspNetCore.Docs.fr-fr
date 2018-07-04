---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Créer une application de Client OData v4 (c#) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: 1ea6121db781c2d08bc8c76cd07be3c5a4f23f62
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377390"
---
<a name="create-an-odata-v4-client-app-c"></a><span data-ttu-id="c4584-102">Créer une application de Client OData v4 (c#)</span><span class="sxs-lookup"><span data-stu-id="c4584-102">Create an OData v4 Client App (C#)</span></span>
====================
<span data-ttu-id="c4584-103">par [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c4584-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c4584-104">Dans le didacticiel précédent, vous avez créé un service OData de base qui prend en charge les opérations CRUD.</span><span class="sxs-lookup"><span data-stu-id="c4584-104">In the previous tutorial, you created a basic OData service that supports CRUD operations.</span></span> <span data-ttu-id="c4584-105">Maintenant nous allons créer un client pour le service.</span><span class="sxs-lookup"><span data-stu-id="c4584-105">Now let's create a client for the service.</span></span>

<span data-ttu-id="c4584-106">Démarrez une nouvelle instance de Visual Studio et créez un nouveau projet d’application console.</span><span class="sxs-lookup"><span data-stu-id="c4584-106">Start a new instance of Visual Studio and create a new console application project.</span></span> <span data-ttu-id="c4584-107">Dans le **nouveau projet** boîte de dialogue, sélectionnez **installé** &gt; **modèles** &gt; **Visual C#** &gt; **Windows Desktop**, puis sélectionnez le **Application Console** modèle.</span><span class="sxs-lookup"><span data-stu-id="c4584-107">In the **New Project** dialog, select **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Windows Desktop**, and select the **Console Application** template.</span></span> <span data-ttu-id="c4584-108">Nommez le projet &quot;ProductsApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="c4584-108">Name the project &quot;ProductsApp&quot;.</span></span>

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="c4584-109">Vous pouvez également ajouter l’application console à la même solution Visual Studio qui contient le service OData.</span><span class="sxs-lookup"><span data-stu-id="c4584-109">You can also add the console app to the same Visual Studio solution that contains the OData service.</span></span>


## <a name="install-the-odata-client-code-generator"></a><span data-ttu-id="c4584-110">Installer le Générateur de Code Client OData</span><span class="sxs-lookup"><span data-stu-id="c4584-110">Install the OData Client Code Generator</span></span>

<span data-ttu-id="c4584-111">À partir de la **outils** menu, sélectionnez **Extensions et mises à jour**.</span><span class="sxs-lookup"><span data-stu-id="c4584-111">From the **Tools** menu, select **Extensions and Updates**.</span></span> <span data-ttu-id="c4584-112">Sélectionnez **Online** &gt; **galerie Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="c4584-112">Select **Online** &gt; **Visual Studio Gallery**.</span></span> <span data-ttu-id="c4584-113">Dans la zone de recherche, recherchez &quot;Générateur de Code Client OData&quot;.</span><span class="sxs-lookup"><span data-stu-id="c4584-113">In the search box, search for &quot;OData Client Code Generator&quot;.</span></span> <span data-ttu-id="c4584-114">Cliquez sur **télécharger** pour installer l’extension VSIX.</span><span class="sxs-lookup"><span data-stu-id="c4584-114">Click **Download** to install the VSIX.</span></span> <span data-ttu-id="c4584-115">Vous pouvez être invité à redémarrer Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c4584-115">You might be prompted to restart Visual Studio.</span></span>

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a><span data-ttu-id="c4584-116">Exécuter le Service OData local</span><span class="sxs-lookup"><span data-stu-id="c4584-116">Run the OData Service Locally</span></span>

<span data-ttu-id="c4584-117">Exécutez le projet ProductService à partir de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c4584-117">Run the ProductService project from Visual Studio.</span></span> <span data-ttu-id="c4584-118">Par défaut, Visual Studio lance un navigateur à la racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="c4584-118">By default, Visual Studio launches a browser to the application root.</span></span> <span data-ttu-id="c4584-119">Notez l’URI ; vous en aurez besoin à l’étape suivante.</span><span class="sxs-lookup"><span data-stu-id="c4584-119">Note the URI; you will need this in the next step.</span></span> <span data-ttu-id="c4584-120">Laissez l'application en cours d'exécution.</span><span class="sxs-lookup"><span data-stu-id="c4584-120">Leave the application running.</span></span>

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="c4584-121">Si vous placez les deux projets dans la même solution, veillez à exécuter le projet ProductService sans débogage.</span><span class="sxs-lookup"><span data-stu-id="c4584-121">If you put both projects in the same solution, make sure to run the ProductService project without debugging.</span></span> <span data-ttu-id="c4584-122">Dans l’étape suivante, vous devez maintenir le service en cours d’exécution pendant que vous modifiez le projet d’application console.</span><span class="sxs-lookup"><span data-stu-id="c4584-122">In the next step, you will need to keep the service running while you modify the console application project.</span></span>


## <a name="generate-the-service-proxy"></a><span data-ttu-id="c4584-123">Générer le Proxy de Service</span><span class="sxs-lookup"><span data-stu-id="c4584-123">Generate the Service Proxy</span></span>

<span data-ttu-id="c4584-124">Le proxy de service est une classe .NET qui définit des méthodes pour l’accès au service OData.</span><span class="sxs-lookup"><span data-stu-id="c4584-124">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="c4584-125">Le proxy se traduit par des appels de méthode dans les requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="c4584-125">The proxy translates method calls into HTTP requests.</span></span> <span data-ttu-id="c4584-126">Vous allez créer la classe proxy en exécutant un [modèle T4](https://msdn.microsoft.com/library/bb126445.aspx).</span><span class="sxs-lookup"><span data-stu-id="c4584-126">You will create the proxy class by running a [T4 template](https://msdn.microsoft.com/library/bb126445.aspx).</span></span>

<span data-ttu-id="c4584-127">Cliquez sur le projet.</span><span class="sxs-lookup"><span data-stu-id="c4584-127">Right-click the project.</span></span> <span data-ttu-id="c4584-128">Sélectionnez **ajouter** &gt; **un nouvel élément**.</span><span class="sxs-lookup"><span data-stu-id="c4584-128">Select **Add** &gt; **New Item**.</span></span>

![](create-an-odata-v4-client-app/_static/image5.png)

<span data-ttu-id="c4584-129">Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez **éléments Visual c#** &gt; **Code** &gt; **OData Client**.</span><span class="sxs-lookup"><span data-stu-id="c4584-129">In the **Add New Item** dialog, select **Visual C# Items** &gt; **Code** &gt; **OData Client**.</span></span> <span data-ttu-id="c4584-130">Nommez le modèle &quot;ProductClient.tt&quot;.</span><span class="sxs-lookup"><span data-stu-id="c4584-130">Name the template &quot;ProductClient.tt&quot;.</span></span> <span data-ttu-id="c4584-131">Cliquez sur **ajouter** et cliquez sur l’avertissement de sécurité.</span><span class="sxs-lookup"><span data-stu-id="c4584-131">Click **Add** and click through the security warning.</span></span>

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

<span data-ttu-id="c4584-132">À ce stade, vous obtiendrez une erreur, vous pouvez l’ignorer.</span><span class="sxs-lookup"><span data-stu-id="c4584-132">At this point, you'll get an error, which you can ignore.</span></span> <span data-ttu-id="c4584-133">Visual Studio exécute automatiquement le modèle, mais le modèle a besoin de certains paramètres de configuration premier.</span><span class="sxs-lookup"><span data-stu-id="c4584-133">Visual Studio automatically runs the template, but the template needs some configuration settings first.</span></span>

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

<span data-ttu-id="c4584-134">Ouvrez le fichier ProductClient.odata.config. Dans le `Parameter` élément, collez l’URI du projet ProductService (étape précédente).</span><span class="sxs-lookup"><span data-stu-id="c4584-134">Open the file ProductClient.odata.config. In the `Parameter` element, paste in the URI from the ProductService project (previous step).</span></span> <span data-ttu-id="c4584-135">Exemple :</span><span class="sxs-lookup"><span data-stu-id="c4584-135">For example:</span></span>

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

<span data-ttu-id="c4584-136">Exécuter à nouveau le modèle.</span><span class="sxs-lookup"><span data-stu-id="c4584-136">Run the template again.</span></span> <span data-ttu-id="c4584-137">Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le fichier ProductClient.tt et sélectionnez **exécuter un outil personnalisé**.</span><span class="sxs-lookup"><span data-stu-id="c4584-137">In Solution Explorer, right click the ProductClient.tt file and select **Run Custom Tool**.</span></span>

<span data-ttu-id="c4584-138">Le modèle crée un fichier de code nommé ProductClient.cs qui définit le proxy.</span><span class="sxs-lookup"><span data-stu-id="c4584-138">The template creates a code file named ProductClient.cs that defines the proxy.</span></span> <span data-ttu-id="c4584-139">Lorsque vous développez votre application, si vous modifiez le point de terminaison OData, exécuter le modèle pour mettre à jour le proxy.</span><span class="sxs-lookup"><span data-stu-id="c4584-139">As you develop your app, if you change the OData endpoint, run the template again to update the proxy.</span></span>

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a><span data-ttu-id="c4584-140">Utiliser le Proxy de Service pour appeler le Service OData</span><span class="sxs-lookup"><span data-stu-id="c4584-140">Use the Service Proxy to Call the OData Service</span></span>

<span data-ttu-id="c4584-141">Ouvrez le fichier Program.cs et remplacez le code réutilisable avec les éléments suivants.</span><span class="sxs-lookup"><span data-stu-id="c4584-141">Open the file Program.cs and replace the boilerplate code with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

<span data-ttu-id="c4584-142">Remplacez la valeur de *serviceUri* avec l’URI de service indiquée précédemment.</span><span class="sxs-lookup"><span data-stu-id="c4584-142">Replace the value of *serviceUri* with the service URI from earlier.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

<span data-ttu-id="c4584-143">Lorsque vous exécutez l’application, il doit générer ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="c4584-143">When you run the app, it should output the following:</span></span>

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
