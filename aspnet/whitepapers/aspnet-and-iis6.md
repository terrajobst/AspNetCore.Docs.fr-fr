---
uid: whitepapers/aspnet-and-iis6
title: Exécution d’ASP.NET 1.1 avec IIS 6.0 | Microsoft Docs
author: rick-anderson
description: Tandis que Windows Server 2003 inclut IIS 6.0 et ASP.NET 1.1, ces composants sont désactivés par défaut. Ce livre blanc décrit comment activer IIS 6.0 un...
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 338059104f46a6c5517212db6e1a54c12677e133
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839292"
---
<a name="running-aspnet-11-with-iis-60"></a><span data-ttu-id="dd940-104">En cours d’exécution ASP.NET 1.1 avec IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="dd940-104">Running ASP.NET 1.1 with IIS 6.0</span></span>
====================
> <span data-ttu-id="dd940-105">Tandis que Windows Server 2003 inclut IIS 6.0 et ASP.NET 1.1, ces composants sont désactivés par défaut.</span><span class="sxs-lookup"><span data-stu-id="dd940-105">While Windows Server 2003 includes both IIS 6.0 and ASP.NET 1.1, these components are disabled by default.</span></span> <span data-ttu-id="dd940-106">Ce livre blanc décrit comment activer IIS 6.0 et ASP.NET 1.1 et vous recommande de plusieurs paramètres de configuration pour obtenir des performances optimales à partir d’IIS et ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="dd940-106">This whitepaper describes how to enable IIS 6.0 and ASP.NET 1.1, and recommends several configuration settings to get the optimal performance from IIS and ASP.NET.</span></span>
> 
> <span data-ttu-id="dd940-107">S’applique à IIS 6.0 et ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="dd940-107">Applies to ASP.NET 1.1 and IIS 6.0.</span></span>


<span data-ttu-id="dd940-108">ASP.NET 1.1 est livré avec Windows Server 2003, qui inclut également la dernière version d’Internet Information Server (IIS) version 6.0.</span><span class="sxs-lookup"><span data-stu-id="dd940-108">ASP.NET 1.1 ships with Windows Server 2003, which also includes the latest version of Internet Information Server (IIS) version 6.0.</span></span> <span data-ttu-id="dd940-109">IIS 6.0 et ASP.NET 1.1 sont conçus pour s’intégrer parfaitement et ASP.NET désormais par défaut est le nouveau modèle de processus de travail IIS 6.0.</span><span class="sxs-lookup"><span data-stu-id="dd940-109">IIS 6.0 and ASP.NET 1.1 are designed to integrate seamlessly and ASP.NET now defaults to the new IIS 6.0 worker process model.</span></span>

## <a name="aspnet-11-is-not-installed-by-default"></a><span data-ttu-id="dd940-110">ASP.NET 1.1 n’est pas installé par défaut</span><span class="sxs-lookup"><span data-stu-id="dd940-110">ASP.NET 1.1 is not installed by default</span></span>

<span data-ttu-id="dd940-111">Contrairement aux versions précédentes des systèmes d’exploitation de serveur de Microsoft, Internet Information Server (IIS) n’est pas activée par défaut ; pas plus que ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="dd940-111">Unlike previous versions of Microsoft's server operating systems, Internet Information Server (IIS) is not enabled by default; nor is ASP.NET 1.1.</span></span> <span data-ttu-id="dd940-112">Il existe deux options pour l’activation d’IIS :</span><span class="sxs-lookup"><span data-stu-id="dd940-112">There are two options for enabling IIS:</span></span>

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a><span data-ttu-id="dd940-113">L’activation d’IIS, option #1 - Assistant Configurer votre serveur</span><span class="sxs-lookup"><span data-stu-id="dd940-113">Enabling IIS, option #1 - Configure Your Server Wizard</span></span>

<span data-ttu-id="dd940-114">Windows Server 2003 est livré à un nouveau « Assistant Configurer votre serveur » pour vous aider à configurer correctement votre serveur dans le mode souhaité.</span><span class="sxs-lookup"><span data-stu-id="dd940-114">Windows Server 2003 ships a new 'Configure Your Server Wizard' to help you properly configure your server in the desired mode.</span></span>

