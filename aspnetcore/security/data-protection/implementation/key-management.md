---
title: Gestion de clés dans ASP.NET Core
author: rick-anderson
description: Découvrez les détails d’implémentation de la gestion de clés de Protection des données ASP.NET Core API.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-management
ms.openlocfilehash: 431bdf2d3076c83279b78f327ddb647f69e6e584
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892296"
---
# <a name="key-management-in-aspnet-core"></a>Gestion de clés dans ASP.NET Core

<a name="data-protection-implementation-key-management"></a>

Le système de protection de données gère automatiquement la durée de vie des clés principales utilisées pour protéger et déprotéger les charges utiles. Chaque clé peut exister dans un des quatre phases :

* A été créé : la clé existe dans le key ring, mais n’a pas encore été activée. La clé ne doit pas être utilisée pour les nouvelles opérations de protéger jusqu'à ce que suffisamment de temps écoulé que la clé a eu l’occasion d’être propagées à tous les ordinateurs qui utilisent ce porte-clés.

* Actif - la clé existe dans le key ring et doit être utilisé pour toutes les opérations de protéger de nouveau.

* Expiré - la clé a exécuté sa durée de vie naturelle et ne doit plus être utilisée pour les nouvelles opérations de protéger.

* La clé révoquée - est compromise et ne doit pas être utilisée pour les nouvelles opérations de protéger.

