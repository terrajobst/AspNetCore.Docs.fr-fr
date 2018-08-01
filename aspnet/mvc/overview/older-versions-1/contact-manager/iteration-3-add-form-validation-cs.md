---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
title: 'Itération #3 : ajouter une validation de formulaire (c#) | Microsoft Docs'
author: microsoft
description: Dans la troisième itération, nous ajouter la validation de formulaire de base. Nous empêcher des personnes à partir de l’envoi d’un formulaire sans compléter les champs obligatoires. Nous avons également valider emai...
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: 51a0d175-913b-43d8-95e3-840fb96ad1a9
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: d7f5d183b98ea123a780714dec2d806e4e794c2d
ms.sourcegitcommit: a25b572eaed21791230c85416f449f66a405ec19
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39396074"
---
<a name="iteration-3--add-form-validation-c"></a>Itération #3 : ajouter une validation de formulaire (c#)
====================
par [Microsoft](https://github.com/microsoft)

[Télécharger le Code](iteration-3-add-form-validation-cs/_static/contactmanager_3_cs1.zip)

> Dans la troisième itération, nous ajouter la validation de formulaire de base. Nous empêcher des personnes à partir de l’envoi d’un formulaire sans compléter les champs obligatoires. Nous avons également valider que les adresses de messagerie et les numéros de téléphone.


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

Dans ce deuxième itération de l’application Gestionnaire de contacts, nous ajouter la validation de formulaire de base. Nous empêcher des personnes de soumettre un contact sans entrer de valeurs pour les champs obligatoires. Nous avons également valider que les numéros de téléphone et adresses de messagerie (voir Figure 1).


[![La boîte de dialogue Nouveau projet](iteration-3-add-form-validation-cs/_static/image1.jpg)](iteration-3-add-form-validation-cs/_static/image1.png)

**Figure 01**: un formulaire avec la validation ([cliquez pour afficher l’image en taille réelle](iteration-3-add-form-validation-cs/_static/image2.png))


Dans cette itération, nous ajoutons la logique de validation directement sur les actions de contrôleur. En règle générale, cela n’est pas recommandée pour ajouter une validation à une application ASP.NET MVC. Une meilleure approche consiste à placer une logique de validation application s dans une fonction [couche de service](http://martinfowler.com/eaaCatalog/serviceLayer.html). Dans l’itération suivante, nous refactoriser l’application Gestionnaire de contacts pour rendre l’application plus facile à gérer.

Dans cette itération, pour simplifier les choses, nous écrire tout le code de validation à la main. Au lieu d’écrire le code de validation nous-mêmes, nous pouvons tirer parti d’une infrastructure de validation. Par exemple, vous pouvez utiliser le Microsoft Enterprise Validation Application bloc (Library) pour implémenter la logique de validation pour votre application ASP.NET MVC. Pour en savoir plus sur le bloc applicatif de Validation, consultez :

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Ajout d’une Validation à la vue Create

Permettent de commencer par ajouter la logique de validation à la vue Create s. Heureusement, étant donné que nous avons généré la vue de créer avec Visual Studio, la vue Create contient déjà toute la logique d’interface utilisateur nécessaires pour afficher les messages de validation. La vue de créer est contenue dans le Listing 1.

**Liste 1 - \Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-cs/samples/sample1.aspx)]

Notez l’appel à la méthode d’assistance Html.ValidationSummary() qui apparaît immédiatement au-dessus du formulaire HTML. S’il existe des messages d’erreur de validation, cette méthode affiche les messages de validation dans une liste à puces.

Notez que, en outre, les appels à Html.ValidationMessage() qui s’affichent en regard de chaque champ de formulaire. Le programme d’assistance ValidationMessage() affiche un message d’erreur de validation individuelles. Dans le cas de liste 1, un astérisque est affiché lorsqu’une erreur de validation.

Enfin, l’application d’assistance Html.TextBox() affiche automatiquement une classe de feuille de Style en cascade lorsqu’il existe une erreur de validation associée à la propriété affichée par l’application auxiliaire. Le programme d’assistance Html.TextBox() restitue une classe nommée **erreur de validation d’entrée**.

Lorsque vous créez une application ASP.NET MVC, une feuille de style nommée Site.css est automatiquement créée dans le dossier de contenu. Cette feuille de style contient les définitions suivantes pour les classes CSS liées à l’apparence des messages d’erreur de validation :

[!code-css[Main](iteration-3-add-form-validation-cs/samples/sample2.css)]

La classe d’erreur de validation de champ est utilisée pour définir le style de la sortie rendue par l’application d’assistance Html.ValidationMessage(). La classe d’erreur de validation d’entrée est utilisée pour définir le style de la zone de texte (entrée) rendue par l’application d’assistance Html.TextBox(). La classe d’erreurs de résumé de validation est utilisée pour définir le style de la liste non triée restituée par l’application d’assistance Html.ValidationSummary().

