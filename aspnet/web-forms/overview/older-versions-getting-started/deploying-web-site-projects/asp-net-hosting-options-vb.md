---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-vb
title: ASP.NET (VB) d’Options d’hébergement | Microsoft Docs
author: rick-anderson
description: Les applications web ASP.NET sont généralement conçues, créé, testé dans un environnement de développement local et doivent être déployées pour un o d’environnement de production...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 492f5ae2-bad7-4107-89a9-f04a9525dee7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-vb
msc.type: authoredcontent
ms.openlocfilehash: 95df9c7595dc10f2bfce44845236d11457b8b3cb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838383"
---
<a name="aspnet-hosting-options-vb"></a>Options d’hébergement ASP.NET (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_vb.pdf)

> Les applications web ASP.NET sont généralement conçues, créées et testées dans un environnement de développement local et un doivent être déployées dans un environnement de production dès qu’elle sera prête à être publiée. Ce didacticiel fournit une vue d’ensemble du processus de déploiement et sert d’introduction à cette série de didacticiels.


## <a name="introduction"></a>Introduction

Applications Web sont généralement conçues, créées et testées dans un environnement de développement qui est accessible uniquement pour les programmeurs travaillant sur le site. Une fois que l’application est prête à être publiée, elle est déplacée vers un environnement de production où le site est accessible par toute personne sur Internet. Ce processus de déploiement introduit un certain nombre de défis :

- Un environnement de production doit exister et être correctement le programme d’installation avant de pouvoir déployer une application ASP.NET ; en outre, l’environnement de production doit rester à jour avec les derniers correctifs de sécurité.
- Le jeu approprié des fichiers de balisage, les fichiers de code et les fichiers de prise en charge doit être copié à partir de l’environnement de développement à l’environnement de production. Pour les applications pilotées par les données, vous devrez peut-être copier le schéma de base de données et/ou les données, ainsi.
- Il peut exister des différences de configuration entre les deux environnements. Le serveur de chaîne ou un e-mail de connexion de base de données utilisé dans l’environnement de développement sera probablement différent de celle de l’environnement de production. De plus, le comportement de l’application peut dépendre de l’environnement. Par exemple, lorsqu’une erreur se produit dans le développement détails de l’erreur peuvent être affichés à l’écran, mais lorsqu’une erreur se produit en production, une page d’erreur convivial doit être affichée à la place, et les détails d’erreur envoyé par courrier électronique pour les développeurs.

Pour la transmission de la première difficulté - configuration et maintenance d’un environnement de production - nombreuses personnes et entreprises externaliser leurs environnements de production à *web des fournisseurs d’hébergement*. Un fournisseur d’hébergement web est une société qui gère l’environnement de production à votre place. Il existe un nombre incalculable de web des fournisseurs d’hébergement, chacune avec différents prix et les niveaux de service ; consultez la section « Recherche d’un fournisseur d’hébergement Web » pour obtenir des conseils sur la localisation d’un fournisseur de services de ce type.

Il s’agit de la première d’une série de didacticiels qui examinent les étapes impliquées dans le déploiement d’une application web ASP.NET dans un environnement de production gérée par un fournisseur d’hébergement web. Au cours de ces didacticiels, nous allons examiner :

- Les fichiers devant être déployés sur le fournisseur d’hébergement web.
- Outils pour rationaliser le processus de déploiement.
- Comment déployer une base de données.
- Conseils pour le déploiement d’une base de données qui utilise [le fournisseur d’appartenances SQL et les rôles](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md), ainsi que la manière d’imiter l’outil d’Administration de site Web dans un environnement de production.
- Stratégies de mise à jour sans heurts de la base de données en production avec les modifications apportées au cours du développement.
- Techniques pour la journalisation des erreurs qui se produisent sur la production et des méthodes pour avertir les développeurs lorsqu’une erreur se produit.

Ces didacticiels sont conçues pour être concis et fournir des instructions pas à pas avec de nombreuses captures d’écran pour vous guident tout au long du processus visuellement. Ce didacticiel inaugurale fournit une vue d’ensemble du processus de déploiement ASP.NET et des conseils sur la recherche d’un fournisseur d’hébergement web. C’est parti !

## <a name="an-overview-of-the-aspnet-deployment-process"></a>Une vue d’ensemble du processus de déploiement ASP.NET

En bref, le déploiement d’une application ASP.NET implique les trois étapes suivantes :

