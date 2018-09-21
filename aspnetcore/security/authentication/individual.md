---
title: Articles basés sur les projets ASP.NET Core créés avec les comptes d’utilisateur individuels
author: rick-anderson
description: Découvrir des articles basés sur les projets ASP.NET Core créés avec les comptes d’utilisateur individuels.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: ac843342ffc73632fbf9f6359c6c1a5878dcef0d
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523062"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="7e222-103">Articles basés sur les projets ASP.NET Core créés avec les comptes d’utilisateur individuels</span><span class="sxs-lookup"><span data-stu-id="7e222-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="7e222-104">ASP.NET Core Identity est inclus dans les modèles de projet dans Visual Studio avec l’option « Comptes d’utilisateur individuels ».</span><span class="sxs-lookup"><span data-stu-id="7e222-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="7e222-105">Les modèles d’authentification sont disponibles dans l’interface CLI .NET Core avec `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="7e222-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<a name="no"></a>
## <a name="no-authentication"></a><span data-ttu-id="7e222-106">Aucune authentification</span><span class="sxs-lookup"><span data-stu-id="7e222-106">No Authentication</span></span>

<span data-ttu-id="7e222-107">L’authentification est spécifiée dans l’interface CLI .NET Core avec la `-au` option.</span><span class="sxs-lookup"><span data-stu-id="7e222-107">Authentication is specified in the .NET Core CLI with the `-au` option.</span></span> <span data-ttu-id="7e222-108">Dans Visual Studio, le **modifier l’authentification** boîte de dialogue est disponible pour les nouvelles applications web.</span><span class="sxs-lookup"><span data-stu-id="7e222-108">In Visual Studio, the **Change Authentication** dialog is available for new web applications.</span></span> <span data-ttu-id="7e222-109">La valeur par défaut pour les nouvelles applications web dans Visual Studio est **aucune authentification**.</span><span class="sxs-lookup"><span data-stu-id="7e222-109">The default for new web apps in Visual Studio is **No Authentication**.</span></span>

<span data-ttu-id="7e222-110">Projets créés avec aucune authentification :</span><span class="sxs-lookup"><span data-stu-id="7e222-110">Projects created with no authentication:</span></span>

* <span data-ttu-id="7e222-111">Ne contiennent pas les pages web et l’interface utilisateur pour se connecter et se déconnecter.</span><span class="sxs-lookup"><span data-stu-id="7e222-111">Don't contain web pages and UI to sign in and sign out.</span></span>
* <span data-ttu-id="7e222-112">Ne contiennent pas le code d’authentification.</span><span class="sxs-lookup"><span data-stu-id="7e222-112">Don't contain authentication code.</span></span>

<a name="win"></a>
## <a name="windows-authentication"></a><span data-ttu-id="7e222-113">Authentification Windows</span><span class="sxs-lookup"><span data-stu-id="7e222-113">Windows Authentication</span></span>

<span data-ttu-id="7e222-114">L’authentification Windows est spécifiée pour les nouvelles applications web dans l’interface CLI .NET Core avec la `-au Windows` option.</span><span class="sxs-lookup"><span data-stu-id="7e222-114">Windows Authentication is specified for new web apps in the .NET Core CLI with the `-au Windows` option.</span></span> <span data-ttu-id="7e222-115">Dans Visual Studio, le **modifier l’authentification** boîte de dialogue fournit le **l’authentification Windows** options.</span><span class="sxs-lookup"><span data-stu-id="7e222-115">In Visual Studio, the **Change Authentication** dialog provides the **Windows Authentication** options.</span></span>

<span data-ttu-id="7e222-116">Si l’authentification Windows est sélectionnée, l’application est configurée pour utiliser le [module IIS de l’authentification Windows](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="7e222-116">If Windows Authentication is selected, the app is configured to use the [Windows Authentication IIS module](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="7e222-117">L’authentification Windows est conçue pour les sites web Intranet.</span><span class="sxs-lookup"><span data-stu-id="7e222-117">Windows Authentication is intended for Intranet web sites.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7e222-118">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="7e222-118">Additional resources</span></span>

<span data-ttu-id="7e222-119">Les articles suivants vous montrent comment utiliser le code généré dans les modèles ASP.NET Core qui utilisent des comptes d’utilisateur individuels :</span><span class="sxs-lookup"><span data-stu-id="7e222-119">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="7e222-120">Authentification à deux facteurs avec SMS</span><span class="sxs-lookup"><span data-stu-id="7e222-120">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="7e222-121">Account confirmation and password recovery in ASP.NET Core (Confirmation de compte et récupération de mot de passe dans ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="7e222-121">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="7e222-122">Créer une application ASP.NET Core avec des données utilisateur protégées par une autorisation</span><span class="sxs-lookup"><span data-stu-id="7e222-122">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
