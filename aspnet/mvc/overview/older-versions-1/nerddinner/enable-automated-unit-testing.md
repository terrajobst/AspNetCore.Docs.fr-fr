---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: Activer les tests unitaires automatisés | Microsoft Docs
author: microsoft
description: Étape 12 montre comment développer une suite de tests d’unités automatisés qui vérifient les nos fonctionnalités de NerdDinner, et qui nous donnera la confiance nécessaire pour apporter des modifications...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: 74abf391bb4aab3ff0d5079e0a24ba20287e18fb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830774"
---
<a name="enable-automated-unit-testing"></a>Activer les tests d’unités automatisés
====================
par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit d’étape 12 a gratuit [« « l’application NerdDinner](introducing-the-nerddinner-tutorial.md) qui présente en détail comment créer un petit mais terminé, l’application web à l’aide d’ASP.NET MVC 1.
> 
> Étape 12 montre comment développer une suite de tests d’unités automatisés qui vérifient les nos fonctionnalités de NerdDinner, et qui nous donnera la confiance nécessaire pour apporter des modifications et améliorations apportées à l’application à l’avenir.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre le [mise en route avec MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de musique MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) didacticiels.


## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner étape 12 : Tests unitaires

Nous allons développer une suite de tests d’unités automatisés qui vérifient les nos fonctionnalités de NerdDinner, et qui nous donnera la confiance nécessaire pour apporter des modifications et améliorations apportées à l’application à l’avenir.

### <a name="why-unit-test"></a>Pourquoi de Test unitaire ?

Sur le lecteur de travail un matin, vous avez un flash soudain d’inspiration relatives à une application que vous utilisez. Vous réalisez une modification que vous pouvez implémenter qui rendent l’améliorera considérablement de l’application. Il peut être une refactorisation qui nettoie le code, ajoute une nouvelle fonctionnalité ou corrige un bogue.

La question qui vous confronts lorsque vous arrivez sur votre ordinateur est : « mode sans échec est pour rendre cette amélioration ? » Que se passe-t-il si la modification a des effets ou qui interrompt quelque chose ? La modification peut être simple et ne prendre que quelques minutes à implémenter, mais que se passe-t-il s’il faut des heures afin de tester manuellement tous les scénarios d’application ? Que se passe-t-il si vous oubliez couvrir un scénario et une application interrompue mise en production ? Cette amélioration vraiment tous les efforts récompensés émane ?

Tests d’unités automatisés peuvent fournir un filet de sécurité qui vous permet d’améliorer continuellement vos applications et évitez d’être peur du code que vous utilisez. Avoir tests automatisés qui vérifier rapidement la fonctionnalité vous permet de coder en toute confiance – et vous aider à apporter des améliorations que vous pouvez sinon pas ont pensé à l’aise permet de faire. Ils permettent aussi de créer des solutions qui sont plus facile à gérer et ont une durée de vie plus longue - qui entraîne un meilleur retour sur investissement.

L’infrastructure ASP.NET MVC rend facile et naturel de la fonctionnalité application de test unitaire. Il permet également d’un flux de travail de développement de piloté par tests (TDD) qui permet le développement en fonction de test en premier.

### <a name="nerddinnertests-project"></a>Projet de NerdDinner.Tests

Lorsque nous avons créé notre application NerdDinner au début de ce didacticiel, nous avons invités avec une boîte de dialogue vous demandant si nous souhaitons créer un projet de test unitaire pour accéder en même temps que le projet d’application :

![](enable-automated-unit-testing/_static/image1.png)

Nous avons conservé la « Oui, créer un projet de test unitaire » case cochée : ce qui a entraîné un projet de « NerdDinner.Tests » ajouté à notre solution :

![](enable-automated-unit-testing/_static/image2.png)

Le projet NerdDinner.Tests fait référence à l’assembly de projet d’application NerdDinner et nous permet d’ajouter facilement des tests automatisés qui vérifient les fonctionnalités de l’application.

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Création de Tests unitaires pour notre classe de modèle dîner

