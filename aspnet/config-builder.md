---
uid: config-builder
title: Générateurs de configuration pour ASP.NET
author: rick-anderson
description: Découvrez comment obtenir des données de configuration à partir de sources autres que les valeurs web.config provenant de sources externes.
ms.author: riande
ms.date: 10/29/2018
ms.technology: aspnet
msc.type: content
ms.openlocfilehash: 4dcc62573fad13ec8b37b2c59e884eec7ca80b92
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148835"
---
# <a name="configuration-builders-for-aspnet"></a><span data-ttu-id="38ecf-103">Générateurs de configuration pour ASP.NET</span><span class="sxs-lookup"><span data-stu-id="38ecf-103">Configuration builders for ASP.NET</span></span>

<span data-ttu-id="38ecf-104">Par [Stephen Molloy](https://github.com/StephenMolloy) et [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="38ecf-104">By [Stephen Molloy](https://github.com/StephenMolloy) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="38ecf-105">Générateurs de configuration fournissent un mécanisme modern et agile pour les applications ASP.NET obtenir les valeurs de configuration à partir de sources externes.</span><span class="sxs-lookup"><span data-stu-id="38ecf-105">Configuration builders provide a modern and agile mechanism for ASP.NET apps to get configuration values from external sources.</span></span>

<span data-ttu-id="38ecf-106">Générateurs de configuration :</span><span class="sxs-lookup"><span data-stu-id="38ecf-106">Configuration builders:</span></span>

* <span data-ttu-id="38ecf-107">Sont disponibles dans .NET Framework 4.7.1 et versions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="38ecf-107">Are available in .NET Framework 4.7.1 and later.</span></span>
* <span data-ttu-id="38ecf-108">Fournir un mécanisme flexible pour la lecture des valeurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="38ecf-108">Provide a flexible mechanism for reading configuration values.</span></span>
* <span data-ttu-id="38ecf-109">Résoudre certains besoins fondamentaux des applications dans leur déplacement dans un conteneur et environnement ayant le focus de cloud.</span><span class="sxs-lookup"><span data-stu-id="38ecf-109">Address some of the basic needs of apps as they move into a container and cloud focused environment.</span></span>
* <span data-ttu-id="38ecf-110">Peut être utilisé pour améliorer la protection des données de configuration par dessin à partir de sources précédemment pas disponibles (par exemple, variables d’environnement et d’Azure Key Vault) dans le système de configuration .NET.</span><span class="sxs-lookup"><span data-stu-id="38ecf-110">Can be used to improve protection of configuration data by drawing from sources previously unavailable (for example, Azure Key Vault and environment variables) in the .NET configuration system.</span></span>

## <a name="keyvalue-configuration-builders"></a><span data-ttu-id="38ecf-111">Générateurs de configuration clé/valeur</span><span class="sxs-lookup"><span data-stu-id="38ecf-111">Key/value configuration builders</span></span>

<span data-ttu-id="38ecf-112">Un scénario courant qui peut être géré par les générateurs de configuration est de fournir un mécanisme de remplacement de clé/valeur de base pour les sections de configuration qui suivent un modèle de clé/valeur.</span><span class="sxs-lookup"><span data-stu-id="38ecf-112">A common scenario that can be handled by configuration builders is to provide a basic key/value replacement mechanism for configuration sections that follow a key/value pattern.</span></span> <span data-ttu-id="38ecf-113">Le concept de .NET Framework de ConfigurationBuilders n’est pas limité aux sections de configuration spécifiques ou des modèles.</span><span class="sxs-lookup"><span data-stu-id="38ecf-113">The .NET Framework concept of ConfigurationBuilders is not limited to specific configuration sections or patterns.</span></span> <span data-ttu-id="38ecf-114">Toutefois, un grand nombre des générateurs de configuration dans `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) professionnel dans le modèle de clé/valeur.</span><span class="sxs-lookup"><span data-stu-id="38ecf-114">However, many of the configuration builders in `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) work within the key/value pattern.</span></span>

## <a name="keyvalue-configuration-builders-settings"></a><span data-ttu-id="38ecf-115">Paramètres de générateurs de configuration de clé/valeur</span><span class="sxs-lookup"><span data-stu-id="38ecf-115">Key/value configuration builders settings</span></span>

<span data-ttu-id="38ecf-116">Les paramètres suivants s’appliquent à tous les générateurs de configuration de clé/valeur dans `Microsoft.Configuration.ConfigurationBuilders`.</span><span class="sxs-lookup"><span data-stu-id="38ecf-116">The following settings apply to all key/value configuration builders in `Microsoft.Configuration.ConfigurationBuilders`.</span></span>

### <a name="mode"></a><span data-ttu-id="38ecf-117">Mode</span><span class="sxs-lookup"><span data-stu-id="38ecf-117">Mode</span></span>

<span data-ttu-id="38ecf-118">Les générateurs de configuration utilisent une source externe d’informations de clé/valeur pour remplir les éléments de clé/valeur sélectionnée du système de configuration.</span><span class="sxs-lookup"><span data-stu-id="38ecf-118">The configuration builders use an external source of key/value information to populate selected key/value elements of the configuration system.</span></span> <span data-ttu-id="38ecf-119">Plus précisément, le `<appSettings/>` et `<connectionStrings/>` sections reçoivent un traitement spécial dans les générateurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="38ecf-119">Specifically, the `<appSettings/>` and `<connectionStrings/>` sections receive special treatment from the configuration builders.</span></span> <span data-ttu-id="38ecf-120">Les générateurs de fonctionnent dans trois modes :</span><span class="sxs-lookup"><span data-stu-id="38ecf-120">The builders work in three modes:</span></span>

* <span data-ttu-id="38ecf-121">`Strict` -Le mode par défaut.</span><span class="sxs-lookup"><span data-stu-id="38ecf-121">`Strict` - The default mode.</span></span> <span data-ttu-id="38ecf-122">Dans ce mode, le Générateur de configuration ne fonctionne que sur les sections de configuration clé/valeur-centric bien connu.</span><span class="sxs-lookup"><span data-stu-id="38ecf-122">In this mode, the configuration builder only operates on well-known key/value-centric configuration sections.</span></span> <span data-ttu-id="38ecf-123">`Strict` mode énumère chaque clé dans la section.</span><span class="sxs-lookup"><span data-stu-id="38ecf-123">`Strict` mode enumerates each key in the section.</span></span> <span data-ttu-id="38ecf-124">Si une clé correspondante est trouvée dans la source externe :</span><span class="sxs-lookup"><span data-stu-id="38ecf-124">If a matching key is found in the external source:</span></span>

   * <span data-ttu-id="38ecf-125">Les générateurs de configuration remplacement la valeur dans la section de configuration qui en résulte par la valeur de la source externe.</span><span class="sxs-lookup"><span data-stu-id="38ecf-125">The configuration builders replace the value in the resulting configuration section with the value from the external source.</span></span>
