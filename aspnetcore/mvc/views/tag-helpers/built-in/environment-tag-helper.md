---
title: Tag Helper Environnement dans ASP.NET Core
author: pkellner
description: Tag Helper Environnement ASP.NET Core défini avec toutes les propriétés
ms.author: riande
ms.date: 07/14/2017
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 4a283a3a03aa6cac228ec6effd02e3f1095be260
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342222"
---
# <a name="environment-tag-helper-in-aspnet-core"></a>Tag Helper Environnement dans ASP.NET Core

Par [Peter Kellner](http://peterkellner.net) et [Hisham Bin Ateya](https://twitter.com/hishambinateya)

Le Tag Helper Environnement restitue de façon conditionnelle son contenu joint en fonction de l’environnement d’hébergement actuel. Son seul attribut `names` est une liste séparée par des virgules de noms d’environnement qui, s’ils correspondent à l’environnement actuel, déclenchent l’affichage du contenu joint.

## <a name="environment-tag-helper-attributes"></a>Attributs de Tag Helper Environnement

### <a name="names"></a>noms

Accepte un seul nom d’environnement d’hébergement ou une liste séparée par des virgules de noms d’environnement d’hébergement qui déclenchent l’affichage du contenu joint.

Ces valeurs sont comparées à la valeur actuelle retournée par la propriété statique ASP.NET Core `HostingEnvironment.EnvironmentName`.  Cette valeur est l’une des suivantes : **Préproduction**, **Développement** ou **Production**. La comparaison ignore la casse.

Un exemple de Tag Helper `environment` valide est :

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a>Attributs include et exclude

ASP.NET Core 2.x ajoute les attributs `include` & `exclude`. Ces attributs contrôlent le rendu du contenu joint en fonction des noms d’environnement d’hébergement inclus ou exclus.

### <a name="include-aspnet-core-20-and-later"></a>Propriété include ASP.NET Core 2.0 et ultérieur

La propriété `include` a un comportement semblable à l’attribut `names` dans ASP.NET Core 1.0.

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a>Propriété exclude ASP.NET Core 2.0 et ultérieur

En revanche, la propriété `exclude` permet à `EnvironmentTagHelper` de restituer le contenu joint pour tous les noms d’environnement d’hébergement à l’exception de ceux que vous avez spécifiés.

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:fundamentals/environments>
