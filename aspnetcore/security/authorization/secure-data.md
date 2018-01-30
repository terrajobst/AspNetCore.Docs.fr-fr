---
title: "Créer une application ASP.NET Core et de données utilisateur protégées par l’autorisation"
description: "Découvrez comment créer une application de Pages Razor avec les données utilisateur protégées par l’autorisation. Inclut le SSL, l’authentification, la sécurité, ASP.NET Core Identity."
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 01/24/2018
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: security/authorization/secure-data
ms.openlocfilehash: ff5c97feca58318d5f4e5b6a6a930c92469602ba
ms.sourcegitcommit: 18ff1fdaa3e1ae204ed6a2ba9351ce8cf1371c85
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="90c9a-104">Créer une application ASP.NET Core et de données utilisateur protégées par l’autorisation</span><span class="sxs-lookup"><span data-stu-id="90c9a-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="90c9a-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="90c9a-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="90c9a-106">Ce didacticiel montre comment créer une application de web ASP.NET Core et de données utilisateur protégées par l’autorisation.</span><span class="sxs-lookup"><span data-stu-id="90c9a-106">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="90c9a-107">Il affiche une liste de contacts (inscrits) les utilisateurs authentifiés ont créés.</span><span class="sxs-lookup"><span data-stu-id="90c9a-107">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="90c9a-108">Il existe trois groupes de sécurité :</span><span class="sxs-lookup"><span data-stu-id="90c9a-108">There are three security groups:</span></span>

* <span data-ttu-id="90c9a-109">**Utilisateurs inscrits** peuvent afficher toutes les données approuvées et peut modifier/supprimer leurs propres données.</span><span class="sxs-lookup"><span data-stu-id="90c9a-109">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="90c9a-110">**Gestionnaires de** peuvent approuver ou rejeter les données de contact.</span><span class="sxs-lookup"><span data-stu-id="90c9a-110">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="90c9a-111">Seuls les contacts approuvés sont visibles par les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="90c9a-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="90c9a-112">**Les administrateurs** peut approuver/refuser et modifier/supprimer des données.</span><span class="sxs-lookup"><span data-stu-id="90c9a-112">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="90c9a-113">Dans l’image suivante, l’utilisateur Rick (`rick@example.com`) n’est connecté.</span><span class="sxs-lookup"><span data-stu-id="90c9a-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="90c9a-114">Rick peut consulter uniquement les contacts approuvés et **modifier**/**supprimer**/**créer un nouveau** liens pour ses contacts.</span><span class="sxs-lookup"><span data-stu-id="90c9a-114">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="90c9a-115">Seul le dernier enregistrement, créé par Rick, affiche **modifier** et **supprimer** des liens.</span><span class="sxs-lookup"><span data-stu-id="90c9a-115">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="90c9a-116">Les autres utilisateurs ne verront pas le dernier enregistrement jusqu'à ce qu’un gestionnaire ou un administrateur change le statut « Approved ».</span><span class="sxs-lookup"><span data-stu-id="90c9a-116">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![image décrit précédent](secure-data/_static/rick.png)

<span data-ttu-id="90c9a-118">Dans l’image suivante, `manager@contoso.com` est signé dans et dans le rôle gestionnaires :</span><span class="sxs-lookup"><span data-stu-id="90c9a-118">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![image décrit précédent](secure-data/_static/manager1.png)

<span data-ttu-id="90c9a-120">L’illustration suivante montre les gestionnaires de vue des détails d’un contact :</span><span class="sxs-lookup"><span data-stu-id="90c9a-120">The following image shows the managers details view of a contact:</span></span>

![image décrit précédent](secure-data/_static/manager.png)

<span data-ttu-id="90c9a-122">Le **approuver** et **rejeter** boutons sont affichés uniquement pour les responsables et les administrateurs.</span><span class="sxs-lookup"><span data-stu-id="90c9a-122">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="90c9a-123">Dans l’image suivante, `admin@contoso.com` est signé dans et dans le rôle Administrateurs :</span><span class="sxs-lookup"><span data-stu-id="90c9a-123">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![image décrit précédent](secure-data/_static/admin.png)