* <span data-ttu-id="38ecf-126">`Greedy` -Ce mode est étroitement lié à `Strict` mode.</span><span class="sxs-lookup"><span data-stu-id="38ecf-126">`Greedy` - This mode is closely related to `Strict` mode.</span></span> <span data-ttu-id="38ecf-127">Au lieu d’être limité aux clés qui existent déjà dans la configuration d’origine :</span><span class="sxs-lookup"><span data-stu-id="38ecf-127">Rather than being limited to keys that already exist in the original configuration:</span></span>

  * <span data-ttu-id="38ecf-128">Les générateurs de configuration ajoute toutes les paires clé/valeur à partir de la source externe dans la section de configuration qui en résulte.</span><span class="sxs-lookup"><span data-stu-id="38ecf-128">The configuration builders adds all key/value pairs from the external source into the resulting configuration section.</span></span>

* <span data-ttu-id="38ecf-129">`Expand` -Opère sur le code XML brut avant qu’il est analysé dans un objet de section de configuration.</span><span class="sxs-lookup"><span data-stu-id="38ecf-129">`Expand` - Operates on the raw XML before it's parsed into a configuration section object.</span></span> <span data-ttu-id="38ecf-130">Elle peut être considérée comme expansion de jetons présents dans une chaîne.</span><span class="sxs-lookup"><span data-stu-id="38ecf-130">It can be thought of as an expansion of tokens in a string.</span></span> <span data-ttu-id="38ecf-131">N’importe quelle partie de la chaîne XML brute qui correspond au modèle `${token}` est un candidat pour l’expansion de jeton.</span><span class="sxs-lookup"><span data-stu-id="38ecf-131">Any part of the raw XML string that matches the pattern `${token}` is a candidate for token expansion.</span></span> <span data-ttu-id="38ecf-132">Si aucune valeur correspondante n’est trouvée dans la source externe, le jeton n’est pas modifié.</span><span class="sxs-lookup"><span data-stu-id="38ecf-132">If no corresponding value is found in the external source, then the token is not changed.</span></span> <span data-ttu-id="38ecf-133">Générateurs dans ce mode ne sont pas limités à la `<appSettings/>` et `<connectionStrings/>` sections.</span><span class="sxs-lookup"><span data-stu-id="38ecf-133">Builders in this mode are not limited to the `<appSettings/>` and `<connectionStrings/>` sections.</span></span>

<span data-ttu-id="38ecf-134">Le balisage suivant à partir de *web.config* permet la [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) dans `Strict` mode :</span><span class="sxs-lookup"><span data-stu-id="38ecf-134">The following markup from *web.config* enables the [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) in `Strict` mode:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

<span data-ttu-id="38ecf-135">Le code suivant lit le `<appSettings/>` et `<connectionStrings/>` indiqué dans l’exemple précédent *web.config* fichier :</span><span class="sxs-lookup"><span data-stu-id="38ecf-135">The following code reads the `<appSettings/>` and `<connectionStrings/>` shown in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

<span data-ttu-id="38ecf-136">Le code précédent définit les valeurs de propriété :</span><span class="sxs-lookup"><span data-stu-id="38ecf-136">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="38ecf-137">Les valeurs dans le *web.config* fichier si les clés ne sont pas définies dans les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="38ecf-137">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="38ecf-138">Les valeurs de l’environnement variable, si définie.</span><span class="sxs-lookup"><span data-stu-id="38ecf-138">The values of the environment variable, if set.</span></span>

<span data-ttu-id="38ecf-139">Par exemple, `ServiceID` contiendra :</span><span class="sxs-lookup"><span data-stu-id="38ecf-139">For example, `ServiceID` will contain:</span></span>

* <span data-ttu-id="38ecf-140">« ServiceID valeur à partir de web.config », si la variable d’environnement `ServiceID` n’est pas définie.</span><span class="sxs-lookup"><span data-stu-id="38ecf-140">"ServiceID value from web.config", if the environment variable `ServiceID` is not set.</span></span>
* <span data-ttu-id="38ecf-141">La valeur de la `ServiceID` if variable d’environnement définie.</span><span class="sxs-lookup"><span data-stu-id="38ecf-141">The value of the `ServiceID` environment variable, if set.</span></span>

