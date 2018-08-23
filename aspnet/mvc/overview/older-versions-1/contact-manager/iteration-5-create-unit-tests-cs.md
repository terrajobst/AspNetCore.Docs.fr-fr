---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-cs
title: 'Itération #5 : créer des tests unitaires (c#) | Microsoft Docs'
author: microsoft
description: Dans la cinquième itération, nous faciliter notre application mettre à jour et modifier en ajoutant des tests unitaires. Nous simuler nos classes de modèle de données et générer des tests unitaires pour o...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 28ad8f80-b8a5-444e-b478-8b15a846060c
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-cs
msc.type: authoredcontent
ms.openlocfilehash: 68d0ae15db115685b3e1a44f8b3b5b7e33674a8b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826812"
---
<a name="iteration-5--create-unit-tests-c"></a>Itération #5 : créer des tests unitaires (c#)
====================
par [Microsoft](https://github.com/microsoft)

[Télécharger le Code](iteration-5-create-unit-tests-cs/_static/contactmanager_5_cs1.zip)

> Dans la cinquième itération, nous faciliter notre application mettre à jour et modifier en ajoutant des tests unitaires. Nous simuler nos classes de modèle de données et générer des tests unitaires pour nos contrôleurs et la logique de validation.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Création d’une Application ASP.NET MVC de gestion des contacts (c#)

Dans cette série de didacticiels, nous créer une application de gestion des contacts entière à partir du début à la fin. L’application Gestionnaire de Contact permet vous permettent de stocker les informations de contact (noms, numéros de téléphone et adresses de messagerie) pour obtenir la liste de personnes.

Nous générer l’application sur de multiples itérations. Avec chaque itération, nous améliorer progressivement l’application. L’objectif de cette approche à plusieurs itération consiste à vous permettre de comprendre la raison de chaque modification.

- Itération #1 : créer l’application. Dans la première itération, nous créons le Gestionnaire de Contact de la façon la plus simple possible. Nous ajoutons la prise en charge pour les opérations de base de données : création, lecture, mise à jour et suppression (CRUD).

- Itération #2 : donner l’application une apparence intéressante. Dans cette itération, nous améliorer l’apparence de l’application en modifiant la valeur par défaut de page maître de vue ASP.NET MVC et en cascade de feuille de style.

- Itération #3 - Ajouter une validation de formulaire. Dans la troisième itération, nous ajouter la validation de formulaire de base. Nous empêcher des personnes à partir de l’envoi d’un formulaire sans compléter les champs obligatoires. Nous avons également valider que les adresses de messagerie et les numéros de téléphone.

- Itération #4 : rendre l’application faiblement couplée. Dans ce quatrième itération, nous profitons de plusieurs modèles de conception de logiciels pour le rendre plus facile à gérer et modifier l’application Gestionnaire de contacts. Par exemple, nous refactoriser notre application pour utiliser le modèle dépôt et le modèle d’Injection de dépendance.

- Itération #5 - créer des tests unitaires. Dans la cinquième itération, nous faciliter notre application mettre à jour et modifier en ajoutant des tests unitaires. Nous simuler nos classes de modèle de données et générer des tests unitaires pour nos contrôleurs et la logique de validation.

- Itération #6 - utiliser le développement piloté par test. Dans cette itération sixième, nous ajoutons les nouvelles fonctionnalités à notre application en écrivant des tests unitaires tout d’abord et écrire du code pour les tests unitaires. Dans cette itération, nous ajouter des groupes de contacts.

- Itération #7 - ajouter des fonctionnalités Ajax. Dans l’itération septième, nous améliorer la réactivité et les performances de notre application en ajoutant la prise en charge d’Ajax.


## <a name="this-iteration"></a>Cette itération

Dans l’itération précédente de l’application Gestionnaire de contacts, nous avons refactorisé l’application à être plus faiblement couplées. Nous avons séparé de l’application dans le contrôleur distincte, service et les couches de référentiel. Chaque couche interagit avec la couche en dessous via des interfaces.

Nous avons refactorisé l’application pour rendre l’application plus facile à maintenir et à modifier. Par exemple, si nous devons utiliser une nouvelle technologie d’accès aux données, nous pouvons simplement modifier la couche de référentiels sans toucher le contrôleur ou une couche de service. En rendant le Gestionnaire de contacts faiblement couplés et nous sauter l’application plus résiliente à modifier.

Mais, que se passe-t-il lorsque nous devons ajouter une nouvelle fonctionnalité à l’application Gestionnaire de contacts ? Ou bien, que se passe-t-il quand nous corriger un bogue ? Une vérité triste, mais également éprouvée, d’écriture de code est que chaque fois que vous touchez le code que vous créez le risque d’introduire de nouveaux bogues.

Par exemple, un jour précis, votre gestionnaire peut vous demander pour ajouter une nouvelle fonctionnalité au Gestionnaire de contacts. Elle souhaite vous permet d’ajouter la prise en charge pour les groupes de Contact. Elle vous demande de permettre aux utilisateurs d’organiser leurs contacts en groupes tels que des amis, entreprise et ainsi de suite.

Pour implémenter cette nouvelle fonctionnalité, vous devez modifier tous les trois couches de l’application Gestionnaire de contacts. Vous devez ajouter de nouvelles fonctionnalités pour les contrôleurs, la couche de service et le référentiel. Dès que vous commencez à modifier de code, vous risquez de rompre les fonctionnalités qui fonctionnaient avant.

Refactorisation de notre application en couches distinctes, comme nous l’avons fait dans l’itération précédente, était une bonne chose. C’était une bonne chose car elle permet d’apporter des modifications aux couches entières sans toucher le reste de l’application. Toutefois, si vous souhaitez rendre le code au sein d’une couche plus facile à maintenir et à modifier, vous devez créer des tests unitaires pour le code.

Vous utilisez une unité de test à une unité individuelle de code. Ces unités de code sont plus petites que les couches application dans son intégralité. En général, vous utilisez un test unitaire pour vérifier si une méthode particulière dans votre code se comporte de la façon que vous attendez. Par exemple, vous créeriez un test unitaire pour la méthode CreateContact() exposé par la classe ContactManagerService.

Les tests unitaires pour un travail d’application simplement comme filet de sécurité. Chaque fois que vous modifiez le code dans une application, vous pouvez exécuter un ensemble de tests unitaires pour vérifier si la modification s’interrompt les fonctionnalités existantes. Tests unitaires rendent votre code est déconseillé de modifier. Tests unitaires que tout le code dans votre application plus résiliente à modifier.

Dans cette itération, nous ajoutons des tests unitaires à notre application de gestionnaire de contacts. De cette façon, dans l’itération suivante, nous pouvons ajouter des groupes de Contact à notre application sans vous soucier de compromettre les fonctionnalités existantes.

> [!NOTE] 
> 
> Il existe une variété de notamment NUnit et xUnit.net MbUnit des infrastructures de tests unitaires. Dans ce didacticiel, nous utilisons framework inclus avec Visual Studio de test unitaire. Toutefois, vous pouvez tout aussi facilement utiliser une de ces autres infrastructures.


## <a name="what-gets-tested"></a>Ce qui est testé

Dans le monde parfait, ensemble de votre code sont couverts par les tests unitaires. Dans le monde parfait, vous devez le filet parfait. Vous ne pourrez pas modifier n’importe quelle ligne de code dans votre application et de connaître instantanément, en exécutant vos tests unitaires, si la modification s’est arrêtée de fonctionnalités existantes.

Toutefois, nous ne t en direct dans un monde parfait. Dans la pratique, lors de l’écriture de tests unitaires, vous vous concentrer sur l’écriture de tests pour votre logique métier (par exemple, logique de validation). En particulier, vous *ne le faites pas* écrire des tests unitaires pour vos données d’accès logique ou votre logique d’affichage.

Pour être utile, les tests unitaires doivent s’exécuter très rapidement. Vous pouvez facilement accumuler des centaines (ou même des milliers) de tests unitaires pour une application. Si les tests unitaires prennent beaucoup de temps pour exécuter puis vous éviterez les exécuter. En d’autres termes, les tests unitaires longue sont inutiles pour le jour à l’autre à des fins de codage.

Pour cette raison, vous en général, n’écrivez pas de tests unitaires pour du code qui interagit avec une base de données. En cours d’exécution des centaines de tests unitaires sur une base de données en direct seraient trop lents. Au lieu de cela, vous simuler votre base de données et écrivez du code qui interagit avec la base de données factice (nous y reviendrons simulation d’une base de données ci-dessous).

Pour des raisons similaires, en général, ne pas écrire les tests unitaires pour les vues. Pour tester une vue, vous devez activer un serveur web. Étant donné que la mise en place d’un serveur web est un processus relativement lent, la création de tests unitaires pour les vues n’est pas recommandée.

Si votre vue contient une logique complexe vous devez envisager le déplacement de la logique dans les méthodes d’assistance. Vous pouvez écrire des tests unitaires pour les méthodes d’assistance qui s’exécutent sans mise en place d’un serveur web.

> [!NOTE] 
> 
> Alors que l’écriture de tests pour la logique d’accès aux données ou logique d’affichage n’est pas une bonne idée lors de l’écriture de tests unitaires, ces tests peuvent être très utiles lors de la construction fonctionnelle ou l’intégration des tests.


> [!NOTE] 
> 
> ASP.NET MVC est le moteur d’affichage Web Forms. Alors que le moteur d’affichage Web Forms est dépendant sur un serveur web, les autres moteurs d’affichage ne peuvent pas être.


## <a name="using-a-mock-object-framework"></a>À l’aide d’une infrastructure de l’objet factice

Lors de la génération des tests unitaires, vous devez presque toujours tirer parti d’une infrastructure de simuler un objet. Une infrastructure de simuler un objet vous permet de créer des simulations et stubs pour les classes dans votre application.

Par exemple, vous pouvez utiliser une infrastructure de simuler un objet pour générer une version de votre classe de référentiel fictive. De cette façon, vous pouvez utiliser la classe de référentiel factice au lieu de la classe de dépôt réelles dans vos tests unitaires. Utiliser le référentiel factice vous permet d’éviter l’exécution de code de base de données lors de l’exécution d’un test unitaire.

Visual Studio n’inclut pas d’une infrastructure de simuler un objet. Toutefois, il existe plusieurs infrastructures de simuler un objet commerciaux et open source disponibles pour le .NET framework :

1. Moq - cette infrastructure est disponible sous licence BSD open source. Vous pouvez télécharger Moq de [ https://code.google.com/p/moq/ ](https://code.google.com/p/moq/).
2. Rhino Mocks, cette infrastructure est disponible sous licence BSD open source. Vous pouvez télécharger Rhino Mocks de [ http://ayende.com/projects/rhino-mocks.aspx ](http://ayende.com/projects/rhino-mocks.aspx).
3. Typemock Isolator - il s’agit d’un cadre commercial. Vous pouvez télécharger une version d’essai [ http://www.typemock.com/ ](http://www.typemock.com/).

Dans ce didacticiel, j’ai décidé d’utiliser Moq. Toutefois, vous pouvez tout aussi facilement utiliser Rhino Mocks ou Typemock Isolator pour créer le simulacre objets pour l’application Gestionnaire de contacts.

Avant de pouvoir utiliser Moq, vous devez suivre les étapes suivantes :

1. .
2. Avant de vous décompressez le téléchargement, assurez-vous que vous cliquez sur le fichier et cliquez sur le bouton intitulé **Unblock** (voir Figure 1).
3. Décompressez le téléchargement.
4. Ajoutez une référence à l’assembly Moq en double-cliquant sur le dossier références dans le projet ContactManager.Tests et en sélectionnant **ajouter une référence**. Sous l’onglet Parcourir, accédez au dossier où vous avez décompressé Moq et sélectionnez l’assembly Moq.dll. Cliquez sur le **OK** bouton.
5. Après avoir effectué ces étapes, votre dossier références doit ressembler à la Figure 2.


[![Déblocage Moq](iteration-5-create-unit-tests-cs/_static/image1.jpg)](iteration-5-create-unit-tests-cs/_static/image1.png)

**Figure 01**: Moq déblocage ([cliquez pour afficher l’image en taille réelle](iteration-5-create-unit-tests-cs/_static/image2.png))


[![Références après l’ajout de Moq](iteration-5-create-unit-tests-cs/_static/image2.jpg)](iteration-5-create-unit-tests-cs/_static/image3.png)

**Figure 02**: références après l’ajout de Moq ([cliquez pour afficher l’image en taille réelle](iteration-5-create-unit-tests-cs/_static/image4.png))


## <a name="creating-unit-tests-for-the-service-layer"></a>Création de Tests unitaires pour la couche de Service

Permettent de commencer par créer un ensemble de tests unitaires pour notre couche de service de gestionnaire de contacts application s s. Nous allons utiliser ces tests pour vérifier notre logique de validation.

Créez un dossier nommé Models dans le projet ContactManager.Tests. Ensuite, cliquez sur le dossier Modèles et sélectionnez **ajouter, nouveau Test**. Le **ajouter un nouveau Test** boîte de dialogue illustrée dans la Figure 3 s’affiche. Sélectionnez le **de Test unitaire** modèle et nommez votre nouveau test ContactManagerServiceTest.cs. Cliquez sur le **OK** pour ajouter votre nouveau test à votre projet de Test.

> [!NOTE] 
> 
> En général, vous souhaitez que la structure de dossiers de votre projet de Test pour correspondre à la structure de dossiers de votre projet ASP.NET MVC. Par exemple, vous placez des tests du contrôleur dans un dossier contrôleurs, les tests de modèle dans un dossier de modèles et ainsi de suite.


[![Models\ContactManagerServiceTest.cs](iteration-5-create-unit-tests-cs/_static/image3.jpg)](iteration-5-create-unit-tests-cs/_static/image5.png)

**Figure 03**: Models\ContactManagerServiceTest.cs ([cliquez pour afficher l’image en taille réelle](iteration-5-create-unit-tests-cs/_static/image6.png))


Au départ, nous souhaitons tester la méthode CreateContact() exposée par la classe ContactManagerService. Nous allons créer les cinq tests suivants :

- CreateContact() - Tests ce CreateContact() retourne la valeur true quand un Contact valid est passé à la méthode.
- CreateContactRequiredFirstName() - Tests qu’un message d’erreur est ajouté à l’état de modèle lorsqu’un Contact avec un prénom manquant est transmis à la méthode CreateContact().
- CreateContactRequredLastName() - Tests qu’un message d’erreur est ajouté à l’état de modèle lorsqu’un Contact avec un nom de famille manquant est transmis à la méthode CreateContact().
- CreateContactInvalidPhone() - Tests qu’un message d’erreur est ajouté à l’état de modèle lorsqu’un Contact avec un numéro de téléphone non valide est passé à la méthode CreateContact().
- CreateContactInvalidEmail() - Tests qu’un message d’erreur est ajouté à l’état de modèle lorsqu’un Contact avec une adresse de messagerie non valide est passé à la méthode CreateContact()...

Le premier test vérifie qu’un Contact valid ne génère pas d’une erreur de validation. Les tests restants vérifient chacune des règles de validation.

Le code pour ces tests est contenu dans le Listing 1.

**Liste 1 - Models\ContactManagerServiceTest.cs**

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample1.cs)]


Étant donné que nous utilisons la classe Contact dans le Listing 1, nous devons ajouter une référence à Microsoft Entity Framework à notre projet de Test. Ajoutez une référence à l’assembly System.Data.Entity.


Listing 1 contient une méthode nommée Initialize() est décorée avec l’attribut [TestInitialize]. Cette méthode est appelée automatiquement avant l’exécution chacun des tests unitaires (elle est appelée 5 fois juste avant chaque test unitaire). La méthode Initialize() crée un référentiel factice avec la ligne de code suivante :

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample2.cs)]

Cette ligne de code utilise le framework Moq pour générer un référentiel factice à partir de l’interface IContactManagerRepository. Le référentiel factice est utilisé au lieu du EntityContactManagerRepository réel afin d’éviter l’accès à la base de données lors de chaque test unitaire est exécuté. Le référentiel fictif implémente les méthodes de l’interface IContactManagerRepository, mais le t de don méthodes fait rien.

> [!NOTE] 
> 
> Lorsque vous utilisez le framework Moq, il existe une distinction entre \_mockRepository et \_mockRepository.Object. L’ancienne base de données fait référence au simulacre&lt;IContactManagerRepository&gt; classe qui contient des méthodes permettant de spécifier le comporte du référentiel factice. Ce dernier fait référence au référentiel fictif réels qui implémente l’interface IContactManagerRepository.


Le référentiel factice est utilisé dans la méthode Initialize() lors de la création d’une instance de la classe ContactManagerService. Tous les tests unitaires individuels d’utilisent cette instance de la classe ContactManagerService.

Listing 1 contient cinq méthodes qui correspondent à chacun des tests unitaires. Chacune de ces méthodes est décorée avec l’attribut [TestMethod]. Lorsque vous exécutez les tests unitaires, n’importe quelle méthode possédant cet attribut est appelée. En d’autres termes, toute méthode qui est décorée avec l’attribut [TestMethod] est un test unitaire.

Le premier test unitaire, nommé CreateContact(), vérifie que l’appelant CreateContact() retourne la valeur true quand une instance de la classe Contact valide est passée à la méthode. Le test crée une instance de la classe Contact, appelle la méthode CreateContact() et vérifie que CreateContact() retourne la valeur true.

Les tests restants vérifient que lorsque la méthode CreateContact() est appelée avec un Contact non valide la méthode retourne la valeur false, puis le message d’erreur de validation attendu est ajouté à l’état du modèle. Par exemple, le test CreateContactRequiredFirstName() crée une instance de la classe Contact avec une chaîne vide pour sa propriété FirstName. Ensuite, la méthode CreateContact() est appelée avec le Contact non valide. Enfin, le test vérifie que CreateContact() retourne la valeur false et qu’état du modèle contient le message d’erreur de validation attendu « prénom est obligatoire. »

Vous pouvez exécuter les tests unitaires dans le Listing 1 en sélectionnant l’option de menu **série de tests, tous les Tests de la Solution (CTRL + R, A)**. Les résultats des tests sont affichés dans la fenêtre Résultats des tests (voir Figure 4).


[![Résultats des tests](iteration-5-create-unit-tests-cs/_static/image4.jpg)](iteration-5-create-unit-tests-cs/_static/image7.png)

**Figure 04**: résultats des tests ([cliquez pour afficher l’image en taille réelle](iteration-5-create-unit-tests-cs/_static/image8.png))


## <a name="creating-unit-tests-for-controllers"></a>Création de Tests unitaires pour les contrôleurs

Application de ASP.NETMVC contrôlent le flux d’interaction utilisateur. Lorsque vous testez un contrôleur, vous souhaitez tester si le contrôleur retourne l’action bon résultat et afficher les données. Vous pouvez également vérifier si un contrôleur interagit avec les classes de modèle de la manière attendue.

Par exemple, le Listing 2 contient deux tests unitaires pour le contrôleur de Contact méthode Create(). Le premier test unitaire vérifie que lorsqu’un Contact valid est passé à la méthode Create() ensuite la méthode Create() redirige vers l’action de l’Index. En d’autres termes, quand il est passé à un Contact valid, la méthode Create() doit retourner un RedirectToRouteResult qui représente l’action de l’Index.

Nous ne souhaitez t pour tester la couche de service ContactManager lorsque nous testons la couche de contrôleur. Par conséquent, nous simuler la couche de service avec le code suivant dans la méthode Initialize :

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample3.cs)]