<span data-ttu-id="90c9a-125">L’administrateur a tous les privilèges.</span><span class="sxs-lookup"><span data-stu-id="90c9a-125">The administrator has all privileges.</span></span> <span data-ttu-id="90c9a-126">Elle peut en lecture/modification/suppression de contact et modifier l’état de contacts.</span><span class="sxs-lookup"><span data-stu-id="90c9a-126">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="90c9a-127">L’application a été créée par [la structure](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) suit `Contact` modèle :</span><span class="sxs-lookup"><span data-stu-id="90c9a-127">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="90c9a-128">L’exemple contient les gestionnaires d’autorisation suivants :</span><span class="sxs-lookup"><span data-stu-id="90c9a-128">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="90c9a-129">`ContactIsOwnerAuthorizationHandler`: Permet de s’assurer qu’un utilisateur peut modifier uniquement leurs données.</span><span class="sxs-lookup"><span data-stu-id="90c9a-129">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="90c9a-130">`ContactManagerAuthorizationHandler`: Permet aux responsables d’approuver ou rejeter des contacts.</span><span class="sxs-lookup"><span data-stu-id="90c9a-130">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="90c9a-131">`ContactAdministratorsAuthorizationHandler`: Permet aux administrateurs pour approuver ou rejeter des contacts et à modifier/supprimer des contacts.</span><span class="sxs-lookup"><span data-stu-id="90c9a-131">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="90c9a-132">Prérequis</span><span class="sxs-lookup"><span data-stu-id="90c9a-132">Prerequisites</span></span>

<span data-ttu-id="90c9a-133">Ce didacticiel est avancé.</span><span class="sxs-lookup"><span data-stu-id="90c9a-133">This tutorial is advanced.</span></span> <span data-ttu-id="90c9a-134">Vous devez être familiarisé avec :</span><span class="sxs-lookup"><span data-stu-id="90c9a-134">You should be familiar with:</span></span>

* [<span data-ttu-id="90c9a-135">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="90c9a-135">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="90c9a-136">Authentification</span><span class="sxs-lookup"><span data-stu-id="90c9a-136">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="90c9a-137">Confirmation de compte et récupération de mot de passe</span><span class="sxs-lookup"><span data-stu-id="90c9a-137">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="90c9a-138">Autorisation</span><span class="sxs-lookup"><span data-stu-id="90c9a-138">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="90c9a-139">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="90c9a-139">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="90c9a-140">La version de ASP.NET Core 1.1 de ce didacticiel est [cela](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) dossier.</span><span class="sxs-lookup"><span data-stu-id="90c9a-140">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="90c9a-141">La version 1.1 de ASP.NET Core exemple se trouve dans le [exemples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="90c9a-141">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="90c9a-142">Le démarrage et l’application terminée</span><span class="sxs-lookup"><span data-stu-id="90c9a-142">The starter and completed app</span></span>

<span data-ttu-id="90c9a-143">[Télécharger](xref:tutorials/index#how-to-download-a-sample) le [terminé](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) application.</span><span class="sxs-lookup"><span data-stu-id="90c9a-143">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="90c9a-144">[Test](#test-the-completed-app) application terminée afin de vous familiariser avec ses fonctionnalités de sécurité.</span><span class="sxs-lookup"><span data-stu-id="90c9a-144">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="90c9a-145">L’application de démarrage</span><span class="sxs-lookup"><span data-stu-id="90c9a-145">The starter app</span></span>

<span data-ttu-id="90c9a-146">[Télécharger](xref:tutorials/index#how-to-download-a-sample) le [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) application.</span><span class="sxs-lookup"><span data-stu-id="90c9a-146">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="90c9a-147">Exécuter l’application, appuyez sur la **ContactManager** la liaison et vérifiez que vous pouvez créer, modifier et supprimer un contact.</span><span class="sxs-lookup"><span data-stu-id="90c9a-147">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="90c9a-148">Sécuriser les données utilisateur</span><span class="sxs-lookup"><span data-stu-id="90c9a-148">Secure user data</span></span>

<span data-ttu-id="90c9a-149">Les sections suivantes ont les principales étapes pour créer l’application de données utilisateur sécurisée.</span><span class="sxs-lookup"><span data-stu-id="90c9a-149">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="90c9a-150">Il peut s’avérer utile de faire référence au projet terminé.</span><span class="sxs-lookup"><span data-stu-id="90c9a-150">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="90c9a-151">Lier les données de contact à l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="90c9a-151">Tie the contact data to the user</span></span>

<span data-ttu-id="90c9a-152">Utilisez ASP.NET [identité](xref:security/authentication/identity) ID d’utilisateur pour vérifier les utilisateurs permettre modifier leurs données, mais pas les autres données utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="90c9a-152">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="90c9a-153">Ajouter `OwnerID` et `ContactStatus` pour la `Contact` modèle :</span><span class="sxs-lookup"><span data-stu-id="90c9a-153">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]

<span data-ttu-id="90c9a-154">`OwnerID`est l’ID de l’utilisateur à partir de la `AspNetUser` de table dans le [identité](xref:security/authentication/identity) base de données.</span><span class="sxs-lookup"><span data-stu-id="90c9a-154">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="90c9a-155">Le `Status` champ détermine si un contact est visible par les utilisateurs standard.</span><span class="sxs-lookup"><span data-stu-id="90c9a-155">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="90c9a-156">Créer une nouvelle migration et mise à jour de la base de données :</span><span class="sxs-lookup"><span data-stu-id="90c9a-156">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-ssl-and-authenticated-users"></a><span data-ttu-id="90c9a-157">Exiger SSL et les utilisateurs authentifiés</span><span class="sxs-lookup"><span data-stu-id="90c9a-157">Require SSL and authenticated users</span></span>

<span data-ttu-id="90c9a-158">Ajouter [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) à `Startup`:</span><span class="sxs-lookup"><span data-stu-id="90c9a-158">Add [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) to `Startup`:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_env)]

<span data-ttu-id="90c9a-159">Dans le `ConfigureServices` méthode de la *Startup.cs* , ajoutez le [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) filtre d’autorisation :</span><span class="sxs-lookup"><span data-stu-id="90c9a-159">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=19-)]