<span data-ttu-id="38ecf-142">L’illustration suivante montre le `<appSettings/>` clés/valeurs à partir de l’exemple précédent *web.config* jeu de fichiers dans l’éditeur de l’environnement :</span><span class="sxs-lookup"><span data-stu-id="38ecf-142">The following image shows the `<appSettings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![éditeur de l’environnement](config-builder/static/env.png)

<span data-ttu-id="38ecf-144">Remarque : Vous devrez peut-être quitter et redémarrer Visual Studio pour afficher les modifications dans les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="38ecf-144">Note: You might need to exit and restart Visual Studio to see changes in environment variables.</span></span>

### <a name="prefix-handling"></a><span data-ttu-id="38ecf-145">Gestion de préfixe</span><span class="sxs-lookup"><span data-stu-id="38ecf-145">Prefix handling</span></span>

<span data-ttu-id="38ecf-146">Préfixes de clé peuvent simplifier les clés de paramètres, car :</span><span class="sxs-lookup"><span data-stu-id="38ecf-146">Key prefixes can simplify setting keys because:</span></span>

* <span data-ttu-id="38ecf-147">La configuration de .NET Framework est complexe et imbriqués.</span><span class="sxs-lookup"><span data-stu-id="38ecf-147">The .NET Framework configuration is complex and nested.</span></span>
* <span data-ttu-id="38ecf-148">Sources de clé/valeur externes sont couramment basiques et plat par nature.</span><span class="sxs-lookup"><span data-stu-id="38ecf-148">External key/value sources are commonly basic and flat by nature.</span></span> <span data-ttu-id="38ecf-149">Par exemple, les variables d’environnement ne sont pas imbriqués.</span><span class="sxs-lookup"><span data-stu-id="38ecf-149">For example, environment variables are not nested.</span></span>

<span data-ttu-id="38ecf-150">Utiliser une des approches suivantes pour injecter à la fois `<appSettings/>` et `<connectionStrings/>` dans la configuration par le biais de variables d’environnement :</span><span class="sxs-lookup"><span data-stu-id="38ecf-150">Use any of the following approaches to inject both `<appSettings/>` and `<connectionStrings/>` into the configuration via environment variables:</span></span>

* <span data-ttu-id="38ecf-151">Avec le `EnvironmentConfigBuilder` dans la valeur par défaut `Strict` mode et les noms de clé appropriés dans le fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="38ecf-151">With the `EnvironmentConfigBuilder` in the default `Strict` mode and the appropriate key names in the configuration file.</span></span> <span data-ttu-id="38ecf-152">Le code et le balisage précédent adopte cette approche.</span><span class="sxs-lookup"><span data-stu-id="38ecf-152">The preceding code and markup takes this approach.</span></span> <span data-ttu-id="38ecf-153">À l’aide de cette approche, vous pouvez **pas** ont un nom identique dans les deux clés de `<appSettings/>` et `<connectionStrings/>`.</span><span class="sxs-lookup"><span data-stu-id="38ecf-153">Using this approach you can **not** have identically named keys in both `<appSettings/>` and `<connectionStrings/>`.</span></span>
* <span data-ttu-id="38ecf-154">Utilisez deux `EnvironmentConfigBuilder`s dans `Greedy` mode avec des préfixes distincts et `stripPrefix`.</span><span class="sxs-lookup"><span data-stu-id="38ecf-154">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes and `stripPrefix`.</span></span> <span data-ttu-id="38ecf-155">Avec cette approche, l’application peut lire `<appSettings/>` et `<connectionStrings/>` sans avoir à mettre à jour le fichier de configuration.</span><span class="sxs-lookup"><span data-stu-id="38ecf-155">With this approach, the app can read `<appSettings/>` and `<connectionStrings/>` without needing to update the configuration file.</span></span> <span data-ttu-id="38ecf-156">La section suivante, [stripPrefix](#stripprefix), montre comment effectuer cette opération.</span><span class="sxs-lookup"><span data-stu-id="38ecf-156">The next section,  [stripPrefix](#stripprefix),  shows how to do this.</span></span>
* <span data-ttu-id="38ecf-157">Utilisez deux `EnvironmentConfigBuilder`s dans `Greedy` mode avec des préfixes distincts.</span><span class="sxs-lookup"><span data-stu-id="38ecf-157">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes.</span></span> <span data-ttu-id="38ecf-158">Avec cette approche, vous ne pouvez avoir des noms de clé en double comme noms de clés doivent différer par préfixe.</span><span class="sxs-lookup"><span data-stu-id="38ecf-158">With this approach you can't have duplicate key names as key names must differ by prefix.</span></span>  <span data-ttu-id="38ecf-159">Exemple :</span><span class="sxs-lookup"><span data-stu-id="38ecf-159">For example:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

<span data-ttu-id="38ecf-160">Avec le balisage précédent, la même source de clé-valeur plates peut être utilisée pour remplir la configuration pour les deux sections différentes.</span><span class="sxs-lookup"><span data-stu-id="38ecf-160">With the preceding markup, the same flat key/value source can be used to populate configuration for two different sections.</span></span>

<span data-ttu-id="38ecf-161">L’illustration suivante montre le `<appSettings/>` et `<connectionStrings/>` clés/valeurs à partir de l’exemple précédent *web.config* jeu de fichiers dans l’éditeur de l’environnement :</span><span class="sxs-lookup"><span data-stu-id="38ecf-161">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![éditeur de l’environnement](config-builder/static/prefix.png)

<span data-ttu-id="38ecf-163">Le code suivant lit le `<appSettings/>` et `<connectionStrings/>` clés/valeurs contenues dans l’exemple précédent *web.config* fichier :</span><span class="sxs-lookup"><span data-stu-id="38ecf-163">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

<span data-ttu-id="38ecf-164">Le code précédent définit les valeurs de propriété :</span><span class="sxs-lookup"><span data-stu-id="38ecf-164">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="38ecf-165">Les valeurs dans le *web.config* fichier si les clés ne sont pas définies dans les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="38ecf-165">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="38ecf-166">Les valeurs de l’environnement variable, si définie.</span><span class="sxs-lookup"><span data-stu-id="38ecf-166">The values of the environment variable, if set.</span></span>

<span data-ttu-id="38ecf-167">Par exemple, à l’aide de la précédente *web.config* fichier, les clés/valeurs dans l’image de l’éditeur environnement précédente et le code précédent, les valeurs suivantes sont définies :</span><span class="sxs-lookup"><span data-stu-id="38ecf-167">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="38ecf-168">Touche</span><span class="sxs-lookup"><span data-stu-id="38ecf-168">Key</span></span>              | <span data-ttu-id="38ecf-169">Value</span><span class="sxs-lookup"><span data-stu-id="38ecf-169">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="38ecf-170">AppSetting_ServiceID</span><span class="sxs-lookup"><span data-stu-id="38ecf-170">AppSetting_ServiceID</span></span>           | <span data-ttu-id="38ecf-171">AppSetting_ServiceID à partir de variables d’env</span><span class="sxs-lookup"><span data-stu-id="38ecf-171">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="38ecf-172">AppSetting_default</span><span class="sxs-lookup"><span data-stu-id="38ecf-172">AppSetting_default</span></span>            | <span data-ttu-id="38ecf-173">Valeur de AppSetting_default à partir d’env</span><span class="sxs-lookup"><span data-stu-id="38ecf-173">AppSetting_default value from env</span></span> |
|       <span data-ttu-id="38ecf-174">ConnStr_default</span><span class="sxs-lookup"><span data-stu-id="38ecf-174">ConnStr_default</span></span>         | <span data-ttu-id="38ecf-175">Val ConnStr_default à partir d’env</span><span class="sxs-lookup"><span data-stu-id="38ecf-175">ConnStr_default val from env</span></span>|

### <a name="stripprefix"></a><span data-ttu-id="38ecf-176">stripPrefix</span><span class="sxs-lookup"><span data-stu-id="38ecf-176">stripPrefix</span></span>

<span data-ttu-id="38ecf-177">`stripPrefix`: valeur booléenne, valeur par défaut est `false`.</span><span class="sxs-lookup"><span data-stu-id="38ecf-177">`stripPrefix`: boolean, defaults to `false`.</span></span> 

<span data-ttu-id="38ecf-178">Le balisage XML précédent sépare les paramètres de l’application à partir de chaînes de connexion, mais nécessite toutes les clés dans le *web.config* fichier à utiliser le préfixe spécifié.</span><span class="sxs-lookup"><span data-stu-id="38ecf-178">The preceding XML markup separates app settings from connection strings but requires all the keys in the *web.config* file to use the specified prefix.</span></span> <span data-ttu-id="38ecf-179">Par exemple, le préfixe `AppSetting` doit être ajouté à la `ServiceID` clé (« AppSetting_ServiceID »).</span><span class="sxs-lookup"><span data-stu-id="38ecf-179">For example, the prefix `AppSetting` must be added to the `ServiceID` key ("AppSetting_ServiceID").</span></span> <span data-ttu-id="38ecf-180">Avec `stripPrefix`, le préfixe n’est pas utilisé dans le *web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="38ecf-180">With `stripPrefix`, the prefix is not used in the *web.config* file.</span></span> <span data-ttu-id="38ecf-181">Le préfixe est requis dans la source du Générateur de configuration (par exemple, dans l’environnement.) Nous pensons que la plupart des développeurs utilisera `stripPrefix`.</span><span class="sxs-lookup"><span data-stu-id="38ecf-181">The prefix is required in the configuration builder source (for example, in the environment.) We anticipate most developers will use `stripPrefix`.</span></span>

<span data-ttu-id="38ecf-182">Applications suppriment généralement le préfixe.</span><span class="sxs-lookup"><span data-stu-id="38ecf-182">Applications typically strip off the prefix.</span></span> <span data-ttu-id="38ecf-183">Ce qui suit *web.config* supprime le préfixe :</span><span class="sxs-lookup"><span data-stu-id="38ecf-183">The following *web.config* strips the prefix:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

<span data-ttu-id="38ecf-184">Dans l’exemple précédent *web.config* fichier, le `default` clé se trouve dans les deux le `<appSettings/>` et `<connectionStrings/>`.</span><span class="sxs-lookup"><span data-stu-id="38ecf-184">In the preceding *web.config* file, the `default` key is in both the `<appSettings/>` and `<connectionStrings/>`.</span></span>

<span data-ttu-id="38ecf-185">L’illustration suivante montre le `<appSettings/>` et `<connectionStrings/>` clés/valeurs à partir de l’exemple précédent *web.config* jeu de fichiers dans l’éditeur de l’environnement :</span><span class="sxs-lookup"><span data-stu-id="38ecf-185">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![éditeur de l’environnement](config-builder/static/prefix.png)

<span data-ttu-id="38ecf-187">Le code suivant lit le `<appSettings/>` et `<connectionStrings/>` clés/valeurs contenues dans l’exemple précédent *web.config* fichier :</span><span class="sxs-lookup"><span data-stu-id="38ecf-187">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

<span data-ttu-id="38ecf-188">Le code précédent définit les valeurs de propriété :</span><span class="sxs-lookup"><span data-stu-id="38ecf-188">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="38ecf-189">Les valeurs dans le *web.config* fichier si les clés ne sont pas définies dans les variables d’environnement.</span><span class="sxs-lookup"><span data-stu-id="38ecf-189">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="38ecf-190">Les valeurs de l’environnement variable, si définie.</span><span class="sxs-lookup"><span data-stu-id="38ecf-190">The values of the environment variable, if set.</span></span>

<span data-ttu-id="38ecf-191">Par exemple, à l’aide de la précédente *web.config* fichier, les clés/valeurs dans l’image de l’éditeur environnement précédente et le code précédent, les valeurs suivantes sont définies :</span><span class="sxs-lookup"><span data-stu-id="38ecf-191">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="38ecf-192">Touche</span><span class="sxs-lookup"><span data-stu-id="38ecf-192">Key</span></span>              | <span data-ttu-id="38ecf-193">Value</span><span class="sxs-lookup"><span data-stu-id="38ecf-193">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="38ecf-194">ServiceID</span><span class="sxs-lookup"><span data-stu-id="38ecf-194">ServiceID</span></span>           | <span data-ttu-id="38ecf-195">AppSetting_ServiceID à partir de variables d’env</span><span class="sxs-lookup"><span data-stu-id="38ecf-195">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="38ecf-196">default</span><span class="sxs-lookup"><span data-stu-id="38ecf-196">default</span></span>            | <span data-ttu-id="38ecf-197">Valeur de AppSetting_default à partir d’env</span><span class="sxs-lookup"><span data-stu-id="38ecf-197">AppSetting_default value from env</span></span> |
|    <span data-ttu-id="38ecf-198">default</span><span class="sxs-lookup"><span data-stu-id="38ecf-198">default</span></span>         | <span data-ttu-id="38ecf-199">Val ConnStr_default à partir d’env</span><span class="sxs-lookup"><span data-stu-id="38ecf-199">ConnStr_default val from env</span></span>|

### <a name="tokenpattern"></a><span data-ttu-id="38ecf-200">tokenPattern</span><span class="sxs-lookup"><span data-stu-id="38ecf-200">tokenPattern</span></span>

<span data-ttu-id="38ecf-201">`tokenPattern`: Chaîne, valeur par défaut est `@"\$\{(\w+)\}"`</span><span class="sxs-lookup"><span data-stu-id="38ecf-201">`tokenPattern`: String, defaults to `@"\$\{(\w+)\}"`</span></span>

<span data-ttu-id="38ecf-202">Le `Expand` comportement des générateurs de recherche dans le code XML brut pour les jetons qui ressemblent à `${token}`.</span><span class="sxs-lookup"><span data-stu-id="38ecf-202">The `Expand` behavior of the builders searches the raw XML for tokens that look like `${token}`.</span></span> <span data-ttu-id="38ecf-203">La recherche est effectuée avec l’expression régulière par défaut `@"\$\{(\w+)\}"`.</span><span class="sxs-lookup"><span data-stu-id="38ecf-203">Searching is done with the default regular expression `@"\$\{(\w+)\}"`.</span></span> <span data-ttu-id="38ecf-204">Le jeu de caractères qui correspond à `\w` est plus stricte que XML et nombreuses sources de configuration ne l’autorisent.</span><span class="sxs-lookup"><span data-stu-id="38ecf-204">The set of characters that matches `\w` is more strict than XML and many configuration sources allow.</span></span> <span data-ttu-id="38ecf-205">Utilisez `tokenPattern` lorsque plus de caractères que `@"\$\{(\w+)\}"` sont requis dans le nom du jeton.</span><span class="sxs-lookup"><span data-stu-id="38ecf-205">Use `tokenPattern` when more characters than `@"\$\{(\w+)\}"` are required in the token name.</span></span>

<span data-ttu-id="38ecf-206">`tokenPattern`: Chaîne :</span><span class="sxs-lookup"><span data-stu-id="38ecf-206">`tokenPattern`: String:</span></span>

* <span data-ttu-id="38ecf-207">Permet aux développeurs de modifier l’expression régulière qui est utilisé pour la correspondance de jeton.</span><span class="sxs-lookup"><span data-stu-id="38ecf-207">Allows developers to change the regex that is used for token matching.</span></span>
* <span data-ttu-id="38ecf-208">Aucune validation n’est effectuée pour vous assurer qu’il est une expression régulière bien formée, non dangereux.</span><span class="sxs-lookup"><span data-stu-id="38ecf-208">No validation is done to make sure it is a well-formed, non-dangerous regex.</span></span>
* <span data-ttu-id="38ecf-209">Il doit contenir un groupe de capture.</span><span class="sxs-lookup"><span data-stu-id="38ecf-209">It must contain a capture group.</span></span> <span data-ttu-id="38ecf-210">L’expression régulière entière doit correspondre au jeton entier.</span><span class="sxs-lookup"><span data-stu-id="38ecf-210">The entire regex must match the entire token.</span></span> <span data-ttu-id="38ecf-211">La première capture doit être le nom du jeton à rechercher dans la source de configuration.</span><span class="sxs-lookup"><span data-stu-id="38ecf-211">The first capture must be the token name to look up in the configuration source.</span></span>

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a><span data-ttu-id="38ecf-212">Générateurs de configuration dans Microsoft.Configuration.ConfigurationBuilders</span><span class="sxs-lookup"><span data-stu-id="38ecf-212">Configuration builders in Microsoft.Configuration.ConfigurationBuilders</span></span>

### <a name="environmentconfigbuilder"></a><span data-ttu-id="38ecf-213">EnvironmentConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="38ecf-213">EnvironmentConfigBuilder</span></span>

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

<span data-ttu-id="38ecf-214">Le [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span><span class="sxs-lookup"><span data-stu-id="38ecf-214">The [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span></span>

* <span data-ttu-id="38ecf-215">Est la plus simple des générateurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="38ecf-215">Is the simplest of the configuration builders.</span></span>
* <span data-ttu-id="38ecf-216">Lit les valeurs à partir de l’environnement.</span><span class="sxs-lookup"><span data-stu-id="38ecf-216">Reads values from the environment.</span></span>
* <span data-ttu-id="38ecf-217">Ne dispose pas des options de configuration supplémentaires.</span><span class="sxs-lookup"><span data-stu-id="38ecf-217">Does not have any additional configuration options.</span></span>
* <span data-ttu-id="38ecf-218">Le `name` valeur d’attribut est arbitraire.</span><span class="sxs-lookup"><span data-stu-id="38ecf-218">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="38ecf-219">**Remarque :** dans un environnement de conteneur Windows, les variables définies au moment de l’exécution sont uniquement injectés dans l’environnement de processus de point d’entrée.</span><span class="sxs-lookup"><span data-stu-id="38ecf-219">**Note:** In a Windows container environment, variables set at run time are only injected into the EntryPoint process environment.</span></span> <span data-ttu-id="38ecf-220">Les applications qui s’exécutent en tant que service ou un processus non EntryPoint ne récupèrent pas ces variables, sauf si elles sont injectées dans le cas contraire via un mécanisme dans le conteneur.</span><span class="sxs-lookup"><span data-stu-id="38ecf-220">Apps that run as a service or a non-EntryPoint process do not pick up these variables unless they are otherwise injected through a mechanism in the container.</span></span> <span data-ttu-id="38ecf-221">Pour [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-en fonction des conteneurs, la version actuelle de [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) gère cela dans le *DefaultAppPool* uniquement.</span><span class="sxs-lookup"><span data-stu-id="38ecf-221">For [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-based containers, the current version of [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) handles this in the *DefaultAppPool* only.</span></span> <span data-ttu-id="38ecf-222">Autres variantes de conteneur basé sur Windows devrez peut-être développer leur propre mécanisme d’injection de code pour les processus non EntryPoint.</span><span class="sxs-lookup"><span data-stu-id="38ecf-222">Other Windows-based container variants may need to develop their own injection mechanism for non-EntryPoint processes.</span></span>

### <a name="usersecretsconfigbuilder"></a><span data-ttu-id="38ecf-223">UserSecretsConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="38ecf-223">UserSecretsConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="38ecf-224">Jamais magasin de mots de passe, chaînes de connexion sensibles ou autres données sensibles dans le code source.</span><span class="sxs-lookup"><span data-stu-id="38ecf-224">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="38ecf-225">Aucun secret de production ne doit pas être utilisé pour le développement ou de test.</span><span class="sxs-lookup"><span data-stu-id="38ecf-225">Production secrets should not be used for development or test.</span></span>

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

<span data-ttu-id="38ecf-226">Ce générateur de configuration fournit une fonctionnalité similaire à [Secret Manager d’ASP.NET Core](/aspnet/core/security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="38ecf-226">This configuration builder provides a feature similar to [ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets).</span></span>

<span data-ttu-id="38ecf-227">Le [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) peut être utilisé dans les projets .NET Framework, mais un fichier de secrets doit être spécifié.</span><span class="sxs-lookup"><span data-stu-id="38ecf-227">The [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) can be used in .NET Framework projects, but a secrets file must be specified.</span></span> <span data-ttu-id="38ecf-228">Vous pouvez également définir le `UserSecretsId` propriété dans le projet fichiers et de créer le fichier de secrets brutes à l’emplacement approprié pour la lecture.</span><span class="sxs-lookup"><span data-stu-id="38ecf-228">Alternatively, you can define the `UserSecretsId` property in the project file and create the raw secrets file in the correct location for reading.</span></span> <span data-ttu-id="38ecf-229">Pour conserver les dépendances externes en dehors de votre projet, le fichier de secret est au format XML.</span><span class="sxs-lookup"><span data-stu-id="38ecf-229">To keep external dependencies out of your project, the secret file is XML formatted.</span></span> <span data-ttu-id="38ecf-230">La mise en forme XML est un détail d’implémentation, et le format ne doit pas se reposer.</span><span class="sxs-lookup"><span data-stu-id="38ecf-230">The XML formatting is an implementation detail, and the format should not be relied upon.</span></span> <span data-ttu-id="38ecf-231">Si vous avez besoin de partager un *secrets.json* de fichiers avec les projets .NET Core, envisagez d’utiliser le [SimpleJsonConfigBuilder](#simplejsonconfig).</span><span class="sxs-lookup"><span data-stu-id="38ecf-231">If you need to share a *secrets.json* file with .NET Core projects, consider using the [SimpleJsonConfigBuilder](#simplejsonconfig).</span></span> <span data-ttu-id="38ecf-232">Le `SimpleJsonConfigBuilder` mettre en forme pour .NET Core doit également être considéré comme un détail d’implémentation susceptibles d’être modifiées.</span><span class="sxs-lookup"><span data-stu-id="38ecf-232">The `SimpleJsonConfigBuilder` format for .NET Core should also be considered an implementation detail subject to change.</span></span>

<span data-ttu-id="38ecf-233">Les attributs de configuration de `UserSecretsConfigBuilder`:</span><span class="sxs-lookup"><span data-stu-id="38ecf-233">Configuration attributes for `UserSecretsConfigBuilder`:</span></span>

* <span data-ttu-id="38ecf-234">`userSecretsId` -Il s’agit de la méthode recommandée pour l’identification d’un fichier de secrets XML.</span><span class="sxs-lookup"><span data-stu-id="38ecf-234">`userSecretsId` - This is the preferred method for identifying an XML secrets file.</span></span> <span data-ttu-id="38ecf-235">Il est similaire à .NET Core, qui utilise un `UserSecretsId` propriété pour stocker cet identificateur du projet.</span><span class="sxs-lookup"><span data-stu-id="38ecf-235">It works similar to .NET Core, which uses a `UserSecretsId` project property to store this identifier.</span></span> <span data-ttu-id="38ecf-236">La chaîne doit être unique, il n’a pas besoin être un GUID.</span><span class="sxs-lookup"><span data-stu-id="38ecf-236">The string must be unique, it doesn't need to be a GUID.</span></span> <span data-ttu-id="38ecf-237">Avec cet attribut, le `UserSecretsConfigBuilder` Regarder dans un emplacement local connu (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) pour un fichier de secrets appartenant à cet identificateur.</span><span class="sxs-lookup"><span data-stu-id="38ecf-237">With this attribute, the `UserSecretsConfigBuilder` look in a well-known local location (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) for a secrets file belonging to this identifier.</span></span>
* <span data-ttu-id="38ecf-238">`userSecretsFile` -Attribut facultatif spécifiant le fichier contenant les clés secrètes.</span><span class="sxs-lookup"><span data-stu-id="38ecf-238">`userSecretsFile` - An optional attribute specifying the file containing the secrets.</span></span> <span data-ttu-id="38ecf-239">Le `~` caractère peut être utilisé au début pour faire référence à la racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="38ecf-239">The `~` character can be used at the start to reference the application root.</span></span> <span data-ttu-id="38ecf-240">Cet attribut ou le `userSecretsId` attribut est requis.</span><span class="sxs-lookup"><span data-stu-id="38ecf-240">Either this attribute or the `userSecretsId` attribute is required.</span></span> <span data-ttu-id="38ecf-241">Si les deux sont spécifiés, `userSecretsFile` est prioritaire.</span><span class="sxs-lookup"><span data-stu-id="38ecf-241">If both are specified, `userSecretsFile` takes precedence.</span></span>
* <span data-ttu-id="38ecf-242">`optional`: boolean, valeur par défaut `true` -empêche une exception si le fichier de secrets ne peut pas être trouvé.</span><span class="sxs-lookup"><span data-stu-id="38ecf-242">`optional`: boolean, default value `true` - Prevents an exception if the secrets file cannot be found.</span></span> 
* <span data-ttu-id="38ecf-243">Le `name` valeur d’attribut est arbitraire.</span><span class="sxs-lookup"><span data-stu-id="38ecf-243">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="38ecf-244">Le fichier de secrets a le format suivant :</span><span class="sxs-lookup"><span data-stu-id="38ecf-244">The secrets file has the following format:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a><span data-ttu-id="38ecf-245">AzureKeyVaultConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="38ecf-245">AzureKeyVaultConfigBuilder</span></span>

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

<span data-ttu-id="38ecf-246">Le [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) lit les valeurs stockées dans le [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span><span class="sxs-lookup"><span data-stu-id="38ecf-246">The [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) reads values stored in the [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span></span>

<span data-ttu-id="38ecf-247">`vaultName` est requis (le nom du coffre) ou un URI dans le coffre.</span><span class="sxs-lookup"><span data-stu-id="38ecf-247">`vaultName` is required (either the name of the vault or a URI to the vault).</span></span> <span data-ttu-id="38ecf-248">Les autres attributs permettent un contrôle sur le coffre pour se connecter à, mais sont nécessaires seulement si l’application n’est pas en cours d’exécution dans un environnement qui fonctionne avec `Microsoft.Azure.Services.AppAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="38ecf-248">The other attributes allow control about which vault to connect to, but are only necessary if the application is not running in an environment that works with `Microsoft.Azure.Services.AppAuthentication`.</span></span> <span data-ttu-id="38ecf-249">La bibliothèque d’authentification des Services Azure est utilisée pour choisir automatiquement les informations de connexion à partir de l’environnement d’exécution si possible.</span><span class="sxs-lookup"><span data-stu-id="38ecf-249">The Azure Services Authentication library is used to automatically pick up connection information from the execution environment if possible.</span></span> <span data-ttu-id="38ecf-250">Vous pouvez remplacer automatiquement récupérer les informations de connexion en fournissant une chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="38ecf-250">You can override automatically pick up of connection information by providing a connection string.</span></span>

