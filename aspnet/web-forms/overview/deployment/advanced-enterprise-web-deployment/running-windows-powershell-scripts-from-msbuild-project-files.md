---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: En cours d’exécution des Scripts Windows PowerShell à partir de fichiers de projet MSBuild | Microsoft Docs
author: jrjlee
description: Cette rubrique décrit comment exécuter un script Windows PowerShell en tant que partie d’un processus de génération et de déploiement. Vous pouvez exécuter un script localement (en d’autres termes, sur la b...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: 018a962c3bac774a770b83b2fd1f44f72b6f5b09
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830715"
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a><span data-ttu-id="aed63-104">En cours d’exécution des Scripts Windows PowerShell à partir de fichiers de projet MSBuild</span><span class="sxs-lookup"><span data-stu-id="aed63-104">Running Windows PowerShell Scripts from MSBuild Project Files</span></span>
====================
<span data-ttu-id="aed63-105">par [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="aed63-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="aed63-106">Télécharger PDF</span><span class="sxs-lookup"><span data-stu-id="aed63-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="aed63-107">Cette rubrique décrit comment exécuter un script Windows PowerShell en tant que partie d’un processus de génération et de déploiement.</span><span class="sxs-lookup"><span data-stu-id="aed63-107">This topic describes how to run a Windows PowerShell script as part of a build and deployment process.</span></span> <span data-ttu-id="aed63-108">Vous pouvez exécuter un script localement (en d’autres termes, sur le serveur de builds) ou à distance, comme sur un serveur de base de données ou un serveur web de destination.</span><span class="sxs-lookup"><span data-stu-id="aed63-108">You can run a script locally (in other words, on the build server) or remotely, like on a destination web server or database server.</span></span>
> 
> <span data-ttu-id="aed63-109">Il existe un grand nombre de raisons pourquoi vous souhaiterez peut-être exécuter un script Windows PowerShell de post-déploiement.</span><span class="sxs-lookup"><span data-stu-id="aed63-109">There are lots of reasons why you might want to run a post-deployment Windows PowerShell script.</span></span> <span data-ttu-id="aed63-110">Vous pouvez, par exemple, décider d'effectuer les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="aed63-110">For example, you might want to:</span></span>
> 
> - <span data-ttu-id="aed63-111">Ajouter une source de l’événement personnalisé dans le Registre.</span><span class="sxs-lookup"><span data-stu-id="aed63-111">Add a custom event source to the registry.</span></span>
> - <span data-ttu-id="aed63-112">Générer un répertoire de système de fichiers pour les téléchargements.</span><span class="sxs-lookup"><span data-stu-id="aed63-112">Generate a file system directory for uploads.</span></span>
> - <span data-ttu-id="aed63-113">Nettoyer les répertoires de build.</span><span class="sxs-lookup"><span data-stu-id="aed63-113">Clean up build directories.</span></span>
> - <span data-ttu-id="aed63-114">Écrire des entrées dans un fichier journal personnalisé.</span><span class="sxs-lookup"><span data-stu-id="aed63-114">Write entries to a custom log file.</span></span>
> - <span data-ttu-id="aed63-115">Envoyer des e-mails de l’invitation d’utilisateurs à une application web qui vient d’être approvisionné.</span><span class="sxs-lookup"><span data-stu-id="aed63-115">Send emails inviting users to a newly provisioned web application.</span></span>
> - <span data-ttu-id="aed63-116">Créer des comptes d’utilisateur avec les autorisations appropriées.</span><span class="sxs-lookup"><span data-stu-id="aed63-116">Create user accounts with the appropriate permissions.</span></span>
> - <span data-ttu-id="aed63-117">Configurer la réplication entre les instances de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="aed63-117">Configure replication between SQL Server instances.</span></span>
> 
> <span data-ttu-id="aed63-118">Cette rubrique sera vous montrent comment exécuter des scripts Windows PowerShell à la fois localement et à distance à partir d’une cible personnalisée dans un fichier de projet Microsoft Build Engine (MSBuild).</span><span class="sxs-lookup"><span data-stu-id="aed63-118">This topic will show you how to run Windows PowerShell scripts both locally and remotely from a custom target in a Microsoft Build Engine (MSBuild) project file.</span></span>


<span data-ttu-id="aed63-119">Cette rubrique fait partie d’une série de didacticiels basées sur les exigences de déploiement d’entreprise de la société fictive Fabrikam, Inc. Cette série de didacticiels utilise un exemple de solution&#x2014;le [solution Gestionnaire de contacts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;pour représenter une application web avec un niveau réaliste de complexité, y compris une application ASP.NET MVC 3, une Communication de Windows Foundation (WCF) service et un projet de base de données.</span><span class="sxs-lookup"><span data-stu-id="aed63-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="aed63-120">La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet de fractionnement décrite dans [présentation du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md), dans lequel le processus de génération est contrôlé par deux fichiers de projet&#x2014;contenant un seul les instructions qui s’appliquent à chaque environnement de destination et celui qui contient les paramètres de génération et de déploiement spécifiques à l’environnement de génération.</span><span class="sxs-lookup"><span data-stu-id="aed63-120">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="aed63-121">Au moment de la génération, le fichier de projet spécifique à l’environnement est fusionné dans le fichier de projet d’indépendant de l’environnement pour former un ensemble complet d’instructions de génération.</span><span class="sxs-lookup"><span data-stu-id="aed63-121">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="aed63-122">Présentation de la tâche</span><span class="sxs-lookup"><span data-stu-id="aed63-122">Task Overview</span></span>

<span data-ttu-id="aed63-123">Pour exécuter un script Windows PowerShell en tant que partie d’un processus de déploiement automatisé ou pas à pas, vous devez effectuer ces tâches de haut niveau :</span><span class="sxs-lookup"><span data-stu-id="aed63-123">To run a Windows PowerShell script as part of an automated or single-step deployment process, you'll need to complete these high-level tasks:</span></span>

- <span data-ttu-id="aed63-124">Ajouter le script Windows PowerShell à votre solution et au contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="aed63-124">Add the Windows PowerShell script to your solution and to source control.</span></span>
- <span data-ttu-id="aed63-125">Créer une commande qui appelle votre script Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="aed63-125">Create a command that invokes your Windows PowerShell script.</span></span>
- <span data-ttu-id="aed63-126">Séquence d’échappement des caractères XML réservés dans votre commande.</span><span class="sxs-lookup"><span data-stu-id="aed63-126">Escape any reserved XML characters in your command.</span></span>
- <span data-ttu-id="aed63-127">Créer une cible dans votre fichier de projet MSBuild personnalisé et utiliser le **Exec** tâche pour exécuter votre commande.</span><span class="sxs-lookup"><span data-stu-id="aed63-127">Create a target in your custom MSBuild project file and use the **Exec** task to run your command.</span></span>

<span data-ttu-id="aed63-128">Cette rubrique sera vous montrent comment effectuer ces procédures.</span><span class="sxs-lookup"><span data-stu-id="aed63-128">This topic will show you how to perform these procedures.</span></span> <span data-ttu-id="aed63-129">Les tâches et les procédures pas à pas dans cette rubrique supposent que vous êtes déjà familiarisé avec les propriétés et cibles MSBuild, et que vous comprenez comment utiliser un fichier de projet MSBuild personnalisé pour piloter un processus de génération et de déploiement.</span><span class="sxs-lookup"><span data-stu-id="aed63-129">The tasks and walkthroughs in this topic assume that you're already familiar with MSBuild targets and properties, and that you understand how to use a custom MSBuild project file to drive a build and deployment process.</span></span> <span data-ttu-id="aed63-130">Pour plus d’informations, consultez [présentation du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md) et [comprendre le processus de génération](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="aed63-130">For more information, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

## <a name="creating-and-adding-windows-powershell-scripts"></a><span data-ttu-id="aed63-131">Création et l’ajout de Scripts de Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="aed63-131">Creating and Adding Windows PowerShell Scripts</span></span>

<span data-ttu-id="aed63-132">Les tâches de cette rubrique utilisent un exemple de script Windows PowerShell nommé **LogDeploy.ps1** pour illustrer comment exécuter des scripts à partir de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="aed63-132">The tasks in this topic use a sample Windows PowerShell script named **LogDeploy.ps1** to illustrate how to run scripts from MSBuild.</span></span> <span data-ttu-id="aed63-133">Le **LogDeploy.ps1** script contient une fonction simple qui écrit une entrée à ligne unique dans un fichier journal :</span><span class="sxs-lookup"><span data-stu-id="aed63-133">The **LogDeploy.ps1** script contains a simple function that writes a single-line entry to a log file:</span></span>


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.ps1)]


