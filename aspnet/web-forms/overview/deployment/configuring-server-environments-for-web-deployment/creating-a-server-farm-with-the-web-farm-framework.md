---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
title: Création d’une batterie de serveurs avec l’infrastructure de batterie de serveurs Web | Microsoft Docs
author: jrjlee
description: Cette rubrique décrit l’utilisation de Web Farm Framework (WFF) 2.0 pour créer et configurer une batterie de serveurs web à partir d’une collection de serveurs.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 656dd06d-806c-467c-863d-9fc45e5ba3ab
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
msc.type: authoredcontent
ms.openlocfilehash: 5fdd1088f1f4e21f5e79b8c8379b7865f825ad53
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808938"
---
<a name="creating-a-server-farm-with-the-web-farm-framework"></a>Création d’une batterie de serveurs avec l’infrastructure de batterie de serveurs Web
====================
par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Cette rubrique décrit l’utilisation de Web Farm Framework (WFF) 2.0 pour créer et configurer une batterie de serveurs web à partir d’une collection de serveurs.


WFF vous permet de synchroniser des produits de la plate-forme web et les composants, les applications web, les sites Web et les paramètres de configuration sur plusieurs serveurs web avec équilibrage de charge. Dans les scénarios où vous avez besoin de plusieurs serveurs web, comme les environnements intermédiaire et de production, cela peut considérablement simplifier votre processus de déploiement et la configuration. Vous pouvez déployer une application web à un seul serveur&#x2014;le *serveur principal*&#x2014;et WFF vont être répliqués automatiquement cette application web sur tous les autres serveurs de web dans la batterie de serveurs.

## <a name="understanding-the-web-farm-framework"></a>Présentation de l’infrastructure de batterie de serveurs Web

Vous pouvez utiliser WFF 2.0 pour configurer, gérer et déployer du contenu à un groupe de serveurs web. Un déploiement WFF se compose de trois rôles de serveur principaux :

- Le *serveur contrôleur*. Vous utilisez ce serveur pour créer et configurer des batteries de serveurs WFF. Le serveur de contrôleur gère la synchronisation des composants de la plateforme web, des paramètres de configuration et des applications entre les serveurs web dans une batterie de serveurs. Vous installez WFF 2.0 sur le serveur de contrôleur, et le serveur de contrôleur à son tour installera l’agent WFF sur chacun des serveurs dans une batterie de serveurs. Le serveur de contrôleur n’appartient pas sur le plan conceptuel à une batterie de serveurs WFF et un serveur de contrôleur unique peut gérer plusieurs batteries de serveurs. Dans ce scénario, vous utilisez un seul serveur de contrôleur WFF pour créer et gérer la batterie de serveurs intermédiaires et de la batterie de serveurs de production.
- Le *serveur principal*. Chaque batterie de serveurs WFF inclut un seul serveur principal. Lorsque vous installez les composants de la plateforme web ou déployez des applications sur le serveur principal, le WFF synchronise vos modifications à tous les autres serveurs dans la batterie de serveurs.
- Le *serveur secondaire*. Chaque batterie de serveurs WFF inclut un ou plusieurs serveurs secondaires. Toute modification apportée au serveur principal est répliquées sur chaque serveur secondaire au sein de la batterie de serveurs.

Cet exemple montre comment ces rôles de serveur sont liés pour les environnements intermédiaire et de production de Fabrikam, Inc. :

![](creating-a-server-farm-with-the-web-farm-framework/_static/image1.png)

Dans ce scénario, l’environnement intermédiaire et l’environnement de production sont configurés en tant que les batteries de serveurs WFF. Un seul serveur de contrôleur WFF gère deux batteries de serveurs. Dans chaque batterie de serveurs, toutes les modifications vers le serveur principal sont répliquées vers chaque serveur secondaire.

Avant de commencer à configurer vos environnements intermédiaire et de production, nous vous recommandons de lire ces articles pour vous familiariser avec les concepts clés de WFF 2.0 :