* <span data-ttu-id="38ecf-251">`vaultName` -Obligatoire si `uri` dans ne pas fourni.</span><span class="sxs-lookup"><span data-stu-id="38ecf-251">`vaultName` - Required if `uri` in not provided.</span></span> <span data-ttu-id="38ecf-252">Spécifie le nom du coffre dans votre abonnement Azure à partir duquel lire les paires clé/valeur.</span><span class="sxs-lookup"><span data-stu-id="38ecf-252">Specifies the name of the vault in your Azure subscription from which to read key/value pairs.</span></span>
* <span data-ttu-id="38ecf-253">`connectionString` -Une chaîne de connexion utilisable par [le paramètre AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)</span><span class="sxs-lookup"><span data-stu-id="38ecf-253">`connectionString` - A connection string usable by [AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)</span></span>
* <span data-ttu-id="38ecf-254">`uri` -Se connecte à d’autres fournisseurs de Key Vault avec la valeur `uri` valeur.</span><span class="sxs-lookup"><span data-stu-id="38ecf-254">`uri` - Connects to other Key Vault providers with the specified `uri` value.</span></span> <span data-ttu-id="38ecf-255">Si non spécifié, Azure (`vaultName`) est le fournisseur du coffre.</span><span class="sxs-lookup"><span data-stu-id="38ecf-255">If not specified, Azure (`vaultName`) is the vault provider.</span></span>
* <span data-ttu-id="38ecf-256">`version` -Azure Key Vault fournit une fonctionnalité de contrôle de version pour les clés secrètes.</span><span class="sxs-lookup"><span data-stu-id="38ecf-256">`version` - Azure Key Vault provides a versioning feature for secrets.</span></span> <span data-ttu-id="38ecf-257">Si `version` est spécifié, le générateur récupère uniquement les secrets correspondant à cette version.</span><span class="sxs-lookup"><span data-stu-id="38ecf-257">If `version` is specified, the builder only retrieves secrets matching this version.</span></span>
* <span data-ttu-id="38ecf-258">`preloadSecretNames` -Par défaut, ce générateur de requêtes **tous les** dans le coffre de clés, les noms de clé lors de son initialisation.</span><span class="sxs-lookup"><span data-stu-id="38ecf-258">`preloadSecretNames` - By default, this builder querys **all** key names in the key vault when it is initialized.</span></span> <span data-ttu-id="38ecf-259">Pour empêcher la lecture de toutes les valeurs de clés, définissez cet attribut sur `false`.</span><span class="sxs-lookup"><span data-stu-id="38ecf-259">To prevent reading all key values, set this attribute to `false`.</span></span> <span data-ttu-id="38ecf-260">Définir cette valeur sur `false` lit les secrets celui à la fois.</span><span class="sxs-lookup"><span data-stu-id="38ecf-260">Setting this to `false` reads secrets one at a time.</span></span> <span data-ttu-id="38ecf-261">Lecture de secrets à la fois est utile si le coffre autorise l’accès « Get » mais pas « liste » accéder.</span><span class="sxs-lookup"><span data-stu-id="38ecf-261">Reading secrets one at a time can useful if the vault allows "Get" access but not "List" access.</span></span> <span data-ttu-id="38ecf-262">**Remarque :** lors de l’utilisation `Greedy` mode, `preloadSecretNames` doit être `true` (la valeur par défaut.)</span><span class="sxs-lookup"><span data-stu-id="38ecf-262">**Note:** When using `Greedy` mode, `preloadSecretNames` must be `true` (the default.)</span></span>

