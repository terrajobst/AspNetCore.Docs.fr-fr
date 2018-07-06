---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
title: Présentation des filtres d’Action (VB) | Microsoft Docs
author: microsoft
description: L’objectif de ce didacticiel est d’expliquer les filtres d’action. Un filtre d’action est un attribut que vous pouvez appliquer à une action de contrôleur--ou l’ensemble du contrôleur...
ms.author: aspnetcontent
ms.date: 10/16/2008
ms.assetid: e83812f2-c53e-4a43-a7c1-d64c59ecf694
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
msc.type: authoredcontent
ms.openlocfilehash: 07e02330747b3b8f8bb318d30c7a984aa15baeef
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811180"
---
<a name="understanding-action-filters-vb"></a>Présentation des filtres d’Action (VB)
====================
par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_VB.pdf)

> L’objectif de ce didacticiel est d’expliquer les filtres d’action. Un filtre d’action est un attribut que vous pouvez appliquer à une action de contrôleur--ou un ensemble du contrôleur--qui modifie la façon dont dans lequel l’action est exécutée.


## <a name="understanding-action-filters"></a>Présentation des filtres d’Action

L’objectif de ce didacticiel est d’expliquer les filtres d’action. Un filtre d’action est un attribut que vous pouvez appliquer à une action de contrôleur--ou un ensemble du contrôleur--qui modifie la façon dont dans lequel l’action est exécutée. L’infrastructure ASP.NET MVC inclut plusieurs filtres d’action :

- OutputCache – ce filtre d’action met en cache la sortie d’une action de contrôleur pour un laps de temps.
- HandleError – ce filtre d’action gère les erreurs sont survenues lors de l’exécution d’une action de contrôleur.
- Autoriser : ce filtre d’action vous permet de restreindre l’accès à un utilisateur particulier ou un rôle.

Vous pouvez également créer vos propres filtres d’action personnalisée. Par exemple, vous souhaiterez peut-être créer un filtre d’action personnalisé pour implémenter un système d’authentification personnalisé. Ou bien, vous souhaiterez peut-être créer un filtre d’action qui modifie les données retournées par une action de contrôleur d’affichage.

Dans ce didacticiel, vous allez apprendre à créer un filtre d’action à partir de zéro. Nous créons un filtre d’action de journal qui consigne les différentes étapes du traitement d’une action dans la fenêtre de sortie de Visual Studio.

### <a name="using-an-action-filter"></a>À l’aide d’un filtre d’Action

Un filtre d’action est un attribut. Vous pouvez appliquer la plupart des filtres d’action à une action de contrôleur individuel ou un ensemble du contrôleur.

Par exemple, le contrôleur de données dans la liste 1 expose une action nommée `Index()` qui retourne l’heure actuelle. Cette action est décorée avec le `OutputCache` filtre d’action. Ce filtre provoque la valeur retournée par l’action doit être mis en cache pendant 10 secondes.

