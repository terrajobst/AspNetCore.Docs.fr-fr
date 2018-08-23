---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
title: 'Itération #4 : rendre l’application faiblement couplée (c#) | Microsoft Docs'
author: microsoft
description: Dans ce quatrième itération, nous profitons de plusieurs modèles de conception de logiciels pour le rendre plus facile à gérer et modifier l’application Gestionnaire de contacts. Pré...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 829f589f-e201-4f6e-9ae6-08ae84322065
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-cs
msc.type: authoredcontent
ms.openlocfilehash: 955991c148d48020c66cbc63ec6da45e3a9cc282
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836778"
---
<a name="iteration-4--make-the-application-loosely-coupled-c"></a>Itération #4 : rendre l’application faiblement couplée (c#)
====================
par [Microsoft](https://github.com/microsoft)

[Télécharger le Code](iteration-4-make-the-application-loosely-coupled-cs/_static/contactmanager_4_cs1.zip)

> Dans ce quatrième itération, nous profitons de plusieurs modèles de conception de logiciels pour le rendre plus facile à gérer et modifier l’application Gestionnaire de contacts. Par exemple, nous refactoriser notre application pour utiliser le modèle dépôt et le modèle d’Injection de dépendance.


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

Dans ce quatrième itération de l’application Gestionnaire de contacts, nous refactoriser l’application pour rendre l’application plus faiblement couplée. Lorsqu’une application est faiblement couplée, vous pouvez modifier le code dans une partie de l’application sans avoir à modifier le code dans d’autres parties de l’application. Applications faiblement couplées sont plus résilientes à modifier.

Actuellement, toute la logique d’accès et de validation de données utilisée par l’application Gestionnaire de contacts est contenue dans les classes de contrôleur. Il s’agit d’une mauvaise idée. Chaque fois que vous avez besoin modifier une partie de votre application, vous risquez d’introduire des bogues dans une autre partie de votre application. Par exemple, si vous modifiez votre logique de validation, vous risquez introduire de nouveaux bogues dans votre logique d’accès ou un contrôleur de données.

> [!NOTE] 
> 
> (SRP), une classe doit avoir jamais plus d’une raison de changer. Mélange de contrôleur, validation et une logique de base de données est une violation massive du principe de responsabilité unique.


Il existe plusieurs raisons, vous devrez peut-être modifier votre application. Vous devrez peut-être ajouter une nouvelle fonctionnalité à votre application, vous devrez peut-être corriger un bogue dans votre application, ou vous devrez peut-être modifier la façon dont une fonctionnalité de votre application est implémentée. Les applications sont rarement statiques. Ils ont tendance à croître et muter au fil du temps.

Par exemple, imaginez que vous décidez de modifier la façon dont vous implémentez votre couche d’accès aux données. Droite, l’application Gestionnaire de contacts utilise désormais Microsoft Entity Framework pour accéder à la base de données. Toutefois, vous pouvez décider de migrer vers une technologie d’accès aux données nouvelle ou sur un autre telles que ADO.NET Data Services ou NHibernate. Toutefois, étant donné que le code d’accès aux données n’est pas isolé à partir du code de validation et de contrôleur, il n’existe aucun moyen de modifier le code d’accès aux données dans votre application sans modifier d’autres codes qui n’est pas directement liée à l’accès aux données.

Lorsqu’une application est faiblement couplée et quant à eux, vous pouvez apporter des modifications à une partie d’une application sans toucher aux autres parties d’une application. Par exemple, vous pouvez basculer des technologies d’accès aux données sans modifier votre logique de validation ou de contrôleur.

Dans cette itération, nous profitons de plusieurs modèles de conception de logiciel qui nous permettent de refactoriser notre application de gestionnaire de contacts dans une application plus faiblement couplée. Lorsque nous avons terminé, le Gestionnaire de Contact a gagné t faire quoi que ce soit il n’a pas à le faire avant. Toutefois, nous serons en mesure de modifier l’application plus facilement à l’avenir.

> [!NOTE] 
> 
> Refactorisation est le processus de réécriture d’une application de sorte qu’il ne perd pas toutes les fonctionnalités existantes.


## <a name="using-the-repository-software-design-pattern"></a>À l’aide du modèle de conception de logiciel de référentiel

Notre premier changement consiste à tirer parti d’un modèle de conception de logiciel appelé le modèle de référentiel. Nous allons utiliser le modèle de référentiel pour isoler notre code d’accès aux données du reste de notre application.

Implémentation du modèle de référentiel nous oblige à effectuer les deux étapes suivantes :

1. Créer une interface
2. Créer une classe concrète qui implémente l’interface

Tout d’abord, nous devons créer une interface qui décrit toutes les méthodes d’accès de données que nous devons effectuer. L’interface IContactManagerRepository est contenue dans le Listing 1. Cette interface décrit les cinq méthodes : CreateContact() DeleteContact(), EditContact(), GetContact et ListContacts().

**Liste 1 - Models\IContactManagerRepositiory.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample1.cs)]

