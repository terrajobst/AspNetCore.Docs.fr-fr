---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: Éléments à ne pas faire dans ASP.NET et comment réagir à la place | Microsoft Docs
author: tfitzmac
description: Cette rubrique décrit les erreurs courantes plusieurs personnes effectué dans les projets web ASP.NET. Il fournit des recommandations pour la procédure à suivre pour éviter ces Commu...
ms.author: riande
ms.date: 05/08/2014
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 7f2366ef8cb258a08c3cb96407f605864e5ec9a9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831178"
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a>Éléments à ne pas faire dans ASP.NET et comment réagir à la place
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Cette rubrique décrit les erreurs courantes plusieurs personnes effectué dans les projets web ASP.NET. Il fournit des recommandations pour la procédure à suivre pour éviter ces erreurs courantes. Il est basé sur un [présentation](http://vimeo.com/68390507) par **Damian Edwards** à couronne conférence de développeurs.


## <a name="disclaimer"></a>Exclusion de responsabilité

Cette rubrique n'est pas conçue comme un guide complet pour vous assurer de que votre application est sécurisé et efficace. Vous devez toujours suivre les meilleures pratiques pour la sécurité et de performances qui ne sont pas décrites dans cette rubrique. Il suggère uniquement comment éviter les erreurs courantes liées aux processus et des classes .NET.

## <a name="overview"></a>Vue d'ensemble

Cette rubrique contient les sections suivantes :

- [Conformité aux normes](#standards)

    - [Adaptateurs de contrôle](#adapters)
    - [Propriétés de style des contrôles](#styleprop)
    - [Page et les rappels de contrôle](#callback)
    - [Détection de fonctionnalité de navigateur](#browsercap)
- [Sécurité](#security)

    - [Validation de la demande](#validation)
    - [Session et l’authentification par formulaire sans cookie](#cookieless)
    - [EnableViewStateMac](#viewstatemac)
    - [Confiance moyenne](#medium)
    - [&lt;appSettings&gt;](#appsettings)
    - [UrlPathEncode](#urlpathencode)
- [Fiabilité et performances](#performance)

    - [PreSendRequestHeaders et PreSendRequestContent](#presend)
    - [Événements de Page asynchrone avec les Web Forms](#asyncevents)
    - [Déclenchement et oubli de travail](#fire)
    - [Corps d’entité](#requestentity)
    - [Compatibilité entre Response.Redirect et Response.End](#redirect)
    - [EnableViewState et ViewStateMode](#viewstatemode)
    - [SqlMembershipProvider](#sqlprovider)
    - [Les longues demandes en cours d’exécution (> 110 secondes)](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a>Conformité aux normes

<a id="adapters"></a>

### <a name="control-adapters"></a>Adaptateurs de contrôle

Recommandation : Arrêtez à l’aide des adaptateurs de contrôle pour le rendu adaptatif et utiliser à la place des requêtes de média CSS et HTML conforme aux normes.

Les adaptateurs de contrôles ont été introduites dans .NET 2.0 pour restituer le code de présentation qui a été personnalisé pour les environnements et les différents appareils. À présent, ce rendu adaptatif peut être effectué avec CSS et HTML. Vous devez cesser d’utiliser les adaptateurs de contrôle et convertir les adaptateurs existants vers CSS et HTML.

Pour plus d’informations, consultez [les requêtes de média](http://www.w3.org/TR/css3-mediaqueries/) et [Comment : ajouter des Pages mobiles à vos pages Web Forms ASP.NET / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a>Propriétés de style des contrôles

Recommandation : Arrêter de valeurs de style dans le balisage du contrôle et définir à la place des valeurs de mise en forme dans les feuilles de style CSS.

Les contrôles serveur Web contiennent des dizaines de propriétés qui peuvent être utilisées pour définir les propriétés de style intraligne. Par exemple, la propriété ForeColor définit la couleur du texte pour un contrôle. Vous pouvez obtenir cet effet même plus efficacement par le biais de feuilles de style CSS. Feuilles de style permettent de centraliser les valeurs de style et évitez de définir ces valeurs dans toute votre application.

L’exemple suivant montre une classe CSS le texte de jeux en rouge.

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

L’exemple suivant montre comment appliquer de manière dynamique la classe CSS.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a>Page et les rappels de contrôle

Recommandation : Arrêter d’utiliser les rappels de page et de contrôle et à la place utiliser les éléments suivants : AJAX, UpdatePanel, méthodes d’action MVC, API Web ou SignalR.

Dans les versions antérieures d’ASP.NET, Page et contrôle des méthodes de rappel vous autorisé à mettre à jour de la partie de la page web sans actualiser une page entière. Vous pouvez désormais effectuer des mises à jour de page partielle via [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [API Web](../../../web-api/index.md) ou [SignalR](../../../signalr/index.md). Vous devez arrêter à l’aide de méthodes de rappel car ils peuvent provoquer des problèmes avec des URL conviviales et le routage. Par défaut, les contrôles ne permettent pas de méthodes de rappel, mais si vous avez activé cette fonctionnalité dans un contrôle, vous devez le désactiver.

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a>Détection de fonctionnalité de navigateur

Recommandation : Arrêtez à l’aide de la détection de fonctionnalité de navigateur statique et à la place utiliser la détection de fonctionnalité dynamique.

Dans les versions antérieures d’ASP.NET, les fonctionnalités prises en charge pour chaque navigateur ont été stockées dans un fichier XML. Prise en charge des fonctionnalités Détection via une recherche statique n’est pas la meilleure approche. Maintenant, vous pouvez détecter dynamiquement un navigateur de fonctionnalités prises en charge en utilisant une infrastructure de détection de fonctionnalité, telle que [Modernizr](http://modernizr.com/). Détection de fonctionnalité détermine la prise en charge en tente d’utiliser une méthode ou propriété, puis cochez pour voir si le navigateur a produit le résultat souhaité. Par défaut, Modernizr est inclus dans les modèles d’application Web.

<a id="security"></a>

## <a name="security"></a>Sécurité

<a id="validation"></a>

### <a name="request-validation"></a>Validation de la demande

Recommandation : Valider l’entrée utilisateur et encoder une sortie à partir des utilisateurs.

Validation de la demande est une fonctionnalité d’ASP.NET qui inspecte chaque requête et s’arrête à la demande si une menace perçue est trouvée. Ne dépendent pas de validation de la demande pour la sécurisation de votre application contre les attaques de script entre sites. Au lieu de cela, validez toutes les entrées des utilisateurs et encoder la sortie. Dans certains cas, vous pouvez utiliser des expressions régulières pour valider l’entrée, mais dans les cas plus complexes, vous devez valider l’entrée utilisateur à l’aide de classes .NET qui déterminent si la valeur correspond à des valeurs autorisées.

L’exemple suivant montre comment utiliser une méthode statique dans la classe Uri pour déterminer si l’Uri fourni par un utilisateur est valide.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

Toutefois, pour vérifier suffisamment l’Uri, vous devez également vérifier pour vous assurer qu’il spécifie `http` ou `https`. L’exemple suivant utilise les méthodes d’instance pour vérifier que l’Uri est valide.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

Avant le rendu des entrées d’utilisateur au format HTML ou d’inclure l’entrée d’utilisateur dans une requête SQL, encoder les valeurs pour garantir un code malveillant n’est pas inclus.

Vous pouvez HTML encoder la valeur dans le balisage avec la &lt;% : %&gt; syntaxe, comme indiqué ci-dessous.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

Ou, dans la syntaxe Razor, vous pouvez HTML Encoder avec @, comme illustré ci-dessous.

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

L’exemple suivant montre comment au format HTML encode une valeur dans le code-behind.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

Pour encoder en toute sécurité une valeur pour les commandes SQL, utilisez les paramètres de commande tels que le [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx). <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a>Session et l’authentification par formulaire sans cookie

Recommandation : Recours à des cookies.

En passant les informations d’authentification dans la chaîne de requête n’est pas sécurisée. Par conséquent, requièrent des cookies lorsque votre application inclut une authentification. Si votre cookie stocke des informations sensibles, envisagez d’exiger SSL pour le cookie.

L’exemple suivant montre comment spécifier dans le fichier Web.config que l’authentification par formulaire requiert un cookie qui est transmis via le protocole SSL.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a>EnableViewStateMac

Recommandation : Jamais a la valeur false.

Par défaut, EnbableViewStateMac est définie sur true. Même si votre application n’utilise pas l’état d’affichage, ne définissez pas EnableViewStateMac sur false. La valeur false rendre votre application vulnérable aux scripts entre sites.

À partir de ASP.NET 4.5.2, le runtime applique **EnableViewStateMac = true**. Même si vous le définissez sur false, le runtime ignore cette valeur et se poursuit avec la valeur définie sur true. Pour plus d’informations, consultez [ASP.NET 4.5.2 et EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).

L’exemple suivant montre comment définir EnableViewStateMac sur true. Il est inutile de réellement définir cette valeur sur true, car il est vrai par défaut. Toutefois, si vous avez affectez-lui false sur n’importe quelle page dans votre application, vous devez corriger immédiatement cette valeur.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a>Confiance moyenne

Recommandation : Ne dépendent pas de confiance moyenne (ou tout autre niveau de confiance) comme une limite de sécurité.

Confiance partielle ne protège pas correctement votre application et ne doit pas être utilisée. Au lieu de cela, utilisez la confiance totale et isoler les applications non approuvées dans des pools d’applications distincts. Exécutez en outre, chaque pool d’applications sous une identité unique. Pour plus d’informations, consultez [ASP.NET de confiance partielle ne garantit pas l’isolation des applications](https://support.microsoft.com/kb/2698981).

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a>&lt;appSettings&gt;

Recommandation : Ne désactivez pas les paramètres de sécurité dans &lt;appSettings&gt; élément.

L’élément appSettings contient de nombreuses valeurs qui sont nécessaires pour les mises à jour de sécurité. Vous ne devez pas modifier ou désactiver ces valeurs. Si vous devez désactiver ces valeurs lors du déploiement d’une mise à jour, réactivez immédiatement après avoir effectué le déploiement.

Pour plus d’informations, consultez [élément appSettings ASP.NET](https://msdn.microsoft.com/library/hh975440.aspx).

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a>UrlPathEncode

Recommandation : Utilisez [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) à la place.

La méthode UrlPathEncode a été ajoutée au .NET Framework pour résoudre un problème de compatibilité de navigateur très spécifique. Il n’encode pas correctement une URL et ne protège pas votre application à partir de scripts entre sites. Vous devez jamais l’utiliser dans votre application. Au lieu de cela, utilisez [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).

L’exemple suivant montre comment passer une URL encodée comme un paramètre de chaîne de requête pour un contrôle de lien hypertexte.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a>Fiabilité et performances

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a>PreSendRequestHeaders et PreSendRequestContent

Recommandation : N’utilisez pas ces événements avec des modules gérés. En revanche, écrire un module IIS natif pour effectuer la tâche requise. Consultez [création de Modules de Code natif HTTP](https://msdn.microsoft.com/library/ms693629.aspx).

Vous pouvez utiliser la [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) et [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) événements avec des modules IIS natifs.
> [!WARNING]
> N’utilisez pas `PreSendRequestHeaders` et `PreSendRequestContent` avec des modules managés qui implémentent `IHttpModule`. Définition de ces propriétés peut entraîner des problèmes avec les demandes asynchrones. La combinaison de l’Application demandée Routing (ARR) et websockets peut entraîner des exceptions de violation d’accès qui peuvent provoquer w3wp se bloque. Par exemple, iiscore ! W3_CONTEXT_BASE::GetIsLastNotification + 68 dans iiscore.dll a provoqué une exception de violation d’accès (0xC0000005).

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a>Événements de Page asynchrone avec les Web Forms

Recommandation : Dans Web Forms, éviter d’écrire async void méthodes pour les événements de cycle de vie de Page et à la place utiliser [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) pour le code asynchrone.

Lorsque vous marquez un événement de page avec **async** et **void**, vous ne peut pas déterminer lorsque le code asynchrone est terminé. Au lieu de cela, utilisez Page.RegisterAsyncTask pour exécuter le code asynchrone d’une manière qui vous permet d’effectuer le suivi de son achèvement.

L’exemple suivant montre un bouton Cliquez sur Gestionnaire qui contient le code asynchrone. Cet exemple inclut la lecture d’une valeur de chaîne de façon asynchrone, qui est fourni uniquement comme un exemple simplifié d’une tâche asynchrone et non comme une pratique recommandée.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

Si vous utilisez des tâches asynchrones, la valeur est le framework cible de runtime Http 4.5 dans le fichier Web.config. Définition de l’infrastructure cible sur 4.5 tours sur le nouveau contexte de synchronisation qui a été ajoutée dans .NET 4.5. Cette valeur est définie par défaut dans les nouveaux projets dans Visual Studio 2012, mais n’est ne pas être définie si vous travaillez avec un projet existant.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a>Déclenchement et oubli de travail

Recommandation : Lors du traitement d’une demande au sein d’ASP.NET, éviter le lancement de travail « fire-et-forget » (ces appelant la méthode ThreadPool.QueueUserWorkItem, ou création d’une minuterie qui appelle à plusieurs reprises un délégué).

Si votre application a du travail « fire-et-forget » qui s’exécute au sein d’ASP.NET, votre application peut être désynchronisée. À tout moment, le domaine d’application peut être détruit ce qui signifie que votre processus en cours ne corresponde plus à l’état actuel de l’application.

Vous devez déplacer ce type de travail en dehors d’ASP.NET. Vous pouvez utiliser les tâches Web, Service de Windows ou un rôle de travail dans Azure pour effectuer le travail en cours et exécuter ce code à partir d’un autre processus.

Si vous devez effectuer ce travail au sein d’ASP.NET, vous pouvez ajouter le package Nuget appelé [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) pour exécuter le code.

<a id="requestentity"></a>

### <a name="request-entity-body"></a>Corps d’entité

Recommandation : Éviter la lecture de Request.Form ou Request.InputStream avant d’exécuter l’événement du gestionnaire.

Dès que possible vous devez lire à partir de Request.Form ou Request.InputStream est au cours du gestionnaire exécuter l’événement. Dans MVC, le contrôleur est le gestionnaire et l’événement d’exécution est lorsque la méthode d’action s’exécute. Dans Web Forms, la Page est le gestionnaire et l’événement d’exécution est lorsque l’événement Page.Init se déclenche. Si vous lisez le corps d’entité de demande antérieure à l’événement d’exécution, vous interférer avec le traitement de la demande.

Si vous avez besoin lire le corps d’entité de demande avant l’événement d’exécution, utilisez [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) ou [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx). Lorsque vous utilisez GetBufferlessInputStream, vous obtenez le flux de données brutes à partir de la demande et assumer la responsabilité de traiter la requête entière. Après avoir appelé GetBufferlessInputStream, Request.Form et Request.InputStream ne sont pas disponibles, car ils n’ont pas été remplies par ASP.NET. Lorsque vous utilisez GetBufferedInputStream, vous obtenez une copie du flux de données à partir de la demande. Request.Form et Request.InputStream sont toujours disponibles plus loin dans la demande, car ASP.NET remplit l’autre exemplaire.

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a>Compatibilité entre Response.Redirect et Response.End

Recommandation : Tenez compte des différences dans les modalités de thread après avoir appelé [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).

Le [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) méthode appelle la méthode Response.End. Dans un processus synchrone, appelant Request.Redirect provoque le thread actuel abandonner immédiatement. Toutefois, dans un processus asynchrone, l’appel de Response.Redirect n’abandonne pas le thread actuel, afin de l’exécution de code se poursuit pour la demande. Dans un processus asynchrone, vous devez retourner la tâche à partir de la méthode pour arrêter l’exécution de code.

Dans un projet MVC, n’appelez pas Response.Redirect. Au lieu de cela, retourner un RedirectResult.

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a>EnableViewState et ViewStateMode

Recommandation : Utilisez ViewStateMode, au lieu de EnableViewState, pour fournir un contrôle granulaire sur laquelle les contrôles utilisent l’état d’affichage.

Lorsque vous définissez EnableViewState sur false dans la directive de Page, l’état d’affichage est désactivé pour tous les contrôles dans la page et ne peut pas être activée. Si vous souhaitez activer l’état d’affichage que pour certains contrôles dans votre page, la valeur ViewStateMode désactivé pour la Page.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

Ensuite, définissez ViewStateMode Enabled sur uniquement les contrôles qui doivent en fait état d’affichage.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

En activant l’état d’affichage pour seulement les contrôles qui en ont besoin, vous pouvez réduire la taille de l’état d’affichage pour vos pages web.

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a>SqlMembershipProvider

Recommandation : Utilisez les fournisseurs universels.

Dans les modèles de projet actuel, SqlMembershipProvider a été remplacé par [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), qui est disponible comme package NuGet. Si vous utilisez SqlMembershipProvider dans un projet qui a été généré avec une version antérieure des modèles, vous devez basculer vers les fournisseurs universels. Les fournisseurs universels fonctionnent avec toutes les bases de données qui sont prises en charge par Entity Framework.

Pour plus d’informations, consultez [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a>Requêtes longues (> 110 secondes)

Recommandation : Utilisez [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) ou [SignalR](../../../signalr/index.md) pour les clients connectés et utilisez les opérations d’e/s asynchrones.

Requêtes longues peuvent provoquer des résultats imprévisibles et des performances médiocres dans votre application web. Le paramètre de délai d’expiration par défaut pour une demande est de 110 secondes. Si vous utilisez l’état de session avec une requête longue, ASP.NET libère le verrou sur l’objet de Session après 110 secondes. Toutefois, votre application peut être au milieu d’une opération sur l’objet de Session lorsque le verrou est libéré, et l’opération ne peut pas terminer correctement. Si une deuxième requête à partir de l’utilisateur est bloquée pendant l’exécution de la première demande, la deuxième demande peut accéder à l’objet de Session dans un état incohérent.

Si votre application inclut des opérations d’e/s bloquantes (ou synchrones), l’application sera ne répond pas.

Pour améliorer les performances, utilisez les opérations d’e/s asynchrones dans le .NET Framework. En outre, utilisez WebSockets ou SignalR pour les clients qui se connectent au serveur. Ces fonctionnalités sont conçues pour gérer efficacement les requêtes longues.
