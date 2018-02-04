---
title: "Créer une application ASP.NET Core et de données utilisateur protégées par l’autorisation"
author: rick-anderson
description: "Découvrez comment créer une application de Pages Razor avec les données utilisateur protégées par l’autorisation. Inclut le SSL, l’authentification, la sécurité, ASP.NET Core Identity."
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/secure-data
ms.openlocfilehash: 6333082a2b2b4f6d3f1ce2afc600b4203a0f5dca
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/03/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="df7e0-104">Créer une application ASP.NET Core et de données utilisateur protégées par l’autorisation</span><span class="sxs-lookup"><span data-stu-id="df7e0-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="df7e0-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="df7e0-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="df7e0-106">Ce didacticiel montre comment créer une application de web ASP.NET Core et de données utilisateur protégées par l’autorisation.</span><span class="sxs-lookup"><span data-stu-id="df7e0-106">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="df7e0-107">Il affiche une liste de contacts (inscrits) les utilisateurs authentifiés ont créés.</span><span class="sxs-lookup"><span data-stu-id="df7e0-107">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="df7e0-108">Il existe trois groupes de sécurité :</span><span class="sxs-lookup"><span data-stu-id="df7e0-108">There are three security groups:</span></span>

* <span data-ttu-id="df7e0-109">**Utilisateurs inscrits** peuvent afficher toutes les données approuvées et peut modifier/supprimer leurs propres données.</span><span class="sxs-lookup"><span data-stu-id="df7e0-109">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="df7e0-110">**Gestionnaires de** peuvent approuver ou rejeter les données de contact.</span><span class="sxs-lookup"><span data-stu-id="df7e0-110">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="df7e0-111">Seuls les contacts approuvés sont visibles par les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="df7e0-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="df7e0-112">**Les administrateurs** peut approuver/refuser et modifier/supprimer des données.</span><span class="sxs-lookup"><span data-stu-id="df7e0-112">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="df7e0-113">Dans l’image suivante, l’utilisateur Rick (`rick@example.com`) n’est connecté.</span><span class="sxs-lookup"><span data-stu-id="df7e0-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="df7e0-114">Rick peut consulter uniquement les contacts approuvés et **modifier**/**supprimer**/**créer un nouveau** liens pour ses contacts.</span><span class="sxs-lookup"><span data-stu-id="df7e0-114">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="df7e0-115">Seul le dernier enregistrement, créé par Rick, affiche **modifier** et **supprimer** des liens.</span><span class="sxs-lookup"><span data-stu-id="df7e0-115">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="df7e0-116">Les autres utilisateurs ne verront pas le dernier enregistrement jusqu'à ce qu’un gestionnaire ou un administrateur change le statut « Approved ».</span><span class="sxs-lookup"><span data-stu-id="df7e0-116">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![image décrit précédent](secure-data/_static/rick.png)

<span data-ttu-id="df7e0-118">Dans l’image suivante, `manager@contoso.com` est signé dans et dans le rôle gestionnaires :</span><span class="sxs-lookup"><span data-stu-id="df7e0-118">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![image décrit précédent](secure-data/_static/manager1.png)

<span data-ttu-id="df7e0-120">L’illustration suivante montre les gestionnaires de vue des détails d’un contact :</span><span class="sxs-lookup"><span data-stu-id="df7e0-120">The following image shows the managers details view of a contact:</span></span>

![image décrit précédent](secure-data/_static/manager.png)

<span data-ttu-id="df7e0-122">Le **approuver** et **rejeter** boutons sont affichés uniquement pour les responsables et les administrateurs.</span><span class="sxs-lookup"><span data-stu-id="df7e0-122">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="df7e0-123">Dans l’image suivante, `admin@contoso.com` est signé dans et dans le rôle Administrateurs :</span><span class="sxs-lookup"><span data-stu-id="df7e0-123">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![image décrit précédent](secure-data/_static/admin.png)

<span data-ttu-id="df7e0-125">L’administrateur a tous les privilèges.</span><span class="sxs-lookup"><span data-stu-id="df7e0-125">The administrator has all privileges.</span></span> <span data-ttu-id="df7e0-126">Elle peut en lecture/modification/suppression de contact et modifier l’état de contacts.</span><span class="sxs-lookup"><span data-stu-id="df7e0-126">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="df7e0-127">L’application a été créée par [la structure](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) suit `Contact` modèle :</span><span class="sxs-lookup"><span data-stu-id="df7e0-127">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="df7e0-128">L’exemple contient les gestionnaires d’autorisation suivants :</span><span class="sxs-lookup"><span data-stu-id="df7e0-128">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="df7e0-129">`ContactIsOwnerAuthorizationHandler`: Permet de s’assurer qu’un utilisateur peut modifier uniquement leurs données.</span><span class="sxs-lookup"><span data-stu-id="df7e0-129">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="df7e0-130">`ContactManagerAuthorizationHandler`: Permet aux responsables d’approuver ou rejeter des contacts.</span><span class="sxs-lookup"><span data-stu-id="df7e0-130">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="df7e0-131">`ContactAdministratorsAuthorizationHandler`: Permet aux administrateurs pour approuver ou rejeter des contacts et à modifier/supprimer des contacts.</span><span class="sxs-lookup"><span data-stu-id="df7e0-131">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="df7e0-132">Prérequis</span><span class="sxs-lookup"><span data-stu-id="df7e0-132">Prerequisites</span></span>

