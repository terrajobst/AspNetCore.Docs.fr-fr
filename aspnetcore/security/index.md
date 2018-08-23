---
title: Vue d’ensemble de la sécurité ASP.NET Core
author: tdykstra
description: Découvrez les concepts de base de l’authentification, de l’autorisation et de la sécurité dans ASP.NET Core.
ms.author: tdykstra
ms.date: 11/01/2017
uid: security/index
ms.openlocfilehash: 3a1c1ea1ad28fccbe5ae91b0be193938b095f60b
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41749887"
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
* [Attaques par injection de code SQL](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [Falsification de requête intersites (CSRF, Cross Site Request Forgery)](xref:security/anti-request-forgery)
* [Attaques par redirection ouverte](xref:security/preventing-open-redirects)

Il existe d’autres failles de sécurité que vous devez connaître. Pour plus d’informations, consultez la section de ce document relative à la *Documentation sur la sécurité ASP.NET Core*.

## <a name="aspnet-core-security-documentation"></a>Documentation sur la sécurité ASP.NET Core

*   [Authentification](xref:security/authentication/index)
    *   [Présentation d’Identity](xref:security/authentication/identity)
    *   [Activer l’authentification à l’aide de Facebook, Google et d’autres fournisseurs externes](xref:security/authentication/social/index)
    *   [Activer l’authentification avec WS-Federation](xref:security/authentication/ws-federation)
    * [Configurer l’authentification Windows](xref:security/authentication/windowsauth)
    *   [Confirmation de compte et récupération de mot de passe](xref:security/authentication/accconfirm)
    *   [Authentification à deux facteurs avec SMS](xref:security/authentication/2fa)
    *   [Utiliser l’authentification par cookie sans Identity](xref:security/authentication/cookie)
    *   [Azure Active Directory](xref:security/authentication/azure-active-directory/index)
        *   [Intégrer Azure AD dans une application web ASP.NET Core](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Appeler une API web ASP.NET Core à partir d’une application WPF via Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Appeler une API web dans une application web ASP.NET Core via Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [Une application web ASP.NET Core Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [Sécuriser les applications ASP.NET Core avec IdentityServer4](https://identityserver4.readthedocs.io)
*   [Autorisation](xref:security/authorization/index)
    *   [Introduction](xref:security/authorization/introduction)
    *   [Créer une application avec des données utilisateur protégées par une autorisation](xref:security/authorization/secure-data)
    *   [Autorisation simple](xref:security/authorization/simple)
    *   [Autorisation basée sur des rôles](xref:security/authorization/roles)
    *   [Autorisation basée sur des revendications](xref:security/authorization/claims)
    *   [Autorisation basée sur une stratégie](xref:security/authorization/policies)
    *   [Injection de dépendances dans les gestionnaires d’exigences](xref:security/authorization/dependencyinjection)
    *   [Autorisation basée sur les ressources](xref:security/authorization/resourcebased)
    *   [Autorisation basée sur les vues](xref:security/authorization/views)
    *   [Limiter une identité par schéma](xref:security/authorization/limitingidentitybyscheme)
*   [Protection des données](xref:security/data-protection/index)
    *   [Présentation de la protection des données](xref:security/data-protection/introduction)
    *   [Bien démarrer avec les API de protection des données](xref:security/data-protection/using-data-protection)
    *   [API de contrôle serveur consommateur](xref:security/data-protection/consumer-apis/index)
        *   [Vue d’ensemble des API de contrôle serveur consommateur](xref:security/data-protection/consumer-apis/overview)
        *   [Chaînes d’objectifs](xref:security/data-protection/consumer-apis/purpose-strings)
        *   [Hiérarchie d’objectifs et architecture mutualisée](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)
        *   [Hacher les mots de passe](xref:security/data-protection/consumer-apis/password-hashing)
        *   [Limiter la durée de vie des charges utiles protégées](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)
        *   [Retirer la protection des charges utiles dont les clés ont été révoquées](xref:security/data-protection/consumer-apis/dangerous-unprotect)
    *   [Configuration](xref:security/data-protection/configuration/index)
        *   [Configurer la protection des données](xref:security/data-protection/configuration/overview)
        *   [Paramètres par défaut](xref:security/data-protection/configuration/default-settings)
        *   [Stratégie à l’échelle de l’ordinateur](xref:security/data-protection/configuration/machine-wide-policy)
        *   [Scénarios non compatibles avec l’injection de dépendances](xref:security/data-protection/configuration/non-di-scenarios)
    *   [API d’extensibilité](xref:security/data-protection/extensibility/index)
        *   [Extensibilité du chiffrement de base](xref:security/data-protection/extensibility/core-crypto)
        *   [Extensibilité de la gestion de clés](xref:security/data-protection/extensibility/key-management)
        *   [API diverses](xref:security/data-protection/extensibility/misc-apis)
    *   [Implémentation](xref:security/data-protection/implementation/index)
        *   [Détails du chiffrement authentifié](xref:security/data-protection/implementation/authenticated-encryption-details)
        *   [Dérivation de sous-clé et chiffrement authentifié](xref:security/data-protection/implementation/subkeyderivation)
        *   [En-têtes de contexte](xref:security/data-protection/implementation/context-headers)
        *   [Gestion des clés](xref:security/data-protection/implementation/key-management)
        *   [Fournisseurs de stockage de clés](xref:security/data-protection/implementation/key-storage-providers)
        *   [Chiffrement à clé au repos](xref:security/data-protection/implementation/key-encryption-at-rest)
        *   [Immuabilité et paramètres des clés](xref:security/data-protection/implementation/key-immutability)
        *   [Format de stockage des clés](xref:security/data-protection/implementation/key-storage-format)
        *   [Fournisseurs de protection des données éphémères](xref:security/data-protection/implementation/key-storage-ephemeral)
    *   [Compatibilité](xref:security/data-protection/compatibility/index)
        *   [Remplacer <machineKey> dans ASP.NET](xref:security/data-protection/compatibility/replacing-machinekey)
*   [Créer une application avec des données utilisateur protégées par une autorisation](xref:security/authorization/secure-data)
*   [Stockage sécurisé des secrets des applications lors du développement](xref:security/app-secrets)
*   [Fournisseur de configuration Azure Key Vault](xref:security/key-vault-configuration)
*   [Appliquer SSL](xref:security/enforcing-ssl)
*   [Protection contre la falsification de requête](xref:security/anti-request-forgery)
*   [Bloquer les attaques par redirection ouverte](xref:security/preventing-open-redirects)
*   [Bloquer les scripts intersites](xref:security/cross-site-scripting)
*   [Activer les requêtes d’origines différentes](xref:security/cors)
*   [Partager des cookies entre applications](xref:security/cookie-sharing)
