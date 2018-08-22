---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 'Déploiement de Web ASP.NET à l’aide de Visual Studio : Transformations du fichier Web.config | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) un ASP.NET web application dans Azure App Service Web Apps ou à un fournisseur d’hébergement tiers, en utilisant des éléments...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: 58f3daac5e9fbe6d327f19b6ae78ea17a26cd7e7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829232"
---
<a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>Déploiement de Web ASP.NET à l’aide de Visual Studio : Transformations du fichier Web.config
====================
par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Cette série de didacticiels vous montre comment déployer (publier) un ASP.NET web application dans Azure App Service Web Apps ou à un fournisseur d’hébergement tiers, à l’aide de Visual Studio 2012 ou Visual Studio 2010. Pour plus d’informations sur la série, consultez [le premier didacticiel de la série](introduction.md).


## <a name="overview"></a>Vue d'ensemble

Ce didacticiel vous montre comment automatiser le processus de modification de la *Web.config* lors du déploiement pour différents environnements de destination de fichier. La plupart des applications ont des paramètres dans le *Web.config* fichier qui doit être différent lorsque l’application est déployée. Automatisation du processus de ces modifications vous évite de devoir le faire manuellement chaque fois que vous déployez, ce qui serait trop fastidieux et sujet aux erreurs.

