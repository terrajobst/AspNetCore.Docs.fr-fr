---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
title: Déploiement de votre Site à l’aide de Visual Studio (VB) | Microsoft Docs
author: rick-anderson
description: Visual Studio inclut des outils pour le déploiement d’un site Web. En savoir plus sur ces outils dans ce didacticiel.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 977105f3-7987-4e50-8be7-afb53b4ca28a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
msc.type: authoredcontent
ms.openlocfilehash: 127f051d9e9e2e4bd5b180185846ebd67586f41d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402679"
---
<a name="deploying-your-site-using-visual-studio-vb"></a>Déploiement de votre Site à l’aide de Visual Studio (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_VB.zip) ou [télécharger le PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_vb.pdf)

> Visual Studio inclut des outils pour le déploiement d’un site Web. En savoir plus sur ces outils dans ce didacticiel.


## <a name="introduction"></a>Introduction

Le didacticiel précédent examiné comment déployer une application de web ASP.NET simple pour un fournisseur d’hébergement web. Plus précisément, le didacticiel vous a montré comment utiliser un client FTP comme FileZilla pour transférer les fichiers nécessaires à partir de l’environnement de développement à l’environnement de production. Visual Studio offre également des outils intégrés pour faciliter le déploiement sur un fournisseur d’hôte web. Ce didacticiel examine deux de ces outils : l’outil Copier le Site Web, où vous pouvez déplacer les fichiers vers et depuis un serveur web à distance à l’aide de FTP ou les Extensions serveur FrontPage ; et l’outil de publication, qui copie tout le site Web vers un emplacement spécifié.


