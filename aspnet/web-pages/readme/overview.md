---
uid: web-pages/readme/overview
title: Fichier Lisez-moi de WebMatrix | Microsoft Docs
author: rick-anderson
description: WebMatrix et ASP.NET Web Pages (Razor) version 1.0 Lisez-moi
ms.author: aspnetcontent
ms.date: 01/06/2011
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: 8aa0eedb3156ca33905c106b72d5fa553a917b16
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822860"
---
<a name="webmatrix-readme"></a><span data-ttu-id="6fe2e-103">Fichier Lisez-moi de WebMatrix</span><span class="sxs-lookup"><span data-stu-id="6fe2e-103">WebMatrix Readme</span></span>
====================
<span data-ttu-id="6fe2e-104">13 janvier 2011</span><span class="sxs-lookup"><span data-stu-id="6fe2e-104">13 January 2011</span></span>

## <a name="contents"></a><span data-ttu-id="6fe2e-105">Sommaire</span><span class="sxs-lookup"><span data-stu-id="6fe2e-105">Contents</span></span>

> [!NOTE]
> <span data-ttu-id="6fe2e-106">Ce fichier Lisez-moi s’applique à la 1.0 version de WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-106">This readme applies to the 1.0 release of WebMatrix.</span></span>


- [<span data-ttu-id="6fe2e-107">Vue d’ensemble</span><span class="sxs-lookup"><span data-stu-id="6fe2e-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="6fe2e-108">Installation</span><span class="sxs-lookup"><span data-stu-id="6fe2e-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="6fe2e-109">Comment publier des Applications</span><span class="sxs-lookup"><span data-stu-id="6fe2e-109">How to Publish Applications</span></span>](#InstructionsForPublishingApplications)
- [<span data-ttu-id="6fe2e-110">Modifications et les problèmes</span><span class="sxs-lookup"><span data-stu-id="6fe2e-110">Changes and Issues</span></span>](#ChangesAndIssues)

    - [<span data-ttu-id="6fe2e-111">Installation de WebMatrix 1.0</span><span class="sxs-lookup"><span data-stu-id="6fe2e-111">WebMatrix 1.0 Installation</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="6fe2e-112">Pages Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6fe2e-112">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="6fe2e-113">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="6fe2e-113">WebMatrix</span></span>](#Known_Issues_WebMatrix)
    - [<span data-ttu-id="6fe2e-114">IIS Express</span><span class="sxs-lookup"><span data-stu-id="6fe2e-114">IIS Express</span></span>](#Known_Issues_IISExpress)
    - [<span data-ttu-id="6fe2e-115">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="6fe2e-115">SQL Server Compact</span></span>](#Known_Issues_SQLServerCompact)
    - [<span data-ttu-id="6fe2e-116">Installation d’Applications</span><span class="sxs-lookup"><span data-stu-id="6fe2e-116">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="6fe2e-117">Publication d’Applications</span><span class="sxs-lookup"><span data-stu-id="6fe2e-117">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
- [<span data-ttu-id="6fe2e-118">Pour plus d’informations</span><span class="sxs-lookup"><span data-stu-id="6fe2e-118">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="6fe2e-119">Vue d'ensemble</span><span class="sxs-lookup"><span data-stu-id="6fe2e-119">Overview</span></span>

> <span data-ttu-id="6fe2e-120">Microsoft WebMatrix 1.0 est une pile de développement web gratuit qui s’installe en quelques minutes.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-120">Microsoft WebMatrix 1.0 is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="6fe2e-121">Il s’intègre un serveur web de base de données et infrastructures pour créer une expérience unique et intégrée de programmation.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-121">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="6fe2e-122">Vous pouvez utiliser WebMatrix pour simplifier la manière dont votre code, tester, publier votre propre site Web ASP.NET ou PHP ou vous pouvez utiliser WebMatrix pour démarrer un nouveau site Web à l’aide d’applications open source populaires telles que DotNetNuke, Umbraco, WordPress ou Joomla.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-122">You can use WebMatrix to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="6fe2e-123">WebMatrix utilise le même serveur web puissant, moteur de base de données et infrastructures que ceux qui exécutera votre site Web sur internet, ce qui rend la transition du développement en production fluide et homogène.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-123">WebMatrix uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="6fe2e-124">Installation</span><span class="sxs-lookup"><span data-stu-id="6fe2e-124">Installation</span></span>

> <span data-ttu-id="6fe2e-125">Pour installer WebMatrix 1.0, vous devez d’abord installer le [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="6fe2e-125">To install WebMatrix 1.0, you must first install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="6fe2e-126">Après avoir installé Web Platform Installer, vous pouvez l’utiliser pour installer WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-126">After you've installed the Web Platform Installer, you can use it to install WebMatrix.</span></span>
> 
> <span data-ttu-id="6fe2e-127">Si vous rencontrez des problèmes lors de l’installation, reportez-vous à [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="6fe2e-127">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a><span data-ttu-id="6fe2e-128">Comment publier des Applications</span><span class="sxs-lookup"><span data-stu-id="6fe2e-128">How to Publish Applications</span></span>

> <span data-ttu-id="6fe2e-129">Consultez [obtenir des Instructions détaillées pour la publication d’Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="6fe2e-129">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a><span data-ttu-id="6fe2e-130">Modifications et les problèmes</span><span class="sxs-lookup"><span data-stu-id="6fe2e-130">Changes and Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a><span data-ttu-id="6fe2e-131">Problèmes d’Installation 1.0 de WebMatrix</span><span class="sxs-lookup"><span data-stu-id="6fe2e-131">WebMatrix 1.0 Installation Issues</span></span>

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="6fe2e-132">Problème : WebMatrix 1.0 est disponible uniquement sur les plateformes qui prennent en charge de Microsoft .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="6fe2e-132">Issue: WebMatrix 1.0 is available only on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="6fe2e-133">Le .NET Framework version 4 est requis pour WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-133">The .NET Framework version 4 is required for WebMatrix.</span></span> <span data-ttu-id="6fe2e-134">Dans certains cas, le programme d’installation de WebMatrix 1.0 permet d’essayer d’installer sur une plateforme qui ne fait pas partie de l’ensemble de la configuration prise en charge.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-134">In certain cases, the WebMatrix 1.0 installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="6fe2e-135">En particulier, Vista Windows sans la mise à jour SP1 vous permet de commencer l’installation de WebMatrix, mais le composant .NET Framework 4 échoue et bloquer votre installation.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-135">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="6fe2e-136">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-136">**Workaround**</span></span>  
> <span data-ttu-id="6fe2e-137">Installer sur une plateforme prise en charge, ce qui inclut :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-137">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="6fe2e-138">Windows 7</span><span class="sxs-lookup"><span data-stu-id="6fe2e-138">Windows 7</span></span>
> - <span data-ttu-id="6fe2e-139">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="6fe2e-139">Windows Server 2008</span></span>
> - <span data-ttu-id="6fe2e-140">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="6fe2e-140">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="6fe2e-141">Windows Vista SP1 ou ultérieur</span><span class="sxs-lookup"><span data-stu-id="6fe2e-141">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="6fe2e-142">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="6fe2e-142">Windows XP SP3</span></span>
> - <span data-ttu-id="6fe2e-143">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="6fe2e-143">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="6fe2e-144">Problème : Ne peut pas installer WebMatrix 1.0 si Microsoft Visual Studio 2008 est installé sans Microsoft Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="6fe2e-144">Issue: Cannot install WebMatrix 1.0 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="6fe2e-145">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-145">**Workaround**</span></span>  
> <span data-ttu-id="6fe2e-146">Installer [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) à partir du centre de téléchargement Microsoft.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-146">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="6fe2e-147">Problème : Certains assemblys pour SQL Server Compact 4.0 ne sont pas installés dans le GAC</span><span class="sxs-lookup"><span data-stu-id="6fe2e-147">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="6fe2e-148">Les assemblys managés pour SQL Server Compact 4.0 ne sont pas placés dans le global assembly cache (GAC) lorsque vous installez SQL Server Compact 4.0 sur un ordinateur 64 bits et de l’ordinateur a uniquement le .NET Framework 3.5 SP1 Client Profile installé.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-148">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="6fe2e-149">Les assemblys managés qui ne sont pas installés dans le GAC sont :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-149">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="6fe2e-150">*System.Data.SqlServerCe.dll* (fournisseur ADO.NET)</span><span class="sxs-lookup"><span data-stu-id="6fe2e-150">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="6fe2e-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)</span><span class="sxs-lookup"><span data-stu-id="6fe2e-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="6fe2e-152">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-152">**Workaround**</span></span>  
> <span data-ttu-id="6fe2e-153">Désinstaller SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-153">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="6fe2e-154">Téléchargez et installez la version complète de .NET Framework 3.5 SP1 à partir de l’emplacement suivant :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-154">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="6fe2e-155">Microsoft .NET Framework 3.5 Service pack 1 (indépendant)</span><span class="sxs-lookup"><span data-stu-id="6fe2e-155">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="6fe2e-156">Puis réinstallez SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-156">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="6fe2e-157">Problème : Impossible de désinstaller SQL Server Compact à l’aide de la ligne de commande</span><span class="sxs-lookup"><span data-stu-id="6fe2e-157">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="6fe2e-158">La désinstallation de SQL Server Compact à l’aide des options de ligne de commande ne fonctionne pas dans cette version.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-158">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="6fe2e-159">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-159">**Workaround**</span></span>  
> <span data-ttu-id="6fe2e-160">Utilisez *programmes et fonctionnalités* dans le panneau de configuration Windows pour désinstaller Microsoft SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-160">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="6fe2e-161">Pages web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6fe2e-161">ASP.NET Web Pages</span></span>

<span data-ttu-id="6fe2e-162">Cette section du document décrit les nouvelles fonctionnalités, des modifications et des problèmes connus avec la 1.0 version d’ASP.NET Web Pages avec syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-162">This section of the document describes new features, changes, and known issues with the 1.0 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="6fe2e-163">Nouvelles fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="6fe2e-163">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="6fe2e-164">Modifications</span><span class="sxs-lookup"><span data-stu-id="6fe2e-164">Changes</span></span>](#Changes)
- [<span data-ttu-id="6fe2e-165">Problèmes</span><span class="sxs-lookup"><span data-stu-id="6fe2e-165">Issues</span></span>](#Issues)

#### <a id="NewFeatures"></a>  <span data-ttu-id="6fe2e-166">Nouvelles fonctionnalités</span><span class="sxs-lookup"><span data-stu-id="6fe2e-166">New Features</span></span>

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a><span data-ttu-id="6fe2e-167">Nouveauté : Paramètre de Configuration est ajoutée pour désactiver le Gestionnaire de package</span><span class="sxs-lookup"><span data-stu-id="6fe2e-167">New: Configuration setting added to disable the package manager</span></span>

> <span data-ttu-id="6fe2e-168">Un nouveau `asp:AdminManagerEnabled` clé est disponible pour le `<appSettings>` élément dans le *web.config* fichier, ce qui vous permet de désactiver complètement le Gestionnaire de package.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-168">A new `asp:AdminManagerEnabled` key is available for the `<appSettings>` element in the *web.config* file, which lets you completely disable the package manager.</span></span> <span data-ttu-id="6fe2e-169">La valeur par défaut pour cet élément est la valeur est true, ce qui signifie que s’il n’est pas inclus dans le *web.config* fichier, le Gestionnaire de package est activé.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-169">The default value for this element is true, meaning that if it is not included in the *web.config* file, the package manager is enabled.</span></span> <span data-ttu-id="6fe2e-170">Pour désactiver le Gestionnaire de package, ajoutez l’élément suivant à la *web.config* fichier à la racine du site Web :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-170">To disable the package manager, add the following element to the *web.config* file in the root of the website:</span></span>
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  <span data-ttu-id="6fe2e-171">Modifications</span><span class="sxs-lookup"><span data-stu-id="6fe2e-171">Changes</span></span>

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a><span data-ttu-id="6fe2e-172">Modifier : la clé de « webPages:AdminFolderVirtualPath » renommée en « asp : AdminFolderVirtualPath »</span><span class="sxs-lookup"><span data-stu-id="6fe2e-172">Change: "webPages:AdminFolderVirtualPath" key renamed to "asp:AdminFolderVirtualPath"</span></span>

> <span data-ttu-id="6fe2e-173">Le `webPages:AdminFolderVirtualPath` clé qui peut être ajouté à la *web.config* fichier pour spécifier l’emplacement du Gestionnaire de package a été renommé pour utiliser le `asp:` espace de noms au lieu du `webPages` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-173">The `webPages:AdminFolderVirtualPath` key that can be added to the *web.config* file to specify the location of the package manager has been renamed to use the `asp:` namespace instead of the `webPages` namespace.</span></span> <span data-ttu-id="6fe2e-174">Si vous avez utilisé cet élément, vous devez le renommer dans le fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-174">If you have used this element, you must rename it in the configuration file.</span></span>


#### <a id="Issues"></a>  <span data-ttu-id="6fe2e-175">Problèmes connus</span><span class="sxs-lookup"><span data-stu-id="6fe2e-175">Known Issues</span></span>

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a><span data-ttu-id="6fe2e-176">Problème : Les mots de passe pour les utilisateurs d’appartenance n’est plus reconnus</span><span class="sxs-lookup"><span data-stu-id="6fe2e-176">Issue: Passwords for membership users no longer recognized</span></span>

> <span data-ttu-id="6fe2e-177">L’algorithme pour la création et le stockage des mots de passe de l’appartenance (connexion) a été modifié pour être plus sûr.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-177">The algorithm for creating and storing membership (login) passwords has been changed to be more secure.</span></span> <span data-ttu-id="6fe2e-178">Par conséquent, les mots de passe stockés pour les membres (utilisateurs) créés dans les versions bêta d’ASP.NET Razor ne seront pas reconnues.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-178">As a result, the passwords stored for members (users) created in Beta versions of ASP.NET Razor will not be recognized.</span></span> 
> 
> <span data-ttu-id="6fe2e-179">**Solution de contournement** si le site n’a pas encore été mis en production, supprimer les enregistrements d’utilisateur à partir de la base de données d’appartenance.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-179">**Workaround** If the site has not yet been put into production, remove the user records from the membership database.</span></span> <span data-ttu-id="6fe2e-180">Si la base de données est en ligne, par programmation régénérer des mots de passe existants dans la base de données d’appartenance.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-180">If database is live, programmatically regenerate existing passwords in the membership database.</span></span>


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="6fe2e-181">Problème : Un comportement inattendu lors de l’utilisation d’une table utilisateur personnalisée pour l’appartenance</span><span class="sxs-lookup"><span data-stu-id="6fe2e-181">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="6fe2e-182">Pour initialiser le fournisseur d’appartenances pour un site Web ASP.NET Razor, vous appelez le `WebSecurity.InitializeDatabaseConnection` (méthode).</span><span class="sxs-lookup"><span data-stu-id="6fe2e-182">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="6fe2e-183">(Dans WebMatrix, du modèle Starter Site inclut un appel à cette méthode dans le  *\_AppStart.cshtml* fichier.) Si le `autoCreateTables` paramètre de cette méthode est définie sur true (par défaut, il est défini sur true dans le modèle Starter Site), et si un nom de table non reconnu est passé à la méthode (le deuxième paramètre), la méthode ne lève pas une erreur.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-183">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="6fe2e-184">Au lieu de cela, il crée automatiquement la table.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-184">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="6fe2e-185">Cela peut poser un problème si vous avez l’intention d’utiliser une table utilisateur personnalisée pour l’appartenance, mais passez le nom de table incorrect à la `WebSecurity.InitializeDatabaseConnection` (méthode).</span><span class="sxs-lookup"><span data-stu-id="6fe2e-185">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="6fe2e-186">Étant donné que la méthode ne pas par défaut déclenche une erreur si la table spécifiée n’existe pas, et parce qu’il crée à la place une nouvelle table, l’application peut apparaître à travailler.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-186">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="6fe2e-187">Toutefois, code d’application qui s’appuie sur la table utilisateur personnalisée (et sur les champs qu’il contient) peut finit par échouer avec des erreurs inattendues.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-187">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="6fe2e-188">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-188">**Workaround**</span></span>  
> <span data-ttu-id="6fe2e-189">Assurez-vous que le nom transmis dans le `InitializeDatabaseConnection` correspondances méthode le profil utilisateur figurant dans la base de données d’appartenance, ou vous assurer que le `autoCreateTables` paramètre est défini sur false.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-189">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a><span data-ttu-id="6fe2e-190">Problème : Message d’erreur « le Module d’administration requiert l’accès à ~/App\_données »</span><span class="sxs-lookup"><span data-stu-id="6fe2e-190">Issue: Error message "The Admin Module requires access to ~/App\_Data"</span></span>

> <span data-ttu-id="6fe2e-191">Dans certaines circonstances, essayez de créer des utilisateurs ou de travailler avec le système d’appartenance ASP.NET peut provoquer la page pour afficher l’erreur *le Module d’administration requiert l’accès à ~/App\_données*.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-191">Under some circumstances, trying to create users or otherwise work with the ASP.NET membership system can cause the page to display the error *The Admin Module requires access to ~/App\_Data*.</span></span> <span data-ttu-id="6fe2e-192">Cela se produit si le compte IIS ou IIS Express sous lequel s’exécute ne dispose pas des autorisations pour créer et écrire dans le *application\_données* dossier sous la racine du site Web.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-192">This occurs if the account that IIS or IIS Express is running under does not have permissions to create and write to the *App\_Data* folder under the website root.</span></span> 
> 
> <span data-ttu-id="6fe2e-193">**Solution de contournement** créer manuellement un *application\_données* dossier pour le site Web.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-193">**Workaround** Manually create an *App\_Data* folder for the website.</span></span> <span data-ttu-id="6fe2e-194">Assurez-vous que le compte Windows de l’application sous lequel s’exécute (généralement NETWORK SERVICE) dispose des autorisations en lecture/écriture pour les dossiers racine de l’application et pour les sous-dossiers, par exemple application\_données.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-194">Then make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as App\_Data.</span></span> <span data-ttu-id="6fe2e-195">Des informations plus détaillées sont disponibles dans l’article de la base de connaissances [des problèmes avec les projets d’Application Web ASP.net et d’utilisateur SQL Server Express l'instanciation](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="6fe2e-195">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="6fe2e-196">Problème : erreur « Échec de générer une instance utilisateur de SQL Server »</span><span class="sxs-lookup"><span data-stu-id="6fe2e-196">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="6fe2e-197">Si une application Web de WebMatrix utilise SQL Server Express et est en cours d’exécution IIS 7.5 sur Windows 7 ou Windows Server 2008 R2, vous pouvez voir une erreur qui indique que SQL Server ne peut pas récupérer le chemin d’accès de l’utilisateur local d’application en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-197">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="6fe2e-198">**Solution de contournement** vous assurer que le compte Windows de l’application sous lequel s’exécute (généralement NETWORK SERVICE) dispose des autorisations de lecture/écriture pour les dossiers racine de l’application et des sous-dossiers comme *application\_données*.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-198">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="6fe2e-199">Des informations plus détaillées sont disponibles dans l’article de la base de connaissances [des problèmes avec les projets d’Application Web ASP.net et d’utilisateur SQL Server Express l'instanciation](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="6fe2e-199">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a><span data-ttu-id="6fe2e-200">Problème : Les fichiers qui contient les ressources du Gestionnaire de package ou les mots de passe de gestionnaire de package sont étant délivrables sous IIS 6.0 et versions antérieures</span><span class="sxs-lookup"><span data-stu-id="6fe2e-200">Issue: Files that contains package-manager resources or package-manager passwords are servable under IIS 6.0 and earlier</span></span>

> <span data-ttu-id="6fe2e-201">Si vous déployez une application ASP.NET Web Pages (Razor) qui a été créée à l’aide de la version RC2, et si l’application contient un *password.txt* ou *packagesources.txt* de fichiers sous */App\_ Données/admin*, IIS 6.0 renverra au fichier si nécessaire, les exposant potentiellement les mots de passe pour votre instance de gestionnaire de package.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-201">If you deploy an ASP.NET Web Pages (Razor) application that was built using the RC2 release, and if the application contains a *password.txt* or *packagesources.txt* file under */App\_Data/admin*, IIS 6.0 will serve the file if requested, potentially exposing the passwords for your package manager instance.</span></span> 
> 
> <span data-ttu-id="6fe2e-202">**Solution de contournement** renommer le *password.txt* ou *packagesources.txt* fichier *password.config* ou *packagesources.config*. Par défaut, IIS 6.0 ne traite pas les fichiers qui ont le *.config* extension.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-202">**Workaround** Rename the *password.txt* or *packagesources.txt* file to *password.config* or *packagesources.config*. By default, IIS 6.0 does not serve files that have the *.config* extension.</span></span> <span data-ttu-id="6fe2e-203">(Dans IIS 7, aucun fichier dans le *application\_données* dossier sont pris en charge, vous n’avez pas besoin de renommer les fichiers.)</span><span class="sxs-lookup"><span data-stu-id="6fe2e-203">(In IIS 7, no files in the *App\_Data* folder are served, so you do not need to rename the files.)</span></span>


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a><span data-ttu-id="6fe2e-204">Problème : Les packages installés à l’aide de la version bêta 3 de désinstallation ne supprime pas complètement les composants de package</span><span class="sxs-lookup"><span data-stu-id="6fe2e-204">Issue: Uninstalling packages installed using the Beta 3 release does not completely remove package components</span></span>

> <span data-ttu-id="6fe2e-205">Si vous installé un package à l’aide du Gestionnaire de package dans la version bêta 3 et réessayez de désinstaller à l’aide de la version actuelle, le package n’est pas complètement désinstallé.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-205">If you installed a package using the package manager in the Beta 3 release and then try to uninstall it using the current release, the package is not completely uninstalled.</span></span> <span data-ttu-id="6fe2e-206">À l’aide du Gestionnaire de package **désinstallation** bouton supprime certains composants, mais laisse le code de la bibliothèque du package et ne pas mettre à jour le *package.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-206">Using the package manager's **Uninstall** button removes some components, but leaves the package's library code and does not update the *package.config* file.</span></span>
> 
> <span data-ttu-id="6fe2e-207">**Solution de contournement** </span><span class="sxs-lookup"><span data-stu-id="6fe2e-207">**Workaround** </span></span>  
> <span data-ttu-id="6fe2e-208">Procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-208">Perform these steps:</span></span>  
> 1. <span data-ttu-id="6fe2e-209">Supprimer le *application\_Data\packages* dossier.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-209">Delete the *App\_Data\packages* folder.</span></span> <span data-ttu-id="6fe2e-210">Cette opération supprime tous les packages.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-210">This removes all packages.</span></span>   
> 2. <span data-ttu-id="6fe2e-211">Supprimer le *packages.config* fichier à la racine du site Web.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-211">Delete the *packages.config* file in the root of the website.</span></span>


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a><span data-ttu-id="6fe2e-212">Problème : Dans Visual Studio, appeler le Gestionnaire de package basé sur le web met l’application hors connexion</span><span class="sxs-lookup"><span data-stu-id="6fe2e-212">Issue: In Visual Studio, invoking the web-based package manager takes the application offline</span></span>

> <span data-ttu-id="6fe2e-213">Si vous travaillez dans Visual Studio (pas WebMatrix) et que vous utilisez le  *\_administrateur* fonctionnalité pour démarrer le Gestionnaire de package, Visual Studio met l’application hors connexion et publie le *application\_ Offline.htm* dans la racine du site Web, ce qui interrompt votre capacité à utiliser le Gestionnaire de package.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-213">If you are working in Visual Studio (not WebMatrix) and use the *\_admin* functionality to start the package manager, Visual Studio takes the application offline and posts the *app\_offline.htm* into the website root, which disrupts your ability to use the package manager.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="6fe2e-214">Bien que vous verriez généralement ce comportement lors de l’utilisation de l’interface de gestionnaire de package basé sur le web, le même comportement se produit si vous ajoutez, supprimez ou modifiez des fichiers dans le *application\_données* dossier.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-214">Although you would most typically see this behavior when using the web-based package manager interface, the same behavior occurs if you add, remove, or modify any files in the *App\_Data* folder.</span></span>
> 
> <span data-ttu-id="6fe2e-215">**Solution de contournement** </span><span class="sxs-lookup"><span data-stu-id="6fe2e-215">**Workaround** </span></span>  
> <span data-ttu-id="6fe2e-216">Pour utiliser des packages dans Visual Studio, utilisez l’extension NuGet au lieu du Gestionnaire de package basé sur le web.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-216">To work with packages in Visual Studio, use the NuGet extension instead of the web-based package manager.</span></span> <span data-ttu-id="6fe2e-217">Pour plus d’informations, consultez le [documentation de NuGet](https://docs.microsoft.com/nuget/).</span><span class="sxs-lookup"><span data-stu-id="6fe2e-217">For information, see the [NuGet documentation](https://docs.microsoft.com/nuget/).</span></span> <span data-ttu-id="6fe2e-218">Si vous travaillez avec d’autres fichiers dans le *application\_données* dossier, envisagez de conserver les fichiers ailleurs pour éviter ce problème.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-218">If you are working with other files in the *App\_Data* folder, consider keeping the files elsewhere to avoid this issue.</span></span> <span data-ttu-id="6fe2e-219">Si ce n’est pas pratique, supprimez le *application\_offline.htm* fichier manuellement ou patienter jusqu'à ce que le site revient en ligne automatiquement (par défaut, après 30 secondes).</span><span class="sxs-lookup"><span data-stu-id="6fe2e-219">If that's not practical, delete the *app\_offline.htm* file manually or wait until the site comes back online automatically (by default, after 30 seconds).</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="6fe2e-220">Problème : Visual Studio IntelliSense modèles de projet et disponibles uniquement dans ASP.NET MVC version 3</span><span class="sxs-lookup"><span data-stu-id="6fe2e-220">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="6fe2e-221">L’installation d’ASP.NET Web Pages ne pas également installe outils pour Visual Studio telles que des modèles de projet et IntelliSense pour les applications ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-221">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="6fe2e-222">**Solution de contournement** pour utiliser des modèles de projet et IntelliSense pour les applications de Pages Web ASP.NET dans Visual Studio, installer ASP.NET MVC 3 RC via Web Platform Installer ou le [programme d’installation autonome](https://go.microsoft.com/fwlink/?LinkID=191797).</span><span class="sxs-lookup"><span data-stu-id="6fe2e-222">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="6fe2e-223">Problème : Lecture des flux ou autres données externes via un serveur proxy</span><span class="sxs-lookup"><span data-stu-id="6fe2e-223">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="6fe2e-224">Si le serveur qui exécute le site est derrière un serveur proxy, vous devrez peut-être configurer les informations de proxy dans le *web.config* fichier afin d’être en mesure de lire les informations provenant de l’extérieur de votre site.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-224">If the server running the site is behind a proxy server, you might need to configure proxy information in the *web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="6fe2e-225">Par exemple, si vous utilisez le `ReCaptcha` helper, l’application d’assistance communique avec le service reCAPTCHA, mais peut être bloquée par votre serveur proxy.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-225">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="6fe2e-226">De même, les flux sont utilisés dans les Pages Web ASP.NET, par exemple, le flux utilisé par le Gestionnaire de package, peuvent nécessiter la configuration du proxy.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-226">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="6fe2e-227">Si vous rencontrez des problèmes dans avec un service externe ou fonctionnent correctement avec le flux du package, placez les éléments suivants dans la racine de votre application *web.config* fichier :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-227">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *web.config* file:</span></span>
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> <span data-ttu-id="6fe2e-228">Pour plus d’informations sur la configuration d’un serveur proxy, consultez [ &lt;proxy&gt; , élément (paramètres réseau)](https://msdn.microsoft.com/library/sa91de1e.aspx) sur le site Web MSDN.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-228">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="6fe2e-229">Problème : La désinstallation de la version 4 du .NET Framework désactive ASP.NET Web Pages avec syntaxe Razor</span><span class="sxs-lookup"><span data-stu-id="6fe2e-229">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="6fe2e-230">Si vous désinstallez le Kit de développement .NET Framework version 4, puis le réinstaller, ASP.NET Web Pages avec syntaxe Razor est désactivés.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-230">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="6fe2e-231">Pages avec la *.cshtml* extension ne s’exécutent pas correctement.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-231">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="6fe2e-232">ASP.NET Web Pages enregistre un assembly dans la racine de la machine *web.config* des fichiers et la suppression de .NET Framework supprime ce fichier.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-232">ASP.NET Web Pages registers an assembly in the machine root *web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="6fe2e-233">Réinstallez le .NET Framework installe une nouvelle version du fichier de configuration, mais n’ajoute pas la référence de l’assembly d’ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-233">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="6fe2e-234">**Solution de contournement** après la réinstallation de .NET Framework, réinstallez ASP.NET Web Pages avec syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-234">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="6fe2e-235">Cette opération ajoute l’élément suivant à la *web.config* fichier à la racine de l’ordinateur, qui est en général à l’emplacement suivant :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-235">This adds the following element to the *web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="6fe2e-236">Problème : Les URL sans extension ne trouvent pas les fichiers.cshtml/.vbhtml sur IIS 7 ou IIS 7.5</span><span class="sxs-lookup"><span data-stu-id="6fe2e-236">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="6fe2e-237">Sur IIS 7 ou IIS 7.5, les requêtes avec une URL semblable à la suivante ne sont pas en mesure de trouver des pages qui ont le *.cshtml* ou *.vbhtml* extension :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-237">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="6fe2e-238">Le problème survient car la réécriture d’URL n’est pas activée par défaut pour IIS 7 ou IIS 7.5.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-238">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="6fe2e-239">Le scénario voyez généralement cette est que vous ne voyez pas le problème lorsque vous testez localement à l’aide d’IIS Express, mais que vous le rencontrez lorsque vous déployez votre site Web sur un site Web d’hébergement.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-239">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="6fe2e-240">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-240">**Workaround**</span></span>
> 
> - <span data-ttu-id="6fe2e-241">Si vous avez un contrôle sur l’ordinateur du serveur, sur l’ordinateur serveur installer la mise à jour qui est décrite dans [une mise à jour est disponible que permet certains gestionnaires d’IIS 7.0 ou IIS 7.5 pour gérer les demandes dont les URL ne se termine pas par un point](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="6fe2e-241">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="6fe2e-242">Si vous n’avez pas de contrôle sur l’ordinateur du serveur (par exemple, vous déployez sur un site Web d’hébergement), ajoutez le code suivant du site Web *web.config* fichier :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-242">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *web.config* file:</span></span> 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="6fe2e-243">Problème : Déploiement d’une application sur un ordinateur qui n’a pas de SQL Server Compact installé</span><span class="sxs-lookup"><span data-stu-id="6fe2e-243">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="6fe2e-244">Les applications qui incluent des bases de données SQL Server Compact peuvent s’exécuter sur un ordinateur où SQL Server Compact n’est pas installé.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-244">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="6fe2e-245">Microsoft WebMatrix 1.0 automatiquement des copies de ces fichiers binaires et effectue approprié *web.config* transformations de fichiers.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-245">Microsoft WebMatrix 1.0 automatically copies these binaries for you and performs the appropriate *web.config* file transforms.</span></span>
> 
> <span data-ttu-id="6fe2e-246">**Solution de contournement** si vous devez copier ces fichiers et rendre le *web.config* modifications de fichiers manuellement, effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-246">**Workaround** If you need to copy these files and make the *web.config* file changes manually, do the following:</span></span>
> 
> 1. <span data-ttu-id="6fe2e-247">Copiez les assemblys du moteur de base de données à la *Bin* dossier (et sous-dossiers) de l’application sur l’ordinateur cible :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-247">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span>  
> 
>    - <span data-ttu-id="6fe2e-248">Copie *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span><span class="sxs-lookup"><span data-stu-id="6fe2e-248">Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span></span>  
>        <span data-ttu-id="6fe2e-249">**pour** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="6fe2e-249">**to** *\Bin*</span></span>
>    - <span data-ttu-id="6fe2e-250">Copie <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>à</em></strong>\Bin\x86\*</span><span class="sxs-lookup"><span data-stu-id="6fe2e-250">Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>to</em></strong>\Bin\x86\*</span></span>
>    - <span data-ttu-id="6fe2e-251">Copie <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>\* <strong>à</strong><em>\Bin\amd64</em></span><span class="sxs-lookup"><span data-stu-id="6fe2e-251">Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>\* <strong>to</strong><em>\Bin\amd64</em></span></span>
> 
> 2. <span data-ttu-id="6fe2e-252">Dans le dossier racine du site Web, créez ou ouvrez un *web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-252">In the root folder of the website, create or open a *web.config* file.</span></span> <span data-ttu-id="6fe2e-253">(Dans WebMatrix, 1.0, ce type de fichier est disponible si vous cliquez sur **tous les** dans le **choisir un Type de fichier** boîte de dialogue.)</span><span class="sxs-lookup"><span data-stu-id="6fe2e-253">(In WebMatrix 1.0, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="6fe2e-254">Ajoutez l’élément suivant en tant qu’enfant de le `<configuration>` élément (pas à l’intérieur de la `<system.web>` élément) :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-254">Add the following element as a child of the `<configuration>` element (not inside the `<system.web>` element):</span></span>
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="6fe2e-255">Problème : « Database » et « WebGrid » helpers ne fonctionnent pas dans Medium Trust en Visual Basic</span><span class="sxs-lookup"><span data-stu-id="6fe2e-255">Issue: "Database" and "WebGrid" helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="6fe2e-256">Si vous utilisez Visual Basic (création *.vbhtml* fichiers), le `Database` et `WebGrid` helpers ne fonctionnera pas si l’application est configurée pour utiliser la confiance moyenne.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-256">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="6fe2e-257">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-257">**Workaround**</span></span>  
> <span data-ttu-id="6fe2e-258">Si vous utilisez Visual Studio 2010, vous pouvez résoudre ce problème en installant la version Service Pack 1.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-258">If you use Visual Studio 2010, you can resolve this problem by installing the Service Pack 1 release.</span></span> <span data-ttu-id="6fe2e-259">Jusqu'à ce que la version finale de la version SP1 est disponible, vous pouvez télécharger la version bêta du SP1 depuis le [Microsoft Visual Studio 2010 Service Pack 1 bêta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) page du Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-259">Until the final version of the SP1 release is available, you can download the Beta version of SP1 from the [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) page on the Microsoft Download Center.</span></span>   
>   
> <span data-ttu-id="6fe2e-260">Si ce n’est pas pratique, ou si vous n’utilisez pas Visual Studio 2010, vous pouvez temporairement définie l’application pour utiliser la confiance totale.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-260">If this is not practical, or if you do not use Visual Studio 2010, you can temporarily set the application to use Full Trust.</span></span>


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a><span data-ttu-id="6fe2e-261">Problème : « ApplicationPart » ressources accessibles en externe</span><span class="sxs-lookup"><span data-stu-id="6fe2e-261">Issue: "ApplicationPart" resources are externally accessible</span></span>

> <span data-ttu-id="6fe2e-262">Si un assembly contient des objets qui dérive de la `ApplicationPart` classe, que les ressources de l’assembly sont exposées par le `ResourceRouteHandler` classe.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-262">If an assembly contains objects that derives from the `ApplicationPart` class, that assembly's resources are exposed by the `ResourceRouteHandler` class.</span></span> <span data-ttu-id="6fe2e-263">Considérez par exemple l’URL suivante :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-263">For example, consider the following URL:</span></span>  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> <span data-ttu-id="6fe2e-264">Cette requête télécharge toutes les chaînes de ressources dans le *System.Web.WebPages.Administration.dll* assembly.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-264">This request downloads all of the resource strings in the *System.Web.WebPages.Administration.dll* assembly.</span></span> <span data-ttu-id="6fe2e-265">Toutes les ressources incorporées (même ceux qui ne sont pas destinées à être traitées en tant que contenu statique) sont téléchargées.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-265">All of the embedded resources (even those that are not intended to be served as static content) are downloaded.</span></span> <span data-ttu-id="6fe2e-266">Si les ressources intégrées contiennent des informations sensibles, cela peut représenter un risque de sécurité.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-266">If the embedded resources contain sensitive information, this can represent a security risk.</span></span> 
> 
> <span data-ttu-id="6fe2e-267">**Solution de contournement** </span><span class="sxs-lookup"><span data-stu-id="6fe2e-267">**Workaround** </span></span>  
> <span data-ttu-id="6fe2e-268">Si vous créez un **ApplicationPart** d’objet, assurez-vous que les ressources incorporées associés **ApplicationPart** assembly de l’objet ne contiennent pas d’informations sensibles.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-268">If you create an **ApplicationPart** object, make sure that the embedded resources associated with that **ApplicationPart** object's assembly do not contain sensitive information.</span></span>


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a><span data-ttu-id="6fe2e-269">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="6fe2e-269">WebMatrix</span></span>

> [!NOTE]
> <span data-ttu-id="6fe2e-270">Pour plus d’informations sur les problèmes d’installation de WebMatrix, consultez [problèmes d’Installation de WebMatrix](#Known_Issues_Installation) plus haut dans ce document.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-270">For information about installation issues for WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>


<span data-ttu-id="6fe2e-271">Cette section du document décrit les problèmes connus pour l’environnement de développement WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-271">This section of the document describes known issues for the WebMatrix development environment.</span></span>

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a><span data-ttu-id="6fe2e-272">Problème : Les modifications dans le nom d’utilisateur ou le mot de passe d’une chaîne de connexion de base de données dans un fichier web.config ne sont pas répercutées dans l’espace de travail de bases de données</span><span class="sxs-lookup"><span data-stu-id="6fe2e-272">Issue: Changes in the username or password of a database connection string in a web.config file are not reflected in the Databases workspace</span></span>

> <span data-ttu-id="6fe2e-273">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-273">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="6fe2e-274">Dans le *web.config* , modifiez le nom de la base de données dans la chaîne de connexion (par exemple, ajoutez « 1 » à ce dernier).</span><span class="sxs-lookup"><span data-stu-id="6fe2e-274">In the *web.config* file, change the database name in the connection string (for example, add "1" to it).</span></span>
> 2. <span data-ttu-id="6fe2e-275">Enregistrer le *web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-275">Save the *web.config* file.</span></span>
> 3. <span data-ttu-id="6fe2e-276">Cliquez sur **bases de données** et actualiser.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-276">Click **Databases** and refresh.</span></span>
> 4. <span data-ttu-id="6fe2e-277">Modifier le nom de la base de données dans la chaîne de connexion dans le *web.config* fichier vers le nom de base de données d’origine.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-277">Change the database name in the connection string in the *web.config* file back to the original database name.</span></span>
> 5. <span data-ttu-id="6fe2e-278">Enregistrer le *web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-278">Save the *web.config* file.</span></span>
> 6. <span data-ttu-id="6fe2e-279">Cliquez sur **bases de données** et actualiser.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-279">Click **Databases** and refresh.</span></span>


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a><span data-ttu-id="6fe2e-280">Problème : Les dossiers créés par WebMatrix ne peut pas être supprimés.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-280">Issue: Folders created by WebMatrix cannot be deleted</span></span>

> <span data-ttu-id="6fe2e-281">Si WebMatrix est en cours d’exécution à l’aide d’autorisations élevées (autrement dit, vous avez démarré à l’aide de WebMatrix le **exécuter en tant qu’administrateur** option dans Windows), les dossiers qui sont créés par WebMatrix ne peut pas être supprimés à l’aide de l’Explorateur Windows.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-281">If WebMatrix is running using elevated permissions (that is, you started WebMatrix using the **Run as Administrator** option in Windows), folders that are created by WebMatrix cannot be deleted using Windows Explorer.</span></span>
> 
> <span data-ttu-id="6fe2e-282">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-282">**Workaround**</span></span>  
> <span data-ttu-id="6fe2e-283">Exécutez l’Explorateur Windows à l’aide d’autorisations élevées.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-283">Run Windows Explorer using elevated permissions.</span></span> <span data-ttu-id="6fe2e-284">Procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-284">Follow these steps:</span></span>  
> 
> 1. <span data-ttu-id="6fe2e-285">Dans Windows, cliquez sur **Démarrer**.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-285">In Windows, click **Start**.</span></span>
> 2. <span data-ttu-id="6fe2e-286">Entrez « Windows Explorer » et cliquez sur l’entrée pour **Windows Explorer**.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-286">Enter "Windows Explorer" and right-click the entry for **Windows Explorer**.</span></span>
> 3. <span data-ttu-id="6fe2e-287">Cliquez sur **exécuter en tant qu’administrateur**.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-287">Click **Run as Administrator**.</span></span> <span data-ttu-id="6fe2e-288">Vous pouvez ensuite supprimer les dossiers.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-288">You can then delete the folders.</span></span>


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="6fe2e-289">Problème : WebMatrix 1.0 ne peut pas effectuer certaines tâches qui requièrent une élévation</span><span class="sxs-lookup"><span data-stu-id="6fe2e-289">Issue: WebMatrix 1.0 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="6fe2e-290">WebMatrix 1.0 ne peut pas effectuer certaines tâches qui requièrent une élévation, telles que l’installation des composants supplémentaires dans les situations suivantes :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-290">WebMatrix 1.0 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="6fe2e-291">Sur Windows Vista ou Windows 7, vous êtes connecté avec un compte qui ne dispose pas des privilèges d’administrateur et contrôle de compte utilisateur (UAC) est désactivé.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-291">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="6fe2e-292">Vous utilisez Microsoft Windows XP ou Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-292">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="6fe2e-293">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-293">**Workaround**</span></span>  
> <span data-ttu-id="6fe2e-294">La plupart des tâches dans WebMatrix 1.0 ne nécessitent pas d’autorisation d’administrateur.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-294">Most tasks in WebMatrix 1.0 do not require administrative permission.</span></span> <span data-ttu-id="6fe2e-295">Pour les autres, vous pouvez effectuer l’opération en tant qu’administrateur, ou procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-295">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="6fe2e-296">Sur Windows Vista ou Windows 7, activer l’UAC.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-296">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="6fe2e-297">Sur Windows XP, ajoutez l’utilisateur au groupe de sécurité Administrateurs.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-297">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="6fe2e-298">Problème : « Site depuis la galerie Web » est désactivée.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-298">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="6fe2e-299">Le **Site à partir de la galerie Web** option est désactivée si le serveur Web Platform Installer 3.0 n’est pas installé.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-299">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="6fe2e-300">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-300">**Workaround**</span></span>  
> <span data-ttu-id="6fe2e-301">Installer le [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="6fe2e-301">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="6fe2e-302">Problème : Google Chrome n’est pas disponible en tant qu’une option d’exécution</span><span class="sxs-lookup"><span data-stu-id="6fe2e-302">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="6fe2e-303">Google Chrome n’est pas affiché dans la liste des navigateurs sous **exécuter** sur le **accueil** onglet.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-303">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="6fe2e-304">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-304">**Workaround**</span></span>  
> <span data-ttu-id="6fe2e-305">Certaines versions de Google Chrome n’inscrivez pas eux-mêmes correctement avec la fonctionnalité programmes par défaut dans Windows.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-305">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="6fe2e-306">Pour résoudre ce problème, démarrez Google Chrome, cliquez sur le *personnaliser et contrôle Google Chrome* menu, cliquez sur *Options*, puis cliquez sur *Make Google Chrome mon navigateur par défaut*.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-306">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="6fe2e-307">Problème : La boîte de dialogue « Foreign Key » n’autorise pas entrer une clé primaire</span><span class="sxs-lookup"><span data-stu-id="6fe2e-307">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="6fe2e-308">Le **clé étrangère** boîte de dialogue ne vous permet pas à entrer le nom de clé primaire à partir de la table de clé primaire.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-308">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="6fe2e-309">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-309">**Workaround**</span></span>  
> <span data-ttu-id="6fe2e-310">Ceci est intentionnel.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-310">This is intentional.</span></span> <span data-ttu-id="6fe2e-311">Vous n’avez pas besoin d’entrer le nom de la clé primaire à partir de la table de clé primaire.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-311">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a><span data-ttu-id="6fe2e-312">Problème : IntelliSense n’est pas disponible dans WebMatrix pour Razor syntaxe, c# ou Visual Basic</span><span class="sxs-lookup"><span data-stu-id="6fe2e-312">Issue: IntelliSense is not available in WebMatrix for Razor syntax, C#, or Visual Basic</span></span>

> <span data-ttu-id="6fe2e-313">IntelliSense est prise en charge dans WebMatrix pour HTML et CSS.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-313">IntelliSense is supported in WebMatrix for HTML and CSS.</span></span> <span data-ttu-id="6fe2e-314">Toutefois, il n’est pas disponible pour d’autres langages.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-314">However, it is not available for other languages.</span></span> 
> 
> <span data-ttu-id="6fe2e-315">**Solution de contournement** </span><span class="sxs-lookup"><span data-stu-id="6fe2e-315">**Workaround** </span></span>  
> <span data-ttu-id="6fe2e-316">Aucun.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-316">None.</span></span>


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a><span data-ttu-id="6fe2e-317">Problème : IntelliSense pour HTML et CSS suggère des éléments qui ne sont pas appropriées en fonction du contexte</span><span class="sxs-lookup"><span data-stu-id="6fe2e-317">Issue: IntelliSense for HTML and CSS suggests elements that are not contextually appropriate</span></span>

> <span data-ttu-id="6fe2e-318">IntelliSense pour le balisage dans WebMatrix prend en charge le HTML à l’aide du [schéma XHTML 1.0 Transitional](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) et à l’aide de CSS le [CSS 2.1 schéma](http://www.w3.org/TR/CSS2/).</span><span class="sxs-lookup"><span data-stu-id="6fe2e-318">IntelliSense for markup in WebMatrix supports HTML using the [XHTML 1.0 Transitional schema](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) and CSS using the [CSS 2.1 schema](http://www.w3.org/TR/CSS2/).</span></span> <span data-ttu-id="6fe2e-319">IntelliSense étant basé sur ces schémas spécifiques, certaines balises, des attributs ou propriétés peuvent suggérées qui ne sont pas appropriées pour la définition de style ou de la page actuelle.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-319">Because IntelliSense is based on these specific schemas, certain tags, attributes, or properties might be suggested that are not appropriate for the current page or style definition.</span></span> <span data-ttu-id="6fe2e-320">Pour HTML, il peut également entraîner suggestions inattendues dans le contenu qui peut être interprétée en tant que XHTML incorrect (par exemple, lorsque les balises ne sont pas fermées).</span><span class="sxs-lookup"><span data-stu-id="6fe2e-320">For HTML, it can also lead to unexpected suggestions in content that might be interpreted as malformed XHTML (for example, when tags are not closed).</span></span> <span data-ttu-id="6fe2e-321">Ce problème peut être plus visible si le point d’insertion est à l’intérieur d’une balise incomplète ; Dans ce cas, IntelliSense peut suggérer des balises d’ouverture ou proposer d’autres suggestions incorrectes.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-321">This issue might be more noticeable if the insertion point is inside an incomplete tag; in that case, IntelliSense might suggest new opening tags or offer other incorrect suggestions.</span></span> 
> 
> <span data-ttu-id="6fe2e-322">**Solution de contournement** </span><span class="sxs-lookup"><span data-stu-id="6fe2e-322">**Workaround** </span></span>  
> <span data-ttu-id="6fe2e-323">Pour HTML, assurez-vous que vous travaillez dans une page XHTML correcte et complet.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-323">For HTML, make sure that you are working within a well-formed, complete XHTML page.</span></span> <span data-ttu-id="6fe2e-324">Pour le CSS, il n’existe aucune solution de contournement.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-324">For CSS, there is no workaround.</span></span>


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a><span data-ttu-id="6fe2e-325">Problème : IntelliSense n’est pas appelée en cours de frappe</span><span class="sxs-lookup"><span data-stu-id="6fe2e-325">Issue: IntelliSense is not invoked while you type</span></span>

> <span data-ttu-id="6fe2e-326">Dans certains cas, IntelliSense ne peut pas être appelée comme code HTML ou CSS est entré dans l’éditeur.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-326">At times, IntelliSense might not be invoked as HTML or CSS is being entered in the editor.</span></span> <span data-ttu-id="6fe2e-327">En particulier, cela peut se produire lorsque le point d’insertion est directement en regard d’un autre élément ou à la fin d’un fichier.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-327">In particular, this might happen when the insertion point is directly next to another element or at the end of a file.</span></span> 
> 
> <span data-ttu-id="6fe2e-328">**Solution de contournement** </span><span class="sxs-lookup"><span data-stu-id="6fe2e-328">**Workaround** </span></span>  
> <span data-ttu-id="6fe2e-329">Assurez-vous qu’il y a un espace blanc autour du point d’insertion et que le point d’insertion n’est pas à la fin d’un fichier.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-329">Make sure that there is whitespace around the insertion point and that the insertion point is not at the end of a file.</span></span> <span data-ttu-id="6fe2e-330">Vous pouvez également appeler IntelliSense manuellement en appuyant sur Ctrl + espace.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-330">You can also invoke IntelliSense manually by pressing Ctrl+Space.</span></span>


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a><span data-ttu-id="6fe2e-331">Problème : Aucune interface utilisateur n’est disponible pour la désactivation d’IntelliSense</span><span class="sxs-lookup"><span data-stu-id="6fe2e-331">Issue: No UI is available for disabling IntelliSense</span></span>

> <span data-ttu-id="6fe2e-332">WebMatrix 1.0 ne fournit aucune interface utilisateur ou un mouvement de la désactivation d’IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-332">WebMatrix 1.0 provides no UI or gesture for disabling IntelliSense.</span></span> 
> 
> <span data-ttu-id="6fe2e-333">**Solution de contournement** </span><span class="sxs-lookup"><span data-stu-id="6fe2e-333">**Workaround** </span></span>  
> <span data-ttu-id="6fe2e-334">Démarrez WebMatrix à l’aide de la commande suivante, qui inclut un commutateur qui désactive IntelliSense :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-334">Start WebMatrix using the following command, which includes a switch that disables IntelliSense:</span></span>  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a><span data-ttu-id="6fe2e-335">IIS Express</span><span class="sxs-lookup"><span data-stu-id="6fe2e-335">IIS Express</span></span>

<span data-ttu-id="6fe2e-336">IIS Express a son propre fichier readme, qui est disponible à l’adresse suivante :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-336">IIS Express has its own readme file, which is available at the following URL:</span></span>

[<span data-ttu-id="6fe2e-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp; clcid = 0 x 409</span><span class="sxs-lookup"><span data-stu-id="6fe2e-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409</span></span>](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a><span data-ttu-id="6fe2e-338">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="6fe2e-338">SQL Server Compact</span></span>

<span data-ttu-id="6fe2e-339">SQL Server Compact a son propre fichier readme, qui est disponible à l’adresse suivante :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-339">SQL Server Compact has its own readme file, which is available at the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

<span data-ttu-id="6fe2e-340">Pour plus d’informations sur les problèmes qui impliquent l’installation de SQL Server Compact comme faisant partie de WebMatrix, consultez [problèmes d’Installation de WebMatrix](#Known_Issues_Installation) plus haut dans ce document.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-340">For information about issues that involve installing SQL Server Compact as part of WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>

### <a id="Known_Issues_Installing_Applications"></a>  <span data-ttu-id="6fe2e-341">Installation d’Applications</span><span class="sxs-lookup"><span data-stu-id="6fe2e-341">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="6fe2e-342">Problème : Installation d’une application peut prendre un certain temps si le dossier Mes Documents de l’utilisateur est redirigé vers un partage réseau</span><span class="sxs-lookup"><span data-stu-id="6fe2e-342">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="6fe2e-343">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-343">**Workaround**</span></span>  
> <span data-ttu-id="6fe2e-344">Aucun.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-344">None.</span></span> <span data-ttu-id="6fe2e-345">L’application peut prendre un certain temps à installer, mais ne s’installe correctement.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-345">The application might take a while to install, but will install correctly.</span></span>


### <a id="Known_Issues_Publishing_Applications"></a>  <span data-ttu-id="6fe2e-346">Publication d’Applications</span><span class="sxs-lookup"><span data-stu-id="6fe2e-346">Publishing Applications</span></span>

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a><span data-ttu-id="6fe2e-347">Problème : « requise autorisations ne peuvent pas être acquises » erreur lors de la publication d’une base de données SQL Compact</span><span class="sxs-lookup"><span data-stu-id="6fe2e-347">Issue: "Required permissions cannot be acquired" error when publishing a SQL Compact Database</span></span>

> <span data-ttu-id="6fe2e-348">WebMatrix ne prend pas entièrement en charge déployer les fichiers binaires de prise en charge pour SQL Server Compact sur un serveur qui est en cours d’exécution .NET Framework version 3.5 avec une configuration de niveau de confiance moyen.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-348">WebMatrix does not fully support deploying supporting binaries for SQL Server Compact to a server that is running .NET Framework version 3.5 with a medium trust configuration.</span></span>
> 
> <span data-ttu-id="6fe2e-349">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-349">**Workaround**</span></span>  
> <span data-ttu-id="6fe2e-350">La solution préférée consiste à installer le .NET Framework 4 sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-350">The preferred workaround is to install the .NET Framework 4 on the server.</span></span> <span data-ttu-id="6fe2e-351">Vous pouvez également effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-351">Alternatively, do the following:</span></span>
> 
> 1. <span data-ttu-id="6fe2e-352">Ajoutez les éléments suivants à la `SecurityClasses` section *Web\_MediumTrust.config* fichier :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-352">Add the following elements to the `SecurityClasses` section in *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. <span data-ttu-id="6fe2e-353">Créer une nouveau jeu d’autorisations dans le *Web\_MediumTrust.config* fichier avec les autorisations requises suivantes :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-353">Create a new permission set in the *Web\_MediumTrust.config* file with the following required permissions:</span></span>
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. <span data-ttu-id="6fe2e-354">Appliquer la jeu d’autorisations à SQL Server Compact en plaçant les éléments suivants dans le *Web\_MediumTrust.config* fichier :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-354">Apply the permission set to SQL Server Compact by putting the following elements in the *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a><span data-ttu-id="6fe2e-355">Problème : Les applications web galerie et PhpBB affichent une erreur « Le Service n’est pas disponible » après la publication</span><span class="sxs-lookup"><span data-stu-id="6fe2e-355">Issue: Gallery and PhpBB web applications display a "Service is unavailable" error after publishing</span></span>

> <span data-ttu-id="6fe2e-356">Dans certaines circonstances, la publication d’une application provoque une erreur « le service n’est pas disponible ».</span><span class="sxs-lookup"><span data-stu-id="6fe2e-356">Under some circumstances, publishing an application causes a "service is unavailable" error.</span></span>
> 
> <span data-ttu-id="6fe2e-357">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-357">**Workaround**</span></span>  
> <span data-ttu-id="6fe2e-358">Dans WebMatrix, ajoutez une barre oblique inverse (\) à la fin du nom du serveur dans le **paramètres de publication** fenêtre puis publier à nouveau l’application.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-358">In WebMatrix, add a backslash (\) to the end of the server name in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a><span data-ttu-id="6fe2e-359">Problème : Mise en page du site Web de Moodle et les liens sont rompus après la publication</span><span class="sxs-lookup"><span data-stu-id="6fe2e-359">Issue: Moodle website layout and links are broken after publishing</span></span>

> <span data-ttu-id="6fe2e-360">Une fois que vous publiez une application de Moodle, l’application ne fonctionne pas correctement.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-360">After you publish a Moodle application, the application does not work correctly.</span></span>
> 
> <span data-ttu-id="6fe2e-361">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-361">**Workaround**</span></span>  
> <span data-ttu-id="6fe2e-362">Dans WebMatrix, ajoutez une barre oblique (/) à la fin de la **nom du Site** champ dans le **paramètres de publication** fenêtre puis publier à nouveau l’application.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-362">In WebMatrix, add a slash (/) to the end of the **Site Name** field in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a><span data-ttu-id="6fe2e-363">Problème : Publication nopCommerce échoue avec une erreur de base de données</span><span class="sxs-lookup"><span data-stu-id="6fe2e-363">Issue: Publishing nopCommerce fails with a database error</span></span>

> <span data-ttu-id="6fe2e-364">NopCommerce publication échoue et signale une erreur de base de données, comme « insérer dans le nop\_table du journal a échoué. »</span><span class="sxs-lookup"><span data-stu-id="6fe2e-364">Publishing nopCommerce fails and reports a database error like "Insert into the nop\_log table failed."</span></span>
> 
> <span data-ttu-id="6fe2e-365">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-365">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="6fe2e-366">Dans WebMatrix, cliquez sur **exécuter** pour lancer nopCommerce localement.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-366">In WebMatrix, click **Run** to launch nopCommerce locally.</span></span>
> 2. <span data-ttu-id="6fe2e-367">Connectez-vous à la page d’administration.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-367">Log into the administration page.</span></span>
> 3. <span data-ttu-id="6fe2e-368">Cliquez sur le **système** menu.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-368">Click the **System** menu.</span></span>
> 4. <span data-ttu-id="6fe2e-369">Cliquez sur le **journal** option.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-369">Click the **Log** option.</span></span>
> 5. <span data-ttu-id="6fe2e-370">Cliquez sur le **effacer le journal** bouton.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-370">Click the **Clear Log** button.</span></span>
> 6. <span data-ttu-id="6fe2e-371">Republiez nopCommerce.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-371">Publish nopCommerce again.</span></span>


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a><span data-ttu-id="6fe2e-372">Problème : Silverstripe CMS affiche une « erreur HTTP 500 PHP FCGI » lorsque vous téléchargez un site publié</span><span class="sxs-lookup"><span data-stu-id="6fe2e-372">Issue: Silverstripe CMS displays a "HTTP 500 PHP FCGI Error" when you download a published site</span></span>

> <span data-ttu-id="6fe2e-373">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-373">**Workaround**</span></span>  
> <span data-ttu-id="6fe2e-374">Après avoir cliqué sur **téléchargement publiée site**, ignorer `silverstripe-cache/manifest_main` dans **publier l’aperçu**.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-374">After you click **Download published site**, skip `silverstripe-cache/manifest_main` in **Publish Preview**.</span></span> <span data-ttu-id="6fe2e-375">Ce fichier est utilisé pour la mise en cache et est spécifique à chaque ordinateur.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-375">This file is used for caching purposes and is specific to each computer.</span></span>


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a><span data-ttu-id="6fe2e-376">Problème : Subtext affiche « Erreur de serveur dans l’Application '/' » lorsque vous téléchargez un site publié</span><span class="sxs-lookup"><span data-stu-id="6fe2e-376">Issue: Subtext displays "Server Error in '/' Application" when you download a published site</span></span>

> <span data-ttu-id="6fe2e-377">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-377">**Workaround**</span></span>  
> <span data-ttu-id="6fe2e-378">Ouvrez le site *web.config* fichier et remplacez l’ID utilisateur et le mot de passe dans la chaîne de connexion de base de données avec les informations d’identification d’administrateur SQL Server (les informations d’identification « sa »).</span><span class="sxs-lookup"><span data-stu-id="6fe2e-378">Open the site's *web.config* file and replace the user ID and password in the database connection string with the SQL Server administrator credentials (the "sa" credentials).</span></span>
> 
> <span data-ttu-id="6fe2e-379">Vous pouvez également procéder comme suit afin d’accorder le compte d’utilisateur que vous êtes connecté avec `db_owner` autorisations :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-379">Alternatively, follow these steps in order to give the user account you are logged in with `db_owner` permissions:</span></span>
> 
> 1. <span data-ttu-id="6fe2e-380">Installer SQL Server Management Studio à l’aide de Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-380">Install SQL Server Management Studio using the Web Platform Installer.</span></span>
> 2. <span data-ttu-id="6fe2e-381">Se connecter à l’instance de SQL Server Express locale (par défaut, `.\SQLEXPRESS`).</span><span class="sxs-lookup"><span data-stu-id="6fe2e-381">Connect to the local SQL Server Express instance (by default, `.\SQLEXPRESS`).</span></span>
> 3. <span data-ttu-id="6fe2e-382">Cliquez sur **bases de données** &gt; *[localSubtextDatabase]* &gt; **sécurité** &gt; **utilisateurs** &gt; *[localSubtextUser*] (valeur par défaut est `subtextuser`], avec le bouton droit, puis cliquez sur **propriétés**.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-382">Click **Databases** &gt; *[localSubtextDatabase]* &gt; **Security** &gt; **Users** &gt; *[localSubtextUser*] (default is `subtextuser`], right-click, and click **Properties**.</span></span>
> 4. <span data-ttu-id="6fe2e-383">Sélectionnez **db\_propriétaire** dans la section de l’appartenance au rôle.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-383">Select **db\_owner** in the role membership section.</span></span>


#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="6fe2e-384">Problème : Site peuvent ne pas fonctionne après la publication si le champ « URL de Destination » n’est pas précédé par http:// ou https://</span><span class="sxs-lookup"><span data-stu-id="6fe2e-384">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="6fe2e-385">Dans le **paramètres de publication** boîte de dialogue, si l’URL de destination ne commence pas par `http://` ou `https://`, le site peut ne pas fonctionne après le déploiement.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-385">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="6fe2e-386">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-386">**Workaround**</span></span>  
> <span data-ttu-id="6fe2e-387">Assurez-vous que l’option avant de publier un site, l’URL de destination dans le **paramètres de publication** boîte de dialogue commence par `http://` ou `https://`.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-387">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="6fe2e-388">Problème : Publication d’une base de données MySQL échoue avec l’erreur « Échec de publication de la base de données.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-388">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="6fe2e-389">Cela peut se produire si la base de données distante ne peut pas exécuter le script. »</span><span class="sxs-lookup"><span data-stu-id="6fe2e-389">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="6fe2e-390">L’erreur peut se produire pour plusieurs raisons.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-390">The error can occur for a number of reasons.</span></span> <span data-ttu-id="6fe2e-391">Vous pouvez voir cette erreur est si le script de base de données contient un guillemet simple (') et le jeu de caractères par défaut de la destination MySQL de base de données n’est pas au format UTF-8.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-391">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="6fe2e-392">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-392">**Workaround**</span></span>  
> <span data-ttu-id="6fe2e-393">Définir le caractère par défaut définie pour la base de données MySQL à distance au format UTF-8.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-393">Set the default character set for the remote MySQL database to UTF-8.</span></span>


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a><span data-ttu-id="6fe2e-394">Problème : Certains liens ne sont pas visibles dans DotNetNuke après avoir publié ou de téléchargement du site</span><span class="sxs-lookup"><span data-stu-id="6fe2e-394">Issue: Some links are not visible in DotNetNuke after publishing or downloading the site</span></span>

> <span data-ttu-id="6fe2e-395">Si vous publiez ou téléchargez un site DotNetNuke, vous devrez peut-être effacer le cache pour obtenir les nouveaux liens à afficher sur le site.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-395">If you publish or download a DotNetNuke site, you might need to clear the cache to get the new links to appear on the site.</span></span>
> 
> <span data-ttu-id="6fe2e-396">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-396">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="6fe2e-397">Connectez-vous en tant que « Host ».</span><span class="sxs-lookup"><span data-stu-id="6fe2e-397">Log in as "Host".</span></span>
> 2. <span data-ttu-id="6fe2e-398">Accédez au menu hôte et sélectionnez **paramètres de l’hôte**.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-398">Go to the host menu and select **Host Settings**.</span></span>
> 3. <span data-ttu-id="6fe2e-399">Faites défiler et sous **paramètres avancés**, développez **les paramètres de performances**.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-399">Scroll down and under **Advanced Settings**, expand **Performance Settings**.</span></span>
> 4. <span data-ttu-id="6fe2e-400">Cliquez sur le **vider le Cache** lien pour les pages.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-400">Click the **Clear Cache** link for pages.</span></span>
> 5. <span data-ttu-id="6fe2e-401">Accédez au bas de la page et redémarrer l’application.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-401">Go to the bottom of the page and restart the application.</span></span>


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a><span data-ttu-id="6fe2e-402">Problème : Certains liens dans AtomSite sont interrompues après avoir téléchargé un site publié</span><span class="sxs-lookup"><span data-stu-id="6fe2e-402">Issue: Some links in AtomSite are broken after you download a published site</span></span>

> <span data-ttu-id="6fe2e-403">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-403">**Workaround**</span></span>  
> <span data-ttu-id="6fe2e-404">Dans le *service.config* fichier, *users.config* fichier et tous *.xml* fichiers, remplacez la chaîne d’URL (par exemple, `http://myhost.com/atomsite`) avec celui local (par exemple, `http://localhost:1239`).</span><span class="sxs-lookup"><span data-stu-id="6fe2e-404">In the *service.config* file, *users.config* file, and all *.xml* files, replace the URL string (for example, `http://myhost.com/atomsite`) with the local one (for example, `http://localhost:1239`).</span></span>


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a><span data-ttu-id="6fe2e-405">Problème : Basée sur MySQL applications telles que WordPress de ne pas publier et de signaler une erreur de base de données</span><span class="sxs-lookup"><span data-stu-id="6fe2e-405">Issue: MySQL-based applications like WordPress fail to publish and report a database error</span></span>

> <span data-ttu-id="6fe2e-406">Par défaut, WebMatrix installé MySQL avec le jeu de caractères UTF-8.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-406">By default, WebMatrix installs MySQL with the UTF-8 character set.</span></span> <span data-ttu-id="6fe2e-407">Si vous installez MySQL sur votre propre, et le jeu de caractères n’est pas UTF-8 (par exemple, il est Latin1), le processus de publication pour les bases de données peut échouer.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-407">If you install MySQL on your own, and the character set is not UTF-8 (for example, it is Latin1), the publish process for databases might fail.</span></span>
> 
> <span data-ttu-id="6fe2e-408">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-408">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="6fe2e-409">Modifier le jeu de caractères pour MySQL au format UTF-8.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-409">Change the character set for MySQL to UTF-8.</span></span> <span data-ttu-id="6fe2e-410">(Pour plus d’informations, consultez [serveur du jeu de caractères et le classement](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) sur le site Web MySQL.)</span><span class="sxs-lookup"><span data-stu-id="6fe2e-410">(For details, see [Server Character Set and Collation](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) on the MySQL website.)</span></span>
> 2. <span data-ttu-id="6fe2e-411">Réinstallez l’application.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-411">Reinstall the application.</span></span>
> 3. <span data-ttu-id="6fe2e-412">Republier l’application.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-412">Republish the application.</span></span>


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a><span data-ttu-id="6fe2e-413">Problème : « Téléchargement publiés site » échoue pour les applications qui ont le programme d’installation basé sur navigateur</span><span class="sxs-lookup"><span data-stu-id="6fe2e-413">Issue: "Download published site" fails for applications that have browser-based setup</span></span>

> <span data-ttu-id="6fe2e-414">Certaines applications (par exemple, Kentico CMS) vous obligent à les lancer dans le navigateur pour exécuter le programme d’installation après l’installation telles que la création d’une base de données.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-414">Some applications (for example, Kentico CMS) require you to launch them in the browser in order to perform post-installation setup such as creating a database.</span></span> <span data-ttu-id="6fe2e-415">Si vous publiez une application comme celle-ci sans terminer l’installation basée sur navigateur, tentez de télécharger le même site à partir d’un serveur distant échoue.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-415">If you publish an application like this without completing the browser-based setup, attempting to download the same site from a remote server will fail.</span></span>
> 
> <span data-ttu-id="6fe2e-416">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-416">**Workaround**</span></span>  
> <span data-ttu-id="6fe2e-417">Terminer le programme d’installation basée sur navigateur avant de publier le site.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-417">Finish browser-based setup before publishing the site.</span></span>


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a><span data-ttu-id="6fe2e-418">Problème : « Téléchargement publiés site » échoue avec une erreur de base de données pour DotNetNuke et CMS Kooboo</span><span class="sxs-lookup"><span data-stu-id="6fe2e-418">Issue: "Download published site" fails with a database error for DotNetNuke and Kooboo CMS</span></span>

> <span data-ttu-id="6fe2e-419">Si vous essayez de télécharger une application à partir d’un serveur et que vous disposez des informations d’identification d’administrateur dans la chaîne de connexion de base de données dans le **paramètres de publication** boîte de dialogue, vous pouvez voir l’erreur suivante dans le journal de publication :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-419">If you try to download an application from a server and you have administrator credentials in the database connection string in the **Publish Settings** dialog, you might see the following error in the publish log:</span></span>
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> <span data-ttu-id="6fe2e-420">**Solution de contournement**</span><span class="sxs-lookup"><span data-stu-id="6fe2e-420">**Workaround**</span></span>  
> <span data-ttu-id="6fe2e-421">Si cela est possible, republiez le site (ou publication) à l’aide des informations d’identification non-administrateur pour la base de données.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-421">If practical, republish the site (or have it published) using non-administrator credentials for the database.</span></span>


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="6fe2e-422">Pour plus d'informations</span><span class="sxs-lookup"><span data-stu-id="6fe2e-422">For More Information</span></span>

<span data-ttu-id="6fe2e-423">Pour plus d’informations sur WebMatrix 1.0, consultez les sites Web suivants :</span><span class="sxs-lookup"><span data-stu-id="6fe2e-423">For more information about WebMatrix 1.0, see the following websites:</span></span>

- [<span data-ttu-id="6fe2e-424">IIS.net</span><span class="sxs-lookup"><span data-stu-id="6fe2e-424">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="6fe2e-425">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6fe2e-425">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="6fe2e-426">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="6fe2e-426">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

<span data-ttu-id="6fe2e-427">© 2011 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-427">© 2011 Microsoft Corporation.</span></span> <span data-ttu-id="6fe2e-428">Tous droits réservés.</span><span class="sxs-lookup"><span data-stu-id="6fe2e-428">All Rights Reserved.</span></span> <span data-ttu-id="6fe2e-429">[Conditions d’utilisation](https://msdn.microsoft.cos/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="6fe2e-429">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
