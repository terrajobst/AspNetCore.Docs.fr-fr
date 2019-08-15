---
title: Exemples d’authentification pour ASP.NET Core
author: rick-anderson
description: Fournit des liens vers les exemples d’authentification dans le référentiel ASP.NET Core.
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: efa177245faceddad4eb80de9e6f6d38e1a4261c
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022413"
---
# <a name="authentication-samples-for-aspnet-core"></a>Exemples d’authentification pour ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

Le [référentiel ASP.net Core](https://github.com/aspnet/AspNetCore) contient les exemples d’authentification suivants dans le dossier *AspNetCore/SRC/Security/Samples* :

* [Transformation des revendications](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [Authentification par cookie](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [Fournisseur de stratégie personnalisée-IAuthorizationPolicyProvider](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [Options et schémas d’authentification dynamique](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [Revendications externes](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [Sélection entre un cookie et un autre schéma d’authentification en fonction de la demande](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [Restreint l’accès aux fichiers statiques](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>Exécuter les exemples

* Sélectionnez une [branche](https://github.com/aspnet/AspNetCore). Par exemple, `Tag:v3.0.0`.
* Clonez ou téléchargez le [référentiel ASP.net Core](https://github.com/aspnet/AspNetCore).
* Vérifiez que vous avez installé la version de [Kit SDK .net Core](https://www.microsoft.com/net/download/all) qui correspond au clone du référentiel ASP.net core.
* Accédez à un exemple dans *AspNetCore/SRC/Security/Samples* et exécutez l’exemple `dotnet run`avec.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Le [référentiel ASP.net Core](https://github.com/aspnet/AspNetCore) contient les exemples d’authentification suivants dans le dossier *AspNetCore/SRC/Security/Samples* :

* [Transformation des revendications](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [Authentification par cookie](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [Fournisseur de stratégie personnalisée-IAuthorizationPolicyProvider](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [Options et schémas d’authentification dynamique](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [Revendications externes](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [Sélection entre un cookie et un autre schéma d’authentification en fonction de la demande](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [Restreint l’accès aux fichiers statiques](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>Exécuter les exemples

* Sélectionnez une [branche](https://github.com/aspnet/AspNetCore). Par exemple, `release/2.2`.
* Clonez ou téléchargez le [référentiel ASP.net Core](https://github.com/aspnet/AspNetCore).
* Vérifiez que vous avez installé la version de [Kit SDK .net Core](https://www.microsoft.com/net/download/all) qui correspond au clone du référentiel ASP.net core.
* Accédez à un exemple dans *AspNetCore/SRC/Security/Samples* et exécutez l’exemple `dotnet run`avec.

::: moniker-end