<span data-ttu-id="90c9a-160">Si vous utilisez Visual Studio, activez SSL.</span><span class="sxs-lookup"><span data-stu-id="90c9a-160">If you're using Visual Studio, enable SSL.</span></span>

<span data-ttu-id="90c9a-161">Pour rediriger les demandes HTTP vers HTTPS, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="90c9a-161">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="90c9a-162">Si vous êtes à l’aide de Visual Studio Code ou de test sur une plateforme locale qui n’inclut pas un certificat de test pour le protocole SSL :</span><span class="sxs-lookup"><span data-stu-id="90c9a-162">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for SSL:</span></span>

  <span data-ttu-id="90c9a-163">Définissez `"LocalTest:skipSSL": true` dans le *appsettings. Developement.JSON* fichier.</span><span class="sxs-lookup"><span data-stu-id="90c9a-163">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="90c9a-164">Demander aux utilisateurs authentifiés</span><span class="sxs-lookup"><span data-stu-id="90c9a-164">Require authenticated users</span></span>

<span data-ttu-id="90c9a-165">Définir la stratégie d’authentification par défaut pour les utilisateurs doivent être authentifiés.</span><span class="sxs-lookup"><span data-stu-id="90c9a-165">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="90c9a-166">Vous pouvez désactiver l’authentification au niveau de la méthode Page Razor, de contrôleur ou d’action avec le `[AllowAnonymous]` attribut.</span><span class="sxs-lookup"><span data-stu-id="90c9a-166">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="90c9a-167">Définition de la stratégie d’authentification par défaut pour les utilisateurs doivent être authentifiés protège nouvellement ajouté les Pages Razor et tous les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="90c9a-167">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="90c9a-168">Avec l’authentification requise par défaut est plus sûre que de s’appuyer sur nouveaux contrôleurs et les Pages Razor à inclure le `[Authorize]` attribut.</span><span class="sxs-lookup"><span data-stu-id="90c9a-168">Having authentication required by default is safer than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span> <span data-ttu-id="90c9a-169">Ajoutez le code suivant à la `ConfigureServices` méthode de la *Startup.cs* fichier :</span><span class="sxs-lookup"><span data-stu-id="90c9a-169">Add the following to the `ConfigureServices` method of the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=31-)]

<span data-ttu-id="90c9a-170">Ajouter [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) à l’Index, des pages sur et de Contact pour les utilisateurs anonymes peuvent obtenir des informations sur le site avant qu’ils s’inscrire.</span><span class="sxs-lookup"><span data-stu-id="90c9a-170">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span> 

