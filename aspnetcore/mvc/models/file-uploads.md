---
title: Chargements de fichiers dans ASP.NET Core
author: ardalis
description: Comment utiliser la liaison de modèle et le streaming pour charger des fichiers dans ASP.NET Core MVC.
manager: wpickett
ms.author: riande
ms.date: 07/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/models/file-uploads
ms.openlocfilehash: 7ba4f6d9e3901c310fe9fa7a70382d9243d8b347
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32740411"
---
# <a name="file-uploads-in-aspnet-core"></a><span data-ttu-id="1b966-103">Chargements de fichiers dans ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1b966-103">File uploads in ASP.NET Core</span></span>

<span data-ttu-id="1b966-104">Par [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="1b966-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="1b966-105">Les actions d’ASP.NET MVC prennent en charge le chargement d’un ou plusieurs fichiers, avec une liaison de modèle simple pour les fichiers de petite taille ou avec le streaming pour les fichiers plus volumineux.</span><span class="sxs-lookup"><span data-stu-id="1b966-105">ASP.NET MVC actions support uploading of one or more files using simple model binding for smaller files or streaming for larger files.</span></span>

<span data-ttu-id="1b966-106">[Affichez ou téléchargez un exemple depuis GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample).</span><span class="sxs-lookup"><span data-stu-id="1b966-106">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)</span></span>

## <a name="uploading-small-files-with-model-binding"></a><span data-ttu-id="1b966-107">Chargement de petits fichiers avec la liaison de modèle</span><span class="sxs-lookup"><span data-stu-id="1b966-107">Uploading small files with model binding</span></span>

<span data-ttu-id="1b966-108">Pour charger des petits fichiers, vous pouvez utiliser un formulaire HTML en plusieurs parties ou construire une demande POST avec JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1b966-108">To upload small files, you can use a multi-part HTML form or construct a POST request using JavaScript.</span></span> <span data-ttu-id="1b966-109">Voici un exemple de formulaire utilisant Razor, qui prend en charge plusieurs fichiers chargés :</span><span class="sxs-lookup"><span data-stu-id="1b966-109">An example form using Razor, which supports multiple uploaded files, is shown below:</span></span>

```html
<form method="post" enctype="multipart/form-data" asp-controller="UploadFiles" asp-action="Index">
    <div class="form-group">
        <div class="col-md-10">
            <p>Upload one or more files using this form:</p>
            <input type="file" name="files" multiple />
        </div>
    </div>
    <div class="form-group">
        <div class="col-md-10">
            <input type="submit" value="Upload" />
        </div>
    </div>
</form>
```

