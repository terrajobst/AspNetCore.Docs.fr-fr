---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Utiliser AJAX pour fournir des mises à jour dynamiques | Microsoft Docs
author: microsoft
description: Étape 10 prend en charge les utilisateurs connectés à RSVP leur souhait de participer à un dîner, à l’aide d’une approche basée sur Ajax intégrée dans le détail dîner...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 9f11c4c15c0ac9bab8d53b18a4e07be4b864b2c7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825199"
---
<a name="use-ajax-to-deliver-dynamic-updates"></a>Utiliser AJAX pour fournir des mises à jour dynamiques
====================
par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit d’étape 10 a gratuit [« « l’application NerdDinner](introducing-the-nerddinner-tutorial.md) qui présente en détail comment créer un petit mais terminé, l’application web à l’aide d’ASP.NET MVC 1.
> 
> Étape 10 prend en charge les utilisateurs connectés à RSVP leur souhait de participer à un dîner, à l’aide d’une approche basée sur Ajax intégrée dans la page de détails dîner.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre le [mise en route avec MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de musique MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) didacticiels.


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a>NerdDinner étape 10 : Accepte d’AJAX, l’activation des RSVP

Nous allons maintenant implémenter la prise en charge pour les utilisateurs connectés à RSVP leur souhait de participer à un dîner. Nous allons activer cette option à l’aide d’une approche basée sur AJAX est intégrée dans la page de détails dinner.

### <a name="indicating-whether-the-user-is-rsvpd"></a>Qui indique si l’utilisateur est répondu

Les utilisateurs peuvent visiter le */Dinners/détails / [id*] URL pour afficher les détails sur un dîner particulier :

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

La méthode d’action est implémentée de Details() comme suit :

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

Notre première étape pour implémenter la prise en charge du protocole RSVP sera pour ajouter une méthode d’assistance de « IsUserRegistered(username) » à notre objet dîner (au sein de la classe partielle Dinner.cs que nous avons créé précédemment). Cette méthode d’assistance retourne true ou false selon si l’utilisateur est actuellement répondu pour le dîner :

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

Nous pouvons ensuite ajouter le code suivant à notre modèle de vue Details.aspx affiche un message approprié indiquant si l’utilisateur est inscrit ou non pour l’événement :

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

Et maintenant lorsqu’un utilisateur visite un dîner, ils sont inscrits pour ils voient ce message :

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

Et lorsqu’ils accèdent à un dîner ne sont pas inscrits pour ils voient le message ci-dessous :

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a>Implémentation de la méthode d’Action Register

Nous allons maintenant ajouter les fonctionnalités nécessaires pour permettre aux utilisateurs RSVP pour un dîner à partir de la page de détails.

Pour implémenter cela, nous allons créer une nouvelle classe « RSVPController » en cliquant sur le répertoire \Controllers et en choisissant Add -&gt;commande de menu de contrôleur.

Nous allons implémenter une méthode d’action « S’inscrire » au sein de la nouvelle classe RSVPController qui prend un id pour un dîner en tant qu’argument, récupère l’objet dîner approprié, vérifie si l’utilisateur connecté est actuellement dans la liste des utilisateurs qui se sont inscrits pour celui-ci et si pas ajoute un objet RSVP pour eux :

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

Notez que ci-dessus comment nous allons retourner une chaîne simple en tant que la sortie de la méthode d’action. Nous pourrions comporter ce message au sein d’un modèle de vue – mais dans la mesure où il est si petit nous utilisons simplement la méthode d’assistance Content() sur la classe de base de contrôleur et retour un message de chaîne du type ci-dessus.

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a>Appel de la méthode d’Action RSVPForEvent à l’aide d’AJAX

Nous allons utiliser AJAX pour appeler la méthode d’action Register notre mode Détails. L’implémentation de cela est assez facile. Tout d’abord, nous allons ajouter deux références de bibliothèque de script :

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

La première bibliothèque fait référence à la bibliothèque de script côté client AJAX ASP.NET core. Ce fichier est d’environ 24 Ko (compressé) et contient des fonctionnalités AJAX core côté client. La deuxième bibliothèque contient des fonctions utilitaires qui s’intègrent AJAX d’assistance méthodes intégrées de ASP.NET MVC (que nous allons utiliser peu de temps).

Nous pouvons ensuite la mise à jour le code du modèle de vue nous avons ajouté précédemment afin qu’au lieu de la génération d’un message « Vous n’êtes pas inscrit pour cet événement », nous avons à la place d’afficher un lien que lors de l’objet d’un push effectue un appel AJAX qui appelle notre méthode d’action RSVPForEvent sur notre contrôleur RSVP et RSVPs l’utilisateur :

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

La méthode d’assistance Ajax.ActionLink() utilisée ci-dessus est intégrée à ASP.NET MVC et est similaire à la méthode d’assistance Html.ActionLink(), sauf qu’au lieu d’effectuer une navigation standard rend un appel AJAX de la méthode d’action lorsque l’utilisateur clique sur le lien. Ci-dessus nous allons appeler la méthode d’action « S’inscrire » sur le contrôleur « RSVP » et le DinnerID en tant que le paramètre « id » en lui transmettant. Le dernier paramètre AjaxOptions que nous passons indique que nous voulons prendre le contenu retourné à partir de la méthode d’action et de mettre à jour le code HTML &lt;div&gt; élément sur la page dont l’id est « rsvpmsg ».

Et maintenant quand un utilisateur accède à un dîner qu’ils ne sont pas inscrits mais ils voient un lien vers le protocole RSVP pour qu’il :

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