<span data-ttu-id="dd940-115">Pour démarrer l’Assistant - Notez que pour exécuter l’Assistant, vous devez être connecté en tant qu’administrateur - accédez à : Démarrer | Programmes | Outils d’administration et sélectionnez « Configurer votre serveur ».</span><span class="sxs-lookup"><span data-stu-id="dd940-115">To start the wizard - note, to run the wizard you must be logged in as an administrator - go to: Start | Programs | Administrative Tools and select 'Configure Your Server'.</span></span>

<span data-ttu-id="dd940-116">Une fois la sélection effectuée, vous devez voir l’écran d’ouverture de « Assistant Configurer votre serveur » :</span><span class="sxs-lookup"><span data-stu-id="dd940-116">Once selected you should see the 'Configure Your Server Wizard' opening screen:</span></span>

![](aspnet-and-iis6/_static/image1.jpg)

<span data-ttu-id="dd940-117">Cliquez sur « Suivant &gt;» :</span><span class="sxs-lookup"><span data-stu-id="dd940-117">Click 'Next &gt;':</span></span>

![](aspnet-and-iis6/_static/image2.jpg)

<span data-ttu-id="dd940-118">Cliquez sur « Suivant &gt;'</span><span class="sxs-lookup"><span data-stu-id="dd940-118">Click 'Next &gt;'</span></span>

![](aspnet-and-iis6/_static/image3.jpg)

<span data-ttu-id="dd940-119">Dans cet écran, vous devez sélectionner « serveur d’applications (IIS, ASP.NET) en tant que les options de configuration.</span><span class="sxs-lookup"><span data-stu-id="dd940-119">On this screen you will need to select 'Application server (IIS, ASP.NET) as the options to configure.</span></span>

<span data-ttu-id="dd940-120">Cliquez sur « Suivant &gt;».</span><span class="sxs-lookup"><span data-stu-id="dd940-120">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image4.jpg)

<span data-ttu-id="dd940-121">Après avoir sélectionné cette option pour configurer le serveur comme serveur d’applications, cet écran s’affichera invitant les fonctionnalités supplémentaires doivent être installées.</span><span class="sxs-lookup"><span data-stu-id="dd940-121">After selecting to configure the server as an Application Server, this screen will be displayed prompting what additional capabilities should be installed.</span></span> <span data-ttu-id="dd940-122">Aucune de ces options est sélectionné par défaut.</span><span class="sxs-lookup"><span data-stu-id="dd940-122">Neither option is selected by default.</span></span> <span data-ttu-id="dd940-123">Pour activer ASP.NET automatiquement, vous devez sélectionner « ASP activer. NET ».</span><span class="sxs-lookup"><span data-stu-id="dd940-123">To enable ASP.NET automatically, you need to select 'Enable ASP.NET'.</span></span>

<span data-ttu-id="dd940-124">Cliquez sur « Suivant &gt;».</span><span class="sxs-lookup"><span data-stu-id="dd940-124">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image5.jpg)

<span data-ttu-id="dd940-125">Cet écran affiche les options doivent être installés.</span><span class="sxs-lookup"><span data-stu-id="dd940-125">This screen displays what options are to be installed.</span></span>

<span data-ttu-id="dd940-126">Cliquez sur « Suivant &gt;».</span><span class="sxs-lookup"><span data-stu-id="dd940-126">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image6.jpg)

<span data-ttu-id="dd940-127">Vous verrez cet écran pendant que vous avez sélectionnées sont en cours d’installation.</span><span class="sxs-lookup"><span data-stu-id="dd940-127">You will see this screen while the options you selected are being installed.</span></span> <span data-ttu-id="dd940-128">Il est normal que l’autre boîte de dialogue apparaît comme services sont en cours d’installation.</span><span class="sxs-lookup"><span data-stu-id="dd940-128">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="dd940-129">En outre, vous pouvez être invité pour l’emplacement du CD d’installation de Windows 2003 Server.</span><span class="sxs-lookup"><span data-stu-id="dd940-129">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="dd940-130">Cliquez sur « Suivant &gt;» lorsque vous avez terminé.</span><span class="sxs-lookup"><span data-stu-id="dd940-130">Click 'Next &gt;' when complete.</span></span>

