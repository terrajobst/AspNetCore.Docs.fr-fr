---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: Intergiciel (middleware) OWIN dans IIS intégré pipeline | Microsoft Docs
author: Praburaj
description: Cet article explique comment exécuter les composants d’intergiciel (middleware) OWIN (OMCs) dans le pipeline intégré IIS, et comment définir l’événement de pipeline un OMC s’exécute sur. Tu devrais...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2013
ms.topic: article
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 1c2f7a9b948331eae2f5b44f25219adb76a0c745
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379058"
---
<a name="owin-middleware-in-the-iis-integrated-pipeline"></a>Intergiciel (middleware) OWIN dans le pipeline intégré IIS
====================
par [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> Cet article explique comment exécuter les composants d’intergiciel (middleware) OWIN (OMCs) dans le pipeline intégré IIS, et comment définir l’événement de pipeline un OMC s’exécute sur. Vous devez examiner [une vue d’ensemble du projet Katana](an-overview-of-project-katana.md) et [détection de classe de démarrage OWIN](owin-startup-class-detection.md) avant la lecture de ce didacticiel. Ce didacticiel a été rédigé par Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Howard Dierking, Chris Ross et Praburaj Thiagarajan ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).


Bien que [OWIN](an-overview-of-project-katana.md) composants d’intergiciel (middleware) (OMCs) sont principalement conçus pour s’exécuter dans un pipeline indépendant du serveur, il est possible d’exécuter un OMC dans le pipeline intégré IIS ainsi (**en mode classique est *pas* pris en charge**). Un OMC possible de faire fonctionner dans le pipeline intégré IIS en installant le package suivant à partir du Package Manager de la console :

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

Cela signifie que toutes les infrastructures d’application, y compris celles qui ne sont pas encore en mesure de s’exécuter en dehors d’IIS et System.Web, peuvent bénéficier à partir des composants d’intergiciel (middleware) OWIN existants. 

> [!NOTE]
> Tous les `Microsoft.Owin.Security.*` packages livré avec le nouveau système d’identité dans Visual Studio 2013 (par exemple : les Cookies, Account Microsoft, Google, Facebook, Twitter, [le jeton du porteur](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html), OAuth, serveur d’autorisation, JWT, Azure Active répertoire et Active directory federation services) sont créés en tant que OMCs et peuvent être utilisée dans les scénarios auto-hébergés et hébergés dans IIS.

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>Comment l’intergiciel OWIN s’exécute dans le Pipeline intégré IIS

Pour les applications de console OWIN, le pipeline d’application créés à l’aide du [configuration de démarrage](owin-startup-class-detection.md) est définie par l’ordre les composants sont ajoutés à l’aide de la `IAppBuilder.Use` (méthode). Autrement dit, le pipeline OWIN dans le [Katana](an-overview-of-project-katana.md) runtime traitera OMCs dans l’ordre de leur inscription à l’aide de `IAppBuilder.Use`. Dans le pipeline intégré IIS, le pipeline de requête se compose de [HttpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx) abonné à un ensemble prédéfini d’événements de pipeline comme [BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx), etc.

Si l'on compare un OMC à celle d’un [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) dans le monde d’ASP.NET, un OMC doit être inscrit à l’événement correct de pipeline prédéfinies. Par exemple, l’HttpModule `MyModule` sera invoquée lorsqu’une demande arrive la [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) étape du pipeline :

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

Dans l’ordre pour un OMC à participer à ce classement de même, en fonction des événements de l’exécution, le [Katana](an-overview-of-project-katana.md) code du runtime parcourt la [configuration de démarrage](owin-startup-class-detection.md) et s’abonne à chacun des composants d’intergiciel (middleware) à un événements de pipeline intégré. Par exemple, le code suivant OMC et l’inscription vous permet de voir l’inscription d’événement par défaut des composants d’intergiciel (middleware). (Pour obtenir des instructions sur la création d’une classe de démarrage OWIN, consultez [détection de classe de démarrage OWIN](owin-startup-class-detection.md).)