### <a name="keyperfileconfigbuilder"></a><span data-ttu-id="38ecf-263">KeyPerFileConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="38ecf-263">KeyPerFileConfigBuilder</span></span>

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

<span data-ttu-id="38ecf-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) est un générateur de configuration de base qui utilise les fichiers d’un répertoire en tant que source de valeurs.</span><span class="sxs-lookup"><span data-stu-id="38ecf-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) is a basic configuration builder that uses a directory's files as a source of values.</span></span> <span data-ttu-id="38ecf-265">Nom d’un fichier est la clé, et le contenu est la valeur.</span><span class="sxs-lookup"><span data-stu-id="38ecf-265">A file's name is the key, and the contents are the value.</span></span> <span data-ttu-id="38ecf-266">Ce générateur de configuration peut être utile lors de l’exécution dans un environnement de conteneur orchestrée.</span><span class="sxs-lookup"><span data-stu-id="38ecf-266">This configuration builder can be useful when running in an orchestrated container environment.</span></span> <span data-ttu-id="38ecf-267">Systèmes, comme Docker Swarm et Kubernetes fournissent `secrets` à leurs conteneurs orchestrée windows de cette manière par-fichier de clé.</span><span class="sxs-lookup"><span data-stu-id="38ecf-267">Systems like Docker Swarm and Kubernetes provide `secrets` to their orchestrated windows containers in this key-per-file manner.</span></span>

