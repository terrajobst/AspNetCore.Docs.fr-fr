---
title: Tag Helper Image dans ASP.NET Core
author: pkellner
description: Montre comment utiliser un Tag Helper Image
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 6aa9175f873c4ea62e0319c812e5312cd3331141
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/22/2018
---
# <a name="image-tag-helper-in-aspnet-core"></a>Tag Helper Image dans ASP.NET Core

Par [Peter Kellner](http://peterkellner.net) 

Le Tag Helper Image améliore la balise `img` (`<img>`). Il nécessite une balise `src` ainsi que l’attribut `boolean` `asp-append-version`.

Si la source d’image (`src`) est un fichier statique sur le serveur web hôte, une chaîne de cache busting unique est ajoutée en tant que paramètre de requête à la source d’image. Ainsi, si le fichier sur le serveur web hôte change, une URL de requête unique qui inclut le paramètre de requête mis à jour est générée. La chaîne de cache busting est une valeur unique représentant le hachage du fichier image statique.

Si la source d’image (`src`) n’est pas un fichier statique (par exemple, une URL distante ou le fichier n’existe pas sur le serveur), l’attribut `src` de la balise `<img>` est généré sans paramètre de chaîne de requête de cache busting.

## <a name="image-tag-helper-attributes"></a>Attributs de Tag Helper Image


### <a name="asp-append-version"></a>asp-append-version

Quand il est spécifié avec un attribut `src`, le Tag Helper Image est appelé.

Un exemple de Tag Helper `img` valide est :

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

Si le fichier statique existe dans le répertoire *..wwwroot/images/asplogo.png*, le code html généré est semblable au suivant (le hachage sera différent) :

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

La valeur affectée au paramètre `v` est la valeur de hachage du fichier sur le disque. Si le serveur web ne peut pas obtenir l’accès en lecture au fichier statique référencé, aucun paramètre `v` n’est ajouté à l’attribut `src`.

- - -

### <a name="src"></a>src

Pour activer le Tag Helper Image, l’attribut src est obligatoire sur l’élément `<img>`. 

> [!NOTE]
> Le Tag Helper Image utilise le fournisseur `Cache` sur le serveur web local pour stocker le hachage `Sha512` calculé d’un fichier donné. Si le fichier est à nouveau demandé, `Sha512` ne doit pas être recalculé. Le cache est invalidé par un observateur de fichier qui est associé au fichier quand le hachage `Sha512` du fichier est calculé.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:performance/caching/memory>
