---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
title: 'Déploiement d’une Application de Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : résolution des problèmes (partie 12 sur 12) | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer une ASP.NET (publier) projet d’application web qui inclut une base de données SQL Server Compact à l’aide de Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 3fc23eed-921d-4d46-a610-a2d156e4bd03
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
msc.type: authoredcontent
ms.openlocfilehash: bc3a412820638b347d2e5781c01481f622dd2033
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376347"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-troubleshooting-12-of-12"></a>Déploiement d’une Application de Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : résolution des problèmes (partie 12 sur 12)
====================
par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Cette série de didacticiels vous montre comment déployer une ASP.NET (publier) projet d’application web qui inclut une base de données SQL Server Compact à l’aide de Visual Studio 2012 RC ou Visual Studio Express 2012 RC pour le Web. Vous pouvez également utiliser Visual Studio 2010 si vous installez la mise à jour de publication Web. Pour une introduction à la série, consultez [le premier didacticiel de la série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Pour obtenir un didacticiel qui montre les fonctionnalités de déploiement introduites après la version RC de Visual Studio 2012, montre comment déployer des éditions de SQL Server autre que SQL Server Compact et montre comment déployer sur les Sites Web Windows Azure, consultez [déploiement Web ASP.NET à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


Cette page décrit certains problèmes courants qui peuvent survenir lorsque vous déployez une application web ASP.NET à l’aide de Visual Studio. Pour chacune d’elles, une ou plusieurs causes possibles et les solutions correspondantes sont fournies.

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Erreur de serveur dans l’Application - '/' paramètres d’erreur personnalisés actuels empêchent les détails de l’erreur de s’afficher à distance

### <a name="scenario"></a>Scénario

Après avoir déployé un site à un hôte distant, vous obtenez un message d’erreur qui mentionne le paramètre customErrors dans le fichier Web.config, mais n’indique pas a été la cause réelle de l’erreur :

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample1.cmd)]

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Par défaut, ASP.NET affiche des informations d’erreur détaillées uniquement lorsque votre application web est en cours d’exécution sur l’ordinateur local. En général vous ne souhaitez pas afficher les informations d’erreur détaillées lorsque votre application web est disponible publiquement sur Internet, étant donné que les pirates peuvent être en mesure d’utiliser ces informations pour rechercher les vulnérabilités dans l’application. Toutefois, lorsque vous déployez un site ou les mises à jour à un site, parfois quelque chose pose problème et vous devez obtenir le message d’erreur.

Pour activer l’application afficher les messages d’erreur détaillés quand il s’exécute sur l’hôte distant, modifiez le fichier Web.config pour définir `customErrors` mode désactivé, redéployez l’application et réexécuter l’application :

1. Si le fichier Web.config de l’application a un `customErrors` élément dans le `system.web` élément, la modification la `mode` attribut « désactivé ». Sinon, utilisez un `customErrors` élément dans le `system.web` élément avec la `mode` attribut défini sur « désactivé », comme indiqué dans l’exemple suivant :

    [!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample2.xml?highlight=3)]
2. Déployez l’application.
3. Exécutez l’application, puis répétez ce que vous l’avez fait précédemment qui a provoqué l’erreur se produise. Vous pouvez maintenant voir ce qui est le message d’erreur.
4. Lorsque vous avez résolu l’erreur, restaurez la version d’origine `customErrors` définition et de redéployer l’application.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Accès refusé dans une Page Web qu’utilise SQL Server Compact

### <a name="scenario"></a>Scénario

Lorsque vous déployez un site qui utilise SQL Server Compact et vous exécutez une page dans le site déployé qui accède à la base de données, vous consultez le message d’erreur suivant :

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Le compte SERVICE réseau sur le serveur doit être en mesure de lire des fichiers binaires natifs SQL Service Compact qui se trouvent dans le *bin\amd64* ou *bin\x86* dossier, mais il ne pas disposer des autorisations de lecture pour ces dossiers. Jeu d’accès en lecture pour le SERVICE réseau sur le *bin* dossier, en veillant à étendre les autorisations de sous-dossiers.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Impossible de lire le fichier de Configuration en raison d’autorisations insuffisantes