<span data-ttu-id="df7e0-133">Ce didacticiel est avancé.</span><span class="sxs-lookup"><span data-stu-id="df7e0-133">This tutorial is advanced.</span></span> <span data-ttu-id="df7e0-134">Vous devez être familiarisé avec :</span><span class="sxs-lookup"><span data-stu-id="df7e0-134">You should be familiar with:</span></span>

* [<span data-ttu-id="df7e0-135">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="df7e0-135">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="df7e0-136">Authentification</span><span class="sxs-lookup"><span data-stu-id="df7e0-136">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="df7e0-137">Confirmation de compte et récupération de mot de passe</span><span class="sxs-lookup"><span data-stu-id="df7e0-137">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="df7e0-138">Autorisation</span><span class="sxs-lookup"><span data-stu-id="df7e0-138">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="df7e0-139">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="df7e0-139">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="df7e0-140">Consultez [ce fichier PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) pour la version d’ASP.NET MVC de base.</span><span class="sxs-lookup"><span data-stu-id="df7e0-140">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="df7e0-141">La version de ASP.NET Core 1.1 de ce didacticiel est [cela](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) dossier.</span><span class="sxs-lookup"><span data-stu-id="df7e0-141">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="df7e0-142">La version 1.1 de ASP.NET Core exemple se trouve dans le [exemples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="df7e0-142">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="df7e0-143">Le démarrage et l’application terminée</span><span class="sxs-lookup"><span data-stu-id="df7e0-143">The starter and completed app</span></span>

<span data-ttu-id="df7e0-144">[Télécharger](xref:tutorials/index#how-to-download-a-sample) le [terminé](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) application.</span><span class="sxs-lookup"><span data-stu-id="df7e0-144">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="df7e0-145">[Test](#test-the-completed-app) application terminée afin de vous familiariser avec ses fonctionnalités de sécurité.</span><span class="sxs-lookup"><span data-stu-id="df7e0-145">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="df7e0-146">L’application de démarrage</span><span class="sxs-lookup"><span data-stu-id="df7e0-146">The starter app</span></span>

<span data-ttu-id="df7e0-147">[Télécharger](xref:tutorials/index#how-to-download-a-sample) le [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) application.</span><span class="sxs-lookup"><span data-stu-id="df7e0-147">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="df7e0-148">Exécuter l’application, appuyez sur la **ContactManager** la liaison et vérifiez que vous pouvez créer, modifier et supprimer un contact.</span><span class="sxs-lookup"><span data-stu-id="df7e0-148">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="df7e0-149">Sécuriser les données utilisateur</span><span class="sxs-lookup"><span data-stu-id="df7e0-149">Secure user data</span></span>

<span data-ttu-id="df7e0-150">Les sections suivantes ont les principales étapes pour créer l’application de données utilisateur sécurisée.</span><span class="sxs-lookup"><span data-stu-id="df7e0-150">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="df7e0-151">Il peut s’avérer utile de faire référence au projet terminé.</span><span class="sxs-lookup"><span data-stu-id="df7e0-151">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="df7e0-152">Lier les données de contact à l’utilisateur</span><span class="sxs-lookup"><span data-stu-id="df7e0-152">Tie the contact data to the user</span></span>

<span data-ttu-id="df7e0-153">Utilisez ASP.NET [identité](xref:security/authentication/identity) ID d’utilisateur pour vérifier les utilisateurs permettre modifier leurs données, mais pas les autres données utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="df7e0-153">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="df7e0-154">Ajouter `OwnerID` et `ContactStatus` pour la `Contact` modèle :</span><span class="sxs-lookup"><span data-stu-id="df7e0-154">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="df7e0-155">`OwnerID`est l’ID de l’utilisateur à partir de la `AspNetUser` de table dans le [identité](xref:security/authentication/identity) base de données.</span><span class="sxs-lookup"><span data-stu-id="df7e0-155">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="df7e0-156">Le `Status` champ détermine si un contact est visible par les utilisateurs standard.</span><span class="sxs-lookup"><span data-stu-id="df7e0-156">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="df7e0-157">Créer une nouvelle migration et mise à jour de la base de données :</span><span class="sxs-lookup"><span data-stu-id="df7e0-157">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-ssl-and-authenticated-users"></a><span data-ttu-id="df7e0-158">Exiger SSL et les utilisateurs authentifiés</span><span class="sxs-lookup"><span data-stu-id="df7e0-158">Require SSL and authenticated users</span></span>

<span data-ttu-id="df7e0-159">Ajouter [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) à `Startup`:</span><span class="sxs-lookup"><span data-stu-id="df7e0-159">Add [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) to `Startup`:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_env)]

<span data-ttu-id="df7e0-160">Dans le `ConfigureServices` méthode de la *Startup.cs* , ajoutez le [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) filtre d’autorisation :</span><span class="sxs-lookup"><span data-stu-id="df7e0-160">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=19-999)]

