---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Ajout d’un contrôleur | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 77389bfa4774857eb2a607b0a70e982826312e03
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38119261"
---
<a name="adding-a-controller"></a><span data-ttu-id="b80f9-102">Ajour d’un contrôleur</span><span class="sxs-lookup"><span data-stu-id="b80f9-102">Adding a Controller</span></span>
====================
<span data-ttu-id="b80f9-103">par [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="b80f9-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="b80f9-104">MVC est l’acronyme *model-view-controller*.</span><span class="sxs-lookup"><span data-stu-id="b80f9-104">MVC stands for *model-view-controller*.</span></span> <span data-ttu-id="b80f9-105">MVC est un modèle de développement d’applications sont bien conçue, testable et facile à gérer.</span><span class="sxs-lookup"><span data-stu-id="b80f9-105">MVC is a pattern for developing applications that are well architected, testable and easy to maintain.</span></span> <span data-ttu-id="b80f9-106">Applications basées sur MVC contiennent :</span><span class="sxs-lookup"><span data-stu-id="b80f9-106">MVC-based applications contain:</span></span>

- <span data-ttu-id="b80f9-107">**M** odèles : les Classes qui représentent les données de l’application et qui utilisent une logique de validation pour appliquer des règles d’entreprise pour ces données.</span><span class="sxs-lookup"><span data-stu-id="b80f9-107">**M** odels: Classes that represent the data of the application and that use validation logic to enforce business rules for that data.</span></span>
- <span data-ttu-id="b80f9-108">**V** ues : les fichiers de modèle que votre application utilise pour générer dynamiquement des réponses HTML.</span><span class="sxs-lookup"><span data-stu-id="b80f9-108">**V** iews: Template files that your application uses to dynamically generate HTML responses.</span></span>
- <span data-ttu-id="b80f9-109">**C** ontrôleurs : Classes qui gèrent les demandes du navigateur entrantes, récupérer des données de modèle, puis spécifiez les modèles de vue qui retournent une réponse au navigateur.</span><span class="sxs-lookup"><span data-stu-id="b80f9-109">**C** ontrollers: Classes that handle incoming browser requests, retrieve model data, and then specify view templates that return a response to the browser.</span></span>

<span data-ttu-id="b80f9-110">Nous allons être couvrant tous ces concepts dans cette série de didacticiels et vous montrer comment les utiliser pour créer une application.</span><span class="sxs-lookup"><span data-stu-id="b80f9-110">We'll be covering all these concepts in this tutorial series and show you how to use them to build an application.</span></span>

> [!NOTE]
> <span data-ttu-id="b80f9-111">À l’étape précédente du MVC par défaut le modèle a été sélectionné.</span><span class="sxs-lookup"><span data-stu-id="b80f9-111">In the previous step the Default MVC template was selected.</span></span> <span data-ttu-id="b80f9-112">Cela crée la maison, compte et gérer les contrôleurs par défaut.</span><span class="sxs-lookup"><span data-stu-id="b80f9-112">This creates Home, Account and Manage Controllers by default.</span></span>

<span data-ttu-id="b80f9-113">Nous allons commencer en créant une classe de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="b80f9-113">Let's begin by creating a controller class.</span></span> <span data-ttu-id="b80f9-114">Dans **l’Explorateur de solutions**, avec le bouton droit le *contrôleurs* dossier, puis cliquez sur **ajouter**, puis **contrôleur**.</span><span class="sxs-lookup"><span data-stu-id="b80f9-114">In **Solution Explorer**, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>


![](adding-a-controller/_static/image1.png)

<span data-ttu-id="b80f9-115">Dans le **ajouter une structure** boîte de dialogue, cliquez sur **contrôleur MVC 5 - vide**, puis cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="b80f9-115">In the **Add Scaffold** dialog box, click **MVC 5 Controller - Empty**, and then click **Add**.</span></span>

![](adding-a-controller/_static/image2.png)  
 

<span data-ttu-id="b80f9-116">Nommez votre contrôleur « HelloWorldController » et cliquez sur **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="b80f9-116">Name your new controller "HelloWorldController" and click **Add**.</span></span>

![Ajouter un contrôleur](adding-a-controller/_static/image3.png)

<span data-ttu-id="b80f9-118">Notez que dans **l’Explorateur de solutions** un nouveau fichier a été créé nommé *HelloWorldController.cs* et un nouveau dossier *Views\HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="b80f9-118">Notice in **Solution Explorer** that a new file has been created named *HelloWorldController.cs* and a new folder *Views\HelloWorld*.</span></span> <span data-ttu-id="b80f9-119">Le contrôleur est ouvert dans l’IDE.</span><span class="sxs-lookup"><span data-stu-id="b80f9-119">The controller is open in the IDE.</span></span>

![](adding-a-controller/_static/image4.png)

<span data-ttu-id="b80f9-120">Remplacez le contenu du fichier par le code suivant.</span><span class="sxs-lookup"><span data-stu-id="b80f9-120">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

<span data-ttu-id="b80f9-121">Les méthodes de contrôleur retourne une chaîne de code HTML par exemple.</span><span class="sxs-lookup"><span data-stu-id="b80f9-121">The controller methods will return a string of HTML as an example.</span></span> <span data-ttu-id="b80f9-122">Le contrôleur est nommé `HelloWorldController` et la première méthode se nomme `Index`.</span><span class="sxs-lookup"><span data-stu-id="b80f9-122">The controller is named `HelloWorldController` and the first method is named `Index`.</span></span> <span data-ttu-id="b80f9-123">Nous allons l’appeler à partir d’un navigateur.</span><span class="sxs-lookup"><span data-stu-id="b80f9-123">Let's invoke it from a browser.</span></span> <span data-ttu-id="b80f9-124">Exécutez l’application (appuyez sur F5 ou Ctrl + F5).</span><span class="sxs-lookup"><span data-stu-id="b80f9-124">Run the application (press F5 or Ctrl+F5).</span></span> <span data-ttu-id="b80f9-125">Dans le navigateur, ajoutez &quot;HelloWorld&quot; pour le chemin d’accès dans la barre d’adresses.</span><span class="sxs-lookup"><span data-stu-id="b80f9-125">In the browser, append &quot;HelloWorld&quot; to the path in the address bar.</span></span> <span data-ttu-id="b80f9-126">(Par exemple, dans l’illustration ci-dessous, ses `http://localhost:1234/HelloWorld.`) la page dans le navigateur ressemblera à la capture d’écran suivante.</span><span class="sxs-lookup"><span data-stu-id="b80f9-126">(For example, in the illustration below, it's `http://localhost:1234/HelloWorld.`) The page in the browser will look like the following screenshot.</span></span> <span data-ttu-id="b80f9-127">Dans la méthode ci-dessus, le code a renvoyé une chaîne directement.</span><span class="sxs-lookup"><span data-stu-id="b80f9-127">In the method above, the code returned a string directly.</span></span> <span data-ttu-id="b80f9-128">Vous avez demandé le système pour retourner uniquement des codes HTML, et c’était le cas !</span><span class="sxs-lookup"><span data-stu-id="b80f9-128">You told the system to just return some HTML, and it did!</span></span>