Dans le test unitaire CreateValidContact(), nous simuler le comportement de l’appel de la couche de service méthode CreateContact() avec la ligne de code suivante :

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample4.cs)]

Cette ligne de code, le service de ContactManager factice retourner la valeur true quand sa méthode CreateContact() est appelée. Par la couche de service de simulation, nous pouvons tester le comportement de notre contrôleur sans avoir besoin d’exécuter du code dans la couche de service.

Le deuxième test unitaire vérifie que l’action Create() retourne la vue Create lorsqu’un contact non valide est passé à la méthode. Nous provoquer la couche de service CreateContact() méthode retourne la valeur false avec la ligne de code suivante :

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample5.cs)]

Si la méthode Create() se comporte comme prévu, il doit renvoyer la vue Create lors de la couche de service retourne la valeur false. De cette façon, le contrôleur peut afficher les messages d’erreur de validation dans la vue de créer et de l’utilisateur a la possibilité de corriger ce Contact de propriétés non valides.


Si vous envisagez de générer des tests unitaires pour vos contrôleurs vous devez retourner les noms de vue explicite à partir de vos actions de contrôleur. Par exemple, ne retournent pas d’une vue comme suit :

retourner View() ;

Retourner à la place, la vue comme suit :

retourner View("Create") ;

