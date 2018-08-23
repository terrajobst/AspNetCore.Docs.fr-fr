---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: 'Déploiement d’une Application de Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : définition des autorisations dossier - 6 de 12 | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer une ASP.NET (publier) projet d’application web qui inclut une base de données SQL Server Compact à l’aide de Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 81d5107c7e25a10218d13175c47913c94e9ab3fd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837721"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>Déploiement d’une Application de Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : définition des autorisations dossier - 6 de 12
====================
par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Cette série de didacticiels vous montre comment déployer une ASP.NET (publier) projet d’application web qui inclut une base de données SQL Server Compact à l’aide de Visual Studio 2012 RC ou Visual Studio Express 2012 RC pour le Web. Vous pouvez également utiliser Visual Studio 2010 si vous installez la mise à jour de publication Web. Pour une introduction à la série, consultez [le premier didacticiel de la série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Pour obtenir un didacticiel qui montre les fonctionnalités de déploiement introduites après la version RC de Visual Studio 2012, montre comment déployer des éditions de SQL Server autre que SQL Server Compact et montre comment déployer dans Azure App Service Web Apps, consultez [déploiement Web ASP.NET à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Vue d'ensemble

Dans ce didacticiel, vous définissez des autorisations de dossier pour le *Elmah* dossier dans le site web déployé site afin que l’application peut créer des fichiers journaux dans ce dossier.

Lorsque vous testez une application web dans Visual Studio à l’aide du serveur Visual Studio Development (Cassini), l’application s’exécute sous votre identité. Vous êtes probablement un administrateur sur votre ordinateur de développement et tout pouvoir rien à faire dans n’importe quel fichier dans n’importe quel dossier. Mais quand une application s’exécute sous IIS, il s’exécute sous l’identité définie pour le pool d’applications auquel appartient le site. Il s’agit généralement d’un compte définie par le système qui a des autorisations limitées. Par défaut, il a lu et les autorisations d’exécution sur les fichiers et dossiers de votre application web, mais qu’elle ait un accès en écriture.

Cela devient un problème si votre application crée ou des fichiers de mises à jour, qui est commune dans les applications web. Dans l’application Contoso University, Elmah crée des fichiers XML dans le *Elmah* dossier afin d’enregistrer les détails sur les erreurs. Même si vous n’utilisez pas quelque chose comme Elmah, votre site peut permettre aux utilisateurs de télécharger des fichiers ou effectuer d’autres tâches qui écrivent des données dans un dossier sur votre site.

Rappel : Si vous obtenez un message d’erreur ou quelque chose ne fonctionne pas lorsque vous parcourez le didacticiel, veillez à consulter le [page Résolution des problèmes](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="testing-error-logging-and-reporting"></a>Journalisation des erreurs et création de rapports de test

Pour voir comment l’application ne fonctionne pas correctement dans IIS (bien qu’elle le faisait lorsque vous l’avez testée dans Visual Studio), vous pouvez provoquer une erreur qui serait normalement être enregistrée par Elmah, puis ouvrez le journal des erreurs Elmah pour afficher les détails. Si Elmah n’a pas pu créer un fichier XML et de stocker les détails de l’erreur, vous consultez un rapport d’erreurs vide.

Ouvrez un navigateur et accédez à `http://localhost/ContosoUniversity`, et ensuite demander une URL non valide comme *Studentsxxx.aspx*. Vous voyez une page d’erreur générés par le système au lieu du *GenericErrorPage.aspx* page, car le `customErrors` paramètre dans le fichier Web.config est « RemoteOnly » et que vous exécutez IIS localement :

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

Exécutez maintenant *Elmah.axd* pour afficher le rapport d’erreurs. Vous voyez une page de journal d’erreur vide car Elmah n’a pas pu créer un fichier XML dans le *Elmah* dossier :

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>Paramètre autorisation d’écriture sur le dossier Elmah

Vous pouvez définir manuellement les autorisations du dossier, ou vous pouvez faciliter automatique dans le cadre du processus de déploiement. Rend automatique nécessite un code MSBuild complexe et étant donné que vous ne devez procéder à la première fois que vous déployez, ce didacticiel montre uniquement comment le faire manuellement. (Pour plus d’informations sur la façon de rendre cette partie du processus de déploiement, consultez [définition des autorisations de dossier sur le Web Publish](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) sur le blog de Sayed Hashimi.)

Dans **Windows Explorer**, accédez à *C:\inetpub\wwwroot\ContosoUniversity*. Avec le bouton droit le *Elmah* dossier, sélectionnez **propriétés**, puis sélectionnez le **sécurité** onglet.

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(Si vous ne voyez pas **DefaultAppPool** dans le **noms d’utilisateur ou groupe** liste, vous avez probablement utilisé une autre méthode que celle spécifiée dans ce didacticiel pour configurer IIS et ASP.NET 4 sur votre ordinateur. Dans ce cas, découvrez de quelle identité est utilisée par le pool d’applications affecté à l’application Contoso University et accorder l’autorisation d’écriture à cette identité. Consultez les liens sur les identités du pool d’applications à la fin de ce didacticiel.)

Cliquez sur **Modifier**. Dans le **autorisations pour Elmah** boîte de dialogue, sélectionnez **DefaultAppPool**, puis sélectionnez le **écrire** case à cocher dans la **autoriser** colonne.

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

Cliquez sur **OK** dans les deux boîtes de dialogue.

## <a name="retesting-error-logging-and-reporting"></a>Un nouveau test de la journalisation des erreurs et création de rapports

Testez en provoquant une erreur à nouveau de la même façon (demande une URL incorrecte) et exécutez le **journal des erreurs** page. Cette fois, l’erreur s’affiche sur la page.

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

Vous devez également écrire des autorisations sur le *application\_données* dossier, car vous avez des fichiers de base de données SQL Server Compact dans ce dossier, et que vous souhaitez être en mesure de mettre à jour des données dans ces bases de données. Dans ce cas, toutefois, vous n’avez rien à faire supplémentaire, car le processus de déploiement définit automatiquement les autorisations d’écriture sur le *application\_données* dossier.

Vous avez maintenant toutes les tâches nécessaires pour obtenir de Contoso University terminé fonctionne correctement dans IIS sur votre ordinateur local. Dans le didacticiel suivant, vous rendrez le site publiquement accessibles en la déployant sur un fournisseur d’hébergement.

## <a name="more-information"></a>Informations complémentaires

Dans cet exemple, la raison pour laquelle pourquoi Elmah n’a pas pu enregistrer les fichiers journaux a été assez évident. Vous pouvez utiliser le suivi de IIS dans les cas où la cause du problème n’est pas évidente ; consultez [dépannage des demandes à l’aide de suivi des ayant échoué dans IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) sur le site IIS.net.

Pour plus d’informations sur la façon d’accorder des autorisations pour les identités du pool d’applications, consultez [identités du Pool d’applications](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) et [le contenu sécurisé dans IIS via ACL de système de fichiers](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) sur le site IIS.net.

> [!div class="step-by-step"]
> [Précédent](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
> [Suivant](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
