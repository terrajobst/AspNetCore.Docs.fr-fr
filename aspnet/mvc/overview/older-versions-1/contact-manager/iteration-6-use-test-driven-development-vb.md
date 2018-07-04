---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
title: 'Itération #6 : utiliser le développement piloté par test (VB) | Microsoft Docs'
author: microsoft
description: Dans cette itération sixième, nous ajoutons les nouvelles fonctionnalités à notre application en écrivant des tests unitaires tout d’abord et écrire du code pour les tests unitaires. Dans cette itération,...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: e1fd226f-3f8e-4575-a179-5c75b240333d
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
msc.type: authoredcontent
ms.openlocfilehash: 3b55bb689a5a6dfdb07b02a7d595244f886f0fd2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377244"
---
<a name="iteration-6--use-test-driven-development-vb"></a>Itération #6 : utiliser le développement piloté par test (VB)
====================
par [Microsoft](https://github.com/microsoft)

[Télécharger le Code](iteration-6-use-test-driven-development-vb/_static/contactmanager_6_vb1.zip)

> Dans cette itération sixième, nous ajoutons les nouvelles fonctionnalités à notre application en écrivant des tests unitaires tout d’abord et écrire du code pour les tests unitaires. Dans cette itération, nous ajouter des groupes de contacts.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Création d’une Application ASP.NET MVC de gestion des contacts (VB)
  

Dans cette série de didacticiels, nous créer une application de gestion des contacts entière à partir du début à la fin. L’application Gestionnaire de Contact permet vous permettent de stocker les informations de contact (noms, numéros de téléphone et adresses de messagerie) pour obtenir la liste de personnes.

Nous générer l’application sur de multiples itérations. Avec chaque itération, nous améliorer progressivement l’application. L’objectif de cette approche à plusieurs itération consiste à vous permettre de comprendre la raison de chaque modification.

- Itération #1 : créer l’application. Dans la première itération, nous créons le Gestionnaire de Contact de la façon la plus simple possible. Nous ajoutons la prise en charge pour les opérations de base de données : création, lecture, mise à jour et suppression (CRUD).

- Itération #2 : donner l’application une apparence intéressante. Dans cette itération, nous améliorer l’apparence de l’application en modifiant la valeur par défaut de page maître de vue ASP.NET MVC et en cascade de feuille de style.

- Itération #3 - Ajouter une validation de formulaire. Dans la troisième itération, nous ajouter la validation de formulaire de base. Nous empêcher des personnes à partir de l’envoi d’un formulaire sans compléter les champs obligatoires. Nous avons également valider que les adresses de messagerie et les numéros de téléphone.

- Itération #4 : rendre l’application faiblement couplée. Dans cette troisième itération, nous profitons de plusieurs modèles de conception de logiciels pour le rendre plus facile à gérer et modifier l’application Gestionnaire de contacts. Par exemple, nous refactoriser notre application pour utiliser le modèle dépôt et le modèle d’Injection de dépendance.

- Itération #5 - créer des tests unitaires. Dans la cinquième itération, nous faciliter notre application mettre à jour et modifier en ajoutant des tests unitaires. Nous simuler nos classes de modèle de données et générer des tests unitaires pour nos contrôleurs et la logique de validation.

- Itération #6 - utiliser le développement piloté par test. Dans cette itération sixième, nous ajoutons les nouvelles fonctionnalités à notre application en écrivant des tests unitaires tout d’abord et écrire du code pour les tests unitaires. Dans cette itération, nous ajouter des groupes de contacts.

- Itération #7 - ajouter des fonctionnalités Ajax. Dans l’itération septième, nous améliorer la réactivité et les performances de notre application en ajoutant la prise en charge d’Ajax.

## <a name="this-iteration"></a>Cette itération

Dans l’itération précédente de l’application Gestionnaire de contacts, nous avons créé des tests unitaires afin de fournir un filet de sécurité pour notre code. La motivation pour créer les tests unitaires consistait à améliorer la résilience pour changer de notre code. Avec des tests unitaires en place, nous pouvons heureusement apporte aucune modification à notre code et savez immédiatement si nous avons altéré les fonctionnalités existantes.

Dans cette itération, nous utilisons des tests unitaires pour un objectif entièrement différent. Dans cette itération, nous utilisons des tests unitaires dans le cadre d’une philosophie de conception d’application appelé *développement piloté par test*. Lorsque vous entraîner le développement piloté par test, vous écrivez tout d’abord des tests et ensuite écrivez du code pour les tests.

Plus précisément, voie à double développement piloté par test, il existe trois étapes que vous effectuez lors de la création de code (rouge / vert/refactoriser) :

1. Écrire un test unitaire qui tombe en panne (rouge)
2. Écrire du code qui réussit le test unitaire (vert)
3. Refactorisez votre code (refactorisation)

Vous écrivez d’abord, le test unitaire. Le test unitaire doive exprimer votre intention pour que votre code se comporte. Lorsque vous créez tout d’abord le test unitaire, le test unitaire doit échouer. Le test échoue parce que vous n’avez pas encore écrit n’importe quel code d’application qui satisfait au test.

Ensuite, vous écrivez juste assez de code dans l’ordre pour le test unitaire à passer. L’objectif est d’écrire le code de la manière laziest, sloppiest et plus rapide possible. Vous devez perdez pas de temps à réfléchir sur l’architecture de votre application. Au lieu de cela, vous devez vous concentrer sur l’écriture de la quantité minimale de code nécessaire pour répondre à l’intention exprimée par le test unitaire.

Enfin, une fois que vous avez écrit suffisamment de code, vous pouvez prendre du recul et prendre en compte l’architecture globale de votre application. Dans cette étape, vous (refactorisation) le réécrire votre code en tirant parti de la conception logicielle modèles--tels que le modèle de référentiel--afin que votre code est plus facile à gérer. Vous pouvez crainte réécrire votre code dans cette étape, car votre code est couverte par les tests unitaires.

Il existe de nombreux avantages qui résultent de pratiquer le développement piloté par test. Tout d’abord, piloté par test développement vous oblige à vous concentrer sur le code qui doit réellement être écrit. Étant donné que vous avez le focus en permanence sur l’écriture juste assez de code pour passer un test particulier, vous ne pourrez pas dont y dans les plantes et l’écriture de grandes quantités de code que vous n’utiliserez jamais.

En second lieu, une méthodologie de conception « test en premier » vous oblige à écrire du code du point de vue de l’utilisation de votre code. En d’autres termes, lorsque pratiquant développement piloté par test, vous en permanence écrivez vos tests à partir d’un point de vue utilisateur. Par conséquent, développement piloté par test peut entraîner des API plus propres et plus compréhensibles.

Enfin, développement piloté par test vous oblige à écrire des tests unitaires dans le cadre du processus normal de l’écriture d’une application. Lorsqu’une date d’échéance approche, test est généralement la première chose qui sort de la fenêtre. Lorsque votre partenaire de développement piloté par test, quant à eux, vous êtes plus susceptible d’être vertueux sur l’écriture de tests unitaires, car le développement piloté par test effectue des tests unitaires centrale pour le processus de création d’une application.

> [!NOTE] 
> 
> Pour en savoir plus sur le développement piloté par test, je vous recommande de lire le livre de Michael Feathers **Working Effectively with Legacy Code**.


Dans cette itération, nous ajoutons une nouvelle fonctionnalité à notre application de gestionnaire de contacts. Nous ajoutons la prise en charge pour les groupes de Contact. Vous pouvez utiliser des groupes de Contact pour organiser vos contacts en catégories telles que les entreprises et les groupes de Friend.

Nous allons ajouter cette nouvelle fonctionnalité à notre application en suivant un processus de développement piloté par test. Nous allons écrire nos tests unitaires tout d’abord, et nous allons écrire tout notre code par rapport à ces tests.

## <a name="what-gets-tested"></a>Ce qui est testé

Comme expliqué dans l’itération précédente, en général, ne pas écrire des tests unitaires pour la logique d’accès aux données ou afficher logique. Vous n t écrire des tests unitaires pour la logique d’accès aux données, car l’accès à une base de données est une opération relativement lente. Vous n t écrire des tests unitaires pour la logique d’affichage, car l’accès à une vue nécessite mise en place d’un serveur web qui est une opération relativement lente. T ne doit pas vous écrivez un test unitaire, sauf si le test peut être exécuté indéfiniment très rapide

Étant donné que le développement piloté par test est piloté par les tests unitaires, nous nous concentrons initialement sur l’écriture de contrôleur et la logique métier. Nous Évitez de toucher la base de données ou des vues. Nous avons gagné t modifier la base de données ou de créer notre vues jusqu'à la fin de ce didacticiel. Nous commençons par ce qui peut être testé.

## <a name="creating-user-stories"></a>Création de récits utilisateur

Voie à double développement piloté par test, toujours commencer par écrire un test. Cela déclenche immédiatement la question : comment savoir quel test d’écrire tout d’abord ? Pour répondre à cette question, vous devez écrire un ensemble de [ *récits utilisateur*](http://en.wikipedia.org/wiki/User_stories).

Un récit utilisateur est une très brève description de (généralement une phrase) d’une configuration logicielle requise. Il doit être une description non technique d’une spécification écrite à partir d’un point de vue utilisateur.

Voici l’ensemble des récits utilisateur qui décrivent les fonctionnalités requises par la nouvelle fonctionnalité de groupe de Contact de s :

1. Utilisateur peut afficher une liste de groupes de contacts.
2. Utilisateur peut créer un nouveau groupe de contact.
3. Utilisateur peut supprimer un groupe de contact existant.
4. Utilisateur peut sélectionner un groupe de contacts lors de la création d’un nouveau contact.
5. Utilisateur peut sélectionner un groupe de contacts lors de la modification d’un contact existant.
6. Une liste de groupes de contact s’affiche dans la vue Index.
7. Lorsqu’un utilisateur clique sur un groupe de contacts, une liste de contacts correspondants s’affiche.

Notez que cette liste de récits utilisateur est parfaitement compréhensible par le client. Il n’existe aucune mention de détails d’implémentation technique.

Lors du processus de génération de votre application, l’ensemble des récits utilisateur peut devenir plus affinée. Vous risquez d’altérer un récit utilisateur en plusieurs récits (configuration requise). Par exemple, vous pouvez décider que la création d’un nouveau groupe de contact doit impliquer validation. Envoi d’un groupe sans nom de contact doit retourner une erreur de validation.

Après avoir créé une liste de récits utilisateur, vous êtes prêt à écrire votre premier test unitaire. Nous allons commencer en créant un test unitaire pour afficher la liste des groupes de contacts.

## <a name="listing-contact-groups"></a>Liste des groupes de contacts

Notre première récit utilisateur est qu’un utilisateur doit être en mesure d’afficher une liste de groupes de contacts. Nous avons besoin exprimer ce scénario avec un test.

Créer un nouveau test unitaire en double-cliquant sur le dossier contrôleurs dans le projet ContactManager.Tests, en sélectionnant **ajouter, nouveau Test**et en sélectionnant le **de Test unitaire** modèle (voir Figure 1). Nom de la nouvelle unité GroupControllerTest.vb de test et cliquez sur le **OK** bouton.


[![Ajout du test unitaire GroupControllerTest](iteration-6-use-test-driven-development-vb/_static/image1.jpg)](iteration-6-use-test-driven-development-vb/_static/image1.png)

**Figure 01**: ajout du test unitaire GroupControllerTest ([cliquez pour afficher l’image en taille réelle](iteration-6-use-test-driven-development-vb/_static/image2.png))


Notre premier test unitaire est contenue dans le Listing 1. Ce test vérifie que la méthode Index() du contrôleur de groupe renvoie un ensemble de groupes. Le test vérifie qu’une collection de groupes est retournée dans la vue données.

**Liste 1 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample1.vb)]

