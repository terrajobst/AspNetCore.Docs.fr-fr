---
uid: web-forms/what-is-web-forms
title: What ' s Web Forms | Microsoft Docs
author: rick-anderson
description: ''
ms.author: aspnetcontent
ms.date: 02/21/2014
ms.assetid: 5fa1daf9-1161-4cfa-bd4c-658f48b2c229
msc.legacyurl: /web-forms/what-is-web-forms
msc.type: content
ms.openlocfilehash: 06dfc642f2903be1e4928391806fd815d763c903
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823947"
---
<a name="what-is-web-forms"></a>Nouveautés de Web Forms
====================
ASP.NET Web Forms fait partie de l’infrastructure d’application web ASP.NET et est inclus avec [Visual Studio](https://www.asp.net/downloads). C’est un des quatre modèles de programmation que vous pouvez utiliser pour créer des applications web ASP.NET, les autres sont ASP.NET MVC, les Pages Web ASP.NET et les Applications à Page unique ASP.NET.

Web Forms sont des pages qui demandent de vos utilisateurs à l’aide de leur navigateur. Ces pages peuvent être écrites à l’aide d’une combinaison de HTML, client-script, de contrôles serveur et de code serveur. Lorsque des utilisateurs demandent une page, il est compilé et exécuté sur le serveur par l’infrastructure, et l’infrastructure génère ensuite le balisage HTML qui peut être rendus par le navigateur. Une page Web Forms ASP.NET présente des informations à l’utilisateur dans n’importe quel navigateur ou l’appareil client.

À l’aide de Visual Studio, vous pouvez créer des pages Web Forms ASP.NET. L’environnement de développement intégré de Visual Studio (IDE) vous permet de faire glisser des contrôles de serveur pour présenter votre page Web Forms. Vous pouvez ensuite facilement définir propriétés, méthodes et événements pour les contrôles sur la page ou pour la page elle-même. Ces propriétés, les méthodes et les événements sont utilisés pour définir la page web comportement, apparence et ainsi de suite. Pour écrire du code serveur pour gérer la logique de la page, vous pouvez utiliser un langage .NET tels que Visual Basic ou c#.

> [!NOTE] 
> 
> Documentation d’ASP.NET et Visual Studio s’étend sur plusieurs versions. Les rubriques qui présentent les fonctionnalités des versions précédentes peuvent être utiles pour vos tâches en cours et les scénarios à l’aide des versions les plus récentes.


**ASP.NET Web Forms sont :** 

- Basé sur la technologie de Microsoft ASP.NET, dans laquelle code qui s’exécute sur le serveur de manière dynamique génère la sortie de page Web sur le navigateur ou le périphérique client.
- Compatible avec n’importe quel navigateur ou l’appareil mobile. Une page Web ASP.NET restitue automatiquement le HTML conforme au navigateur correct des fonctionnalités telles que les styles, disposition et ainsi de suite.
- Compatible avec n’importe quel langage pris en charge par le common language runtime .NET, tels que Microsoft Visual Basic et Microsoft Visual c#.
- Basé sur Microsoft .NET Framework. Ainsi, tous les avantages de l’infrastructure, y compris un environnement géré, la sécurité de type et l’héritage.
- Flexible, car vous pouvez ajouter créés par l’utilisateur et de contrôles tiers pour eux.

**ASP.NET Web Forms offre :** 

- Séparation du code HTML et autre code d’interface utilisateur à partir de la logique d’application.
- Une suite riche des contrôles de serveur pour les tâches courantes, notamment l’accès aux données.
- Puissantes liaison de données, avec prise en charge de l’outil idéal.
- Prise en charge des scripts côté client qui s’exécute dans le navigateur.
- Prise en charge pour une variété d’autres fonctionnalités, notamment le routage, sécurité, performances, internationalisation, test, le débogage, la gestion des erreurs et la gestion d’état.

## <a name="aspnet-web-forms-helps-you-overcome-challenges"></a>ASP.NET Web Forms vous permet de relever les défis

Programmation d’applications Web présente des défis qui n’est généralement pas confronté lors de la programmation d’applications clientes traditionnelles. Parmi les défis auxquels sont :

- **Implémentation d’une interface utilisateur de Web riches** : il peut être difficile et fastidieux de concevoir et implémenter un utilisateur de l’interface à l’aide des fonctionnalités HTML de base, en particulier si la page a une disposition complexe, une grande quantité de contenu dynamique et complète objets utilisateur interactif.
- **Séparation du client et serveur** -application dans un Web, le client (navigateur) et le serveur sont différents de programmes en cours d’exécution sur des ordinateurs différents (et même sur différents systèmes d’exploitation). Par conséquent, les deux moitiés de l’application partagent très peu d’informations ; ils peuvent communiquer, mais généralement échanger uniquement de petits morceaux de simples informations.
- **L’exécution sans état** : quand un serveur Web reçoit une demande pour une page, il recherche la page, traite, il envoie au navigateur et ignore ensuite toutes les informations de page. Si l’utilisateur demande à nouveau la même page, le serveur répète toute la séquence, le retraitement de la page à partir de zéro. Autrement dit, un serveur n’a aucune mémoire de pages qu’il a traité : page sont sans état. Par conséquent, si une application doit mettre à jour les informations sur une page, sa nature sans état peut devenir un problème.
- **Fonctionnalités du client inconnu** -dans de nombreux cas, les applications Web sont accessibles à plusieurs utilisateurs à l’aide de différents navigateurs. Navigateurs ont des fonctionnalités différentes, rendant difficile de créer une application qui s’exécutera aussi bien sur chacun d'entre eux.
- **Complications avec accès aux données** -lecture à partir d’et en écriture à une source de données dans les applications Web traditionnelles peuvent être complexes et gourmandes en ressources.
- **Complications avec évolutivité** : dans de nombreux cas, les applications Web conçues avec des méthodes existantes échouent pour répondre aux objectifs d’évolutivité en raison du manque de compatibilité entre les différents composants de l’application. Il s’agit souvent un point de défaillance courants pour les applications sous un cycle de croissance importante.

Ces défis pour les applications Web peut nécessiter et fastidieux. ASP.NET Web Forms et l’infrastructure ASP.NET relever ces défis de plusieurs manières :

- **Modèle objet cohérent, intuitive** -infrastructure de page ASP.NET présente un modèle d’objet qui vous permet de considérer vos formulaires en tant qu’unité, et non comme des éléments distincts de client et serveur. Dans ce modèle, vous pouvez programmer la page de façon plus intuitive que dans les applications Web traditionnelles, y compris la possibilité de définir les propriétés d’éléments de la page et répondre aux événements. En outre, les contrôles serveur ASP.NET sont une abstraction du contenu physique d’une page HTML et de l’interaction directe entre le navigateur et le serveur. En règle générale, vous pouvez utiliser les contrôles serveur comme si vous pouvez travailler avec des contrôles dans une application cliente et sans avoir à réfléchir sur la façon de créer le code HTML pour présenter et traiter les contrôles et leur contenu.
- **Modèle de programmation pilotée par événements** -ASP.NET Web Forms offrent aux applications Web le modèle familier d’écriture de gestionnaires d’événements pour les événements qui se produisent sur le serveur ou client. L’infrastructure de page ASP.NET résume ce modèle de sorte que le mécanisme sous-jacent de capture d’un événement sur le client, transmettre au serveur et appeler la méthode appropriée est entièrement automatique et invisible pour vous. Le résultat est une structure de code claire et facile à écrire qui prend en charge le développement piloté par événement.
- **Gestion des états intuitive** : infrastructure de page ASP.NET gère automatiquement la tâche de maintenance de l’état de votre page et ses contrôles, et il vous offre des moyens explicite pour maintenir l’état des informations spécifiques aux applications. Cela est effectué sans utilisation intensive des ressources du serveur et peut être implémenté avec ou sans envoi de cookies au navigateur.
- **Applications indépendantes du navigateur** -infrastructure de page ASP.NET vous permet de créer toute logique d’application sur le serveur, éliminant ainsi la nécessité de coder explicitement pour connaître les différences dans les navigateurs. Toutefois, il toujours afin de pouvoir tirer parti des fonctionnalités spécifiques au navigateur en écrivant du code côté client pour fournir des performances améliorées et une expérience client plus riche.
- **Prise en charge de .NET framework common language runtime** -infrastructure de page ASP.NET repose sur le .NET Framework, l’infrastructure entière est donc disponible pour n’importe quelle application ASP.NET. Vos applications peuvent être écrites dans n’importe quel langage qui est compatible avec le runtime. En outre, l’accès aux données est simplifiée à l’aide de l’infrastructure d’accès de données fourni par le .NET Framework, notamment ADO.NET.
- **Performance des serveurs évolutifs .NET framework** -infrastructure de page ASP.NET vous permet de mettre à l’échelle votre application Web à partir d’un seul ordinateur avec un seul processeur à une batterie de serveurs Web sur plusieurs ordinateurs et sans modifications complexes apportées à l’application logique.

## <a name="features-of-aspnet-web-forms"></a>Fonctionnalités des Web Forms ASP.NET

- **Contrôles serveur**-contrôles serveur Web ASP.NET sont des objets sur les pages Web ASP.NET qui s’exécutent lorsque la page est demandée et ce balisage rendu dans le navigateur. De nombreux contrôles de serveur Web sont semblables à des éléments HTML familiers, tels que des boutons et des zones de texte. Autres contrôles ont un comportement complexe, par exemple un calendrier de contrôles et les contrôles que vous pouvez utiliser pour vous connecter aux sources de données et afficher des données.
- **Pages maîtres**-pages maîtres ASP.NET vous autorisent à créer une disposition cohérente pour les pages dans votre application. Une seule page maître définit l’apparence et le comportement standard que vous souhaitez pour toutes les pages (ou un groupe de pages) dans votre application. Vous pouvez ensuite créer des pages de contenu individuelles qui contiennent le contenu que vous souhaitez afficher. Lorsque des utilisateurs demandent les pages de contenu, ils fusionnent avec la page maître pour produire une sortie qui associe la disposition de la page maître avec le contenu à partir de la page de contenu.
- **Utilisation des données**-ASP.NET offre de nombreuses options pour stocker, extraire et afficher les données. Dans une application ASP.NET Web Forms, des contrôles liés aux données vous permet d’automatiser la présentation ou l’entrée de données dans les éléments d’interface utilisateur de page web tels que les tables et les zones de texte et les listes déroulantes.
- **L’appartenance**-ASP.NET Identity stocke les informations d’identification de vos utilisateurs dans une base de données créée par l’application. Quand vos utilisateurs de se connectent, l’application valide le fait de leurs informations d’identification en lisant la base de données. De votre projet *compte* dossier contient les fichiers qui implémentent différentes parties d’appartenance : l’inscription, connexion, modification d’un mot de passe et autoriser l’accès. En outre, ASP.NET Web Forms prend en charge OAuth et OpenID. Ces améliorations de l’authentification autoriser les utilisateurs à se connecter à votre site à l’aide des informations d’identification existantes, à partir de ces comptes, ainsi que Facebook, Twitter, Windows Live, Google. Par défaut, le modèle crée une base de données d’appartenance à l’aide d’un nom de base de données par défaut sur une instance de SQL Server Express LocalDB, le serveur de base de données de développement qui est fourni avec Visual Studio Express 2013 pour le Web.
- **Script client et les infrastructures Client**-vous pouvez améliorer les fonctionnalités basées sur le serveur d’ASP.NET en incluant des fonctionnalités de script client dans les pages Web Form ASP.NET. Vous pouvez utiliser le script client pour fournir une interface utilisateur plus riche et plus réactive aux utilisateurs. Vous pouvez également utiliser le script client pour effectuer des appels asynchrones au serveur Web pendant l’exécution d’une page dans le navigateur.
- **Routage**-routage d’URL vous permet de configurer une application pour accepter les URL qui ne sont pas mappent à des fichiers physiques de la requête. Une URL de demande est simplement l’URL un utilisateur entre dans son navigateur pour rechercher une page sur votre site web. Utiliser le routage pour définir des URL qui sont sémantiquement explicites pour les utilisateurs et qui peut faciliter l’optimisation de moteur de recherche (SEO).
- **Gestion d’état**-ASP.NET Web Forms inclut plusieurs options qui vous aident à préserver les données sur une base par page et une base de l’application.
- **Sécurité**-une partie importante du développement d’une application plus sécurisée consiste à comprendre les menaces. Microsoft a développé un moyen de classer les menaces : l’usurpation d’identité, falsification, répudiation, divulgation d’informations, déni de service, une élévation de privilège (STRIDE). Dans ASP.NET Web Forms, vous pouvez ajouter des points d’extensibilité et les options de configuration qui vous permettent de personnaliser les différents comportements de sécurité dans ASP.NET Web Forms.
- **Performances**-performances peuvent être un facteur clé dans un projet ou un site Web a réussi. ASP.NET Web Forms vous permet de modifier les performances liés à la page et traitement du contrôle serveur, la gestion d’état, accès aux données, configuration de l’application et le chargement et efficace de pratiques de codage.
- **Internationalisation**- ASP.NET Web Forms vous permet de créer des pages web qui peut obtenir du contenu et autres données en fonction du paramètre de langue par défaut pour le navigateur ou selon les choix explicite de l’utilisateur du langage. Contenu et d’autres données correspond aux ressources et ces données peuvent être stockées dans les fichiers de ressources ou d’autres sources. Une page Web Forms ASP.NET, vous permet de configurer des contrôles pour obtenir leurs valeurs de propriété à partir de ressources. Au moment de l’exécution, les expressions de ressource sont remplacées par les ressources à partir du fichier de ressources localisé approprié.
- **Débogage et gestion des erreurs**-ASP.NET inclut des fonctionnalités pour vous aider à diagnostiquer les problèmes qui peuvent se produire dans votre application Web Forms. Débogage et gestion des erreurs sont également prises en charge dans ASP.NET Web Forms afin que vos applications compiler et exécuter efficacement.
- **Déploiement et hébergement**-Visual Studio, ASP.NET, Azure et IIS fournissent des outils qui vous aideront à travers le processus de déploiement et hébergement de votre application Web Forms.

## <a name="deciding-when-to-create-a-web-forms-application"></a>Quand faut-il créer une Application Web Forms

Vous devez déterminer avec soin si pour implémenter une application Web à l’aide ASP.NET Web Forms modèle ou un autre modèle, telles que l’infrastructure ASP.NET MVC. L’infrastructure MVC ne remplace pas le modèle Web Forms ; Vous pouvez utiliser un framework pour les applications Web. Avant de décider d’utiliser le modèle Web Forms ou l’infrastructure MVC pour un site Web spécifique, évaluez les avantages de chaque approche.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Avantages d’une Application Web basée sur les formulaires de Web

L’infrastructure basée sur des Web Forms offre les avantages suivants :

- Il prend en charge un modèle d’événement qui conserve l’état via HTTP, ce qui améliore le développement d’applications Web line-of-business. L’application basée sur des Web Forms fournit des dizaines d’événements qui sont pris en charge dans des centaines de contrôles serveur.
- Elle utilise un modèle de contrôleur de Page qui ajoute des fonctionnalités à des pages individuelles. Pour plus d’informations, consultez [Page Controller](https://go.microsoft.com/fwlink/?LinkId=106359 "Page Controller") sur le site Web MSDN.
- Elle utilise l’état d’affichage ou les formulaires de serveur, qui peuvent faciliter la gestion des informations d’état.
- Il fonctionne bien pour les petites équipes de développeurs et concepteurs qui souhaitent tirer parti du grand nombre de composants disponibles pour le développement rapide d’applications Web.
- En général, il est moins complexe pour le développement d’applications, car les composants (la **Page** classe, les contrôles, etc.) sont étroitement intégrés et requièrent habituellement moins de code que le modèle MVC.

### <a name="advantages-of-an-mvc-based-web-application"></a>Avantages d’une Application Web basée sur MVC

L’infrastructure ASP.NET MVC offre les avantages suivants :

- Il est plus facile à gérer la complexité en décomposant une application dans le modèle, la vue et le contrôleur.
- Il n’utilise pas l’état d’affichage ou de formulaires basés sur le serveur. L’infrastructure MVC est donc idéal pour les développeurs qui veulent un contrôle total sur le comportement d’une application.
- Il utilise un modèle de contrôleur frontal qui traite les demandes d’application Web via un seul contrôleur. Cela vous permet de concevoir une application qui prend en charge une riche infrastructure de routage. Pour plus d’informations, consultez [Front Controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Front Controller") sur le site Web MSDN.
- Il fournit une meilleure prise en charge pour le développement piloté par tests (TDD).
- Il fonctionne bien pour les applications Web qui sont pris en charge par de grandes équipes de développeurs et concepteurs Web qui ont besoin d’un degré élevé de contrôle sur le comportement de l’application.
