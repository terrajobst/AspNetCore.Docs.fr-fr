---
title: Dérivation de sous-clé et chiffrement authentifié dans ASP.NET Core
author: rick-anderson
description: Découvrez les détails d’implémentation de la Protection des données ASP.NET Core dérivation de sous-clé et authentifiés de chiffrement.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/subkeyderivation
ms.openlocfilehash: bbfde378755b09cd5b1217b8cf66249b9fa1d6ad
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814385"
---
# <a name="subkey-derivation-and-authenticated-encryption-in-aspnet-core"></a>Dérivation de sous-clé et chiffrement authentifié dans ASP.NET Core

<a name="data-protection-implementation-subkey-derivation"></a>

La plupart des touches dans le key ring contiendra une certaine forme d’entropie et contient des informations algorithmiques indiquant « le chiffrement en mode CBC + validation de HMAC » ou « chiffrement de GCM + validation ». Dans ce cas, nous faisons référence à l’entropie incorporé en tant que le support de gestion (ou KM) pour cette clé, et nous effectuons une fonction de dérivation de clé pour dériver les clés qui seront utilisés pour les opérations de chiffrement réelles.

> [!NOTE]
> Les clés sont abstraites, et une implémentation personnalisée ne peut-être pas se comporter comme indiqué ci-dessous. Si la clé fournit sa propre implémentation de `IAuthenticatedEncryptor` au lieu d’utiliser un de nos fabriques intégrées, le mécanisme décrit dans cette section ne s’applique plus.

<a name="data-protection-implementation-subkey-derivation-aad"></a>

## <a name="additional-authenticated-data-and-subkey-derivation"></a>Données authentifiées supplémentaires et dérivation de sous-clé

Le `IAuthenticatedEncryptor` interface sert d’interface de base pour toutes les opérations de chiffrement authentifié. Son `Encrypt` méthode accepte deux mémoires tampons : texte en clair et additionalAuthenticatedData (AAD). Le flux de contenu de texte en clair inchangé de l’appel à `IDataProtector.Protect`, mais AAD est généré par le système et se compose de trois composants :

1. L’en-tête magique de 32 bits 09 F0 C9 F0 qui identifie cette version du système de protection des données.

2. L’id de clé de 128 bits.

3. Une chaîne de longueur variable formée à partir de la chaîne d’usage qui a créé le `IDataProtector` qui effectue cette opération.

Car AAD est unique pour le tuple de tous les trois composants, nous pouvons l’utiliser pour dériver les nouvelles clés à partir du Gestionnaire de clés au lieu d’utiliser le Gestionnaire de clés lui-même dans tous les de nos opérations de chiffrement. Pour chaque appel à `IAuthenticatedEncryptor.Encrypt`, le processus de dérivation de clé suivant a lieu :

( K_E, K_H ) = SP800_108_CTR_HMACSHA512(K_M, AAD, contextHeader || keyModifier)

Ici, nous appelons NIST SP800-108 KDF en Mode de compteur (consultez [NIST SP800-108](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-108.pdf), s. 5.1) avec les paramètres suivants :

* Clé de dérivation de clé (KDK) = K_M

* PRF = HMACSHA512

* label = additionalAuthenticatedData

* context = contextHeader || keyModifier

L’en-tête de contexte de longueur variable, constitue essentiellement une empreinte numérique des algorithmes pour lequel nous allons dérivation K_E et K_H. Le modificateur de clé est une chaîne de 128 bits générée de manière aléatoire pour chaque appel à `Encrypt` et permet de garantir avec surcharger la probabilité que KE et KH sont uniques pour cette opération de chiffrement d’authentification spécifique, même si toute autre entrée au KDF est constante.

Pour le chiffrement en mode CBC + des opérations de validation HMAC, | K_E | est la longueur de la clé de chiffrement par bloc symétriques, et | K_H | est la taille de la synthèse de la routine HMAC. Pour le chiffrement de GCM + des opérations de validation, | K_H | = 0.

## <a name="cbc-mode-encryption--hmac-validation"></a>Le chiffrement en mode CBC + validation de HMAC

Une fois K_E est généré par le biais du mécanisme ci-dessus, nous générer un vecteur d’initialisation aléatoire et exécuter l’algorithme de chiffrement symétrique pour chiffrer le texte en clair. Le vecteur d’initialisation et le texte chiffré sont ensuite exécutées via la routine HMAC initialisée avec la clé K_H pour produire le Mac. Ce processus et la valeur de retour est représenté sous forme graphique ci-dessous.

![Retour et les processus en mode CBC](subkeyderivation/_static/cbcprocess.png)

*sortie : = keyModifier || vecteur d’initialisation || E_cbc (données K_E, iv) || HMAC (K_H, iv || E_cbc (données K_E, iv,))*

> [!NOTE]
> Le `IDataProtector.Protect` implémentation sera [ajouter l’en-tête magique et un id de clé](xref:security/data-protection/implementation/authenticated-encryption-details) à sortie avant de le renvoyer à l’appelant. Étant donné que l’en-tête magique et id de clé sont implicitement dans le cadre de [AAD](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation-aad), et étant donné que le modificateur de clé est acheminé comme entrée au KDF, cela signifie que chaque octet de la charge utile de retourné finale est authentifié par l’ordinateur Mac.

## <a name="galoiscounter-mode-encryption--validation"></a>Le chiffrement en Mode de compteur/GALOIS + validation

Une fois K_E est généré par le biais du mécanisme ci-dessus, nous générer une valeur à usage unique à 96 bits aléatoire et exécuter l’algorithme de chiffrement symétrique pour chiffrer le texte brut et génère la balise d’authentification de 128 bits.

![Retour et les processus en mode GCM](subkeyderivation/_static/galoisprocess.png)

*sortie : = keyModifier || valeur à usage unique || E_gcm (K_E, valeur à usage unique, les données) || authTag*

> [!NOTE]
> Bien que GCM prend nativement en charge le concept d’AAD, nous sommes toujours à l’alimentation AAD uniquement le KDF d’origine, qui optent pour transmettre une chaîne vide dans GCM pour son paramètre AAD. La raison à cela est double. Tout d’abord, [pour prendre en charge d’agilité](xref:security/data-protection/implementation/context-headers#data-protection-implementation-context-headers) nous souhaitons jamais utiliser K_M directement en tant que la clé de chiffrement. En outre, GCM impose des exigences très strictes de l’unicité sur ses entrées. La probabilité que la routine de chiffrement GCM n’est jamais appelée sur deux ou plus distincts définit des données d’entrée avec le même (clé, valeur à usage unique) paire ne doit pas dépasser 2 ^ 32. Si nous K_E résoudre le problème, nous ne pouvons pas effectuer plus de 2 ^ 32 opérations de chiffrement avant de nous afoul exécuter of le 2 ^ limiter -32. Cela peut sembler un très grand nombre d’opérations, mais un serveur web de fort trafic peut passer par 4 milliards de requêtes dans des jours simples, bien au sein de la durée de vie normale pour ces clés. En garantissant la conformité de 2 ^ limite de la probabilité de-32, que nous continuons d’utiliser un modificateur de clé de 128 bits et les 96 bits valeur à usage unique, qui étend radicalement le nombre d’opérations utilisable pour n’importe quel K_M donné. Par souci de simplicité de conception, nous partageons le chemin d’accès du code KDF entre les opérations CBC et GCM, et AAD est déjà prises en compte dans le KDF il est inutile de transférer à la routine GCM.