Nous allons ajouter des tests à notre projet NerdDinner.Tests qui vérifient la classe Dinner que nous avons créé lorsque nous avons créé notre couche du modèle.

Nous allons commencer en créant un nouveau dossier au sein de notre projet de test appelé « Models » où nous allons placer nos tests associés au modèle. Nous allons ensuite avec le bouton droit sur le dossier et choisissez le **Add -&gt;nouveau Test** commande de menu. Cela fera apparaître la boîte de dialogue « Ajouter un nouveau Test ».

Nous allons choisir créer un « Test unitaire » et nommez-le « DinnerTest.cs » :

![](enable-automated-unit-testing/_static/image3.png)

Lorsque nous cliquons sur le bouton « OK » Visual Studio ajoute (et l’ouvre) un fichier DinnerTest.cs au projet :

![](enable-automated-unit-testing/_static/image4.png)

Le modèle de test unitaire Visual Studio par défaut comporte beaucoup de code standard dedans que je trouve un peu compliquée. Nous allons le nettoyer simplement contenir le code ci-dessous :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

L’attribut [TestClass] sur la classe DinnerTest ci-dessus identifie comme une classe qui contiendra les tests, ainsi que l’initialisation de test facultatif et code de destruction. Nous pouvons définir des tests qu’il contient en ajoutant des méthodes publiques qui ont un attribut [TestMethod] sur ces derniers.

Voici la première des deux cadre de tests que nous allons ajouter notre classe dîner. Le premier test vérifie que notre dîner n’est pas valide si un dîner nouveau est créé sans toutes les propriétés sont correctement définies. Le second test vérifie que notre dîner est valide lorsqu’un dîner a toutes les propriétés définies avec les valeurs valides :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

Vous remarquerez ci-dessus que les noms de test sont très explicite (et quelque peu détaillée). Nous faisons cela parce que nous pourrions finissent par créer des centaines voire des milliers de tests peu, et nous voulons être facile de déterminer rapidement l’intention et le comportement de chacun d’eux (en particulier lorsque nous recherchons dans une liste d’échecs dans un test runner). Les noms de test doivent être nommés d’après les fonctionnalités qu'ils testent. Ci-dessus, nous utilisons un « substantif\_doit\_verbe « modèle d’affectation de noms.

Nous sommes structurer les tests à l’aide de tests pattern – signifiant « Arrange, Act, Assert », « AAA » :

- Organiser : Configurer l’unité testée
- ACT : Exercer le test unitaire et capturer les résultats
- Assert : Vérifier le comportement

Lorsque nous écrivons les tests que nous souhaitons éviter que les tests individuels faire trop. Au lieu de cela, chaque test doit vérifier un concept unique (ce qui rend beaucoup plus facile d’identifier la cause des erreurs). Une bonne indication consiste à essayer et avoir uniquement une seule instruction pour chaque test d’assertion. Si vous avez plus d’une instruction dans une méthode de test d’assertion, assurez-vous qu’ils sont tous utilisés pour tester le même concept. En cas de doute, vérifiez un autre test.

### <a name="running-tests"></a>Exécution des tests

Visual Studio 2008 Professional (et versions supérieures) incluent un test runner intégré qui peut être utilisé pour exécuter des projets de tests unitaires Visual Studio dans l’IDE. Nous pouvons sélectionner le **Test -&gt;exécution -&gt;tous les Tests de la Solution** menu command (ou type R Ctrl, A) pour exécuter l’ensemble de nos tests unitaires. Ou vous pouvez également nous pouvons positionner notre curseur au sein d’une méthode de test ou de la classe de test spécifique et utiliser le **Test -&gt;exécution -&gt;Tests dans le contexte actuel** commande de menu (ou tapez Ctrl R et T) pour exécuter un sous-ensemble des tests unitaires.

