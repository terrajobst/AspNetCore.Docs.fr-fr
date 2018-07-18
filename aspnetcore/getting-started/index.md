---
title: Bien démarrer avec ASP.NET Core
author: rick-anderson
description: Didacticiel rapide qui crée et exécute une application Hello World simple à l’aide d’ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 22e9c982921cc03d89506e18ff99bf481027dda6
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/18/2018
ms.locfileid: "38216211"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="66b2b-103">Bien démarrer avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="66b2b-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="66b2b-104">Installez le [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="66b2b-104">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="66b2b-105">Créez un projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="66b2b-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="66b2b-106">Ouvrez un interpréteur de commandes et entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="66b2b-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    <span data-ttu-id="66b2b-107">[!INCLUDE [](~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]</span><span class="sxs-lookup"><span data-stu-id="66b2b-107">[!INCLUDE [](~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]</span></span>

3. <span data-ttu-id="66b2b-108">Approuvez le certificat de développement HTTPS :</span><span class="sxs-lookup"><span data-stu-id="66b2b-108">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="66b2b-109">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="66b2b-109">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[<span data-ttu-id="66b2b-110">macOS</span><span class="sxs-lookup"><span data-stu-id="66b2b-110">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[<span data-ttu-id="66b2b-111">Linux</span><span class="sxs-lookup"><span data-stu-id="66b2b-111">Linux</span></span>](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. <span data-ttu-id="66b2b-112">Exécutez l’application :</span><span class="sxs-lookup"><span data-stu-id="66b2b-112">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="66b2b-113">Accédez à [http://localhost:5001](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="66b2b-113">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="66b2b-114">Cliquez sur **Accepter** pour accepter la politique de confidentialité et de cookies.</span><span class="sxs-lookup"><span data-stu-id="66b2b-114">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="66b2b-115">Cette application ne conserve pas les informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="66b2b-115">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="66b2b-116">Ouvrez *Pages/About.cshtml* et modifiez la page avec le balisage mis en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="66b2b-116">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="66b2b-117">Accédez à [http://localhost:5001/About](http://localhost:5001/About) et vérifiez que les changements sont affichés.</span><span class="sxs-lookup"><span data-stu-id="66b2b-117">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="66b2b-118">Installez le [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="66b2b-118">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="66b2b-119">Créez un projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="66b2b-119">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="66b2b-120">Ouvrez un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="66b2b-120">Open a command shell.</span></span> <span data-ttu-id="66b2b-121">Entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="66b2b-121">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="66b2b-122">Exécutez l’application avec les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="66b2b-122">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="66b2b-123">Accédez à [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="66b2b-123">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="66b2b-124">Ouvrez *Pages/About.cshtml*, puis modifiez la page de façon à afficher le message « Hello, world!</span><span class="sxs-lookup"><span data-stu-id="66b2b-124">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="66b2b-125">The time on the server is @DateTime.Now » :</span><span class="sxs-lookup"><span data-stu-id="66b2b-125">The time on the server is @DateTime.Now":</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="66b2b-126">Accédez à [http://localhost:5000/About](http://localhost:5000/About) et vérifiez les changements.</span><span class="sxs-lookup"><span data-stu-id="66b2b-126">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="66b2b-127">Installez le **programme d’installation du SDK** .NET Core pour le SDK 1.0.4 à partir de la [page de tous les téléchargements .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="66b2b-127">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="66b2b-128">Créez un dossier pour un nouveau projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="66b2b-128">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="66b2b-129">Ouvrez un interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="66b2b-129">Open a command shell.</span></span> <span data-ttu-id="66b2b-130">Entrez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="66b2b-130">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="66b2b-131">Si vous avez installé une version ultérieure du SDK sur votre ordinateur, créez un fichier *global.json* pour sélectionner le SDK 1.0.4.</span><span class="sxs-lookup"><span data-stu-id="66b2b-131">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="66b2b-132">Créez un projet ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="66b2b-132">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="66b2b-133">Restaurez les packages.</span><span class="sxs-lookup"><span data-stu-id="66b2b-133">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="66b2b-134">Exécutez l’application.</span><span class="sxs-lookup"><span data-stu-id="66b2b-134">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="66b2b-135">La commande [dotnet run](/dotnet/core/tools/dotnet-run) commence par créer l’application, si nécessaire.</span><span class="sxs-lookup"><span data-stu-id="66b2b-135">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="66b2b-136">Accédez à `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="66b2b-136">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
