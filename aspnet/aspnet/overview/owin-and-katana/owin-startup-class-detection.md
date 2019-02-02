---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Détection de classe de démarrage OWIN | Microsoft Docs
author: Praburaj
description: Ce didacticiel montre comment configurer la classe de démarrage OWIN est chargée. Pour plus d’informations sur OWIN, consultez une vue d’ensemble du projet Katana. Ce didacticiel a été...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 0b34cca8b48383dbb028106651758dff889ed614
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667295"
---
<a name="owin-startup-class-detection"></a><span data-ttu-id="67744-105">Détection de classe de démarrage OWIN</span><span class="sxs-lookup"><span data-stu-id="67744-105">OWIN Startup Class Detection</span></span>
====================

> <span data-ttu-id="67744-106">Ce didacticiel montre comment configurer la classe de démarrage OWIN est chargée.</span><span class="sxs-lookup"><span data-stu-id="67744-106">This tutorial shows how to configure which OWIN startup class is loaded.</span></span> <span data-ttu-id="67744-107">Pour plus d’informations sur OWIN, consultez [une vue d’ensemble du projet Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="67744-107">For more information on OWIN, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="67744-108">Ce didacticiel a été rédigé par Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan et Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).</span><span class="sxs-lookup"><span data-stu-id="67744-108">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>
>
> ## <a name="prerequisites"></a><span data-ttu-id="67744-109">Prérequis</span><span class="sxs-lookup"><span data-stu-id="67744-109">Prerequisites</span></span>
>
> [<span data-ttu-id="67744-110">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="67744-110">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)


## <a name="owin-startup-class-detection"></a><span data-ttu-id="67744-111">Détection de classe de démarrage OWIN</span><span class="sxs-lookup"><span data-stu-id="67744-111">OWIN Startup Class Detection</span></span>

 <span data-ttu-id="67744-112">Chaque Application OWIN a une classe de démarrage où vous spécifiez des composants pour le pipeline d’application.</span><span class="sxs-lookup"><span data-stu-id="67744-112">Every OWIN Application has a startup class where you specify components for the application pipeline.</span></span> <span data-ttu-id="67744-113">Il existe différentes façons, vous pouvez vous connecter à votre classe de démarrage avec le runtime, selon le modèle d’hébergement que vous choisissez (OwinHost, IIS et IIS Express).</span><span class="sxs-lookup"><span data-stu-id="67744-113">There are different ways you can connect your startup class with the runtime, depending on the hosting model you choose (OwinHost, IIS, and IIS-Express).</span></span> <span data-ttu-id="67744-114">La classe de démarrage présentée dans ce didacticiel peut être utilisée dans chaque application d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="67744-114">The startup class shown in this tutorial can be used in every hosting application.</span></span> <span data-ttu-id="67744-115">Vous vous connectez à la classe de démarrage avec l’hébergement runtime à l’aide d’une de ces approches :</span><span class="sxs-lookup"><span data-stu-id="67744-115">You connect the startup class with the hosting runtime using one of the these approaches:</span></span>

