---
title: Bien démarrer avec ASP.NET Core
author: rick-anderson
description: Bref tutoriel qui crée et exécute une application Hello World de base à l’aide d’ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/22/2019
uid: getting-started
ms.openlocfilehash: 798f1ee87c05d886d8991e3f0230c8ebc6341ba8
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925105"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="08184-103">Tutoriel : Bien démarrer avec ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="08184-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="08184-104">Ce tutoriel montre comment utiliser l’interface de ligne de commande .NET Core pour créer et exécuter une application web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="08184-104">This tutorial shows how to use the .NET Core command-line interface to create and run an ASP.NET Core web app.</span></span>

<span data-ttu-id="08184-105">Vous allez apprendre à :</span><span class="sxs-lookup"><span data-stu-id="08184-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="08184-106">Créer un projet application web.</span><span class="sxs-lookup"><span data-stu-id="08184-106">Create a web app project.</span></span>
> * <span data-ttu-id="08184-107">Approuver le certificat de développement.</span><span class="sxs-lookup"><span data-stu-id="08184-107">Trust the development certificate.</span></span>
> * <span data-ttu-id="08184-108">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="08184-108">Run the app.</span></span>
> * <span data-ttu-id="08184-109">Modifier une page Razor.</span><span class="sxs-lookup"><span data-stu-id="08184-109">Edit a Razor page.</span></span>

<span data-ttu-id="08184-110">À la fin du tutoriel, vous disposerez d’une application web qui fonctionne et s’exécute sur votre machine locale.</span><span class="sxs-lookup"><span data-stu-id="08184-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Page d’accueil d’application web](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="08184-112">Prérequis</span><span class="sxs-lookup"><span data-stu-id="08184-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/3.0-SDK.md)]

## <a name="create-a-web-app-project"></a><span data-ttu-id="08184-113">Créer un projet application web</span><span class="sxs-lookup"><span data-stu-id="08184-113">Create a web app project</span></span>

<span data-ttu-id="08184-114">Ouvrez un interpréteur de commandes, puis entrez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="08184-114">Open a command shell, and enter the following command:</span></span>

```dotnetcli
dotnet new webapp -o aspnetcoreapp
```

<span data-ttu-id="08184-115">La commande précédente :</span><span class="sxs-lookup"><span data-stu-id="08184-115">The preceding command:</span></span>

* <span data-ttu-id="08184-116">Crée une application Web.</span><span class="sxs-lookup"><span data-stu-id="08184-116">Creates a new web app.</span></span>  
* <span data-ttu-id="08184-117">Le `-o aspnetcoreapp` paramètre crée un répertoire nommé *aspnetcoreapp* avec les fichiers sources de l’application.</span><span class="sxs-lookup"><span data-stu-id="08184-117">The `-o aspnetcoreapp` parameter creates a directory named *aspnetcoreapp* with the source files for the app.</span></span>

### <a name="trust-the-development-certificate"></a><span data-ttu-id="08184-118">Approuver le certificat de développement</span><span class="sxs-lookup"><span data-stu-id="08184-118">Trust the development certificate</span></span>

