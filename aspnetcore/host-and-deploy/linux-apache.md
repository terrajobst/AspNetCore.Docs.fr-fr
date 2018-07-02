---
title: Héberger ASP.NET Core sur Linux avec Apache
description: Découvrez comment configurer Apache comme serveur proxy inverse sur CentOS pour rediriger le trafic HTTP vers une application web ASP.NET Core s’exécutant sur Kestrel.
author: spboyer
ms.author: spboyer
ms.custom: mvc
ms.date: 03/13/2018
uid: host-and-deploy/linux-apache
ms.openlocfilehash: c46c0aa578867ce306adc67a2e0d8b650b5fa5bd
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960890"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a>Héberger ASP.NET Core sur Linux avec Apache

Par [Shayne Boyer](https://github.com/spboyer)

À l’aide de ce guide, découvrez comment configurer [Apache](https://httpd.apache.org/) comme serveur proxy inverse sur [CentOS 7](https://www.centos.org/) pour rediriger le trafic HTTP vers une application web ASP.NET Core s’exécutant sur [Kestrel](xref:fundamentals/servers/kestrel). [L’extension mod_proxy](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) et les modules associés créent le proxy inverse du serveur.

## <a name="prerequisites"></a>Prérequis

1. Serveur exécutant CentOS 7 avec un compte d’utilisateur standard disposant du privilège sudo.
1. Installez le runtime .NET Core sur le serveur.
   1. Visitez la [page All Downloads de .NET Core](https://www.microsoft.com/net/download/all).
   1. Dans la liste **Runtime**, sélectionnez le runtime le plus récent qui n’est pas une préversion.
   1. Sélectionnez et suivez les instructions pour CentOS/Oracle.
1. Une application ASP.NET Core existante.

## <a name="publish-and-copy-over-the-app"></a>Publier et copier sur l’application

Configurez l’application pour un [déploiement dépendant du framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd).

Exécutez [dotnet publish](/dotnet/core/tools/dotnet-publish) à partir de l’environnement de développement pour empaqueter une application dans un répertoire (par exemple, *bin/Release/&lt;moniker_framework_target&gt;/publish*) exécutable sur le serveur :

```console
dotnet publish --configuration Release
```

L’application peut également être publiée en tant que [déploiement autonome](/dotnet/core/deploying/#self-contained-deployments-scd) si vous préférez ne pas gérer le runtime .NET Core sur le serveur.

Copiez l’application ASP.NET Core sur le serveur à l’aide d’un outil qui s’intègre au flux de travail de l’organisation (par exemple, SCP ou SFTP). Il est courant de placer les applications web sous le répertoire *var* (par exemple, *var/aspnetcore/hellomvc*).

> [!NOTE]
> Dans un scénario de déploiement en production, un workflow d’intégration continue effectue le travail de publication de l’application et de copie des composants sur le serveur.

## <a name="configure-a-proxy-server"></a>Configurer un serveur proxy

Un proxy inverse est une configuration courante pour traiter les applications web dynamiques. Le proxy inverse met fin à la requête HTTP et la transfère à l’application ASP.NET.

Un serveur proxy est un serveur qui transfère les requêtes des clients à un autre serveur, au lieu de traiter celles-ci lui-même. Un proxy inverse transfère à une destination fixe, en général pour le compte de clients arbitraires. Dans ce guide, Apache est configuré comme proxy inverse s’exécutant sur le même serveur où Kestrel traite l’application ASP.NET Core.

Les requêtes étant transférées par le proxy inverse, utilisez le [middleware (intergiciel) des en-têtes transférés](xref:host-and-deploy/proxy-load-balancer) à partir du package [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/). Le middleware met à jour le `Request.Scheme`, à l’aide de l’en-tête `X-Forwarded-Proto`, afin que les URI de redirection et d’autres stratégies de sécurité fonctionnent correctement.

Tout composant qui dépend du schéma, tel que l’authentification, la génération de lien, les redirections et la géolocalisation, doit être placé après l’appel du middleware des en-têtes transférés. En règle générale, le middleware des en-têtes transférés doit être exécuté avant les autres middlewares, à l’exception des middlewares de gestion des erreurs et de diagnostic. Cet ordre permet au middleware qui repose sur les informations des en-têtes transférés d’utiliser les valeurs d’en-tête pour le traitement.

::: moniker range=">= aspnetcore-2.0"
> [!NOTE]
> Les deux configurations&mdash;avec ou sans serveur proxy inverse&mdash;sont des configurations d’hébergement valides et prises en charge pour les applications ASP.NET Core versions 2.0 ou ultérieures. Pour plus d’informations, consultez [Quand utiliser Kestrel avec un proxy inverse ?](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).
::: moniker-end

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Appelez la méthode [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) dans `Startup.Configure` avant d’appeler [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) ou un middleware de schéma d’authentification similaire. Configurez le middleware pour transférer les en-têtes `X-Forwarded-For` et `X-Forwarded-Proto` :

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Appelez la méthode [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) dans `Startup.Configure` avant d’appeler [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) et [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) ou un middleware de schéma d’authentification similaire. Configurez le middleware pour transférer les en-têtes `X-Forwarded-For` et `X-Forwarded-Proto` :

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseIdentity();
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

Si aucune option [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) n’est spécifiée au middleware, les en-têtes par défaut à transférer sont `None`.

Une configuration supplémentaire peut être nécessaire pour les applications hébergées derrière des serveurs proxy et des équilibreurs de charge. Pour plus d’informations, consultez [Configurer ASP.NET Core pour l’utilisation de serveurs proxy et d’équilibreurs de charge](xref:host-and-deploy/proxy-load-balancer).

### <a name="install-apache"></a>Installer Apache

Mettez à jour les packages CentOS vers leur dernière version stable :

```bash
sudo yum update -y
```

Installez le serveur web Apache sur CentOS avec une seule commande `yum` :

```bash
sudo yum -y install httpd mod_ssl
```

Exemple de sortie après exécution de la commande :

```bash
Downloading packages:
httpd-2.4.6-40.el7.centos.4.x86_64.rpm               | 2.7 MB  00:00:01
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
Installing : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 
Verifying  : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 

Installed:
httpd.x86_64 0:2.4.6-40.el7.centos.4

Complete!
```

> [!NOTE]
> Dans cet exemple, la sortie correspond à httpd.86_64, car la version de CentOS 7 est en 64 bits. Pour vérifier où Apache est installé, exécutez `whereis httpd` à partir d’une invite de commandes.

### <a name="configure-apache"></a>Configurer Apache

Les fichiers de configuration pour Apache se trouvent dans le répertoire `/etc/httpd/conf.d/`. Les fichiers avec l’extension *.conf* sont traités par ordre alphabétique en plus des fichiers de configuration des modules dans `/etc/httpd/conf.modules.d/`, qui contient tous les fichiers de configuration nécessaires au chargement des modules.

Créez un fichier de configuration nommé *hellomvc.conf*, pour l’application :

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ServerName www.example.com
    ServerAlias *.example.com
    ErrorLog ${APACHE_LOG_DIR}hellomvc-error.log
    CustomLog ${APACHE_LOG_DIR}hellomvc-access.log common
</VirtualHost>
```

Le bloc `VirtualHost` peut apparaître plusieurs fois, dans un ou plusieurs fichiers sur un serveur. Dans le fichier de configuration précédent, Apache accepte le trafic public sur le port 80. Le domaine `www.example.com` est pris en charge, et l’alias `*.example.com` aboutit au même site web. Pour plus d’informations, consultez [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) (Prise en charge des hôtes virtuels par nom). Les requêtes sont transmises par proxy à la racine vers le port 5000 du serveur sur 127.0.0.1. Pour la communication bidirectionnelle, `ProxyPass` et `ProxyPassReverse` sont requis.

> [!WARNING]
> La spécification d’une [directive ServerName](https://httpd.apache.org/docs/current/mod/core.html#servername) incorrecte dans le bloc **VirtualHost** expose votre application à des failles de sécurité. Une liaison générique de sous-domaine (par exemple, `*.example.com`) ne présente pas ce risque de sécurité si vous contrôlez le domaine parent en entier (par opposition à `*.com`, qui est vulnérable). Consultez la [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) pour plus d’informations.

Vous pouvez configurer la journalisation par `VirtualHost` à l’aide des directives `ErrorLog` et `CustomLog`. `ErrorLog` est l’emplacement où le serveur journalise les erreurs, tandis que `CustomLog` définit le nom de fichier et le format du fichier journal. Dans ce cas, c’est l’endroit où les informations des requêtes sont journalisées. Il y a une ligne pour chaque requête.

Enregistrez le fichier et testez la configuration. Si tout réussit, la réponse doit être `Syntax [OK]`.

```bash
sudo service httpd configtest
```

Redémarrez Apache :

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a>Surveillance de l’application

Apache est désormais configuré pour transférer les requêtes faites à `http://localhost:80` vers l’application ASP.NET Core s’exécutant sur Kestrel à l’adresse `http://127.0.0.1:5000`.  Cependant, Apache n’est pas configuré pour gérer le processus Kestrel. Utilisez *systemd* pour créer un fichier de service afin de démarrer et surveiller l’application web sous-jacente. *systemd* est un système d’initialisation qui fournit de nombreuses et puissantes fonctionnalités pour le démarrage, l’arrêt et la gestion des processus. 

### <a name="create-the-service-file"></a>Créer le fichier de service

Créez le fichier de définition de service :

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

Exemple de fichier de service pour l’application :

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

> [!NOTE]
> **Utilisateur** &mdash; Si l’utilisateur *apache* n’est pas utilisé par la configuration, vous devez d’abord créer l’utilisateur et l’affecter en tant que propriétaire des fichiers.

> [!NOTE]
> Certaines valeurs (par exemple, les chaînes de connexion SQL) doivent être placées dans une séquence d’échappement afin que les fournisseurs de configuration puissent lire les variables d’environnement. Utilisez la commande suivante pour générer une valeur correctement placée dans une séquence d’échappement en vue de son utilisation dans le fichier de configuration :
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

Enregistrez le fichier et activez le service :

```bash
systemctl enable kestrel-hellomvc.service
```

Démarrez le service et vérifiez qu’il s’exécute :

```bash
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on CentOS 7
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

Le proxy inverse étant configuré et Kestrel étant géré via *systemd*, l’application web est maintenant entièrement configurée et accessible à partir d’un navigateur sur la machine locale à l’adresse `http://localhost`. Comme l’indiquent les en-têtes de réponse, l’en-tête **Server** montre que l’application ASP.NET Core est traitée par Kestrel :

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>Affichage des journaux

Puisque l’application web utilisant Kestrel est gérée à l’aide de *systemd*, les processus et les événements sont enregistrés dans un journal centralisé. Cependant, ce journal comprend les entrées de tous les services et processus gérés par *systemd*. Pour afficher les éléments propres à `kestrel-hellomvc.service`, utilisez la commande suivante :

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

Pour le filtrage du temps, spécifiez les options appropriées dans la commande. Par exemple, utilisez `--since today` pour filtrer en fonction du jour actuel ou `--until 1 hour ago` pour voir les entrées de l’heure précédente. Pour plus d’informations, consultez la [page man pour journalctl](https://www.unix.com/man-page/centos/1/journalctl/).

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a>Sécurisation de l’application

### <a name="configure-firewall"></a>Configurer le pare-feu

*Firewalld* est un démon dynamique pour gérer le pare-feu avec prise en charge des zones réseau. Les ports et le filtrage des paquets peuvent toujours être gérés par iptables. *Firewalld* doit être installé par défaut. Vous pouvez utiliser `yum` pour installer le package ou vérifier qu’il est installé.

```bash
sudo yum install firewalld -y
```

Utilisez `firewalld` pour ouvrir seulement les ports nécessaires à l’application. Dans ce cas, les ports 80 et 443 sont utilisés. Les commandes suivantes définissent l’ouverture des ports 80 et 443 de façon permanente :

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

Rechargez les paramètres du pare-feu. Vérifiez les services et les ports disponibles dans la zone par défaut. Vous voyez les options disponibles en examinant le résultat de `firewall-cmd -h`.

```bash
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

```bash
public (default, active)
interfaces: eth0
sources: 
services: dhcpv6-client
ports: 443/tcp 80/tcp
masquerade: no
forward-ports: 
icmp-blocks: 
rich rules: 
```

### <a name="ssl-configuration"></a>Configuration de SSL

Pour configurer Apache pour SSL, vous utilisez le module *mod_ssl*. Le module *mod_ssl* a été installé parallèlement au module *httpd*. S’il n’a pas été installé, utilisez `yum` pour l’ajouter à la configuration.

```bash
sudo yum install mod_ssl
```

Pour mettre en œuvre SSL, installez le module `mod_rewrite` afin de permettre la réécriture d’URL :

```bash
sudo yum install mod_rewrite
```

Modifiez le fichier *hellomvc.conf* pour permettre la réécriture d’URL et sécuriser les communications sur le port 443 :

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

> [!NOTE]
> Cet exemple utilise un certificat généré localement. **SSLCertificateFile** doit être le fichier de certificat principal pour le nom de domaine. **SSLCertificateKeyFile** doit être le fichier de clé généré quand la demande de signature de certificat est créée. **SSLCertificateChainFile** doit être le fichier de certificat intermédiaire (le cas échéant) qui a été fourni par l’autorité de certification.

Enregistrez le fichier et testez la configuration :

```bash
sudo service httpd configtest
```

Redémarrez Apache :

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a>Autres suggestions pour Apache

### <a name="additional-headers"></a>En-têtes facultatifs

Afin d’assurer une protection contre les attaques malveillantes, vous devez modifier ou ajouter quelques en-têtes. Vérifiez que le module `mod_headers` est installé :

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a>Sécuriser Apache contre les attaques par détournement de clic

Le [détournement de clic](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), également appelé *UI redress attack*, est une attaque malveillante qui amène le visiteur d’un site web à cliquer sur un lien ou un bouton sur une page différente de celle qu’il est en train de visiter. Utilisez `X-FRAME-OPTIONS` pour sécuriser le site.

Modifiez le fichier *httpd.conf* :

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

Ajoutez la ligne `Header append X-FRAME-OPTIONS "SAMEORIGIN"`. Enregistrez le fichier. Redémarrez Apache.

#### <a name="mime-type-sniffing"></a>Détection de type MIME

L’en-tête `X-Content-Type-Options` empêche Internet Explorer d’effectuer une *détection de données MIME* (détermination du `Content-Type` d’un fichier à partir de son contenu). Si le serveur définit l’en-tête `Content-Type` sur `text/html` et que l’option `nosniff` est définie, Internet Explorer restitue le contenu en tant que `text/html`, quel que soit le contenu du fichier.

Modifiez le fichier *httpd.conf* :

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

Ajoutez la ligne `Header set X-Content-Type-Options "nosniff"`. Enregistrez le fichier. Redémarrez Apache.

### <a name="load-balancing"></a>Équilibrage de charge

Cet exemple montre comment installer et configurer Apache sur CentOS 7 et Kestrel sur la même machine de l’instance. Afin de multiplier les instances en cas de défaillance, l’utilisation de *mod_proxy_balancer* et la modification du **VirtualHost** permettent de gérer plusieurs instances des applications web derrière le serveur proxy Apache.

```bash
sudo yum install mod_proxy_balancer
```

Dans le fichier de configuration illustré ci-dessous, une instance supplémentaire de l’application `hellomvc` est configurée pour s’exécuter sur le port 5001. La section *Proxy* est définie avec une configuration d’équilibreur qui comprend deux membres pour équilibrer la charge selon la méthode *byrequests*.

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPass / balancer://mycluster/ 

    ProxyPassReverse / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5001/

    <Proxy balancer://mycluster>
        BalancerMember http://127.0.0.1:5000
        BalancerMember http://127.0.0.1:5001 
        ProxySet lbmethod=byrequests
    </Proxy>

    <Location />
        SetHandler balancer
    </Location>
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

### <a name="rate-limits"></a>Limites du débit

Vous pouvez utiliser *mod_ratelimit*, qui est inclus dans le module *httpd*, pour limiter la bande passante des clients :

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
L’exemple de fichier limite la bande passante à 600 Ko/s sous l’emplacement racine :

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

## <a name="additional-resources"></a>Ressources supplémentaires

* [Configurer ASP.NET Core pour l’utilisation de serveurs proxy et d’équilibreurs de charge](xref:host-and-deploy/proxy-load-balancer)
