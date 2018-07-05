---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
title: Résilience des connexions et Interception des commandes avec Entity Framework dans une Application ASP.NET MVC | Microsoft Docs
author: tdykstra
description: L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 5 à l’aide de l’Entity Framework 6 Code First et Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/13/2015
ms.topic: article
ms.assetid: c89d809f-6c65-4425-a3fa-c9f6e8ac89f2
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: bacc49ff60b758b729c200f7943ff654f614ac4c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394792"
---
<a name="connection-resiliency-and-command-interception-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Résilience des connexions et Interception des commandes avec Entity Framework dans une Application ASP.NET MVC
====================
par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet terminé](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) ou [télécharger le PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> L’exemple d’application web Contoso University montre comment créer des applications ASP.NET MVC 5 à l’aide de l’Entity Framework 6 Code First et Visual Studio 2013. Pour obtenir des informations sur la série de didacticiels, consultez [le premier didacticiel de la série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Jusqu'à présent, l’application a été exécuté localement dans IIS Express sur votre ordinateur de développement. Pour rendre une application réelle disponible pour d’autres personnes à utiliser sur Internet, vous devez déployer sur un fournisseur d’hébergement web, et vous devez déployer la base de données à un serveur de base de données.

Dans ce didacticiel, vous allez apprendre à utiliser deux fonctionnalités d’Entity Framework 6 sont particulièrement utiles lorsque vous déployez à l’environnement de cloud : la résilience de connexion (les nouvelles tentatives automatiques pour les erreurs temporaires) et interception des commandes (catch toutes les requêtes SQL envoyé à la base de données afin d’ouvrir une session ou de les modifier).

Ce didacticiel de l’interception de la résilience et la commande connexion est facultatif. Si vous ignorez ce didacticiel, quelques ajustements mineurs seront ne doit être faite dans les didacticiels suivants.

## <a name="enable-connection-resiliency"></a>Activer la résilience des connexions

Lorsque vous déployez l’application sur Windows Azure, vous allez déployer la base de données vers Microsoft Azure SQL Database, un service de base de données cloud. Erreurs de connexion temporaires sont généralement plus fréquents lorsque vous vous connectez à un service de base de données de cloud que lorsque votre serveur web et votre serveur de base de données sont directement interconnectés dans le même centre de données. Même si un serveur web de cloud et un service de base de données de cloud sont hébergés dans le même centre de données, il existe plusieurs connexions réseau entre eux qui peuvent avoir des problèmes, tels que les équilibreurs de charge.

Un service cloud est également généralement partagé par d’autres utilisateurs, ce qui signifie que sa réactivité peut être affectée par ces derniers. Et votre accès à la base de données peut être soumis à la limitation. La limitation signifie que le service de base de données lève des exceptions lorsque vous essayez d’y accéder plus fréquemment qu’il est autorisé dans votre contrat de niveau de Service (SLA).

Nombre ou la plupart des problèmes de connexion lorsque vous accédez à un service cloud sont temporaires, autrement dit, ils résolvent d’eux-mêmes dans une courte période de temps. Par conséquent, lorsque vous tentez une opération de base de données et obtenez un type d’erreur qui est généralement temporaire, vous pouvez essayer à nouveau l’opération après qu’un court délai d’attente et l’opération est peut-être réussie. Vous pouvez fournir une expérience nettement meilleure pour vos utilisateurs, si vous gérez des erreurs temporaires en réessayant d’automatiquement, en rendant la plupart d'entre eux invisible au client. La fonctionnalité de résilience de connexion dans Entity Framework 6 automatise qu’Échec du processus de nouvelle tentative de requêtes SQL.

La fonctionnalité de résilience de connexion doit être correctement configurée pour un service de base de données particulière :

- Il doit connaître les exceptions qui sont susceptibles d’être temporaire. Voulez-vous réessayer d’erreurs provoquées par une perte temporaire de connectivité réseau, pas les erreurs provoquées par des bogues de programme, par exemple.
- Il doit attendre une quantité appropriée de temps entre les tentatives d’une opération ayant échoué. Vous pouvez attendre de plus entre les nouvelles tentatives pour un traitement par lots que vous pouvez le faire pour une page web en ligne où un utilisateur est en attente d’une réponse.
- Il doit réessayer un nombre approprié de fois avant d’abandonner. Vous souhaiterez peut-être plusieurs nouvelles tentatives dans un traitement par lots que vous le feriez dans une application en ligne.

Vous pouvez configurer ces paramètres manuellement pour n’importe quel environnement de base de données pris en charge par un fournisseur Entity Framework, mais les valeurs par défaut qui fonctionnent généralement bien pour une application en ligne qui utilise la base de données SQL Windows Azure ont déjà été configurés pour vous, et Ce sont les paramètres que vous allez implémenter pour l’application Contoso University.

Il vous suffit pour activer la résilience des connexions est créer une classe dans votre assembly qui dérive de la [DbConfiguration](https://msdn.microsoft.com/data/jj680699.aspx) classe et, dans cette classe, définissez la base de données SQL *stratégie d’exécution*, qui est dans EF un autre terme pour *stratégie de nouvelle tentative*.

1. Dans le dossier de la couche DAL, ajoutez un fichier de classe nommé *SchoolConfiguration.cs*.
2. Remplacez le code du modèle par le code suivant :

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

    Entity Framework s’exécute automatiquement le code qu’il trouve dans une classe qui dérive de `DbConfiguration`. Vous pouvez utiliser la `DbConfiguration` classe pour effectuer des tâches de configuration dans le code que vous le feriez dans le cas contraire dans le *Web.config* fichier. Pour plus d’informations, consultez [EntityFramework Configuration basée sur le Code](https://msdn.microsoft.com/data/jj680699).
3. Dans *StudentController.cs*, ajoutez un `using` instruction pour `System.Data.Entity.Infrastructure`.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]
4. Modifier tous les `catch` bloque qui interceptent `DataException` exceptions afin qu’elles interceptent `RetryLimitExceededException` exceptions à la place. Exemple :

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=1)]

    Vous utilisiez `DataException` pour essayer d’identifier les erreurs qui peuvent être temporaires afin d’octroyer à un message convivial « réessayez ». Mais maintenant que vous avez activé une stratégie de nouvelle tentative, seules les erreurs susceptibles d’être temporaire déjà être essayés et a échoué plusieurs fois et l’exception réelle retournée sera encapsulée dans le `RetryLimitExceededException` exception.

Pour plus d’informations, consultez [Entity Framework la résilience des connexions / logique de nouvelle tentative](https://msdn.microsoft.com/data/dn456835).

## <a name="enable-command-interception"></a>Activer l’Interception des commandes

Maintenant que vous avez activé une stratégie de nouvelle tentative, comment tester pour vérifier qu’il fonctionne comme prévu ? Il n’est pas si facile forcer une erreur temporaire se produire, en particulier lorsque vous exécutez localement, et il serait particulièrement difficile à intégrer des erreurs temporaires réels dans un test unitaire automatisé. Pour tester la fonctionnalité de résilience de connexion, vous avez besoin d’un moyen d’intercepter les requêtes Entity Framework envoie à SQL Server et remplacez la réponse de SQL Server avec un type d’exception qui est généralement temporaire.

Vous pouvez également utiliser l’interception de requête pour mettre en œuvre une meilleure pratique pour les applications cloud : [consigne la latence et la réussite ou l’échec de tous les appels aux services externes](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) tels que les services de base de données. EF6 fournit un [dédié API de journalisation](https://msdn.microsoft.com/data/dn469464) qui peut faciliter la connexion, mais dans cette section du didacticiel, vous allez apprendre à utiliser Entity Framework [fonction d’interception](https://msdn.microsoft.com/data/dn469464) directement, à la fois pour journalisation et de simulation d’erreurs temporaires.

### <a name="create-a-logging-interface-and-class"></a>Créer une interface de journalisation et de la classe

Un [meilleures pratiques pour la journalisation](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) est de le faire à l’aide d’une interface plutôt que de coder en dur des appels à System.Diagnostics.Trace ou à une classe de journalisation. Cela facilite la modifier ultérieurement votre mécanisme de journalisation si vous avez besoin pour ce faire. Donc dans cette section, vous allez créer l’interface de journalisation et d’une classe pour implémenter de celui-ci. / p > 

1. Créez un dossier dans le projet et nommez-le *journalisation*.
2. Dans le *journalisation* dossier, créez un fichier de classe nommé *ILogger.cs*et remplacez le code du modèle par le code suivant :

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    L’interface fournit trois niveaux de suivi pour indiquer l’importance relative des journaux et l’autre conçu pour fournir des informations de latence pour les appels de service externe telles que les requêtes de base de données. Les méthodes de journalisation ont des surcharges qui vous permettent de passer d’une exception. Il s’agit afin que les informations sur les exceptions, y compris stack trace et les exceptions internes sont fiable enregistrées par la classe qui implémente l’interface, au lieu de compter sur une fois l’opération effectuée dans chaque appel de méthode de journalisation dans toute l’application.

    Les méthodes TraceApi permettent d’effectuer le suivi de la latence de chaque appel à un service externe, comme SQL Database.
3. Dans le *journalisation* dossier, créez un fichier de classe nommé *Logger.cs*et remplacez le code du modèle par le code suivant :

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    L’implémentation utilise System.Diagnostics pour effectuer le suivi. Il s’agit d’une fonctionnalité intégrée de .NET qui permet de facilement générer et utiliser les informations de traçage. Il existe de nombreux « écouteurs » que vous pouvez utiliser avec le suivi System.Diagnostics, à écrire des journaux dans des fichiers, par exemple, ou à les écrire dans le stockage d’objets blob dans Azure. Certaines des options et des liens vers d’autres ressources pour plus d’informations, consultez dans [dépannage de Sites Web Azure dans Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Pour ce didacticiel, vous explorerez uniquement des journaux dans Visual Studio **sortie** fenêtre.

    Dans une application de production, vous souhaiterez prendre en compte les packages suivi System.Diagnostics, et l’interface ILogger relativement facilite à basculer vers un mécanisme de suivi différent si vous décidez de le faire.

### <a name="create-interceptor-classes"></a>Créer des classes de l’intercepteur

Ensuite, vous allez créer les classes Entity Framework appellera chaque fois qu’il va envoyer une requête à la base de données : une pour simuler des erreurs temporaires et pour effectuer la journalisation. Ces classes de l’intercepteur doivent dériver de la `DbCommandInterceptor` classe. Dans les, vous écrivez des substitutions de méthode qui sont appelées automatiquement lors de la requête est prête à être exécutée. Dans ces méthodes, vous pouvez examiner ou la requête qui est envoyée à la base de données de journal, et vous pouvez modifier la requête avant leur envoi à la base de données ou renvoyer quelque chose à Entity Framework vous-même sans même passer la requête à la base de données.

1. Pour créer la classe de l’intercepteur qui enregistrera chaque requête SQL qui est envoyé à la base de données, créez un fichier de classe nommé *SchoolInterceptorLogging.cs* dans le *DAL* dossier et remplacez le modèle de code avec le code suivant :

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

    Pour les requêtes ont abouti ou les commandes, ce code écrit un journal d’informations avec les informations de latence. Pour les exceptions, il crée un journal des erreurs.
2. Pour créer la classe de l’intercepteur qui génère des erreurs temporaires factices lorsque vous entrez « Lever » dans le **recherche** boîte, créez un fichier de classe nommé *SchoolInterceptorTransientErrors.cs* dans le *DAL* dossier, puis remplacez le code du modèle par le code suivant :

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

    Ce code substitue uniquement le `ReaderExecuting` (méthode), qui est appelée pour les requêtes qui peuvent retourner plusieurs lignes de données. Si vous souhaitez vérifier la résilience des connexions pour les autres types de requêtes, vous pouvez également substituer la `NonQueryExecuting` et `ScalarExecuting` méthodes, comme l’intercepteur de journalisation fait.

    Lorsque vous exécutez la page pour les étudiants et que vous entrez « Lever » comme chaîne de recherche, ce code crée une exception de base de données SQL factice pour le numéro d’erreur 20, un type connu pour être généralement temporaire. Autres numéros d’erreur actuellement reconnus comme provisoire sont 64 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 et 40613, mais ceux-ci sont susceptibles de changer dans les nouvelles versions de base de données SQL.

    Le code retourne l’exception à Entity Framework au lieu d’exécuter la requête et en passant les résultats de la requête de précédent. L’exception transitoire est retournée à quatre fois, et reprend ensuite le code à la procédure normale de transmission de la requête à la base de données.

    Étant donné que tout est connecté, vous serez en mesure de voir que Entity Framework essaie d’exécuter la requête quatre fois avant de réussir et la seule différence dans l’application est qu’il prend plus de temps pour afficher une page avec les résultats de la requête.

    Le nombre de tentatives de Entity Framework est configurable ; le code spécifie quatre fois car il s’agit de la valeur par défaut pour la stratégie d’exécution de base de données SQL. Si vous modifiez la stratégie d’exécution, il fallait également modifier le code qui spécifie le nombre de fois où les erreurs temporaires sont générés. Vous pouvez également modifier le code pour générer des exceptions plus afin que Entity Framework lèvera le `RetryLimitExceededException` exception.

    La valeur que vous entrez dans la zone de recherche sera dans `command.Parameters[0]` et `command.Parameters[1]` (un est utilisé pour le prénom et l’autre pour le nom de famille). Quand la valeur « Throw % » est trouvé, « Lever » est remplacé dans ces paramètres par « an » afin que certains étudiants seront trouvés et retournés.

    Il s’agit simplement d’un moyen pratique pour tester la résilience des connexions basée sur la modification d’une entrée à l’interface utilisateur de l’application. Vous pouvez également écrire du code qui génère des erreurs temporaires pour toutes les requêtes ou les mises à jour, comme expliqué plus loin dans les commentaires sur le *DbInterception.Add* (méthode).
3. Dans *Global.asax*, ajoutez le code suivant `using` instructions :

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]
4. Ajoutez les lignes en surbrillance à la `Application_Start` méthode :

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=7-8)]

    Ces lignes de code sont la cause de votre code de l’intercepteur à exécuter lors de l’Entity Framework envoie des requêtes à la base de données. Notez que parce que vous avez créé des classes de l’intercepteur distinct pour la simulation d’une erreur temporaire et la journalisation, vous pouvez indépendamment activer et désactiver les.

    Vous pouvez ajouter des intercepteurs d’à l’aide de la `DbInterception.Add` méthode n’importe où dans votre code ; pas nécessairement se trouver dans le `Application_Start` (méthode). Une autre option consiste à placer ce code dans la classe DbConfiguration que vous avez créé précédemment pour configurer la stratégie d’exécution.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=6-7)]

    Partout où vous placez ce code, être veillez à ne pas exécuter `DbInterception.Add` de l’intercepteur même plusieurs fois, ou vous obtiendrez les instances supplémentaires de l’intercepteur. Par exemple, si vous ajoutez l’intercepteur de journalisation à deux reprises, vous verrez deux journaux pour chaque requête SQL.

    Les intercepteurs sont exécutées dans l’ordre d’enregistrement (l’ordre dans lequel le `DbInterception.Add` méthode est appelée). L’ordre peut concerner selon ce que vous faites dans l’intercepteur. Par exemple, un intercepteur peut changer la commande SQL qu’il obtient dans le `CommandText` propriété. Si elle ne modifie pas la commande SQL, l’intercepteur suivant obtient la commande SQL modifiée, pas la commande SQL d’origine.

    Vous avez écrit le code de simulation d’erreur temporaire d’une manière qui vous permet de provoquer des erreurs temporaires en entrant une valeur différente dans l’interface utilisateur. Comme alternative, vous pouvez écrire le code de l’intercepteur pour toujours générer la séquence d’exceptions temporaires sans vérification pour une valeur de paramètre particulier. Vous pouvez ensuite ajouter l’intercepteur uniquement lorsque vous souhaitez générer des erreurs temporaires. Si vous le faites, cependant, n’ajoutez pas l’intercepteur jusqu'à ce que la fin de l’initialisation de base de données. En d’autres termes, effectuez l’opération d’au moins une base de données telles qu’une requête sur l’un des jeux d’entités avant de commencer à générer des erreurs temporaires. Entity Framework exécute plusieurs requêtes pendant l’initialisation de base de données, et elles ne sont pas exécutées dans une transaction, les erreurs pendant l’initialisation risquerait de provoquer le contexte obtenir un état incohérent.

