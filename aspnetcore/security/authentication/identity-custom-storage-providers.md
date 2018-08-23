---
title: Fournisseurs de stockage personnalisés pour ASP.NET Core Identity
author: ardalis
description: Découvrez comment configurer les fournisseurs de stockage personnalisés pour ASP.NET Core Identity.
ms.author: riande
ms.date: 05/24/2017
uid: security/authentication/identity-custom-storage-providers
ms.openlocfilehash: 4b210a52ae9761bb838dd5611e86ce8f71345499
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837181"
---
# <a name="custom-storage-providers-for-aspnet-core-identity"></a>Fournisseurs de stockage personnalisés pour ASP.NET Core Identity

Par [Steve Smith](https://ardalis.com/)

ASP.NET Core Identity est un système extensible qui vous permet de créer un fournisseur de stockage personnalisé et le connecter à votre application. Cette rubrique décrit comment créer un fournisseur de stockage personnalisés pour ASP.NET Core Identity. Il aborde les concepts importants pour la création de votre propre fournisseur de stockage, mais n’est pas une procédure pas à pas.

[Affichez ou téléchargez un exemple depuis GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample).

## <a name="introduction"></a>Introduction

Par défaut, le système d’ASP.NET Core Identity stocke les informations utilisateur dans une base de données SQL Server à l’aide d’Entity Framework Core. Pour de nombreuses applications, cette approche fonctionne bien. Toutefois, vous pouvez utiliser un mécanisme de persistance différents ou un schéma de données. Exemple :

* Vous utilisez [stockage Table Azure](https://docs.microsoft.com/azure/storage/) ou un autre magasin de données.
* Vos tables de base de données ont une structure différente. 
* Vous pouvez utiliser une approche de l’accès des données différentes, telles que [Dapper](https://github.com/StackExchange/Dapper). 

Dans chacun de ces cas, vous pouvez écrire un fournisseur personnalisé pour votre mécanisme de stockage et connectez ce fournisseur de votre application.

ASP.NET Core Identity est inclus dans les modèles de projet dans Visual Studio avec l’option « Comptes d’utilisateur individuels ».

Lorsque vous utilisez l’interface CLI .NET Core, ajoutez `-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
```

## <a name="the-aspnet-core-identity-architecture"></a>L’architecture d’ASP.NET Core Identity

ASP.NET Core Identity se compose de classes appelée responsables et les magasins. *Gestionnaires de* sont des classes de haut niveau qui utilise le développeur d’applications pour effectuer des opérations, telles que la création d’un utilisateur de l’identité. *Magasins* sont des classes de niveau inférieur qui spécifient le mode de conservation des entités, telles que les utilisateurs et les rôles. Magasins de suivent le [modèle de référentiel](xref:fundamentals/repository-pattern) et sont étroitement couplées avec le mécanisme de persistance. Gestionnaires sont dissociés des magasins, ce qui signifie que vous pouvez remplacer le mécanisme de persistance sans modifier votre code d’application (à l’exception de configuration).

Le diagramme suivant montre comment une application web interagit avec les gestionnaires, tandis que les magasins interagissent avec la couche d’accès aux données.

![Les applications ASP.NET Core fonctionnent avec les gestionnaires (par exemple, « UserManager », « RoleManager »). Gestionnaires de travaillent avec des magasins (par exemple, « UserStore ») qui communiquent avec une Source de données à l’aide d’une bibliothèque comme Entity Framework Core.](identity-custom-storage-providers/_static/identity-architecture-diagram.png)

Pour créer un fournisseur de stockage personnalisé, créez la source de données, la couche d’accès aux données et les classes de magasin qui interagissent avec cette couche d’accès aux données (zones grises et vert dans le diagramme ci-dessus). Vous n’avez pas besoin de personnaliser les gestionnaires d’ou de votre code d’application qui interagit avec eux (les cases bleues ci-dessus).

Lorsque vous créez une nouvelle instance de `UserManager` ou `RoleManager` vous fournir le type de la classe d’utilisateur et passez une instance de la classe store en tant qu’argument. Cette approche vous permet de brancher vos classes personnalisées dans ASP.NET Core. 

[Reconfigurer l’application pour utiliser le nouveau fournisseur de stockage](#reconfigure-app-to-use-a-new-storage-provider) montre comment instancier `UserManager` et `RoleManager` avec un magasin personnalisé.

## <a name="aspnet-core-identity-stores-data-types"></a>ASP.NET Core Identity stocke des types de données

[ASP.NET Core Identity](https://github.com/aspnet/identity) types de données sont détaillées dans les sections suivantes :

### <a name="users"></a>Utilisateurs

Utilisateurs inscrits de votre site web. Le [IdentityUser](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser) type peut être étendu ou utilisé comme exemple pour votre propre type personnalisé. Vous n’avez pas besoin d’hériter d’un type particulier d’implémenter votre propre solution de stockage d’identité personnalisée.

### <a name="user-claims"></a>Revendications d’utilisateur

Un ensemble d’instructions (ou [revendications](/dotnet/api/system.security.claims.claim)) sur l’utilisateur qui représentent l’identité de l’utilisateur. Peut activer la plus grande expression d’identité de l’utilisateur que peut être obtenue via des rôles.

### <a name="user-logins"></a>Connexions utilisateur

Informations sur le fournisseur d’authentification externes (tels que Facebook ou un compte Microsoft) à utiliser lors de la connexion d’un utilisateur. [Exemple](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin)

### <a name="roles"></a>Rôles

Groupes d’autorisations pour votre site. Inclut le nom Id et le rôle du rôle (par exemple, « Admin » ou « Employee »). [Exemple](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityrole)

## <a name="the-data-access-layer"></a>La couche d’accès aux données

Cette rubrique suppose que vous êtes familiarisé avec le mécanisme de persistance que vous vous apprêtez à utiliser et comment créer des entités pour ce mécanisme. Cette rubrique ne fournit pas d’informations sur la création de référentiels ou classes d’accès aux données ; Il fournit des suggestions sur les décisions de conception lorsque vous travaillez avec ASP.NET Core Identity.

Vous avez une grande liberté lors de la conception de la couche d’accès aux données pour un fournisseur de magasin personnalisé. Vous devez uniquement créer des mécanismes de persistance pour les fonctionnalités que vous souhaitez utiliser dans votre application. Par exemple, si vous n’utilisez pas de rôles dans votre application, vous n’avez pas besoin de créer un emplacement pour les rôles ou les associations de rôle d’utilisateur. Votre technologie et l’infrastructure existante peuvent nécessiter une structure qui est très différente de l’implémentation par défaut d’ASP.NET Core Identity. Dans votre couche d’accès aux données, vous fournissez la logique à utiliser avec la structure de votre implémentation de stockage.

La couche d’accès aux données fournit la logique pour enregistrer les données à partir d’ASP.NET Core Identity à une source de données. La couche d’accès aux données pour votre fournisseur de stockage personnalisé peut inclure les classes suivantes pour stocker les informations utilisateur et le rôle.

### <a name="context-class"></a>Context (classe)

Encapsule les informations pour vous connecter à votre mécanisme de persistance et d’exécuter des requêtes. Plusieurs classes de données nécessitent une instance de cette classe, généralement fournie via l’injection de dépendances. [Exemple](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identitydbcontext-1).

### <a name="user-storage"></a>Stockage de l’utilisateur

Stocke et récupère les informations de l’utilisateur (par exemple, le hachage du nom et mot de passe utilisateur). [Exemple](/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="role-storage"></a>Stockage de rôle

Stocke et récupère les informations de rôle (par exemple, le nom du rôle). [Exemple](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1)

### <a name="userclaims-storage"></a>Stockage d’userclaims.

Stocke et récupère les informations de revendication d’utilisateur (par exemple, le type de revendication et la valeur). [Exemple](/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userlogins-storage"></a>Stockage d’userlogins.

Stocke et récupère des informations de connexion d’utilisateur (par exemple, un fournisseur d’authentification externe). [Exemple](/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

### <a name="userrole-storage"></a>Stockage du rôle d’utilisateur

Stocke et récupère les rôles sont attribués à quels utilisateurs. [Exemple](/aspnet/core/api/microsoft.aspnet.identity.corecompat.userstore-1)

**Conseil :** uniquement implémenter les classes que vous souhaitez utiliser dans votre application.

Dans les classes d’accès aux données, fournissez le code pour effectuer des opérations de données pour votre mécanisme de persistance. Par exemple, au sein d’un fournisseur personnalisé, vous pouvez avoir le code suivant pour créer un nouvel utilisateur dans le *stocker* classe :

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs?name=createuser&highlight=7)]

La logique d’implémentation pour la création de l’utilisateur est dans le `_usersTable.CreateAsync` méthode, illustrée ci-dessous.

## <a name="customize-the-user-class"></a>Personnaliser la classe d’utilisateur

Lorsque vous implémentez un fournisseur de stockage, créez une classe d’utilisateur qui est équivalente à la [ `IdentityUser` classe](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuser).

Au minimum, votre classe d’utilisateur doit inclure un `Id` et un `UserName` propriété.

Le `IdentityUser` classe définit les propriétés qui la `UserManager` appels lorsque vous effectuez les opérations demandées. Le type par défaut de la `Id` propriété est une chaîne, mais vous pouvez hériter `IdentityUser<TKey, TUserClaim, TUserRole, TUserLogin, TUserToken>` et spécifiez un autre type. Le framework attend l’implémentation de stockage pour gérer les conversions de types de données.

## <a name="customize-the-user-store"></a>Personnaliser le magasin d’utilisateurs

Créer un `UserStore` classe qui fournit les méthodes pour toutes les opérations de données sur l’utilisateur. Cette classe est équivalente à la [UserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.userstore-1) classe. Dans votre `UserStore` classe, implémentez `IUserStore<TUser>` et les interfaces facultatives requises. Vous sélectionnez les interfaces facultatives à implémenter, selon les fonctionnalités fournies dans votre application.

### <a name="optional-interfaces"></a>Interfaces facultatives

* [IUserRoleStore](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1)
* [IUserClaimStore](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1)
* [IUserPasswordStore](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1)
* [IUserSecurityStampStore](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1)
* [IUserEmailStore](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1)
* [IPhoneNumberStore](/dotnet/api/microsoft.aspnetcore.identity.iphonenumberstore-1)
* [IQueryableUserStore](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1)
* [IUserLoginStore](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1)
* [IUserTwoFactorStore](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1)
* [IUserLockoutStore](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1)

Les interfaces facultatives héritent `IUserStore<TUser>`. Vous pouvez voir un exemple partiellement implémentée d’utilisateur à stocker dans le [exemple d’application](https://github.com/aspnet/Docs/blob/master/aspnetcore/security/authentication/identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/CustomUserStore.cs).

Dans le `UserStore` (classe), vous utilisez les classes d’accès aux données que vous avez créé pour effectuer des opérations. Ceux-ci sont passées à l’aide de l’injection de dépendances. Par exemple, dans SQL Server avec une implémentation Dapper, le `UserStore` classe a le `CreateAsync` méthode qui utilise une instance de `DapperUsersTable` pour insérer un nouvel enregistrement :

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/DapperUsersTable.cs?name=createuser&highlight=7)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Interfaces à implémenter lors de la personnalisation du magasin de l’utilisateur

- **IUserStore**  
 Le [IUserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserstore-1) interface est la seule interface, vous devez implémenter dans le magasin de l’utilisateur. Il définit des méthodes pour la création, la mise à jour, la suppression et la récupération des utilisateurs.
- **IUserClaimStore**  
 Le [IUserClaimStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserclaimstore-1) interface définit les méthodes que vous implémentez pour activer les revendications d’utilisateur. Il contient des méthodes pour ajouter, supprimer et récupérer des revendications d’utilisateur.
- **IUserLoginStore**  
 Le [IUserLoginStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserloginstore-1) définit les méthodes que vous implémentez pour activer les fournisseurs d’authentification externes. Il contient des méthodes pour ajouter, supprimer et récupérer les connexions utilisateur et une méthode pour récupérer un utilisateur selon les informations de connexion.
- **IUserRoleStore**  
 Le [IUserRoleStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserrolestore-1) interface définit les méthodes que vous implémentez pour mapper un utilisateur à un rôle. Il contient des méthodes pour ajouter, supprimer et récupérer des rôles d’un utilisateur et une méthode pour vérifier si un utilisateur est affecté à un rôle.
- **IUserPasswordStore**  
 Le [IUserPasswordStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserpasswordstore-1) interface définit les méthodes que vous implémentez pour conserver les mots de passe hachés. Il contient des méthodes pour obtenir et définir le mot de passe haché et une méthode qui indique si l’utilisateur a défini un mot de passe.
- **IUserSecurityStampStore**  
 Le [IUserSecurityStampStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iusersecuritystampstore-1) interface définit les méthodes que vous implémentez pour utiliser un tampon de sécurité permettant d’indiquer si les informations de compte de l’utilisateur a changé. Cet horodatage est mis à jour quand un utilisateur modifie le mot de passe, ou ajoute ou supprime des connexions. Il contient des méthodes pour obtenir et définir le tampon de sécurité.
- **IUserTwoFactorStore**  
 Le [IUserTwoFactorStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iusertwofactorstore-1) interface définit les méthodes que vous implémentez pour prendre en charge l’authentification à deux facteurs. Il contient des méthodes pour obtenir et définir si l’authentification à deux facteurs est activée pour un utilisateur.
- **IUserPhoneNumberStore**  
 Le [IUserPhoneNumberStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserphonenumberstore-1) interface définit les méthodes que vous implémentez pour stocker les numéros de téléphone d’utilisateur. Il contient des méthodes pour obtenir et définir le numéro de téléphone et indique si le numéro de téléphone est confirmé.
- **IUserEmailStore**  
 Le [IUserEmailStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuseremailstore-1) interface définit les méthodes que vous implémentez pour stocker les adresses e-mail des utilisateurs. Il contient des méthodes pour obtenir et définir l’adresse de messagerie et si l’adresse de messagerie est confirmée.
- **IUserLockoutStore**  
 Le [IUserLockoutStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iuserlockoutstore-1) interface définit les méthodes que vous implémentez pour stocker des informations sur le verrouillage d’un compte. Il contient des méthodes pour le suivi des tentatives d’accès ayant échoué et de verrouillages.
- **IQueryableUserStore**  
 Le [IQueryableUserStore&lt;TUser&gt; ](/dotnet/api/microsoft.aspnetcore.identity.iqueryableuserstore-1) interface définit les membres que vous implémentez pour fournir un magasin d’utilisateurs utilisable.

Vous implémentez uniquement les interfaces qui sont nécessaires dans votre application. Exemple :

```csharp
public class UserStore : IUserStore<IdentityUser>,
                         IUserClaimStore<IdentityUser>,
                         IUserLoginStore<IdentityUser>,
                         IUserRoleStore<IdentityUser>,
                         IUserPasswordStore<IdentityUser>,
                         IUserSecurityStampStore<IdentityUser>
{
    // interface implementations not shown
}
```

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim, IdentityUserLogin et IdentityUserRole

Le `Microsoft.AspNet.Identity.EntityFramework` espace de noms contient des implémentations de la [IdentityUserClaim](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserclaim-1), [IdentityUserLogin](/aspnet/core/api/microsoft.aspnet.identity.corecompat.identityuserlogin), et [IdentityUserRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuserrole-1) classes. Si vous utilisez ces fonctionnalités, vous souhaiterez créer vos propres versions de ces classes et de définir les propriétés de votre application. Toutefois, il est parfois plus efficace de ne charger pas ces entités en mémoire lors de l’exécution des opérations de base (par exemple, ajout ou suppression de revendication de l’utilisateur). Au lieu de cela, les classes de magasin principal peuvent exécuter ces opérations directement sur la source de données. Par exemple, le `UserStore.GetClaimsAsync` méthode peut appeler le `userClaimTable.FindByUserId(user.Id)` méthode pour exécuter une requête sur table directement et retourner une liste de revendications.

## <a name="customize-the-role-class"></a>Personnaliser la classe de rôle

Lorsque vous implémentez un fournisseur de stockage de rôle, vous pouvez créer un type de rôle personnalisé. Il ne doive pas implémenter une interface particulière, mais il doit avoir un `Id` et il aura généralement un `Name` propriété.

Voici un exemple de classe de rôle :

[!code-csharp[](identity-custom-storage-providers/sample/CustomIdentityProviderSample/CustomProvider/ApplicationRole.cs)]

## <a name="customize-the-role-store"></a>Personnaliser le magasin de rôles

Vous pouvez créer un `RoleStore` classe qui fournit les méthodes pour toutes les opérations de données sur les rôles. Cette classe est équivalente à la [RoleStore&lt;TRole&gt; ](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.rolestore-1) classe. Dans le `RoleStore` (classe), vous implémentez le `IRoleStore<TRole>` et éventuellement le `IQueryableRoleStore<TRole>` interface.

- **IRoleStore&lt;TRole&gt;**  
 Le [IRoleStore&lt;TRole&gt; ](/dotnet/api/microsoft.aspnetcore.identity.irolestore-1) interface définit les méthodes à implémenter dans la classe de magasin de rôle. Il contient des méthodes pour la création, la mise à jour, la suppression et la récupération des rôles.
- **RoleStore&lt;TRole&gt;**  
 Pour personnaliser `RoleStore`, créez une classe qui implémente le `IRoleStore<TRole>` interface. 

## <a name="reconfigure-app-to-use-a-new-storage-provider"></a>Reconfigurer l’application pour utiliser un nouveau fournisseur de stockage

Une fois que vous avez implémenté un fournisseur de stockage, vous configurez votre application pour l’utiliser. Si votre application utilisé le fournisseur par défaut, remplacez-le par votre fournisseur personnalisé.

1. Supprimer le `Microsoft.AspNetCore.EntityFramework.Identity` package NuGet.
1. Si le fournisseur de stockage se trouve dans un projet distinct ou un package, ajoutez une référence à celui-ci.
1. Remplacez toutes les références à `Microsoft.AspNetCore.EntityFramework.Identity` avec un à l’aide de l’instruction pour l’espace de noms de votre fournisseur de stockage.
1. Dans le `ConfigureServices` méthode, change la `AddIdentity` méthode à utiliser vos types personnalisés. Vous pouvez créer vos propres méthodes d’extension à cet effet. Consultez [IdentityServiceCollectionExtensions](https://github.com/aspnet/Identity/blob/rel/1.1.0/src/Microsoft.AspNetCore.Identity/IdentityServiceCollectionExtensions.cs) pour obtenir un exemple.
1. Si vous utilisez des rôles, mettre à jour le `RoleManager` à utiliser votre `RoleStore` classe.
1. Mettre à jour la chaîne de connexion et les informations d’identification pour la configuration de votre application.

Exemple :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add identity types
    services.AddIdentity<ApplicationUser, ApplicationRole>()
        .AddDefaultTokenProviders();

    // Identity Services
    services.AddTransient<IUserStore<ApplicationUser>, CustomUserStore>();
    services.AddTransient<IRoleStore<ApplicationRole>, CustomRoleStore>();
    string connectionString = Configuration.GetConnectionString("DefaultConnection");
    services.AddTransient<SqlConnection>(e => new SqlConnection(connectionString));
    services.AddTransient<DapperUsersTable>();

    // additional configuration
}
```

## <a name="references"></a>Références

- [Fournisseurs de stockage personnalisés pour ASP.NET 4.x Identity](/aspnet/identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity)
- [ASP.NET Core Identity](https://github.com/aspnet/identity) -ce référentiel inclut des liens vers la Communauté gérée des fournisseurs de magasins.
