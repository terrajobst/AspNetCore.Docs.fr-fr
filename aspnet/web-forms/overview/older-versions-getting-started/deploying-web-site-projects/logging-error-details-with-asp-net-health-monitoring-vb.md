---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
title: Journalisation des détails de l’erreur avec l’intégrité ASP.NET (VB) de surveillance | Microsoft Docs
author: rick-anderson
description: Système de surveillance de l’intégrité de Microsoft fournit un moyen facile et personnalisable pour vous connecter divers événements web, y compris les exceptions non gérées. Ce didacticiel décrit le transfert...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 09a6c74e-936a-4c04-8547-5bb313a4e4a3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
msc.type: authoredcontent
ms.openlocfilehash: 1ed7b63989dc6ea7e46210a45612e2672a662177
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382470"
---
<a name="logging-error-details-with-aspnet-health-monitoring-vb"></a>Journalisation des détails de l’erreur avec l’intégrité ASP.NET analyse (VB)
====================
par [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Télécharger le Code](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_13_VB.zip) ou [télécharger le PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial13_HealthMonitoring_vb.pdf)

> Système de surveillance de l’intégrité de Microsoft fournit un moyen facile et personnalisable pour vous connecter divers événements web, y compris les exceptions non gérées. Ce didacticiel Guide d’installation de l’intégrité système de surveillance pour consigner les exceptions non gérées dans une base de données et pour avertir les développeurs via un message électronique.


## <a name="introduction"></a>Introduction

La journalisation est un outil utile pour surveiller l’intégrité d’une application déployée et de diagnostic des problèmes qui peuvent survenir. Il est particulièrement important de consigner les erreurs qui se produisent dans une application déployée afin qu’il peuvent être résolus. Le `Error` événement est déclenché chaque fois qu’une exception non gérée se produit dans une application ASP.NET ; le [didacticiel précédent](processing-unhandled-exceptions-vb.md) a montré comment avertir un développeur d’une erreur et consigner ses détails en créant un gestionnaire d’événements pour le `Error` événement. Toutefois, en créant un `Error` Gestionnaire d’événements à consigner les détails de l’erreur et de notifier un développeur n’est pas nécessaire, car cette tâche peut être effectuée par ASP. NET *contrôle d’intégrité système de surveillance*.

Le contrôle d’intégrité système de surveillance a été introduit dans ASP.NET 2.0 et est conçu pour surveiller l’intégrité d’une application ASP.NET déployée en consignant des événements qui se produisent pendant la durée de vie de la demande ou de l’application. Les événements consignés par le contrôle d’intégrité système de surveillance sont appelés *analyse les événements d’intégrité* ou *événements Web*et incluent :

- Événements de durée de vie d’application, telles que lorsqu’une application démarre ou s’arrête
- Événements de sécurité, y compris les tentatives de connexion ayant échoué et échec des demandes d’autorisation d’URL
- Erreurs d’application, y compris les exceptions non gérées, état d’affichage analyse des exceptions, les exceptions de validation de demande et les erreurs de compilation, parmi les autres types d’erreurs.

Quand une intégrité de la surveillance d’événement est déclenchée il peut être connecté à n’importe quel nombre de spécifié *connecter des sources*. Le contrôle d’intégrité système de surveillance est livré avec des sources de journal du journal des événements Web à une base de données Microsoft SQL Server, dans le journal des événements Windows, ou via un message électronique, entre autres. Vous pouvez également créer vos propres sources de journal.

Les événements de journaux de l’intégrité système de surveillance, ainsi que les sources de journal utilisées, sont définis dans `Web.config`. Avec quelques lignes de balisage de configuration, vous pouvez utiliser pour consigner toutes les exceptions non gérées dans une base de données et pour vous avertir de l’exception par courrier électronique de contrôle d’intégrité.

## <a name="exploring-the-health-monitoring-systems-configuration"></a>Exploration de la Configuration du système de contrôle d’état

