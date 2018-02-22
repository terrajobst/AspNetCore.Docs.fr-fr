---
title: "Introduction à l’autorisation"
author: rick-anderson
description: "Ce document fournit une explication de l’autorisation de base et explique comment l’autorisation est lié à ASP.NET Core."
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
# <a name="introduction"></a><span data-ttu-id="c9ded-103">Introduction</span><span class="sxs-lookup"><span data-stu-id="c9ded-103">Introduction</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="c9ded-104">Autorisation désigne le processus qui détermine ce qu’un utilisateur est en mesure d’effectuer.</span><span class="sxs-lookup"><span data-stu-id="c9ded-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="c9ded-105">Par exemple, un utilisateur administratif est autorisé à créer une bibliothèque de documents, ajoutez les documents, modifier des documents et les supprimer.</span><span class="sxs-lookup"><span data-stu-id="c9ded-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="c9ded-106">Un utilisateur non-administrateur, utilisation de la bibliothèque est uniquement autorisé à lire les documents.</span><span class="sxs-lookup"><span data-stu-id="c9ded-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="c9ded-107">L’autorisation est orthogonales et indépendante de l’authentification, qui est le processus d’évaluation qui est un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="c9ded-107">Authorization is orthogonal and independent from authentication, which is the process of ascertaining who a user is.</span></span> <span data-ttu-id="c9ded-108">L’authentification peut créer une ou plusieurs identités pour l’utilisateur actuel.</span><span class="sxs-lookup"><span data-stu-id="c9ded-108">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="c9ded-109">Types d’autorisation</span><span class="sxs-lookup"><span data-stu-id="c9ded-109">Authorization types</span></span>

<span data-ttu-id="c9ded-110">Autorisation de ASP.NET Core fournit un simple, déclaratif [rôle](roles.md) et une riche [basée sur des stratégies](policies.md) modèle.</span><span class="sxs-lookup"><span data-stu-id="c9ded-110">ASP.NET Core authorization provides a simple, declarative [role](roles.md) and a rich [policy-based](policies.md) model.</span></span> <span data-ttu-id="c9ded-111">L’autorisation est exprimée dans la configuration requise et gestionnaires d’évaluent les revendications d’un utilisateur par rapport aux exigences.</span><span class="sxs-lookup"><span data-stu-id="c9ded-111">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="c9ded-112">Vérifications impératives peuvent reposer sur des stratégies simples ou qui évaluent l’identité de l’utilisateur et les propriétés de la ressource à laquelle l’utilisateur tente d’accéder.</span><span class="sxs-lookup"><span data-stu-id="c9ded-112">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="c9ded-113">Espaces de noms</span><span class="sxs-lookup"><span data-stu-id="c9ded-113">Namespaces</span></span>

<span data-ttu-id="c9ded-114">Les composants d’autorisation, y compris le `AuthorizeAttribute` et `AllowAnonymousAttribute` attributs, se trouvent dans le `Microsoft.AspNetCore.Authorization` espace de noms.</span><span class="sxs-lookup"><span data-stu-id="c9ded-114">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="c9ded-115">Consultez la documentation sur [une autorisation simple](xref:security/authorization/simple).</span><span class="sxs-lookup"><span data-stu-id="c9ded-115">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
