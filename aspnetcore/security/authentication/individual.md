---
title: Articles basés sur les projets ASP.NET Core créés avec des comptes d’utilisateur individuels
author: rick-anderson
description: Découvrez les articles basés sur les projets ASP.NET Core créés avec des comptes d’utilisateur individuels.
manager: wpickett
ms.author: riande
ms.date: 11/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/individual
ms.openlocfilehash: 6d05cd8c0ee6c9029eb9bbafc15d9b19ee7633de
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252020"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="03775-103">Articles basés sur les projets ASP.NET Core créés avec des comptes d’utilisateur individuels</span><span class="sxs-lookup"><span data-stu-id="03775-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="03775-104">Identité de ASP.NET Core est incluse dans les modèles de projet dans Visual Studio avec l’option « Comptes d’utilisateur individuels ».</span><span class="sxs-lookup"><span data-stu-id="03775-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="03775-105">Les modèles d’authentification sont disponibles dans l’interface CLI de base .NET avec `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="03775-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<span data-ttu-id="03775-107">Les articles suivants montrent comment utiliser le code généré dans les modèles ASP.NET Core qui utilisent des comptes d’utilisateur individuels :</span><span class="sxs-lookup"><span data-stu-id="03775-107">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="03775-108">Authentification à deux facteurs avec SMS</span><span class="sxs-lookup"><span data-stu-id="03775-108">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="03775-109">Account confirmation and password recovery in ASP.NET Core (Confirmation de compte et récupération de mot de passe dans ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="03775-109">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="03775-110">Créer une application ASP.NET Core et de données utilisateur protégées par l’autorisation</span><span class="sxs-lookup"><span data-stu-id="03775-110">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