Le comportement du système de contrôle d’état est défini par ses informations de configuration, ce qui se trouve dans le [ `<healthMonitoring>` élément](https://msdn.microsoft.com/library/2fwh2ss9.aspx) dans `Web.config`. Cette section de configuration définit, entre autres choses, les trois éléments importants d’informations :

1. Les événements de contrôle d’état qui, lorsque déclenché, doivent être enregistrés,
2. Les sources de journal, et
3. Comment chaque contrôle d’intégrité analyse l’événement défini dans (1) est mappé aux sources de journal défini dans (2).

Ces informations sont spécifiées par le biais des éléments de configuration de trois enfants : [ `<eventMappings>` ](https://msdn.microsoft.com/library/yc5yk01w.aspx), [ `<providers>` ](https://msdn.microsoft.com/library/zaa41kz1.aspx), et [ `<rules>` ](https://msdn.microsoft.com/library/fe5wyxa0.aspx), respectivement.

Vous trouverez les informations de configuration système de contrôle d’état par défaut dans le `Web.config` fichier `%WINDIR%\Microsoft.NET\Framework\version\CONFIG` dossier. Ces informations de configuration par défaut, avec des balises supprimées par souci de concision, sont indiquées ci-dessous :

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample1.xml)]

L’intégrité de surveillance des événements d’intérêt sont définis dans le `<eventMappings>` élément, ce qui donne un nom convivial pour une classe d’événements de contrôle d’intégrité. Dans le balisage ci-dessus, le `<eventMappings>` élément attribue le nom convivial « Toutes les erreurs » événements de type de contrôle d’intégrité `WebBaseErrorEvent` et le nom « Audits des échecs » pour les événements de type de contrôle d’intégrité `WebFailureAuditEvent`.

Le `<providers>` élément définit les sources de journal, en leur donnant un nom convivial et en spécifiant les informations de configuration spécifique à la source de journal. La première `<add>` élément définit le fournisseur « EventLogProvider », qui consigne l’événements à l’aide de contrôle d’état spécifié la `EventLogWebEventProvider` classe. Le `EventLogWebEventProvider` classe consigne l’événement dans le journal des événements Windows. La seconde `<add>` élément définit le fournisseur « SqlWebEventProvider », qui enregistre les événements dans une base de données Microsoft SQL Server via la `SqlWebEventProvider` classe. La configuration « SqlWebEventProvider » spécifie la chaîne de connexion de la base de données (`connectionStringName`) parmi les autres options de configuration.

Le `<rules>` élément mappe les événements spécifiés dans le `<eventMappings>` élément pour vous connecter des sources le `<providers>` élément. Par défaut, les applications web ASP.NET enregistrer toutes les exceptions non gérées et auditer les échecs dans le journal des événements Windows.

## <a name="logging-events-to-a-database"></a>Journalisation des événements dans une base de données

Le contrôle de configuration du système par défaut d’état peut être personnalisé sur une base d’application par web application web en ajoutant un `<healthMonitoring>` section à l’application `Web.config` fichier. Vous pouvez inclure des éléments supplémentaires dans le `<eventMappings>`, `<providers>`, et `<rules>` sections à l’aide de la `<add>` élément. Pour supprimer un paramètre à partir de l’utilisation de la configuration par défaut le `<remove>` élément, ou utilisez `<clear />` pour supprimer toutes les valeurs par défaut de l’une de ces sections. Nous allons maintenant configurer l’application web de critiques de livres pour vous connecter des exceptions non gérées à une base de données Microsoft SQL Server à l’aide la `SqlWebEventProvider` classe.