- [Vue d’ensemble de l’infrastructure de batterie de serveurs Web 2.0 pour IIS 7](https://go.microsoft.com/?linkid=9805126)
- [Configuration d’une batterie de serveurs avec le Framework de batterie de serveurs Web 2.0 pour IIS 7](https://go.microsoft.com/?linkid=9805127)
- [Spécifications de plateforme pour l’infrastructure de batterie de serveurs Web 2.0 pour IIS 7 et du système](https://go.microsoft.com/?linkid=9805128)

## <a name="task-overview"></a>Présentation de la tâche

Pour effectuer les tâches et les procédures pas à pas dans cette rubrique, vous aurez besoin d’au moins trois serveurs&#x2014;un seul contrôleur WFF, un serveur web principal pour la batterie de serveurs et un ou plusieurs serveurs web secondaire pour la batterie de serveurs. Vous pouvez ajouter plusieurs serveurs secondaires à une batterie de serveurs WFF à tout moment. À un niveau élevé, pour créer et configurer une batterie de serveurs WFF pour votre environnement intermédiaire ou de production, que vous devez :

- Créer un serveur de contrôleur en installant des Services Internet (IIS) 7.5 et WFF 2.0.
- Préparer les serveurs principaux et secondaires en créant un compte d’administrateur commun et de configuration des exceptions de pare-feu.
- Configurer la batterie de serveurs à l’aide du Gestionnaire des services Internet sur le serveur de contrôleur.
- Configurer l’équilibrage de charge à l’aide d’IIS Application Request Routing (ARR) ou une autre technologie d’équilibrage de charge.

Les tâches et les procédures pas à pas dans cette rubrique supposent que vous débutez avec des builds de serveur propre exécutant Windows Server 2008 R2. Avant de commencer, pour chaque serveur, vérifiez que :

- Windows Server 2008 R2 Service Pack 1 et toutes les mises à jour disponibles sont installés.
- Le serveur est joint au domaine.
- Le serveur a une adresse IP statique.

> [!NOTE]
> Pour plus d’informations sur la jonction des ordinateurs à un domaine, consultez [joindre des ordinateurs au domaine et journalisation](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Pour plus d’informations sur la configuration des adresses IP statiques, consultez [configurer une adresse IP statique](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).


## <a name="create-the-wff-controller-server"></a>Créer le serveur de contrôleur WFF

Pour créer un serveur de contrôleur WFF, vous devez installer IIS 7 ou version ultérieure et WFF 2.0 ou version ultérieure. En coulisses, WFF utilise l’outil de déploiement Web IIS (Web Deploy) 2.x pour synchroniser les serveurs dans votre batterie de serveurs. Si vous utilisez Web Platform Installer pour installer WFF, le programme d’installation sera automatiquement télécharger et installer Web Deploy pour vous.

**Pour créer le serveur de contrôleur WFF**

1. Téléchargez et installez le [Web Platform Installer](https://go.microsoft.com/?linkid=9739157).
2. En haut de la **Web Platform Installer 3.0** fenêtre, cliquez sur **produits**.
3. Sur le côté gauche de la fenêtre, dans le volet de navigation, cliquez sur **Server**.
4. Dans le **Configuration IIS 7 recommandée** de ligne, cliquez sur **ajouter**.
5. Dans le <strong>Web Farm Framework 2.</strong> <em>x</em> de ligne, cliquez sur <strong>ajouter</strong>.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image2.png)
6. Cliquez sur **Installer**. Notez que le programme Web Platform Installer a ajouté l’outil de déploiement Web, ainsi que d’autres dépendances, à la liste de l’installation.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image3.png)
7. Passez en revue les termes du contrat de licence et si vous acceptez les termes du contrat, cliquez sur **J’accepte**.
8. Une fois l’installation terminée, cliquez sur **Terminer**, puis fermez le **Web Platform Installer 3.0** fenêtre.

## <a name="configure-the-primary-and-secondary-servers"></a>Configurer les serveurs primaires et secondaires

Avant de créer une batterie de serveurs WFF, vous devez effectuer certaines tâches de préparation sur les serveurs web qui composent la batterie de serveurs :

- Ajouter des exceptions de pare-feu pour autoriser le **Core Networking**, **Administration à distance**, et **partage de fichiers et imprimantes** fonctionnalités pour communiquer avec le serveur de contrôleur WFF .
- Créer un compte de domaine (par exemple, **FABRIKAM\stagingfarm**) dans Active Directory et ajoutez-le au groupe Administrateurs local sur chaque serveur. Vous utiliserez ce compte comme compte administrateur de batterie de serveurs lorsque vous créez la batterie de serveurs.

Pour plus d’informations sur la façon de configurer ces exceptions de pare-feu dans le pare-feu Windows, consultez [système en matière de plate-forme pour le Framework de batterie de serveurs Web 2.0 pour IIS 7](https://go.microsoft.com/?linkid=9805128). Pour les autres systèmes de pare-feu, consultez votre documentation du produit.

Vous pouvez utiliser la procédure suivante pour ajouter un compte de domaine au groupe Administrateurs local dans Windows Server 2008 R2. Vous devez effectuer cette procédure sur chaque serveur que vous souhaitez ajouter à la batterie de serveurs&#x2014;en d’autres termes, ajoutez le même compte de domaine au groupe Administrateurs local sur le serveur principal et sur chaque serveur secondaire.

**Pour ajouter un compte de domaine au groupe Administrateurs local**

1. Sur le **Démarrer** menu, pointez sur **outils d’administration**, puis cliquez sur **le Gestionnaire de serveur**.
2. Dans le **le Gestionnaire de serveur** fenêtre, dans le volet d’arborescence, développez **Configuration**, développez **utilisateurs et groupes locaux**, puis cliquez sur **groupes**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image4.png)
3. Dans le **groupes** volet, double-cliquez sur **administrateurs**.
4. Dans le **propriétés du groupe Administrateurs** boîte de dialogue, cliquez sur **ajouter**.
5. Dans le **sélectionner des utilisateurs, les ordinateurs, les comptes de Service ou les groupes** boîte de dialogue, entrez (ou recherchez) à votre compte de domaine (par exemple, **FABRIKAM\stagingfarm**), puis cliquez sur **OK**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image5.png)
6. Dans le **propriétés du groupe Administrateurs** boîte de dialogue, cliquez sur **OK**.

Vos serveurs êtes maintenant prêts à être ajouté à une batterie de serveurs. Dans le cas le serveur principal, vous pouvez configurer le serveur pour répondre aux exigences de votre application avant ou après avoir créé la batterie de serveurs&#x2014;dans les deux cas, le WFF synchronisera les serveurs en déployant des produits, de composants ou de configuration pour vos serveurs secondaires. Par souci de simplicité, ce didacticiel suppose que vous allez configurer le serveur principal lorsque vous avez fini de créer la batterie de serveurs.

## <a name="create-the-wff-server-farm"></a>Créer la batterie de serveurs WFF

À ce stade, tous vos serveurs sont prêts à être ajouté à une batterie de serveurs WFF :

- Vous avez installé WFF sur le serveur de contrôleur.
- Vous avez configuré des exceptions de pare-feu sur vos serveurs web principaux et secondaires.
- Vous avez ajouté un compte de domaine au groupe Administrateurs local sur vos serveurs web principaux et secondaires.

L’étape suivante consiste à créer la batterie de serveurs dans WFF. Faire cela dans le Gestionnaire des services Internet sur le serveur de contrôleur WFF.

**Pour créer une batterie de serveurs WFF**

1. Sur le serveur de contrôleur WFF, sur le **Démarrer** menu, pointez sur **outils d’administration**, puis cliquez sur **Internet Information Services (IIS) Manager**.
2. Dans le **connexions** volet, développez le nœud de serveur local, avec le bouton droit **batteries de serveurs**, puis cliquez sur **créer une batterie de serveurs**.
3. Dans le **créer une batterie de serveurs** boîte de dialogue, tapez un nom explicite pour la batterie de serveurs (par exemple, **batterie de serveurs de mise en lots**), puis sélectionnez **batterie de serveurs approvisionner**.
4. Tapez le nom d’utilisateur et le mot de passe du compte de domaine que vous avez ajouté au groupe Administrateurs local sur chaque serveur.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image6.png)
5. Cliquez sur **Suivant**.
6. Sur le **ajouter des serveurs** page, tapez le nom de domaine complet (FQDN) du serveur principal, sélectionnez **serveur principal**, puis cliquez sur **ajouter**.
7. À ce stade, WFF tente de contacter le serveur principal à l’aide des informations d’identification que vous avez fourni. Si la connexion réussit, le serveur principal sera ajouté à la table sur le **ajouter des serveurs** page.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image7.png)

    > [!NOTE]
    > Vous avez peut-être remarqué que **serveur est disponible pour l’équilibrage de charge** est sélectionnée par défaut. WFF utilise le module ARR IIS pour implémenter l’équilibrage de charge et ainsi distribuer les requêtes entre les serveurs web dans votre batterie de serveurs. Dans la plupart des scénarios, vous devez uniquement désactiver le **serveur est disponible pour l’équilibrage de charge** option si vous souhaitez utiliser une solution à la place d’équilibrage de charge tiers.
