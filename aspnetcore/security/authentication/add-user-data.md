---
title: Ajouter, télécharger et supprimer des données utilisateur à l’identité dans un projet ASP.NET Core
author: rick-anderson
description: Découvrez comment ajouter des données utilisateur personnalisées à l’identité dans un projet ASP.NET Core. Supprimer des données par RGPD.
ms.author: riande
ms.date: 01/28/2020
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 7a67f55da0e685ed3fd5badb30e8be683411a5ae
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662686"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="e12f4-104">Ajouter, télécharger et supprimer des données utilisateur personnalisées pour l’identité dans un projet ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e12f4-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="e12f4-105">De [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e12f4-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e12f4-106">Cet article montre comment :</span><span class="sxs-lookup"><span data-stu-id="e12f4-106">This article shows how to:</span></span>

* <span data-ttu-id="e12f4-107">Ajouter des données utilisateur personnalisées pour une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e12f4-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="e12f4-108">Marquez le modèle de données utilisateur personnalisé avec l’attribut <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> pour qu’il soit automatiquement disponible au téléchargement et à la suppression.</span><span class="sxs-lookup"><span data-stu-id="e12f4-108">Mark the custom user data model with the <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="e12f4-109">La possibilité de télécharger et de supprimer les données vous aide à répondre aux exigences de [RGPD](xref:security/gdpr) .</span><span class="sxs-lookup"><span data-stu-id="e12f4-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="e12f4-110">L’exemple de projet est créé à partir d’une application web de Pages Razor, mais les instructions sont similaires pour une application web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="e12f4-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="e12f4-111">[Affichez ou téléchargez l’exemple de code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([procédure de téléchargement](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e12f4-111">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e12f4-112">Conditions préalables requises</span><span class="sxs-lookup"><span data-stu-id="e12f4-112">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a><span data-ttu-id="e12f4-113">Créer une application web Razor</span><span class="sxs-lookup"><span data-stu-id="e12f4-113">Create a Razor web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e12f4-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e12f4-114">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="e12f4-115">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="e12f4-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="e12f4-116">Nommez le projet **application Web 1** si vous souhaitez qu’il corresponde à l’espace de noms de l’exemple de code de [Téléchargement](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) .</span><span class="sxs-lookup"><span data-stu-id="e12f4-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="e12f4-117">Sélectionnez **ASP.net Core application Web** > **OK**</span><span class="sxs-lookup"><span data-stu-id="e12f4-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="e12f4-118">Sélectionnez **ASP.NET Core 3,0** dans la liste déroulante</span><span class="sxs-lookup"><span data-stu-id="e12f4-118">Select **ASP.NET Core 3.0** in the dropdown</span></span>
* <span data-ttu-id="e12f4-119">Sélectionnez **application Web** > **OK**</span><span class="sxs-lookup"><span data-stu-id="e12f4-119">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="e12f4-120">Générez et exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="e12f4-120">Build and run the project.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="e12f4-121">Dans Visual Studio, dans le menu **Fichier**, sélectionnez **Nouveau** > **Projet**.</span><span class="sxs-lookup"><span data-stu-id="e12f4-121">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="e12f4-122">Nommez le projet **application Web 1** si vous souhaitez qu’il corresponde à l’espace de noms de l’exemple de code de [Téléchargement](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) .</span><span class="sxs-lookup"><span data-stu-id="e12f4-122">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="e12f4-123">Sélectionnez **ASP.net Core application Web** > **OK**</span><span class="sxs-lookup"><span data-stu-id="e12f4-123">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="e12f4-124">Sélectionnez **ASP.NET Core 2,2** dans la liste déroulante</span><span class="sxs-lookup"><span data-stu-id="e12f4-124">Select **ASP.NET Core 2.2** in the dropdown</span></span>
* <span data-ttu-id="e12f4-125">Sélectionnez **application Web** > **OK**</span><span class="sxs-lookup"><span data-stu-id="e12f4-125">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="e12f4-126">Générez et exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="e12f4-126">Build and run the project.</span></span>

::: moniker-end


# <a name="net-core-cli"></a>[<span data-ttu-id="e12f4-127">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="e12f4-127">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="e12f4-128">Exécuter le Générateur de modèles automatique identité</span><span class="sxs-lookup"><span data-stu-id="e12f4-128">Run the Identity scaffolder</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e12f4-129">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e12f4-129">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e12f4-130">Dans **Explorateur de solutions**, cliquez avec le bouton droit sur le projet > **Ajouter** > **nouvel élément de génération de modèles**automatique.</span><span class="sxs-lookup"><span data-stu-id="e12f4-130">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="e12f4-131">Dans le volet gauche de la boîte de dialogue **Ajouter une structure** , sélectionnez **identité** > **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e12f4-131">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="e12f4-132">Dans la boîte de dialogue **Ajouter une identité** , les options suivantes sont disponibles :</span><span class="sxs-lookup"><span data-stu-id="e12f4-132">In the **Add Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="e12f4-133">Sélectionner le fichier de disposition existant *~/Pages/Shared/_Layout. cshtml*</span><span class="sxs-lookup"><span data-stu-id="e12f4-133">Select the existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="e12f4-134">Sélectionnez les fichiers suivants pour remplacer :</span><span class="sxs-lookup"><span data-stu-id="e12f4-134">Select the following files to override:</span></span>
    * <span data-ttu-id="e12f4-135">**Compte/inscription**</span><span class="sxs-lookup"><span data-stu-id="e12f4-135">**Account/Register**</span></span>
    * <span data-ttu-id="e12f4-136">**Compte/gestion/index**</span><span class="sxs-lookup"><span data-stu-id="e12f4-136">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="e12f4-137">Sélectionnez le bouton **+** pour créer une **classe de contexte de données**.</span><span class="sxs-lookup"><span data-stu-id="e12f4-137">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="e12f4-138">Acceptez le type (**application Web 1. Models. WebApp1Context** si le projet est nommé **application Web 1**).</span><span class="sxs-lookup"><span data-stu-id="e12f4-138">Accept the type (**WebApp1.Models.WebApp1Context** if the project is named **WebApp1**).</span></span>
  * <span data-ttu-id="e12f4-139">Sélectionnez le bouton **+** pour créer une nouvelle **classe d’utilisateur**.</span><span class="sxs-lookup"><span data-stu-id="e12f4-139">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="e12f4-140">Acceptez le type (**WebApp1User** si le projet est nommé **application Web 1**) > **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e12f4-140">Accept the type (**WebApp1User** if the project is named **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="e12f4-141">Sélectionnez **Ajouter**.</span><span class="sxs-lookup"><span data-stu-id="e12f4-141">Select **Add**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="e12f4-142">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="e12f4-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e12f4-143">Si vous n’avez pas encore installé le Générateur de modèles automatique ASP.NET Core, vous pouvez l’installer maintenant :</span><span class="sxs-lookup"><span data-stu-id="e12f4-143">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="e12f4-144">Ajoutez une référence de package à [Microsoft. VisualStudio. Web. CodeGeneration. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) dans le fichier projet (. csproj).</span><span class="sxs-lookup"><span data-stu-id="e12f4-144">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="e12f4-145">Dans le répertoire du projet, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="e12f4-145">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="e12f4-146">Exécutez la commande suivante pour répertorier les options de génération de modèles automatique d’identité :</span><span class="sxs-lookup"><span data-stu-id="e12f4-146">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="e12f4-147">Dans le dossier du projet, exécutez le Générateur de modèles automatique identité :</span><span class="sxs-lookup"><span data-stu-id="e12f4-147">In the project folder, run the Identity scaffolder:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

<span data-ttu-id="e12f4-148">Suivez les instructions dans [migrations, UseAuthentication et Layout](xref:security/authentication/scaffold-identity#efm) pour effectuer les étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="e12f4-148">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="e12f4-149">Créer une migration et mettre à jour de la base de données.</span><span class="sxs-lookup"><span data-stu-id="e12f4-149">Create a migration and update the database.</span></span>
* <span data-ttu-id="e12f4-150">Ajout de `UseAuthentication` à `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="e12f4-150">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="e12f4-151">Ajoutez `<partial name="_LoginPartial" />` au fichier de disposition.</span><span class="sxs-lookup"><span data-stu-id="e12f4-151">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="e12f4-152">Testez l’application :</span><span class="sxs-lookup"><span data-stu-id="e12f4-152">Test the app:</span></span>
  * <span data-ttu-id="e12f4-153">Inscrire un utilisateur</span><span class="sxs-lookup"><span data-stu-id="e12f4-153">Register a user</span></span>
  * <span data-ttu-id="e12f4-154">Sélectionnez le nouveau nom d’utilisateur (à côté du lien de **déconnexion** ).</span><span class="sxs-lookup"><span data-stu-id="e12f4-154">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="e12f4-155">Vous devrez peut-être agrandir la fenêtre ou sélectionnez l’icône de barre de navigation pour afficher le nom d’utilisateur et d’autres liens.</span><span class="sxs-lookup"><span data-stu-id="e12f4-155">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="e12f4-156">Sélectionnez l’onglet **données personnelles** .</span><span class="sxs-lookup"><span data-stu-id="e12f4-156">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="e12f4-157">Sélectionnez le bouton **Télécharger** et examinez le fichier *PersonalData. JSON* .</span><span class="sxs-lookup"><span data-stu-id="e12f4-157">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="e12f4-158">Testez le bouton **supprimer** , qui supprime l’utilisateur connecté.</span><span class="sxs-lookup"><span data-stu-id="e12f4-158">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="e12f4-159">Ajouter des données utilisateur personnalisées pour la base de données d’identité</span><span class="sxs-lookup"><span data-stu-id="e12f4-159">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="e12f4-160">Mettez à jour la classe dérivée `IdentityUser` avec des propriétés personnalisées.</span><span class="sxs-lookup"><span data-stu-id="e12f4-160">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="e12f4-161">Si vous avez nommé le projet application Web 1, le fichier est nommé *Areas/Identity/Data/WebApp1User. cs*.</span><span class="sxs-lookup"><span data-stu-id="e12f4-161">If you named the project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="e12f4-162">Mettre à jour le fichier avec le code suivant :</span><span class="sxs-lookup"><span data-stu-id="e12f4-162">Update the file with the following code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

<span data-ttu-id="e12f4-163">Les propriétés avec l’attribut [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) sont les suivantes :</span><span class="sxs-lookup"><span data-stu-id="e12f4-163">Properties with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) attribute are:</span></span>

* <span data-ttu-id="e12f4-164">Supprimé lorsque la page Razor *Areas/Identity/pages/Account/Manage/DeletePersonalData. cshtml* appelle `UserManager.Delete`.</span><span class="sxs-lookup"><span data-stu-id="e12f4-164">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="e12f4-165">Inclus dans les données téléchargées par la page Razor *Areas/Identity/pages/Account/Manage/DownloadPersonalData. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="e12f4-165">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="e12f4-166">Mettre à jour de la page Account/Manage/Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="e12f4-166">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="e12f4-167">Mettez à jour le `InputModel` dans *Areas/Identity/pages/Account/Manage/index. cshtml. cs* avec le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="e12f4-167">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

<span data-ttu-id="e12f4-168">Mettez à jour les *zones/Identity/pages/Account/Manage/index. cshtml* avec le balisage en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="e12f4-168">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

<span data-ttu-id="e12f4-169">Mettez à jour les *zones/Identity/pages/Account/Manage/index. cshtml* avec le balisage en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="e12f4-169">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="e12f4-170">Mettre à jour de la page Account/Register.cshtml en</span><span class="sxs-lookup"><span data-stu-id="e12f4-170">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="e12f4-171">Mettez à jour le `InputModel` dans *Areas/Identity/pages/Account/Register. cshtml. cs* avec le code en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="e12f4-171">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

<span data-ttu-id="e12f4-172">Mettez à jour les *zones/Identity/pages/Account/Register. cshtml* avec le balisage en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="e12f4-172">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

<span data-ttu-id="e12f4-173">Mettez à jour les *zones/Identity/pages/Account/Register. cshtml* avec le balisage en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="e12f4-173">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


<span data-ttu-id="e12f4-174">Créez le projet.</span><span class="sxs-lookup"><span data-stu-id="e12f4-174">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="e12f4-175">Ajoutez une migration pour les données utilisateur personnalisées</span><span class="sxs-lookup"><span data-stu-id="e12f4-175">Add a migration for the custom user data</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e12f4-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e12f4-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e12f4-177">Dans la console du **Gestionnaire de package**Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="e12f4-177">In the Visual Studio **Package Manager Console**:</span></span>

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-cli"></a>[<span data-ttu-id="e12f4-178">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="e12f4-178">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="e12f4-179">Test de créer, afficher, télécharger, supprimer les données utilisateur personnalisées</span><span class="sxs-lookup"><span data-stu-id="e12f4-179">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="e12f4-180">Testez l’application :</span><span class="sxs-lookup"><span data-stu-id="e12f4-180">Test the app:</span></span>

* <span data-ttu-id="e12f4-181">Inscrire un nouvel utilisateur.</span><span class="sxs-lookup"><span data-stu-id="e12f4-181">Register a new user.</span></span>
* <span data-ttu-id="e12f4-182">Affichez les données utilisateur personnalisées sur la page `/Identity/Account/Manage`.</span><span class="sxs-lookup"><span data-stu-id="e12f4-182">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="e12f4-183">Téléchargez et affichez les données personnelles des utilisateurs à partir de la page `/Identity/Account/Manage/PersonalData`.</span><span class="sxs-lookup"><span data-stu-id="e12f4-183">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>
