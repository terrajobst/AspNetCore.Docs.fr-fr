---
uid: web-pages/readme/beta3
title: WebMatrix et ASP.NET Web Pages (Razor) bêta 3 version Lisez-moi | Microsoft Docs
author: rick-anderson
description: WebMatrix et ASP.NET Web Pages (Razor) bêta 3 version Lisez-moi
ms.author: aspnetcontent
ms.date: 01/10/2011
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 16b324e555b5450fc1e05c63e7e19985a2d02b89
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831830"
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a><span data-ttu-id="4188a-103">WebMatrix et ASP.NET Web Pages (Razor) bêta 3 version Lisez-moi</span><span class="sxs-lookup"><span data-stu-id="4188a-103">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>
====================
> <span data-ttu-id="4188a-104">WebMatrix et ASP.NET Web Pages (Razor) bêta 3 version Lisez-moi</span><span class="sxs-lookup"><span data-stu-id="4188a-104">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>

<span data-ttu-id="4188a-105">9 novembre 2010</span><span class="sxs-lookup"><span data-stu-id="4188a-105">9 November 2010</span></span>

## <a name="contents"></a><span data-ttu-id="4188a-106">Sommaire</span><span class="sxs-lookup"><span data-stu-id="4188a-106">Contents</span></span>

- [<span data-ttu-id="4188a-107">Vue d’ensemble</span><span class="sxs-lookup"><span data-stu-id="4188a-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="4188a-108">Installation</span><span class="sxs-lookup"><span data-stu-id="4188a-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="4188a-109">Nouvelles fonctionnalités, les modifications et les problèmes connus dans la version bêta 3</span><span class="sxs-lookup"><span data-stu-id="4188a-109">New Features, Changes, and Known Issues in the Beta 3 release</span></span>](#Known_Issues)

    - [<span data-ttu-id="4188a-110">Problèmes d’Installation de WebMatrix</span><span class="sxs-lookup"><span data-stu-id="4188a-110">WebMatrix Installation Issues</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="4188a-111">Pages Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4188a-111">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="4188a-112">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="4188a-112">SQL Server Compact</span></span>](#Known_Issues_SQL_Server_Compact)
    - [<span data-ttu-id="4188a-113">Installation d’Applications</span><span class="sxs-lookup"><span data-stu-id="4188a-113">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="4188a-114">Publication d’Applications</span><span class="sxs-lookup"><span data-stu-id="4188a-114">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
    - [<span data-ttu-id="4188a-115">Autres problèmes</span><span class="sxs-lookup"><span data-stu-id="4188a-115">Other Issues</span></span>](#Known_Issues_Other_Issues)
- [<span data-ttu-id="4188a-116">Pour plus d’informations</span><span class="sxs-lookup"><span data-stu-id="4188a-116">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="4188a-117">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="4188a-117">Overview</span></span>

> <span data-ttu-id="4188a-118">Microsoft WebMatrix Beta est une pile de développement web gratuit qui s’installe en quelques minutes.</span><span class="sxs-lookup"><span data-stu-id="4188a-118">Microsoft WebMatrix Beta is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="4188a-119">Il s’intègre un serveur web de base de données et infrastructures pour créer une expérience unique et intégrée de programmation.</span><span class="sxs-lookup"><span data-stu-id="4188a-119">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="4188a-120">Vous pouvez utiliser la version bêta de WebMatrix pour simplifier la manière dont votre code, tester, publier votre propre site Web ASP.NET ou PHP, ou vous pouvez utiliser la version bêta de WebMatrix pour démarrer un nouveau site Web à l’aide d’applications open source populaires telles que DotNetNuke, Umbraco, WordPress ou Joomla.</span><span class="sxs-lookup"><span data-stu-id="4188a-120">You can use WebMatrix Beta to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix Beta to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="4188a-121">Version bêta de WebMatrix utilise le même serveur web puissant, moteur de base de données et infrastructures que ceux qui exécutera votre site Web sur internet, ce qui rend la transition du développement en production fluide et homogène.</span><span class="sxs-lookup"><span data-stu-id="4188a-121">WebMatrix Beta uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="4188a-122">Installation</span><span class="sxs-lookup"><span data-stu-id="4188a-122">Installation</span></span>

> <span data-ttu-id="4188a-123">Pour installer la version bêta 3 de WebMatrix, vous utilisez [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="4188a-123">To install WebMatrix Beta 3, you use [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="4188a-124">Après avoir installé Web Platform Installer, vous pouvez l’utiliser pour installer la version bêta 3 de WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="4188a-124">After you've installed the Web Platform Installer, you can use it to install WebMatrix Beta 3.</span></span>
> 
> <span data-ttu-id="4188a-125">Si vous rencontrez des problèmes lors de l’installation, reportez-vous à [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="4188a-125">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a><span data-ttu-id="4188a-126">Instructions pour la publication d’Applications</span><span class="sxs-lookup"><span data-stu-id="4188a-126">Instructions for Publishing Applications</span></span>

> <span data-ttu-id="4188a-127">Consultez [obtenir des Instructions détaillées pour la publication d’Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="4188a-127">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a><span data-ttu-id="4188a-128">Nouveaux problèmes andKnown de fonctionnalités, les changements</span><span class="sxs-lookup"><span data-stu-id="4188a-128">New Features, Changes, andKnown Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a><span data-ttu-id="4188a-129">Installation de la version bêta 3 de WebMatrix</span><span class="sxs-lookup"><span data-stu-id="4188a-129">WebMatrix Beta 3 Installation</span></span>

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="4188a-130">Problème : Version bêta 3 de WebMatrix est disponible uniquement sur les plateformes qui prennent en charge de Microsoft .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="4188a-130">Issue: WebMatrix Beta 3 is only available on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="4188a-131">Le .NET Framework version 4 est requis pour la version bêta de WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="4188a-131">The .NET Framework version 4 is required for WebMatrix Beta.</span></span> <span data-ttu-id="4188a-132">Dans certains cas, le programme d’installation de la version bêta de WebMatrix permet d’essayer d’installer sur une plateforme qui ne fait pas partie de l’ensemble de la configuration prise en charge.</span><span class="sxs-lookup"><span data-stu-id="4188a-132">In certain cases, the WebMatrix Beta installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="4188a-133">En particulier, Vista Windows sans la mise à jour SP1 vous permet de commencer l’installation de WebMatrix bêta, mais le composant .NET Framework 4 échoue et bloquer votre installation.</span><span class="sxs-lookup"><span data-stu-id="4188a-133">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix Beta, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="4188a-134">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="4188a-134">**Workaround**</span></span>  
> <span data-ttu-id="4188a-135">Installer sur une plateforme prise en charge, ce qui inclut :</span><span class="sxs-lookup"><span data-stu-id="4188a-135">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="4188a-136">Windows 7</span><span class="sxs-lookup"><span data-stu-id="4188a-136">Windows 7</span></span>
> - <span data-ttu-id="4188a-137">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="4188a-137">Windows Server 2008</span></span>
> - <span data-ttu-id="4188a-138">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="4188a-138">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="4188a-139">Windows Vista SP1 ou ultérieur</span><span class="sxs-lookup"><span data-stu-id="4188a-139">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="4188a-140">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="4188a-140">Windows XP SP3</span></span>
> - <span data-ttu-id="4188a-141">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="4188a-141">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="4188a-142">Problème : Ne peut pas installer WebMatrix bêta 3 si Microsoft Visual Studio 2008 est installé sans Microsoft Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="4188a-142">Issue: Cannot install WebMatrix Beta 3 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="4188a-143">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="4188a-143">**Workaround**</span></span>  
> <span data-ttu-id="4188a-144">Installer [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) à partir du centre de téléchargement Microsoft.</span><span class="sxs-lookup"><span data-stu-id="4188a-144">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="4188a-145">Problème : Certains assemblys pour SQL Server Compact 4.0 ne sont pas installés dans le GAC</span><span class="sxs-lookup"><span data-stu-id="4188a-145">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="4188a-146">Les assemblys managés pour SQL Server Compact 4.0 ne sont pas placés dans le global assembly cache (GAC) lorsque vous installez SQL Server Compact 4.0 sur un ordinateur 64 bits et de l’ordinateur a uniquement le .NET Framework 3.5 SP1 Client Profile installé.</span><span class="sxs-lookup"><span data-stu-id="4188a-146">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="4188a-147">Les assemblys managés qui ne sont pas installés dans le GAC sont :</span><span class="sxs-lookup"><span data-stu-id="4188a-147">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="4188a-148">*System.Data.SqlServerCe.dll* (fournisseur ADO.NET)</span><span class="sxs-lookup"><span data-stu-id="4188a-148">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="4188a-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)</span><span class="sxs-lookup"><span data-stu-id="4188a-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="4188a-150">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="4188a-150">**Workaround**</span></span>  
> <span data-ttu-id="4188a-151">Désinstaller SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="4188a-151">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="4188a-152">Téléchargez et installez la version complète de .NET Framework 3.5 SP1 à partir de l’emplacement suivant :</span><span class="sxs-lookup"><span data-stu-id="4188a-152">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="4188a-153">Microsoft .NET Framework 3.5 Service pack 1 (indépendant)</span><span class="sxs-lookup"><span data-stu-id="4188a-153">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="4188a-154">Puis réinstallez SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="4188a-154">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="4188a-155">Problème : Impossible de désinstaller SQL Server Compact à l’aide de la ligne de commande</span><span class="sxs-lookup"><span data-stu-id="4188a-155">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="4188a-156">La désinstallation de SQL Server Compact à l’aide des options de ligne de commande ne fonctionne pas dans cette version.</span><span class="sxs-lookup"><span data-stu-id="4188a-156">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="4188a-157">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="4188a-157">**Workaround**</span></span>  
> <span data-ttu-id="4188a-158">Utilisez *programmes et fonctionnalités* dans le panneau de configuration Windows pour désinstaller Microsoft SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="4188a-158">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="4188a-159">Pages web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4188a-159">ASP.NET Web Pages</span></span>

<span data-ttu-id="4188a-160">Cette section du document décrit les nouvelles fonctionnalités, des modifications et des problèmes connus avec la version bêta 3 d’ASP.NET Web Pages avec syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="4188a-160">This section of the document describes new features, changes, and known issues with the Beta 3 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="4188a-161">Nouvelles fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="4188a-161">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="4188a-162">Modifications</span><span class="sxs-lookup"><span data-stu-id="4188a-162">Changes</span></span>](#Changes)
- [<span data-ttu-id="4188a-163">Problèmes</span><span class="sxs-lookup"><span data-stu-id="4188a-163">Issues</span></span>](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="4188a-164">Nouvelles fonctionnalités dans la version bêta 3 pour ASP.NET Web Pages avec syntaxe Razor</span><span class="sxs-lookup"><span data-stu-id="4188a-164">New Features in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a><span data-ttu-id="4188a-165">Nouveauté : Méthode de « Utilisation de Html.Raw » restitue un balisage non codé</span><span class="sxs-lookup"><span data-stu-id="4188a-165">New: "Html.Raw" method renders unencoded markup</span></span>

> <span data-ttu-id="4188a-166">La nouvelle `Html.Raw` méthode vous permet de restituer le balisage HTML en tant que balisage au lieu du rendu de sortie encodée.</span><span class="sxs-lookup"><span data-stu-id="4188a-166">The new `Html.Raw` method lets you render HTML markup as markup instead of rendering encoded output.</span></span> <span data-ttu-id="4188a-167">(Par défaut, ASP.NET Razor encode les chaînes avant leur rendu.) La syntaxe est :</span><span class="sxs-lookup"><span data-stu-id="4188a-167">(By default, ASP.NET Razor encodes strings before rendering them.) The syntax is:</span></span>
> 
> `Html.Raw(value)`
> 
> <span data-ttu-id="4188a-168">L'exemple suivant montre comment utiliser `Html.Raw` :</span><span class="sxs-lookup"><span data-stu-id="4188a-168">The following example shows how to use `Html.Raw`:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="4188a-169">Modifications dans la version bêta 3 pour ASP.NET Web Pages avec syntaxe Razor</span><span class="sxs-lookup"><span data-stu-id="4188a-169">Changes in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="change-hrefattribute-method-removed"></a><span data-ttu-id="4188a-170">Change : Méthode de « HrefAttribute » supprimée</span><span class="sxs-lookup"><span data-stu-id="4188a-170">Change: "HrefAttribute" method removed</span></span>

> <span data-ttu-id="4188a-171">Le `HrefAttribute` méthode de la `WebPage` classe a été supprimée.</span><span class="sxs-lookup"><span data-stu-id="4188a-171">The `HrefAttribute` method of the `WebPage` class has been removed.</span></span> <span data-ttu-id="4188a-172">Ce programme d’assistance a été utilisé pour encoder des caractères non sécurisés dans les URL.</span><span class="sxs-lookup"><span data-stu-id="4188a-172">This helper was used to encode unsafe characters in URLs.</span></span> <span data-ttu-id="4188a-173">Il n’est plus nécessaire, car ASP.NET Razor encode automatiquement les chaînes.</span><span class="sxs-lookup"><span data-stu-id="4188a-173">It is no longer required because ASP.NET Razor automatically encodes strings.</span></span> <span data-ttu-id="4188a-174">(Utilisez la nouvelle `Html.Raw` méthode pour restituer des chaînes non codés.)</span><span class="sxs-lookup"><span data-stu-id="4188a-174">(Use the new `Html.Raw` method to render unencoded strings.)</span></span>


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a><span data-ttu-id="4188a-175">Modification : Syntaxe déclarative «@helper« helpers modifiés</span><span class="sxs-lookup"><span data-stu-id="4188a-175">Change: Syntax for declarative "@helper" helpers changed</span></span>

> <span data-ttu-id="4188a-176">Dans la version bêta 3, qu’apporte ASP.NET comment il analyse les programmes d’assistance qui sont créés à l’aide de la `@helper` syntaxe.</span><span class="sxs-lookup"><span data-stu-id="4188a-176">In the Beta 3 release, ASP.NET changes how it parses helpers that are created using the `@helper` syntax.</span></span> <span data-ttu-id="4188a-177">En bref, le `@helper` syntaxe est maintenant analysée comme un bloc de code et non comme un bloc de balisage qui peut inclure du code.</span><span class="sxs-lookup"><span data-stu-id="4188a-177">In essence, the `@helper` syntax is now parsed as a code block instead of as a block of markup that can include code.</span></span> <span data-ttu-id="4188a-178">Par conséquent, le code à l’intérieur du programme d’assistance sans devoir être placé entre `@{ }` blocs.</span><span class="sxs-lookup"><span data-stu-id="4188a-178">Therefore, code inside the helper does not need to be enclosed in `@{ }` blocks.</span></span> <span data-ttu-id="4188a-179">À l’inverse, balisage à l’intérieur du programme d’assistance doit être explicitement inclus dans les éléments HTML ou ASP.NET Razor `<text></text>` balises.</span><span class="sxs-lookup"><span data-stu-id="4188a-179">Conversely, markup inside the helper has to be explicitly included in HTML elements or in ASP.NET Razor `<text></text>` tags.</span></span>
> 
> <span data-ttu-id="4188a-180">Par exemple, ce qui suit `@helper` fonctionne de la syntaxe dans la version bêta 3 :</span><span class="sxs-lookup"><span data-stu-id="4188a-180">For example, the following `@helper` syntax works in the Beta 3 release:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> <span data-ttu-id="4188a-181">Dans la version bêta 3, ce programme d’assistance doit être modifiée pour ressembler à l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="4188a-181">In the Beta 3 release, this helper must be changed to look like the following example:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> <span data-ttu-id="4188a-182">Notez que le `@{ }` caractères autour du code initial dans l’application auxiliaire n’est plus utilisé.</span><span class="sxs-lookup"><span data-stu-id="4188a-182">Notice that the `@{ }` characters around the initial code in the helper is no longer used.</span></span> <span data-ttu-id="4188a-183">Il s’agit, car le contenu des programmes d’assistance est traité comme un bloc de code par défaut.</span><span class="sxs-lookup"><span data-stu-id="4188a-183">This is because the contents of the helpers are treated as a code block by default.</span></span> <span data-ttu-id="4188a-184">Le programme d’assistance restitue le balisage, qui commence à l’ouverture `<a>` balise.</span><span class="sxs-lookup"><span data-stu-id="4188a-184">The helper renders markup, which starts with the opening `<a>` tag.</span></span> <span data-ttu-id="4188a-185">Si l’application d’assistance doit effectuer le rendu du texte brut ou les balises qui n’incluent pas une balise de fermeture (par exemple, `<meta>` balises), le contenu rendu doit être dans `<text></text>` balises.</span><span class="sxs-lookup"><span data-stu-id="4188a-185">If the helper must render plain text or tags that do not include a closing tag (for example, `<meta>` tags), the content to be rendered must be in `<text></text>` tags.</span></span>


#### <a name="change-webpagecontexthttpcontext-removed"></a><span data-ttu-id="4188a-186">Modifications : « WebPageContext.HttpContext » supprimées</span><span class="sxs-lookup"><span data-stu-id="4188a-186">Change: "WebPageContext.HttpContext" removed</span></span>

> <span data-ttu-id="4188a-187">Le `WebPageContext.HttpContext` propriété a été supprimée.</span><span class="sxs-lookup"><span data-stu-id="4188a-187">The `WebPageContext.HttpContext` property has been removed.</span></span> <span data-ttu-id="4188a-188">Utilisez plutôt `HttpContext.Current`.</span><span class="sxs-lookup"><span data-stu-id="4188a-188">Use `HttpContext.Current` instead.</span></span> <span data-ttu-id="4188a-189">(Le `WebPageContext.HttpContext` propriété simplement encapsulée cela.)</span><span class="sxs-lookup"><span data-stu-id="4188a-189">(The `WebPageContext.HttpContext` property simply wrapped this.)</span></span>


#### <a name="change-facebook-helper-moved-to-new-package"></a><span data-ttu-id="4188a-190">Modification : Déplacé vers le nouveau package application auxiliaire de « Facebook »</span><span class="sxs-lookup"><span data-stu-id="4188a-190">Change: "Facebook" helper moved to new package</span></span>

> <span data-ttu-id="4188a-191">Le `Facebook` helper a été déplacé vers le *Facebook.Helper* library, qui inclut le `Facebook` d’assistance et des fonctionnalités supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="4188a-191">The `Facebook` helper has been moved to the *Facebook.Helper* library, which includes the `Facebook` helper and additional functionality.</span></span> <span data-ttu-id="4188a-192">Vous devez installer cette bibliothèque en tant qu’un package distinct, comme décrit dans « L’installation de programmes d’assistance avec Package Manager » dans le didacticiel [mise en route avec les Pages ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).</span><span class="sxs-lookup"><span data-stu-id="4188a-192">You must install this library as a separate package, as described in "Installing Helpers with Package Manager" in the tutorial [Getting Started with ASP.NET Pages](https://go.microsoft.com/fwlink/?LinkId=202889).</span></span>


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a><span data-ttu-id="4188a-193">Modification : L’appartenance au rôle et la sécurité des types déplace au nouvel assembly</span><span class="sxs-lookup"><span data-stu-id="4188a-193">Change: Membership, Role, and Security types moves to new assembly</span></span>

> <span data-ttu-id="4188a-194">Les types suivants ont été déplacées vers le `WebMatrix.WebData` assembly :</span><span class="sxs-lookup"><span data-stu-id="4188a-194">The following types were moved to the `WebMatrix.WebData` assembly:</span></span>
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a><span data-ttu-id="4188a-195">Modification : Classe « TagBuilder » déplacé vers System.Web.WebPages.dll assembly</span><span class="sxs-lookup"><span data-stu-id="4188a-195">Change: "TagBuilder" class moved to System.Web.WebPages.dll assembly</span></span>

> <span data-ttu-id="4188a-196">Le `TagBuilde` classe r a été déplacée vers l’assembly System.Web.WebPages.dll.</span><span class="sxs-lookup"><span data-stu-id="4188a-196">The `TagBuilde` r class has been moved to the System.Web.WebPages.dll assembly.</span></span> <span data-ttu-id="4188a-197">Auparavant, cette opération était dans un assembly qui faisait partie d’ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="4188a-197">Previously, this was in an assembly that was part of ASP.NET MVC.</span></span> <span data-ttu-id="4188a-198">Cette modification signifie que vous n’êtes pas obligé d’installer ASP.NET MVC pour pouvoir utiliser le `TagBuilder` classe.</span><span class="sxs-lookup"><span data-stu-id="4188a-198">This change means that you do not have to install ASP.NET MVC in order to use the `TagBuilder` class.</span></span>
> 
> <span data-ttu-id="4188a-199">Toutefois, la classe est toujours dans le `System.Web.Mvc` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="4188a-199">However, the class is still in the `System.Web.Mvc` namespace.</span></span> <span data-ttu-id="4188a-200">Pour pouvoir utiliser le `TagBuilder` classe (par exemple, dans un ASP.NET Razor d’assistance personnalisée), vous devez référencer l’espace de noms (par exemple, en ajoutant `@using System.Web.Mvc` à votre code).</span><span class="sxs-lookup"><span data-stu-id="4188a-200">In order to use the `TagBuilder` class (for example, in a custom ASP.NET Razor helper), you must reference the namespace (for example, by adding `@using System.Web.Mvc` to your code).</span></span>


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a><span data-ttu-id="4188a-201">Modification : Demande syntaxe de validation modifié ; Classe de « Validation » supprimé</span><span class="sxs-lookup"><span data-stu-id="4188a-201">Change: Request validation syntax changed; "Validation" class removed</span></span>

> <span data-ttu-id="4188a-202">Dans la version bêta 3, pour désactiver la validation pour un champ individuel ou un ensemble de champs, que vous pouvez appeler la `Validation.Exclude` méthode, en passant l’ou les noms des champs à exclure de la validation.</span><span class="sxs-lookup"><span data-stu-id="4188a-202">In the Beta 3 release, to disable validation for an individual field or set of fields, you can call the `Validation.Exclude` method, passing in the name or names of the fields to exclude from validation.</span></span> <span data-ttu-id="4188a-203">Une nouvelle syntaxe est disponible dans la version bêta 3 pour contourner la validation.</span><span class="sxs-lookup"><span data-stu-id="4188a-203">A new syntax is available in the Beta 3 release for bypassing validation.</span></span> <span data-ttu-id="4188a-204">Le `Validation` méthode utilisée dans la version bêta 3 a été supprimé.</span><span class="sxs-lookup"><span data-stu-id="4188a-204">The `Validation` method used in Beta 3 has been removed.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="4188a-205">Si vous ne désactivez pas la validation de la demande, si des utilisateurs tentent de télécharger le balisage HTML (par exemple, en utilisant un éditeur de texte sur une page), le site Web signale une erreur telle que *une valeur potentiellement dangereuse de Request.Form a été détectée à partir du client*et l’entrée d’utilisateur n’est pas acceptée.</span><span class="sxs-lookup"><span data-stu-id="4188a-205">If you do not disable request validation, if users try to upload HTML markup (for example, by using a rich text editor on a page), the website will report an error like *A potentially dangerous Request.Form value was detected from the client* and the user input is not accepted.</span></span> <span data-ttu-id="4188a-206">Si vous désactivez la validation de la demande, vous devez vérifier manuellement les entrées d’utilisateur pour vous assurer qu’il ne contient pas balisage potentiellement dangereux ou script à l’aide de quelque chose comme la [Microsoft Anti-Cross Site Scripting Library V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span><span class="sxs-lookup"><span data-stu-id="4188a-206">If you disable request validation, you must manually check user input to make sure that it does not contain potentially dangerous markup or script using something like the [Microsoft Anti-Cross Site Scripting Library V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span></span>
> 
> 
> <span data-ttu-id="4188a-207">Pour désactiver la validation de demande automatique, appelez le `Request.Unvalidated` méthode, en lui transmettant le nom du champ ou l’autre objet du billet que vous souhaitez ignorer la validation de demande pour.</span><span class="sxs-lookup"><span data-stu-id="4188a-207">To disable automatic request validation, call the `Request.Unvalidated` method, passing it the name of the field or other post object that you want to bypass request validation for.</span></span> <span data-ttu-id="4188a-208">Vous pouvez utiliser cette méthode pour ignorer la validation pour tous les éléments dans le `Form`, `QueryString`, `Cookies`, et `ServerVariables` collections.</span><span class="sxs-lookup"><span data-stu-id="4188a-208">You can use this method to bypass validation for any items in the `Form`, `QueryString`, `Cookies`, and `ServerVariables` collections.</span></span> <span data-ttu-id="4188a-209">Les exemples suivants montrent comment utiliser le `Unvalidated` méthode :</span><span class="sxs-lookup"><span data-stu-id="4188a-209">The following examples show how to use the `Unvalidated` method:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="4188a-210">Problèmes connus pour ASP.NET Web Pages avec syntaxe Razor</span><span class="sxs-lookup"><span data-stu-id="4188a-210">Known Issues for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="4188a-211">Problème : Un comportement inattendu lors de l’utilisation d’une table utilisateur personnalisée pour l’appartenance</span><span class="sxs-lookup"><span data-stu-id="4188a-211">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="4188a-212">Pour initialiser le fournisseur d’appartenances pour un site Web ASP.NET Razor, vous appelez le `WebSecurity.InitializeDatabaseConnection` (méthode).</span><span class="sxs-lookup"><span data-stu-id="4188a-212">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="4188a-213">(Dans WebMatrix, du modèle Starter Site inclut un appel à cette méthode dans le  *\_AppStart.cshtml* fichier.) Si le `autoCreateTables` paramètre de cette méthode est définie sur true (par défaut, il est défini sur true dans le modèle Starter Site), et si un nom de table non reconnu est passé à la méthode (le deuxième paramètre), la méthode ne lève pas une erreur.</span><span class="sxs-lookup"><span data-stu-id="4188a-213">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="4188a-214">Au lieu de cela, il crée automatiquement la table.</span><span class="sxs-lookup"><span data-stu-id="4188a-214">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="4188a-215">Cela peut poser un problème si vous avez l’intention d’utiliser une table utilisateur personnalisée pour l’appartenance, mais passez le nom de table incorrect à la `WebSecurity.InitializeDatabaseConnection` (méthode).</span><span class="sxs-lookup"><span data-stu-id="4188a-215">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="4188a-216">Étant donné que la méthode ne pas par défaut déclenche une erreur si la table spécifiée n’existe pas, et parce qu’il crée à la place une nouvelle table, l’application peut apparaître à travailler.</span><span class="sxs-lookup"><span data-stu-id="4188a-216">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="4188a-217">Toutefois, code d’application qui s’appuie sur la table utilisateur personnalisée (et sur les champs qu’il contient) peut finit par échouer avec des erreurs inattendues.</span><span class="sxs-lookup"><span data-stu-id="4188a-217">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="4188a-218">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="4188a-218">**Workaround**</span></span>  
> <span data-ttu-id="4188a-219">Assurez-vous que le nom transmis dans le `InitializeDatabaseConnection` correspondances méthode le profil utilisateur figurant dans la base de données d’appartenance, ou vous assurer que le `autoCreateTables` paramètre est défini sur false.</span><span class="sxs-lookup"><span data-stu-id="4188a-219">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="4188a-220">Problème : erreur « Échec de générer une instance utilisateur de SQL Server »</span><span class="sxs-lookup"><span data-stu-id="4188a-220">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="4188a-221">Si une application Web de WebMatrix utilise SQL Server Express et est en cours d’exécution IIS 7.5 sur Windows 7 ou Windows Server 2008 R2, vous pouvez voir une erreur qui indique que SQL Server ne peut pas récupérer le chemin d’accès de l’utilisateur local d’application en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="4188a-221">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="4188a-222">**Solution de contournement** vous assurer que le compte Windows de l’application sous lequel s’exécute (généralement NETWORK SERVICE) dispose des autorisations de lecture/écriture pour les dossiers racine de l’application et des sous-dossiers comme *application\_données*.</span><span class="sxs-lookup"><span data-stu-id="4188a-222">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="4188a-223">Des informations plus détaillées sont disponibles dans l’article de la base de connaissances [des problèmes avec les projets d’Application Web ASP.net et d’utilisateur SQL Server Express l'instanciation](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="4188a-223">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a><span data-ttu-id="4188a-224">Problème : Dans Visual Studio, espaces de noms pour les assemblys personnalisés (DLL) ne sont pas importés automatiquement</span><span class="sxs-lookup"><span data-stu-id="4188a-224">Issue: In Visual Studio, namespaces for custom assemblies (DLLs) are not imported automatically</span></span>

> <span data-ttu-id="4188a-225">Si vous utilisez des assemblys personnalisés dans un projet dans Visual Studio, les espaces de noms déclarés dans ces assemblys ne sont pas importés automatiquement au moment du design.</span><span class="sxs-lookup"><span data-stu-id="4188a-225">If you use custom assemblies in a project in Visual Studio, the namespaces declared in those assemblies are not automatically imported at design time.</span></span> <span data-ttu-id="4188a-226">Par conséquent, les références à des types personnalisés peuvent ne pas être reconnus au moment du design et sont marqués comme non reconnu dans Visual Studio (à l’aide d’un tilde « »).</span><span class="sxs-lookup"><span data-stu-id="4188a-226">As a result, references to custom types might not be recognized at design time and are marked as not recognized in Visual Studio (using a "squiggle").</span></span> <span data-ttu-id="4188a-227">Ce problème se produit uniquement au moment du design dans Visual Studio. l’application elle-même s’exécute correctement.</span><span class="sxs-lookup"><span data-stu-id="4188a-227">This problem occurs only at design time in Visual Studio; the application itself runs properly.</span></span>
> 
> <span data-ttu-id="4188a-228">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="4188a-228">**Workaround**</span></span>  
> <span data-ttu-id="4188a-229">Inclure un `using` instruction (`imports` en Visual Basic) qui référence les entités qui ne sont pas reconnues au moment du design.</span><span class="sxs-lookup"><span data-stu-id="4188a-229">Include a `using` statement (`imports` in Visual Basic) that references the entities that are not recognized at design time.</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="4188a-230">Problème : Visual Studio IntelliSense modèles de projet et disponibles uniquement dans ASP.NET MVC version 3</span><span class="sxs-lookup"><span data-stu-id="4188a-230">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="4188a-231">L’installation d’ASP.NET Web Pages ne pas également installe outils pour Visual Studio telles que des modèles de projet et IntelliSense pour les applications ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="4188a-231">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="4188a-232">**Solution de contournement** pour utiliser des modèles de projet et IntelliSense pour les applications de Pages Web ASP.NET dans Visual Studio, installer ASP.NET MVC 3 RC via Web Platform Installer ou le [programme d’installation autonome](https://go.microsoft.com/fwlink/?LinkID=191797).</span><span class="sxs-lookup"><span data-stu-id="4188a-232">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a><span data-ttu-id="4188a-233">Problème : «&lt;helper&gt; Impossible de trouver la classe « erreur</span><span class="sxs-lookup"><span data-stu-id="4188a-233">Issue: "&lt;helper&gt; class cannot be found" error</span></span>

> <span data-ttu-id="4188a-234">Une fois que vous mettez à niveau vers la version bêta 3, vous pouvez obtenir une erreur qui une classe d’assistance (par exemple, le `Facebook` classe) ne peut pas être introuvable.</span><span class="sxs-lookup"><span data-stu-id="4188a-234">After you upgrade to Beta 3, you might see an error that a helper class (for example, the `Facebook` class) cannot not be found.</span></span> <span data-ttu-id="4188a-235">À compter de la version bêta 2 et continuer dans la version bêta 3, les programmes d’assistance ont été déplacés aux packages que vous devez installer explicitement.</span><span class="sxs-lookup"><span data-stu-id="4188a-235">Starting in Beta 2 and continuing in Beta 3, helpers have been moved to packages that you must explicitly install.</span></span> <span data-ttu-id="4188a-236">Sites existants ne sont pas mis à niveau pour inclure ces packages ; Cela inclut les sites dans le *\My Documents\IISExpress* ou *\My Documents\My Sites Web* dossiers.</span><span class="sxs-lookup"><span data-stu-id="4188a-236">Existing sites are not upgraded to include these packages; this includes sites in the *\My Documents\IISExpress* or *\My Documents\My Web Sites* folders.</span></span> <span data-ttu-id="4188a-237">En particulier, vous verrez cette erreur si vous utilisez le site par défaut dans *Mes Sites* (WebSite1), qui inclut une référence à la `Twitter` helper.</span><span class="sxs-lookup"><span data-stu-id="4188a-237">In particular, you will see this error if you use the default site in *My Sites* (WebSite1), which includes a reference to the `Twitter` helper.</span></span>
> 
> <span data-ttu-id="4188a-238">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="4188a-238">**Workaround**</span></span>  
> <span data-ttu-id="4188a-239">Commentez les appels à des programmes d’assistance dans le site, exécutez le  *\_administrateur* page et installer l’ou les packages qui incluent les programmes d’assistance que vous souhaitez utiliser.</span><span class="sxs-lookup"><span data-stu-id="4188a-239">Comment out calls to any helpers in the site, run the *\_Admin* page, and install the package or packages that include the helpers that you want to use.</span></span> <span data-ttu-id="4188a-240">Une fois que vous avez installé le package, vous pouvez ne pas commenter les lignes qui référencent des programmes d’assistance.</span><span class="sxs-lookup"><span data-stu-id="4188a-240">After you've installed the package, you can uncomment the lines that reference helpers.</span></span>


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a><span data-ttu-id="4188a-241">Problème : Déploiement d’assemblys de la version bêta 3 ASP.NET Razor dans le dossier Bin peut ne pas fonctionne sur l’hébergement de sites</span><span class="sxs-lookup"><span data-stu-id="4188a-241">Issue: Deploying Beta 3 ASP.NET Razor assemblies to the Bin folder might not work on hosting sites</span></span>

> <span data-ttu-id="4188a-242">Si vous déployez un site Web ASP.NET Web Pages sur un site d’hébergement, et si vous déployez les assemblys ASP.NET Razor bêta 3 sur le site *Bin* dossier, vous pouvez rencontrer des erreurs, y compris les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="4188a-242">If you deploy an ASP.NET Web Pages website to a hosting site, and if you deploy the ASP.NET Razor Beta 3 assemblies to the site's *Bin* folder, you might experience errors, including the following:</span></span>
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> <span data-ttu-id="4188a-243">Cela peut se produire si le fournisseur d’hébergement a installé les assemblys ASP.NET Web Pages bêta 1 dans le cache du serveur application global (GAC).</span><span class="sxs-lookup"><span data-stu-id="4188a-243">This can happen if the hosting provider has installed the ASP.NET Web Pages Beta 1 assemblies into the server's global application cache (GAC).</span></span> <span data-ttu-id="4188a-244">Assemblys dans le GAC obtenir la priorité sur les assemblys installés localement dans le *Bin* dossier.</span><span class="sxs-lookup"><span data-stu-id="4188a-244">Assemblies in the GAC get precedence over assemblies installed locally in the *Bin* folder.</span></span>
> 
> <span data-ttu-id="4188a-245">**Solution de contournement** contactez votre fournisseur d’hébergement pour confirmer que les erreurs que vous voyez sont en raison d’un conflit entre les versions du fournisseur des assemblys et les vôtres.</span><span class="sxs-lookup"><span data-stu-id="4188a-245">**Workaround** Contact your hosting provider to confirm that the errors you are seeing are due to a conflict between the provider's versions of the assemblies and yours.</span></span> <span data-ttu-id="4188a-246">Dans ce cas, demander que le fournisseur d’hébergement mettre à jour les assemblys dans le GAC du serveur.</span><span class="sxs-lookup"><span data-stu-id="4188a-246">If so, request that the hosting provider update the assemblies in the server's GAC.</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="4188a-247">Problème : Lecture des flux ou autres données externes via un serveur proxy</span><span class="sxs-lookup"><span data-stu-id="4188a-247">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="4188a-248">Si le serveur qui exécute le site est derrière un serveur proxy, vous devrez peut-être configurer les informations de proxy dans le *Web.config* fichier afin d’être en mesure de lire les informations provenant de l’extérieur de votre site.</span><span class="sxs-lookup"><span data-stu-id="4188a-248">If the server running the site is behind a proxy server, you might need to configure proxy information in the *Web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="4188a-249">Par exemple, si vous utilisez le `ReCaptcha` helper, l’application d’assistance communique avec le service reCAPTCHA, mais peut être bloquée par votre serveur proxy.</span><span class="sxs-lookup"><span data-stu-id="4188a-249">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="4188a-250">De même, les flux sont utilisés dans les Pages Web ASP.NET, par exemple, le flux utilisé par le Gestionnaire de package, peuvent nécessiter la configuration du proxy.</span><span class="sxs-lookup"><span data-stu-id="4188a-250">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="4188a-251">Si vous rencontrez des problèmes dans avec un service externe ou fonctionnent correctement avec le flux du package, placez les éléments suivants dans la racine de votre application *Web.config* fichier :</span><span class="sxs-lookup"><span data-stu-id="4188a-251">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *Web.config* file:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> <span data-ttu-id="4188a-252">Pour plus d’informations sur la configuration d’un serveur proxy, consultez [ &lt;proxy&gt; , élément (paramètres réseau)](https://msdn.microsoft.com/library/sa91de1e.aspx) sur le site Web MSDN.</span><span class="sxs-lookup"><span data-stu-id="4188a-252">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a><span data-ttu-id="4188a-253">Problème : L’erreur « Impossible de charger Microsoft.Web.Infrastructure.dll »</span><span class="sxs-lookup"><span data-stu-id="4188a-253">Issue: "Microsoft.Web.Infrastructure.dll cannot be loaded" error</span></span>

> <span data-ttu-id="4188a-254">Si vous déjà installé la version bêta 1 d’ASP.NET Web Pages avec syntaxe Razor et puis installez la version bêta 3, tous les assemblys appropriés sont installés dans le GAC à l’exception *Microsoft.Web.Infrastructure.dll*.</span><span class="sxs-lookup"><span data-stu-id="4188a-254">If you previously installed the Beta 1 version of ASP.NET Web Pages with Razor syntax and then install the Beta 3 version, all appropriate assemblies are installed in the GAC except *Microsoft.Web.Infrastructure.dll*.</span></span> <span data-ttu-id="4188a-255">Par conséquent, lorsque vous exécutez des pages ASP.NET Razor, vous voyez une erreur qui indique que *Microsoft.Web.Infrastructure.dll* pas pu être chargé.</span><span class="sxs-lookup"><span data-stu-id="4188a-255">As a consequence, when you run ASP.NET Razor pages, you see an error that indicates that *Microsoft.Web.Infrastructure.dll* could not be loaded.</span></span>
> 
> <span data-ttu-id="4188a-256">Ce problème ne se produit pas si vous avez chargé la version bêta 3 sur un ordinateur sain.</span><span class="sxs-lookup"><span data-stu-id="4188a-256">This issue does not occur if you loaded the Beta 3 release on a clean computer.</span></span>
> 
> <span data-ttu-id="4188a-257">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="4188a-257">**Workaround**</span></span>  
> <span data-ttu-id="4188a-258">Dans le panneau de configuration, désinstallez les Pages Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4188a-258">In Control Panel, uninstall ASP.NET Web Pages.</span></span> <span data-ttu-id="4188a-259">Puis réinstallez la version bêta 3.</span><span class="sxs-lookup"><span data-stu-id="4188a-259">Then reinstall the Beta 3 release.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="4188a-260">Problème : La désinstallation de la version 4 du .NET Framework désactive ASP.NET Web Pages avec syntaxe Razor</span><span class="sxs-lookup"><span data-stu-id="4188a-260">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="4188a-261">Si vous désinstallez le Kit de développement .NET Framework version 4, puis le réinstaller, ASP.NET Web Pages avec syntaxe Razor est désactivés.</span><span class="sxs-lookup"><span data-stu-id="4188a-261">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="4188a-262">Pages avec la *.cshtml* extension ne s’exécutent pas correctement.</span><span class="sxs-lookup"><span data-stu-id="4188a-262">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="4188a-263">ASP.NET Web Pages enregistre un assembly dans la racine de la machine *Web.config* des fichiers et la suppression de .NET Framework supprime ce fichier.</span><span class="sxs-lookup"><span data-stu-id="4188a-263">ASP.NET Web Pages registers an assembly in the machine root *Web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="4188a-264">Réinstallez le .NET Framework installe une nouvelle version du fichier de configuration, mais n’ajoute pas la référence de l’assembly d’ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="4188a-264">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="4188a-265">**Solution de contournement** après la réinstallation de .NET Framework, réinstallez ASP.NET Web Pages avec syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="4188a-265">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="4188a-266">Cette opération ajoute l’élément suivant à la *Web.config* fichier à la racine de l’ordinateur, qui est en général à l’emplacement suivant :</span><span class="sxs-lookup"><span data-stu-id="4188a-266">This adds the following element to the *Web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a><span data-ttu-id="4188a-267">Problème : Les Applications précédemment déployées avec des assemblys ASP.NET dans le dossier Bin rencontrent des erreurs</span><span class="sxs-lookup"><span data-stu-id="4188a-267">Issue: Applications previously deployed with ASP.NET assemblies in the Bin folder experience errors</span></span>

> <span data-ttu-id="4188a-268">Au cours du déploiement, les copies des assemblys ASP.NET Web Pages (par exemple, *Microsoft.WebPages.dll*) pour le *Bin* dossier du site Web sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="4188a-268">During deployment, copies of the ASP.NET Web Pages assemblies (for example, *Microsoft.WebPages.dll*) to the *Bin* folder of the website on the server.</span></span> <span data-ttu-id="4188a-269">(Cela peut se sont produites automatiquement pendant le déploiement ou parce que le développeur copié explicitement les assemblys.) Toutefois, lorsque la version bêta 3 est installée, les erreurs se produit, telles que les erreurs qui ne figure pas certains types.</span><span class="sxs-lookup"><span data-stu-id="4188a-269">(This might have happened automatically during deployment or because the developer explicitly copied the assemblies.) However, when the Beta 3 release is installed, errors occurs, such as errors that certain types cannot be found.</span></span> <span data-ttu-id="4188a-270">Cela se produit, car un nombre de types de Pages Web ASP.NET ont été déplacées dans différents espaces de noms pour la version bêta 3.</span><span class="sxs-lookup"><span data-stu-id="4188a-270">This occurs because a number of ASP.NET Web Pages types were moved into different namespaces for the Beta 3 release.</span></span>
> 
> <span data-ttu-id="4188a-271">**Solution de contournement** </span><span class="sxs-lookup"><span data-stu-id="4188a-271">**Workaround** </span></span>  
> <span data-ttu-id="4188a-272">Effacer la *Bin* dossier de l’application déployée, copier les nouveaux assemblys dans le dossier (ou redéployer l’application), puis redémarrez l’application.</span><span class="sxs-lookup"><span data-stu-id="4188a-272">Clear the *Bin* folder of the deployed application, copy the new assemblies to the folder (or redeploy the application), and then restart the application.</span></span>


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="4188a-273">Problème : Les URL sans extension ne trouvent pas les fichiers.cshtml/.vbhtml sur IIS 7 ou IIS 7.5</span><span class="sxs-lookup"><span data-stu-id="4188a-273">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="4188a-274">Sur IIS 7 ou IIS 7.5, les requêtes avec une URL semblable à la suivante ne sont pas en mesure de trouver des pages qui ont le *.cshtml* ou *.vbhtml* extension :</span><span class="sxs-lookup"><span data-stu-id="4188a-274">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="4188a-275">Le problème survient car la réécriture d’URL n’est pas activée par défaut pour IIS 7 ou IIS 7.5.</span><span class="sxs-lookup"><span data-stu-id="4188a-275">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="4188a-276">Le scénario voyez généralement cette est que vous ne voyez pas le problème lorsque vous testez localement à l’aide d’IIS Express, mais que vous le rencontrez lorsque vous déployez votre site Web sur un site Web d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="4188a-276">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="4188a-277">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="4188a-277">**Workaround**</span></span>
> 
> - <span data-ttu-id="4188a-278">Si vous avez un contrôle sur l’ordinateur du serveur, sur l’ordinateur serveur installer la mise à jour qui est décrite dans [une mise à jour est disponible que permet certains gestionnaires d’IIS 7.0 ou IIS 7.5 pour gérer les demandes dont les URL ne se termine pas par un point](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="4188a-278">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="4188a-279">Si vous n’avez pas de contrôle sur l’ordinateur du serveur (par exemple, vous déployez sur un site Web d’hébergement), ajoutez le code suivant du site Web *Web.config* fichier :</span><span class="sxs-lookup"><span data-stu-id="4188a-279">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *Web.config* file:</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a><span data-ttu-id="4188a-280">Problème : À l’aide de projet d’Application Web ou ASP.NET MVC et ASP.NET Web pages dans la même application</span><span class="sxs-lookup"><span data-stu-id="4188a-280">Issue: Using Web Application Project or ASP.NET MVC and ASP.NET Web pages in the same application</span></span>

> <span data-ttu-id="4188a-281">Si vous utilisiez ASP.NET Web Pages dans un projet d’Application Web ou d’une application ASP.NET MVC, vous pouvez obtenir une erreur qui *WebPageHttpApplication* est introuvable.</span><span class="sxs-lookup"><span data-stu-id="4188a-281">If you were using ASP.NET Web Pages in a Web Application project or ASP.NET MVC application, you might see an error that *WebPageHttpApplication* cannot be found.</span></span>
> 
> <span data-ttu-id="4188a-282">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="4188a-282">**Workaround**</span></span>  
> <span data-ttu-id="4188a-283">Si vous obtenez cette erreur, modifiez la classe de base dont dérive l’application.</span><span class="sxs-lookup"><span data-stu-id="4188a-283">If you get this error, change the base class from which the application derives.</span></span> <span data-ttu-id="4188a-284">Dans le *Global.asax* , modifiez la ligne suivante :</span><span class="sxs-lookup"><span data-stu-id="4188a-284">In the *Global.asax* file, change the following line:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> <span data-ttu-id="4188a-285">Par ceci :</span><span class="sxs-lookup"><span data-stu-id="4188a-285">To this:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> <span data-ttu-id="4188a-286">Cela inverse ce qui entraîne une modification a été introduite pour la version bêta 1 d’ASP.NET Web Pages avec syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="4188a-286">This in effect reverses a change that was introduced for the Beta 1 release of ASP.NET Web Pages with Razor syntax.</span></span>


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="4188a-287">Problème : Déploiement d’une application sur un ordinateur qui n’a pas de SQL Server Compact installé</span><span class="sxs-lookup"><span data-stu-id="4188a-287">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="4188a-288">Les applications qui incluent des bases de données SQL Server Compact peuvent s’exécuter sur un ordinateur où SQL Server Compact n’est pas installé.</span><span class="sxs-lookup"><span data-stu-id="4188a-288">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="4188a-289">Version bêta 3 de Microsoft WebMatrix automatiquement des copies de ces fichiers binaires et effectue approprié *Web.config* transformations de fichiers.</span><span class="sxs-lookup"><span data-stu-id="4188a-289">Microsoft WebMatrix Beta 3 automatically copies these binaries for you and performs the appropriate *Web.config* file transforms.</span></span>
> 
> <span data-ttu-id="4188a-290">**Solution de contournement** si vous devez copier ces fichiers et rendre le *Web.config* modifications de fichiers manuellement, effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="4188a-290">**Workaround** If you need to copy these files and make the *Web.config* file changes manually, do the following :</span></span>
> 
> 1. <span data-ttu-id="4188a-291">Copiez les assemblys du moteur de base de données à la *Bin* dossier (et sous-dossiers) de l’application sur l’ordinateur cible :</span><span class="sxs-lookup"><span data-stu-id="4188a-291">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span> 
> 
>     - <span data-ttu-id="4188a-292">Copie *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **à** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="4188a-292">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*</span></span>
>     - <span data-ttu-id="4188a-293">Copie *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **à** *\Bin\x86*</span><span class="sxs-lookup"><span data-stu-id="4188a-293">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **to** *\Bin\x86*</span></span>
>     - <span data-ttu-id="4188a-294">Copie *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **à** *\Bin\amd64*</span><span class="sxs-lookup"><span data-stu-id="4188a-294">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **to** *\Bin\amd64*</span></span>
> 2. <span data-ttu-id="4188a-295">Dans le dossier racine du site Web, créez ou ouvrez un *Web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="4188a-295">In the root folder of the website, create or open a *Web.config* file.</span></span> <span data-ttu-id="4188a-296">(Dans la version bêta 3 de WebMatrix, ce type de fichier est disponible si vous cliquez sur **tous les** dans le **choisir un Type de fichier** boîte de dialogue.)</span><span class="sxs-lookup"><span data-stu-id="4188a-296">(In WebMatrix Beta 3, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="4188a-297">Ajoutez l’élément suivant en tant qu’enfant de le **&lt;configuration&gt;** élément (pas à l’intérieur de la **&lt;system.web&gt;** élément) :</span><span class="sxs-lookup"><span data-stu-id="4188a-297">Add the following element as a child of the **&lt;configuration&gt;** element (not inside the **&lt;system.web&gt;** element):</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="4188a-298">Problème : La base de données et de WebGrid helpers ne fonctionnent pas dans Medium Trust en Visual Basic</span><span class="sxs-lookup"><span data-stu-id="4188a-298">Issue: Database and WebGrid helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="4188a-299">Si vous utilisez Visual Basic (création *.vbhtml* fichiers), le `Database` et `WebGrid` helpers ne fonctionnera pas si l’application est configurée pour utiliser la confiance moyenne.</span><span class="sxs-lookup"><span data-stu-id="4188a-299">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="4188a-300">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="4188a-300">**Workaround**</span></span>  
> <span data-ttu-id="4188a-301">Définissez temporairement l’application pour utiliser la confiance totale.</span><span class="sxs-lookup"><span data-stu-id="4188a-301">Temporarily set the application to use Full Trust.</span></span>

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a><span data-ttu-id="4188a-302">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="4188a-302">SQL Server Compact</span></span>

#### <a name="issue-encrypt-property-is-not-recognized"></a><span data-ttu-id="4188a-303">Problème : Propriété de « Encrypt » n’est pas reconnue.</span><span class="sxs-lookup"><span data-stu-id="4188a-303">Issue: "Encrypt" property is not recognized</span></span>

> <span data-ttu-id="4188a-304">SQL Server Compact 4.0 ne reconnaît pas le `Encrypt` propriété de la `SqlCeConnection` classe.</span><span class="sxs-lookup"><span data-stu-id="4188a-304">SQL Server Compact 4.0 does not recognize the `Encrypt` property of the `SqlCeConnection` class.</span></span> <span data-ttu-id="4188a-305">Vous n’utilisez pas cette propriété pour chiffrer les fichiers de base de données.</span><span class="sxs-lookup"><span data-stu-id="4188a-305">You should not use this property to encrypt database files.</span></span> <span data-ttu-id="4188a-306">Le `Encrypt` propriété a été déconseillée dans la version 3.5 de SQL Server Compact et a été conservée uniquement pour la compatibilité descendante.</span><span class="sxs-lookup"><span data-stu-id="4188a-306">The `Encrypt` property was deprecated in SQL Server Compact 3.5 release and was retained only for backward compatibility.</span></span> 
> 
> <span data-ttu-id="4188a-307">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="4188a-307">**Workaround**</span></span>  
> <span data-ttu-id="4188a-308">Utilisez le `Encryption Mode` propriété de la `SqlCeConnection` classe pour chiffrer les fichiers de base de données SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="4188a-308">Use the `Encryption Mode` property of the `SqlCeConnection` class to encrypt SQL Server Compact 4.0 database files.</span></span> <span data-ttu-id="4188a-309">L’exemple suivant montre comment créer une base de données SQL Server Compact 4.0 chiffrée à l’aide du `Encryption Mode` propriété :</span><span class="sxs-lookup"><span data-stu-id="4188a-309">The following example shows how to create an encrypted SQL Server Compact 4.0 database using the `Encryption Mode` property:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> <span data-ttu-id="4188a-310">Pour modifier le mode de chiffrement de base de données SQL Server Compact 4.0 existante, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="4188a-310">To change the encryption mode of an existing SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> <span data-ttu-id="4188a-311">Pour chiffrer une base de données SQL Server Compact 4.0 non chiffré, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="4188a-311">To encrypt an unencrypted SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a><span data-ttu-id="4188a-312">Problème : Les bibliothèques runtime Microsoft Visual C++ 2008 sont requis</span><span class="sxs-lookup"><span data-stu-id="4188a-312">Issue: Microsoft Visual C++ 2008 runtime libraries are required</span></span>

> <span data-ttu-id="4188a-313">Le natif DLL de SQL Server Compact 4.0 doivent le Microsoft Visual C++ 2008 bibliothèques Runtime (x 86, IA64 et x 64), le Service Pack 1.</span><span class="sxs-lookup"><span data-stu-id="4188a-313">The native DLLs of SQL Server Compact 4.0 need the Microsoft Visual C++ 2008 Runtime Libraries (x86, IA64, and x64), Service Pack 1.</span></span>
> 
> <span data-ttu-id="4188a-314">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="4188a-314">**Workaround**</span></span>  
> <span data-ttu-id="4188a-315">Installer le .NET Framework 3.5 SP1.</span><span class="sxs-lookup"><span data-stu-id="4188a-315">Install the .NET Framework 3.5 SP1.</span></span> <span data-ttu-id="4188a-316">Cette commande installe également le Runtime bibliothèques de Visual C++ 2008 SP1.</span><span class="sxs-lookup"><span data-stu-id="4188a-316">This also installs the Visual C++ 2008 Runtime Libraries SP1.</span></span> <span data-ttu-id="4188a-317">Vous pouvez télécharger les bibliothèques à partir de l’emplacement suivant :</span><span class="sxs-lookup"><span data-stu-id="4188a-317">You can download the libraries from the following location:</span></span>   
>   
> [<span data-ttu-id="4188a-318">Mise à jour de la sécurité de Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL</span><span class="sxs-lookup"><span data-stu-id="4188a-318">Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL Security Update</span></span>](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> <span data-ttu-id="4188a-319">Notez que le .NET Framework 2.0, 3.0, l’installation ou de fait de 4 *pas* installer le SP1 bibliothèques Runtime de Visual C++ 2008.</span><span class="sxs-lookup"><span data-stu-id="4188a-319">Note that installing the .NET Framework 2.0, 3.0, or 4 does *not* install the Visual C++ 2008 Runtime Libraries SP1.</span></span>


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a><span data-ttu-id="4188a-320">Problème : Si SQL Server Compact est installé avant d’installer le .NET Framework sur l’ordinateur, son nom invariant du fournisseur n'est pas inscrit dans le fichier machine.config de .NET Framework</span><span class="sxs-lookup"><span data-stu-id="4188a-320">Issue: If SQL Server Compact is installed prior to installing .NET Framework on the computer, its provider invariant name is not registered in the .NET Framework machine.config file</span></span>

> <span data-ttu-id="4188a-321">SQL Server Compact peut être installé sur un ordinateur qui n’a pas de .NET Framework est installé, car SQL Server Compact nécessite le .NET framework.</span><span class="sxs-lookup"><span data-stu-id="4188a-321">SQL Server Compact can be installed on a machine that does not have .NET Framework installed because SQL Server Compact does require the .NET framework.</span></span> <span data-ttu-id="4188a-322">Si ni .NET Framework version 3.5 ni 4 est installé avant d’installer SQL Server Compact, le programme d’installation SQL Server Compact n’inscrit pas son nom invariant du fournisseur dans le *machine.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="4188a-322">If neither .NET Framework version 3.5 nor 4 is installed before you install SQL Server Compact, the SQL Server Compact Setup does not register its provider invariant name in the *machine.config* file.</span></span> <span data-ttu-id="4188a-323">N’importe quelle application qui s’appuie sur l’entrée de SQL Server Compact dans le *machine.config* fichier échoue.</span><span class="sxs-lookup"><span data-stu-id="4188a-323">Any application that relies on the SQL Server Compact entry in the *machine.config* file will fail.</span></span> <span data-ttu-id="4188a-324">L’entrée d’inscription de nom invariant dans *machine.config* ressemble à l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="4188a-324">The invariant name registration entry in *machine.config* looks like the following example:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> <span data-ttu-id="4188a-325">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="4188a-325">**Workaround**</span></span>  
> <span data-ttu-id="4188a-326">Désinstaller SQL Server Compact 4.0 CTP1.</span><span class="sxs-lookup"><span data-stu-id="4188a-326">Uninstall SQL Server Compact 4.0 CTP1.</span></span> <span data-ttu-id="4188a-327">Téléchargez et installez les versions complètes de .NET Framework à partir de l’emplacement suivant :</span><span class="sxs-lookup"><span data-stu-id="4188a-327">Download and install the full versions of the .NET Framework from the following location:</span></span>
> 
> [<span data-ttu-id="4188a-328">Microsoft .NET Framework 3.5 Service pack 1 (indépendant)</span><span class="sxs-lookup"><span data-stu-id="4188a-328">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [<span data-ttu-id="4188a-329">Mise en production de Microsoft .NET Framework 4.0 (indépendant)</span><span class="sxs-lookup"><span data-stu-id="4188a-329">Microsoft .NET Framework 4.0 Release (Full Package)</span></span>](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> <span data-ttu-id="4188a-330">Puis réinstallez [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span><span class="sxs-lookup"><span data-stu-id="4188a-330">Then reinstall [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span></span>


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a><span data-ttu-id="4188a-331">Installation d’Applications</span><span class="sxs-lookup"><span data-stu-id="4188a-331">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="4188a-332">Problème : Installation d’une application peut prendre un certain temps si le dossier Mes Documents de l’utilisateur est redirigé vers un partage réseau</span><span class="sxs-lookup"><span data-stu-id="4188a-332">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="4188a-333">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="4188a-333">**Workaround**</span></span>  
> <span data-ttu-id="4188a-334">Aucun.</span><span class="sxs-lookup"><span data-stu-id="4188a-334">None.</span></span> <span data-ttu-id="4188a-335">L’application peut prendre un certain temps à installer, mais ne s’installe correctement.</span><span class="sxs-lookup"><span data-stu-id="4188a-335">The application might take a while to install, but will install correctly.</span></span>


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a><span data-ttu-id="4188a-336">Publication d’Applications</span><span class="sxs-lookup"><span data-stu-id="4188a-336">Publishing Applications</span></span>

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="4188a-337">Problème : Site peuvent ne pas fonctionne après la publication si le champ « URL de Destination » n’est pas précédé par http:// ou https://</span><span class="sxs-lookup"><span data-stu-id="4188a-337">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="4188a-338">Dans le **paramètres de publication** boîte de dialogue, si l’URL de destination ne commence pas par `http://` ou `https://`, le site peut ne pas fonctionne après le déploiement.</span><span class="sxs-lookup"><span data-stu-id="4188a-338">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="4188a-339">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="4188a-339">**Workaround**</span></span>  
> <span data-ttu-id="4188a-340">Assurez-vous que l’option avant de publier un site, l’URL de destination dans le **paramètres de publication** boîte de dialogue commence par `http://` ou `https://`.</span><span class="sxs-lookup"><span data-stu-id="4188a-340">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="4188a-341">Problème : Publication d’une base de données MySQL échoue avec l’erreur « Échec de publication de la base de données.</span><span class="sxs-lookup"><span data-stu-id="4188a-341">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="4188a-342">Cela peut se produire si la base de données distante ne peut pas exécuter le script. »</span><span class="sxs-lookup"><span data-stu-id="4188a-342">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="4188a-343">L’erreur peut se produire pour plusieurs raisons.</span><span class="sxs-lookup"><span data-stu-id="4188a-343">The error can occur for a number of reasons.</span></span> <span data-ttu-id="4188a-344">Vous pouvez voir cette erreur est si le script de base de données contient un guillemet simple (') et le jeu de caractères par défaut de la destination MySQL de base de données n’est pas au format UTF-8.</span><span class="sxs-lookup"><span data-stu-id="4188a-344">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="4188a-345">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="4188a-345">**Workaround**</span></span>  
> <span data-ttu-id="4188a-346">Définir le caractère par défaut définie pour la base de données MySQL à distance au format UTF-8.</span><span class="sxs-lookup"><span data-stu-id="4188a-346">Set the default character set for the remote MySQL database to UTF-8.</span></span>


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a><span data-ttu-id="4188a-347">Autres problèmes</span><span class="sxs-lookup"><span data-stu-id="4188a-347">Other Issues</span></span>

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a><span data-ttu-id="4188a-348">Problème : Les recherche/filtre ne fonctionne pas dans les rapports pour Group By : Type de problème</span><span class="sxs-lookup"><span data-stu-id="4188a-348">Issue: Search/Filter does not work in Reports for Group By: Issue Type</span></span>

> <span data-ttu-id="4188a-349">Lorsque vous exécutez un rapport pour un site, si vous entrez du texte dans le *filtrer par URL* , puis cliquez sur *recherche*, rien ne se produit.</span><span class="sxs-lookup"><span data-stu-id="4188a-349">When you run a report for a site, if you enter text in the *Filter by URL* box and click *Search*, nothing happens.</span></span> <span data-ttu-id="4188a-350">Il s’agit, car ce contrôle ne fonctionne pas lors de la *Group By* état du rapport est défini sur *Type de problème*, qui est la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="4188a-350">This is because this control is not functional while the *Group By* state of the report is set to *Issue Type*, which is the default.</span></span>
> 
> <span data-ttu-id="4188a-351">**Solution de contournement** dans le *Group By* onglet du ruban, cliquez sur *URL* pour regrouper les entrées par leur URL source.</span><span class="sxs-lookup"><span data-stu-id="4188a-351">**Workaround** In the *Group By* tab of ribbon, click *URL* to group the entries by their source URL.</span></span> <span data-ttu-id="4188a-352">La zone de texte et un bouton pour filtrer les entrées sont fonctionnels dans cet état.</span><span class="sxs-lookup"><span data-stu-id="4188a-352">The text box and button to filter the entries are functional while in this state.</span></span>


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a><span data-ttu-id="4188a-353">Problème : WCF applications ne fonctionnent pas avec IIS Express</span><span class="sxs-lookup"><span data-stu-id="4188a-353">Issue: WCF applications fail to run with IIS Express</span></span>

> <span data-ttu-id="4188a-354">Accédant à une application WCF génère une erreur semblable à celui-ci :</span><span class="sxs-lookup"><span data-stu-id="4188a-354">Browsing to a WCF application results in an error like the following one:</span></span>
> 
> <span data-ttu-id="4188a-355">*Impossible de charger le fichier ou l’assembly ' Microsoft.Web.Administration, Version = 7.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' ou une de ses dépendances. Le système ne parvient pas à localiser le fichier spécifié.*</span><span class="sxs-lookup"><span data-stu-id="4188a-355">*Could not load file or assembly 'Microsoft.Web.Administration, Version=7.0.0.0, Culture=neutral,PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The system cannot find the file specified.*</span></span>
> 
> <span data-ttu-id="4188a-356">Cela se produit, car la version IIS Express Bêta ne prend pas en charge WCF par défaut.</span><span class="sxs-lookup"><span data-stu-id="4188a-356">This occurs because IIS Express Beta release doesn't support WCF by default.</span></span>
> 
> <span data-ttu-id="4188a-357">**Solution de contournement** utiliser l’une des solutions de contournement suivantes (solution de contournement #2 requiert Microsoft Windows Vista ou version ultérieure) :</span><span class="sxs-lookup"><span data-stu-id="4188a-357">**Workaround** Use any one of the following workarounds (workaround #2 requires Microsoft Windows Vista or higher):</span></span>
> 
> 
> 1. <span data-ttu-id="4188a-358">Copie le *Microsoft.Web.dll* et *Microsoft.Web.Administration.dll* assemblys à partir de l’emplacement d’installation de WebMatrix vers le *bin* répertoire de WCF application.</span><span class="sxs-lookup"><span data-stu-id="4188a-358">Copy the *Microsoft.Web.dll* and *Microsoft.Web.Administration.dll* assemblies from the WebMatrix installation location to the *bin* directory of the WCF application.</span></span> <span data-ttu-id="4188a-359">Par défaut, WebMatrix est installé dans le *Microsoft WebMatrix* sous-dossier sous le système *Program Files* dossier.</span><span class="sxs-lookup"><span data-stu-id="4188a-359">By default, WebMatrix is installed in the *Microsoft WebMatrix* subfolder under the system's *Program Files* folder.</span></span>
> 2. <span data-ttu-id="4188a-360">Sur Microsoft Windows Vista ou version ultérieure, créez un lien symbolique vers les assemblys dans le *bin* répertoire en utilisant les commandes suivantes.</span><span class="sxs-lookup"><span data-stu-id="4188a-360">On Microsoft Windows Vista or higher, create a symlink to the assemblies in the *bin* directory using the following commands.</span></span> <span data-ttu-id="4188a-361">(Cette approche présente l’avantage qu’il ne crée pas une copie des assemblys.)</span><span class="sxs-lookup"><span data-stu-id="4188a-361">(This approach has the advantage that it does not create a copy of the assemblies.)</span></span>
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. <span data-ttu-id="4188a-362">Installer les deux assemblys dans le GAC.</span><span class="sxs-lookup"><span data-stu-id="4188a-362">Install the two assemblies in the GAC.</span></span> <span data-ttu-id="4188a-363">À partir d’une invite de commandes avec élévation de privilèges, exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="4188a-363">From an elevated prompt, run the following commands:</span></span>
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="4188a-364">Problème : La version bêta 3 de WebMatrix est impossible d’effectuer certaines tâches qui requièrent une élévation</span><span class="sxs-lookup"><span data-stu-id="4188a-364">Issue: WebMatrix Beta 3 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="4188a-365">Version bêta 3 de WebMatrix est impossible d’effectuer certaines tâches qui requièrent une élévation, telles que l’installation des composants supplémentaires dans les situations suivantes :</span><span class="sxs-lookup"><span data-stu-id="4188a-365">WebMatrix Beta 3 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="4188a-366">Sur Windows Vista ou Windows 7, vous êtes connecté avec un compte qui ne dispose pas des privilèges d’administrateur et contrôle de compte utilisateur (UAC) est désactivé.</span><span class="sxs-lookup"><span data-stu-id="4188a-366">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="4188a-367">Vous utilisez Microsoft Windows XP ou Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="4188a-367">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="4188a-368">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="4188a-368">**Workaround**</span></span>  
> <span data-ttu-id="4188a-369">La plupart des tâches dans la version bêta 3 de WebMatrix ne nécessitent pas d’autorisation d’administrateur.</span><span class="sxs-lookup"><span data-stu-id="4188a-369">Most tasks in WebMatrix Beta 3 do not require administrative permission.</span></span> <span data-ttu-id="4188a-370">Pour les autres, vous pouvez effectuer l’opération en tant qu’administrateur, ou procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="4188a-370">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="4188a-371">Sur Windows Vista ou Windows 7, activer l’UAC.</span><span class="sxs-lookup"><span data-stu-id="4188a-371">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="4188a-372">Sur Windows XP, ajoutez l’utilisateur au groupe de sécurité Administrateurs.</span><span class="sxs-lookup"><span data-stu-id="4188a-372">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="4188a-373">Problème : « Site depuis la galerie Web » est désactivée.</span><span class="sxs-lookup"><span data-stu-id="4188a-373">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="4188a-374">Le **Site à partir de la galerie Web** option est désactivée si le serveur Web Platform Installer 3.0 n’est pas installé.</span><span class="sxs-lookup"><span data-stu-id="4188a-374">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="4188a-375">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="4188a-375">**Workaround**</span></span>  
> <span data-ttu-id="4188a-376">Installer le [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="4188a-376">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a><span data-ttu-id="4188a-377">Problème : Sur Windows Server 2003, IIS Express ne démarre pas pour un utilisateur non-administrateur</span><span class="sxs-lookup"><span data-stu-id="4188a-377">Issue: On Windows Server 2003, IIS Express does not start for a non-administrative user</span></span>

> <span data-ttu-id="4188a-378">Sur Windows Server 2003, lance une page ou au démarrage d’IIS Express, IIS Express ne démarre pas.</span><span class="sxs-lookup"><span data-stu-id="4188a-378">On Windows Server 2003, when you launch a page or start IIS Express, IIS Express does not start.</span></span> <span data-ttu-id="4188a-379">Pour les pages Web, une erreur s’affiche qui indique que l’application a été démarrée par un utilisateur non-administrateur.</span><span class="sxs-lookup"><span data-stu-id="4188a-379">For Web pages, an error is displayed that indicates that the application has been started by a non-administrative user.</span></span>
> 
> <span data-ttu-id="4188a-380">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="4188a-380">**Workaround**</span></span>  
> <span data-ttu-id="4188a-381">Démarrer la version bêta 3 de WebMatrix en tant qu’administrateur.</span><span class="sxs-lookup"><span data-stu-id="4188a-381">Start WebMatrix Beta 3 as an administrative user.</span></span> <span data-ttu-id="4188a-382">Pour plus d’informations, consultez l’article suivant de la base de connaissances :</span><span class="sxs-lookup"><span data-stu-id="4188a-382">For more details, see the following KnowledgeBase article:</span></span>  
>   
> [<span data-ttu-id="4188a-383">Une application est démarrée par un utilisateur non-administrateur ne peut pas écouter le trafic HTTP de l’ordinateur sur lequel l’application s’exécute dans Windows Vista, Windows Server 2003 ou Windows XP.</span><span class="sxs-lookup"><span data-stu-id="4188a-383">An application that is started by a non-administrative user cannot listen to the HTTP traffic of the computer on which the application is running in Windows Vista, Windows Server 2003, or Windows XP.</span></span>](https://support.microsoft.com/kb/939786)


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="4188a-384">Problème : Google Chrome n’est pas disponible en tant qu’une option d’exécution</span><span class="sxs-lookup"><span data-stu-id="4188a-384">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="4188a-385">Google Chrome n’est pas affiché dans la liste des navigateurs sous **exécuter** sur le **accueil** onglet.</span><span class="sxs-lookup"><span data-stu-id="4188a-385">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="4188a-386">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="4188a-386">**Workaround**</span></span>  
> <span data-ttu-id="4188a-387">Certaines versions de Google Chrome n’inscrivez pas eux-mêmes correctement avec la fonctionnalité programmes par défaut dans Windows.</span><span class="sxs-lookup"><span data-stu-id="4188a-387">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="4188a-388">Pour résoudre ce problème, démarrez Google Chrome, cliquez sur le *personnaliser et contrôle Google Chrome* menu, cliquez sur *Options*, puis cliquez sur *Make Google Chrome mon navigateur par défaut*.</span><span class="sxs-lookup"><span data-stu-id="4188a-388">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="4188a-389">Problème : La boîte de dialogue « Foreign Key » n’autorise pas entrer une clé primaire</span><span class="sxs-lookup"><span data-stu-id="4188a-389">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="4188a-390">Le **clé étrangère** boîte de dialogue ne vous permet pas à entrer le nom de clé primaire à partir de la table de clé primaire.</span><span class="sxs-lookup"><span data-stu-id="4188a-390">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="4188a-391">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="4188a-391">**Workaround**</span></span>  
> <span data-ttu-id="4188a-392">Ceci est intentionnel.</span><span class="sxs-lookup"><span data-stu-id="4188a-392">This is intentional.</span></span> <span data-ttu-id="4188a-393">Vous n’avez pas besoin d’entrer le nom de la clé primaire à partir de la table de clé primaire.</span><span class="sxs-lookup"><span data-stu-id="4188a-393">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-the-relationships-button-is-disabled"></a><span data-ttu-id="4188a-394">Problème : Le bouton « Relations » est désactivé.</span><span class="sxs-lookup"><span data-stu-id="4188a-394">Issue: The "Relationships" button is disabled</span></span>

> <span data-ttu-id="4188a-395">Le **relations** bouton sous le **Table** onglet dans le **bases de données** espace de travail est désactivé pour les bases de données SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="4188a-395">The **Relationships** button under the **Table** tab in the **Databases** workspace is disabled for SQL Server Compact databases.</span></span>
> 
> <span data-ttu-id="4188a-396">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="4188a-396">**Workaround**</span></span>  
> <span data-ttu-id="4188a-397">Aucun.</span><span class="sxs-lookup"><span data-stu-id="4188a-397">None.</span></span> <span data-ttu-id="4188a-398">SQL Server Compact ne prend pas en charge les relations entre les tables.</span><span class="sxs-lookup"><span data-stu-id="4188a-398">SQL Server Compact does not support relationships between tables.</span></span>


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a><span data-ttu-id="4188a-399">Problème : Les requêtes SQL paramétrables lever des exceptions</span><span class="sxs-lookup"><span data-stu-id="4188a-399">Issue: Parameterized SQL queries throw exceptions</span></span>

> <span data-ttu-id="4188a-400">Dans SQL Server Compact 4.0, si vous ne spécifiez pas un type de données tel que `SqlDbType` ou `DbType` pour les paramètres dans les requêtes paramétrables, une exception est levée lorsque la requête s’exécute.</span><span class="sxs-lookup"><span data-stu-id="4188a-400">In SQL Server Compact 4.0, if you do not specify a data type such as `SqlDbType` or `DbType` for parameters in parameterized queries, an exception is thrown when the query runs.</span></span>
> 
> <span data-ttu-id="4188a-401">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="4188a-401">**Workaround**</span></span>  
> <span data-ttu-id="4188a-402">Définir explicitement le type de données pour les paramètres tels que `SqlDbType` ou `DbType`.</span><span class="sxs-lookup"><span data-stu-id="4188a-402">Explicitly set the data type for parameters such as `SqlDbType` or `DbType`.</span></span> <span data-ttu-id="4188a-403">Cela est essentiel dans le cas des types de données BLOB (`image` et `ntext`).</span><span class="sxs-lookup"><span data-stu-id="4188a-403">This is critical in the case of BLOB data types (`image` and `ntext`).</span></span> <span data-ttu-id="4188a-404">Utilisez un code semblable au suivant :</span><span class="sxs-lookup"><span data-stu-id="4188a-404">Use code like the following:</span></span>
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="4188a-405">Pour plus d'informations</span><span class="sxs-lookup"><span data-stu-id="4188a-405">For More Information</span></span>

<span data-ttu-id="4188a-406">Pour plus d’informations sur la version bêta 3 de WebMatrix, consultez les sites Web suivants :</span><span class="sxs-lookup"><span data-stu-id="4188a-406">For more information about WebMatrix Beta 3, see the following websites:</span></span>

- [<span data-ttu-id="4188a-407">IIS.net</span><span class="sxs-lookup"><span data-stu-id="4188a-407">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="4188a-408">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4188a-408">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="4188a-409">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="4188a-409">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

* * *

<span data-ttu-id="4188a-410">© 2010 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="4188a-410">© 2010 Microsoft Corporation.</span></span> <span data-ttu-id="4188a-411">Tous droits réservés.</span><span class="sxs-lookup"><span data-stu-id="4188a-411">All Rights Reserved.</span></span> <span data-ttu-id="4188a-412">[Conditions d’utilisation](https://msdn.microsoft.cos/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="4188a-412">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