S’il clique sur le lien « Inscrivez-vous à cet événement », ils veulent effectuer un appel AJAX à la méthode d’action de s’inscrire sur le contrôleur RSVP, et lorsqu’elle est terminée, ils voient un message mis à jour comme ci-dessous :

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

La bande passante réseau et le trafic impliqués lors de l’établissement de cet appel AJAX est très légère. Lorsque l’utilisateur clique sur le lien « Inscrivez-vous à cet événement », un petit HTTP POST réseau est demandé pour le */Dinners/Register/1* URL se présente comme ci-dessous sur le câble :

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

Et la réponse à partir de notre méthode d’action Register est simplement :

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

Cet appel léger est rapide et fonctionne même sur un réseau lent.

### <a name="adding-a-jquery-animation"></a>Ajout d’une Animation de jQuery

Nous avons implémenté les fonctionnalités AJAX fonctionnent bien et rapide. Il peut parfois se produire si vite, cependant, qu’un utilisateur n’avez peut-être remarqué que le lien RSVP a été remplacé par un nouveau texte. Pour rendre le résultat un peu plus évident, nous pouvons ajouter une animation simple pour attirer l’attention sur le message de mise à jour.

La valeur par défaut du modèle de projet ASP.NET MVC inclut jQuery – une bibliothèque JavaScript open source excellente (et très populaire) qui est également pris en charge par Microsoft. jQuery fournit un nombre de fonctionnalités, notamment une bibliothèque de sélection et effets nice DOM HTML.

Pour utiliser jQuery, nous allons tout d’abord ajouter une référence de script à celui-ci. Étant donné que nous allons utiliser jQuery dans différents emplacements au sein de notre site, nous allons ajouter la référence de script au sein de notre fichier de page maître Site.master afin que toutes les pages de l’utiliser.

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

*Conseil : Vérifiez que vous avez installé le correctif de logiciel JavaScript intellisense pour Visual Studio 2008 SP1 qui permet une prise en charge intellisense pour les fichiers JavaScript (y compris jQuery). Vous pouvez le télécharger à partir de : http://tinyurl.com/vs2008javascripthotfix*

Le code écrit à l’aide de JQuery souvent utilise un « $() » global méthode JavaScript qui Récupère un ou plusieurs éléments HTML à l’aide d’un sélecteur CSS. Par exemple, <em>$("#rsvpmsg")</em> sélectionne tout élément HTML avec l’id de rsvpmsg, tandis que <em>$(".something")</em> sélectionneriez tous les éléments avec le « quelque chose » CSS nom de la classe. Vous pouvez également écrire des requêtes plus avancées telles que « retourner tous les boutons radio activé » à l’aide d’une requête de sélecteur comme : <em>$(« entrée [@type= radio] [@checked] »)</em>.

Une fois que vous avez sélectionné des éléments, vous pouvez appeler des méthodes sur ces derniers entrent en action, comme les masquer : *$(#rsvpmsg").hide() » ;*

Pour notre scénario RSVP, nous allons définir une fonction JavaScript simple nommée « AnimateRSVPMessage » qui sélectionne le « rsvpmsg » &lt;div&gt; et anime la taille de son contenu de texte. Le code ci-dessous démarre le texte de petite, puis les causes à augmenter sur un laps de temps 400 millisecondes :

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

Nous pouvons ensuite AutoEventWireup cette fonction JavaScript à appeler après notre appel AJAX en passant son nom à notre méthode d’assistance de Ajax.ActionLink() (via le « OnSuccess » AjaxOptions propriété d’événement) :

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

Et maintenant lorsque l’utilisateur clique sur le lien « Inscrivez-vous à cet événement » et notre appel AJAX termine correctement, le message contenu envoyé sera animer et devenir volumineux :

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

En plus de fournir un événement « OnSuccess », l’objet AjaxOptions expose les méthodes OnBegin OnFailure, événements et OnComplete que vous pouvez gérer (ainsi que divers autres propriétés et les options utiles).

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a>Nettoyage - refactoriser une vue partielle RSVP

Notre modèle de vue Détails commence à obtenir un peu long, les heures supplémentaires rend un peu plus difficile à comprendre. Pour aider à améliorer la lisibilité du code, nous allons terminer en créant une vue partielle – RSVPStatus.ascx – qui encapsule tout le code de vue RSVP pour notre page de détails.

Nous pouvons le faire en cliquant sur le dossier \Views\Dinners puis en choisissant Add -&gt;afficher la commande de menu. Nous en aurons il prennent un objet dîner comme son ViewModel fortement typée. Nous pouvons ensuite copier/coller le contenu RSVP à partir de notre fichier details.aspx dedans.

Une fois que nous avons fait cela, nous allons également créer une autre vue partielle – EditAndDeleteLinks.ascx - qui encapsule notre code de vue de lien Edit et Delete. Nous allons également l’avoir prennent un objet dîner comme son modèle de vues fortement typées et copier/coller la logique de modifier et supprimer à partir de notre fichier details.aspx dedans.

Notre détails afficher le modèle peut ensuitent simplement incluent deux appels de méthode Html.RenderPartial() en bas :

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

Cela rend le code plus clair à lire et à gérer.

### <a name="next-step"></a>Étape suivante

Examinons à présent comment nous pouvons utiliser AJAX encore plus loin et ajouter la prise en charge du mappage interactives à notre application.

> [!div class="step-by-step"]
> [Précédent](secure-applications-using-authentication-and-authorization.md)
> [Suivant](use-ajax-to-implement-mapping-scenarios.md)
