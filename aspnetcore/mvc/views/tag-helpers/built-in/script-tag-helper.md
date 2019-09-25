---
title: Tag Helper de script dans ASP.NET Core
author: rick-anderson
ms.author: riande
description: Découvrez les attributs d’assistance de balise de script ASP.NET Core et le rôle joué par chaque attribut lors de l’extension du comportement de la balise de script HTML.
ms.custom: mvc
ms.date: 12/18/2018
uid: mvc/views/tag-helpers/builtin-th/script-tag-helper
ms.openlocfilehash: 5f2fb8a45048804afa8aff2989cd53489e45a33b
ms.sourcegitcommit: fae6f0e253f9d62d8f39de5884d2ba2b4b2a6050
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71256497"
---
# <a name="script-tag-helper-in-aspnet-core"></a>Tag Helper de script dans ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Le [tag Helper script](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) génère un lien vers un fichier de script principal ou de retour. En général, le fichier de script principal se trouve sur un [réseau de distribution de contenu](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).

[!INCLUDE[](~/includes/cdn.md)]

Le tag Helper script vous permet de spécifier un CDN pour le fichier de script et un secours lorsque le CDN n’est pas disponible. Le tag Helper script fournit l’avantage en termes de performances d’un CDN avec la robustesse de l’hébergement local.

Le balisage Razor suivant montre `script` l’élément d’un fichier de disposition créé avec le modèle d’application Web ASP.net Core :

[!code-html[](link-tag-helper/sample/_Layout.cshtml?name=snippet2)]

Les éléments suivants sont similaires au code HTML rendu du code précédent (dans un environnement de non-développement) :

[!code-csharp[](link-tag-helper/sample/HtmlPage2.html)]

Dans le code précédent, le tag Helper script générait le deuxième élément script `<script>  (window.jQuery || document.write(`(), qui `window.jQuery`teste. Si `window.jQuery` est introuvable `document.write(` , exécute et crée un script 

## <a name="commonly-used-script-tag-helper-attributes"></a>Attributs d’assistance de balise de script couramment utilisés

Consultez [l’aide de balises de script](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) pour obtenir tous les attributs, propriétés et méthodes du programme d’assistance des balises de script.

### <a name="href"></a>href

Adresse préférée de la ressource liée. Dans tous les cas, l’adresse est passée au code HTML généré.

### <a name="asp-fallback-href"></a>ASP-Fallback-href

URL d’une feuille de style CSS vers laquelle effectuer le secours dans le cas où l’URL principale échoue.

### <a name="asp-fallback-test-class"></a>ASP-Fallback-test-Class

Nom de classe défini dans la feuille de style à utiliser pour le test de secours. Pour plus d'informations, consultez <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestClass>.

### <a name="asp-fallback-test-property"></a>ASP-Fallback-test-propriété

Nom de la propriété CSS à utiliser pour le test de secours. Pour plus d'informations, consultez <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestProperty>.

### <a name="asp-fallback-test-value"></a>ASP-Fallback-test-value

Valeur de propriété CSS à utiliser pour le test de secours. Pour plus d'informations, consultez <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.

### <a name="asp-fallback-test-value"></a>ASP-Fallback-test-value

Valeur de propriété CSS à utiliser pour le test de secours. Pour plus d'informations, consultez <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>

## <a name="additional-resources"></a>Ressources supplémentaires

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