<span data-ttu-id="df7e0-161">Si vous utilisez Visual Studio, activez SSL.</span><span class="sxs-lookup"><span data-stu-id="df7e0-161">If you're using Visual Studio, enable SSL.</span></span>

<span data-ttu-id="df7e0-162">Pour rediriger les demandes HTTP vers HTTPS, consultez [intergiciel (middleware) réécriture d’URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="df7e0-162">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="df7e0-163">Si vous êtes à l’aide de Visual Studio Code ou de test sur une plateforme locale qui n’inclut pas un certificat de test pour le protocole SSL :</span><span class="sxs-lookup"><span data-stu-id="df7e0-163">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for SSL:</span></span>

  <span data-ttu-id="df7e0-164">Définissez `"LocalTest:skipSSL": true` dans le *appsettings. Developement.JSON* fichier.</span><span class="sxs-lookup"><span data-stu-id="df7e0-164">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="df7e0-165">Demander aux utilisateurs authentifiés</span><span class="sxs-lookup"><span data-stu-id="df7e0-165">Require authenticated users</span></span>

<span data-ttu-id="df7e0-166">Définir la stratégie d’authentification par défaut pour les utilisateurs doivent être authentifiés.</span><span class="sxs-lookup"><span data-stu-id="df7e0-166">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="df7e0-167">Vous pouvez désactiver l’authentification au niveau de la méthode Page Razor, de contrôleur ou d’action avec le `[AllowAnonymous]` attribut.</span><span class="sxs-lookup"><span data-stu-id="df7e0-167">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="df7e0-168">Définition de la stratégie d’authentification par défaut pour les utilisateurs doivent être authentifiés protège nouvellement ajouté les Pages Razor et tous les contrôleurs.</span><span class="sxs-lookup"><span data-stu-id="df7e0-168">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="df7e0-169">Avec l’authentification requise par défaut est plus sûre que de s’appuyer sur nouveaux contrôleurs et les Pages Razor à inclure le `[Authorize]` attribut.</span><span class="sxs-lookup"><span data-stu-id="df7e0-169">Having authentication required by default is safer than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span> <span data-ttu-id="df7e0-170">Ajoutez le code suivant à la `ConfigureServices` méthode de la *Startup.cs* fichier :</span><span class="sxs-lookup"><span data-stu-id="df7e0-170">Add the following to the `ConfigureServices` method of the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=31-999)]

<span data-ttu-id="df7e0-171">Ajouter [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) à l’Index, des pages sur et de Contact pour les utilisateurs anonymes peuvent obtenir des informations sur le site avant qu’ils s’inscrire.</span><span class="sxs-lookup"><span data-stu-id="df7e0-171">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span> 

