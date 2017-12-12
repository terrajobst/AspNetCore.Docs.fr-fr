---
title: "Limiter la durée de vie de charges utiles protégés"
author: rick-anderson
description: "Ce document explique comment limiter la durée de vie d’un contenu protégé à l’aide de l’API de protection des données ASP.NET Core."
keywords: "ASP.NET Core, protection des données, la durée de vie de charge utile"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 000175e2-10fc-43dd-bfc2-51e004b97b44
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 3fe2e97db118a92cf6fa003b9ce8a9dedf253136
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/10/2017
---
# <a name="limiting-the-lifetime-of-protected-payloads"></a><span data-ttu-id="ef627-104">Limiter la durée de vie de charges utiles protégés</span><span class="sxs-lookup"><span data-stu-id="ef627-104">Limiting the lifetime of protected payloads</span></span>

<span data-ttu-id="ef627-105">Il existe des scénarios où le développeur souhaite créer une charge utile protégée qui expire après un laps de temps défini.</span><span class="sxs-lookup"><span data-stu-id="ef627-105">There are scenarios where the application developer wants to create a protected payload that expires after a set period of time.</span></span> <span data-ttu-id="ef627-106">Par exemple, la charge utile protégée peut représenter un jeton de réinitialisation de mot de passe qui ne doit être valid pendant une heure.</span><span class="sxs-lookup"><span data-stu-id="ef627-106">For instance, the protected payload might represent a password reset token that should only be valid for one hour.</span></span> <span data-ttu-id="ef627-107">Il est tout à fait possible aux développeurs de créer leur propres format de charge utile qui contient une date d’expiration incorporé, et les développeurs expérimentés peuvent utile quand même, mais pour la majorité des développeurs de la gestion de ces délais d’expiration peut atteindre fastidieuse.</span><span class="sxs-lookup"><span data-stu-id="ef627-107">It is certainly possible for the developer to create their own payload format that contains an embedded expiration date, and advanced developers may wish to do this anyway, but for the majority of developers managing these expirations can grow tedious.</span></span>

<span data-ttu-id="ef627-108">Pour faciliter cette opération pour notre public de développeur, le package [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contient les API de l’utilitaire pour la création de charges utiles qui expirent automatiquement après un laps de temps défini.</span><span class="sxs-lookup"><span data-stu-id="ef627-108">To make this easier for our developer audience, the package [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contains utility APIs for creating payloads that automatically expire after a set period of time.</span></span> <span data-ttu-id="ef627-109">Ces API de blocage de le `ITimeLimitedDataProtector` type.</span><span class="sxs-lookup"><span data-stu-id="ef627-109">These APIs hang off of the `ITimeLimitedDataProtector` type.</span></span>

## <a name="api-usage"></a><span data-ttu-id="ef627-110">Utilisation de l’API</span><span class="sxs-lookup"><span data-stu-id="ef627-110">API usage</span></span>

<span data-ttu-id="ef627-111">Le `ITimeLimitedDataProtector` interface est l’interface de base pour protéger et déprotéger les charges utiles de durée limitée / automatique qui arrive à expiration.</span><span class="sxs-lookup"><span data-stu-id="ef627-111">The `ITimeLimitedDataProtector` interface is the core interface for protecting and unprotecting time-limited / self-expiring payloads.</span></span> <span data-ttu-id="ef627-112">Pour créer une instance d’un `ITimeLimitedDataProtector`, vous devez tout d’abord une instance d’une expression régulière [IDataProtector](overview.md) construit avec un objet spécifique.</span><span class="sxs-lookup"><span data-stu-id="ef627-112">To create an instance of an `ITimeLimitedDataProtector`, you'll first need an instance of a regular [IDataProtector](overview.md) constructed with a specific purpose.</span></span> <span data-ttu-id="ef627-113">Une fois la `IDataProtector` instance n’est disponible, appelez le `IDataProtector.ToTimeLimitedDataProtector` méthode d’extension pour retourner un protecteur d’avec les fonctionnalités d’expiration prédéfinie.</span><span class="sxs-lookup"><span data-stu-id="ef627-113">Once the `IDataProtector` instance is available, call the `IDataProtector.ToTimeLimitedDataProtector` extension method to get back a protector with built-in expiration capabilities.</span></span>

<span data-ttu-id="ef627-114">`ITimeLimitedDataProtector`expose les méthodes de surface et les extensions d’API suivantes :</span><span class="sxs-lookup"><span data-stu-id="ef627-114">`ITimeLimitedDataProtector` exposes the following API surface and extension methods:</span></span>

* <span data-ttu-id="ef627-115">CreateProtector (fin de chaîne) : ITimeLimitedDataProtector - cette API méthode est similaire à l’objet existant `IDataProtectionProvider.CreateProtector` car il peut être utilisé pour créer [objectif chaînes](purpose-strings.md) à partir d’un protecteur de durée limitée racine.</span><span class="sxs-lookup"><span data-stu-id="ef627-115">CreateProtector(string purpose) : ITimeLimitedDataProtector - This API is similar to the existing `IDataProtectionProvider.CreateProtector` in that it can be used to create [purpose chains](purpose-strings.md) from a root time-limited protector.</span></span>

* <span data-ttu-id="ef627-116">Protéger (byte [] texte brut, d’expiration de DateTimeOffset) : byte]</span><span class="sxs-lookup"><span data-stu-id="ef627-116">Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="ef627-117">Protéger (texte en clair de byte [], durée de vie TimeSpan) : byte]</span><span class="sxs-lookup"><span data-stu-id="ef627-117">Protect(byte[] plaintext, TimeSpan lifetime) : byte[]</span></span>

