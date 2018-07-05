---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: Choisir l’approche appropriée pour le déploiement Web | Microsoft Docs
author: jrjlee
description: Lorsque vous travaillez avec l’Internet Information Services (IIS) outil de déploiement Web (Web Deploy) 2.0 ou version ultérieure, il existe trois approches principales, que vous pouvez utiliser pour obtenir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: f1c3a09d9e960a53a25e0dd0b953224204a9ba7b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367743"
---
<a name="choosing-the-right-approach-to-web-deployment"></a>Choisir l’approche appropriée pour le déploiement Web
====================
par [Jason Lee](https://github.com/jrjlee)

[Télécharger PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Lorsque vous travaillez avec l’Internet Information Services (IIS) outil de déploiement Web (Web Deploy) 2.0 ou version ultérieure, il existe trois approches principales, que vous pouvez utiliser pour vos applications web empaquetée sur un serveur web. Vous pouvez :
> 
> - Déployer l’application à partir d’un emplacement distant en ciblant le *Service de l’Agent de déploiement Web* (également connu sous le terme « à distance agent ») sur le serveur de destination.
> - Déployez l’application à partir d’un emplacement distant à l’aide de Web déployer à la demande (également connu sous le terme « temp agent »).
> - Déployer l’application à partir d’un emplacement distant en ciblant le *Gestionnaire de déploiement Web IIS* sur le serveur de destination.
> - Déployez l’application en copie le package web sur le serveur de destination et en les important via le Gestionnaire des services IIS manuellement.
> 
> Façon dont vous configurez vos serveurs web de destination dépend de quelle approche au déploiement que vous souhaitez utiliser. Cette rubrique vous aidera à déterminer l’approche de déploiement qui vous consiste.


Ce tableau montre les principaux avantages et les inconvénients de chaque approche de déploiement, ainsi que les scénarios plus adaptés à chaque approche.

| Approche | Avantages | Inconvénients | Scénarios classiques |
| --- | --- | --- | --- |
| Agent distant | Il est facile à configurer. Il convient pour les mises à jour régulières aux applications web et de contenu. | L’utilisateur doit être un administrateur sur le serveur cible. l’utilisateur ne peut pas fournir d’autres informations d’identification. | Environnements de développement. Environnements de test. |
| Agent temporaire | Il est inutile d’installer Web Deploy sur l’ordinateur cible. La dernière version de Web Deploy est automatiquement utilisée. | L’utilisateur doit être un administrateur sur le serveur cible. L’utilisateur ne peut pas fournir d’autres informations d’identification. | Environnements de développement. Environnements de test. |
| Gestionnaire Web Deploy | Les utilisateurs ne sont pas administrateur peuvent déployer le contenu. Il convient pour les mises à jour régulières aux applications web et de contenu. | Il est beaucoup plus difficile à configurer. | Environnements de préproduction. Environnements de production intranet. Environnements hébergés. |
| Déploiement hors connexion | Il est très facile à configurer. Il est adapté aux environnements isolés. | L’administrateur du serveur doit manuellement copier et importer le package web chaque fois. | Environnements de production sur Internet. Environnements de réseau isolé. |
  

## <a name="using-the-remote-agent"></a>À l’aide de l’Agent distant

Lorsque vous installez Web Deploy à l’aide des paramètres par défaut sur un serveur de destination, le Service de l’Agent de déploiement Web (le « agent distant ») est automatiquement installé et démarré. Par défaut, l’agent distant expose un point de terminaison HTTP à cette adresse :


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]


> [!NOTE]
> Vous pouvez remplacer [*server*] par le nom de la machine de votre serveur web, une adresse IP pour votre serveur web ou un nom d’hôte qui correspond à votre serveur web.


Les administrateurs de serveur peuvent déployer des packages web à partir d’un emplacement distant, comme un ordinateur de développeur ou un serveur de builds, en spécifiant cette adresse de point de terminaison. Par exemple, supposons que Matt Hink chez Fabrikam, Inc. a créé le projet d’application web ContactManager.Mvc sur son ordinateur de développement. Le processus de génération génère un package web, avec un *. deploy.cmd* fichier qui contient les commandes Web Deploy requis pour installer le package. Si Matt est un administrateur de serveur sur le serveur TESTWEB1, il peut déployer l’application web sur le serveur web de test en exécutant cette commande sur son ordinateur de développement :


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]


