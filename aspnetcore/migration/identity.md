---
title: Migrer l’authentification et l’identité vers ASP.NET Core
author: ardalis
description: Découvrez comment migrer l’authentification et l’identité d’un projet MVC ASP.NET vers un projet ASP.NET Core MVC.
ms.author: riande
ms.date: 10/14/2016
uid: migration/identity
ms.openlocfilehash: f821930dbd36de18db31104cddf34c563009a506
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661853"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core"></a><span data-ttu-id="b46e0-103">Migrer l’authentification et l’identité vers ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b46e0-103">Migrate Authentication and Identity to ASP.NET Core</span></span>

<span data-ttu-id="b46e0-104">Par [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="b46e0-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="b46e0-105">Dans l’article précédent, nous avons [migré la configuration d’un projet mvc ASP.net vers ASP.net Core MVC](xref:migration/configuration).</span><span class="sxs-lookup"><span data-stu-id="b46e0-105">In the previous article, we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/configuration).</span></span> <span data-ttu-id="b46e0-106">Dans cet article, nous migrons les fonctionnalités d’inscription, de connexion et de gestion des utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="b46e0-106">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="b46e0-107">Configurer l’identité et l’appartenance</span><span class="sxs-lookup"><span data-stu-id="b46e0-107">Configure Identity and Membership</span></span>

<span data-ttu-id="b46e0-108">Dans ASP.NET MVC, les fonctionnalités d’authentification et d’identité sont configurées à l’aide de ASP.NET Identity dans *Startup.auth.cs* et *IdentityConfig.cs*, situées dans le dossier *App_Start* .</span><span class="sxs-lookup"><span data-stu-id="b46e0-108">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in *Startup.Auth.cs* and *IdentityConfig.cs*, located in the *App_Start* folder.</span></span> <span data-ttu-id="b46e0-109">Dans ASP.NET Core MVC, ces fonctionnalités sont configurées dans *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="b46e0-109">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="b46e0-110">Installez les packages NuGet `Microsoft.AspNetCore.Identity.EntityFrameworkCore` et `Microsoft.AspNetCore.Authentication.Cookies`.</span><span class="sxs-lookup"><span data-stu-id="b46e0-110">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="b46e0-111">Ensuite, ouvrez *Startup.cs* et mettez à jour la méthode `Startup.ConfigureServices` pour utiliser Entity Framework et les services d’identité :</span><span class="sxs-lookup"><span data-stu-id="b46e0-111">Then, open *Startup.cs* and update the `Startup.ConfigureServices` method to use Entity Framework and Identity services:</span></span>

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

<span data-ttu-id="b46e0-112">À ce stade, il existe deux types référencés dans le code ci-dessus, que nous n’avons pas encore migrés à partir du projet MVC ASP.NET : `ApplicationDbContext` et `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="b46e0-112">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="b46e0-113">Créez un nouveau dossier *Models* dans le projet ASP.net Core et ajoutez-y deux classes correspondant à ces types.</span><span class="sxs-lookup"><span data-stu-id="b46e0-113">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="b46e0-114">Les versions ASP.NET MVC de ces classes sont disponibles dans */Models/IdentityModels.cs*, mais nous allons utiliser un fichier par classe dans le projet migré, car cela est plus clair.</span><span class="sxs-lookup"><span data-stu-id="b46e0-114">You will find the ASP.NET MVC versions of these classes in */Models/IdentityModels.cs*, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="b46e0-115">*ApplicationUser.cs*:</span><span class="sxs-lookup"><span data-stu-id="b46e0-115">*ApplicationUser.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="b46e0-116">*ApplicationDbContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="b46e0-116">*ApplicationDbContext.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
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

<span data-ttu-id="b46e0-117">Le projet Web de démarrage de ASP.NET Core MVC n’inclut pas de nombreuses personnalisations des utilisateurs ou des `ApplicationDbContext`.</span><span class="sxs-lookup"><span data-stu-id="b46e0-117">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the `ApplicationDbContext`.</span></span> <span data-ttu-id="b46e0-118">Lors de la migration d’une application réelle, vous devez également migrer toutes les propriétés et méthodes personnalisées des classes utilisateur et `DbContext` de votre application, ainsi que toute autre classe de modèle utilisée par votre application.</span><span class="sxs-lookup"><span data-stu-id="b46e0-118">When migrating a real app, you also need to migrate all of the custom properties and methods of your app's user and `DbContext` classes, as well as any other Model classes your app utilizes.</span></span> <span data-ttu-id="b46e0-119">Par exemple, si votre `DbContext` a un `DbSet<Album>`, vous devez migrer la classe `Album`.</span><span class="sxs-lookup"><span data-stu-id="b46e0-119">For example, if your `DbContext` has a `DbSet<Album>`, you need to migrate the `Album` class.</span></span>

<span data-ttu-id="b46e0-120">Une fois ces fichiers mis en place, le fichier *Startup.cs* peut être compilé en mettant à jour ses instructions `using` :</span><span class="sxs-lookup"><span data-stu-id="b46e0-120">With these files in place, the *Startup.cs* file can be made to compile by updating its `using` statements:</span></span>

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

<span data-ttu-id="b46e0-121">Notre application est maintenant prête à prendre en charge l’authentification et les services d’identité.</span><span class="sxs-lookup"><span data-stu-id="b46e0-121">Our app is now ready to support authentication and Identity services.</span></span> <span data-ttu-id="b46e0-122">Ces fonctionnalités doivent simplement être exposées aux utilisateurs.</span><span class="sxs-lookup"><span data-stu-id="b46e0-122">It just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="b46e0-123">Migrer la logique de connexion et d’inscription</span><span class="sxs-lookup"><span data-stu-id="b46e0-123">Migrate registration and login logic</span></span>

<span data-ttu-id="b46e0-124">Avec les services d’identité configurés pour l’application et l’accès aux données configurés à l’aide de Entity Framework et SQL Server, nous sommes prêts à ajouter la prise en charge de l’inscription et de la connexion à l’application.</span><span class="sxs-lookup"><span data-stu-id="b46e0-124">With Identity services configured for the app and data access configured using Entity Framework and SQL Server, we're ready to add support for registration and login to the app.</span></span> <span data-ttu-id="b46e0-125">Rappelez-vous qu' [auparavant dans le processus de migration](xref:migration/mvc#migrate-the-layout-file) , nous avons commenté une référence à *_LoginPartial* dans *_Layout. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b46e0-125">Recall that [earlier in the migration process](xref:migration/mvc#migrate-the-layout-file) we commented out a reference to *_LoginPartial* in *_Layout.cshtml*.</span></span> <span data-ttu-id="b46e0-126">Il est maintenant temps de revenir à ce code, de supprimer les marques de commentaire et d’ajouter les contrôleurs et les vues nécessaires pour prendre en charge la fonctionnalité de connexion.</span><span class="sxs-lookup"><span data-stu-id="b46e0-126">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="b46e0-127">Supprimez les marques de commentaire de la ligne de `@Html.Partial` dans *_Layout. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b46e0-127">Uncomment the `@Html.Partial` line in *_Layout.cshtml*:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="b46e0-128">À présent, ajoutez une nouvelle vue Razor appelée *_LoginPartial* au dossier *Views/Shared* :</span><span class="sxs-lookup"><span data-stu-id="b46e0-128">Now, add a new Razor view called *_LoginPartial* to the *Views/Shared* folder:</span></span>

<span data-ttu-id="b46e0-129">Mettez à jour *_LoginPartial. cshtml* avec le code suivant (remplacez tout son contenu) :</span><span class="sxs-lookup"><span data-stu-id="b46e0-129">Update *_LoginPartial.cshtml* with the following code (replace all of its contents):</span></span>

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

<span data-ttu-id="b46e0-130">À ce stade, vous devez être en mesure d’actualiser le site dans votre navigateur.</span><span class="sxs-lookup"><span data-stu-id="b46e0-130">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="b46e0-131">Résumé</span><span class="sxs-lookup"><span data-stu-id="b46e0-131">Summary</span></span>

<span data-ttu-id="b46e0-132">ASP.NET Core introduit les modifications apportées aux fonctionnalités de ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="b46e0-132">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="b46e0-133">Dans cet article, vous avez vu comment migrer les fonctionnalités d’authentification et de gestion des utilisateurs de ASP.NET Identity vers ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b46e0-133">In this article, you have seen how to migrate the authentication and user management features of ASP.NET Identity to ASP.NET Core.</span></span>
