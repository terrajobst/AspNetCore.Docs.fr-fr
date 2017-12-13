---
uid: whitepapers/denied-access-to-iis-directories
title: "Accès refusé à ASP.NET pour les répertoires IIS | Documents Microsoft"
author: rick-anderson
description: "Ce livre blanc décrit ce que vous devez faire si une demande à votre application ASP.NET renvoie l’erreur « accès refusé au répertoire du nom du répertoire. Impossible de s..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: 64118ac7a5f280775106d2dc7636923b08f28d89
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-denied-access-to-iis-directories"></a><span data-ttu-id="3640b-104">Accès refusé à ASP.NET pour les répertoires IIS</span><span class="sxs-lookup"><span data-stu-id="3640b-104">ASP.NET Denied Access to IIS Directories</span></span>
====================
> <span data-ttu-id="3640b-105">Ce livre blanc décrit ce que vous devez faire si une demande à votre application ASP.NET retourne l’erreur, « refuser l’accès à *Nom_répertoire* active.</span><span class="sxs-lookup"><span data-stu-id="3640b-105">This whitepaper describes what you must do if a request to your ASP.NET application returns the error, "Denied Access to *DirectoryName* directory.</span></span> <span data-ttu-id="3640b-106">Échec de démarrage de l’analyse du répertoire chaanges. »</span><span class="sxs-lookup"><span data-stu-id="3640b-106">Failed to start monitoring directory chaanges."</span></span>
> 
> <span data-ttu-id="3640b-107">S’applique à ASP.NET 1.0 et 1.1 d’ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3640b-107">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="3640b-108">Version RTM de V1 ASP.NET s’exécute maintenant à l’aide d’une moins privilégié du compte windows - inscrit en tant que le compte « ASPNET » sur un ordinateur local.</span><span class="sxs-lookup"><span data-stu-id="3640b-108">ASP.NET V1 RTM now runs using a less privileged windows account - registered as the "ASPNET" account on a local machine.</span></span>

<span data-ttu-id="3640b-109">Sur certains systèmes verrouillés, ce compte peut pas par défaut ont accès en lecture sécurité pour les répertoires de contenu d’un site Web, le répertoire racine de l’application ou le répertoire racine du site web.</span><span class="sxs-lookup"><span data-stu-id="3640b-109">On some locked down systems, this account may not by default have read security access to a website's content directories, the application root directory, or the web site root directory.</span></span> <span data-ttu-id="3640b-110">Dans ce cas, vous recevrez l’erreur suivante lors de la demande de pages à partir d’une application web donnée :</span><span class="sxs-lookup"><span data-stu-id="3640b-110">In this case you will receive the following error when requesting pages from a given web application:</span></span>

![](denied-access-to-iis-directories/_static/image1.jpg)

<span data-ttu-id="3640b-111">Pour résoudre ce problème, vous devrez modifier les autorisations de sécurité sur les répertoires appropriés.</span><span class="sxs-lookup"><span data-stu-id="3640b-111">To fix this you will need to change the security permissions on the appropriate directories.</span></span>

<span data-ttu-id="3640b-112">Plus précisément, ASP.NET nécessite une lecture, exécuter et liste d’accès pour le compte ASPNET pour la racine du site web (par exemple : c:\inetpub\wwwroot ou n’importe quel répertoire autre site que vous avez peut-être configuré dans IIS), le répertoire de contenu et le répertoire racine de l’application pour surveiller les modifications de fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="3640b-112">Specifically, ASP.NET requires read, execute, and list access for the ASPNET account for the web site root (for example: c:\inetpub\wwwroot or any alternative site directory you may have configured in IIS), the content directory and the application root directory in order to monitor for configuration file changes.</span></span> <span data-ttu-id="3640b-113">La racine de l’application correspond au chemin de dossier associé au répertoire virtuel d’application dans l’outil d’Administration IIS (inetmgr).</span><span class="sxs-lookup"><span data-stu-id="3640b-113">The application root corresponds to the folder path associated with the application virtual directory in the IIS Administration tool (inetmgr).</span></span>

<span data-ttu-id="3640b-114">Par exemple, considérez la hiérarchie d’application suivant sous le dossier wwwroot.</span><span class="sxs-lookup"><span data-stu-id="3640b-114">For example, consider the following application hierarchy under the wwwroot folder.</span></span>

`C:\inetpub\wwwroot\myapp\default.aspx`

