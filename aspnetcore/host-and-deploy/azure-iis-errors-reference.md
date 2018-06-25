---
title: Informations de référence sur les erreurs courantes pour Azure App Service et IIS avec ASP.NET Core
author: guardrex
description: Repérer les erreurs courantes liées à l’hébergement d’applications ASP.NET Core sur Azure Apps Service et IIS.
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2017
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 30b7f1d8e1cfdfd3d1db865ff428eb2094a84d32
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277317"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a>Informations de référence sur les erreurs courantes pour Azure App Service et IIS avec ASP.NET Core

Par [Luke Latham](https://github.com/guardrex)

Cette liste d’erreurs n’est pas exhaustive. Si vous rencontrez une erreur non répertoriée ici, [ouvrez un nouveau problème](https://github.com/aspnet/Docs/issues/new) en indiquant comment reproduire l’erreur.

Collectez les informations suivantes :

* Comportement de l’explorateur
* Entrées du journal des événements de l’application
* Entrées du journal stdout du module ASP.NET Core

Comparez les informations aux erreurs courantes suivantes. Si vous trouvez une correspondance, suivez les conseils de dépannage.

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="installer-unable-to-obtain-vc-redistributable"></a>Le programme d’installation ne parvient pas à obtenir VC++ Redistributable

* **Exception de programme d’installation :** 0x80072efd ou 0x80072f76 - Erreur non spécifiée

* **Exception du journal d’installation&#8224;:** Erreur 0x80072efd ou 0x80072f76 : Impossible d’exécuter le package EXE

  &#8224;Le journal se trouve dans C:\Users\\{USER}\AppData\Local\Temp\dd_DotNetCoreWinSvrHosting__{timestamp}.log.

Résolution des problèmes :

* Si le système n’a pas accès à Internet au moment de l’installation du bundle d’hébergement, cette exception se produit quand le programme d’installation ne peut pas obtenir *Microsoft Visual C++ 2015 Redistributable*. Obtenez un programme d’installation à partir du [Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=53840). Si le programme d’installation échoue, le serveur risque de ne pas recevoir le runtime .NET Core nécessaire pour héberger un déploiement dépendant du framework. En cas d’hébergement d’un déploiement dépendant du framework, vérifiez que le runtime est installé dans Programmes &amp; Fonctionnalités. Le cas échéant, obtenez un programme d’installation du runtime à partir de [.NET All Downloads](https://www.microsoft.com/net/download/all) (Tous les téléchargements .NET). Après avoir installé le runtime, redémarrez le système ou IIS en exécutant **net stop was /y** suivi de **net start w3svc** à partir d’une invite de commandes.

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a>La mise à niveau du système d’exploitation a supprimé le Module ASP.NET Core 32 bits

* **Journal des applications :** Le fichier DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** du module n’a pas pu se charger. Les données sont erronées.

Résolution des problèmes :

* Les fichiers autres que les fichiers de système d’exploitation dans le répertoire **C:\Windows\SysWOW64\inetsrv** ne sont pas conservés pendant la mise à niveau du système d’exploitation. Si le module ASP.NET Core est installé avant la mise à niveau du système d’exploitation et qu’un pool d’applications en mode 32 bits est ensuite exécuté après une mise à niveau du système d’exploitation, ce problème se produit. Après une mise à niveau du système d’exploitation, réparez le Module ASP.NET Core. Consultez [Installer le bundle d’hébergement .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle). Sélectionnez **Réparer** quand le programme d’installation est exécuté.

## <a name="platform-conflicts-with-rid"></a>Conflits de plateforme avec RID

* **Navigateur :** Erreur HTTP 502.5 - Échec du processus

* **Journal des applications :** L’application « MACHINE/WEBROOT/APPHOST/{ASSEMBLY} » avec la racine physique 'C:\{PATH}\' n’a pas pu démarrer le processus avec la ligne de commande '« C:\\{PATH}{assembly}.{exe|dll} » ', Code d’erreur = 0x80004005 : ff.

* **Journal du module ASP.NET Core :** Exception non gérée : System.BadImageFormatException : Impossible de charger le fichier ou l’assembly « {assembly}.dll ». Tentative de chargement d’un programme au format incorrect.

Résolution des problèmes :

* Vérifiez que l’application s’exécute localement sur Kestrel. Un échec de processus peut être dû à un problème au niveau de l’application. Pour plus d’informations, consultez [Dépannage](xref:host-and-deploy/iis/troubleshoot).

* Vérifiez que le `<PlatformTarget>` dans le *.csproj* n’entre pas en conflit avec le RID. Par exemple, ne spécifiez pas `x86` comme `<PlatformTarget>` et en publiant avec un RID `win10-x64`, à l’aide de *dotnet publish -c Release -r win10-x64* ou en affectant la valeur `win10-x64` à `<RuntimeIdentifiers>` dans le *.csproj*. Le projet est publié sans avertissement ni erreur, mais il échoue avec les exceptions journalisées ci-dessus sur le système.

* Si cette exception se produit pour un déploiement d’applications Azure pendant la mise à niveau d’une application et le déploiement de nouveaux assemblys, supprimez manuellement tous les fichiers du déploiement précédent. Le fait de laisser des assemblys incompatibles peut provoquer une exception `System.BadImageFormatException` lors du déploiement d’une application mise à niveau.

## <a name="uri-endpoint-wrong-or-stopped-website"></a>Point de terminaison d’URI incorrect ou site web arrêté

* **Navigateur :** ERR_CONNECTION_REFUSED

* **Journal des applications :** Aucune entrée

* **Journal du Module ASP.NET Core :** Fichier journal non créé

Résolution des problèmes :

* Vérifiez que le point de terminaison d’URI correct est utilisé pour l’application. Vérifiez les liaisons.

* Vérifiez que le site web IIS n’est pas à l’état *Arrêté*.

## <a name="corewebengine-or-w3svc-server-features-disabled"></a>Fonctionnalités de serveur W3SVC ou CoreWebEngine désactivées

* **Exception de système d’exploitation :** Les fonctionnalités CoreWebEngine et W3SVC d’IIS 7.0 doivent être installées pour utiliser le Module ASP.NET Core.

Résolution des problèmes :

* Vérifiez que le rôle et les fonctionnalités appropriés sont activés. Consultez [Configuration d’IIS](xref:host-and-deploy/iis/index#iis-configuration).

## <a name="incorrect-website-physical-path-or-app-missing"></a>Chemin physique de site web incorrect ou application manquante

* **Navigateur :** 403 - Interdit : accès refusé **--ou--** 403.14 - Interdit : Le serveur Web est configuré pour ne pas afficher le contenu de ce répertoire.

* **Journal des applications :** Aucune entrée

* **Journal du Module ASP.NET Core :** Fichier journal non créé

Résolution des problèmes :

* Consultez les **Paramètres de base** du site web IIS et le dossier d’application physique. Vérifiez que l’application est dans le dossier sur le **chemin physique** du site web IIS.

## <a name="incorrect-role-module-not-installed-or-incorrect-permissions"></a>Rôle incorrect, module non installé ou autorisations incorrectes

* **Navigateur :** 500.19 Erreur interne du serveur : Impossible d’accéder à la page que vous avez demandée, car les données de configuration connexes relatives à la page ne sont pas valides.

* **Journal des applications :** Aucune entrée

* **Journal du Module ASP.NET Core :** Fichier journal non créé

Résolution des problèmes :

* Vérifiez que le rôle approprié est activé. Consultez [Configuration d’IIS](xref:host-and-deploy/iis/index#iis-configuration).

* Accédez à **Programmes &amp; Fonctionnalités** et vérifiez que le **Module Microsoft ASP.NET Core** a été installé. Si le **Module Microsoft ASP.NET Core** n’est pas présent dans la liste des programmes installés, installez-le. Consultez [Installer le bundle d’hébergement .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).

* Vérifiez que **Pool d’applications** > **Modèle de processus** > **Identité** a la valeur **ApplicationPoolIdentity** ou que l’identité personnalisée dispose des autorisations appropriées pour accéder au dossier de déploiement de l’application.

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a>processPath incorrect, variable de chemin manquante, bundle d’hébergement non installé, système/IIS non redémarré, VC++ Redistributable non installé ou violation d’accès dotnet.exe

* **Navigateur :** Erreur HTTP 502.5 - Échec du processus

* **Journal des applications :** L’application « MACHINE/WEBROOT/APPHOST/{ASSEMBLY} » avec la racine physique 'C:\\{PATH}\' n’a pas pu démarrer le processus avec la ligne de commande '« .\{assembly}.exe » ', Code d’erreur = 0x80070002 : 0.

* **Journal du Module ASP.NET Core :** Fichier journal créé mais vide

Résolution des problèmes :

* Vérifiez que l’application s’exécute localement sur Kestrel. Un échec de processus peut être dû à un problème au niveau de l’application. Pour plus d’informations, consultez [Dépannage](xref:host-and-deploy/iis/troubleshoot).

* Vérifiez l’attribut *processPath* de l’élément `<aspNetCore>` dans *web.config* pour confirmer qu’il s’agit de *dotnet* pour un déploiement dépendant du framework ou de *.\{assembly}.exe* pour un déploiement autonome.

* Pour un déploiement dépendant du framework, *dotnet.exe* peut ne pas être accessible via les paramètres PATH. Vérifiez que *C:\Program Files\dotnet\* existe dans les paramètres PATH du système.

* Pour un déploiement dépendant du framework, *dotnet.exe* peut ne pas être accessible pour l’identité d’utilisateur du pool d’applications. Vérifiez que l’identité utilisateur du pool d’applications a accès au répertoire *C:\Program Files\dotnet*. Vérifiez qu’aucune règle de refus d’accès n’est configurée pour l’identité utilisateur du pool d’applications sur les répertoires *C:\Program Files\dotnet* et d’application.

* Peut-être qu’un déploiement dépendant du framework a été déployé et que .NET Core a été installé sans redémarrage d’IIS. Redémarrez le serveur ou IIS en exécutant **net stop was /y** suivi de **net start w3svc** à partir d’une invite de commandes.

* Peut-être qu’un déploiement dépendant du framework a été déployé sans que le runtime .NET Core soit installé sur le système hôte. Si le runtime .NET Core n’a pas été installé, exécutez le **programme d’installation du bundle d’hébergement .NET Core** sur le système. Consultez [Installer le bundle d’hébergement .NET Core](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle). Si vous essayez d’installer le runtime .NET Core sur un système sans connexion Internet, obtenez le runtime à partir de [.NET All Downloads](https://www.microsoft.com/net/download/all) (Tous les téléchargements .NET) et exécutez le programme d’installation du bundle d’hébergement pour installer le module ASP.NET Core. Terminez l’installation en redémarrant le système ou IIS en exécutant **net stop was /y** suivi de **net start w3svc** à partir d’une invite de commandes.

* Un déploiement dépendant du framework a peut-être été déployé et *Microsoft Visual C++ 2015 Redistributable (x64)* n’est pas installé sur le système. Obtenez un programme d’installation à partir du [Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=53840).

## <a name="incorrect-arguments-of-aspnetcore-element"></a>Arguments incorrects de l’élément \<aspNetCore\>

* **Navigateur :** Erreur HTTP 502.5 - Échec du processus

* **Journal des applications :** L’application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' avec la racine physique 'C:\\{PATH}\' n’a pas pu démarrer le processus avec la ligne de commande' « dotnet » .\{assembly}.dll» ', Code d’erreur = '0x80004005 : 80008081.

* **Journal du module ASP.NET Core :** L’application à exécuter n’existe pas : 'PATH\{assembly}.dll'

Résolution des problèmes :

* Vérifiez que l’application s’exécute localement sur Kestrel. Un échec de processus peut être dû à un problème au niveau de l’application. Pour plus d’informations, consultez [Dépannage](xref:host-and-deploy/iis/troubleshoot).

* Examinez l’attribut *arguments* de l’élément `<aspNetCore>` dans *web.config* pour confirmer (a) qu’il s’agit de *.\{assembly}.dll* pour un déploiement dépendant du framework, ou (b) qu’il est absent ou qu’il s’agit d’une chaîne vide (*arguments=""*) ou d’une liste d’arguments de l’application (*arguments="arg1, arg2, ..."*) pour un déploiement autonome.

## <a name="missing-net-framework-version"></a>Version du .NET Framework manquante

* **Navigateur :** 502.3 Passerelle incorrecte : Une erreur de connexion s’est produite lors de la tentative de routage de la requête.

* **Journal des applications :** Code d’erreur = L’application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' avec la racine physique 'C:\\{PATH}\' n’a pas pu démarrer le processus avec la ligne de commande' « dotnet » .\{assembly}.dll» ', Code d’erreur = '0x80004005 : 80008081.

* **Journal du Module ASP.NET Core :** Exception d’assembly, de méthode ou de fichier manquante. La méthode, le fichier ou l’assembly spécifié dans l’exception est une méthode, un fichier ou un assembly .NET Framework.

Résolution des problèmes :

* Installez la version de .NET Framework manquante sur le système.

* Pour un déploiement dépendant du framework, vérifiez que le runtime approprié est installé sur le système. Si le projet est mis à niveau depuis 1.1 vers 2.0, qu’il est déployé sur le système hôte et que cette exception est levée, vérifiez que le framework 2.0 est sur le système hôte.

## <a name="stopped-application-pool"></a>Pool d’applications arrêté

* **Navigateur :** 503 Service non disponible

* **Journal des applications :** Aucune entrée

* **Journal du Module ASP.NET Core :** Fichier journal non créé

Résolution des problèmes

* Vérifiez que le pool d’applications n’est pas à l’état *Arrêté*.

## <a name="iis-integration-middleware-not-implemented"></a>L’intergiciel d’intégration IIS n’est pas implémenté

* **Navigateur :** Erreur HTTP 502.5 - Échec du processus

* **Journal des applications :** L’application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' avec racine physique 'C:\\{PATH}\' a créé des processus avec la ligne de commande '« C:\\{PATH}\{assembly}.{exe|dll} » ' mais s’est bloquée ou n’a pas répondu ou écouté sur le port spécifié '{PORT}', Code d’erreur = '0x800705b4'

* **Journal du Module ASP.NET Core :** Fichier journal créé et indiquant un fonctionnement normal.

Résolution des problèmes

* Vérifiez que l’application s’exécute localement sur Kestrel. Un échec de processus peut être dû à un problème au niveau de l’application. Pour plus d’informations, consultez [Dépannage](xref:host-and-deploy/iis/troubleshoot).

* Vérifiez l’un des points suivants :
  * Le middleware (intergiciel) d’intégration IIS est référencé par l’appel de la méthode `UseIISIntegration` sur le `WebHostBuilder` de l’application (ASP.NET Core 1.x)
  * L’application utilise la méthode `CreateDefaultBuilder` (ASP.NET Core 2.x).
  
  Consultez [Héberger dans ASP.NET Core](xref:fundamentals/host/index) pour plus d’informations.

## <a name="sub-application-includes-a-handlers-section"></a>La sous-application inclut une section \<handlers\>

* **Navigateur :** Erreur HTTP 500.19 : Erreur interne du serveur

* **Journal des applications :** Aucune entrée

* **Journal du module ASP.NET Core :** Fichier journal créé et indiquant un fonctionnement normal pour l’application racine. Fichier journal non créé pour la sous-application.

Résolution des problèmes

* Vérifiez que le fichier *web.config* de la sous-application n’inclut pas de section `<handlers>`.

## <a name="stdout-log-path-incorrect"></a>Chemin du journal stdout incorrect

* **Navigateur :** l’application répond normalement.

* **Journal des applications :** Avertissement : Impossible de créer le fichier journal stdout \\?\C:\_apps\app_folder\bin\Release\netcoreapp2.0\win10-x64\publish\logs\chemin_inexistant\stdout_8748_201831835937.log, Code d’erreur = -2147024893.

* **Journal du Module ASP.NET Core :** Fichier journal non créé

Résolution des problèmes

* Le chemin `stdoutLogFile` spécifié dans l’élément `<aspNetCore>` de *web.config* n’existe pas. Pour plus d’informations, consultez la section [Création et redirection des journaux](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) de la rubrique des informations de référence sur la configuration du module ASP.NET Core.

## <a name="application-configuration-general-issue"></a>Problème général lié à la configuration d’application

* **Navigateur :** Erreur HTTP 502.5 - Échec du processus

* **Journal des applications :** L’application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' avec racine physique 'C:\\{PATH}\' a créé des processus avec la ligne de commande '« C:\\{PATH}\{assembly}.{exe|dll} » ' mais s’est bloquée ou n’a pas répondu ou écouté sur le port spécifié '{PORT}', Code d’erreur = '0x800705b4'

* **Journal du Module ASP.NET Core :** Fichier journal créé mais vide

Résolution des problèmes

* Cette exception générale indique que le démarrage du processus a échoué, probablement en raison d’un problème de configuration d’application. En vous référant à [Structure de répertoires](xref:host-and-deploy/directory-structure), vérifiez que les fichiers et dossiers déployés de l’application sont corrects et que les fichiers de configuration de l’application sont présents et contiennent les paramètres appropriés pour l’application et l’environnement. Pour plus d’informations, consultez [Dépannage](xref:host-and-deploy/iis/troubleshoot).