<span data-ttu-id="38ecf-268">Détails de l’attribut :</span><span class="sxs-lookup"><span data-stu-id="38ecf-268">Attribute details:</span></span>

* <span data-ttu-id="38ecf-269">`directoryPath` - Requis.</span><span class="sxs-lookup"><span data-stu-id="38ecf-269">`directoryPath` - Required.</span></span> <span data-ttu-id="38ecf-270">Spécifie un chemin d’accès pour rechercher les valeurs.</span><span class="sxs-lookup"><span data-stu-id="38ecf-270">Specifies a path to look in for values.</span></span> <span data-ttu-id="38ecf-271">Docker pour Windows secrets sont stockés dans le *C:\ProgramData\Docker\secrets* répertoire par défaut.</span><span class="sxs-lookup"><span data-stu-id="38ecf-271">Docker for Windows secrets are stored in the *C:\ProgramData\Docker\secrets* directory by default.</span></span>
* <span data-ttu-id="38ecf-272">`ignorePrefix` -Les fichiers qui commencent par ce préfixe sont exclus.</span><span class="sxs-lookup"><span data-stu-id="38ecf-272">`ignorePrefix` - Files that start with this prefix are excluded.</span></span> <span data-ttu-id="38ecf-273">Valeur par défaut est « ignorer ».</span><span class="sxs-lookup"><span data-stu-id="38ecf-273">Defaults to "ignore.".</span></span>
* <span data-ttu-id="38ecf-274">`keyDelimiter` -Valeur par défaut est `null`.</span><span class="sxs-lookup"><span data-stu-id="38ecf-274">`keyDelimiter` - Default value is `null`.</span></span> <span data-ttu-id="38ecf-275">Si spécifié, le Générateur de configuration traverse plusieurs niveaux du répertoire, la création de noms de clés avec ce séparateur.</span><span class="sxs-lookup"><span data-stu-id="38ecf-275">If specified, the configuration builder traverses multiple levels of the directory, building up key names with this delimiter.</span></span> <span data-ttu-id="38ecf-276">Si cette valeur est `null`, le Générateur de configuration recherche uniquement au niveau supérieur du répertoire.</span><span class="sxs-lookup"><span data-stu-id="38ecf-276">If this value is `null`, the configuration builder only looks at the top level of the directory.</span></span>
* <span data-ttu-id="38ecf-277">`optional` -Valeur par défaut est `false`.</span><span class="sxs-lookup"><span data-stu-id="38ecf-277">`optional` -  Default value is `false`.</span></span> <span data-ttu-id="38ecf-278">Spécifie si le Générateur de configuration doit provoquer des erreurs si le répertoire source n’existe pas.</span><span class="sxs-lookup"><span data-stu-id="38ecf-278">Specifies whether the configuration builder should cause errors if the source directory doesn't exist.</span></span>

