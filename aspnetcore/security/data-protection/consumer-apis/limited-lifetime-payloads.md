---
title: Limite la durée de vie des charges utiles protégées dans ASP.NET Core
author: rick-anderson
description: Découvrez comment limiter la durée de vie d’une charge protégée à l’aide de l’API de Protection des données ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 8dc3b856ec67477ec8ae777749c9bf3107eb4eda
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897116"
---
# <a name="limit-the-lifetime-of-protected-payloads-in-aspnet-core"></a>Limite la durée de vie des charges utiles protégées dans ASP.NET Core

Il existe des scénarios où le développeur souhaite créer une charge utile protégée qui expire après un laps de temps défini. Par exemple, la charge utile protégée peut représenter un jeton de réinitialisation de mot de passe qui ne doit être valid pendant une heure. Il est tout à fait possible pour les développeurs à créer leur propre format de charge utile qui contient une date d’expiration incorporé, et les développeurs expérimentés souhaiterez peut-être le faire quand même, mais pour la majorité des développeurs la gestion de ces délais d’expiration peut devenir fastidieuse.

Pour faciliter cette opération pour notre public de développeur, le package [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contient les API de l’utilitaire pour la création de charges utiles qui expirent automatiquement après un laps de temps défini. Ces API de blocage issu de la `ITimeLimitedDataProtector` type.

## <a name="api-usage"></a>Utilisation de l’API

Le `ITimeLimitedDataProtector` interface est l’interface principale pour protéger et déprotéger les charges utiles à expiration automatique / de durée limitée. Pour créer une instance d’un `ITimeLimitedDataProtector`, vous devez tout d’abord une instance d’une expression régulière [IDataProtector](xref:security/data-protection/consumer-apis/overview) construit avec un objectif spécifique. Une fois le `IDataProtector` instance n’est disponible, appelez le `IDataProtector.ToTimeLimitedDataProtector` méthode d’extension se remettre un protecteur avec les fonctionnalités intégrées d’expiration.

`ITimeLimitedDataProtector` expose les méthodes de surface et l’extension d’API suivantes :

* CreateProtector (objectif de chaîne) : ITimeLimitedDataProtector - cette API est similaire à l’objet de `IDataProtectionProvider.CreateProtector` car il peut être utilisé pour créer [usage chaînes](xref:security/data-protection/consumer-apis/purpose-strings) à partir d’un protecteur de durée limitée de racine.

* Protéger (byte [] en texte brut, d’expiration DateTimeOffset) : byte]

* Protéger (texte en clair de byte [], durée de vie TimeSpan) : byte]

* Protéger (texte en clair de byte []) : byte]

* Protéger (texte en clair chaîne, d’expiration DateTimeOffset) : chaîne

* Protéger (texte en clair de la chaîne, durée de vie TimeSpan) : chaîne

* Protéger (texte en clair de la chaîne) : chaîne

Outre la base `Protect` méthodes qui prennent uniquement le texte en clair, il existe de nouvelles surcharges qui permettent de spécifier la date d’expiration de la charge utile. La date d’expiration peut être spécifiée comme une date absolue (via un `DateTimeOffset`) ou comme une heure relative (à partir du système actuel temps, via un `TimeSpan`). Si une surcharge qui ne prend pas d’un délai d’expiration est appelée, la charge utile est censée ne jamais pour expirer.

* Ôter la protection (protectedData [] octets, à l’expiration de DateTimeOffset) : byte]

* Unprotect(byte[] protectedData) : byte[]

* Ôter la protection (chaîne protectedData out d’expiration DateTimeOffset) : chaîne

* Ôter la protection (chaîne protectedData) : chaîne

Le `Unprotect` méthodes retournent les données non protégées d’origine. Si la charge utile n’a pas encore expiré, l’expiration absolue est retournée en tant que paramètre, ainsi que les données non protégées d’origine de sortie facultatif. Si la charge utile a expiré, toutes les surcharges de la méthode Unprotect lèvera CryptographicException.

>[!WARNING]
> Il n’est pas conseillé d’utiliser ces API pour protéger les charges utiles qui nécessitent la persistance à long terme ou indéterminée. « Puis-je me permettre d’utiliser pour les charges utiles protégées à être définitivement irrécupérable après un mois ? » peut servir à une règle empirique ; Si la réponse est Aucun développeur puis ne doit prendre en compte les autres API.

L’exemple ci-dessous utilise la [chemins de code de l’injection de dépendance non](xref:security/data-protection/configuration/non-di-scenarios) pour instancier le système de protection des données. Pour exécuter cet exemple, assurez-vous que vous avez tout d’abord ajouté une référence au package Microsoft.AspNetCore.DataProtection.Extensions.

[!code-csharp[](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