![](aspnet-and-iis6/_static/image7.jpg)

<span data-ttu-id="dd940-131">Cliquez sur « Terminer » - Windows Server 2003 est désormais configuré pour prendre en charge IIS 6.0 et ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="dd940-131">Click 'Finish' - the Windows Server 2003 is now configured to support IIS 6.0 and ASP.NET 1.1.</span></span>

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a><span data-ttu-id="dd940-132">L’activation d’IIS, option #2 : configurer manuellement IIS et ASP.NET</span><span class="sxs-lookup"><span data-stu-id="dd940-132">Enabling IIS, option #2 - Manually configuring IIS and ASP.NET</span></span>

<span data-ttu-id="dd940-133">Si vous ne souhaitez pas utiliser le « Assistant Configurer votre serveur », vous pouvez éventuellement installer IIS 6.0 et ASP.NET 1.1 à l’aide de 'Ajout / Suppression de programmes' à partir du panneau.</span><span class="sxs-lookup"><span data-stu-id="dd940-133">If you do not wish to use the 'Configure Your Server Wizard' you can optionally install IIS 6.0 and ASP.NET 1.1 using 'Add or Remove Programs' from the Control Panel.</span></span>

<span data-ttu-id="dd940-134">Tout d’abord ouvrir le panneau de configuration :</span><span class="sxs-lookup"><span data-stu-id="dd940-134">First open the Control Panel:</span></span>

![](aspnet-and-iis6/_static/image8.jpg)

<span data-ttu-id="dd940-135">Cliquez ensuite sur « Ajouter/supprimer Windows composants » afin d’ouvrir l’Assistant Composants de Windows :</span><span class="sxs-lookup"><span data-stu-id="dd940-135">Next, click on 'Add/Remove Windows Components' which will open the 'Windows Components Wizard':</span></span>

![](aspnet-and-iis6/_static/image9.jpg)

<span data-ttu-id="dd940-136">Mettez en surbrillance et « Serveur d’applications », puis cliquez sur « Détails » ?</span><span class="sxs-lookup"><span data-stu-id="dd940-136">Highlight and check 'Application Server' and then click the 'Details?'</span></span> <span data-ttu-id="dd940-137">Bouton :</span><span class="sxs-lookup"><span data-stu-id="dd940-137">button:</span></span>

![](aspnet-and-iis6/_static/image10.jpg)

<span data-ttu-id="dd940-138">Pour installer ASP.NET, consultez « ASP. NET ».</span><span class="sxs-lookup"><span data-stu-id="dd940-138">To install ASP.NET, check 'ASP.NET'.</span></span>

<span data-ttu-id="dd940-139">Cliquez sur « OK » pour revenir à l’Assistant Composants de Windows.</span><span class="sxs-lookup"><span data-stu-id="dd940-139">Click 'OK' to return to the Windows Component Wizard.</span></span> <span data-ttu-id="dd940-140">Cliquez sur « Suivant &gt;' à partir de l’Assistant Composants de Windows pour commencer l’installation :</span><span class="sxs-lookup"><span data-stu-id="dd940-140">Click 'Next &gt;' from the Windows Component Wizard to begin installing:</span></span>

![](aspnet-and-iis6/_static/image11.jpg)

<span data-ttu-id="dd940-141">Il est normal que l’autre boîte de dialogue apparaît comme services sont en cours d’installation.</span><span class="sxs-lookup"><span data-stu-id="dd940-141">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="dd940-142">En outre, vous pouvez être invité pour l’emplacement du CD d’installation de Windows 2003 Server.</span><span class="sxs-lookup"><span data-stu-id="dd940-142">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="dd940-143">Lors de l’installation est terminée, vous verrez le dernier écran de l’Assistant Composants de Windows :</span><span class="sxs-lookup"><span data-stu-id="dd940-143">When installation is complete you will see the last screen of the Windows Component Wizard:</span></span>

![](aspnet-and-iis6/_static/image12.jpg)

<span data-ttu-id="dd940-144">IIS 6.0 et ASP.NET 1.1 sont maintenant configurés et disponibles.</span><span class="sxs-lookup"><span data-stu-id="dd940-144">IIS 6.0 and ASP.NET 1.1 are now configured and available.</span></span>

