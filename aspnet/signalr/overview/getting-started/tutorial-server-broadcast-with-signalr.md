---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Tutoriel : Serveur de diffusion avec SignalR 2 | Microsoft Docs'
author: tdykstra
description: Ce didacticiel montre comment créer une application web qui utilise ASP.NET SignalR 2 pour fournir des fonctionnalités de diffusion de serveur.
ms.author: riande
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: a6014e604613492db91b2dc6f846c3c73d938d99
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099297"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a>Tutoriel : Serveur de diffusion avec SignalR 2

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

Ce didacticiel montre comment créer une application web qui utilise ASP.NET SignalR 2 pour fournir des fonctionnalités de diffusion de serveur. Diffusion de serveur signifie que le serveur démarre les communications envoyées aux clients.

L’application que vous allez créer dans ce didacticiel simule un téléscripteur, un scénario classique pour les fonctionnalités de diffusion de serveur. Périodiquement, le serveur de façon aléatoire met à jour de bourse et diffuser les mises à jour pour tous les clients connectés. Dans le navigateur, des chiffres et des symboles dans le **modifier** et **%** colonnes changent dynamiquement en réponse aux notifications à partir du serveur. Si vous ouvrez des navigateurs supplémentaires à la même URL, elles affichent toutes les mêmes données et les mêmes modifications aux données simultanément.

![Créer le web](tutorial-server-broadcast-with-signalr/_static/image1.png)

Dans ce didacticiel, vous :

> [!div class="checklist"]
> * Créer le projet
> * Configurer le code de serveur
> * Examinez le code de serveur
> * Définissez le code client
> * Examinez le code client
> * Tester l’application
> * Activer la journalisation

