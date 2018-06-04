---
title: Modules IIS avec ASP.NET Core
author: guardrex
description: Découvrez les modules IIS actifs et inactifs pour les applications ASP.NET Core et comment gérer les modules IIS.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/modules
ms.openlocfilehash: 4a60b6c9bab77e8095cb9f19e615219817702b32
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34566644"
---
# <a name="iis-modules-with-aspnet-core"></a>Modules IIS avec ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

Les applications ASP.NET Core sont hébergées par IIS dans une configuration de proxy inverse. Certains des modules IIS natifs et tous les modules managés IIS ne sont pas disponibles pour traiter les requêtes liées aux applications ASP.NET Core. Dans de nombreux cas, ASP.NET Core offre une alternative aux fonctionnalités des modules natifs et managés IIS.

## <a name="native-modules"></a>Modules natifs

Le tableau indique les modules IIS natifs qui sont opérationnels sur les requêtes de proxy inverse destinées aux applications ASP.NET Core.

| Module | Opérationnel avec les applications ASP.NET Core | Option ASP.NET Core |
| ------ | :-------------------------------: | ------------------- |
| **Authentification anonyme**<br>`AnonymousAuthenticationModule` | Oui | |
| **Authentification de base**<br>`BasicAuthenticationModule` | Oui | |
| **Authentification par mappage de certification cliente**<br>`CertificateMappingAuthenticationModule` | Oui | |
| **CGI**<br>`CgiModule` | Non | |
| **Validation de configuration**<br>`ConfigurationValidationModule` | Oui | |
| **Erreurs HTTP**<br>`CustomErrorModule` | Non | [Middleware (intergiciel) de pages de codes d’état](xref:fundamentals/error-handling#configuring-status-code-pages) |
| **Journalisation personnalisée**<br>`CustomLoggingModule` | Oui | |
| **Document par défaut**<br>`DefaultDocumentModule` | Non | [Middleware de fichiers par défaut](xref:fundamentals/static-files#serve-a-default-document) |
| **Authentification Digest**<br>`DigestAuthenticationModule` | Oui | |
| **Exploration des répertoires**<br>`DirectoryListingModule` | Non | [Middleware d’exploration des répertoires](xref:fundamentals/static-files#enable-directory-browsing) |
| **Compression dynamique**<br>`DynamicCompressionModule` | Oui | [Intergiciel (middleware) de compression des réponses](xref:performance/response-compression) |
| **Suivi**<br>`FailedRequestsTracingModule` | Oui | [Journalisation ASP.NET Core](xref:fundamentals/logging/index#tracesource-provider) |
| **Mise en cache des fichiers**<br>`FileCacheModule` | Non | [Intergiciel de mise en cache des réponses](xref:performance/caching/middleware) |
| **Mise en cache HTTP**<br>`HttpCacheModule` | Non | [Intergiciel de mise en cache des réponses](xref:performance/caching/middleware) |
| **Journalisation HTTP**<br>`HttpLoggingModule` | Oui | [Journalisation ASP.NET Core](xref:fundamentals/logging/index)<br>Implémentations : [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging), [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging), [NLog](https://github.com/NLog/NLog.Extensions.Logging), [Serilog](https://github.com/serilog/serilog-extensions-logging)
| **Redirection HTTP**<br>`HttpRedirectionModule` | Oui | [Intergiciel de réécriture d’URL](xref:fundamentals/url-rewriting) |
| **Authentification par mappage de certificat client IIS**<br>`IISCertificateMappingAuthenticationModule` | Oui | |
| **Restrictions IP et de domaine**<br>`IpRestrictionModule` | Oui | |
| **Filtres ISAPI**<br>`IsapiFilterModule` | Oui | [Intergiciel (middleware)](xref:fundamentals/middleware/index) |
| **ISAPI**<br>`IsapiModule` | Oui | [Intergiciel (middleware)](xref:fundamentals/middleware/index) |
| **Prise en charge des protocoles**<br>`ProtocolSupportModule` | Oui | |
| **Filtrage des demandes**<br>`RequestFilteringModule` | Oui | [Middleware de réécriture d’URL`IRule`](xref:fundamentals/url-rewriting#irule-based-rule) |
| **Observateur de demandes**<br>`RequestMonitorModule` | Oui | |
| **Réécriture d’URL**<br>`RewriteModule` | Oui&#8224; | [Intergiciel de réécriture d’URL](xref:fundamentals/url-rewriting) |
| **Textes insérés par le serveur**<br>`ServerSideIncludeModule` | Non | |
| **Compression statique**<br>`StaticCompressionModule` | Non | [Intergiciel (middleware) de compression des réponses](xref:performance/response-compression) |
| **Contenu statique**<br>`StaticFileModule` | Non | [Middleware de fichiers statiques](xref:fundamentals/static-files) |
| **Mise en cache de jeton**<br>`TokenCacheModule` | Oui | |
| **Mise en cache d’URI**<br>`UriCacheModule` | Oui | |
| **Autorisation URL**<br>`UrlAuthorizationModule` | Oui | [Identité ASP.NET Core](xref:security/authentication/identity) |
| **Authentification Windows**<br>`WindowsAuthenticationModule` | Oui | |

&#8224;Les types de correspondance `isFile` et `isDirectory` du module de réécriture d’URL ne fonctionnent pas avec les applications ASP.NET Core en raison des modifications apportées à la [structure de répertoires](xref:host-and-deploy/directory-structure).

## <a name="managed-modules"></a>Modules managés

Les modules managés *ne sont pas opérationnels* avec les applications ASP.NET Core hébergées quand la version CLR .NET du pool d’applications est définie sur **Aucun code managé**. ASP.NET Core offre des alternatives de middleware dans plusieurs cas.

| Module                  | Option ASP.NET Core |
| ----------------------- | ------------------- |
| AnonymousIdentification | |
| DefaultAuthentication   | |
| FileAuthorization       | |
| FormsAuthentication     | [Middleware d’authentification par cookie](xref:security/authentication/cookie) |
| OutputCache             | [Intergiciel de mise en cache des réponses](xref:performance/caching/middleware) |
| Profil                 | |
| RoleManager             | |
| ScriptModule-4.0        | |
| Session                 | [Middleware de session](xref:fundamentals/app-state) |
| UrlAuthorization        | |
| UrlMappingsModule       | [Intergiciel de réécriture d’URL](xref:fundamentals/url-rewriting) |
| UrlRoutingModule-4.0    | [Identité ASP.NET Core](xref:security/authentication/identity) |
| WindowsAuthentication   | |

## <a name="iis-manager-application-changes"></a>Modification de l’application avec le Gestionnaire IIS

Quand vous utilisez le Gestionnaire IIS pour configurer les paramètres, le fichier *web.config* de l’application est modifié. Si vous déployez une application et que vous incluez *web.config*, toutes les modifications apportées avec le Gestionnaire IIS sont remplacées par le fichier *web.config* déployé. Si des modifications sont apportées au fichier *web.config* du serveur, copiez le fichier *web.config* mis à jour sur le serveur dans le projet local immédiatement.

## <a name="disabling-iis-modules"></a>Désactivation des modules IIS

Si un module IIS configuré au niveau du serveur doit être désactivé pour une application, un ajout au fichier *web.config* de l’application peut désactiver le module. Conservez le module en place et désactivez-le à l’aide d’un paramètre de configuration (si disponible) ou supprimez le module de l’application.

### <a name="module-deactivation"></a>Désactivation du module

De nombreux modules disposent d’un paramètre de configuration qui vous permet de les désactiver sans les supprimer de l’application. Il s’agit de la façon la plus simple et plus rapide pour désactiver un module. Par exemple, le module de redirection HTTP peut être désactivé avec l’élément **\<httpRedirect>** dans *web.config* :

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

Pour plus d’informations sur la désactivation de modules avec des paramètres de configuration, suivez les liens dans la section *Éléments enfants* de la page [IIS \<system.webServer>](/iis/configuration/system.webServer/).

### <a name="module-removal"></a>Suppression de module

Si vous décidez de supprimer un module avec un paramètre dans *web.config*, déverrouillez le module et déverrouillez la section **\<modules>** de *web.config* en premier :

1. Déverrouillez le module au niveau du serveur. Sélectionnez le serveur IIS dans la barre latérale **Connexions** du Gestionnaire IIS. Ouvrez les **Modules** dans la zone **IIS**. Sélectionnez le module dans la liste. Dans la barre latérale **Actions** à droite, sélectionnez **Déverrouiller**. Déverrouillez tous les modules que vous envisagez de supprimer de *web.config*.

2. Déployez l’application sans section **\<modules>** dans *web.config*. Si une application est déployée avec un fichier *web.config* contenant la section **\<modules>** et que celle-ci n’a pas été préalablement déverrouillée dans le Gestionnaire IIS, Configuration Manager lève une exception au moment du déverrouillage de la section. Vous devez donc déployer l’application sans section **\<modules>**.

3. Déverrouillez la section **\<modules>** de *web.config*. Dans la barre latérale **Connexions**, sélectionnez le site web dans **Sites**. Dans la zone **Gestion**, ouvrez **l’Éditeur de configuration**. Utilisez les contrôles de navigation pour sélectionner la section `system.webServer/modules`. Dans la barre latérale **Actions** à droite, sélectionnez l’option permettant de **Déverrouiller** la section.

4. À ce stade, vous pouvez ajouter une section **\<modules>** au fichier *web.config* avec un élément **\<remove>** pour supprimer le module de l’application. Vous pouvez ajouter plusieurs éléments **\<remove>** pour supprimer plusieurs modules. Si des modifications sont apportées au fichier *web.config* sur le serveur, effectuez immédiatement les mêmes modifications dans le fichier *web.config* du projet localement. La suppression d’un module de cette manière n’affecte pas l’utilisation du module avec d’autres applications sur le serveur.

   ```xml
   <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
   </configuration>
   ```

Vous pouvez également supprimer un module IIS avec *Appcmd.exe*. Fournissez `MODULE_NAME` et `APPLICATION_NAME` dans la commande :

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

Par exemple, supprimez `DynamicCompressionModule` du site web par défaut :

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimum-module-configuration"></a>Configuration minimale des modules

Les seuls modules requis pour exécuter une application ASP.NET Core sont le module d’authentification anonyme et le module ASP.NET Core.

![Gestionnaire IIS ouvert sur la page Modules montrant la configuration minimale des modules](modules/_static/modules.png)

Le module de mise en cache d’URI (`UriCacheModule`) permet à IIS de mettre en cache la configuration du site web au niveau de l’URL. Sans ce module, IIS doit lire et analyser la configuration à chaque requête, même quand la même URL est demandée à plusieurs reprises. L’analyse de la configuration à chaque requête entraîne une baisse significative des performances. *Bien que le module de mise en cache d’URI ne soit pas absolument nécessaire à l’exécution d’une application ASP.NET Core hébergée, nous vous recommandons de l’activer pour tous les déploiements ASP.NET Core.*

Le module de mise en cache HTTP (`HttpCacheModule`) implémente le cache de sortie IIS, ainsi que la logique de mise en cache des éléments dans le cache HTTP.sys. Sans ce module, le contenu n’est plus mis en cache en mode noyau, et les profils de cache sont ignorés. En règle générale, la suppression du module de mise en cache HTTP a des effets négatifs sur les performances et l’utilisation des ressources. *Bien que le module de mise en cache HTTP ne soit pas absolument nécessaire à l’exécution d’une application ASP.NET Core hébergée, nous vous recommandons de l’activer pour tous les déploiements ASP.NET Core.*

## <a name="additional-resources"></a>Ressources supplémentaires

* [Héberger sur Windows avec IIS](xref:host-and-deploy/iis/index)
* [Présentation des architectures IIS : modules dans IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#modules-in-iis)
* [Vue d’ensemble des modules IIS](/iis/get-started/introduction-to-iis/iis-modules-overview)
* [Customizing IIS 7.0 Roles and Modules](https://technet.microsoft.com/library/cc627313.aspx) (Personnalisation des rôles et des modules dans IIS 7.0)
* [IIS `<system.webServer>`](/iis/configuration/system.webServer/)