Ensuite, nous devons créer une classe concrète qui implémente l’interface IContactManagerRepository. Étant donné que nous utilisons Microsoft Entity Framework pour accéder à la base de données, nous allons créer une nouvelle classe nommée EntityContactManagerRepository. Cette classe est contenue dans le Listing 2.

**Listing 2 - Models\EntityContactManagerRepository.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample2.cs)]

Notez que la classe EntityContactManagerRepository implémente l’interface IContactManagerRepository. La classe implémente toutes les cinq méthodes décrites par cette interface.

Vous vous demandez peut-être pourquoi nous devons s’embêter avec une interface. Pourquoi nous devons créer une interface et une classe qui l’implémente ?

Avec une exception, le reste de notre application interagit avec l’interface et pas la classe concrète. Au lieu d’appeler les méthodes exposées par la classe EntityContactManagerRepository, nous allons appeler les méthodes exposées par l’interface IContactManagerRepository.

De cette façon, nous pouvons implémenter l’interface avec une nouvelle classe sans avoir à modifier le reste de notre application. Par exemple, à une date future, nous pourrions implémenter une classe DataServicesContactManagerRepository qui implémente l’interface IContactManagerRepository. La classe DataServicesContactManagerRepository peut utiliser ADO.NET Data Services pour accéder à une base de données au lieu de Microsoft Entity Framework.

Si notre code d’application est programmé par rapport à l’interface IContactManagerRepository au lieu de la classe EntityContactManagerRepository concrète nous pouvons basculer les classes concrètes sans modifier la partie de notre code. Par exemple, nous pouvons basculer à partir de la classe EntityContactManagerRepository à la classe DataServicesContactManagerRepository sans modifier notre logique d’accès ou de validation de données.

La programmation dans les interfaces (abstractions) au lieu de classes concrètes rend notre application plus résiliente à modifier.

> [!NOTE] 
> 
> Vous pouvez rapidement créer une interface à partir d’une classe concrète dans Visual Studio en sélectionnant l’option de menu refactoriser, extraire l’Interface. Par exemple, vous pouvez créez d’abord la classe EntityContactManagerRepository et ensuite utiliser extraire l’Interface pour générer l’interface IContactManagerRepository automatiquement.


## <a name="using-the-dependency-injection-software-design-pattern"></a>À l’aide du modèle de conception de logiciels d’Injection dépendance

Maintenant que nous avons migré notre code d’accès aux données à une classe distincte de référentiel, nous devons modifier notre contrôleur de Contact pour utiliser cette classe. Nous tirera parti d’un modèle de conception de logiciel appelé Injection de dépendance pour utiliser la classe de dépôt dans notre contrôleur.

Le contrôleur de Contact modifié est contenu dans le Listing 3.

**Liste 3 - Controllers\ContactController.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample3.cs)]

