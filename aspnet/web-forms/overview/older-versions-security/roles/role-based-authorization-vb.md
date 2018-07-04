---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-vb
title: Autorisation en fonction du rôle (VB) | Microsoft Docs
author: rick-anderson
description: Ce didacticiel commence par examiner comment le framework de rôles associe des rôles d’un utilisateur à son contexte de sécurité. Il examine ensuite comment appliquer des URL en fonction du rôle...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 83b4f5a4-4f5a-4380-ba33-f0b5c5ac6a75
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-vb
msc.type: authoredcontent
ms.openlocfilehash: b8cf8cc5fa802eb812482b236c8b2825423027e7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390983"
---
<a name="role-based-authorization-vb"></a>Autorisation en fonction du rôle (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.11.zip) ou [télécharger le PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_vb.pdf)

> Ce didacticiel commence par examiner comment le framework de rôles associe des rôles d’un utilisateur à son contexte de sécurité. Il examine ensuite comment appliquer des règles d’autorisation d’URL en fonction du rôle. Suivant, nous allons examiner à l’aide de moyens déclaratives et par programme pour modifier les données affichées et les fonctionnalités offertes par une page ASP.NET.


## <a name="introduction"></a>Introduction

Dans le <a id="_msoanchor_1"> </a> [ *autorisation basée sur l’utilisateur* ](../membership/user-based-authorization-vb.md) didacticiel, nous avons vu comment utiliser l’autorisation d’URL pour spécifier quels utilisateurs peuvent visiter un ensemble particulier de pages. Avec un peu de balisage dans `Web.config`, nous pourrions indiquer à ASP.NET pour autoriser uniquement les utilisateurs authentifiés à visiter une page. Nous pourrions exigent que seuls les utilisateurs Tito et Bob ont été autorisés, ou indiquer que tous les utilisateurs authentifiés à l’exception de Sam étaient autorisées.

En plus de l’autorisation d’URL, nous avons également vu déclaratives et par programme des techniques permettant de contrôler les données affichées et les fonctionnalités offertes par une page en fonction de l’utilisateur visite. En particulier, nous avons créé une page qui répertoriés le contenu du répertoire actif. Tout le monde peut consulter cette page, mais seuls les utilisateurs authentifiés peuvent afficher les contenus des dossiers, et uniquement Tito pu supprimer les fichiers.

Application des règles d’autorisation sur une base de l’utilisateur par l’utilisateur peut devenir un cauchemar en termes de comptabilité. Une approche plus facile à gérer consiste à utiliser l’autorisation en fonction du rôle. La bonne nouvelle est que les outils à notre disposition pour appliquer des règles d’autorisation fonctionnent aussi bien avec les rôles comme ils le font pour les comptes d’utilisateur. Règles d’autorisation d’URL peuvent spécifier des rôles plutôt qu’aux utilisateurs. Le contrôle LoginView, qui restitue une sortie différente pour les utilisateurs authentifiés et anonymes, peut être configuré pour afficher un contenu différent en fonction des rôles de l’utilisateur connecté. Et l’API de rôles inclut des méthodes pour déterminer les rôles de l’utilisateur connecté.

Ce didacticiel commence par examiner comment le framework de rôles associe des rôles d’un utilisateur à son contexte de sécurité. Il examine ensuite comment appliquer des règles d’autorisation d’URL en fonction du rôle. Suivant, nous allons examiner à l’aide de moyens déclaratives et par programme pour modifier les données affichées et les fonctionnalités offertes par une page ASP.NET. C’est parti !

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>Comprendre comment les rôles sont associés un contexte de sécurité utilisateur