## <a name="recommended-settings"></a><span data-ttu-id="dd940-145">Paramètres recommandés</span><span class="sxs-lookup"><span data-stu-id="dd940-145">Recommended Settings</span></span>

<span data-ttu-id="dd940-146">Lors de l’exécution d’ASP.NET 1.1 avec IIS 6.0, il existe plusieurs paramètres de configuration qui sont recommandés pour obtenir des performances optimales à partir d’ASP.NET :</span><span class="sxs-lookup"><span data-stu-id="dd940-146">When running ASP.NET 1.1 with IIS 6.0 there are several configuration settings that are recommended to get the optimal performance from ASP.NET:</span></span>

- <span data-ttu-id="dd940-147">Configuration limites de mémoire de processus de travail</span><span class="sxs-lookup"><span data-stu-id="dd940-147">Configuring worker process memory limits</span></span>
- <span data-ttu-id="dd940-148">Configuration de recyclage de processus de travail</span><span class="sxs-lookup"><span data-stu-id="dd940-148">Configuring worker process recycling</span></span>

### <a name="configuring-worker-process-memory-limits"></a><span data-ttu-id="dd940-149">Configuration limites de mémoire de processus de travail</span><span class="sxs-lookup"><span data-stu-id="dd940-149">Configuring worker process memory limits</span></span>

<span data-ttu-id="dd940-150">Par défaut, IIS 6.0 ne définit pas une limite sur la quantité de mémoire que IIS est autorisé à utiliser.</span><span class="sxs-lookup"><span data-stu-id="dd940-150">By default IIS 6.0 does not set a limit on the amount of memory that IIS is allowed to use.</span></span> <span data-ttu-id="dd940-151">ASP. Fonctionnalité de Cache de NET s’appuie sur une limitation de mémoire pour le Cache peut supprimer de manière proactive les éléments inutilisés de la mémoire.</span><span class="sxs-lookup"><span data-stu-id="dd940-151">ASP.NET's Cache feature relies on a limitation of memory so the Cache can proactively remove unused items from memory.</span></span>

<span data-ttu-id="dd940-152">Il est recommandé de configurer la fonctionnalité d’IIS 6.0 de recyclage de la mémoire.</span><span class="sxs-lookup"><span data-stu-id="dd940-152">It is recommended that you configure the memory recycling feature of IIS 6.0.</span></span> <span data-ttu-id="dd940-153">Pour configurer ce gestionnaire des Services IIS ouvert (Démarrer | Programmes | Outils d’administration | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="dd940-153">To configure this open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="dd940-154">Une fois ouvert, développez le dossier « Pools d’applications :</span><span class="sxs-lookup"><span data-stu-id="dd940-154">Once open, expand the 'Application Pools' folder:</span></span>

<span data-ttu-id="dd940-155">Pour chaque pool d’applications :</span><span class="sxs-lookup"><span data-stu-id="dd940-155">For each application pool:</span></span>

![](aspnet-and-iis6/_static/image13.jpg)

1. <span data-ttu-id="dd940-156">Avec le bouton droit sur le pool d’applications, par exemple « DefaultAppPool » et sélectionnez « Propriétés » :</span><span class="sxs-lookup"><span data-stu-id="dd940-156">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image14.jpg)

2. <span data-ttu-id="dd940-157">Ensuite, activez le recyclage de la mémoire en cliquant sur un « mémoire maximale utilisée (en mégaoctets) :'.</span><span class="sxs-lookup"><span data-stu-id="dd940-157">Next, enable Memory recycling by clicking on either 'Maximum used memory (in megabytes):'.</span></span> <span data-ttu-id="dd940-158">La valeur ne doit pas être supérieure à la quantité de mémoire physique (non virtuel) sur le serveur, une bonne approximation est de 60 % de la mémoire physique, par exemple, pour un serveur avec 512 Mo de mémoire physique, sélectionnez 310.</span><span class="sxs-lookup"><span data-stu-id="dd940-158">The value should not be more than the amount of physical (not virtual) memory on the server, a good approximation is 60% of the physical memory, i.e. for a server with 512MB of physical memory select 310.</span></span> <span data-ttu-id="dd940-159">Il est également recommandé que la valeur maximale dépasser 800 Mo lors de l’utilisation d’un espace d’adressage de 2 Go.</span><span class="sxs-lookup"><span data-stu-id="dd940-159">It is also recommended that the maximum not exceed 800MB when using a 2GB address space.</span></span> <span data-ttu-id="dd940-160">Si l’espace d’adressage de mémoire du serveur est de 3 Go, la limite maximale de mémoire pour le processus de travail peut être avec un maximum de 1, 800 Mo :</span><span class="sxs-lookup"><span data-stu-id="dd940-160">If the memory address space of the server is 3GB, the maximum memory limit for the worker process can be as high as 1,800MB:</span></span>

