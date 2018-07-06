---
uid: web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
title: ASP.NET Web Forms la résilience des connexions et Interception des commandes | Microsoft Docs
author: Erikre
description: Ce didacticiel décrit comment modifier un exemple d’application pour prendre en charge la résilience des connexions et interception des commandes.
ms.author: aspnetcontent
ms.date: 03/31/2014
ms.assetid: 6d497001-fa80-4765-b4cc-181fe90b894e
msc.legacyurl: /web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
msc.type: authoredcontent
ms.openlocfilehash: 9a66b297608a5a8cd536b9af2a9ae4bb600a6bbb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807087"
---
<a name="aspnet-web-forms-connection-resiliency-and-command-interception"></a>ASP.NET Web Forms la résilience des connexions et Interception des commandes
====================
par [Erik Reitan](https://github.com/Erikre)

Dans ce didacticiel, vous allez modifier l’exemple d’application Wingtip Toys pour prendre en charge la résilience des connexions et interception des commandes. En activant la résilience des connexions, l’exemple d’application Wingtip Toys retente automatiquement les appels de données lorsque des erreurs temporaires typiques d’un environnement de cloud se produisent. En outre, en implémentant l’interception des commandes, l’exemple d’application Wingtip Toys intercepte toutes les requêtes SQL envoyées à la base de données afin de se connecter ou les modifier.

> [!NOTE] 
> 
> Ce didacticiel de Web Forms a été basé sur didacticiel MVC de Tom Dykstra suivant :  
> [Résilience des connexions et Interception des commandes avec Entity Framework dans une Application ASP.NET MVC](../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)


## <a name="what-youll-learn"></a>Ce que vous allez apprendre :

- Comment assurer la résilience de connexion.
- Comment implémenter l’interception des commandes.

## <a name="prerequisites"></a>Prérequis

Avant de commencer, assurez-vous que vous avez installé sur votre ordinateur les logiciels suivants :

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) ou [Microsoft Visual Studio Express 2013 pour le Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Le .NET Framework est installé automatiquement.
- Le Wingtip Toys exemple de projet, afin que vous pouvez implémenter les fonctionnalités mentionnées dans ce didacticiel dans le projet de Wingtip Toys. Le lien suivant fournit des détails du téléchargement :

    - [Mise en route avec ASP.NET 4.5.1 Web Forms - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) (c#)
- Avant la fin de ce didacticiel, pour passer en revue la série de didacticiels associée, [bien démarrer avec Web Forms ASP.NET 4.5 et Visual Studio 2013](../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). La série de didacticiels vous aideront à vous familiariser avec la **WingtipToys** projet et code.

## <a name="connection-resiliency"></a>Résilience de la connexion

Lorsque vous envisagez de déployer une application dans Windows Azure, une option à envisager consiste à déployer la base de données **Windows** **base de données SQL Azure**, un service de base de données cloud. Erreurs de connexion temporaires sont généralement plus fréquents lorsque vous vous connectez à un service de base de données de cloud que lorsque votre serveur web et votre serveur de base de données sont directement interconnectés dans le même centre de données. Même si un serveur web de cloud et un service de base de données de cloud sont hébergés dans le même centre de données, il existe plusieurs connexions réseau entre eux qui peuvent avoir des problèmes, tels que les équilibreurs de charge.

Un service cloud est également généralement partagé par d’autres utilisateurs, ce qui signifie que sa réactivité peut être affectée par ces derniers. Et votre accès à la base de données peut être soumis à la limitation. La limitation signifie que le service de base de données lève des exceptions lorsque vous essayez d’y accéder plus fréquemment qu’il est autorisé dans votre *contrat de niveau de Service* (SLA).

Nombre ou la plupart des problèmes de connexion qui se produisent lorsque vous accédez à un service cloud sont temporaires, autrement dit, ils résolvent d’eux-mêmes dans une courte période de temps. Par conséquent, lorsque vous tentez une opération de base de données et obtenez un type d’erreur qui est généralement temporaire, vous pouvez essayer à nouveau l’opération après qu’un court délai d’attente et l’opération est peut-être réussie. Vous pouvez fournir une expérience nettement meilleure pour vos utilisateurs, si vous gérez des erreurs temporaires en réessayant d’automatiquement, en rendant la plupart d'entre eux invisible au client. La fonctionnalité de résilience de connexion dans Entity Framework 6 automatise qu’Échec du processus de nouvelle tentative de requêtes SQL.

La fonctionnalité de résilience de connexion doit être correctement configurée pour un service de base de données particulière :

1. Il doit connaître les exceptions qui sont susceptibles d’être temporaire. Voulez-vous réessayer d’erreurs provoquées par une perte temporaire de connectivité réseau, pas les erreurs provoquées par des bogues de programme, par exemple.
2. Il doit attendre une quantité appropriée de temps entre les tentatives d’une opération ayant échoué. Vous pouvez attendre de plus entre les nouvelles tentatives pour un traitement par lots que vous pouvez le faire pour une page web en ligne où un utilisateur est en attente d’une réponse.
3. Il doit réessayer un nombre approprié de fois avant d’abandonner. Vous souhaiterez peut-être plusieurs nouvelles tentatives dans un traitement par lots que vous le feriez dans une application en ligne.

Vous pouvez configurer ces paramètres manuellement pour n’importe quel environnement de base de données pris en charge par un fournisseur Entity Framework.

Il vous suffit pour activer la résilience des connexions est de créer une classe dans votre assembly qui dérive de la `DbConfiguration` classe et, dans cette classe, définissez la stratégie d’exécution de base de données SQL, qui est un autre terme pour la stratégie de nouvelle tentative dans Entity Framework.

### <a name="implementing-connection-resiliency"></a>Implémentation de la résilience des connexions

1. Téléchargez et ouvrez le [WingtipToys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) exemple d’application Web Forms dans Visual Studio.
2. Dans le *logique* dossier de la **WingtipToys** application, ajoutez un fichier de classe nommé *WingtipToysConfiguration.cs*.
3. Remplacez le code existant par celui-ci :  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample1.cs)]

