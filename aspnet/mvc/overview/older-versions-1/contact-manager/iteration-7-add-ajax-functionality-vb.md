---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
title: 'Itération #7 : ajouter des fonctionnalités Ajax (VB) | Microsoft Docs'
author: microsoft
description: Dans l’itération septième, nous améliorer la réactivité et les performances de notre application en ajoutant la prise en charge d’Ajax.
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: f640e063-150e-453d-8cfc-7e54a6ce0f1e
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-vb
msc.type: authoredcontent
ms.openlocfilehash: b39a251acc96a78245238da5b82365958223e4c7
ms.sourcegitcommit: a25b572eaed21791230c85416f449f66a405ec19
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39396177"
---
<a name="iteration-7--add-ajax-functionality-vb"></a>Itération #7 : ajouter des fonctionnalités Ajax (VB)
====================
par [Microsoft](https://github.com/microsoft)

[Télécharger le Code](iteration-7-add-ajax-functionality-vb/_static/contactmanager_7_vb1.zip)

> Dans l’itération septième, nous améliorer la réactivité et les performances de notre application en ajoutant la prise en charge d’Ajax.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Création d’une Application ASP.NET MVC de gestion des contacts (VB)
  

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

Dans cette itération de l’application Gestionnaire de contacts, nous refactoriser notre application d’utiliser Ajax. En tirant parti d’Ajax, nous rendre notre application plus réactive. Nous pouvons éviter le rendu d’une page entière lorsque nous devons mettre à jour uniquement une certaine région dans une page.

Nous allons refactoriser notre vue Index afin que nous n’avez besoin de t pour réafficher la page entière chaque fois qu’un utilisateur sélectionne un nouveau groupe de contact. Au lieu de cela, lorsque quelqu'un clique sur un groupe de contact, nous simplement mettre à jour la liste des contacts et laissez le reste de la page uniquement.

Vous allez également modifier la façon de notre delete lien fonctionne. Au lieu d’afficher une page de confirmation distinct, nous affichons une boîte de dialogue de confirmation de JavaScript. Si vous confirmez que vous souhaitez supprimer un contact, une opération HTTP DELETE est exécutée sur le serveur pour supprimer l’enregistrement de contact à partir de la base de données.

En outre, nous allons utiliser de jQuery pour ajouter des effets d’animation à notre vue Index. Nous affichons une animation lorsque la nouvelle liste de contacts est en cours d’extraction à partir du serveur.

Enfin, nous allons tirer parti de la prise en charge du framework ASP.NET AJAX pour la gestion de l’historique du navigateur. Nous allons créer des points d’historique à chaque fois que nous effectuons un appel Ajax pour mettre à jour la liste des contacts. De cette façon, le navigateur ascendante et descendante boutons va fonctionner.

## <a name="why-use-ajax"></a>Pourquoi utiliser Ajax ?

À l’aide d’Ajax présente de nombreux avantages. Tout d’abord, ajout de fonctionnalités Ajax à une application implique une meilleure expérience utilisateur. Dans une application web normal, la page entière doit être publiée sur le serveur chaque fois qu'un utilisateur effectue une action. Chaque fois que vous effectuez une action, les verrous de navigateur et l’utilisateur doivent patienter jusqu'à ce que la page entière est extraite et affiche de nouveau.

Il s’agit d’une expérience inacceptable dans le cas d’une application de bureau. Mais, en règle générale, nous vécu cette expérience utilisateur incorrects dans le cas d’une application web, car nous ne savait pas que nous pourrions faire mieux. Nous avons vu une limitation des applications web quand, en réalité, il s’agissait juste d’une limitation de notre imaginations.

Dans une application Ajax, vous n’avez pas besoin de t pour permettre l’arrêt simplement mettre à jour une page de l’expérience utilisateur. Au lieu de cela, vous pouvez effectuer une requête asynchrone en arrière-plan pour mettre à jour de la page. Vous n’avez pas les forcer t l’utilisateur à attendre la partie de la page est mise à jour.

En tirant parti d’Ajax, vous pouvez également améliorer les performances de votre application. Prendre en compte comment l’application Gestionnaire de contacts fonctionne maintenant sans les fonctionnalités Ajax. Lorsque vous cliquez sur un groupe de contacts, toute la vue Index doit être actualisée. La liste des contacts et la liste des groupes de contacts doivent être récupérées à partir du serveur de base de données. Toutes ces données doivent être passées via le réseau à partir du serveur web au navigateur web.

Toutefois, une fois que nous ajoutons des fonctionnalités Ajax à notre application, nous pouvons éviter réaffichage de la page entière quand un utilisateur clique sur un groupe de contacts. Nous n’avez plus besoin récupérer les groupes de contact à partir de la base de données. Nous n’également besoin de t pour transmettre toute la vue Index sur le câble. En tirant parti d’Ajax, nous réduisons la quantité de travail que notre serveur de base de données doit effectuer et nous réduisons la quantité de trafic réseau requis par notre application.

## <a name="don-t-be-afraid-of-ajax"></a>Ne pas être peur d’Ajax

Certains développeurs évitent à l’aide d’Ajax, car ils vous inquiétez pas sur les navigateurs de niveau inférieur. Ils veulent s’assurer que leurs applications web continue de fonctionner lors de l’accès par un navigateur qui ne prend pas en charge JavaScript. Étant donné que Ajax dépend de JavaScript, certains développeurs évitent d’utiliser Ajax.

Toutefois, si vous êtes prudent de la façon dont vous implémentez Ajax vous pouvez créer des applications qui fonctionnent avec les navigateurs de niveau supérieur et de niveau inférieur. Notre application Contact Manager fonctionne avec les navigateurs qui prennent en charge JavaScript et les navigateurs qui ne le faites pas.

Si vous utilisez l’application Gestionnaire de contacts avec un navigateur qui prend en charge JavaScript vous aura une meilleure expérience utilisateur. Par exemple, lorsque vous cliquez sur un groupe de contacts, seule la région de la page qui affiche les contacts sera mis à jour.

Si, en revanche, vous utilisez l’application Gestionnaire de contacts avec un navigateur qui ne prend pas en charge JavaScript (ou qui aurait désactivé JavaScript) vous aura une expérience utilisateur légèrement moins précise. Par exemple, lorsque vous cliquez sur un groupe de contacts, toute la vue Index doit être publiée sur le navigateur pour afficher la liste des contacts correspondants.

## <a name="adding-the-required-javascript-files"></a>Ajout de fichiers JavaScript requis

Nous allons devoir utiliser les trois fichiers JavaScript pour ajouter la fonctionnalité Ajax à notre application. Trois de ces fichiers sont inclus dans le dossier Scripts d’une application ASP.NET MVC.

Si vous envisagez d’utiliser Ajax dans plusieurs pages dans votre application puis il judicieux d’inclure les fichiers JavaScript requis dans votre page maître de vue s d’une application. De cette façon, les fichiers JavaScript doit être inclus dans toutes les pages dans votre application automatiquement.

Ajoutez le code JavaScript suivant inclut à l’intérieur de la &lt;head&gt; balise de votre page maître de vue :

[!code-html[Main](iteration-7-add-ajax-functionality-vb/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>Refactorisation de la vue Index pour utiliser Ajax

Laissez s commencez par modifier notre vue Index afin qu’en cliquant sur un groupe de contacts met à jour uniquement pour la région de la vue qui affiche les contacts. La zone rouge dans la Figure 1 contient la région que nous souhaitons mettre à jour.


[![La mise à jour uniquement les contacts](iteration-7-add-ajax-functionality-vb/_static/image1.jpg)](iteration-7-add-ajax-functionality-vb/_static/image1.png)

**Figure 01**: la mise à jour uniquement les contacts ([cliquez pour afficher l’image en taille réelle](iteration-7-add-ajax-functionality-vb/_static/image2.png))


La première étape consiste à séparer la partie de la vue que nous souhaitons mettre à jour de façon asynchrone dans un partiel distinct (contrôle utilisateur). La section de la vue Index qui affiche la table de contacts a été déplacée dans le partielle dans le Listing 1.

**Liste 1 - Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample2.aspx)]

Remarquez qu’un autre modèle que la vue Index partielle dans le Listing 1. Le *Inherits* d’attribut dans le &lt;% @ Page %&gt; directive spécifie que le partielle hérite le ViewUserControl&lt;groupe&gt; classe.

La vue Index mis à jour est contenue dans le Listing 2.

**Listing 2 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample3.aspx)]

