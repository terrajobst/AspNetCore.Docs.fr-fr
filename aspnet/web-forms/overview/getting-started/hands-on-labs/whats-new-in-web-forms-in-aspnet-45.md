---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: Quelles sont les nouveautés dans Web Forms dans ASP.NET 4.5 | Microsoft Docs
author: rick-anderson
description: La nouvelle version de Web Forms ASP.NET introduit plusieurs améliorations axées sur l’amélioration de l’expérience utilisateur lorsque vous travaillez avec des données. Dans les versions précédentes de...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: f36c50b64ed2363ba648297a1424b638bf9d4af5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830410"
---
<a name="whats-new-in-web-forms-in-aspnet-45"></a>Quelles sont les nouveautés dans Web Forms dans ASP.NET 4.5
====================
par [Web Camps Team](https://twitter.com/webcamps)

> La nouvelle version de Web Forms ASP.NET introduit plusieurs améliorations axées sur l’amélioration de l’expérience utilisateur lorsque vous travaillez avec des données.
> 
> Dans les versions précédentes des Web Forms, lors de l’utilisation de la liaison de données pour émettre la valeur d’un membre d’objet, vous avez utilisé les expressions de liaison de données Bind() ou Eval(). Dans la nouvelle version d’ASP.NET, vous êtes en mesure de déclarer le type de données un contrôle va être lié à l’aide d’une nouvelle propriété ItemType. Définition de cette propriété vous permettra d’utiliser une variable fortement typée pour profiter pleinement des avantages de l’expérience de développement Visual Studio, telles que IntelliSense, navigation de membre et la vérification au moment de la compilation.
> 
> Avec les contrôles liés aux données, vous pouvez maintenant également spécifier vos propres méthodes personnalisées pour la sélection, la mise à jour, suppression et insertion de données, ce qui simplifie l’interaction entre les contrôles de page et votre logique d’application. En outre, les fonctionnalités de liaison de modèle ont été ajoutées à ASP.NET, ce qui signifie que vous pouvez mapper des données à partir de la page directement dans les paramètres de type de méthode.
> 
> Validation des entrées utilisateur doit-elle également être plus facile avec la dernière version de Web Forms. Vous pouvez désormais annoter vos classes de modèle avec des attributs de validation à partir de la **System.ComponentModel.DataAnnotations** espace de noms et demande tous les sites de votre contrôle valident l’entrée à l’aide de ces informations utilisateur. Validation côté client dans Web Forms est désormais intégrée avec jQuery, auquel le nettoyeur le code côté client et des fonctionnalités de JavaScript discrets.
> 
> Dans la zone de validation de demande, des améliorations ont été apportées pour le rendre plus facile de désactiver la validation de demande de certaines parties de vos applications ou de lire les données de demande non valide de manière sélective.
> 
> Des améliorations ont été apportées à Web Forms des contrôles de serveur pour tirer parti des nouvelles fonctionnalités d’HTML5 :
> 
> - La propriété TextMode du contrôle zone de texte a été mis à jour pour prendre en charge les nouveaux types d’entrées HTML5 comme les e-mails, datetime et ainsi de suite.
> - Du contrôle FileUpload prend désormais en charge plusieurs chargements de fichiers à partir de navigateurs qui prennent en charge cette fonctionnalité HTML5.
> - Validateur contrôle désormais prise en charge lors de la validation d’entrée éléments HTML5.
> - Prise en charge de nouveaux éléments HTML5 qui ont des attributs qui représentent une URL maintenant runat =&quot;server&quot;. Par conséquent, vous pouvez utiliser les conventions ASP.NET dans les chemins d’URL, comme le ~ opérateur pour représenter la racine de l’application (par exemple, &lt;vidéo runat =&quot;server&quot; src =&quot;~/myVideo.wmv&quot; &gt; &lt;/vidéo&gt;).
> - Le contrôle UpdatePanel a été résolu pour prendre en charge des champs d’entrée de validation HTML5.
> 
> Vous trouverez plus d’exemples des nouvelles fonctionnalités dans ASP.NET WebForms 4.5 dans le portail ASP.NET officiels : [What ' s New in ASP.NET 4.5 et Visual Studio 2012](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)
> 
> Tous les exemples de code et extraits de code sont inclus dans le Kit de formation Camps Web, disponible à l’adresse [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Objectifs

Dans cet atelier, vous allez apprendre comment :

- Utiliser des expressions de liaison de données fortement typées
- Utiliser les nouvelles fonctionnalités de liaison de modèle dans Web Forms
- Utiliser des fournisseurs de valeurs pour le mappage des données de page aux méthodes de code-behind
- Utiliser des Annotations de données pour la validation d’entrée utilisateur
- Prendre advange d’unobstrusive la validation côté client avec jQuery dans les Web Forms
- Implémenter la validation de demande granulaire
- Implémenter une page asynchrone de traitement dans Web Forms

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prérequis

Vous devez disposer des éléments suivants pour terminer ce laboratoire :

- [Microsoft Visual Studio Express 2012 pour Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou supérieure (lire [annexe A](#AppendixA) pour obtenir des instructions sur la façon d’installer).

<a id="Setup"></a>
### <a name="setup"></a>Installation

**L’installation des extraits de Code**

Pour des raisons pratiques, une grande partie du code que vous gérez le long de ce laboratoire est disponible en tant que les extraits de code Visual Studio. Pour installer les extraits de code exécuter **.\Source\Setup\CodeSnippets.vsi** fichier.

Si vous n’êtes pas familiarisé avec les extraits de Code Visual Studio et que vous souhaitiez savoir comment les utiliser, vous pouvez faire référence à l’annexe de ce document &quot; [extraits de Code à l’aide d’annexe c :](#AppendixC)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Exercices

Ce laboratoire pratique inclut les exercices suivants :

1. [Exercice 1 : Liaison de données dans ASP.NET Web Forms](#Exercise1)
2. [Exercice 2 : Validation des données](#Exercise2)
3. [Exercice 3 : Page asynchrone dans ASP.NET Web Forms](#Exercise3)

> [!NOTE]
> Chaque exercice est accompagné par un **fin** dossier contenant la solution obtenue, vous devez obtenir après avoir effectué les exercices. Si vous avez besoin d’aide supplémentaire sur l’utilisation via les exercices, vous pouvez utiliser cette solution comme guide.


Durée estimée pour effectuer ce laboratoire : **60 minutes**.

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a>Exercice 1 : Liaison de données dans ASP.NET Web Forms

La nouvelle version de Web Forms ASP.NET introduit de nombreuses améliorations axées sur l’amélioration de l’expérience lorsque vous travaillez avec des données. Tout au long de cet exercice, vous allez en savoir plus sur les contrôles de données fortement typées et liaison de modèle.

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a>Tâche 1 : utilisation des liaisons de données fortement typées

Dans cette tâche, vous allez découvrir les nouvelles fortement typée liaisons disponibles dans ASP.NET 4.5.

1. Ouvrez le **commencer** solution situé dans **/Ex1-ModelBinding/début du fichierSource/** dossier.

   1. Vous devrez télécharger certains packages NuGet manquants avant de continuer. Pour ce faire, cliquez sur le **projet** menu et sélectionnez **gérer les Packages NuGet**.
   2. Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.

      > [!NOTE]
      > Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ouvrez le **Customers.aspx** page. Placer une liste non numérotée dans le contrôle principal et inclure un contrôle repeater à l’intérieur pour répertorier chaque client. La valeur est le nom du répéteur **customersRepeater** comme indiqué dans le code suivant.

    Dans les versions précédentes des Web Forms, lors de l’utilisation de la liaison de données pour émettre la valeur d’un membre sur un objet vous êtes de liaison de données, vous utiliseriez une expression de liaison de données, ainsi que d’un appel à la méthode Eval, en passant le nom du membre en tant que chaîne.

    Lors de l’exécution, ces appels à Eval utilise la réflexion par rapport à l’objet actuellement lié pour lire la valeur du membre avec le nom donné et afficher le résultat dans le code HTML. Cette approche rend très facile de lier les données par rapport aux données arbitraires, unshaped.

    Malheureusement, vous perdez de nombreuses fonctionnalités de bons résultats lors du développement dans Visual Studio, notamment IntelliSense pour les noms de membres, la prise en charge de navigation (par exemple, atteindre la définition) et la vérification de la compilation.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. Ouvrez le **Customers.aspx.cs** fichier.
4. Ajoutez le code suivant à l’aide d’instruction.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. Dans le **Page\_charge** (méthode), ajoutez le code pour remplir le répéteur avec la liste des clients.

    (Code Snippet - *Web Source de données des clients Forms Lab - Ex01 - liaison*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    La solution utilise Entity Framework avec CodeFirst pour créer et accéder à la base de données. Dans le code suivant, le customersRepeater est liée à une requête matérialisée qui retourne tous les clients à partir de la base de données.
6. Appuyez sur **F5** pour exécuter la solution, passez à la **clients** page pour voir le repeater en action. Comme la solution est à l’aide de CodeFirst, la base de données sera créée et remplie dans votre instance locale de SQL Express lors de l’exécution de l’application.

    ![Répertorier les clients avec un répéteur](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "répertoriant les clients avec un répéteur")

    *Répertorier les clients avec un répéteur*

    > [!NOTE]
    > Dans Visual Studio 2012, IIS Express est le serveur de développement Web par défaut.
7. Fermez le navigateur et revenez à Visual Studio.
8. Maintenant remplacer l’implémentation pour utiliser des liaisons fortement typées. Ouvrez le **Customers.aspx** page et utiliser la nouvelle **ItemType** attribut dans le répéteur pour définir le **client** type que le type de liaison.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    La propriété ItemType vous permet de déclarer le type de données le contrôle doit être lié à et vous permet d’utiliser-liaison fortement typées à l’intérieur du contrôle lié aux données.
9. Remplacez l’ItemTemplate contenu par le code suivant.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    Un inconvénient avec les approches ci-dessus est que les appels à Eval() et Bind() sont à liaison tardive - ce qui signifie que vous passez des chaînes pour représenter les noms de propriété. Cela signifie que vous n’obtenez pas Intellisense pour les noms de membres, prise en charge de navigation dans le code (par exemple, atteindre la définition), ni prise en charge de la vérification de la compilation.

    La définition de la propriété ItemType entraîne deux nouvelles variables typées doit être généré dans l’étendue des expressions de liaison de données : **élément** et **BindItem**. Vous pouvez utiliser ces variables fortement typées dans les expressions de liaison de données et obtenir le meilleur parti de l’expérience de développement Visual Studio.

    Le &quot; **:** &quot; utilisé dans l’expression sera automatiquement encoder en HTML la sortie pour éviter les problèmes de sécurité (par exemple, les scripts intersites). Cette notation n’était disponible depuis .NET 4 pour l’écriture de réponse, mais il est également disponible dans les expressions de liaison de données.

    > [!NOTE]
    > Le membre de l’élément fonctionne pour la liaison unidirectionnelle. Si vous souhaitez effectuer une liaison bidirectionnelle utilisation le **BindItem** membre.

    ![Prise en charge d’IntelliSense dans la liaison fortement typée](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "prise en charge d’IntelliSense dans la liaison fortement typées")

    *Prise en charge d’IntelliSense dans la liaison fortement typées*
10. Appuyez sur **F5** pour exécuter la solution et allez à la page de clients pour s’assurer que les modifications fonctionnent comme prévu.

    ![Affichage des détails sur le client](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "répertoriant les détails du client")

    *Affichage des détails du client*
11. Fermez le navigateur et revenez à Visual Studio.

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a>Tâche 2 - modèle de présentation de liaison dans Web Forms

Dans les versions précédentes d’ASP.NET Web Forms, lorsque vous souhaitez effectuer la liaison de données bidirectionnelle, à la fois la récupération et la mise à jour des données, vous deviez utiliser un objet de Source de données. Cela peut être un objet de Source de données, une Source de données SQL, une Source de données LINQ et ainsi de suite. Toutefois si votre scénario nécessitait un code personnalisé pour gérer les données, vous deviez utiliser l’objet de Source de données, et cela mis certains inconvénients. Par exemple, vous deviez éviter les types complexes et vous avez besoin de gérer les exceptions lors de l’exécution de la logique de validation.

Dans la nouvelle version de Web Forms ASP.NET les contrôles liés aux données prennent en charge la liaison de modèle. Cela signifie que vous pouvez spécifier le sélectionnez, mettre à jour, insérer et supprimer les méthodes directement dans le contrôle lié aux données pour appeler la logique à partir de votre fichier code-behind ou d’une autre classe.

Pour plus d’informations, vous allez utiliser un GridView pour répertorier les catégories de produits à l’aide de la nouvelle **SelectMethod** attribut. Cet attribut vous permet de spécifier une méthode de récupération des données de GridView.

1. Ouvrez le **Products.aspx** page et inclure un **GridView**. Configurez le contrôle GridView comme indiqué ci-dessous pour utiliser des liaisons fortement typée et activer le tri et la pagination.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. Utilisez la nouvelle **SelectMethod** attribut pour configurer le contrôle GridView à appeler une **GetCategories** méthode afin de sélectionner les données.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. Ouvrez le **Products.aspx.cs** code-behind du fichier et ajoutez le code suivant à l’aide d’instructions.

    (Code Snippet - *Web Forms Lab - Ex01 - espaces de noms*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. Ajouter un membre privé dans le **produits** classe et affecter une nouvelle instance de **ProductsContext**. Cette propriété stocke le contexte de données Entity Framework qui vous permet de se connecter à la base de données.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. Créer un **GetCategories** méthode pour récupérer la liste des catégories à l’aide de LINQ. La requête inclut le **produits** propriété donc le contrôle GridView peut afficher la quantité de produits pour chaque catégorie. Notez que la méthode retourne un objet IQueryable brutes qui représentent la requête soit exécutée par la suite le cycle de vie de page.

    (Code Snippet - *Web Forms Lab - Ex01 - GetCategories*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > Dans les versions précédentes d’ASP.NET Web Forms, l’activation de tri et pagination à l’aide de votre propre logique de référentiel dans un contexte d’objet Source de données, requis pour écrire votre propre code personnalisé et recevoir tous les paramètres nécessaires. À présent, comme les méthodes de liaison de données peuvent retourner IQueryable et cela représente une requête toujours pour être exécutée, ASP.NET peut prendre en charge de la modification de la requête pour ajouter un tri correct et les paramètres de pagination.
6. Appuyez sur **F5** pour démarrer le débogage du site et accédez à la page de produits. Vous devez voir que le contrôle GridView est rempli avec les catégories retournés par la méthode GetCategories.

    ![Remplissage d’un GridView à l’aide de la liaison de modèle](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "alimenter un objet GridView à l’aide de la liaison de modèle")

    *Remplissage d’un GridView à l’aide de la liaison de modèle*
7. Appuyez sur **MAJ**+**F5** arrêter le débogage.

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a>Tâche 3 : fournisseurs de valeurs dans la liaison de modèle

Liaison de modèle permet non seulement vous permettent de spécifier des méthodes personnalisées pour travailler avec vos données directement dans le contrôle lié aux données, mais permet également de mapper les données à partir de la page dans les paramètres à partir de ces méthodes. Sur le paramètre de méthode, vous pouvez utiliser des attributs de fournisseur de valeur pour spécifier la source de données de la valeur. Exemple :

- Contrôles sur la page
- Valeurs de chaîne de requête
- Afficher les données
- État de session
- Cookies
- Données de formulaire publiées
- État d’affichage
- Fournisseurs de valeurs personnalisés sont également pris en charge

Si vous avez utilisé ASP.NET MVC 4, vous remarquerez que la prise en charge de liaison de modèle est similaire. En effet, ces fonctionnalités ont été effectuées à partir d’ASP.NET MVC et déplacées dans le **System.Web** assembly pour être en mesure d’utiliser aussi bien sur des Web Forms.

Dans cette tâche, vous serez mise à jour le contrôle GridView pour filtrer les résultats par la quantité de produits pour chaque catégorie, reçoit le paramètre de filtre avec la liaison de modèle.

1. Revenez à la **Products.aspx** page.
2. En haut du contrôle GridView, ajoutez un **étiquette** et un **ComboBox** pour sélectionner le nombre de produits pour chaque catégorie, comme indiqué ci-dessous.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. Ajouter un **EmptyDataTemplate** pour le contrôle GridView pour afficher un message lorsqu’il n’y a aucune catégorie avec le nombre de produits sélectionné.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. Ouvrez le **Products.aspx.cs** code-behind et ajoutez le code suivant à l’aide d’instruction.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. Modifier le **GetCategories** méthode pour recevoir un entier **minProductsCount** argument et filtrer les résultats retournés. Pour ce faire, remplacez la méthode par le code suivant.

    (Code Snippet - *Web Forms Lab - Ex01 - GetCategories 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    La nouvelle **[contrôle]** d’attribut sur le **minProductsCount** argument informe ASP.NET sa valeur doit être remplie à l’aide d’un contrôle dans la page. ASP.NET recherchera à n’importe quel contrôle correspondant au nom de l’argument (minProductsCount) et effectuer le mappage nécessaire et la conversion pour remplir le paramètre avec la valeur du contrôle.

    Vous pouvez également l’attribut fournit un constructeur surchargé qui vous permet de spécifier le contrôle à partir de laquelle obtenir la valeur.

    > [!NOTE]
    > Une des fonctionnalités de liaison de données vise à réduire la quantité de code qui doit être écrit pour l’interaction de la page. Outre le fournisseur de valeurs [contrôle], vous pouvez utiliser les autres fournisseurs de liaison de modèle dans vos paramètres de méthode. Certains d'entre eux sont répertoriés dans l’introduction de la tâche.
6. Appuyez sur **F5** pour démarrer le débogage du site et accédez à la page de produits. Sélectionnez un nombre de produits dans la liste déroulante et notez la façon dont le contrôle GridView est désormais mis à jour.

    ![Le contrôle GridView avec une valeur de liste déroulante de filtrage](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "filtrage GridView avec une valeur de liste déroulante")

    *Le contrôle GridView avec une valeur de liste déroulante de filtrage*
7. Arrêter le débogage.

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a>Tâche 4 : modèle à l’aide de la liaison pour le filtrage

Dans cette tâche, vous allez ajouter une deuxième enfant GridView pour afficher les produits de la catégorie sélectionnée.

1. Ouvrez le **Products.aspx** page et mettre à jour les catégories GridView pour générer automatiquement le bouton de sélection.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. Ajoutez une deuxième **GridView** nommé **productsGrid** en bas. Définir le **ItemType** à **WebFormsLab.Model.Product**, le **DataKeyNames** à **ProductId** et **SelectMethod**  à **GetProducts**. Définissez **AutoGenerateColumns** à **false** et ajoutez les colonnes pour ProductId, ProductName, Description et prix unitaire.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. Ouvrez le **Products.aspx.cs** fichier code-behind. Implémentez le **GetProducts** méthode pour recevoir l’ID de catégorie à partir de la catégorie GridView et de filtrer les produits. Liaison de modèle définit la valeur du paramètre à l’aide de la ligne sélectionnée dans le **categoriesGrid**. Étant donné que le nom d’argument et le nom de contrôle ne correspondent pas, vous devez spécifier le nom du contrôle dans le fournisseur de valeurs de contrôle.

    (Code Snippet - *Web Forms Lab - Ex01 - GetProducts*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > Cette approche facilite unité ces méthodes de test. Dans un contexte de test unitaire, où Web Forms n’est pas exécuté, l’attribut [contrôle] n’effectue aucune action spécifique.
4. Ouvrez le **Products.aspx** page et recherchez les produits GridView. Mettre à jour les produits GridView pour afficher un lien pour l’édition du produit sélectionné.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. Ouvrez le **ProductDetails.aspx** page code-behind et remplacez le **SelectProduct** méthode avec le code suivant.

    (Code Snippet - *Web Forms Lab - Ex01 - SelectProduct méthode*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > Notez que le **[QueryString]** attribut est utilisé pour remplir le paramètre de méthode à partir d’un paramètre de productId dans la chaîne de requête.
6. Appuyez sur **F5** pour démarrer le débogage du site et accédez à la page de produits. Sélectionnez n’importe quelle catégorie dans les catégories GridView et notez que les produits GridView est mis à jour.

    ![Montrant les produits à partir de la catégorie sélectionnée](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "montrant les produits à partir de la catégorie sélectionnée")

    *Montrant les produits à partir de la catégorie sélectionnée*
7. Cliquez sur le **vue** lien sur un produit pour ouvrir la page ProductDetails.aspx.

    Notez que la page récupère le produit avec la méthode SelectMethod en utilisant le paramètre de productId à partir de la chaîne de requête.

    ![Affichage des détails du produit](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "affichage des détails du produit")

    *Affichage des détails du produit*

    > [!NOTE]
    > La possibilité de taper une description HTML sera implémentée dans l’exercice suivant.

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a>Tâche 5 : modèle à l’aide de la liaison pour les opérations de mise à jour

Dans la tâche précédente, vous avez utilisé la liaison de modèle principalement de la sélection des données, dans cette tâche, vous allez apprendre à utiliser la liaison de modèle dans les opérations de mise à jour.

Vous mettrez à jour les catégories GridView pour permettre à l’utilisateur de mettre à jour des catégories.

1. Ouvrez le **Products.aspx** page et mettre à jour les catégories GridView pour générer automatiquement le bouton Modifier et utiliser le nouveau **UpdateMethod** attribut pour spécifier un **UpdateCategory ne**méthode pour mettre à jour de l’élément sélectionné.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    L’attribut DataKeyNames dans le contrôle GridView définir qui sont les membres qui identifient de façon unique l’objet lié au modèle et par conséquent, qui sont les paramètres de que la méthode de mise à jour doit s’afficher au moins.
2. Ouvrez le **Products.aspx.cs** fichier code-behind et mettre en œuvre la **UpdateCategory ne** (méthode). La méthode doit recevoir l’ID de catégorie pour charger la catégorie actuelle, remplir les valeurs à partir de la GridView et puis mettez à jour de la catégorie.

    (Code Snippet - *Web Forms, Lab - Ex01 - UpdateCategory ne*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    La nouvelle **TryUpdateModel** méthode dans la classe de Page est chargée de remplissage de l’objet de modèle en utilisant les valeurs des contrôles dans la page. Dans ce cas, il remplacera les valeurs mises à jour à partir de la ligne GridView actuelle en cours de modification dans le **catégorie** objet.

    > [!NOTE]
    > L’exercice suivant explique l’utilisation de la ModelState.IsValid pour valider les données entrées par l’utilisateur lors de la modification de l’objet.
3. Exécutez le site et accédez à la page de produits. Modifier une catégorie. Tapez un nouveau nom, puis **mise à jour** pour conserver les modifications.

    ![Modification des catégories](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "modification des catégories")

    *Modification des catégories*

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a>Exercice 2 : Validation des données

Dans cet exercice, vous allez découvrir les nouvelles fonctionnalités de validation de données dans ASP.NET 4.5. Vous vérifierez les nouvelles fonctions de validation non obstrusive dans Web Forms. Vous allez utiliser des annotations de données dans les classes de modèle d’application pour la validation des entrées utilisateur, et enfin, vous allez apprendre à activer ou désactiver la validation de la demande par des contrôles individuels dans une page.

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a>Tâche 1 : la Validation non Obstrusive

Formulaires avec des données complexes, y compris les validateurs ont tendance à générer trop de code JavaScript dans la page, ce qui peut représenter environ 60 % du code. Avec la validation non obstrusive est activée, votre code HTML ressemblera plus claire et plus propre.

Dans cette section, vous allez activer la validation non obstrusive dans ASP.NET pour comparer le code HTML généré par les deux configurations.

1. Ouvrez **Visual Studio 2012** et ouvrez le **commencer** solution situé dans le **Source\Ex2-Validation\Begin** dossier de ce laboratoire. Ou bien, vous pouvez continuer à travailler sur votre solution existante à partir de l’exercice précédent.

   1. Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer. Pour ce faire, dans l’Explorateur de solutions, cliquez sur le **WebFormsLab** projet **gérer les Packages NuGet**.
   2. Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.

      > [!NOTE]
      > Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Appuyez sur **F5** pour démarrer l’application web. Accédez aux clients de page et cliquez sur le **ajouter un nouveau client** lien.
3. Avec le bouton droit sur la page du navigateur, puis sélectionnez **afficher la Source** option pour ouvrir le code HTML généré par l’application.

    ![Affichage de la page de code HTML](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "affichant la page de code HTML")

    *Affichage de la page de code HTML*
4. Faites défiler le code source de la page et notez que ASP.NET a injecté JavaScript code et les données validateurs dans la page pour effectuer les validations et afficher la liste d’erreurs.

    ![Code JavaScript de validation dans la page de CustomerDetails](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "code de Validation JavaScript dans la page de CustomerDetails")

    *Code JavaScript de validation dans la page de CustomerDetails*
5. Fermez le navigateur et revenez à Visual Studio.
6. Maintenant, vous allez activer la validation non obstrusive. Ouvrez **Web.Config** et recherchez **ValidationSettings:UnobtrusiveValidationMode** clé dans le **AppSettings** section **.** La valeur est la valeur de clé **WebForms**.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > Vous pouvez également définir cette propriété dans le &quot; **Page\_charge** &quot; événement dans le cas, vous souhaitez activer la Validation non Obstrusive uniquement pour certaines pages.
7. Ouvrez **CustomerDetails.aspx** et appuyez sur **F5** pour démarrer l’application Web.
8. Appuyez sur la touche F12 pour ouvrir les outils de développement Internet Explorer. Une fois que les outils de développement est ouvert, sélectionnez l’onglet de script. Sélectionnez **CustomerDetails.aspx** dans le menu prendre note que les scripts requis pour exécuter jQuery sur la page ont été chargés dans le navigateur à partir du site local.

    ![Le chargement du JavaScript jQuery fichiers directement depuis le serveur IIS local](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "chargement le JavaScript jQuery fichiers directement depuis le serveur IIS local")

    *Le chargement des fichiers JavaScript jQuery directement depuis le serveur IIS local*
9. Fermez le navigateur pour revenir à Visual Studio. Ouvrez le **Site.Master** de fichiers à nouveau et recherchez le **ScriptManager**. Ajoutez l’attribut **EnableCdn** propriété avec la valeur **True**. Cela forcera jQuery à charger à partir de l’URL en ligne et non à partir de l’URL de site local.
10. Ouvrez **CustomerDetails.aspx** dans Visual Studio. Appuyez sur la touche F5 pour exécuter le site. Une fois Internet Explorer s’ouvre, appuyez sur la touche F12 pour ouvrir les outils de développement. Sélectionnez le **Script** onglet, puis examinez la liste déroulante. Notez que les fichiers JavaScript jQuery ne sont plus en cours chargés à partir du site local, mais plutôt qu’à partir de la ligne jQuery CDN.

    ![Le chargement du JavaScript jQuery des fichiers à partir du CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "chargement le JavaScript jQuery des fichiers à partir du CDN")

    *Le chargement des fichiers JavaScript jQuery du CDN*
11. Ouvrez le code source de page HTML à nouveau à l’aide de l’option de source d’affichage dans le navigateur. Notez qu’en activant la validation non obstrusive ASP.NET a remplacé le code JavaScript injecté par data - \*attributs.

    ![Code de validation non obstrusive](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "code de validation non Obstrusive")

    *Code de validation non obstrusive*

    > [!NOTE]
    > Dans cet exemple, vous avez vu comment un résumé et annotations de données de validation a été simplifiée pour seulement quelques HTML et JavaScript lignes. Auparavant, sans validation discrète, les contrôles de validation plus vous ajoutez, plus votre code de validation JavaScript augmentera.

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a>Tâche 2 : validation du modèle avec des Annotations de données

ASP.NET 4.5 introduit la validation des Web Forms annotations de données. Au lieu d’avoir un contrôle de validation sur chaque entrée, vous pouvez maintenant définir des contraintes dans vos classes de modèle et les utiliser sur tous les de votre application web. Dans cette section, vous allez apprendre à utiliser des annotations de données pour valider un formulaire nouveau/modifier un client.

1. Ouvrez **CustomerDetail.aspx** page. Notez que le client le prénom et nom de la seconde dans le **EditItemTemplate** et **InsertItemTemplate** sections sont validées à l’aide d’un contrôle RequiredFieldValidator. Chaque programme de validation est associé à une condition particulière, vous devez inclure des validateurs autant en tant que conditions à vérifier.
2. Ajouter des annotations de données pour valider la classe de modèle Customer. Ouvrez **Customer.cs** classe dans le **modèle** dossier et *décorer* chaque propriété à l’aide des attributs d’annotation de données.

    (Code Snippet - *Web Forms des Annotations de données Lab - Ex02 -*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > .NET framework 4.5 a étendu la collection d’annotations de données existant. Voici quelques-unes des annotations de données que vous pouvez utiliser : [CreditCard], [Phone], [EmailAddress], [plage], [comparer], [Url], [FileExtensions], [Required], [Key], [RegularExpression].
    > 
    > Quelques exemples d’utilisation :
    > 
    > [Key]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > Vous pouvez également définir vos propres messages d’erreur au sein de chaque attribut.
3. Ouvrez **CustomerDetails.aspx** et supprimer tous les RequiredFieldvalidators pour les champs nom et prénom dans les sections EditItemTemplate et InsertItemTemplate du contrôle FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > L’un des avantages de l’utilisation d’annotations de données sont que la logique de validation n’est pas dupliquée dans vos pages d’application. Vous définissez une seule fois dans le modèle et l’utiliser sur toutes les pages d’application qui manipulent des données.
4. Ouvrez **CustomerDetails.aspx** code-behind et recherchez la méthode SaveCustomer. Cette méthode est appelée lors de l’insertion d’un nouveau client et reçoit le paramètre de client à partir des valeurs de contrôle FormView. Lorsque le mappage entre les contrôles de page et le se produit lorsque des objets de paramètre, ASP.NET exécute la validation du modèle par rapport à toutes les annotations de données des attributs et remplir le dictionnaire ModelState avec les erreurs rencontrées, le cas échéant.

    Le ModelState.IsValid retournera uniquement la valeur true si tous les champs de votre modèle sont valides après avoir effectué la validation.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. Ajouter un **ValidationSummary** contrôle à la fin de la page CustomerDetails pour afficher la liste des erreurs de modèle.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    Le **ShowModelStateErrors** est une nouvelle propriété sur le contrôle ValidationSummary contrôler que lorsque la valeur **true**, le contrôle affichera les erreurs à partir du dictionnaire ModelState. Ces erreurs proviennent de la validation d’annotations de données.
6. Appuyez sur **F5** pour exécuter l’application Web. Remplissez le formulaire avec des valeurs erronées et cliquez sur **enregistrer** pour exécuter la validation. Notez que le résumé en bas de l’erreur.

    ![Validation avec des Annotations de données](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "Validation avec des Annotations de données")

    *Validation avec des Annotations de données*

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a>Tâche 3 : gestion des erreurs de base de données personnalisées avec ModelState

Dans la version précédente de Web Forms, gestion des erreurs de base de données comme une chaîne trop longue ou une violation de clé unique peut impliquer la levée d’exceptions dans votre code de référentiel et ensuite gérer des exceptions dans votre code-behind pour afficher une erreur. Une grande quantité de code est nécessaire pour faire quelque chose de relativement simple.

Dans Web Forms 4.5, l’objet de ModelState peut être utilisé pour afficher les erreurs dans la page, à partir de votre modèle ou à partir de la base de données, de manière cohérente.

Dans cette tâche, vous ajouterez du code pour gérer les exceptions de base de données et afficher le message approprié à l’utilisateur à l’aide de l’objet ModelState correctement.

1. Tandis que l’application est en cours d’exécution, essayez de mettre à jour le nom d’une catégorie à l’aide d’une valeur en double.

    ![Mise à jour d’une catégorie avec un nom dupliqué](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "mise à jour d’une catégorie avec un nom dupliqué")

    *Mise à jour d’une catégorie avec un nom dupliqué*

    Notez qu’une exception est levée en raison du &quot;unique&quot; contrainte de la **CategoryName** colonne.

    ![Exception pour les noms de catégorie dupliqué](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "Exception pour les noms de catégorie dupliqué")

    *Exception pour les noms de catégorie dupliqué*
2. Arrêter le débogage. Dans le **Products.aspx.cs** fichier code-behind, mettez à jour le **UpdateCategory ne** méthode pour gérer les exceptions levées par la base de données. Méthode de SaveChanges() appeler et ajoute une erreur à la **ModelState** objet.

    La nouvelle **TryUpdateModel** méthode met à jour l’objet category récupérée à partir de la base de données en utilisant les données de formulaire fournies par l’utilisateur.

    (Code Snippet - *Web Forms Lab - Ex02 - UpdateCategory n’Handle erreurs*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > Dans l’idéal, vous seriez obligé d’identifier la cause de l’exception DbUpdateException et vérifiez si la cause racine est la violation d’une contrainte de clé unique.
3. Ouvrez **Products.aspx** et ajoutez un **ValidationSummary** contrôle sous les catégories GridView pour afficher la liste des erreurs de modèle.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. Exécutez le site et accédez à la page de produits. Essayez de mettre à jour le nom d’une catégorie à l’aide d’une valeur en double.

    Notez que l’exception a été gérée et le message d’erreur s’affiche dans le **ValidationSummary** contrôle.

    ![Dupliqué erreur catégorie](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "dupliqué d’erreur de catégorie")

    *Erreur de catégorie dupliqué*

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a>Tâche 4 : demander la Validation dans Web Forms ASP.NET 4.5

La fonctionnalité de validation de demande dans ASP.NET fournit un certain niveau de protection par défaut contre les attaques de cross-site scripting (XSS). Dans les versions précédentes d’ASP.NET, validation de la demande a été activée par défaut et peut uniquement être désactivée pour une page entière. Avec la nouvelle version de Web Forms ASP.NET vous pouvez maintenant désactiver la validation de demande pour un seul contrôle, effectuer la validation de requête différée ou accéder aux données de demande non validée (soyez prudent si vous le faire !).

1. Appuyez sur **Ctrl + F5** pour démarrer le site sans débogage, passez à la page de produits. Sélectionnez une catégorie, puis cliquez sur le **modifier** lien sur tous les produits.
2. Tapez une description contenant le contenu potentiellement dangereux, par exemple, y compris les balises HTML. Prenez en compte de l’exception levée en raison de la validation de la demande.

    ![Modification d’un produit avec le contenu potentiellement dangereux](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "modification d’un produit avec le contenu potentiellement dangereux")

    *Modification d’un produit avec le contenu potentiellement dangereux*

    ![Exception levée en raison de la validation de la demande](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "Exception levée en raison de la validation de la demande")

    *Exception levée en raison de la validation de la demande*
3. Fermez la page et, dans Visual Studio, appuyez sur **MAJ + F5** pour arrêter le débogage.
4. Ouvrez le **ProductDetails.aspx** page et recherchez le **Description** zone de texte.
5. Ajoutez la nouvelle **ValidateRequestMode** propriété à la zone de texte et définissez sa valeur sur **désactivé**.

    La nouvelle **ValidateRequestMode** attribut vous permet de désactiver la validation de demande granulaire sur chaque contrôle. Cela est utile lorsque vous souhaitez utiliser une entrée qui peut-être recevoir le code HTML, mais souhaitez conserver la validation du travail pour le reste de la page.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. Appuyez sur **F5** pour exécuter l’application web. Rouvrez la page de produit de modification et une description de produit, y compris les balises HTML complète. Notez que vous pouvez maintenant ajouter du contenu HTML à la description.

    ![Validation désactivée pour la description du produit de la demande](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "validation désactivée pour la description du produit de la demande")

    *Validation de la demande désactivée pour la description du produit*

    > [!NOTE]
    > Dans une application de production, vous devez nettoyer le code HTML entré par l’utilisateur de s’assurer que les balises HTML uniquement sécurisés sont entrés (par exemple, il y a aucune &lt;script&gt; balises). Pour ce faire, vous pouvez utiliser [Microsoft Web Protection Library](https://www.nuget.org/packages/AntiXSS).
7. Modifier le produit à nouveau. Tapez le code HTML dans le champ nom, cliquez sur **enregistrer**. Notez que la Validation des demandes est désactivée uniquement pour le champ de Description et le reste des champs re est toujours validé par rapport au contenu potentiellement dangereux.

    ![La validation est activée dans le reste des champs de requête](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "la validation est activée dans le reste des champs de requête")

    *Validation de la demande est activée dans le reste des champs*

    Web Forms ASP.NET 4.5 inclut un nouveau mode de validation de demande pour effectuer la validation de demande tardivement. Avec le mode de validation de demande défini sur **4.5**, si un morceau de code accède *Request.Form [&quot;clé&quot;]*, demande validation ne concernent que de ASP.NET 4.5 la validation de requête pour cet élément spécifique dans la collection de formulaires.

    En outre, ASP.NET 4.5 inclut désormais les routines d’encodage principales à partir de la version 4.0 de Microsoft Anti-XSS Library. L’Anti-XSS routines d’encodage sont implémentées par le nouveau *AntiXssEncoder* type figurant dans le nouveau **System.Web.Security.AntiXss** espace de noms. Avec le **encoderType** paramètre configuré pour utiliser *AntiXssEncoder*, toutes les sorties d’encodage automatiquement au sein d’ASP.NET utilise les nouvelles routines d’encodage.
8. Validation de demande 4.5 ASP.NET prend également en charge les accès non validé pour demander des données. ASP.NET 4.5 ajoute une nouvelle propriété de collection pour le **HttpRequest** objet appelé **Unvalidated**. Lorsque vous accédez dans **HttpRequest.Unvalidated** vous avez accès à toutes les parties communes des données de requête, y compris les formulaires, les chaînes de requête, les Cookies, les URL et ainsi de suite.

    ![Objet de Request.Unvalidated](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request.Unvalidated objet")

    *Objet de Request.Unvalidated*

    > [!NOTE]
    > **Utilisez la propriété HttpRequest.Unvalidated avec précaution !** Assurez-vous que vous effectuez soigneusement une validation personnalisée sur les données de demande brute pour garantir que le texte dangereux n'est pas un aller-retour et restitué en clients peu méfiants !

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a>Exercice 3 : Page asynchrone dans ASP.NET Web Forms

Dans cet exercice, il vous serez présenté à la page asynchrone nouvelles fonctionnalités dans ASP.NET Web Forms de traitement.

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a>Tâche 1 : la mise à jour du produit décrit en détail la Page pour télécharger et afficher les Images

Dans cette tâche, vous mettrez à jour la page Détails du produit pour autoriser l’utilisateur à spécifier une URL d’image pour le produit et les afficher dans la vue en lecture seule. Vous allez créer une copie locale de l’image spécifiée en téléchargeant de façon synchrone. Dans la tâche suivante, vous mettrez à jour cette implémentation pour qu’il fonctionne de façon asynchrone.

1. Open **Visual Studio 2012** et charger le **commencer** solution situé dans **Source\Ex3-Async\Begin** à partir du dossier de ce laboratoire. Ou bien, vous pouvez continuer à travailler sur votre solution existante à partir des exercices précédents.

   1. Si vous avez ouvert le fourni **commencer** solution, vous devrez télécharger certains packages NuGet manquants avant de continuer. Pour ce faire, dans l’Explorateur de solutions, cliquez sur le **WebFormsLab** de projet et sélectionnez **gérer les Packages NuGet**.
   2. Dans le **gérer les Packages NuGet** boîte de dialogue, cliquez sur **restaurer** afin de télécharger les packages manquants.
   3. Enfin, générez la solution en cliquant sur **Build** | **générer la Solution**.

      > [!NOTE]
      > Un des avantages de l’utilisation de NuGet est que vous n’êtes pas obligé expédier toutes les bibliothèques dans votre projet, ce qui réduit la taille du projet. Avec NuGet Power Tools, en spécifiant les versions de package dans le fichier Packages.config, vous serez en mesure de télécharger toutes les bibliothèques nécessaires à la première fois que vous exécutez le projet. C’est pourquoi vous devrez exécuter ces étapes après avoir ouvert une solution existante à partir de ce laboratoire.
2. Ouvrez le **ProductDetails.aspx** page source et ajouter un champ dans ItemTemplate de FormView pour afficher l’image de produit.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. Ajoutez un champ pour spécifier l’URL d’image dans un FormView EditTemplate.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. Ouvrez le **ProductDetails.aspx.cs** code-behind du fichier et ajoutez les directives d’espace de noms suivantes.

    (Code Snippet - *Web Forms Lab - Ex03 - espaces de noms*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. Créer un **UpdateProductImage** méthode pour stocker des images à distance local **Images** dossier et mise à jour l’entité product avec la nouvelle valeur d’emplacement image.

    (Code Snippet - *Web Forms Lab - Ex03 - UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. Mise à jour le **UpdateProduct** méthode à appeler le **UpdateProductImage** (méthode).

    (Code Snippet - *Web Forms Lab - Ex03 - UpdateProductImage appel*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. Exécutez l’application et essayez de télécharger une image pour un produit. Par exemple, vous pouvez utiliser l’URL d’image suivante à partir d’Office Clip Arts : [[http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)

    ![Définition d’une image pour un produit](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "définition d’une image pour un produit")

    *Définition d’une image pour un produit*

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a>Tâche 2 - Ajout de traitement à la Page Détails du produit asynchrone

Dans cette tâche, vous mettrez à jour la page Détails du produit pour qu’il fonctionne de façon asynchrone. Vous allez améliorer une tâche longue - le processus de téléchargement d’image - à l’aide du traitement de page asynchrone d’ASP.NET 4.5.

Les méthodes asynchrones dans les applications web peuvent être utilisés pour optimiser la façon dont les pools de threads ASP.NET sont utilisées. Dans ASP.NET il sont un nombre limité de threads du pool de threads pour attending [présent] demande, par conséquent, lorsque tous les threads sont occupés, ASP.NET commence à rejeter les nouvelles demandes, envoie des messages d’erreur application et permet de définir votre site indisponible.

Opérations de longue durée sur votre site web sont d’excellents candidats pour la programmation asynchrone, car ils occupent le thread assigné pendant une longue période. Cela inclut les demandes en cours d’exécution longue, pages avec un grand nombre des différents éléments et les pages qui nécessitent des opérations hors connexion, ces interrogation d’une base de données ou l’accès à un serveur web externe. L’avantage est que si vous utilisez des méthodes asynchrones pour ces opérations, lors du traitement de la page, le thread est libéré et retourné au thread du pool et peut être utilisé pour assister à une nouvelle demande de page. Cela signifie que, la page commence à traiter dans un thread du pool de threads et le traitement dans un autre, peut se terminer après que le traitement asynchrone est terminée.

1. Ouvrez le **ProductDetails.aspx** page. Ajouter le **Async** d’attribut dans le **Page** élément et affectez-lui la valeur **true**. Cet attribut indique à ASP.NET pour implémenter l’interface IHttpAsyncHandler.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. Ajoutez une étiquette au bas de la page pour afficher les détails des threads d’exécution de la page.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. Ouvrez **ProductDetails.aspx.cs** et ajoutez les directives d’espace de noms suivantes.

    (Code Snippet - *Web Forms espaces de noms Lab - Ex03 - 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. Modifier le **UpdateProductImage** méthode pour télécharger l’image avec une tâche asynchrone. Vous allez remplacer le **WebClient** **DownloadFile** méthode avec le **DownloadFileTaskAsync** (méthode) et inclure le **await** mot clé.

    (Code Snippet - *Web Forms Lab - Ex03 - UpdateProductImage Async*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    Le RegisterAsyncTask enregistre une nouvelle tâche asynchrone de page doit être exécuté dans un thread différent. Il reçoit une expression lambda avec la tâche (t) doit être exécuté. Le **await** mot clé dans le **DownloadFileTaskAsync** méthode convertit le reste de la méthode en un rappel qui est appelé de façon asynchrone après le **DownloadFileTaskAsync** méthode est terminée. ASP.NET reprend l’exécution de la méthode en gérant automatiquement tous les HTTP demande valeurs d’origine. Le nouveau modèle de programmation asynchrone dans .NET 4.5 permet d’écrire du code asynchrone qui ressemble beaucoup à code synchrone et laisser le compilateur gérer les complications de fonctions de rappel ou code de continuation.
    > [!NOTE]
    > RegisterAsyncTask et PageAsyncTask étaient déjà disponibles depuis le .NET 2.0. Le mot clé await est une nouveauté à partir du modèle de programmation asynchrone .NET 4.5 et peut être utilisé avec les nouvelles méthodes TaskAsync à partir de l’objet WebClient .NET.
5. Ajoutez du code pour afficher les threads sur lequel le code est démarré et terminé l’exécution. Pour ce faire, mettez à jour le **UpdateProductImage** méthode avec le code suivant.

    (Code Snippet - *threads de Web Forms Lab - Ex03 - Show*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. Ouvrez le site web **Web.config** fichier. Ajoutez la variable appSetting suivante.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. Appuyez sur **F5** pour exécuter l’application et de télécharger une image pour le produit. Notez l’ID de threads où le code de début et de fin peut être différent. Il s’agit, car les tâches asynchrones s’exécutent sur un thread séparé à partir du pool de threads ASP.NET. Lorsque la tâche se termine, ASP.NET place la tâche dans la file d’attente et attribue des threads disponibles.

    ![Télécharger une image de manière asynchrone](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "télécharger une image de manière asynchrone")

    *Télécharger une image de manière asynchrone*

> [!NOTE]
> En outre, vous pouvez déployer cette application à Azure suit [publication annexe b : une Application ASP.NET MVC 4, à l’aide de Web Deploy](#AppendixB).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Récapitulatif

Dans ce laboratoire pratique, les concepts suivants ont été traités et présentés :

- Utiliser des expressions de liaison de données fortement typées
- Utiliser les nouvelles fonctionnalités de liaison de modèle dans Web Forms
- Utiliser des fournisseurs de valeurs pour le mappage des données de page aux méthodes de code-behind
- Utiliser des Annotations de données pour la validation d’entrée utilisateur
- Prendre advange d’unobstrusive la validation côté client avec jQuery dans les Web Forms
- Implémenter la validation de demande granulaire
- Implémenter une page asynchrone de traitement dans Web Forms

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Annexe a : installation de Visual Studio Express 2012 pour le Web

Vous pouvez installer **Microsoft Visual Studio Express 2012 pour Web** ou un autre &quot;Express&quot; à l’aide de la version du **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Les instructions suivantes vous guident dans les étapes requises pour installer *Visual studio Express 2012 pour Web* à l’aide de *Microsoft Web Platform Installer*.

1. Accédez à [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Si vous avez déjà installé Web Platform Installer, vous pouvez également ouvrir il et recherchez le produit &quot; <em>Visual Studio Express 2012 pour le Web avec le Kit de développement</em>&quot;.
2. Cliquez sur **installer maintenant**. Si vous n’avez pas **Web Platform Installer** vous allez être redirigé pour télécharger et installer en premier.
3. Une fois **Web Platform Installer** est ouvert, cliquez sur **installer** pour démarrer le programme d’installation.

    ![Installer Visual Studio Express](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "installer Visual Studio Express")

    *Installer Visual Studio Express*
4. Lisez les licences et les termes du contrat de tous les produits et cliquez sur **J’accepte** pour continuer.

    ![Accepter les termes du contrat de licence](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    *Accepter les termes du contrat de licence*
5. Attendez que le processus de téléchargement et l’installation se termine.

    ![Progression de l'installation](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    *Progression de l’installation*
6. Une fois l’installation terminée, cliquez sur **Terminer**.

    ![Installation est terminée](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    *Installation est terminée*
7. Cliquez sur **Exit** pour fermer Web Platform Installer.
8. Pour ouvrir Visual Studio Express pour le Web, accédez à la **Démarrer** écran et démarrer l’écriture &quot; **VS Express**&quot;, puis cliquez sur le **Visual Studio Express pour le Web** vignette.

    ![VS Express pour une vignette de Web](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    *VS Express pour une vignette de Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Annexe b : publication d’une Application ASP.NET MVC 4, à l’aide de Web Deploy

Cette annexe sera vous montrent comment créer un nouveau site web à partir du portail Azure et publiez l’application que vous avez obtenu en suivant le laboratoire, en tirant parti de la fonctionnalité de publication Web Deploy fournie par Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Tâche 1 : création d’un nouveau Site Web à partir du portail Azure

1. Accédez à la [portail de gestion](https://manage.windowsazure.com/) et connectez-vous en utilisant les informations d’identification Microsoft associées à votre abonnement.

    > [!NOTE]
    > Avec Azure, vous pouvez héberger 10 Sites Web ASP.NET gratuitement et faites ensuite évoluer que votre trafic augmente. Vous pouvez vous inscrire [ici](http://aka.ms/aspnet-hol-azure).

    ![Ouvrez une session sur le portail Windows Azure](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "ouvrez une session sur le portail Windows Azure")

    *Connectez-vous au portail*
2. Cliquez sur **New** sur la barre de commandes.

    ![Création d’un Site Web](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "création d’un Site Web")

    *Création d’un Site Web*
3. Cliquez sur **calcul** | **Site Web**. Puis sélectionnez **création rapide** option. Fournir une URL disponible pour le nouveau site web et cliquez sur **créer un Site Web**.

    > [!NOTE]
    > Azure est l’hôte pour une application web en cours d’exécution dans le cloud que vous pouvez contrôler et gérer. L’option Création rapide vous permet de déployer une application web terminée sur Azure à partir en dehors du portail. Il n’inclut pas les étapes de configuration d’une base de données.

    ![Création d’un Site Web à l’aide de la création rapide](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "création d’un Site Web à l’aide de la création rapide")

    *Création d’un Site Web à l’aide de la création rapide*
4. Attendez que la nouvelle **Site Web** est créé.
5. Une fois que le Site Web est créé, cliquez sur le lien situé sous le **URL** colonne. Vérifiez que le nouveau Site Web fonctionne.

    ![Navigation vers le nouveau site web](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "exploration vers le nouveau site web")

    *Navigation vers le nouveau site web*

    ![Site Web en cours d’exécution](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "site Web en cours d’exécution")

    *Site Web en cours d’exécution*
6. Revenez au portail et cliquez sur le nom du site web sous le **nom** colonne pour afficher les pages de gestion.

    ![Ouvrir les pages de gestion de site web](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "ouvrir les pages de gestion de site web")

    *Ouvrir les pages de gestion de Site Web*
7. Dans le **tableau de bord** page, sous la **aperçu rapide** , cliquez sur le **télécharger le profil de publication** lien.

    > [!NOTE]
    > Le *le profil de publication* contient toutes les informations requises pour publier une application web vers Azure pour chaque méthode de publication est activée. Le profil de publication contient les URL, les informations d’identification utilisateur et les chaînes de base de données requis pour la connexion et l’authentification par rapport à chacun des points de terminaison pour lequel une méthode de publication est activée. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express pour Web** et **Microsoft Visual Studio 2012** prise en charge de la lecture des profils pour automatiser la configuration de ces programmes pour de publication publication d’applications web sur Azure.

    ![Téléchargement du site web le profil de publication](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "téléchargement du site web le profil de publication")

    *Téléchargement du Site Web le profil de publication*
8. Télécharger le fichier de profil de publication dans un emplacement connu. Davantage dans cet exercice, vous verrez comment utiliser ce fichier pour publier une application web vers Azure à partir de Visual Studio.

    ![L’enregistrement du fichier de profil de publication](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "l’enregistrement du profil de publication")

    *L’enregistrement du fichier de profil de publication*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tâche 2 : configuration du serveur de base de données

Si votre application se sert de SQL Server vous devez créer un serveur de base de données SQL des bases de données. Si vous souhaitez déployer une application simple qui n’utilise pas de SQL Server, vous pouvez ignorer cette tâche.

1. Vous devez un serveur de base de données SQL pour stocker la base de données d’application. Vous pouvez afficher les serveurs de base de données SQL à partir de votre abonnement dans le portail de gestion Azure à **bases de données Sql** | **serveurs** | **tableau de bord du serveur**. Si vous n’avez pas un serveur créé, vous pouvez créer un à l’aide du **ajouter** bouton sur la barre de commandes. Prenez note de la **nom du serveur et les URL, nom de connexion d’administrateur et mot de passe**, car vous les utiliserez dans les tâches suivantes. Ne créez pas encore, la base de données tel qu’il sera créé dans une étape ultérieure.

    ![Tableau de bord de serveur SQL Database](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "tableau de bord serveur de base de données SQL")

    *Tableau de bord serveur de base de données SQL*
2. Dans la tâche suivante, vous allez tester la connexion de base de données à partir de Visual Studio, pour cette raison, vous devez inclure votre adresse IP locale dans la liste du serveur de **adresses IP autorisées**. Pour ce faire, cliquez sur **configurer**, sélectionnez l’adresse IP à partir de **adresse IP cliente actuelle** et collez-le sur le **adresse IP de début** et **adresse IP de fin** zones de texte et cliquez sur le ![add-client-ip-address-ok-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png) bouton.

    ![Ajout d’adresse IP du Client](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    *Ajout d’adresse IP du Client*
3. Une fois le **adresse IP du Client** est ajouté à le des adresses IP autorisées de liste, cliquez sur **enregistrer** pour confirmer les modifications.

    ![Confirmer les modifications](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    *Confirmer les modifications*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tâche 3 : publication d’une Application ASP.NET MVC 4, à l’aide de Web Deploy

1. Revenez à la solution ASP.NET MVC 4. Dans le **l’Explorateur de solutions**, cliquez sur le projet de site web et sélectionnez **publier**.

    ![Publication de l’Application](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "publication de l’Application")

    *Publier le site web*
2. Importer le profil de publication que vous avez enregistré dans la première tâche.

    ![L’importation du profil de publication](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "l’importation du profil de publication")

    *Importation du profil de publication*
3. Cliquez sur **valider la connexion**. Une fois la Validation terminée. Cliquez sur **suivant**.

    > [!NOTE]
    > La validation est terminée une fois que vous voyez une coche verte apparaît en regard du bouton Valider la connexion.

    ![Validation de la connexion](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "validation de la connexion")

    *Validation de la connexion*
4. Dans le **paramètres** page, sous la **bases de données** , cliquez sur le bouton en regard de la zone de texte de la connexion de votre base de données (par exemple, **DefaultConnection**).

    ![Configuration de déploiement Web](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "configuration de déploiement Web")

    *Configuration de déploiement Web*
5. Configurez la connexion de base de données comme suit :

   - Dans le **nom du serveur** tapez votre URL de base de données SQL server à l’aide du *tcp :* préfixe.
   - Dans **nom d’utilisateur** tapez votre nom de connexion d’administrateur serveur.
   - Dans **mot de passe** tapez votre mot de passe du compte de connexion administrateur serveur.
   - Tapez un nouveau nom de base de données.

     ![Configuration de chaîne de connexion de destination](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "configuration de chaîne de connexion de destination")

     *Configuration de chaîne de connexion de destination*
6. Cliquez ensuite sur **OK**. Lorsque vous êtes invité à créer la base de données, cliquez sur **Oui**.

    ![Création de la base de données](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "création de la chaîne de la base de données")

    *Création de la base de données*
7. La chaîne de connexion que vous utiliserez pour vous connecter à la base de données SQL Azure est indiquée dans la zone de texte de la connexion par défaut. Cliquez ensuite sur **Suivant**.

    ![Chaîne de connexion pointant vers la base de données SQL](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "chaîne de connexion pointant vers la base de données SQL")

    *Chaîne de connexion pointant vers la base de données SQL*
8. Dans le **aperçu** , cliquez sur **publier**.

    ![Publication de l’application web](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "publication de l’application web")

    *Publication de l’application web*
9. Une fois le processus de publication terminé, votre navigateur par défaut s’ouvre le site web publié.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Annexe c : l’utilisation des extraits de Code

Avec des extraits de code, vous avez tout le code que vous avez besoin à portée de main. Le document de laboratoire vous indiquera exactement quand vous pouvez les utiliser, comme indiqué dans l’illustration suivante.

![À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "extraits de code à l’aide de Visual Studio pour insérer du code dans votre projet")

*À l’aide d’extraits de code Visual Studio pour insérer du code dans votre projet*

***Pour ajouter un extrait de code à l’aide du clavier (c# uniquement)***

1. Placez le curseur où vous souhaitez insérer le code.
2. Commencez à taper le nom de l’extrait de code (sans espaces ou des traits d’union).
3. Regarder en tant qu’IntelliSense affiche les noms des extraits correspondants.
4. Sélectionnez l’extrait de code correct (ou continuez à taper jusqu'à ce que le nom de l’extrait de code entière est sélectionnée).
5. Appuyez sur la touche Tab à deux reprises pour insérer l’extrait de code à l’emplacement du curseur.

![Commencez à taper le nom de l’extrait de code](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "commencez à taper le nom de l’extrait de code")

*Commencez à taper le nom de l’extrait de code*

![Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "appuyez sur Tab pour sélectionner l’extrait de code en surbrillance")

*Appuyez sur Tab pour sélectionner l’extrait de code en surbrillance*

![Appuyez sur Tab à nouveau et l’extrait de code seront développe](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "appuyez sur Tab à nouveau et l’extrait de code seront développe.")

*Appuyez sur Tab à nouveau et l’extrait de code seront développe.*

***Pour ajouter un extrait de code à l’aide de la souris (c#, Visual Basic et XML)*** 1. Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code.

1. Sélectionnez **insérer un extrait** suivie **mes extraits de Code**.
2. Choisissez l’extrait de code approprié dans la liste, en cliquant dessus.

![Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait")

*Avec le bouton droit dans laquelle vous souhaitez insérer l’extrait de code et sélectionnez Insérer un extrait*

![Choisissez l’extrait de code approprié dans la liste, en cliquant dessus](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "choisir l’extrait de code approprié dans la liste, en cliquant sur celle-ci")

*Choisissez l’extrait de code approprié dans la liste, en cliquant sur celle-ci*
