---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: Modifiez la clé primaire pour les utilisateurs dans ASP.NET Identity | Microsoft Docs
author: tfitzmac
description: Dans Visual Studio 2013, l’application web par défaut utilise une valeur de chaîne pour la clé pour les comptes d’utilisateur. ASP.NET Identity vous permet de modifier le type de la...
ms.author: riande
ms.date: 09/30/2014
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 2ec9894df2f9a48ef482715ce71bb09fb4758127
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837448"
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a><span data-ttu-id="f415a-104">Modifier la clé primaire pour les utilisateurs dans ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="f415a-104">Change Primary Key for Users in ASP.NET Identity</span></span>
====================
<span data-ttu-id="f415a-105">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f415a-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f415a-106">Dans Visual Studio 2013, l’application web par défaut utilise une valeur de chaîne pour la clé pour les comptes d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f415a-106">In Visual Studio 2013, the default web application uses a string value for the key for user accounts.</span></span> <span data-ttu-id="f415a-107">ASP.NET Identity vous permet de modifier le type de la clé pour répondre aux besoins de vos données.</span><span class="sxs-lookup"><span data-stu-id="f415a-107">ASP.NET Identity enables you to change the type of the key to meet your data requirements.</span></span> <span data-ttu-id="f415a-108">Par exemple, vous pouvez modifier le type de la clé à partir d’une chaîne en entier.</span><span class="sxs-lookup"><span data-stu-id="f415a-108">For example, you can change the type of the key from a string to an integer.</span></span>
> 
> <span data-ttu-id="f415a-109">Cette rubrique montre comment commencer avec la valeur par défaut application web et modifier la clé de compte utilisateur à un entier.</span><span class="sxs-lookup"><span data-stu-id="f415a-109">This topic shows how to start with the default web application and change the user account key to an integer.</span></span> <span data-ttu-id="f415a-110">Vous pouvez utiliser les mêmes modifications à implémenter n’importe quel type de clé dans votre projet.</span><span class="sxs-lookup"><span data-stu-id="f415a-110">You can use the same modifications to implement any type of key in your project.</span></span> <span data-ttu-id="f415a-111">Il montre comment effectuer ces modifications dans l’application web par défaut, mais vous pouvez appliquer des modifications similaires à une application personnalisée.</span><span class="sxs-lookup"><span data-stu-id="f415a-111">It shows how to make these changes in the default web application, but you could apply similar modifications to a customized application.</span></span> <span data-ttu-id="f415a-112">Il montre les modifications nécessaires lorsque vous travaillez avec MVC ou Web Forms.</span><span class="sxs-lookup"><span data-stu-id="f415a-112">It shows the changes needed when working with MVC or Web Forms.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f415a-113">Versions des logiciels utilisées dans le didacticiel</span><span class="sxs-lookup"><span data-stu-id="f415a-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f415a-114">Visual Studio 2013 avec Update 2 (ou version ultérieure)</span><span class="sxs-lookup"><span data-stu-id="f415a-114">Visual Studio 2013 with Update 2 (or later)</span></span>
> - <span data-ttu-id="f415a-115">ASP.NET Identity 2.1 ou version ultérieure</span><span class="sxs-lookup"><span data-stu-id="f415a-115">ASP.NET Identity 2.1 or later</span></span>


<span data-ttu-id="f415a-116">Pour effectuer les étapes décrites dans ce didacticiel, vous devez disposer de Visual Studio 2013 Update 2 (ou version ultérieure) et une application web créée à partir du modèle d’Application Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f415a-116">To perform the steps in this tutorial, you must have Visual Studio 2013 Update 2 (or later), and a web application created from the ASP.NET Web Application template.</span></span> <span data-ttu-id="f415a-117">Le modèle modifié dans Update 3.</span><span class="sxs-lookup"><span data-stu-id="f415a-117">The template changed in Update 3.</span></span> <span data-ttu-id="f415a-118">Cette rubrique montre comment modifier le modèle dans Update 2 et 3 de la mise à jour.</span><span class="sxs-lookup"><span data-stu-id="f415a-118">This topic shows how to change the template in Update 2 and Update 3.</span></span>

