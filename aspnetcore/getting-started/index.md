---
title: Bien démarrer avec ASP.NET Core
author: rick-anderson
description: Didacticiel rapide qui crée et exécute une application Hello World simple à l’aide d’ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 7ab9f303d74786c4ac76f002d0f2c66371e78cb8
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228580"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="4113f-103">Bien démarrer avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4113f-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="4113f-104">Installez le [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="4113f-104">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="4113f-105">Créez un projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4113f-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="4113f-106">Ouvrez un interpréteur de commandes et entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="4113f-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE [](~/includes/webapp-alias-notice.md)]

3. <span data-ttu-id="4113f-107">Approuvez le certificat de développement HTTPS :</span><span class="sxs-lookup"><span data-stu-id="4113f-107">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="4113f-108">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="4113f-108">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

   <span data-ttu-id="4113f-109">La commande précédente affiche la boîte de dialogue suivante :</span><span class="sxs-lookup"><span data-stu-id="4113f-109">The preceding command displays the following dialog:</span></span>

   ![Boîte de dialogue Avertissement de sécurité](_static/cert.png)

   <span data-ttu-id="4113f-111">Sélectionnez **Oui** si vous acceptez d’approuver le certificat de développement.</span><span class="sxs-lookup"><span data-stu-id="4113f-111">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="4113f-112">macOS</span><span class="sxs-lookup"><span data-stu-id="4113f-112">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

   <span data-ttu-id="4113f-113">La commande précédente affiche le message suivant :</span><span class="sxs-lookup"><span data-stu-id="4113f-113">The preceding command displays the following message:</span></span>

   <span data-ttu-id="4113f-114">*L’approbation du certificat de développement HTTPS a été demandée. Si le certificat n’est pas déjà approuvé, nous exécutons la commande suivante :* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'` *Cette commande risque de vous demander votre mot de passe pour installer le certificat sur le trousseau système.    Mot de passe :*</span><span class="sxs-lookup"><span data-stu-id="4113f-114">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'` *This command might prompt you for your password to install the certificate on the system keychain.    Password:*</span></span>

   <span data-ttu-id="4113f-115">Entrez votre mot de passe si vous acceptez d’approuver le certificat de développement.</span><span class="sxs-lookup"><span data-stu-id="4113f-115">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="4113f-116">Linux</span><span class="sxs-lookup"><span data-stu-id="4113f-116">Linux</span></span>](#tab/linux)

   <a name="see-the-documentation-for-your-linux-distribution-on-how-to-trust-the-https-development-certificate"></a><span data-ttu-id="4113f-117">Consultez la documentation de votre distribution Linux pour savoir comment approuver le certificat de développement HTTPS</span><span class="sxs-lookup"><span data-stu-id="4113f-117">See the documentation for your Linux distribution on how to trust the HTTPS development certificate</span></span>
---

4. <span data-ttu-id="4113f-118">Exécutez l’application :</span><span class="sxs-lookup"><span data-stu-id="4113f-118">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="4113f-119">Accédez à [http://localhost:5001](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="4113f-119">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="4113f-120">Cliquez sur **Accepter** pour accepter la politique de confidentialité et de cookies.</span><span class="sxs-lookup"><span data-stu-id="4113f-120">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="4113f-121">Cette application ne conserve pas les informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="4113f-121">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="4113f-122">Ouvrez *Pages/About.cshtml* et modifiez la page avec le balisage mis en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="4113f-122">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="4113f-123">Accédez à [http://localhost:5001/About](http://localhost:5001/About) et vérifiez que les changements sont affichés.</span><span class="sxs-lookup"><span data-stu-id="4113f-123">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="4113f-124">Installez le [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="4113f-124">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="4113f-125">Créez un projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4113f-125">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="4113f-126">Ouvrez un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="4113f-126">Open a command shell.</span></span> <span data-ttu-id="4113f-127">Entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="4113f-127">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="4113f-128">Exécutez l’application avec les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="4113f-128">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="4113f-129">Accédez à [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="4113f-129">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="4113f-130">Ouvrez *Pages/About.cshtml*, puis modifiez la page de façon à afficher le message « Hello, world!</span><span class="sxs-lookup"><span data-stu-id="4113f-130">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="4113f-131">The time on the server is @DateTime.Now » :</span><span class="sxs-lookup"><span data-stu-id="4113f-131">The time on the server is @DateTime.Now":</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="4113f-132">Accédez à [http://localhost:5000/About](http://localhost:5000/About) et vérifiez les changements.</span><span class="sxs-lookup"><span data-stu-id="4113f-132">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="4113f-133">Installez le **programme d’installation du SDK** .NET Core pour le SDK 1.0.4 à partir de la [page de tous les téléchargements .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="4113f-133">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="4113f-134">Créez un dossier pour un nouveau projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4113f-134">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="4113f-135">Ouvrez un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="4113f-135">Open a command shell.</span></span> <span data-ttu-id="4113f-136">Entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="4113f-136">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="4113f-137">Si vous avez installé une version ultérieure du SDK sur votre ordinateur, créez un fichier *global.json* pour sélectionner le SDK 1.0.4.</span><span class="sxs-lookup"><span data-stu-id="4113f-137">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="4113f-138">Créez un projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4113f-138">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="4113f-139">Restaurez les packages.</span><span class="sxs-lookup"><span data-stu-id="4113f-139">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="4113f-140">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="4113f-140">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="4113f-141">La commande [dotnet run](/dotnet/core/tools/dotnet-run) commence par créer l’application, si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="4113f-141">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="4113f-142">Accédez à `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="4113f-142">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