Lorsque vous tapez tout d’abord le code dans la liste de 1 dans Visual Studio, vous obtenez un grand nombre de lignes ondulées rouges. Nous n’avons pas créé les classes GroupController ou un groupe.

À ce stade, nous pouvons build même t notre application afin que nous t exécuter notre premier test unitaire. S bon. Qui est comptabilisé comme un test ayant échoué. Par conséquent, nous disposons désormais d’autorisation pour commencer à écrire le code d’application. Nous devons écrire suffisamment de code pour exécuter notre test.

La classe de contrôleur de groupe dans la liste 2 contient le strict minimum de code requis pour réussir le test unitaire. L’action Index() retourne une liste codée de manière statique de groupes (la classe du groupe est définie dans le Listing 3).

**Listing 2 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample2.vb)]

**Liste 3 - Models\Group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample3.vb)]

Une fois que nous ajoutons les classes GroupController et groupe à notre projet, notre premier test unitaire se termine correctement (voir Figure 2). Nous avons effectué le travail minimal requis pour réussir le test. Il est temps de faire la fête.


[![Succès !](iteration-6-use-test-driven-development-vb/_static/image2.jpg)](iteration-6-use-test-driven-development-vb/_static/image3.png)

**Figure 02**: réussite ! () [Cliquez pour afficher l’image en taille réelle](iteration-6-use-test-driven-development-vb/_static/image4.png))


