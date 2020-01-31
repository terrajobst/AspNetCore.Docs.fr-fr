---
title: Ajouter, télécharger et supprimer des données utilisateur à l’identité dans un projet ASP.NET Core
author: rick-anderson
description: Découvrez comment ajouter des données utilisateur personnalisées à l’identité dans un projet ASP.NET Core. Supprimer des données par RGPD.
ms.author: riande
ms.date: 01/28/2020
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: e08c02e2e5d4a429aae10c59e7ae3ea48c975067
ms.sourcegitcommit: c81ef12a1b6e6ac838e5e07042717cf492e6635b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76885547"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="54bb5-104">Ajouter, télécharger et supprimer des données utilisateur personnalisées pour l’identité dans un projet ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="54bb5-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="54bb5-105">Par [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="54bb5-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="54bb5-106">Cet article explique comment :</span><span class="sxs-lookup"><span data-stu-id="54bb5-106">This article shows how to:</span></span>

* <span data-ttu-id="54bb5-107">Ajouter des données utilisateur personnalisées pour une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="54bb5-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="54bb5-108">Marquez le modèle de données utilisateur personnalisé avec l’attribut <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> pour qu’il soit automatiquement disponible au téléchargement et à la suppression.</span><span class="sxs-lookup"><span data-stu-id="54bb5-108">Mark the custom user data model with the <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="54bb5-109">Rendre les données peuvent être téléchargés et supprimés à mieux répondre à [RGPD](xref:security/gdpr) configuration requise.</span><span class="sxs-lookup"><span data-stu-id="54bb5-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="54bb5-110">L’exemple de projet est créé à partir d’une application web de Pages Razor, mais les instructions sont similaires pour une application web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="54bb5-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="54bb5-111">[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="54bb5-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="54bb5-112">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="54bb5-112">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a><span data-ttu-id="54bb5-113">Créer une application web Razor</span><span class="sxs-lookup"><span data-stu-id="54bb5-113">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="54bb5-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="54bb5-114">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="54bb5-115">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="54bb5-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="54bb5-116">Nommez le projet **application Web 1** si vous souhaitez qu’il correspond à l’espace de noms de la [télécharger l’exemple](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span><span class="sxs-lookup"><span data-stu-id="54bb5-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="54bb5-117">Sélectionnez **ASP.net Core application Web** > **OK**</span><span class="sxs-lookup"><span data-stu-id="54bb5-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="54bb5-118">Sélectionnez **ASP.NET Core 3,0** dans la liste déroulante</span><span class="sxs-lookup"><span data-stu-id="54bb5-118">Select **ASP.NET Core 3.0** in the dropdown</span></span>
* <span data-ttu-id="54bb5-119">Sélectionnez **application Web** > **OK**</span><span class="sxs-lookup"><span data-stu-id="54bb5-119">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="54bb5-120">Générez et exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="54bb5-120">Build and run the project.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="54bb5-121">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="54bb5-121">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="54bb5-122">Nommez le projet **application Web 1** si vous souhaitez qu’il correspond à l’espace de noms de la [télécharger l’exemple](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span><span class="sxs-lookup"><span data-stu-id="54bb5-122">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="54bb5-123">Sélectionnez **ASP.net Core application Web** > **OK**</span><span class="sxs-lookup"><span data-stu-id="54bb5-123">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="54bb5-124">Sélectionnez **ASP.NET Core 2,2** dans la liste déroulante</span><span class="sxs-lookup"><span data-stu-id="54bb5-124">Select **ASP.NET Core 2.2** in the dropdown</span></span>
* <span data-ttu-id="54bb5-125">Sélectionnez **application Web** > **OK**</span><span class="sxs-lookup"><span data-stu-id="54bb5-125">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="54bb5-126">Générez et exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="54bb5-126">Build and run the project.</span></span>

::: moniker-end


# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="54bb5-127">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="54bb5-127">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="54bb5-128">Exécuter le Générateur de modèles automatique identité</span><span class="sxs-lookup"><span data-stu-id="54bb5-128">Run the Identity scaffolder</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="54bb5-129">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="54bb5-129">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="54bb5-130">À partir de **l’Explorateur de solutions**, avec le bouton droit sur le projet > **ajouter** > **nouvel élément structuré**.</span><span class="sxs-lookup"><span data-stu-id="54bb5-130">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="54bb5-131">Dans le volet gauche de la boîte de dialogue **Ajouter une structure** , sélectionnez **identité** > **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="54bb5-131">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="54bb5-132">Dans la boîte de dialogue **Ajouter une identité** , les options suivantes sont disponibles :</span><span class="sxs-lookup"><span data-stu-id="54bb5-132">In the **Add Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="54bb5-133">Sélectionnez le fichier de disposition existante *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="54bb5-133">Select the existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="54bb5-134">Sélectionnez les fichiers suivants pour remplacer :</span><span class="sxs-lookup"><span data-stu-id="54bb5-134">Select the following files to override:</span></span>
    * <span data-ttu-id="54bb5-135">**Compte/inscrire**</span><span class="sxs-lookup"><span data-stu-id="54bb5-135">**Account/Register**</span></span>
    * <span data-ttu-id="54bb5-136">**Compte/gérer/Index**</span><span class="sxs-lookup"><span data-stu-id="54bb5-136">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="54bb5-137">Sélectionnez le **+** bouton pour créer un nouveau **classe de contexte de données**.</span><span class="sxs-lookup"><span data-stu-id="54bb5-137">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="54bb5-138">Acceptez le type (**WebApp1.Models.WebApp1Context** si le projet est nommé **application Web 1**).</span><span class="sxs-lookup"><span data-stu-id="54bb5-138">Accept the type (**WebApp1.Models.WebApp1Context** if the project is named **WebApp1**).</span></span>
  * <span data-ttu-id="54bb5-139">Sélectionnez le **+** bouton pour créer un nouveau **classe utilisateur**.</span><span class="sxs-lookup"><span data-stu-id="54bb5-139">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="54bb5-140">Acceptez le type (**WebApp1User** si le projet est nommé **application Web 1**) > **ajouter**.</span><span class="sxs-lookup"><span data-stu-id="54bb5-140">Accept the type (**WebApp1User** if the project is named **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="54bb5-141">Sélectionnez **Ajouter** .</span><span class="sxs-lookup"><span data-stu-id="54bb5-141">Select **Add**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="54bb5-142">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="54bb5-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="54bb5-143">Si vous n’avez pas encore installé le Générateur de modèles automatique ASP.NET Core, vous pouvez l’installer maintenant :</span><span class="sxs-lookup"><span data-stu-id="54bb5-143">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="54bb5-144">Ajouter une référence de package à [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) au fichier projet (.csproj).</span><span class="sxs-lookup"><span data-stu-id="54bb5-144">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="54bb5-145">Dans le répertoire du projet, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="54bb5-145">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="54bb5-146">Exécutez la commande suivante pour répertorier les options de génération de modèles automatique d’identité :</span><span class="sxs-lookup"><span data-stu-id="54bb5-146">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="54bb5-147">Dans le dossier du projet, exécutez le Générateur de modèles automatique identité :</span><span class="sxs-lookup"><span data-stu-id="54bb5-147">In the project folder, run the Identity scaffolder:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

<span data-ttu-id="54bb5-148">Suivez les instructions de [Migrations, UseAuthentication et disposition](xref:security/authentication/scaffold-identity#efm) pour effectuer les étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="54bb5-148">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="54bb5-149">Créer une migration et mettre à jour de la base de données.</span><span class="sxs-lookup"><span data-stu-id="54bb5-149">Create a migration and update the database.</span></span>
* <span data-ttu-id="54bb5-150">Ajoutez `UseAuthentication` à `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="54bb5-150">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="54bb5-151">Ajouter `<partial name="_LoginPartial" />` pour le fichier de disposition.</span><span class="sxs-lookup"><span data-stu-id="54bb5-151">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="54bb5-152">Testez l’application :</span><span class="sxs-lookup"><span data-stu-id="54bb5-152">Test the app:</span></span>
  * <span data-ttu-id="54bb5-153">Inscrire un utilisateur</span><span class="sxs-lookup"><span data-stu-id="54bb5-153">Register a user</span></span>
  * <span data-ttu-id="54bb5-154">Sélectionnez le nouveau nom d’utilisateur (à côté du **déconnexion** lien).</span><span class="sxs-lookup"><span data-stu-id="54bb5-154">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="54bb5-155">Vous devrez peut-être agrandir la fenêtre ou sélectionnez l’icône de barre de navigation pour afficher le nom d’utilisateur et d’autres liens.</span><span class="sxs-lookup"><span data-stu-id="54bb5-155">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="54bb5-156">Sélectionnez le **les données personnelles** onglet.</span><span class="sxs-lookup"><span data-stu-id="54bb5-156">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="54bb5-157">Sélectionnez le **télécharger** bouton et examiné la *PersonalData.json* fichier.</span><span class="sxs-lookup"><span data-stu-id="54bb5-157">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="54bb5-158">Test du **supprimer** bouton, ce qui supprime le connecté sur l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="54bb5-158">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="54bb5-159">Ajouter des données utilisateur personnalisées pour la base de données d’identité</span><span class="sxs-lookup"><span data-stu-id="54bb5-159">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="54bb5-160">Mise à jour le `IdentityUser` dérivée de la classe avec des propriétés personnalisées.</span><span class="sxs-lookup"><span data-stu-id="54bb5-160">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="54bb5-161">Si vous avez nommé le projet d’application Web 1, le fichier est nommé *Areas/Identity/Data/WebApp1User.cs*.</span><span class="sxs-lookup"><span data-stu-id="54bb5-161">If you named the project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="54bb5-162">Mettre à jour le fichier avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="54bb5-162">Update the file with the following code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

<span data-ttu-id="54bb5-163">Les propriétés avec l’attribut [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="54bb5-163">Properties with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) attribute are:</span></span>

* <span data-ttu-id="54bb5-164">Supprimé quand le *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Page Razor appelle `UserManager.Delete`.</span><span class="sxs-lookup"><span data-stu-id="54bb5-164">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="54bb5-165">Inclus dans les données téléchargées par le *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Page Razor.</span><span class="sxs-lookup"><span data-stu-id="54bb5-165">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="54bb5-166">Mettre à jour de la page Account/Manage/Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="54bb5-166">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="54bb5-167">Mise à jour le `InputModel` dans *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* code en surbrillance avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="54bb5-167">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

<span data-ttu-id="54bb5-168">Mise à jour le *Areas/Identity/Pages/Account/Manage/Index.cshtml* avec le balisage en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="54bb5-168">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

<span data-ttu-id="54bb5-169">Mise à jour le *Areas/Identity/Pages/Account/Manage/Index.cshtml* avec le balisage en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="54bb5-169">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="54bb5-170">Mettre à jour de la page Account/Register.cshtml en</span><span class="sxs-lookup"><span data-stu-id="54bb5-170">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="54bb5-171">Mise à jour le `InputModel` dans *Areas/Identity/Pages/Account/Register.cshtml.cs* code en surbrillance avec les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="54bb5-171">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

<span data-ttu-id="54bb5-172">Mise à jour le *Areas/Identity/Pages/Account/Register.cshtml* avec le balisage en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="54bb5-172">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

<span data-ttu-id="54bb5-173">Mise à jour le *Areas/Identity/Pages/Account/Register.cshtml* avec le balisage en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="54bb5-173">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


<span data-ttu-id="54bb5-174">créer le projet ;</span><span class="sxs-lookup"><span data-stu-id="54bb5-174">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="54bb5-175">Ajoutez une migration pour les données utilisateur personnalisées</span><span class="sxs-lookup"><span data-stu-id="54bb5-175">Add a migration for the custom user data</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="54bb5-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="54bb5-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="54bb5-177">Dans Visual Studio **Console du Gestionnaire de Package**:</span><span class="sxs-lookup"><span data-stu-id="54bb5-177">In the Visual Studio **Package Manager Console**:</span></span>

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="54bb5-178">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="54bb5-178">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="54bb5-179">Test de créer, afficher, télécharger, supprimer les données utilisateur personnalisées</span><span class="sxs-lookup"><span data-stu-id="54bb5-179">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="54bb5-180">Testez l’application :</span><span class="sxs-lookup"><span data-stu-id="54bb5-180">Test the app:</span></span>

* <span data-ttu-id="54bb5-181">Inscrire un nouvel utilisateur.</span><span class="sxs-lookup"><span data-stu-id="54bb5-181">Register a new user.</span></span>
* <span data-ttu-id="54bb5-182">Afficher les données utilisateur personnalisées sur le `/Identity/Account/Manage` page.</span><span class="sxs-lookup"><span data-stu-id="54bb5-182">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="54bb5-183">Télécharger et afficher les données personnelles les utilisateurs à partir de la `/Identity/Account/Manage/PersonalData` page.</span><span class="sxs-lookup"><span data-stu-id="54bb5-183">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>
