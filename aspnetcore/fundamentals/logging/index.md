---
title: Journalisation dans ASP.NET Core
author: tdykstra
description: Découvrez plus en détail le framework de journalisation dans ASP.NET Core. Apprenez à utiliser les fournisseurs de journalisation intégrés et découvrez-en plus sur les fournisseurs tiers les plus courants.
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/01/2019
uid: fundamentals/logging/index
ms.openlocfilehash: ee7d4b2ae04b5f6c262acc5da0f86f90ab50585f
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65085667"
---
# <a name="logging-in-aspnet-core"></a>Journalisation dans ASP.NET Core

Article rédigé par [Steve Smith](https://ardalis.com/) et [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core prend en charge une API de journalisation qui fonctionne avec différents fournisseurs de journalisation tiers et intégrés. Cet article explique comment utiliser cette API de journalisation avec les fournisseurs de journalisation intégrés.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="add-providers"></a>Ajouter des fournisseurs

Un fournisseur de journalisation affiche ou stocke des journaux. Par exemple, le fournisseur Console affiche les journaux dans la console, et le fournisseur Azure Application Insights les stocke dans Azure Application Insights. Il est possible d’envoyer les journaux à plusieurs endroits en ajoutant plusieurs fournisseurs.

::: moniker range=">= aspnetcore-2.0"

Pour ajouter un fournisseur, appelez la méthode d’extension `Add{provider name}` du fournisseur dans *Program.cs* :

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=17-19)]

Le modèle de projet par défaut appelle <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, qui ajoute les fournisseurs de journalisation suivants :

* Console
* Débogage
* EventSource (à partir d’ASP.NET Core 2.2)

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_TemplateCode&highlight=7)]

Si vous utilisez `CreateDefaultBuilder`, vous pouvez remplacer les fournisseurs par défaut par ceux de votre choix. Appelez <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> et ajoutez les fournisseurs que vous souhaitez.

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=18-22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Pour utiliser un fournisseur, installez son package NuGet et appelez sa méthode d’extension sur une instance de <xref:Microsoft.Extensions.Logging.ILoggerFactory> :

[!code-csharp[](index/samples/1.x/TodoApiSample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

[L’injection de dépendances](xref:fundamentals/dependency-injection) ASP.NET Core fournit l’instance `ILoggerFactory`. Les méthodes d’extension `AddConsole` et `AddDebug` sont définies dans les packages [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) et [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/). Chaque méthode d’extension appelle la méthode `ILoggerFactory.AddProvider`, en passant une instance du fournisseur.

> [!NOTE]
> L’[exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/logging/index/samples/1.x) ajoute des fournisseurs de journalisation dans la méthode `Startup.Configure`. Pour obtenir la sortie de journal du code exécuté plus haut, ajoutez les fournisseurs de journalisation au constructeur de classe `Startup`.

::: moniker-end

Vous trouverez des informations sur les [fournisseurs de journalisation intégrés](#built-in-logging-providers) et les [fournisseurs de journalisation tiers](#third-party-logging-providers) plus loin dans cet article.

## <a name="create-logs"></a>Créer des journaux

Récupérez un objet <xref:Microsoft.Extensions.Logging.ILogger%601> auprès de l’injection de dépendances.

::: moniker range=">= aspnetcore-2.0"

L’exemple de contrôleur suivant crée des journaux `Information` et `Warning`. La *catégorie* est `TodoApiSample.Controllers.TodoController` (le nom de classe complet de `TodoController` dans l’exemple d’application) :

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=4,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

L’exemple Razor Pages suivant crée des journaux de *niveau* `Information` et de *catégorie* `TodoApiSample.Pages.AboutModel` :

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_LoggerDI&highlight=3, 7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Pages/About.cshtml.cs?name=snippet_CallLogMethods&highlight=4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

L’exemple précédent crée des journaux de *niveau* `Information` et `Warning` et de *catégorie* classe `TodoController`. 

::: moniker-end

Le *niveau* du journal indique la gravité de l’événement consigné. La *catégorie* du journal est une chaîne associée à chaque journal. L’instance `ILogger<T>` crée des journaux ayant comme catégorie un nom complet de type `T`. Les [niveaux](#log-level) et les [catégories](#log-category) sont expliqués plus en détail plus loin dans cet article. 

::: moniker range=">= aspnetcore-2.0"

### <a name="create-logs-in-startup"></a>Créer des journaux au démarrage

Pour écrire des journaux dans la classe `Startup`, ajoutez un paramètre `ILogger` dans la signature de constructeur :

[!code-csharp[](index/samples/2.x/TodoApiSample/Startup.cs?name=snippet_Startup&highlight=3,5,8,20,27)]

### <a name="create-logs-in-program"></a>Créer des journaux dans le programme

Pour écrire des journaux dans la classe `Program`, récupérez une instance `ILogger` auprès de l’injection de dépendances :

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_LogFromMain&highlight=9,10)]

::: moniker-end

### <a name="no-asynchronous-logger-methods"></a>Aucune méthode d’enregistreur d’événements asynchrone

La journalisation doit être suffisamment rapide par rapport au coût du code asynchrone en matière de performances. Si votre magasin de données de journalisation est lent, n’écrivez pas directement dedans. Écrivez au départ les messages de journal dans un magasin rapide, puis déplacez-les dans le magasin lent. Par exemple, si vous vous connectez à SQL Server, évitez de le faire directement dans une méthode `Log`, car les méthodes `Log` sont synchrones. Ajoutez plutôt de façon synchronisée des messages de journal à une file d’attente en mémoire, puis configurez un traitement en arrière-plan afin d’extraire les messages de la file d’attente et d’effectuer le travail asynchrone d’envoi des données vers SQL Server.

## <a name="configuration"></a>Configuration

La configuration de fournisseur de journalisation est fournie par un ou plusieurs fournisseurs de configuration :

* Formats de fichiers (INI, JSON et XML).
* Arguments de ligne de commande
* Variables d'environnement.
* Objets .NET en mémoire.
* Stockage [Secret Manager](xref:security/app-secrets) non chiffré.
* Magasin utilisateur chiffré comme [Azure Key Vault](xref:security/key-vault-configuration).
* Fournisseurs personnalisés (installés ou créés).

Par exemple, la configuration de journalisation est généralement fournie par la section `Logging` des fichiers de paramètres d’application. L’exemple suivant montre le contenu d’un fichier *appsettings.Development.json* standard :

::: moniker range=">= aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    },
    "Console":
    {
      "IncludeScopes": true
    }
  }
}
```

La propriété `Logging` peut avoir un niveau `LogLevel` et des propriétés de module fournisseur d'informations (Console ici).

La propriété `LogLevel` sous `Logging` spécifie le [niveau](#log-level) de journalisation minimal pour les catégories sélectionnées. Dans l’exemple, les catégories `System` et `Microsoft` sont consignées au niveau `Information`, et toutes les autres au niveau `Debug`.

Les autres propriétés sous `Logging` spécifient les fournisseurs de journalisation. Cet exemple concerne le fournisseur Console. Lorsqu’un fournisseur prend en charge les [étendues de journal](#log-scopes), `IncludeScopes` indique si elles sont activées. Une propriété de fournisseur (comme `Console` dans l’exemple) peut également spécifier une propriété `LogLevel`. `LogLevel` sous un fournisseur spécifie les niveaux à consigner pour ce fournisseur.

Si les niveaux sont spécifiés dans `Logging.{providername}.LogLevel`, ils remplacent ce qui est défini dans `Logging.LogLevel`.

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    }
  }
}
```

