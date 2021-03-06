---
title: Schémas de stratégie dans ASP.NET Core
author: rick-anderson
description: Les schémas de stratégie d’authentification facilitent l’utilisation d’un seul schéma d’authentification logique.
ms.author: riande
ms.date: 12/05/2019
uid: security/authentication/policyschemes
ms.openlocfilehash: f02d8e5cac20a9b60c5eddbd28253efacf682ea1
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660733"
---
# <a name="policy-schemes-in-aspnet-core"></a>Schémas de stratégie dans ASP.NET Core

Les schémas de stratégie d’authentification facilitent l’utilisation d’un seul schéma d’authentification logique. Par exemple, un modèle de stratégie peut utiliser l’authentification Google pour résoudre les problèmes et l’authentification des cookies pour tout le reste. Les schémas de stratégie d’authentification le font :

* Action d’authentification facile à transférer vers un autre schéma.
* Transférer dynamiquement en fonction de la requête.

Tous les schémas d’authentification qui utilisent des <xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions> dérivées et les [>s de\<AuthenticationHandler](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1)associées sont les suivants :

* Sont automatiquement des schémas de stratégie dans ASP.NET Core 2,1 et versions ultérieures.
* Peut être activé par le biais de la configuration des options du schéma.

[!code-csharp[sample](policyschemes/samples/AuthenticationSchemeOptions.cs?name=snippet)]

## <a name="examples"></a>Exemples

L’exemple suivant montre un modèle de niveau supérieur qui combine des schémas de niveau inférieur. L’authentification Google est utilisée pour les défis et l’authentification par cookie est utilisée pour tout le reste :

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet1)]

L’exemple suivant active la sélection dynamique de schémas pour chaque demande. Autrement dit, comment mélanger les cookies et l’authentification de l’API :

 <!-- REVIEW, missing If set in public Func<HttpContext, string> ForwardDefaultSelector -->

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet2)]
