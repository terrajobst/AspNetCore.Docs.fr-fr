---
title: Configurer l’authentification Windows dans ASP.NET Core
author: scottaddie
description: Découvrez comment configurer l’authentification Windows dans ASP.NET Core, à l’aide d’IIS Express, IIS et HTTP.sys.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 01/15/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: c98bdedcf943a9057c96a8e5d62615e400074899
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341652"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a><span data-ttu-id="f9387-103">Configurer l’authentification Windows dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f9387-103">Configure Windows Authentication in ASP.NET Core</span></span>

<span data-ttu-id="f9387-104">Par [Scott Addie](https://twitter.com/Scott_Addie) et [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f9387-104">By [Scott Addie](https://twitter.com/Scott_Addie) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f9387-105">[L’authentification Windows](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) peut être configuré pour les applications ASP.NET Core hébergées avec [IIS](xref:host-and-deploy/iis/index) ou [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="f9387-105">[Windows Authentication](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) can be configured for ASP.NET Core apps hosted with [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="f9387-106">L’authentification Windows s’appuie sur le système d’exploitation pour authentifier les utilisateurs des applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f9387-106">Windows Authentication relies on the operating system to authenticate users of ASP.NET Core apps.</span></span> <span data-ttu-id="f9387-107">Vous pouvez utiliser l’authentification Windows lorsque votre serveur s’exécute sur un réseau d’entreprise à l’aide d’identités de domaine Active Directory ou les comptes Windows pour identifier les utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="f9387-107">You can use Windows Authentication when your server runs on a corporate network using Active Directory domain identities or Windows accounts to identify users.</span></span> <span data-ttu-id="f9387-108">L’authentification Windows est mieux adaptée aux environnements intranet où les utilisateurs, les applications clientes et les serveurs web appartiennent au même domaine Windows.</span><span class="sxs-lookup"><span data-stu-id="f9387-108">Windows Authentication is best suited to intranet environments where users, client apps, and web servers belong to the same Windows domain.</span></span>

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a><span data-ttu-id="f9387-109">Activer l’authentification Windows dans une application ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f9387-109">Enable Windows Authentication in an ASP.NET Core app</span></span>

<span data-ttu-id="f9387-110">Le **Web Application** modèle disponible via Visual Studio ou l’interface CLI .NET Core peut être configuré pour prendre en charge l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="f9387-110">The **Web Application** template available via Visual Studio or the .NET Core CLI can be configured to support Windows Authentication.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f9387-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f9387-111">Visual Studio</span></span>](#tab/visual-studio)

### <a name="use-the-windows-authentication-app-template-for-a-new-project"></a><span data-ttu-id="f9387-112">Utiliser le modèle d’application de l’authentification Windows pour un nouveau projet</span><span class="sxs-lookup"><span data-stu-id="f9387-112">Use the Windows Authentication app template for a new project</span></span>

<span data-ttu-id="f9387-113">Dans Visual Studio :</span><span class="sxs-lookup"><span data-stu-id="f9387-113">In Visual Studio:</span></span>

1. <span data-ttu-id="f9387-114">Créer un nouveau **Application Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="f9387-114">Create a new **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="f9387-115">Sélectionnez **Web Application** à partir de la liste des modèles.</span><span class="sxs-lookup"><span data-stu-id="f9387-115">Select **Web Application** from the list of templates.</span></span>
1. <span data-ttu-id="f9387-116">Sélectionnez le **modifier l’authentification** bouton et sélectionnez **l’authentification Windows**.</span><span class="sxs-lookup"><span data-stu-id="f9387-116">Select the **Change Authentication** button and select **Windows Authentication**.</span></span>

<span data-ttu-id="f9387-117">Exécuter l’application.</span><span class="sxs-lookup"><span data-stu-id="f9387-117">Run the app.</span></span> <span data-ttu-id="f9387-118">Le nom d’utilisateur s’affiche dans l’interface utilisateur de l’application rendue.</span><span class="sxs-lookup"><span data-stu-id="f9387-118">The username appears in the rendered app's user interface.</span></span>

### <a name="manual-configuration-for-an-existing-project"></a><span data-ttu-id="f9387-119">Configuration manuelle pour un projet existant</span><span class="sxs-lookup"><span data-stu-id="f9387-119">Manual configuration for an existing project</span></span>

<span data-ttu-id="f9387-120">Les propriétés du projet vous autorise à activer l’authentification Windows et désactivez l’authentification anonyme :</span><span class="sxs-lookup"><span data-stu-id="f9387-120">The project's properties allow you to enable Windows Authentication and disable Anonymous Authentication:</span></span>

1. <span data-ttu-id="f9387-121">Cliquez sur le projet dans Visual Studio **l’Explorateur de solutions** et sélectionnez **propriétés**.</span><span class="sxs-lookup"><span data-stu-id="f9387-121">Right-click the project in Visual Studio's **Solution Explorer** and select **Properties**.</span></span>
1. <span data-ttu-id="f9387-122">Sélectionnez l’onglet **Débogage**.</span><span class="sxs-lookup"><span data-stu-id="f9387-122">Select the **Debug** tab.</span></span>
1. <span data-ttu-id="f9387-123">Désactivez la case à cocher **activer l’authentification anonyme**.</span><span class="sxs-lookup"><span data-stu-id="f9387-123">Clear the check box for **Enable Anonymous Authentication**.</span></span>
1. <span data-ttu-id="f9387-124">Sélectionnez la case à cocher **activer l’authentification Windows**.</span><span class="sxs-lookup"><span data-stu-id="f9387-124">Select the check box for **Enable Windows Authentication**.</span></span>

<span data-ttu-id="f9387-125">Vous pouvez également les propriétés peuvent être configurées dans le `iisSettings` nœud de la *launchSettings.json* fichier :</span><span class="sxs-lookup"><span data-stu-id="f9387-125">Alternatively, the properties can be configured in the `iisSettings` node of the *launchSettings.json* file:</span></span>

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f9387-126">CLI .NET Core</span><span class="sxs-lookup"><span data-stu-id="f9387-126">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f9387-127">Utilisez le **l’authentification Windows** modèle d’application.</span><span class="sxs-lookup"><span data-stu-id="f9387-127">Use the **Windows Authentication** app template.</span></span>

<span data-ttu-id="f9387-128">Exécuter le [dotnet nouvelle](/dotnet/core/tools/dotnet-new) commande avec le `webapp` argument (application de Web ASP.NET Core) et `--auth Windows` commutateur :</span><span class="sxs-lookup"><span data-stu-id="f9387-128">Execute the [dotnet new](/dotnet/core/tools/dotnet-new) command with the `webapp` argument (ASP.NET Core Web App) and `--auth Windows` switch:</span></span>

```console
dotnet new webapp --auth Windows
```

---

<span data-ttu-id="f9387-129">Lorsque vous modifiez un projet existant, vérifiez que le fichier projet contient une référence de package pour le [Microsoft.AspNetCore.App métapackage](xref:fundamentals/metapackage-app) **ou** le [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) package NuGet.</span><span class="sxs-lookup"><span data-stu-id="f9387-129">When modifying an existing project, confirm that the project file includes a package reference for the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) **or** the [Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) NuGet package.</span></span>

