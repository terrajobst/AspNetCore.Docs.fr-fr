---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: Global Gestion des erreurs dans ASP.NET Web API 2 | Microsoft Docs
author: davidmatson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 8703ea4c20003148c330bcd534ccacc1dc72c850
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377829"
---
<a name="global-error-handling-in-aspnet-web-api-2"></a>Global Gestion des erreurs dans ASP.NET Web API 2
====================
par [David Matson](https://github.com/davidmatson), [Rick Anderson](https://github.com/Rick-Anderson)

Aujourd'hui il n’existe aucun moyen facile dans l’API Web pour vous connecter ou gérer les erreurs dans le monde entier. Certaines exceptions non gérées peuvent être traitées via [filtres d’exception](exception-handling.md), mais il existe un nombre de cas qui ne peut pas gérer les filtres d’exception. Exemple :

1. Exceptions levées à partir des constructeurs de contrôleur.
2. Exceptions levées à partir des gestionnaires de messages.
3. Exceptions levées pendant le routage.
4. Exceptions levées pendant la sérialisation du contenu de réponse.

Nous voulons fournir un moyen simple et cohérent pour vous connecter et gérer (éventuellement) ces exceptions. 

Il existe deux cas principaux pour la gestion des exceptions, le cas où nous sommes en mesure d’envoyer une réponse d’erreur et le cas où tout ce que nous pouvons faire est l’exception de journal. Par exemple pour ce dernier cas, lorsqu’une exception est levée au milieu de diffusion en continu de contenu de la réponse ; Dans ce cas il est trop tard pour envoyer un nouveau message de réponse dans la mesure où le code d’état, des en-têtes et contenu partiel sont déjà passées sur le câble, donc nous simplement abandonner la connexion. Même si l’exception ne peut pas être gérée pour produire un nouveau message de réponse, nous avons toujours prendre en charge la journalisation de l’exception. Dans les cas où nous pouvons détecter une erreur, nous pouvons renvoyer une réponse d’erreur approprié comme indiqué dans le code suivant :

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>Options existantes

En plus de [filtres d’exception](exception-handling.md), [gestionnaires de messages](../advanced/http-message-handlers.md) peut être utilisé aujourd'hui pour observer toutes les réponses de niveau 500, mais agissant sur ces réponses est difficile, car ils ne disposent pas de contexte sur l’erreur d’origine. Gestionnaires de messages présentent également certaines des mêmes restrictions que les filtres d’exception concernant les cas elles peuvent traiter. Tandis que l’API Web dispose d’une infrastructure de traçage qui capture les conditions d’erreur l’infrastructure de suivi est destiné à des fins de diagnostics et n'est pas conçu ou adapté à l’exécution dans les environnements de production. Global Gestion des exceptions et journalisation doivent être des services qui peuvent s’exécuter lors de la production et être branchés de solutions de surveillance existantes (par exemple, [ELMAH](https://code.google.com/p/elmah/) ).

### <a name="solution-overview"></a>Présentation de la solution

 Nous fournissons deux nouveaux services remplaçable par l’utilisateur, [IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md) et IExceptionHandler, pour vous connecter et gérer les exceptions non gérées. Les services sont très similaires, avec deux principales différences :

1. Nous prenons en charge l’inscription de plusieurs journaux d’exception, mais uniquement un seul gestionnaire d’exceptions.
2. Enregistreurs d’événements exception toujours appelées, même si nous sommes sur le point d’abandonner la connexion. Gestionnaires d’exceptions appelés uniquement lorsque nous sommes toujours en mesure de choisir le message de réponse à envoyer.

Les deux services fournissent un accès à un contexte d’exception contenant des informations pertinentes à partir du point où l’exception a été détectée, en particulier le [HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx), le [HttpRequestContext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), la levée d’exception et la source de l’exception (voir détails ci-dessous).

### <a name="design-principles"></a>Principes de conception

1. **Aucune modification avec rupture** , car cette fonctionnalité est ajoutée dans une version mineure, une contrainte importante ayant un impact sur la solution est qu’il y avoir aucune modification avec rupture, soit vers le type de contrat ou un comportement. Cette contrainte écarté un nettoyage que nous souhaitons vous l’avez fait en termes de blocs catch existants activation des exceptions dans les 500 réponses. Ce nettoyage supplémentaire est quelque chose que nous pourrions prendre en compte pour une version majeure à venir. Si cela est important de vous votez sur à l’adresse [uservoice d’API Web ASP.NET](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception).
2. **Maintenir la cohérence avec l’API Web construit** pipeline de filtres de l’API Web est un excellent moyen pour gérer les problèmes transversaux grâce à la flexibilité de l’application de la logique dans une étendue spécifique à l’action globale ou contrôleur spécifique. Filtres, notamment les filtres d’exception, ont toujours les contextes d’action et le contrôleur, même lorsque les inscrit au niveau global. Que le contrat est judicieux pour les filtres, mais cela signifie que les filtres d’exception, celles qui sont encore dans le monde entier avec étendue, ne sont pas adapté à certains cas, tels que les exceptions à partir des gestionnaires de messages, des exceptions où aucun contexte d’action ou contrôleur existe. Si vous souhaitez utiliser l’étendue flexible offerte par les filtres pour la gestion des exceptions, nous avons besoin de filtres d’exception. Mais si nous avons besoin gérer les exceptions en dehors d’un contexte de contrôleur, nous devons également une construction distincte pour gérer les erreurs complet mondial (quelque chose sans les contraintes de contexte de contrôleur contexte et action).

### <a name="when-to-use"></a>Quand utiliser

- Enregistreurs d’événements exception sont la solution de voir tous les exception non gérée détectée par l’API Web.
- Gestionnaires d’exceptions sont la solution de personnalisation de toutes les réponses possibles aux exceptions non gérées, interceptées par l’API Web.
- Filtres d’exception sont la solution la plus simple pour traiter les exceptions non prises en charge de sous-ensemble liées à une action spécifique ou un contrôleur.

### <a name="service-details"></a>Détails du service

 Les interfaces de service Enregistreur d’événements et le Gestionnaire d’exception sont les méthodes async simple en prenant les contextes respectifs : 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 Nous fournissons également des classes de base pour ces deux interfaces. Substitution des méthodes core (synchrone ou asynchrone) est tout ce qui est nécessaire pour vous connecter ou gérer l’architecture recommandée à la fois. Pour la journalisation, la `ExceptionLogger` classe de base garantit que la méthode de journalisation principal est uniquement appelée une fois pour chaque exception (même si plus tard, il propage davantage la pile des appels et est interceptée à nouveau). Le `ExceptionHandler` classe de base appellera le cœur de méthode de gestion uniquement pour les exceptions en haut de la pile des appels, en ignorant hérité imbriquée des blocs catch. (Versions simplifiées de ces classes de base sont dans l’annexe ci-dessous). Les deux `IExceptionLogger` et `IExceptionHandler` recevoir des informations sur l’exception via un `ExceptionContext`.

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

Lorsque l’infrastructure appelle un enregistreur d’exceptions ou un gestionnaire d’exceptions, il fournira toujours une `Exception` et un `Request`. À l’exception des tests unitaires, il fournira également toujours un `RequestContext`. Il fournira rarement un `ControllerContext` et `ActionContext` (uniquement lorsque vous appelez à partir du bloc catch pour les filtres d’exception). Il fournira très rarement un `Response`(uniquement dans certains cas IIS lorsque, au milieu d’essayer d’écrire la réponse). Étant donné que certaines de ces propriétés peuvent être `null` il incombe au consommateur de vérifier les `null` avant d’accéder aux membres de la classe d’exception.`CatchBlock` est une chaîne indiquant le bloc catch vu l’exception. Les chaînes de bloc catch sont les suivantes :

- Ftpserver (méthode SendAsync)
- HttpControllerDispatcher (méthode SendAsync)
- HttpBatchHandler (méthode SendAsync)
- IExceptionFilter (traitement du ApiController du pipeline de filtres d’exception dans ExecuteAsync)
- Hôte OWIN :

    - HttpMessageHandlerAdapter.BufferResponseContentAsync (pour la mise en mémoire tampon de sortie)
    - HttpMessageHandlerAdapter.CopyResponseContentAsync (pour la diffusion en continu de sortie)
- Hôte Web :

    - HttpControllerHandler.WriteBufferedResponseContentAsync (pour la mise en mémoire tampon de sortie)
    - HttpControllerHandler.WriteStreamedResponseContentAsync (pour la diffusion en continu de sortie)
    - HttpControllerHandler.WriteErrorResponseContentAsync (pour les échecs de récupération d’erreur en mode de mise en mémoire tampon de sortie)

La liste de chaînes de bloc catch est également disponible via les propriétés statiques en lecture seule. (La chaîne de bloc catch core se trouvent sur le ExceptionCatchBlocks statique ; le reste s’affichent sur une classe statique chaque pour OWIN et web hôte).`IsTopLevelCatchBlock` est utile pour suivre le modèle recommandé de gestion des exceptions uniquement en haut de la pile des appels. Au lieu de transformer les exceptions dans les 500 réponses où que vous soyez qu'un bloc catch imbriqué se produit, un gestionnaire d’exceptions peut laisser des exceptions propager jusqu'à ce qu’ils sont sur le point d’être vu par l’hôte.

Outre le `ExceptionContext`, un enregistreur d’événements Obtient une information plus par le biais de la version complète `ExceptionLoggerContext`:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

La deuxième propriété, `CanBeHandled`, permet un enregistreur d’événements identifier une exception qui ne peut pas être gérée. Lorsque la connexion est sur le point d’être abandonnée et aucun nouveau message de réponse ne peut être envoyé, les enregistreurs d’événements seront appelés mais le gestionnaire sera ***pas*** être appelée, et les enregistreurs d’événements peuvent identifier ce scénario à partir de cette propriété.

Dans plus de la `ExceptionContext`, un gestionnaire obtient une seule propriété supplémentaire, elle peut donner à la version complète `ExceptionHandlerContext` pour gérer l’exception :

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

Un gestionnaire d’exceptions indique qu’il a géré une exception en définissant le `Result` propriété à un résultat d’action (par exemple, un [ExceptionResult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx), [InternalServerErrorResult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx), [ StatusCodeResult](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx), ou un résultat personnalisé). Si le `Result` propriété a la valeur null, l’exception n’est pas gérée et l’exception d’origine est levée à nouveau.

Pour les exceptions en haut de la pile des appels, nous avons une étape supplémentaire pour garantir que la réponse est appropriée pour les appelants de l’API. Si l’exception se propage jusqu'à l’ordinateur hôte, l’appelant verriez l’écran jaune de la mort ou un autre hôte fourni de réponse qui est généralement au format HTML et ne sont généralement pas une réponse d’erreur API appropriée. Dans ce cas, le démarrage du résultat non null, et uniquement si un gestionnaire d’exceptions personnalisé la définit explicitement retour au `null` (non gérées) est l’exception se propager à l’hôte. Paramètre `Result` à `null` dans ce cas peut être utile pour deux scénarios :

1. OWIN hébergé API Web avec les exceptions personnalisées d’intergiciel (middleware) inscrit avant/extérieur API Web.
2. Débogage via un navigateur local, où l’écran jaune de la mort est en fait une réponse utile pour une exception non gérée.

Pour les gestionnaires d’exceptions et les enregistreurs d’événements exception, nous ne le faites pas quoi que ce soit pour récupérer si l’enregistreur d’événements ou le gestionnaire lui-même lève une exception. (Autres que de laisser l’exception se propager, laisser des commentaires en bas de cette page si vous avez une meilleure approche.) Le contrat pour les enregistreurs d’événements exception et gestionnaires est qu’ils ne devraient pas laisser exceptions propager jusqu'à leurs appelants ; Sinon, l’exception juste propageront, souvent à l’hôte, ce qui entraîne une erreur HTML (par exemple, le code ASP. Écran jaune de NET) envoyées au client (qui n’est généralement pas l’option recommandée pour les appelants d’API qui attendent JSON ou XML).

## <a name="examples"></a>Exemples

### <a name="tracing-exception-logger"></a>Enregistreur d’exceptions de suivi

L’enregistreur d’événements exception ci-dessous envoyer des données d’exception à des sources de Trace configurés (y compris la fenêtre de sortie de débogage dans Visual Studio).

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>Gestionnaire d’exceptions Message erreur personnalisée

Les éléments suivants ci-dessous génère une réponse d’erreur personnalisée aux clients, notamment une adresse de messagerie pour contacter le support.

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>L’inscription des filtres d’Exception

Si vous utilisez le modèle de projet « Application Web ASP.NET MVC 4 » pour créer votre projet, placez votre code de configuration d’API Web à l’intérieur de la `WebApiConfig` de classe, dans le *application/_démarrer* dossier :

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>L’annexe : Détails de classe de Base

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
