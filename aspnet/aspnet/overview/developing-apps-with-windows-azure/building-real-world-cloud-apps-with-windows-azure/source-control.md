---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: Source de contrôle (génération d’applications Cloud réalistes avec Azure) | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with Azure e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent il...
ms.author: riande
ms.date: 06/23/2015
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: 8402b73f5f9d063d958df39f98267468e4aef746
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832603"
---
<a name="source-control-building-real-world-cloud-apps-with-azure"></a>Contrôle de code source (génération d’applications Cloud réalistes avec Azure)
====================
par [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Téléchargement Fix It projet](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [télécharger l’E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Le **Building Real World Cloud Apps with Azure** e-book est basé sur une présentation développée par Scott Guthrie. Il explique 13 modèles et pratiques qui peuvent vous aider à réussir le développement d’applications web pour le cloud. Pour plus d’informations sur l’e-book, consultez [le premier chapitre](introduction.md).


Contrôle de code source est essentiel pour tous les projets de développement cloud, pas seulement les environnements d’équipe. Vous ne pensez de modification du code source, ou même un document Word sans une fonction d’annulation et de sauvegardes automatiques et de contrôle de code source vous donne ces fonctions au niveau du projet où ils peuvent gagner du temps quand une erreur survient. Avec les services de contrôle de source de cloud, vous n’avez plus à vous soucier de configuration complexe, et vous pouvez utiliser gratuitement jusqu'à 5 utilisateurs de contrôle de code source Visual Studio Online.

La première partie de ce chapitre explique les trois meilleures pratiques clés à prendre en compte :

- [Traiter les scripts d’automatisation comme code source](#scripts) et version leur ainsi que votre code d’application.
- [Ne jamais rechercher dans les secrets](#secrets) (des données sensibles telles que des informations d’identification) dans un référentiel de code source.
- [Configurer les branches source](#devops) pour activer le flux de travail DevOps.

Le reste de ce chapitre donne des exemples d’implémentation de ces modèles dans Visual Studio, Azure et Visual Studio Online :

- [Ajouter des scripts pour le contrôle de code source dans Visual Studio](#vsscripts)
- [Store les données sensibles dans Azure](#appsettings)
- [Utiliser Git dans Visual Studio et Visual Studio Online](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>Traiter les scripts d’automatisation comme code source

Lorsque vous travaillez sur un projet cloud que vous modifiez fréquemment les choses et que vous souhaitez être en mesure de réagir rapidement aux problèmes signalés par vos clients. Répondre rapidement implique l’utilisation de scripts d’automatisation, comme expliqué dans la [automatiser tout](automate-everything.md) chapitre. Tous les scripts que vous utilisez pour créer votre environnement, déployez sur elle, mise à l’échelle, etc., doivent être synchronisées avec le code source de votre application.

Pour conserver la synchronisation avec le code de scripts, les stocker dans votre système de contrôle de code source. Puis si vous avez besoin restaurer les modifications, ou bien un correctif rapide au code de production qui est différent du code de développement, vous n’êtes pas obligé de perdre du temps dépistage quels paramètres ont été modifiés ou les membres de l’équipe ont des copies de la version que vous avez besoin. Vous êtes assuré que les scripts que vous avez besoin sont synchronisées avec la base de code que vous avez besoin pour, et vous êtes assuré que tous les membres de l’équipe fonctionnent avec les mêmes scripts. Si vous avez besoin automatiser les tests et le déploiement d’un correctif à chaud pour la production ou développement de nouvelles fonctionnalités, vous aurez le script approprié pour le code qui doit être mis à jour.

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>N’archivez pas secrets

Un référentiel de code source est généralement accessible trop grand nombre de personnes pour qu’il soit un emplacement sécurisé de manière appropriée pour les données sensibles telles que les mots de passe. Si les scripts s’appuient sur des secrets tels que les mots de passe, paramétrer ces paramètres afin qu’ils ne sont enregistrés dans le code source et stocker vos secrets à un autre endroit.

Par exemple, Azure vous permet de que télécharger des fichiers qui contiennent des paramètres de publication afin d’automatiser la création de profils de publication. Ces fichiers incluent les noms d’utilisateur et mots de passe qui sont autorisés à gérer vos services Azure. Si vous utilisez cette méthode pour créer des profils de publication, et si vous archivez ces fichiers au contrôle de code source, toute personne ayant accès à votre référentiel peut voir ces noms d’utilisateur et les mots de passe. Vous pouvez stocker en toute sécurité le mot de passe du profil de publication lui-même, car il est chiffré et se trouve dans un *. pubxml.user* fichier par défaut n’est pas inclus dans le contrôle de code source.

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>Branches de source de structure pour faciliter le flux de travail DevOps

Comment implémenter des branches dans votre référentiel affecte votre capacité à développer de nouvelles fonctionnalités et de résoudre les problèmes de production. Voici un modèle qu’un grand nombre de moyennes en taille réelle les équipes utilisent :

![Structure de la branche source](source-control/_static/image1.png)

La branche principale correspond toujours au code de production. Branches sous forme de base correspondent aux différentes étapes dans le cycle de vie de développement. La branche développement est où vous implémentez les nouvelles fonctionnalités. Pour une petite équipe peut-être avez-vous un maître et développement, mais souvent recommandé que les personnes ont une branche intermédiaire entre le développement et master. Vous pouvez utiliser la mise en lots pour les tests d’intégration finale avant une mise à jour est déplacé vers la production.

Pour les équipes big il peut y avoir des branches séparées pour chaque nouvelle fonctionnalité ; pour une petite équipe, vous pouvez avoir tout le monde archiver à la branche de développement.

Si vous disposez d’une branche pour chaque fonctionnalité, lors de la fonctionnalité A est prêt fusion vous ses modifications du code source dans le développement de branche et le bas dans les autres branches de fonctionnalité. Ce code source des processus de fusion peut prendre du temps, et pour éviter ce travail tout en conservant les fonctionnalités distinctes, certaines équipes de mettre en œuvre alternative appelée *[bascules de fonctionnalité](http://en.wikipedia.org/wiki/Feature_toggle)* (également appelé *indicateurs de fonctionnalités*). Cela signifie que tout le code pour toutes les fonctionnalités est dans la même branche, mais que vous activez ou désactivez chaque fonctionnalité dans le code à l’aide des commutateurs. Par exemple, la fonctionnalité A est un nouveau champ pour les tâches d’application Fix It et la fonctionnalité B ajoute des fonctionnalités de mise en cache. Le code pour ces deux fonctionnalités peut être dans la branche de développement, mais l’application sera seul affichage du nouveau champ quand une variable est définie sur true et il utilisera uniquement la mise en cache lorsqu’une variable différente est définie sur true. Si la fonctionnalité A n’est pas prête à être promu, mais la fonctionnalité B est prête, vous pouvez promouvoir tout le code en Production avec le commutateur de fonctionnalité A désactivé et activer la fonctionnalité B. Vous pouvez ensuite terminer de caractéristiques A et promouvoir une version ultérieure, toutes avec aucune fusion de code source.

Si vous utilisez des branches ou les bascules pour les fonctionnalités, une structure de branches, comme cela vous permet de flux de votre code à partir de développement en production de manière agile et reproductible.

Cette structure vous permet également de réagir rapidement aux commentaires des clients. Si vous avez besoin rendre un correctif rapide en production, vous pouvez également le faire efficacement de manière agile. Vous pouvez créer une branche depuis master ou intermédiaire, et lorsqu’il est prêt fusionner des principales d’et vers le bas en branches de développement et de fonctionnalité.

![Branche du correctif logiciel](source-control/_static/image2.png)

Sans une structure de branches ainsi sa séparation des branches de développement et de production, un problème de production est susceptible de vous en position d’avoir à promouvoir le nouveau code de fonction, ainsi que votre correctif de production. Le nouveau code de fonction n’est peut-être pas entièrement testée et prête pour production et vous devrez peut-être faire beaucoup de travail de sauvegarde les modifications qui ne sont pas prêtes. Ou bien, vous devrez peut-être retarder votre correctif afin de tester les modifications et préparez-vous au déploiement.

Vous verrez ensuite des exemples montrant comment implémenter ces trois modèles dans Visual Studio, Azure et Visual Studio Online. Il s’agit d’exemples plutôt que des instructions détaillées de how-to--informatique ; Pour obtenir des instructions détaillées qui procurent toutes le contexte nécessaire, consultez le [ressources](#resources) section à la fin de ce chapitre.

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>Ajouter des scripts pour le contrôle de code source dans Visual Studio

Vous pouvez ajouter des scripts pour le contrôle de code source dans Visual Studio en les incluant dans un dossier de solution Visual Studio (en supposant que votre projet est sous contrôle de code source). Voici une façon de procéder.

Créez un dossier pour les scripts dans votre dossier de solution (le même dossier que celui qui a votre *.sln* fichier).

![Dossier d’Automation](source-control/_static/image3.png)

Copiez les fichiers de script dans le dossier.

![Contenu du dossier Automation](source-control/_static/image4.png)

Dans Visual Studio, ajoutez un dossier de solution pour le projet.

![Sélection de menu Nouveau dossier de Solution](source-control/_static/image5.png)

Et ajoutez les fichiers de script dans le dossier de solution.

![Ajout d’une sélection de menu d’un élément existant](source-control/_static/image6.png)

![Boîte de dialogue Ajouter un élément existant](source-control/_static/image7.png)

Les fichiers de script sont désormais inclus dans votre projet et contrôle de code source effectue le suivi des modifications de la version avec les modifications de code source correspondant.

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>Store les données sensibles dans Azure

Si vous exécutez votre application dans un Site Web Azure, un pour éviter de stocker des informations d’identification dans le contrôle de code source consiste à les stocker dans Azure.

Par exemple, l’application Fix It stocke dans son fichier Web.config fichier deux chaînes de connexion ayant des mots de passe en production et une clé qui donne accès à votre compte de stockage Azure.

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

Si vous placez les valeurs de production réel de ces paramètres dans votre *Web.config* fichier, ou si vous les placez dans le *Web.Release.config* fichier pour configurer une transformation de Web.config pour les insérer pendant le déploiement, ils amèneront stockés dans le référentiel source. Si vous entrez les chaînes de connexion de base de données dans la production le profil de publication, le mot de passe sera dans votre *.pubxml* fichier. (Vous pourriez exclure les *.pubxml* de fichiers à partir du contrôle de code source, mais vous perdrez alors l’avantage de partage de tous les autres paramètres de déploiement.)

Azure vous offre une alternative pour la **appSettings** et sections de chaînes de connexion le *Web.config* fichier. Voici la partie pertinente de la **Configuration** onglet d’un site web du portail de gestion Azure :

![appSettings et connectionStrings dans le portail](source-control/_static/image8.png)

Lorsque vous déployez un projet sur ce site web et l’application s’exécute, toutes les valeurs que vous avez stocké dans Azure remplacent toutes les valeurs sont dans le fichier Web.config.

Vous pouvez définir ces valeurs dans Azure à l’aide du portail de gestion ou de scripts. Le script d’automatisation de la création environnement vous l’avez vu dans la [automatiser tout](automate-everything.md) chapitre crée une base de données SQL Azure, obtient le stockage et les chaînes de connexion de base de données SQL et stocke ces secrets dans les paramètres de votre site web.

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

Notez que les scripts sont paramétrés de sorte que les valeurs réelles n’obtenir conservées dans le référentiel source.

Lorsque vous exécutez localement dans votre environnement de développement, l’application lit votre fichier Web.config local et votre connexion pointe de chaîne vers une base de données de base de données locale SQL Server dans le *application\_données* dossier de votre projet web. Lorsque vous exécutez l’application dans Azure et l’application tente de lire ces valeurs dans le fichier Web.config, il récupère et utilise sont les valeurs stockées pour le Site Web, pas ce qui est réellement dans le fichier Web.config.

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-visual-studio-online"></a>Utiliser Git dans Visual Studio et Visual Studio Online

Vous pouvez utiliser n’importe quel environnement de contrôle de code source pour implémenter la structure en branches DevOps présentée précédemment. Pour les équipes multisites un [système de contrôle de version distribué](http://en.wikipedia.org/wiki/Distributed_revision_control) (gérer) peut fonctionner mieux ; pour les autres équipes un [système centralisé](http://en.wikipedia.org/wiki/Revision_control) peuvent mieux fonctionner.

[GIT](http://git-scm.com/) est un gérer est est devenu très populaire. Lorsque vous utilisez Git pour le contrôle de code source, vous avez une copie complète du référentiel avec l’ensemble de son historique sur votre ordinateur local. De nombreuses personnes préfèrent que car il est plus facile pour continuer à utiliser lorsque vous n’êtes pas connecté au réseau, vous pouvez continuer à effectuer est validée et restaurations, créer et changer de branches et ainsi de suite. Même lorsque vous êtes connecté au réseau, il est plus facile et plus rapide de créer des branches et changer de branches lorsque tout est local. Vous pouvez également effectuer des validations locales et des restaurations sans avoir un impact sur les autres développeurs. Et vous pouvez créer des lots validations avant de les envoyer au serveur.

[Microsoft Visual Studio Online](https://www.visualstudio.com/)(VSO), anciennement connu en tant que Team Foundation Service, propose les deux Git et [Team Foundation Version Control](https://msdn.microsoft.com/library/ms181237(v=vs.120).aspx) (TFVC ; centralisé de contrôle de code source). Ici, chez Microsoft dans le groupe Azure certaines équipes utilisent contrôle de source centralisé, certaines utilisent distribuée, et certaines utilisent une combinaison (centralisée pour certains projets et distribuée pour d’autres projets). Le service de Visual Studio Online est gratuit jusqu'à 5 utilisateurs. Vous pouvez vous inscrire pour un plan gratuit [ici](https://go.microsoft.com/fwlink/?LinkId=307137).

Visual Studio 2013 inclut intégrées de première classe [prise en charge Git](https://msdn.microsoft.com/library/hh850437.aspx); Voici une rapide démonstration de son fonctionnement.

Avec un projet ouvert dans Visual Studio 2013, cliquez sur la solution dans **l’Explorateur de solutions**, puis choisissez **ajouter la Solution au contrôle de code Source**.

![Ajoutez la Solution au contrôle de code Source](source-control/_static/image9.png)

Visual Studio vous demande si vous souhaitez utiliser TFVC (contrôle de version centralisé) ou Git.

![Choisissez le contrôle de code Source](source-control/_static/image10.png)

Lorsque vous sélectionnez Git et cliquez sur **OK**, Visual Studio crée un nouveau référentiel Git local dans votre dossier de solution. Le nouveau dépôt aucun fichier n’a encore ; Vous devez les ajouter au référentiel en effectuant une validation de Git. Avec le bouton droit de la solution dans **l’Explorateur de solutions**, puis cliquez sur **valider**.

![Valider](source-control/_static/image11.png)

Visual Studio automatiquement tous les fichiers de projet pour la validation des étapes et les répertorie dans **Team Explorer** dans le **modifications incluses** volet. (S’il y a certains vous ne souhaitez pas inclure dans la validation, vous pouvez sélectionner les, avec le bouton droit, puis cliquez sur **exclure**.)

![Team Explorer](source-control/_static/image12.png)

Entrez un commentaire de validation, cliquez sur **validation**, et Visual Studio exécute la validation et affiche le code de validation.

![Modifications de Team Explorer](source-control/_static/image13.png)

Maintenant si vous modifiez du code afin qu’elle soit différente de ce qui est dans le référentiel, vous pouvez facilement afficher les différences. Clic droit un fichier que vous avez modifié, sélectionnez **comparer avec Unmodified**, et vous obtenez un affichage de comparaison qui affiche vos modifications non validées.

![Comparer avec non modifié](source-control/_static/image14.png)

![Diff avec modifications](source-control/_static/image15.png)

Vous pouvez facilement voir les modifications apportées et de les archiver.

Supposons que vous devez apporter une branche : vous pouvez le faire dans Visual Studio. Dans **Team Explorer**, cliquez sur **nouvelle branche**.

![Nouvelle branche de Team Explorer](source-control/_static/image16.png)

Entrez un nom de branche, cliquez sur **créer une branche**, et si vous avez sélectionné **extraire la branche**, Visual Studio extrait automatiquement la nouvelle branche.

![Nouvelle branche de Team Explorer](source-control/_static/image17.png)

Vous pouvez désormais apporter des modifications aux fichiers et les archiver dans cette branche. Et vous pouvez facilement basculer entre les branches et de Visual Studio automatiquement les fichiers selon la branche où vous ont extrait les synchronisations. Dans cet exemple, la page web titre dans  *\_Layout.cshtml* a été remplacé par « Correctif 1 « HotFix1 branche.

![Branche de Hotfix1](source-control/_static/image18.png)

Si vous basculez vers le contrôleur de créer une branche, le contenu de la  *\_Layout.cshtml* fichier revient automatiquement à ce qu’ils sont dans la branche principale.

![Branche principale](source-control/_static/image19.png)

Présente un exemple simple de la façon dont vous pouvez rapidement créer une branche et retourner dans les deux sens entre les branches. Cette fonctionnalité permet à un flux de travail hautement agile à l’aide de la structure de branche et de scripts d’automatisation présentée dans le [automatiser tout](automate-everything.md) chapitre. Par exemple, vous pouvez être travailler dans la branche de développement, créer une branche de correctif logiciel depuis master, basculez vers la nouvelle branche, apportez vos modifications il et les valider, puis revenez à la branche de développement et continuer ce que vous faisiez.

Ce que vous avez vu ici est la façon dont vous travaillez avec un référentiel Git dans Visual Studio. Dans un environnement d’équipe vous généralement également transmettre modifications à un référentiel commun. Les outils de Visual Studio vous permettent également pointer vers un référentiel Git distant. Vous pouvez utiliser GitHub.com à cette fin, ou vous pouvez utiliser [Git dans Visual Studio Online](https://msdn.microsoft.com/library/hh850437.aspx) intégré avec toutes les autres fonctionnalités Visual Studio Online tels que l’élément de travail et de suivi des bogues.

Ce n’est pas la seule façon, vous pouvez implémenter une stratégie de création de branche agile, bien sûr. Vous pouvez activer le même flux de travail agile à l’aide d’un référentiel de contrôle de code source centralisé.

## <a name="summary"></a>Récapitulatif

Mesurer la réussite de votre système de contrôle de code source en fonction de la vitesse à laquelle vous pouvez apporter une modification et obtenir en direct de manière sécurisée et prévisible. Si vous avez peur d’apporter une modification, car vous devez effectuer une ou deux jours de tests manuels sur celui-ci, vous vous demandez peut-être ce que vous devez faire process-wise ou test-wise afin que vous pouvez effectuer cette modification en quelques minutes ou au pire n’est plus à une heure. Une stratégie pour effectuer cette opération consiste à implémenter l’intégration continue et la livraison continue, que nous aborderons dans les [chapitre suivant](continuous-integration-and-continuous-delivery.md).

<a id="resources"></a>
## <a name="resources"></a>Ressources

Le [Visual Studio Online](https://www.visualstudio.com/) portail fournit des services de documentation et de prise en charge, et vous pouvez vous inscrire pour un compte. Si vous disposez de Visual Studio 2012 et que vous souhaitez utiliser Git, consultez [Visual Studio Tools pour Git](https://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c).

Pour plus d’informations sur TFVC (contrôle de version centralisé) et Git (contrôle de version distribué), consultez les ressources suivantes :

- [Quel système de contrôle de version dois-je utiliser : TFVC ou Git ?](https://msdn.microsoft.com/library/vstudio/ms181368.aspx#tfvc_or_git_summary) Documentation MSDN, inclut un tableau récapitulant les différences entre TFVC et Git.
- [Eh bien, j’aime Team Foundation Server et j’aime Git, mais qui est mieux ?](https://blogs.msdn.com/b/visualstudiouk/archive/2013/08/05/well-i-like-team-foundation-server-and-i-like-git-but-which-is-better.aspx) Comparaison de Git et TFVC.

Pour plus d’informations sur les stratégies de création de branche, consultez les ressources suivantes :

- [Création d’un Pipeline de mise en production avec Team Foundation Server 2012](https://msdn.microsoft.com/library/dn449957.aspx). Documentation de Microsoft Patterns and Practices. Consultez le chapitre 6 pour une présentation des stratégies de branchement. Fonctionnalité de conseillers Active/désactive sur les branches de fonctionnalité et si les branches de fonctionnalités sont utilisées, les défenseurs en les gardant à courte durée (heures ou jours au maximum).
- [Guide du contrôle de version](https://aka.ms/vsarsolutions). Guide sur les stratégies de branchement par le groupe ALM Rangers. Consultez Strategies.pdf branchement sur l’onglet téléchargements.
- [Développement de logiciel avec le « Feature Toggle »](https://msdn.microsoft.com/magazine/dn683796.aspx). Article de MSDN Magazine.
- [Activer/désactiver des fonctionnalités](http://martinfowler.com/bliki/FeatureToggle.html). Introduction à la fonctionnalité Active/désactive / indicateurs de fonctionnalités sur le blog de Martin Fowler.
- [Fonctionnalité bascules vs Branches de fonctionnalité](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx). Un autre billet de blog sur « feature Toggle », par Dylan Smith.

Pour plus d’informations sur la gestion des informations sensibles qui ne doivent pas être conservées dans des référentiels de contrôle de code source, consultez les ressources suivantes :

- [Meilleures pratiques pour le déploiement des mots de passe et autres données sensibles sur ASP.NET et Azure App Service](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
- [Les Sites Web Azure : Comment chaînes d’Application et de connexion chaînes](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Décrit la fonctionnalité Azure qui substitue `appSettings` et `connectionStrings` données dans le *Web.config* fichier.
- [Custom configuration et application des paramètres dans les Sites Web Azure - avec Stefan Schackow](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/).

Pour plus d’informations sur les autres méthodes pour la conservation des informations sensibles en dehors du contrôle de code source, consultez [ASP.NET MVC : conserver les paramètres de Source contrôle privé](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).

> [!div class="step-by-step"]
> [Précédent](automate-everything.md)
> [Suivant](continuous-integration-and-continuous-delivery.md)