Entity Framework s’exécute automatiquement le code qu’il trouve dans une classe qui dérive de `DbConfiguration`. Vous pouvez utiliser la `DbConfiguration` classe pour effectuer des tâches de configuration dans le code que vous le feriez dans le cas contraire dans le *Web.config* fichier. Pour plus d’informations, consultez [EntityFramework Configuration basée sur le Code](https://msdn.microsoft.com/data/jj680699).

1. Dans le *logique* dossier, ouvrez le *AddProducts.cs* fichier.
2. Ajouter un `using` instruction pour `System.Data.Entity.Infrastructure` comme indiqué en surbrillance en jaune :  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample2.cs?highlight=6)]
3. Ajouter un `catch` bloquer à la `AddProduct` méthode afin que le `RetryLimitExceededException` est enregistré comme mis en surbrillance en jaune :   

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample3.cs?highlight=14-15,17-22)]

En ajoutant le `RetryLimitExceededException` exception, vous pouvez fournir une meilleure journalisation ou afficher un message d’erreur à l’utilisateur où ils peuvent choisir pour recommencer le processus. En interceptant le `RetryLimitExceededException` exception, seules les erreurs susceptibles d’être temporaire déjà être essayée et échouée plusieurs fois. L’exception réelle retournée sera encapsulée dans le `RetryLimitExceededException` exception. En outre, vous avez également ajouté un bloc catch général. Pour plus d’informations sur la `RetryLimitExceededException` exception, consultez [Entity Framework la résilience des connexions / logique de nouvelle tentative](https://msdn.microsoft.com/data/dn456835).

## <a name="command-interception"></a>Interception des commandes

Maintenant que vous avez activé une stratégie de nouvelle tentative, comment tester pour vérifier qu’il fonctionne comme prévu ? Il n’est pas si facile forcer une erreur temporaire se produire, en particulier lorsque vous exécutez localement, et il serait particulièrement difficile à intégrer des erreurs temporaires réels dans un test unitaire automatisé. Pour tester la fonctionnalité de résilience de connexion, vous avez besoin d’un moyen d’intercepter les requêtes Entity Framework envoie à SQL Server et remplacez la réponse de SQL Server avec un type d’exception qui est généralement temporaire.

Vous pouvez également utiliser l’interception de requête pour mettre en œuvre une meilleure pratique pour les applications cloud : journal de la latence et la réussite ou l’échec de tous les appels externe à des services tels que les services de base de données.

Dans cette section du didacticiel, vous allez utiliser Entity Framework [ *fonction d’interception* ](https://msdn.microsoft.com/data/dn469464) à la fois pour la journalisation et pour simuler des erreurs temporaires.

### <a name="create-a-logging-interface-and-class"></a>Créer une interface de journalisation et de la classe

Recommandé pour la journalisation est de le faire à l’aide un [ `interface` ](https://msdn.microsoft.com/library/ms173156.aspx) au lieu de coder en dur les appels à `System.Diagnostics.Trace` ou une classe de journalisation. Cela facilite la modifier ultérieurement votre mécanisme de journalisation si vous avez besoin pour ce faire. Par conséquent, dans cette section, vous allez créer l’interface de journalisation et d’une classe pour l’implémenter.

Selon la procédure ci-dessus, vous avez téléchargé et ouvert le **WingtipToys** exemple d’application dans Visual Studio.

1. Créer un dossier dans le **WingtipToys** de projet et nommez-le *journalisation*.
2. Dans le *journalisation* dossier, créez un fichier de classe nommé *ILogger.cs* et remplacez le code par défaut par le code suivant :  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample4.cs)]

   L’interface fournit trois niveaux de suivi pour indiquer l’importance relative des journaux et l’autre conçu pour fournir des informations de latence pour les appels de service externe telles que les requêtes de base de données. Les méthodes de journalisation ont des surcharges qui vous permettent de passer d’une exception. Il s’agit afin que les informations sur les exceptions, y compris stack trace et les exceptions internes sont fiable enregistrées par la classe qui implémente l’interface, au lieu de compter sur une fois l’opération effectuée dans chaque appel de méthode de journalisation dans toute l’application.  
  
   Le `TraceApi` méthodes permettent d’effectuer le suivi de la latence de chaque appel à un service externe, comme SQL Database.
3. Dans le *journalisation* dossier, créez un fichier de classe nommé *Logger.cs* et remplacez le code par défaut par le code suivant :  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample5.cs)]

