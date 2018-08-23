---
title: Migrer d’authentification et identité vers ASP.NET Core
author: ardalis
description: Découvrez comment migrer d’authentification et identité à partir d’un projet ASP.NET MVC à un projet ASP.NET Core MVC.
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: 72e62e78e37325ec47d54abbc11a875ae87fb63a
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838430"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a>Migrer d’authentification et identité vers ASP.NET Core

Par [Steve Smith](https://ardalis.com/)

Dans l’article précédent, nous [migrées de configuration à partir d’un projet ASP.NET MVC vers ASP.NET Core MVC](xref:migration/configuration). Dans cet article, nous migrons les fonctionnalités de gestion de l’inscription, connexion et un utilisateur.

## <a name="configure-identity-and-membership"></a>Configurer l’identité et appartenance

Dans ASP.NET MVC, les fonctionnalités d’authentification et identité sont configurées à l’aide d’ASP.NET Identity dans *Startup.Auth.cs* et *IdentityConfig.cs*, situé dans le *App_Start* dossier. Dans ASP.NET Core MVC, ces fonctionnalités sont configurées dans *Startup.cs*.

Installez les packages NuGet `Microsoft.AspNetCore.Identity.EntityFrameworkCore` et `Microsoft.AspNetCore.Authentication.Cookies`.

Ensuite, ouvrir *Startup.cs* et mettre à jour le `Startup.ConfigureServices` méthode à utiliser les services Entity Framework et d’identité :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add EF services to the services container.
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

     services.AddMvc();
}
```

À ce stade, il existe deux types référencés dans le code ci-dessus nous n’avons pas encore été migrés à partir du projet ASP.NET MVC : `ApplicationDbContext` et `ApplicationUser`. Créer un nouveau *modèles* dossier dans ASP.NET Core projet, puis ajoutez deux classes lui correspondant à ces types. Vous trouverez ASP.NET MVC versions de ces classes dans */Models/IdentityModels.cs*, mais nous allons utiliser un fichier par la classe dans le projet migré puisque c’est plus clair.

*ApplicationUser.cs*:

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

*ApplicationDbContext.cs*:

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvcProject.Models
{
    public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }

        protected override void OnModelCreating(ModelBuilder builder)
        {
            base.OnModelCreating(builder);
            // Customize the ASP.NET Core Identity model and override the defaults if needed.
            // For example, you can rename the ASP.NET Core Identity table names and more.
            // Add your customizations after calling base.OnModelCreating(builder);
        }
    }
}
```

Le projet ASP.NET Core MVC Starter Web n’inclut pas grande personnalisation des utilisateurs, ou le `ApplicationDbContext`. Lorsque vous migrez une application réelle, vous devez également migrer toutes les propriétés personnalisées et les méthodes de l’utilisateur de votre application et `DbContext` classes, ainsi que d’autres classes de modèle, votre application utilise. Par exemple, si votre `DbContext` a un `DbSet<Album>`, vous devez migrer le `Album` classe.

Avec ces fichiers en place, le *Startup.cs* fichier peut être apportée à compiler en mettant à jour son `using` instructions :

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

Notre application est maintenant prête à prendre en charge des services d’authentification et identité. Il a juste besoin d’activer ces fonctionnalités exposées aux utilisateurs.

## <a name="migrate-registration-and-login-logic"></a>Migrer la logique de l’inscription et connexion

Avec les services d’identité configurés pour l’application et l’accès aux données configuré à l’aide d’Entity Framework et SQL Server, nous sommes prêts à ajouter la prise en charge pour l’inscription et connexion à l’application. N’oubliez pas que [plus haut dans le processus de migration](xref:migration/mvc#migrate-the-layout-file) nous commenté une référence à *_LoginPartial* dans *_Layout.cshtml*. Il est maintenant temps pour revenir à ce code, supprimez les commentaires et l’ajouter dans les contrôleurs nécessaires et les vues pour prendre en charge la fonctionnalité de connexion.

Supprimez les commentaires de la `@Html.Partial` de ligne dans *_Layout.cshtml*:

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

Maintenant, ajoutez une nouvelle vue Razor appelée *_LoginPartial* à la *Views/Shared* dossier :

Mise à jour *_LoginPartial.cshtml* avec le code suivant (Remplacez tout son contenu) :

```cshtml
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="Logout" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log out</button>
            </li>
        </ul>
    </form>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="" asp-controller="Account" asp-action="Register">Register</a></li>
        <li><a asp-area="" asp-controller="Account" asp-action="Login">Log in</a></li>
    </ul>
}
```

À ce stade, vous devez pouvoir actualiser le site dans votre navigateur.

## <a name="summary"></a>Récapitulatif

ASP.NET Core apporte des modifications aux fonctionnalités ASP.NET Identity. Dans cet article, vous avez vu comment migrer les fonctionnalités de gestion de l’authentification et l’utilisateur de l’identité d’ASP.NET vers ASP.NET Core.