> [!NOTE]
> Autres outils liées au déploiement proposées par Visual Studio incluent [projets d’installation Web](https://msdn.microsoft.com/library/wx3b589t.aspx) et [Web Deployment Projects](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) Add-In. Projets d’installation Web empaqueter le contenu d’un site Web et les informations de configuration dans un seul fichier MSI. Cette option est particulièrement utile pour les sites Web déployés au sein d’un intranet ou pour les entreprises qui vendent une application web prépackagés qui installer des clients sur leurs propres serveurs web. Le déploiement de projets complément est qu'un Visual Studio Add-In qui facilite la spécification des différences de configuration entre les versions pour les environnements de développement et les environnements de production. Projets d’installation Web ne sont pas abordées dans cette série de didacticiels ; Projets de déploiement Web sont résumées dans le [ *différences entre développement Configuration commun et Production* ](common-configuration-differences-between-development-and-production-vb.md) didacticiel.


## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>Déploiement de votre Site à l’aide de l’outil Copier le Site Web

Outil de copier le Site Web de Visual Studio est des fonctionnalités similaires à un client FTP autonome. En bref, l’outil Copier le Site Web vous permet de se connecter à un site web à distance via FTP ou les Extensions serveur FrontPage. Comme pour l’interface utilisateur de FileZilla, l’interface utilisateur de copier le Site Web se compose de deux volets : le volet gauche répertorie les fichiers locaux, tandis que le volet droit répertorie ces fichiers sur le serveur de destination.

> [!NOTE]
> L’outil Copier le Site Web est uniquement disponible pour les projets de Site Web. Visual Studio offre cet outil lorsque vous travaillez avec un projet d’Application Web.


Jetons un œil à l’utilisation de l’outil Copier le Site Web pour publier l’application critique de livre en production. Étant donné que l’outil Copier le Site Web fonctionne uniquement avec les projets qui utilisent le modèle de projet de Site Web que nous pouvons examiner uniquement à l’aide de cet outil avec le projet BookReviewsWSP. Ouvrir ce projet.

Lancer le projet d’outil Copier le Site Web en cliquant sur l’icône Copier le Site Web dans l’Explorateur de solutions (cette icône est encerclée dans la Figure 1) ; ou bien, vous pouvez sélectionner l’option Copier le Site Web dans le menu du site Web. Chacune de ces approches lance l’interface utilisateur de copier le Site Web indiqué dans la Figure 1 ; seul le volet de gauche dans la Figure 1 est rempli, car nous devons encore se connecter à un serveur distant.


[![Interface utilisateur de l’outil Copier le Site Web est divisée en deux volets.](deploying-your-site-using-visual-studio-vb/_static/image2.png)](deploying-your-site-using-visual-studio-vb/_static/image1.png)

**Figure 1**: Interface utilisateur de l’outil de la copie Site Web est divisée en deux volets ([cliquez pour afficher l’image en taille réelle](deploying-your-site-using-visual-studio-vb/_static/image3.png))


Pour déployer notre site, nous devons d’abord vous connecter au fournisseur de serveur web. Cliquez sur le bouton de connexion en haut de l’interface utilisateur de copier le Site Web. Cela affiche la boîte de dialogue Ouvrir le Site Web indiquée dans la Figure 2.

Vous pouvez vous connecter au site Web de destination en sélectionnant une des quatre options de la gauche :

- **Système de fichiers** -Sélectionnez cette option pour déployer votre site à un dossier ou un partage réseau accessible à partir de votre ordinateur.
- **Serveur IIS local** -Utilisez cette option pour déployer le site sur le serveur web IIS installé sur votre ordinateur.
- **FTP Site** -se connecter à un site web à distance à l’aide de FTP.
- **Site distant** -se connecter à un site Web distant, à l’aide des Extensions serveur FrontPage.

La plupart des fournisseurs d’hébergement web prennent en charge FTP, mais moins offrent la prise en charge des extensions serveur FrontPage. Pour cette raison, j’ai sélectionné l’option de FTP Site et ensuite entré les informations de connexion comme indiqué dans la Figure 2.


[![Spécifier le site Web de Destination](deploying-your-site-using-visual-studio-vb/_static/image5.png)](deploying-your-site-using-visual-studio-vb/_static/image4.png)

**Figure 2**: spécifiez le site Web de Destination ([cliquez pour afficher l’image en taille réelle](deploying-your-site-using-visual-studio-vb/_static/image6.png))


Après vous être connecté, l’outil Copier le Site Web charge les fichiers sur le site distant dans le volet droit et indique l’état de chaque fichier : nouveaux, supprimés, modifiés ou Unchanged. Vous pouvez copier un fichier à partir du site local vers le site distant, ou inversement a.

Nous allons ajouter une nouvelle page au projet BookReviewsWSP, puis le déployer afin que nous pouvons voir l’outil Copier le Site Web en action. Créer une nouvelle page ASP.NET dans Visual Studio dans le répertoire racine nommé `Privacy.aspx`. Que la page à utiliser la page maître `Site.master` et ajoutez la déclaration de confidentialité de votre site à cette page. Figure 3 montre Visual Studio après que cette page a été créée.


[![Ajouter une Page nommée &lt;code&gt;Privacy.aspx&lt;/code&gt; au dossier de racine du site Web](deploying-your-site-using-visual-studio-vb/_static/image8.png)](deploying-your-site-using-visual-studio-vb/_static/image7.png)

**Figure 3**: ajouter une Page nommée `Privacy.aspx` au dossier racine du site Web de ([cliquez pour afficher l’image en taille réelle](deploying-your-site-using-visual-studio-vb/_static/image9.png))


Revenez ensuite à l’interface utilisateur de copier le Site Web. Comme le montre la Figure 4, le volet gauche inclut désormais les nouveaux fichiers - `Policy.aspx` et `Policy.aspx.vb`. De plus, ces fichiers sont marqués avec une icône de flèche et un état de nouveau, indiquant qu’elles existent sur le site local, mais pas sur le site distant.


[![L’outil Copier le Site Web inclut la nouveau &lt;code&gt;Privacy.aspx&lt;/code&gt; Page dans le volet de gauche](deploying-your-site-using-visual-studio-vb/_static/image11.png)](deploying-your-site-using-visual-studio-vb/_static/image10.png)

**Figure 4**: l’outil Copier le Site Web inclut la nouveau `Privacy.aspx` Page dans le volet de gauche ([cliquez pour afficher l’image en taille réelle](deploying-your-site-using-visual-studio-vb/_static/image12.png))


Pour déployer les nouveaux fichiers, sélectionnez-les et puis cliquez sur l’icône de flèche pour les transférer vers le site distant. Une fois le transfert terminé le `Policy.aspx` et `Policy.aspx.vb` fichiers existent sur les sites locaux et distants avec l’état Unchanged.

En même temps que la liste de nouveaux fichiers, l’outil Copier le Site Web met en surbrillance tous les fichiers qui diffèrent entre les sites locaux et distants. Pour voir comment cela fonctionne, revenez à la `Privacy.aspx` page et ajouter quelques mots à la politique de confidentialité. Enregistrez la page, puis revenez à l’outil Copier le Site Web. Comme le montre la Figure 5, le `Privacy.aspx` page dans le volet de gauche a le statut Changed indiquant qu’il est désynchronisé avec le site distant.


[![L’outil Copier le Site Web indique que le &lt;code&gt;Privacy.aspx&lt;/code&gt; Page a été modifiée.](deploying-your-site-using-visual-studio-vb/_static/image14.png)](deploying-your-site-using-visual-studio-vb/_static/image13.png)

**Figure 5**: l’outil Copier le Site Web indique que le `Privacy.aspx` Page a été modifiée ([cliquez pour afficher l’image en taille réelle](deploying-your-site-using-visual-studio-vb/_static/image15.png))


L’outil Copier le Site Web indique également si un fichier a été supprimé depuis la dernière opération de copie. Supprimer le `Privacy.aspx` à partir du projet local et l’actualisation de l’outil Copier le Site Web. Le `Privacy.aspx` et `Privacy.aspx.vb` les fichiers sont toujours répertoriées dans le volet gauche, mais présentent l’état Deleted indiquant qu’ils ont été supprimés depuis la dernière opération de copie.

## <a name="publishing-a-web-application"></a>Publication d’une Application Web

Une autre façon de déployer votre application web à partir de Visual Studio consiste à utiliser l’option de publication, qui est accessible via le menu Générer. L’option Publier explicitement compile l’application, puis copie tous les fichiers nécessaires jusqu’au site distant spécifié. Comme nous le verrons bientôt, l’option Publier n’est plus évident que l’outil Copier le Site Web. Tandis que l’outil Copier le Site Web vous permet d’examiner les fichiers sur les sites locaux et distants et vous permet de charger ou télécharger des fichiers individuels en fonction des besoins, l’option Publier déploie l’application web tout entier.


En plus de la copie de tous les fichiers nécessaires sur le site distant spécifié, l’option Publier compile également explicitement l’application. Étant donné que les projets d’Application Web doivent être compilées explicitement celle-ci doit provenir pas étonnant que l’option de publication est disponible pour les projets d’Application Web. Ce qui peut être un peu surprenant, c’est que l’option de publication est également disponible pour les projets de Site Web. Comme indiqué dans le [ *déterminer quels fichiers doivent être déployés* ](determining-what-files-need-to-be-deployed-vb.md) didacticiel, les projets de Site Web peut être compilée explicitement via un processus appelé *précompilation*. Ce didacticiel se concentre sur l’utilisation de l’option publier avec les projets d’Application Web ; un futur didacticiel explorera la précompilation, après quoi, nous allons retourner pour examiner l’utilisation de l’option publier avec les projets de Site Web.

> [!NOTE]
> Alors que l’option de publication est disponible dans Visual Studio pour les projets de Site Web et projets d’Application Web, Visual Web Developer offre uniquement la possibilité de publier pour les projets d’Application Web.


Examinons à présent déployer l’application de critiques de livres à l’aide de l’option Publier. Commencez par ouvrir BookReviewsWAP (le projet d’Application Web) dans Visual Studio. Dans le menu Publier choisir le projet de BookReviewsWAP générer. Ceci fait apparaître une boîte de dialogue vous invite à entrer pour l’emplacement cible, entre autres options de configuration (voir Figure 6). Bien, comme avec l’outil Copier le Site Web vous pouvez entrer un emplacement qui pointe vers un dossier local, un site Web local sur IIS, un site Web à distance qui prend en charge les Extensions serveur FrontPage, ou une adresse de serveur FTP. Vous pouvez choisir s’il faut remplacer les fichiers sur le serveur web à distance avec les fichiers déployés ou supprimer tout le contenu sur le site distant avant la publication. Vous pouvez également spécifier s’il faut copier :

- Seuls les fichiers dans le projet nécessaires pour exécuter l’application, qui omet le code source inutiles et des fichiers associés au projet.
- Tous les fichiers de projet, qui inclut les fichiers de code source et les fichiers de projet Visual Studio telles que le fichier Solution.
- Tous les fichiers dans le dossier source du projet, qui copie tous les fichiers dans le dossier source du projet indépendamment de si ils sont inclus dans le projet.

Il existe également une option permettant de charger le contenu de la `App_Data` dossier.


[![Spécifier le site Web de Destination](deploying-your-site-using-visual-studio-vb/_static/image17.png)](deploying-your-site-using-visual-studio-vb/_static/image16.png)

**Figure 6**: spécifiez le site Web de Destination ([cliquez pour afficher l’image en taille réelle](deploying-your-site-using-visual-studio-vb/_static/image18.png))


Pour l’application critique de livre le site distant contient les fichiers déployés lors de la copie du projet BookReviewsWSP via l’outil Copier le Site Web. Nous allons donc l’option Publier commencer par supprimer tout le contenu existant. En outre, nous allons simplement copier les fichiers nécessaires au lieu d’encombrer l’environnement de production avec des fichiers de projet et de code source inutiles. Après la spécification de ces options, cliquez sur le bouton Publier. Sur les prochaines secondes Visual Studio déploiera les fichiers nécessaires au site de destination, affichant sa progression dans la fenêtre de sortie.

La figure 7 illustre les fichiers sur le site FTP après que l’opération de publication est terminée. Notez que seules les pages de balisage et les fichiers de prise en charge nécessaire du serveur et côté client ont été téléchargés.


[![Uniquement les fichiers nécessaires ont été publiés dans l’environnement de Production](deploying-your-site-using-visual-studio-vb/_static/image20.png)](deploying-your-site-using-visual-studio-vb/_static/image19.png)

**Figure 7**: uniquement le nécessaire fichiers ont été publiés dans l’environnement de Production ([cliquez pour afficher l’image en taille réelle](deploying-your-site-using-visual-studio-vb/_static/image21.png))


L’option de publication est un outil moins subtils que l’outil Copier le Site Web. Tandis que l’outil Copier le Site Web vous permet d’inspecter les fichiers sur les sites locaux et distants et de voir à quoi ils diffèrent, l’option Publier ne fournit aucun interface. En outre, l’outil Copier le Site Web vous permet d’apporter des modifications uniques téléchargement ou la suppression des fichiers individuels. L’option Publier n’autorise pas un tel contrôle précis ; au lieu de cela, elle publie le *entière* application. Ce comportement a ses avantages et inconvénients. De plus, vous savez lorsque vous utilisez l’option de publication que vous ne sont pas oublier de charger un fichier important. Considérez que se passe-t-il que si vous avez apporté une petite modification à un site Web très volumineux, avec l’option Publier vous ne pouvez pas mettre à jour cette page ou deux qui a été modifié, mais au lieu de cela, vous devez attendre pendant que Visual Studio déploie l’ensemble du site.

Il n’est pas rare pour permettre de certains fichiers dont le contenu diffère entre les environnements de développement et de production. Un exemple de clé est le fichier de configuration de l’application, `Web.config`. Étant donné que l’option de publication copie les fichiers d’application web il remplace les fichiers de configuration personnalisé de l’environnement de production avec la version dans l’environnement de développement. Le didacticiel suivant explore davantage cette rubrique et propose des conseils pour le déploiement d’une application web lorsqu’il existent de telles différences.

## <a name="summary"></a>Récapitulatif

Déploiement d’un site Web implique de copier les fichiers nécessaires à partir de l’environnement de développement à l’environnement de production. Le didacticiel précédent a montré comment faire pour transférer des fichiers à l’aide d’un client FTP comme FileZilla. Ce didacticiel examiné deux outils de déploiement dans Visual Studio : l’outil Copier le Site Web et l’option Publier. L’outil Copier le Site Web est similaire à un client FTP doté d’une interface à deux volets affichant les fichiers sur l’ordinateur local et un ordinateur distant spécifié qui rend plus facile de télécharger des fichiers entre les deux ordinateurs. L’option Publier est un outil plus pointu explicitement compile le projet et déploie ensuite l’application entière vers la destination spécifiée.

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Copie de Site Web avec l’outil Copier le Site Web](https://msdn.microsoft.com/library/1cc82atw.aspx)
- [Comment faire : déployer un Site Web à l’aide de l’outil Copier le Site Web](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md) (vidéo)
- [Comment : Publier des projets d’Application Web](https://msdn.microsoft.com/library/aa983453.aspx)
- [Comment : Publier des Sites Web](https://msdn.microsoft.com/library/20yh9f1b.aspx)
- [Le programme d’installation et déploiement des projets dans Visual Studio](https://msdn.microsoft.com/library/wx3b589t.aspx)

> [!div class="step-by-step"]
> [Précédent](deploying-your-site-using-an-ftp-client-vb.md)
> [Suivant](common-configuration-differences-between-development-and-production-vb.md)