![](aspnet-and-iis6/_static/image15.jpg)

<span data-ttu-id="dd940-161">Cliquez sur « Appliquer » et le « OK » pour quitter la boîte de dialogue de propriétés.</span><span class="sxs-lookup"><span data-stu-id="dd940-161">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="dd940-162">Répétez cette procédure pour tous les pools d’applications disponibles.</span><span class="sxs-lookup"><span data-stu-id="dd940-162">Repeat this for all available application pools.</span></span>

### <a name="configuring-worker-recycling"></a><span data-ttu-id="dd940-163">Configuration de worker de recyclage</span><span class="sxs-lookup"><span data-stu-id="dd940-163">Configuring worker recycling</span></span>

<span data-ttu-id="dd940-164">Par défaut, IIS 6.0 est configuré pour recycler son processus de travail toutes les 29 heures.</span><span class="sxs-lookup"><span data-stu-id="dd940-164">By default IIS 6.0 is configured to recycle its worker process every 29 hours.</span></span> <span data-ttu-id="dd940-165">Il s’agit d’un peu agressive pour une application ASP.NET en cours d’exécution et il est recommandé que le recyclage de processus de travail automatique est désactivé.</span><span class="sxs-lookup"><span data-stu-id="dd940-165">This is a bit aggressive for an application running ASP.NET and it is recommended that automatic worker process recycling is disabled.</span></span>

<span data-ttu-id="dd940-166">Pour désactiver le recyclage des processus de travail automatique, ouvrez d’abord Gestionnaire des Services Internet (Démarrer | Programmes | Outils d’administration | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="dd940-166">To disable automatic worker process recycling, first open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="dd940-167">Une fois ouvert, développez le dossier « Pools d’applications :</span><span class="sxs-lookup"><span data-stu-id="dd940-167">Once open, expand the 'Application Pools' folder:</span></span>

![](aspnet-and-iis6/_static/image16.jpg)

<span data-ttu-id="dd940-168">Pour chaque pool d’applications :</span><span class="sxs-lookup"><span data-stu-id="dd940-168">For each application pool:</span></span>

1. <span data-ttu-id="dd940-169">Avec le bouton droit sur le pool d’applications, par exemple « DefaultAppPool » et sélectionnez « Propriétés » :</span><span class="sxs-lookup"><span data-stu-id="dd940-169">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image17.jpg)

2. <span data-ttu-id="dd940-170">Décochez la case « recycler les processus de travail (en minutes) :':</span><span class="sxs-lookup"><span data-stu-id="dd940-170">Uncheck 'Recycle worker process (in minutes):':</span></span>

![](aspnet-and-iis6/_static/image18.jpg)

<span data-ttu-id="dd940-171">Cliquez sur « Appliquer » et le « OK » pour quitter la boîte de dialogue de propriétés.</span><span class="sxs-lookup"><span data-stu-id="dd940-171">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="dd940-172">Répétez cette procédure pour tous les pools d’applications disponibles.</span><span class="sxs-lookup"><span data-stu-id="dd940-172">Repeat this for all available application pools.</span></span>

## <a name="granting-write-access-to-the-file-system"></a><span data-ttu-id="dd940-173">L’octroi de l’accès en écriture au système de fichiers</span><span class="sxs-lookup"><span data-stu-id="dd940-173">Granting write access to the file system</span></span>

<span data-ttu-id="dd940-174">Si votre application nécessite l’accès en écriture au système de fichiers et que vous utilisez NTFS, vous devez modifier une liste de contrôle d’accès (ACL) sur le dossier ou fichier pour accorder l’accès d’ASP.NET pour l’accès.</span><span class="sxs-lookup"><span data-stu-id="dd940-174">If your application requires write access to the file system and you are using NTFS you will need to modify an Access Control List (ACL) on the folder or file to grant ASP.NET access to.</span></span>

