---
title: Configurer l’authentification Windows dans ASP.NET Core
author: scottaddie
description: Découvrez comment configurer l’authentification Windows dans ASP.NET Core, à l’aide d’IIS Express, IIS et HTTP.sys.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 12/23/2018
uid: security/authentication/windowsauth
ms.openlocfilehash: 64178c8fce71445fc6a728a236d811484b21e3e0
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099258"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="83c4d-103">Configurer l’authentification Windows dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="83c4d-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="83c4d-104">Par [Scott Addie](https://twitter.com/Scott_Addie) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="83c4d-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="83c4d-105">[L’authentification Windows](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) peut être configuré pour les applications ASP.NET Core hébergées avec [IIS](xref:host-and-deploy/iis/index) ou [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="83c4d-105">[Windows Authentication](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="83c4d-106">L’authentification Windows s’appuie sur le système d’exploitation pour authentifier les utilisateurs des applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="83c4d-106">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="83c4d-107">Vous pouvez utiliser l’authentification Windows lorsque votre serveur s’exécute sur un réseau d’entreprise à l’aide d’identités de domaine Active Directory ou les comptes Windows pour identifier les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="83c4d-107">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="83c4d-108">L’authentification Windows est mieux adaptée aux environnements intranet où les utilisateurs, les applications clientes et les serveurs web appartiennent au même domaine Windows.</span><span class="sxs-lookup"><span data-stu-id="83c4d-108">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="83c4d-109">Activer l’authentification Windows dans une application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="83c4d-109">Enable Windows Authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="83c4d-110">Le **Web Application** modèle disponible via Visual Studio ou l’interface CLI .NET Core peut être configuré pour prendre en charge l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="83c4d-110">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="83c4d-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83c4d-111">Visual Studio</span></span>](#tab/visual-studio)

### <a name="use-the-windows-authentication-app-template-for-a-new-project"></a><span data-ttu-id="83c4d-112">Utiliser le modèle d’application de l’authentification Windows pour un nouveau projet</span><span class="sxs-lookup"><span data-stu-id="83c4d-112">Use the Windows Authentication app template for a new project</span></span>

<span data-ttu-id="83c4d-113">Dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="83c4d-113">In Visual Studio:</span></span>

1. <span data-ttu-id="83c4d-114">Créer un nouveau **Application Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="83c4d-114">Create a new **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="83c4d-115">Sélectionnez **Web Application** à partir de la liste des modèles.</span><span class="sxs-lookup"><span data-stu-id="83c4d-115">Select **Web Application** from the list of templates.</span></span>
1. <span data-ttu-id="83c4d-116">Sélectionnez le **modifier l’authentification** bouton et sélectionnez **l’authentification Windows**.</span><span class="sxs-lookup"><span data-stu-id="83c4d-116">Select the **Change Authentication** button and select **Windows Authentication**.</span></span>

<span data-ttu-id="83c4d-117">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="83c4d-117">Run the app.</span></span> <span data-ttu-id="83c4d-118">Le nom d’utilisateur s’affiche dans l’interface utilisateur de l’application rendue.</span><span class="sxs-lookup"><span data-stu-id="83c4d-118">The username appears in the rendered app's user interface.</span></span>

### <a name="manual-configuration-for-an-existing-project"></a><span data-ttu-id="83c4d-119">Configuration manuelle pour un projet existant</span><span class="sxs-lookup"><span data-stu-id="83c4d-119">Manual configuration for an existing project</span></span>

<span data-ttu-id="83c4d-120">Les propriétés du projet vous autorise à activer l’authentification Windows et désactivez l’authentification anonyme :</span><span class="sxs-lookup"><span data-stu-id="83c4d-120">The project's properties allow you to enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="83c4d-121">Cliquez sur le projet dans Visual Studio **l’Explorateur de solutions** et sélectionnez **propriétés**.</span><span class="sxs-lookup"><span data-stu-id="83c4d-121">Right-click the project in Visual Studio's **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="83c4d-122">Sélectionnez l’onglet **Débogage**.</span><span class="sxs-lookup"><span data-stu-id="83c4d-122">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="83c4d-123">Désactivez la case à cocher **activer l’authentification anonyme**.</span><span class="sxs-lookup"><span data-stu-id="83c4d-123">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="83c4d-124">Sélectionnez la case à cocher **activer l’authentification Windows**.</span><span class="sxs-lookup"><span data-stu-id="83c4d-124">Select the check box for **Enable Windows Authentication**.</span></span>

<span data-ttu-id="83c4d-125">Vous pouvez également les propriétés peuvent être configurées dans le `iisSettings` nœud de la *launchSettings.json* fichier :</span><span class="sxs-lookup"><span data-stu-id="83c4d-125">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="83c4d-126">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="83c4d-126">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="83c4d-127">Utilisez le **l’authentification Windows** modèle d’application.</span><span class="sxs-lookup"><span data-stu-id="83c4d-127">Use the **Windows Authentication** app template.</span></span>

<span data-ttu-id="83c4d-128">Exécuter le [dotnet nouvelle](/dotnet/core/tools/dotnet-new) commande avec le `webapp` argument (application de Web ASP.NET Core) et `--auth Windows` commutateur :</span><span class="sxs-lookup"><span data-stu-id="83c4d-128">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```console
dotnet new webapp --auth Windows
```

---

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="83c4d-129">Activer l’authentification Windows avec IIS</span><span class="sxs-lookup"><span data-stu-id="83c4d-129">Enable Windows Authentication with IIS</span></span>

<span data-ttu-id="83c4d-130">IIS utilise le [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) pour héberger des applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="83c4d-130">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="83c4d-131">L’authentification Windows est configurée pour IIS via le *web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="83c4d-131">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="83c4d-132">Les sections suivantes montrent comment :</span><span class="sxs-lookup"><span data-stu-id="83c4d-132">The following sections show how to:</span></span>

* <span data-ttu-id="83c4d-133">Fournir une variable locale *web.config* fichier qui active l’authentification Windows sur le serveur quand l’application est déployée.</span><span class="sxs-lookup"><span data-stu-id="83c4d-133">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="83c4d-134">Utilisez le Gestionnaire des services Internet pour configurer le *web.config* fichier d’une application ASP.NET Core qui a déjà été déployée sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="83c4d-134">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

### <a name="iis-configuration"></a><span data-ttu-id="83c4d-135">Configuration d’IIS</span><span class="sxs-lookup"><span data-stu-id="83c4d-135">IIS configuration</span></span>

<span data-ttu-id="83c4d-136">Si vous n’avez pas déjà fait, activez IIS pour héberger des applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="83c4d-136">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="83c4d-137">Pour plus d'informations, consultez <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="83c4d-137">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="83c4d-138">Activer le Service de rôle IIS pour l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="83c4d-138">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="83c4d-139">Pour plus d’informations, consultez [activer l’authentification Windows dans les Services de rôle IIS (voir étape 2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="83c4d-139">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="83c4d-140">Intergiciel d’intégration IIS est configuré pour authentifier les demandes automatiquement par défaut.</span><span class="sxs-lookup"><span data-stu-id="83c4d-140">IIS Integration Middleware is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="83c4d-141">Pour plus d’informations, consultez [héberger ASP.NET Core sur Windows avec IIS : Options d’IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="83c4d-141">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="83c4d-142">Le Module ASP.NET Core est configuré pour transférer le jeton d’authentification de Windows à l’application par défaut.</span><span class="sxs-lookup"><span data-stu-id="83c4d-142">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="83c4d-143">Pour plus d’informations, consultez [référence de configuration de Module ASP.NET Core : Attributs de l’élément aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="83c4d-143">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="83c4d-144">Créer un nouveau site IIS</span><span class="sxs-lookup"><span data-stu-id="83c4d-144">Create a new IIS site</span></span>

<span data-ttu-id="83c4d-145">Spécifiez un nom et le dossier et lui permettre de créer un nouveau pool d’applications.</span><span class="sxs-lookup"><span data-stu-id="83c4d-145">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="enable-windows-authentication-for-the-app-in-iis"></a><span data-ttu-id="83c4d-146">Activer l’authentification Windows pour l’application dans IIS</span><span class="sxs-lookup"><span data-stu-id="83c4d-146">Enable Windows Authentication for the app in IIS</span></span>

<span data-ttu-id="83c4d-147">Utilisez **soit** des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="83c4d-147">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="83c4d-148">[Configuration de développement côté avant de publier l’application](#development-side-configuration-with-a-local-webconfig-file) (*recommandé*)</span><span class="sxs-lookup"><span data-stu-id="83c4d-148">[Development-side configuration before publishing the app](#development-side-configuration-with-a-local-webconfig-file) (*Recommended*)</span></span>
* [<span data-ttu-id="83c4d-149">Configuration côté serveur après la publication de l’application</span><span class="sxs-lookup"><span data-stu-id="83c4d-149">Server-side configuration after publishing the app</span></span>](#server-side-configuration-with-the-iis-manager)

#### <a name="development-side-configuration-with-a-local-webconfig-file"></a><span data-ttu-id="83c4d-150">Configuration de développement côté avec un fichier web.config local</span><span class="sxs-lookup"><span data-stu-id="83c4d-150">Development-side configuration with a local web.config file</span></span>

<span data-ttu-id="83c4d-151">Procédez comme suit **avant** vous [publier et déployer votre projet](#publish-and-deploy-your-project-to-the-iis-site-folder).</span><span class="sxs-lookup"><span data-stu-id="83c4d-151">Perform the following steps **before** you [publish and deploy your project](#publish-and-deploy-your-project-to-the-iis-site-folder).</span></span>

<span data-ttu-id="83c4d-152">Ajoutez le code suivant *web.config* fichier à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="83c4d-152">Add the following *web.config* file to the project root:</span></span>

[!code-xml[](windowsauth/sample_snapshot/web_2.config)]

<span data-ttu-id="83c4d-153">Lorsque le projet est publié par le Kit de développement logiciel (sans le `<IsTransformWebConfigDisabled>` propriété définie sur `true` dans le fichier projet), publié *web.config* fichier inclut le `<location><system.webServer><security><authentication>` section.</span><span class="sxs-lookup"><span data-stu-id="83c4d-153">When the project is published by the SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="83c4d-154">Pour plus d’informations sur la `<IsTransformWebConfigDisabled>` propriété, consultez <xref:host-and-deploy/iis/index#webconfig-file>.</span><span class="sxs-lookup"><span data-stu-id="83c4d-154">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

#### <a name="server-side-configuration-with-the-iis-manager"></a><span data-ttu-id="83c4d-155">Configuration côté serveur avec le Gestionnaire des services Internet</span><span class="sxs-lookup"><span data-stu-id="83c4d-155">Server-side configuration with the IIS Manager</span></span>

<span data-ttu-id="83c4d-156">Procédez comme suit **après** vous [publier et déployer votre projet](#publish-and-deploy-your-project-to-the-iis-site-folder).</span><span class="sxs-lookup"><span data-stu-id="83c4d-156">Perform the following steps **after** you [publish and deploy your project](#publish-and-deploy-your-project-to-the-iis-site-folder).</span></span>

1. <span data-ttu-id="83c4d-157">Dans le Gestionnaire des services Internet, sélectionnez le site IIS sous la **Sites** nœud de la **connexions** encadré.</span><span class="sxs-lookup"><span data-stu-id="83c4d-157">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
1. <span data-ttu-id="83c4d-158">Double-cliquez sur **authentification** dans le **IIS** zone.</span><span class="sxs-lookup"><span data-stu-id="83c4d-158">Double-click **Authentication** in the **IIS** area.</span></span>
1. <span data-ttu-id="83c4d-159">Sélectionnez **l’authentification anonyme**.</span><span class="sxs-lookup"><span data-stu-id="83c4d-159">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="83c4d-160">Sélectionnez **désactiver** dans le **Actions** encadré.</span><span class="sxs-lookup"><span data-stu-id="83c4d-160">Select **Disable** in the **Actions** sidebar.</span></span>
1. <span data-ttu-id="83c4d-161">Sélectionnez **l’authentification Windows**.</span><span class="sxs-lookup"><span data-stu-id="83c4d-161">Select **Windows Authentication**.</span></span> <span data-ttu-id="83c4d-162">Sélectionnez **activer** dans le **Actions** encadré.</span><span class="sxs-lookup"><span data-stu-id="83c4d-162">Select **Enable** in the **Actions** sidebar.</span></span>

<span data-ttu-id="83c4d-163">Lorsque ces actions sont effectuées, le Gestionnaire des services Internet modifie l’application *web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="83c4d-163">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="83c4d-164">Un `<system.webServer><security><authentication>` nœud est ajouté avec les paramètres mis à jour pour `anonymousAuthentication` et `windowsAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="83c4d-164">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

[!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

<span data-ttu-id="83c4d-165">Le `<system.webServer>` section ajoutée à la *web.config* fichier par le Gestionnaire des services Internet se trouve en dehors de l’application `<location>` section ajoutée par le SDK .NET Core lors de l’application est publiée.</span><span class="sxs-lookup"><span data-stu-id="83c4d-165">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="83c4d-166">Étant donné que la section est ajoutée à l’extérieur de la `<location>` nœud, les paramètres sont hérités par les [sous-applications](xref:host-and-deploy/iis/index#sub-applications) à l’application actuelle.</span><span class="sxs-lookup"><span data-stu-id="83c4d-166">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="83c4d-167">Pour empêcher l’héritage, déplacer la `<security>` section à l’intérieur de la `<location><system.webServer>` section du Kit de développement SDK.</span><span class="sxs-lookup"><span data-stu-id="83c4d-167">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the SDK provided.</span></span>

<span data-ttu-id="83c4d-168">Lorsque le Gestionnaire des services Internet est utilisé pour ajouter la configuration d’IIS, il affecte uniquement l’application *web.config* fichier sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="83c4d-168">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="83c4d-169">Un autre déploiement de l’application peut remplacer les paramètres sur le serveur si la copie du serveur de *web.config* est remplacé par le projet *web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="83c4d-169">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="83c4d-170">Utilisez **soit** des approches suivantes pour gérer les paramètres :</span><span class="sxs-lookup"><span data-stu-id="83c4d-170">Use **either** of the following approaches to manage the settings:</span></span>

* <span data-ttu-id="83c4d-171">Utilisez le Gestionnaire IIS pour réinitialiser les paramètres dans le *web.config* une fois que le fichier est remplacé sur le déploiement de fichiers.</span><span class="sxs-lookup"><span data-stu-id="83c4d-171">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
* <span data-ttu-id="83c4d-172">Ajouter un *fichier web.config* à l’application localement avec les paramètres.</span><span class="sxs-lookup"><span data-stu-id="83c4d-172">Add a *web.config file* to the app locally with the settings.</span></span> <span data-ttu-id="83c4d-173">Pour plus d’informations, consultez le [configuration développement côté](#development-side-configuration-with-a-local-webconfig-file) section.</span><span class="sxs-lookup"><span data-stu-id="83c4d-173">For more information, see the [Development-side configuration](#development-side-configuration-with-a-local-webconfig-file) section.</span></span>

### <a name="publish-and-deploy-your-project-to-the-iis-site-folder"></a><span data-ttu-id="83c4d-174">Publier et déployer votre projet dans le dossier de site IIS</span><span class="sxs-lookup"><span data-stu-id="83c4d-174">Publish and deploy your project to the IIS site folder</span></span>

<span data-ttu-id="83c4d-175">À l’aide de Visual Studio ou l’interface CLI .NET Core, publier et déployer l’application dans le dossier de destination.</span><span class="sxs-lookup"><span data-stu-id="83c4d-175">Using Visual Studio or the .NET Core CLI, publish and deploy the app to the destination folder.</span></span>

<span data-ttu-id="83c4d-176">Pour plus d’informations sur l’hébergement avec IIS, la publication et déploiement, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="83c4d-176">For more information on hosting with IIS, publishing, and deployment, see the following topics:</span></span>

* [<span data-ttu-id="83c4d-177">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="83c4d-177">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>

<span data-ttu-id="83c4d-178">Lancez l’application pour vérifier que l’utilisation de l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="83c4d-178">Launch the app to verify Windows Authentication is working.</span></span>

## <a name="enable-windows-authentication-with-httpsys"></a><span data-ttu-id="83c4d-179">Activer l’authentification Windows avec HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="83c4d-179">Enable Windows Authentication with HTTP.sys</span></span>

<span data-ttu-id="83c4d-180">Bien que Kestrel ne prend pas en charge l’authentification Windows, vous pouvez utiliser [HTTP.sys](xref:fundamentals/servers/httpsys) pour prendre en charge les scénarios auto-hébergés sur Windows.</span><span class="sxs-lookup"><span data-stu-id="83c4d-180">Although Kestrel doesn't support Windows Authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="83c4d-181">L’exemple suivant configure un hôte web de l’application pour utiliser HTTP.sys avec l’authentification Windows :</span><span class="sxs-lookup"><span data-stu-id="83c4d-181">The following example configures the app's web host to use HTTP.sys with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Program.cs?highlight=9-14)]

> [!NOTE]
> <span data-ttu-id="83c4d-182">HTTP.sys délègue l’authentification en mode noyau avec le protocole d’authentification Kerberos.</span><span class="sxs-lookup"><span data-stu-id="83c4d-182">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="83c4d-183">L’authentification en mode utilisateur n’est pas prise en charge avec Kerberos et HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="83c4d-183">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="83c4d-184">Le compte d’ordinateur doit être utilisé pour déchiffrer le ticket/jeton Kerberos obtenu à partir d’Active Directory et transféré par le client au serveur afin d’authentifier l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="83c4d-184">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="83c4d-185">Inscrivez le nom de principal du service (SPN) pour l’hôte, et non l’utilisateur de l’application.</span><span class="sxs-lookup"><span data-stu-id="83c4d-185">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="83c4d-186">HTTP.sys n’est pas pris en charge sur Nano Server version 1709 ou ultérieure.</span><span class="sxs-lookup"><span data-stu-id="83c4d-186">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="83c4d-187">Pour utiliser l’authentification Windows et HTTP.sys avec Nano Server, utilisez un [conteneur de Server Core (microsoft/windowsservercore)](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="83c4d-187">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="83c4d-188">Pour plus d’informations sur Server Core, consultez [quel est l’option d’installation Server Core de Windows Server ?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="83c4d-188">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="work-with-windows-authentication"></a><span data-ttu-id="83c4d-189">Utiliser l’authentification Windows</span><span class="sxs-lookup"><span data-stu-id="83c4d-189">Work with Windows Authentication</span></span>

<span data-ttu-id="83c4d-190">L’état de configuration de l’accès anonyme détermine la façon dont le `[Authorize]` et `[AllowAnonymous]` attributs sont utilisés dans l’application.</span><span class="sxs-lookup"><span data-stu-id="83c4d-190">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="83c4d-191">Les deux sections suivantes expliquent comment gérer les États de configuration non autorisés et autorisées de l’accès anonyme.</span><span class="sxs-lookup"><span data-stu-id="83c4d-191">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="83c4d-192">Interdire l’accès anonyme</span><span class="sxs-lookup"><span data-stu-id="83c4d-192">Disallow anonymous access</span></span>

<span data-ttu-id="83c4d-193">Lorsque l’authentification Windows est activée et que l’accès anonyme est désactivé, le `[Authorize]` et `[AllowAnonymous]` attributs n’ont aucun effet.</span><span class="sxs-lookup"><span data-stu-id="83c4d-193">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="83c4d-194">Si le site IIS (ou HTTP.sys) est configuré pour interdire l’accès anonyme, la demande n’atteint jamais votre application.</span><span class="sxs-lookup"><span data-stu-id="83c4d-194">If the IIS site (or HTTP.sys) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="83c4d-195">Pour cette raison, le `[AllowAnonymous]` attribut n’est pas applicable.</span><span class="sxs-lookup"><span data-stu-id="83c4d-195">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="83c4d-196">Autoriser l’accès anonyme</span><span class="sxs-lookup"><span data-stu-id="83c4d-196">Allow anonymous access</span></span>

<span data-ttu-id="83c4d-197">Lorsque l’authentification Windows et l’accès anonyme sont activées, utiliser le `[Authorize]` et `[AllowAnonymous]` attributs.</span><span class="sxs-lookup"><span data-stu-id="83c4d-197">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="83c4d-198">Le `[Authorize]` attribut vous permet de sécuriser les éléments de l’application qui véritablement nécessitent l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="83c4d-198">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows Authentication.</span></span> <span data-ttu-id="83c4d-199">Le `[AllowAnonymous]` substitutions d’attribut `[Authorize]` attribut l’utilisation dans les applications qui permettent l’accès anonyme.</span><span class="sxs-lookup"><span data-stu-id="83c4d-199">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="83c4d-200">Consultez [autorisation Simple](xref:security/authorization/simple) pour des détails d’utilisation de l’attribut.</span><span class="sxs-lookup"><span data-stu-id="83c4d-200">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="83c4d-201">Dans ASP.NET Core 2.x, le `[Authorize]` attribut nécessite une configuration supplémentaire dans *Startup.cs* en question les demandes anonymes pour l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="83c4d-201">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows Authentication.</span></span> <span data-ttu-id="83c4d-202">La configuration recommandée varie légèrement selon le serveur web utilisé.</span><span class="sxs-lookup"><span data-stu-id="83c4d-202">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="83c4d-203">Par défaut, les utilisateurs qui ne disposent pas d’autorisation pour accéder à une page sont présentées avec une réponse HTTP 403 vide.</span><span class="sxs-lookup"><span data-stu-id="83c4d-203">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="83c4d-204">Le [StatusCodePages intergiciel (middleware)](xref:fundamentals/error-handling#configure-status-code-pages) peut être configuré pour fournir aux utilisateurs une meilleure expérience « Accès refusé ».</span><span class="sxs-lookup"><span data-stu-id="83c4d-204">The [StatusCodePages middleware](xref:fundamentals/error-handling#configure-status-code-pages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="83c4d-205">IIS</span><span class="sxs-lookup"><span data-stu-id="83c4d-205">IIS</span></span>

<span data-ttu-id="83c4d-206">Si vous utilisez IIS, ajoutez le code suivant à la `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="83c4d-206">If using IIS, add the following to the `ConfigureServices` method:</span></span>

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="83c4d-207">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="83c4d-207">HTTP.sys</span></span>

<span data-ttu-id="83c4d-208">Si vous utilisez HTTP.sys, ajoutez le code suivant à la `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="83c4d-208">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="83c4d-209">Emprunt d'identité</span><span class="sxs-lookup"><span data-stu-id="83c4d-209">Impersonation</span></span>

<span data-ttu-id="83c4d-210">ASP.NET Core n’implémente pas l’emprunt d’identité.</span><span class="sxs-lookup"><span data-stu-id="83c4d-210">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="83c4d-211">Applications s’exécutent avec l’identité d’application pour toutes les demandes, à l’aide d’identité de pool ou les processus de l’application.</span><span class="sxs-lookup"><span data-stu-id="83c4d-211">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="83c4d-212">Si vous avez besoin effectuer explicitement une action de la part d’un utilisateur, utilisez [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) dans un [intergiciel (middleware) inline terminal](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="83c4d-212">If you need to explicitly perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="83c4d-213">Exécuter une action unique dans ce contexte, puis fermez le contexte.</span><span class="sxs-lookup"><span data-stu-id="83c4d-213">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

<span data-ttu-id="83c4d-214">`RunImpersonated` ne prend pas en charge les opérations asynchrones et ne doit pas être utilisé pour les scénarios complexes.</span><span class="sxs-lookup"><span data-stu-id="83c4d-214">`RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="83c4d-215">Par exemple, encapsulant les demandes entières ou des chaînes d’intergiciel (middleware) n’est pas pris en charge ni recommandé.</span><span class="sxs-lookup"><span data-stu-id="83c4d-215">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>
