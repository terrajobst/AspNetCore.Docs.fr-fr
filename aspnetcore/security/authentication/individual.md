---
title: "Articles basés sur les projets créés avec des comptes d’utilisateur individuels"
author: rick-anderson
description: "Ce document répertorie les articles basés sur les projets créés avec des comptes d’utilisateur individuels."
keywords: "ASP.NET Core, d’autorisation, IAuthorizationService"
ms.author: riande
manager: wpickett
ms.date: 11/30/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/individual
ms.openlocfilehash: 1864625e0ad6b4ec6fc2ada3fa7d93edec91b633
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/01/2017
---
# <a name="articles-based-on-projects-created-with-individual-user-accounts"></a><span data-ttu-id="70dbb-104">Articles basés sur les projets créés avec des comptes d’utilisateur individuels</span><span class="sxs-lookup"><span data-stu-id="70dbb-104">Articles based on projects created with individual user accounts</span></span>

<span data-ttu-id="70dbb-105">Identité de ASP.NET Core est incluse dans les modèles de projet dans Visual Studio avec l’option « Comptes d’utilisateur individuels ».</span><span class="sxs-lookup"><span data-stu-id="70dbb-105">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="70dbb-106">Les modèles d’authentification sont disponibles dans l’interface CLI de base .NET avec `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="70dbb-106">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

<span data-ttu-id="70dbb-107">Les articles suivants montrent comment utiliser le code généré dans les modèles ASP.NET Core qui utilisent des comptes d’utilisateur individuels :</span><span class="sxs-lookup"><span data-stu-id="70dbb-107">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="70dbb-108">Authentification à deux facteurs avec SMS</span><span class="sxs-lookup"><span data-stu-id="70dbb-108">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="70dbb-109">Account confirmation and password recovery in ASP.NET Core (Confirmation de compte et récupération de mot de passe dans ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="70dbb-109">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="70dbb-110">Créer une application ASP.NET Core et de données utilisateur protégées par l’autorisation</span><span class="sxs-lookup"><span data-stu-id="70dbb-110">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)