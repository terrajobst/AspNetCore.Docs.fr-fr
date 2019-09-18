---
title: Créer une application ASP.NET Core avec des données utilisateur protégées par une autorisation
author: rick-anderson
description: Découvrez comment créer une application Pages Razor avec des données utilisateur protégées par une autorisation. Inclut HTTPS, l’authentification, sécurité, ASP.NET Core Identity.
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: d827f6f839c9e42e6d3d7b04fe8b24a1c9732aee
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082441"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Créer une application ASP.NET Core avec des données utilisateur protégées par une autorisation

Par [Rick Anderson](https://twitter.com/RickAndMSFT) et [Joe Audette](https://twitter.com/joeaudette)

::: moniker range="<= aspnetcore-1.1"

Consultez [ce fichier PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) pour la version d’ASP.NET Core MVC. La version d’ASP.NET Core 1.1 de ce didacticiel est dans [cela](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) dossier. L’exemple ASP.NET Core 1.1 du [exemples](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Consultez [ce fichier pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

Ce didacticiel montre comment créer une application web ASP.NET Core avec des données utilisateur protégées par une autorisation. Il affiche une liste de contacts (inscrits) les utilisateurs authentifiés ont créés. Il existe trois groupes de sécurité :

* **Utilisateurs inscrits** peut afficher toutes les données approuvées et peuvent modifier ou supprimer leurs propres données.
* **Gestionnaires de** peuvent approuver ou rejeter des données de contact. Seuls les contacts approuvés sont visibles aux utilisateurs.
* **Les administrateurs** peut approuver/rejeter et modifier ou de supprimer toutes les données.

Les images de ce document ne correspondent pas exactement aux modèles les plus récents.

Dans l’image suivante, l’utilisateur Rick (`rick@example.com`) n’est connecté. Rick peut uniquement afficher les contacts approuvés et **modifier**/**supprimer**/**créer un nouveau** liens pour ses contacts. Seul le dernier enregistrement créé par Rick, affiche **modifier** et **supprimer** des liens. Autres utilisateurs ne voient le dernier enregistrement jusqu'à ce qu’un gestionnaire ou un administrateur modifie le statut « Approved ».

![Capture d’écran montrant Rick connecté](secure-data/_static/rick.png)

Dans l’image suivante, `manager@contoso.com` est connecté et dans le rôle du responsable :

![Capture d’écran manager@contoso.com connecté](secure-data/_static/manager1.png)

L’illustration suivante montre les gestionnaires de vue des détails d’un contact :

![Vue du responsable d’un contact](secure-data/_static/manager.png)

Le **approuver** et **rejeter** boutons sont affichés uniquement pour les responsables et les administrateurs.

Dans l’image suivante, `admin@contoso.com` est connecté et dans le rôle de l’administrateur :

![Capture d’écran admin@contoso.com connecté](secure-data/_static/admin.png)

L’administrateur a tous les privilèges. Elle peut lire/modifier/supprimer un contact et modifier l’état de contacts.

L’application a été créée par [la structure](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) suit `Contact` modèle :

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

L’exemple contient les gestionnaires d’autorisation suivants :

* `ContactIsOwnerAuthorizationHandler`: Garantit qu’un utilisateur peut uniquement modifier ses données.
* `ContactManagerAuthorizationHandler`: Permet aux gestionnaires d’approuver ou de rejeter les contacts.
* `ContactAdministratorsAuthorizationHandler`: Permet aux administrateurs d’approuver ou de refuser des contacts, ainsi que de modifier/supprimer des contacts.

## <a name="prerequisites"></a>Prérequis

Ce didacticiel est avancé. Vous devez être familiarisé avec :

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Authentification](xref:security/authentication/identity)
* [Confirmation de compte et récupération de mot de passe](xref:security/authentication/accconfirm)
* [Autorisation](xref:security/authorization/introduction)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>Le démarrage et l’application terminée

[Télécharger](xref:index#how-to-download-a-sample) le [terminé](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) application. [Test](#test-the-completed-app) l’application terminée afin de vous familiariser avec ses fonctionnalités de sécurité.

### <a name="the-starter-app"></a>L’application de démarrage

[Télécharger](xref:index#how-to-download-a-sample) le [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) application.

Exécutez l’application, appuyez sur la **ContactManager** lien et vérifiez que vous pouvez créer, modifier et supprimer un contact.

## <a name="secure-user-data"></a>Sécuriser les données utilisateur

Les sections suivantes ont toutes les principales étapes pour créer l’application de données utilisateur sécurisée. Il peut s’avérer utile pour faire référence au projet terminé.

### <a name="tie-the-contact-data-to-the-user"></a>Lier les données de contact à l’utilisateur

Utilisez ASP.NET [identité](xref:security/authentication/identity) ID d’utilisateur pour garantir les utilisateurs permettre modifier leurs données, mais pas d’autres données utilisateurs. Ajouter `OwnerID` et `ContactStatus` à la `Contact` modèle :

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID` est l’ID d’utilisateur à partir de la `AspNetUser` table dans le [identité](xref:security/authentication/identity) base de données. Le `Status` champ détermine si un contact est visible par les utilisateurs généraux.

Créer une nouvelle migration et mettre à jour de la base de données :

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a>Ajouter des services de rôle à l’identité

Ajouter [fonctionnalité Ajouter des rôles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) pour ajouter des services de rôle :

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a>Demander aux utilisateurs authentifiés

Définir la stratégie d’authentification par défaut pour exiger l’authentification des utilisateurs :

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 Vous pouvez refuser l’authentification au niveau de la méthode Page Razor, de contrôleur ou d’action avec la `[AllowAnonymous]` attribut. Définition de la stratégie d’authentification par défaut pour les utilisateurs doivent être authentifiés protège nouvellement ajouté les Pages Razor et les contrôleurs. Avec l’authentification requise par défaut est plus sécurisée que de s’appuyer sur de nouveaux contrôleurs et les Pages Razor pour inclure le `[Authorize]` attribut.

Ajoutez [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) aux pages d’index et de confidentialité afin que les utilisateurs anonymes puissent obtenir des informations sur le site avant de s’inscrire.

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a>Configurer le compte de test

Le `SeedData` classe crée deux comptes : administrateur et gestionnaire. Utilisez le [outil Secret Manager](xref:security/app-secrets) pour définir un mot de passe pour ces comptes. Définir le mot de passe à partir du répertoire de projet (le répertoire contenant *Program.cs*) :

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

Si un mot de passe n’est pas spécifié, une exception est levée lorsque `SeedData.Initialize` est appelée.

Mise à jour `Main` à utiliser le mot de passe de test :

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>Créez les comptes de test et de mettre à jour les contacts

Mise à jour le `Initialize` méthode dans la `SeedData` classe pour créer les comptes de test :

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

Ajouter l’ID d’utilisateur administrateur et `ContactStatus` aux contacts. Rendre des contacts « Envoyé » et un « rejeté ». Ajoutez l’ID d’utilisateur et l’état pour tous les contacts. Un seul contact est affiché :

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Créer le propriétaire, le gestionnaire et gestionnaires d’autorisations d’administrateur

Créer un `ContactIsOwnerAuthorizationHandler` classe dans le *autorisation* dossier. Le `ContactIsOwnerAuthorizationHandler` vérifie que l’utilisateur agissant sur une ressource propriétaire de la ressource.

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

Le `ContactIsOwnerAuthorizationHandler` appels [contexte. Réussir](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) si l’utilisateur authentifié actuel est le propriétaire du contact. Les gestionnaires d’autorisation généralement :

* Retourner `context.Succeed` lorsque les conditions sont remplies.
* Retourner `Task.CompletedTask` lorsque les conditions ne sont pas remplies. `Task.CompletedTask`n’est pas un succès&mdash;ou un échec. il permet l’exécution d’autres gestionnaires d’autorisations.

Si vous devez explicitement échouer, retourner [contexte. Échec](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).

L’application permet aux propriétaires de contact de modifier, supprimer ou créer leurs propres données. `ContactIsOwnerAuthorizationHandler` n’a pas besoin vérifier l’opération passée dans le paramètre de condition.

### <a name="create-a-manager-authorization-handler"></a>Créer un gestionnaire d’autorisation de gestionnaire

Créer un `ContactManagerAuthorizationHandler` classe dans le *autorisation* dossier. Le `ContactManagerAuthorizationHandler` vérifie l’utilisateur agissant sur la ressource est un gestionnaire. Seuls les responsables peuvent approuver ou rejeter les modifications de contenu (nouvelle ou modifiées).

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Créer un gestionnaire d’autorisations d’administrateur

Créer un `ContactAdministratorsAuthorizationHandler` classe dans le *autorisation* dossier. Le `ContactAdministratorsAuthorizationHandler` vérifie que l’utilisateur agissant sur la ressource est un administrateur. Administrateur peut effectuer toutes les opérations.

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Inscrire les gestionnaires d’autorisation

Services à l’aide d’Entity Framework Core doivent être inscrit pour [l’injection de dépendances](xref:fundamentals/dependency-injection) à l’aide de [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). Le `ContactIsOwnerAuthorizationHandler` utilise ASP.NET Core [identité](xref:security/authentication/identity), qui est basé sur Entity Framework Core. Enregistrer les gestionnaires avec la collection de service afin qu’elles soient disponibles pour le `ContactsController` via [l’injection de dépendances](xref:fundamentals/dependency-injection). Ajoutez le code suivant à la fin de `ConfigureServices`:

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

`ContactAdministratorsAuthorizationHandler` et `ContactManagerAuthorizationHandler` sont ajoutés en tant que singletons. Ils sont des singletons, car ils n’utilisent pas EF et toutes les informations nécessaires sont dans le `Context` paramètre de la `HandleRequirementAsync` (méthode).

## <a name="support-authorization"></a>Autorisation de la prise en charge

Dans cette section, vous mettez à jour les Pages Razor et ajoutez une classe de configuration requise des opérations.

### <a name="review-the-contact-operations-requirements-class"></a>Passez en revue la classe de configuration requise de contact des opérations

Examinez la `ContactOperations` classe. Cette classe contient les exigences de l’application prend en charge :

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a>Créer une classe de base pour les Pages Razor de Contacts

Créer une classe de base qui contient les services utilisés dans les Pages Razor de contacts. La classe de base place le code d’initialisation dans un emplacement :

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

Le code précédent :

* Ajoute le `IAuthorizationService` service d’accéder aux gestionnaires d’autorisation.
* Ajoute l’identité `UserManager` service.
* Ajoutez la `ApplicationDbContext`.

### <a name="update-the-createmodel"></a>Mettre à jour le CreateModel

Mettre à jour le constructeur de modèle de page de création à utiliser le `DI_BasePageModel` classe de base :

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

Mise à jour le `CreateModel.OnPostAsync` méthode à :

* Ajoutez l’ID utilisateur pour la `Contact` modèle.
* Appeler le Gestionnaire d’autorisation pour vérifier que l’utilisateur est autorisé à créer des contacts.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>Mettre à jour le IndexModel

Mise à jour le `OnGetAsync` méthode approuvées uniquement les contacts sont présentés aux utilisateurs généraux :

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>Mettre à jour le EditModel

Ajoutez un gestionnaire d’autorisation pour vérifier que l’utilisateur propriétaire du contact. Étant donné que l’autorisation de ressource est en cours de validation, le `[Authorize]` attribut n’est pas suffisant. L’application n’a pas accès à la ressource lors de l’évaluation des attributs. Autorisation basée sur la ressource doit être impérative. Vérifications doivent être effectuées une fois que l’application a accès à la ressource, en le chargeant dans le modèle de page ou en le chargeant dans le gestionnaire lui-même. Vous accédez fréquemment la ressource en passant la clé de ressource.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>Mettre à jour le DeleteModel

Mettre à jour le modèle de page delete pour utiliser le Gestionnaire d’autorisation pour vérifier que l’utilisateur a l’autorisation de suppression sur le contact.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>Injecter le service d’autorisation dans les vues

Actuellement, le montre l’interface utilisateur modifie et supprime des liens pour les contacts de que l’utilisateur ne peut pas modifier.

Injecter le service d’autorisation dans le fichier *pages/_ViewImports. cshtml* afin qu’il soit disponible pour tous les affichages :

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

Le balisage précédent ajoute plusieurs `using` instructions.

Mise à jour le **modifier** et **supprimer** lie dans *Pages/Contacts/Index.cshtml* afin d’être affichées uniquement pour les utilisateurs disposant des autorisations appropriées :

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> Masquage des liens à partir des utilisateurs qui ne sont pas autorisés à modifier les données ne sécuriser l’application. Masquage des liens rend l’application plus conviviale en affichant les liens ne sont valides. Les utilisateurs peuvent hack l’URL générées pour appeler modifier et supprimer des opérations sur les données qu’ils ne possèdent pas. Le contrôleur ou une Page Razor doit appliquer les vérifications d’accès pour sécuriser les données.

### <a name="update-details"></a>Détails de la mise à jour

Mettre à jour l’affichage des détails pour les responsables peuvent approuver ou rejeter des contacts :

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

Mettre à jour le modèle de page de détails :

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a>Ajouter ou supprimer un utilisateur à un rôle

Consultez [ce problème](https://github.com/aspnet/AspNetCore.Docs/issues/8502) pour plus d’informations sur :

* Suppression de privilèges à partir d’un utilisateur. Par exemple, la désactivation d’un utilisateur dans une application de conversation.
* Ajout des privilèges à un utilisateur.

## <a name="test-the-completed-app"></a>Tester l’application terminée

Si vous n’avez pas déjà défini un mot de passe pour les comptes d’utilisateur amorcée, utilisez le [outil Secret Manager](xref:security/app-secrets#secret-manager) pour définir un mot de passe :

* Choisir un mot de passe fort : Utilisez au moins huit caractères et au moins un caractère majuscule, un chiffre et un symbole. Par exemple, `Passw0rd!` répond aux exigences de mot de passe fort.
* Exécutez la commande suivante à partir du dossier du projet, où `<PW>` est le mot de passe :

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

Si l’application a des contacts :

* Supprimer tous les enregistrements dans la `Contact` table.
* Redémarrez l’application pour amorcer la base de données.

Un moyen simple de tester l’application terminée consiste à lancer les trois différents navigateurs (ou incognito/InPrivate sessions). Dans un navigateur, inscrire un nouvel utilisateur (par exemple, `test@example.com`). Connectez-vous à chaque navigateur avec un autre utilisateur. Vérifiez les opérations suivantes :

* Les utilisateurs inscrits peuvent afficher toutes les données de contact approuvées.
* Les utilisateurs inscrits peuvent modifier ou supprimer leurs propres données.
* Gestionnaires peuvent approuver/rejeter les données de contact. Le `Details` afficher montre **approuver** et **rejeter** boutons.
* Les administrateurs peuvent approuver/rejeter et modifier ou de supprimer toutes les données.

| Utilisateur                | Amorcée par l’application | Options                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | Non                | Modifier/supprimer les données propres.                |
| manager@contoso.com | Oui               | Approuver/rejeter et modifier ou de supprimer des données propres. |
| admin@contoso.com   | Oui               | Approuver/rejeter et de modifier ou de supprimer toutes les données. |

Créer un contact dans le navigateur de l’administrateur. Copiez l’URL pour supprimer et modifier à partir du contact de l’administrateur. Collez ces liens dans le navigateur de l’utilisateur de test pour vérifier que l’utilisateur de test ne peut pas effectuer ces opérations.

## <a name="create-the-starter-app"></a>Créer l’application de démarrage

* Créer une application Pages Razor nommée « ContactManager »
  * Créer l’application avec **comptes d’utilisateur individuels**.
  * Nommez-Le « ContactManager » afin de l’espace de noms correspond à l’espace de noms utilisé dans l’exemple.
  * `-uld` Spécifie la base de données locale au lieu de SQLite

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* Ajouter des *modèles/contact. cs*:

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* Une structure le `Contact` modèle.
* La migration initiale de créer et mettre à jour de la base de données :

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

Si vous rencontrez un bogue avec `dotnet aspnet-codegenerator razorpage` la commande, consultez [ce problème GitHub](https://github.com/aspnet/Scaffolding/issues/984).

* Mettez à jour l’ancre **ContactManager** dans le fichier *pages/Shared/_ Layout. cshtml* :

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* Testez l’application en création, modification et suppression d’un contact

### <a name="seed-the-database"></a>Amorcer la base de données

Ajoutez la classe [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) au dossier *Data* :

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

Appelez `SeedData.Initialize` de `Main`:

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

Tester l’application d’amorçage la base de données. S’il existe des lignes dans la base de données de contact, la méthode seed ne s’exécute pas.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

Ce didacticiel montre comment créer une application web ASP.NET Core avec des données utilisateur protégées par une autorisation. Il affiche une liste de contacts (inscrits) les utilisateurs authentifiés ont créés. Il existe trois groupes de sécurité :

* **Utilisateurs inscrits** peut afficher toutes les données approuvées et peuvent modifier ou supprimer leurs propres données.
* **Gestionnaires de** peuvent approuver ou rejeter des données de contact. Seuls les contacts approuvés sont visibles aux utilisateurs.
* **Les administrateurs** peut approuver/rejeter et modifier ou de supprimer toutes les données.

Dans l’image suivante, l’utilisateur Rick (`rick@example.com`) n’est connecté. Rick peut uniquement afficher les contacts approuvés et **modifier**/**supprimer**/**créer un nouveau** liens pour ses contacts. Seul le dernier enregistrement créé par Rick, affiche **modifier** et **supprimer** des liens. Autres utilisateurs ne voient le dernier enregistrement jusqu'à ce qu’un gestionnaire ou un administrateur modifie le statut « Approved ».

![Capture d’écran montrant Rick connecté](secure-data/_static/rick.png)

Dans l’image suivante, `manager@contoso.com` est connecté et dans le rôle du responsable :

![Capture d’écran manager@contoso.com connecté](secure-data/_static/manager1.png)

L’illustration suivante montre les gestionnaires de vue des détails d’un contact :

![Vue du responsable d’un contact](secure-data/_static/manager.png)

Le **approuver** et **rejeter** boutons sont affichés uniquement pour les responsables et les administrateurs.

Dans l’image suivante, `admin@contoso.com` est connecté et dans le rôle de l’administrateur :

![Capture d’écran admin@contoso.com connecté](secure-data/_static/admin.png)

L’administrateur a tous les privilèges. Elle peut lire/modifier/supprimer un contact et modifier l’état de contacts.

L’application a été créée par [la structure](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) suit `Contact` modèle :

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

L’exemple contient les gestionnaires d’autorisation suivants :

* `ContactIsOwnerAuthorizationHandler`: Garantit qu’un utilisateur peut uniquement modifier ses données.
* `ContactManagerAuthorizationHandler`: Permet aux gestionnaires d’approuver ou de rejeter les contacts.
* `ContactAdministratorsAuthorizationHandler`: Permet aux administrateurs d’approuver ou de refuser des contacts, ainsi que de modifier/supprimer des contacts.

## <a name="prerequisites"></a>Prérequis

Ce didacticiel est avancé. Vous devez être familiarisé avec :

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Authentification](xref:security/authentication/identity)
* [Confirmation de compte et récupération de mot de passe](xref:security/authentication/accconfirm)
* [Autorisation](xref:security/authorization/introduction)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>Le démarrage et l’application terminée

[Télécharger](xref:index#how-to-download-a-sample) le [terminé](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) application. [Test](#test-the-completed-app) l’application terminée afin de vous familiariser avec ses fonctionnalités de sécurité.

### <a name="the-starter-app"></a>L’application de démarrage

[Télécharger](xref:index#how-to-download-a-sample) le [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) application.

Exécutez l’application, appuyez sur la **ContactManager** lien et vérifiez que vous pouvez créer, modifier et supprimer un contact.

## <a name="secure-user-data"></a>Sécuriser les données utilisateur

Les sections suivantes ont toutes les principales étapes pour créer l’application de données utilisateur sécurisée. Il peut s’avérer utile pour faire référence au projet terminé.

### <a name="tie-the-contact-data-to-the-user"></a>Lier les données de contact à l’utilisateur

Utilisez ASP.NET [identité](xref:security/authentication/identity) ID d’utilisateur pour garantir les utilisateurs permettre modifier leurs données, mais pas d’autres données utilisateurs. Ajouter `OwnerID` et `ContactStatus` à la `Contact` modèle :

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID` est l’ID d’utilisateur à partir de la `AspNetUser` table dans le [identité](xref:security/authentication/identity) base de données. Le `Status` champ détermine si un contact est visible par les utilisateurs généraux.

Créer une nouvelle migration et mettre à jour de la base de données :

```dotnetcli
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a>Ajouter des services de rôle à l’identité

Ajouter [fonctionnalité Ajouter des rôles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) pour ajouter des services de rôle :

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a>Demander aux utilisateurs authentifiés

Définir la stratégie d’authentification par défaut pour exiger l’authentification des utilisateurs :

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 Vous pouvez refuser l’authentification au niveau de la méthode Page Razor, de contrôleur ou d’action avec la `[AllowAnonymous]` attribut. Définition de la stratégie d’authentification par défaut pour les utilisateurs doivent être authentifiés protège nouvellement ajouté les Pages Razor et les contrôleurs. Avec l’authentification requise par défaut est plus sécurisée que de s’appuyer sur de nouveaux contrôleurs et les Pages Razor pour inclure le `[Authorize]` attribut.

Ajouter [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) à l’Index, des pages sur et Contact pour les utilisateurs anonymes peuvent obtenir des informations sur le site avant ils s’inscrivent.

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a>Configurer le compte de test

Le `SeedData` classe crée deux comptes : administrateur et gestionnaire. Utilisez le [outil Secret Manager](xref:security/app-secrets) pour définir un mot de passe pour ces comptes. Définir le mot de passe à partir du répertoire de projet (le répertoire contenant *Program.cs*) :

```dotnetcli
dotnet user-secrets set SeedUserPW <PW>
```

Si un mot de passe n’est pas spécifié, une exception est levée lorsque `SeedData.Initialize` est appelée.

Mise à jour `Main` à utiliser le mot de passe de test :

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>Créez les comptes de test et de mettre à jour les contacts

Mise à jour le `Initialize` méthode dans la `SeedData` classe pour créer les comptes de test :

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

Ajouter l’ID d’utilisateur administrateur et `ContactStatus` aux contacts. Rendre des contacts « Envoyé » et un « rejeté ». Ajoutez l’ID d’utilisateur et l’état pour tous les contacts. Un seul contact est affiché :

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Créer le propriétaire, le gestionnaire et gestionnaires d’autorisations d’administrateur

Créez un dossier *authorization* et créez `ContactIsOwnerAuthorizationHandler` une classe dans celui-ci. Le `ContactIsOwnerAuthorizationHandler` vérifie que l’utilisateur agissant sur une ressource propriétaire de la ressource.

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

Le `ContactIsOwnerAuthorizationHandler` appels [contexte. Réussir](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) si l’utilisateur authentifié actuel est le propriétaire du contact. Les gestionnaires d’autorisation généralement :

* Retourner `context.Succeed` lorsque les conditions sont remplies.
* Retourner `Task.CompletedTask` lorsque les conditions ne sont pas remplies. `Task.CompletedTask`n’est pas un succès&mdash;ou un échec. il permet l’exécution d’autres gestionnaires d’autorisations.

Si vous devez explicitement échouer, retourner [contexte. Échec](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).

L’application permet aux propriétaires de contact de modifier, supprimer ou créer leurs propres données. `ContactIsOwnerAuthorizationHandler` n’a pas besoin vérifier l’opération passée dans le paramètre de condition.

### <a name="create-a-manager-authorization-handler"></a>Créer un gestionnaire d’autorisation de gestionnaire

Créer un `ContactManagerAuthorizationHandler` classe dans le *autorisation* dossier. Le `ContactManagerAuthorizationHandler` vérifie l’utilisateur agissant sur la ressource est un gestionnaire. Seuls les responsables peuvent approuver ou rejeter les modifications de contenu (nouvelle ou modifiées).

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Créer un gestionnaire d’autorisations d’administrateur

Créer un `ContactAdministratorsAuthorizationHandler` classe dans le *autorisation* dossier. Le `ContactAdministratorsAuthorizationHandler` vérifie que l’utilisateur agissant sur la ressource est un administrateur. Administrateur peut effectuer toutes les opérations.

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Inscrire les gestionnaires d’autorisation

Services à l’aide d’Entity Framework Core doivent être inscrit pour [l’injection de dépendances](xref:fundamentals/dependency-injection) à l’aide de [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). Le `ContactIsOwnerAuthorizationHandler` utilise ASP.NET Core [identité](xref:security/authentication/identity), qui est basé sur Entity Framework Core. Enregistrer les gestionnaires avec la collection de service afin qu’elles soient disponibles pour le `ContactsController` via [l’injection de dépendances](xref:fundamentals/dependency-injection). Ajoutez le code suivant à la fin de `ConfigureServices`:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

`ContactAdministratorsAuthorizationHandler` et `ContactManagerAuthorizationHandler` sont ajoutés en tant que singletons. Ils sont des singletons, car ils n’utilisent pas EF et toutes les informations nécessaires sont dans le `Context` paramètre de la `HandleRequirementAsync` (méthode).

## <a name="support-authorization"></a>Autorisation de la prise en charge

Dans cette section, vous mettez à jour les Pages Razor et ajoutez une classe de configuration requise des opérations.

### <a name="review-the-contact-operations-requirements-class"></a>Passez en revue la classe de configuration requise de contact des opérations

Examinez la `ContactOperations` classe. Cette classe contient les exigences de l’application prend en charge :

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a>Créer une classe de base pour les Pages Razor de Contacts

Créer une classe de base qui contient les services utilisés dans les Pages Razor de contacts. La classe de base place le code d’initialisation dans un emplacement :

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

Le code précédent :

* Ajoute le `IAuthorizationService` service d’accéder aux gestionnaires d’autorisation.
* Ajoute l’identité `UserManager` service.
* Ajoutez la `ApplicationDbContext`.

### <a name="update-the-createmodel"></a>Mettre à jour le CreateModel

Mettre à jour le constructeur de modèle de page de création à utiliser le `DI_BasePageModel` classe de base :

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

Mise à jour le `CreateModel.OnPostAsync` méthode à :

* Ajoutez l’ID utilisateur pour la `Contact` modèle.
* Appeler le Gestionnaire d’autorisation pour vérifier que l’utilisateur est autorisé à créer des contacts.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>Mettre à jour le IndexModel

Mise à jour le `OnGetAsync` méthode approuvées uniquement les contacts sont présentés aux utilisateurs généraux :

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>Mettre à jour le EditModel

Ajoutez un gestionnaire d’autorisation pour vérifier que l’utilisateur propriétaire du contact. Étant donné que l’autorisation de ressource est en cours de validation, le `[Authorize]` attribut n’est pas suffisant. L’application n’a pas accès à la ressource lors de l’évaluation des attributs. Autorisation basée sur la ressource doit être impérative. Vérifications doivent être effectuées une fois que l’application a accès à la ressource, en le chargeant dans le modèle de page ou en le chargeant dans le gestionnaire lui-même. Vous accédez fréquemment la ressource en passant la clé de ressource.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>Mettre à jour le DeleteModel

Mettre à jour le modèle de page delete pour utiliser le Gestionnaire d’autorisation pour vérifier que l’utilisateur a l’autorisation de suppression sur le contact.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>Injecter le service d’autorisation dans les vues

Actuellement, le montre l’interface utilisateur modifie et supprime des liens pour les contacts de que l’utilisateur ne peut pas modifier.

Injecter le service d’autorisation dans le *Views/_viewimports.cshtml* afin qu’il soit disponible pour toutes les vues de fichiers :

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

Le balisage précédent ajoute plusieurs `using` instructions.

Mise à jour le **modifier** et **supprimer** lie dans *Pages/Contacts/Index.cshtml* afin d’être affichées uniquement pour les utilisateurs disposant des autorisations appropriées :

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> Masquage des liens à partir des utilisateurs qui ne sont pas autorisés à modifier les données ne sécuriser l’application. Masquage des liens rend l’application plus conviviale en affichant les liens ne sont valides. Les utilisateurs peuvent hack l’URL générées pour appeler modifier et supprimer des opérations sur les données qu’ils ne possèdent pas. Le contrôleur ou une Page Razor doit appliquer les vérifications d’accès pour sécuriser les données.

### <a name="update-details"></a>Détails de la mise à jour

Mettre à jour l’affichage des détails pour les responsables peuvent approuver ou rejeter des contacts :

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

Mettre à jour le modèle de page de détails :

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a>Ajouter ou supprimer un utilisateur à un rôle

Consultez [ce problème](https://github.com/aspnet/AspNetCore.Docs/issues/8502) pour plus d’informations sur :

* Suppression de privilèges à partir d’un utilisateur. Par exemple, la désactivation d’un utilisateur dans une application de conversation.
* Ajout des privilèges à un utilisateur.

## <a name="test-the-completed-app"></a>Tester l’application terminée

Si vous n’avez pas déjà défini un mot de passe pour les comptes d’utilisateur amorcée, utilisez le [outil Secret Manager](xref:security/app-secrets#secret-manager) pour définir un mot de passe :

* Choisir un mot de passe fort : Utilisez au moins huit caractères et au moins un caractère majuscule, un chiffre et un symbole. Par exemple, `Passw0rd!` répond aux exigences de mot de passe fort.
* Exécutez la commande suivante à partir du dossier du projet, où `<PW>` est le mot de passe :

  ```dotnetcli
  dotnet user-secrets set SeedUserPW <PW>
  ```

* Supprimer et mettre à jour la base de données

  ```dotnetcli
  dotnet ef database drop -f
  dotnet ef database update  
  ```

* Redémarrez l’application pour amorcer la base de données.

Un moyen simple de tester l’application terminée consiste à lancer les trois différents navigateurs (ou incognito/InPrivate sessions). Dans un navigateur, inscrire un nouvel utilisateur (par exemple, `test@example.com`). Connectez-vous à chaque navigateur avec un autre utilisateur. Vérifiez les opérations suivantes :

* Les utilisateurs inscrits peuvent afficher toutes les données de contact approuvées.
* Les utilisateurs inscrits peuvent modifier ou supprimer leurs propres données.
* Gestionnaires peuvent approuver/rejeter les données de contact. Le `Details` afficher montre **approuver** et **rejeter** boutons.
* Les administrateurs peuvent approuver/rejeter et modifier ou de supprimer toutes les données.

| Utilisateur                | Amorcée par l’application | Options                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | Non                | Modifier/supprimer les données propres.                |
| manager@contoso.com | Oui               | Approuver/rejeter et modifier ou de supprimer des données propres. |
| admin@contoso.com   | Oui               | Approuver/rejeter et de modifier ou de supprimer toutes les données. |

Créer un contact dans le navigateur de l’administrateur. Copiez l’URL pour supprimer et modifier à partir du contact de l’administrateur. Collez ces liens dans le navigateur de l’utilisateur de test pour vérifier que l’utilisateur de test ne peut pas effectuer ces opérations.

## <a name="create-the-starter-app"></a>Créer l’application de démarrage

* Créer une application Pages Razor nommée « ContactManager »
  * Créer l’application avec **comptes d’utilisateur individuels**.
  * Nommez-Le « ContactManager » afin de l’espace de noms correspond à l’espace de noms utilisé dans l’exemple.
  * `-uld` Spécifie la base de données locale au lieu de SQLite

  ```dotnetcli
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* Ajouter des *modèles/contact. cs*:

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* Une structure le `Contact` modèle.
* La migration initiale de créer et mettre à jour de la base de données :

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* Mise à jour le **ContactManager** ancrer dans le *pages/_Layout.cshtml* fichier :

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* Testez l’application en création, modification et suppression d’un contact

### <a name="seed-the-database"></a>Amorcer la base de données

Ajouter le [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) classe à la *données* dossier.

Appelez `SeedData.Initialize` de `Main`:

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

Tester l’application d’amorçage la base de données. S’il existe des lignes dans la base de données de contact, la méthode seed ne s’exécute pas.

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>Ressources supplémentaires

* [Créer une application web .NET Core et SQL Database dans Azure App Service](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [ASP.NET Core d’autorisation Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop). Ce laboratoire présente plus en détails sur les fonctionnalités de sécurité présentées dans ce didacticiel.
* <xref:security/authorization/introduction>
* [Autorisation basée sur une stratégie personnalisée](xref:security/authorization/policies)