Clés créées, actifs et expirés peuvent utilisés pour ôter la protection des charges utiles entrants. Clés révoqués par défaut ne peuvent pas servir pour ôter la protection des charges utiles, mais le développeur d’applications peut [remplacer ce comportement](xref:security/data-protection/consumer-apis/dangerous-unprotect#data-protection-consumer-apis-dangerous-unprotect) si nécessaire.

>[!WARNING]
> Le développeur peut être tenté de supprimer une clé à partir de l’anneau de clé (par exemple, en supprimant le fichier correspondant du système de fichiers). À ce stade, toutes les données protégées par la clé est définitivement indéchiffrables, et il n’existe aucun remplacement d’urgence, comme avec les clés révoqués. Suppression d’une clé est réellement destructive comportement, et, par conséquent, le système de protection des données n’expose aucune API de première classe pour effectuer cette opération.

## <a name="default-key-selection"></a>Sélection de la clé par défaut

Lorsque le système de protection de données lit le key ring à partir du référentiel de sauvegarde, il tente de localiser une clé « default » à partir de l’anneau de clé. La clé par défaut est utilisée pour les nouvelles opérations de protéger.

L’heuristique général est que le système de protection de données choisit la clé avec la dernière date d’activation en tant que la clé par défaut. (Il existe un petit manœuvre pour permettre un décalage d’horloge de serveur à serveur.) Si la clé est arrivé à expiration ou révoquée, et si l’application n’a pas désactivé automatique de génération de clés, une nouvelle clé est générée avec l’activation immédiate par le [d’expiration et la restauration de clé](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management-expiration) stratégie ci-dessous.

La raison pour laquelle le système de protection des données génère une nouvelle clé immédiatement au lieu de revenir à une autre clé est que la nouvelle génération de clé doit être traitée comme un délai d’expiration implicite de toutes les clés qui ont été activés avant la nouvelle clé. L’idée générale est que les nouvelles clés ont été configurés avec différents algorithmes ou de mécanismes de chiffrement au repos que les anciennes clés, et le système doit préférer le retour de la configuration actuelle.

Il existe une exception. Si le développeur d’applications a [désactivé la génération automatique de la clé](xref:security/data-protection/configuration/overview#disableautomatickeygeneration), puis le système de protection des données doit choisir quelque chose comme la clé par défaut. Dans ce scénario de secours, le système choisira la clé non révoqué avec la dernière date d’activation, de préférence vers les clés qui ont eu le temps de se propager à d’autres machines du cluster. Le système de secours peut retrouver en choisissant une clé ayant expiré par défaut en conséquence. Le système de secours ne sera jamais choisir une clé révoquée en tant que la clé par défaut, et si le key ring est vide ou de chaque clé a été révoqué le système produira une erreur lors de l’initialisation.

<a name="data-protection-implementation-key-management-expiration"></a>

## <a name="key-expiration-and-rolling"></a>Expiration de la clé et le déploiement

Lorsqu’une clé est créée, elle a automatiquement donné une date d’activation de {now + 2 jours} et une date d’expiration de {now + 90 jours}. Le délai de 2 jours avant l’activation donne le temps clé pour se propager via le système. Autrement dit, il permet aux autres applications pointant vers le magasin de stockage observer la clé à leur prochaine période d’actualisation automatique, optimisant ainsi les chances que lorsque la clé en anneau actif effectue deviennent propagation à toutes les applications devront peut-être l’utiliser.

Si la clé par défaut va expirer dans les 2 jours et si le key ring n’a pas encore une clé qui est active à l’expiration de la clé par défaut, le système de protection des données persistera automatiquement une nouvelle clé dans le key ring. Cette nouvelle clé a une date d’activation de {date d’expiration de la clé par défaut} et une date d’expiration de {now + 90 jours}. Cela permet au système déployer automatiquement des clés sur une base régulière sans aucune interruption de service.

Il peut y avoir des circonstances où une clé sera créée avec l’activation immédiate. Un exemple serait lorsque l’application n’a pas exécuté pendant une période et que toutes les clés dans le key ring arrivées à expiration. Dans ce cas, la clé porte une date d’activation de {maintenant} sans le délai d’activation de 2 jours normal.

La durée de vie de clé par défaut est 90 jours, si cela est configurable dans l’exemple suivant.

```csharp
services.AddDataProtection()
       // use 14-day lifetime instead of 90-day lifetime
       .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
```

Un administrateur peut également modifier la valeur par défaut au niveau du système, même si un appel explicite à `SetDefaultKeyLifetime` remplacent toute stratégie de l’échelle du système. La durée de vie de clé par défaut ne peut pas être inférieure à 7 jours.

## <a name="automatic-key-ring-refresh"></a>Actualisation automatique de porte-clés

Quand le système de protection des données s’initialise, il lit le key ring à partir du référentiel sous-jacent et met en cache en mémoire. Ce cache autorise les opérations Protect et Unprotect continuer sans s’appuyer sur le magasin de stockage. Le système vérifie automatiquement le magasin de stockage pour les modifications environ toutes les 24 heures ou de l’expiration de la clé par défaut actuelle, le premier prévalant.

>[!WARNING]
> Les développeurs doivent très rarement (le cas échéant) à utiliser la gestion des clés API directement. Le système de protection de données effectue la gestion automatique des clés comme décrit ci-dessus.

Le système de protection des données expose une interface `IKeyManager` qui peut être utilisé pour inspecter et modifier le key ring. Le système d’injection de dépendance qui a fourni l’instance de `IDataProtectionProvider` peut également fournir une instance de `IKeyManager` votre consommation. Vous pouvez également extraire le `IKeyManager` directement à partir de la `IServiceProvider` comme dans l’exemple ci-dessous.

Toute opération qui modifie le key ring (création d’une nouvelle clé explicitement ou effectuer une révocation) invalide le cache en mémoire. L’appel suivant à `Protect` ou `Unprotect` entraîne le système de protection des données de relecture par le key ring et de recréer le cache.

L’exemple ci-dessous montre comment utiliser le `IKeyManager` interface pour inspecter et manipuler le key ring, y compris révocation existant de clés et générer une nouvelle clé manuellement.

[!code-csharp[](key-management/samples/key-management.cs)]

## <a name="key-storage"></a>Stockage de clés

Le système de protection de données a une méthode heuristique par laquelle il tente de déduire un mécanisme de chiffrement au repos et un emplacement de stockage de clé approprié automatiquement. Le mécanisme de persistance des clés est également configurable par le développeur de l’application. Les documents suivants décrivent les implémentations de l’emploi de ces mécanismes :

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>
