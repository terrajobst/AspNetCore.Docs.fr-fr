---
title: Protection des données dans ASP.NET Core
author: rick-anderson
description: Ce document constitue la table des matières des différentes rubriques relatives à la protection des données ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/index
ms.openlocfilehash: 83b5bb1e6a4942a4d3e5ec0d445fa6e5a21fb533
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="data-protection-in-aspnet-core"></a><span data-ttu-id="ce774-103">Protection des données dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ce774-103">Data Protection in ASP.NET Core</span></span>

* [<span data-ttu-id="ce774-104">Présentation de la protection des données</span><span class="sxs-lookup"><span data-stu-id="ce774-104">Introduction to data protection</span></span>](xref:security/data-protection/introduction)

* [<span data-ttu-id="ce774-105">Bien démarrer avec les API de protection des données</span><span class="sxs-lookup"><span data-stu-id="ce774-105">Get started with the Data Protection APIs</span></span>](xref:security/data-protection/using-data-protection)

* [<span data-ttu-id="ce774-106">API de contrôle serveur consommateur</span><span class="sxs-lookup"><span data-stu-id="ce774-106">Consumer APIs</span></span>](xref:security/data-protection/consumer-apis/index)

  * [<span data-ttu-id="ce774-107">Vue d’ensemble des API de contrôle serveur consommateur</span><span class="sxs-lookup"><span data-stu-id="ce774-107">Consumer APIs overview</span></span>](xref:security/data-protection/consumer-apis/overview)

  * [<span data-ttu-id="ce774-108">Chaînes d’objectifs</span><span class="sxs-lookup"><span data-stu-id="ce774-108">Purpose strings</span></span>](xref:security/data-protection/consumer-apis/purpose-strings)

  * [<span data-ttu-id="ce774-109">Hiérarchie d’objectifs et architecture mutualisée</span><span class="sxs-lookup"><span data-stu-id="ce774-109">Purpose hierarchy and multi-tenancy</span></span>](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [<span data-ttu-id="ce774-110">Hacher les mots de passe</span><span class="sxs-lookup"><span data-stu-id="ce774-110">Hash passwords</span></span>](xref:security/data-protection/consumer-apis/password-hashing)

  * [<span data-ttu-id="ce774-111">Limiter la durée de vie des charges utiles protégées</span><span class="sxs-lookup"><span data-stu-id="ce774-111">Limit the lifetime of protected payloads</span></span>](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [<span data-ttu-id="ce774-112">Retirer la protection des charges utiles dont les clés ont été révoquées</span><span class="sxs-lookup"><span data-stu-id="ce774-112">Unprotect payloads whose keys have been revoked</span></span>](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [<span data-ttu-id="ce774-113">Configuration</span><span class="sxs-lookup"><span data-stu-id="ce774-113">Configuration</span></span>](xref:security/data-protection/configuration/index)

  * [<span data-ttu-id="ce774-114">Configurer la protection des données ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ce774-114">Configure ASP.NET Core Data Protection</span></span>](xref:security/data-protection/configuration/overview)

  * [<span data-ttu-id="ce774-115">Paramètres par défaut</span><span class="sxs-lookup"><span data-stu-id="ce774-115">Default settings</span></span>](xref:security/data-protection/configuration/default-settings)

  * [<span data-ttu-id="ce774-116">Stratégie à l’échelle de l’ordinateur</span><span class="sxs-lookup"><span data-stu-id="ce774-116">Machine-wide policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)

  * [<span data-ttu-id="ce774-117">Scénarios non compatibles avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="ce774-117">Non DI-aware scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)

* [<span data-ttu-id="ce774-118">API d’extensibilité</span><span class="sxs-lookup"><span data-stu-id="ce774-118">Extensibility APIs</span></span>](xref:security/data-protection/extensibility/index)

  * [<span data-ttu-id="ce774-119">Extensibilité du chiffrement de base</span><span class="sxs-lookup"><span data-stu-id="ce774-119">Core cryptography extensibility</span></span>](xref:security/data-protection/extensibility/core-crypto)

  * [<span data-ttu-id="ce774-120">Extensibilité de la gestion de clés</span><span class="sxs-lookup"><span data-stu-id="ce774-120">Key management extensibility</span></span>](xref:security/data-protection/extensibility/key-management)

  * [<span data-ttu-id="ce774-121">API diverses</span><span class="sxs-lookup"><span data-stu-id="ce774-121">Miscellaneous APIs</span></span>](xref:security/data-protection/extensibility/misc-apis)

* [<span data-ttu-id="ce774-122">Implémentation</span><span class="sxs-lookup"><span data-stu-id="ce774-122">Implementation</span></span>](xref:security/data-protection/implementation/index)

  * [<span data-ttu-id="ce774-123">Détails du chiffrement authentifié</span><span class="sxs-lookup"><span data-stu-id="ce774-123">Authenticated encryption details</span></span>](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [<span data-ttu-id="ce774-124">Dérivation de sous-clé et chiffrement authentifié</span><span class="sxs-lookup"><span data-stu-id="ce774-124">Subkey derivation and authenticated encryption</span></span>](xref:security/data-protection/implementation/subkeyderivation)

  * [<span data-ttu-id="ce774-125">En-têtes de contexte</span><span class="sxs-lookup"><span data-stu-id="ce774-125">Context headers</span></span>](xref:security/data-protection/implementation/context-headers)

  * [<span data-ttu-id="ce774-126">Gestion des clés</span><span class="sxs-lookup"><span data-stu-id="ce774-126">Key management</span></span>](xref:security/data-protection/implementation/key-management)

  * [<span data-ttu-id="ce774-127">Fournisseurs de stockage de clés</span><span class="sxs-lookup"><span data-stu-id="ce774-127">Key storage providers</span></span>](xref:security/data-protection/implementation/key-storage-providers)

  * [<span data-ttu-id="ce774-128">Chiffrement à clé au repos</span><span class="sxs-lookup"><span data-stu-id="ce774-128">Key encryption at rest</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [<span data-ttu-id="ce774-129">Immuabilité et paramètres des clés</span><span class="sxs-lookup"><span data-stu-id="ce774-129">Key immutability and settings</span></span>](xref:security/data-protection/implementation/key-immutability)

  * [<span data-ttu-id="ce774-130">Format de stockage des clés</span><span class="sxs-lookup"><span data-stu-id="ce774-130">Key storage format</span></span>](xref:security/data-protection/implementation/key-storage-format)

  * [<span data-ttu-id="ce774-131">Fournisseurs de protection des données éphémères</span><span class="sxs-lookup"><span data-stu-id="ce774-131">Ephemeral data protection providers</span></span>](xref:security/data-protection/implementation/key-storage-ephemeral)

* [<span data-ttu-id="ce774-132">Compatibilité</span><span class="sxs-lookup"><span data-stu-id="ce774-132">Compatibility</span></span>](xref:security/data-protection/compatibility/index)

  * [<span data-ttu-id="ce774-133">Remplacement de <machineKey> d’ASP.NET dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ce774-133">Replacing ASP.NET <machineKey> in ASP.NET Core</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