## <a name="enable-windows-authentication-with-iis"></a><span data-ttu-id="f9387-130">Activer l’authentification Windows avec IIS</span><span class="sxs-lookup"><span data-stu-id="f9387-130">Enable Windows Authentication with IIS</span></span>

<span data-ttu-id="f9387-131">IIS utilise le [Module ASP.NET Core](xref:host-and-deploy/aspnet-core-module) pour héberger des applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f9387-131">IIS uses the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to host ASP.NET Core apps.</span></span> <span data-ttu-id="f9387-132">L’authentification Windows est configurée pour IIS via le *web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="f9387-132">Windows Authentication is configured for IIS via the *web.config* file.</span></span> <span data-ttu-id="f9387-133">Les sections suivantes montrent comment :</span><span class="sxs-lookup"><span data-stu-id="f9387-133">The following sections show how to:</span></span>

* <span data-ttu-id="f9387-134">Fournir une variable locale *web.config* fichier qui active l’authentification Windows sur le serveur quand l’application est déployée.</span><span class="sxs-lookup"><span data-stu-id="f9387-134">Provide a local *web.config* file that activates Windows Authentication on the server when the app is deployed.</span></span>
* <span data-ttu-id="f9387-135">Utilisez le Gestionnaire des services Internet pour configurer le *web.config* fichier d’une application ASP.NET Core qui a déjà été déployée sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f9387-135">Use the IIS Manager to configure the *web.config* file of an ASP.NET Core app that has already been deployed to the server.</span></span>

