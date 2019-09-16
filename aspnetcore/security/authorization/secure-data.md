---
title: Créer une application ASP.NET Core avec des données utilisateur protégées par une autorisation
author: rick-anderson
description: Découvrez comment créer une application Pages Razor avec des données utilisateur protégées par une autorisation. Inclut HTTPS, l’authentification, sécurité, ASP.NET Core Identity.
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: d95f44394d6ecc3c3896b45c5bebc73fa2d92445
ms.sourcegitcommit: dc5b293e08336dc236de66ed1834f7ef78359531
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71011186"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="d6d0a-104">Créer une application ASP.NET Core avec des données utilisateur protégées par une autorisation</span><span class="sxs-lookup"><span data-stu-id="d6d0a-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="d6d0a-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="d6d0a-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="d6d0a-106">Consultez [ce fichier PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) pour la version d’ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-106">See [this PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="d6d0a-107">La version d’ASP.NET Core 1.1 de ce didacticiel est dans [cela](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) dossier.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-107">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="d6d0a-108">L’exemple ASP.NET Core 1.1 du [exemples](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="d6d0a-108">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="d6d0a-109">Consultez [ce fichier pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="d6d0a-109">See [this pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d6d0a-110">Ce didacticiel montre comment créer une application web ASP.NET Core avec des données utilisateur protégées par une autorisation.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="d6d0a-111">Il affiche une liste de contacts (inscrits) les utilisateurs authentifiés ont créés.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="d6d0a-112">Il existe trois groupes de sécurité :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-112">There are three security groups:</span></span>

* <span data-ttu-id="d6d0a-113">**Utilisateurs inscrits** peut afficher toutes les données approuvées et peuvent modifier ou supprimer leurs propres données.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="d6d0a-114">**Gestionnaires de** peuvent approuver ou rejeter des données de contact.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="d6d0a-115">Seuls les contacts approuvés sont visibles aux utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="d6d0a-116">**Les administrateurs** peut approuver/rejeter et modifier ou de supprimer toutes les données.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="d6d0a-117">Les images de ce document ne correspondent pas exactement aux modèles les plus récents.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-117">The images in this document don't exactly match the latest templates.</span></span>

<span data-ttu-id="d6d0a-118">Dans l’image suivante, l’utilisateur Rick (`rick@example.com`) n’est connecté.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-118">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="d6d0a-119">Rick peut uniquement afficher les contacts approuvés et **modifier**/**supprimer**/**créer un nouveau** liens pour ses contacts.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-119">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="d6d0a-120">Seul le dernier enregistrement créé par Rick, affiche **modifier** et **supprimer** des liens.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-120">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="d6d0a-121">Autres utilisateurs ne voient le dernier enregistrement jusqu'à ce qu’un gestionnaire ou un administrateur modifie le statut « Approved ».</span><span class="sxs-lookup"><span data-stu-id="d6d0a-121">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Capture d’écran montrant Rick connecté](secure-data/_static/rick.png)

<span data-ttu-id="d6d0a-123">Dans l’image suivante, `manager@contoso.com` est connecté et dans le rôle du responsable :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-123">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Capture d’écran manager@contoso.com connecté](secure-data/_static/manager1.png)

<span data-ttu-id="d6d0a-125">L’illustration suivante montre les gestionnaires de vue des détails d’un contact :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-125">The following image shows the managers details view of a contact:</span></span>

![Vue du responsable d’un contact](secure-data/_static/manager.png)

<span data-ttu-id="d6d0a-127">Le **approuver** et **rejeter** boutons sont affichés uniquement pour les responsables et les administrateurs.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-127">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="d6d0a-128">Dans l’image suivante, `admin@contoso.com` est connecté et dans le rôle de l’administrateur :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-128">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Capture d’écran admin@contoso.com connecté](secure-data/_static/admin.png)

<span data-ttu-id="d6d0a-130">L’administrateur a tous les privilèges.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-130">The administrator has all privileges.</span></span> <span data-ttu-id="d6d0a-131">Elle peut lire/modifier/supprimer un contact et modifier l’état de contacts.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-131">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="d6d0a-132">L’application a été créée par [la structure](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) suit `Contact` modèle :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-132">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="d6d0a-133">L’exemple contient les gestionnaires d’autorisation suivants :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-133">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="d6d0a-134">`ContactIsOwnerAuthorizationHandler`: Garantit qu’un utilisateur peut uniquement modifier ses données.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-134">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="d6d0a-135">`ContactManagerAuthorizationHandler`: Permet aux gestionnaires d’approuver ou de rejeter les contacts.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-135">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="d6d0a-136">`ContactAdministratorsAuthorizationHandler`: Permet aux administrateurs d’approuver ou de refuser des contacts, ainsi que de modifier/supprimer des contacts.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-136">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d6d0a-137">Prérequis</span><span class="sxs-lookup"><span data-stu-id="d6d0a-137">Prerequisites</span></span>

<span data-ttu-id="d6d0a-138">Ce didacticiel est avancé.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-138">This tutorial is advanced.</span></span> <span data-ttu-id="d6d0a-139">Vous devez être familiarisé avec :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-139">You should be familiar with:</span></span>

* [<span data-ttu-id="d6d0a-140">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d6d0a-140">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="d6d0a-141">Authentification</span><span class="sxs-lookup"><span data-stu-id="d6d0a-141">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="d6d0a-142">Confirmation de compte et récupération de mot de passe</span><span class="sxs-lookup"><span data-stu-id="d6d0a-142">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="d6d0a-143">Autorisation</span><span class="sxs-lookup"><span data-stu-id="d6d0a-143">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="d6d0a-144">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="d6d0a-144">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="d6d0a-145">Le démarrage et l’application terminée</span><span class="sxs-lookup"><span data-stu-id="d6d0a-145">The starter and completed app</span></span>

<span data-ttu-id="d6d0a-146">[Télécharger](xref:index#how-to-download-a-sample) le [terminé](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) application.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-146">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="d6d0a-147">[Test](#test-the-completed-app) l’application terminée afin de vous familiariser avec ses fonctionnalités de sécurité.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-147">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="d6d0a-148">L’application de démarrage</span><span class="sxs-lookup"><span data-stu-id="d6d0a-148">The starter app</span></span>

<span data-ttu-id="d6d0a-149">[Télécharger](xref:index#how-to-download-a-sample) le [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) application.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-149">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="d6d0a-150">Exécutez l’application, appuyez sur la **ContactManager** lien et vérifiez que vous pouvez créer, modifier et supprimer un contact.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-150">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="d6d0a-151">Sécuriser les données utilisateur</span><span class="sxs-lookup"><span data-stu-id="d6d0a-151">Secure user data</span></span>

<span data-ttu-id="d6d0a-152">Les sections suivantes ont toutes les principales étapes pour créer l’application de données utilisateur sécurisée.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-152">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="d6d0a-153">Il peut s’avérer utile pour faire référence au projet terminé.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-153">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="d6d0a-154">Lier les données de contact à l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="d6d0a-154">Tie the contact data to the user</span></span>

<span data-ttu-id="d6d0a-155">Utilisez ASP.NET [identité](xref:security/authentication/identity) ID d’utilisateur pour garantir les utilisateurs permettre modifier leurs données, mais pas d’autres données utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-155">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="d6d0a-156">Ajouter `OwnerID` et `ContactStatus` à la `Contact` modèle :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-156">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="d6d0a-157">`OwnerID` est l’ID d’utilisateur à partir de la `AspNetUser` table dans le [identité](xref:security/authentication/identity) base de données.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-157">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="d6d0a-158">Le `Status` champ détermine si un contact est visible par les utilisateurs généraux.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-158">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="d6d0a-159">Créer une nouvelle migration et mettre à jour de la base de données :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-159">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="d6d0a-160">Ajouter des services de rôle à l’identité</span><span class="sxs-lookup"><span data-stu-id="d6d0a-160">Add Role services to Identity</span></span>

<span data-ttu-id="d6d0a-161">Ajouter [fonctionnalité Ajouter des rôles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) pour ajouter des services de rôle :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-161">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a><span data-ttu-id="d6d0a-162">Demander aux utilisateurs authentifiés</span><span class="sxs-lookup"><span data-stu-id="d6d0a-162">Require authenticated users</span></span>

<span data-ttu-id="d6d0a-163">Définir la stratégie d’authentification par défaut pour exiger l’authentification des utilisateurs :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-163">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 <span data-ttu-id="d6d0a-164">Vous pouvez refuser l’authentification au niveau de la méthode Page Razor, de contrôleur ou d’action avec la `[AllowAnonymous]` attribut.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-164">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="d6d0a-165">Définition de la stratégie d’authentification par défaut pour les utilisateurs doivent être authentifiés protège nouvellement ajouté les Pages Razor et les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-165">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="d6d0a-166">Avec l’authentification requise par défaut est plus sécurisée que de s’appuyer sur de nouveaux contrôleurs et les Pages Razor pour inclure le `[Authorize]` attribut.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-166">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="d6d0a-167">Ajoutez [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) aux pages d’index et de confidentialité afin que les utilisateurs anonymes puissent obtenir des informations sur le site avant de s’inscrire.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-167">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index and Privacy pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a><span data-ttu-id="d6d0a-168">Configurer le compte de test</span><span class="sxs-lookup"><span data-stu-id="d6d0a-168">Configure the test account</span></span>

<span data-ttu-id="d6d0a-169">Le `SeedData` classe crée deux comptes : administrateur et gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-169">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="d6d0a-170">Utilisez le [outil Secret Manager](xref:security/app-secrets) pour définir un mot de passe pour ces comptes.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-170">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="d6d0a-171">Définir le mot de passe à partir du répertoire de projet (le répertoire contenant *Program.cs*) :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-171">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="d6d0a-172">Si un mot de passe n’est pas spécifié, une exception est levée lorsque `SeedData.Initialize` est appelée.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-172">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="d6d0a-173">Mise à jour `Main` à utiliser le mot de passe de test :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-173">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="d6d0a-174">Créez les comptes de test et de mettre à jour les contacts</span><span class="sxs-lookup"><span data-stu-id="d6d0a-174">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="d6d0a-175">Mise à jour le `Initialize` méthode dans la `SeedData` classe pour créer les comptes de test :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-175">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="d6d0a-176">Ajouter l’ID d’utilisateur administrateur et `ContactStatus` aux contacts.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-176">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="d6d0a-177">Rendre des contacts « Envoyé » et un « rejeté ».</span><span class="sxs-lookup"><span data-stu-id="d6d0a-177">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="d6d0a-178">Ajoutez l’ID d’utilisateur et l’état pour tous les contacts.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-178">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="d6d0a-179">Un seul contact est affiché :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-179">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="d6d0a-180">Créer le propriétaire, le gestionnaire et gestionnaires d’autorisations d’administrateur</span><span class="sxs-lookup"><span data-stu-id="d6d0a-180">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="d6d0a-181">Créer un `ContactIsOwnerAuthorizationHandler` classe dans le *autorisation* dossier.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-181">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="d6d0a-182">Le `ContactIsOwnerAuthorizationHandler` vérifie que l’utilisateur agissant sur une ressource propriétaire de la ressource.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-182">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="d6d0a-183">Le `ContactIsOwnerAuthorizationHandler` appels [contexte. Réussir](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) si l’utilisateur authentifié actuel est le propriétaire du contact.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-183">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="d6d0a-184">Les gestionnaires d’autorisation généralement :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-184">Authorization handlers generally:</span></span>

* <span data-ttu-id="d6d0a-185">Retourner `context.Succeed` lorsque les conditions sont remplies.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-185">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="d6d0a-186">Retourner `Task.CompletedTask` lorsque les conditions ne sont pas remplies.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-186">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="d6d0a-187">`Task.CompletedTask`n’est pas un succès&mdash;ou un échec. il permet l’exécution d’autres gestionnaires d’autorisations.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-187">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="d6d0a-188">Si vous devez explicitement échouer, retourner [contexte. Échec](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="d6d0a-188">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="d6d0a-189">L’application permet aux propriétaires de contact de modifier, supprimer ou créer leurs propres données.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-189">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="d6d0a-190">`ContactIsOwnerAuthorizationHandler` n’a pas besoin vérifier l’opération passée dans le paramètre de condition.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-190">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="d6d0a-191">Créer un gestionnaire d’autorisation de gestionnaire</span><span class="sxs-lookup"><span data-stu-id="d6d0a-191">Create a manager authorization handler</span></span>

<span data-ttu-id="d6d0a-192">Créer un `ContactManagerAuthorizationHandler` classe dans le *autorisation* dossier.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-192">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="d6d0a-193">Le `ContactManagerAuthorizationHandler` vérifie l’utilisateur agissant sur la ressource est un gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-193">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="d6d0a-194">Seuls les responsables peuvent approuver ou rejeter les modifications de contenu (nouvelle ou modifiées).</span><span class="sxs-lookup"><span data-stu-id="d6d0a-194">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="d6d0a-195">Créer un gestionnaire d’autorisations d’administrateur</span><span class="sxs-lookup"><span data-stu-id="d6d0a-195">Create an administrator authorization handler</span></span>

<span data-ttu-id="d6d0a-196">Créer un `ContactAdministratorsAuthorizationHandler` classe dans le *autorisation* dossier.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-196">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="d6d0a-197">Le `ContactAdministratorsAuthorizationHandler` vérifie que l’utilisateur agissant sur la ressource est un administrateur.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-197">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="d6d0a-198">Administrateur peut effectuer toutes les opérations.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-198">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="d6d0a-199">Inscrire les gestionnaires d’autorisation</span><span class="sxs-lookup"><span data-stu-id="d6d0a-199">Register the authorization handlers</span></span>

<span data-ttu-id="d6d0a-200">Services à l’aide d’Entity Framework Core doivent être inscrit pour [l’injection de dépendances](xref:fundamentals/dependency-injection) à l’aide de [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="d6d0a-200">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="d6d0a-201">Le `ContactIsOwnerAuthorizationHandler` utilise ASP.NET Core [identité](xref:security/authentication/identity), qui est basé sur Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-201">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="d6d0a-202">Enregistrer les gestionnaires avec la collection de service afin qu’elles soient disponibles pour le `ContactsController` via [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d6d0a-202">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d6d0a-203">Ajoutez le code suivant à la fin de `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d6d0a-203">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

<span data-ttu-id="d6d0a-204">`ContactAdministratorsAuthorizationHandler` et `ContactManagerAuthorizationHandler` sont ajoutés en tant que singletons.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-204">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="d6d0a-205">Ils sont des singletons, car ils n’utilisent pas EF et toutes les informations nécessaires sont dans le `Context` paramètre de la `HandleRequirementAsync` (méthode).</span><span class="sxs-lookup"><span data-stu-id="d6d0a-205">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="d6d0a-206">Autorisation de la prise en charge</span><span class="sxs-lookup"><span data-stu-id="d6d0a-206">Support authorization</span></span>

<span data-ttu-id="d6d0a-207">Dans cette section, vous mettez à jour les Pages Razor et ajoutez une classe de configuration requise des opérations.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-207">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="d6d0a-208">Passez en revue la classe de configuration requise de contact des opérations</span><span class="sxs-lookup"><span data-stu-id="d6d0a-208">Review the contact operations requirements class</span></span>

<span data-ttu-id="d6d0a-209">Examinez la `ContactOperations` classe.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-209">Review the `ContactOperations` class.</span></span> <span data-ttu-id="d6d0a-210">Cette classe contient les exigences de l’application prend en charge :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-210">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="d6d0a-211">Créer une classe de base pour les Pages Razor de Contacts</span><span class="sxs-lookup"><span data-stu-id="d6d0a-211">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="d6d0a-212">Créer une classe de base qui contient les services utilisés dans les Pages Razor de contacts.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-212">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="d6d0a-213">La classe de base place le code d’initialisation dans un emplacement :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-213">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="d6d0a-214">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-214">The preceding code:</span></span>

* <span data-ttu-id="d6d0a-215">Ajoute le `IAuthorizationService` service d’accéder aux gestionnaires d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-215">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="d6d0a-216">Ajoute l’identité `UserManager` service.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-216">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="d6d0a-217">Ajoutez la `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-217">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="d6d0a-218">Mettre à jour le CreateModel</span><span class="sxs-lookup"><span data-stu-id="d6d0a-218">Update the CreateModel</span></span>

<span data-ttu-id="d6d0a-219">Mettre à jour le constructeur de modèle de page de création à utiliser le `DI_BasePageModel` classe de base :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-219">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="d6d0a-220">Mise à jour le `CreateModel.OnPostAsync` méthode à :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-220">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="d6d0a-221">Ajoutez l’ID utilisateur pour la `Contact` modèle.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-221">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="d6d0a-222">Appeler le Gestionnaire d’autorisation pour vérifier que l’utilisateur est autorisé à créer des contacts.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-222">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="d6d0a-223">Mettre à jour le IndexModel</span><span class="sxs-lookup"><span data-stu-id="d6d0a-223">Update the IndexModel</span></span>

<span data-ttu-id="d6d0a-224">Mise à jour le `OnGetAsync` méthode approuvées uniquement les contacts sont présentés aux utilisateurs généraux :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-224">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="d6d0a-225">Mettre à jour le EditModel</span><span class="sxs-lookup"><span data-stu-id="d6d0a-225">Update the EditModel</span></span>

<span data-ttu-id="d6d0a-226">Ajoutez un gestionnaire d’autorisation pour vérifier que l’utilisateur propriétaire du contact.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-226">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="d6d0a-227">Étant donné que l’autorisation de ressource est en cours de validation, le `[Authorize]` attribut n’est pas suffisant.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-227">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="d6d0a-228">L’application n’a pas accès à la ressource lors de l’évaluation des attributs.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-228">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="d6d0a-229">Autorisation basée sur la ressource doit être impérative.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-229">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="d6d0a-230">Vérifications doivent être effectuées une fois que l’application a accès à la ressource, en le chargeant dans le modèle de page ou en le chargeant dans le gestionnaire lui-même.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-230">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="d6d0a-231">Vous accédez fréquemment la ressource en passant la clé de ressource.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-231">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="d6d0a-232">Mettre à jour le DeleteModel</span><span class="sxs-lookup"><span data-stu-id="d6d0a-232">Update the DeleteModel</span></span>

<span data-ttu-id="d6d0a-233">Mettre à jour le modèle de page delete pour utiliser le Gestionnaire d’autorisation pour vérifier que l’utilisateur a l’autorisation de suppression sur le contact.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-233">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="d6d0a-234">Injecter le service d’autorisation dans les vues</span><span class="sxs-lookup"><span data-stu-id="d6d0a-234">Inject the authorization service into the views</span></span>

<span data-ttu-id="d6d0a-235">Actuellement, le montre l’interface utilisateur modifie et supprime des liens pour les contacts de que l’utilisateur ne peut pas modifier.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-235">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="d6d0a-236">Injecter le service d’autorisation dans le fichier *pages/_ViewImports. cshtml* afin qu’il soit disponible pour tous les affichages :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-236">Inject the authorization service in the *Pages/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="d6d0a-237">Le balisage précédent ajoute plusieurs `using` instructions.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-237">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="d6d0a-238">Mise à jour le **modifier** et **supprimer** lie dans *Pages/Contacts/Index.cshtml* afin d’être affichées uniquement pour les utilisateurs disposant des autorisations appropriées :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-238">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="d6d0a-239">Masquage des liens à partir des utilisateurs qui ne sont pas autorisés à modifier les données ne sécuriser l’application.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-239">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="d6d0a-240">Masquage des liens rend l’application plus conviviale en affichant les liens ne sont valides.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-240">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="d6d0a-241">Les utilisateurs peuvent hack l’URL générées pour appeler modifier et supprimer des opérations sur les données qu’ils ne possèdent pas.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-241">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="d6d0a-242">Le contrôleur ou une Page Razor doit appliquer les vérifications d’accès pour sécuriser les données.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-242">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="d6d0a-243">Détails de la mise à jour</span><span class="sxs-lookup"><span data-stu-id="d6d0a-243">Update Details</span></span>

<span data-ttu-id="d6d0a-244">Mettre à jour l’affichage des détails pour les responsables peuvent approuver ou rejeter des contacts :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-244">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="d6d0a-245">Mettre à jour le modèle de page de détails :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-245">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="d6d0a-246">Ajouter ou supprimer un utilisateur à un rôle</span><span class="sxs-lookup"><span data-stu-id="d6d0a-246">Add or remove a user to a role</span></span>

<span data-ttu-id="d6d0a-247">Consultez [ce problème](https://github.com/aspnet/AspNetCore.Docs/issues/8502) pour plus d’informations sur :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-247">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="d6d0a-248">Suppression de privilèges à partir d’un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-248">Removing privileges from a user.</span></span> <span data-ttu-id="d6d0a-249">Par exemple, la désactivation d’un utilisateur dans une application de conversation.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-249">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="d6d0a-250">Ajout des privilèges à un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-250">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="d6d0a-251">Tester l’application terminée</span><span class="sxs-lookup"><span data-stu-id="d6d0a-251">Test the completed app</span></span>

<span data-ttu-id="d6d0a-252">Si vous n’avez pas déjà défini un mot de passe pour les comptes d’utilisateur amorcée, utilisez le [outil Secret Manager](xref:security/app-secrets#secret-manager) pour définir un mot de passe :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-252">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="d6d0a-253">Choisir un mot de passe fort : Utilisez au moins huit caractères et au moins un caractère majuscule, un chiffre et un symbole.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-253">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="d6d0a-254">Par exemple, `Passw0rd!` répond aux exigences de mot de passe fort.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-254">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="d6d0a-255">Exécutez la commande suivante à partir du dossier du projet, où `<PW>` est le mot de passe :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-255">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

<span data-ttu-id="d6d0a-256">Si l’application a des contacts :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-256">If the app has contacts:</span></span>

* <span data-ttu-id="d6d0a-257">Supprimer tous les enregistrements dans la `Contact` table.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-257">Delete all of the records in the `Contact` table.</span></span>
* <span data-ttu-id="d6d0a-258">Redémarrez l’application pour amorcer la base de données.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-258">Restart the app to seed the database.</span></span>

<span data-ttu-id="d6d0a-259">Un moyen simple de tester l’application terminée consiste à lancer les trois différents navigateurs (ou incognito/InPrivate sessions).</span><span class="sxs-lookup"><span data-stu-id="d6d0a-259">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="d6d0a-260">Dans un navigateur, inscrire un nouvel utilisateur (par exemple, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="d6d0a-260">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="d6d0a-261">Connectez-vous à chaque navigateur avec un autre utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-261">Sign in to each browser with a different user.</span></span> <span data-ttu-id="d6d0a-262">Vérifiez les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-262">Verify the following operations:</span></span>

* <span data-ttu-id="d6d0a-263">Les utilisateurs inscrits peuvent afficher toutes les données de contact approuvées.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-263">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="d6d0a-264">Les utilisateurs inscrits peuvent modifier ou supprimer leurs propres données.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-264">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="d6d0a-265">Gestionnaires peuvent approuver/rejeter les données de contact.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-265">Managers can approve/reject contact data.</span></span> <span data-ttu-id="d6d0a-266">Le `Details` afficher montre **approuver** et **rejeter** boutons.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-266">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="d6d0a-267">Les administrateurs peuvent approuver/rejeter et modifier ou de supprimer toutes les données.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-267">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="d6d0a-268">Utilisateur</span><span class="sxs-lookup"><span data-stu-id="d6d0a-268">User</span></span>                | <span data-ttu-id="d6d0a-269">Amorcée par l’application</span><span class="sxs-lookup"><span data-stu-id="d6d0a-269">Seeded by the app</span></span> | <span data-ttu-id="d6d0a-270">Options</span><span class="sxs-lookup"><span data-stu-id="d6d0a-270">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="d6d0a-271">Non</span><span class="sxs-lookup"><span data-stu-id="d6d0a-271">No</span></span>                | <span data-ttu-id="d6d0a-272">Modifier/supprimer les données propres.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-272">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="d6d0a-273">Oui</span><span class="sxs-lookup"><span data-stu-id="d6d0a-273">Yes</span></span>               | <span data-ttu-id="d6d0a-274">Approuver/rejeter et modifier ou de supprimer des données propres.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-274">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="d6d0a-275">Oui</span><span class="sxs-lookup"><span data-stu-id="d6d0a-275">Yes</span></span>               | <span data-ttu-id="d6d0a-276">Approuver/rejeter et de modifier ou de supprimer toutes les données.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-276">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="d6d0a-277">Créer un contact dans le navigateur de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-277">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="d6d0a-278">Copiez l’URL pour supprimer et modifier à partir du contact de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-278">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="d6d0a-279">Collez ces liens dans le navigateur de l’utilisateur de test pour vérifier que l’utilisateur de test ne peut pas effectuer ces opérations.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-279">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="d6d0a-280">Créer l’application de démarrage</span><span class="sxs-lookup"><span data-stu-id="d6d0a-280">Create the starter app</span></span>

* <span data-ttu-id="d6d0a-281">Créer une application Pages Razor nommée « ContactManager »</span><span class="sxs-lookup"><span data-stu-id="d6d0a-281">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="d6d0a-282">Créer l’application avec **comptes d’utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-282">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="d6d0a-283">Nommez-Le « ContactManager » afin de l’espace de noms correspond à l’espace de noms utilisé dans l’exemple.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-283">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="d6d0a-284">`-uld` Spécifie la base de données locale au lieu de SQLite</span><span class="sxs-lookup"><span data-stu-id="d6d0a-284">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="d6d0a-285">Ajouter des *modèles/contact. cs*:</span><span class="sxs-lookup"><span data-stu-id="d6d0a-285">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="d6d0a-286">Une structure le `Contact` modèle.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-286">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="d6d0a-287">La migration initiale de créer et mettre à jour de la base de données :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-287">Create initial migration and update the database:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
  ```

<span data-ttu-id="d6d0a-288">Si vous rencontrez un bogue avec `dotnet aspnet-codegenerator razorpage` la commande, consultez [ce problème GitHub](https://github.com/aspnet/Scaffolding/issues/984).</span><span class="sxs-lookup"><span data-stu-id="d6d0a-288">If you experience a bug with the `dotnet aspnet-codegenerator razorpage` command, see [this GitHub issue](https://github.com/aspnet/Scaffolding/issues/984).</span></span>

* <span data-ttu-id="d6d0a-289">Mettez à jour l’ancre **ContactManager** dans le fichier *pages/Shared/_ Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-289">Update the **ContactManager** anchor in the *Pages/Shared/_Layout.cshtml* file:</span></span>

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* <span data-ttu-id="d6d0a-290">Testez l’application en création, modification et suppression d’un contact</span><span class="sxs-lookup"><span data-stu-id="d6d0a-290">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="d6d0a-291">Amorcer la base de données</span><span class="sxs-lookup"><span data-stu-id="d6d0a-291">Seed the database</span></span>

<span data-ttu-id="d6d0a-292">Ajoutez la classe [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) au dossier *Data* :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-292">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) class to the *Data* folder:</span></span>

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

<span data-ttu-id="d6d0a-293">Appelez `SeedData.Initialize` de `Main`:</span><span class="sxs-lookup"><span data-stu-id="d6d0a-293">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

<span data-ttu-id="d6d0a-294">Tester l’application d’amorçage la base de données.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-294">Test that the app seeded the database.</span></span> <span data-ttu-id="d6d0a-295">S’il existe des lignes dans la base de données de contact, la méthode seed ne s’exécute pas.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-295">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

<span data-ttu-id="d6d0a-296">Ce didacticiel montre comment créer une application web ASP.NET Core avec des données utilisateur protégées par une autorisation.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-296">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="d6d0a-297">Il affiche une liste de contacts (inscrits) les utilisateurs authentifiés ont créés.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-297">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="d6d0a-298">Il existe trois groupes de sécurité :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-298">There are three security groups:</span></span>

* <span data-ttu-id="d6d0a-299">**Utilisateurs inscrits** peut afficher toutes les données approuvées et peuvent modifier ou supprimer leurs propres données.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-299">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="d6d0a-300">**Gestionnaires de** peuvent approuver ou rejeter des données de contact.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-300">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="d6d0a-301">Seuls les contacts approuvés sont visibles aux utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-301">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="d6d0a-302">**Les administrateurs** peut approuver/rejeter et modifier ou de supprimer toutes les données.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-302">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="d6d0a-303">Dans l’image suivante, l’utilisateur Rick (`rick@example.com`) n’est connecté.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-303">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="d6d0a-304">Rick peut uniquement afficher les contacts approuvés et **modifier**/**supprimer**/**créer un nouveau** liens pour ses contacts.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-304">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="d6d0a-305">Seul le dernier enregistrement créé par Rick, affiche **modifier** et **supprimer** des liens.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-305">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="d6d0a-306">Autres utilisateurs ne voient le dernier enregistrement jusqu'à ce qu’un gestionnaire ou un administrateur modifie le statut « Approved ».</span><span class="sxs-lookup"><span data-stu-id="d6d0a-306">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Capture d’écran montrant Rick connecté](secure-data/_static/rick.png)

<span data-ttu-id="d6d0a-308">Dans l’image suivante, `manager@contoso.com` est connecté et dans le rôle du responsable :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-308">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Capture d’écran manager@contoso.com connecté](secure-data/_static/manager1.png)

<span data-ttu-id="d6d0a-310">L’illustration suivante montre les gestionnaires de vue des détails d’un contact :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-310">The following image shows the managers details view of a contact:</span></span>

![Vue du responsable d’un contact](secure-data/_static/manager.png)

<span data-ttu-id="d6d0a-312">Le **approuver** et **rejeter** boutons sont affichés uniquement pour les responsables et les administrateurs.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-312">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="d6d0a-313">Dans l’image suivante, `admin@contoso.com` est connecté et dans le rôle de l’administrateur :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-313">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Capture d’écran admin@contoso.com connecté](secure-data/_static/admin.png)

<span data-ttu-id="d6d0a-315">L’administrateur a tous les privilèges.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-315">The administrator has all privileges.</span></span> <span data-ttu-id="d6d0a-316">Elle peut lire/modifier/supprimer un contact et modifier l’état de contacts.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-316">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="d6d0a-317">L’application a été créée par [la structure](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) suit `Contact` modèle :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-317">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="d6d0a-318">L’exemple contient les gestionnaires d’autorisation suivants :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-318">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="d6d0a-319">`ContactIsOwnerAuthorizationHandler`: Garantit qu’un utilisateur peut uniquement modifier ses données.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-319">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="d6d0a-320">`ContactManagerAuthorizationHandler`: Permet aux gestionnaires d’approuver ou de rejeter les contacts.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-320">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="d6d0a-321">`ContactAdministratorsAuthorizationHandler`: Permet aux administrateurs d’approuver ou de refuser des contacts, ainsi que de modifier/supprimer des contacts.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-321">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d6d0a-322">Prérequis</span><span class="sxs-lookup"><span data-stu-id="d6d0a-322">Prerequisites</span></span>

<span data-ttu-id="d6d0a-323">Ce didacticiel est avancé.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-323">This tutorial is advanced.</span></span> <span data-ttu-id="d6d0a-324">Vous devez être familiarisé avec :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-324">You should be familiar with:</span></span>

* [<span data-ttu-id="d6d0a-325">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d6d0a-325">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="d6d0a-326">Authentification</span><span class="sxs-lookup"><span data-stu-id="d6d0a-326">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="d6d0a-327">Confirmation de compte et récupération de mot de passe</span><span class="sxs-lookup"><span data-stu-id="d6d0a-327">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="d6d0a-328">Autorisation</span><span class="sxs-lookup"><span data-stu-id="d6d0a-328">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="d6d0a-329">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="d6d0a-329">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="d6d0a-330">Le démarrage et l’application terminée</span><span class="sxs-lookup"><span data-stu-id="d6d0a-330">The starter and completed app</span></span>

<span data-ttu-id="d6d0a-331">[Télécharger](xref:index#how-to-download-a-sample) le [terminé](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) application.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-331">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="d6d0a-332">[Test](#test-the-completed-app) l’application terminée afin de vous familiariser avec ses fonctionnalités de sécurité.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-332">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="d6d0a-333">L’application de démarrage</span><span class="sxs-lookup"><span data-stu-id="d6d0a-333">The starter app</span></span>

<span data-ttu-id="d6d0a-334">[Télécharger](xref:index#how-to-download-a-sample) le [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) application.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-334">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="d6d0a-335">Exécutez l’application, appuyez sur la **ContactManager** lien et vérifiez que vous pouvez créer, modifier et supprimer un contact.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-335">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="d6d0a-336">Sécuriser les données utilisateur</span><span class="sxs-lookup"><span data-stu-id="d6d0a-336">Secure user data</span></span>

<span data-ttu-id="d6d0a-337">Les sections suivantes ont toutes les principales étapes pour créer l’application de données utilisateur sécurisée.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-337">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="d6d0a-338">Il peut s’avérer utile pour faire référence au projet terminé.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-338">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="d6d0a-339">Lier les données de contact à l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="d6d0a-339">Tie the contact data to the user</span></span>

<span data-ttu-id="d6d0a-340">Utilisez ASP.NET [identité](xref:security/authentication/identity) ID d’utilisateur pour garantir les utilisateurs permettre modifier leurs données, mais pas d’autres données utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-340">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="d6d0a-341">Ajouter `OwnerID` et `ContactStatus` à la `Contact` modèle :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-341">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="d6d0a-342">`OwnerID` est l’ID d’utilisateur à partir de la `AspNetUser` table dans le [identité](xref:security/authentication/identity) base de données.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-342">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="d6d0a-343">Le `Status` champ détermine si un contact est visible par les utilisateurs généraux.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-343">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="d6d0a-344">Créer une nouvelle migration et mettre à jour de la base de données :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-344">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="d6d0a-345">Ajouter des services de rôle à l’identité</span><span class="sxs-lookup"><span data-stu-id="d6d0a-345">Add Role services to Identity</span></span>

<span data-ttu-id="d6d0a-346">Ajouter [fonctionnalité Ajouter des rôles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) pour ajouter des services de rôle :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-346">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="d6d0a-347">Demander aux utilisateurs authentifiés</span><span class="sxs-lookup"><span data-stu-id="d6d0a-347">Require authenticated users</span></span>

<span data-ttu-id="d6d0a-348">Définir la stratégie d’authentification par défaut pour exiger l’authentification des utilisateurs :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-348">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="d6d0a-349">Vous pouvez refuser l’authentification au niveau de la méthode Page Razor, de contrôleur ou d’action avec la `[AllowAnonymous]` attribut.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-349">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="d6d0a-350">Définition de la stratégie d’authentification par défaut pour les utilisateurs doivent être authentifiés protège nouvellement ajouté les Pages Razor et les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-350">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="d6d0a-351">Avec l’authentification requise par défaut est plus sécurisée que de s’appuyer sur de nouveaux contrôleurs et les Pages Razor pour inclure le `[Authorize]` attribut.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-351">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="d6d0a-352">Ajouter [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) à l’Index, des pages sur et Contact pour les utilisateurs anonymes peuvent obtenir des informations sur le site avant ils s’inscrivent.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-352">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="d6d0a-353">Configurer le compte de test</span><span class="sxs-lookup"><span data-stu-id="d6d0a-353">Configure the test account</span></span>

<span data-ttu-id="d6d0a-354">Le `SeedData` classe crée deux comptes : administrateur et gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-354">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="d6d0a-355">Utilisez le [outil Secret Manager](xref:security/app-secrets) pour définir un mot de passe pour ces comptes.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-355">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="d6d0a-356">Définir le mot de passe à partir du répertoire de projet (le répertoire contenant *Program.cs*) :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-356">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="d6d0a-357">Si un mot de passe n’est pas spécifié, une exception est levée lorsque `SeedData.Initialize` est appelée.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-357">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="d6d0a-358">Mise à jour `Main` à utiliser le mot de passe de test :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-358">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="d6d0a-359">Créez les comptes de test et de mettre à jour les contacts</span><span class="sxs-lookup"><span data-stu-id="d6d0a-359">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="d6d0a-360">Mise à jour le `Initialize` méthode dans la `SeedData` classe pour créer les comptes de test :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-360">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="d6d0a-361">Ajouter l’ID d’utilisateur administrateur et `ContactStatus` aux contacts.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-361">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="d6d0a-362">Rendre des contacts « Envoyé » et un « rejeté ».</span><span class="sxs-lookup"><span data-stu-id="d6d0a-362">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="d6d0a-363">Ajoutez l’ID d’utilisateur et l’état pour tous les contacts.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-363">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="d6d0a-364">Un seul contact est affiché :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-364">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="d6d0a-365">Créer le propriétaire, le gestionnaire et gestionnaires d’autorisations d’administrateur</span><span class="sxs-lookup"><span data-stu-id="d6d0a-365">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="d6d0a-366">Créez un dossier *authorization* et créez `ContactIsOwnerAuthorizationHandler` une classe dans celui-ci.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-366">Create an *Authorization* folder and create a `ContactIsOwnerAuthorizationHandler` class in it.</span></span> <span data-ttu-id="d6d0a-367">Le `ContactIsOwnerAuthorizationHandler` vérifie que l’utilisateur agissant sur une ressource propriétaire de la ressource.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-367">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="d6d0a-368">Le `ContactIsOwnerAuthorizationHandler` appels [contexte. Réussir](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) si l’utilisateur authentifié actuel est le propriétaire du contact.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-368">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="d6d0a-369">Les gestionnaires d’autorisation généralement :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-369">Authorization handlers generally:</span></span>

* <span data-ttu-id="d6d0a-370">Retourner `context.Succeed` lorsque les conditions sont remplies.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-370">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="d6d0a-371">Retourner `Task.CompletedTask` lorsque les conditions ne sont pas remplies.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-371">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="d6d0a-372">`Task.CompletedTask`n’est pas un succès&mdash;ou un échec. il permet l’exécution d’autres gestionnaires d’autorisations.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-372">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="d6d0a-373">Si vous devez explicitement échouer, retourner [contexte. Échec](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="d6d0a-373">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="d6d0a-374">L’application permet aux propriétaires de contact de modifier, supprimer ou créer leurs propres données.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-374">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="d6d0a-375">`ContactIsOwnerAuthorizationHandler` n’a pas besoin vérifier l’opération passée dans le paramètre de condition.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-375">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="d6d0a-376">Créer un gestionnaire d’autorisation de gestionnaire</span><span class="sxs-lookup"><span data-stu-id="d6d0a-376">Create a manager authorization handler</span></span>

<span data-ttu-id="d6d0a-377">Créer un `ContactManagerAuthorizationHandler` classe dans le *autorisation* dossier.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-377">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="d6d0a-378">Le `ContactManagerAuthorizationHandler` vérifie l’utilisateur agissant sur la ressource est un gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-378">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="d6d0a-379">Seuls les responsables peuvent approuver ou rejeter les modifications de contenu (nouvelle ou modifiées).</span><span class="sxs-lookup"><span data-stu-id="d6d0a-379">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="d6d0a-380">Créer un gestionnaire d’autorisations d’administrateur</span><span class="sxs-lookup"><span data-stu-id="d6d0a-380">Create an administrator authorization handler</span></span>

<span data-ttu-id="d6d0a-381">Créer un `ContactAdministratorsAuthorizationHandler` classe dans le *autorisation* dossier.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-381">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="d6d0a-382">Le `ContactAdministratorsAuthorizationHandler` vérifie que l’utilisateur agissant sur la ressource est un administrateur.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-382">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="d6d0a-383">Administrateur peut effectuer toutes les opérations.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-383">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="d6d0a-384">Inscrire les gestionnaires d’autorisation</span><span class="sxs-lookup"><span data-stu-id="d6d0a-384">Register the authorization handlers</span></span>

<span data-ttu-id="d6d0a-385">Services à l’aide d’Entity Framework Core doivent être inscrit pour [l’injection de dépendances](xref:fundamentals/dependency-injection) à l’aide de [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="d6d0a-385">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="d6d0a-386">Le `ContactIsOwnerAuthorizationHandler` utilise ASP.NET Core [identité](xref:security/authentication/identity), qui est basé sur Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-386">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="d6d0a-387">Enregistrer les gestionnaires avec la collection de service afin qu’elles soient disponibles pour le `ContactsController` via [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="d6d0a-387">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d6d0a-388">Ajoutez le code suivant à la fin de `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d6d0a-388">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="d6d0a-389">`ContactAdministratorsAuthorizationHandler` et `ContactManagerAuthorizationHandler` sont ajoutés en tant que singletons.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-389">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="d6d0a-390">Ils sont des singletons, car ils n’utilisent pas EF et toutes les informations nécessaires sont dans le `Context` paramètre de la `HandleRequirementAsync` (méthode).</span><span class="sxs-lookup"><span data-stu-id="d6d0a-390">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="d6d0a-391">Autorisation de la prise en charge</span><span class="sxs-lookup"><span data-stu-id="d6d0a-391">Support authorization</span></span>

<span data-ttu-id="d6d0a-392">Dans cette section, vous mettez à jour les Pages Razor et ajoutez une classe de configuration requise des opérations.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-392">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="d6d0a-393">Passez en revue la classe de configuration requise de contact des opérations</span><span class="sxs-lookup"><span data-stu-id="d6d0a-393">Review the contact operations requirements class</span></span>

<span data-ttu-id="d6d0a-394">Examinez la `ContactOperations` classe.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-394">Review the `ContactOperations` class.</span></span> <span data-ttu-id="d6d0a-395">Cette classe contient les exigences de l’application prend en charge :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-395">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="d6d0a-396">Créer une classe de base pour les Pages Razor de Contacts</span><span class="sxs-lookup"><span data-stu-id="d6d0a-396">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="d6d0a-397">Créer une classe de base qui contient les services utilisés dans les Pages Razor de contacts.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-397">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="d6d0a-398">La classe de base place le code d’initialisation dans un emplacement :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-398">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="d6d0a-399">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-399">The preceding code:</span></span>

* <span data-ttu-id="d6d0a-400">Ajoute le `IAuthorizationService` service d’accéder aux gestionnaires d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-400">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="d6d0a-401">Ajoute l’identité `UserManager` service.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-401">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="d6d0a-402">Ajoutez la `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-402">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="d6d0a-403">Mettre à jour le CreateModel</span><span class="sxs-lookup"><span data-stu-id="d6d0a-403">Update the CreateModel</span></span>

<span data-ttu-id="d6d0a-404">Mettre à jour le constructeur de modèle de page de création à utiliser le `DI_BasePageModel` classe de base :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-404">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="d6d0a-405">Mise à jour le `CreateModel.OnPostAsync` méthode à :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-405">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="d6d0a-406">Ajoutez l’ID utilisateur pour la `Contact` modèle.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-406">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="d6d0a-407">Appeler le Gestionnaire d’autorisation pour vérifier que l’utilisateur est autorisé à créer des contacts.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-407">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="d6d0a-408">Mettre à jour le IndexModel</span><span class="sxs-lookup"><span data-stu-id="d6d0a-408">Update the IndexModel</span></span>

<span data-ttu-id="d6d0a-409">Mise à jour le `OnGetAsync` méthode approuvées uniquement les contacts sont présentés aux utilisateurs généraux :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-409">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="d6d0a-410">Mettre à jour le EditModel</span><span class="sxs-lookup"><span data-stu-id="d6d0a-410">Update the EditModel</span></span>

<span data-ttu-id="d6d0a-411">Ajoutez un gestionnaire d’autorisation pour vérifier que l’utilisateur propriétaire du contact.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-411">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="d6d0a-412">Étant donné que l’autorisation de ressource est en cours de validation, le `[Authorize]` attribut n’est pas suffisant.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-412">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="d6d0a-413">L’application n’a pas accès à la ressource lors de l’évaluation des attributs.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-413">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="d6d0a-414">Autorisation basée sur la ressource doit être impérative.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-414">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="d6d0a-415">Vérifications doivent être effectuées une fois que l’application a accès à la ressource, en le chargeant dans le modèle de page ou en le chargeant dans le gestionnaire lui-même.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-415">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="d6d0a-416">Vous accédez fréquemment la ressource en passant la clé de ressource.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-416">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="d6d0a-417">Mettre à jour le DeleteModel</span><span class="sxs-lookup"><span data-stu-id="d6d0a-417">Update the DeleteModel</span></span>

<span data-ttu-id="d6d0a-418">Mettre à jour le modèle de page delete pour utiliser le Gestionnaire d’autorisation pour vérifier que l’utilisateur a l’autorisation de suppression sur le contact.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-418">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="d6d0a-419">Injecter le service d’autorisation dans les vues</span><span class="sxs-lookup"><span data-stu-id="d6d0a-419">Inject the authorization service into the views</span></span>

<span data-ttu-id="d6d0a-420">Actuellement, le montre l’interface utilisateur modifie et supprime des liens pour les contacts de que l’utilisateur ne peut pas modifier.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-420">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="d6d0a-421">Injecter le service d’autorisation dans le *Views/_viewimports.cshtml* afin qu’il soit disponible pour toutes les vues de fichiers :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-421">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="d6d0a-422">Le balisage précédent ajoute plusieurs `using` instructions.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-422">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="d6d0a-423">Mise à jour le **modifier** et **supprimer** lie dans *Pages/Contacts/Index.cshtml* afin d’être affichées uniquement pour les utilisateurs disposant des autorisations appropriées :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-423">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="d6d0a-424">Masquage des liens à partir des utilisateurs qui ne sont pas autorisés à modifier les données ne sécuriser l’application.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-424">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="d6d0a-425">Masquage des liens rend l’application plus conviviale en affichant les liens ne sont valides.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-425">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="d6d0a-426">Les utilisateurs peuvent hack l’URL générées pour appeler modifier et supprimer des opérations sur les données qu’ils ne possèdent pas.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-426">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="d6d0a-427">Le contrôleur ou une Page Razor doit appliquer les vérifications d’accès pour sécuriser les données.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-427">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="d6d0a-428">Détails de la mise à jour</span><span class="sxs-lookup"><span data-stu-id="d6d0a-428">Update Details</span></span>

<span data-ttu-id="d6d0a-429">Mettre à jour l’affichage des détails pour les responsables peuvent approuver ou rejeter des contacts :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-429">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="d6d0a-430">Mettre à jour le modèle de page de détails :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-430">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="d6d0a-431">Ajouter ou supprimer un utilisateur à un rôle</span><span class="sxs-lookup"><span data-stu-id="d6d0a-431">Add or remove a user to a role</span></span>

<span data-ttu-id="d6d0a-432">Consultez [ce problème](https://github.com/aspnet/AspNetCore.Docs/issues/8502) pour plus d’informations sur :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-432">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="d6d0a-433">Suppression de privilèges à partir d’un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-433">Removing privileges from a user.</span></span> <span data-ttu-id="d6d0a-434">Par exemple, la désactivation d’un utilisateur dans une application de conversation.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-434">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="d6d0a-435">Ajout des privilèges à un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-435">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="d6d0a-436">Tester l’application terminée</span><span class="sxs-lookup"><span data-stu-id="d6d0a-436">Test the completed app</span></span>

<span data-ttu-id="d6d0a-437">Si vous n’avez pas déjà défini un mot de passe pour les comptes d’utilisateur amorcée, utilisez le [outil Secret Manager](xref:security/app-secrets#secret-manager) pour définir un mot de passe :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-437">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="d6d0a-438">Choisir un mot de passe fort : Utilisez au moins huit caractères et au moins un caractère majuscule, un chiffre et un symbole.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-438">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="d6d0a-439">Par exemple, `Passw0rd!` répond aux exigences de mot de passe fort.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-439">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="d6d0a-440">Exécutez la commande suivante à partir du dossier du projet, où `<PW>` est le mot de passe :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-440">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

* <span data-ttu-id="d6d0a-441">Supprimer et mettre à jour la base de données</span><span class="sxs-lookup"><span data-stu-id="d6d0a-441">Drop and update the Database</span></span>

    ```console
     dotnet ef database drop -f
     dotnet ef database update  
     ```

* <span data-ttu-id="d6d0a-442">Redémarrez l’application pour amorcer la base de données.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-442">Restart the app to seed the database.</span></span>

<span data-ttu-id="d6d0a-443">Un moyen simple de tester l’application terminée consiste à lancer les trois différents navigateurs (ou incognito/InPrivate sessions).</span><span class="sxs-lookup"><span data-stu-id="d6d0a-443">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="d6d0a-444">Dans un navigateur, inscrire un nouvel utilisateur (par exemple, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="d6d0a-444">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="d6d0a-445">Connectez-vous à chaque navigateur avec un autre utilisateur.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-445">Sign in to each browser with a different user.</span></span> <span data-ttu-id="d6d0a-446">Vérifiez les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-446">Verify the following operations:</span></span>

* <span data-ttu-id="d6d0a-447">Les utilisateurs inscrits peuvent afficher toutes les données de contact approuvées.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-447">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="d6d0a-448">Les utilisateurs inscrits peuvent modifier ou supprimer leurs propres données.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-448">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="d6d0a-449">Gestionnaires peuvent approuver/rejeter les données de contact.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-449">Managers can approve/reject contact data.</span></span> <span data-ttu-id="d6d0a-450">Le `Details` afficher montre **approuver** et **rejeter** boutons.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-450">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="d6d0a-451">Les administrateurs peuvent approuver/rejeter et modifier ou de supprimer toutes les données.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-451">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="d6d0a-452">Utilisateur</span><span class="sxs-lookup"><span data-stu-id="d6d0a-452">User</span></span>                | <span data-ttu-id="d6d0a-453">Amorcée par l’application</span><span class="sxs-lookup"><span data-stu-id="d6d0a-453">Seeded by the app</span></span> | <span data-ttu-id="d6d0a-454">Options</span><span class="sxs-lookup"><span data-stu-id="d6d0a-454">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="d6d0a-455">Non</span><span class="sxs-lookup"><span data-stu-id="d6d0a-455">No</span></span>                | <span data-ttu-id="d6d0a-456">Modifier/supprimer les données propres.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-456">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="d6d0a-457">Oui</span><span class="sxs-lookup"><span data-stu-id="d6d0a-457">Yes</span></span>               | <span data-ttu-id="d6d0a-458">Approuver/rejeter et modifier ou de supprimer des données propres.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-458">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="d6d0a-459">Oui</span><span class="sxs-lookup"><span data-stu-id="d6d0a-459">Yes</span></span>               | <span data-ttu-id="d6d0a-460">Approuver/rejeter et de modifier ou de supprimer toutes les données.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-460">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="d6d0a-461">Créer un contact dans le navigateur de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-461">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="d6d0a-462">Copiez l’URL pour supprimer et modifier à partir du contact de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-462">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="d6d0a-463">Collez ces liens dans le navigateur de l’utilisateur de test pour vérifier que l’utilisateur de test ne peut pas effectuer ces opérations.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-463">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="d6d0a-464">Créer l’application de démarrage</span><span class="sxs-lookup"><span data-stu-id="d6d0a-464">Create the starter app</span></span>

* <span data-ttu-id="d6d0a-465">Créer une application Pages Razor nommée « ContactManager »</span><span class="sxs-lookup"><span data-stu-id="d6d0a-465">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="d6d0a-466">Créer l’application avec **comptes d’utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-466">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="d6d0a-467">Nommez-Le « ContactManager » afin de l’espace de noms correspond à l’espace de noms utilisé dans l’exemple.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-467">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="d6d0a-468">`-uld` Spécifie la base de données locale au lieu de SQLite</span><span class="sxs-lookup"><span data-stu-id="d6d0a-468">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="d6d0a-469">Ajouter des *modèles/contact. cs*:</span><span class="sxs-lookup"><span data-stu-id="d6d0a-469">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="d6d0a-470">Une structure le `Contact` modèle.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-470">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="d6d0a-471">La migration initiale de créer et mettre à jour de la base de données :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-471">Create initial migration and update the database:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* <span data-ttu-id="d6d0a-472">Mise à jour le **ContactManager** ancrer dans le *pages/_Layout.cshtml* fichier :</span><span class="sxs-lookup"><span data-stu-id="d6d0a-472">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* <span data-ttu-id="d6d0a-473">Testez l’application en création, modification et suppression d’un contact</span><span class="sxs-lookup"><span data-stu-id="d6d0a-473">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="d6d0a-474">Amorcer la base de données</span><span class="sxs-lookup"><span data-stu-id="d6d0a-474">Seed the database</span></span>

<span data-ttu-id="d6d0a-475">Ajouter le [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) classe à la *données* dossier.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-475">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="d6d0a-476">Appelez `SeedData.Initialize` de `Main`:</span><span class="sxs-lookup"><span data-stu-id="d6d0a-476">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="d6d0a-477">Tester l’application d’amorçage la base de données.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-477">Test that the app seeded the database.</span></span> <span data-ttu-id="d6d0a-478">S’il existe des lignes dans la base de données de contact, la méthode seed ne s’exécute pas.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-478">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="d6d0a-479">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="d6d0a-479">Additional resources</span></span>

* [<span data-ttu-id="d6d0a-480">Créer une application web .NET Core et SQL Database dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="d6d0a-480">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="d6d0a-481">[ASP.NET Core d’autorisation Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="d6d0a-481">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="d6d0a-482">Ce laboratoire présente plus en détails sur les fonctionnalités de sécurité présentées dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="d6d0a-482">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* <xref:security/authorization/introduction>
* [<span data-ttu-id="d6d0a-483">Autorisation basée sur une stratégie personnalisée</span><span class="sxs-lookup"><span data-stu-id="d6d0a-483">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
