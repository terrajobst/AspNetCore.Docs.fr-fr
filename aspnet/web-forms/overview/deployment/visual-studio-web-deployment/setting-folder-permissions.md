---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: 'Déploiement de Web ASP.NET à l’aide de Visual Studio : définition des autorisations de dossier | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) un ASP.NET web application dans Azure App Service Web Apps ou à un fournisseur d’hébergement tiers, en utilisant des éléments...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: 0660a464063783406a69caf663036811c8ac818e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802031"
---
<a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a>Déploiement de Web ASP.NET à l’aide de Visual Studio : définition des autorisations de dossier
====================
par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Cette série de didacticiels vous montre comment déployer (publier) un ASP.NET web application dans Azure App Service Web Apps ou à un fournisseur d’hébergement tiers, à l’aide de Visual Studio 2012 ou Visual Studio 2010. Pour plus d’informations sur la série, consultez [le premier didacticiel de la série](introduction.md).


## <a name="overview"></a>Vue d'ensemble

Dans ce didacticiel, vous définissez des autorisations de dossier pour le *Elmah* dossier dans le site web déployé site afin que l’application peut créer des fichiers journaux dans ce dossier.

Lorsque vous testez une application web dans Visual Studio à l’aide du serveur Visual Studio Development (Cassini) ou IIS Express, l’application s’exécute sous votre identité. Vous êtes probablement un administrateur sur votre ordinateur de développement et tout pouvoir rien à faire dans n’importe quel fichier dans n’importe quel dossier. Mais quand une application s’exécute sous IIS, il s’exécute sous l’identité définie pour le pool d’applications auquel appartient le site. Il s’agit généralement d’un compte définie par le système qui a des autorisations limitées. Par défaut, il a lu et les autorisations d’exécution sur les fichiers et dossiers de votre application web, mais qu’elle ait un accès en écriture.

Cela devient un problème si votre application crée ou des fichiers de mises à jour, qui est commune dans les applications web. Dans l’application Contoso University, Elmah crée des fichiers XML dans le *Elmah* dossier afin d’enregistrer les détails sur les erreurs. Même si vous n’utilisez pas quelque chose comme Elmah, votre site peut permettre aux utilisateurs de télécharger des fichiers ou effectuer d’autres tâches qui écrivent des données dans un dossier sur votre site.

Rappel : Si vous obtenez un message d’erreur ou quelque chose ne fonctionne pas lorsque vous parcourez le didacticiel, veillez à consulter le [page Résolution des problèmes](troubleshooting.md).

## <a name="test-error-logging-and-reporting"></a>Journalisation des erreurs de test et de création de rapports

Pour voir comment l’application ne fonctionne pas correctement dans IIS (bien qu’elle le faisait lorsque vous l’avez testée dans Visual Studio), vous pouvez provoquer une erreur qui serait normalement être enregistrée par Elmah, puis ouvrez le journal des erreurs Elmah pour afficher les détails. Si Elmah n’a pas pu créer un fichier XML et de stocker les détails de l’erreur, vous consultez un rapport d’erreurs vide.

Ouvrez un navigateur et accédez à `http://localhost/ContosoUniversity`, et ensuite demander une URL non valide comme *Studentsxxx.aspx*. Vous voyez une page d’erreur générés par le système au lieu du *GenericErrorPage.aspx* page, car le `customErrors` paramètre dans le fichier Web.config est « RemoteOnly » et que vous exécutez IIS localement :

![Page d’erreur 404 HTTP](setting-folder-permissions/_static/image1.png)

Exécutez maintenant *Elmah.axd* pour afficher le rapport d’erreurs. Une fois que vous vous connecter avec les informations d’identification du compte administrateur (&quot;administrateur&quot; et &quot;devpwd&quot;), vous voyez une page de journal d’erreur vide car Elmah n’a pas pu créer un fichier XML dans le *Elmah*dossier :

![Erreur journal vide](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a>Définir une autorisation d’écriture sur le dossier Elmah

Vous pouvez définir manuellement les autorisations du dossier, ou vous pouvez faciliter automatique dans le cadre du processus de déploiement. Rend automatique nécessite un code MSBuild complexe, et étant donné que vous avez uniquement la première fois que vous déployez pour cela, décrites ci-dessous comment le faire manuellement. (Pour plus d’informations sur la façon de rendre cette partie du processus de déploiement, consultez [définition des autorisations de dossier sur le Web Publish](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) sur le blog de Sayed Hashimi.)

1. Dans **Explorateur de fichiers**, accédez à *C:\inetpub\wwwroot\ContosoUniversity*. Avec le bouton droit le *Elmah* dossier, sélectionnez **propriétés**, puis sélectionnez le **sécurité** onglet.
2. Cliquez sur **Modifier**.
3. Dans le **autorisations pour Elmah** boîte de dialogue, sélectionnez **DefaultAppPool**, puis sélectionnez le **écrire** case à cocher dans la **autoriser** colonne.

    ![Autorisations pour ELMAH dossier](setting-folder-permissions/_static/image3.png)

    (Si vous ne voyez pas **DefaultAppPool** dans le **noms d’utilisateur ou groupe** liste, vous avez probablement utilisé une autre méthode que celle spécifiée dans ce didacticiel pour configurer IIS et ASP.NET 4 sur votre ordinateur. Dans ce cas, découvrez de quelle identité est utilisée par le pool d’applications affecté à l’application Contoso University et accorder l’autorisation d’écriture à cette identité. Consultez les liens sur les identités du pool d’applications à la fin de ce didacticiel.) Cliquez sur **OK** dans les deux boîtes de dialogue.

## <a name="retest-error-logging-and-reporting"></a>Retester la journalisation des erreurs et la création de rapports

Testez en provoquant une erreur à nouveau de la même façon (demande une URL incorrecte) et exécutez le **journal des erreurs** page. Cette fois, l’erreur s’affiche sur la page.

![Page de journal d’erreurs ELMAH](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a>Récapitulatif

Vous avez maintenant toutes les tâches nécessaires pour obtenir de Contoso University terminé fonctionne correctement dans IIS sur votre ordinateur local. Dans le didacticiel suivant, vous rendrez le site publiquement accessibles en la déployant sur Azure.

## <a name="more-information"></a>Complément d'information

Dans cet exemple, la raison pour laquelle pourquoi Elmah n’a pas pu enregistrer les fichiers journaux a été assez évident. Vous pouvez utiliser le suivi de IIS dans les cas où la cause du problème n’est pas évidente ; consultez [dépannage des demandes à l’aide de suivi des ayant échoué dans IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) sur le site IIS.net.

Pour plus d’informations sur la façon d’accorder des autorisations pour les identités du pool d’applications, consultez [identités du Pool d’applications](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) et [le contenu sécurisé dans IIS via ACL de système de fichiers](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) sur le site IIS.net.

> [!div class="step-by-step"]
> [Précédent](deploying-to-iis.md)
> [Suivant](deploying-to-production.md)
