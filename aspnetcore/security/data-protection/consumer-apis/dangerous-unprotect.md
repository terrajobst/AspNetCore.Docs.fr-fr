---
title: Ôter la protection des charges utiles dont les clés ont été révoquées dans ASP.NET Core
author: rick-anderson
description: Découvrez comment ôter la protection des données, protégées par des clés qui ont depuis été révoqués, dans une application ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 26061d048dcd9c1e3d8909e9388d8b565376fa2f
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897106"
---
# <a name="unprotect-payloads-whose-keys-have-been-revoked-in-aspnet-core"></a>Ôter la protection des charges utiles dont les clés ont été révoquées dans ASP.NET Core

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

L’API de protection des données ASP.NET Core s’appliquent pas principalement pour la persistance indéfini de charges utiles confidentielles. Autres technologies telles que [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) et [Azure Rights Management](/rights-management/) sont plus adaptés pour le scénario de stockage indéterminée, et ils ont des fonctionnalités de gestion de clé forte en conséquence. Ceci dit, il n’y a rien interdire à un développeur à l’aide de l’API de protection des données ASP.NET Core pour la protection à long terme des données confidentielles. Les clés ne sont jamais supprimés du key ring, par conséquent, `IDataProtector.Unprotect` peut toujours récupérer les charges utiles existants tant que les clés sont disponibles et valides.

Toutefois, un problème survient lorsque le développeur tente d’ôter la protection des données qui a été protégées par une clé révoquée, en tant que `IDataProtector.Unprotect` lève une exception dans ce cas. Cela peut être parfait pour les charges utiles temporaires (par exemple, les jetons d’authentification), car ces types de charges utiles peuvent être recréés facilement par le système, et au pire le visiteur du site peut-être être nécessaires pour vous connecter à nouveau. Mais pour les charges utiles persistantes, avoir `Unprotect` throw peut entraîner une perte de données inacceptable.

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

Pour prendre en charge le scénario d’autoriser des charges utiles à déprotéger même en cas de clés révoqués, le système de protection de données contient un `IPersistedDataProtector` type. Pour obtenir une instance de `IPersistedDataProtector`, simplement obtenir une instance de `IDataProtector` dans la méthode traditionnelle et try cast le `IDataProtector` à `IPersistedDataProtector`.

> [!NOTE]
> Pas tous `IDataProtector` instances pouvant être castés en `IPersistedDataProtector`. Les développeurs doivent utiliser le C# en tant qu’opérateur ou similaire pour éviter les exceptions runtime dû au fait des casts non valides, et ils doivent être préparées gérer le cas de défaillance de manière appropriée.

`IPersistedDataProtector` expose la surface d’API suivante :

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

Cette API prend la charge utile protégée (comme un tableau d’octets) et retourne la charge utile non protégée. Il n’existe aucune surcharge basé sur chaîne. Les deux paramètres out sont comme suit.

* `requiresMigration`: défini sur « True » si la clé utilisée pour protéger cette charge utile n’est plus la clé active par défaut, par exemple, la clé utilisée pour protéger cette charge utile est ancienne et une opération de restauration de clé a depuis accéderez sur place. Vous pouvez l’appelant à prendre en compte la reprotection de la charge utile en fonction de leurs besoins professionnels.

* `wasRevoked`: sera défini sur « True » si la clé utilisée pour protéger cette charge utile a été révoquée.

>[!WARNING]
> Très vigilant lors de la transmission `ignoreRevocationErrors: true` à la `DangerousUnprotect` (méthode). Si après l’appel de cette méthode le `wasRevoked` valeur est true, la clé utilisée pour protéger cette charge utile a été révoquée, puis les authenticité de la charge utile doivent être traitée comme étant suspecte. Dans ce cas, uniquement poursuivra sur la charge utile non protégée si vous avez une garantie distincte qu’il est authentique, par exemple, qu’elles proviennent d’une base de données sécurisée, plutôt que d’envoyées par un client web non approuvés.

[!code-csharp[](dangerous-unprotect/samples/dangerous-unprotect.cs)]