<span data-ttu-id="1b966-110">Pour prendre en charge les chargements de fichiers, les formulaires HTML doivent spécifier un `enctype` défini comme étant `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="1b966-110">In order to support file uploads, HTML forms must specify an `enctype` of `multipart/form-data`.</span></span> <span data-ttu-id="1b966-111">L’élément d’entrée `files` montré ci-dessus prend en charge le chargement de plusieurs fichiers.</span><span class="sxs-lookup"><span data-stu-id="1b966-111">The `files` input element shown above supports uploading multiple files.</span></span> <span data-ttu-id="1b966-112">Omettez l’attribut `multiple` sur cet élément d’entrée pour autoriser le chargement d’un seul fichier.</span><span class="sxs-lookup"><span data-stu-id="1b966-112">Omit the `multiple` attribute on this input element to allow just a single file to be uploaded.</span></span> <span data-ttu-id="1b966-113">Le balisage ci-dessus est rendu comme ceci dans un navigateur :</span><span class="sxs-lookup"><span data-stu-id="1b966-113">The above markup renders in a browser as:</span></span>

![Formulaire de chargement de fichier](file-uploads/_static/upload-form.png)

<span data-ttu-id="1b966-115">Les fichiers individuels chargés sur le serveur sont accessibles via la [liaison de modèle](xref:mvc/models/model-binding) avec l’interface [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile).</span><span class="sxs-lookup"><span data-stu-id="1b966-115">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using the [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) interface.</span></span> <span data-ttu-id="1b966-116">`IFormFile` a la structure suivante :</span><span class="sxs-lookup"><span data-stu-id="1b966-116">`IFormFile` has the following structure:</span></span>

```csharp
public interface IFormFile
{
    string ContentType { get; }
    string ContentDisposition { get; }
    IHeaderDictionary Headers { get; }
    long Length { get; }
    string Name { get; }
    string FileName { get; }
    Stream OpenReadStream();
    void CopyTo(Stream target);
    Task CopyToAsync(Stream target, CancellationToken cancellationToken = null);
}
```

> [!WARNING]
> <span data-ttu-id="1b966-117">Ne vous basez pas ou ne faites pas confiance à la propriété `FileName` sans validation.</span><span class="sxs-lookup"><span data-stu-id="1b966-117">Don't rely on or trust the `FileName` property without validation.</span></span> <span data-ttu-id="1b966-118">La propriété `FileName` doit être utilisée seulement pour l’affichage.</span><span class="sxs-lookup"><span data-stu-id="1b966-118">The `FileName` property should only be used for display purposes.</span></span>

<span data-ttu-id="1b966-119">Lors du chargement de fichiers avec la liaison de modèle et l’interface `IFormFile`, la méthode d’action peut accepter un seul `IFormFile` ou un `IEnumerable<IFormFile>` (ou `List<IFormFile>`) représentant plusieurs fichiers.</span><span class="sxs-lookup"><span data-stu-id="1b966-119">When uploading files using model binding and the `IFormFile` interface, the action method can accept either a single `IFormFile` or an `IEnumerable<IFormFile>` (or `List<IFormFile>`) representing several files.</span></span> <span data-ttu-id="1b966-120">L’exemple suivant boucle dans un ou plusieurs fichiers chargés, les enregistre dans le système de fichiers local et retourne le nombre total et la taille totale des fichiers chargés.</span><span class="sxs-lookup"><span data-stu-id="1b966-120">The following example loops through one or more uploaded files, saves them to the local file system, and returns the total number and size of files uploaded.</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

<span data-ttu-id="1b966-121">Les fichiers chargés avec la technique `IFormFile` sont placés dans une mémoire tampon ou sur disque du serveur web avant d’être traités.</span><span class="sxs-lookup"><span data-stu-id="1b966-121">Files uploaded using the `IFormFile` technique are buffered in memory or on disk on the web server before being processed.</span></span> <span data-ttu-id="1b966-122">Dans la méthode d’action, le contenu de `IFormFile` est accessible sous forme de flux.</span><span class="sxs-lookup"><span data-stu-id="1b966-122">Inside the action method, the `IFormFile` contents are accessible as a stream.</span></span> <span data-ttu-id="1b966-123">En plus du système de fichiers local, les fichiers peuvent être diffusés en continu vers [Stockage Blob Azure](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) ou [Entity Framework](https://docs.microsoft.com/ef/core/index).</span><span class="sxs-lookup"><span data-stu-id="1b966-123">In addition to the local file system, files can be streamed to [Azure Blob storage](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) or [Entity Framework](https://docs.microsoft.com/ef/core/index).</span></span>

<span data-ttu-id="1b966-124">Pour stocker les données de fichiers binaires dans une base de données avec Entity Framework, définissez une propriété de type `byte[]` sur l’entité :</span><span class="sxs-lookup"><span data-stu-id="1b966-124">To store binary file data in a database using Entity Framework, define a property of type `byte[]` on the entity:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

<span data-ttu-id="1b966-125">Spécifiez une propriété viewmodel de type `IFormFile` :</span><span class="sxs-lookup"><span data-stu-id="1b966-125">Specify a viewmodel property of type `IFormFile`:</span></span>

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="1b966-126">`IFormFile` peut être utilisé directement comme paramètre de méthode d’action ou comme propriété viewmodel, comme montré ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="1b966-126">`IFormFile` can be used directly as an action method parameter or as a viewmodel property, as shown above.</span></span>

<span data-ttu-id="1b966-127">Copiez le `IFormFile` dans un flux et enregistrez-le dans le tableau d’octets :</span><span class="sxs-lookup"><span data-stu-id="1b966-127">Copy the `IFormFile` to a stream and save it to the byte array:</span></span>

```csharp
// POST: /Account/Register
[HttpPost]
[AllowAnonymous]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Register(RegisterViewModel model)
{
    ViewData["ReturnUrl"] = returnUrl;
    if  (ModelState.IsValid)
    {
        var user = new ApplicationUser {
          UserName = model.Email,
          Email = model.Email
        };
        using (var memoryStream = new MemoryStream())
        {
            await model.AvatarImage.CopyToAsync(memoryStream);
            user.AvatarImage = memoryStream.ToArray();
        }
    // additional logic omitted
    
    // Don't rely on or trust the model.AvatarImage.FileName property 
    // without validation.
}
```

> [!NOTE]
> <span data-ttu-id="1b966-128">Soyez prudent quand vous stockez des données binaires dans des bases de données relationnelles, car cela peut avoir un impact négatif sur les performances.</span><span class="sxs-lookup"><span data-stu-id="1b966-128">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>

## <a name="uploading-large-files-with-streaming"></a><span data-ttu-id="1b966-129">Chargement de fichiers volumineux avec le streaming</span><span class="sxs-lookup"><span data-stu-id="1b966-129">Uploading large files with streaming</span></span>

<span data-ttu-id="1b966-130">Si la taille ou la fréquence des chargements de fichiers pose des problèmes de ressources pour l’application, envisagez le chargement des fichiers par streaming au lieu de les placer en totalité dans une mémoire tampon, comme le fait l’approche par liaison de modèle ci-dessus.</span><span class="sxs-lookup"><span data-stu-id="1b966-130">If the size or frequency of file uploads is causing resource problems for the app, consider streaming the file upload rather than buffering it in its entirety, as the model binding approach shown above does.</span></span> <span data-ttu-id="1b966-131">Si l’utilisation de `IFormFile` et de la liaison de modèle est une solution beaucoup plus simple, le streaming nécessite l’implémentation correcte d’un certain nombre d’étapes.</span><span class="sxs-lookup"><span data-stu-id="1b966-131">While using `IFormFile` and model binding is a much simpler solution, streaming requires a number of steps to implement properly.</span></span>

> [!NOTE]
> <span data-ttu-id="1b966-132">Tout fichier individuel mis en mémoire tampon et dépassant 64 Ko est déplacé de la RAM dans un fichier temporaire sur le disque du serveur.</span><span class="sxs-lookup"><span data-stu-id="1b966-132">Any single buffered file exceeding 64KB will be moved from RAM to a temp file on disk on the server.</span></span> <span data-ttu-id="1b966-133">Les ressources (disque, RAM) utilisées par les chargements de fichiers varient selon le nombre et la taille des chargements de fichiers simultanés.</span><span class="sxs-lookup"><span data-stu-id="1b966-133">The resources (disk, RAM) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="1b966-134">Le problème du streaming n’est pas la performance : il est relatif à l’échelle.</span><span class="sxs-lookup"><span data-stu-id="1b966-134">Streaming isn't so much about perf, it's about scale.</span></span> <span data-ttu-id="1b966-135">Si vous essayez de mettre trop de chargements en mémoire tampon, votre site se bloque dès lors qu’il manque de mémoire ou d’espace disque.</span><span class="sxs-lookup"><span data-stu-id="1b966-135">If you try to buffer too many uploads, your site will crash when it runs out of memory or disk space.</span></span>

<span data-ttu-id="1b966-136">L’exemple suivant montre l’utilisation de JavaScript/Angular pour diffuser en continu vers une action de contrôleur.</span><span class="sxs-lookup"><span data-stu-id="1b966-136">The following example demonstrates using JavaScript/Angular to stream to a controller action.</span></span> <span data-ttu-id="1b966-137">Le jeton anti-contrefaçon du fichier est généré en utilisant un attribut de filtre personnalisé, et il est passé dans les en-têtes HTTP et non pas dans le corps de la requête.</span><span class="sxs-lookup"><span data-stu-id="1b966-137">The file's antiforgery token is generated using a custom filter attribute and passed in HTTP headers instead of in the request body.</span></span> <span data-ttu-id="1b966-138">Comme la méthode d’action traite les données chargées directement, la liaison de modèle est désactivée par un autre filtre.</span><span class="sxs-lookup"><span data-stu-id="1b966-138">Because the action method processes the uploaded data directly, model binding is disabled by another filter.</span></span> <span data-ttu-id="1b966-139">Dans l’action, le contenu du formulaire est lu en avec `MultipartReader`, qui lit chaque `MultipartSection` individuelle, traitant le fichier ou enregistrant le contenu selon ce qui est approprié.</span><span class="sxs-lookup"><span data-stu-id="1b966-139">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="1b966-140">Une fois que toutes les sections ont été lues, l’action effectue sa propre liaison de modèle.</span><span class="sxs-lookup"><span data-stu-id="1b966-140">Once all sections have been read, the action performs its own model binding.</span></span>

<span data-ttu-id="1b966-141">L’action initiale charge le formulaire et enregistre un jeton anti-contrefaçon dans un cookie (via l’attribut `GenerateAntiforgeryTokenCookieForAjax`) :</span><span class="sxs-lookup"><span data-stu-id="1b966-141">The initial action loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieForAjax` attribute):</span></span>

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

