---
title: Prise en main les API de Protection des données dans ASP.NET Core
author: rick-anderson
description: Découvrez comment utiliser l’API de protection des données ASP.NET Core pour protéger et déprotéger les données dans une application.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 3a69abd2b58e02f87ccaf2317b0a8a2a7e9d7b4a
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
ms.locfileid: "30076986"
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a>Prise en main les API de Protection des données dans ASP.NET Core

<a name="security-data-protection-getting-started"></a>

À ses données la plus simple, la protection se compose des étapes suivantes :

1. Créer un données protecteur à partir d’un fournisseur de protection des données.

2. Appelez le `Protect` méthode avec les données que vous souhaitez protéger.

3. Appelez le `Unprotect` méthode avec les données que vous souhaitez reconvertir en texte brut.

La plupart des infrastructures et des modèles d’application, telles que ASP.NET ou SignalR, déjà configurer le système de protection des données et l’ajouter à un conteneur de service que vous pouvez accéder via l’injection de dépendances. L’exemple suivant montre comment configurer un conteneur de service pour l’injection de dépendance et l’inscription de la pile de protection des données, recevoir le fournisseur de protection des données via DI, création d’un protecteur et la protection puis ôter la protection des données

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Lorsque vous créez un logiciel de protection, vous devez fournir un ou plusieurs [objectif chaînes](xref:security/data-protection/consumer-apis/purpose-strings). Une chaîne de l’objet fournit une isolation entre les consommateurs. Par exemple, un protecteur créé avec une chaîne de l’objectif de « vert » semblent ne pas pouvoir ôter la protection de données fournies par un protecteur dans un but de « violet ».

>[!TIP]
> Instances de `IDataProtectionProvider` et `IDataProtector` sont thread-safe pour les appelants plusieurs. Il a prévu qui une fois qu’un composant obtient une référence à un `IDataProtector` via un appel à `CreateProtector`, il utilisera cette référence pour les appels multiples à `Protect` et `Unprotect`.
>
>Un appel à `Unprotect` lève CryptographicException si la charge utile protégée ne peut pas être vérifiée ou déchiffrée. Certains composants peuvent souhaiter ignorer les erreurs pendant les opérations ; ôter la protection un composant qui lit les cookies d’authentification peut gérer cette erreur et traiter la demande comme s’il n’avait aucun cookie tout plutôt qu’échouer la requête ferme. Les composants dont vous souhaitez que ce comportement doivent spécifiquement intercepter CryptographicException au lieu d’absorber toutes les exceptions.