Les clés `LogLevel` représentent des noms de journal. La clé `Default` s’applique aux journaux qui ne sont pas explicitement répertoriés. La valeur représente le [niveau de journal](#log-level) appliqué au journal donné.

::: moniker-end

Pour plus d’informations sur l’implémentation des fournisseurs de configuration, consultez <xref:fundamentals/configuration/index>.

## <a name="sample-logging-output"></a>Exemple de sortie de la journalisation

Avec l’exemple de code présenté dans la section précédente, les journaux s’affichent dans la console lorsque l’application est exécutée en ligne de commande. Voici un exemple de sortie de la console :

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```

Les journaux précédents ont été générés par une requête HTTP Get à l’exemple d’application à l’adresse `http://localhost:5000/api/todo/0`.

Voici un exemple des mêmes journaux tels qu’ils s’affichent dans la fenêtre Débogage quand vous exécutez l’exemple d’application dans Visual Studio :

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

Les journaux créés par les appels de `ILogger` illustrés dans la section précédente commencent par « TodoApi.Controllers.TodoController ». Ceux qui commencent par les catégories « Microsoft » proviennent du code du framework ASP.NET Core. ASP.NET Core et le code d’application utilisent la même API de journalisation et les mêmes fournisseurs.

Les autres sections de cet article détaillent certains points et présentent les options de journalisation.

## <a name="nuget-packages"></a>Packages NuGet

Les interfaces `ILogger` et `ILoggerFactory` se trouvent dans [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), et leurs implémentations par défaut se trouvent dans [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/).

## <a name="log-category"></a>Catégorie de log

Quand un objet `ILogger` est créé, une *catégorie* lui est spécifiée. Cette catégorie est incluse dans tous les messages de journal créés par cette instance de `ILogger`. Si la catégorie peut être n’importe quelle chaîne, la convention est d’utiliser le nom de la classe, par exemple « TodoApi.Controllers.TodoController ».

Utilisez `ILogger<T>` pour obtenir une instance de `ILogger` qui utilise le nom de type complet `T` comme catégorie :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

::: moniker-end

Pour spécifier explicitement la catégorie, appelez `ILoggerFactory.CreateLogger` :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

::: moniker-end

`ILogger<T>` équivaut à appeler `CreateLogger` avec le nom de type complet `T`.

## <a name="log-level"></a>Niveau du log

Chaque journal spécifie une valeur <xref:Microsoft.Extensions.Logging.LogLevel>. Le niveau de journalisation indique la gravité ou l’importance. Vous pourriez par exemple écrire un journal `Information` lorsqu’une méthode se termine normalement et un journal `Warning` lorsqu’une méthode retourne un code de statut *404 Not Found*.

Le code suivant crée des journaux `Information` et `Warning` :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

Dans le code précédent, le premier paramètre est [l’ID de l’événement de journal](#log-event-id). Le deuxième paramètre est un modèle de message contenant des espaces réservés pour les valeurs d’argument fournies par les autres paramètres de méthode. Les paramètres de méthode sont expliqués dans la section [Modèle de message](#log-message-template) plus loin dans cet article.

Les méthodes de journal qui comportent le niveau dans leur nom (par exemple, `LogInformation` et `LogWarning`) sont des [méthodes d’extension pour ILogger](xref:Microsoft.Extensions.Logging.LoggerExtensions). Elles appellent une méthode `Log` qui prend un paramètre `LogLevel`. Vous pouvez appeler la méthode `Log` directement au lieu d’appeler ces méthodes d’extension, mais la syntaxe est relativement complexe. Pour plus d’informations, voir <xref:Microsoft.Extensions.Logging.ILogger> et le [code source des extensions d’enregistreur d'événements](https://github.com/aspnet/Extensions/blob/release/2.2/src/Logging/Logging.Abstractions/src/LoggerExtensions.cs).

ASP.NET Core définit les niveaux de journalisation suivants, classés selon leur degré de gravité (du plus faible au plus élevé).

* Trace = 0

  Informations en général exclusivement utiles à des fins de débogage. Ces messages peuvent contenir des données d’application sensibles. Ils ne doivent donc pas être activés dans un environnement de production. *Niveau désactivé par défaut.*

* Debug = 1

  Informations qui peuvent être utiles dans le développement et le débogage. Exemple : `Entering method Configure with flag set to true.` En raison de leur volume élevé, activez les journaux de niveau `Debug` en production seulement pour résoudre des problèmes.

* Information = 2

  Informations de suivi du flux général de l’application. Ces journaux ont généralement une utilité à long terme. Exemple : `Request received for path /api/todo`

* Warning = 3

  Informations sur les événements anormaux ou inattendus dans le flux de l’application. Il peut s’agir d’erreurs ou d’autres situations qui ne provoquent pas l’arrêt de l’application, mais qu’il peut être intéressant d’examiner. Le niveau de journalisation `Warning` est généralement utilisé pour les exceptions gérées. Exemple : `FileNotFoundException for file quotes.txt.`

* Error = 4

  Fournit des informations sur des erreurs et des exceptions qui ne peuvent pas être gérées. Ces messages indiquent un échec de l’activité ou de l’opération en cours (par exemple, la requête HTTP actuellement exécutée), pas un échec au niveau de l’application. Exemple de message de log : `Cannot insert record due to duplicate key violation.`

* Critical = 5

  Fournit des informations sur des échecs qui nécessitent un examen immédiat. Exemples : perte de données, espace disque insuffisant.

Le niveau de journalisation permet de contrôler le volume de la sortie de journal écrite sur un support de stockage ou dans une fenêtre d’affichage. Par exemple :

* En production, envoyez `Trace` au niveau `Information` dans un magasin de données de volume. Envoyez `Warning` au niveau `Critical` à un magasin de données de valeurs.
* Pendant le développement, envoyez `Warning` au niveau `Critical` à la console et ajoutez `Trace` au niveau `Information` lors de la résolution des problèmes.

La section [Filtrage de log](#log-filtering) plus loin dans cet article explique comment déterminer les niveaux de journalisation gérés par un fournisseur.

ASP.NET Core écrit des journaux pour les événements de framework. Aucun journal du niveau `Debug` ou `Trace` n’était créé dans les exemples de journaux présentés plus haut dans cet article, puisque les journaux au-dessous du niveau `Information` étaient exclus. Voici un exemple de journaux Console produits par l’exemple d’application configurée pour afficher les journaux `Debug` :

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

## <a name="log-event-id"></a>ID d’événement de log

Chaque journal peut spécifier un *ID d’événement*. Pour cela, l’exemple d’application utilise une classe `LoggingEvents` définie localement :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/2.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/samples/1.x/TodoApiSample/Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

::: moniker-end

Un ID d’événement associe un jeu d’événements. Par exemple, tous les journaux liés à l’affichage d’une liste d’éléments sur une page peuvent être 1001.

Le fournisseur de journalisation peut stocker l’ID d’événement dans un champ ID, dans le message de journalisation, ou pas du tout. Le fournisseur Debug n’affiche pas les ID d’événements. Le fournisseur Console affiche les ID d’événements entre crochets après la catégorie :

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>Modèle de message de log

Chaque journal spécifie un modèle de message. Ce dernier peut contenir des espaces réservés pour lesquels les arguments sont fournis. Utilisez des noms et non des nombres pour les espaces réservés.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

::: moniker-end

L’ordre des espaces réservés, pas leurs noms, détermine quels paramètres sont utilisés pour fournir leurs valeurs. Dans le code suivant, on voit que les noms de paramètres sont hors séquence dans le modèle de message :

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

Ce code crée un message de journal avec les valeurs des paramètres dans la séquence :

```
Parameter values: parm1, parm2
```

Le framework de journalisation fonctionne ainsi pour permettre aux fournisseurs de journalisation d’implémenter la [journalisation sémantique, également appelée journalisation structurée](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging). Les arguments proprement dits, et pas seulement le modèle de message mis en forme, sont transmis au système de journalisation. C’est grâce à ces informations que les fournisseurs de journalisation peuvent stocker les valeurs des paramètres sous forme de champs. Supposons par exemple que les appels de méthodes d’enregistreur d’événements se présentent ainsi :

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

Si vous envoyez les journaux au Stockage Table Azure, chaque entité Table Azure peut avoir les propriétés `ID` et `RequestTime`, ce qui simplifie les requêtes sur les données de journaux. Une requête peut rechercher tous les journaux compris dans une plage `RequestTime` spécifique sans analyser le délai d’expiration du message texte.

## <a name="logging-exceptions"></a>Journalisation des exceptions

Les méthodes logger ont des surcharges qui vous permettent de passer une exception, comme dans l’exemple suivant :

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

::: moniker-end

Tous les fournisseurs ne gèrent pas les informations sur les exceptions de la même façon. Voici un exemple de sortie du fournisseur Debug extrait du code montré plus haut.

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>Filtrage de journal

::: moniker range=">= aspnetcore-2.0"

Vous pouvez spécifier un niveau de journalisation minimum pour une catégorie ou un fournisseur spécifique, ou pour l’ensemble des fournisseurs ou des catégories. Les enregistrements de log en dessous du niveau minimum ne sont pas passés à ce fournisseur, et ne sont donc pas affichés ou stockés.

Pour supprimer tous les journaux, choisissez `LogLevel.None` comme niveau de journalisation minimal. La valeur entière de `LogLevel.None` est 6, soit un niveau supérieur à `LogLevel.Critical` (5).

### <a name="create-filter-rules-in-configuration"></a>Créer des règles de filtre dans la configuration

Le code du modèle de projet appelle `CreateDefaultBuilder` afin de configurer la journalisation pour les fournisseurs Console et Debug. La méthode `CreateDefaultBuilder` définit également la journalisation pour rechercher la configuration dans une section `Logging`, à l’aide de code similaire à celui-ci :

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_ExpandDefault&highlight=16)]

Les données de configuration spécifient des niveaux de journalisation minimum par fournisseur et par catégorie, comme dans l’exemple suivant :

[!code-json[](index/samples/2.x/TodoApiSample/appsettings.json)]

Ce code JSON crée six règles de filtre : une pour le fournisseur Debug, quatre pour le fournisseur Console et une pour tous les fournisseurs. Une seule règle est choisie pour chaque fournisseur à la création d’un objet `ILogger`.

### <a name="filter-rules-in-code"></a>Règles de filtre dans le code

L'exemple suivant montre comment enregistrer des règles de filtre dans le code :

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

Le second `AddFilter` spécifie le fournisseur Debug par son nom de type. Le premier `AddFilter` s’applique à tous les fournisseurs, car il ne spécifie aucun type de fournisseur.

### <a name="how-filtering-rules-are-applied"></a>Mode d’application des règles de filtre

Les données de configuration et le code `AddFilter` contenus dans les exemples précédents créent les règles présentées dans le tableau suivant. Les six premières proviennent de l’exemple de configuration et les deux dernières, de l’exemple de code.

| nombre | Fournisseur      | Catégories commençant par...          | Niveau de journalisation minimum |
| :----: | ------------- | --------------------------------------- | ----------------- |
| 1      | Débogage         | Toutes les catégories                          | Information       |
| 2      | Console       | Microsoft.AspNetCore.Mvc.Razor.Internal | Warning           |
| 3      | Console       | Microsoft.AspNetCore.Mvc.Razor.Razor    | Débogage             |
| 4      | Console       | Microsoft.AspNetCore.Mvc.Razor          | Error             |
| 5      | Console       | Toutes les catégories                          | Information       |
| 6      | Tous les fournisseurs | Toutes les catégories                          | Débogage             |
| 7      | Tous les fournisseurs | Système                                  | Débogage             |
| 8      | Débogage         | Microsoft                               | Suivi             |

À la création d’un objet `ILogger`, l’objet `ILoggerFactory` sélectionne une seule règle à appliquer à cet enregistrement d’événements par fournisseur. Tous les messages écrits par une instance `ILogger` sont filtrés selon les règles sélectionnées. La règle la plus spécifique pouvant être appliquée à chaque paire catégorie/fournisseur est sélectionnée parmi les règles disponibles.

L’algorithme suivant est utilisé pour chaque fournisseur quand un objet `ILogger` est créé pour une catégorie donnée :

* Sélectionnez toutes les règles qui correspondent au fournisseur ou à son alias. Si aucune correspondance n’est trouvée, sélectionnez toutes les règles avec un fournisseur vide.
* À partir du résultat de l’étape précédente, sélectionnez les règles ayant le plus long préfixe de catégorie correspondant. Si aucune correspondance n’est trouvée, sélectionnez toutes les règles qui ne spécifient pas de catégorie.
* Si plusieurs règles sont sélectionnées, prenez la **dernière**.
* Si aucune règle n’est sélectionnée, utilisez `MinimumLevel`.

Avec la liste de règles précédente, supposons que vous créez un objet `ILogger` pour la catégorie « Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine » :

* Les règles 1, 6 et 8 s’appliquent au fournisseur Debug. La règle 8 est sélectionnée, car c’est la plus spécifique.
* Les règles 3, 4, 5 et 6 s’appliquent au fournisseur Console. La règle 3 est la plus spécifique.

L’instance `ILogger` ainsi produite envoie des journaux de niveau `Trace` ou supérieur au fournisseur Debug. Les journaux de niveau `Debug` et supérieurs sont envoyés au fournisseur Console.

### <a name="provider-aliases"></a>Alias de fournisseur

Chaque fournisseur définit un *alias* qui peut être utilisé dans la configuration à la place du nom de type complet.  Pour les fournisseurs intégrés, utilisez les alias suivants :

* Console
* Débogage
* EventSource
* EventLog
* TraceSource
* AzureAppServicesFile
* AzureAppServicesBlob
* ApplicationInsights

### <a name="default-minimum-level"></a>Niveau minimum par défaut

Un paramètre de niveau minimum est utilisé uniquement si aucune règle de la configuration ou du code ne s’applique à une catégorie ou un fournisseur spécifique. L’exemple suivant montre comment définir le niveau minimum :

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_MinLevel&highlight=3)]

