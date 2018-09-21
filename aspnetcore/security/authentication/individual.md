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
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Articles basés sur les projets ASP.NET Core créés avec les comptes d’utilisateur individuels

ASP.NET Core Identity est inclus dans les modèles de projet dans Visual Studio avec l’option « Comptes d’utilisateur individuels ».

Les modèles d’authentification sont disponibles dans l’interface CLI .NET Core avec `-au Individual`:

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
## <a name="no-authentication"></a>Aucune authentification

L’authentification est spécifiée dans l’interface CLI .NET Core avec la `-au` option. Dans Visual Studio, le **modifier l’authentification** boîte de dialogue est disponible pour les nouvelles applications web. La valeur par défaut pour les nouvelles applications web dans Visual Studio est **aucune authentification**.

Projets créés avec aucune authentification :

* Ne contiennent pas les pages web et l’interface utilisateur pour se connecter et se déconnecter.
* Ne contiennent pas le code d’authentification.

<a name="win"></a>
## <a name="windows-authentication"></a>Authentification Windows

L’authentification Windows est spécifiée pour les nouvelles applications web dans l’interface CLI .NET Core avec la `-au Windows` option. Dans Visual Studio, le **modifier l’authentification** boîte de dialogue fournit le **l’authentification Windows** options.

Si l’authentification Windows est sélectionnée, l’application est configurée pour utiliser le [module IIS de l’authentification Windows](xref:host-and-deploy/iis/modules). L’authentification Windows est conçue pour les sites web Intranet.

## <a name="additional-resources"></a>Ressources supplémentaires

Les articles suivants vous montrent comment utiliser le code généré dans les modèles ASP.NET Core qui utilisent des comptes d’utilisateur individuels :

* [Authentification à deux facteurs avec SMS](xref:security/authentication/2fa)
* [Account confirmation and password recovery in ASP.NET Core (Confirmation de compte et récupération de mot de passe dans ASP.NET Core)](xref:security/authentication/accconfirm)
* [Créer une application ASP.NET Core avec des données utilisateur protégées par une autorisation](xref:security/authorization/secure-data)
