---
uid: whitepapers/request-validation
title: Validation des demandes - prévention des attaques de Script | Microsoft Docs
author: rick-anderson
description: Cet article décrit la fonctionnalité de validation de demande d’ASP.NET où, par défaut, l’application ne peut pas traiter non codé soumettre des contenus de HTML...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 087f30428602137e01f574825f3ebcd4db9285ff
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830527"
---
<a name="request-validation---preventing-script-attacks"></a>Validation des demandes - prévention des attaques de Script
====================
> Cet article décrit la fonctionnalité de validation de demande d’ASP.NET où, par défaut, l’application ne peut pas traiter du contenu HTML non codé envoyé au serveur. Cette fonctionnalité de validation de demande peut être désactivée lors de l’application a été conçue pour traiter en toute sécurité des données HTML.
> 
> S’applique à ASP.NET 1.1 et ASP.NET 2.0.


Validation de la demande, une fonctionnalité d’ASP.NET depuis la version 1.1, empêche l’acceptation de contenu HTML non encodé contenant le serveur. Cette fonctionnalité est conçue pour aider à éviter certaines attaques d’injection de script par laquelle le code de script client ou HTML peut être envoyée involontairement à un serveur, stockée et ensuite présenté à d’autres utilisateurs. Nous vous recommandons vivement que vous validez les données d’entrée et encoder en HTML, le cas échéant.

Par exemple, vous créez une page Web qui demande l’adresse de messagerie d’un utilisateur, puis stocke cette adresse dans une base de données de messagerie. Si l’utilisateur entre &lt;SCRIPT&gt;alerte (« hello from script »)&lt;/SCRIPT&gt; au lieu d’une adresse e-mail valide, lorsque ces données sont présentées, ce script peut être exécuté si le contenu n’est pas encodé correctement. La fonctionnalité de validation de demande d’ASP.NET permet d’éviter ce problème.

## <a name="why-this-feature-is-useful"></a>Pourquoi cette fonctionnalité est utile

De nombreux sites ne sont pas conscients qu’ils sont ouverts aux attaques par injection de script simple. Si l’objectif de ces attaques est de modifier le site en affichant le code HTML ou potentiellement exécuter un script client pour rediriger l’utilisateur vers un site pirate, les attaques par injection de script sont un problème que vous doivent faire face les développeurs Web.

Les attaques par injection de script sont une préoccupation de tous les développeurs web, si elles sont à l’aide de ASP.NET, ASP ou autres technologies de développement web.

La fonctionnalité de validation de demande ASP.NET empêche proactive ces attaques en n’autorisant ne pas de contenu HTML non codé être traités par le serveur, sauf si le développeur décide d’autoriser que le contenu.

## <a name="what-to-expect-error-page"></a>À quoi s’attendre : Page d’erreur

La capture d’écran ci-dessous montre un exemple de code ASP.NET :

![](request-validation/_static/image1.png)

Exécutez les résultats de ce code dans une page simple qui vous permet d’entrer du texte dans la zone de texte, cliquez sur le bouton et afficher le texte dans le contrôle d’étiquette :

![](request-validation/_static/image2.png)

Toutefois, ont été JavaScript, telles que `<script>alert("hello!")</script>` soit entré et soumis nous obtiendrions une exception :

![](request-validation/_static/image3.png)

Le message d’erreur indique qu’un « potentiellement dangereux Request.Form valeur a été détectée » et fournit plus de détails dans la description d’exactement ce qui s’est produite et comment modifier le comportement. Exemple :

Validation de la demande a détecté une valeur d’entrée du client potentiellement dangereux et traitement de la demande a été abandonné. Cette valeur peut indiquer une tentative de compromettre la sécurité de votre application, tel qu’une attaque de script entre sites. Vous pouvez désactiver la validation de la demande en définissant `validateRequest=false` dans la directive de Page ou dans la section de configuration. Toutefois, il est fortement recommandé que votre application vérifie explicitement toutes les entrées dans ce cas.

## <a name="disabling-request-validation-on-a-page"></a>Désactivation de la validation de demande sur une page

Pour désactiver la validation de demande sur une page, vous devez définir le `validateRequest` attribut de la directive de Page à `false`:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> Lors de la validation de la demande est désactivée, le contenu peut être soumis à une page ; C’est la responsabilité du développeur de page pour vous assurer que le contenu est correctement encodée ou traitée.

## <a name="disabling-request-validation-for-your-application"></a>Désactivation de la validation de demande pour votre application

Pour désactiver la validation de la demande pour votre application, vous devez modifier ou créer un fichier Web.config pour votre application et définissez l’attribut validateRequest de la `<pages />` section à `false`:

[!code-xml[Main](request-validation/samples/sample2.xml)]

Si vous souhaitez désactiver la validation de demande pour toutes les applications sur votre serveur, vous pouvez effectuer cette modification à votre fichier Machine.config.

> [!CAUTION]
> Lors de la validation de la demande est désactivée, le contenu peut être envoyé à votre application ; C’est la responsabilité du développeur de l’application pour vous assurer que le contenu est correctement encodée ou traitée.

Le code ci-dessous est modifié pour désactiver la validation de demande :

![](request-validation/_static/image4.png)

Maintenant, si le code JavaScript suivant a été entré dans la zone de texte `<script>alert("hello!")</script>` le résultat serait :

![](request-validation/_static/image5.png)

Pour éviter ce problème, avec la validation de la demande mises hors tension, nous devons au format HTML encoder le contenu.

## <a name="how-to-html-encode-content"></a>Comment coder du contenu au format HTML

Si vous avez désactivé la validation de la demande, il est conseillé de contenu à encoder en HTML qui est stocké pour une utilisation ultérieure. L’encodage HTML remplacent automatiquement les «&lt;'ou'&gt;' (ainsi que plusieurs autres symboles) avec HTML correspondantes représentation encodée. Par exemple, «&lt;« est remplacé par »&amp;lt ;' et '&gt;« est remplacé par »&amp;gt ;'. Les navigateurs utilisent ces codes spéciaux pour afficher le «&lt;« ou »&gt;» dans le navigateur.

Contenu peut être facilement codées en HTML sur le serveur en utilisant le `Server.HtmlEncode(string)` API. Contenu peut également être facilement décodée en HTML, autrement dit, restauré vers HTML standard à l’aide du `Server.HtmlDecode(string)` (méthode).

![](request-validation/_static/image6.png)

Ce qui entraîne :

![](request-validation/_static/image7.png)