### <a name="scenario"></a>Scénario

Lorsque vous cliquez sur la publication Visual Studio bouton pour déployer une application à IIS sur votre ordinateur local, publication échoue et le **sortie** fenêtre affiche un message d’erreur similaire à ceci :

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample4.cmd)]

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Pour utiliser un seul clic publier sur IIS sur votre ordinateur local, vous devez exécuter Visual Studio avec des autorisations d’administrateur. Fermez Visual Studio et redémarrez-le avec des autorisations d’administrateur.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>N’a pas pu se connecter à l’ordinateur de Destination... À l’aide du processus spécifié

### <a name="scenario"></a>Scénario

Lorsque vous cliquez sur la publication Visual Studio bouton pour déployer une application, publication échoue et le **sortie** fenêtre affiche un message d’erreur similaire à ceci :

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample5.cmd)]

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Un serveur proxy est interrompre la communication avec le serveur de destination. À partir du Panneau de configuration de Windows ou dans Internet Explorer, sélectionnez **Options Internet** et sélectionnez le **connexions** onglet. Dans le **propriétés Internet** boîte de dialogue, cliquez sur **paramètres LAN**. Dans le **les paramètres de réseau local (LAN)** boîte de dialogue, désactivez le **détecter automatiquement les paramètres** case à cocher. Puis cliquez sur le bouton Publier à nouveau.

Si le problème persiste, contactez votre administrateur système pour déterminer ce qui peut être fait avec les paramètres de proxy ou pare-feu. Le problème se produit parce que Web Deploy utilise un port non standard pour le déploiement de Service de gestion Web (8172) ; pour les autres connexions, Web Deploy utilise le port 80. Lorsque vous déployez sur un fournisseur d’hébergement tiers, vous utilisez en général le Service de gestion Web.

## <a name="default-net-40-application-pool-does-not-exist"></a>Par défaut .NET 4.0 Application Pool n’existe pas

### <a name="scenario"></a>Scénario

Lorsque vous déployez une application qui nécessite le .NET Framework 4, vous consultez le message d’erreur suivant :

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample6.cmd)]

### <a name="possible-cause-and-solution"></a>Cause possible et solution

ASP.NET 4 n’est pas installé dans IIS. Si le serveur que vous effectuez le déploiement est votre ordinateur de développement et a Visual Studio 2010 installé dessus, ASP.NET 4 est installé sur l’ordinateur, mais ne peut pas être installé dans IIS. Sur le serveur sur lequel vous effectuez le déploiement, ouvrez une invite de commandes avec élévation de privilèges et installation ASP.NET 4 dans IIS en exécutant les commandes suivantes :

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample7.cmd)]

Vous devrez peut-être également définir manuellement la version de .NET Framework du pool d’applications par défaut. Pour plus d’informations, consultez le [déploiement vers IIS comme environnement de Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) didacticiel.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Format de la chaîne d’initialisation n’est pas conforme à la spécification qui débute à l’index 0.

### <a name="scenario"></a>Scénario

Une fois que vous déployez une application à l’aide d’un clic publier, lorsque vous exécutez une page qui accède à la base de données vous recevez le message d’erreur suivant :

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample8.cmd)]

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Ouvrez le *Web.config* fichier dans le site déployé et vérifiez si les valeurs de chaîne de connexion commencent par `$(ReplacableToken_`, comme dans l’exemple suivant :

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample9.xml)]

Si les chaînes de connexion ressemblent à cet exemple, modifiez le fichier de projet et ajoutez la propriété suivante à la `PropertyGroup` élément qui est pour toutes les configurations de build :

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample10.xml)]

Puis redéployer l’application.

## <a name="http-500-internal-server-error"></a>Erreur de serveur interne 500 HTTP

### <a name="scenario"></a>Scénario

Lorsque vous exécutez le site déployé, vous verrez le message d’erreur suivant sans les informations spécifiques qui indique la cause de l’erreur :

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample11.cmd)]

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Il existe plusieurs causes de 500 erreurs, mais il est possible que si vous suivez ces didacticiels que vous placez un élément XML au mauvais endroit dans un des fichiers de transformation XML. Par exemple, vous obtiendriez cette erreur si vous placez la transformation qui insère un `<location>` élément sous `<system.web>` au lieu de directement sous `<configuration>`. Dans ce cas, la solution est pour corriger le fichier de transformation XML et de redéployer.