1. <span data-ttu-id="67744-116">**Convention de dénomination**: Recherche de Katana pour une classe nommée `Startup` dans l’espace de noms mise en correspondance le nom d’assembly ou de l’espace de noms global.</span><span class="sxs-lookup"><span data-stu-id="67744-116">**Naming Convention**: Katana looks for a class named `Startup` in namespace matching the assembly name or the global namespace.</span></span>
2. <span data-ttu-id="67744-117">**Attribut de OwinStartup**: Il s’agit de l’approche que prendra la plupart des développeurs pour spécifier la classe de démarrage.</span><span class="sxs-lookup"><span data-stu-id="67744-117">**OwinStartup Attribute**: This is the approach most developers will take to specify the startup class.</span></span> <span data-ttu-id="67744-118">L’attribut suivant définit la classe de démarrage le `TestStartup` classe dans le `StartupDemo` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="67744-118">The following attribute will set the startup class to the `TestStartup` class in the `StartupDemo` namespace.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   <span data-ttu-id="67744-119">Le `OwinStartup` attribut remplace la convention d’affectation de noms.</span><span class="sxs-lookup"><span data-stu-id="67744-119">The `OwinStartup` attribute overrides the naming convention.</span></span> <span data-ttu-id="67744-120">Vous pouvez également spécifier un nom convivial avec cet attribut, toutefois, à l’aide d’un nom convivial, vous devez également utiliser le `appSetting` élément dans le fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="67744-120">You can also specify a friendly name with this attribute, however, using a friendly name requires you to also use the `appSetting` element in the configuration file.</span></span>
3. <span data-ttu-id="67744-121">**L’élément appSetting dans le fichier de Configuration**: Le `appSetting` élément remplace la `OwinStartup` attribut et la convention d’affectation de noms.</span><span class="sxs-lookup"><span data-stu-id="67744-121">**The appSetting element in the Configuration file**: The `appSetting` element overrides the `OwinStartup` attribute and naming convention.</span></span> <span data-ttu-id="67744-122">Vous pouvez avoir plusieurs classes de démarrage (chacun utilisant une `OwinStartup` attribut) et configurer la classe de démarrage qui sera chargé dans un fichier de configuration à l’aide de balisage similaire à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="67744-122">You can have multiple startup classes (each using an `OwinStartup` attribute) and configure which startup class will be loaded in a configuration file using markup similar to the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   <span data-ttu-id="67744-123">La clé suivante, qui spécifie explicitement l’assembly et la classe de démarrage peut également être utilisée :</span><span class="sxs-lookup"><span data-stu-id="67744-123">The following key, which explicitly specifies the startup class and assembly can also be used:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   <span data-ttu-id="67744-124">Le code XML suivant dans le fichier de configuration spécifie un nom de classe de démarrage convivial `ProductionConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="67744-124">The following XML in the configuration file specifies a friendly startup class name of `ProductionConfiguration`.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   <span data-ttu-id="67744-125">Le balisage ci-dessus doit être utilisé par le code suivant `OwinStartup` attribut qui spécifie un nom convivial et provoque la `ProductionStartup2` classe à exécuter.</span><span class="sxs-lookup"><span data-stu-id="67744-125">The above markup must be used with the following `OwinStartup` attribute which specifies a friendly name and causes the `ProductionStartup2` class to run.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. <span data-ttu-id="67744-126">Pour désactiver la détection de démarrage OWIN ajouter la `appSetting owin:AutomaticAppStartup` avec la valeur `"false"` dans le fichier web.config.</span><span class="sxs-lookup"><span data-stu-id="67744-126">To disable OWIN startup discovery add the `appSetting owin:AutomaticAppStartup` with a value of `"false"` in the web.config file.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a><span data-ttu-id="67744-127">Créer une application de Web ASP.NET à l’aide de démarrage OWIN</span><span class="sxs-lookup"><span data-stu-id="67744-127">Create an ASP.NET Web App using OWIN Startup</span></span>

1. <span data-ttu-id="67744-128">Créez une application de web Asp.Net vide et nommez-la **StartupDemo**.</span><span class="sxs-lookup"><span data-stu-id="67744-128">Create an empty Asp.Net web application and name it **StartupDemo**.</span></span> <span data-ttu-id="67744-129">-Installez `Microsoft.Owin.Host.SystemWeb` à l’aide du Gestionnaire de package NuGet.</span><span class="sxs-lookup"><span data-stu-id="67744-129">- Install `Microsoft.Owin.Host.SystemWeb` using the NuGet package manager.</span></span> <span data-ttu-id="67744-130">À partir de la **outils** menu, sélectionnez **Gestionnaire de Package NuGet**, puis **Console du Gestionnaire de Package**.</span><span class="sxs-lookup"><span data-stu-id="67744-130">From the **Tools** menu, select **NuGet Package Manager**, and then **Package Manager Console**.</span></span> <span data-ttu-id="67744-131">Entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="67744-131">Enter the following command:</span></span>

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. <span data-ttu-id="67744-132">Ajouter une classe de démarrage OWIN.</span><span class="sxs-lookup"><span data-stu-id="67744-132">Add an OWIN startup class.</span></span> <span data-ttu-id="67744-133">Dans Visual Studio 2017, cliquez sur le projet et sélectionnez **ajouter une classe**. - dans le **ajouter un nouvel élément** boîte de dialogue, entrez *OWIN* dans le champ de recherche, puis remplacez le nom par Startup.cs, puis sélectionnez **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="67744-133">In Visual Studio 2017 right-click the project and select **Add Class**.- In the **Add New Item** dialog box, enter *OWIN* in the search field, and change the name to Startup.cs, and then select **Add**.</span></span>

     ![](owin-startup-class-detection/_static/image1.png)

   <span data-ttu-id="67744-134">La prochaine fois que vous souhaitez ajouter un *classe de démarrage Owin*, il sera dans disponible à partir de la **ajouter** menu.</span><span class="sxs-lookup"><span data-stu-id="67744-134">The next time you want to add an *Owin Startup class*, it will be in available from the **Add** menu.</span></span>

     ![](owin-startup-class-detection/_static/image2.png)

   <span data-ttu-id="67744-135">Ou bien, vous pouvez cliquez sur le projet et sélectionnez **ajouter**, puis sélectionnez **un nouvel élément**, puis sélectionnez le **classe de démarrage Owin**.</span><span class="sxs-lookup"><span data-stu-id="67744-135">Alternatively, you can right-click the project and select **Add**, then select **New Item**, and then select the **Owin Startup class**.</span></span>

     ![](owin-startup-class-detection/_static/image3.png)

- <span data-ttu-id="67744-136">Remplacez le code généré dans le *Startup.cs* fichier par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="67744-136">Replace the generated code in the *Startup.cs* file with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]

  <span data-ttu-id="67744-137">Le `app.Use` expression lambda est utilisée pour enregistrer le composant d’intergiciel (middleware) spécifié au pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="67744-137">The `app.Use` lambda expression is used to register the specified middleware component to the OWIN pipeline.</span></span> <span data-ttu-id="67744-138">Dans ce cas, nous configurons la journalisation des demandes entrantes avant de répondre à la demande entrante.</span><span class="sxs-lookup"><span data-stu-id="67744-138">In this case we are setting up logging of incoming requests before responding to the incoming request.</span></span> <span data-ttu-id="67744-139">Le `next` paramètre est le délégué ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [tâche](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) au composant suivant dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="67744-139">The `next` parameter is the delegate ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) to the next component in the pipeline.</span></span> <span data-ttu-id="67744-140">Le `app.Run` raccorde le pipeline aux demandes entrantes de l’expression lambda et fournit le mécanisme de réponse.</span><span class="sxs-lookup"><span data-stu-id="67744-140">The `app.Run` lambda expression hooks up the pipeline to incoming requests and provides the response mechanism.</span></span>
     > [!NOTE]
     > <span data-ttu-id="67744-141">Dans le code ci-dessus nous avons mis en commentaire le `OwinStartup` attribut et nous allons s’appuyer sur la convention de l’exécution de la classe nommée `Startup` .-Press ***F5*** pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="67744-141">In the code above we have commented out the `OwinStartup` attribute and we're relying on the convention of running the class named `Startup` .- Press ***F5*** to run the application.</span></span> <span data-ttu-id="67744-142">Cliquez sur Actualiser plusieurs fois.</span><span class="sxs-lookup"><span data-stu-id="67744-142">Hit refresh a few times.</span></span>

    <span data-ttu-id="67744-143">![](owin-startup-class-detection/_static/image4.png) Remarque : Le nombre indiqué dans les images dans ce didacticiel ne correspondra pas le numéro que vous voyez.</span><span class="sxs-lookup"><span data-stu-id="67744-143">![](owin-startup-class-detection/_static/image4.png) Note: The number shown in the images in this tutorial will not match the number you see.</span></span> <span data-ttu-id="67744-144">La chaîne de milliseconde est utilisée pour afficher une nouvelle réponse lorsque vous actualisez la page.</span><span class="sxs-lookup"><span data-stu-id="67744-144">The millisecond string is used to show a new response when you refresh the page.</span></span>
  <span data-ttu-id="67744-145">Vous pouvez voir les informations de trace dans le **sortie** fenêtre.</span><span class="sxs-lookup"><span data-stu-id="67744-145">You can see the trace information in the **Output** window.</span></span>

    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a><span data-ttu-id="67744-146">Ajouter plusieurs Classes de démarrage</span><span class="sxs-lookup"><span data-stu-id="67744-146">Add More Startup Classes</span></span>

<span data-ttu-id="67744-147">Dans cette section, nous allons ajouter une autre classe de démarrage.</span><span class="sxs-lookup"><span data-stu-id="67744-147">In this section we'll add another Startup class.</span></span> <span data-ttu-id="67744-148">Vous pouvez ajouter plusieurs classe de démarrage OWIN à votre application.</span><span class="sxs-lookup"><span data-stu-id="67744-148">You can add multiple OWIN startup class to your application.</span></span> <span data-ttu-id="67744-149">Par exemple, vous souhaiterez peut-être créer des classes de démarrage pour le développement, test et production.</span><span class="sxs-lookup"><span data-stu-id="67744-149">For example, you might want to create startup classes for development, testing and production.</span></span>

1. <span data-ttu-id="67744-150">Créez une nouvelle classe de démarrage OWIN et nommez-le `ProductionStartup`.</span><span class="sxs-lookup"><span data-stu-id="67744-150">Create a new OWIN Startup class and name it `ProductionStartup`.</span></span>
2. <span data-ttu-id="67744-151">Remplacez le code généré par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="67744-151">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. <span data-ttu-id="67744-152">Appuyez sur F5 de contrôle pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="67744-152">Press Control F5 to run the app.</span></span> <span data-ttu-id="67744-153">Le `OwinStartup` attribut spécifie la classe de démarrage de production est exécutée.</span><span class="sxs-lookup"><span data-stu-id="67744-153">The `OwinStartup` attribute specifies the production startup class is run.</span></span>

    ![](owin-startup-class-detection/_static/image6.png)
4. <span data-ttu-id="67744-154">Créez une autre classe de démarrage OWIN et nommez-le `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="67744-154">Create another OWIN Startup class and name it `TestStartup`.</span></span>
5. <span data-ttu-id="67744-155">Remplacez le code généré par ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="67744-155">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   <span data-ttu-id="67744-156">Le `OwinStartup` surcharge attribut ci-dessus spécifie `TestingConfiguration` en tant que le *convivial* nom de la classe de démarrage.</span><span class="sxs-lookup"><span data-stu-id="67744-156">The `OwinStartup` attribute overload above specifies `TestingConfiguration` as the *friendly* name of the Startup class.</span></span>
6. <span data-ttu-id="67744-157">Ouvrez le *web.config* fichier, puis ajoutez la clé de démarrage OWIN application qui spécifie le nom convivial de la classe de démarrage :</span><span class="sxs-lookup"><span data-stu-id="67744-157">Open the *web.config* file and add the OWIN App startup key which specifies the friendly name of the Startup class:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. <span data-ttu-id="67744-158">Appuyez sur F5 de contrôle pour exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="67744-158">Press Control F5 to run the app.</span></span> <span data-ttu-id="67744-159">L’élément de paramètres d’application est prioritaire et le test de configuration est exécutée.</span><span class="sxs-lookup"><span data-stu-id="67744-159">The app settings element takes precedent, and the test configuration is run.</span></span>

    ![](owin-startup-class-detection/_static/image7.png)