Voici deux choses que vous devriez remarquer que sur la vue mise à jour dans le Listing 2. Tout d’abord, notez que tous les éléments déplacés vers le partielle est remplacé par un appel à Html.RenderPartial(). La méthode Html.RenderPartial() est appelée lors de la première requête de la vue Index afin d’afficher l’ensemble initial de contacts.

En second lieu, notez que le Html.ActionLink() utilisé pour afficher les groupes de contact a été remplacée par une Ajax.ActionLink(). Le Ajax.ActionLink() est appelée avec les paramètres suivants :

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample4.aspx)]

Le premier paramètre représente le texte à afficher pour le lien, le deuxième paramètre représente les valeurs d’itinéraire et le troisième paramètre représente les options Ajax. Dans ce cas, nous utilisons l’option UpdateTargetId Ajax pour pointer vers le code HTML &lt;div&gt; balise que nous souhaitons mettre à jour une fois terminée la requête Ajax. Nous souhaitons mettre à jour le &lt;div&gt; balise avec la nouvelle liste de contacts.

La méthode Index() mis à jour du contrôleur de Contact est contenue dans le Listing 3.

**Liste 3 - Controllers\ContactController.vb (méthode d’Index)**

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample5.vb)]

L’action Index() mis à jour retourne conditionnellement une des deux choses. Si l’action Index() est appelée par une requête Ajax le contrôleur retourne un partiel. Sinon, l’action Index() retourne une vue entière.

