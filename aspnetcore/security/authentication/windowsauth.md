---
title: Configurer l’authentification Windows dans ASP.NET Core
author: scottaddie
description: Découvrez comment configurer l’authentification Windows dans ASP.NET Core pour IIS et HTTP.sys.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 07/01/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 30f1f554a29412ed6b84115d457d2da1aba91c17
ms.sourcegitcommit: eb3e51d58dd713eefc242148f45bd9486be3a78a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67500499"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>Configurer l’authentification Windows dans ASP.NET Core

Par [Scott Addie](https://twitter.com/Scott_Addie) et [Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

Authentification Windows (également appelé authentification Negotiate, Kerberos ou NTLM) peut être configurée pour les applications ASP.NET Core hébergées avec [IIS](xref:host-and-deploy/iis/index), [Kestrel](xref:fundamentals/servers/kestrel), ou [HTTP.sys](xref:fundamentals/servers/httpsys) .

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Authentification Windows (également appelé authentification Negotiate, Kerberos ou NTLM) peut être configurée pour les applications ASP.NET Core hébergées avec [IIS](xref:host-and-deploy/iis/index) ou [HTTP.sys](xref:fundamentals/servers/httpsys).

::: moniker-end

L’authentification Windows s’appuie sur le système d’exploitation pour authentifier les utilisateurs des applications ASP.NET Core. Vous pouvez utiliser l’authentification Windows lorsque votre serveur s’exécute sur un réseau d’entreprise à l’aide d’identités de domaine Active Directory ou les comptes Windows pour identifier les utilisateurs. L’authentification Windows est mieux adaptée aux environnements intranet où les utilisateurs, les applications clientes et les serveurs web appartiennent au même domaine Windows.

> [!NOTE]
> L’authentification Windows n’est pas pris en charge avec HTTP/2. Demandes d’authentification peuvent être envoyés sur les réponses HTTP/2, mais le client doit mettre à niveau vers HTTP/1.1 avant l’authentification.

## <a name="iisiis-express"></a>IIS/IIS Express

Ajoutez des services d’authentification en appelant <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> espace de noms) dans `Startup.ConfigureServices`:

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

### <a name="launch-settings-debugger"></a>Lancer des paramètres (débogueur)