## <a name="test-logging-and-connection-resiliency"></a>Résilience des connexions et de journalisation de test

1. Appuyez sur F5 pour exécuter l’application en mode débogage, puis cliquez sur le **étudiants** onglet.
2. Examinez le Visual Studio **sortie** fenêtre pour afficher la sortie de traçage. Vous devrez peut-être défiler vers le haut au-delà de certaines erreurs JavaScript pour obtenir les journaux écrits par votre journal.

    Notez que vous pouvez voir les requêtes SQL réelles envoyées à la base de données. Vous consultez certaines requêtes initiales et de commandes qui Entity Framework effectue pour commencer, une vérification de la version de base de données et de table d’historique de migration (vous allez en savoir plus sur les migrations dans le didacticiel suivant). Et vous voyez une requête pour la pagination, pour connaître les étudiants combien il existe, et enfin vous voyez la requête qui obtient les données de l’étudiant.

    ![Journalisation pour la requête normale](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Dans le **étudiants** page, entrez « Lever » comme chaîne de recherche, puis cliquez sur **recherche**.

    ![Lever la chaîne de recherche](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Vous remarquerez que le navigateur semble bloquée pendant plusieurs secondes tandis que Entity Framework tente à nouveau la requête plusieurs fois. La première nouvelle tentative produit très rapidement, puis l’attente avant augmente avant chaque nouvelle tentative supplémentaire. Ce processus d’attente plus avant chaque nouvelle tentative est appelée *interruption exponentielle*.

    Lorsque la page s’affiche, montrant les étudiants qui ont « un » dans leurs noms, examinez la fenêtre Sortie, et vous verrez que la même requête a été tentée cinq fois, le tout d’abord quatre fois retournant temporaires exceptions. Pour chaque erreur temporaire, vous verrez le journal que vous écrivez lors de la génération de l’erreur temporaire dans le `SchoolInterceptorTransientErrors` classe (« Returning erreur temporaire de la commande... ») et vous verrez le journal écrit quand `SchoolInterceptorLogging` Obtient l’exception.

    ![Sortie de journalisation montrant les nouvelles tentatives](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    Étant donné que vous avez entré une chaîne de recherche, la requête qui retourne des données sur les étudiants est paramétrée :

    [!code-sql[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.sql)]

    Vous vous connectez pas la valeur des paramètres, mais vous pouvez le faire. Si vous souhaitez afficher les valeurs de paramètre, vous pouvez écrire du code de journalisation pour obtenir les valeurs de paramètre à partir de la `Parameters` propriété de la `DbCommand` objet que vous obtenez dans les méthodes de l’intercepteur.

    Notez que vous ne pouvez pas répéter ce test, sauf si vous arrêtez l’application et redémarrez. Si vous souhaitez être en mesure de tester la résilience des connexions à plusieurs fois en une seule exécution de l’application, vous pouvez écrire du code pour réinitialiser le compteur d’erreurs dans `SchoolInterceptorTransientErrors`.
4. Pour voir la différence la stratégie d’exécution (stratégie de nouvelle tentative) rend, commentaire le `SetExecutionStrategy` de ligne dans *SchoolConfiguration.cs*, réexécutez la page des étudiants en mode débogage et recherchez « Throw » à nouveau.

    Cette fois le débogueur s’arrête sur la première exception générée immédiatement lorsqu’il tente d’exécuter la requête de la première fois.

    ![Exception factice](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
5. Supprimez les commentaires de la *SetExecutionStrategy* de ligne dans *SchoolConfiguration.cs*.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez vu comment activer la résilience des connexions et enregistrer les commandes SQL qui Entity Framework compose et envoie à la base de données. Dans le didacticiel suivant, vous allez déployer l’application à Internet, à l’aide des Migrations Code First pour déployer la base de données.

Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer. Vous pouvez également demander de nouvelles rubriques à [afficher les leçons de Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Vous trouverez des liens vers d’autres ressources Entity Framework dans [accès aux données ASP.NET - ressources recommandées](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Précédent](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Suivant](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