### <a name="simplejsonconfigbuilder"></a><span data-ttu-id="38ecf-279">SimpleJsonConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="38ecf-279">SimpleJsonConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="38ecf-280">Jamais magasin de mots de passe, chaînes de connexion sensibles ou autres données sensibles dans le code source.</span><span class="sxs-lookup"><span data-stu-id="38ecf-280">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="38ecf-281">Aucun secret de production ne doit pas être utilisé pour le développement ou de test.</span><span class="sxs-lookup"><span data-stu-id="38ecf-281">Production secrets should not be used for development or test.</span></span>

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

<span data-ttu-id="38ecf-282">Projets .NET core utilisent fréquemment des fichiers JSON pour la configuration.</span><span class="sxs-lookup"><span data-stu-id="38ecf-282">.NET Core projects frequently use JSON files for configuration.</span></span> <span data-ttu-id="38ecf-283">Le [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) builder permet aux fichiers de .NET Core JSON à utiliser dans le .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="38ecf-283">The [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) builder allows .NET Core JSON files to be used in the .NET Framework.</span></span> <span data-ttu-id="38ecf-284">Ce générateur de configuration est fournit un mappage de base à partir d’une source de clé-valeur plates en zones de clé/valeur spécifique de configuration de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="38ecf-284">This configuration builder is provides a basic mapping from a flat key/value source into specific key/value areas of .NET Framework configuration.</span></span> <span data-ttu-id="38ecf-285">Ce générateur de configuration est **pas** fournir pour les configurations hiérarchiques.</span><span class="sxs-lookup"><span data-stu-id="38ecf-285">This configuration builder does **not** provide for hierarchical configurations.</span></span> <span data-ttu-id="38ecf-286">Le fichier de sauvegarde JSON est similaire à un dictionnaire, pas un objet hiérarchiques complexes.</span><span class="sxs-lookup"><span data-stu-id="38ecf-286">The JSON backing file is similar to a dictionary, not a complex hierarchical object.</span></span> <span data-ttu-id="38ecf-287">Un fichier hiérarchique à plusieurs niveaux peut être utilisé.</span><span class="sxs-lookup"><span data-stu-id="38ecf-287">A multi-level hierarchical file can be used.</span></span> <span data-ttu-id="38ecf-288">Ce fournisseur `flatten`la profondeur en ajoutant le nom de propriété à chaque niveau à l’aide de s `:` comme délimiteur.</span><span class="sxs-lookup"><span data-stu-id="38ecf-288">This provider `flatten`s the depth by appending the property name at each level using `:` as a delimiter.</span></span>