Nous allons notre curseur est placé dans la classe DinnerTest et tapez « Ctrl R, T » pour exécuter les deux nous venons de définir des tests. Lorsque nous faisons cela une fenêtre « Résultats de Test » s’affiche dans Visual Studio et nous allons voir les résultats de notre série répertoriées qu’il contient :

![](enable-automated-unit-testing/_static/image5.png)

*Remarque : La fenêtre de résultats des tests de Visual Studio n’affiche pas la colonne nom de la classe par défaut. Vous pouvez ajouter cela en effectuant un clic droit dans la fenêtre Résultats des tests et à l’aide de la commande de menu Ajouter/supprimer des colonnes.*

Nos deux tests prenaient uniquement une fraction de seconde pour exécuter – et que vous pouvez voir les deux passés. Nous pouvons maintenant aller et augmenter leur nombre en créant des tests supplémentaires pour vérifier les validations de règle spécifique, ainsient que couvrent les deux méthodes d’assistance - IsUserHost() et IsUserRegisterd() – que nous avons ajouté à la classe dîner. Avoir tous ces tests en place pour la classe dîner rend beaucoup plus facile et plus sûr d’ajouter des règles d’entreprise validations à celui-ci et à l’avenir. Nous pouvons ajouter notre nouvelle logique de règle à dîner, puis vérifiez qu’il n’a pas rompu toute notre fonctionnalité logique précédente en quelques secondes.

Notez comment à l’aide d’un nom descriptif test rend facile à comprendre rapidement ce que chaque test consiste à vérifier. Je recommande d’utiliser le **Tools -&gt;Options** commande de menu, ouvrir les outils de Test -&gt;écran de configuration de l’exécution du Test, et la vérification du » en double-cliquant sur un résultat de test unitaire Échec ou non concluant affiche case à cocher au point de défaillance dans le test ». Cela vous permettra de double-cliquer sur un échec dans la fenêtre Résultats des tests et passer immédiatement à l’échec d’assert.

### <a name="creating-dinnerscontroller-unit-tests"></a>Création de Tests unitaires de DinnersController

Nous allons maintenant créer quelques tests unitaires qui vérifient les fonctionnalités de notre DinnersController. Nous allons commencer en cliquant sur le dossier « Contrôleurs » au sein de notre projet de Test, puis choisissez le **Add -&gt;nouveau Test** commande de menu. Nous allons créer des « Un Test unitaire » et nommez-le « DinnersControllerTest.cs ».

Nous allons créer deux méthodes de test qui vérifient la méthode d’action Details() sur le DinnersController. Le premier vérifie qu’une vue est retournée lorsqu’un dîner existant est demandé. Le second vérifie qu’une vue « NotFound » est retournée lorsqu’un dîner inexistante est demandé :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

Le code ci-dessus compile le nettoyage. Lorsque nous exécutons les tests, cependant, elles risquent d’échouer :

![](enable-automated-unit-testing/_static/image6.png)

Si nous examinons les messages d’erreur, nous verrons que la raison de l’échec de tests a été étant donné que notre classe DinnersRepository n’a pas pu se connecter à une base de données. Notre application NerdDinner utilise une chaîne de connexion dans un fichier local SQL Server Express qui se trouve sous le \App\_répertoire de données du projet d’application NerdDinner. Étant donné que notre projet NerdDinner.Tests compile et s’exécute dans un autre répertoire puis le projet d’application, l’emplacement de chemin d’accès relatif de notre chaîne de connexion est incorrect.

Nous *pourrait* résoudre ce problème en copiant le fichier de base de données SQL Express à notre projet de test et ajoutez une chaîne de connexion de test approprié à ce dernier dans le fichier App.config de notre projet de test. Cela offre les tests ci-dessus débloqué et en cours d’exécution.

Tests unitaires de code à l’aide d’une base de données réel, cependant, offre un certain nombre de défis. Plus précisément :