L’implémentation utilise `System.Diagnostics` pour effectuer le suivi. Il s’agit d’une fonctionnalité intégrée de .NET qui permet de facilement générer et utiliser les informations de traçage. Il existe de nombreux &quot;écouteurs&quot; vous pouvez utiliser avec `System.Diagnostics` le suivi, d’écrire des journaux aux fichiers, par exemple, ou de les écrire dans le stockage d’objets blob Windows Azure. Certaines des options et des liens vers d’autres ressources pour plus d’informations, consultez dans [dépannage de Windows Azure Web Sites, dans Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Pour ce didacticiel, vous allez uniquement examiner les journaux dans Visual Studio **sortie** fenêtre.

Dans une application de production, vous souhaiterez envisager d’utiliser les infrastructures de suivi autre que `System.Diagnostics`et le `ILogger` interface facilite relativement basculer vers un mécanisme de suivi différent si vous décidez de le faire.

### <a name="create-interceptor-classes"></a>Créer des classes de l’intercepteur

Ensuite, vous allez créer les classes Entity Framework appellera chaque fois qu’il va envoyer une requête à la base de données : une pour simuler des erreurs temporaires et pour effectuer la journalisation. Ces classes de l’intercepteur doivent dériver de la `DbCommandInterceptor` classe. Dans, vous écrivez des substitutions de méthode qui sont appelées automatiquement lorsque la requête est prête à être exécutée. Dans ces méthodes, vous pouvez examiner ou la requête qui est envoyée à la base de données de journal, et vous pouvez modifier la requête avant leur envoi à la base de données ou renvoyer quelque chose à Entity Framework vous-même sans même passer la requête à la base de données.

1. Pour créer la classe de l’intercepteur qui enregistrera toutes les requêtes SQL avant leur envoi à la base de données, créez un fichier de classe nommé *InterceptorLogging.cs* dans le *logique* dossier et remplacez la valeur par défaut de code avec le code suivant :  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample6.cs)]

   Pour les requêtes ont abouti ou les commandes, ce code écrit un journal d’informations avec les informations de latence. Pour les exceptions, il crée un journal des erreurs.
