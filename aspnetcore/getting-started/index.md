---
title: Bien démarrer avec ASP.NET Core
author: rick-anderson
description: Tutoriel rapide qui crée et exécute une application Hello World simple à l’aide d’ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 4a5a0cc5a5dab2171ab8ef43818185a4ee91af0e
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912564"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="fe0a9-103">Tutoriel : Bien démarrer avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fe0a9-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="fe0a9-104">Ce tutoriel montre comment utiliser l’interface de ligne de commande .NET Core pour créer une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fe0a9-104">This tutorial shows how to use the .NET Core command-line interface to create an ASP.NET Core web app.</span></span> <span data-ttu-id="fe0a9-105">Vous allez apprendre à :</span><span class="sxs-lookup"><span data-stu-id="fe0a9-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fe0a9-106">Créer un projet application web.</span><span class="sxs-lookup"><span data-stu-id="fe0a9-106">Create a web app project.</span></span>
> * <span data-ttu-id="fe0a9-107">Activer HTTPS localement.</span><span class="sxs-lookup"><span data-stu-id="fe0a9-107">Enable local HTTPS.</span></span>
> * <span data-ttu-id="fe0a9-108">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="fe0a9-108">Run the app.</span></span>
> * <span data-ttu-id="fe0a9-109">Modifier une page Razor.</span><span class="sxs-lookup"><span data-stu-id="fe0a9-109">Edit a Razor page.</span></span>

<span data-ttu-id="fe0a9-110">À la fin du tutoriel, vous disposerez d’une application web qui fonctionne et s’exécute sur votre machine locale.</span><span class="sxs-lookup"><span data-stu-id="fe0a9-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Page d’accueil d’application web](_static/home-page.png)


## <a name="prerequisites"></a><span data-ttu-id="fe0a9-112">Prérequis</span><span class="sxs-lookup"><span data-stu-id="fe0a9-112">Prerequisites</span></span>

* <span data-ttu-id="fe0a9-113">Installez le [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="fe0a9-113">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

## <a name="create-a-web-app-project"></a><span data-ttu-id="fe0a9-114">Créer un projet application web</span><span class="sxs-lookup"><span data-stu-id="fe0a9-114">Create a web app project</span></span>

* <span data-ttu-id="fe0a9-115">Ouvrez un interpréteur de commandes, puis entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="fe0a9-115">Open a command shell, and enter the following command:</span></span>

   ```console
   dotnet new webapp -o aspnetcoreapp
   ```

## <a name="enable-local-https"></a><span data-ttu-id="fe0a9-116">Activer HTTPS localement</span><span class="sxs-lookup"><span data-stu-id="fe0a9-116">Enable local HTTPS</span></span>

* <span data-ttu-id="fe0a9-117">Approuvez le certificat de développement HTTPS :</span><span class="sxs-lookup"><span data-stu-id="fe0a9-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="fe0a9-118">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="fe0a9-118">Windows</span></span>](#tab/windows)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="fe0a9-119">La commande précédente affiche la boîte de dialogue suivante :</span><span class="sxs-lookup"><span data-stu-id="fe0a9-119">The preceding command displays the following dialog:</span></span>

  ![Boîte de dialogue Avertissement de sécurité](_static/cert.png)

  <span data-ttu-id="fe0a9-121">Sélectionnez **Oui** si vous acceptez d’approuver le certificat de développement.</span><span class="sxs-lookup"><span data-stu-id="fe0a9-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="fe0a9-122">macOS</span><span class="sxs-lookup"><span data-stu-id="fe0a9-122">macOS</span></span>](#tab/macos)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="fe0a9-123">La commande précédente affiche le message suivant :</span><span class="sxs-lookup"><span data-stu-id="fe0a9-123">The preceding command displays the following message:</span></span>

  <span data-ttu-id="fe0a9-124">*L’approbation du certificat de développement HTTPS a été demandée. Si le certificat n’a pas encore été approuvé, nous exécutons la commande suivante :* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="fe0a9-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span></span>  
  <span data-ttu-id="fe0a9-125">\* Cette commande peut vous demander votre mot de passe afin d’installer le certificat sur le trousseau système.</span><span class="sxs-lookup"><span data-stu-id="fe0a9-125">\*This command might prompt you for your password to install the certificate on the system keychain.</span></span>
  
  <span data-ttu-id="fe0a9-126">Mot de passe :\*</span><span class="sxs-lookup"><span data-stu-id="fe0a9-126">Password:\*</span></span>

  <span data-ttu-id="fe0a9-127">Entrez votre mot de passe si vous acceptez d’approuver le certificat de développement.</span><span class="sxs-lookup"><span data-stu-id="fe0a9-127">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="fe0a9-128">Linux</span><span class="sxs-lookup"><span data-stu-id="fe0a9-128">Linux</span></span>](#tab/linux)

  <span data-ttu-id="fe0a9-129">Consultez la documentation de votre distribution Linux pour savoir comment approuver le certificat de développement HTTPS.</span><span class="sxs-lookup"><span data-stu-id="fe0a9-129">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>
   
---

## <a name="run-the-app"></a><span data-ttu-id="fe0a9-130">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="fe0a9-130">Run the app</span></span>

* <span data-ttu-id="fe0a9-131">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="fe0a9-131">Run the following commands:</span></span>

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

* <span data-ttu-id="fe0a9-132">Accédez à [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="fe0a9-132">Browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="fe0a9-133">Cliquez sur **Accepter** pour accepter la politique de confidentialité et de cookies.</span><span class="sxs-lookup"><span data-stu-id="fe0a9-133">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="fe0a9-134">Cette application ne conserve pas les informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="fe0a9-134">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="fe0a9-135">Modifier une page Razor</span><span class="sxs-lookup"><span data-stu-id="fe0a9-135">Edit a Razor page</span></span>

* <span data-ttu-id="fe0a9-136">Ouvrez *Pages/About.cshtml* et modifiez la page avec le balisage mis en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="fe0a9-136">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

* <span data-ttu-id="fe0a9-137">Accédez à [https://localhost:5001/About](https://localhost:5001/About) et vérifiez que les changements sont affichés.</span><span class="sxs-lookup"><span data-stu-id="fe0a9-137">Browse to [https://localhost:5001/About](https://localhost:5001/About) and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fe0a9-138">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="fe0a9-138">Next steps</span></span>

<span data-ttu-id="fe0a9-139">Dans ce didacticiel, vous avez appris à :</span><span class="sxs-lookup"><span data-stu-id="fe0a9-139">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fe0a9-140">Créer un projet application web.</span><span class="sxs-lookup"><span data-stu-id="fe0a9-140">Create a web app project.</span></span>
> * <span data-ttu-id="fe0a9-141">Activer HTTPS localement.</span><span class="sxs-lookup"><span data-stu-id="fe0a9-141">Enable local HTTPS.</span></span>
> * <span data-ttu-id="fe0a9-142">Exécutez le projet.</span><span class="sxs-lookup"><span data-stu-id="fe0a9-142">Run the project.</span></span>
> * <span data-ttu-id="fe0a9-143">Apportez la modification souhaitée.</span><span class="sxs-lookup"><span data-stu-id="fe0a9-143">Make a change.</span></span>

<span data-ttu-id="fe0a9-144">Pour en savoir plus sur ASP.NET Core, consultez la présentation :</span><span class="sxs-lookup"><span data-stu-id="fe0a9-144">To learn more about ASP.NET Core, see the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index>
