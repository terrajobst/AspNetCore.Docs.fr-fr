---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: Présentation du fichier de projet | Microsoft Docs
author: jrjlee
description: Fichiers de projet Microsoft Build Engine (MSBuild) se trouvent au cœur du processus de génération et de déploiement. Cette rubrique commence par une vue d’ensemble conceptuelle de MSBuild...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 89c5c7906ccfc453195b788cbc6393dc74cda1fb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377202"
---
<a name="understanding-the-project-file"></a>Présentation du fichier de projet
====================
par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Fichiers de projet Microsoft Build Engine (MSBuild) se trouvent au cœur du processus de génération et de déploiement. Cette rubrique commence par une vue d’ensemble conceptuelle de MSBuild et le fichier projet. Il décrit les composants clés que vous allez rencontrer lorsque vous travaillez avec des fichiers projet, et il passe à travers un exemple de comment vous pouvez utiliser des fichiers projet pour déployer des applications du monde réel.
> 
> Ce que vous allez apprendre :
> 
> - Comment MSBuild utilise des fichiers projet MSBuild pour générer des projets.
> - Comment MSBuild s’intègre aux technologies de déploiement, comme l’outil de déploiement Web Internet Information Services (IIS) (Web Deploy).
> - Comment comprendre les composants clés d’un fichier projet.
> - Comment vous pouvez utiliser des fichiers de projet pour créer et déployer des applications complexes.


## <a name="msbuild-and-the-project-file"></a>MSBuild et le fichier de projet

Lorsque vous créez et créez des solutions dans Visual Studio, Visual Studio utilise MSBuild pour générer chaque projet dans votre solution. Chaque projet Visual Studio inclut un fichier de projet MSBuild, avec une extension de fichier qui reflète le type de projet&#x2014;par exemple, un projet c# (.csproj), un projet Visual Basic.NET (.vbproj) ou un projet de base de données (.dbproj). Pour pouvoir générer un projet, MSBuild doit traiter le fichier projet associé au projet. Le fichier projet est un document XML qui contient toutes les informations et instructions que MSBuild a besoin pour générer votre projet, telles que le contenu à inclure, exigences de plates-formes, informations de versioning, serveur web ou paramètres du serveur de base de données et le tâches qui doivent être réalisées.

