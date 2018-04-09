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
ms.openlocfilehash: 40715debb48c0a7121ce84d7843b8517b0973e74
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Articles basés sur les projets ASP.NET Core créés avec des comptes d’utilisateur individuels

Identité de ASP.NET Core est incluse dans les modèles de projet dans Visual Studio avec l’option « Comptes d’utilisateur individuels ».

Les modèles d’authentification sont disponibles dans l’interface CLI de base .NET avec `-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

Les articles suivants montrent comment utiliser le code généré dans les modèles ASP.NET Core qui utilisent des comptes d’utilisateur individuels :

* [Authentification à deux facteurs avec SMS](xref:security/authentication/2fa)
* [Account confirmation and password recovery in ASP.NET Core (Confirmation de compte et récupération de mot de passe dans ASP.NET Core)](xref:security/authentication/accconfirm)
* [Créer une application ASP.NET Core et de données utilisateur protégées par l’autorisation](xref:security/authorization/secure-data)