2. Pour créer la classe de l’intercepteur qui génère des erreurs temporaires factices lorsque vous entrez &quot;lever&quot; dans le **nom** zone de texte sur la page nommée *AdminPage.aspx*, créez une classe fichier nommé *InterceptorTransientErrors.cs* dans le *logique* dossier et remplacez la valeur par défaut de code avec le code suivant :  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample7.cs)]

    Ce code substitue uniquement le `ReaderExecuting` (méthode), qui est appelée pour les requêtes qui peuvent retourner plusieurs lignes de données. Si vous souhaitez vérifier la résilience des connexions pour les autres types de requêtes, vous pouvez également substituer la `NonQueryExecuting` et `ScalarExecuting` méthodes, comme l’intercepteur de journalisation fait.  
  
   Plus tard, vous connecter en tant que « Administrateur » et sélectionnez le **administrateur** lien dans la barre de navigation supérieure. Ensuite, dans le *AdminPage.aspx* page, vous allez ajouter un produit nommé &quot;lever&quot;. Le code crée une exception de base de données SQL factice pour le numéro d’erreur 20, un type connu pour être généralement temporaire. Autres numéros d’erreur actuellement reconnus comme provisoire sont 64 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 et 40613, mais ceux-ci sont susceptibles de changer dans les nouvelles versions de base de données SQL. Le produit sera renommé en « TransientErrorExample », vous pouvez suivre dans le code de la *InterceptorTransientErrors.cs* fichier.  
  
   Le code retourne l’exception à Entity Framework au lieu d’exécuter la requête et en passant les résultats en retour. L’exception transitoire est retournée *quatre* fois, et reprend ensuite le code à la procédure normale de transmission de la requête à la base de données.

    Étant donné que tout est connecté, vous serez en mesure de voir que Entity Framework essaie d’exécuter la requête quatre fois avant de réussir et la seule différence dans l’application est qu’il prend plus de temps pour afficher une page avec les résultats de la requête.  
  
   Le nombre de tentatives de Entity Framework est configurable ; le code spécifie quatre fois car il s’agit de la valeur par défaut pour la stratégie d’exécution de base de données SQL. Si vous modifiez la stratégie d’exécution, il fallait également modifier le code qui spécifie le nombre de fois où les erreurs temporaires sont générés. Vous pouvez également modifier le code pour générer des exceptions plus afin que Entity Framework lèvera le `RetryLimitExceededException` exception.
3. Dans *Global.asax*, ajoutez le code suivant à l’aide d’instructions :  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample8.cs)]
4. Ensuite, ajoutez les lignes en surbrillance à la `Application_Start` méthode :  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample9.cs?highlight=17-20)]

Ces lignes de code sont la cause de votre code de l’intercepteur à exécuter lors de l’Entity Framework envoie des requêtes à la base de données. Notez que parce que vous avez créé des classes de l’intercepteur distinct pour la simulation d’une erreur temporaire et la journalisation, vous pouvez indépendamment activer et désactiver les.   
  
 Vous pouvez ajouter des intercepteurs d’à l’aide de la `DbInterception.Add` méthode n’importe où dans votre code ; pas nécessairement se trouver dans le `Application_Start` (méthode). Une autre option, si vous n’avez pas ajouté les intercepteurs dans le `Application_Start` méthode, consisterait à mettre à jour ou ajoutez la classe nommée *WingtipToysConfiguration.cs* et placez le code ci-dessus à la fin du constructeur de la `WingtipToysbConfiguration` classe.

Partout où vous placez ce code, être veillez à ne pas exécuter `DbInterception.Add` de l’intercepteur même plusieurs fois, ou vous obtiendrez les instances supplémentaires de l’intercepteur. Par exemple, si vous ajoutez l’intercepteur de journalisation à deux reprises, vous verrez deux journaux pour chaque requête SQL.

Les intercepteurs sont exécutées dans l’ordre d’enregistrement (l’ordre dans lequel le `DbInterception.Add` méthode est appelée). L’ordre peut concerner selon ce que vous faites dans l’intercepteur. Par exemple, un intercepteur peut changer la commande SQL qu’il obtient dans le `CommandText` propriété. Si elle ne modifie pas la commande SQL, l’intercepteur suivant obtient la commande SQL modifiée, pas la commande SQL d’origine.

