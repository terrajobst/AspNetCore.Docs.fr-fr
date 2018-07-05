---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: Optimisation des performances avec Entity Framework 4.0 dans une Application Web 4 ASP.NET | Microsoft Docs
author: tdykstra
description: Cette série de didacticiels s’appuie sur l’application web Contoso University créé par la mise en route avec la série de didacticiels Entity Framework 4.0. J’AI...
ms.author: aspnetcontent
ms.date: 01/26/2011
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 2c5aa6c8a512ccd2ff4b94f5a8a60b31aaa39fc5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828278"
---
<a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Optimisation des performances avec Entity Framework 4.0 dans une Application Web 4 ASP.NET
====================
par [Tom Dykstra](https://github.com/tdykstra)

> Cette série de didacticiels s’appuie sur l’application web Contoso University créé par le [mise en route avec Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) série de didacticiels. Si vous n’avez pas effectué les didacticiels précédents, comme point de départ pour ce didacticiel vous pouvez [télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que vous l’auriez créée. Vous pouvez également [télécharger l’application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) qui est créé par la série de didacticiels terminée. Si vous avez des questions sur les didacticiels, vous pouvez les publier à le [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


Dans le didacticiel précédent, vous avez vu comment gérer les conflits d’accès concurrentiel. Ce didacticiel montre les options pour améliorer les performances d’une application web ASP.NET qui utilise Entity Framework. Vous allez découvrir plusieurs méthodes pour optimiser les performances ou pour diagnostiquer les problèmes de performances.

Les informations présentées dans les sections suivantes sont susceptibles d’être utile dans un large éventail de scénarios :

- Charger efficacement les données associées.
- Gérer l’état d’affichage.

Les informations présentées dans les sections suivantes peuvent être utiles si vous avez des requêtes individuelles qui présente des problèmes de performances :

- Utilisez la `NoTracking` option de fusion.
- La précompilation des requêtes LINQ.
- Examinez les commandes de requête envoyés à la base de données.

Les informations présentées dans la section suivante peut être utiles pour les applications qui ont des modèles de données extrêmement volumineux :

- Prégénérer des vues.

> [!NOTE]
> Performances de l’application Web est affectée par de nombreux facteurs, notamment des renseignements tels que la taille des données de demande et de réponse, la vitesse des requêtes de base de données, de nombre de requêtes vers que le serveur peut être en file et de la vitesse à laquelle il peut traiter et même l’efficacité de n’importe quel vous utilisez peut-être de bibliothèques de script client. Si les performances sont critiques dans votre application, ou si le test ou l’expérience montre que les performances de l’application n’est pas satisfaisante, vous devez suivre le protocole normal pour le réglage des performances. Mesurer pour déterminer où sont produisent les goulots d’étranglement et puis résoudre les domaines ayant le plus grand impact sur les performances globales de l’application.
> 
> Cette rubrique se concentre principalement sur les façons dans lequel vous pouvez potentiellement améliorer les performances en particulier d’Entity Framework dans ASP.NET. Les suggestions ici sont utiles si vous déterminez que l’accès aux données est un des goulots d’étranglement de performances dans votre application. Sauf comme mentionné, les méthodes expliquées ici ne doit pas être considéré comme &quot;meilleures pratiques&quot; en général, bon nombre d'entre eux sont appropriées uniquement dans des situations exceptionnelles ou à l’adresse très particuliers des goulots d’étranglement de performances.


Pour démarrer le didacticiel, démarrez Visual Studio et ouvrez l’application web Contoso University que vous utilisez dans le didacticiel précédent.

## <a name="efficiently-loading-related-data"></a>Chargement efficacement des données associées

Il existe plusieurs façons que Entity Framework peut charger des données associées dans les propriétés de navigation d’une entité :

- *Chargement différé*. Quand l’entité est lue pour la première fois, les données associées ne sont pas récupérées. Toutefois, la première fois que vous essayez d’accéder à une propriété de navigation, les données requises pour cette propriété de navigation sont récupérées automatiquement. Il en résulte plusieurs requêtes sont envoyées à la base de données : une pour l’entité elle-même et chaque fois que les données pour l’entité associées doivent être récupérées. 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*Chargement hâtif*. Quand l’entité est lue, ses données associées sont également récupérées. Cela génère en général une requête de jointure unique qui récupère toutes les données nécessaires. Vous spécifiez un chargement hâtif à l’aide de la `Include` (méthode), comme vous l’avez déjà vu dans ces didacticiels.

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *Chargement explicite*. Cela revient à chargement différé, à ceci près que vous récupériez les données associées dans le code. Il ne se produit pas automatiquement lorsque vous accédez à une propriété de navigation. Vous chargez des données connexes manuellement à l’aide de la `Load` méthode de la propriété de navigation pour les collections ou que vous utilisez le `Load` méthode de la propriété de référence pour les propriétés qui contiennent un seul objet. (Par exemple, vous appelez le `PersonReference.Load` méthode pour charger le `Person` propriété de navigation d’un `Department` entity.)

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Car ils ne récupèrent pas immédiatement les valeurs de propriété, le chargement différé et le chargement explicite sont également tous deux appelés *le chargement différé*.

Le chargement différé est le comportement par défaut pour un contexte d’objet qui a été généré par le concepteur. Si vous ouvrez le *SchoolModel.Designer.cs* fichier qui définit la classe de contexte d’objet, vous trouverez trois méthodes de constructeur, et chacun d’eux inclut l’instruction suivante :

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

En règle générale, si vous savez que vous devez les données associées pour chaque entité récupéré, hâtif chargement offre les meilleures performances, car une requête unique envoyée à la base de données est généralement plus efficace que les requêtes distinctes pour chaque entité récupérée. En revanche, si vous avez besoin accéder aux propriétés de navigation d’une entité que rarement ou uniquement pour un petit ensemble d’entités, de chargement différé ou de chargement explicite peut être plus efficace, car le chargement hâtif permet de récupérer plus de données que vous avez besoin.

Dans une application web, le chargement différé peut être relativement peu d’intérêt tout de même, étant donné que les actions de l’utilisateur qui affectent la nécessité pour les données associées ont lieu dans le navigateur, qui ne possède aucune connexion au contexte d’objet que la page rendue. En revanche, lorsque vous databind un contrôle, généralement savoir quelles données vous avez besoin et par conséquent, il est généralement meilleures pour choisir un chargement hâtif ou le chargement différé selon ce qui est approprié dans chaque scénario.

En outre, un contrôle lié aux données peut utiliser un objet d’entité, une fois que le contexte de l’objet est supprimé. Dans ce cas, une tentative de chargement différé une propriété de navigation échouerait. Vous recevez le message d’erreur est clair : &quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

Le `EntityDataSource` contrôle désactive le chargement différé par défaut. Pour le `ObjectDataSource` contrôle que vous utilisez pour le didacticiel actuel (ou si vous accéder au contexte d’objet à partir du code de page), il existe plusieurs façons, vous pouvez rendre différé chargement désactivé par défaut. Vous pouvez le désactiver lorsque vous instanciez un contexte d’objet. Par exemple, vous pouvez ajouter la ligne suivante à la méthode de constructeur de la `SchoolRepository` classe :

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Pour l’application Contoso University, vous apporterez le contexte d’objet automatiquement désactiver le chargement différé pour que cette propriété ne doit être définie chaque fois qu’un contexte est instancié.

Ouvrez le *SchoolModel.edmx* données de modèle et cliquez sur l’aire de conception, puis, dans le volet Propriétés, définissez la **activée de chargement différé** propriété `False`. Enregistrez et fermez le modèle de données.

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>Gestion de l’état d’affichage

Pour fournir des fonctionnalités de mise à jour, une page web ASP.NET doit stocker les valeurs de propriété d’origine d’une entité lorsqu’une page est rendue. Au cours de la publication (postback) du contrôle de traitement recréer l’état d’origine de l’entité, appelez l’entité `Attach` méthode avant d’appliquer les modifications et en appelant le `SaveChanges` (méthode). Par défaut, les contrôles de données ASP.NET Web Forms utilisent l’état d’affichage pour stocker les valeurs d’origine. Toutefois, l’état d’affichage peut affecter les performances, car il est stocké dans des champs masqués qui peuvent augmenter considérablement la taille de la page est envoyée vers et depuis le navigateur.

Techniques de gestion d’état d’affichage ou alternatives telles que l’état de session, ne sont pas spécifiques à Entity Framework, afin de ce didacticiel n’aborde ce sujet en détail. Pour plus d’informations consultez les liens à la fin du didacticiel.

Toutefois, la version 4 d’ASP.NET fournit une nouvelle façon de travailler avec l’état d’affichage dont tous les développeurs d’applications Web Forms ASP.NET doivent être conscient : le `ViewStateMode` propriété. Cette nouvelle propriété peut être définie au niveau de la page ou le contrôle, et il vous permet de désactiver l’état d’affichage par défaut pour une page et l’activer uniquement pour les contrôles qui en ont besoin.

Pour les applications où les performances sont critiques, une bonne pratique est toujours désactiver l’état d’affichage au niveau de la page et l’activer uniquement pour les contrôles qui en ont besoin. La taille de l’état d’affichage dans les pages de Contoso University n’être substantiellement réduite par cette méthode, mais pour voir comment il fonctionne, vous allez le faire le *Instructors.aspx* page. Cette page contient de nombreux contrôles, y compris un `Label` contrôle qui a désactivé l’état d’affichage. Aucun des contrôles sur cette page réellement faut afficher l’état activé. (Le `DataKeyNames` propriété de la `GridView` contrôle spécifie l’état qui doit être maintenue entre publications (postback), mais ces valeurs sont conservées dans l’état de contrôle, ce qui n’est pas affectée par le `ViewStateMode` propriété.)

Le `Page` directive et `Label` balisage du contrôle ressemble actuellement à l’exemple suivant :

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Apportez les modifications suivantes :

- Ajouter `ViewStateMode="Disabled"` à la `Page` directive.
- Supprimer `ViewStateMode="Disabled"` à partir de la `Label` contrôle.

Le balisage ressemble maintenant à l’exemple suivant :

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

État d’affichage est désormais désactivé pour tous les contrôles. Si vous ajoutez ultérieurement un contrôle qui a besoin d’utiliser l’état d’affichage, il vous suffit est incluent le `ViewStateMode="Enabled"` attribut pour ce contrôle.

## <a name="using-the-notracking-merge-option"></a>À l’aide de l’Option de fusion NoTracking

Quand un contexte d’objet récupère les lignes de base de données et crée des objets d’entité qui les représentent, par défaut il suit également ces objets d’entité à l’aide de son gestionnaire d’état objet. Ces données de suivi agissant comme un cache et sont utilisées lorsque vous mettez à jour une entité. Car une application web possède généralement des instances de contexte d’objet éphémère, requêtes souvent retournent des données qui ne doivent être suivis, car le contexte de l’objet qui lit les est supprimé avant d’une des entités, qu'il lit réutilisée ou mis à jour.

Dans Entity Framework, vous pouvez spécifier si le contexte de l’objet effectue le suivi des objets d’entité en définissant un *option de fusion*. Vous pouvez définir l’option de fusion pour les requêtes individuelles ou pour les jeux d’entités. Si vous définissez pour un jeu d’entités, qui signifie que vous définissez l’option de fusion par défaut pour toutes les requêtes qui sont créés pour ce jeu d’entités.

Pour l’application Contoso University, le suivi n’est pas nécessaire pour tous les jeux d’entités qui accèdent à partir du référentiel, vous pouvez donc définir l’option de fusion `NoTracking` pour ces jeux d’entités lorsque vous instanciez le contexte de l’objet dans la classe de dépôt. (Notez que dans ce didacticiel, l’option de fusion ne sont pas avoir un effet notable sur les performances de l’application. Le `NoTracking` option est susceptible d’apporter une amélioration des performances observable dans certains scénarios de données volumineuses.)

Dans le dossier de la couche DAL, ouvrez le *SchoolRepository.cs* fichier, puis ajoutez une méthode de constructeur qui définit l’option de fusion pour les jeux d’entités que le référentiel accède à :

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>Compilation des requêtes LINQ

La première fois que l’Entity Framework exécute une requête Entity SQL au sein de la durée de vie d’une donnée `ObjectContext` instance, il prend un certain temps pour compiler la requête. Le résultat de la compilation est mis en cache, ce qui signifie que les exécutions ultérieures de la requête sont beaucoup plus rapides. Requêtes LINQ suivent un modèle similaire, à ceci près que la partie du travail requis pour compiler la requête est effectuée chaque fois que la requête est exécutée. En d’autres termes, pour les requêtes LINQ, par défaut pas tous les résultats de la compilation sont mis en cache.

Si vous avez une requête LINQ qui vous prévoyez d’exécuter à plusieurs reprises dans la vie d’un contexte d’objet, vous pouvez écrire du code qui provoque de tous les résultats de la compilation à mettre en cache la première exécution de la requête LINQ.

En guise d’illustration, vous verrez comment procéder pour deux `Get` méthodes dans le `SchoolRepository` (classe), un d'entre eux n’accepte aucun paramètre (le `GetInstructorNames` méthode) et l’autre ne nécessitant pas de paramètre (le `GetDepartmentsByAdministrator` (méthode)). Ces méthodes dans leur état maintenant réellement n’avez pas besoin de compiler, car ils ne sont pas des requêtes LINQ :

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

Toutefois, afin que vous pouvez essayer les requêtes compilées, vous allez continuer comme s’il avaient été écrit en tant que les requêtes LINQ suivantes :

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Vous pouvez modifier le code de ces méthodes à ce qui a indiqué ci-dessus et exécutez l’application pour vérifier qu’elle fonctionne avant de continuer. Mais les instructions suivantes lancer directement dans la création de versions précompilées d'entre eux.

Créez un fichier de classe dans le *DAL* dossier, nommez-le *SchoolEntities.cs*et remplacez le code existant par le code suivant :

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Ce code crée une classe partielle qui étend la classe de contexte d’objet généré automatiquement. La classe partielle inclut deux requêtes LINQ compilées à l’aide de la `Compile` méthode de la `CompiledQuery` classe. Il crée également des méthodes que vous pouvez utiliser pour appeler les requêtes. Enregistrez et fermez ce fichier.

Ensuite, dans *SchoolRepository.cs*, modifier les existants `GetInstructorNames` et `GetDepartmentsByAdministrator` méthodes dans le référentiel classe afin qu’ils appellent les requêtes compilées :

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

Exécutez le *Departments.aspx* page pour vérifier qu’elle fonctionne comme avant. Le `GetInstructorNames` méthode est appelée pour remplir la liste déroulante administrateur et le `GetDepartmentsByAdministrator` méthode est appelée lorsque vous cliquez sur **mise à jour** afin de vérifier qu’aucun formateur n’est un administrateur de plusieurs département.

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

Vous avez les requêtes précompilées dans l’application Contoso University uniquement pour voir comment le faire, pas, car elle n’améliore sensiblement les performances. La précompilation de requêtes LINQ ajouter un niveau de complexité à votre code, par conséquent, assurez-vous que vous l’effectuez uniquement pour les requêtes qui représentent les goulots d’étranglement dans votre application.

## <a name="examining-queries-sent-to-the-database"></a>Examen des requêtes envoyées à la base de données

Lorsque vous recherchez des informations sur les problèmes de performances, il est parfois utile de connaître les commandes SQL exacts qui envoie à la base de données Entity Framework. Si vous travaillez avec un `IQueryable` de l’objet, un pour ce faire consiste à utiliser le `ToTraceString` (méthode).

Dans *SchoolRepository.cs*, modifiez le code dans le `GetDepartmentsByName` méthode correspondant à l’exemple suivant :

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

Le `departments` variable doit être convertie en un `ObjectQuery` type uniquement, car le `Where` méthode à la fin de la ligne précédente crée un `IQueryable` objet ; sans le `Where` (méthode), le cast ne serait pas nécessaire.

Définir un point d’arrêt sur la `return` de ligne, puis exécutez le *Departments.aspx* page dans le débogueur. Lorsque vous atteignez le point d’arrêt, examiner le `commandText` variable dans le **variables locales** fenêtre et utilisez le visualiseur de texte (la Loupe dans la **valeur** colonne) pour afficher sa valeur dans le **Visualiseur de texte** fenêtre. Vous pouvez voir la commande SQL entière qui résulte de ce code :

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Comme alternative, la fonctionnalité IntelliTrace dans Visual Studio Ultimate fournit un moyen d’afficher les commandes SQL générées par Entity Framework ne vous obliger à modifier votre code ou même définir un point d’arrêt.

> [!NOTE]
> Vous pouvez effectuer les procédures suivantes uniquement si vous avez Visual Studio Ultimate.


Restaurer le code d’origine dans le `GetDepartmentsByName` (méthode), puis exécutez le *Departments.aspx* page dans le débogueur.

Dans Visual Studio, sélectionnez le **déboguer** menu, puis **IntelliTrace**, puis **événements IntelliTrace**.

[![Image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Dans le **IntelliTrace** fenêtre, cliquez sur **interrompre tout**.

[![Image12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Le **IntelliTrace** fenêtre affiche une liste d’événements récents :

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Cliquez sur le **ADO.NET** ligne. Il se développe pour vous afficher le texte de commande :

[![Image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Vous pouvez copier la chaîne de texte de commande entière dans le Presse-papiers à partir de la **variables locales** fenêtre.

Supposons que vous travailliez avec une base de données avec plusieurs tables, relations et de colonnes que la simple `School` base de données. Vous constaterez qu’une requête qui regroupe toutes les informations que vous devez dans un seul `Select` instruction contenant plusieurs `Join` clauses devient trop complexe pour travailler efficacement. Dans ce cas, vous pouvez passer de chargement pour le chargement explicite pour simplifier la requête hâtif.

Par exemple, essayez de modifier le code dans le `GetDepartmentsByName` méthode dans *SchoolRepository.cs*. Actuellement dans la mesure méthode que vous avez une requête d’objet qui a `Include` méthodes pour le `Person` et `Courses` propriétés de navigation. Remplacez le `return` instruction avec le code qui effectue le chargement explicite, comme illustré dans l’exemple suivant :

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Exécutez le *Departments.aspx* page dans le débogueur et vérifier le **IntelliTrace** fenêtre à nouveau en tant que vous l’avez fait auparavant. Lorsqu’il y a une seule requête avant, vous voyez maintenant, une longue séquence d’eux.

[![Image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Cliquez sur le premier **ADO.NET** ligne pour voir ce qui est arrivé à la requête complexe affiché précédemment.

[![Image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

La requête à partir des services est devenue une simple `Select` de requête sans aucune `Join` clause, mais il est suivi par des requêtes distinctes qui extraient des cours et un administrateur, à l’aide d’un ensemble de deux requêtes pour chaque service retourné par la version d’origine requête.

> [!NOTE]
> Si vous laissez différé le chargement est activé, le modèle que vous voyez ici, avec la même requête répétée à plusieurs reprises, peut entraîner le chargement différé. Un modèle que vous souhaitez généralement éviter est sur le chargement différé des données associées pour chaque ligne de la table primaire. À moins que vous avez vérifié qu’une requête de jointure unique est trop complexe pour être efficace, vous pourrez généralement améliorer les performances dans ce cas en modifiant la requête principale pour utiliser le chargement hâtif.


## <a name="pre-generating-views"></a>Pré-générer des affichages

Quand un `ObjectContext` objet est créé dans un nouveau domaine d’application, Entity Framework génère un ensemble de classes qu’il utilise pour accéder à la base de données. Ces classes sont appelées *vues*, et si vous avez un modèle de données très volumineux, génération de ces vues peut retarder réponse du site web à la première requête pour une page après l’initialisation d’un nouveau domaine d’application. Vous pouvez réduire ce délai de la première requête en créant des vues au moment de la compilation plutôt qu’au moment de l’exécution.

> [!NOTE]
> Si votre application n’a pas un modèle de données extrêmement volumineux, ou si elle a un modèle de données de grande taille, mais vous ne vous inquiétez pas un problème de performances qui affecte uniquement la première requête de page une fois que IIS est recyclé, vous pouvez ignorer cette section. Vue de la création ne se produit chaque fois que vous instanciez un `ObjectContext` de l’objet, car les vues sont mis en cache dans le domaine d’application. Par conséquent, sauf si vous êtes recyclage fréquemment votre application dans IIS, les demandes de pages très peu tireront à partir des vues prégénérées.


Vous pouvez prégénérer des vues à l’aide de la *EdmGen.exe* outil de ligne de commande ou en utilisant un *Text Template Transformation Toolkit* modèle (T4). Dans ce didacticiel, vous allez utiliser un modèle T4.

Dans le *DAL* dossier, ajouter un fichier en utilisant le **modèle de texte** modèle (il se trouve sous le **général** nœud dans le **modèles installés** liste), et nommez-le *SchoolModel.Views.tt*. Remplacez le code existant dans le fichier par le code suivant :

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

Ce code génère des vues pour un *.edmx* fichier qui se trouve dans le même dossier que le modèle et qui porte le même nom que le fichier de modèle. Par exemple, si votre fichier de modèle est nommé *SchoolModel.Views.tt*, il recherche un fichier de modèle de données nommé *SchoolModel.edmx*.

Enregistrez le fichier, puis cliquez sur le fichier dans **l’Explorateur de solutions** et sélectionnez **exécuter un outil personnalisé**.

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Visual Studio génère un fichier de code qui crée les vues, qui est nommé *SchoolModel.Views.cs* basé sur le modèle. (Vous avez peut-être remarqué que le fichier de code est généré avant même que vous sélectionnez **exécuter un outil personnalisé**, dès que vous enregistrez le fichier de modèle.)

[![Image01](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Vous pouvez maintenant exécuter l’application et vérifier qu’elle fonctionne comme avant.

Pour plus d’informations sur les vues prégénérées, consultez les ressources suivantes :

- [Comment : prégénérer des vues pour améliorer les performances de requête](https://msdn.microsoft.com/library/bb896240.aspx) sur le site web MSDN. Explique comment utiliser le `EdmGen.exe` outil de ligne de commande pour prégénérer des vues.
- [Isolation des performances avec les vues de précompilées/prégénérées dans Entity Framework 4](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx) sur le blog de Windows Server AppFabric Customer Advisory Team.

Cette étape termine l’introduction à l’amélioration des performances dans une application web ASP.NET qui utilise Entity Framework. Pour plus d'informations, reportez-vous aux ressources suivantes :

- [Considérations sur les performances (Entity Framework)](https://msdn.microsoft.com/library/cc853327.aspx) sur le site web MSDN.
- [Les publications relatives aux performances sur le blog de l’équipe de Entity Framework](https://blogs.msdn.com/b/adonet/archive/tags/performance/).
- [EF Options de fusion et requêtes compilées](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx). Billet de blog qui explique les comportements inattendus de requêtes compilées et fusion options telles que `NoTracking`. Si vous envisagez d’utiliser des requêtes compilées ou manipuler les paramètres d’option de fusion dans votre application, lisez d’abord ceci.
- [Entité liée de Framework publie dans le blog de données et modélisation Customer Advisory Team](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/). Inclut des billets sur les requêtes compilées et à l’aide de la Profiler de 2010 Visual Studio pour détecter les problèmes de performances.
- [Thread de forum Entity Framework avec des conseils sur l’amélioration des performances des requêtes très complexes](https://social.msdn.microsoft.com/Forums/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6).
- [Recommandations de gestion d’état ASP.NET](https://msdn.microsoft.com/library/z1hkazw7.aspx).
- [À l’aide d’Entity Framework et ObjectDataSource : la pagination personnalisée](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx). Billet de blog qui s’appuie sur l’application ContosoUniversity créée dans ces didacticiels pour expliquer comment implémenter la pagination dans le *Departments.aspx* page.

Le didacticiel suivant passe en revue certaines des améliorations importantes apportées à Entity Framework qui sont nouvelles dans la version 4.

> [!div class="step-by-step"]
> [Précédent](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
> [Suivant](what-s-new-in-the-entity-framework-4.md)
