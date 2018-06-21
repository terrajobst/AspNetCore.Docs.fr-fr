---
title: Protection des données dans ASP.NET Core
author: rick-anderson
description: Ce document constitue la table des matières des différentes rubriques relatives à la protection des données ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/index
ms.openlocfilehash: 1c38c587956dd99078fa4386c2f4423d784abc60
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277122"
---
# <a name="data-protection-in-aspnet-core"></a><span data-ttu-id="ebd5a-103">Protection des données dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ebd5a-103">Data Protection in ASP.NET Core</span></span>

* [<span data-ttu-id="ebd5a-104">Présentation de la protection des données</span><span class="sxs-lookup"><span data-stu-id="ebd5a-104">Introduction to data protection</span></span>](xref:security/data-protection/introduction)

* [<span data-ttu-id="ebd5a-105">Bien démarrer avec les API de protection des données</span><span class="sxs-lookup"><span data-stu-id="ebd5a-105">Get started with the Data Protection APIs</span></span>](xref:security/data-protection/using-data-protection)

* [<span data-ttu-id="ebd5a-106">API de contrôle serveur consommateur</span><span class="sxs-lookup"><span data-stu-id="ebd5a-106">Consumer APIs</span></span>](xref:security/data-protection/consumer-apis/index)

  * [<span data-ttu-id="ebd5a-107">Vue d’ensemble des API de contrôle serveur consommateur</span><span class="sxs-lookup"><span data-stu-id="ebd5a-107">Consumer APIs overview</span></span>](xref:security/data-protection/consumer-apis/overview)

  * [<span data-ttu-id="ebd5a-108">Chaînes d’objectifs</span><span class="sxs-lookup"><span data-stu-id="ebd5a-108">Purpose strings</span></span>](xref:security/data-protection/consumer-apis/purpose-strings)

  * [<span data-ttu-id="ebd5a-109">Hiérarchie d’objectifs et architecture mutualisée</span><span class="sxs-lookup"><span data-stu-id="ebd5a-109">Purpose hierarchy and multi-tenancy</span></span>](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [<span data-ttu-id="ebd5a-110">Hacher les mots de passe</span><span class="sxs-lookup"><span data-stu-id="ebd5a-110">Hash passwords</span></span>](xref:security/data-protection/consumer-apis/password-hashing)

  * [<span data-ttu-id="ebd5a-111">Limiter la durée de vie des charges utiles protégées</span><span class="sxs-lookup"><span data-stu-id="ebd5a-111">Limit the lifetime of protected payloads</span></span>](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [<span data-ttu-id="ebd5a-112">Retirer la protection des charges utiles dont les clés ont été révoquées</span><span class="sxs-lookup"><span data-stu-id="ebd5a-112">Unprotect payloads whose keys have been revoked</span></span>](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [<span data-ttu-id="ebd5a-113">Configuration</span><span class="sxs-lookup"><span data-stu-id="ebd5a-113">Configuration</span></span>](xref:security/data-protection/configuration/index)

  * [<span data-ttu-id="ebd5a-114">Configurer la protection des données ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ebd5a-114">Configure ASP.NET Core Data Protection</span></span>](xref:security/data-protection/configuration/overview)

  * [<span data-ttu-id="ebd5a-115">Paramètres par défaut</span><span class="sxs-lookup"><span data-stu-id="ebd5a-115">Default settings</span></span>](xref:security/data-protection/configuration/default-settings)

  * [<span data-ttu-id="ebd5a-116">Stratégie à l’échelle de l’ordinateur</span><span class="sxs-lookup"><span data-stu-id="ebd5a-116">Machine-wide policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)

  * [<span data-ttu-id="ebd5a-117">Scénarios non compatibles avec l’injection de dépendances</span><span class="sxs-lookup"><span data-stu-id="ebd5a-117">Non DI-aware scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)

* [<span data-ttu-id="ebd5a-118">API d’extensibilité</span><span class="sxs-lookup"><span data-stu-id="ebd5a-118">Extensibility APIs</span></span>](xref:security/data-protection/extensibility/index)

  * [<span data-ttu-id="ebd5a-119">Extensibilité du chiffrement de base</span><span class="sxs-lookup"><span data-stu-id="ebd5a-119">Core cryptography extensibility</span></span>](xref:security/data-protection/extensibility/core-crypto)

  * [<span data-ttu-id="ebd5a-120">Extensibilité de la gestion de clés</span><span class="sxs-lookup"><span data-stu-id="ebd5a-120">Key management extensibility</span></span>](xref:security/data-protection/extensibility/key-management)

  * [<span data-ttu-id="ebd5a-121">API diverses</span><span class="sxs-lookup"><span data-stu-id="ebd5a-121">Miscellaneous APIs</span></span>](xref:security/data-protection/extensibility/misc-apis)

* [<span data-ttu-id="ebd5a-122">Implémentation</span><span class="sxs-lookup"><span data-stu-id="ebd5a-122">Implementation</span></span>](xref:security/data-protection/implementation/index)

  * [<span data-ttu-id="ebd5a-123">Détails du chiffrement authentifié</span><span class="sxs-lookup"><span data-stu-id="ebd5a-123">Authenticated encryption details</span></span>](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [<span data-ttu-id="ebd5a-124">Dérivation de sous-clé et chiffrement authentifié</span><span class="sxs-lookup"><span data-stu-id="ebd5a-124">Subkey derivation and authenticated encryption</span></span>](xref:security/data-protection/implementation/subkeyderivation)

  * [<span data-ttu-id="ebd5a-125">En-têtes de contexte</span><span class="sxs-lookup"><span data-stu-id="ebd5a-125">Context headers</span></span>](xref:security/data-protection/implementation/context-headers)

  * [<span data-ttu-id="ebd5a-126">Gestion des clés</span><span class="sxs-lookup"><span data-stu-id="ebd5a-126">Key management</span></span>](xref:security/data-protection/implementation/key-management)

  * [<span data-ttu-id="ebd5a-127">Fournisseurs de stockage de clés</span><span class="sxs-lookup"><span data-stu-id="ebd5a-127">Key storage providers</span></span>](xref:security/data-protection/implementation/key-storage-providers)

  * [<span data-ttu-id="ebd5a-128">Chiffrement à clé au repos</span><span class="sxs-lookup"><span data-stu-id="ebd5a-128">Key encryption at rest</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [<span data-ttu-id="ebd5a-129">Immuabilité et paramètres des clés</span><span class="sxs-lookup"><span data-stu-id="ebd5a-129">Key immutability and settings</span></span>](xref:security/data-protection/implementation/key-immutability)

  * [<span data-ttu-id="ebd5a-130">Format de stockage des clés</span><span class="sxs-lookup"><span data-stu-id="ebd5a-130">Key storage format</span></span>](xref:security/data-protection/implementation/key-storage-format)

  * [<span data-ttu-id="ebd5a-131">Fournisseurs de protection des données éphémères</span><span class="sxs-lookup"><span data-stu-id="ebd5a-131">Ephemeral data protection providers</span></span>](xref:security/data-protection/implementation/key-storage-ephemeral)

* [<span data-ttu-id="ebd5a-132">Compatibilité</span><span class="sxs-lookup"><span data-stu-id="ebd5a-132">Compatibility</span></span>](xref:security/data-protection/compatibility/index)

  * [<span data-ttu-id="ebd5a-133">Remplacement de <machineKey> d’ASP.NET dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ebd5a-133">Replacing ASP.NET <machineKey> in ASP.NET Core</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
