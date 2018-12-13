---
title: Bien démarrer avec ASP.NET Core
author: rick-anderson
description: Didacticiel rapide qui crée et exécute une application Hello World simple à l’aide d’ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/11/2018
uid: getting-started
ms.openlocfilehash: cf9e731f7638687b3f40b42864ef7ee8f5522b39
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284349"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="40896-103">Tutoriel : Bien démarrer avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="40896-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="40896-104">Ce tutoriel montre comment utiliser l’interface de ligne de commande .NET Core pour créer une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="40896-104">This tutorial shows how to use the .NET Core command-line interface to create an ASP.NET Core web app.</span></span>

<span data-ttu-id="40896-105">Vous allez apprendre à :</span><span class="sxs-lookup"><span data-stu-id="40896-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="40896-106">Créer un projet application web.</span><span class="sxs-lookup"><span data-stu-id="40896-106">Create a web app project.</span></span>
> * <span data-ttu-id="40896-107">Activer HTTPS localement.</span><span class="sxs-lookup"><span data-stu-id="40896-107">Enable local HTTPS.</span></span>
> * <span data-ttu-id="40896-108">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="40896-108">Run the app.</span></span>
> * <span data-ttu-id="40896-109">Modifier une page Razor.</span><span class="sxs-lookup"><span data-stu-id="40896-109">Edit a Razor page.</span></span>

<span data-ttu-id="40896-110">À la fin du tutoriel, vous disposerez d’une application web qui fonctionne et s’exécute sur votre machine locale.</span><span class="sxs-lookup"><span data-stu-id="40896-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Page d’accueil d’application web](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="40896-112">Prérequis</span><span class="sxs-lookup"><span data-stu-id="40896-112">Prerequisites</span></span>

* [<span data-ttu-id="40896-113">SDK .NET Core 2.2</span><span class="sxs-lookup"><span data-stu-id="40896-113">.NET Core 2.2 SDK</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a><span data-ttu-id="40896-114">Créer un projet application web</span><span class="sxs-lookup"><span data-stu-id="40896-114">Create a web app project</span></span>

<span data-ttu-id="40896-115">Ouvrez un interpréteur de commandes, puis entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="40896-115">Open a command shell, and enter the following command:</span></span>

```console
dotnet new webapp -o aspnetcoreapp
```

## <a name="enable-local-https"></a><span data-ttu-id="40896-116">Activer HTTPS localement</span><span class="sxs-lookup"><span data-stu-id="40896-116">Enable local HTTPS</span></span>

<span data-ttu-id="40896-117">Approuvez le certificat de développement HTTPS :</span><span class="sxs-lookup"><span data-stu-id="40896-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="40896-118">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="40896-118">Windows</span></span>](#tab/windows)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="40896-119">La commande précédente affiche la boîte de dialogue suivante :</span><span class="sxs-lookup"><span data-stu-id="40896-119">The preceding command displays the following dialog:</span></span>

![Boîte de dialogue Avertissement de sécurité](_static/cert.png)

<span data-ttu-id="40896-121">Sélectionnez **Oui** si vous acceptez d’approuver le certificat de développement.</span><span class="sxs-lookup"><span data-stu-id="40896-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="40896-122">macOS</span><span class="sxs-lookup"><span data-stu-id="40896-122">macOS</span></span>](#tab/macos)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="40896-123">La commande précédente affiche le message suivant :</span><span class="sxs-lookup"><span data-stu-id="40896-123">The preceding command displays the following message:</span></span>

<span data-ttu-id="40896-124">*L’approbation du certificat de développement HTTPS a été demandée. Si le certificat n’a pas encore été approuvé, nous exécutons la commande suivante :* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="40896-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span></span>  
<span data-ttu-id="40896-125">\* Cette commande peut vous demander votre mot de passe afin d’installer le certificat sur le trousseau système.</span><span class="sxs-lookup"><span data-stu-id="40896-125">\*This command might prompt you for your password to install the certificate on the system keychain.</span></span>

<span data-ttu-id="40896-126">Mot de passe :\*</span><span class="sxs-lookup"><span data-stu-id="40896-126">Password:\*</span></span>

<span data-ttu-id="40896-127">Entrez votre mot de passe si vous acceptez d’approuver le certificat de développement.</span><span class="sxs-lookup"><span data-stu-id="40896-127">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="40896-128">Linux</span><span class="sxs-lookup"><span data-stu-id="40896-128">Linux</span></span>](#tab/linux)

<span data-ttu-id="40896-129">Consultez la documentation de votre distribution Linux pour savoir comment approuver le certificat de développement HTTPS.</span><span class="sxs-lookup"><span data-stu-id="40896-129">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

## <a name="run-the-app"></a><span data-ttu-id="40896-130">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="40896-130">Run the app</span></span>

<span data-ttu-id="40896-131">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="40896-131">Run the following commands:</span></span>

```console
cd aspnetcoreapp
dotnet run
```

<span data-ttu-id="40896-132">Accédez à [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="40896-132">Browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="40896-133">Cliquez sur **Accepter** pour accepter la politique de confidentialité et de cookies.</span><span class="sxs-lookup"><span data-stu-id="40896-133">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="40896-134">Cette application ne conserve pas les informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="40896-134">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="40896-135">Modifier une page Razor</span><span class="sxs-lookup"><span data-stu-id="40896-135">Edit a Razor page</span></span>

<span data-ttu-id="40896-136">Ouvrez *Pages/Index.cshtml* et modifiez la page avec le balisage mis en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="40896-136">Open *Pages/Index.cshtml* and modify the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="40896-137">Accédez à [https://localhost:5001](https://localhost:5001) et vérifiez que les changements sont affichés.</span><span class="sxs-lookup"><span data-stu-id="40896-137">Browse to [https://localhost:5001](https://localhost:5001), and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="40896-138">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="40896-138">Next steps</span></span>

<span data-ttu-id="40896-139">Dans ce didacticiel, vous avez appris à :</span><span class="sxs-lookup"><span data-stu-id="40896-139">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="40896-140">Créer un projet application web.</span><span class="sxs-lookup"><span data-stu-id="40896-140">Create a web app project.</span></span>
> * <span data-ttu-id="40896-141">Activer HTTPS localement.</span><span class="sxs-lookup"><span data-stu-id="40896-141">Enable local HTTPS.</span></span>
> * <span data-ttu-id="40896-142">Exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="40896-142">Run the project.</span></span>
> * <span data-ttu-id="40896-143">Apportez la modification souhaitée.</span><span class="sxs-lookup"><span data-stu-id="40896-143">Make a change.</span></span>

<span data-ttu-id="40896-144">Pour en savoir plus sur ASP.NET Core, consultez la présentation :</span><span class="sxs-lookup"><span data-stu-id="40896-144">To learn more about ASP.NET Core, see the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index>

> [!NOTE]
> <span data-ttu-id="40896-145">Nous testons la facilité d’utilisation d’une nouvelle structure proposée pour la table des matières d’ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="40896-145">We're testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span> <span data-ttu-id="40896-146">Si vous avez quelques minutes pour essayer un exercice de recherche de sept différentes rubriques dans la table des matières actuelle ou proposée, [cliquez ici pour participer à l’étude](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span><span class="sxs-lookup"><span data-stu-id="40896-146">If you have a few minutes to try an exercise of finding seven different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span></span>
