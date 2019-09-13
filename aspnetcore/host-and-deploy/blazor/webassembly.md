---
title: Héberger et déployer ASP.NET Core éblouissant webassembly
author: guardrex
description: Découvrez comment héberger et déployer une application Blazor avec ASP.NET Core, des réseaux de distribution de contenu (CDN), des serveurs de fichiers et GitHub Pages.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: host-and-deploy/blazor/webassembly
ms.openlocfilehash: 06316cacb02d9d7619ff7a210bd596696f86021b
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70964253"
---
# <a name="host-and-deploy-aspnet-core-blazor-webassembly"></a>Héberger et déployer ASP.NET Core éblouissant webassembly

Par [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) et [Daniel Roth](https://github.com/danroth27)

Avec le [modèle d’hébergement de Webassembly éblouissant](xref:blazor/hosting-models#blazor-webassembly):

* L’application Blazor, ses dépendances et le runtime .NET sont téléchargés sur le navigateur.
* L’application est exécutée directement sur le thread d’interface utilisateur du navigateur.

Les stratégies de déploiement suivantes sont prises en charge :

* L’application Blazor est fournie par une application ASP.NET Core. Cette stratégie est abordée dans la section [Déploiement hébergé avec ASP.NET Core](#hosted-deployment-with-aspnet-core).
* L’application Blazor est placée sur un service ou un serveur web d’hébergement statique, où .NET n’est pas utilisé pour servir l’application Blazor. Cette stratégie est traitée dans la section [Déploiement autonome](#standalone-deployment) , qui comprend des informations sur l’hébergement d’une application de webassembly éblouissante en tant que sous-application IIS.

## <a name="rewrite-urls-for-correct-routing"></a>Réécriture d’URL pour un routage correct

Le routage des requêtes pour les composants de page dans une application de webassembly éblouissante n’est pas aussi simple que le routage des demandes dans un serveur éblouissant, une application hébergée. Prenons l’exemple d’une application de webassembly éblouissant avec deux composants :

* *Main.razor* &ndash; Se charge à la racine de l’application et contient un lien vers le composant `About` (`href="About"`).
* Composant *About.Razor* &ndash; `About`.

Quand le document par défaut de l’application est demandé à l’aide de la barre d’adresses du navigateur (par exemple, `https://www.contoso.com/`) :

1. Le navigateur effectue une requête.
1. La page par défaut est retournée, généralement *index.html*.
1. *index.HTML* amorce l’application.
1. Le routeur de Blazor se charge et le composant `Main` Razor est rendu.

Dans la page principale, la sélection du composant `About` fonctionne sur le client, car le routeur Blazor empêche le navigateur d’effectuer une requête sur Internet à `www.contoso.com` pour `About` et fournit lui-même le composant `About` rendu. Toutes les requêtes pour les points de terminaison internes de *l’application de Webassembly éblouissant* fonctionnent de la même façon : Les requêtes ne déclenchent pas de requêtes basées sur le navigateur pour des ressources hébergées par le serveur sur Internet. Le routeur gère les requêtes en interne.

Si une requête pour `www.contoso.com/About` est effectuée à l’aide de la barre d’adresses du navigateur, elle échoue. Comme cette ressource n’existe pas sur l’hôte Internet de l’application, une réponse *404 – Non trouvé* est retournée.

Étant donné que les navigateurs envoient des requêtes aux hôtes basés sur Internet pour des pages côté client, les serveurs web et les services d’hébergement doivent réécrire toutes les requêtes pour les ressources qui ne se trouvent pas physiquement sur le serveur afin qu’elles pointent vers la page *index.html*. Quand *index. html* est retourné, le routeur éblouissant de l’application prend le relais et répond avec la ressource correcte.

Lors du déploiement sur un serveur IIS, vous pouvez utiliser le module de réécriture d’URL avec le fichier *Web. config* publié de l’application. Pour plus d’informations, consultez la section [IIS](#iis) .

## <a name="hosted-deployment-with-aspnet-core"></a>Déploiement hébergé avec ASP.NET Core

Un *déploiement hébergé* sert l’application de webassembly éblouissant à des navigateurs à partir d’une [application ASP.net Core](xref:index) qui s’exécute sur un serveur Web.

L’application Blazor est incluse dans l’application ASP.NET Core dans la sortie publiée, afin que les deux applications soient déployées ensemble. Un serveur web capable d’héberger une application ASP.NET Core est nécessaire. Pour un déploiement hébergé, Visual Studio inclut le modèle de projet **Application Blazor WebAssembly** (modèle `blazorwasm` quand vous utilisez la commande [dotnet new](/dotnet/core/tools/dotnet-new)) avec l’option **Hébergé** sélectionnée.

Pour plus d’informations sur l’hébergement et le déploiement d’applications ASP.NET Core, consultez <xref:host-and-deploy/index>.

Pour plus d’informations concernant le déploiement sur Azure App Service, consultez <xref:tutorials/publish-to-azure-webapp-using-vs>.

## <a name="standalone-deployment"></a>Déploiement autonome

Un *Déploiement autonome* sert l’application de webassembly éblouissant sous la forme d’un ensemble de fichiers statiques demandés directement par les clients. N’importe quel serveur de fichiers statiques est capable de servir l’application Blazor.

Les ressources d’un déploiement autonome sont publiées dans le dossier *bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist*.

### <a name="iis"></a>IIS

IIS est un serveur de fichiers statiques compatible avec les applications Blazor. Pour configurer IIS afin d’héberger Blazor, consultez [Générer un site web statique sur IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).

Les ressources publiées sont créées dans le dossier */bin/Release/{FRAMEWORK CIBLE}/publish*. Hébergez le contenu du dossier *publish* sur le serveur web ou le service d’hébergement.

#### <a name="webconfig"></a>web.config

Quand un projet Blazor est publié, un fichier *web.config* est créé avec la configuration IIS suivante :

* Les types MIME sont définis pour les extensions de fichiers suivantes :
  * *.dll* &ndash; `application/octet-stream`
  * *.json* &ndash; `application/json`
  * *.wasm* &ndash; `application/wasm`
  * *.woff* &ndash; `application/font-woff`
  * *.woff2* &ndash; `application/font-woff`
* La compression HTTP est activée pour les types MIME suivants :
  * `application/octet-stream`
  * `application/wasm`
* Des règles du module de réécriture d’URL sont établies :
  * Servir le sous-répertoire où résident les ressources statiques de l’application ( *{NOM ASSEMBLY}/dist/{CHEMIN DEMANDÉ}* ).
  * Créer un routage de secours SPA afin que les demandes de ressources autres que des fichiers soient redirigées vers le document par défaut de l’application dans son dossier de ressources statiques ( *{NOM ASSEMBLY}/dist/index.html*).

#### <a name="install-the-url-rewrite-module"></a>Installer le module de réécriture d’URL

Le [module de réécriture d’URL](https://www.iis.net/downloads/microsoft/url-rewrite) est nécessaire pour réécrire les URL. Il n’est pas installé par défaut, et ne peut pas l’être en tant que fonctionnalité de service de rôle Serveur Web (IIS). Vous devez le télécharger à partir du site web IIS. Utilisez Web Platform Installer pour installer le module :

1. Localement, accédez à la [page des téléchargements du Module de réécriture d’URL](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads). Pour la version anglaise, sélectionnez **WebPI** pour télécharger le programme d’installation WebPI. Pour les autres langues, sélectionnez l’architecture appropriée pour le serveur (x86/x64) afin de télécharger le programme d’installation.
1. Copiez le programme d’installation sur le serveur. Exécutez le programme d’installation. Sélectionnez le bouton **Installer** et acceptez les termes du contrat de licence. Il n’est pas nécessaire de redémarrer le serveur après l’installation.

#### <a name="configure-the-website"></a>Configurer le site web

Affectez le dossier de l’application comme **chemin physique** du site web. Le dossier contient :

* Le fichier *web.config* utilisé par IIS pour configurer le site web, notamment les règles de redirection nécessaires et les types de contenu de fichiers
* Le dossier de ressources statiques de l’application

#### <a name="host-as-an-iis-sub-app"></a>Héberger en tant que sous-application IIS

Si une application autonome est hébergée en tant que sous-application IIS, effectuez l’une des opérations suivantes :

* Désactivez le gestionnaire de module ASP.NET Core hérité.

  Supprimez le gestionnaire dans le fichier *Web. config* publié de l’application éblouissant en ajoutant `<handlers>` une section au fichier :

  ```xml
  <handlers>
    <remove name="aspNetCore" />
  </handlers>
  ```

* Désactivez l’héritage de la section de l' `<system.webServer>` application racine (parent) `inheritInChildApplications` à l' `false`aide d’un `<location>` élément dont la valeur est :

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <configuration>
    <location path="." inheritInChildApplications="false">
      <system.webServer>
        <handlers>
          <add name="aspNetCore" ... />
        </handlers>
        <aspNetCore ... />
      </system.webServer>
    </location>
  </configuration>
  ```

La suppression du gestionnaire ou la désactivation de l’héritage est effectuée en plus de [la configuration du chemin d’accès de base de l’application](xref:host-and-deploy/blazor/index#app-base-path). Dans le fichier *index.html* de l’application, définissez le chemin de base de l’application sur l’alias IIS utilisé lors de la configuration de la sous-application dans IIS.

#### <a name="troubleshooting"></a>Résolution des problèmes

Si vous recevez un message *500 – Erreur interne du serveur* et que le Gestionnaire IIS lève des erreurs quand vous tentez d’accéder à la configuration du site web, vérifiez que le module de réécriture d’URL est installé. Quand le module n’est pas installé, le fichier *web.config* ne peut pas être analysé par IIS. Cela empêche le Gestionnaire IIS de charger la configuration du site web et empêche le site web de fournir les fichiers statiques Blazor.

Pour plus d’informations sur le dépannage des déploiements sur IIS, consultez <xref:test/troubleshoot-azure-iis>.

### <a name="azure-storage"></a>Stockage Azure

L’hébergement de fichiers statiques [Azure Storage](/azure/storage/) permet l’hébergement d’applications éblouissantes sans serveur. Les noms de domaine personnalisé, le réseau de distribution de contenu Azure (CDN) et HTTPS sont pris en charge.

Lorsque le service blob est activé pour l’hébergement de site Web statique sur un compte de stockage :

* Définissez le **nom du document d’index** sur `index.html`.
* Définissez le **chemin d’accès au document d’erreur** sur `index.html`. Les composants de Razor et autres points de terminaison non-fichier ne se trouvent sur les chemins d’accès physiques dans le contenu statique stocké par le service blob. Lors de la réception d’une requête pour l’une de ces ressources que le routeur Blazor doit gérer, l’erreur *404 - introuvable* générée par le service blob achemine la requête vers le **chemin d’accès au document d’erreur**. Le blob *index.html* est retourné et le routeur Blazor charge et traite le chemin d’accès.

Pour plus d’informations, consultez [Hébergement de site Web statique dans le stockage Azure](/azure/storage/blobs/storage-blob-static-website).

### <a name="nginx"></a>Nginx

Le fichier *nginx.conf* suivant est simplifié afin de montrer comment configurer Nginx pour envoyer le fichier *index.html* chaque fois qu’il ne trouve aucun fichier correspondant sur le disque.

```
events { }
http {
    server {
        listen 80;

        location / {
            root /usr/share/nginx/html;
            try_files $uri $uri/ /index.html =404;
        }
    }
}
```

Pour plus d’informations sur la configuration du serveur web Nginx de production, consultez [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/) (Création de fichiers de configuration NGINX et NGINX Plus).

### <a name="nginx-in-docker"></a>Nginx dans Docker

Pour héberger Blazor dans Docker à l’aide de Nginx, configurez le fichier Dockerfile pour utiliser l’image Nginx basée sur Alpine. Mettez à jour le fichier Dockerfile pour copier le fichier *nginx.config* dans le conteneur.

Ajoutez une ligne au fichier Dockerfile, comme indiqué dans l’exemple suivant :

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="github-pages"></a>GitHub Pages

Pour gérer les réécritures d’URL, ajoutez un fichier *404.html* avec un script qui gère la redirection de la requête vers la page *index.html*. Pour obtenir un exemple d’implémentation fournie par la communauté, consultez [Single Page Apps for GitHub Pages](https://spa-github-pages.rafrex.com/) (Applications à une seule page pour GitHub Pages) ([rafrex/spa-github-pages sur GitHub](https://github.com/rafrex/spa-github-pages#readme)). Vous pouvez obtenir un exemple d’utilisation de cette approche à l’adresse [blazor-demo/blazor-demo.github.io sur GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([site actif](https://blazor-demo.github.io/)).

Quand vous utilisez un site de projet plutôt qu’un site d’entreprise, ajoutez ou mettez à jour la balise `<base>` dans *index.html*. Définissez la valeur d’attribut `href` sur le nom du dépôt GitHub avec une barre oblique (par exemple, `my-repository/`.

## <a name="host-configuration-values"></a>Valeurs de configuration de l’hôte

Les [applications Webassembly éblouissantes](xref:blazor/hosting-models#blazor-webassembly) peuvent accepter les valeurs de configuration d’hôte suivantes comme arguments de ligne de commande au moment de l’exécution dans l’environnement de développement.

### <a name="content-root"></a>Racine de contenu

L’argument `--contentroot` définit le chemin absolu du répertoire qui contient les fichiers de contenu de l’application. Dans les exemples suivants, `/content-root-path` est le chemin racine du contenu de l’application.

* Passez l’argument lors de l’exécution de l’application localement à une invite de commandes. À partir du répertoire de l’application, exécutez :

  ```console
  dotnet run --contentroot=/content-root-path
  ```

* Ajoutez une entrée au fichier *launchSettings.json* de l’application dans le profil **IIS Express**. Ce paramètre est utilisé en cas d’exécution de l’application avec le débogueur Visual Studio et dans une invite de commandes avec `dotnet run`.

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* Dans Visual Studio, spécifiez l’argument dans **Propriétés** > **Déboguer** > **Arguments d’application**. Le fait de définir l’argument dans la page de propriétés Visual Studio l’ajoute au fichier *launchSettings.json*.

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a>Base du chemin

L' `--pathbase` argument définit le chemin d’accès de base d’application pour une application exécutée localement avec un chemin d’URL relatif `href` non racine (la balise est `/` définie sur un chemin d’accès autre que pour la mise en lots et la `<base>` production). Dans les exemples suivants, `/relative-URL-path` est la base du chemin de l’application. Pour plus d’informations, consultez chemin de la base de l' [application](xref:host-and-deploy/blazor/index#app-base-path).

> [!IMPORTANT]
> Contrairement au chemin fourni au `href` de la balise `<base>`, n’incluez pas de barre oblique (`/`) quand vous passez la valeur d’argument `--pathbase`. Si vous spécifiez `<base href="/CoolApp/">` (inclut une barre oblique) comme chemin de base de l’application dans la balise `<base>`, passez `--pathbase=/CoolApp` (aucune barre oblique de fin) comme valeur d’argument de ligne de commande.

* Passez l’argument lors de l’exécution de l’application localement à une invite de commandes. À partir du répertoire de l’application, exécutez :

  ```console
  dotnet run --pathbase=/relative-URL-path
  ```

* Ajoutez une entrée au fichier *launchSettings.json* de l’application dans le profil **IIS Express**. Ce paramètre est utilisé en cas d’exécution de l’application avec le débogueur Visual Studio et dans une invite de commandes avec `dotnet run`.

  ```json
  "commandLineArgs": "--pathbase=/relative-URL-path"
  ```

* Dans Visual Studio, spécifiez l’argument dans **Propriétés** > **Déboguer** > **Arguments d’application**. Le fait de définir l’argument dans la page de propriétés Visual Studio l’ajoute au fichier *launchSettings.json*.

  ```console
  --pathbase=/relative-URL-path
  ```

### <a name="urls"></a>URL

L’argument `--urls` définit les adresses IP ou les adresses d’hôtes avec les ports et protocoles sur lesquels il faut écouter les demandes.

* Passez l’argument lors de l’exécution de l’application localement à une invite de commandes. À partir du répertoire de l’application, exécutez :

  ```console
  dotnet run --urls=http://127.0.0.1:0
  ```

* Ajoutez une entrée au fichier *launchSettings.json* de l’application dans le profil **IIS Express**. Ce paramètre est utilisé en cas d’exécution de l’application avec le débogueur Visual Studio et dans une invite de commandes avec `dotnet run`.

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* Dans Visual Studio, spécifiez l’argument dans **Propriétés** > **Déboguer** > **Arguments d’application**. Le fait de définir l’argument dans la page de propriétés Visual Studio l’ajoute au fichier *launchSettings.json*.

  ```console
  --urls=http://127.0.0.1:0
  ```

## <a name="configure-the-linker"></a>Configurer l'éditeur de liens

Blazor effectue la liaison de langage intermédiaire (IL) lors de chaque génération afin de supprimer tout langage intermédiaire inutile des assemblys de sortie. La liaison d’assembly peut être contrôlée pendant le processus de build. Pour plus d'informations, consultez <xref:host-and-deploy/blazor/configure-linker>.
