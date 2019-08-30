---
title: Tester des API web avec la boucle REPL HTTP
author: scottaddie
description: Découvrez comment utiliser l’outil global REPL HTTP de .NET Core pour parcourir et tester une API web ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/25/2019
uid: web-api/http-repl
ms.openlocfilehash: d2c5f774595e7a2223e84cc76eecdb9baa04adfe
ms.sourcegitcommit: 776598f71da0d1e4c9e923b3b395d3c3b5825796
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70024801"
---
# <a name="test-web-apis-with-the-http-repl"></a><span data-ttu-id="27919-103">Tester des API web avec la boucle REPL HTTP</span><span class="sxs-lookup"><span data-stu-id="27919-103">Test web APIs with the HTTP REPL</span></span>

<span data-ttu-id="27919-104">Par [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="27919-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="27919-105">La boucle REPL (Read-Eval-Print Loop) HTTP est :</span><span class="sxs-lookup"><span data-stu-id="27919-105">The HTTP Read-Eval-Print Loop (REPL) is:</span></span>

* <span data-ttu-id="27919-106">Un outil en ligne de commande léger et multiplateforme qui est pris en charge partout où .NET Core est pris en charge.</span><span class="sxs-lookup"><span data-stu-id="27919-106">A lightweight, cross-platform command-line tool that's supported everywhere .NET Core is supported.</span></span>
* <span data-ttu-id="27919-107">Utilisée pour effectuer des requêtes HTTP pour tester des API web ASP.NET Core (et des API web ASP.NET Core non-ASP.NET) et voir leurs résultats.</span><span class="sxs-lookup"><span data-stu-id="27919-107">Used for making HTTP requests to test ASP.NET Core web APIs (and non-ASP.NET Core web APIs) and view their results.</span></span>
* <span data-ttu-id="27919-108">Capable de tester les API web hébergées dans n’importe quel environnement, notamment localhost et Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="27919-108">Capable of testing web APIs hosted in any environment, including localhost and Azure App Service.</span></span>

<span data-ttu-id="27919-109">Les [verbes HTTP](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) suivants sont pris en charge :</span><span class="sxs-lookup"><span data-stu-id="27919-109">The following [HTTP verbs](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) are supported:</span></span>

* [<span data-ttu-id="27919-110">DELETE</span><span class="sxs-lookup"><span data-stu-id="27919-110">DELETE</span></span>](#test-http-delete-requests)
* [<span data-ttu-id="27919-111">GET</span><span class="sxs-lookup"><span data-stu-id="27919-111">GET</span></span>](#test-http-get-requests)
* [<span data-ttu-id="27919-112">HEAD</span><span class="sxs-lookup"><span data-stu-id="27919-112">HEAD</span></span>](#test-http-head-requests)
* [<span data-ttu-id="27919-113">OPTIONS</span><span class="sxs-lookup"><span data-stu-id="27919-113">OPTIONS</span></span>](#test-http-options-requests)
* [<span data-ttu-id="27919-114">PATCH</span><span class="sxs-lookup"><span data-stu-id="27919-114">PATCH</span></span>](#test-http-patch-requests)
* [<span data-ttu-id="27919-115">POST</span><span class="sxs-lookup"><span data-stu-id="27919-115">POST</span></span>](#test-http-post-requests)
* [<span data-ttu-id="27919-116">PUT</span><span class="sxs-lookup"><span data-stu-id="27919-116">PUT</span></span>](#test-http-put-requests)

<span data-ttu-id="27919-117">Pour continuer, [consultez ou téléchargez l’exemple d’API web ASP.NET Core](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([comment télécharger](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="27919-117">To follow along, [view or download the sample ASP.NET Core web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="27919-118">Prérequis</span><span class="sxs-lookup"><span data-stu-id="27919-118">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](~/includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="27919-119">Installation</span><span class="sxs-lookup"><span data-stu-id="27919-119">Installation</span></span>

<span data-ttu-id="27919-120">Pour installer la boucle REPL HTTP, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="27919-120">To install the HTTP REPL, run the following command:</span></span>

```console
dotnet tool install -g Microsoft.dotnet-httprepl --version "3.0.0-*"
```

<span data-ttu-id="27919-121">Un [outil global .NET Core](/dotnet/core/tools/global-tools#install-a-global-tool) est installé à partir du package NuGet [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl).</span><span class="sxs-lookup"><span data-stu-id="27919-121">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl) NuGet package.</span></span>

## <a name="usage"></a><span data-ttu-id="27919-122">Utilisation</span><span class="sxs-lookup"><span data-stu-id="27919-122">Usage</span></span>

<span data-ttu-id="27919-123">Une fois l’installation de l’outil réussie, exécutez la commande suivante pour démarrer la boucle REPL HTTP :</span><span class="sxs-lookup"><span data-stu-id="27919-123">After successful installation of the tool, run the following command to start the HTTP REPL:</span></span>

```console
dotnet httprepl
```

<span data-ttu-id="27919-124">Pour voir les commandes de la boucle REPL HTTP disponibles, exécutez une des commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="27919-124">To view the available HTTP REPL commands, run one of the following commands:</span></span>

```console
dotnet httprepl -h
```

```console
dotnet httprepl --help
```

<span data-ttu-id="27919-125">La sortie suivante s’affiche :</span><span class="sxs-lookup"><span data-stu-id="27919-125">The following output is displayed:</span></span>

```console
Usage:
  dotnet httprepl [<BASE_ADDRESS>] [options]

Arguments:
  <BASE_ADDRESS> - The initial base address for the REPL.

Options:
  -h|--help - Show help information.

Once the REPL starts, these commands are valid:

Setup Commands:
Use these commands to configure the tool for your API server

connect        Configures the directory structure and base address of the api server
set header     Sets or clears a header for all requests. e.g. `set header content-type application/json`

HTTP Commands:
Use these commands to execute requests against your application.

GET            get - Issues a GET request
POST           post - Issues a POST request
PUT            put - Issues a PUT request
DELETE         delete - Issues a DELETE request
PATCH          patch - Issues a PATCH request
HEAD           head - Issues a HEAD request
OPTIONS        options - Issues a OPTIONS request

Navigation Commands:
The REPL allows you to navigate your URL space and focus on specific APIs that you are working on.

set base       Set the base URI. e.g. `set base http://locahost:5000`
ls             Show all endpoints for the current path
cd             Append the given directory to the currently selected path, or move up a path when using `cd ..`

Shell Commands:
Use these commands to interact with the REPL shell.

clear          Removes all text from the shell
echo [on/off]  Turns request echoing on or off, show the request that was made when using request commands
exit           Exit the shell

REPL Customization Commands:
Use these commands to customize the REPL behavior.

pref [get/set] Allows viewing or changing preferences, e.g. 'pref set editor.command.default 'C:\\Program Files\\Microsoft VS Code\\Code.exe'`
run            Runs the script at the given path. A script is a set of commands that can be typed with one command per line
ui             Displays the Swagger UI page, if available, in the default browser

Use `help <COMMAND>` for more detail on an individual command. e.g. `help get`.
For detailed tool info, see https://aka.ms/http-repl-doc.
```

<span data-ttu-id="27919-126">La boucle REPL HTTP offre la complétion des commandes.</span><span class="sxs-lookup"><span data-stu-id="27919-126">The HTTP REPL offers command completion.</span></span> <span data-ttu-id="27919-127">Un appui sur la touche <kbd>Tab</kbd> itère au sein de la liste des commandes qui complètent les caractères ou le point de terminaison d’API que vous avez tapés.</span><span class="sxs-lookup"><span data-stu-id="27919-127">Pressing the <kbd>Tab</kbd> key iterates through the list of commands that complete the characters or API endpoint that you typed.</span></span> <span data-ttu-id="27919-128">Les sections suivantes décrivent les commandes CLI disponibles.</span><span class="sxs-lookup"><span data-stu-id="27919-128">The following sections outline the available CLI commands.</span></span>

## <a name="connect-to-the-web-api"></a><span data-ttu-id="27919-129">Se connecter à l’API web</span><span class="sxs-lookup"><span data-stu-id="27919-129">Connect to the web API</span></span>

<span data-ttu-id="27919-130">Connectez-vous à une API web en exécutant la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="27919-130">Connect to a web API by running the following command:</span></span>

```console
dotnet httprepl <ROOT URI>
```

<span data-ttu-id="27919-131">`<ROOT URI>` est l’URI de base pour l’API web.</span><span class="sxs-lookup"><span data-stu-id="27919-131">`<ROOT URI>` is the base URI for the web API.</span></span> <span data-ttu-id="27919-132">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="27919-132">For example:</span></span>

```console
dotnet httprepl https://localhost:5001
```

<span data-ttu-id="27919-133">Vous pouvez également exécuter la commande suivante à tout moment pendant l’exécution de la boucle REPL HTTP :</span><span class="sxs-lookup"><span data-stu-id="27919-133">Alternatively, run the following command at any time while the HTTP REPL is running:</span></span>

```console
connect <ROOT URI>
```

<span data-ttu-id="27919-134">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="27919-134">For example:</span></span>

```console
(Disconnected)~ connect https://localhost:5001
```

## <a name="manually-point-to-the-swagger-document-for-the-web-api"></a><span data-ttu-id="27919-135">Pointer manuellement sur le document Swagger pour l’API web</span><span class="sxs-lookup"><span data-stu-id="27919-135">Manually point to the Swagger document for the web API</span></span>

<span data-ttu-id="27919-136">La commande connect ci-dessus tente de trouver automatiquement le document Swagger.</span><span class="sxs-lookup"><span data-stu-id="27919-136">The connect command above will attempt to find the Swagger document automatically.</span></span> <span data-ttu-id="27919-137">Si, pour une raison quelconque, elle n’y parvient pas, vous pouvez spécifier l’URI du document Swagger pour l’API web à l’aide de l’option `--swagger` :</span><span class="sxs-lookup"><span data-stu-id="27919-137">If for some reason it is unable to do so, you can specify the URI of the Swagger document for the web API by using the `--swagger` option:</span></span>

```console
connect <ROOT URI> --swagger <SWAGGER URI>
```

<span data-ttu-id="27919-138">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="27919-138">For example:</span></span>

```console
(Disconnected)~ connect https://localhost:5001 --swagger /swagger/v1/swagger.json
```

## <a name="navigate-the-web-api"></a><span data-ttu-id="27919-139">Accéder à l’API web</span><span class="sxs-lookup"><span data-stu-id="27919-139">Navigate the web API</span></span>

### <a name="view-available-endpoints"></a><span data-ttu-id="27919-140">Voir les points de terminaison disponibles</span><span class="sxs-lookup"><span data-stu-id="27919-140">View available endpoints</span></span>

<span data-ttu-id="27919-141">Pour lister les différents points de terminaison (contrôleurs) sur le chemin actuel de l’adresse de l’API web, exécutez la commande `ls` ou `dir` :</span><span class="sxs-lookup"><span data-stu-id="27919-141">To list the different endpoints (controllers) at the current path of the web API address, run the `ls` or `dir` command:</span></span>

```console
https://localhot:5001/~ ls
```

<span data-ttu-id="27919-142">Le format de sortie suivant s’affiche :</span><span class="sxs-lookup"><span data-stu-id="27919-142">The following output format is displayed:</span></span>

```console
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="27919-143">La sortie précédente indique que deux contrôleurs sont disponibles : `Fruits` et `People`.</span><span class="sxs-lookup"><span data-stu-id="27919-143">The preceding output indicates that there are two controllers available: `Fruits` and `People`.</span></span> <span data-ttu-id="27919-144">Les deux contrôleurs prennent en charge les opérations HTTP GET et POST sans paramètres.</span><span class="sxs-lookup"><span data-stu-id="27919-144">Both controllers support parameterless HTTP GET and POST operations.</span></span>

<span data-ttu-id="27919-145">La navigation dans un contrôleur spécifique révèle plus de détails.</span><span class="sxs-lookup"><span data-stu-id="27919-145">Navigating into a specific controller reveals more detail.</span></span> <span data-ttu-id="27919-146">Par exemple, la sortie de la commande suivante indique que le contrôleur `Fruits` prend également en charge les opérations HTTP GET, PUT et DELETE.</span><span class="sxs-lookup"><span data-stu-id="27919-146">For example, the following command's output shows the `Fruits` controller also supports HTTP GET, PUT, and DELETE operations.</span></span> <span data-ttu-id="27919-147">Chacune de ces opérations attend un paramètre `id` dans la route :</span><span class="sxs-lookup"><span data-stu-id="27919-147">Each of these operations expects an `id` parameter in the route:</span></span>

```console
https://localhost:5001/fruits~ ls
.      [get|post]
..     []
{id}   [get|put|delete]

https://localhost:5001/fruits~
```

<span data-ttu-id="27919-148">Vous pouvez également exécuter la commande `ui` pour ouvrir la page de l’interface utilisateur Swagger de l’API web dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="27919-148">Alternatively, run the `ui` command to open the web API's Swagger UI page in a browser.</span></span> <span data-ttu-id="27919-149">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="27919-149">For example:</span></span>

```console
https://localhost:5001/~ ui
```

### <a name="navigate-to-an-endpoint"></a><span data-ttu-id="27919-150">Accéder à un point de terminaison</span><span class="sxs-lookup"><span data-stu-id="27919-150">Navigate to an endpoint</span></span>

<span data-ttu-id="27919-151">Pour accéder à un autre point de terminaison sur l’API web, exécutez la commande `cd` :</span><span class="sxs-lookup"><span data-stu-id="27919-151">To navigate to a different endpoint on the web API, run the `cd` command:</span></span>

```console
https://localhost:5001/~ cd people
```

<span data-ttu-id="27919-152">Le chemin qui suit la commande `cd` ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="27919-152">The path following the `cd` command is case insensitive.</span></span> <span data-ttu-id="27919-153">Le format de sortie suivant s’affiche :</span><span class="sxs-lookup"><span data-stu-id="27919-153">The following output format is displayed:</span></span>

```console
/people    [get|post]

https://localhost:5001/people~
```

## <a name="customize-the-http-repl"></a><span data-ttu-id="27919-154">Personnaliser la boucle REPL HTTP</span><span class="sxs-lookup"><span data-stu-id="27919-154">Customize the HTTP REPL</span></span>

<span data-ttu-id="27919-155">Les [couleurs](#set-color-preferences) de la boucle REPL HTTP peuvent être personnalisées.</span><span class="sxs-lookup"><span data-stu-id="27919-155">The HTTP REPL's default [colors](#set-color-preferences) can be customized.</span></span> <span data-ttu-id="27919-156">Vous pouvez aussi définir un [éditeur de texte par défaut](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="27919-156">Additionally, a [default text editor](#set-the-default-text-editor) can be defined.</span></span> <span data-ttu-id="27919-157">Les préférences de la boucle REPL HTTP sont conservées dans la session active et dans les sessions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="27919-157">The HTTP REPL preferences are persisted across the current session and are honored in future sessions.</span></span> <span data-ttu-id="27919-158">Une fois modifiées, les préférences sont stockées dans le fichier suivant :</span><span class="sxs-lookup"><span data-stu-id="27919-158">Once modified, the preferences are stored in the following file:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="27919-159">Linux</span><span class="sxs-lookup"><span data-stu-id="27919-159">Linux</span></span>](#tab/linux)

<span data-ttu-id="27919-160">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="27919-160">*%HOME%/.httpreplprefs*</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="27919-161">macOS</span><span class="sxs-lookup"><span data-stu-id="27919-161">macOS</span></span>](#tab/macos)

<span data-ttu-id="27919-162">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="27919-162">*%HOME%/.httpreplprefs*</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="27919-163">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="27919-163">Windows</span></span>](#tab/windows)

<span data-ttu-id="27919-164">*%USERPROFILE%\\.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="27919-164">*%USERPROFILE%\\.httpreplprefs*</span></span>

---

<span data-ttu-id="27919-165">Le fichier *.httpreplprefs* est chargé au démarrage et ses modifications ne sont pas surveillées pendant l’exécution.</span><span class="sxs-lookup"><span data-stu-id="27919-165">The *.httpreplprefs* file is loaded on startup and not monitored for changes at runtime.</span></span> <span data-ttu-id="27919-166">Les modifications manuelles apportées au fichier prennent effet seulement après un redémarrage de l’outil.</span><span class="sxs-lookup"><span data-stu-id="27919-166">Manual modifications to the file take effect only after restarting the tool.</span></span>

### <a name="view-the-settings"></a><span data-ttu-id="27919-167">Voir les paramètres</span><span class="sxs-lookup"><span data-stu-id="27919-167">View the settings</span></span>

<span data-ttu-id="27919-168">Pour voir les paramètres disponibles, exécutez la commande `pref get`.</span><span class="sxs-lookup"><span data-stu-id="27919-168">To view the available settings, run the `pref get` command.</span></span> <span data-ttu-id="27919-169">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="27919-169">For example:</span></span>

```console
https://localhost:5001/~ pref get
```

<span data-ttu-id="27919-170">La commande précédente affiche les paires clé-valeur disponibles :</span><span class="sxs-lookup"><span data-stu-id="27919-170">The preceding command displays the available key-value pairs:</span></span>

```console
colors.json=Green
colors.json.arrayBrace=BoldCyan
colors.json.comma=BoldYellow
colors.json.name=BoldMagenta
colors.json.nameSeparator=BoldWhite
colors.json.objectBrace=Cyan
colors.protocol=BoldGreen
colors.status=BoldYellow
```

### <a name="set-color-preferences"></a><span data-ttu-id="27919-171">Définir les préférences de couleur</span><span class="sxs-lookup"><span data-stu-id="27919-171">Set color preferences</span></span>

<span data-ttu-id="27919-172">La colorisation des réponses est actuellement prise en charge seulement pour JSON.</span><span class="sxs-lookup"><span data-stu-id="27919-172">Response colorization is currently supported for JSON only.</span></span> <span data-ttu-id="27919-173">Pour personnaliser les couleurs par défaut de l’outil REPL HTTP, localisez la clé correspondant à la couleur à changer.</span><span class="sxs-lookup"><span data-stu-id="27919-173">To customize the default HTTP REPL tool coloring, locate the key corresponding to the color to be changed.</span></span> <span data-ttu-id="27919-174">Pour des instructions sur la façon de trouver les clés, consultez la section [Voir les paramètres](#view-the-settings).</span><span class="sxs-lookup"><span data-stu-id="27919-174">For instructions on how to find the keys, see the [View the settings](#view-the-settings) section.</span></span> <span data-ttu-id="27919-175">Par exemple, remplacez la valeur de la clé `colors.json` de `Green` par `White` :</span><span class="sxs-lookup"><span data-stu-id="27919-175">For example, change the `colors.json` key value from `Green` to `White` as follows:</span></span>

```console
https://localhost:5001/people~ pref set colors.json White
```

<span data-ttu-id="27919-176">Seules les [couleurs autorisées](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) peuvent être utilisées.</span><span class="sxs-lookup"><span data-stu-id="27919-176">Only the [allowed colors](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) may be used.</span></span> <span data-ttu-id="27919-177">Les requêtes HTTP suivantes affichent la sortie avec les nouvelles couleurs.</span><span class="sxs-lookup"><span data-stu-id="27919-177">Subsequent HTTP requests display output with the new coloring.</span></span>

<span data-ttu-id="27919-178">Quand des clés d’une couleur spécifique ne sont pas définies, des clés plus génériques sont prises en compte.</span><span class="sxs-lookup"><span data-stu-id="27919-178">When specific color keys aren't set, more generic keys are considered.</span></span> <span data-ttu-id="27919-179">Pour illustrer ce comportement de repli, considérez l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="27919-179">To demonstrate this fallback behavior, consider the following example:</span></span>

* <span data-ttu-id="27919-180">Si `colors.json.name` n’a pas de valeur, `colors.json.string` est utilisée.</span><span class="sxs-lookup"><span data-stu-id="27919-180">If `colors.json.name` doesn't have a value, `colors.json.string` is used.</span></span>
* <span data-ttu-id="27919-181">Si `colors.json.string` n’a pas de valeur, `colors.json.literal` est utilisée.</span><span class="sxs-lookup"><span data-stu-id="27919-181">If `colors.json.string` doesn't have a value, `colors.json.literal` is used.</span></span>
* <span data-ttu-id="27919-182">Si `colors.json.literal` n’a pas de valeur, `colors.json` est utilisée.</span><span class="sxs-lookup"><span data-stu-id="27919-182">If `colors.json.literal` doesn't have a value, `colors.json` is used.</span></span> 
* <span data-ttu-id="27919-183">Si `colors.json` n’a pas de valeur, la couleur de texte par défaut de l’interpréteur de commandes (`AllowedColors.None`) est utilisée.</span><span class="sxs-lookup"><span data-stu-id="27919-183">If `colors.json` doesn't have a value, the command shell's default text color (`AllowedColors.None`) is used.</span></span>

### <a name="set-indentation-size"></a><span data-ttu-id="27919-184">Définir la taille de la mise en retrait</span><span class="sxs-lookup"><span data-stu-id="27919-184">Set indentation size</span></span>

<span data-ttu-id="27919-185">La personnalisation de la taille de la mise en retrait de la réponse est actuellement prise en charge pour JSON uniquement.</span><span class="sxs-lookup"><span data-stu-id="27919-185">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="27919-186">La taille par défaut est de deux espaces.</span><span class="sxs-lookup"><span data-stu-id="27919-186">The default size is two spaces.</span></span> <span data-ttu-id="27919-187">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="27919-187">For example:</span></span>

```json
[
  {
    "id": 1,
    "name": "Apple"
  },
  {
    "id": 2,
    "name": "Orange"
  },
  {
    "id": 3,
    "name": "Strawberry"
  }
]
```

<span data-ttu-id="27919-188">Pour changer la taille par défaut, définissez la clé `formatting.json.indentSize`.</span><span class="sxs-lookup"><span data-stu-id="27919-188">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="27919-189">Par exemple, pour toujours utiliser quatre espaces :</span><span class="sxs-lookup"><span data-stu-id="27919-189">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="27919-190">Les réponses suivantes adoptent la valeur correspondant à quatre espaces :</span><span class="sxs-lookup"><span data-stu-id="27919-190">Subsequent responses honor the setting of four spaces:</span></span>

```json
[
    {
        "id": 1,
        "name": "Apple"
    },
    {
        "id": 2,
        "name": "Orange"
    },
    {
        "id": 3,
        "name": "Strawberry"
    }
]
```

### <a name="set-indentation-size"></a><span data-ttu-id="27919-191">Définir la taille de la mise en retrait</span><span class="sxs-lookup"><span data-stu-id="27919-191">Set indentation size</span></span>

<span data-ttu-id="27919-192">La personnalisation de la taille de la mise en retrait de la réponse est actuellement prise en charge pour JSON uniquement.</span><span class="sxs-lookup"><span data-stu-id="27919-192">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="27919-193">La taille par défaut est de deux espaces.</span><span class="sxs-lookup"><span data-stu-id="27919-193">The default size is two spaces.</span></span> <span data-ttu-id="27919-194">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="27919-194">For example:</span></span>

```json
[
  {
    "id": 1,
    "name": "Apple"
  },
  {
    "id": 2,
    "name": "Orange"
  },
  {
    "id": 3,
    "name": "Strawberry"
  }
]
```

<span data-ttu-id="27919-195">Pour changer la taille par défaut, définissez la clé `formatting.json.indentSize`.</span><span class="sxs-lookup"><span data-stu-id="27919-195">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="27919-196">Par exemple, pour toujours utiliser quatre espaces :</span><span class="sxs-lookup"><span data-stu-id="27919-196">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="27919-197">Les réponses suivantes adoptent la valeur correspondant à quatre espaces :</span><span class="sxs-lookup"><span data-stu-id="27919-197">Subsequent responses honor the setting of four spaces:</span></span>

```json
[
    {
        "id": 1,
        "name": "Apple"
    },
    {
        "id": 2,
        "name": "Orange"
    },
    {
        "id": 3,
        "name": "Strawberry"
    }
]
```

### <a name="set-the-default-text-editor"></a><span data-ttu-id="27919-198">Définir l’éditeur de texte par défaut</span><span class="sxs-lookup"><span data-stu-id="27919-198">Set the default text editor</span></span>

<span data-ttu-id="27919-199">Par défaut, la boucle REPL HTTP n’a pas d’éditeur de texte configuré pour l’utilisation.</span><span class="sxs-lookup"><span data-stu-id="27919-199">By default, the HTTP REPL has no text editor configured for use.</span></span> <span data-ttu-id="27919-200">Pour tester les méthodes d’API web nécessitant un corps de requête HTTP, vous devez définir un éditeur de texte par défaut.</span><span class="sxs-lookup"><span data-stu-id="27919-200">To test web API methods requiring an HTTP request body, a default text editor must be set.</span></span> <span data-ttu-id="27919-201">L’outil REPL HTTP lance l’éditeur de texte configuré dans le seul but de permettre la composition du corps de la requête.</span><span class="sxs-lookup"><span data-stu-id="27919-201">The HTTP REPL tool launches the configured text editor for the sole purpose of composing the request body.</span></span> <span data-ttu-id="27919-202">Exécutez la commande suivante pour définir votre éditeur de texte préféré comme éditeur par défaut :</span><span class="sxs-lookup"><span data-stu-id="27919-202">Run the following command to set your preferred text editor as the default:</span></span>

```console
pref set editor.command.default "<EXECUTABLE>"
```

<span data-ttu-id="27919-203">Dans la commande précédente, `<EXECUTABLE>` est le chemin complet du fichier exécutable de l’éditeur de texte.</span><span class="sxs-lookup"><span data-stu-id="27919-203">In the preceding command, `<EXECUTABLE>` is the full path to the text editor's executable file.</span></span> <span data-ttu-id="27919-204">Par exemple, exécutez la commande suivante pour définir Visual Studio Code comme éditeur de texte par défaut :</span><span class="sxs-lookup"><span data-stu-id="27919-204">For example, run the following command to set Visual Studio Code as the default text editor:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="27919-205">Linux</span><span class="sxs-lookup"><span data-stu-id="27919-205">Linux</span></span>](#tab/linux)

```console
pref set editor.command.default "/usr/bin/code"
```

# <a name="macostabmacos"></a>[<span data-ttu-id="27919-206">macOS</span><span class="sxs-lookup"><span data-stu-id="27919-206">macOS</span></span>](#tab/macos)

```console
pref set editor.command.default "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code"
```

# <a name="windowstabwindows"></a>[<span data-ttu-id="27919-207">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="27919-207">Windows</span></span>](#tab/windows)

```console
pref set editor.command.default "C:\Program Files\Microsoft VS Code\Code.exe"
```

---

<span data-ttu-id="27919-208">Pour lancer l’éditeur de texte par défaut avec des arguments CLI spécifiques, définissez la clé `editor.command.default.arguments`.</span><span class="sxs-lookup"><span data-stu-id="27919-208">To launch the default text editor with specific CLI arguments, set the `editor.command.default.arguments` key.</span></span> <span data-ttu-id="27919-209">Par exemple, supposons que Visual Studio Code est l’éditeur de texte par défaut et que vous voulez que la boucle REPL HTTP ouvre toujours Visual Studio Code dans une nouvelle session avec les extensions désactivées.</span><span class="sxs-lookup"><span data-stu-id="27919-209">For example, assume Visual Studio Code is the default text editor and that you always want the HTTP REPL to open Visual Studio Code in a new session with extensions disabled.</span></span> <span data-ttu-id="27919-210">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="27919-210">Run the following command:</span></span>

```console
pref set editor.command.default.arguments "--disable-extensions --new-window"
```

### <a name="set-the-swagger-search-paths"></a><span data-ttu-id="27919-211">Définir les chemins de recherche Swagger</span><span class="sxs-lookup"><span data-stu-id="27919-211">Set the Swagger search paths</span></span>

<span data-ttu-id="27919-212">Par défaut, HTTP REPL possède un ensemble de chemins relatifs qu’il utilise pour rechercher le document Swagger lors de l'exécution de la commande `connect` sans l’option `--swagger`.</span><span class="sxs-lookup"><span data-stu-id="27919-212">By default, the HTTP REPL has a set of relative paths that it uses to find the Swagger document when executing the `connect` command without the `--swagger` option.</span></span> <span data-ttu-id="27919-213">Ces chemins relatifs sont combinés avec les chemins racine et de base spécifiés dans la commande `connect`.</span><span class="sxs-lookup"><span data-stu-id="27919-213">These relative paths are combined with the root and base paths specified in the `connect` command.</span></span> <span data-ttu-id="27919-214">Les chemins relatifs par défaut sont les suivants :</span><span class="sxs-lookup"><span data-stu-id="27919-214">The default relative paths are:</span></span>

- <span data-ttu-id="27919-215">*swagger.json*</span><span class="sxs-lookup"><span data-stu-id="27919-215">*swagger.json*</span></span>
- <span data-ttu-id="27919-216">*swagger/v1/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="27919-216">*swagger/v1/swagger.json*</span></span>
- <span data-ttu-id="27919-217">*/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="27919-217">*/swagger.json*</span></span>
- <span data-ttu-id="27919-218">*/swagger/v1/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="27919-218">*/swagger/v1/swagger.json*</span></span>

<span data-ttu-id="27919-219">Pour utiliser un autre ensemble de chemins de recherche dans votre environnement, définissez la préférence `swagger.searchPaths`.</span><span class="sxs-lookup"><span data-stu-id="27919-219">To use a different set of search paths in your environment, set the `swagger.searchPaths` preference.</span></span> <span data-ttu-id="27919-220">La valeur doit être une liste de chemins relatifs délimités par des barres verticales.</span><span class="sxs-lookup"><span data-stu-id="27919-220">The value must be a pipe-delimited list of relative paths.</span></span> <span data-ttu-id="27919-221">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="27919-221">For example:</span></span>

```console
pref set swagger.searchPaths "swagger/v2/swagger.json|swagger/v3/swagger.json
```

## <a name="test-http-get-requests"></a><span data-ttu-id="27919-222">Tester des requêtes HTTP GET</span><span class="sxs-lookup"><span data-stu-id="27919-222">Test HTTP GET requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="27919-223">Résumé</span><span class="sxs-lookup"><span data-stu-id="27919-223">Synopsis</span></span>

```console
get <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="27919-224">Arguments</span><span class="sxs-lookup"><span data-stu-id="27919-224">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="27919-225">Paramètre de route, le cas échéant, attendu par la méthode d’action du contrôleur associé.</span><span class="sxs-lookup"><span data-stu-id="27919-225">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="27919-226">Options</span><span class="sxs-lookup"><span data-stu-id="27919-226">Options</span></span>

<span data-ttu-id="27919-227">Les options suivantes sont disponibles pour la commande `get` :</span><span class="sxs-lookup"><span data-stu-id="27919-227">The following options are available for the `get` command:</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="27919-228">Exemples</span><span class="sxs-lookup"><span data-stu-id="27919-228">Example</span></span>

<span data-ttu-id="27919-229">Pour émettre une requête HTTP GET :</span><span class="sxs-lookup"><span data-stu-id="27919-229">To issue an HTTP GET request:</span></span>

1. <span data-ttu-id="27919-230">Exécutez la commande `get` sur un point de terminaison qui la prend en charge :</span><span class="sxs-lookup"><span data-stu-id="27919-230">Run the `get` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ get
    ```

    <span data-ttu-id="27919-231">La commande précédente affiche le format de sortie suivant :</span><span class="sxs-lookup"><span data-stu-id="27919-231">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Fri, 21 Jun 2019 03:38:45 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "name": "Scott Hunter"
      },
      {
        "id": 2,
        "name": "Scott Hanselman"
      },
      {
        "id": 3,
        "name": "Scott Guthrie"
      }
    ]


    https://localhost:5001/people~
    ```

1. <span data-ttu-id="27919-232">Récupérez un enregistrement spécifique en passant un paramètre à la commande `get` :</span><span class="sxs-lookup"><span data-stu-id="27919-232">Retrieve a specific record by passing a parameter to the `get` command:</span></span>

    ```console
    https://localhost:5001/people~ get 2
    ```

    <span data-ttu-id="27919-233">La commande précédente affiche le format de sortie suivant :</span><span class="sxs-lookup"><span data-stu-id="27919-233">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Fri, 21 Jun 2019 06:17:57 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 2,
        "name": "Scott Hanselman"
      }
    ]


    https://localhost:5001/people~
    ```

## <a name="test-http-post-requests"></a><span data-ttu-id="27919-234">Tester des requêtes HTTP POST</span><span class="sxs-lookup"><span data-stu-id="27919-234">Test HTTP POST requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="27919-235">Résumé</span><span class="sxs-lookup"><span data-stu-id="27919-235">Synopsis</span></span>

```console
post <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="27919-236">Arguments</span><span class="sxs-lookup"><span data-stu-id="27919-236">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="27919-237">Paramètre de route, le cas échéant, attendu par la méthode d’action du contrôleur associé.</span><span class="sxs-lookup"><span data-stu-id="27919-237">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="27919-238">Options</span><span class="sxs-lookup"><span data-stu-id="27919-238">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="27919-239">Exemples</span><span class="sxs-lookup"><span data-stu-id="27919-239">Example</span></span>

<span data-ttu-id="27919-240">Pour émettre une requête HTTP POST :</span><span class="sxs-lookup"><span data-stu-id="27919-240">To issue an HTTP POST request:</span></span>

1. <span data-ttu-id="27919-241">Exécutez la commande `post` sur un point de terminaison qui la prend en charge :</span><span class="sxs-lookup"><span data-stu-id="27919-241">Run the `post` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ post -h Content-Type=application/json
    ```

    <span data-ttu-id="27919-242">Dans la commande précédente, l’en-tête `Content-Type` de la requête HTTP est défini pour indiquer un type de média de corps de requête JSON.</span><span class="sxs-lookup"><span data-stu-id="27919-242">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="27919-243">L’éditeur de texte par défaut ouvre un fichier *.tmp* avec un modèle JSON représentant le corps de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="27919-243">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="27919-244">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="27919-244">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="27919-245">Pour définir l’éditeur de texte par défaut, consultez la section [Définir l’éditeur de texte par défaut](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="27919-245">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="27919-246">Modifiez le modèle JSON pour répondre aux exigences de validation du modèle :</span><span class="sxs-lookup"><span data-stu-id="27919-246">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 0,
      "name": "Scott Addie"
    }
    ```

1. <span data-ttu-id="27919-247">Enregistrez le fichier *.tmp* et fermez l’éditeur de texte.</span><span class="sxs-lookup"><span data-stu-id="27919-247">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="27919-248">La sortie suivante s’affiche dans l’interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="27919-248">The following output appears in the command shell:</span></span>

    ```console
    HTTP/1.1 201 Created
    Content-Type: application/json; charset=utf-8
    Date: Thu, 27 Jun 2019 21:24:18 GMT
    Location: https://localhost:5001/people/4
    Server: Kestrel
    Transfer-Encoding: chunked

    {
      "id": 4,
      "name": "Scott Addie"
    }


    https://localhost:5001/people~
    ```

## <a name="test-http-put-requests"></a><span data-ttu-id="27919-249">Tester des requêtes HTTP PUT</span><span class="sxs-lookup"><span data-stu-id="27919-249">Test HTTP PUT requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="27919-250">Résumé</span><span class="sxs-lookup"><span data-stu-id="27919-250">Synopsis</span></span>

```console
put <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="27919-251">Arguments</span><span class="sxs-lookup"><span data-stu-id="27919-251">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="27919-252">Paramètre de route, le cas échéant, attendu par la méthode d’action du contrôleur associé.</span><span class="sxs-lookup"><span data-stu-id="27919-252">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="27919-253">Options</span><span class="sxs-lookup"><span data-stu-id="27919-253">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="27919-254">Exemples</span><span class="sxs-lookup"><span data-stu-id="27919-254">Example</span></span>

<span data-ttu-id="27919-255">Pour émettre une requête HTTP PUT :</span><span class="sxs-lookup"><span data-stu-id="27919-255">To issue an HTTP PUT request:</span></span>

1. <span data-ttu-id="27919-256">*Facultatif* : Exécutez la commande `get` pour afficher les données avant de les modifier :</span><span class="sxs-lookup"><span data-stu-id="27919-256">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:07:32 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 2,
        "data": "Orange"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]

1. Run the `put` command on an endpoint that supports it:

    ```console
    https://localhost:5001/fruits~ put 2 -h Content-Type=application/json
    ```

    <span data-ttu-id="27919-257">Dans la commande précédente, l’en-tête `Content-Type` de la requête HTTP est défini pour indiquer un type de média de corps de requête JSON.</span><span class="sxs-lookup"><span data-stu-id="27919-257">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="27919-258">L’éditeur de texte par défaut ouvre un fichier *.tmp* avec un modèle JSON représentant le corps de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="27919-258">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="27919-259">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="27919-259">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="27919-260">Pour définir l’éditeur de texte par défaut, consultez la section [Définir l’éditeur de texte par défaut](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="27919-260">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="27919-261">Modifiez le modèle JSON pour répondre aux exigences de validation du modèle :</span><span class="sxs-lookup"><span data-stu-id="27919-261">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 2,
      "name": "Cherry"
    }
    ```

1. <span data-ttu-id="27919-262">Enregistrez le fichier *.tmp* et fermez l’éditeur de texte.</span><span class="sxs-lookup"><span data-stu-id="27919-262">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="27919-263">La sortie suivante s’affiche dans l’interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="27919-263">The following output appears in the command shell:</span></span>

    ```console
    [main 2019-06-28T17:27:01.805Z] update#setState idle
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:28:21 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="27919-264">*Facultatif* : Émettez une commande `get` pour voir les modifications.</span><span class="sxs-lookup"><span data-stu-id="27919-264">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="27919-265">Par exemple, si vous avez tapé « Cherry » dans l’éditeur de texte, une commande `get` retourne ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="27919-265">For example, if you typed "Cherry" in the text editor, a `get` returns the following:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:08:20 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 2,
        "data": "Cherry"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]


    https://localhost:5001/fruits~
    ```

## <a name="test-http-delete-requests"></a><span data-ttu-id="27919-266">Tester des requêtes HTTP DELETE</span><span class="sxs-lookup"><span data-stu-id="27919-266">Test HTTP DELETE requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="27919-267">Résumé</span><span class="sxs-lookup"><span data-stu-id="27919-267">Synopsis</span></span>

```console
delete <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="27919-268">Arguments</span><span class="sxs-lookup"><span data-stu-id="27919-268">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="27919-269">Paramètre de route, le cas échéant, attendu par la méthode d’action du contrôleur associé.</span><span class="sxs-lookup"><span data-stu-id="27919-269">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="27919-270">Options</span><span class="sxs-lookup"><span data-stu-id="27919-270">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="27919-271">Exemples</span><span class="sxs-lookup"><span data-stu-id="27919-271">Example</span></span>

<span data-ttu-id="27919-272">Pour émettre une requête HTTP DELETE :</span><span class="sxs-lookup"><span data-stu-id="27919-272">To issue an HTTP DELETE request:</span></span>

1. <span data-ttu-id="27919-273">*Facultatif* : Exécutez la commande `get` pour voir les données avant de les modifier :</span><span class="sxs-lookup"><span data-stu-id="27919-273">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:07:32 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 2,
        "data": "Orange"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]

1. Run the `delete` command on an endpoint that supports it:

    ```console
    https://localhost:5001/fruits~ delete 2
    ```

    <span data-ttu-id="27919-274">La commande précédente affiche le format de sortie suivant :</span><span class="sxs-lookup"><span data-stu-id="27919-274">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:36:42 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="27919-275">*Facultatif* : Émettez une commande `get` pour voir les modifications.</span><span class="sxs-lookup"><span data-stu-id="27919-275">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="27919-276">Dans cet exemple, une commande `get` retourne ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="27919-276">In this example, a `get` returns the following:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:16:30 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]


    https://localhost:5001/fruits~
    ```

## <a name="test-http-patch-requests"></a><span data-ttu-id="27919-277">Tester des requêtes HTTP PATCH</span><span class="sxs-lookup"><span data-stu-id="27919-277">Test HTTP PATCH requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="27919-278">Résumé</span><span class="sxs-lookup"><span data-stu-id="27919-278">Synopsis</span></span>

```console
patch <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="27919-279">Arguments</span><span class="sxs-lookup"><span data-stu-id="27919-279">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="27919-280">Paramètre de route, le cas échéant, attendu par la méthode d’action du contrôleur associé.</span><span class="sxs-lookup"><span data-stu-id="27919-280">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="27919-281">Options</span><span class="sxs-lookup"><span data-stu-id="27919-281">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

## <a name="test-http-head-requests"></a><span data-ttu-id="27919-282">Tester des requêtes HTTP HEAD</span><span class="sxs-lookup"><span data-stu-id="27919-282">Test HTTP HEAD requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="27919-283">Résumé</span><span class="sxs-lookup"><span data-stu-id="27919-283">Synopsis</span></span>

```console
head <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="27919-284">Arguments</span><span class="sxs-lookup"><span data-stu-id="27919-284">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="27919-285">Paramètre de route, le cas échéant, attendu par la méthode d’action du contrôleur associé.</span><span class="sxs-lookup"><span data-stu-id="27919-285">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="27919-286">Options</span><span class="sxs-lookup"><span data-stu-id="27919-286">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="test-http-options-requests"></a><span data-ttu-id="27919-287">Tester des requêtes HTTP OPTIONS</span><span class="sxs-lookup"><span data-stu-id="27919-287">Test HTTP OPTIONS requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="27919-288">Résumé</span><span class="sxs-lookup"><span data-stu-id="27919-288">Synopsis</span></span>

```console
options <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="27919-289">Arguments</span><span class="sxs-lookup"><span data-stu-id="27919-289">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="27919-290">Paramètre de route, le cas échéant, attendu par la méthode d’action du contrôleur associé.</span><span class="sxs-lookup"><span data-stu-id="27919-290">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="27919-291">Options</span><span class="sxs-lookup"><span data-stu-id="27919-291">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="set-http-request-headers"></a><span data-ttu-id="27919-292">Définir des en-têtes de requête HTTP</span><span class="sxs-lookup"><span data-stu-id="27919-292">Set HTTP request headers</span></span>

<span data-ttu-id="27919-293">Pour définir un en-tête de requête HTTP, utilisez une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="27919-293">To set an HTTP request header, use one of the following approaches:</span></span>

1. <span data-ttu-id="27919-294">Définir inline avec la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="27919-294">Set inline with the HTTP request.</span></span> <span data-ttu-id="27919-295">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="27919-295">For example:</span></span>

  ```console
  https://localhost:5001/people~ post -h Content-Type=application/json
  ```

  <span data-ttu-id="27919-296">Avec l’approche précédente, chaque en-tête de requête HTTP distinct nécessite sa propre option `-h`.</span><span class="sxs-lookup"><span data-stu-id="27919-296">With the preceding approach, each distinct HTTP request header requires its own `-h` option.</span></span>

1. <span data-ttu-id="27919-297">Définir avant l’envoi de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="27919-297">Set before sending the HTTP request.</span></span> <span data-ttu-id="27919-298">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="27919-298">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type application/json
  ```

  <span data-ttu-id="27919-299">Si l’en-tête est défini avant l’envoi d’une requête, l’en-tête reste défini pour la durée de la session de l’interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="27919-299">When setting the header before sending a request, the header remains set for the duration of the command shell session.</span></span> <span data-ttu-id="27919-300">Pour effacer l’en-tête, spécifiez une valeur vide.</span><span class="sxs-lookup"><span data-stu-id="27919-300">To clear the header, provide an empty value.</span></span> <span data-ttu-id="27919-301">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="27919-301">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type
  ```

## <a name="toggle-http-request-display"></a><span data-ttu-id="27919-302">Activer/désactiver l’affichage des requêtes HTTP</span><span class="sxs-lookup"><span data-stu-id="27919-302">Toggle HTTP request display</span></span>

<span data-ttu-id="27919-303">Par défaut, l’affichage de la requête HTTP envoyée est supprimé.</span><span class="sxs-lookup"><span data-stu-id="27919-303">By default, display of the HTTP request being sent is suppressed.</span></span> <span data-ttu-id="27919-304">Il est possible de changer le paramètre correspondant pour la durée de la session de l’interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="27919-304">It's possible to change the corresponding setting for the duration of the command shell session.</span></span>

### <a name="enable-request-display"></a><span data-ttu-id="27919-305">Activer l’affichage des requêtes</span><span class="sxs-lookup"><span data-stu-id="27919-305">Enable request display</span></span>

<span data-ttu-id="27919-306">Affichez la requête HTTP envoyée en exécutant la commande `echo on`.</span><span class="sxs-lookup"><span data-stu-id="27919-306">View the HTTP request being sent by running the `echo on` command.</span></span> <span data-ttu-id="27919-307">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="27919-307">For example:</span></span>

```console
https://localhost:5001/people~ echo on
Request echoing is on
```

<span data-ttu-id="27919-308">Les requêtes HTTP suivantes dans la session active affichent les en-têtes de requête.</span><span class="sxs-lookup"><span data-stu-id="27919-308">Subsequent HTTP requests in the current session display the request headers.</span></span> <span data-ttu-id="27919-309">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="27919-309">For example:</span></span>

```console
https://localhost:5001/people~ post

[main 2019-06-28T18:50:11.930Z] update#setState idle
Request to https://localhost:5001...

POST /people HTTP/1.1
Content-Length: 41
Content-Type: application/json
User-Agent: HTTP-REPL

{
  "id": 0,
  "name": "Scott Addie"
}

Response from https://localhost:5001...

HTTP/1.1 201 Created
Content-Type: application/json; charset=utf-8
Date: Fri, 28 Jun 2019 18:50:21 GMT
Location: https://localhost:5001/people/4
Server: Kestrel
Transfer-Encoding: chunked

{
  "id": 4,
  "name": "Scott Addie"
}


https://localhost:5001/people~
```

### <a name="disable-request-display"></a><span data-ttu-id="27919-310">Désactiver l’affichage des requêtes</span><span class="sxs-lookup"><span data-stu-id="27919-310">Disable request display</span></span>

<span data-ttu-id="27919-311">Supprimez l’affichage de la requête HTTP envoyée en exécutant la commande `echo off`.</span><span class="sxs-lookup"><span data-stu-id="27919-311">Suppress display of the HTTP request being sent by running the `echo off` command.</span></span> <span data-ttu-id="27919-312">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="27919-312">For example:</span></span>

```console
https://localhost:5001/people~ echo off
Request echoing is off
```

## <a name="run-a-script"></a><span data-ttu-id="27919-313">Exécuter un script</span><span class="sxs-lookup"><span data-stu-id="27919-313">Run a script</span></span>

<span data-ttu-id="27919-314">Si vous exécutez fréquemment le même jeu de commandes REPL HTTP, envisagez de les stocker dans un fichier texte.</span><span class="sxs-lookup"><span data-stu-id="27919-314">If you frequently execute the same set of HTTP REPL commands, consider storing them in a text file.</span></span> <span data-ttu-id="27919-315">Les commandes placées dans le fichier sont de la même forme que celles exécutées manuellement sur la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="27919-315">Commands in the file take the same form as those executed manually on the command line.</span></span> <span data-ttu-id="27919-316">Les commandes peuvent être exécutées de façon groupée avec la commande `run`.</span><span class="sxs-lookup"><span data-stu-id="27919-316">The commands can be executed in a batched fashion using the `run` command.</span></span> <span data-ttu-id="27919-317">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="27919-317">For example:</span></span>

1. <span data-ttu-id="27919-318">Créez un fichier texte contenant un ensemble de commandes délimitées par des sauts de ligne.</span><span class="sxs-lookup"><span data-stu-id="27919-318">Create a text file containing a set of newline-delimited commands.</span></span> <span data-ttu-id="27919-319">Pour illustrer ceci, considérez un fichier *people-script.txt* contenant les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="27919-319">To illustrate, consider a *people-script.txt* file containing the following commands:</span></span>

    ```text
    set base https://localhost:5001
    ls
    cd People
    ls
    get 1
    ```

1. <span data-ttu-id="27919-320">Exécutez la commande `run`, en passant le chemin du fichier texte.</span><span class="sxs-lookup"><span data-stu-id="27919-320">Execute the `run` command, passing in the text file's path.</span></span> <span data-ttu-id="27919-321">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="27919-321">For example:</span></span>

    ```console
    https://localhost:5001/~ run C:\http-repl-scripts\people-script.txt
    ```

    <span data-ttu-id="27919-322">La sortie suivante apparaît :</span><span class="sxs-lookup"><span data-stu-id="27919-322">The following output appears:</span></span>

    ```console
    https://localhost:5001/~ set base https://localhost:5001
    Using swagger metadata from https://localhost:5001/swagger/v1/swagger.json
    
    https://localhost:5001/~ ls
    .        []
    Fruits   [get|post]
    People   [get|post]
    
    https://localhost:5001/~ cd People
    /People    [get|post]
    
    https://localhost:5001/People~ ls
    .      [get|post]
    ..     []
    {id}   [get|put|delete]
    
    https://localhost:5001/People~ get 1
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Fri, 12 Jul 2019 19:20:10 GMT
    Server: Kestrel
    Transfer-Encoding: chunked
    
    {
      "id": 1,
      "name": "Scott Hunter"
    }
    
    
    https://localhost:5001/People~
    ```

## <a name="clear-the-output"></a><span data-ttu-id="27919-323">Effacer la sortie</span><span class="sxs-lookup"><span data-stu-id="27919-323">Clear the output</span></span>

<span data-ttu-id="27919-324">Pour supprimer toutes les sorties écrites dans l’interpréteur de commandes par l’outil REPL HTTP, exécutez la commande `clear` ou `cls`.</span><span class="sxs-lookup"><span data-stu-id="27919-324">To remove all output written to the command shell by the HTTP REPL tool, run the `clear` or `cls` command.</span></span> <span data-ttu-id="27919-325">À titre d’illustration, imaginez que l’interpréteur de commandes contient la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="27919-325">To illustrate, imagine the command shell contains the following output:</span></span>

```console
dotnet httprepl https://localhost:5001
(Disconnected)~ set base "https://localhost:5001"
Using swagger metadata from https://localhost:5001/swagger/v1/swagger.json

https://localhost:5001/~ ls
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="27919-326">Exécutez la commande suivante pour effacer la sortie :</span><span class="sxs-lookup"><span data-stu-id="27919-326">Run the following command to clear the output:</span></span>

```console
https://localhost:5001/~ clear
```

<span data-ttu-id="27919-327">Après l’exécution de la commande précédente, l’interpréteur de commandes contient seulement la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="27919-327">After running the preceding command, the command shell contains only the following output:</span></span>

```console
https://localhost:5001/~
```

## <a name="additional-resources"></a><span data-ttu-id="27919-328">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="27919-328">Additional resources</span></span>

* [<span data-ttu-id="27919-329">Requêtes de l’API REST</span><span class="sxs-lookup"><span data-stu-id="27919-329">REST API requests</span></span>](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)
* [<span data-ttu-id="27919-330">Dépôt GitHub REPL HTTP</span><span class="sxs-lookup"><span data-stu-id="27919-330">HTTP REPL GitHub repository</span></span>](https://github.com/aspnet/HttpRepl)
