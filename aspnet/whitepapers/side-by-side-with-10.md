---
uid: whitepapers/side-by-side-with-10
title: L’exécution d’ASP.NET côte à côte du .NET Framework 1.0 et 1.1 | Documents Microsoft
author: rick-anderson
description: Ce livre blanc décrit comment installer .NET 1.0 et 1.1 de .NET sur votre ordinateur, en permettant à une application Web ASP.NET pour s’exécuter sur des versions de l’encadrer...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 939b04d440955b184bf5f4c40a2ef8175641edfa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530178"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a><span data-ttu-id="4a5a3-103">Exécution d’ASP.NET côte à côte du .NET Framework 1.0 et 1.1</span><span class="sxs-lookup"><span data-stu-id="4a5a3-103">ASP.NET Side-by-Side Execution of .NET Framework 1.0 and 1.1</span></span>
====================
> <span data-ttu-id="4a5a3-104">Ce livre blanc décrit comment installer .NET 1.0 et 1.1 de .NET sur votre ordinateur, ce qui permet une application Web ASP.NET pour s’exécuter sur des versions du framework.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-104">This whitepaper describes how to install both .NET 1.0 and .NET 1.1 on your machine, allowing an ASP.NET Web application to run on either version of the framework.</span></span>
> 
> <span data-ttu-id="4a5a3-105">S’applique à ASP.NET 1.0 et 1.1 d’ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-105">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="4a5a3-106">Dans ASP.NET, on dit des applications à s’exécuter côte à côte lorsqu’elles sont installées sur le même ordinateur, mais utilisent des versions différentes du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-106">In ASP.NET, applications are said to be running side by side when they are installed on the same computer, but use different versions of the .NET Framework.</span></span> <span data-ttu-id="4a5a3-107">La rubrique suivante décrit comment configurer des applications ASP.NET pour une exécution côte à côte et fournit les étapes détaillées pour :</span><span class="sxs-lookup"><span data-stu-id="4a5a3-107">The following topic describes how to configure ASP.NET applications for side-by-side execution and provides detailed steps to:</span></span>

