---
title: Identité d’une structure de projets ASP.NET Core
author: rick-anderson
description: Découvrez comment structurer d’identité dans un projet ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 08/16/2018
uid: security/authentication/scaffold-identity
ms.openlocfilehash: e35836fa9c20729da7c857243410833749b3a595
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055846"
---
# <a name="scaffold-identity-in-aspnet-core-projects"></a>Identité d’une structure de projets ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core 2.1 et versions ultérieures offre [ASP.NET Core Identity](xref:security/authentication/identity) comme un [bibliothèque de classes Razor](xref:razor-pages/ui-class). Les applications qui incluent l’identité peuvent s’appliquer à la génération de modèles automatique pour ajouter du code source contenu dans la bibliothèque de classes Razor (RCL) identité de façon sélective. Vous pouvez souhaiter générer le code source afin de pouvoir modifier le code et changer le comportement. Par exemple, vous pouvez demander au générateur de modèles automatique de générer le code utilisé dans l’inscription. Le code généré est prioritaire sur le même code dans la bibliothèque de classes Razor d’identité. Pour obtenir le contrôle intégral de l’interface utilisateur et n’utilisez pas la valeur par défaut RCL, consultez la section [créer une source de l’interface utilisateur complète identité](#full).

Les applications qui effectuent **pas** incluent l’authentification peut s’appliquer à la génération de modèles automatique pour ajouter le package RCL identité. Vous pouvez sélectionner le code d’identité à générer.

Bien que le Générateur de modèles automatique génère la plupart du code nécessaire, vous devrez mettre à jour votre projet pour terminer le processus. Ce document explique les étapes nécessaires pour effectuer une mise à jour de la génération de modèles automatique identité.

Lorsque le Générateur de modèles automatique identité est exécuté, un *ScaffoldingReadme.txt* fichier est créé dans le répertoire du projet. Le *ScaffoldingReadme.txt* fichier contient des instructions générales sur ce qui est nécessaire pour terminer la mise à jour de la génération de modèles automatique identité. Ce document contient des instructions plus complètes que les *ScaffoldingReadme.txt* fichier.

Nous vous recommandons d’utiliser un système de contrôle de source qui illustre les différences entre les fichiers et vous permet d’annuler les modifications. Examiner les modifications après l’exécution de la génération de modèles automatique identité.

> [!NOTE]
> Services sont requis lorsque vous utilisez [authentification à deux facteurs](xref:security/authentication/identity-enable-qrcodes), [confirmation et le mot de passe de récupération de compte](xref:security/authentication/accconfirm)et d’autres fonctionnalités de sécurité avec l’identité. Services ou stubs de service ne sont pas générés lors de la génération de modèles automatique identité. Services pour activer ces fonctionnalités doivent être ajoutés manuellement. Par exemple, consultez [nécessitent une Confirmation de courrier électronique](xref:security/authentication/accconfirm#require-email-confirmation).

## <a name="scaffold-identity-into-an-empty-project"></a>Identité d’une structure dans un projet vide

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg.md)]

Ajoutez les appels en surbrillance suivants à la `Startup` classe :

[!code-csharp[](scaffold-identity/sample/StartupEmpty.cs?name=snippet1&highlight=5,20-23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

## <a name="scaffold-identity-into-a-razor-project-without-existing-authorization"></a>Identité d’une structure dans un projet Razor sans autorisation existant

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

<a name="efm"></a>

### <a name="migrations-useauthentication-and-layout"></a>Migrations, UseAuthentication et la disposition

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

<a name="useauthentication"></a>

### <a name="enable-authentication"></a>Activer l’authentification

Dans le `Configure` méthode de la `Startup` classe, appelez [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) après `UseStaticFiles`:

[!code-csharp[](scaffold-identity/sample/StartupRPnoAuth.cs?name=snippet1&highlight=29)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

### <a name="layout-changes"></a>Changements de disposition

Facultatif : Ajouter la connexion partielle (`_LoginPartial`) pour le fichier de disposition :

[!code-html[Main](scaffold-identity/sample/_Layout.cshtml?highlight=37)]

## <a name="scaffold-identity-into-a-razor-project-with-authorization"></a>Identité d’une structure dans un projet Razor disposant d’autorisations

<!--
Use >=2.1: dotnet new webapp -au Individual -o RPauth
Use = 2.0: dotnet new razor -au Individual -o RPauth
cd RPauth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc RPauth.Data.ApplicationDbContext --files Account.Register

[!INCLUDE[](~/includes/webapp-alias-notice.md)]
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]
Certaines options d’identité sont configurées dans *Areas/Identity/IdentityHostingStartup.cs*. Pour plus d’informations, consultez [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).

## <a name="scaffold-identity-into-an-mvc-project-without-existing-authorization"></a>Identité d’une structure dans un projet MVC sans autorisation existant

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

Facultatif : Ajouter la connexion partielle (`_LoginPartial`) pour le *Views/Shared/_Layout.cshtml* fichier :

[!code-html[](scaffold-identity/sample/_LayoutMvc.cshtml?highlight=37)]

* Déplacer le *Pages/Shared/_LoginPartial.cshtml* fichier *Views/Shared/_LoginPartial.cshtml*

Identité est configurée dans *Areas/Identity/IdentityHostingStartup.cs*. Pour plus d’informations, consultez IHostingStartup.

[!INCLUDE[](~/includes/scaffold-identity/migrations.md)]

Appelez [UseAuthentication](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_) après `UseStaticFiles`:

[!code-csharp[](scaffold-identity/sample/StartupMvcNoAuth.cs?name=snippet1&highlight=23)]

[!INCLUDE[](~/includes/scaffold-identity/hsts.md)]

## <a name="scaffold-identity-into-an-mvc-project-with-authorization"></a>Identité d’une structure dans un projet MVC avec l’autorisation

<!--
dotnet new mvc -au Individual -o MvcAuth
cd MvcAuth
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -dc MvcAuth.Data.ApplicationDbContext --files Account.Register
-->

[!INCLUDE[](~/includes/scaffold-identity/id-scaffold-dlg-auth.md)]

Supprimer le *Pages/Shared* dossier et les fichiers dans ce dossier.

<a name="full"></a>

## <a name="create-full-identity-ui-source"></a>Créer la source de l’interface utilisateur d’identité complète

Pour conserver le contrôle intégral de l’interface utilisateur d’identité, exécutez le Générateur de modèles automatique identité et sélectionnez **remplacer tous les fichiers**.

Le code en surbrillance suivant montre les modifications apportées à remplacer la valeur par défaut de l’interface utilisateur de l’identité avec l’identité dans une application web de ASP.NET Core 2.1. Vous souhaiterez peut-être effectuer cette opération pour avoir un contrôle total de l’interface utilisateur d’identité.

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet1&highlight=13-14,17-999)]

L’identité par défaut est remplacé dans le code suivant :

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet2)]

Ce qui suit le code définit le [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath), [valeur de LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath), et [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath):

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet3)]

Inscrire un `IEmailSender` implémentation, par exemple :

[!code-csharp[](scaffold-identity/sample/StartupFull.cs?name=snippet4)]
