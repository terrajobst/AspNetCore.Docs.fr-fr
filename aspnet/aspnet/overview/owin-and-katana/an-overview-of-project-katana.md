---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: Une vue d’ensemble du projet Katana | Microsoft Docs
author: howarddierking
description: L’infrastructure ASP.NET existe depuis plus de dix ans, et la plateforme a activé le développement d’innombrables sites et services Web. En tant qu’application de Web...
ms.author: aspnetcontent
ms.date: 08/30/2013
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: a4c7d6cb5dc40ad68280a87acd89f60106a260a8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803092"
---
<a name="an-overview-of-project-katana"></a>Une vue d’ensemble du projet Katana
====================
par [Howard Dierking](https://github.com/howarddierking)

> L’infrastructure ASP.NET existe depuis plus de dix ans, et la plateforme a activé le développement d’innombrables sites et services Web. Comme les stratégies de développement d’applications Web ont évolué, l’infrastructure a été capable d’évoluer à l’étape des technologies telles que ASP.NET MVC et API Web ASP.NET. Développement d’applications Web utilise son étape suivante dans le monde du cloud computing, projet [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) fournit le jeu sous-jacent de composants pour les applications ASP.NET, leur permettant d’être flexible et portable léger et offrent de meilleures performances : autrement dit, le projet [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) cloud permet d’optimiser vos applications ASP.NET.


## <a name="why-katana--why-now"></a>Pourquoi Katana – pourquoi maintenant ?

 Qu’un parle d’un produit de framework ou de l’utilisateur final de développeur, il est important de comprendre les motivations sous-jacentes pour la création du produit et une partie de permet de savoir qui a le produit a été créé pour. ASP.NET a été créée avec deux de vos clients à l’esprit.   
  
**Le premier groupe de clients a été les développeurs ASP classiques.** Dans le temps, ASP était une des principales technologies pour la création d’applications et des sites Web dynamiques, pilotés par les données par l’entrelacement de balisage et le script côté serveur. Le runtime ASP fourni un script côté serveur avec un ensemble d’objets qui les aspects principaux du protocole HTTP sous-jacent et le serveur Web et les mettre en cache, fournie par l’accès à d’autres services telle session état des applications, etc. Bien que puissante, les applications ASP classiques est devenu difficile de gérer le fil de l’augmentation de la taille et la complexité. Cela a été en grande partie en raison de l’absence de structure trouvée dans des environnements associés à la duplication de code résultant de l’entrelacement de code et le balisage de script. Afin de profiter des avantages de l’ASP classique tout en répondant à ses défis, ASP.NET a tiré parti de l’organisation du code fournie par les langages orientés objet du .NET Framework tout en conservant le modèle de programmation côté serveur à quels ASP classique développeurs avaient pris l’habitude.

**Le deuxième groupe de clients cible pour ASP.NET a été développeurs d’applications métier Windows.** Contrairement aux développeurs ASP classiques, qui étaient habitués à écrire un balisage HTML et le code pour générer un balisage HTML plus, les développeurs de WinForms (comme les développeurs VB6 avant eux) étaient habitués à un processus de conception qui comprenait une zone de dessin et un ensemble complet de l’utilisateur contrôles d’interface. La première version d’ASP.NET – également appelé « Web Forms » fourni un processus similaire de conception, ainsi que d’un modèle d’événements côté serveur pour les composants d’interface utilisateur et un ensemble de fonctionnalités d’infrastructure (par exemple, ViewState) pour créer une expérience de développement FLUIDE entre le client et la programmation du côté serveur. Web Forms hid efficacement la nature sans état d’un site Web sous un modèle d’événement avec état qui était familière aux développeurs de WinForms.

### <a name="challenges-raised-by-the-historical-model"></a>Défis déclenchés par le modèle historique

**Le résultat net est un runtime mature et riche en fonctionnalités et le modèle de programmation de développeur.** Toutefois, cette richesse des fonctionnalités provenait quelques défis importants. Tout d’abord, l’infrastructure a été **monolithique**, avec des unités logiquement disparates de fonctionnalités être étroitement couplé dans le même assembly System.Web.dll (par exemple, les objets de base HTTP avec l’infrastructure de formulaires Web). Ensuite, ASP.NET a été inclus dans le cadre du plus grand .NET Framework, ce qui signifiait que le **temps entre les versions a été l’ordre des années.** Cela rendait difficile pour ASP.NET pour s’adapter à toutes les modifications qui se produisent dans le développement Web en constante évolution. Enfin, System.Web.dll lui-même a été couplée de différentes manières sur une option d’hébergement de Web spécifique : Internet Information Services (IIS).

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>Étapes évolutionnaires : ASP.NET MVC et API Web ASP.NET

Et un grand nombre de modifications qui se produisent dans le développement Web ! Applications Web ont été plus en plus en cours de développement comme ayant le focus une série de petits composants plutôt que de grandes infrastructures. Le nombre de composants, ainsi que la fréquence avec laquelle ils ont été publiés augmentaient à un rythme jamais plus rapide. Il était clair qu’également suivre le rythme avec le Web nécessiterait des infrastructures d’obtenir des plus petites, découplé et plus ciblée au lieu de la plus grande et plus riche en, par conséquent le **équipe ASP.NET a eu plusieurs étapes pour activer ASP.NET en tant qu’une famille de des composants enfichables Web plutôt que d’une infrastructure unique**.

Une des premières modifications a été le succès croissant du modèle de conception model-view-controller (MVC) bien connu grâce aux infrastructures de développement Web comme Ruby on Rails. Ce style de la création d’applications Web a donné le développeur plus grand contrôle sur le balisage de l’application tout en conservant la séparation de balisage et la logique métier, qui était un des points de vente initiales pour ASP.NET. Pour répondre à la demande pour ce style de développement d’applications Web, Microsoft a eu la possibilité de se positionner mieux pour l’avenir en **développement d’ASP.NET MVC hors bande** (et ne pas l’inclure dans le .NET Framework). ASP.NET MVC a été publié comme un téléchargement indépendant. L’équipe d’ingénierie a donné la possibilité de fournir des mises à jour beaucoup plus fréquemment avait été possibles auparavant.

Un autre changement majeur dans le développement d’applications Web a été le passage des pages Web dynamiques générées par le serveur à initiale du balisage statique avec des sections dynamiques de la page générée à partir d’un script côté client communication **avec les API Web principales via Les demandes AJAX**. Ce changement architectural a permis à assurer la propulsion l’essor d’API Web et le développement de l’infrastructure de l’API Web ASP.NET. Comme dans le cas d’ASP.NET MVC, la version de l’API Web ASP.NET fourni une autre possibilité d’évoluer ASP.NET davantage comme une infrastructure plus modulaire. L’équipe d’ingénierie a tiré parti de l’opportunité et **générés API Web ASP.NET de sorte que qu’elle n’avait aucune dépendance sur un des types de framework core trouvés dans System.Web.dll**. Cette option activée deux choses : tout d’abord, elle voulait dire que ASP.NET Web API susceptible d’évoluer de manière complètement autonome (et il peut continuer à effectuer une itération rapidement, car il est remis via NuGet). En second lieu, car il y a aucune dépendance externe à System.Web.dll et par conséquent, aucune dépendance sur IIS, ASP.NET Web API inclus la possibilité d’exécuter dans un hôte personnalisé (par exemple, une application console, service de Windows, etc.)

### <a name="the-future-a-nimble-framework"></a>L’avenir : Une infrastructure de rapidité

Par le découplage des composants du framework uns des autres et en les libérant ensuite sur NuGet, infrastructures pourraient désormais **effectuer une itération de façon plus indépendante, plus rapidement**. En outre, la puissance et la flexibilité de la capacité d’auto-hébergement de l’API Web s’est avéré très attrayantes pour les développeurs qui souhaitaient un **hôte petit et léger** pour leurs services. Il s’est avéré si attrayante, en fait, que d’autres infrastructures souhaitait également cette fonctionnalité, et cela présenté un nouveau défi car chaque framework exécuté dans son propre processus d’hôte sur sa propre adresse de base et devait être géré (démarré, arrêté, etc.) indépendamment. Une application Web moderne prend généralement en charge l’envoi de fichier statique, la génération de page dynamique, API Web et plus récemment real-time/notifications push. Attend que chacun de ces services doit-elle être exécuté et géré indépendamment n’était tout simplement pas réaliste.

Il fallait donc une simple abstraction d’hébergement qui pourraient permettre à un développeur composer une application à partir d’une variété de différents composants et infrastructures, puis exécuter cette application sur un hôte de prise en charge.

## <a name="the-open-web-interface-for-net-owin"></a>L’Interface Web ouverte pour .NET (OWIN)

 Inspirés par les avantages obtenus par [Rack](http://rack.github.io/) dans la Communauté Ruby, plusieurs membres de la Communauté .NET décidé de créer une abstraction entre les serveurs Web et les composants framework. Deux objectifs de conception pour l’abstraction OWIN ont été que c’était simple et qu’il a fallu peu de dépendances possible sur d’autres types de framework. Ces deux objectifs afin de garantir :

- Nouveaux composants peuvent plus facilement développés et consommées.
- Applications peuvent être déplacées plus facilement entre les hôtes et les systèmes potentiellement entière d’exploitation/plateformes.

L’abstraction qui en résulte se compose de deux éléments principaux. Le premier est le dictionnaire d’environnement. Cette structure de données est chargée de stocker l’ensemble de l’état nécessaire pour le traitement d’une requête HTTP et de réponse, ainsi que n’importe quel état du serveur approprié. Le dictionnaire d’environnement est défini comme suit :

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

Un serveur Web compatible avec OWIN est chargé de remplir le dictionnaire d’environnement avec des données telles que le flux du corps et les collections d’en-têtes pour une requête et réponse HTTP. Il est ensuite la responsabilité des composants d’application ou une infrastructure pour remplir ou de mettre à jour le dictionnaire avec des valeurs supplémentaires et d’écrire dans le flux du corps de réponse.

En plus de spécifier le type pour le dictionnaire d’environnement, la spécification OWIN définit une liste de paires clé-valeur de dictionnaire principal. Par exemple, le tableau suivant montre les clés de dictionnaire requis pour une requête HTTP :

| Nom de clé | Description de la valeur |
| --- | --- |
| `"owin.RequestBody"` | Un Stream avec le corps de la demande, le cas échéant. Stream.Null peut être utilisé comme un espace réservé s’il n’existe aucun corps de la demande. Consultez [corps de la demande](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics). |
| `"owin.RequestHeaders"` | Un `IDictionary<string, string[]>` des en-têtes de demande. Consultez [en-têtes](http://owin.org/html/owin.html#3-3-headers). |
| `"owin.RequestMethod"` | Un `string` contenant la méthode de demande HTTP de la demande (par exemple, `"GET"`, `"POST"`). |
| `"owin.RequestPath"` | Un `string` contenant le chemin d’accès de la demande. Le chemin d’accès doit être relatif à la « racine » du délégué d’application ; consultez [chemins d’accès](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestPathBase"` | Un `string` contenant la partie du chemin d’accès de demande correspondant à la « racine » du délégué d’application ; consultez [chemins d’accès](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestProtocol"` | Un `string` contenant le nom de protocole et la version (par exemple, `"HTTP/1.0"` ou `"HTTP/1.1"`). |
| `"owin.RequestQueryString"` | Un `string` contenant le composant de chaîne de requête de l’URI, de la requête HTTP sans le caractère de début « ? » (par exemple, `"foo=bar&baz=quux"`). La valeur peut être une chaîne vide. |
| `"owin.RequestScheme"` | Un `string` contenant le schéma d’URI utilisé pour la demande (par exemple, `"http"`, `"https"`) ; consultez [schéma d’URI](http://owin.org/html/owin.html#5-1-uri-scheme). |

Le deuxième élément clé du OWIN est le délégué d’application. Il s’agit d’une signature de fonction qui sert d’interface principale entre tous les composants dans une application OWIN. La définition pour le délégué d’application est la suivante :

`Func<IDictionary<string, object>, Task>;`

Ensuite, le délégué d’application est simplement une implémentation du type de délégué Func où la fonction accepte le dictionnaire d’environnement en tant qu’entrée et retourne une tâche. Cette conception a plusieurs conséquences pour les développeurs :

- Il existe un très petit nombre de dépendances de type nécessaire pour écrire des composants OWIN. Cela augmente considérablement l’accessibilité d’OWIN pour les développeurs.
- La conception asynchrone permet l’abstraction d’être efficaces avec sa gestion des ressources informatiques, en particulier dans des opérations gourmandes en e/s plus.
- Étant donné que le délégué d’application est une unité atomique de l’exécution et parce que le dictionnaire d’environnement est transmis sous la forme d’un paramètre sur le délégué, composants OWIN peuvent facilement être chaînés ensemble pour créer des pipelines de traitement de HTTP complex.

Du point de vue de l’implémentation, OWIN est une spécification ([http://owin.org/html/owin.html](http://owin.org/html/owin.html)). Son objectif doit ne pas être l’infrastructure Web suivante, mais plutôt une spécification pour l’interagissent des infrastructures Web et les serveurs Web.

Si vous avez examiné [OWIN](http://owin.org/) ou [Katana](https://github.com/aspnet/AspNetKatana/wiki), vous avez pouvez également remarquer les [package NuGet de Owin](http://nuget.org/packages/Owin) et Owin.dll. Cette bibliothèque contient une seule interface, [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs), qui formalise et codifie décrite dans la séquence de démarrage [section 4](http://owin.org/html/owin.html#4-application-startup) de la spécification OWIN. Bien que non obligatoire pour générer des serveurs OWIN, les [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs) interface fournit un point de référence concret, et il est utilisé par les composants du projet Katana.

## <a name="project-katana"></a>Projet Katana

Tandis qu’à la fois le [OWIN](http://owin.org/html/owin.html) spécification et *Owin.dll* sont détenus et Communauté exécuter efforts open source, le [Katana](https://github.com/aspnet/AspNetKatana/wiki) projet représente l’ensemble des OWIN composants qui, lors de la source de toujours ouverte, sont créées et publiées par Microsoft. Ces composants incluent les composants d’infrastructure, tels que les ordinateurs hôtes et serveurs, ainsi que des composants fonctionnels, tels que les composants de l’authentification et de liaisons aux infrastructures telles que [SignalR](../../../signalr/index.md) et [Web ASP.NET API](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md). Le projet comporte les trois objectifs de haut niveau suivants : 

- **Portable** – composants doivent pouvoir être facilement remplacée pour les nouveaux composants qu’elles sont disponibles. Cela inclut tous les types de composants, à partir de l’infrastructure pour le serveur et l’hôte. La conséquence de cet objectif est que tiers infrastructures peuvent en toute transparence s’exécuter sur les serveurs Microsoft tandis que les infrastructures de Microsoft peuvent éventuellement l’exécuter sur les hôtes et des serveurs tiers.
- **Modulaire/flexible**– Contrairement à nombreuses infrastructures qui comprennent une multitude de fonctionnalités qui sont activées par défaut, les composants du projet Katana doivent être réduite et focalisée, ce qui donne un contrôle au développeur d’applications pour déterminer quels composants de utiliser dans son application.
- **Lightweight et performante/plus évolutif** : en divisant la notion d’une infrastructure traditionnelle en un ensemble de petits, concentré de composants qui sont ajoutés explicitement par le développeur d’applications, une application de Katana qui en résulte peut consommer moins de l’informatique ressources et par conséquent, la gestion des charges plus, qu’avec d’autres types de serveurs et d’infrastructures. Comme la configuration requise de l’application à la demande davantage de fonctionnalités à partir de l’infrastructure sous-jacente, ceux peuvent être ajoutés au pipeline OWIN, mais qui doit être explicitement la décision de part le développeur d’applications. En outre, la possibilité de substituer des composants de niveau inférieur signifie que lorsqu’elles sont disponibles, nouveaux serveurs hautes performances peuvent en toute transparence être introduites pour améliorer les performances des applications OWIN sans arrêt de ces applications.

## <a name="getting-started-with-katana-components"></a>Mise en route avec les composants Katana

Quand il a été introduite, un aspect de la [Node.js](http://nodejs.org/) framework qu’immédiatement que vous avez dessinée l’attention a été la simplicité avec laquelle un peut créer et exécuter un serveur Web. Si les objectifs de Katana ont été insérées dans la lumière de [Node.js](http://nodejs.org/), un peut les résumer en disant que Katana apporte de nombreux avantages de [Node.js](http://nodejs.org/) (et les infrastructures comme il) sans obliger le développeur parler tout ce dont elle sait sur le développement d’applications Web ASP.NET. Pour cette instruction contenir la valeur est true, mise en route avec le projet Katana doit être tout aussi simple de nature à [Node.js](http://nodejs.org/).

## <a name="creating-hello-world"></a>Création de « Hello World ! »

Une différence notable entre JavaScript et le développement .NET est la présence (ou non) d’un compilateur. Par conséquent, le point de départ pour un simple serveur Katana est un projet Visual Studio. Toutefois, nous pouvons commencer par le plus minimal des types de projets : l’Application Web ASP.NET vide.

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

Ensuite, nous allons installer le [Microsoft.Owin.Host.SystemWeb](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) package NuGet dans le projet. Ce package fournit un serveur OWIN qui s’exécute dans le pipeline de demande ASP.NET. Il se trouve sur le [galerie NuGet](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) et peut être installé à l’aide de la boîte de dialogue Gestionnaire de package Visual Studio ou de la console du Gestionnaire de package avec la commande suivante :

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

L’installation de le `Microsoft.Owin.Host.SystemWeb` package installe quelques packages supplémentaires en tant que dépendances. Une de ces dépendances est `Microsoft.Owin`, une bibliothèque qui fournit plusieurs types d’assistance et des méthodes pour le développement d’applications OWIN. Nous pouvons utiliser ces types pour écrire rapidement le serveur suivant de « hello world ».

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

Ce serveur Web très simple peut maintenant être exécuté à l’aide de Visual Studio **F5** commande et inclut la prise en charge complète pour le débogage.

## <a name="switching-hosts"></a>Hôtes de commutation

Par défaut, l’exemple « hello world » précédent s’exécute dans le pipeline de demande ASP.NET, qui utilise System.Web dans le contexte d’IIS. Cela peut par lui-même ajouter une valeur ajoutée considérable car elle permet de tirer parti de la flexibilité et la composabilité d’un pipeline OWIN avec les fonctionnalités de gestion et de maturité globale d’IIS. Toutefois, il peut arriver dans lequel les avantages fournis par IIS ne sont pas nécessaires et le désir est destiné à un hôte de plus petit et plus léger. Ensuite, ce qui est nécessaire pour exécuter notre serveur Web simple en dehors d’IIS et System.Web ?

Pour illustrer l’objectif de la portabilité, le passage d’un hôte de serveur Web vers un hôte de la ligne de commande nécessite simplement en ajoutant les nouvelles dépendances de serveur et l’hôte au dossier de sortie du projet, puis en démarrant l’ordinateur hôte. Dans cet exemple, nous allons héberger notre serveur Web dans un hôte Katana appelé `OwinHost.exe` et utilisera le serveur basée sur Katana HttpListener. De même pour les autres composants Katana, il seront acquis à partir de NuGet à l’aide de la commande suivante :

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

À partir de la ligne de commande, nous pouvons puis accédez au dossier projet racine et exécutez simplement le `OwinHost.exe` (qui a été installé dans le dossier Outils de son package NuGet respectif). Par défaut, `OwinHost.exe` est configuré pour rechercher le serveur HttpListener et aucune configuration supplémentaire n’est donc nécessaire. Navigation dans un navigateur Web pour `http://localhost:5000/` montre l’application en cours d’exécution via la console.

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Architecture de Katana

 L’architecture des composants Katana divise une application en quatre couches logiques, comme illustré ci-dessous : *hôte, le serveur, intergiciel (middleware),* et *application*. L’architecture des composants est conçue de telle façon que les implémentations de ces couches puissent être facilement remplacées, dans de nombreux cas, sans nécessiter de recompilation de l’application.   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>Hôte

 L’hôte est chargé de :

- La gestion du processus sous-jacent.
- Orchestration de flux de travail qui entraîne la sélection d’un serveur et de la construction d’un pipeline OWIN via les demandes est traité.

  À l’heure actuelle, il existe 3 options d’hébergement principales pour les applications Katana :  
  
**IIS/ASP.NET**: l’utilisation des types HttpModule et HttpHandler standards, les pipelines OWIN peuvent s’exécutent sur IIS dans le cadre d’un flux de demande ASP.NET. Hébergement de prise en charge ASP.NET est activé en installant le package NuGet de Microsoft.AspNet.Host.SystemWeb dans un projet d’application Web. En outre, étant donné que IIS agit comme un hôte et un serveur, la distinction de serveur/hôte OWIN va de pair dans ce package NuGet, ce qui signifie que si vous utilisez l’hôte SystemWeb, un développeur ne peut pas remplacer une implémentation de l’autre serveur.  
  
**Hôte personnalisé**: suite de composants Katana The donne au développeur la possibilité d’héberger des applications dans son propre processus personnalisé, si c’est une application console, un service de Windows, un périphérique. Cette fonctionnalité est similaire à la fonctionnalité Self-host fournie par l’API Web. L’exemple suivant montre un hôte personnalisé de code de l’API Web :  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

Le programme d’installation Self-host pour une application de Katana est similaire :

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

Une différence notable entre les API Web et Katana Self-host exemples est que le code de configuration de Web API est manquant dans l’exemple Self-host de Katana. Pour permettre la portabilité et composabilité, Katana sépare le code qui démarre le serveur à partir du code qui configure le pipeline de traitement de requête. Le code qui configure l’API Web, puis est contenu dans la classe de démarrage, qui est en outre spécifié comme paramètre de type dans WebApplication.Start.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

La classe de démarrage est abordée plus en détail plus loin dans l’article. Toutefois, le code requis pour démarrer un processus Self-host est remarquablement similaire au code que vous utilisez peut-être dès aujourd'hui dans les applications ASP.NET Web API Self-host de Katana.

**OwinHost.exe**: alors que certains souhaiterez écrire un processus personnalisé pour exécuter des applications Web de Katana, nombreuses préférez simplement lancer un exécutable prédéfini qui peut démarrer un serveur et exécuter leur application. Pour ce scénario, la suite de composants Katana inclut `OwinHost.exe`. Exécutée à partir d’un répertoire de projet racine, ce fichier exécutable démarre un serveur (il utilise le serveur HttpListener par défaut) et utiliser des conventions pour rechercher et exécuter la classe de démarrage de l’utilisateur. Pour un contrôle plus précis, l’exécutable fournit un nombre de paramètres de ligne de commande supplémentaires.

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>Serveur

 Alors que l’hôte est responsable du démarrage et de gestion des processus dans lequel l’application s’exécute, la responsabilité du serveur pour ouvrir un socket réseau, écouter les demandes et les envoyer via le pipeline de composants OWIN spécifié par l’utilisateur (en tant que vous avez peut-être déjà remarqué, ce pipeline est spécifié dans la classe de démarrage du développeur de l’application). Actuellement, le projet Katana comprend deux implémentations de serveur : 

- **Microsoft.Owin.Host.SystemWeb**: comme mentionné précédemment, IIS conjointement avec le pipeline ASP.NET agit comme un hôte et un serveur. Par conséquent, lors du choix de cette option d’hébergement, IIS à la fois gère les problèmes d’au niveau hôte, tels que l’activation des processus et écoute les requêtes HTTP. Pour les applications Web ASP.NET, il envoie ensuite les demandes dans le pipeline ASP.NET. L’hôte Katana SystemWeb inscrit une ASP.NET HttpModule et un HttpHandler pour intercepter des requêtes car elles circulent dans le pipeline HTTP et les envoyer via le pipeline OWIN spécifié par l’utilisateur.
- **Microsoft.Owin.Host.HttpListener**: comme son nom l’indique, ce serveur Katana utilise la classe de .NET Framework HttpListener pour ouvrir un socket et envoyer des demandes dans un pipeline OWIN spécifiés par les développeurs. Il s’agit actuellement la sélection de serveur par défaut pour les API d’auto-hébergement Katana et OwinHost.exe.

## <a name="middlewareframework"></a>Intergiciel (middleware) / framework

 Comme mentionné précédemment, lorsque le serveur accepte une demande d’un client, il est responsable de la transmission via un pipeline de composants OWIN, qui sont spécifiées par le code de démarrage du développeur. Ces composants de pipeline sont connus comme intergiciel (middleware).  
 À un niveau très fondamental, un composant d’intergiciel (middleware) OWIN doit simplement implémenter le délégué d’application OWIN afin qu’il soit pouvant être appelé.

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

Toutefois, afin de simplifier le développement et la composition des composants d’intergiciel (middleware), Katana prend en charge un certain nombre de conventions et types d’assistance pour les composants d’intergiciel (middleware). La plus courante de ces est la `OwinMiddleware` classe. Un composant d’intergiciel (middleware) personnalisé créé à l’aide de cette classe serait similaire à ce qui suit : 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 Cette classe est dérivée de `OwinMiddleware`, implémente un constructeur qui accepte une instance de l’intergiciel suivant dans le pipeline en tant qu’un de ses arguments et puis le passe au constructeur de base. Arguments supplémentaires utilisés pour configurer l’intergiciel (middleware) sont également déclarés en tant que paramètres de constructeur après le paramètre d’intergiciel (middleware) suivant.   
  
Lors de l’exécution, l’intergiciel (middleware) est exécutée via substituées `Invoke` (méthode). Cette méthode accepte un seul argument de type `OwinContext`. Cet objet de contexte est fourni par le `Microsoft.Owin` package NuGet décrits précédemment et fournit l’accès fortement typé pour le dictionnaire de demande, de réponse et d’environnement, ainsi que de quelques types d’assistance supplémentaires.   
  
La classe d’intergiciel (middleware) peut être facilement ajoutée au pipeline OWIN dans le code de démarrage de l’application comme suit :   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

Étant donné que l’infrastructure de Katana génère simplement un pipeline de composants d’intergiciel (middleware) OWIN, et étant donné que les composants doivent simplement prendre en charge le délégué d’application pour participer le pipeline, les composants d’intergiciel (middleware) peuvent varier en complexité de simple enregistreurs d’événements à des infrastructures entières comme ASP.NET, API Web, ou [SignalR](../../../signalr/index.md). Par exemple, l’ajout d’API Web ASP.NET au pipeline OWIN précédent nécessite l’ajout le code de démarrage suivant :

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

L’infrastructure de Katana générera le pipeline de composants d’intergiciel (middleware) selon l’ordre dans lequel ils ont été ajoutés à l’objet d’IAppBuilder dans la méthode de Configuration. Puis, dans notre exemple, LoggerMiddleware peut gérer toutes les demandes qui transitent par le pipeline, quelle que soit la façon dont ces demandes sont finalement gérées. Cela permet des scénarios puissants où un composant d’intergiciel (middleware) (par exemple, un composant d’authentification) peut traiter les demandes d’un pipeline qui comprend plusieurs composants et frameworks (par exemple, API Web ASP.NET SignalR et un serveur de fichiers statiques).
 
## <a name="applications"></a>Applications

Comme illustré dans les exemples précédents, OWIN et le projet Katana ne doivent pas être considérés comme un nouveau modèle de programmation d’application, mais plutôt comme une abstraction de dissocier les modèles de programmation d’application et les infrastructures de serveur et l’infrastructure d’hébergement. Par exemple, lorsque vous générez des applications API Web, le framework developer continue à utiliser l’infrastructure API Web ASP.NET, quel que soit ou non, l’application s’exécute dans un pipeline OWIN à l’aide de composants à partir du projet Katana. L’endroit où code lié à OWIN seront visible par le développeur d’applications sera le code de démarrage d’application, où le développeur compose le pipeline OWIN. Dans le code de démarrage, le développeur inscrira une série d’instructions UseXx, généralement un pour chaque composant d’intergiciel (middleware) qui traitera les demandes entrantes. Cette expérience aura le même effet que l’enregistrement des modules HTTP dans le monde actuel de System.Web. En règle générale, une plus grande infrastructure intergiciel (middleware), telles que les API Web ASP.NET ou [SignalR](../../../signalr/index.md) sera inscrit à la fin du pipeline. Composants d’intergiciel transversales, telles que celles pour l’authentification ou la mise en cache, sont généralement enregistrés vers le début du pipeline afin qu’ils traitent les demandes pour toutes les infrastructures et les composants enregistrés ultérieurement dans le pipeline. Cette séparation des composants d’intergiciel (middleware) de l’autre et à partir des composants d’infrastructure sous-jacent permet aux composants d’évoluer à des vitesses élevées différents tout en garantissant que l’ensemble du système reste stable.

## <a name="components--nuget-packages"></a>Composants : les Packages NuGet

Comme de nombreuses bibliothèques actuels et infrastructures, les composants du projet Katana sont fournies sous la forme d’un ensemble de packages NuGet. Pour la prochaine version 2.0, le graphique de dépendance de package Katana se présente comme suit. (Cliquez sur l’image pour l’agrandir.)

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Presque chaque package dans le projet Katana dépend, directement ou indirectement, le package Owin. Vous vous souvenez peut-être qu’il s’agit du package qui contient l’interface IAppBuilder, qui fournit une implémentation concrète de la séquence de démarrage d’application décrite dans la section 4 de la spécification OWIN. En outre, la plupart des packages dépendent de Microsoft.Owin, qui fournit un ensemble de types d’assistance pour l’utilisation des requêtes et réponses HTTP. Le reste du package peut être classé en tant que packages infrastructure (serveurs ou hôtes) hébergement ou intergiciel (middleware). Packages et dépendances externes au projet Katana sont affichés en orange.

L’infrastructure d’hébergement pour Katana 2.0 inclut le SystemWeb et serveurs HttpListener, le package OwinHost pour exécuter des applications OWIN à l’aide de OwinHost.exe et le package Microsoft.Owin.Hosting pour l’hébergement automatique des applications OWIN dans un hôte personnalisé (par exemple, application console, service de Windows, etc.)

Katana 2.0, les composants d’intergiciel (middleware) sont principalement destinés à fournir différents moyens d’authentification. Un composant d’intergiciel (middleware) supplémentaires pour les diagnostics est fourni, ce qui permet la prise en charge pour une page de démarrage et d’erreur. Que OWIN augmente dans l’abstraction d’hébergement de fait, l’écosystème de composants d’intergiciel (middleware), c'est-à-dire ceux développés par Microsoft et tiers, également croître dans le nombre.

## <a name="conclusion"></a>Conclusion

 À partir de son début, les objectifs du projet Katana n’a pas été de créer et de forcer ainsi aux développeurs pour en savoir encore une autre infrastructure Web. Au lieu de cela, l’objectif a été de créer une abstraction pour donner aux développeurs d’applications Web .NET plus de choix qu’il a précédemment été possible. En décomposant les couches logiques d’une pile d’applications Web typique dans un ensemble de composants remplaçables, le projet Katana permet aux composants dans toute la pile pour améliorer à quel que soit le taux de sens pour ces composants. En créant tous les composants de l’abstraction OWIN simple, Katana permet d’infrastructures et les applications reposant sur les soit portable sur une variété de différents serveurs et ordinateurs hôtes. En plaçant le développeur dans le contrôle de la pile, Katana permet de s’assurer que le développeur fait le choix ultimate sur comment léger ou comment riche sa pile Web doit correspondre.  
  

## <a name="for-more-information-about-katana"></a>Pour plus d’informations sur Katana

- Le projet Katana sur GitHub : [ https://github.com/aspnet/AspNetKatana/ ](https://github.com/aspnet/AspNetKatana/).
- Vidéo : [le projet Katana - OWIN pour ASP.NET](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET), par Howard Dierking.

## <a name="acknowledgements"></a>Remerciements

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT) ) Rick est rédactrice en programmation senior pour Microsoft Azure et de MVC.
- [Scott Hanselman](http://www.hanselman.com/blog/): (twitter [ @shanselman ](https://twitter.com/shanselman) )
- [Jon Galloway](https://weblogs.asp.net/jgalloway/default.aspx): (twitter [ @jongalloway ](https://twitter.com/jongalloway) )
