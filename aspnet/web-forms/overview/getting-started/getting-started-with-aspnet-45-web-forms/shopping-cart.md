---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
title: Panier d’achat | Microsoft Docs
author: Erikre
description: Cette série de didacticiels vous apprend les notions de base de la création d’une application Web Forms ASP.NET à l’aide de ASP.NET 4.5 et Microsoft Visual Studio Express 2013 pour nous...
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 6898c601-6c31-432f-8388-e6843f8a17cb
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
msc.type: authoredcontent
ms.openlocfilehash: 00fd00778477bea21add76c799dbfe24dfdec1dc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817000"
---
<a name="shopping-cart"></a>Panier d’achat
====================
par [Erik Reitan](https://github.com/Erikre)

[Télécharger le projet de Wingtip Toys exemple (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ou [télécharger l’E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Cette série de didacticiels vous apprend les notions de base de la création d’une application Web Forms ASP.NET à l’aide de ASP.NET 4.5 et Microsoft Visual Studio Express 2013 pour le Web. Un Visual Studio 2013 [projet avec du code source c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) est disponible pour accompagner cette série de didacticiels.


Ce didacticiel décrit la logique métier nécessaire pour ajouter un panier d’achat à l’application de Web Forms ASP.NET exemple Wingtip Toys. Ce didacticiel s’appuie sur le didacticiel précédent, « Affichage données et les détails des éléments » et fait partie de la série de didacticiels Wingtip Toys Store. Lorsque vous avez terminé ce didacticiel, les utilisateurs de votre exemple d’application sera en mesure d’ajouter, supprimer et modifier les produits dans leur panier d’achat.

## <a name="what-youll-learn"></a>Ce que vous allez apprendre :

1. Comment créer un panier d’achat pour l’application web.
2. Explique comment permettre aux utilisateurs d’ajouter des éléments au panier.
3. Comment ajouter un [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction) contrôle pour afficher les détails de panier d’achat.
4. Comment calculer et afficher le total de la commande.
5. Comment supprimer et mettre à jour des éléments dans le panier d’achat.
6. Comment inclure un compteur de panier d’achat.

## <a name="code-features-in-this-tutorial"></a>Fonctionnalités de code dans ce didacticiel :

1. Entity Framework Code First
2. Annotations de données
3. Contrôles de données fortement typés
4. Liaison de données

## <a name="creating-a-shopping-cart"></a>Création d’un panier d’achat

Plus haut dans cette série de didacticiels, vous avez ajouté les pages et le code pour afficher les données de produit à partir d’une base de données. Dans ce didacticiel, vous allez créer un panier d’achat pour gérer les produits que les utilisateurs souhaitent acheter. Les utilisateurs seront en mesure de parcourir et ajouter des éléments au panier même s’ils ne sont pas inscrits ou connectés. Pour gérer l’accès de panier d’achat, vous allez attribuer aux utilisateurs une unique `ID` à l’aide d’un identificateur global unique (GUID) lorsque l’utilisateur accède à l’achat de panier pour la première fois. Vous allez stocker ce `ID` à l’aide de l’état de Session ASP.NET.

> [!NOTE] 
> 
> L’état de Session ASP.NET est un emplacement pratique pour stocker des informations spécifiques à l’utilisateur qui va expirer après que l’utilisateur a quitté le site. Utilisation incorrecte de l’état de session peut avoir des implications en matière de performances sur les sites plus grands, compatibles utilisation de session état fonctionne bien à des fins de démonstration. L’exemple de projet de Wingtip Toys montre comment utiliser l’état de session sans un fournisseur externe, où l’état de session est stockée in-process sur le serveur web hébergeant le site. Sites de grande envergure qui fournissent plusieurs instances d’une application ou des sites qui exécutent plusieurs instances d’une application sur différents serveurs, envisagez d’utiliser **Windows Azure Cache Service**. Ce Service fournit un service de mise en cache distribué qui est externe au site web et résout le problème de l’état de session in-process. Pour plus d’informations, consultez [comment utiliser l’état de Session ASP.NET avec les Sites Web Windows Azure](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider).


### <a name="add-cartitem-as-a-model-class"></a>Ajouter CartItem comme une classe de modèle

Plus haut dans cette série de didacticiels, vous avez défini le schéma pour les données de catégorie et de produit en créant le `Category` et `Product` classes dans le *modèles* dossier. Maintenant, ajoutez une nouvelle classe pour définir le schéma du panier d’achat. Plus loin dans ce didacticiel, vous allez ajouter une classe pour gérer l’accès aux données le `CartItem` table. Cette classe fournit la logique métier pour ajouter, supprimer et mettre à jour des éléments dans le panier d’achat.

1. Avec le bouton droit le *modèles* dossier et sélectionnez **ajouter**  - &gt; **un nouvel élément**. 

    ![Panier d’achat - le nouvel élément](shopping-cart/_static/image1.png)
2. La boîte de dialogue **Ajouter un nouvel élément** s’affiche. Sélectionnez **Code**, puis sélectionnez **classe**. 

    ![Panier - ajouter la boîte de dialogue Nouvel élément](shopping-cart/_static/image2.png)
3. Nommez cette nouvelle classe *CartItem.cs*.
4. Cliquez sur **Ajouter**.  
   Le nouveau fichier de classe s’affiche dans l’éditeur.
5. Remplacez le code par défaut par le code suivant :   

    [!code-csharp[Main](shopping-cart/samples/sample1.cs)]

Le `CartItem` classe contient le schéma qui définit chaque produit un utilisateur ajoute au panier. Cette classe est semblable pour les autres classes de schéma que vous avez créé plus haut dans cette série de didacticiels. Par convention, Entity Framework Code First s’attend que la clé primaire pour la `CartItem` table sera `CartItemId` ou `ID`. Toutefois, le code substitue le comportement par défaut à l’aide de l’annotation de données `[Key]` attribut. Le `Key` attribut de la propriété ItemId Spécifie que le `ItemID` propriété est la clé primaire.

Le `CartId` propriété spécifie le `ID` de l’utilisateur qui est associé à l’élément à acheter. Vous ajouterez du code pour créer cet utilisateur `ID` lorsque l’utilisateur accède au panier d’achat. Cela `ID` sera également stocké comme une variable de Session ASP.NET.

### <a name="update-the-product-context"></a>Mettre à jour le contexte de produit

Outre l’ajout de la `CartItem` (classe), vous devez mettre à jour la classe de contexte de base de données qui gère les classes d’entité et qui fournit l’accès à la base de données. Pour ce faire, vous allez ajouter nouvellement créé `CartItem` à la classe de modèle la `ProductContext` classe.

1. Dans **l’Explorateur de solutions**, recherchez et ouvrez le *ProductContext.cs* de fichiers dans le *modèles* dossier.
2. Ajoutez le code en surbrillance à la *ProductContext.cs* de fichiers comme suit :  

    [!code-csharp[Main](shopping-cart/samples/sample2.cs?highlight=14)]

Comme mentionné précédemment dans cette série de didacticiels, le code dans le *ProductContext.cs* fichier ajoute le `System.Data.Entity` espace de noms afin que vous avez accès à toutes les fonctionnalités principales d’Entity Framework. Cette fonctionnalité inclut la possibilité d’interroger, insérer, mettre à jour et supprimer des données en travaillant avec des objets fortement typés. Le `ProductContext` ajoute l’accès vers la nouvelle classe `CartItem` classe de modèle.

### <a name="managing-the-shopping-cart-business-logic"></a>La gestion de la logique métier de panier d’achat

Ensuite, vous allez créer le `ShoppingCart` classe dans un nouveau *logique* dossier. Le `ShoppingCart` classe gère l’accès aux données le `CartItem` table. La classe inclura également la logique métier pour ajouter, supprimer et mettre à jour des éléments dans le panier d’achat.

La logique de panier d’achat que vous ajouterez contiendra les fonctionnalités pour gérer les actions suivantes :

1. Ajout d’articles au panier d’achat
2. Suppression d’éléments de panier d’achat
3. Obtention de l’ID du caddie
4. Récupération des éléments du panier d’achat
5. Un total de la quantité de tous les éléments de panier d’achat
6. La mise à jour les données de panier d’achat

Une page de panier d’achat (*ShoppingCart.aspx*) et la classe de panier d’achat est utilisée ensemble pour accéder aux données de panier d’achat. La page de panier d’achat affiche tous les éléments de que l’utilisateur ajoute au panier. Outre le shopping panier page et la classe, vous allez créer une page (*AddToCart.aspx*) pour ajouter des produits au panier. Vous allez également ajouter du code pour le *ProductList.aspx* page et le *ProductDetails.aspx* page qui fournit un lien vers le *AddToCart.aspx* page, afin que l’utilisateur peut ajouter produits de panier d’achat.

Le diagramme suivant illustre le processus de base qui se produit lorsque l’utilisateur ajoute un produit au panier.

![Panier d’achat - ajout au panier](shopping-cart/_static/image3.png)

Lorsque l’utilisateur clique sur le **Add To Cart** lien soit le *ProductList.aspx* page ou le *ProductDetails.aspx* page, l’application permet d’accéder à la *AddToCart.aspx* page, puis automatiquement à la *ShoppingCart.aspx* page. Le *AddToCart.aspx* page ajoutera le sélectionner le produit au panier en appelant une méthode dans la classe ShoppingCart. Le *ShoppingCart.aspx* page affiche les produits qui ont été ajoutées au panier.

#### <a name="creating-the-shopping-cart-class"></a>Création de la classe de panier d’achat

Le `ShoppingCart` classe est ajoutée à un dossier distinct dans l’application afin qu’il y aura une distinction claire entre le modèle (dossier Models), les pages (dossier racine) et la logique (dossier logique).

1. Dans **l’Explorateur de solutions**, avec le bouton droit le **WingtipToys**de projet et sélectionnez **ajouter**-&gt;**nouveau dossier**. Nommez le nouveau dossier *logique*.
2. Cliquez sur le *logique* dossier, puis sélectionnez **ajouter**  - &gt; **un nouvel élément**.
3. Ajoutez un nouveau fichier de classe nommé *ShoppingCartActions.cs*.
4. Remplacez le code par défaut par le code suivant :   

    [!code-csharp[Main](shopping-cart/samples/sample3.cs)]

Le `AddToCart` méthode permet à des produits individuels à inclure dans le panier d’achat en fonction du produit `ID`. Le produit est ajouté au panier d’achat, ou si le panier contient déjà un élément de ce produit, la quantité est incrémentée.

Le `GetCartId` méthode retourne le panier `ID` pour l’utilisateur. Le panier `ID` est utilisé pour suivre les éléments dont dispose un utilisateur dans leur panier d’achat. Si l’utilisateur ne dispose pas d’un panier existant `ID`, un panier d’achat nouvelle `ID` est créé pour lui. Si l’utilisateur est connecté en tant qu’un utilisateur inscrit, le panier `ID` est défini sur son nom d’utilisateur. Toutefois, si l’utilisateur n’est pas connecté, le panier `ID` est définie sur une valeur unique (GUID). Un GUID garantit que seul panier est créé pour chaque utilisateur, en fonction de la session.

Le `GetCartItems` méthode retourne une liste des articles du panier pour l’utilisateur. Plus loin dans ce didacticiel, vous verrez que la liaison de modèle est utilisée pour afficher les éléments de panier du panier d’achat à l’aide du `GetCartItems` (méthode).

### <a name="creating-the-add-to-cart-functionality"></a>Création de la fonctionnalité à ajouter au panier

Comme mentionné précédemment, vous allez créer une page de traitement nommée *AddToCart.aspx* qui sera utilisé pour ajouter de nouveaux produits pour le panier d’achat de l’utilisateur. Cette page appellera le `AddToCart` méthode dans la `ShoppingCart` classe que vous venez de créer. Le *AddToCart.aspx* page qui attend un produit `ID` lui est passé. Ce produit `ID` sera utilisé lors de l’appel le `AddToCart` méthode dans la `ShoppingCart` classe.

> [!NOTE] 
> 
> Vous allez modifier le code-behind (*AddToCart.aspx.cs*) de cette page, pas la page de l’interface utilisateur (*AddToCart.aspx*).


#### <a name="to-create-the-add-to-cart-functionality"></a>Pour créer l’ajouter au panier fonctionnalités :

1. Dans **l’Explorateur de solutions**, avec le bouton droit le **WingtipToys**du projet, cliquez sur **ajouter**  - &gt; **un nouvel élément**.  
   La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
2. Ajouter une nouvelle page standard (Web Form) à l’application nommée *AddToCart.aspx*. 

    ![Panier - ajouter un formulaire Web](shopping-cart/_static/image4.png)
3. Dans **l’Explorateur de solutions**, cliquez sur le *AddToCart.aspx* page, puis cliquez sur **afficher le Code**. Le *AddToCart.aspx.cs* fichier code-behind s’ouvre dans l’éditeur.
4. Remplacez le code existant dans le *AddToCart.aspx.cs* code-behind par le code suivant :   

    [!code-csharp[Main](shopping-cart/samples/sample4.cs)]

Lorsque le *AddToCart.aspx* page est chargée, le produit `ID` est récupéré à partir de la chaîne de requête. Ensuite, une instance de la classe de panier d’achat est créée et utilisée pour appeler le `AddToCart` méthode que vous avez ajouté précédemment dans ce didacticiel. Le `AddToCart` méthode, contenu dans le *ShoppingCartActions.cs* de fichier, inclut la logique pour ajouter le produit sélectionné le panier d’achat ou d’incrémenter la quantité de produit du produit sélectionné. Si le produit n’a pas été ajouté au panier, le produit est ajouté à la `CartItem` table de la base de données. Si le produit a déjà été ajouté au panier et que l’utilisateur ajoute un élément supplémentaire du même produit, la quantité de produit est incrémentée dans la `CartItem` table. Enfin, la page se redirige vers le *ShoppingCart.aspx* page que vous ajouterez à l’étape suivante, où l’utilisateur voit une liste mise à jour des éléments dans le panier d’achat.

Comme mentionné précédemment, un utilisateur `ID` sert à identifier les produits qui sont associés à un utilisateur spécifique. Cela `ID` est ajouté à une ligne dans le `CartItem` table chaque fois que l’utilisateur ajoute un produit au panier.

### <a name="creating-the-shopping-cart-ui"></a>Création de l’interface utilisateur de panier d’achat

Le *ShoppingCart.aspx* page affiche les produits que l’utilisateur a ajouté à leur panier d’achat. Elle fournit également la possibilité d’ajouter, supprimer et mettre à jour des éléments dans le panier d’achat.

1. Dans **l’Explorateur de solutions**, avec le bouton droit **WingtipToys**, cliquez sur **ajouter**  - &gt; **un nouvel élément**.  
   La boîte de dialogue **Ajouter un nouvel élément** s’affiche.
2. Ajoutez une nouvelle page (formulaire Web) qui inclut une page maître en sélectionnant **Web Form avec Page maître**. Nommez la nouvelle page *ShoppingCart.aspx*.
3. Sélectionnez **Site.Master** pour attacher la page maître vers le nouvel *.aspx* page.
4. Dans le *ShoppingCart.aspx* page, remplacez le balisage existant par le balisage suivant :   

    [!code-aspx[Main](shopping-cart/samples/sample5.aspx)]

Le *ShoppingCart.aspx* page inclut un **GridView** contrôle nommé `CartList`. Ce contrôle utilise la liaison de modèle pour lier les données de panier d’achat à partir de la base de données pour le **GridView** contrôle. Lorsque vous définissez la `ItemType` propriété de la **GridView** contrôler, l’expression de liaison de données `Item` est disponible dans le balisage du contrôle et le contrôle devienne fortement typé. Comme mentionné plus haut dans cette série de didacticiels, vous pouvez sélectionner les détails de la `Item` de l’objet à l’aide d’IntelliSense. Pour configurer un contrôle de données pour utiliser la liaison de modèle pour sélectionner les données, vous définissez le `SelectMethod` propriété du contrôle. Dans le balisage ci-dessus, vous définissez le `SelectMethod` à utiliser la méthode GetShoppingCartItems qui retourne une liste de `CartItem` objets. Le **GridView** contrôle de données appelle la méthode au moment opportun dans le cycle de vie de page et lie automatiquement les données retournées. Le `GetShoppingCartItems` méthode doit toujours être ajoutée.

#### <a name="retrieving-the-shopping-cart-items"></a>Récupération des éléments du panier d'

Ensuite, vous ajoutez le code à la *ShoppingCart.aspx.cs* code-behind pour extraire et remplir l’interface utilisateur de panier d’achat.

1. Dans **l’Explorateur de solutions**, cliquez sur le *ShoppingCart.aspx* page, puis cliquez sur **afficher le Code**. Le *ShoppingCart.aspx.cs* fichier code-behind s’ouvre dans l’éditeur.
2. Remplacez le code existant par le code ci-dessous :  

    [!code-csharp[Main](shopping-cart/samples/sample6.cs)]

Comme mentionné ci-dessus, le `GridView` appels de contrôle de données le `GetShoppingCartItems` méthode au moment approprié dans la durée de vie de page cycle et lie automatiquement les données retournées. Le `GetShoppingCartItems` méthode crée une instance de la `ShoppingCartActions` objet. Le code utilise ensuite cette instance pour retourner les éléments dans le panier d’achat en appelant le `GetCartItems` (méthode).

### <a name="adding-products-to-the-shopping-cart"></a>Ajout de produits au panier

Lorsque soit la *ProductList.aspx* ou *ProductDetails.aspx* page s’affiche, l’utilisateur sera en mesure d’ajouter le produit pour le panier d’achat à l’aide d’un lien. Lorsqu’ils cliquent sur le lien, l’application accède à la page de traitement nommée *AddToCart.aspx*. Le *AddToCart.aspx* page appellera le `AddToCart` méthode dans la `ShoppingCart` classe que vous avez ajouté précédemment dans ce didacticiel.

À présent, vous allez ajouter un **ajouter au panier** lien à la fois à la *ProductList.aspx* page et le *ProductDetails.aspx* page. Ce lien inclura le produit `ID` qui est récupéré à partir de la base de données.

1. Dans **l’Explorateur de solutions**, recherchez et ouvrez la page nommée *ProductList.aspx*.
2. Ajoutez le balisage mis en surbrillance en jaune pour le *ProductList.aspx* page afin que la page entière s’affiche comme suit :  

    [!code-aspx[Main](shopping-cart/samples/sample7.aspx?highlight=50-54)]

### <a name="testing-the-shopping-cart"></a>Tester le panier d’achat

Exécutez l’application pour voir comment ajouter des produits au panier.

1. Appuyez sur **F5** pour exécuter l’application.  
 Une fois le projet recrée la base de données, le navigateur s’ouvre et affiche le *Default.aspx* page.
2. Sélectionnez **voitures** dans le menu de navigation de catégorie.  
 Le *ProductList.aspx* page affiche uniquement les produits inclus dans la catégorie « Cars ». 

    ![Panier d’achat - voitures](shopping-cart/_static/image5.png)
3. Cliquez sur le **ajouter au panier** lien en regard de la première produit répertorié (la voiture convertible).   
 Le *ShoppingCart.aspx* page s’affiche, montrant la sélection dans votre panier d’achat. 

    ![Panier - panier d’achat](shopping-cart/_static/image6.png)
4. Afficher les produits supplémentaires en sélectionnant **plans** dans le menu de navigation de catégorie.
5. Cliquez sur le **ajouter au panier** lien en regard de la première produit répertorié.  
 Le *ShoppingCart.aspx* page est affichée avec l’élément supplémentaire.
6. Fermez le navigateur.

### <a name="calculating-and-displaying-the-order-total"></a>Calcul et affichage Total de la commande

Outre l’ajout de produits dans le panier d’achat, vous allez ajouter un `GetTotal` méthode à la `ShoppingCart` classe et afficher le montant total de la commande dans la page du panier.

1. Dans **l’Explorateur de solutions**, ouvrez le *ShoppingCartActions.cs* de fichiers dans le *logique* dossier.
2. Ajoutez le code suivant `GetTotal` méthode mis en surbrillance en jaune pour le `ShoppingCart` classe, afin que la classe s’affiche comme suit :   

    [!code-csharp[Main](shopping-cart/samples/sample8.cs?highlight=85-97)]

Tout d’abord, le `GetTotal` méthode obtient l’ID de panier d’achat pour l’utilisateur. La méthode obtient ensuite le panier total en multipliant le prix du produit par la quantité de produit pour chaque produit répertorié dans le panier d’achat.

> [!NOTE] 
> 
> Le code ci-dessus utilise le type nullable «`int?`». Types Nullable peuvent représenter toutes les valeurs d’un type sous-jacent et également comme une valeur null. Pour plus d’informations, consultez [à l’aide des Types Nullable](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx).


### <a name="modify-the-shopping-cart-display"></a>Modifier l’affichage de panier d’achat

Ensuite, vous allez modifier le code pour le *ShoppingCart.aspx* page pour appeler le `GetTotal` (méthode) et affichage total sur le *ShoppingCart.aspx* page lorsque la page se charge.

1. Dans **l’Explorateur de solutions**, cliquez sur le *ShoppingCart.aspx* page et sélectionnez **afficher le Code**.
2. Dans le *ShoppingCart.aspx.cs* de fichiers, mettez à jour le `Page_Load` gestionnaire en ajoutant le code suivant mis en surbrillance en jaune :   

    [!code-csharp[Main](shopping-cart/samples/sample9.cs?highlight=16-31)]

Lorsque le *ShoppingCart.aspx* page se charge, il charge l’objet panier d’achat et récupère ensuite le total du panier en appelant le `GetTotal` méthode de la `ShoppingCart` classe. Si le panier d’achat est vide, un message s’affiche à cet effet.

### <a name="testing-the-shopping-cart-total"></a>Tester le Total du panier d’achat

Exécutez maintenant l’application pour voir comment vous pouvez ajoute pas seulement un produit au panier d’achat, mais vous pouvez voir le total du panier d’achat.

1. Appuyez sur **F5** pour exécuter l’application.  
 Le navigateur s’ouvre et affiche le *Default.aspx* page.
2. Sélectionnez **voitures** dans le menu de navigation de catégorie.
3. Cliquez sur le **Add To Cart** lien en regard du produit en premier.   
 Le *ShoppingCart.aspx* page est affichée avec le total de la commande. 

    ![Panier d’achat - Total du panier](shopping-cart/_static/image7.png)
4. Ajouter d’autres produits (par exemple, un plan) au panier.
5. Le *ShoppingCart.aspx* page est affichée avec un total mis à jour pour tous les produits que vous avez ajouté. 

    ![Panier d’achat - plusieurs produits](shopping-cart/_static/image8.png)
6. Arrêter l’application en cours d’exécution en fermant la fenêtre du navigateur.

### <a name="adding-update-and-checkout-buttons-to-the-shopping-cart"></a>Ajout de mise à jour et les boutons d’extraction pour le panier d’achat

Pour autoriser les utilisateurs à modifier le panier d’achat, vous allez ajouter un **mise à jour** bouton et un **extraction** bouton à la page de panier d’achat. Le **extraction** bouton n’est pas utilisé jusqu'à ce que plus loin dans cette série de didacticiels.

1. Dans **l’Explorateur de solutions**, ouvrez le *ShoppingCart.aspx* page à la racine du projet d’application web.
2. Pour ajouter le **mise à jour** bouton et le **extraction** bouton à la *ShoppingCart.aspx* page, ajoutez le balisage mis en surbrillance en jaune au balisage, comme indiqué dans le code suivant :   

    [!code-aspx[Main](shopping-cart/samples/sample10.aspx?highlight=36-45)]

Lorsque l’utilisateur clique sur le **mise à jour** bouton, le `UpdateBtn_Click` sera appelé gestionnaire d’événements. Ce gestionnaire d’événements appelle le code que vous ajouterez à l’étape suivante.

Ensuite, vous pouvez mettre à jour le code contenu dans le *ShoppingCart.aspx.cs* fichier pour itérer sur les éléments du panier et appelez le `RemoveItem` et `UpdateItem` méthodes.

1. Dans **l’Explorateur de solutions**, ouvrez le *ShoppingCart.aspx.cs* fichier à la racine du projet d’application web.
2. Ajoutez les sections suivantes de code mis en surbrillance en jaune pour le *ShoppingCart.aspx.cs* fichier :   

    [!code-csharp[Main](shopping-cart/samples/sample11.cs?highlight=9-11,33,44-89)]

Lorsque l’utilisateur clique sur le **mise à jour** bouton sur le *ShoppingCart.aspx* page, la méthode UpdateCartItems est appelée. La méthode UpdateCartItems Obtient les valeurs mises à jour pour chaque élément dans le panier d’achat. Ensuite, la méthode UpdateCartItems appelle le `UpdateShoppingCartDatabase` (méthode) (ajouté et expliqué dans l’étape suivante) pour ajouter ou supprimer des éléments de panier d’achat. Une fois que la base de données a été mis à jour pour refléter les mises à jour le panier d’achat, le **GridView** contrôle est mis à jour sur la page du panier en appelant le `DataBind` méthode pour le **GridView**. En outre, le montant total de la commande sur la page de panier d’achat est mis à jour pour refléter la liste d’éléments de mise à jour.

### <a name="updating-and-removing-shopping-cart-items"></a>La mise à jour et suppression d’articles du panier

Sur le *ShoppingCart.aspx* page, vous pouvez voir les contrôles ont été ajoutés pour la quantité d’un élément de la mise à jour et suppression d’un élément. Maintenant, ajoutez le code qui fera ces contrôles fonctionnent.

1. Dans **l’Explorateur de solutions**, ouvrez le *ShoppingCartActions.cs* de fichiers dans le *logique* dossier.
2. Ajoutez le code suivant mis en surbrillance en jaune pour le *ShoppingCartActions.cs* fichier de classe :   

    [!code-csharp[Main](shopping-cart/samples/sample12.cs?highlight=99-213)]

Le `UpdateShoppingCartDatabase` méthode, appelée à partir de la `UpdateCartItems` méthode sur le *ShoppingCart.aspx.cs* page, contient la logique pour mettre à jour ou supprimer des éléments de panier d’achat. Le `UpdateShoppingCartDatabase` méthode effectue une itération dans toutes les lignes dans la liste de panier d’achat. Si un article du panier d’achat a été marqué pour être supprimé, ou si la quantité est inférieure à 1, le `RemoveItem` méthode est appelée. Sinon, l’élément de panier d’achat est activé pour des mises à jour lorsque le `UpdateItem` méthode est appelée. Une fois que l’article du panier d’achat a été supprimé ou mis à jour, les modifications de base de données sont enregistrées.

Le `ShoppingCartUpdates` structure est utilisée pour contenir tous les éléments du panier d’achat. Le `UpdateShoppingCartDatabase` méthode utilise le `ShoppingCartUpdates` structure pour déterminer si les éléments doivent être mis à jour ou supprimé.

Dans le didacticiel suivant, vous allez utiliser le `EmptyCart` méthode pour effacer le shopping panier après l’achat de produits. Mais pour l’instant, vous allez utiliser le `GetCount` méthode que vous venez d’ajouter à la *ShoppingCartActions.cs* fichier pour déterminer le nombre d’éléments dans le panier d’achat.

### <a name="adding-a-shopping-cart-counter"></a>Ajout d’un compteur de panier d’achat

Pour autoriser l’utilisateur d’afficher le nombre total d’éléments dans le panier d’achat, vous allez ajouter un compteur à la *Site.Master* page. Ce compteur sera également agir comme un lien vers le panier d’achat.

1. Dans **l’Explorateur de solutions**, ouvrez le *Site.Master* page.
2. Modifiez le balisage en ajoutant le lien de compteur de panier d’achat comme indiqué en jaune dans la section de navigation afin qu’il apparaisse comme suit :  

    [!code-html[Main](shopping-cart/samples/sample13.html?highlight=6)]
3. Ensuite, mettez à jour le code-behind de la *Site.Master.cs* fichier en ajoutant le code mis en surbrillance en jaune comme suit :  

    [!code-csharp[Main](shopping-cart/samples/sample14.cs?highlight=11,77-84)]

Avant de la page est rendue au format HTML, le `Page_PreRender` événement est déclenché. Dans le `Page_PreRender` gestionnaire, le nombre total de panier d’achat est déterminée en appelant le `GetCount` (méthode). La valeur retournée est ajoutée à la `cartCount` étendue inclus dans le balisage de la *Site.Master* page. Le `<span>` balises permet les éléments internes doivent être rendus correctement. Lorsque n’importe quelle page du site s’affiche, le total du panier d’achat s’affichera. L’utilisateur peut cliquer également le total du panier d’achat pour afficher le panier d’achat.

## <a name="testing-the-completed-shopping-cart"></a>Tester le panier d’achat terminé

Vous pouvez exécuter l’application maintenant pour voir comment vous pouvez ajouter, supprimer et les éléments de mise à jour dans le panier d’achat. Le total du panier mentionnera le coût total de tous les éléments dans le panier d’achat.

1. Appuyez sur **F5** pour exécuter l’application.  
 Le navigateur s’ouvre et affiche le *Default.aspx* page.
2. Sélectionnez **voitures** dans le menu de navigation de catégorie.
3. Cliquez sur le **Add To Cart** lien en regard du produit en premier.   
 Le *ShoppingCart.aspx* page est affichée avec le total de la commande.
4. Sélectionnez **plans** dans le menu de navigation de catégorie.
5. Cliquez sur le **Add To Cart** lien en regard du produit en premier.
6. Définir la quantité du premier élément dans le panier d’achat à 3, puis sélectionnez le **supprimer un élément** case à cocher du deuxième élément.<a id="a"></a>
7. Cliquez sur le **mettre à jour** bouton pour mettre à jour de la page du panier et afficher le total de la nouvelle commande. 

    ![Panier d’achat - mise à jour du panier](shopping-cart/_static/image9.png)

## <a name="summary"></a>Récapitulatif

Dans ce didacticiel, vous avez créé un panier d’achat pour l’exemple d’application Wingtip Toys Web Forms. Au cours de ce didacticiel, vous avez utilisé Entity Framework Code First, annotations de données, les contrôles de données fortement typées et liaison de modèle.

Le panier d’achat prend en charge l’ajout, la suppression et la mise à jour des éléments de l’utilisateur a sélectionné à l’achat. En plus d’implémenter les fonctionnalités de panier d’achat, vous avez appris à afficher les éléments du panier d’achat dans un **GridView** contrôler et calculer le total de la commande.

## <a name="addition-information"></a>Plus d’informations

[Vue d’ensemble de l’état de Session ASP.NET](https://msdn.microsoft.com/library/ms178581.aspx)

> [!div class="step-by-step"]
> [Précédent](display_data_items_and_details.md)
> [Suivant](checkout-and-payment-with-paypal.md)
