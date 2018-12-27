---
title: Configurer l’authentification Windows dans ASP.NET Core
author: scottaddie
description: Découvrez comment configurer l’authentification Windows dans ASP.NET Core, à l’aide d’IIS Express, IIS et HTTP.sys.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 12/18/2018
uid: security/authentication/windowsauth
ms.openlocfilehash: 94dff2f47b2b076cb15f8d385239179b52786678
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637818"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>Configurer l’authentification Windows dans ASP.NET Core

Par [Steve Smith](https://ardalis.com) et [Scott Addie](https://twitter.com/Scott_Addie)

L’authentification Windows peut être configurée pour les applications ASP.NET Core hébergées avec IIS ou [HTTP.sys](xref:fundamentals/servers/httpsys).

## <a name="windows-authentication"></a>Authentification Windows

L’authentification Windows s’appuie sur le système d’exploitation pour authentifier les utilisateurs des applications ASP.NET Core. Vous pouvez utiliser l’authentification Windows lorsque votre serveur s’exécute sur un réseau d’entreprise à l’aide d’identités de domaine Active Directory ou d’autres comptes Windows pour identifier les utilisateurs. L’authentification Windows est mieux adaptée aux environnements intranet dans lequel les utilisateurs, les applications clientes et les serveurs web appartiennent au même domaine Windows.

[En savoir plus sur l’authentification Windows et l’installation pour IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>Activer l’authentification Windows dans une application ASP.NET Core

Le modèle d’Application Web de Visual Studio peut être configuré pour prendre en charge l’authentification Windows.

### <a name="use-the-windows-authentication-app-template"></a>Utiliser le modèle d’application de l’authentification Windows

Dans Visual Studio :

1. Créez une application web ASP.NET Core.
1. Sélectionnez l’Application Web à partir de la liste des modèles.
1. Sélectionnez le **modifier l’authentification** bouton et sélectionnez **l’authentification Windows**.

Exécuter l’application. Le nom d’utilisateur s’affiche dans le coin supérieur droit de l’application.

![Capture d’écran de navigateur de l’authentification Windows](windowsauth/_static/browser-screenshot.png)

Pour le travail de développement à l’aide d’IIS Express, le modèle fournit toute la configuration nécessaire pour utiliser l’authentification Windows. La section suivante montre comment configurer manuellement une application ASP.NET Core pour l’authentification Windows.

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>Paramètres de Visual Studio pour Windows et l’authentification anonyme

Le projet Visual Studio **propriétés** de page **déboguer** onglet fournit des cases à cocher pour l’authentification Windows et l’authentification anonyme.

![Capture d’écran du navigateur de l’authentification Windows avec les options d’authentification mis en surbrillance](windowsauth/_static/vs-auth-property-menu.png)

Vous pouvez également ces deux propriétés peuvent être configurées dans le *launchSettings.json* fichier :

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a>Activer l’authentification Windows avec IIS

IIS utilise le [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) pour héberger des applications ASP.NET Core. L’authentification Windows est configurée dans IIS, pas l’application. Les sections suivantes vous montrent comment utiliser le Gestionnaire des services Internet pour configurer une application ASP.NET Core pour utiliser l’authentification Windows.

### <a name="iis-configuration"></a>Configuration d’IIS

Activer le Service de rôle IIS pour l’authentification Windows. Pour plus d’informations, consultez [activer l’authentification Windows dans les Services de rôle IIS (voir étape 2)](xref:host-and-deploy/iis/index#iis-configuration).

Intergiciel d’intégration IIS est configuré pour authentifier les demandes automatiquement par défaut. Pour plus d’informations, consultez [héberger ASP.NET Core sur Windows avec IIS : Options d’IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).

Le Module ASP.NET Core est configuré pour transférer le jeton d’authentification de Windows à l’application par défaut. Pour plus d’informations, consultez [référence de configuration de Module ASP.NET Core : Attributs de l’élément aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

### <a name="create-a-new-iis-site"></a>Créer un nouveau site IIS

Spécifiez un nom et le dossier et lui permettre de créer un nouveau pool d’applications.

### <a name="customize-authentication"></a>Personnaliser l’authentification

Ouvrez les fonctionnalités d’authentification pour le site.

![Menu de l’authentification IIS](windowsauth/_static/iis-authentication-menu.png)

Désactivez l’authentification anonyme et activer l’authentification Windows.

![Paramètres d’authentification IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>Publier votre projet dans le dossier de site IIS

À l’aide de Visual Studio ou l’interface CLI .NET Core, de publier l’application dans le dossier de destination.

![Boîte de dialogue de publication de Visual Studio](windowsauth/_static/vs-publish-app.png)

En savoir plus sur [publication sur IIS](xref:host-and-deploy/iis/index).

Lancez l’application pour vérifier que l’utilisation de l’authentification Windows.

## <a name="enable-windows-authentication-with-httpsys"></a>Activer l’authentification Windows avec HTTP.sys

Bien que Kestrel ne prend pas en charge l’authentification Windows, vous pouvez utiliser [HTTP.sys](xref:fundamentals/servers/httpsys) pour prendre en charge les scénarios auto-hébergés sur Windows. L’exemple suivant configure un hôte web de l’application pour utiliser HTTP.sys avec l’authentification Windows :

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

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
> Par défaut, les utilisateurs qui ne disposent pas d’autorisation pour accéder à une page sont présentées avec une réponse HTTP 403 vide. Le [StatusCodePages intergiciel (middleware)](xref:fundamentals/error-handling#configure-status-code-pages) peut être configuré pour fournir aux utilisateurs une meilleure expérience « Accès refusé ».

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

ASP.NET Core n’implémente pas l’emprunt d’identité. Applications s’exécutent avec l’identité de l’application pour toutes les demandes, à l’aide d’identité de pool ou les processus de l’application. Si vous avez besoin effectuer explicitement une action de la part d’un utilisateur, utilisez `WindowsIdentity.RunImpersonated`. Exécuter une action unique dans ce contexte, puis fermez le contexte.

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

Notez que `RunImpersonated` ne prend pas en charge les opérations asynchrones et ne doit pas être utilisé pour les scénarios complexes. Par exemple, encapsulant les demandes entières ou des chaînes d’intergiciel (middleware) n’est pas pris en charge ni recommandé.