> [!IMPORTANT]
> Si vous ne souhaitez pas fonctionner à travers les étapes de génération de l’application, vous pouvez installer le package SignalR.Sample dans un nouveau projet d’Application Web ASP.NET vide. Si vous installez le package NuGet sans effectuer les étapes décrites dans ce didacticiel, vous devez suivre les instructions fournies dans le *readme.txt* fichier. Pour exécuter le package, vous devez ajouter un démarrage OWIN classe qui appelle le `ConfigureSignalR` méthode dans le package installé. Vous recevrez une erreur si vous n’ajoutez pas de la classe de démarrage OWIN. Consultez le [installer l’exemple StockTicker](#install-the-stockticker-sample) section de cet article.


## <a name="prerequisites"></a>Prérequis

 * [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) avec la **ASP.NET et développement web** charge de travail.

## <a name="create-the-project"></a>Créer le projet

Cette section montre comment utiliser Visual Studio 2017 pour créer une Application de Web ASP.NET vide.

1. Dans Visual Studio, créez une Application Web ASP.NET.

    ![Créer le web](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. Dans le **nouvelle Application Web ASP.NET - SignalR.StockTicker** fenêtre, laissez le champ **vide** sélectionné et sélectionnez **OK**.

## <a name="set-up-the-server-code"></a>Configurer le code de serveur

Dans cette section, vous définissez le code qui s’exécute sur le serveur.

### <a name="create-the-stock-class"></a>Créer la classe de Stock

Vous commencez par créer le *Stock* classe que vous utiliserez pour stocker et transmettre des informations concernant une action de modèle.

1. Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **classe**.

1. Nommez la classe *Stock* et ajoutez-le au projet.

1. Remplacez le code dans le *Stock.cs* fichier avec ce code :

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    Les deux propriétés que vous définirez lorsque vous créez des stocks sont `Symbol` (par exemple, MSFT pour Microsoft) et `Price`. Les autres propriétés dépendent comment et quand vous définissez `Price`. La première fois que vous définissez `Price`, la valeur transmise à `DayOpen`. Après cela, lorsque vous définissez `Price`, l’application calcule le `Change` et `PercentChange` les valeurs de propriété basées sur la différence entre `Price` et `DayOpen`.

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a>Créer les classes StockTickerHub et StockTicker

Vous allez utiliser l’API de concentrateur SignalR pour gérer les interactions client-serveur. Un `StockTickerHub` classe qui dérive de la `SignalRHub` classe gérera reçoit des connexions et des appels de méthode à partir de clients. Vous devez également mettre à jour les données de stock et exécuter un `Timer` objet. Le `Timer` objet déclenche périodiquement des mises à jour de prix indépendants des connexions clientes. Vous ne pouvez pas mettre ces fonctions un `Hub` classe, étant donné que les concentrateurs sont temporaires. L’application crée un `Hub` instance de classe pour chaque tâche sur le concentrateur, telles que les connexions et les appels à partir du client au serveur. Par conséquent, le mécanisme qui conserve les données de stock, les prix des mises à jour et diffuse les mises à jour de prix doit s’exécuter dans une classe distincte. Vous allez nommer la classe `StockTicker`.

![Diffusion à partir de StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

Vous souhaitez uniquement une seule instance de la `StockTicker` classe exécutés sur le serveur, vous devez définir une référence à partir de chaque `StockTickerHub` instance au singleton `StockTicker` instance. Le `StockTicker` classe a diffuser aux clients, car il possède les données de stock et déclenche les mises à jour, mais `StockTicker` n’est pas un `Hub` classe. Le `StockTicker` classe a obtenir une référence à l’objet de contexte de connexion de concentrateur SignalR. Il peut ensuite utiliser l’objet de contexte de connexion SignalR pour diffuser aux clients.

#### <a name="create-stocktickerhubcs"></a>Créer StockTickerHub.cs

1. Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **un nouvel élément**.

1. Dans **ajouter un nouvel élément - SignalR.StockTicker**, sélectionnez **installé** > **Visual C#**   >  **Web**  >  **SignalR** , puis sélectionnez **classe de concentrateur SignalR (v2)**.

1. Nommez la classe *StockTickerHub* et ajoutez-le au projet.

    Cette étape crée la *StockTickerHub.cs* fichier de classe. Simultanément, il ajoute un ensemble de fichiers de script et les références d’assembly qui prend en charge SignalR au projet.

1. Remplacez le code dans le *StockTickerHub.cs* fichier avec ce code :

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. Enregistrez le fichier.

L’application utilise le [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) classe pour définir des méthodes, les clients peuvent appeler sur le serveur. Vous définissez une méthode : `GetAllStocks()`. Lorsqu’un client se connecte initialement au serveur, il appellera cette méthode pour obtenir une liste de tous les stocks avec leurs prix. La méthode peut exécuter de façon synchrone et renvoyer `IEnumerable<Stock>` , car elle retourne des données à partir de la mémoire.

Si la méthode devait obtenir les données par une opération qui nécessiterait en attente, comme une recherche de base de données ou un appel de service web, vous devez spécifier `Task<IEnumerable<Stock>>` en tant que la valeur de retour pour permettre un traitement asynchrone. Pour plus d’informations, consultez [Guide de l’API ASP.NET SignalR Hubs - Server - quand exécuter de façon asynchrone](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).

Le `HubName` attribut spécifie la façon dont l’application référencera le Hub dans le code JavaScript sur le client. Le nom par défaut sur le client si vous n’utilisez pas cet attribut, est une version de la casse mixte du nom de classe, qui dans ce cas serait `stockTickerHub`.

Comme vous le verrez plus tard lorsque vous créez le `StockTicker` (classe), l’application crée une instance de singleton de cette classe dans son statique `Instance` propriété. Cette instance singleton de `StockTicker` est en mémoire, quel que soit le nombre de clients connecter ou déconnecter. Cette instance est ce que fait le `GetAllStocks()` méthode utilise pour retourner des informations de stock en cours.

#### <a name="create-stocktickercs"></a>Créer StockTicker.cs

1. Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **classe**.

1. Nommez la classe *StockTicker* et ajoutez-le au projet.

1. Remplacez le code dans le *StockTicker.cs* fichier avec ce code :

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

Étant donné que tous les threads exécutera la même instance de StockTicker code, la classe StockTicker doit être thread-safe.

### <a name="examine-the-server-code"></a>Examinez le code de serveur

Si vous examinez le code du serveur, il sera vous aider à comprendre le fonctionne de l’application.

#### <a name="storing-the-singleton-instance-in-a-static-field"></a>Stockage de l’instance de singleton dans un champ statique

Le code initialise statiques `_instance` champ qui stocke le `Instance` propriété avec une instance de la classe. Étant donné que le constructeur est privé, il est la seule instance de la classe que l’application peut créer. L’application utilise [l’initialisation tardive](/dotnet/framework/performance/lazy-initialization) pour le `_instance` champ. Il n’est pas pour des raisons de performances. Il est de s’assurer que la création de l’instance est thread-safe.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

Chaque fois un client se connecte au serveur, une nouvelle instance de la classe StockTickerHub en cours d’exécution dans un thread distinct Obtient l’instance de singleton StockTicker à partir de la `StockTicker.Instance` propriété statique, comme vous l’avez vu précédemment dans la `StockTickerHub` classe.

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a>Stockage des données de stock dans un ConcurrentDictionary

Le constructeur initialise la `_stocks` collection avec des exemples de données boursières, et `GetAllStocks` retourne les stocks. Comme vous l’avez vu précédemment, cette collection de stocks est retournée par `StockTickerHub.GetAllStocks`, qui est une méthode de serveur dans la `Hub` classe les clients peuvent appeler.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

La collection de stocks est définie comme un [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) type cohérence de thread. Comme alternative, vous pouvez utiliser un [dictionnaire](https://msdn.microsoft.com/library/xfhwa508.aspx) de l’objet et de verrouiller explicitement le dictionnaire lorsque vous apportez des modifications à celui-ci.

Pour cet exemple d’application, il est OK pour stocker les données d’application en mémoire et de perdre les données lors de l’application supprime le `StockTicker` instance. Dans une application réelle, vous fonctionnerait avec un magasin de données back-end comme une base de données.

#### <a name="periodically-updating-stock-prices"></a>Mise à jour périodique des actions

Le constructeur démarre un `Timer` objet qui appelle régulièrement des méthodes qui mettent à jour de bourse sur une base aléatoire.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 `Timer` appels `UpdateStockPrices`, qui passe dans la valeur null dans le paramètre d’état. Avant la mise à jour de prix, l’application prend un verrou sur le `_updateStockPricesLock` objet. Le code vérifie si un autre thread est déjà la mise à jour les prix, et il appelle ensuite `TryUpdateStockPrice` sur chaque action dans la liste. Le `TryUpdateStockPrice` méthode décide s’il faut modifier la cotation et la quantité pour la modifier. Si la cotation change, l’application appelle `BroadcastStockPrice` pour diffuser la modification de la cotation à tous les clients connectés.

Le `_updatingStockPrices` indicateur désigné [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) pour vous assurer qu’il est thread-safe.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

Dans une application réelle, le `TryUpdateStockPrice` méthode appelez un service web pour rechercher le prix. Dans ce code, l’application utilise un générateur de nombres aléatoires pour apporter des modifications de façon aléatoire.

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a>Obtention du contexte de SignalR afin que la classe StockTicker peut diffuser vers les clients

Étant donné que les modifications de prix sont originaires ici le `StockTicker` de l’objet, c’est l’objet qui doit appeler une `updateStockPrice` (méthode) sur tous les clients connectés. Dans un `Hub` (classe), vous avez une API pour appeler les méthodes du client, mais `StockTicker` ne dérive pas de la `Hub` classe et n’a pas une référence à tout `Hub` objet. Diffusion vers les clients connectés, le `StockTicker` classe a obtenir l’instance de contexte de SignalR pour la `StockTickerHub` classe et l’utiliser pour appeler des méthodes sur les clients.

Le code obtient une référence au contexte de SignalR lorsqu’il crée l’instance de la classe singleton passes qui font référence au constructeur, puis le constructeur de la `Clients` propriété.

Il existe deux raisons raison pour laquelle vous souhaitez obtenir le contexte qu’une seule fois : obtention du contexte est une tâche coûteuse et le faire une fois garantit que l’application conserve l’ordre prévu des messages envoyés aux clients.

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

Obtention de la `Clients` propriété du contexte et de sa mise en le `StockTickerClient` propriété vous permet d’écrire du code pour appeler des méthodes de client qui a le même aspect comme il le ferait dans un `Hub` classe. Par exemple, pour une diffusion à tous les clients, vous pouvez écrire `Clients.All.updateStockPrice(stock)`.

Le `updateStockPrice` méthode que vous appelez dans `BroadcastStockPrice` n’existe pas encore. Vous allez l’ajouter ultérieurement lorsque vous écrivez du code qui s’exécute sur le client. Vous pouvez faire référence à `updateStockPrice` ici, car `Clients.All` est dynamique, ce qui signifie que l’application évalue l’expression lors de l’exécution. Lorsque cet appel de méthode s’exécute, SignalR envoie le nom de méthode et la valeur du paramètre du client, et si le client possède une méthode nommée `updateStockPrice`, l’application appelle cette méthode et lui passer la valeur du paramètre.

`Clients.All` signifie envoyer à tous les clients. SignalR vous donne d’autres options pour spécifier quels clients ou les groupes de clients à envoyer à. Pour plus d’informations, consultez [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).

### <a name="register-the-signalr-route"></a>Inscrire l’itinéraire de SignalR

Le serveur doit connaître l’URL à intercepter et diriger vers SignalR. Pour ce faire, ajoutez une classe de démarrage OWIN :

1. Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **un nouvel élément**.

1. Dans **ajouter un nouvel élément - SignalR.StockTicker** sélectionnez **installé** > **Visual C#**   >  **Web** et puis sélectionnez **classe de démarrage OWIN**.

1. Nommez la classe *démarrage* et sélectionnez **OK**.

1. Remplacez le code par défaut dans le *Startup.cs* fichier avec ce code :

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

Vous avez terminé de paramétrer le code du serveur. Dans la section suivante, vous allez configurer le client.

## <a name="set-up-the-client-code"></a>Définissez le code client

Dans cette section, vous définissez le code qui s’exécute sur le client.

### <a name="create-the-html-page-and-javascript-file"></a>Créer la page HTML et un fichier JavaScript

La page HTML affiche les données et le fichier JavaScript s’organiser les données.

#### <a name="create-stocktickerhtml"></a>Créer StockTicker.html

Tout d’abord, vous allez ajouter le client HTML.

1. Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **HTML Page**.

1. Nommez le fichier *StockTicker* et sélectionnez **OK**.

1. Remplacez le code par défaut dans le *StockTicker.html* fichier avec ce code :

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    Le code HTML crée une table avec cinq colonnes, une ligne d’en-tête et une ligne de données avec une seule cellule qui s’étend sur toutes les cinq colonnes. La ligne de données affiche « chargement... » momentanément lorsque l’application démarre. Code JavaScript sera supprimer cette ligne et ajoutez dans ses lignes sur place avec des données boursières extraites du serveur.

    Les balises de script spécifient :

    * Le fichier de script jQuery.

    * Le fichier de script SignalR core.

    * Le fichier de script de proxys de SignalR.

    * Un fichier de script StockTicker que vous créerez ultérieurement.

    L’application génère dynamiquement le fichier de script de proxys de SignalR. Il spécifie l’URL « / signalr hubs » et définit les méthodes de proxy pour les méthodes sur la classe de concentrateur, dans ce cas, pour `StockTickerHub.GetAllStocks`. Si vous préférez, vous pouvez générer manuellement ce fichier JavaScript à l’aide de [SignalR utilitaires](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/). N’oubliez pas de désactiver la création de fichier dynamique dans le `MapHubs` appel de méthode.

1. Dans **l’Explorateur de solutions**, développez **Scripts**.

    Bibliothèques de scripts pour jQuery et SignalR sont visibles dans le projet.

    > [!IMPORTANT]
    > Le Gestionnaire de package installe une version ultérieure des scripts SignalR.

1. Mettre à jour les références de script dans le bloc de code pour qu’elles correspondent aux versions des fichiers de script dans le projet.

1. Dans **l’Explorateur de solutions**, avec le bouton droit *StockTicker.html*, puis sélectionnez **définir comme Page de démarrage**.

#### <a name="create-stocktickerjs"></a>Créer StockTicker.js

Créez maintenant le fichier JavaScript.

1. Dans **l’Explorateur de solutions**, cliquez sur le projet et sélectionnez **ajouter** > **fichier JavaScript**.

1. Nommez le fichier *StockTicker* et sélectionnez **OK**.

1. Ajoutez ce code à la *StockTicker.js* fichier :

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a>Examinez le code client

Si vous examinez le code client, il vous permettra de savoir comment le code client interagit avec le code du serveur pour que l’application fonctionne.

#### <a name="starting-the-connection"></a>Démarrage de la connexion

`$.connection` fait référence aux proxys SignalR. Le code obtient une référence au proxy pour le `StockTickerHub` classe et le place dans le `ticker` variable. Le nom de proxy est celui qui a été défini par le `HubName` attribut :

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

Après avoir défini toutes les variables et fonctions, la dernière ligne de code dans le fichier initialise la connexion SignalR en appelant SignalR `start` (fonction). Le `start` fonction exécute de façon asynchrone et retourne un [jQuery différé objet](http://api.jquery.com/category/deferred-object/). Vous pouvez appeler la fonction terminée pour spécifier la fonction à appeler lorsque l’application termine l’action asynchrone.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a>Obtention de tous les stocks

Le `init` appels de fonction le `getAllStocks` fonction sur le serveur et utilise les informations retournés par le serveur pour mettre à jour la table de stock. Notez que, par défaut, vous devez utiliser camelCasing sur le client, même si le nom de la méthode est sur le serveur de la casse pascal. La règle de casse mixte s’applique uniquement aux méthodes, pas les objets. Par exemple, vous faites référence à `stock.Symbol` et `stock.Price`, et non `stock.symbol` ou `stock.price`.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

Dans le `init` (méthode), l’application crée le HTML pour une ligne de table pour chaque objet de stock reçu à partir du serveur en appelant `formatStock` aux propriétés de format de la `stock` de l’objet, puis en appelant `supplant` pour remplacer les espaces réservés dans le `rowTemplate` variable avec le `stock` les valeurs de propriété de l’objet. Le code HTML résultant est ensuite ajouté à la table de stock.

> [!NOTE]
> Vous appelez `init` en la passant comme un `callback` fonction qui s’exécute après asynchrone `start` fonction se termine. Si vous avez appelé `init` comme une instruction JavaScript séparée après avoir appelé `start`, la fonction échoue, car il est exécuté immédiatement sans attendre que la fonction de démarrage à la fin de l’établissement de la connexion. Dans ce cas, le `init` fonction essaie d’appeler le `getAllStocks` fonctionner avant que l’application établit une connexion au serveur.

#### <a name="getting-updated-stock-prices"></a>Obtention des mises à jour de bourse

Lorsque le serveur modifie les prix d’une action, il appelle le `updateStockPrice` sur les clients connectés. L’application ajoute la fonction à la propriété de client de la `stockTicker` proxy pour le rendre disponible aux appels à partir du serveur.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

Le `updateStockPrice` un objet stock reçu à partir du serveur dans une table de ligne de la même façon que dans des formats de fonction le `init` (fonction). Au lieu de l’ajout de la ligne à la table, il recherche la ligne actuelle de l’action dans la table et remplace cette ligne par le nouveau.

## <a name="test-the-application"></a>Tester l’application

Vous pouvez tester l’application pour vous assurer qu’elle fonctionne. Vous verrez toutes les fenêtres de navigateur et afficher la table de stock en direct avec des actions fluctue.

1. Dans la barre d’outils, activez **le débogage de Script** , puis sélectionnez le bouton lecture pour exécuter l’application en mode débogage.

    ![Capture d’écran de l’utilisateur sous tension en sélectionnant play et de débogage.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    Une fenêtre de navigateur s’ouvre avec le **Live la Table de Stock**. La table stock affiche initialement la ligne « chargement... », puis, après une courte période, l’application affiche les données de stock initiales et commencez à modifier la bourse.

1. Copiez l’URL à partir du navigateur, ouvrez les deux autres navigateurs et collez l’URL dans la barre d’adresse.

    L’affichage initial de stock est le même que le premier navigateur et les modifications se produisent simultanément.

1. Fermez tous les navigateurs, ouvrez une nouvelle fenêtre de navigateur et accédez à la même URL.

    L’objet de singleton StockTicker continué à fonctionner dans le serveur. Le **Live la Table de Stock** montre que les stocks ont continué à modifier. Vous ne voyez pas la table initiale avec zéro modifier des chiffres.

1. Fermez le navigateur.

## <a name="enable-logging"></a>Activer la journalisation

SignalR possède une fonction de journalisation intégrés que vous pouvez activer sur le client pour faciliter le dépannage. Dans cette section, vous activez la journalisation et voir des exemples qui montrent comment journaux vous indiquer parmi les méthodes de transport suivants à l’aide de SignalR :

* [WebSockets](http://en.wikipedia.org/wiki/WebSocket), pris en charge par IIS 8 et les navigateurs actuels.

* [Événements de serveur envoyé](http://en.wikipedia.org/wiki/Server-sent_events), pris en charge par les navigateurs autres qu’Internet Explorer.

* [Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), pris en charge par Internet Explorer.

* [AJAX interrogation longue](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), pris en charge par tous les navigateurs.

Pour une connexion donnée, SignalR choisit la meilleure méthode de transport qui prennent en charge par le serveur et le client.

1. Ouvrez *StockTicker.js*.

1. Ajoutez la ligne en surbrillance du code pour activer la journalisation immédiatement avant le code qui initialise la connexion à la fin du fichier :

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. Appuyez sur **F5** pour exécuter le projet.

1. Ouvrez la fenêtre Outils de développement de votre navigateur, puis sélectionnez la Console pour afficher les journaux. Vous devrez peut-être actualiser la page pour afficher les journaux de SignalR négocier la méthode de transport pour une nouvelle connexion.

    * Si vous exécutez Internet Explorer 10 sur Windows 8 (IIS 8), la méthode de transport est **WebSockets**.

    * Si vous exécutez Internet Explorer 10 sur Windows 7 (IIS 7.5), la méthode de transport est **iframe**.

    * Si vous utilisez Firefox 19 sur Windows 8 (IIS 8), la méthode de transport est **WebSockets**.

        > [!TIP]
        > Dans Firefox, installez le complément Firebug pour obtenir une fenêtre de Console.

    * Si vous utilisez Firefox 19 sur Windows 7 (IIS 7.5), la méthode de transport est **envoyées au serveur** événements.

## <a name="install-the-stockticker-sample"></a>Installer l’exemple StockTicker

Le [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installe l’application StockTicker. Le package NuGet inclut plus de fonctionnalités que la version simplifiée que vous avez créé à partir de zéro. Dans cette section du didacticiel, vous installez le package NuGet et passez en revue les nouvelles fonctionnalités et le code qui implémente les.

> [!IMPORTANT]
> Si vous installez le package sans effectuer les étapes précédentes de ce didacticiel, vous devez ajouter une classe de démarrage OWIN à votre projet. Ce fichier readme.txt pour le package NuGet explique cette étape.

### <a name="install-the-signalrsample-nuget-package"></a>Installez le package NuGet de SignalR.Sample

1. Dans **l’Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Gérer les packages NuGet**.

1. Dans **le Gestionnaire de NuGet Package : SignalR.StockTicker**, sélectionnez **Parcourir**.

1. À partir de **source du Package**, sélectionnez **nuget.org**.

1. Entrez *SignalR.Sample* dans la zone de recherche, puis sélectionnez **Microsoft.AspNet.SignalR.Sample** > **installer**.

1. Dans **l’Explorateur de solutions**, développez le *SignalR.Sample* dossier.

    Installation du package SignalR.Sample créé le dossier et son contenu.

1. Dans le *SignalR.Sample* dossier, avec le bouton droit *StockTicker.html*, puis sélectionnez **définir comme Page de démarrage**.

    > [!NOTE]
    > Installation de NuGet de SignalR.Sample le package peut changer la version de jQuery dont vous disposez dans votre *Scripts* dossier. La nouvelle *StockTicker.html* fichier que le package installe dans le *SignalR.Sample* dossier sera synchronisée avec la version de jQuery que le package installe, mais si vous souhaitez exécuter votre original *StockTicker.html* fichier à nouveau, vous devrez peut-être mettre à jour la référence de jQuery dans la balise de script tout d’abord.

### <a name="run-the-application"></a>Exécuter l'application

 La table que vous avez vu dans la première application comportait des fonctionnalités utiles. L’application de cotations boursières complète montre les nouvelles fonctionnalités : une fenêtre de défilement horizontal qui affiche les données de stock et les stocks qui changent de couleur lorsqu’ils augmentent et se situent.

1. Appuyez sur **F5** pour exécuter l'application.

     Lorsque vous exécutez l’application pour la première fois, le « marché » est « fermé » et vous voyez un tableau statique et une fenêtre de code qui n’est pas le défilement.

1. Sélectionnez **Open Market**.

    ![Capture d’écran du téléscripteur en direct.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * Le **Live le Téléscripteur** boîte commence à faire défiler horizontalement, et le serveur commence à diffuser régulièrement les modifications de prix de l’action de manière aléatoire.

    * Chaque fois qu’une cotation change, l’application met à jour à la fois le **Live la Table de Stock** et **téléscripteur de Live**.

    * Lorsque le changement de prix d’une action est un nombre positif, l’application affiche le stock avec un arrière-plan vert.

    * Lorsque la modification est négative, l’application affiche le stock avec un arrière-plan rouge.

1. Sélectionnez **fermer marché**.

    * Le tableau met à jour les mots vides.

    * Le téléscripteur arrête le défilement.

1. Sélectionnez **réinitialiser**.

    * Toutes les données de stock est réinitialisé.

    * L’application restaure l’état initial avant de démarrer les modifications de prix.

1. Copiez l’URL à partir du navigateur, ouvrez les deux autres navigateurs et collez l’URL dans la barre d’adresse.

1. Vous voyez les mêmes données mis à jour dynamiquement en même temps dans chaque navigateur.

1. Lorsque vous sélectionnez un des contrôles, tous les navigateurs répondent de la même manière en même temps.

### <a name="live-stock-ticker-display"></a>Affichage de téléscripteur dynamique

Le **téléscripteur de Live** affichage est une liste non triée dans un `<div>` élément mis en forme en une seule ligne par les styles CSS. L’application initialise et met à jour le téléscripteur de la même façon que la table : en remplaçant les espaces réservés dans une `<li>` chaîne de modèle et ajouter dynamiquement le `<li>` éléments à la `<ul>` élément. L’application inclut le défilement à l’aide de le jQuery `animate` (fonction) pour faire varier la marge gauche de la liste non triée dans le `<div>`.

#### <a name="signalrsample-stocktickerhtml"></a>SignalR.Sample StockTicker.html

La code HTML de cotations boursières :

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a>SignalR.Sample StockTicker.css

La code CSS de cotations boursières :

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a>SignalR.Sample SignalR.StockTicker.js

Le code jQuery qui permet de faire défiler :

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a>Sur le serveur que le client peut appeler des méthodes supplémentaires

Pour ajouter de la flexibilité à l’application, il existe de l’application peut appeler des méthodes supplémentaires.

#### <a name="signalrsample-stocktickerhubcs"></a>SignalR.Sample StockTickerHub.cs

Le `StockTickerHub` classe définit quatre méthodes supplémentaires que le client peut appeler :

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

Les appels de l’application `OpenMarket`, `CloseMarket`, et `Reset` en réponse aux boutons en haut de la page. Elles illustrent le modèle d’un seul client déclenchant un changement d’état immédiatement propagée à tous les clients. Chacune de ces méthodes appelle une méthode la `StockTicker` classe qui provoque le changement d’état sur le marché et diffuse ensuite le nouvel état.

#### <a name="signalrsample-stocktickercs"></a>SignalR.Sample StockTicker.cs

Dans le `StockTicker` (classe), l’application gère l’état du marché avec un `MarketState` propriété qui retourne un `MarketState` valeur enum :

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

Chacune des méthodes qui modifient l’état de mise sur le marché le faire à l’intérieur d’un bloc de verrous, car la `StockTicker` classe doit être thread-safe :

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

S’assurer que ce code est thread-safe, le `_marketState` champ qui stocke le `MarketState` propriété désignée `volatile`:

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

Le `BroadcastMarketStateChange` et `BroadcastMarketReset` méthodes sont similaires à la méthode BroadcastStockPrice que vous avez déjà vu, à ceci près qu’ils appellent des méthodes différentes définies au niveau du client :

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a>Sur le client que le serveur peut appeler des fonctions supplémentaires

Le `updateStockPrice` fonction gère désormais le tableau et l’affichage de téléscripteur, et il utilise `jQuery.Color` pour flasher couleurs rouges et vertes.

Nouvelles fonctions dans *SignalR.StockTicker.js* activer et désactiver les boutons en fonction de l’état de mise sur le marché. Ils également arrêter ou démarrer le **Live le Téléscripteur** un défilement horizontal. Dans la mesure où de nombreuses fonctions sont ajoutées à `ticker.client`, l’application utilise le [jQuery étendre la fonction](http://api.jquery.com/jQuery.extend/) pour les ajouter.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a>Programme d’installation de client supplémentaires après avoir établi la connexion

Une fois que le client établit la connexion, il a un travail supplémentaire à effectuer :

* Déterminer si le marché est ouvert ou fermé à appeler approprié `marketOpened` ou `marketClosed` (fonction).

* Attachez les appels de méthode de serveur pour les boutons.

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

Les méthodes de serveur liés ne sont pas les boutons jusqu'à une fois que l’application établit la connexion. Il est donc le code ne peut pas appeler les méthodes de serveur avant qu’ils sont disponibles.

## <a name="additional-resources"></a>Ressources supplémentaires

Dans ce didacticiel, vous avez appris comment programmer une application SignalR qui diffuse des messages à partir du serveur à tous les clients connectés. Maintenant vous pouvez diffuser des messages de manière périodique et en réponse aux notifications à partir de n’importe quel client. Vous pouvez utiliser le concept de l’instance de singleton multithread pour maintenir l’état de serveur dans des scénarios multijoueurs de jeux en ligne. Pour obtenir un exemple, consultez [le jeu ShootR selon SignalR](https://github.com/NTaylorMullen/ShootR).

Pour des didacticiels qui montrent des scénarios de communication d’égal à égal, consultez [bien démarrer avec SignalR](introduction-to-signalr.md) et [la mise à jour d’en temps réel avec SignalR](tutorial-high-frequency-realtime-with-signalr.md).

Pour en savoir plus sur SignalR, consultez les ressources suivantes :

* [ASP.NET SignalR](../../index.md)
* [Projet de SignalR](http://signalr.net/)
* [SignalR GitHub et exemples](https://github.com/SignalR/SignalR)
* [Wiki de SignalR](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous :

> [!div class="checklist"]
> * Créé le projet
> * Configurer le code de serveur
> * Examiner le code du serveur
> * Définissez le code client
> * Examiner le code client
> * Test de l’application
> * Journalisation est activée

Passez à l’article suivant pour apprendre à créer une application web en temps réel qui utilise ASP.NET SignalR 2.
> [!div class="nextstepaction"]
> [Créer l’application web en temps réel avec SignalR](real-time-web-applications-with-signalr.md)