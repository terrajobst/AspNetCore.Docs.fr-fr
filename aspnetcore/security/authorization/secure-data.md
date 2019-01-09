---
title: Créer une application ASP.NET Core avec des données utilisateur protégées par une autorisation
author: rick-anderson
description: Découvrez comment créer une application Pages Razor avec des données utilisateur protégées par une autorisation. Inclut HTTPS, l’authentification, sécurité, ASP.NET Core Identity.
ms.author: riande
ms.date: 12/18/2018
ms.custom: seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: bdba706c1ef24ebe35129cb8bb2d9949196245a1
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098920"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="cf326-104">Créer une application ASP.NET Core avec des données utilisateur protégées par une autorisation</span><span class="sxs-lookup"><span data-stu-id="cf326-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="cf326-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="cf326-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="cf326-106">Consultez [ce fichier PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) pour la version d’ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="cf326-106">See [this PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="cf326-107">La version d’ASP.NET Core 1.1 de ce didacticiel est dans [cela](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) dossier.</span><span class="sxs-lookup"><span data-stu-id="cf326-107">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="cf326-108">L’exemple ASP.NET Core 1.1 du [exemples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="cf326-108">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="cf326-109">Consultez [ce fichier pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="cf326-109">See [this pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="cf326-110">Ce didacticiel montre comment créer une application web ASP.NET Core avec des données utilisateur protégées par une autorisation.</span><span class="sxs-lookup"><span data-stu-id="cf326-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="cf326-111">Il affiche une liste de contacts (inscrits) les utilisateurs authentifiés ont créés.</span><span class="sxs-lookup"><span data-stu-id="cf326-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="cf326-112">Il existe trois groupes de sécurité :</span><span class="sxs-lookup"><span data-stu-id="cf326-112">There are three security groups:</span></span>

* <span data-ttu-id="cf326-113">**Utilisateurs inscrits** peut afficher toutes les données approuvées et peuvent modifier ou supprimer leurs propres données.</span><span class="sxs-lookup"><span data-stu-id="cf326-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="cf326-114">**Gestionnaires de** peuvent approuver ou rejeter des données de contact.</span><span class="sxs-lookup"><span data-stu-id="cf326-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="cf326-115">Seuls les contacts approuvés sont visibles aux utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="cf326-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="cf326-116">**Les administrateurs** peut approuver/rejeter et modifier ou de supprimer toutes les données.</span><span class="sxs-lookup"><span data-stu-id="cf326-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="cf326-117">Dans l’image suivante, l’utilisateur Rick (`rick@example.com`) n’est connecté.</span><span class="sxs-lookup"><span data-stu-id="cf326-117">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="cf326-118">Rick peut uniquement afficher les contacts approuvés et **modifier**/**supprimer**/**créer un nouveau** liens pour ses contacts.</span><span class="sxs-lookup"><span data-stu-id="cf326-118">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="cf326-119">Seul le dernier enregistrement créé par Rick, affiche **modifier** et **supprimer** des liens.</span><span class="sxs-lookup"><span data-stu-id="cf326-119">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="cf326-120">Autres utilisateurs ne voient le dernier enregistrement jusqu'à ce qu’un gestionnaire ou un administrateur modifie le statut « Approved ».</span><span class="sxs-lookup"><span data-stu-id="cf326-120">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Capture d’écran montrant Rick connecté](secure-data/_static/rick.png)

<span data-ttu-id="cf326-122">Dans l’image suivante, `manager@contoso.com` est signé dans et dans le rôle gestionnaires :</span><span class="sxs-lookup"><span data-stu-id="cf326-122">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![Capture d’écran manager@contoso.com connecté](secure-data/_static/manager1.png)

<span data-ttu-id="cf326-124">L’illustration suivante montre les gestionnaires de vue des détails d’un contact :</span><span class="sxs-lookup"><span data-stu-id="cf326-124">The following image shows the managers details view of a contact:</span></span>

![Vue du responsable d’un contact](secure-data/_static/manager.png)

<span data-ttu-id="cf326-126">Le **approuver** et **rejeter** boutons sont affichés uniquement pour les responsables et les administrateurs.</span><span class="sxs-lookup"><span data-stu-id="cf326-126">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="cf326-127">Dans l’image suivante, `admin@contoso.com` est signé dans et dans le rôle Administrateurs :</span><span class="sxs-lookup"><span data-stu-id="cf326-127">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![Capture d’écran admin@contoso.com connecté](secure-data/_static/admin.png)

<span data-ttu-id="cf326-129">L’administrateur a tous les privilèges.</span><span class="sxs-lookup"><span data-stu-id="cf326-129">The administrator has all privileges.</span></span> <span data-ttu-id="cf326-130">Elle peut lire/modifier/supprimer un contact et modifier l’état de contacts.</span><span class="sxs-lookup"><span data-stu-id="cf326-130">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="cf326-131">L’application a été créée par [la structure](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) suit `Contact` modèle :</span><span class="sxs-lookup"><span data-stu-id="cf326-131">The app was created by [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="cf326-132">L’exemple contient les gestionnaires d’autorisation suivants :</span><span class="sxs-lookup"><span data-stu-id="cf326-132">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="cf326-133">`ContactIsOwnerAuthorizationHandler`: Garantit qu’un utilisateur peut modifier uniquement leurs données.</span><span class="sxs-lookup"><span data-stu-id="cf326-133">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="cf326-134">`ContactManagerAuthorizationHandler`: Permet aux gestionnaires d’approuver ou rejeter des contacts.</span><span class="sxs-lookup"><span data-stu-id="cf326-134">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="cf326-135">`ContactAdministratorsAuthorizationHandler`: Permet aux administrateurs d’approuver ou rejeter des contacts et à modifier/supprimer des contacts.</span><span class="sxs-lookup"><span data-stu-id="cf326-135">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cf326-136">Prérequis</span><span class="sxs-lookup"><span data-stu-id="cf326-136">Prerequisites</span></span>

<span data-ttu-id="cf326-137">Ce didacticiel est avancé.</span><span class="sxs-lookup"><span data-stu-id="cf326-137">This tutorial is advanced.</span></span> <span data-ttu-id="cf326-138">Vous devez être familiarisé avec :</span><span class="sxs-lookup"><span data-stu-id="cf326-138">You should be familiar with:</span></span>

* [<span data-ttu-id="cf326-139">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cf326-139">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="cf326-140">Authentification</span><span class="sxs-lookup"><span data-stu-id="cf326-140">Authentication</span></span>](xref:security/authentication/identity)
* [<span data-ttu-id="cf326-141">Confirmation de compte et récupération de mot de passe</span><span class="sxs-lookup"><span data-stu-id="cf326-141">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="cf326-142">Autorisation</span><span class="sxs-lookup"><span data-stu-id="cf326-142">Authorization</span></span>](xref:security/authorization/introduction)
* [<span data-ttu-id="cf326-143">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="cf326-143">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="cf326-144">Dans ASP.NET Core 2.1, `User.IsInRole` échoue lors de l’utilisation `AddDefaultIdentity`.</span><span class="sxs-lookup"><span data-stu-id="cf326-144">In ASP.NET Core 2.1, `User.IsInRole` fails when using `AddDefaultIdentity`.</span></span> <span data-ttu-id="cf326-145">Ce didacticiel utilise `AddDefaultIdentity` et par conséquent nécessite ASP.NET Core 2.2 ou version ultérieure.</span><span class="sxs-lookup"><span data-stu-id="cf326-145">This tutorial uses `AddDefaultIdentity` and therefore requires ASP.NET Core 2.2 or later.</span></span> <span data-ttu-id="cf326-146">Consultez [ce problème GitHub](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) pour une solution de contournement.</span><span class="sxs-lookup"><span data-stu-id="cf326-146">See [this GitHub issue](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) for a work-around.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="cf326-147">Le démarrage et l’application terminée</span><span class="sxs-lookup"><span data-stu-id="cf326-147">The starter and completed app</span></span>

<span data-ttu-id="cf326-148">[Télécharger](xref:index#how-to-download-a-sample) le [terminé](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) application.</span><span class="sxs-lookup"><span data-stu-id="cf326-148">[Download](xref:index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="cf326-149">[Test](#test-the-completed-app) l’application terminée afin de vous familiariser avec ses fonctionnalités de sécurité.</span><span class="sxs-lookup"><span data-stu-id="cf326-149">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="cf326-150">L’application de démarrage</span><span class="sxs-lookup"><span data-stu-id="cf326-150">The starter app</span></span>

<span data-ttu-id="cf326-151">[Télécharger](xref:index#how-to-download-a-sample) le [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) application.</span><span class="sxs-lookup"><span data-stu-id="cf326-151">[Download](xref:index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="cf326-152">Exécutez l’application, appuyez sur la **ContactManager** lien et vérifiez que vous pouvez créer, modifier et supprimer un contact.</span><span class="sxs-lookup"><span data-stu-id="cf326-152">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="cf326-153">Sécuriser les données utilisateur</span><span class="sxs-lookup"><span data-stu-id="cf326-153">Secure user data</span></span>

<span data-ttu-id="cf326-154">Les sections suivantes ont toutes les principales étapes pour créer l’application de données utilisateur sécurisée.</span><span class="sxs-lookup"><span data-stu-id="cf326-154">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="cf326-155">Il peut s’avérer utile pour faire référence au projet terminé.</span><span class="sxs-lookup"><span data-stu-id="cf326-155">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="cf326-156">Lier les données de contact à l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="cf326-156">Tie the contact data to the user</span></span>

<span data-ttu-id="cf326-157">Utilisez ASP.NET [identité](xref:security/authentication/identity) ID d’utilisateur pour garantir les utilisateurs permettre modifier leurs données, mais pas d’autres données utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="cf326-157">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="cf326-158">Ajouter `OwnerID` et `ContactStatus` à la `Contact` modèle :</span><span class="sxs-lookup"><span data-stu-id="cf326-158">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="cf326-159">`OwnerID` est l’ID d’utilisateur à partir de la `AspNetUser` table dans le [identité](xref:security/authentication/identity) base de données.</span><span class="sxs-lookup"><span data-stu-id="cf326-159">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="cf326-160">Le `Status` champ détermine si un contact est visible par les utilisateurs généraux.</span><span class="sxs-lookup"><span data-stu-id="cf326-160">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="cf326-161">Créer une nouvelle migration et mettre à jour de la base de données :</span><span class="sxs-lookup"><span data-stu-id="cf326-161">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="cf326-162">Ajouter des services de rôle à l’identité</span><span class="sxs-lookup"><span data-stu-id="cf326-162">Add Role services to Identity</span></span>

<span data-ttu-id="cf326-163">Ajouter [fonctionnalité Ajouter des rôles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) pour ajouter des services de rôle :</span><span class="sxs-lookup"><span data-stu-id="cf326-163">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="cf326-164">Demander aux utilisateurs authentifiés</span><span class="sxs-lookup"><span data-stu-id="cf326-164">Require authenticated users</span></span>

<span data-ttu-id="cf326-165">Définir la stratégie d’authentification par défaut pour exiger l’authentification des utilisateurs :</span><span class="sxs-lookup"><span data-stu-id="cf326-165">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="cf326-166">Vous pouvez refuser l’authentification au niveau de la méthode Page Razor, de contrôleur ou d’action avec la `[AllowAnonymous]` attribut.</span><span class="sxs-lookup"><span data-stu-id="cf326-166">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="cf326-167">Définition de la stratégie d’authentification par défaut pour les utilisateurs doivent être authentifiés protège nouvellement ajouté les Pages Razor et les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="cf326-167">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="cf326-168">Avec l’authentification requise par défaut est plus sécurisée que de s’appuyer sur de nouveaux contrôleurs et les Pages Razor pour inclure le `[Authorize]` attribut.</span><span class="sxs-lookup"><span data-stu-id="cf326-168">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="cf326-169">Ajouter [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) à l’Index, des pages sur et Contact pour les utilisateurs anonymes peuvent obtenir des informations sur le site avant ils s’inscrivent.</span><span class="sxs-lookup"><span data-stu-id="cf326-169">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="cf326-170">Configurer le compte de test</span><span class="sxs-lookup"><span data-stu-id="cf326-170">Configure the test account</span></span>

<span data-ttu-id="cf326-171">Le `SeedData` classe crée deux comptes : administrateur et gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="cf326-171">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="cf326-172">Utilisez le [outil Secret Manager](xref:security/app-secrets) pour définir un mot de passe pour ces comptes.</span><span class="sxs-lookup"><span data-stu-id="cf326-172">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="cf326-173">Définir le mot de passe à partir du répertoire de projet (le répertoire contenant *Program.cs*) :</span><span class="sxs-lookup"><span data-stu-id="cf326-173">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="cf326-174">Si un mot de passe n’est pas spécifié, une exception est levée lorsque `SeedData.Initialize` est appelée.</span><span class="sxs-lookup"><span data-stu-id="cf326-174">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="cf326-175">Mise à jour `Main` à utiliser le mot de passe de test :</span><span class="sxs-lookup"><span data-stu-id="cf326-175">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="cf326-176">Créez les comptes de test et de mettre à jour les contacts</span><span class="sxs-lookup"><span data-stu-id="cf326-176">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="cf326-177">Mise à jour le `Initialize` méthode dans la `SeedData` classe pour créer les comptes de test :</span><span class="sxs-lookup"><span data-stu-id="cf326-177">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="cf326-178">Ajouter l’ID d’utilisateur administrateur et `ContactStatus` aux contacts.</span><span class="sxs-lookup"><span data-stu-id="cf326-178">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="cf326-179">Rendre des contacts « Envoyé » et un « rejeté ».</span><span class="sxs-lookup"><span data-stu-id="cf326-179">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="cf326-180">Ajoutez l’ID d’utilisateur et l’état pour tous les contacts.</span><span class="sxs-lookup"><span data-stu-id="cf326-180">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="cf326-181">Un seul contact est affiché :</span><span class="sxs-lookup"><span data-stu-id="cf326-181">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="cf326-182">Créer le propriétaire, le gestionnaire et gestionnaires d’autorisations d’administrateur</span><span class="sxs-lookup"><span data-stu-id="cf326-182">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="cf326-183">Créer un `ContactIsOwnerAuthorizationHandler` classe dans le *autorisation* dossier.</span><span class="sxs-lookup"><span data-stu-id="cf326-183">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="cf326-184">Le `ContactIsOwnerAuthorizationHandler` vérifie que l’utilisateur agissant sur une ressource propriétaire de la ressource.</span><span class="sxs-lookup"><span data-stu-id="cf326-184">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="cf326-185">Le `ContactIsOwnerAuthorizationHandler` appels [contexte. Réussir](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) si l’utilisateur authentifié actuel est le propriétaire du contact.</span><span class="sxs-lookup"><span data-stu-id="cf326-185">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="cf326-186">Les gestionnaires d’autorisation généralement :</span><span class="sxs-lookup"><span data-stu-id="cf326-186">Authorization handlers generally:</span></span>

* <span data-ttu-id="cf326-187">Retourner `context.Succeed` lorsque les conditions sont remplies.</span><span class="sxs-lookup"><span data-stu-id="cf326-187">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="cf326-188">Retourner `Task.CompletedTask` lorsque les conditions ne sont pas remplies.</span><span class="sxs-lookup"><span data-stu-id="cf326-188">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="cf326-189">`Task.CompletedTask` est une réussite ou l’échec&mdash;permet d’autres gestionnaires d’autorisation Exécuter.</span><span class="sxs-lookup"><span data-stu-id="cf326-189">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="cf326-190">Si vous devez explicitement échouer, retourner [contexte. Échec](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="cf326-190">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="cf326-191">L’application permet aux propriétaires de contact de modifier, supprimer ou créer leurs propres données.</span><span class="sxs-lookup"><span data-stu-id="cf326-191">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="cf326-192">`ContactIsOwnerAuthorizationHandler` n’a pas besoin vérifier l’opération passée dans le paramètre de condition.</span><span class="sxs-lookup"><span data-stu-id="cf326-192">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="cf326-193">Créer un gestionnaire d’autorisation de gestionnaire</span><span class="sxs-lookup"><span data-stu-id="cf326-193">Create a manager authorization handler</span></span>

<span data-ttu-id="cf326-194">Créer un `ContactManagerAuthorizationHandler` classe dans le *autorisation* dossier.</span><span class="sxs-lookup"><span data-stu-id="cf326-194">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="cf326-195">Le `ContactManagerAuthorizationHandler` vérifie l’utilisateur agissant sur la ressource est un gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="cf326-195">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="cf326-196">Seuls les responsables peuvent approuver ou rejeter les modifications de contenu (nouvelle ou modifiées).</span><span class="sxs-lookup"><span data-stu-id="cf326-196">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="cf326-197">Créer un gestionnaire d’autorisations d’administrateur</span><span class="sxs-lookup"><span data-stu-id="cf326-197">Create an administrator authorization handler</span></span>

<span data-ttu-id="cf326-198">Créer un `ContactAdministratorsAuthorizationHandler` classe dans le *autorisation* dossier.</span><span class="sxs-lookup"><span data-stu-id="cf326-198">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="cf326-199">Le `ContactAdministratorsAuthorizationHandler` vérifie que l’utilisateur agissant sur la ressource est un administrateur.</span><span class="sxs-lookup"><span data-stu-id="cf326-199">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="cf326-200">Administrateur peut effectuer toutes les opérations.</span><span class="sxs-lookup"><span data-stu-id="cf326-200">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="cf326-201">Inscrire les gestionnaires d’autorisation</span><span class="sxs-lookup"><span data-stu-id="cf326-201">Register the authorization handlers</span></span>

<span data-ttu-id="cf326-202">Services à l’aide d’Entity Framework Core doivent être inscrit pour [l’injection de dépendances](xref:fundamentals/dependency-injection) à l’aide de [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="cf326-202">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="cf326-203">Le `ContactIsOwnerAuthorizationHandler` utilise ASP.NET Core [identité](xref:security/authentication/identity), qui est basé sur Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="cf326-203">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="cf326-204">Enregistrer les gestionnaires avec la collection de service afin qu’elles soient disponibles pour le `ContactsController` via [l’injection de dépendances](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="cf326-204">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="cf326-205">Ajoutez le code suivant à la fin de `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="cf326-205">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="cf326-206">`ContactAdministratorsAuthorizationHandler` et `ContactManagerAuthorizationHandler` sont ajoutés en tant que singletons.</span><span class="sxs-lookup"><span data-stu-id="cf326-206">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="cf326-207">Ils sont des singletons, car ils n’utilisent pas EF et toutes les informations nécessaires sont dans le `Context` paramètre de la `HandleRequirementAsync` (méthode).</span><span class="sxs-lookup"><span data-stu-id="cf326-207">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="cf326-208">Autorisation de la prise en charge</span><span class="sxs-lookup"><span data-stu-id="cf326-208">Support authorization</span></span>

<span data-ttu-id="cf326-209">Dans cette section, vous mettez à jour les Pages Razor et ajoutez une classe de configuration requise des opérations.</span><span class="sxs-lookup"><span data-stu-id="cf326-209">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="cf326-210">Passez en revue la classe de configuration requise de contact des opérations</span><span class="sxs-lookup"><span data-stu-id="cf326-210">Review the contact operations requirements class</span></span>

<span data-ttu-id="cf326-211">Examinez la `ContactOperations` classe.</span><span class="sxs-lookup"><span data-stu-id="cf326-211">Review the `ContactOperations` class.</span></span> <span data-ttu-id="cf326-212">Cette classe contient les exigences de l’application prend en charge :</span><span class="sxs-lookup"><span data-stu-id="cf326-212">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="cf326-213">Créer une classe de base pour les Pages Razor de Contacts</span><span class="sxs-lookup"><span data-stu-id="cf326-213">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="cf326-214">Créer une classe de base qui contient les services utilisés dans les Pages Razor de contacts.</span><span class="sxs-lookup"><span data-stu-id="cf326-214">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="cf326-215">La classe de base place le code d’initialisation dans un emplacement :</span><span class="sxs-lookup"><span data-stu-id="cf326-215">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="cf326-216">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="cf326-216">The preceding code:</span></span>

* <span data-ttu-id="cf326-217">Ajoute le `IAuthorizationService` service d’accéder aux gestionnaires d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="cf326-217">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="cf326-218">Ajoute l’identité `UserManager` service.</span><span class="sxs-lookup"><span data-stu-id="cf326-218">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="cf326-219">Ajoutez la `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="cf326-219">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="cf326-220">Mettre à jour le CreateModel</span><span class="sxs-lookup"><span data-stu-id="cf326-220">Update the CreateModel</span></span>

<span data-ttu-id="cf326-221">Mettre à jour le constructeur de modèle de page de création à utiliser le `DI_BasePageModel` classe de base :</span><span class="sxs-lookup"><span data-stu-id="cf326-221">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="cf326-222">Mise à jour le `CreateModel.OnPostAsync` méthode à :</span><span class="sxs-lookup"><span data-stu-id="cf326-222">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="cf326-223">Ajoutez l’ID utilisateur pour la `Contact` modèle.</span><span class="sxs-lookup"><span data-stu-id="cf326-223">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="cf326-224">Appeler le Gestionnaire d’autorisation pour vérifier que l’utilisateur est autorisé à créer des contacts.</span><span class="sxs-lookup"><span data-stu-id="cf326-224">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="cf326-225">Mettre à jour le IndexModel</span><span class="sxs-lookup"><span data-stu-id="cf326-225">Update the IndexModel</span></span>

<span data-ttu-id="cf326-226">Mise à jour le `OnGetAsync` méthode approuvées uniquement les contacts sont présentés aux utilisateurs généraux :</span><span class="sxs-lookup"><span data-stu-id="cf326-226">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="cf326-227">Mettre à jour le EditModel</span><span class="sxs-lookup"><span data-stu-id="cf326-227">Update the EditModel</span></span>

<span data-ttu-id="cf326-228">Ajoutez un gestionnaire d’autorisation pour vérifier que l’utilisateur propriétaire du contact.</span><span class="sxs-lookup"><span data-stu-id="cf326-228">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="cf326-229">Étant donné que l’autorisation de ressource est en cours de validation, le `[Authorize]` attribut n’est pas suffisant.</span><span class="sxs-lookup"><span data-stu-id="cf326-229">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="cf326-230">L’application n’a pas accès à la ressource lors de l’évaluation des attributs.</span><span class="sxs-lookup"><span data-stu-id="cf326-230">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="cf326-231">Autorisation basée sur la ressource doit être impérative.</span><span class="sxs-lookup"><span data-stu-id="cf326-231">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="cf326-232">Vérifications doivent être effectuées une fois que l’application a accès à la ressource, en le chargeant dans le modèle de page ou en le chargeant dans le gestionnaire lui-même.</span><span class="sxs-lookup"><span data-stu-id="cf326-232">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="cf326-233">Vous accédez fréquemment la ressource en passant la clé de ressource.</span><span class="sxs-lookup"><span data-stu-id="cf326-233">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="cf326-234">Mettre à jour le DeleteModel</span><span class="sxs-lookup"><span data-stu-id="cf326-234">Update the DeleteModel</span></span>

<span data-ttu-id="cf326-235">Mettre à jour le modèle de page delete pour utiliser le Gestionnaire d’autorisation pour vérifier que l’utilisateur a l’autorisation de suppression sur le contact.</span><span class="sxs-lookup"><span data-stu-id="cf326-235">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="cf326-236">Injecter le service d’autorisation dans les vues</span><span class="sxs-lookup"><span data-stu-id="cf326-236">Inject the authorization service into the views</span></span>

<span data-ttu-id="cf326-237">Actuellement, le montre l’interface utilisateur modifie et supprime des liens pour les contacts de que l’utilisateur ne peut pas modifier.</span><span class="sxs-lookup"><span data-stu-id="cf326-237">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="cf326-238">Injecter le service d’autorisation dans le *Views/_viewimports.cshtml* afin qu’il soit disponible pour toutes les vues de fichiers :</span><span class="sxs-lookup"><span data-stu-id="cf326-238">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="cf326-239">Le balisage précédent ajoute plusieurs `using` instructions.</span><span class="sxs-lookup"><span data-stu-id="cf326-239">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="cf326-240">Mise à jour le **modifier** et **supprimer** lie dans *Pages/Contacts/Index.cshtml* afin d’être affichées uniquement pour les utilisateurs disposant des autorisations appropriées :</span><span class="sxs-lookup"><span data-stu-id="cf326-240">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="cf326-241">Masquage des liens à partir des utilisateurs qui ne sont pas autorisés à modifier les données ne sécuriser l’application.</span><span class="sxs-lookup"><span data-stu-id="cf326-241">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="cf326-242">Masquage des liens rend l’application plus conviviale en affichant les liens ne sont valides.</span><span class="sxs-lookup"><span data-stu-id="cf326-242">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="cf326-243">Les utilisateurs peuvent hack l’URL générées pour appeler modifier et supprimer des opérations sur les données qu’ils ne possèdent pas.</span><span class="sxs-lookup"><span data-stu-id="cf326-243">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="cf326-244">Le contrôleur ou une Page Razor doit appliquer les vérifications d’accès pour sécuriser les données.</span><span class="sxs-lookup"><span data-stu-id="cf326-244">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="cf326-245">Détails de la mise à jour</span><span class="sxs-lookup"><span data-stu-id="cf326-245">Update Details</span></span>

<span data-ttu-id="cf326-246">Mettre à jour l’affichage des détails pour les responsables peuvent approuver ou rejeter des contacts :</span><span class="sxs-lookup"><span data-stu-id="cf326-246">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="cf326-247">Mettre à jour le modèle de page de détails :</span><span class="sxs-lookup"><span data-stu-id="cf326-247">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a><span data-ttu-id="cf326-248">Ajouter ou supprimer un utilisateur à un rôle</span><span class="sxs-lookup"><span data-stu-id="cf326-248">Add or remove a user to a role</span></span>

<span data-ttu-id="cf326-249">Consultez [ce problème](https://github.com/aspnet/Docs/issues/8502) pour plus d’informations sur :</span><span class="sxs-lookup"><span data-stu-id="cf326-249">See [this issue](https://github.com/aspnet/Docs/issues/8502) for information on:</span></span>

* <span data-ttu-id="cf326-250">Suppression de privilèges à partir d’un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="cf326-250">Removing privileges from a user.</span></span> <span data-ttu-id="cf326-251">Désactivation par exemple un utilisateur dans une application de conversation.</span><span class="sxs-lookup"><span data-stu-id="cf326-251">For example muting a user in a chat app.</span></span>
* <span data-ttu-id="cf326-252">Ajout des privilèges à un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="cf326-252">Adding privileges to a user.</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="cf326-253">Tester l’application terminée</span><span class="sxs-lookup"><span data-stu-id="cf326-253">Test the completed app</span></span>

<span data-ttu-id="cf326-254">Si vous n’avez pas déjà défini un mot de passe pour les comptes d’utilisateur amorcée, utilisez le [outil Secret Manager](xref:security/app-secrets#secret-manager) pour définir un mot de passe :</span><span class="sxs-lookup"><span data-stu-id="cf326-254">If you haven't already set a password for seeded user accounts, use the [Secret Manager tool](xref:security/app-secrets#secret-manager) to set a password:</span></span>

* <span data-ttu-id="cf326-255">Choisissez un mot de passe fort : Utiliser huit ou plus de caractères et au moins un caractère majuscule, numéro et de symboles.</span><span class="sxs-lookup"><span data-stu-id="cf326-255">Choose a strong password: Use eight or more characters and at least one upper-case character, number, and symbol.</span></span> <span data-ttu-id="cf326-256">Par exemple, `Passw0rd!` répond aux exigences de mot de passe fort.</span><span class="sxs-lookup"><span data-stu-id="cf326-256">For example, `Passw0rd!` meets the strong password requirements.</span></span>
* <span data-ttu-id="cf326-257">Exécutez la commande suivante à partir du dossier du projet, où `<PW>` est le mot de passe :</span><span class="sxs-lookup"><span data-stu-id="cf326-257">Execute the following command from the project's folder, where `<PW>` is the password:</span></span>

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

<span data-ttu-id="cf326-258">Si l’application a des contacts :</span><span class="sxs-lookup"><span data-stu-id="cf326-258">If the app has contacts:</span></span>

* <span data-ttu-id="cf326-259">Supprimer tous les enregistrements dans la `Contact` table.</span><span class="sxs-lookup"><span data-stu-id="cf326-259">Delete all of the records in the `Contact` table.</span></span>
* <span data-ttu-id="cf326-260">Redémarrez l’application pour amorcer la base de données.</span><span class="sxs-lookup"><span data-stu-id="cf326-260">Restart the app to seed the database.</span></span>

<span data-ttu-id="cf326-261">Un moyen simple de tester l’application terminée consiste à lancer les trois différents navigateurs (ou incognito/InPrivate sessions).</span><span class="sxs-lookup"><span data-stu-id="cf326-261">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate sessions).</span></span> <span data-ttu-id="cf326-262">Dans un navigateur, inscrire un nouvel utilisateur (par exemple, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="cf326-262">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="cf326-263">Connectez-vous à chaque navigateur avec un autre utilisateur.</span><span class="sxs-lookup"><span data-stu-id="cf326-263">Sign in to each browser with a different user.</span></span> <span data-ttu-id="cf326-264">Vérifiez les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="cf326-264">Verify the following operations:</span></span>

* <span data-ttu-id="cf326-265">Les utilisateurs inscrits peuvent afficher toutes les données de contact approuvées.</span><span class="sxs-lookup"><span data-stu-id="cf326-265">Registered users can view all of the approved contact data.</span></span>
* <span data-ttu-id="cf326-266">Les utilisateurs inscrits peuvent modifier ou supprimer leurs propres données.</span><span class="sxs-lookup"><span data-stu-id="cf326-266">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="cf326-267">Gestionnaires peuvent approuver/rejeter les données de contact.</span><span class="sxs-lookup"><span data-stu-id="cf326-267">Managers can approve/reject contact data.</span></span> <span data-ttu-id="cf326-268">Le `Details` afficher montre **approuver** et **rejeter** boutons.</span><span class="sxs-lookup"><span data-stu-id="cf326-268">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="cf326-269">Les administrateurs peuvent approuver/rejeter et modifier ou de supprimer toutes les données.</span><span class="sxs-lookup"><span data-stu-id="cf326-269">Administrators can approve/reject and edit/delete all data.</span></span>

| <span data-ttu-id="cf326-270">Utilisateur</span><span class="sxs-lookup"><span data-stu-id="cf326-270">User</span></span>                | <span data-ttu-id="cf326-271">Amorcée par l’application</span><span class="sxs-lookup"><span data-stu-id="cf326-271">Seeded by the app</span></span> | <span data-ttu-id="cf326-272">Options</span><span class="sxs-lookup"><span data-stu-id="cf326-272">Options</span></span>                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | <span data-ttu-id="cf326-273">Non</span><span class="sxs-lookup"><span data-stu-id="cf326-273">No</span></span>                | <span data-ttu-id="cf326-274">Modifier/supprimer les données propres.</span><span class="sxs-lookup"><span data-stu-id="cf326-274">Edit/delete the own data.</span></span>                |
| manager@contoso.com | <span data-ttu-id="cf326-275">Oui</span><span class="sxs-lookup"><span data-stu-id="cf326-275">Yes</span></span>               | <span data-ttu-id="cf326-276">Approuver/rejeter et modifier ou de supprimer des données propres.</span><span class="sxs-lookup"><span data-stu-id="cf326-276">Approve/reject and edit/delete own data.</span></span> |
| admin@contoso.com   | <span data-ttu-id="cf326-277">Oui</span><span class="sxs-lookup"><span data-stu-id="cf326-277">Yes</span></span>               | <span data-ttu-id="cf326-278">Approuver/rejeter et de modifier ou de supprimer toutes les données.</span><span class="sxs-lookup"><span data-stu-id="cf326-278">Approve/reject and edit/delete all data.</span></span> |

<span data-ttu-id="cf326-279">Créer un contact dans le navigateur de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="cf326-279">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="cf326-280">Copiez l’URL pour supprimer et modifier à partir du contact de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="cf326-280">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="cf326-281">Collez ces liens dans le navigateur de l’utilisateur de test pour vérifier que l’utilisateur de test ne peut pas effectuer ces opérations.</span><span class="sxs-lookup"><span data-stu-id="cf326-281">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="cf326-282">Créer l’application de démarrage</span><span class="sxs-lookup"><span data-stu-id="cf326-282">Create the starter app</span></span>

* <span data-ttu-id="cf326-283">Créer une application Pages Razor nommée « ContactManager »</span><span class="sxs-lookup"><span data-stu-id="cf326-283">Create a Razor Pages app named "ContactManager"</span></span>
  * <span data-ttu-id="cf326-284">Créer l’application avec **comptes d’utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="cf326-284">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="cf326-285">Nommez-Le « ContactManager » afin de l’espace de noms correspond à l’espace de noms utilisé dans l’exemple.</span><span class="sxs-lookup"><span data-stu-id="cf326-285">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
  * <span data-ttu-id="cf326-286">`-uld` Spécifie la base de données locale au lieu de SQLite</span><span class="sxs-lookup"><span data-stu-id="cf326-286">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="cf326-287">Ajouter *Models/Contact.cs*:</span><span class="sxs-lookup"><span data-stu-id="cf326-287">Add *Models/Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="cf326-288">Une structure le `Contact` modèle.</span><span class="sxs-lookup"><span data-stu-id="cf326-288">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="cf326-289">La migration initiale de créer et mettre à jour de la base de données :</span><span class="sxs-lookup"><span data-stu-id="cf326-289">Create initial migration and update the database:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* <span data-ttu-id="cf326-290">Mise à jour le **ContactManager** ancrer dans le *pages/_Layout.cshtml* fichier :</span><span class="sxs-lookup"><span data-stu-id="cf326-290">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* <span data-ttu-id="cf326-291">Testez l’application en création, modification et suppression d’un contact</span><span class="sxs-lookup"><span data-stu-id="cf326-291">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="cf326-292">Amorcer la base de données</span><span class="sxs-lookup"><span data-stu-id="cf326-292">Seed the database</span></span>

<span data-ttu-id="cf326-293">Ajouter le [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) classe à la *données* dossier.</span><span class="sxs-lookup"><span data-stu-id="cf326-293">Add the [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="cf326-294">Appelez `SeedData.Initialize` de `Main`:</span><span class="sxs-lookup"><span data-stu-id="cf326-294">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="cf326-295">Tester l’application d’amorçage la base de données.</span><span class="sxs-lookup"><span data-stu-id="cf326-295">Test that the app seeded the database.</span></span> <span data-ttu-id="cf326-296">S’il existe des lignes dans la base de données de contact, la méthode seed ne s’exécute pas.</span><span class="sxs-lookup"><span data-stu-id="cf326-296">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="cf326-297">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="cf326-297">Additional resources</span></span>

* [<span data-ttu-id="cf326-298">Créer une application web .NET Core et SQL Database dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="cf326-298">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* <span data-ttu-id="cf326-299">[ASP.NET Core d’autorisation Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="cf326-299">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="cf326-300">Ce laboratoire présente plus en détails sur les fonctionnalités de sécurité présentées dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="cf326-300">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* <xref:security/authorization/introduction>
* [<span data-ttu-id="cf326-301">Autorisation basée sur une stratégie personnalisée</span><span class="sxs-lookup"><span data-stu-id="cf326-301">Custom policy-based authorization</span></span>](xref:security/authorization/policies)

::: moniker-end
