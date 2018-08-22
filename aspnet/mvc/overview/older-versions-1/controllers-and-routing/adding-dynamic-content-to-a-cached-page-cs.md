---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: Ajout de contenu dynamique à une Page mise en cache (c#) | Microsoft Docs
author: microsoft
description: Découvrez comment combiner le contenu dynamique et mise en cache dans la même page. Post-cache vous permet d’afficher le contenu dynamique, tels que bannière publications o...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: a03f943b936c68215d65dca92e62431642226993
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830751"
---
<a name="adding-dynamic-content-to-a-cached-page-c"></a>Ajout de contenu dynamique à une Page mise en cache (c#)
====================
par [Microsoft](https://github.com/microsoft)

> Découvrez comment combiner le contenu dynamique et mise en cache dans la même page. Post-cache vous permet d’afficher le contenu dynamique, tels que les publications de bannière ou de nouveaux éléments au sein d’une page qui a été une sortie mise en cache.


En tirant parti de la mise en cache de sortie, vous pouvez améliorer considérablement les performances d’une application ASP.NET MVC. Au lieu de la régénération d’une page chaque fois que la page est demandée, la page peut être générée une fois et mis en cache en mémoire pour plusieurs utilisateurs.

Mais il existe un problème. Que se passe-t-il si vous devez afficher le contenu dynamique dans la page ? Par exemple, imaginez que vous souhaitez afficher une publicité de bannière dans la page. Vous ne voulez pas la publication de la bannière doit être mis en cache afin que chaque utilisateur voit la même publication. Vous n’aurait pas gagner de l’argent de cette façon !

Heureusement, il existe une solution facile. Vous pouvez tirer parti d’une fonctionnalité de l’infrastructure ASP.NET appelée *post-cache*. Post-cache vous permet de remplacer le contenu dynamique dans une page qui a été mis en cache en mémoire.


Normalement, lors de la sortie du cache une page à l’aide de l’attribut [OutputCache], la page est mise en cache sur le serveur et le client (navigateur web). Lorsque vous utilisez post-cache, une page est mise en cache uniquement sur le serveur.


#### <a name="using-post-cache-substitution"></a>À l’aide de la substitution post-cache

À l’aide de la substitution post-cache nécessite deux étapes. Tout d’abord, vous devez définir une méthode qui retourne une chaîne qui représente le contenu dynamique que vous souhaitez afficher dans la page mise en cache. Ensuite, vous appelez la méthode HttpResponse.WriteSubstitution() pour injecter le contenu dynamique dans la page.

Par exemple, imaginez que vous souhaitez afficher au hasard des éléments d’informations différente dans une page mise en cache. La classe dans le Listing 1 expose une méthode unique, nommée RenderNews(), qui renvoie au hasard un élément d’information dans une liste de trois éléments d’actualités.

**Liste 1 – Models\News.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

Pour tirer parti de la substitution post-cache, vous appelez la méthode HttpResponse.WriteSubstitution(). La méthode WriteSubstitution() définit le code pour remplacer une région de la page mise en cache avec un contenu dynamique. La méthode WriteSubstitution() est utilisée pour afficher l’article aléatoire dans la vue dans la liste 2.

**Listing 2 – Views\Home\Index.aspx**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

La méthode RenderNews est passée à la méthode WriteSubstitution(). Notez que la méthode RenderNews n’est pas appelée (il existe sans parenthèses). Au lieu de cela, une référence à la méthode est passée à WriteSubstitution().

La vue Index est mis en cache. La vue est retournée par le contrôleur dans le Listing 3. Notez que l’action Index() est décorée avec un attribut [OutputCache] qui provoque l’affichage de l’Index doit être mis en cache pendant 60 secondes.

**Liste 3 – Controllers\HomeController.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

Même si la vue de l’Index est mis en cache, les éléments de news aléatoires différents sont affichés lorsque vous demandez la page d’Index. Lorsque vous demandez la page d’Index, l’heure affichée par la page ne change pas pendant 60 secondes (voir Figure 1). Le fait que l’heure ne change pas prouve que la page est mise en cache. Toutefois, le contenu injectées par les modifications de la méthode – aléatoire actualité – WriteSubstitution() avec chaque requête.

**Figure 1 – injectant des actualités dynamique dans une page mise en cache**

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>À l’aide de la substitution post-cache dans les méthodes d’assistance

Un moyen plus simple pour tirer parti de la substitution post-cache est d’encapsuler l’appel à la méthode WriteSubstitution() au sein d’une méthode d’assistance personnalisés. Cette approche est illustrée par la méthode d’assistance dans la liste 4.

**Liste 4 – AdHelper.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

Liste 4 contient une classe statique qui expose deux méthodes : RenderBanner() et RenderBannerInternal(). La méthode RenderBanner() représente la méthode d’assistance réelle. Cette méthode étend la classe HtmlHelper de MVC ASP.NET standard, afin que vous pouvez appeler Html.RenderBanner() dans une vue comme toute autre méthode d’assistance.

La méthode RenderBanner() appelle la méthode de HttpResponse.WriteSubstitution() en passant de la méthode RenderBannerInternal() à la méthode WriteSubsitution().

La méthode RenderBannerInternal() est une méthode privée. Cette méthode ne soient pas exposée en tant qu’une méthode d’assistance. La méthode RenderBannerInternal() retourne au hasard une image de bannière la publication dans une liste de trois images de publication de bannière.

La vue Index modifiée dans la liste 5 illustre comment vous pouvez utiliser la méthode d’assistance RenderBanner(). Notez que supplémentaires &lt;% @ Import %&gt; directive est incluse en haut de la vue pour importer l’espace de noms MvcApplication1.Helpers. Si vous oubliez d’importer cet espace de noms, la méthode RenderBanner() ne s’affiche en tant que méthode sur la propriété Html.

**Liste 5 – Views\Home\Index.aspx (avec la méthode RenderBanner())**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

Lorsque vous demandez la page rendue par la vue dans la liste 5, une publication de bannière différent est affichée avec chaque requête (voir Figure 2). La page est mise en cache, mais la publication de la bannière est injectée dynamiquement par la méthode d’assistance RenderBanner().

**Figure 2 – la vue Index affichant une publication de bannière aléatoire**

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a>Récapitulatif

Ce didacticiel vous a expliqué comment vous pouvez mettre à jour dynamiquement le contenu dans une page mise en cache. Vous avez appris comment utiliser la méthode HttpResponse.WriteSubstitution() pour permettre à injecter dans une page mise en cache du contenu dynamique. Vous avez également appris comment encapsuler l’appel à la méthode WriteSubstitution() dans une méthode d’assistance HTML.

Tirer parti de la mise en cache si possible : il peut avoir un impact considérable sur les performances de vos applications web. Comme expliqué dans ce didacticiel, vous pouvez tirer parti de la mise en cache même lorsque vous avez besoin afficher le contenu dynamique dans vos pages.

## 

## 

> [!div class="step-by-step"]
> [Précédent](improving-performance-with-output-caching-cs.md)
> [Suivant](creating-a-controller-cs.md)
