---
title: Charger des fichiers dans ASP.NET Core
author: guardrex
description: Comment utiliser la liaison de modèle et le streaming pour charger des fichiers dans ASP.NET Core MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/02/2019
uid: mvc/models/file-uploads
ms.openlocfilehash: de8bfee22e39dfc5a6ed254cf0555887891d4590
ms.sourcegitcommit: d81912782a8b0bd164f30a516ad80f8defb5d020
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72179307"
---
# <a name="upload-files-in-aspnet-core"></a>Charger des fichiers dans ASP.NET Core

Par [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/)et [Rutger Storm](https://github.com/rutix)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core prend en charge le chargement d’un ou plusieurs fichiers à l’aide d’une liaison de modèle mise en mémoire tampon pour les fichiers plus petits et la diffusion en continu sans mise en mémoire tampon pour les fichiers plus volumineux

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="security-considerations"></a>Considérations relatives à la sécurité

Soyez prudent lorsque vous fournissez aux utilisateurs la possibilité de charger des fichiers sur un serveur. Les attaquants peuvent tenter d’effectuer les opérations suivantes :

* Exécuter [des attaques par déni de service](/windows-hardware/drivers/ifs/denial-of-service) .
* Télécharger des virus ou des programmes malveillants.
* Compromettre les réseaux et les serveurs d’une autre manière.

Les étapes de sécurité qui réduisent la probabilité d’une attaque réussie sont les suivantes :

* Chargez les fichiers dans une zone de chargement de fichier dédiée sur le système, de préférence sur un lecteur non-système. À l’aide d’un emplacement dédié, il est plus facile d’imposer des restrictions de sécurité sur les fichiers téléchargés. Désactivez les autorisations d’exécution sur l’emplacement de chargement du fichier. &dagger;
* Ne conserve jamais les fichiers téléchargés dans la même arborescence de répertoires que l’application. &dagger;
* Utilisez un nom de fichier sécurisé déterminé par l’application. N’utilisez pas un nom de fichier fourni par l’utilisateur ou le nom de fichier non approuvé du fichier téléchargé. &dagger; Pour afficher un nom de fichier non approuvé dans une interface utilisateur ou dans un message de journalisation, encodez la valeur en HTML.
* Autorise uniquement un ensemble spécifique d’extensions de fichiers approuvées. &dagger;
* Vérifiez la signature de format de fichier pour empêcher un utilisateur de télécharger un fichier fictif. &dagger; Par exemple, n’autorisez pas un utilisateur à télécharger un fichier *. exe* avec une extension *. txt* .
* Vérifiez que les vérifications côté client sont également effectuées sur le serveur. &dagger; Les vérifications côté client sont faciles à contourner.
* Vérifiez la taille d’un fichier téléchargé et empêchez les chargements supérieurs à ceux attendus. &dagger;
* Lorsque les fichiers ne doivent pas être remplacés par un fichier téléchargé portant le même nom, vérifiez le nom du fichier par rapport à la base de données ou au stockage physique avant de charger le fichier.
* **Exécutez un programme de détection de virus et de logiciels malveillants sur le contenu chargé avant que le fichier ne soit stocké.**

l’exemple d’application &dagger;The illustre une approche qui répond aux critères.

> [!WARNING]
> Le chargement d’un code malveillant sur un système est généralement la première étape de l’exécution de code capable de :
>
> * Maîtrisez complètement un système.
> * Surchargez un système avec le résultat du blocage du système.
> * Compromettre les données utilisateur ou système.
> * Appliquez Graffiti à une interface utilisateur publique.
>
> Pour plus d’informations sur la réduction de la surface d’attaque quand vous acceptez des fichiers d’utilisateurs, consultez les ressources suivantes :
>
> * [Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Chargement de fichiers illimité)
> * Sécurité de la @no__t 0Azure : Vérifiez que les contrôles appropriés sont en place lors de l’acceptation des fichiers des utilisateurs @ no__t-0

Pour plus d’informations sur l’implémentation de mesures de sécurité, y compris des exemples de l’exemple d’application, consultez la section [validation](#validation) .

## <a name="storage-scenarios"></a>Scénarios de stockage

Les options de stockage courantes pour les fichiers sont les suivantes :

* Base de données

  * Pour les petits chargements de fichiers, une base de données est souvent plus rapide que les options de stockage physique (système de fichiers ou partage réseau).
  * Une base de données est souvent plus pratique que les options de stockage physique, car la récupération d’un enregistrement de base de données pour les données utilisateur peut fournir simultanément le contenu du fichier (par exemple, une image de l’avatar).
  * Une base de données est potentiellement moins coûteuse que l’utilisation d’un service de stockage de données.

* Stockage physique (système de fichiers ou partage réseau)

  * Pour les chargements de fichiers volumineux :
    * Les limites de base de données peuvent limiter la taille du téléchargement.
    * Le stockage physique est souvent moins économique que le stockage dans une base de données.
  * Le stockage physique est potentiellement moins onéreux que l’utilisation d’un service de stockage de données.
  * Le processus de l’application doit disposer d’autorisations en lecture et en écriture sur l’emplacement de stockage. **N’accordez jamais l’autorisation EXECUTE.**

* Service de stockage de données (par exemple, [stockage d’objets BLOB Azure](https://azure.microsoft.com/services/storage/blobs/))

  * Les services offrent généralement une évolutivité et une résilience améliorées par rapport aux solutions locales qui sont généralement sujettes à des points de défaillance uniques.
  * Les services sont potentiellement moins coûteux dans les scénarios d’infrastructure de stockage volumineux.

  Pour plus d’informations, consultez [Démarrage rapide : Utilisez .NET pour créer un objet BLOB dans le stockage d’objets @ no__t-0. La rubrique montre <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, mais <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> peut être utilisé pour enregistrer un <xref:System.IO.FileStream> dans le stockage BLOB lors de l’utilisation d’un <xref:System.IO.Stream>.

## <a name="file-upload-scenarios"></a>Scénarios de chargement de fichiers

La mise en mémoire tampon et la diffusion en continu sont deux approches générales pour le téléchargement de fichiers.

**Mise en tampon**

La totalité du fichier est lue dans un <xref:Microsoft.AspNetCore.Http.IFormFile>, qui est C# une représentation du fichier utilisé pour traiter ou enregistrer le fichier.

Les ressources (disque, mémoire) utilisées par les chargements de fichiers dépendent du nombre et de la taille des chargements de fichiers simultanés. Si une application tente de mettre en mémoire tampon un trop grand nombre de chargements, le site se bloque lorsqu’il manque de mémoire ou d’espace disque. Si la taille ou la fréquence des chargements de fichiers épuisent les ressources d’application, utilisez la diffusion en continu.

> [!NOTE]
> Tout fichier mis en mémoire tampon dépassant 64 Ko est déplacé de la mémoire vers un fichier temporaire sur le disque.

La mise en mémoire tampon de petits fichiers est traitée dans les sections suivantes de cette rubrique :

* [Stockage physique](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [Sauvegarde de la base de données](#upload-small-files-with-buffered-model-binding-to-a-database)

**Streaming**

Le fichier est reçu à partir d’une demande en plusieurs parties et est directement traité ou enregistré par l’application. La diffusion en continu n’améliore pas les performances de manière significative. La diffusion en continu réduit les demandes de mémoire ou d’espace disque lors du chargement de fichiers.

La diffusion en continu de fichiers volumineux est traitée dans la section [chargement de fichiers volumineux avec diffusion](#upload-large-files-with-streaming) .

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a>Charger des fichiers de petite taille avec une liaison de modèle mise en mémoire tampon vers un stockage physique

Pour télécharger des fichiers de petite taille, utilisez un formulaire en plusieurs parties ou créez une requête de publication à l’aide de JavaScript.

L’exemple suivant illustre l’utilisation d’un formulaire Razor Pages pour télécharger un seul fichier (*pages/BufferedSingleFileUploadPhysical. cshtml* dans l’exemple d’application) :

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
            <span asp-validation-for="FileUpload.FormFile"></span>
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload" />
</form>
```

L’exemple suivant est similaire à l’exemple précédent, à l’exception de :

* JavaScript ([API FETCH](https://developer.mozilla.org/docs/Web/API/Fetch_API)) est utilisé pour envoyer les données du formulaire.
* Il n’y a aucune validation.

```cshtml
<form action="BufferedSingleFileUploadPhysical/?handler=Upload" 
      enctype="multipart/form-data" onsubmit="AJAXSubmit(this);return false;" 
      method="post">
    <dl>
        <dt>
            <label for="FileUpload_FormFile">File</label>
        </dt>
        <dd>
            <input id="FileUpload_FormFile" type="file" 
                name="FileUpload.FormFile" />
        </dd>
    </dl>

    <input class="btn" type="submit" value="Upload" />

    <div style="margin-top:15px">
        <output name="result"></output>
    </div>
</form>

<script>
  async function AJAXSubmit (oFormElement) {
    var resultElement = oFormElement.elements.namedItem("result");
    const formData = new FormData(oFormElement);

    try {
    const response = await fetch(oFormElement.action, {
      method: 'POST',
      body: formData
    });

    if (response.ok) {
      window.location.href = '/';
    }

    resultElement.value = 'Result: ' + response.status + ' ' + 
      response.statusText;
    } catch (error) {
      console.error('Error:', error);
    }
  }
</script>
```

Pour exécuter la publication de formulaire dans JavaScript pour les clients qui [ne prennent pas en charge l’API FETCH](https://caniuse.com/#feat=fetch), utilisez l’une des approches suivantes :

* Utilisez un Polyfill d’extraction (par exemple, [Window. Fetch Polyfill (GitHub/fetch)](https://github.com/github/fetch)).
* Utilisez `XMLHttpRequest`. Exemple :

  ```javascript
  <script>
    "use strict";

    function AJAXSubmit (oFormElement) {
      var oReq = new XMLHttpRequest();
      oReq.onload = function(e) { 
      oFormElement.elements.namedItem("result").value = 
        'Result: ' + this.status + ' ' + this.statusText;
      };
      oReq.open("post", oFormElement.action);
      oReq.send(new FormData(oFormElement));
    }
  </script>
  ```

Afin de prendre en charge les chargements de fichiers, les formulaires HTML doivent spécifier un type d’encodage (`enctype`) de `multipart/form-data`.

Pour qu’un élément d’entrée `files` prenne en charge le téléchargement de plusieurs fichiers, fournissez l’attribut `multiple` sur l’élément `<input>` :

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

Les fichiers individuels téléchargés vers le serveur sont accessibles via la [liaison de modèle](xref:mvc/models/model-binding) à l’aide de <xref:Microsoft.AspNetCore.Http.IFormFile>. L’exemple d’application illustre plusieurs chargements de fichiers mis en mémoire tampon pour les scénarios de base de données et de stockage physique.

> [!WARNING]
> N’utilisez pas ou n’approuvez pas la propriété `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> sans validation. La propriété `FileName` doit uniquement être utilisée à des fins d’affichage et uniquement après l’encodage HTML de la valeur.
>
> Les exemples fournis jusqu’à présent ne prennent pas en compte les considérations de sécurité. Des informations supplémentaires sont fournies par les sections suivantes et l' [exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):
>
> * [Considérations relatives à la sécurité](#security-considerations)
> * [Validation](#validation)

Lors du chargement de fichiers à l’aide de la liaison de modèle et de <xref:Microsoft.AspNetCore.Http.IFormFile>, la méthode d’action peut accepter :

* Une seule <xref:Microsoft.AspNetCore.Http.IFormFile>.
* L’un des regroupements suivants qui représentent plusieurs fichiers :
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * [Liste](xref:System.Collections.Generic.List`1)\< @ no__t-2 @ no__t-3

> [!NOTE]
> La liaison correspond aux fichiers de formulaire par nom. Par exemple, la valeur HTML `name` dans `<input type="file" name="formFile">` doit correspondre au C# paramètre/à la propriété lié (`FormFile`). Pour plus d’informations, consultez la section valeur de l' [attribut de nom de correspondance pour le nom de paramètre de la méthode de publication](#match-name-attribute-value-to-parameter-name-of-post-method) .

L’exemple suivant :

* Effectue une boucle sur un ou plusieurs fichiers téléchargés.
* Utilise [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) pour retourner le chemin d’accès complet d’un fichier, y compris le nom de fichier. 
* Enregistre les fichiers dans le système de fichiers local à l’aide d’un nom de fichier généré par l’application.
* Retourne le nombre total et la taille des fichiers téléchargés.

```csharp
public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> files)
{
    long size = files.Sum(f => f.Length);

    foreach (var formFile in files)
    {
        if (formFile.Length > 0)
        {
            var filePath = Path.GetTempFileName();

            using (var stream = System.IO.File.Create(filePath))
            {
                await formFile.CopyToAsync(stream);
            }
        }
    }

    // Process uploaded files
    // Don't rely on or trust the FileName property without validation.

    return Ok(new { count = files.Count, size, filePath });
}
```

Utilisez `Path.GetRandomFileName` pour générer un nom de fichier sans chemin d’accès. Dans l’exemple suivant, le chemin d’accès est obtenu à partir de la configuration :

```csharp
foreach (var formFile in files)
{
    if (formFile.Length > 0)
    {
        var filePath = Path.Combine(_config["StoredFilesPath"], 
            Path.GetRandomFileName());

        using (var stream = System.IO.File.Create(filePath))
        {
            await formFile.CopyToAsync(stream);
        }
    }
}
```

Le chemin d’accès passé à la <xref:System.IO.FileStream> *doit* inclure le nom de fichier. Si le nom de fichier n’est pas fourni, un <xref:System.UnauthorizedAccessException> est levé au moment de l’exécution.

Les fichiers chargés à l’aide de la technique <xref:Microsoft.AspNetCore.Http.IFormFile> sont mis en mémoire tampon ou sur disque sur le serveur avant le traitement. À l’intérieur de la méthode d’action, le contenu <xref:Microsoft.AspNetCore.Http.IFormFile> est accessible en tant que <xref:System.IO.Stream>. Outre le système de fichiers local, les fichiers peuvent être enregistrés sur un partage réseau ou un service de stockage de fichiers, tel que le [stockage d’objets BLOB Azure](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).

Pour obtenir un autre exemple qui effectue une boucle sur plusieurs fichiers pour le téléchargement et utilise des noms de fichiers sécurisés, consultez *pages/BufferedMultipleFileUploadPhysical. cshtml. cs* dans l’exemple d’application.

> [!WARNING]
> [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) lève une <xref:System.IO.IOException> si plus de 65 535 fichiers sont créés sans supprimer les fichiers temporaires précédents. La limite de 65 535 fichiers est une limite par serveur. Pour plus d’informations sur cette limite sur le système d’exploitation Windows, consultez les notes dans les rubriques suivantes :
>
> * [GetTempFileNameA fonction)](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a>Charger des fichiers de petite taille avec une liaison de modèle mise en mémoire tampon vers une base de données

Pour stocker des données de fichier binaires dans une base de données à l’aide de [Entity Framework](/ef/core/index), définissez une propriété de tableau <xref:System.Byte> sur l’entité :

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

Spécifiez une propriété de modèle de page pour la classe qui comprend un <xref:Microsoft.AspNetCore.Http.IFormFile> :

```csharp
public class BufferedSingleFileUploadDbModel : PageModel
{
    ...

    [BindProperty]
    public BufferedSingleFileUploadDb FileUpload { get; set; }

    ...
}

public class BufferedSingleFileUploadDb
{
    [Required]
    [Display(Name="File")]
    public IFormFile FormFile { get; set; }
}
```

> [!NOTE]
> <xref:Microsoft.AspNetCore.Http.IFormFile> peut être utilisé directement comme paramètre de méthode d’action ou comme propriété de modèle liée. L’exemple précédent utilise une propriété de modèle liée.

La `FileUpload` est utilisée dans le formulaire de Razor Pages :

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload">
</form>
```

Lorsque le formulaire est publié sur le serveur, copiez le <xref:Microsoft.AspNetCore.Http.IFormFile> dans un flux et enregistrez-le en tant que tableau d’octets dans la base de données. Dans l’exemple suivant, `_dbContext` stocke le contexte de base de données de l’application :

```csharp
public async Task<IActionResult> OnPostUploadAsync()
{
    using (var memoryStream = new MemoryStream())
    {
        await FileUpload.FormFile.CopyToAsync(memoryStream);

        // Upload the file if less than 2 MB
        if (memoryStream.Length < 2097152)
        {
            var file = new AppFile()
            {
                Content = memoryStream.ToArray()
            };

            _dbContext.File.Add(file);

            await _dbContext.SaveChangesAsync();
        }
        else
        {
            ModelState.AddModelError("File", "The file is too large.");
        }
    }

    return Page();
}
```

L’exemple précédent est semblable à un scénario illustré dans l’exemple d’application :

* *Pages/BufferedSingleFileUploadDb. cshtml*
* *Pages/BufferedSingleFileUploadDb. cshtml. cs*

> [!WARNING]
> Soyez prudent quand vous stockez des données binaires dans des bases de données relationnelles, car cela peut avoir un impact négatif sur les performances.
>
> N’utilisez pas ou n’approuvez pas la propriété `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> sans validation. La propriété `FileName` doit uniquement être utilisée à des fins d’affichage et uniquement après l’encodage HTML.
>
> Les exemples fournis ne prennent pas en compte les considérations de sécurité. Des informations supplémentaires sont fournies par les sections suivantes et l' [exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):
>
> * [Considérations relatives à la sécurité](#security-considerations)
> * [Validation](#validation)

### <a name="upload-large-files-with-streaming"></a>Charger des fichiers volumineux avec streaming

L’exemple suivant montre comment utiliser JavaScript pour diffuser un fichier vers une action de contrôleur. Le jeton anti-contrefaçon du fichier est généré à l’aide d’un attribut de filtre personnalisé et transmis aux en-têtes HTTP du client plutôt que dans le corps de la demande. Étant donné que la méthode d’action traite directement les données chargées, la liaison de modèle de formulaire est désactivée par un autre filtre personnalisé. Dans l’action, le contenu du formulaire est lu en avec `MultipartReader`, qui lit chaque `MultipartSection` individuelle, traitant le fichier ou enregistrant le contenu selon ce qui est approprié. Une fois les sections en plusieurs parties lues, l’action effectue sa propre liaison de modèle.

La réponse de page initiale charge le formulaire et enregistre un jeton anti-contrefaçon dans un cookie (via l’attribut `GenerateAntiforgeryTokenCookieAttribute`). L’attribut utilise la [prise en charge de l’anticontrefaçon](xref:security/anti-request-forgery) intégrée de ASP.net Core pour définir un cookie avec un jeton de demande :

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

Le `DisableFormValueModelBindingAttribute` est utilisé pour désactiver la liaison de modèle :

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

Dans l’exemple d’application, `GenerateAntiforgeryTokenCookieAttribute` et `DisableFormValueModelBindingAttribute` sont appliqués en tant que filtres aux modèles d’application de page `/StreamedSingleFileUploadDb` et `/StreamedSingleFileUploadPhysical` dans `Startup.ConfigureServices` à l’aide des [conventions de Razor pages](xref:razor-pages/razor-pages-conventions):

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Startup.cs?name=snippet_AddRazorPages&highlight=8-11,17-20)]

Étant donné que la liaison de modèle ne lit pas le formulaire, les paramètres qui sont liés depuis le formulaire ne sont pas liés (la requête, l’itinéraire et l’en-tête continuent de fonctionner). La méthode d’action fonctionne directement avec la propriété `Request`. Un `MultipartReader` est utilisé pour lire chaque section. Les données de clé/valeur sont stockées dans un `KeyValueAccumulator`. Une fois les sections en plusieurs parties lues, le contenu du `KeyValueAccumulator` est utilisé pour lier les données de formulaire à un type de modèle.

La méthode `StreamingController.UploadDatabase` complète pour la diffusion en continu vers une base de données avec EF Core :

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

`MultipartRequestHelper` (*Utilities/MultipartRequestHelper. cs*) :

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

La méthode `StreamingController.UploadPhysical` complète pour la diffusion en continu vers un emplacement physique :

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

Dans l’exemple d’application, les contrôles de validation sont gérés par `FileHelpers.ProcessStreamedFile`.

## <a name="validation"></a>Validation

La classe `FileHelpers` de l’exemple d’application illustre plusieurs vérifications pour les chargements de fichiers mis en mémoire tampon <xref:Microsoft.AspNetCore.Http.IFormFile> et en continu. Pour le traitement des chargements de fichiers mis en mémoire tampon <xref:Microsoft.AspNetCore.Http.IFormFile> dans l’exemple d’application, consultez la méthode `ProcessFormFile` dans le fichier *Utilities/FileHelpers. cs* . Pour le traitement de fichiers en continu, consultez la méthode `ProcessStreamedFile` dans le même fichier.

> [!WARNING]
> Les méthodes de traitement de validation présentées dans l’exemple d’application n’analysent pas le contenu des fichiers téléchargés. Dans la plupart des scénarios de production, une API de détection de virus et de logiciels malveillants est utilisée sur le fichier avant que le fichier soit mis à la disposition des utilisateurs ou d’autres systèmes.
>
> Bien que l’exemple de rubrique fournit un exemple fonctionnel de techniques de validation, n’implémentez pas la classe `FileHelpers` dans une application de production, sauf si vous :
>
> * Comprenez pleinement l’implémentation.
> * Modifiez l’implémentation en fonction de l’environnement et des spécifications de l’application.
>
> **N’implémentez jamais non plus de code de sécurité dans une application sans répondre à ces exigences.**

### <a name="content-validation"></a>Validation du contenu

**Utilisez une API d’analyse de virus/programme malveillant sur le contenu chargé.**

L’analyse des fichiers exige des ressources serveur dans des scénarios de volume élevé. Si les performances de traitement des demandes sont réduites en raison de l’analyse de fichiers, envisagez de décharger le travail d’analyse vers un [service en arrière-plan](xref:fundamentals/host/hosted-services), éventuellement un service qui s’exécute sur un serveur différent du serveur de l’application. En règle générale, les fichiers téléchargés sont conservés dans une zone en quarantaine jusqu’à ce que l’antivirus en arrière-plan les vérifie. Lorsqu’un fichier réussit, le fichier est déplacé vers l’emplacement de stockage de fichiers normal. Ces étapes sont généralement effectuées conjointement avec un enregistrement de base de données qui indique l’état d’analyse d’un fichier. À l’aide de cette approche, l’application et le serveur d’applications restent concentrés sur la réponse aux requêtes.

### <a name="file-extension-validation"></a>Validation de l’extension de fichier

L’extension du fichier chargé doit être vérifiée par rapport à une liste d’extensions autorisées. Exemple :

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a>Validation de la signature de fichier

La signature d’un fichier est déterminée par les premiers octets au début d’un fichier. Ces octets peuvent être utilisés pour indiquer si l’extension correspond au contenu du fichier. L’exemple d’application vérifie les signatures de fichiers pour quelques types de fichiers courants. Dans l’exemple suivant, la signature de fichier pour une image JPEG est vérifiée par rapport au fichier :

```csharp
private static readonly Dictionary<string, List<byte[]>> _fileSignature = 
    new Dictionary<string, List<byte[]>>
{
    { ".jpeg", new List<byte[]>
        {
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE2 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE3 },
        }
    },
};

using (var reader = new BinaryReader(uploadedFileData))
{
    var signatures = _fileSignature[ext];
    var headerBytes = reader.ReadBytes(signatures.Max(m => m.Length));
    
    return signatures.Any(signature => 
        headerBytes.Take(signature.Length).SequenceEqual(signature));
}
```

Pour obtenir des signatures de fichiers supplémentaires, consultez la [base de données signatures de fichiers](https://www.filesignatures.net/) et les spécifications de fichier officielles.

### <a name="file-name-security"></a>Sécurité des noms de fichiers

N’utilisez jamais un nom de fichier fourni par le client pour enregistrer un fichier dans le stockage physique. Créez un nom de fichier sécurisé pour le fichier à l’aide de [Path. GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) ou [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) pour créer un chemin d’accès complet (y compris le nom de fichier) pour le stockage temporaire.

Razor code automatiquement les valeurs de propriété pour l’affichage. Le code suivant peut être utilisé en toute sécurité :

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

En dehors de Razor, toujours <xref:System.Net.WebUtility.HtmlEncode*> le contenu du nom de fichier à partir de la demande d’un utilisateur.

De nombreuses implémentations doivent inclure une vérification de l’existence du fichier ; dans le cas contraire, le fichier est remplacé par un fichier du même nom. Fournissez une logique supplémentaire pour répondre aux spécifications de votre application.

### <a name="size-validation"></a>Validation de la taille

Limitez la taille des fichiers téléchargés.

Dans l’exemple d’application, la taille du fichier est limitée à 2 Mo (indiquée en octets). La limite est fournie via la [configuration](xref:fundamentals/configuration/index) à partir du fichier *appSettings. JSON* :

```json
{
  "FileSizeLimit": 2097152
}
```

La `FileSizeLimit` est injectée dans les classes `PageModel` :

```csharp
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    private readonly long _fileSizeLimit;

    public BufferedSingleFileUploadPhysicalModel(IConfiguration config)
    {
        _fileSizeLimit = config.GetValue<long>("FileSizeLimit");
    }

    ...
}
```

Quand une taille de fichier dépasse la limite, le fichier est rejeté :

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a>Valeur de l’attribut de nom de correspondance avec le nom de paramètre de la méthode de publication

Dans les formulaires non-Razor qui PUBLIEnt des données de formulaire ou utilisent directement le `FormData` de JavaScript, le nom spécifié dans l’élément du formulaire ou `FormData` doit correspondre au nom du paramètre dans l’action du contrôleur.

Dans l’exemple suivant :

* Lors de l’utilisation d’un élément `<input>`, l’attribut `name` est défini sur la valeur `battlePlans` :

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* Lors de l’utilisation de `FormData` dans JavaScript, le nom est défini sur la valeur `battlePlans` :

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

Utilisez un nom correspondant pour le paramètre de la C# méthode (`battlePlans`) :

* Pour une méthode de gestionnaire de page Razor Pages nommée `Upload` :

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* Pour une méthode d’action du billet de contrôleur MVC :

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a>Configuration du serveur et de l’application

### <a name="multipart-body-length-limit"></a>Longueur limite du corps en plusieurs parties

<xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> définit la limite de la longueur de chaque corps en plusieurs parties. Les sections de formulaire qui dépassent cette limite lèvent une <xref:System.IO.InvalidDataException> lorsqu’elles sont analysées. La valeur par défaut est 134 217 728 (128 Mo). Personnaliser la limite à l’aide du paramètre <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> dans `Startup.ConfigureServices` :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<FormOptions>(options =>
    {
        // Set the limit to 256 MB
        options.MultipartBodyLengthLimit = 268435456;
    });
}
```

<xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> est utilisé pour définir la <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> pour une seule page ou action.

Dans une application Razor Pages, appliquez le filtre avec une [Convention](xref:razor-pages/razor-pages-conventions) dans `Startup.ConfigureServices` :

```csharp
services.AddRazorPages()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model.Filters.Add(
                    new RequestFormLimitsAttribute()
                    {
                        // Set the limit to 256 MB
                        MultipartBodyLengthLimit = 268435456
                    });
    });
```

Dans une application Razor Pages ou une application MVC, appliquez le filtre au modèle de page ou à la méthode d’action :

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a>Taille maximale du corps de la demande Kestrel

Pour les applications hébergées par Kestrel, la taille de corps de requête maximale par défaut est de 30 millions octets, soit environ 28,6 Mo. Personnaliser la limite à l’aide de l’option de serveur [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel :

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            // Handle requests up to 50 MB
            options.Limits.MaxRequestBodySize = 52428800;
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> est utilisé pour définir le [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) pour une seule page ou action.

Dans une application Razor Pages, appliquez le filtre avec une [Convention](xref:razor-pages/razor-pages-conventions) dans `Startup.ConfigureServices` :

```csharp
services.AddRazorPages()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model =>
                {
                    // Handle requests up to 50 MB
                    model.Filters.Add(
                        new RequestSizeLimitAttribute(52428800));
                });
    });
```

Dans une application de pages Razor ou une application MVC, appliquez le filtre à la classe du gestionnaire de page ou à la méthode d’action :

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

La `RequestSizeLimitAttribute` peut également être appliquée à l’aide de la directive Razor [@attribute](xref:mvc/views/razor#attribute) :

```cshtml
@attribute [RequestSizeLimitAttribute(52428800)]
```

### <a name="other-kestrel-limits"></a>Autres limites Kestrel

D’autres limites Kestrel peuvent s’appliquer aux applications hébergées par Kestrel :

* [Nombre maximal de connexions client](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [Taux de données de demande et de réponse](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a>Limite de longueur du contenu IIS

La limite de demandes par défaut (`maxAllowedContentLength`) est de 30 millions octets, soit environ 28,6 Mo. Personnaliser la limite dans le fichier *Web. config* :

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- Handle requests up to 50 MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

Ce paramètre s’applique seulement à IIS. Par défaut, ce comportement ne se produit pas dans le cas d’un hébergement sur Kestrel. Pour plus d’informations, consultez [limites de demande \<requestLimits >](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).

Les limitations du module ASP.NET Core ou la présence du module de filtrage des demandes IIS peuvent limiter les chargements à 2 ou 4 Go. Pour plus d’informations, consultez [Impossible de télécharger un fichier d’une taille supérieure à 2 Go (ASPNET/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).

## <a name="troubleshoot"></a>Résolution des problèmes

Voici certains problèmes courants rencontrés avec le chargement de fichiers et leurs solutions possibles.

### <a name="not-found-error-when-deployed-to-an-iis-server"></a>Erreur introuvable lors du déploiement sur un serveur IIS

L’erreur suivante indique que le fichier chargé dépasse la longueur du contenu configurée du serveur :

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

Pour plus d’informations sur l’amélioration de la limite, consultez la section [limite de longueur du contenu IIS](#iis-content-length-limit) .

### <a name="connection-failure"></a>Échec de la connexion

Une erreur de connexion et une connexion du serveur de réinitialisation indiquent probablement que le fichier téléchargé dépasse la taille maximale du corps de la demande de Kestrel. Pour plus d’informations, consultez la section [taille maximale du corps de la demande Kestrel](#kestrel-maximum-request-body-size) . Les limites de connexion du client Kestrel peuvent également nécessiter des ajustements.

### <a name="null-reference-exception-with-iformfile"></a>Exception de référence null avec IFormFile

Si le contrôleur accepte les fichiers téléchargés à l’aide de <xref:Microsoft.AspNetCore.Http.IFormFile>, mais que la valeur est `null`, vérifiez que le formulaire HTML spécifie une valeur `enctype` de `multipart/form-data`. Si cet attribut n’est pas défini sur l’élément `<form>`, le chargement du fichier ne se produit pas et tous les arguments <xref:Microsoft.AspNetCore.Http.IFormFile> liés sont `null`. Vérifiez également que l' [appellation de chargement dans les données de formulaire correspond à celle de l’application](#match-name-attribute-value-to-parameter-name-of-post-method).

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core prend en charge le chargement d’un ou plusieurs fichiers à l’aide d’une liaison de modèle mise en mémoire tampon pour les fichiers plus petits et la diffusion en continu sans mise en mémoire tampon pour les fichiers plus volumineux

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="security-considerations"></a>Considérations relatives à la sécurité

Soyez prudent lorsque vous fournissez aux utilisateurs la possibilité de charger des fichiers sur un serveur. Les attaquants peuvent tenter d’effectuer les opérations suivantes :

* Exécuter [des attaques par déni de service](/windows-hardware/drivers/ifs/denial-of-service) .
* Télécharger des virus ou des programmes malveillants.
* Compromettre les réseaux et les serveurs d’une autre manière.

Les étapes de sécurité qui réduisent la probabilité d’une attaque réussie sont les suivantes :

* Chargez les fichiers dans une zone de chargement de fichier dédiée sur le système, de préférence sur un lecteur non-système. À l’aide d’un emplacement dédié, il est plus facile d’imposer des restrictions de sécurité sur les fichiers téléchargés. Désactivez les autorisations d’exécution sur l’emplacement de chargement du fichier. &dagger;
* Ne conserve jamais les fichiers téléchargés dans la même arborescence de répertoires que l’application. &dagger;
* Utilisez un nom de fichier sécurisé déterminé par l’application. N’utilisez pas un nom de fichier fourni par l’utilisateur ou le nom de fichier non approuvé du fichier téléchargé. &dagger; Pour afficher un nom de fichier non approuvé dans une interface utilisateur ou dans un message de journalisation, encodez la valeur en HTML.
* Autorise uniquement un ensemble spécifique d’extensions de fichiers approuvées. &dagger;
* Vérifiez la signature de format de fichier pour empêcher un utilisateur de télécharger un fichier fictif. &dagger; Par exemple, n’autorisez pas un utilisateur à télécharger un fichier *. exe* avec une extension *. txt* .
* Vérifiez que les vérifications côté client sont également effectuées sur le serveur. &dagger; Les vérifications côté client sont faciles à contourner.
* Vérifiez la taille d’un fichier téléchargé et empêchez les chargements supérieurs à ceux attendus. &dagger;
* Lorsque les fichiers ne doivent pas être remplacés par un fichier téléchargé portant le même nom, vérifiez le nom du fichier par rapport à la base de données ou au stockage physique avant de charger le fichier.
* **Exécutez un programme de détection de virus et de logiciels malveillants sur le contenu chargé avant que le fichier ne soit stocké.**

l’exemple d’application &dagger;The illustre une approche qui répond aux critères.

> [!WARNING]
> Le chargement d’un code malveillant sur un système est généralement la première étape de l’exécution de code capable de :
>
> * Maîtrisez complètement un système.
> * Surchargez un système avec le résultat du blocage du système.
> * Compromettre les données utilisateur ou système.
> * Appliquez Graffiti à une interface utilisateur publique.
>
> Pour plus d’informations sur la réduction de la surface d’attaque quand vous acceptez des fichiers d’utilisateurs, consultez les ressources suivantes :
>
> * [Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Chargement de fichiers illimité)
> * Sécurité de la @no__t 0Azure : Vérifiez que les contrôles appropriés sont en place lors de l’acceptation des fichiers des utilisateurs @ no__t-0

Pour plus d’informations sur l’implémentation de mesures de sécurité, y compris des exemples de l’exemple d’application, consultez la section [validation](#validation) .

## <a name="storage-scenarios"></a>Scénarios de stockage

Les options de stockage courantes pour les fichiers sont les suivantes :

* Base de données

  * Pour les petits chargements de fichiers, une base de données est souvent plus rapide que les options de stockage physique (système de fichiers ou partage réseau).
  * Une base de données est souvent plus pratique que les options de stockage physique, car la récupération d’un enregistrement de base de données pour les données utilisateur peut fournir simultanément le contenu du fichier (par exemple, une image de l’avatar).
  * Une base de données est potentiellement moins coûteuse que l’utilisation d’un service de stockage de données.

* Stockage physique (système de fichiers ou partage réseau)

  * Pour les chargements de fichiers volumineux :
    * Les limites de base de données peuvent limiter la taille du téléchargement.
    * Le stockage physique est souvent moins économique que le stockage dans une base de données.
  * Le stockage physique est potentiellement moins onéreux que l’utilisation d’un service de stockage de données.
  * Le processus de l’application doit disposer d’autorisations en lecture et en écriture sur l’emplacement de stockage. **N’accordez jamais l’autorisation EXECUTE.**

* Service de stockage de données (par exemple, [stockage d’objets BLOB Azure](https://azure.microsoft.com/services/storage/blobs/))

  * Les services offrent généralement une évolutivité et une résilience améliorées par rapport aux solutions locales qui sont généralement sujettes à des points de défaillance uniques.
  * Les services sont potentiellement moins coûteux dans les scénarios d’infrastructure de stockage volumineux.

  Pour plus d’informations, consultez [Démarrage rapide : Utilisez .NET pour créer un objet BLOB dans le stockage d’objets @ no__t-0. La rubrique montre <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, mais <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> peut être utilisé pour enregistrer un <xref:System.IO.FileStream> dans le stockage BLOB lors de l’utilisation d’un <xref:System.IO.Stream>.

## <a name="file-upload-scenarios"></a>Scénarios de chargement de fichiers

La mise en mémoire tampon et la diffusion en continu sont deux approches générales pour le téléchargement de fichiers.

**Mise en tampon**

La totalité du fichier est lue dans un <xref:Microsoft.AspNetCore.Http.IFormFile>, qui est C# une représentation du fichier utilisé pour traiter ou enregistrer le fichier.

Les ressources (disque, mémoire) utilisées par les chargements de fichiers dépendent du nombre et de la taille des chargements de fichiers simultanés. Si une application tente de mettre en mémoire tampon un trop grand nombre de chargements, le site se bloque lorsqu’il manque de mémoire ou d’espace disque. Si la taille ou la fréquence des chargements de fichiers épuisent les ressources d’application, utilisez la diffusion en continu.

> [!NOTE]
> Tout fichier mis en mémoire tampon dépassant 64 Ko est déplacé de la mémoire vers un fichier temporaire sur le disque.

La mise en mémoire tampon de petits fichiers est traitée dans les sections suivantes de cette rubrique :

* [Stockage physique](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [Sauvegarde de la base de données](#upload-small-files-with-buffered-model-binding-to-a-database)

**Streaming**

Le fichier est reçu à partir d’une demande en plusieurs parties et est directement traité ou enregistré par l’application. La diffusion en continu n’améliore pas les performances de manière significative. La diffusion en continu réduit les demandes de mémoire ou d’espace disque lors du chargement de fichiers.

La diffusion en continu de fichiers volumineux est traitée dans la section [chargement de fichiers volumineux avec diffusion](#upload-large-files-with-streaming) .

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a>Charger des fichiers de petite taille avec une liaison de modèle mise en mémoire tampon vers un stockage physique

Pour télécharger des fichiers de petite taille, utilisez un formulaire en plusieurs parties ou créez une requête de publication à l’aide de JavaScript.

L’exemple suivant illustre l’utilisation d’un formulaire Razor Pages pour télécharger un seul fichier (*pages/BufferedSingleFileUploadPhysical. cshtml* dans l’exemple d’application) :

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
            <span asp-validation-for="FileUpload.FormFile"></span>
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload" />
</form>
```

L’exemple suivant est similaire à l’exemple précédent, à l’exception de :

* JavaScript ([API FETCH](https://developer.mozilla.org/docs/Web/API/Fetch_API)) est utilisé pour envoyer les données du formulaire.
* Il n’y a aucune validation.

```cshtml
<form action="BufferedSingleFileUploadPhysical/?handler=Upload" 
      enctype="multipart/form-data" onsubmit="AJAXSubmit(this);return false;" 
      method="post">
    <dl>
        <dt>
            <label for="FileUpload_FormFile">File</label>
        </dt>
        <dd>
            <input id="FileUpload_FormFile" type="file" 
                name="FileUpload.FormFile" />
        </dd>
    </dl>

    <input class="btn" type="submit" value="Upload" />

    <div style="margin-top:15px">
        <output name="result"></output>
    </div>
</form>

<script>
  async function AJAXSubmit (oFormElement) {
    var resultElement = oFormElement.elements.namedItem("result");
    const formData = new FormData(oFormElement);

    try {
    const response = await fetch(oFormElement.action, {
      method: 'POST',
      body: formData
    });

    if (response.ok) {
      window.location.href = '/';
    }

    resultElement.value = 'Result: ' + response.status + ' ' + 
      response.statusText;
    } catch (error) {
      console.error('Error:', error);
    }
  }
</script>
```

Pour exécuter la publication de formulaire dans JavaScript pour les clients qui [ne prennent pas en charge l’API FETCH](https://caniuse.com/#feat=fetch), utilisez l’une des approches suivantes :

* Utilisez un Polyfill d’extraction (par exemple, [Window. Fetch Polyfill (GitHub/fetch)](https://github.com/github/fetch)).
* Utilisez `XMLHttpRequest`. Exemple :

  ```javascript
  <script>
    "use strict";

    function AJAXSubmit (oFormElement) {
      var oReq = new XMLHttpRequest();
      oReq.onload = function(e) { 
      oFormElement.elements.namedItem("result").value = 
        'Result: ' + this.status + ' ' + this.statusText;
      };
      oReq.open("post", oFormElement.action);
      oReq.send(new FormData(oFormElement));
    }
  </script>
  ```

Afin de prendre en charge les chargements de fichiers, les formulaires HTML doivent spécifier un type d’encodage (`enctype`) de `multipart/form-data`.

Pour qu’un élément d’entrée `files` prenne en charge le téléchargement de plusieurs fichiers, fournissez l’attribut `multiple` sur l’élément `<input>` :

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

Les fichiers individuels téléchargés vers le serveur sont accessibles via la [liaison de modèle](xref:mvc/models/model-binding) à l’aide de <xref:Microsoft.AspNetCore.Http.IFormFile>. L’exemple d’application illustre plusieurs chargements de fichiers mis en mémoire tampon pour les scénarios de base de données et de stockage physique.

> [!WARNING]
> N’utilisez pas ou n’approuvez pas la propriété `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> sans validation. La propriété `FileName` doit uniquement être utilisée à des fins d’affichage et uniquement après l’encodage HTML de la valeur.
>
> Les exemples fournis jusqu’à présent ne prennent pas en compte les considérations de sécurité. Des informations supplémentaires sont fournies par les sections suivantes et l' [exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):
>
> * [Considérations relatives à la sécurité](#security-considerations)
> * [Validation](#validation)

Lors du chargement de fichiers à l’aide de la liaison de modèle et de <xref:Microsoft.AspNetCore.Http.IFormFile>, la méthode d’action peut accepter :

* Une seule <xref:Microsoft.AspNetCore.Http.IFormFile>.
* L’un des regroupements suivants qui représentent plusieurs fichiers :
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * [Liste](xref:System.Collections.Generic.List`1)\< @ no__t-2 @ no__t-3

> [!NOTE]
> La liaison correspond aux fichiers de formulaire par nom. Par exemple, la valeur HTML `name` dans `<input type="file" name="formFile">` doit correspondre au C# paramètre/à la propriété lié (`FormFile`). Pour plus d’informations, consultez la section valeur de l' [attribut de nom de correspondance pour le nom de paramètre de la méthode de publication](#match-name-attribute-value-to-parameter-name-of-post-method) .

L’exemple suivant :

* Effectue une boucle sur un ou plusieurs fichiers téléchargés.
* Utilise [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) pour retourner le chemin d’accès complet d’un fichier, y compris le nom de fichier. 
* Enregistre les fichiers dans le système de fichiers local à l’aide d’un nom de fichier généré par l’application.
* Retourne le nombre total et la taille des fichiers téléchargés.

```csharp
public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> files)
{
    long size = files.Sum(f => f.Length);

    foreach (var formFile in files)
    {
        if (formFile.Length > 0)
        {
            var filePath = Path.GetTempFileName();

            using (var stream = System.IO.File.Create(filePath))
            {
                await formFile.CopyToAsync(stream);
            }
        }
    }

    // Process uploaded files
    // Don't rely on or trust the FileName property without validation.

    return Ok(new { count = files.Count, size, filePath });
}
```

Utilisez `Path.GetRandomFileName` pour générer un nom de fichier sans chemin d’accès. Dans l’exemple suivant, le chemin d’accès est obtenu à partir de la configuration :

```csharp
foreach (var formFile in files)
{
    if (formFile.Length > 0)
    {
        var filePath = Path.Combine(_config["StoredFilesPath"], 
            Path.GetRandomFileName());

        using (var stream = System.IO.File.Create(filePath))
        {
            await formFile.CopyToAsync(stream);
        }
    }
}
```

Le chemin d’accès passé à la <xref:System.IO.FileStream> *doit* inclure le nom de fichier. Si le nom de fichier n’est pas fourni, un <xref:System.UnauthorizedAccessException> est levé au moment de l’exécution.

Les fichiers chargés à l’aide de la technique <xref:Microsoft.AspNetCore.Http.IFormFile> sont mis en mémoire tampon ou sur disque sur le serveur avant le traitement. À l’intérieur de la méthode d’action, le contenu <xref:Microsoft.AspNetCore.Http.IFormFile> est accessible en tant que <xref:System.IO.Stream>. Outre le système de fichiers local, les fichiers peuvent être enregistrés sur un partage réseau ou un service de stockage de fichiers, tel que le [stockage d’objets BLOB Azure](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).

Pour obtenir un autre exemple qui effectue une boucle sur plusieurs fichiers pour le téléchargement et utilise des noms de fichiers sécurisés, consultez *pages/BufferedMultipleFileUploadPhysical. cshtml. cs* dans l’exemple d’application.

> [!WARNING]
> [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) lève une <xref:System.IO.IOException> si plus de 65 535 fichiers sont créés sans supprimer les fichiers temporaires précédents. La limite de 65 535 fichiers est une limite par serveur. Pour plus d’informations sur cette limite sur le système d’exploitation Windows, consultez les notes dans les rubriques suivantes :
>
> * [GetTempFileNameA fonction)](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a>Charger des fichiers de petite taille avec une liaison de modèle mise en mémoire tampon vers une base de données

Pour stocker des données de fichier binaires dans une base de données à l’aide de [Entity Framework](/ef/core/index), définissez une propriété de tableau <xref:System.Byte> sur l’entité :

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

Spécifiez une propriété de modèle de page pour la classe qui comprend un <xref:Microsoft.AspNetCore.Http.IFormFile> :

```csharp
public class BufferedSingleFileUploadDbModel : PageModel
{
    ...

    [BindProperty]
    public BufferedSingleFileUploadDb FileUpload { get; set; }

    ...
}

public class BufferedSingleFileUploadDb
{
    [Required]
    [Display(Name="File")]
    public IFormFile FormFile { get; set; }
}
```

> [!NOTE]
> <xref:Microsoft.AspNetCore.Http.IFormFile> peut être utilisé directement comme paramètre de méthode d’action ou comme propriété de modèle liée. L’exemple précédent utilise une propriété de modèle liée.

La `FileUpload` est utilisée dans le formulaire de Razor Pages :

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload">
</form>
```

Lorsque le formulaire est publié sur le serveur, copiez le <xref:Microsoft.AspNetCore.Http.IFormFile> dans un flux et enregistrez-le en tant que tableau d’octets dans la base de données. Dans l’exemple suivant, `_dbContext` stocke le contexte de base de données de l’application :

```csharp
public async Task<IActionResult> OnPostUploadAsync()
{
    using (var memoryStream = new MemoryStream())
    {
        await FileUpload.FormFile.CopyToAsync(memoryStream);

        // Upload the file if less than 2 MB
        if (memoryStream.Length < 2097152)
        {
            var file = new AppFile()
            {
                Content = memoryStream.ToArray()
            };

            _dbContext.File.Add(file);

            await _dbContext.SaveChangesAsync();
        }
        else
        {
            ModelState.AddModelError("File", "The file is too large.");
        }
    }

    return Page();
}
```

L’exemple précédent est semblable à un scénario illustré dans l’exemple d’application :

* *Pages/BufferedSingleFileUploadDb. cshtml*
* *Pages/BufferedSingleFileUploadDb. cshtml. cs*

> [!WARNING]
> Soyez prudent quand vous stockez des données binaires dans des bases de données relationnelles, car cela peut avoir un impact négatif sur les performances.
>
> N’utilisez pas ou n’approuvez pas la propriété `FileName` de <xref:Microsoft.AspNetCore.Http.IFormFile> sans validation. La propriété `FileName` doit uniquement être utilisée à des fins d’affichage et uniquement après l’encodage HTML.
>
> Les exemples fournis ne prennent pas en compte les considérations de sécurité. Des informations supplémentaires sont fournies par les sections suivantes et l' [exemple d’application](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):
>
> * [Considérations relatives à la sécurité](#security-considerations)
> * [Validation](#validation)

### <a name="upload-large-files-with-streaming"></a>Charger des fichiers volumineux avec streaming

L’exemple suivant montre comment utiliser JavaScript pour diffuser un fichier vers une action de contrôleur. Le jeton anti-contrefaçon du fichier est généré à l’aide d’un attribut de filtre personnalisé et transmis aux en-têtes HTTP du client plutôt que dans le corps de la demande. Étant donné que la méthode d’action traite directement les données chargées, la liaison de modèle de formulaire est désactivée par un autre filtre personnalisé. Dans l’action, le contenu du formulaire est lu en avec `MultipartReader`, qui lit chaque `MultipartSection` individuelle, traitant le fichier ou enregistrant le contenu selon ce qui est approprié. Une fois les sections en plusieurs parties lues, l’action effectue sa propre liaison de modèle.

La réponse de page initiale charge le formulaire et enregistre un jeton anti-contrefaçon dans un cookie (via l’attribut `GenerateAntiforgeryTokenCookieAttribute`). L’attribut utilise la [prise en charge de l’anticontrefaçon](xref:security/anti-request-forgery) intégrée de ASP.net Core pour définir un cookie avec un jeton de demande :

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

Le `DisableFormValueModelBindingAttribute` est utilisé pour désactiver la liaison de modèle :

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

Dans l’exemple d’application, `GenerateAntiforgeryTokenCookieAttribute` et `DisableFormValueModelBindingAttribute` sont appliqués en tant que filtres aux modèles d’application de page `/StreamedSingleFileUploadDb` et `/StreamedSingleFileUploadPhysical` dans `Startup.ConfigureServices` à l’aide des [conventions de Razor pages](xref:razor-pages/razor-pages-conventions):

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Startup.cs?name=snippet_AddMvc&highlight=8-11,17-20)]

Étant donné que la liaison de modèle ne lit pas le formulaire, les paramètres qui sont liés depuis le formulaire ne sont pas liés (la requête, l’itinéraire et l’en-tête continuent de fonctionner). La méthode d’action fonctionne directement avec la propriété `Request`. Un `MultipartReader` est utilisé pour lire chaque section. Les données de clé/valeur sont stockées dans un `KeyValueAccumulator`. Une fois les sections en plusieurs parties lues, le contenu du `KeyValueAccumulator` est utilisé pour lier les données de formulaire à un type de modèle.

La méthode `StreamingController.UploadDatabase` complète pour la diffusion en continu vers une base de données avec EF Core :

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

`MultipartRequestHelper` (*Utilities/MultipartRequestHelper. cs*) :

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

La méthode `StreamingController.UploadPhysical` complète pour la diffusion en continu vers un emplacement physique :

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

Dans l’exemple d’application, les contrôles de validation sont gérés par `FileHelpers.ProcessStreamedFile`.

## <a name="validation"></a>Validation

La classe `FileHelpers` de l’exemple d’application illustre plusieurs vérifications pour les chargements de fichiers mis en mémoire tampon <xref:Microsoft.AspNetCore.Http.IFormFile> et en continu. Pour le traitement des chargements de fichiers mis en mémoire tampon <xref:Microsoft.AspNetCore.Http.IFormFile> dans l’exemple d’application, consultez la méthode `ProcessFormFile` dans le fichier *Utilities/FileHelpers. cs* . Pour le traitement de fichiers en continu, consultez la méthode `ProcessStreamedFile` dans le même fichier.

> [!WARNING]
> Les méthodes de traitement de validation présentées dans l’exemple d’application n’analysent pas le contenu des fichiers téléchargés. Dans la plupart des scénarios de production, une API de détection de virus et de logiciels malveillants est utilisée sur le fichier avant que le fichier soit mis à la disposition des utilisateurs ou d’autres systèmes.
>
> Bien que l’exemple de rubrique fournit un exemple fonctionnel de techniques de validation, n’implémentez pas la classe `FileHelpers` dans une application de production, sauf si vous :
>
> * Comprenez pleinement l’implémentation.
> * Modifiez l’implémentation en fonction de l’environnement et des spécifications de l’application.
>
> **N’implémentez jamais non plus de code de sécurité dans une application sans répondre à ces exigences.**

### <a name="content-validation"></a>Validation du contenu

**Utilisez une API d’analyse de virus/programme malveillant sur le contenu chargé.**

L’analyse des fichiers exige des ressources serveur dans des scénarios de volume élevé. Si les performances de traitement des demandes sont réduites en raison de l’analyse de fichiers, envisagez de décharger le travail d’analyse vers un [service en arrière-plan](xref:fundamentals/host/hosted-services), éventuellement un service qui s’exécute sur un serveur différent du serveur de l’application. En règle générale, les fichiers téléchargés sont conservés dans une zone en quarantaine jusqu’à ce que l’antivirus en arrière-plan les vérifie. Lorsqu’un fichier réussit, le fichier est déplacé vers l’emplacement de stockage de fichiers normal. Ces étapes sont généralement effectuées conjointement avec un enregistrement de base de données qui indique l’état d’analyse d’un fichier. À l’aide de cette approche, l’application et le serveur d’applications restent concentrés sur la réponse aux requêtes.

### <a name="file-extension-validation"></a>Validation de l’extension de fichier

L’extension du fichier chargé doit être vérifiée par rapport à une liste d’extensions autorisées. Exemple :

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a>Validation de la signature de fichier

La signature d’un fichier est déterminée par les premiers octets au début d’un fichier. Ces octets peuvent être utilisés pour indiquer si l’extension correspond au contenu du fichier. L’exemple d’application vérifie les signatures de fichiers pour quelques types de fichiers courants. Dans l’exemple suivant, la signature de fichier pour une image JPEG est vérifiée par rapport au fichier :

```csharp
private static readonly Dictionary<string, List<byte[]>> _fileSignature = 
    new Dictionary<string, List<byte[]>>
{
    { ".jpeg", new List<byte[]>
        {
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE2 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE3 },
        }
    },
};

using (var reader = new BinaryReader(uploadedFileData))
{
    var signatures = _fileSignature[ext];
    var headerBytes = reader.ReadBytes(signatures.Max(m => m.Length));
    
    return signatures.Any(signature => 
        headerBytes.Take(signature.Length).SequenceEqual(signature));
}
```

Pour obtenir des signatures de fichiers supplémentaires, consultez la [base de données signatures de fichiers](https://www.filesignatures.net/) et les spécifications de fichier officielles.

### <a name="file-name-security"></a>Sécurité des noms de fichiers

N’utilisez jamais un nom de fichier fourni par le client pour enregistrer un fichier dans le stockage physique. Créez un nom de fichier sécurisé pour le fichier à l’aide de [Path. GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) ou [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) pour créer un chemin d’accès complet (y compris le nom de fichier) pour le stockage temporaire.

Razor code automatiquement les valeurs de propriété pour l’affichage. Le code suivant peut être utilisé en toute sécurité :

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

En dehors de Razor, toujours <xref:System.Net.WebUtility.HtmlEncode*> le contenu du nom de fichier à partir de la demande d’un utilisateur.

De nombreuses implémentations doivent inclure une vérification de l’existence du fichier ; dans le cas contraire, le fichier est remplacé par un fichier du même nom. Fournissez une logique supplémentaire pour répondre aux spécifications de votre application.

### <a name="size-validation"></a>Validation de la taille

Limitez la taille des fichiers téléchargés.

Dans l’exemple d’application, la taille du fichier est limitée à 2 Mo (indiquée en octets). La limite est fournie via la [configuration](xref:fundamentals/configuration/index) à partir du fichier *appSettings. JSON* :

```json
{
  "FileSizeLimit": 2097152
}
```

La `FileSizeLimit` est injectée dans les classes `PageModel` :

```csharp
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    private readonly long _fileSizeLimit;

    public BufferedSingleFileUploadPhysicalModel(IConfiguration config)
    {
        _fileSizeLimit = config.GetValue<long>("FileSizeLimit");
    }

    ...
}
```

Quand une taille de fichier dépasse la limite, le fichier est rejeté :

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a>Valeur de l’attribut de nom de correspondance avec le nom de paramètre de la méthode de publication

Dans les formulaires non-Razor qui PUBLIEnt des données de formulaire ou utilisent directement le `FormData` de JavaScript, le nom spécifié dans l’élément du formulaire ou `FormData` doit correspondre au nom du paramètre dans l’action du contrôleur.

Dans l’exemple suivant :

* Lors de l’utilisation d’un élément `<input>`, l’attribut `name` est défini sur la valeur `battlePlans` :

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* Lors de l’utilisation de `FormData` dans JavaScript, le nom est défini sur la valeur `battlePlans` :

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

Utilisez un nom correspondant pour le paramètre de la C# méthode (`battlePlans`) :

* Pour une méthode de gestionnaire de page Razor Pages nommée `Upload` :

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* Pour une méthode d’action du billet de contrôleur MVC :

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a>Configuration du serveur et de l’application

### <a name="multipart-body-length-limit"></a>Longueur limite du corps en plusieurs parties

<xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> définit la limite de la longueur de chaque corps en plusieurs parties. Les sections de formulaire qui dépassent cette limite lèvent une <xref:System.IO.InvalidDataException> lorsqu’elles sont analysées. La valeur par défaut est 134 217 728 (128 Mo). Personnaliser la limite à l’aide du paramètre <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> dans `Startup.ConfigureServices` :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<FormOptions>(options =>
    {
        // Set the limit to 256 MB
        options.MultipartBodyLengthLimit = 268435456;
    });
}
```

<xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> est utilisé pour définir la <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> pour une seule page ou action.

Dans une application Razor Pages, appliquez le filtre avec une [Convention](xref:razor-pages/razor-pages-conventions) dans `Startup.ConfigureServices` :

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model.Filters.Add(
                    new RequestFormLimitsAttribute()
                    {
                        // Set the limit to 256 MB
                        MultipartBodyLengthLimit = 268435456
                    });
    })
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

Dans une application Razor Pages ou une application MVC, appliquez le filtre au modèle de page ou à la méthode d’action :

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a>Taille maximale du corps de la demande Kestrel

Pour les applications hébergées par Kestrel, la taille de corps de requête maximale par défaut est de 30 millions octets, soit environ 28,6 Mo. Personnaliser la limite à l’aide de l’option de serveur [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel :

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Handle requests up to 50 MB
            options.Limits.MaxRequestBodySize = 52428800;
        });
```

<xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> est utilisé pour définir le [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) pour une seule page ou action.

Dans une application Razor Pages, appliquez le filtre avec une [Convention](xref:razor-pages/razor-pages-conventions) dans `Startup.ConfigureServices` :

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model =>
                {
                    // Handle requests up to 50 MB
                    model.Filters.Add(
                        new RequestSizeLimitAttribute(52428800));
                });
    })
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

Dans une application de pages Razor ou une application MVC, appliquez le filtre à la classe du gestionnaire de page ou à la méthode d’action :

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="other-kestrel-limits"></a>Autres limites Kestrel

D’autres limites Kestrel peuvent s’appliquer aux applications hébergées par Kestrel :

* [Nombre maximal de connexions client](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [Taux de données de demande et de réponse](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a>Limite de longueur du contenu IIS

La limite de demandes par défaut (`maxAllowedContentLength`) est de 30 millions octets, soit environ 28,6 Mo. Personnaliser la limite dans le fichier *Web. config* :

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- Handle requests up to 50 MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

Ce paramètre s’applique seulement à IIS. Par défaut, ce comportement ne se produit pas dans le cas d’un hébergement sur Kestrel. Pour plus d’informations, consultez [limites de demande \<requestLimits >](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).

Les limitations du module ASP.NET Core ou la présence du module de filtrage des demandes IIS peuvent limiter les chargements à 2 ou 4 Go. Pour plus d’informations, consultez [Impossible de télécharger un fichier d’une taille supérieure à 2 Go (ASPNET/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).

## <a name="troubleshoot"></a>Résolution des problèmes

Voici certains problèmes courants rencontrés avec le chargement de fichiers et leurs solutions possibles.

### <a name="not-found-error-when-deployed-to-an-iis-server"></a>Erreur introuvable lors du déploiement sur un serveur IIS

L’erreur suivante indique que le fichier chargé dépasse la longueur du contenu configurée du serveur :

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

Pour plus d’informations sur l’amélioration de la limite, consultez la section [limite de longueur du contenu IIS](#iis-content-length-limit) .

### <a name="connection-failure"></a>Échec de la connexion

Une erreur de connexion et une connexion du serveur de réinitialisation indiquent probablement que le fichier téléchargé dépasse la taille maximale du corps de la demande de Kestrel. Pour plus d’informations, consultez la section [taille maximale du corps de la demande Kestrel](#kestrel-maximum-request-body-size) . Les limites de connexion du client Kestrel peuvent également nécessiter des ajustements.

### <a name="null-reference-exception-with-iformfile"></a>Exception de référence null avec IFormFile

Si le contrôleur accepte les fichiers téléchargés à l’aide de <xref:Microsoft.AspNetCore.Http.IFormFile>, mais que la valeur est `null`, vérifiez que le formulaire HTML spécifie une valeur `enctype` de `multipart/form-data`. Si cet attribut n’est pas défini sur l’élément `<form>`, le chargement du fichier ne se produit pas et tous les arguments <xref:Microsoft.AspNetCore.Http.IFormFile> liés sont `null`. Vérifiez également que l' [appellation de chargement dans les données de formulaire correspond à celle de l’application](#match-name-attribute-value-to-parameter-name-of-post-method).

::: moniker-end


## <a name="additional-resources"></a>Ressources supplémentaires

* [Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Chargement de fichiers illimité)
* Sécurité de la @no__t 0Azure : Cadre de sécurité : Validation des entrées | Atténuations @ no__t-0
* Modèles de conception de Cloud @no__t 0Azure : Modèle de clé valet @ no__t-0