Chaque fois qu’une demande entre dans le pipeline ASP.NET, il est associé à un contexte de sécurité, qui inclut des informations d’identification du demandeur. Lorsque vous utilisez l’authentification par formulaire, un ticket d’authentification est utilisé comme un jeton d’identité. Comme expliqué dans la <a id="_msoanchor_2"> </a> [ *une vue d’ensemble de l’authentification par formulaire* ](../introduction/an-overview-of-forms-authentication-vb.md) et <a id="_msoanchor_3"> </a> [ *Forms Configuration de l’authentification et des sujets avancés* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) didacticiels, le `FormsAuthenticationModule` est responsable de la détermination de l’identité du demandeur, ce qu’il fait pendant le [ `AuthenticateRequest` événement](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Si un ticket d’authentification valide, non expiré est trouvé, le `FormsAuthenticationModule` décode afin de déterminer l’identité du demandeur. Il crée un nouveau `GenericPrincipal` de l’objet et affecte à la `HttpContext.User` objet. L’objectif d’une entité, telles que `GenericPrincipal`, consiste à identifier le nom de l’utilisateur authentifié et qu’elle appartiennent à des rôles. Cet effet est évident par le fait que tous les objets principal ont une `Identity` propriété et un `IsInRole(roleName)` (méthode). Le `FormsAuthenticationModule`, toutefois, n’est pas intéressé par l’enregistrement des informations de rôle et le `GenericPrincipal` objet qu’il crée ne spécifie pas de tous les rôles.

Si l’infrastructure de rôles est activée, le [ `RoleManagerModule` ](https://msdn.microsoft.com/library/system.web.security.rolemanagermodule.aspx) HTTP Module les étapes après la `FormsAuthenticationModule` et identifie les rôles de l’utilisateur authentifié lors de la [ `PostAuthenticateRequest` événement](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), qui se déclenche après la `AuthenticateRequest` événement. Si la demande provient d’un utilisateur authentifié, le `RoleManagerModule` remplace le `GenericPrincipal` objet créé par le `FormsAuthenticationModule` et la remplace par une [ `RolePrincipal` objet](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx). Le `RolePrincipal` classe utilise l’API de rôles pour déterminer les rôles de l’utilisateur appartient.

Figure 1 illustre le flux de travail du pipeline ASP.NET lors de l’utilisation de l’authentification par formulaire et l’infrastructure de rôles. Le `FormsAuthenticationModule` s’exécute en premier, identifie l’utilisateur via son ticket d’authentification et crée un nouveau `GenericPrincipal` objet. Ensuite, le `RoleManagerModule` intervient et remplace le `GenericPrincipal` de l’objet avec un `RolePrincipal` objet.

Si un utilisateur anonyme visite le site, ni le `FormsAuthenticationModule` ni le `RoleManagerModule` crée un objet principal.


[![Les événements de Pipeline ASP.NET pour un utilisateur authentifié lors de l’utilisation de l’authentification par formulaire et l’infrastructure de rôles](role-based-authorization-vb/_static/image2.png)](role-based-authorization-vb/_static/image1.png)

**Figure 1**: les événements de Pipeline ASP.NET pour une authentification utilisateur lorsque à l’aide de l’authentification par formulaire et l’infrastructure de rôles ([cliquez pour afficher l’image en taille réelle](role-based-authorization-vb/_static/image3.png))


### <a name="caching-role-information-in-a-cookie"></a>La mise en cache des informations de rôle dans un Cookie

Le `RolePrincipal` l’objet `IsInRole(roleName)` les appels de méthode `Roles`.`GetRolesForUser` Pour obtenir les rôles pour l’utilisateur afin de déterminer si l’utilisateur est membre du *roleName*. Lorsque vous utilisez le `SqlRoleProvider`, il en résulte dans une requête à la base de données du magasin de rôle. Lorsque vous utilisez des règles d’autorisation URL en fonction du rôle le `RolePrincipal`de `IsInRole` méthode sera appelée à chaque demande pour une page qui est protégée par les règles d’autorisation d’URL en fonction du rôle. Plutôt que d’avoir à rechercher les informations de rôle dans la base de données à chaque demande, le `Roles` framework inclut une option pour mettre en cache des rôles de l’utilisateur dans un cookie.

Si l’infrastructure de rôles est configurée pour mettre en cache des rôles de l’utilisateur dans un cookie, le `RoleManagerModule` crée le cookie pendant le pipeline ASP.NET [ `EndRequest` événement](https://msdn.microsoft.com/library/system.web.httpapplication.endrequest.aspx). Ce cookie est utilisé dans les demandes suivantes dans le `PostAuthenticateRequest`, c'est-à-dire lorsque la `RolePrincipal` objet est créé. Si le cookie est valide et n’a pas expiré, les données dans le cookie sont analysées et utilisées pour remplir les rôles de l’utilisateur, ce qui l’enregistrement le `RolePrincipal` d’avoir à effectuer un appel à la `Roles` classe pour déterminer les rôles de l’utilisateur. La figure 2 illustre ce flux de travail.


[![Informations de rôle de l’utilisateur peuvent être stockées dans un Cookie pour améliorer les performances](role-based-authorization-vb/_static/image5.png)](role-based-authorization-vb/_static/image4.png)

**Figure 2**: rôle informations peuvent être stockées l’utilisateur le dans un Cookie pour améliorer les performances ([cliquez pour afficher l’image en taille réelle](role-based-authorization-vb/_static/image6.png))


Par défaut, le mécanisme de cookie de rôle du cache est désactivé. Il peut être activé via le `<roleManager>`; balisage de configuration dans `Web.config`. Nous avons abordé à l’aide de la [ `<roleManager>` élément](https://msdn.microsoft.com/library/ms164660.aspx) pour spécifier des fournisseurs de rôles dans le <a id="_msoanchor_4"> </a> [ *création et gestion des rôles* ](creating-and-managing-roles-vb.md) (didacticiel), afin de vous disposez déjà cet élément de votre application `Web.config` fichier. Les paramètres de cookies de cache de rôle sont spécifiées en tant qu’attributs de la `<roleManager>`; élément et sont résumées dans le tableau 1.

> [!NOTE]
> Les paramètres de configuration répertoriées dans le tableau 1 spécifient les propriétés du cookie de cache de rôle qui en résulte. Pour plus d’informations sur les cookies, leur fonctionnement et leurs différentes propriétés, consultez [ce didacticiel de Cookies](http://www.quirksmode.org/js/cookies.html).


| <strong>Property</strong> |                                                                                                                                                                                                                                                                                                                                                         <strong>Description</strong>                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   `cacheRolesInCookie`    |                                                                                                                                                                                                                                                                                                                              Une valeur booléenne qui indique si la mise en cache de cookie est utilisé. La valeur par défaut est `false`.                                                                                                                                                                                                                                                                                                                              |
|       `cookieName`        |                                                                                                                                                                                                                                                                                                                                     Le nom du cookie de cache de rôle. La valeur par défaut est «. ASPXROLES ».                                                                                                                                                                                                                                                                                                                                     |
|       `cookiePath`        |                                                                                                                                                                                                                                Le chemin d’accès au cookie de rôles nom. L’attribut de chemin d’accès permet à un développeur limiter l’étendue d’un cookie pour une hiérarchie de répertoires particulière. La valeur par défaut est « / », qui informe le navigateur pour envoyer le cookie du ticket d’authentification à toute demande adressée au domaine.                                                                                                                                                                                                                                 |
|    `cookieProtection`     |                                                                                                                                                               Indique quelles techniques sont utilisées pour protéger le cookie de cache de rôle. Les valeurs autorisées sont : `All` (la valeur par défaut) ; `Encryption`; `None`; et `Validation`. Revenir à l’étape 3 de la <a id="_anchor_5"> </a> [ *Configuration de l’authentification de formulaires et des sujets avancés* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) didacticiel pour plus d’informations sur les niveaux de protection.                                                                                                                                                                |
|    `cookieRequireSSL`     |                                                                                                                                                                                                                                                                                                   Une valeur booléenne qui indique si une connexion SSL est requise pour transmettre le cookie d’authentification. La valeur par défaut est `false`.                                                                                                                                                                                                                                                                                                   |
| `cookieSlidingExpiration` |                                                                                                                                                                                                                                                  Valeur booléenne qui indique si le délai d’expiration du cookie est réinitialisée à chaque fois que l’utilisateur visite le site pendant une session unique. La valeur par défaut est `false`. Cette valeur est pertinente uniquement lorsque `createPersistentCookie` est défini sur `true`.                                                                                                                                                                                                                                                  |
|      `cookieTimeout`      |                                                                                                                                                                                                                                                                         Spécifie la durée, en minutes, après laquelle le cookie du ticket d’authentification expire. La valeur par défaut est `30`. Cette valeur est pertinente uniquement lorsque `createPersistentCookie` est défini sur `true`.                                                                                                                                                                                                                                                                         |
| `createPersistentCookie`  |                                                                                                                                                                   Valeur booléenne qui spécifie si le cookie de cache de rôle est un cookie de session ou un cookie persistant. Si `false` (la valeur par défaut), un cookie de session est utilisé, ce qui est supprimé lorsque le navigateur est fermé. Si `true`, un cookie persistant est utilisé ; il arrive à expiration `cookieTimeout` nombre de minutes après qu’elle a été créée ou après la visite précédente, selon la valeur de `cookieSlidingExpiration`.                                                                                                                                                                    |
|         `domain`          |                                                                                                                                                 Spécifie la valeur du cookie domaine. La valeur par défaut est une chaîne vide, ce qui entraîne le navigateur à utiliser le domaine à partir duquel il a été émis (par exemple, www.yourdomain.com). Dans ce cas, le cookie sera <strong>pas</strong> envoyer lors de la fabrication des demandes à sous-domaines, tels que admin.yourdomain.com. Si vous souhaitez que le cookie à passer à tous les sous-domaines dont vous avez besoin personnaliser le `domain` attribut, la valeur « votredomaine.com ».                                                                                                                                                 |
|    `maxCachedResults`     | Spécifie le nombre maximal de noms de rôles sont mis en cache dans le cookie. La valeur par défaut est 25. Le `RoleManagerModule` ne crée pas un cookie pour les utilisateurs qui appartiennent à plusieurs `maxCachedResults` rôles. Par conséquent, le `RolePrincipal` l’objet `IsInRole` méthode utilisera la `Roles` classe pour déterminer les rôles de l’utilisateur. La raison pour laquelle `maxCachedResults` existe est, car le nombre d’agents utilisateur n’autorise pas les cookies supérieurs à 4 096 octets. Par conséquent, cet embout vise à réduire le risque de dépasser cette limite de taille. Si vous avez des noms de rôles très longs, vous souhaiterez en spécifiant un petit `maxCachedResults` valeur ; contrariwise, si vous avez des noms de rôles très court, vous pouvez sans doute augmenter cette valeur. |

**Tableau 1**: les Options de Configuration Cookie de rôle du Cache

Nous allons configurer notre application à utiliser des cookies de cache de rôle non persistant. Pour ce faire, mettez à jour le `<roleManager>` élément `Web.config` pour inclure les attributs de cookies suivants :

[!code-xml[Main](role-based-authorization-vb/samples/sample1.xml)]

J’ai mis à jour le `<roleManager>`; élément en ajoutant trois attributs : `cacheRolesInCookie`, `createPersistentCookie`, et `cookieProtection`. En définissant `cacheRolesInCookie` à `true`, le `RoleManagerModule` met alors automatiquement en cache des rôles de l’utilisateur dans un cookie, plutôt que d’avoir à rechercher des informations de rôle de l’utilisateur à chaque demande. Définir explicitement la `createPersistentCookie` et `cookieProtection` attributs `false` et `All`, respectivement. Techniquement, j’ai n’a pas besoin spécifier les valeurs de ces attributs dans la mesure où j’ai simplement affectés à leurs valeurs par défaut, mais j’ai placé ici pour effectuer de manière explicite effacer que je n’utilise des cookies persistants et que le cookie est chiffré et validé.

C’est tout cela ! Désormais, le framework de rôles met en cache des rôles d’utilisateurs dans des cookies. Si le navigateur de l’utilisateur ne prend pas en charge les cookies, ou si les cookies sont supprimés ou perdus, d’une certaine manière, il n’est pas très important : le `RolePrincipal` objet utilisera tout simplement la `Roles` classe dans les cas où aucun cookie (ou une version non valide ou a expiré) est disponible.

> [!NOTE]
> Les modèles de Microsoft &amp; groupe de pratiques déconseille l’utilisation de cookies de cache de rôle persistant. Étant donné que la possession du cookie de cache de rôle est suffisante pour prouver l’appartenance au rôle, si un pirate informatique d’une certaine manière peuvent-ils accéder à des cookies d’un utilisateur valide qu’il peut emprunter l’identité de cet utilisateur. La probabilité de cette situation se produise augmente si le cookie est rendu persistant sur le navigateur de l’utilisateur. Pour plus d’informations sur cette recommandation de sécurité, ainsi que d’autres problèmes de sécurité, reportez-vous à la [liste de questions de sécurité pour ASP.NET 2.0](https://msdn.microsoft.com/library/ms998375.aspx).


## <a name="step-1-defining-role-based-url-authorization-rules"></a>Étape 1 : Définition des règles d’autorisation d’URL en fonction du rôle

Comme indiqué dans le <a id="_msoanchor_6"> </a> [ *autorisation basée sur l’utilisateur* ](../membership/user-based-authorization-vb.md) (didacticiel), l’autorisation d’URL offre un moyen de restreindre l’accès à un ensemble de pages sur un utilisateur par l’utilisateur ou un rôle par rôle base. Les règles d’autorisation d’URL sont énoncés dans `Web.config` à l’aide de la [ `<authorization>` élément](https://msdn.microsoft.com/library/8d82143t.aspx) avec `<allow>` et `<deny>` éléments enfants. Outre les règles d’autorisation relatifs à l’utilisateur indiqués dans les didacticiels précédents, chaque `<allow>` et `<deny>` élément enfant peut également inclure :

- Un rôle particulier
- Une liste délimitée par des virgules des rôles

Par exemple, les règles d’autorisation d’URL accorder l’accès à ces utilisateurs dans les rôles de contrôleurs et des administrateurs, mais refusent l’accès à tous les autres :

[!code-xml[Main](role-based-authorization-vb/samples/sample2.xml)]

Le `<allow>` élément dans le balisage ci-dessus indique que les rôles des contrôleurs et les administrateurs sont autorisés ; la `<deny>`; élément indique que *tous les* refusé aux utilisateurs.

Nous allons configurer notre application afin que le `ManageRoles.aspx`, `UsersAndRoles.aspx`, et `CreateUserWizardWithRoles.aspx` pages sont uniquement accessibles aux utilisateurs dans le rôle Administrateurs, alors que le `RoleBasedAuthorization.aspx` page reste accessible à tous les visiteurs.

Pour ce faire, commencez par ajouter un `Web.config` de fichiers à la `Roles` dossier.


[![Ajouter un fichier Web.config dans le répertoire de rôles](role-based-authorization-vb/_static/image8.png)](role-based-authorization-vb/_static/image7.png)

**Figure 3**: ajouter un `Web.config` de fichiers à la `Roles` répertoire ([cliquez pour afficher l’image en taille réelle](role-based-authorization-vb/_static/image9.png))


Ensuite, ajoutez le balisage de configuration suivant à `Web.config`:

[!code-xml[Main](role-based-authorization-vb/samples/sample3.xml)]

Le `<authorization>` élément dans le `<system.web>` section indique que seuls les utilisateurs dans le rôle Administrateurs peuvent accéder aux ressources ASP.NET dans le `Roles` directory. Le `<location>` élément définit un autre ensemble de règles d’autorisation d’URL pour le `RoleBasedAuthorization.aspx` page, ce qui permet de tous les utilisateurs à visiter la page.

Après avoir enregistré vos modifications à `Web.config`, connectez-vous en tant qu’utilisateur qui n’est pas dans le rôle Administrateurs et puis essayez de visiter une des pages protégées. Le `UrlAuthorizationModule` détecte que vous n’êtes pas autorisé à consulter la ressource demandée ; par conséquent, le `FormsAuthenticationModule` vous redirigera vers la page de connexion. La page de connexion puis vous êtes redirigé vers le `UnauthorizedAccess.aspx` page (voir Figure 4). Cette redirection finale à partir de la page de connexion à `UnauthorizedAccess.aspx` se produit en raison du code que nous avons ajouté à la page de connexion à l’étape 2 de la <a id="_msoanchor_7"> </a> [ *autorisation basée sur l’utilisateur* ](../membership/user-based-authorization-vb.md) didacticiel. En particulier, la page de connexion redirige automatiquement aux utilisateurs authentifiés de `UnauthorizedAccess.aspx` si la chaîne de requête contienne un `ReturnUrl` paramètre, en tant que ce paramètre indique que l’utilisateur est arrivé à la page de connexion quand vous tentez d’afficher une page, il n’était pas autorisé à afficher.


[![Seuls les utilisateurs dans le rôle Administrateurs peuvent afficher les Pages protégées](role-based-authorization-vb/_static/image11.png)](role-based-authorization-vb/_static/image10.png)

**Figure 4**: seuls les utilisateurs dans le rôle Administrateurs peuvent afficher les Pages protégées ([cliquez pour afficher l’image en taille réelle](role-based-authorization-vb/_static/image12.png))


Fermez la session et ensuite vous connecter en tant qu’utilisateur qui se trouve dans le rôle Administrateurs. Maintenant vous devez être en mesure d’afficher les trois pages protégées.


[![Tito peuvent visiter le UsersAndRoles.aspx Page, car il est dans le rôle Administrateurs](role-based-authorization-vb/_static/image14.png)](role-based-authorization-vb/_static/image13.png)

**Figure 5**: Tito peuvent visiter le `UsersAndRoles.aspx` Page, car il est dans le rôle Administrateurs ([cliquez pour afficher l’image en taille réelle](role-based-authorization-vb/_static/image15.png))


> [!NOTE]
> Lorsque vous spécifiez des règles d’autorisation d’URL – pour les rôles ou les utilisateurs – il est important de garder à l’esprit que les règles sont analysée individuellement, à partir du haut vers le bas. Dès qu’une correspondance est trouvée, l’utilisateur est accordé ou refusé l’accès, selon que la correspondance a été trouvée dans un `<allow>` ou `<deny>` élément. **Si aucune correspondance n’est trouvée, l’utilisateur est autorisé à accéder.** Par conséquent, si vous souhaitez restreindre l’accès à un ou plusieurs comptes d’utilisateur, il est impératif d’utiliser un `<deny>` élément en tant que le dernier élément dans la configuration de l’autorisation d’URL. **Si vos règles d’autorisation d’URL n’incluent pas un**`<deny>`**élément, tous les utilisateurs auront accès.** Pour une discussion plus détaillée sur la façon dont les règles d’autorisation d’URL sont analysées, vous référer à la « observez comment la `UrlAuthorizationModule` utilise les règles d’autorisation pour accorder ou refuser l’accès « section de la <a id="_msoanchor_8"> </a> [  *Autorisation basée sur l’utilisateur* ](../membership/user-based-authorization-vb.md) didacticiel.


## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>Étape 2 : Limitation des fonctionnalités de basée sur les rôles de l’utilisateur actuellement connecté

Permet de d’autorisation d’URL il plus facile de spécifier d’autorisation grossier règles cet état quelles identités est autorisées et celles qui est refusées à afficher une page particulière (ou toutes les pages dans un dossier et ses sous-dossiers). Toutefois, dans certains cas, nous pouvons qu’Autoriser tous les utilisateurs à visiter une page, mais limiter les fonctionnalités de la page en fonction des rôles de l’utilisateur visite. Ceci peut impliquer l’affichage ou masquage des données en fonction du rôle de l’utilisateur ou offre des fonctionnalités supplémentaires aux utilisateurs qui appartiennent à un rôle particulier.

Ces règles d’autorisation basée sur un rôle précis peuvent être implémentées de façon déclarative ou par programmation (ou via une combinaison des deux). Dans la section suivante, nous verrons comment implémenter l’autorisation de granularité fine déclarative via le contrôle LoginView. Ensuite, nous explorerons les techniques de programmation. Avant que nous pouvons examiner d’application des règles d’autorisation précis, toutefois, nous devons d’abord créer une page dont la fonctionnalité varie selon le rôle de l’utilisateur qu’il visite.

Nous allons créer une page qui répertorie tous les comptes d’utilisateur dans le système dans un GridView. Le contrôle GridView inclura les nom d’utilisateur, adresse de messagerie, date de la dernière connexion et commentaires à propos de l’utilisateur de chaque utilisateur. Outre l’affichage des informations de chaque utilisateur, le contrôle GridView inclut modifier et supprimer des fonctionnalités. Au départ nous créer cette page avec la modification et supprimer des fonctionnalités disponibles pour tous les utilisateurs. Dans les sections « À l’aide du contrôle LoginView » et « Limitation par programmation des fonctionnalités », nous verrons comment activer ou désactiver ces fonctionnalités en fonction du rôle de l’utilisateur visite.

> [!NOTE]
> La page ASP.NET que nous sommes sur le point de build utilise un contrôle GridView pour afficher les comptes d’utilisateur. Depuis ce didacticiel série se concentre sur l’authentification par formulaire, l’autorisation, comptes d’utilisateurs et rôles, je ne souhaite pas consacrer trop longtemps décrivant le fonctionnement interne du contrôle GridView. Alors que ce didacticiel fournit des instructions spécifiques pour la configuration de cette page, il ne pas étudier en détail pourquoi certains choix ont été apportées, ou qu’ont les propriétés particulières d’effet sur la sortie rendue. Pour un examen approfondi du contrôle GridView, regardez mon *[utilisation des données dans ASP.NET 2.0](../../data-access/index.md)* série de didacticiels.


Commencez par ouvrir le `RoleBasedAuthorization.aspx` page dans le `Roles` dossier. Faites glisser un GridView à partir de la page sur le concepteur et le jeu de son `ID` à `UserGrid`. Dans un instant, nous allons écrire le code qui appelle le `Membership`.`GetAllUsers` méthode et lie le résultat `MembershipUserCollection` objet au GridView. Le `MembershipUserCollection` contient un `MembershipUser` objet pour chaque compte d’utilisateur dans le système ; `MembershipUser` objets ont des propriétés telles que `UserName`,`Email`,`LastLoginDate` et ainsi de suite.

Avant d’écrire le code qui lie les comptes d’utilisateur à la grille, nous allons tout d’abord définir les champs du contrôle GridView. À partir de la balise active le contrôle GridView, cliquez sur le lien « Modifier les colonnes » pour lancer la boîte de dialogue champs zone (voir Figure 6). À ce stade, désactivez la case à cocher « Générer automatiquement des champs » dans l’angle inférieur gauche. Dans la mesure où nous voulons ce GridView pour inclure la modification et suppression de fonctionnalités, ajouter un CommandField et définir son `ShowEditButton` et `ShowDeleteButton` propriétés sur True. Ensuite, ajoutez quatre champs pour l’affichage de la `UserName`, `Email`, `LastLoginDate`, et `Comment` propriétés. Utiliser un BoundField pour les deux propriétés en lecture seule (`UserName` et `LastLoginDate`) et TemplateField pour les deux champs modifiables (`Email` et `Comment`).

Affiche de BoundField la première la `UserName` propriété ; ensemble son `HeaderText` et `DataField` propriétés « UserName ». Ce champ n’est pas modifiables, afin de définir son `ReadOnly` True à la propriété. Configurer le `LastLoginDate` BoundField en définissant son `HeaderText` à « Dernière Login » et ses `DataField` à « LastLoginDate ». Nous allons mettre en forme la sortie de cette BoundField donc simplement la date est affichée (au lieu de la date et l’heure). Pour ce faire, affectez de cette BoundField `HtmlEncode` False à la propriété et sa `DataFormatString` propriété à «{0:d}». Définissez également la `ReadOnly` True à la propriété.

Définir le `HeaderText` propriétés de la deux TemplateField pour « Email » et « Commentaire ».


[![Champs le contrôle GridView peuvent être configurées via la boîte de dialogue champs](role-based-authorization-vb/_static/image17.png)](role-based-authorization-vb/_static/image16.png)

**Figure 6**: champs peut être configuré via la boîte le contrôle GridView de dialogue champs ([cliquez pour afficher l’image en taille réelle](role-based-authorization-vb/_static/image18.png))


Nous devons maintenant définir le `ItemTemplate` et `EditItemTemplate` pour le « Email » et « Commentaire » TemplateField. Ajouter un contrôle Web Label à chacun de la `ItemTemplates` et lier leurs `Text` propriétés pour le `Email` et `Comment` propriétés, respectivement.

Pour le TemplateField contenu « Email », ajoutez une zone de texte nommée `Email` à son `EditItemTemplate` et lier sa `Text` propriété le `Email` propriété à l’aide de la liaison de données bidirectionnelle. Ajouter un contrôle RequiredFieldValidator et un RegularExpressionValidator à la `EditItemTemplate` pour vous assurer qu’un visiteur de modification de la propriété de messagerie a entré une adresse e-mail valide. Pour le TemplateField contenu « Commentaire », ajoutez une zone de texte multiligne nommé `Comment` à son `EditItemTemplate`. Définir la zone de texte `Columns` et `Rows` propriétés à 40 et 4, respectivement, puis de lier son `Text` propriété le `Comment` propriété à l’aide de la liaison de données bidirectionnelle.

Après avoir configuré ces TemplateField, leur balisage déclaratif doit ressembler à ce qui suit :

[!code-aspx[Main](role-based-authorization-vb/samples/sample4.aspx)]

Lors de la modification ou la suppression d’un compte d’utilisateur que nous devons savoir de cet utilisateur `UserName` valeur de propriété. Définir le GridView `DataKeyNames` propriété à « UserName » afin que ces informations sont disponibles via le GridView `DataKeys` collection.

Enfin, ajoutez un contrôle ValidationSummary à la page et définissez son `ShowMessageBox` True à la propriété et sa `ShowSummary` False à la propriété. Avec ces paramètres, le contrôle ValidationSummary affiche une alerte côté client si l’utilisateur tente de modifier un compte d’utilisateur avec une adresse de messagerie manquant ou non valide.

[!code-aspx[Main](role-based-authorization-vb/samples/sample5.aspx)]

Nous avons terminé le balisage déclaratif de cette page. Notre tâche suivante consiste à lier l’ensemble des comptes d’utilisateur pour le contrôle GridView. Ajoutez une méthode nommée `BindUserGrid` à la `RoleBasedAuthorization.aspx` classe code-behind de la page qui lie la `MembershipUserCollection` retourné par `Membership.GetAllUsers` à la `UserGrid` GridView. Appelez cette méthode à partir de la `Page_Load` Gestionnaire d’événements sur la première visite de page.

[!code-vb[Main](role-based-authorization-vb/samples/sample6.vb)]

Avec ce code en place, visitez la page via un navigateur. Comme le montre la Figure 7, vous devez voir un GridView répertoriant des informations sur chaque compte d’utilisateur dans le système.


[![Le contrôle UserGrid GridView répertorie des informations sur chaque utilisateur dans le système](role-based-authorization-vb/_static/image20.png)](role-based-authorization-vb/_static/image19.png)

**Figure 7**: le `UserGrid` GridView répertorie des informations sur chaque utilisateur dans le système ([cliquez pour afficher l’image en taille réelle](role-based-authorization-vb/_static/image21.png))


> [!NOTE]
> Le `UserGrid` GridView répertorie tous les utilisateurs dans une interface de réserve non paginée. Cette interface grille simple ne convient pas pour les scénarios où il existe plusieurs dizaines ou plusieurs utilisateurs. Une option consiste à configurer le contrôle GridView pour activer la pagination. Le `Membership.GetAllUsers` méthode a deux surcharges : une qui n’accepte aucun paramètre d’entrée et retourne tous les utilisateurs et l’autre qui prend les valeurs entières pour les index et la taille de la page et retourne uniquement le sous-ensemble spécifié d’utilisateurs. La deuxième surcharge peut être utilisé plus efficacement pour paginer à travers les utilisateurs dans la mesure où elle retourne uniquement le sous-ensemble précis des comptes d’utilisateur au lieu de *tous les* d'entre eux. Si vous avez des milliers de comptes d’utilisateur, vous souhaiterez une interface basée sur les filtres, qui affiche uniquement les utilisateurs dont nom d’utilisateur commence par un caractère sélectionné, par exemple. Le [ `Membership.FindUsersByName` méthode](https://msdn.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) est idéal pour la création d’une interface utilisateur basée sur le filtre. Nous allons examiner la création d’une telle interface dans un futur didacticiel.


Le contrôle GridView offre intégrée de modification et de suppression de prise en charge lorsque le contrôle est lié à un contrôle de source de données correctement configuré, tels que SqlDataSource ou de l’ObjectDataSource. Le `UserGrid` GridView, toutefois, a ses données lier par programmation ; par conséquent, nous devons écrire du code pour effectuer ces deux tâches. En particulier, nous devons créer des gestionnaires d’événements pour le contrôle GridView `RowEditing`, `RowCancelingEdit`, `RowUpdating`, et `RowDeleting` les événements qui sont déclenchés lorsqu’un visiteur clique le GridView modifier Annuler, mise à jour, ou supprimer des boutons.

Commencez par créer les gestionnaires d’événements pour le contrôle GridView `RowEditing`, `RowCancelingEdit`, et `RowUpdating` événements, puis ajoutez le code suivant :

[!code-vb[Main](role-based-authorization-vb/samples/sample7.vb)]

Le `RowEditing` et `RowCancelingEdit` gestionnaires d’événements définissent simplement le GridView `EditIndex` propriété et la reliaison puis la liste des utilisateurs de comptes à la grille. La plus intéressante dans le `RowUpdating` Gestionnaire d’événements. Ce gestionnaire d’événements démarre en vérifiant que les données ne sont valides et qu’il extrait ensuite le `UserName` valeur du compte d’utilisateur modifié à partir de la `DataKeys` collection. Le `Email` et `Comment` zones de texte dans les deux TemplateField `EditItemTemplate` s sont ensuite par programmation référencés. Leur `Text` propriétés contiennent l’adresse de messagerie modifiée et le commentaire.

Pour mettre à jour d’un compte d’utilisateur via l’API d’appartenance que nous devons tout d’abord obtenir les informations d’utilisateur, ce que nous faisons via un appel à `Membership.GetUser(userName)`. Retourné `MembershipUser` l’objet `Email` et `Comment` sont ensuite mises à jour avec les valeurs entrées dans les deux zones de texte à partir de l’interface de modification. Enfin, ces modifications sont enregistrées par un appel à [ `Membership.UpdateUser` ](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx). Le `RowUpdating` Gestionnaire d’événements se termine en rétablissant le contrôle GridView à son interface de modification préalable.

Ensuite, créez le `RowDeleting` RowDeleting Gestionnaire d’événements, puis ajoutez le code suivant :

[!code-vb[Main](role-based-authorization-vb/samples/sample8.vb)]

Le Gestionnaire d’événements ci-dessus commence par s’emparer du `UserName` valeur à partir de la GridView `DataKeys` collection ; cette `UserName` valeur est ensuite passée à la classe d’appartenance [ `DeleteUser` méthode](https://msdn.microsoft.com/library/system.web.security.membership.deleteuser.aspx). Le `DeleteUser` méthode supprime le compte d’utilisateur à partir du système, y compris les données d’appartenance connexes (telles que les rôles de cet utilisateur appartient). Après la suppression de l’utilisateur, la grille `EditIndex` a la valeur -1 (au cas où l’utilisateur a cliqué sur Supprimer alors qu’une autre ligne était en mode édition) et le `BindUserGrid` méthode est appelée.

> [!NOTE]
> Le bouton Supprimer ne nécessite pas une forme quelconque de confirmation de l’utilisateur avant de supprimer le compte d’utilisateur. Je vous encourage à ajouter une forme de confirmation de l’utilisateur pour réduire le risque d’un compte en cours de suppression par inadvertance. Une des méthodes plus simples pour confirmer une action est via une boîte de dialogue de confirmation du côté client. Pour plus d’informations sur cette technique, consultez [Ajout côté Client Confirmation lors de la suppression](https://asp.net/learn/data-access/tutorial-42-vb.aspx).


Vérifiez que cette page fonctionne comme prévu. Vous pourrez modifier l’adresse de messagerie d’un utilisateur et le commentaire, ainsi que supprimer n’importe quel compte d’utilisateur. Dans la mesure où le `RoleBasedAuthorization.aspx` page est accessible à tous les utilisateurs, tout utilisateur – les visiteurs anonymes même – visitez cette page et modifier et supprimer des comptes d’utilisateurs ! Nous allons mettre à jour cette page afin que seuls les utilisateurs dans les rôles superviseurs et les administrateurs peuvent modifier une adresse e-mail utilisateur et le commentaire, et seuls les administrateurs peuvent supprimer un compte d’utilisateur.

La section « À l’aide du contrôle LoginView » examine à l’aide du contrôle LoginView pour afficher les instructions spécifiques au rôle de l’utilisateur. Si une personne dans le rôle Administrateurs visite cette page, nous allons montrer des instructions sur la façon de modifier et supprimer des utilisateurs. Si un utilisateur dans le rôle superviseurs atteint cette page, nous allons montrer des instructions sur la modification des utilisateurs. Et si le visiteur est anonyme ou n’est pas dans le rôle Administrateurs ou les superviseurs, nous affichera un message expliquant qu’ils ne peuvent pas modifier ou supprimer les informations de compte d’utilisateur. Dans la section « Limitation par programmation des fonctionnalités » nous allons écrire le code qui affiche ou masque les boutons Modifier et supprimer en fonction du rôle de l’utilisateur par programme.

### <a name="using-the-loginview-control"></a>Utilisation du contrôle LoginView

Comme nous l’avons vu dans les didacticiels passées, le contrôle LoginView est utile pour afficher différentes interfaces pour les utilisateurs authentifiés et anonymes, mais le contrôle LoginView peut également être utilisé pour afficher différents balisages basé sur les rôles de l’utilisateur. Nous allons utiliser un contrôle LoginView pour afficher des instructions différentes en fonction du rôle de l’utilisateur visite.

Démarrage en ajoutant un LoginView ci-dessus le `UserGrid` GridView. Comme nous l’avons vu précédemment, le contrôle LoginView a deux modèles intégrés : `AnonymousTemplate` et `LoggedInTemplate`. Entrez un bref message dans ces deux modèles qui informe l’utilisateur qu’ils ne peuvent pas modifier ou supprimer toutes les informations utilisateur.

[!code-aspx[Main](role-based-authorization-vb/samples/sample9.aspx)]

Outre le `AnonymousTemplate` et `LoggedInTemplate`, le contrôle LoginView peut inclure *RoleGroups*, qui sont des modèles de rôle spécifique. Chaque RoleGroup contient une propriété unique, `Roles`, qui spécifie les rôles le RoleGroup s’applique à. Le `Roles` propriété peut être définie à un rôle unique (par exemple, « administrateurs ») ou à une liste délimitée par des virgules des rôles (par exemple, « administrateurs, les superviseurs »).

Pour gérer les RoleGroups, cliquez sur le lien « Modifier les RoleGroups » à partir de balise du contrôle active pour afficher l’éditeur de collections RoleGroup. Ajoutez deux RoleGroups de nouveau. Définir le RoleGroup première `Roles` à la propriété « Administrateurs » et le second à « Superviseurs ».


[![Gérer les modèles de rôle spécifique de la vue de connexion via l’éditeur de collections RoleGroup.](role-based-authorization-vb/_static/image23.png)](role-based-authorization-vb/_static/image22.png)

**Figure 8**: gestion spécifiques au rôle modèles via l’éditeur de la vue de connexion de collections RoleGroup ([cliquez pour afficher l’image en taille réelle](role-based-authorization-vb/_static/image24.png))


Cliquez sur OK pour fermer l’éditeur de collections RoleGroup ; Cela met à jour le balisage déclaratif de la vue de connexion pour inclure un `<RoleGroups>` section avec un `<asp:RoleGroup>` élément enfant pour chaque RoleGroup défini dans l’éditeur de collections RoleGroup. En outre, la liste déroulante « Vues » liste dans la fonction de balise active de la vue de connexion - listées initialement uniquement le `AnonymousTemplate` et `LoggedInTemplate` – inclut désormais les RoleGroups ajoutés également.

Modifier les RoleGroups afin que les utilisateurs dans le rôle superviseurs les instructions affichées sur la façon de modifier des comptes d’utilisateurs, tandis que les utilisateurs du rôle Administrateurs sont des instructions pour la modification et suppression. Après avoir apporté ces modifications, balisage déclaratif de votre LoginView doit ressembler à ce qui suit.

[!code-aspx[Main](role-based-authorization-vb/samples/sample10.aspx)]

Après avoir apporté ces modifications, enregistrez la page, puis vous l’accédez via un navigateur. Tout d’abord, visitez la page en tant qu’utilisateur anonyme. Vous montrez le message, « vous n’êtes pas connecté au système. Par conséquent, vous ne pouvez pas modifier ou supprimer toutes les informations utilisateur. » Ensuite vous connecter en tant qu’un utilisateur authentifié, mais qui n’est ni dans le rôle superviseurs ni les administrateurs. Cette fois, vous devez voir le message « vous n’êtes pas membre des rôles superviseurs ou les administrateurs. Par conséquent, vous ne pouvez pas modifier ou supprimer toutes les informations utilisateur. »

Ensuite, connectez-vous en tant qu’utilisateur membre du rôle superviseurs. Cette fois, vous devez voir les superviseurs spécifiques au rôle de message (voir Figure 9). Et si vous vous connectez en tant qu’utilisateur dans les administrateurs de rôle, vous devez voir les administrateurs de rôle spécifiques du message (voir Figure 10).


[![Bruce voit le Message de spécifiques au rôle superviseurs](role-based-authorization-vb/_static/image26.png)](role-based-authorization-vb/_static/image25.png)

**Figure 9**: Bruce voit le Message de spécifiques au rôle superviseurs ([cliquez pour afficher l’image en taille réelle](role-based-authorization-vb/_static/image27.png))


[![Tito voit le Message de spécifiques au rôle Administrateurs](role-based-authorization-vb/_static/image29.png)](role-based-authorization-vb/_static/image28.png)

**Figure 10**: Tito voit le Message de spécifiques au rôle Administrateurs ([cliquez pour afficher l’image en taille réelle](role-based-authorization-vb/_static/image30.png))


En tant que les captures d’écran dans les Figures 9 et 10 show, la vue de connexion seulement s’affiche un modèle, même si plusieurs modèles s’appliquent. Bruce et Tito sont à la fois les utilisateurs connectés, mais la vue de connexion affiche uniquement le correspondante RoleGroup et non le `LoggedInTemplate`. En outre, Tito appartient aux rôles les administrateurs et les superviseurs, mais le contrôle LoginView effectue le rendu du modèle de spécifiques au rôle d’administrateurs au lieu des superviseurs une.

Figure 11 illustre le flux de travail utilisé par le contrôle LoginView pour déterminer quel modèle de rendu. Notez que si plusieurs RoleGroup spécifié, le modèle LoginView rend le *premier* RoleGroup qui correspond à. En d’autres termes, si nous avions placé le RoleGroup superviseurs en tant que la première RoleGroup et les administrateurs en tant que le second, puis lorsque Tito visité cette page il voyez le message superviseurs.


[![Flux de travail du contrôle LoginView pour déterminer quel modèle de rendu](role-based-authorization-vb/_static/image32.png)](role-based-authorization-vb/_static/image31.png)

**Figure 11**: flux de travail du contrôle LoginView pour déterminer quel modèle rendu ([cliquez pour afficher l’image en taille réelle](role-based-authorization-vb/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Limitation par programmation des fonctionnalités

Tandis que le contrôle LoginView affiche des instructions différentes en fonction du rôle de l’utilisateur accédant à la page, les boutons Modifier et Annuler restent visibles à tous. Nous devons masquer par programmation les boutons Modifier et supprimer pour les visiteurs anonymes et les utilisateurs figurant dans le rôle Administrateurs ni superviseurs. Nous avons besoin de masquer le bouton Supprimer pour tous les utilisateurs n’est pas un administrateur. Pour ce faire, nous allons écrire un peu de code qui référence par programmation le CommandField modifier et supprimer le LinkButton et définit leur `Visible` propriétés à `False`, si nécessaire.

Le moyen le plus simple de référencer par programmation des contrôles dans un CommandField doit tout d’abord le convertir en modèle. Pour ce faire, cliquez sur le lien « Modifier les colonnes » à partir de la balise active le contrôle GridView, sélectionnez le CommandField dans la liste de champs en cours, cliquez sur le lien « Convertir ce champ en TemplateField ». Cela transforme le CommandField en TemplateField avec un `ItemTemplate` et `EditItemTemplate`. Le `ItemTemplate` contient le modifier et supprimer le LinkButton lors de la `EditItemTemplate` héberge la mise à jour et annuler le LinkButton.


[![Convertir le CommandField en TemplateField](role-based-authorization-vb/_static/image35.png)](role-based-authorization-vb/_static/image34.png)

**Figure 12**: convertir le CommandField dans TemplateField de contenu ([cliquez pour afficher l’image en taille réelle](role-based-authorization-vb/_static/image36.png))


Mettre à jour le modifier et supprimer le LinkButton dans le `ItemTemplate`, ce qui affecte leur `ID` les valeurs de propriétés `EditButton` et `DeleteButton`, respectivement.

[!code-aspx[Main](role-based-authorization-vb/samples/sample11.aspx)]

Chaque fois que les données sont liées au GridView, le contrôle GridView énumère les enregistrements dans son `DataSource` propriété et génère un correspondant `GridViewRow` objet. Comme chaque `GridViewRow` objet est créé, le `RowCreated` événement est déclenché. Pour masquer les boutons Modifier et supprimer des utilisateurs non autorisés, nous devons créer un gestionnaire d’événements pour cet événement et le référencer par programme le modifier et supprimer LinkButton, en définissant leurs `Visible` propriétés en conséquence.

Créer un gestionnaire d’événements le `RowCreated` événement, puis ajoutez le code suivant :

[!code-vb[Main](role-based-authorization-vb/samples/sample12.vb)]

N’oubliez pas que le `RowCreated` événement est déclenché pour *tous les* des lignes GridView, y compris l’en-tête, le pied de page, l’interface de récepteur de radiomessagerie et ainsi de suite. Nous ne souhaitons pas faire référence par programme le modifier et supprimer le LinkButton si nous avons affaire à une ligne de données, pas en mode édition (étant donné que la ligne en mode édition comporte des boutons de mise à jour et annuler au lieu de modifier et supprimer). Cette vérification est gérée par le `If` instruction.

Si nous avons affaire à une ligne de données qui n’est pas en mode édition, le modifier et supprimer le LinkButton sont référencé et leurs `Visible` propriétés sont définies par les valeurs booléennes retournés par la `User` l’objet `IsInRole(roleName)` (méthode). Le `User` objet fait référence à l’entité de sécurité créée par le `RoleManagerModule`; par conséquent, le `IsInRole(roleName)` méthode utilise l’API de rôles pour déterminer si le visiteur actuel appartient à *roleName*.

> [!NOTE]
> Nous pourrions avoir utilisé la classe rôles directement, en remplaçant l’appel à `User.IsInRole(roleName)` avec un appel à la [ `Roles.IsUserInRole(roleName)` méthode](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx). J’ai décidé d’utiliser l’objet principal `IsInRole(roleName)` méthode dans cet exemple, car il est plus efficace que l’utilisation de l’API de rôles directement. Précédemment dans ce didacticiel, nous avons configuré le Gestionnaire de rôles pour mettre en cache des rôles de l’utilisateur dans un cookie. Cette mise en cache des données de cookie est utilisé uniquement lorsque l’entité de sécurité `IsInRole(roleName)` méthode est appelée ; les appels directs à l’API de rôles impliquent toujours un voyage au magasin de rôles. Même si les rôles ne sont pas mises en cache dans un cookie, l’appel de l’objet principal `IsInRole(roleName)` méthode est généralement plus efficace, car lorsqu’elle est appelée pour la première fois lors d’une requête met en cache les résultats. L’API de rôles, en revanche, n’effectue aucune mise en cache. Étant donné que le `RowCreated` événement est déclenché une fois pour chaque ligne dans le contrôle GridView, à l’aide de `User.IsInRole(roleName)` implique qu’un seul aller-retour vers le magasin de rôles, tandis que `Roles.IsUserInRole(roleName)` requiert *N* courses, où *N* est le nombre de comptes d’utilisateur affiché dans la grille.


Le bouton Modifier `Visible` propriété est définie sur `True` si l’utilisateur visite cette page est dans le rôle Administrateurs ou les superviseurs ; sinon, elle a la valeur `False`. Le bouton Supprimer `Visible` propriété est définie sur `True` uniquement si l’utilisateur est dans le rôle Administrateurs.

Testez cette page via un navigateur. Si vous visitez la page sous la forme d’un visiteur anonyme ou un utilisateur qui n’est ni un superviseur, ni un administrateur, le CommandField est vide. Il en existe, mais comme un éclat mince sans le modifier ou supprimer des boutons.

> [!NOTE]
> Il est possible de masquer le CommandField complètement quand un non-superviseur et un non-administrateur visite la page. Je laisse cela en guise d’exercice pour le lecteur.


[![La modifier et supprimer des boutons sont masqués pour les Non-superviseurs et par les non-administrateurs](role-based-authorization-vb/_static/image38.png)](role-based-authorization-vb/_static/image37.png)

**Figure 13**: le modifier et supprimer des boutons sont masqués pour les Non-superviseurs et les utilisateurs non-administrateurs ([cliquez pour afficher l’image en taille réelle](role-based-authorization-vb/_static/image39.png))


Si un utilisateur qui appartient au rôle superviseurs (mais pas au rôle Administrateurs) visite, il voit uniquement le bouton Modifier.


[![Alors que le bouton Modifier est disponible pour les superviseurs, le bouton Supprimer est masqué.](role-based-authorization-vb/_static/image41.png)](role-based-authorization-vb/_static/image40.png)

**Figure 14**: alors que le bouton Modifier est disponible pour les superviseurs, le bouton Supprimer est masqué ([cliquez pour afficher l’image en taille réelle](role-based-authorization-vb/_static/image42.png))


Et si un administrateur visite, il a accès à ces deux boutons Modifier et supprimer.


[![La modifier et supprimer des boutons sont disponibles uniquement pour les administrateurs](role-based-authorization-vb/_static/image44.png)](role-based-authorization-vb/_static/image43.png)

**Figure 15**: le modifier et supprimer des boutons sont disponibles uniquement pour les administrateurs ([cliquez pour afficher l’image en taille réelle](role-based-authorization-vb/_static/image45.png))


## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>Étape 3 : Appliquer des règles d’autorisation basées sur le rôle à des Classes et méthodes

À l’étape 2, que nous avons limité modifier les fonctionnalités aux utilisateurs dans les rôles superviseurs et les administrateurs et supprimer des fonctionnalités pour les administrateurs uniquement. Cela a été accompli en masquant les éléments d’interface utilisateur associé pour les utilisateurs non autorisés via des techniques de programmation. Ces mesures ne garantissent pas qu’un utilisateur non autorisé ne pourront pas être effectuer une action privilégiée. Les éléments d’interface utilisateur qui sont ajoutées plus tard ou peut-être que nous avons oublié de masquer aux utilisateurs non autorisés. Ou bien, un pirate peut découvrir une autre façon pour obtenir la page ASP.NET pour exécuter la méthode de votre choix.

Un moyen simple pour vous assurer qu’une partie spécifique des fonctionnalités ne peut pas être accessible par un utilisateur non autorisé consiste à décorer cette classe ou une méthode avec le [ `PrincipalPermission` attribut](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Lorsque le runtime .NET utilise une classe ou exécute l’une de ses méthodes, il vérifie que le contexte de sécurité actuel a l’autorisation. Le `PrincipalPermission` attribut fournit un mécanisme par lequel nous pouvons définir ces règles.

Nous avons examiné à l’aide de la `PrincipalPermission` d’attribut dans le <a id="_msoanchor_9"> </a> [ *autorisation basée sur l’utilisateur* ](../membership/user-based-authorization-vb.md) didacticiel. Plus précisément, nous avons vu comment décorer le GridView `SelectedIndexChanged` et `RowDeleting` Gestionnaire d’événements afin qu’ils peuvent uniquement être exécutées par les utilisateurs authentifiés et Tito, respectivement. Le `PrincipalPermission` attribut fonctionne aussi bien avec les rôles.

Nous allons illustrer l’utilisation de la `PrincipalPermission` attribut sur le contrôle GridView `RowUpdating` et `RowDeleting` gestionnaires d’événements pour interdire l’exécution pour les utilisateurs non autorisés. Tout nous devons faire est d’ajouter l’attribut approprié située en haut de chaque définition de fonction :

[!code-vb[Main](role-based-authorization-vb/samples/sample13.vb)]

L’attribut pour le `RowUpdating` Gestionnaire d’événements indique que seuls les utilisateurs aux rôles Administrateurs ou les superviseurs peuvent exécuter le Gestionnaire d’événements, alors que l’attribut sur le `RowDeleting` Gestionnaire d’événements limite l’exécution aux utilisateurs dans les administrateurs rôle.


> [!NOTE]
> Le `PrincipalPermission` attribut est représenté en tant que classe dans le `System.Security.Permissions` espace de noms. Veillez à ajouter un `Imports System.Security.Permissions` instruction en haut de votre fichier de classe code-behind pour importer cet espace de noms.


Si, d’une certaine manière, un utilisateur non-administrateur tente d’exécuter le `RowDeleting` Gestionnaire d’événements ou si un tentatives non superviseur ou non administrateur pour exécuter le `RowUpdating` Gestionnaire d’événements, le runtime .NET déclenchera un `SecurityException`.


[![Si le contexte de sécurité n’est pas autorisé à exécuter la méthode, une SecurityException est levée.](role-based-authorization-vb/_static/image47.png)](role-based-authorization-vb/_static/image46.png)

**Figure 16**: si le contexte de sécurité n’est pas autorisé à exécuter la méthode, un `SecurityException` est levée ([cliquez pour afficher l’image en taille réelle](role-based-authorization-vb/_static/image48.png))


En plus de pages ASP.NET, de nombreuses applications ont également une architecture qui inclut les différentes couches, telles que la logique métier et couches d’accès aux données. Ces couches sont généralement implémentés en tant que bibliothèques de classes et offrent des classes et méthodes permettant d’exécuter la logique et les données relatives des fonctionnalités métier. Le `PrincipalPermission` attribut est utile pour appliquer des règles d’autorisation à ces couches également.

Pour plus d’informations sur l’utilisation de la `PrincipalPermission` attribut à définir des règles d’autorisation sur les classes et méthodes, reportez-vous à [Scott Guthrie](https://weblogs.asp.net/scottgu/)d’entrée de blog [Ajout de règles d’autorisation pour l’entreprise et les couches de données à l’aide de `PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, nous avons vu comment spécifier grossier et règles d’autorisation de granularité fine basée sur les rôles de l’utilisateur. ASP. Fonctionnalité de l’autorisation d’URL de NET permet à un développeur de page spécifier quelles identités sont accès autorisées ou refusées pour les pages. Comme nous l’avons vu dans la <a id="_msoanchor_10"> </a> [ *autorisation basée sur l’utilisateur* ](../membership/user-based-authorization-vb.md) (didacticiel), l’autorisation d’URL règles peuvent être appliquées sur une base utilisateur par l’utilisateur. Ils peuvent également être appliqués à une base de rôle par rôle, comme nous l’avons vu à l’étape 1 de ce didacticiel.

Règles d’autorisation de grain fin peuvent être appliqués de manière déclarative ou par programmation. À l’étape 2, nous avons vu à l’aide de la fonctionnalité de RoleGroups du contrôle LoginView pour afficher la sortie différents en fonction des rôles de l’utilisateur visite. Nous avions également étudié comment déterminer par programmation si un utilisateur appartient à un rôle spécifique et comment ajuster les fonctionnalités de la page en conséquence.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Ajout de règles d’autorisation à l’entreprise et les couches de données à l’aide de `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Examen d’ASP.NET de 2.0 l’appartenance, rôles et profil : utilisation des rôles](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [Liste de questions de sécurité pour ASP.NET 2.0](https://msdn.microsoft.com/library/ms998375.aspx)
- [Documentation technique pour le `<roleManager>` élément](https://msdn.microsoft.com/library/ms164660.aspx)

### <a name="about-the-author"></a>À propos de l’auteur

Scott Mitchell, auteur de plusieurs livres sur ASP/ASP.NET et fondateur de 4GuysFromRolla.com, travaille avec les technologies Web Microsoft depuis 1998. Scott fonctionne comme un consultant indépendant, formateur et writer. Son dernier ouvrage est *[SAM animer vous-même ASP.NET 2.0 des dernières 24 heures](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott peut être atteint à [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou via son blog à [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Remerciements particuliers à...

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Les réviseurs tête pour ce didacticiel incluent Suchi Banerjee et Teresa Murphy. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Précédent](assigning-roles-to-users-vb.md)
