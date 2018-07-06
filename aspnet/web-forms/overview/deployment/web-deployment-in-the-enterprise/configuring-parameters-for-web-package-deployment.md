---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: Configuration des paramètres de déploiement du Package Web | Microsoft Docs
author: jrjlee
description: Cette rubrique décrit comment définir des valeurs de paramètre, telles que les noms d’application web Internet Information Services (IIS), les chaînes de connexion et les points de terminaison de service...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: dd8924b0b0055bd32ef55a9ec3a139c4d9b4eb81
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825113"
---
<a name="configuring-parameters-for-web-package-deployment"></a>Configuration des paramètres de déploiement de Package Web
====================
par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique explique comment définir des valeurs de paramètre, telles que les noms d’application web Internet Information Services (IIS), les chaînes de connexion et les points de terminaison de service, lorsque vous déployez un package web sur un serveur web IIS à distance.


Lorsque vous générez un projet d’application web, la génération et le processus d’empaquetage génère trois fichiers de clés :

- Un *[nom_projet] .zip* fichier. Il s’agit de package de déploiement web pour votre projet d’application web. Ce package contient tous les assemblys, les fichiers, les scripts de base de données et les ressources nécessaires pour recréer votre application web sur un serveur web IIS à distance.
- Un *[nom_projet].deploy.cmd* fichier. Contient un ensemble de commandes paramétrables de Web Deploy (MSDeploy.exe) qui publier votre package de déploiement web sur un serveur web IIS à distance.
- Un *[nom_projet]. SetParameters.xml* fichier. Cela fournit un ensemble de valeurs de paramètre à la commande MSDeploy.exe. Vous pouvez mettre à jour les valeurs dans ce fichier et passer à Web Deploy comme paramètre de ligne de commande lorsque vous déployez votre package web.

> [!NOTE]
> Pour plus d’informations sur la génération et le processus d’empaquetage, consultez [génération et empaquetage Web Application Projects](building-and-packaging-web-application-projects.md).


Le *SetParameters.xml* généré dynamiquement à partir de votre fichier de projet d’application web et des fichiers de configuration au sein de votre projet. Lorsque vous générez et empaqueter votre projet, le Pipeline de publication Web (WPP) détecte automatiquement un grand nombre des variables qui sont susceptibles de changer entre les environnements de déploiement, telles que l’application web IIS de destination et les chaînes de connexion de base de données. Ces valeurs sont automatiquement paramétrés dans le package de déploiement web et ajoutés à la *SetParameters.xml* fichier. Par exemple, si vous ajoutez une chaîne de connexion à la *web.config* fichier dans votre projet d’application web, le processus de génération détecte cette modification et ajoutera une entrée à la *SetParameters.xml* fichier en conséquence.

Dans de nombreux cas, ce paramétrage automatique sera suffisant. Toutefois, si vos utilisateurs ont besoin pour faire varier les autres paramètres entre les environnements de déploiement, telles que les paramètres de l’application ou l’URL du point de terminaison de service, vous devez indiquer les fournisseurs de services pour paramétrer ces valeurs dans le package de déploiement et ajouter des entrées correspondantes à la *SetParameters.xml* fichier. Les sections qui suivent expliquent comment effectuer cette opération.

### <a name="automatic-parameterization"></a>Paramétrage automatique

Lorsque vous générez et empaquetez une application web, les fournisseurs de services seront paramétrer automatiquement ces éléments :

- La destination IIS web les nom et chemin d’accès de l’application.
- Toute connexion des chaînes dans votre *web.config* fichier.
- Chaînes de connexion pour les bases de données que vous ajoutez à la **Package/Publication SQL** onglet dans les pages de propriétés de projet.

Par exemple, si vous deviez générer et empaqueter le [Contact Manager](the-contact-manager-solution.md) génère l’exemple de solution sans toucher le processus de paramétrage de quelque façon, les fournisseurs de services cette *ContactManager.Mvc.SetParameters.xml* fichier :


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]


Dans ce cas :

- Le **nom de l’Application Web IIS** paramètre est le chemin d’accès IIS où vous souhaitez déployer l’application web. La valeur par défaut provient de la **Package/Publication Web** page dans les pages de propriétés de projet.
- Le **ApplicationServices-Web.config Connection String** paramètre a été généré à partir d’un **ajouter/connectionStrings** élément dans le *web.config* fichier. Il représente la chaîne de connexion que l’application doit utiliser pour contacter la base de données d’appartenance. La valeur que vous fournissez ici sera remplacé dans déployé *web.config* fichier. La valeur par défaut provient de l’avant le déploiement *web.config* fichier.

