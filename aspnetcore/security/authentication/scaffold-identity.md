---
title: Identité de vue de structure dans les projets ASP.NET Core
author: rick-anderson
description: Découvrez comment structurer d’identité dans un projet ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/scaffold-identity
ms.openlocfilehash: a43b7bbaf1f90d3373b3846bc3f4f32be6b80bd4
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729608"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a>Identité de vue de structure dans les projets ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core 2.1 et versions ultérieures fournit [ASP.NET Core Identity](xref:security/authentication/identity) comme un [bibliothèque de classes Razor](xref:mvc/razor-pages/ui-class). Les applications qui incluent l’identité peuvent appliquer le scaffolder pour ajouter du code source contenu dans la bibliothèque de classe de Razor (RCL) identité de manière sélective. Vous pouvez souhaiter générer le code source pour vous permettre de modifier le code et de modifier le comportement. Par exemple, vous pouvez demander à le scaffolder pour générer le code utilisé dans l’inscription. Code généré est prioritaire sur le même code de l’identité RCL.

Les applications qui effectuent **pas** incluent l’authentification peut appliquer le scaffolder pour ajouter le package RCL identité. Vous avez la possibilité de sélectionner une code d’identité doit être créé.

Bien que le scaffolder génère la plupart du code nécessaire, vous devrez mettre à jour votre projet pour terminer le processus. Ce document explique les étapes nécessaires pour effectuer une mise à jour de la génération de modèles automatique identité.

Lorsque le scaffolder d’identité est exécuté, un *ScaffoldingReadme.txt* fichier est créé dans le répertoire du projet. Le *ScaffoldingReadme.txt* fichier contient des instructions générales sur ce qui est nécessaire pour terminer la mise à jour de la génération de modèles automatique identité. Ce document contient des instructions détaillées de la lecture du *ScaffoldingReadme.txt* fichier.

Nous recommandons d’utiliser un système de contrôle de source qui montre les différences entre les fichiers et vous permet d’annuler les modifications. Vérifiez que les modifications après avoir exécuté le scaffolder d’identité.

## <a name="scaffold-identity-into-an-empty-project"></a>Identité de vue de structure dans un projet vide

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Ajouter les appels en surbrillance suivants à la `Startup` classe :

[!code-csharp[Main](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a>Identité de vue de structure dans un projet Razor sans autorisation existante

<!--
set projNam=RPnoAuth
set projType=razor
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Identité est configurée dans *Areas/Identity/IdentityHostingStartup.cs*. Pour plus d’informations, consultez [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

Dans le `Configure` méthode de la `Startup` classe, appelez [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) après `UseStaticFiles`:

[!code-csharp[Main](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a>Changements de disposition

Facultatif : Ajoutez la connexion partielle (`_LoginPartial`) pour le fichier de disposition :

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a>Identité de vue de structure dans un projet Razor disposant d’autorisations

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
Certaines options d’identité sont configurées dans *Areas/Identity/IdentityHostingStartup.cs*. Pour plus d’informations, consultez [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a>Identité de vue de structure dans un projet MVC sans autorisation existante

<!--
set projNam=MvcNoAuth
set projType=mvc
set version=2.1.0

dotnet new %projType% -o %projNam%
cd %projNam%
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design -v %version%
dotnet restore
dotnet aspnet-codegenerator identity --useDefaultUI
dotnet ef migrations add CreateIdentitySchema
dotnet ef database update
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Facultatif : Ajoutez la connexion partielle (`_LoginPartial`) pour le *Views/Shared/_Layout.cshtml* fichier :

[!code-html[Main](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* Déplacer le *Pages/Shared/_LoginPartial.cshtml* le fichier *Views/Shared/_LoginPartial.cshtml*

Identité est configurée dans *Areas/Identity/IdentityHostingStartup.cs*. Pour plus d’informations, consultez IHostingStartup.

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

Appelez [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) après `UseStaticFiles`:

[!code-csharp[Main](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a>Identité de vue de structure dans un projet MVC avec autorisation

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

Supprimer le *Pages/Shared* dossier et les fichiers dans ce dossier.