<span data-ttu-id="aed63-134">Le **LogDeploy.ps1** script accepte deux paramètres.</span><span class="sxs-lookup"><span data-stu-id="aed63-134">The **LogDeploy.ps1** script accepts two parameters.</span></span> <span data-ttu-id="aed63-135">Le premier paramètre représente le chemin d’accès complet au fichier journal à laquelle vous souhaitez ajouter une entrée, et le deuxième paramètre représente la destination du déploiement que vous souhaitez enregistrer dans le fichier journal.</span><span class="sxs-lookup"><span data-stu-id="aed63-135">The first parameter represents the full path to the log file to which you want to add an entry, and the second parameter represents the deployment destination that you want to record in the log file.</span></span> <span data-ttu-id="aed63-136">Lorsque vous exécutez le script, il ajoute une ligne dans le fichier journal au format suivant :</span><span class="sxs-lookup"><span data-stu-id="aed63-136">When you run the script, it adds a line to the log file in this format:</span></span>


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


<span data-ttu-id="aed63-137">Pour rendre le **LogDeploy.ps1** script disponible à MSBuild, vous devez :</span><span class="sxs-lookup"><span data-stu-id="aed63-137">To make the **LogDeploy.ps1** script available to MSBuild, you need to:</span></span>

- <span data-ttu-id="aed63-138">Ajoutez le script pour le contrôle de code source.</span><span class="sxs-lookup"><span data-stu-id="aed63-138">Add the script to source control.</span></span>
- <span data-ttu-id="aed63-139">Ajoutez le script à votre solution dans Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="aed63-139">Add the script to your solution in Visual Studio 2010.</span></span>

