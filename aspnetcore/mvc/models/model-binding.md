---
title: "Liaison de modèle"
author: rachelappel
description: "Pour plus d’informations sur la liaison de modèle dans ASP.NET MVC de base"
ms.author: rachelap
manager: wpickett
ms.date: 01/22/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
uid: mvc/models/model-binding
ms.openlocfilehash: 26c4c016548cc3e465991c5ebf16893d4022145d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/24/2018
---
# <a name="model-binding"></a>Liaison de modèle

Par [Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-binding"></a>Introduction à la liaison de modèle

La liaison de modèle dans ASP.NET Core MVC mappe les données de requêtes HTTP aux paramètres de méthode d’action. Les paramètres peuvent être des types simples tels que des chaînes, des entiers ou des nombres à virgule flottante, ou peuvent être des types complexes. Il s’agit d’une fonctionnalité intéressante de MVC, car le mappage des données entrantes à un homologue est un scénario souvent répété, quelle que soit la taille ou la complexité des données. MVC résout ce problème en abstrayant la liaison de données afin d'éviter aux développeurs de réécrire une version légèrement différente de ce même code dans chaque application. L’écriture de votre propre texte pour le code de convertisseur de type est fastidieux et sujet aux erreurs.

## <a name="how-model-binding-works"></a>Fonctionnement de la liaison de modèle

Lorsque MVC reçoit une requête HTTP, il l'achemine vers une méthode d’action spécifique d’un contrôleur. Il détermine la méthode d’action à exécuter en fonction de ce qui est dans les données de routing, puis lie les valeurs de la requête HTTP aux paramètres de cette méthode d’action. Par exemple, considérez l’URL suivante : 

`http://contoso.com/movies/edit/2`

Étant donné que le modèle de routing ressemble à ceci, `{controller=Home}/{action=Index}/{id?}`, `movies/edit/2` achemine vers le contrôleur `Movies` et sa méthode d’action `Edit`. Il accepte également un paramètre facultatif, appelé `id`. Le code de la méthode d’action doit ressembler à ceci : 

```csharp
public IActionResult Edit(int? id)
   ```

Remarque : Les chaînes dans la route d’URL ne respectent pas la casse.

MVC tente de lier des données de la demande aux paramètres d’action par nom. MVC recherche des valeurs pour chaque paramètre à l’aide du nom du paramètre et des noms de ses propriétés définissables publiques. Dans l’exemple ci-dessus, le paramètre de la seule action est nommé `id`, auquel MVC lie la valeur portant le même nom dans les valeurs de routing. En plus des valeurs de routing MVC lie des données à partir de différentes parties de la demande et ce, dans un ordre bien défini. Voici une liste des sources de données dans l’ordre dans lequel la liaison de modèle les recherche :

1. `Form values`: Ce sont des valeurs de formulaire qui vont dans la requête HTTP à l’aide de la méthode POST. (y compris les requêtes POST jQuery).

2. `Route values`: Le jeu de valeurs de routing fourni par le [routage](xref:fundamentals/routing)

3. `Query strings`: La partie de chaîne de requête de l’URI.

<!-- DocFX BUG
The link works but generates an error when building with DocFX
@fundamentals/routing
[Routing](xref:fundamentals/routing)
-->

Remarque : Les valeurs de formulaire, les données de routing et les chaînes de requête sont toutes stockées en tant que paires nom-valeur.

Étant donné que la liaison de modèle invite à entrer une clé nommée `id` et qu'il n'y aucun élément nommé `id` dans les valeurs de formulaire, il passe aux valeurs de routing en recherchant cette clé. Dans notre exemple, il y a correspondance. La liaison se produit, et la valeur est convertie en entier 2. La même demande en utilisant Edit(string id) permet de convertir la chaîne "2".

Jusqu'à présent, l’exemple utilise des types simples. Dans MVC les types simples sont tous les types primitifs .NET ou avec un convertisseur de type chaîne. Si le paramètre de la méthode d’action était une classe comme le type `Movie`, qui contient des types simples et complexes tels que les propriétés, la liaison de modèle MVC continuera à le gérer correctement. Il utilise la réflexion et la récurrence pour parcourir les propriétés des types complexes en recherchant des correspondances. La liaison de modèle recherche le modèle *parameter_name.property_name* pour lier des valeurs aux propriétés. S’il ne trouve pas les valeurs correspondantes de ce formulaire, il va tenter de lier en utilisant simplement le nom de propriété. Pour ces types, tels que les types `Collection`, la  liaison de modèle recherche des correspondances au *nom_paramètre [index]* ou simplement *[index]*. La liaison de modèle traite les types `Dictionary` de la même façon, en demandant de *parameter_name[key]* ou simplement *[index]*, à condition que les clés soient des types simples. Les clés qui sont prises en charge correspondent aux noms de champ HTML et tags helpers générés pour le même type de modèle. Ainsi, cela active l'aller-retour sur les valeurs afin que les champs de formulaire restent remplis avec l’entrée d’utilisateur pour leur faciliter la tâche, par exemple, lorsque les données liées à partir d’une création ou la modification n’ont pas passé la validation.

Pour que la liaison se produise la classe doit avoir un constructeur public par défaut et les membres liés doivent être des propriétés publiques accessibles en écriture. Lorsque la liaison de modèle se produit, la classe sera uniquement instanciée en utilisant le constructeur public par défaut, puis les propriétés peuvent être définies.

Lorsqu’un paramètre est lié, la liaison de modèle arrête la recherche de valeurs portant ce nom et elle passe à la liaison de paramètre suivant. Sinon, le comportement de liaison de modèle par défaut définit les paramètres sur leurs valeurs par défaut en fonction de leur type : 

* `T[]`: À l’exception des tableaux de type `byte[]`, la liaison définit les paramètres de type `T[]` sur `Array.Empty<T>()`. Les tableaux de type `byte[]` ont la valeur `null`.

* Types de référence : La liaison crée une instance d’une classe avec le constructeur par défaut sans définir ses propriétés. Toutefois, la liaison de modèle défini les paramètres `string` sur `null`. 

* Types Nullable : Les types Nullable sont définis sur `null`. Dans l’exemple ci-dessus, la liaison de modèle définit l'`id` sur `null` car il est de type `int?`. 

* Types de valeur : les types de valeur Non nullable de type `T` ont la valeur `default(T)`. Par exemple, la liaison de modèle définit un paramètre `int id` sur 0. Pensez à utiliser la validation des modèles ou des types nullable au lieu d'utiliser les valeurs par défaut. 

Si la liaison échoue, MVC ne provoque pas d'erreur. Chaque action qui accepte une entrée utilisateur doit vérifier la propriété `ModelState.IsValid`. 

Remarque : Chaque entrée dans la propriété `ModelState` du contrôleur est un `ModelStateEntry` contenant une propriété `Errors`. Il est rarement nécessaire d'interroger cette collection vous-même. Utilisez plutôt `ModelState.IsValid`.

En outre, il existe certains types de données spéciaux que MVC doit prendre en compte lors de la liaison de modèle :

* `IFormFile`, `IEnumerable<IFormFile>`: Un ou plusieurs fichiers téléchargés qui font partie de la requête HTTP.

* `CancellationToken`: Utilisé pour annuler l’activité dans les contrôleurs asynchrones.

Ces types peuvent être liés aux paramètres d’action ou à des propriétés sur un type de classe.

Une fois que la liaison de modèle est terminée, la [Validation](validation.md) se produit. La liaison de modèle par défaut fonctionne bien pour la majorité des scénarios de développement. Elle est également extensible afin de pouvoir personnaliser le comportement intégré si vous avez des besoins spécifiques.

## <a name="customize-model-binding-behavior-with-attributes"></a>Personnaliser le comportement de liaison de modèle avec des attributs

MVC contient plusieurs attributs que vous pouvez utiliser pour indiquer son comportement de liaison de modèle par défaut à une source différente. Par exemple, vous pouvez spécifier si la liaison est requise pour une propriété ou si elle ne doit jamais se produire du tout en utilisant les attributs `[BindRequired]` ou `[BindNever]`. Vous pouvez également remplacer la source de données par défaut et spécifier la source de données du model binder. Vous trouverez ci-dessous la liste des attributs de liaison de modèle :

* `[BindRequired]`: Cet attribut ajoute une erreur d’état de modèle si la liaison ne peut pas se produire.

* `[BindNever]`: Indique au model binder de ne jamais lier ce paramètre. 

* `[FromHeader]`, `[FromQuery]`, `[FromRoute]`, `[FromForm]`: Utilisez-les pour spécifier la source de liaison exacte que vous souhaitez appliquer.

* `[FromServices]`: Cet attribut utilise l'[injection de dépendance](../../fundamentals/dependency-injection.md) pour lier les paramètres à partir des services.

* `[FromBody]`: Utilisez les formateurs configurés pour lier des données à partir du corps de la demande. Le module de formatage est sélectionné selon le type de contenu de la demande.

* `[ModelBinder]`: Utilisé pour remplacer le model binder par défaut, la source de liaison et le nom.

Les attributs sont très utiles lorsque vous devez remplacer le comportement par défaut de la liaison de modèle.

## <a name="bind-formatted-data-from-the-request-body"></a>Lier des données mises en forme depuis le corps de la demande 

Les données de la demande peuvent venir dans divers formats, notamment JSON, XML et bien d’autres. Lorsque vous utilisez l’attribut [FromBody] pour indiquer que vous souhaitez lier un paramètre à des données dans le corps de la demande, MVC utilise un jeu de formateurs configuré pour gérer les données de la demande en fonction de son type de contenu. Par défaut, MVC inclut une classe `JsonInputFormatter` pour la gestion des données JSON, mais vous peuvent ajouter des formateurs supplémentaires pour la gestion de XML et d'autres formats personnalisés.

> [!NOTE]
> Il peut y avoir au plus un paramètre par action décoré avec `[FromBody]`. Le runtime Core MVC ASP.NET délègue la responsabilité de lecture du flux de demandes au formateur. Une fois que le flux de demandes est lu pour un paramètre, il n’est généralement pas possible de lire à nouveau le flux de demandes pour la liaison d'autres paramètres `[FromBody]`. 

> [!NOTE]
> `JsonInputFormatter` est le formateur par défaut et est basé sur [Json.NET](https://www.newtonsoft.com/json). 

ASP.NET sélectionne les formateurs d’entrée selon l'en-tête [Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html) et le type du paramètre, sauf s’il existe un attribut appliqué en le spécifiant dans le cas contraire. Si vous souhaitez utiliser des données XML ou un autre format vous devez le configurer dans le fichier *Startup.cs*, mais vous devrez peut-être d’abord obtenir une référence à `Microsoft.AspNetCore.Mvc.Formatters.Xml` à l’aide de NuGet. Votre code de démarrage doit ressembler à ceci :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddXmlSerializerFormatters();
   }
```

Le code dans le fichier *Startup.cs* contient une méthode `ConfigureServices` avec un argument `services` que vous pouvez utiliser pour générer des services pour votre application ASP.NET. Dans l’exemple, nous ajoutons un formateur XML en tant que service que MVC fournit pour cette application. L'argument `options` passé dans la méthode `AddMvc` permet d’ajouter et de gérer des filtres, les formateurs et les autres options système de MVC lors du démarrage de l’application. Puis appliquez l'attribut `Consumes` aux classes de contrôleur ou aux méthodes d’action pour travailler avec le format souhaité.

### <a name="custom-model-binding"></a>Liaison de modèle personnalisé

Vous pouvez étendre la liaison de modèle en écrivant vos propres model binders personnalisés. En savoir plus sur la [liaison de modèle personnalisée](../advanced/custom-model-binding.md). 
