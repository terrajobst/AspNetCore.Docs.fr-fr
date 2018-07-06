---
uid: whitepapers/aspnet4/overview
title: ASP.NET 4 et présentation de développement Web Visual Studio 2010 | Microsoft Docs
author: rick-anderson
description: Ce document fournit une vue d’ensemble d’un grand nombre des nouvelles fonctionnalités pour ASP.NET qui sont inclus dans le.NET Framework 4 et Visual Studio 2010.
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: 5c7aa95b18bc0a97f42cc981476c110830286fa5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829041"
---
<a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a>ASP.NET 4 et Visual Studio 2010 Web présentation du développement
====================
> Ce document fournit une vue d’ensemble d’un grand nombre des nouvelles fonctionnalités pour ASP.NET qui sont inclus dans le.NET Framework 4 et Visual Studio 2010.
> 
> [Téléchargez ce livre blanc](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)


**Sommaire**

**[Services principaux](#0.2__Toc253429238 "_Toc253429238")**  
[Refactorisation du fichier Web.config](#0.2__Toc253429239 "_Toc253429239")  
[La mise en cache de sortie extensible](#0.2__Toc253429240 "_Toc253429240")  
[Démarrage automatique des Applications Web](#0.2__Toc253429241 "_Toc253429241")  
[Redirection définitive d’une Page](#0.2__Toc253429242 "_Toc253429242")  
[Réduction de l’état de Session](#0.2__Toc253429243 "_Toc253429243")  
[Étendez la plage d’URL autorisées](#0.2__Toc253429244 "_Toc253429244")  
[Validation de requête extensible](#0.2__Toc253429245 "_Toc253429245")  
[La mise en cache de l’objet et de l’objet d’extensibilité de la mise en cache](#0.2__Toc253429246 "_Toc253429246")  
[Encodage d’en-tête HTTP, l’URL et extensible HTML](#0.2__Toc253429247 "_Toc253429247")  
[Surveillance des performances pour les Applications individuelles dans un seul processus de travail](#0.2__Toc253429248 "_Toc253429248")  
[Multi-Targeting](#0.2__Toc253429249 "_Toc253429249")

**[Ajax](#0.2__Toc253429250 "_Toc253429250")**  
[jQuery dans les Web Forms et MVC](#0.2__Toc253429251 "_Toc253429251")  
[Prise en charge du réseau de distribution de contenu](#0.2__Toc253429252 "_Toc253429252")  
[Scripts explicite de ScriptManager](#0.2__Toc253429253 "_Toc253429253")

**[Web Forms](#0.2__Toc253429256 "_Toc253429256")**  
[Définition de balises Meta avec les propriétés Page.MetaKeywords et Page.MetaDescription](#0.2__Toc253429257 "_Toc253429257")  
[L’activation de l’état d’affichage pour des contrôles individuels](#0.2__Toc253429258 "_Toc253429258")  
[Modifications apportées aux fonctionnalités du navigateur](#0.2__Toc253429259 "_Toc253429259")  
[Routage dans ASP.NET 4](#0.2__Toc253429260 "_Toc253429260")  
[Définition d’ID de Client](#0.2__Toc253429261 "_Toc253429261")  
[Sélection de ligne persistante dans les contrôles de données](#0.2__Toc253429262 "_Toc253429262")  
[Contrôle de graphique ASP.NET](#0.2__Toc253429263 "_Toc253429263")  
[Filtrage des données avec le contrôle QueryExtender](#0.2__Toc253429264 "_Toc253429264")  
[Expressions de Code de codé au format HTML](#0.2__Toc253429265 "_Toc253429265")  
[Modifications du modèle de projet](#0.2__Toc253429266 "_Toc253429266")  
[Améliorations des CSS](#0.2__Toc253429267 "_Toc253429267")  
[Masquage div éléments autour des champs masqués](#0.2__Toc253429268 "_Toc253429268")  
[Rendu d’une Table externe pour les contrôles basés sur des modèles](#0.2__Toc253429269 "_Toc253429269")  
[Améliorations des contrôles de ListView](#0.2__Toc253429270 "_Toc253429270")  
[CheckBoxList et RadioButtonList contrôle améliorations](#0.2__Toc253429271 "_Toc253429271")  
[Améliorations du contrôle menu](#0.2__Toc253429272 "_Toc253429272")  
[Assistant et contrôles CreateUserWizard 56](#0.2__Toc253429273 "_Toc253429273")

**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**  
[Prise en charge des zones](#0.2__Toc253429275 "_Toc253429275")  
[Prise en charge de Validation Annotation de données attribut](#0.2__Toc253429276 "_Toc253429276")  
[Programmes d’assistance basés sur des modèles](#0.2__Toc253429277 "_Toc253429277")

**[Dynamic Data](#0.2__Toc253429278 "_Toc253429278")**  
[L’activation des données dynamiques pour les projets existants](#0.2__Toc253429279 "_Toc253429279")  
[Syntaxe déclarative contrôle DynamicDataManager](#0.2__Toc253429280 "_Toc253429280")  
[Modèles d’entité](#0.2__Toc253429281 "_Toc253429281")  
[Nouveaux modèles de champ d’URL et adresses de messagerie](#0.2__Toc253429282 "_Toc253429282")  
[Création de liens avec le contrôle DynamicHyperLink](#0.2__Toc253429283 "_Toc253429283")  
[Prise en charge de l’héritage dans le modèle de données](#0.2__Toc253429284 "_Toc253429284")  
[Prise en charge pour les relations plusieurs-à-plusieurs (Entity Framework uniquement)](#0.2__Toc253429285 "_Toc253429285")  
[Nouveaux attributs pour contrôler l’affichage et la prise en charge des énumérations](#0.2__Toc253429286 "_Toc253429286")  
[Meilleure prise en charge pour les filtres](#0.2__Toc253429287 "_Toc253429287")

**[Améliorations du développement Web Visual Studio 2010](#0.2__Toc253429288 "_Toc253429288")**  
[Amélioration de compatibilité CSS](#0.2__Toc253429289 "_Toc253429289")  
[HTML et des extraits de code JavaScript](#0.2__Toc253429290 "_Toc253429290")  
[Améliorations de JavaScript IntelliSense](#0.2__Toc253429291 "_Toc253429291")

**[Déploiement d’Application avec Visual Studio 2010 Web](#0.2__Toc253429292 "_Toc253429292")**  
[Web Packaging](#0.2__Toc253429293 "_Toc253429293")  
[Transformation Web.config](#0.2__Toc253429294 "_Toc253429294")  
[Déploiement de base de données](#0.2__Toc253429295 "_Toc253429295")  
[Publication en un clic pour les Applications Web](#0.2__Toc253429296 "_Toc253429296")  
[Ressources](#0.2__Toc253429297 "_Toc253429297")

**[Exclusion de responsabilité](#0.2__Toc253429298 "_Toc253429298")**

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a>Services principaux

ASP.NET 4 présente un certain nombre de fonctionnalités qui améliorent les services ASP.NET principaux tels que la mise en cache de sortie et le stockage de l’état de session.

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a>Refactorisation du fichier Web.config

Le `Web.config` fichier qui contient la configuration pour une application Web a augmenté considérablement avec des versions précédentes du .NET Framework, comme les nouvelles fonctionnalités ont été ajoutées, tels que Ajax, routage et l’intégration avec IIS 7. Cela rend plus difficile à configurer ou démarrer de nouvelles applications Web sans un outil tel que Visual Studio. Dans .la NET Framework 4, les principaux éléments de configuration ont été déplacées vers le `machine.config` fichier et les applications héritent maintenant de ces paramètres. Cela permet la `Web.config` fichier dans les applications ASP.NET 4 pour être vide ou contenir uniquement les lignes suivantes, qui spécifient, pour Visual Studio, quelle version du framework cible de l’application :

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a>La mise en cache de sortie extensible

Depuis la publication d’ASP.NET 1.0, la mise en cache de sortie a activé aux développeurs de stocker la sortie générée des pages, les contrôles et les réponses HTTP en mémoire. Pour les demandes Web suivantes, ASP.NET peut servir du contenu plus rapidement en récupérant la sortie générée à partir de la mémoire au lieu de la régénération de la sortie à partir de zéro. Toutefois, cette approche présente une limitation : contenu généré doit toujours être stockées en mémoire et sur les serveurs qui rencontrent un trafic dense, la mémoire consommée par la mise en cache de sortie peut entrer en concurrence avec les exigences de mémoire des autres parties d’une application Web.

ASP.NET 4 ajoute un point d’extensibilité pour la mise en cache de sortie qui vous permet de configurer un ou plusieurs fournisseurs de cache de sortie personnalisés. Les fournisseurs de cache de sortie peuvent utiliser n’importe quel mécanisme de stockage pour conserver le contenu HTML. Cela permet de créer des fournisseurs de cache de sortie personnalisés pour les mécanismes de persistance varié, qui peuvent inclure des disques locaux ou distants, le stockage cloud et distribué des moteurs de cache.

Vous créez un fournisseur de cache de sortie personnalisé en tant que classe qui dérive de la nouvelle *System.Web.Caching.OutputCacheProvider* type. Vous pouvez ensuite configurer le fournisseur dans le `Web.config` fichier à l’aide de la nouvelle *fournisseurs* sous-section de la *outputCache* élément, comme indiqué dans l’exemple suivant :

[!code-xml[Main](overview/samples/sample2.xml)]

Par défaut dans ASP.NET 4, toutes les réponses HTTP, pages rendues et contrôles utilisent le cache de sortie en mémoire, comme illustré dans l’exemple précédent, où le *defaultProvider* attribut est défini sur AspNetInternalProvider. Vous pouvez modifier le fournisseur de cache de sortie par défaut utilisé pour une application Web en spécifiant un nom de fournisseur différent pour *defaultProvider*.

En outre, vous pouvez sélectionner différents fournisseurs de cache de sortie par le contrôle et par demande. Le moyen le plus simple de choisir un autre fournisseur de cache de sortie pour les contrôles utilisateur Web différents consiste à faire de façon déclarative à l’aide de la nouvelle *providerName* d’attribut dans une directive de contrôle, comme indiqué dans l’exemple suivant :

[!code-aspx[Main](overview/samples/sample3.aspx)]

Spécification d’un fournisseur de cache de sortie différent pour une requête HTTP nécessite un peu plus de travail. Au lieu de spécifier le fournisseur de façon déclarative, vous substituez la nouvelle *GetOuputCacheProviderName* méthode dans le `Global.asax` fichier pour spécifier le fournisseur à utiliser pour une demande spécifique. L'exemple suivant montre comment effectuer cette opération.

[!code-csharp[Main](overview/samples/sample4.cs)]

Avec l’ajout de l’extensibilité des fournisseurs de cache de sortie vers ASP.NET 4, vous pouvez désormais adopter des stratégies de mise en cache de sortie plus agressives et plus intelligentes pour vos sites Web. Par exemple, il est désormais possible de mettre en cache les pages « Top 10 » d’un site dans la mémoire, lors de la mise en cache de pages qui obtiennent un trafic inférieur sur le disque. Vous pouvez également mettre en cache chaque variation de combinaison pour une page rendue, mais utiliser un cache distribué afin que la consommation de mémoire est déchargée à partir de serveurs Web frontaux.

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a>Démarrage automatique des Applications Web

Certaines applications Web doivent charger de grandes quantités de données ou effectuer une initialisation gourmande en ressources de traitement avant de servir la première demande. Dans les versions antérieures d’ASP.NET, pour ces situations que vous deviez imaginer des approches personnalisées pour « réveiller » une application ASP.NET, puis exécutez le code d’initialisation pendant la *Application\_charge* méthode dans le `Global.asax` fichier.

Une nouvelle fonctionnalité d’extensibilité nommée *démarrage automatique* que directement les adresses de ce scénario est disponible lorsque ASP.NET 4 s’exécute sur IIS 7.5 sur Windows Server 2008 R2. La fonctionnalité de démarrage automatique fournit une approche contrôlée pour le démarrage d’un pool d’applications, l’initialisation d’une application ASP.NET et accepter des demandes HTTP.

> [!NOTE] 
> 
> Module de mise en route d’applications IIS pour IIS 7.5
> 
> L’équipe IIS a publié la première version de bêta-test du Module Application Warm-up pour IIS 7.5. Cela rend le préchauffage à vos applications encore plus faciles que décrit précédemment. Au lieu d’écrire un code personnalisé, vous spécifiez les URL des ressources à exécuter avant l’application Web accepte les demandes à partir du réseau. Cette mise en route se produit lors du démarrage du service IIS (si vous avez configuré le pool d’applications IIS en tant que *AlwaysRunning*) et lorsqu’un processus de travail IIS recycle. Au cours de recyclage, le processus de travail IIS ancien continue d’exécuter des demandes jusqu'à ce que le processus de travail générée récemment entièrement préchauffé, afin que les applications expérience sans interruptions ou autres problèmes dues aux caches unprimed. Notez que ce module fonctionne avec n’importe quelle version d’ASP.NET, en commençant par la version 2.0.
> 
> Pour plus d’informations, consultez [Application Warm-up](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) sur le site Web de IIS.net. Pour une procédure pas à pas qui montre comment utiliser la fonctionnalité de mise en route, consultez [mise en route avec le Module IIS 7.5 préchauffage](https://www.iis.net/learn/manage) sur le site Web de IIS.net.


Pour utiliser la fonctionnalité de démarrage automatique, un administrateur IIS définit un pool d’applications dans IIS 7.5 doit être démarré automatiquement à l’aide de la configuration suivante dans le `applicationHost.config` fichier :

[!code-xml[Main](overview/samples/sample5.xml)]

Comme un seul pool d’applications peut contenir plusieurs applications, vous spécifiez des applications individuelles à démarrer automatiquement à l’aide de la configuration suivante dans le `applicationHost.config` fichier :

[!code-xml[Main](overview/samples/sample6.xml)]

Lorsque le serveur IIS 7.5 est démarré à froid ou lorsqu’un pool d’applications est recyclé, IIS 7.5 utilise les informations contenues dans le `applicationHost.config` fichier pour déterminer quels Web applications doit être démarré automatiquement. Pour chaque application est marquée pour le démarrage automatique, IIS 7.5 envoie une demande vers ASP.NET 4 pour démarrer l’application dans un état au cours de laquelle l’application temporairement n’accepte pas les requêtes HTTP. Lorsqu’il est dans cet état, ASP.NET instancie le type défini par le *serviceAutoStartProvider* attribut (comme indiqué dans l’exemple précédent) et appelle à son point d’entrée public.

Vous créez un type de démarrage automatique gérés avec le point d’entrée nécessaire en implémentant la *IProcessHostPreloadClient* interface, comme indiqué dans l’exemple suivant :

[!code-csharp[Main](overview/samples/sample7.cs)]

Après l’initialisation de votre code s’exécute dans le *préchargement* méthode et la méthode est retournée, l’application ASP.NET est prête à traiter les demandes.

Avec l’ajout de démarrage automatique pour.5 d’IIS et ASP.NET 4, vous avez maintenant une approche bien définie pour effectuer l’initialisation d’applications coûteuses avant le traitement de la première requête HTTP. Par exemple, vous pouvez utiliser la nouvelle fonctionnalité de démarrage automatique pour initialiser une application, puis signaler un équilibreur de charge que l’application a été initialisé et prêt à accepter le trafic HTTP.

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a>Redirection définitive d’une Page

Il est courant dans les applications Web pour déplacer des pages et autres contenus autour au fil du temps, ce qui peut entraîner une accumulation de liens obsolètes dans les moteurs de recherche. Dans ASP.NET, les développeurs ont traditionnellement géré les requêtes pour les anciennes URL à l’aide à l’aide de la *Response.Redirect* méthode pour transmettre une demande à la nouvelle URL. Toutefois, le *rediriger* méthode émet une réponse HTTP 302 trouvé (redirection temporaire), ce qui entraîne un HTTP supplémentaire aller-retour lorsque les utilisateurs tentent d’accéder à l’ancienne URL.

ASP.NET 4 ajoute un nouveau *RedirectPermanent* méthode d’assistance qui facilite le problème HTTP 301 déplacé de façon permanente les réponses, comme dans l’exemple suivant :

[!code-csharp[Main](overview/samples/sample8.cs)]

Moteurs de recherche et d’autres agents utilisateurs qui reconnaissent les redirections permanentes stockera la nouvelle URL qui est associée avec le contenu, ce qui élimine l’aller-retour inutile effectué par le navigateur pour les redirections temporaires.

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a>Réduction de l’état de Session

ASP.NET fournit deux options par défaut pour stocker l’état de session dans une batterie de serveurs Web : un fournisseur d’état de session qui appelle un serveur d’état de session out-of-process et un fournisseur d’état de session qui stocke les données dans une base de données Microsoft SQL Server. Étant donné que les deux options impliquent le stockage des informations d’état en dehors du processus de travail d’une application Web, l’état de session doit être sérialisé avant d’être envoyé vers le stockage étendu. Selon la quantité d’informations développeur enregistre dans l’état de session, la taille des données sérialisées peut devenir assez volumineuse.

ASP.NET 4 introduit une nouvelle option de compression pour les deux types de fournisseurs d’état de session out-of-process. Lorsque le *compressionEnabled* option de configuration indiquée dans l’exemple suivant est définie sur *true*, ASP.NET sera compresser (et décompresser) état de session sérialisé à l’aide de .NET Framework  *System.IO.Compression.GZipStream* classe.

[!code-xml[Main](overview/samples/sample9.xml)]

Avec le simple ajout du nouvel attribut à la `Web.config` fichier, les applications avec des cycles processeur disponibles sur les serveurs Web peut réaliser des réductions substantielles de la taille des données d’état de session sérialisées.

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a>Étendez la plage d’URL autorisées

ASP.NET 4 introduit de nouvelles options pour l’augmentation de la taille des URL d’application. Les versions précédentes d’ASP.NET contraint les longueurs de chemin d’accès d’URL à 260 caractères, selon la limite de chemin d’accès de fichier NTFS. Dans ASP.NET 4, vous avez la possibilité pour augmenter (ou diminuer) cette limite en fonction de vos applications, à l’aide de deux nouvelles *httpRuntime* attributs de configuration. L’exemple suivant montre ces nouveaux attributs.

[!code-xml[Main](overview/samples/sample10.xml)]

Pour permettre les chemins d’accès plus ou moins longtemps (la partie de l’URL qui n’inclut pas de protocole, nom du serveur et chaîne de requête), modifiez le *[maxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* attribut. Pour permettre les chaînes de requête plus ou moins longtemps, modifiez la valeur de la *[maxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* attribut.

ASP.NET 4 vous permet également de configurer les caractères qui sont utilisés par la vérification des caractères d’URL. Lorsque ASP.NET trouve un caractère non valide dans la partie chemin d’accès d’une URL, il rejette la demande et génère une erreur HTTP 400. Dans les versions précédentes d’ASP.NET, la vérification de caractères d’URL était limitée à un ensemble fixe de caractères. Dans ASP.NET 4, vous pouvez personnaliser le jeu de caractères valides à l’aide de la nouvelle *requestPathInvalidChars* attribut de la *httpRuntime* élément de configuration, comme indiqué dans l’exemple suivant :

[!code-xml[Main](overview/samples/sample11.xml)]

Par défaut, le <em>requestPathInvalidChars</em> attribut définit huit caractères comme étant non valide. (Dans la chaîne est assignée à <em>requestPathInvalidChars</em> par défaut<em>,</em>l’inférieur à (&lt;), supérieur à (&gt;) et « et commercial » (&amp;) sont des caractères codé, car le `Web.config` est un fichier XML.) Vous pouvez personnaliser le jeu de caractères non valides en fonction des besoins.

> [!NOTE]
> Remarque ASP.NET 4 rejette toujours les chemins d’URL qui contiennent des caractères dans la plage ASCII de 0 x 00 à 0x1F, car ce sont des caractères d’URL non valides, tel que défini dans la RFC 2396 de l’IETF ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt)). Sur les versions de Windows Server qui exécutent IIS 6 ou une version ultérieure, le pilote de périphérique de protocole http.sys rejette automatiquement les URL avec ces caractères.


<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a>Validation de requête extensible

Validation de demande ASP.NET recherche les données de requête HTTP entrante pour les chaînes qui sont couramment utilisés dans les attaques par script intersites (XSS). Si les chaînes XSS potentielles sont trouvés, la validation de demande signale la chaîne suspecte et retourne une erreur. La validation de requête intégrées retourne une erreur uniquement lorsqu’il recherche les chaînes courantes utilisées dans les attaques XSS. Précédentes tentatives pour rendre la validation XSS plus agressive a entraîné un trop grand nombre de faux positifs. Les clients peuvent cependant la validation de demande qui est plus agressive ou à l’inverse peut souhaiter assouplir intentionnellement XSS vérifie pour des pages spécifiques ou pour certains types de demandes.

Dans ASP.NET 4, la fonctionnalité de validation de demande a été rendue extensible afin que vous pouvez utiliser la logique de validation de demande personnalisée. Pour étendre la validation de la demande, vous créez une classe qui dérive de la nouvelle *System.Web.Util.RequestValidator* type et que vous configurez l’application (dans le *httpRuntime* section de la `Web.config`fichier) à utiliser le type personnalisé. L’exemple suivant montre comment configurer une classe de validation de demande personnalisé :

[!code-xml[Main](overview/samples/sample12.xml)]

La nouvelle *requestValidationType* attribut nécessite une chaîne d’identificateur de type .NET Framework standard qui spécifie la classe qui assure la validation de requête personnalisé. Pour chaque demande, ASP.NET appelle le type personnalisé pour traiter chaque élément de données de demande HTTP entrantes. L’URL entrante, tous les en-têtes HTTP (cookies et en-têtes personnalisés) et le corps d’entité sont tous disponibles pour l’inspection par une classe de validation de demande personnalisé similaire à celui affiché dans l’exemple suivant :

[!code-csharp[Main](overview/samples/sample13.cs)]

Pour les cas où vous ne souhaitez pas inspecter un élément de données HTTP entrantes, la classe de validation de la demande peut avoir recours à exécuter la validation de demande ASP.NET par défaut en appelant simplement *base. IsValidRequestString.*

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a>La mise en cache de l’objet et de l’objet d’extensibilité de la mise en cache

Depuis sa première version, ASP.NET a inclus un cache des objets en mémoire puissant (*System.Web.Caching.Cache*). L’implémentation de cache a été très répandue, qu’il a été utilisé dans les applications non-Web. Toutefois, il est peu maniable pour une application Windows Forms ou WPF pour inclure une référence à `System.Web.dll` juste pour être en mesure d’utiliser le cache d’objets ASP.NET.

Pour rendre la mise en cache disponible pour toutes les applications, le .NET Framework 4 introduit un nouvel assembly, un nouvel espace de noms, certains types de base et une implémentation de la mise en cache de concret. La nouvelle `System.Runtime.Caching.dll` assembly contient une nouvelle API de mise en cache dans le *System.Runtime.Caching* espace de noms. L’espace de noms contient deux principaux ensembles de classes :

- Types abstraits qui fournissent la base pour générer tout type d’implémentation de cache personnalisée.
- Une implémentation de cache d’objets en mémoire concrète (le *System.Runtime.Caching.MemoryCache* classe).

La nouvelle *MemoryCache* classe s’inspire sur le cache ASP.NET, et partage une grande partie de la logique de moteur de cache interne avec ASP.NET. Bien que les API publiques mise en cache dans *System.Runtime.Caching* ont été mis à jour pour prendre en charge le développement de caches personnalisés, si vous avez utilisé ASP.NET *Cache* objet, vous trouverez des concepts familiers dans le nouvelles API.

Une étude approfondie de la nouvelle *MemoryCache* classe et prenant en charge les API de base nécessiterait un document entier. Toutefois, l’exemple suivant vous donne une idée du fonctionne de la nouvelle API de cache. L’exemple a été écrit pour une application Windows Forms, sans aucune dépendance sur `System.Web.dll`.

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a>Encodage d’en-tête HTTP, l’URL et extensible HTML

Dans ASP.NET 4, vous pouvez créer des routines d’encodage personnalisées pour les tâches de codage de texte courantes suivantes :

- Codage HTML.
- Encodage d’URL.
- Codage d’attribut HTML.
- Encodage des en-têtes HTTP sortants.

Vous pouvez créer un encodeur personnalisé en dérivant de la nouvelle *System.Web.Util.HttpEncoder* type et la configuration d’ASP.NET pour utiliser le type personnalisé dans le *httpRuntime* section de la `Web.config` fichier, en tant que indiqué dans l’exemple suivant :

[!code-xml[Main](overview/samples/sample15.xml)]

Une fois un encodeur personnalisé a été configuré, ASP.NET appelle automatiquement l’implémentation de codage personnalisée chaque fois que public de codage de méthodes de la *System.Web.HttpUtility* ou *System.Web.HttpServerUtility* les classes sont appelées. Cela permet à une partie d’une équipe de développement Web de créer un encodeur personnalisé qui implémente le codage de caractères agressif, tandis que le reste de l’équipe de développement Web continue à utiliser les API d’encodage ASP.NET publiques. En configurant de manière centralisée un encodeur personnalisé dans le *httpRuntime* élément, vous êtes assuré que tous les appels d’encodage de texte à partir de l’API d’encodage ASP.NET publiques sont routés via l’encodeur personnalisé.

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a>Surveillance des performances pour les Applications individuelles dans un seul processus de travail

Afin d’accroître le nombre de sites Web qui peut être hébergé sur un serveur unique, nombreux hébergeurs exécutent plusieurs applications ASP.NET dans un seul processus de travail. Toutefois, si plusieurs applications utilisent un processus de travail partagé unique, il est difficile pour les administrateurs de serveur identifier une application qui rencontre des problèmes.

ASP.NET 4 tire parti des nouvelles fonctionnalités de surveillance des ressources introduites par le CLR. Pour activer cette fonctionnalité, vous pouvez ajouter l’extrait de configuration XML suivant à la `aspnet.config` fichier de configuration.

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> Remarque la `aspnet.config` fichier se trouve dans le répertoire où le .NET Framework est installé. Il n’est pas le `Web.config` fichier.


Lorsque le *appDomainResourceMonitoring* fonctionnalité a été activée, deux nouveaux compteurs de performance sont disponibles dans la catégorie de performance « ASP.NET Applications » : *% du temps processeur géré* et  *Gestion de la mémoire utilisée*. Les deux de ces compteurs de performance utilisent la nouvelle fonctionnalité de gestion de ressources du domaine d’application CLR pour effectuer le suivi de la durée estimée de l’UC et utilisation de la mémoire managée de chaque application ASP.NET. Par conséquent, avec ASP.NET 4, les administrateurs ont désormais une vue plus granulaire de la consommation des ressources d’applications individuelles en cours d’exécution dans un seul processus de travail.

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a>Multi-ciblage

Vous pouvez créer une application qui cible une version spécifique du .NET Framework. Dans ASP.NET 4, un nouvel attribut dans le *compilation* élément de la `Web.config` fichier vous permet de cibler le .NET Framework 4 et versions ultérieur. Si vous ciblez le .NET Framework 4, et si vous incluez des éléments facultatifs dans la `Web.config` fichier tels que les entrées pour *system.codedom*, ces éléments doivent être correctes configurée pour le .NET Framework 4. (Si vous ne ciblez pas explicitement le .NET Framework 4, le framework cible est déduit à partir de l’absence d’une entrée dans le `Web.config` fichier.)

L’exemple suivant illustre l’utilisation de la *targetFramework* d’attribut dans le *compilation* élément de la `Web.config` fichier.

[!code-xml[Main](overview/samples/sample17.xml)]

Remarques sur le ciblage d’une version spécifique du .NET Framework :

- Dans un pool d’applications .NET Framework 4, le système de génération ASP.NET suppose que le .NET Framework 4 en tant que cible si la `Web.config` fichier n’inclut pas le *targetFramework* attribut ou si le `Web.config` fichier est manquant. (Vous devrez peut-être apporter des modifications de codage à votre application pour qu’il s’exécute sous le .NET Framework 4.)
- Si vous n’incluez pas le *targetFramework* attribut et si le *system.codeDom* élément est défini dans le `Web.config` fichier, ce fichier doit contenir les entrées correctes pour le .NET Framework 4.
- Si vous utilisez le *aspnet\_compilateur* commande précompiler votre application (comme dans un environnement de génération), vous devez utiliser la version correcte de la *aspnet\_compilateur* commande pour le framework cible. Utiliser le compilateur fourni avec le .NET Framework 2.0 (% WINDIR%\Microsoft.NET\Framework\v2.0.50727) pour compiler pour le .NET Framework 3.5 et les versions antérieures. Utiliser le compilateur qui est fourni avec le .NET Framework 4 pour compiler des applications créées à l’aide de cette infrastructure ou versions ultérieures.
- Au moment de l’exécution, le compilateur utilise les assemblys framework plus récentes qui sont installés sur l’ordinateur (et par conséquent, dans le GAC). Si une mise à jour est effectuée plus tard à l’infrastructure (par exemple, une hypothétique version 4.1 est installée), vous serez en mesure d’utiliser des fonctionnalités dans la version la plus récente du framework, bien que le *targetFramework* attribut cible une version antérieure (par exemple, 4.0). (Toutefois, au moment du design dans Visual Studio 2010 ou lorsque vous utilisez le *aspnet\_compilateur* commande, à l’aide des nouvelles fonctionnalités du framework entraîne des erreurs du compilateur).

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a>Ajax

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a>jQuery dans les Web Forms et MVC

Les modèles Visual Studio Web Forms et MVC incluent la bibliothèque jQuery open source. Lorsque vous créez un nouveau site Web ou un projet, un dossier de Scripts contenant les 3 fichiers suivants est créé :

- jQuery-1.4.1.js – la contrôlable de visu, unminified version de la bibliothèque jQuery.
- jQuery-14.1.min.js – version réduite de la bibliothèque jQuery.
- jQuery-1.4.1-vsdoc.js : le fichier de documentation d’Intellisense pour la bibliothèque jQuery.

Inclure la version unminified de jQuery lors du développement d’une application. Inclure la version minifiée de jQuery pour les applications de production.

Par exemple, la page Web Forms suivante illustre comment vous pouvez utiliser jQuery pour modifier la couleur d’arrière-plan des contrôles de zone de texte de ASP.NET sur le jaune lorsqu’ils ont le focus.

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a>Prise en charge du réseau de distribution de contenu

Le Microsoft Ajax Content Delivery Network (CDN) vous permet de facilement ajouter ASP.NET Ajax et des scripts jQuery à vos applications Web. Par exemple, vous pouvez démarrer à l’aide de la bibliothèque jQuery en ajoutant simplement un `<script>` balise à votre page qui pointe vers Ajax.microsoft.com comme suit :

[!code-html[Main](overview/samples/sample19.html)]

En tirant parti du CDN Microsoft Ajax, vous pouvez considérablement améliorer les performances de vos applications Ajax. Le contenu du CDN Microsoft Ajax est mis en cache sur les serveurs situés dans le monde entier. En outre, le CDN Microsoft Ajax permet aux navigateurs de réutiliser des fichiers JavaScript mis en cache pour les sites Web qui se trouvent dans des domaines différents.

Microsoft Ajax Content Delivery Network prend en charge SSL (HTTPS) au cas où vous deviez traiter une page web à l’aide de Secure Sockets Layer.

Implémenter une solution de secours lorsque le CDN n’est pas disponible. Tester la procédure de secours.

Pour en savoir plus sur le CDN Microsoft Ajax, visitez le site Web suivant :

[https://www.asp.net/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

Le ScriptManager ASP.NET prend en charge le CDN Microsoft Ajax. Simplement en définir une propriété, la propriété EnableCdn, vous pouvez récupérer tous les fichiers JavaScript de framework ASP.NET à partir du CDN :

[!code-aspx[Main](overview/samples/sample20.aspx)]

Une fois que vous définissez la propriété EnableCdn sur la valeur true, l’infrastructure ASP.NET récupère tous les fichiers JavaScript de framework ASP.NET à partir du CDN, y compris tous les fichiers JavaScript utilisés pour la validation et le contrôle UpdatePanel. Définition de cette un propriété peut avoir un impact considérable sur les performances de votre application web.

Vous pouvez définir le chemin d’accès CDN pour vos propres fichiers JavaScript à l’aide de l’attribut WebResource. La nouvelle propriété CdnPath Spécifie le chemin d’accès au CDN utilisé lorsque vous définissez la propriété EnableCdn sur la valeur true :

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a>Scripts explicite de ScriptManager

Dans le passé, si vous avez utilisé le ScriptManger ASP.NET puis vous deviez charger la bibliothèque Ajax de ASP.NET monolithique entière. En tirant parti de la nouvelle propriété ScriptManager.AjaxFrameworkMode, vous pouvez contrôler exactement quels composants de la bibliothèque Ajax ASP.NET sont chargés et charger uniquement les composants de la bibliothèque Ajax ASP.NET dont vous avez besoin.

La propriété ScriptManager.AjaxFrameworkMode peut définir les valeurs suivantes :

- Activé : Spécifie que le contrôle ScriptManager inclut automatiquement le fichier de script MicrosoftAjax.js, qui est un fichier de script combiné de chaque script d’infrastructure principale (comportement hérité).
- Désactivé : Spécifie que toutes les fonctionnalités de script Microsoft Ajax sont désactivées et que le contrôle ScriptManager ne référence pas automatiquement tous les scripts.
- Explicite--Spécifie que vous inclurez explicitement des références de script au fichier de script individuels infrastructure principale requis par votre page et que vous inclurez des références aux dépendances requises par chaque fichier de script.

Par exemple, si vous définissez la propriété AjaxFrameworkMode à la valeur Explicit vous pouvez spécifier les scripts de composant ASP.NET Ajax particuliers dont vous avez besoin :

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a>Web Forms

Web Forms a été une fonctionnalité principale dans ASP.NET depuis la version d’ASP.NET 1.0. De nombreuses améliorations ont été dans cette zone pour ASP.NET 4, notamment les suivantes :

- La possibilité de définir *meta* balises.
- Davantage de contrôle sur l’état d’affichage.
- Moyens plus simples pour travailler avec les fonctionnalités du navigateur.
- Prise en charge à l’aide du routage ASP.NET avec Web Forms.
- Davantage de contrôle sur les ID générés.
- La possibilité de conserver les lignes sélectionnées dans les contrôles de données.
- Davantage de contrôle sur le HTML restitué dans le *FormView* et *ListView* contrôles.
- Filtrage de la prise en charge pour les contrôles de source de données.

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a>Définition de balises Meta avec les propriétés Page.MetaKeywords et Page.MetaDescription

ASP.NET 4 ajoute deux propriétés pour le *Page* (classe), *toute* et *MetaDescription*. Ces deux propriétés représentent correspondant *meta* balises dans votre page, comme indiqué dans l’exemple suivant :

[!code-aspx[Main](overview/samples/sample23.aspx)]

Ces deux propriétés fonctionnent de la même façon que la page *titre* propriété. Ils suivent ces règles :

1. S’il existe aucune *meta* des balises dans le *head* élément qui correspondent aux noms de propriété (autrement dit, le nom = « keywords » pour *Page.MetaKeywords* et le nom = « description » de  *Page.MetaDescription*, ce qui signifie que ces propriétés n’ont pas été définies), le *meta* balises seront ajoutés à la page lorsqu’il est restitué.
2. S’il existe déjà *meta* balises avec ces noms, ces propriétés servent de méthodes pour le contenu des balises existantes get et set.

Vous pouvez définir ces propriétés au moment de l’exécution, ce qui vous permet d’obtenir le contenu à partir d’une base de données ou une autre source, et ce qui vous permet de définir les balises dynamiquement de décrire ce qui concerne les une page particulière.

Vous pouvez également définir le *mots clés* et *Description* propriétés dans le *@ Page* directive en haut de la balise de page Web Forms, comme dans l’exemple suivant :

[!code-aspx[Main](overview/samples/sample24.aspx)]

Cela remplacera le *meta* balise contenu (le cas échéant) est déjà déclaré dans la page.

Le contenu de la description *meta* balise sont utilisés pour l’amélioration de la recherche répertoriant les aperçus dans Google. (Pour plus d’informations, consultez [améliorer les extraits de code avec une modernisation de description meta](http://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html) sur le blog de l’administrateur Web de Google Central.) Google et Windows Live Search n’utilisent pas le contenu des mots clés pour quoi que ce soit, mais peut-être d’autres moteurs de recherche. Pour plus d’informations, consultez [conseils de mots clés Meta](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php) sur le site Web de Guide de moteur de recherche.

Ces nouvelles propriétés sont une fonctionnalité simple, mais ils vous faire gagner de l’exigence de les ajouter manuellement ou d’écrire votre propre code pour créer le *meta* balises.

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a>L’activation de l’état d’affichage pour les contrôles individuels

Par défaut, l’état d’affichage est activé pour la page, avec le résultat que chaque contrôle sur la page potentiellement stocke l’état d’affichage, même si elle n’est pas obligatoire pour l’application. Données d’état d’affichage sont incluses dans le balisage qu’une page génère et augmente la quantité de temps que nécessaire pour envoyer une page au client et de publier de nouveau. Stocker l’état d’affichage plus que nécessaire peut provoquer une dégradation significative des performances. Dans les versions antérieures d’ASP.NET, les développeurs pourraient désactiver état d’affichage pour les contrôles individuels afin de réduire la taille de la page, mais eu à faire de façon explicite pour les contrôles individuels. Dans ASP.NET 4, les contrôles serveur Web incluent un *ViewStateMode* propriété qui vous permet de désactiver l’état d’affichage par défaut, puis l’activer uniquement pour les contrôles qui en ont besoin dans la page.

Le *ViewStateMode* propriété accepte une énumération qui a trois valeurs : *activé*, *désactivé*, et *hériter*. *Activé* permet d’afficher d’état pour ce contrôle et pour tous les contrôles enfants qui sont définies sur *hériter* ou que rien avoir. *Désactivé* désactive l’état d’affichage et *hériter* Spécifie que le contrôle utilise le *ViewStateMode* définition à partir du contrôle parent.

L’exemple suivant montre comment la *ViewStateMode* fonctionne de la propriété. Le balisage et le code pour les contrôles dans la page suivante inclut des valeurs pour le *ViewStateMode* propriété :

[!code-aspx[Main](overview/samples/sample25.aspx)]

Comme vous pouvez le voir, le code désactive état d’affichage pour le contrôle PlaceHolder1. Le contrôle de label1 enfant hérite de cette valeur de propriété (*hériter* est la valeur par défaut *ViewStateMode* pour les contrôles.) et l’enregistre donc aucun état d’affichage. Dans le contrôle Espace_réservé2, *ViewStateMode* a la valeur *activé*, de sorte que label2 hérite de cette propriété et enregistre état d’affichage. Lorsque la page est chargée, le *texte* propriété des deux *étiquette* contrôles est définie sur la chaîne « [DynamicValue] ».

L’effet de ces paramètres est que lorsque la page charge de la première fois, la sortie suivante s’affiche dans le navigateur :

Désactivé `: [DynamicValue]`

Activé :`[DynamicValue]`

Toutefois, après une publication (postback), la sortie suivante s’affiche :

Désactivé `: [DeclaredValue]`

Activé :`[DynamicValue]`

Le contrôle label1 (dont *ViewStateMode* a la valeur *désactivé*) n’a pas conservé la valeur qui lui est affectée dans le code. Toutefois, les de label2 contrôler (dont *ViewStateMode* a la valeur *activé*) a conservé son état.

Vous pouvez également définir *ViewStateMode* dans le *@ Page* directive, comme dans l’exemple suivant :

[!code-aspx[Main](overview/samples/sample26.aspx)]

Le *Page* classe est simplement un autre contrôle ; il agit comme le contrôle parent pour tous les autres contrôles dans la page. La valeur par défaut *ViewStateMode* est *activé* pour les instances de *Page*. Étant donné que les contrôles par défaut à *hériter*, contrôles héritent le *activé* valeur de propriété, sauf si vous définissez *ViewStateMode* au niveau page ou du contrôle.

La valeur de la *ViewStateMode* propriété détermine si l’état d’affichage est conservée uniquement si le *EnableViewState* propriété est définie sur *true*. Si le *EnableViewState* propriété est définie sur *false*, état d’affichage n’est pas conservé même si *ViewStateMode* a la valeur *activé*.

Un bon usage de cette fonctionnalité est avec *ContentPlaceHolder* contrôles dans les pages maîtres, où vous pouvez définir *ViewStateMode* à *désactivé* pour le masque de page, puis l’activer individuellement pour *ContentPlaceHolder* contrôles qui contiennent à leur tour des contrôles qui nécessitent l’état d’affichage.

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a>Modifications apportées aux fonctionnalités du navigateur

ASP.NET détermine les fonctionnalités du navigateur à l’aide d’un utilisateur à parcourir votre site à l’aide d’une fonctionnalité appelée *fonctionnalités du navigateur*. Fonctionnalités du navigateur sont représentées par le *HttpBrowserCapabilities* objet (exposé par le *Request.Browser* propriété). Par exemple, vous pouvez utiliser la *HttpBrowserCapabilities* objet pour déterminer si le type et la version du navigateur actuel prend en charge une version particulière de JavaScript. Vous pouvez également utiliser le *HttpBrowserCapabilities* objet pour déterminer si la demande provient d’un appareil mobile.

Le *HttpBrowserCapabilities* objet est piloté par un ensemble de fichiers de définition de navigateur. Ces fichiers contiennent des informations sur les capacités des navigateurs particuliers. Dans ASP.NET 4, ces fichiers de définition de navigateur ont été mis à jour contiennent des informations sur les navigateurs récemment introduites et les appareils tels que Google Chrome, Research de smartphones de Motion BlackBerry et Apple iPhone.

La liste suivante présente des fichiers de définition de nouveau navigateur :

- *blackberry.browser*
- *chrome.browser*
- *Default.Browser*
- *firefox.browser*
- *gateway.browser*
- *generic.browser*
- *ie.browser*
- *iemobile.browser*
- *iphone.browser*
- *opera.browser*
- *safari.browser*

#### <a name="using-browser-capabilities-providers"></a>À l’aide de fournisseurs de fonctionnalités de navigateur

Dans ASP.NET version 3.5 Service Pack 1, vous pouvez définir les fonctionnalités un navigateur dispose de plusieurs manières :

- Au niveau de l’ordinateur, vous créez ou mettez à jour un `.browser` fichier XML dans le dossier suivant :

- [!code-console[Main](overview/samples/sample27.cmd)]

- Une fois que vous définissez la capacité du navigateur, vous exécutez la commande suivante à partir de la Visual Studio invite de commandes afin de régénérer l’assembly de fonctionnalités de navigateur et ajoutez-le dans le GAC :

- [!code-console[Main](overview/samples/sample28.cmd)]

- Pour une application donnée, vous créez un `.browser` fichier dans l’application `App_Browsers` dossier.

Ces approches vous obligent à modifier des fichiers XML et les modifications au niveau de l’ordinateur, vous devez redémarrer l’application après avoir exécuté le compte aspnet\_regbrowsers.exe processus.

ASP.NET 4 inclut une fonctionnalité appelée *les fournisseurs de fonctionnalités de navigateur*. Comme son nom l’indique, ce vous permet de créer un fournisseur qui à son tour vous permet d’utilise votre propre code pour déterminer les fonctionnalités du navigateur.

Dans la pratique, les développeurs ne définissent souvent pas les fonctionnalités de navigateur personnalisé. Explorateur de fichiers sont difficile à mettre à jour, le processus pour les mettre à jour est assez complexe et la syntaxe XML de `.browser` fichiers peuvent être complexes à utiliser et à définir. Ce qui rendrait ce processus beaucoup plus facile est s’il y avait une syntaxe de définition de navigateur courants, ou une base de données contenant des définitions de navigateur à jour, ou même un service Web pour une base de données. La nouvelle fonctionnalité de fournisseurs de fonctionnalités de navigateur rend ces scénarios possibles et pratiques pour les développeurs tiers.

Il existe deux approches principales pour l’utilisation de la nouvelle fonctionnalité de fournisseur de fonctionnalités de navigateur ASP.NET 4 : étendre les fonctionnalités du navigateur ASP.NET les fonctionnalités de définition, ou de le remplacer totalement. Les sections suivantes décrivent tout d’abord comment remplacer la fonctionnalité, puis comment l’étendre.

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a>Remplacement de la fonctionnalité de fonctionnalités du navigateur ASP.NET

Pour remplacer complètement la fonction de définition de fonctionnalités de navigateur ASP.NET, procédez comme suit :

1. Créer une classe de fournisseur qui dérive de *HttpCapabilitiesProvider* et qui remplace le *GetBrowserCapabilities* méthode, comme dans l’exemple suivant : 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    Le code dans cet exemple crée un nouveau *HttpBrowserCapabilities* d’objet, en spécifiant uniquement la fonctionnalité nommée navigateur et en définissant cette fonctionnalité sur MyCustomBrowser.
2. Inscrire le fournisseur avec l’application. 

    Pour utiliser un fournisseur avec une application, vous devez ajouter le *fournisseur* attribut le *browserCaps* section dans le `Web.config` ou `Machine.config` fichiers. (Vous pouvez également définir les attributs de fournisseur dans un *emplacement* , élément pour les répertoires spécifiques dans l’application, par exemple dans un dossier pour un appareil mobile spécifique.) L’exemple suivant montre comment définir le *fournisseur* attribut dans un fichier de configuration :

    [!code-xml[Main](overview/samples/sample30.xml)]

    Une autre façon d’inscrire la nouvelle définition de fonctionnalité de navigateur consiste à utiliser le code, comme illustré dans l’exemple suivant :

    [!code-csharp[Main](overview/samples/sample31.cs)]

    Ce code doit s’exécuter le *Application\_Démarrer* événement de le `Global.asax` fichier. Toute modification apportée à la *BrowserCapabilitiesProvider* classe doit se produire avant l’exécution de n’importe quel code dans l’application, afin de vous assurer que le cache reste dans un état valide pour le résoudre *HttpCapabilitiesBase* objet.

#### <a name="caching-the-httpbrowsercapabilities-object"></a>La mise en cache l’objet HttpBrowserCapabilities

L’exemple précédent a un problème, c'est-à-dire que le code s’exécute chaque fois que le fournisseur personnalisé est appelé afin d’obtenir le *HttpBrowserCapabilities* objet. Cela peut se produire plusieurs fois au cours de chaque demande. Dans l’exemple, le code pour le fournisseur ne fait pas. Toutefois, si le code de votre fournisseur personnalisé effectue un travail considérable dans afin d’obtenir le *HttpBrowserCapabilities* de l’objet, cela peut affecter les performances. Pour éviter ce problème, vous pouvez mettre en cache le *HttpBrowserCapabilities* objet. Procédez comme suit :

1. Créez une classe qui dérive de *HttpCapabilitiesProvider*, comme celui de l’exemple suivant : 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    Dans l’exemple, le code génère une clé de cache en appelant une méthode BuildCacheKey personnalisée, et elle obtient la longueur de la mise en cache en appelant une méthode GetCacheTime personnalisée. Le code ajoute ensuite résolu *HttpBrowserCapabilities* objet au cache. L’objet peut être récupéré à partir du cache et réutilisé sur les demandes suivantes qui utilisent le fournisseur personnalisé.
2. Inscrire le fournisseur avec l’application comme décrit dans la procédure précédente.

#### <a name="extending-aspnet-browser-capabilities-functionality"></a>Extension des fonctionnalités de fonctionnalités de navigateur ASP.NET

La section précédente a décrit comment créer un nouveau *HttpBrowserCapabilities* objet dans ASP.NET 4. Vous pouvez également étendre les fonctionnalités de fonctionnalités du navigateur ASP.NET en ajoutant de nouvelles définitions de fonctionnalités de navigateur à ceux qui sont déjà dans ASP.NET. Vous pouvez faire cela sans en utilisant les définitions du navigateur XML. La procédure suivante montre comment.

1. Créez une classe qui dérive de *HttpCapabilitiesEvaluator* et qui remplace le *GetBrowserCapabilities* méthode, comme indiqué dans l’exemple suivant : 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    Ce code utilise d’abord la fonctionnalité de fonctionnalités du navigateur ASP.NET pour identifier le navigateur. Toutefois, si aucun navigateur n’est identifié en fonction des informations définies dans la demande (autrement dit, si le *navigateur* propriété de la *HttpBrowserCapabilities* objet est la chaîne « Unknown »), les appels de code le fournisseur personnalisé (MyBrowserCapabilitiesEvaluator) pour identifier le navigateur.
2. Inscrire le fournisseur avec l’application comme décrit dans l’exemple précédent.

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a>Extension des fonctionnalités de fonctionnalités de navigateur en ajoutant de nouvelles fonctionnalités à des définitions de fonctions existantes

Outre la création d’un fournisseur de définition de navigateur personnalisé et pour créer dynamiquement des définitions de navigateur, vous pouvez étendre les définitions de navigateur existantes avec des fonctionnalités supplémentaires. Cela vous permet d’utiliser une définition qui est proche de ce que vous voulez, mais ne dispose pas de seulement quelques fonctionnalités. Pour ce faire, effectuez les étapes suivantes.

1. Créez une classe qui dérive de *HttpCapabilitiesEvaluator* et qui remplace le *GetBrowserCapabilities* méthode, comme indiqué dans l’exemple suivant : 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    L’exemple de code étend ASP.NET existants *HttpCapabilitiesEvaluator* classe et obtient le *HttpBrowserCapabilities* objet qui correspond à la définition de la requête actuelle en utilisant le code suivant :

    [!code-csharp[Main](overview/samples/sample35.cs)]

    Le code peut ensuite ajouter ou modifier une fonctionnalité pour ce navigateur. Il existe deux façons de spécifier une nouvelle fonctionnalité de navigateur :

    - Ajouter une paire clé/valeur pour le *IDictionary* objet exposé par le *fonctionnalités* propriété de la *HttpCapabilitiesBase* objet. Dans l’exemple précédent, le code ajoute une fonctionnalité nommée MultiTouch avec la valeur de *true*.
    - Définir les propriétés existantes de la *HttpCapabilitiesBase* objet. Dans l’exemple précédent, le code définit le *Frames* propriété *true*. Cette propriété est simplement un accesseur pour le *IDictionary* objet exposé par le *fonctionnalités* propriété. 

        > [!NOTE]
        > Notez ce modèle s’applique à n’importe quelle propriété de *HttpBrowserCapabilities*, y compris les adaptateurs de contrôle.
2. Inscrire le fournisseur avec l’application comme décrit dans la procédure précédente.

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a>Routage dans ASP.NET 4

ASP.NET 4 ajoute la prise en charge intégrée pour l’utilisation du routage avec les Web Forms. URL qui ne sont pas mappent à des fichiers physiques de demande de routage vous permet de que configurer une application à accepter. Au lieu de cela, vous pouvez utiliser le routage pour définir des URL qui sont significatives pour les utilisateurs et qui peut faciliter l’optimisation des moteurs de recherche (SEO) pour votre application. Par exemple, l’URL pour une page qui affiche les catégories de produits dans une application existante peut se présenter comme dans l’exemple suivant :

[!code-console[Main](overview/samples/sample36.cmd)]

En utilisant le routage, vous pouvez configurer l’application pour accepter l’URL suivante pour afficher les mêmes informations :

[!code-console[Main](overview/samples/sample37.cmd)]

Routage a été disponible à partir de ASP.NET 3.5 SP1. (Pour obtenir un exemple montrant comment utiliser le routage dans ASP.NET 3.5 SP1, consultez l’entrée [à l’aide de routage avec Web Forms](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "titre de cette entrée.") sur du blog de Phil.) Toutefois, ASP.NET 4 inclut certaines fonctionnalités qui le rendent plus facile à utiliser le routage, notamment les suivantes :

- Le *PageRouteHandler* (classe), qui est un gestionnaire HTTP simple que vous utilisez lorsque vous définissez des itinéraires. La classe transmet des données à la page qui la demande est acheminée vers.
- Les nouvelles propriétés *HttpRequest.RequestContext* et *Page.RouteData* (qui est un proxy pour le *HttpRequest.RequestContext.RouteData* objet). Ces propriétés facilitent pour accéder aux informations qui est passé à partir de l’itinéraire.
- Les suivant nouveaux générateurs d’expressions, qui sont définies dans *System.Web.Compilation.RouteUrlExpressionBuilder* et *System.Web.Compilation.RouteValueExpressionBuilder*:
- *RouteUrl*, qui fournit un moyen simple de créer une URL qui correspond à une URL de routage au sein d’un contrôle serveur ASP.NET.
- *RouteValue*, qui fournit un moyen simple d’extraire des informations à partir de la *RouteContext* objet.
- Le *RouteParameter* (classe), ce qui le rend plus facile de transmettre des données contenues dans un *RouteContext* objet à une requête pour un contrôle de source de données (semblable à [ *FormParameter* ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).

#### <a name="routing-for-web-forms-pages"></a>Routage pour les Pages Web Forms

L’exemple suivant montre comment définir un itinéraire de Web Forms à l’aide de la nouvelle *MapPageRoute* méthode de la *itinéraire* classe :

[!code-csharp[Main](overview/samples/sample38.cs)]

ASP.NET 4 présente les *MapPageRoute* (méthode). L’exemple suivant est équivalent à la définition de SearchRoute indiquée dans l’exemple précédent, mais utilise le *PageRouteHandler* classe.

[!code-csharp[Main](overview/samples/sample39.cs)]

Le code dans l’exemple mappe l’itinéraire à une page physique (dans le premier itinéraire à `~/search.aspx`). La première définition de l’itinéraire spécifie également que le paramètre nommé searchterm doit être extrait à partir de l’URL et passé à la page.

Le *MapPageRoute* méthode prend en charge les surcharges de méthode suivantes :

- *MapPageRoute (routeName de chaîne, chaîne routeUrl, physicalFile de chaîne, bool checkPhysicalUrlAccess)*
- *MapPageRoute (routeName de chaîne, chaîne routeUrl, physicalFile de chaîne, bool checkPhysicalUrlAccess, valeurs par défaut RouteValueDictionary)*
- *MapPageRoute (routeName de chaîne, chaîne routeUrl, physicalFile de chaîne, bool checkPhysicalUrlAccess, valeurs par défaut RouteValueDictionary, RouteValueDictionary contraintes)*

Le *checkPhysicalUrlAccess* paramètre spécifie si l’itinéraire doit vérifier les autorisations de sécurité pour la page physique qui est routé vers (dans ce cas, search.aspx) et les autorisations sur l’URL entrante (dans ce cas, de recherche / {searchterm}). Si la valeur de *checkPhysicalUrlAccess* est *false*, seules les autorisations de l’URL entrante seront vérifiées. Ces autorisations sont définies dans le `Web.config` fichier à l’aide des paramètres tels que les suivants :

[!code-xml[Main](overview/samples/sample40.xml)]

Dans l’exemple de configuration, l’accès est refusé à la page physique `search.aspx` pour tous les utilisateurs sauf ceux qui se trouvent dans le rôle d’administrateur. Lorsque le *checkPhysicalUrlAccess* paramètre est défini sur *true* (qui est sa valeur par défaut), seuls des utilisateurs administrateurs sont autorisés à accéder la /search/ URL {searchterm}, car la page physique search.aspx est limité aux utilisateurs appartenant à ce rôle. Si *checkPhysicalUrlAccess* a la valeur *false* et le site est configuré comme indiqué dans l’exemple précédent, tous les utilisateurs authentifiés sont autorisés à accéder à l’URL de /search/ {searchterm}.

#### <a name="reading-routing-information-in-a-web-forms-page"></a>Lecture d’informations de routage dans une Page Web Forms

Dans le code de la page physique de Web Forms, vous pouvez accéder aux informations de routage a extrait à partir de l’URL (ou d’autres informations sur un autre objet ayant ajouté à la *RouteData* objet) à l’aide de deux nouvelles propriétés :  *HttpRequest.RequestContext* et *Page.RouteData*. (*Page.RouteData* encapsule *HttpRequest.RequestContext.RouteData*.) L’exemple suivant montre comment utiliser *Page.RouteData*.

[!code-csharp[Main](overview/samples/sample41.cs)]

Le code extrait la valeur qui a été passée pour le paramètre searchterm, comme défini précédemment dans l’exemple d’itinéraire. Considérez l’URL de demande suivante :

[!code-console[Main](overview/samples/sample42.cmd)]

Lorsque cette requête est effectuée, le mot « scott » est rendu dans le `search.aspx` page.

#### <a name="accessing-routing-information-in-markup"></a>L’accès aux informations de routage dans le balisage

La méthode décrite dans la section précédente montre comment obtenir les données d’itinéraire dans le code dans une page Web Forms. Vous pouvez également utiliser des expressions dans le balisage qui vous donnent accès aux mêmes informations. Générateurs d’expressions sont un moyen puissant et élégant pour travailler avec le code déclaratif. (Pour plus d’informations, consultez l’entrée [Express vous-même avec personnalisé générateurs d’expressions](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) sur du blog de Phil.)

ASP.NET 4 inclut deux nouveaux générateurs d’expression pour le routage des Web Forms. L’exemple suivant montre comment les utiliser.

[!code-aspx[Main](overview/samples/sample43.aspx)]

Dans l’exemple, le *RouteUrl* expression est utilisée pour définir une URL qui est basée sur un paramètre d’itinéraire. Cela vous évite d’avoir à coder en dur l’URL complète dans le balisage et vous permet de modifier la structure d’URL plus tard sans nécessiter de toute modification apportée à ce lien.

Selon l’itinéraire défini précédemment, ce balisage génère l’URL suivante :

[!code-console[Main](overview/samples/sample44.cmd)]

ASP.NET fonctionne automatiquement l’itinéraire correct (autrement dit, il génère l’URL correcte) en fonction des paramètres d’entrée. Vous pouvez également inclure un nom d’itinéraire dans l’expression, qui vous permet de spécifier un itinéraire à utiliser.

L’exemple suivant montre comment utiliser le *RouteValue* expression.

[!code-aspx[Main](overview/samples/sample45.aspx)]

Lorsque la page qui contient ce contrôle s’exécute, la valeur « scott » s’affiche dans l’étiquette.

Le *RouteValue* expression rend facile à utiliser les données d’itinéraire dans le balisage, et elle évite d’avoir à travailler avec le plus complexe Page.RouteData["x »] syntaxe dans le balisage.

#### <a name="using-route-data-for-data-source-control-parameters"></a>À l’aide des données d’itinéraire pour les paramètres de contrôle de Source de données

Le *RouteParameter* classe vous permet de spécifier les données d’itinéraire comme une valeur de paramètre pour les requêtes dans un contrôle de source de données. Il [fonctionne comme beaucoup le](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx) classe, comme indiqué dans l’exemple suivant :

[!code-aspx[Main](overview/samples/sample46.aspx)]

Dans ce cas, la valeur de la searchterm de paramètre d’itinéraire sera utilisée pour le @companyname paramètre dans le <em>sélectionnez</em> instruction.

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a>Définition d’ID de Client

La nouvelle *ClientIDMode* propriété résout un problème de longue date dans ASP.NET, à savoir comment créent des contrôles le *id* attribut pour les éléments qui leur rendu. Connaître le *id* attribut pour les éléments rendus est important si votre application inclut un script client qui fait référence à ces éléments.

Le *id* attribut au format HTML qui est rendu pour les contrôles serveur Web est généré en fonction de la *ClientID* propriété du contrôle. Jusqu'à ce que ASP.NET 4, l’algorithme permettant de générer le *id* attribut à partir de la *ClientID* la propriété a été pour concaténer le conteneur de dénomination (le cas échéant) avec l’ID et dans le cas des contrôles répétés (comme dans contrôles de données), pour ajouter un préfixe et un numéro séquentiel. Bien que cela a toujours la garantie que les ID des contrôles dans la page sont uniques, l’algorithme a procuré des ID qui n’étaient pas prévisible et par conséquent difficiles à référence dans le script client de contrôle.

La nouvelle *ClientIDMode* propriété vous permet de spécifier plus précisément comment l’ID client est généré pour les contrôles. Vous pouvez définir le *ClientIDMode* propriété pour n’importe quel contrôle, y compris pour la page. Les paramètres possibles sont les suivantes :

- *AutoID* – cela équivaut à l’algorithme de génération *ClientID* les valeurs de propriété qui a été utilisé dans les versions antérieures d’ASP.NET.
- *Statique* – cela spécifie que le *ClientID* valeur sera la même que l’ID sans concaténation de l’ID du parent aux noms de conteneurs. Cela peut être utile dans les contrôles utilisateur Web. Étant donné que le contrôle utilisateur Web peut se trouver sur des pages différentes et dans les contrôles de conteneur différent, il peut être difficile à écrire le script client pour les contrôles qui utilisent la *AutoID* algorithme, car vous ne pouvez pas prédire les valeurs d’ID sera .
- *Prévisible* – cette option est principalement pour une utilisation dans les contrôles de données qui utilisent les modèles répétés. Il concatène les propriétés de l’ID de conteneurs d’attribution de noms du contrôle, mais généré *ClientID* valeurs ne contiennent pas de chaînes telles que « ctlxxx ». Ce paramètre fonctionne conjointement avec le *ClientIDRowSuffix* propriété du contrôle. Vous définissez le *ClientIDRowSuffix* propriété au nom d’un champ de données, et la valeur de ce champ est utilisée en guise de suffixe pour généré *ClientID* valeur. En général, vous utiliseriez la clé primaire d’un enregistrement de données en tant que le *ClientIDRowSuffix* valeur.
- *Hériter* – ce paramètre est le comportement par défaut pour les contrôles ; elle spécifie que la génération d’ID d’un contrôle est identique à son parent.

Vous pouvez définir le *ClientIDMode* propriété au niveau de la page. Cela définit la valeur par défaut *ClientIDMode* valeur pour tous les contrôles dans la page actuelle.

La valeur par défaut *ClientIDMode* valeur au niveau de la page est *AutoID*et la valeur par défaut *ClientIDMode* valeur au niveau du contrôle est *hériter*. Par conséquent, si vous ne définissez pas n’importe où cette propriété dans votre code, tous les contrôles par défaut est le *AutoID* algorithme.

Vous définissez la valeur au niveau des pages dans le *@ Page* directive, comme indiqué dans l’exemple suivant :

[!code-aspx[Main](overview/samples/sample47.aspx)]

Vous pouvez également définir le *ClientIDMode* valeur dans le fichier de configuration au niveau de l’ordinateur (machine) ou au niveau de l’application. Cela définit la valeur par défaut *ClientIDMode* paramètre pour tous les contrôles dans toutes les pages de l’application. Si vous définissez la valeur au niveau de l’ordinateur, il définit la valeur par défaut *ClientIDMode* configuration pour tous les sites Web sur cet ordinateur. L’exemple suivant montre le *ClientIDMode* définissant dans le fichier de configuration :

[!code-xml[Main](overview/samples/sample48.xml)]

Comme indiqué précédemment, la valeur de la *ClientID* propriété est dérivée à partir du conteneur d’attribution de noms pour le parent d’un contrôle. Dans certains scénarios, comme lorsque vous utilisez des pages maîtres, contrôles peuvent vous retrouver avec des ID que celui illustré à la suivante HTML restitué :

[!code-html[Main](overview/samples/sample49.html)]

Même si le *d’entrée* élément indiqué dans le balisage (à partir d’un *zone de texte* contrôle) est uniquement deux conteneurs d’attribution de noms profondeur dans la page (imbriqué *ContentPlaceholder* contrôles), en raison du mode de traitement des pages maîtres, le résultat final est un ID de contrôle comme suit :

[!code-console[Main](overview/samples/sample50.cmd)]

Cet ID est garanti être unique dans la page, mais est inutilement long pour la plupart des cas. Imaginez que vous souhaitiez afin de réduire la longueur de l’ID de rendu et à mieux contrôler la façon dont l’ID est généré. (Par exemple, vous souhaitez éliminer les préfixes « ctlxxx ».) Le plus simple pour y parvenir consiste en définissant le *ClientIDMode* comme indiqué dans l’exemple suivant :

[!code-aspx[Main](overview/samples/sample51.aspx)]

Dans cet exemple, le *ClientIDMode* propriété est définie sur *statique* pour le plus externe *NamingPanel* élément et la valeur *prédictible* pour l’exception interne *NamingControl* élément. Ces réglages entraînent dans le balisage suivant (le reste de la page et la page maître est censé pour être le même que dans l’exemple précédent) :

[!code-html[Main](overview/samples/sample52.html)]

Le *statique* paramètre a pour effet de la réinitialisation de la hiérarchie d’affectation de noms pour tous les contrôles à l’intérieur de l’extérieur *NamingPanel* élément et d’éliminer le *ContentPlaceHolder* et *MasterPage* ID à partir de l’ID généré. (Le *nom* attribut des éléments rendus n’est pas affecté, donc les fonctionnalités ASP.NET normales sont conservée pour les événements, l’état d’affichage et ainsi de suite.) Un effet secondaire de la réinitialisation de la hiérarchie d’affectation de noms est que même si vous déplacez le balisage pour le *NamingPanel* éléments vers un autre *ContentPlaceholder* contrôle, les ID de client rendu restent les mêmes.

> [!NOTE]
> Notez que c’est à vous pour vous assurer que les ID de rendu du contrôle sont uniques. Si elles ne sont pas, il peut rompre toutes les fonctionnalités nécessitant des identificateurs uniques pour les éléments HTML individuels, tels que le client *document.getElementById* (fonction).


#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a>Création d’ID de Client prévisibles dans des contrôles liés aux données

Le *ClientID* valeurs qui sont générés pour les contrôles dans un contrôle de liste lié aux données par l’algorithme héritée peuvent être long et ne sont pas vraiment prévisibles. Le *ClientIDMode* fonctionnalité peut vous aider à avoir plus de contrôle sur la façon dont ces ID sont générés.

Le balisage dans l’exemple suivant inclut un *ListView* contrôle :

[!code-aspx[Main](overview/samples/sample53.aspx)]

Dans l’exemple précédent, le *ClientIDMode* et *RowClientIDRowSuffix* propriétés sont définies dans le balisage. Le *ClientIDRowSuffix* propriété peut être utilisée uniquement dans les contrôles liés aux données et son comportement est différent selon laquelle le contrôle que vous utilisez. Les différences sont celles-ci :

- *GridView* contrôle, vous pouvez spécifier le nom d’une ou plusieurs colonnes dans la source de données qui sont associées au moment de l’exécution pour créer des ID de client. Par exemple, si vous définissez *RowClientIDRowSuffix* « ProductName, ProductId », ID de contrôle pour les éléments rendus a un format comme suit :

- [!code-console[Main](overview/samples/sample54.cmd)]

- *ListView* contrôle, vous pouvez spécifier une seule colonne dans la source de données est ajoutée à l’ID de client. Par exemple, si vous définissez *ClientIDRowSuffix* à « ProductName », le rendu du contrôle ID a un format comme suit :

- [!code-console[Main](overview/samples/sample55.cmd)]

- Dans ce cas, la fin 1 est dérivée de l’ID de produit de l’élément de données actuel.

- *Repeater* contrôle, ce contrôle ne prend pas en charge la *ClientIDRowSuffix* propriété. Dans un *Repeater* contrôle, l’index de la ligne actuelle est utilisé. Lorsque vous utilisez la propriété ClientIDMode = « Prédictible » avec un *Repeater* contrôler, client ID sont générés et qui ont le format suivant :

- [!code-console[Main](overview/samples/sample56.cmd)]

- La valeur 0 à droite est l’index de la ligne actuelle.

Le *FormView* et *DetailsView* contrôles n’affichent pas plusieurs lignes, afin qu’ils ne prennent pas en charge la *ClientIDRowSuffix* propriété.

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a>Sélection de ligne persistante dans les contrôles de données

Le *GridView* et *ListView* contrôles peuvent aux utilisateurs de sélectionner une ligne. Dans les versions précédentes d’ASP.NET, la sélection a été basée sur l’index de ligne dans la page. Par exemple, si vous sélectionnez le troisième élément sur la page 1 et puis passer à la page 2, le troisième élément dans cette page est sélectionné.

Persistante sélection a été initialement prise en charge uniquement dans les projets de données dynamiques dans le .NET Framework 3.5 SP1. Lorsque cette fonctionnalité est activée, l’élément actuellement sélectionné est basé sur la clé de données pour l’élément. Cela signifie que si vous sélectionnez la troisième ligne sur la page 1 et que vous passez à la page 2, rien n’est sélectionné sur la page 2. Lorsque vous déplacez vers la page 1, la troisième ligne est toujours sélectionnée. Sélection rendue persistante est maintenant prise en charge pour le *GridView* et *ListView* contrôles dans tous les projets à l’aide de la *EnablePersistedSelection* propriété, comme indiqué dans le exemple suivant :

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a>Contrôle de graphique ASP.NET

ASP.NET *graphique* contrôle développe les offres de visualisation des données dans le .NET Framework. À l’aide de la *graphique* contrôle, vous pouvez créer des pages ASP.NET qui ont des graphiques intuitifs et visuellement attrayantes pour des analyses statistiques ou financières complexes. ASP.NET *graphique* contrôle a été présenté comme un module complémentaire à la version finale de .NET Framework version 3.5 SP1 et fait partie de la version de .NET Framework 4.

Le contrôle comprend les fonctionnalités suivantes :

- 35 types de graphiques distincts.
- Un nombre illimité de zones de graphique, titres, légendes et annotations.
- Une grande variété de paramètres d’apparence de tous les éléments de graphique.
- Prise en charge 3D pour la plupart des types de graphiques.
- Étiquettes de données intelligentes qui peuvent s’ajuster automatiquement autour des points de données.
- Franges séparations d’échelle et la mise à l’échelle logarithmique.
- Plus de 50 formules financières et statistiques pour l'analyse et la transformation de données.
- Liaison simple et la manipulation des données du graphique.
- Prise en charge des formats de données courantes telles que les dates, heures et devises.
- Prise en charge pour l’interactivité et événementiel de personnalisation, notamment le client, cliquez sur les événements à l’aide d’Ajax.
- Gestion d'état.
- Flux binaires.

Les figures suivantes montrent des exemples de graphiques financiers qui sont produites par le contrôle Chart de ASP.NET.

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

Figure 2 : Exemples de contrôle de graphique ASP.NET

Pour plus d’exemples montrant comment utiliser le contrôle ASP.NET Chart, téléchargez l’exemple de code sur le [exemples d’environnement pour Microsoft Chart Controls](https://go.microsoft.com/fwlink/?LinkId=128300) page sur le site Web MSDN. Vous trouverez davantage d’exemples de communauté contenu à la [Forum de contrôle de graphique](https://go.microsoft.com/fwlink/?LinkId=128713).

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a>Ajout du contrôle de graphique à une Page ASP.NET

L’exemple suivant montre comment ajouter un *graphique* contrôle à une page ASP.NET à l’aide de balisage. Dans l’exemple, le *graphique* contrôle génère un graphique à colonnes pour les points de données statiques.

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a>À l’aide de graphiques 3D

Le *graphique* contrôle contient un *ChartAreas* collection, qui peut contenir *ChartArea* objets qui définissent les caractéristiques des zones de graphique. Par exemple, pour utiliser 3D pour une zone de graphique, utilisez le *Area3DStyle* propriété comme dans l’exemple suivant :

[!code-aspx[Main](overview/samples/sample59.aspx)]

La figure ci-dessous montre un graphique 3D avec quatre séries de la *barre* type de graphique.

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

Figure 3 : graphique à barres 3D

#### <a name="using-scale-breaks-and-logarithmic-scales"></a>À l’aide de séparateurs d’échelle et échelles logarithmiques

Séparations d’échelle et échelles logarithmiques sont deux autres méthodes permettent d’ajouter la sophistication au graphique. Ces fonctionnalités sont spécifiques à chaque axe dans une zone de graphique. Par exemple, pour utiliser ces fonctionnalités sur l’axe Y principal d’une zone de graphique, utilisez le *AxisY.IsLogarithmic* et *ScaleBreakStyle* propriétés dans un *ChartArea* objet. L’extrait de code suivant montre comment utiliser les séparations d’échelle sur l’axe Y principal.

[!code-aspx[Main](overview/samples/sample60.aspx)]

La figure ci-dessous montre l’axe des Y avec séparations d’échelle.

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

Figure 4 : Séparations d’échelle

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a>Filtrage des données avec le contrôle QueryExtender

Une tâche très courante pour les développeurs qui créent des pages Web pilotées par des données consiste à filtrer les données. Cela s’effectue traditionnellement en générant *où* contrôles de source de clauses dans les données. Cette approche peut être compliquée et dans certains cas le *où* syntaxe ne vous permet pas de tirer parti de toutes les fonctionnalités de la base de données sous-jacente.

Pour simplifier le filtrage, un nouveau *QueryExtender* contrôle a été ajouté dans ASP.NET 4. Ce contrôle peut être ajouté à *EntityDataSource* ou *LinqDataSource* contrôles afin de filtrer les données retournées par ces contrôles. Étant donné que le *QueryExtender* contrôle s’appuie sur LINQ, le filtre est appliqué sur le serveur de base de données avant que les données sont envoyées à la page, ce qui entraîne des opérations très efficaces.

Le *QueryExtender* contrôle prend en charge une variété d’options de filtre. Les sections suivantes décrivent ces options et fournissent des exemples montrant comment les utiliser.

#### <a name="search"></a>Rechercher

Pour l’option de recherche, le *QueryExtender* contrôle effectue une recherche dans les champs spécifiés. Dans l’exemple suivant, le contrôle utilise le texte entré dans le contrôle de TextBoxSearch et recherche de leur contenu dans le `ProductName` et `Supplier.CompanyName` colonnes de données qui sont retournées à partir de la *LinqDataSource* contrôle.

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a>Plage

L’option de la plage est similaire à l’option de recherche, mais spécifie une paire de valeurs pour définir la plage. Dans l’exemple suivant, le *QueryExtender* contrôler des recherches dans le `UnitPrice` colonne dans les données retournées à partir de la *LinqDataSource* contrôle. La plage est lue depuis les contrôles TextBoxFrom et TextBoxTo sur la page.

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a>PropertyExpression

L’option d’expression de propriété vous permet de définir une comparaison à une valeur de propriété. Si l’expression prend la valeur *true*, les données en cours d’examen sont retournées. Dans l’exemple suivant, le *QueryExtender* contrôle filtre les données en comparant les données dans le `Discontinued` colonne à la valeur à partir du contrôle CheckBoxDiscontinued sur la page.

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a>CustomExpression

Enfin, vous pouvez spécifier une expression personnalisée à utiliser avec le *QueryExtender* contrôle. Cette option vous permet d’appeler une fonction dans la page qui définit la logique de filtre personnalisé. L’exemple suivant montre comment spécifier de façon déclarative une expression personnalisée dans le *QueryExtender* contrôle.

[!code-aspx[Main](overview/samples/sample64.aspx)]

L’exemple suivant montre la fonction personnalisée qui est appelée par le *QueryExtender* contrôle. Dans ce cas, plutôt qu’une requête de base de données qui inclut un *où* clause, le code utilise une requête LINQ pour filtrer les données.

[!code-csharp[Main](overview/samples/sample65.cs)]

Ces exemples ne montrent qu’une seule expression utilisée dans le *QueryExtender* contrôle à la fois. Toutefois, vous pouvez inclure plusieurs expressions à l’intérieur de la *QueryExtender* contrôle.

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a>Expressions de Code de codé au format HTML

Certains sites ASP.NET (surtout avec ASP.NET MVC) reposent fortement sur l’utilisation `<%` =  `expression %>` syntaxe (souvent appelé « pépites de code ») pour écrire du texte dans la réponse. Lorsque vous utilisez des expressions de code, il est facile d’oublier de coder en HTML le texte si le texte est fourni à partir de l’utilisateur d’entrée, il peut exposer les pages à une attaque XSS (Cross Site Scripting).

ASP.NET 4 présente la nouvelle syntaxe pour les expressions de code suivante :

[!code-aspx[Main](overview/samples/sample66.aspx)]

Cette syntaxe utilise le codage HTML par défaut lors de l’écriture dans la réponse. Cette nouvelle expression efficacement se traduit par les éléments suivants :

[!code-aspx[Main](overview/samples/sample67.aspx)]

Par exemple, &lt;% : % de la demande [« UserInput »]&gt; effectue un encodage HTML sur la valeur de *demande [« UserInput »]*.

L’objectif de cette fonctionnalité consiste à rendre possible de remplacer toutes les instances de l’ancienne syntaxe avec la nouvelle syntaxe afin que vous n’êtes pas obligé de décider à chaque étape celui que vous souhaitez utiliser. Toutefois, il existe certains cas dans lequel le texte en cours de sortie est destiné à être HTML ou est déjà codé, auquel cas cela peut conduire à double encodage.

Dans ces cas, ASP.NET 4 introduit une nouvelle interface, *IHtmlString*, ainsi que d’une implémentation concrète, *HtmlString*. Instances de ces types vous permettent d’indiquer que la valeur de retour est déjà correctement encodée (ou sinon examinée) pour l’affichage au format HTML, et que par conséquent, la valeur de ne doit pas être encodée en HTML à nouveau. Par exemple, ce qui suit ne doit pas être (et n’est pas) encodée en HTML :

[!code-aspx[Main](overview/samples/sample68.aspx)]

Des méthodes d’assistance ASP.NET MVC 2 ont été mis à jour pour fonctionner avec cette nouvelle syntaxe afin qu’ils ne soient pas double encodé, mais uniquement lorsque vous exécutez ASP.NET 4. Cette nouvelle syntaxe ne fonctionne pas lorsque vous exécutez une application à l’aide d’ASP.NET 3.5 SP1.

N’oubliez pas que cela ne garantit pas la protection contre les attaques XSS. Par exemple, HTML qui utilise des valeurs d’attribut qui ne sont pas entre guillemets peut contenir des entrées d’utilisateur qui sont toujours susceptible de. Notez que la sortie des contrôles ASP.NET et les applications auxiliaires ASP.NET MVC inclut toujours les valeurs d’attribut entre guillemets, qui est l’approche recommandée.

De même, cette syntaxe n’effectue pas de codage JavaScript, comme lorsque vous créez une chaîne JavaScript basée sur l’entrée d’utilisateur.

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a>Modifications de modèle de projet

Dans les versions antérieures d’ASP.NET, lorsque vous utilisez Visual Studio pour créer un nouveau projet de Site Web ou d’un projet d’Application Web, les projets qui en résulte contient uniquement une page Default.aspx, une valeur par défaut `Web.config` fichier et le `App_Data` dossier, comme indiqué dans le code suivant illustration :

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

Visual Studio prend également en charge un type de projet de Site Web vide, qui ne contient aucun fichier du tout, comme indiqué dans la figure suivante :

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

Le résultat est que pour les débutants, il existe très peu de conseils sur la création d’une application Web de production. Par conséquent, ASP.NET 4 introduit trois nouveaux modèles, un pour un projet d’application Web vide et un pour un projet d’Application Web et le Site Web.

#### <a name="empty-web-application-template"></a>Modèle d’Application Web vide

Comme son nom l’indique, le modèle d’Application Web vide est un projet d’Application Web simplifiée. Vous sélectionnez ce modèle de projet à partir de la boîte de dialogue Nouveau projet Visual Studio, comme illustré dans la figure suivante :

[![](overview/_static/image7.png)](overview/_static/image6.png)

([Cliquez pour afficher l’image en taille réelle](overview/_static/image8.png))

Lorsque vous créez une Application de Web ASP.NET vide, Visual Studio crée la disposition du dossier suivant :

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

Cela revient à la disposition du Site Web vide à partir de versions antérieures d’ASP.NET, à une exception près. Dans Visual Studio 2010, projets d’Application Web vide et le Site Web vide contiennent les éléments suivants minimale `Web.config` fichier qui contient des informations utilisées par Visual Studio pour identifier le framework ciblée par le projet :

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

Sans cela *targetFramework* propriété, les valeurs par défaut de Visual Studio au ciblage de .NET Framework 2.0 afin de préserver la compatibilité lors de l’ouverture des applications plus anciennes.

#### <a name="web-application-and-web-site-project-templates"></a>Application Web et les modèles de projet de Site Web

Les autres deux nouveaux modèles de projet qui sont livrés avec Visual Studio 2010 contiennent des modifications majeures. La figure suivante montre la disposition du projet qui est créée lorsque vous créez un nouveau projet d’Application Web. (La mise en page pour un projet de Site Web est pratiquement identique).

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

Le projet inclut un nombre de fichiers qui n’ont pas été créés dans les versions antérieures. En outre, le nouveau projet d’Application Web est configuré avec la fonctionnalité d’abonnement de base, qui vous permet de commencer rapidement dans la sécurisation de l’accès à la nouvelle application. En raison de cette inscription, le `Web.config` de fichier pour le nouveau projet comprend des entrées qui sont utilisées pour configurer l’appartenance, les rôles et les profils. L’exemple suivant montre le `Web.config` fichier pour un nouveau projet d’Application Web. (Dans ce cas, *roleManager* est désactivé.)

[![](overview/_static/image13.png)](overview/_static/image12.png)

([Cliquez pour afficher l’image en taille réelle](overview/_static/image14.png))

Le projet contient également un deuxième `Web.config` de fichiers dans le `Account` directory. Le deuxième fichier de configuration fournit un moyen pour sécuriser l’accès à la page ChangePassword.aspx de non-les utilisateurs connectés. L’exemple suivant montre le contenu du deuxième `Web.config` fichier.

![](overview/_static/image15.png)

Les pages créées par défaut dans les nouveaux modèles de projet contiennent également plus de contenu que dans les versions précédentes. Le projet contient une page maître par défaut et le fichier CSS, et la page par défaut (Default.aspx) est configurée pour utiliser la page maître par défaut. Le résultat est que lorsque vous exécutez l’application Web ou un site Web pour la première fois, la page (accueil) par défaut est déjà fonctionnelle. En fait, il est similaire à la page par défaut que vous voyez lorsque vous démarrez une application MVC.

[![](overview/_static/image17.png)](overview/_static/image16.png)

([Cliquez pour afficher l’image en taille réelle](overview/_static/image18.png))

L’objectif de ces changements pour les modèles de projet est de fournir des conseils sur la façon de commencer à créer une nouvelle application Web. Avec sémantiquement correct strict XHTML 1.0 conformes balisage et mise en page qui est spécifié à l’aide de CSS, les pages dans les modèles représentent les meilleures pratiques pour créer des applications Web ASP.NET 4. Les pages par défaut ont également une disposition de deux colonnes que vous pouvez facilement personnaliser.

Par exemple, imaginez que pour une nouvelle Application Web que vous souhaitiez modifier certains des couleurs et d’insérer le logo de votre société à la place le logo de mon Application ASP.NET. Pour ce faire, vous créez un répertoire sous `Content` pour stocker votre image de logo :

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

Pour ajouter l’image à la page, vous ouvrez le `Site.Master` de fichier, trouver où le texte de mon Application ASP.NET est définie et remplacez-la par une *image* élément dont *src* attribut est défini sur le nouveau logo image, comme dans l’exemple suivant :

[![](overview/_static/image21.png)](overview/_static/image20.png)

([Cliquez pour afficher l’image en taille réelle](overview/_static/image22.png))

Vous pouvez ensuite aller dans le fichier Site.css et modifier des définitions de classe CSS pour modifier la couleur d’arrière-plan de la page, ainsi que celle de l’en-tête.

Le résultat de ces modifications est que vous pouvez afficher une page d’accueil personnalisée avec très peu d’efforts :

[![](overview/_static/image24.png)](overview/_static/image23.png)

([Cliquez pour afficher l’image en taille réelle](overview/_static/image25.png))

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a>Améliorations de CSS

Parmi les principales zones de travail dans ASP.NET 4 a été pour aider à effectuer le rendu HTML qui est compatible avec les dernières normes HTML. Cela inclut les modifications apportées à la façon dont les contrôles serveur ASP.NET Web utilisent des styles CSS.

#### <a name="compatibility-setting-for-rendering"></a>Paramètre de compatibilité pour le rendu

Par défaut, lorsqu’une application Web ou le site Web cible le .NET Framework 4, le *controlRenderingCompatibilityVersion* attribut de la *pages* élément est défini sur « 4.0 ». Cet élément est défini dans le niveau de l’ordinateur `Web.config` de fichiers et par défaut s’applique à toutes les applications ASP.NET 4 :

[!code-xml[Main](overview/samples/sample69.xml)]

La valeur de *controlRenderingCompatibility* est une chaîne, ce qui permet de nouvelles définitions de version potentiels dans les versions futures. Dans la version actuelle, les valeurs suivantes sont prises en charge pour cette propriété :

- "3.5". Ce paramètre indique le balisage et le rendu hérité. Balisage restitué par les contrôles est rétrocompatible de 100 % et le paramètre de la *xhtmlConformance* propriété est honorée.
- "4.0". Si la propriété possède ce paramètre, les contrôles serveur ASP.NET Web les opérations suivantes :
- Le *xhtmlConformance* propriété est toujours traitée comme « Strict ». Par conséquent, les contrôles restituent un balisage XHTML 1.0 Strict.
- La désactivation des contrôles non input n’est plus effectue le rendu des styles non valides.
- *div* éléments autour des champs masqués sont maintenant filtrables afin qu’ils n’interfèrent pas avec des règles CSS créés par l’utilisateur.
- Les contrôles menu restituent un balisage qui est sémantiquement correct et conformes aux règles d’accessibilité.
- Contrôles de validation ne sont pas rendent styles intralignes.
- Contrôles précédemment rendu border = « 0 » (contrôles qui dérivent de l’ASP.NET *Table* contrôle et ASP.NET *Image* contrôle) n’affichent plus cet attribut.

#### <a name="disabling-controls"></a>La désactivation des contrôles

Dans ASP.NET 3.5 SP1 et versions antérieures, l’infrastructure affiche le *désactivé* d’attribut dans le balisage HTML pour tout contrôle dont *activé* propriété définie sur *false*. Toutefois, conformément à la spécification HTML 4.01, uniquement *d’entrée* éléments doivent avoir cet attribut.

Dans ASP.NET 4, vous pouvez définir le *controlRenderingCompatabilityVersion* propriété sur « 3.5 », comme dans l’exemple suivant :

[!code-xml[Main](overview/samples/sample70.xml)]

Vous pouvez créer un balisage pour un *étiquette* contrôle similaire à celui-ci, ce qui désactive le contrôle :

[!code-aspx[Main](overview/samples/sample71.aspx)]

Le *étiquette* contrôle affichant le code HTML suivant :

[!code-html[Main](overview/samples/sample72.html)]

Dans ASP.NET 4, vous pouvez définir le *controlRenderingCompatabilityVersion* sur « 4.0 ». Dans ce cas, seuls les contrôles qui sont restituées *d’entrée* éléments affichera un *désactivé* attribut lorsque le contrôle *activé* propriété est définie sur *false* . Les contrôles qui ne sont pas rendent HTML *d’entrée* éléments sont rendus au lieu de cela un *classe* attribut qui fait référence à une classe CSS que vous pouvez utiliser pour définir un aspect désactivé pour le contrôle. Par exemple, le *étiquette* contrôle indiqué dans l’exemple précédent génère le balisage suivant :

[!code-html[Main](overview/samples/sample73.html)]

La valeur par défaut pour la classe spécifiée pour ce contrôle est « aspNetDisabled ». Toutefois, vous pouvez modifier cette valeur par défaut en définissant la méthode statique *DisabledCssClass* propriété statique de la *WebControl* classe. Pour les développeurs de contrôles, le comportement à utiliser pour un contrôle spécifique peut également être défini à l’aide de la *SupportsDisabledAttribute* propriété.

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a>Masquage div éléments autour des champs masqués

ASP.NET 2.0 et versions ultérieures restituent des champs masqués spécifiques du système (telles que la *masqué* élément utilisé pour stocker les informations d’état de vue) à l’intérieur de *div* élément afin de se conformer à la norme XHTML. Toutefois, cela peut poser un problème lorsqu’une règle CSS affecte *div* éléments sur une page. Par exemple, cela peut entraîner une ligne d’un pixel qui apparaissent dans la page environ masqué *div* éléments. Dans ASP.NET 4, *div* éléments qui entourent les champs masqués générés par ASP.NET ajoutent une référence de classe CSS, comme dans l’exemple suivant :

[!code-html[Main](overview/samples/sample74.html)]

Vous pouvez ensuite définir une classe CSS qui s’applique uniquement à la *masqué* les éléments qui sont générés par ASP.NET, comme dans l’exemple suivant :

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a>Rendu d’une Table externe pour les contrôles basés sur des modèles

Par défaut, les contrôles serveur Web ASP.NET suivants qui prennent en charge les modèles sont automatiquement encapsulées dans une table externe qui est utilisée pour appliquer des styles inline :

- *FormView*
- *Connexion*
- *PasswordRecovery*
- *ChangePassword*
- *Assistant*
- *CreateUserWizard*

Une nouvelle propriété nommée *RenderOuterTable* a été ajouté à ces contrôles qui permet à la table externe à supprimer à partir du balisage. Par exemple, considérez l’exemple suivant d’un *FormView* contrôle :

[!code-aspx[Main](overview/samples/sample76.aspx)]

Ce balisage restitue la sortie suivante à la page, ce qui inclut un tableau HTML :

[!code-html[Main](overview/samples/sample77.html)]

Pour empêcher le rendu de la table, vous pouvez définir le *FormView* du contrôle *RenderOuterTable* propriété, comme dans l’exemple suivant :

[!code-aspx[Main](overview/samples/sample78.aspx)]

L’exemple précédent restitue la sortie suivante, sans le *table*, *tr*, et *td* éléments :

> Contenu


Cette amélioration peut faciliter le contenu du contrôle avec CSS, de style, car aucune balise inattendus n’est affichés par le contrôle.

> [!NOTE]
> Notez que cette modification désactive la prise en charge de la fonction de mise en forme automatique dans le concepteur Visual Studio 2010, car il n’est plus un *table* élément qui peut héberger des attributs de style qui sont générés par l’option de mise en forme automatique.


<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a>Améliorations des contrôles de ListView

Le *ListView* contrôle est devenu plus facile à utiliser dans ASP.NET 4. La version antérieure du contrôle obligatoire de spécifier un modèle de disposition contenant un contrôle serveur possédant un ID connu. Le balisage suivant montre un exemple typique d’utilisation le *ListView* contrôle dans ASP.NET 3.5.

[!code-aspx[Main](overview/samples/sample79.aspx)]

Dans ASP.NET 4, le *ListView* ne requiert pas un modèle de disposition. Le balisage indiqué dans l’exemple précédent peut être remplacé par le balisage suivant :

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a>CheckBoxList et RadioButtonList contrôle améliorations

Dans ASP.NET 3.5, vous pouvez spécifier la mise en page pour le *CheckBoxList* et *RadioButtonList* à l’aide de le des deux paramètres suivants :

- *Flux*. Le contrôle restitue *span* éléments à son contenu.
- *Table*. Le contrôle restitue un *table* élément doit contenir son contenu.

L’exemple suivant présente le balisage pour chacun de ces contrôles.

[!code-aspx[Main](overview/samples/sample81.aspx)]

Par défaut, les contrôles de rendent HTML similaire à ce qui suit :

[!code-html[Main](overview/samples/sample82.html)]

Étant donné que ces contrôles contiennent des listes d’éléments, pour le rendu HTML sémantiquement correct, ils doivent être restituées leur contenu à l’aide de liste HTML (*li*) éléments. Cela rend plus facile pour les utilisateurs de lire les pages Web à l’aide de la technologie d’assistance et simplifie les contrôles de style à l’aide de CSS.

Dans ASP.NET 4, le *CheckBoxList* et *RadioButtonList* contrôles prennent en charge les nouvelles valeurs suivantes pour le *RepeatLayout* propriété :

- *OrderedList* : le contenu est restitué sous forme *li* éléments au sein d’un *ol* élément.
- *UnorderedList* : le contenu est restitué sous forme *li* éléments au sein d’un *ul* élément.

L’exemple suivant montre comment utiliser ces nouvelles valeurs.

[!code-aspx[Main](overview/samples/sample83.aspx)]

Le balisage précédent génère le code HTML suivant :

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> Remarque Si vous définissez *RepeatLayout* à *OrderedList* ou *UnorderedList*, le *RepeatDirection* propriété ne peut plus être utilisée et sera lève une exception au moment de l’exécution si la propriété a été définie au sein de votre code ou le balisage. La propriété n’aurait aucune valeur, car la disposition visuelle de ces contrôles est définie à l’aide de CSS à la place.


<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a>Améliorations du contrôle menu

Avant ASP.NET 4, le *Menu* contrôle restitué une série de tableaux HTML. Cela rend plus difficile d’appliquer des styles CSS en dehors de la définition des propriétés de ligne et a été également pas conforme aux normes d’accessibilité.

Dans ASP.NET 4, le contrôle affiche maintenant des éléments HTML en utilisant un balisage sémantique qui se compose d’une liste non triée et les éléments de liste. L’exemple suivant présente le balisage dans une page ASP.NET pour le *Menu* contrôle.

[!code-aspx[Main](overview/samples/sample85.aspx)]

Lorsque la page s’affiche, le contrôle génère le code HTML suivant (le *onclick* code a été omis par souci de clarté) :

[!code-html[Main](overview/samples/sample86.html)]

En plus des améliorations de rendu, la navigation au clavier du menu a été améliorée à l’aide de la gestion du focus. Lorsque le *Menu* contrôle a le focus, vous pouvez utiliser les touches de direction pour parcourir les éléments. Le *Menu* contrôle également maintenant les attache accessible rôles (ARIA) d’applications internet riches et les attributs Vale[wing le](http://www.w3.org/TR/wai-aria-practices/#menu "instructions de Menu ARIA")améliorées accessibilité.

Styles du contrôle de menu sont rendus dans un bloc de style en haut de la page, plutôt qu’en plus des éléments HTML rendus. Si vous souhaitez prendre le contrôle total sur le style pour le contrôle, vous pouvez définir la nouvelle *IncludeStyleBlock* propriété *false*, auquel cas le bloc de style n’est pas émis. Une manière d’utiliser cette propriété consiste à utiliser la fonctionnalité de mise en forme automatique dans le concepteur Visual Studio pour définir l’apparence du menu. Vous pouvez ensuite exécuter la page, ouvrez la page source, puis copiez le bloc de style de rendu dans un fichier CSS externe. Dans Visual Studio, annuler le style et un jeu *IncludeStyleBlock* à *false*. Le résultat est que l’apparence du menu est définie à l’aide de styles dans une feuille de style externe.

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a>Assistant et contrôles de CreateUserWizard

ASP.NET *Assistant* et *CreateUserWizard* contrôles prennent en charge les modèles qui vous permettent de définir le HTML rendu. (*CreateUserWizard* dérive *Assistant*.) L’exemple suivant montre le balisage pour un entièrement basé sur des modèles *CreateUserWizard* contrôle :

[!code-aspx[Main](overview/samples/sample87.aspx)]

Le contrôle rend un HTML similaire à ce qui suit :

[!code-html[Main](overview/samples/sample88.html)]

Dans ASP.NET 3.5 SP1, bien que vous puissiez modifier le contenu du modèle, vous toujours avez un contrôle limité sur la sortie de la *Assistant* contrôle. Dans ASP.NET 4, vous pouvez créer un *LayoutTemplate* modèle et insert *espace réservé* contrôle (à l’aide de noms réservés) pour spécifier comment vous voulez le *contrôle de l’Assistant* à restituer. L’exemple suivant illustre cela :

[!code-aspx[Main](overview/samples/sample89.aspx)]

L’exemple contient les éléments suivants nommés des espaces réservés dans le *LayoutTemplate* élément :

- *headerPlaceholder* – en cours d’exécution, il est remplacé par le contenu de la *HeaderTemplate* élément.
- *sideBarPlaceholder* – en cours d’exécution, il est remplacé par le contenu de la *SideBarTemplate* élément.
- *wizardStepPlaceHolder* – en cours d’exécution, il est remplacé par le contenu de la *WizardStepTemplate* élément.
- *navigationPlaceholder* – en cours d’exécution, qui est remplacée par tous les modèles de navigation que vous avez définies.

Le balisage dans l’exemple qui utilise des espaces réservés restitue le code HTML suivant (sans contenu réellement défini dans les modèles) :

[!code-html[Main](overview/samples/sample90.html)]

Le code HTML uniquement qui est maintenant pas défini par l’utilisateur est un *span* élément. (Nous estimons que dans les futures versions, même si le *span* élément ne sera pas restitué.) Vous disposez à présent un contrôle total sur pratiquement tout le contenu qui est généré par le *Assistant* contrôle.

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a>ASP.NET MVC

ASP.NET MVC a été introduit comme une infrastructure de module complémentaire à ASP.NET 3.5 SP1 en mars 2009. Visual Studio 2010 inclut ASP.NET MVC 2, qui inclut les nouvelles fonctionnalités et capacités.

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a>Prise en charge des zones

Zones vous permettent de contrôleurs de groupe et des vues dans les sections d’une grande application dans un isolement relatif à partir d’autres sections. Chaque zone peut être implémenté comme un projet ASP.NET MVC distinct qui peut ensuite être référencé par l’application principale. Cela permet de gérer la complexité lorsque vous générez une application volumineuse et plus facilement plusieurs équipes de travailler ensemble sur une seule application.

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a>Prise en charge de Validation d’attribut Annotation de données

*DataAnnotations* attributs vous permettent de joindre la logique de validation à un modèle à l’aide des attributs de métadonnées. *DataAnnotations* attributs ont été introduits dans Dynamic Data ASP.NET dans ASP.NET 3.5 SP1. Ces attributs ont été intégrés dans le classeur de modèles par défaut et fournissent un moyen piloté par les métadonnées pour valider l’entrée utilisateur.

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a>Programmes d’assistance basés sur des modèles

Programmes d’assistance basés sur des modèles vous permettent de modifier automatiquement associer et affichent les modèles avec des types de données. Par exemple, vous pouvez utiliser une application d’assistance de modèle pour spécifier qu’un élément d’interface utilisateur de sélecteur de dates est affiché automatiquement pour un *System.DateTime* valeur. Cela est similaire aux modèles de champ dans Dynamic Data ASP.NET.

Le *Html.EditorFor* et *Html.DisplayFor* méthodes d’assistance ont prise en charge intégrée pour les types de données standard de rendu des objets ainsi que complexes avec plusieurs propriétés. Ils également personnaliser le rendu en vous permettant d’appliquer des attributs d’annotation de données comme *DisplayName* et *ScaffoldColumn* à la *ViewModel* objet.

Fréquence à laquelle vous souhaitez personnaliser la sortie à partir de programmes d’assistance de l’interface utilisateur encore plus loin et contrôlez entièrement ce qui est généré. Le *Html.EditorFor* et *Html.DisplayFor* méthodes d’assistance prennent en charge cela à l’aide d’un mécanisme de création de modèles qui vous permet de définir des modèles externes peuvent substituer et contrôle la sortie rendue. Les modèles peuvent être rendus individuellement pour une classe.

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a>Dynamic Data

Dynamic Data a été introduite dans la version de .NET Framework 3.5 SP1 au milieu de l’année 2008. Cette fonctionnalité offre de nombreuses améliorations pour la création d’applications pilotées par les données, y compris les éléments suivants :

- Une expérience de RAD permettant de créer rapidement un site Web orientées données.
- Validation automatique est basée sur les contraintes définies dans le modèle de données.
- La possibilité de modifier facilement le balisage généré pour les champs dans le *GridView* et *DetailsView* contrôles à l’aide des modèles de champ qui font partie de votre projet Dynamic Data.

> [!NOTE]
> Remarque Pour plus d’informations, consultez le [documentation Dynamic Data](https://msdn.microsoft.com/library/cc488545.aspx) dans MSDN Library.


Pour ASP.NET 4, Dynamic Data a été amélioré pour offrir aux développeurs encore plus de puissance pour créer rapidement des sites Web orientés données.

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a>L’activation des données dynamiques pour les projets existants

Les fonctionnalités de données dynamiques fournie dans le .NET Framework 3.5 SP1 mis des nouvelles fonctionnalités telles que les éléments suivants :

- Modèles de champ – ceux-ci fournissent des modèles basée sur le type de données pour des contrôles liés aux données. Modèles de champ fournissent un moyen plus simple de personnaliser l’apparence des contrôles de données que l’utilisation de champs de modèle pour chaque champ.
- Validation – Dynamic Data vous permet d’utiliser des attributs sur les classes de données pour la validation personnalisée et la validation pour les scénarios courants tels que les champs obligatoires, vérification de plage, la vérification de type, à l’aide d’expressions régulières de critères spéciaux. La validation est appliquée par les contrôles de données.

Toutefois, ces fonctionnalités présentant les exigences suivantes :

- La couche d’accès aux données devait être basé sur Entity Framework ou LINQ to SQL.
- La seules source de données contrôles pris en charge pour ces fonctionnalités ont été le *EntityDataSource* ou *LinqDataSource* contrôles.
- Les fonctionnalités nécessaires à un projet Web qui a été créé à l’aide de la dynamique des données ou des modèles de Dynamic Data Entities afin de disposer de tous les fichiers qui ont été nécessaires pour prendre en charge la fonctionnalité.

L’objectif principal de la prise en charge des données dynamiques dans ASP.NET 4 consiste à activer la nouvelle fonctionnalité de données dynamiques pour n’importe quelle application ASP.NET. L’exemple suivant présente le balisage pour les contrôles qui peuvent profiter des fonctionnalités de données dynamiques dans une page existante.

[!code-aspx[Main](overview/samples/sample91.aspx)]

Dans le code de la page, le code suivant doit être ajouté pour permettre la prise en charge des données dynamiques pour ces contrôles :

[!code-csharp[Main](overview/samples/sample92.cs)]

Lorsque le *GridView* contrôle est en mode édition, Dynamic Data automatiquement valide le fait que les données entrées sont au format approprié. Si elle n’est pas le cas, un message d’erreur s’affiche.

Cette fonctionnalité fournit également d’autres avantages, comme la possibilité de spécifier des valeurs pour le mode insertion. Sans les données dynamiques, pour implémenter une valeur par défaut pour un champ, vous devez attacher à un événement, localisez le contrôle (à l’aide de *FindControl*) et définissez sa valeur. Dans ASP.NET 4, le *EnableDynamicData* appel prend en charge un deuxième paramètre qui vous permet de passer des valeurs par défaut pour n’importe quel champ sur l’objet, comme illustré dans cet exemple :

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a>Syntaxe déclarative contrôle DynamicDataManager

Le *DynamicDataManager* contrôle a été amélioré afin que vous puissiez le configurer de façon déclarative, comme avec la plupart des contrôles dans ASP.NET, au lieu d’uniquement dans le code. Le balisage pour le *DynamicDataManager* contrôle ressemble à l’exemple suivant :

[!code-aspx[Main](overview/samples/sample94.aspx)]

Cette balise active le comportement Dynamic Data pour le contrôle GridView1 qui est référencé dans le *DataControls* section de la *DynamicDataManager* contrôle.

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a>Modèles d’entité

Modèles d’entité offrent une nouvelle façon de personnaliser la disposition des données sans avoir à créer une page personnalisée. Modèles de page utilisent le *FormView* contrôle (au lieu du *DetailsView* contrôler, tel qu’utilisé dans les modèles de pages dans les versions antérieures des données dynamiques) et le *DynamicEntity* contrôle à rendre les modèles d’entité. Cela vous donne davantage de contrôle sur le balisage qui est restitué par Dynamic Data.

La liste suivante présente la nouvelle disposition de répertoire de projet qui contient les modèles d’entité :

[!code-console[Main](overview/samples/sample95.cmd)]

Le `EntityTemplate` répertoire contient des modèles pour savoir comment afficher les objets de modèle de données. Par défaut, les objets sont affichés à l’aide de la `Default.ascx` modèle, qui fournit le balisage qui ressemble à la balise créée par le *DetailsView* contrôle utilisé par Dynamic Data ASP.NET 3.5 SP1. L’exemple suivant montre le balisage pour le `Default.ascx` contrôle :

[!code-aspx[Main](overview/samples/sample96.aspx)]

Les modèles par défaut peuvent être modifiés pour changer l’apparence pour l’ensemble du site. Il existe des modèles pour l’affichage, de modifier et d’opérations d’insertion. Nouveaux modèles peuvent être ajoutés en fonction du nom de l’objet de données afin de modifier l’apparence d’un seul type d’objet. Par exemple, vous pouvez ajouter le modèle suivant :

[!code-console[Main](overview/samples/sample97.cmd)]

Le modèle peut contenir le balisage suivant :

[!code-aspx[Main](overview/samples/sample98.aspx)]

Les nouveaux modèles d’entité sont affichés sur une page à l’aide de la nouvelle *DynamicEntity* contrôle. Au moment de l’exécution, ce contrôle est remplacé par le contenu du modèle d’entité. L’exemple de balisage suivant le *FormView* dans contrôler le `Detail.aspx` modèle de page qui utilise le modèle d’entité. Notez que le *DynamicEntity* élément dans le balisage.

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-email-addresses"></a>Nouveaux modèles de champ d’URL et adresses de messagerie

ASP.NET 4 présente deux nouveaux modèles de champ prédéfini, `EmailAddress.ascx` et `Url.ascx`. Ces modèles sont utilisés pour les champs qui sont marqués comme *EmailAddress* ou *Url* avec la *DataType* attribut. Pour *EmailAddress* objets, le champ est affiché comme un lien hypertexte qui est créé à l’aide de la *mailto :* protocole. Lorsque les utilisateurs cliquent sur le lien, il ouvre le client de messagerie de l’utilisateur et crée un message squelette. Objets de type *Url* sont affichés sous forme de liens hypertexte ordinaires.

L’exemple suivant montre comment les champs sont marquées.

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a>Création de liens avec le contrôle DynamicHyperLink

Dynamic Data utilise la nouvelle fonctionnalité de routage a été ajoutée dans le .NET Framework 3.5 SP1 pour contrôler les URL que les utilisateurs voient lorsqu’ils accèdent au site Web. La nouvelle *DynamicHyperLink* contrôle facilite la création de liens vers les pages d’un site Dynamic Data. L’exemple suivant montre comment utiliser le *DynamicHyperLink* contrôle :

[!code-aspx[Main](overview/samples/sample101.aspx)]

Ce balisage crée un lien qui pointe vers la page de liste pour le `Products` table basée sur les itinéraires définis dans le `Global.asax` fichier. Le contrôle utilise automatiquement le nom de table par défaut basé sur la page de données dynamiques.

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a>Prise en charge de l’héritage dans le modèle de données

Entity Framework et LINQ to SQL prend en charge l’héritage dans leurs modèles de données. Un exemple de ce peut être une base de données qui a un `InsurancePolicy` table. Il peut également contenir des `CarPolicy` et `HousePolicy` tables qui ont les mêmes champs que `InsurancePolicy` , puis ajoutez davantage de champs. Dynamic Data a été modifié pour comprendre les objets hérités dans le modèle de données et pour prendre en charge la génération de modèles automatique pour les tables héritées.

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a>Prise en charge pour les relations plusieurs-à-plusieurs (Entity Framework uniquement)

Entity Framework est prise en charge des relations plusieurs-à-plusieurs entre des tables, qui est implémentée en exposant la relation sous forme de collection sur un *entité* objet. Nouvelle `ManyToMany.ascx` et `ManyToMany_Edit.ascx` les modèles de champ ont été ajoutés pour prendre en charge pour l’affichage et modification des données qui sont impliquées dans les relations plusieurs-à-plusieurs.

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a>Nouveaux attributs pour contrôler l’affichage et la prise en charge des énumérations

Le *DisplayAttribute* a été ajouté pour vous donner un contrôle supplémentaire sur la façon dont les champs sont affichés. Le *DisplayName* attribut dans les versions antérieures de Dynamic Data vous permettait de modifier le nom qui est utilisé en tant que légende pour un champ. La nouvelle *DisplayAttribute* classe vous permet de spécifier davantage d’options pour l’affichage d’un champ, tels que l’ordre dans lequel un champ s’affiche et indique si un champ sera utilisé comme filtre. L’attribut offre également un contrôle indépendant du nom utilisé pour les étiquettes dans un *GridView* contrôler, le nom utilisé dans un *DetailsView* contrôlent, le texte d’aide pour le champ, et le filigrane utilisés pour le champ (si le champ accepte la saisie de texte).

Le *EnumDataTypeAttribute* classe a été ajoutée pour vous permettre de mapper des champs aux énumérations. Lorsque vous appliquez cet attribut à un champ, vous spécifiez un type d’énumération. Dynamic Data utilise le nouveau `Enumeration.ascx` modèle de champ pour créer l’interface utilisateur pour afficher et modifier les valeurs d’énumération. Le modèle mappe les valeurs de la base de données pour les noms dans l’énumération.

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a>Prise en charge améliorée pour les filtres

Dynamic Data 1.0 livrés avec les filtres intégrés pour les colonnes booléennes et les colonnes de clé étrangère. Les filtres, il n’a pas autorisé à spécifier si elles étaient affichées ou ordre dans lequel ils ont été affichés. La nouvelle *DisplayAttribute* attribut répondent à ces deux de ces problèmes en vous offrant de contrôler si une colonne s’affiche en tant que filtre et dans quel ordre il sera affiché.

Une amélioration supplémentaire est que le filtrage de prise en charge a été[écrites pour utiliser le nouveau](#0.2__QueryExtender "_QueryExtender") fonctionnalité de Web Forms. Cela vous permet de créer des filtres sans nécessiter de connaissances du contrôle de source de données à utiliser avec les filtres. Avec ces extensions, les filtres ont également été transformés en contrôles de modèle, qui vous permet d’ajouter d’autres. Enfin, le *DisplayAttribute* classe mentionné plus haut permet le filtre par défaut d’être substitué, de la même façon que *UIHint* permet au modèle de champ par défaut pour une colonne d’être substitué.

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a>Améliorations du développement Web Visual Studio 2010

Développement Web dans Visual Studio 2010 a été amélioré pour une compatibilité supérieure avec CSS, une productivité accrue grâce à des extraits de balisage HTML et ASP.NET et nouveau IntelliSense JavaScript dynamique.

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a>Compatibilité CSS améliorée

Le concepteur Visual Web Developer dans Visual Studio 2010 a été mis à jour pour améliorer la conformité aux normes CSS 2.1. Le Concepteur conserve l’intégrité de la source HTML mieux et est plus fiable que dans les versions précédentes de Visual Studio. En coulisses, les améliorations architecturales ont également été apportées qui vous permettent de mieux les améliorations futures de rendu, la disposition et facilité de maintenance.

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a>HTML et des extraits de code JavaScript

Dans l’éditeur HTML, IntelliSense complète automatiquement les noms de balise. La fonctionnalité d’extraits de code IntelliSense complète automatiquement les balises entières et bien plus encore. Dans Visual Studio 2010, les extraits de code IntelliSense sont pris en charge pour JavaScript, ainsi que c# et Visual Basic, qui ont été pris en charge dans les versions antérieures de Visual Studio.

Visual Studio 2010 inclut plus de 200 extraits de code qui vous aident à compléter automatiquement les balises ASP.NET et HTML courantes, notamment les attributs obligatoires (tels que runat = « server ») et les attributs courants spécifiques à une balise (tels que *ID*,  *DataSourceID*, *ControlToValidate*, et *texte*).

Vous pouvez télécharger des extraits de code supplémentaires, ou vous pouvez écrire vos propres extraits de code qui encapsulent les blocs de balisage que vous ou votre équipe utilisez pour les tâches courantes.

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a>Améliorations de JavaScript IntelliSense

Dans Visual 2010, JavaScript IntelliSense a été repensé pour offrir une expérience de modification encore plus riche. IntelliSense reconnaît désormais les objets qui ont été générés dynamiquement par les méthodes telles que *registerNamespace* et par les techniques semblables utilisées par d’autres infrastructures JavaScript. Performances ont été améliorées pour analyser de grandes bibliothèques de scripts et afficher IntelliSense avec peu ou aucun retard de traitement. Compatibilité a été considérablement augmentée pour prendre en charge presque toutes les bibliothèques tierces et pour prendre en charge de divers styles de codage. Commentaires de documentation sont maintenant analysés comme vous tapez et immédiatement exploités par IntelliSense.

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a>Déploiement d’applications Web avec Visual Studio 2010

Lorsque les développeurs ASP.NET déploiement une application Web, il est souvent trouver de qu’ils rencontrent des problèmes tels que les éléments suivants :

- Déploiement sur un site d’hébergement partagé nécessite des technologies telles que FTP, qui peut être lente. En outre, vous devez effectuer manuellement des tâches telles que l’exécution de scripts SQL pour configurer une base de données et vous devez modifier les paramètres IIS, telles que la configuration d’un dossier du répertoire virtuel en tant qu’application.
- Dans un environnement d’entreprise, en plus de déployer les fichiers d’application Web, les administrateurs doivent modifier fréquemment les fichiers de configuration ASP.NET et les paramètres IIS. Les administrateurs de base de données doivent exécuter une série de scripts SQL pour obtenir la base de données de l’application en cours d’exécution. Ces installations sont laborieux, souvent prendre des heures et doit être soigneusement documentées.

Visual Studio 2010 inclut des technologies qui répondent à ces problèmes et qui vous permettent de déployer en toute transparence des applications Web. Un de ces technologies est l’outil de déploiement Web IIS (MsDeploy.exe).

Fonctionnalités de déploiement Web dans Visual Studio 2010 incluent les principaux domaines suivants :

- Empaquetage de Web
- Transformation de Web.config
- Déploiement de base de données
- Publication en un clic pour les applications Web

Les sections suivantes fournissent des informations sur ces fonctionnalités.

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a>Empaquetage de Web

Visual Studio 2010 utilise l’outil MSDeploy pour créer un fichier compressé (.zip) pour votre application, ce qui correspond à un *package Web*. Le fichier de package contient des métadonnées sur votre application ainsi que le contenu suivant :

- Paramètres IIS, ce qui inclut les paramètres de pool d’application, les paramètres de page d’erreur et ainsi de suite.
- Le contenu Web réel, qui inclut des pages Web, les contrôles utilisateur, contenu statique (images et les fichiers HTML) et ainsi de suite.
- Schémas de base de données SQL Server et les données.
- Certificats de sécurité, de composants à installer dans le GAC, paramètres du Registre et ainsi de suite.

Un package Web peut être copié vers n’importe quel serveur et ensuite installé manuellement à l’aide du Gestionnaire des services Internet. Vous pouvez également, pour le déploiement automatisé, le package peut être installé à l’aide en ligne de commande ou à l’aide des API de déploiement.

Visual Studio 2010 fournit des tâches MSBuild et cibles pour créer des packages Web intégrées. Pour plus d’informations, consultez [vue d’ensemble du déploiement de projet d’Application ASP.NET Web](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx) sur le site Web de MSDN et [10 + 20 raisons pourquoi vous devez créer un Package Web](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html) sur le blog de Vishal.

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a>Transformation de Web.config

Pour le déploiement d’application Web, Visual Studio 2010 introduit [transformer XDT (XML Document)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html), qui est une fonctionnalité qui vous permet de transformer un `Web.config` fichier à partir des paramètres de développement pour les paramètres de production. Paramètres de transformation sont spécifiés dans les fichiers de transformation nommées `web.debug.config`, `web.release.config`, et ainsi de suite. (Les noms de ces fichiers correspondent aux configurations de MSBuild.) Un fichier de transformation inclut uniquement les modifications que vous devez apporter à un déployé `Web.config` fichier. Vous spécifiez les modifications apportées à l’aide d’une syntaxe simple.

L’exemple suivant montre une partie d’un `web.release.config` fichier qui peut-être être généré pour le déploiement de votre configuration de mise en production. Le mot clé de remplacement dans l’exemple spécifie que pendant le déploiement du *connectionString* nœud dans le `Web.config` le fichier sera remplacé par les valeurs qui sont répertoriés dans l’exemple.

[!code-xml[Main](overview/samples/sample102.xml)]

Pour plus d’informations, consultez [syntaxe de Transformation de Web.config pour le déploiement de projet d’Application Web](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx) sur le site MSDN <a id="0.2_a"> </a> site Web et[déploiement Web : Transformation Web.Config](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)sur le blog de Vishal.

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a>Déploiement de base de données

Un package de déploiement de Visual Studio 2010 peut inclure des dépendances sur les bases de données SQL Server. Dans le cadre de la définition de package, vous fournissez la chaîne de connexion pour votre base de données source. Lorsque vous créez le package Web, Visual Studio 2010 crée les scripts SQL pour le schéma de base de données et éventuellement pour les données, puis les ajoute au package. Vous pouvez également fournir des scripts SQL personnalisés et spécifier l’ordre dans lequel il doit s’exécuter sur le serveur. Au moment du déploiement, vous fournissez une chaîne de connexion qui est appropriée pour le serveur cible ; le processus de déploiement utilise ensuite cette chaîne de connexion pour exécuter les scripts qui créent le schéma de base de données et ajoutent des données.

En outre, à l’aide d’un clic publier, vous pouvez configurer le déploiement pour publier directement votre base de données lorsque l’application est publiée sur un site d’hébergement partagé distant. Pour plus d’informations, consultez [Comment : déployer une base de données avec un projet d’Application Web](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx) sur le site Web de MSDN et [déploiement de base de données avec Visual Studio 2010](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) sur le blog de Vishal.

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a>Publication en un clic pour les Applications Web

Visual Studio 2010 vous permet également d’utiliser le service de gestion à distance d’IIS pour publier une application Web à un serveur distant. Vous pouvez créer un profil de publication pour votre compte d’hébergement ou pour les serveurs de test ou de serveurs de mise en lots. Chaque profil peut enregistrer des informations d’identification appropriées en toute sécurité. Vous pouvez ensuite déployer sur un de la cible de barre d’outils de publication de serveurs avec un seul clic à l’aide du Web un clic. Avec Visual Studio 2010, vous pouvez également publier à l’aide de la ligne de commande MSBuild. Cela vous permet de configurer votre environnement de build d’équipe afin d’inclure la publication dans un modèle d’intégration continue.

Pour plus d’informations, consultez [Comment : déployer un Web Application Project à l’aide de publication en un clic et Web Deploy](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx) sur le site Web de MSDN et [Web publier 1 clic avec VS 2010](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) sur le blog de Vishal. Pour afficher des présentations vidéo sur le déploiement d’application Web dans Visual Studio 2010, consultez [VS 2010 pour les aperçus pour développeurs Web](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) sur le blog de Vishal.

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a>Ressources

Les sites Web suivants fournissent des informations supplémentaires sur ASP.NET 4 et Visual Studio 2010.

- [ASP.NET 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) — la documentation officielle pour ASP.NET 4 sur le site Web MSDN.
- [https://www.asp.net/](https://www.asp.net/) — ASP.NET site Web de l’équipe.
- [https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx) et [ASP.NET Dynamic Data Content Map](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx) — des ressources en ligne sur le site d’équipe ASP.NET et dans la documentation officielle pour Dynamic Data ASP.NET.
- [https://www.asp.net/ajax/](../../ajax/index.md) : Les ressources Web principal pour le développement d’ASP.NET Ajax.
- [https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) — Le blog de Visual Web Developer Team, qui inclut des informations sur les fonctionnalités dans Visual Studio 2010.
- [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack) — la ressource Web principale des versions préliminaires d’ASP.NET.

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a>Exclusion de responsabilité

Ce document est une version préliminaire et peut être modifié substantiellement avant le lancement de la mise en production commerciale finale du logiciel qu’il décrit.

Les informations contenues dans ce document correspondent à la connaissance que Microsoft Corporation possède des problèmes abordés à la date de la publication. Microsoft devant répondre à des conditions de marché qui évoluent, ce document ne doit pas être considéré comme un engagement de sa part, et Microsoft ne peut pas garantir l’exactitude des informations présentées à la date de la publication.

Ce livre blanc est fourni à titre d'information uniquement. MICROSOFT NE FOURNIT AUCUNE GARANTIE, EXPRESSE, IMPLICITE OU LÉGALE, QUANT AUX INFORMATIONS CONTENUES DANS CE DOCUMENT.

L'utilisateur est tenu d'observer la réglementation relative aux droits d'auteur applicable dans son pays. Aucune partie de ce document ne peut être reproduite, stockée ou introduite dans un système de restitution, ou transmise à quelque fin ou par quelque moyen que ce soit (électronique, mécanique, photocopie, enregistrement ou autre) sans la permission expresse et écrite de Microsoft Corporation.

Microsoft peut détenir des brevets, avoir déposé des demandes d’enregistrement de brevets ou être titulaire de marques, droits d’auteur ou autres droits de propriété intellectuelle portant sur tout ou partie des éléments qui font l’objet du présent document. Sauf stipulation expresse contraire d’un contrat de licence écrit de Microsoft, la fourniture de ce document n’a pas pour effet de vous concéder une licence sur ces brevets, marques, droits d’auteur ou autres droits de propriété intellectuelle.

Sauf indication contraire, les exemples de sociétés, les organisations, les produits, les noms de domaine, adresses de messagerie, logos, personnes, lieux et événements mentionnés sont fictif et aucune association avec n’importe quel ressemblance organisation, produit, nom de domaine, courrier électronique adresse logo, personne ou des événements est destiné, ou doit être déduite.

© 2009 Microsoft Corporation. Tous droits réservés.

Microsoft et Windows sont soit des marques déposées de Microsoft Corporation, soit des marques de Microsoft Corporation aux États-Unis et/ou dans d'autres pays.

Les noms des sociétés et des produits mentionnés dans le présent document peuvent être des marques de leurs propriétaires respectifs.
