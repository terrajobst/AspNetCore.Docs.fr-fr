---
title: Surveiller et déboguer - DevOps avec ASP.NET Core et Azure
author: CamSoper
description: Surveillance et le débogage de votre code en tant que partie d’une solution DevOps avec ASP.NET Core et Azure
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 07/10/2019
uid: azure/devops/monitor
ms.openlocfilehash: 1d8ed99f4387dbc99929164c558cc2ce14bd9ea0
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68307956"
---
# <a name="monitor-and-debug"></a><span data-ttu-id="572e5-103">Surveiller et déboguer</span><span class="sxs-lookup"><span data-stu-id="572e5-103">Monitor and debug</span></span>

<span data-ttu-id="572e5-104">Après avoir déployé l’application et créé un pipeline DevOps, il est important de comprendre comment surveiller et résoudre les problèmes de l’application.</span><span class="sxs-lookup"><span data-stu-id="572e5-104">Having deployed the app and built a DevOps pipeline, it's important to understand how to monitor and troubleshoot the app.</span></span>

<span data-ttu-id="572e5-105">Dans cette section, vous allez effectuer les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="572e5-105">In this section, you'll complete the following tasks:</span></span>

* <span data-ttu-id="572e5-106">Rechercher la base de surveillance et de résolution des problèmes de données dans le portail Azure</span><span class="sxs-lookup"><span data-stu-id="572e5-106">Find basic monitoring and troubleshooting data in the Azure portal</span></span>
* <span data-ttu-id="572e5-107">Découvrez comment Azure Monitor fournit un aperçu plus approfondi des mesures pour tous les services Azure</span><span class="sxs-lookup"><span data-stu-id="572e5-107">Learn how Azure Monitor provides a deeper look at metrics across all Azure services</span></span>
* <span data-ttu-id="572e5-108">Connecter l’application web avec Application Insights pour le profilage d’applications</span><span class="sxs-lookup"><span data-stu-id="572e5-108">Connect the web app with Application Insights for app profiling</span></span>
* <span data-ttu-id="572e5-109">Activer la journalisation et apprenez à télécharger les journaux</span><span class="sxs-lookup"><span data-stu-id="572e5-109">Turn on logging and learn where to download logs</span></span>
* <span data-ttu-id="572e5-110">Stream journaux en temps réel</span><span class="sxs-lookup"><span data-stu-id="572e5-110">Stream logs in real time</span></span>
* <span data-ttu-id="572e5-111">Découvrez où définir des alertes</span><span class="sxs-lookup"><span data-stu-id="572e5-111">Learn where to set up alerts</span></span>
* <span data-ttu-id="572e5-112">Découvrez à distance débogage Azure App Service web apps.</span><span class="sxs-lookup"><span data-stu-id="572e5-112">Learn about remote debugging Azure App Service web apps.</span></span>

## <a name="basic-monitoring-and-troubleshooting"></a><span data-ttu-id="572e5-113">Surveillance de base et la résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="572e5-113">Basic monitoring and troubleshooting</span></span>

<span data-ttu-id="572e5-114">Applications web App Service sont facilement surveillées en temps réel.</span><span class="sxs-lookup"><span data-stu-id="572e5-114">App Service web apps are easily monitored in real time.</span></span> <span data-ttu-id="572e5-115">Le portail Azure affiche des métriques graphiques faciles à comprendre et.</span><span class="sxs-lookup"><span data-stu-id="572e5-115">The Azure portal renders metrics in easy-to-understand charts and graphs.</span></span>

