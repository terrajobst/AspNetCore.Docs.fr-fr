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
ms.openlocfilehash: 699def0133f53b922477ac294f70db41998945ef
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729546"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="17361-103">Articles basés sur les projets ASP.NET Core créés avec des comptes d’utilisateur individuels</span><span class="sxs-lookup"><span data-stu-id="17361-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="17361-104">Identité de ASP.NET Core est incluse dans les modèles de projet dans Visual Studio avec l’option « Comptes d’utilisateur individuels ».</span><span class="sxs-lookup"><span data-stu-id="17361-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="17361-105">Les modèles d’authentification sont disponibles dans l’interface CLI de base .NET avec `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="17361-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

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

<span data-ttu-id="17361-106">Les articles suivants montrent comment utiliser le code généré dans les modèles ASP.NET Core qui utilisent des comptes d’utilisateur individuels :</span><span class="sxs-lookup"><span data-stu-id="17361-106">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="17361-107">Authentification à deux facteurs avec SMS</span><span class="sxs-lookup"><span data-stu-id="17361-107">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="17361-108">Account confirmation and password recovery in ASP.NET Core (Confirmation de compte et récupération de mot de passe dans ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="17361-108">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="17361-109">Créer une application ASP.NET Core et de données utilisateur protégées par l’autorisation</span><span class="sxs-lookup"><span data-stu-id="17361-109">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