* <span data-ttu-id="ef627-118">Protéger (texte en clair de byte []) : byte]</span><span class="sxs-lookup"><span data-stu-id="ef627-118">Protect(byte[] plaintext) : byte[]</span></span>

* <span data-ttu-id="ef627-119">Protéger (texte en clair chaîne, d’expiration de DateTimeOffset) : chaîne</span><span class="sxs-lookup"><span data-stu-id="ef627-119">Protect(string plaintext, DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="ef627-120">Protéger (texte en clair de chaîne, durée de vie TimeSpan) : chaîne</span><span class="sxs-lookup"><span data-stu-id="ef627-120">Protect(string plaintext, TimeSpan lifetime) : string</span></span>

* <span data-ttu-id="ef627-121">Protéger (texte en clair de chaîne) : chaîne</span><span class="sxs-lookup"><span data-stu-id="ef627-121">Protect(string plaintext) : string</span></span>

<span data-ttu-id="ef627-122">En plus du noyau `Protect` les méthodes qui prennent uniquement le texte en clair, il existe de nouvelles surcharges qui permettent de spécifier la date d’expiration de la charge utile.</span><span class="sxs-lookup"><span data-stu-id="ef627-122">In addition to the core `Protect` methods which take only the plaintext, there are new overloads which allow specifying the payload's expiration date.</span></span> <span data-ttu-id="ef627-123">La date d’expiration peut être spécifiée comme une date absolue (via une `DateTimeOffset`) ou sous la forme d’une heure relative (à partir du système actuel time, via un `TimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="ef627-123">The expiration date can be specified as an absolute date (via a `DateTimeOffset`) or as a relative time (from the current system time, via a `TimeSpan`).</span></span> <span data-ttu-id="ef627-124">Si une surcharge qui n’accepte pas un délai d’expiration est appelée, la charge utile est censée ne jamais pour expirer.</span><span class="sxs-lookup"><span data-stu-id="ef627-124">If an overload which doesn't take an expiration is called, the payload is assumed never to expire.</span></span>

* <span data-ttu-id="ef627-125">Ôter la protection (protectedData [] octets, délai d’expiration de DateTimeOffset) : byte]</span><span class="sxs-lookup"><span data-stu-id="ef627-125">Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="ef627-126">Ôter la protection (protectedData de [] octets) : byte]</span><span class="sxs-lookup"><span data-stu-id="ef627-126">Unprotect(byte[] protectedData) : byte[]</span></span>

* <span data-ttu-id="ef627-127">Ôter la protection (chaîne protectedData, délai d’expiration de DateTimeOffset) : chaîne</span><span class="sxs-lookup"><span data-stu-id="ef627-127">Unprotect(string protectedData, out DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="ef627-128">Ôter la protection (chaîne protectedData) : chaîne</span><span class="sxs-lookup"><span data-stu-id="ef627-128">Unprotect(string protectedData) : string</span></span>

<span data-ttu-id="ef627-129">Le `Unprotect` méthodes retournent les données non protégées d’origine.</span><span class="sxs-lookup"><span data-stu-id="ef627-129">The `Unprotect` methods return the original unprotected data.</span></span> <span data-ttu-id="ef627-130">Si la charge utile n’a pas encore expiré, l’expiration absolue est retournée en tant que paramètre, ainsi que les données non protégées d’origine de sortie facultatif.</span><span class="sxs-lookup"><span data-stu-id="ef627-130">If the payload hasn't yet expired, the absolute expiration is returned as an optional out parameter along with the original unprotected data.</span></span> <span data-ttu-id="ef627-131">Si la charge utile est expirée, toutes les surcharges de la méthode Unprotect lèvera CryptographicException.</span><span class="sxs-lookup"><span data-stu-id="ef627-131">If the payload is expired, all overloads of the Unprotect method will throw CryptographicException.</span></span>

>[!WARNING]
> <span data-ttu-id="ef627-132">Il est conseillé de ne pas utiliser ces API pour protéger les charges utiles qui nécessitent la persistance à long terme ou indéfinie.</span><span class="sxs-lookup"><span data-stu-id="ef627-132">It is not advised to use these APIs to protect payloads which require long-term or indefinite persistence.</span></span> <span data-ttu-id="ef627-133">« Mes moyens pour les charges utiles protégés d’être sont définitivement irrécupérables après un mois ? »</span><span class="sxs-lookup"><span data-stu-id="ef627-133">"Can I afford for the protected payloads to be permanently unrecoverable after a month?"</span></span> <span data-ttu-id="ef627-134">peut servir de règle empirique ; Si la réponse est aucune développeurs puis n’envisagez autre API.</span><span class="sxs-lookup"><span data-stu-id="ef627-134">can serve as a good rule of thumb; if the answer is no then developers should consider alternative APIs.</span></span>

<span data-ttu-id="ef627-135">L’exemple ci-dessous utilise le [chemins de code de non-DI](../configuration/non-di-scenarios.md) pour instancier le système de protection des données.</span><span class="sxs-lookup"><span data-stu-id="ef627-135">The sample below uses the [non-DI code paths](../configuration/non-di-scenarios.md) for instantiating the data protection system.</span></span> <span data-ttu-id="ef627-136">Pour exécuter cet exemple, assurez-vous que vous avez tout d’abord ajouté une référence au package Microsoft.AspNetCore.DataProtection.Extensions.</span><span class="sxs-lookup"><span data-stu-id="ef627-136">To run this sample, ensure that you have first added a reference to the Microsoft.AspNetCore.DataProtection.Extensions package.</span></span>

[!code-csharp[Main](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
