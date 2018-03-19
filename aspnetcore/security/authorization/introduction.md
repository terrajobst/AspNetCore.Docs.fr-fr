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
# <a name="introduction"></a><span data-ttu-id="dc886-103">Introduction</span><span class="sxs-lookup"><span data-stu-id="dc886-103">Introduction</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="dc886-104">Le terme Autorisation désigne le processus qui détermine ce qu’un utilisateur est en mesure d’effectuer.</span><span class="sxs-lookup"><span data-stu-id="dc886-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="dc886-105">Par exemple, un utilisateur administratif est autorisé à créer une bibliothèque de documents, ajoutez les documents, modifier des documents et les supprimer.</span><span class="sxs-lookup"><span data-stu-id="dc886-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="dc886-106">Un utilisateur non-administrateur, utilisant la bibliothèque est uniquement autorisé à lire les documents.</span><span class="sxs-lookup"><span data-stu-id="dc886-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="dc886-107">L’autorisation est orthogonale et indépendante de l’authentification, qui est le processus d’évaluation de qui est l'utilisateur.</span><span class="sxs-lookup"><span data-stu-id="dc886-107">Authorization is orthogonal and independent from authentication, which is the process of ascertaining who a user is.</span></span> <span data-ttu-id="dc886-108">L’authentification peut créer une ou plusieurs identités pour l’utilisateur actuel.</span><span class="sxs-lookup"><span data-stu-id="dc886-108">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="dc886-109">Types d’autorisation</span><span class="sxs-lookup"><span data-stu-id="dc886-109">Authorization types</span></span>

<span data-ttu-id="dc886-110">L'autorisation de ASP.NET Core fournit un simple [rôle](roles.md) déclaratif et un [modèle enrichie basée sur la stratégie](policies.md).</span><span class="sxs-lookup"><span data-stu-id="dc886-110">ASP.NET Core authorization provides a simple, declarative [role](roles.md) and a rich [policy-based](policies.md) model.</span></span> <span data-ttu-id="dc886-111">L’autorisation est exprimée dans les exigences et les gestionnaires évaluent les revendications d’un utilisateur par rapport aux exigences.</span><span class="sxs-lookup"><span data-stu-id="dc886-111">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="dc886-112">Les vérifications impératives peuvent reposer sur des stratégies simples ou des stratégies qui évaluent l’identité de l’utilisateur et les propriétés de la ressource à laquelle l’utilisateur tente d’accéder.</span><span class="sxs-lookup"><span data-stu-id="dc886-112">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="dc886-113">Espaces de noms</span><span class="sxs-lookup"><span data-stu-id="dc886-113">Namespaces</span></span>

<span data-ttu-id="dc886-114">Les composants d’autorisation, y compris les attributs `AuthorizeAttribute` et `AllowAnonymousAttribute` se trouvent dans l'espace de noms `Microsoft.AspNetCore.Authorization`.</span><span class="sxs-lookup"><span data-stu-id="dc886-114">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="dc886-115">Consultez la documentation sur [une autorisation simple](xref:security/authorization/simple).</span><span class="sxs-lookup"><span data-stu-id="dc886-115">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