Rappel : Si vous obtenez un message d’erreur ou quelque chose ne fonctionne pas lorsque vous parcourez le didacticiel, veillez à consulter le [page Résolution des problèmes](troubleshooting.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Transformations Web.config par rapport aux paramètres Web Deploy

Il existe deux façons d’automatiser le processus de modification *Web.config* les paramètres du fichier : [transformations Web.config](https://msdn.microsoft.com/library/dd465326.aspx) et [paramètres Web Deploy](https://msdn.microsoft.com/library/ff398068.aspx). Un *Web.config* fichier de transformation contient le balisage XML qui spécifie comment modifier le *Web.config* fichier lorsqu’elle est déployée. Vous pouvez spécifier différentes modifications pour spécifique des configurations de build et profils de publication spécifique. Les configurations de build par défaut sont Debug et Release, et vous pouvez créer des configurations de build personnalisée. Un profil de publication correspond généralement à un environnement de destination. (Vous en apprendrez plus sur Publier les profils dans le [déploiement vers IIS comme environnement de Test](deploying-to-iis.md) didacticiel.)

Paramètres de déploiement de Web peuvent être utilisés pour spécifier les différents types de paramètres qui doivent être configurés au cours du déploiement, y compris les paramètres qui sont trouvent dans *Web.config* fichiers. Lorsqu’il est utilisé pour spécifier *Web.config* des modifications de fichiers, les paramètres de Web Deploy sont plus complexes à configurer, mais elles sont utiles lorsque vous ne connaissez pas la valeur à définir jusqu'à ce que vous déployez. Par exemple, dans un environnement d’entreprise, vous pouvez créer un *package de déploiement* et lui donner à une personne du département informatique à installer en production, et cette personne doit être en mesure d’entrer des chaînes de connexion ou mots de passe que vous n’est pas le cas savoir.

Pour le scénario qui couvre cette série de didacticiels, vous savez à l’avance tout ce qui doit être effectuée à le *Web.config* de fichiers, donc vous n’avez pas besoin d’utiliser les paramètres de Web Deploy. Vous devez configurer certaines transformations qui diffèrent selon la configuration de build utilisée, et certaines qui diffèrent selon le profil de publication utilisé.

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Spécifiez les paramètres Web.config dans Azure

Si le *Web.config* sont des paramètres du fichier que vous souhaitez modifier dans le `<connectionStrings>` ou `<appSettings>` élément, et si vous déployez des applications Web dans Azure App Service, vous disposez d’une autre option pour l’automatisation des modifications pendant déploiement. Vous pouvez entrer les paramètres que vous souhaitez prendre effet dans Azure dans le **configurer** onglet de la page de portail de gestion pour votre application web (faites défiler jusqu'à la **paramètres de l’application** et **les chaînes de connexion**  sections). Lorsque vous déployez le projet, Azure applique automatiquement les modifications. Pour plus d’informations, consultez [Sites Web Windows Azure : fonctionnement des chaînes d’Application et de connexion](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

## <a name="default-transformation-files"></a>Fichiers de transformation par défaut

Dans **l’Explorateur de solutions**, développez *Web.config* pour voir les *Web.Debug.config* et *Web.Release.config* fichiers de transformation sont créés par défaut pour les configurations de build par défaut.

![Web.config_transform_files](web-config-transformations/_static/image1.png)

Vous pouvez créer des fichiers de transformation pour les configurations de build personnalisée en double-cliquant sur le fichier Web.config et en choisissant **ajouter des transformations de configuration** dans le menu contextuel. Pour ce didacticiel vous n’avez pas besoin pour ce faire, et l’option de menu est désactivée, car vous n’avez pas créé des configurations de build personnalisée.

Vous allez créer ultérieurement des fichiers de transformation plus trois, et un pour le test, intermédiaire et production des profils de publication. Un exemple typique d’un paramètre que vous pouvez gérer dans un fichier de transformation de profil de publication, car elle dépend de l’environnement de destination est un point de terminaison WCF est différente pour le test ou production. Vous créerez publier les fichiers de transformation de profil dans les didacticiels suivants après avoir créé les profils de publication qui passent avec.

## <a name="disable-debug-mode"></a>Désactiver le mode débogage

Un exemple d’un paramètre qui varie selon la configuration de build au lieu d’environnement de destination est le `debug` attribut. Pour une version Release, vous tenez débogage est désactivé, quel que soit l’environnement dans lequel vous effectuez le déploiement. Par conséquent, par défaut, Visual Studio, modèles de projet créent *Web.Release.config* transformer des fichiers avec du code qui supprime le `debug` attribut à partir de la `compilation` élément. Ici est la valeur par défaut *Web.Release.config*: en plus un exemple de code de transformation qui est commenté, il inclut le code dans le `compilation` élément supprime le `debug` attribut :

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

Le `xdt:Transform="RemoveAttributes(debug)"` attribut spécifie que vous souhaitez le `debug` attribut à supprimer de la `system.web/compilation` élément déployé *Web.config* fichier. Cette opération est effectuée chaque fois que vous déployez une version Release.

## <a name="limit-error-log-access-to-administrators"></a>Limiter l’accès de journal d’erreur pour les administrateurs

Si une erreur s’est alors que l’application s’exécute, l’application affiche une page d’erreur générique à la place de la page d’erreur générés par le système, et il utilise le [package Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) pour la journalisation des erreurs et création de rapports. Le `customErrors` élément dans l’application *Web.config* fichier spécifie la page d’erreur :

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

Pour afficher la page d’erreur, modifiez temporairement le `mode` attribut de la `customErrors` élément à partir de « RemoteOnly » sur « Activé » et exécutez l’application à partir de Visual Studio. Provoque une erreur en demandant une URL non valide, tel que *Studentsxxx.aspx*. Au lieu d’une erreur généré par IIS « la ressource est introuvable » page, vous voyez la *GenericErrorPage.aspx* page.

![Page d’erreur](web-config-transformations/_static/image2.png)

Pour afficher le journal des erreurs, remplacez tous les éléments dans l’URL après le numéro de port avec *elmah.axd* (par exemple, `http://localhost:51130/elmah.axd`) et appuyez sur ENTRÉE :

![Page d’ELMAH](web-config-transformations/_static/image3.png)

N’oubliez pas de définir le `customErrors` élément revenir au mode « RemoteOnly » quand vous avez terminé.

Sur votre ordinateur de développement, il convient d’autoriser l’accès gratuit à la page journal d’erreurs, mais en production qui serait un risque de sécurité. Pour le site de production, vous souhaitez ajouter une règle d’autorisation qui limite l’accès de journal d’erreur pour les administrateurs et pour vous assurer que la restriction fonctionne vous le souhaitez dans le test et intermédiaires également. Par conséquent, il s’agit d’une autre modification que vous souhaitez implémenter chaque fois que vous déployez une version Release, et donc sa place dans le *Web.Release.config* fichier.

Ouvrez *Web.Release.config* et ajoutez un nouveau `location` élément situé juste avant la fermeture `configuration` balise, comme indiqué ici.

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

Le `Transform` cela entraîne la valeur d’attribut de « Insert » `location` élément à ajouter en tant que frère à n’importe quel existant `location` éléments dans le *Web.config* fichier. (Il existe déjà un `location` élément qui spécifie l’autorisation des règles pour le **crédits de la mise à jour** page.)

Maintenant, vous pouvez prévisualiser la transformation s’assurer que codé correctement.

Dans **l’Explorateur de solutions**, avec le bouton droit *Web.Release.config* et cliquez sur **aperçu transformer**.

![Menu Aperçu de transformation](web-config-transformations/_static/image4.png)

Une s’ouvre de page qui vous présente le développement *Web.config* fichier sur la gauche et ce qu’il déployé *Web.config* fichier ressemblera à droite, avec les modifications mises en surbrillance.

![Aperçu de la transformation de débogage](web-config-transformations/_static/image5.png)

![Aperçu de la transformation de l’emplacement](web-config-transformations/_static/image6.png)

(Dans la version préliminaire, vous remarquerez certaines modifications supplémentaires que vous n’avez pas écrit transforme pour : ils impliquent généralement la suppression de l’espace blanc qui n’affecte pas les fonctionnalités.)

Lorsque vous testez le site après le déploiement, vous allez également tester pour vérifier que la règle d’autorisation est appliquée.

> [!NOTE] 
> 
> **Note de sécurité** jamais afficher les détails de l’erreur pour le grand public dans une application de production, ou stocker ces informations dans un emplacement public. Les attaquants peuvent utiliser les informations d’erreur pour découvrir des vulnérabilités dans un site. Si vous utilisez ELMAH dans votre propre application, configurez ELMAH pour réduire les risques de sécurité. L’exemple ELMAH dans ce didacticiel ne doit pas être considérée une configuration recommandée. Il est un exemple qui a été choisi afin d’illustrer comment gérer un dossier que l’application doit être en mesure de créer des fichiers dans. Pour plus d’informations, consultez [sécuriser le point de terminaison ELMAH](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).


## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>Un paramètre que vous allez gérer dans publier des fichiers de transformation de profil

Un scénario courant consiste à avoir *Web.config* paramètres qui doivent être différents dans chaque environnement que vous déployez sur les fichiers. Par exemple, une application qui appelle un service WCF peut avoir besoin d’un autre point de terminaison dans les environnements de test et de production. L’application Contoso University inclut également un paramètre de ce type. Ce paramètre contrôle un indicateur visible sur les pages d’un site qui vous indique l’environnement dans lequel vous vous trouvez dans, telles que le développement, de test ou de production. La valeur du paramètre détermine si l’application permet d’ajouter « (Dev) » ou « (Test) » pour le titre principal dans le *Site.Master* page maître :

![Indicateur d’environnement](web-config-transformations/_static/image7.png)

L’indicateur d’environnement est omis lors de l’application s’exécute dans un environnement intermédiaire ou de production.

Les pages web de Contoso University lire une valeur qui est définie dans `appSettings` dans le *Web.config* fichier afin de déterminer quel environnement de l’application est en cours d’exécution dans :

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

La valeur doit être « Test » dans l’environnement de test et « Prod » pour la préproduction et production.

Le code suivant dans un fichier de transformation implémentera cette transformation :

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

Le `xdt:Transform` attribut la valeur « SetAttributes » indique que l’objectif de cette transformation consiste à modifier les valeurs d’attribut d’un élément existant dans le *Web.config* fichier. Le `xdt:Locator` valeur « Match(key) » indique que l’élément à modifier est celui de l’attribut dont `key` attribut correspond à la `key` attribut spécifié ici. Le seul attribut autres de la `add` élément est `value`, et c’est ce que sera modifié dans déployé *Web.config* fichier. Le code indiqué ici causes le `value` attribut de la `Environment` `appSettings` élément soit définie à « Test » dans le *Web.config* fichier qui est déployé.

Cette transformation appartienne dans les fichiers de transformation de profil de publication, qui vous n’avez pas encore créé. Vous allez créer et mettre à jour les fichiers de transformation qui implémentent cette modification lorsque vous créez les profils de publication pour les environnements de test, intermédiaire et de production. Vous pouvez utiliser la [déployer sur IIS](deploying-to-iis.md) et [déployer en production](deploying-to-production.md) didacticiels.

> [!NOTE]
> Étant donné que ce paramètre se trouve dans le `<appSettings>` élément, vous avez une autre solution pour la spécification de la transformation lorsque vous déployez vers Web Apps dans Azure App Service, voir [Web.config en spécifiant les paramètres dans Azure](#watransforms) plus haut dans Cette rubrique.


## <a name="setting-connection-strings"></a>Définition des chaînes de connexion

Bien que le fichier de transformation par défaut contient un exemple qui montre comment mettre à jour d’une chaîne de connexion, dans la plupart des cas il est inutile de configurer des transformations de chaîne de connexion, car vous pouvez spécifier des chaînes de connexion dans le profil de publication. Vous pouvez utiliser la [déployer sur IIS](deploying-to-iis.md) et [déployer en production](deploying-to-production.md) didacticiels.

## <a name="summary"></a>Récapitulatif

Vous avez maintenant terminé autant que possible avec *Web.config* transformations avant de vous créez les profils de publication, et vous l’avez vu un aperçu de ce que sera dans le fichier Web.config déployé.

![Aperçu de la transformation de l’emplacement](web-config-transformations/_static/image8.png)

Dans ce didacticiel, vous allez prendre soin des tâches de configuration de déploiement qui nécessitent la configuration des propriétés du projet.

## <a name="more-information"></a>Informations complémentaires

Pour plus d’informations sur les sujets traités dans ce didacticiel, consultez [Web.config à l’aide des transformations pour modifier les paramètres dans le fichier Web.config de destination ou le fichier app.config au cours du déploiement](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) dans le plan de contenu de déploiement Web pour Visual Studio et ASP.NET.

> [!div class="step-by-step"]
> [Précédent](preparing-databases.md)
> [Suivant](project-properties.md)