## <a name="http-50021-internal-server-error"></a>Erreur de serveur interne 500.21 HTTP

### <a name="scenario"></a>Scénario

Lorsque vous exécutez le site déployé, vous consultez le message d’erreur suivant :

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample12.cmd)]

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Le site, vous avez déployé cibles ASP.NET 4, mais ASP.NET 4 n’est pas inscrit dans IIS sur le serveur. Sur le serveur, ouvrez une invite de commandes avec élévation de privilèges et inscrire ASP.NET 4 en exécutant les commandes suivantes :

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample13.cmd)]

Vous devrez peut-être également définir manuellement la version de .NET Framework du pool d’applications par défaut. Pour plus d’informations, consultez le [déploiement vers IIS comme environnement de Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) didacticiel.

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>Échec de la connexion ouverture SQL Server Express de base de données dans l’application\_données

### <a name="scenario"></a>Scénario

Vous mis à jour le *Web.config* fichier de chaîne de connexion pour pointer vers une base de données SQL Server Express comme un *.mdf* de fichiers dans votre *application\_données* dossier et le premier exécution de l’application que vous voyez le message d’erreur suivant :

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample14.cmd)]

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Le nom de la *.mdf* fichier ne peut pas correspondre au nom d’une base de données SQL Server Express qui existe déjà sur votre ordinateur, même si vous avez supprimé le *.mdf* fichier de la base de données existe déjà. Modifier le nom de la *.mdf* fichier un nom qui n’a jamais été utilisé comme nom de la base de données de modification le *Web.config* fichier à utiliser le nouveau nom. Comme alternative, vous pouvez utiliser [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) à supprimer existe déjà SQL Server Express bases de données.

## <a name="model-compatibility-cannot-be-checked"></a>Compatibilité de modèle ne peut pas être vérifiée.

### <a name="scenario"></a>Scénario

Vous mis à jour le *Web.config* fichier de chaîne de connexion pour pointer vers une nouvelle base de données SQL Server Express, et la première fois que vous exécutez l’application que vous voyez le message d’erreur suivant :

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample15.cmd)]

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Si le nom de la base de données que vous placez dans le fichier Web.config a déjà été utilisé avant sur votre ordinateur, une base de données existe peut-être déjà avec des tables qu’il contient. Sélectionnez un nouveau nom n’a pas été utilisé sur votre ordinateur avant et de la modification la *Web.config* fichier pour pointer pour utiliser ce nouveau nom de base de données. Comme alternative, vous pouvez utiliser [utilitaire SQL Server Express](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) ou [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) pour supprimer la base de données existante.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>Erreur SQL lorsqu’un Script tente de créer des utilisateurs ou des rôles

### <a name="scenario"></a>Scénario

Vous utilisez un déploiement de base de données configurée sur le **Package/Publication SQL** onglet, les scripts SQL qui s’exécutent pendant le déploiement incluent les commandes Create User ou créer un rôle et Échec de l’exécution de script lorsque ces commandes sont exécutées. Vous pouvez voir plus de messages, tels que les éléments suivants :

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample16.cmd)]

Si cette erreur se produit lorsque vous avez configuré le déploiement de base de données dans le **publier le site Web** Assistant plutôt que la **Package/Publication SQL** , onglet de création d’un thread dans le [Configuration et Déploiement](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) forum et la solution seront ajouté à cette page de résolution des problèmes.

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Le compte d’utilisateur que vous utilisez pour réaliser le déploiement n’a pas l’autorisation de créer des utilisateurs ou des rôles. Par exemple, la société d’hébergement peut affecter la `db_datareader`, `db_datawriter`, et `db_ddladmin` rôles pour le compte d’utilisateur qu’elle configure pour vous. Ceux-ci sont suffisantes pour la création de la plupart des objets de base de données, mais ne pas pour la création d’utilisateurs ou des rôles. Pour éviter l’erreur est en excluant des utilisateurs et rôles de déploiement de base de données. Vous pouvez le faire en modifiant le `PreSource` élément pour la base de données généré automatiquement de script afin qu’il inclue les attributs suivants :

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample17.cmd)]

