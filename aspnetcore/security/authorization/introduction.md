---
title: Présentation de l’autorisation dans ASP.NET Core
author: rick-anderson
description: Découvrez les principes de base de l’autorisation et le fonctionnement de l’autorisation dans les applications ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/introduction
ms.openlocfilehash: b5e60b3c256941fff5e54e1a02e077c34c535902
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660712"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a>Présentation de l’autorisation dans ASP.NET Core

<a name="security-authorization-introduction"></a>

Le terme Autorisation désigne le processus qui détermine ce qu’un utilisateur est en mesure d’effectuer. Par exemple, un utilisateur administratif est autorisé à créer une bibliothèque de documents, ajoutez les documents, modifier des documents et les supprimer. Un utilisateur non-administrateur, utilisant la bibliothèque est uniquement autorisé à lire les documents.

L’autorisation est orthogonale et indépendante de l’authentification. Toutefois, l’autorisation nécessite un mécanisme d’authentification. L’authentification est le processus qui consiste à déterminer l’identité d’un utilisateur. L’authentification peut créer une ou plusieurs identités pour l’utilisateur actuel.

Pour plus d’informations sur l’authentification dans ASP.NET Core, consultez <xref:security/authentication/index>.

## <a name="authorization-types"></a>Types d’autorisation

ASP.NET Core autorisation fournit un [rôle](xref:security/authorization/roles) simple, déclaratif et un modèle riche [basé sur des stratégies](xref:security/authorization/policies) . L’autorisation est exprimée dans les exigences et les gestionnaires évaluent les revendications d’un utilisateur par rapport aux exigences. Les vérifications impératives peuvent reposer sur des stratégies simples ou des stratégies qui évaluent l’identité de l’utilisateur et les propriétés de la ressource à laquelle l’utilisateur tente d’accéder.

## <a name="namespaces"></a>Espaces de noms

Les composants d’autorisation, y compris les attributs `AuthorizeAttribute` et `AllowAnonymousAttribute`, se trouvent dans l’espace de noms `Microsoft.AspNetCore.Authorization`.

Consultez la documentation sur l' [autorisation simple](xref:security/authorization/simple).