## <a name="creating-contact-groups"></a>Création de groupes de Contact

Nous pouvons maintenant passer à la deuxième récit utilisateur. Nous devons être en mesure de créer des groupes de contact. Nous avons besoin exprimer cette intention avec un test.

Le test sur la liste 4 vérifie que l’appel de la méthode avec un nouveau groupe ajoute le groupe à la liste des groupes retournés par la méthode Index() de Create(). En d’autres termes, si je crée un nouveau groupe puis j’ai dois-je être en mesure de revenir au nouveau groupe dans la liste des groupes retournés par la méthode Index().

**Liste 4 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample4.vb)]

Le test sur la liste 4 appelle la méthode Create() avec un nouveau contact, groupe du contrôleur de groupe. Ensuite, le test vérifie qu’appelant le contrôleur de groupe Index() méthode retourne le nouveau groupe dans les données d’affichage.

Le contrôleur de groupe modifié sur la liste 5 contient le strict minimum de modifications requises pour transmettre le nouveau test.

**Liste 5 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample5.vb)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>Le contrôleur de groupe dans la liste 5 a une nouvelle action Create(). Cette action ajoute un groupe à une collection de groupes. Notez que l’action Index() a été modifiée pour retourner le contenu de la collection de groupes.

Une fois encore, nous avons effectué le minimum de travail requise pour réussir le test unitaire. Une fois que nous apporter ces modifications vers le contrôleur de groupe, tous les de notre unité tests réussissent.