- Elle ralentit considérablement le temps d’exécution de tests unitaires. Plus longue à exécuter des tests, vous êtes moins susceptible d’exécuter les fréquemment. Dans l’idéal, vous souhaitez que vos tests unitaires pour pouvoir être exécuté en secondes et qu’elle vous faire soit en tant que naturellement en tant que la compilation du projet.
- Elle complique la logique de configuration et de nettoyage au sein de tests. Vous souhaitez que chaque test unitaire soit isolée et indépendant des autres (sans effets secondaires ni dépendances). Lorsque vous travaillez sur une base de données réelle, que vous devez être conscient de l’état et réinitialisez-le entre les tests.

Nous allons examiner un modèle de conception appelé « injection de dépendance » qui permettre nous aider à contourner ces problèmes et éviter la nécessité d’utiliser une base de données réel avec nos tests.

### <a name="dependency-injection"></a>Injection de dépendances

Maintenant DinnersController est « étroitement » à la classe DinnerRepository. « COUPLAGE » fait référence à une situation où une classe explicitement s’appuie sur une autre classe afin de fonctionner :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

Étant donné que la classe DinnerRepository requiert l’accès à une base de données, la dépendance fortement couplée la classe DinnersController a sur les terminaisons DinnerRepository demande de permis de disposer d’une base de données afin que les méthodes d’action DinnersController à tester.

Nous pouvons contourner ce problème en utilisant un modèle de conception appelé « injection de dépendance – » qui est une approche où les dépendances (telles que les classes du référentiel qui fournissent l’accès aux données) sont créées n’est plus implicitement dans les classes qui les utilisent. Au lieu de cela, des dépendances peuvent être explicitement transmis à la classe qui les utilise à l’aide des arguments de constructeur. Si les dépendances sont définies à l’aide des interfaces, nous avons ensuite la possibilité de transmettre des implémentations de dépendance « faux » pour les scénarios de test unitaire. Cela permet de créer des implémentations spécifiques aux tests dépendance qui ne nécessitent pas réellement d’accès à une base de données.

Pour voir cela en action, nous allons implémenter l’injection de dépendances avec notre DinnersController.

#### <a name="extracting-an-idinnerrepository-interface"></a>Extraction d’une interface IDinnerRepository

Notre première étape sera de créer une nouvelle interface IDinnerRepository qui encapsule le contrat de référentiel que nos contrôleurs nécessitent pour récupérer et mettre à jour dîners.

Nous pouvons définir ce contrat de l’interface manuellement en cliquant sur le dossier \Models, puis en choisissant le **Add -&gt;un nouvel élément** commande de menu et de la création d’une nouvelle interface nommée IDinnerRepository.cs.

Vous pouvez également utiliser l’extraction de refactorisation outils intégrés à Visual Studio Professional (et versions supérieures) pour automatiquement et nous créer une interface pour nous, à partir de notre classe DinnerRepository existante. Pour extraire cette interface à l’aide de Visual Studio, simplement positionner le curseur dans l’éditeur de texte sur la classe DinnerRepository, puis avec le bouton droit et choisissez le **refactoriser -&gt;extraire l’Interface** commande de menu :

![](enable-automated-unit-testing/_static/image7.png)

Cela lance la boîte de dialogue « Extraire l’Interface » et nous demander le nom de l’interface à créer. Il sera par défaut IDinnerRepository et sélectionner automatiquement toutes les méthodes publiques sur la classe DinnerRepository existante à ajouter à l’interface :

![](enable-automated-unit-testing/_static/image8.png)

Lorsque nous cliquons sur le bouton « OK », Visual Studio ajoute une nouvelle interface IDinnerRepository à notre application :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

Et notre classe DinnerRepository existante sera mise à jour afin qu’elle implémente l’interface :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>La mise à jour DinnersController pour prendre en charge l’injection de constructeur

Nous allons maintenant mettre à jour la classe DinnersController pour utiliser la nouvelle interface.

Actuellement DinnersController est codé en dur telles que son champ « dinnerRepository » est toujours une classe DinnerRepository :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