<span data-ttu-id="38ecf-289">Détails de l’attribut :</span><span class="sxs-lookup"><span data-stu-id="38ecf-289">Attribute details:</span></span>

* <span data-ttu-id="38ecf-290">`jsonFile` - Requis.</span><span class="sxs-lookup"><span data-stu-id="38ecf-290">`jsonFile` - Required.</span></span> <span data-ttu-id="38ecf-291">Spécifie le fichier JSON qui lit.</span><span class="sxs-lookup"><span data-stu-id="38ecf-291">Specifies the JSON file to read from.</span></span> <span data-ttu-id="38ecf-292">Le `~` caractère peut être utilisé au début pour faire référence à la racine de l’application.</span><span class="sxs-lookup"><span data-stu-id="38ecf-292">The `~` character can be used at the start to reference the app root.</span></span>
* <span data-ttu-id="38ecf-293">`optional` -Booléenne, valeur par défaut est `true`.</span><span class="sxs-lookup"><span data-stu-id="38ecf-293">`optional` - Boolean,  default value is `true`.</span></span> <span data-ttu-id="38ecf-294">Empêche la levée d’exceptions si le fichier JSON est introuvable.</span><span class="sxs-lookup"><span data-stu-id="38ecf-294">Prevents throwing exceptions if the JSON file cannot be found.</span></span>
* <span data-ttu-id="38ecf-295">`jsonMode` - `[Flat|Sectional]`.</span><span class="sxs-lookup"><span data-stu-id="38ecf-295">`jsonMode` - `[Flat|Sectional]`.</span></span> <span data-ttu-id="38ecf-296">`Flat` est la valeur par défaut.</span><span class="sxs-lookup"><span data-stu-id="38ecf-296">`Flat` is the default.</span></span> <span data-ttu-id="38ecf-297">Lorsque `jsonMode` est `Flat`, le fichier JSON est une source unique clé-valeur plates.</span><span class="sxs-lookup"><span data-stu-id="38ecf-297">When `jsonMode` is `Flat`, the JSON file is a single flat key/value source.</span></span> <span data-ttu-id="38ecf-298">Le `EnvironmentConfigBuilder` et `AzureKeyVaultConfigBuilder` sont également des sources de clé/valeur plat unique.</span><span class="sxs-lookup"><span data-stu-id="38ecf-298">The `EnvironmentConfigBuilder` and `AzureKeyVaultConfigBuilder` are also single flat key/value sources.</span></span> <span data-ttu-id="38ecf-299">Lorsque le `SimpleJsonConfigBuilder` est configuré dans `Sectional` mode :</span><span class="sxs-lookup"><span data-stu-id="38ecf-299">When the `SimpleJsonConfigBuilder` is configured in `Sectional` mode:</span></span>

  * <span data-ttu-id="38ecf-300">Le fichier JSON est conceptuellement divisé simplement au niveau supérieur en plusieurs dictionnaires.</span><span class="sxs-lookup"><span data-stu-id="38ecf-300">The JSON file is conceptually divided just at the top level into multiple dictionaries.</span></span>
  * <span data-ttu-id="38ecf-301">Chacun des dictionnaires est uniquement appliquée à la section de configuration qui correspond au nom de propriété de niveau supérieur qui leur sont attaché.</span><span class="sxs-lookup"><span data-stu-id="38ecf-301">Each of the dictionaries is only applied to the configuration section that matches the top-level property name attached to them.</span></span> <span data-ttu-id="38ecf-302">Exemple :</span><span class="sxs-lookup"><span data-stu-id="38ecf-302">For example:</span></span>

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a><span data-ttu-id="38ecf-303">Implémentation d’un générateur de configuration clé-valeur personnalisées</span><span class="sxs-lookup"><span data-stu-id="38ecf-303">Implementing a custom key/value configuration builder</span></span>

<span data-ttu-id="38ecf-304">Si les générateurs de configuration ne répondent pas à vos besoins, vous pouvez écrire un personnalisé.</span><span class="sxs-lookup"><span data-stu-id="38ecf-304">If the configuration builders don't meet your needs, you can write a custom one.</span></span> <span data-ttu-id="38ecf-305">Le `KeyValueConfigBuilder` classe de base gère les modes de substitution et la plupart des problèmes de préfixe.</span><span class="sxs-lookup"><span data-stu-id="38ecf-305">The `KeyValueConfigBuilder` base class handles substitution modes and most prefix concerns.</span></span> <span data-ttu-id="38ecf-306">Un projet d’implémentation ont seulement besoin :</span><span class="sxs-lookup"><span data-stu-id="38ecf-306">An implementing project need only:</span></span>

* <span data-ttu-id="38ecf-307">Hériter de la classe de base et implémenter une source de base de paires clé/valeur via la `GetValue` et `GetAllValues`:</span><span class="sxs-lookup"><span data-stu-id="38ecf-307">Inherit from the base class and implement a basic source of key/value pairs through the `GetValue` and `GetAllValues`:</span></span>
* <span data-ttu-id="38ecf-308">Ajouter le [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) au projet.</span><span class="sxs-lookup"><span data-stu-id="38ecf-308">Add the [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) to the project.</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

<span data-ttu-id="38ecf-309">Le `KeyValueConfigBuilder` classe de base fournit une grande partie du comportement professionnel et cohérent entre la clé/valeur générateurs de configuration.</span><span class="sxs-lookup"><span data-stu-id="38ecf-309">The `KeyValueConfigBuilder` base class provides much of the work and consistent behavior across key/value configuration builders.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="38ecf-310">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="38ecf-310">Additional resources</span></span>

* [<span data-ttu-id="38ecf-311">Dépôt GitHub de générateurs de configuration</span><span class="sxs-lookup"><span data-stu-id="38ecf-311">Configuration Builders GitHub repository</span></span>](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [<span data-ttu-id="38ecf-312">Authentification de service à service dans Azure Key Vault à l’aide de .NET</span><span class="sxs-lookup"><span data-stu-id="38ecf-312">Service-to-service authentication to Azure Key Vault using .NET</span></span>](/azure/key-vault/service-to-service-authentication#connection-string-support)