Notez que l’action Index() sans devoir revenir autant de données lorsqu’elle est appelée par une requête Ajax. Dans le contexte d’une demande normale, l’action Index retourne une liste de tous les groupes de contact et le groupe de contact sélectionné. Dans le contexte d’une requête Ajax, l’action Index() retourne uniquement le groupe sélectionné. AJAX signifie moins de travail sur votre serveur de base de données.

Notre affichage Index modifié fonctionne dans le cas des navigateurs de niveau supérieur et de niveau inférieur. Si vous cliquez sur un groupe, et que votre navigateur prend en charge JavaScript, seule la région de la vue qui contient la liste de contacts est mis à jour. Si, en revanche, votre navigateur ne prend pas en charge JavaScript, toute la vue est mise à jour.


Notre vue Index mis à jour a un problème. Lorsque vous cliquez sur un groupe de contacts, le groupe sélectionné n’est pas mis en surbrillance. Étant donné que la liste des groupes s’affiche en dehors de la région qui est mis à jour pendant une requête Ajax, le groupe de droite ne pas obtenir mis en surbrillance. Nous allons corriger ce problème dans la section suivante.


## <a name="adding-jquery-animation-effects"></a>Ajout d’effets d’Animation de jQuery

Normalement, lorsque vous cliquez sur un lien dans une page web, vous pouvez utiliser la barre de progression du navigateur pour déterminer si le navigateur activement extrait le contenu mis à jour. Lorsque vous effectuez une requête Ajax, en revanche, la barre de progression de navigateur n’affiche pas un progrès quelconque. Cela peut rendre les utilisateurs nerveux. Comment savoir si le navigateur a figée ?

Il existe plusieurs méthodes que vous pouvez indiquer à l’utilisateur que le travail est effectué lors de l’exécution d’une requête Ajax. Une approche consiste à afficher une animation simple. Par exemple, vous pouvez fondu une région quand une requête Ajax commence et fondu dans la région à l’issue de la demande.

Nous allons utiliser la bibliothèque jQuery qui est incluse avec l’infrastructure Microsoft ASP.NET MVC, pour créer les effets d’animation. La vue Index mis à jour est contenue dans la liste 4.

**Liste 4 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample6.aspx)]

