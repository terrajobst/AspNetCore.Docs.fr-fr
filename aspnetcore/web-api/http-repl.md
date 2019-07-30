---
title: Tester des API web avec la boucle REPL HTTP
author: scottaddie
description: Découvrez comment utiliser l’outil global REPL HTTP de .NET Core pour parcourir et tester une API web ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/23/2019
uid: web-api/http-repl
ms.openlocfilehash: 1ceda6182c62bb1be06cd95f14e6a46a1809253e
ms.sourcegitcommit: 059ab380744fa3be3b69aa90d431b563c57092cf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68410886"
---
# <a name="test-web-apis-with-the-http-repl"></a><span data-ttu-id="f731a-103">Tester des API web avec la boucle REPL HTTP</span><span class="sxs-lookup"><span data-stu-id="f731a-103">Test web APIs with the HTTP REPL</span></span>

<span data-ttu-id="f731a-104">Par [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="f731a-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="f731a-105">La boucle REPL (Read-Eval-Print Loop) HTTP est :</span><span class="sxs-lookup"><span data-stu-id="f731a-105">The HTTP Read-Eval-Print Loop (REPL) is:</span></span>

* <span data-ttu-id="f731a-106">Un outil en ligne de commande léger et multiplateforme qui est pris en charge partout où .NET Core est pris en charge.</span><span class="sxs-lookup"><span data-stu-id="f731a-106">A lightweight, cross-platform command-line tool that's supported everywhere .NET Core is supported.</span></span>
* <span data-ttu-id="f731a-107">Utilisé pour effectuer des requêtes HTTP afin de tester des API web ASP.NET Core et de voir leurs résultats.</span><span class="sxs-lookup"><span data-stu-id="f731a-107">Used for making HTTP requests to test ASP.NET Core web APIs and view their results.</span></span>

<span data-ttu-id="f731a-108">Les [verbes HTTP](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) suivants sont pris en charge :</span><span class="sxs-lookup"><span data-stu-id="f731a-108">The following [HTTP verbs](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) are supported:</span></span>

* [<span data-ttu-id="f731a-109">DELETE</span><span class="sxs-lookup"><span data-stu-id="f731a-109">DELETE</span></span>](#test-http-delete-requests)
* [<span data-ttu-id="f731a-110">GET</span><span class="sxs-lookup"><span data-stu-id="f731a-110">GET</span></span>](#test-http-get-requests)
* [<span data-ttu-id="f731a-111">HEAD</span><span class="sxs-lookup"><span data-stu-id="f731a-111">HEAD</span></span>](#test-http-head-requests)
* [<span data-ttu-id="f731a-112">OPTIONS</span><span class="sxs-lookup"><span data-stu-id="f731a-112">OPTIONS</span></span>](#test-http-options-requests)
* [<span data-ttu-id="f731a-113">PATCH</span><span class="sxs-lookup"><span data-stu-id="f731a-113">PATCH</span></span>](#test-http-patch-requests)
* [<span data-ttu-id="f731a-114">POST</span><span class="sxs-lookup"><span data-stu-id="f731a-114">POST</span></span>](#test-http-post-requests)
* [<span data-ttu-id="f731a-115">PUT</span><span class="sxs-lookup"><span data-stu-id="f731a-115">PUT</span></span>](#test-http-put-requests)

<span data-ttu-id="f731a-116">Pour continuer, [consultez ou téléchargez l’exemple d’API web ASP.NET Core](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([comment télécharger](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f731a-116">To follow along, [view or download the sample ASP.NET Core web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f731a-117">Prérequis</span><span class="sxs-lookup"><span data-stu-id="f731a-117">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](~/includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="f731a-118">Installation</span><span class="sxs-lookup"><span data-stu-id="f731a-118">Installation</span></span>

<span data-ttu-id="f731a-119">Pour installer la boucle REPL HTTP, exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="f731a-119">To install the HTTP REPL, run the following command:</span></span>

```console
dotnet tool install -g Microsoft.dotnet-httprepl --version 3.0.0-*
```

<span data-ttu-id="f731a-120">Un [outil global .NET Core](/dotnet/core/tools/global-tools#install-a-global-tool) est installé à partir du package NuGet [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl).</span><span class="sxs-lookup"><span data-stu-id="f731a-120">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl) NuGet package.</span></span>

## <a name="usage"></a><span data-ttu-id="f731a-121">Utilisation</span><span class="sxs-lookup"><span data-stu-id="f731a-121">Usage</span></span>

<span data-ttu-id="f731a-122">Une fois l’installation de l’outil réussie, exécutez la commande suivante pour démarrer la boucle REPL HTTP :</span><span class="sxs-lookup"><span data-stu-id="f731a-122">After successful installation of the tool, run the following command to start the HTTP REPL:</span></span>

```console
dotnet httprepl
```

<span data-ttu-id="f731a-123">Pour voir les commandes de la boucle REPL HTTP disponibles, exécutez une des commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="f731a-123">To view the available HTTP REPL commands, run one of the following commands:</span></span>

```console
dotnet httprepl -h
```

```console
dotnet httprepl --help
```

<span data-ttu-id="f731a-124">La sortie suivante s’affiche :</span><span class="sxs-lookup"><span data-stu-id="f731a-124">The following output is displayed:</span></span>

```console
Usage:
  dotnet httprepl [<BASE_ADDRESS>] [options]

Arguments:
  <BASE_ADDRESS> - The initial base address for the REPL.

Options:
  -h|--help - Show help information.

Once the REPL starts, these commands are valid:

HTTP Commands:
Use these commands to execute requests against your application.

GET            get - Issues a GET request
POST           post - Issues a POST request
PUT            put - Issues a PUT request
DELETE         delete - Issues a DELETE request
PATCH          patch - Issues a PATCH request
HEAD           head - Issues a HEAD request
OPTIONS        options - Issues a OPTIONS request

set header     Sets or clears a header for all requests. e.g. `set header content-type application/json`

Navigation Commands:
The REPL allows you to navigate your URL space and focus on specific APIs that you are working on.

set base       Set the base URI. e.g. `set base http://locahost:5000`
set swagger    Sets the swagger document to use for information about the current server
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

<span data-ttu-id="f731a-125">La boucle REPL HTTP offre la complétion des commandes.</span><span class="sxs-lookup"><span data-stu-id="f731a-125">The HTTP REPL offers command completion.</span></span> <span data-ttu-id="f731a-126">Un appui sur la touche <kbd>Tab</kbd> itère au sein de la liste des commandes qui complètent les caractères ou le point de terminaison d’API que vous avez tapés.</span><span class="sxs-lookup"><span data-stu-id="f731a-126">Pressing the <kbd>Tab</kbd> key iterates through the list of commands that complete the characters or API endpoint that you typed.</span></span> <span data-ttu-id="f731a-127">Les sections suivantes décrivent les commandes CLI disponibles.</span><span class="sxs-lookup"><span data-stu-id="f731a-127">The following sections outline the available CLI commands.</span></span>

## <a name="connect-to-the-web-api"></a><span data-ttu-id="f731a-128">Se connecter à l’API web</span><span class="sxs-lookup"><span data-stu-id="f731a-128">Connect to the web API</span></span>

<span data-ttu-id="f731a-129">Connectez-vous à une API web en exécutant la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="f731a-129">Connect to a web API by running the following command:</span></span>

```console
dotnet httprepl <BASE URI>
```

<span data-ttu-id="f731a-130">`<BASE URI>` est l’URI de base pour l’API web.</span><span class="sxs-lookup"><span data-stu-id="f731a-130">`<BASE URI>` is the base URI for the web API.</span></span> <span data-ttu-id="f731a-131">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f731a-131">For example:</span></span>

```console
dotnet httprepl https://localhost:5001
```

<span data-ttu-id="f731a-132">Vous pouvez également exécuter la commande suivante à tout moment pendant l’exécution de la boucle REPL HTTP :</span><span class="sxs-lookup"><span data-stu-id="f731a-132">Alternatively, run the following command at any time while the HTTP REPL is running:</span></span>

```console
set base <BASE URI>
```

<span data-ttu-id="f731a-133">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f731a-133">For example:</span></span>

```console
(Disconnected)~ set base https://localhost:5001
```

## <a name="point-to-the-swagger-document-for-the-web-api"></a><span data-ttu-id="f731a-134">Pointer sur le document Swagger pour l’API web</span><span class="sxs-lookup"><span data-stu-id="f731a-134">Point to the Swagger document for the web API</span></span>

<span data-ttu-id="f731a-135">Pour inspecter correctement l’API web, définissez l’URI relatif sur le document Swagger pour l’API web.</span><span class="sxs-lookup"><span data-stu-id="f731a-135">To properly inspect the web API, set the relative URI to the Swagger document for the web API.</span></span> <span data-ttu-id="f731a-136">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="f731a-136">Run the following command:</span></span>

```console
set swagger <RELATIVE URI>
```

<span data-ttu-id="f731a-137">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f731a-137">For example:</span></span>

```console
https://localhost:5001/~ set swagger /swagger/v1/swagger.json
```

## <a name="navigate-the-web-api"></a><span data-ttu-id="f731a-138">Accéder à l’API web</span><span class="sxs-lookup"><span data-stu-id="f731a-138">Navigate the web API</span></span>

### <a name="view-available-endpoints"></a><span data-ttu-id="f731a-139">Voir les points de terminaison disponibles</span><span class="sxs-lookup"><span data-stu-id="f731a-139">View available endpoints</span></span>

<span data-ttu-id="f731a-140">Pour lister les différents points de terminaison (contrôleurs) sur le chemin actuel de l’adresse de l’API web, exécutez la commande `ls` ou `dir` :</span><span class="sxs-lookup"><span data-stu-id="f731a-140">To list the different endpoints (controllers) at the current path of the web API address, run the `ls` or `dir` command:</span></span>

```console
https://localhot:5001/~ ls
```

<span data-ttu-id="f731a-141">Le format de sortie suivant s’affiche :</span><span class="sxs-lookup"><span data-stu-id="f731a-141">The following output format is displayed:</span></span>

```console
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="f731a-142">La sortie précédente indique que deux contrôleurs sont disponibles : `Fruits` et `People`.</span><span class="sxs-lookup"><span data-stu-id="f731a-142">The preceding output indicates that there are two controllers available: `Fruits` and `People`.</span></span> <span data-ttu-id="f731a-143">Les deux contrôleurs prennent en charge les opérations HTTP GET et POST sans paramètres.</span><span class="sxs-lookup"><span data-stu-id="f731a-143">Both controllers support parameterless HTTP GET and POST operations.</span></span>

<span data-ttu-id="f731a-144">La navigation dans un contrôleur spécifique révèle plus de détails.</span><span class="sxs-lookup"><span data-stu-id="f731a-144">Navigating into a specific controller reveals more detail.</span></span> <span data-ttu-id="f731a-145">Par exemple, la sortie de la commande suivante indique que le contrôleur `Fruits` prend également en charge les opérations HTTP GET, PUT et DELETE.</span><span class="sxs-lookup"><span data-stu-id="f731a-145">For example, the following command's output shows the `Fruits` controller also supports HTTP GET, PUT, and DELETE operations.</span></span> <span data-ttu-id="f731a-146">Chacune de ces opérations attend un paramètre `id` dans la route :</span><span class="sxs-lookup"><span data-stu-id="f731a-146">Each of these operations expects an `id` parameter in the route:</span></span>

```console
https://localhost:5001/fruits~ ls
.      [get|post]
..     []
{id}   [get|put|delete]

https://localhost:5001/fruits~
```

<span data-ttu-id="f731a-147">Vous pouvez également exécuter la commande `ui` pour ouvrir la page de l’interface utilisateur Swagger de l’API web dans un navigateur.</span><span class="sxs-lookup"><span data-stu-id="f731a-147">Alternatively, run the `ui` command to open the web API's Swagger UI page in a browser.</span></span> <span data-ttu-id="f731a-148">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f731a-148">For example:</span></span>

```console
https://localhost:5001/~ ui
```

### <a name="navigate-to-an-endpoint"></a><span data-ttu-id="f731a-149">Accéder à un point de terminaison</span><span class="sxs-lookup"><span data-stu-id="f731a-149">Navigate to an endpoint</span></span>

<span data-ttu-id="f731a-150">Pour accéder à un autre point de terminaison sur l’API web, exécutez la commande `cd` :</span><span class="sxs-lookup"><span data-stu-id="f731a-150">To navigate to a different endpoint on the web API, run the `cd` command:</span></span>

```console
https://localhost:5001/~ cd people
```

<span data-ttu-id="f731a-151">Le chemin qui suit la commande `cd` ne respecte pas la casse.</span><span class="sxs-lookup"><span data-stu-id="f731a-151">The path following the `cd` command is case insensitive.</span></span> <span data-ttu-id="f731a-152">Le format de sortie suivant s’affiche :</span><span class="sxs-lookup"><span data-stu-id="f731a-152">The following output format is displayed:</span></span>

```console
/people    [get|post]

https://localhost:5001/people~
```

## <a name="customize-the-http-repl"></a><span data-ttu-id="f731a-153">Personnaliser la boucle REPL HTTP</span><span class="sxs-lookup"><span data-stu-id="f731a-153">Customize the HTTP REPL</span></span>

<span data-ttu-id="f731a-154">Les [couleurs](#set-color-preferences) de la boucle REPL HTTP peuvent être personnalisées.</span><span class="sxs-lookup"><span data-stu-id="f731a-154">The HTTP REPL's default [colors](#set-color-preferences) can be customized.</span></span> <span data-ttu-id="f731a-155">Vous pouvez aussi définir un [éditeur de texte par défaut](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="f731a-155">Additionally, a [default text editor](#set-the-default-text-editor) can be defined.</span></span> <span data-ttu-id="f731a-156">Les préférences de la boucle REPL HTTP sont conservées dans la session active et dans les sessions ultérieures.</span><span class="sxs-lookup"><span data-stu-id="f731a-156">The HTTP REPL preferences are persisted across the current session and are honored in future sessions.</span></span> <span data-ttu-id="f731a-157">Une fois modifiées, les préférences sont stockées dans le fichier suivant :</span><span class="sxs-lookup"><span data-stu-id="f731a-157">Once modified, the preferences are stored in the following file:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="f731a-158">Linux</span><span class="sxs-lookup"><span data-stu-id="f731a-158">Linux</span></span>](#tab/linux)

<span data-ttu-id="f731a-159">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="f731a-159">*%HOME%/.httpreplprefs*</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="f731a-160">macOS</span><span class="sxs-lookup"><span data-stu-id="f731a-160">macOS</span></span>](#tab/macos)

<span data-ttu-id="f731a-161">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="f731a-161">*%HOME%/.httpreplprefs*</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="f731a-162">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="f731a-162">Windows</span></span>](#tab/windows)

<span data-ttu-id="f731a-163">*%USERPROFILE%\\.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="f731a-163">*%USERPROFILE%\\.httpreplprefs*</span></span>

---

<span data-ttu-id="f731a-164">Le fichier *.httpreplprefs* est chargé au démarrage et ses modifications ne sont pas surveillées pendant l’exécution.</span><span class="sxs-lookup"><span data-stu-id="f731a-164">The *.httpreplprefs* file is loaded on startup and not monitored for changes at runtime.</span></span> <span data-ttu-id="f731a-165">Les modifications manuelles apportées au fichier prennent effet seulement après un redémarrage de l’outil.</span><span class="sxs-lookup"><span data-stu-id="f731a-165">Manual modifications to the file take effect only after restarting the tool.</span></span>

### <a name="view-the-settings"></a><span data-ttu-id="f731a-166">Voir les paramètres</span><span class="sxs-lookup"><span data-stu-id="f731a-166">View the settings</span></span>

<span data-ttu-id="f731a-167">Pour voir les paramètres disponibles, exécutez la commande `pref get`.</span><span class="sxs-lookup"><span data-stu-id="f731a-167">To view the available settings, run the `pref get` command.</span></span> <span data-ttu-id="f731a-168">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f731a-168">For example:</span></span>

```console
https://localhost:5001/~ pref get
```

<span data-ttu-id="f731a-169">La commande précédente affiche les paires clé-valeur disponibles :</span><span class="sxs-lookup"><span data-stu-id="f731a-169">The preceding command displays the available key-value pairs:</span></span>

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

### <a name="set-color-preferences"></a><span data-ttu-id="f731a-170">Définir les préférences de couleur</span><span class="sxs-lookup"><span data-stu-id="f731a-170">Set color preferences</span></span>

<span data-ttu-id="f731a-171">La colorisation des réponses est actuellement prise en charge seulement pour JSON.</span><span class="sxs-lookup"><span data-stu-id="f731a-171">Response colorization is currently supported for JSON only.</span></span> <span data-ttu-id="f731a-172">Pour personnaliser les couleurs par défaut de l’outil REPL HTTP, localisez la clé correspondant à la couleur à changer.</span><span class="sxs-lookup"><span data-stu-id="f731a-172">To customize the default HTTP REPL tool coloring, locate the key corresponding to the color to be changed.</span></span> <span data-ttu-id="f731a-173">Pour des instructions sur la façon de trouver les clés, consultez la section [Voir les paramètres](#view-the-settings).</span><span class="sxs-lookup"><span data-stu-id="f731a-173">For instructions on how to find the keys, see the [View the settings](#view-the-settings) section.</span></span> <span data-ttu-id="f731a-174">Par exemple, remplacez la valeur de la clé `colors.json` de `Green` par `White` :</span><span class="sxs-lookup"><span data-stu-id="f731a-174">For example, change the `colors.json` key value from `Green` to `White` as follows:</span></span>

```console
https://localhost:5001/people~ pref set colors.json White
```

<span data-ttu-id="f731a-175">Seules les [couleurs autorisées](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) peuvent être utilisées.</span><span class="sxs-lookup"><span data-stu-id="f731a-175">Only the [allowed colors](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) may be used.</span></span> <span data-ttu-id="f731a-176">Les requêtes HTTP suivantes affichent la sortie avec les nouvelles couleurs.</span><span class="sxs-lookup"><span data-stu-id="f731a-176">Subsequent HTTP requests display output with the new coloring.</span></span>

<span data-ttu-id="f731a-177">Quand des clés d’une couleur spécifique ne sont pas définies, des clés plus génériques sont prises en compte.</span><span class="sxs-lookup"><span data-stu-id="f731a-177">When specific color keys aren't set, more generic keys are considered.</span></span> <span data-ttu-id="f731a-178">Pour illustrer ce comportement de repli, considérez l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="f731a-178">To demonstrate this fallback behavior, consider the following example:</span></span>

* <span data-ttu-id="f731a-179">Si `colors.json.name` n’a pas de valeur, `colors.json.string` est utilisée.</span><span class="sxs-lookup"><span data-stu-id="f731a-179">If `colors.json.name` doesn't have a value, `colors.json.string` is used.</span></span>
* <span data-ttu-id="f731a-180">Si `colors.json.string` n’a pas de valeur, `colors.json.literal` est utilisée.</span><span class="sxs-lookup"><span data-stu-id="f731a-180">If `colors.json.string` doesn't have a value, `colors.json.literal` is used.</span></span>
* <span data-ttu-id="f731a-181">Si `colors.json.literal` n’a pas de valeur, `colors.json` est utilisée.</span><span class="sxs-lookup"><span data-stu-id="f731a-181">If `colors.json.literal` doesn't have a value, `colors.json` is used.</span></span> 
* <span data-ttu-id="f731a-182">Si `colors.json` n’a pas de valeur, la couleur de texte par défaut de l’interpréteur de commandes (`AllowedColors.None`) est utilisée.</span><span class="sxs-lookup"><span data-stu-id="f731a-182">If `colors.json` doesn't have a value, the command shell's default text color (`AllowedColors.None`) is used.</span></span>

### <a name="set-indentation-size"></a><span data-ttu-id="f731a-183">Définir la taille de la mise en retrait</span><span class="sxs-lookup"><span data-stu-id="f731a-183">Set indentation size</span></span>

<span data-ttu-id="f731a-184">La personnalisation de la taille de la mise en retrait de la réponse est actuellement prise en charge pour JSON uniquement.</span><span class="sxs-lookup"><span data-stu-id="f731a-184">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="f731a-185">La taille par défaut est de deux espaces.</span><span class="sxs-lookup"><span data-stu-id="f731a-185">The default size is two spaces.</span></span> <span data-ttu-id="f731a-186">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f731a-186">For example:</span></span>

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

<span data-ttu-id="f731a-187">Pour changer la taille par défaut, définissez la clé `formatting.json.indentSize`.</span><span class="sxs-lookup"><span data-stu-id="f731a-187">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="f731a-188">Par exemple, pour toujours utiliser quatre espaces :</span><span class="sxs-lookup"><span data-stu-id="f731a-188">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="f731a-189">Les réponses suivantes adoptent la valeur correspondant à quatre espaces :</span><span class="sxs-lookup"><span data-stu-id="f731a-189">Subsequent responses honor the setting of four spaces:</span></span>

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

### <a name="set-indentation-size"></a><span data-ttu-id="f731a-190">Définir la taille de la mise en retrait</span><span class="sxs-lookup"><span data-stu-id="f731a-190">Set indentation size</span></span>

<span data-ttu-id="f731a-191">La personnalisation de la taille de la mise en retrait de la réponse est actuellement prise en charge pour JSON uniquement.</span><span class="sxs-lookup"><span data-stu-id="f731a-191">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="f731a-192">La taille par défaut est de deux espaces.</span><span class="sxs-lookup"><span data-stu-id="f731a-192">The default size is two spaces.</span></span> <span data-ttu-id="f731a-193">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f731a-193">For example:</span></span>

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

<span data-ttu-id="f731a-194">Pour changer la taille par défaut, définissez la clé `formatting.json.indentSize`.</span><span class="sxs-lookup"><span data-stu-id="f731a-194">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="f731a-195">Par exemple, pour toujours utiliser quatre espaces :</span><span class="sxs-lookup"><span data-stu-id="f731a-195">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="f731a-196">Les réponses suivantes adoptent la valeur correspondant à quatre espaces :</span><span class="sxs-lookup"><span data-stu-id="f731a-196">Subsequent responses honor the setting of four spaces:</span></span>

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

### <a name="set-the-default-text-editor"></a><span data-ttu-id="f731a-197">Définir l’éditeur de texte par défaut</span><span class="sxs-lookup"><span data-stu-id="f731a-197">Set the default text editor</span></span>

<span data-ttu-id="f731a-198">Par défaut, la boucle REPL HTTP n’a pas d’éditeur de texte configuré pour l’utilisation.</span><span class="sxs-lookup"><span data-stu-id="f731a-198">By default, the HTTP REPL has no text editor configured for use.</span></span> <span data-ttu-id="f731a-199">Pour tester les méthodes d’API web nécessitant un corps de requête HTTP, vous devez définir un éditeur de texte par défaut.</span><span class="sxs-lookup"><span data-stu-id="f731a-199">To test web API methods requiring an HTTP request body, a default text editor must be set.</span></span> <span data-ttu-id="f731a-200">L’outil REPL HTTP lance l’éditeur de texte configuré dans le seul but de permettre la composition du corps de la requête.</span><span class="sxs-lookup"><span data-stu-id="f731a-200">The HTTP REPL tool launches the configured text editor for the sole purpose of composing the request body.</span></span> <span data-ttu-id="f731a-201">Exécutez la commande suivante pour définir votre éditeur de texte préféré comme éditeur par défaut :</span><span class="sxs-lookup"><span data-stu-id="f731a-201">Run the following command to set your preferred text editor as the default:</span></span>

```console
pref set editor.command.default "<EXECUTABLE>"
```

<span data-ttu-id="f731a-202">Dans la commande précédente, `<EXECUTABLE>` est le chemin complet du fichier exécutable de l’éditeur de texte.</span><span class="sxs-lookup"><span data-stu-id="f731a-202">In the preceding command, `<EXECUTABLE>` is the full path to the text editor's executable file.</span></span> <span data-ttu-id="f731a-203">Par exemple, exécutez la commande suivante pour définir Visual Studio Code comme éditeur de texte par défaut :</span><span class="sxs-lookup"><span data-stu-id="f731a-203">For example, run the following command to set Visual Studio Code as the default text editor:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="f731a-204">Linux</span><span class="sxs-lookup"><span data-stu-id="f731a-204">Linux</span></span>](#tab/linux)

```console
pref set editor.command.default "/usr/bin/code"
```

# <a name="macostabmacos"></a>[<span data-ttu-id="f731a-205">macOS</span><span class="sxs-lookup"><span data-stu-id="f731a-205">macOS</span></span>](#tab/macos)

```console
pref set editor.command.default "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code"
```

# <a name="windowstabwindows"></a>[<span data-ttu-id="f731a-206">Fenêtres</span><span class="sxs-lookup"><span data-stu-id="f731a-206">Windows</span></span>](#tab/windows)

```console
pref set editor.command.default "C:\Program Files\Microsoft VS Code\Code.exe"
```

---

<span data-ttu-id="f731a-207">Pour lancer l’éditeur de texte par défaut avec des arguments CLI spécifiques, définissez la clé `editor.command.default.arguments`.</span><span class="sxs-lookup"><span data-stu-id="f731a-207">To launch the default text editor with specific CLI arguments, set the `editor.command.default.arguments` key.</span></span> <span data-ttu-id="f731a-208">Par exemple, supposons que Visual Studio Code est l’éditeur de texte par défaut et que vous voulez que la boucle REPL HTTP ouvre toujours Visual Studio Code dans une nouvelle session avec les extensions désactivées.</span><span class="sxs-lookup"><span data-stu-id="f731a-208">For example, assume Visual Studio Code is the default text editor and that you always want the HTTP REPL to open Visual Studio Code in a new session with extensions disabled.</span></span> <span data-ttu-id="f731a-209">Exécutez la commande suivante :</span><span class="sxs-lookup"><span data-stu-id="f731a-209">Run the following command:</span></span>

```console
pref set editor.command.default.arguments "--disable-extensions --new-window"
```

## <a name="test-http-get-requests"></a><span data-ttu-id="f731a-210">Tester des requêtes HTTP GET</span><span class="sxs-lookup"><span data-stu-id="f731a-210">Test HTTP GET requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="f731a-211">Résumé</span><span class="sxs-lookup"><span data-stu-id="f731a-211">Synopsis</span></span>

```console
get <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="f731a-212">Arguments</span><span class="sxs-lookup"><span data-stu-id="f731a-212">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="f731a-213">Paramètre de route, le cas échéant, attendu par la méthode d’action du contrôleur associé.</span><span class="sxs-lookup"><span data-stu-id="f731a-213">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="f731a-214">Options</span><span class="sxs-lookup"><span data-stu-id="f731a-214">Options</span></span>

<span data-ttu-id="f731a-215">Les options suivantes sont disponibles pour la commande `get` :</span><span class="sxs-lookup"><span data-stu-id="f731a-215">The following options are available for the `get` command:</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="f731a-216">Exemples</span><span class="sxs-lookup"><span data-stu-id="f731a-216">Example</span></span>

<span data-ttu-id="f731a-217">Pour émettre une requête HTTP GET :</span><span class="sxs-lookup"><span data-stu-id="f731a-217">To issue an HTTP GET request:</span></span>

1. <span data-ttu-id="f731a-218">Exécutez la commande `get` sur un point de terminaison qui la prend en charge :</span><span class="sxs-lookup"><span data-stu-id="f731a-218">Run the `get` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ get
    ```

    <span data-ttu-id="f731a-219">La commande précédente affiche le format de sortie suivant :</span><span class="sxs-lookup"><span data-stu-id="f731a-219">The preceding command displays the following output format:</span></span>

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

1. <span data-ttu-id="f731a-220">Récupérez un enregistrement spécifique en passant un paramètre à la commande `get` :</span><span class="sxs-lookup"><span data-stu-id="f731a-220">Retrieve a specific record by passing a parameter to the `get` command:</span></span>

    ```console
    https://localhost:5001/people~ get 2
    ```

    <span data-ttu-id="f731a-221">La commande précédente affiche le format de sortie suivant :</span><span class="sxs-lookup"><span data-stu-id="f731a-221">The preceding command displays the following output format:</span></span>

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

## <a name="test-http-post-requests"></a><span data-ttu-id="f731a-222">Tester des requêtes HTTP POST</span><span class="sxs-lookup"><span data-stu-id="f731a-222">Test HTTP POST requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="f731a-223">Résumé</span><span class="sxs-lookup"><span data-stu-id="f731a-223">Synopsis</span></span>

```console
post <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="f731a-224">Arguments</span><span class="sxs-lookup"><span data-stu-id="f731a-224">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="f731a-225">Paramètre de route, le cas échéant, attendu par la méthode d’action du contrôleur associé.</span><span class="sxs-lookup"><span data-stu-id="f731a-225">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="f731a-226">Options</span><span class="sxs-lookup"><span data-stu-id="f731a-226">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="f731a-227">Exemples</span><span class="sxs-lookup"><span data-stu-id="f731a-227">Example</span></span>

<span data-ttu-id="f731a-228">Pour émettre une requête HTTP POST :</span><span class="sxs-lookup"><span data-stu-id="f731a-228">To issue an HTTP POST request:</span></span>

1. <span data-ttu-id="f731a-229">Exécutez la commande `post` sur un point de terminaison qui la prend en charge :</span><span class="sxs-lookup"><span data-stu-id="f731a-229">Run the `post` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ post -h Content-Type=application/json
    ```

    <span data-ttu-id="f731a-230">Dans la commande précédente, l’en-tête `Content-Type` de la requête HTTP est défini pour indiquer un type de média de corps de requête JSON.</span><span class="sxs-lookup"><span data-stu-id="f731a-230">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="f731a-231">L’éditeur de texte par défaut ouvre un fichier *.tmp* avec un modèle JSON représentant le corps de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="f731a-231">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="f731a-232">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f731a-232">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="f731a-233">Pour définir l’éditeur de texte par défaut, consultez la section [Définir l’éditeur de texte par défaut](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="f731a-233">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="f731a-234">Modifiez le modèle JSON pour répondre aux exigences de validation du modèle :</span><span class="sxs-lookup"><span data-stu-id="f731a-234">Modify the JSON template to satisfy model validation requirements:</span></span>

  ```json
  {
    "id": 0,
    "name": "Scott Addie"
  }
  ```

1. <span data-ttu-id="f731a-235">Enregistrez le fichier *.tmp* et fermez l’éditeur de texte.</span><span class="sxs-lookup"><span data-stu-id="f731a-235">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="f731a-236">La sortie suivante s’affiche dans l’interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="f731a-236">The following output appears in the command shell:</span></span>

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

## <a name="test-http-put-requests"></a><span data-ttu-id="f731a-237">Tester des requêtes HTTP PUT</span><span class="sxs-lookup"><span data-stu-id="f731a-237">Test HTTP PUT requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="f731a-238">Résumé</span><span class="sxs-lookup"><span data-stu-id="f731a-238">Synopsis</span></span>

```console
put <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="f731a-239">Arguments</span><span class="sxs-lookup"><span data-stu-id="f731a-239">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="f731a-240">Paramètre de route, le cas échéant, attendu par la méthode d’action du contrôleur associé.</span><span class="sxs-lookup"><span data-stu-id="f731a-240">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="f731a-241">Options</span><span class="sxs-lookup"><span data-stu-id="f731a-241">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="f731a-242">Exemples</span><span class="sxs-lookup"><span data-stu-id="f731a-242">Example</span></span>

<span data-ttu-id="f731a-243">Pour émettre une requête HTTP PUT :</span><span class="sxs-lookup"><span data-stu-id="f731a-243">To issue an HTTP PUT request:</span></span>

1. <span data-ttu-id="f731a-244">*Facultatif* : Exécutez la commande `get` pour afficher les données avant de les modifier :</span><span class="sxs-lookup"><span data-stu-id="f731a-244">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="f731a-245">Dans la commande précédente, l’en-tête `Content-Type` de la requête HTTP est défini pour indiquer un type de média de corps de requête JSON.</span><span class="sxs-lookup"><span data-stu-id="f731a-245">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="f731a-246">L’éditeur de texte par défaut ouvre un fichier *.tmp* avec un modèle JSON représentant le corps de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="f731a-246">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="f731a-247">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f731a-247">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="f731a-248">Pour définir l’éditeur de texte par défaut, consultez la section [Définir l’éditeur de texte par défaut](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="f731a-248">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="f731a-249">Modifiez le modèle JSON pour répondre aux exigences de validation du modèle :</span><span class="sxs-lookup"><span data-stu-id="f731a-249">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 2,
      "name": "Cherry"
    }
    ```

1. <span data-ttu-id="f731a-250">Enregistrez le fichier *.tmp* et fermez l’éditeur de texte.</span><span class="sxs-lookup"><span data-stu-id="f731a-250">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="f731a-251">La sortie suivante s’affiche dans l’interpréteur de commandes :</span><span class="sxs-lookup"><span data-stu-id="f731a-251">The following output appears in the command shell:</span></span>

    ```console
    [main 2019-06-28T17:27:01.805Z] update#setState idle
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:28:21 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="f731a-252">*Facultatif* : Émettez une commande `get` pour voir les modifications.</span><span class="sxs-lookup"><span data-stu-id="f731a-252">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="f731a-253">Par exemple, si vous avez tapé « Cherry » dans l’éditeur de texte, une commande `get` retourne ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="f731a-253">For example, if you typed "Cherry" in the text editor, a `get` returns the following:</span></span>

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

## <a name="test-http-delete-requests"></a><span data-ttu-id="f731a-254">Tester des requêtes HTTP DELETE</span><span class="sxs-lookup"><span data-stu-id="f731a-254">Test HTTP DELETE requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="f731a-255">Résumé</span><span class="sxs-lookup"><span data-stu-id="f731a-255">Synopsis</span></span>

```console
delete <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="f731a-256">Arguments</span><span class="sxs-lookup"><span data-stu-id="f731a-256">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="f731a-257">Paramètre de route, le cas échéant, attendu par la méthode d’action du contrôleur associé.</span><span class="sxs-lookup"><span data-stu-id="f731a-257">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="f731a-258">Options</span><span class="sxs-lookup"><span data-stu-id="f731a-258">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="f731a-259">Exemples</span><span class="sxs-lookup"><span data-stu-id="f731a-259">Example</span></span>

<span data-ttu-id="f731a-260">Pour émettre une requête HTTP DELETE :</span><span class="sxs-lookup"><span data-stu-id="f731a-260">To issue an HTTP DELETE request:</span></span>

1. <span data-ttu-id="f731a-261">*Facultatif* : Exécutez la commande `get` pour voir les données avant de les modifier :</span><span class="sxs-lookup"><span data-stu-id="f731a-261">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="f731a-262">La commande précédente affiche le format de sortie suivant :</span><span class="sxs-lookup"><span data-stu-id="f731a-262">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:36:42 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="f731a-263">*Facultatif* : Émettez une commande `get` pour voir les modifications.</span><span class="sxs-lookup"><span data-stu-id="f731a-263">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="f731a-264">Dans cet exemple, une commande `get` retourne ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="f731a-264">In this example, a `get` returns the following:</span></span>

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

## <a name="test-http-patch-requests"></a><span data-ttu-id="f731a-265">Tester des requêtes HTTP PATCH</span><span class="sxs-lookup"><span data-stu-id="f731a-265">Test HTTP PATCH requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="f731a-266">Résumé</span><span class="sxs-lookup"><span data-stu-id="f731a-266">Synopsis</span></span>

```console
patch <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="f731a-267">Arguments</span><span class="sxs-lookup"><span data-stu-id="f731a-267">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="f731a-268">Paramètre de route, le cas échéant, attendu par la méthode d’action du contrôleur associé.</span><span class="sxs-lookup"><span data-stu-id="f731a-268">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="f731a-269">Options</span><span class="sxs-lookup"><span data-stu-id="f731a-269">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

## <a name="test-http-head-requests"></a><span data-ttu-id="f731a-270">Tester des requêtes HTTP HEAD</span><span class="sxs-lookup"><span data-stu-id="f731a-270">Test HTTP HEAD requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="f731a-271">Résumé</span><span class="sxs-lookup"><span data-stu-id="f731a-271">Synopsis</span></span>

```console
head <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="f731a-272">Arguments</span><span class="sxs-lookup"><span data-stu-id="f731a-272">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="f731a-273">Paramètre de route, le cas échéant, attendu par la méthode d’action du contrôleur associé.</span><span class="sxs-lookup"><span data-stu-id="f731a-273">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="f731a-274">Options</span><span class="sxs-lookup"><span data-stu-id="f731a-274">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="test-http-options-requests"></a><span data-ttu-id="f731a-275">Tester des requêtes HTTP OPTIONS</span><span class="sxs-lookup"><span data-stu-id="f731a-275">Test HTTP OPTIONS requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="f731a-276">Résumé</span><span class="sxs-lookup"><span data-stu-id="f731a-276">Synopsis</span></span>

```console
options <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="f731a-277">Arguments</span><span class="sxs-lookup"><span data-stu-id="f731a-277">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="f731a-278">Paramètre de route, le cas échéant, attendu par la méthode d’action du contrôleur associé.</span><span class="sxs-lookup"><span data-stu-id="f731a-278">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="f731a-279">Options</span><span class="sxs-lookup"><span data-stu-id="f731a-279">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="set-http-request-headers"></a><span data-ttu-id="f731a-280">Définir des en-têtes de requête HTTP</span><span class="sxs-lookup"><span data-stu-id="f731a-280">Set HTTP request headers</span></span>

<span data-ttu-id="f731a-281">Pour définir un en-tête de requête HTTP, utilisez une des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="f731a-281">To set an HTTP request header, use one of the following approaches:</span></span>

1. <span data-ttu-id="f731a-282">Définir inline avec la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="f731a-282">Set inline with the HTTP request.</span></span> <span data-ttu-id="f731a-283">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f731a-283">For example:</span></span>

  ```console
  https://localhost:5001/people~ post -h Content-Type=application/json
  ```

  <span data-ttu-id="f731a-284">Avec l’approche précédente, chaque en-tête de requête HTTP distinct nécessite sa propre option `-h`.</span><span class="sxs-lookup"><span data-stu-id="f731a-284">With the preceding approach, each distinct HTTP request header requires its own `-h` option.</span></span>

1. <span data-ttu-id="f731a-285">Définir avant l’envoi de la requête HTTP.</span><span class="sxs-lookup"><span data-stu-id="f731a-285">Set before sending the HTTP request.</span></span> <span data-ttu-id="f731a-286">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f731a-286">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type application/json
  ```

  <span data-ttu-id="f731a-287">Si l’en-tête est défini avant l’envoi d’une requête, l’en-tête reste défini pour la durée de la session de l’interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="f731a-287">When setting the header before sending a request, the header remains set for the duration of the command shell session.</span></span> <span data-ttu-id="f731a-288">Pour effacer l’en-tête, spécifiez une valeur vide.</span><span class="sxs-lookup"><span data-stu-id="f731a-288">To clear the header, provide an empty value.</span></span> <span data-ttu-id="f731a-289">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f731a-289">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type
  ```

## <a name="toggle-http-request-display"></a><span data-ttu-id="f731a-290">Activer/désactiver l’affichage des requêtes HTTP</span><span class="sxs-lookup"><span data-stu-id="f731a-290">Toggle HTTP request display</span></span>

<span data-ttu-id="f731a-291">Par défaut, l’affichage de la requête HTTP envoyée est supprimé.</span><span class="sxs-lookup"><span data-stu-id="f731a-291">By default, display of the HTTP request being sent is suppressed.</span></span> <span data-ttu-id="f731a-292">Il est possible de changer le paramètre correspondant pour la durée de la session de l’interpréteur de commandes.</span><span class="sxs-lookup"><span data-stu-id="f731a-292">It's possible to change the corresponding setting for the duration of the command shell session.</span></span>

### <a name="enable-request-display"></a><span data-ttu-id="f731a-293">Activer l’affichage des requêtes</span><span class="sxs-lookup"><span data-stu-id="f731a-293">Enable request display</span></span>

<span data-ttu-id="f731a-294">Affichez la requête HTTP envoyée en exécutant la commande `echo on`.</span><span class="sxs-lookup"><span data-stu-id="f731a-294">View the HTTP request being sent by running the `echo on` command.</span></span> <span data-ttu-id="f731a-295">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f731a-295">For example:</span></span>

```console
https://localhost:5001/people~ echo on
Request echoing is on
```

<span data-ttu-id="f731a-296">Les requêtes HTTP suivantes dans la session active affichent les en-têtes de requête.</span><span class="sxs-lookup"><span data-stu-id="f731a-296">Subsequent HTTP requests in the current session display the request headers.</span></span> <span data-ttu-id="f731a-297">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f731a-297">For example:</span></span>

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

### <a name="disable-request-display"></a><span data-ttu-id="f731a-298">Désactiver l’affichage des requêtes</span><span class="sxs-lookup"><span data-stu-id="f731a-298">Disable request display</span></span>

<span data-ttu-id="f731a-299">Supprimez l’affichage de la requête HTTP envoyée en exécutant la commande `echo off`.</span><span class="sxs-lookup"><span data-stu-id="f731a-299">Suppress display of the HTTP request being sent by running the `echo off` command.</span></span> <span data-ttu-id="f731a-300">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f731a-300">For example:</span></span>

```console
https://localhost:5001/people~ echo off
Request echoing is off
```

## <a name="run-a-script"></a><span data-ttu-id="f731a-301">Exécuter un script</span><span class="sxs-lookup"><span data-stu-id="f731a-301">Run a script</span></span>

<span data-ttu-id="f731a-302">Si vous exécutez fréquemment le même jeu de commandes REPL HTTP, envisagez de les stocker dans un fichier texte.</span><span class="sxs-lookup"><span data-stu-id="f731a-302">If you frequently execute the same set of HTTP REPL commands, consider storing them in a text file.</span></span> <span data-ttu-id="f731a-303">Les commandes placées dans le fichier sont de la même forme que celles exécutées manuellement sur la ligne de commande.</span><span class="sxs-lookup"><span data-stu-id="f731a-303">Commands in the file take the same form as those executed manually on the command line.</span></span> <span data-ttu-id="f731a-304">Les commandes peuvent être exécutées de façon groupée avec la commande `run`.</span><span class="sxs-lookup"><span data-stu-id="f731a-304">The commands can be executed in a batched fashion using the `run` command.</span></span> <span data-ttu-id="f731a-305">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f731a-305">For example:</span></span>

1. <span data-ttu-id="f731a-306">Créez un fichier texte contenant un ensemble de commandes délimitées par des sauts de ligne.</span><span class="sxs-lookup"><span data-stu-id="f731a-306">Create a text file containing a set of newline-delimited commands.</span></span> <span data-ttu-id="f731a-307">Pour illustrer ceci, considérez un fichier *people-script.txt* contenant les commandes suivantes :</span><span class="sxs-lookup"><span data-stu-id="f731a-307">To illustrate, consider a *people-script.txt* file containing the following commands:</span></span>

    ```text
    set base https://localhost:5001
    ls
    cd People
    ls
    get 1
    ```

1. <span data-ttu-id="f731a-308">Exécutez la commande `run`, en passant le chemin du fichier texte.</span><span class="sxs-lookup"><span data-stu-id="f731a-308">Execute the `run` command, passing in the text file's path.</span></span> <span data-ttu-id="f731a-309">Par exemple :</span><span class="sxs-lookup"><span data-stu-id="f731a-309">For example:</span></span>

    ```console
    https://localhost:5001/~ run C:\http-repl-scripts\people-script.txt
    ```

    <span data-ttu-id="f731a-310">La sortie suivante apparaît :</span><span class="sxs-lookup"><span data-stu-id="f731a-310">The following output appears:</span></span>

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

## <a name="clear-the-output"></a><span data-ttu-id="f731a-311">Effacer la sortie</span><span class="sxs-lookup"><span data-stu-id="f731a-311">Clear the output</span></span>

<span data-ttu-id="f731a-312">Pour supprimer toutes les sorties écrites dans l’interpréteur de commandes par l’outil REPL HTTP, exécutez la commande `clear` ou `cls`.</span><span class="sxs-lookup"><span data-stu-id="f731a-312">To remove all output written to the command shell by the HTTP REPL tool, run the `clear` or `cls` command.</span></span> <span data-ttu-id="f731a-313">À titre d’illustration, imaginez que l’interpréteur de commandes contient la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="f731a-313">To illustrate, imagine the command shell contains the following output:</span></span>

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

<span data-ttu-id="f731a-314">Exécutez la commande suivante pour effacer la sortie :</span><span class="sxs-lookup"><span data-stu-id="f731a-314">Run the following command to clear the output:</span></span>

```console
https://localhost:5001/~ clear
```

<span data-ttu-id="f731a-315">Après l’exécution de la commande précédente, l’interpréteur de commandes contient seulement la sortie suivante :</span><span class="sxs-lookup"><span data-stu-id="f731a-315">After running the preceding command, the command shell contains only the following output:</span></span>

```console
https://localhost:5001/~
```

## <a name="additional-resources"></a><span data-ttu-id="f731a-316">Ressources supplémentaires</span><span class="sxs-lookup"><span data-stu-id="f731a-316">Additional resources</span></span>

* [<span data-ttu-id="f731a-317">Requêtes de l’API REST</span><span class="sxs-lookup"><span data-stu-id="f731a-317">REST API requests</span></span>](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)
* [<span data-ttu-id="f731a-318">Dépôt GitHub REPL HTTP</span><span class="sxs-lookup"><span data-stu-id="f731a-318">HTTP REPL GitHub repository</span></span>](https://github.com/aspnet/HttpRepl)
