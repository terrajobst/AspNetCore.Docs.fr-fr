---
title: Vue d’ensemble des API de consommateur pour ASP.NET Core
author: rick-anderson
description: Recevez une brève présentation des différentes API de consommateur disponibles dans la bibliothèque de protection des données ASP.NET Core.
ms.author: riande
ms.date: 06/11/2019
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: ff9badb55813cae0aa72d3a95dc53792332f109b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666585"
---
# <a name="consumer-apis-overview-for-aspnet-core"></a>Vue d’ensemble des API de consommateur pour ASP.NET Core

Les interfaces `IDataProtectionProvider` et `IDataProtector` sont les interfaces de base via lesquelles les consommateurs utilisent le système de protection des données. Ils se trouvent dans le package [Microsoft. AspNetCore. dataprotection. Abstracts](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/) .

## <a name="idataprotectionprovider"></a>IDataProtectionProvider

L’interface du fournisseur représente la racine du système de protection des données. Il ne peut pas être utilisé directement pour protéger ou ôter la protection des données. Au lieu de cela, le consommateur doit obtenir une référence à un `IDataProtector` en appelant `IDataProtectionProvider.CreateProtector(purpose)`, où objet est une chaîne qui décrit le cas d’usage consommateur prévu. Pour plus d’informations sur l’intention de ce paramètre et sur la manière de choisir une valeur appropriée, consultez [chaînes d’objectif](xref:security/data-protection/consumer-apis/purpose-strings) .

## <a name="idataprotector"></a>IDataProtector

L’interface de protecteur est retournée par un appel à `CreateProtector`, et c’est cette interface que les consommateurs peuvent utiliser pour effectuer des opérations de protection et d’ôter la protection.

Pour protéger un élément de données, transmettez les données à la méthode `Protect`. L’interface de base définit une méthode qui convertit Byte []-> Byte [], mais il existe également une surcharge (fournie en tant que méthode d’extension) qui convertit la chaîne > chaîne. La sécurité offerte par les deux méthodes est identique ; le développeur doit choisir la surcharge la plus pratique pour le cas d’usage. Quelle que soit la surcharge choisie, la valeur retournée par la méthode Protect est désormais protégée (chiffrée et inviolable) et l’application peut l’envoyer à un client non approuvé.

Pour ôter la protection d’un élément de données précédemment protégé, transmettez les données protégées à la méthode `Unprotect`. (Il existe des surcharges basées sur un octet [] et une chaîne pour faciliter le développement.) Si la charge utile protégée a été générée par un appel antérieur à `Protect` sur ce même `IDataProtector`, la méthode `Unprotect` retourne la charge utile non protégée d’origine. Si la charge utile protégée a été falsifiée ou a été produite par un autre `IDataProtector`, la méthode `Unprotect` lèvera CryptographicException.

Le concept des mêmes différences et des `IDataProtector` différents est lié au concept de finalité. Si deux instances de `IDataProtector` ont été générées à partir du même `IDataProtectionProvider` racine mais par le biais de chaînes d’objectif différentes dans l’appel à `IDataProtectionProvider.CreateProtector`, elles sont considérées comme des [protecteurs différents](xref:security/data-protection/consumer-apis/purpose-strings)et l’autre ne peut pas ôter la protection des charges utiles générées par l’autre.

## <a name="consuming-these-interfaces"></a>Utilisation de ces interfaces

Pour un composant de DI-Aware, l’utilisation prévue est que le composant prend un paramètre `IDataProtectionProvider` dans son constructeur et que le système DI fournit automatiquement ce service lorsque le composant est instancié.

> [!NOTE]
> Certaines applications (telles que les applications console ou les applications ASP.NET 4. x) peuvent ne pas être compatibles avec la transprise en charge. par conséquent, il n’est pas possible d’utiliser le mécanisme décrit ici. Pour ces scénarios, consultez le document relatif aux [scénarios sans di-Aware](xref:security/data-protection/configuration/non-di-scenarios) pour plus d’informations sur l’obtention d’une instance d’un fournisseur de `IDataProtection` sans passer par di.

L’exemple suivant illustre trois concepts :

1. [Ajoutez le système de protection des données](xref:security/data-protection/configuration/overview) au conteneur de service,

2. Utilisation de DI pour recevoir une instance d’un `IDataProtectionProvider`, et

3. Création d’un `IDataProtector` à partir d’un `IDataProtectionProvider` et utilisation de celui-ci pour protéger et ôter la protection des données.

[!code-csharp[](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Le package Microsoft. AspNetCore. DataProtection. Abstracts contient une méthode d’extension `IServiceProvider.GetDataProtector` en tant que commodité du développeur. Il encapsule comme une opération unique en extrayant un `IDataProtectionProvider` du fournisseur de services et en appelant `IDataProtectionProvider.CreateProtector`. L’exemple suivant illustre son utilisation.

[!code-csharp[](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> Les instances de `IDataProtectionProvider` et `IDataProtector` sont thread-safe pour plusieurs appelants. Il est prévu qu’une fois qu’un composant obtient une référence à un `IDataProtector` via un appel à `CreateProtector`, il utilise cette référence pour plusieurs appels à `Protect` et `Unprotect`. Un appel à `Unprotect` lèvera CryptographicException si la charge utile protégée ne peut pas être vérifiée ou déchiffrée. Certains composants peuvent souhaiter ignorer des erreurs lors de l’opération de non-protection. un composant qui lit les cookies d’authentification peut gérer cette erreur et traiter la demande comme s’il n’avait aucun cookie au lieu de faire échouer la demande. Les composants qui veulent ce comportement doivent intercepter spécifiquement CryptographicException au lieu d’absorber toutes les exceptions.