En effet, l’exécutable de Web Deploy peut déduire l’adresse de point de terminaison de l’agent distant si vous fournissez le nom de l’ordinateur, par conséquent, Matt doit uniquement ce type :


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]


> [!NOTE]
> Pour plus d’informations sur la syntaxe de ligne de commande de Web Deploy et *. deploy.cmd* de fichiers, consultez [Comment : installer un Package de déploiement à l’aide du fichier deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).


L’agent distant offre un moyen simple de déployer le contenu depuis un emplacement distant, et cette approche fonctionne bien avec un clic ou automatisé de déploiement. Toutefois, l’utilisateur qui exécute la commande de déploiement doit également être un administrateur de domaine ou un membre du groupe Administrateurs local sur le serveur de destination. En outre, l’agent distant ne prend en charge l’authentification de base, vous ne pouvez pas passer les autres informations d’identification sur la ligne de commande.

L’agent à distance fournit une approche très utile pour le déploiement dans les scénarios de test, où il n’est pas rare pour les développeurs d’avoir un contrôle administrateur complet sur un environnement de serveur de test, et les applications sont généralement reconstruites et redéployées très ou de développement fréquemment. Toutefois, cette approche est généralement moins acceptable pour les environnements de production ou intermédiaire.

Pour obtenir un exemple de bout en bout d’un scénario qui utilise l’approche de l’agent distant, consultez [scénario : configuration d’un environnement de Test pour le déploiement Web](scenario-configuring-a-test-environment-for-web-deployment.md).

## <a name="using-the-temp-agent"></a>À l’aide de l’Agent temporaire

L’approche temporaire de l’agent de déploiement est similaire à l’approche de l’agent distant. Toutefois, contrairement à l’approche de l’agent distant, vous n’avez pas besoin d’installer Web Deploy sur le serveur web de destination. Au lieu de cela, lorsque vous effectuez le déploiement, Web Deploy installe une version temporaire du service de l’agent de déploiement web sur le serveur de destination et l’utiliserez pour déployer votre contenu sur IIS. Lorsque le déploiement est terminé, tous les fichiers temporaires sont supprimés.

Si vous souhaitez utiliser le paramètre de fournisseur temporaire de l’agent, ajoutez le **/g** indicateur à votre commande de déploiement :


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]


> [!NOTE]
> Vous ne pouvez pas utiliser l’agent temporaire si le service de l’agent de déploiement web est installé sur l’ordinateur de destination, même si le service n’est pas en cours d’exécution.


L’avantage de cette approche est que vous n’avez pas besoin de mettre à jour les installations de Web Deploy sur vos serveurs de destination. En outre, vous n’avez pas besoin de vous assurer que les ordinateurs source et de destination exécutent la même version de Web Deploy. Toutefois, cette approche comporte les mêmes limitations principal en tant que l’approche de l’agent distant, à savoir que vous devez être un administrateur local sur le serveur de destination afin de déployer du contenu, et seule l’authentification NTLM est pris en charge. L’approche temp agent nécessite également la configuration initiale de beaucoup plus de l’environnement de destination.

Pour plus d’informations sur l’utilisation de l’agent temporaire, consultez [Comment : installer un Package de déploiement à l’aide du fichier deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx) et [Web déployer à la demande](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

## <a name="using-the-web-deploy-handler"></a>Utilisation du Web de déployer le Gestionnaire

Pour IIS 7 et versions ultérieures, le Web Deploy propose une approche de déploiement autre par le biais du Gestionnaire de déploiement Web IIS. Le Gestionnaire de déploiement Web est étroitement intégré avec le Service de gestion Web (WMSvc), IIS qui est conçu pour permettre aux utilisateurs de gérer des sites Web IIS à partir d’emplacements distants.

Par défaut, l’agent distant expose un point de terminaison HTTP à cette adresse :


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]


> [!NOTE]
> Vous pouvez remplacer [*server*] par le nom de la machine de votre serveur web, une adresse IP pour votre serveur web ou un nom d’hôte qui correspond à votre serveur web.


