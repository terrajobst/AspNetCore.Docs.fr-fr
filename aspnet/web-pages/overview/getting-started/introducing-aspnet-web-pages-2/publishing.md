---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: 'Présentation des Pages Web ASP.NET : publication d’un Site à l’aide de WebMatrix | Microsoft Docs'
author: tfitzmac
description: Ce didacticiel est le dernier volet de l’ensemble de didacticiels qui présente ASP.NET Web Pages et WebMatrix de Microsoft. Il explique comment publier votre site t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: c6fa7692527b7aa65e93cd57ed5bd56f42e54bd6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373485"
---
<a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a>Présentation des Pages Web ASP.NET - publication d’un Site à l’aide de WebMatrix
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Ce didacticiel est le dernier volet de l’ensemble de didacticiels qui présente ASP.NET Web Pages et WebMatrix de Microsoft. Il explique comment publier votre site sur Internet afin que d’autres peuvent utiliser. Il part du principe que vous avez terminé la série via [création d’une zone de recherche cohérents pour les Sites ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251585).
> 
> Vous allez apprendre à publier votre site à l’aide de :
> 
> - Microsoft Azure
> - Société d’hébergement Web


## <a name="about-publishing-your-site"></a>Sur la publication de votre Site

Jusqu'à présent, vous avez fait tout votre travail sur un ordinateur local, notamment le test de vos pages. Pour exécuter votre<em>.cshtml</em> pages, vous avez utilisé le serveur web qui est intégré à WebMatrix, à savoir IIS Express. Mais bien sûr, aucune autre peut voir le site que vous avez créé à l’exception de vous. Pour d’autres permettent de travailler avec votre site, vous devez le publier sur Internet.

Sauf si vous avez déjà accès à un serveur web public, publication signifie que vous devez disposer d’un compte avec un *plateforme cloud* ou un *fournisseur d’hébergement*. Une plateforme cloud, tels que Microsoft Azure, fournit une infrastructure de la demande pour vos applications. Un fournisseur d’hébergement est une société qui possède des serveurs web accessible publiquement et qui sera louer vous espace pour votre site. Hébergement des plans d’exécution à partir de quelques dollars par mois (ou même gratuits) pour les sites de petite taille à plusieurs centaines de dollars par mois pour les sites Web commerciaux de haut volume.

> [!NOTE]
> Vous aurez peut-être accès à un serveur web publique via le fournisseur de services internet (ISP) qui vous permet d’obtenir le service internet à la maison. Toutefois, votre fournisseur d’hébergement doit prendre en charge les Pages Web ASP.NET. De nombreux ISP ne, mais il est toujours intéressant de vérification.


Dans ce didacticiel, nous allons vous donner une vue d’ensemble de la publication. Il n’est pas pratique fournir des détails précis pour tout, étant donné que le processus diffère légèrement pour chaque fournisseur d’hébergement. Mais, vous obtiendrez une bonne idée du fonctionnement du processus.

Ce didacticiel contient quatre sections :

