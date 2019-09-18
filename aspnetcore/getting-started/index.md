---
title: Bien démarrer avec ASP.NET Core
author: rick-anderson
description: Bref tutoriel qui crée et exécute une application Hello World de base à l’aide d’ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/15/2019
uid: getting-started
ms.openlocfilehash: d1edf91f1b37ba2b69732471dc6c1f306ac5ad24
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081122"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="dc04a-103">Tutoriel : Bien démarrer avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dc04a-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="dc04a-104">Ce tutoriel montre comment utiliser l’interface de ligne de commande .NET Core pour créer et exécuter une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dc04a-104">This tutorial shows how to use the .NET Core command-line interface to create and run an ASP.NET Core web app.</span></span>

<span data-ttu-id="dc04a-105">Vous découvrirez comment effectuer les actions suivantes :</span><span class="sxs-lookup"><span data-stu-id="dc04a-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dc04a-106">Créer un projet application web.</span><span class="sxs-lookup"><span data-stu-id="dc04a-106">Create a web app project.</span></span>
> * <span data-ttu-id="dc04a-107">Approuver le certificat de développement.</span><span class="sxs-lookup"><span data-stu-id="dc04a-107">Trust the development certificate.</span></span>
> * <span data-ttu-id="dc04a-108">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="dc04a-108">Run the app.</span></span>
> * <span data-ttu-id="dc04a-109">Modifier une page Razor.</span><span class="sxs-lookup"><span data-stu-id="dc04a-109">Edit a Razor page.</span></span>

<span data-ttu-id="dc04a-110">À la fin du tutoriel, vous disposerez d’une application web qui fonctionne et s’exécute sur votre machine locale.</span><span class="sxs-lookup"><span data-stu-id="dc04a-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Page d’accueil d’application web](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="dc04a-112">Prérequis</span><span class="sxs-lookup"><span data-stu-id="dc04a-112">Prerequisites</span></span>