![](adding-a-controller/_static/image5.png)

<span data-ttu-id="b80f9-129">ASP.NET MVC appelle les classes de l’autre contrôleur (et différentes méthodes d’action au sein de celles-ci) en fonction de l’URL entrante.</span><span class="sxs-lookup"><span data-stu-id="b80f9-129">ASP.NET MVC invokes different controller classes (and different action methods within them) depending on the incoming URL.</span></span> <span data-ttu-id="b80f9-130">La logique de routage URL par défaut utilisée par ASP.NET MVC utilise un format comme celui-ci pour déterminer quel code pour appeler :</span><span class="sxs-lookup"><span data-stu-id="b80f9-130">The default URL routing logic used by ASP.NET MVC uses a format like this to determine what code to invoke:</span></span>

`/[Controller]/[ActionName]/[Parameters]`

<span data-ttu-id="b80f9-131">Vous définissez le format pour le routage dans le *application\_Start/RouteConfig.cs* fichier.</span><span class="sxs-lookup"><span data-stu-id="b80f9-131">You set the format for routing in the *App\_Start/RouteConfig.cs* file.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

<span data-ttu-id="b80f9-132">Lorsque vous exécutez l’application et que vous ne fournissez aucun segment d’URL, les valeurs par défaut pour le contrôleur « Home » et la méthode d’action « Index » spécifiée dans la section valeurs par défaut du code ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="b80f9-132">When you run the application and don't supply any URL segments, it defaults to the "Home" controller and the "Index" action method specified in the defaults section of the code above.</span></span>