1. <span data-ttu-id="572e5-116">Ouvrez le [portail Azure](https://portal.azure.com), puis accédez à la *mywebapp\<unique_number\>*  App Service.</span><span class="sxs-lookup"><span data-stu-id="572e5-116">Open the [Azure portal](https://portal.azure.com), and then navigate to the *mywebapp\<unique_number\>* App Service.</span></span>

1. <span data-ttu-id="572e5-117">Le **vue d’ensemble** onglet affiche des informations de « at-a-glance » utiles, notamment des graphiques affichant les métriques récentes.</span><span class="sxs-lookup"><span data-stu-id="572e5-117">The **Overview** tab displays useful "at-a-glance" information, including graphs displaying recent metrics.</span></span>

    ![Panneau de vue d’ensemble de capture d’écran montrant](./media/monitoring/overview.png)

    * <span data-ttu-id="572e5-119">**Http 5xx**: Nombre d’erreurs côté serveur, généralement les exceptions dans le code ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="572e5-119">**Http 5xx**: Count of server-side errors, usually exceptions in ASP.NET Core code.</span></span>
    * <span data-ttu-id="572e5-120">**Données dans**: Entrée de données entrant dans votre application Web.</span><span class="sxs-lookup"><span data-stu-id="572e5-120">**Data In**: Data ingress coming into your web app.</span></span>
    * <span data-ttu-id="572e5-121">**Données sortantes**: Sortie de données de votre application Web vers les clients.</span><span class="sxs-lookup"><span data-stu-id="572e5-121">**Data Out**: Data egress from your web app to clients.</span></span>
    * <span data-ttu-id="572e5-122">**Demandes**: Nombre de requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="572e5-122">**Requests**: Count of HTTP requests.</span></span>
    * <span data-ttu-id="572e5-123">**Temps de réponse moyen**: Temps moyen pour que l’application Web réponde aux requêtes HTTP.</span><span class="sxs-lookup"><span data-stu-id="572e5-123">**Average Response Time**: Average time for the web app to respond to HTTP requests.</span></span>

    <span data-ttu-id="572e5-124">Plusieurs outils en libre-service pour le dépannage et l’optimisation figurent également sur cette page.</span><span class="sxs-lookup"><span data-stu-id="572e5-124">Several self-service tools for troubleshooting and optimization are also found on this page.</span></span>

    ![Capture d’écran montrant libre-service tools](./media/monitoring/wizards.png)

    * <span data-ttu-id="572e5-126">**Diagnostiquer et résoudre les problèmes** est un dépanneur libre-service.</span><span class="sxs-lookup"><span data-stu-id="572e5-126">**Diagnose and solve problems** is a self-service troubleshooter.</span></span>
    * <span data-ttu-id="572e5-127">**Application Insights** est pour le profilage des performances et le comportement de l’application et est décrit plus loin dans cette section.</span><span class="sxs-lookup"><span data-stu-id="572e5-127">**Application Insights** is for profiling performance and app behavior, and is discussed later in this section.</span></span>
    * <span data-ttu-id="572e5-128">**App Service Advisor** émet des recommandations pour paramétrer votre expérience de l’application.</span><span class="sxs-lookup"><span data-stu-id="572e5-128">**App Service Advisor** makes recommendations to tune your app experience.</span></span>

## <a name="advanced-monitoring"></a><span data-ttu-id="572e5-129">Surveillance avancée</span><span class="sxs-lookup"><span data-stu-id="572e5-129">Advanced monitoring</span></span>

<span data-ttu-id="572e5-130">[Azure Monitor](/azure/monitoring-and-diagnostics/) est un service centralisé de toutes les mesures de surveillance et la définition d’alertes sur les services Azure.</span><span class="sxs-lookup"><span data-stu-id="572e5-130">[Azure Monitor](/azure/monitoring-and-diagnostics/) is the centralized service for monitoring all metrics and setting alerts across Azure services.</span></span> <span data-ttu-id="572e5-131">Dans Azure Monitor, les administrateurs peuvent définir de façon précise le suivi des performances et identifier les tendances.</span><span class="sxs-lookup"><span data-stu-id="572e5-131">Within Azure Monitor, administrators can granularly track performance and identify trends.</span></span> <span data-ttu-id="572e5-132">Chaque service Azure propose son propre [ensemble de mesures](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) à Azure Monitor.</span><span class="sxs-lookup"><span data-stu-id="572e5-132">Each Azure service offers its own [set of metrics](/azure/monitoring-and-diagnostics/monitoring-supported-metrics#microsoftwebsites-excluding-functions) to Azure Monitor.</span></span>

## <a name="profile-with-application-insights"></a><span data-ttu-id="572e5-133">Profil avec Application Insights</span><span class="sxs-lookup"><span data-stu-id="572e5-133">Profile with Application Insights</span></span>

<span data-ttu-id="572e5-134">[Application Insights](/azure/application-insights/app-insights-overview) est un service Azure pour l’analyse des performances et la stabilité des applications web et la façon dont les utilisateurs les utiliser.</span><span class="sxs-lookup"><span data-stu-id="572e5-134">[Application Insights](/azure/application-insights/app-insights-overview) is an Azure service for analyzing the performance and stability of web apps and how users use them.</span></span> <span data-ttu-id="572e5-135">Les données d’Application Insights soient élargie et plus complète que celle d’Azure Monitor.</span><span class="sxs-lookup"><span data-stu-id="572e5-135">The data from Application Insights is broader and deeper than that of Azure Monitor.</span></span> <span data-ttu-id="572e5-136">Les données peuvent fournir aux développeurs et aux administrateurs des informations clées pour améliorer les applications.</span><span class="sxs-lookup"><span data-stu-id="572e5-136">The data can provide developers and administrators with key information for improving apps.</span></span> <span data-ttu-id="572e5-137">Application Insights peuvent être ajoutés à une ressource Azure App Service sans modification du code.</span><span class="sxs-lookup"><span data-stu-id="572e5-137">Application Insights can be added to an Azure App Service resource without code changes.</span></span>

1. <span data-ttu-id="572e5-138">Ouvrez le [portail Azure](https://portal.azure.com), puis accédez à la *mywebapp\<unique_number\>*  App Service.</span><span class="sxs-lookup"><span data-stu-id="572e5-138">Open the [Azure portal](https://portal.azure.com), and then navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="572e5-139">À partir de la **vue d’ensemble** , cliquez sur le **Application Insights** vignette.</span><span class="sxs-lookup"><span data-stu-id="572e5-139">From the **Overview** tab, click the **Application Insights** tile.</span></span>

    ![Vignette application Insights](./media/monitoring/app-insights.png)

1. <span data-ttu-id="572e5-141">Sélectionnez le **Créer nouvelle ressource** case d’option.</span><span class="sxs-lookup"><span data-stu-id="572e5-141">Select the **Create new resource** radio button.</span></span> <span data-ttu-id="572e5-142">Utilisez le nom de ressources par défaut, puis sélectionnez l’emplacement de la ressource Application Insights.</span><span class="sxs-lookup"><span data-stu-id="572e5-142">Use the default resource name, and select the location for the Application Insights resource.</span></span> <span data-ttu-id="572e5-143">L’emplacement n’a pas besoin de correspondre à celui de votre application web.</span><span class="sxs-lookup"><span data-stu-id="572e5-143">The location doesn't need to match that of your web app.</span></span>

    ![Programme d’installation de application Insights](./media/monitoring/new-app-insights.png)

1. <span data-ttu-id="572e5-145">Pour **Runtime/Framework**, sélectionnez **ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="572e5-145">For **Runtime/Framework**, select **ASP.NET Core**.</span></span> <span data-ttu-id="572e5-146">Acceptez les paramètres par défaut.</span><span class="sxs-lookup"><span data-stu-id="572e5-146">Accept the default settings.</span></span>
1. <span data-ttu-id="572e5-147">Sélectionnez **OK**.</span><span class="sxs-lookup"><span data-stu-id="572e5-147">Select **OK**.</span></span> <span data-ttu-id="572e5-148">Si vous êtes invité à confirmer, sélectionnez **continuer**.</span><span class="sxs-lookup"><span data-stu-id="572e5-148">If prompted to confirm, select **Continue**.</span></span>
1. <span data-ttu-id="572e5-149">Une fois que la ressource a été créée, cliquez sur le nom de ressource Application Insights pour accéder directement à la page d’Application Insights.</span><span class="sxs-lookup"><span data-stu-id="572e5-149">After the resource has been created, click the name of Application Insights resource to navigate directly to the Application Insights page.</span></span>

    ![Nouvelle ressource Application Insights est prêt](./media/monitoring/new-app-insights-done.png)

<span data-ttu-id="572e5-151">Comme l’application est utilisée, les données s’accumulent.</span><span class="sxs-lookup"><span data-stu-id="572e5-151">As the app is used, data accumulates.</span></span> <span data-ttu-id="572e5-152">Sélectionnez **Actualiser** à recharger le panneau avec de nouvelles données.</span><span class="sxs-lookup"><span data-stu-id="572e5-152">Select **Refresh** to reload the blade with new data.</span></span>

![Onglet de vue d’ensemble application Insights](./media/monitoring/app-insights-overview.png)

<span data-ttu-id="572e5-154">Application Insights fournit des informations utiles côté serveur sans aucune configuration supplémentaire.</span><span class="sxs-lookup"><span data-stu-id="572e5-154">Application Insights provides useful server-side information with no additional configuration.</span></span> <span data-ttu-id="572e5-155">Pour tirer le meilleur parti d’Application Insights, [instrumenter votre application avec le SDK Application Insights](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="572e5-155">To get the most value from Application Insights, [instrument your app with the Application Insights SDK](/azure/application-insights/app-insights-asp-net-core).</span></span> <span data-ttu-id="572e5-156">Lorsque correctement configuré, le service fournit la surveillance de bout en bout entre le serveur web et le navigateur, y compris les performances côté client.</span><span class="sxs-lookup"><span data-stu-id="572e5-156">When properly configured, the service provides end-to-end monitoring across the web server and browser, including client-side performance.</span></span> <span data-ttu-id="572e5-157">Pour plus d’informations, consultez le [documentation Application Insights](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="572e5-157">For more information, see the [Application Insights documentation](/azure/application-insights/app-insights-overview).</span></span>

## <a name="logging"></a><span data-ttu-id="572e5-158">Journalisation</span><span class="sxs-lookup"><span data-stu-id="572e5-158">Logging</span></span>

<span data-ttu-id="572e5-159">Les journaux de serveur et d’application Web sont désactivés par défaut dans Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="572e5-159">Web server and app logs are disabled by default in Azure App Service.</span></span> <span data-ttu-id="572e5-160">Activer les journaux avec les étapes suivantes :</span><span class="sxs-lookup"><span data-stu-id="572e5-160">Enable the logs with the following steps:</span></span>

1. <span data-ttu-id="572e5-161">Ouvrez le [portail Azure](https://portal.azure.com)et accédez à la *mywebapp\<unique_number\>*  App Service.</span><span class="sxs-lookup"><span data-stu-id="572e5-161">Open the [Azure portal](https://portal.azure.com), and navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="572e5-162">Dans le menu à gauche, faites défiler jusqu'à la **surveillance** section.</span><span class="sxs-lookup"><span data-stu-id="572e5-162">In the menu to the left, scroll down to the **Monitoring** section.</span></span> <span data-ttu-id="572e5-163">Sélectionnez **les journaux de diagnostic**.</span><span class="sxs-lookup"><span data-stu-id="572e5-163">Select **Diagnostics logs**.</span></span>

    ![Lien des journaux de diagnostic](./media/monitoring/logging.png)

1. <span data-ttu-id="572e5-165">Activer **journalisation des applications (Filesystem)** .</span><span class="sxs-lookup"><span data-stu-id="572e5-165">Turn on **Application Logging (Filesystem)**.</span></span> <span data-ttu-id="572e5-166">Si vous y êtes invité, cliquez sur la zone pour installer les extensions pour activer l’application de journalisation dans l’application web.</span><span class="sxs-lookup"><span data-stu-id="572e5-166">If prompted, click the box to install the extensions to enable app logging in the web app.</span></span>
1. <span data-ttu-id="572e5-167">Définissez **journalisation du serveur Web** à **système de fichiers**.</span><span class="sxs-lookup"><span data-stu-id="572e5-167">Set **Web server logging** to **File System**.</span></span>
1. <span data-ttu-id="572e5-168">Entrez le **période de rétention** en jours.</span><span class="sxs-lookup"><span data-stu-id="572e5-168">Enter the **Retention Period** in days.</span></span> <span data-ttu-id="572e5-169">Par exemple, 30.</span><span class="sxs-lookup"><span data-stu-id="572e5-169">For example, 30.</span></span>
1. <span data-ttu-id="572e5-170">Cliquez sur **Enregistrer**.</span><span class="sxs-lookup"><span data-stu-id="572e5-170">Click **Save**.</span></span>

<span data-ttu-id="572e5-171">Les journaux de serveur (application Service) web et ASP.NET Core sont générés pour l’application web.</span><span class="sxs-lookup"><span data-stu-id="572e5-171">ASP.NET Core and web server (App Service) logs are generated for the web app.</span></span> <span data-ttu-id="572e5-172">Ils peuvent être téléchargés à l’aide des informations FTP/FTPS affichées.</span><span class="sxs-lookup"><span data-stu-id="572e5-172">They can be downloaded using the FTP/FTPS information displayed.</span></span> <span data-ttu-id="572e5-173">Le mot de passe est le même que les informations d’identification de déploiement créées précédemment dans ce guide.</span><span class="sxs-lookup"><span data-stu-id="572e5-173">The password is the same as the deployment credentials created earlier in this guide.</span></span> <span data-ttu-id="572e5-174">Les journaux peuvent être [transmis en continu directement sur votre ordinateur local avec Azure CLI ou PowerShell](/azure/app-service/web-sites-enable-diagnostic-log#download).</span><span class="sxs-lookup"><span data-stu-id="572e5-174">The logs can be [streamed directly to your local machine with PowerShell or Azure CLI](/azure/app-service/web-sites-enable-diagnostic-log#download).</span></span> <span data-ttu-id="572e5-175">Journaux peuvent également être [affichés dans Application Insights](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).</span><span class="sxs-lookup"><span data-stu-id="572e5-175">Logs can also be [viewed in Application Insights](/azure/app-service/web-sites-enable-diagnostic-log#how-to-view-logs-in-application-insights).</span></span>

## <a name="log-streaming"></a><span data-ttu-id="572e5-176">Diffusion de journaux</span><span class="sxs-lookup"><span data-stu-id="572e5-176">Log streaming</span></span>

<span data-ttu-id="572e5-177">Journaux du serveur web et application peuvent être diffusés en temps réel via le portail.</span><span class="sxs-lookup"><span data-stu-id="572e5-177">App and web server logs can be streamed in real time through the portal.</span></span>

1. <span data-ttu-id="572e5-178">Ouvrez le [portail Azure](https://portal.azure.com)et accédez à la *mywebapp\<unique_number\>*  App Service.</span><span class="sxs-lookup"><span data-stu-id="572e5-178">Open the [Azure portal](https://portal.azure.com), and navigate to the *mywebapp\<unique_number\>* App Service.</span></span>
1. <span data-ttu-id="572e5-179">Dans le menu à gauche, faites défiler jusqu'à la **surveillance** section et sélectionnez **flux de journal**.</span><span class="sxs-lookup"><span data-stu-id="572e5-179">In the menu to the left, scroll down to the **Monitoring** section and select **Log stream**.</span></span>

    ![Capture d’écran montrant le lien flux journal](./media/monitoring/log-stream.png)

<span data-ttu-id="572e5-181">Journaux peuvent également être [transmis en continu via Azure CLI ou Azure PowerShell](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs), y compris via le Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="572e5-181">Logs can also be [streamed via Azure CLI or Azure PowerShell](/azure/app-service/web-sites-enable-diagnostic-log#streamlogs), including through the Cloud Shell.</span></span>

## <a name="alerts"></a><span data-ttu-id="572e5-182">Alertes</span><span class="sxs-lookup"><span data-stu-id="572e5-182">Alerts</span></span>

<span data-ttu-id="572e5-183">Azure Monitor fournit également [alertes en temps réel](/azure/monitoring-and-diagnostics/insights-alerts-portal) selon des mesures, événements d’administration et d’autres critères.</span><span class="sxs-lookup"><span data-stu-id="572e5-183">Azure Monitor also provides [real time alerts](/azure/monitoring-and-diagnostics/insights-alerts-portal) based on metrics, administrative events, and other criteria.</span></span>

> <span data-ttu-id="572e5-184">*Remarque : Actuellement, les alertes sur les métriques de l’application Web sont uniquement disponibles dans le service alertes (Classic).*</span><span class="sxs-lookup"><span data-stu-id="572e5-184">*Note: Currently alerting on web app metrics is only available in the Alerts (classic) service.*</span></span>

<span data-ttu-id="572e5-185">Le [alertes (classique) service](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) se trouvent dans Azure Monitor ou sous le **surveillance** section des paramètres du Service d’application.</span><span class="sxs-lookup"><span data-stu-id="572e5-185">The [Alerts (classic) service](/azure/monitoring-and-diagnostics/monitor-quick-resource-metric-alert-portal) can be found in Azure Monitor or under the **Monitoring** section of the App Service settings.</span></span>

![Lien (classique) des alertes](./media/monitoring/alerts.png)

## <a name="live-debugging"></a><span data-ttu-id="572e5-187">Le débogage en direct</span><span class="sxs-lookup"><span data-stu-id="572e5-187">Live debugging</span></span>

<span data-ttu-id="572e5-188">Azure App Service peut être [de débogage à distance avec Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) lorsque les journaux ne fournissent suffisamment d’informations.</span><span class="sxs-lookup"><span data-stu-id="572e5-188">Azure App Service can be [debugged remotely with Visual Studio](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio#remotedebug) when logs don't provide enough information.</span></span> <span data-ttu-id="572e5-189">Toutefois, le débogage à distance nécessite l’application à être compilé avec les symboles de débogage.</span><span class="sxs-lookup"><span data-stu-id="572e5-189">However, remote debugging requires the app to be compiled with debug symbols.</span></span> <span data-ttu-id="572e5-190">Le débogage ne doivent pas le faire en production, sauf qu’en dernier recours.</span><span class="sxs-lookup"><span data-stu-id="572e5-190">Debugging shouldn't be done in production, except as a last resort.</span></span>

## <a name="conclusion"></a><span data-ttu-id="572e5-191">Conclusion</span><span class="sxs-lookup"><span data-stu-id="572e5-191">Conclusion</span></span>

<span data-ttu-id="572e5-192">Dans cette section, vous effectué les tâches suivantes :</span><span class="sxs-lookup"><span data-stu-id="572e5-192">In this section, you completed the following tasks:</span></span>

* <span data-ttu-id="572e5-193">Rechercher la base de surveillance et de résolution des problèmes de données dans le portail Azure</span><span class="sxs-lookup"><span data-stu-id="572e5-193">Find basic monitoring and troubleshooting data in the Azure portal</span></span>
* <span data-ttu-id="572e5-194">Découvrez comment Azure Monitor fournit un aperçu plus approfondi des mesures pour tous les services Azure</span><span class="sxs-lookup"><span data-stu-id="572e5-194">Learn how Azure Monitor provides a deeper look at metrics across all Azure services</span></span>
* <span data-ttu-id="572e5-195">Connecter l’application web avec Application Insights pour le profilage d’applications</span><span class="sxs-lookup"><span data-stu-id="572e5-195">Connect the web app with Application Insights for app profiling</span></span>
* <span data-ttu-id="572e5-196">Activer la journalisation et apprenez à télécharger les journaux</span><span class="sxs-lookup"><span data-stu-id="572e5-196">Turn on logging and learn where to download logs</span></span>
* <span data-ttu-id="572e5-197">Stream journaux en temps réel</span><span class="sxs-lookup"><span data-stu-id="572e5-197">Stream logs in real time</span></span>
* <span data-ttu-id="572e5-198">Découvrez où définir des alertes</span><span class="sxs-lookup"><span data-stu-id="572e5-198">Learn where to set up alerts</span></span>
* <span data-ttu-id="572e5-199">Découvrez à distance débogage Azure App Service web apps.</span><span class="sxs-lookup"><span data-stu-id="572e5-199">Learn about remote debugging Azure App Service web apps.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="572e5-200">Lecture supplémentaire</span><span class="sxs-lookup"><span data-stu-id="572e5-200">Additional reading</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="572e5-201">Surveiller les performances d’application web Azure avec Application Insights</span><span class="sxs-lookup"><span data-stu-id="572e5-201">Monitor Azure web app performance with Application Insights</span></span>](/azure/application-insights/app-insights-azure-web-apps)
* [<span data-ttu-id="572e5-202">Activer la journalisation des diagnostics pour les applications web dans Azure App Service</span><span class="sxs-lookup"><span data-stu-id="572e5-202">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)
* [<span data-ttu-id="572e5-203">Résoudre les problèmes d’une application web dans Azure App Service avec Visual Studio</span><span class="sxs-lookup"><span data-stu-id="572e5-203">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](/azure/app-service/web-sites-dotnet-troubleshoot-visual-studio)
* [<span data-ttu-id="572e5-204">Créer des alertes de métriques classiques dans Azure Monitor pour les services Azure - portail Azure</span><span class="sxs-lookup"><span data-stu-id="572e5-204">Create classic metric alerts in Azure Monitor for Azure services - Azure portal</span></span>](/azure/monitoring-and-diagnostics/insights-alerts-portal)
