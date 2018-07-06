---
uid: whitepapers/aspnet-web-deployment-content-map
title: Ressources recommandées de déploiement Web ASP.NET - | Microsoft Docs
author: rick-anderson
description: Cette rubrique fournit des liens vers la documentation de ressources sur le déploiement (publier) ASP.NET web applications vers IIS à l’aide de Visual Studio 2010, Visual Web De...
ms.author: aspnetcontent
ms.date: 03/14/2014
ms.assetid: 58b583cd-c4ab-47a3-8527-8c92c298c91f
msc.legacyurl: /whitepapers/aspnet-web-deployment-content-map
msc.type: content
ms.openlocfilehash: f46a588144fb1af958737b07b73acbff60318fe6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809418"
---
<a name="aspnet-web-deployment---recommended-resources"></a>Ressources recommandées de déploiement Web ASP.NET-
====================
> Cette rubrique fournit des liens vers la documentation de ressources sur le déploiement (publier) ASP.NET web applications vers IIS à l’aide de Visual Studio 2010, Visual Web Developer 2010 et versions ultérieures.
> 
> Si vous connaissez un excellent blog valider, [stackoverflow](http://stackoverflow.com) thread ou un autre lien qui serait utile, [nous envoyer un e-mail](mailto:aspnetue@microsoft.com?subject=Deployment Content Map) avec le lien.
> 
> > [!NOTE] 
> > 
> > La plupart de ces ressources décrivent les fonctionnalités de déploiement qui sont disponibles uniquement si vous installez une version récente de la [Visual Studio Web publier mise à jour](https://go.microsoft.com/fwlink/?LinkID=208120). Certaines fonctionnalités sont uniquement disponibles dans Visual Studio 2012 ou Visual Studio 2013.


Cette rubrique contient les sections suivantes :

- [Présentation des options de déploiement pour les projets web](#understanding)
- [Recherche d’une application ASP.NET, les fournisseurs d’hébergement](#findinghosting)
- [Déploiement d’une application web à partir de Visual Studio](#fromvs)
- [Déploiement d’une application web en créant et en installant un package de déploiement web](#package)
- [Déploiement d’une application web à l’aide d’un processus d’intégration continue (CI)](#ci)
- [Pour modifier les paramètres dans le fichier Web.config de destination ou le fichier app.config au cours du déploiement à l’aide de transformations Web.config](#transforms)
- [À l’aide des paramètres Web Deploy pour modifier les paramètres dans l’application web de destination pendant le déploiement](#webdeployparms)
- [Assurant une application est hors connexion pendant le déploiement](#appoffline)
- [Déploiement d’une base de données ou des modifications à une base de données dans le cadre du déploiement d’applications web](#databasewithweb)
- [Déploiement d’une base de données séparément de déploiement d’applications web](#databaseseparate)
- [Déploiement d’une application web qui utilise ASP.NET application des services tels que les appartenances et de profilage](#aspnetmembership)
- [Précompilation pour le déploiement](#precompiling)
- [Déploiement d’une application de web intranet](#intranet)
- [Automatisation des tâches de déploiement courantes qui ne sont pas automatisés prêts à l’emploi](#automating)
- [Configuration de serveurs web afin que les développeurs peuvent déployer des applications web à l’aide de Web Deploy](#configuringservers)
- [Configuration de serveurs pour un fournisseur d’hébergement](#hostingprovider)
- [Résolution des problèmes de déploiement](#troubleshooting)
- [Obtention d’aide avec une question de déploiement spécifique](#gettinghelp)
- [Ressources supplémentaires pour MSBuild](#additional)


<a id="understanding"></a>


## <a name="understanding-deployment-options-for-web-projects"></a>Présentation des options de déploiement pour les projets web

- [Vue d’ensemble du déploiement de Web pour Visual Studio et ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx) (MSDN).
- [Comment déployer un Site Web Microsoft Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Explique les options et des liens vers des ressources pour le déploiement de projets web pour Sites Web Windows Azure, y compris [livraison continue](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) (automatisés à partir de [contrôle de code source](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)) ainsi qu’à l’aide de Visual Studio.
- [Améliorations de la publication Visual Studio 2012 Web](../visual-studio/overview/2012/visual-studio-2012-web-publishing-improvements.md) (vidéo par Scott Hanselman).
- [Billet de vue d’ensemble pour le déploiement Web dans VS 2010](http://vishaljoshi.blogspot.com/2009/09/overview-post-for-web-deployment-in-vs.html) (blog de Vishal). Un billet de blog plus anciens, mais certaines des ressources de Visual Studio 2010, qu'il est lié pour disposer des informations qui sont toujours applicables à Visual Studio 2012.


<a id="findinghosting"></a>


## <a name="finding-hosting-providers-for-an-aspnet-application"></a>Recherche d’une application ASP.NET, les fournisseurs d’hébergement

- [Hébergement ASP.NET](https://asp.net/hosting)


<a id="fromvs"></a>


## <a name="deploying-a-web-application-from-visual-studio"></a>Déploiement d’une application web à partir de Visual Studio

- [Comment déployer un Site Web Microsoft Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Décrit les options et fournit des liens vers des ressources pour le déploiement de projets web sur les Sites Web Windows Azure. Inclut une section sur le déploiement à partir de Visual Studio.
- [Déploiement de Web ASP.NET à l’aide de Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12-série de didacticiels, montre comment déployer des applications web avec les bases de données SQL Server. Pour la base de données déploiement utilise le fournisseur de dbDacFx et Migrations Entity Framework Code First. Inclut également des informations sur [transformations du fichier Web.config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md), [déploiement de fichiers individuels](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md#specificfiles), [déploiement en ligne de commande](../web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment.md), et [comment personnaliser le web de Visual Studio Pipe-Line de publication en modifiant les fichiers de .pubxml](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md). S’applique à tous les projets web ASP.NET, y compris les Web Forms, MVC et API Web.)
- [Comment : déployer un projet Web à l’aide de clic publication dans Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (informations de référence pour l’Assistant Publication de Visual Studio Web.)
- [Déploiement d’une Application de Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). Il s’agit d’une version antérieure de **le déploiement Web ASP.NET à l’aide de Visual Studio** figurant en haut de cette section. Principalement utile maintenant pour plus d’informations sur la façon de déployer des bases de données SQL Server Compact et comment migrer à partir de SQL Server Compact vers une version complète de SQL Server.
- [Tables de stockage à l’aide de .NET à plusieurs niveaux Application, les files d’attente et les objets BLOB](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (site Microsoft Azure). 5-série de didacticiels, montre comment créer un projet MVC et déployez-le sur un Service Cloud Windows Azure.


<a id="package"></a>
## <a name="deploying-a-web-application-by-creating-and-installing-a-web-deployment-package"></a>Déploiement d’une application web en créant et en installant un package de déploiement web

- [Comment : créer un Package de déploiement Web dans Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx) (MSDN).
- [Comment : installer un Package de déploiement à l’aide du fichier deploy.cmd créé par Visual Studio](https://msdn.microsoft.com/library/ff356104.aspx) (MSDN).
- [À l’aide d’un package Web Deploy pour déployer dans IIS sur la boîte de développement et à un hôte tiers](http://sedodream.com/2011/11/08/UsingAWebDeployPackageToDeployToIISOnTheDevBoxAndToAThirdPartyHost.aspx) (blog de Sayed Hashimi). Comment utiliser le Gestionnaire des services Internet pour installer un package de déploiement dans IIS sur l’ordinateur local et à un hébergement d’entreprise qui prend en charge le Gestionnaire des services Internet pour l’Administration à distance.
- [Création d’un Web déploiement de Package à partir de Visual Studio 2010](https://www.iis.net/learn/publish/using-web-deploy/building-a-web-deploy-package-from-visual-studio-2010) (IIS.NET web site). Inclut des instructions pour la création du package de ligne de commande et l’installation.
- [Une fois publier n’importe quel endroit du package](http://sedodream.com/2012/03/14/PackageWebUpdatedAndVideoBelow.aspx) (blog de Sayed Hashimi). Introduit un package NuGet qui automatise le processus de transformation du fichier Web.config pour plusieurs environnements de destination, afin que vous pouvez déployer un package sur plusieurs serveurs. Voir aussi le [PackageWeb vidéo](https://www.youtube.com/watch?v=-LvUJFI8CzM) par Sayed Hashimi.

Consultez également la section suivante.


<a id="ci"></a>


## <a name="deploying-a-web-application-using-a-continuous-integration-ci-process"></a>Déploiement d’une application web à l’aide d’un processus d’intégration continue (CI)

- [Intégration continue et livraison continues (génération d’applications Cloud réalistes avec Windows Azure).](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) Chapitre de livre électronique qui présente l’intégration continue et livraison continue.
- [Comment déployer un Site Web Microsoft Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Décrit les options et des liens vers des ressources pour le déploiement de projets web sur les Sites Web Windows Azure. Inclut une section sur l’automatisation de déploiement à partir du contrôle de code source.
- [Déploiement d’Applications Web dans les scénarios d’entreprise](../web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). 40-série de didacticiels, montre comment automatiser le déploiement dans un processus d’intégration continue à l’aide de Visual Studio 2010 et Team Foundation Server 2010.
- [À l’intérieur de Microsoft Build Engine : utilisation de MSBuild et Team Foundation Build, par Sayed Hashimi et William Bartholomew](http://msbuildbook.com). Il s’agit d’un livre, pas une ressource web, mais il est un guide essentiel pour apprendre à configurer MSBuild pour les scénarios d’intégration continue.
- [Pack d’extensions de MSBuild](https://github.com/mikefourie/MSBuildExtensionPack). Inclut des tâches de déploiement.
- [Team Foundation Build personnalisation Guide](https://aka.ms/vsarsolutions). Documentation par groupe ALM Rangers sur la configuration de Team Foundation Server couvre le déploiement web et inclut des didacticiels et vidéos.
- [Transforme les SlowCheetah XML à partir d’un serveur CI](http://sedodream.com/2011/12/12/SlowCheetahXMLTransformsFromACIServer.aspx) (blog de Sayed Hashimi). Explique comment utiliser SlowCheetah, complément A Visual Studio pour la transformation de app.config et autres fichiers XML.

Voir aussi [assurant une application est hors connexion pendant le déploiement](aspnet-web-deployment-content-map.md#appoffline) plus loin dans cette page.


<a id="transforms"></a>


## <a name="using-webconfig-transformations-to-change-settings-in-the-destination-webconfig-file-or-appconfig-file-during-deployment"></a>Pour modifier les paramètres dans le fichier Web.config de destination ou le fichier app.config au cours du déploiement à l’aide de transformations Web.config

- [Transformations du fichier Web.config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md).
- [Syntaxe de Transformation de Web.config pour le déploiement de projet Web à l’aide de Visual Studio](https://msdn.microsoft.com/library/dd465326.aspx) (MSDN).
- [Web Tools 2012.2 - transformation web.config](https://www.youtube.com/watch?v=HdPK8mxpKEI) (vidéo YouTube par Sayed Hashimi). Montre comment configurer et afficher un aperçu des transformations de Web.config.
- [Désactivation de transformation Web.config](https://msdn.microsoft.com/library/ee942158.aspx#disable_web_config_transformation) (MSDN).
- [Quand dois-je utiliser Web Deploy de paramètres au lieu de transformations Web.config ?](https://msdn.microsoft.com/library/ee942158.aspx#web_deploy_parameters) (MSDN).
- [XDT (XML Document Transformation) publié sur codeplex.com](https://blogs.msdn.com/b/webdev/archive/2013/04/23/xdt-xml-document-transform-released-on-codeplex-com.aspx) (blog .NET Web Development and Tools). Annonce la disponibilité du code source pour le moteur de transformation du fichier Web.config et répertorie certains outils qui l’utilisent.
- [Sites Web Windows Azure : Comment Application des chaînes et connexion](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (blog Microsoft Azure). Une alternative au fichier Web.config transforme si votre environnement de destination est de Sites Web Windows Azure et que vous souhaitez transformer `appSettings` ou `connectionStrings`.


<a id="webdeployparms"></a>


## <a name="using-web-deploy-parameters-to-change-settings-in-the-destination-web-application-during-deployment"></a>À l’aide des paramètres Web Deploy pour modifier les paramètres dans l’application web de destination pendant le déploiement

- [Comment : utiliser Web déployer des paramètres dans un Package de déploiement Web](https://msdn.microsoft.com/library/ff398068.aspx) (MSDN).
- [MSDeploy : Comment mettre à jour les paramètres de l’application sur la publication basée sur le profil de publication](http://sedodream.com/2013/03/02/MSDeployHowToUpdateAppSettingsOnPublishBasedOnThePublishProfile.aspx) (blog de Sayed Hashimi). Montre comment intégrer paramètres de déploiement Web dans Visual Studio des profils de publication.
- [Déployer le paramétrage de Web](https://www.iis.net/learn/publish/using-web-deploy/web-deploy-parameterization) (IIS.NET web site).
- [Déployer le paramétrage dans l’Action de Web](http://vishaljoshi.blogspot.com/2010/07/web-deploy-parameterization-in-action.html) (blog de Vishal).
- [Déployer le paramétrage de Web Visual Studio. Transformation Web.config](http://vishaljoshi.blogspot.com/2010/06/parameterization-vs-webconfig.html) (blog de Vishal).
- [Sites Web Windows Azure : Comment Application des chaînes et connexion](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (blog Microsoft Azure). Une alternative à Web si votre environnement de destination est de Sites Web Windows Azure et que vous souhaitez paramétrer des paramètres de déploiement `appSettings` ou `connectionStrings`.


<a id="appoffline"></a>


## <a name="making-sure-an-application-is-off-line-during-deployment"></a>Assurant une application est hors connexion pendant le déploiement

- [Déploiement de Web ASP.NET à l’aide de Visual Studio : déploiement d’une mise à jour du Code](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Consultez la section **mettre l’application hors ligne pendant le déploiement.**
- [Mettre une Application hors connexion avant la publication](https://www.iis.net/learn/publish/deploying-application-packages/taking-an-application-offline-before-publishing) (IIS.net site). Décrit une fonctionnalité intégrée à Web Deploy 3.0 qui automatise la gestion d’une application\_offline.htm fichier. Cette fonctionnalité ne fonctionne pas avec une application personnalisée\_offline.htm fichiers.
- [Comment faire passer votre application web en mode hors connexion lors de la publication](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx) (blog de Sayed Hashimi). Comment automatiser le processus d’utilisation d’une application personnalisée\_offline.htm fichier.
- [Web publishing mises à jour pour l’application en mode hors connexion et usechecksum](https://blogs.msdn.com/b/webdev/archive/2013/10/30/web-publishing-updates-for-app-offline-and-usechecksum.aspx) (blog de développement Web de Microsoft). Une autre option pour automatiser l’utilisation de l’application\_offline.htm fichier.
- [Web RTW de 3.5 déployer](https://blogs.iis.net/msdeploy/archive/2013/07/09/webdeploy-3-5-rtw.aspx) (IIS.net site). Nouvelle fonctionnalité dans 3.5 de déploiement Web pour une application personnalisée\_offline.htm fichiers.


<a id="databasewithweb"></a>


## <a name="deploying-a-database-or-changes-to-a-database-as-part-of-web-application-deployment"></a>Déploiement d’une base de données ou des modifications à une base de données dans le cadre du déploiement d’applications web

- [Configuration du déploiement de base de données dans Visual Studio](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment) (MSDN). Vue d’ensemble des options de déploiement d’une base de données avec un projet web.
- [Déploiement de Web ASP.NET à l’aide de Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12-série de didacticiels, montre le déploiement de base de données à l’aide de dbDacFx fournisseur et Migrations Entity Framework Code First.
- [Comment : déployer un site Web publier le projet à l’aide d’un seul clic dans Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (MSDN).
- [Déployer une application ASP.NET MVC 5 sécurisée avec appartenance, OAuth et base de données SQL sur un Site Web Microsoft Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Un didacticiel long qui génère et déploie une application qui utilise un seul serveur SQL de base de données à la fois pour les données d’appartenance et d’application.
- [Déploiement d’une Application de Web ASP.NET avec SQL Server Compact à l’aide de Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). 12-série de didacticiels, montre comment déployer des bases de données SQL Server Compact et comment migrer à partir de SQL Server Compact vers une version complète de SQL Server.

Voir aussi déployer une application web en créant et d’installation d’un package de déploiement web et de déploiement d’une application web à l’aide d’un processus d’intégration continue (CI) plus haut dans cette page.


<a id="databaseseparate"></a>


## <a name="deploying-a-database-separately-from-web-application-deployment"></a>Déploiement d’une base de données séparément de déploiement d’applications web

- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686(v=vs.103).aspx) (MSDN).
- [Y compris les données dans un projet de base de données SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx) (blog de l’équipe SQL Server Data Tools). Comment déployer le schéma et les données lors du déploiement d’une base de données.
- [Comment déployer une base de données sur Windows Azure](https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate) (site Microsoft Azure)
- [Bases de données de migration vers Windows Azure SQL Database (anciennement SQL Azure)](https://msdn.microsoft.com/library/windowsazure/ee730904.aspx) (MSDN).
- [Migration d’une base de données vers SQL Azure à l’aide de SSDT](https://blogs.msdn.com/b/ssdt/archive/2012/04/19/migrating-a-database-to-sql-azure-using-ssdt.aspx) (blog de l’équipe SQL Server Data Tools).
- [Migration d’Applications centrées sur les données vers Windows Azure](https://msdn.microsoft.com/library/jj156154.aspx) (MSDN).
- [Migration des bases de données SQL Server vers Microsoft Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).


<a id="aspnetmembership"></a>


## <a name="deploying-a-web-application-that-uses-aspnet-application-services-such-as-membership-and-profiling"></a>Déploiement d’une application web qui utilise ASP.NET application des services tels que les appartenances et de profilage

- [Déployer une application ASP.NET MVC 5 sécurisée avec appartenance, OAuth et base de données SQL sur un Site Web Microsoft Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Un didacticiel long qui génère et déploie une application qui utilise un seul serveur SQL de base de données à la fois pour les données d’appartenance et d’application.
- [ASP.NET Identity](https://asp.net/identity/). Ressources pour ASP.NET Identity.
- [Déploiement de Web ASP.NET à l’aide de Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12-série de didacticiels, montre comment déployer une base de données d’appartenance ASP.NET.
- [Configuration d’un site Web qui utilise les Services d’Application](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs.md). Pour le site web de projets, mais s’applique également aux projets d’application web.
- [Utilisateurs et rôles sur le site Web de Production](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs.md). Pour le site web de projets, mais s’applique également aux projets d’application web.


<a id="precompiling"></a>


## <a name="precompiling-for-deployment"></a>Précompilation pour le déploiement

- [Présentation de la précompilation projet Web ASP.NET Application](https://msdn.microsoft.com/library/aa983464.aspx) (MSDN).
- [Package/publication de l’onglet Web, propriétés de projet](https://msdn.microsoft.com/library/dd410108.aspx) (MSDN).
- [Advanced précompiler boîte de dialogue Paramètres](https://msdn.microsoft.com/library/hh475319.aspx) (MSDN).


<a id="intranet"></a>


## <a name="deploying-an-intranet-web-application"></a>Déploiement d’une application de web intranet

- [Utiliser l’Option de l’authentification d’organisation en local (AD FS) avec ASP.NET dans Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/) (Blog de Vittorio Bertocci.).
- [Comment créer un Site Intranet à l’aide d’ASP.NET MVC](https://msdn.microsoft.com/library/gg703322(VS.98).aspx) (MSDN). Ancienne procédure pas à pas écrit pour Visual Studio 2010, ne reflète pas les modifications majeures dans les modèles de projet intranet introduites dans Visual Studio 2013.


<a id="automating"></a>


## <a name="automating-common-deployment-tasks-that-are-not-automated-out-of-the-box"></a>Automatisation des tâches de déploiement courantes qui ne sont pas automatisés prêts à l’emploi

- [Déploiement de Web ASP.NET à l’aide de Visual Studio : déploiement de fichiers supplémentaires](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md).
- [Définition des autorisations de dossier sur le Web Publish](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) (blog de Sayed Hashimi).
- [Comment étendre le fichier de cibles pour inclure les paramètres du Registre pour un package de projet web](https://blogs.msdn.com/webdevtools/archive/2010/02/09/how-to-extend-target-file-to-include-registry-settings-for-web-project-package.aspx) (blog sur les outils de développement Web).
- [Extension de transformation XML (Web.config)](http://sedodream.com/2010/09/09/ExtendingXMLWebconfigConfigTransformation.aspx) (blog de Sayed Hashimi). Montre comment créer des transformations XDT personnalisées.
- [Web prennent de fournisseur personnalisée de l’outil de déploiement (MSDeploy) 1](http://sedodream.com/2010/03/11/WebDeploymentToolMSDeployCustomProviderTake1.aspx) (blog de Sayed Hashimi). Montre comment créer un fournisseur personnalisé de Web Deploy.
- [Comment empaqueter et déployer des composants COM](https://blogs.msdn.com/webdevtools/archive/2010/03/03/how-to-package-and-deploy-com-component.aspx) (blog sur les outils de développement Web).
- [Comment les assemblys de package .NET](https://blogs.msdn.com/webdevtools/archive/2010/02/19/how-to-package-com-component.aspx) (blog sur les outils de développement Web). Comment déployer des assemblys dans le GAC.
- [Script tout - initialiser votre machine virtuelle Windows Azure pour votre serveur Web avec IIS, de Web Deploy et d’autres choses](http://www.tugberkugurlu.com/archive/script-out-everything-initialize-your-windows-azure-vm-for-your-web-server-with-iis-web-deploy-and-other-stuff) (blog de Tugberk Ugurlu).


<a id="configuringservers"></a>


## <a name="configuring-web-servers-so-that-developers-can-deploy-web-applications-to-them-using-web-deploy"></a>Configuration de serveurs web afin que les développeurs peuvent déployer des applications web à l’aide de Web Deploy

- [Installation et configuration de Web Deploy pour administrateur et non-administrateur déploiements](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy) (IIS.net site).


<a id="hostingprovider"></a>


## <a name="configuring-servers-for-a-hosting-provider"></a>Configuration de serveurs pour un fournisseur d’hébergement

- [Guide de déploiement d’hébergement Microsoft ASP.NET 4](https://go.microsoft.com/fwlink/?LinkId=191365) (centre de téléchargement Microsoft).
- [Générer un fichier XML de profil](https://www.iis.net/learn/web-hosting/joining-the-web-hosting-gallery/generate-a-profile-xml-file) (IIS.net site).


<a id="troubleshooting"></a>


## <a name="troubleshooting-deployment-problems"></a>Résolution des problèmes de déploiement

- [Dépannage des Sites Web de Windows Azure dans Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) (site Microsoft Azure).
- [Déploiement de Web ASP.NET à l’aide de Visual Studio : résolution des problèmes](../web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting.md).
- [Dépannage des problèmes courants avec Web déploiement](https://www.iis.net/learn/publish/troubleshooting-web-deploy/troubleshooting-common-problems-with-web-deploy).
- [Codes d’erreur de déploiement de Web](https://www.iis.net/learn/publish/troubleshooting-web-deploy/web-deploy-error-codes) (IIS.net site).
- [Forum aux questions de déploiement pour Visual Studio et ASP.NET Web](https://msdn.microsoft.com/library/ee942158.aspx) (MSDN).
- [Principales différences entre IIS et le serveur de développement ASP.NET](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md).
- [Les différences de Configuration courantes entre le développement et de Production](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs.md).
- [Hébergement d’Applications ASP.NET dans Medium Trust](http://www.4guysfromrolla.com/articles/100307-1.aspx) (4 Guys Rolla site).


<a id="gettinghelp"></a>


## <a name="getting-help-with-a-specific-deployment-question"></a>Obtention d’aide avec une question de déploiement spécifique

- [Forum de déploiement et de Configuration ASP.NET](https://forums.asp.net/26.aspx/1?Configuration and Deployment).
- [StackOverflow.com](http://www.StackOverflow.com).


<a id="additional"></a>


## <a name="additional-resources"></a>Ressources supplémentaires

Cette section fournit des liens vers des ressources supplémentaires qui sont utiles pour en savoir plus sur l’utilisation des outils de déploiement de Visual Studio et IIS.

Les blogs suivants contiennent fréquemment des informations sur le déploiement web Visual Studio :

- [Outils de développement Web au blog Microsoft](https://blogs.msdn.com/b/webdevtools/).
- [Blog de Sayed Hashimi](http://www.sedodream.com/).

Les ressources suivantes fournissent une documentation sur le Web Deploy, l’infrastructure IIS que Visual Studio utilise pour effectuer des tâches de déploiement de projet application web. Vous pouvez poser des questions sur Web Deploy dans le [forum de l’outil de déploiement Web](https://go.microsoft.com/fwlink/?LinkId=149411) sur le site web IIS.net.

- [Introduction à Web déployer](https://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy).
- [Installation et la configuration Web déploiement](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy).
- [Le programme d’installation de déployer des Scripts PowerShell pour l’automatisation Web](https://www.iis.net/learn/publish/using-web-deploy/powershell-scripts-for-automating-web-deploy-setup).
- [Outil de déploiement Web](https://go.microsoft.com/fwlink/?LinkId=151481). Table de niveau supérieur du nœud de contenu pour obtenir une documentation Web Deploy sur le site TechNet. Inclut des informations de référence utiles mais la plupart de la TechNet pages n’ont pas été mis à jour depuis des années.
- [Microsoft.Web.Deployment Namespace](https://go.microsoft.com/fwlink/?LinkId=148630). Documentation de l’API, n’a pas été mis à jour depuis la version 1.0.
- [Blog de l’équipe de déploiement Web Microsoft](https://blogs.iis.net/msdeploy/default.aspx).
- [Publication : onglet dans le site web de IIS.net](https://www.iis.net/learn/publish).