Nous allons le modifier afin que le champ « dinnerRepository » est de type IDinnerRepository au lieu de DinnerRepository. Nous allons ensuite ajouter deux constructeurs DinnersController publics. Un des constructeurs permet une IDinnerRepository à passer en tant qu’argument. L’autre est un constructeur par défaut qui utilise notre implémentation DinnerRepository existante :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

Étant donné que ASP.NET MVC par défaut crée des classes de contrôleur à l’aide de constructeurs par défaut, notre DinnersController lors de l’exécution continue à utiliser la classe DinnerRepository pour accéder aux données.

Nous pouvons maintenant mettre à jour nos tests d’unité, cependant, pour passer dans une implémentation de référentiel dîner « faux » en utilisant le paramètre de constructeur. Ce dépôt dîner « faux » ne nécessite pas d’accès à une base de données réelle et utilise à la place les données de l’exemple en mémoire.

#### <a name="creating-the-fakedinnerrepository-class"></a>Création de la classe FakeDinnerRepository

Nous allons créer une classe FakeDinnerRepository.

Nous allons commencer par créer un répertoire de « Fakes » au sein de notre projet NerdDinner.Tests et puis ajoutez-y une nouvelle classe FakeDinnerRepository à (avec le bouton droit sur le dossier et choisissez **Add -&gt;nouvelle classe**) :

![](enable-automated-unit-testing/_static/image9.png)

Nous mettrons à jour le code afin que la classe FakeDinnerRepository implémente l’interface IDinnerRepository. Nous pouvons ensuite avec le bouton droit dessus et choisissez la commande de menu contextuel « Implement interface IDinnerRepository » :

![](enable-automated-unit-testing/_static/image10.png)

Ainsi, Visual Studio pour tous les membres d’interface IDinnerRepository ajouter automatiquement à notre classe FakeDinnerRepository avec des implémentations par défaut « épargner » :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

Nous pouvons ensuite à jour l’implémentation FakeDinnerRepository pour travailler sur une liste en mémoire&lt;dîner&gt; collection passée comme un argument de constructeur :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

Nous disposons désormais d’une implémentation IDinnerRepository factice qui ne nécessite pas d’une base de données et peut fonctionner à la place une liste en mémoire des objets de Dinner.

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>À l’aide de la FakeDinnerRepository avec des Tests unitaires

Nous allons revenir aux tests unitaires DinnersController ayant échoué précédemment, car la base de données n’était pas disponible. Nous pouvons mettre à jour les méthodes de test pour utiliser un FakeDinnerRepository rempli avec les données de dîner exemple en mémoire pour le DinnersController en utilisant le code ci-dessous :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

Et maintenant quand nous exécutons ces tests à la fois transmettre :

![](enable-automated-unit-testing/_static/image11.png)

De plus, elles nécessitent uniquement une fraction de seconde à exécuter et ne nécessitent pas une logique complexe d’installation/nettoyage. Nous pouvons maintenant le test unitaire tout notre code de méthode action DinnersController (y compris la liste, la pagination, détails, créer, mettre à jour et supprimer) sans jamais avoir à se connecter à une base de données réel.

| **Côté rubrique : Frameworks d’Injection de dépendance** |
| --- |
| Exécution de l’injection de dépendances manuelle (par exemple, nous sommes ci-dessus) fonctionne correctement, mais est plus difficile à gérer en tant que le nombre de dépendances et les composants dans une application augmente. Plusieurs infrastructures d’injection de dépendance existent pour .NET, qui peuvent aider à offrir davantage de souplesse gestion dépendance. Ces infrastructures, également appelés conteneurs « Inversion de contrôle » (IoC), fournissent des mécanismes qui permettent à un niveau supplémentaire de prise en charge de configuration pour spécifier et en passant des dépendances à des objets lors de l’exécution (plus souvent à l’aide de l’injection de constructeur ). Parmi les plus populaires OSS l’Injection de dépendances / structures IOC dans .NET incluent : AutoFac, Ninject, Spring.NET, StructureMap et Windsor. ASP.NET MVC expose des API qui permettent aux développeurs de participer à la résolution et l’instanciation de contrôleurs, et ce qui permet l’Injection de dépendances d’extensibilité / structures IoC à intégrer correctement au sein de ce processus. À l’aide d’une infrastructure d’injection de dépendances/inversion de contrôle serait également nous permettent de supprimer le constructeur par défaut à partir de notre DinnersController – supprimer complètement le couplage entre elle et la DinnerRepositorys. Nous ne l’utiliserons une injection de dépendances / framework IOC avec notre application NerdDinner. Mais c’est quelque chose que nous pourrions considérer pour l’avenir, si la base de code de NerdDinner et fonctionnalités a augmenté. |

