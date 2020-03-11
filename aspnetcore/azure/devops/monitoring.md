---
title: Surveiller et déboguer - DevOps avec ASP.NET Core et Azure
author: CamSoper
description: Surveillance et le débogage de votre code en tant que partie d’une solution DevOps avec ASP.NET Core et Azure
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 07/10/2019
uid: azure/devops/monitor
ms.openlocfilehash: 1d8ed99f4387dbc99929164c558cc2ce14bd9ea0
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659501"
---
# <a name="monitor-and-debug"></a>Surveiller et déboguer

Après avoir déployé l’application et créé un pipeline DevOps, il est important de comprendre comment surveiller et résoudre les problèmes de l’application.

Dans cette section, vous allez effectuer les tâches suivantes :

* Rechercher la base de surveillance et de résolution des problèmes de données dans le portail Azure
* Découvrez comment Azure Monitor fournit un aperçu plus approfondi des mesures pour tous les services Azure
* Connecter l’application web avec Application Insights pour le profilage d’applications
* Activer la journalisation et apprenez à télécharger les journaux
* Stream journaux en temps réel
* Découvrez où définir des alertes
* Découvrez à distance débogage Azure App Service web apps.

## <a name="basic-monitoring-and-troubleshooting"></a>Surveillance de base et la résolution des problèmes

Applications web App Service sont facilement surveillées en temps réel. Le portail Azure affiche des métriques graphiques faciles à comprendre et.