<span data-ttu-id="dd940-175">Par exemple, pour accorder ASP.NET accès en écriture à la c:\inetpub\wwwroot tout d’abord ouvrir l’Explorateur et accédez au répertoire :</span><span class="sxs-lookup"><span data-stu-id="dd940-175">For example, to grant ASP.NET write access to the c:\inetpub\wwwroot first open explorer and navigate to the directory:</span></span>

![](aspnet-and-iis6/_static/image19.jpg)

<span data-ttu-id="dd940-176">Ensuite, avec le bouton droit sur le répertoire, par exemple, « wwwroot » et sélectionnez Propriétés.</span><span class="sxs-lookup"><span data-stu-id="dd940-176">Next, right-click on the directory, e.g. 'wwwroot' and select properties.</span></span> <span data-ttu-id="dd940-177">La boîte de dialogue Propriétés s’affiche, sélectionnez l’onglet « Sécurité » :</span><span class="sxs-lookup"><span data-stu-id="dd940-177">After the properties dialog opens, select the 'Security' tab:</span></span>

![](aspnet-and-iis6/_static/image20.jpg)

<span data-ttu-id="dd940-178">Le répertoire c:\inetpub\wwwroot\ est un répertoire spécial dans qui le groupe IIS 6.0 spécial ' IIS\_WPG' est déjà accordée en lecture &amp; autorisations d’exécution, affichage du contenu du dossier et lecture.</span><span class="sxs-lookup"><span data-stu-id="dd940-178">The c:\inetpub\wwwroot\ directory is a special directory in that the special IIS 6.0 group 'IIS\_WPG' is already granted Read &amp; Execute, List Folder Contents, and Read permissions.</span></span> <span data-ttu-id="dd940-179">Toutefois, pour accorder l’autorisation d’écriture, vous devez cliquer sur la case à cocher Autoriser pour l’écriture :</span><span class="sxs-lookup"><span data-stu-id="dd940-179">However, to grant Write permission, you need to click the Allow checkbox for Write:</span></span>

![](aspnet-and-iis6/_static/image21.jpg)

<span data-ttu-id="dd940-180">IIS 6.0 a maintenant une autorisation d’écriture sur ce dossier.</span><span class="sxs-lookup"><span data-stu-id="dd940-180">IIS 6.0 now has write permission on this folder.</span></span> <span data-ttu-id="dd940-181">Pour accorder des autorisations d’écriture sur les autres dossiers, procédez comme suit : une remarque, vous devrez peut-être ajouter le site IIS\_groupe WPG si elle n’existe pas déjà.</span><span class="sxs-lookup"><span data-stu-id="dd940-181">To grant write permissions on other folders, follow these steps - note, you may need to add the IIS\_WPG group if it does not already exist.</span></span>

> [!CAUTION]
> <span data-ttu-id="dd940-182">Octroi d’autorisation d’écriture à IIS\_WPG permettra de n’importe quelle application ASP.NET écrire dans ce répertoire.</span><span class="sxs-lookup"><span data-stu-id="dd940-182">Granting write permission to IIS\_WPG will allow any ASP.NET application to write to this directory.</span></span>

## <a name="supporting-integrated-authentication-with-sql-server"></a><span data-ttu-id="dd940-183">Prise en charge de l’authentification intégrée avec SQL Server</span><span class="sxs-lookup"><span data-stu-id="dd940-183">Supporting integrated authentication with SQL Server</span></span>

