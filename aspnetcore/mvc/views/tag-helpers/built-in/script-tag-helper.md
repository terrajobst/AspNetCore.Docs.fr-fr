---
title: Tag Helper de script dans ASP.NET Core
author: rick-anderson
ms.author: riande
description: Découvrez les attributs d’assistance de balise de script ASP.NET Core et le rôle joué par chaque attribut lors de l’extension du comportement de la balise de script HTML.
ms.custom: mvc
ms.date: 12/18/2018
uid: mvc/views/tag-helpers/builtin-th/script-tag-helper
ms.openlocfilehash: c3d9148bd62dcc045873cc3a72884ae458349d70
ms.sourcegitcommit: 3e503ef510008e77be6dd82ee79213c9f7b97607
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74317122"
---
# <a name="script-tag-helper-in-aspnet-core"></a>Tag Helper de script dans ASP.NET Core

De [Rick Anderson](https://twitter.com/RickAndMSFT)

Le [tag Helper script](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) génère un lien vers un fichier de script principal ou de retour. En général, le fichier de script principal se trouve sur un [réseau de distribution de contenu](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).

[!INCLUDE[](~/includes/cdn.md)]

Le tag Helper script vous permet de spécifier un CDN pour le fichier de script et un secours lorsque le CDN n’est pas disponible. Le tag Helper script fournit l’avantage en termes de performances d’un CDN avec la robustesse de l’hébergement local.

Le balisage Razor suivant montre un élément `script` avec une solution de secours :

```HTML
<script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-3.3.1.min.js"
        asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
        asp-fallback-test="window.jQuery"
        crossorigin="anonymous"
        integrity="sha384-tsQFqpEReu7ZLhBV2VZlAu7zcOV+rXbYlF2cqB8txI/8aZajjp4Bqd+V6D5IgvKT">
</script>
```

## <a name="commonly-used-script-tag-helper-attributes"></a>Attributs d’assistance de balise de script couramment utilisés

Consultez [l’aide de balises de script](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) pour obtenir tous les attributs, propriétés et méthodes du programme d’assistance des balises de script.

### <a name="asp-fallback-test"></a>ASP-Fallback-test

Méthode de script définie dans le script principal à utiliser pour le test de secours. Pour plus d'informations, consultez <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackTestExpression>.

### <a name="asp-fallback-src"></a>ASP-Fallback-SRC

URL d’une balise de script vers laquelle effectuer un secours dans le cas où le principal échoue. Pour plus d'informations, consultez <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackSrc>.

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