Le `SqlWebEventProvider` classe fait partie de l’intégrité du système de surveillance et consigne un événement à une base de données SQL Server spécifiée de contrôle d’état. Le `SqlWebEventProvider` classe attend que la base de données spécifiée inclut une procédure stockée nommée `aspnet_WebEvent_LogEvent`. Cette procédure stockée est passée les détails de l’événement et est chargée de stocker les détails de l’événement. La bonne nouvelle est que vous n’avez pas besoin de créer cette procédure procédure ni la table pour stocker les détails de l’événement. Vous pouvez ajouter ces objets à votre base de données en utilisant le `aspnet_regsql.exe` outil.

> [!NOTE]
> Le `aspnet_regsql.exe` outil a été abordé dans le [ *configuration d’un site Web qu’utilise les Services d’Application* didacticiel](configuring-a-website-that-uses-application-services-vb.md) lorsque nous avons ajouté la prise en charge pour ASP. Services d’application de NET. Par conséquent, base de données du site Web critiques de livres contient déjà le `aspnet_WebEvent_LogEvent` procédure stockée, qui stocke les informations d’événement dans une table nommée `aspnet_WebEvent_Events`.


Une fois que vous disposez de la procédure stockée nécessaire et la table ajoutée à votre base de données, reste qu’à indiquer pour consigner toutes les exceptions non gérées dans la base de données de contrôle d’intégrité. Cela en ajoutant le balisage suivant à votre site Web `Web.config` fichier :

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample2.xml)]

Le contrôle d’intégrité analyse le balisage de configuration ci-dessus utilise `<clear />` éléments à réinitialiser l’intégrité prédéfinie analyse les informations de configuration de la `<eventMappings>`, `<providers>`, et `<rules>` sections. Il ajoute ensuite une entrée unique pour chacune de ces sections.

- Le `<eventMappings>` élément définit un événement nommé « Toutes les erreurs », ce qui est déclenché chaque fois qu’une exception non gérée se produit de contrôle d’état unique.
- Le `<providers>` élément définit une source de journal unique nommée « SqlWebEventProvider » qui utilise le `SqlWebEventProvider` classe. Le `connectionStringName` attribut a été défini sur « ReviewsConnectionString », qui est le nom de notre connexion chaîne définie dans la `<connectionStrings>` section.
- Enfin, le &lt;règles&gt; élément indique que quand un événement de « Toutes les erreurs » est apparu qu’il doit être enregistré à l’aide du fournisseur « SqlWebEventProvider ».

Ces informations de configuration indique à l’intégrité système pour enregistrer toutes les exceptions non gérées dans la base de données critiques de livres de surveillance.

> [!NOTE]
> Le `WebBaseErrorEvent` événement est déclenché uniquement pour les erreurs de serveur ; il n’est pas déclenché pour les erreurs HTTP, telle qu’une demande pour une ressource ASP.NET qui est introuvable. Ce comportement diffère de la `HttpApplication` la classe `Error` événement, qui est déclenché pour le serveur et erreurs HTTP.


Pour voir l’intégrité système dans l’action de surveillance, visitez le site Web et générer une erreur d’exécution en vous rendant sur `Genre.aspx?ID=foo`. Vous devez voir la page d’erreur approprié - Exception détails jaune écran de décès (lors de la visite localement) ou la page d’erreur personnalisée (lors de la visite le site de production). Dans les coulisses, l’intégrité système de surveillance enregistrées les informations d’erreur dans la base de données. Il doit y avoir un seul enregistrement dans le `aspnet_WebEvent_Events` table (voir **Figure 1**) ; cet enregistrement contient des informations sur l’erreur d’exécution qui s’est produite.

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image2.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image1.png)

**Figure 1**: détails de l’erreur ont été consignés pour le `aspnet_WebEvent_Events` Table  
([Cliquez pour afficher l’image en taille réelle](logging-error-details-with-asp-net-health-monitoring-vb/_static/image3.png))

### <a name="displaying-the-error-log-in-a-web-page"></a>Afficher le journal des erreurs dans une Page Web