Fichiers projet MSBuild sont basées sur le [schéma XML MSBuild](https://msdn.microsoft.com/library/5dy88c2e.aspx), et ainsi le processus de génération est entièrement ouverte et transparente. En outre, vous n’avez pas besoin d’installer Visual Studio pour pouvoir utiliser le moteur MSBuild&#x2014;l’exécutable de MSBuild.exe fait partie du .NET Framework, et vous pouvez l’exécuter à partir d’une invite de commandes. En tant que développeur, vous pouvez créer vos propres fichiers de projet MSBuild, à l’aide du schéma XML MSBuild, d’imposer un contrôle sophistiqué et précis sur la façon dont vos projets sont générés et déployés. Ces fichiers de projet personnalisés fonctionnent dans la même façon que les fichiers de projet Visual Studio génère automatiquement.

> [!NOTE]
> Vous pouvez également utiliser des fichiers projet MSBuild avec le service Team Build dans Team Foundation Server (TFS). Par exemple, vous pouvez utiliser des fichiers de projet dans les scénarios d’intégration continue (CI) pour automatiser le déploiement dans un environnement de test lorsque le nouveau code est archivé. Pour plus d’informations, consultez [configuration Team Foundation Server pour le déploiement de Web automatisée](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).


### <a name="project-file-naming-conventions"></a>Conventions d’affectation de noms de fichier de projet

Lorsque vous créez vos propres fichiers de projet, vous pouvez utiliser n’importe quelle extension de fichier que. Toutefois, pour rendre vos solutions plus facile à comprendre, vous devez utiliser ces conventions courantes :

- Utilisez l’extension .proj lorsque vous créez un fichier de projet qui génère des projets.
- Utiliser l’extension .targets lorsque vous créez un fichier de projet réutilisable à importer dans d’autres fichiers de projet. Fichiers portant l’extension .targets en général, ne créez pas quoi que ce soit elles-mêmes, qu’ils contiennent simplement les instructions que vous pouvez importer dans vos fichiers .proj.

### <a name="integration-with-deployment-technologies"></a>Intégration aux Technologies de déploiement

Si vous avez travaillé avec les projets d’application web dans Visual Studio 2010, telles que les applications web ASP.NET et les applications web ASP.NET MVC, vous saurez que ces projets incluent une prise en charge intégrée pour l’empaquetage et déploiement de l’application web à un environnement cible. Le **propriétés** pour ces projets incluent des pages **Package/Publication Web** et **Package/Publication SQL** onglets que vous pouvez utiliser pour configurer comment les composants de votre application sont empaquetées et déployées. Cet exemple montre la **Package/Publication Web** onglet :

![](understanding-the-project-file/_static/image1.png)

La technologie sous-jacente derrière ces fonctionnalités est connue en tant que le Pipeline de publication Web (WPP). Les fournisseurs de services apporte essentiellement MSBuild et [Web Deploy](https://go.microsoft.com/?linkid=9805122) ensemble pour fournir un processus de génération, déploiement du package et complète pour vos applications web.

La bonne nouvelle est que vous pouvez tirer parti des points d’intégration fournies par les fournisseurs de services lorsque vous créez des fichiers de projet personnalisés pour les projets web. Vous pouvez inclure des instructions de déploiement dans votre fichier projet, ce qui vous permet de générer vos projets et créer des packages de déploiement web installer ces packages sur des serveurs distants via un seul fichier projet et un seul appel à MSBuild. Vous pouvez également appeler les autres fichiers exécutables que dans le cadre de votre processus de génération. Par exemple, vous pouvez exécuter l’outil de ligne de commande VSDBCMD.exe pour déployer une base de données à partir d’un fichier de schéma. Au cours de cette rubrique, vous verrez comment vous pouvez tirer parti de ces fonctionnalités pour satisfaire les besoins de vos scénarios de déploiement d’entreprise.

> [!NOTE]
> Pour plus d’informations sur comment fonctionne le processus de déploiement d’application web, consultez [vue d’ensemble du déploiement de projet d’Application ASP.NET Web](https://msdn.microsoft.com/library/dd394698.aspx).


## <a name="the-anatomy-of-a-project-file"></a>Anatomie d’un fichier de projet

Avant d’examiner plus en détail le processus de génération, il est important de prendre quelques instants pour vous familiariser avec la structure de base d’un fichier projet MSBuild. Cette section fournit une vue d’ensemble des éléments plus communs que vous allez rencontrer lorsque vous passez en revue, modifiez ou créez un fichier projet. En particulier, vous apprendrez à :

- Comment utiliser *propriétés* pour gérer les variables pour le processus de génération.
- Comment utiliser *éléments* pour identifier les entrées dans le processus de génération, telles que les fichiers de code.
- Comment utiliser *cibles* et *tâches* pour fournir des instructions d’exécution à MSBuild, à l’aide de *propriétés* et *éléments* définie ailleurs dans le fichier projet.

Cet exemple montre la relation entre les éléments clés dans un fichier projet MSBuild :

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a>L’élément de projet

Le [projet](https://msdn.microsoft.com/library/bcxfsh87.aspx) élément est l’élément racine de chaque fichier de projet. En plus d’identifier le schéma XML pour le fichier projet, le **projet** élément peut inclure des attributs pour spécifier les points d’entrée pour le processus de génération. Par exemple, dans le [solution d’exemple de gestionnaire de contacts](the-contact-manager-solution.md), le *Publish.proj* fichier Spécifie que la build doit démarrer en appelant la cible nommée **FullPublish**.


[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]


### <a name="properties-and-conditions"></a>Propriétés et des Conditions

Un fichier de projet doit généralement fournir un grand nombre d’éléments d’information afin de créer et déployer vos projets différents. Ces éléments d’information peut inclure des noms de serveur, les chaînes de connexion, informations d’identification, les configurations de build, chemins d’accès de fichier source et de destination et toute autre information que vous souhaitez inclure pour prendre en charge la personnalisation. Dans un fichier de projet, les propriétés doivent être définies dans un [PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx) élément. Propriétés MSBuild sont constitués de paires clé-valeur. Dans le **PropertyGroup** élément, le nom d’élément définit la clé de propriété et le contenu de l’élément définit la valeur de propriété. Par exemple, vous pouvez définir des propriétés nommées **nom_serveur** et **ConnectionString** pour stocker une chaîne de connexion et de nom de serveur statique.


[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]


Pour récupérer une valeur de propriété, vous utilisez le format <strong>$(</strong><em>PropertyName</em><strong>)</strong><em>.</em> Par exemple, pour récupérer la valeur de la <strong>nom_serveur</strong> propriété, vous devez taper :


[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]


> [!NOTE]
> Vous verrez des exemples d’et quand utiliser les valeurs de propriété plus loin dans cette rubrique.


Incorporation d’informations en tant que propriétés statiques dans un fichier projet n’est pas toujours l’approche idéale pour la gestion du processus de génération. Dans de nombreux scénarios, vous souhaitez obtenir les informations provenant d’autres sources, ou permettre à l’utilisateur à fournir les informations à partir de l’invite de commandes. MSBuild vous permet de spécifier n’importe quelle valeur de propriété comme un paramètre de ligne de commande. Par exemple, l’utilisateur peut fournir une valeur pour **nom_serveur** quand il ou elle exécute MSBuild.exe à partir de la ligne de commande.


[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]


> [!NOTE]
> Pour plus d’informations sur les arguments et les commutateurs que vous pouvez utiliser avec MSBuild.exe, consultez [référence de ligne de commande MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).


Vous pouvez utiliser la même syntaxe de propriété pour obtenir les valeurs des variables d’environnement et des propriétés de projet intégrés. Un grand nombre de propriétés couramment utilisées est défini pour vous, et vous pouvez les utiliser dans vos fichiers projet en incluant le nom de paramètre concerné. Par exemple, pour récupérer la plateforme de projet actuelle&#x2014;, par exemple, **x86** ou **AnyCpu**&#x2014;vous pouvez inclure le **$(Platform)** référence de propriété dans votre fichier projet. Pour plus d’informations, consultez [Macros pour les commandes de génération et les propriétés](https://msdn.microsoft.com/library/c02as0cs.aspx), [propriétés communes des projets MSBuild](https://msdn.microsoft.com/library/bb629394.aspx), et [propriétés réservées](https://msdn.microsoft.com/library/ms164309.aspx).

Propriétés sont souvent utilisées conjointement avec *conditions*. La plupart des éléments MSBuild prennent en charge la **Condition** attribut, qui vous permet de spécifier les critères sur lesquels MSBuild doit évaluer l’élément. Par exemple, considérez cette définition de propriété :


[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]


Quand MSBuild traite cette définition de propriété, il vérifie d’abord pour voir si un **$(OutputRoot)** valeur de propriété n’est disponible. Si la valeur de propriété est vide&#x2014;en d’autres termes, l’utilisateur n’a pas fourni une valeur pour cette propriété&#x2014;la condition prend la valeur **true** et la valeur de propriété est définie sur **... \Publish\Out**. Si l’utilisateur a fourni une valeur pour cette propriété, la condition prend la valeur **false** et la valeur de propriété statique n’est pas utilisée.

Pour plus d’informations sur les différentes façons dans lequel vous pouvez spécifier des conditions, consultez [Conditions MSBuild](https://msdn.microsoft.com/library/7szfhaft.aspx).

### <a name="items-and-item-groups"></a>Éléments et des groupes d’éléments

Un des rôles importants du fichier projet consiste à définir les entrées dans le processus de génération. En règle générale, ces entrées sont des fichiers&#x2014;code des fichiers, les fichiers de configuration, les fichiers de commandes et tous les autres fichiers dont vous avez besoin pour traiter ou de copier en tant que partie du processus de génération. Dans le schéma de projet MSBuild, ces entrées sont représentées par [élément](https://msdn.microsoft.com/library/ms164283.aspx) éléments. Dans un fichier de projet, les éléments doivent être définies dans un [ItemGroup](https://msdn.microsoft.com/library/646dk05y.aspx) élément. Tout comme **propriété** éléments, vous pouvez nommer un **élément** élément comme vous le souhaitez. Toutefois, vous devez spécifier un **Include** attribut pour identifier le fichier ou le caractère générique qui représente l’élément.


[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]


En spécifiant plusieurs **élément** éléments portant le même nom, que vous créez en fait une liste nommée des ressources. Un bon moyen de voir comment cela fonctionne consiste à jeter un coup de œil à l’intérieur d’un des fichiers du projet créé par Visual Studio. Par exemple, le *ContactManager.Mvc.csproj* le fichier dans l’exemple de solution inclut un grand nombre de groupes d’éléments, chacun avec plusieurs nom identique **élément** éléments.


[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]


De cette façon, le fichier projet est Indiquez à MSBuild pour construire des listes de fichiers qui doivent être traitées de la même façon&#x2014;le **référence** liste inclut des assemblys qui doivent être en place pour une génération réussie, le  **Compiler** liste inclut les fichiers de code qui doivent être compilés, et le **contenu** liste contient des ressources qui doivent être copiés sans modification. Nous allons examiner comment le processus de génération fait référence et utilise ces éléments plus loin dans cette rubrique.

Éléments item peuvent également inclure [ItemMetadata](https://msdn.microsoft.com/library/ms164284.aspx) éléments enfants. Celles-ci sont définies par l’utilisateur des paires clé-valeur et essentiellement représentent les propriétés qui sont spécifiques à cet élément. Par exemple, un grand nombre de la **compiler** incluent des éléments dans le fichier projet **DependentUpon** éléments enfants.


[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]


> [!NOTE]
> En plus des métadonnées de l’élément créée par l’utilisateur, tous les éléments sont affectés à différentes métadonnées communes lors de la création. Pour plus d’informations, consultez l’article [Well-known Item Metadata (Métadonnées d’élément connues MSBuild)](https://msdn.microsoft.com/library/ms164313.aspx).


Vous pouvez créer **ItemGroup** éléments dans le niveau racine **projet** élément ou au sein de spécifiques **cible** éléments. **ItemGroup** éléments prennent également en charge **Condition** attributs, qui vous permet de personnaliser les entrées dans le processus de génération en fonction des conditions telles que la configuration de projet ou de la plateforme.

### <a name="targets-and-tasks"></a>Cibles et tâches

Dans le schéma MSBuild, un [tâche](https://msdn.microsoft.com/library/77f2hx1s.aspx) élément représente une instruction build individuel (ou une tâche). MSBuild inclut une multitude de tâches prédéfinies. Exemple :

- Le **copie** tâche copie des fichiers vers un nouvel emplacement.
- Le **Csc** tâche appelle le compilateur Visual c#.
- Le **Vbc** tâche appelle le compilateur Visual Basic.
- Le **Exec** tâche exécute un programme spécifié.
- Le **Message** tâche écrit un message dans un enregistreur d’événements.

> [!NOTE]
> Pour obtenir des informations complètes sur les tâches qui sont disponibles prêtes à l’emploi, consultez [référence des tâches MSBuild](https://msdn.microsoft.com/library/7z253716.aspx). Pour plus d’informations sur les tâches, y compris comment créer vos propres tâches personnalisées, consultez [tâches MSBuild](https://msdn.microsoft.com/library/ms171466.aspx).


Tâches doivent toujours être contenues dans [cible](https://msdn.microsoft.com/library/t50z2hka.aspx) éléments. Un **cible** élément est un ensemble d’une ou plusieurs tâches sont exécutées séquentiellement, et un fichier de projet peut contenir plusieurs cibles. Lorsque vous souhaitez exécuter une tâche ou un ensemble de tâches, vous appelez la cible qui les contient. Par exemple, supposons que vous disposez d’un fichier de projet simple qui enregistre un message.


[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]


Vous pouvez appeler la cible à partir de la ligne de commande à l’aide de la **/t** commutateur pour spécifier la cible.


[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]


Vous pouvez également ajouter un **DefaultTargets** attribut le **projet** élément, pour spécifier les cibles que vous souhaitez appeler.


[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]


Dans ce cas, vous n’avez pas besoin de spécifier la cible à partir de la ligne de commande. Vous pouvez spécifier simplement le fichier projet et MSBuild appellera le **FullPublish** cible pour vous.


[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]


Cibles et tâches peuvent inclure **Condition** attributs. Par conséquent, vous pouvez choisir d’omettre des cibles entières ou des tâches individuelles si certaines conditions sont remplies.

En règle générale, lorsque vous créez des tâches utiles et cibles, vous devez faire référence aux propriétés et aux éléments que vous avez défini ailleurs dans le fichier projet :

- Pour utiliser une valeur de propriété, tapez <strong>$(</strong><em>PropertyName</em><strong>)</strong>, où <em>PropertyName</em> est le nom de la <strong>propriété</strong> élément ou le nom du paramètre.
- Pour utiliser un élément, tapez <strong>@(</strong><em>ItemName</em><strong>)</strong>, où <em>ItemName</em> est le nom de la <strong>élément</strong> élément.

> [!NOTE]
> N’oubliez pas que si vous créez plusieurs éléments portant le même nom, vous créez une liste. En revanche, si vous créez plusieurs propriétés portant le même nom, la dernière valeur de propriété que vous fournissez remplacera toutes les propriétés précédentes portant le même nom&#x2014;une propriété peut contenir uniquement une valeur unique.


Par exemple, dans le *Publish.proj* de fichiers dans l’exemple de solution, jetez un coup de œil à la **BuildProjects** cible.


[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]


Dans cet exemple, vous pouvez observer ces points clés :

- Si le **BuildingInTeamBuild** paramètre est spécifié et a la valeur **true**, aucune des tâches au sein de cette cible sera exécuté.
- La cible contient une seule instance de la [MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx) tâche. Cette tâche vous permet de créer d’autres projets MSBuild.
- Le **ProjectsToBuild** élément est passé à la tâche. Cet élément peut représenter une liste de fichiers projet ou solution, tout définis par **ProjectsToBuild** des éléments au sein d’un groupe d’éléments item. Dans ce cas, le **ProjectsToBuild** élément fait référence à un fichier de solution unique.

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- Les valeurs de propriété passées à la **MSBuild** tâches incluent des paramètres nommés **OutputRoot** et **Configuration**. Ces propriétés sont définies pour les valeurs des paramètres si elles sont fournies, ou les valeurs de propriété statique si elles ne sont pas.

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

Vous pouvez également voir que le **MSBuild** tâche appelle une cible nommée **Build**. C’est une de plusieurs cibles intégrées qui sont largement utilisées dans les fichiers de projet Visual Studio et sont disponible dans vos fichiers de projet personnalisé, comme **Build**, **Clean**, **reconstruire**, et **publier**. Vous en apprendrez davantage sur l’utilisation de cibles et des tâches pour contrôler le processus de génération, ainsi que sur le **MSBuild** de tâches, en particulier, plus loin dans cette rubrique.

> [!NOTE]
> Pour plus d’informations sur les cibles, consultez [cibles MSBuild](https://msdn.microsoft.com/library/ms171462.aspx).


## <a name="splitting-project-files-to-support-multiple-environments"></a>Fractionner des fichiers de projet pour prendre en charge plusieurs environnements

Supposons que vous souhaitez être en mesure de déployer une solution dans plusieurs environnements, tels que les serveurs de test, intermédiaires plateformes et les environnements de production. La configuration peut varier considérablement entre ces environnements&#x2014;non seulement en termes de noms de serveur, chaînes de connexion et ainsi de suite, mais aussi potentiellement en termes d’informations d’identification, paramètres de sécurité et bien d’autres facteurs. Si vous devez faire cela régulièrement, il n’est pas vraiment utile de modifier plusieurs propriétés de votre fichier projet chaque fois que vous changez l’environnement cible. Et n’est pas une solution idéale pour exiger une liste sans fin des valeurs de propriété doit être fourni pour le processus de génération.

Heureusement il existe une alternative. MSBuild vous permet de fractionner votre configuration de build dans plusieurs fichiers de projet. Pour voir comment cela fonctionne, dans l’exemple de solution, notez qu’il existe deux fichiers de projet personnalisé :

- *Publish.proj*, qui contient des propriétés, éléments, et les cibles qui sont communes à tous les environnements.
- *Env-Dev.proj*, qui contient des propriétés qui sont spécifiques à un environnement de développement.

Maintenant Notez que le *Publish.proj* fichier inclut une [importation](https://msdn.microsoft.com/library/92x05xfs.aspx) élément, immédiatement sous l’accolade ouvrante **projet** balise.


[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]


Le **importer** élément est utilisé pour importer le contenu d’un autre fichier de projet MSBuild dans le fichier de projet MSBuild en cours. Dans ce cas, le **TargetEnvPropsFile** paramètre fournit le nom de fichier du fichier projet à importer. Vous pouvez fournir une valeur pour ce paramètre lorsque vous exécutez MSBuild.


[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]


Ce efficace fusionne le contenu des deux fichiers dans un seul fichier projet. À l’aide de cette approche, vous pouvez créer un fichier de projet qui contient votre configuration de build universelle et plusieurs fichiers de projet supplémentaires qui contient les propriétés spécifiques à l’environnement. Par conséquent, il vous suffit d’exécuter une commande avec une valeur de paramètre différente vous permet de déployer votre solution vers un autre environnement.

![](understanding-the-project-file/_static/image3.png)

Fractionnement de vos fichiers de projet de cette façon est une bonne pratique à suivre. Il permet aux développeurs de déployer dans plusieurs environnements en exécutant une commande unique, tout en évitant la duplication des propriétés de build universel dans plusieurs fichiers de projet.

> [!NOTE]
> Pour obtenir des conseils sur la façon de personnaliser les fichiers de projet spécifique à l’environnement pour vos propres environnements serveur, consultez [configuration des propriétés de déploiement pour un environnement cible](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="conclusion"></a>Conclusion

Cette rubrique fourni une introduction générale aux fichiers projet MSBuild et expliqué comment vous pouvez créer vos propres fichiers de projet personnalisé pour contrôler le processus de génération. Il a également introduit le concept de fractionner des fichiers projet dans les instructions de génération universelle et propriétés spécifiques à l’environnement build, pour faciliter le travail générer et déployer des projets vers plusieurs destinations.

La rubrique suivante, [comprendre le processus de génération](understanding-the-build-process.md), fournit plus d’informations sur comment vous pouvez utiliser des fichiers de projet pour la génération du contrôle et de déploiement en vous guidant à travers le déploiement d’une solution avec un niveau réaliste de complexité.

## <a name="further-reading"></a>informations supplémentaires

Pour une présentation plus approfondie des fichiers projet et les fournisseurs de services, consultez [à l’intérieur de la Microsoft Build Engine : utilisation de MSBuild et Team Foundation Build](http://amzn.com/0735645248) par Sayed Ibrahim Hashimi et William Bartholomew, ISBN : 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Précédent](setting-up-the-contact-manager-solution.md)
> [Suivant](understanding-the-build-process.md)
