---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: Des Pages Web ASP.NET (Razor) Guide de référence rapide de l’API | Documents Microsoft
author: tfitzmac
description: Cette page contient une liste avec des exemples brièvement les objets couramment utilisés, les propriétés et les méthodes de programmation ASP.NET Web Pages avec syntaxe Razor.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: 5f9d84f4d453583d7d4eae12e4fc510275255616
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897582"
---
<a name="aspnet-web-pages-razor-api-quick-reference"></a><span data-ttu-id="40095-103">Référence des API rapide (Razor) des Pages Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="40095-103">ASP.NET Web Pages (Razor) API Quick Reference</span></span>
====================
<span data-ttu-id="40095-104">par [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="40095-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="40095-105">Cette page contient une liste avec des exemples brièvement les objets couramment utilisés, les propriétés et les méthodes de programmation ASP.NET Web Pages avec syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="40095-105">This page contains a list with brief examples of the most commonly used objects, properties, and methods for programming ASP.NET Web Pages with Razor syntax.</span></span>
> 
> <span data-ttu-id="40095-106">Descriptions marquées avec « (v2) » ont été introduites dans les Pages Web ASP.NET version 2.</span><span class="sxs-lookup"><span data-stu-id="40095-106">Descriptions marked with "(v2)" were introduced in ASP.NET Web Pages version 2.</span></span>
> 
> <span data-ttu-id="40095-107">Pour la documentation de référence d’API, consultez le [documentation de référence sur les Pages Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=208659) sur MSDN.</span><span class="sxs-lookup"><span data-stu-id="40095-107">For API reference documentation, see the [ASP.NET Web Pages Reference Documentation](https://go.microsoft.com/fwlink/?LinkId=208659) on MSDN.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="40095-108">Versions du logiciel</span><span class="sxs-lookup"><span data-stu-id="40095-108">Software versions</span></span>
> 
> 
> - <span data-ttu-id="40095-109">Pages Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="40095-109">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="40095-110">Ce didacticiel fonctionne également avec ASP.NET Web Pages 2 et la version 1.0 de ASP.NET Web Pages (à l’exception des fonctions marquées v2).</span><span class="sxs-lookup"><span data-stu-id="40095-110">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0 (except for features marked v2).</span></span>


<span data-ttu-id="40095-111">Cette page contient des informations de référence pour les éléments suivants :</span><span class="sxs-lookup"><span data-stu-id="40095-111">This page contains reference information for the following:</span></span>

- [<span data-ttu-id="40095-112">Classes</span><span class="sxs-lookup"><span data-stu-id="40095-112">Classes</span></span>](#Classes)
- [<span data-ttu-id="40095-113">Données</span><span class="sxs-lookup"><span data-stu-id="40095-113">Data</span></span>](#Data)
- [<span data-ttu-id="40095-114">Programmes d’assistance</span><span class="sxs-lookup"><span data-stu-id="40095-114">Helpers</span></span>](#Helpers)
- [<span data-ttu-id="40095-115">Validation</span><span class="sxs-lookup"><span data-stu-id="40095-115">Validation</span></span>](#Validation)

<a id="Classes"></a>
## <a name="classes"></a><span data-ttu-id="40095-116">Classes</span><span class="sxs-lookup"><span data-stu-id="40095-116">Classes</span></span>

### `AppState[key], AppState[index],App`

<span data-ttu-id="40095-117">Contient des données qui peuvent être partagées par toutes les pages dans l’application.</span><span class="sxs-lookup"><span data-stu-id="40095-117">Contains data that can be shared by any pages in the application.</span></span> <span data-ttu-id="40095-118">Vous pouvez utiliser la dynamique `App` propriété pour accéder aux mêmes données, comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="40095-118">You can use the dynamic `App` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

<span data-ttu-id="40095-119">Convertit une valeur de chaîne en valeur booléenne (true/false).</span><span class="sxs-lookup"><span data-stu-id="40095-119">Converts a string value to a Boolean value (true/false).</span></span> <span data-ttu-id="40095-120">Retourne la valeur false ou la valeur spécifiée si la chaîne ne représente pas la valeur true/false.</span><span class="sxs-lookup"><span data-stu-id="40095-120">Returns false or the specified value if the string does not represent true/false.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

<span data-ttu-id="40095-121">Convertit une valeur de chaîne en date/heure.</span><span class="sxs-lookup"><span data-stu-id="40095-121">Converts a string value to date/time.</span></span> <span data-ttu-id="40095-122">Retourne `DateTime.MinValue` ou la valeur spécifiée si la chaîne ne représente pas une date/heure.</span><span class="sxs-lookup"><span data-stu-id="40095-122">Returns `DateTime.MinValue` or the specified value if the string does not represent a date/time.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

<span data-ttu-id="40095-123">Convertit une valeur de chaîne en une valeur décimale.</span><span class="sxs-lookup"><span data-stu-id="40095-123">Converts a string value to a decimal value.</span></span> <span data-ttu-id="40095-124">Retourne 0.0 ou la valeur spécifiée si la chaîne ne représente pas une valeur décimale.</span><span class="sxs-lookup"><span data-stu-id="40095-124">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

<span data-ttu-id="40095-125">Convertit une valeur de chaîne à virgule flottante.</span><span class="sxs-lookup"><span data-stu-id="40095-125">Converts a string value to a float.</span></span> <span data-ttu-id="40095-126">Retourne 0.0 ou la valeur spécifiée si la chaîne ne représente pas une valeur décimale.</span><span class="sxs-lookup"><span data-stu-id="40095-126">Returns 0.0 or the specified value if the string does not represent a decimal value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

<span data-ttu-id="40095-127">Convertit une valeur de chaîne en entier.</span><span class="sxs-lookup"><span data-stu-id="40095-127">Converts a string value to an integer.</span></span> <span data-ttu-id="40095-128">Renvoie la valeur 0 ou la valeur spécifiée si la chaîne ne représente pas un entier.</span><span class="sxs-lookup"><span data-stu-id="40095-128">Returns 0 or the specified value if the string does not represent an integer.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

<span data-ttu-id="40095-129">Crée une URL de navigateur à partir d’un chemin d’accès local, avec des parties du chemin d’accès supplémentaire facultatif.</span><span class="sxs-lookup"><span data-stu-id="40095-129">Creates a browser-compatible URL from a local file path, with optional additional path parts.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

<span data-ttu-id="40095-130">Restitue *valeur* en tant que balisage HTML au lieu que codée en HTML la sortie de rendu.</span><span class="sxs-lookup"><span data-stu-id="40095-130">Renders *value* as HTML markup instead of rendering it as HTML-encoded output.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

<span data-ttu-id="40095-131">Retourne la valeur true si la valeur peut être convertie à partir d’une chaîne vers le type spécifié.</span><span class="sxs-lookup"><span data-stu-id="40095-131">Returns true if the value can be converted from a string to the specified type.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

<span data-ttu-id="40095-132">Retourne la valeur true si l’objet ou la variable n’a aucune valeur.</span><span class="sxs-lookup"><span data-stu-id="40095-132">Returns true if the object or variable has no value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

<span data-ttu-id="40095-133">Retourne la valeur true si la demande est une demande POST.</span><span class="sxs-lookup"><span data-stu-id="40095-133">Returns true if the request is a POST.</span></span> <span data-ttu-id="40095-134">(Demandes initiales sont généralement une opération GET).</span><span class="sxs-lookup"><span data-stu-id="40095-134">(Initial requests are usually a GET.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

<span data-ttu-id="40095-135">Spécifie le chemin d’accès d’une page de disposition à appliquer à cette page.</span><span class="sxs-lookup"><span data-stu-id="40095-135">Specifies the path of a layout page to apply to this page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

<span data-ttu-id="40095-136">Contient des données partagées entre la page, les pages de disposition et les pages partielles dans la requête actuelle.</span><span class="sxs-lookup"><span data-stu-id="40095-136">Contains data shared between the page, layout pages, and partial pages in the current request.</span></span> <span data-ttu-id="40095-137">Vous pouvez utiliser la dynamique `Page` propriété pour accéder aux mêmes données, comme dans l’exemple suivant :</span><span class="sxs-lookup"><span data-stu-id="40095-137">You can use the dynamic `Page` property to access the same data, as in the following example:</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

<span data-ttu-id="40095-138">(Pages de disposition) Restitue le contenu d’une page de contenu qui n’est pas dans les sections nommées.</span><span class="sxs-lookup"><span data-stu-id="40095-138">(Layout pages) Renders the content of a content page that is not in any named sections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

<span data-ttu-id="40095-139">Restitue une page de contenu à l’aide du chemin d’accès spécifié et des données supplémentaires facultatives.</span><span class="sxs-lookup"><span data-stu-id="40095-139">Renders a content page using the specified path and optional extra data.</span></span> <span data-ttu-id="40095-140">Vous pouvez obtenir les valeurs des paramètres supplémentaires à partir de `PageData` par position (par exemple, 1) ou par clé (par exemple, 2).</span><span class="sxs-lookup"><span data-stu-id="40095-140">You can get the values of the extra parameters from `PageData` by position (example 1) or key (example 2).</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

<span data-ttu-id="40095-141">(Pages de disposition) Restitue une section de contenu qui a un nom.</span><span class="sxs-lookup"><span data-stu-id="40095-141">(Layout pages) Renders a content section that has a name.</span></span> <span data-ttu-id="40095-142">Définissez *requis* sur false pour rendre une section facultative.</span><span class="sxs-lookup"><span data-stu-id="40095-142">Set *required* to false to make a section optional.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

<span data-ttu-id="40095-143">Obtient ou définit la valeur d’un cookie HTTP.</span><span class="sxs-lookup"><span data-stu-id="40095-143">Gets or sets the value of an HTTP cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

<span data-ttu-id="40095-144">Obtient les fichiers qui ont été téléchargés dans la requête actuelle.</span><span class="sxs-lookup"><span data-stu-id="40095-144">Gets the files that were uploaded in the current request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

<span data-ttu-id="40095-145">Obtient les données qui a été validées dans un formulaire (sous forme de chaînes).</span><span class="sxs-lookup"><span data-stu-id="40095-145">Gets data that was posted in a form (as strings).</span></span> <span data-ttu-id="40095-146">`Request[key]` vérifie à la fois le `Request.Form` et `Request.QueryString` collections.</span><span class="sxs-lookup"><span data-stu-id="40095-146">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

<span data-ttu-id="40095-147">Obtient les données qui a été spécifiées dans la chaîne de requête URL.</span><span class="sxs-lookup"><span data-stu-id="40095-147">Gets data that was specified in the URL query string.</span></span> <span data-ttu-id="40095-148">`Request[key]` vérifie à la fois le `Request.Form` et `Request.QueryString` collections.</span><span class="sxs-lookup"><span data-stu-id="40095-148">`Request[key]` checks both the `Request.Form` and the `Request.QueryString` collections.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

<span data-ttu-id="40095-149">Sélectivement désactive la validation pour un élément de formulaire, une valeur de chaîne de requête, un cookie ou une valeur d’en-tête de requête.</span><span class="sxs-lookup"><span data-stu-id="40095-149">Selectively disables request validation for a form element, query-string value, cookie, or header value.</span></span> <span data-ttu-id="40095-150">Validation de la demande est activée par défaut et empêche les utilisateurs de la validation de balisage ou tout autre contenu potentiellement dangereux.</span><span class="sxs-lookup"><span data-stu-id="40095-150">Request validation is enabled by default and prevents users from posting markup or other potentially dangerous content.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

<span data-ttu-id="40095-151">Ajoute un en-tête de serveur HTTP à la réponse.</span><span class="sxs-lookup"><span data-stu-id="40095-151">Adds an HTTP server header to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

<span data-ttu-id="40095-152">Met en cache la sortie de page pendant une période spécifiée.</span><span class="sxs-lookup"><span data-stu-id="40095-152">Caches the page output for a specified time.</span></span> <span data-ttu-id="40095-153">Définissez éventuellement *glissante* pour réinitialiser le délai d’attente sur chaque accès à la page et *varyByParams* pour mettre en cache les différentes versions de la page pour chaque chaîne de requête différents dans la demande de page.</span><span class="sxs-lookup"><span data-stu-id="40095-153">Optionally set *sliding* to reset the timeout on each page access and *varyByParams* to cache different versions of the page for each different query string in the page request.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

<span data-ttu-id="40095-154">Redirige la demande de navigateur à un nouvel emplacement.</span><span class="sxs-lookup"><span data-stu-id="40095-154">Redirects the browser request to a new location.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

<span data-ttu-id="40095-155">Définit le code d’état HTTP envoyé au navigateur.</span><span class="sxs-lookup"><span data-stu-id="40095-155">Sets the HTTP status code sent to the browser.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

<span data-ttu-id="40095-156">Écrit le contenu de *données* à la réponse avec un type MIME facultatif.</span><span class="sxs-lookup"><span data-stu-id="40095-156">Writes the contents of *data* to the response with an optional MIME type.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

<span data-ttu-id="40095-157">Écrit le contenu d’un fichier dans la réponse.</span><span class="sxs-lookup"><span data-stu-id="40095-157">Writes the contents of a file to the response.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

<span data-ttu-id="40095-158">(Pages de disposition) Définit une section de contenu qui a un nom.</span><span class="sxs-lookup"><span data-stu-id="40095-158">(Layout pages) Defines a content section that has a name.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

<span data-ttu-id="40095-159">Décode une chaîne est encodée en HTML.</span><span class="sxs-lookup"><span data-stu-id="40095-159">Decodes a string that is HTML encoded.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

<span data-ttu-id="40095-160">Encode une chaîne pour le rendu dans le balisage HTML.</span><span class="sxs-lookup"><span data-stu-id="40095-160">Encodes a string for rendering in HTML markup.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

<span data-ttu-id="40095-161">Retourne le chemin d’accès physique de serveur pour le chemin d’accès virtuel spécifié.</span><span class="sxs-lookup"><span data-stu-id="40095-161">Returns the server physical path for the specified virtual path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

<span data-ttu-id="40095-162">Décode le texte à partir d’une URL.</span><span class="sxs-lookup"><span data-stu-id="40095-162">Decodes text from a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

<span data-ttu-id="40095-163">Encode le texte à placer dans une URL.</span><span class="sxs-lookup"><span data-stu-id="40095-163">Encodes text to put in a URL.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

<span data-ttu-id="40095-164">Obtient ou définit une valeur qui existe jusqu'à ce que l’utilisateur ferme le navigateur.</span><span class="sxs-lookup"><span data-stu-id="40095-164">Gets or sets a value that exists until the user closes the browser.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

<span data-ttu-id="40095-165">Affiche une représentation sous forme de chaîne de valeur de l’objet.</span><span class="sxs-lookup"><span data-stu-id="40095-165">Displays a string representation of the object's value.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

<span data-ttu-id="40095-166">Obtient les données supplémentaires à partir de l’URL (par exemple, *MyPage/ExtraData*).</span><span class="sxs-lookup"><span data-stu-id="40095-166">Gets additional data from the URL (for example, */MyPage/ExtraData*).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

<span data-ttu-id="40095-167">Modifie le mot de passe pour l’utilisateur spécifié.</span><span class="sxs-lookup"><span data-stu-id="40095-167">Changes the password for the specified user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

<span data-ttu-id="40095-168">Confirme un compte à l’aide du jeton de confirmation du compte.</span><span class="sxs-lookup"><span data-stu-id="40095-168">Confirms an account using the account confirmation token.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

<span data-ttu-id="40095-169">Crée un nouveau compte d’utilisateur avec le nom d’utilisateur spécifié et le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="40095-169">Creates a new user account with the specified user name and password.</span></span> <span data-ttu-id="40095-170">Pour demander un jeton de confirmation, passez la valeur true pour *requireConfirmationToken.*</span><span class="sxs-lookup"><span data-stu-id="40095-170">To require a confirmation token, pass true for *requireConfirmationToken.*</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

<span data-ttu-id="40095-171">Obtient l’identificateur entier pour l’utilisateur actuellement connecté.</span><span class="sxs-lookup"><span data-stu-id="40095-171">Gets the integer identifier for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

<span data-ttu-id="40095-172">Obtient le nom de l’utilisateur actuellement connecté.</span><span class="sxs-lookup"><span data-stu-id="40095-172">Gets the name for the currently logged-in user.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

<span data-ttu-id="40095-173">Génère un jeton de réinitialisation de mot de passe qui peut être envoyé par courrier électronique à un utilisateur afin que l’utilisateur peut réinitialiser le mot de passe.</span><span class="sxs-lookup"><span data-stu-id="40095-173">Generates a password-reset token that can be sent in email to a user so that the user can reset the password.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

<span data-ttu-id="40095-174">Retourne l’ID d’utilisateur à partir du nom d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="40095-174">Returns the user ID from the user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

<span data-ttu-id="40095-175">Retourne la valeur true si l’utilisateur actuel s’est connecté.</span><span class="sxs-lookup"><span data-stu-id="40095-175">Returns true if the current user is logged in.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

<span data-ttu-id="40095-176">Retourne la valeur true si l’utilisateur a été confirmé (par exemple, via un message électronique de confirmation).</span><span class="sxs-lookup"><span data-stu-id="40095-176">Returns true if the user has been confirmed (for example, through a confirmation email).</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

<span data-ttu-id="40095-177">Retourne la valeur true si le nom de l’utilisateur actuel correspond au nom d’utilisateur spécifié.</span><span class="sxs-lookup"><span data-stu-id="40095-177">Returns true if the current user's name matches the specified user name.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

<span data-ttu-id="40095-178">Connecte l’utilisateur en définissant un jeton d’authentification dans le cookie.</span><span class="sxs-lookup"><span data-stu-id="40095-178">Logs the user in by setting an authentication token in the cookie.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

<span data-ttu-id="40095-179">Se connecte à l’utilisateur en supprimant le cookie de jeton d’authentification.</span><span class="sxs-lookup"><span data-stu-id="40095-179">Logs the user out by removing the authentication token cookie.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

<span data-ttu-id="40095-180">Si l’utilisateur n’est pas authentifié, définit l’état HTTP 401 (non autorisé).</span><span class="sxs-lookup"><span data-stu-id="40095-180">If the user is not authenticated, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

<span data-ttu-id="40095-181">Si l’utilisateur actuel n’est pas un membre de l’un des rôles spécifiés, définit l’état HTTP 401 (non autorisé).</span><span class="sxs-lookup"><span data-stu-id="40095-181">If the current user is not a member of one of the specified roles, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

<span data-ttu-id="40095-182">Si l’utilisateur actuel n’est pas l’utilisateur spécifié par *nom d’utilisateur*, définit l’état HTTP 401 (non autorisé).</span><span class="sxs-lookup"><span data-stu-id="40095-182">If the current user is not the user specified by *username*, sets the HTTP status to 401 (Unauthorized).</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

<span data-ttu-id="40095-183">Si le jeton de réinitialisation de mot de passe est valide, modifie le mot de passe pour le nouveau mot de passe.</span><span class="sxs-lookup"><span data-stu-id="40095-183">If the password reset token is valid, changes the user's password to the new password.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a><span data-ttu-id="40095-184">Données</span><span class="sxs-lookup"><span data-stu-id="40095-184">Data</span></span>

### `Database.Execute(SQLstatement [,parameters]`

<span data-ttu-id="40095-185">Exécute *SQLstatement* (avec les paramètres facultatifs) telles que INSERT, DELETE ou UPDATE et retourne le nombre d’enregistrements affectés.</span><span class="sxs-lookup"><span data-stu-id="40095-185">Executes *SQLstatement* (with optional parameters) such as INSERT, DELETE, or UPDATE and returns a count of affected records.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

<span data-ttu-id="40095-186">Retourne la colonne d’identité à partir de la dernière ligne insérée.</span><span class="sxs-lookup"><span data-stu-id="40095-186">Returns the identity column from the most recently inserted row.</span></span>

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

<span data-ttu-id="40095-187">Ouvre le fichier de base de données spécifiée ou la base de données spécifiée à l’aide d’une chaîne de connexion nommée à partir de la *Web.config* fichier.</span><span class="sxs-lookup"><span data-stu-id="40095-187">Opens either the specified database file or the database specified using a named connection string from the *Web.config* file.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

<span data-ttu-id="40095-188">Ouvre une base de données à l’aide de la chaîne de connexion.</span><span class="sxs-lookup"><span data-stu-id="40095-188">Opens a database using the connection string.</span></span> <span data-ttu-id="40095-189">(Elle s’oppose à `Database.Open`, qui utilise un nom de chaîne de connexion.)</span><span class="sxs-lookup"><span data-stu-id="40095-189">(This contrasts with `Database.Open`, which uses a connection string name.)</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

<span data-ttu-id="40095-190">La base de données à l’aide des requêtes *SQLstatement* (passant éventuellement des paramètres) et retourne les résultats sous forme de collection.</span><span class="sxs-lookup"><span data-stu-id="40095-190">Queries the database using *SQLstatement* (optionally passing parameters) and returns the results as a collection.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

<span data-ttu-id="40095-191">Exécute *SQLstatement* (avec les paramètres facultatifs) et retourne un enregistrement unique.</span><span class="sxs-lookup"><span data-stu-id="40095-191">Executes *SQLstatement* (with optional parameters) and returns a single record.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

<span data-ttu-id="40095-192">Exécute *SQLstatement* (avec les paramètres facultatifs) et retourne une valeur unique.</span><span class="sxs-lookup"><span data-stu-id="40095-192">Executes *SQLstatement* (with optional parameters) and returns a single value.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a><span data-ttu-id="40095-193">Programmes d’assistance</span><span class="sxs-lookup"><span data-stu-id="40095-193">Helpers</span></span>

### `Analytics.GetGoogleHtml(webPropertyId)`

<span data-ttu-id="40095-194">Restitue le code JavaScript d’Analytique Google pour l’ID spécifié.</span><span class="sxs-lookup"><span data-stu-id="40095-194">Renders the Google Analytics JavaScript code for the specified ID.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

<span data-ttu-id="40095-195">Restitue le code StatCounter Analytique JavaScript pour le projet spécifié.</span><span class="sxs-lookup"><span data-stu-id="40095-195">Renders the StatCounter Analytics JavaScript code for the specified project.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

<span data-ttu-id="40095-196">Restitue le code Yahoo Analytique JavaScript pour le compte spécifié.</span><span class="sxs-lookup"><span data-stu-id="40095-196">Renders the Yahoo Analytics JavaScript code for the specified account.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

<span data-ttu-id="40095-197">Transmet une recherche à Bing.</span><span class="sxs-lookup"><span data-stu-id="40095-197">Passes a search to Bing.</span></span> <span data-ttu-id="40095-198">Pour spécifier le site à la recherche et un titre pour la zone de recherche, vous pouvez définir le `Bing.SiteUrl` et `Bing.SiteTitle` propriétés.</span><span class="sxs-lookup"><span data-stu-id="40095-198">To specify the site to search and a title for the search box, you can set the `Bing.SiteUrl` and `Bing.SiteTitle` properties.</span></span> <span data-ttu-id="40095-199">Normalement, vous définissez ces propriétés le  *\_AppStart* page.</span><span class="sxs-lookup"><span data-stu-id="40095-199">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

<span data-ttu-id="40095-200">Initialise un graphique.</span><span class="sxs-lookup"><span data-stu-id="40095-200">Initializes a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

<span data-ttu-id="40095-201">Ajoute une légende à un graphique.</span><span class="sxs-lookup"><span data-stu-id="40095-201">Adds a legend to a chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

<span data-ttu-id="40095-202">Ajoute une série de valeurs au graphique.</span><span class="sxs-lookup"><span data-stu-id="40095-202">Adds a series of values to the chart.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

<span data-ttu-id="40095-203">Retourne un hachage pour les données spécifiées.</span><span class="sxs-lookup"><span data-stu-id="40095-203">Returns a hash for the specified data.</span></span> <span data-ttu-id="40095-204">L’algorithme par défaut est `sha256`.</span><span class="sxs-lookup"><span data-stu-id="40095-204">The default algorithm is `sha256`.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

<span data-ttu-id="40095-205">Permet aux utilisateurs de Facebook établir une connexion à des pages.</span><span class="sxs-lookup"><span data-stu-id="40095-205">Lets Facebook users make a connection to pages.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

<span data-ttu-id="40095-206">Restitue l’interface utilisateur pour le téléchargement des fichiers.</span><span class="sxs-lookup"><span data-stu-id="40095-206">Renders UI for uploading files.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

<span data-ttu-id="40095-207">Restitue la balise de jeux Xbox spécifiée.</span><span class="sxs-lookup"><span data-stu-id="40095-207">Renders the specified Xbox gamer tag.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

<span data-ttu-id="40095-208">Restitue l’image Gravatar pour l’adresse de messagerie spécifiée.</span><span class="sxs-lookup"><span data-stu-id="40095-208">Renders the Gravatar image for the specified email address.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

<span data-ttu-id="40095-209">Convertit un objet de données en une chaîne au format JavaScript Objet Notation (JSON).</span><span class="sxs-lookup"><span data-stu-id="40095-209">Converts a data object to a string in the JavaScript Object Notation (JSON) format.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

<span data-ttu-id="40095-210">Convertit une chaîne d’entrée encodées en JSON en un objet de données que vous pouvez parcourir ou les insérer dans une base de données.</span><span class="sxs-lookup"><span data-stu-id="40095-210">Converts a JSON-encoded input string to a data object that you can iterate over or insert into a database.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

<span data-ttu-id="40095-211">Effectue le rendu des liens de réseau sociales à l’aide du titre spécifié et l’URL facultatif.</span><span class="sxs-lookup"><span data-stu-id="40095-211">Renders social networking links using the specified title and optional URL.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

<span data-ttu-id="40095-212">Associe un message d’erreur à un champ de formulaire.</span><span class="sxs-lookup"><span data-stu-id="40095-212">Associates an error message with a form field.</span></span> <span data-ttu-id="40095-213">Utilisez le `ModelState` application d’assistance pour accéder à ce membre.</span><span class="sxs-lookup"><span data-stu-id="40095-213">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

<span data-ttu-id="40095-214">Associe un message d’erreur à un formulaire.</span><span class="sxs-lookup"><span data-stu-id="40095-214">Associates an error message with a form.</span></span> <span data-ttu-id="40095-215">Utilisez le `ModelState` application d’assistance pour accéder à ce membre.</span><span class="sxs-lookup"><span data-stu-id="40095-215">Use the `ModelState` helper to access this member.</span></span>

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

<span data-ttu-id="40095-216">Retourne true s’il n’y a aucune erreur de validation.</span><span class="sxs-lookup"><span data-stu-id="40095-216">Returns true if there are no validation errors.</span></span> <span data-ttu-id="40095-217">Utilisez le `ModelState` application d’assistance pour accéder à ce membre.</span><span class="sxs-lookup"><span data-stu-id="40095-217">Use the `ModelState` helper to access this member.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

<span data-ttu-id="40095-218">Affiche les propriétés et les valeurs d’un objet et tout enfant objets.</span><span class="sxs-lookup"><span data-stu-id="40095-218">Renders the properties and values of an object and any child objects.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

<span data-ttu-id="40095-219">Restitue le test de vérification reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="40095-219">Renders the reCAPTCHA verification test.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

<span data-ttu-id="40095-220">Définit les clés publiques et privées pour le service reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="40095-220">Sets public and private keys for the reCAPTCHA service.</span></span> <span data-ttu-id="40095-221">Normalement, vous définissez ces propriétés le  *\_AppStart* page.</span><span class="sxs-lookup"><span data-stu-id="40095-221">Normally you set these properties in the *\_AppStart* page.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

<span data-ttu-id="40095-222">Retourne le résultat du test reCAPTCHA.</span><span class="sxs-lookup"><span data-stu-id="40095-222">Returns the result of the reCAPTCHA test.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

<span data-ttu-id="40095-223">Affiche les informations d’état sur les Pages Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="40095-223">Renders status information about ASP.NET Web Pages.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

<span data-ttu-id="40095-224">Restitue un flux Twitter de l’utilisateur spécifié.</span><span class="sxs-lookup"><span data-stu-id="40095-224">Renders a Twitter stream for the specified user.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

<span data-ttu-id="40095-225">Restitue un flux Twitter pour le texte de recherche spécifié.</span><span class="sxs-lookup"><span data-stu-id="40095-225">Renders a Twitter stream for the specified search text.</span></span>

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

<span data-ttu-id="40095-226">Restitue un lecteur vidéo Flash pour le fichier spécifié avec l’option largeur et hauteur.</span><span class="sxs-lookup"><span data-stu-id="40095-226">Renders a Flash video player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

<span data-ttu-id="40095-227">Restitue un lecteur Windows Media pour le fichier spécifié avec l’option largeur et hauteur.</span><span class="sxs-lookup"><span data-stu-id="40095-227">Renders a Windows Media player for the specified file with optional width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

<span data-ttu-id="40095-228">Restitue un lecteur Silverlight spécifié *.xap* fichier avec la largeur et hauteur.</span><span class="sxs-lookup"><span data-stu-id="40095-228">Renders a Silverlight player for the specified *.xap* file with required width and height.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

<span data-ttu-id="40095-229">Retourne l’objet spécifié par *clé*, ou null si l’objet est introuvable.</span><span class="sxs-lookup"><span data-stu-id="40095-229">Returns the object specified by *key*, or null if the object is not found.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

<span data-ttu-id="40095-230">Supprime l’objet spécifié par *clé* à partir du cache.</span><span class="sxs-lookup"><span data-stu-id="40095-230">Removes the object specified by *key* from the cache.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

<span data-ttu-id="40095-231">Place *valeur* dans le cache sous le nom spécifié par *clé*.</span><span class="sxs-lookup"><span data-stu-id="40095-231">Puts *value* into the cache under the name specified by *key*.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

<span data-ttu-id="40095-232">Crée un `WebGrid` à l’aide des données à partir d’une requête de l’objet.</span><span class="sxs-lookup"><span data-stu-id="40095-232">Creates a new `WebGrid` object using data from a query.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

<span data-ttu-id="40095-233">Restitue la balise pour afficher des données dans une table HTML.</span><span class="sxs-lookup"><span data-stu-id="40095-233">Renders markup to display data in an HTML table.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

<span data-ttu-id="40095-234">Restitue un récepteur de radiomessagerie pour la `WebGrid` objet.</span><span class="sxs-lookup"><span data-stu-id="40095-234">Renders a pager for the `WebGrid` object.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

<span data-ttu-id="40095-235">Charge une image à partir du chemin d’accès spécifié.</span><span class="sxs-lookup"><span data-stu-id="40095-235">Loads an image from the specified path.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

<span data-ttu-id="40095-236">Ajoute l’image spécifiée en tant que filigrane.</span><span class="sxs-lookup"><span data-stu-id="40095-236">Adds the specified image as a watermark.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

<span data-ttu-id="40095-237">Ajoute le texte spécifié à l’image.</span><span class="sxs-lookup"><span data-stu-id="40095-237">Adds the specified text to the image.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

<span data-ttu-id="40095-238">Retourne l’image horizontalement ou verticalement.</span><span class="sxs-lookup"><span data-stu-id="40095-238">Flips the image horizontally or vertically.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

<span data-ttu-id="40095-239">Charge une image lorsqu’une image est publiée vers une page pendant le chargement d’un fichier.</span><span class="sxs-lookup"><span data-stu-id="40095-239">Loads an image when an image is posted to a page during a file upload.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

<span data-ttu-id="40095-240">Redimensionne une l’image.</span><span class="sxs-lookup"><span data-stu-id="40095-240">Resizes an the image.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

<span data-ttu-id="40095-241">Fait pivoter l’image vers la gauche ou droite.</span><span class="sxs-lookup"><span data-stu-id="40095-241">Rotates the image to the left or the right.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

<span data-ttu-id="40095-242">Enregistre l’image dans le chemin d’accès spécifié.</span><span class="sxs-lookup"><span data-stu-id="40095-242">Saves the image to the specified path.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

<span data-ttu-id="40095-243">Définit le mot de passe pour le serveur SMTP.</span><span class="sxs-lookup"><span data-stu-id="40095-243">Sets the password for the SMTP server.</span></span> <span data-ttu-id="40095-244">Normalement, vous définissez cette propriété le  *\_AppStart* page.</span><span class="sxs-lookup"><span data-stu-id="40095-244">Normally you set this property in the *\_AppStart* page.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

<span data-ttu-id="40095-245">Envoie un e-mail.</span><span class="sxs-lookup"><span data-stu-id="40095-245">Sends an email message.</span></span>

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

<span data-ttu-id="40095-246">Définit le nom du serveur SMTP.</span><span class="sxs-lookup"><span data-stu-id="40095-246">Sets the SMTP server name.</span></span> <span data-ttu-id="40095-247">Normalement, vous définissez cette propriété le<em>\_AppStart</em> page.</span><span class="sxs-lookup"><span data-stu-id="40095-247">Normally you set this property in the<em>\_AppStart</em> page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

<span data-ttu-id="40095-248">Définit le nom d’utilisateur pour le serveur SMTP.</span><span class="sxs-lookup"><span data-stu-id="40095-248">Sets the user name for the SMTP server.</span></span> <span data-ttu-id="40095-249">Normalement, vous devez définir cette propriété le  *\_AppStart* page.</span><span class="sxs-lookup"><span data-stu-id="40095-249">Normally you should set this property in the *\_AppStart* page.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a><span data-ttu-id="40095-250">Validation</span><span class="sxs-lookup"><span data-stu-id="40095-250">Validation</span></span>

### `Html.ValidationMessage(field)`

<span data-ttu-id="40095-251">(v2) Affiche un message d’erreur de validation pour le champ spécifié.</span><span class="sxs-lookup"><span data-stu-id="40095-251">(v2) Renders a validation error message for the specified field.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

<span data-ttu-id="40095-252">(v2) Affiche une liste de toutes les erreurs de validation.</span><span class="sxs-lookup"><span data-stu-id="40095-252">(v2) Displays a list of all validation errors.</span></span>

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

<span data-ttu-id="40095-253">(v2) Enregistre un élément d’entrée d’utilisateur pour le type de validation spécifié.</span><span class="sxs-lookup"><span data-stu-id="40095-253">(v2) Registers a user input element for the specified type of validation.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

<span data-ttu-id="40095-254">(v2) Restitue dynamiquement les attributs de classe CSS pour la validation côté client afin que vous pouvez mettre les messages d’erreur de validation.</span><span class="sxs-lookup"><span data-stu-id="40095-254">(v2) Dynamically renders CSS class attributes for client-side validation so that you can format validation error messages.</span></span> <span data-ttu-id="40095-255">(Nécessite que vous référencez les bibliothèques de script client approprié et que vous définissez des classes CSS.)</span><span class="sxs-lookup"><span data-stu-id="40095-255">(Requires that you reference the appropriate client-script libraries and that you define CSS classes.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

<span data-ttu-id="40095-256">(v2) Active la validation côté client pour le champ d’entrée d’utilisateur.</span><span class="sxs-lookup"><span data-stu-id="40095-256">(v2) Enables client-side validation for the user input field.</span></span> <span data-ttu-id="40095-257">(Nécessite que vous référencez les bibliothèques de script client approprié).</span><span class="sxs-lookup"><span data-stu-id="40095-257">(Requires that you reference the appropriate client-script libraries.)</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

<span data-ttu-id="40095-258">(v2) Retourne la valeur true si tous les éléments d’entrée utilisateur sont enregistrées pour la validation contiennent des valeurs valides.</span><span class="sxs-lookup"><span data-stu-id="40095-258">(v2) Returns true if all user input elements that are registred for validation contain valid values.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

<span data-ttu-id="40095-259">(v2) Spécifie que les utilisateurs doivent fournir une valeur pour l’élément d’entrée.</span><span class="sxs-lookup"><span data-stu-id="40095-259">(v2) Specifies that users must provide a value for the user input element.</span></span>

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

<span data-ttu-id="40095-260">(v2) Spécifie que les utilisateurs doivent fournir des valeurs pour chacun des éléments d’entrée utilisateur.</span><span class="sxs-lookup"><span data-stu-id="40095-260">(v2) Specifies that users must provide values for each of the user input elements.</span></span> <span data-ttu-id="40095-261">Cette méthode ne permet pas de spécifier un message d’erreur personnalisé.</span><span class="sxs-lookup"><span data-stu-id="40095-261">This method does not let you specify a custom error message.</span></span>

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

<span data-ttu-id="40095-262">(v2) Spécifie un test de validation lorsque vous utilisez la `Validation.Add` (méthode).</span><span class="sxs-lookup"><span data-stu-id="40095-262">(v2) Specifies a validation test when you use the `Validation.Add` method.</span></span>

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
