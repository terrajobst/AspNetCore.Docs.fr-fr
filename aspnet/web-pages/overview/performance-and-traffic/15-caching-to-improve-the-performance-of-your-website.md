---
uid: web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
title: La mise en cache des données dans une application Web Pages (Razor) Site pour optimiser les performances | Microsoft Docs
author: tfitzmac
description: 'Vous pouvez accélérer votre site Web en le faisant magasin : autrement dit, cache - les résultats de données qui normalement prendrait beaucoup de temps pour récupérer ou traiter un...'
ms.author: riande
ms.date: 02/14/2014
ms.assetid: 961e525b-7700-469e-8a68-d7010b6fb68c
msc.legacyurl: /web-pages/overview/performance-and-traffic/15-caching-to-improve-the-performance-of-your-website
msc.type: authoredcontent
ms.openlocfilehash: 53a3f38c23dc20479ddf231fe24a200d823d0345
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838101"
---
<a name="caching-data-in-an-aspnet-web-pages-razor-site-for-better-performance"></a>La mise en cache des données dans un Site ASP.NET Web Pages (Razor) pour optimiser les performances
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment utiliser une application d’assistance en cache les informations pour améliorer les performances dans un site Web ASP.NET Web Pages (Razor). Vous pouvez accélérer votre site Web en le faisant stocker &#8212; , autrement dit, mettre en cache &#8212; les résultats de données qui normalement prendrait beaucoup de temps pour récupérer ou traiter et qui ne changent pas souvent.
> 
> **Ce que vous allez apprendre :** 
> 
> - Comment utiliser la mise en cache pour améliorer la réactivité de votre site Web.
> 
> Voici les fonctionnalités d’ASP.NET introduites dans l’article :
> 
> - Le `WebCache` helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Ce didacticiel fonctionne également avec ASP.NET Web Pages 2.


Chaque fois qu’un utilisateur demande une page de votre site, le serveur web doit effectuer un travail afin de répondre à la demande. Pour certaines de vos pages, le serveur peut devoir effectuer des tâches qui prennent (comparativement) beaucoup de temps, par exemple pour récupérer des données à partir d’une base de données. Même si ces tâches ne prennent pas longtemps en termes absolus, si votre site connaît un trafic important, une série de requêtes individuelles qui provoquent le serveur web effectuer la tâche complexe ou lente peut totaliser une grande quantité de travail. Cela peut finalement affecter les performances du site.

Une façon d’améliorer les performances de votre site Web dans un cas comme celui-ci est en cache des données. Si votre site reçoit des requêtes répétées pour les mêmes informations et les informations n’a pas besoin d’être modifiées pour chaque personne, et il n’est pas le temps sensibles, au lieu de nouveau l’extraction ou le recalcul, vous pouvez extraire les données qu’une seule fois et puis stocker les résultats. La prochaine fois qu’une demande arrive pour que plus d’informations, vous simplement obtenir hors du cache.

En règle générale, vous mettez en cache les informations qui ne changent pas fréquemment. Quand vous placez des informations dans le cache, il est stocké en mémoire sur le serveur web. Vous pouvez spécifier la durée pendant laquelle il doit être mis en cache, de quelques secondes à jours. Lorsque la période de mise en cache expire, les informations sont supprimées automatiquement à partir du cache.

> [!NOTE]
> Entrées dans le cache risque d’être retirées pour des raisons autres que qu’ils ont expiré. Par exemple, le serveur web peut temporairement manquer de mémoire, et une façon qu’il puisse récupérer la mémoire est en levant des entrées du cache. Comme vous le verrez, même si vous avez fourni d’informations dans le cache, vous devez vérifiez si qu'elle existe toujours lorsque vous en avez besoin.


Imaginez que votre site Web comporte une page qui affiche la température actuelle et les prévisions météorologiques. Pour obtenir ce type d’information, vous pouvez envoyer une demande à un service externe. Étant donné que ces informations ne changent pas beaucoup (dans un délai de deux heures, par exemple) et dans la mesure où les appels externes prendre du temps et bande passante, il est un bon candidat pour la mise en cache.

