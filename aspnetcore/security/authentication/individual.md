---
title: Articles basés sur des projets de ASP.NET Core créés avec des comptes d’utilisateur individuels
author: rick-anderson
description: Découvrez des articles basés sur des projets ASP.NET Core créés avec des comptes d’utilisateur individuels.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: 91c5665dc50124b3ba09bdcfbf3ba501f684c604
ms.sourcegitcommit: 9e85c2562df5e108d7933635c830297f484bb775
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73463032"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="8d225-103">Articles basés sur des projets de ASP.NET Core créés avec des comptes d’utilisateur individuels</span><span class="sxs-lookup"><span data-stu-id="8d225-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="8d225-104">ASP.NET Core identité est incluse dans les modèles de projet dans Visual Studio avec l’option « comptes d’utilisateur individuels ».</span><span class="sxs-lookup"><span data-stu-id="8d225-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="8d225-105">Les modèles d’authentification sont disponibles dans CLI .NET Core avec `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="8d225-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```dotnetcli
dotnet new mvc -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```dotnetcli
dotnet new mvc -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<span data-ttu-id="8d225-106">Consultez [ce problème GitHub](https://github.com/aspnet/AspNetCore/issues/5833) pour l’authentification de l’API Web.</span><span class="sxs-lookup"><span data-stu-id="8d225-106">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5833) for web API authentication.</span></span>

<a name="no"></a>

## <a name="no-authentication"></a><span data-ttu-id="8d225-107">Aucune authentification</span><span class="sxs-lookup"><span data-stu-id="8d225-107">No Authentication</span></span>

<span data-ttu-id="8d225-108">L’authentification est spécifiée dans le CLI .NET Core avec l’option `-au`.</span><span class="sxs-lookup"><span data-stu-id="8d225-108">Authentication is specified in the .NET Core CLI with the `-au` option.</span></span> <span data-ttu-id="8d225-109">Dans Visual Studio, la boîte de dialogue **modifier l’authentification** est disponible pour les nouvelles applications Web.</span><span class="sxs-lookup"><span data-stu-id="8d225-109">In Visual Studio, the **Change Authentication** dialog is available for new web applications.</span></span> <span data-ttu-id="8d225-110">La valeur par défaut pour les nouvelles applications Web dans Visual Studio n’est **pas une authentification**.</span><span class="sxs-lookup"><span data-stu-id="8d225-110">The default for new web apps in Visual Studio is **No Authentication**.</span></span>

<span data-ttu-id="8d225-111">Projets créés sans authentification :</span><span class="sxs-lookup"><span data-stu-id="8d225-111">Projects created with no authentication:</span></span>

* <span data-ttu-id="8d225-112">Ne contiennent pas les pages Web et l’interface utilisateur pour la connexion et la déconnexion.</span><span class="sxs-lookup"><span data-stu-id="8d225-112">Don't contain web pages and UI to sign in and sign out.</span></span>
* <span data-ttu-id="8d225-113">Ne contiennent pas de code d’authentification.</span><span class="sxs-lookup"><span data-stu-id="8d225-113">Don't contain authentication code.</span></span>

<a name="win"></a>

## <a name="windows-authentication"></a><span data-ttu-id="8d225-114">Authentification Windows</span><span class="sxs-lookup"><span data-stu-id="8d225-114">Windows Authentication</span></span>

<span data-ttu-id="8d225-115">L’authentification Windows est spécifiée pour les nouvelles applications Web dans le CLI .NET Core avec l’option `-au Windows`.</span><span class="sxs-lookup"><span data-stu-id="8d225-115">Windows Authentication is specified for new web apps in the .NET Core CLI with the `-au Windows` option.</span></span> <span data-ttu-id="8d225-116">Dans Visual Studio, la boîte de dialogue **modifier l’authentification** fournit les options **d’authentification Windows** .</span><span class="sxs-lookup"><span data-stu-id="8d225-116">In Visual Studio, the **Change Authentication** dialog provides the **Windows Authentication** options.</span></span>

<span data-ttu-id="8d225-117">Si l’authentification Windows est sélectionnée, l’application est configurée pour utiliser le [module IIS d’authentification Windows](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="8d225-117">If Windows Authentication is selected, the app is configured to use the [Windows Authentication IIS module](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="8d225-118">L’authentification Windows est destinée aux sites Web intranet.</span><span class="sxs-lookup"><span data-stu-id="8d225-118">Windows Authentication is intended for Intranet web sites.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8d225-119">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="8d225-119">Additional resources</span></span>

<span data-ttu-id="8d225-120">Les articles suivants montrent comment utiliser le code généré dans ASP.NET Core modèles qui utilisent des comptes d’utilisateur individuels :</span><span class="sxs-lookup"><span data-stu-id="8d225-120">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="8d225-121">Account confirmation and password recovery in ASP.NET Core (Confirmation de compte et récupération de mot de passe dans ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="8d225-121">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="8d225-122">Créer une application ASP.NET Core avec les données utilisateur protégées par l’autorisation</span><span class="sxs-lookup"><span data-stu-id="8d225-122">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
