---
title: Migrer d’authentification et identité vers ASP.NET Core
author: ardalis
description: Découvrez comment migrer d’authentification et identité à partir d’un projet ASP.NET MVC à un projet ASP.NET Core MVC.
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: 72e62e78e37325ec47d54abbc11a875ae87fb63a
ms.sourcegitcommit: b72bbc9ae91e4bd37c9ea9b2d09ebf47afb25dd7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/11/2019
ms.locfileid: "41838430"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a><span data-ttu-id="7650a-103">Migrer d’authentification et identité vers ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7650a-103">Migrate Authentication and Identity to ASP.NET Core</span></span>

<span data-ttu-id="7650a-104">Par [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="7650a-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="7650a-105">Dans l’article précédent, nous [migrées de configuration à partir d’un projet ASP.NET MVC vers ASP.NET Core MVC](xref:migration/configuration).</span><span class="sxs-lookup"><span data-stu-id="7650a-105">In the previous article, we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/configuration).</span></span> <span data-ttu-id="7650a-106">Dans cet article, nous migrons les fonctionnalités de gestion de l’inscription, connexion et un utilisateur.</span><span class="sxs-lookup"><span data-stu-id="7650a-106">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="7650a-107">Configurer l’identité et appartenance</span><span class="sxs-lookup"><span data-stu-id="7650a-107">Configure Identity and Membership</span></span>

<span data-ttu-id="7650a-108">Dans ASP.NET MVC, les fonctionnalités d’authentification et identité sont configurées à l’aide d’ASP.NET Identity dans *Startup.Auth.cs* et *IdentityConfig.cs*, situé dans le *App_Start* dossier.</span><span class="sxs-lookup"><span data-stu-id="7650a-108">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in *Startup.Auth.cs* and *IdentityConfig.cs*, located in the *App_Start* folder.</span></span> <span data-ttu-id="7650a-109">Dans ASP.NET Core MVC, ces fonctionnalités sont configurées dans *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="7650a-109">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="7650a-110">Installez les packages NuGet `Microsoft.AspNetCore.Identity.EntityFrameworkCore` et `Microsoft.AspNetCore.Authentication.Cookies`.</span><span class="sxs-lookup"><span data-stu-id="7650a-110">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="7650a-111">Ensuite, ouvrir *Startup.cs* et mettre à jour le `Startup.ConfigureServices` méthode à utiliser les services Entity Framework et d’identité :</span><span class="sxs-lookup"><span data-stu-id="7650a-111">Then, open *Startup.cs* and update the `Startup.ConfigureServices` method to use Entity Framework and Identity services:</span></span>

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

<span data-ttu-id="7650a-112">À ce stade, il existe deux types référencés dans le code ci-dessus nous n’avons pas encore été migrés à partir du projet ASP.NET MVC : `ApplicationDbContext` et `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="7650a-112">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="7650a-113">Créer un nouveau *modèles* dossier dans ASP.NET Core projet, puis ajoutez deux classes lui correspondant à ces types.</span><span class="sxs-lookup"><span data-stu-id="7650a-113">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="7650a-114">Vous trouverez ASP.NET MVC versions de ces classes dans */Models/IdentityModels.cs*, mais nous allons utiliser un fichier par la classe dans le projet migré puisque c’est plus clair.</span><span class="sxs-lookup"><span data-stu-id="7650a-114">You will find the ASP.NET MVC versions of these classes in */Models/IdentityModels.cs*, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="7650a-115">*ApplicationUser.cs*:</span><span class="sxs-lookup"><span data-stu-id="7650a-115">*ApplicationUser.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="7650a-116">*ApplicationDbContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="7650a-116">*ApplicationDbContext.cs*:</span></span>

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

<span data-ttu-id="7650a-117">Le projet ASP.NET Core MVC Starter Web n’inclut pas grande personnalisation des utilisateurs, ou le `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="7650a-117">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the `ApplicationDbContext`.</span></span> <span data-ttu-id="7650a-118">Lorsque vous migrez une application réelle, vous devez également migrer toutes les propriétés personnalisées et les méthodes de l’utilisateur de votre application et `DbContext` classes, ainsi que d’autres classes de modèle, votre application utilise.</span><span class="sxs-lookup"><span data-stu-id="7650a-118">When migrating a real app, you also need to migrate all of the custom properties and methods of your app's user and `DbContext` classes, as well as any other Model classes your app utilizes.</span></span> <span data-ttu-id="7650a-119">Par exemple, si votre `DbContext` a un `DbSet<Album>`, vous devez migrer le `Album` classe.</span><span class="sxs-lookup"><span data-stu-id="7650a-119">For example, if your `DbContext` has a `DbSet<Album>`, you need to migrate the `Album` class.</span></span>

<span data-ttu-id="7650a-120">Avec ces fichiers en place, le *Startup.cs* fichier peut être apportée à compiler en mettant à jour son `using` instructions :</span><span class="sxs-lookup"><span data-stu-id="7650a-120">With these files in place, the *Startup.cs* file can be made to compile by updating its `using` statements:</span></span>

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

<span data-ttu-id="7650a-121">Notre application est maintenant prête à prendre en charge des services d’authentification et identité.</span><span class="sxs-lookup"><span data-stu-id="7650a-121">Our app is now ready to support authentication and Identity services.</span></span> <span data-ttu-id="7650a-122">Il a juste besoin d’activer ces fonctionnalités exposées aux utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="7650a-122">It just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="7650a-123">Migrer la logique de l’inscription et connexion</span><span class="sxs-lookup"><span data-stu-id="7650a-123">Migrate registration and login logic</span></span>

<span data-ttu-id="7650a-124">Avec les services d’identité configurés pour l’application et l’accès aux données configuré à l’aide d’Entity Framework et SQL Server, nous sommes prêts à ajouter la prise en charge pour l’inscription et connexion à l’application.</span><span class="sxs-lookup"><span data-stu-id="7650a-124">With Identity services configured for the app and data access configured using Entity Framework and SQL Server, we're ready to add support for registration and login to the app.</span></span> <span data-ttu-id="7650a-125">N’oubliez pas que [plus haut dans le processus de migration](xref:migration/mvc#migrate-the-layout-file) nous commenté une référence à *_LoginPartial* dans *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7650a-125">Recall that [earlier in the migration process](xref:migration/mvc#migrate-the-layout-file) we commented out a reference to *_LoginPartial* in *_Layout.cshtml*.</span></span> <span data-ttu-id="7650a-126">Il est maintenant temps pour revenir à ce code, supprimez les commentaires et l’ajouter dans les contrôleurs nécessaires et les vues pour prendre en charge la fonctionnalité de connexion.</span><span class="sxs-lookup"><span data-stu-id="7650a-126">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="7650a-127">Supprimez les commentaires de la `@Html.Partial` de ligne dans *_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="7650a-127">Uncomment the `@Html.Partial` line in *_Layout.cshtml*:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="7650a-128">Maintenant, ajoutez une nouvelle vue Razor appelée *_LoginPartial* à la *Views/Shared* dossier :</span><span class="sxs-lookup"><span data-stu-id="7650a-128">Now, add a new Razor view called *_LoginPartial* to the *Views/Shared* folder:</span></span>

<span data-ttu-id="7650a-129">Mise à jour *_LoginPartial.cshtml* avec le code suivant (Remplacez tout son contenu) :</span><span class="sxs-lookup"><span data-stu-id="7650a-129">Update *_LoginPartial.cshtml* with the following code (replace all of its contents):</span></span>

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

<span data-ttu-id="7650a-130">À ce stade, vous devez pouvoir actualiser le site dans votre navigateur.</span><span class="sxs-lookup"><span data-stu-id="7650a-130">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="7650a-131">Récapitulatif</span><span class="sxs-lookup"><span data-stu-id="7650a-131">Summary</span></span>

<span data-ttu-id="7650a-132">ASP.NET Core apporte des modifications aux fonctionnalités ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="7650a-132">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="7650a-133">Dans cet article, vous avez vu comment migrer les fonctionnalités de gestion de l’authentification et l’utilisateur de l’identité d’ASP.NET vers ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7650a-133">In this article, you have seen how to migrate the authentication and user management features of ASP.NET Identity to ASP.NET Core.</span></span>
