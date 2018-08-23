---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
title: 'Déploiement d’une Application de Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : Transformations du fichier Web.Config - 3 sur 12 | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer une ASP.NET (publier) projet d’application web qui inclut une base de données SQL Server Compact à l’aide de Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 2b0df3d9-450b-4ea6-b315-4c9650722cad
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
msc.type: authoredcontent
ms.openlocfilehash: 9673887fb39874e1b4f063fa9cfe3470e4ccde00
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827667"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-webconfig-file-transformations---3-of-12"></a>Déploiement d’une Application de Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : Transformations du fichier Web.Config - 3 sur 12
====================
par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Cette série de didacticiels vous montre comment déployer une ASP.NET (publier) projet d’application web qui inclut une base de données SQL Server Compact à l’aide de Visual Studio 2012 RC ou Visual Studio Express 2012 RC pour le Web. Vous pouvez également utiliser Visual Studio 2010 si vous installez la mise à jour de publication Web. Pour une introduction à la série, consultez [le premier didacticiel de la série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Pour obtenir un didacticiel qui montre les fonctionnalités de déploiement introduites après la version RC de Visual Studio 2012, montre comment déployer des éditions de SQL Server autre que SQL Server Compact et montre comment déployer dans Azure App Service Web Apps, consultez [déploiement Web ASP.NET à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Vue d'ensemble

Ce didacticiel vous montre comment automatiser le processus de modification de la *Web.config* lors du déploiement pour différents environnements de destination de fichier. La plupart des applications ont des paramètres dans le *Web.config* fichier qui doit être différent lorsque l’application est déployée. Automatisation du processus de ces modifications vous évite de devoir le faire manuellement chaque fois que vous déployez, ce qui serait trop fastidieux et sujet aux erreurs.

Rappel : Si vous obtenez un message d’erreur ou quelque chose ne fonctionne pas lorsque vous parcourez le didacticiel, veillez à consulter le [page Résolution des problèmes](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Les Transformations Web.config et Web des paramètres de déploiement

Il existe deux façons d’automatiser le processus de modification *Web.config* les paramètres du fichier : [transformations Web.config](https://msdn.microsoft.com/library/dd465326.aspx) et [paramètres Web Deploy](https://msdn.microsoft.com/library/ff398068.aspx). Un *Web.config* fichier de transformation contient le balisage XML qui spécifie comment modifier le *Web.config* fichier lorsqu’elle est déployée. Vous pouvez spécifier différentes modifications pour spécifique des configurations de build et profils de publication spécifique. Les configurations de build par défaut sont Debug et Release, et vous pouvez créer des configurations de build personnalisée. Un profil de publication correspond généralement à un environnement de destination. (Vous en apprendrez plus sur Publier les profils dans le [déploiement vers IIS comme environnement de Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) didacticiel.)

Paramètres de déploiement de Web peuvent être utilisés pour spécifier les différents types de paramètres qui doivent être configurés au cours du déploiement, y compris les paramètres qui sont trouvent dans *Web.config* fichiers. Lorsqu’il est utilisé pour spécifier *Web.config* des modifications de fichiers, les paramètres de Web Deploy sont plus complexes à configurer, mais elles sont utiles lorsque vous ne connaissez pas la valeur à définir jusqu'à ce que vous déployez. Par exemple, dans un environnement d’entreprise, vous pouvez créer un *package de déploiement* et lui donner à une personne du département informatique à installer en production, et cette personne doit être en mesure d’entrer des chaînes de connexion ou mots de passe que vous n’est pas le cas savoir.

Pour le scénario qui couvre ce didacticiel, vous savez tout ce qui doit être effectuée à le *Web.config* de fichiers, donc vous n’avez pas besoin d’utiliser les paramètres de Web Deploy. Vous devez configurer certaines transformations qui diffèrent selon la configuration de build utilisée, et certaines qui diffèrent selon le profil de publication utilisé.

## <a name="creating-transformation-files-for-publish-profiles"></a>Création de fichiers de Transformation pour les profils de publication

Dans **l’Explorateur de solutions**, développez *Web.config* pour voir les *Web.Debug.config* et *Web.Release.config* fichiers de transformation sont créés par défaut pour les configurations de build par défaut.

![Web.config_transform_files](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image1.png)

Vous pouvez créer des fichiers de transformation pour les configurations de build personnalisée en double-cliquant sur le fichier Web.config et en choisissant **ajouter des transformations de configuration** dans le menu contextuel, mais pour ce didacticiel vous n’avez pas besoin de le faire.

Vous avez besoin des deux autres fichiers de transformation, pour la configuration des modifications qui sont liées à la destination du déploiement plutôt qu’à la configuration de build. Un exemple typique de ce type de paramètre est un point de terminaison WCF est différente pour le test ou production. Dans les didacticiels suivants, vous allez créer des profils nommées Test et Production, donc vous devez de publication un *Web.Test.config* fichier et un *Web.Production.config* fichier.

Les fichiers de transformation qui sont liées aux profils de publication doivent être créées manuellement. Dans **l’Explorateur de solutions**, cliquez sur le projet ContosoUniversity et sélectionnez **ouvrir le dossier dans l’Explorateur Windows**.

![Open_folder_in_Windows_Explorer](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image2.png)

Dans **Windows Explorer**, sélectionnez le *Web.Release.config* de fichiers, copiez le fichier, puis collez les deux copies de ce dernier. Renommer ces copies *Web.Production.config* et *Web.Test.config*, puis fermez **Windows Explorer**.

Dans **l’Explorateur de solutions**, cliquez sur **Actualiser** pour voir les nouveaux fichiers.

Sélectionnez les nouveaux fichiers, avec le bouton droit, puis cliquez sur **inclure dans le projet** dans le menu contextuel.

![Y compris les fichiers de configuration de Test et de Production dans le projet](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image3.png)

Pour empêcher le déploiement de ces fichiers, sélectionnez-les dans **l’Explorateur de solutions**, puis, dans le **propriétés** modification de la fenêtre la **Action de génération** propriété à partir de **Contenu** à **aucun**. (Les fichiers de transformation qui reposent sur les configurations de build ne peuvent pas automatiquement du déploiement.)

Vous êtes maintenant prêt à entrer *Web.config* transformations dans le *Web.config* les fichiers de transformation.

## <a name="limiting-error-log-access-to-administrators"></a>Limiter l’accès de journal d’erreur aux administrateurs

Si une erreur s’est alors que l’application s’exécute, l’application affiche une page d’erreur générique à la place de la page d’erreur générés par le système, et il utilise [package Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) pour la journalisation des erreurs et création de rapports. Le `customErrors` élément dans le *Web.config* fichier spécifie la page d’erreur :

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample1.xml)]

Pour afficher la page d’erreur, modifiez temporairement le `mode` attribut de la `customErrors` élément à partir de « RemoteOnly » sur « Activé » et exécutez l’application à partir de Visual Studio. Provoque une erreur en demandant une URL non valide, tel que *Studentsxxx.aspx*. Au lieu d’une page d’erreur générés par IIS « page introuvable », vous voyez la *GenericErrorPage.aspx* page.

[![Error_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image5.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image4.png)

Pour afficher le journal des erreurs, remplacez tous les éléments dans l’URL après le numéro de port avec *elmah.axd* (pour l’exemple dans la capture d’écran, `http://localhost:51130/elmah.axd`) et appuyez sur ENTRÉE :

[![Elmah_log_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image7.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image6.png)

N’oubliez pas de définir le `customErrors` élément revenir au mode « RemoteOnly » quand vous avez terminé.

Sur votre ordinateur de développement, il convient d’autoriser l’accès gratuit à la page journal d’erreurs, mais en production qui serait un risque de sécurité. Pour le site de production, vous pouvez ajouter une règle d’autorisation qui restreint l’accès au journal erreur seulement par les administrateurs en configurant une transformation dans le *Web.Production.config* fichier.

Ouvrez *Web.Production.config* et ajoutez un nouveau `location` élément situé juste après l’ouverture `configuration` balise, comme indiqué ici. (Assurez-vous que vous ajoutez uniquement le `location` élément et pas le balisage environnant qui est indiqué ici uniquement pour fournir un contexte.)

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample2.xml?highlight=2-9)]

Le `Transform` cela entraîne la valeur d’attribut de « Insert » `location` élément à ajouter en tant que frère à n’importe quel existant `location` éléments dans le *Web.config* fichier. (Il existe déjà un `location` élément qui spécifie l’autorisation des règles pour le **crédits de la mise à jour** page.) Lorsque vous testez le site de production après le déploiement, vous allez tester pour vérifier que cette règle d’autorisation est appliquée.

Vous n’êtes pas obligé de restreindre l’accès de journal d’erreur dans l’environnement de test, donc vous n’êtes pas obligé d’ajouter ce code dans le *Web.Test.config* fichier.

> [!NOTE] 
> 
> **Note de sécurité** jamais afficher les détails de l’erreur pour le grand public dans une application de production, ou stocker ces informations dans un emplacement public. Les attaquants peuvent utiliser les informations d’erreur pour découvrir des vulnérabilités dans un site. Si vous utilisez ELMAH dans votre propre application, veillez à examiner les différentes façons dans lequel ELMAH peut être configuré afin de minimiser les risques de sécurité. L’exemple ELMAH dans ce didacticiel ne doit pas être considérée une configuration recommandée. Il est un exemple qui a été choisi afin d’illustrer comment gérer un dossier que l’application doit être en mesure de créer des fichiers dans.


## <a name="setting-an-environment-indicator"></a>Définition d’un indicateur d’environnement

Un scénario courant consiste à avoir *Web.config* paramètres qui doivent être différents dans chaque environnement que vous déployez sur les fichiers. Par exemple, une application qui appelle un service WCF peut avoir besoin d’un autre point de terminaison dans les environnements de test et de production. L’application Contoso University inclut également un paramètre de ce type. Ce paramètre contrôle un indicateur visible sur les pages d’un site qui vous indique l’environnement dans lequel vous vous trouvez dans, telles que le développement, de test ou de production. La valeur du paramètre détermine si l’application permet d’ajouter « (Dev) » ou « (Test) » pour le titre principal dans le *Site.Master* page maître :

[![Environment_indicator](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image9.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image8.png)

L’indicateur d’environnement est omis lors de l’application s’exécute en production.

Les pages web de Contoso University lire une valeur qui est définie dans `appSettings` dans le *Web.config* fichier afin de déterminer quel environnement de l’application est en cours d’exécution dans :

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample3.xml)]

La valeur doit être « Test » dans l’environnement de test et « Prod » dans l’environnement de production.

Ouvrez *Web.Production.config* et ajoutez un `appSettings` élément situé juste avant la balise d’ouverture de la `location` élément que vous avez ajouté précédemment :

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample4.xml)]