Avec la configuration actuelle du site Web, le contrôle d’intégrité système de surveillance consigne toutes les exceptions non gérées dans la base de données. Toutefois, la surveillance de l’intégrité ne fournit pas n’importe quel mécanisme pour afficher le journal des erreurs via une page web. Toutefois, vous pouvez générer une page ASP.NET qui affiche ces informations à partir de la base de données. (Comme nous le verrons dans un instant, vous pouvez choisir d’avoir les détails de l’erreur envoyées dans un message électronique.)

Si vous créez une telle page, veillez à que prendre des mesures pour autoriser uniquement les utilisateurs autorisés à afficher les détails de l’erreur. Si votre site utilise déjà des comptes d’utilisateur vous pouvez utiliser des règles d’autorisation d’URL pour restreindre l’accès à la page pour certains utilisateurs ou les rôles. Pour plus d’informations sur comment autoriser ou restreindre l’accès aux pages web selon l’utilisateur connecté, reportez-vous à mon [didacticiels de sécurité de site Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

> [!NOTE]
> Le didacticiel suivant explore une autre erreur journalisation et notification système nommé ELMAH. ELMAH inclut un mécanisme intégré pour afficher le journal des erreurs depuis les deux une page web et comme un flux RSS.


## <a name="logging-events-to-email"></a>Journalisation des événements à la messagerie

Le contrôle d’intégrité système de surveillance comprend un fournisseur de source de journal qui « journaux » un événement à un message électronique. La source du journal inclut les mêmes informations sont consignées dans la base de données dans le corps du message électronique. Vous pouvez utiliser cette source de journal pour notifier un développeur lorsqu’un certain événement de contrôle d’intégrité se produit.

Nous allons mettre à jour la configuration du site Web afin que nous recevons un message électronique chaque fois qu’une exception se produit de critiques de livres. Pour ce faire, nous devons effectuer trois tâches :

1. Configurer l’application web ASP.NET pour envoyer un e-mail. Cela s’effectue en spécifiant la façon dont les messages électroniques sont envoyés le `<system.net>` élément de configuration. Pour plus d’informations sur l’envoi de courriers électroniques, messages dans une application ASP.NET font référence à [envoi de courrier électronique dans ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx) et [System.Net.Mail FAQ](http://systemnetmail.com/).
2. Inscrire le fournisseur de source de journal de messagerie dans le `<providers>` élément, et
3. Ajouter une entrée à la `<rules>` élément qui mappe l’événement « Toutes les erreurs » pour le fournisseur de source de journal ajouté à l’étape (2).

Le contrôle d’intégrité système de surveillance comprend deux classes du fournisseur de source journal e-mail : `SimpleMailWebEventProvider` et `TemplatedMailWebEventProvider`. Le [ `SimpleMailWebEventProvider` classe](https://msdn.microsoft.com/library/system.web.management.simplemailwebeventprovider.aspx) envoie un e-mail en texte brut qui inclut l’événement décrit en détail et fournit peu de personnalisation du corps du courrier électronique. Avec le [ `TemplatedMailWebEventProvider` classe](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) vous spécifiez une page ASP.NET dont le balisage rendu est utilisé en tant que le corps de l’e-mail. Le [ `TemplatedMailWebEventProvider` classe](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) vous donne un contrôle plus le contenu et le format du message électronique, mais nécessite un peu plus de travail initial que vous devez créer la page ASP.NET qui génère le corps du message de courrier électronique. Ce didacticiel se concentre sur l’utilisation de la `SimpleMailWebEventProvider` classe.

Mettre à jour de l’intégrité système de surveillance `<providers>` élément dans le `Web.config` fichier à inclure une source de journal pour la `SimpleMailWebEventProvider` classe :

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample3.xml)]

Le balisage ci-dessus utilise la `SimpleMailWebEventProvider` classe en tant que le fournisseur de source de journal et lui attribue le nom convivial « EmailWebEventProvider ». En outre, le `<add>` attribut inclut des options de configuration supplémentaires, tels que les champs à et à partir d’adresses du message électronique.

Avec la source de journal de messagerie définie, tous ne reste qu’à demander à l’intégrité système pour utiliser cette source pour les exceptions non gérées du « journal » de surveillance. Cela s’effectue en ajoutant une nouvelle règle dans la `<rules>` section :

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample4.xml)]