### <a name="creating-edit-action-unit-tests"></a>Création de Tests unitaires de Action modifier

Nous allons maintenant créer quelques tests unitaires qui vérifient les fonctionnalités de modification de la DinnersController. Nous allons commencer par tester la version de HTTP-GET de notre action de modification :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

Nous allons créer un test qui vérifie qu’une vue soutenue par un objet DinnerFormViewModel est restituée quand un dîner valid est demandé :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

Cependant, lorsque nous exécutons le test, nous y trouverez qu’elle échoue, car une exception NullReferenceException est levée lorsque la méthode Edit accède à la propriété User.Identity.Name pour effectuer la vérification Dinner.IsHostedBy().

L’objet User sur la classe de base de contrôleur contient des informations sur l’utilisateur connecté et est rempli par ASP.NET MVC lorsqu’il crée le contrôleur lors de l’exécution. Étant donné que nous testons la DinnersController en dehors d’un environnement de serveur web, l’objet utilisateur n’est pas défini (par conséquent, l’exception de référence null).

### <a name="mocking-the-useridentityname-property"></a>La propriété User.Identity.Name de simulation

Infrastructures factices faciliter le test en nous permettant de créer dynamiquement des versions fictives des objets dépendants qui prennent en charge de nos tests. Par exemple, nous pouvons utiliser une infrastructure de simulation dans notre test d’action de modification pour créer dynamiquement un objet utilisateur que notre DinnersController peut utiliser pour rechercher un nom d’utilisateur simulé. Cela permet d’éviter une référence null ne soit levée lorsque nous exécutons notre test.

