---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: "Profiler et déboguer votre application ASP.NET MVC avec Glimpse | Documents Microsoft"
author: Rick-Anderson
description: "Aperçu est plein essor et augmente la famille de packages de NuGet open source qui fournit des performances détaillés, débogage et des informations de diagnostic pour ASP.NET un..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 9cfdced21251b482ca527dda9c3a698de77cc8ca
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="bb3bc-103">Profiler et déboguer votre application ASP.NET MVC avec aperçu</span><span class="sxs-lookup"><span data-stu-id="bb3bc-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>
====================
<span data-ttu-id="bb3bc-104">Par [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="bb3bc-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="bb3bc-105">Aperçu est plein essor et augmente la famille de packages de NuGet open source qui fournit des performances détaillés, débogage et des informations de diagnostic pour les applications ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="bb3bc-106">Il est très facile à installer, léger et ultra rapide et affiche les mesures de performances clés au bas de chaque page.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="bb3bc-107">Il vous permet d’approfondir votre application lorsque vous avez besoin savoir ce qui se passe au niveau du serveur.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="bb3bc-108">Aperçu fournit des informations précieuses bien que nous vous recommandons de que vous l’utilisez dans votre cycle de développement, y compris de votre environnement de test Azure.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="bb3bc-109">Alors que [Fiddler](http://www.telerik.com/fiddler) et [outils de développement F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) fournissent un côté client vue Aperçu fournit une vue détaillée du serveur.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="bb3bc-110">Ce didacticiel se concentrera sur l’utilisation de l’aperçu ASP.NET MVC et les packages EF, mais de nombreux autres packages sont disponibles.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="bb3bc-111">Lorsque cela est possible de lier sera à approprié [visualiser docs](http://getglimpse.com/Docs/) qui vous aider à gérer.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="bb3bc-112">Aperçu est un projet open source, vous pouvez trop contribuer au code source et les documents.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>


- [<span data-ttu-id="bb3bc-113">L’installation d’aperçu</span><span class="sxs-lookup"><span data-stu-id="bb3bc-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="bb3bc-114">Activer l’aperçu pour localhost</span><span class="sxs-lookup"><span data-stu-id="bb3bc-114">Enable Glimpse for localhost</span></span>](#eg)
- [<span data-ttu-id="bb3bc-115">L’onglet de la chronologie</span><span class="sxs-lookup"><span data-stu-id="bb3bc-115">The Timeline tab</span></span>](#Time)
- [<span data-ttu-id="bb3bc-116">Liaison de données</span><span class="sxs-lookup"><span data-stu-id="bb3bc-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="bb3bc-117">Routes</span><span class="sxs-lookup"><span data-stu-id="bb3bc-117">Routes</span></span>](#route)
- [<span data-ttu-id="bb3bc-118">À l’aide de Glimpse dans Azure</span><span class="sxs-lookup"><span data-stu-id="bb3bc-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="bb3bc-119">Ressources supplémentaires pour MSBuild</span><span class="sxs-lookup"><span data-stu-id="bb3bc-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="bb3bc-120">L’installation d’aperçu</span><span class="sxs-lookup"><span data-stu-id="bb3bc-120">Installing Glimpse</span></span>

<span data-ttu-id="bb3bc-121">Vous pouvez installer aperçu à partir de la console Gestionnaire de package NuGet ou de la **gérer les Packages NuGet** console.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="bb3bc-122">Pour cette démonstration, vous allez installer les packages Mvc5 et EF6 :</span><span class="sxs-lookup"><span data-stu-id="bb3bc-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![installer l’aperçu de NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="bb3bc-124">Recherchez *Glimpse.EF*</span><span class="sxs-lookup"><span data-stu-id="bb3bc-124">Search for *Glimpse.EF*</span></span>

![Glimpse.EF de NuGet installation dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="bb3bc-126">En sélectionnant **les packages installés**, vous pouvez voir les modules dépendants aperçu installés :</span><span class="sxs-lookup"><span data-stu-id="bb3bc-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![Dans les packages de Glimpse installés à partir de DLg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="bb3bc-128">Les commandes suivantes installent les modules aperçu MVC5 et EF6 à partir de la console du Gestionnaire de package :</span><span class="sxs-lookup"><span data-stu-id="bb3bc-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="bb3bc-129">Activer l’aperçu pour localhost</span><span class="sxs-lookup"><span data-stu-id="bb3bc-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="bb3bc-130">Accédez à http://localhost :&lt;# port&gt;/glimpse.axd et cliquez sur le **activer l’aperçu sur** bouton.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the **Turn Glimpse On** button.</span></span>

![Page d’aperçu axd](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="bb3bc-132">Si vous avez affiché votre barre des Favoris, faites glisser et déposez les boutons d’aperçu et les ajouter comme bookmarklets :</span><span class="sxs-lookup"><span data-stu-id="bb3bc-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![Internet Explorer avec Glimpse boookmarklets](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="bb3bc-134">Vous pouvez désormais accéder votre application et le **têtes d’affichage** (HUD) est indiqué en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![Page Gestionnaire de contacts avec HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="bb3bc-136">Le [page d’aperçu HUD](http://getglimpse.com/Docs/Heads-up-Display) toutes les informations de minutage indiquées ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="bb3bc-137">L’affiche de données le HUD performances discrète peut vous avertir d’un problème immédiatement - avant d’arriver au cycle de test.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="bb3bc-138">En cliquant sur le &quot;g&quot; dans l’angle inférieur droit affiche le volet d’aperçu :</span><span class="sxs-lookup"><span data-stu-id="bb3bc-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Volet d’aperçu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="bb3bc-140">Dans l’image ci-dessus, le [onglet exécution](http://getglimpse.com/Docs/Execution-Tab) est sélectionnée, qui affiche les détails de minutage des actions et des filtres dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="bb3bc-141">Vous pouvez voir mon [horloge pour arrêter la surveillance filtre](http://www.nuget.org/packages/StopWatch/) commencent à l’étape 6 du pipeline.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="bb3bc-142">Alors que mon minuteur léger peut fournir utile profil/synchronisation des données, il n’arrive pas tout le temps passé dans l’autorisation et de rendu de l’affichage.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="bb3bc-143">Vous pouvez lire sur mon minuterie à [de profil et l’heure de votre application ASP.NET MVC à Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span><span class="sxs-lookup"><span data-stu-id="bb3bc-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="bb3bc-144">Le [onglets](http://getglimpse.com/Docs/Tabs) page fournit des liens vers des informations détaillées sur chaque onglet.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="bb3bc-145">L’onglet de la chronologie</span><span class="sxs-lookup"><span data-stu-id="bb3bc-145">The Timeline tab</span></span>

<span data-ttu-id="bb3bc-146">J’ai modifié de Tom Dykstra en suspens [didacticiel de 6/MVC 5 EF](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) modifier au contrôleur instructeurs avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="bb3bc-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="bb3bc-147">Le code ci-dessus me permet de passer dans la chaîne de requête (`eager`) contrôle eager ou explicite le chargement de données.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="bb3bc-148">Dans l’image ci-dessous, le chargement explicite est utilisé et la page de synchronisation affiche chaque inscription chargée dans le `Index` méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="bb3bc-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![chargement explicite](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="bb3bc-150">Dans le code suivant, eager est spécifié, et chaque inscription est atteinte après la `Index` vue est appelée :</span><span class="sxs-lookup"><span data-stu-id="bb3bc-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![Eager est spécifié.](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="bb3bc-152">Vous pouvez pointer sur un segment de temps pour obtenir des informations de temporisation détaillées :</span><span class="sxs-lookup"><span data-stu-id="bb3bc-152">You can hover over a time segment to get detailed timing information:</span></span>

![pointage pour voir de temporisation détaillées](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="bb3bc-154">Liaison de modèle</span><span class="sxs-lookup"><span data-stu-id="bb3bc-154">Model Binding</span></span>

<span data-ttu-id="bb3bc-155">Le [onglet de liaison de modèle](http://getglimpse.com/Docs/Model-Binding-Tab) fournit une mine d’informations pour vous aider à comprendre comment vos variables de formulaire sont liés et la raison pour laquelle certaines ne sont pas liés comme l’attend.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="bb3bc-156">L’image ci-dessous montre les **?**</span><span class="sxs-lookup"><span data-stu-id="bb3bc-156">The image below shows the **?**</span></span> <span data-ttu-id="bb3bc-157">icône, vous pouvez cliquer sur pour afficher la page d’aide Aperçu pour cette fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![visualiser la vue de modèle de liaison](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="bb3bc-159">Routes</span><span class="sxs-lookup"><span data-stu-id="bb3bc-159">Routes</span></span>

 <span data-ttu-id="bb3bc-160">L’onglet Aperçu itinéraires sera peuvent vous aider à déboguer et comprendre le routage.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="bb3bc-161">Dans l’image ci-dessous, l’itinéraire de produit est sélectionné (et elle est affichée en vert, une convention d’aperçu).</span><span class="sxs-lookup"><span data-stu-id="bb3bc-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="bb3bc-162">![nom du produit sélectionné](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) jetons contraintes, les zones et les données d’itinéraire sont également affichés.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="bb3bc-163">Consultez [Glimpse itinéraires](http://getglimpse.com/Docs/Routes-Tab) et [attribut routage dans ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="bb3bc-164">À l’aide de Glimpse dans Azure</span><span class="sxs-lookup"><span data-stu-id="bb3bc-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="bb3bc-165">La stratégie de sécurité par défaut aperçu autorise uniquement les données d’aperçu s’affiche à partir de l’hôte local.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="bb3bc-166">Vous pouvez modifier cette stratégie de sécurité afin de pouvoir consulter ces données sur un serveur distant (par exemple, une application web sur Azure).</span><span class="sxs-lookup"><span data-stu-id="bb3bc-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="bb3bc-167">Pour les environnements de test sur Azure, ajoutez la marque en surbrillance jusqu’au bas de la *web.confg* fichier pour activer l’aperçu :</span><span class="sxs-lookup"><span data-stu-id="bb3bc-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.confg* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="bb3bc-168">Cette modification uniquement, tout utilisateur peut consulter vos données d’aperçu sur un site distant.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="bb3bc-169">Envisagez d’ajouter le balisage ci-dessus pour un profil de publication afin qu’il a déployé uniquement un appliqué lorsque vous utilisez ce profil de publication (par exemple, votre proifle test Azure.) Pour restreindre les données d’aperçu, nous allons ajouter le `canViewGlimpseData` rôle et autorise uniquement les utilisateurs de ce rôle pour afficher les données d’aperçu.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-169">Consider adding the markup above to a publish profile so it's only deployed an applyed when you use that publish profile (for example, your Azure test proifle.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="bb3bc-170">Supprimez les commentaires de la *GlimpseSecurityPolicy.cs* de fichiers et de modifier le [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) appeler à partir de `Administrator` à la `canViewGlimpseData` rôle :</span><span class="sxs-lookup"><span data-stu-id="bb3bc-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="bb3bc-171">Sécurité - les données enrichies fournies par l’aperçu peut exposer la sécurité de votre application.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="bb3bc-172">Microsoft n’a pas effectué un audit de sécurité de Glimpse pour une utilisation sur les applications de l’environnement de production.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>


<span data-ttu-id="bb3bc-173">Pour plus d’informations sur l’ajout de rôles, consultez Mes [déployer une application web de Secure ASP.NET MVC 5 avec l’appartenance, OAuth et base de données SQL Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) didacticiel.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="bb3bc-174">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="bb3bc-174">Additional Resources</span></span>

- [<span data-ttu-id="bb3bc-175">Déployer une application sécurisée ASP.NET MVC 5 avec l’appartenance, OAuth et base de données SQL Azure</span><span class="sxs-lookup"><span data-stu-id="bb3bc-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="bb3bc-176">[Visualiser la Configuration](http://getglimpse.com/Docs/Configuration) -page de documentation sur la configuration des onglets, stratégie d’exécution, la journalisation et bien plus encore.</span><span class="sxs-lookup"><span data-stu-id="bb3bc-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
