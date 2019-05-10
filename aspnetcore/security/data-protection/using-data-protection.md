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
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a>Prise en main l’API de Protection des données dans ASP.NET Core

<a name="security-data-protection-getting-started"></a>

À ses données la plus simple, protection se compose des étapes suivantes :

1. Créer une données protecteur à partir d’un fournisseur de protection des données.

2. Appelez le `Protect` méthode avec les données que vous souhaitez protéger.

3. Appelez le `Unprotect` méthode avec les données que vous souhaitez reconvertir en texte brut.

La plupart des infrastructures et modèles d’application, telles que ASP.NET Core ou SignalR, déjà configurer le système de protection des données et l’ajouter à un conteneur de service que vous pouvez accéder via l’injection de dépendances. L’exemple suivant montre comment configurer un conteneur de service pour l’injection de dépendances et l’inscription de la pile de protection des données, recevoir le fournisseur de protection des données via l’injection de dépendances, création d’un protecteur et les données de protection puis ôter la protection.

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Lorsque vous créez un protecteur, vous devez fournir un ou plusieurs [chaînes d’objectifs](xref:security/data-protection/consumer-apis/purpose-strings). Une chaîne d’usage fournit une isolation entre les consommateurs. Par exemple, un protecteur créé avec une chaîne de l’objectif de « vert » ne pourriez pas ôter la protection des données fournies par un protecteur dont l’objet est « violet ».

>[!TIP]
> Instances de `IDataProtectionProvider` et `IDataProtector` soient thread-safe pour les appelants plusieurs. Elles sont censées qui une fois qu’un composant obtient une référence à un `IDataProtector` via un appel à `CreateProtector`, il utilisera cette référence pour plusieurs appels à `Protect` et `Unprotect`.
>
>Un appel à `Unprotect` lèvera CryptographicException si la charge utile protégée ne peut pas être vérifiée ou déchiffrée. Certains composants souhaiterez peut-être ignorer les erreurs pendant les opérations ; ôter la protection un composant qui lit des cookies d’authentification peut gérer cette erreur et traiter la demande comme s’il n’avait aucun cookie tout plutôt qu’échouer la requête directement ce dernier. Les composants que ce comportement doivent spécifiquement intercepter CryptographicException au lieu d’absorber toutes les exceptions.
