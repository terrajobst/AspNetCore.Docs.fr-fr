---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
title: Déploiement de Packages Web | Microsoft Docs
author: jrjlee
description: Cette rubrique décrit comment vous pouvez publier des packages de déploiement web à un serveur distant à l’aide de l’outil de déploiement Web Internet Information Services (IIS) (Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: a5c5eed2-8683-40a5-a2e1-35c9f8d17c29
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
msc.type: authoredcontent
ms.openlocfilehash: a660a9a5593d41064ab099296d4a7a675b206bb5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37392856"
---
<a name="deploying-web-packages"></a>Déploiement de Packages Web
====================
par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique décrit comment vous pouvez publier des packages de déploiement web à un serveur distant à l’aide de l’outil de déploiement Web Internet Information Services (IIS) (Web Deploy) 2.0.
> 
> Il existe deux façons dans lequel vous pouvez déployer un package web sur un serveur distant :
> 
> - Vous pouvez utiliser l’utilitaire de ligne de commande MSDeploy.exe directement.
> - Vous pouvez exécuter la *[nom_projet].deploy.cmd* qui génère le processus de génération.
> 
> Le résultat final est le même quel que soit l’approche que vous utilisez. En fait, toutes les *. deploy.cmd* fait le fichier consiste à exécuter MSDeploy.exe avec certaines valeurs prédéterminés, afin que vous n’êtes pas obligé de fournir autant d’informations afin de déployer le package. Cela simplifie le processus de déploiement. En revanche, à l’aide de MSDeploy.exe directement vous donne une plus grande flexibilité sur exactement comment votre package est déployé.
> 
> Approche que vous utilisez dépend de divers facteurs, notamment quel degré de contrôle que vous avez besoin sur le processus de déploiement et si vous ciblez le service Web déployer l’Agent à distance ou le Gestionnaire de déploiement Web. Cette rubrique explique comment utiliser chaque approche et identifie quand chaque approche est appropriée.
> 
> Les tâches et les procédures pas à pas dans cette rubrique supposent que :
> 
> - Vous avez généré et empaqueté votre application web, comme décrit dans [génération et empaquetage Web Application Projects](building-and-packaging-web-application-projects.md).
> - Vous avez modifié le *SetParameters.xml* fichier pour fournir les valeurs de paramètre de droite pour votre environnement cible, comme décrit dans [configuration des paramètres pour le déploiement du Package Web](configuring-parameters-for-web-package-deployment.md).


En cours d’exécution le [*nom_projet*]*. deploy.cmd* fichier est la façon la plus simple pour déployer un package web. En particulier, à l’aide de la *. deploy.cmd* fichier offre les avantages suivants par rapport à l’aide de MSDeploy.exe directement :

- Vous n’avez pas besoin de spécifier l’emplacement du package de déploiement web&#x2014;le *. deploy.cmd* fichier sait déjà où il est.
- Vous n’avez pas besoin de spécifier l’emplacement de la *SetParameters.xml* fichier&#x2014;le *. deploy.cmd* fichier sait déjà où il est.
- Vous n’avez pas besoin de spécifier des fournisseurs de MSDeploy source et destination&#x2014;le *. deploy.cmd* fichier sait déjà les valeurs à utiliser.
- Vous n’avez pas besoin de spécifier les paramètres d’opération MSDeploy&#x2014;le *. deploy.cmd* fichier ajoute automatiquement les valeurs couramment requises à la commande MSDeploy.exe.

Avant d’utiliser le *. deploy.cmd* fichier pour déployer un package web, vous devez vous assurer que :

- Le *. deploy.cmd* fichier, le [*nom_projet*]. *SetParameters.xml* fichier et le package web ([*nom_projet*]. *ZIP*) se trouvent dans le même dossier.
- Web Deploy (MSDeploy.exe) est installé sur l’ordinateur qui exécute le *. deploy.cmd* fichier.

Le *. deploy.cmd* fichier prend en charge diverses options de ligne de commande. Lorsque vous exécutez le fichier à partir d’une invite de commandes, il s’agit de la syntaxe de base :


[!code-console[Main](deploying-web-packages/samples/sample1.cmd)]


Vous devez spécifier soit un **/T** indicateur ou un **/Y** indicateur, pour indiquer si vous souhaitez effectuer un essai ou un déploiement en direct respectivement (n’utilisez pas les deux indicateurs dans la même commande). Ce tableau explique l’objectif de chacun de ces indicateurs.

| Indicateur | Description |
| --- | --- |
| **/T** | Appelle MSDeploy.exe avec le **– whatif** indicateur qui signale une tentative d’exécution. Plutôt que de déployer le package, il crée un rapport de ce qui se passerait si vous n’avez déployé le package. |
| **/Y** | Appelle MSDeploy.exe sans le **– whatif** indicateur. Il déploie le package sur l’ordinateur local ou le serveur de destination spécifié. |
| **/M** | Spécifie le serveur de destination nom ou URL du service. Pour plus d’informations sur les valeurs que vous pouvez fournir ici, consultez le **considérations sur le point de terminaison** dans cette rubrique. Si vous omettez le **/M** indicateur, le package sera déployé sur l’ordinateur local. |
| **/A** | Spécifie le type d’authentification que MSDeploy.exe doit utiliser pour effectuer le déploiement. Les valeurs possibles sont **NTLM** et **base**. Si vous omettez le **/A** indicateur, le type d’authentification par défaut est **NTLM** pour le déploiement vers le service Web déployer l’Agent à distance et en **base** pour le déploiement sur le Web Deploy Gestionnaire. |
| **/U** | Spécifie le nom d’utilisateur. Cela s’applique uniquement si vous utilisez l’authentification de base. |
| **/P** | Spécifie le mot de passe. Cela s’applique uniquement si vous utilisez l’authentification de base. |
| **/L** | Indique que le package doit être déployé à l’instance locale d’IIS Express. |
| **/G** | Spécifie que le package est déployé à l’aide de la [paramètre de fournisseur tempAgent](https://technet.microsoft.com/library/ee517345(WS.10).aspx). Si vous omettez le **/G** indicateur, la valeur par défaut est **false**. |

> [!NOTE]
> Chaque fois que le processus de génération crée un package web, il crée également un fichier nommé *[nom_projet] .deploy-readme.txt* qui explique ces options de déploiement.


Outre ces indicateurs, vous pouvez spécifier des paramètres d’opération Web Deploy comme supplémentaires *. deploy.cmd* paramètres. Tous les autres paramètres que vous spécifiez sont simplement transmises à la commande MSDeploy.exe sous-jacent. Pour plus d’informations sur ces paramètres, consultez [Web déployer des paramètres de fonctionnement](https://technet.microsoft.com/library/dd569089(WS.10).aspx).

Supposons que vous souhaitez déployer le projet d’application web ContactManager.Mvc dans un environnement de test en exécutant le *. deploy.cmd* fichier. Votre environnement de test est configuré pour utiliser le service Web déployer l’Agent à distance, comme décrit dans [configurer un serveur Web pour déployer publication sur le Web (Agent distant)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Pour déployer l’application web, vous devez suivre les étapes suivantes.

**Pour déployer une application web à l’aide du. fichier deploy.cmd**

1. Générer et empaqueter le projet d’application web, comme décrit dans [génération et empaquetage Web Application Projects](building-and-packaging-web-application-projects.md).
2. Modifier le *ContactManager.Mvc.SetParameters.xml* fichier doit contenir des valeurs de paramètre correct pour votre environnement de test, comme décrit dans [configuration des paramètres pour le déploiement du Package Web](configuring-parameters-for-web-package-deployment.md).
3. Ouvrez une fenêtre d’invite de commandes et accédez à l’emplacement de la *ContactManager.Mvc.deploy.cmd* fichier.
4. Tapez la commande suivante et appuyez sur ENTRÉE :

    [!code-console[Main](deploying-web-packages/samples/sample2.cmd)]

Dans cet exemple :

- Le **/Y** indicateur indique que vous souhaitez déployer le package, au lieu de faire un essai d’exécuter.
- Le **/M** indicateur indique que vous souhaitez déployer le package sur le serveur nommé TESTWEB1. À partir de cette valeur, MSDeploy.exe va tenter de déployer le package sur le service Web déployer l’Agent à distance à http://TESTWEB1/MSDeployAgentService.
- Le **/A** indicateur indique que vous souhaitez utiliser l’authentification NTLM. Par conséquent, vous n’avez pas besoin de spécifier un nom d’utilisateur et le mot de passe.

Pour illustrer comment l’utilisation du *. deploy.cmd* fichier simplifie le processus de déploiement, examinons la commande MSDeploy.exe qui obtient générée et exécutée lorsque vous exécutez *ContactManager.Mvc.deploy.cmd* en utilisant les options ci-dessus.


[!code-console[Main](deploying-web-packages/samples/sample3.cmd)]


Pour plus d’informations sur l’utilisation de la *. deploy.cmd* fichier pour déployer un package web, consultez [Comment : installer un Package de déploiement à l’aide du fichier deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).

## <a name="using-msdeployexe"></a>À l’aide de MSDeploy.exe

Bien que l’utilisation du *. deploy.cmd* plus généralement, le fichier simplifie le processus de déploiement, il existe certaines situations lorsqu’il est préférable d’utiliser directement les MSDeploy.exe. Exemple :

- Si vous souhaitez déployer sur le Gestionnaire de déploiement Web en tant qu’un utilisateur non-administrateur, vous ne pouvez pas utiliser le *. deploy.cmd* fichier. Il s’agit d’un bogue dans Web Deploy 2.0, comme décrit sous **considérations sur le point de terminaison**.
- Si vous souhaitez basculer manuellement entre les différents *SetParameters.xml* fichiers dans différents emplacements, vous pouvez utiliser directement les MSDeploy.exe.
- Si vous souhaitez remplacer plusieurs arguments de ligne de commande MSDeploy.exe, vous pouvez utiliser directement les MSDeploy.exe.

Lorsque vous utilisez MSDeploy.exe, vous devez fournir trois informations essentielles :

- Un **– source** paramètre indiquant d'où proviennent vos données.
- Un **– dest** paramètre qui indique où vos données va.
- Un **– verbe** paramètre qui indique le [opération](https://technet.microsoft.com/library/dd568989(WS.10).aspx) vous souhaitez effectuer.

MSDeploy.exe s’appuie sur [fournisseurs Web Deploy](https://technet.microsoft.com/library/dd569040(WS.10).aspx) pour traiter les données source et de destination. Web Deploy inclut un grand nombre de fournisseurs qui représente la plage des applications et sources de données peut fonctionner avec&#x2014;, par exemple, il existe des fournisseurs pour les bases de données SQL Server, les serveurs web IIS, les certificats, les assemblys global assembly cache (GAC), différents différents fichiers de configuration et bien d’autres types de données. Les deux le **– source** paramètre et le **– dest** paramètre doit spécifier un fournisseur, sous la forme **– source**: [*providerName*] = [*emplacement*]. Lorsque vous déployez un package web sur un site Web IIS, vous devez utiliser ces valeurs :

- Le **– source** fournisseur est toujours [package](https://technet.microsoft.com/library/dd569019(WS.10).aspx). Exemple :

    [!code-console[Main](deploying-web-packages/samples/sample4.cmd)]
- Le **– dest** fournisseur est toujours [automatique](https://technet.microsoft.com/library/dd569016(WS.10).aspx). Exemple :

    [!code-console[Main](deploying-web-packages/samples/sample5.cmd)]
- Le **– verbe** est toujours **synchronisation**.

    [!code-console[Main](deploying-web-packages/samples/sample6.cmd)]

En outre, vous devez spécifier plusieurs autres [paramètres spécifiques au fournisseur](https://technet.microsoft.com/library/dd569001(WS.10).aspx) et général [paramètres d’opération](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Par exemple, supposons que vous souhaitez déployer l’application web de ContactManager.Mvc dans un environnement intermédiaire. Le déploiement cible le Gestionnaire de déploiement Web et devez utiliser l’authentification de base. Pour déployer l’application web, vous devez suivre les étapes suivantes.

**Pour déployer une application web à l’aide de MSDeploy.exe**

1. Générer et empaqueter le projet d’application web, comme décrit dans [génération et empaquetage Web Application Projects](building-and-packaging-web-application-projects.md).
2. Modifier le *ContactManager.Mvc.SetParameters.xml* fichier doit contenir des valeurs de paramètre correct pour votre environnement intermédiaire, comme décrit dans [configuration des paramètres pour le déploiement du Package Web](configuring-parameters-for-web-package-deployment.md).
3. Ouvrez une fenêtre d’invite de commandes et accédez à l’emplacement de MSDeploy.exe. Il s’agit généralement à %PROGRAMFILES%\IIS\Microsoft Web déployer V2\msdeploy.exe.
4. Tapez la commande suivante et appuyez sur entrée (ignorez les sauts de ligne) :

    [!code-console[Main](deploying-web-packages/samples/sample7.cmd)]

Dans cet exemple :

- Le **– source** paramètre spécifie le **package** fournisseur et indique l’emplacement du package web.
- Le **– dest** paramètre spécifie le **automatique** fournisseur. Le **computerName** paramètre fournit l’URL du service du Gestionnaire de déploiement Web sur le serveur de destination. Le **authtype** paramètre indique que vous souhaitez utiliser l’authentification de base, et par conséquent, vous devez fournir un **nom d’utilisateur** et un **mot de passe**. Enfin, le **includeAcls = « False »** paramètre indique que vous ne souhaitez pas copier les listes de contrôle d’accès (ACL) des fichiers dans votre application de web source vers le serveur de destination.
- Le **– verbe : synchronisation** argument indique que vous souhaitez répliquer le contenu source sur le serveur de destination.
- Le **– disableLink** arguments indiquent que vous ne souhaitez pas répliquer les pools d’applications, la configuration du répertoire virtuel ou certificats Secure Sockets Layer (SSL) sur le serveur de destination. Pour plus d’informations, consultez [Web Deploy lien Extensions](https://technet.microsoft.com/library/dd569028(WS.10).aspx).
- Le **– setParamFile** paramètre fournit l’emplacement de la *SetParameters.xml* fichier.
- Le **– allowUntrusted** commutateur indique que Web Deploy doit accepter des certificats SSL qui n’ont pas été émis par une autorité de certification approuvée. Si vous déployez dans le Gestionnaire de déploiement Web et que vous avez utilisé un certificat auto-signé pour sécuriser l’URL du service, vous devez inclure ce commutateur.

## <a name="automating-web-package-deployment"></a>Automatisation du déploiement du Package Web

Dans de nombreux scénarios d’entreprise, vous souhaiterez déployer vos packages web en tant que partie d’une plus grande étape unique ou d’un déploiement automatisé. Que vous choisissiez de déployer vos packages web en exécutant la *. deploy.cmd* de fichiers ou à l’aide de MSDeploy.exe directement, vous pouvez paramétrer vos commandes et les appeler à partir d’une cible dans un Microsoft Build Engine (MSBuild) fichier de projet.

Dans l’exemple de solution du Gestionnaire de contacts, examinons le **PublishWebPackages** cibler dans le *Publish.proj* fichier. Cette cible s’exécute une fois pour chaque *. deploy.cmd* fichier identifié par une liste d’éléments nommée **PublishPackages**. La cible utilise les propriétés et métadonnées d’élément pour générer un ensemble complet de valeurs d’argument pour chaque *. deploy.cmd* fichier, puis utilise le **Exec** tâche pour exécuter la commande.


[!code-xml[Main](deploying-web-packages/samples/sample8.xml)]


> [!NOTE]
> Pour une vue d’ensemble plus large dans le fichier du modèle de projet dans l’exemple de solution et une introduction aux fichiers de projet personnalisé en général, consultez [présentation du fichier projet](understanding-the-project-file.md) et [comprendre le processus de génération](understanding-the-build-process.md).


## <a name="endpoint-considerations"></a>Considérations sur le point de terminaison

Indépendamment de si vous déployez votre package web en exécutant la *. deploy.cmd* de fichiers ou à l’aide de MSDeploy.exe directement, vous devez spécifier un nom d’ordinateur ou un point de terminaison de service pour votre déploiement.

Si le serveur web de destination est configuré pour le déploiement à l’aide du service Web déployer l’Agent à distance, vous spécifiez l’URL du service cible en tant que votre destination.


[!code-console[Main](deploying-web-packages/samples/sample9.cmd)]


Sinon, vous pouvez spécifier uniquement le nom du serveur en tant que votre destination et Web Deploy déduira l’URL du service agent distant.


[!code-console[Main](deploying-web-packages/samples/sample10.cmd)]


Si le serveur web de destination est configuré pour le déploiement à l’aide du Gestionnaire de déploiement Web, vous devez spécifier l’adresse de point de terminaison du Service de gestion IIS Web (WMSvc) en tant que votre destination. Par défaut, cela prend la forme :


[!code-console[Main](deploying-web-packages/samples/sample11.cmd)]


Vous pouvez cibler un de ces points de terminaison à l’aide du *. deploy.cmd* MSDeploy.exe directement ou fichier. Toutefois, si vous souhaitez déployer sur le Gestionnaire de déploiement Web en tant qu’un utilisateur non-administrateur, comme décrit dans [configurer un serveur Web pour déployer publication sur le Web (Gestionnaire de déploiement Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md), vous devez ajouter une chaîne de requête à l’adresse de point de terminaison de service.


[!code-console[Main](deploying-web-packages/samples/sample12.cmd)]


Il s’agit, car l’utilisateur non administrateur n’a pas accès au niveau du serveur IIS ; il ou elle n’a accès à un site Web IIS spécifique. Au moment de l’écriture, en raison d’un bogue dans le Pipeline de publication Web (WPP), vous ne pouvez pas exécuter le *. deploy.cmd* fichier à l’aide d’une adresse de point de terminaison qui inclut une chaîne de requête. Dans ce scénario, vous devez déployer votre package web à l’aide de MSDeploy.exe directement.

> [!NOTE]
> Pour plus d’informations sur le service Web déployer l’Agent à distance et le Gestionnaire de déploiement Web, consultez [choix de l’approche de droite pour le déploiement Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md). Pour obtenir des conseils sur la façon de configurer vos fichiers de projet spécifique à l’environnement pour déployer sur ces points de terminaison, consultez [configurer les propriétés de déploiement pour un environnement cible](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="authentication-considerations"></a>Considérations relatives à l’authentification

Indépendamment de si vous déployez votre package web en exécutant la *. deploy.cmd* de fichiers ou à l’aide de MSDeploy.exe directement, vous devez spécifier un type d’authentification. Web Deploy accepte deux valeurs possibles : **NTLM** ou **base**. Si vous spécifiez l’authentification de base, vous devez également fournir un nom d’utilisateur et le mot de passe. Il existe différents facteurs, que vous devez être conscient lorsque vous sélectionnez un type d’authentification :

- Si vous déployez dans le service Web déployer l’Agent à distance, vous devez utiliser l’authentification NTLM. Le service de l’agent distant n’accepte pas les informations d’identification de l’authentification de base.
- Si vous déployez dans le Gestionnaire de déploiement Web, vous pouvez utiliser NTLM ou l’authentification de base. Le paramètre par défaut est l’authentification de base. Bien que l’authentification de base s’appuie sur les noms d’utilisateur et mots de passe transmis en texte brut, vos informations d’identification sont protégées comme le Gestionnaire de déploiement Web utilise toujours le chiffrement SSL.
- Si votre package web inclut une base de données et le serveur web et le serveur de base de données sont des ordinateurs distincts, vous ne pourrez pas déployer la base de données à l’aide de l’authentification NTLM en raison du [limitation de NTLM « double saut »](https://go.microsoft.com/?linkid=9805120). Vous devez utiliser les informations d’identification SQL Server dans votre chaîne de connexion de déploiement ou fournir des informations d’identification de l’authentification de base pour Web Deploy. Ce problème est décrit plus en détail dans [déploiement de bases de données d’appartenance pour les environnements d’entreprise](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

## <a name="conclusion"></a>Conclusion

Cette rubrique décrit comment déployer un package web en exécutant la *. deploy.cmd* fichier ou à l’aide de MSDeploy.exe directement. Vous avez appris lorsque chaque approche peut être appropriée, et il décrit comment vous pouvez paramétrer et exécuter une commande de déploiement dans le cadre d’un processus plus vaste de build étape unique ou automatisés.

## <a name="further-reading"></a>informations supplémentaires

Pour obtenir des conseils sur la façon de créer et de paramétrer un package de déploiement web, consultez [génération et empaquetage Web Application Projects](building-and-packaging-web-application-projects.md) et [configuration des paramètres pour le déploiement du Package Web](configuring-parameters-for-web-package-deployment.md). Pour obtenir des conseils sur la façon de créer et déployer des packages web à partir d’une instance de Team Foundation Server (TFS), consultez [configuration Team Foundation Server pour le déploiement de Web automatisée](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Pour plus d’informations sur la façon de personnaliser et dépanner le processus de déploiement, consultez [à l’exclusion de fichiers et dossiers de déploiement](../advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Précédent](configuring-parameters-for-web-package-deployment.md)
> [Suivant](deploying-database-projects.md)
