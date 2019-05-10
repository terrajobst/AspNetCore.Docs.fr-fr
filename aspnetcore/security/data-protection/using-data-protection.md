---
title: Prise en main l’API de Protection des données dans ASP.NET Core
author: rick-anderson
description: Découvrez comment utiliser l’API de protection des données ASP.NET Core pour protéger et déprotéger les données dans une application.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 25bf099a3d9edd7e6e0872725cbc3707750314e6
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087639"
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a><span data-ttu-id="5e64f-103">Prise en main l’API de Protection des données dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e64f-103">Get started with the Data Protection APIs in ASP.NET Core</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="5e64f-104">À ses données la plus simple, protection se compose des étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="5e64f-104">At its simplest, protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="5e64f-105">Créer une données protecteur à partir d’un fournisseur de protection des données.</span><span class="sxs-lookup"><span data-stu-id="5e64f-105">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="5e64f-106">Appelez le `Protect` méthode avec les données que vous souhaitez protéger.</span><span class="sxs-lookup"><span data-stu-id="5e64f-106">Call the `Protect` method with the data you want to protect.</span></span>

3. <span data-ttu-id="5e64f-107">Appelez le `Unprotect` méthode avec les données que vous souhaitez reconvertir en texte brut.</span><span class="sxs-lookup"><span data-stu-id="5e64f-107">Call the `Unprotect` method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="5e64f-108">La plupart des infrastructures et modèles d’application, telles que ASP.NET Core ou SignalR, déjà configurer le système de protection des données et l’ajouter à un conteneur de service que vous pouvez accéder via l’injection de dépendances.</span><span class="sxs-lookup"><span data-stu-id="5e64f-108">Most frameworks and app models, such as ASP.NET Core or SignalR, already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="5e64f-109">L’exemple suivant montre comment configurer un conteneur de service pour l’injection de dépendances et l’inscription de la pile de protection des données, recevoir le fournisseur de protection des données via l’injection de dépendances, création d’un protecteur et les données de protection puis ôter la protection.</span><span class="sxs-lookup"><span data-stu-id="5e64f-109">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data.</span></span>

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="5e64f-110">Lorsque vous créez un protecteur, vous devez fournir un ou plusieurs [chaînes d’objectifs](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="5e64f-110">When you create a protector you must provide one or more [Purpose Strings](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="5e64f-111">Une chaîne d’usage fournit une isolation entre les consommateurs.</span><span class="sxs-lookup"><span data-stu-id="5e64f-111">A purpose string provides isolation between consumers.</span></span> <span data-ttu-id="5e64f-112">Par exemple, un protecteur créé avec une chaîne de l’objectif de « vert » ne pourriez pas ôter la protection des données fournies par un protecteur dont l’objet est « violet ».</span><span class="sxs-lookup"><span data-stu-id="5e64f-112">For example, a protector created with a purpose string of "green" wouldn't be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="5e64f-113">Instances de `IDataProtectionProvider` et `IDataProtector` soient thread-safe pour les appelants plusieurs.</span><span class="sxs-lookup"><span data-stu-id="5e64f-113">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="5e64f-114">Elles sont censées qui une fois qu’un composant obtient une référence à un `IDataProtector` via un appel à `CreateProtector`, il utilisera cette référence pour plusieurs appels à `Protect` et `Unprotect`.</span><span class="sxs-lookup"><span data-stu-id="5e64f-114">It's intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span>
>
><span data-ttu-id="5e64f-115">Un appel à `Unprotect` lèvera CryptographicException si la charge utile protégée ne peut pas être vérifiée ou déchiffrée.</span><span class="sxs-lookup"><span data-stu-id="5e64f-115">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="5e64f-116">Certains composants souhaiterez peut-être ignorer les erreurs pendant les opérations ; ôter la protection un composant qui lit des cookies d’authentification peut gérer cette erreur et traiter la demande comme s’il n’avait aucun cookie tout plutôt qu’échouer la requête directement ce dernier.</span><span class="sxs-lookup"><span data-stu-id="5e64f-116">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="5e64f-117">Les composants que ce comportement doivent spécifiquement intercepter CryptographicException au lieu d’absorber toutes les exceptions.</span><span class="sxs-lookup"><span data-stu-id="5e64f-117">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
