---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Déploiement de Web ASP.NET à l’aide de Visual Studio : déploiement d’une mise à jour du Code | Microsoft Docs'
author: tdykstra
description: Cette série de didacticiels vous montre comment déployer (publier) un ASP.NET web application dans Azure App Service Web Apps ou à un fournisseur d’hébergement tiers, en utilisant des éléments...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: a3d181f3ec8db74781c550720ba4331bdf6478b3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831135"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>Déploiement de Web ASP.NET à l’aide de Visual Studio : déploiement d’une mise à jour du Code
====================
par [Tom Dykstra](https://github.com/tdykstra)

[Télécharger le projet de démarrage](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Cette série de didacticiels vous montre comment déployer (publier) un ASP.NET web application dans Azure App Service Web Apps ou à un fournisseur d’hébergement tiers, à l’aide de Visual Studio 2012 ou Visual Studio 2010. Pour plus d’informations sur la série, consultez [le premier didacticiel de la série](introduction.md).


## <a name="overview"></a>Vue d'ensemble

Après le déploiement initial, votre travail de maintenir et de développer votre site web continue, et avant de longue durée vous souhaitez déployer une mise à jour. Ce didacticiel vous accompagne tout au long du processus de déploiement d’une mise à jour à votre code d’application. La mise à jour que vous implémentez et déployez dans ce didacticiel n’implique pas une modification de la base de données ; Vous verrez en quoi diffère sur le déploiement d’une modification de la base de données dans le didacticiel suivant.

Rappel : Si vous obtenez un message d’erreur ou quelque chose ne fonctionne pas lorsque vous parcourez le didacticiel, veillez à consulter le [page Résolution des problèmes](troubleshooting.md).

## <a name="make-a-code-change"></a>Modifiez le code

Un exemple simple d’une mise à jour à votre application, vous allez ajouter à la **formateurs** page une liste des cours dispensés par le formateur sélectionné.

Si vous exécutez le **formateurs** page, vous remarquerez qu’il n’y **sélectionnez** des liens dans la grille, mais ils ne le faites pas autre chose qu’une marque la couleur grise tour de ligne en arrière-plan.

![Page des formateurs avec sélection](deploying-a-code-update/_static/image1.png)

Maintenant vous ajouterez du code qui s’exécute lorsque le **sélectionnez** lien cliqué et affiche une liste des cours dispensés par le formateur sélectionné.

1. Dans *Instructors.aspx*, ajoutez le balisage suivant immédiatement après le **ErrorMessageLabel** `Label` contrôle :

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. Exécutez la page et sélectionnez un formateur. Vous voyez une liste de cours dispensés par ce formateur.

    ![Page des formateurs avec les cours dispensés](deploying-a-code-update/_static/image2.png)
3. Fermez le navigateur.

## <a name="deploy-the-code-update-to-the-test-environment"></a>Déployer la mise à jour du code dans l’environnement de test

Avant de pouvoir utiliser les profils de publication pour déployer sur le test, intermédiaire et de production, vous devez modifier les options de publication de base de données. Vous n’avez plus besoin d’exécuter les scripts de déploiement grant et des données pour la base de données d’appartenance.

1. Ouvrez le **publier le site Web** Assistant en double-cliquant sur le projet ContosoUniversity en cliquant sur **publier**.
2. Cliquez sur le **Test** Profiler dans le **profil** liste déroulante.
3. Cliquez sur le **paramètres** onglet.
4. Sous **DefaultConnection** dans le **bases de données** section, désactivez le **base de données de mise à jour** case à cocher.
5. Cliquez sur le **profil** onglet, puis cliquez sur le **intermédiaire** Profiler dans le **profil** liste déroulante.
6. Lorsque le système vous demande si vous souhaitez enregistrer les modifications apportées à la **Test** de profil, cliquez sur **Oui**.
7. Apporter la même modification dans le profil de mise en lots.
8. Répétez le processus pour apporter la même modification dans le profil de Production.
9. Fermer le **publier le site Web** Assistant.

Déploiement sur l’environnement de test est maintenant très simple de l’exécution d’un seul clic publier à nouveau. Pour accélérer ce processus, vous pouvez utiliser la **publication Web en un clic** barre d’outils.

1. Dans le **vue** menu, choisissez **barres d’outils** , puis sélectionnez **publication Web en un clic**.

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. Dans **l’Explorateur de solutions**, sélectionnez le projet ContosoUniversity.
3. le **publication Web en un clic** barre d’outils, choisissez le **Test** le profil de publication, puis cliquez sur **publier le site Web** (l’icône des flèches pointant vers la gauche et droite).

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. Visual Studio déploie l’application mis à jour, et le navigateur s’ouvre automatiquement à la page d’accueil.
5. Exécutez la page des formateurs et sélectionnez un formateur pour vérifier que la mise à jour a été déployé avec succès.

Vous le feriez normalement également effectuer des tests de régression (autrement dit, le reste du site pour vous assurer que la nouvelle modification n’arrête toutes les fonctionnalités de test). Mais pour ce didacticiel vous ignorez cette étape et passez à déployer la mise à jour à intermédiaire et de production.

Lorsque vous redéployez, Web Deploy détermine automatiquement les fichiers qui ont été modifiés et seules les copies des fichiers modifiés sur le serveur. Par défaut, Web Deploy utilise des dates de dernière modification sur les fichiers pour déterminer celles qui ont changé. Certains systèmes de contrôle de code source modifier les dates mêmes fichier lorsque vous ne modifiez pas le contenu du fichier. Dans ce cas, vous souhaiterez peut-être configurer Web Deploy pour utiliser les sommes de contrôle pour déterminer quels fichiers ont changé. Pour plus d’informations, consultez [pourquoi tous mes fichiers sont redéployés bien que n’avez pas les modifier ?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) dans le Forum aux questions de déploiement ASP.NET.

## <a name="take-the-application-offline-during-deployment"></a>Mettre l’application hors ligne pendant le déploiement

La modification que vous effectuez un déploiement est maintenant une modification simple à une seule page. Mais parfois vous déployez des modifications majeures, ou vous déployez les modifications de code et de base de données, et le site peut-être se comporter correctement si un utilisateur demande une page avant le déploiement est terminé. Pour empêcher les utilisateurs d’accéder au site pendant le déploiement est en cours, vous pouvez utiliser un *application\_offline.htm* fichier. Lorsque vous placez un fichier nommé *application\_offline.htm* dans le dossier racine de votre application, IIS affiche automatiquement ce fichier au lieu d’exécuter votre application. Pour empêcher l’accès au cours du déploiement, vous placer *application\_offline.htm* dans le dossier racine, exécutez le processus de déploiement et supprimez *application\_offline.htm* après réussite déploiement.

Vous pouvez configurer Web Deploy pour mettre automatiquement une valeur par défaut *application\_offline.htm* sur le serveur de fichiers lorsqu’il démarre le déploiement et le supprimer quand elle est terminée. Pour faire que tout ce que vous avez à faire est d’ajouter l’élément XML suivant à votre fichier de profil (.pubxml) de publication :

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

Pour ce didacticiel, vous verrez comment créer et utiliser un personnalisé *application\_offline.htm* fichier.

À l’aide de *application\_offline.htm* dans le site intermédiaire n’est pas nécessaire, car vous n’avez pas les utilisateurs accédant au site intermédiaire. Mais il est conseillé d’utiliser la mise en lots de tout tester la façon que vous envisagez de déployer en production.

### <a name="create-appofflinehtm"></a>Créer application\_offline.htm

1. Dans **l’Explorateur de solutions**, avec le bouton droit de la solution, puis cliquez sur **ajouter**, puis cliquez sur **un nouvel élément**.
2. Créer un **HTML Page** nommé *application\_offline.htm* (supprimer le dernier « l » dans le *.html* extension Visual Studio crée par défaut).
3. Remplacez le balisage de modèle par le balisage suivant :

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. Enregistrez et fermez le fichier.

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a>Copier l’application\_offline.htm dans le dossier racine du site web

Vous pouvez utiliser n’importe quel outil FTP pour copier des fichiers vers le site web. [FileZilla](http://filezilla-project.org/) est un outil FTP populaire et est indiqué dans les captures d’écran.

Pour utiliser un outil FTP, vous avez besoin de trois choses : l’URL de FTP, le nom d’utilisateur et le mot de passe.

L’URL est affichée sur la page de tableau de bord du site web dans le portail de gestion et le nom d’utilisateur et le mot de passe FTP peuvent être trouvées dans le *.publishsettings* fichier que vous avez téléchargé précédemment. Les étapes suivantes montrent comment obtenir ces valeurs.

1. Dans le portail de gestion Azure, cliquez sur **Sites Web** onglet, puis cliquez sur le site web intermédiaire.
2. Sur le **tableau de bord** page, faites défiler jusqu'à trouver le FTP nom d’hôte dans le **aperçu rapide** section.

    ![Nom d’hôte FTP](deploying-a-code-update/_static/image5.png)
3. Ouvrez la mise en lots *.publishsettings* fichier dans le bloc-notes ou un autre éditeur de texte.
4. Rechercher le `publishProfile` élément pour le profil FTP.
5. Copie le `userName` et `userPWD` valeurs.

    ![Informations d’identification FTP](deploying-a-code-update/_static/image6.png)
6. Ouvrez votre outil FTP et l’ouvrez une session sur l’URL FTP.
7. Copie *application\_offline.htm* à partir du dossier de solution pour le */site/wwwroot* dossier dans le site intermédiaire.

    ![Copie app_offline](deploying-a-code-update/_static/image7.png)
8. Accédez à l’URL de votre site intermédiaire. Vous pouvez constater que le *application\_offline.htm* page est maintenant affichée au lieu de votre page d’accueil.

    ![App_offline.htm dans la fenêtre de navigateur](deploying-a-code-update/_static/image8.png)

Vous êtes maintenant prêt à déployer dans un environnement intermédiaire.

## <a name="deploy-the-code-update-to-staging-and-production"></a>Déployer la mise à jour du code à intermédiaire et de production

1. Dans le **publication Web en un clic** barre d’outils, choisissez le **intermédiaire** le profil de publication, puis cliquez sur **publier le site Web**.

    Visual Studio déploie l’application mis à jour et ouvre le navigateur à la page d’accueil du site. Le *application\_offline.htm* fichier s’affiche. Avant de pouvoir tester pour vérifier la réussite du déploiement, vous devez supprimer le *application\_offline.htm* fichier.
2. Revenir à votre outil FTP et supprimer **application\_offline.htm** à partir du site intermédiaire.
3. Dans le navigateur, ouvrez la page des formateurs dans le site intermédiaire et sélectionnez un formateur pour vérifier que la mise à jour a été déployé avec succès.
4. Suivez la même procédure pour la production comme vous l’avez fait mise en lots.

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>Révision des modifications et le déploiement des fichiers spécifiques

Visual Studio 2012 vous donne également la possibilité de déployer des fichiers individuels. Pour un fichier sélectionné, vous pouvez afficher les différences entre la version locale et la version déployée, déployez le fichier dans l’environnement de destination ou copiez le fichier à partir de l’environnement de destination pour le projet local. Dans cette section du didacticiel, vous allez apprendre à utiliser ces fonctionnalités.

### <a name="make-a-change-to-deploy"></a>Apportez une modification à déployer

1. Ouvrez *Content/Site.css*et rechercher le bloc pour le `body` balise.
2. Modifiez la valeur de `background-color` de `#fff` à `darkblue`.

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>Visualiser les modifications dans la fenêtre d’aperçu de publication

Lorsque vous utilisez le **publier le site Web** Assistant pour publier le projet, vous pouvez voir les modifications qui vont être publiés en double-cliquant sur le fichier dans le **aperçu** fenêtre.

1. Cliquez sur le projet ContosoUniversity et cliquez sur **publier**.
2. À partir de la **profil** liste déroulante, sélectionnez le **Test** le profil de publication.
3. Cliquez sur **aperçu**, puis cliquez sur **démarrer l’aperçu**.
4. Dans le **aperçu** volet, double-cliquez sur **Site.css**.

    ![Double-cliquez sur Site.css](deploying-a-code-update/_static/image9.png)

    Le **aperçu des modifications** boîte de dialogue affiche un aperçu des modifications qui seront déployés.

    ![Aperçu des modifications à Site.css](deploying-a-code-update/_static/image10.png)

    Si vous double-cliquez sur le *Web.config* fichier, le **aperçu des modifications** boîte de dialogue montre l’effet de votre build transformations de configuration et les transformations de profil de publication. À ce stade n’est pas fait tout ce qui entraînerait la *Web.config* fichier sur le serveur à modifier, donc vous devriez ne voir aucune modification. Toutefois, le **aperçu des modifications** fenêtre affiche incorrectement deux modifications. Il semble que les deux éléments XML seront supprimés. Ces éléments sont ajoutés par le processus de publication lorsque vous sélectionnez **Execute Code First Migrations sur le démarrage de l’application** pour une classe de contexte de Code First. La comparaison est effectuée avant que le processus de publication ajoute ces éléments, donc il semble qu’ils sont en cours de suppression bien qu’ils ne seront pas supprimés. Cette erreur sera corrigée dans une version ultérieure.
5. Cliquez sur **Fermer**.
6. Cliquez sur **Publier**.
7. Lorsque le navigateur ouvre la page d’accueil du site de Test, appuyez sur CTRL + F5 pour actualiser dur afin de voir l’effet de la modification CSS.

    ![Effet de changement CSS](deploying-a-code-update/_static/image11.png)
8. Fermez le navigateur.

### <a name="publish-specific-files-from-solution-explorer"></a>Publier des fichiers spécifiques de l’Explorateur de solutions

Supposons que vous ne telles que l’arrière-plan est bleu et souhaitez revenir à la couleur d’origine. Dans cette section, vous devez restaurer les paramètres d’origine en publiant un fichier spécifique directement à partir de **l’Explorateur de solutions**.

1. Ouvrez *Content/Site.css* et restaurer le `background-color` à `#fff`.
2. Dans **l’Explorateur de solutions**, cliquez sur le *Content/Site.css* fichier.

    Le menu contextuel affiche que trois options de publication.

    ![Publier les options à partir de l’Explorateur de solutions](deploying-a-code-update/_static/image12.png)
3. Cliquez sur **aperçu des modifications à Site.css**.

    Une fenêtre s’ouvre pour afficher les différences entre le fichier local et la version de celui-ci dans l’environnement de destination.

    ![Diff-contenu/Site.css](deploying-a-code-update/_static/image13.png)
4. Dans **l’Explorateur de solutions**, avec le bouton droit **Site.css** à nouveau et cliquez sur **Site.css publier**.

    Le **activité de publication Web** fenêtre montre que le fichier a été publié.

    ![Fenêtre d’activité de publication Web](deploying-a-code-update/_static/image14.png)
5. Ouvrez un navigateur à le `http://localhost/contosouniversity` URL et appuyez sur CTRL + F5 pour provoquer un disque dur Actualiser pour voir l’effet de la CSS à modifier.

    ![Page d’accueil avec CSS normal](deploying-a-code-update/_static/image15.png)
6. Fermez le navigateur.

## <a name="summary"></a>Récapitulatif

Vous avez maintenant vu plusieurs façons de déployer une mise à jour d’application qui n’implique pas une modification de la base de données, et que vous avez vu comment prévisualiser les modifications pour vérifier que ce qui sera mis à jour est ce que vous attendez. La page des formateurs a maintenant un **cours enseignés** section.

![Page des formateurs avec les cours dispensés](deploying-a-code-update/_static/image16.png)

Le didacticiel suivant vous montre comment déployer une modification de la base de données : vous allez ajouter un champ de date de naissance à la base de données et à la page des formateurs.

> [!div class="step-by-step"]
> [Précédent](deploying-to-production.md)
> [Suivant](deploying-a-database-update.md)