Notez que le contrôleur de Contact dans la liste 3 possède deux constructeurs. Le premier constructeur passe une instance concrète de l’interface IContactManagerRepository pour le deuxième constructeur. La classe de contrôleur Contact utilise *l’Injection de dépendances de constructeur*.

Celle et uniquement sur place que la classe EntityContactManagerRepository est utilisée est le premier constructeur. Le reste de la classe utilise l’interface IContactManagerRepository au lieu de la classe EntityContactManagerRepository concrète.

Cela rend plus facile basculer les implémentations de la classe IContactManagerRepository à l’avenir. Si vous souhaitez utiliser la classe DataServicesContactRepository au lieu de la classe EntityContactManagerRepository, modifiez simplement le premier constructeur.

L’injection de dépendances de constructeur permet également de la classe de contrôleur Contact très faciles à tester. Dans vos tests unitaires, vous pouvez instancier le contrôleur de Contact en passant une implémentation fictive de la classe IContactManagerRepository. Cette fonctionnalité d’Injection de dépendances sera grande importance dans l’itération suivante lorsque nous générons des tests unitaires pour l’application Gestionnaire de contacts.

> [!NOTE] 
> 
> Si vous souhaitez complètement découpler la classe de contrôleur de Contact à partir d’une implémentation particulière de l’interface IContactManagerRepository vous pouvez tirer parti d’une infrastructure qui prend en charge l’Injection de dépendances telles que StructureMap ou Microsoft Entity Framework (MEF). En tirant parti d’une infrastructure d’Injection de dépendance, vous devez jamais faire référence à une classe concrète dans votre code.


## <a name="creating-a-service-layer"></a>Création d’une couche de Service

Vous avez peut-être remarqué que notre logique de validation est toujours mélangé avec notre logique du contrôleur dans la classe de contrôleur modifié dans le Listing 3. Pour la même raison qu’il est judicieux d’isoler notre logique d’accès aux données, il est judicieux d’isoler notre logique de validation.

Pour résoudre ce problème, nous pouvons créer un distinct [ *couche de service*](http://martinfowler.com/eaaCatalog/serviceLayer.html). La couche de service est une couche distincte qui nous pouvons insérer entre notre contrôleur et les classes du référentiel. La couche de service contient notre logique métier, y compris toute notre logique de validation.

Le ContactManagerService est contenue dans la liste 4. Il contient la logique de validation à partir de la classe de contrôleur de Contact.

**Liste 4 - Models\ContactManagerService.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample4.cs)]

Notez que le constructeur pour le ContactManagerService requiert un ValidationDictionary. La couche de service communique avec la couche de contrôleur via cette ValidationDictionary. Nous abordons la ValidationDictionary en détail dans la section suivante lorsque nous aborderons le modèle Decorator.

En outre, notez que le ContactManagerService implémente l’interface IContactManagerService. Vous devez toujours vous efforcer de programmer des interfaces, et non des classes concrètes. Autres classes dans l’application Gestionnaire de contacts n’interagissent pas directement avec la classe ContactManagerService. Au lieu de cela, avec une exception, le reste de l’application Gestionnaire de contacts est programmé par rapport à l’interface IContactManagerService.

L’interface IContactManagerService est contenue dans la liste 5.

**Liste 5 - Models\IContactManagerService.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample5.cs)]

La classe de contrôleur Contact modifiée est contenue dans la liste 6. Notez que le contrôleur de Contact est n’est plus interagit avec le référentiel de ContactManager. Au lieu de cela, le contrôleur de contacts interagit avec le service ContactManager. Chaque couche est isolé autant que possible à partir d’autres couches.

**Liste 6 - Controllers\ContactController.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample6.cs)]

Notre application n’est plus afoul exécute des dates le principe de responsabilité unique (SRP). Le contrôleur de Contact de la liste 6 a été enlevé de chaque responsabilité autre que le contrôle du flux de l’exécution d’applications. Toute la logique de validation a été supprimée à partir du contrôleur de Contact et placée dans la couche de service. Toute la logique de base de données a été placé dans la couche de référentiels.

