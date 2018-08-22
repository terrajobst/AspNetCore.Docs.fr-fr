---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: ASP.NET et Web Tools 2013.2 pour Visual Studio 2013 Release Notes de publication | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 03/06/2014
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 2a22c5b686cb8e02054f421f78a8fc910af7ce28
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834640"
---
<a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>ASP.NET et Web Tools 2013.2 pour Visual Studio 2013 Release Notes de publication
====================
par [Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>Notes d’installation

ASP.NET et Web Tools pour Visual Studio 2013.2 sont regroupés dans le programme d’installation principal et peut être téléchargé dans le cadre de [Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521).

## <a name="documentation"></a>Documentation

Didacticiels et autres informations sur ASP.NET et Web Tools pour Visual Studio 2013.2 sont disponibles à partir de la [site web ASP.NET](https://www.asp.net/).

## <a name="software-requirements"></a>Configuration logicielle

ASP.NET et Web Tools pour Visual Studio 2013.2 requiert Visual Studio 2013.

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>Nouvelles fonctionnalités dans ASP.NET et Web Tools pour Visual Studio 2013.2

Les sections suivantes décrivent les fonctionnalités qui ont été introduites dans la version.

- [Modèles d’un projet ASP.NET](#oneaspnet)
- [Prend en charge SSL lors du lancement des Applications Web sur IIS Express](#ssl)
- [Améliorations de l’éditeur Web de Visual Studio](#vswebeditor)
- [Lien du navigateur](#browserlink)
- [Prise en charge pour Azure App Service Web Apps dans Visual Studio](#waws)
- [Créer des ressources Azure à distance lors de la création d’un nouveau projet Web](#AzureResources)
- [Améliorations de la publication sur le Web](#webpublish)
- [Génération de modèles automatique ASP.NET](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [ASP.NET Web Forms](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [2.1.2 l’API Web ASP.NET](#webapi)
- [3.1.2 les Pages Web ASP.NET](#webpages)
- [Entity Framework 6.1](#ef)
- [ASP.NET Identity 2.0.0](#identity)
- [Composants Microsoft OWIN](#owin)
- [ASP.NET SignalR 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>Modèles d’un projet ASP.NET

- Mises à jour de modèles de projet ASP.NET pour prendre en charge la confirmation de compte et mot de passe réinitialisé.
- Modèle d’API Web ASP.NET de mises à jour pour prendre en charge l’authentification à l’aide sur site comptes professionnels.
- Le modèle ASP.NET SPA contient désormais l’authentification basée sur les affichages MVC et le serveur. Le modèle a un contrôleur d’API Web qui est accessible uniquement par les utilisateurs authentifiés.

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>Prend en charge SSL lors du lancement des Applications Web sur IIS Express

Pour éliminer l’avertissement de sécurité lors de la navigation et le débogage de HTTPS sur localhost, nous avons ajouté une boîte de dialogue permettant de Internet Explorer et Chrome pour approuver l’auto-signé IIS express certificat SSL.

Par exemple, une propriété de projet web peut être définie pour utiliser SSL. Appuyez sur F4 pour afficher la boîte de dialogue de propriétés. Modification **SSL activé** sur true. Copiez l’URL SSL.

![La propriété Enabled de SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

Ensemble de l’onglet web page de propriété de projet web à utiliser le protocole HTTPS en fonction des URL (URL SSL sera `https://localhost:44300/` , sauf si vous avez déjà créé des Sites Web SSL.)

![Définir l’URL du projet (HTTPS)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Appuyez sur CTRL+F5 pour exécuter l'application. Suivez les instructions pour approuver le certificat auto-signé IIS Express a généré.

![Avertissement SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

Lire le **avertissement de sécurité** boîte de dialogue, puis cliquez sur **Oui** si vous souhaitez installer le certificat représentant localhost.

![Avertissement de sécurité](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

Le site s’affichera dans Internet Explorer ou Chrome sans l’avertissement de certificat dans le navigateur.

![Page HTTPS sans avertissement](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

Firefox utilise son propre magasin de certificats, par conséquent, il contient un avertissement.

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Améliorations de l’éditeur Web de Visual Studio

- **Nouvel élément de projet JSON et éditeur**: nous avons ajouté un élément de projet JSON et l’éditeur à Visual Studio. Fonctionnalités de l’éditeur JSON actuelles incluent la colorisation, validation de la syntaxe, fin d’accolade, mise en relief, définition de l’option outils et bien plus encore.

    ![Éditeur JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    IntelliSense prend désormais en charge [schéma JSON](http://json-schema.org/) v3 et v4. Il existe une zone de liste déroulante de schéma pour choisir des schémas existants, modifier le chemin d’accès local de schéma, ou simplement glisser-déplacer un fichier JSON de projet pour obtenir le chemin d’accès relatif.

    ![Intellisense JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![Éditeur de schéma JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **Nouvel éditeur Sass (SCSS)**: nous avons ajouté inférieure dans VS2013 RTM, et nous disposons désormais d’un élément de projet Sass et l’éditeur. Éditeur sass fonctionnalités sont comparables à l’éditeur LESS et incluent la colorisation, variable et Mixins IntelliSense, enlevez des marques de commentaire, info Express, mise en forme, validation de la syntaxe, le mode plan, atteindre la définition, le sélecteur de couleurs, les outils de définition etc. de l’option.

    ![Ajouter un nouvel élément : Une feuille de Style SCSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![Éditeur de feuilles de style](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **Nouveau sélecteur d’URL en HTML, Razor, CSS, moins et les documents Sass :** VS 2013 livrés avec aucun sélecteur d’URL en dehors des pages Web Forms. Le nouveau sélecteur d’URL pour HTML, Razor, CSS, LESS et Sass éditeurs est un sélecteur de frappe gratuit à la boîte de dialogue, fluent qui comprend '..' et fichier de filtres répertorie correctement des liens et des balises img.

    ![Sélecteur d’URL pour la balise d’image](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![Sélecteur d’URL pour les vues](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![Sélecteur d’URL pour le CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **Mises à jour de l’éditeur LESS en ajoutant davantage de fonctionnalités**
- **Mise à niveau de Knockout Intellisense**: nous avons ajouté une syntaxe de KnockOut non standard pour Visual Studio intelliSense, « ko-vs-éditeur viewModel : « syntaxe. Il peut être utilisée pour lier à plusieurs modèles de vue sur une page à l’aide de commentaires sous la forme :

    ![Knockout Intellisense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    Nous avons également ajouté la prise en charge pour imbriquée ViewModel IntelliSense, afin de vous pourrez examiner les objets profondément imbriquées sur le ViewModel.

    `<div data-bind="text: foo.bar.baz.etc" />`

    Le IntelilSense affichée est la fonctionnalité IntelliSense complète de l’objet JavaScript.

    ![IntelliSense affichant complète JavaScript de l’objet](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **Nouveau sélecteur d’URL en HTML, Razor, CSS, moins et les documents Sass**: VS 2013 livrés avec aucun sélecteur d’URL en dehors des pages Web Forms. Le nouveau sélecteur d’URL pour HTML, Razor, CSS, LESS et Sass éditeurs est un sélecteur de frappe gratuit à la boîte de dialogue, fluent qui comprend '..' et fichier de filtres répertorie correctement des liens et des balises img.

    ![Sélecteur d’URL pour la balise d’image](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![Sélecteur d’URL pour les vues](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![Sélecteur d’URL pour le CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>Lien du navigateur

- Maintenant, le lien du navigateur prend en charge les connexions HTTPS et listera que dans le tableau de bord avec d’autres connexions tant que le certificat est approuvé par le navigateur.
- Mappage de la source HTML statique
- SPA prend en charge pour les données de mappage
- Données de mappage de mise à jour automatique

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Prise en charge pour Azure App Service Web Apps dans Visual Studio

- **Prise en charge Azure se connecter.**
- **Débogage à distance et affichage à distance pour les applications web**: nous prennent désormais en charge [le débogage à distance pour les applications web dans Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) et afficher à distance des fichiers de contenu de l’application web dans l’Explorateur de serveurs.

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>Créer des ressources Azure à distance lors de la création d’un nouveau projet Web

Nous avons ajouté un Azure [« Créer des ressources distantes »](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) case à cocher sur la nouvelle boîte de dialogue application web. En la sélectionnant, vous serez en mesure d’intégrer l’expérience de création d’une application web, configurer le site de publication Azure pour le test et la création du profil de publication en quelques étapes simples.

![Nouveau projet avec des ressources Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Publication sur Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Améliorations de la publication sur le Web

- Améliorer l’expérience utilisateur pour la publication.

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>Génération de modèles automatique ASP.NET

- **Prise en charge de l’enum :** si votre modèle est à l’aide des énumérations, le Générateur de modèles automatique MVC génère la liste déroulante pour Enum. Cet exemple utilise les programmes d’assistance Enum dans MVC.
- **Prise en charge des données d’amorçage**: mise à jour les modèles EditorFor dans la structure MVC afin qu’ils utilisent les classes de données d’amorçage.
- **Prise en charge du package**: MVC et les générateurs de structure API Web ajoute les 5.1 packages pour MVC et API Web

Les captures d’écran suivantes illustrent les modèles de génération de modèles automatique.

- Code de modèle :

     ![Code de modèle](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- Compiler le code de modèle, avec le bouton droit, puis sélectionnez **ajouter**, **nouvel élément structuré**.

     ![Ajouter un nouvel élément généré automatiquement](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- Choisissez **contrôleur MVC5 avec vues, utilisant Entity Framework**:

     ![Ajouter le nouveau contrôleur MVC5 avec des vues](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **Ajouter un contrôleur** à l’aide du modèle :

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Vérifiez le code généré, par exemple Views/WeekdayModels/Edit.cshtml contient `@Html.EnumDropDownListFor`: ![vue contenant EnumDropDownListFor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- Exécutez la page pour voir la zone de liste déroulante enum généré, notez que si une valeur peut être null, une chaîne vide peut être choisie pour la zone de liste déroulante. Par exemple, le **créer** page affiche les éléments suivants :

    ![Zone de liste déroulante permettant de chaîne vide](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

NuGet 2.8.1 que RTM sera publié en avril 2014. Voici les points saillants dans les notes de publication, mais Veuillez vérifier le [notes de version complet](http://docs.nuget.org/docs/release-notes/nuget-2.8) pour plus d’informations sur ces modifications.

- **Cibler les Applications de Windows Phone 8.1**: NuGet 2.8.1 prend désormais en charge le ciblage Windows Phone 8.1 Applications à l’aide des monikers du framework cible 'WindowsPhoneApp', 'WPA', 'WindowsPhoneApp81' et 'WPA81'.
- **Résolution des correctifs pour les dépendances**: lors de la résolution des dépendances de package, NuGet a implémenté par le passé d’une stratégie de sélection de la version majeure et mineure package le plus bas qui satisfait aux dépendances sur le package. Toutefois, contrairement à la version majeure et mineure, la version du correctif a été résolue toujours vers la version la plus élevée. Bien que le comportement a été bien intentionné, il créé un manque de déterminisme pour installer des packages avec des dépendances.
- **Commutateur DependencyVersion**: bien que NuGet 2.8 modifie le *par défaut* comportement pour la résolution des dépendances, il ajoute également un contrôle plus précis sur le processus de résolution de dépendance via le commutateur - DependencyVersion dans le console du Gestionnaire de package. Le commutateur permet la résolution des dépendances à la version la plus basse possible (comportement par défaut), la version la plus élevée possible, ou le plus élevé version mineure ou patch. Ce commutateur fonctionne uniquement pour le package d’installation dans la commande powershell.
- **Attribut de DependencyVersion**: en plus du commutateur DependencyVersion - détaillé plus haut, NuGet a également autorisé pour la capacité à définir un nouvel attribut dans le fichier nuget.config à définir ce qui est la valeur par défaut, si la DependencyVersion - basculer n’est pas spécifié dans un appel de package d’installation. Cette valeur sera également être respectée par la boîte de dialogue Gestionnaire de Package NuGet pour les opérations de package d’installation. Pour définir cette valeur, ajoutez l’attribut ci-dessous à votre fichier nuget.config :

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **Afficher un aperçu des opérations de NuGet avec - whatif**: les packages NuGet certains peuvent avoir des graphiques de dépendance approfondie, et par conséquent, il peut s’avérer utile lors d’une installation, la désinstallation ou la mise à jour pour voir tout d’abord ce qui se produira. NuGet 2.8 ajoute la commande PowerShell standard-que se passe-t-il si basculer vers les commandes de package d’installation, désinstaller-package et package de mise à jour pour permettre la visualisation de la fermeture complète des packages à laquelle la commande sera appliquée.
- **Rétrograder le Package**: il n’est pas rare pour installer une version préliminaire d’un package afin d’étudier les nouvelles fonctionnalités et décider ensuite de revenir à la dernière version stable. Avant NuGet 2.8, c’était un processus en plusieurs étapes de la version préliminaire package et ses dépendances, installation et la désinstallation puis la version antérieure. Avec NuGet 2.8, toutefois, le package de mise à jour maintenant annule la fermeture de l’ensemble du package (par exemple, arborescence des dépendances du package) à la version précédente.
- **Dépendances de développement**: différents types de fonctionnalités peuvent être remis sous forme de packages NuGet - y compris les outils qui sont utilisés pour optimiser le processus de développement. Ces composants, pendant qu’ils peuvent vous aider à développer un nouveau package, ne doivent pas être considérées publié une dépendance du nouveau package lorsqu’il est plus loin. NuGet 2.8 permet à un package pour s’identifier dans le fichier .nuspec comme un developmentDependency. Lors de l’installation, ces métadonnées également figurera dans le fichier packages.config du projet dans lequel le package a été installé. Lorsque ce fichier packages.config est analysé ultérieurement au cours du pack de nuget.exe pour les dépendances NuGet, il exclut ces dépendances marquées en tant que dépendances de développement.
- **Fichiers packages.config individuels pour différentes plateformes**: lors du développement d’applications pour plusieurs plateformes cibles, il est courant d’avoir des fichiers de projet différent pour chacun des environnements de build respectifs. Il est également courant de consommer différents packages NuGet dans différents fichiers projet, comme les packages ont différents niveaux de prise en charge pour les différentes plateformes. NuGet 2.8 fournit la prise en charge améliorée pour ce scénario en créant des fichiers packages.config différents pour les fichiers de projet spécifiques à la plateforme différent.
- **Secours dans le Cache Local**: bien que NuGet packages sont généralement consommées depuis une galerie à distance tels que le [galerie NuGet](http://www.nuget.org) à l’aide d’une connexion réseau, il existe de nombreux scénarios où le client n’est pas connecté. Sans une connexion réseau, le client NuGet n’a pas pu installer correctement les packages - même lorsque ces packages étaient déjà présents sur l’ordinateur client dans le cache NuGet local. NuGet 2.8 ajoute automatique du cache secours à la console du Gestionnaire de package.

    La fonctionnalité de cache de secours ne nécessite pas d’argument de commande spécifique. En outre, cache de secours fonctionne actuellement uniquement dans la console du Gestionnaire de package : le comportement ne fonctionne pas actuellement dans la boîte de dialogue de gestionnaire de package.
- **Correctifs de bogues**: parmi les principaux correctifs de bogues apportées était l’amélioration des performances dans le package de mise à jour-réinstaller la commande.

    Outre ces fonctionnalités et le correctif de performances mentionnés ci-dessus, cette version de NuGet inclut également plusieurs autres correctifs de bogues. Vous rencontrez des problèmes de total 181 résolus dans la version. Pour obtenir la liste complète des travaux éléments résolus dans NuGet 2.8, veuillez vue le [NuGet Issue Tracker](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all) pour cette version.

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>ASP.NET Web Forms

- Les modèles Web Forms affichent maintenant comment effectuer la Confirmation de compte et mot de passe réinitialisé pour ASP.NET Identity.
- Le contrôle de Source de données d’entité et le fournisseur de données dynamiques pour Entity Framework 6. Pour plus d’informations, consultez le blog MSDN suivant : [fournisseur de données dynamique et de contrôle EntityDataSource pour Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx).

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [Améliorations du routage d’attribut](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [Prise en charge d’amorçage pour les modèles de l’éditeur](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [Prise en charge de l’énumération dans les vues](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Prise en charge de Unobstrusive de MinLength / MaxLength attributs](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [Prise en charge le contexte 'THI' dans Ajax discrète](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- Divers [des correctifs de bogues](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>2.1.2 l’API Web ASP.NET

- [Gestion des erreurs globales](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [Améliorations de routage d’attribut](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [Améliorations des pages d’aide](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [Prise en charge IgnoreRoute](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [Formateur de type de média BSON](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [Meilleure prise en charge pour les filtres d’async](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [Analyse du client de mise en forme de la bibliothèque de la requête](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- Divers [des correctifs de bogues](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>3.1.2 les Pages Web ASP.NET

- Divers [des correctifs de bogues](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Entity Framework 6.1

Entity Framework a été mis à jour vers la version 6.1 de runtime et des outils. 6.1 Entity Framework (EF) est une mise à jour mineure pour Entity Framework 6 et inclut un certain nombre de correctifs de bogues et de nouvelles fonctionnalités. Pour plus d’informations sur EF6.1, notamment des liens vers la documentation pour les nouvelles fonctionnalités, consultez [l’historique de Version Entity Framework](https://msdn.microsoft.com/data/jj574253). Les nouvelles fonctionnalités dans cette version sont les suivantes :

- **Outils de consolidation** fournit un moyen cohérent de créer un nouveau modèle EF. Cette fonctionnalité s’étend de l’Assistant d’ADO.NET Entity Data Model pour prendre en charge la création de modèles de Code First, y compris l’ingénierie à rebours à partir d’une base de données existante. Ces fonctionnalités étaient précédemment disponibles dans la qualité bêta les EF Power Tools.
- **Gestion des échecs de validation de transaction** propose les nouvelles [System.Data.Entity.Infrastructure.CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx) qui utilise la capacité récemment introduite pour intercepter les opérations de transaction. Le **CommitFailureHandler** permet la récupération automatique à partir d’échecs de connexion lors de la validation d’une transaction.
- **IndexAttribute** autorise les index de la définir en plaçant un attribut sur une propriété (ou propriétés) dans votre modèle Code First. Code tout d’abord crée ensuite un index correspondant dans la base de données.
- **L’API de mappage publique** fournit l’accès aux informations EF a sur la façon dont les propriétés et les types sont mappés aux colonnes et tables dans la base de données. Dans les versions précédentes cette API a été interne.
- **Possibilité de configurer des intercepteurs via le fichier App/Web.config**(autorisant les intercepteurs doit être ajouté sans recompiler l’application).
- **DatabaseLogger** est un intercepteur de qui facilite la consigner toutes les opérations de base de données dans un fichier. En association avec la fonctionnalité précédente, cela vous permet de basculer facilement la journalisation des opérations de base de données pour une application déployée, sans avoir à recompiler.
- **Détection de modification de modèle de migrations** a été améliorée afin que les migrations généré automatiquement sont plus précises ; les performances du processus de détection de modification a été considérablement amélioré.
- **Améliorations des performances** y compris les opérations de base de données réduites lors de l’initialisation, les optimisations pour la comparaison d’égalité de valeur null dans les requêtes LINQ, afficher plus rapidement génération (la création de modèles) dans d’autres scénarios et plus efficace matérialisation d’entités suivies avec plusieurs associations.

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>ASP.NET Identity 2.0.0

- **Authentification à deux facteurs**: identité ASP.NET prend désormais en charge l’authentification à deux facteurs. Authentification à deux facteurs fournit une couche supplémentaire de sécurité à vos comptes d’utilisateur dans le cas où votre mot de passe est compromis. Il existe également une protection pour les attaques par force brute contre les codes d’authentification à deux facteurs.
- **Verrouillage de compte :** permet de verrouiller l’utilisateur si l’utilisateur entre son mot de passe ou des codes de deux facteurs incorrectement. Le nombre de tentatives non valides et l’intervalle de temps pour les utilisateurs sont verrouillés peut être configuré. Un développeur peut éventuellement désactiver le verrouillage de compte pour certains comptes d’utilisateurs qu’ils doivent.
- **Confirmation du compte :** le système d’identité ASP.NET prend désormais en charge la Confirmation du compte. Il s’agit d’un scénario assez courant dans la plupart des sites Web aujourd'hui où, quand vous inscrivez pour un nouveau compte sur le site Web, vous devez confirmer votre adresse de messagerie avant de faire quoi que ce soit dans le site Web. E-mail de Confirmation est utile, car il empêche la création des comptes factices. Cela est particulièrement utile si vous utilisez le courrier électronique en tant que méthode de communication avec les utilisateurs de votre site Web tels que les sites de Forum, bancaire, de commerce électronique ou des sites web sociaux.
- **Réinitialisation du mot de passe :** réinitialisation de mot de passe est une fonctionnalité où l’utilisateur peut réinitialiser leurs mots de passe s’ils ont oublié leur mot de passe.
- **Tampon de sécurité (déconnexion partout) :** prend en charge une méthode pour régénérer le jeton de sécurité pour l’utilisateur dans le cas lorsque l’utilisateur modifie son mot de passe ou toute autre sécurité liés à des informations telles que la suppression d’une connexion associée (tels que Facebook, Google, Compte Microsoft et ainsi de suite). Cela est nécessaire pour vous assurer que tous les jetons générés avec l’ancien mot de passe sont invalidés. Dans l’exemple de projet, si vous modifiez le mot de passe ensuite un nouveau jeton est généré pour l’utilisateur et tous les jetons précédentes sont invalidés. Cette fonctionnalité fournit une couche supplémentaire de sécurité à votre application depuis lorsque vous modifiez votre mot de passe, vous allez être déconnecté depuis n’importe où (tous les autres navigateurs) où vous êtes connecté à cette application.
- **Vérifiez le type de clé primaire à être extensible pour les utilisateurs et rôles**: dans ASP.NET Identity 1.0, le type de clé primaire pour les rôles et les utilisateurs de la table était chaînes. Cela signifie que lorsque le système d’identité ASP.NET a été rendue persistante dans SQL Server à l’aide d’Entity Framework, nous utilisions nvarchar. Il était nombreuses discussions autour de cette implémentation par défaut sur Stack Overflow et basés sur les commentaires entrant. Nous avons fourni un raccordement d’extensibilité où vous pouvez spécifier ce qui doit être la clé primaire de votre table utilisateurs et rôles. Ce point d’interception d’extensibilité est particulièrement utile que si vous migrez votre application et de l’application a été UserIds stockage sont des entiers ou des GUID.
- **Prend en charge IQueryable sur les utilisateurs et rôles**: prise en charge ajoutée pour IQueryable sur UsersStore et RolesStore, vous pouvez facilement obtenir la liste des utilisateurs et des rôles.
- **Opération de suppression de prise en charge par le biais du UserManager**
- **L’indexation sur le nom d’utilisateur**: implémentation de l’infrastructure d’entité d’identité dans ASP.NET, nous avons ajouté un index unique sur le nom d’utilisateur à l’aide de la nouvelle IndexAttribute dans EF 6.1.0. Cela permet de s’assurer que les noms d’utilisateur sont toujours uniques et il n’a aucune condition de concurrence dans laquelle vous pouvez vous retrouver avec des noms d’utilisateur en double.
- **Validateur de mot de passe améliorée :** le validateur de mot de passe qui a été expédié dans ASP.NET Identity 1.0 a été un validateur de mot de passe assez basique qui a été valider uniquement la longueur minimale. Il est un nouveau mot de passe du programme de validation qui vous donne davantage de contrôle sur la complexité du mot de passe. Veuillez noter que même si vous activez tous les paramètres de ce mot de passe, nous vous encourageons à activer l’authentification à deux facteurs pour les comptes d’utilisateur.
- **Intergiciel (middleware) IdentityFactory / CreatePerOwinContext**:

    - **Le Gestionnaire des utilisateurs**: vous pouvez utiliser l’implémentation de la fabrique pour obtenir une instance de UserManager à partir du contexte OWIN. Ce modèle est similaire à ce que nous utilisons pour l’obtention de AuthenticationManager à partir du contexte d’OWIN pour la connexion et déconnexion. Il s’agit d’une méthode recommandée pour obtenir une instance de UserManager par demande pour l’application.
    - **DbContextFactory**: ASP.NET Identity utilise Entity Framework pour rendre persistant le système d’identité dans SQL Server. Pour ce faire le système d’identité a une référence à la ApplicationDbContext. Le DbContextFactory Middleware retourne une instance de la ApplicationDbContext par demande que vous pouvez utiliser dans votre application.
- **ASP.NET Identity exemples NuGet package**: l’exemples package NuGet peut rendre plus facile à installer et exécuter des exemples pour ASP.NET Identity et suivez les meilleures pratiques. Il s’agit d’un exemple d’application d’ASP.NET MVC. Veuillez modifier le code pour l’adapter à votre application avant de déployer cela en production. L’exemple doit être installé dans une application ASP.NET vide. Pour plus d’informations sur le package, consultez le billet de blog suivant : [annonce RTM d’ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Composants Microsoft OWIN

Il y avait beaucoup de bogues qui ont été résolus dans cette version. Consultez le [notes de publication pour le 2.1.0 version](https://katanaproject.codeplex.com/releases/view/113281) pour des informations plus détaillées.

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

Il y avait beaucoup de bogues qui ont été résolus dans cette version. Consultez le [notes de publication pour le 2.0.2 version](https://github.com/SignalR/SignalR/releases/tag/2.0.2) pour des informations plus détaillées.
