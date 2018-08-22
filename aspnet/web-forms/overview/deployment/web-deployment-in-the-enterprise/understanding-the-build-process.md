---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: Comprendre le processus de génération | Microsoft Docs
author: jrjlee
description: Cette rubrique fournit une procédure pas à pas d’un processus de génération et de déploiement de l’échelle de l’entreprise. L’approche décrite dans cette rubrique utilise du Microsoft Build moteur personnalisé...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 9df145b281b086f546c55d0b26a8b0e44e896bb0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823683"
---
<a name="understanding-the-build-process"></a>Comprendre le processus de génération
====================
par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique fournit une procédure pas à pas d’un processus de génération et de déploiement de l’échelle de l’entreprise. L’approche décrite dans cette rubrique utilise les fichiers de projet personnalisés Microsoft Build Engine (MSBuild) pour fournir un contrôle affiné sur tous les aspects du processus. Dans les fichiers projet, les cibles de MSBuild personnalisées sont utilisés pour exécuter des utilitaires de déploiement comme l’outil de déploiement Web Internet Information Services (IIS) (MSDeploy.exe) et l’utilitaire de déploiement de base de données VSDBCMD.exe.
> 
> > [!NOTE]
> > La rubrique précédente, [présentation du fichier de projet](understanding-the-project-file.md), décrit les principaux composants d’un fichier projet MSBuild et a introduit le concept de fractionnement des fichiers de projet pour prendre en charge le déploiement sur plusieurs environnements cibles. Si vous n’êtes pas déjà familiarisé avec ces concepts, vous devez examiner [présentation du fichier projet](understanding-the-project-file.md) avant d’utiliser cette rubrique.


Cette rubrique fait partie d’une série de didacticiels basées sur les exigences de déploiement d’entreprise de la société fictive Fabrikam, Inc. Cette série de didacticiels utilise un exemple de solution&#x2014;le [solution Gestionnaire de contacts](the-contact-manager-solution.md)&#x2014;pour représenter une application web avec un niveau réaliste de complexité, y compris une application ASP.NET MVC 3, une Communication de Windows Foundation (WCF) service et un projet de base de données.

La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet de fractionnement décrite dans [présentation du fichier projet](understanding-the-project-file.md), dans lequel le processus de génération est contrôlé par deux fichiers de projet&#x2014;contenant un seul les instructions qui s’appliquent à chaque environnement de destination et celui qui contient les paramètres de génération et de déploiement spécifiques à l’environnement de génération. Au moment de la génération, le fichier de projet spécifique à l’environnement est fusionné dans le fichier de projet d’indépendant de l’environnement pour former un ensemble complet d’instructions de génération.

## <a name="build-and-deployment-overview"></a>Vue d’ensemble de déploiement et de la build

Dans le [solution Gestionnaire de contacts](the-contact-manager-solution.md), trois fichiers de contrôlent le processus de génération et de déploiement :

- Un *fichier projet universelle* (*Publish.proj*). Il contient des instructions de génération et de déploiement qui ne changent pas entre les environnements de destination.
- Un *fichier de projet spécifique à un environnement* (*Env-Dev.proj*). Il contient des paramètres de génération et de déploiement qui sont spécifiques à un environnement de destination particulier. Par exemple, vous pouvez utiliser la *Env-Dev.proj* fichier pour fournir des paramètres pour un environnement de développement ou de test et créer un autre fichier nommé *Env-Stage.proj* pour fournir les paramètres pour une mise en lots environnement.
- Un *fichier de commandes* (*publier-Dev.cmd*). Ce fichier contient une commande qui spécifie les fichiers de projet auquel vous souhaitez exécuter de MSBuild.exe. Vous pouvez créer un fichier de commandes pour chaque environnement de destination, où chaque fichier contient une commande MSBuild.exe qui spécifie un fichier de projet spécifiques à l’environnement différent. Cela permet au développeur de déployer sur différents environnements simplement en exécutant le fichier de commande appropriée.

Dans l’exemple de solution, vous trouverez ces trois fichiers dans le dossier de solution de publication.

![](understanding-the-build-process/_static/image1.png)

Avant d’examiner ces fichiers plus en détail, jetons un œil au fonctionnement du processus de build global lorsque vous utilisez cette approche. À un niveau élevé, le processus de génération et de déploiement ressemble à ceci :

![](understanding-the-build-process/_static/image2.png)