Les fournisseurs de services ajuste également ces propriétés dans le package de déploiement qu’il génère. Vous pouvez fournir des valeurs pour ces propriétés lorsque vous installez le package de déploiement. Si vous installez le package manuellement via le Gestionnaire IIS, comme décrit dans [installer manuellement des Packages Web](manually-installing-web-packages.md), l’Assistant installation vous invite à fournir des valeurs pour tous les paramètres. Si vous installez le package à distance en utilisant le *. deploy.cmd* de fichiers, comme décrit dans [déploiement des Packages Web](deploying-web-packages.md), Web Deploy ressemblera à ce *SetParameters.xml* fichier Indiquez les valeurs de paramètre. Vous pouvez modifier les valeurs dans le *SetParameters.xml* fichier manuellement, ou vous pouvez personnaliser le fichier dans le cadre d’un processus de génération et de déploiement automatisé. Ce processus est décrit plus en détail plus loin dans cette rubrique.

### <a name="custom-parameterization"></a>Paramétrage personnalisé

Dans les scénarios de déploiement plus complexes, vous aurez souvent besoin de paramétrer des propriétés supplémentaires avant de déployer votre projet. En règle générale, vous devez paramétrer toutes les propriétés et paramètres qui varient entre les environnements de destination. Celles-ci peuvent inclure :

- Des points de terminaison dans le *web.config* fichier.
- Paramètres d’application dans le *web.config* fichier.
- Toutes les autres propriétés déclaratives que vous souhaitez invitent les utilisateurs à spécifier.

Pour paramétrer ces propriétés le plus simple consiste à ajouter un *parameters.xml* fichier dans le dossier racine de votre projet d’application web. Par exemple, dans la solution Gestionnaire de contacts, le projet ContactManager.Mvc inclut un *parameters.xml* fichier dans le dossier racine.

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

Si vous ouvrez ce fichier, vous verrez qu’il contient un seul **paramètre** entrée. L’entrée utilise une requête XML Path Language (XPath) pour localiser et de paramétrer l’URL de point de terminaison du service ContactService Windows Communication Foundation (WCF) dans le *web.config* fichier.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]


En plus du paramétrage de l’URL de point de terminaison dans le package de déploiement, les fournisseurs de services ajoute également une entrée correspondante à la *SetParameters.xml* fichier est généré en même temps que le package de déploiement.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]


Si vous installez manuellement le package de déploiement, le Gestionnaire IIS vous invitera pour l’adresse de point de terminaison de service en même temps que les propriétés qui ont été paramétré automatiquement. Si vous installez le package de déploiement en exécutant la *. deploy.cmd* fichier, vous pouvez modifier le *SetParameters.xml* fichier pour fournir une valeur pour l’adresse de point de terminaison de service ainsi que des valeurs pour le propriétés qui ont été paramétrées automatiquement.

Pour plus d’informations sur la création d’un *parameters.xml* de fichiers, consultez [Comment : utiliser les paramètres pour configurer les paramètres lorsqu’un Package de déploiement est installé](https://msdn.microsoft.com/library/ff398068.aspx). La procédure nommée **à utiliser des paramètres de déploiement pour les paramètres du fichier Web.config** fournit des instructions détaillées.

## <a name="modifying-the-setparametersxml-file"></a>Modification du fichier SetParameters.xml

Si vous envisagez de déployer le package d’application web manuellement&#x2014;en exécutant la *. deploy.cmd* fichier ou en exécutant MSDeploy.exe à partir de la ligne de commande&#x2014;rien ne vous modifier manuellement le  *SetParameters.xml* fichier avant le déploiement. Toutefois, si vous travaillez sur une solution à l’échelle de l’entreprise, vous devez déployer un package d’application web en tant que partie d’un processus de génération et de déploiement plus vastes et automatisé. Dans ce scénario, vous avez besoin de Microsoft Build Engine (MSBuild) pour modifier le *SetParameters.xml* fichier pour vous. Vous pouvez faire à l’aide de MSBuild **XmlPoke** tâche.

Le [solution d’exemple de gestionnaire de contacts](the-contact-manager-solution.md) illustre ce processus. Les exemples de code qui suivent ont été modifiées pour afficher uniquement les détails pertinents pour cet exemple.

> [!NOTE]
> Pour une vue d’ensemble plus large dans le fichier du modèle de projet dans l’exemple de solution et une introduction aux fichiers de projet personnalisé en général, consultez [présentation du fichier projet](understanding-the-project-file.md) et [comprendre le processus de génération](understanding-the-build-process.md).


Tout d’abord, les valeurs de paramètre d’intérêt sont définies en tant que propriétés dans le fichier de projet spécifique à l’environnement (par exemple, *Env-Dev.proj*).


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]