<span data-ttu-id="b80f9-133">La première partie de l’URL détermine la classe de contrôleur à exécuter.</span><span class="sxs-lookup"><span data-stu-id="b80f9-133">The first part of the URL determines the controller class to execute.</span></span> <span data-ttu-id="b80f9-134">Par conséquent, */HelloWorld* mappe à la `HelloWorldController` classe.</span><span class="sxs-lookup"><span data-stu-id="b80f9-134">So */HelloWorld* maps to the `HelloWorldController` class.</span></span> <span data-ttu-id="b80f9-135">La deuxième partie de l’URL détermine la méthode d’action sur la classe à exécuter.</span><span class="sxs-lookup"><span data-stu-id="b80f9-135">The second part of the URL determines the action method on the class to execute.</span></span> <span data-ttu-id="b80f9-136">Par conséquent, */HelloWorld/Index* provoquerait la `Index` méthode de la `HelloWorldController` classe à exécuter.</span><span class="sxs-lookup"><span data-stu-id="b80f9-136">So */HelloWorld/Index* would cause the `Index` method of the `HelloWorldController` class to execute.</span></span> <span data-ttu-id="b80f9-137">Notez que nous n’avions pour accéder à */HelloWorld* et `Index` méthode a été utilisée par défaut.</span><span class="sxs-lookup"><span data-stu-id="b80f9-137">Notice that we only had to browse to */HelloWorld* and the `Index` method was used by default.</span></span> <span data-ttu-id="b80f9-138">Il s’agit, car une méthode nommée `Index` est la méthode par défaut qui sera appelée sur un contrôleur s’il n’est pas explicitement spécifié.</span><span class="sxs-lookup"><span data-stu-id="b80f9-138">This is because a method named `Index` is the default method that will be called on a controller if one is not explicitly specified.</span></span> <span data-ttu-id="b80f9-139">La troisième partie du segment d’URL (`Parameters`) concerne les données de routage.</span><span class="sxs-lookup"><span data-stu-id="b80f9-139">The third part of the URL segment ( `Parameters`) is for route data.</span></span> <span data-ttu-id="b80f9-140">Nous allons voir les données d’itinéraire par la suite dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="b80f9-140">We'll see route data later on in this tutorial.</span></span>

<span data-ttu-id="b80f9-141">Accédez à `http://localhost:xxxx/HelloWorld/Welcome`.</span><span class="sxs-lookup"><span data-stu-id="b80f9-141">Browse to `http://localhost:xxxx/HelloWorld/Welcome`.</span></span> <span data-ttu-id="b80f9-142">Le `Welcome` méthode s’exécute et retourne la chaîne &quot;c’est la méthode d’action Bienvenue... &quot;.</span><span class="sxs-lookup"><span data-stu-id="b80f9-142">The `Welcome` method runs and returns the string &quot;This is the Welcome action method...&quot;.</span></span> <span data-ttu-id="b80f9-143">Le mappage de MVC par défaut est `/[Controller]/[ActionName]/[Parameters]`.</span><span class="sxs-lookup"><span data-stu-id="b80f9-143">The default MVC mapping is `/[Controller]/[ActionName]/[Parameters]`.</span></span> <span data-ttu-id="b80f9-144">Pour cette URL, le contrôleur est `HelloWorld`, et `Welcome` est la méthode d’action.</span><span class="sxs-lookup"><span data-stu-id="b80f9-144">For this URL, the controller is `HelloWorld` and `Welcome` is the action method.</span></span> <span data-ttu-id="b80f9-145">Vous n’avez pas encore utilisé la partie `[Parameters]` de l’URL.</span><span class="sxs-lookup"><span data-stu-id="b80f9-145">You haven't used the `[Parameters]` part of the URL yet.</span></span>

![](adding-a-controller/_static/image6.png)

