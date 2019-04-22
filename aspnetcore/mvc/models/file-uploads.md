---
title: Chargements de fichiers dans ASP.NET Core
author: ardalis
description: Comment utiliser la liaison de modèle et le streaming pour charger des fichiers dans ASP.NET Core MVC.
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
uid: mvc/models/file-uploads
ms.openlocfilehash: 831f0c84f0ff062e9e24ccbf4ca81b7143c66e66
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59468551"
---
# <a name="file-uploads-in-aspnet-core"></a>Chargements de fichiers dans ASP.NET Core

Par [Steve Smith](https://ardalis.com/)

Les actions d’ASP.NET MVC prennent en charge le chargement d’un ou plusieurs fichiers, avec une liaison de modèle simple pour les fichiers de petite taille ou avec le streaming pour les fichiers plus volumineux.

[Affichez ou téléchargez un exemple depuis GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample).

## <a name="uploading-small-files-with-model-binding"></a>Chargement de petits fichiers avec la liaison de modèle

Pour charger des petits fichiers, vous pouvez utiliser un formulaire HTML en plusieurs parties ou construire une demande POST avec JavaScript. Voici un exemple de formulaire utilisant Razor, qui prend en charge plusieurs fichiers chargés :

```html
<form method="post" enctype="multipart/form-data" asp-controller="UploadFiles" asp-action="Index">
    <div class="form-group">
        <div class="col-md-10">
            <p>Upload one or more files using this form:</p>
            <input type="file" name="files" multiple>
        </div>
    </div>
    <div class="form-group">
        <div class="col-md-10">
            <input type="submit" value="Upload">
        </div>
    </div>
</form>
```

Pour prendre en charge les chargements de fichiers, les formulaires HTML doivent spécifier un `enctype` défini comme étant `multipart/form-data`. L’élément d’entrée `files` montré ci-dessus prend en charge le chargement de plusieurs fichiers. Omettez l’attribut `multiple` sur cet élément d’entrée pour autoriser le chargement d’un seul fichier. Le balisage ci-dessus est rendu comme ceci dans un navigateur :

![Formulaire de chargement de fichier](file-uploads/_static/upload-form.png)

Les fichiers individuels chargés sur le serveur sont accessibles via la [liaison de modèle](xref:mvc/models/model-binding) avec l’interface [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile). `IFormFile` a la structure suivante :

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
> Ne vous basez pas ou ne faites pas confiance à la propriété `FileName` sans validation. La propriété `FileName` doit être utilisée seulement pour l’affichage.

Lors du chargement de fichiers avec la liaison de modèle et l’interface `IFormFile`, la méthode d’action peut accepter un seul `IFormFile` ou un `IEnumerable<IFormFile>` (ou `List<IFormFile>`) représentant plusieurs fichiers. L’exemple suivant boucle dans un ou plusieurs fichiers chargés, les enregistre dans le système de fichiers local et retourne le nombre total et la taille totale des fichiers chargés.

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

Les fichiers chargés avec la technique `IFormFile` sont placés dans une mémoire tampon ou sur disque du serveur web avant d’être traités. Dans la méthode d’action, le contenu de `IFormFile` est accessible sous forme de flux. En plus du système de fichiers local, les fichiers peuvent être diffusés en continu vers [Stockage Blob Azure](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs) ou [Entity Framework](/ef/core/index).

Pour stocker les données de fichiers binaires dans une base de données avec Entity Framework, définissez une propriété de type `byte[]` sur l’entité :

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

Spécifiez une propriété viewmodel de type `IFormFile` :

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> `IFormFile` peut être utilisé directement comme paramètre de méthode d’action ou comme propriété viewmodel, comme montré ci-dessus.

Copiez le `IFormFile` dans un flux et enregistrez-le dans le tableau d’octets :

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
        var user = new ApplicationUser 
        {
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
> Soyez prudent quand vous stockez des données binaires dans des bases de données relationnelles, car cela peut avoir un impact négatif sur les performances.

## <a name="uploading-large-files-with-streaming"></a>Chargement de fichiers volumineux avec le streaming

Si la taille ou la fréquence des chargements de fichiers pose des problèmes de ressources pour l’application, envisagez le chargement des fichiers par streaming au lieu de les placer en totalité dans une mémoire tampon, comme le fait l’approche par liaison de modèle ci-dessus. Si l’utilisation de `IFormFile` et de la liaison de modèle est une solution beaucoup plus simple, le streaming nécessite l’implémentation correcte d’un certain nombre d’étapes.

> [!NOTE]
> Tout fichier individuel mis en mémoire tampon et dépassant 64 Ko est déplacé de la RAM dans un fichier temporaire sur le disque du serveur. Les ressources (disque, RAM) utilisées par les chargements de fichiers varient selon le nombre et la taille des chargements de fichiers simultanés. Le problème du streaming n’est pas la performance : il est relatif à l’échelle. Si vous essayez de mettre trop de chargements en mémoire tampon, votre site se bloque dès lors qu’il manque de mémoire ou d’espace disque.

L’exemple suivant montre l’utilisation de JavaScript/Angular pour diffuser en continu vers une action de contrôleur. Le jeton anti-contrefaçon du fichier est généré en utilisant un attribut de filtre personnalisé, et il est passé dans les en-têtes HTTP et non pas dans le corps de la requête. Comme la méthode d’action traite les données chargées directement, la liaison de modèle est désactivée par un autre filtre. Dans l’action, le contenu du formulaire est lu en avec `MultipartReader`, qui lit chaque `MultipartSection` individuelle, traitant le fichier ou enregistrant le contenu selon ce qui est approprié. Une fois que toutes les sections ont été lues, l’action effectue sa propre liaison de modèle.

L’action initiale charge le formulaire et enregistre un jeton anti-contrefaçon dans un cookie (via l’attribut `GenerateAntiforgeryTokenCookieForAjax`) :

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

L’attribut utilise la prise en charge [anti-contrefaçon](xref:security/anti-request-forgery) intégrée d’ASP.NET Core pour définir un cookie avec un jeton de requête :

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

Angular passe automatiquement un jeton anti-contrefaçon dans un en-tête de requête nommé `X-XSRF-TOKEN`. L’application ASP.NET Core MVC est configurée pour référencer cet en-tête dans sa configuration, qui se trouve dans *Startup.cs* :

[!code-csharp[](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

L’attribut `DisableFormValueModelBinding`, montré ci-dessous, est utilisé pour désactiver la liaison de modèle pour la méthode d’action `Upload`.

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

La liaison de modèle étant désactivée, la méthode d’action `Upload` n’accepte pas de paramètres. Elle utilise directement la propriété `Request` de `ControllerBase`. Un `MultipartReader` est utilisé pour lire chaque section. Le fichier est enregistré avec un nom de fichier GUID, et les données clé/valeur sont stockées dans un `KeyValueAccumulator`. Une fois que toutes les sections ont été lues, le contenu du `KeyValueAccumulator` est utilisé pour lier les données de formulaire à un type de modèle.

Voici la méthode `Upload` complète :

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a>Résolution des problèmes

Voici certains problèmes courants rencontrés avec le chargement de fichiers et leurs solutions possibles.

### <a name="unexpected-not-found-error-with-iis"></a>Erreur inattendue Non trouvé avec IIS

L’erreur suivante indique que le chargement de votre fichier dépasse la valeur de `maxAllowedContentLength` configurée sur le serveur :

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

La valeur par défaut est `30000000`, ce qui correspond à environ 28,6 Mo. Vous pouvez personnaliser la valeur en modifiant *web.config* :

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

Ce paramètre s’applique seulement à IIS. Par défaut, ce comportement ne se produit pas dans le cas d’un hébergement sur Kestrel. Pour plus d’informations, consultez [Request Limits \<requestLimits\>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).

### <a name="null-reference-exception-with-iformfile"></a>Exception de référence null avec IFormFile

Si votre contrôleur accepte des fichiers chargés avec `IFormFile`, mais que vous découvrirez que la valeur est toujours null, vérifiez que votre formulaire HTML une valeur `enctype` pour `multipart/form-data`. Si cet attribut n’est pas défini sur l’élément `<form>`, le chargement des fichiers ne se produit pas et les arguments `IFormFile` liés sont null.