Le `xdt:Transform` attribut la valeur « SetAttributes » indique que l’objectif de cette transformation consiste à modifier les valeurs d’attribut d’un élément existant dans le *Web.config* fichier. Le `xdt:Locator` valeur « Match(key) » indique que l’élément à modifier est celui de l’attribut dont `key` attribut correspond à la `key` attribut spécifié ici. Le seul attribut autres de la `add` élément est `value`, et c’est ce que sera modifié dans déployé *Web.config* fichier. Ce code provoque la `value` attribut de la `Environment` `appSettings` élément à être définie sur « Prod » le *Web.config* fichier qui est déployée en production.

Ensuite, appliquez la même modification à *Web.Test.config* fichier, que de définir le `value` à « Test » au lieu de « Prod ». Lorsque vous avez terminé, le `appSettings` section *Web.Test.config* ressemblera à l’exemple suivant :

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample5.xml)]

## <a name="disabling-debug-mode"></a>Désactiver le Mode de débogage

Pour une version Release, vous ne souhaitez pas activer le débogage, quel que soit l’environnement dans lequel vous effectuez le déploiement. Par défaut le *Web.Release.config* fichier de transformation est automatiquement créé avec le code qui supprime le `debug` attribut à partir de la `compilation` élément :

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample6.xml)]

