---
title: Modules IIS avec ASP.NET Core
author: guardrex
description: Découvrir actives et inactives des modules IIS pour les applications ASP.NET Core et comment gérer les modules IIS.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/modules
ms.openlocfilehash: e88526d997618658f58488adb37ae1e519ea3f59
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2018
---
# <a name="iis-modules-with-aspnet-core"></a>Modules IIS avec ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

Dans une configuration de proxy inverse, les applications ASP.NET Core sont hébergées par IIS. Certains des modules IIS natifs et tous les modules IIS gérés ne sont pas disponibles pour traiter les demandes pour les applications ASP.NET Core. Dans de nombreux cas, ASP.NET Core offre une alternative aux fonctionnalités des modules natifs et managés d’IIS.

## <a name="native-modules"></a>Modules natifs

Le tableau indique les modules IIS natifs qui fonctionnent sur les demandes de proxy inverse pour les applications ASP.NET Core.

| Module | Fonctionnel avec les applications ASP.NET Core | Option ASP.NET Core |
| ------ | :-------------------------------: | ------------------- |
| **Authentification anonyme**<br>`AnonymousAuthenticationModule` | Oui | |
| **Authentification de base**<br>`BasicAuthenticationModule` | Oui | |
| **Authentification par mappage de Certification de client**<br>`CertificateMappingAuthenticationModule` | Oui | |
| **CGI**<br>`CgiModule` | Non | |
| **Validation de la configuration**<br>`ConfigurationValidationModule` | Oui | |
| **Erreurs HTTP**<br>`CustomErrorModule` | Non | [Intergiciel (middleware) Pages de Code état](xref:fundamentals/error-handling#configuring-status-code-pages) |
| **Journalisation personnalisée**<br>`CustomLoggingModule` | Oui | |
| **Document par défaut**<br>`DefaultDocumentModule` | Non | [Intergiciel de fichiers par défaut](xref:fundamentals/static-files#serve-a-default-document) |
| **Authentification Digest**<br>`DigestAuthenticationModule` | Oui | |
| **Exploration des répertoires**<br>`DirectoryListingModule` | Non | [Intergiciel (middleware) exploration de répertoire](xref:fundamentals/static-files#enable-directory-browsing) |
| **Compression dynamique**<br>`DynamicCompressionModule` | Oui | [Intergiciel (middleware) de compression des réponses](xref:performance/response-compression) |
| **Suivi**<br>`FailedRequestsTracingModule` | Oui | [Enregistrement d’ASP.NET Core](xref:fundamentals/logging/index#the-tracesource-provider) |
| **La mise en cache du fichier**<br>`FileCacheModule` | Non | [Intergiciel de mise en cache des réponses](xref:performance/caching/middleware) |
| **Mise en cache HTTP**<br>`HttpCacheModule` | Non | [Intergiciel de mise en cache des réponses](xref:performance/caching/middleware) |
| **Journalisation HTTP**<br>`HttpLoggingModule` | Oui | [Enregistrement d’ASP.NET Core](xref:fundamentals/logging/index)<br>Implémentations : [elmah.io](https://github.com/elmahio/Elmah.Io.Extensions.Logging), [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging), [NLog](https://github.com/NLog/NLog.Extensions.Logging), [Serilog](https://github.com/serilog/serilog-extensions-logging)
| **Redirection HTTP**<br>`HttpRedirectionModule` | Oui | [Intergiciel de réécriture d’URL](xref:fundamentals/url-rewriting) |
| **Authentification par mappage de certificat Client IIS**<br>`IISCertificateMappingAuthenticationModule` | Oui | |
| **Restrictions IP et de domaine**<br>`IpRestrictionModule` | Oui | |
| **Filtres ISAPI**<br>`IsapiFilterModule` | Oui | [Intergiciel (middleware)](xref:fundamentals/middleware/index) |
| **ISAPI**<br>`IsapiModule` | Oui | [Intergiciel (middleware)](xref:fundamentals/middleware/index) |
| **Prise en charge du protocole**<br>`ProtocolSupportModule` | Oui | |
| **Filtrage des demandes**<br>`RequestFilteringModule` | Oui | [Intergiciel (middleware) réécriture d’URL `IRule`](xref:fundamentals/url-rewriting#irule-based-rule) |
| **Observateur de demandes**<br>`RequestMonitorModule` | Oui | |
| **Réécriture d’URL**<br>`RewriteModule` | Oui&#8224; | [Intergiciel de réécriture d’URL](xref:fundamentals/url-rewriting) |
| **Inclusions côté serveur**<br>`ServerSideIncludeModule` | Non | |
| **Compression statique**<br>`StaticCompressionModule` | Non | [Intergiciel (middleware) de compression des réponses](xref:performance/response-compression) |
| **Contenu statique**<br>`StaticFileModule` | Non | [Intergiciel (middleware) de fichiers statiques](xref:fundamentals/static-files) |
| **Mise en cache de jeton**<br>`TokenCacheModule` | Oui | |
| **La mise en cache URI**<br>`UriCacheModule` | Oui | |
| **Autorisation URL**<br>`UrlAuthorizationModule` | Oui | [Identité de ASP.NET Core](xref:security/authentication/identity) |
| **Authentification Windows**<br>`WindowsAuthenticationModule` | Oui | |

&#8224;De l’URL du Module de réécriture `isFile` et `isDirectory` correspond aux types ne fonctionne pas avec les applications ASP.NET Core en raison de modifications dans [structure de répertoires](xref:host-and-deploy/directory-structure).

## <a name="managed-modules"></a>Modules managés

Les modules managés sont *pas* fonctionnel avec les applications ASP.NET Core hébergées lors de la version du CLR .NET de pool d’applications est définie sur **aucun Code managé**. ASP.NET Core offrent des alternatives de l’intergiciel (middleware) dans plusieurs cas.

| Module                  | Option ASP.NET Core |
| ----------------------- | ------------------- |
| AnonymousIdentification | |
| DefaultAuthentication   | |
| FileAuthorization       | |
| FormsAuthentication     | [Intergiciel (middleware) de cookie d’authentification](xref:security/authentication/cookie) |
| OutputCache             | [Intergiciel de mise en cache des réponses](xref:performance/caching/middleware) |
| Profil                 | |
| RoleManager             | |
| ScriptModule-4.0        | |
| Session                 | [Intergiciel (middleware) de session](xref:fundamentals/app-state) |
| UrlAuthorization        | |
| UrlMappingsModule       | [Intergiciel de réécriture d’URL](xref:fundamentals/url-rewriting) |
| UrlRoutingModule-4.0    | [Identité de ASP.NET Core](xref:security/authentication/identity) |
| WindowsAuthentication   | |

## <a name="iis-manager-application-changes"></a>Modification de l’application Gestionnaire des services Internet

Lorsque vous utilisez le Gestionnaire des services Internet pour configurer les paramètres, le *web.config* fichier de l’application est modifié. Si le déploiement d’une application et en incluant *web.config*, toutes les modifications apportées avec le Gestionnaire des services Internet sont remplacées par la version déployée *web.config* fichier. Si des modifications sont apportées au serveur *web.config* de fichiers, copiez la mise à jour *web.config* fichier sur le serveur pour le projet local immédiatement.

## <a name="disabling-iis-modules"></a>La désactivation des modules IIS

Si un module IIS est configuré au niveau du serveur qui doit être désactivé pour une application, un complément de l’application *web.config* fichier peut désactiver le module. Conservez le module en place et désactiver à l’aide d’un paramètre de configuration (si disponible) ou supprimez le module de l’application.

### <a name="module-deactivation"></a>Désactivation du module

Nombre de modules offre un paramètre de configuration qui leur permet d’être désactivé sans supprimer le module de l’application. Il s’agit de la façon la plus simple et plus rapide pour désactiver un module. Par exemple, le Module de la Redirection HTTP peut être désactivé avec le  **\<httpRedirect >** élément *web.config*:

```xml
<configuration>
  <system.webServer>
     <httpRedirect enabled="false" />
  </system.webServer>
</configuration>
```

Pour plus d’informations sur la désactivation de modules avec les paramètres de configuration, suivez les liens dans le *des éléments enfants* section de [IIS \<system.webServer >](/iis/configuration/system.webServer/).

### <a name="module-removal"></a>Retrait du module

Si vous être inscrit pour supprimer un module avec un paramètre dans *web.config*, déverrouillage du module et le déverrouillage du  **\<modules >** section de *web.config* premier :

1. Déverrouiller le module au niveau du serveur. Sélectionnez le serveur IIS dans le Gestionnaire IIS **connexions** barre latérale. Ouvrez le **Modules** dans les **IIS** zone. Sélectionnez le module dans la liste. Dans le **Actions** encadré à droite, sélectionnez **Unlock**. Déverrouiller autant de modules que vous envisagez de retirer *web.config* plus tard.

2. Déployer l’application sans un  **\<modules >** section *web.config*. Si une application est déployée avec une *web.config* contenant le  **\<modules >** section sans avoir déverrouillé la section tout d’abord dans le Gestionnaire des services Internet, le Gestionnaire de Configuration lève une exception lors de la tentative de déverrouillage de la section. Par conséquent, déployez l’application sans un  **\<modules >** section.

3. Déverrouiller le  **\<modules >** section de *web.config*. Dans le **connexions** encadré, sélectionnez le site Web dans **Sites**. Dans le **gestion** zone, ouvrez le **l’éditeur de Configuration**. Utilisez les contrôles de navigation pour sélectionner le `system.webServer/modules` section. Dans le **Actions** encadré à droite, sélectionnez cette option pour **Unlock** la section.

4. À ce stade, un  **\<modules >** section peut être ajoutée à la *web.config* de fichiers avec un  **\<Supprimer >** élément à supprimer le module à partir de l’application. Plusieurs  **\<Supprimer >** éléments peuvent être ajoutés pour supprimer plusieurs modules. Si *web.config* modifications sont effectuées sur le serveur, immédiatement apporter les mêmes modifications dans le fichier *web.config* fichier localement. Retrait d’un module de cette manière n’affecte pas l’utilisation du module avec d’autres applications sur le serveur.

   ```xml
   <configuration> 
    <system.webServer> 
      <modules> 
        <remove name="MODULE_NAME" /> 
      </modules> 
    </system.webServer> 
   </configuration>
   ```

Un module IIS peut également être supprimé avec *Appcmd.exe*. Fournir le `MODULE_NAME` et `APPLICATION_NAME` dans la commande :

```console
Appcmd.exe delete module MODULE_NAME /app.name:APPLICATION_NAME
```

Par exemple, supprimez le `DynamicCompressionModule` à partir du Site Web par défaut :

```console
%windir%\system32\inetsrv\appcmd.exe delete module DynamicCompressionModule /app.name:"Default Web Site"
```

## <a name="minimum-module-configuration"></a>Configuration du module minimale

Les modules uniquement requis pour exécuter une application ASP.NET Core sont le Module d’authentification anonyme et le Module de base ASP.NET.

![Ouvrir le Gestionnaire des services Internet aux Modules avec la configuration de modules minimale indiquée](modules/_static/modules.png)

Le Module de mise en cache URI (`UriCacheModule`) permet à IIS à la configuration de site Web de cache au niveau de l’URL. Sans ce module, IIS doit lire et analyser la configuration sur chaque demande, même lorsque la même URL est demandée à plusieurs reprises. L’analyse de la configuration de chaque demande entraîne une baisse significative des performances. *Bien que le Module de mise en cache URI n’est pas strictement obligatoire pour une application ASP.NET Core hébergée s’exécute, nous recommandons que le Module de mise en cache URI soit activée pour tous les déploiements ASP.NET Core.*

Le Module de mise en cache HTTP (`HttpCacheModule`) implémente le cache de sortie IIS et également la logique de mise en cache des éléments dans le cache de HTTP.sys. Sans ce module, le contenu n’est plus mise en cache du mode noyau, et les profils de cache sont ignorés. La suppression du Module de mise en cache HTTP généralement a des effets négatifs sur les performances et l’utilisation des ressources. *Bien que le Module de mise en cache HTTP n’est pas strictement obligatoire pour une application ASP.NET Core hébergée s’exécute, nous recommandons que le Module de mise en cache HTTP soit activée pour tous les déploiements ASP.NET Core.*

## <a name="additional-resources"></a>Ressources supplémentaires

* [Héberger sur Windows avec IIS](xref:host-and-deploy/iis/index)
* [Présentation des Architectures IIS à : Modules dans IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#modules-in-iis)
* [Vue d’ensemble des Modules IIS](/iis/get-started/introduction-to-iis/iis-modules-overview)
* [Personnalisation de IIS 7.0 rôles et des Modules](https://technet.microsoft.com/library/cc627313.aspx)
* [IIS `<system.webServer>`](/iis/configuration/system.webServer/)
