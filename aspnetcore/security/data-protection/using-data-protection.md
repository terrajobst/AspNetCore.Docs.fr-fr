---
title: "Prise en main de l’API de Protection des données"
author: rick-anderson
description: "Ce document explique comment utiliser l’API de protection des données ASP.NET Core pour protéger et déprotéger les données dans une application."
keywords: "ASP.NET Core, protection des données"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 39b7a73c-29d4-4137-b311-49529adcf3cb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 535bfaf2077cda91c27e7d0d68b9804e8596070e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-the-data-protection-apis"></a><span data-ttu-id="0503e-104">Prise en main de l’API de Protection des données</span><span class="sxs-lookup"><span data-stu-id="0503e-104">Getting Started with the Data Protection APIs</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="0503e-105">À ses données la plus simple, la protection se compose des étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="0503e-105">At its simplest, protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="0503e-106">Créer un données protecteur à partir d’un fournisseur de protection des données.</span><span class="sxs-lookup"><span data-stu-id="0503e-106">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="0503e-107">Appelez le `Protect` méthode avec les données que vous souhaitez protéger.</span><span class="sxs-lookup"><span data-stu-id="0503e-107">Call the `Protect` method with the data you want to protect.</span></span>

3. <span data-ttu-id="0503e-108">Appelez le `Unprotect` méthode avec les données que vous souhaitez reconvertir en texte brut.</span><span class="sxs-lookup"><span data-stu-id="0503e-108">Call the `Unprotect` method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="0503e-109">La plupart des infrastructures et des modèles d’application, telles que ASP.NET ou SignalR, déjà configurer le système de protection des données et l’ajouter à un conteneur de service que vous pouvez accéder via l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="0503e-109">Most frameworks and app models, such as ASP.NET or SignalR, already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="0503e-110">L’exemple suivant montre comment configurer un conteneur de service pour l’injection de dépendance et l’inscription de la pile de protection des données, recevoir le fournisseur de protection des données via DI, création d’un protecteur et la protection puis ôter la protection des données</span><span class="sxs-lookup"><span data-stu-id="0503e-110">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data</span></span>

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="0503e-111">Lorsque vous créez un logiciel de protection, vous devez fournir un ou plusieurs [objectif chaînes](consumer-apis/purpose-strings.md).</span><span class="sxs-lookup"><span data-stu-id="0503e-111">When you create a protector you must provide one or more [Purpose Strings](consumer-apis/purpose-strings.md).</span></span> <span data-ttu-id="0503e-112">Une chaîne de l’objet fournit une isolation entre les consommateurs.</span><span class="sxs-lookup"><span data-stu-id="0503e-112">A purpose string provides isolation between consumers.</span></span> <span data-ttu-id="0503e-113">Par exemple, un protecteur créé avec une chaîne de l’objectif de « vert » serait pourrez peut-être pas ôter la protection des données fournies par un protecteur dans un but de « violet ».</span><span class="sxs-lookup"><span data-stu-id="0503e-113">For example, a protector created with a purpose string of "green" would not be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="0503e-114">Instances de `IDataProtectionProvider` et `IDataProtector` sont thread-safe pour les appelants plusieurs.</span><span class="sxs-lookup"><span data-stu-id="0503e-114">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="0503e-115">Il est prévu que, une fois qu’un composant obtient une référence à une `IDataProtector` via un appel à `CreateProtector`, il utilisera cette référence pour les appels multiples à `Protect` et `Unprotect`.</span><span class="sxs-lookup"><span data-stu-id="0503e-115">It is intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span>
>
><span data-ttu-id="0503e-116">Un appel à `Unprotect` lève CryptographicException si la charge utile protégée ne peut pas être vérifiée ou déchiffrée.</span><span class="sxs-lookup"><span data-stu-id="0503e-116">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="0503e-117">Certains composants peuvent souhaiter ignorer les erreurs pendant les opérations ; ôter la protection un composant qui lit les cookies d’authentification peut gérer cette erreur et traiter la demande comme s’il n’avait aucun cookie tout plutôt qu’échouer la requête ferme.</span><span class="sxs-lookup"><span data-stu-id="0503e-117">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="0503e-118">Les composants dont vous souhaitez que ce comportement doivent spécifiquement intercepter CryptographicException au lieu d’absorber toutes les exceptions.</span><span class="sxs-lookup"><span data-stu-id="0503e-118">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