### <a name="iis-configuration"></a><span data-ttu-id="f9387-136">Configuration d’IIS</span><span class="sxs-lookup"><span data-stu-id="f9387-136">IIS configuration</span></span>

<span data-ttu-id="f9387-137">Si vous n’avez pas déjà fait, activez IIS pour héberger des applications ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f9387-137">If you haven't already done so, enable IIS to host ASP.NET Core apps.</span></span> <span data-ttu-id="f9387-138">Pour plus d'informations, consultez <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="f9387-138">For more information, see <xref:host-and-deploy/iis/index>.</span></span>

<span data-ttu-id="f9387-139">Activer le Service de rôle IIS pour l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="f9387-139">Enable the IIS Role Service for Windows Authentication.</span></span> <span data-ttu-id="f9387-140">Pour plus d’informations, consultez [activer l’authentification Windows dans les Services de rôle IIS (voir étape 2)](xref:host-and-deploy/iis/index#iis-configuration).</span><span class="sxs-lookup"><span data-stu-id="f9387-140">For more information, see [Enable Windows Authentication in IIS Role Services (see Step 2)](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

<span data-ttu-id="f9387-141">Intergiciel d’intégration IIS est configuré pour authentifier les demandes automatiquement par défaut.</span><span class="sxs-lookup"><span data-stu-id="f9387-141">IIS Integration Middleware is configured to automatically authenticate requests by default.</span></span> <span data-ttu-id="f9387-142">Pour plus d’informations, consultez [héberger ASP.NET Core sur Windows avec IIS : Options d’IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="f9387-142">For more information, see [Host ASP.NET Core on Windows with IIS: IIS options (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="f9387-143">Le Module ASP.NET Core est configuré pour transférer le jeton d’authentification de Windows à l’application par défaut.</span><span class="sxs-lookup"><span data-stu-id="f9387-143">The ASP.NET Core Module is configured to forward the Windows Authentication token to the app by default.</span></span> <span data-ttu-id="f9387-144">Pour plus d’informations, consultez [référence de configuration de Module ASP.NET Core : Attributs de l’élément aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span><span class="sxs-lookup"><span data-stu-id="f9387-144">For more information, see [ASP.NET Core Module configuration reference: Attributes of the aspNetCore element](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).</span></span>

### <a name="create-a-new-iis-site"></a><span data-ttu-id="f9387-145">Créer un nouveau site IIS</span><span class="sxs-lookup"><span data-stu-id="f9387-145">Create a new IIS site</span></span>

<span data-ttu-id="f9387-146">Spécifiez un nom et le dossier et lui permettre de créer un nouveau pool d’applications.</span><span class="sxs-lookup"><span data-stu-id="f9387-146">Specify a name and folder and allow it to create a new application pool.</span></span>

### <a name="enable-windows-authentication-for-the-app-in-iis"></a><span data-ttu-id="f9387-147">Activer l’authentification Windows pour l’application dans IIS</span><span class="sxs-lookup"><span data-stu-id="f9387-147">Enable Windows Authentication for the app in IIS</span></span>

<span data-ttu-id="f9387-148">Utilisez **soit** des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="f9387-148">Use **either** of the following approaches:</span></span>

* <span data-ttu-id="f9387-149">[Configuration de développement côté avant de publier l’application](#development-side-configuration-with-a-local-webconfig-file) (*recommandé*)</span><span class="sxs-lookup"><span data-stu-id="f9387-149">[Development-side configuration before publishing the app](#development-side-configuration-with-a-local-webconfig-file) (*Recommended*)</span></span>
* [<span data-ttu-id="f9387-150">Configuration côté serveur après la publication de l’application</span><span class="sxs-lookup"><span data-stu-id="f9387-150">Server-side configuration after publishing the app</span></span>](#server-side-configuration-with-the-iis-manager)

#### <a name="development-side-configuration-with-a-local-webconfig-file"></a><span data-ttu-id="f9387-151">Configuration de développement côté avec un fichier web.config local</span><span class="sxs-lookup"><span data-stu-id="f9387-151">Development-side configuration with a local web.config file</span></span>

<span data-ttu-id="f9387-152">Procédez comme suit **avant** vous [publier et déployer votre projet](#publish-and-deploy-your-project-to-the-iis-site-folder).</span><span class="sxs-lookup"><span data-stu-id="f9387-152">Perform the following steps **before** you [publish and deploy your project](#publish-and-deploy-your-project-to-the-iis-site-folder).</span></span>

<span data-ttu-id="f9387-153">Ajoutez le code suivant *web.config* fichier à la racine du projet :</span><span class="sxs-lookup"><span data-stu-id="f9387-153">Add the following *web.config* file to the project root:</span></span>

[!code-xml[](windowsauth/sample_snapshot/web_2.config)]

<span data-ttu-id="f9387-154">Lorsque le projet est publié par le Kit de développement logiciel (sans le `<IsTransformWebConfigDisabled>` propriété définie sur `true` dans le fichier projet), publié *web.config* fichier inclut le `<location><system.webServer><security><authentication>` section.</span><span class="sxs-lookup"><span data-stu-id="f9387-154">When the project is published by the SDK (without the `<IsTransformWebConfigDisabled>` property set to `true` in the project file), the published *web.config* file includes the `<location><system.webServer><security><authentication>` section.</span></span> <span data-ttu-id="f9387-155">Pour plus d’informations sur la `<IsTransformWebConfigDisabled>` propriété, consultez <xref:host-and-deploy/iis/index#webconfig-file>.</span><span class="sxs-lookup"><span data-stu-id="f9387-155">For more information on the `<IsTransformWebConfigDisabled>` property, see <xref:host-and-deploy/iis/index#webconfig-file>.</span></span>

#### <a name="server-side-configuration-with-the-iis-manager"></a><span data-ttu-id="f9387-156">Configuration côté serveur avec le Gestionnaire des services Internet</span><span class="sxs-lookup"><span data-stu-id="f9387-156">Server-side configuration with the IIS Manager</span></span>

<span data-ttu-id="f9387-157">Procédez comme suit **après** vous [publier et déployer votre projet](#publish-and-deploy-your-project-to-the-iis-site-folder).</span><span class="sxs-lookup"><span data-stu-id="f9387-157">Perform the following steps **after** you [publish and deploy your project](#publish-and-deploy-your-project-to-the-iis-site-folder).</span></span>

1. <span data-ttu-id="f9387-158">Dans le Gestionnaire des services Internet, sélectionnez le site IIS sous la **Sites** nœud de la **connexions** encadré.</span><span class="sxs-lookup"><span data-stu-id="f9387-158">In IIS Manager, select the IIS site under the **Sites** node of the **Connections** sidebar.</span></span>
1. <span data-ttu-id="f9387-159">Double-cliquez sur **authentification** dans le **IIS** zone.</span><span class="sxs-lookup"><span data-stu-id="f9387-159">Double-click **Authentication** in the **IIS** area.</span></span>
1. <span data-ttu-id="f9387-160">Sélectionnez **l’authentification anonyme**.</span><span class="sxs-lookup"><span data-stu-id="f9387-160">Select **Anonymous Authentication**.</span></span> <span data-ttu-id="f9387-161">Sélectionnez **désactiver** dans le **Actions** encadré.</span><span class="sxs-lookup"><span data-stu-id="f9387-161">Select **Disable** in the **Actions** sidebar.</span></span>
1. <span data-ttu-id="f9387-162">Sélectionnez **l’authentification Windows**.</span><span class="sxs-lookup"><span data-stu-id="f9387-162">Select **Windows Authentication**.</span></span> <span data-ttu-id="f9387-163">Sélectionnez **activer** dans le **Actions** encadré.</span><span class="sxs-lookup"><span data-stu-id="f9387-163">Select **Enable** in the **Actions** sidebar.</span></span>

<span data-ttu-id="f9387-164">Lorsque ces actions sont effectuées, le Gestionnaire des services Internet modifie l’application *web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="f9387-164">When these actions are taken, IIS Manager modifies the app's *web.config* file.</span></span> <span data-ttu-id="f9387-165">Un `<system.webServer><security><authentication>` nœud est ajouté avec les paramètres mis à jour pour `anonymousAuthentication` et `windowsAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="f9387-165">A `<system.webServer><security><authentication>` node is added with updated settings for `anonymousAuthentication` and `windowsAuthentication`:</span></span>

[!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

<span data-ttu-id="f9387-166">Le `<system.webServer>` section ajoutée à la *web.config* fichier par le Gestionnaire des services Internet se trouve en dehors de l’application `<location>` section ajoutée par le SDK .NET Core lors de l’application est publiée.</span><span class="sxs-lookup"><span data-stu-id="f9387-166">The `<system.webServer>` section added to the *web.config* file by IIS Manager is outside of the app's `<location>` section added by the .NET Core SDK when the app is published.</span></span> <span data-ttu-id="f9387-167">Étant donné que la section est ajoutée à l’extérieur de la `<location>` nœud, les paramètres sont hérités par les [sous-applications](xref:host-and-deploy/iis/index#sub-applications) à l’application actuelle.</span><span class="sxs-lookup"><span data-stu-id="f9387-167">Because the section is added outside of the `<location>` node, the settings are inherited by any [sub-apps](xref:host-and-deploy/iis/index#sub-applications) to the current app.</span></span> <span data-ttu-id="f9387-168">Pour empêcher l’héritage, déplacer la `<security>` section à l’intérieur de la `<location><system.webServer>` section du Kit de développement SDK.</span><span class="sxs-lookup"><span data-stu-id="f9387-168">To prevent inheritance, move the added `<security>` section inside of the `<location><system.webServer>` section that the SDK provided.</span></span>

<span data-ttu-id="f9387-169">Lorsque le Gestionnaire des services Internet est utilisé pour ajouter la configuration d’IIS, il affecte uniquement l’application *web.config* fichier sur le serveur.</span><span class="sxs-lookup"><span data-stu-id="f9387-169">When IIS Manager is used to add the IIS configuration, it only affects the app's *web.config* file on the server.</span></span> <span data-ttu-id="f9387-170">Un autre déploiement de l’application peut remplacer les paramètres sur le serveur si la copie du serveur de *web.config* est remplacé par le projet *web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="f9387-170">A subsequent deployment of the app may overwrite the settings on the server if the server's copy of *web.config* is replaced by the project's *web.config* file.</span></span> <span data-ttu-id="f9387-171">Utilisez **soit** des approches suivantes pour gérer les paramètres :</span><span class="sxs-lookup"><span data-stu-id="f9387-171">Use **either** of the following approaches to manage the settings:</span></span>

* <span data-ttu-id="f9387-172">Utilisez le Gestionnaire IIS pour réinitialiser les paramètres dans le *web.config* une fois que le fichier est remplacé sur le déploiement de fichiers.</span><span class="sxs-lookup"><span data-stu-id="f9387-172">Use IIS Manager to reset the settings in the *web.config* file after the file is overwritten on deployment.</span></span>
* <span data-ttu-id="f9387-173">Ajouter un *fichier web.config* à l’application localement avec les paramètres.</span><span class="sxs-lookup"><span data-stu-id="f9387-173">Add a *web.config file* to the app locally with the settings.</span></span> <span data-ttu-id="f9387-174">Pour plus d’informations, consultez le [configuration développement côté](#development-side-configuration-with-a-local-webconfig-file) section.</span><span class="sxs-lookup"><span data-stu-id="f9387-174">For more information, see the [Development-side configuration](#development-side-configuration-with-a-local-webconfig-file) section.</span></span>

### <a name="publish-and-deploy-your-project-to-the-iis-site-folder"></a><span data-ttu-id="f9387-175">Publier et déployer votre projet dans le dossier de site IIS</span><span class="sxs-lookup"><span data-stu-id="f9387-175">Publish and deploy your project to the IIS site folder</span></span>

<span data-ttu-id="f9387-176">À l’aide de Visual Studio ou l’interface CLI .NET Core, publier et déployer l’application dans le dossier de destination.</span><span class="sxs-lookup"><span data-stu-id="f9387-176">Using Visual Studio or the .NET Core CLI, publish and deploy the app to the destination folder.</span></span>

<span data-ttu-id="f9387-177">Pour plus d’informations sur l’hébergement avec IIS, la publication et déploiement, consultez les rubriques suivantes :</span><span class="sxs-lookup"><span data-stu-id="f9387-177">For more information on hosting with IIS, publishing, and deployment, see the following topics:</span></span>

* [<span data-ttu-id="f9387-178">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="f9387-178">dotnet publish</span></span>](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>

<span data-ttu-id="f9387-179">Lancez l’application pour vérifier que l’utilisation de l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="f9387-179">Launch the app to verify Windows Authentication is working.</span></span>

## <a name="enable-windows-authentication-with-httpsys"></a><span data-ttu-id="f9387-180">Activer l’authentification Windows avec HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="f9387-180">Enable Windows Authentication with HTTP.sys</span></span>

<span data-ttu-id="f9387-181">Bien que Kestrel ne prend pas en charge l’authentification Windows, vous pouvez utiliser [HTTP.sys](xref:fundamentals/servers/httpsys) pour prendre en charge les scénarios auto-hébergés sur Windows.</span><span class="sxs-lookup"><span data-stu-id="f9387-181">Although Kestrel doesn't support Windows Authentication, you can use [HTTP.sys](xref:fundamentals/servers/httpsys) to support self-hosted scenarios on Windows.</span></span> <span data-ttu-id="f9387-182">L’exemple suivant configure un hôte web de l’application pour utiliser HTTP.sys avec l’authentification Windows :</span><span class="sxs-lookup"><span data-stu-id="f9387-182">The following example configures the app's web host to use HTTP.sys with Windows Authentication:</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Program.cs?highlight=9-14)]

> [!NOTE]
> <span data-ttu-id="f9387-183">HTTP.sys délègue l’authentification en mode noyau avec le protocole d’authentification Kerberos.</span><span class="sxs-lookup"><span data-stu-id="f9387-183">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="f9387-184">L’authentification en mode utilisateur n’est pas prise en charge avec Kerberos et HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="f9387-184">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="f9387-185">Le compte d’ordinateur doit être utilisé pour déchiffrer le ticket/jeton Kerberos obtenu à partir d’Active Directory et transféré par le client au serveur afin d’authentifier l’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="f9387-185">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="f9387-186">Inscrivez le nom de principal du service (SPN) pour l’hôte, et non l’utilisateur de l’application.</span><span class="sxs-lookup"><span data-stu-id="f9387-186">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

> [!NOTE]
> <span data-ttu-id="f9387-187">HTTP.sys n’est pas pris en charge sur Nano Server version 1709 ou ultérieure.</span><span class="sxs-lookup"><span data-stu-id="f9387-187">HTTP.sys isn't supported on Nano Server version 1709 or later.</span></span> <span data-ttu-id="f9387-188">Pour utiliser l’authentification Windows et HTTP.sys avec Nano Server, utilisez un [conteneur de Server Core (microsoft/windowsservercore)](https://hub.docker.com/r/microsoft/windowsservercore/).</span><span class="sxs-lookup"><span data-stu-id="f9387-188">To use Windows Authentication and HTTP.sys with Nano Server, use a [Server Core (microsoft/windowsservercore) container](https://hub.docker.com/r/microsoft/windowsservercore/).</span></span> <span data-ttu-id="f9387-189">Pour plus d’informations sur Server Core, consultez [quel est l’option d’installation Server Core de Windows Server ?](/windows-server/administration/server-core/what-is-server-core).</span><span class="sxs-lookup"><span data-stu-id="f9387-189">For more information on Server Core, see [What is the Server Core installation option in Windows Server?](/windows-server/administration/server-core/what-is-server-core).</span></span>

## <a name="work-with-windows-authentication"></a><span data-ttu-id="f9387-190">Utiliser l’authentification Windows</span><span class="sxs-lookup"><span data-stu-id="f9387-190">Work with Windows Authentication</span></span>

<span data-ttu-id="f9387-191">L’état de configuration de l’accès anonyme détermine la façon dont le `[Authorize]` et `[AllowAnonymous]` attributs sont utilisés dans l’application.</span><span class="sxs-lookup"><span data-stu-id="f9387-191">The configuration state of anonymous access determines the way in which the `[Authorize]` and `[AllowAnonymous]` attributes are used in the app.</span></span> <span data-ttu-id="f9387-192">Les deux sections suivantes expliquent comment gérer les États de configuration non autorisés et autorisées de l’accès anonyme.</span><span class="sxs-lookup"><span data-stu-id="f9387-192">The following two sections explain how to handle the disallowed and allowed configuration states of anonymous access.</span></span>

### <a name="disallow-anonymous-access"></a><span data-ttu-id="f9387-193">Interdire l’accès anonyme</span><span class="sxs-lookup"><span data-stu-id="f9387-193">Disallow anonymous access</span></span>

<span data-ttu-id="f9387-194">Lorsque l’authentification Windows est activée et que l’accès anonyme est désactivé, le `[Authorize]` et `[AllowAnonymous]` attributs n’ont aucun effet.</span><span class="sxs-lookup"><span data-stu-id="f9387-194">When Windows Authentication is enabled and anonymous access is disabled, the `[Authorize]` and `[AllowAnonymous]` attributes have no effect.</span></span> <span data-ttu-id="f9387-195">Si le site IIS (ou HTTP.sys) est configuré pour interdire l’accès anonyme, la demande n’atteint jamais votre application.</span><span class="sxs-lookup"><span data-stu-id="f9387-195">If the IIS site (or HTTP.sys) is configured to disallow anonymous access, the request never reaches your app.</span></span> <span data-ttu-id="f9387-196">Pour cette raison, le `[AllowAnonymous]` attribut n’est pas applicable.</span><span class="sxs-lookup"><span data-stu-id="f9387-196">For this reason, the `[AllowAnonymous]` attribute isn't applicable.</span></span>

### <a name="allow-anonymous-access"></a><span data-ttu-id="f9387-197">Autoriser l’accès anonyme</span><span class="sxs-lookup"><span data-stu-id="f9387-197">Allow anonymous access</span></span>

<span data-ttu-id="f9387-198">Lorsque l’authentification Windows et l’accès anonyme sont activées, utiliser le `[Authorize]` et `[AllowAnonymous]` attributs.</span><span class="sxs-lookup"><span data-stu-id="f9387-198">When both Windows Authentication and anonymous access are enabled, use the `[Authorize]` and `[AllowAnonymous]` attributes.</span></span> <span data-ttu-id="f9387-199">Le `[Authorize]` attribut vous permet de sécuriser les éléments de l’application qui véritablement nécessitent l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="f9387-199">The `[Authorize]` attribute allows you to secure pieces of the app which truly do require Windows Authentication.</span></span> <span data-ttu-id="f9387-200">Le `[AllowAnonymous]` substitutions d’attribut `[Authorize]` attribut l’utilisation dans les applications qui permettent l’accès anonyme.</span><span class="sxs-lookup"><span data-stu-id="f9387-200">The `[AllowAnonymous]` attribute overrides `[Authorize]` attribute usage within apps which allow anonymous access.</span></span> <span data-ttu-id="f9387-201">Consultez [autorisation Simple](xref:security/authorization/simple) pour des détails d’utilisation de l’attribut.</span><span class="sxs-lookup"><span data-stu-id="f9387-201">See [Simple Authorization](xref:security/authorization/simple) for attribute usage details.</span></span>

<span data-ttu-id="f9387-202">Dans ASP.NET Core 2.x, le `[Authorize]` attribut nécessite une configuration supplémentaire dans *Startup.cs* en question les demandes anonymes pour l’authentification Windows.</span><span class="sxs-lookup"><span data-stu-id="f9387-202">In ASP.NET Core 2.x, the `[Authorize]` attribute requires additional configuration in *Startup.cs* to challenge anonymous requests for Windows Authentication.</span></span> <span data-ttu-id="f9387-203">La configuration recommandée varie légèrement selon le serveur web utilisé.</span><span class="sxs-lookup"><span data-stu-id="f9387-203">The recommended configuration varies slightly based on the web server being used.</span></span>

> [!NOTE]
> <span data-ttu-id="f9387-204">Par défaut, les utilisateurs qui ne disposent pas d’autorisation pour accéder à une page sont présentées avec une réponse HTTP 403 vide.</span><span class="sxs-lookup"><span data-stu-id="f9387-204">By default, users who lack authorization to access a page are presented with an empty HTTP 403 response.</span></span> <span data-ttu-id="f9387-205">Le [StatusCodePages intergiciel (middleware)](xref:fundamentals/error-handling#configure-status-code-pages) peut être configuré pour fournir aux utilisateurs une meilleure expérience « Accès refusé ».</span><span class="sxs-lookup"><span data-stu-id="f9387-205">The [StatusCodePages middleware](xref:fundamentals/error-handling#configure-status-code-pages) can be configured to provide users with a better "Access Denied" experience.</span></span>

#### <a name="iis"></a><span data-ttu-id="f9387-206">IIS</span><span class="sxs-lookup"><span data-stu-id="f9387-206">IIS</span></span>

<span data-ttu-id="f9387-207">Si vous utilisez IIS, ajoutez le code suivant à la `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="f9387-207">If using IIS, add the following to the `ConfigureServices` method:</span></span>

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a><span data-ttu-id="f9387-208">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="f9387-208">HTTP.sys</span></span>

<span data-ttu-id="f9387-209">Si vous utilisez HTTP.sys, ajoutez le code suivant à la `ConfigureServices` méthode :</span><span class="sxs-lookup"><span data-stu-id="f9387-209">If using HTTP.sys, add the following to the `ConfigureServices` method:</span></span>

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a><span data-ttu-id="f9387-210">Emprunt d'identité</span><span class="sxs-lookup"><span data-stu-id="f9387-210">Impersonation</span></span>

<span data-ttu-id="f9387-211">ASP.NET Core n’implémente pas l’emprunt d’identité.</span><span class="sxs-lookup"><span data-stu-id="f9387-211">ASP.NET Core doesn't implement impersonation.</span></span> <span data-ttu-id="f9387-212">Applications s’exécutent avec l’identité d’application pour toutes les demandes, à l’aide d’identité de pool ou les processus de l’application.</span><span class="sxs-lookup"><span data-stu-id="f9387-212">Apps run with the app's identity for all requests, using app pool or process identity.</span></span> <span data-ttu-id="f9387-213">Si vous avez besoin effectuer explicitement une action de la part d’un utilisateur, utilisez [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) dans un [intergiciel (middleware) inline terminal](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) dans `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="f9387-213">If you need to explicitly perform an action on behalf of a user, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) in a [terminal inline middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) in `Startup.Configure`.</span></span> <span data-ttu-id="f9387-214">Exécuter une action unique dans ce contexte, puis fermez le contexte.</span><span class="sxs-lookup"><span data-stu-id="f9387-214">Run a single action in this context and then close the context.</span></span>

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

<span data-ttu-id="f9387-215">`RunImpersonated` ne prend pas en charge les opérations asynchrones et ne doit pas être utilisé pour les scénarios complexes.</span><span class="sxs-lookup"><span data-stu-id="f9387-215">`RunImpersonated` doesn't support asynchronous operations and shouldn't be used for complex scenarios.</span></span> <span data-ttu-id="f9387-216">Par exemple, encapsulant les demandes entières ou des chaînes d’intergiciel (middleware) n’est pas pris en charge ni recommandé.</span><span class="sxs-lookup"><span data-stu-id="f9387-216">For example, wrapping entire requests or middleware chains isn't supported or recommended.</span></span>