**Liste 1 : `Controllers\DataController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample1.vb)]

Si vous appelez plusieurs fois le `Index()` action en entrant l’URL/Data/Index dans la barre d’adresses de votre navigateur et en appuyant sur l’actualisation bouton plusieurs fois, vous verrez le même temps pendant 10 secondes. La sortie de la `Index()` action est mise en cache pendant 10 secondes (voir Figure 1).


[![Temps en cache](understanding-action-filters-vb/_static/image2.png)](understanding-action-filters-vb/_static/image1.png)

**Figure 01**: heure de mise en cache ([cliquez pour afficher l’image en taille réelle](understanding-action-filters-vb/_static/image3.png))


Dans la liste 1, un filtre d’action unique : le `OutputCache` filtre d’action – est appliqué à la `Index()` (méthode). Si vous avez besoin, vous pouvez appliquer plusieurs filtres d’action à la même action. Par exemple, vous souhaiterez peut-être appliquer à la fois le `OutputCache` et `HandleError` filtres d’action à la même action.

Dans la liste 1, le `OutputCache` filtre d’action est appliqué à la `Index()` action. Vous pouvez également appliquer cet attribut à la `DataController` classe lui-même. Dans ce cas, le résultat retourné par une action exposée par le contrôleur est mis en cache pendant 10 secondes.

### <a name="the-different-types-of-filters"></a>Les différents Types de filtres

L’infrastructure ASP.NET MVC prend en charge quatre types de filtres :

1. Filtres d’autorisation : implémente le `IAuthorizationFilter` attribut.
2. Filtres d’action : implémente le `IActionFilter` attribut.
3. Filtres – implémente de résultats le `IResultFilter` attribut.
4. Filtres d’exception : implémente le `IExceptionFilter` attribut.

Les filtres sont exécutés dans l’ordre indiqué ci-dessus. Par exemple, les filtres d’autorisation sont toujours exécutés avant les filtres d’action et les filtres d’exception sont toujours exécutées après tous les autres types de filtre.

Filtres d’autorisation sont utilisés pour implémenter l’authentification et autorisation pour les actions de contrôleur. Par exemple, le filtre Authorize est un exemple de filtre d’autorisation.

Filtres d’action contiennent une logique qui est exécutée avant et après l’exécution d’une action de contrôleur. Vous pouvez utiliser un filtre d’action, par exemple, pour modifier les données d’affichage qui retourne une action de contrôleur.

Les filtres de résultats contiennent une logique qui est exécutée avant et après l’exécution du résultat d’une vue. Par exemple, vous souhaiterez peut-être modifier un résultat de vue juste avant que la vue est restituée dans le navigateur.

Filtres d’exception sont le dernier type de filtre à exécuter. Vous pouvez utiliser un filtre d’exception à gérer les erreurs déclenchées par vos actions de contrôleur ou résultats d’action de contrôleur. Vous pouvez également utiliser des filtres d’exception pour journaliser les erreurs.

Chaque type de filtre est exécuté dans un ordre particulier. Si vous souhaitez contrôler l’ordre dans lequel les filtres du même type sont exécutés vous pouvez définir les propriétés d’ordre d’un filtre.

La classe de base pour tous les filtres d’action est le `System.Web.Mvc.FilterAttribute` classe. Si vous souhaitez implémenter un type particulier de filtre, puis vous devez créer une classe qui hérite de la classe de filtre de base et implémente une ou plusieurs des IAuthorizationFilter, IActionFilter, IResultFilter ou ExceptionFilter interfaces.

### <a name="the-base-actionfilterattribute-class"></a>La classe de ActionFilterAttribute Base

Pour le rendre plus facile à implémenter un filtre d’action personnalisé, l’infrastructure ASP.NET MVC inclut une base `ActionFilterAttribute` classe. Cette classe implémente à la fois le `IActionFilter` et `IResultFilter` interfaces et hérite la `Filter` classe.

La terminologie employée ici n’est pas entièrement cohérente. Techniquement, une classe qui hérite de la classe ActionFilterAttribute est un filtre d’action et un filtre de résultat. Toutefois, dans le sens libre, le filtre d’action de word est utilisé pour faire référence à n’importe quel type de filtre dans l’infrastructure ASP.NET MVC.

La classe de ActionFilterAttribute base a les méthodes suivantes que vous pouvez substituer :

- OnActionExecuting – cette méthode est appelée avant l’exécution d’une action de contrôleur.
- OnActionExecuted – cette méthode est appelée après l’exécution d’une action de contrôleur.
- OnResultExecuting : cette méthode est appelée avant l’exécution d’un résultat d’action de contrôleur.
- OnResultExecuted – cette méthode est appelée après l’exécution d’un résultat d’action de contrôleur.

Dans la section suivante, nous verrons comment vous pouvez implémenter chacune de ces méthodes différentes.

### <a name="creating-a-log-action-filter"></a>Création d’un filtre d’Action de journal

Afin d’illustrer comment vous pouvez créer un filtre d’action personnalisé, nous allons créer un filtre d’action personnalisé qui enregistre les étapes du traitement d’une action de contrôleur à la fenêtre Sortie de Visual Studio. Notre `LogActionFilter` est contenue dans le Listing 2.

**Listing 2 : `ActionFilters\LogActionFilter.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample2.vb)]

Dans la liste 2, le `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`, et `OnResultExecuted()` toutes les méthodes appellent le `Log()` (méthode). Le nom de la méthode et les données d’itinéraire actuel est passé à la `Log()` (méthode). Le `Log()` méthode écrit un message dans la fenêtre Sortie de Visual Studio (voir Figure 2).


[![Écriture dans la fenêtre Sortie de Visual Studio](understanding-action-filters-vb/_static/image5.png)](understanding-action-filters-vb/_static/image4.png)

**Figure 02**: écriture dans la fenêtre Sortie de Visual Studio ([cliquez pour afficher l’image en taille réelle](understanding-action-filters-vb/_static/image6.png))


Le contrôleur Home dans le Listing 3 illustre comment vous pouvez appliquer le filtre d’action de journal à une classe d’ensemble du contrôleur. Chaque fois que toutes les actions exposées par le contrôleur Home sont appelés – soit les `Index()` méthode ou le `About()` méthode – les étapes du traitement de l’action sont enregistrés dans la fenêtre Sortie de Visual Studio.

**Liste 3 : `Controllers\HomeController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample3.vb)]

### <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous a présenté les filtres d’action ASP.NET MVC. Vous avez découvert les quatre différents types de filtres : filtres d’autorisation, les filtres d’action, les filtres de résultats et les filtres d’exception. Vous avez également appris sur la base `ActionFilterAttribute` classe.

Enfin, vous avez appris à implémenter un filtre d’action simple. Nous avons créé un filtre d’action de journal qui enregistre les étapes du traitement d’une action de contrôleur à la fenêtre Sortie de Visual Studio.

> [!div class="step-by-step"]
> [Précédent](asp-net-mvc-routing-overview-vb.md)
> [Suivant](improving-performance-with-output-caching-vb.md)
