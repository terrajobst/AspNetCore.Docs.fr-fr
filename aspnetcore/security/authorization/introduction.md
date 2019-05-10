---
title: Introduction aux autorisations dans ASP.NET Core
author: rick-anderson
description: Découvrez les principes de base d’autorisation et de fonctionne de l’autorisation dans les applications ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/introduction
ms.openlocfilehash: 5465eb7875ebecd77b628376ef886db0ddd05025
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896976"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a>Introduction aux autorisations dans ASP.NET Core

<a name="security-authorization-introduction"></a>

Le terme Autorisation désigne le processus qui détermine ce qu’un utilisateur est en mesure d’effectuer. Par exemple, un utilisateur administratif est autorisé à créer une bibliothèque de documents, ajoutez les documents, modifier des documents et les supprimer. Un utilisateur non-administrateur, utilisant la bibliothèque est uniquement autorisé à lire les documents.

L’autorisation est orthogonale et indépendante de l’authentification. Toutefois, l’autorisation nécessite un mécanisme d’authentification. L’authentification est le processus d’évaluation qui est un utilisateur. L’authentification peut créer une ou plusieurs identités pour l’utilisateur actuel.

## <a name="authorization-types"></a>Types d’autorisation

L'autorisation de ASP.NET Core fournit un simple [rôle](xref:security/authorization/roles) déclaratif et un [modèle enrichie basée sur la stratégie](xref:security/authorization/policies). L’autorisation est exprimée dans les exigences et les gestionnaires évaluent les revendications d’un utilisateur par rapport aux exigences. Les vérifications impératives peuvent reposer sur des stratégies simples ou des stratégies qui évaluent l’identité de l’utilisateur et les propriétés de la ressource à laquelle l’utilisateur tente d’accéder.

## <a name="namespaces"></a>Espaces de noms

Les composants d’autorisation, y compris les attributs `AuthorizeAttribute` et `AllowAnonymousAttribute` se trouvent dans l'espace de noms `Microsoft.AspNetCore.Authorization`.

Consultez la documentation sur [autorisation simple](xref:security/authorization/simple).
