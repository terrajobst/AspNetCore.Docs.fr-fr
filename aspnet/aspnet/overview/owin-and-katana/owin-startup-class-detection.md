---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Détection de classe de démarrage OWIN | Microsoft Docs
author: Praburaj
description: Ce didacticiel montre comment configurer la classe de démarrage OWIN est chargée. Pour plus d’informations sur OWIN, consultez une vue d’ensemble du projet Katana. Ce didacticiel a été...
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 591a04f429284ae73896807a6c2837ad498e8ae1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824280"
---
<a name="owin-startup-class-detection"></a><span data-ttu-id="f2b98-105">Détection de classe de démarrage OWIN</span><span class="sxs-lookup"><span data-stu-id="f2b98-105">OWIN Startup Class Detection</span></span>
====================
<span data-ttu-id="f2b98-106">par [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="f2b98-106">by [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="f2b98-107">Ce didacticiel montre comment configurer la classe de démarrage OWIN est chargée.</span><span class="sxs-lookup"><span data-stu-id="f2b98-107">This tutorial shows how to configure which OWIN startup class is loaded.</span></span> <span data-ttu-id="f2b98-108">Pour plus d’informations sur OWIN, consultez [une vue d’ensemble du projet Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="f2b98-108">For more information on OWIN, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="f2b98-109">Ce didacticiel a été rédigé par Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan et Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).</span><span class="sxs-lookup"><span data-stu-id="f2b98-109">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>
> 
> ## <a name="prerequisites"></a><span data-ttu-id="f2b98-110">Prérequis</span><span class="sxs-lookup"><span data-stu-id="f2b98-110">Prerequisites</span></span>
> 
> [<span data-ttu-id="f2b98-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f2b98-111">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a><span data-ttu-id="f2b98-112">Détection de classe de démarrage OWIN</span><span class="sxs-lookup"><span data-stu-id="f2b98-112">OWIN Startup Class Detection</span></span>

 <span data-ttu-id="f2b98-113">Chaque Application OWIN a une classe de démarrage où vous spécifiez des composants pour le pipeline d’application.</span><span class="sxs-lookup"><span data-stu-id="f2b98-113">Every OWIN Application has a startup class where you specify components for the application pipeline.</span></span> <span data-ttu-id="f2b98-114">Il existe différentes façons, vous pouvez vous connecter à votre classe de démarrage avec le runtime, selon le modèle d’hébergement que vous choisissez (OwinHost, IIS et IIS Express).</span><span class="sxs-lookup"><span data-stu-id="f2b98-114">There are different ways you can connect your startup class with the runtime, depending on the hosting model you choose (OwinHost, IIS, and IIS-Express).</span></span> <span data-ttu-id="f2b98-115">La classe de démarrage présentée dans ce didacticiel peut être utilisée dans chaque application d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="f2b98-115">The startup class shown in this tutorial can be used in every hosting application.</span></span> <span data-ttu-id="f2b98-116">Vous vous connectez à la classe de démarrage avec l’hébergement runtime à l’aide d’une de ces approches :</span><span class="sxs-lookup"><span data-stu-id="f2b98-116">You connect the startup class with the hosting runtime using one of the these approaches:</span></span>  

1. <span data-ttu-id="f2b98-117">**Convention de dénomination**: Katana recherche une classe nommée `Startup` dans l’espace de noms mise en correspondance le nom d’assembly ou de l’espace de noms global.</span><span class="sxs-lookup"><span data-stu-id="f2b98-117">**Naming Convention**: Katana looks for a class named `Startup` in namespace matching the assembly name or the global namespace.</span></span>
2. <span data-ttu-id="f2b98-118">**Attribut de OwinStartup**: c’est l’approche prendra la plupart des développeurs pour spécifier la classe de démarrage.</span><span class="sxs-lookup"><span data-stu-id="f2b98-118">**OwinStartup Attribute**: This is the approach most developers will take to specify the startup class.</span></span> <span data-ttu-id="f2b98-119">L’attribut suivant définit la classe de démarrage le `TestStartup` classe dans le `StartupDemo` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="f2b98-119">The following attribute will set the startup class to the `TestStartup` class in the `StartupDemo` namespace.</span></span> 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   <span data-ttu-id="f2b98-120">Le `OwinStartup` attribut remplace la convention d’affectation de noms.</span><span class="sxs-lookup"><span data-stu-id="f2b98-120">The `OwinStartup` attribute overrides the naming convention.</span></span> <span data-ttu-id="f2b98-121">Vous pouvez également spécifier un nom convivial avec cet attribut, toutefois, à l’aide d’un nom convivial, vous devez également utiliser le `appSetting` élément dans le fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="f2b98-121">You can also specify a friendly name with this attribute, however, using a friendly name requires you to also use the `appSetting` element in the configuration file.</span></span>
3. <span data-ttu-id="f2b98-122">**L’élément appSetting dans le fichier de Configuration**: le `appSetting` élément remplace la `OwinStartup` attribut et la convention d’affectation de noms.</span><span class="sxs-lookup"><span data-stu-id="f2b98-122">**The appSetting element in the Configuration file**: The `appSetting` element overrides the `OwinStartup` attribute and naming convention.</span></span> <span data-ttu-id="f2b98-123">Vous pouvez avoir plusieurs classes de démarrage (chacun utilisant une `OwinStartup` attribut) et configurer la classe de démarrage qui sera chargé dans un fichier de configuration à l’aide de balisage similaire à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="f2b98-123">You can have multiple startup classes (each using an `OwinStartup` attribute) and configure which startup class will be loaded in a configuration file using markup similar to the following:</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   <span data-ttu-id="f2b98-124">La clé suivante, qui spécifie explicitement l’assembly et la classe de démarrage peut également être utilisée :</span><span class="sxs-lookup"><span data-stu-id="f2b98-124">The following key, which explicitly specifies the startup class and assembly can also be used:</span></span> 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   <span data-ttu-id="f2b98-125">Le code XML suivant dans le fichier de configuration spécifie un nom de classe de démarrage convivial `ProductionConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="f2b98-125">The following XML in the configuration file specifies a friendly startup class name of `ProductionConfiguration`.</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   <span data-ttu-id="f2b98-126">Le balisage ci-dessus doit être utilisé par le code suivant `OwinStartup` attribut qui spécifie un nom convivial et provoque la `ProductionStartup2` classe à exécuter.</span><span class="sxs-lookup"><span data-stu-id="f2b98-126">The above markup must be used with the following `OwinStartup` attribute which specifies a friendly name and causes the `ProductionStartup2` class to run.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. <span data-ttu-id="f2b98-127">Pour désactiver la détection de démarrage OWIN ajouter la `appSetting owin:AutomaticAppStartup` avec la valeur `"false"` dans le fichier web.config.</span><span class="sxs-lookup"><span data-stu-id="f2b98-127">To disable OWIN startup discovery add the `appSetting owin:AutomaticAppStartup` with a value of `"false"` in the web.config file.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a><span data-ttu-id="f2b98-128">Créer une application de Web ASP.NET à l’aide de démarrage OWIN</span><span class="sxs-lookup"><span data-stu-id="f2b98-128">Create an ASP.NET Web App using OWIN Startup</span></span>

1. <span data-ttu-id="f2b98-129">Créez une application de web Asp.Net vide et nommez-la **StartupDemo**.</span><span class="sxs-lookup"><span data-stu-id="f2b98-129">Create an empty Asp.Net web application and name it **StartupDemo**.</span></span> <span data-ttu-id="f2b98-130">-Installez `Microsoft.Owin.Host.SystemWeb` à l’aide du Gestionnaire de package NuGet.</span><span class="sxs-lookup"><span data-stu-id="f2b98-130">- Install `Microsoft.Owin.Host.SystemWeb` using the NuGet package manager.</span></span> <span data-ttu-id="f2b98-131">À partir de la **outils** menu, sélectionnez **Library Package Manager**, puis **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="f2b98-131">From the **Tools** menu, select **Library Package Manager**, and then **Package Manager Console**.</span></span> <span data-ttu-id="f2b98-132">Entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="f2b98-132">Enter the following command:</span></span>  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. <span data-ttu-id="f2b98-133">Ajouter une classe de démarrage OWIN.</span><span class="sxs-lookup"><span data-stu-id="f2b98-133">Add an OWIN startup class.</span></span> <span data-ttu-id="f2b98-134">Dans Visual Studio 2013 avec le bouton droit sur le projet et sélectionnez **ajouter une classe**. - dans le **ajouter un nouvel élément** boîte de dialogue, entrez *OWIN* dans le champ de recherche, puis remplacez le nom par Startup.cs, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="f2b98-134">In Visual Studio 2013 right click the project and select **Add Class**.- In the **Add New Item** dialog box, enter *OWIN* in the search field, and change the name to Startup.cs, and then click **Add**.</span></span>  
  
     ![](owin-startup-class-detection/_static/image1.png)   
  
   <span data-ttu-id="f2b98-135">La prochaine fois que vous souhaitez ajouter un *classe de démarrage Owin*, il sera dans disponible à partir de la **ajouter** menu.</span><span class="sxs-lookup"><span data-stu-id="f2b98-135">The next time you want to add an *Owin Startup class*, it will be in available from the **Add** menu.</span></span>  
   
     ![](owin-startup-class-detection/_static/image2.png)  
  
   <span data-ttu-id="f2b98-136">Ou bien, vous pouvez cliquez avec le bouton droit sur le projet et sélectionnez **ajouter**, puis sélectionnez **un nouvel élément**, puis sélectionnez le **classe de démarrage Owin**.</span><span class="sxs-lookup"><span data-stu-id="f2b98-136">Alternatively, you can right click the project and select **Add**, then select **New Item**, and then select the **Owin Startup class**.</span></span>  
  
     ![](owin-startup-class-detection/_static/image3.png)  
  
- <span data-ttu-id="f2b98-137">Remplacez le code généré dans le *Startup.cs* fichier par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="f2b98-137">Replace the generated code in the *Startup.cs* file with the following:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
  <span data-ttu-id="f2b98-138">Le `app.Use` expression lambda est utilisée pour enregistrer le composant d’intergiciel (middleware) spécifié au pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="f2b98-138">The `app.Use` lambda expression is used to register the specified middleware component to the OWIN pipeline.</span></span> <span data-ttu-id="f2b98-139">Dans ce cas, nous configurons la journalisation des demandes entrantes avant de répondre à la demande entrante.</span><span class="sxs-lookup"><span data-stu-id="f2b98-139">In this case we are setting up logging of incoming requests before responding to the incoming request.</span></span> <span data-ttu-id="f2b98-140">Le `next` paramètre est le délégué ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [tâche](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) au composant suivant dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="f2b98-140">The `next` parameter is the delegate ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) to the next component in the pipeline.</span></span> <span data-ttu-id="f2b98-141">Le `app.Run` raccorde le pipeline aux demandes entrantes de l’expression lambda et fournit le mécanisme de réponse.</span><span class="sxs-lookup"><span data-stu-id="f2b98-141">The `app.Run` lambda expression hooks up the pipeline to incoming requests and provides the response mechanism.</span></span>
     > [!NOTE]
     > <span data-ttu-id="f2b98-142">Dans le code ci-dessus nous avons mis en commentaire le `OwinStartup` attribut et nous allons s’appuyer sur la convention de l’exécution de la classe nommée `Startup` .-Press ***F5*** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="f2b98-142">In the code above we have commented out the `OwinStartup` attribute and we're relying on the convention of running the class named `Startup` .- Press ***F5*** to run the application.</span></span> <span data-ttu-id="f2b98-143">Cliquez sur Actualiser plusieurs fois.</span><span class="sxs-lookup"><span data-stu-id="f2b98-143">Hit refresh a few times.</span></span>  
  
    ![](owin-startup-class-detection/_static/image4.png)  
  <span data-ttu-id="f2b98-144">Remarque : Le nombre indiqué dans les images dans ce didacticiel pas correspond le numéro que vous voyez.</span><span class="sxs-lookup"><span data-stu-id="f2b98-144">Note: The number shown in the images in this tutorial will not match the number you see.</span></span> <span data-ttu-id="f2b98-145">La chaîne de milliseconde est utilisée pour afficher une nouvelle réponse lorsque vous actualisez la page.</span><span class="sxs-lookup"><span data-stu-id="f2b98-145">The millisecond string is used to show a new response when you refresh the page.</span></span>  
  <span data-ttu-id="f2b98-146">Vous pouvez voir les informations de trace dans le **sortie** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="f2b98-146">You can see the trace information in the **Output** window.</span></span>  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a><span data-ttu-id="f2b98-147">Ajouter plusieurs Classes de démarrage</span><span class="sxs-lookup"><span data-stu-id="f2b98-147">Add More Startup Classes</span></span>

<span data-ttu-id="f2b98-148">Dans cette section, nous allons ajouter une autre classe de démarrage.</span><span class="sxs-lookup"><span data-stu-id="f2b98-148">In this section we'll add another Startup class.</span></span> <span data-ttu-id="f2b98-149">Vous pouvez ajouter plusieurs classe de démarrage OWIN à votre application.</span><span class="sxs-lookup"><span data-stu-id="f2b98-149">You can add multiple OWIN startup class to your application.</span></span> <span data-ttu-id="f2b98-150">Par exemple, vous souhaiterez peut-être créer des classes de démarrage pour le développement, test et production.</span><span class="sxs-lookup"><span data-stu-id="f2b98-150">For example, you might want to create startup classes for development, testing and production.</span></span>

1. <span data-ttu-id="f2b98-151">Créez une nouvelle classe de démarrage OWIN et nommez-le `ProductionStartup`.</span><span class="sxs-lookup"><span data-stu-id="f2b98-151">Create a new OWIN Startup class and name it `ProductionStartup`.</span></span>
2. <span data-ttu-id="f2b98-152">Remplacez le code généré par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="f2b98-152">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. <span data-ttu-id="f2b98-153">Appuyez sur F5 de contrôle pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="f2b98-153">Press Control F5 to run the app.</span></span> <span data-ttu-id="f2b98-154">Le `OwinStartup` attribut spécifie la classe de démarrage de production est exécutée.</span><span class="sxs-lookup"><span data-stu-id="f2b98-154">The `OwinStartup` attribute specifies the production startup class is run.</span></span>  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. <span data-ttu-id="f2b98-155">Créez une autre classe de démarrage OWIN et nommez-le `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="f2b98-155">Create another OWIN Startup class and name it `TestStartup`.</span></span>
5. <span data-ttu-id="f2b98-156">Remplacez le code généré par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="f2b98-156">Replace the generated code with the following:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   <span data-ttu-id="f2b98-157">Le `OwinStartup` surcharge attribut ci-dessus spécifie `TestingConfiguration` en tant que le *convivial* nom de la classe de démarrage.</span><span class="sxs-lookup"><span data-stu-id="f2b98-157">The `OwinStartup` attribute overload above specifies `TestingConfiguration` as the *friendly* name of the Startup class.</span></span>
6. <span data-ttu-id="f2b98-158">Ouvrez le *web.config* fichier, puis ajoutez la clé de démarrage OWIN application qui spécifie le nom convivial de la classe de démarrage :</span><span class="sxs-lookup"><span data-stu-id="f2b98-158">Open the *web.config* file and add the OWIN App startup key which specifies the friendly name of the Startup class:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. <span data-ttu-id="f2b98-159">Appuyez sur F5 de contrôle pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="f2b98-159">Press Control F5 to run the app.</span></span> <span data-ttu-id="f2b98-160">L’élément de paramètres d’application est prioritaire et le test de configuration est exécutée.</span><span class="sxs-lookup"><span data-stu-id="f2b98-160">The app settings element takes precedent, and the test configuration is run.</span></span>  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. <span data-ttu-id="f2b98-161">Supprimer le *convivial* nom de la `OwinStartup` d’attribut dans la `TestStartup` classe.</span><span class="sxs-lookup"><span data-stu-id="f2b98-161">Remove the *friendly* name from the `OwinStartup` attribute in the `TestStartup` class.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. <span data-ttu-id="f2b98-162">Remplacer la clé de démarrage OWIN application dans le *web.config* fichier par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="f2b98-162">Replace the OWIN App startup key in the *web.config* file with the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. <span data-ttu-id="f2b98-163">Rétablir le `OwinStartup` attribut dans chaque classe pour le code d’attribut par défaut généré par Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="f2b98-163">Revert the `OwinStartup` attribute in each class to the default attribute code generated by Visual Studio:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    <span data-ttu-id="f2b98-164">Chacune des clés de démarrage OWIN application ci-dessous entraînera la classe de production s’exécute.</span><span class="sxs-lookup"><span data-stu-id="f2b98-164">Each of the OWIN App startup keys below will cause the production class to run.</span></span> 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    <span data-ttu-id="f2b98-165">La dernière clé de démarrage spécifie la méthode de configuration de démarrage.</span><span class="sxs-lookup"><span data-stu-id="f2b98-165">The last startup key specifies the startup configuration method.</span></span> <span data-ttu-id="f2b98-166">La clé de démarrage OWIN application suivante vous permet de modifier le nom de la classe de configuration à `MyConfiguration` .</span><span class="sxs-lookup"><span data-stu-id="f2b98-166">The following OWIN App startup key allows you to change the name of the configuration class to `MyConfiguration` .</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a><span data-ttu-id="f2b98-167">À l’aide d’Owinhost.exe</span><span class="sxs-lookup"><span data-stu-id="f2b98-167">Using Owinhost.exe</span></span>

1. <span data-ttu-id="f2b98-168">Remplacez le fichier Web.config par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="f2b98-168">Replace the Web.config file with the following markup:</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   <span data-ttu-id="f2b98-169">La dernière clé de wins, donc dans ce cas `TestStartup` est spécifié.</span><span class="sxs-lookup"><span data-stu-id="f2b98-169">The last key wins, so in this case `TestStartup` is specified.</span></span>
2. <span data-ttu-id="f2b98-170">Installer Owinhost à partir de la PMC :</span><span class="sxs-lookup"><span data-stu-id="f2b98-170">Install Owinhost from the PMC:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. <span data-ttu-id="f2b98-171">Accédez au dossier d’application (le dossier qui contient le *Web.config* fichier) et dans une invite de commande et tapez :</span><span class="sxs-lookup"><span data-stu-id="f2b98-171">Navigate to the application folder (the folder containing the *Web.config* file) and in a command prompt and type:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   <span data-ttu-id="f2b98-172">Affiche la fenêtre de commande :</span><span class="sxs-lookup"><span data-stu-id="f2b98-172">The command window will show:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. <span data-ttu-id="f2b98-173">Lancer un navigateur avec l’URL `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="f2b98-173">Launch a browser with the URL `http://localhost:5000/`.</span></span>  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
   <span data-ttu-id="f2b98-174">OwinHost respecté les conventions de démarrage répertoriées ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="f2b98-174">OwinHost honored the startup conventions listed above.</span></span>
5. <span data-ttu-id="f2b98-175">Dans la fenêtre de commande, appuyez sur ENTRÉE pour quitter OwinHost.</span><span class="sxs-lookup"><span data-stu-id="f2b98-175">In the command window, press Enter to exit OwinHost.</span></span>
6. <span data-ttu-id="f2b98-176">Dans le `ProductionStartup` de classe, ajoutez l’attribut OwinStartup suivant qui spécifie un nom convivial de *ProductionConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="f2b98-176">In the `ProductionStartup` class, add the following OwinStartup attribute which specifies a friendly name of *ProductionConfiguration*.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. <span data-ttu-id="f2b98-177">Dans l’invite de commandes tapez :</span><span class="sxs-lookup"><span data-stu-id="f2b98-177">In the command prompt and type:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   <span data-ttu-id="f2b98-178">Chargement de la classe de démarrage de Production.</span><span class="sxs-lookup"><span data-stu-id="f2b98-178">The Production startup class is loaded.</span></span>  
    ![](owin-startup-class-detection/_static/image9.png)  
   <span data-ttu-id="f2b98-179">Notre application a plusieurs classes de démarrage et dans cet exemple, nous avons différée quelle classe de démarrage pour charger jusqu'à l’exécution.</span><span class="sxs-lookup"><span data-stu-id="f2b98-179">Our application has multiple startup classes, and in this example we have deferred which startup class to load until runtime.</span></span>
8. <span data-ttu-id="f2b98-180">Tester les options de démarrage runtime suivantes :</span><span class="sxs-lookup"><span data-stu-id="f2b98-180">Test the following runtime startup options:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