## <a name="adding-caching-to-a-page"></a>Ajout de la mise en cache à une Page

ASP.NET inclut un `WebCache` helper qui permet de facilement ajouter la mise en cache à votre site et ajouter des données dans le cache. Dans cette procédure, vous allez créer une page qui met en cache de l’heure actuelle. Ce n’est pas un exemple concret, étant donné que l’heure actuelle est quelque chose qui ne changent pas souvent et qui n’est plus complexe à calculer. Toutefois, il est un bon moyen pour illustrer la mise en cache en action.

1. Ajoutez une nouvelle page nommée *WebCache.cshtml* au site Web.
2. Ajoutez le code et le balisage suivant à la page :

    [!code-cshtml[Main](15-caching-to-improve-the-performance-of-your-website/samples/sample1.cshtml)]

    Lors de la mise en cache des données, vous placez dans le cache à l’aide d’un nom de ceci est unique sur le site Web. Dans ce cas, vous allez utiliser une entrée de cache nommée `CachedTime`. Il s’agit du `cacheItemKey` indiqué dans l’exemple de code.

    Le code lit d’abord le `CachedTime` l’entrée de cache. Si une valeur est renvoyée (autrement dit, si l’entrée de cache n’est pas null), le code définit simplement la valeur de la variable d’heure pour les données du cache.

    Toutefois, si l’entrée de cache n’existe pas (autrement dit, il est null), le code définit la valeur d’heure, il ajoute au cache et définit une valeur d’expiration d’une minute. Une fois la minute, l’entrée du cache est ignorée. (La valeur d’expiration par défaut pour un élément dans le cache est de 20 minutes). La commande `WebCache.Set(cacheItemKey, time, 1, false)` montre comment ajouter la valeur de l’heure actuelle au cache et défini son expiration sur 1 minute. Définition de la *slidingExpiration* paramètre `false` signifie que le délai d’expiration n’est pas renouvelé chaque fois qu’il est demandé. Date d’expiration exactement 1 minute après que qu’il a été ajouté au cache. Si vous définissez cette valeur sur `true` le délai d’expiration de 1 minute est réinitialisé chaque fois que la valeur est demandée à partir du cache.

    Ce code illustre le modèle que vous devez toujours utiliser lorsque le cache de données. Avant de commencer quelque chose dans le cache, toujours vérifier d’abord si le `WebCache.Get` méthode a retourné une valeur null. N’oubliez pas que l’entrée de cache a peut-être expiré ou a peut-être été supprimée pour une raison quelconque, donc toute entrée donnée n’est jamais assurée de se trouver dans le cache.
3. Exécutez *WebCache.cshtml* dans un navigateur. (Assurez-vous que la page est sélectionnée dans le **fichiers** espace de travail avant de l’exécuter.) La première fois que vous demandez la page, les données d’heure n’est pas dans le cache et le code doit ajouter la valeur de temps au cache.

    ![cache-1](15-caching-to-improve-the-performance-of-your-website/_static/image1.jpg)
4. Actualiser *WebCache.cshtml* dans le navigateur. Cette fois-ci, les données d’heure sont dans le cache. Notez que l’heure n’a pas changé depuis la dernière fois que vous avez consulté la page.

    ![cache-2](15-caching-to-improve-the-performance-of-your-website/_static/image2.jpg)
5. Attendez une minute pour le cache à être vidé, puis actualisez la page. Encore une fois, la page indique que les données de temps n’a pas été trouvées dans le cache et l’heure de mise à jour est ajoutée au cache.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires


- [Affichage de données dans un graphique](https://go.microsoft.com/fwlink/?LinkId=202895)
- [Référence de l’API de WebCache](https://msdn.microsoft.com/library/system.web.helpers.webcache(v=vs.99).aspx) (MSDN)