- [<span data-ttu-id="4a5a3-108">Mettre à jour le mappage de votre application Web pour la version 1.0 du .NET Framework lors de l’installation</span><span class="sxs-lookup"><span data-stu-id="4a5a3-108">Maintain your Web application's mapping to .NET Framework version 1.0 during installation</span></span>](#1)
- [<span data-ttu-id="4a5a3-109">Mapper une application Web vers une version spécifique du .NET Framework</span><span class="sxs-lookup"><span data-stu-id="4a5a3-109">Map a Web application to a specific version of the .NET Framework</span></span>](#2)
- [<span data-ttu-id="4a5a3-110">Rechercher la version du .NET Framework qui utilise un site Web</span><span class="sxs-lookup"><span data-stu-id="4a5a3-110">Find the version of the .NET Framework that a Web site is using</span></span>](#3)

<span data-ttu-id="4a5a3-111">En règle générale, lorsqu’un composant ou une application est mise à jour sur un ordinateur, l’ancienne version est supprimée et remplacée par la version plus récente.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-111">Traditionally, when a component or application is updated on a computer, the older version is removed and replaced with the newer version.</span></span> <span data-ttu-id="4a5a3-112">Si la nouvelle version n’est pas compatible avec la version précédente, cela arrête généralement d’autres applications qui utilisent le composant ou l’application.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-112">If the new version is not compatible with the previous version, this usually breaks other applications that use the component or application.</span></span> <span data-ttu-id="4a5a3-113">Le .NET Framework prend en charge l’exécution côte à côte, ce qui permet à plusieurs versions d’un assembly ou d’une application soit installée sur le même ordinateur en même temps.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-113">The .NET Framework provides support for side-by-side execution, which allows multiple versions of an assembly or application to be installed on the same computer at the same time.</span></span> <span data-ttu-id="4a5a3-114">Étant donné que plusieurs versions peuvent être installées simultanément, les applications gérées peuvent sélectionner la version à utiliser sans affecter les applications qui utilisent une version différente.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-114">Because multiple versions can be installed simultaneously, managed applications can select which version to use without affecting applications that use a different version.</span></span>

<span data-ttu-id="4a5a3-115">Par défaut, lors de l’installation du .NET Framework version 1.1, toutes les applications ASP.NET existantes sont automatiquement reconfigurées pour utiliser la dernière version du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-115">By default, during the installation of the .NET Framework version 1.1, all existing ASP.NET applications are automatically reconfigured to use the latest version of the .NET Framework.</span></span> <span data-ttu-id="4a5a3-116">Si vous ne souhaitez pas que vos applications ASP.NET par défaut pour le .NET Framework 1.1, cliquez sur [ici](#1) pour savoir comment éviter ce problème lors de l’installation.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-116">If you do not want your ASP.NET applications to default to .NET Framework 1.1, click [here](#1) to learn how to prevent this during installation.</span></span>

<span data-ttu-id="4a5a3-117">Si vous mettez à jour votre serveur Web vers la version 1.1 de .NET Framework et que vous souhaitez une ou plusieurs applications Web pour exécuter le .NET Framework 1.0, vous devez mettre à jour le mappage de Script de Internet Information Services (IIS).</span><span class="sxs-lookup"><span data-stu-id="4a5a3-117">If you update your Web server to .NET Framework 1.1 and want one or more Web applications to run .NET Framework 1.0, you need to update the Internet Information Services (IIS) Script Map.</span></span> <span data-ttu-id="4a5a3-118">Le mappage du script est le mécanisme pour mapper l’extension de fichier .aspx pour une application Web spécifique à une version du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-118">The script mapping is the mechanism to map the .aspx file extension for a specific Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="4a5a3-119">Cliquez sur [ici](#2) pour savoir comment mapper une application Web vers une version spécifique du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-119">Click [here](#2) to learn how to map a Web application to a specific version of the .NET Framework.</span></span>

<span data-ttu-id="4a5a3-120">Vous pouvez utiliser le Gestionnaire d’informations Internet ou de l’outil ASP.NET IIS Registration (Aspnet\_regiis.exe) pour rechercher la version de .NET Framework s’exécute une application Web particulière.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-120">You can use the Internet Information Manger or the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe) to find which .NET Framework version is running a particular Web application.</span></span> <span data-ttu-id="4a5a3-121">Cliquez sur [ici](#3) pour savoir comment rechercher la version du .NET Framework qui utilise un site Web.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-121">Click [here](#3) to learn how to find the version of the .NET Framework that a Web site is using.</span></span>

<span data-ttu-id="4a5a3-122">Une importation pas prises en compte lors de la migration vers la version 1.1 de .NET Framework est que chaque version du .NET Framework utilise son propre fichier Machine.config.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-122">One import consideration when migrating to .NET Framework 1.1 is that each version of the .NET Framework uses its own Machine.config file.</span></span> <span data-ttu-id="4a5a3-123">Par conséquent, si un administrateur Web a apporté des modifications au fichier Machine.config, ces modifications doivent être migrées dans le fichier Machine.config de .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-123">As a result, if a Web administrator has made changes to the Machine.config file, those changes must be migrated to the .NET Framework 1.1 Machine.config file.</span></span>

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a><span data-ttu-id="4a5a3-124">Gestion de mappage de votre application Web pour .NET Framework 1.0 pendant l’installation</span><span class="sxs-lookup"><span data-stu-id="4a5a3-124">Maintaining your Web application's mapping to .NET Framework 1.0 during installation</span></span>

<span data-ttu-id="4a5a3-125">Par défaut, toutes les applications ASP.NET existantes sont automatiquement reconfigurées pendant l’installation pour utiliser la nouvelle version du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-125">By default, all existing ASP.NET applications are automatically reconfigured during installation to use the newer version of the .NET Framework.</span></span> <span data-ttu-id="4a5a3-126">À l’aide de la version la plus récente du .NET Framework, les applications peuvent tirer pleinement parti des améliorations et nouvelles fonctionnalités incluses dans la nouvelle version.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-126">Using the newer version of the .NET Framework, applications can take full advantage of improvements and new features included in the new release.</span></span> <span data-ttu-id="4a5a3-127">En même temps, l’administrateur Web, qui souhaitent un contrôle granulaire sur les applications qui sont mis à jour, peut empêcher le remappage automatique de toutes les applications ASP.NET existantes lors de l’installation du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-127">At the same time, the Web administrator, who might want granular control over which applications are updated, can prevent the automatic remapping of all existing ASP.NET applications during installation of the .NET Framework.</span></span>

<span data-ttu-id="4a5a3-128">Pour éviter le remappage automatique de l’application ASP.NET à la version la plus récente du .NET Framework, l’administrateur Web peut utiliser l’option de ligne de commande /noaspupgrade avec le programme d’installation de Dotnetfx.exe.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-128">To prevent the automatic remapping of the entire ASP.NET application to the newer version of the .NET Framework, the Web administrator can use the /noaspupgrade command-line option with the Dotnetfx.exe setup program.</span></span>

<span data-ttu-id="4a5a3-129">**Pour empêcher le remappage total d’application ASP.NET pour une version plus récente**</span><span class="sxs-lookup"><span data-stu-id="4a5a3-129">**To prevent total remapping of ASP.NET application to newer version**</span></span>

1. <span data-ttu-id="4a5a3-130">Accédez à **Démarrer**.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-130">Go to **Start**.</span></span>
2. <span data-ttu-id="4a5a3-131">Cliquez sur **exécuter**.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-131">Click on **run**.</span></span>
3. <span data-ttu-id="4a5a3-132">Tapez **cmd**.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-132">Type **cmd**.</span></span>
4. <span data-ttu-id="4a5a3-133">Cliquez sur **OK**.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-133">Click **OK**.</span></span>  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. <span data-ttu-id="4a5a3-134">À partir de l’invite de commandes, tapez la ligne suivante pour démarrer l’installation du .NET Framework : **/c: de Dotnetfx.exe « installer /noaspupgrade ?**.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-134">From the command prompt, type the following line to start the installation of the .NET Framework: **Dotnetfx.exe /c:"install /noaspupgrade?**.</span></span>  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. <span data-ttu-id="4a5a3-135">Cliquez sur **Oui** dans le programme d’installation de Microsoft .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-135">Click **Yes** in the Microsoft .NET Framework 1.1 Setup.</span></span> <span data-ttu-id="4a5a3-136">Démarre le processus d’installation du .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-136">This will start the setup process of the .NET Framework 1.1.</span></span>  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a><span data-ttu-id="4a5a3-137">Mapper une application Web vers une version spécifique du .NET Framework</span><span class="sxs-lookup"><span data-stu-id="4a5a3-137">Map a Web application to a specific version of the .NET Framework</span></span>

<span data-ttu-id="4a5a3-138">Chaque version du .NET Framework inclut une version de l’outil ASP.NET IIS Registration (Aspnet\_regiis.exe).</span><span class="sxs-lookup"><span data-stu-id="4a5a3-138">Each version of the .NET Framework includes a version of the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe).</span></span> <span data-ttu-id="4a5a3-139">Cet outil permet aux administrateurs de spécifier qu’une application Web être exécuté sous une version particulière du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-139">This tool enables administrators to specify that a Web application be run under a particular version of the .NET Framework.</span></span> <span data-ttu-id="4a5a3-140">Cela est appelé mappage d’une application Web vers une version du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-140">This is referred to as mapping a Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="4a5a3-141">Les administrateurs doivent sélectionner le compte Aspnet\_regiis.exe qui correspond à la version du .NET Framework qui sera associé à l’application Web.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-141">Administrators must select the Aspnet\_regiis.exe that corresponds to the version of the .NET Framework that will be associated with the Web application.</span></span> <span data-ttu-id="4a5a3-142">Par exemple, un administrateur souhaite pour spécifier qu’un site Web, utilisez .NET Framework 1.1 doive utiliser le compte Aspnet\_regiis.exe qui est fourni avec .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-142">For example, an administrator who wants to specify that a Web site use .NET Framework 1.1 must use the Aspnet\_regiis.exe that comes with .NET Framework 1.1.</span></span>

<span data-ttu-id="4a5a3-143">Le compte Aspnet\_regiis.exe pour la version 1.0 est situé :</span><span class="sxs-lookup"><span data-stu-id="4a5a3-143">The Aspnet\_regiis.exe for version 1.0 is located at:</span></span>

- <span data-ttu-id="4a5a3-144">C:\Windows\Microsoft.NET\Framework\**v1.0.3705** \aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="4a5a3-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis</span></span>

<span data-ttu-id="4a5a3-145">Le compte Aspnet\_regiis.exe pour la version 1,1 se trouve dans :</span><span class="sxs-lookup"><span data-stu-id="4a5a3-145">The Aspnet\_regiis.exe for version 1,1 is located at:</span></span>

- <span data-ttu-id="4a5a3-146">C:\Windows\Microsoft.NET\Framework\**v1.1.4322** \aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="4a5a3-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis</span></span>

<span data-ttu-id="4a5a3-147">Le compte Aspnet\_regiis.exe fournit deux options pour une application Web de mappage de script :</span><span class="sxs-lookup"><span data-stu-id="4a5a3-147">The Aspnet\_regiis.exe provides two options for script mapping a Web application:</span></span>

- <span data-ttu-id="4a5a3-148">**-s** définit le mappage de script dans le chemin d’accès et de ses enfants répertoires.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-148">**-s** sets the script map in the path and in its child directories.</span></span>
- <span data-ttu-id="4a5a3-149">**-sn** définit le mappage de script dans le chemin d’accès uniquement.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-149">**-sn** sets the script map in the path only.</span></span>

<span data-ttu-id="4a5a3-150">Le chemin d’accès définit le chemin de métadonnées IIS Web application, qui est défini dans l’écran de W3SVC/ROOT / {WebSiteNumber} / {Application\_nom}.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-150">The path defines the Web application IIS metadata path, which is defined in the form of W3SVC/ROOT/{WebSiteNumber}/{Application\_Name}.</span></span> <span data-ttu-id="4a5a3-151">Par exemple, pour une application Web appelée portail situé sous le site Web par défaut, le chemin d’accès de métabase est W3SVC/1/racine/portail.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-151">For example, for a Web application called Portal located under the default Web site, the metabase path is W3SVC/1/ROOT/Portal.</span></span>

![](side-by-side-with-10/_static/image4.gif)

<span data-ttu-id="4a5a3-152">Notez que vous pouvez également utiliser un outil appelé éditeur de la métabase pour obtenir le chemin d’accès de métabase.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-152">Note You can also use a tool called the Metabase Editor to get the metabase path.</span></span> <span data-ttu-id="4a5a3-153">Vous pouvez télécharger cet outil à partir du site de Support technique de Microsoft à [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span><span class="sxs-lookup"><span data-stu-id="4a5a3-153">You can download this tool from the Microsoft Support site at [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span></span>

- <span data-ttu-id="4a5a3-154">Exécutez Aspnet\_regiis.exe -s W3SVC/1/racine/portail pour mettre à jour le portail IIS de script de carte et de ses subapplication.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-154">Run Aspnet\_regiis.exe -s W3SVC/1/ROOT/Portal to update the portal IIS script map and its subapplication.</span></span>  
  
    ![](side-by-side-with-10/_static/image5.gif)

- <span data-ttu-id="4a5a3-155">Exécutez Aspnet\_regiis.exe -sn W3SVC/1/racine/portail pour mettre à jour le script d’IIS portail mapper, sans affecter les applications dans le portail ? dans les sous-répertoires s.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-155">Run Aspnet\_regiis.exe -sn W3SVC/1/ROOT/Portal to update the portal IIS script map, without affecting applications in the portal?s subdirectories.</span></span>  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a><span data-ttu-id="4a5a3-156">Rechercher la version de .NET Framework qui utilise une application Web</span><span class="sxs-lookup"><span data-stu-id="4a5a3-156">Find the .NET Framework version that a Web application is using</span></span>

<span data-ttu-id="4a5a3-157">Un administrateur peut utiliser le Gestionnaire des services Internet pour rechercher la version du .NET Framework s’exécute à un site Web.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-157">An administrator can use the Internet Service Manager to find which version of the .NET Framework runs a Web site.</span></span> <span data-ttu-id="4a5a3-158">Versions du système d’exploitation différent lancement le Gestionnaire des services Internet différemment.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-158">Different operating system versions launch the Internet Service Manager differently.</span></span> <span data-ttu-id="4a5a3-159">Pour démarrer le Gestionnaire des services, suivez les étapes répertoriées ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-159">To start the service manager, follow the steps listed below.</span></span>

<span data-ttu-id="4a5a3-160">**Pour démarrer le Gestionnaire des services Internet**</span><span class="sxs-lookup"><span data-stu-id="4a5a3-160">**To start Internet Service Manager**</span></span>

1. <span data-ttu-id="4a5a3-161">Accédez à **Démarrer**.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-161">Go to **Start**.</span></span>
2. <span data-ttu-id="4a5a3-162">Cliquez sur **exécuter**.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-162">Click on **run**.</span></span>
3. <span data-ttu-id="4a5a3-163">Type **inetmgr**.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-163">Type **inetmgr**.</span></span>  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. <span data-ttu-id="4a5a3-164">À partir du Gestionnaire des services Internet, sélectionnez l’application Web dont vous souhaitez connaître la version du .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-164">From the Internet Service Manager, select the Web application whose version of the .NET Framework you want to know.</span></span>  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. <span data-ttu-id="4a5a3-165">Avec le bouton droit sur l’application Web, puis cliquez sur **propriétés.**</span><span class="sxs-lookup"><span data-stu-id="4a5a3-165">Right-click on the Web application, and click on **Properties.**</span></span>  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. <span data-ttu-id="4a5a3-166">Dans la fenêtre Propriétés, sélectionnez **Configuration.**</span><span class="sxs-lookup"><span data-stu-id="4a5a3-166">From the Property window, select **Configuration.**</span></span>  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. <span data-ttu-id="4a5a3-167">Dans la table de mappage d’application, sélectionnez **.aspx**, puis cliquez sur **modifier**.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-167">From the application mapping table, select **.aspx**, and click **Edit**.</span></span>  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. <span data-ttu-id="4a5a3-168">À partir de la **exécutable** zone de texte, examinez le répertoire de version faisant défiler l’écran.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-168">From the **Executable** text box, look at the version directory by scrolling.</span></span> <span data-ttu-id="4a5a3-169">Si le répertoire de version est v.1.1.4322, l’application est mappée vers la version 1.1 de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-169">If the version directory is v.1.1.4322, the application is mapped to .NET Framework 1.1.</span></span> <span data-ttu-id="4a5a3-170">À l’inverse, si le répertoire de version est v1.0.3705, l’application est mappée à .NET Framework 1.0.</span><span class="sxs-lookup"><span data-stu-id="4a5a3-170">Conversely, if the version directory is v1.0.3705, the application is mapped to .NET Framework 1.0.</span></span>  
  
    ![](side-by-side-with-10/_static/image12.gif)
