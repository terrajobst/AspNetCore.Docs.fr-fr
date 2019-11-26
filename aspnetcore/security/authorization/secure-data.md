---
title: Créer une application ASP.NET Core avec des données utilisateur protégées par une autorisation
author: rick-anderson
description: Découvrez comment créer une application Pages Razor avec des données utilisateur protégées par une autorisation. Inclut HTTPS, l’authentification, sécurité, ASP.NET Core Identity.
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: 65c72d4dd457f85451796c5713bedebafec7a7de
ms.sourcegitcommit: 8157e5a351f49aeef3769f7d38b787b4386aad5f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74239835"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="b9d56-104">Créer une application ASP.NET Core avec des données utilisateur protégées par une autorisation</span><span class="sxs-lookup"><span data-stu-id="b9d56-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="b9d56-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="b9d56-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="b9d56-106">Consultez [ce fichier PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) pour la version ASP.net Core Mvc.</span><span class="sxs-lookup"><span data-stu-id="b9d56-106">See [this PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="b9d56-107">Le ASP.NET Core version 1,1 de ce didacticiel se trouve dans [ce](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) dossier.</span><span class="sxs-lookup"><span data-stu-id="b9d56-107">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="b9d56-108">L’exemple 1,1 ASP.NET Core se trouve dans les [exemples](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="b9d56-108">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="b9d56-109">Voir [ce PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="b9d56-109">See [this pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b9d56-110">Ce didacticiel montre comment créer une application web ASP.NET Core avec des données utilisateur protégées par une autorisation.</span><span class="sxs-lookup"><span data-stu-id="b9d56-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="b9d56-111">Il affiche une liste de contacts (inscrits) les utilisateurs authentifiés ont créés.</span><span class="sxs-lookup"><span data-stu-id="b9d56-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="b9d56-112">Il existe trois groupes de sécurité :</span><span class="sxs-lookup"><span data-stu-id="b9d56-112">There are three security groups:</span></span>

* <span data-ttu-id="b9d56-113">Les **utilisateurs inscrits** peuvent afficher toutes les données approuvées et peuvent modifier/supprimer leurs propres données.</span><span class="sxs-lookup"><span data-stu-id="b9d56-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="b9d56-114">Les **responsables** peuvent approuver ou rejeter les données de contact.</span><span class="sxs-lookup"><span data-stu-id="b9d56-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="b9d56-115">Seuls les contacts approuvés sont visibles aux utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="b9d56-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="b9d56-116">**Les administrateurs** peuvent approuver/refuser et modifier/supprimer des données.</span><span class="sxs-lookup"><span data-stu-id="b9d56-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="b9d56-117">Les images de ce document ne correspondent pas exactement aux modèles les plus récents.</span><span class="sxs-lookup"><span data-stu-id="b9d56-117">The images in this document don't exactly match the latest templates.</span></span>

<span data-ttu-id="b9d56-118">Dans l’image suivante, l’utilisateur Rick (`rick@example.com`) est connecté.</span><span class="sxs-lookup"><span data-stu-id="b9d56-118">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="b9d56-119">Rick peut uniquement afficher les contacts approuvés et **modifier**/**supprimer**/**créer** des liens pour ses contacts.</span><span class="sxs-lookup"><span data-stu-id="b9d56-119">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="b9d56-120">Seul le dernier enregistrement créé par Rick affiche des liens **modifier** et **supprimer** .</span><span class="sxs-lookup"><span data-stu-id="b9d56-120">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="b9d56-121">Autres utilisateurs ne voient le dernier enregistrement jusqu'à ce qu’un gestionnaire ou un administrateur modifie le statut « Approved ».</span><span class="sxs-lookup"><span data-stu-id="b9d56-121">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Capture d’écran montrant Rick connecté](secure-data/_static/rick.png)

<span data-ttu-id="b9d56-123">Dans l’image suivante, `manager@contoso.com` est connecté et dans le rôle du responsable :</span><span class="sxs-lookup"><span data-stu-id="b9d56-123">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Capture d’écran montrant manager@contoso.com connecté](secure-data/_static/manager1.png)

<span data-ttu-id="b9d56-125">L’illustration suivante montre les gestionnaires de vue des détails d’un contact :</span><span class="sxs-lookup"><span data-stu-id="b9d56-125">The following image shows the managers details view of a contact:</span></span>

![Vue du responsable d’un contact](secure-data/_static/manager.png)

<span data-ttu-id="b9d56-127">Les boutons **approuver** et **rejeter** s’affichent uniquement pour les responsables et les administrateurs.</span><span class="sxs-lookup"><span data-stu-id="b9d56-127">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="b9d56-128">Dans l’image suivante, `admin@contoso.com` est connecté et dans le rôle de l’administrateur :</span><span class="sxs-lookup"><span data-stu-id="b9d56-128">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Capture d’écran montrant admin@contoso.com connecté](secure-data/_static/admin.png)

<span data-ttu-id="b9d56-130">L’administrateur a tous les privilèges.</span><span class="sxs-lookup"><span data-stu-id="b9d56-130">The administrator has all privileges.</span></span> <span data-ttu-id="b9d56-131">Elle peut lire/modifier/supprimer un contact et modifier l’état de contacts.</span><span class="sxs-lookup"><span data-stu-id="b9d56-131">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="b9d56-132">L’application a été créée par la [génération](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) de modèles automatique du modèle de `Contact` suivant :</span><span class="sxs-lookup"><span data-stu-id="b9d56-132">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="b9d56-133">L’exemple contient les gestionnaires d’autorisation suivants :</span><span class="sxs-lookup"><span data-stu-id="b9d56-133">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="b9d56-134">`ContactIsOwnerAuthorizationHandler`: garantit qu’un utilisateur peut uniquement modifier ses données.</span><span class="sxs-lookup"><span data-stu-id="b9d56-134">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="b9d56-135">`ContactManagerAuthorizationHandler`: permet aux gestionnaires d’approuver ou de rejeter les contacts.</span><span class="sxs-lookup"><span data-stu-id="b9d56-135">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="b9d56-136">`ContactAdministratorsAuthorizationHandler`: permet aux administrateurs d’approuver ou de refuser des contacts, ainsi que de modifier/supprimer des contacts.</span><span class="sxs-lookup"><span data-stu-id="b9d56-136">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b9d56-137">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="b9d56-137">Prerequisites</span></span>

<span data-ttu-id="b9d56-138">Ce didacticiel est avancé.</span><span class="sxs-lookup"><span data-stu-id="b9d56-138">This tutorial is advanced.</span></span> <span data-ttu-id="b9d56-139">Vous devez être familiarisé avec :</span><span class="sxs-lookup"><span data-stu-id="b9d56-139">You should be familiar with:</span></span>

* [<span data-ttu-id="b9d56-140">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b9d56-140">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="b9d56-141">Authentification</span><span class="sxs-lookup"><span data-stu-id="b9d56-141">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="b9d56-142">Confirmation de compte et récupération de mot de passe</span><span class="sxs-lookup"><span data-stu-id="b9d56-142">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="b9d56-143">Autorisation</span><span class="sxs-lookup"><span data-stu-id="b9d56-143">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="b9d56-144">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="b9d56-144">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="b9d56-145">Le démarrage et l’application terminée</span><span class="sxs-lookup"><span data-stu-id="b9d56-145">The starter and completed app</span></span>

<span data-ttu-id="b9d56-146">[Téléchargez](xref:index#how-to-download-a-sample) l’application [terminée](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) .</span><span class="sxs-lookup"><span data-stu-id="b9d56-146">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="b9d56-147">[Testez](#test-the-completed-app) l’application terminée afin de vous familiariser avec ses fonctionnalités de sécurité.</span><span class="sxs-lookup"><span data-stu-id="b9d56-147">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="b9d56-148">L’application de démarrage</span><span class="sxs-lookup"><span data-stu-id="b9d56-148">The starter app</span></span>

<span data-ttu-id="b9d56-149">[Téléchargez](xref:index#how-to-download-a-sample) l’application de [démarrage](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) .</span><span class="sxs-lookup"><span data-stu-id="b9d56-149">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="b9d56-150">Exécutez l’application, appuyez sur le lien **ContactManager** et vérifiez que vous pouvez créer, modifier et supprimer un contact.</span><span class="sxs-lookup"><span data-stu-id="b9d56-150">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="b9d56-151">Sécuriser les données utilisateur</span><span class="sxs-lookup"><span data-stu-id="b9d56-151">Secure user data</span></span>

<span data-ttu-id="b9d56-152">Les sections suivantes ont toutes les principales étapes pour créer l’application de données utilisateur sécurisée.</span><span class="sxs-lookup"><span data-stu-id="b9d56-152">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="b9d56-153">Il peut s’avérer utile pour faire référence au projet terminé.</span><span class="sxs-lookup"><span data-stu-id="b9d56-153">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="b9d56-154">Lier les données de contact à l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="b9d56-154">Tie the contact data to the user</span></span>

<span data-ttu-id="b9d56-155">Utilisez l’IDENTIFIant utilisateur de l' [identité](xref:security/authentication/identity) ASP.net pour vous assurer que les utilisateurs peuvent modifier leurs données, mais pas les données d’autres utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="b9d56-155">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="b9d56-156">Ajoutez `OwnerID` et `ContactStatus` au modèle `Contact` :</span><span class="sxs-lookup"><span data-stu-id="b9d56-156">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="b9d56-157">`OwnerID` est l’ID de l’utilisateur de la table `AspNetUser` dans la base de données d' [identité](xref:security/authentication/identity) .</span><span class="sxs-lookup"><span data-stu-id="b9d56-157">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="b9d56-158">Le champ `Status` détermine si un contact est visible par les utilisateurs généraux.</span><span class="sxs-lookup"><span data-stu-id="b9d56-158">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="b9d56-159">Créer une nouvelle migration et mettre à jour de la base de données :</span><span class="sxs-lookup"><span data-stu-id="b9d56-159">Create a new migration and update the database:</span></span>

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="b9d56-160">Ajouter des services de rôle à l’identité</span><span class="sxs-lookup"><span data-stu-id="b9d56-160">Add Role services to Identity</span></span>

<span data-ttu-id="b9d56-161">Ajoutez [rôles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) pour ajouter des services de rôle :</span><span class="sxs-lookup"><span data-stu-id="b9d56-161">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a><span data-ttu-id="b9d56-162">Demander aux utilisateurs authentifiés</span><span class="sxs-lookup"><span data-stu-id="b9d56-162">Require authenticated users</span></span>

<span data-ttu-id="b9d56-163">Définir la stratégie d’authentification par défaut pour exiger l’authentification des utilisateurs :</span><span class="sxs-lookup"><span data-stu-id="b9d56-163">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 <span data-ttu-id="b9d56-164">Vous pouvez refuser l’authentification au niveau de la page Razor, du contrôleur ou de la méthode d’action avec l’attribut `[AllowAnonymous]`.</span><span class="sxs-lookup"><span data-stu-id="b9d56-164">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="b9d56-165">Définition de la stratégie d’authentification par défaut pour les utilisateurs doivent être authentifiés protège nouvellement ajouté les Pages Razor et les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="b9d56-165">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="b9d56-166">L’authentification requise par défaut est plus sécurisée que le fait de s’appuyer sur de nouveaux contrôleurs et Razor Pages d’inclure l’attribut `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="b9d56-166">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="b9d56-167">Ajoutez [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) aux pages d’index et de confidentialité afin que les utilisateurs anonymes puissent obtenir des informations sur le site avant de s’inscrire.</span><span class="sxs-lookup"><span data-stu-id="b9d56-167">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index and Privacy pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a><span data-ttu-id="b9d56-168">Configurer le compte de test</span><span class="sxs-lookup"><span data-stu-id="b9d56-168">Configure the test account</span></span>

<span data-ttu-id="b9d56-169">La classe `SeedData` crée deux comptes : administrateur et gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="b9d56-169">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="b9d56-170">Utilisez l' [outil secret Manager](xref:security/app-secrets) pour définir un mot de passe pour ces comptes.</span><span class="sxs-lookup"><span data-stu-id="b9d56-170">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="b9d56-171">Définissez le mot de passe à partir du répertoire du projet (répertoire contenant *Program.cs*) :</span><span class="sxs-lookup"><span data-stu-id="b9d56-171">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="b9d56-172">Si un mot de passe fort n’est pas spécifié, une exception est levée lors de l’appel de `SeedData.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="b9d56-172">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="b9d56-173">Mettez à jour `Main` pour utiliser le mot de passe de test :</span><span class="sxs-lookup"><span data-stu-id="b9d56-173">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="b9d56-174">Créez les comptes de test et de mettre à jour les contacts</span><span class="sxs-lookup"><span data-stu-id="b9d56-174">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="b9d56-175">Mettez à jour la méthode `Initialize` dans la classe `SeedData` pour créer les comptes de test :</span><span class="sxs-lookup"><span data-stu-id="b9d56-175">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="b9d56-176">Ajoutez l’ID d’utilisateur de l’administrateur et `ContactStatus` aux contacts.</span><span class="sxs-lookup"><span data-stu-id="b9d56-176">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="b9d56-177">Rendre des contacts « Envoyé » et un « rejeté ».</span><span class="sxs-lookup"><span data-stu-id="b9d56-177">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="b9d56-178">Ajoutez l’ID d’utilisateur et l’état pour tous les contacts.</span><span class="sxs-lookup"><span data-stu-id="b9d56-178">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="b9d56-179">Un seul contact est affiché :</span><span class="sxs-lookup"><span data-stu-id="b9d56-179">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="b9d56-180">Créer le propriétaire, le gestionnaire et gestionnaires d’autorisations d’administrateur</span><span class="sxs-lookup"><span data-stu-id="b9d56-180">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="b9d56-181">Créez une classe `ContactIsOwnerAuthorizationHandler` dans le dossier *authorization* .</span><span class="sxs-lookup"><span data-stu-id="b9d56-181">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="b9d56-182">Le `ContactIsOwnerAuthorizationHandler` vérifie que l’utilisateur agissant sur une ressource possède la ressource.</span><span class="sxs-lookup"><span data-stu-id="b9d56-182">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="b9d56-183">Le `ContactIsOwnerAuthorizationHandler` appelle le [contexte. Réussie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) si l’utilisateur authentifié actuel est le propriétaire du contact.</span><span class="sxs-lookup"><span data-stu-id="b9d56-183">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="b9d56-184">Les gestionnaires d’autorisation généralement :</span><span class="sxs-lookup"><span data-stu-id="b9d56-184">Authorization handlers generally:</span></span>

* <span data-ttu-id="b9d56-185">Retourne `context.Succeed` lorsque la configuration requise est respectée.</span><span class="sxs-lookup"><span data-stu-id="b9d56-185">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="b9d56-186">Retourne `Task.CompletedTask` lorsque les spécifications ne sont pas respectées.</span><span class="sxs-lookup"><span data-stu-id="b9d56-186">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="b9d56-187">`Task.CompletedTask` n’a pas réussi ou a échoué&mdash;il permet l’exécution d’autres gestionnaires d’autorisations.</span><span class="sxs-lookup"><span data-stu-id="b9d56-187">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="b9d56-188">Si vous devez faire échouer explicitement, retournez le [contexte. Échoue](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="b9d56-188">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="b9d56-189">L’application permet aux propriétaires de contact de modifier, supprimer ou créer leurs propres données.</span><span class="sxs-lookup"><span data-stu-id="b9d56-189">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="b9d56-190">`ContactIsOwnerAuthorizationHandler` n’a pas besoin de vérifier l’opération passée dans le paramètre d’exigence.</span><span class="sxs-lookup"><span data-stu-id="b9d56-190">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="b9d56-191">Créer un gestionnaire d’autorisation de gestionnaire</span><span class="sxs-lookup"><span data-stu-id="b9d56-191">Create a manager authorization handler</span></span>

<span data-ttu-id="b9d56-192">Créez une classe `ContactManagerAuthorizationHandler` dans le dossier *authorization* .</span><span class="sxs-lookup"><span data-stu-id="b9d56-192">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="b9d56-193">Le `ContactManagerAuthorizationHandler` vérifie que l’utilisateur agissant sur la ressource est un responsable.</span><span class="sxs-lookup"><span data-stu-id="b9d56-193">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="b9d56-194">Seuls les responsables peuvent approuver ou rejeter les modifications de contenu (nouvelle ou modifiées).</span><span class="sxs-lookup"><span data-stu-id="b9d56-194">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="b9d56-195">Créer un gestionnaire d’autorisations d’administrateur</span><span class="sxs-lookup"><span data-stu-id="b9d56-195">Create an administrator authorization handler</span></span>

<span data-ttu-id="b9d56-196">Créez une classe `ContactAdministratorsAuthorizationHandler` dans le dossier *authorization* .</span><span class="sxs-lookup"><span data-stu-id="b9d56-196">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="b9d56-197">Le `ContactAdministratorsAuthorizationHandler` vérifie que l’utilisateur agissant sur la ressource est un administrateur.</span><span class="sxs-lookup"><span data-stu-id="b9d56-197">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="b9d56-198">Administrateur peut effectuer toutes les opérations.</span><span class="sxs-lookup"><span data-stu-id="b9d56-198">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="b9d56-199">Inscrire les gestionnaires d’autorisation</span><span class="sxs-lookup"><span data-stu-id="b9d56-199">Register the authorization handlers</span></span>

<span data-ttu-id="b9d56-200">Les services qui utilisent Entity Framework Core doivent être inscrits pour l' [injection de dépendances](xref:fundamentals/dependency-injection) à l’aide de [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="b9d56-200">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="b9d56-201">Le `ContactIsOwnerAuthorizationHandler` utilise ASP.NET Core [identité](xref:security/authentication/identity), qui repose sur Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b9d56-201">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="b9d56-202">Inscrivez les gestionnaires auprès de la collection de services afin qu’ils soient disponibles pour le `ContactsController` par le biais de l' [injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b9d56-202">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="b9d56-203">Ajoutez le code suivant à la fin de `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b9d56-203">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

<span data-ttu-id="b9d56-204">les `ContactAdministratorsAuthorizationHandler` et les `ContactManagerAuthorizationHandler` sont ajoutés en tant que singletons.</span><span class="sxs-lookup"><span data-stu-id="b9d56-204">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="b9d56-205">Il s’agit de singletons, car ils n’utilisent pas EF et toutes les informations nécessaires sont dans le paramètre `Context` de la méthode `HandleRequirementAsync`.</span><span class="sxs-lookup"><span data-stu-id="b9d56-205">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="b9d56-206">Autorisation de la prise en charge</span><span class="sxs-lookup"><span data-stu-id="b9d56-206">Support authorization</span></span>

<span data-ttu-id="b9d56-207">Dans cette section, vous mettez à jour les Pages Razor et ajoutez une classe de configuration requise des opérations.</span><span class="sxs-lookup"><span data-stu-id="b9d56-207">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="b9d56-208">Passez en revue la classe de configuration requise de contact des opérations</span><span class="sxs-lookup"><span data-stu-id="b9d56-208">Review the contact operations requirements class</span></span>

<span data-ttu-id="b9d56-209">Passez en revue la classe `ContactOperations`.</span><span class="sxs-lookup"><span data-stu-id="b9d56-209">Review the `ContactOperations` class.</span></span> <span data-ttu-id="b9d56-210">Cette classe contient les exigences de l’application prend en charge :</span><span class="sxs-lookup"><span data-stu-id="b9d56-210">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="b9d56-211">Créer une classe de base pour les Pages Razor de Contacts</span><span class="sxs-lookup"><span data-stu-id="b9d56-211">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="b9d56-212">Créer une classe de base qui contient les services utilisés dans les Pages Razor de contacts.</span><span class="sxs-lookup"><span data-stu-id="b9d56-212">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="b9d56-213">La classe de base place le code d’initialisation dans un emplacement :</span><span class="sxs-lookup"><span data-stu-id="b9d56-213">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="b9d56-214">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="b9d56-214">The preceding code:</span></span>

* <span data-ttu-id="b9d56-215">Ajoute le service `IAuthorizationService` pour accéder aux gestionnaires d’autorisations.</span><span class="sxs-lookup"><span data-stu-id="b9d56-215">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="b9d56-216">Ajoute l’identité `UserManager` service.</span><span class="sxs-lookup"><span data-stu-id="b9d56-216">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="b9d56-217">Ajoutez la `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="b9d56-217">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="b9d56-218">Mettre à jour le CreateModel</span><span class="sxs-lookup"><span data-stu-id="b9d56-218">Update the CreateModel</span></span>

<span data-ttu-id="b9d56-219">Mettez à jour le constructeur de modèle de page de création pour utiliser la classe de base `DI_BasePageModel` :</span><span class="sxs-lookup"><span data-stu-id="b9d56-219">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="b9d56-220">Mettez à jour la méthode `CreateModel.OnPostAsync` pour :</span><span class="sxs-lookup"><span data-stu-id="b9d56-220">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="b9d56-221">Ajoutez l’ID d’utilisateur au modèle de `Contact`.</span><span class="sxs-lookup"><span data-stu-id="b9d56-221">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="b9d56-222">Appeler le Gestionnaire d’autorisation pour vérifier que l’utilisateur est autorisé à créer des contacts.</span><span class="sxs-lookup"><span data-stu-id="b9d56-222">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="b9d56-223">Mettre à jour le IndexModel</span><span class="sxs-lookup"><span data-stu-id="b9d56-223">Update the IndexModel</span></span>

<span data-ttu-id="b9d56-224">Mettez à jour la méthode `OnGetAsync` afin que seuls les contacts approuvés soient affichés pour les utilisateurs généraux :</span><span class="sxs-lookup"><span data-stu-id="b9d56-224">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="b9d56-225">Mettre à jour le EditModel</span><span class="sxs-lookup"><span data-stu-id="b9d56-225">Update the EditModel</span></span>

<span data-ttu-id="b9d56-226">Ajoutez un gestionnaire d’autorisation pour vérifier que l’utilisateur propriétaire du contact.</span><span class="sxs-lookup"><span data-stu-id="b9d56-226">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="b9d56-227">Étant donné que l’autorisation des ressources est en cours de validation, l’attribut `[Authorize]` n’est pas suffisant.</span><span class="sxs-lookup"><span data-stu-id="b9d56-227">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="b9d56-228">L’application n’a pas accès à la ressource lors de l’évaluation des attributs.</span><span class="sxs-lookup"><span data-stu-id="b9d56-228">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="b9d56-229">Autorisation basée sur la ressource doit être impérative.</span><span class="sxs-lookup"><span data-stu-id="b9d56-229">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="b9d56-230">Vérifications doivent être effectuées une fois que l’application a accès à la ressource, en le chargeant dans le modèle de page ou en le chargeant dans le gestionnaire lui-même.</span><span class="sxs-lookup"><span data-stu-id="b9d56-230">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="b9d56-231">Vous accédez fréquemment la ressource en passant la clé de ressource.</span><span class="sxs-lookup"><span data-stu-id="b9d56-231">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="b9d56-232">Mettre à jour le DeleteModel</span><span class="sxs-lookup"><span data-stu-id="b9d56-232">Update the DeleteModel</span></span>

<span data-ttu-id="b9d56-233">Mettre à jour le modèle de page delete pour utiliser le Gestionnaire d’autorisation pour vérifier que l’utilisateur a l’autorisation de suppression sur le contact.</span><span class="sxs-lookup"><span data-stu-id="b9d56-233">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="b9d56-234">Injecter le service d’autorisation dans les vues</span><span class="sxs-lookup"><span data-stu-id="b9d56-234">Inject the authorization service into the views</span></span>

<span data-ttu-id="b9d56-235">Actuellement, le montre l’interface utilisateur modifie et supprime des liens pour les contacts de que l’utilisateur ne peut pas modifier.</span><span class="sxs-lookup"><span data-stu-id="b9d56-235">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="b9d56-236">Injecter le service d’autorisation dans le fichier *pages/_ViewImports. cshtml* afin qu’il soit disponible pour tous les affichages :</span><span class="sxs-lookup"><span data-stu-id="b9d56-236">Inject the authorization service in the *Pages/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="b9d56-237">Le balisage précédent ajoute plusieurs instructions `using`.</span><span class="sxs-lookup"><span data-stu-id="b9d56-237">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="b9d56-238">Mettez à jour les liens **modifier** et **supprimer** dans *pages/contacts/index. cshtml* afin qu’ils soient affichés uniquement pour les utilisateurs disposant des autorisations appropriées :</span><span class="sxs-lookup"><span data-stu-id="b9d56-238">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="b9d56-239">Masquage des liens à partir des utilisateurs qui ne sont pas autorisés à modifier les données ne sécuriser l’application.</span><span class="sxs-lookup"><span data-stu-id="b9d56-239">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="b9d56-240">Masquage des liens rend l’application plus conviviale en affichant les liens ne sont valides.</span><span class="sxs-lookup"><span data-stu-id="b9d56-240">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="b9d56-241">Les utilisateurs peuvent hack l’URL générées pour appeler modifier et supprimer des opérations sur les données qu’ils ne possèdent pas.</span><span class="sxs-lookup"><span data-stu-id="b9d56-241">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="b9d56-242">Le contrôleur ou une Page Razor doit appliquer les vérifications d’accès pour sécuriser les données.</span><span class="sxs-lookup"><span data-stu-id="b9d56-242">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="b9d56-243">Détails de la mise à jour</span><span class="sxs-lookup"><span data-stu-id="b9d56-243">Update Details</span></span>

<span data-ttu-id="b9d56-244">Mettre à jour l’affichage des détails pour les responsables peuvent approuver ou rejeter des contacts :</span><span class="sxs-lookup"><span data-stu-id="b9d56-244">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="b9d56-245">Mettre à jour le modèle de page de détails :</span><span class="sxs-lookup"><span data-stu-id="b9d56-245">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="b9d56-246">Ajouter ou supprimer un utilisateur à un rôle</span><span class="sxs-lookup"><span data-stu-id="b9d56-246">Add or remove a user to a role</span></span>

<span data-ttu-id="b9d56-247">Consultez [ce numéro](https://github.com/aspnet/AspNetCore.Docs/issues/8502) pour plus d’informations sur :</span><span class="sxs-lookup"><span data-stu-id="b9d56-247">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="b9d56-248">Suppression de privilèges à partir d’un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b9d56-248">Removing privileges from a user.</span></span> <span data-ttu-id="b9d56-249">Par exemple, la désactivation d’un utilisateur dans une application de conversation.</span><span class="sxs-lookup"><span data-stu-id="b9d56-249">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="b9d56-250">Ajout des privilèges à un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b9d56-250">Adding privileges to a user.</span></span>

<a name="challenge"></a>

## <a name="differences-between-challenge-and-forbid"></a><span data-ttu-id="b9d56-251">Différences entre Challenge et interdire</span><span class="sxs-lookup"><span data-stu-id="b9d56-251">Differences between Challenge and Forbid</span></span>

<span data-ttu-id="b9d56-252">Cette application définit la stratégie par défaut pour [exiger des utilisateurs authentifiés](#require-authenticated-users).</span><span class="sxs-lookup"><span data-stu-id="b9d56-252">This app sets the default policy to [require authenticated users](#require-authenticated-users).</span></span> <span data-ttu-id="b9d56-253">Le code suivant autorise les utilisateurs anonymes.</span><span class="sxs-lookup"><span data-stu-id="b9d56-253">The following code allows anonymous users.</span></span> <span data-ttu-id="b9d56-254">Les utilisateurs anonymes sont autorisés à afficher les différences entre la stimulation et l’interdiction.</span><span class="sxs-lookup"><span data-stu-id="b9d56-254">Anonymous users are allowed to show the differences between Challenge vs Forbid.</span></span>

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Details2.cshtml.cs?name=snippet)]

<span data-ttu-id="b9d56-255">Dans le code précédent :</span><span class="sxs-lookup"><span data-stu-id="b9d56-255">In the preceding code:</span></span>

* <span data-ttu-id="b9d56-256">Lorsque l’utilisateur n’est **pas** authentifié, une `ChallengeResult` est retournée.</span><span class="sxs-lookup"><span data-stu-id="b9d56-256">When the user is **not** authenticated, a `ChallengeResult` is returned.</span></span> <span data-ttu-id="b9d56-257">Lorsqu’un `ChallengeResult` est retourné, l’utilisateur est redirigé vers la page de connexion.</span><span class="sxs-lookup"><span data-stu-id="b9d56-257">When a `ChallengeResult` is returned, the user is redirected to the sign-in page.</span></span>
* <span data-ttu-id="b9d56-258">Lorsque l’utilisateur est authentifié, mais n’est pas autorisé, une `ForbidResult` est retournée.</span><span class="sxs-lookup"><span data-stu-id="b9d56-258">When the user is authenticated, but not authorized, a `ForbidResult` is returned.</span></span> <span data-ttu-id="b9d56-259">Lorsqu’un `ForbidResult` est retourné, l’utilisateur est redirigé vers la page accès refusé.</span><span class="sxs-lookup"><span data-stu-id="b9d56-259">When a `ForbidResult` is returned, the user is redirected to the access denied page.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="b9d56-260">Tester l’application terminée</span><span class="sxs-lookup"><span data-stu-id="b9d56-260">Test the completed app</span></span>

<span data-ttu-id="b9d56-261">Si vous n’avez pas encore défini de mot de passe pour les comptes d’utilisateurs amorcés, utilisez l' [outil secret Manager](xref:security/app-secrets#secret-manager) pour définir un mot de passe :</span><span class="sxs-lookup"><span data-stu-id="b9d56-261">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="b9d56-262">Choisissez un mot de passe fort : utilisez huit ou plus caractères et au moins un caractère majuscule, nombre et symboles.</span><span class="sxs-lookup"><span data-stu-id="b9d56-262">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="b9d56-263">Par exemple, `Passw0rd!` répond aux exigences de mot de passe fort.</span><span class="sxs-lookup"><span data-stu-id="b9d56-263">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="b9d56-264">Exécutez la commande suivante à partir du dossier du projet, où `<PW>` est le mot de passe :</span><span class="sxs-lookup"><span data-stu-id="b9d56-264">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

<span data-ttu-id="b9d56-265">Si l’application a des contacts :</span><span class="sxs-lookup"><span data-stu-id="b9d56-265">If the app has contacts:</span></span>

* <span data-ttu-id="b9d56-266">Supprimez tous les enregistrements dans la table `Contact`.</span><span class="sxs-lookup"><span data-stu-id="b9d56-266">Delete all of the records in the `Contact` table.</span></span>
* <span data-ttu-id="b9d56-267">Redémarrez l’application pour amorcer la base de données.</span><span class="sxs-lookup"><span data-stu-id="b9d56-267">Restart the app to seed the database.</span></span>

<span data-ttu-id="b9d56-268">Un moyen simple de tester l’application terminée consiste à lancer les trois différents navigateurs (ou incognito/InPrivate sessions).</span><span class="sxs-lookup"><span data-stu-id="b9d56-268">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="b9d56-269">Dans un navigateur, inscrivez un nouvel utilisateur (par exemple, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="b9d56-269">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="b9d56-270">Connectez-vous à chaque navigateur avec un autre utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b9d56-270">Sign in to each browser with a different user.</span></span> <span data-ttu-id="b9d56-271">Vérifiez les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="b9d56-271">Verify the following operations:</span></span>

* <span data-ttu-id="b9d56-272">Les utilisateurs inscrits peuvent afficher toutes les données de contact approuvées.</span><span class="sxs-lookup"><span data-stu-id="b9d56-272">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="b9d56-273">Les utilisateurs inscrits peuvent modifier ou supprimer leurs propres données.</span><span class="sxs-lookup"><span data-stu-id="b9d56-273">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="b9d56-274">Gestionnaires peuvent approuver/rejeter les données de contact.</span><span class="sxs-lookup"><span data-stu-id="b9d56-274">Managers can approve/reject contact data.</span></span> <span data-ttu-id="b9d56-275">La vue `Details` affiche les boutons **approuver** et **rejeter** .</span><span class="sxs-lookup"><span data-stu-id="b9d56-275">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="b9d56-276">Les administrateurs peuvent approuver/rejeter et modifier ou de supprimer toutes les données.</span><span class="sxs-lookup"><span data-stu-id="b9d56-276">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="b9d56-277">Utilisateur</span><span class="sxs-lookup"><span data-stu-id="b9d56-277">User</span></span>                | <span data-ttu-id="b9d56-278">Amorcée par l’application</span><span class="sxs-lookup"><span data-stu-id="b9d56-278">Seeded by the app</span></span> | <span data-ttu-id="b9d56-279">Options</span><span class="sxs-lookup"><span data-stu-id="b9d56-279">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="b9d56-280">Non</span><span class="sxs-lookup"><span data-stu-id="b9d56-280">No</span></span>                | <span data-ttu-id="b9d56-281">Modifier/supprimer les données propres.</span><span class="sxs-lookup"><span data-stu-id="b9d56-281">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="b9d56-282">Oui</span><span class="sxs-lookup"><span data-stu-id="b9d56-282">Yes</span></span>               | <span data-ttu-id="b9d56-283">Approuver/rejeter et modifier ou de supprimer des données propres.</span><span class="sxs-lookup"><span data-stu-id="b9d56-283">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="b9d56-284">Oui</span><span class="sxs-lookup"><span data-stu-id="b9d56-284">Yes</span></span>               | <span data-ttu-id="b9d56-285">Approuver/rejeter et de modifier ou de supprimer toutes les données.</span><span class="sxs-lookup"><span data-stu-id="b9d56-285">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="b9d56-286">Créer un contact dans le navigateur de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="b9d56-286">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="b9d56-287">Copiez l’URL pour supprimer et modifier à partir du contact de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="b9d56-287">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="b9d56-288">Collez ces liens dans le navigateur de l’utilisateur de test pour vérifier que l’utilisateur de test ne peut pas effectuer ces opérations.</span><span class="sxs-lookup"><span data-stu-id="b9d56-288">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="b9d56-289">Créer l’application de démarrage</span><span class="sxs-lookup"><span data-stu-id="b9d56-289">Create the starter app</span></span>

* <span data-ttu-id="b9d56-290">Créer une application Pages Razor nommée « ContactManager »</span><span class="sxs-lookup"><span data-stu-id="b9d56-290">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="b9d56-291">Créez l’application avec des **comptes d’utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="b9d56-291">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="b9d56-292">Nommez-Le « ContactManager » afin de l’espace de noms correspond à l’espace de noms utilisé dans l’exemple.</span><span class="sxs-lookup"><span data-stu-id="b9d56-292">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="b9d56-293">`-uld` spécifie la base de données locale au lieu de SQLite</span><span class="sxs-lookup"><span data-stu-id="b9d56-293">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="b9d56-294">Ajouter des *modèles/contact. cs*:</span><span class="sxs-lookup"><span data-stu-id="b9d56-294">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="b9d56-295">Structurez le modèle de `Contact`.</span><span class="sxs-lookup"><span data-stu-id="b9d56-295">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="b9d56-296">La migration initiale de créer et mettre à jour de la base de données :</span><span class="sxs-lookup"><span data-stu-id="b9d56-296">Create initial migration and update the database:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

<span data-ttu-id="b9d56-297">Si vous rencontrez un bogue avec la commande `dotnet aspnet-codegenerator razorpage`, consultez [ce problème GitHub](https://github.com/aspnet/Scaffolding/issues/984).</span><span class="sxs-lookup"><span data-stu-id="b9d56-297">If you experience a bug with the `dotnet aspnet-codegenerator razorpage` command, see [this GitHub issue](https://github.com/aspnet/Scaffolding/issues/984).</span></span>

* <span data-ttu-id="b9d56-298">Mettez à jour l’ancre **ContactManager** dans le fichier *pages/shared/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="b9d56-298">Update the **ContactManager** anchor in the *Pages/Shared/_Layout.cshtml* file:</span></span>

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* <span data-ttu-id="b9d56-299">Testez l’application en création, modification et suppression d’un contact</span><span class="sxs-lookup"><span data-stu-id="b9d56-299">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="b9d56-300">Amorcer la base de données</span><span class="sxs-lookup"><span data-stu-id="b9d56-300">Seed the database</span></span>

<span data-ttu-id="b9d56-301">Ajoutez la classe [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) au dossier *Data* :</span><span class="sxs-lookup"><span data-stu-id="b9d56-301">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) class to the *Data* folder:</span></span>

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

<span data-ttu-id="b9d56-302">Appelez `SeedData.Initialize` à partir de `Main`:</span><span class="sxs-lookup"><span data-stu-id="b9d56-302">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

<span data-ttu-id="b9d56-303">Tester l’application d’amorçage la base de données.</span><span class="sxs-lookup"><span data-stu-id="b9d56-303">Test that the app seeded the database.</span></span> <span data-ttu-id="b9d56-304">S’il existe des lignes dans la base de données de contact, la méthode seed ne s’exécute pas.</span><span class="sxs-lookup"><span data-stu-id="b9d56-304">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

<span data-ttu-id="b9d56-305">Ce didacticiel montre comment créer une application web ASP.NET Core avec des données utilisateur protégées par une autorisation.</span><span class="sxs-lookup"><span data-stu-id="b9d56-305">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="b9d56-306">Il affiche une liste de contacts (inscrits) les utilisateurs authentifiés ont créés.</span><span class="sxs-lookup"><span data-stu-id="b9d56-306">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="b9d56-307">Il existe trois groupes de sécurité :</span><span class="sxs-lookup"><span data-stu-id="b9d56-307">There are three security groups:</span></span>

* <span data-ttu-id="b9d56-308">Les **utilisateurs inscrits** peuvent afficher toutes les données approuvées et peuvent modifier/supprimer leurs propres données.</span><span class="sxs-lookup"><span data-stu-id="b9d56-308">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="b9d56-309">Les **responsables** peuvent approuver ou rejeter les données de contact.</span><span class="sxs-lookup"><span data-stu-id="b9d56-309">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="b9d56-310">Seuls les contacts approuvés sont visibles aux utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="b9d56-310">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="b9d56-311">**Les administrateurs** peuvent approuver/refuser et modifier/supprimer des données.</span><span class="sxs-lookup"><span data-stu-id="b9d56-311">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="b9d56-312">Dans l’image suivante, l’utilisateur Rick (`rick@example.com`) est connecté.</span><span class="sxs-lookup"><span data-stu-id="b9d56-312">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="b9d56-313">Rick peut uniquement afficher les contacts approuvés et **modifier**/**supprimer**/**créer** des liens pour ses contacts.</span><span class="sxs-lookup"><span data-stu-id="b9d56-313">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="b9d56-314">Seul le dernier enregistrement créé par Rick affiche des liens **modifier** et **supprimer** .</span><span class="sxs-lookup"><span data-stu-id="b9d56-314">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="b9d56-315">Autres utilisateurs ne voient le dernier enregistrement jusqu'à ce qu’un gestionnaire ou un administrateur modifie le statut « Approved ».</span><span class="sxs-lookup"><span data-stu-id="b9d56-315">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Capture d’écran montrant Rick connecté](secure-data/_static/rick.png)

<span data-ttu-id="b9d56-317">Dans l’image suivante, `manager@contoso.com` est connecté et dans le rôle du responsable :</span><span class="sxs-lookup"><span data-stu-id="b9d56-317">In the following image, `manager@contoso.com` is signed in and in the manager's role:</span></span>

![Capture d’écran montrant manager@contoso.com connecté](secure-data/_static/manager1.png)

<span data-ttu-id="b9d56-319">L’illustration suivante montre les gestionnaires de vue des détails d’un contact :</span><span class="sxs-lookup"><span data-stu-id="b9d56-319">The following image shows the managers details view of a contact:</span></span>

![Vue du responsable d’un contact](secure-data/_static/manager.png)

<span data-ttu-id="b9d56-321">Les boutons **approuver** et **rejeter** s’affichent uniquement pour les responsables et les administrateurs.</span><span class="sxs-lookup"><span data-stu-id="b9d56-321">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="b9d56-322">Dans l’image suivante, `admin@contoso.com` est connecté et dans le rôle de l’administrateur :</span><span class="sxs-lookup"><span data-stu-id="b9d56-322">In the following image, `admin@contoso.com` is signed in and in the administrator's role:</span></span>

![Capture d’écran montrant admin@contoso.com connecté](secure-data/_static/admin.png)

<span data-ttu-id="b9d56-324">L’administrateur a tous les privilèges.</span><span class="sxs-lookup"><span data-stu-id="b9d56-324">The administrator has all privileges.</span></span> <span data-ttu-id="b9d56-325">Elle peut lire/modifier/supprimer un contact et modifier l’état de contacts.</span><span class="sxs-lookup"><span data-stu-id="b9d56-325">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="b9d56-326">L’application a été créée par la [génération](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) de modèles automatique du modèle de `Contact` suivant :</span><span class="sxs-lookup"><span data-stu-id="b9d56-326">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="b9d56-327">L’exemple contient les gestionnaires d’autorisation suivants :</span><span class="sxs-lookup"><span data-stu-id="b9d56-327">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="b9d56-328">`ContactIsOwnerAuthorizationHandler`: garantit qu’un utilisateur peut uniquement modifier ses données.</span><span class="sxs-lookup"><span data-stu-id="b9d56-328">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="b9d56-329">`ContactManagerAuthorizationHandler`: permet aux gestionnaires d’approuver ou de rejeter les contacts.</span><span class="sxs-lookup"><span data-stu-id="b9d56-329">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="b9d56-330">`ContactAdministratorsAuthorizationHandler`: permet aux administrateurs d’approuver ou de refuser des contacts, ainsi que de modifier/supprimer des contacts.</span><span class="sxs-lookup"><span data-stu-id="b9d56-330">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b9d56-331">Configuration requise</span><span class="sxs-lookup"><span data-stu-id="b9d56-331">Prerequisites</span></span>

<span data-ttu-id="b9d56-332">Ce didacticiel est avancé.</span><span class="sxs-lookup"><span data-stu-id="b9d56-332">This tutorial is advanced.</span></span> <span data-ttu-id="b9d56-333">Vous devez être familiarisé avec :</span><span class="sxs-lookup"><span data-stu-id="b9d56-333">You should be familiar with:</span></span>

* [<span data-ttu-id="b9d56-334">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b9d56-334">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="b9d56-335">Authentification</span><span class="sxs-lookup"><span data-stu-id="b9d56-335">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="b9d56-336">Confirmation de compte et récupération de mot de passe</span><span class="sxs-lookup"><span data-stu-id="b9d56-336">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="b9d56-337">Autorisation</span><span class="sxs-lookup"><span data-stu-id="b9d56-337">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="b9d56-338">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="b9d56-338">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="b9d56-339">Le démarrage et l’application terminée</span><span class="sxs-lookup"><span data-stu-id="b9d56-339">The starter and completed app</span></span>

<span data-ttu-id="b9d56-340">[Téléchargez](xref:index#how-to-download-a-sample) l’application [terminée](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) .</span><span class="sxs-lookup"><span data-stu-id="b9d56-340">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app.</span></span> <span data-ttu-id="b9d56-341">[Testez](#test-the-completed-app) l’application terminée afin de vous familiariser avec ses fonctionnalités de sécurité.</span><span class="sxs-lookup"><span data-stu-id="b9d56-341">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="b9d56-342">L’application de démarrage</span><span class="sxs-lookup"><span data-stu-id="b9d56-342">The starter app</span></span>

<span data-ttu-id="b9d56-343">[Téléchargez](xref:index#how-to-download-a-sample) l’application de [démarrage](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) .</span><span class="sxs-lookup"><span data-stu-id="b9d56-343">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.</span></span>

<span data-ttu-id="b9d56-344">Exécutez l’application, appuyez sur le lien **ContactManager** et vérifiez que vous pouvez créer, modifier et supprimer un contact.</span><span class="sxs-lookup"><span data-stu-id="b9d56-344">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="b9d56-345">Sécuriser les données utilisateur</span><span class="sxs-lookup"><span data-stu-id="b9d56-345">Secure user data</span></span>

<span data-ttu-id="b9d56-346">Les sections suivantes ont toutes les principales étapes pour créer l’application de données utilisateur sécurisée.</span><span class="sxs-lookup"><span data-stu-id="b9d56-346">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="b9d56-347">Il peut s’avérer utile pour faire référence au projet terminé.</span><span class="sxs-lookup"><span data-stu-id="b9d56-347">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="b9d56-348">Lier les données de contact à l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="b9d56-348">Tie the contact data to the user</span></span>

<span data-ttu-id="b9d56-349">Utilisez l’IDENTIFIant utilisateur de l' [identité](xref:security/authentication/identity) ASP.net pour vous assurer que les utilisateurs peuvent modifier leurs données, mais pas les données d’autres utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="b9d56-349">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="b9d56-350">Ajoutez `OwnerID` et `ContactStatus` au modèle `Contact` :</span><span class="sxs-lookup"><span data-stu-id="b9d56-350">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="b9d56-351">`OwnerID` est l’ID de l’utilisateur de la table `AspNetUser` dans la base de données d' [identité](xref:security/authentication/identity) .</span><span class="sxs-lookup"><span data-stu-id="b9d56-351">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="b9d56-352">Le champ `Status` détermine si un contact est visible par les utilisateurs généraux.</span><span class="sxs-lookup"><span data-stu-id="b9d56-352">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="b9d56-353">Créer une nouvelle migration et mettre à jour de la base de données :</span><span class="sxs-lookup"><span data-stu-id="b9d56-353">Create a new migration and update the database:</span></span>

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="b9d56-354">Ajouter des services de rôle à l’identité</span><span class="sxs-lookup"><span data-stu-id="b9d56-354">Add Role services to Identity</span></span>

<span data-ttu-id="b9d56-355">Ajoutez [rôles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) pour ajouter des services de rôle :</span><span class="sxs-lookup"><span data-stu-id="b9d56-355">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="b9d56-356">Demander aux utilisateurs authentifiés</span><span class="sxs-lookup"><span data-stu-id="b9d56-356">Require authenticated users</span></span>

<span data-ttu-id="b9d56-357">Définir la stratégie d’authentification par défaut pour exiger l’authentification des utilisateurs :</span><span class="sxs-lookup"><span data-stu-id="b9d56-357">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="b9d56-358">Vous pouvez refuser l’authentification au niveau de la page Razor, du contrôleur ou de la méthode d’action avec l’attribut `[AllowAnonymous]`.</span><span class="sxs-lookup"><span data-stu-id="b9d56-358">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="b9d56-359">Définition de la stratégie d’authentification par défaut pour les utilisateurs doivent être authentifiés protège nouvellement ajouté les Pages Razor et les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="b9d56-359">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="b9d56-360">L’authentification requise par défaut est plus sécurisée que le fait de s’appuyer sur de nouveaux contrôleurs et Razor Pages d’inclure l’attribut `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="b9d56-360">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="b9d56-361">Ajoutez [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) aux pages d’index, à propos de et contact afin que les utilisateurs anonymes puissent obtenir des informations sur le site avant de s’inscrire.</span><span class="sxs-lookup"><span data-stu-id="b9d56-361">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="b9d56-362">Configurer le compte de test</span><span class="sxs-lookup"><span data-stu-id="b9d56-362">Configure the test account</span></span>

<span data-ttu-id="b9d56-363">La classe `SeedData` crée deux comptes : administrateur et gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="b9d56-363">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="b9d56-364">Utilisez l' [outil secret Manager](xref:security/app-secrets) pour définir un mot de passe pour ces comptes.</span><span class="sxs-lookup"><span data-stu-id="b9d56-364">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="b9d56-365">Définissez le mot de passe à partir du répertoire du projet (répertoire contenant *Program.cs*) :</span><span class="sxs-lookup"><span data-stu-id="b9d56-365">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="b9d56-366">Si un mot de passe fort n’est pas spécifié, une exception est levée lors de l’appel de `SeedData.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="b9d56-366">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="b9d56-367">Mettez à jour `Main` pour utiliser le mot de passe de test :</span><span class="sxs-lookup"><span data-stu-id="b9d56-367">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="b9d56-368">Créez les comptes de test et de mettre à jour les contacts</span><span class="sxs-lookup"><span data-stu-id="b9d56-368">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="b9d56-369">Mettez à jour la méthode `Initialize` dans la classe `SeedData` pour créer les comptes de test :</span><span class="sxs-lookup"><span data-stu-id="b9d56-369">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="b9d56-370">Ajoutez l’ID d’utilisateur de l’administrateur et `ContactStatus` aux contacts.</span><span class="sxs-lookup"><span data-stu-id="b9d56-370">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="b9d56-371">Rendre des contacts « Envoyé » et un « rejeté ».</span><span class="sxs-lookup"><span data-stu-id="b9d56-371">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="b9d56-372">Ajoutez l’ID d’utilisateur et l’état pour tous les contacts.</span><span class="sxs-lookup"><span data-stu-id="b9d56-372">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="b9d56-373">Un seul contact est affiché :</span><span class="sxs-lookup"><span data-stu-id="b9d56-373">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="b9d56-374">Créer le propriétaire, le gestionnaire et gestionnaires d’autorisations d’administrateur</span><span class="sxs-lookup"><span data-stu-id="b9d56-374">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="b9d56-375">Créez un dossier *authorization* et créez une classe `ContactIsOwnerAuthorizationHandler` dans celui-ci.</span><span class="sxs-lookup"><span data-stu-id="b9d56-375">Create an *Authorization* folder and create a `ContactIsOwnerAuthorizationHandler` class in it.</span></span> <span data-ttu-id="b9d56-376">Le `ContactIsOwnerAuthorizationHandler` vérifie que l’utilisateur agissant sur une ressource possède la ressource.</span><span class="sxs-lookup"><span data-stu-id="b9d56-376">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="b9d56-377">Le `ContactIsOwnerAuthorizationHandler` appelle le [contexte. Réussie](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) si l’utilisateur authentifié actuel est le propriétaire du contact.</span><span class="sxs-lookup"><span data-stu-id="b9d56-377">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="b9d56-378">Les gestionnaires d’autorisation généralement :</span><span class="sxs-lookup"><span data-stu-id="b9d56-378">Authorization handlers generally:</span></span>

* <span data-ttu-id="b9d56-379">Retourne `context.Succeed` lorsque la configuration requise est respectée.</span><span class="sxs-lookup"><span data-stu-id="b9d56-379">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="b9d56-380">Retourne `Task.CompletedTask` lorsque les spécifications ne sont pas respectées.</span><span class="sxs-lookup"><span data-stu-id="b9d56-380">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="b9d56-381">`Task.CompletedTask` n’a pas réussi ou a échoué&mdash;il permet l’exécution d’autres gestionnaires d’autorisations.</span><span class="sxs-lookup"><span data-stu-id="b9d56-381">`Task.CompletedTask` is not success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="b9d56-382">Si vous devez faire échouer explicitement, retournez le [contexte. Échoue](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="b9d56-382">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="b9d56-383">L’application permet aux propriétaires de contact de modifier, supprimer ou créer leurs propres données.</span><span class="sxs-lookup"><span data-stu-id="b9d56-383">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="b9d56-384">`ContactIsOwnerAuthorizationHandler` n’a pas besoin de vérifier l’opération passée dans le paramètre d’exigence.</span><span class="sxs-lookup"><span data-stu-id="b9d56-384">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="b9d56-385">Créer un gestionnaire d’autorisation de gestionnaire</span><span class="sxs-lookup"><span data-stu-id="b9d56-385">Create a manager authorization handler</span></span>

<span data-ttu-id="b9d56-386">Créez une classe `ContactManagerAuthorizationHandler` dans le dossier *authorization* .</span><span class="sxs-lookup"><span data-stu-id="b9d56-386">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="b9d56-387">Le `ContactManagerAuthorizationHandler` vérifie que l’utilisateur agissant sur la ressource est un responsable.</span><span class="sxs-lookup"><span data-stu-id="b9d56-387">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="b9d56-388">Seuls les responsables peuvent approuver ou rejeter les modifications de contenu (nouvelle ou modifiées).</span><span class="sxs-lookup"><span data-stu-id="b9d56-388">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="b9d56-389">Créer un gestionnaire d’autorisations d’administrateur</span><span class="sxs-lookup"><span data-stu-id="b9d56-389">Create an administrator authorization handler</span></span>

<span data-ttu-id="b9d56-390">Créez une classe `ContactAdministratorsAuthorizationHandler` dans le dossier *authorization* .</span><span class="sxs-lookup"><span data-stu-id="b9d56-390">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="b9d56-391">Le `ContactAdministratorsAuthorizationHandler` vérifie que l’utilisateur agissant sur la ressource est un administrateur.</span><span class="sxs-lookup"><span data-stu-id="b9d56-391">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="b9d56-392">Administrateur peut effectuer toutes les opérations.</span><span class="sxs-lookup"><span data-stu-id="b9d56-392">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="b9d56-393">Inscrire les gestionnaires d’autorisation</span><span class="sxs-lookup"><span data-stu-id="b9d56-393">Register the authorization handlers</span></span>

<span data-ttu-id="b9d56-394">Les services qui utilisent Entity Framework Core doivent être inscrits pour l' [injection de dépendances](xref:fundamentals/dependency-injection) à l’aide de [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="b9d56-394">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="b9d56-395">Le `ContactIsOwnerAuthorizationHandler` utilise ASP.NET Core [identité](xref:security/authentication/identity), qui repose sur Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b9d56-395">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="b9d56-396">Inscrivez les gestionnaires auprès de la collection de services afin qu’ils soient disponibles pour le `ContactsController` par le biais de l' [injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="b9d56-396">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="b9d56-397">Ajoutez le code suivant à la fin de `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b9d56-397">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="b9d56-398">les `ContactAdministratorsAuthorizationHandler` et les `ContactManagerAuthorizationHandler` sont ajoutés en tant que singletons.</span><span class="sxs-lookup"><span data-stu-id="b9d56-398">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="b9d56-399">Il s’agit de singletons, car ils n’utilisent pas EF et toutes les informations nécessaires sont dans le paramètre `Context` de la méthode `HandleRequirementAsync`.</span><span class="sxs-lookup"><span data-stu-id="b9d56-399">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="b9d56-400">Autorisation de la prise en charge</span><span class="sxs-lookup"><span data-stu-id="b9d56-400">Support authorization</span></span>

<span data-ttu-id="b9d56-401">Dans cette section, vous mettez à jour les Pages Razor et ajoutez une classe de configuration requise des opérations.</span><span class="sxs-lookup"><span data-stu-id="b9d56-401">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="b9d56-402">Passez en revue la classe de configuration requise de contact des opérations</span><span class="sxs-lookup"><span data-stu-id="b9d56-402">Review the contact operations requirements class</span></span>

<span data-ttu-id="b9d56-403">Passez en revue la classe `ContactOperations`.</span><span class="sxs-lookup"><span data-stu-id="b9d56-403">Review the `ContactOperations` class.</span></span> <span data-ttu-id="b9d56-404">Cette classe contient les exigences de l’application prend en charge :</span><span class="sxs-lookup"><span data-stu-id="b9d56-404">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="b9d56-405">Créer une classe de base pour les Pages Razor de Contacts</span><span class="sxs-lookup"><span data-stu-id="b9d56-405">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="b9d56-406">Créer une classe de base qui contient les services utilisés dans les Pages Razor de contacts.</span><span class="sxs-lookup"><span data-stu-id="b9d56-406">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="b9d56-407">La classe de base place le code d’initialisation dans un emplacement :</span><span class="sxs-lookup"><span data-stu-id="b9d56-407">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="b9d56-408">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="b9d56-408">The preceding code:</span></span>

* <span data-ttu-id="b9d56-409">Ajoute le service `IAuthorizationService` pour accéder aux gestionnaires d’autorisations.</span><span class="sxs-lookup"><span data-stu-id="b9d56-409">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="b9d56-410">Ajoute l’identité `UserManager` service.</span><span class="sxs-lookup"><span data-stu-id="b9d56-410">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="b9d56-411">Ajoutez la `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="b9d56-411">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="b9d56-412">Mettre à jour le CreateModel</span><span class="sxs-lookup"><span data-stu-id="b9d56-412">Update the CreateModel</span></span>

<span data-ttu-id="b9d56-413">Mettez à jour le constructeur de modèle de page de création pour utiliser la classe de base `DI_BasePageModel` :</span><span class="sxs-lookup"><span data-stu-id="b9d56-413">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="b9d56-414">Mettez à jour la méthode `CreateModel.OnPostAsync` pour :</span><span class="sxs-lookup"><span data-stu-id="b9d56-414">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="b9d56-415">Ajoutez l’ID d’utilisateur au modèle de `Contact`.</span><span class="sxs-lookup"><span data-stu-id="b9d56-415">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="b9d56-416">Appeler le Gestionnaire d’autorisation pour vérifier que l’utilisateur est autorisé à créer des contacts.</span><span class="sxs-lookup"><span data-stu-id="b9d56-416">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="b9d56-417">Mettre à jour le IndexModel</span><span class="sxs-lookup"><span data-stu-id="b9d56-417">Update the IndexModel</span></span>

<span data-ttu-id="b9d56-418">Mettez à jour la méthode `OnGetAsync` afin que seuls les contacts approuvés soient affichés pour les utilisateurs généraux :</span><span class="sxs-lookup"><span data-stu-id="b9d56-418">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="b9d56-419">Mettre à jour le EditModel</span><span class="sxs-lookup"><span data-stu-id="b9d56-419">Update the EditModel</span></span>

<span data-ttu-id="b9d56-420">Ajoutez un gestionnaire d’autorisation pour vérifier que l’utilisateur propriétaire du contact.</span><span class="sxs-lookup"><span data-stu-id="b9d56-420">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="b9d56-421">Étant donné que l’autorisation des ressources est en cours de validation, l’attribut `[Authorize]` n’est pas suffisant.</span><span class="sxs-lookup"><span data-stu-id="b9d56-421">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="b9d56-422">L’application n’a pas accès à la ressource lors de l’évaluation des attributs.</span><span class="sxs-lookup"><span data-stu-id="b9d56-422">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="b9d56-423">Autorisation basée sur la ressource doit être impérative.</span><span class="sxs-lookup"><span data-stu-id="b9d56-423">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="b9d56-424">Vérifications doivent être effectuées une fois que l’application a accès à la ressource, en le chargeant dans le modèle de page ou en le chargeant dans le gestionnaire lui-même.</span><span class="sxs-lookup"><span data-stu-id="b9d56-424">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="b9d56-425">Vous accédez fréquemment la ressource en passant la clé de ressource.</span><span class="sxs-lookup"><span data-stu-id="b9d56-425">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="b9d56-426">Mettre à jour le DeleteModel</span><span class="sxs-lookup"><span data-stu-id="b9d56-426">Update the DeleteModel</span></span>

<span data-ttu-id="b9d56-427">Mettre à jour le modèle de page delete pour utiliser le Gestionnaire d’autorisation pour vérifier que l’utilisateur a l’autorisation de suppression sur le contact.</span><span class="sxs-lookup"><span data-stu-id="b9d56-427">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="b9d56-428">Injecter le service d’autorisation dans les vues</span><span class="sxs-lookup"><span data-stu-id="b9d56-428">Inject the authorization service into the views</span></span>

<span data-ttu-id="b9d56-429">Actuellement, le montre l’interface utilisateur modifie et supprime des liens pour les contacts de que l’utilisateur ne peut pas modifier.</span><span class="sxs-lookup"><span data-stu-id="b9d56-429">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="b9d56-430">Injecter le service d’autorisation dans le fichier *views/_ViewImports. cshtml* afin qu’il soit disponible pour tous les affichages :</span><span class="sxs-lookup"><span data-stu-id="b9d56-430">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="b9d56-431">Le balisage précédent ajoute plusieurs instructions `using`.</span><span class="sxs-lookup"><span data-stu-id="b9d56-431">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="b9d56-432">Mettez à jour les liens **modifier** et **supprimer** dans *pages/contacts/index. cshtml* afin qu’ils soient affichés uniquement pour les utilisateurs disposant des autorisations appropriées :</span><span class="sxs-lookup"><span data-stu-id="b9d56-432">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="b9d56-433">Masquage des liens à partir des utilisateurs qui ne sont pas autorisés à modifier les données ne sécuriser l’application.</span><span class="sxs-lookup"><span data-stu-id="b9d56-433">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="b9d56-434">Masquage des liens rend l’application plus conviviale en affichant les liens ne sont valides.</span><span class="sxs-lookup"><span data-stu-id="b9d56-434">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="b9d56-435">Les utilisateurs peuvent hack l’URL générées pour appeler modifier et supprimer des opérations sur les données qu’ils ne possèdent pas.</span><span class="sxs-lookup"><span data-stu-id="b9d56-435">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="b9d56-436">Le contrôleur ou une Page Razor doit appliquer les vérifications d’accès pour sécuriser les données.</span><span class="sxs-lookup"><span data-stu-id="b9d56-436">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="b9d56-437">Détails de la mise à jour</span><span class="sxs-lookup"><span data-stu-id="b9d56-437">Update Details</span></span>

<span data-ttu-id="b9d56-438">Mettre à jour l’affichage des détails pour les responsables peuvent approuver ou rejeter des contacts :</span><span class="sxs-lookup"><span data-stu-id="b9d56-438">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="b9d56-439">Mettre à jour le modèle de page de détails :</span><span class="sxs-lookup"><span data-stu-id="b9d56-439">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="b9d56-440">Ajouter ou supprimer un utilisateur à un rôle</span><span class="sxs-lookup"><span data-stu-id="b9d56-440">Add or remove a user to a role</span></span>

<span data-ttu-id="b9d56-441">Consultez [ce numéro](https://github.com/aspnet/AspNetCore.Docs/issues/8502) pour plus d’informations sur :</span><span class="sxs-lookup"><span data-stu-id="b9d56-441">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="b9d56-442">Suppression de privilèges à partir d’un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b9d56-442">Removing privileges from a user.</span></span> <span data-ttu-id="b9d56-443">Par exemple, la désactivation d’un utilisateur dans une application de conversation.</span><span class="sxs-lookup"><span data-stu-id="b9d56-443">For example, muting a user in a chat app.</span></span>
* <span data-ttu-id="b9d56-444">Ajout des privilèges à un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b9d56-444">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="b9d56-445">Tester l’application terminée</span><span class="sxs-lookup"><span data-stu-id="b9d56-445">Test the completed app</span></span>

<span data-ttu-id="b9d56-446">Si vous n’avez pas encore défini de mot de passe pour les comptes d’utilisateurs amorcés, utilisez l' [outil secret Manager](xref:security/app-secrets#secret-manager) pour définir un mot de passe :</span><span class="sxs-lookup"><span data-stu-id="b9d56-446">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="b9d56-447">Choisissez un mot de passe fort : utilisez huit ou plus caractères et au moins un caractère majuscule, nombre et symboles.</span><span class="sxs-lookup"><span data-stu-id="b9d56-447">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="b9d56-448">Par exemple, `Passw0rd!` répond aux exigences de mot de passe fort.</span><span class="sxs-lookup"><span data-stu-id="b9d56-448">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="b9d56-449">Exécutez la commande suivante à partir du dossier du projet, où `<PW>` est le mot de passe :</span><span class="sxs-lookup"><span data-stu-id="b9d56-449">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

* <span data-ttu-id="b9d56-450">Supprimer et mettre à jour la base de données</span><span class="sxs-lookup"><span data-stu-id="b9d56-450">Drop and update the Database</span></span>

  ```dotnetcli
  dotnet ef database drop -f
  dotnet ef database update  
  ```

* <span data-ttu-id="b9d56-451">Redémarrez l’application pour amorcer la base de données.</span><span class="sxs-lookup"><span data-stu-id="b9d56-451">Restart the app to seed the database.</span></span>

<span data-ttu-id="b9d56-452">Un moyen simple de tester l’application terminée consiste à lancer les trois différents navigateurs (ou incognito/InPrivate sessions).</span><span class="sxs-lookup"><span data-stu-id="b9d56-452">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="b9d56-453">Dans un navigateur, inscrivez un nouvel utilisateur (par exemple, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="b9d56-453">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="b9d56-454">Connectez-vous à chaque navigateur avec un autre utilisateur.</span><span class="sxs-lookup"><span data-stu-id="b9d56-454">Sign in to each browser with a different user.</span></span> <span data-ttu-id="b9d56-455">Vérifiez les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="b9d56-455">Verify the following operations:</span></span>

* <span data-ttu-id="b9d56-456">Les utilisateurs inscrits peuvent afficher toutes les données de contact approuvées.</span><span class="sxs-lookup"><span data-stu-id="b9d56-456">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="b9d56-457">Les utilisateurs inscrits peuvent modifier ou supprimer leurs propres données.</span><span class="sxs-lookup"><span data-stu-id="b9d56-457">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="b9d56-458">Gestionnaires peuvent approuver/rejeter les données de contact.</span><span class="sxs-lookup"><span data-stu-id="b9d56-458">Managers can approve/reject contact data.</span></span> <span data-ttu-id="b9d56-459">La vue `Details` affiche les boutons **approuver** et **rejeter** .</span><span class="sxs-lookup"><span data-stu-id="b9d56-459">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="b9d56-460">Les administrateurs peuvent approuver/rejeter et modifier ou de supprimer toutes les données.</span><span class="sxs-lookup"><span data-stu-id="b9d56-460">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="b9d56-461">Utilisateur</span><span class="sxs-lookup"><span data-stu-id="b9d56-461">User</span></span>                | <span data-ttu-id="b9d56-462">Amorcée par l’application</span><span class="sxs-lookup"><span data-stu-id="b9d56-462">Seeded by the app</span></span> | <span data-ttu-id="b9d56-463">Options</span><span class="sxs-lookup"><span data-stu-id="b9d56-463">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="b9d56-464">Non</span><span class="sxs-lookup"><span data-stu-id="b9d56-464">No</span></span>                | <span data-ttu-id="b9d56-465">Modifier/supprimer les données propres.</span><span class="sxs-lookup"><span data-stu-id="b9d56-465">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="b9d56-466">Oui</span><span class="sxs-lookup"><span data-stu-id="b9d56-466">Yes</span></span>               | <span data-ttu-id="b9d56-467">Approuver/rejeter et modifier ou de supprimer des données propres.</span><span class="sxs-lookup"><span data-stu-id="b9d56-467">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="b9d56-468">Oui</span><span class="sxs-lookup"><span data-stu-id="b9d56-468">Yes</span></span>               | <span data-ttu-id="b9d56-469">Approuver/rejeter et de modifier ou de supprimer toutes les données.</span><span class="sxs-lookup"><span data-stu-id="b9d56-469">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="b9d56-470">Créer un contact dans le navigateur de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="b9d56-470">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="b9d56-471">Copiez l’URL pour supprimer et modifier à partir du contact de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="b9d56-471">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="b9d56-472">Collez ces liens dans le navigateur de l’utilisateur de test pour vérifier que l’utilisateur de test ne peut pas effectuer ces opérations.</span><span class="sxs-lookup"><span data-stu-id="b9d56-472">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="b9d56-473">Créer l’application de démarrage</span><span class="sxs-lookup"><span data-stu-id="b9d56-473">Create the starter app</span></span>

* <span data-ttu-id="b9d56-474">Créer une application Pages Razor nommée « ContactManager »</span><span class="sxs-lookup"><span data-stu-id="b9d56-474">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="b9d56-475">Créez l’application avec des **comptes d’utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="b9d56-475">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="b9d56-476">Nommez-Le « ContactManager » afin de l’espace de noms correspond à l’espace de noms utilisé dans l’exemple.</span><span class="sxs-lookup"><span data-stu-id="b9d56-476">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="b9d56-477">`-uld` spécifie la base de données locale au lieu de SQLite</span><span class="sxs-lookup"><span data-stu-id="b9d56-477">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="b9d56-478">Ajouter des *modèles/contact. cs*:</span><span class="sxs-lookup"><span data-stu-id="b9d56-478">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="b9d56-479">Structurez le modèle de `Contact`.</span><span class="sxs-lookup"><span data-stu-id="b9d56-479">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="b9d56-480">La migration initiale de créer et mettre à jour de la base de données :</span><span class="sxs-lookup"><span data-stu-id="b9d56-480">Create initial migration and update the database:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* <span data-ttu-id="b9d56-481">Mettez à jour l’ancre **ContactManager** dans le fichier *pages/_Layout. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="b9d56-481">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* <span data-ttu-id="b9d56-482">Testez l’application en création, modification et suppression d’un contact</span><span class="sxs-lookup"><span data-stu-id="b9d56-482">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="b9d56-483">Amorcer la base de données</span><span class="sxs-lookup"><span data-stu-id="b9d56-483">Seed the database</span></span>

<span data-ttu-id="b9d56-484">Ajoutez la classe [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) au dossier de *données* .</span><span class="sxs-lookup"><span data-stu-id="b9d56-484">Add the [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="b9d56-485">Appelez `SeedData.Initialize` à partir de `Main`:</span><span class="sxs-lookup"><span data-stu-id="b9d56-485">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="b9d56-486">Tester l’application d’amorçage la base de données.</span><span class="sxs-lookup"><span data-stu-id="b9d56-486">Test that the app seeded the database.</span></span> <span data-ttu-id="b9d56-487">S’il existe des lignes dans la base de données de contact, la méthode seed ne s’exécute pas.</span><span class="sxs-lookup"><span data-stu-id="b9d56-487">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="b9d56-488">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="b9d56-488">Additional resources</span></span>

* [<span data-ttu-id="b9d56-489">Créer une application Web .NET Core et SQL Database dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b9d56-489">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="b9d56-490">[ASP.net Core laboratoire d’autorisation](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="b9d56-490">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="b9d56-491">Ce laboratoire présente plus en détails sur les fonctionnalités de sécurité présentées dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="b9d56-491">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* <xref:security/authorization/introduction>
* [<span data-ttu-id="b9d56-492">Autorisation basée sur une stratégie personnalisée</span><span class="sxs-lookup"><span data-stu-id="b9d56-492">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