Notez que la vue Index mis à jour contient trois nouvelles fonctions JavaScript. Les deux premières fonctions utilisent jQuery pour fondu et fondu dans la liste des contacts lorsque vous cliquez sur un nouveau groupe de contact. La troisième fonction affiche un message d’erreur quand une requête Ajax génère une erreur (par exemple, délai d’expiration réseau).

La première fonction prend également en charge de mise en surbrillance le groupe sélectionné. Une classe = attribut sélectionné est ajouté à l’élément parent (l’élément LI) de l’élément cliqué. Là encore, jQuery facilite la sélection de l’élément approprié et ajouter la classe CSS.

Ces scripts sont liés aux liens de groupe à l’aide du paramètre Ajax.ActionLink() AjaxOptions. L’appel de méthode Ajax.ActionLink() mis à jour ressemble à ceci :

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>Ajout de prise en charge de l’historique de navigateur

Normalement, lorsque vous cliquez sur un lien vers une page de mise à jour, votre historique de navigation est mis à jour. De cette façon, vous pouvez cliquer sur le bouton précédent du navigateur pour revenir dans le temps à l’état précédent de la page. Par exemple, si vous cliquez sur le groupe de contact de vos amis et puis cliquez sur le groupe de contact d’entreprise, vous pouvez cliquer sur le bouton précédent du navigateur pour revenir à l’état de la page lorsque le groupe de contact amis a été sélectionné.

Malheureusement, une requête Ajax ne pas automatiquement mises à jour l’historique du navigateur. Si vous cliquez sur un groupe, et la liste de contacts correspondants est récupérée par une requête Ajax, l’historique du navigateur n’est pas actualisé. Vous ne pouvez pas utiliser le bouton précédent du navigateur pour revenir à un groupe de contacts après avoir sélectionné un nouveau groupe de contact.

Si vous souhaitez que les utilisateurs soient en mesure d’utiliser le précédent du navigateur bouton après avoir effectué des demandes Ajax vous devez effectuer un peu plus de travail. Vous avez besoin tirer parti de la fonctionnalité de gestion de l’historique de navigateur intégrée dans l’infrastructure ASP.NET AJAX.

Historique du navigateur ASP.NET AJAX, vous devez faire trois choses :

1. Activer l’historique du navigateur en définissant la propriété enableBrowserHistory sur true.
2. Enregistrer les points d’historique lorsque l’état d’une vue est modifiée en appelant la méthode addHistoryPoint().
3. Reconstruire l’état de la vue lorsque l’événement de navigation est déclenché.

La vue Index mis à jour est contenue dans la liste 5.

**Liste 5 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample8.aspx)]

Dans la liste 5, l’historique du navigateur est activée dans la fonction pageInit(). La fonction pageInit() est également utilisée pour configurer le Gestionnaire d’événements pour l’événement de navigation. L’événement de navigation est déclenché chaque fois que le navigateur vers l’avant ou un bouton précédent provoque l’état de la page à modifier.

La méthode beginContactList() est appelée lorsque vous cliquez sur un groupe de contacts. Cette méthode crée un nouveau point d’historique en appelant la méthode addHistoryPoint(). L’id du groupe contact cliqué est ajouté à l’historique.

L’id de groupe est récupéré à partir d’un attribut expando sur le lien de contact de groupe. Le lien est restitué avec l’appel suivant à Ajax.ActionLink().

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample9.aspx)]

Le dernier paramètre passé à la Ajax.ActionLink() ajoute un attribut expando nommé groupid au lien (en minuscules pour assurer la compatibilité XHTML).

Lorsqu’un utilisateur atteint le précédent du navigateur ou le bouton suivant, l’événement de navigation est déclenché et la méthode navigate() est appelée. Cette méthode met à jour les contacts s’affichés dans la page pour correspondre à l’état de la page qui correspond au point d’historique de navigateur passé à la méthode navigate.

## <a name="performing-ajax-deletes"></a>L’exécution de Ajax supprime

Actuellement, pour supprimer un contact, vous devez cliquer sur le lien Supprimer et puis cliquez sur le bouton de suppression affiché dans la page de confirmation de suppression (voir Figure 2). Cela semble être un grand nombre de demandes de pages de faire quelque chose de simple comme la suppression d’un enregistrement de base de données.


