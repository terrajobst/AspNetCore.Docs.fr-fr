---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
title: Lutte contre les robots (c#) | Microsoft Docs
author: wenz
description: Les robots automatisés plâtre weblogs et autres sites Web indésirable, envoyer des formulaires de commentaire sans interaction de l’utilisateur. Le contrôle nobot de dans la Con AJAX ASP.NET...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0a1917e0-884a-4576-8e93-9ed660faae51
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-cs
msc.type: authoredcontent
ms.openlocfilehash: 52ed34e7640cd125a3b4c3b50ab760a7c1d713f1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826378"
---
<a name="fighting-bots-c"></a>Lutte contre les robots (c#)
====================
par [Christian Wenz](https://github.com/wenz)

[Télécharger le Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.cs.zip) ou [télécharger le PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0CS.pdf)

> Les robots automatisés plâtre weblogs et autres sites Web indésirable, envoyer des formulaires de commentaire sans interaction de l’utilisateur. Le contrôle nobot de dans ASP.NET AJAX Control Toolkit peut aider à lutter contre ces robots.


## <a name="overview"></a>Vue d'ensemble

Les robots automatisés plâtre weblogs et autres sites Web indésirable, envoyer des formulaires de commentaire sans interaction de l’utilisateur. Le contrôle nobot de dans ASP.NET AJAX Control Toolkit peut aider à lutter contre ces robots.

## <a name="steps"></a>Étapes

Une approche courante pour passer outre les robots consiste à utiliser des CAPTCHAs entièrement automatisée publique Turing test pour indiquer les ordinateurs et les uns des autres êtres humains. Un test Turing a été initialement un test où quelqu'un nécessaire pour décider si un partenaire de communication est un être humain ou un ordinateur. Dans le site web, un CAPTCHA se compose généralement d’une image avec certaines lettres déformées dessus. L’idée est qu’uniquement un humain peut lire les lettres de l’image, tandis que les algorithmes de reconnaissance optique de caractères échoue.

Il existe plusieurs avantages et inconvénients de cette approche, mais une discussion de ce est dépasse le cadre de ce didacticiel. Il existe cependant un contrôle dans ASP.NET AJAX Control Toolkit qui offre une approche similaire : `NoBot`. Il est plus facile à surmonter à un test CAPTCHA, mais il est très facile à utiliser et tarifs très bien sur les sites Web tels que des blogs où il est considéré comme un succès si la plupart des tentatives de spam sont vaincus, ce qui le `NoBot` peut faire.

`NoBot` intercepte la publication (postback) du formulaire web ASP.NET en cours si au moins une des conditions suivantes est remplie :

- Le navigateur ne parvient pas à résoudre un puzzle JavaScript (par exemple quand JavaScript est désactivé)
- L’utilisateur a envoyé le formulaire à rapide
- L’adresse IP du client a envoyé le formulaire trop souvent dans un certain laps de temps.

Pour vérifier ces conditions, la `NoBot` contrôle nécessite ces attributs (d'entre eux facultatif) :

- `ResponseMinimumDelaySeconds` quantité minimale de secondes entre les publications (postback)
- `CutoffWindowSeconds` longueur d’intervalle de temps dans laquelle les publications (postback) à partir d’une adresse IP est des mesures
- `CutoffMaximumInstances` quantité maximale de l’intervalle de temps, en secondes

Les demandes de balisage suivant au moins deux secondes s’écoulent entre publications (postback) et qu’il existe des publications que cinq maximum dans un intervalle de 30 secondes :

[!code-aspx[Main](fighting-bots-cs/samples/sample1.aspx)]

Comme d’habitude veillez à inclure le `ScriptManager` dans la page afin que la bibliothèque ASP.NET AJAX est chargée et les outils de contrôle peut être utilisé :

[!code-aspx[Main](fighting-bots-cs/samples/sample2.aspx)]

Car la plupart des vérifications `NoBot` fait se produisent sur le côté serveur, vous devez vérifier le résultat de ces validations. Cela est possible en appelant `NoBot`de `IsValid()` (méthode). Il possède un seul argument (comme un `out` paramètre /`ByRef` paramètre) qui est de type `NoBotState`. Sa représentation sous forme de chaîne contient la raison en cas d’échec de la vérification et `Valid` dans le cas contraire. Le code suivant génère un message en fonction de `NoBot`du résultat :

[!code-aspx[Main](fighting-bots-cs/samples/sample3.aspx)]

Enfin, vous avez besoin d’un formulaire à envoyer et un élément d’étiquette pour le message de sortie, et que vous avez terminé !

[!code-aspx[Main](fighting-bots-cs/samples/sample4.aspx)]

Lorsque vous exécutez ce script et désactivez JavaScript ou envoyez le formulaire dans les deux premières secondes ou soumettez le formulaire sept fois au sein de trente secondes, vous obtiendrez un message d’erreur. Toutefois utiliser ce contrôle avec soin, puisque qu’environ 90 à 95 % des utilisateurs ont JavaScript activated, par conséquent 5-10 % des utilisateurs ne sera pas `NoBot`du test.


[![Ce message d’erreur peut être dû à un robot](fighting-bots-cs/_static/image2.png)](fighting-bots-cs/_static/image1.png)

Ce message d’erreur peut être dû à un robot ([cliquez pour afficher l’image en taille réelle](fighting-bots-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](fighting-bots-vb.md)