Pour plus d’informations sur la façon de modifier le `PreSource` élément dans le fichier projet, consultez [Comment : modifier les paramètres de déploiement dans le fichier projet](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Si les utilisateurs ou les rôles dans votre base de données de développement doivent se trouver dans la base de données de destination, contactez votre fournisseur d’hébergement pour obtenir une assistance.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>Erreur de délai d’expiration SQL Server lors de l’exécution des Scripts personnalisés au cours du déploiement

### <a name="scenario"></a>Scénario

Vous avez spécifié des scripts SQL personnalisés à exécuter pendant le déploiement, et lorsque le Web Deploy où ils sont exécutés, leur délai d’attente.

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Plusieurs scripts qui ont des modes différents de transaction en cours d’exécution peut provoquer des erreurs de délai d’attente. Par défaut, les scripts générés automatiquement s’exécutent dans une transaction, mais n’est pas le cas de scripts personnalisés. Si vous sélectionnez le **extraire les données et/ou le schéma à partir d’une base de données existante** option sur le **Package/Publication SQL** onglet, et si vous ajoutez un script SQL personnalisé, vous devez modifier les paramètres des transactions dans des scripts afin que tous les scripts utilisent les mêmes paramètres de transaction. Pour plus d’informations, consultez [Comment : déployer une base de données avec un projet d’Application Web](https://msdn.microsoft.com/library/dd465343.aspx).

Si vous avez configuré les paramètres des transactions afin que toutes sont identiques mais que vous obtenez encore cette erreur, une solution de contournement possible consiste à exécuter les scripts séparément. Dans le **des Scripts de base de données** grille dans le **Package/Publication** onglet SQL, désactivez le **Include** case à cocher pour le script qui provoque l’erreur de délai d’attente, puis publiez le projet. Puis accédez à la **des Scripts de base de données** grille, sélectionnez de ce script **Include** case à cocher, puis désactivez la **Include** cases à cocher pour les autres scripts. Puis publiez de nouveau le projet. Cette fois, lorsque vous publiez, uniquement les exécutions de script personnalisé sélectionné.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Données Stream du manifeste de Site ne sont pas encore disponibles

### <a name="scenario"></a>Scénario

Lorsque vous installez un package à l’aide de la *deploy.cmd* de fichiers avec la `t` option (test), vous voyez le message d’erreur suivant :

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample18.cmd)]

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Le message d’erreur signifie que la commande ne peut pas produire un rapport de test. Toutefois, la commande peut s’exécuter si vous utilisez la `y` option (installation réelle). Le message indique uniquement qu’il existe un problème avec la commande en cours d’exécution en mode test.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Cette Application nécessite ManagedRuntimeVersion v4.0

### <a name="scenario"></a>Scénario

Lorsque vous tentez de déployer, vous consultez le message d’erreur suivant :

 Erreur : Les données de flux de ' sitemanifest/dbFullSql [@path= 'C:\TEMP\AdventureWorksGrant.sql']/sqlScript' n’est pas encore disponible. Le pool d’applications que vous tentez d’utiliser a la propriété « managedRuntimeVersion » défini sur « v2.0 ». Cette application est « v4.0 ». 

### <a name="possible-cause-and-solution"></a>Cause possible et solution

ASP.NET 4 n’est pas installé dans IIS. Si le serveur que vous effectuez le déploiement est votre ordinateur de développement et a Visual Studio 2010 installé dessus, ASP.NET 4 est installé sur l’ordinateur, mais ne peut pas être installé dans IIS. Sur le serveur sur lequel vous effectuez le déploiement, ouvrez une invite de commandes avec élévation de privilèges et installation ASP.NET 4 dans IIS en exécutant les commandes suivantes :

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample19.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Impossible de convertir Microsoft.Web.Deployment.DeploymentProviderOptions

### <a name="scenario"></a>Scénario

