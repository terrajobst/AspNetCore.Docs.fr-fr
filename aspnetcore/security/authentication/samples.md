---
title: Exemples d’authentification pour ASP.NET Core
author: rick-anderson
description: Fournit des liens vers les exemples d’authentification dans le référentiel d’ASP.NET Core.
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: 7b3c911d60ad4737ebd12ce6f7628ad624b11658
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236673"
---
# <a name="authentication-samples-for-aspnet-core"></a>Exemples d’authentification pour ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Le [référentiel d’ASP.NET Core](https://github.com/aspnet/AspNetCore) contient les exemples d’authentification suivants dans le *AspNetCore/src/sécurité/samples* dossier :

* [Transformation des revendications](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [Authentification des cookies](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [Fournisseur de stratégie personnalisée - IAuthorizationPolicyProvider](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [Options et les schémas d’authentification dynamique](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [Revendications externes](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [En sélectionnant entre cookie et un autre schéma d’authentification en fonction de la demande](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [Restreint l’accès aux fichiers statiques](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>Exécuter les exemples

* Sélectionnez un [branche](https://github.com/aspnet/AspNetCore). Par exemple, `release/2.2`.
* Clonez ou téléchargez le [référentiel d’ASP.NET Core](https://github.com/aspnet/AspNetCore).
* Vérifiez que vous avez installé le [du SDK .NET Core](https://www.microsoft.com/net/download/all) version mise en correspondance le clone du référentiel ASP.NET Core.
* Accédez à un exemple dans *AspNetCore/src/sécurité/samples* et exécuter l’exemple avec `dotnet run`.