La première chose qui se produit est que les deux fichiers projet&#x2014;celui qui contient des instructions de génération et de déploiement universelles et celui qui contient les paramètres spécifiques à l’environnement&#x2014;sont fusionnés dans un seul fichier projet. MSBuild utilise ensuite les instructions fournies dans le fichier projet. Il génère chaque projet dans la solution, en utilisant le fichier de projet pour chaque projet. Il appelle ensuite aux autres outils, tels que Web Deploy (MSDeploy.exe) et l’utilitaire VSDBCMD pour déployer votre contenu web et les bases de données à l’environnement cible.

À partir du début à la fin, le processus de génération et de déploiement effectue ces tâches :

1. Il supprime le contenu du répertoire de sortie, en préparation pour une nouvelle génération.
2. Il génère chaque projet dans la solution :

    1. Pour les projets web&#x2014;dans ce cas, une application web ASP.NET MVC et un service WCF web service&#x2014;le processus de génération crée un package de déploiement web pour chaque projet.
    2. Pour les projets de base de données, le processus de génération crée un manifeste de déploiement (fichier .deploymanifest) pour chaque projet.
3. Elle utilise l’utilitaire VSDBCMD.exe pour déployer chaque projet de base de données dans la solution, à l’aide de différentes propriétés des fichiers de projet&#x2014;une chaîne de connexion cible et un nom de base de données&#x2014;ainsi que le fichier .deploymanifest.
4. Elle utilise l’utilitaire MSDeploy.exe pour déployer chaque projet web dans la solution, à l’aide de différentes propriétés des fichiers de projet pour contrôler le processus de déploiement.

Vous pouvez utiliser l’exemple de solution pour effectuer le suivi de ce processus plus en détail.