<span data-ttu-id="aed63-140">Vous n’avez pas besoin de déployer le script avec votre contenu de la solution, indépendamment de si vous projetez d’exécuter le script sur le serveur de builds ou sur un ordinateur distant.</span><span class="sxs-lookup"><span data-stu-id="aed63-140">You don't need to deploy the script with your solution content, regardless of whether you plan to run the script on the build server or on a remote computer.</span></span> <span data-ttu-id="aed63-141">Une option consiste à ajouter le script à un dossier de solution.</span><span class="sxs-lookup"><span data-stu-id="aed63-141">One option is to add the script to a solution folder.</span></span> <span data-ttu-id="aed63-142">Dans l’exemple de gestionnaire de contacts, étant donné que vous souhaitez utiliser le script Windows PowerShell en tant que partie du processus de déploiement, il est judicieux d’ajouter le script dans le dossier de solution de publication.</span><span class="sxs-lookup"><span data-stu-id="aed63-142">In the Contact Manager example, because you want to use the Windows PowerShell script as part of the deployment process, it makes sense to add the script to the Publish solution folder.</span></span>

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

<span data-ttu-id="aed63-143">Le contenu des dossiers de solution est copié pour créer des serveurs sources.</span><span class="sxs-lookup"><span data-stu-id="aed63-143">The contents of solution folders are copied to build servers as source material.</span></span> <span data-ttu-id="aed63-144">Toutefois, ils ne forment aucune partie de la sortie du projet.</span><span class="sxs-lookup"><span data-stu-id="aed63-144">However, they form no part of any project output.</span></span>

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a><span data-ttu-id="aed63-145">L’exécution d’un Script PowerShell de Windows sur le serveur de builds</span><span class="sxs-lookup"><span data-stu-id="aed63-145">Executing a Windows PowerShell Script on the Build Server</span></span>