8. <span data-ttu-id="67744-160">Supprimer le *convivial* nom de la `OwinStartup` d’attribut dans la `TestStartup` classe.</span><span class="sxs-lookup"><span data-stu-id="67744-160">Remove the *friendly* name from the `OwinStartup` attribute in the `TestStartup` class.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. <span data-ttu-id="67744-161">Remplacer la clé de démarrage OWIN application dans le *web.config* fichier par le code suivant :</span><span class="sxs-lookup"><span data-stu-id="67744-161">Replace the OWIN App startup key in the *web.config* file with the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. <span data-ttu-id="67744-162">Rétablir le `OwinStartup` attribut dans chaque classe pour le code d’attribut par défaut généré par Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="67744-162">Revert the `OwinStartup` attribute in each class to the default attribute code generated by Visual Studio:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    <span data-ttu-id="67744-163">Chacune des clés de démarrage OWIN application ci-dessous entraînera la classe de production s’exécute.</span><span class="sxs-lookup"><span data-stu-id="67744-163">Each of the OWIN App startup keys below will cause the production class to run.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    <span data-ttu-id="67744-164">La dernière clé de démarrage spécifie la méthode de configuration de démarrage.</span><span class="sxs-lookup"><span data-stu-id="67744-164">The last startup key specifies the startup configuration method.</span></span> <span data-ttu-id="67744-165">La clé de démarrage OWIN application suivante vous permet de modifier le nom de la classe de configuration à `MyConfiguration` .</span><span class="sxs-lookup"><span data-stu-id="67744-165">The following OWIN App startup key allows you to change the name of the configuration class to `MyConfiguration` .</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a><span data-ttu-id="67744-166">À l’aide d’Owinhost.exe</span><span class="sxs-lookup"><span data-stu-id="67744-166">Using Owinhost.exe</span></span>