<span data-ttu-id="1b966-142">L’attribut utilise la prise en charge [anti-contrefaçon](xref:security/anti-request-forgery) intégrée d’ASP.NET Core pour définir un cookie avec un jeton de requête :</span><span class="sxs-lookup"><span data-stu-id="1b966-142">The attribute uses ASP.NET Core's built-in [Antiforgery](xref:security/anti-request-forgery) support to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

<span data-ttu-id="1b966-143">Angular passe automatiquement un jeton anti-contrefaçon dans un en-tête de requête nommé `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="1b966-143">Angular automatically passes an antiforgery token in a request header named `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="1b966-144">L’application ASP.NET Core MVC est configurée pour référencer cet en-tête dans sa configuration, qui se trouve dans *Startup.cs* :</span><span class="sxs-lookup"><span data-stu-id="1b966-144">The ASP.NET Core MVC app is configured to refer to this header in its configuration in *Startup.cs*:</span></span>

[!code-csharp[](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

<span data-ttu-id="1b966-145">L’attribut `DisableFormValueModelBinding`, montré ci-dessous, est utilisé pour désactiver la liaison de modèle pour la méthode d’action `Upload`.</span><span class="sxs-lookup"><span data-stu-id="1b966-145">The `DisableFormValueModelBinding` attribute, shown below, is used to disable model binding for the `Upload` action method.</span></span>

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

<span data-ttu-id="1b966-146">La liaison de modèle étant désactivée, la méthode d’action `Upload` n’accepte pas de paramètres.</span><span class="sxs-lookup"><span data-stu-id="1b966-146">Since model binding is disabled, the `Upload` action method doesn't accept parameters.</span></span> <span data-ttu-id="1b966-147">Elle utilise directement la propriété `Request` de `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="1b966-147">It works directly with the `Request` property of `ControllerBase`.</span></span> <span data-ttu-id="1b966-148">Un `MultipartReader` est utilisé pour lire chaque section.</span><span class="sxs-lookup"><span data-stu-id="1b966-148">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="1b966-149">Le fichier est enregistré avec un nom de fichier GUID, et les données clé/valeur sont stockées dans un `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="1b966-149">The file is saved with a GUID filename and the key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="1b966-150">Une fois que toutes les sections ont été lues, le contenu du `KeyValueAccumulator` est utilisé pour lier les données de formulaire à un type de modèle.</span><span class="sxs-lookup"><span data-stu-id="1b966-150">Once all sections have been read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="1b966-151">Voici la méthode `Upload` complète :</span><span class="sxs-lookup"><span data-stu-id="1b966-151">The complete `Upload` method is shown below:</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a><span data-ttu-id="1b966-152">Résolution des problèmes</span><span class="sxs-lookup"><span data-stu-id="1b966-152">Troubleshooting</span></span>

<span data-ttu-id="1b966-153">Voici certains problèmes courants rencontrés avec le chargement de fichiers et leurs solutions possibles.</span><span class="sxs-lookup"><span data-stu-id="1b966-153">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="unexpected-not-found-error-with-iis"></a><span data-ttu-id="1b966-154">Erreur inattendue Non trouvé avec IIS</span><span class="sxs-lookup"><span data-stu-id="1b966-154">Unexpected Not Found error with IIS</span></span>

<span data-ttu-id="1b966-155">L’erreur suivante indique que le chargement de votre fichier dépasse la valeur de `maxAllowedContentLength` configurée sur le serveur :</span><span class="sxs-lookup"><span data-stu-id="1b966-155">The following error indicates your file upload exceeds the server's configured `maxAllowedContentLength`:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="1b966-156">La valeur par défaut est `30000000`, ce qui correspond à environ 28,6 Mo.</span><span class="sxs-lookup"><span data-stu-id="1b966-156">The default setting is `30000000`, which is approximately 28.6MB.</span></span> <span data-ttu-id="1b966-157">Vous pouvez personnaliser la valeur en modifiant *web.config* :</span><span class="sxs-lookup"><span data-stu-id="1b966-157">The value can be customized by editing *web.config*:</span></span>

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- This will handle requests up to 50MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

<span data-ttu-id="1b966-158">Ce paramètre s’applique seulement à IIS.</span><span class="sxs-lookup"><span data-stu-id="1b966-158">This setting only applies to IIS.</span></span> <span data-ttu-id="1b966-159">Par défaut, ce comportement ne se produit pas dans le cas d’un hébergement sur Kestrel.</span><span class="sxs-lookup"><span data-stu-id="1b966-159">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="1b966-160">Pour plus d’informations, consultez [Request Limits \<requestLimits\>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="1b966-160">For more information, see [Request Limits \<requestLimits\>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="1b966-161">Exception de référence null avec IFormFile</span><span class="sxs-lookup"><span data-stu-id="1b966-161">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="1b966-162">Si votre contrôleur accepte des fichiers chargés avec `IFormFile`, mais que vous découvrirez que la valeur est toujours null, vérifiez que votre formulaire HTML une valeur `enctype` pour `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="1b966-162">If your controller is accepting uploaded files using `IFormFile` but you find that the value is always null, confirm that your HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="1b966-163">Si cet attribut n’est pas défini sur l’élément `<form>`, le chargement des fichiers ne se produit pas et les arguments `IFormFile` liés sont null.</span><span class="sxs-lookup"><span data-stu-id="1b966-163">If this attribute isn't set on the `<form>` element, the file upload won't occur and any bound `IFormFile` arguments will be null.</span></span>