Lorsque vous déployez un package, vous consultez le message d’erreur suivant :

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample20.cmd)]

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Vous essayez de déployer à partir de Gestionnaire des services Internet à l’aide de la déployer 1.1 interface utilisateur Web pour un serveur doté de Web Deploy 2.0 est installé. Si vous utilisez l’outil d’Administration à distance de IIS pour déployer à l’importation d’un package, vérifiez le **nouvelles fonctionnalités disponibles** boîte de dialogue lorsque vous établissez la connexion. (Cette boîte de dialogue peut uniquement être affichée qu’une seule fois lorsque la connexion est établie pour la première. Pour effacer la connexion et recommencer, fermez le Gestionnaire IIS et démarrent à nouveau en entrant `inetmgr /reset` à l’invite de commandes.) Si l’une des fonctionnalités répertoriées est **l’interface utilisateur de déploiement Web**et il a un numéro de version inférieur à 8, le serveur que vous effectuez le déploiement peut avoir des versions 1.1 et 2.0 de Web Deploy installé. Pour déployer à partir d’un client qui a 2.0 est installé, le serveur doit avoir uniquement Web Deploy 2.0 installé. Vous devrez contacter votre fournisseur d’hébergement pour résoudre ce problème.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>Impossible de charger les composants natifs de SQL Server Compact

### <a name="scenario"></a>Scénario

Lorsque vous exécutez le site déployé, vous consultez le message d’erreur suivant :

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample21.cmd)]

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Le site déployé n’a pas *amd64* et *x86* sous-dossiers avec les assemblys natifs dans les sous l’application *bin* dossier. Sur un ordinateur doté de SQL Server Compact installé, les assemblys natifs sont stockés dans *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*. La meilleure façon d’obtenir les fichiers appropriés dans les dossiers appropriés dans un projet Visual Studio consiste à installer le package NuGet SqlServerCompact. Installation du package ajoute un script de post-build pour copier les assemblys natifs dans *amd64* et *x86*. Toutefois, dans l’ordre de ces derniers puissent être déployés, vous devez les inclure manuellement dans le projet. Pour plus d’informations, consultez le [déploiement SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) didacticiel.

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>Erreur de « Chemin d’accès n’est pas valide » après avoir déployé une application Entity Framework Code First

### <a name="scenario"></a>Scénario

Vous déployez une application qui utilise des Migrations Entity Framework Code First et un SGBD tels que SQL Server Compact qui stocke sa base de données dans un fichier dans l’application\_dossier de données. Vous avez configuré pour créer la base de données après votre premier déploiement de Code First Migrations. Lorsque vous exécutez l’application, vous obtenez un message d’erreur comme dans l’exemple suivant :

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample22.cmd)]

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Code tout d’abord tente de créer la base de données, mais l’application\_dossier de données n’existe pas. Soit vous n’avez tous les fichiers le *application\_données* dossier lorsque vous avez déployé ou que vous avez sélectionné **application exclure\_données** sur le **Package/Publication Web** onglet de la **propriétés du projet** fenêtre. Le processus de déploiement ne sont pas créer un dossier sur le serveur s’il en existe aucun fichier dans le dossier à copier sur le serveur. Si vous disposez déjà de la base de données défini dans le site, le processus de déploiement supprimera les fichiers et les *application\_données* dossier lui-même. Si vous avez sélectionné **supprimer les fichiers supplémentaires à la destination** dans le profil de publication. Pour résoudre ce problème, placez un fichier de l’espace réservé tel qu’un fichier .txt dans le *application\_données* dossier, assurez-vous que vous n’avez pas **application exclure\_données** sélectionné, puis redéployer. 

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>« Objet COM qui a été séparé de son RCW sous-jacent ne peut pas être utilisé. »

### <a name="scenario"></a>Scénario

Vous avez été correctement à l’aide d’un clic publier pour déployer votre application et de lancer cette erreur se produit :

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample23.cmd)]

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Fermer et de redémarrer Visual Studio sont généralement tout ce qui est nécessaire pour résoudre cette erreur.

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>Déploiement échoue, car utilisateur informations d’identification utilisées pour la publication n’avez pas setACL autorité