1. <span data-ttu-id="67744-167">Remplacez le fichier Web.config par le balisage suivant :</span><span class="sxs-lookup"><span data-stu-id="67744-167">Replace the Web.config file with the following markup:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   <span data-ttu-id="67744-168">La dernière clé de wins, donc dans ce cas `TestStartup` est spécifié.</span><span class="sxs-lookup"><span data-stu-id="67744-168">The last key wins, so in this case `TestStartup` is specified.</span></span>
2. <span data-ttu-id="67744-169">Installer Owinhost à partir de la PMC :</span><span class="sxs-lookup"><span data-stu-id="67744-169">Install Owinhost from the PMC:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. <span data-ttu-id="67744-170">Accédez au dossier d’application (le dossier qui contient le *Web.config* fichier) et dans une invite de commande et tapez :</span><span class="sxs-lookup"><span data-stu-id="67744-170">Navigate to the application folder (the folder containing the *Web.config* file) and in a command prompt and type:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   <span data-ttu-id="67744-171">Affiche la fenêtre de commande :</span><span class="sxs-lookup"><span data-stu-id="67744-171">The command window will show:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. <span data-ttu-id="67744-172">Lancer un navigateur avec l’URL `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="67744-172">Launch a browser with the URL `http://localhost:5000/`.</span></span>

    ![](owin-startup-class-detection/_static/image8.png)

   <span data-ttu-id="67744-173">OwinHost respecté les conventions de démarrage répertoriées ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="67744-173">OwinHost honored the startup conventions listed above.</span></span>
