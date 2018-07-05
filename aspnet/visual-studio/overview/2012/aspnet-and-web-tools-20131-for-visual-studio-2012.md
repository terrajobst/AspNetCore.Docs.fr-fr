---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Notes de version pour ASP.NET et Web Tools 2013.1 pour Visual Studio 2012 | Microsoft Docs
author: microsoft
description: Ce document décrit la version d’ASP.NET et Web Tools 2013.1 pour Visual Studio 2012.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2013
ms.topic: article
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 85cd45c25e0f2ad3c8d6d6de73a1a493533e7f7b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374258"
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Notes de publication pour ASP.NET et Web Tools 2013.1 pour Visual Studio 2012
====================
par [Microsoft](https://github.com/microsoft)

> Ce document décrit la version d’ASP.NET et Web Tools 2013.1 pour Visual Studio 2012.


## <a name="contents"></a>Sommaire

- [Notes d’installation](#install)
- [Configuration logicielle requise](#requirements)
- Nouvelles fonctionnalités dans ASP.NET et Web Tools 2013.1 pour Visual Studio 2012

    - [Bootstrap](#bootstrap)
    - [Modèles](#templates)

        - [Modèle ASP.NET MVC 5](#mvc5template)
        - [Modèle d’API Web 2 ASP.NET](#apitemplate)
        - [Modèles d’élément](#itemtemplate)
    - [Entity Framework 6](#ef6)
    - [Génération de modèles automatique ASP.NET](#scaffold)
    - [Razor-éditeur](#razor)
    - [NuGet 2.7](#nuget)
- Problèmes connus et les modifications avec rupture

    - [Génération de modèles automatique ASP.NET](#issuescaffolding)

        - [MVC et Web API génération de modèles automatique - erreur HTTP 404, erreur introuvable](#404issue)
        - [Visual Studio Express 2012 pour Web cesse de fonctionner après l’ajout d’un élément généré automatiquement](#expressissue)
    - [Razor ASP.NET 3](#issuerazor)

        - [Affichage fichier cshtml avec naviguer avec ou F5 provoque une erreur de serveur](#browseissue)
        - [Tilde(~) et réécriture d’Url](#rewriteissue)
    - [Modèles](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a>Notes d’installation

[Installer](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET et Web Tools 2013.1 pour Visual Studio 2012.

<a id="requirements"></a>
## <a name="software-requirements"></a>Configuration logicielle

Vous devez disposer de Visual Studio 2012 ou Visual Studio Express 2012 pour le Web.

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Nouvelles fonctionnalités dans ASP.NET et Web Tools 2013.1 pour Visual Studio 2012

<a id="bootstrap"></a>
### <a name="bootstrap"></a>Programme d’amorçage

Lorsque vous structurer les contrôleurs MVC 5 et des vues, le balisage pour les vues utilise [Bootstrap](http://getbootstrap.com/).

<a id="templates"></a>
### <a name="templates"></a>Modèles

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>Modèle ASP.NET MVC 5

Nous avons ajouté un nouveau modèle MVC 5. Il référence les derniers packages NuGet de 5 MVC, et vous pouvez utiliser la génération de modèles automatique pour ajouter des contrôleurs et des vues.

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>Modèle d’API Web 2 ASP.NET

Nous avons ajouté un nouveau modèle API Web 2. Il référence les derniers packages NuGet de Web API 2, et vous pouvez utiliser la génération de modèles automatique pour ajouter des contrôleurs et des vues.

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>Modèles d’élément

Nous avons ajouté de nouveaux modèles d’élément pour les vues MVC 5, Web Pages (Razor 3) et les contrôleurs API Web 2. Ils installent les packages NuGet associées au projet lors de l’ajout de nouveaux éléments.

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

Lorsque vous générer automatiquement un contrôleur MVC ou API Web à l’aide d’Entity Framework, nous utilisons le Framework 6. Pour plus d’informations sur Entity Framework, consultez le [l’historique de Version Entity Framework](https://msdn.com/data/jj574253).

Vous pouvez également télécharger et installer les outils Entity Framework 6 pour Visual Studio 2012. Consultez le [obtenir Entity Framework](https://msdn.com/data/ee712906#tooling).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>Génération de modèles automatique ASP.NET

Génération de modèles automatique ASP.NET est une infrastructure de génération de code pour les applications Web ASP.NET. Il est facile d’ajouter du code réutilisable à votre projet qui interagit avec un modèle de données.

Dans les versions précédentes de Visual Studio, génération de modèles automatique a été limitée pour les projets ASP.NET MVC. Avec cette mise à jour, vous pouvez maintenant utiliser la génération de modèles automatique pour un projet ASP.NET, y compris les Web Forms. Cette mise à jour ne prend pas en charge la génération de pages pour un projet Web Forms, mais vous pouvez toujours utiliser la structure avec les Web Forms en ajoutant des dépendances MVC au projet. Prise en charge pour la génération de pages pour Web Forms sera ajoutée dans une prochaine mise à jour.

Lorsque vous utilisez la génération de modèles automatique, nous nous assurer que tous les requis dépendances sont installées dans le projet. Par exemple, si vous commencez avec un projet Web Forms ASP.NET et ensuite utilisez la structure pour ajouter un contrôleur d’API Web, références et les packages NuGet requis sont ajoutés à votre projet automatiquement.

Pour ajouter la structure MVC à un projet Web Forms, ajoutez un **nouvel élément structuré** et sélectionnez **des dépendances MVC 5** dans la boîte de dialogue. Il existe deux options pour la génération de modèles automatique MVC ; Minimale et complète. Si vous sélectionnez Minimal, uniquement les packages NuGet et les références pour ASP.NET MVC sont ajoutés à votre projet. Si vous sélectionnez l’option complet, les dépendances minimales sont ajoutés, ainsi que les fichiers de contenu requis pour un projet MVC.

Prise en charge de la génération de modèles automatique des contrôleurs d’async utilise les nouvelles fonctionnalités asynchrones à partir d’Entity Framework 6.

Pour plus d’informations et des didacticiels, consultez [vue d’ensemble de la génération de modèles automatique ASP.NET](../2013/aspnet-scaffolding-overview.md). Ces didacticiels montrent la structure avec Visual Studio 2013, mais ils sont également applicables à ASP.NET et Web Tools 2013.1 pour Visual Studio 2012.

<a id="razor"></a>
### <a name="razor-editor"></a>Razor-éditeur

Avec cette mise à jour, Visual Studio 2012 prend désormais en charge l’édition/outils Razor 3.

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 inclut un ensemble complet de nouvelles fonctionnalités qui sont décrites en détail à [Notes de publication de NuGet 2.7](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Cette version de NuGet supprime la nécessité pour les utilisateurs à autoriser explicitement NuGet pour restaurer les packages manquants. Lorsque vous installez NuGet 2.7, les utilisateurs implicitement donner son consentement pour restaurer automatiquement les packages manquants. Les utilisateurs peuvent refuser explicitement la restauration de package via les paramètres de NuGet dans Visual Studio. Cette modification simplifie le fonctionne de la restauration de package.

## <a name="known-issues-and-breaking-changes"></a>Problèmes connus et les modifications avec rupture

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>Génération de modèles automatique ASP.NET

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC et Web API génération de modèles automatique - erreur HTTP 404, erreur introuvable

Si vous rencontrez une erreur lors de l’ajout d’un élément généré automatiquement à un projet, il est possible de que votre projet reste dans un état incohérent. Certaines des modifications apportées à être de génération de modèles automatique seront restaurée, mais d’autres modifications, comme les packages NuGet installés, ne seront pas restaurées. Si les modifications de configuration de routage sont annulées, les utilisateurs recevront une erreur HTTP 404 lorsque accédant à structurée des éléments.

Pour corriger cette erreur pour MVC, ajoutez un nouvel élément généré automatiquement et sélectionnez des dépendances MVC 5 (complète ou minimale). Ce processus sera ajoutez toutes les modifications nécessaires à votre projet.

Pour corriger cette erreur pour l’API Web :

1. Ajoutez la classe WebApiConfig suivante à votre projet.

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. Configurer WebApiConfig.Register dans l’Application\_méthode Start dans Global.asax comme suit :

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 pour Web cesse de fonctionner après l’ajout d’un élément généré automatiquement

Si Visual Studio Express 2012 pour Web cesse de fonctionner après l’ajout d’un élément généré automatiquement avec Entity Framework (par exemple, contrôleur API Web 2 avec actions, à l’aide d’Entity Framework), il est possible que Visual Studio Express Impossible de charger l’image native d’un assembly en fonction de System.Web.Extensions.

Pour corriger ce problème, configurez Visual Studio Express pour travailler avec l’image MSIL de System.Web.Extensions :

1. Ouvrez l’invite de commandes en mode administrateur.
2. Accédez à %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE ou % ProgramFiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (pour 64 bits de Windows).
3. Ouvrez VWDExpress.exe.config dans un éditeur de texte.
4. Ajoutez la ligne suivante sous le &lt;configuration&gt;/&lt;runtime&gt; élément :  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. Redémarrez Visual Studio Express 2012 pour le Web.

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>Razor ASP.NET 3

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a>Affichage cshtml fichier withBrowse WithorF5causes une erreur de serveur

Lorsque vous créez un projet MVC 5 dans Visual Studio 2012 (ou les ouvrir dans un projet Visual Studio 2012 un MVC 5 qui a été créé dans Visual Studio 2013) et que vous essayez d’afficher un fichier cshtml à l’aide de naviguer avec ou F5, vous recevrez une erreur indiquant - **erreur serveur dans Application '/'**. Le serveur tente d’accéder à `http://localhost:XXXX/Views/../XXXX.cshtml`

Pour résoudre ce problème, modifiez le **Action de démarrage** définissant dans votre projet pour **Page spécifique**. Vous n’avez pas besoin fournir une valeur pour la page.

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

Après avoir apporté cette modification, en sélectionnant F5 accède à la racine de votre application (`http://localhost:XXXX`). Ce comportement n’est pas le même que le comportement pour les projets MVC 5 dans Visual Studio 2013, où le **Page actuelle** paramètre lance la page ouverte.

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>Tilde(~) et réécriture d’Url

Après la mise à niveau à 3 de Razor ASP.NET ou ASP.NET MVC 5, la notation tilde(~) peut ne plus fonctionner correctement si vous utilisez l’URL réécritures. La réécriture d’URL affecte la notation tilde(~) dans les éléments HTML tels que &lt;A /&gt;, &lt;SCRIPT /&gt;, &lt;lien /&gt;, et par conséquent le tilde mappe n’est plus dans le répertoire racine.

Par exemple, si vous réécrivez les requêtes pour **asp.net/content** à **asp.net**, l’attribut href dans &lt;A href = « ~/content/ » /&gt; se résout en **/content/ contenu /** au lieu de **/**. Pour supprimer cette modification, vous pouvez définir le **IIS\_WasUrlRewritten** contexte sur false dans chaque Page Web ou dans **Application\_BeginRequest** dans Global.asax.

<a id="templateissue"></a>
### <a name="templates"></a>Modèles

Lorsque vous créez ASP.NET MVC projets avec Visual Studio 2012 sur Windows 8.1 ou Windows Server 2012 R2, Visual Studio affiche un message d’erreur indiquant « Configuration Web [url] pour ASP.NET 4.5 a échoué ».

![Erreur de configuration](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

Vous voyez cette erreur, car Visual Studio 2012 n’active pas la fonctionnalité de ASP.NET 4.5 lorsqu’il est installé sur ces versions de Windows. Pour activer ASP.NET 4.5, effectuez les étapes décrites dans [ou désactiver des fonctionnalités Windows activer](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).

![activer ou désactiver des fonctionnalités de Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

Vous pouvez également activer ASP.NET 4.5 via la ligne de commande.

1. Ouvrez l’invite de commandes en mode administrateur.
2. Exécutez la commande suivante pour activer ASP.NET 4.5.  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