Si vous n’êtes pas explicite lors du retour d’une vue de la propriété ViewResult.ViewName retourne une chaîne vide.


**Listing 2 - Controllers\ContactControllerTest.cs**

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample6.cs)]

## <a name="summary"></a>Récapitulatif

Dans cette itération, nous avons créé des tests unitaires pour notre application de gestionnaire de contacts. Nous pouvons exécuter ces tests unitaires à tout moment pour vérifier que notre application se comporte toujours de la manière que nous prévoyons. Les tests unitaires agissent comme un filet de sécurité pour notre application nous permettant de modifier en toute sécurité de notre application à l’avenir.

Nous avons créé deux jeux de tests unitaires. Tout d’abord, nous avons testé notre logique de validation en créant des tests unitaires pour notre couche de service. Ensuite, nous avons testé notre logique de contrôle de flux en créant des tests unitaires pour la couche de notre contrôleur. Lorsque vous testez la couche de notre service, nous isolé nos tests pour notre couche de service à partir de la couche de nos référentiels par simulation notre couche de référentiels. Lorsque vous testez la couche de contrôleur, nous isolé nos tests pour la couche de notre contrôleur par la couche de service de simulation.

Dans l’itération suivante, nous modifier l’application Gestionnaire de contacts afin qu’il prend en charge les groupes de Contact. Nous allons ajouter cette nouvelle fonctionnalité à notre application à l’aide d’un processus de conception de logiciel appelé développement piloté par test.

> [!div class="step-by-step"]
> [Précédent](iteration-4-make-the-application-loosely-coupled-cs.md)
> [Suivant](iteration-6-use-test-driven-development-cs.md)