Si vous ne définissez pas explicitement le niveau minimum, la valeur par défaut est `Information`, ce qui signifie que les niveaux `Trace` et `Debug` sont ignorés.

### <a name="filter-functions"></a>Fonctions de filtre

Une fonction de filtre est appelée pour tous les fournisseurs et toutes les catégories pour lesquels la configuration ou le code n’applique aucune règle. Le code de la fonction a accès au type de fournisseur, à la catégorie et au niveau de journalisation. Par exemple :

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Certains fournisseurs de journalisation vous permettent de spécifier quand des enregistrements de log doivent être écrits sur un média de stockage, ou au contraire ignorés, en fonction de la catégorie et du niveau de journalisation.

Les méthodes d’extension `AddConsole` et `AddDebug` offrent des surcharges qui acceptent des critères de filtrage. Dans l’exemple de code suivant, le fournisseur Console ignore les enregistrements en dessous du niveau `Warning`, et le fournisseur Debug ignore les enregistrements créés par le framework.

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

La méthode `AddEventLog` a une surcharge qui accepte une instance `EventLogSettings`, dont la propriété `Filter` peut contenir une fonction de filtre. Le fournisseur TraceSource ne fournit aucune de ces surcharges, étant donné que son niveau de journalisation et d’autres paramètres dépendent des `SourceSwitch` et `TraceListener` qu’il utilise.