1. Ouvrez le [portail Azure](https://portal.azure.com), puis accédez au *\<mywebapp unique_number\>* App service.

1. L’onglet **vue d’ensemble** affiche des informations utiles « en un clin d’œil », y compris des graphiques qui affichent les métriques récentes.

    ![Panneau de vue d’ensemble de capture d’écran montrant](./media/monitoring/overview.png)

    * **Http 5xx**: nombre d’erreurs côté serveur, généralement les exceptions dans ASP.net Core code.
    * **Données dans**: entrée de données entrant dans votre application Web.
    * **Données sortantes**: sortie de données de votre application Web vers les clients.
    * **Demandes**: nombre de requêtes http.
    * **Temps de réponse moyen**: temps moyen pour que l’application Web réponde aux requêtes http.

    Plusieurs outils en libre-service pour le dépannage et l’optimisation figurent également sur cette page.

    ![Capture d’écran montrant libre-service tools](./media/monitoring/wizards.png)

    * **Diagnostiquer et résoudre les problèmes** est un utilitaire de résolution des problèmes en libre-service.
    * **Application Insights** concerne les performances de profilage et le comportement des applications, et est abordé plus loin dans cette section.
    * **App service Advisor** émet des recommandations pour optimiser l’expérience de votre application.

## <a name="advanced-monitoring"></a>Surveillance avancée

[Azure Monitor](/azure/monitoring-and-diagnostics/) est le service centralisé pour surveiller toutes les métriques et définir des alertes dans les services Azure. Dans Azure Monitor, les administrateurs peuvent définir de façon précise le suivi des performances et identifier les tendances. Chaque service Azure offre son propre [ensemble de mesures](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) à Azure Monitor.

## <a name="profile-with-application-insights"></a>Profil avec Application Insights

[Application Insights](/azure/application-insights/app-insights-overview) est un service Azure qui permet d’analyser les performances et la stabilité des applications Web, ainsi que la manière dont les utilisateurs les utilisent. Les données d’Application Insights soient élargie et plus complète que celle d’Azure Monitor. Les données peuvent fournir aux développeurs et aux administrateurs des informations clées pour améliorer les applications. Application Insights peuvent être ajoutés à une ressource Azure App Service sans modification du code.

1. Ouvrez le [portail Azure](https://portal.azure.com), puis accédez au *\<mywebapp unique_number\>* App service.
1. Dans l’onglet **vue d’ensemble** , cliquez sur la vignette **application Insights** .

    ![Vignette Application Insights](./media/monitoring/app-insights.png)

1. Sélectionnez la case d’option **créer une nouvelle ressource** . Utilisez le nom de ressources par défaut, puis sélectionnez l’emplacement de la ressource Application Insights. L’emplacement n’a pas besoin de correspondre à celui de votre application web.

    ![Programme d’installation de application Insights](./media/monitoring/new-app-insights.png)

1. Pour **Runtime/Framework**, sélectionnez **ASP.net Core**. Acceptez les paramètres par défaut.
1. Sélectionnez **OK**. Si vous êtes invité à confirmer, sélectionnez **Continuer**.
1. Une fois que la ressource a été créée, cliquez sur le nom de ressource Application Insights pour accéder directement à la page d’Application Insights.

    ![Nouvelle ressource Application Insights est prêt](./media/monitoring/new-app-insights-done.png)

Comme l’application est utilisée, les données s’accumulent. Sélectionnez **Actualiser** pour recharger le panneau avec de nouvelles données.

![Onglet de vue d’ensemble application Insights](./media/monitoring/app-insights-overview.png)

Application Insights fournit des informations utiles côté serveur sans aucune configuration supplémentaire. Pour tirer le meilleur parti de Application Insights, [instrumentez votre application avec le kit de développement logiciel (SDK) application Insights](/azure/application-insights/app-insights-asp-net-core). Lorsque correctement configuré, le service fournit la surveillance de bout en bout entre le serveur web et le navigateur, y compris les performances côté client. Pour plus d’informations, consultez la [documentation application Insights](/azure/application-insights/app-insights-overview).

## <a name="logging"></a>Journalisation

Les journaux de serveur et d’application Web sont désactivés par défaut dans Azure App Service. Activer les journaux avec les étapes suivantes :

1. Ouvrez le [portail Azure](https://portal.azure.com), puis accédez au *\<mywebapp unique_number\>* App service.
1. Dans le menu de gauche, faites défiler jusqu’à la section **surveillance** . Sélectionnez **journaux de diagnostic**.

    ![Lien des journaux de diagnostic](./media/monitoring/logging.png)

1. Activez la **journalisation des applications (système de fichiers)** . Si vous y êtes invité, cliquez sur la zone pour installer les extensions pour activer l’application de journalisation dans l’application web.
1. Définissez la **journalisation du serveur Web** dans le **système de fichiers**.
1. Entrez la **période de rétention** en jours. Par exemple, 30.
1. Cliquez sur **Enregistrer**.

Les journaux de serveur (application Service) web et ASP.NET Core sont générés pour l’application web. Ils peuvent être téléchargés à l’aide des informations FTP/FTPS affichées. Le mot de passe est le même que les informations d’identification de déploiement créées précédemment dans ce guide. Les journaux peuvent être [transmis en continu directement sur votre machine locale avec PowerShell ou Azure CLI](/azure/app-service/web-sites-enable-diagnostic-log#download). Vous pouvez également consulter les journaux [dans application Insights](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).

## <a name="log-streaming"></a>Diffusion de journaux

Journaux du serveur web et application peuvent être diffusés en temps réel via le portail.

1. Ouvrez le [portail Azure](https://portal.azure.com), puis accédez au *\<mywebapp unique_number\>* App service.
1. Dans le menu de gauche, faites défiler jusqu’à la section **surveillance** , puis sélectionnez **flux de journal**.

    ![Capture d’écran montrant le lien flux journal](./media/monitoring/log-stream.png)

Les journaux peuvent également être [diffusés en continu via Azure CLI ou Azure PowerShell](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs), y compris via le Cloud Shell.

## <a name="alerts"></a>Alertes

Azure Monitor fournit également des [alertes en temps réel](/azure/monitoring-and-diagnostics/insights-alerts-portal) en fonction des métriques, des événements administratifs et d’autres critères.

> *Remarque : actuellement, les alertes sur les métriques de l’application Web sont uniquement disponibles dans le service alertes (Classic).*

Le [service alertes (Classic)](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) se trouve dans Azure Monitor ou sous la section **surveillance** des paramètres App service.

![Lien (classique) des alertes](./media/monitoring/alerts.png)

## <a name="live-debugging"></a>Le débogage en direct

Les Azure App Service peuvent être [débogués à distance avec Visual Studio quand les](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) journaux ne fournissent pas suffisamment d’informations. Toutefois, le débogage à distance nécessite l’application à être compilé avec les symboles de débogage. Le débogage ne doivent pas le faire en production, sauf qu’en dernier recours.

## <a name="conclusion"></a>Conclusion

Dans cette section, vous effectué les tâches suivantes :

* Rechercher la base de surveillance et de résolution des problèmes de données dans le portail Azure
* Découvrez comment Azure Monitor fournit un aperçu plus approfondi des mesures pour tous les services Azure
* Connecter l’application web avec Application Insights pour le profilage d’applications
* Activer la journalisation et apprenez à télécharger les journaux
* Stream journaux en temps réel
* Découvrez où définir des alertes
* Découvrez à distance débogage Azure App Service web apps.

## <a name="additional-reading"></a>Documentation supplémentaire

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [Surveiller les performances de l’application web Azure avec Application Insights](/azure/application-insights/app-insights-azure-web-apps)
* [Activer la journalisation des diagnostics pour les applications web dans Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log)
* [Résoudre les problèmes d’une application web dans Azure App Service avec Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Créer des alertes de métriques classiques dans Azure Monitor pour les services Azure-Portail Azure](/azure/monitoring-and-diagnostics/insights-alerts-portal)