> [!NOTE]
> Pour obtenir des conseils sur la façon de personnaliser les fichiers de projet spécifique à l’environnement pour vos propres environnements serveur, consultez [configurer les propriétés de déploiement pour un environnement cible](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


Ensuite, le *Publish.proj* fichier importe ces propriétés. Étant donné que chaque *SetParameters.xml* fichier est associé un *. deploy.cmd* de fichiers et nous avons finalement que le fichier projet pour appeler chaque *. deploy.cmd* de fichiers, le projet fichier crée une MSBuild *élément* pour chaque *. deploy.cmd* de fichiers et définit les propriétés dignes d’intérêt en tant que *métadonnées d’élément*.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]


Dans ce cas :

- Le **ParametersXml** valeur des métadonnées indique l’emplacement de la *SetParameters.xml* fichier.
- Le **IisWebAppName** valeur est le chemin d’accès IIS à laquelle vous souhaitez déployer l’application web.
- Le **MembershipDBConnectionString** valeur est la chaîne de connexion pour la base de données d’appartenance et le **MembershipDBConnectionName** valeur est la **nom** attribut du paramètre correspondant dans le *SetParameters.xml* fichier.
- Le **ServiceEndpointValue** valeur est l’adresse de point de terminaison pour le service WCF sur le serveur de destination et le **ServiceEndpointParamName** valeur est l’attribut de nom du paramètre correspondant dans le *SetParameters.xml* fichier.

Enfin, dans le *Publish.proj* fichier, le **PublishWebPackages** cible utilise la **XmlPoke** tâche pour modifier ces valeurs dans le *SetParameters.xml* fichier.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]


Vous remarquerez que chaque **XmlPoke** tâche spécifie quatre valeurs d’attribut :

- Le **XmlInputPath** attribut indique à la tâche où trouver le fichier que vous souhaitez modifier.
- Le **requête** attribut est une requête XPath qui identifie le nœud XML que vous souhaitez modifier.
- Le **valeur** attribut est la nouvelle valeur que vous souhaitez insérer dans le nœud XML sélectionné.
- Le **Condition** attribut est le critère sur lequel la tâche doit s’exécuter ou non. Dans ce cas, la condition permet de s’assurer que vous n’essayez pas d’insérer une valeur null ou vide dans le *SetParameters.xml* fichier.

## <a name="conclusion"></a>Conclusion

Cette rubrique décrit le rôle de la *SetParameters.xml* de fichiers et expliqué comment il est généré lorsque vous générez un projet d’application web. Vous avez appris comment vous pouvez paramétrer des paramètres supplémentaires en ajoutant un *parameters.xml* fichier à votre projet. Il décrit également comment vous pouvez modifier le *SetParameters.xml* fichier en tant que partie d’un processus de génération plus vastes et automatisés, à l’aide de la **XmlPoke** tâche dans vos fichiers projet.

La rubrique suivante, [déploiement des Packages Web](deploying-web-packages.md), décrit comment déployer un package web en exécutant la *. deploy.cmd* de fichiers ou commandes directement à l’aide de MSDeploy.exe. Dans les deux cas, vous pouvez spécifier votre *SetParameters.xml* fichier en tant que paramètre de déploiement.

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur la façon de créer des packages web, consultez [génération et empaquetage Web Application Projects](building-and-packaging-web-application-projects.md). Pour obtenir des conseils sur la façon de les déployer un package web, consultez [déploiement des Packages Web](deploying-web-packages.md). Pour connaître la procédure pas à pas comment créer un *parameters.xml* de fichiers, consultez [Comment : utiliser les paramètres pour configurer les paramètres lorsqu’un Package de déploiement est installé](https://msdn.microsoft.com/library/ff398068.aspx).

Pour obtenir des informations plus générales sur le paramétrage de Web Deploy, consultez [paramétrage de déploiement Web en Action](https://go.microsoft.com/?linkid=9805119) (billet de blog).

> [!div class="step-by-step"]
> [Précédent](building-and-packaging-web-application-projects.md)
> [Suivant](deploying-web-packages.md)
