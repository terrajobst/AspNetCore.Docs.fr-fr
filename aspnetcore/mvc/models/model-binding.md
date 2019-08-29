---
title: Liaison de données dans ASP.NET Core
author: rick-anderson
description: Découvrez comment fonctionne la liaison de modèle avec ASP.NET Core, et comment personnaliser son comportement.
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: riande
ms.date: 05/31/2019
uid: mvc/models/model-binding
ms.openlocfilehash: 298e305cf918117ec2d313060a7420a1e721a365
ms.sourcegitcommit: 8835b6777682da6fb3becf9f9121c03f89dc7614
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69975295"
---
# <a name="model-binding-in-aspnet-core"></a>Liaison de données dans ASP.NET Core

Cet article explique ce qu’est la liaison de modèle, comment elle fonctionne et comment personnaliser son comportement.

[Affichez ou téléchargez un exemple de code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([procédure de téléchargement](xref:index#how-to-download-a-sample)).

## <a name="what-is-model-binding"></a>Description de la liaison de modèle

Les contrôleurs et Razor Pages utilisent des données provenant de requêtes HTTP. Par exemple, les données de routage peuvent fournir une clé d’enregistrement, et les champs de formulaire posté peuvent fournir des valeurs pour les propriétés du modèle. L’écriture du code permettant de récupérer chacune de ces valeurs et de les convertir en types .NET à partir de chaînes est fastidieuse et source d’erreurs. La liaison de modèle automatise ce processus. Le système de liaison de modèle :

* Récupère les données de diverses sources telles que les données de routage, les champs de formulaire et les chaînes de requête
* Fournit les données aux contrôleurs et à Razor Pages dans les paramètres de méthode et les propriétés publiques
* Convertit les données de chaîne en types .NET
* Met à jour les propriétés des types complexes

## <a name="example"></a>Exemples

Supposons que vous ayez la méthode d’action suivante :

[!code-csharp[](model-binding/samples/2.x/Controllers/PetsController.cs?name=snippet_DogsOnly)]

Et que l’application reçoive une requête avec l’URL suivante :

```
http://contoso.com/api/pets/2?DogsOnly=true
```

La liaison de modèle suit les étapes ci-après une fois que le système de routage a sélectionné la méthode d’action :

* Elle recherche le premier paramètre de `GetByID`, un entier nommé `id`.
* Elle parcourt les sources disponibles dans la requête HTTP et trouve `id` = « 2 » dans les données de routage.
* Elle convertit la chaîne « 2 » en entier 2.
* Elle recherche le paramètre suivant de `GetByID`, un booléen nommé `dogsOnly`.
* Elle parcourt les sources et trouve « DogsOnly=true » dans la chaîne de requête. La mise en correspondance des noms ne respecte pas la casse.
* Elle convertit la chaîne « true » en booléen `true`.

Le framework appelle ensuite la méthode `GetById`, en passant 2 pour le paramètre `id`, et `true` pour le paramètre `dogsOnly`.

Dans l’exemple précédent, les cibles de liaison de modèle sont des paramètres de méthode qui sont des types simples. Les cibles peuvent être également les propriétés d’un type complexe. Une fois chaque propriété correctement liée, la [validation du modèle](xref:mvc/models/validation) est effectuée pour la propriété concernée. L’enregistrement des données liées au modèle (ainsi que des erreurs de liaison ou de validation) est stocké dans [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) ou [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState). Pour savoir si ce processus a abouti, l’application vérifie l’indicateur [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid).

## <a name="targets"></a>Cibles

La liaison de modèle tente de trouver des valeurs pour les genres de cible suivants :

* Paramètres de la méthode d’action de contrôleur vers laquelle une requête est routée.
* Paramètres de la méthode de gestionnaire Razor Pages vers laquelle une requête est routée. 
* Propriétés publiques d’un contrôleur ou d’une classe `PageModel`, si elles sont spécifiées par des attributs.

### <a name="bindproperty-attribute"></a>Attribut [BindProperty]

Peut être appliqué à une propriété publique d’un contrôleur ou à une classe `PageModel` pour que la liaison de modèle cible cette propriété :

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=7-8)]

### <a name="bindpropertiesattribute"></a>Attribut [BindProperties]

Disponible avec ASP.NET Core 2.1 et les versions ultérieures.  Peut être appliqué à un contrôleur ou à une classe `PageModel` pour indiquer à la liaison de modèle de cibler toutes les propriétés publiques de la classe :

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a>Liaison de modèle pour les requêtes HTTP GET

Par défaut, les propriétés ne sont pas liées pour les requêtes HTTP GET. En règle générale, le paramètre ID d’un enregistrement est tout ce dont vous avez besoin pour une requête GET. L’ID d’enregistrement est utilisé pour rechercher l’élément dans la base de données. Il n’est donc pas nécessaire de lier une propriété qui contient une instance du modèle. Pour les scénarios dans lesquels vous souhaitez que les propriétés soient liées aux données provenant de requêtes GET, affectez à la propriété `SupportsGet` la valeur `true` :

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a>Sources

Par défaut, la liaison de modèle obtient des données sous la forme de paires clé-valeur à partir des sources suivantes dans une requête HTTP :

1. Champs de formulaire 
1. Corps de la requête (pour les [contrôleurs ayant l’attribut [ApiController]](xref:web-api/index#binding-source-parameter-inference).)
1. Données de route
1. Paramètres de la chaîne de requête
1. Fichiers chargés 

Pour chaque paramètre ou propriété cible, les sources sont analysées dans l’ordre indiqué dans cette liste. Il existe quelques exceptions :

* Les données de routage et les valeurs de chaîne de requête sont utilisées uniquement pour les types simples.
* Les fichiers chargés sont liés uniquement aux types cibles qui implémentent `IFormFile` ou `IEnumerable<IFormFile>`.

Si le comportement par défaut ne donne pas les bons résultats, vous pouvez vous servir de l’un des attributs suivants pour spécifier la source à utiliser pour une cible donnée. 

* [[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) - Obtient les valeurs à partir de la chaîne de requête. 
* [[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) - Obtient les valeurs à partir des données de routage.
* [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) - Obtient les valeurs à partir des champs de formulaire postés.
* [[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) - Obtient les valeurs à partir du corps de la requête.
* [[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) - Obtient les valeurs à partir des en-têtes HTTP.

Ces attributs :

* Sont ajoutés aux propriétés du modèle individuellement (et non à la classe de modèle), comme dans l’exemple suivant :

  [!code-csharp[](model-binding/samples/2.x/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* Acceptent éventuellement une valeur de nom de modèle dans le constructeur. Cette option est fournie au cas où le nom de propriété ne correspondrait pas à la valeur de la requête. Par exemple, la valeur de la requête peut être un en-tête avec un trait d’union dans son nom, comme dans l’exemple suivant :

  [!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a>Attribut [FromBody]

Les données du corps de la requête sont analysées à l’aide de formateurs d’entrée spécifiques au type de contenu de la requête. Les formateurs d’entrée sont décrits [plus loin dans cet article](#input-formatters).

N’appliquez pas `[FromBody]` à plus d’un paramètre par méthode d’action. Le runtime ASP.NET Core délègue la responsabilité de la lecture du flux de requête au formateur d’entrée. Une fois le flux de requête lu, il ne peut plus être relu pour lier d’autres paramètres `[FromBody]`.

### <a name="additional-sources"></a>Sources supplémentaires

Les données sources sont fournies au système de liaison de modèle par les *fournisseurs de valeurs*. Vous pouvez écrire et inscrire des fournisseurs de valeurs personnalisés qui obtiennent des données de liaison de modèle à partir d’autres sources. Par exemple, vous pouvez obtenir des données provenant de cookies ou de l’état de session. Pour obtenir des données provenant d’une nouvelle source :

* Créez une classe qui implémente `IValueProvider`.
* Créez une classe qui implémente `IValueProviderFactory`.
* Inscrivez la classe de fabrique dans `Startup.ConfigureServices`.

L’exemple d’application comprend un exemple de [fournisseur de valeurs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProvider.cs) et de [fabrique](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProviderFactory.cs), qui permet de récupérer les valeurs provenant des cookies. Voici le code d’inscription dans `Startup.ConfigureServices` :

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=3)]

Le code affiché place le fournisseur de valeurs personnalisé après tous les fournisseurs de valeurs intégrés.  Pour en faire le premier fournisseur de la liste, appelez `Insert(0, new CookieValueProviderFactory())` à la place de `Add`.

## <a name="no-source-for-a-model-property"></a>Aucune source pour une propriété de modèle

Par défaut, aucune erreur d’état de modèle n’est créée, s’il n’existe aucune valeur de propriété de modèle. La propriété a une valeur null ou une valeur par défaut :

* Les types simples Nullable ont une valeur `null`.
* Les types valeur non Nullable ont la valeur `default(T)`. Par exemple, un paramètre `int id` a la valeur 0.
* Pour les types complexes, la liaison de modèle crée une instance à l’aide du constructeur par défaut, sans définir de propriétés.
* Les tableaux ont la valeur `Array.Empty<T>()`, sauf les tableaux `byte[]` qui ont une valeur `null`.

Si l’état de modèle doit être rendu non valide quand les champs de formulaire d’une propriété de modèle ne contiennent rien, utilisez l’attribut [[BindRequired]](#bindrequired-attribute).

Notez que ce comportement de `[BindRequired]` s’applique à la liaison de modèle des données de formulaire postées, et non aux données JSON ou XML d’un corps de requête. Les données du corps de requête sont prises en charge par les [formateurs d’entrée](#input-formatters).

## <a name="type-conversion-errors"></a>Erreurs de conversion de type

Si une source est localisée mais qu’elle ne peut pas être convertie vers le type cible, l’état du modèle est marqué comme étant non valide. Le paramètre ou la propriété cible a une valeur null ou une valeur par défaut, comme indiqué dans la section précédente.

Dans un contrôleur d’API ayant l’attribut `[ApiController]`, un état de modèle non valide entraîne une réponse HTTP 400 automatique.

Dans une page Razor Pages, réaffichez la page avec un message d’erreur :

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

La validation côté client intercepte la plupart des données incorrectes qui sont envoyées à un formulaire Razor Pages. Cette validation rend difficile le déclenchement du code en surbrillance indiqué plus haut. L’exemple d’application comprend un bouton **Submit with Invalid Date** (Envoyer avec une date non valide), qui place les données incorrectes dans le champ **Hire Date** (Date d’embauche) et envoie le formulaire. Ce bouton montre comment fonctionne le code permettant de réafficher la page quand des erreurs de conversion de données se produisent.

Quand la page est réaffichée par le code précédent, l’entrée non valide n’est pas visible dans le champ de formulaire. En effet, la propriété de modèle à une valeur null ou une valeur par défaut. L’entrée non valide apparaît dans un message d’erreur. Toutefois, si vous souhaitez réafficher les données incorrectes dans le champ de formulaire, transformez la propriété de modèle en chaîne et procédez à la conversion des données manuellement.

La même stratégie est recommandée si vous ne souhaitez pas que les erreurs de conversion de type entraînent des erreurs d’état de modèle. Dans ce cas, transformez la propriété de modèle en chaîne.

## <a name="simple-types"></a>Types simples

Les types simples que le lieur de modèle peut convertir en chaînes sources sont les suivants :

* [Boolean](xref:System.ComponentModel.BooleanConverter)
* [Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)
* [Char](xref:System.ComponentModel.CharConverter)
* [DateTime](xref:System.ComponentModel.DateTimeConverter)
* [DateTimeOffset](xref:System.ComponentModel.DateTimeOffsetConverter)
* [Decimal](xref:System.ComponentModel.DecimalConverter)
* [Double](xref:System.ComponentModel.DoubleConverter)
* [Enum](xref:System.ComponentModel.EnumConverter)
* [Guid](xref:System.ComponentModel.GuidConverter)
* [Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)
* [Single](xref:System.ComponentModel.SingleConverter)
* [TimeSpan](xref:System.ComponentModel.TimeSpanConverter)
* [UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)
* [Uri](xref:System.UriTypeConverter)
* [Version](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a>Types complexes

Un type complexe doit avoir un constructeur public par défaut et des propriétés publiques accessibles en écriture à lier. Quand la liaison de modèle se produit, la classe est instanciée à l’aide du constructeur public par défaut. 

Pour chaque propriété du type complexe, la liaison de modèle recherche dans les sources le modèle de nom *préfixe.nom_propriété*. Si rien n’est trouvé, elle recherche uniquement *nom_propriété* sans le préfixe.

Dans le cas d’une liaison à un paramètre, le préfixe représente le nom du paramètre. Dans le cas d’une liaison à une propriété publique `PageModel`, le préfixe représente le nom de la propriété publique. Certains attributs ont une propriété `Prefix` qui vous permet de remplacer l’utilisation par défaut du nom de paramètre ou de propriété.

Par exemple, supposons que le type complexe corresponde à la classe `Instructor` suivante :

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a>Préfixe = nom de paramètre

Si le modèle à lier est un paramètre nommé `instructorToUpdate` :

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

La liaison de modèle commence par rechercher dans les sources la clé `instructorToUpdate.ID`. Si elle est introuvable, elle recherche `ID` sans préfixe.

### <a name="prefix--property-name"></a>Préfixe = nom de propriété

Si le modèle à lier est une propriété nommée `Instructor` du contrôleur ou de la classe `PageModel` :

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

La liaison de modèle commence par rechercher dans les sources la clé `Instructor.ID`. Si elle est introuvable, elle recherche `ID` sans préfixe.

### <a name="custom-prefix"></a>Préfixe personnalisé

Si le modèle à lier est un paramètre nommé `instructorToUpdate` et si un attribut `Bind` spécifie `Instructor` en tant que préfixe :

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

La liaison de modèle commence par rechercher dans les sources la clé `Instructor.ID`. Si elle est introuvable, elle recherche `ID` sans préfixe.

### <a name="attributes-for-complex-type-targets"></a>Attributs des cibles de type complexe

Plusieurs attributs intégrés sont disponibles pour contrôler la liaison de modèle des types complexes :

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> Ces attributs affectent la liaison de modèle quand les données de formulaire postées représentent la source des valeurs. Ils n’affectent pas les formateurs d’entrée, qui traitent les corps de requête JSON et XML postés. Les formateurs d’entrée sont décrits [plus loin dans cet article](#input-formatters).
>
> Consultez également la discussion sur l’attribut `[Required]` dans [Validation de modèle](xref:mvc/models/validation#required-attribute).

### <a name="bindrequired-attribute"></a>Attribut [BindRequired]

Il s’applique uniquement aux propriétés de modèle, pas aux paramètres de méthode. Il oblige la liaison de modèle à ajouter une erreur d’état de modèle si la liaison est impossible pour la propriété d’un modèle. Voici un exemple :

[!code-csharp[](model-binding/samples/2.x/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a>Attribut [BindNever]

Il s’applique uniquement aux propriétés de modèle, pas aux paramètres de méthode. Il empêche la liaison de modèle de définir la propriété d’un modèle. Voici un exemple :

[!code-csharp[](model-binding/samples/2.x/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a>Attribut [Bind]

Il peut être appliqué à une classe ou à un paramètre de méthode. Il spécifie les propriétés d’un modèle à inclure dans la liaison de modèle.

Dans l’exemple suivant, seules les propriétés spécifiées du modèle `Instructor` sont liées quand une méthode de gestionnaire ou une méthode d’action est appelée :

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

Dans l’exemple suivant, seules les propriétés spécifiées du modèle `Instructor` sont liées quand la méthode `OnPost` est appelée :

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

Vous pouvez utiliser l’attribut `[Bind]` pour éviter le surpostage dans les scénarios de *création*. Il ne fonctionne pas bien dans les scénarios de modification, car les propriétés exclues ont une valeur null ou une valeur par défaut au lieu de rester inchangées. Pour empêcher le surpostage, il est recommandé d’utiliser des modèles de vues à la place de l’attribut `[Bind]`. Pour plus d’informations, consultez [Remarque sur la sécurité concernant le surpostage](xref:data/ef-mvc/crud#security-note-about-overposting).

## <a name="collections"></a>Collections

Pour les cibles qui sont des collections de types simples, la liaison de modèle recherche les correspondances avec *nom_paramètre* ou *nom_propriété*. Si aucune correspondance n’est localisée, elle recherche l’un des formats pris en charge sans le préfixe. Par exemple :

* Supposons que le paramètre à lier soit un tableau nommé `selectedCourses` :

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* Les données de formulaire ou de chaîne de requête peuvent avoir l’un des formats suivants :
   
  ```
  selectedCourses=1050&selectedCourses=2000 
  ```

  ```
  selectedCourses[0]=1050&selectedCourses[1]=2000
  ```

  ```
  [0]=1050&[1]=2000
  ```

  ```
  selectedCourses[a]=1050&selectedCourses[b]=2000&selectedCourses.index=a&selectedCourses.index=b
  ```

  ```
  [a]=1050&[b]=2000&index=a&index=b
  ```

* Le format suivant est pris en charge uniquement dans les données de formulaire :

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* Pour tous les exemples de formats précédents, la liaison de modèle passe un tableau de deux éléments au paramètre `selectedCourses` :

  * selectedCourses[0]=1050
  * selectedCourses[1]=2000

  Les formats de données qui utilisent des nombres en indice (... [0] ... [1] ...) doivent être impérativement numérotés de manière séquentielle à partir de zéro. S’il existe des vides dans la numérotation en indice, tous les éléments suivants sont ignorés. Par exemple, si les indices sont 0 et 2 au lieu de 0 et 1, le second élément est ignoré.

## <a name="dictionaries"></a>Dictionnaires

Pour les cibles `Dictionary`, la liaison de modèle recherche les correspondances avec *nom_paramètre* ou *nom_propriété*. Si aucune correspondance n’est localisée, elle recherche l’un des formats pris en charge sans le préfixe. Par exemple :

* Supposons que le paramètre cible soit un `Dictionary<int, string>` nommé `selectedCourses` :

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* Les données de chaîne de requête ou de formulaire posté peuvent ressembler à l’un des exemples suivants :

  ```
  selectedCourses[1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  [1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  selectedCourses[0].Key=1050&selectedCourses[0].Value=Chemistry&
  selectedCourses[1].Key=2000&selectedCourses[1].Value=Economics
  ```

  ```
  [0].Key=1050&[0].Value=Chemistry&[1].Key=2000&[1].Value=Economics
  ```

* Pour tous les exemples de formats précédents, la liaison de modèle passe un dictionnaire de deux éléments au paramètre `selectedCourses` :

  * selectedCourses["1050"]="Chemistry"
  * selectedCourses["2000"]="Economics"

## <a name="special-data-types"></a>Types de données spéciaux

Certains types de données spéciaux peuvent être pris en charge par la liaison de modèle.

### <a name="iformfile-and-iformfilecollection"></a>IFormFile et IFormFileCollection

Fichier chargé inclus dans la requête HTTP.  `IEnumerable<IFormFile>` est également pris en charge pour plusieurs fichiers.

### <a name="cancellationtoken"></a>CancellationToken

utilisé pour annuler l’activité dans les contrôleurs asynchrones.

### <a name="formcollection"></a>FormCollection

Permet de récupérer toutes les valeurs des données de formulaire posté.

## <a name="input-formatters"></a>Formateurs d’entrée

Les données contenues dans le corps de la requête peuvent être au format JSON, XML ou tout autre format. Pour analyser ces données, la liaison de modèle utilise un *formateur d’entrée* configuré pour prendre en charge un type de contenu particulier. Par défaut, ASP.NET Core inclut des formateurs d’entrée basés sur le format JSON pour prendre en charge les données JSON. Vous pouvez ajouter d’autres formateurs pour d’autres types de contenu.

ASP.NET Core sélectionne les formateurs d’entrée en fonction de l’attribut [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute). Si aucun attribut n’est présent, il utilise l’[en-tête Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).

Pour utiliser les formateurs d’entrée XML intégrés :

* Installez le package NuGet `Microsoft.AspNetCore.Mvc.Formatters.Xml`.

* Dans `Startup.ConfigureServices`, appelez <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> ou <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.

  [!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=9)]

* Appliquez l’attribut `Consumes` aux classes de contrôleur ou aux méthodes d’action devant contenir des données XML dans le corps de la requête.

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  Pour plus d’informations, consultez [Introduction à la sérialisation XML](https://docs.microsoft.com/en-us/dotnet/standard/serialization/introducing-xml-serialization).

## <a name="exclude-specified-types-from-model-binding"></a>Exclure les types spécifiés de la liaison de modèle

Le comportement de la liaison de modèle et du système de validation est régi par [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata). Vous pouvez personnaliser `ModelMetadata` en ajoutant un fournisseur de détails à [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders). Des fournisseurs de détails intégrés sont disponibles pour désactiver la liaison de modèle ou la validation des types spécifiés.

Pour désactiver la liaison de modèle sur tous les modèles d’un type spécifique, ajoutez <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> dans `Startup.ConfigureServices`. Par exemple, pour désactiver la liaison de modèle sur tous les modèles de type `System.Version` :

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=4-5)]

Pour désactiver la validation des propriétés d’un type spécifique, ajoutez <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> dans `Startup.ConfigureServices`. Par exemple, pour désactiver la validation sur les propriétés de type `System.Guid` :

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=6-7)]

## <a name="custom-model-binders"></a>Lieurs de modèles personnalisés

Vous pouvez étendre la liaison de modèle en écrivant un lieur de modèle personnalisé et en utilisant l’attribut `[ModelBinder]` afin de le sélectionner pour une cible donnée. Découvrez plus d’informations sur la [liaison de modèle personnalisée](xref:mvc/advanced/custom-model-binding).

## <a name="manual-model-binding"></a>Liaison de modèle manuelle

Vous pouvez appeler la liaison de modèle manuellement à l’aide de la méthode <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>. La méthode est définie sur les classes `ControllerBase` et `PageModel`. Les surcharges de méthode vous permettent de spécifier le préfixe et le fournisseur de valeurs à utiliser. La méthode retourne `false` en cas d’échec de la liaison de modèle. Voici un exemple :

[!code-csharp[](model-binding/samples/2.x/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

## <a name="fromservices-attribute"></a>Attribut [FromServices]

Le nom de cet attribut suit le modèle des attributs de liaison de modèle qui spécifient une source de données. Toutefois, il ne permet pas de lier les données d’un fournisseur de valeurs. Il obtient une instance d’un type à partir du conteneur d’[injection de dépendances](xref:fundamentals/dependency-injection). Son objectif est de fournir une alternative à l’injection de constructeurs quand vous avez besoin d’un service uniquement si une méthode particulière est appelée.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>
