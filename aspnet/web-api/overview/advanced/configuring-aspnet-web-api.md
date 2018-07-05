---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: Configuration d’ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 8fd08098b5a425f2cbd7939f5f90550b98c34071
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387076"
---
<a name="configuring-aspnet-web-api-2"></a>Configuration d’ASP.NET Web API 2
====================
par [Mike Wasson](https://github.com/MikeWasson)

Cette rubrique décrit comment configurer l’API Web ASP.NET.

- [Paramètres de configuration](#settings)
- [Configuration des API Web avec hébergement ASP.NET](#webhost)
- [Configuration des API Web avec auto-hébergement OWIN](#selfhost)
- [Services d’API Web à l’international](#services)
- [Configuration par contrôleur](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>Paramètres de configuration

Paramètres de configuration d’API Web sont définies dans le [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) classe.

| Membre | Description |
| --- | --- |
| **DependencyResolver** | Permet l’injection de dépendances pour les contrôleurs. Consultez [à l’aide du résolveur de dépendance Web API](dependency-injection.md). |
| **Les filtres** | Filtres d’action. |
| **Formateurs** | [Formateurs de type de média](../formats-and-model-binding/media-formatters.md). |
| **IncludeErrorDetailPolicy** | Spécifie si le serveur doit inclure des détails de l’erreur, telles que les messages d’exception et des traces de pile dans les messages de réponse HTTP. Consultez [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)). |
| **Initialiseur** | Une fonction qui effectue l’initialisation finale de la **HttpConfiguration**. |
| **MessageHandlers** | [Gestionnaires de messages HTTP](http-message-handlers.md). |
| **ParameterBindingRules** | Une collection de règles pour la liaison de paramètres sur les actions de contrôleur. |
| **Propriétés** | Un jeu de propriétés génériques. |
| **Itinéraires** | La collection d’itinéraires. Consultez [routage dans ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md). |
| **Services** | La collection de services. Consultez [Services](#services). |


## <a name="prerequisites"></a>Prérequis

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional ou Enterprise Edition.

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>Configuration des API Web avec hébergement ASP.NET

Dans une application ASP.NET, configurer des API Web en appelant [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) dans le **Application\_Démarrer** (méthode). Le **configurer** méthode prend un délégué avec un seul paramètre de type **HttpConfiguration**. Effectuer toutes les votre configuration à l’intérieur du délégué.

Voici un exemple d’utilisation d’un délégué anonyme :

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

Dans Visual Studio 2017, le modèle de projet « Application de Web ASP.NET » configure automatiquement le code de configuration, si vous sélectionnez « API Web » dans le **nouveau projet ASP.NET** boîte de dialogue.

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

Le modèle de projet crée un fichier nommé WebApiConfig.cs à l’intérieur de l’application\_dossier de démarrage. Ce fichier de code définit le délégué dans lequel vous devez placer le code de configuration de votre API Web.

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

Le modèle de projet ajoute également le code qui appelle le délégué à partir de **Application\_Démarrer**.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>Configuration des API Web avec auto-hébergement OWIN

Si vous êtes d’auto-hébergement avec OWIN, créer un nouveau **HttpConfiguration** instance. Effectuer la configuration sur cette instance, puis passez l’instance à la **Owin.UseWebApi** méthode d’extension.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

Le didacticiel [OWIN d’utilisation pour auto-héberger ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) montre les étapes à suivre.

<a id="services"></a>
## <a name="global-web-api-services"></a>Services d’API Web à l’international

Le **HttpConfiguration.Services** collection contient un ensemble de services globales qui utilise des API Web pour effectuer diverses tâches, telles que de la négociation de contenu et de sélection du contrôleur.

> [!NOTE]
> Le **Services** collection n’est pas un mécanisme à usage général pour l’injection des services de découverte ou la dépendance. Il stocke uniquement les types de service qui sont connus pour l’infrastructure API Web.


Le **Services** collection est initialisée avec un ensemble de services par défaut, et vous pouvez fournir vos propres implémentations personnalisées. Certains services prend en charge plusieurs instances, alors que d’autres utilisateurs peuvent avoir qu’une seule instance. (Toutefois, vous pouvez également fournir des services au niveau du contrôleur ; consultez [par contrôleur Configuration](#percontrollerconfig).

Services d’Instance unique


| Service | Description |
| --- | --- |
| **IActionValueBinder** | Obtient une liaison pour un paramètre. |
| **IApiExplorer** | Obtient les descriptions des API exposées par l’application. Consultez [création d’une Page d’aide pour une API Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IAssembliesResolver** | Obtient une liste des assemblys de l’application. Consultez [routage et sélection d’Action](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IBodyModelValidator** | Valide un modèle qui est lu à partir du corps de demande par un formateur de type de média. |
| **IContentNegotiator** | Effectue une négociation de contenu. |
| **IDocumentationProvider** | Fournit la documentation pour les API. La valeur par défaut est **null**. Consultez [création d’une Page d’aide pour une API Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IHostBufferPolicySelector** | Indique si l’hôte doit mettre en mémoire tampon corps d’entité HTTP message. |
| **IHttpActionInvoker** | Appelle une action de contrôleur. Consultez [routage et sélection d’Action](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpActionSelector** | Sélectionne une action de contrôleur. Consultez [routage et sélection d’Action](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerActivator** | Active un contrôleur. Consultez [routage et sélection d’Action](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerSelector** | Sélectionne un contrôleur. Consultez [routage et sélection d’Action](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerTypeResolver** | Fournit une liste des types de contrôleur d’API Web dans l’application. Consultez [routage et sélection d’Action](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **ITraceManager** | Initialise l’infrastructure de suivi. Consultez [Tracing in ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **ITraceWriter** | Fournit un writer de suivi. La valeur par défaut est un writer de suivi de « no-op ». Consultez [Tracing in ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **IModelValidatorCache** | Fournit un cache de validateurs de modèle. |

Services de plusieurs instances


|                 Service                 |                                                                                                              Description                                                                                                               |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <strong>IFilterProvider</strong>     |                                                                                           Retourne une liste de filtres pour une action de contrôleur.                                                                                           |
|  <strong>ModelBinderProvider</strong>   |                                                                                                Retourne un classeur de modèles pour un type donné.                                                                                                |
| <strong>ModelMetadataProvider</strong>  |                                                                                                     Fournit des métadonnées pour un modèle.                                                                                                     |
| <strong>ModelValidatorProvider</strong> |                                                                                                   Fournit un validateur pour un modèle.                                                                                                    |
|  <strong>ValueProviderFactory</strong>  | Crée un fournisseur de valeur. Pour plus d’informations, consultez blog de Mike Stall [comment créer un fournisseur de valeur personnalisée dans l’API Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |

Pour ajouter une implémentation personnalisée à un service à instances multiples, appelez **ajouter** ou **insérer** sur le **Services** collection :

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

Pour remplacer un service à instance unique avec une implémentation personnalisée, appelez **remplacer** sur le **Services** collection :

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>Configuration par contrôleur

Vous pouvez remplacer les paramètres suivants sur une base par contrôleur :

- Formateurs de type de média
- Règles de liaison de paramètre
- Services

Pour ce faire, définissez un attribut personnalisé qui implémente le **IControllerConfiguration** interface. Ensuite, appliquez l’attribut au contrôleur.

L’exemple suivant remplace les formateurs de type de média par défaut par un formateur personnalisé.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

Le **IControllerConfiguration.Initialize** méthode accepte deux paramètres :

- Un **HttpControllerSettings** objet
- Un **HttpControllerDescriptor** objet

Le **HttpControllerDescriptor** contient une description du contrôleur, vous pouvez examiner à titre d’information (par exemple, pour faire la distinction entre les deux contrôleurs).

Utilisez le **HttpControllerSettings** objet utilisé pour configurer le contrôleur. Cet objet contient le sous-ensemble de paramètres de configuration que vous pouvez remplacer sur une base par contrôleur. Tous les paramètres que vous ne modifiez pas la valeur par défaut global **HttpConfiguration** objet.
