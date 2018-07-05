---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: 'Déploiement d’une Application de Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : déploiement dans l’environnement de Production - 7 sur 12 | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer une ASP.NET (publier) projet d’application web qui inclut une base de données SQL Server Compact à l’aide de Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: 26a832fd336f886ba1ddfb1682930afa4df56c58
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383634"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>Déploiement d’une Application de Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio ou Visual Web Developer : déploiement dans l’environnement de Production - 7 sur 12
====================
par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Cette série de didacticiels vous montre comment déployer une ASP.NET (publier) projet d’application web qui inclut une base de données SQL Server Compact à l’aide de Visual Studio 2012 RC ou Visual Studio Express 2012 RC pour le Web. Vous pouvez également utiliser Visual Studio 2010 si vous installez la mise à jour de publication Web. Pour une introduction à la série, consultez [le premier didacticiel de la série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Pour obtenir un didacticiel qui montre les fonctionnalités de déploiement introduites après la version RC de Visual Studio 2012, montre comment déployer des éditions de SQL Server autre que SQL Server Compact et montre comment déployer dans Azure App Service Web Apps, consultez [déploiement Web ASP.NET à l’aide de Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Vue d'ensemble

Dans ce didacticiel, vous configurez un compte avec un fournisseur d’hébergement et déployer votre application ASP.NET application web à l’environnement de production à l’aide de Visual Studio clic publier la fonctionnalité.

Rappel : Si vous obtenez un message d’erreur ou quelque chose ne fonctionne pas lorsque vous parcourez le didacticiel, veillez à consulter le [page Résolution des problèmes](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="selecting-a-hosting-provider"></a>Sélection d’un fournisseur d’hébergement

Pour l’application Contoso University et cette série de didacticiels, vous avez besoin d’un fournisseur qui prend en charge ASP.NET 4 et Web Deploy. Une société d’hébergement spécifique a été choisie afin que les didacticiels pourraient illustrer l’expérience complète de déploiement sur un site Web actif. Chaque société d’hébergement fournit différentes fonctionnalités et l’expérience de déploiement sur leurs serveurs varie légèrement. Toutefois, la procédure décrite dans ce didacticiel est généralement utilisée pour l’ensemble du processus. Le fournisseur d’hébergement pour ce didacticiel, Cytanium.com, est un des nombreux qui sont disponibles, et son utilisation dans ce didacticiel ne constitue pas une approbation ou la recommandation.

Lorsque vous êtes prêt à sélectionner votre propre fournisseur d’hébergement, vous pouvez comparer les fonctionnalités et les prix dans la [galerie de fournisseurs](https://www.microsoft.com/web/hosting) sur le site Web Microsoft.com.

## <a name="creating-an-account"></a>Création d’un compte

Créez un compte auprès de votre fournisseur sélectionné. Si prise en charge une base de données SQL Server complète est un ajout supplémentaire, vous n’avez pas besoin de le sélectionner pour ce didacticiel, mais vous en aurez besoin pour le [migration vers SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) didacticiel plus loin dans cette série.

Pour ces didacticiels, vous n’êtes pas obligé d’inscrire un nouveau nom de domaine. Vous pouvez tester pour vérifier la réussite du déploiement à l’aide de l’URL temporaire attribué au site par le fournisseur.

Une fois que le compte a été créé, vous recevez généralement un e-mail de bienvenue qui contient toutes les informations que vous avez besoin pour déployer et gérer votre site. Les informations qui vous envoie par votre fournisseur d’hébergement seront similaires à celles indiquées ici. L’e-mail de bienvenue Cytanium est envoyé aux nouveaux propriétaires de comptes comprend les informations suivantes :

- URL vers le site du fournisseur contrôle panneau, où vous pouvez gérer les paramètres de votre site. L’ID et le mot de passe spécifiés sont inclus dans cette partie de l’e-mail de bienvenue pour faciliter la référence. (Les deux ont été modifiées pour une valeur de démonstration pour cette illustration.)

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- La version du .NET Framework par défaut et les informations sur la modification. Nombreux hébergement sites par défaut vers la version 2.0, qui fonctionne avec les applications ASP.NET qui ciblent le .NET Framework 2.0, 3.0 ou 3.5. Toutefois Contoso University étant une application .NET Framework 4, vous devez modifier ce paramètre. (Pour une application ASP.NET 4.5 vous utiliseriez le paramètre .NET 4.0.)

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- L’URL temporaire que vous pouvez utiliser pour accéder à votre site web. Lorsque ce compte a été créé, « contosouniversity.com » a été entré en tant que le nom de domaine existant. Par conséquent, l’URL temporaire est `http://contosouniversity.com.vserver01.cytanium.com`.

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- Informations sur la configuration des bases de données et les chaînes de connexion dont vous avez besoin afin d’y accéder :

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- Informations sur les outils et les paramètres pour le déploiement de votre site. (Le courrier électronique à partir de Cytanium mentionne également WebMatrix, qui est omis ici.)

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>Définition de la Version du .NET Framework

L’e-mail de bienvenue Cytanium inclut un lien pour obtenir des instructions sur la modification de la version du .NET Framework. Ces instructions expliquent que cela est possible via le panneau de configuration Cytanium. Autres fournisseurs ont des sites de panneau de contrôle qui paraissent différentes, ou il peuvent vous demander de procéder de manière différente.

Accédez à l’URL de panneau de contrôle. Une fois connecté avec votre nom d’utilisateur et le mot de passe, vous consultez le panneau de configuration.

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

Dans le **espaces hébergement** boîte, maintenez le pointeur sur l’icône Web et sélectionnez **Sites Web** dans le menu.

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

Dans le **Sites Web** , cliquez sur **contosouniversity.com** (le nom du site que vous avez utilisé lorsque vous avez créé le compte).

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

Dans le **propriétés du Site Web** boîte, sélectionnez le **Extensions** onglet.

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

Changer d’ASP.NET à partir de **2.0 Pipeline intégré** à **4.0 (Pipeline intégré)**, puis cliquez sur **mise à jour**.

## <a name="publishing-to-the-hosting-provider"></a>Publication sur le fournisseur d’hébergement

L’e-mail de Bienvenue à partir du fournisseur d’hébergement inclut tous les paramètres que vous avez besoin pour publier le projet, et vous pouvez entrer manuellement ces informations dans un profil de publication. Mais vous allez utiliser un plus facile et moins sujette aux erreurs méthode pour configurer le déploiement vers le fournisseur : vous allez télécharger un *.publishsettings* de fichiers et les importer dans un profil de publication.

Dans votre navigateur, accédez au panneau de commande Cytanium et sélectionnez **Web** , puis sélectionnez **Sites Web.**

![Le panneau de configuration en sélectionnant des Sites Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

Sélectionnez le **contosouniversity.com** site web.

![Le panneau de configuration en sélectionnant contosouniversity.com](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

Sélectionnez le **publication Web** onglet.

![Onglet de contrôle de publication sur le panneau de configuration Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

Créer des informations d’identification à utiliser pour le web de publication en entrant un nom d’utilisateur et le mot de passe. Vous pouvez entrer les informations d’identification que vous utilisez pour vous connecter au panneau de commande. Puis cliquez sur **activer**.

![Le panneau de configuration créer les informations d’identification de publication](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

Cliquez sur **télécharger le profil de publication pour ce site web**.

![Profil de publication de contrôler le téléchargement du Panneau de configuration](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

Lorsque vous êtes invité à ouvrir ou enregistrer le fichier, enregistrez-le.

![Enregistrez le fichier de profil de publication](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

Dans **l’Explorateur de solutions** dans Visual Studio, cliquez sur le projet ContosoUniversity et sélectionnez **publier**. Le **publier le site Web** boîte de dialogue s’ouvre sur la **aperçu** onglet avec le **Test** profil est sélectionné, car c’est le dernier profil que vous avez utilisé.

Sélectionnez le **profil** onglet, puis cliquez sur **importation**.

![Web Assistant Importation bouton Publier](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

Dans le **importer les paramètres de publication** boîte de dialogue, sélectionnez le *.publishsettings* fichier que vous avez téléchargé, puis cliquez sur **Open**. L’Assistant avance à l’onglet connexion avec tous les champs renseignés.

![Publication : onglet Web Assistant connexion](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

Le fichier .publishsettings place l’URL permanente planifié pour le site dans la zone URL de Destination, mais si vous n’avez pas acheté ce domaine encore, remplacez la valeur par l’URL temporaire. Pour cet exemple, l’URL est  *[ http://contosouniversity.com.vserver01.cytanium.com ](http://contosouniversity.com.vserver01.cytanium.com).* Le seul objectif de cette zone consiste à spécifier les URL que le navigateur s’ouvre pour automatiquement après avoir correctement après le déploiement. Si vous laissez vide, la seule conséquence est que le navigateur ne démarre pas automatiquement après le déploiement.

Cliquez sur **valider la connexion** pour vérifier que les paramètres sont corrects et que vous pouvez vous connecter au serveur. Comme vous l’avez vu précédemment, une coche verte vérifie que la connexion est établie.

Lorsque vous cliquez sur Valider la connexion, vous pouvez voir un **erreur de certificat** boîte de dialogue. Si vous le faites, vérifiez que le nom du serveur est ce que vous attendez. Dans le cas, sélectionnez **enregistrer ce certificat pour les sessions ultérieures de Visual Studio** et cliquez sur **Accept**. (Cette erreur signifie que le fournisseur d’hébergement a choisi d’éviter de devoir acheter un certificat SSL pour l’URL que vous effectuez le déploiement. Si vous préférez établir une connexion sécurisée à l’aide d’un certificat valide, contactez votre fournisseur d’hébergement.)

![Erreur de certificat](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

Cliquez sur **Suivant**.

Dans le **bases de données** section de la **paramètres** , entrez les mêmes valeurs que vous avez entré pour le Test de profil de publication. Vous trouverez les chaînes de connexion que vous avez besoin dans les listes déroulantes.

- Dans la zone chaîne de connexion pour **SchoolContext,** sélectionnez `Data Source=|DataDirectory|School-Prod.sdf`
- Sous **SchoolContext**, sélectionnez **appliquer des Migrations Code First**.
- Dans la zone chaîne de connexion pour **DefaultConnection**, sélectionnez `Data Source=|DataDirectory|aspnet-Prod.sdf`
- Sous **DefaultConnection**, laissez **base de données de mise à jour** désactivée.

![Publication : onglet Paramètres Assistant Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

Cliquez sur **Suivant**.

Dans le **aperçu** , cliquez sur **démarrer l’aperçu** pour afficher la liste des fichiers qui seront copiés. Vous voyez la même liste que vous avez vu plus tôt lors du déploiement vers IIS sur l’ordinateur local.

Avant de publier, modifier le nom du profil afin que votre fichier de transformation Web.Production.config sera appliqué. Sélectionnez le **profil** onglet et cliquez sur **gérer les profils**.

![Web Assistant Gérer les profils de publication](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

Dans le **modifier les profils de publication Web** boîte de dialogue, sélectionnez le profil de production, cliquez sur **renommer**, puis remplacez le nom de profil en Production. Puis cliquez sur **fermer**.

![Modifier la boîte de dialogue profils de publication Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

Cliquez sur **Publier**.

L’application est publiée dans le fournisseur d’hébergement. Le résultat s’affiche dans le **sortie** fenêtre.

![Fenêtre sortie après le déploiement](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

Le navigateur s’ouvre automatiquement à l’URL que vous avez entré dans **URL de Destination** zone sur le **connexion** onglet de la **publier le site Web** Assistant. Vous voyez la page d’accueil même lorsque vous exécutez le site dans Visual Studio, mais maintenant il n’existe aucun « (Test) » ou d’un indicateur d’environnement « (Dev) » dans la barre de titre. Cela indique que l’indicateur d’environnement *Web.config* transformation a fonctionné correctement.

> [!NOTE]
> Si vous voyez toujours « (Test) » dans le titre, supprimez le *obj* dossier le projet de ContosoUniversity et de redéploiement. Dans les versions préliminaires du logiciel, le fichier de transformation précédemment appliquées (Web.Test.config) peut obtenir appliqué à nouveau même si vous utilisez le profil de Production.


[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

Avant d’exécuter une page qui provoque l’accès de base de données, assurez-vous que Elmah sera en mesure de consignation des erreurs qui se produisent.

## <a name="setting-folder-permissions-for-elmah"></a>Définition des autorisations de dossier pour Elmah

Comme vous vous souvenez du didacticiel précédent de cette série, il se peut que vous devez vous assurer que l’application dispose des autorisations d’écriture pour le dossier dans votre application où Elmah stocke les fichiers journaux des erreurs. Lors du déploiement vers IIS localement sur votre ordinateur, vous définissez manuellement ces autorisations. Dans cette section, vous verrez comment définir des autorisations au Cytanium. (Certains fournisseurs d’hébergement ne permettent pas cela ; il peut offrir un ou plusieurs dossiers prédéfinis avec des autorisations d’écriture. Dans ce cas vous devrez modifier votre application pour utiliser les dossiers spécifiés.)

Vous pouvez définir des autorisations de dossier dans le panneau de configuration Cytanium. Accédez au contrôle de panneau URL et sélectionnez **Gestionnaire de fichiers**.

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

Dans le **Gestionnaire de fichiers** boîte, sélectionnez **contosouniversity.com** , puis **wwwrooot** pour afficher le dossier racine de l’application. Cliquez sur l’icône de cadenas en regard **Elmah**.

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

Dans le **fichier**/**autorisations du dossier** fenêtre, sélectionnez le **en lecture** et **écrire** cases à cocher pour  **contosouniversity.com** et cliquez sur **définir des autorisations**.

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

Assurez-vous que Elmah a accès en écriture à la *Elmah* dossier en provoquant une erreur, puis en affichant le rapport d’erreurs Elmah. Demander une URL non valide comme *Studentsxxx.aspx*. Comme auparavant, vous voyez la *GenericErrorPage.aspx* page. Cliquez sur le **se déconnecter** lier, puis exécutez *Elmah.axd*. Vous obtenez le **Log In** page tout d’abord, ce qui valide le fait que le *Web.config* transformation ajouté avec succès d’autorisation Elmah. Une fois que vous êtes connecté, vous voyez le rapport qui affiche l’erreur que vous venez d’origine.

[![Elmah.axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>Test dans l’environnement de Production

Exécutez le **étudiants** page. L’application essaie d’accéder à la base de données School pour la première fois, ce qui déclenche les Migrations Code First pour créer la base de données. Lorsque la page s’affiche après le délai de tout moment, il montre qu’il n’y a aucun étudiant.

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

Exécutez le **formateurs** page pour vérifier que les données d’amorçage insérées avec succès les données des formateurs dans la base de données.

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

Comme vous l’avez fait dans l’environnement de test, que vous souhaitez vérifier que les mises à jour de la base de données fonctionnent dans l’environnement de production, mais en règle générale, vous ne souhaitez pas entrer des données de test dans votre base de données de production. Pour ce didacticiel, vous allez utiliser la même méthode que vous l’avez fait dans le test. Mais dans une application réelle, que vous souhaiterez sans doute trouver une méthode qui valide de cette base de données mises à jour réussissent sans introduire de données de test dans la base de données de production. Dans certaines applications, il peut être pratique ajouter un élément, puis le supprimer.

Ajoutez un étudiant, puis affichez les données que vous avez entré dans le **étudiants** page pour vérifier que vous pouvez mettre à jour des données dans la base de données.

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

Vérifiez que les règles d’autorisation sont fonctionne correctement en sélectionnant **crédits de la mise à jour** à partir de la **cours** menu. Le **Log In** page s’affiche. Entrez vos informations d’identification du compte administrateur, cliquez sur **Log In**et le **mise à jour crédits** page s’affiche.

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

Si la connexion est réussie, le **mise à jour crédits** page s’affiche. Cela indique que la base de données d’appartenance ASP.NET (avec le compte administrateur unique) a été déployé avec succès.

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

Vous disposez maintenant de déployer et testé votre site et il est disponible publiquement sur Internet.

## <a name="creating-a-more-reliable-test-environment"></a>Création d’un environnement de Test plus fiable

Comme expliqué dans la [déploiement dans l’environnement de Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) didacticiel, l’environnement de test plus fiable serait un deuxième compte au fournisseur d’hébergement qui a comme le compte de production. Il s’agit plus onéreux qu’utiliser IIS local comme environnement de test, dans la mesure où vous seriez obligé de s’inscrire à un deuxième compte d’hébergement. Mais si elle empêche les erreurs de site de production ou de pannes, vous pouvez décider que cela vaut la peine.

La majeure partie du processus de création et de déploiement pour un compte de test est similaire à ce que vous avez déjà fait pour déployer en production :

- Créer un *Web.config* fichier de transformation.
- Créer un compte au fournisseur d’hébergement.
- Créer un nouveau profil de publication et déployez sur le compte de test.

### <a name="preventing-public-access-to-the-test-site"></a>Empêche l’accès Public sur le Site de Test

Une considération importante pour le compte de test est qu’il sera en direct sur Internet, mais vous ne souhaitez pas le public à l’utiliser. Pour conserver le site privé, vous pouvez utiliser un ou plusieurs des méthodes suivantes :

- Contactez le fournisseur d’hébergement pour définir des règles de pare-feu qui autorisent l’accès au site de test uniquement à partir d’adresses IP que vous utilisez pour le test.
- Masquer l’URL afin qu’il n’est pas similaire à l’URL du site public.
- Utilisez un *robots.txt* fichier pour vous assurer que les moteurs de recherche pas analysera les liens de site et les rapports de test à ce dernier dans les résultats de la recherche.

La première de ces méthodes est évidemment le plus sécurisé, mais la procédure qui est spécifique à chaque fournisseur d’hébergement et n’est pas traitée dans ce didacticiel. Si vous effectuez l’organiser avec votre fournisseur d’hébergement pour autoriser uniquement votre adresse IP accéder à l’URL de compte de test, en théorie n’avoir à vous soucier de l’analyse il de moteurs de recherche. Mais même dans ce cas, déployez un *robots.txt* fichier est une bonne idée en tant que sauvegarde au cas où cette règle de pare-feu est jamais accidentellement désactivée.

Le *robots.txt* fichier est placé dans votre dossier de projet et doit contenir le texte suivant :

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

Le `User-agent` ligne indique les moteurs de recherche les règles dans le fichier s’appliquent à tous les web robots d’indexation (robots), et le `Disallow` ligne spécifie qu’aucune page sur le site ne doit être analysé.

Vous ne voulez probablement pas que les moteurs de recherche pour le catalogue de votre site de production, vous devez exclure ce fichier de déploiement de production. Pour ce faire, consultez **exclure certains fichiers ou dossiers de déploiement ?** dans [Forum aux questions du déploiement de projet d’Application ASP.NET Web](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment). Assurez-vous que vous spécifiez l’exclusion uniquement pour le profil de publication de la Production.

Créer un deuxième compte d’hébergement est une approche à travailler avec un environnement de test qui n’est pas obligatoire, mais peut être intéressant de la dépense supplémentaire. Dans les didacticiels suivants, vous allez continuer à utiliser IIS comme votre environnement de test.

Dans le didacticiel suivant, vous allez mettre à jour le code d’application et déploient vos modifications dans les environnements de test et de production.

> [!div class="step-by-step"]
> [Précédent](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
> [Suivant](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
