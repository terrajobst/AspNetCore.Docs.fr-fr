---
title: Bien démarrer avec ASP.NET Core
author: rick-anderson
description: Didacticiel rapide qui crée et exécute une application Hello World simple à l’aide d’ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/11/2018
uid: getting-started
ms.openlocfilehash: dc85fe9189c93476859a6c00d60ec24d6eeec3fa
ms.sourcegitcommit: a16352c1c88a71770ab3922200a8cd148fb278a6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53335310"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="14f2e-103">Tutoriel : Bien démarrer avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="14f2e-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="14f2e-104">Ce tutoriel montre comment utiliser l’interface de ligne de commande .NET Core pour créer une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="14f2e-104">This tutorial shows how to use the .NET Core command-line interface to create an ASP.NET Core web app.</span></span>

<span data-ttu-id="14f2e-105">Vous allez apprendre à :</span><span class="sxs-lookup"><span data-stu-id="14f2e-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="14f2e-106">Créer un projet application web.</span><span class="sxs-lookup"><span data-stu-id="14f2e-106">Create a web app project.</span></span>
> * <span data-ttu-id="14f2e-107">Activer HTTPS localement.</span><span class="sxs-lookup"><span data-stu-id="14f2e-107">Enable local HTTPS.</span></span>
> * <span data-ttu-id="14f2e-108">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="14f2e-108">Run the app.</span></span>
> * <span data-ttu-id="14f2e-109">Modifier une page Razor.</span><span class="sxs-lookup"><span data-stu-id="14f2e-109">Edit a Razor page.</span></span>

<span data-ttu-id="14f2e-110">À la fin du tutoriel, vous disposerez d’une application web qui fonctionne et s’exécute sur votre machine locale.</span><span class="sxs-lookup"><span data-stu-id="14f2e-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Page d’accueil d’application web](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="14f2e-112">Prérequis</span><span class="sxs-lookup"><span data-stu-id="14f2e-112">Prerequisites</span></span>

* [<span data-ttu-id="14f2e-113">SDK .NET Core 2.2</span><span class="sxs-lookup"><span data-stu-id="14f2e-113">.NET Core 2.2 SDK</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a><span data-ttu-id="14f2e-114">Créer un projet application web</span><span class="sxs-lookup"><span data-stu-id="14f2e-114">Create a web app project</span></span>

<span data-ttu-id="14f2e-115">Ouvrez un interpréteur de commandes, puis entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="14f2e-115">Open a command shell, and enter the following command:</span></span>

```console
dotnet new webapp -o aspnetcoreapp
```

## <a name="enable-local-https"></a><span data-ttu-id="14f2e-116">Activer HTTPS localement</span><span class="sxs-lookup"><span data-stu-id="14f2e-116">Enable local HTTPS</span></span>

<span data-ttu-id="14f2e-117">Approuvez le certificat de développement HTTPS :</span><span class="sxs-lookup"><span data-stu-id="14f2e-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="14f2e-118">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="14f2e-118">Windows</span></span>](#tab/windows)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="14f2e-119">La commande précédente affiche la boîte de dialogue suivante :</span><span class="sxs-lookup"><span data-stu-id="14f2e-119">The preceding command displays the following dialog:</span></span>

![Boîte de dialogue Avertissement de sécurité](_static/cert.png)

<span data-ttu-id="14f2e-121">Sélectionnez **Oui** si vous acceptez d’approuver le certificat de développement.</span><span class="sxs-lookup"><span data-stu-id="14f2e-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="14f2e-122">macOS</span><span class="sxs-lookup"><span data-stu-id="14f2e-122">macOS</span></span>](#tab/macos)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="14f2e-123">La commande précédente affiche le message suivant :</span><span class="sxs-lookup"><span data-stu-id="14f2e-123">The preceding command displays the following message:</span></span>

<span data-ttu-id="14f2e-124">*L’approbation du certificat de développement HTTPS a été demandée. Si le certificat n’a pas encore été approuvé, nous exécutons la commande suivante :* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="14f2e-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span></span>  
<span data-ttu-id="14f2e-125">\* Cette commande peut vous demander votre mot de passe afin d’installer le certificat sur le trousseau système.</span><span class="sxs-lookup"><span data-stu-id="14f2e-125">\*This command might prompt you for your password to install the certificate on the system keychain.</span></span>

<span data-ttu-id="14f2e-126">Mot de passe :\*</span><span class="sxs-lookup"><span data-stu-id="14f2e-126">Password:\*</span></span>

<span data-ttu-id="14f2e-127">Entrez votre mot de passe si vous acceptez d’approuver le certificat de développement.</span><span class="sxs-lookup"><span data-stu-id="14f2e-127">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="14f2e-128">Linux</span><span class="sxs-lookup"><span data-stu-id="14f2e-128">Linux</span></span>](#tab/linux)

<span data-ttu-id="14f2e-129">Consultez la documentation de votre distribution Linux pour savoir comment approuver le certificat de développement HTTPS.</span><span class="sxs-lookup"><span data-stu-id="14f2e-129">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

## <a name="run-the-app"></a><span data-ttu-id="14f2e-130">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="14f2e-130">Run the app</span></span>

<span data-ttu-id="14f2e-131">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="14f2e-131">Run the following commands:</span></span>

```console
cd aspnetcoreapp
dotnet run
```

<span data-ttu-id="14f2e-132">Accédez à [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="14f2e-132">Browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="14f2e-133">Cliquez sur **Accepter** pour accepter la politique de confidentialité et de cookies.</span><span class="sxs-lookup"><span data-stu-id="14f2e-133">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="14f2e-134">Cette application ne conserve pas les informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="14f2e-134">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="14f2e-135">Modifier une page Razor</span><span class="sxs-lookup"><span data-stu-id="14f2e-135">Edit a Razor page</span></span>

<span data-ttu-id="14f2e-136">Ouvrez *Pages/Index.cshtml* et modifiez la page avec le balisage mis en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="14f2e-136">Open *Pages/Index.cshtml* and modify the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="14f2e-137">Accédez à [https://localhost:5001](https://localhost:5001) et vérifiez que les changements sont affichés.</span><span class="sxs-lookup"><span data-stu-id="14f2e-137">Browse to [https://localhost:5001](https://localhost:5001), and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="14f2e-138">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="14f2e-138">Next steps</span></span>

<span data-ttu-id="14f2e-139">Dans ce didacticiel, vous avez appris à :</span><span class="sxs-lookup"><span data-stu-id="14f2e-139">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="14f2e-140">Créer un projet application web.</span><span class="sxs-lookup"><span data-stu-id="14f2e-140">Create a web app project.</span></span>
> * <span data-ttu-id="14f2e-141">Activer HTTPS localement.</span><span class="sxs-lookup"><span data-stu-id="14f2e-141">Enable local HTTPS.</span></span>
> * <span data-ttu-id="14f2e-142">Exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="14f2e-142">Run the project.</span></span>
> * <span data-ttu-id="14f2e-143">Apportez la modification souhaitée.</span><span class="sxs-lookup"><span data-stu-id="14f2e-143">Make a change.</span></span>

<span data-ttu-id="14f2e-144">Pour en savoir plus sur ASP.NET Core, consultez la présentation :</span><span class="sxs-lookup"><span data-stu-id="14f2e-144">To learn more about ASP.NET Core, see the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index>
