---
title: Migration de l’authentification et identité vers ASP.NET Core
author: ardalis
description: En savoir plus sur la migration de l’authentification et identité à partir d’un projet ASP.NET MVC à un projet ASP.NET MVC de base.
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: e05d72ca78c7b8191a47f78cda31ee40e04d0706
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275695"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a>Migration de l’authentification et identité vers ASP.NET Core

Par [Steve Smith](https://ardalis.com/)

Dans l’article précédent, nous [migré la configuration à partir d’un projet ASP.NET MVC à ASP.NET MVC de base](xref:migration/configuration). Dans cet article, nous migrer les fonctionnalités de gestion d’inscription et connexion utilisateur.

## <a name="configure-identity-and-membership"></a>Configurez l’identité et l’appartenance

Dans ASP.NET MVC, les fonctionnalités d’authentification et identité sont configurées à l’aide d’ASP.NET Identity dans *Startup.Auth.cs* et *IdentityConfig.cs*, situé dans le *App_Start* dossier. Dans ASP.NET MVC de base, ces fonctionnalités sont configurées dans *Startup.cs*.

Installez les packages NuGet `Microsoft.AspNetCore.Identity.EntityFrameworkCore` et `Microsoft.AspNetCore.Authentication.Cookies`.

Ensuite, ouvrez *Startup.cs* et mettre à jour le `Startup.ConfigureServices` méthode à utiliser les services d’Entity Framework et de l’identité :

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

À ce stade, il existe des deux types référencés dans le code ci-dessus, nous n’avons pas encore été migrés à partir du projet ASP.NET MVC : `ApplicationDbContext` et `ApplicationUser`. Créer un nouveau *modèles* dossier dans le noyau ASP.NET le projet, puis ajoutez les deux classes lui correspondant à ces types. Vous trouverez le ASP.NET MVC versions de ces classes dans */Models/IdentityModels.cs*, mais nous allons utiliser un fichier par la classe dans le projet migré car il s’agit plus claire.

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
            // Customize the ASP.NET Identity model and override the defaults if needed.
            // For example, you can rename the ASP.NET Identity table names and more.
            // Add your customizations after calling base.OnModelCreating(builder);
        }
    }
}
```

Personnalisation des utilisateurs, n’inclut pas le projet Web de Starter MVC ASP.NET Core ou `ApplicationDbContext`. Lorsque vous migrez une application réelle, vous devez également migrer toutes les propriétés personnalisées et les méthodes de l’utilisateur de votre application et `DbContext` classes, ainsi que d’autres classes de modèle, votre application utilise. Par exemple, si votre `DbContext` a un `DbSet<Album>`, vous devez migrer le `Album` classe.

Avec ces fichiers en place, le *Startup.cs* fichier possible de compiler en mettant à jour son `using` instructions :

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

Notre application est maintenant prête à prendre en charge les services d’authentification et identité. Il suffit d’activer ces fonctionnalités exposées aux utilisateurs.

## <a name="migrate-registration-and-login-logic"></a>Migrer la logique de l’inscription et connexion

Avec les services d’identité configurées pour l’application et l’accès aux données configuré à l’aide d’Entity Framework et SQL Server, nous pouvons ajouter la prise en charge pour l’inscription et connexion à l’application. N’oubliez pas que [plus haut dans le processus de migration](xref:migration/mvc#migrate-the-layout-file) nous commenté une référence à *_LoginPartial* dans *_Layout.cshtml*. Il est maintenant temps pour revenir à ce code, supprimez les commentaires et l’ajouter dans les contrôleurs nécessaires et les vues pour prendre en charge les fonctionnalités de connexion.

Ne pas commenter le `@Html.Partial` ligne *_Layout.cshtml*:

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

À ce stade, vous devez être en mesure d’actualiser le site dans votre navigateur.

## <a name="summary"></a>Récapitulatif

ASP.NET Core introduit des modifications pour les fonctionnalités d’identité ASP.NET. Dans cet article, vous avez vu comment migrer les fonctionnalités de gestion de l’authentification et l’utilisateur d’identité ASP.NET vers ASP.NET Core.
