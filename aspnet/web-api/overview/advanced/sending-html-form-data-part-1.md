---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Envoi des données de formulaire HTML dans l’API Web ASP.NET : données de Form-UrlEncode | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/15/2012
ms.topic: article
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 24410c92df828d4aaaa3b91dd3e9fa14575fd300
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399868"
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a>Envoi des données de formulaire HTML dans l’API Web ASP.NET : données de Form-UrlEncode
====================
par [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-1-form-urlencoded-data"></a>Partie 1 : Données de Form-UrlEncode

Cet article explique comment publier des données de form-UrlEncode sur un contrôleur d’API Web.

- [Vue d’ensemble des formulaires HTML](#overview_of_html_forms)
- [Envoi de Types complexes](#sending_complex_types)
- [Envoi de données de formulaire via AJAX](#sending_form_data_via_ajax)
- [Envoi de Types simples](#sending_simple_types)

> [!NOTE]
> [Télécharger le projet achevé](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a>Vue d’ensemble des formulaires HTML

Utilisation de formulaires HTML soit GET ou POST pour envoyer des données au serveur. Le **méthode** attribut de la **formulaire** élément donne la méthode HTTP :

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

La méthode par défaut est GET. Si le formulaire utilise GET, le formulaire de données sont encodées dans l’URI sous la forme d’une chaîne de requête. Si le formulaire utilise POST, les données du formulaire sont placées dans le corps de la demande. Pour les données de publication, le **enctype** attribut spécifie le format du corps de la demande :

| Enctype | Description |
| --- | --- |
| application/x-www-form-urlencoded | Données de formulaire sont encodées sous forme de paires nom/valeur similaires à une chaîne de requête URI. Il s’agit du format par défaut d’une requête POST. |
| multipart/form-data | Données de formulaire sont encodées comme un message MIME en plusieurs parties. Utilisez ce format si vous téléchargez un fichier vers le serveur. |

Partie 1 de cet article examine de format x--www-form-urlencoded. [Partie 2](sending-html-form-data-part-2.md) décrit MIME en plusieurs parties.

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a>Envoi de Types complexes

En règle générale, vous enverrez un type complexe, composé de valeurs provenant de plusieurs contrôles de formulaire. Prenez en compte le modèle suivant qui représente une mise à jour d’état :

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

Voici un contrôleur d’API Web qui accepte un `Update` objet via POST.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> Ce contrôleur utilise [le routage basé sur l’action](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), de sorte que le modèle d’itinéraire est &quot;api / {controller} / {action} / {id}&quot;. Le client valide les données à &quot;/api/updates/complex&quot;.


Maintenant nous allons écrire un formulaire HTML pour les utilisateurs à envoyer une mise à jour d’état.

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

Notez que le **action** attribut sur le formulaire est l’URI de notre action de contrôleur. Voici le formulaire avec des valeurs entrées dans :

![](sending-html-form-data-part-1/_static/image1.png)

Lorsque l’utilisateur clique sur Envoyer, le navigateur envoie une requête HTTP similaire à ce qui suit :

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

Notez que le corps de la requête contient les données du formulaire, sous formatées de paires nom/valeur. API Web convertit automatiquement les paires nom/valeur dans une instance de la `Update` classe.

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a>Envoi de données de formulaire via AJAX

Lorsqu’un utilisateur soumet un formulaire, le navigateur navigue en dehors de la page actuelle et affiche le corps du message de réponse. C’est OK lorsque la réponse est une page HTML. Avec une API web, toutefois, le corps de réponse est généralement soit vide ou contient des données structurées, tel que JSON. Dans ce cas, il est plus judicieux d’envoyer les données du formulaire à l’aide d’AJAX demande, afin que la page peut traiter la réponse.

Le code suivant montre comment valider des données de formulaire à l’aide de jQuery.

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

Le jQuery **soumettre** fonction remplace l’action de formulaire par une nouvelle fonction. Cela remplace le comportement par défaut du bouton Envoyer. Le **sérialiser** fonction sérialise les données de formulaire dans les paires nom/valeur. Pour envoyer les données du formulaire sur le serveur, appelez `$.post()`.

Lorsque la demande est terminée, le `.success()` ou `.error()` gestionnaire affiche un message approprié pour l’utilisateur.

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a>Envoi de Types simples

Dans les sections précédentes, nous avons envoyé un type complexe, API Web désérialisée en une instance d’une classe de modèle. Vous pouvez également envoyer des types simples, telles qu’une chaîne.

> [!NOTE]
> Avant d’envoyer un type simple, envisagez d’encapsuler la valeur dans un type complexe à la place. Cela vous offre les avantages de la validation du modèle sur le côté serveur et rend plus facile à étendre votre modèle, si nécessaire.


Les étapes de base pour envoyer un type simple sont les mêmes, mais il existe deux différences subtiles. Tout d’abord, dans le contrôleur, vous devez décorer le nom du paramètre avec le **FromBody** attribut.

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

Par défaut, les API Web tente d’obtenir des types simples à partir de l’URI de demande. Le **FromBody** attribut indique à l’API Web pour lire la valeur à partir du corps de demande.

> [!NOTE]
> API Web lit le corps de réponse au maximum une fois, uniquement un seul paramètre d’une action peut provenir de corps de la demande. Si vous avez besoin obtenir plusieurs valeurs à partir du corps de demande, définir un type complexe.


En second lieu, le client doit envoyer la valeur au format suivant :

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

Plus précisément, la partie nom de la paire nom/valeur doit être vide pour un type simple. Pas tous les navigateurs prennent en charge pour les formulaires HTML, mais vous créez ce format dans le script comme suit :

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

Voici un exemple de formulaire :

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

Et Voici le script pour envoyer la valeur de formulaire. La seule différence avec le script précédent est l’argument passé à la **valider** (fonction).

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

Vous pouvez utiliser la même approche pour envoyer un tableau de types simples :

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a>Ressources supplémentaires

[Partie 2 : Chargement du fichier et MIME à parties multiples](sending-html-form-data-part-2.md)
