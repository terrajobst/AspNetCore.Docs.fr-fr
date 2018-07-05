---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET-accès refusé aux répertoires IIS | Microsoft Docs
author: rick-anderson
description: Ce livre blanc décrit la marche à suivre si une demande à votre application ASP.NET renvoie l’erreur « accès refusé au répertoire DirectoryName. Échec de s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: ''
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: 22868738e02472f5f433c729967ac324131a0fdc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383060"
---
<a name="aspnet-denied-access-to-iis-directories"></a><span data-ttu-id="07a30-104">ASP.NET-accès refusé aux répertoires IIS</span><span class="sxs-lookup"><span data-stu-id="07a30-104">ASP.NET Denied Access to IIS Directories</span></span>
====================
> <span data-ttu-id="07a30-105">Ce livre blanc décrit la marche à suivre si une demande à votre application ASP.NET retourne l’erreur, « refuser l’accès à *NomRépertoire* directory.</span><span class="sxs-lookup"><span data-stu-id="07a30-105">This whitepaper describes what you must do if a request to your ASP.NET application returns the error, "Denied Access to *DirectoryName* directory.</span></span> <span data-ttu-id="07a30-106">Échec de démarrage de l’analyse des modifications d’annuaire. »</span><span class="sxs-lookup"><span data-stu-id="07a30-106">Failed to start monitoring directory changes."</span></span>
> 
> <span data-ttu-id="07a30-107">S’applique à ASP.NET 1.0 et ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="07a30-107">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="07a30-108">ASP.NET V1 RTM s’exécute désormais à l’aide d’une moins privilégié d’un compte windows - enregistrés en tant que le compte « ASPNET » sur un ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="07a30-108">ASP.NET V1 RTM now runs using a less privileged windows account - registered as the "ASPNET" account on a local machine.</span></span>

<span data-ttu-id="07a30-109">Sur certains systèmes verrouillés, ce compte peut pas par défaut ont accès en lecture sécurité pour les répertoires de contenu d’un site Web, le répertoire racine de l’application ou le répertoire racine du site web.</span><span class="sxs-lookup"><span data-stu-id="07a30-109">On some locked down systems, this account may not by default have read security access to a website's content directories, the application root directory, or the web site root directory.</span></span> <span data-ttu-id="07a30-110">Dans ce cas, vous recevrez l’erreur suivante lors de la demande des pages à partir d’une application web donnée :</span><span class="sxs-lookup"><span data-stu-id="07a30-110">In this case you will receive the following error when requesting pages from a given web application:</span></span>

![](denied-access-to-iis-directories/_static/image1.jpg)

<span data-ttu-id="07a30-111">Pour résoudre ce problème, vous devrez modifier les autorisations de sécurité sur les répertoires appropriés.</span><span class="sxs-lookup"><span data-stu-id="07a30-111">To fix this you will need to change the security permissions on the appropriate directories.</span></span>

<span data-ttu-id="07a30-112">Plus précisément, ASP.NET nécessite une lecture, exécuter et liste d’accès pour le compte ASPNET pour la racine du site web (par exemple : c:\inetpub\wwwroot ou n’importe quel répertoire de site autre que vous avez peut-être configuré dans IIS), le répertoire de contenu et le répertoire racine de l’application pour surveiller les modifications de fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="07a30-112">Specifically, ASP.NET requires read, execute, and list access for the ASPNET account for the web site root (for example: c:\inetpub\wwwroot or any alternative site directory you may have configured in IIS), the content directory and the application root directory in order to monitor for configuration file changes.</span></span> <span data-ttu-id="07a30-113">La racine de l’application correspond au chemin de dossier associé au répertoire virtuel d’application dans l’outil d’Administration IIS (inetmgr).</span><span class="sxs-lookup"><span data-stu-id="07a30-113">The application root corresponds to the folder path associated with the application virtual directory in the IIS Administration tool (inetmgr).</span></span>

<span data-ttu-id="07a30-114">Par exemple, considérez la hiérarchie d’application suivante sous le dossier wwwroot.</span><span class="sxs-lookup"><span data-stu-id="07a30-114">For example, consider the following application hierarchy under the wwwroot folder.</span></span>

`C:\inetpub\wwwroot\myapp\default.aspx`