* [<span data-ttu-id="dc04a-113">SDK .NET Core 2.2</span><span class="sxs-lookup"><span data-stu-id="dc04a-113">.NET Core 2.2 SDK</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a><span data-ttu-id="dc04a-114">Créer un projet application web</span><span class="sxs-lookup"><span data-stu-id="dc04a-114">Create a web app project</span></span>

<span data-ttu-id="dc04a-115">Ouvrez un interpréteur de commandes, puis entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="dc04a-115">Open a command shell, and enter the following command:</span></span>

```dotnetcli
dotnet new webapp -o aspnetcoreapp
```

### <a name="trust-the-development-certificate"></a><span data-ttu-id="dc04a-116">Approuver le certificat de développement</span><span class="sxs-lookup"><span data-stu-id="dc04a-116">Trust the development certificate</span></span>

<span data-ttu-id="dc04a-117">Approuvez le certificat de développement HTTPS :</span><span class="sxs-lookup"><span data-stu-id="dc04a-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="dc04a-118">Windows</span><span class="sxs-lookup"><span data-stu-id="dc04a-118">Windows</span></span>](#tab/windows)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="dc04a-119">La commande précédente affiche la boîte de dialogue suivante :</span><span class="sxs-lookup"><span data-stu-id="dc04a-119">The preceding command displays the following dialog:</span></span>

![Boîte de dialogue Avertissement de sécurité](~/getting-started/_static/cert.png)

<span data-ttu-id="dc04a-121">Sélectionnez **Oui** si vous acceptez d’approuver le certificat de développement.</span><span class="sxs-lookup"><span data-stu-id="dc04a-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="dc04a-122">macOS</span><span class="sxs-lookup"><span data-stu-id="dc04a-122">macOS</span></span>](#tab/macos)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="dc04a-123">La commande précédente affiche le message suivant :</span><span class="sxs-lookup"><span data-stu-id="dc04a-123">The preceding command displays the following message:</span></span>

<span data-ttu-id="dc04a-124">*L’approbation du certificat de développement HTTPS a été demandée. Si le certificat n’a pas encore été approuvé, nous exécutons la commande suivante :* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span><span class="sxs-lookup"><span data-stu-id="dc04a-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>

<span data-ttu-id="dc04a-125">Cette commande risque de vous demander votre mot de passe afin d’installer le certificat sur le trousseau système.</span><span class="sxs-lookup"><span data-stu-id="dc04a-125">This command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="dc04a-126">Entrez votre mot de passe si vous acceptez d’approuver le certificat de développement.</span><span class="sxs-lookup"><span data-stu-id="dc04a-126">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="dc04a-127">Linux</span><span class="sxs-lookup"><span data-stu-id="dc04a-127">Linux</span></span>](#tab/linux)

<span data-ttu-id="dc04a-128">Pour le sous-système Windows pour Linux, consultez [Approuver le certificat HTTPS à partir du sous-système Windows pour Linux](xref:security/enforcing-ssl#wsl).</span><span class="sxs-lookup"><span data-stu-id="dc04a-128">For Windows Subsystem for Linux, see [Trust HTTPS certificate from Windows Subsystem for Linux](xref:security/enforcing-ssl#wsl).</span></span>

<span data-ttu-id="dc04a-129">Consultez la documentation de votre distribution Linux pour savoir comment approuver le certificat de développement HTTPS.</span><span class="sxs-lookup"><span data-stu-id="dc04a-129">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="dc04a-130">Pour plus d’informations, consultez [Approuver le certificat de développement ASP.NET Core HTTPS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span><span class="sxs-lookup"><span data-stu-id="dc04a-130">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="dc04a-131">Exécution de l'application</span><span class="sxs-lookup"><span data-stu-id="dc04a-131">Run the app</span></span>

<span data-ttu-id="dc04a-132">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="dc04a-132">Run the following commands:</span></span>

```dotnetcli
cd aspnetcoreapp
dotnet run
```

<span data-ttu-id="dc04a-133">Une fois que l’interface de commande indique que l’application a démarré, accédez à [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="dc04a-133">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="dc04a-134">Cliquez sur **Accepter** pour accepter la politique de confidentialité et de cookies.</span><span class="sxs-lookup"><span data-stu-id="dc04a-134">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="dc04a-135">Cette application ne conserve pas les informations personnelles.</span><span class="sxs-lookup"><span data-stu-id="dc04a-135">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="dc04a-136">Modifier une page Razor</span><span class="sxs-lookup"><span data-stu-id="dc04a-136">Edit a Razor page</span></span>

<span data-ttu-id="dc04a-137">Ouvrez *Pages/Index.cshtml* et modifiez la page avec le balisage mis en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="dc04a-137">Open *Pages/Index.cshtml* and modify the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="dc04a-138">Accédez à [https://localhost:5001](https://localhost:5001) et vérifiez que les changements sont affichés.</span><span class="sxs-lookup"><span data-stu-id="dc04a-138">Browse to [https://localhost:5001](https://localhost:5001), and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dc04a-139">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="dc04a-139">Next steps</span></span>

<span data-ttu-id="dc04a-140">Dans ce tutoriel, vous avez appris à :</span><span class="sxs-lookup"><span data-stu-id="dc04a-140">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="dc04a-141">Créer un projet application web.</span><span class="sxs-lookup"><span data-stu-id="dc04a-141">Create a web app project.</span></span>
> * <span data-ttu-id="dc04a-142">Approuver le certificat de développement.</span><span class="sxs-lookup"><span data-stu-id="dc04a-142">Trust the development certificate.</span></span>
> * <span data-ttu-id="dc04a-143">Exécuter le projet.</span><span class="sxs-lookup"><span data-stu-id="dc04a-143">Run the project.</span></span>
> * <span data-ttu-id="dc04a-144">Effectuer une modification.</span><span class="sxs-lookup"><span data-stu-id="dc04a-144">Make a change.</span></span>

<span data-ttu-id="dc04a-145">Pour en savoir plus sur ASP.NET Core, consultez le parcours d’apprentissage recommandé dans la présentation :</span><span class="sxs-lookup"><span data-stu-id="dc04a-145">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