<span data-ttu-id="08184-119">Approuvez le certificat de développement HTTPS :</span><span class="sxs-lookup"><span data-stu-id="08184-119">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="08184-120">Windows</span><span class="sxs-lookup"><span data-stu-id="08184-120">Windows</span></span>](#tab/windows)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="08184-121">La commande précédente affiche la boîte de dialogue suivante :</span><span class="sxs-lookup"><span data-stu-id="08184-121">The preceding command displays the following dialog:</span></span>

![Boîte de dialogue Avertissement de sécurité](~/getting-started/_static/cert.png)

<span data-ttu-id="08184-123">Sélectionnez **Oui** si vous acceptez d’approuver le certificat de développement.</span><span class="sxs-lookup"><span data-stu-id="08184-123">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="08184-124">macOS</span><span class="sxs-lookup"><span data-stu-id="08184-124">macOS</span></span>](#tab/macos)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="08184-125">La commande précédente affiche le message suivant :</span><span class="sxs-lookup"><span data-stu-id="08184-125">The preceding command displays the following message:</span></span>

<span data-ttu-id="08184-126">*L’approbation du certificat de développement HTTPS a été demandée. Si le certificat n’a pas encore été approuvé, nous exécutons la commande suivante :* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span><span class="sxs-lookup"><span data-stu-id="08184-126">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>

<span data-ttu-id="08184-127">Cette commande risque de vous demander votre mot de passe afin d’installer le certificat sur le trousseau système.</span><span class="sxs-lookup"><span data-stu-id="08184-127">This command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="08184-128">Entrez votre mot de passe si vous acceptez d’approuver le certificat de développement.</span><span class="sxs-lookup"><span data-stu-id="08184-128">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="08184-129">Linux</span><span class="sxs-lookup"><span data-stu-id="08184-129">Linux</span></span>](#tab/linux)

<span data-ttu-id="08184-130">Pour le sous-système Windows pour Linux, consultez [Approuver le certificat HTTPS à partir du sous-système Windows pour Linux](xref:security/enforcing-ssl#wsl).</span><span class="sxs-lookup"><span data-stu-id="08184-130">For Windows Subsystem for Linux, see [Trust HTTPS certificate from Windows Subsystem for Linux](xref:security/enforcing-ssl#wsl).</span></span>

<span data-ttu-id="08184-131">Consultez la documentation de votre distribution Linux pour savoir comment approuver le certificat de développement HTTPS.</span><span class="sxs-lookup"><span data-stu-id="08184-131">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="08184-132">Pour plus d’informations, consultez [Approuver le certificat de développement ASP.NET Core HTTPS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span><span class="sxs-lookup"><span data-stu-id="08184-132">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="08184-133">Exécuter l'application</span><span class="sxs-lookup"><span data-stu-id="08184-133">Run the app</span></span>

<span data-ttu-id="08184-134">Exécutez les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="08184-134">Run the following commands:</span></span>

```dotnetcli
cd aspnetcoreapp
dotnet watch run
```

<span data-ttu-id="08184-135">Une fois que l’interface de commande indique que l’application a démarré, accédez à [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="08184-135">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="08184-136">Modifier une page Razor</span><span class="sxs-lookup"><span data-stu-id="08184-136">Edit a Razor page</span></span>

<span data-ttu-id="08184-137">Ouvrez *pages/index. cshtml* et modifiez et enregistrez la page avec le balisage en surbrillance suivant :</span><span class="sxs-lookup"><span data-stu-id="08184-137">Open *Pages/Index.cshtml* and modify and save the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="08184-138">Accédez à [https://localhost:5001](https://localhost:5001), actualisez la page et vérifiez que les modifications sont affichées.</span><span class="sxs-lookup"><span data-stu-id="08184-138">Browse to [https://localhost:5001](https://localhost:5001), refresh the page and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="08184-139">Étapes suivantes</span><span class="sxs-lookup"><span data-stu-id="08184-139">Next steps</span></span>

<span data-ttu-id="08184-140">Dans ce didacticiel, vous avez appris à :</span><span class="sxs-lookup"><span data-stu-id="08184-140">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="08184-141">Créer un projet application web.</span><span class="sxs-lookup"><span data-stu-id="08184-141">Create a web app project.</span></span>
> * <span data-ttu-id="08184-142">Approuver le certificat de développement.</span><span class="sxs-lookup"><span data-stu-id="08184-142">Trust the development certificate.</span></span>
> * <span data-ttu-id="08184-143">Exécuter le projet.</span><span class="sxs-lookup"><span data-stu-id="08184-143">Run the project.</span></span>
> * <span data-ttu-id="08184-144">Effectuer une modification.</span><span class="sxs-lookup"><span data-stu-id="08184-144">Make a change.</span></span>

<span data-ttu-id="08184-145">Pour en savoir plus sur ASP.NET Core, consultez le parcours d’apprentissage recommandé dans la présentation :</span><span class="sxs-lookup"><span data-stu-id="08184-145">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