<span data-ttu-id="b80f9-146">Nous allons modifier légèrement l’exemple afin que vous pouvez passer des informations de paramètre à partir de l’URL au contrôleur (par exemple, */HelloWorld/Welcome ? nom = Scott&amp;numtimes = 4*).</span><span class="sxs-lookup"><span data-stu-id="b80f9-146">Let's modify the example slightly so that you can pass some parameter information from the URL to the controller (for example, */HelloWorld/Welcome?name=Scott&amp;numtimes=4*).</span></span> <span data-ttu-id="b80f9-147">Modifier votre `Welcome` méthode pour inclure les deux paramètres comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="b80f9-147">Change your `Welcome` method to include two parameters as shown below.</span></span> <span data-ttu-id="b80f9-148">Notez que le code utilise la fonctionnalité de paramètre facultatif de c# pour indiquer que le `numTimes` paramètre par défaut 1 si aucune valeur n’est passée pour ce paramètre.</span><span class="sxs-lookup"><span data-stu-id="b80f9-148">Note that the code uses the C# optional-parameter feature to indicate that the `numTimes` parameter should default to 1 if no value is passed for that parameter.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="b80f9-149">Note de sécurité : Le code ci-dessus utilise [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) pour protéger l’application à partir des entrées malveillantes (à savoir JavaScript).</span><span class="sxs-lookup"><span data-stu-id="b80f9-149">Security Note: The code above uses [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) to protect the application from malicious input (namely JavaScript).</span></span> <span data-ttu-id="b80f9-150">Pour plus d’informations, consultez [Comment : protéger contre les attaques de Script dans une Application Web en utilisant l’encodage HTML en chaînes](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="b80f9-150">For more information see [How to: Protect Against Script Exploits in a Web Application by Applying HTML Encoding to Strings](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).</span></span>


 <span data-ttu-id="b80f9-151">Exécutez votre application et accédez à l’exemple d’URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span><span class="sxs-lookup"><span data-stu-id="b80f9-151">Run your application and browse to the example URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`).</span></span> <span data-ttu-id="b80f9-152">Vous pouvez essayer différentes valeurs pour `name` et `numtimes` dans l’URL.</span><span class="sxs-lookup"><span data-stu-id="b80f9-152">You can try different values for `name` and `numtimes` in the URL.</span></span> <span data-ttu-id="b80f9-153">Le [système de liaison de modèle ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) mappe automatiquement les paramètres nommés à partir de la chaîne de requête dans la barre d’adresses aux paramètres de votre méthode.</span><span class="sxs-lookup"><span data-stu-id="b80f9-153">The [ASP.NET MVC model binding system](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) automatically maps the named parameters from the query string in the address bar to parameters in your method.</span></span>

![](adding-a-controller/_static/image7.png)

<span data-ttu-id="b80f9-154">Dans l’exemple ci-dessus, le segment d’URL ( `Parameters`) n’est pas utilisé, le `name` et `numTimes` paramètres sont passés en tant que [chaînes de requête](http://en.wikipedia.org/wiki/Query_string).</span><span class="sxs-lookup"><span data-stu-id="b80f9-154">In the sample above, the URL segment ( `Parameters`) is not used, the `name` and `numTimes` parameters are passed as [query strings](http://en.wikipedia.org/wiki/Query_string).</span></span> <span data-ttu-id="b80f9-155">Le caractère générique ?</span><span class="sxs-lookup"><span data-stu-id="b80f9-155">The ?</span></span> <span data-ttu-id="b80f9-156">(point d’interrogation) dans l’URL ci-dessus est un séparateur, suivent les chaînes de requête.</span><span class="sxs-lookup"><span data-stu-id="b80f9-156">(question mark) in the above URL is a separator, and the query strings follow.</span></span> <span data-ttu-id="b80f9-157">Le caractère &amp; sépare les chaînes de requête.</span><span class="sxs-lookup"><span data-stu-id="b80f9-157">The &amp; character separates query strings.</span></span>

<span data-ttu-id="b80f9-158">Remplacez la méthode de bienvenue avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="b80f9-158">Replace the Welcome method with the following code:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

<span data-ttu-id="b80f9-159">Exécutez l’application et entrez l’URL suivante : `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span><span class="sxs-lookup"><span data-stu-id="b80f9-159">Run the application and enter the following URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`</span></span>

![](adding-a-controller/_static/image8.png)

<span data-ttu-id="b80f9-160">Cette fois le troisième segment d’URL mise en correspondance le paramètre d’itinéraire `ID.` le `Welcome` méthode d’action contient un paramètre (`ID`) correspondant à la spécification d’URL dans le `RegisterRoutes` (méthode).</span><span class="sxs-lookup"><span data-stu-id="b80f9-160">This time the third URL segment matched the route parameter `ID.` The `Welcome` action method contains a parameter (`ID`) that matched the URL specification in the `RegisterRoutes` method.</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

<span data-ttu-id="b80f9-161">Dans les applications ASP.NET MVC, il est plus courant de passer des paramètres en tant que données d’itinéraire (comme nous l’avons fait avec l’ID ci-dessus) que de les transmettre sous forme de chaînes de requête.</span><span class="sxs-lookup"><span data-stu-id="b80f9-161">In ASP.NET MVC applications, it's more typical to pass in parameters as route data (like we did with ID above) than passing them as query strings.</span></span> <span data-ttu-id="b80f9-162">Vous pouvez également ajouter un itinéraire pour passer à la fois le `name` et `numtimes` dans les paramètres en tant que données d’itinéraire dans l’URL.</span><span class="sxs-lookup"><span data-stu-id="b80f9-162">You could also add a route to pass both the `name` and `numtimes` in parameters as route data in the URL.</span></span> <span data-ttu-id="b80f9-163">Dans le *application\_start\routeconfig* , ajoutez l’itinéraire « Hello » :</span><span class="sxs-lookup"><span data-stu-id="b80f9-163">In the *App\_Start\RouteConfig.cs* file, add the "Hello" route:</span></span>

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

<span data-ttu-id="b80f9-164">Exécutez l’application et accédez à `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span><span class="sxs-lookup"><span data-stu-id="b80f9-164">Run the application and browse to `/localhost:XXX/HelloWorld/Welcome/Scott/3`.</span></span>