## <a name="using-the-decorator-pattern"></a>Utilisation du modèle Decorator

Nous souhaitons pouvoir séparer complètement notre couche de service à partir de la couche de notre contrôleur. En principe, nous devrions être en mesure de compiler notre couche de service dans un assembly distinct à partir de la couche de notre contrôleur sans avoir à ajouter une référence à notre application MVC.

Toutefois, notre couche de service doit être en mesure de retransmettre des messages d’erreur de validation à la couche de contrôleur. Comment pouvons-nous permettre la couche de service communiquer les messages d’erreur de validation sans couplage le contrôleur et la couche de service ? Nous pouvons tirer parti d’un modèle de conception de logiciel nommé le [modèle Decorator](http://en.wikipedia.org/wiki/Decorator_pattern).

Un contrôleur utilise un ModelStateDictionary nommé ModelState pour représenter les erreurs de validation. Par conséquent, il se peut que vous pouvez être tenté de passer de ModelState à partir de la couche de contrôleur à la couche de service. Toutefois, dans la couche de service à l’aide de ModelState rendrait votre couche de service dépend d’une fonctionnalité de l’infrastructure ASP.NET MVC. Ce serait incorrect, car un jour, vous souhaiterez peut-être utiliser la couche de service avec une application WPF au lieu d’une application ASP.NET MVC. Dans ce cas, t n’expliquerait pas vous souhaitez faire référence à l’infrastructure ASP.NET MVC pour utiliser la classe ModelStateDictionary.

Le modèle Decorator vous permet d’encapsuler une classe existante dans une nouvelle classe pour implémenter une interface. Notre projet de gestionnaire de contacts inclut la classe ModelStateWrapper contenue dans la liste 7. La classe ModelStateWrapper implémente l’interface dans la liste 8.

**Liste 7 - Models\Validation\ModelStateWrapper.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample7.cs)]

**Liste 8 - Models\Validation\IValidationDictionary.cs**

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample8.cs)]

Si vous jetez un œil à la liste 5 puis vous verrez que la couche de service ContactManager utilise l’interface IValidationDictionary exclusivement. Le service ContactManager n’est pas dépendant de la classe ModelStateDictionary. Lorsque le contrôleur de Contact crée le service ContactManager, le contrôleur encapsule son ModelState comme suit :

[!code-csharp[Main](iteration-4-make-the-application-loosely-coupled-cs/samples/sample9.cs)]

## <a name="summary"></a>Récapitulatif

Dans cette itération, nous n’avez pas ajouté de nouvelles fonctionnalités à l’application Gestionnaire de contacts. L’objectif de cette itération a été de refactoriser l’application Gestionnaire de contacts pour qu’elle soit plus facile à maintenir et à modifier.

Tout d’abord, nous avons implémenté le modèle de conception de logiciel de référentiel. Nous avons migré tout le code d’accès aux données à une classe de référentiel ContactManager distincte.

Nous avons isolé également notre logique de validation à partir de notre logique du contrôleur. Nous avons créé une couche de service distinct qui contient l’ensemble de notre code de validation. La couche de contrôleur interagit avec la couche de service, et la couche de service interagit avec la couche de référentiels.

Lorsque nous avons créé la couche de service, nous avons tiré parti du modèle d’élément décoratif pour isoler les ModelState à partir de la couche de notre service. Dans notre couche de service, nous programmé par rapport à l’interface IValidationDictionary au lieu de ModelState.

Enfin, nous avons tiré parti d’un modèle de conception de logiciel nommé le modèle d’Injection de dépendance. Ce modèle nous permet de programmer des interfaces (abstractions) au lieu de classes concrètes. Implémentation du modèle de conception de l’Injection de dépendances permet également de notre code plus facile à tester. Dans l’itération suivante, nous ajoutons des tests unitaires à notre projet.

> [!div class="step-by-step"]
> [Précédent](iteration-3-add-form-validation-cs.md)
> [Suivant](iteration-5-create-unit-tests-cs.md)