1. Créez un projet d’application web vide et nommez-la **owin2**.
2. À partir de la Manager Console (package), exécutez la commande suivante : 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. Ajouter un `OWIN Startup Class` et nommez-le `Startup`. Remplacez le code généré par le code suivant (les modifications sont mises en surbrillance) :  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. Appuyez sur F5 pour exécuter l’application.

Il définit la configuration de démarrage d’un pipeline avec l’intergiciel (middleware) trois composants, les deux premières affichant des informations de diagnostic et la dernière réponse aux événements (et en affichant des informations de diagnostic). Le `PrintCurrentIntegratedPipelineStage` méthode affiche un message et l’événement de pipeline intégré ce middleware est appelé sur. Les fenêtres de sortie affiche les informations suivantes :

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

Le runtime Katana mappés chacun des composants d’intergiciel (middleware) OWIN pour [PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) par défaut, qui correspond à l’événement de pipeline IIS [PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx).

## <a name="stage-markers"></a>Marqueurs d’étape

Vous pouvez marquer OMCs à exécuter à plusieurs étapes du pipeline à l’aide de la `IAppBuilder UseStageMarker()` méthode d’extension. Pour exécuter un ensemble de composants d’intergiciel (middleware) au cours d’une étape particulière, insérez un marqueur d’étape juste après le dernier composant est l’ensemble lors de l’inscription. Il existe des règles sur quelle étape du pipeline, vous pouvez exécuter intergiciel (middleware) et les composants de la commande doivent s’exécuter (les règles sont expliquées plus loin dans ce didacticiel). Ajouter le `UseStageMarker` méthode à la `Configuration` de code comme indiqué ci-dessous :

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

Le `app.UseStageMarker(PipelineStage.Authenticate)` appel configure tous les composants d’intergiciel (middleware) précédemment enregistré (dans ce cas, il s’agit de nos deux composants de Diagnostics) pour s’exécuter sur l’étape d’authentification du pipeline. Le dernier composant d’intergiciel (middleware) (qui affiche des diagnostics et répond aux demandes) s’exécute sur le `ResolveCache` étape (la [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx) événement).

Appuyez sur F5 pour exécuter l’application. La fenêtre Sortie affiche les informations suivantes :

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>Règles de marqueur d’étape

Composants d’intergiciel (middleware) Owin (OMC) peuvent être configurés pour exécuter les événements d’étape de pipeline OWIN suivant :

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. Par défaut, OMCs s’exécutent sur le dernier événement (`PreHandlerExecute`). C’est pourquoi notre premier exemple de code affiche « PreExecuteRequestHandler ».
2. Vous pouvez utiliser l’un `app.UseStageMarker` méthode pour inscrire un OMC pour s’exécuter plus tôt, à n’importe quelle étape du pipeline OWIN répertoriées dans le `PipelineStage` enum.
3. Le pipeline OWIN et le pipeline IIS est triée, par conséquent, les appels à `app.UseStageMarker` doit être dans l’ordre. Impossible de définir le Gestionnaire d’événements à un événement qui précède le dernier événement inscrit auprès de `app.UseStageMarker`. Par exemple, *après* appelant :

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

   les appels à `app.UseStageMarker` passant `Authenticate` ou `PostAuthenticate` ne sera pas pris en compte, et aucune exception n’est levée. OMCs exécuter à la dernière étape, qui est par défaut `PreHandlerExecute`. Les marqueurs de la scène sont utilisés s’ils s’exécutent plus tôt. Si vous spécifiez des marqueurs de la scène en désordre, nous arrondir au marqueur antérieures. En d’autres termes, l’ajout d’un marqueur d’étape indique « Ne plus tard le stade X exécuter ». Exécution de OMC sur le marqueur d’étape plus tôt ajouté après ces derniers dans le pipeline OWIN.
4. La première étape d’appels à `app.UseStageMarker` wins. Par exemple, si vous changez l’ordre de `app.UseStageMarker` appels à partir de notre exemple précédent :

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

   Affiche la fenêtre Sortie : 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

   Les OMCs tous s’exécuter dans le `AuthenticateRequest` à l’étape, car le dernier OMC enregistrés avec le `Authenticate` événement et le `Authenticate` événement précède tous les autres événements.