5. <span data-ttu-id="67744-174">Dans la fenêtre de commande, appuyez sur ENTRÉE pour quitter OwinHost.</span><span class="sxs-lookup"><span data-stu-id="67744-174">In the command window, press Enter to exit OwinHost.</span></span>
6. <span data-ttu-id="67744-175">Dans le `ProductionStartup` de classe, ajoutez l’attribut OwinStartup suivant qui spécifie un nom convivial de *ProductionConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="67744-175">In the `ProductionStartup` class, add the following OwinStartup attribute which specifies a friendly name of *ProductionConfiguration*.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. <span data-ttu-id="67744-176">Dans l’invite de commandes tapez :</span><span class="sxs-lookup"><span data-stu-id="67744-176">In the command prompt and type:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   <span data-ttu-id="67744-177">Chargement de la classe de démarrage de Production.</span><span class="sxs-lookup"><span data-stu-id="67744-177">The Production startup class is loaded.</span></span>
    ![](owin-startup-class-detection/_static/image9.png)

   <span data-ttu-id="67744-178">Notre application a plusieurs classes de démarrage et dans cet exemple, nous avons différée quelle classe de démarrage pour charger jusqu'à l’exécution.</span><span class="sxs-lookup"><span data-stu-id="67744-178">Our application has multiple startup classes, and in this example we have deferred which startup class to load until runtime.</span></span>
8. <span data-ttu-id="67744-179">Tester les options de démarrage runtime suivantes :</span><span class="sxs-lookup"><span data-stu-id="67744-179">Test the following runtime startup options:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
