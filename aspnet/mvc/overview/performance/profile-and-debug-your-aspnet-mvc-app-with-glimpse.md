---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Profiler et déboguer votre application ASP.NET MVC avec Glimpse | Microsoft Docs
author: Rick-Anderson
description: Aperçu est un plein essor et en constante évolution de la famille de packages NuGet d’open source qui fournit les données de performances détaillées, débogage et informations de diagnostic pour ASP.NET un...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: f5d174ff6823d654a24dcb2c90f10a3cbd24f1e7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836225"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="415f7-103">Profiler et déboguer votre application ASP.NET MVC avec Glimpse</span><span class="sxs-lookup"><span data-stu-id="415f7-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>
====================
<span data-ttu-id="415f7-104">par [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="415f7-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="415f7-105">Aperçu est plein essor et en constante évolution de la famille de packages NuGet d’open source qui fournit les données de performances détaillées, débogage et des informations de diagnostic pour les applications ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="415f7-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="415f7-106">Il est très facile à installer, léger et ultra rapide et affiche les mesures de performances clés en bas de chaque page.</span><span class="sxs-lookup"><span data-stu-id="415f7-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="415f7-107">Il vous permet d’approfondir votre application lorsque vous avez besoin savoir ce qui se passait au niveau du serveur.</span><span class="sxs-lookup"><span data-stu-id="415f7-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="415f7-108">Aperçu fournit des informations précieuses tellement que nous vous recommandons de qu'utiliser cela dans votre cycle de développement, y compris votre environnement de test Azure.</span><span class="sxs-lookup"><span data-stu-id="415f7-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="415f7-109">Tandis que [Fiddler](http://www.telerik.com/fiddler) et [les outils de développement F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) fournissent un côté client vue Aperçu fournit une vue détaillée à partir du serveur.</span><span class="sxs-lookup"><span data-stu-id="415f7-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="415f7-110">Ce didacticiel aborde l’utilisation de l’aperçu ASP.NET MVC et les packages d’EF, mais de nombreux autres packages sont disponibles.</span><span class="sxs-lookup"><span data-stu-id="415f7-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="415f7-111">Lorsque cela est possible de lier sera à approprié [apercevoir docs](http://getglimpse.com/Docs/) dont j’ai aider à maintenir.</span><span class="sxs-lookup"><span data-stu-id="415f7-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="415f7-112">Aperçu est un projet open source, vous pouvez trop contribuer au code source et la documentation.</span><span class="sxs-lookup"><span data-stu-id="415f7-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>


- [<span data-ttu-id="415f7-113">L’installation d’aperçu</span><span class="sxs-lookup"><span data-stu-id="415f7-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="415f7-114">Activer l’aperçu pour localhost</span><span class="sxs-lookup"><span data-stu-id="415f7-114">Enable Glimpse for localhost</span></span>](#eg)
- [<span data-ttu-id="415f7-115">L’onglet de la chronologie</span><span class="sxs-lookup"><span data-stu-id="415f7-115">The Timeline tab</span></span>](#Time)
- [<span data-ttu-id="415f7-116">Liaison de données</span><span class="sxs-lookup"><span data-stu-id="415f7-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="415f7-117">Itinéraires</span><span class="sxs-lookup"><span data-stu-id="415f7-117">Routes</span></span>](#route)
- [<span data-ttu-id="415f7-118">À l’aide de Glimpse sur Azure</span><span class="sxs-lookup"><span data-stu-id="415f7-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="415f7-119">Ressources supplémentaires pour MSBuild</span><span class="sxs-lookup"><span data-stu-id="415f7-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="415f7-120">L’installation d’aperçu</span><span class="sxs-lookup"><span data-stu-id="415f7-120">Installing Glimpse</span></span>

<span data-ttu-id="415f7-121">Vous pouvez installer aperçu à partir de la console de gestionnaire de package NuGet ou de la **gérer les Packages NuGet** console.</span><span class="sxs-lookup"><span data-stu-id="415f7-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="415f7-122">Pour cette démonstration, j’installe les packages Mvc5 et EF6 :</span><span class="sxs-lookup"><span data-stu-id="415f7-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![installer l’aperçu à partir de NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="415f7-124">Recherchez *Glimpse.EF*</span><span class="sxs-lookup"><span data-stu-id="415f7-124">Search for *Glimpse.EF*</span></span>

![Glimpse.EF à partir de NuGet install dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="415f7-126">En sélectionnant **packages installés**, vous pouvez voir les modules dépendants aperçu installés :</span><span class="sxs-lookup"><span data-stu-id="415f7-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![Packages de Glimpse installés à partir de DLg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="415f7-128">Les commandes suivantes installent les modules de Glimpse MVC5 et EF6 à partir de la console du Gestionnaire de package :</span><span class="sxs-lookup"><span data-stu-id="415f7-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="415f7-129">Activer l’aperçu pour localhost</span><span class="sxs-lookup"><span data-stu-id="415f7-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="415f7-130">Accédez à http://localhost:&lt; port #&gt;/glimpse.axd et cliquez sur le <strong>activer un aperçu sur</strong> bouton.</span><span class="sxs-lookup"><span data-stu-id="415f7-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the <strong>Turn Glimpse On</strong> button.</span></span>

![Page d’aperçu axd](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="415f7-132">Si vous avez votre barre des favoris affiché, vous pouvez faire glisser et déposer les boutons d’aperçu et ajoutez-les en tant que bookmarklets :</span><span class="sxs-lookup"><span data-stu-id="415f7-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![Internet Explorer avec Glimpse boookmarklets](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="415f7-134">Vous pouvez désormais parcourir votre application et le **têtes d’affichage** (HUD) s’affiche en bas de la page.</span><span class="sxs-lookup"><span data-stu-id="415f7-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![Page Gestionnaire de contacts avec HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="415f7-136">Le [page d’aperçu HUD](http://getglimpse.com/Docs/Heads-up-Display) décrit en détail les informations de minutage indiquées ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="415f7-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="415f7-137">Les affichages de données le HUD performances discrète peuvent vous avertir d’un problème immédiatement - avant de commencer le cycle de test.</span><span class="sxs-lookup"><span data-stu-id="415f7-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="415f7-138">En cliquant sur le &quot;g&quot; dans le coin inférieur droit permet d’afficher le volet d’aperçu :</span><span class="sxs-lookup"><span data-stu-id="415f7-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Volet d’aperçu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="415f7-140">Dans l’image ci-dessus, le [onglet exécution](http://getglimpse.com/Docs/Execution-Tab) est sélectionnée, qui affiche les détails de minutage des actions et des filtres dans le pipeline.</span><span class="sxs-lookup"><span data-stu-id="415f7-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="415f7-141">Vous pouvez voir mon [minuteur de filtre d’arrêter un espion](http://www.nuget.org/packages/StopWatch/) commencent à l’étape 6 du pipeline.</span><span class="sxs-lookup"><span data-stu-id="415f7-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="415f7-142">Mon minuteur léger peut contribuer utile profil/synchronisation de données, il n’arrive pas tout le temps passé en matière d’autorisation et de rendu de l’affichage.</span><span class="sxs-lookup"><span data-stu-id="415f7-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="415f7-143">Vous pouvez lire sur mon minuterie à [Profiler et l’heure de votre application ASP.NET MVC jusqu'à Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span><span class="sxs-lookup"><span data-stu-id="415f7-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="415f7-144">Le [onglets](http://getglimpse.com/Docs/Tabs) page fournit des liens vers des informations détaillées sur chaque onglet.</span><span class="sxs-lookup"><span data-stu-id="415f7-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="415f7-145">L’onglet de la chronologie</span><span class="sxs-lookup"><span data-stu-id="415f7-145">The Timeline tab</span></span>

<span data-ttu-id="415f7-146">J’ai modifié de Tom Dykstra en suspens [didacticiel sur EF 6/MVC 5](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) par le code suivant, remplacez par le contrôleur instructors :</span><span class="sxs-lookup"><span data-stu-id="415f7-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="415f7-147">Le code ci-dessus me permet de passer dans la chaîne de requête (`eager`) au contrôle hâtif ou explicite le chargement de données.</span><span class="sxs-lookup"><span data-stu-id="415f7-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="415f7-148">Dans l’image ci-dessous, le chargement explicite est utilisé et la page de minutage affiche chaque inscription chargée dans le `Index` méthode d’action :</span><span class="sxs-lookup"><span data-stu-id="415f7-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![chargement explicite](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="415f7-150">Dans le code suivant, hâtif est spécifié, et chaque inscription est extrait après le `Index` vue est appelée :</span><span class="sxs-lookup"><span data-stu-id="415f7-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![Eager est spécifié.](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="415f7-152">Vous pouvez pointer sur un segment de temps pour obtenir des informations de minutage détaillées :</span><span class="sxs-lookup"><span data-stu-id="415f7-152">You can hover over a time segment to get detailed timing information:</span></span>

![pointage pour voir de temporisation détaillées](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="415f7-154">Liaison de modèle</span><span class="sxs-lookup"><span data-stu-id="415f7-154">Model Binding</span></span>

<span data-ttu-id="415f7-155">Le [onglet de liaison de modèle](http://getglimpse.com/Docs/Model-Binding-Tab) fournit une mine d’informations pour vous aider à comprendre comment vos variables de formulaire sont liés et pourquoi certaines ne sont pas liés comme peut l’attendre.</span><span class="sxs-lookup"><span data-stu-id="415f7-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="415f7-156">L’image ci-dessous montre le **?**</span><span class="sxs-lookup"><span data-stu-id="415f7-156">The image below shows the **?**</span></span> <span data-ttu-id="415f7-157">icône, vous pouvez cliquer sur pour afficher la page d’aide Aperçu pour cette fonctionnalité.</span><span class="sxs-lookup"><span data-stu-id="415f7-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![visualiser la vue de liaison de modèle](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="415f7-159">Routes</span><span class="sxs-lookup"><span data-stu-id="415f7-159">Routes</span></span>

 <span data-ttu-id="415f7-160">L’onglet Aperçu itinéraires sera peuvent vous aider à déboguer et comprendre le routage.</span><span class="sxs-lookup"><span data-stu-id="415f7-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="415f7-161">Dans l’image ci-dessous, l’itinéraire de produit est sélectionné (et elle est affichée en vert, une convention d’aperçu).</span><span class="sxs-lookup"><span data-stu-id="415f7-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="415f7-162">![nom du produit sélectionné](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) jetons contraintes, les zones et les données d’itinéraire sont également affichés.</span><span class="sxs-lookup"><span data-stu-id="415f7-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="415f7-163">Consultez [Glimpse itinéraires](http://getglimpse.com/Docs/Routes-Tab) et [routage par attributs dans ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) pour plus d’informations.</span><span class="sxs-lookup"><span data-stu-id="415f7-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="415f7-164">À l’aide de Glimpse sur Azure</span><span class="sxs-lookup"><span data-stu-id="415f7-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="415f7-165">La stratégie de sécurité d’aperçu par défaut autorise uniquement les données d’aperçu à afficher à partir de l’hôte local.</span><span class="sxs-lookup"><span data-stu-id="415f7-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="415f7-166">Vous pouvez modifier cette stratégie de sécurité pour pouvoir afficher ces données sur un serveur distant (par exemple, une application web sur Azure).</span><span class="sxs-lookup"><span data-stu-id="415f7-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="415f7-167">Pour les environnements de test sur Azure, ajouter la marque en surbrillance jusqu'à la partie inférieure de la *web.confg* fichier pour activer l’aperçu :</span><span class="sxs-lookup"><span data-stu-id="415f7-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.confg* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="415f7-168">Avec cette modification uniquement, n’importe quel utilisateur peut voir vos données d’aperçu sur un site distant.</span><span class="sxs-lookup"><span data-stu-id="415f7-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="415f7-169">Envisagez d’ajouter le balisage ci-dessus à un profil de publication afin qu’il a déployé uniquement un appliqués lorsque vous utilisez ce profil de publication (par exemple, votre proifle test Azure.) Pour restreindre les données d’aperçu, nous allons ajouter le `canViewGlimpseData` rôle et d’autoriser uniquement les utilisateurs à ce rôle pour afficher les données d’aperçu.</span><span class="sxs-lookup"><span data-stu-id="415f7-169">Consider adding the markup above to a publish profile so it's only deployed an applyed when you use that publish profile (for example, your Azure test proifle.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="415f7-170">Supprimez les commentaires de la *GlimpseSecurityPolicy.cs* du fichier et changer la [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) appeler à partir de `Administrator` à la `canViewGlimpseData` rôle :</span><span class="sxs-lookup"><span data-stu-id="415f7-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="415f7-171">Sécurité - données riches fournies par l’aperçu peut exposer la sécurité de votre application.</span><span class="sxs-lookup"><span data-stu-id="415f7-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="415f7-172">Microsoft n’a pas effectué un audit de sécurité de l’aperçu pour une utilisation sur les applications de production.</span><span class="sxs-lookup"><span data-stu-id="415f7-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>


<span data-ttu-id="415f7-173">Pour plus d’informations sur l’ajout de rôles, consultez mon [déployer une application web de Secure ASP.NET MVC 5 avec appartenance, OAuth et base de données SQL dans Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) didacticiel.</span><span class="sxs-lookup"><span data-stu-id="415f7-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="415f7-174">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="415f7-174">Additional Resources</span></span>

- [<span data-ttu-id="415f7-175">Déployer une application ASP.NET MVC 5 sécurisée avec appartenance, OAuth et base de données SQL dans Azure</span><span class="sxs-lookup"><span data-stu-id="415f7-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="415f7-176">[Configuration de glimpse](http://getglimpse.com/Docs/Configuration) -page de documentation sur la configuration des onglets, la stratégie runtime, journalisation et bien plus encore.</span><span class="sxs-lookup"><span data-stu-id="415f7-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