> [!NOTE]
> Pour obtenir des conseils sur la façon de personnaliser les fichiers de projet spécifique à l’environnement pour vos propres environnements serveur, consultez [configurer les propriétés de déploiement pour un environnement cible](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="invoking-the-build-and-deployment-process"></a>Appel de la génération et le processus de déploiement

Pour déployer la solution de gestionnaire de contacts dans un environnement de test de développeur, le développeur exécute la *publier-Dev.cmd* fichier de commandes. Cela appelle MSBuild.exe, en spécifiant *Publish.proj* en tant que le fichier projet à exécuter et *Env-Dev.proj* comme valeur de paramètre.


[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]


> [!NOTE]
> Le **/fl** basculer (abréviation de **/fileLogger**) consigne la sortie de génération dans un fichier nommé *msbuild.log* dans le répertoire actif. Pour plus d’informations, consultez le [référence de ligne de commande MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).


À ce stade, MSBuild commence à s’exécuter, charge le *Publish.proj* fichier et commence à traiter les instructions qu’il contient. La première instruction indique à MSBuild d’importer le projet de fichiers qui le **TargetEnvPropsFile** spécifie de paramètre.


[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]


Le **TargetEnvPropsFile** paramètre spécifie le *Env-Dev.proj* de fichiers, donc MSBuild fusionne le contenu de la *Env-Dev.proj* de fichiers dans le  *Publish.proj* fichier.

Les éléments suivants MSBuild rencontre dans le fichier projet fusionné sont des groupes de propriétés. Propriétés sont traitées dans l’ordre dans lequel ils apparaissent dans le fichier. MSBuild crée une paire clé-valeur pour chaque propriété, en fournissant que les conditions spécifiées sont remplies. Propriétés définies ultérieurement dans le fichier remplacera toutes les propriétés portant le même nom que celui défini précédemment dans le fichier. Par exemple, considérez le **OutputRoot** propriétés.


[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]


Quand MSBuild traite le premier **OutputRoot** élément, en fournissant un paramètre portant le même nom n’a pas été fourni, il définit la valeur de la **OutputRoot** propriété **... \Publish\Out**. Lorsqu’il rencontre le second **OutputRoot** élément, si la condition prend la valeur **true**, elle remplace la valeur de la **OutputRoot** propriété avec la valeur de la **OutDir** paramètre.

L’élément suivant qui rencontre de MSBuild est un groupe d’éléments unique, contenant un élément nommé **ProjectsToBuild**.


[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]


MSBuild traite cette instruction en créant une liste d’éléments nommée **ProjectsToBuild**. Dans ce cas, la liste d’éléments contient une seule valeur&#x2014;le chemin d’accès et le nom du fichier solution.

À ce stade, les éléments restants sont des cibles. Cibles sont traitées différemment des propriétés et des éléments&#x2014;pour l’essentiel, cibles ne sont pas traités, sauf si elles sont explicitement spécifiés par l’utilisateur ou appelées par une autre construction dans le fichier projet. N’oubliez pas que l’ouverture **projet** balise inclut un **DefaultTargets** attribut.


[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]


Cela indique à MSBuild d’appeler le **FullPublish** cible, si les cibles ne sont pas spécifiées quand MSBuild.exe est appelée. Le **FullPublish** cible ne contient pas toutes les tâches ; au lieu de cela elle spécifie simplement une liste de dépendances.


[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]


Cette dépendance indique à MSBuild que dans l’ordre pour exécuter le **FullPublish** cible, il doit appeler cette liste de cibles dans l’ordre indiqué :

1. Il doit appeler la **Clean** cible.
2. Il doit appeler la **BuildProjects** cible.
3. Il doit appeler la **GatherPackagesForPublishing** cible.
4. Il doit appeler la **PublishDbPackages** cible.
5. Il doit appeler la **PublishWebPackages** cible.

### <a name="the-clean-target"></a>La cible Clean

Le **Clean** cible supprime en fait le répertoire de sortie et tout son contenu, la préparation d’une nouvelle génération.


[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]


Notez que la cible inclut un **ItemGroup** élément. Lorsque vous définissez les propriétés ou les éléments dans un **cible** élément, que vous créiez *dynamique* propriétés et des éléments. En d’autres termes, les propriétés ou les éléments ne sont pas traités tant que la cible est exécutée. Le répertoire de sortie ne peut pas existe ou contenir tous les fichiers jusqu'à ce que le processus de génération commence, afin que vous ne pouvez pas créer le  **\_FilesToDelete** liste comme un élément statique ; vous devez attendre jusqu'à ce que l’exécution est en cours d’exécution. Par conséquent, vous générez la liste comme un élément dynamique au sein de la cible.

> [!NOTE]
> Dans ce cas, étant donné que le **Clean** cible est le premier à être exécuté, il est inutile de réel à utiliser un groupe d’éléments dynamiques. Toutefois, il est recommandé d’utiliser des propriétés dynamiques et des éléments dans ce type de scénario, comme vous pouvez souhaiter exécuter des cibles dans un ordre différent à un moment donné.  
> Vous devez également veiller à éviter de déclarer les éléments qui ne seront jamais utilisés. Si vous disposez des éléments qui seront uniquement utilisés par une cible spécifique, envisagez de les placer à l’intérieur de la cible à supprimer toute charge inutile sur le processus de génération.


Dynamique des éléments mis à part, le **Clean** cible est assez simple et il utilise des intégrés **Message**, **supprimer**, et **RemoveDir**tâches pour :

1. Envoyer un message à l’enregistreur d’événements.
2. Créez une liste des fichiers à supprimer.
3. Supprimez les fichiers.
4. Supprimer le répertoire de sortie.

### <a name="the-buildprojects-target"></a>La cible BuildProjects

Le **BuildProjects** cible génère en fait tous les projets dans l’exemple de solution.


[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]


Cette cible a été décrit en détail dans la rubrique précédente, [présentation du fichier de projet](understanding-the-project-file.md), pour illustrer comment tâches et cibles référencent des propriétés et des éléments. À ce stade, vous êtes principalement intéressé la **MSBuild** tâche. Vous pouvez utiliser cette tâche pour générer plusieurs projets. La tâche ne crée pas une nouvelle instance de MSBuild.exe ; Il utilise l’instance actuelle en cours d’exécution pour générer chaque projet. Les points clés qui vous intéresse dans cet exemple sont les propriétés de déploiement :

- Le **DeployOnBuild** propriété indique à MSBuild à exécuter les instructions de déploiement dans les paramètres du projet lors de la génération de chaque projet est terminée.
- Le **DeployTarget** propriété identifie la cible que vous souhaitez appeler une fois que le projet est généré. Dans ce cas, le **Package** cible génère la sortie du projet dans un package déployable.

> [!NOTE]
> Le **Package** cible appelle Web Publishing Pipeline (WPP), qui fournit l’intégration entre MSBuild et Web Deploy. Si vous souhaitez examiner les cibles intégrées fournies par les fournisseurs de services, consultez le *Microsoft.Web.Publishing.targets* fichier dans le dossier de %\MSBuild\Microsoft\VisualStudio\v10.0\Web % PROGRAMFILES (x 86).


### <a name="the-gatherpackagesforpublishing-target"></a>La cible GatherPackagesForPublishing

Si vous étudiez la **GatherPackagesForPublishing** cible, vous remarquerez qu’il ne contient en fait de toutes les tâches. Au lieu de cela, elle contient un groupe d’éléments unique qui définit les trois éléments dynamiques.


[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]


Ces éléments font référence aux packages de déploiement qui ont été créées lorsque le **BuildProjects** cible a été exécutée. Vous n’a pas pu définir ces éléments statiquement dans le fichier projet, car les fichiers auquel font référence les éléments n’existent pas jusqu'à ce que le **BuildProjects** cible est exécutée. Au lieu de cela, les éléments doivent être définis dynamiquement dans une cible qui n’est pas appelée tant qu’après le **BuildProjects** cible est exécutée.

Les éléments ne sont pas utilisés dans cette cible&#x2014;cette cible génère simplement les éléments et les métadonnées associées à chaque valeur de l’élément. Une fois que ces éléments sont traités, le **PublishPackages** élément contiendra deux valeurs, le chemin d’accès à la *ContactManager.Mvc.deploy.cmd* fichier et le chemin d’accès à la  *ContactManager.Service.deploy.cmd* fichier. Web Deploy crée ces fichiers dans le cadre du package pour chaque projet web, et ce sont les fichiers que vous devez appeler sur le serveur de destination afin de déployer les packages. Si vous ouvrez un de ces fichiers, vous verrez en fait une commande MSDeploy.exe avec différentes valeurs de paramètre de build spécifique.

Le **DbPublishPackages** élément contient une valeur unique, le chemin d’accès à la *ContactManager.Database.deploymanifest* fichier.

> [!NOTE]
> Un fichier .deploymanifest est généré lorsque vous générez un projet de base de données, et il utilise le même schéma comme un fichier projet MSBuild. Il contient toutes les informations requises pour déployer une base de données, y compris l’emplacement du schéma de base de données (.dbschema) et les détails de tout script de prédéploiement et de post-déploiement. Pour plus d’informations, consultez [une vue d’ensemble de base de données Build and Deployment](https://msdn.microsoft.com/library/aa833165.aspx).


Vous en apprendrez davantage sur la façon dont les packages de déploiement et des manifestes de déploiement de base de données sont créées et utilisées dans [génération et empaquetage Web Application Projects](building-and-packaging-web-application-projects.md) et [déploiement de projets de base de données](deploying-database-projects.md).

### <a name="the-publishdbpackages-target"></a>La cible PublishDbPackages

Parler brièvement, le **PublishDbPackages** cible appelle l’utilitaire VSDBCMD pour déployer le **ContactManager** base de données à un environnement cible. Configuration du déploiement de base de données implique un grand nombre de décisions et nuances et vous en apprendrez davantage à ce sujet dans [déploiement de projets de base de données](deploying-database-projects.md) et [personnalisation des déploiements de base de données pour plusieurs environnements](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Dans cette rubrique, nous allons nous concentrer sur la façon dont fonctionne cette cible.

Tout d’abord, notez que la balise d’ouverture inclut un **sorties** attribut.


[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]


Il s’agit d’un exemple de *cible par lots*. Dans les fichiers de projet MSBuild, le traitement par lots est une technique pour itérer sur des collections. La valeur de la **sorties** attribut, **« % (DbPublishPackages.Identity) »**, fait référence à la **identité** propriété de métadonnées de la **DbPublishPackages**  liste d’éléments. Cette notation, **Outputs=%***(ItemList.ItemMetadataName)*, est traduit par :

- Fractionner les éléments de **DbPublishPackages** dans des lots d’éléments qui contiennent les mêmes **identité** valeur des métadonnées.
- Exécuter la cible une fois par lot.

> [!NOTE]
> **Identité** est un de la [les valeurs de métadonnées intégrées](https://msdn.microsoft.com/library/ms164313.aspx) qui est assigné à chaque élément lors de la création. Il fait référence à la valeur de la **Include** d’attribut dans le **élément** élément&#x2014;en d’autres termes, le chemin d’accès et le nom de l’élément.


Dans ce cas, car il ne doit jamais y avoir plus d’un élément avec le même chemin d’accès et nom de fichier, nous travaillons essentiellement avec de l’une tailles de lot. La cible est exécutée une seule fois pour chaque package de base de données.

Vous pouvez voir une notation similaire dans le  **\_Cmd** propriété, qui génère une commande VSDBCMD avec les commutateurs appropriés.


[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]


Dans ce cas, **%(DbPublishPackages.DatabaseConnectionString)**, **%(DbPublishPackages.TargetDatabase)**, et **%(DbPublishPackages.FullPath)** toutes faire référence à les valeurs de métadonnées de la **DbPublishPackages** collection d’éléments. Le  **\_Cmd** propriété est utilisée par le **Exec** tâche, qui appelle la commande.


[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]


À la suite de cette notation, le **Exec** tâche créera des lots en fonction des combinaisons uniques de la **DatabaseConnectionString**, **TargetDatabase**et **FullPath** valeurs de métadonnées et la tâche seront exécuté une fois pour chaque lot. Il s’agit d’un exemple de *tâche de traitement par lot*. Toutefois, étant donné que le traitement par lots du niveau de la cible a déjà divisé notre collection d’éléments en lots seul élément, le **Exec** tâche sera exécutée une seule fois et une seule fois pour chaque itération de la cible. En d’autres termes, cette tâche appelle l’utilitaire VSDBCMD une fois pour chaque package de base de données dans la solution.

> [!NOTE]
> Pour plus d’informations sur la cible et la tâche de traitement par lot, consultez MSBuild [le traitement par lot](https://msdn.microsoft.com/library/ms171473.aspx), [métadonnées d’éléments dans le traitement par lot cible](https://msdn.microsoft.com/library/ms228229.aspx), et [métadonnées d’éléments dans la tâche de traitement par lot](https://msdn.microsoft.com/library/ms171474.aspx).


### <a name="the-publishwebpackages-target"></a>La cible PublishWebPackages

À ce stade, vous avez appelé la **BuildProjects** cible, qui génère un package de déploiement web pour chaque projet dans l’exemple de solution. Chaque package d’accompagnement est un *deploy.cmd* fichier qui contient les commandes de MSDeploy.exe nécessaires pour déployer le package à l’environnement cible, et un *SetParameters.xml* fichier, qui spécifie le informations nécessaires de l’environnement cible. Vous avez également appelées le **GatherPackagesForPublishing** cible, qui génère un élément de collection contenant les *deploy.cmd* vous êtes intéressé par les fichiers. Essentiellement, le **PublishWebPackages** cible exécute ces fonctions :

- Il manipule le *SetParameters.xml* fichier pour chaque package inclure les détails corrects de l’environnement cible, à l’aide de la **XmlPoke** tâche.
- Il appelle le *deploy.cmd* fichier pour chaque package, en utilisant les commutateurs appropriés.

Tout comme le **PublishDbPackages** cible, le **PublishWebPackages** cible utilise le traitement par lot par cible pour vous assurer que la cible est exécutée une fois pour chaque package web.


[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]


Dans la cible, le **Exec** tâche est utilisée pour exécuter le *deploy.cmd* fichier pour chaque package web.


[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]


Pour plus d’informations sur la configuration du déploiement de packages web, consultez [génération et empaquetage Web Application Projects](building-and-packaging-web-application-projects.md).

## <a name="conclusion"></a>Conclusion

Cette rubrique fourni une procédure pas à pas de l’utilisation des fichiers de projet de fractionnement pour contrôler le processus de génération et de déploiement à partir du début à la fin de l’exemple de solution de gestionnaire de contacts. Cette approche vous permet de vous exécutez complexe, les déploiements d’échelle de l’entreprise dans une étape unique, reproductible, simplement en exécutant un fichier de commandes propre à l’environnement.

## <a name="further-reading"></a>informations supplémentaires

Pour une présentation plus approfondie des fichiers projet et les fournisseurs de services, consultez [à l’intérieur de la Microsoft Build Engine : utilisation de MSBuild et Team Foundation Build](http://amzn.com/0735645248) par Sayed Ibrahim Hashimi et William Bartholomew, ISBN : 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Précédent](understanding-the-project-file.md)
> [Suivant](building-and-packaging-web-application-projects.md)
