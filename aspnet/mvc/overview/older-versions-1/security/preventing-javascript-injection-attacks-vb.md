---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
title: Prévention des attaques d’injection de code JavaScript (VB) | Microsoft Docs
author: StephenWalther
description: Empêcher les attaques par Injection de JavaScript et attaques de script entre sites pour vous. Dans ce didacticiel, Stephen Walther explique comment vous pouvez facilement de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 9274a72e-34dd-4dae-8452-ed733ae71377
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-vb
msc.type: authoredcontent
ms.openlocfilehash: 6c022847462239624d5b84816999918ec77f8337
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402339"
---
<a name="preventing-javascript-injection-attacks-vb"></a>Prévention des attaques d’injection de code JavaScript (VB)
====================
par [Stephen Walther](https://github.com/StephenWalther)

[Télécharger PDF](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_VB.pdf)

> Empêcher les attaques par Injection de JavaScript et attaques de script entre sites pour vous. Dans ce didacticiel, Stephen Walther explique comment vous pouvez facilement mettre en échec ces types d’attaques par codage votre contenu HTML.


L’objectif de ce didacticiel est d’expliquer comment vous pouvez empêcher les attaques par injection de JavaScript dans vos applications ASP.NET MVC. Ce didacticiel aborde deux approches pour la défense de votre site Web par rapport à une attaque par injection de JavaScript. Vous allez apprendre à empêcher les attaques par injection de JavaScript en encodant les données que vous affichez. Vous allez également apprendre à empêcher les attaques par injection de JavaScript par les données que vous acceptez d’encodage.

## <a name="what-is-a-javascript-injection-attack"></a>Qu’est une attaque par Injection de JavaScript ?

Chaque fois que vous acceptez l’entrée d’utilisateur et réaffichez l’entrée d’utilisateur, vous ouvrez votre site Web à des attaques par injection de code JavaScript. Commençons par examiner une application concrète qui est ouverte aux attaques par injection de JavaScript.

Imaginez que vous avez créé un site Web des commentaires de clients (voir Figure 1). Les clients peuvent visiter le site Web et entrer des commentaires sur leur expérience à l’aide de vos produits. Lorsqu’un client soumet leurs commentaires, les commentaires s’affiche de nouveau sur la page de commentaires.


[![Site de commentaires client](preventing-javascript-injection-attacks-vb/_static/image2.png)](preventing-javascript-injection-attacks-vb/_static/image1.png)

**Figure 01**: site Web de commentaires de clients ([cliquez pour afficher l’image en taille réelle](preventing-javascript-injection-attacks-vb/_static/image3.png))


Le site de commentaires client utilise le `controller` dans le Listing 1. Cela `controller` contient deux actions nommées `Index()` et `Create()`.

**Liste 1 : `HomeController.vb`**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample1.vb)]

Le `Index()` méthode affiche le `Index` vue. Cette méthode passe tous les commentaires client précédente à la `Index` vue en récupérant les commentaires à partir de la base de données (à l’aide d’une requête LINQ to SQL).

Le `Create()` méthode crée un nouvel élément de commentaires et l’ajoute à la base de données. Le message que le client entre dans le formulaire est passé à la `Create()` méthode dans le paramètre de message. Un commentaire est créé et le message est affecté à l’élément de commentaire `Message` propriété. L’élément de commentaires est soumis à la base de données avec le `DataContext.SubmitChanges()` appel de méthode. Enfin, le visiteur est redirigé vers le `Index` vue où tous les commentaires sont affichées.

Le `Index` affichage est inclus dans le Listing 2.

**Listing 2 : `Index.aspx`**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample2.aspx)]

Le `Index` vue comporte deux sections. La section supérieure contient le formulaire de commentaires de clients réels. La section inférieure contient un For.. Chaque boucle qui effectue une itération sur tous les éléments de commentaires client précédente et affiche les propriétés EntryDate et de Message pour chaque élément de commentaires.

Le site de commentaires client est un site Web simple. Malheureusement, le site Web est ouvert aux attaques par injection de JavaScript.

Imaginez que vous entrez le texte suivant dans le formulaire de commentaires client :

[!code-html[Main](preventing-javascript-injection-attacks-vb/samples/sample3.html)]

Ce texte représente un script JavaScript qui affiche une boîte de message d’alerte. Une fois que quelqu'un envoie ce script dans les commentaires de formulaire, le message <em>Boo !</em> s’affiche chaque fois que tout le monde visite le site Web des commentaires des clients à l’avenir (voir Figure 2).


[![Injection de code JavaScript](preventing-javascript-injection-attacks-vb/_static/image5.png)](preventing-javascript-injection-attacks-vb/_static/image4.png)

**Figure 02**: l’injection de code JavaScript ([cliquez pour afficher l’image en taille réelle](preventing-javascript-injection-attacks-vb/_static/image6.png))


À présent, votre réponse initiale à des attaques par injection de code JavaScript peut être indifférence. Vous pourriez penser que les attaques par injection de JavaScript sont simplement un type de *dégradation* attaque. Vous pensez qu’aucune autre faire quoi que ce soit réellement malveillant en validant une attaque par injection de JavaScript.