[!code-csharp[Main](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

<span data-ttu-id="90c9a-171">Ajouter `[AllowAnonymous]` à la [LoginModel et RegisterModel](https://github.com/aspnet/templating/issues/238).</span><span class="sxs-lookup"><span data-stu-id="90c9a-171">Add `[AllowAnonymous]` to the [LoginModel and RegisterModel](https://github.com/aspnet/templating/issues/238).</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="90c9a-172">Configurer le compte de test</span><span class="sxs-lookup"><span data-stu-id="90c9a-172">Configure the test account</span></span>

<span data-ttu-id="90c9a-173">La `SeedData` classe crée deux comptes : administrateur et le gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="90c9a-173">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="90c9a-174">Utilisez le [Secret gestionnaire](xref:security/app-secrets) pour définir un mot de passe pour ces comptes.</span><span class="sxs-lookup"><span data-stu-id="90c9a-174">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="90c9a-175">Définir le mot de passe à partir du répertoire de projet (le répertoire contenant *Program.cs*) :</span><span class="sxs-lookup"><span data-stu-id="90c9a-175">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="90c9a-176">Mise à jour `Main` à utiliser le mot de passe de test :</span><span class="sxs-lookup"><span data-stu-id="90c9a-176">Update `Main` to use the test password:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="90c9a-177">Créez les comptes de test et de mettre à jour les contacts</span><span class="sxs-lookup"><span data-stu-id="90c9a-177">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="90c9a-178">Mise à jour la `Initialize` méthode dans la `SeedData` classe pour créer les comptes de test :</span><span class="sxs-lookup"><span data-stu-id="90c9a-178">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="90c9a-179">Ajouter l’ID d’utilisateur administrateur et `ContactStatus` aux contacts.</span><span class="sxs-lookup"><span data-stu-id="90c9a-179">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="90c9a-180">Apportez l’une des contacts « Envoyé » et un « Rejected ».</span><span class="sxs-lookup"><span data-stu-id="90c9a-180">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="90c9a-181">Ajouter l’ID d’utilisateur et l’état de tous les contacts.</span><span class="sxs-lookup"><span data-stu-id="90c9a-181">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="90c9a-182">Un seul contact s’affiche :</span><span class="sxs-lookup"><span data-stu-id="90c9a-182">Only one contact is shown:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="90c9a-183">Créer le propriétaire et Gestionnaire de gestionnaires d’autorisations d’administrateur</span><span class="sxs-lookup"><span data-stu-id="90c9a-183">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="90c9a-184">Créer un `ContactIsOwnerAuthorizationHandler` classe dans le *autorisation* dossier.</span><span class="sxs-lookup"><span data-stu-id="90c9a-184">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="90c9a-185">Le `ContactIsOwnerAuthorizationHandler` vérifie que l’utilisateur qui agit sur une ressource de propriétaire de la ressource.</span><span class="sxs-lookup"><span data-stu-id="90c9a-185">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="90c9a-186">Le `ContactIsOwnerAuthorizationHandler` appelle [contexte. Réussir](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) si l’utilisateur authentifié actuel est le propriétaire du contact.</span><span class="sxs-lookup"><span data-stu-id="90c9a-186">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="90c9a-187">Les gestionnaires d’autorisation généralement :</span><span class="sxs-lookup"><span data-stu-id="90c9a-187">Authorization handlers generally:</span></span>

* <span data-ttu-id="90c9a-188">Retourner `context.Succeed` lorsque les conditions sont remplies.</span><span class="sxs-lookup"><span data-stu-id="90c9a-188">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="90c9a-189">Retourner `Task.CompletedTask` lorsque les conditions ne sont pas remplies.</span><span class="sxs-lookup"><span data-stu-id="90c9a-189">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="90c9a-190">`Task.CompletedTask`est une réussite ou l’échec&mdash;permet d’autres gestionnaires d’autorisation Exécuter.</span><span class="sxs-lookup"><span data-stu-id="90c9a-190">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="90c9a-191">Si vous devez explicitement échouer, retourner [contexte. Échec de](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="90c9a-191">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="90c9a-192">L’application permet les propriétaires de contact à modifier, supprimer ou créer leurs propres données.</span><span class="sxs-lookup"><span data-stu-id="90c9a-192">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="90c9a-193">`ContactIsOwnerAuthorizationHandler`n’a pas besoin vérifier l’opération passée dans le paramètre de condition.</span><span class="sxs-lookup"><span data-stu-id="90c9a-193">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="90c9a-194">Créer un gestionnaire d’autorisation de gestionnaire</span><span class="sxs-lookup"><span data-stu-id="90c9a-194">Create a manager authorization handler</span></span>

<span data-ttu-id="90c9a-195">Créer un `ContactManagerAuthorizationHandler` classe dans le *autorisation* dossier.</span><span class="sxs-lookup"><span data-stu-id="90c9a-195">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="90c9a-196">Le `ContactManagerAuthorizationHandler` vérifie l’utilisateur qui agit sur la ressource est un gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="90c9a-196">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="90c9a-197">Seuls les responsables peuvent approuver ou rejeter les modifications de contenu (nouvelles ou modifiées).</span><span class="sxs-lookup"><span data-stu-id="90c9a-197">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="90c9a-198">Créer un gestionnaire d’autorisations d’administrateur</span><span class="sxs-lookup"><span data-stu-id="90c9a-198">Create an administrator authorization handler</span></span>

<span data-ttu-id="90c9a-199">Créer un `ContactAdministratorsAuthorizationHandler` classe dans le *autorisation* dossier.</span><span class="sxs-lookup"><span data-stu-id="90c9a-199">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="90c9a-200">Le `ContactAdministratorsAuthorizationHandler` vérifie l’utilisateur qui agit sur la ressource est un administrateur.</span><span class="sxs-lookup"><span data-stu-id="90c9a-200">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="90c9a-201">Administrateur peut effectuer toutes les opérations.</span><span class="sxs-lookup"><span data-stu-id="90c9a-201">Administrator can do all operations.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="90c9a-202">Inscrire les gestionnaires d’autorisation</span><span class="sxs-lookup"><span data-stu-id="90c9a-202">Register the authorization handlers</span></span>

<span data-ttu-id="90c9a-203">Services à l’aide d’Entity Framework Core doivent être inscrit pour [injection de dépendance](xref:fundamentals/dependency-injection) à l’aide de [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="90c9a-203">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="90c9a-204">Le `ContactIsOwnerAuthorizationHandler` utilise ASP.NET Core [identité](xref:security/authentication/identity), qui est basé sur Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="90c9a-204">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="90c9a-205">Enregistrez les gestionnaires avec la collection de service afin qu’elles soient disponibles pour le `ContactsController` via [injection de dépendance](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="90c9a-205">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="90c9a-206">Ajoutez le code suivant à la fin de `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="90c9a-206">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-)]

<span data-ttu-id="90c9a-207">`ContactAdministratorsAuthorizationHandler`et `ContactManagerAuthorizationHandler` sont ajoutés en tant que singletons.</span><span class="sxs-lookup"><span data-stu-id="90c9a-207">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="90c9a-208">Ils sont singletons, car ils n’utilisent pas EF et toutes les informations nécessitées sont dans le `Context` paramètre de la `HandleRequirementAsync` (méthode).</span><span class="sxs-lookup"><span data-stu-id="90c9a-208">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="90c9a-209">Autorisation de la prise en charge</span><span class="sxs-lookup"><span data-stu-id="90c9a-209">Support authorization</span></span>

<span data-ttu-id="90c9a-210">Dans cette section, vous mettez à jour les Pages Razor et ajoutez une classe de configuration requise des opérations.</span><span class="sxs-lookup"><span data-stu-id="90c9a-210">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="90c9a-211">Passez en revue la classe de la configuration requise operations contact</span><span class="sxs-lookup"><span data-stu-id="90c9a-211">Review the contact operations requirements class</span></span>

<span data-ttu-id="90c9a-212">Examinez la `ContactOperations` classe.</span><span class="sxs-lookup"><span data-stu-id="90c9a-212">Review the `ContactOperations` class.</span></span> <span data-ttu-id="90c9a-213">Cette classe contient les spécifications le prend en charge de l’application :</span><span class="sxs-lookup"><span data-stu-id="90c9a-213">This class contains the requirements the app supports:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a><span data-ttu-id="90c9a-214">Créer une classe de base pour les Pages Razor</span><span class="sxs-lookup"><span data-stu-id="90c9a-214">Create a base class for the Razor Pages</span></span>

<span data-ttu-id="90c9a-215">Créer une classe de base qui contient les services utilisés dans les Pages Razor contacts.</span><span class="sxs-lookup"><span data-stu-id="90c9a-215">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="90c9a-216">La classe de base place ce code d’initialisation dans un emplacement :</span><span class="sxs-lookup"><span data-stu-id="90c9a-216">The base class puts that initialization code in one location:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="90c9a-217">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="90c9a-217">The preceding code:</span></span>

* <span data-ttu-id="90c9a-218">Ajoute la `IAuthorizationService` service pour accéder aux gestionnaires d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="90c9a-218">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="90c9a-219">Ajoute l’identité `UserManager` service.</span><span class="sxs-lookup"><span data-stu-id="90c9a-219">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="90c9a-220">Ajoutez la `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="90c9a-220">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="90c9a-221">Mettre à jour le CreateModel</span><span class="sxs-lookup"><span data-stu-id="90c9a-221">Update the CreateModel</span></span>

<span data-ttu-id="90c9a-222">Mettre à jour le constructeur de modèle de page de création à utiliser le `DI_BasePageModel` classe de base :</span><span class="sxs-lookup"><span data-stu-id="90c9a-222">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="90c9a-223">Mise à jour le `CreateModel.OnPostAsync` méthode :</span><span class="sxs-lookup"><span data-stu-id="90c9a-223">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="90c9a-224">Ajouter l’ID utilisateur à la `Contact` modèle.</span><span class="sxs-lookup"><span data-stu-id="90c9a-224">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="90c9a-225">Appelle le Gestionnaire d’autorisation pour vérifier que l’utilisateur est autorisé à créer des contacts.</span><span class="sxs-lookup"><span data-stu-id="90c9a-225">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="90c9a-226">Mettre à jour le IndexModel</span><span class="sxs-lookup"><span data-stu-id="90c9a-226">Update the IndexModel</span></span>

<span data-ttu-id="90c9a-227">Mise à jour le `OnGetAsync` méthode approuvées uniquement les contacts sont présentés aux utilisateurs généraux :</span><span class="sxs-lookup"><span data-stu-id="90c9a-227">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="90c9a-228">Mettre à jour le EditModel</span><span class="sxs-lookup"><span data-stu-id="90c9a-228">Update the EditModel</span></span>

<span data-ttu-id="90c9a-229">Ajoutez un gestionnaire d’autorisation pour vérifier que l’utilisateur propriétaire du contact.</span><span class="sxs-lookup"><span data-stu-id="90c9a-229">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="90c9a-230">Étant donné que l’autorisation de ressource est en cours de validation, le `[Authorize]` attribut n’est pas suffisant.</span><span class="sxs-lookup"><span data-stu-id="90c9a-230">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="90c9a-231">L’application n’a pas accès à la ressource lorsque des attributs sont évaluées.</span><span class="sxs-lookup"><span data-stu-id="90c9a-231">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="90c9a-232">Autorisation basée sur les ressources doit être impérative.</span><span class="sxs-lookup"><span data-stu-id="90c9a-232">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="90c9a-233">Vérifications doivent être effectuées une fois que l’application a accès à la ressource, en le chargeant dans le modèle de page ou en le chargeant dans le Gestionnaire d’elle-même.</span><span class="sxs-lookup"><span data-stu-id="90c9a-233">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="90c9a-234">Vous accédez fréquemment à la ressource en passant la clé de ressource.</span><span class="sxs-lookup"><span data-stu-id="90c9a-234">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="90c9a-235">Mettre à jour le DeleteModel</span><span class="sxs-lookup"><span data-stu-id="90c9a-235">Update the DeleteModel</span></span>

<span data-ttu-id="90c9a-236">Mettre à jour le modèle de page de suppression pour utiliser le Gestionnaire d’autorisation pour vérifier que l’utilisateur a l’autorisation de suppression sur le contact.</span><span class="sxs-lookup"><span data-stu-id="90c9a-236">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="90c9a-237">Le service d’autorisation d’injecter dans les vues</span><span class="sxs-lookup"><span data-stu-id="90c9a-237">Inject the authorization service into the views</span></span>

<span data-ttu-id="90c9a-238">Actuellement, le montre l’interface utilisateur modifie et supprimer les liens pour l’utilisateur ne peut pas modifier les données.</span><span class="sxs-lookup"><span data-stu-id="90c9a-238">Currently, the UI shows edit and delete links for data the user can't modify.</span></span> <span data-ttu-id="90c9a-239">L’interface utilisateur est résolu en appliquant le Gestionnaire d’autorisation pour les vues.</span><span class="sxs-lookup"><span data-stu-id="90c9a-239">The UI is fixed by applying the authorization handler to the views.</span></span>

<span data-ttu-id="90c9a-240">Injecter le service d’autorisation dans le *Views/_ViewImports.cshtml* afin qu’il soit disponible pour toutes les vues de fichiers :</span><span class="sxs-lookup"><span data-stu-id="90c9a-240">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

<span data-ttu-id="90c9a-241">Le balisage précédent ajoute plusieurs `using` instructions.</span><span class="sxs-lookup"><span data-stu-id="90c9a-241">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="90c9a-242">Mise à jour la **modifier** et **supprimer** des liaisons dans *Pages/Contacts/Index.cshtml* afin qu’ils sont rendus uniquement pour les utilisateurs disposant des autorisations appropriées :</span><span class="sxs-lookup"><span data-stu-id="90c9a-242">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-)]

> [!WARNING]
> <span data-ttu-id="90c9a-243">Le masquage des liens à partir des utilisateurs qui ne sont pas autorisés à modifier les données ne sécuriser l’application.</span><span class="sxs-lookup"><span data-stu-id="90c9a-243">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="90c9a-244">Le masquage des liens rend l’application plus conviviale en affichant des liens n’est valides.</span><span class="sxs-lookup"><span data-stu-id="90c9a-244">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="90c9a-245">Les utilisateurs peuvent hack les URL générées pour appeler modifier et supprimer des opérations sur les données qu’ils ne possèdent pas.</span><span class="sxs-lookup"><span data-stu-id="90c9a-245">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="90c9a-246">Le contrôleur ou Page Razor doit appliquer les vérifications d’accès pour sécuriser les données.</span><span class="sxs-lookup"><span data-stu-id="90c9a-246">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="90c9a-247">Détails de la mise à jour</span><span class="sxs-lookup"><span data-stu-id="90c9a-247">Update Details</span></span>

<span data-ttu-id="90c9a-248">Mettre à jour l’affichage des détails pour les gestionnaires peuvent approuver ou rejeter des contacts :</span><span class="sxs-lookup"><span data-stu-id="90c9a-248">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-)]

<span data-ttu-id="90c9a-249">Mettre à jour le modèle de page de détails :</span><span class="sxs-lookup"><span data-stu-id="90c9a-249">Update the details page model:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="90c9a-250">Tester l’application terminée</span><span class="sxs-lookup"><span data-stu-id="90c9a-250">Test the completed app</span></span>

<span data-ttu-id="90c9a-251">Si vous êtes à l’aide de Visual Studio Code ou de test sur une plateforme locale qui n’inclut pas un certificat de test pour le protocole SSL :</span><span class="sxs-lookup"><span data-stu-id="90c9a-251">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for SSL:</span></span>

* <span data-ttu-id="90c9a-252">Définissez `"LocalTest:skipSSL": true` dans le *appsettings. Developement.JSON* fichiers à ignorer la spécification SSL.</span><span class="sxs-lookup"><span data-stu-id="90c9a-252">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file to skip the SSL requirement.</span></span> <span data-ttu-id="90c9a-253">Ignorer le protocole SSL uniquement sur un ordinateur de développement.</span><span class="sxs-lookup"><span data-stu-id="90c9a-253">Skip SSL only on a development machine.</span></span>

<span data-ttu-id="90c9a-254">Si l’application contient des contacts :</span><span class="sxs-lookup"><span data-stu-id="90c9a-254">If the app has contacts:</span></span>

* <span data-ttu-id="90c9a-255">Supprimer tous les enregistrements dans la `Contact` table.</span><span class="sxs-lookup"><span data-stu-id="90c9a-255">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="90c9a-256">Redémarrez l’application pour amorcer la base de données.</span><span class="sxs-lookup"><span data-stu-id="90c9a-256">Restart the app to seed the database.</span></span>

<span data-ttu-id="90c9a-257">Inscrire un utilisateur pour parcourir les contacts.</span><span class="sxs-lookup"><span data-stu-id="90c9a-257">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="90c9a-258">Un moyen simple pour tester l’application terminée consiste à lancer des trois différents navigateurs (ou les versions furtif/navigation InPrivate).</span><span class="sxs-lookup"><span data-stu-id="90c9a-258">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="90c9a-259">Dans un navigateur, inscrire un nouvel utilisateur (par exemple, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="90c9a-259">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="90c9a-260">Connectez-vous à chaque navigateur avec un autre utilisateur.</span><span class="sxs-lookup"><span data-stu-id="90c9a-260">Sign in to each browser with a different user.</span></span> <span data-ttu-id="90c9a-261">Vérifiez les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="90c9a-261">Verify the following operations:</span></span>

* <span data-ttu-id="90c9a-262">Les utilisateurs inscrits peuvent afficher toutes les données de contact approuvées.</span><span class="sxs-lookup"><span data-stu-id="90c9a-262">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="90c9a-263">Les utilisateurs inscrits peuvent modifier/supprimer leurs propres données.</span><span class="sxs-lookup"><span data-stu-id="90c9a-263">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="90c9a-264">Gestionnaires peuvent approuver ou rejeter les données de contact.</span><span class="sxs-lookup"><span data-stu-id="90c9a-264">Managers can approve or reject contact data.</span></span> <span data-ttu-id="90c9a-265">Le `Details` vue présente **approuver** et **rejeter** boutons.</span><span class="sxs-lookup"><span data-stu-id="90c9a-265">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="90c9a-266">Les administrateurs peuvent approuver/refuser et modifier/supprimer des données.</span><span class="sxs-lookup"><span data-stu-id="90c9a-266">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="90c9a-267">Utilisateur</span><span class="sxs-lookup"><span data-stu-id="90c9a-267">User</span></span>| <span data-ttu-id="90c9a-268">Options</span><span class="sxs-lookup"><span data-stu-id="90c9a-268">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="90c9a-269">Peut modifier/supprimer des données propres</span><span class="sxs-lookup"><span data-stu-id="90c9a-269">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="90c9a-270">Peuvent approuver/refuser et modification/suppression posséder de données</span><span class="sxs-lookup"><span data-stu-id="90c9a-270">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="90c9a-271">Peut modifier/supprimer et approuver/refuser toutes les données</span><span class="sxs-lookup"><span data-stu-id="90c9a-271">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="90c9a-272">Créer un contact dans le navigateur de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="90c9a-272">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="90c9a-273">Copier l’URL pour le supprimer et modifier du contact de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="90c9a-273">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="90c9a-274">Collez ces liens dans le navigateur de l’utilisateur de test pour vérifier que l’utilisateur de test ne peut pas effectuer ces opérations.</span><span class="sxs-lookup"><span data-stu-id="90c9a-274">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="90c9a-275">Créer l’application de démarrage</span><span class="sxs-lookup"><span data-stu-id="90c9a-275">Create the starter app</span></span>

* <span data-ttu-id="90c9a-276">Créer une application de Pages Razor nommée « ContactManager »</span><span class="sxs-lookup"><span data-stu-id="90c9a-276">Create a Razor Pages app named "ContactManager"</span></span>

  * <span data-ttu-id="90c9a-277">Créer l’application avec **comptes d’utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="90c9a-277">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="90c9a-278">Pour votre espace de noms correspond à l’espace de noms utilisé dans l’exemple, nommez-Le « ContactManager ».</span><span class="sxs-lookup"><span data-stu-id="90c9a-278">Name it "ContactManager" so your namespace matches the namespace used in the sample.</span></span>

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

  * <span data-ttu-id="90c9a-279">`-uld`Spécifie la base de données locale au lieu de SQLite</span><span class="sxs-lookup"><span data-stu-id="90c9a-279">`-uld` specifies LocalDB instead of SQLite</span></span>

* <span data-ttu-id="90c9a-280">Ajoutez le code suivant `Contact` modèle :</span><span class="sxs-lookup"><span data-stu-id="90c9a-280">Add the following `Contact` model:</span></span>

  [!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="90c9a-281">Structure du `Contact` modèle :</span><span class="sxs-lookup"><span data-stu-id="90c9a-281">Scaffold the `Contact` model:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* <span data-ttu-id="90c9a-282">Mise à jour la **ContactManager** d’ancrage dans le *Pages/_Layout.cshtml* fichier :</span><span class="sxs-lookup"><span data-stu-id="90c9a-282">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="90c9a-283">Structurez la migration initiale et mettre à jour de la base de données :</span><span class="sxs-lookup"><span data-stu-id="90c9a-283">Scaffold the initial migration and update the database:</span></span>

```console
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="90c9a-284">Tester l’application par la création, modification et suppression d’un contact</span><span class="sxs-lookup"><span data-stu-id="90c9a-284">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="90c9a-285">Amorcer la base de données</span><span class="sxs-lookup"><span data-stu-id="90c9a-285">Seed the database</span></span>

<span data-ttu-id="90c9a-286">Ajouter le `SeedData` classe le *données* dossier.</span><span class="sxs-lookup"><span data-stu-id="90c9a-286">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="90c9a-287">Si vous avez téléchargé l’exemple, vous pouvez copier le *SeedData.cs* de fichiers à la *données* dossier du projet de démarrage.</span><span class="sxs-lookup"><span data-stu-id="90c9a-287">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="90c9a-288">Appelez `SeedData.Initialize` de `Main`:</span><span class="sxs-lookup"><span data-stu-id="90c9a-288">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[Main](secure-data/samples/starter2/Program.cs?name=snippet)]

<span data-ttu-id="90c9a-289">Test de l’application d’amorçage la base de données.</span><span class="sxs-lookup"><span data-stu-id="90c9a-289">Test that the app seeded the database.</span></span> <span data-ttu-id="90c9a-290">S’il existe des lignes dans la base de données de contact, la méthode de valeur initiale ne s’exécute pas.</span><span class="sxs-lookup"><span data-stu-id="90c9a-290">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="90c9a-291">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="90c9a-291">Additional resources</span></span>

* <span data-ttu-id="90c9a-292">[ASP.NET Core d’autorisation Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="90c9a-292">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="90c9a-293">Ce laboratoire présente en détail sur les fonctionnalités de sécurité présentées dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="90c9a-293">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="90c9a-294">Autorisation dans ASP.NET Core : Simple, des rôles, basée sur les revendications et personnalisées</span><span class="sxs-lookup"><span data-stu-id="90c9a-294">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="90c9a-295">Autorisation basée sur une stratégie personnalisée</span><span class="sxs-lookup"><span data-stu-id="90c9a-295">Custom policy-based authorization</span></span>](xref:security/authorization/policies)