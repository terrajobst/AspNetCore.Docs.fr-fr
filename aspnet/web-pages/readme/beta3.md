---
uid: web-pages/readme/beta3
title: "Matrice et ASP.NET Web Pages (Razor) version bêta 3 version Lisezmoi Web | Documents Microsoft"
author: rick-anderson
description: "Matrice Web et Lisezmoi de version bêta 3 ASP.NET Web Pages (Razor)"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/10/2011
ms.topic: article
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 5fad4b659dafe5470aeb84d320ff711b8840d1e0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a><span data-ttu-id="72a15-103">Matrice Web et Lisezmoi de version bêta 3 ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="72a15-103">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>
====================
> <span data-ttu-id="72a15-104">Matrice Web et Lisezmoi de version bêta 3 ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="72a15-104">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>

<span data-ttu-id="72a15-105">9 novembre 2010</span><span class="sxs-lookup"><span data-stu-id="72a15-105">9 November 2010</span></span>

## <a name="contents"></a><span data-ttu-id="72a15-106">Sommaire</span><span class="sxs-lookup"><span data-stu-id="72a15-106">Contents</span></span>

- [<span data-ttu-id="72a15-107">Vue d’ensemble</span><span class="sxs-lookup"><span data-stu-id="72a15-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="72a15-108">Installation</span><span class="sxs-lookup"><span data-stu-id="72a15-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="72a15-109">Nouvelles fonctionnalités, des modifications et des problèmes connus dans la version bêta 3</span><span class="sxs-lookup"><span data-stu-id="72a15-109">New Features, Changes, and Known Issues in the Beta 3 release</span></span>](#Known_Issues)

    - [<span data-ttu-id="72a15-110">Problèmes d’Installation de WebMatrix</span><span class="sxs-lookup"><span data-stu-id="72a15-110">WebMatrix Installation Issues</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="72a15-111">Pages Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="72a15-111">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="72a15-112">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="72a15-112">SQL Server Compact</span></span>](#Known_Issues_SQL_Server_Compact)
    - [<span data-ttu-id="72a15-113">L’installation d’Applications</span><span class="sxs-lookup"><span data-stu-id="72a15-113">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="72a15-114">Publication d’Applications</span><span class="sxs-lookup"><span data-stu-id="72a15-114">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
    - [<span data-ttu-id="72a15-115">Autres problèmes</span><span class="sxs-lookup"><span data-stu-id="72a15-115">Other Issues</span></span>](#Known_Issues_Other_Issues)
- [<span data-ttu-id="72a15-116">Pour plus d’informations</span><span class="sxs-lookup"><span data-stu-id="72a15-116">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="72a15-117">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="72a15-117">Overview</span></span>

> <span data-ttu-id="72a15-118">Microsoft WebMatrix bêta est une pile de développement web gratuit qui installe en quelques minutes.</span><span class="sxs-lookup"><span data-stu-id="72a15-118">Microsoft WebMatrix Beta is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="72a15-119">Elle intègre un serveur web avec la base de données et de la programmation des infrastructures pour créer une expérience intégrée unique.</span><span class="sxs-lookup"><span data-stu-id="72a15-119">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="72a15-120">Vous pouvez utiliser la version bêta de WebMatrix pour simplifier la manière dont vous coder, testerez et publiez vos propres du site Web ASP.NET ou PHP ou bêta de WebMatrix vous permet de démarrer un nouveau site Web à l’aide d’applications open source populaires telles que DotNetNuke, Umbraco, WordPress ou Joomla.</span><span class="sxs-lookup"><span data-stu-id="72a15-120">You can use WebMatrix Beta to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix Beta to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="72a15-121">Version bêta de WebMatrix utilise le même serveur web puissant, le moteur de base de données et infrastructures que ceux qui s’exécutera votre site Web sur internet, ce qui rend la phase de développement jusqu'à la production fluide et homogène.</span><span class="sxs-lookup"><span data-stu-id="72a15-121">WebMatrix Beta uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="72a15-122">Installation</span><span class="sxs-lookup"><span data-stu-id="72a15-122">Installation</span></span>

> <span data-ttu-id="72a15-123">Pour installer la version bêta 3 de WebMatrix, vous utilisez [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="72a15-123">To install WebMatrix Beta 3, you use [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="72a15-124">Après avoir installé Web Platform Installer, vous pouvez l’utiliser pour installer la version bêta 3 de WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="72a15-124">After you've installed the Web Platform Installer, you can use it to install WebMatrix Beta 3.</span></span>
> 
> <span data-ttu-id="72a15-125">Si vous rencontrez des problèmes lors de l’installation, reportez-vous à [, résolution des problèmes avec Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="72a15-125">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a><span data-ttu-id="72a15-126">Instructions relatives à la publication d’Applications</span><span class="sxs-lookup"><span data-stu-id="72a15-126">Instructions for Publishing Applications</span></span>

> <span data-ttu-id="72a15-127">Consultez [des Instructions détaillées pour la publication d’Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="72a15-127">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a><span data-ttu-id="72a15-128">Nouveaux problèmes andKnown de fonctionnalités, les changements</span><span class="sxs-lookup"><span data-stu-id="72a15-128">New Features, Changes, andKnown Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a><span data-ttu-id="72a15-129">Installation de WebMatrix bêta 3</span><span class="sxs-lookup"><span data-stu-id="72a15-129">WebMatrix Beta 3 Installation</span></span>

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="72a15-130">Problème : Version bêta 3 de WebMatrix est disponible uniquement sur les plateformes qui prennent en charge de Microsoft .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="72a15-130">Issue: WebMatrix Beta 3 is only available on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="72a15-131">Le .NET Framework version 4 est requis pour la version bêta de WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="72a15-131">The .NET Framework version 4 is required for WebMatrix Beta.</span></span> <span data-ttu-id="72a15-132">Dans certains cas, le programme d’installation de la version bêta de WebMatrix vous permet d’essayer d’installer sur une plateforme qui ne fait pas partie de l’ensemble de la configuration prise en charge.</span><span class="sxs-lookup"><span data-stu-id="72a15-132">In certain cases, the WebMatrix Beta installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="72a15-133">En particulier, Windows Vista sans la mise à jour SP1 vous permet de commencer l’installation de WebMatrix bêta, mais le composant .NET Framework 4 échoue et bloquer l’installation.</span><span class="sxs-lookup"><span data-stu-id="72a15-133">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix Beta, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="72a15-134">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="72a15-134">**Workaround**</span></span>  
> <span data-ttu-id="72a15-135">Installer sur une plateforme prise en charge, notamment :</span><span class="sxs-lookup"><span data-stu-id="72a15-135">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="72a15-136">Windows 7</span><span class="sxs-lookup"><span data-stu-id="72a15-136">Windows 7</span></span>
> - <span data-ttu-id="72a15-137">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="72a15-137">Windows Server 2008</span></span>
> - <span data-ttu-id="72a15-138">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="72a15-138">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="72a15-139">Windows Vista SP1 ou ultérieur</span><span class="sxs-lookup"><span data-stu-id="72a15-139">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="72a15-140">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="72a15-140">Windows XP SP3</span></span>
> - <span data-ttu-id="72a15-141">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="72a15-141">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="72a15-142">Problème : Ne peut pas installer WebMatrix bêta 3 si Microsoft Visual Studio 2008 est installé sans Microsoft Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="72a15-142">Issue: Cannot install WebMatrix Beta 3 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="72a15-143">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="72a15-143">**Workaround**</span></span>  
> <span data-ttu-id="72a15-144">Installer [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) à partir du centre de téléchargement Microsoft.</span><span class="sxs-lookup"><span data-stu-id="72a15-144">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="72a15-145">Problème : Certains assemblys pour SQL Server Compact 4.0 ne sont pas installés dans le GAC</span><span class="sxs-lookup"><span data-stu-id="72a15-145">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="72a15-146">Les assemblys managés pour SQL Server Compact 4.0 ne sont pas placés dans le global assembly cache (GAC) lorsque vous installez SQL Server Compact 4.0 sur un ordinateur 64 bits et de l’ordinateur dispose uniquement du .NET Framework 3.5 SP1 Client Profile installé.</span><span class="sxs-lookup"><span data-stu-id="72a15-146">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="72a15-147">Les assemblys managés qui ne sont pas installés dans le GAC sont :</span><span class="sxs-lookup"><span data-stu-id="72a15-147">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="72a15-148">*System.Data.SqlServerCe.dll* (fournisseur ADO.NET)</span><span class="sxs-lookup"><span data-stu-id="72a15-148">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="72a15-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)</span><span class="sxs-lookup"><span data-stu-id="72a15-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="72a15-150">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="72a15-150">**Workaround**</span></span>  
> <span data-ttu-id="72a15-151">Désinstaller SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="72a15-151">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="72a15-152">Téléchargez et installez la version complète de .NET Framework 3.5 SP1 à partir de l’emplacement suivant :</span><span class="sxs-lookup"><span data-stu-id="72a15-152">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="72a15-153">Microsoft .NET Framework 3.5 Service pack 1 (Package complet)</span><span class="sxs-lookup"><span data-stu-id="72a15-153">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="72a15-154">Ensuite, réinstallez SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="72a15-154">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="72a15-155">Problème : Impossible de désinstaller SQL Server Compact à l’aide de la ligne de commande</span><span class="sxs-lookup"><span data-stu-id="72a15-155">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="72a15-156">La désinstallation de SQL Server Compact à l’aide des options de ligne de commande ne fonctionne pas dans cette version.</span><span class="sxs-lookup"><span data-stu-id="72a15-156">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="72a15-157">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="72a15-157">**Workaround**</span></span>  
> <span data-ttu-id="72a15-158">Utilisez *programmes et fonctionnalités* dans le panneau de configuration Windows pour désinstaller Microsoft SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="72a15-158">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="72a15-159">Pages web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="72a15-159">ASP.NET Web Pages</span></span>

<span data-ttu-id="72a15-160">Cette section du document décrit les nouvelles fonctionnalités, des modifications et des problèmes connus avec la version bêta 3 d’ASP.NET Web Pages avec syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="72a15-160">This section of the document describes new features, changes, and known issues with the Beta 3 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="72a15-161">Nouvelles fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="72a15-161">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="72a15-162">Modifications</span><span class="sxs-lookup"><span data-stu-id="72a15-162">Changes</span></span>](#Changes)
- [<span data-ttu-id="72a15-163">Problèmes</span><span class="sxs-lookup"><span data-stu-id="72a15-163">Issues</span></span>](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="72a15-164">Nouvelles fonctionnalités dans la version bêta 3 pour ASP.NET Web Pages avec syntaxe Razor</span><span class="sxs-lookup"><span data-stu-id="72a15-164">New Features in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a><span data-ttu-id="72a15-165">Nouveau : « Html.Raw » méthode restitue le balisage non codé</span><span class="sxs-lookup"><span data-stu-id="72a15-165">New: "Html.Raw" method renders unencoded markup</span></span>

> <span data-ttu-id="72a15-166">La nouvelle `Html.Raw` méthode vous permet de restituer le balisage HTML sous forme de balisage au lieu de rendu en sortie encodée.</span><span class="sxs-lookup"><span data-stu-id="72a15-166">The new `Html.Raw` method lets you render HTML markup as markup instead of rendering encoded output.</span></span> <span data-ttu-id="72a15-167">(Par défaut, ASP.NET Razor encode des chaînes avant leur rendu.) La syntaxe est :</span><span class="sxs-lookup"><span data-stu-id="72a15-167">(By default, ASP.NET Razor encodes strings before rendering them.) The syntax is:</span></span>
> 
> `Html.Raw(value)`
> 
> <span data-ttu-id="72a15-168">L'exemple suivant montre comment utiliser `Html.Raw` :</span><span class="sxs-lookup"><span data-stu-id="72a15-168">The following example shows how to use `Html.Raw`:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="72a15-169">Modifications dans la version bêta 3 pour ASP.NET Web Pages avec syntaxe Razor</span><span class="sxs-lookup"><span data-stu-id="72a15-169">Changes in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="change-hrefattribute-method-removed"></a><span data-ttu-id="72a15-170">Change : Méthode de « HrefAttribute » supprimée</span><span class="sxs-lookup"><span data-stu-id="72a15-170">Change: "HrefAttribute" method removed</span></span>

> <span data-ttu-id="72a15-171">Le `HrefAttribute` méthode de la `WebPage` classe a été supprimée.</span><span class="sxs-lookup"><span data-stu-id="72a15-171">The `HrefAttribute` method of the `WebPage` class has been removed.</span></span> <span data-ttu-id="72a15-172">Ce programme d’assistance a été utilisé pour encoder des caractères non sécurisés dans les URL.</span><span class="sxs-lookup"><span data-stu-id="72a15-172">This helper was used to encode unsafe characters in URLs.</span></span> <span data-ttu-id="72a15-173">Il n’est plus nécessaire, car ASP.NET Razor encode automatiquement les chaînes.</span><span class="sxs-lookup"><span data-stu-id="72a15-173">It is no longer required because ASP.NET Razor automatically encodes strings.</span></span> <span data-ttu-id="72a15-174">(Utilisez la nouvelle `Html.Raw` méthode pour restituer des chaînes non codés.)</span><span class="sxs-lookup"><span data-stu-id="72a15-174">(Use the new `Html.Raw` method to render unencoded strings.)</span></span>


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a><span data-ttu-id="72a15-175">Modification : Syntaxe déclarative «@helper» les programmes d’assistance modifiés</span><span class="sxs-lookup"><span data-stu-id="72a15-175">Change: Syntax for declarative "@helper" helpers changed</span></span>

> <span data-ttu-id="72a15-176">Dans la version bêta 3, ASP.NET modifie la façon dont il analyse les programmes d’assistance qui sont créés à l’aide de la `@helper` syntaxe.</span><span class="sxs-lookup"><span data-stu-id="72a15-176">In the Beta 3 release, ASP.NET changes how it parses helpers that are created using the `@helper` syntax.</span></span> <span data-ttu-id="72a15-177">Fondamentalement, le `@helper` syntaxe est maintenant analysée comme un bloc de code et non comme un bloc de balisage qui peut inclure du code.</span><span class="sxs-lookup"><span data-stu-id="72a15-177">In essence, the `@helper` syntax is now parsed as a code block instead of as a block of markup that can include code.</span></span> <span data-ttu-id="72a15-178">Par conséquent, code à l’intérieur de l’application d’assistance n’a pas besoin d’être mises entre `@{ }` blocs.</span><span class="sxs-lookup"><span data-stu-id="72a15-178">Therefore, code inside the helper does not need to be enclosed in `@{ }` blocks.</span></span> <span data-ttu-id="72a15-179">À l’inverse, le balisage à l’intérieur de l’application d’assistance doit être explicitement inclus dans les éléments HTML ou ASP.NET Razor `<text></text>` balises.</span><span class="sxs-lookup"><span data-stu-id="72a15-179">Conversely, markup inside the helper has to be explicitly included in HTML elements or in ASP.NET Razor `<text></text>` tags.</span></span>
> 
> <span data-ttu-id="72a15-180">Par exemple, les éléments suivants `@helper` fonctionne de la syntaxe dans la version bêta 3 :</span><span class="sxs-lookup"><span data-stu-id="72a15-180">For example, the following `@helper` syntax works in the Beta 3 release:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> <span data-ttu-id="72a15-181">Dans la version bêta 3, ce programme d’assistance doit être modifiée pour ressembler à l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="72a15-181">In the Beta 3 release, this helper must be changed to look like the following example:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> <span data-ttu-id="72a15-182">Notez que les `@{ }` caractères autour du code initial dans l’application d’assistance n’est plus utilisé.</span><span class="sxs-lookup"><span data-stu-id="72a15-182">Notice that the `@{ }` characters around the initial code in the helper is no longer used.</span></span> <span data-ttu-id="72a15-183">Il s’agit, car le contenu des programmes d’assistance est traité comme un bloc de code par défaut.</span><span class="sxs-lookup"><span data-stu-id="72a15-183">This is because the contents of the helpers are treated as a code block by default.</span></span> <span data-ttu-id="72a15-184">L’application d’assistance restitue la balise, qui commence à l’ouverture `<a>` balise.</span><span class="sxs-lookup"><span data-stu-id="72a15-184">The helper renders markup, which starts with the opening `<a>` tag.</span></span> <span data-ttu-id="72a15-185">Si l’application d’assistance doit effectuer le rendu de texte brut ou les balises qui n’incluent pas de balise de fermeture (par exemple, `<meta>` balises), le contenu à restituer doit être dans `<text></text>` balises.</span><span class="sxs-lookup"><span data-stu-id="72a15-185">If the helper must render plain text or tags that do not include a closing tag (for example, `<meta>` tags), the content to be rendered must be in `<text></text>` tags.</span></span>


#### <a name="change-webpagecontexthttpcontext-removed"></a><span data-ttu-id="72a15-186">Modifications : « WebPageContext.HttpContext » supprimées</span><span class="sxs-lookup"><span data-stu-id="72a15-186">Change: "WebPageContext.HttpContext" removed</span></span>

> <span data-ttu-id="72a15-187">Le `WebPageContext.HttpContext` propriété a été supprimée.</span><span class="sxs-lookup"><span data-stu-id="72a15-187">The `WebPageContext.HttpContext` property has been removed.</span></span> <span data-ttu-id="72a15-188">Utilisez plutôt `HttpContext.Current` .</span><span class="sxs-lookup"><span data-stu-id="72a15-188">Use `HttpContext.Current` instead.</span></span> <span data-ttu-id="72a15-189">(Le `WebPageContext.HttpContext` propriété simplement encapsulée cela.)</span><span class="sxs-lookup"><span data-stu-id="72a15-189">(The `WebPageContext.HttpContext` property simply wrapped this.)</span></span>


#### <a name="change-facebook-helper-moved-to-new-package"></a><span data-ttu-id="72a15-190">Modification : Application auxiliaire « Facebook » déplacé vers le nouveau package</span><span class="sxs-lookup"><span data-stu-id="72a15-190">Change: "Facebook" helper moved to new package</span></span>

> <span data-ttu-id="72a15-191">Le `Facebook` assistance a été déplacé vers le *Facebook.Helper* library, qui comprend le `Facebook` d’assistance et des fonctionnalités supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="72a15-191">The `Facebook` helper has been moved to the *Facebook.Helper* library, which includes the `Facebook` helper and additional functionality.</span></span> <span data-ttu-id="72a15-192">Vous devez installer cette bibliothèque en tant que package distinct, comme décrit dans « L’installation de programmes d’assistance avec Package Manager » dans le didacticiel [prise en main de Pages ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).</span><span class="sxs-lookup"><span data-stu-id="72a15-192">You must install this library as a separate package, as described in "Installing Helpers with Package Manager" in the tutorial [Getting Started with ASP.NET Pages](https://go.microsoft.com/fwlink/?LinkId=202889).</span></span>


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a><span data-ttu-id="72a15-193">Modification : Types d’appartenance, rôles et la sécurité se déplace vers le nouvel assembly</span><span class="sxs-lookup"><span data-stu-id="72a15-193">Change: Membership, Role, and Security types moves to new assembly</span></span>

> <span data-ttu-id="72a15-194">Les types suivants ont été déplacées vers le `WebMatrix.WebData` assembly :</span><span class="sxs-lookup"><span data-stu-id="72a15-194">The following types were moved to the `WebMatrix.WebData` assembly:</span></span>
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a><span data-ttu-id="72a15-195">Change : Classe de « TagBuilder » déplacé vers l’assembly de System.Web.WebPages.dll</span><span class="sxs-lookup"><span data-stu-id="72a15-195">Change: "TagBuilder" class moved to System.Web.WebPages.dll assembly</span></span>

> <span data-ttu-id="72a15-196">La `TagBuilde` classe r a été déplacé vers l’assembly System.Web.WebPages.dll.</span><span class="sxs-lookup"><span data-stu-id="72a15-196">The `TagBuilde` r class has been moved to the System.Web.WebPages.dll assembly.</span></span> <span data-ttu-id="72a15-197">Auparavant, il était dans un assembly qui faisait partie d’ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="72a15-197">Previously, this was in an assembly that was part of ASP.NET MVC.</span></span> <span data-ttu-id="72a15-198">Cette modification signifie que vous n’avez pas à installer ASP.NET MVC pour pouvoir utiliser la `TagBuilder` classe.</span><span class="sxs-lookup"><span data-stu-id="72a15-198">This change means that you do not have to install ASP.NET MVC in order to use the `TagBuilder` class.</span></span>
> 
> <span data-ttu-id="72a15-199">Toutefois, la classe est toujours dans le `System.Web.Mvc` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="72a15-199">However, the class is still in the `System.Web.Mvc` namespace.</span></span> <span data-ttu-id="72a15-200">Pour pouvoir utiliser le `TagBuilder` classe (par exemple, dans une assistance ASP.NET Razor personnalisé), vous devez référencer l’espace de noms (par exemple, en ajoutant `@using System.Web.Mvc` à votre code).</span><span class="sxs-lookup"><span data-stu-id="72a15-200">In order to use the `TagBuilder` class (for example, in a custom ASP.NET Razor helper), you must reference the namespace (for example, by adding `@using System.Web.Mvc` to your code).</span></span>


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a><span data-ttu-id="72a15-201">Modification : Demande syntaxe de validation modifié ; Classe de « Validation » supprimé</span><span class="sxs-lookup"><span data-stu-id="72a15-201">Change: Request validation syntax changed; "Validation" class removed</span></span>

> <span data-ttu-id="72a15-202">Dans la version bêta 3, pour désactiver la validation pour un champ individuel ou un ensemble de champs, que vous pouvez appeler la `Validation.Exclude` méthode, en passant l’ou les noms des champs à exclure de la validation.</span><span class="sxs-lookup"><span data-stu-id="72a15-202">In the Beta 3 release, to disable validation for an individual field or set of fields, you can call the `Validation.Exclude` method, passing in the name or names of the fields to exclude from validation.</span></span> <span data-ttu-id="72a15-203">Une nouvelle syntaxe est disponible dans la version bêta 3 pour contourner la validation.</span><span class="sxs-lookup"><span data-stu-id="72a15-203">A new syntax is available in the Beta 3 release for bypassing validation.</span></span> <span data-ttu-id="72a15-204">Le `Validation` méthode utilisée dans la version bêta 3 a été supprimé.</span><span class="sxs-lookup"><span data-stu-id="72a15-204">The `Validation` method used in Beta 3 has been removed.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="72a15-205">Si vous ne désactivez pas la validation de la demande, si des utilisateurs tentent de télécharger un balisage HTML (par exemple, en utilisant un éditeur de texte sur une page), le site Web signale une erreur comme *une valeur Request.Form potentiellement dangereuse a été détectée à partir du client*et l’entrée d’utilisateur n’est pas acceptée.</span><span class="sxs-lookup"><span data-stu-id="72a15-205">If you do not disable request validation, if users try to upload HTML markup (for example, by using a rich text editor on a page), the website will report an error like *A potentially dangerous Request.Form value was detected from the client* and the user input is not accepted.</span></span> <span data-ttu-id="72a15-206">Si vous désactivez la validation de la demande, vous devez vérifier manuellement l’entrée d’utilisateur pour vous assurer qu’il ne contient pas balisage potentiellement dangereux ou générer un script à l’aide d’un nom tel que le [Microsoft entre un Site Scripting bibliothèque V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span><span class="sxs-lookup"><span data-stu-id="72a15-206">If you disable request validation, you must manually check user input to make sure that it does not contain potentially dangerous markup or script using something like the [Microsoft Anti-Cross Site Scripting Library V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span></span>
> 
> 
> <span data-ttu-id="72a15-207">Pour désactiver la validation de la demande automatique, appelez le `Request.Unvalidated` méthode, en lui passant le nom du champ ou l’autre objet de publication que vous souhaitez ignorer la validation de demande pour.</span><span class="sxs-lookup"><span data-stu-id="72a15-207">To disable automatic request validation, call the `Request.Unvalidated` method, passing it the name of the field or other post object that you want to bypass request validation for.</span></span> <span data-ttu-id="72a15-208">Vous pouvez utiliser cette méthode pour ignorer la validation pour tous les éléments de la `Form`, `QueryString`, `Cookies`, et `ServerVariables` collections.</span><span class="sxs-lookup"><span data-stu-id="72a15-208">You can use this method to bypass validation for any items in the `Form`, `QueryString`, `Cookies`, and `ServerVariables` collections.</span></span> <span data-ttu-id="72a15-209">Les exemples suivants montrent comment utiliser le `Unvalidated` méthode :</span><span class="sxs-lookup"><span data-stu-id="72a15-209">The following examples show how to use the `Unvalidated` method:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="72a15-210">Problèmes connus relatifs à ASP.NET Web Pages avec syntaxe Razor</span><span class="sxs-lookup"><span data-stu-id="72a15-210">Known Issues for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="72a15-211">Problème : Un comportement inattendu lors de l’utilisation d’une table utilisateur personnalisée pour l’appartenance</span><span class="sxs-lookup"><span data-stu-id="72a15-211">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="72a15-212">Pour initialiser le fournisseur d’appartenances pour un site Web ASP.NET Razor, vous appelez le `WebSecurity.InitializeDatabaseConnection` (méthode).</span><span class="sxs-lookup"><span data-stu-id="72a15-212">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="72a15-213">(Dans WebMatrix, du modèle Starter Site inclut un appel à cette méthode dans le  *\_AppStart.cshtml* fichier.) Si le `autoCreateTables` paramètre de cette méthode est défini sur true (par défaut, il est défini true dans le modèle Starter Site), et si un nom de table non reconnu est passé à la méthode (le deuxième paramètre), la méthode ne lève pas d’une erreur.</span><span class="sxs-lookup"><span data-stu-id="72a15-213">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="72a15-214">Au lieu de cela, il crée automatiquement la table.</span><span class="sxs-lookup"><span data-stu-id="72a15-214">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="72a15-215">Cela peut poser un problème si vous envisagez d’utiliser une table utilisateur personnalisée pour l’appartenance mais passez le nom de table incorrect pour le `WebSecurity.InitializeDatabaseConnection` (méthode).</span><span class="sxs-lookup"><span data-stu-id="72a15-215">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="72a15-216">Étant donné que la méthode ne pas par défaut lève une erreur si la table que vous spécifiez n’existe pas, et parce qu’il crée à la place d’une nouvelle table, l’application peut apparaître pour travailler.</span><span class="sxs-lookup"><span data-stu-id="72a15-216">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="72a15-217">Toutefois, code d’application qui s’appuie sur la table d’utilisateur personnalisée (et sur les champs qu’elle contient) peut éventuellement échouer avec des erreurs inattendues.</span><span class="sxs-lookup"><span data-stu-id="72a15-217">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="72a15-218">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="72a15-218">**Workaround**</span></span>  
> <span data-ttu-id="72a15-219">Assurez-vous que le nom est passé dans le `InitializeDatabaseConnection` correspondances méthode le profil utilisateur dans la base de données d’appartenance de table, ou assurez-vous que le `autoCreateTables` paramètre est défini sur false.</span><span class="sxs-lookup"><span data-stu-id="72a15-219">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="72a15-220">Problème : erreur « Échec de générer une instance utilisateur de SQL Server »</span><span class="sxs-lookup"><span data-stu-id="72a15-220">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="72a15-221">Si une application Web de WebMatrix utilise SQL Server Express et IIS 7.5 sur Windows 7 ou Windows Server 2008 R2, vous pouvez voir une erreur indiquant que SQL Server ne peut pas récupérer le chemin d’accès de l’utilisateur local d’application en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="72a15-221">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="72a15-222">**Solution de contournement** vous assurer que le compte Windows que l’application s’exécute sous (en général, le SERVICE réseau) dispose des autorisations en lecture/écriture pour les dossiers racine de l’application et des sous-dossiers comme *application\_données*.</span><span class="sxs-lookup"><span data-stu-id="72a15-222">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="72a15-223">Informations plus détaillées sont disponibles dans l’article de la base de connaissances [des problèmes avec les projets d’Application Web ASP.net et de création d’instances SQL Server Express utilisateur](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="72a15-223">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a><span data-ttu-id="72a15-224">Problème : Dans Visual Studio, espaces de noms pour les assemblys personnalisés (DLL) ne sont pas importés automatiquement</span><span class="sxs-lookup"><span data-stu-id="72a15-224">Issue: In Visual Studio, namespaces for custom assemblies (DLLs) are not imported automatically</span></span>

> <span data-ttu-id="72a15-225">Si vous utilisez des assemblys personnalisés dans un projet dans Visual Studio, les espaces de noms déclarés dans ces assemblys ne sont pas importés automatiquement au moment du design.</span><span class="sxs-lookup"><span data-stu-id="72a15-225">If you use custom assemblies in a project in Visual Studio, the namespaces declared in those assemblies are not automatically imported at design time.</span></span> <span data-ttu-id="72a15-226">Par conséquent, les références aux types personnalisés peuvent ne pas être reconnus au moment du design et sont marqués comme non reconnu dans Visual Studio (en utilisant un tilde « »).</span><span class="sxs-lookup"><span data-stu-id="72a15-226">As a result, references to custom types might not be recognized at design time and are marked as not recognized in Visual Studio (using a "squiggle").</span></span> <span data-ttu-id="72a15-227">Ce problème se produit uniquement au moment du design dans Visual Studio ; l’application s’exécute correctement.</span><span class="sxs-lookup"><span data-stu-id="72a15-227">This problem occurs only at design time in Visual Studio; the application itself runs properly.</span></span>
> 
> <span data-ttu-id="72a15-228">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="72a15-228">**Workaround**</span></span>  
> <span data-ttu-id="72a15-229">Inclure un `using` instruction (`imports` en Visual Basic) qui fait référence à des entités qui ne sont pas reconnues au moment du design.</span><span class="sxs-lookup"><span data-stu-id="72a15-229">Include a `using` statement (`imports` in Visual Basic) that references the entities that are not recognized at design time.</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="72a15-230">Problème : Visual Studio IntelliSense modèles de projet et disponibles uniquement dans ASP.NET MVC version 3</span><span class="sxs-lookup"><span data-stu-id="72a15-230">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="72a15-231">Installation de ASP.NET Web Pages ne pas également installe outils pour Visual Studio telles que des modèles de projet et IntelliSense pour les applications ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="72a15-231">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="72a15-232">**Solution de contournement** pour utiliser des modèles de projet et IntelliSense pour les applications ASP.NET Web Pages dans Visual Studio, installez ASP.NET MVC 3 RC via Web Platform Installer ou [programme d’installation autonome](https://go.microsoft.com/fwlink/?LinkID=191797).</span><span class="sxs-lookup"><span data-stu-id="72a15-232">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a><span data-ttu-id="72a15-233">Problème : «&lt;assistance&gt; Impossible de trouver la classe « erreur</span><span class="sxs-lookup"><span data-stu-id="72a15-233">Issue: "&lt;helper&gt; class cannot be found" error</span></span>

> <span data-ttu-id="72a15-234">Une fois que vous mettez à niveau vers la version bêta 3, vous pouvez voir une erreur qui une classe d’assistance (par exemple, la `Facebook` classe) est introuvable.</span><span class="sxs-lookup"><span data-stu-id="72a15-234">After you upgrade to Beta 3, you might see an error that a helper class (for example, the `Facebook` class) cannot not be found.</span></span> <span data-ttu-id="72a15-235">À compter de la version bêta 2 et en continuant à la bêta 3, les programmes d’assistance ont été déplacés vers les packages que vous devez installer explicitement.</span><span class="sxs-lookup"><span data-stu-id="72a15-235">Starting in Beta 2 and continuing in Beta 3, helpers have been moved to packages that you must explicitly install.</span></span> <span data-ttu-id="72a15-236">Les sites existants ne sont pas mis à niveau afin d’inclure ces packages ; Cela inclut les sites de la *\My Documents\IISExpress* ou *\My Documents\My Sites Web* dossiers.</span><span class="sxs-lookup"><span data-stu-id="72a15-236">Existing sites are not upgraded to include these packages; this includes sites in the *\My Documents\IISExpress* or *\My Documents\My Web Sites* folders.</span></span> <span data-ttu-id="72a15-237">En particulier, vous recevez cette erreur si vous utilisez le site par défaut dans *Mes Sites* (WebSite1), qui inclut une référence à la `Twitter` helper.</span><span class="sxs-lookup"><span data-stu-id="72a15-237">In particular, you will see this error if you use the default site in *My Sites* (WebSite1), which includes a reference to the `Twitter` helper.</span></span>
> 
> <span data-ttu-id="72a15-238">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="72a15-238">**Workaround**</span></span>  
> <span data-ttu-id="72a15-239">Commentez les appels à des programmes d’assistance dans le site, exécutez le  *\_Admin* page et installer l’ou les packages qui incluent les programmes d’assistance que vous souhaitez utiliser.</span><span class="sxs-lookup"><span data-stu-id="72a15-239">Comment out calls to any helpers in the site, run the *\_Admin* page, and install the package or packages that include the helpers that you want to use.</span></span> <span data-ttu-id="72a15-240">Une fois que vous avez installé le package, vous pouvez ne pas commenter les lignes qui référencent des programmes d’assistance.</span><span class="sxs-lookup"><span data-stu-id="72a15-240">After you've installed the package, you can uncomment the lines that reference helpers.</span></span>


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a><span data-ttu-id="72a15-241">Problème : Déployer les assemblys de la version bêta 3 ASP.NET Razor dans le dossier Bin ne fonctionnent pas sur l’hébergement de sites</span><span class="sxs-lookup"><span data-stu-id="72a15-241">Issue: Deploying Beta 3 ASP.NET Razor assemblies to the Bin folder might not work on hosting sites</span></span>

> <span data-ttu-id="72a15-242">Si vous déployez un site Web ASP.NET Web Pages dans un site d’hébergement, et si vous déployez les assemblys ASP.NET Razor bêta 3 pour le site *Bin* dossier, vous risquez de rencontrer des erreurs, y compris les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="72a15-242">If you deploy an ASP.NET Web Pages website to a hosting site, and if you deploy the ASP.NET Razor Beta 3 assemblies to the site's *Bin* folder, you might experience errors, including the following:</span></span>
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> <span data-ttu-id="72a15-243">Cela peut se produire si le fournisseur d’hébergement a installé les assemblys ASP.NET Web Pages bêta 1 dans le cache d’application globale du serveur (GAC).</span><span class="sxs-lookup"><span data-stu-id="72a15-243">This can happen if the hosting provider has installed the ASP.NET Web Pages Beta 1 assemblies into the server's global application cache (GAC).</span></span> <span data-ttu-id="72a15-244">Assemblys dans le GAC obtenir la priorité sur les assemblys installés localement dans le *Bin* dossier.</span><span class="sxs-lookup"><span data-stu-id="72a15-244">Assemblies in the GAC get precedence over assemblies installed locally in the *Bin* folder.</span></span>
> 
> <span data-ttu-id="72a15-245">**Solution de contournement** contactez votre fournisseur d’hébergement pour confirmer que les erreurs que vous voyez sont en raison d’un conflit entre les versions du fournisseur des assemblys et que le vôtre.</span><span class="sxs-lookup"><span data-stu-id="72a15-245">**Workaround** Contact your hosting provider to confirm that the errors you are seeing are due to a conflict between the provider's versions of the assemblies and yours.</span></span> <span data-ttu-id="72a15-246">Dans ce cas, demander que le fournisseur d’hébergement mettre à jour les assemblys dans le GAC de celle du serveur.</span><span class="sxs-lookup"><span data-stu-id="72a15-246">If so, request that the hosting provider update the assemblies in the server's GAC.</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="72a15-247">Problème : La lecture des flux ou autres données externes via un serveur proxy</span><span class="sxs-lookup"><span data-stu-id="72a15-247">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="72a15-248">Si le serveur qui exécute le site est derrière un serveur proxy, vous devrez peut-être configurer les informations de proxy dans le *Web.config* fichier afin d’être en mesure de lire les informations provenant de l’extérieur de votre site.</span><span class="sxs-lookup"><span data-stu-id="72a15-248">If the server running the site is behind a proxy server, you might need to configure proxy information in the *Web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="72a15-249">Par exemple, si vous utilisez la `ReCaptcha` assistance, le programme d’assistance communique avec le service reCAPTCHA, mais peut être bloquée par votre serveur proxy.</span><span class="sxs-lookup"><span data-stu-id="72a15-249">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="72a15-250">De même, les flux sont utilisés dans ASP.NET Web Pages, telles que le flux utilisé par le Gestionnaire de package, peuvent requérir une configuration proxy.</span><span class="sxs-lookup"><span data-stu-id="72a15-250">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="72a15-251">Si vous rencontrez des problèmes dans l’utilisation d’un service externe ou utiliser le package de flux, placez les éléments suivants dans la racine de votre application *Web.config* fichier :</span><span class="sxs-lookup"><span data-stu-id="72a15-251">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *Web.config* file:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> <span data-ttu-id="72a15-252">Pour plus d’informations sur la configuration d’un serveur proxy, consultez [ &lt;proxy&gt; élément (paramètres réseau)](https://msdn.microsoft.com/en-us/library/sa91de1e.aspx) sur le site Web MSDN.</span><span class="sxs-lookup"><span data-stu-id="72a15-252">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/en-us/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a><span data-ttu-id="72a15-253">Problème : Erreur « Impossible de charger Microsoft.Web.Infrastructure.dll »</span><span class="sxs-lookup"><span data-stu-id="72a15-253">Issue: "Microsoft.Web.Infrastructure.dll cannot be loaded" error</span></span>

> <span data-ttu-id="72a15-254">Si vous déjà installé la version bêta 1 de ASP.NET Web Pages avec syntaxe Razor et puis installez la version bêta 3, tous les assemblys appropriés sont installés dans le GAC, à l’exception *Microsoft.Web.Infrastructure.dll*.</span><span class="sxs-lookup"><span data-stu-id="72a15-254">If you previously installed the Beta 1 version of ASP.NET Web Pages with Razor syntax and then install the Beta 3 version, all appropriate assemblies are installed in the GAC except *Microsoft.Web.Infrastructure.dll*.</span></span> <span data-ttu-id="72a15-255">Par conséquent, lorsque vous exécutez des pages ASP.NET Razor, vous voyez une erreur qui indique que *Microsoft.Web.Infrastructure.dll* pas pu être chargé.</span><span class="sxs-lookup"><span data-stu-id="72a15-255">As a consequence, when you run ASP.NET Razor pages, you see an error that indicates that *Microsoft.Web.Infrastructure.dll* could not be loaded.</span></span>
> 
> <span data-ttu-id="72a15-256">Ce problème ne se produit pas si vous avez chargé la version bêta 3 sur un ordinateur sain.</span><span class="sxs-lookup"><span data-stu-id="72a15-256">This issue does not occur if you loaded the Beta 3 release on a clean computer.</span></span>
> 
> <span data-ttu-id="72a15-257">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="72a15-257">**Workaround**</span></span>  
> <span data-ttu-id="72a15-258">Dans le panneau de configuration, désinstallez les Pages Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="72a15-258">In Control Panel, uninstall ASP.NET Web Pages.</span></span> <span data-ttu-id="72a15-259">Ensuite, réinstallez la version bêta 3.</span><span class="sxs-lookup"><span data-stu-id="72a15-259">Then reinstall the Beta 3 release.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="72a15-260">Problème : Désinstallation du .NET Framework version 4 désactive ASP.NET Web Pages avec syntaxe Razor</span><span class="sxs-lookup"><span data-stu-id="72a15-260">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="72a15-261">Si vous désinstallez le Kit de développement .NET Framework version 4 et puis réinstallez, ASP.NET Web Pages avec syntaxe Razor est désactivées.</span><span class="sxs-lookup"><span data-stu-id="72a15-261">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="72a15-262">Pages avec la *.cshtml* extension ne fonctionnent pas correctement.</span><span class="sxs-lookup"><span data-stu-id="72a15-262">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="72a15-263">Les Pages Web ASP.NET enregistre un assembly dans la racine de l’ordinateur *Web.config* des fichiers et suppression de .NET Framework supprime ce fichier.</span><span class="sxs-lookup"><span data-stu-id="72a15-263">ASP.NET Web Pages registers an assembly in the machine root *Web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="72a15-264">Réinstallez le .NET Framework installe une nouvelle version du fichier de configuration, mais ne pas ajoute la référence de l’assembly ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="72a15-264">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="72a15-265">**Solution de contournement** après la réinstallation de .NET Framework, réinstallez ASP.NET Web Pages avec syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="72a15-265">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="72a15-266">Cette opération ajoute l’élément suivant à la *Web.config* fichier à la racine de l’ordinateur, qui correspond généralement à l’emplacement suivant :</span><span class="sxs-lookup"><span data-stu-id="72a15-266">This adds the following element to the *Web.config* file in the machine root, which is typically in the following location:</span></span>  
>   
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
>   
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a><span data-ttu-id="72a15-267">Problème : Application précédemment déployée avec des assemblys dans le dossier Bin ASP.NET rencontrer des erreurs</span><span class="sxs-lookup"><span data-stu-id="72a15-267">Issue: Applications previously deployed with ASP.NET assemblies in the Bin folder experience errors</span></span>

> <span data-ttu-id="72a15-268">Pendant le déploiement, les copies des assemblys ASP.NET Web Pages (par exemple, *Microsoft.WebPages.dll*) pour le *Bin* dossier du site Web sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="72a15-268">During deployment, copies of the ASP.NET Web Pages assemblies (for example, *Microsoft.WebPages.dll*) to the *Bin* folder of the website on the server.</span></span> <span data-ttu-id="72a15-269">(Cela peut être dû au automatiquement pendant le déploiement ou parce que le développeur copiée explicitement les assemblys.) Toutefois, lorsque la version bêta 3 est installée, erreurs se produit, telles que les erreurs certains types ne peut pas être trouvés.</span><span class="sxs-lookup"><span data-stu-id="72a15-269">(This might have happened automatically during deployment or because the developer explicitly copied the assemblies.) However, when the Beta 3 release is installed, errors occurs, such as errors that certain types cannot be found.</span></span> <span data-ttu-id="72a15-270">Cela se produit, car plusieurs types de Pages Web ASP.NET ont été déplacées dans différents espaces de noms pour la version bêta 3.</span><span class="sxs-lookup"><span data-stu-id="72a15-270">This occurs because a number of ASP.NET Web Pages types were moved into different namespaces for the Beta 3 release.</span></span>
> 
> <span data-ttu-id="72a15-271">**Solution de contournement** </span><span class="sxs-lookup"><span data-stu-id="72a15-271">**Workaround** </span></span>  
> <span data-ttu-id="72a15-272">Désactivez le *Bin* dossier de l’application déployée, copiez les nouveaux assemblys dans le dossier (ou redéployer l’application), puis redémarrez l’application.</span><span class="sxs-lookup"><span data-stu-id="72a15-272">Clear the *Bin* folder of the deployed application, copy the new assemblies to the folder (or redeploy the application), and then restart the application.</span></span>


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="72a15-273">Problème : URL sans extension ne trouvent pas les fichiers.cshtml/.vbhtml sur IIS 7 ou IIS 7.5</span><span class="sxs-lookup"><span data-stu-id="72a15-273">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="72a15-274">Sur IIS 7 ou IIS 7.5, les demandes avec une URL semblable à la suivante ne sont pas en mesure de trouver les pages qui ont le *.cshtml* ou *.vbhtml* extension :</span><span class="sxs-lookup"><span data-stu-id="72a15-274">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
>   
> `http://www.example.com/ExampleSite/ExampleFile`  
>   
> <span data-ttu-id="72a15-275">Le problème se produit car la réécriture d’URL n’est pas activée par défaut pour IIS 7 ou IIS 7.5.</span><span class="sxs-lookup"><span data-stu-id="72a15-275">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="72a15-276">Le scénario voyez généralement cette est que vous ne voyez pas le problème lors du test localement à l’aide d’IIS Express, mais que vous le rencontrez lorsque vous déployez votre site Web sur un site Web d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="72a15-276">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="72a15-277">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="72a15-277">**Workaround**</span></span>
> 
> - <span data-ttu-id="72a15-278">Si vous avez un contrôle sur l’ordinateur du serveur, sur l’ordinateur serveur installer la mise à jour qui est décrite dans [une mise à jour n’est disponible qu’Active certains gestionnaires d’IIS 7.0 ou IIS 7.5 pour gérer les demandes dont l’URL ne se termine pas par un point](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="72a15-278">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="72a15-279">Si vous n’avez pas de contrôle sur l’ordinateur du serveur (par exemple, vous déployez sur un site Web d’hébergement), ajoutez le code suivant du site Web *Web.config* fichier :</span><span class="sxs-lookup"><span data-stu-id="72a15-279">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *Web.config* file:</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a><span data-ttu-id="72a15-280">Problème : Projet d’Application Web ou des pages Web ASP.NET et d’ASP.NET MVC à l’aide dans la même application</span><span class="sxs-lookup"><span data-stu-id="72a15-280">Issue: Using Web Application Project or ASP.NET MVC and ASP.NET Web pages in the same application</span></span>

> <span data-ttu-id="72a15-281">Si vous utilisez les Pages Web ASP.NET dans un projet d’Application Web ou d’une application ASP.NET MVC, vous pouvez voir une erreur qui *WebPageHttpApplication* est introuvable.</span><span class="sxs-lookup"><span data-stu-id="72a15-281">If you were using ASP.NET Web Pages in a Web Application project or ASP.NET MVC application, you might see an error that *WebPageHttpApplication* cannot be found.</span></span>
> 
> <span data-ttu-id="72a15-282">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="72a15-282">**Workaround**</span></span>  
> <span data-ttu-id="72a15-283">Si vous obtenez cette erreur, modifiez la classe de base dont dérive l’application.</span><span class="sxs-lookup"><span data-stu-id="72a15-283">If you get this error, change the base class from which the application derives.</span></span> <span data-ttu-id="72a15-284">Dans le *Global.asax* , modifiez la ligne suivante :</span><span class="sxs-lookup"><span data-stu-id="72a15-284">In the *Global.asax* file, change the following line:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> <span data-ttu-id="72a15-285">par ceci :</span><span class="sxs-lookup"><span data-stu-id="72a15-285">To this:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> <span data-ttu-id="72a15-286">Cela annule en effet une modification a été introduite pour la version bêta 1 de ASP.NET Web Pages avec syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="72a15-286">This in effect reverses a change that was introduced for the Beta 1 release of ASP.NET Web Pages with Razor syntax.</span></span>


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="72a15-287">Problème : Déploiement d’une application sur un ordinateur qui n’a pas de SQL Server Compact installé</span><span class="sxs-lookup"><span data-stu-id="72a15-287">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="72a15-288">Les applications qui incluent des bases de données SQL Server Compact peuvent être exécutées sur un ordinateur sur lequel SQL Server Compact n’est pas installé.</span><span class="sxs-lookup"><span data-stu-id="72a15-288">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="72a15-289">Microsoft WebMatrix bêta 3 automatiquement des copies de ces fichiers binaires et effectue les *Web.config* transformations de fichiers.</span><span class="sxs-lookup"><span data-stu-id="72a15-289">Microsoft WebMatrix Beta 3 automatically copies these binaries for you and performs the appropriate *Web.config* file transforms.</span></span>
> 
> <span data-ttu-id="72a15-290">**Solution de contournement** si vous devez copier ces fichiers et de rendre le *Web.config* modifications du fichier manuellement, effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="72a15-290">**Workaround** If you need to copy these files and make the *Web.config* file changes manually, do the following :</span></span>
> 
> 1. <span data-ttu-id="72a15-291">Copiez les assemblys du moteur de base de données à la *Bin* dossier (et sous-dossiers) de l’application sur l’ordinateur cible :</span><span class="sxs-lookup"><span data-stu-id="72a15-291">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span> 
> 
>     - <span data-ttu-id="72a15-292">Copie *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **à** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="72a15-292">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*</span></span>
>     - <span data-ttu-id="72a15-293">Copie *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\** **à** *\Bin\x86*</span><span class="sxs-lookup"><span data-stu-id="72a15-293">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\** **to** *\Bin\x86*</span></span>
>     - <span data-ttu-id="72a15-294">Copie *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **à** *\Bin\amd64*</span><span class="sxs-lookup"><span data-stu-id="72a15-294">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **to** *\Bin\amd64*</span></span>
> 2. <span data-ttu-id="72a15-295">Dans le dossier racine du site Web, créez ou ouvrez un *Web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="72a15-295">In the root folder of the website, create or open a *Web.config* file.</span></span> <span data-ttu-id="72a15-296">(Dans la version bêta 3 de WebMatrix, ce type de fichier est disponible si vous cliquez sur **tous les** dans les **choisir un Type de fichier** boîte de dialogue.)</span><span class="sxs-lookup"><span data-stu-id="72a15-296">(In WebMatrix Beta 3, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="72a15-297">Ajoutez l’élément suivant en tant qu’enfant de la  **&lt;configuration&gt;**  élément (pas à l’intérieur de la  **&lt;system.web&gt;**  élément) :</span><span class="sxs-lookup"><span data-stu-id="72a15-297">Add the following element as a child of the **&lt;configuration&gt;** element (not inside the **&lt;system.web&gt;** element):</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="72a15-298">Problème : Les programmes d’assistance WebGrid et de base de données ne fonctionnent pas dans la confiance moyenne dans Visual Basic</span><span class="sxs-lookup"><span data-stu-id="72a15-298">Issue: Database and WebGrid helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="72a15-299">Si vous utilisez Visual Basic (création *.vbhtml* fichiers), le `Database` et `WebGrid` programmes d’assistance ne fonctionnent pas si l’application est configurée pour utiliser la confiance moyenne.</span><span class="sxs-lookup"><span data-stu-id="72a15-299">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="72a15-300">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="72a15-300">**Workaround**</span></span>  
> <span data-ttu-id="72a15-301">Définissez temporairement l’application pour utiliser la confiance totale.</span><span class="sxs-lookup"><span data-stu-id="72a15-301">Temporarily set the application to use Full Trust.</span></span>

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a><span data-ttu-id="72a15-302">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="72a15-302">SQL Server Compact</span></span>

#### <a name="issue-encrypt-property-is-not-recognized"></a><span data-ttu-id="72a15-303">Problème : Propriété « Encrypt » n’est pas reconnue.</span><span class="sxs-lookup"><span data-stu-id="72a15-303">Issue: "Encrypt" property is not recognized</span></span>

> <span data-ttu-id="72a15-304">SQL Server Compact 4.0 ne reconnaît pas le `Encrypt` propriété de la `SqlCeConnection` classe.</span><span class="sxs-lookup"><span data-stu-id="72a15-304">SQL Server Compact 4.0 does not recognize the `Encrypt` property of the `SqlCeConnection` class.</span></span> <span data-ttu-id="72a15-305">Vous ne devez pas utiliser cette propriété pour chiffrer les fichiers de base de données.</span><span class="sxs-lookup"><span data-stu-id="72a15-305">You should not use this property to encrypt database files.</span></span> <span data-ttu-id="72a15-306">Le `Encrypt` propriété a été déconseillée dans la version 3.5 de SQL Server Compact et a été conservée uniquement pour la compatibilité descendante.</span><span class="sxs-lookup"><span data-stu-id="72a15-306">The `Encrypt` property was deprecated in SQL Server Compact 3.5 release and was retained only for backward compatibility.</span></span> 
> 
> <span data-ttu-id="72a15-307">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="72a15-307">**Workaround**</span></span>  
> <span data-ttu-id="72a15-308">Utilisez le `Encryption Mode` propriété de la `SqlCeConnection` classe pour chiffrer les fichiers de base de données SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="72a15-308">Use the `Encryption Mode` property of the `SqlCeConnection` class to encrypt SQL Server Compact 4.0 database files.</span></span> <span data-ttu-id="72a15-309">L’exemple suivant montre comment créer une base de données SQL Server Compact 4.0 chiffrées à l’aide du `Encryption Mode` propriété :</span><span class="sxs-lookup"><span data-stu-id="72a15-309">The following example shows how to create an encrypted SQL Server Compact 4.0 database using the `Encryption Mode` property:</span></span>
>  
> [!code-csharp[Main](beta3/samples/sample11.cs)]
>  
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> <span data-ttu-id="72a15-310">Pour modifier le mode de chiffrement de base de données SQL Server Compact 4.0 existante, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="72a15-310">To change the encryption mode of an existing SQL Server Compact 4.0 database, do the following:</span></span>
>  
> [!code-csharp[Main](beta3/samples/sample13.cs)]
>  
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> <span data-ttu-id="72a15-311">Pour chiffrer une base de données SQL Server Compact 4.0, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="72a15-311">To encrypt an unencrypted SQL Server Compact 4.0 database, do the following:</span></span>
>  
> [!code-csharp[Main](beta3/samples/sample15.cs)]
>  
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a><span data-ttu-id="72a15-312">Problème : Les bibliothèques runtime Microsoft Visual C++ 2008 sont requis</span><span class="sxs-lookup"><span data-stu-id="72a15-312">Issue: Microsoft Visual C++ 2008 runtime libraries are required</span></span>

> <span data-ttu-id="72a15-313">Le natif DLL de SQL Server Compact 4.0 peut-être le Microsoft Visual C++ 2008 bibliothèques Runtime (x 86, IA64 et x 64), le Service Pack 1.</span><span class="sxs-lookup"><span data-stu-id="72a15-313">The native DLLs of SQL Server Compact 4.0 need the Microsoft Visual C++ 2008 Runtime Libraries (x86, IA64, and x64), Service Pack 1.</span></span>
> 
> <span data-ttu-id="72a15-314">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="72a15-314">**Workaround**</span></span>  
> <span data-ttu-id="72a15-315">Installez le .NET Framework 3.5 SP1.</span><span class="sxs-lookup"><span data-stu-id="72a15-315">Install the .NET Framework 3.5 SP1.</span></span> <span data-ttu-id="72a15-316">Cette commande installe également le Runtime bibliothèques de Visual C++ 2008 SP1.</span><span class="sxs-lookup"><span data-stu-id="72a15-316">This also installs the Visual C++ 2008 Runtime Libraries SP1.</span></span> <span data-ttu-id="72a15-317">Vous pouvez télécharger les bibliothèques à partir de l’emplacement suivant :</span><span class="sxs-lookup"><span data-stu-id="72a15-317">You can download the libraries from the following location:</span></span>   
>   
> [<span data-ttu-id="72a15-318">Mise à jour de la sécurité de Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL</span><span class="sxs-lookup"><span data-stu-id="72a15-318">Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL Security Update</span></span>](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> <span data-ttu-id="72a15-319">Notez que l’installation de .NET Framework 2.0, 3.0, ou le 4 *pas* installer le SP1 bibliothèques Runtime de Visual C++ 2008.</span><span class="sxs-lookup"><span data-stu-id="72a15-319">Note that installing the .NET Framework 2.0, 3.0, or 4 does *not* install the Visual C++ 2008 Runtime Libraries SP1.</span></span>


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a><span data-ttu-id="72a15-320">Problème : Si SQL Server Compact est installé avant d’installer le .NET Framework sur l’ordinateur, son nom invariant du fournisseur est inscrit pas dans le fichier machine.config de .NET Framework</span><span class="sxs-lookup"><span data-stu-id="72a15-320">Issue: If SQL Server Compact is installed prior to installing .NET Framework on the computer, its provider invariant name is not registered in the .NET Framework machine.config file</span></span>

> <span data-ttu-id="72a15-321">SQL Server Compact peut être installé sur un ordinateur qui n’a pas de .NET Framework est installé, car SQL Server Compact requiert le .NET framework.</span><span class="sxs-lookup"><span data-stu-id="72a15-321">SQL Server Compact can be installed on a machine that does not have .NET Framework installed because SQL Server Compact does require the .NET framework.</span></span> <span data-ttu-id="72a15-322">Si .NET Framework version 3.5 ni 4 est installé avant d’installer SQL Server Compact, le programme d’installation SQL Server Compact n’inscrit pas son nom invariant du fournisseur dans le *machine.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="72a15-322">If neither .NET Framework version 3.5 nor 4 is installed before you install SQL Server Compact, the SQL Server Compact Setup does not register its provider invariant name in the *machine.config* file.</span></span> <span data-ttu-id="72a15-323">Toute application qui s’appuie sur l’entrée de SQL Server Compact dans le *machine.config* fichier échouera.</span><span class="sxs-lookup"><span data-stu-id="72a15-323">Any application that relies on the SQL Server Compact entry in the *machine.config* file will fail.</span></span> <span data-ttu-id="72a15-324">L’entrée d’inscription de nom invariant dans *machine.config* ressemble à l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="72a15-324">The invariant name registration entry in *machine.config* looks like the following example:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> <span data-ttu-id="72a15-325">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="72a15-325">**Workaround**</span></span>  
> <span data-ttu-id="72a15-326">Désinstaller SQL Server Compact 4.0 CTP1.</span><span class="sxs-lookup"><span data-stu-id="72a15-326">Uninstall SQL Server Compact 4.0 CTP1.</span></span> <span data-ttu-id="72a15-327">Téléchargez et installez la version complète du .NET Framework à partir de l’emplacement suivant :</span><span class="sxs-lookup"><span data-stu-id="72a15-327">Download and install the full versions of the .NET Framework from the following location:</span></span>
> 
> [<span data-ttu-id="72a15-328">Microsoft .NET Framework 3.5 Service pack 1 (Package complet)</span><span class="sxs-lookup"><span data-stu-id="72a15-328">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [<span data-ttu-id="72a15-329">Mise en production de Microsoft .NET Framework 4.0 (Package complet)</span><span class="sxs-lookup"><span data-stu-id="72a15-329">Microsoft .NET Framework 4.0 Release (Full Package)</span></span>](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> <span data-ttu-id="72a15-330">Puis réinstallez [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span><span class="sxs-lookup"><span data-stu-id="72a15-330">Then reinstall [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span></span>


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a><span data-ttu-id="72a15-331">L’installation d’Applications</span><span class="sxs-lookup"><span data-stu-id="72a15-331">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="72a15-332">Problème : Installation d’une application peut prendre beaucoup de temps si le dossier Mes Documents de l’utilisateur est redirigé vers un partage réseau</span><span class="sxs-lookup"><span data-stu-id="72a15-332">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="72a15-333">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="72a15-333">**Workaround**</span></span>  
> <span data-ttu-id="72a15-334">Aucun.</span><span class="sxs-lookup"><span data-stu-id="72a15-334">None.</span></span> <span data-ttu-id="72a15-335">L’application peut prendre un certain temps à installer, mais ne s’installe correctement.</span><span class="sxs-lookup"><span data-stu-id="72a15-335">The application might take a while to install, but will install correctly.</span></span>


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a><span data-ttu-id="72a15-336">Publication d’Applications</span><span class="sxs-lookup"><span data-stu-id="72a15-336">Publishing Applications</span></span>

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="72a15-337">Problème : Site peut ne pas fonctionne après la publication si le champ « URL de Destination » n’est pas précédé par http:// ou https://</span><span class="sxs-lookup"><span data-stu-id="72a15-337">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="72a15-338">Dans le **paramètres de publication** boîte de dialogue, si l’URL de destination ne commence pas par `http://` ou `https://`, le site peut ne pas fonctionne après le déploiement.</span><span class="sxs-lookup"><span data-stu-id="72a15-338">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="72a15-339">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="72a15-339">**Workaround**</span></span>  
> <span data-ttu-id="72a15-340">Assurez-vous qu’avant de publier un site, l’URL de destination dans le **paramètres de publication** boîte de dialogue commence par `http://` ou `https://`.</span><span class="sxs-lookup"><span data-stu-id="72a15-340">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="72a15-341">Problème : Publication d’une base de données MySQL échoue avec l’erreur « Échec de la base de données de publication.</span><span class="sxs-lookup"><span data-stu-id="72a15-341">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="72a15-342">Cela peut se produire si la base de données distante ne peut pas exécuter le script. »</span><span class="sxs-lookup"><span data-stu-id="72a15-342">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="72a15-343">L’erreur peut se produire pour plusieurs raisons.</span><span class="sxs-lookup"><span data-stu-id="72a15-343">The error can occur for a number of reasons.</span></span> <span data-ttu-id="72a15-344">Vous pouvez voir cette erreur est si le script de base de données contient un guillemet simple (') et le jeu de caractères par défaut de la destination MySQL de base de données n’est pas au format UTF-8.</span><span class="sxs-lookup"><span data-stu-id="72a15-344">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="72a15-345">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="72a15-345">**Workaround**</span></span>  
> <span data-ttu-id="72a15-346">Définir le caractère par défaut définie pour la base de données MySQL à distance au format UTF-8.</span><span class="sxs-lookup"><span data-stu-id="72a15-346">Set the default character set for the remote MySQL database to UTF-8.</span></span>


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a><span data-ttu-id="72a15-347">Autres problèmes</span><span class="sxs-lookup"><span data-stu-id="72a15-347">Other Issues</span></span>

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a><span data-ttu-id="72a15-348">Problème : Les/filtre de recherche ne fonctionne pas dans les rapports pour regrouper par : Type de problème</span><span class="sxs-lookup"><span data-stu-id="72a15-348">Issue: Search/Filter does not work in Reports for Group By: Issue Type</span></span>

> <span data-ttu-id="72a15-349">Lorsque vous exécutez un rapport pour un site, si vous entrez du texte dans le *filtre par URL* , puis cliquez sur *recherche*, rien ne se produit.</span><span class="sxs-lookup"><span data-stu-id="72a15-349">When you run a report for a site, if you enter text in the *Filter by URL* box and click *Search*, nothing happens.</span></span> <span data-ttu-id="72a15-350">Il s’agit, car ce contrôle ne fonctionne pas lors de la *Group By* l’état de l’état est défini sur *Type de problème*, qui est la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="72a15-350">This is because this control is not functional while the *Group By* state of the report is set to *Issue Type*, which is the default.</span></span>
> 
> <span data-ttu-id="72a15-351">**Solution de contournement** dans les *Group By* onglet du ruban, cliquez sur *URL* pour regrouper les entrées par leur URL de la source.</span><span class="sxs-lookup"><span data-stu-id="72a15-351">**Workaround** In the *Group By* tab of ribbon, click *URL* to group the entries by their source URL.</span></span> <span data-ttu-id="72a15-352">La zone de texte et un bouton pour filtrer les entrées sont fonctionnels dans cet état.</span><span class="sxs-lookup"><span data-stu-id="72a15-352">The text box and button to filter the entries are functional while in this state.</span></span>


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a><span data-ttu-id="72a15-353">Problème : Les applications WCF que l’exécution échouent avec IIS Express</span><span class="sxs-lookup"><span data-stu-id="72a15-353">Issue: WCF applications fail to run with IIS Express</span></span>

> <span data-ttu-id="72a15-354">Accéder à une application WCF génère une erreur comme la suivante :</span><span class="sxs-lookup"><span data-stu-id="72a15-354">Browsing to a WCF application results in an error like the following one:</span></span>
> 
> <span data-ttu-id="72a15-355">*Impossible de charger le fichier ou l’assembly ' Microsoft.Web.Administration, Version = 7.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' ou une de ses dépendances. Le système ne parvient pas à localiser le fichier spécifié.*</span><span class="sxs-lookup"><span data-stu-id="72a15-355">*Could not load file or assembly 'Microsoft.Web.Administration, Version=7.0.0.0, Culture=neutral,PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The system cannot find the file specified.*</span></span>
> 
> <span data-ttu-id="72a15-356">Cela se produit, car la version bêta de IIS Express ne prend pas en charge WCF par défaut.</span><span class="sxs-lookup"><span data-stu-id="72a15-356">This occurs because IIS Express Beta release doesn't support WCF by default.</span></span>
> 
> <span data-ttu-id="72a15-357">**Solution de contournement** utiliser l’une des solutions de contournement suivantes (solution de contournement #2 requiert Microsoft Windows Vista ou version ultérieure) :</span><span class="sxs-lookup"><span data-stu-id="72a15-357">**Workaround** Use any one of the following workarounds (workaround #2 requires Microsoft Windows Vista or higher):</span></span>
> 
> 
> 1. <span data-ttu-id="72a15-358">Copie le *Microsoft.Web.dll* et *Microsoft.Web.Administration.dll* assemblys à partir de l’emplacement d’installation de WebMatrix pour le *bin* de WCF application.</span><span class="sxs-lookup"><span data-stu-id="72a15-358">Copy the *Microsoft.Web.dll* and *Microsoft.Web.Administration.dll* assemblies from the WebMatrix installation location to the *bin* directory of the WCF application.</span></span> <span data-ttu-id="72a15-359">Par défaut, WebMatrix est installé dans le *Microsoft WebMatrix* sous-dossier du système *Program Files* dossier.</span><span class="sxs-lookup"><span data-stu-id="72a15-359">By default, WebMatrix is installed in the *Microsoft WebMatrix* subfolder under the system's *Program Files* folder.</span></span>
> 2. <span data-ttu-id="72a15-360">Dans Microsoft Windows Vista ou version ultérieure, créez un lien symbolique vers les assemblys dans le *bin* répertoire en utilisant les commandes suivantes.</span><span class="sxs-lookup"><span data-stu-id="72a15-360">On Microsoft Windows Vista or higher, create a symlink to the assemblies in the *bin* directory using the following commands.</span></span> <span data-ttu-id="72a15-361">(Cette approche a l’avantage qu’il ne crée pas une copie des assemblys.)</span><span class="sxs-lookup"><span data-stu-id="72a15-361">(This approach has the advantage that it does not create a copy of the assemblies.)</span></span>
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. <span data-ttu-id="72a15-362">Installez les deux assemblys dans le GAC.</span><span class="sxs-lookup"><span data-stu-id="72a15-362">Install the two assemblies in the GAC.</span></span> <span data-ttu-id="72a15-363">À partir d’une invite de commandes avec élévation de privilèges, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="72a15-363">From an elevated prompt, run the following commands:</span></span>
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="72a15-364">Problème : La version bêta 3 de WebMatrix est impossible d’effectuer certaines tâches qui requièrent une élévation</span><span class="sxs-lookup"><span data-stu-id="72a15-364">Issue: WebMatrix Beta 3 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="72a15-365">WebMatrix bêta 3 ne parvient pas à effectuer certaines tâches qui requièrent une élévation, telles que l’installation des composants supplémentaires dans les situations suivantes :</span><span class="sxs-lookup"><span data-stu-id="72a15-365">WebMatrix Beta 3 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="72a15-366">Sur Windows Vista ou Windows 7, vous êtes connecté avec un compte qui ne dispose pas des privilèges d’administrateur et contrôle de compte d’utilisateur (UAC) est désactivé.</span><span class="sxs-lookup"><span data-stu-id="72a15-366">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="72a15-367">Vous utilisez Microsoft Windows XP ou Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="72a15-367">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="72a15-368">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="72a15-368">**Workaround**</span></span>  
> <span data-ttu-id="72a15-369">La plupart des tâches dans la version bêta 3 de WebMatrix ne nécessitent pas d’autorisation d’administrateur.</span><span class="sxs-lookup"><span data-stu-id="72a15-369">Most tasks in WebMatrix Beta 3 do not require administrative permission.</span></span> <span data-ttu-id="72a15-370">Pour ce faire, vous pouvez effectuer l’opération en tant qu’administrateur, ou procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="72a15-370">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="72a15-371">Sur Windows Vista ou Windows 7, activer le compte d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="72a15-371">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="72a15-372">Sous Windows XP, ajoutez l’utilisateur au groupe de sécurité Administrateurs.</span><span class="sxs-lookup"><span data-stu-id="72a15-372">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="72a15-373">Problème : « Site de la galerie Web » est désactivée.</span><span class="sxs-lookup"><span data-stu-id="72a15-373">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="72a15-374">Le **Site à partir de la galerie Web** option est désactivée si le serveur Web Platform Installer 3.0 n’est pas installé.</span><span class="sxs-lookup"><span data-stu-id="72a15-374">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="72a15-375">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="72a15-375">**Workaround**</span></span>  
> <span data-ttu-id="72a15-376">Installer le [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="72a15-376">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a><span data-ttu-id="72a15-377">Problème : Sur Windows Server 2003, IIS Express ne démarre pas pour un utilisateur non-administrateur</span><span class="sxs-lookup"><span data-stu-id="72a15-377">Issue: On Windows Server 2003, IIS Express does not start for a non-administrative user</span></span>

> <span data-ttu-id="72a15-378">Sur Windows Server 2003, lorsque vous lancez une page ou que vous démarrez IIS Express, IIS Express ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="72a15-378">On Windows Server 2003, when you launch a page or start IIS Express, IIS Express does not start.</span></span> <span data-ttu-id="72a15-379">Pour les pages Web, une erreur s’affiche qui indique que l’application a été démarrée par un utilisateur non-administrateur.</span><span class="sxs-lookup"><span data-stu-id="72a15-379">For Web pages, an error is displayed that indicates that the application has been started by a non-administrative user.</span></span>
> 
> <span data-ttu-id="72a15-380">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="72a15-380">**Workaround**</span></span>  
> <span data-ttu-id="72a15-381">Démarrer la version bêta 3 de WebMatrix en tant qu’administrateur.</span><span class="sxs-lookup"><span data-stu-id="72a15-381">Start WebMatrix Beta 3 as an administrative user.</span></span> <span data-ttu-id="72a15-382">Pour plus d’informations, consultez l’article suivant de la base de connaissances :</span><span class="sxs-lookup"><span data-stu-id="72a15-382">For more details, see the following KnowledgeBase article:</span></span>  
>   
> [<span data-ttu-id="72a15-383">Une application est démarrée par un utilisateur non-administrateur ne peut pas écouter le trafic HTTP de l’ordinateur sur lequel l’application s’exécute sous Windows Vista, Windows Server 2003 ou Windows XP.</span><span class="sxs-lookup"><span data-stu-id="72a15-383">An application that is started by a non-administrative user cannot listen to the HTTP traffic of the computer on which the application is running in Windows Vista, Windows Server 2003, or Windows XP.</span></span>](https://support.microsoft.com/kb/939786)


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="72a15-384">Problème : Google Chrome n’est pas disponible comme option de série</span><span class="sxs-lookup"><span data-stu-id="72a15-384">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="72a15-385">Google Chrome n’est pas affiché dans la liste des navigateurs sous **exécuter** sur la **accueil** onglet.</span><span class="sxs-lookup"><span data-stu-id="72a15-385">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="72a15-386">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="72a15-386">**Workaround**</span></span>  
> <span data-ttu-id="72a15-387">Certaines versions de Google Chrome n’inscrivent pas eux-mêmes correctement avec la fonctionnalité programmes par défaut de Windows.</span><span class="sxs-lookup"><span data-stu-id="72a15-387">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="72a15-388">Pour résoudre ce problème, démarrez Google Chrome, cliquez sur le *personnaliser et contrôle Google Chrome* menu, cliquez sur *Options*, puis cliquez sur *Vérifiez Google Chrome mon navigateur par défaut*.</span><span class="sxs-lookup"><span data-stu-id="72a15-388">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="72a15-389">Problème : La boîte de dialogue « Foreign Key » n’autorise pas entrer une clé primaire</span><span class="sxs-lookup"><span data-stu-id="72a15-389">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="72a15-390">Le **clé étrangère** boîte de dialogue ne vous permet pas à entrer le nom de clé primaire de la table de clé primaire.</span><span class="sxs-lookup"><span data-stu-id="72a15-390">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="72a15-391">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="72a15-391">**Workaround**</span></span>  
> <span data-ttu-id="72a15-392">Cela est intentionnel.</span><span class="sxs-lookup"><span data-stu-id="72a15-392">This is intentional.</span></span> <span data-ttu-id="72a15-393">Vous n’avez pas besoin d’entrer le nom de la clé primaire de la table de clé primaire.</span><span class="sxs-lookup"><span data-stu-id="72a15-393">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-the-relationships-button-is-disabled"></a><span data-ttu-id="72a15-394">Problème : Le bouton « Relations » est désactivé.</span><span class="sxs-lookup"><span data-stu-id="72a15-394">Issue: The "Relationships" button is disabled</span></span>

> <span data-ttu-id="72a15-395">Le **relations** sous le **Table** onglet dans le **bases de données** espace de travail est désactivé pour les bases de données SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="72a15-395">The **Relationships** button under the **Table** tab in the **Databases** workspace is disabled for SQL Server Compact databases.</span></span>
> 
> <span data-ttu-id="72a15-396">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="72a15-396">**Workaround**</span></span>  
> <span data-ttu-id="72a15-397">Aucun.</span><span class="sxs-lookup"><span data-stu-id="72a15-397">None.</span></span> <span data-ttu-id="72a15-398">SQL Server Compact ne prend pas en charge les relations entre les tables.</span><span class="sxs-lookup"><span data-stu-id="72a15-398">SQL Server Compact does not support relationships between tables.</span></span>


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a><span data-ttu-id="72a15-399">Problème : Les requêtes SQL paramétrées lèvent des exceptions</span><span class="sxs-lookup"><span data-stu-id="72a15-399">Issue: Parameterized SQL queries throw exceptions</span></span>

> <span data-ttu-id="72a15-400">Dans SQL Server Compact 4.0, si vous ne spécifiez pas un type de données tel que `SqlDbType` ou `DbType` pour les paramètres dans les requêtes paramétrables, une exception est levée lorsque la requête s’exécute.</span><span class="sxs-lookup"><span data-stu-id="72a15-400">In SQL Server Compact 4.0, if you do not specify a data type such as `SqlDbType` or `DbType` for parameters in parameterized queries, an exception is thrown when the query runs.</span></span>
> 
> <span data-ttu-id="72a15-401">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="72a15-401">**Workaround**</span></span>  
> <span data-ttu-id="72a15-402">Définir explicitement le type de données pour les paramètres tels que `SqlDbType` ou `DbType`.</span><span class="sxs-lookup"><span data-stu-id="72a15-402">Explicitly set the data type for parameters such as `SqlDbType` or `DbType`.</span></span> <span data-ttu-id="72a15-403">Ceci est très important dans le cas des types de données BLOB (`image` et `ntext`).</span><span class="sxs-lookup"><span data-stu-id="72a15-403">This is critical in the case of BLOB data types (`image` and `ntext`).</span></span> <span data-ttu-id="72a15-404">Utilisez un code semblable au suivant :</span><span class="sxs-lookup"><span data-stu-id="72a15-404">Use code like the following:</span></span>
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
>  
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="72a15-405">Pour plus d'informations</span><span class="sxs-lookup"><span data-stu-id="72a15-405">For More Information</span></span>

<span data-ttu-id="72a15-406">Pour plus d’informations sur la version bêta 3 de WebMatrix, consultez les sites Web suivants :</span><span class="sxs-lookup"><span data-stu-id="72a15-406">For more information about WebMatrix Beta 3, see the following websites:</span></span>

- [<span data-ttu-id="72a15-407">IIS.net</span><span class="sxs-lookup"><span data-stu-id="72a15-407">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="72a15-408">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="72a15-408">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="72a15-409">Web Microsoft.com à</span><span class="sxs-lookup"><span data-stu-id="72a15-409">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

* * *

<span data-ttu-id="72a15-410">© 2010 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="72a15-410">© 2010 Microsoft Corporation.</span></span> <span data-ttu-id="72a15-411">Tous droits réservés.</span><span class="sxs-lookup"><span data-stu-id="72a15-411">All Rights Reserved.</span></span> <span data-ttu-id="72a15-412">[Conditions d’utilisation](https://msdn.microsoft.com/en-us/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="72a15-412">[Terms of Use](https://msdn.microsoft.com/en-us/cc300389.aspx).</span></span>