Configuration des paramètres de lancement affecte uniquement le *Properties/launchSettings.json* de fichiers pour IIS Express et ne configure IIS pour l’authentification Windows. Configuration du serveur est expliquée dans le [IIS](#iis) section.

Le **Web Application** modèle disponible via Visual Studio ou l’interface CLI .NET Core peut être configuré pour prendre en charge l’authentification Windows, ce qui met à jour le *Properties/launchSettings.json* fichier automatiquement.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

**nouveau projet**

1. Créer un nouveau projet.
1. Sélectionnez **Nouvelle application web ASP.NET Core**. Sélectionnez **Suivant**.
1. Fournissez un nom dans la **nom_projet** champ. Confirmer la **emplacement** entrée est correcte ou indiquez un emplacement pour le projet. Sélectionnez **Créer**.
1. Sélectionnez **modification** sous **authentification**.
1. Dans le **modifier l’authentification** fenêtre, sélectionnez **l’authentification Windows**. Sélectionnez **OK**.
1. Sélectionnez **Application web**.
1. Sélectionnez **Créer**.

Exécuter l’application. Le nom d’utilisateur s’affiche dans l’interface utilisateur de l’application rendue.

**Projet existant**

Les propriétés du projet activer l’authentification Windows et désactivez l’authentification anonyme :

1. Cliquez avec le bouton droit sur le projet dans **l’Explorateur de solutions**, puis sélectionnez **Propriétés**.
1. Sélectionnez l’onglet **Débogage**.
1. Désactivez la case à cocher **activer l’authentification anonyme**.
1. Sélectionnez la case à cocher **activer l’authentification Windows**.
1. Enregistrez et fermez la page de propriétés.

Vous pouvez également les propriétés peuvent être configurées dans le `iisSettings` nœud de la *launchSettings.json* fichier :

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[Visual Studio Code/.NET Core CLI](#tab/visual-studio-code+netcore-cli)

**nouveau projet**

Exécuter le [dotnet nouvelle](/dotnet/core/tools/dotnet-new) commande avec le `webapp` argument (application de Web ASP.NET Core) et `--auth Windows` commutateur :

```console
dotnet new webapp --auth Windows
```

**Projet existant**

Mise à jour le `iisSettings` nœud de la *launchSettings.json* fichier :

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

---

Lorsque vous modifiez un projet existant, vérifiez que le fichier projet contient une référence de package pour le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app) **ou** le [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) package NuGet.

### <a name="iis"></a>IIS

IIS utilise le [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) pour héberger des applications ASP.NET Core. L’authentification Windows est configurée pour IIS via le *web.config* fichier. Les sections suivantes montrent comment :

* Fournir une variable locale *web.config* fichier qui active l’authentification Windows sur le serveur quand l’application est déployée.
* Utilisez le Gestionnaire des services Internet pour configurer le *web.config* fichier d’une application ASP.NET Core qui a déjà été déployée sur le serveur.

Si vous n’avez pas déjà fait, activez IIS pour héberger des applications ASP.NET Core. Pour plus d'informations, consultez <xref:host-and-deploy/iis/index>.

Activer le Service de rôle IIS pour l’authentification Windows. Pour plus d’informations, consultez [activer l’authentification Windows dans les Services de rôle IIS (voir étape 2)](xref:host-and-deploy/iis/index#iis-configuration).

[Intergiciel d’intégration IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) est configuré pour authentifier les demandes automatiquement par défaut. Pour plus d’informations, consultez [héberger ASP.NET Core sur Windows avec IIS : Options d’IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).

Le Module ASP.NET Core est configuré pour transférer le jeton d’authentification de Windows à l’application par défaut. Pour plus d’informations, consultez [référence de configuration de Module ASP.NET Core : Attributs de l’élément aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

Utilisez **soit** des approches suivantes :

* **Avant la publication et le déploiement du projet,** ajoutez le code suivant *web.config* fichier à la racine du projet :

  [!code-xml[](windowsauth/sample_snapshot/web_2.config)]

  Lorsque le projet est publié par le SDK .NET Core (sans le `<IsTransformWebConfigDisabled>` propriété définie sur `true` dans le fichier projet), publié *web.config* fichier inclut le `<location><system.webServer><security><authentication>` section. Pour plus d’informations sur la `<IsTransformWebConfigDisabled>` propriété, consultez <xref:host-and-deploy/iis/index#webconfig-file>.

* **Après la publication et le déploiement du projet,** effectuer la configuration côté serveur avec le Gestionnaire des services Internet :

  1. Dans le Gestionnaire des services Internet, sélectionnez le site IIS sous la **Sites** nœud de la **connexions** encadré.
  1. Double-cliquez sur **authentification** dans le **IIS** zone.
  1. Sélectionnez **l’authentification anonyme**. Sélectionnez **désactiver** dans le **Actions** encadré.
  1. Sélectionnez **l’authentification Windows**. Sélectionnez **activer** dans le **Actions** encadré.

  Lorsque ces actions sont effectuées, le Gestionnaire des services Internet modifie l’application *web.config* fichier. Un `<system.webServer><security><authentication>` nœud est ajouté avec les paramètres mis à jour pour `anonymousAuthentication` et `windowsAuthentication`:

  [!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

  Le `<system.webServer>` section ajoutée à la *web.config* fichier par le Gestionnaire des services Internet se trouve en dehors de l’application `<location>` section ajoutée par le SDK .NET Core lors de l’application est publiée. Étant donné que la section est ajoutée à l’extérieur de la `<location>` nœud, les paramètres sont hérités par les [sous-applications](xref:host-and-deploy/iis/index#sub-applications) à l’application actuelle. Pour empêcher l’héritage, déplacer la `<security>` section à l’intérieur de la `<location><system.webServer>` section fourni le SDK .NET Core.

  Lorsque le Gestionnaire des services Internet est utilisé pour ajouter la configuration d’IIS, il affecte uniquement l’application *web.config* fichier sur le serveur. Un autre déploiement de l’application peut remplacer les paramètres sur le serveur si la copie du serveur de *web.config* est remplacé par le projet *web.config* fichier. Utilisez **soit** des approches suivantes pour gérer les paramètres :

  * Utilisez le Gestionnaire IIS pour réinitialiser les paramètres dans le *web.config* une fois que le fichier est remplacé sur le déploiement de fichiers.
  * Ajouter un *fichier web.config* à l’application localement avec les paramètres.

::: moniker range=">= aspnetcore-3.0"

## <a name="kestrel"></a>Kestrel

 Le [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) package NuGet peut être utilisé avec [Kestrel](xref:fundamentals/servers/kestrel) pour prendre en charge l’authentification Windows à l’aide de Negotiate, Kerberos et NTLM sur Windows, Linux et macOS.

> [!WARNING]
> Informations d’identification peuvent être conservées dans les demandes sur une connexion. *Négocier l’authentification ne doit pas être utilisée avec les serveurs proxy, sauf si le serveur proxy gère une affinité de connexion de 1:1 (une connexion persistante) avec Kestrel.*

> [!NOTE]
> Le gestionnaire à Negotiate détecte si le serveur sous-jacent prend en charge l’authentification Windows en mode natif et s’il est activé. Si le serveur prend en charge l’authentification Windows, mais il est désactivé, une erreur est générée pour vous demander d’activer l’implémentation du serveur. Lorsque l’authentification Windows est activée sur le serveur, le gestionnaire à Negotiate transfère en toute transparence à celui-ci.

 Ajoutez des services d’authentification en appelant <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (`Microsoft.AspNetCore.Authentication.Negotiate` espace de noms) et `AddNegotitate` (`Microsoft.AspNetCore.Authentication.Negotiate` espace de noms) dans `Startup.ConfigureServices`:

 ```csharp
services.AddAuthentication(NegotiateDefaults.AuthenticationScheme)
    .AddNegotiate();
```

Ajoutez l’intergiciel d’authentification en appelant <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> dans `Startup.Configure`:

 ```csharp
app.UseAuthentication();

app.UseMvc();
```

Pour plus d’informations sur l’intergiciel (middleware), consultez <xref:fundamentals/middleware/index>.

Les demandes anonymes sont autorisés. Utilisez [ASP.NET Core autorisation](xref:security/authorization/introduction) contester les demandes anonymes pour l’authentification.

### <a name="windows-environment-configuration"></a>Configuration de l’environnement Windows

Le [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) composant effectue l’authentification en Mode utilisateur. Noms de principal du service (SPN) doit être ajoutés au compte d’utilisateur exécutant le service, pas le compte d’ordinateur. Exécutez `setspn -S HTTP/mysrevername.mydomain.com myuser` dans un interpréteur de commandes d’administration.

### <a name="linux-and-macos-environment-configuration"></a>Configuration de l’environnement Linux et macOS

Instructions pour joindre un ordinateur Linux ou macOS à un domaine Windows sont disponibles dans le [connecter Azure Data Studio à votre serveur SQL à l’aide de l’authentification Windows - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) article. Les instructions de créer un compte d’ordinateur pour l’ordinateur Linux sur le domaine. Noms principaux de service doit être ajouté à ce compte d’ordinateur.

> [!NOTE]
> Lorsque vous suivez les instructions de la [connecter Azure Data Studio à votre serveur SQL à l’aide de l’authentification Windows - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) article, remplacez `python-software-properties` avec `python3-software-properties` si nécessaire.

Une fois l’ordinateur Linux ou macOS est joint au domaine, les étapes supplémentaires sont nécessaires pour fournir un [fichier keytab](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) avec les noms SPN :

* Sur le contrôleur de domaine, ajoutez le nouveau service web SPN pour le compte d’ordinateur :
  * `setspn -S HTTP/mywebservice.mydomain.com mymachine`
  * `setspn -S HTTP/mywebservice@MYDOMAIN.COM mymachine`
* Utilisez [ktpass](/windows-server/administration/windows-commands/ktpass) pour générer un fichier keytab :
  * `ktpass -princ HTTP/mywebservice.mydomain.com@MYDOMAIN.COM -pass myKeyTabFilePassword -mapuser MYDOMAIN\mymachine$ -pType KRB5_NT_PRINCIPAL -out c:\temp\mymachine.HTTP.keytab -crypto AES256-SHA1`
  * Certains champs doivent être spécifiés en majuscules comme indiqué.
* Copiez le fichier keytab sur l’ordinateur Linux ou macOS.
* Sélectionnez le fichier keytab via une variable d’environnement : `export KRB5_KTNAME=/tmp/mymachine.HTTP.keytab`
* Appeler `klist` pour afficher les noms SPN actuellement disponibles pour utilisation.

> [!NOTE]
> Un fichier keytab contient des informations d’identification d’accès de domaine et doit être protégé en conséquence.

::: moniker-end

## <a name="httpsys"></a>HTTP.sys

[HTTP.sys](xref:fundamentals/servers/httpsys) prend en charge l’authentification de Windows en Mode noyau à l’aide de Negotiate, NTLM ou l’authentification de base.

Ajoutez des services d’authentification en appelant <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> espace de noms) dans `Startup.ConfigureServices`:

```csharp
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

Configurer l’hôte web de l’application pour utiliser HTTP.sys avec l’authentification Windows (*Program.cs*). <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> est dans le <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> espace de noms.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_GenericHost.cs?highlight=13-19)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_WebHost.cs?highlight=9-15)]

::: moniker-end

> [!NOTE]
> HTTP.sys délègue l’authentification en mode noyau avec le protocole d’authentification Kerberos. L’authentification en mode utilisateur n’est pas prise en charge avec Kerberos et HTTP.sys. Le compte d’ordinateur doit être utilisé pour déchiffrer le ticket/jeton Kerberos obtenu à partir d’Active Directory et transféré par le client au serveur afin d’authentifier l’utilisateur. Inscrivez le nom de principal du service (SPN) pour l’hôte, et non l’utilisateur de l’application.

> [!NOTE]
> HTTP.sys n’est pas pris en charge sur Nano Server version 1709 ou ultérieure. Pour utiliser l’authentification Windows et HTTP.sys avec Nano Server, utilisez un [conteneur de Server Core (microsoft/windowsservercore)](https://hub.docker.com/r/microsoft/windowsservercore/). Pour plus d’informations sur Server Core, consultez [quel est l’option d’installation Server Core de Windows Server ?](/windows-server/administration/server-core/what-is-server-core).

## <a name="authorize-users"></a>Autoriser les utilisateurs

L’état de configuration de l’accès anonyme détermine la façon dont le `[Authorize]` et `[AllowAnonymous]` attributs sont utilisés dans l’application. Les deux sections suivantes expliquent comment gérer les États de configuration non autorisés et autorisées de l’accès anonyme.

### <a name="disallow-anonymous-access"></a>Interdire l’accès anonyme

Lorsque l’authentification Windows est activée et que l’accès anonyme est désactivé, le `[Authorize]` et `[AllowAnonymous]` attributs n’ont aucun effet. Si un site IIS est configuré pour interdire l’accès anonyme, la demande n’atteint jamais l’application. Pour cette raison, le `[AllowAnonymous]` attribut n’est pas applicable.

### <a name="allow-anonymous-access"></a>Autoriser l’accès anonyme

Lorsque l’authentification Windows et l’accès anonyme sont activées, utiliser le `[Authorize]` et `[AllowAnonymous]` attributs. Le `[Authorize]` attribut vous permet de sécuriser les points de terminaison de l’application qui requièrent une authentification. Le `[AllowAnonymous]` substitutions d’attribut le `[Authorize]` attribut dans les applications qui autorisent l’accès anonyme. Pour plus d’informations de l’utilisation d’attribut, consultez <xref:security/authorization/simple>.

> [!NOTE]
> Par défaut, les utilisateurs qui ne disposent pas d’autorisation pour accéder à une page sont présentées avec une réponse HTTP 403 vide. Le [StatusCodePages Middleware](xref:fundamentals/error-handling#usestatuscodepages) peut être configuré pour fournir aux utilisateurs une meilleure expérience « Accès refusé ».

## <a name="impersonation"></a>emprunt d'identité

ASP.NET Core n’implémente pas l’emprunt d’identité. Applications s’exécutent avec l’identité d’application pour toutes les demandes, à l’aide d’identité de pool ou les processus de l’application. Si l’application doit effectuer une action de la part d’un utilisateur, utilisez [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) dans un [intergiciel (middleware) inline terminal](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) dans `Startup.Configure`. Exécuter une action unique dans ce contexte, puis fermez le contexte.

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

`RunImpersonated` ne prend pas en charge les opérations asynchrones et ne doit pas être utilisé pour les scénarios complexes. Par exemple, encapsulant les demandes entières ou des chaînes d’intergiciel (middleware) n’est pas pris en charge ni recommandé.

::: moniker range=">= aspnetcore-3.0"

Bien que le [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) package permet à l’authentification sur Windows, Linux et macOS, l’emprunt d’identité est uniquement pris en charge sur Windows.

::: moniker-end

## <a name="claims-transformations"></a>Transformations de revendications

::: moniker range=">= aspnetcore-3.0"

Lors de l’hébergement avec IIS, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> n’est pas appelée en interne pour initialiser un utilisateur. Par conséquent, une implémentation de <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> utilisée pour transformer les revendications après chaque authentification n’est pas activée par défaut. Pour plus d’informations et un exemple de code qui active les transformations de revendications, consultez <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Lorsque vous hébergez avec mode in-process IIS, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> n’est pas appelée en interne pour initialiser un utilisateur. Par conséquent, une implémentation de <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> utilisée pour transformer les revendications après chaque authentification n’est pas activée par défaut. Pour plus d’informations et un exemple de code qui active les transformations de revendications lors de l’hébergement intra-processus, consultez <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.

::: moniker-end

## <a name="additional-resources"></a>Ressources supplémentaires

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>