Pour définir des règles de filtrage sur tous les fournisseurs inscrits auprès d’une instance `ILoggerFactory`, utilisez la méthode d’extension `WithFilter`. L’exemple suivant limite les journaux du framework (dont la catégorie commence par « Microsoft » ou « System ») aux avertissements, tout en effectuant une journalisation au niveau Debug pour les journaux créés par le code de l’application.

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

Pour empêcher l’écriture de journaux, choisissez `LogLevel.None` comme niveau de journalisation minimal. La valeur entière de `LogLevel.None` est 6, soit un niveau supérieur à `LogLevel.Critical` (5).

La méthode d’extension `WithFilter` est fournie par le package NuGet [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter). La méthode retourne une nouvelle instance `ILoggerFactory` qui filtre les messages de log passés à tous les fournisseurs de journalisation inscrits dans cette méthode. Cela n’affecte pas les autres instances `ILoggerFactory`, y compris l’instance `ILoggerFactory` initiale.

::: moniker-end

## <a name="system-categories-and-levels"></a>Niveaux et les catégories de système

Voici quelques catégories utilisées par ASP.NET Core et Entity Framework Core, avec des notes sur les journaux associés :

| Category                            | Notes |
| ----------------------------------- | ----- |
| Microsoft.AspNetCore                | Diagnostics ASP.NET Core généraux. |
| Microsoft.AspNetCore.DataProtection | Liste des clés considérées, trouvées et utilisées. |
| Microsoft.AspNetCore.HostFiltering  | Hôtes autorisés. |
| Microsoft.AspNetCore.Hosting        | Temps de traitement des requêtes HTTP et heure de démarrage. Liste des assemblys de démarrage d’hébergement chargés. |
| Microsoft.AspNetCore.Mvc            | Diagnostics MVC et Razor. Liaison de données, exécution de filtres, compilation de vues, sélection d’actions. |
| Microsoft.AspNetCore.Routing        | Informations de correspondance des itinéraires. |
| Microsoft.AspNetCore.Server         | Démarrage et arrêt de la connexion, et réponses persistantes. Informations du certificat HTTPS. |
| Microsoft.AspNetCore.StaticFiles    | Fichiers pris en charge. |
| Microsoft.EntityFrameworkCore       | Diagnostics Entity Framework Core généraux. Activité et configuration de la base de données, détection des modifications, migrations. |