## <a name="adding-validation"></a>Ajout de la validation

Cette exigence n’a pas été explicitement précisée dans le récit utilisateur. Toutefois, il est raisonnable de nécessitent qu’un nom à un groupe. Sinon, organisation des contacts en groupes serait pas très utile.

Liste 6 contient un nouveau test qui exprime l’objectif. Ce test vérifie que la tentative de création d’un groupe sans fournir un nom aboutit à un message d’erreur de validation dans l’état du modèle.

**Liste 6 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample6.vb)]

Afin de répondre à ce test, que nous devons ajouter une propriété de nom à notre classe de groupe (voir la liste 7). En outre, nous devons ajouter une petite quantité de logique de validation à notre contrôleur groupe s Create() action (voir liste 8).

**Liste 7 - Models\Group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample7.vb)]

**Liste 8 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample8.vb)]

Notez que le contrôleur de groupe action Create() maintenant contient la logique de validation et de base de données. Actuellement, la base de données utilisé par le contrôleur de groupe se compose de rien de plus qu’une collection en mémoire.

## <a name="time-to-refactor"></a>Temps de refactoriser

La troisième étape de rouge/vert/refactoriser est la partie de la refactorisation. À ce stade, nous devons étape à partir de notre code et de prendre en compte la façon dont nous pouvons refactoriser notre application afin d’améliorer sa conception. L’étape de refactorisation est l’étape à laquelle nous tâchez vraiment le meilleur moyen de l’implémentation de modèles et principes de conception de logiciels.