Le `<rules>` section inclut désormais deux règles. Le, nommé « Toutes les erreurs à l’E-mail », envoie toutes les exceptions non gérées à la source du journal « EmailWebEventProvider ». Cette règle a pour effet d’envoyer des détails sur les erreurs sur le site Web spécifié à l’adresse. La règle « Toutes les erreurs de base de données » consigne les détails de l’erreur dans la base de données. Par conséquent, chaque fois qu’une exception non gérée produit sur le site ses détails sont à la fois consignées à la base de données et envoyées à l’adresse de messagerie spécifiée.

**Figure 2** montrant l’e-mail généré par le `SimpleMailWebEventProvider` classe lors de la visite `Genre.aspx?ID=foo`.

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image5.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image4.png)

**Figure 2**: les détails de l’erreur sont envoyés dans un Message électronique  
([Cliquez pour afficher l’image en taille réelle](logging-error-details-with-asp-net-health-monitoring-vb/_static/image6.png))

## <a name="summary"></a>Récapitulatif

Le système de surveillance de l’intégrité ASP.NET est conçu pour permettre aux administrateurs de surveiller l’intégrité d’une application web déployée. Événements de contrôle d’intégrité sont déclenchés lorsque certaines actions se déroulent, telles que lorsque l’application s’arrête, quand un utilisateur se connecte au site, ou lorsqu’une exception non gérée se produit. Ces événements peuvent être enregistrés dans le nombre de sources de journal. Ce didacticiel vous a montré comment consigner les détails d’exceptions non gérées dans une base de données et via un message électronique.

Ce didacticiel se concentre sur l’utilisation pour les exceptions non gérées, mais n’oubliez pas que l’analyse du fonctionnement est conçue pour mesurer l’intégrité globale d’une application ASP.NET déployée et comprend une multitude d’événements d’analyse du fonctionnement se connecter et sources pas de contrôle d’intégrité exploré ici. Quel est le plus, vous pouvez créer vos propres événements et des sources de journaux, de contrôle d’état en cas de besoin surviennent. Si vous souhaitez en savoir plus sur la surveillance de l’intégrité, la première étape consiste à lire [Erik Reitan](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)de [Forum aux questions de contrôle d’intégrité](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx). Ensuite, consultez [comment faire : utiliser la surveillance d’intégrité dans ASP.NET 2.0](https://msdn.microsoft.com/library/ms998306.aspx).

Bonne programmation !

### <a name="further-reading"></a>informations supplémentaires

Pour plus d’informations sur les sujets abordés dans ce didacticiel, consultez les ressources suivantes :

- [Vue d’ensemble de la surveillance d’état ASP.NET](https://msdn.microsoft.com/library/bb398933.aspx)
- [Configuration et la personnalisation de l’intégrité de surveillance du système d’ASP.NET](http://dotnetslackers.com/articles/aspnet/ConfiguringAndCustomizingTheHealthMonitoringSystemOfASPNET.aspx)
- [FAQ - contrôle d’intégrité de la surveillance dans ASP.NET 2.0](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)
- [Comment : Envoyer un courrier électronique pour les Notifications de contrôle d’intégrité](https://msdn.microsoft.com/library/ms227553.aspx)
- [Guide pratique pour utiliser le contrôle d’intégrité dans ASP.NET](https://msdn.microsoft.com/library/ms998306.aspx)
- [Intégrité de la surveillance dans ASP.NET](http://aspnet.4guysfromrolla.com/articles/031407-1.aspx)

> [!div class="step-by-step"]
> [Précédent](processing-unhandled-exceptions-vb.md)
> [Suivant](logging-error-details-with-elmah-vb.md)
