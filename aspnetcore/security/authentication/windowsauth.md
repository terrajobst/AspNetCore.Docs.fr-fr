---
title: Configurer l’authentification Windows dans ASP.NET Core
author: scottaddie
description: Découvrez comment configurer l’authentification Windows dans ASP.NET Core, à l’aide d’IIS Express, IIS et HTTP.sys.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 04/03/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: bd4ffa79c4d1e0070c820fa9c06b0a84c3aaae74
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59468651"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>Configurer l’authentification Windows dans ASP.NET Core

Par [Scott Addie](https://twitter.com/Scott_Addie) et [Luke Latham](https://github.com/guardrex)

[L’authentification Windows](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) peut être configuré pour les applications ASP.NET Core hébergées avec [IIS](xref:host-and-deploy/iis/index) ou [HTTP.sys](xref:fundamentals/servers/httpsys).

L’authentification Windows s’appuie sur le système d’exploitation pour authentifier les utilisateurs des applications ASP.NET Core. Vous pouvez utiliser l’authentification Windows lorsque votre serveur s’exécute sur un réseau d’entreprise à l’aide d’identités de domaine Active Directory ou les comptes Windows pour identifier les utilisateurs. L’authentification Windows est mieux adaptée aux environnements intranet où les utilisateurs, les applications clientes et les serveurs web appartiennent au même domaine Windows.

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>Activer l’authentification Windows dans une application ASP.NET Core

Le **Web Application** modèle disponible via Visual Studio ou l’interface CLI .NET Core peut être configuré pour prendre en charge l’authentification Windows.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="use-the-windows-authentication-app-template-for-a-new-project"></a>Utiliser le modèle d’application de l’authentification Windows pour un nouveau projet

Dans Visual Studio :

1. Créer un nouveau projet.
1. Sélectionnez **Nouvelle application web ASP.NET Core**. Sélectionnez **Suivant**.
1. Fournissez un nom dans la **nom_projet** champ. Confirmer la **emplacement** entrée est correcte ou indiquez un emplacement pour le projet. Sélectionnez **Créer**.
1. Sélectionnez **modification** sous **authentification**.
1. Dans le **modifier l’authentification** fenêtre, sélectionnez **l’authentification Windows**. Sélectionnez **OK**.
1. Sélectionnez **Application web**.
1. Sélectionnez **Créer**.

Exécuter l’application. Le nom d’utilisateur s’affiche dans l’interface utilisateur de l’application rendue.

### <a name="manual-configuration-for-an-existing-project"></a>Configuration manuelle pour un projet existant

Les propriétés du projet vous autorise à activer l’authentification Windows et désactivez l’authentification anonyme :

1. Cliquez sur le projet dans Visual Studio **l’Explorateur de solutions** et sélectionnez **propriétés**.
1. Sélectionnez l’onglet **Débogage**.
1. Désactivez la case à cocher **activer l’authentification anonyme**.
1. Sélectionnez la case à cocher **activer l’authentification Windows**.

Vous pouvez également les propriétés peuvent être configurées dans le `iisSettings` nœud de la *launchSettings.json* fichier :

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="net-core-clitabnetcore-cli"></a>[CLI .NET Core](#tab/netcore-cli)

Utilisez le **l’authentification Windows** modèle d’application.

Exécuter le [dotnet nouvelle](/dotnet/core/tools/dotnet-new) commande avec le `webapp` argument (application de Web ASP.NET Core) et `--auth Windows` commutateur :

```console
dotnet new webapp --auth Windows
```

---

Lorsque vous modifiez un projet existant, vérifiez que le fichier projet contient une référence de package pour le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app) **ou** le [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) package NuGet.

## <a name="enable-windows-authentication-with-iis"></a>Activer l’authentification Windows avec IIS

IIS utilise le [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) pour héberger des applications ASP.NET Core. L’authentification Windows est configurée pour IIS via le *web.config* fichier. Les sections suivantes montrent comment :

* Fournir une variable locale *web.config* fichier qui active l’authentification Windows sur le serveur quand l’application est déployée.
* Utilisez le Gestionnaire des services Internet pour configurer le *web.config* fichier d’une application ASP.NET Core qui a déjà été déployée sur le serveur.

### <a name="iis-configuration"></a>Configuration d’IIS

Si vous n’avez pas déjà fait, activez IIS pour héberger des applications ASP.NET Core. Pour plus d'informations, consultez <xref:host-and-deploy/iis/index>.

Activer le Service de rôle IIS pour l’authentification Windows. Pour plus d’informations, consultez [activer l’authentification Windows dans les Services de rôle IIS (voir étape 2)](xref:host-and-deploy/iis/index#iis-configuration).

Intergiciel d’intégration IIS est configuré pour authentifier les demandes automatiquement par défaut. Pour plus d’informations, consultez [héberger ASP.NET Core sur Windows avec IIS : Options d’IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).

Le Module ASP.NET Core est configuré pour transférer le jeton d’authentification de Windows à l’application par défaut. Pour plus d’informations, consultez [référence de configuration de Module ASP.NET Core : Attributs de l’élément aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

### <a name="create-a-new-iis-site"></a>Créer un nouveau site IIS

Spécifiez un nom et le dossier et lui permettre de créer un nouveau pool d’applications.

### <a name="enable-windows-authentication-for-the-app-in-iis"></a>Activer l’authentification Windows pour l’application dans IIS

Utilisez **soit** des approches suivantes :

* [Configuration de développement côté avant de publier l’application](#development-side-configuration-with-a-local-webconfig-file) (*recommandé*)
* [Configuration côté serveur après la publication de l’application](#server-side-configuration-with-the-iis-manager)

#### <a name="development-side-configuration-with-a-local-webconfig-file"></a>Configuration de développement côté avec un fichier web.config local

Procédez comme suit **avant** vous [publier et déployer votre projet](#publish-and-deploy-your-project-to-the-iis-site-folder).

Ajoutez le code suivant *web.config* fichier à la racine du projet :

[!code-xml[](windowsauth/sample_snapshot/web_2.config)]

Lorsque le projet est publié par le Kit de développement logiciel (sans le `<IsTransformWebConfigDisabled>` propriété définie sur `true` dans le fichier projet), publié *web.config* fichier inclut le `<location><system.webServer><security><authentication>` section. Pour plus d’informations sur la `<IsTransformWebConfigDisabled>` propriété, consultez <xref:host-and-deploy/iis/index#webconfig-file>.

#### <a name="server-side-configuration-with-the-iis-manager"></a>Configuration côté serveur avec le Gestionnaire des services Internet

Procédez comme suit **après** vous [publier et déployer votre projet](#publish-and-deploy-your-project-to-the-iis-site-folder).

1. Dans le Gestionnaire des services Internet, sélectionnez le site IIS sous la **Sites** nœud de la **connexions** encadré.
1. Double-cliquez sur **authentification** dans le **IIS** zone.
1. Sélectionnez **l’authentification anonyme**. Sélectionnez **désactiver** dans le **Actions** encadré.
1. Sélectionnez **l’authentification Windows**. Sélectionnez **activer** dans le **Actions** encadré.

Lorsque ces actions sont effectuées, le Gestionnaire des services Internet modifie l’application *web.config* fichier. Un `<system.webServer><security><authentication>` nœud est ajouté avec les paramètres mis à jour pour `anonymousAuthentication` et `windowsAuthentication`:

[!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

Le `<system.webServer>` section ajoutée à la *web.config* fichier par le Gestionnaire des services Internet se trouve en dehors de l’application `<location>` section ajoutée par le SDK .NET Core lors de l’application est publiée. Étant donné que la section est ajoutée à l’extérieur de la `<location>` nœud, les paramètres sont hérités par les [sous-applications](xref:host-and-deploy/iis/index#sub-applications) à l’application actuelle. Pour empêcher l’héritage, déplacer la `<security>` section à l’intérieur de la `<location><system.webServer>` section du Kit de développement SDK.

Lorsque le Gestionnaire des services Internet est utilisé pour ajouter la configuration d’IIS, il affecte uniquement l’application *web.config* fichier sur le serveur. Un autre déploiement de l’application peut remplacer les paramètres sur le serveur si la copie du serveur de *web.config* est remplacé par le projet *web.config* fichier. Utilisez **soit** des approches suivantes pour gérer les paramètres :

* Utilisez le Gestionnaire IIS pour réinitialiser les paramètres dans le *web.config* une fois que le fichier est remplacé sur le déploiement de fichiers.
* Ajouter un *fichier web.config* à l’application localement avec les paramètres. Pour plus d’informations, consultez le [configuration développement côté](#development-side-configuration-with-a-local-webconfig-file) section.

### <a name="publish-and-deploy-your-project-to-the-iis-site-folder"></a>Publier et déployer votre projet dans le dossier de site IIS

À l’aide de Visual Studio ou l’interface CLI .NET Core, publier et déployer l’application dans le dossier de destination.

Pour plus d’informations sur l’hébergement avec IIS, la publication et déploiement, consultez les rubriques suivantes :

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>

Lancez l’application pour vérifier que l’utilisation de l’authentification Windows.

## <a name="enable-windows-authentication-with-httpsys"></a>Activer l’authentification Windows avec HTTP.sys

Bien que Kestrel ne prend pas en charge l’authentification Windows, vous pouvez utiliser [HTTP.sys](xref:fundamentals/servers/httpsys) pour prendre en charge les scénarios auto-hébergés sur Windows. L’exemple suivant configure un hôte web de l’application pour utiliser HTTP.sys avec l’authentification Windows :

[!code-csharp[](windowsauth/sample_snapshot/Program.cs?highlight=9-14)]

> [!NOTE]
> HTTP.sys délègue l’authentification en mode noyau avec le protocole d’authentification Kerberos. L’authentification en mode utilisateur n’est pas prise en charge avec Kerberos et HTTP.sys. Le compte d’ordinateur doit être utilisé pour déchiffrer le ticket/jeton Kerberos obtenu à partir d’Active Directory et transféré par le client au serveur afin d’authentifier l’utilisateur. Inscrivez le nom de principal du service (SPN) pour l’hôte, et non l’utilisateur de l’application.

> [!NOTE]
> HTTP.sys n’est pas pris en charge sur Nano Server version 1709 ou ultérieure. Pour utiliser l’authentification Windows et HTTP.sys avec Nano Server, utilisez un [conteneur de Server Core (microsoft/windowsservercore)](https://hub.docker.com/r/microsoft/windowsservercore/). Pour plus d’informations sur Server Core, consultez [quel est l’option d’installation Server Core de Windows Server ?](/windows-server/administration/server-core/what-is-server-core).

## <a name="work-with-windows-authentication"></a>Utiliser l’authentification Windows

L’état de configuration de l’accès anonyme détermine la façon dont le `[Authorize]` et `[AllowAnonymous]` attributs sont utilisés dans l’application. Les deux sections suivantes expliquent comment gérer les États de configuration non autorisés et autorisées de l’accès anonyme.

### <a name="disallow-anonymous-access"></a>Interdire l’accès anonyme

Lorsque l’authentification Windows est activée et que l’accès anonyme est désactivé, le `[Authorize]` et `[AllowAnonymous]` attributs n’ont aucun effet. Si le site IIS (ou HTTP.sys) est configuré pour interdire l’accès anonyme, la demande n’atteint jamais votre application. Pour cette raison, le `[AllowAnonymous]` attribut n’est pas applicable.

### <a name="allow-anonymous-access"></a>Autoriser l’accès anonyme

Lorsque l’authentification Windows et l’accès anonyme sont activées, utiliser le `[Authorize]` et `[AllowAnonymous]` attributs. Le `[Authorize]` attribut vous permet de sécuriser les éléments de l’application qui véritablement nécessitent l’authentification Windows. Le `[AllowAnonymous]` substitutions d’attribut `[Authorize]` attribut l’utilisation dans les applications qui permettent l’accès anonyme. Consultez [autorisation Simple](xref:security/authorization/simple) pour des détails d’utilisation de l’attribut.

Dans ASP.NET Core 2.x, le `[Authorize]` attribut nécessite une configuration supplémentaire dans *Startup.cs* en question les demandes anonymes pour l’authentification Windows. La configuration recommandée varie légèrement selon le serveur web utilisé.

> [!NOTE]
> Par défaut, les utilisateurs qui ne disposent pas d’autorisation pour accéder à une page sont présentées avec une réponse HTTP 403 vide. Le [StatusCodePages intergiciel (middleware)](xref:fundamentals/error-handling#usestatuscodepages) peut être configuré pour fournir aux utilisateurs une meilleure expérience « Accès refusé ».

#### <a name="iis"></a>IIS

Si vous utilisez IIS, ajoutez le code suivant à la `ConfigureServices` méthode :

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a>HTTP.sys

Si vous utilisez HTTP.sys, ajoutez le code suivant à la `ConfigureServices` méthode :

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a>Emprunt d'identité

ASP.NET Core n’implémente pas l’emprunt d’identité. Applications s’exécutent avec l’identité d’application pour toutes les demandes, à l’aide d’identité de pool ou les processus de l’application. Si vous avez besoin effectuer explicitement une action de la part d’un utilisateur, utilisez [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) dans un [intergiciel (middleware) inline terminal](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) dans `Startup.Configure`. Exécuter une action unique dans ce contexte, puis fermez le contexte.

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

`RunImpersonated` ne prend pas en charge les opérations asynchrones et ne doit pas être utilisé pour les scénarios complexes. Par exemple, encapsulant les demandes entières ou des chaînes d’intergiciel (middleware) n’est pas pris en charge ni recommandé.

### <a name="claims-transformations"></a>Transformations de revendications

Lorsque vous hébergez avec mode in-process IIS, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> n’est pas appelée en interne pour initialiser un utilisateur. Par conséquent, une implémentation de <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> utilisée pour transformer les revendications après chaque authentification n’est pas activée par défaut. Pour plus d’informations et un exemple de code qui active les transformations de revendications lors de l’hébergement intra-processus, consultez <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.