> [!NOTE] 
> 
> Vous pouvez modifier les classes de feuille de style décrites dans cette section pour personnaliser l’apparence des messages d’erreur de validation.


## <a name="adding-validation-logic-to-the-create-action"></a>Ajout de la logique de Validation à l’Action de création

Actuellement, la vue Create affiche jamais les messages d’erreur de validation, car nous avons écrit pas la logique pour générer des messages. Pour afficher les messages d’erreur de validation, vous devez ajouter les messages d’erreur à ModelState.

> [!NOTE] 
> 
> La méthode UpdateModel() ajoute automatiquement les messages d’erreur à ModelState lorsqu’il existe une erreur, affectez la valeur d’un champ de formulaire à une propriété. Par exemple, si vous tentez d’affecter la chaîne « apple » à une propriété de date de naissance qui accepte des valeurs de date/heure, la méthode UpdateModel() ajoute une erreur à ModelState.


La méthode Create() modifiée dans le Listing 2 contient une nouvelle section qui valide les propriétés de la classe Contact avant que le nouveau contact est inséré dans la base de données.

**Listing 2 - Controllers\ContactController.cs (créer avec validation)**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample3.cs)]

La section de validation applique quatre règles de validation distinctes :

- La propriété FirstName doit avoir une longueur supérieure à zéro (et il ne peut pas être composé uniquement d’espaces)
- La propriété LastName doit avoir une longueur supérieure à zéro (et il ne peut pas être composé uniquement d’espaces)
- Si la propriété de téléphone a la valeur (a une longueur supérieure à 0), puis la propriété de téléphone doit correspondre à une expression régulière.
- Si la propriété de messagerie a une valeur (a une longueur supérieure à 0), puis la propriété de messagerie doit correspondre à une expression régulière.

Lorsqu’il existe une violation de règle de validation, un message d’erreur est ajouté à ModelState à l’aide de la méthode AddModelError(). Lorsque vous ajoutez un message à ModelState, vous fournissez le nom d’une propriété et le texte d’un message d’erreur de validation. Ce message d’erreur s’affiche dans la vue par les méthodes d’assistance Html.ValidationSummary() et Html.ValidationMessage().

Une fois que les règles de validation sont exécutées, la propriété IsValid de ModelState est vérifiée. La propriété IsValid retourne false lorsque les messages d’erreur de validation ont été ajoutés à ModelState. Si la validation échoue, le formulaire de création est réaffiché avec les messages d’erreur.

> [!NOTE] 
> 
> J’ai reçu les expressions régulières pour valider l’adresse de messagerie et le numéro de téléphone à partir du référentiel de l’expression régulière à [*http://regexlib.com*](http://regexlib.com)


## <a name="adding-validation-logic-to-the-edit-action"></a>Ajout d’une logique de Validation à l’Action de modification

L’action Edit() met à jour un Contact. L’action Edit() doit effectuer exactement la même validation que l’action Create(). Au lieu de répéter le même code de validation, nous devons refactoriser le contrôleur de Contact afin que les Create() Edit() actions et appellent la même méthode de validation.

La classe de contrôleur Contact modifiée est contenue dans le Listing 3. Cette classe possède une nouvelle méthode ValidateContact() qui est appelée dans les Create() Edit() actions et.

**Liste 3 - Controllers\ContactController.cs**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample4.cs)]

## <a name="summary"></a>Récapitulatif

Dans cette itération, nous avons ajouté la validation de formulaire de base à notre application de gestionnaire de contacts. Notre logique de validation empêche les utilisateurs de soumettre un nouveau contact ou la modification d’un contact existant sans fournir des valeurs pour les propriétés FirstName et LastName. En outre, les utilisateurs doivent fournir des adresses de messagerie et les numéros de téléphone valide.

Dans cette itération, nous avons ajouté la logique de validation à notre application de gestionnaire de contacts dans le moyen le plus simple possible. Toutefois, combinaison de notre logique de validation dans notre logique du contrôleur créera des problèmes pour nous à long terme. Notre application sera plus difficile à maintenir et à modifier au fil du temps.

Dans l’itération suivante, nous devez refactoriser notre logique de validation et la logique d’accès de base de données hors de nos contrôleurs. Nous allons tirer parti de plusieurs principes de conception de logiciels afin de nous permettre de créer une application plus souple et plus facile à gérer.

> [!div class="step-by-step"]
> [Précédent](iteration-2-make-the-application-look-nice-cs.md)
> [Suivant](iteration-4-make-the-application-loosely-coupled-cs.md)