1. Configurer l’application web, le serveur web et la base de données dans l’environnement de production.
2. Synchroniser les pages ASP.NET, les fichiers de code, les assemblys dans le `Bin` dossier et les fichiers de prise en charge de HTML associés tels que les fichiers CSS et JavaScript.
3. Synchroniser le schéma de base de données et/ou les données.

Les informations de configuration pour une application web se trouve généralement dans le `Web.config` de fichiers et inclut des chaînes de connexion de base de données, critères de la gestion des erreurs, URL de réécriture des règles et les informations de serveur de messagerie. Souvent, ces informations sont différentes pour une application dans le développement par rapport à la même application en production. Par exemple, lorsque vous développez une application, il est préférable d’utiliser une base de données de développement afin que vous ne testez pas sur la base de données de production. Par conséquent, les chaînes de connexion de base de données diffèrent généralement entre les applications de développement et de production. En raison de ces différences, partie de déploiement implique d’apporter des modifications aux informations de configuration de l’application web.

En plus des modifications de configuration d’application web, étape 1 peut également impliquer le configuration pour le serveur web et de la base de données. Par exemple, si une page ASP.NET crée ou supprime des fichiers d’un répertoire sur le serveur web du serveur web doit être configuré pour autoriser ces modifications du système de fichiers. De même, il peut exister des paramètres d’autorisation ou d’authentification qui doivent être apportées à la base de données.


Étape 2 implique la synchronisation de l’ensemble d’essential ASP.NET pages et les fichiers de prise en charge entre les environnements de développement et de production. L’ensemble de ASP. NET-fichiers qui doivent être synchronisées entre les deux environnements varie selon le type de projet créé dans Visual Studio et est la discussion dans le didacticiel suivant,  <em>[déterminer quels fichiers doivent être déployés](determining-what-files-need-to-be-deployed-vb.md)</em>. Les didacticiels troisième et quatrième -  <em>[déploiement de votre Site à l’aide de FTP](deploying-your-site-using-an-ftp-client-vb.md)</em>et <em>[déploiement de votre Site à l’aide de Visual Studio](deploying-your-site-using-visual-studio-vb.md)</em> -examiner différents outils et techniques pour la synchronisation de ces fichiers.

Lors de la création d’applications orientées données, il existe généralement deux bases de données utilisés : un pour le développement et l’autre sur la production. Pendant le développement, schéma de la base de données développement peut être modifié pour inclure les nouvelles tables, colonnes, procédures stockées et déclencheurs, ou peut être modifié pour supprimer ou renommer des objets de base de données existante. Entre le moment où ces modifications sont apportées et l’heure de que l’application est déployée en production, les bases de données de développement et de production ne sont pas synchronisés. Ce comportement asynchrone doit être fixe pendant le processus de déploiement. Ces défis seront examinées dans les didacticiels futures.

## <a name="finding-a-web-host-provider"></a>Recherche d’un fournisseur d’hébergement Web

Les applications ASP.NET peuvent être déployées vers un serveur web qui a le .NET Framework et Internet Information Services (IIS). Vous pouvez héberger un site à partir de votre ordinateur personnel, en supposant que vous aviez une connexion haut débit à Internet et au savoir comment configurer votre routeur pour autoriser les demandes web entrantes. Vous pouvez également héberger un site à partir d’un ordinateur dans un intranet, contrairement à de nombreuses entreprises. L’objectif de ces didacticiels, toutefois, l’hébergement de votre site Web avec un fournisseur d’hébergement web.