Il existe de nombreuses .NET frameworks peuvent être utilisées avec ASP.NET MVC de simulation (vous pouvez voir une liste ici : [ http://www.mockframeworks.com/ ](http://www.mockframeworks.com/)). Pour tester notre application NerdDinner que nous allons utiliser un framework appelée « Moq » de simulation, open source, qui peut être téléchargé gratuitement à partir de [ http://www.mockframeworks.com/moq ](http://www.mockframeworks.com/moq).

Une fois téléchargé, nous allons ajouter une référence dans notre projet NerdDinner.Tests à l’assembly Moq.dll :

![](enable-automated-unit-testing/_static/image12.png)

Nous allons ensuite ajouter une méthode d’assistance de « CreateDinnersControllerAs(username) » à notre classe de test qui prend un nom d’utilisateur comme paramètre et qui, puis la propriété User.Identity.Name sur l’instance DinnersController « mocks » :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

Ci-dessus, nous utilisons Moq pour créer un objet fictif qui imite un objet ControllerContext (qui est ce que ASP.NET MVC passe aux classes de contrôleur pour exposer des objets runtime tels que l’utilisateur, demande, réponse et Session). Nous allons appeler la méthode « SetupGet » sur le simulacre pour indiquer que la propriété HttpContext.User.Identity.Name sur ControllerContext doit retourner la chaîne de nom d’utilisateur que nous avons transmis à la méthode d’assistance.

Nous pouvons simuler n’importe quel nombre de ControllerContext propriétés et méthodes. Pour illustrer ce propos, j’ai également ajouté un appel SetupGet() pour la propriété Request.IsAuthenticated (qui n’est pas réellement nécessaire pour les tests ci-dessous – mais qui permet d’illustrer comment vous pouvez simuler les propriétés de la demande). Lorsque nous avons terminé nous affectons une instance de l’objet factice ControllerContext à la DinnersController notre méthode d’assistance est retournée.

Nous pouvons maintenant écrire des tests unitaires qui utilisent cette méthode d’assistance pour tester les scénarios de modification impliquant des différents utilisateurs :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

Et maintenant lorsque nous exécutons les tests passent :

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>Scénarios de test UpdateModel()

Nous avons créé des tests qui couvrent la version de HTTP-GET de l’action de modification. Nous allons maintenant créer des tests qui vérifient la version de HTTP-POST de l’action de modification :

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

Le nouveau scénario test intéressant pour nous permettre de prendre en charge avec cette méthode d’action est son utilisation de la méthode d’assistance UpdateModel() sur la classe de base du contrôleur. Nous utilisons cette méthode d’assistance pour lier les valeurs de publication de formulaire à notre instance d’objet dîner.

Voici deux tests qui montre comment nous pouvons fournir formulaire publié des valeurs pour la méthode d’assistance UpdateModel() à utiliser. Nous allons faire cela en créant et en remplissant un objet FormCollection et puis l’affecter à la propriété « ValueProvider » sur le contrôleur.

Le premier test vérifie qu’une sauvegarde réussie, le navigateur est redirigé à l’action de détails. Le second test vérifie que lors de la validation d’entrée non valide l’action réaffiche la vue edit à nouveau avec un message d’erreur.


[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>Conclusion de test

Nous avons couvert les concepts principaux impliqués dans les classes de contrôleur de test unitaire. Nous pouvons utiliser ces techniques pour créer facilement des centaines de tests simples qui vérifient le comportement de notre application.

Étant donné que notre contrôleur et les tests de modèle ne nécessitent pas une véritable base de données, elles sont extrêmement rapide et facile à exécuter. Nous serons en mesure d’exécuter des centaines de tests automatisés en quelques secondes et obtenez immédiatement des commentaires concernant une modification que nous avons apporté quelque chose s’est arrêtée si. Cela vous aidera à nous faire part de continuellement améliorer, refactoriser et à affiner notre application en toute confiance.

Nous avons abordé le test en tant que la dernière rubrique de ce chapitre – mais pas parce que le test est quelque chose doit procéder à la fin d’un processus de développement ! Au contraire, vous devez écrire des tests automatisés dès que possible dans votre processus de développement. Cette opération vous permet obtenir des commentaires immédiats lorsque vous développez, vous permet de pensez réfléchie de scénarios de cas d’utilisation de votre application et vous guide pour concevoir votre application avec nettoyage superposition et n’oubliez pas de couplage.

Un chapitre plus loin dans le livre décrit développement piloté par des tests (TDD) et comment l’utiliser avec ASP.NET MVC. TDD est une pratique de codage itérative dans lequel vous écrivez d’abord les tests répondant à votre code qui en résulte. Avec TDD commencer chaque fonctionnalité en créant un test qui vérifie les fonctionnalités que vous allez implémenter. Écriture de l’unité de test permet tout d’abord vous assurer que vous comprenez clairement la fonctionnalité et la façon dont elle est censée pour fonctionner. Une fois que le test est écrit (et que vous avez vérifié qu’il échoue) ne vous puis implémenter les fonctionnalités réelles, que le test vérifie. Étant donné que vous avez déjà passé du temps à réfléchir sur le cas d’usage de la façon dont la fonction est censée pour fonctionner, vous aurez une meilleure compréhension des exigences et comment mieux pour les implémenter. Lorsque vous avez terminé avec l’implémentation, vous pouvez réexécuter le test – et obtenir des retours immédiats en tant que pour que la fonctionnalité fonctionne correctement. Nous traiterons TDD plus dans le chapitre 10.

### <a name="next-step"></a>Étape suivante

Certains wrap finale des commentaires.

> [!div class="step-by-step"]
> [Précédent](use-ajax-to-implement-mapping-scenarios.md)
> [Suivant](nerddinner-wrap-up.md)
