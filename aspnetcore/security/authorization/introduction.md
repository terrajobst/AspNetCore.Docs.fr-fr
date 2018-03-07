---
title: "Introduction à l’autorisation"
author: rick-anderson
description: "Ce document fournit une explication de l’autorisation de base et explique comment l’autorisation s'integre à ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/introduction
ms.openlocfilehash: 3fef6d38672af8871c04b65834789a39a7df8487
ms.sourcegitcommit: d43c84c4c80527c85e49d53691b293669557a79d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/20/2018
---
# <a name="introduction"></a>Introduction

<a name="security-authorization-introduction"></a>

Le terme Autorisation désigne le processus qui détermine ce qu’un utilisateur est en mesure d’effectuer. Par exemple, un utilisateur administrateur est autorisé à créer une bibliothèque de documents, à ajouter, modifier et supprimer des documents. Un utilisateur non-administrateur, utilisant la bibliothèque est uniquement autorisé à lire les documents.

L’autorisation est orthogonale et indépendante de l’authentification, qui est le processus d’évaluation de qui est l'utilisateur. L’authentification peut créer une ou plusieurs identités pour l’utilisateur actuel.

## <a name="authorization-types"></a>Types d’autorisation

Autorisation de ASP.NET Core fournit un simple, déclaratif [rôle](roles.md) et une riche [basée sur des stratégies](policies.md) modèle. L’autorisation est exprimée dans la configuration requise et gestionnaires d’évaluent les revendications d’un utilisateur par rapport aux exigences. Vérifications impératives peuvent reposer sur des stratégies simples ou qui évaluent l’identité de l’utilisateur et les propriétés de la ressource à laquelle l’utilisateur tente d’accéder.

## <a name="namespaces"></a>Espaces de noms

Les composants d’autorisation, y compris le `AuthorizeAttribute` et `AllowAnonymousAttribute` attributs, se trouvent dans le `Microsoft.AspNetCore.Authorization` espace de noms.

Consultez la documentation sur [une autorisation simple](xref:security/authorization/simple).