> [!NOTE]
> [IIS](https://www.iis.net/) est le serveur web de qualité professionnelle de Microsoft. Il est livré avec les éditions de non-accueil de Windows, tels que Windows Server 2008 et certaines éditions de Windows Vista. Vous n’avez pas besoin d’installer IIS pour servir des applications ASP.NET dans un environnement de développement, car Visual Studio inclut le serveur Web de développement ASP.NET. Toutefois, le serveur Web de développement ASP.NET accepte uniquement les connexions locales et ne peut donc pas être utilisé dans un environnement de production.


Avant de pouvoir déployer votre site à un fournisseur d’hébergement web vous devez d’abord déterminer quelle société commerciale. Il existe un nombre incalculable web hébergeant des entreprises dans la place de marché ; une recherche « société d’hébergement web » renvoie les résultats de plus de cinq millions. Comment trouver celle qui vous convient ? Votre moteur de recherche est un bon point de départ, comme les sites Web tels que [TopHosts](http://www.tophosts.com/) et [HostCritique](http://www.hostcritique.net/), qui de comparer différents services d’hébergement. Je vous conseille également de poser vos collègues et vos collègues pour les recommandations ; Vous pouvez également demander de recommandations à le [hébergement Open Forum](https://forums.asp.net/158.aspx) ici à la [Forums ASP.NET](https://forums.asp.net/).

En général, sociétés d’hébergement Web offrent des plans d’hébergement partagés et dédié de plans d’hébergement. Avec partagé qui héberge un hôte de serveur web unique des dizaines si ce n’est pas des centaines de différents sites Web. Avec un hébergement dédié vous louez un ordinateur à partir de l’entreprise qui sert votre site et votre site autonome. Un plan d’hébergement partagé peut inclure la prise en charge pour les pages ASP.NET, la possibilité de travailler avec les bases de données Microsoft Access, 5 Go d’espace disque et le trafic de la bande passante mensuel de 100 Go pour 9,95 USD par mois. Un autre plan d’hébergement partagé peut inclure la prise en charge pour les pages ASP.NET, l’accès au serveur de base de données Microsoft SQL Server 2008, 10 Go d’espace disque et 250 Go du trafic de la bande passante mensuel pour 19,95 $ par mois. Dédié de plans d’hébergement sont généralement beaucoup plus onéreuse, d’évaluation des coûts plusieurs dollars par mois, mais offre de meilleures performances et davantage de contrôle que partagé options d’hébergement. Quel plan que vous choisissez dépend de votre budget, la quantité de trafic votre site Web reçoit et les fonctionnalités que vous pensez que vous avez besoin.

Deux considérations importantes lors du choix d’un fournisseur d’hébergement web sont le service clientèle et qualité de service. Si vous avez une question ou un problème de configuration, combien de temps faut-il de soumettre votre problème au support technique de l’hôte web jusqu'à ce que vous obtenez une réponse ? La fiabilité sont des services de la société ? Ils ont souvent des pannes de base de données ? La fréquence à laquelle leur serveur de messagerie mis hors connexion ? Vous pouvez toujours demander une société et fournissent des informations sur les temps de fonctionnement et de vous renseigner sur leur stratégie de service client, mais un moyen plus sûr est de solliciter les commentaires des clients antérieurs et actuels, vous pouvez le faire via les forums en ligne, les groupes de discussion et les serveurs de liste .

> [!NOTE]
> Certaines entreprises d’hébergement web concentrent leurs activités sur une pile de technologie particulière, telles que .NET ou [LAMP](http://en.wikipedia.org/wiki/LAMP_stack) (**L** inux :, **A** pache, **M** ysql :, et **P** HP), par conséquent, assurez-vous que la société que vous sélectionnez héberge les applications ASP.NET. Vérifiez également qu’ils prennent en charge la version d’ASP.NET que vous utilisez pour générer votre application. Et si vous générez une application orientée données, assurez-vous que l’hôte web offre le même serveur de base de données et la même version que vous utilisez.


## <a name="summary"></a>Récapitulatif

Les applications web ASP.NET sont généralement conçues, créées et testées dans un environnement de développement local. Une fois qu’une version est prête à être publiée, elle est déplacée vers un environnement de production. Bien qu’il soit possible d’héberger ASP.NET des sites Web sur votre ordinateur personnel ou des serveurs au sein de votre entreprise, de nombreuses entreprises et individus choisissent d’externaliser leur hébergement sur un fournisseur d’hôte web.

Cette série de didacticiels examine les étapes pour déployer une application ASP.NET sur un fournisseur d’hôte web, exploration des défis courants. Ce didacticiel proposé une vue d’ensemble du processus de déploiement d’ASP.NET et a donné des conseils pour trouver un fournisseur d’hôte web approprié. Le didacticiel suivant examine liées à ASP.NET fichiers qui doivent être copiés vers l’environnement de production lors du déploiement de votre site Web.

Bonne programmation !

### <a name="special-thanks-to"></a>Remerciements particuliers à...

Cette série de didacticiels a été révisée par plusieurs réviseurs utiles. Entraîner un réviseur pour ce didacticiel a été Teresa Murphy. Qui souhaitent consulter mes prochains articles MSDN ? Dans ce cas, envoyez-moi une ligne à [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Précédent](users-and-roles-on-the-production-website-cs.md)
> [Suivant](determining-what-files-need-to-be-deployed-vb.md)
