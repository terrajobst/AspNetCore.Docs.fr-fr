---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: Appartenance et Administration | Microsoft Docs
author: Erikre
description: Cette série de didacticiels vous apprend les notions de base de la création d’une application Web Forms ASP.NET à l’aide de ASP.NET 4.5 et Microsoft Visual Studio Express 2013 pour nous...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: 58eda260ccf2d0f237aaade65017760ed87a8c4f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368296"
---
<a name="membership-and-administration"></a>Appartenance et Administration
====================
par [Erik Reitan](https://github.com/Erikre)

[Télécharger le projet de Wingtip Toys exemple (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [télécharger l’E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Cette série de didacticiels vous apprend les notions de base de la création d’une application Web Forms ASP.NET à l’aide de ASP.NET 4.5 et Microsoft Visual Studio Express 2013 pour le Web. Un Visual Studio 2013 [projet avec du code source c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) est disponible pour accompagner cette série de didacticiels.


Ce didacticiel vous montre comment mettre à jour de l’exemple d’application Wingtip Toys pour ajouter un rôle personnalisé et utiliser ASP.NET Identity. Il montre également comment implémenter une page d’administration à partir duquel l’utilisateur avec un rôle personnalisé peut ajouter et supprimer des produits depuis le site Web.

[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) est le système d’appartenance utilisé pour générer l’application web ASP.NET et est disponible dans ASP.NET 4.5. ASP.NET Identity est utilisée dans le modèle de projet Web Forms de Visual Studio 2013, ainsi que les modèles pour [ASP.NET MVC](../../../../mvc/index.md), [API Web ASP.NET](../../../../web-api/index.md), et [Application à Page unique ASP.NET](../../../../single-page-application/index.md). Vous pouvez également spécifiquement installer le système d’identité ASP.NET à l’aide de NuGet lorsque vous démarrez avec une application Web vide. Toutefois, dans cette série de didacticiels vous utiliser le **Web Forms**projecttemplate, qui inclut le système d’identité ASP.NET. ASP.NET Identity vous permet de facilement intégrer des données de profil d’utilisateur avec les données d’application. En outre, ASP.NET Identity vous permet de choisir le modèle de persistance pour les profils utilisateur dans votre application. Vous pouvez stocker les données dans une base de données SQL Server ou un autre magasin de données, y compris *NoSQL* stocke les données telles que les Tables de stockage Windows Azure.

Ce didacticiel s’appuie sur le didacticiel précédent intitulé « Extraction et paiement avec PayPal » dans la série de didacticiels Wingtip Toys.

## <a name="what-youll-learn"></a>Ce que vous allez apprendre :

- Comment utiliser du code pour ajouter un rôle personnalisé et un utilisateur à l’application.
- Comment restreindre l’accès à la page et le dossier d’administration.
- Guide pratique pour fournir une navigation de l’utilisateur qui appartient au rôle personnalisé.
- Comment utiliser la liaison de modèle pour remplir un [DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx) contrôle avec les catégories de produits.
- Comment charger un fichier dans l’application web à l’aide du [FileUpload](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx) contrôle.
- L’utilisation des contrôles de validation pour implémenter la validation d’entrée.
- Comment ajouter et supprimer des produits à partir de l’application.

## <a name="these-features-are-included-in-the-tutorial"></a>Ces fonctionnalités sont incluses dans le didacticiel :

- ASP.NET Identity
- Configuration et autorisation
- Liaison de modèle
- Validation non obstrusive

ASP.NET Web Forms fournit des capacités d’appartenance. En utilisant le modèle par défaut, vous avez la fonctionnalité d’appartenance intégrée que vous pouvez utiliser immédiatement lorsque l’application s’exécute. Ce didacticiel vous montre comment utiliser ASP.NET Identity pour ajouter un rôle personnalisé et affecter un utilisateur à ce rôle. Vous allez apprendre à restreindre l’accès au dossier d’administration. Vous allez ajouter une page vers le dossier d’administration qui permet à un utilisateur avec un rôle personnalisé pour ajouter et supprimer des produits et pour afficher un aperçu d’un produit une fois qu’il a été ajouté.

## <a name="adding-a-custom-role"></a>Ajout d’un rôle personnalisé

À l’aide d’ASP.NET Identity, vous pouvez ajouter un rôle personnalisé et affecter un utilisateur à ce rôle à l’aide de code.

1. Dans **l’Explorateur de solutions**, avec le bouton droit sur le *logique* dossier et créez une classe.
2. Nommez la nouvelle classe *RoleActions.cs*.
3. Modifier le code afin qu’il apparaisse comme suit :  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. Dans **l’Explorateur de solutions**, ouvrez le *Global.asax.cs* fichier.
5. Modifier le *Global.asax.cs* fichier en ajoutant le code mis en surbrillance en jaune afin qu’il apparaisse comme suit :  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. Notez que `AddUserAndRole` est souligné en rouge. Double-cliquez sur le code AddUserAndRole.  
   La lettre « A » au début de la méthode de mise en surbrillance est soulignée.
7. Placez le curseur sur la lettre « A » et cliquez sur l’interface utilisateur qui vous permet de générer un stub de méthode pour le `AddUserAndRole` (méthode). 

    ![L’appartenance et Advministration - générer le Stub de méthode](membership-and-administration/_static/image1.png)
8. Cliquez sur l’option intitulée :  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. Ouvrez le *RoleActions.cs* de fichiers à partir de la *logique* dossier.  
   Le `AddUserAndRole` méthode a été ajoutée au fichier de classe.
10. Modifier le *RoleActions.cs* fichier en supprimant le `NotImplementedeException` et en ajoutant le code mis en surbrillance en jaune, afin qu’il apparaisse comme suit :  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

Le code ci-dessus établit d’abord un contexte de base de données pour la base de données d’appartenance. La base de données d’appartenance est également stockée comme un *.mdf* de fichiers dans le *application\_données* dossier. Vous serez en mesure d’afficher cette base de données une fois que le premier utilisateur est connecté à cette application web. 

> [!NOTE] 
> 
> Si vous souhaitez stocker les données d’appartenance, ainsi que les données de produit, vous pouvez envisager d’utiliser le même **DbContext** que vous avez utilisé pour stocker les données de produit dans le code ci-dessus.


 Le *interne* mot clé est un modificateur d’accès pour les types (classes) et les membres de type (par exemple, les méthodes ou propriétés). Types internes ou les membres sont accessibles uniquement dans les fichiers contenus dans le même assembly *(.dll* fichier). Lorsque vous générez votre application, un fichier d’assembly *(.dll*) est créé qui contient le code qui est exécuté lorsque vous exécutez votre application. 

Un `RoleStore` objet, qui fournit la gestion des rôles, est créé en fonction du contexte de base de données.

> [!NOTE] 
> 
> Notez que lorsque le `RoleStore` objet est créé qu’il utilise un générique `IdentityRole` type. Cela signifie que le `RoleStore` peut uniquement contenir `IdentityRole` objets. Également par l’utilisation de génériques, les ressources en mémoire sont plus facile à gérer.


Ensuite, le `RoleManager` , est créé selon la `RoleStore` objet que vous venez de créer. le `RoleManager` objet expose rôle API associée qui peut être utilisé pour enregistrer automatiquement les modifications apportées à la `RoleStore`. Le `RoleManager` peut uniquement contenir `IdentityRole` objets, car le code utilise le `<IdentityRole>` type générique.

Vous appelez le `RoleExists` méthode pour déterminer si le rôle « canEdit » est présent dans la base de données d’appartenance. Si elle n’est pas le cas, vous créez le rôle.

Création de la `UserManager` objet semble être plus compliqué que le `RoleManager` contrôler, toutefois, il est presque le même. Il est simplement codé sur une seule ligne et non plusieurs. Ici, le paramètre que vous passez consiste à instancier en tant qu’un nouvel objet contenu dans les parenthèses.

Ensuite vous créez l’utilisateur « canEditUser » en créant un nouveau `ApplicationUser` objet. Ensuite, si vous créez avec succès de l’utilisateur, vous devez ajouter l’utilisateur au nouveau rôle.

> [!NOTE] 
> 
> La gestion des erreurs seront actualisée au cours du didacticiel « Gestion d’erreur ASP.NET » plus loin dans cette série de didacticiels.


La prochaine fois que l’application démarre, l’utilisateur nommé « canEditUser » sera ajouté en tant que le rôle nommé « canEdit » de l’application. Plus loin dans ce didacticiel, vous vous connecterez en tant que l’utilisateur « canEditUser » pour afficher les fonctionnalités supplémentaires qui vous serez ajoutés au cours de ce didacticiel. Pour plus d’informations de l’API sur l’identité d’ASP.NET, consultez le [Microsoft.AspNet.Identity Namespace](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx). Pour plus d’informations sur l’initialisation du système d’identité ASP.NET, consultez le [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs).

### <a name="restricting-access-to-the-administration-page"></a>Restreindre l’accès à la Page d’Administration

L’exemple d’application Wingtip Toys permet aux utilisateurs anonymes et les utilisateurs connectés à afficher et acheter des produits. Toutefois, l’utilisateur connecté qui a le rôle personnalisé « canEdit » peut accéder à une page restreinte afin d’ajouter et supprimer des produits.

#### <a name="add-an-administration-folder-and-page"></a>Ajouter un dossier d’Administration et de la Page

Ensuite, vous allez créer un dossier nommé *administrateur* exemple d’application pour l’utilisateur « canEditUser » appartenant au rôle personnalisé de la Wingtip Toys.

1. Cliquez sur le nom de projet (**Wingtip Toys**) dans **l’Explorateur de solutions** et sélectionnez **ajouter**  - &gt; **nouveau dossier**.
2. Nommez le nouveau dossier *administrateur*.
3. Avec le bouton droit le *administrateur* dossier, puis sélectionnez **ajouter**  - &gt; **un nouvel élément**.   
   La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
4. Sélectionnez le <strong>Visual C#</strong> - &gt; <strong>Web</strong> groupe de modèles sur la gauche. Dans la liste du milieu, sélectionnez <strong>Web Form avec Page maître</strong>, nommez-le <em>AdminPage.aspx</em><strong>,</strong> , puis sélectionnez <strong>ajouter</strong>.
5. Sélectionnez le *Site.Master* de fichiers en tant que la page maître, puis choisissez **OK**.

#### <a name="add-a-webconfig-file"></a>Ajouter un fichier Web.config

En ajoutant un *Web.config* de fichiers à la *administrateur* dossier, vous pouvez restreindre l’accès à la page contenue dans le dossier.

1. Avec le bouton droit le *administrateur* dossier et sélectionnez **ajouter**  - &gt; **un nouvel élément**.  
   La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
2. Dans la liste des modèles web Visual c#, sélectionnez <strong>fichier de Configuration Web</strong>à partir de la liste du milieu, acceptez le nom par défaut <em>Web.config</em><strong>,</strong> , puis sélectionnez <strong>Ajouter</strong>.
3. Remplacez le contenu XML existant dans le *Web.config* fichier par le code suivant :  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

Enregistrer le *Web.config* fichier. Le *Web.config* fichier Spécifie que seul l’utilisateur appartenant au rôle « canEdit » de l’application peut accéder à la page contenue dans le *administrateur* dossier.

### <a name="including-custom-role-navigation"></a>Notamment la Navigation du rôle personnalisé

Pour autoriser l’utilisateur du rôle personnalisé « canEdit » accéder à la section administration de l’application, vous devez ajouter un lien vers le *Site.Master* page. Seuls les utilisateurs qui appartiennent au rôle « canEdit » seront en mesure de voir les **administrateur** lier et accéder à la section administration.

1. Dans l’Explorateur de solutions, recherchez et ouvrez le *Site.Master* page.
2. Pour créer un lien pour l’utilisateur du rôle « canEdit », ajoutez le balisage mis en surbrillance en jaune à la liste non triée suivante `<ul>` élément afin que la liste s’affiche comme suit :  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. Ouvrez le *Site.Master.cs* fichier. Rendre le **administrateur** lien visible uniquement à l’utilisateur « canEditUser » en ajoutant le code mis en surbrillance en jaune pour le `Page_Load` gestionnaire. Le `Page_Load` gestionnaire s’affiche comme suit :   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

Lorsque la page se charge, le code vérifie si l’utilisateur connecté a le rôle de « canEdit ». Si l’utilisateur appartient au rôle « canEdit », l’élément span qui contient le lien vers le *AdminPage.aspx* page (et par conséquent le lien à l’intérieur de l’étendue) sont rendues visibles.

### <a name="enabling-product-administration"></a>L’activation de l’Administration de produit

Jusqu’ici, vous avez créé de rôle « canEdit » et ajouté un utilisateur de « canEditUser », un dossier d’administration et une page d’administration. Vous avez défini les droits d’accès pour le dossier d’administration et de la page et avez ajouté un lien de navigation de l’utilisateur du rôle « canEdit » à l’application. Ensuite, vous allez ajouter le balisage à la *AdminPage.aspx* page et le code vers le *AdminPage.aspx.cs* fichier code-behind qui vont permettre aux utilisateurs avec le rôle « canEdit » ajouter et supprimer des produits.

1. Dans **l’Explorateur de solutions**, ouvrez le *AdminPage.aspx* de fichiers à partir de la *administrateur* dossier.
2. Remplacez le balisage existant par le code suivant :  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. Ensuite, ouvrez le *AdminPage.aspx.cs* fichier code-behind en double-cliquant sur le *AdminPage.aspx* et en cliquant sur **afficher le Code**.
4. Remplacez le code existant dans le *AdminPage.aspx.cs* fichier code-behind par le code suivant :  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

Dans le code que vous avez entré pour le *AdminPage.aspx.cs* fichier code-behind, une classe appelée `AddProducts` effectue le travail de l’ajout de produits à la base de données. Cette classe n’existe pas encore, vous allez donc créer cela maintenant.

1. Dans **l’Explorateur de solutions**, avec le bouton droit le *logique* dossier, puis sélectionnez **ajouter**  - &gt; **un nouvel élément**.   
   La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
2. Sélectionnez le **Visual C#**  - &gt; **Code** groupe de modèles sur la gauche. Ensuite, sélectionnez **classe**à partir du milieu de liste et nommez-le *AddProducts.cs*.   
   Le nouveau fichier de classe s’affiche.
3. Remplacez le code existant par le code ci-dessous :  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

Le *AdminPage.aspx* page permet à l’utilisateur appartenant au rôle « canEdit » ajouter et supprimer des produits. Lors de l’ajout d’un nouveau produit, les détails sur le produit sont validées et puis entrés dans la base de données. Le nouveau produit est immédiatement disponible pour tous les utilisateurs de l’application web.

#### <a name="unobtrusive-validation"></a>Validation non obstrusive

Les détails du produit fournies par l’utilisateur sur le *AdminPage.aspx* page sont validés à l’aide de contrôles de validation (`RequiredFieldValidator` et `RegularExpressionValidator`). Ces contrôles utilisent automatiquement la validation non obstrusive. Validation non obstrusive permet aux contrôles de validation à utiliser JavaScript pour la logique de validation côté client, ce qui signifie que la page ne nécessite pas un aller-retour vers le serveur à valider. Par défaut, la validation non obstrusive est incluse dans le *Web.config* fichier basé sur le paramètre de configuration suivant :

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>Expressions régulières

Le prix du produit sur le *AdminPage.aspx* page est validée à l’aide un **RegularExpressionValidator** contrôle. Ce contrôle vérifie si la valeur du contrôle d’entrée associé (la zone de texte « AddProductPrice ») correspond au modèle spécifié par l’expression régulière. Une expression régulière est une notation de critères spéciaux qui vous permet de rapidement rechercher des modèles de caractères spécifiques. Le **RegularExpressionValidator** contrôle inclut une propriété nommée `ValidationExpression` qui contient l’expression régulière utilisée pour valider une entrée de prix, comme indiqué ci-dessous :

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>Contrôle fileUpload

Outre les contrôles d’entrée et de validation, vous avez ajouté le **FileUpload** le contrôle à la *AdminPage.aspx* page. Ce contrôle vous permet de télécharger des fichiers. Dans ce cas, vous autorisez uniquement les fichiers image à télécharger. Dans le fichier code-behind (*AdminPage.aspx.cs*), lorsque le `AddProductButton` est activé, le code vérifie les `HasFile` propriété de la **FileUpload** contrôle. Si le contrôle dispose d’un fichier et si le type de fichier (basé sur l’extension de fichier) est autorisé, l’image est enregistrée dans le *Images* dossier et le *Images/Thumbs* dossier de l’application.

#### <a name="model-binding"></a>Liaison de modèle

Plus haut dans cette série de didacticiels, vous avez utilisé la liaison de modèle pour remplir un **ListView** contrôle, un **FormsView** contrôle, un **GridView** contrôle et un  **DetailView** contrôle. Dans ce didacticiel, vous utilisez la liaison de modèle pour remplir un **DropDownList** contrôle avec une liste des catégories de produits.

Le balisage que vous avez ajouté à la *AdminPage.aspx* fichier contient un **DropDownList** contrôle appelé `DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

Liaison de modèle vous permet de remplir cette **DropDownList** en définissant le `ItemType` attribut et le `SelectMethod` attribut. Le `ItemType` attribut indique que vous utilisez le `WingtipToys.Models.Category` tapez lors du remplissage du contrôle. Définie de ce type au début de cette série de didacticiels en créant la `Category` classe (voir ci-dessous). Le `Category` classe se trouve dans le *modèles* dossier à l’intérieur de la *Category.cs* fichier.

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

Le `SelectMethod` attribut de la **DropDownList** contrôle indique que vous utilisez le `GetCategories` (méthode) (voir ci-dessous) qui est inclus dans le fichier code-behind (*AdminPage.aspx.cs*).

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

Cette méthode spécifie qu’un `IQueryable` interface est utilisée pour évaluer une requête sur un `Category` type. La valeur retournée est utilisée pour remplir le **DropDownList** dans le balisage de la page (*AdminPage.aspx*).

Le texte affiché pour chaque élément dans la liste est spécifié en définissant le `DataTextField` attribut. Le `DataTextField` attribut utilise la `CategoryName` de la `Category` classe (illustré ci-dessus) pour afficher chaque catégorie dans le **DropDownList** contrôle. La valeur réelle est transmise lorsqu’un élément est sélectionné dans le **DropDownList** contrôle est basé sur le `DataValueField` attribut. Le `DataValueField` attribut est défini sur le `CategoryID` comme définir dans la `Category` classe (illustré ci-dessus).

### <a name="how-the-application-will-work"></a>Fonctionne de l’Application

Lorsque l’utilisateur appartenant au rôle « canEdit » accède à la page pour la première fois, le `DropDownAddCategory` **DropDownList** contrôle est rempli comme décrit ci-dessus. Le `DropDownRemoveProduct` **DropDownList** contrôle est également rempli avec les produits à l’aide de la même approche. L’utilisateur appartenant au rôle « canEdit » sélectionne le type de catégorie et ajoute les détails du produit (**nom**, **Description**, **prix**, et **fichier Image**). Lorsque l’utilisateur appartenant au rôle « canEdit » clique le **ajouter un produit** bouton, le `AddProductButton_Click` Gestionnaire d’événements est déclenché. Le `AddProductButton_Click` Gestionnaire d’événements se trouve dans le fichier code-behind (*AdminPage.aspx.cs*) vérifie le fichier d’image pour vous assurer qu’il correspond aux types de fichiers autorisés *(.gif*, *.png*, *.jpeg*, ou *.jpg*). Ensuite, le fichier image est enregistré dans un dossier de l’exemple d’application Wingtip Toys. Ensuite, le nouveau produit est ajouté à la base de données. Pour ajouter un nouveau produit, une nouvelle instance de la `AddProducts` classe est créée et nommée produits. Le `AddProducts` classe a une méthode nommée `AddProduct`, et l’objet de produits appelle cette méthode pour ajouter des produits à la base de données.

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

Si le code ajoute correctement le nouveau produit à la base de données, la page est rechargée avec la valeur de chaîne de requête `ProductAction=add`.

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

Lorsque la page se recharge, la chaîne de requête est incluse dans l’URL. Rechargez la page, l’utilisateur appartenant au rôle « canEdit » peut voir immédiatement les mises à jour dans le **DropDownList** contrôle sur le *AdminPage.aspx* page. En outre, en incluant la chaîne de requête avec l’URL, la page peut afficher un message de réussite à l’utilisateur appartenant au rôle « canEdit ».

Lorsque le *AdminPage.aspx* page recharge, le `Page_Load` événement est appelé.

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

Le `Page_Load` Gestionnaire d’événements vérifie la valeur de chaîne de requête et détermine s’il faut afficher un message de réussite.

## <a name="running-the-application"></a>Exécution de l'application

Vous pouvez exécuter l’application maintenant pour voir comment vous pouvez ajouter, supprimer et les éléments de mise à jour dans le panier d’achat. Le total du panier mentionnera le coût total de tous les éléments dans le panier d’achat.

1. Dans l’Explorateur de solutions, appuyez sur **F5** pour exécuter l’exemple d’application Wingtip Toys.  
   Le navigateur s’ouvre et affiche le *Default.aspx* page.
2. Cliquez sur le **connectez-vous** lien en haut de la page. 

    ![Appartenance et Administration - connectez-vous de lien](membership-and-administration/_static/image2.png)

   Le *Login.aspx* page s’affiche.
3. Utilisez le nom d’utilisateur suivant et le mot de passe :  
   Nom d’utilisateur : canEditUser@wingtiptoys.com  
   Mot de passe : Pa$ $word1 

    ![Appartenance et Administration - Page de connexion](membership-and-administration/_static/image3.png)
4. Cliquez sur le **connectez-vous** bouton vers le bas de la page.
5. En haut de la page suivante, sélectionnez le **administrateur** lien pour accéder à la *AdminPage.aspx* page. 

    ![Appartenance et Administration - lien de l’administrateur](membership-and-administration/_static/image4.png)
6. Pour tester la validation d’entrée, cliquez sur le **ajouter un produit** bouton sans ajouter les détails du produit. 

    ![Appartenance et Administration - Page d’administration](membership-and-administration/_static/image5.png)

   Notez que les messages de champ obligatoire s’affichent.
7. Ajouter les détails d’un nouveau produit, puis cliquez sur le **ajouter un produit** bouton. 

    ![Appartenance et Administration - ajouter un produit](membership-and-administration/_static/image6.png)
8. Sélectionnez **produits** dans le menu de navigation supérieure pour afficher le nouveau produit que vous avez ajouté. 

    ![Appartenance et Administration - afficher le nouveau produit](membership-and-administration/_static/image7.png)
9. Cliquez sur le **administrateur** lien pour revenir à la page d’administration.
10. Dans le **produit Supprimer** section de la page, sélectionnez le nouveau produit que vous avez ajouté dans le **DropDownListBox**.
11. Cliquez sur le **produit Supprimer** bouton pour supprimer le nouveau produit de l’application. 

    ![Appartenance et Administration - Remove produit](membership-and-administration/_static/image8.png)
12. Sélectionnez **produits** dans le menu de navigation supérieure pour confirmer que le produit a été supprimé.
13. Cliquez sur **déconnecter** d’exister en mode administration.   
    Notez que le volet de navigation supérieure n’affiche plus le **administrateur** élément de menu.

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous ajouté un rôle personnalisé et un utilisateur appartenant au rôle personnalisé, un accès limité à la page et le dossier d’administration et fourni la navigation pour l’utilisateur appartenant au rôle personnalisé. Liaison de modèle vous permet de remplir un **DropDownList** contrôle avec des données. Vous avez implémenté le **FileUpload** contrôle et les contrôles de validation. En outre, vous avez appris comment ajouter et supprimer des produits à partir d’une base de données. Dans le didacticiel suivant, vous allez apprendre à implémenter le routage ASP.NET.

## <a name="additional-resources"></a>Ressources supplémentaires

[Web.config - authorization, élément](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[Déployer une application de formulaires Web ASP.NET sécurisée avec appartenance, OAuth et base de données SQL vers un Site Web Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - version d’évaluation gratuite](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Précédent](checkout-and-payment-with-paypal.md)
> [Suivant](url-routing.md)