<span data-ttu-id="f415a-119">Cette rubrique contient les sections suivantes :</span><span class="sxs-lookup"><span data-stu-id="f415a-119">This topic contains the following sections:</span></span>

- [<span data-ttu-id="f415a-120">Modifier le type de la clé dans la classe d’identité utilisateur</span><span class="sxs-lookup"><span data-stu-id="f415a-120">Change the type of the key in the Identity user class</span></span>](#userclass)
- [<span data-ttu-id="f415a-121">Ajouter des classes personnalisées de l’identité qui utilisent le type de clé</span><span class="sxs-lookup"><span data-stu-id="f415a-121">Add customized Identity classes that use the key type</span></span>](#customclass)
- [<span data-ttu-id="f415a-122">Modifier le Gestionnaire de contexte utilisateur et pour utiliser le type de clé</span><span class="sxs-lookup"><span data-stu-id="f415a-122">Change the context class and user manager to use the key type</span></span>](#context)
- [<span data-ttu-id="f415a-123">Modifier la configuration de démarrage à utiliser le type de clé</span><span class="sxs-lookup"><span data-stu-id="f415a-123">Change start-up configuration to use the key type</span></span>](#startup)
- [<span data-ttu-id="f415a-124">Pour MVC avec Update 2, modifiez le AccountController pour passer le type de clé</span><span class="sxs-lookup"><span data-stu-id="f415a-124">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="f415a-125">Pour MVC avec Update 3, modifier AccountController les ManageController pour passer le type de clé</span><span class="sxs-lookup"><span data-stu-id="f415a-125">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="f415a-126">Pour les formulaires Web avec Update 2, modifiez les pages de compte pour passer le type de clé</span><span class="sxs-lookup"><span data-stu-id="f415a-126">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="f415a-127">Pour les formulaires Web avec Update 3, modifiez les pages de compte pour passer le type de clé</span><span class="sxs-lookup"><span data-stu-id="f415a-127">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)
- [<span data-ttu-id="f415a-128">Exécuter l’application</span><span class="sxs-lookup"><span data-stu-id="f415a-128">Run application</span></span>](#run)
- [<span data-ttu-id="f415a-129">Autres ressources</span><span class="sxs-lookup"><span data-stu-id="f415a-129">Other resources</span></span>](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a><span data-ttu-id="f415a-130">Modifier le type de la clé dans la classe d’identité utilisateur</span><span class="sxs-lookup"><span data-stu-id="f415a-130">Change the type of the key in the Identity user class</span></span>

<span data-ttu-id="f415a-131">Dans votre projet à partir du modèle d’Application Web ASP.NET, spécifiez que la classe ApplicationUser utilise un entier pour la clé pour les comptes d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f415a-131">In your project created from the ASP.NET Web Application template, specify that the ApplicationUser class uses an integer for the key for user accounts.</span></span> <span data-ttu-id="f415a-132">Dans IdentityModels.cs, modifiez la classe ApplicationUser d’hériter de IdentityUser qui a un type de **int** pour le paramètre générique TKey.</span><span class="sxs-lookup"><span data-stu-id="f415a-132">In IdentityModels.cs, change the ApplicationUser class to inherit from IdentityUser that has a type of **int** for the TKey generic parameter.</span></span> <span data-ttu-id="f415a-133">Vous transmettez également les noms des trois classes personnalisées dont vous n’avez pas encore implémenté.</span><span class="sxs-lookup"><span data-stu-id="f415a-133">You also pass the names of three customized class which you have not implemented yet.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

<span data-ttu-id="f415a-134">Vous avez changé le type de la clé, mais par défaut, le reste de l’application suppose toujours que la clé est une chaîne.</span><span class="sxs-lookup"><span data-stu-id="f415a-134">You have changed the type of the key, but, by default, the rest of the application still assumes the key is a string.</span></span> <span data-ttu-id="f415a-135">Vous devez indiquer explicitement le type de la clé dans le code qui suppose une chaîne.</span><span class="sxs-lookup"><span data-stu-id="f415a-135">You must explicitly indicate the type of the key in code that assumes a string.</span></span>

<span data-ttu-id="f415a-136">Dans le **ApplicationUser** de classe, de modifier le **GenerateUserIdentityAsync** (méthode) à inclure int, comme indiqué dans le code en surbrillance ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="f415a-136">In the **ApplicationUser** class, change the **GenerateUserIdentityAsync** method to include int, as shown in the highlighted code below.</span></span> <span data-ttu-id="f415a-137">Cette modification n’est pas nécessaire pour les projets Web Forms avec le modèle de mise à jour 3.</span><span class="sxs-lookup"><span data-stu-id="f415a-137">This change is not necessary for Web Forms projects with the Update 3 template.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a><span data-ttu-id="f415a-138">Ajouter des classes personnalisées de l’identité qui utilisent le type de clé</span><span class="sxs-lookup"><span data-stu-id="f415a-138">Add customized Identity classes that use the key type</span></span>

<span data-ttu-id="f415a-139">Les autres classes d’identité, telles que les IdentityUserRole IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, sont toujours configurées pour utiliser une clé de chaîne.</span><span class="sxs-lookup"><span data-stu-id="f415a-139">The other Identity classes, such as IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, are still set up to use a string key.</span></span> <span data-ttu-id="f415a-140">Créer de nouvelles versions de ces classes qui spécifient un entier pour la clé.</span><span class="sxs-lookup"><span data-stu-id="f415a-140">Create new versions of these classes that specify an integer for the key.</span></span> <span data-ttu-id="f415a-141">Vous n’avez pas besoin de fournir beaucoup code d’implémentation dans ces classes, vous définissez principalement simplement int comme clé.</span><span class="sxs-lookup"><span data-stu-id="f415a-141">You do not need to provide much implementation code in these classes, you are primarily just setting int as the key.</span></span>

<span data-ttu-id="f415a-142">Ajoutez les classes suivantes à votre fichier IdentityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="f415a-142">Add the following classes to your IdentityModels.cs file.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a><span data-ttu-id="f415a-143">Modifier le Gestionnaire de contexte utilisateur et pour utiliser le type de clé</span><span class="sxs-lookup"><span data-stu-id="f415a-143">Change the context class and user manager to use the key type</span></span>

<span data-ttu-id="f415a-144">Dans IdentityModels.cs, modifiez la définition de la **ApplicationDbContext** classe à utiliser votre nouveau personnalisé des classes et une **int** pour la clé, comme indiqué dans le code en surbrillance.</span><span class="sxs-lookup"><span data-stu-id="f415a-144">In IdentityModels.cs, change the definition of the **ApplicationDbContext** class to use your new customized classes and an **int** for the key, as shown in the highlighted code.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

<span data-ttu-id="f415a-145">Le paramètre ThrowIfV1Schema n’est plus valide dans le constructeur.</span><span class="sxs-lookup"><span data-stu-id="f415a-145">The ThrowIfV1Schema parameter is no longer valid in the constructor.</span></span> <span data-ttu-id="f415a-146">Modifiez le constructeur afin qu’il ne passe pas une valeur ThrowIfV1Schema.</span><span class="sxs-lookup"><span data-stu-id="f415a-146">Change the constructor so it does not pass a ThrowIfV1Schema value.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

<span data-ttu-id="f415a-147">Ouvrez IdentityConfig.cs et modifiez le **ApplicationUserManger** classe pour la conservation des données de banque de classe à utiliser votre nouvel utilisateur et un **int** pour la clé.</span><span class="sxs-lookup"><span data-stu-id="f415a-147">Open IdentityConfig.cs, and change the **ApplicationUserManger** class to use your new user store class for persisting data and an **int** for the key.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

<span data-ttu-id="f415a-148">Dans le modèle de mise à jour 3, vous devez modifier la classe ApplicationSignInManager.</span><span class="sxs-lookup"><span data-stu-id="f415a-148">In the Update 3 template, you must change the ApplicationSignInManager class.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a><span data-ttu-id="f415a-149">Modifier la configuration de démarrage à utiliser le type de clé</span><span class="sxs-lookup"><span data-stu-id="f415a-149">Change start-up configuration to use the key type</span></span>

<span data-ttu-id="f415a-150">Dans Startup.Auth.cs, remplacez le code OnValidateIdentity, comme indiqué ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="f415a-150">In Startup.Auth.cs, replace the OnValidateIdentity code, as highlighted below.</span></span> <span data-ttu-id="f415a-151">Notez que la définition de getUserIdCallback, analyse la valeur de chaîne en entier.</span><span class="sxs-lookup"><span data-stu-id="f415a-151">Notice that the getUserIdCallback definition, parses the string value into an integer.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

<span data-ttu-id="f415a-152">Si votre projet ne reconnaît pas l’implémentation générique de la **GetUserId** (méthode), vous devrez peut-être mettre à jour le package NuGet d’identité ASP.NET à la version 2.1</span><span class="sxs-lookup"><span data-stu-id="f415a-152">If your project does not recognize the generic implementation of the **GetUserId** method, you may need to update the ASP.NET Identity NuGet package to version 2.1</span></span>

<span data-ttu-id="f415a-153">Vous avez effectué un grand nombre de modifications pour les classes d’infrastructure utilisées par ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="f415a-153">You have made a lot of changes to the infrastructure classes used by ASP.NET Identity.</span></span> <span data-ttu-id="f415a-154">Si vous essayez de compiler le projet, vous remarquerez un grand nombre d’erreurs.</span><span class="sxs-lookup"><span data-stu-id="f415a-154">If you try compiling the project, you will notice a lot of errors.</span></span> <span data-ttu-id="f415a-155">Heureusement, les erreurs restantes sont similaires.</span><span class="sxs-lookup"><span data-stu-id="f415a-155">Fortunately, the remaining errors are all similar.</span></span> <span data-ttu-id="f415a-156">La classe Identity attend un entier pour la clé, mais le contrôleur (ou un formulaire Web) est en passant une valeur de chaîne.</span><span class="sxs-lookup"><span data-stu-id="f415a-156">The Identity class expects an integer for the key, but the controller (or Web Form) is passing a string value.</span></span> <span data-ttu-id="f415a-157">Dans chaque cas, vous devez convertir à partir d’une chaîne et un entier en appelant **GetUserId&lt;int&gt;**.</span><span class="sxs-lookup"><span data-stu-id="f415a-157">In each case, you need to convert from a string to and integer by calling **GetUserId&lt;int&gt;**.</span></span> <span data-ttu-id="f415a-158">Vous pouvez parcourir la liste d’erreurs de compilation ou suivre les modifications ci-dessous.</span><span class="sxs-lookup"><span data-stu-id="f415a-158">You can either work through the error list from compilation or follow the changes below.</span></span>

<span data-ttu-id="f415a-159">Les modifications restantes varient selon le type de projet que vous créez et quelle mise à jour que vous avez installé dans Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f415a-159">The remaining changes depend on the type of project you are creating and which update you have installed in Visual Studio.</span></span> <span data-ttu-id="f415a-160">Vous pouvez accéder directement à la section appropriée via les liens suivants</span><span class="sxs-lookup"><span data-stu-id="f415a-160">You can go directly to the relevant section through the following links</span></span>

- [<span data-ttu-id="f415a-161">Pour MVC avec Update 2, modifiez le AccountController pour passer le type de clé</span><span class="sxs-lookup"><span data-stu-id="f415a-161">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="f415a-162">Pour MVC avec Update 3, modifier AccountController les ManageController pour passer le type de clé</span><span class="sxs-lookup"><span data-stu-id="f415a-162">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="f415a-163">Pour les formulaires Web avec Update 2, modifiez les pages de compte pour passer le type de clé</span><span class="sxs-lookup"><span data-stu-id="f415a-163">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="f415a-164">Pour les formulaires Web avec Update 3, modifiez les pages de compte pour passer le type de clé</span><span class="sxs-lookup"><span data-stu-id="f415a-164">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a><span data-ttu-id="f415a-165">Pour MVC avec Update 2, modifiez le AccountController pour passer le type de clé</span><span class="sxs-lookup"><span data-stu-id="f415a-165">For MVC with Update 2, change the AccountController to pass the key type</span></span>

<span data-ttu-id="f415a-166">Ouvrez le fichier AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="f415a-166">Open the AccountController.cs file.</span></span> <span data-ttu-id="f415a-167">Vous devez modifier les méthodes suivantes.</span><span class="sxs-lookup"><span data-stu-id="f415a-167">You need to change the following methods.</span></span>

<span data-ttu-id="f415a-168">**ConfirmEmail** (méthode)</span><span class="sxs-lookup"><span data-stu-id="f415a-168">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

<span data-ttu-id="f415a-169">**Dissocier** (méthode)</span><span class="sxs-lookup"><span data-stu-id="f415a-169">**Disassociate** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

<span data-ttu-id="f415a-170">**Manage(ManageUserViewModel)** (méthode)</span><span class="sxs-lookup"><span data-stu-id="f415a-170">**Manage(ManageUserViewModel)** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

<span data-ttu-id="f415a-171">**LinkLoginCallback** (méthode)</span><span class="sxs-lookup"><span data-stu-id="f415a-171">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

<span data-ttu-id="f415a-172">**RemoveAccountList** (méthode)</span><span class="sxs-lookup"><span data-stu-id="f415a-172">**RemoveAccountList** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

<span data-ttu-id="f415a-173">**HasPassword** (méthode)</span><span class="sxs-lookup"><span data-stu-id="f415a-173">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

<span data-ttu-id="f415a-174">Vous pouvez à présent [exécuter l’application](#run) et inscrire un nouvel utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f415a-174">You can now [run the application](#run) and register a new user.</span></span>

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a><span data-ttu-id="f415a-175">Pour MVC avec Update 3, modifier AccountController les ManageController pour passer le type de clé</span><span class="sxs-lookup"><span data-stu-id="f415a-175">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>

<span data-ttu-id="f415a-176">Ouvrez le fichier AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="f415a-176">Open the AccountController.cs file.</span></span> <span data-ttu-id="f415a-177">Vous devez modifier la méthode suivante.</span><span class="sxs-lookup"><span data-stu-id="f415a-177">You need to change the following method.</span></span>

<span data-ttu-id="f415a-178">**ConfirmEmail** (méthode)</span><span class="sxs-lookup"><span data-stu-id="f415a-178">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

<span data-ttu-id="f415a-179">**SendCode** (méthode)</span><span class="sxs-lookup"><span data-stu-id="f415a-179">**SendCode** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

<span data-ttu-id="f415a-180">Ouvrez le fichier ManageController.cs.</span><span class="sxs-lookup"><span data-stu-id="f415a-180">Open the ManageController.cs file.</span></span> <span data-ttu-id="f415a-181">Vous devez modifier les méthodes suivantes.</span><span class="sxs-lookup"><span data-stu-id="f415a-181">You need to change the following methods.</span></span>

<span data-ttu-id="f415a-182">**Index** (méthode)</span><span class="sxs-lookup"><span data-stu-id="f415a-182">**Index** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

<span data-ttu-id="f415a-183">**RemoveLogin** méthodes</span><span class="sxs-lookup"><span data-stu-id="f415a-183">**RemoveLogin** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

<span data-ttu-id="f415a-184">**AddPhoneNumber** (méthode)</span><span class="sxs-lookup"><span data-stu-id="f415a-184">**AddPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

<span data-ttu-id="f415a-185">**EnableTwoFactorAuthentication** (méthode)</span><span class="sxs-lookup"><span data-stu-id="f415a-185">**EnableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

<span data-ttu-id="f415a-186">**DisableTwoFactorAuthentication** (méthode)</span><span class="sxs-lookup"><span data-stu-id="f415a-186">**DisableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

<span data-ttu-id="f415a-187">**VerifyPhoneNumber** méthodes</span><span class="sxs-lookup"><span data-stu-id="f415a-187">**VerifyPhoneNumber** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

<span data-ttu-id="f415a-188">**RemovePhoneNumber** (méthode)</span><span class="sxs-lookup"><span data-stu-id="f415a-188">**RemovePhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

<span data-ttu-id="f415a-189">**ChangePassword** (méthode)</span><span class="sxs-lookup"><span data-stu-id="f415a-189">**ChangePassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

<span data-ttu-id="f415a-190">**SetPassword** (méthode)</span><span class="sxs-lookup"><span data-stu-id="f415a-190">**SetPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

<span data-ttu-id="f415a-191">**ManageLogins** (méthode)</span><span class="sxs-lookup"><span data-stu-id="f415a-191">**ManageLogins** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

<span data-ttu-id="f415a-192">**LinkLoginCallback** (méthode)</span><span class="sxs-lookup"><span data-stu-id="f415a-192">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

<span data-ttu-id="f415a-193">**HasPassword** (méthode)</span><span class="sxs-lookup"><span data-stu-id="f415a-193">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

<span data-ttu-id="f415a-194">**HasPhoneNumber** (méthode)</span><span class="sxs-lookup"><span data-stu-id="f415a-194">**HasPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

<span data-ttu-id="f415a-195">Vous pouvez à présent [exécuter l’application](#run) et inscrire un nouvel utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f415a-195">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="f415a-196">Pour les formulaires Web avec Update 2, modifiez les pages de compte pour passer le type de clé</span><span class="sxs-lookup"><span data-stu-id="f415a-196">For Web Forms with Update 2, change Account pages to pass the key type</span></span>

<span data-ttu-id="f415a-197">Pour les formulaires Web avec Update 2, vous devez modifier les pages suivantes.</span><span class="sxs-lookup"><span data-stu-id="f415a-197">For Web Forms with Update 2, you need to change the following pages.</span></span>

<span data-ttu-id="f415a-198">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="f415a-198">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

<span data-ttu-id="f415a-199">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="f415a-199">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

<span data-ttu-id="f415a-200">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="f415a-200">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

<span data-ttu-id="f415a-201">Vous pouvez à présent [exécuter l’application](#run) et inscrire un nouvel utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f415a-201">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="f415a-202">Pour les formulaires Web avec Update 3, modifiez les pages de compte pour passer le type de clé</span><span class="sxs-lookup"><span data-stu-id="f415a-202">For Web Forms with Update 3, change Account pages to pass the key type</span></span>

<span data-ttu-id="f415a-203">Pour les formulaires Web avec Update 3, vous devez modifier les pages suivantes.</span><span class="sxs-lookup"><span data-stu-id="f415a-203">For Web Forms with Update 3, you need to change the following pages.</span></span>

<span data-ttu-id="f415a-204">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="f415a-204">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

<span data-ttu-id="f415a-205">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="f415a-205">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

<span data-ttu-id="f415a-206">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="f415a-206">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

<span data-ttu-id="f415a-207">**VerifyPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="f415a-207">**VerifyPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

<span data-ttu-id="f415a-208">**AddPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="f415a-208">**AddPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

<span data-ttu-id="f415a-209">**ManagePassword.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="f415a-209">**ManagePassword.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

<span data-ttu-id="f415a-210">**ManageLogins.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="f415a-210">**ManageLogins.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

<span data-ttu-id="f415a-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="f415a-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a><span data-ttu-id="f415a-212">Exécuter l’application</span><span class="sxs-lookup"><span data-stu-id="f415a-212">Run application</span></span>

<span data-ttu-id="f415a-213">Vous avez terminé toutes les modifications nécessaires dans le modèle d’Application Web par défaut.</span><span class="sxs-lookup"><span data-stu-id="f415a-213">You have finished all of the required changes to the default Web Application template.</span></span> <span data-ttu-id="f415a-214">Exécutez l’application et inscrire un nouvel utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f415a-214">Run the application and register a new user.</span></span> <span data-ttu-id="f415a-215">Après avoir inscrit l’utilisateur, vous remarquerez que la table AspNetUsers possède une colonne d’Id qui est un entier.</span><span class="sxs-lookup"><span data-stu-id="f415a-215">After registering the user, you will notice that the AspNetUsers table has an Id column that is an integer.</span></span>

![nouvelle clé primaire](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

<span data-ttu-id="f415a-217">Si vous avez précédemment créé à l’identité ASP.NET tables avec une autre clé primaire, vous devez apporter des modifications supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="f415a-217">If you have previously created the ASP.NET Identity tables with a different primary key, you need to make some additional changes.</span></span> <span data-ttu-id="f415a-218">Si possible, supprimez simplement la base de données existante.</span><span class="sxs-lookup"><span data-stu-id="f415a-218">If possible, just delete the existing database.</span></span> <span data-ttu-id="f415a-219">La base de données sera recréée avec la conception correcte lorsque vous exécutez l’application web et ajoutez un nouvel utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f415a-219">The database will be re-created with the correct design when you run the web application and add a new user.</span></span> <span data-ttu-id="f415a-220">Si la suppression n’est pas possible, exécutez les migrations code first pour modifier les tables.</span><span class="sxs-lookup"><span data-stu-id="f415a-220">If deletion is not possible, run code first migrations to change the tables.</span></span> <span data-ttu-id="f415a-221">Toutefois, la nouvelle clé primaire entière ne sera pas être définie comme une propriété d’identité de SQL dans la base de données.</span><span class="sxs-lookup"><span data-stu-id="f415a-221">However, the new integer primary key will not be set up as a SQL IDENTITY property in the database.</span></span> <span data-ttu-id="f415a-222">Vous devez définir manuellement la colonne Id comme une identité.</span><span class="sxs-lookup"><span data-stu-id="f415a-222">You must manually set the Id column as an IDENTITY.</span></span>

<a id="other"></a>
## <a name="other-resources"></a><span data-ttu-id="f415a-223">Autres ressources</span><span class="sxs-lookup"><span data-stu-id="f415a-223">Other resources</span></span>

- [<span data-ttu-id="f415a-224">Vue d’ensemble des fournisseurs de stockage personnalisés pour ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="f415a-224">Overview of Custom Storage Providers for ASP.NET Identity</span></span>](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [<span data-ttu-id="f415a-225">Migration d’un site web existant de l’appartenance SQL vers ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="f415a-225">Migrating an Existing Website from SQL Membership to ASP.NET Identity</span></span>](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [<span data-ttu-id="f415a-226">Migration des données du fournisseur universel pour l’appartenance et les profils utilisateur vers ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="f415a-226">Migrating Universal Provider Data for Membership and User Profiles to ASP.NET Identity</span></span>](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- <span data-ttu-id="f415a-227">[Exemple d’application](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) avec la clé primaire modifiée</span><span class="sxs-lookup"><span data-stu-id="f415a-227">[Sample application](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) with changed primary key</span></span>