8. Sur le **ajouter des serveurs** page, tapez le nom de domaine complet de votre premier serveur secondaire, puis cliquez sur **ajouter**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image8.png)
9. Répétez l’étape 7 pour tous les serveurs secondaires supplémentaires dans votre batterie de serveurs, puis cliquez sur **Terminer**.

Votre batterie de serveurs WFF est maintenant en cours d’exécution. Les produits de la plate-forme web ou les composants que vous installez sur le serveur principal et les applications web ou le contenu que vous déployez sur le serveur principal, seront automatiquement approvisionnés sur tous vos serveurs secondaires.

WFF est un sujet vaste et complex, et vous pouvez en savoir plus sur la [Microsoft Web Farm Framework 2.0 pour IIS 7](https://go.microsoft.com/?linkid=9805129) site Web. Pour l’instant, toutefois, voici deux zones de fonctionnalités que vous devez être conscient :

- *Approvisionnement d’application* est le processus qui réplique le contenu à partir du serveur principal, tels que les applications web et les paramètres de configuration sur tous les serveurs secondaires dans la batterie de serveurs. Par exemple, si vous déployez la solution d’exemple de gestionnaire de contacts à votre serveur de site principal, le processus de déploiement d’application WFF déploiera cette solution à tous vos serveurs de site secondaire. Par défaut, le processus de déploiement d’application s’exécute toutes les 30 secondes.
- *Approvisionnement de la plateforme* est le processus qui synchronise les produits de la plate-forme web et les composants du serveur principal pour tous les serveurs secondaires dans la batterie de serveurs. Par exemple, si vous installez ASP.NET MVC 3 sur votre serveur de site principal, le processus d’approvisionnement de plateforme utilisera Web Platform Installer pour installer ASP.NET MVC 3 sur tous vos serveurs de site secondaire. Par défaut, le processus d’approvisionnement de plateforme exécute toutes les cinq minutes.

Vous pouvez gérer une application de base et les paramètres de configuration de plateforme dans le Gestionnaire des services Internet sur votre serveur de contrôleur WFF.

**Explorez la plate-forme d’applications et paramètres d’approvisionnement**

1. Dans le Gestionnaire des services Internet, dans le **connexions** volet, sélectionnez votre batterie de serveurs.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image9.png)
2. Dans le **batterie de serveurs** volet, double-cliquez sur **approvisionnement d’Application**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image10.png)
3. Comme vous pouvez le voir, la batterie de serveurs est actuellement configurée pour synchroniser les paramètres de configuration et de contenu web entre le serveur principal et les serveurs secondaires toutes les 30 secondes.
4. Cliquez sur **retour**, puis double-cliquez sur **approvisionnement de la plateforme**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image11.png)
5. Comme vous pouvez le voir, la batterie de serveurs est actuellement configurée pour synchroniser des produits de la plate-forme web et des composants entre le serveur principal et les serveurs secondaires toutes les cinq minutes.
6. Cliquez sur **retour**.
7. Pour forcer la batterie de serveurs pour synchroniser immédiatement, les produits de la plate-forme web dans le **Actions** volet, cliquez sur **approvisionne la plateforme**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image12.png)

    > [!NOTE]
    > Approvisionnement de la plateforme peut prendre un certain temps. Le processus d’installation s’exécute en arrière-plan sur les serveurs secondaires dans votre batterie de serveurs.