Le `Transform` attribut provoque la `debug` attribut omission de déployé *Web.config* fichier chaque fois que vous déployez une version Release.

Cette transformation même est Test et de Production les fichiers de transformation, car vous les avez créés en copiant le fichier de transformation de mise en production. Vous n’avez pas besoin de dupliquer il, chacun de ces fichiers, ouvrez, supprimez le **compilation** élément et enregistrez et fermez chaque fichier.

## <a name="setting-connection-strings"></a>Définition des chaînes de connexion

Dans la plupart des cas il est inutile de configurer des transformations de chaîne de connexion, car vous pouvez spécifier des chaînes de connexion dans le profil de publication. Mais il existe une exception lorsque vous déployez une base de données SQL Server Compact et vous utilisez des Migrations Entity Framework Code First pour mettre à jour de la base de données sur le serveur de destination. Pour ce scénario, vous devez spécifier une chaîne de connexion supplémentaires qui sera utilisée pour la mise à jour le schéma de base de données sur le serveur. Pour configurer cette transformation, ajoutez un **&lt;connectionStrings&gt;** élément situé juste après l’ouverture **&lt;configuration&gt;** balise dans les deux le *Web.Test.config* et *Web.Production.config* transformer des fichiers :

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample7.xml)]

Le `Transform` attribut spécifie que cette chaîne de connexion est ajoutée à la *connectionStrings* élément déployé *Web.config* fichier. (Le processus de publication crée automatiquement cette chaîne de connexion supplémentaires pour vous si elle n’existe pas, mais par défaut le **providerName** attribut Obtient la valeur `System.Data.SqlClient`, qui ne fonctionne pas pas pour SQL Server Compact. En ajoutant manuellement la chaîne de connexion, vous gardez le processus de déploiement à partir de la création d’un élément de chaîne de connexion avec le nom du fournisseur incorrect.)

Vous avez spécifié maintenant toutes les *Web.config* transformations dont vous avez besoin pour le déploiement de l’application Contoso University afin de tester et production. Dans ce didacticiel, vous allez prendre soin des tâches de configuration de déploiement qui nécessitent la configuration des propriétés du projet.

## <a name="more-information"></a>Informations complémentaires

Pour plus d’informations sur les sujets traités dans ce didacticiel, consultez le scénario de transformation Web.config dans [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521.aspx).

> [!div class="step-by-step"]
> [Précédent](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
> [Suivant](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
