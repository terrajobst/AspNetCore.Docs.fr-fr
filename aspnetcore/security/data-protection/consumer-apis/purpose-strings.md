---
title: Chaînes d’objectifs dans ASP.NET Core
author: rick-anderson
description: Découvrez comment les chaînes d’objectifs sont utilisées dans l’API de Protection des données ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: 4c85423f8de7e4b784ae1bb304a884541df251b6
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087548"
---
# <a name="purpose-strings-in-aspnet-core"></a>Chaînes d’objectifs dans ASP.NET Core

<a name="data-protection-consumer-apis-purposes"></a>

Les composants qui consomment `IDataProtectionProvider` doit passer une valeur unique *à des fins* paramètre à la `CreateProtector` (méthode). Les besoins *paramètre* est inhérent à la sécurité du système de protection de données, car elle fournit une isolation entre les consommateurs de services de chiffrement, même si les clés de chiffrement racine sont les mêmes.

Quand un consommateur spécifie un objectif, la chaîne de l’objectif est utilisée, ainsi que les clés de chiffrement de racine pour dériver des sous-clés de chiffrement uniques pour ce consommateur. Cela permet d’isoler le consommateur à partir de tous les autres consommateurs de services de chiffrement dans l’application : aucun autre composant ne peut lire ses charges utiles, et il ne peut pas lire des charges utiles de n’importe quel autre composant. Cette isolation restitue également irréalisable différentes catégories d’attaque contre le composant.

![Exemple de diagramme d’objectif](purpose-strings/_static/purposes.png)

Dans le diagramme ci-dessus, `IDataProtector` instances A et B **ne peut pas** lire d’autres charges utiles, uniquement leurs propres.

La chaîne de fin ne doit pas être secret. Il doit simplement être unique en ce sens qu’aucun autre composant se comportant bien ne fournira jamais la même chaîne de fin.

>[!TIP]
> À l’aide de l’espace de noms et nom de type du composant consomme l’API de protection des données est une règle empirique, comme dans les pratiques de que ces informations ne seront jamais entrent en conflit.
>
>Un composant créé par Contoso, qui est responsable de minting jetons du porteur peut utiliser Contoso.Security.BearerToken comme sa chaîne d’objectif. Ou - plus - il peut utiliser Contoso.Security.BearerToken.v1 en tant que chaîne de son objectif. Ajoutant le numéro de version permet à une future version à utiliser Contoso.Security.BearerToken.v2 comme son objectif, et les différentes versions serait complètement isolées les uns des autres aussi loin que charges utiles.

Depuis le paramètre à des fins de `CreateProtector` est un tableau de chaînes, la méthode ci-dessus pourrait avoir été au lieu de cela spécifiée comme `[ "Contoso.Security.BearerToken", "v1" ]`. Cela permet l’établissement d’une hiérarchie des objectifs et ouvre la possibilité de scénarios d’une architecture mutualisées avec le système de protection des données.

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> Composants ne doit pas autoriser l’entrée utilisateur non fiable être la seule source d’entrée de la chaîne à des fins.
>
>Par exemple, considérez un composant Contoso.Messaging.SecureMessage qui est responsable du stockage des messages sécurisés. Si le composant de messagerie sécurisé devait appeler `CreateProtector([ username ])`, puis un utilisateur malveillant peut créer un compte avec le nom d’utilisateur « Contoso.Security.BearerToken » dans une tentative d’obtention du composant à appeler `CreateProtector([ "Contoso.Security.BearerToken" ])`, par inadvertance entraînant la messagerie sécurisée système de charges utiles mint qui pourraient être perçus comme des jetons d’authentification.
>
>Une chaîne à des fins de mieux pour le composant de messagerie serait `CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`, qui fournit une isolation appropriée.

L’isolation assurée par et les comportements de `IDataProtectionProvider`, `IDataProtector`, et à des fins sont les suivantes :

* Pour une donnée `IDataProtectionProvider` objet, le `CreateProtector` méthode crée un `IDataProtector` objet exclusivement liés à la fois à la `IDataProtectionProvider` objet qui a créé et le paramètre à des fins qui a été passé dans la méthode.

* Le paramètre de fin ne doit pas être null. (Si à des fins est spécifié sous forme de tableau, cela signifie que le tableau ne doit pas être de longueur nulle et que tous les éléments du tableau doivent être non null.) Un objectif de la chaîne vide est autorisée techniquement mais est déconseillé.

* Arguments de deux objectifs sont équivalentes si et seulement si elles contiennent les mêmes chaînes (à l’aide d’un comparateur ordinal) dans le même ordre. Un argument unique objectif équivaut au tableau correspondant à des fins de l’élément.

* Deux `IDataProtector` objets sont équivalents si et seulement si elles sont créées à partir de l’équivalent `IDataProtectionProvider` objets avec des paramètres équivalents à des fins.

* Pour une donnée `IDataProtector` objet, un appel à `Unprotect(protectedData)` retournera l’original `unprotectedData` si et seulement si `protectedData := Protect(unprotectedData)` pour l’équivalent `IDataProtector` objet.

> [!NOTE]
> Nous n’envisageons pas le cas où un composant choisit intentionnellement une chaîne d’usage qui est connue pour entrer en conflit avec un autre composant. Un tel composant est essentiellement considéré comme nuisible, et ce système n’est pas destiné à fournir des garanties de sécurité dans le cas où un code malveillant est déjà en cours d’exécution à l’intérieur du processus de travail.