8. Une fois que vous avez autorisé un délai suffisant pour terminer le processus d’approvisionnement, vous pouvez vérifier que les produits et les composants que vous avez ajouté au serveur principal ont désormais été répliqués sur les serveurs secondaires. Par exemple, vous pouvez ouvrir une session sur un serveur secondaire et utiliser le **le Gestionnaire de serveur** fenêtre pour vérifier que le rôle de serveur web a été installé.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image13.png)
9. Vous pouvez également vérifier la liste des programmes installés pour vérifier que les différents composants de plate-forme web ont été ajoutés.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image14.png)

## <a name="configure-load-balancing"></a>Configurer l’équilibrage de charge

Lorsque vous créez une batterie de serveurs web, vous devez configurer une forme de pour distribuer les requêtes HTTP entre vos serveurs web d’équilibrage de charge. Il peut s’agir de Windows Server 2008 réseau équilibrage de charge, ARR IIS, ou une tiers basée sur le matériel ou logiciel d’équilibrage de charge solution.

WFF est conçu pour s’intégrer étroitement avec IIS ARR. Pour tirer parti de cette intégration, vous devez installer le module ARR sur le serveur de contrôleur WFF. Vous puis de diriger votre trafic web au serveur du contrôleur, généralement en configurant les enregistrements de nom de domaine (DNS). Le serveur de contrôleur puis répartit les demandes entrantes entre les serveurs dans votre batterie de serveurs, en fonction de la disponibilité du serveur et divers autres critères.