<span data-ttu-id="3640b-115">Pour cet exemple, le compte ASPNET doit les autorisations de lecture définies ci-dessus pour le contenu dans le myapp et le répertoire wwwroot.</span><span class="sxs-lookup"><span data-stu-id="3640b-115">For this example, the ASPNET account needs the read permissions defined above for content in both the myapp and the wwwroot directory.</span></span> <span data-ttu-id="3640b-116">Une ACL unique ou sur le dossier racine peut également éventuellement servir pour les deux répertoires si elles sont imbriquées.</span><span class="sxs-lookup"><span data-stu-id="3640b-116">A single inherited ACL on the root folder can also be optionally used for both directories if they're nested.</span></span>

<span data-ttu-id="3640b-117">Pour ajouter des autorisations pour un répertoire, procédez comme suit :</span><span class="sxs-lookup"><span data-stu-id="3640b-117">To add permissions to a directory, perform the following steps:</span></span>

- <span data-ttu-id="3640b-118">À l’aide de l’Explorateur Windows, accédez au répertoire</span><span class="sxs-lookup"><span data-stu-id="3640b-118">Using the Windows explorer, navigate to the directory</span></span>
- <span data-ttu-id="3640b-119">Cliquez avec le bouton droit sur le dossier de répertoire et choisissez « Propriétés »</span><span class="sxs-lookup"><span data-stu-id="3640b-119">Right click on the directory folder and choose "Properties"</span></span>
- <span data-ttu-id="3640b-120">Accédez à l’onglet « Sécurité » dans la boîte de dialogue propriété</span><span class="sxs-lookup"><span data-stu-id="3640b-120">Navigate to the "Security" tab on the property dialog</span></span>
- <span data-ttu-id="3640b-121">Cliquez sur le bouton « Ajouter » et entrez le nom d’ordinateur suivi du nom de compte ASPNET.</span><span class="sxs-lookup"><span data-stu-id="3640b-121">Click the "Add" button and enter the machine name followed by the ASPNET account name.</span></span> <span data-ttu-id="3640b-122">Par exemple, sur un ordinateur nommé « webdev », vous entrez webdev\ASPNET et appuyez sur « OK ».</span><span class="sxs-lookup"><span data-stu-id="3640b-122">For example, on a machine named "webdev", you would enter webdev\ASPNET and hit "OK".</span></span>
- <span data-ttu-id="3640b-123">Assurez-vous que le compte ASPNET possède le « en lecture &amp; Execute », « Afficher le contenu du dossier » et les cases à cocher « Lecture » vérifiée.</span><span class="sxs-lookup"><span data-stu-id="3640b-123">Ensure that the ASPNET account has the "Read &amp; Execute", "List Folder Contents", and "Read" checkboxes checked.</span></span>
- <span data-ttu-id="3640b-124">Appuyez sur OK pour fermer la boîte de dialogue et enregistrer les modifications.</span><span class="sxs-lookup"><span data-stu-id="3640b-124">Hit OK to dismiss the dialog and save the changes.</span></span>

![](denied-access-to-iis-directories/_static/image2.jpg)

<span data-ttu-id="3640b-125">Si vous le souhaitez, ces modifications peuvent être automatisées à l’aide de scripts ou l’outil « cacls.exe » qui est fourni avec Windows.</span><span class="sxs-lookup"><span data-stu-id="3640b-125">If desired, these changes can be automated using scripts or the "cacls.exe" tool that ships with Windows.</span></span> <span data-ttu-id="3640b-126">Pour plus d’informations sur le compte ASPNET, veuillez consulter le [document FAQ](https://go.microsoft.com/fwlink/?LinkId=5828).</span><span class="sxs-lookup"><span data-stu-id="3640b-126">For more information on the ASPNET account, please see the [FAQ document](https://go.microsoft.com/fwlink/?LinkId=5828).</span></span>

<span data-ttu-id="3640b-127">Si une application web donnée repose sur la disponibilité d’écriture ou modifier les autorisations dans un fichier ou un dossier particulier, celui-ci peut être octroyé en suivant la même procédure et en activant les cases à cocher « Écriture » et/ou « Modifier ».</span><span class="sxs-lookup"><span data-stu-id="3640b-127">If a given web application relies on having write or modify permissions to a particular folder or file, this can be granted by following the same procedure and checking the "Write" and/or "Modify" checkboxes.</span></span>

<span data-ttu-id="3640b-128">Sur les ordinateurs qui permettent à tout le monde ou au groupe utilisateurs lecture l’accès à ces répertoires (qui est la configuration par défaut), aucun problème n’est détecté, et les étapes ci-dessus ne sont pas requis.</span><span class="sxs-lookup"><span data-stu-id="3640b-128">On machines that allow Everyone or the Users group read access to these directories (which is the default configuration), no issues will be encountered and the above steps will not be required.</span></span>