Nous sommes libres de modifier notre code de quelque façon que nous choisissons d’améliorer la conception du code. Nous avons un filet de tests unitaires qui nous empêchent de compromettre les fonctionnalités existantes.

Pour l’instant, notre contrôleur de groupe est n’importe quoi à partir de la perspective d’une bonne conception logicielle. Le contrôleur de groupe contient n’importe quoi toile de validation et le code d’accès aux données. Pour ne pas violer le principe de responsabilité unique, nous devons séparer ces problèmes en différentes classes.

Notre classe de contrôleur groupe refactorisée est contenue dans la liste 9. Le contrôleur a été modifié pour utiliser la couche de service ContactManager. Il s’agit de la même couche de service que nous utilisons avec le contrôleur de Contact.

Liste 10 contient les nouvelles méthodes ajoutés à la couche de service ContactManager pour prendre en charge la validation, de répertorier et de création de groupes. L’interface IContactManagerService a été mis à jour pour inclure les nouvelles méthodes.

Liste 11 contient une nouvelle classe FakeContactManagerRepository qui implémente l’interface IContactManagerRepository. Contrairement à la classe EntityContactManagerRepository qui implémente également l’interface IContactManagerRepository, notre nouvelle classe FakeContactManagerRepository ne communique pas avec la base de données. La classe FakeContactManagerRepository utilise une collection en mémoire en tant que proxy pour la base de données. Nous allons utiliser cette classe dans nos tests unitaires comme une couche de référentiels faux.

**Liste 9 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample9.vb)]

**Liste 10 - Controllers\ContactManagerService.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample10.vb)]

**Liste 11 - Controllers\FakeContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample11.vb)]


Modifier le IContactManagerRepository interface requiert utiliser pour implémenter les méthodes CreateGroup() et ListGroups() dans la classe EntityContactManagerRepository. Le laziest et le plus rapide pour ce faire consiste à ajouter des méthodes stub ressemblent à ceci :

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample12.vb)]


Enfin, ces modifications à la conception de notre application nous amener à apporter des modifications à nos tests unitaires. Nous devons maintenant utiliser le FakeContactManagerRepository lorsque vous effectuez les tests unitaires. La classe GroupControllerTest mis à jour est contenue dans la liste 12.

**Liste 12 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample13.vb)]

Une fois que nous avons apporté tous ces, là aussi, toutes les modifications de nos passe des tests unitaires. Nous avons terminé le cycle complet de rouge/vert/Refactor. Nous avons implémenté les récits deux utilisateur première. Nous disposons désormais la prise en charge des tests unitaires pour les besoins exprimés dans les récits utilisateur. L’implémentation du reste des récits utilisateur implique la répéter le même cycle de rouge/vert/Refactor.

## <a name="modifying-our-database"></a>Modification de notre base de données

Malheureusement, bien que nous avons satisfait toutes les exigences exprimés par nos tests unitaires, notre travail n’est pas terminé. Nous devons encore modifier notre base de données.

Nous devons créer une table de base de données de groupe. Procédez comme suit :

1. Dans la fenêtre Explorateur de serveurs, cliquez sur le dossier Tables et sélectionnez l’option de menu **ajouter une nouvelle Table**.
2. Entrez les deux colonnes décrites ci-dessous dans le Concepteur de tables.
3. Marquer la colonne Id comme une clé primaire et une colonne d’identité.
4. Enregistrer la nouvelle table avec les groupes de nom en cliquant sur l’icône de disquette.

<a id="0.12_table01"></a>


| **Nom de colonne** | **Type de données** | **Autoriser les valeurs null** |
| --- | --- | --- |
| Id | int | False |
| Name | nvarchar (50) | False |


Ensuite, nous devons supprimer toutes les données de la table de Contacts (dans le cas contraire, nous avons gagné t être en mesure de créer une relation entre les tableaux Contacts et groupes). Procédez comme suit :

1. Avec le bouton droit de la table Contacts, puis sélectionnez l’option de menu **afficher les données de Table**.
2. Supprimer toutes les lignes.