1. [Configuration de la page par défaut](#defaultpage)
2. Publication (choisissez une des opérations suivantes)  
 a. [Publication de votre Site vers Microsoft Azure](#azure)  
 b. [Publication de votre Site pour une société d’hébergement Web](#host)
3. [La mise à jour le Site en direct : republication](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a>Configuration de la page par défaut

Lorsqu’un utilisateur accède à l’adresse de base pour votre site web, la page par défaut pour votre site s’affiche à l’utilisateur. Par exemple, lorsque Default.htm est définie en tant que la page par défaut pour le site à www.contoso.com, puis accédez à <strong>www.contoso.com</strong> est identique à la navigation vers <strong>www.contoso.com/Default.htm</strong>.

Actuellement, votre site utilise **Default.cshtml** en tant que la page par défaut. Cette page est parfaite pour votre page par défaut, mais dans ce didacticiel vous n’avez ajouté aucun contenu à cette page afin de s’afficher une page vierge. Ouvrez Default.cshtml et remplacez le contenu par le code suivant.

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

Votre site est maintenant prêt pour la publication. Tout d’abord, vous verrez comment déployer le site vers Azure, puis comment le déployer pour une société d’hébergement web. L’option fonctionne pour votre site web et vous ne devez suivre une des options de déploiement.

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a>Publication de votre Site vers Microsoft Azure

Ce didacticiel vous indiquera tout d’abord comment déployer votre site sur Microsoft Azure. En vous connectant avec un compte Microsoft, vous pouvez créer jusqu'à 10 sites gratuits sur Azure. Ces sites gratuits fournissent un moyen pratique pour tester vos sites. Vous pouvez toujours supprimer ce site exemple ultérieurement pour éviter d’utiliser tous vos sites gratuits. Vous pouvez créer un compte d’essai gratuit en quelques minutes. Pour plus d’informations, consultez [version d’évaluation gratuite Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Dans le ruban de WebMatrix, cliquez sur le **publier** bouton.

!['Publish' bouton dans le ruban de WebMatrix](publishing/_static/image1.png)

Le **publier votre Site** boîte de dialogue s’affiche. Si vous n’avez pas connecté à votre compte Microsoft, la boîte de dialogue contiendra une **prise en main Azure** lien. Cliquez sur ce lien.

![Publiez votre site](publishing/_static/image2.png)

Si vous n’avez pas connecté à un compte Microsoft, vous obtenez à nouveau la possibilité de se connecter. Vous devez vous connecter à un compte Microsoft pour publier votre site sur Azure.

![Se connecter](publishing/_static/image3.png)

Après vous être connecté à votre compte Microsoft, la boîte de dialogue contient des liens pour créer un nouveau site sur Azure ou vous connecter à un de vos sites existants sur Azure.

![Créer un nouveau Site](publishing/_static/image4.png)

Sélectionnez **créer un nouveau site**.

Si vous avez nommé votre projet **WebPagesMovies**, le nom par défaut pour votre site sera **webpagesmovies.azurewebsites.net**. Ce nom par défaut est probablement pas disponible, comme indiqué par le point d’exclamation rouge.

![nom du site Web par défaut](publishing/_static/image5.png)

Modifier le nom du site à un élément qui est disponible, puis sélectionnez un emplacement qui est proche de votre emplacement.

![nom du site modifié](publishing/_static/image6.png)

Cliquez sur **OK**.

WebMatrix performss un test pour déterminer si le serveur est compatible avec votre site.

![test de compatibilité](publishing/_static/image7.png)

Sélectionnez **continuer**.

Les résultats du test de compatibilité sont affichés.

![résultat de la compatibilité](publishing/_static/image8.png)

Sélectionnez **continuer**.

WebMatrix affiche les fichiers et les bases de données qui seront publiés sur le site. Dans la mesure où il s’agit de la première fois que vous publiez le site, tous les fichiers sont répertoriés. Vous pouvez désactiver un fichier qui n’est pas prêt à être publié. Dans les publications ultérieures, seuls les fichiers qui ont été modifiés seront affichera. Consultez [la mise à jour le Site en direct : republication](#update).

![Aperçu de la publication](publishing/_static/image9.png)

Sélectionnez **continuer**.

Une fois que le site a été déployé sur Azure, un message s’affiche indiquant que le déploiement terminé.

![publication terminée](publishing/_static/image10.png)

Votre site et la base de données ont été publiés sur Azure et sont désormais disponibles au public. Cliquez sur le lien dans le message indiquant la fin de la publication, et vous voyez maintenant votre site déployé. Vous ou toute personne ayant accès à Internet peut ajouter ou modifier des enregistrements dans la base de données.

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a>Publication de votre Site pour une société d’hébergement Web

Si vous décidez de ne pas publier sur Azure, vous pouvez à la place publier votre site pour une société d’hébergement web.

Cliquez sur le **rechercher un hébergement web** lien.

![Bouton de « Rechercher un hébergement web » dans la boîte de dialogue Paramètres de publication](publishing/_static/image12.png)

Vous accédez à une page sur le site de Microsoft qui répertorie les fournisseurs d’hébergement qui prennent en charge ASP.NET.

![Page sur le site de Microsoft qui répertorie les fournisseurs d’hébergement](publishing/_static/image13.png)

Évidemment, il peut être difficile de savoir exactement quelles fonctionnalités d’hébergement que vous pouvez avoir besoin sur le long terme. Voici quelques éléments à prendre en compte :

- Dans le cadre du site WebPagesMovies, vous n’êtes pas obligé de disposer d’un module complémentaire distinct pour SQL Server, qui souvent les coûts supplémentaires. Dans votre site, vous êtes à l’aide de SQL Server Compact Edition, qui est autonome. Toutefois, vous devrez peut-être accès à SQL Server pour un travail de site Web futures que procéder. Si vous pensez que vous pouvez, assurez-vous que vous pouvez ajouter ultérieurement des capacités de SQL Server.
- Vérifiez si le fournisseur d’hébergement prend en charge le protocole de publication Web Deploy. Vous pouvez publier à l’aide du protocole FTP, mais il est plus pratique d’utiliser Web Deploy.

Certains sites proposent une période d’essai gratuite. Une version d’évaluation gratuite est un bon moyen de tenter de publier et d’hébergement pendant que vous êtes toujours expérimenter WebMatrix et ASP.NET Web Pages.

Choisissez une qui vous convient. Pour ce didacticiel, nous avons sélectionné DiscountASP.NET, car bien que nous étions en train de créer le didacticiel, cette société avait une promotion permettant d’héberger un site gratuit pendant quelques mois.

> [!NOTE]
> Notre choix d’un fournisseur d’hébergement pour ce didacticiel ne doit pas être interprété comme une approbation de cette société plutôt que les autres. Mais nous avons dû choisir un à titre d’illustration et DiscountASP.NET est une des nombreuses sociétés qui prend en charge des Pages Web ASP.NET et le protocole Web Deploy pour la publication.


En règle générale, une fois que vous avez inscrit avec le fournisseur d’hébergement, la société vous envoie un e-mail contenant un nom d’utilisateur et le mot de passe, l’URL du serveur de web et ainsi de suite. Si la société d’hébergement prend en charge le protocole Web Deploy, ils peuvent envoyer vous un fichier qui contient les paramètres de publication, ou vous permettent de télécharger un. Un fichier de paramètres de publication simplifie le processus pour vous.

Lorsque vous êtes inscrit et que vous êtes prêt à publier, cliquez sur le **publier** bouton dans le ruban de WebMatrix. Le **paramètres de publication** boîte de dialogue s’affiche.

Si le fournisseur d’hébergement vous avons envoyé un fichier de paramètres de publication, cliquez sur le **importer les paramètres de publication** lier, puis importez le fichier. Si vous n’avez pas un fichier de paramètres de publication, renseignez les champs en utilisant les valeurs de la société d’hébergement vous avons envoyé par courrier électronique. Voici à quoi le **paramètres de publication** boîte de dialogue peut se présenter comme lorsque vous avez terminé :

![Renseignez les paramètres de publication dans la boîte de dialogue Publier les paramètres](publishing/_static/image14.png)

Cliquez sur **valider la connexion**. Si tout est OK, la boîte de dialogue indique **connecté avec succès**, ce qui signifie qu’il peut communiquer avec le serveur d’hébergement du fournisseur.

![Message de réussite si publier les paramètres sont corrects](publishing/_static/image15.png)

S’il existe un problème, WebMatrix fait de son mieux pour vous indiquer l’origine du problème :

![Message d’erreur s’il existe un problème avec les paramètres de publication](publishing/_static/image16.png)

Cliquez sur **enregistrer** pour enregistrer vos paramètres. WebMatrix propose effectuer un test pour vous assurer qu’il peut communiquer correctement avec le site d’hébergement :

![Message offre pour effectuer un test du processus de publication](publishing/_static/image17.png)

Cliquez sur **Oui**. WebMatrix télécharge les fichiers d’exemple au fournisseur d’hébergement. Lorsque le test de compatibilité est effectué, WebMatrix signale les résultats :

![Résultats du test de publication](publishing/_static/image18.png)

Si vous êtes prêt à l’emploi, continuez et cliquez sur **continuer** pour démarrer le processus de publication réellement. WebMatrix détermine quels fichiers sont dans votre site et se trouvent déjà sur le serveur hôte (maintenant, aucun), puis vous donne un aperçu du processus de publication :

![Aperçu des fichiers à télécharger le processus de publication](publishing/_static/image19.png)

La liste des fichiers à publier inclut les pages web que vous avez créée comme *Movies.cshtml*. La liste inclut également des fichiers pour les programmes d’assistance que vous avez installé, les fichiers pour exécuter SQL Server Compact Edition pour votre base de données et ainsi de suite. Par conséquent, les processus de publication d’initial peut être importante.

Cliquez sur **Continuer**. WebMatrix copie vos fichiers au serveur d’hébergement du fournisseur. Une fois terminé, les résultats sont signalés dans la barre d’état :

![Message de barre d’état lorsque le processus de publication terminée avec succès](publishing/_static/image20.png)

Pour afficher votre site dynamique, cliquez sur le lien dans la barre d’état. Ajouter *films* à l’URL, et vous verrez la *Movies.cshtml* fichier que vous avez créé :

![Le site en direct affichant la page de films](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a>La mise à jour le Site en direct : republication

Une fois que vous avez publié votre site (vers Azure ou une société d’hébergement web), il existe deux copies de ce dernier &mdash; la version sur votre ordinateur et la version sur le fournisseur de services. Vous souhaiterez probablement continuer à développer le site (si rien d’autre, dans le cadre de l’ensemble du didacticiel suivant). Lorsque vous le faites, vous devez republier votre site pour copier les modifications depuis votre ordinateur au fournisseur de services. Le processus de publication dans WebMatrix peut déterminer ce que les fichiers ont été modifiés sur votre site et de publier uniquement les fichiers.

Pour voir comment fonctionnement la republication, ouvrez le *Movies.cshtml* de site, apportez une petite modification, puis enregistrez le fichier. Par exemple, remplacez le titre par `Movies - Updated`.

Cliquez sur le **publier** bouton du ruban. WebMatrix détermine ce qui est modifié et affiche un aperçu des fichiers qu'il publie.

![La boîte de dialogue « Publier » montrant modifiées fichiers prêts à être republication](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> Par défaut, WebMatrix publie votre base de données (*.sdf* fichier) uniquement la première fois que vous publierez le site. Une fois votre site est publié et les personnes interagissent avec le site Web, la base de données sur le site a en général, les données du site réel. Vous devez être très attention à ne pas remplacer la base de données en direct avec le *.sdf* fichier qui se trouve sur votre ordinateur, qui contient généralement des données de test uniquement. C’est pourquoi vous voyez l’avertissement **publication remplacera les bases de données distantes**, et pourquoi la case à cocher *WebPagesMovies.sdf* est désactivée par défaut.


Cliquez sur **Continuer**. WebMatrix publie les fichiers modifiés et affiche un message de réussite, semblable à celle de la première fois que vous avez publié.

Accédez au site en direct (vous pouvez cliquer sur le lien dans le message de réussite si elle n’est toujours affichée) et vérifiez que votre modification a été publiée.

> [!TIP] 
> 
> **Modification des fichiers à distance**
> 
> Comme alternative à la modification de votre site, puis republication, vous pouvez modifier les fichiers distants directement dans WebMatrix. Dans ce scénario, vous ouvrez un fichier qui se trouve sur le fournisseur de services et WebMatrix télécharge une copie de celle-ci, vous pouvez modifier. Chaque fois que vous enregistrez le fichier, WebMatrix envoie les modifications apportées au site.
> 
> Modifier à distance est un moyen simple pour apporter des modifications à votre site dynamique. Toutefois, les modifications de cette façon ne sont pas synchronisées avec les fichiers dans votre site local. Pour synchroniser les fichiers locaux avec le site distant, vous pouvez télécharger les fichiers à distance. Ce processus fonctionne comme la publication, à l’exception dans l’ordre inverse.
> 
> Nous ne décrirons plus sur les mode de modification à distance et à distance-téléchargement installations de WebMatrix ici. Elles sont particulièrement utiles si plusieurs personnes de travailler sur le même site sur des ordinateurs différents. Pour plus d’informations, consultez [publier et modifier un Site distant avec la version bêta 2 de WebMatrix](https://go.microsoft.com/fwlink/?LinkId=251591).


## <a name="additional-resources"></a>Ressources supplémentaires

- [Forum de WebMatrix ASP.NET Web Pages ASP.NET](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), un endroit idéal pour poser des questions et obtenez des réponses.

> [!div class="step-by-step"]
> [Précédent](layouts.md)
