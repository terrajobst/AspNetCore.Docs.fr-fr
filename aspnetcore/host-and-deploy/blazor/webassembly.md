---
title: Héberger et déployer ASP.NET Core Blazor webassembly
author: guardrex
description: Découvrez comment héberger et déployer une application Blazor à l’aide de ASP.NET Core, de réseaux de distribution de contenu (CDN), de serveurs de fichiers et de pages GitHub.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/webassembly
ms.openlocfilehash: 8ed95cdb96804e08c3f1273bbea8f64a8e4f173c
ms.sourcegitcommit: 9ee99300a48c810ca6fd4f7700cd95c3ccb85972
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76160247"
---
# <a name="host-and-deploy-aspnet-core-opno-locblazor-webassembly"></a>Héberger et déployer ASP.NET Core Blazor webassembly

Par [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) et [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Avec le [modèle d’hébergement WebassemblyBlazor](xref:blazor/hosting-models#blazor-webassembly):

* L’application Blazor, ses dépendances et le Runtime .NET sont téléchargés dans le navigateur.
* L’application est exécutée directement sur le thread d’interface utilisateur du navigateur.

Les stratégies de déploiement suivantes sont prises en charge :

* L’application Blazor est fournie par une application ASP.NET Core. Cette stratégie est abordée dans la section [Déploiement hébergé avec ASP.NET Core](#hosted-deployment-with-aspnet-core).
* L’application Blazor est placée sur un service ou un serveur Web d’hébergement statique, où .NET n’est pas utilisé pour traiter l’application Blazor. Cette stratégie est traitée dans la section [Déploiement autonome](#standalone-deployment) , qui comprend des informations sur l’hébergement d’une application Blazor webassembly en tant que sous-application IIS.

## <a name="rewrite-urls-for-correct-routing"></a>Réécriture d’URL pour un routage correct

Les requêtes de routage pour les composants de page dans une application Blazor webassembly ne sont pas aussi simples que les demandes de routage dans un serveur de Blazor, une application hébergée. Prenons l’exemple d’une application Blazor webassembly avec deux composants :

* *Main. razor* &ndash; se charge à la racine de l’application et contient un lien vers le composant `About` (`href="About"`).
* *À propos de* la &ndash; du composant `About`. Razor.

Quand le document par défaut de l’application est demandé à l’aide de la barre d’adresses du navigateur (par exemple, `https://www.contoso.com/`) :

1. Le navigateur effectue une requête.
1. La page par défaut est retournée, généralement *index.html*.
1. *index.HTML* amorce l’application.
1. le routeur de Blazorest chargé et le composant de `Main` Razor est rendu.

Dans la page principale, le fait de sélectionner le lien vers le composant `About` fonctionne sur le client, car le routeur Blazor empêche le navigateur de faire une demande sur Internet pour `www.contoso.com` pour `About` et sert le composant `About` rendu lui-même. Toutes les requêtes pour les points de terminaison internes *au sein de l’application Blazor Webassembly* fonctionnent de la même façon : les demandes ne déclenchent pas de demandes basées sur un navigateur vers des ressources hébergées sur le serveur sur Internet. Le routeur gère les requêtes en interne.

Si une requête pour `www.contoso.com/About` est effectuée à l’aide de la barre d’adresses du navigateur, elle échoue. Comme cette ressource n’existe pas sur l’hôte Internet de l’application, une réponse *404 – Non trouvé* est retournée.

Étant donné que les navigateurs envoient des requêtes aux hôtes basés sur Internet pour des pages côté client, les serveurs web et les services d’hébergement doivent réécrire toutes les requêtes pour les ressources qui ne se trouvent pas physiquement sur le serveur afin qu’elles pointent vers la page *index.html*. Quand *index. html* est retourné, le routeur de Blazor de l’application prend le relais et répond avec la ressource correcte.

Lors du déploiement sur un serveur IIS, vous pouvez utiliser le module de réécriture d’URL avec le fichier *Web. config* publié de l’application. Pour plus d’informations, consultez la section [IIS](#iis) .

## <a name="hosted-deployment-with-aspnet-core"></a>Déploiement hébergé avec ASP.NET Core

Un *déploiement hébergé* sert l’application Blazor webassembly aux navigateurs à partir d’une [application ASP.net Core](xref:index) qui s’exécute sur un serveur Web.

L’application Blazor est incluse avec l’application ASP.NET Core dans la sortie publiée afin que les deux applications soient déployées ensemble. Un serveur web capable d’héberger une application ASP.NET Core est nécessaire. Pour un déploiement hébergé, Visual Studio comprend le modèle de projet d' **applicationBlazor Webassembly** (`blazorwasm` modèle lors de l’utilisation de la commande [dotnet New](/dotnet/core/tools/dotnet-new) ) avec l’option **hébergée** sélectionnée.

Pour plus d’informations sur l’hébergement et le déploiement d’applications ASP.NET Core, consultez <xref:host-and-deploy/index>.

Pour plus d’informations concernant le déploiement sur Azure App Service, consultez <xref:tutorials/publish-to-azure-webapp-using-vs>.

## <a name="standalone-deployment"></a>Déploiement autonome

Un *Déploiement autonome* sert l’application Blazor webassembly sous la forme d’un ensemble de fichiers statiques demandés directement par les clients. Tout serveur de fichiers statique peut traiter l’application Blazor.

Les ressources d’un déploiement autonome sont publiées dans le dossier *bin/Release/{TARGET FRAMEWORK}/publish/{ASSEMBLY NAME}/dist*.

### <a name="iis"></a>IIS

IIS est un serveur de fichiers statiques permettant de Blazor des applications. Pour configurer IIS pour héberger des Blazor, consultez [créer un site Web statique sur IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).

Les ressources publiées sont créées dans le dossier */bin/Release/{FRAMEWORK CIBLE}/publish*. Hébergez le contenu du dossier *publish* sur le serveur web ou le service d’hébergement.

#### <a name="webconfig"></a>web.config

Quand un projet de Blazor est publié, un fichier *Web. config* est créé avec la configuration IIS suivante :

* Les types MIME sont définis pour les extensions de fichiers suivantes :
  * *. dll* &ndash; `application/octet-stream`
  * &ndash; *. json* `application/json`
  * *wasm* &ndash; `application/wasm`
  * *woff* &ndash; `application/font-woff`
  * *woff2* &ndash; `application/font-woff`
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

  Supprimez le gestionnaire dans le fichier *Web. config* publié de Blazor application en ajoutant une section `<handlers>` au fichier :

  ```xml
  <handlers>
    <remove name="aspNetCore" />
  </handlers>
  ```

* Désactivez l’héritage de la section `<system.webServer>` de l’application racine (parent) à l’aide d’un élément `<location>` avec `inheritInChildApplications` défini sur `false`:

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

Si vous recevez un message *500 – Erreur interne du serveur* et que le Gestionnaire IIS lève des erreurs quand vous tentez d’accéder à la configuration du site web, vérifiez que le module de réécriture d’URL est installé. Quand le module n’est pas installé, le fichier *web.config* ne peut pas être analysé par IIS. Cela empêche le gestionnaire des services Internet de charger la configuration du site Web et le site Web de servir les fichiers statiques de Blazor.

Pour plus d’informations sur le dépannage des déploiements sur IIS, consultez <xref:test/troubleshoot-azure-iis>.

### <a name="azure-storage"></a>Azure Storage

L’hébergement de fichiers statiques [Azure Storage](/azure/storage/) permet l’hébergement d’applications Blazor sans serveur. Les noms de domaine personnalisé, le réseau de distribution de contenu Azure (CDN) et HTTPS sont pris en charge.

Lorsque le service blob est activé pour l’hébergement de site Web statique sur un compte de stockage :

* Définissez le **nom du document d’index** sur `index.html`.
* Définissez le **chemin d’accès au document d’erreur** sur `index.html`. Les composants de Razor et autres points de terminaison non-fichier ne se trouvent sur les chemins d’accès physiques dans le contenu statique stocké par le service blob. Lorsqu’une demande pour l’une de ces ressources est reçue que le routeur Blazor doit gérer, l’erreur *404-introuvable* générée par le service BLOB achemine la requête vers le **chemin d’accès du document d’erreur**. L’objet BLOB *index. html* est retourné et le routeur Blazor charge et traite le chemin d’accès.

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

Pour héberger des Blazor dans l’Ancreur à l’aide de nginx, configurez le fichier dockerfile pour utiliser l’image Nginx alpine. Mettez à jour le fichier Dockerfile pour copier le fichier *nginx.config* dans le conteneur.

Ajoutez une ligne au fichier Dockerfile, comme indiqué dans l’exemple suivant :

```Dockerfile
FROM nginx:alpine
COPY ./bin/Release/netstandard2.0/publish /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

### <a name="apache"></a>Apache

Pour déployer une application Blazor webassembly sur CentOS 7 ou version ultérieure :

1. Créez le fichier de configuration Apache. L’exemple suivant est un fichier de configuration simplifié (*blazorapp. config*) :

   ```
   <VirtualHost *:80>
       ServerName www.example.com
       ServerAlias *.example.com

       DocumentRoot "/var/www/blazorapp"
       ErrorDocument 404 /index.html

       AddType aplication/wasm .wasm
       AddType application/octet-stream .dll
   
       <Directory "/var/www/blazorapp">
           Options -Indexes
           AllowOverride None
       </Directory>

       <IfModule mod_deflate.c>
           AddOutputFilterByType DEFLATE text/css
           AddOutputFilterByType DEFLATE application/javascript
           AddOutputFilterByType DEFLATE text/html
           AddOutputFilterByType DEFLATE application/octet-stream
           AddOutputFilterByType DEFLATE application/wasm
           <IfModule mod_setenvif.c>
           BrowserMatch ^Mozilla/4 gzip-only-text/html
           BrowserMatch ^Mozilla/4.0[678] no-gzip
           BrowserMatch bMSIE !no-gzip !gzip-only-text/html
       </IfModule>
       </IfModule>

       ErrorLog /var/log/httpd/blazorapp-error.log
       CustomLog /var/log/httpd/blazorapp-access.log common
   </VirtualHost>
   ```

1. Placez le fichier de configuration Apache dans le répertoire `/etc/httpd/conf.d/`, qui est le répertoire de configuration Apache par défaut dans CentOS 7.

1. Placez les fichiers de l’application dans le répertoire `/var/www/blazorapp` (l’emplacement spécifié pour `DocumentRoot` dans le fichier de configuration).

1. Redémarrez le service Apache.

Pour plus d’informations, consultez [mod_mime](https://httpd.apache.org/docs/2.4/mod/mod_mime.html) et [mod_deflate](https://httpd.apache.org/docs/current/mod/mod_deflate.html).

### <a name="github-pages"></a>GitHub Pages

Pour gérer les réécritures d’URL, ajoutez un fichier *404.html* avec un script qui gère la redirection de la requête vers la page *index.html*. Pour obtenir un exemple d’implémentation fournie par la communauté, consultez [Single Page Apps for GitHub Pages](https://spa-github-pages.rafrex.com/) (Applications à une seule page pour GitHub Pages) ([rafrex/spa-github-pages sur GitHub](https://github.com/rafrex/spa-github-pages#readme)). Vous pouvez obtenir un exemple d’utilisation de cette approche à l’adresse [blazor-demo/blazor-demo.github.io sur GitHub](https://github.com/blazor-demo/blazor-demo.github.io) ([site actif](https://blazor-demo.github.io/)).

Quand vous utilisez un site de projet plutôt qu’un site d’entreprise, ajoutez ou mettez à jour la balise `<base>` dans *index.html*. Définissez la valeur d’attribut `href` sur le nom du dépôt GitHub avec une barre oblique (par exemple, `my-repository/`.

## <a name="host-configuration-values"></a>Valeurs de configuration de l’hôte

[Blazor applications Webassembly](xref:blazor/hosting-models#blazor-webassembly) peuvent accepter les valeurs de configuration d’hôte suivantes comme arguments de ligne de commande au moment de l’exécution dans l’environnement de développement.

### <a name="content-root"></a>Racine de contenu

L’argument `--contentroot` définit le chemin d’accès absolu au répertoire qui contient les fichiers de contenu de l’application ([racine du contenu](xref:fundamentals/index#content-root)). Dans les exemples suivants, `/content-root-path` est le chemin racine du contenu de l’application.

* Passez l’argument lors de l’exécution de l’application localement à une invite de commandes. À partir du répertoire de l’application, exécutez :

  ```dotnetcli
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

L’argument `--pathbase` définit le chemin d’accès de base d’application pour une application exécutée localement avec un chemin d’URL relatif non racine (la balise `<base>` `href` est définie sur un chemin d’accès autre que `/` pour la mise en lots et la production). Dans les exemples suivants, `/relative-URL-path` est la base du chemin de l’application. Pour plus d’informations, consultez chemin de la base de l' [application](xref:host-and-deploy/blazor/index#app-base-path).

> [!IMPORTANT]
> Contrairement au chemin fourni au `href` de la balise `<base>`, n’incluez pas de barre oblique (`/`) quand vous passez la valeur d’argument `--pathbase`. Si vous spécifiez `<base href="/CoolApp/">` (inclut une barre oblique) comme chemin de base de l’application dans la balise `<base>`, passez `--pathbase=/CoolApp` (aucune barre oblique de fin) comme valeur d’argument de ligne de commande.

* Passez l’argument lors de l’exécution de l’application localement à une invite de commandes. À partir du répertoire de l’application, exécutez :

  ```dotnetcli
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

### <a name="urls"></a>Adresses URL

L’argument `--urls` définit les adresses IP ou les adresses d’hôtes avec les ports et protocoles sur lesquels il faut écouter les demandes.

* Passez l’argument lors de l’exécution de l’application localement à une invite de commandes. À partir du répertoire de l’application, exécutez :

  ```dotnetcli
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

Blazor effectue une liaison IL (Intermediate Language) sur chaque Build pour supprimer l’IL inutile des assemblys de sortie. La liaison d’assembly peut être contrôlée pendant le processus de build. Pour plus d'informations, consultez <xref:host-and-deploy/blazor/configure-linker>.