Ensuite, nous devons définir une relation entre la table de base de données de groupes et de la table de base de données de Contacts existante. Procédez comme suit :

1. Double-cliquez sur la table de Contacts dans la fenêtre Explorateur de serveurs pour ouvrir le Concepteur de tables.
2. Ajouter une nouvelle colonne d’entiers à la table Contacts nommée GroupId.
3. Cliquez sur le bouton de relation pour ouvrir la boîte de dialogue relations de clé étrangère (voir Figure 3).
4. Cliquez sur le bouton Ajouter.
5. Cliquez sur le bouton de sélection apparaît en regard du bouton de la Table et une spécification de colonnes.
6. Dans la boîte de dialogue Tables et colonnes, sélectionnez les groupes en tant que la table de clé primaire et l’Id en tant que colonne de clé primaire. Sélectionnez des Contacts en tant que la table de clé étrangère et GroupId comme colonne de clé étrangère (voir Figure 4). Cliquez sur le bouton OK.
7. Sous **Spécification INSERT et UPDATE**, sélectionnez la valeur **Cascade** pour **supprimer la règle**.
8. Cliquez sur le bouton Fermer pour fermer la boîte de dialogue relations de clé étrangère.
9. Cliquez sur le bouton Enregistrer pour enregistrer les modifications apportées à la table Contacts.


[![Création d’une relation de table de base de données](iteration-6-use-test-driven-development-vb/_static/image3.jpg)](iteration-6-use-test-driven-development-vb/_static/image5.png)

**Figure 03**: création d’une relation de table de base de données ([cliquez pour afficher l’image en taille réelle](iteration-6-use-test-driven-development-vb/_static/image6.png))


[![Spécification des relations de table](iteration-6-use-test-driven-development-vb/_static/image4.jpg)](iteration-6-use-test-driven-development-vb/_static/image7.png)

**Figure 04**: spécification des relations de table ([cliquez pour afficher l’image en taille réelle](iteration-6-use-test-driven-development-vb/_static/image8.png))


### <a name="updating-our-data-model"></a>La mise à jour de notre modèle de données

Ensuite, nous devons mettre à jour de notre modèle de données pour représenter la nouvelle table de base de données. Procédez comme suit :

1. Double-cliquez sur le fichier ContactManagerModel.edmx dans le dossier de modèles pour ouvrir le Concepteur d’entités.
2. Cliquez sur l’aire du concepteur et sélectionnez l’option de menu **modèle de mise à jour à partir de la base de données**.
3. Dans l’Assistant Mise à jour, sélectionnez les groupes de table et cliquez sur Terminer bouton (voir Figure 5).
4. Avec le bouton droit de l’entité de groupes et sélectionnez l’option de menu **renommer**. Modifier le nom de la *groupes* entité à *groupe* (singulier).
5. Avec le bouton droit de la propriété de navigation de groupes qui s’affiche en bas de l’entité Contact. Modifier le nom de la *groupes* propriété de navigation *groupe* (singulier).


[![La mise à jour un modèle Entity Framework à partir de la base de données](iteration-6-use-test-driven-development-vb/_static/image5.jpg)](iteration-6-use-test-driven-development-vb/_static/image9.png)

**Figure 05**: la mise à jour un modèle Entity Framework à partir de la base de données ([cliquez pour afficher l’image en taille réelle](iteration-6-use-test-driven-development-vb/_static/image10.png))


Après avoir effectué ces étapes, votre modèle de données représente les Contacts et les groupes de tables. Le Concepteur d’entités doit afficher les deux entités (voir Figure 6).


[![Concepteur d’entités affichant le groupe et Contact](iteration-6-use-test-driven-development-vb/_static/image6.jpg)](iteration-6-use-test-driven-development-vb/_static/image11.png)

**Figure 06**: Concepteur d’entités affichant le groupe et Contact ([cliquez pour afficher l’image en taille réelle](iteration-6-use-test-driven-development-vb/_static/image12.png))


### <a name="creating-our-repository-classes"></a>Création de Classes de notre référentiel

