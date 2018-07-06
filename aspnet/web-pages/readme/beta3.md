---
uid: web-pages/readme/beta3
title: WebMatrix et ASP.NET Web Pages (Razor) bêta 3 version Lisez-moi | Microsoft Docs
author: rick-anderson
description: WebMatrix et ASP.NET Web Pages (Razor) bêta 3 version Lisez-moi
ms.author: aspnetcontent
ms.date: 01/10/2011
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 16b324e555b5450fc1e05c63e7e19985a2d02b89
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831830"
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a>WebMatrix et ASP.NET Web Pages (Razor) bêta 3 version Lisez-moi
====================
> WebMatrix et ASP.NET Web Pages (Razor) bêta 3 version Lisez-moi

9 novembre 2010

## <a name="contents"></a>Sommaire

- [Vue d’ensemble](#Overview)
- [Installation](#Installation_Notes)
- [Nouvelles fonctionnalités, les modifications et les problèmes connus dans la version bêta 3](#Known_Issues)

    - [Problèmes d’Installation de WebMatrix](#Known_Issues_Installation)
    - [Pages Web ASP.NET](#Known_Issues_ASPNET)
    - [SQL Server Compact](#Known_Issues_SQL_Server_Compact)
    - [Installation d’Applications](#Known_Issues_Installing_Applications)
    - [Publication d’Applications](#Known_Issues_Publishing_Applications)
    - [Autres problèmes](#Known_Issues_Other_Issues)
- [Pour plus d’informations](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Vue d'ensemble

> Microsoft WebMatrix Beta est une pile de développement web gratuit qui s’installe en quelques minutes. Il s’intègre un serveur web de base de données et infrastructures pour créer une expérience unique et intégrée de programmation. Vous pouvez utiliser la version bêta de WebMatrix pour simplifier la manière dont votre code, tester, publier votre propre site Web ASP.NET ou PHP, ou vous pouvez utiliser la version bêta de WebMatrix pour démarrer un nouveau site Web à l’aide d’applications open source populaires telles que DotNetNuke, Umbraco, WordPress ou Joomla. Version bêta de WebMatrix utilise le même serveur web puissant, moteur de base de données et infrastructures que ceux qui exécutera votre site Web sur internet, ce qui rend la transition du développement en production fluide et homogène.


<a id="Installation_Notes"></a>

## <a name="installation"></a>Installation

> Pour installer la version bêta 3 de WebMatrix, vous utilisez [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638). Après avoir installé Web Platform Installer, vous pouvez l’utiliser pour installer la version bêta 3 de WebMatrix.
> 
> Si vous rencontrez des problèmes lors de l’installation, reportez-vous à [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>Instructions pour la publication d’Applications

> Consultez [obtenir des Instructions détaillées pour la publication d’Applications](https://go.microsoft.com/fwlink/?LinkID=196149)


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>Nouveaux problèmes andKnown de fonctionnalités, les changements

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>Installation de la version bêta 3 de WebMatrix

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>Problème : Version bêta 3 de WebMatrix est disponible uniquement sur les plateformes qui prennent en charge de Microsoft .NET Framework 4

> Le .NET Framework version 4 est requis pour la version bêta de WebMatrix. Dans certains cas, le programme d’installation de la version bêta de WebMatrix permet d’essayer d’installer sur une plateforme qui ne fait pas partie de l’ensemble de la configuration prise en charge. En particulier, Vista Windows sans la mise à jour SP1 vous permet de commencer l’installation de WebMatrix bêta, mais le composant .NET Framework 4 échoue et bloquer votre installation.
> 
> **Solution de contournement**  
> Installer sur une plateforme prise en charge, ce qui inclut :
> 
> - Windows 7
> - Windows Server 2008
> - Windows Server 2008 R2
> - Windows Vista SP1 ou ultérieur
> - Windows XP SP3
> - Windows Server 2003 SP2


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problème : Ne peut pas installer WebMatrix bêta 3 si Microsoft Visual Studio 2008 est installé sans Microsoft Visual Studio 2008 SP1

> **Solution de contournement**  
> Installer [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) à partir du centre de téléchargement Microsoft.


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>Problème : Certains assemblys pour SQL Server Compact 4.0 ne sont pas installés dans le GAC

> Les assemblys managés pour SQL Server Compact 4.0 ne sont pas placés dans le global assembly cache (GAC) lorsque vous installez SQL Server Compact 4.0 sur un ordinateur 64 bits et de l’ordinateur a uniquement le .NET Framework 3.5 SP1 Client Profile installé. Les assemblys managés qui ne sont pas installés dans le GAC sont :
> 
> - *System.Data.SqlServerCe.dll* (fournisseur ADO.NET)
> - *System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)
> 
> **Solution de contournement**  
> Désinstaller SQL Server Compact 4.0. Téléchargez et installez la version complète de .NET Framework 3.5 SP1 à partir de l’emplacement suivant :  
>   
> [Microsoft .NET Framework 3.5 Service pack 1 (indépendant)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> Puis réinstallez SQL Server Compact 4.0.


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>Problème : Impossible de désinstaller SQL Server Compact à l’aide de la ligne de commande

> La désinstallation de SQL Server Compact à l’aide des options de ligne de commande ne fonctionne pas dans cette version.
> 
> **Solution de contournement**  
> Utilisez *programmes et fonctionnalités* dans le panneau de configuration Windows pour désinstaller Microsoft SQL Server Compact 4.0.


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>Pages web ASP.NET

Cette section du document décrit les nouvelles fonctionnalités, des modifications et des problèmes connus avec la version bêta 3 d’ASP.NET Web Pages avec syntaxe Razor.

- [Nouvelles fonctionnalités](#NewFeatures)
- [Modifications](#Changes)
- [Problèmes](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Nouvelles fonctionnalités dans la version bêta 3 pour ASP.NET Web Pages avec syntaxe Razor

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>Nouveauté : Méthode de « Utilisation de Html.Raw » restitue un balisage non codé

> La nouvelle `Html.Raw` méthode vous permet de restituer le balisage HTML en tant que balisage au lieu du rendu de sortie encodée. (Par défaut, ASP.NET Razor encode les chaînes avant leur rendu.) La syntaxe est :
> 
> `Html.Raw(value)`
> 
> L'exemple suivant montre comment utiliser `Html.Raw` :
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Modifications dans la version bêta 3 pour ASP.NET Web Pages avec syntaxe Razor

#### <a name="change-hrefattribute-method-removed"></a>Change : Méthode de « HrefAttribute » supprimée

> Le `HrefAttribute` méthode de la `WebPage` classe a été supprimée. Ce programme d’assistance a été utilisé pour encoder des caractères non sécurisés dans les URL. Il n’est plus nécessaire, car ASP.NET Razor encode automatiquement les chaînes. (Utilisez la nouvelle `Html.Raw` méthode pour restituer des chaînes non codés.)


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>Modification : Syntaxe déclarative «@helper« helpers modifiés

> Dans la version bêta 3, qu’apporte ASP.NET comment il analyse les programmes d’assistance qui sont créés à l’aide de la `@helper` syntaxe. En bref, le `@helper` syntaxe est maintenant analysée comme un bloc de code et non comme un bloc de balisage qui peut inclure du code. Par conséquent, le code à l’intérieur du programme d’assistance sans devoir être placé entre `@{ }` blocs. À l’inverse, balisage à l’intérieur du programme d’assistance doit être explicitement inclus dans les éléments HTML ou ASP.NET Razor `<text></text>` balises.
> 
> Par exemple, ce qui suit `@helper` fonctionne de la syntaxe dans la version bêta 3 :
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> Dans la version bêta 3, ce programme d’assistance doit être modifiée pour ressembler à l’exemple suivant :
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> Notez que le `@{ }` caractères autour du code initial dans l’application auxiliaire n’est plus utilisé. Il s’agit, car le contenu des programmes d’assistance est traité comme un bloc de code par défaut. Le programme d’assistance restitue le balisage, qui commence à l’ouverture `<a>` balise. Si l’application d’assistance doit effectuer le rendu du texte brut ou les balises qui n’incluent pas une balise de fermeture (par exemple, `<meta>` balises), le contenu rendu doit être dans `<text></text>` balises.


#### <a name="change-webpagecontexthttpcontext-removed"></a>Modifications : « WebPageContext.HttpContext » supprimées

> Le `WebPageContext.HttpContext` propriété a été supprimée. Utilisez plutôt `HttpContext.Current`. (Le `WebPageContext.HttpContext` propriété simplement encapsulée cela.)


#### <a name="change-facebook-helper-moved-to-new-package"></a>Modification : Déplacé vers le nouveau package application auxiliaire de « Facebook »

> Le `Facebook` helper a été déplacé vers le *Facebook.Helper* library, qui inclut le `Facebook` d’assistance et des fonctionnalités supplémentaires. Vous devez installer cette bibliothèque en tant qu’un package distinct, comme décrit dans « L’installation de programmes d’assistance avec Package Manager » dans le didacticiel [mise en route avec les Pages ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>Modification : L’appartenance au rôle et la sécurité des types déplace au nouvel assembly

> Les types suivants ont été déplacées vers le `WebMatrix.WebData` assembly :
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>Modification : Classe « TagBuilder » déplacé vers System.Web.WebPages.dll assembly

> Le `TagBuilde` classe r a été déplacée vers l’assembly System.Web.WebPages.dll. Auparavant, cette opération était dans un assembly qui faisait partie d’ASP.NET MVC. Cette modification signifie que vous n’êtes pas obligé d’installer ASP.NET MVC pour pouvoir utiliser le `TagBuilder` classe.
> 
> Toutefois, la classe est toujours dans le `System.Web.Mvc` espace de noms. Pour pouvoir utiliser le `TagBuilder` classe (par exemple, dans un ASP.NET Razor d’assistance personnalisée), vous devez référencer l’espace de noms (par exemple, en ajoutant `@using System.Web.Mvc` à votre code).


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>Modification : Demande syntaxe de validation modifié ; Classe de « Validation » supprimé

> Dans la version bêta 3, pour désactiver la validation pour un champ individuel ou un ensemble de champs, que vous pouvez appeler la `Validation.Exclude` méthode, en passant l’ou les noms des champs à exclure de la validation. Une nouvelle syntaxe est disponible dans la version bêta 3 pour contourner la validation. Le `Validation` méthode utilisée dans la version bêta 3 a été supprimé.
> 
> > [!NOTE]
> > Si vous ne désactivez pas la validation de la demande, si des utilisateurs tentent de télécharger le balisage HTML (par exemple, en utilisant un éditeur de texte sur une page), le site Web signale une erreur telle que *une valeur potentiellement dangereuse de Request.Form a été détectée à partir du client*et l’entrée d’utilisateur n’est pas acceptée. Si vous désactivez la validation de la demande, vous devez vérifier manuellement les entrées d’utilisateur pour vous assurer qu’il ne contient pas balisage potentiellement dangereux ou script à l’aide de quelque chose comme la [Microsoft Anti-Cross Site Scripting Library V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).
> 
> 
> Pour désactiver la validation de demande automatique, appelez le `Request.Unvalidated` méthode, en lui transmettant le nom du champ ou l’autre objet du billet que vous souhaitez ignorer la validation de demande pour. Vous pouvez utiliser cette méthode pour ignorer la validation pour tous les éléments dans le `Form`, `QueryString`, `Cookies`, et `ServerVariables` collections. Les exemples suivants montrent comment utiliser le `Unvalidated` méthode :
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>Problèmes connus pour ASP.NET Web Pages avec syntaxe Razor

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problème : Un comportement inattendu lors de l’utilisation d’une table utilisateur personnalisée pour l’appartenance

> Pour initialiser le fournisseur d’appartenances pour un site Web ASP.NET Razor, vous appelez le `WebSecurity.InitializeDatabaseConnection` (méthode). (Dans WebMatrix, du modèle Starter Site inclut un appel à cette méthode dans le  *\_AppStart.cshtml* fichier.) Si le `autoCreateTables` paramètre de cette méthode est définie sur true (par défaut, il est défini sur true dans le modèle Starter Site), et si un nom de table non reconnu est passé à la méthode (le deuxième paramètre), la méthode ne lève pas une erreur. Au lieu de cela, il crée automatiquement la table.
> 
> Cela peut poser un problème si vous avez l’intention d’utiliser une table utilisateur personnalisée pour l’appartenance, mais passez le nom de table incorrect à la `WebSecurity.InitializeDatabaseConnection` (méthode). Étant donné que la méthode ne pas par défaut déclenche une erreur si la table spécifiée n’existe pas, et parce qu’il crée à la place une nouvelle table, l’application peut apparaître à travailler. Toutefois, code d’application qui s’appuie sur la table utilisateur personnalisée (et sur les champs qu’il contient) peut finit par échouer avec des erreurs inattendues.
> 
> **Solution de contournement**  
> Assurez-vous que le nom transmis dans le `InitializeDatabaseConnection` correspondances méthode le profil utilisateur figurant dans la base de données d’appartenance, ou vous assurer que le `autoCreateTables` paramètre est défini sur false.


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problème : erreur « Échec de générer une instance utilisateur de SQL Server »

> Si une application Web de WebMatrix utilise SQL Server Express et est en cours d’exécution IIS 7.5 sur Windows 7 ou Windows Server 2008 R2, vous pouvez voir une erreur qui indique que SQL Server ne peut pas récupérer le chemin d’accès de l’utilisateur local d’application en cours d’exécution.
> 
> **Solution de contournement** vous assurer que le compte Windows de l’application sous lequel s’exécute (généralement NETWORK SERVICE) dispose des autorisations de lecture/écriture pour les dossiers racine de l’application et des sous-dossiers comme *application\_données*. Des informations plus détaillées sont disponibles dans l’article de la base de connaissances [des problèmes avec les projets d’Application Web ASP.net et d’utilisateur SQL Server Express l'instanciation](https://support.microsoft.com/kb/2002980).


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>Problème : Dans Visual Studio, espaces de noms pour les assemblys personnalisés (DLL) ne sont pas importés automatiquement

> Si vous utilisez des assemblys personnalisés dans un projet dans Visual Studio, les espaces de noms déclarés dans ces assemblys ne sont pas importés automatiquement au moment du design. Par conséquent, les références à des types personnalisés peuvent ne pas être reconnus au moment du design et sont marqués comme non reconnu dans Visual Studio (à l’aide d’un tilde « »). Ce problème se produit uniquement au moment du design dans Visual Studio. l’application elle-même s’exécute correctement.
> 
> **Solution de contournement**  
> Inclure un `using` instruction (`imports` en Visual Basic) qui référence les entités qui ne sont pas reconnues au moment du design.


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problème : Visual Studio IntelliSense modèles de projet et disponibles uniquement dans ASP.NET MVC version 3

> L’installation d’ASP.NET Web Pages ne pas également installe outils pour Visual Studio telles que des modèles de projet et IntelliSense pour les applications ASP.NET Web Pages.
> 
> **Solution de contournement** pour utiliser des modèles de projet et IntelliSense pour les applications de Pages Web ASP.NET dans Visual Studio, installer ASP.NET MVC 3 RC via Web Platform Installer ou le [programme d’installation autonome](https://go.microsoft.com/fwlink/?LinkID=191797).


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>Problème : «&lt;helper&gt; Impossible de trouver la classe « erreur

> Une fois que vous mettez à niveau vers la version bêta 3, vous pouvez obtenir une erreur qui une classe d’assistance (par exemple, le `Facebook` classe) ne peut pas être introuvable. À compter de la version bêta 2 et continuer dans la version bêta 3, les programmes d’assistance ont été déplacés aux packages que vous devez installer explicitement. Sites existants ne sont pas mis à niveau pour inclure ces packages ; Cela inclut les sites dans le *\My Documents\IISExpress* ou *\My Documents\My Sites Web* dossiers. En particulier, vous verrez cette erreur si vous utilisez le site par défaut dans *Mes Sites* (WebSite1), qui inclut une référence à la `Twitter` helper.
> 
> **Solution de contournement**  
> Commentez les appels à des programmes d’assistance dans le site, exécutez le  *\_administrateur* page et installer l’ou les packages qui incluent les programmes d’assistance que vous souhaitez utiliser. Une fois que vous avez installé le package, vous pouvez ne pas commenter les lignes qui référencent des programmes d’assistance.


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>Problème : Déploiement d’assemblys de la version bêta 3 ASP.NET Razor dans le dossier Bin peut ne pas fonctionne sur l’hébergement de sites

> Si vous déployez un site Web ASP.NET Web Pages sur un site d’hébergement, et si vous déployez les assemblys ASP.NET Razor bêta 3 sur le site *Bin* dossier, vous pouvez rencontrer des erreurs, y compris les éléments suivants :
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> Cela peut se produire si le fournisseur d’hébergement a installé les assemblys ASP.NET Web Pages bêta 1 dans le cache du serveur application global (GAC). Assemblys dans le GAC obtenir la priorité sur les assemblys installés localement dans le *Bin* dossier.
> 
> **Solution de contournement** contactez votre fournisseur d’hébergement pour confirmer que les erreurs que vous voyez sont en raison d’un conflit entre les versions du fournisseur des assemblys et les vôtres. Dans ce cas, demander que le fournisseur d’hébergement mettre à jour les assemblys dans le GAC du serveur.


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problème : Lecture des flux ou autres données externes via un serveur proxy

> Si le serveur qui exécute le site est derrière un serveur proxy, vous devrez peut-être configurer les informations de proxy dans le *Web.config* fichier afin d’être en mesure de lire les informations provenant de l’extérieur de votre site. Par exemple, si vous utilisez le `ReCaptcha` helper, l’application d’assistance communique avec le service reCAPTCHA, mais peut être bloquée par votre serveur proxy. De même, les flux sont utilisés dans les Pages Web ASP.NET, par exemple, le flux utilisé par le Gestionnaire de package, peuvent nécessiter la configuration du proxy.
> 
> Si vous rencontrez des problèmes dans avec un service externe ou fonctionnent correctement avec le flux du package, placez les éléments suivants dans la racine de votre application *Web.config* fichier :
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> Pour plus d’informations sur la configuration d’un serveur proxy, consultez [ &lt;proxy&gt; , élément (paramètres réseau)](https://msdn.microsoft.com/library/sa91de1e.aspx) sur le site Web MSDN.


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>Problème : L’erreur « Impossible de charger Microsoft.Web.Infrastructure.dll »

> Si vous déjà installé la version bêta 1 d’ASP.NET Web Pages avec syntaxe Razor et puis installez la version bêta 3, tous les assemblys appropriés sont installés dans le GAC à l’exception *Microsoft.Web.Infrastructure.dll*. Par conséquent, lorsque vous exécutez des pages ASP.NET Razor, vous voyez une erreur qui indique que *Microsoft.Web.Infrastructure.dll* pas pu être chargé.
> 
> Ce problème ne se produit pas si vous avez chargé la version bêta 3 sur un ordinateur sain.
> 
> **Solution de contournement**  
> Dans le panneau de configuration, désinstallez les Pages Web ASP.NET. Puis réinstallez la version bêta 3.


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problème : La désinstallation de la version 4 du .NET Framework désactive ASP.NET Web Pages avec syntaxe Razor

> Si vous désinstallez le Kit de développement .NET Framework version 4, puis le réinstaller, ASP.NET Web Pages avec syntaxe Razor est désactivés. Pages avec la *.cshtml* extension ne s’exécutent pas correctement. ASP.NET Web Pages enregistre un assembly dans la racine de la machine *Web.config* des fichiers et la suppression de .NET Framework supprime ce fichier. Réinstallez le .NET Framework installe une nouvelle version du fichier de configuration, mais n’ajoute pas la référence de l’assembly d’ASP.NET Web Pages.
> 
> **Solution de contournement** après la réinstallation de .NET Framework, réinstallez ASP.NET Web Pages avec syntaxe Razor. Cette opération ajoute l’élément suivant à la *Web.config* fichier à la racine de l’ordinateur, qui est en général à l’emplacement suivant :  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>Problème : Les Applications précédemment déployées avec des assemblys ASP.NET dans le dossier Bin rencontrent des erreurs

> Au cours du déploiement, les copies des assemblys ASP.NET Web Pages (par exemple, *Microsoft.WebPages.dll*) pour le *Bin* dossier du site Web sur le serveur. (Cela peut se sont produites automatiquement pendant le déploiement ou parce que le développeur copié explicitement les assemblys.) Toutefois, lorsque la version bêta 3 est installée, les erreurs se produit, telles que les erreurs qui ne figure pas certains types. Cela se produit, car un nombre de types de Pages Web ASP.NET ont été déplacées dans différents espaces de noms pour la version bêta 3.
> 
> **Solution de contournement**   
> Effacer la *Bin* dossier de l’application déployée, copier les nouveaux assemblys dans le dossier (ou redéployer l’application), puis redémarrez l’application.


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>Problème : Les URL sans extension ne trouvent pas les fichiers.cshtml/.vbhtml sur IIS 7 ou IIS 7.5

> Sur IIS 7 ou IIS 7.5, les requêtes avec une URL semblable à la suivante ne sont pas en mesure de trouver des pages qui ont le *.cshtml* ou *.vbhtml* extension :  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> Le problème survient car la réécriture d’URL n’est pas activée par défaut pour IIS 7 ou IIS 7.5. Le scénario voyez généralement cette est que vous ne voyez pas le problème lorsque vous testez localement à l’aide d’IIS Express, mais que vous le rencontrez lorsque vous déployez votre site Web sur un site Web d’hébergement.
> 
> **Solution de contournement**
> 
> - Si vous avez un contrôle sur l’ordinateur du serveur, sur l’ordinateur serveur installer la mise à jour qui est décrite dans [une mise à jour est disponible que permet certains gestionnaires d’IIS 7.0 ou IIS 7.5 pour gérer les demandes dont les URL ne se termine pas par un point](https://support.microsoft.com/kb/980368).
> - Si vous n’avez pas de contrôle sur l’ordinateur du serveur (par exemple, vous déployez sur un site Web d’hébergement), ajoutez le code suivant du site Web *Web.config* fichier :
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>Problème : À l’aide de projet d’Application Web ou ASP.NET MVC et ASP.NET Web pages dans la même application

> Si vous utilisiez ASP.NET Web Pages dans un projet d’Application Web ou d’une application ASP.NET MVC, vous pouvez obtenir une erreur qui *WebPageHttpApplication* est introuvable.
> 
> **Solution de contournement**  
> Si vous obtenez cette erreur, modifiez la classe de base dont dérive l’application. Dans le *Global.asax* , modifiez la ligne suivante :
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> Par ceci :
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> Cela inverse ce qui entraîne une modification a été introduite pour la version bêta 1 d’ASP.NET Web Pages avec syntaxe Razor.


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problème : Déploiement d’une application sur un ordinateur qui n’a pas de SQL Server Compact installé

> Les applications qui incluent des bases de données SQL Server Compact peuvent s’exécuter sur un ordinateur où SQL Server Compact n’est pas installé. Version bêta 3 de Microsoft WebMatrix automatiquement des copies de ces fichiers binaires et effectue approprié *Web.config* transformations de fichiers.
> 
> **Solution de contournement** si vous devez copier ces fichiers et rendre le *Web.config* modifications de fichiers manuellement, effectuer les opérations suivantes :
> 
> 1. Copiez les assemblys du moteur de base de données à la *Bin* dossier (et sous-dossiers) de l’application sur l’ordinateur cible : 
> 
>     - Copie *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **à** *\Bin*
>     - Copie *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\** **à** *\Bin\x86*
>     - Copie *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\** **à** *\Bin\amd64*
> 2. Dans le dossier racine du site Web, créez ou ouvrez un *Web.config* fichier. (Dans la version bêta 3 de WebMatrix, ce type de fichier est disponible si vous cliquez sur **tous les** dans le **choisir un Type de fichier** boîte de dialogue.)
> 3. Ajoutez l’élément suivant en tant qu’enfant de le **&lt;configuration&gt;** élément (pas à l’intérieur de la **&lt;system.web&gt;** élément) :
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problème : La base de données et de WebGrid helpers ne fonctionnent pas dans Medium Trust en Visual Basic

> Si vous utilisez Visual Basic (création *.vbhtml* fichiers), le `Database` et `WebGrid` helpers ne fonctionnera pas si l’application est configurée pour utiliser la confiance moyenne.
> 
> **Solution de contournement**  
> Définissez temporairement l’application pour utiliser la confiance totale.

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>Problème : Propriété de « Encrypt » n’est pas reconnue.

> SQL Server Compact 4.0 ne reconnaît pas le `Encrypt` propriété de la `SqlCeConnection` classe. Vous n’utilisez pas cette propriété pour chiffrer les fichiers de base de données. Le `Encrypt` propriété a été déconseillée dans la version 3.5 de SQL Server Compact et a été conservée uniquement pour la compatibilité descendante. 
> 
> **Solution de contournement**  
> Utilisez le `Encryption Mode` propriété de la `SqlCeConnection` classe pour chiffrer les fichiers de base de données SQL Server Compact 4.0. L’exemple suivant montre comment créer une base de données SQL Server Compact 4.0 chiffrée à l’aide du `Encryption Mode` propriété :
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> Pour modifier le mode de chiffrement de base de données SQL Server Compact 4.0 existante, procédez comme suit :
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> Pour chiffrer une base de données SQL Server Compact 4.0 non chiffré, procédez comme suit :
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a>Problème : Les bibliothèques runtime Microsoft Visual C++ 2008 sont requis

> Le natif DLL de SQL Server Compact 4.0 doivent le Microsoft Visual C++ 2008 bibliothèques Runtime (x 86, IA64 et x 64), le Service Pack 1.
> 
> **Solution de contournement**  
> Installer le .NET Framework 3.5 SP1. Cette commande installe également le Runtime bibliothèques de Visual C++ 2008 SP1. Vous pouvez télécharger les bibliothèques à partir de l’emplacement suivant :   
>   
> [Mise à jour de la sécurité de Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> Notez que le .NET Framework 2.0, 3.0, l’installation ou de fait de 4 *pas* installer le SP1 bibliothèques Runtime de Visual C++ 2008.


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>Problème : Si SQL Server Compact est installé avant d’installer le .NET Framework sur l’ordinateur, son nom invariant du fournisseur n'est pas inscrit dans le fichier machine.config de .NET Framework

> SQL Server Compact peut être installé sur un ordinateur qui n’a pas de .NET Framework est installé, car SQL Server Compact nécessite le .NET framework. Si ni .NET Framework version 3.5 ni 4 est installé avant d’installer SQL Server Compact, le programme d’installation SQL Server Compact n’inscrit pas son nom invariant du fournisseur dans le *machine.config* fichier. N’importe quelle application qui s’appuie sur l’entrée de SQL Server Compact dans le *machine.config* fichier échoue. L’entrée d’inscription de nom invariant dans *machine.config* ressemble à l’exemple suivant :
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **Solution de contournement**  
> Désinstaller SQL Server Compact 4.0 CTP1. Téléchargez et installez les versions complètes de .NET Framework à partir de l’emplacement suivant :
> 
> [Microsoft .NET Framework 3.5 Service pack 1 (indépendant)](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Mise en production de Microsoft .NET Framework 4.0 (indépendant)](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> Puis réinstallez [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>Installation d’Applications

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problème : Installation d’une application peut prendre un certain temps si le dossier Mes Documents de l’utilisateur est redirigé vers un partage réseau

> **Solution de contournement**  
> Aucun. L’application peut prendre un certain temps à installer, mais ne s’installe correctement.


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>Publication d’Applications

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>Problème : Site peuvent ne pas fonctionne après la publication si le champ « URL de Destination » n’est pas précédé par http:// ou https://

> Dans le **paramètres de publication** boîte de dialogue, si l’URL de destination ne commence pas par `http://` ou `https://`, le site peut ne pas fonctionne après le déploiement.
> 
> **Solution de contournement**  
> Assurez-vous que l’option avant de publier un site, l’URL de destination dans le **paramètres de publication** boîte de dialogue commence par `http://` ou `https://`.


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>Problème : Publication d’une base de données MySQL échoue avec l’erreur « Échec de publication de la base de données. Cela peut se produire si la base de données distante ne peut pas exécuter le script. »

> L’erreur peut se produire pour plusieurs raisons. Vous pouvez voir cette erreur est si le script de base de données contient un guillemet simple (') et le jeu de caractères par défaut de la destination MySQL de base de données n’est pas au format UTF-8.
> 
> **Solution de contournement**  
> Définir le caractère par défaut définie pour la base de données MySQL à distance au format UTF-8.


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>Autres problèmes

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>Problème : Les recherche/filtre ne fonctionne pas dans les rapports pour Group By : Type de problème

> Lorsque vous exécutez un rapport pour un site, si vous entrez du texte dans le *filtrer par URL* , puis cliquez sur *recherche*, rien ne se produit. Il s’agit, car ce contrôle ne fonctionne pas lors de la *Group By* état du rapport est défini sur *Type de problème*, qui est la valeur par défaut.
> 
> **Solution de contournement** dans le *Group By* onglet du ruban, cliquez sur *URL* pour regrouper les entrées par leur URL source. La zone de texte et un bouton pour filtrer les entrées sont fonctionnels dans cet état.


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>Problème : WCF applications ne fonctionnent pas avec IIS Express

> Accédant à une application WCF génère une erreur semblable à celui-ci :
> 
> *Impossible de charger le fichier ou l’assembly ' Microsoft.Web.Administration, Version = 7.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' ou une de ses dépendances. Le système ne parvient pas à localiser le fichier spécifié.*
> 
> Cela se produit, car la version IIS Express Bêta ne prend pas en charge WCF par défaut.
> 
> **Solution de contournement** utiliser l’une des solutions de contournement suivantes (solution de contournement #2 requiert Microsoft Windows Vista ou version ultérieure) :
> 
> 
> 1. Copie le *Microsoft.Web.dll* et *Microsoft.Web.Administration.dll* assemblys à partir de l’emplacement d’installation de WebMatrix vers le *bin* répertoire de WCF application. Par défaut, WebMatrix est installé dans le *Microsoft WebMatrix* sous-dossier sous le système *Program Files* dossier.
> 2. Sur Microsoft Windows Vista ou version ultérieure, créez un lien symbolique vers les assemblys dans le *bin* répertoire en utilisant les commandes suivantes. (Cette approche présente l’avantage qu’il ne crée pas une copie des assemblys.)
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. Installer les deux assemblys dans le GAC. À partir d’une invite de commandes avec élévation de privilèges, exécutez les commandes suivantes :
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problème : La version bêta 3 de WebMatrix est impossible d’effectuer certaines tâches qui requièrent une élévation

> Version bêta 3 de WebMatrix est impossible d’effectuer certaines tâches qui requièrent une élévation, telles que l’installation des composants supplémentaires dans les situations suivantes :
> 
> - Sur Windows Vista ou Windows 7, vous êtes connecté avec un compte qui ne dispose pas des privilèges d’administrateur et contrôle de compte utilisateur (UAC) est désactivé.
> - Vous utilisez Microsoft Windows XP ou Microsoft Windows Server 2003.
> 
> **Solution de contournement**  
> La plupart des tâches dans la version bêta 3 de WebMatrix ne nécessitent pas d’autorisation d’administrateur. Pour les autres, vous pouvez effectuer l’opération en tant qu’administrateur, ou procédez comme suit :
> 
> - Sur Windows Vista ou Windows 7, activer l’UAC.
> - Sur Windows XP, ajoutez l’utilisateur au groupe de sécurité Administrateurs.


#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problème : « Site depuis la galerie Web » est désactivée.

> Le **Site à partir de la galerie Web** option est désactivée si le serveur Web Platform Installer 3.0 n’est pas installé.
> 
> **Solution de contournement**  
> Installer le [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>Problème : Sur Windows Server 2003, IIS Express ne démarre pas pour un utilisateur non-administrateur

> Sur Windows Server 2003, lance une page ou au démarrage d’IIS Express, IIS Express ne démarre pas. Pour les pages Web, une erreur s’affiche qui indique que l’application a été démarrée par un utilisateur non-administrateur.
> 
> **Solution de contournement**  
> Démarrer la version bêta 3 de WebMatrix en tant qu’administrateur. Pour plus d’informations, consultez l’article suivant de la base de connaissances :  
>   
> [Une application est démarrée par un utilisateur non-administrateur ne peut pas écouter le trafic HTTP de l’ordinateur sur lequel l’application s’exécute dans Windows Vista, Windows Server 2003 ou Windows XP.](https://support.microsoft.com/kb/939786)


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>Problème : Google Chrome n’est pas disponible en tant qu’une option d’exécution

> Google Chrome n’est pas affiché dans la liste des navigateurs sous **exécuter** sur le **accueil** onglet.
> 
> **Solution de contournement**  
> Certaines versions de Google Chrome n’inscrivez pas eux-mêmes correctement avec la fonctionnalité programmes par défaut dans Windows. Pour résoudre ce problème, démarrez Google Chrome, cliquez sur le *personnaliser et contrôle Google Chrome* menu, cliquez sur *Options*, puis cliquez sur *Make Google Chrome mon navigateur par défaut*.


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>Problème : La boîte de dialogue « Foreign Key » n’autorise pas entrer une clé primaire

> Le **clé étrangère** boîte de dialogue ne vous permet pas à entrer le nom de clé primaire à partir de la table de clé primaire.
> 
> **Solution de contournement**  
> Ceci est intentionnel. Vous n’avez pas besoin d’entrer le nom de la clé primaire à partir de la table de clé primaire.


#### <a name="issue-the-relationships-button-is-disabled"></a>Problème : Le bouton « Relations » est désactivé.

> Le **relations** bouton sous le **Table** onglet dans le **bases de données** espace de travail est désactivé pour les bases de données SQL Server Compact.
> 
> **Solution de contournement**  
> Aucun. SQL Server Compact ne prend pas en charge les relations entre les tables.


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>Problème : Les requêtes SQL paramétrables lever des exceptions

> Dans SQL Server Compact 4.0, si vous ne spécifiez pas un type de données tel que `SqlDbType` ou `DbType` pour les paramètres dans les requêtes paramétrables, une exception est levée lorsque la requête s’exécute.
> 
> **Solution de contournement**  
> Définir explicitement le type de données pour les paramètres tels que `SqlDbType` ou `DbType`. Cela est essentiel dans le cas des types de données BLOB (`image` et `ntext`). Utilisez un code semblable au suivant :
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a>Pour plus d'informations

Pour plus d’informations sur la version bêta 3 de WebMatrix, consultez les sites Web suivants :

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

* * *

© 2010 Microsoft Corporation. Tous droits réservés. [Conditions d’utilisation](https://msdn.microsoft.cos/cc300389.aspx).
