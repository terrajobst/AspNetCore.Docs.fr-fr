---
title: DevOps avec ASP.NET Core et Azure | Surveiller et déboguer
author: CamSoper
description: Un guide qui fournit des conseils de bout en bout sur la création d’un pipeline DevOps pour une application ASP.NET Core hébergée dans Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/monitor
ms.openlocfilehash: c2fc88493aee04d7ea2781d17e808581e89d2082
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2018
ms.locfileid: "42910017"
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

1. Ouvrez le [portail Azure](https://portal.azure.com), puis accédez à la *mywebapp\<unique_number\>*  App Service.

1. Le **vue d’ensemble** onglet affiche des informations de « at-a-glance » utiles, notamment des graphiques affichant les métriques récentes.

    ![Panneau Vue d'ensemble](./media/monitoring/overview.png)

    * **HTTP 5xx**: nombre d’erreurs côté serveur, généralement des exceptions dans du code ASP.NET Core.
    * **Données entrantes**: entrée de données entrant dans votre application web.
    * **Données sortantes**: données de sortie à partir de votre application web aux clients.
    * **Demandes**: nombre de requêtes HTTP.
    * **Temps de réponse moyen**: temps moyen de l’application web répondre aux demandes HTTP.

    Plusieurs outils en libre-service pour le dépannage et l’optimisation figurent également sur cette page.

    ![Outils en libre-service](./media/monitoring/wizards.png)

    * **Diagnostiquer et résoudre les problèmes** est un dépanneur libre-service.
    * **Application Insights** est pour le profilage des performances et le comportement de l’application et est décrit plus loin dans cette section.
    * **App Service Advisor** émet des recommandations pour paramétrer votre expérience de l’application.

## <a name="advanced-monitoring"></a>Surveillance avancée

[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) est un service centralisé de toutes les mesures de surveillance et la définition d’alertes sur les services Azure. Dans Azure Monitor, les administrateurs peuvent définir de façon précise le suivi des performances et identifier les tendances. Chaque service Azure propose son propre [ensemble de mesures](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) à Azure Monitor.

## <a name="profile-with-application-insights"></a>Profil avec Application Insights

[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) est un service Azure pour l’analyse des performances et la stabilité des applications web et la façon dont les utilisateurs les utiliser. Les données d’Application Insights soient élargie et plus complète que celle d’Azure Monitor. Les données peuvent fournir aux développeurs et aux administrateurs des informations clées pour améliorer les applications. Application Insights peuvent être ajoutés à une ressource Azure App Service sans modification du code.

1. Ouvrez le [portail Azure](https://portal.azure.com), puis accédez à la *mywebapp\<unique_number\>*  App Service.
1. À partir de la **vue d’ensemble** , cliquez sur le **Application Insights** vignette.

    ![Vignette application Insights](./media/monitoring/app-insights.png)

1. Sélectionnez le **Créer nouvelle ressource** case d’option. Utilisez le nom de ressources par défaut, puis sélectionnez l’emplacement de la ressource Application Insights. L’emplacement n’a pas besoin de correspondre à celui de votre application web.

    ![Programme d’installation de application Insights](./media/monitoring/new-app-insights.png)

1. Pour **Runtime/Framework**, sélectionnez **ASP.NET Core**. Acceptez les paramètres par défaut.
1. Sélectionnez **OK**. Si vous êtes invité à confirmer, sélectionnez **continuer**.
1. Une fois que la ressource a été créée, cliquez sur le nom de ressource Application Insights pour accéder directement à la page d’Application Insights.

    ![Nouvelle ressource Application Insights est prêt](./media/monitoring/new-app-insights-done.png)

Comme l’application est utilisée, les données s’accumulent. Sélectionnez **Actualiser** à recharger le panneau avec de nouvelles données.

![Onglet de vue d’ensemble application Insights](./media/monitoring/app-insights-overview.png)

Application Insights fournit des informations utiles côté serveur sans aucune configuration supplémentaire. Pour tirer le meilleur parti d’Application Insights, [instrumenter votre application avec le SDK Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core). Lorsque correctement configuré, le service fournit la surveillance de bout en bout entre le serveur web et le navigateur, y compris les performances côté client. Pour plus d’informations, consultez le [documentation Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview).

## <a name="logging"></a>Journalisation

Les journaux de serveur et d’application Web sont désactivés par défaut dans Azure App Service. Activer les journaux avec les étapes suivantes :

1. Ouvrez le [portail Azure](https://portal.azure.com)et accédez à la *mywebapp\<unique_number\>*  App Service.
1. Dans le menu à gauche, faites défiler jusqu'à la **surveillance** section. Sélectionnez **les journaux de diagnostic**.

    ![Lien des journaux de diagnostic](./media/monitoring/logging.png)

1. Activer **journalisation des applications (Filesystem)**. Si vous y êtes invité, cliquez sur la zone pour installer les extensions pour activer l’application de journalisation dans l’application web.
1. Définissez **journalisation du serveur Web** à **système de fichiers**.
1. Entrez le **période de rétention** en jours. Par exemple, 30.
1. Cliquez sur **Enregistrer**.

Les journaux de serveur (application Service) web et ASP.NET Core sont générés pour l’application web. Ils peuvent être téléchargés à l’aide des informations FTP/FTPS affichées. Le mot de passe est le même que les informations d’identification de déploiement créées précédemment dans ce guide. Les journaux peuvent être [transmis en continu directement sur votre ordinateur local avec Azure CLI ou PowerShell](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log#download). Journaux peuvent également être [affichés dans Application Insights](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).

## <a name="log-streaming"></a>Diffusion de journaux

Journaux du serveur web et application peuvent être diffusés en temps réel via le portail.

1. Ouvrez le [portail Azure](https://portal.azure.com)et accédez à la *mywebapp\<unique_number\>*  App Service.
1. Dans le menu à gauche, faites défiler jusqu'à la **surveillance** section et sélectionnez **flux de journal**.

    ![Lien de flux de données de journal](./media/monitoring/log-stream.png)

Journaux peuvent également être [transmis en continu via Azure CLI ou Azure PowerShell](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log#streamlogs), y compris via le Cloud Shell.

## <a name="alerts"></a>Alertes

Azure Monitor fournit également [alertes en temps réel](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal) selon des mesures, événements d’administration et d’autres critères.

> *Remarque : Actuellement des alertes sur les métriques de l’application web sont uniquement disponible dans le service alertes (classiques).*

Le [alertes (classique) service](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) se trouvent dans Azure Monitor ou sous le **surveillance** section des paramètres du Service d’application.

![Lien (classique) des alertes](./media/monitoring/alerts.png)

## <a name="live-debugging"></a>Le débogage en direct

Azure App Service peut être [de débogage à distance avec Visual Studio](https://docs.microsoft.com/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) lorsque les journaux ne fournissent suffisamment d’informations. Toutefois, le débogage à distance nécessite l’application à être compilé avec les symboles de débogage. Le débogage ne doivent pas le faire en production, sauf qu’en dernier recours.

## <a name="conclusion"></a>Conclusion

Dans cette section, vous effectué les tâches suivantes :

* Rechercher la base de surveillance et de résolution des problèmes de données dans le portail Azure
* Découvrez comment Azure Monitor fournit un aperçu plus approfondi des mesures pour tous les services Azure
* Connecter l’application web avec Application Insights pour le profilage d’applications
* Activer la journalisation et apprenez à télécharger les journaux
* Stream journaux en temps réel
* Découvrez où définir des alertes
* Découvrez à distance débogage Azure App Service web apps.

## <a name="additional-reading"></a>Lecture supplémentaire

* [Résoudre les problèmes liés à ASP.NET Core sur Azure App Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/azure-apps/troubleshoot)
* [Informations de référence sur les erreurs courantes pour Azure App Service et IIS avec ASP.NET Core](https://docs.microsoft.com/aspnet/core/host-and-deploy/azure-iis-errors-reference)
* [Surveiller les performances d’application web Azure avec Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-azure-web-apps)
* [Activer la journalisation des diagnostics pour les applications web dans Azure App Service](https://docs.microsoft.com/azure/app-service/web-sites-enable-diagnostic-log)
* [Résoudre les problèmes d’une application web dans Azure App Service avec Visual Studio](https://docs.microsoft.com/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [Créer des alertes de métriques classiques dans Azure Monitor pour les services Azure - portail Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-alerts-portal)