<span data-ttu-id="dd940-184">L’authentification intégrée permet pour SQL Server tirer parti de l’authentification Windows NT pour valider les comptes d’ouverture de session de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="dd940-184">Integrated authentication allows for SQL Server to leverage Windows NT authentication to validate SQL Server logon accounts.</span></span> <span data-ttu-id="dd940-185">Ainsi, l’utilisateur à ignorer le processus d’ouverture de session SQL Server standard.</span><span class="sxs-lookup"><span data-stu-id="dd940-185">This allows the user to bypass the standard SQL Server logon process.</span></span> <span data-ttu-id="dd940-186">Avec cette approche, un utilisateur du réseau peut accéder à une base de données SQL Server sans fournir d’identification d’ouverture de session distincte ou de mot de passe, car SQL Server peut obtenir les informations d’utilisateur et mot de passe à partir du processus de sécurité de réseau de Windows NT.</span><span class="sxs-lookup"><span data-stu-id="dd940-186">With this approach, a network user can access a SQL Server database without supplying a separate logon identification or password because SQL Server obtains the user and password information from the Windows NT network security process.</span></span>

<span data-ttu-id="dd940-187">Le choix de l’authentification intégrée pour les applications ASP.NET d’est un bon choix, car aucune information d’identification n’est jamais stockées dans votre chaîne de connexion pour votre application.</span><span class="sxs-lookup"><span data-stu-id="dd940-187">Choosing integrated authentication for ASP.NET applications is a good choice because no credentials are ever stored within your connection string for your application.</span></span> <span data-ttu-id="dd940-188">Au lieu de cela, la chaîne de connexion utilisée pour se connecter à SQL se présentera comme suit :</span><span class="sxs-lookup"><span data-stu-id="dd940-188">Rather the connection string used to connect to SQL will look as follows:</span></span>

`"server=localhost; database=Northwind;Trusted_Connection=true"`

<span data-ttu-id="dd940-189">Cette chaîne de connexion indique à SQL Server à utiliser les informations d’identification Windows de l’application qui tente d’accéder à SQL Server.</span><span class="sxs-lookup"><span data-stu-id="dd940-189">This connection string tells SQL Server to use the Windows credentials of the application attempting to access SQL Server.</span></span> <span data-ttu-id="dd940-190">Dans le cas d’ASP.NET et IIS 6 il s’agit d’un compte dans IIS\_groupe WPG.</span><span class="sxs-lookup"><span data-stu-id="dd940-190">In the case of ASP.NET/IIS 6 this would be an account in the IIS\_WPG group.</span></span>

<span data-ttu-id="dd940-191">Pour activer l’authentification intégrée entre SQL Server et ASP.NET, vous devez d’abord vous assurer que SQL Server est configuré pour l’authentification intégrée ou l’authentification en Mode mixte - Vérifiez auprès de votre administrateur pour le déterminer.</span><span class="sxs-lookup"><span data-stu-id="dd940-191">To enable integrated authentication between SQL Server and ASP.NET, you will need to first ensure that SQL Server is configured for either Integrated authentication or Mixed-Mode authentication - check with your DBA to determine this.</span></span> <span data-ttu-id="dd940-192">Si SQL Server est dans un de ces deux modes, vous pouvez utiliser l’authentification intégrée.</span><span class="sxs-lookup"><span data-stu-id="dd940-192">If SQL Server is in one of these two modes, you can use integrated authentication.</span></span>

<span data-ttu-id="dd940-193">Ouvrez SQL Server Enterprise Manager (Démarrer | Programmes | Microsoft SQL Server | Enterprise Manager), sélectionnez le serveur approprié et développez le dossier sécurité :</span><span class="sxs-lookup"><span data-stu-id="dd940-193">Open SQL Server Enterprise Manager (Start | Programs | Microsoft SQL Server | Enterprise Manager), select the appropriate server, and expand the Security folder:</span></span>

![](aspnet-and-iis6/_static/image22.jpg)

<span data-ttu-id="dd940-194">Si ' BUILTINT\IIS\_WPG' groupe n’est pas répertorié, avec le bouton droit sur connexions et sélectionnez « Nouvelle connexion » :</span><span class="sxs-lookup"><span data-stu-id="dd940-194">If 'BUILTINT\IIS\_WPG' group is not listed, right-click on Logins and select 'New Login':</span></span>

![](aspnet-and-iis6/_static/image23.jpg)

<span data-ttu-id="dd940-195">Dans le ' nom : « zone de texte Entrez ' [nom de serveur ou du domaine] \IIS\_WPG' ou cliquez sur le bouton points de suspension pour ouvrir le sélecteur d’utilisateur ou le groupe Windows NT :</span><span class="sxs-lookup"><span data-stu-id="dd940-195">In the 'Name:' textbox either enter '[Server/Domain Name]\IIS\_WPG' or click on the ellipses button to open the Windows NT user/group picker:</span></span>