Vous avez écrit le code de simulation d’erreur temporaire d’une manière qui vous permet de provoquer des erreurs temporaires en entrant une valeur différente dans l’interface utilisateur. Comme alternative, vous pouvez écrire le code de l’intercepteur pour toujours générer la séquence d’exceptions temporaires sans vérification pour une valeur de paramètre particulier. Vous pouvez ensuite ajouter l’intercepteur uniquement lorsque vous souhaitez générer des erreurs temporaires. Si vous le faites, cependant, n’ajoutez pas l’intercepteur jusqu'à ce que la fin de l’initialisation de base de données. En d’autres termes, effectuez l’opération d’au moins une base de données telles qu’une requête sur l’un des jeux d’entités avant de commencer à générer des erreurs temporaires. Entity Framework exécute plusieurs requêtes pendant l’initialisation de base de données, et elles ne sont pas exécutées dans une transaction, les erreurs pendant l’initialisation risquerait de provoquer le contexte obtenir un état incohérent.

## <a name="test-logging-and-connection-resiliency"></a>Résilience des connexions et de journalisation de test

1. Dans Visual Studio, appuyez sur **F5** pour exécuter l’application en mode débogage, puis connectez-vous en tant que « Admin », à l’aide de « Pa$ $word » comme mot de passe.
2. Sélectionnez **administrateur** à partir de la barre de navigation en haut.
3. Entrez un nouveau produit nommé « Lever » avec le fichier de description, prix et image approprié.
4. Appuyez sur la **ajouter un produit** bouton.  
   Vous remarquerez que le navigateur semble bloquée pendant plusieurs secondes tandis que Entity Framework tente à nouveau la requête plusieurs fois. La première nouvelle tentative se produit très rapidement, puis l’attente augmente avant chaque nouvelle tentative supplémentaire. Ce processus d’attente plus avant chaque nouvelle tentative est appelée *interruption exponentielle* .
5. Attendez que la page n’est plus atttempting à charger.
6. Arrêtez le projet et examinez le Visual Studio **sortie** fenêtre pour afficher la sortie de traçage. Vous pouvez trouver la **sortie** en sélectionnant **déboguer**  - &gt; **Windows**  - &gt;  **Sortie**. Vous devrez peut-être défiler plusieurs autres journaux écrits par votre journal.  
  
   Notez que vous pouvez voir les requêtes SQL réelles envoyées à la base de données. Vous consultez quelques requêtes initiales et les commandes que Entity Framework pour commencer, la vérification de la table d’historique de version et la migration de base de données.   
    ![Sortie (fenêtre)](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image1.png)   
   Notez que vous ne pouvez pas répéter ce test, sauf si vous arrêtez l’application et redémarrez. Si vous souhaitez être en mesure de tester la résilience des connexions à plusieurs fois en une seule exécution de l’application, vous pouvez écrire du code pour réinitialiser le compteur d’erreurs dans `InterceptorTransientErrors` .
7. Pour voir la différence la stratégie d’exécution (stratégie de nouvelle tentative) rend, commentaire le `SetExecutionStrategy` de ligne dans *WingtipToysConfiguration.cs* de fichiers dans le *logique* dossier, exécutez le **Admin**  page à nouveau en mode débogage et ajouter le produit nommé &quot;lever&quot; à nouveau.  
  
   Cette fois le débogueur s’arrête sur la première exception générée immédiatement lorsqu’il tente d’exécuter la requête de la première fois.  
    ![Débogage - afficher les détails](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image2.png)
8. Supprimez les commentaires de la `SetExecutionStrategy` de ligne dans le *WingtipToysConfiguration.cs* fichier.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez vu comment modifier un exemple d’application Web Forms pour prendre en charge la résilience des connexions et interception des commandes.

## <a name="next-steps"></a>Étapes suivantes

Après avoir examiné la résilience des connexions et interception des commandes dans ASP.NET Web Forms, consultez la rubrique de Web Forms ASP.NET [des méthodes asynchrones dans ASP.NET 4.5](../performance-and-caching/using-asynchronous-methods-in-aspnet-45.md). La rubrique présente les principes de base de la création d’une application ASP.NET Web Forms asynchrone à l’aide de Visual Studio.