Le grand avantage du Gestionnaire de déploiement Web sur l’agent distant et l’agent temporaire, est que vous pouvez configurer IIS pour autoriser les utilisateurs non administrateurs de déployer des applications et du contenu à des sites Web IIS spécifiques. Le Gestionnaire de déploiement Web également prend en charge l’authentification de base, vous pouvez fournir d’autres informations d’identification en tant que paramètres dans vos commandes Web Deploy. Le principal inconvénient est que le Gestionnaire de déploiement Web est initialement beaucoup plus compliqué à configurer.

Dans le cas des utilisateurs non administrateurs, le Service de gestion Web (WMSvc) autorise uniquement l’utilisateur à se connecter à IIS à l’aide d’une connexion au niveau du site, plutôt que d’une connexion au niveau du serveur. Pour accéder à un site particulier, vous pouvez inclure une chaîne de requête spécifique au site dans l’adresse de point de terminaison :


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]


Par exemple, qu'un processus de génération est configuré pour déployer automatiquement une application web dans un environnement intermédiaire après chaque build réussie. Si vous avez utilisé l’approche de l’agent distant, vous devez rendre l’identité de processus de génération d’un administrateur sur vos serveurs de destination. En revanche, à l’aide de l’approche de gestionnaire de déploiement Web vous pouvez donner un utilisateur non-administrateur&#x2014;**FABRIKAM\stagingdeployer** dans ce cas&#x2014;autorisation un spécifique du site Web IIS uniquement et le processus de génération peut fournir ces informations d’identification pour déployer le package web.


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]


> [!NOTE]
> Pour plus d’informations sur les opérations de ligne de commande de Web Deploy et la syntaxe, consultez [référence de ligne de commande de déploiement Web](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Pour plus d’informations sur l’utilisation de la *. deploy.cmd* de fichiers, consultez [Comment : installer un Package de déploiement à l’aide du fichier deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).


Le Gestionnaire de déploiement Web fournit une approche très utile pour le déploiement dans les environnements, les environnements hébergés et les environnements de production basées sur l’intranet, où l’accès à distance sur le serveur est disponible, mais les informations d’identification de l’administrateur ne sont pas de transfert.

Pour obtenir un exemple de bout en bout d’un scénario qui utilise l’approche de gestionnaire de déploiement Web, consultez [scénario : configuration d’un environnement de préproduction pour le déploiement Web](scenario-configuring-a-staging-environment-for-web-deployment.md).

## <a name="using-offline-deployment"></a>À l’aide de déploiement hors connexion

Dans certains cas, il n’est pas possible ou pratique pour déployer des applications et du contenu sur un site Web IIS à partir d’un emplacement distant. Par exemple, les ordinateurs source et de destination peuvent être dans des réseaux isolés ou des segments de réseau, ou stratégie de pare-feu ne peut pas autoriser l’accès à distance.

Dans les scénarios comme celles-ci, vous pouvez toujours utiliser l’empaquetage et publication des fonctionnalités de Web Deploy ; Vous ne pouvez pas les utilisent simplement à partir d’un emplacement distant. Au lieu de cela, un administrateur sur le serveur de destination doit copier le package web sur le serveur et importez-le via le Gestionnaire des services Internet.

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

L’approche de déploiement hors connexion est généralement utile dans les environnements de production sur Internet, où les serveurs dans un réseau de périmètre peuvent restreinte connectivité avec les ordinateurs du réseau interne.

Pour obtenir un exemple de bout en bout d’un scénario qui utilise l’approche de déploiement hors connexion, consultez [scénario : configuration d’un environnement de Production pour le déploiement Web](scenario-configuring-a-production-environment-for-web-deployment.md).

## <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les opérations de ligne de commande de Web Deploy et la syntaxe, consultez [référence de ligne de commande de déploiement Web](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Pour plus d’informations sur l’utilisation de la *. deploy.cmd* de fichiers, consultez [Comment : installer un Package de déploiement à l’aide du fichier deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx).

Pour obtenir des instructions plus générales sur les différentes façons dans lequel vous pouvez déployer des packages web à partir d’un ordinateur distant, consultez [à l’aide de Web déployer à distance](https://technet.microsoft.com/library/ee461175(WS.10).aspx). Pour plus d’informations sur l’utilisation de Web déployer à la demande, consultez [Web déployer à la demande](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

> [!div class="step-by-step"]
> [Précédent](configuring-server-environments-for-web-deployment.md)
> [Suivant](scenario-configuring-a-test-environment-for-web-deployment.md)
