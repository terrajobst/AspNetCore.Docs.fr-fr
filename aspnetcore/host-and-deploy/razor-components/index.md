---
title: Héberger et déployer Razor Components
author: guardrex
description: Découvrez comment héberger et déployer des applications Razor Components et Blazor à l’aide d’ASP.NET Core, de réseaux de distribution de contenu (CDN), de serveurs de fichiers et de GitHub Pages.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/22/2019
uid: host-and-deploy/razor-components/index
ms.openlocfilehash: 236e8da27b80dbdb3e0ea885413b6cfd563dde60
ms.sourcegitcommit: 7d6019f762fc5b8cbedcd69801e8310f51a17c18
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419418"
---
# <a name="host-and-deploy-razor-components"></a>Héberger et déployer Razor Components

Par [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) et [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

## <a name="publish-the-app"></a>Publier l'application

Les applications sont publiées pour le déploiement dans la configuration Release avec la commande [dotnet publish](/dotnet/core/tools/dotnet-publish). Un IDE peut gérer l’exécution de la commande `dotnet publish` automatiquement à l’aide de ses fonctionnalités de publication intégrées. Ainsi, en fonction des outils de développement utilisés, il ne sera peut-être pas nécessaire d’exécuter manuellement la commande à partir d’une invite de commandes.

```console
dotnet publish -c Release
```

`dotnet publish` déclenche une [restauration](/dotnet/core/tools/dotnet-restore) des dépendances du projet et [génère](/dotnet/core/tools/dotnet-build) le projet avant de créer les ressources pour le déploiement. Dans le cadre du processus de génération, les assemblys et méthodes inutilisés sont supprimés pour réduire la durée du chargement et la taille du téléchargement de l’application. Le déploiement est créé dans le dossier */bin/Release/&lt;framework_cible&gt;/publish*.

Les ressources dans le dossier *publish* sont déployées sur le serveur web. Le déploiement peut être un processus manuel ou automatisé, en fonction des outils de développement utilisés.

## <a name="host-configuration-values"></a>Valeurs de configuration de l’hôte

Les applications Razor Components qui utilisent le [modèle d’hébergement côté serveur](xref:razor-components/hosting-models#server-side-hosting-model) peuvent accepter des [valeurs de configuration d’hôte web](xref:fundamentals/host/web-host#host-configuration-values).

Les applications Blazor qui utilisent le [modèle d’hébergement côté client](xref:razor-components/hosting-models#client-side-hosting-model) peuvent accepter les valeurs de configuration d’hôte suivantes en tant qu’arguments de ligne de commande au moment de l’exécution dans l’environnement de développement.

### <a name="content-root"></a>Racine de contenu

L’argument `--contentroot` définit le chemin absolu du répertoire qui contient les fichiers de contenu de l’application.

* Passez l’argument lors de l’exécution de l’application localement à une invite de commandes. À partir du répertoire de l’application, exécutez :

  ```console
  dotnet run --contentroot=/<content-root>
  ```

* Ajoutez une entrée au fichier *launchSettings.json* de l’application dans le profil **IIS Express**. Ce paramètre est récupéré lors de l’exécution de l’application avec le débogueur Visual Studio et à partir d’une invite de commandes avec `dotnet run`.

  ```json
  "commandLineArgs": "--contentroot=/<content-root>"
  ```

* Dans Visual Studio, spécifiez l’argument dans **Propriétés** > **Déboguer** > **Arguments d’application**. Le fait de définir l’argument dans la page de propriétés Visual Studio l’ajoute au fichier *launchSettings.json*.

  ```console
  --contentroot=/<content-root>
  ```

### <a name="path-base"></a>Base du chemin

L’argument `--pathbase` définit le chemin de base de l’application pour une application s’exécutant localement avec un chemin virtuel non racine (le `href` de la balise `<base>` a comme valeur un chemin autre que `/` pour la préproduction et la production). Pour plus d’informations, consultez la section [Chemin de base de l’application](#app-base-path).

> [!IMPORTANT]
> Contrairement au chemin fourni au `href` de la balise `<base>`, n’incluez pas de barre oblique (`/`) quand vous passez la valeur d’argument `--pathbase`. Si vous spécifiez `<base href="/CoolApp/" />` (inclut une barre oblique) comme chemin de base de l’application dans la balise `<base>`, passez `--pathbase=/CoolApp` (aucune barre oblique de fin) comme valeur d’argument de ligne de commande.

* Passez l’argument lors de l’exécution de l’application localement à une invite de commandes. À partir du répertoire de l’application, exécutez :

  ```console
  dotnet run --pathbase=/<virtual-path>
  ```

* Ajoutez une entrée au fichier *launchSettings.json* de l’application dans le profil **IIS Express**. Ce paramètre est récupéré lors de l’exécution de l’application avec le débogueur Visual Studio et à partir d’une invite de commandes avec `dotnet run`.

  ```json
  "commandLineArgs": "--pathbase=/<virtual-path>"
  ```

* Dans Visual Studio, spécifiez l’argument dans **Propriétés** > **Déboguer** > **Arguments d’application**. Le fait de définir l’argument dans la page de propriétés Visual Studio l’ajoute au fichier *launchSettings.json*.

  ```console
  --pathbase=/<virtual-path>
  ```

### <a name="urls"></a>URL

L’argument `--urls` indique les adresses IP ou adresses d’hôtes avec les ports et protocoles sur lesquels il faut écouter les requêtes.

* Passez l’argument lors de l’exécution de l’application localement à une invite de commandes. À partir du répertoire de l’application, exécutez :

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* Ajoutez une entrée au fichier *launchSettings.json* de l’application dans le profil **IIS Express**. Ce paramètre est récupéré lors de l’exécution de l’application avec le débogueur Visual Studio et à partir d’une invite de commandes avec `dotnet run`.

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* Dans Visual Studio, spécifiez l’argument dans **Propriétés** > **Déboguer** > **Arguments d’application**. Le fait de définir l’argument dans la page de propriétés Visual Studio l’ajoute au fichier *launchSettings.json*.

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="deploy-a-client-side-blazor-app"></a>Déployer une application Blazor côté client

Avec le [modèle d’hébergement côté client](xref:razor-components/hosting-models#client-side-hosting-model) :

* L’application Blazor, ses dépendances et le runtime .NET sont téléchargés sur le navigateur.
* L’application est exécutée directement sur le thread d’interface utilisateur du navigateur. Les stratégies suivantes sont prises en charge :
  * L’application Blazor est fournie par une application ASP.NET Core. Voir la section [Déploiement Blazor hébergé côté client avec ASP.NET Core](#client-side-blazor-hosted-deployment-with-aspnet-core).
  * L’application Blazor est placée sur un service ou un serveur web d’hébergement statique, où .NET n’est pas utilisé pour servir l’application Blazor. Voir la section [Déploiement Blazor autonome côté client](#client-side-blazor-standalone-deployment).

### <a name="configure-the-linker"></a>Configurer l'éditeur de liens

Blazor effectue la liaison de langage intermédiaire (IL) lors de chaque génération afin de supprimer tout langage intermédiaire inutile des assemblys de sortie. Vous pouvez contrôler la liaison d’assembly lors de la génération. Pour plus d'informations, consultez <xref:host-and-deploy/razor-components/configure-linker>.

### <a name="rewrite-urls-for-correct-routing"></a>Réécriture d’URL pour un routage correct

Le routage des requêtes pour les composants de la page dans une application côté client n’est pas aussi simple que le routage des requêtes vers une application hébergée côté serveur. Considérez une application côté client avec deux pages :

* **_Main.cshtml_** &ndash; Se charge à la racine de l’application et contient un lien vers la page À propos de (`href="About"`).
* **_About.cshtml_** &ndash; Page À propos de.

Quand le document par défaut de l’application est demandé à l’aide de la barre d’adresses du navigateur (par exemple, `https://www.contoso.com/`) :

1. Le navigateur effectue une requête.
1. La page par défaut est retournée, généralement *index.html*.
1. *index.HTML* amorce l’application.
1. Le routeur Blazor se charge et la page principale Razor (*Main.cshtml*) s’affiche.

Dans la page principale, le fait de sélectionner le lien vers la page À propos de charge cette dernière. La sélection du lien vers la page À propos de fonctionne sur le client, car le routeur Blazor empêche le navigateur d’effectuer une requête sur Internet à `www.contoso.com` pour `About` et fournit lui-même la page À propos de. Toutes les requêtes pour des pages internes *au sein de l’application côté client* fonctionnent de manière identique : Les requêtes ne déclenchent pas de requêtes basées sur le navigateur pour des ressources hébergées par le serveur sur Internet. Le routeur gère les requêtes en interne.

Si une requête pour `www.contoso.com/About` est effectuée à l’aide de la barre d’adresses du navigateur, elle échoue. Comme cette ressource n’existe pas sur l’hôte Internet de l’application, une réponse *404 Non trouvé* est retournée.

Étant donné que les navigateurs envoient des requêtes aux hôtes basés sur Internet pour des pages côté client, les serveurs web et les services d’hébergement doivent réécrire toutes les requêtes pour les ressources qui ne se trouvent pas physiquement sur le serveur afin qu’elles pointent vers la page *index.html*. Quand *index.html* est retournée, le routeur côté client de l’application prend le relais et répond avec la ressource appropriée.

### <a name="app-base-path"></a>Chemin de base de l’application

Le chemin de base de l’application est le chemin racine d’application virtuel sur le serveur. Par exemple, une application qui réside sur le serveur Contoso dans un dossier virtuel à `/CoolApp/` est accessible à `https://www.contoso.com/CoolApp` et son chemin de base virtuel est `/CoolApp/`. Le fait d’affecter `CoolApp/` comme chemin de base de l’application lui permet de connaître l’endroit où elle réside virtuellement sur le serveur. L’application peut utiliser le chemin de base de l’application pour construire des URL relatives à la racine d’application à partir d’un composant qui ne se trouve pas dans le répertoire racine. Cela permet aux composants qui existent à différents niveaux de la structure de répertoires de générer des liens vers d’autres ressources à des emplacements dans toute l’application. Le chemin de base de l’application sert également à intercepter les clics sur les liens hypertextes où la cible `href` du lien se trouve dans l’espace d’URI du chemin de base de l’application (le routeur Blazor gère la navigation interne).

Dans de nombreux scénarios d’hébergement, le chemin virtuel du serveur vers l’application est la racine de l’application. Dans ce cas, le chemin de base de l’application est une barre oblique (`<base href="/" />`), ce qui correspond à la configuration par défaut pour une application. Dans d’autres scénarios d’hébergement, tels que GitHub Pages et les sous-applications ou répertoires virtuels IIS, vous devez affecter le chemin virtuel du serveur vers l’application comme chemin de base de l’application. Pour définir le chemin de base de l’application, ajoutez ou mettez à jour la balise `<base>` dans *index.html* qui se trouve entre les éléments de balise `<head>`. Affectez la valeur `<virtual-path>/` à l’attribut `href` (la barre oblique de fin est obligatoire), où `<virtual-path>/` est le chemin racine d’application virtuel complet sur le serveur pour l’application. Dans l’exemple précédent, le chemin virtuel est défini sur `CoolApp/` : `<base href="CoolApp/" />`.

Pour une application avec un chemin virtuel non racine configuré (par exemple `<base href="CoolApp/" />`), l’application ne parvient pas à trouver ses ressources *quand elle est exécutée localement*. Pour surmonter ce problème pendant le développement local et les tests, vous pouvez fournir un argument de *base de chemin* qui correspond à la valeur `href` de la balise `<base>` au moment de l’exécution.

Pour passer l’argument de base de chemin avec le chemin racine (`/`) quand vous exécutez l’application localement, exécutez la commande suivante à partir du répertoire de l’application :

```console
dotnet run --pathbase=/CoolApp
```

L’application répond localement à `http://localhost:port/CoolApp`.

Pour plus d’informations, consultez la section [Valeur de configuration d’hôte Base du chemin](#path-base).

> [!IMPORTANT]
> Si une application utilise le [modèle d’hébergement côté client](xref:razor-components/hosting-models#client-side-hosting-model) (basé sur le modèle de projet **Blazor**) et est hébergée en tant que sous-application IIS dans une application ASP.NET Core, il convient de désactiver le gestionnaire de module ASP.NET Core hérité ou de s’assurer que la section `<handlers>` de l’application racine (parente) dans le fichier *web.config* n’est pas héritée par la sous-application.
>
> Supprimez le gestionnaire dans le fichier *web.config* publié de l’application en ajoutant une section `<handlers>` au fichier :
>
> ```xml
> <handlers>
>   <remove name="aspNetCore" />
> </handlers>
> ```
>
> En guise d’alternative, vous pouvez désactiver l’héritage de la section `<system.webServer>` de l’application racine (parente) en affectant la valeur `false` à l’attribut `inheritInChildApplications` d’un élément `<location>` :
>
> ```xml
> <?xml version="1.0" encoding="utf-8"?>
> <configuration>
>  <location path="." inheritInChildApplications="false">
>    <system.webServer>
>      <handlers>
>        <add name="aspNetCore" ... />
>      </handlers>
>      <aspNetCore ... />
>    </system.webServer>
>  </location>
> </configuration>
> ```
>
> La suppression du gestionnaire ou la désactivation de l’héritage est effectuée en plus de la configuration du chemin de base de l’application comme décrit dans cette section. Dans le fichier *index.html* de l’application, définissez le chemin de base de l’application sur l’alias IIS utilisé lors de la configuration de la sous-application dans IIS.

### <a name="client-side-blazor-hosted-deployment-with-aspnet-core"></a>Déploiement Blazor hébergé côté client avec ASP.NET Core

Un *déploiement hébergé* fournit l’application Blazor côté client aux navigateurs à partir d’une [application ASP.NET Core](xref:index) qui s’exécute sur un serveur.

L’application Blazor est incluse dans l’application ASP.NET Core dans la sortie publiée, afin que les deux applications soient déployées ensemble. Un serveur web capable d’héberger une application ASP.NET Core est nécessaire. Pour un déploiement hébergé, Visual Studio inclut le modèle de projet **Blazor (ASP.NET Core hébergé)** (modèle `blazorhosted` quand vous utilisez la commande [dotnet new](/dotnet/core/tools/dotnet-new)).

Pour plus d’informations sur l’hébergement et le déploiement d’applications ASP.NET Core, consultez <xref:host-and-deploy/index>.

Pour plus d’informations sur le déploiement sur Azure App Service, consultez les rubriques suivantes :

<xref:tutorials/publish-to-azure-webapp-using-vs>

Découvrez comment publier une application ASP.NET Core sur Azure App Service à l’aide de Visual Studio.

### <a name="client-side-blazor-standalone-deployment"></a>Déploiement Blazor autonome côté client

Un *déploiement autonome* fournit l’application Blazor côté client sous la forme d’un ensemble de fichiers statiques qui sont demandés directement par les clients. Aucun serveur web n’est utilisé pour fournir l’application Blazor.

#### <a name="client-side-blazor-standalone-hosting-with-iis"></a>Hébergement Blazor autonome côté client avec IIS

IIS est un serveur de fichiers statiques compatible avec les applications Blazor. Pour configurer IIS afin d’héberger Blazor, consultez [Générer un site web statique sur IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).

Les ressources publiées sont créées dans le dossier *\\bin\\version\\&lt;framework_cible&gt;\\publish*. Hébergez le contenu du dossier *publish* sur le serveur web ou le service d’hébergement.

**web.config**

Quand un projet Blazor est publié, un fichier *web.config* est créé avec la configuration IIS suivante :

* Les types MIME sont définis pour les extensions de fichiers suivantes :
  * *\*.dll* : `application/octet-stream`
  * *\*.json* : `application/json`
  * *\*.wasm* : `application/wasm`
  * *\*.woff* : `application/font-woff`
  * *\*.woff2* : `application/font-woff`
* La compression HTTP est activée pour les types MIME suivants :
  * `application/octet-stream`
  * `application/wasm`
* Des règles du module de réécriture d’URL sont établies :
  * Servir le sous-répertoire où résident les ressources statiques de l’application (*&lt;nom_assembly&gt;\\dist\\&lt;chemin_demandé&gt;*).
  * Créer un routage de secours SPA afin que les requêtes pour des ressources autres que des fichiers soient redirigées vers le document par défaut de l’application dans son dossier de ressources statiques (*&lt;nom_assembly&gt;\\dist\\index.html*).

**Installer le module de réécriture d’URL**

Le [module de réécriture d’URL](https://www.iis.net/downloads/microsoft/url-rewrite) est nécessaire pour réécrire les URL. Il n’est pas installé par défaut, et n’est pas disponible pour l’installation en tant que fonctionnalité de service de rôle Serveur Web (IIS). Vous devez le télécharger à partir du site web IIS. Utilisez Web Platform Installer pour installer le module :

1. Localement, accédez à la [page des téléchargements du Module de réécriture d’URL](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads). Pour la version anglaise, sélectionnez **WebPI** pour télécharger le programme d’installation WebPI. Pour les autres langues, sélectionnez l’architecture appropriée pour le serveur (x86/x64) afin de télécharger le programme d’installation.
1. Copiez le programme d’installation sur le serveur. Exécutez le programme d’installation. Sélectionnez le bouton **Installer** et acceptez les termes du contrat de licence. Il n’est pas nécessaire de redémarrer le serveur après l’installation.

**Configurer le site web**

Affectez le dossier de l’application comme **chemin physique** du site web. Le dossier contient :

* Le fichier *web.config* utilisé par IIS pour configurer le site web, notamment les règles de redirection nécessaires et les types de contenu de fichiers
* Le dossier de ressources statiques de l’application

**Résolution des problèmes**

Si un message *500 Erreur interne du serveur* est reçu et que le Gestionnaire IIS lève des erreurs quand vous tentez d’accéder à la configuration du site web, vérifiez que le module de réécriture d’URL est installé. Quand le module n’est pas installé, le fichier *web.config* ne peut pas être analysé par IIS. Cela empêche le Gestionnaire IIS de charger la configuration du site web et empêche le site web de fournir les fichiers statiques Blazor.

Pour plus d’informations sur le dépannage des déploiements sur IIS, consultez <xref:host-and-deploy/iis/troubleshoot>.

#### <a name="client-side-blazor-standalone-hosting-with-nginx"></a>Hébergement Blazor autonome côté client avec Nginx

Le fichier *nginx.conf* suivant est simplifié afin d’illustrer comment configurer Nginx pour envoyer le fichier *Index.html* chaque fois qu’il ne trouve aucun fichier correspondant sur le disque.

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /Index.html =404;
        }
    }
}
```

Pour plus d’informations sur la configuration du serveur web Nginx de production, consultez [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/) (Création de fichiers de configuration NGINX et NGINX Plus).

#### <a name="client-side-blazor-standalone-hosting-with-nginx-in-docker"></a>Hébergement Blazor autonome côté client avec Nginx dans Docker

Pour héberger Blazor dans Docker à l’aide de Nginx, configurez le fichier Dockerfile pour utiliser l’image Nginx basée sur Alpine. Mettez à jour le fichier Dockerfile pour copier le fichier *nginx.config* dans le conteneur.

Ajoutez une ligne au fichier Dockerfile, comme indiqué dans l’exemple suivant :

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

#### <a name="client-side-blazor-standalone-hosting-with-github-pages"></a>Hébergement Blazor autonome côté client avec GitHub Pages

Pour gérer les réécritures d’URL, ajoutez un fichier *404.html* avec un script qui gère la redirection de la requête vers la page *index.html*. Pour obtenir un exemple d’implémentation fournie par la communauté, consultez [Single Page Apps for GitHub Pages](http://spa-github-pages.rafrex.com/) (Applications à une seule page pour GitHub Pages) ([rafrex/spa-github-pages sur GitHub](https://github.com/rafrex/spa-github-pages#readme)). Vous pouvez obtenir un exemple d’utilisation de cette approche à l’adresse [blazor-demo/blazor-demo.github.io sur GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([site actif](https://blazor-demo.github.io/)).

Quand vous utilisez un site de projet plutôt qu’un site d’entreprise, ajoutez ou mettez à jour la balise `<base>` dans *index.html*. Affectez la valeur `<repository-name>/` à l’attribut `href`, où `<repository-name>/` est le nom du dépôt GitHub.

## <a name="deploy-a-server-side-razor-components-app"></a>Déployer une application Razor Components côté serveur

Avec le [modèle d’hébergement côté serveur](xref:razor-components/hosting-models#server-side-hosting-model), Razor Components est exécuté sur le serveur à partir d’une application ASP.NET Core. Les mises à jour de l’interface utilisateur, la gestion des événements et les appels JavaScript sont gérés par le biais d’une connexion SignalR.

L’application est incluse dans l’application ASP.NET Core dans la sortie publiée, afin que les deux applications soient déployées ensemble. Un serveur web capable d’héberger une application ASP.NET Core est nécessaire. Pour un déploiement côté serveur, Visual Studio inclut le modèle de projet **Razor Components** (modèle `razorcomponents` quand vous utilisez la commande [dotnet new](/dotnet/core/tools/dotnet-new)).

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

Quand l’application ASP.NET Core est publiée, l’application Razor Components est incluse dans la sortie publiée afin que l’application ASP.NET Core et l’application Razor Components puissent être déployées ensemble. Pour plus d’informations sur l’hébergement et le déploiement d’applications ASP.NET Core, consultez <xref:host-and-deploy/index>.

Pour plus d’informations sur le déploiement sur Azure App Service, consultez les rubriques suivantes :

<xref:tutorials/publish-to-azure-webapp-using-vs>

Découvrez comment publier une application ASP.NET Core sur Azure App Service à l’aide de Visual Studio.
