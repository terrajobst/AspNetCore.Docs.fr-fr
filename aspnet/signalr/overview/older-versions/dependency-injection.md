---
uid: signalr/overview/older-versions/dependency-injection
title: L’Injection de dépendances dans SignalR 1.x | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 05/15/2013
ms.assetid: eaa206c4-edb3-487e-8fcb-54a3261fed36
msc.legacyurl: /signalr/overview/older-versions/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 905dea4918be731673c39e788069ce2dc78e1649
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910692"
---
<a name="dependency-injection-in-signalr-1x"></a>L’Injection de dépendances dans SignalR 1.x
====================
par [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

L’injection de dépendances consiste à supprimer codées en dur les dépendances entre les objets, ce qui facilite pour remplacer les dépendances d’un objet, soit pour le test (à l’aide d’objets fictifs) ou pour modifier le comportement au moment de l’exécution. Ce didacticiel montre comment effectuer l’injection de dépendances sur les concentrateurs SignalR. Il montre également comment utiliser des conteneurs IoC avec SignalR. Un conteneur IoC est une infrastructure générale pour l’injection de dépendances.

## <a name="what-is-dependency-injection"></a>Quelle est l’Injection de dépendances ?

Ignorer cette section si vous êtes déjà familiarisé avec l’injection de dépendances.

*L’injection de dépendances* (DI) est un modèle où les objets ne sont pas chargés de créer leurs propres dépendances. Voici un exemple simple pour motiver l’injection de dépendances. Supposons que vous avez un objet dont a besoin d’enregistrer des messages. Vous pouvez définir une interface de journalisation :

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Dans votre objet, vous pouvez créer un `ILogger` pour enregistrer des messages :

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Cela fonctionne, mais il n’est pas la meilleure conception. Si vous souhaitez remplacer `FileLogger` avec un autre `ILogger` implémentation, vous devrez modifier `SomeComponent`. Laisse supposer que beaucoup d’autres objets utilisent `FileLogger`, vous devez modifier l’ensemble d'entre eux. Ou si vous décidez d’apporter `FileLogger` un singleton, vous devez également apporter des modifications tout au long de l’application.

Une meilleure approche consiste à « injecter » un `ILogger` dans l’objet, par exemple, en utilisant un argument de constructeur :

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

L’objet n’est pas responsable de la sélection qui `ILogger` à utiliser. Vous pouvez swich `ILogger` implémentations sans modifier les objets qui en dépendent.

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Ce modèle est appelé [l’injection de constructeur](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Un autre modèle est l’injection de setter, où vous définissez la dépendance par une méthode setter ou une propriété.

## <a name="simple-dependency-injection-in-signalr"></a>Injection de dépendances simple dans SignalR

Envisagez de l’application de conversation à partir du didacticiel [bien démarrer avec SignalR](../getting-started/tutorial-getting-started-with-signalr.md). Voici la classe hub à partir de cette application :

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Supposons que vous souhaitez stocker les messages de conversation sur le serveur avant de les envoyer. Vous définissez une interface qui isole cette fonctionnalité et utilisez l’injection de dépendances pour injecter l’interface dans le `ChatHub` classe.

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

Le seul problème est qu’une application SignalR ne crée pas directement les hubs ; SignalR crée pour vous. Par défaut, SignalR attend une classe de concentrateur pour avoir un constructeur sans paramètre. Toutefois, vous pouvez facilement enregistrer une fonction pour créer des instances de concentrateur et utiliser cette fonction pour effectuer l’injection de dépendances. Enregistrez la fonction en appelant **GlobalHost.DependencyResolver.Register**.

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

Maintenant SignalR appelle cette fonction anonyme chaque fois qu’il a besoin créer un `ChatHub` instance.

## <a name="ioc-containers"></a>Conteneurs IoC

Le code précédent est parfait pour les cas simples. Mais vous deviez toujours écrire ceci :

[!code-css[Main](dependency-injection/samples/sample8.css)]

Dans une application complexe avec de nombreuses dépendances, vous devrez peut-être écrire beaucoup de code de cette « connexion ». Ce code peut être difficile à maintenir, surtout si les dépendances sont imbriqués. Il est également difficile de test unitaire.

Une solution consiste à utiliser un conteneur IoC. Un conteneur IoC est un composant logiciel qui est responsable de la gestion des dépendances. Vous inscrivez des types auprès du conteneur et ensuite utilisez le conteneur pour créer des objets. Le conteneur détermine automatiquement les relations de dépendance. De nombreux conteneurs d’inversion de contrôle vous permettent également de vous permettent de contrôler des éléments tels que la durée de vie et la portée.

> [!NOTE]
> « IoC » est l’acronyme « d’inversion de contrôle », qui est un modèle général où un framework appelle du code d’application. Un conteneur IoC construit vos objets, ce qui le flux habituel de contrôle « inverse ».


## <a name="using-ioc-containers-in-signalr"></a>À l’aide de conteneurs IoC dans SignalR

L’application de conversation est probablement trop simple pour bénéficier d’un conteneur IoC. Au lieu de cela, nous allons examiner la [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) exemple.

L’exemple StockTicker définit deux classes principales :

- `StockTickerHub`: La classe de concentrateur, qui gère les connexions clientes.
- `StockTicker`: Un singleton qui conserve des actions et les met à jour régulièrement.

`StockTickerHub` contient une référence à la `StockTicker` singleton, tandis que `StockTicker` contient une référence à la **IHubConnectionContext** pour le `StockTickerHub`. Il utilise cette interface pour communiquer avec `StockTickerHub` instances. (Pour plus d’informations, consultez [diffusion par le serveur avec ASP.NET SignalR](index.md).)

Nous pouvons utiliser un conteneur IoC pour démêler un peu de ces dépendances. Tout d’abord, nous allons simplifier le `StockTickerHub` et `StockTicker` classes. Dans le code suivant, j’ai commenté les parties que nous n’avez pas besoin.

Supprimez le constructeur sans paramètre à partir de `StockTicker`. Au lieu de cela, nous allons toujours utiliser l’injection de dépendances pour créer le hub.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

Pour StockTicker, supprimez l’instance de singleton. Plus tard, nous allons utiliser le conteneur IoC pour contrôler la durée de vie StockTicker. En outre, rendre le constructeur public.

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

Ensuite, nous pouvons refactoriser le code en créant une interface pour `StockTicker`. Nous allons utiliser cette interface pour découpler le `StockTickerHub` à partir de la `StockTicker` classe.

Visual Studio rend facile ce type de refactorisation. Ouvrez le fichier StockTicker.cs, avec le bouton droit sur le `StockTicker` déclaration de classe, puis sélectionnez **refactoriser** ... **Extraire l’Interface**.

![](dependency-injection/_static/image1.png)

Dans le **extraire l’Interface** boîte de dialogue, cliquez sur **sélectionner tout**. Laissez les autres valeurs par défaut. Cliquez sur **OK**.

![](dependency-injection/_static/image2.png)

Visual Studio crée une nouvelle interface nommée `IStockTicker`et change également `StockTicker` de dériver de `IStockTicker`.

Ouvrez le fichier IStockTicker.cs et modifiez l’interface à **public**.

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

Dans le `StockTickerHub` class, modifiez les deux instances de `StockTicker` à `IStockTicker`:

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

Création d’un `IStockTicker` interface n’est pas strictement nécessaire, mais j’ai voulu montrer comment l’injection de dépendances peut aider à réduire le couplage entre les composants dans votre application.

## <a name="add-the-ninject-library"></a>Ajoutez la bibliothèque Ninject

Il existe de nombreux conteneurs d’inversion de contrôle open source pour .NET. Pour ce didacticiel, j’utiliserai [Ninject](http://www.ninject.org/). (Incluent d’autres bibliothèques populaires [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), et [StructureMap ](http://docs.structuremap.net).)

Utilisez le Gestionnaire de Package NuGet pour installer le [Ninject bibliothèque](https://nuget.org/packages/Ninject/3.0.1.10). Dans Visual Studio, à partir de la **outils** menu, sélectionnez **Gestionnaire de Package NuGet** > **Console du Gestionnaire de Package**. Dans la fenêtre de Console du Gestionnaire de Package, entrez la commande suivante :

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a>Remplacez le résolveur de dépendance de SignalR

Pour utiliser Ninject dans SignalR, créez une classe qui dérive de **DefaultDependencyResolver**.

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

Cette classe substitue la **GetService** et **GetServices** méthodes de **DefaultDependencyResolver**. SignalR appelle ces méthodes pour créer les divers objets lors de l’exécution, y compris les instances du hub, ainsi que différents services utilisés en interne par SignalR.

- Le **GetService** méthode crée une instance unique d’un type. Substituez cette méthode pour appeler le noyau Ninject **TryGet** (méthode). Si cette méthode retourne la valeur null, revenir au programme de résolution par défaut.
- Le **GetServices** méthode crée une collection d’objets d’un type spécifié. Substituez cette méthode pour concaténer les résultats à partir de Ninject avec les résultats à partir du programme de résolution par défaut.

## <a name="configure-ninject-bindings"></a>Configurer les liaisons de Ninject

Maintenant, nous allons utiliser Ninject pour déclarer des liaisons de type.

Ouvrez le fichier RegisterHubs.cs. Dans le `RegisterHubs.Start` (méthode), créer le conteneur Ninject, qui appelle Ninject le *noyau*.

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

Créez une instance de notre programme de résolution de dépendance personnalisée :

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

Créer une liaison pour `IStockTicker` comme suit :

[!code-html[Main](dependency-injection/samples/sample17.html)]

Ce code indique que deux choses. Tout d’abord, chaque fois que l’application a besoin un `IStockTicker`, le noyau doit créer une instance de `StockTicker`. Ensuite, le `StockTicker` classe doit-elle être créés en tant qu’objet singleton. Ninject crée une instance de l’objet et retournent la même instance pour chaque demande.

Créer une liaison pour **IHubConnectionContext** comme suit :

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

Ce code de creatres une fonction anonyme qui retourne un **IHubConnection**. Le **WhenInjectedInto** méthode indique à Ninject à utiliser cette fonction uniquement lors de la création `IStockTicker` instances. La raison est que SignalR crée **IHubConnectionContext** instances en interne, et nous ne souhaitons pas substituer comment SignalR les crée. Cette fonction s’applique uniquement à notre `StockTicker` classe.

Passer le résolveur de dépendance dans le **MapHubs** méthode :

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

Maintenant SignalR utilisera l’outil de résolution spécifié dans **MapHubs**, au lieu du résolveur par défaut.

Voici le code complet pour `RegisterHubs.Start`.

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

Pour exécuter l’application StockTicker dans Visual Studio, appuyez sur F5. Dans la fenêtre du navigateur, accédez à `http://localhost:*port*/SignalR.Sample/StockTicker.html`.

![](dependency-injection/_static/image3.png)

L’application a exactement les mêmes fonctionnalités qu’avant. (Pour obtenir une description, consultez [diffusion par le serveur avec ASP.NET SignalR](index.md).) Nous n’avons pas modifié le comportement ; Venez le code plus facile à tester, gérer et faire évoluer.
