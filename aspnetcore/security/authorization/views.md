---
title: Autorisation basée sur l’affichage dans ASP.NET Core MVC
author: rick-anderson
description: Ce document montre comment injecter et utiliser le service d’autorisation à l’intérieur d’une vue ASP.NET Core Razor.
ms.author: riande
ms.date: 10/30/2017
uid: security/authorization/views
ms.openlocfilehash: e497c41d4dca29fed8733f18cf727804e3f06d8c
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342534"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a>Autorisation basée sur l’affichage dans ASP.NET Core MVC

Un développeur souhaite souvent afficher, masquer ou modifier une interface utilisateur basée sur l’identité de l’utilisateur actuel. Vous pouvez accéder au service de l’autorisation au sein de vues MVC via [l'injection de dépendance](xref:fundamentals/dependency-injection). Pour insérer le service d’autorisation dans une vue Razor, utilisez la directive `@inject`:

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

Si vous souhaitez que le service d’autorisation soit effectif dans chaque vue, placez la directive `@inject` dans le fichier *_ViewImports.cshtml* de la *vues* active. Pour plus d’informations, consultez [injection de dépendances dans les vues](xref:mvc/views/dependency-injection).

Utilisez le service d'injection d’autorisation pour appeller `AuthorizeAsync` de la même façon et ainsi vous vérifieriez [les autorisations basée sur les ressources](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

Dans certains cas, la ressource sera votre modèle de Vue. Utilisez `AuthorizeAsync` de la même façon, ainsi vous vérifieriez [les autorisation basée sur les ressources](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

Dans le code précédent, le modèle est transmis en tant que ressource que l’évaluation de stratégie doit prendre en considération.

> [!WARNING]
> Ne vous fiez bascule si celle-ci visibilité des éléments d’interface utilisateur de votre application en tant que le contrôle d’autorisation unique. Masquer un élément d’interface utilisateur ne peut-être pas complètement empêcher l’accès à son action de contrôleur associé. Par exemple, considérez le bouton dans l’extrait de code précédent. Un utilisateur peut appeler le `Edit` URL de la méthode d’action si il connaisse la ressource relative est */Document/Edit/1*. Pour cette raison, le `Edit` méthode d’action doit exécuter son propre contrôle d’autorisation.
