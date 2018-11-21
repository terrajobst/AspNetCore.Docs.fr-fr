---
title: Vue d’ensemble de la sécurité ASP.NET Core
author: tdykstra
description: Découvrez les concepts de base de l’authentification, de l’autorisation et de la sécurité dans ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: security/index
ms.openlocfilehash: 579e472e01efd08bbafe949e37a3b655a42a5b46
ms.sourcegitcommit: 04b55a5ce9d649ff2df926157ec28ae47afe79e2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52156917"
---
# <a name="overview-of-aspnet-core-security"></a>Vue d’ensemble de la sécurité ASP.NET Core

ASP.NET Core permet aux développeurs de configurer et de gérer facilement la sécurité de leurs applications. ASP.NET Core contient des fonctionnalités permettant de gérer l’authentification, l’autorisation, la protection des données, l’implémentation de SSL, les secrets des applications, la protection contre la falsification de requêtes et la gestion CORS. Ces fonctionnalités de sécurité vous permettent de générer des applications ASP.NET Core robustes et néanmoins sécurisées.

## <a name="aspnet-core-security-features"></a>Fonctionnalités de sécurité ASP.NET Core

ASP.NET Core offre un grand nombre d’outils et de bibliothèques pour sécuriser vos applications, notamment des fournisseurs d’identité intégrés. Cela dit, vous pouvez utiliser des services d’identité tiers tels que Facebook, Twitter ou LinkedIn. Avec ASP.NET Core, vous pouvez facilement gérer les secrets des applications, qui sont un moyen de stocker et d’utiliser des informations confidentielles sans avoir à les exposer dans le code.

## <a name="authentication-vs-authorization"></a>Authentification et Autorisation

L’authentification est un processus selon lequel un utilisateur fournit des informations d’identification qui sont ensuite comparées à celles stockées dans un système d’exploitation, une base de données, une application ou une ressource. Si elles correspondent, les utilisateurs sont authentifiés et peuvent alors effectuer les actions pour lesquelles ils disposent d’autorisations pendant un processus d’autorisation. L’autorisation désigne le processus qui détermine ce qu’un utilisateur est autorisé à faire.

Vous pouvez aussi vous représenter l’authentification comme un moyen d’entrer dans un espace, tel qu’un serveur, une base de données, une application ou une ressource, tandis que l’autorisation consiste à définir quelles actions l’utilisateur peut effectuer sur quels objets à l’intérieur de cet espace (serveur, base de données ou application).

## <a name="common-vulnerabilities-in-software"></a>Failles de sécurité courantes dans les logiciels

ASP.NET Core et Entity Framework contiennent des fonctionnalités qui vous aident à sécuriser vos applications et à empêcher les violations de sécurité. La liste de liens ci-après vous permet d’accéder à une documentation décrivant en détail des techniques destinées à éviter les failles de sécurité les plus courantes dans les applications web :

* [Attaques par exécution de scripts de site à site](xref:security/cross-site-scripting)
* [Attaques par injection de code SQL](/ef/core/querying/raw-sql)
* [Falsification de requête intersites (CSRF, Cross Site Request Forgery)](xref:security/anti-request-forgery)
* [Attaques par redirection ouverte](xref:security/preventing-open-redirects)

Il existe d’autres failles de sécurité que vous devez connaître. Pour plus d’informations, consultez les autres articles de la section **Sécurité et identité** de la table des matières.