Ensuite, nous devons implémenter notre classe de dépôt. Au cours de cette itération, nous avons ajouté plusieurs nouvelles méthodes à l’interface IContactManagerRepository lors de l’écriture de code pour satisfaire nos tests unitaires. La version finale de l’interface IContactManagerRepository est contenue dans la liste de 14.

**Liste 14 - Models\IContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample14.vb)]

Nous haven t réellement implémentées les méthodes liées à l’utilisation avec des groupes de contact dans notre classe EntityContactManagerRepository réel. Actuellement, la classe EntityContactManagerRepository a des méthodes stub pour chacune des méthodes de contact de groupe répertoriés dans l’interface IContactManagerRepository. Par exemple, la méthode ListGroups() actuellement ressemble à ceci :

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample15.vb)]

Les méthodes stub permis de compiler notre application et passer les tests unitaires. Toutefois, il est maintenant temps de réellement implémenter ces méthodes. La version finale de la classe EntityContactManagerRepository est contenue dans la liste 13.

**Liste 13 - Models\EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample16.vb)]

### <a name="creating-the-views"></a>Création de vues

Application d’ASP.NET MVC lorsque vous utilisez le moteur d’affichage ASP.NET par défaut. Par conséquent, ne pas créer des vues en réponse à un test unitaire particulier. Toutefois, car une application serait inutile en l’absence de vues, nous pouvons t terminer cette itération sans créer et modifier les vues contenues dans l’application Gestionnaire de contacts.

Nous devons créer les suivants nouvelles vues de gestion des groupes de contact (voir la Figure 7) :

- Views\Group\Index.aspx - affiche la liste des groupes de contacts
- Views\Group\Delete.aspx - écran de confirmation affiche pour la suppression d’un groupe de contact


[![La vue Index de groupe](iteration-6-use-test-driven-development-vb/_static/image7.jpg)](iteration-6-use-test-driven-development-vb/_static/image13.png)

**Figure 07**: affichage de l’Index de groupe ([cliquez pour afficher l’image en taille réelle](iteration-6-use-test-driven-development-vb/_static/image14.png))


Nous avons besoin de modifier les vues suivantes existants afin qu’ils contiennent des groupes de contacts :

- Views\Home\Create.aspx
- Views\Home\Edit.aspx
- Views\Home\Index.aspx

Vous pouvez voir les vues modifiés en examinant l’application de Visual Studio qui accompagne ce didacticiel. Par exemple, la Figure 8 illustre la vue Index de Contact.


[![La vue Index des contacts](iteration-6-use-test-driven-development-vb/_static/image8.jpg)](iteration-6-use-test-driven-development-vb/_static/image15.png)

**Figure 08**: affichage de l’Index de Contact ([cliquez pour afficher l’image en taille réelle](iteration-6-use-test-driven-development-vb/_static/image16.png))


## <a name="summary"></a>Récapitulatif

Dans cette itération, nous avons ajouté de nouvelles fonctionnalités à notre application de gestionnaire de contacts en suivant une méthodologie de conception d’application développement piloté par test. Nous avons commencé par créer un ensemble de récits utilisateur. Nous avons créé un ensemble de tests unitaires qui correspond aux besoins exprimés par les récits utilisateur. Enfin, nous avons écrit juste assez de code pour satisfaire les besoins exprimés par les tests unitaires.

Une fois que nous avons rédigé suffisamment de code pour satisfaire les besoins exprimés par les tests unitaires, nous avons mis à jour notre base de données et les vues. Nous avons ajouté une nouvelle table de groupes à notre base de données et mise à jour de notre modèle de données Entity Framework. Nous avons également créé et modifié un ensemble de vues.

Dans la prochaine itération--la dernière itération--nous Réécrivons notre application pour tirer parti d’Ajax. En tirant parti d’Ajax, nous allons améliorer la réactivité et les performances de l’application Gestionnaire de contacts.

> [!div class="step-by-step"]
> [Précédent](iteration-5-create-unit-tests-vb.md)
> [Suivant](iteration-7-add-ajax-functionality-vb.md)