<span data-ttu-id="07a30-115">Pour cet exemple, le compte ASPNET doit les autorisations de lecture définies ci-dessus pour le contenu dans le myapp et le répertoire wwwroot.</span><span class="sxs-lookup"><span data-stu-id="07a30-115">For this example, the ASPNET account needs the read permissions defined above for content in both the myapp and the wwwroot directory.</span></span> <span data-ttu-id="07a30-116">Une ACL unique ou sur le dossier racine peut également éventuellement servir pour les deux répertoires si ils sont imbriqués.</span><span class="sxs-lookup"><span data-stu-id="07a30-116">A single inherited ACL on the root folder can also be optionally used for both directories if they're nested.</span></span>

<span data-ttu-id="07a30-117">Pour ajouter des autorisations à un répertoire, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="07a30-117">To add permissions to a directory, perform the following steps:</span></span>

- <span data-ttu-id="07a30-118">À l’aide de l’Explorateur Windows, accédez au répertoire</span><span class="sxs-lookup"><span data-stu-id="07a30-118">Using the Windows explorer, navigate to the directory</span></span>
- <span data-ttu-id="07a30-119">Cliquez avec le bouton droit sur le dossier du répertoire et choisissez « Propriétés »</span><span class="sxs-lookup"><span data-stu-id="07a30-119">Right click on the directory folder and choose "Properties"</span></span>
- <span data-ttu-id="07a30-120">Accédez à l’onglet « Sécurité » dans la boîte de dialogue de propriété</span><span class="sxs-lookup"><span data-stu-id="07a30-120">Navigate to the "Security" tab on the property dialog</span></span>
- <span data-ttu-id="07a30-121">Cliquez sur le bouton « Ajouter » et entrez le nom d’ordinateur suivi par le nom du compte ASPNET.</span><span class="sxs-lookup"><span data-stu-id="07a30-121">Click the "Add" button and enter the machine name followed by the ASPNET account name.</span></span> <span data-ttu-id="07a30-122">Par exemple, sur un ordinateur nommé « webdev », vous serez Entrez webdev\ASPNET et appuyez sur « OK ».</span><span class="sxs-lookup"><span data-stu-id="07a30-122">For example, on a machine named "webdev", you would enter webdev\ASPNET and hit "OK".</span></span>
- <span data-ttu-id="07a30-123">Assurez-vous que le compte ASPNET dispose le « en lecture &amp; Execute », « Contenu du dossier » et « Lecture » les cases cochées.</span><span class="sxs-lookup"><span data-stu-id="07a30-123">Ensure that the ASPNET account has the "Read &amp; Execute", "List Folder Contents", and "Read" checkboxes checked.</span></span>
- <span data-ttu-id="07a30-124">Appuyez sur OK pour fermer la boîte de dialogue et enregistrer les modifications.</span><span class="sxs-lookup"><span data-stu-id="07a30-124">Hit OK to dismiss the dialog and save the changes.</span></span>

![](denied-access-to-iis-directories/_static/image2.jpg)

<span data-ttu-id="07a30-125">Si vous le souhaitez, ces modifications peuvent être automatisées à l’aide de scripts ou l’outil « cacls.exe » qui est fourni avec Windows.</span><span class="sxs-lookup"><span data-stu-id="07a30-125">If desired, these changes can be automated using scripts or the "cacls.exe" tool that ships with Windows.</span></span> <span data-ttu-id="07a30-126">Pour plus d’informations sur le compte ASPNET, veuillez consulter la [document FAQ](https://go.microsoft.com/fwlink/?LinkId=5828).</span><span class="sxs-lookup"><span data-stu-id="07a30-126">For more information on the ASPNET account, please see the [FAQ document](https://go.microsoft.com/fwlink/?LinkId=5828).</span></span>

<span data-ttu-id="07a30-127">Si une application web donnée repose sur l’existence d’écriture ou modifier les autorisations pour un dossier particulier ou un fichier, celui-ci peut être octroyé en suivant la même procédure et en vérifiant les cases à cocher « Écriture » et/ou « Modifier ».</span><span class="sxs-lookup"><span data-stu-id="07a30-127">If a given web application relies on having write or modify permissions to a particular folder or file, this can be granted by following the same procedure and checking the "Write" and/or "Modify" checkboxes.</span></span>

<span data-ttu-id="07a30-128">Ne rencontrerez aucun problème sur les ordinateurs qui permettent la lecture de groupe utilisateurs l’accès à ces répertoires (qui est la configuration par défaut) ou à tout le monde, et les étapes ci-dessus ne sera pas nécessaire.</span><span class="sxs-lookup"><span data-stu-id="07a30-128">On machines that allow Everyone or the Users group read access to these directories (which is the default configuration), no issues will be encountered and the above steps will not be required.</span></span>
