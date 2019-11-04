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
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Articles basés sur des projets de ASP.NET Core créés avec des comptes d’utilisateur individuels

ASP.NET Core identité est incluse dans les modèles de projet dans Visual Studio avec l’option « comptes d’utilisateur individuels ».

Les modèles d’authentification sont disponibles dans CLI .NET Core avec `-au Individual`:

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

Consultez [ce problème GitHub](https://github.com/aspnet/AspNetCore/issues/5833) pour l’authentification de l’API Web.

<a name="no"></a>

## <a name="no-authentication"></a>Aucune authentification

L’authentification est spécifiée dans le CLI .NET Core avec l’option `-au`. Dans Visual Studio, la boîte de dialogue **modifier l’authentification** est disponible pour les nouvelles applications Web. La valeur par défaut pour les nouvelles applications Web dans Visual Studio n’est **pas une authentification**.

Projets créés sans authentification :

* Ne contiennent pas les pages Web et l’interface utilisateur pour la connexion et la déconnexion.
* Ne contiennent pas de code d’authentification.

<a name="win"></a>

## <a name="windows-authentication"></a>Authentification Windows

L’authentification Windows est spécifiée pour les nouvelles applications Web dans le CLI .NET Core avec l’option `-au Windows`. Dans Visual Studio, la boîte de dialogue **modifier l’authentification** fournit les options **d’authentification Windows** .

Si l’authentification Windows est sélectionnée, l’application est configurée pour utiliser le [module IIS d’authentification Windows](xref:host-and-deploy/iis/modules). L’authentification Windows est destinée aux sites Web intranet.

## <a name="additional-resources"></a>Ressources supplémentaires

Les articles suivants montrent comment utiliser le code généré dans ASP.NET Core modèles qui utilisent des comptes d’utilisateur individuels :

* [Account confirmation and password recovery in ASP.NET Core (Confirmation de compte et récupération de mot de passe dans ASP.NET Core)](xref:security/authentication/accconfirm)
* [Créer une application ASP.NET Core avec les données utilisateur protégées par l’autorisation](xref:security/authorization/secure-data)