![](aspnet-and-iis6/_static/image24.jpg)

<span data-ttu-id="dd940-196">Sélectionnez IIS l’ordinateur actuel\_groupe WPG et cliquez sur « Ajouter » et OK pour fermer le sélecteur.</span><span class="sxs-lookup"><span data-stu-id="dd940-196">Select the current machine's IIS\_WPG group and click 'Add' and OK to close the picker.</span></span>

<span data-ttu-id="dd940-197">Ensuite, vous devez également définir la base de données par défaut et les autorisations pour accéder à la base de données.</span><span class="sxs-lookup"><span data-stu-id="dd940-197">You then need to also set the default database and the permissions to access the database.</span></span> <span data-ttu-id="dd940-198">Pour définir la base de données par défaut choisissez dans la liste déroulante, par exemple, ci-dessous Northwind est sélectionné :</span><span class="sxs-lookup"><span data-stu-id="dd940-198">To set the default database choose from the drop down list, e.g. below Northwind is selected:</span></span>

![](aspnet-and-iis6/_static/image25.jpg)

<span data-ttu-id="dd940-199">Cliquez ensuite sur l’onglet accès de la base de données :</span><span class="sxs-lookup"><span data-stu-id="dd940-199">Next, click on the Database Access tab:</span></span>

![](aspnet-and-iis6/_static/image26.jpg)

<span data-ttu-id="dd940-200">Cliquez sur la case à cocher Autoriser pour chaque base de données que vous souhaitez autoriser l’accès à.</span><span class="sxs-lookup"><span data-stu-id="dd940-200">Click on the Permit checkbox for every database that you wish to allow access to.</span></span> <span data-ttu-id="dd940-201">Vous devez également sélectionner des rôles de base de données, vérification de la base de données\_propriétaire garantira que votre connexion a toutes les autorisations nécessaires pour gérer et utiliser la base de données sélectionnée.</span><span class="sxs-lookup"><span data-stu-id="dd940-201">You will also need to select database roles, checking db\_owner will ensure your login has all necessary permissions to manage and use the selected database.</span></span>

<span data-ttu-id="dd940-202">Cliquez sur OK pour quitter la boîte de dialogue de propriété.</span><span class="sxs-lookup"><span data-stu-id="dd940-202">Click OK to exit the property dialog.</span></span> <span data-ttu-id="dd940-203">Votre application ASP.NET est maintenant configurée pour prendre en charge l’authentification intégrée de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="dd940-203">Your ASP.NET application is now configured to support integrated SQL Server authentication.</span></span>

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a><span data-ttu-id="dd940-204">Ne pas exécuter ASP.NET 1.0 en mode natif de IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="dd940-204">Don't run ASP.NET 1.0 in IIS 6.0 native mode</span></span>

<span data-ttu-id="dd940-205">ASP.NET 1.0 sur IIS 6.0 est uniquement pris en charge en mode de compatibilité IIS 5.</span><span class="sxs-lookup"><span data-stu-id="dd940-205">ASP.NET 1.0 on IIS 6.0 is only supported in IIS 5 compatibility mode.</span></span>

<span data-ttu-id="dd940-206">Pour configurer ASP.NET 1.0 pour s’exécuter en mode de compatibilité IIS 5.0, ouvrez le Gestionnaire des Services Internet et cliquez avec le bouton droit sur Sites Web et sélectionnez Propriétés :</span><span class="sxs-lookup"><span data-stu-id="dd940-206">To configure ASP.NET 1.0 to run in IIS 5.0 compatibility mode, open Internet Services Manager and right click Web Sites and select properties:</span></span>

![](aspnet-and-iis6/_static/image27.jpg)

<span data-ttu-id="dd940-207">Basculez vers l’onglet de Service et vérifier ? Exécuter les services Web dans IIS 5.0 Isolation Mode ? :</span><span class="sxs-lookup"><span data-stu-id="dd940-207">Switch to the Service Tab and check ?Run WWW Service in IIS 5.0 Isolation Mode?:</span></span>

![](aspnet-and-iis6/_static/image28.jpg)