<span data-ttu-id="aed63-146">Dans certains scénarios, vous souhaiterez exécuter des scripts Windows PowerShell sur l’ordinateur qui génère de vos projets.</span><span class="sxs-lookup"><span data-stu-id="aed63-146">In some scenarios, you may want to run Windows PowerShell scripts on the computer that builds your projects.</span></span> <span data-ttu-id="aed63-147">Par exemple, vous pouvez utiliser un script Windows PowerShell pour nettoyer les dossiers de build ou d’écrire des entrées dans un fichier journal personnalisé.</span><span class="sxs-lookup"><span data-stu-id="aed63-147">For example, you might use a Windows PowerShell script to clean up build folders or write entries to a custom log file.</span></span>

<span data-ttu-id="aed63-148">En termes de syntaxe, un script Windows PowerShell en cours d’exécution à partir d’un fichier projet MSBuild est identique à un script Windows PowerShell en cours d’exécution à partir d’une invite de commandes standard.</span><span class="sxs-lookup"><span data-stu-id="aed63-148">In terms of syntax, running a Windows PowerShell script from an MSBuild project file is the same as running a Windows PowerShell script from a regular command prompt.</span></span> <span data-ttu-id="aed63-149">Vous devez appeler l’exécutable de powershell.exe et utiliser le **– commande** commutateur pour fournir les commandes PowerShell de Windows pour l’exécution.</span><span class="sxs-lookup"><span data-stu-id="aed63-149">You need to invoke the powershell.exe executable and use the **–command** switch to provide the commands you want Windows PowerShell to run.</span></span> <span data-ttu-id="aed63-150">(Dans Windows PowerShell v2, vous pouvez également utiliser le **– fichier** basculer).</span><span class="sxs-lookup"><span data-stu-id="aed63-150">(In Windows PowerShell v2, you can also use the **–file** switch).</span></span> <span data-ttu-id="aed63-151">La commande doit prendre ce format :</span><span class="sxs-lookup"><span data-stu-id="aed63-151">The command should take this format:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


<span data-ttu-id="aed63-152">Exemple :</span><span class="sxs-lookup"><span data-stu-id="aed63-152">For example:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


<span data-ttu-id="aed63-153">Si le chemin d’accès à votre script inclut des espaces, vous devez placer le chemin d’accès de fichier entre apostrophes précédés par une esperluette.</span><span class="sxs-lookup"><span data-stu-id="aed63-153">If the path to your script includes spaces, you need to enclose the file path in single quotes preceded by an ampersand.</span></span> <span data-ttu-id="aed63-154">Vous ne pouvez pas utiliser des guillemets doubles, étant donné que vous avez déjà utilisé pour encadrer la commande :</span><span class="sxs-lookup"><span data-stu-id="aed63-154">You can't use double quotes, because you've already used them to enclose the command:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


<span data-ttu-id="aed63-155">Il existe quelques considérations supplémentaires lorsque vous appelez cette commande à partir de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="aed63-155">There are a few additional considerations when you invoke this command from MSBuild.</span></span> <span data-ttu-id="aed63-156">Tout d’abord, vous devez inclure le **– NonInteractive** indicateur pour vous assurer que le script s’exécute en mode silencieux.</span><span class="sxs-lookup"><span data-stu-id="aed63-156">First, you should include the **–NonInteractive** flag to ensure that the script executes quietly.</span></span> <span data-ttu-id="aed63-157">Ensuite, vous devez inclure le **– ExecutionPolicy** indicateur avec une valeur d’argument approprié.</span><span class="sxs-lookup"><span data-stu-id="aed63-157">Next, you should include the **–ExecutionPolicy** flag with an appropriate argument value.</span></span> <span data-ttu-id="aed63-158">Spécifie la stratégie d’exécution que Windows PowerShell s’appliquent à votre script et vous permet de substituer la stratégie d’exécution par défaut, ce qui peut empêcher l’exécution de votre script.</span><span class="sxs-lookup"><span data-stu-id="aed63-158">This specifies the execution policy that Windows PowerShell will apply to your script and allows you to override the default execution policy, which may prevent execution of your script.</span></span> <span data-ttu-id="aed63-159">Vous pouvez choisir parmi ces valeurs d’argument :</span><span class="sxs-lookup"><span data-stu-id="aed63-159">You can choose from these argument values:</span></span>