## <a name="log-scopes"></a>Étendues de log

 Une *étendue* peut regrouper un ensemble d’opérations logiques. Ce regroupement permet de joindre les mêmes données à tous les journaux créés au sein d’un ensemble. Par exemple, chaque journal créé dans le cadre du traitement d’une transaction peut contenir l’ID de la transaction.

Une étendue est un type `IDisposable` qui est retourné par la méthode <xref:Microsoft.Extensions.Logging.ILogger.BeginScope*>. Elle s’applique tant qu’elle n’est pas supprimée. Utilisez une étendue en incluant les appels de l’enregistrement d’événements dans un bloc `using` :

[!code-csharp[](index/samples/1.x/TodoApiSample/Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

Le code suivant active les étendues pour le fournisseur Console :

::: moniker range="> aspnetcore-2.0"

*Program.cs* :

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> La configuration de l’option logger `IncludeScopes` pour la console est nécessaire pour activer la journalisation basée sur des étendues.
>
> Pour plus d'informations sur la configuration, reportez-vous à la section [Configuration](#configuration).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

*Program.cs* :

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> La configuration de l’option logger `IncludeScopes` pour la console est nécessaire pour activer la journalisation basée sur des étendues.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

*Startup.cs* :

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

Chaque message de log fournit les informations incluses dans l’étendue :

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>Fournisseurs de journalisation intégrés

ASP.NET Core contient les fournisseurs suivants :

* [Console](#console-provider)
* [Déboguer](#debug-provider)
* [EventSource](#eventsource-provider)
* [EventLog](#windows-eventlog-provider)
* [TraceSource](#tracesource-provider)
* [AzureAppServicesFile](#azure-app-service-provider)
* [AzureAppServicesBlob](#azure-app-service-provider)
* [ApplicationInsights](#azure-application-insights-trace-logging)

Pour plus d'informations sur la journalisation stdout, voir <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log> et <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.

### <a name="console-provider"></a>Fournisseur Console

Le package de fournisseur [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) envoie la sortie de log dans la console. 

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

Les [surcharges AddConsole](xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions) permettent de transmettre un niveau de journalisation minimal, une fonction de filtre et une valeur booléenne qui indique si les étendues sont prises en charge. Une autre option consiste à passer un objet `IConfiguration`, qui permet de spécifier la prise en charge des étendues et les niveaux de journalisation.

Le fournisseur Console a un impact significatif sur les performances et n’est généralement pas adapté à une utilisation en production.

Quand vous créez un projet dans Visual Studio, la méthode `AddConsole` ressemble à ceci :

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

Ce code fait référence à la section `Logging` du fichier *appSettings.json* :

[!code-json[](index/samples/1.x/TodoApiSample//appsettings.json)]

Les paramètres définis limitent les enregistrements du framework aux avertissements, mais active ceux de l’application au niveau Debug, comme cela est expliqué dans la section [Filtrage de log](#log-filtering). Pour plus d’informations, consultez [Configuration](xref:fundamentals/configuration/index).

::: moniker-end

Pour voir la sortie de la journalisation Console, ouvrez une invite de commandes dans le dossier du projet et exécutez la commande suivante :

```console
dotnet run
```

### <a name="debug-provider"></a>Fournisseur Debug

Le package de fournisseur [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) écrit la sortie de log à l’aide de la classe [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (appels de la méthode `Debug.WriteLine`).

Sur Linux, ce fournisseur écrit la sortie de log dans */var/log/message*.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

Les [surcharges AddDebug](xref:Microsoft.Extensions.Logging.DebugLoggerFactoryExtensions) vous permettent de passer un niveau de log minimum ou une fonction de filtre.

::: moniker-end

### <a name="eventsource-provider"></a>Fournisseur EventSource

Pour les applications qui ciblent ASP.NET Core 1.1.0 ou ultérieur, le package de fournisseur [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) peut implémenter le suivi des événements. Sur Windows, il utilise [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803). Le fournisseur est multiplateforme, mais il ne prend pas en charge la collection d’événements ni les outils d’affichage pour Linux ou macOS.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventSourceLogger();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventSourceLogger();
```

::: moniker-end

[L’utilitaire PerfView](https://github.com/Microsoft/perfview) est très utile pour collecter et afficher les journaux. Il existe d’autres outils d’affichage des journaux ETW, mais PerfView est l’outil recommandé pour gérer les événements ETW générés par ASP.NET.

Pour configurer PerfView afin qu’il collecte les événements enregistrés par ce fournisseur, ajoutez la chaîne `*Microsoft-Extensions-Logging` à la liste des **fournisseurs supplémentaires**. (N’oubliez pas d’inclure l’astérisque au début de la chaîne.)

![Fournisseurs supplémentaires dans PerfView](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a>Fournisseur EventLog Windows

Le package de fournisseur [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) envoie la sortie de log dans le journal des événements Windows.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

Les [surcharges AddEventLog](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) vous permettent de passer `EventLogSettings` ou un niveau de journalisation minimum.

::: moniker-end

### <a name="tracesource-provider"></a>Fournisseur TraceSource

Le package de fournisseur [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) utilise les bibliothèques et fournisseurs <xref:System.Diagnostics.TraceSource>.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddTraceSource(sourceSwitchName);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

::: moniker-end

Les [surcharges AddTraceSource](xref:Microsoft.Extensions.Logging.TraceSourceFactoryExtensions) vous permettent de passer un commutateur de source et un écouteur de suivi.

Pour pouvoir utiliser ce fournisseur, il faut que l’application s’exécute sur .NET Framework (au lieu de .NET Core). Le fournisseur peut acheminer les messages vers différents [détecteurs d’événements](/dotnet/framework/debug-trace-profile/trace-listeners), comme <xref:System.Diagnostics.TextWriterTraceListener> (utilisé dans l’exemple d’application).

::: moniker range="< aspnetcore-2.0"

L’exemple suivant configure un fournisseur `TraceSource` qui enregistre les messages `Warning` et de niveau supérieur dans la fenêtre de la console.

[!code-csharp[](index/samples/1.x/TodoApiSample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

::: moniker-end

### <a name="azure-app-service-provider"></a>Fournisseur Azure App Service

Le package de fournisseur [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) écrit les enregistrements de log dans des fichiers texte dans le système de fichiers d’une application Azure App Service, et dans un [stockage Blob](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) dans un compte de stockage Azure. Le package de fournisseur est disponible pour les applications ciblant .NET Core 1.1 ou ultérieur.

::: moniker range=">= aspnetcore-2.0"

Si vous ciblez .NET Core, notez les points suivants :

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* Le package du fournisseur est inclus dans le [métapaquet Microsoft.AspNetCore.All](xref:fundamentals/metapackage) ASP.NET Core.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* Le package du fournisseur n’est pas inclus dans le [métapaquet Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Pour utiliser le fournisseur, installez le package.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Si vous ciblez le .NET Framework ou référencez le métapackage `Microsoft.AspNetCore.App`, ajoutez le package de fournisseur dans le projet. Appelez `AddAzureWebAppDiagnostics` :

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="<= aspnetcore-2.1"

Une surcharge <xref:Microsoft.Extensions.Logging.AzureAppServicesLoggerFactoryExtensions.AddAzureWebAppDiagnostics*> permet de transmettre <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureAppServicesDiagnosticsSettings>. L’objet de paramètres peut remplacer les paramètres par défaut, comme le modèle de sortie de journalisation, le nom d’objet blob et la limite de taille de fichier. (*Modèle de sortie* est un modèle de message qui s’applique à tous les journaux en plus de ce qui est fourni avec un appel de méthode `ILogger`.)

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Pour configurer les paramètres du fournisseur, utilisez <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> et <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, comme illustré dans l’exemple suivant :

[!code-csharp[](index/samples/2.x/TodoApiSample/Program.cs?name=snippet_AzLogOptions&highlight=19-27)]

::: moniker-end

En cas de déploiement sur une application App Service, celle-ci applique les paramètres définis dans la section [Journaux de diagnostic](/azure/app-service/web-sites-enable-diagnostic-log/#enablediag) de la page **App Service** du Portail Azure. Lorsque ces paramètres sont mis à jour, les changements prennent effet immédiatement sans avoir besoin de redémarrer ou redéployer l’application.

![Paramètres de journalisation Azure](index/_static/azure-logging-settings.png)

L’emplacement par défaut des fichiers journaux est le dossier *D:\\racine\\LogFiles\\Application*, et le nom de fichier par défaut est *diagnostics-aaaammjj.txt*. La limite de taille de fichier par défaut est de 10 Mo, et le nombre maximal de fichiers conservés par défaut est de 2. Le nom par défaut du blob est *{app-name}{timestamp}/aaaa/mm/jj/hh/{guid}-applicationLog.txt*.

Le fournisseur fonctionne uniquement quand le projet s’exécute dans l’environnement Azure. Il n’a aucun effet si le projet s’exécute localement &mdash; il n’écrit pas d’enregistrements dans les fichiers locaux ou dans le stockage de développement local pour les objets blob.

#### <a name="azure-log-streaming"></a>Streaming des journaux Azure

La diffusion en continu des journaux Azure permet d’afficher l’activité de journalisation en temps réel aux emplacements suivants :

* Serveur d'applications
* Serveur web
* Suivi des demandes ayant échoué

Pour configurer le streaming des journaux Azure :

* Accédez à la page **Journaux de diagnostic** sur le portail de votre application.
* Définissez **Journalisation des applications (Système de fichiers)** sur **Activé**.

![Page Journaux de diagnostic du portail Azure](index/_static/azure-diagnostic-logs.png)

Accédez à la page **Diffusion en continu des journaux** pour voir les messages d’application. Ils sont consignés par application par le biais de l’interface `ILogger`.

![Streaming des journaux d’application dans le portail Azure](index/_static/azure-log-streaming.png)

::: moniker range=">= aspnetcore-1.1"

### <a name="azure-application-insights-trace-logging"></a>Journalisation des traces Azure Application Insights

Le package de fournisseur [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) écrit des journaux dans Azure Application Insights. Application Insights est un service qui surveille une application web et fournit des outils pour interroger et analyser les données de télémétrie. Si vous utilisez ce fournisseur, vous pouvez interroger et analyser vos journaux à l’aide des outils Application Insights.

Le fournisseur de journalisation est inclus en tant que dépendance de [Microsoft.ApplicationInsights.AspNetCore](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore), le package qui fournit toutes les données de télémétrie disponibles pour ASP.NET Core. Si vous utilisez ce package, vous n’avez pas besoin d’installer le package du fournisseur.

N’utilisez pas le [package Microsoft.ApplicationInsights.Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) package&mdash;destiné à ASP.NET 4.x.

Pour plus d'informations, reportez-vous aux ressources suivantes :

* [Vue d'ensemble d'Application Insights](/azure/application-insights/app-insights-overview)
* [Application Insights pour les applications ASP.NET Core](/azure/azure-monitor/app/asp-net-core-no-visualstudio) : commencez ici si vous souhaitez implémenter la gamme complète des données de télémétrie d’Application Insights en même temps que la journalisation.
* [Journaux ApplicationInsightsLoggerProvider pour .NET Core ILogger](/azure/azure-monitor/app/ilogger) : commencez ici si vous souhaitez implémenter le fournisseur de journalisation sans le reste des données de télémétrie Application Insights.
* [Adaptateurs de journalisation Application Insights](https://github.com/Microsoft/ApplicationInsights-dotnet-logging/blob/develop/README.md).
* [Installer, configurer et initialiser le kit de développement logiciel (SDK) Application Insights](/learn/modules/instrument-web-app-code-with-application-insights) : tutoriel interactif sur le site Microsoft Learn.
::: moniker-end

> [!NOTE]
> Depuis le 1/5/2019, l’article intitulé [Application Insights pour ASP.NET Core](/azure/azure-monitor/app/asp-net-core) est obsolète et les étapes du tutoriel ne fonctionnent plus. Reportez-vous à [Application Insights pour les applications ASP.NET Core](/azure/azure-monitor/app/asp-net-core-no-visualstudio) à la place. Nous sommes conscients du problème et travaillons à y remédier.

## <a name="third-party-logging-providers"></a>Fournisseurs de journalisation tiers

Frameworks de journalisation tiers qui sont pris en charge dans ASP.NET Core :

* [elmah.io](https://elmah.io/) ([dépôt GitHub](https://github.com/elmahio/Elmah.Io.Extensions.Logging))
* [Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([Dépôt GitHub](https://github.com/mattwcole/gelf-extensions-logging))
* [JSNLog](http://jsnlog.com/) ([dépôt GitHub](https://github.com/mperdeck/jsnlog))
* [KissLog.net](https://kisslog.net/) ([référentiel GitHub](https://github.com/catalingavan/KissLog-net))
* [Loggr](http://loggr.net/) ([dépôt GitHub](https://github.com/imobile3/Loggr.Extensions.Logging))
* [NLog](http://nlog-project.org/) ([dépôt GitHub](https://github.com/NLog/NLog.Extensions.Logging))
* [Sentry](https://sentry.io/welcome/) ([dépôt GitHub](https://github.com/getsentry/sentry-dotnet))
* [Serilog](https://serilog.net/) ([dépôt GitHub](https://github.com/serilog/serilog-aspnetcore))
* [Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([Github repo](https://github.com/googleapis/google-cloud-dotnet))

Certains frameworks tiers prennent en charge la [journalisation sémantique, également appelée journalisation structurée](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

L’utilisation d’un framework tiers est semblable à l’utilisation des fournisseurs intégrés :

1. Ajoutez un package NuGet à votre projet.
1. Appelez `ILoggerFactory`.

Pour plus d’informations, consultez la documentation de chaque fournisseur. Les fournisseurs de journalisation tiers ne sont pas pris en charge par Microsoft.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:fundamentals/logging/loggermessage>