[!code-csharp[Main](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

<span data-ttu-id="df7e0-172">Ajouter `[AllowAnonymous]` à la [LoginModel et RegisterModel](https://github.com/aspnet/templating/issues/238).</span><span class="sxs-lookup"><span data-stu-id="df7e0-172">Add `[AllowAnonymous]` to the [LoginModel and RegisterModel](https://github.com/aspnet/templating/issues/238).</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="df7e0-173">Configurer le compte de test</span><span class="sxs-lookup"><span data-stu-id="df7e0-173">Configure the test account</span></span>

<span data-ttu-id="df7e0-174">La `SeedData` classe crée deux comptes : administrateur et le gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="df7e0-174">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="df7e0-175">Utilisez le [Secret gestionnaire](xref:security/app-secrets) pour définir un mot de passe pour ces comptes.</span><span class="sxs-lookup"><span data-stu-id="df7e0-175">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="df7e0-176">Définir le mot de passe à partir du répertoire de projet (le répertoire contenant *Program.cs*) :</span><span class="sxs-lookup"><span data-stu-id="df7e0-176">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="df7e0-177">Mise à jour `Main` à utiliser le mot de passe de test :</span><span class="sxs-lookup"><span data-stu-id="df7e0-177">Update `Main` to use the test password:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="df7e0-178">Créez les comptes de test et de mettre à jour les contacts</span><span class="sxs-lookup"><span data-stu-id="df7e0-178">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="df7e0-179">Mise à jour la `Initialize` méthode dans la `SeedData` classe pour créer les comptes de test :</span><span class="sxs-lookup"><span data-stu-id="df7e0-179">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="df7e0-180">Ajouter l’ID d’utilisateur administrateur et `ContactStatus` aux contacts.</span><span class="sxs-lookup"><span data-stu-id="df7e0-180">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="df7e0-181">Apportez l’une des contacts « Envoyé » et un « Rejected ».</span><span class="sxs-lookup"><span data-stu-id="df7e0-181">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="df7e0-182">Ajouter l’ID d’utilisateur et l’état de tous les contacts.</span><span class="sxs-lookup"><span data-stu-id="df7e0-182">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="df7e0-183">Un seul contact s’affiche :</span><span class="sxs-lookup"><span data-stu-id="df7e0-183">Only one contact is shown:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="df7e0-184">Créer le propriétaire et Gestionnaire de gestionnaires d’autorisations d’administrateur</span><span class="sxs-lookup"><span data-stu-id="df7e0-184">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="df7e0-185">Créer un `ContactIsOwnerAuthorizationHandler` classe dans le *autorisation* dossier.</span><span class="sxs-lookup"><span data-stu-id="df7e0-185">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="df7e0-186">Le `ContactIsOwnerAuthorizationHandler` vérifie que l’utilisateur qui agit sur une ressource de propriétaire de la ressource.</span><span class="sxs-lookup"><span data-stu-id="df7e0-186">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="df7e0-187">Le `ContactIsOwnerAuthorizationHandler` appelle [contexte. Réussir](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) si l’utilisateur authentifié actuel est le propriétaire du contact.</span><span class="sxs-lookup"><span data-stu-id="df7e0-187">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="df7e0-188">Les gestionnaires d’autorisation généralement :</span><span class="sxs-lookup"><span data-stu-id="df7e0-188">Authorization handlers generally:</span></span>

* <span data-ttu-id="df7e0-189">Retourner `context.Succeed` lorsque les conditions sont remplies.</span><span class="sxs-lookup"><span data-stu-id="df7e0-189">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="df7e0-190">Retourner `Task.CompletedTask` lorsque les conditions ne sont pas remplies.</span><span class="sxs-lookup"><span data-stu-id="df7e0-190">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="df7e0-191">`Task.CompletedTask`est une réussite ou l’échec&mdash;permet d’autres gestionnaires d’autorisation Exécuter.</span><span class="sxs-lookup"><span data-stu-id="df7e0-191">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="df7e0-192">Si vous devez explicitement échouer, retourner [contexte. Échec de](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="df7e0-192">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="df7e0-193">L’application permet les propriétaires de contact à modifier, supprimer ou créer leurs propres données.</span><span class="sxs-lookup"><span data-stu-id="df7e0-193">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="df7e0-194">`ContactIsOwnerAuthorizationHandler`n’a pas besoin vérifier l’opération passée dans le paramètre de condition.</span><span class="sxs-lookup"><span data-stu-id="df7e0-194">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="df7e0-195">Créer un gestionnaire d’autorisation de gestionnaire</span><span class="sxs-lookup"><span data-stu-id="df7e0-195">Create a manager authorization handler</span></span>

<span data-ttu-id="df7e0-196">Créer un `ContactManagerAuthorizationHandler` classe dans le *autorisation* dossier.</span><span class="sxs-lookup"><span data-stu-id="df7e0-196">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="df7e0-197">Le `ContactManagerAuthorizationHandler` vérifie l’utilisateur qui agit sur la ressource est un gestionnaire.</span><span class="sxs-lookup"><span data-stu-id="df7e0-197">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="df7e0-198">Seuls les responsables peuvent approuver ou rejeter les modifications de contenu (nouvelles ou modifiées).</span><span class="sxs-lookup"><span data-stu-id="df7e0-198">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="df7e0-199">Créer un gestionnaire d’autorisations d’administrateur</span><span class="sxs-lookup"><span data-stu-id="df7e0-199">Create an administrator authorization handler</span></span>

<span data-ttu-id="df7e0-200">Créer un `ContactAdministratorsAuthorizationHandler` classe dans le *autorisation* dossier.</span><span class="sxs-lookup"><span data-stu-id="df7e0-200">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="df7e0-201">Le `ContactAdministratorsAuthorizationHandler` vérifie l’utilisateur qui agit sur la ressource est un administrateur.</span><span class="sxs-lookup"><span data-stu-id="df7e0-201">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="df7e0-202">Administrateur peut effectuer toutes les opérations.</span><span class="sxs-lookup"><span data-stu-id="df7e0-202">Administrator can do all operations.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="df7e0-203">Inscrire les gestionnaires d’autorisation</span><span class="sxs-lookup"><span data-stu-id="df7e0-203">Register the authorization handlers</span></span>

<span data-ttu-id="df7e0-204">Services à l’aide d’Entity Framework Core doivent être inscrit pour [injection de dépendance](xref:fundamentals/dependency-injection) à l’aide de [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="df7e0-204">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="df7e0-205">Le `ContactIsOwnerAuthorizationHandler` utilise ASP.NET Core [identité](xref:security/authentication/identity), qui est basé sur Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="df7e0-205">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="df7e0-206">Enregistrez les gestionnaires avec la collection de service afin qu’elles soient disponibles pour le `ContactsController` via [injection de dépendance](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="df7e0-206">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="df7e0-207">Ajoutez le code suivant à la fin de `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="df7e0-207">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-999)]

<span data-ttu-id="df7e0-208">`ContactAdministratorsAuthorizationHandler`et `ContactManagerAuthorizationHandler` sont ajoutés en tant que singletons.</span><span class="sxs-lookup"><span data-stu-id="df7e0-208">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="df7e0-209">Ils sont singletons, car ils n’utilisent pas EF et toutes les informations nécessitées sont dans le `Context` paramètre de la `HandleRequirementAsync` (méthode).</span><span class="sxs-lookup"><span data-stu-id="df7e0-209">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="df7e0-210">Autorisation de la prise en charge</span><span class="sxs-lookup"><span data-stu-id="df7e0-210">Support authorization</span></span>

<span data-ttu-id="df7e0-211">Dans cette section, vous mettez à jour les Pages Razor et ajoutez une classe de configuration requise des opérations.</span><span class="sxs-lookup"><span data-stu-id="df7e0-211">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="df7e0-212">Passez en revue la classe de la configuration requise operations contact</span><span class="sxs-lookup"><span data-stu-id="df7e0-212">Review the contact operations requirements class</span></span>

<span data-ttu-id="df7e0-213">Examinez la `ContactOperations` classe.</span><span class="sxs-lookup"><span data-stu-id="df7e0-213">Review the `ContactOperations` class.</span></span> <span data-ttu-id="df7e0-214">Cette classe contient les spécifications le prend en charge de l’application :</span><span class="sxs-lookup"><span data-stu-id="df7e0-214">This class contains the requirements the app supports:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a><span data-ttu-id="df7e0-215">Créer une classe de base pour les Pages Razor</span><span class="sxs-lookup"><span data-stu-id="df7e0-215">Create a base class for the Razor Pages</span></span>

<span data-ttu-id="df7e0-216">Créer une classe de base qui contient les services utilisés dans les Pages Razor contacts.</span><span class="sxs-lookup"><span data-stu-id="df7e0-216">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="df7e0-217">La classe de base place ce code d’initialisation dans un emplacement :</span><span class="sxs-lookup"><span data-stu-id="df7e0-217">The base class puts that initialization code in one location:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="df7e0-218">Le code précédent :</span><span class="sxs-lookup"><span data-stu-id="df7e0-218">The preceding code:</span></span>

* <span data-ttu-id="df7e0-219">Ajoute la `IAuthorizationService` service pour accéder aux gestionnaires d’autorisation.</span><span class="sxs-lookup"><span data-stu-id="df7e0-219">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="df7e0-220">Ajoute l’identité `UserManager` service.</span><span class="sxs-lookup"><span data-stu-id="df7e0-220">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="df7e0-221">Ajoutez la `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="df7e0-221">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="df7e0-222">Mettre à jour le CreateModel</span><span class="sxs-lookup"><span data-stu-id="df7e0-222">Update the CreateModel</span></span>

<span data-ttu-id="df7e0-223">Mettre à jour le constructeur de modèle de page de création à utiliser le `DI_BasePageModel` classe de base :</span><span class="sxs-lookup"><span data-stu-id="df7e0-223">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="df7e0-224">Mise à jour le `CreateModel.OnPostAsync` méthode :</span><span class="sxs-lookup"><span data-stu-id="df7e0-224">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="df7e0-225">Ajouter l’ID utilisateur à la `Contact` modèle.</span><span class="sxs-lookup"><span data-stu-id="df7e0-225">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="df7e0-226">Appelle le Gestionnaire d’autorisation pour vérifier que l’utilisateur est autorisé à créer des contacts.</span><span class="sxs-lookup"><span data-stu-id="df7e0-226">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="df7e0-227">Mettre à jour le IndexModel</span><span class="sxs-lookup"><span data-stu-id="df7e0-227">Update the IndexModel</span></span>

<span data-ttu-id="df7e0-228">Mise à jour le `OnGetAsync` méthode approuvées uniquement les contacts sont présentés aux utilisateurs généraux :</span><span class="sxs-lookup"><span data-stu-id="df7e0-228">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="df7e0-229">Mettre à jour le EditModel</span><span class="sxs-lookup"><span data-stu-id="df7e0-229">Update the EditModel</span></span>

<span data-ttu-id="df7e0-230">Ajoutez un gestionnaire d’autorisation pour vérifier que l’utilisateur propriétaire du contact.</span><span class="sxs-lookup"><span data-stu-id="df7e0-230">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="df7e0-231">Étant donné que l’autorisation de ressource est en cours de validation, le `[Authorize]` attribut n’est pas suffisant.</span><span class="sxs-lookup"><span data-stu-id="df7e0-231">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="df7e0-232">L’application n’a pas accès à la ressource lorsque des attributs sont évaluées.</span><span class="sxs-lookup"><span data-stu-id="df7e0-232">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="df7e0-233">Autorisation basée sur les ressources doit être impérative.</span><span class="sxs-lookup"><span data-stu-id="df7e0-233">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="df7e0-234">Vérifications doivent être effectuées une fois que l’application a accès à la ressource, en le chargeant dans le modèle de page ou en le chargeant dans le Gestionnaire d’elle-même.</span><span class="sxs-lookup"><span data-stu-id="df7e0-234">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="df7e0-235">Vous accédez fréquemment à la ressource en passant la clé de ressource.</span><span class="sxs-lookup"><span data-stu-id="df7e0-235">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="df7e0-236">Mettre à jour le DeleteModel</span><span class="sxs-lookup"><span data-stu-id="df7e0-236">Update the DeleteModel</span></span>

<span data-ttu-id="df7e0-237">Mettre à jour le modèle de page de suppression pour utiliser le Gestionnaire d’autorisation pour vérifier que l’utilisateur a l’autorisation de suppression sur le contact.</span><span class="sxs-lookup"><span data-stu-id="df7e0-237">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="df7e0-238">Le service d’autorisation d’injecter dans les vues</span><span class="sxs-lookup"><span data-stu-id="df7e0-238">Inject the authorization service into the views</span></span>

<span data-ttu-id="df7e0-239">Actuellement, le montre l’interface utilisateur modifie et supprimer les liens pour l’utilisateur ne peut pas modifier les données.</span><span class="sxs-lookup"><span data-stu-id="df7e0-239">Currently, the UI shows edit and delete links for data the user can't modify.</span></span> <span data-ttu-id="df7e0-240">L’interface utilisateur est résolu en appliquant le Gestionnaire d’autorisation pour les vues.</span><span class="sxs-lookup"><span data-stu-id="df7e0-240">The UI is fixed by applying the authorization handler to the views.</span></span>

<span data-ttu-id="df7e0-241">Injecter le service d’autorisation dans le *Views/_ViewImports.cshtml* afin qu’il soit disponible pour toutes les vues de fichiers :</span><span class="sxs-lookup"><span data-stu-id="df7e0-241">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

<span data-ttu-id="df7e0-242">Le balisage précédent ajoute plusieurs `using` instructions.</span><span class="sxs-lookup"><span data-stu-id="df7e0-242">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="df7e0-243">Mise à jour la **modifier** et **supprimer** des liaisons dans *Pages/Contacts/Index.cshtml* afin qu’ils sont rendus uniquement pour les utilisateurs disposant des autorisations appropriées :</span><span class="sxs-lookup"><span data-stu-id="df7e0-243">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-999)]

> [!WARNING]
> <span data-ttu-id="df7e0-244">Le masquage des liens à partir des utilisateurs qui ne sont pas autorisés à modifier les données ne sécuriser l’application.</span><span class="sxs-lookup"><span data-stu-id="df7e0-244">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="df7e0-245">Le masquage des liens rend l’application plus conviviale en affichant des liens n’est valides.</span><span class="sxs-lookup"><span data-stu-id="df7e0-245">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="df7e0-246">Les utilisateurs peuvent hack les URL générées pour appeler modifier et supprimer des opérations sur les données qu’ils ne possèdent pas.</span><span class="sxs-lookup"><span data-stu-id="df7e0-246">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="df7e0-247">Le contrôleur ou Page Razor doit appliquer les vérifications d’accès pour sécuriser les données.</span><span class="sxs-lookup"><span data-stu-id="df7e0-247">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="df7e0-248">Détails de la mise à jour</span><span class="sxs-lookup"><span data-stu-id="df7e0-248">Update Details</span></span>

<span data-ttu-id="df7e0-249">Mettre à jour l’affichage des détails pour les gestionnaires peuvent approuver ou rejeter des contacts :</span><span class="sxs-lookup"><span data-stu-id="df7e0-249">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-999)]

<span data-ttu-id="df7e0-250">Mettre à jour le modèle de page de détails :</span><span class="sxs-lookup"><span data-stu-id="df7e0-250">Update the details page model:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="df7e0-251">Tester l’application terminée</span><span class="sxs-lookup"><span data-stu-id="df7e0-251">Test the completed app</span></span>

<span data-ttu-id="df7e0-252">Si vous êtes à l’aide de Visual Studio Code ou de test sur une plateforme locale qui n’inclut pas un certificat de test pour le protocole SSL :</span><span class="sxs-lookup"><span data-stu-id="df7e0-252">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for SSL:</span></span>

* <span data-ttu-id="df7e0-253">Définissez `"LocalTest:skipSSL": true` dans le *appsettings. Developement.JSON* fichiers à ignorer la spécification SSL.</span><span class="sxs-lookup"><span data-stu-id="df7e0-253">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file to skip the SSL requirement.</span></span> <span data-ttu-id="df7e0-254">Ignorer le protocole SSL uniquement sur un ordinateur de développement.</span><span class="sxs-lookup"><span data-stu-id="df7e0-254">Skip SSL only on a development machine.</span></span>

<span data-ttu-id="df7e0-255">Si l’application contient des contacts :</span><span class="sxs-lookup"><span data-stu-id="df7e0-255">If the app has contacts:</span></span>

* <span data-ttu-id="df7e0-256">Supprimer tous les enregistrements dans la `Contact` table.</span><span class="sxs-lookup"><span data-stu-id="df7e0-256">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="df7e0-257">Redémarrez l’application pour amorcer la base de données.</span><span class="sxs-lookup"><span data-stu-id="df7e0-257">Restart the app to seed the database.</span></span>

<span data-ttu-id="df7e0-258">Inscrire un utilisateur pour parcourir les contacts.</span><span class="sxs-lookup"><span data-stu-id="df7e0-258">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="df7e0-259">Un moyen simple pour tester l’application terminée consiste à lancer des trois différents navigateurs (ou les versions furtif/navigation InPrivate).</span><span class="sxs-lookup"><span data-stu-id="df7e0-259">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="df7e0-260">Dans un navigateur, inscrire un nouvel utilisateur (par exemple, `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="df7e0-260">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="df7e0-261">Connectez-vous à chaque navigateur avec un autre utilisateur.</span><span class="sxs-lookup"><span data-stu-id="df7e0-261">Sign in to each browser with a different user.</span></span> <span data-ttu-id="df7e0-262">Vérifiez les opérations suivantes :</span><span class="sxs-lookup"><span data-stu-id="df7e0-262">Verify the following operations:</span></span>

* <span data-ttu-id="df7e0-263">Les utilisateurs inscrits peuvent afficher toutes les données de contact approuvées.</span><span class="sxs-lookup"><span data-stu-id="df7e0-263">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="df7e0-264">Les utilisateurs inscrits peuvent modifier/supprimer leurs propres données.</span><span class="sxs-lookup"><span data-stu-id="df7e0-264">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="df7e0-265">Gestionnaires peuvent approuver ou rejeter les données de contact.</span><span class="sxs-lookup"><span data-stu-id="df7e0-265">Managers can approve or reject contact data.</span></span> <span data-ttu-id="df7e0-266">Le `Details` vue présente **approuver** et **rejeter** boutons.</span><span class="sxs-lookup"><span data-stu-id="df7e0-266">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="df7e0-267">Les administrateurs peuvent approuver/refuser et modifier/supprimer des données.</span><span class="sxs-lookup"><span data-stu-id="df7e0-267">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="df7e0-268">Utilisateur</span><span class="sxs-lookup"><span data-stu-id="df7e0-268">User</span></span>| <span data-ttu-id="df7e0-269">Options</span><span class="sxs-lookup"><span data-stu-id="df7e0-269">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="df7e0-270">Peut modifier/supprimer des données propres</span><span class="sxs-lookup"><span data-stu-id="df7e0-270">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="df7e0-271">Peuvent approuver/refuser et modification/suppression posséder de données</span><span class="sxs-lookup"><span data-stu-id="df7e0-271">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="df7e0-272">Peut modifier/supprimer et approuver/refuser toutes les données</span><span class="sxs-lookup"><span data-stu-id="df7e0-272">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="df7e0-273">Créer un contact dans le navigateur de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="df7e0-273">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="df7e0-274">Copier l’URL pour le supprimer et modifier du contact de l’administrateur.</span><span class="sxs-lookup"><span data-stu-id="df7e0-274">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="df7e0-275">Collez ces liens dans le navigateur de l’utilisateur de test pour vérifier que l’utilisateur de test ne peut pas effectuer ces opérations.</span><span class="sxs-lookup"><span data-stu-id="df7e0-275">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="df7e0-276">Créer l’application de démarrage</span><span class="sxs-lookup"><span data-stu-id="df7e0-276">Create the starter app</span></span>

* <span data-ttu-id="df7e0-277">Créer une application de Pages Razor nommée « ContactManager »</span><span class="sxs-lookup"><span data-stu-id="df7e0-277">Create a Razor Pages app named "ContactManager"</span></span>

  * <span data-ttu-id="df7e0-278">Créer l’application avec **comptes d’utilisateur individuels**.</span><span class="sxs-lookup"><span data-stu-id="df7e0-278">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="df7e0-279">Pour votre espace de noms correspond à l’espace de noms utilisé dans l’exemple, nommez-Le « ContactManager ».</span><span class="sxs-lookup"><span data-stu-id="df7e0-279">Name it "ContactManager" so your namespace matches the namespace used in the sample.</span></span>

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

  * <span data-ttu-id="df7e0-280">`-uld`Spécifie la base de données locale au lieu de SQLite</span><span class="sxs-lookup"><span data-stu-id="df7e0-280">`-uld` specifies LocalDB instead of SQLite</span></span>

* <span data-ttu-id="df7e0-281">Ajoutez le code suivant `Contact` modèle :</span><span class="sxs-lookup"><span data-stu-id="df7e0-281">Add the following `Contact` model:</span></span>

  [!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="df7e0-282">Structure du `Contact` modèle :</span><span class="sxs-lookup"><span data-stu-id="df7e0-282">Scaffold the `Contact` model:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* <span data-ttu-id="df7e0-283">Mise à jour la **ContactManager** d’ancrage dans le *Pages/_Layout.cshtml* fichier :</span><span class="sxs-lookup"><span data-stu-id="df7e0-283">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="df7e0-284">Structurez la migration initiale et mettre à jour de la base de données :</span><span class="sxs-lookup"><span data-stu-id="df7e0-284">Scaffold the initial migration and update the database:</span></span>

```console
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="df7e0-285">Tester l’application par la création, modification et suppression d’un contact</span><span class="sxs-lookup"><span data-stu-id="df7e0-285">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="df7e0-286">Amorcer la base de données</span><span class="sxs-lookup"><span data-stu-id="df7e0-286">Seed the database</span></span>

<span data-ttu-id="df7e0-287">Ajouter le `SeedData` classe le *données* dossier.</span><span class="sxs-lookup"><span data-stu-id="df7e0-287">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="df7e0-288">Si vous avez téléchargé l’exemple, vous pouvez copier le *SeedData.cs* de fichiers à la *données* dossier du projet de démarrage.</span><span class="sxs-lookup"><span data-stu-id="df7e0-288">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="df7e0-289">Appelez `SeedData.Initialize` de `Main`:</span><span class="sxs-lookup"><span data-stu-id="df7e0-289">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[Main](secure-data/samples/starter2/Program.cs?name=snippet)]

<span data-ttu-id="df7e0-290">Test de l’application d’amorçage la base de données.</span><span class="sxs-lookup"><span data-stu-id="df7e0-290">Test that the app seeded the database.</span></span> <span data-ttu-id="df7e0-291">S’il existe des lignes dans la base de données de contact, la méthode de valeur initiale ne s’exécute pas.</span><span class="sxs-lookup"><span data-stu-id="df7e0-291">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="df7e0-292">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="df7e0-292">Additional resources</span></span>

* <span data-ttu-id="df7e0-293">[ASP.NET Core d’autorisation Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="df7e0-293">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="df7e0-294">Ce laboratoire présente en détail sur les fonctionnalités de sécurité présentées dans ce didacticiel.</span><span class="sxs-lookup"><span data-stu-id="df7e0-294">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="df7e0-295">Autorisation dans ASP.NET Core : Simple, des rôles, basée sur les revendications et personnalisées</span><span class="sxs-lookup"><span data-stu-id="df7e0-295">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="df7e0-296">Autorisation basée sur une stratégie personnalisée</span><span class="sxs-lookup"><span data-stu-id="df7e0-296">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
