---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Création et exécution d’un déploiement de fichier de commandes | Microsoft Docs
author: jrjlee
description: Cette rubrique décrit comment créer un fichier de commandes qui vous permettent d’exécuter un déploiement à l’aide de fichiers de projet Microsoft Build Engine (MSBuild) en tant qu’une seule étape, re...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: 6b2a75e0740f648a3a6e4f39c00bd30609325728
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830300"
---
<a name="creating-and-running-a-deployment-command-file"></a>Création et exécution d’un fichier de commande de déploiement
====================
par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique décrit comment créer un fichier de commandes qui vous permettent d’exécuter un déploiement à l’aide de fichiers de projet Microsoft Build Engine (MSBuild) comme un processus pas à pas et reproductible.


Cette rubrique fait partie d’une série de didacticiels basées sur les exigences de déploiement d’entreprise de la société fictive Fabrikam, Inc. Cette série de didacticiels utilise un exemple de solution&#x2014;le [Contact Manager](the-contact-manager-solution.md) solution&#x2014;pour représenter une application web avec un niveau réaliste de complexité, y compris une application ASP.NET MVC 3, une Communication de Windows Foundation (WCF) service et un projet de base de données.

La méthode de déploiement au cœur de ces didacticiels est basée sur l’approche de fichier de projet de fractionnement décrite dans [comprendre le processus de génération](understanding-the-build-process.md), dans lequel le processus de génération est contrôlé par deux fichiers de projet&#x2014;contenant un seul les instructions qui s’appliquent à chaque environnement de destination et celui qui contient les paramètres de génération et de déploiement spécifiques à l’environnement de génération. Au moment de la génération, le fichier de projet spécifique à l’environnement est fusionné dans le fichier de projet d’indépendant de l’environnement pour former un ensemble complet d’instructions de génération.

## <a name="process-overview"></a>Vue d’ensemble du processus

Dans cette rubrique, vous allez apprendre à créer et exécuter un fichier de commande qui utilise ces fichiers de projet pour effectuer un déploiement reproductible sur votre environnement cible. Essentiellement, le fichier de commandes doit simplement contenir une commande MSBuild qui :

- Indique à MSBuild d’exécuter l’indépendant de l’environnement *Publish.proj* fichier.
- Indique le *Publish.proj* fichier fichier qui contient les paramètres spécifiques à l’environnement et où les trouver.

## <a name="create-an-msbuild-command"></a>Créer une commande MSBuild

Comme décrit dans [comprendre le processus de génération](understanding-the-build-process.md), le fichier de projet spécifique à un environnement&#x2014;, par exemple, *Env-Dev.proj*&#x2014;est conçu pour être importé dans l’environnement agnostique *Publish.proj* fichier au moment de la génération. Ensemble, ces deux fichiers fournissent un ensemble complet d’instructions qui indiquent comment générer et déployer votre solution à MSBuild.

Le *Publish.proj* fichier utilise un **importer** élément pour importer le fichier de projet spécifique à l’environnement.


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


Par conséquent, lorsque vous utilisez MSBuild.exe pour générer et déployer la solution Gestionnaire de contacts, vous devez :

- Exécutez MSBuild.exe le *Publish.proj* fichier.
- Spécifiez l’emplacement du fichier de projet spécifique à un environnement en fournissant un paramètre de ligne de commande nommé **TargetEnvPropsFile**.

Pour ce faire, votre commande MSBuild doit ressembler à ceci :


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


À ce stade, il est une étape simple pour déplacer vers un déploiement reproductible et à étape unique. Il vous suffit de faire consiste à ajouter votre commande de MSBuild dans un fichier .cmd. Dans la solution Gestionnaire de contacts, le dossier de publication inclut un fichier nommé *publier-Dev.cmd* qui effectue exactement cela.


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> Le **/fl** commutateur indique à MSBuild pour créer un fichier journal nommé *msbuild.log* dans le répertoire de travail dans lequel MSBuild.exe a été appelée.


Pour déployer ou de redéployer la solution Gestionnaire de contacts, il vous suffit est exécuté le *publier-Dev.cmd* fichier. Lorsque vous exécutez le fichier, MSBuild va :

- Générer tous les projets de la solution.
- Générer des packages web pouvant être déployées pour les projets d’application web.
- Générer des fichiers .dbschema et .deploymanifest pour les projets de base de données.
- Déployer les packages web sur le serveur web.
- Déployer la base de données sur le serveur de base de données.

## <a name="run-the-deployment"></a>Exécuter le déploiement

Lorsque vous avez créé un fichier de commandes pour votre environnement cible, il se peut que vous devez être en mesure d’effectuer la totalité du déploiement en exécutant simplement le fichier.

**Pour déployer la solution de gestionnaire de contacts dans votre environnement de test**

1. Sur votre station de travail de développeur, ouvrez l’Explorateur Windows, puis accédez à l’emplacement de la *publier-Dev.cmd* fichier.
2. Double-cliquez sur le fichier pour l’exécuter.
3. Si un **ouvrir un fichier – Avertissement de sécurité** boîte de dialogue s’affiche, cliquez sur **exécuter**.
4. Si vos paramètres de configuration et les serveurs de test sont configurés correctement, la fenêtre d’invite de commandes affichera un **Build réussie** message lorsque MSBuild a fini de traiter les fichiers de projet.

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. S’il s’agit de la première fois que vous avez déployé la solution dans cet environnement, vous devez ajouter le compte d’ordinateur du serveur test web pour le **db\_datawriter** et **db\_datareader**rôles sur le **ContactManager** base de données. Cette procédure est décrite dans [configurer un serveur de base de données pour déployer de publication Web](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

    > [!NOTE]
    > Il vous suffit de vous affecter ces autorisations lorsque vous créez la base de données. Par défaut, le processus de génération ne recrée pas la base de données sur chaque déploiement&#x2014;au lieu de cela, il compare la base de données existante pour le schéma le plus récent et assurez-vous que les modifications nécessaires. Par conséquent, vous devez uniquement mapper ces rôles de base de données la première fois que vous déployez la solution.
6. Ouvrez Internet Explorer et accédez à l’URL de l’application Gestionnaire de contacts (par exemple, `http://testweb1:85/ContactManager/`).
7. Vérifiez que l’application fonctionne comme prévu et que vous êtes en mesure d’ajouter des contacts.

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>Conclusion

Création d’un fichier de commandes qui contient vos instructions MSBuild vous offre un moyen rapide et facile de créer et déployer une solution à projets multiples dans un environnement de destination spécifique. Si vous avez besoin déployer votre solution de manière répétée dans plusieurs environnements de destination, vous pouvez créer plusieurs fichiers de commandes. Chaque fichier de commandes, la commande MSBuild génère le même fichier de projet d’application universelle, mais elle spécifie un fichier de projet spécifiques à l’environnement différent. Par exemple, un fichier de commandes pour publier sur un développeur ou l’environnement de test peut contenir cette commande MSBuild :


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


Un fichier de commandes pour publier dans un environnement intermédiaire peut contenir cette commande MSBuild :


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> Pour obtenir des conseils sur la façon de personnaliser les fichiers de projet spécifique à l’environnement pour vos propres environnements serveur, consultez [configurer les propriétés de déploiement pour un environnement cible](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


Vous pouvez également personnaliser le processus de génération pour chaque environnement en substituant les propriétés ou en définissant plusieurs autres commutateurs dans votre commande MSBuild. Pour plus d’informations, consultez [référence de ligne de commande MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

> [!div class="step-by-step"]
> [Précédent](deploying-database-projects.md)
> [Suivant](manually-installing-web-packages.md)
