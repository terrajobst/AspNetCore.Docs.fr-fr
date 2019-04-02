---
title: Mettre en forme les données des réponses dans l’API web ASP.NET Core
author: ardalis
description: Découvrez comment mettre en forme les données des réponses dans l’API web ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
uid: web-api/advanced/formatting
ms.openlocfilehash: b0fce0632fd2d885cb8e9a056923ec365d2f327d
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209984"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a>Mettre en forme les données des réponses dans l’API web ASP.NET Core

Par [Steve Smith](https://ardalis.com/)

ASP.NET Core MVC offre une prise en charge intégrée de la mise en forme des données des réponses, selon un format fixe ou en réponse à des spécifications du client.

[Affichez ou téléchargez l’exemple de code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([procédure de téléchargement](xref:index#how-to-download-a-sample))

## <a name="format-specific-action-results"></a>Résultats d’une action spécifique à un format

Certains types de résultats d’action sont spécifiques à un format particulier, comme `JsonResult` et `ContentResult`. Les actions peuvent retourner des résultats spécifiques qui sont toujours mis en forme d’une manière particulière. Par exemple, retourner un `JsonResult` retourne des données au format JSON, indépendamment des préférences du client. De même, retourner un `ContentResult` retourne des données de chaîne au format texte brut (tout comme retourner une chaîne).

> [!NOTE]
> Une action ne doit pas nécessairement retourner un type particulier ; MVC prend en charge n’importe quelle valeur de retour d’un objet. Si une action retourne une implémentation de `IActionResult` et que le contrôleur hérite de `Controller`, les développeurs disposent de nombreuses méthodes helper correspondant à de nombreux choix différents. Les résultats provenant d’actions qui retournent des objets qui ne sont pas des types `IActionResult` sont sérialisés en utilisant l’implémentation appropriée de `IOutputFormatter`.

Pour retourner des données dans un format spécifique depuis un contrôleur qui hérite de la classe de base `Controller`, utilisez la méthode helper intégrée `Json` pour retourner du JSON et `Content` pour du texte brut. Votre méthode d’action doit retourner le type de résultat spécifique (par exemple `JsonResult`) ou `IActionResult`.

Retour de données au format JSON :

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

Exemple de réponse de cette action :

![Onglet Réseau des Outils de développement dans Microsoft Edge montrant que le type de contenu de la réponse est application/json](formatting/_static/json-response.png)

Notez que le type de contenu de la réponse est `application/json`, qui apparaît à la fois dans la liste des requêtes du réseau et dans la section En-têtes de réponse. Notez également la liste des options présentées par le navigateur (dans ce cas, Microsoft Edge) dans l’en-tête Accept de la section des en-têtes de la requête. La technique actuelle ignore cet en-tête ; sa prise en compte est présentée ci-dessous.

Pour retourner des données mises en forme en texte brut, utilisez `ContentResult` et le helper `Content` :

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

Une réponse de cette action :

![Onglet Réseau des Outils de développement dans Microsoft Edge montrant que le type de contenu de la réponse est text/plain](formatting/_static/text-response.png)

Notez que dans ce cas, le `Content-Type` retourné est `text/plain`. Vous pouvez également obtenir ce comportement en utilisant simplement un type de réponse chaîne :

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> Pour les actions plus complexes avec plusieurs types ou options (par exemple différents codes d’état HTTP en fonction du résultat des opérations effectuées), préférez le type de retour `IActionResult`.

## <a name="content-negotiation"></a>Négociation de contenu

La négociation de contenu (*conneg*, abréviation de « Content negotiation ») se produit quand le client spécifie un [en-tête Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html). Le format par défaut utilisé par ASP.NET Core MVC est JSON. La négociation de contenu est implémentée par `ObjectResult`. Elle est également intégrée dans les résultats d’une action spécifique au code d’état retournés depuis les méthodes helper (qui sont toutes basées sur `ObjectResult`). Vous pouvez aussi retourner un type de modèle (une classe que vous avez définie comme étant le type de transfert de vos données), que le framework encapsule automatiquement dans un `ObjectResult` pour vous.

La méthode d’action suivante utilise les méthodes helper `Ok` et `NotFound` :

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

Une réponse au format JSON est retournée sauf si un autre format a été demandé et que le serveur peut retourner ce format demandé. Vous pouvez utiliser un outil comme [Fiddler](http://www.telerik.com/fiddler) pour créer une requête qui inclut un en-tête Accept et pour spécifier un autre format. Dans ce cas, si le serveur a un *formateur* qui peut produire une réponse dans le format demandé, le résultat est retourné dans le format préféré du client.

![Console Fiddler montrant une requête GET créée manuellement une valeur d’en-tête Accept application/xml](formatting/_static/fiddler-composer.png)

Dans la capture d’écran ci-dessus, Fiddler Composer a été utilisé pour générer une requête, en spécifiant `Accept: application/xml`. Par défaut, ASP.NET Core MVC prend en charge seulement JSON : même si un autre format est spécifié, le résultat retourné est donc toujours au format JSON. Vous verrez comment ajouter d’autres formateurs dans la section suivante.

Les actions du contrôleur peuvent retourner des objets OCT (objets CLR traditionnels), auquel cas ASP.NET Core MVC crée automatiquement pour vous un `ObjectResult` qui encapsule l’objet. Le client reçoit l’objet sérialisé mis en forme (le format JSON est le format par défaut ; vous pouvez configurer un format XML ou d’autres formats). Si l’objet retourné est `null`, le framework retourne une réponse `204 No Content`.

Retour d’un type d’objet :

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

Dans l’exemple, une requête d’alias d’auteur valide reçoit une réponse 200 OK avec les données de l’auteur. Une requête d’alias non valide reçoit une réponse 204 No Content. Les captures d’écran ci-dessous montrent la réponse aux formats XML et JSON.

### <a name="content-negotiation-process"></a>Processus de négociation de contenu

La *négociation* de contenu intervient seulement si un en-tête `Accept` apparaît dans la requête. Quand une requête contient un en-tête Accept, le framework énumère les types de médias dans l’en-tête Accept par ordre de préférence, et tente de trouver un formateur capable de produire une réponse dans un des formats spécifiés par l’en-tête Accept. Si aucun formateur pouvant satisfaire la requête du client n’est trouvé, le framework tente de trouver le premier formateur capable de produire une réponse (sauf si le développeur a configuré l’option sur `MvcOptions` pour retourner à la place « 406 Not Acceptable »). Si la requête spécifie XML, mais que le formateur XML n’a pas été configuré, le formateur JSON est utilisé. Plus généralement, si aucun formateur pouvant fournir le format demandé n’est configuré, le premier formateur qui peut mettre en forme l’objet est utilisé. Si aucun en-tête n’est spécifié, le premier formateur capable de gérer l’objet à retourner est utilisé pour sérialiser la réponse. Dans ce cas, aucune négociation n’est effectuée : le serveur détermine le format à utiliser.

> [!NOTE]
> Si l’en-tête Accept contient `*/*`, l’en-tête est ignoré, sauf si `RespectBrowserAcceptHeader` a la valeur true sur `MvcOptions`.

### <a name="browsers-and-content-negotiation"></a>Navigateurs et négociation de contenu

Contrairement aux clients d’API classiques, les navigateurs web ont tendance à fournir des en-têtes `Accept` dans un grand nombre de formats, incluant des caractères génériques. Par défaut, quand le framework détecte que la requête provient d’un navigateur, il ignore l’en-tête `Accept` et retourne à la place le contenu au format par défaut configuré pour l’application (JSON, sauf si un autre format est configuré). Ceci fournit une expérience plus cohérente lors de l’utilisation de différents navigateurs pour consommer des API.

Si vous préférez que votre application prenne en compte les en-têtes Accept du navigateur, vous pouvez configurer ce comportement dans le cadre de la configuration du modèle MVC en définissant `RespectBrowserAcceptHeader` sur `true` dans la méthode `ConfigureServices`, dans *Startup.cs*.

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a>Configuration des formateurs

Si votre application doit prendre en charge des formats supplémentaires en plus du format par défaut JSON, vous pouvez ajouter des packages NuGet et configurer le modèle MVC pour les prendre en charge. Il existe des formateurs distincts pour les entrées et pour les sorties. Les formateurs d’entrée sont utilisés par la [liaison de modèle](xref:mvc/models/model-binding) ; les formateurs de sortie sont utilisés pour mettre en forme les réponses. Vous pouvez également configurer des [formateurs personnalisés](xref:web-api/advanced/custom-formatters).

### <a name="adding-xml-format-support"></a>Ajout de la prise en charge du format XML

Pour ajouter la prise en charge de la mise en forme XML, vous devez installer le package NuGet `Microsoft.AspNetCore.Mvc.Formatters.Xml`.

Ajoutez XmlSerializerFormatters à la configuration du modèle MVC à *Startup.cs* :

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

Vous pouvez aussi ajouter simplement le formateur de sortie :

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

Ces deux approches sérialisent les résultats avec `System.Xml.Serialization.XmlSerializer`. Si vous préférez, vous pouvez utiliser `System.Runtime.Serialization.DataContractSerializer` en ajoutant son formateur associé :

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

Une fois que vous avez ajouté la prise en charge de la mise en forme XML, vos méthodes de contrôleur doivent retourner le format approprié en fonction de l’en-tête `Accept` de la requête, comme cet exemple Fiddler le montre :

![Console Fiddler : l’onglet Raw pour la requête montre que la valeur de l’en-tête Accept est application/xml. L’onglet Raw pour la réponse montre que la valeur de l’en-tête Content-Type est application/xml.](formatting/_static/xml-response.png)

Vous pouvez voir dans l’onglet Inspectors que la requête GET brute a été faite avec un en-tête `Accept: application/xml`. Le volet de réponse montre l’en-tête `Content-Type: application/xml` et que l’objet `Author` a été sérialisé en XML.

Utilisez l’onglet Composer pour modifier la requête en spécifiant `application/json` dans l’en-tête `Accept`. Exécutez la requête pour constater que la réponse est au format JSON :

![Console Fiddler : l’onglet Raw pour la requête montre que la valeur de l’en-tête Accept est application/json. L’onglet Raw pour la réponse montre que la valeur de l’en-tête Content-Type est application/json.](formatting/_static/json-response-fiddler.png)

Dans cette capture d’écran, vous pouvez voir que la requête définit un en-tête `Accept: application/json` et que la réponse spécifie de même dans son `Content-Type`. L’objet `Author` est montré dans le corps de la réponse, au format JSON.

### <a name="forcing-a-particular-format"></a>Forcer un format particulier

Si vous voulez limiter les formats de réponse pour une action spécifique, vous pouvez appliquer le filtre `[Produces]`. Le filtre `[Produces]` spécifie les formats de réponse pour une action (ou un contrôleur) spécifique. Comme la plupart des [filtres](xref:mvc/controllers/filters), celui-ci peut être appliqué à l’action, au contrôleur ou à l’étendue globale.

```csharp
[Produces("application/json")]
public class AuthorsController
```

Le filtre `[Produces]` force toutes les actions dans `AuthorsController` à retourner des réponses au format JSON, même si d’autres formateurs ont été configurés pour l’application et que le client a fourni un en-tête `Accept` demandant un autre format disponible. Pour plus d’informations, notamment comment appliquer des filtres de façon globale, consultez [Filtres](xref:mvc/controllers/filters).

### <a name="special-case-formatters"></a>Formateurs pour des cas spéciaux

Certains cas spéciaux sont implémentés avec des formateurs intégrés. Par défaut, les types de retour `string` sont formatés en *text/plain* (*text/html* si c’est demandé via l’en-tête `Accept`). Vous pouvez éviter ce comportement en supprimant `TextOutputFormatter`. Vous supprimez des formateurs dans la méthode `Configure` de *Startup.cs* (voir ci-dessous). Les actions qui ont un type de retour d’objet de modèle retournent une réponse « 204 No Content » si `null` est retourné. Vous pouvez éviter ce comportement en supprimant `HttpNoContentOutputFormatter`. Le code suivant supprime `TextOutputFormatter` et `HttpNoContentOutputFormatter`.

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

Par exemple, sans `TextOutputFormatter`, les types de retour `string` retournent « 406 Not Acceptable ». Notez que s’il existe un formateur XML, il met en forme les types de retour `string` si `TextOutputFormatter` est supprimé.

Sans `HttpNoContentOutputFormatter`, les objets null sont mis en forme avec le formateur configuré. Par exemple, le formateur JSON retourne simplement une réponse avec un corps `null`, tandis que le formateur XML retourne un élément XML vide avec l’attribut `xsi:nil="true"`.

## <a name="response-format-url-mappings"></a>Mappages d’URL de format de réponse

Les clients peuvent demander un format particulier dans l’URL, comme dans la chaîne de requête ou une partie du chemin, ou en utilisant une extension de fichier spécifique à un format, comme .xml ou .json. Le mappage du chemin de la requête doit être spécifié dans la route utilisée par l’API. Par exemple :

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

Cette route permet de spécifier le format demandé sous la forme d’une extension de fichier facultative. L’attribut `[FormatFilter]` vérifie l’existence de la valeur du format dans `RouteData` et mappe le format de la réponse au formateur approprié lors de la création de la réponse.

|           Route            |             Formateur              |
|----------------------------|------------------------------------|
|   `/products/GetById/5`    |    Le formateur de sortie par défaut    |
| `/products/GetById/5.json` | Le formateur JSON (s’il est configuré) |
| `/products/GetById/5.xml`  | Le formateur XML (s’il est configuré)  |
