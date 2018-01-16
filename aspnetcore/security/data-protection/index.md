---
title: "Protection des données dans ASP.NET Core"
author: rick-anderson
description: "Ce document constitue la table des matières des différentes rubriques relatives à la protection des données ASP.NET Core."
keywords: "ASP.NET Core,protection des données"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 1f402da8-1052-4970-9835-9f9f16a02dbc
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: cbf18c6ec867fefec22980f3e3493562594ef72d
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="e29ac-104">Protection des données dans ASP.NET Core : API de contrôle serveur consommateur, configuration, API d’extensibilité et implémentation</span><span class="sxs-lookup"><span data-stu-id="e29ac-104">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="e29ac-105">Présentation de la protection des données</span><span class="sxs-lookup"><span data-stu-id="e29ac-105">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="e29ac-106">Bien démarrer avec les API de protection des données</span><span class="sxs-lookup"><span data-stu-id="e29ac-106">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="e29ac-107">API de contrôle serveur consommateur</span><span class="sxs-lookup"><span data-stu-id="e29ac-107">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="e29ac-108">Vue d’ensemble des API de contrôle serveur consommateur</span><span class="sxs-lookup"><span data-stu-id="e29ac-108">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="e29ac-109">Chaînes d’objectifs</span><span class="sxs-lookup"><span data-stu-id="e29ac-109">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="e29ac-110">Hiérarchie d’objectifs et architecture mutualisée</span><span class="sxs-lookup"><span data-stu-id="e29ac-110">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="e29ac-111">Hachage de mot de passe</span><span class="sxs-lookup"><span data-stu-id="e29ac-111">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="e29ac-112">Limitation de la durée de vie des charges utiles protégées</span><span class="sxs-lookup"><span data-stu-id="e29ac-112">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="e29ac-113">Retrait de la protection des charges utiles dont les clés ont été révoquées</span><span class="sxs-lookup"><span data-stu-id="e29ac-113">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="e29ac-114">Configuration</span><span class="sxs-lookup"><span data-stu-id="e29ac-114">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="e29ac-115">Configuration de la protection des données</span><span class="sxs-lookup"><span data-stu-id="e29ac-115">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="e29ac-116">Paramètres par défaut</span><span class="sxs-lookup"><span data-stu-id="e29ac-116">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="e29ac-117">Stratégie à l’échelle de l’ordinateur</span><span class="sxs-lookup"><span data-stu-id="e29ac-117">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="e29ac-118">Scénarios non compatibles avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="e29ac-118">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="e29ac-119">API d’extensibilité</span><span class="sxs-lookup"><span data-stu-id="e29ac-119">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="e29ac-120">Extensibilité du chiffrement de base</span><span class="sxs-lookup"><span data-stu-id="e29ac-120">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="e29ac-121">Extensibilité de la gestion de clés</span><span class="sxs-lookup"><span data-stu-id="e29ac-121">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="e29ac-122">API diverses</span><span class="sxs-lookup"><span data-stu-id="e29ac-122">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="e29ac-123">Implémentation</span><span class="sxs-lookup"><span data-stu-id="e29ac-123">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="e29ac-124">Détails du chiffrement authentifié</span><span class="sxs-lookup"><span data-stu-id="e29ac-124">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="e29ac-125">Dérivation de sous-clé et chiffrement authentifié</span><span class="sxs-lookup"><span data-stu-id="e29ac-125">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="e29ac-126">En-têtes de contexte</span><span class="sxs-lookup"><span data-stu-id="e29ac-126">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="e29ac-127">Gestion des clés</span><span class="sxs-lookup"><span data-stu-id="e29ac-127">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="e29ac-128">Fournisseurs de stockage de clés</span><span class="sxs-lookup"><span data-stu-id="e29ac-128">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="e29ac-129">Chiffrement à clé au repos</span><span class="sxs-lookup"><span data-stu-id="e29ac-129">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="e29ac-130">Immuabilité des clés et modification des paramètres</span><span class="sxs-lookup"><span data-stu-id="e29ac-130">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="e29ac-131">Format de stockage des clés</span><span class="sxs-lookup"><span data-stu-id="e29ac-131">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="e29ac-132">Fournisseurs de protection des données éphémères</span><span class="sxs-lookup"><span data-stu-id="e29ac-132">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="e29ac-133">Compatibilité</span><span class="sxs-lookup"><span data-stu-id="e29ac-133">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="e29ac-134">Partage des cookies entre les applications</span><span class="sxs-lookup"><span data-stu-id="e29ac-134">Sharing cookies between apps</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="e29ac-135">Remplacement de <machineKey> dans ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e29ac-135">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
