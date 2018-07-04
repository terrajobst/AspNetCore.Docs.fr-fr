---
uid: whitepapers/overview
title: Livres blancs | Microsoft Docs
author: rick-anderson
description: Sur cette page, vous trouverez des livres blancs pour vous aider à installer et configurer ASP.NET et pour vous aider à écrire des applications ASP.NET sécurisées, rapides et flexibles.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/15/2011
ms.topic: article
ms.assetid: d5e79470-01f2-4d65-8077-11c3e10a6784
ms.technology: ''
msc.legacyurl: /whitepapers
msc.type: content
ms.openlocfilehash: 3f0c56ed0871df7985a93dd75653e599ea4be582
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363330"
---
<a name="whitepapers"></a>Livres blancs
====================
> Sur cette page, vous trouverez des livres blancs pour vous aider à installer et configurer ASP.NET et pour vous aider à écrire des applications ASP.NET sécurisées, rapides et flexibles.
> 
> - [ASP.NET 4](#aspnet4)
> - [Livres blancs de sécurité ASP.NET](#security)
> - [Installation et des livres blancs sur le programme d’installation](#setup)
> - [Livres blancs SQL Server](#sql)
> - [Livres blancs général](#general)


<a id="aspnet4"></a>
## <a name="aspnet-4"></a>ASP.NET 4

Informations relatives à ASP.NET 4 et Visual Studio 2010.

[Notes de publication d’ASP.NET MVC 4](mvc4-beta-release-notes.md "mvc4--notes de publication")

Ce document décrit les nouvelles fonctionnalités et améliorations introduites dans l’aperçu pour développeurs ASP.NET MVC 4 pour Visual Studio 2010, ainsi que les notes d’installation et les problèmes connus.

[Notes de publication d’ASP.NET MVC 3](mvc3-release-notes.md "mvc3--notes de publication")

Ce document décrit les nouvelles fonctionnalités et améliorations introduites dans ASP.NET MVC 3, ainsi que les notes d’installation et les problèmes connus.

[ASP.NET 4 et Visual Studio 2010 Web présentation du développement](aspnet4/index.md "aspnet4")

Dans le .NET Framework version 4 proviennent de nombreuses évolutions pour ASP.NET. Ce document donne une vue d’ensemble d’un grand nombre des nouvelles fonctionnalités qui sont incluses dans la version à venir.

[ASP.NET 4 Beta 2 dernières modifications](aspnet4/breaking-changes.md "des modifications avec rupture")

Ce document décrit les modifications qui ont été apportées pour la version de .NET Framework 4 version bêta 2 (autrement dit, la version d’ASP.NET 4 Beta 2) qui peut potentiellement affecter les applications qui ont été créées à l’aide de versions antérieures, y compris la version ASP.NET 4 Bêta 1.

[Quelles sont les nouveautés dans ASP.NET MVC 2](what-is-new-in-aspnet-mvc.md "Nouveautés aspnet mvc")

Ce document décrit les nouvelles fonctionnalités et améliorations introduites dans ASP.NET MVC 2.

[La mise à niveau une Application ASP.NET MVC 1.0 vers ASP.NET MVC 2](aspnet-mvc2-upgrade-notes.md "aspnet-mvc2-mise à niveau-notes de publication")

ASP.NET MVC 2 peut être installé côte à côte avec ASP.NET MVC 1.0 sur le même serveur. Flexibilité application aux développeurs de choisir quand mettre à niveau une application ASP.NET MVC 1.0 vers ASP.NET MVC 2. Ce décrit de document à la fois la mise à niveau manuellement et avec un Assistant dans Visual c#...

<a id="security"></a>
## <a name="aspnet-security-whitepapers"></a>Livres blancs de sécurité ASP.NET

La sécurité est un aspect important des applications internet, et ces livres blancs expliquent comment concevoir et implémenter des applications ASP.NET sécurisées.

[Instrumenter ASP.NET 2.0 Applications pour la sécurité](https://msdn.microsoft.com/library/ms998325.aspx)

Cet article explique comment utiliser des événements de contrôle d’intégrité personnalisées pour instrumenter votre application ASP.NET pour effectuer le suivi des opérations et les événements de sécurité. ASP.NET version 2.0 fournit la surveillance de l’état comprend l’instrumentation pour l’édition standard de nombreux...

[Effectuer une révision de déploiement de sécurité pour ASP.NET 2.0](https://msdn.microsoft.com/library/ms998367.aspx)

Cet article explique comment effectuer une révision de déploiement de sécurité pour une application ASP.NET 2.0 identifier les vulnérabilités potentielles introduites par les paramètres de configuration inapproprié. La majorité de l’examen implique la création...

[Utiliser ADAM pour les rôles dans ASP.NET 2.0](https://msdn.microsoft.com/library/ms998331.aspx)

Cet article explique comment vous pouvez développer un site Web ASP.NET qui utilise Active Directory Application Mode (ADAM) pour stocker les rôles ASP.NET. Il vous montre comment configurer ADAM et le magasin de stratégies de gestionnaire d’autorisations (AzMan), comment créer de nouveaux rôles et...

[Utiliser le Gestionnaire d’autorisations (AzMan) avec ASP.NET 2.0](https://msdn.microsoft.com/library/ms998336.aspx)

Cet article explique comment utiliser le Gestionnaire d’autorisations (AzMan) conjointement avec le Gestionnaire de rôles ASP.NET pour gérer les rôles, vérifier l’appartenance du rôle utilisateur et autoriser les rôles d’opérations spécifiques sur un magasin de stratégies d’AzMan. La procédure...

[Utiliser l’appartenance dans ASP.NET 2.0](https://msdn.microsoft.com/library/ms998347.aspx)

Cet article explique comment utiliser la fonctionnalité d’appartenance dans les applications ASP.NET version 2.0. Il vous montre comment utiliser les deux fournisseurs d’appartenances différents : ActiveDirectoryMembershipProvider et SqlMembershipProvider. La fonctionnalité d’appartenance...

[Utiliser le Gestionnaire de rôles dans ASP.NET 2.0](https://msdn.microsoft.com/library/ms998314.aspx)

Cet article explique comment utiliser le Gestionnaire de rôles ASP.NET 2.0. Le Gestionnaire de rôles facilite la gestion des rôles et performant d’autorisation en fonction du rôle dans votre application. Il montre comment configurer les différents fournisseurs de rôles pour une utilisation avec votre...

[Utiliser l’authentification Windows dans ASP.NET 2.0](https://msdn.microsoft.com/library/ms998358.aspx)

Cet article explique comment configurer et utiliser l’authentification Windows dans une application Web ASP.NET. L’authentification Windows est l’approche par défaut chaque fois que les utilisateurs font partie de votre domaine Windows. Cette approche vous permet d’utiliser un magasin d’identités existants...

[Effectuer une révision de Code de sécurité pour le Code managé (activité de base)](https://msdn.microsoft.com/library/ms998364.aspx)

Cet article explique comment effectuer des révisions de code de sécurité. Ce module présente les étapes impliquées dans l’activité et les techniques d’analyse des résultats. Utilisez cette procédure avec « liste de questions de sécurité : Code (.NET Framework 2.0) managé »...

[Effectuer une révision de déploiement de sécurité pour ASP.NET 2.0](https://msdn.microsoft.com/library/ms998367.aspx)

Cet article explique comment effectuer une révision de déploiement de sécurité pour une application ASP.NET 2.0 identifier les vulnérabilités potentielles introduites par les paramètres de configuration inapproprié. La majorité de l’examen implique la création...

[Implémenter la délégation contrainte Kerberos pour Windows 2000](https://msdn.microsoft.com/library/aa302400.aspx)

Délégation Kerberos contrainte vous permet une identité authentifiée à partir de plusieurs niveaux physiques d’une application pour prendre en charge d’autorisation et authentification en aval. Cette procédure montre que vous les étapes de configuration requises pour y parvenir.

[Utiliser l’emprunt d’identité et la délégation dans ASP.NET 2.0](https://msdn.microsoft.com/library/ms998351.aspx)

Cet article explique comment et quand vous devez utiliser l’emprunt d’identité dans les applications ASP.NET 2.0. Par défaut, l’emprunt d’identité est désactivé, et vous pouvez accéder aux ressources en utilisant l’identité du processus de l’application Web ASP.NET. Toutefois, vous pouvez utiliser...

[Créer un modèle de menace pour une Application Web au moment du Design](https://msdn.microsoft.com/library/ms978527.aspx)

Cette procédure décrit une approche pour la création d’un modèle de menace pour une application Web. L’activité de modélisation des menaces vous aide à modéliser votre conception de la sécurité de sorte que vous pouvez exposer des défauts de conception de sécurité potentielles et les vulnérabilités avant d’investir...

### <a name="forms-authentication"></a>Authentification par formulaire

[Protéger l’authentification par formulaire dans ASP.NET 2.0](https://msdn.microsoft.com/library/ms998310.aspx)

Cet article explique comment configurer et utiliser l’authentification par formulaire avec les applications ASP.NET 2.0 en toute sécurité. Les facteurs clés à prendre en compte incluent correctement sécurisation le ticket d’authentification et de sécurisation du magasin d’identités utilisateur et les accès à ce magasin. ...

[Utiliser l’authentification par formulaires avec Active Directory dans ASP.NET 2.0](https://msdn.microsoft.com/library/ms998360.aspx)

Cet article explique comment utiliser l’authentification par formulaire avec le service d’annuaire Microsoft® Active Directory® à l’aide d’ActiveDirectoryMembershipProvider. L’article explique comment configurer le fournisseur et de créer et d’authentifier les utilisateurs...

[Utiliser l’authentification par formulaires avec Active Directory dans plusieurs domaines dans ASP.NET 2.0](https://msdn.microsoft.com/library/ms998345.aspx)

Cet article explique comment utiliser l’authentification par formulaire avec le service d’annuaire Microsoft® Active Directory® à l’aide d’ActiveDirectoryMembershipProvider. L’article explique comment configurer le fournisseur et de créer et d’authentifier les utilisateurs...

[Utiliser l’authentification par formulaires avec SQL Server dans ASP.NET 2.0](https://msdn.microsoft.com/library/ms998317.aspx)

Cet article explique comment vous pouvez utiliser l’authentification par formulaire avec le fournisseur d’appartenances SQL Server. L’authentification par formulaires avec SQL Server n’est plus applicable dans les situations où les utilisateurs de votre application ne sont pas partie de votre domaine Windows, et par conséquent,...

[Créer des objets de type GenericPrincipal avec l’authentification par formulaire dans ASP.NET 1.1](https://msdn.microsoft.com/library/aa302399.aspx)

Cet article explique comment créer et gérer des objets GenericPrincipal et FormsIdentity lorsque vous utilisez l’authentification par formulaire.

[Utiliser l’authentification par formulaires avec Active Directory dans ASP.NET 1.1](https://msdn.microsoft.com/library/aa302397.aspx)

Cet article vous montre comment implémenter l’authentification par formulaire sur un magasin d’informations d’identification Active Directory.

[Utiliser l’authentification par formulaires avec SQL Server dans ASP.NET 1.1](https://msdn.microsoft.com/library/aa302398.aspx)

Cet article explique comment implémenter l’authentification par formulaire sur une banque d’informations d’identification SQL Server. Il montre également comment stocker des résumés de mot de passe dans la base de données.

### <a name="user-input-data-validation"></a>Validation des données d’entrée utilisateur

[Validation des demandes - prévention des attaques de Script](request-validation.md "validation de la demande")

Cet article décrit la fonctionnalité de validation de demande d’ASP.NET où, par défaut, l’application ne peut pas traiter du contenu HTML non codé envoyé au serveur. Cette fonctionnalité de validation de demande peut être désactivée lors de l’application a été...

[Empêcher les scripts inter-sites dans ASP.NET](https://msdn.microsoft.com/library/ms998274.aspx)

Cet article explique comment vous pouvez protéger vos applications ASP.NET contre les attaques de script entre sites à l’aide de techniques de validation d’entrée correcte et par l’encodage de la sortie. Elle décrit également un nombre d’autres mécanismes de protection que vous pouvez utiliser dans...

[Protéger contre une Injection SQL dans ASP.NET](https://msdn.microsoft.com/library/ms998271.aspx)

Cet article explique de plusieurs façons de protéger votre application ASP.NET contre les attaques par injection SQL. L’injection SQL peut se produire lorsqu’une application utilise l’entrée pour construire des instructions SQL dynamiques ou quand il utilise des procédures stockées pour se connecter à la...

[Utiliser des Expressions régulières pour limiter l’entrée dans ASP.NET](https://msdn.microsoft.com/library/ms998267.aspx)

Cet article explique comment vous pouvez utiliser des expressions régulières dans des applications ASP.NET pour limiter les entrées non approuvées. Les expressions régulières constituent un bon moyen pour valider les champs de texte tels que les noms, adresses, numéros de téléphone et autres informations utilisateur. Vous pouvez utiliser...

### <a name="code-access-security"></a>Sécurité d'accès du code

[Utiliser la sécurité d’accès du Code dans ASP.NET 2.0](https://msdn.microsoft.com/library/ms998326.aspx)

Cet article explique comment sélectionner un niveau de confiance approprié pour votre application et le cas échéant, comment créer un fichier de stratégie de sécurité ASP.NET code accès personnalisé à définir un personnalisé niveau de confiance. Vous pouvez utiliser un code différent accès sécurité confiance...

[Créer une autorisation de cryptage personnalisée](https://msdn.microsoft.com/library/aa302362.aspx)

Cet article explique comment créer une autorisation de sécurité des accès de code personnalisé pour contrôler l’accès par programme aux fonctionnalités de cryptage non gérée qui fournit des API Win32® Data Protection (DPAPI). Utilisez cette autorisation personnalisée avec le wrapper DPAPI géré...

[Utiliser la stratégie de sécurité d’accès au Code pour limiter un Assembly](https://msdn.microsoft.com/library/aa302361.aspx)

Un administrateur peut configurer la stratégie de sécurité d’accès au code pour limiter les opérations de code .NET Framework (assemblys). Dans cette procédure, vous configurer la stratégie de sécurité d’accès au code pour limiter la capacité d’un assembly à effectuer d’e/s de fichier et de restreindre...

### <a name="communications-security"></a>Sécurité des communications

[Configuration de SSL sur un serveur Web](https://msdn.microsoft.com/library/aa302411.aspx)

Un serveur Web doit être configuré pour SSL afin de prendre en charge les connexions https à partir d’applications clientes. Cet article explique comment configurer SSL sur un serveur Web.

[Configurer des certificats de Client](https://msdn.microsoft.com/library/aa302412.aspx)

IIS prend en charge l’authentification par certificat client. Cet article explique comment configurer une application Web pour exiger des certificats clients. Il montre également comment installer un certificat sur un ordinateur client et l’utiliser lors de l’appel de l’application Web.

[Utiliser IPSec pour le filtrage des Ports et l’authentification](https://msdn.microsoft.com/library/aa302366.aspx)

Sécurité du protocole Internet (IPSec) est un protocole, pas un service, qui fournit le chiffrement, d’intégrité et les services d’authentification pour le trafic réseau basé sur IP. Étant donné que IPSec offre une protection de serveur à serveur, vous pouvez utiliser IPSec pour contrer les menaces internes de...

[Utiliser IPSec pour fournir une Communication sécurisée entre deux serveurs](https://msdn.microsoft.com/library/aa302413.aspx)

IPSec est une technologie fournie par Windows 2000 qui vous permet de créer des canaux chiffrés entre deux serveurs. IPSec peut être utilisé pour filtrer le trafic IP et pour authentifier les serveurs. Cet article explique comment configurer IPSec pour fournir une (crypté) sécurisé...

[Utiliser SSL pour sécuriser les communications avec SQL Server](https://msdn.microsoft.com/library/aa302414.aspx)

Il est souvent essentiel pour les applications être en mesure de sécuriser les données passées vers et depuis un serveur de base de données SQL Server. Avec SQL Server, vous pouvez utiliser SSL pour créer un canal chiffré. Cet article explique comment installer un certificat sur le serveur de base de données...

[Appeler un Service Web à l’aide de certificats Client via ASP.NET 1.1](https://msdn.microsoft.com/library/aa302408.aspx)

Cet article explique comment vous pouvez transmettre un certificat client à un service Web pour l’authentification à partir d’une application Web ASP.NET ou d’une application Windows Forms. Vous pouvez installer le certificat client dans le magasin ordinateur local ou dans le magasin d’utilisateurs. If...

[Appeler un Service Web à l’aide de SSL à partir d’ASP.NET 1.1](https://msdn.microsoft.com/library/aa302409.aspx)

Le chiffrement Secure Sockets Layer (SSL) permettre être utilisé pour garantir l’intégrité et la confidentialité des messages transmis vers et depuis un service Web. Cet article explique comment utiliser le protocole SSL avec les services Web.

### <a name="cryptography"></a>Chiffrement

[Créer une bibliothèque DPAPI dans .NET Framework 1.1](https://msdn.microsoft.com/library/aa302402.aspx)

Cet article explique comment créer une bibliothèque de classes gérée qui expose la fonctionnalité DPAPI aux applications qui veulent pour chiffrer les données, par exemple, les chaînes de connexion de base de données et les informations d’identification de compte.

[Créer une bibliothèque de cryptage dans .NET Framework 1.1](https://msdn.microsoft.com/library/aa302405.aspx)

Cet article explique comment créer une bibliothèque de classes managée pour fournir des fonctionnalités de chiffrement pour les applications. Il permet à une application choisir l’algorithme de chiffrement. Algorithmes pris en charge incluent DES, Triple DES, RC2 et Rijndael.

[Store d’une chaîne de connexion cryptée dans le Registre dans ASP.NET 1.1](https://msdn.microsoft.com/library/aa302406.aspx)

Applications peuvent choisir de stocker des données chiffrées telles que des chaînes de connexion et les informations d’identification de compte dans le Registre Windows. Cet article explique comment stocker et récupérer les chaînes chiffrées dans le Registre.

[Utiliser DPAPI (Machine Store) à partir d’ASP.NET 1.1](https://msdn.microsoft.com/library/aa302403.aspx)

Cet article explique comment utiliser DPAPI à partir d’une application Web ASP.NET ou un service Web pour chiffrer les données sensibles.

[Utiliser DPAPI (utilisateur Store) à partir d’ASP.NET 1.1 avec Enterprise Services](https://msdn.microsoft.com/library/aa302404.aspx)

Cet article explique comment utiliser DPAPI à partir d’une application Web ASP.NET ou un service pour chiffrer les données sensibles. Cette procédure utilise DPAPI avec le magasin de l’utilisateur, qui requiert l’utilisation d’un processus de composant Enterprise Services à l’emploi.

<a id="setup"></a>
## <a name="installation-and-setup-whitepapers"></a>Installation et des livres blancs sur le programme d’installation

Ces livres blancs fournissent des instructions pas à pas pour l’installation et configuration d’ASP.NET sur votre serveur.

[Créer un compte de Service pour ASP.NET 2.0 Application](https://msdn.microsoft.com/library/ms998297.aspx)

Cet article explique comment créer et configurer un compte de service avec des privilèges minimum personnalisé pour exécuter une application Web ASP.NET. Par défaut, une application ASP.NET sur Microsoft Windows Server 2003 et IIS 6.0 s’exécute à l’aide du Service réseau intégré...

[Améliorer la sécurité lors de l’hébergement de plusieurs Applications dans ASP.NET 2.0](https://msdn.microsoft.com/library/aa480478.aspx)

Cet article explique comment vous pouvez isoler plusieurs applications à partir de l’autre et des ressources système partagées d’un site Web environnement d’hébergement. L’environnement d’hébergement peut être un serveur Web fourni par un fournisseur de services de Internet (ISP) qui héberge plusieurs...

[Utiliser la confiance moyenne dans ASP.NET 2.0](https://msdn.microsoft.com/library/ms998341.aspx)

Cet article explique comment configurer des applications Web ASP.NET pour exécuter en mode de confiance moyenne. Si vous hébergez plusieurs applications sur le même serveur, vous pouvez utiliser la sécurité d’accès du code et le niveau de confiance moyen pour assurer l’isolation de l’application. En définissant...

[Utiliser le compte de Service réseau pour accéder aux ressources dans ASP.NET 2.0](https://msdn.microsoft.com/library/ms998320.aspx)

Cet article explique comment vous pouvez utiliser le compte d’ordinateur NT AUTHORITY\Network Service pour accéder aux locaux et les ressources réseau. Par défaut sur Windows Server 2003, les applications ASP.NET exécutent à l’aide de cette l’identité du compte. C’est un privilège minimum...

[Utiliser la Transition de protocole et la délégation contrainte dans ASP.NET 2.0](https://msdn.microsoft.com/library/ms998355.aspx)

Cet article explique comment configurer et utiliser la transition de protocole et la délégation contrainte pour permettre à votre application ASP.NET à accéder aux ressources réseau en empruntant l’identité de l’appelant d’origine. Système d’exploitation de Microsoft® Windows® 2000...

[L’exécution ASP.NET côte à côte du .NET Framework 1.0 et 1.1](side-by-side-with-10.md "côte à côte avec 1.0")

Ce livre blanc décrit comment installer .NET 1.0 et 1.1 de .NET sur votre ordinateur, ce qui permet une application Web ASP.NET pour s’exécuter sur des versions du framework.

[ASP.NET refusé aux répertoires IIS](denied-access-to-iis-directories.md "refuser l’accès aux répertoires iis")

Ce livre blanc décrit la marche à suivre si une demande à votre application ASP.NET retourne l’erreur, « refuser l’accès à *NomRépertoire* directory. Échec de démarrage de l’analyse des modifications d’annuaire. »

[En cours d’exécution ASP.NET 1.1 avec IIS 6.0](aspnet-and-iis6.md "aspnet et iis6")

Tandis que Windows Server 2003 inclut IIS 6.0 et ASP.NET 1.1, ces composants sont désactivés par défaut. Ce livre blanc décrit comment activer IIS 6.0 et ASP.NET 1.1 et vous recommande de plusieurs paramètres de configuration pour obtenir le meilleur...

[Correctif pour l’erreur de « Application serveur non disponible » après avoir appliqué la mise à jour de sécurité pour Internet Explorer](ms03-32-issue.md "ms03-32-problème")

Cet article décrit le correctif logiciel qui résout un problème avec la mise à jour de sécurité MS03-32 pour Internet Explorer qui affecte les applications ASP.NET 1.0 s’exécutant sur Windows XP Professionnel.

[Créer un compte personnalisé pour exécuter ASP.NET 1.1](https://msdn.microsoft.com/library/aa302396.aspx)

Les applications Web ASP.NET sont généralement pas exécutés à l’aide du compte ASPNET intégré. Parfois, vous souhaiterez utiliser un compte personnalisé à la place. Cet article vous montre comment créer un compte local doté d’autorisations minimales pour exécuter des applications Web ASP.NET. ...

### <a name="configuration"></a>Configuration

[Configurer la clé d’ordinateur dans ASP.NET 2.0](https://msdn.microsoft.com/library/ms998288.aspx)

Cette rubrique montre la &lt;machineKey&gt; élément dans le fichier Web.config et montre comment configurer le &lt;machineKey&gt; élément au contrôle de vérification linguistique de falsification et le chiffrement de l’état d’affichage, de l’authentification par formulaire tickets et les cookies de rôle. État d’affichage est signé...

[Chiffrer les Sections de Configuration dans ASP.NET 2.0 à l’aide de DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)

Cet article explique comment utiliser la fournisseur de configuration protégée de l’interface (DPAPI) et le compte Aspnet de programmation d’application Protection des données Windows\_outil regiis.exe pour chiffrer les sections de vos fichiers de configuration. Vous pouvez utiliser le compte Aspnet\_outil regiis.exe à...

[Chiffrer les Sections de Configuration dans ASP.NET 2.0 à l’aide de RSA](https://msdn.microsoft.com/library/ms998283.aspx)

Cet article explique comment utiliser le fournisseur RSA de la Configuration protégée et le compte Aspnet\_outil regiis.exe pour chiffrer les sections de vos fichiers de configuration. Vous pouvez utiliser Aspnet\_outil regiis.exe pour chiffrer les données sensibles, telles que des chaînes de connexion contenues dans le...

[Utiliser l’emprunt d’identité et la délégation dans ASP.NET 2.0](https://msdn.microsoft.com/library/ms998351.aspx)

Cet article explique comment et quand vous devez utiliser l’emprunt d’identité dans les applications ASP.NET 2.0. Par défaut, l’emprunt d’identité est désactivé, et vous pouvez accéder aux ressources en utilisant l’identité du processus de l’application Web ASP.NET. Toutefois, vous pouvez utiliser...

<a id="sql"></a>
## <a name="sql-server-whitepapers"></a>Livres blancs SQL Server

Bien que ASP.NET fonctionne avec un large éventail de bases de données, ces livres blancs aborder plus spécifiquement la connexion d’applications ASP.NET à SQL Server.

[Se connecter à SQL Server à l’aide de l’authentification SQL dans ASP.NET 2.0](https://msdn.microsoft.com/library/ms998300.aspx)

Cet article vous explique comment connecter une application ASP.NET en toute sécurité à Microsoft® SQL Server™ lors de l’authentification d’accès de base de données utilise l’authentification SQL native. L’authentification Windows est la méthode recommandée pour se connecter à SQL Server, car vous...

[Se connecter à SQL Server à l’aide de l’authentification Windows dans ASP.NET 2.0](https://msdn.microsoft.com/library/ms998292.aspx)

Cet article explique comment se connecter à SQL Server 2000 à l’aide d’un compte de service Windows à partir d’une application ASP.NET version 2.0. Vous devez utiliser l’authentification Windows au lieu de l’authentification SQL chaque fois que possible, car vous évitez de stocker des informations d’identification dans...

[Utiliser SSL pour sécuriser les communications avec SQL Server 2000](https://msdn.microsoft.com/library/aa302414.aspx)

Il est souvent essentiel pour les applications être en mesure de sécuriser les données passées vers et depuis un serveur de base de données SQL Server. Avec SQL Server, vous pouvez utiliser SSL pour créer un canal chiffré. Cet article explique comment installer un certificat sur le serveur de base de données...

<a id="general"></a>
## <a name="general-whitepapers"></a>Livres blancs général

Étude de cas décrit le processus qui a été utilisé pour migrer des sites Web de communauté de Microsoft .NET à partir d’un environnement d’hébergement traditionnel vers Microsoft Azure.

[De migration Microsoft ASP.NET et sites Web IIS.NET communautaire pour Microsoft Azure](https://go.microsoft.com/fwlink/?LinkId=400656)

Ces livres blancs couvrent une variété de sujets concernant ASP.NET.

[Utiliser le contrôle d’intégrité dans ASP.NET 2.0](https://msdn.microsoft.com/library/ms998306.aspx)

Cet article explique comment utiliser le contrôle d’intégrité de l’analyse pour instrumenter votre application pour un événement personnalisé. Pour créer un événement de contrôle d’état personnalisé, vous créez une classe qui dérive de System.Web.Management.WebBaseEvent, configurer le &lt;healthMonitoring&gt; ...

[Implémentez la gestion des correctifs](https://msdn.microsoft.com/library/aa302364.aspx)

Cette rubrique montre gestion des correctifs, y compris comment conserver unique ou plusieurs serveurs à jour. Logiciel supplémentaire n’est pas nécessaire, sauf pour les outils disponibles pour téléchargement à partir de Microsoft. Opérations et la stratégie de sécurité doivent adopter une gestion des correctifs...

[Les contrôles serveur ASP.NET pour Silverlight dans Silverlight 3 SDK](https://go.microsoft.com/fwlink/?LinkId=153377)

Les contrôles serveur ASP.NET pour Silverlight (« contrôles ASP.NET Silverlight »), qui sont les contrôles ASP.NET MediaPlayer et Silverlight, ont été supprimés à partir du SDK de Silverlight pour Silverlight version 3. Ce document fournit des conseils pour les développeurs qui ont travaillé avec ces ASP.NET...

[Création d’Applications Web hautes performances](https://devexpress.com/act)

Découvrez comment utiliser les nouvelles fonctionnalités dans la bibliothèque Ajax ASP.NET pour créer des Applications de Web haute Performance