- <span data-ttu-id="aed63-160">La valeur **Unrestricted** permettra PowerShell de Windows exécuter votre script, indépendamment de si le script est signé.</span><span class="sxs-lookup"><span data-stu-id="aed63-160">A value of **Unrestricted** will allow Windows PowerShell to execute your script, regardless of whether the script is signed.</span></span>
- <span data-ttu-id="aed63-161">La valeur **RemoteSigned** permettra à Windows PowerShell exécute les scripts non signés qui ont été créés sur l’ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="aed63-161">A value of **RemoteSigned** will allow Windows PowerShell to execute unsigned scripts that were created on the local machine.</span></span> <span data-ttu-id="aed63-162">Toutefois, les scripts qui ont été créés ailleurs doivent être signés.</span><span class="sxs-lookup"><span data-stu-id="aed63-162">However, scripts that were created elsewhere must be signed.</span></span> <span data-ttu-id="aed63-163">(Dans la pratique, vous êtes peu probable que vous avez créé un script Windows PowerShell localement sur un serveur de builds).</span><span class="sxs-lookup"><span data-stu-id="aed63-163">(In practice, you're very unlikely to have created a Windows PowerShell script locally on a build server).</span></span>
- <span data-ttu-id="aed63-164">La valeur **AllSigned** permettra à Windows PowerShell exécute les scripts signés uniquement.</span><span class="sxs-lookup"><span data-stu-id="aed63-164">A value of **AllSigned** will allow Windows PowerShell to execute signed scripts only.</span></span>

<span data-ttu-id="aed63-165">La stratégie d’exécution par défaut est **Restricted**, ce qui empêche Windows PowerShell à partir des fichiers de script en cours d’exécution.</span><span class="sxs-lookup"><span data-stu-id="aed63-165">The default execution policy is **Restricted**, which prevents Windows PowerShell from running any script files.</span></span>

<span data-ttu-id="aed63-166">Enfin, vous devez échapper des caractères XML réservés qui se produisent dans votre commande de Windows PowerShell :</span><span class="sxs-lookup"><span data-stu-id="aed63-166">Finally, you need to escape any reserved XML characters that occur in your Windows PowerShell command:</span></span>

- <span data-ttu-id="aed63-167">Remplacez les guillemets simples avec  **&amp;apos ;**</span><span class="sxs-lookup"><span data-stu-id="aed63-167">Replace single quotes with **&amp;apos;**</span></span>
- <span data-ttu-id="aed63-168">Remplacez les guillemets doubles avec  **&amp;quot ;**</span><span class="sxs-lookup"><span data-stu-id="aed63-168">Replace double quotes with **&amp;quot;**</span></span>
- <span data-ttu-id="aed63-169">Remplacez les symboles avec  **&amp;amp ;**</span><span class="sxs-lookup"><span data-stu-id="aed63-169">Replace ampersands with **&amp;amp;**</span></span>

- <span data-ttu-id="aed63-170">Lorsque vous apportez ces modifications, votre commande doit ressembler à ceci :</span><span class="sxs-lookup"><span data-stu-id="aed63-170">When you make these changes, your command will resemble this:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


<span data-ttu-id="aed63-171">Dans votre fichier projet MSBuild personnalisé, vous pouvez créer une nouvelle cible et utiliser le **Exec** tâche pour exécuter cette commande :</span><span class="sxs-lookup"><span data-stu-id="aed63-171">Within your custom MSBuild project file, you can create a new target and use the **Exec** task to run this command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


<span data-ttu-id="aed63-172">Dans cet exemple, notez que :</span><span class="sxs-lookup"><span data-stu-id="aed63-172">In this example, note that:</span></span>

- <span data-ttu-id="aed63-173">Les variables, telles que les valeurs de paramètre et l’emplacement de l’exécutable de Windows PowerShell, sont déclarées en tant que propriétés de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="aed63-173">Any variables, like parameter values and the location of the Windows PowerShell executable, are declared as MSBuild properties.</span></span>
- <span data-ttu-id="aed63-174">Conditions sont incluses pour permettre aux utilisateurs de remplacer ces valeurs à partir de la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="aed63-174">Conditions are included to enable users to override these values from the command line.</span></span>
- <span data-ttu-id="aed63-175">Le **MSDeployComputerName** propriété déclarée ailleurs dans le fichier projet.</span><span class="sxs-lookup"><span data-stu-id="aed63-175">The **MSDeployComputerName** property is declared elsewhere in the project file.</span></span>

<span data-ttu-id="aed63-176">Lorsque vous exécutez cette cible dans le cadre de votre processus de génération, Windows PowerShell exécute votre commande et écrire une entrée de journal dans le fichier que vous avez spécifié.</span><span class="sxs-lookup"><span data-stu-id="aed63-176">When you execute this target as part of your build process, Windows PowerShell will run your command and write a log entry to the file you specified.</span></span>

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a><span data-ttu-id="aed63-177">L’exécution d’un Script PowerShell de Windows sur un ordinateur distant</span><span class="sxs-lookup"><span data-stu-id="aed63-177">Executing a Windows PowerShell Script on a Remote Computer</span></span>

<span data-ttu-id="aed63-178">Windows PowerShell est capable d’exécuter des scripts sur des ordinateurs distants via [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span><span class="sxs-lookup"><span data-stu-id="aed63-178">Windows PowerShell is capable of running scripts on remote computers through [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span></span> <span data-ttu-id="aed63-179">Pour ce faire, vous devez utiliser le [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) applet de commande.</span><span class="sxs-lookup"><span data-stu-id="aed63-179">To do this, you need to use the [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet.</span></span> <span data-ttu-id="aed63-180">Cela vous permet d’exécuter votre script sur un ou plusieurs ordinateurs distants sans copier le script sur les ordinateurs distants.</span><span class="sxs-lookup"><span data-stu-id="aed63-180">This lets you execute your script against one or more remote computers without copying the script to the remote computers.</span></span> <span data-ttu-id="aed63-181">Tous les résultats sont retournés à l’ordinateur local à partir duquel vous avez exécuté le script.</span><span class="sxs-lookup"><span data-stu-id="aed63-181">Any results are returned to the local computer from which you ran the script.</span></span>

> [!NOTE]
> <span data-ttu-id="aed63-182">Avant d’utiliser le **Invoke-Command** scripts de l’applet de commande à exécuter Windows PowerShell sur un ordinateur distant, vous devez configurer un écouteur WinRM pour accepter les messages à distance.</span><span class="sxs-lookup"><span data-stu-id="aed63-182">Before you use the **Invoke-Command** cmdlet to execute Windows PowerShell scripts on a remote computer, you need to configure a WinRM listener to accept remote messages.</span></span> <span data-ttu-id="aed63-183">Vous pouvez le faire en exécutant la commande **winrm quickconfig** sur l’ordinateur distant.</span><span class="sxs-lookup"><span data-stu-id="aed63-183">You can do this by running the command **winrm quickconfig** on the remote computer.</span></span> <span data-ttu-id="aed63-184">Pour plus d’informations, consultez [Installation et Configuration pour Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="aed63-184">For more information, see [Installation and Configuration for Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span></span>


<span data-ttu-id="aed63-185">À partir d’une fenêtre Windows PowerShell, vous devez utiliser cette syntaxe pour exécuter le **LogDeploy.ps1** script sur un ordinateur distant :</span><span class="sxs-lookup"><span data-stu-id="aed63-185">From a Windows PowerShell window, you'd use this syntax to run the **LogDeploy.ps1** script on a remote computer:</span></span>


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> <span data-ttu-id="aed63-186">Il existe diverses autres méthodes d’utilisation **Invoke-Command** pour exécuter un script de fichier, mais cette approche est plus simple que vous deviez fournir des valeurs de paramètre et de gérer les chemins d’accès avec des espaces.</span><span class="sxs-lookup"><span data-stu-id="aed63-186">There are various other ways of using **Invoke-Command** to run a script file, but this approach is the most straightforward when you need to provide parameter values and manage paths with spaces.</span></span>


<span data-ttu-id="aed63-187">Lorsque vous exécutez ceci à partir d’une invite de commandes, vous devez appeler l’exécutable Windows PowerShell et utiliser le **– commande** paramètre pour fournir vos instructions :</span><span class="sxs-lookup"><span data-stu-id="aed63-187">When you run this from a command prompt, you need to invoke the Windows PowerShell executable and use the **–command** parameter to provide your instructions:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


<span data-ttu-id="aed63-188">Comme avant, vous devez fournir certains commutateurs supplémentaires et d’échappement des caractères XML réservés lorsque vous exécutez la commande à partir de MSBuild :</span><span class="sxs-lookup"><span data-stu-id="aed63-188">As before, you need to provide some additional switches and escape any reserved XML characters when you run the command from MSBuild:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


<span data-ttu-id="aed63-189">Enfin, comme auparavant, vous pouvez utiliser la **Exec** tâche dans une cible MSBuild personnalisée pour exécuter votre commande :</span><span class="sxs-lookup"><span data-stu-id="aed63-189">Finally, as before, you can use the **Exec** task within a custom MSBuild target to execute your command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


<span data-ttu-id="aed63-190">Lorsque vous exécutez cette cible dans le cadre de votre processus de génération, Windows PowerShell exécute votre script sur l’ordinateur que vous avez spécifié dans le **– computername** argument.</span><span class="sxs-lookup"><span data-stu-id="aed63-190">When you execute this target as part of your build process, Windows PowerShell will run your script on the computer you specified in the **–computername** argument.</span></span>

## <a name="conclusion"></a><span data-ttu-id="aed63-191">Conclusion</span><span class="sxs-lookup"><span data-stu-id="aed63-191">Conclusion</span></span>

<span data-ttu-id="aed63-192">Cette rubrique décrit comment exécuter un script Windows PowerShell à partir d’un fichier projet MSBuild.</span><span class="sxs-lookup"><span data-stu-id="aed63-192">This topic described how to run a Windows PowerShell script from an MSBuild project file.</span></span> <span data-ttu-id="aed63-193">Vous pouvez utiliser cette approche pour exécuter un script Windows PowerShell, localement ou sur un ordinateur distant, dans le cadre d’un processus de génération et de déploiement automatisé ou étape unique.</span><span class="sxs-lookup"><span data-stu-id="aed63-193">You can use this approach to run a Windows PowerShell script, either locally or on a remote computer, as part of an automated or single-step build and deployment process.</span></span>

## <a name="further-reading"></a><span data-ttu-id="aed63-194">informations supplémentaires</span><span class="sxs-lookup"><span data-stu-id="aed63-194">Further Reading</span></span>

<span data-ttu-id="aed63-195">Pour obtenir des conseils sur l’inscription des scripts Windows PowerShell et la gestion des stratégies d’exécution, consultez [exécution de Scripts Windows PowerShell](https://technet.microsoft.com/library/ee176949.aspx).</span><span class="sxs-lookup"><span data-stu-id="aed63-195">For guidance on signing Windows PowerShell scripts and managing execution policies, see [Running Windows PowerShell Scripts](https://technet.microsoft.com/library/ee176949.aspx).</span></span> <span data-ttu-id="aed63-196">Pour obtenir des conseils sur l’exécution de commandes Windows PowerShell à partir d’un ordinateur distant, consultez [en cours d’exécution des commandes à distance](https://technet.microsoft.com/library/dd819505.aspx).</span><span class="sxs-lookup"><span data-stu-id="aed63-196">For guidance on running Windows PowerShell commands from a remote computer, see [Running Remote Commands](https://technet.microsoft.com/library/dd819505.aspx).</span></span>

<span data-ttu-id="aed63-197">Pour plus d’informations sur l’utilisation de fichiers de projet MSBuild personnalisées pour contrôler le processus de déploiement, consultez [présentation du fichier projet](../web-deployment-in-the-enterprise/understanding-the-project-file.md) et [comprendre le processus de génération](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="aed63-197">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="aed63-198">[Précédent](taking-web-applications-offline-with-web-deploy.md)
> [Suivant](troubleshooting-the-packaging-process.md)</span><span class="sxs-lookup"><span data-stu-id="aed63-198">[Previous](taking-web-applications-offline-with-web-deploy.md)
[Next](troubleshooting-the-packaging-process.md)</span></span>
