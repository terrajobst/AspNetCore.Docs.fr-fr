---
uid: web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
title: À l’aide de méthodes asynchrones dans ASP.NET 4.5 | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les notions de base de la création d’une application ASP.NET Web Forms asynchrone à l’aide de Visual Studio Express 2012 pour le Web, qui est un gratuite...
ms.author: riande
ms.date: 06/06/2012
ms.assetid: a585c9a2-7c8e-478b-9706-90f3739c50d1
msc.legacyurl: /web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 61bb977af9dca6fc080151ffd6ea3d966f1b1592
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325417"
---
<a name="using-asynchronous-methods-in-aspnet-45"></a>À l’aide de méthodes asynchrones dans ASP.NET 4.5
====================
par [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Ce didacticiel vous apprend les notions de base de la création d’une application ASP.NET Web Forms asynchrone à l’aide [Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/11), qui est une version gratuite de Microsoft Visual Studio. Vous pouvez également utiliser [Visual Studio 2012](https://www.microsoft.com/visualstudio/11). Les sections suivantes sont incluses dans ce didacticiel.
> 
> - [Comment les demandes sont traitées par le Pool de threads](#HowRequestsProcessedByTP)
> - [Choix des méthodes synchrones ou asynchrones](#ChoosingSyncVasync)
> - [L’exemple d’Application](#SampleApp)
> - [La Page synchrone Gizmos](#GizmosSynch)
> - [Création d’une Page asynchrone Gizmos](#CreatingAsynchGizmos)
> - [Exécution de plusieurs opérations en parallèle](#Parallel)
> - [À l’aide d’un jeton d’annulation](#CancelToken)
> - [Configuration du serveur pour les appels de Service Web de haute simultanéité/haute latence](#ServerConfig)
> 
> Un exemple complet est fourni pour ce didacticiel à l’adresse  
>  [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/) sur le [GitHub](https://github.com/) site.


ASP.NET 4.5 Web Pages en combinaison [.NET 4.5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) afin de pouvoir inscrire des méthodes asynchrones qui retournent un objet de type [tâche](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Le .NET Framework 4 a introduit un concept de programmation asynchrone appelé une [tâche](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) et prend en charge de ASP.NET 4.5 [tâche](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Les tâches sont représentées par le **tâche** type et les types associés dans le [System.Threading.Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx) espace de noms. Le .NET Framework 4.5 s’appuie sur cette prise en charge asynchrone avec le [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) et [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) mots clés qui facilitent l’utilisation avec [tâche](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) objets beaucoup moins complexes que la précédente approches asynchrones. Le [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) mot clé est un raccourci syntaxique pour indiquer un morceau de code d’attente de façon asynchrone sur un autre fragment de code. Le [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) mot clé représente un indicateur que vous pouvez utiliser pour marquer des méthodes comme méthodes asynchrones basées sur des tâches. La combinaison de **await**, **async**et le **tâche** objet rend beaucoup plus facile à écrire du code asynchrone dans .NET 4.5. Le nouveau modèle pour les méthodes asynchrones est appelé le *Task-based Asynchronous Pattern* (**appuyez sur**). Ce didacticiel suppose que vous êtes familiarisé avec la programmation asynchrone à l’aide [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) et [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) mots clés et les [tâche](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) espace de noms.

Pour plus d’informations sur l’utilisation [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) et [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) mots clés et les [tâche](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) espace de noms, consultez les références suivantes.

- [Livre blanc : Les asynchronie dans .NET](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Forum aux questions d’async/Await](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Programmation asynchrone Visual Basic](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>  Comment les demandes sont traitées par le Pool de threads

Sur le serveur web, le .NET Framework gère un pool de threads qui sont utilisés pour traiter des demandes ASP.NET. Lorsqu’une requête arrive, un thread du pool est distribué pour traiter cette demande. Si la demande est traitée de façon synchrone, le thread qui traite la demande est occupé lors de la demande est en cours de traitement, et que thread ne peut pas traiter d’autre.   
  
Cela peut être un problème, car le pool de threads peut devenir assez grand pour contenir le nombre de threads occupé. Toutefois, le nombre de threads dans le pool de threads est limité (la valeur maximale pour .NET 4.5 par défaut est de 5 000). Dans les grandes applications avec une forte concurrence des requêtes longues, tous les threads disponibles est occupés. Cette condition est appelée privation de thread. Lorsque cette condition est atteinte, le serveur web les files d’attente de demandes. Si la file d’attente de la demande est saturé, le serveur web rejette les demandes dont l’état HTTP 503 (serveur encombré). Le pool de threads CLR présente des limitations sur Nouvelle injections de thread. Si l’accès concurrentiel est en rafale (autrement dit, votre site web peut soudainement obtenez un grand nombre de demandes) et tous les threads de requête disponibles sont occupés en raison d’appels de serveur principal avec une latence élevée, le taux d’injection de thread limité peut rendre votre application à répondre très mal. En outre, chaque nouveau thread ajouté au pool de threads a surcharge (par exemple, 1 Mo de mémoire de la pile). Une application web à l’aide des méthodes synchrones aux appels de service une latence élevée, où le pool de threads a atteint la valeur par défaut de .NET 4.5 maximal de 5 000 threads consomme environ 5 Go plus de mémoire qu’une application en mesure du service de la même demande à l’aide de méthodes asynchrones et les threads que 50. Lorsque vous effectuez le travail asynchrone, vous n’utilisez pas toujours un thread. Par exemple, lorsque vous effectuez une demande de service web asynchrone, ASP.NET n’utilisera pas les threads entre les **async** appel de méthode et la **await**. L’utilisation de pool de threads pour traiter les demandes avec une latence élevée peut entraîner une grande quantité de mémoire et un faible taux d’utilisation du matériel du serveur.

## <a name="processing-asynchronous-requests"></a>Traitement des requêtes asynchrones

Dans les applications web qui voyez un grand nombre de demandes simultanées au démarrage ou a une charge de rafale (où l’accès concurrentiel augmente soudainement), vous rendre les appels de service web asynchrones pour augmenter la réactivité de votre application. Une demande asynchrone prend le même laps de temps à traiter comme une demande synchrone. Par exemple, si une demande appelle un service web qui nécessite deux secondes, la requête prend deux secondes qu’elle soit exécutée de façon synchrone ou asynchrone. Toutefois, lors d’un appel asynchrone, un thread n’est pas bloqué de répondre aux autres requêtes pendant qu’il attend la première demande terminer. Par conséquent, les requêtes asynchrones empêchent croissance de demande files d’attente et le thread de pool lorsqu’il existe plusieurs demandes simultanées qui appellent des opérations à long terme.

## <a id="ChoosingSyncVasync"></a>  Choix des méthodes synchrones ou asynchrones

Cette section répertorie des directives relatives aux situations d’utiliser les méthodes synchrones ou asynchrones. Il s’agit que de directives ; examiner chaque application individuellement pour déterminer si les méthodes asynchrones améliorer les performances.

En général, utilisez les méthodes synchrones pour les conditions suivantes :

- Les opérations sont simples ou exécution courte.
- Simplicité est plus importante que l’efficacité.
- Les opérations sont essentiellement des opérations UC plutôt que des opérations qui impliquent de disque complet ou la charge réseau. À l’aide de méthodes asynchrones sur les opérations de processeur de manière intensive ne présente aucun avantage et entraîne une surcharge plus.

En règle générale, utilisez les méthodes asynchrones pour les conditions suivantes :

- Vous appelez les services qui peuvent être consommées par le biais des méthodes asynchrones, et à l’aide de .NET 4.5 ou version ultérieure.
- Les opérations sont gourmandes en réseau ou/O-e et non le processeur.
- Parallélisme est plus important que la simplicité du code.
- Vous souhaitez fournir un mécanisme qui permet aux utilisateurs d’annuler une demande longue.
- Lorsque l’avantage de la commutation des threads out pondère le coût du commutateur de contexte. En règle générale, vous devez effectuer une méthode asynchrone si la méthode synchrone bloque le thread de demande ASP.NET en effectue aucun travail. En effectuant l’appel asynchrone, le thread de demande ASP.NET n’est pas bloqué effectue aucun travail en attendant que la demande de service web s’achève.
- Les tests montrent que les opérations bloquantes forment un goulet d’étranglement de performances de site et qu’IIS peut traiter plus de requêtes à l’aide de méthodes asynchrones pour ces appels bloquants.

  L’exemple téléchargeable montre comment utiliser efficacement les méthodes asynchrones. L’exemple fourni a été conçu pour fournir une démonstration simple de la programmation asynchrone dans ASP.NET 4.5. L’exemple n’est pas destiné à être une architecture de référence pour la programmation asynchrone dans ASP.NET. L’exemple de programme appelle [API Web ASP.NET](../../../web-api/index.md) méthodes qui appellent ensuite [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) pour simuler des appels de service web d’exécution longue. La plupart des applications de production n’affiche pas ces avantages exceptionnels en utilisant les méthodes asynchrones.   
  
Peu d’applications requièrent toutes les méthodes asynchrones. Souvent, la conversion quelques méthodes synchrones en méthodes asynchrones offre le meilleur rapport performances / travail requis.

## <a id="SampleApp"></a>  L’exemple d’Application

Vous pouvez télécharger l’exemple d’application à partir de [ https://github.com/RickAndMSFT/Async-ASP.NET ](https://github.com/RickAndMSFT/Async-ASP.NET) sur le [GitHub](https://github.com/) site. Le référentiel se compose de trois projets :

- *WebAppAsync*: projet The ASP.NET Web Forms qui utilise l’API Web **WebAPIpwg** service. La plupart du code de ce didacticiel est de ce projet.
- *WebAPIpgw*: projet de l’API Web ASP.NET MVC 4 qui implémente le `Products, Gizmos and Widgets` contrôleurs. Il fournit les données pour le *WebAppAsync* projet et le *Mvc4Async* projet.
- *Mvc4Async*: ASP.NET MVC 4 le projet qui contient le code utilisé dans un autre didacticiel. Il effectue des appels d’API Web à la **WebAPIpwg** service.

## <a id="GizmosSynch"></a>  La Page synchrone Gizmos

 Le code suivant illustre la `Page_Load` méthode synchrone qui est utilisé pour afficher la liste des gizmos. (Pour cet article, un gizmo est un appareil mécanique fictif.) 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample1.cs)]

Le code suivant illustre la `GetGizmos` méthode du service gizmo.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample2.cs)]

Le `GizmoService GetGizmos` méthode passe un URI à un service HTTP de l’API Web ASP.NET qui retourne une liste de données de gizmos. Le *WebAPIpgw* projet contient l’implémentation de l’API Web `gizmos, widget` et `product` contrôleurs.  
L’illustration suivante montre la page gizmos à partir de l’exemple de projet.

![Gizmos](using-asynchronous-methods-in-aspnet-45/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>  Création d’une Page asynchrone Gizmos

L’exemple utilise le nouveau [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) et [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) (disponible dans .NET 4.5 et Visual Studio 2012) de mots clés pour permettre au compilateur d’être chargée de maintenir les transformations complexes nécessaires pour programmation asynchrone. Le compilateur vous permet d’écrire le code à l’aide de que constructions de flux de contrôle synchrone de # et le compilateur applique automatiquement les transformations nécessaires pour utiliser des rappels afin d’éviter le blocage des threads.

Pages asynchrones ASP.NET doivent inclure le [Page](https://msdn.microsoft.com/library/ydy4x04a.aspx) directive avec le `Async` attribut défini sur « true ». Le code suivant illustre la [Page](https://msdn.microsoft.com/library/ydy4x04a.aspx) directive avec la `Async` attribut défini sur « true » pour le *GizmosAsync.aspx* page.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample3.aspx?highlight=1)]

Le code suivant illustre la `Gizmos` synchrone `Page_Load` (méthode) et le `GizmosAsync` page asynchrone. Si votre navigateur prend en charge la [HTML 5 &lt;marquer&gt; élément](http://www.w3.org/wiki/HTML/Elements/mark), vous verrez les modifications dans `GizmosAsync` dans la mise en surbrillance jaune.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample4.cs)]

La version asynchrone :

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample5.cs?highlight=3,6-7,9,11)]

 Les modifications suivantes ont été appliquées pour autoriser le `GizmosAsync` page être asynchrone.

- Le [Page](https://msdn.microsoft.com/library/ydy4x04a.aspx) directive doit avoir le `Async` attribut défini sur « true ».
- Le `RegisterAsyncTask` méthode est utilisée pour enregistrer une tâche asynchrone qui contient le code qui s’exécute de façon asynchrone.
- La nouvelle `GetGizmosSvcAsync` méthode est marquée avec le [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) mot clé, qui indique au compilateur de générer des rappels pour les parties du corps et de créer automatiquement un `Task` qui est retourné.
- &quot;Async&quot; a été ajouté au nom de méthode asynchrone. Ajout de « Async » n’est pas obligatoire, mais est la convention lors de l’écriture de méthodes asynchrones.
- Le type de retour de la nouvelle `GetGizmosSvcAsync` méthode est `Task`. Le type de retour de `Task` représente le travail en cours ainsi que les appelants de la méthode avec un descripteur via lequel attendre la fin de l’opération asynchrone.
- Le [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) mot clé a été appliqué à l’appel de service web.
- L’API de service web asynchrone a été appelée (`GetGizmosAsync`).

À l’intérieur de la `GetGizmosSvcAsync` une autre méthode asynchrone, de corps de méthode `GetGizmosAsync` est appelée. `GetGizmosAsync` retourne immédiatement un `Task<List<Gizmo>>` qui se termine finalement lorsque les données sont disponibles. Étant donné que vous ne souhaitez rien d’autre jusqu'à ce que vous avez les données gizmo, le code attend la tâche (à l’aide de la **await** mot clé). Vous pouvez utiliser la **await** mot clé uniquement dans les méthodes annotées avec le **async** mot clé.

Le **await** mot clé ne bloque pas le thread jusqu'à ce que la tâche est terminée. Il inscrit le reste de la méthode comme un rappel sur la tâche et retourne immédiatement. Lorsque la tâche attendue se termine par la suite, il appellera ce rappel et donc reprendre l’exécution de la droite de la méthode où elle s’était arrêtée. Pour plus d’informations sur l’utilisation de la [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) et [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) mots clés et les [tâche](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) espace de noms, consultez le [async références](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

Le code suivant illustre la `GetGizmos` et `GetGizmosAsync` méthodes.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample6.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample7.cs?highlight=1,4-8)]

 Les modifications asynchrones sont semblables à celles apportées à la **GizmosAsync** ci-dessus. 

- La signature de méthode a été annotée avec le [async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) mot clé, le type de retour a été remplacé par `Task<List<Gizmo>>`, et *Async* a été ajouté au nom de méthode.
- Asynchrone [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) classe est utilisée au lieu de synchrones [WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx) classe.
- Le [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) mot clé a été appliqué à la [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)[GetAsync](https://msdn.microsoft.com/library/hh158944(VS.110).aspx) méthode asynchrone.

L’illustration suivante montre la vue gizmo asynchrone.

![async](using-asynchronous-methods-in-aspnet-45/_static/image2.png)

La présentation de navigateurs des données gizmos est identique à la vue créée par l’appel synchrone. La seule différence est que la version asynchrone peut être plus performant charges sont élevées.

## <a name="registerasynctask-notes"></a>Notes de RegisterAsyncTask

Méthodes raccordement avec `RegisterAsyncTask` s’exécute immédiatement après [PreRender](https://msdn.microsoft.com/library/ms178472.aspx). Vous pouvez également utiliser les événements de page void async directement, comme indiqué dans le code suivant :

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample8.cs)]

L’inconvénient d’événements void async est que les développeurs n’a plus un contrôle total sur quand les exécuter. Par exemple, si les deux un .aspx ainsi qu’un. Définir Master `Page_Load` événements et un ou deux d'entre eux sont asynchrones, l’ordre d’exécution ne peut pas être garanti. L’ordre des gestionnaires d’événements non indeterminiate (tel que `async void Button_Click` ) s’applique. Pour la plupart des développeurs, cela doit être acceptable, mais ceux qui ont besoin d’un contrôle total sur l’ordre d’exécution doivent uniquement utiliser les API telles que `RegisterAsyncTask` qui consomment des méthodes qui retournent un objet de tâche.

## <a id="Parallel"></a>  Exécution de plusieurs opérations en parallèle

Méthodes asynchrones ont un avantage significatif sur les méthodes synchrones lorsqu’une action doit exécuter plusieurs opérations indépendantes. Dans l’exemple fourni, la page synchrone *PWG.aspx*(pour les produits, les Widgets et Gizmos) affiche les résultats de trois appels de service web pour obtenir une liste de produits, widgets et gizmos. Le [API Web ASP.NET](../../../web-api/index.md) projet qui fournit ces services utilise [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) pour simuler les temps de latence ou un réseau lent appelle. Lorsque le délai a la valeur 500 millisecondes, asynchrone *PWGasync.aspx* page prend un peu plus de 500 millisecondes se termine tout synchrones `PWG` version adopte de 1 500 millisecondes. Synchrones *PWG.aspx* page est affichée dans le code suivant.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample9.cs)]

Asynchrone `PWGasync` code-behind est illustré ci-dessous.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample10.cs?highlight=5,11,21)]

L’illustration suivante montre la vue retournée par asynchrone *PWGasync.aspx* page.

![](using-asynchronous-methods-in-aspnet-45/_static/image3.png)

## <a id="CancelToken"></a>  À l’aide d’un jeton d’annulation

Les méthodes asynchrones qui retournent `Task`sont annulable, c'est-à-dire qu’ils prennent un [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) paramètre lorsqu’elle est fournie avec le `AsyncTimeout` attribut de la [Page](https://msdn.microsoft.com/library/ydy4x04a.aspx) directive. Le code suivant illustre la *GizmosCancelAsync.aspx* page avec un délai d’expiration sur la seconde.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample11.aspx?highlight=1)]

Le code suivant illustre la *GizmosCancelAsync.aspx.cs* fichier.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample12.cs?highlight=6,9)]

Dans l’exemple d’application fourni, en sélectionnant le *GizmosCancelAsync* lien appelle le *GizmosCancelAsync.aspx* page et illustre l’annulation (par expiration) de l’appel asynchrone. Étant donné que le temps de retard est dans une plage aléatoire, vous devrez peut-être actualiser la page à deux reprises pour obtenir le message d’erreur de délai d’attente.

## <a id="ServerConfig"></a>  Configuration du serveur pour les appels de Service Web de haute simultanéité/haute latence

Pour profiter des avantages d’une application web asynchrones, vous devrez peut-être apporter des modifications à la configuration du serveur par défaut. Gardez les points suivants à l’esprit lors de la configuration et test de votre application web asynchrones de contrainte.

- Windows 7, Windows Vista, Windows 8 et tous les systèmes d’exploitation de clients Windows ont un maximum de 10 requêtes simultanées. Vous aurez besoin d’un système d’exploitation de Windows Server pour voir les avantages de méthodes asynchrones sous une charge élevée.
- Inscrire le .NET 4.5 avec IIS à partir d’une invite de commandes avec élévation de privilèges à l’aide de la commande suivante :  
  %windir%\Microsoft.NET\Framework64 \v4.0.30319\aspnet\_regiis -i  
  Consultez [outil ASP.NET IIS Registration (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- Vous devrez peut-être augmenter le [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) limite de file d’attente à partir de la valeur par défaut de 1 000 et 5 000. Si le paramètre est trop faible, vous pouvez voir [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) rejeter les demandes avec l’état HTTP 503. Pour modifier la limite de la file d’attente HTTP.sys :

    - Ouvrez le Gestionnaire des services Internet et accédez au volet de Pools d’applications.
    - Cliquez avec le bouton droit sur le pool d’applications cible et sélectionnez **paramètres avancés**.  
        ![Avancé](using-asynchronous-methods-in-aspnet-45/_static/image4.png)
    - Dans le **paramètres avancés** boîte de dialogue, choisissez *longueur de file d’attente* à partir de 1 000 à 5 000.  
        ![Longueur de file d’attente](using-asynchronous-methods-in-aspnet-45/_static/image5.png)  
  
  Notez dans les images ci-dessus, le .NET framework est répertorié en tant que version 4.0, même si le pool d’applications à l’aide de .NET 4.5. Pour comprendre cette différence, consultez les rubriques suivantes :

        - [.NET Versioning and Multi-Targeting - .NET 4.5 is an in-place upgrade to .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
        - [How to set an IIS Application or AppPool to use ASP.NET 3.5 rather than 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
        - [.NET Framework Versions and Dependencies](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- Si votre application est à l’aide des services web ou de noms System.NET pour communiquer avec un serveur principal via HTTP vous devrez peut-être augmenter le [connectionManagement/maxconnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx) élément. Pour les applications ASP.NET, il est limité par la fonctionnalité de configuration automatique à 12 fois le nombre d’unités centrales. Cela signifie que sur une procédure quadruple, vous pouvez avoir au maximum 12 \* 4 = 48 connexions simultanées à un point de terminaison IP. Étant donné que cela est lié à [autoConfig](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx), le moyen le plus simple d’augmenter `maxconnection` dans ASP.NET application consiste à définir [System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) par programmation dans l’à partir de `Application_Start` méthode dans le *global.asax* fichier. Consultez l’exemple à télécharger pour obtenir un exemple.
- Dans .NET 4.5, la valeur par défaut de 5000 pour [MaxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) devraient être appropriées.

## <a name="contributors"></a>Contributors

- [Levi Broderick](http://stackoverflow.com/users/59641/levi)
- [Tom Dykstra](http://www.bing.com/search?q=site%3Aasp.net+%22Tom+Dykstra%22+-forums.asp.net&amp;qs=n&amp;form=QBRE&amp;pq=site%3Aasp.net+%22tom+dykstra%22+-forums.asp.net&amp;sc=8-42&amp;sp=-1&amp;sk=)
- [Brad Wilson.](http://bradwilson.typepad.com/)
- [HongMei Ge](https://blogs.msdn.com/b/hongmeig/)
