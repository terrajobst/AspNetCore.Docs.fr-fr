---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Partie 7 : L’appartenance et l’autorisation | Documents Microsoft'
author: jongalloway
description: Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application de magasin de musique ASP.NET MVC. Partie 7 couvre l’appartenance et l’autorisation.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2010
ms.topic: article
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: a0f599da4691c5bb7c8e6f01625fc0e94ce0eac8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879502"
---
<a name="part-7-membership-and-authorization"></a><span data-ttu-id="2e62d-104">Partie 7 : L’appartenance et l’autorisation</span><span class="sxs-lookup"><span data-stu-id="2e62d-104">Part 7: Membership and Authorization</span></span>
====================
<span data-ttu-id="2e62d-105">par [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="2e62d-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="2e62d-106">Le magasin de musique MVC est une application du didacticiel qui présente et explique étape par étape comment utiliser ASP.NET MVC et Visual Studio pour le développement web.</span><span class="sxs-lookup"><span data-stu-id="2e62d-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="2e62d-107">Le magasin de musique MVC est une implémentation de magasin exemple léger qui vend des albums de musique en ligne et implémente l’administration de site de base, authentification de l’utilisateur et les fonctionnalités de panier d’achat.</span><span class="sxs-lookup"><span data-stu-id="2e62d-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="2e62d-108">Cette série de didacticiels détaille toutes les mesures prises pour générer l’exemple d’application de magasin de musique ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="2e62d-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="2e62d-109">Partie 7 couvre l’appartenance et l’autorisation.</span><span class="sxs-lookup"><span data-stu-id="2e62d-109">Part 7 covers Membership and Authorization.</span></span>


<span data-ttu-id="2e62d-110">Notre contrôleur du Gestionnaire de magasin est actuellement accessible à toute personne visitant notre site.</span><span class="sxs-lookup"><span data-stu-id="2e62d-110">Our Store Manager controller is currently accessible to anyone visiting our site.</span></span> <span data-ttu-id="2e62d-111">Nous allons modifier cette option pour restreindre l’accès aux administrateurs de site.</span><span class="sxs-lookup"><span data-stu-id="2e62d-111">Let's change this to restrict permission to site administrators.</span></span>

## <a name="adding-the-accountcontroller-and-views"></a><span data-ttu-id="2e62d-112">Ajout du AccountController et vues</span><span class="sxs-lookup"><span data-stu-id="2e62d-112">Adding the AccountController and Views</span></span>

<span data-ttu-id="2e62d-113">Une différence entre le modèle d’Application Web de ASP.NET MVC 3 complet et le modèle d’Application ASP.NET MVC 3 vide Web est que le modèle vide n’inclut pas un contrôleur de compte.</span><span class="sxs-lookup"><span data-stu-id="2e62d-113">One difference between the full ASP.NET MVC 3 Web Application template and the ASP.NET MVC 3 Empty Web Application template is that the empty template doesn't include an Account Controller.</span></span> <span data-ttu-id="2e62d-114">Nous allons ajouter un contrôleur de compte en copiant quelques fichiers à partir d’une application ASP.NET MVC à partir du modèle d’Application Web de ASP.NET MVC 3 complet.</span><span class="sxs-lookup"><span data-stu-id="2e62d-114">We'll add an Account Controller by copying a few files from a new ASP.NET MVC application created from the full ASP.NET MVC 3 Web Application template.</span></span>

<span data-ttu-id="2e62d-115">Créez une application ASP.NET MVC en utilisant le modèle d’Application Web de ASP.NET MVC 3 complète et copiez les fichiers suivants dans les mêmes répertoires dans notre projet :</span><span class="sxs-lookup"><span data-stu-id="2e62d-115">Create a new ASP.NET MVC application using the full ASP.NET MVC 3 Web Application template and copy the following files into the same directories in our project:</span></span>

1. <span data-ttu-id="2e62d-116">Copiez AccountController.cs dans le répertoire de contrôleurs</span><span class="sxs-lookup"><span data-stu-id="2e62d-116">Copy AccountController.cs in the Controllers directory</span></span>
2. <span data-ttu-id="2e62d-117">Copiez AccountModels dans le répertoire de modèles</span><span class="sxs-lookup"><span data-stu-id="2e62d-117">Copy AccountModels in the Models directory</span></span>
3. <span data-ttu-id="2e62d-118">Créer un répertoire de compte dans le répertoire de vues et de copier toutes les quatre affichages dans</span><span class="sxs-lookup"><span data-stu-id="2e62d-118">Create an Account directory inside the Views directory and copy all four views in</span></span>

<span data-ttu-id="2e62d-119">Modifier l’espace de noms pour les classes de contrôleur et le modèle afin qu’ils commencent par MvcMusicStore.</span><span class="sxs-lookup"><span data-stu-id="2e62d-119">Change the namespace for the Controller and Model classes so they begin with MvcMusicStore.</span></span> <span data-ttu-id="2e62d-120">La classe AccountController doit utiliser l’espace de noms MvcMusicStore.Controllers, et la classe AccountModels doit utiliser l’espace de noms MvcMusicStore.Models.</span><span class="sxs-lookup"><span data-stu-id="2e62d-120">The AccountController class should use the MvcMusicStore.Controllers namespace, and the AccountModels class should use the MvcMusicStore.Models namespace.</span></span>

<span data-ttu-id="2e62d-121">*Remarque : Ces fichiers sont également disponibles dans le téléchargement MvcMusicStore-Assets.zip à partir de laquelle nous notre site conception fichiers copiés au début du didacticiel. Les fichiers d’appartenance sont situés dans le répertoire de Code.*</span><span class="sxs-lookup"><span data-stu-id="2e62d-121">*Note: These files are also available in the MvcMusicStore-Assets.zip download from which we copied our site design files at the beginning of the tutorial. The Membership files are located in the Code directory.*</span></span>

<span data-ttu-id="2e62d-122">La solution de mise à jour doit se présenter comme suit :</span><span class="sxs-lookup"><span data-stu-id="2e62d-122">The updated solution should look like the following:</span></span>

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a><span data-ttu-id="2e62d-123">Ajout d’un utilisateur administratif avec le site de Configuration ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2e62d-123">Adding an Administrative User with the ASP.NET Configuration site</span></span>

<span data-ttu-id="2e62d-124">Avant que nous avons besoin d’autorisation dans notre site Web, nous devrons créer un utilisateur avec accès.</span><span class="sxs-lookup"><span data-stu-id="2e62d-124">Before we require Authorization in our website, we'll need to create a user with access.</span></span> <span data-ttu-id="2e62d-125">Pour créer un utilisateur, le plus simple consiste à utiliser le site Web de Configuration ASP.NET intégré.</span><span class="sxs-lookup"><span data-stu-id="2e62d-125">The easiest way to create a user is to use the built-in ASP.NET Configuration website.</span></span>

<span data-ttu-id="2e62d-126">Lancez le site Web de Configuration d’ASP.NET en cliquant sur Suivant l’icône dans l’Explorateur de solutions.</span><span class="sxs-lookup"><span data-stu-id="2e62d-126">Launch the ASP.NET Configuration website by clicking following the icon in the Solution Explorer.</span></span>

![](mvc-music-store-part-7/_static/image2.png)

<span data-ttu-id="2e62d-127">Cela permet de lancer un site Web de configuration.</span><span class="sxs-lookup"><span data-stu-id="2e62d-127">This launches a configuration website.</span></span> <span data-ttu-id="2e62d-128">Cliquez sur l’onglet sécurité de l’écran d’accueil, puis cliquez sur le lien « Activer les rôles » dans le centre de l’écran.</span><span class="sxs-lookup"><span data-stu-id="2e62d-128">Click on the Security tab on the home screen, then click the "Enable roles" link in the center of the screen.</span></span>

![](mvc-music-store-part-7/_static/image3.png)

<span data-ttu-id="2e62d-129">Cliquez sur le lien « Créer ou gérer des rôles ».</span><span class="sxs-lookup"><span data-stu-id="2e62d-129">Click the "Create or Manage roles" link.</span></span>

![](mvc-music-store-part-7/_static/image4.png)

<span data-ttu-id="2e62d-130">Entrez le nom de rôle « Administrateur » et appuyez sur le bouton Ajouter un rôle.</span><span class="sxs-lookup"><span data-stu-id="2e62d-130">Enter "Administrator" as the role name and press the Add Role button.</span></span>

![](mvc-music-store-part-7/_static/image5.png)

<span data-ttu-id="2e62d-131">Cliquez sur le bouton précédent, puis cliquez sur le lien de création d’utilisateur sur le côté gauche.</span><span class="sxs-lookup"><span data-stu-id="2e62d-131">Click the Back button, then click on the Create user link on the left side.</span></span>

![](mvc-music-store-part-7/_static/image6.png)

<span data-ttu-id="2e62d-132">Renseignez les champs des informations sur la gauche en utilisant les informations suivantes :</span><span class="sxs-lookup"><span data-stu-id="2e62d-132">Fill in the user information fields on the left using the following information:</span></span>

| <span data-ttu-id="2e62d-133">**Champ**</span><span class="sxs-lookup"><span data-stu-id="2e62d-133">**Field**</span></span> | <span data-ttu-id="2e62d-134">**Valeur**</span><span class="sxs-lookup"><span data-stu-id="2e62d-134">**Value**</span></span> |
| --- | --- |
| <span data-ttu-id="2e62d-135">**Nom d'utilisateur**</span><span class="sxs-lookup"><span data-stu-id="2e62d-135">**User Name**</span></span> | <span data-ttu-id="2e62d-136">Administrateur</span><span class="sxs-lookup"><span data-stu-id="2e62d-136">Administrator</span></span> |
| <span data-ttu-id="2e62d-137">**Mot de passe**</span><span class="sxs-lookup"><span data-stu-id="2e62d-137">**Password**</span></span> | <span data-ttu-id="2e62d-138">password123 !</span><span class="sxs-lookup"><span data-stu-id="2e62d-138">password123!</span></span> |
| <span data-ttu-id="2e62d-139">**Confirmer le mot de passe**</span><span class="sxs-lookup"><span data-stu-id="2e62d-139">**Confirm Password**</span></span> | <span data-ttu-id="2e62d-140">password123 !</span><span class="sxs-lookup"><span data-stu-id="2e62d-140">password123!</span></span> |
| <span data-ttu-id="2e62d-141">**Message électronique**</span><span class="sxs-lookup"><span data-stu-id="2e62d-141">**E-mail**</span></span> | <span data-ttu-id="2e62d-142">(n’importe quelle adresse de messagerie fonctionne)</span><span class="sxs-lookup"><span data-stu-id="2e62d-142">(any email address will work)</span></span> |
| <span data-ttu-id="2e62d-143">**Question de sécurité**</span><span class="sxs-lookup"><span data-stu-id="2e62d-143">**Security Question**</span></span> | <span data-ttu-id="2e62d-144">(comme vous le souhaitez)</span><span class="sxs-lookup"><span data-stu-id="2e62d-144">(whatever you like)</span></span> |
| <span data-ttu-id="2e62d-145">**Réponse de sécurité**</span><span class="sxs-lookup"><span data-stu-id="2e62d-145">**Security Answer**</span></span> | <span data-ttu-id="2e62d-146">(comme vous le souhaitez)</span><span class="sxs-lookup"><span data-stu-id="2e62d-146">(whatever you like)</span></span> |

<span data-ttu-id="2e62d-147">*Remarque : Vous pouvez utiliser naturellement de n’importe quel mot de passe. Les paramètres de sécurité de mot de passe par défaut requièrent un mot de passe est de 7 caractères et contient un caractère non alphanumérique.*</span><span class="sxs-lookup"><span data-stu-id="2e62d-147">*Note: You can of course use any password you'd like. The default password security settings require a password that is 7 characters long and contains one non-alphanumeric character.*</span></span>

<span data-ttu-id="2e62d-148">Sélectionnez le rôle d’administrateur pour cet utilisateur, puis cliquez sur le bouton Créer un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="2e62d-148">Select the Administrator role for this user, and click the Create User button.</span></span>

![](mvc-music-store-part-7/_static/image7.png)

<span data-ttu-id="2e62d-149">À ce stade, vous devez voir un message indiquant que l’utilisateur a été créé avec succès.</span><span class="sxs-lookup"><span data-stu-id="2e62d-149">At this point, you should see a message indicating that the user was created successfully.</span></span>

![](mvc-music-store-part-7/_static/image8.png)

<span data-ttu-id="2e62d-150">Vous pouvez maintenant fermer la fenêtre du navigateur.</span><span class="sxs-lookup"><span data-stu-id="2e62d-150">You can now close the browser window.</span></span>

## <a name="role-based-authorization"></a><span data-ttu-id="2e62d-151">Autorisation basée sur un rôle</span><span class="sxs-lookup"><span data-stu-id="2e62d-151">Role-based Authorization</span></span>

<span data-ttu-id="2e62d-152">Nous pouvons désormais restreindre l’accès à la StoreManagerController à l’aide de l’attribut [Authorize], qui spécifie que l’utilisateur doit être dans le rôle d’administrateur pour accéder à toute action de contrôleur dans la classe.</span><span class="sxs-lookup"><span data-stu-id="2e62d-152">Now we can restrict access to the StoreManagerController using the [Authorize] attribute, specifying that the user must be in the Administrator role to access any controller action in the class.</span></span>

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

<span data-ttu-id="2e62d-153">*Remarque : L’attribut [Authorize] peut être placé sur les méthodes d’action spécifique, ainsi qu’au niveau de la classe de contrôleur.*</span><span class="sxs-lookup"><span data-stu-id="2e62d-153">*Note: The [Authorize] attribute can be placed on specific action methods as well as at the Controller class level.*</span></span>

<span data-ttu-id="2e62d-154">Maintenant accédant à /StoreManager affiche une boîte de dialogue Ouverture de session :</span><span class="sxs-lookup"><span data-stu-id="2e62d-154">Now browsing to /StoreManager brings up a Log On dialog:</span></span>

![](mvc-music-store-part-7/_static/image9.png)

<span data-ttu-id="2e62d-155">Après l’ouverture de session avec notre nouveau compte d’administrateur, nous sommes en mesure de passer à l’écran Album modifier comme avant.</span><span class="sxs-lookup"><span data-stu-id="2e62d-155">After logging on with our new Administrator account, we're able to go to the Album Edit screen as before.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2e62d-156">[Précédent](mvc-music-store-part-6.md)
> [Suivant](mvc-music-store-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="2e62d-156">[Previous](mvc-music-store-part-6.md)
[Next](mvc-music-store-part-8.md)</span></span>