Malheureusement, un pirate peut effectuer certaines vraiment, vraiment malveillant choses en injectant JavaScript dans un site Web. Vous pouvez utiliser une attaque par injection de JavaScript pour effectuer une attaque de Cross-Site Scripting (XSS). Dans une attaque de Cross-Site Scripting, vous allez voler des informations confidentielles et envoyer les informations à un autre site Web.

Par exemple, un pirate peut utiliser une attaque par injection de JavaScript pour voler les valeurs de cookies de navigateur à partir d’autres utilisateurs. Si des informations sensibles, telles que les mots de passe, numéros de carte de crédit ou les numéros de sécurité sociale – sont stockées dans les cookies de navigateur, un pirate peut utiliser une attaque par injection de JavaScript à voler ces informations. Ou, si un utilisateur saisit des informations sensibles dans un champ de formulaire contenu dans une page qui a été compromise par une attaque de JavaScript, puis le pirate peut utiliser le code injecté JavaScript pour récupérer les données du formulaire et l’envoyer à un autre site Web.

*Veuillez pas peur*. Prenons les attaques par injection de JavaScript au sérieux et protéger les informations confidentielles de votre utilisateur. Dans les deux sections suivantes, nous abordons les deux techniques que vous pouvez utiliser pour protéger vos applications ASP.NET MVC contre les attaques par injection de code de JavaScript.

## <a name="approach-1-html-encode-in-the-view"></a>Méthode #1 : Encoder en HTML dans la vue

Une méthode simple pour empêcher les attaques par injection de JavaScript est au format HTML encoder toutes les données entrées par les utilisateurs du site Web lorsque vous affichez de nouveau les données dans une vue. La mise à jour `Index` vue dans la liste 3 suit cette approche.

**Liste 3 – `Index.aspx` (encodée en HTML)**

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample4.aspx)]

Notez que la valeur de `feedback.Message` est encodé en HTML avant la valeur est affichée avec le code suivant :

[!code-aspx[Main](preventing-javascript-injection-attacks-vb/samples/sample5.aspx)]

Quelles en moyenne au format HTML encoder une chaîne ? Lorsque vous HTML encoder une chaîne, dangereux caractères tels que `<` et `>` sont remplacés par des références d’entité HTML comme `&lt;` et `&gt;`. Par conséquent, lorsque la chaîne `<script>alert("Boo!")</script>` est au format HTML encodé, elles sont converties en `&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`. La chaîne encodée ne s’exécute plus comme un script JavaScript lorsqu’il est interprété par un navigateur. Au lieu de cela, vous obtenez la page sans incidence dans la Figure 3.


[![Vaincu attaque JavaScript](preventing-javascript-injection-attacks-vb/_static/image8.png)](preventing-javascript-injection-attacks-vb/_static/image7.png)

**Figure 03**: vaincu attaque JavaScript ([cliquez pour afficher l’image en taille réelle](preventing-javascript-injection-attacks-vb/_static/image9.png))


Notez que dans le `Index` afficher dans la liste 3 uniquement la valeur de `feedback.Message` est encodé. La valeur de `feedback.EntryDate` n’est pas encodé. Vous devez uniquement encoder les données entrées par un utilisateur. Étant donné que la valeur de EntryDate a été générée dans le contrôleur, vous n’avez besoin au format HTML Encoder cette valeur.

## <a name="approach-2-html-encode-in-the-controller"></a>Approche #2 : Encoder en HTML dans le contrôleur

Au lieu de code HTML de l’encodage des données lorsque vous affichez les données dans une vue, vous pouvez HTML encoder les données juste avant d’envoyer les données à la base de données. Cette seconde approche est effectuée dans le cas de la `controller` sur la liste 4.

**Liste 4 – `HomeController.cs` (encodée en HTML)**

[!code-vb[Main](preventing-javascript-injection-attacks-vb/samples/sample6.vb)]

Notez que la valeur du Message est encodé en HTML avant la valeur est soumise à la base de données dans le `Create()` action. Lorsque le Message s’affiche de nouveau dans la vue, le Message est encodé au format HTML et n’importe quel JavaScript injecté dans le Message n’est pas exécutée.

En règle générale, vous devez pencher pour la première approche présentée dans ce didacticiel sur cette seconde approche. Le problème avec cette seconde approche est que vous vous retrouvez avec les données encodées en HTML dans votre base de données. En d’autres termes, votre base de données est salie par les tests avec les caractères qui drôles.

Pourquoi est-ce incorrect ? Si vous avez besoin afficher les données de base de données dans l’autre chose qu’une page web, puis vous aura des problèmes. Par exemple, vous pouvez afficher n’est plus facilement les données dans une application Windows Forms.

## <a name="summary"></a>Récapitulatif

L’objectif de ce didacticiel a été plus vous effrayer sur la perspective d’une attaque par injection de JavaScript. Ce didacticiel décrit deux approches pour la protection de vos applications ASP.NET MVC contre les attaques par injection de JavaScript : vous pouvez soit HTML Encoder soumise par un utilisateur les données dans la vue, ou vous peuvent HTML Encoder soumise par un utilisateur les données dans le contrôleur.

> [!div class="step-by-step"]
> [Précédent](authenticating-users-with-windows-authentication-vb.md)