![](adding-a-controller/_static/image9.png)

<span data-ttu-id="b80f9-165">Pour de nombreuses applications MVC, l’itinéraire par défaut fonctionne bien.</span><span class="sxs-lookup"><span data-stu-id="b80f9-165">For many MVC applications, the default route works fine.</span></span> <span data-ttu-id="b80f9-166">Vous apprendrez plus loin dans ce didacticiel pour passer des données à l’aide du binder de modèle, et vous n’aurez pas à modifier l’itinéraire par défaut pour ce faire.</span><span class="sxs-lookup"><span data-stu-id="b80f9-166">You'll learn later in this tutorial to pass data using the model binder, and you won't have to modify the default route for that.</span></span>

<span data-ttu-id="b80f9-167">Dans ces exemples le contrôleur a fait le &quot;VC&quot; partie de MVC, autrement dit, le travail de la vue et contrôleur.</span><span class="sxs-lookup"><span data-stu-id="b80f9-167">In these examples the controller has been doing the &quot;VC&quot; portion of MVC — that is, the view and controller work.</span></span> <span data-ttu-id="b80f9-168">Le contrôleur retourne directement du HTML.</span><span class="sxs-lookup"><span data-stu-id="b80f9-168">The controller is returning HTML directly.</span></span> <span data-ttu-id="b80f9-169">En règle générale, vous ne voulez contrôleurs retournent directement, du HTML dans la mesure où cela devient très difficile de revenir en code.</span><span class="sxs-lookup"><span data-stu-id="b80f9-169">Ordinarily you don't want controllers returning HTML directly, since that becomes very cumbersome to code.</span></span> <span data-ttu-id="b80f9-170">Au lieu de cela, nous allons utiliser généralement un fichier de modèle de vue séparé afin de générer la réponse HTML.</span><span class="sxs-lookup"><span data-stu-id="b80f9-170">Instead we'll typically use a separate view template file to help generate the HTML response.</span></span> <span data-ttu-id="b80f9-171">Penchons-nous maintenant à la façon dont nous pouvons le faire.</span><span class="sxs-lookup"><span data-stu-id="b80f9-171">Let's look next at how we can do this.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b80f9-172">[Précédent](getting-started.md)
> [Suivant](adding-a-view.md)</span><span class="sxs-lookup"><span data-stu-id="b80f9-172">[Previous](getting-started.md)
[Next](adding-a-view.md)</span></span>