> [!NOTE]
> Vous n’êtes pas obligé d’utiliser ARR avec WFF ; Vous pouvez configurer WFF pour travailler avec les solutions d’équilibrage de charge tiers. Pour plus d’informations, consultez [vue d’ensemble du Framework de batterie de serveurs Web 2.0 pour IIS 7](https://go.microsoft.com/?linkid=9805126).


L’équilibrage de charge à l’aide d’ARR est un sujet complexe, plus dont est dépasse le cadre de ce didacticiel. Toutefois, vous pouvez utiliser la procédure suivante pour installer le module ARR et de prise en main l’équilibrage de charge.

**Comment configurer l’équilibrage de charge sur le serveur de contrôleur WFF**

1. Sur le serveur de contrôleur WFF, lancez le programme Web Platform Installer.
2. En haut de la **Web Platform Installer 3.0** fenêtre, cliquez sur **produits**.
3. Sur le côté gauche de la fenêtre, dans le volet de navigation, cliquez sur **Server**.
4. Dans le **Application demande routage 2.5** de ligne, cliquez sur **ajouter**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image15.png)
5. Cliquez sur **installer**, puis suivez les instructions fournies dans le **Installation de Web Platform** fenêtre.
6. Une fois l’installation terminée, lancez le Gestionnaire IIS, puis, dans le **connexions** volet, cliquez sur le nœud de batterie de serveurs de votre serveur. Notez que plusieurs nouvelles icônes ont été ajoutées à la **batterie de serveurs** volet.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image16.png)
7. Dans le **batterie de serveurs** volet, double-cliquez sur **équilibrer la charge**.
8. Dans le **Load Balance** volet, sélectionnez une charge équilibrer algorithme (par exemple, **demande moins actuelle**).

    > [!NOTE]
    > Pour plus d’informations sur les algorithmes et les autres paramètres de configuration d’équilibrage de charge, consultez [Module de routage Application demande](https://go.microsoft.com/?linkid=9805130).

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image17.png)
9. Dans le **Actions** volet, cliquez sur **appliquer**.

Vous avez maintenant configuré pour les serveurs dans votre batterie de serveurs d’équilibrage de charge de base. Si vous dirigez votre batterie de serveurs trafic web au serveur du contrôleur, les demandes seront distribués entre les serveurs dans votre batterie de serveurs en fonction de la disponibilité et l’algorithme d’équilibrage de charge que vous avez sélectionné.

Pour plus d’informations sur la configuration d’équilibrage de charge avec ARR, consultez [Module de routage Application demande](https://go.microsoft.com/?linkid=9805130).

## <a name="monitor-the-server-farm"></a>Moniteur de la batterie de serveurs

Vous pouvez surveiller l’intégrité de votre batterie de serveurs à tout moment via le Gestionnaire des services Internet sur le serveur de contrôleur. Dans le **connexions** volet, développez votre batterie de serveurs, puis cliquez sur **serveurs**. Le volet central affiche un résumé de chaque serveur dans la batterie de serveurs avec un journal de suivi de l’activité récente.

![](creating-a-server-farm-with-the-web-farm-framework/_static/image18.png)

## <a name="conclusion"></a>Conclusion

Votre batterie de serveurs WFF doit maintenant être en cours d’exécution. Vous pouvez configurer le serveur principal pour prendre en charge de l’approche de déploiement que vous préférez&#x2014;consultez la section obtenir des informations supplémentaires pour plus d’informations&#x2014;et votre configuration est répliquée sur chaque serveur secondaire dans la batterie de serveurs.

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur tous les aspects de la configuration et à l’aide de la WFF, consultez le [Microsoft Web Farm Framework 2.0 pour IIS 7](https://go.microsoft.com/?linkid=9805129) site Web.

> [!div class="step-by-step"]
> [Précédent](configuring-a-database-server-for-web-deploy-publishing.md)
> [Suivant](configuring-deployment-properties-for-a-target-environment.md)
