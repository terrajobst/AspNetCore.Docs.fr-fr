---
title: Vue d’ensemble des API de consommateur pour ASP.NET Core
author: rick-anderson
description: Recevoir une vue d’ensemble du consommateur diverses API disponibles dans la bibliothèque de protection de données ASP.NET Core.
ms.author: riande
ms.date: 06/11/2019
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: ff9badb55813cae0aa72d3a95dc53792332f109b
ms.sourcegitcommit: 1bb3f3f1905b4e7d4ca1b314f2ce6ee5dd8be75f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66837378"
---
# <a name="consumer-apis-overview-for-aspnet-core"></a>Vue d’ensemble des API de consommateur pour ASP.NET Core

Le `IDataProtectionProvider` et `IDataProtector` interfaces sont les interfaces de base par le biais duquel les consommateurs utilisent le système de protection des données. Ils se trouvent dans le [Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/) package.

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

L’interface du fournisseur représente la racine du système de protection des données. Il ne peut pas être directement utilisé pour protéger ou déprotéger les données. Au lieu de cela, le consommateur doit obtenir une référence à un `IDataProtector` en appelant `IDataProtectionProvider.CreateProtector(purpose)`, où le but est une chaîne qui décrit le cas d’usage prévu de consommateur. Consultez [chaînes d’objectifs](xref:security/data-protection/consumer-apis/purpose-strings) pour beaucoup plus d’informations sur l’intention de ce paramètre et comment choisir une valeur appropriée.

## <a name="idataprotector"></a>IDataProtector

L’interface de protecteur est retournée par un appel à `CreateProtector`et il cette interface à laquelle les clients peuvent utiliser pour effectuer protéger et déprotéger les opérations.

Pour protéger un élément de données, passer les données à le `Protect` (méthode). L’interface de base définit une méthode qui convertit byte [-] -> byte [], mais il existe également une surcharge (fournie comme méthode d’extension) qui convertit la chaîne -> chaîne. La sécurité offerte par les deux méthodes est identique. le développeur doit choisir quelle que soit la surcharge est plus pratique pour leurs cas d’usage. Quelle que soit la surcharge choisie, la valeur retournée par la protéger (méthode) est désormais protégée (des et répondre à tous les falsifications), et l’application peut envoyer à un client non fiable.

Pour ôter la protection d’une donnée précédemment protégé, passer les données protégées sur le `Unprotect` (méthode). (Il existe byte []-basé sur chaîne et en fonction des surcharges pour simplifier le développement.) Si la charge utile protégée a été générée par un appel antérieur à `Protect` sur cette même `IDataProtector`, le `Unprotect` méthode retournera la charge non protégée d’origine. Si la charge utile protégée a été falsifiée ou qu’il a été générée par un autre `IDataProtector`, le `Unprotect` méthode lèvera CryptographicException.

Le concept de même et différents `IDataProtector` ties sauvegarder sur le concept d’objectif. Si deux `IDataProtector` instances ont été générés à partir de la même racine `IDataProtectionProvider` mais via des chaînes d’objectifs différents dans l’appel à `IDataProtectionProvider.CreateProtector`, puis être considérées comme [protecteurs différents](xref:security/data-protection/consumer-apis/purpose-strings), et un ne pourrez pas ôter la protection charges utiles générées par l’autre.

## <a name="consuming-these-interfaces"></a>Utilisation de ces interfaces.

Pour un composant prenant en charge l’injection de dépendances, l’utilisation prévue est que le composant utilise un `IDataProtectionProvider` paramètre dans son constructeur et que le système d’injection de dépendance fournit automatiquement ce service lorsque le composant est instancié.

> [!NOTE]
> Certaines applications (telles que les applications de console ou les applications ASP.NET 4.x) ne peuvent pas être conscients de l’injection de dépendances ne pouvez pas utiliser le mécanisme décrit ici. Pour ces scénarios, consultez le [des scénarios compatibles avec l’injection de dépendances Non](xref:security/data-protection/configuration/non-di-scenarios) document pour plus d’informations sur l’obtention d’une instance d’un `IDataProtection` fournisseur sans passer par l’injection de dépendances.

L’exemple suivant montre trois concepts :

1. [Ajouter le système de protection des données](xref:security/data-protection/configuration/overview) au conteneur de service,

2. À l’aide de l’injection de dépendances pour recevoir une instance d’un `IDataProtectionProvider`, et

3. Création d’un `IDataProtector` à partir d’un `IDataProtectionProvider` et son utilisation pour protéger et déprotéger les données.

[!code-csharp[](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Le package Microsoft.AspNetCore.DataProtection.Abstractions contient une méthode d’extension `IServiceProvider.GetDataProtector` en tant que développeur pour des raisons pratiques. Comme une seule opération, il encapsule à la fois récupérer un `IDataProtectionProvider` à partir du fournisseur de services et l’appel `IDataProtectionProvider.CreateProtector`. L’exemple suivant illustre son utilisation.

[!code-csharp[](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> Instances de `IDataProtectionProvider` et `IDataProtector` soient thread-safe pour les appelants plusieurs. Elles sont censées qui une fois qu’un composant obtient une référence à un `IDataProtector` via un appel à `CreateProtector`, il utilisera cette référence pour plusieurs appels à `Protect` et `Unprotect`. Un appel à `Unprotect` lèvera CryptographicException si la charge utile protégée ne peut pas être vérifiée ou déchiffrée. Certains composants souhaiterez peut-être ignorer les erreurs pendant les opérations ; ôter la protection un composant qui lit des cookies d’authentification peut gérer cette erreur et traiter la demande comme s’il n’avait aucun cookie tout plutôt qu’échouer la requête directement ce dernier. Les composants que ce comportement doivent spécifiquement intercepter CryptographicException au lieu d’absorber toutes les exceptions.