[![La page de confirmation de suppression](iteration-7-add-ajax-functionality-vb/_static/image2.jpg)](iteration-7-add-ajax-functionality-vb/_static/image3.png)

**Figure 02**: la page de confirmation de suppression ([cliquez pour afficher l’image en taille réelle](iteration-7-add-ajax-functionality-vb/_static/image4.png))


Il est tentant de les ignorer la page de confirmation de suppression et de supprimer un contact directement à partir de la vue Index. Vous devez éviter la tentation, car cette approche ouvre votre application à des failles de sécurité. En règle générale, don t souhaitez effectuer une opération HTTP GET lors de l’appel d’une action qui modifie l’état de votre application web. Lorsque vous effectuez une suppression, que vous souhaitez effectuer une requête HTTP POST, ou mieux encore, une opération HTTP DELETE.

Le lien Supprimer est contenu dans le ContactList partielle. Une version mise à jour de la ContactList partielle est contenue dans la liste 6.

**Liste 6 - Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample10.aspx)]

Le lien Supprimer est rendu avec l’appel suivant à la méthode Ajax.ImageActionLink() :

[!code-aspx[Main](iteration-7-add-ajax-functionality-vb/samples/sample11.aspx)]

> [!NOTE] 
> 
> Le Ajax.ImageActionLink() n’est pas une partie standard de l’infrastructure ASP.NET MVC. Le Ajax.ImageActionLink() est des méthodes d’assistance personnalisés inclus dans le projet de gestionnaire de contacts.


Le paramètre AjaxOptions a deux propriétés. Tout d’abord, la propriété confirmer est utilisée pour afficher une boîte de dialogue contextuelle JavaScript confirmation. En second lieu, la propriété HttpMethod est utilisée pour effectuer une opération HTTP DELETE.

Liste 7 contient une nouvelle action AjaxDelete() qui a été ajoutée au contrôleur de Contact.

**Liste 7 - Controllers\ContactController.vb (AjaxDelete)**   

[!code-vb[Main](iteration-7-add-ajax-functionality-vb/samples/sample12.vb)]

L’action AjaxDelete() est décorée avec un attribut AcceptVerbs. Cet attribut empêche l’action invoquée, sauf par une opération HTTP autre qu’une opération HTTP DELETE. En particulier, vous ne peut pas appeler cette action avec un verbe HTTP GET.

Après avoir supprimé les enregistrements de base de données, vous devez afficher la liste mise à jour des contacts qui ne contient-elle pas l’enregistrement supprimé. La méthode AjaxDelete() retourne le ContactList partielle et la liste de mises à jour des contacts.

## <a name="summary"></a>Récapitulatif

Dans cette itération, nous avons ajouté la fonctionnalité Ajax à notre application de gestionnaire de contacts. Nous avons utilisé Ajax pour améliorer la réactivité et les performances de notre application.

Tout d’abord, nous avons refactorisé la vue Index afin qu’en cliquant sur un groupe de contacts ne met pas à jour la vue d’ensemble. Au lieu de cela, en cliquant sur un groupe de contacts met à jour uniquement la liste des contacts.

Ensuite, nous avons utilisé les effets d’animation jQuery pour fondu et fondu dans la liste des contacts. Ajout d’une animation à une application Ajax peut être utilisé pour fournir aux utilisateurs de l’application par l’équivalent d’une barre de progression du navigateur.

Nous avons également ajouté des navigateurs pris en charge de l’historique à notre application Ajax. Nous avons activé les utilisateurs de cliquer sur le précédent du navigateur et de boutons pour modifier l’état de la vue Index suivante.

Enfin, nous avons créé un lien de suppression qui prend en charge les opérations HTTP DELETE. Effectuer des suppressions d’Ajax, nous permettre aux utilisateurs de supprimer des enregistrements de base de données sans que l’utilisateur demander une page de confirmation de suppression supplémentaires.

> [!div class="step-by-step"]
> [Précédent](iteration-6-use-test-driven-development-vb.md)