### <a name="scenario"></a>Scénario

Publication échoue avec une erreur indiquant que vous n’avez pas autorité pour définir les autorisations de dossier (le compte d’utilisateur que vous utilisez n’a pas setACL autorité).

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Par défaut, Visual Studio définit les autorisations sur le dossier racine du site autorisations lecture et écriture sur l’application\_dossier de données. Si vous savez que les autorisations par défaut sur les dossiers de site sont correctes et qu’il est inutile d’être définie, vous désactiver ce comportement en ajoutant **&lt;IncludeSetACLProviderOn Destination&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;** au fichier de profil de publication (pour affecter un profil unique) ou au fichier.WPP cible (à affectent tous les profils). Pour plus d’informations sur la façon de modifier ces fichiers, consultez [Comment : modifier les paramètres de déploiement dans les fichiers de profil (.pubxml)](https://msdn.microsoft.com/library/ff398069.aspx). 

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>Erreurs d’accès refusé lors de l’Application tente d’écrire dans un dossier d’Application

### <a name="scenario"></a>Scénario

Erreurs de votre application quand elle tente de créer ou modifier un fichier dans un des dossiers d’application, car il ne dispose pas d’autorité d’écriture pour ce dossier.

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Par défaut, Visual Studio définit les autorisations sur le dossier racine du site autorisations lecture et écriture sur l’application\_dossier de données. Si votre application a besoin d’accès en écriture vers un sous-dossier, vous pouvez définir des autorisations pour ce dossier comme indiqué dans le [définition des autorisations de dossier](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md) et [déploiement dans l’environnement de Production](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) didacticiels. Si votre application a besoin d’accès en écriture au dossier racine du site, vous devez empêcher son affectez à l’accès en lecture seule sur le dossier racine en ajoutant **&lt;IncludeSetACLProviderOn Destination&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;** au fichier de profil de publication (pour affecter un profil unique) ou au fichier.WPP cible (à affectent tous les profils). Pour plus d’informations sur la façon de modifier ces fichiers, consultez [Comment : modifier les paramètres de déploiement dans les fichiers de profil (.pubxml)](https://msdn.microsoft.com/library/ff398069.aspx). <a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Erreur de configuration - attribut targetFramework fait référence à une version ultérieure à la version installée du .NET Framework

### <a name="scenario"></a>Scénario

Vous avez publié avec succès un projet web qui cible ASP.NET 4.5, mais lorsque vous exécutez l’application (avec la `customErrors` mode défini sur « off » dans le fichier Web.config) vous obtenez l’erreur suivante :

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample24.cmd)]

La zone d’erreur de Source de la page d’erreur met en surbrillance la ligne suivante à partir de Web.config en tant que la cause de l’erreur :

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample25.cmd)]

### <a name="possible-cause-and-solution"></a>Cause possible et solution

Le serveur ne prend pas en charge ASP.NET 4.5. Contactez le fournisseur d’hébergement pour déterminer quand et si la prise en charge de ASP.NET 4.5 peut être ajouté. Si la mise à niveau le serveur n’est pas une option, vous devez déployer un projet web ciblant ASP.NET 4 ou version antérieure à la place. Si vous déployez un projet de web antérieures ou pour ASP.NET 4 vers la même destination, sélectionnez le **supprimer les fichiers supplémentaires à la destination** case à cocher sur la **paramètres** onglet de la **publier le site Web**Assistant. Si vous ne sélectionnez pas **supprimer les fichiers supplémentaires à la destination**, vous continuerez à accéder à la page d’erreur de Configuration.

Le projet **propriétés** windows inclut une liste déroulante du framework cible, mais vous ne pouvez pas résoudre ce problème en modifiant sa à partir **.NET Framework 4.5** à **de.NETFramework4**. Si vous modifiez le framework cible vers une version antérieure de framework, le projet contiendra encore des références aux assemblys de la dernière version framework et ne fonctionnera pas. Vous devez manuellement modifier ces références ou créez un projet qui cible .NET Framework 4 ou version antérieure. Pour plus d’informations, consultez [le ciblage du .NET Framework pour les Sites Web](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Précédent](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
