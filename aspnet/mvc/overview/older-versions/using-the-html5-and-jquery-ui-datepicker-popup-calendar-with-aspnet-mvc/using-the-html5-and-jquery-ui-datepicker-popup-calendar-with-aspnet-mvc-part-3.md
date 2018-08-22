---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Utilisation de HTML5 et Datepicker calendrier contextuel jQuery UI avec ASP.NET MVC - partie 3 | Microsoft Docs
author: Rick-Anderson
description: Ce didacticiel vous apprend les notions de base de l’utilisation des modèles de l’éditeur, les modèles d’affichage et le calendrier contextuel jQuery UI datepicker dans une MV ASP.NET...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: 184d93323ac6f2cb7f14d32b401cf6a400eb79a6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829531"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a><span data-ttu-id="1cad5-103">Utilisation de HTML5 et Datepicker calendrier contextuel jQuery UI avec ASP.NET MVC - partie 3</span><span class="sxs-lookup"><span data-stu-id="1cad5-103">Using the HTML5 and jQuery UI Datepicker Popup Calendar with ASP.NET MVC - Part 3</span></span>
====================
<span data-ttu-id="1cad5-104">par [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="1cad5-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="1cad5-105">Ce didacticiel vous apprend les notions de base de l’utilisation des modèles de l’éditeur, les modèles d’affichage et le calendrier contextuel jQuery UI datepicker dans une application Web ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1cad5-105">This tutorial will teach you the basics of how to work with editor templates, display templates, and the jQuery UI datepicker popup calendar in an ASP.NET MVC Web application.</span></span>


## <a name="working-with-complex-types"></a><span data-ttu-id="1cad5-106">Utilisation des Types complexes</span><span class="sxs-lookup"><span data-stu-id="1cad5-106">Working with Complex Types</span></span>

<span data-ttu-id="1cad5-107">Dans cette section, vous allez créer une classe d’adresses et découvrez comment créer un modèle pour l’afficher.</span><span class="sxs-lookup"><span data-stu-id="1cad5-107">In this section you'll create an address class and learn how to create a template to display it.</span></span>

<span data-ttu-id="1cad5-108">Dans le *modèles* dossier, créez un fichier de classe nommé *Person.cs* où vous allez placer les deux types : un `Person` classe et un `Address` classe.</span><span class="sxs-lookup"><span data-stu-id="1cad5-108">In the *Models* folder, create a new class file named *Person.cs* where you'll put two types: a `Person` class and an `Address` class.</span></span> <span data-ttu-id="1cad5-109">Le `Person` classe contiendra une propriété qui est de type `Address`.</span><span class="sxs-lookup"><span data-stu-id="1cad5-109">The `Person` class will contain a property that's typed as `Address`.</span></span> <span data-ttu-id="1cad5-110">Le `Address` type est un type complexe, ce qui signifie qu’il n’est pas un des types intégrés comme `int`, `string`, ou `double`.</span><span class="sxs-lookup"><span data-stu-id="1cad5-110">The `Address` type is a complex type, meaning it's not one of the built-in types like `int`, `string`, or `double`.</span></span> <span data-ttu-id="1cad5-111">Au lieu de cela, il a plusieurs propriétés.</span><span class="sxs-lookup"><span data-stu-id="1cad5-111">Instead, it has several properties.</span></span> <span data-ttu-id="1cad5-112">Le code pour les nouvelles classes ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="1cad5-112">The code for the new classes looks like this:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

<span data-ttu-id="1cad5-113">Dans le `Movie` contrôleur, ajoutez le code suivant `PersonDetail` action pour afficher une instance de personne :</span><span class="sxs-lookup"><span data-stu-id="1cad5-113">In the `Movie` controller, add the following `PersonDetail` action to display a person instance:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

<span data-ttu-id="1cad5-114">Puis ajoutez le code suivant à la `Movie` contrôleur pour remplir la `Person` modèle avec des exemples de données :</span><span class="sxs-lookup"><span data-stu-id="1cad5-114">Then add the following code to the `Movie` controller to populate the `Person` model with some sample data:</span></span>

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

<span data-ttu-id="1cad5-115">Ouvrez le *Views\Movies\PersonDetail.cshtml* fichier, puis ajoutez le balisage suivant pour le `PersonDetail` vue.</span><span class="sxs-lookup"><span data-stu-id="1cad5-115">Open the *Views\Movies\PersonDetail.cshtml* file and add the following markup for the `PersonDetail` view.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

<span data-ttu-id="1cad5-116">Appuyez sur Ctrl + F5 pour exécuter l’application et accédez à *films/PersonDetail*.</span><span class="sxs-lookup"><span data-stu-id="1cad5-116">Press Ctrl+F5 to run the application and navigate to *Movies/PersonDetail*.</span></span>

<span data-ttu-id="1cad5-117">Le `PersonDetail` vue ne contient pas le `Address` type complexe, comme vous pouvez le voir dans cette capture d’écran.</span><span class="sxs-lookup"><span data-stu-id="1cad5-117">The `PersonDetail` view doesn't contain the `Address` complex type, as you can see in this screenshot.</span></span> <span data-ttu-id="1cad5-118">(Aucune adresse n’est indiqué).</span><span class="sxs-lookup"><span data-stu-id="1cad5-118">(No address is shown.)</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

<span data-ttu-id="1cad5-119">Le `Address` des données de modèle ne s’affiche pas, car c’est un type complex.</span><span class="sxs-lookup"><span data-stu-id="1cad5-119">The `Address` model data is not displayed because it's a complex type.</span></span> <span data-ttu-id="1cad5-120">Pour afficher les informations d’adresse, ouvrez le *Views\Movies\PersonDetail.cshtml* fichier à nouveau, puis ajoutez le balisage suivant.</span><span class="sxs-lookup"><span data-stu-id="1cad5-120">To display the address information, open the *Views\Movies\PersonDetail.cshtml* file again and add the following markup.</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

<span data-ttu-id="1cad5-121">Le balisage complet pour le `PersonDetail` maintenant affichage ressemble à ceci :</span><span class="sxs-lookup"><span data-stu-id="1cad5-121">The complete markup for the `PersonDetail` now view looks like this:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

<span data-ttu-id="1cad5-122">Exécuter à nouveau l’application et afficher le `PersonDetail` vue.</span><span class="sxs-lookup"><span data-stu-id="1cad5-122">Run the application again and display the `PersonDetail` view.</span></span> <span data-ttu-id="1cad5-123">Les informations d’adresse s’affiche désormais :</span><span class="sxs-lookup"><span data-stu-id="1cad5-123">The address information is now displayed:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a><span data-ttu-id="1cad5-124">Création d’un modèle pour un Type complexe</span><span class="sxs-lookup"><span data-stu-id="1cad5-124">Creating a Template for a Complex Type</span></span>

<span data-ttu-id="1cad5-125">Dans cette section, vous allez créer un modèle qui sera utilisé pour restituer le `Address` type complexe.</span><span class="sxs-lookup"><span data-stu-id="1cad5-125">In this section you'll create a template that will be used to render the `Address` complex type.</span></span> <span data-ttu-id="1cad5-126">Lorsque vous créez un modèle pour le `Address` type, ASP.NET MVC peut automatiquement utilisez-le pour mettre en forme un modèle d’adresse n’importe où dans l’application.</span><span class="sxs-lookup"><span data-stu-id="1cad5-126">When you create a template for the `Address` type, ASP.NET MVC can automatically use it to format an address model anywhere in the application.</span></span> <span data-ttu-id="1cad5-127">Cela vous donne un moyen de contrôler le rendu de la `Address` type dans un seul endroit dans l’application.</span><span class="sxs-lookup"><span data-stu-id="1cad5-127">This gives you a way to control the rendering of the `Address` type from just one place in the application.</span></span>

<span data-ttu-id="1cad5-128">Dans le *Views\Shared\DisplayTemplates* dossier, créez une vue partielle fortement typée nommée **adresse**:</span><span class="sxs-lookup"><span data-stu-id="1cad5-128">In the *Views\Shared\DisplayTemplates* folder, create a strongly typed partial view named **Address**:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

<span data-ttu-id="1cad5-129">Cliquez sur **ajouter**, puis ouvrez le nouveau *Views\Shared\DisplayTemplates\Address.cshtml* fichier.</span><span class="sxs-lookup"><span data-stu-id="1cad5-129">Click **Add**, and then open the new *Views\Shared\DisplayTemplates\Address.cshtml* file.</span></span> <span data-ttu-id="1cad5-130">La nouvelle vue contient le balisage généré suivant :</span><span class="sxs-lookup"><span data-stu-id="1cad5-130">The new view contains the following generated markup:</span></span>

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

<span data-ttu-id="1cad5-131">Exécuter l’application et afficher le `PersonDetail` vue.</span><span class="sxs-lookup"><span data-stu-id="1cad5-131">Run the application and display the `PersonDetail` view.</span></span> <span data-ttu-id="1cad5-132">Cette fois-ci, le `Address` modèle que vous venez de créer est utilisé pour afficher le `Address` type complexe, afin que l’affichage ressemble à ce qui suit :</span><span class="sxs-lookup"><span data-stu-id="1cad5-132">This time, the `Address` template that you just created is used to display the `Address` complex type, so the display looks like the following:</span></span>

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a><span data-ttu-id="1cad5-133">Résumé : Comment spécifier le Format d’affichage de modèle et le modèle</span><span class="sxs-lookup"><span data-stu-id="1cad5-133">Summary: Ways to Specify the Model Display Format and Template</span></span>

<span data-ttu-id="1cad5-134">Vous avez vu que vous pouvez spécifier le format ou le modèle pour une propriété de modèle à l’aide des approches suivantes :</span><span class="sxs-lookup"><span data-stu-id="1cad5-134">You've seen that you can specify the format or template for a model property by using the following approaches:</span></span>

- <span data-ttu-id="1cad5-135">Appliquer le `DisplayFormat` attribut à une propriété dans le modèle.</span><span class="sxs-lookup"><span data-stu-id="1cad5-135">Applying the `DisplayFormat` attribute to a property in the model.</span></span> <span data-ttu-id="1cad5-136">Par exemple, le code suivant génère la date à afficher sans le temps :</span><span class="sxs-lookup"><span data-stu-id="1cad5-136">For example, the following code causes the date to be displayed without the time:</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- <span data-ttu-id="1cad5-137">Appliquer un [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribut à une propriété dans le modèle et en spécifiant le type de données.</span><span class="sxs-lookup"><span data-stu-id="1cad5-137">Applying a [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attribute to a property in the model and specifying the data type.</span></span> <span data-ttu-id="1cad5-138">Par exemple, le code suivant génère la date à afficher sans le temps.</span><span class="sxs-lookup"><span data-stu-id="1cad5-138">For example, the following code causes the date to be displayed without the time.</span></span>

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    <span data-ttu-id="1cad5-139">Si l’application contient un *date.cshtml* modèle dans le *Views\Shared\DisplayTemplates* dossier ou le *Views\Movies\DisplayTemplates* dossier, ce modèle permet de restituer le `DateTime` propriété.</span><span class="sxs-lookup"><span data-stu-id="1cad5-139">If the application contains a *date.cshtml* template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder, that template will be used to render the `DateTime` property.</span></span> <span data-ttu-id="1cad5-140">Sinon, le système de création de modèles ASP.NET intégré affiche la propriété en tant que date.</span><span class="sxs-lookup"><span data-stu-id="1cad5-140">Otherwise the built-in ASP.NET templating system displays the property as a date.</span></span>
- <span data-ttu-id="1cad5-141">Création d’un modèle d’affichage dans le *Views\Shared\DisplayTemplates* dossier ou le *Views\Movies\DisplayTemplates* dossier dont le nom correspond au type de données que vous souhaitez mettre en forme.</span><span class="sxs-lookup"><span data-stu-id="1cad5-141">Creating a display template in the *Views\Shared\DisplayTemplates* folder or the *Views\Movies\DisplayTemplates* folder whose name matches the data type that you want to format.</span></span> <span data-ttu-id="1cad5-142">Par exemple, vous l’avez vu que le *Views\Shared\DisplayTemplates\DateTime.cshtml* a été utilisé pour restituer `DateTime` propriétés dans un modèle, sans ajout d’un attribut au modèle et sans ajout de tout balisage aux vues.</span><span class="sxs-lookup"><span data-stu-id="1cad5-142">For example, you saw that the *Views\Shared\DisplayTemplates\DateTime.cshtml* was used to render `DateTime` properties in a model, without adding an attribute to the model and without adding any markup to views.</span></span>
- <span data-ttu-id="1cad5-143">À l’aide de la [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribut sur le modèle pour spécifier le modèle pour afficher la propriété de modèle.</span><span class="sxs-lookup"><span data-stu-id="1cad5-143">Using the [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) attribute on the model to specify the template to display the model property.</span></span>
- <span data-ttu-id="1cad5-144">Ajouter explicitement le nom de modèle d’affichage pour le [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) appeler dans une vue.</span><span class="sxs-lookup"><span data-stu-id="1cad5-144">Explicitly adding the display template name to the [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) call in a view.</span></span>

<span data-ttu-id="1cad5-145">L’approche que vous utilisez dépend de ce que vous devez faire dans votre application.</span><span class="sxs-lookup"><span data-stu-id="1cad5-145">The approach you use depends on what you need to do in your application.</span></span> <span data-ttu-id="1cad5-146">Il n’est pas rare de combiner ces approches pour obtenir exactement le type de mise en forme dont vous avez besoin.</span><span class="sxs-lookup"><span data-stu-id="1cad5-146">It's not uncommon to mix these approaches to get exactly the kind of formatting that you need.</span></span>

<span data-ttu-id="1cad5-147">Dans la section suivante, vous allez basculer engins un peu et la déplacer à partir de la personnalisation de l’affichage des données pour personnaliser la façon dont il est entré.</span><span class="sxs-lookup"><span data-stu-id="1cad5-147">In the next section, you'll switch gears a bit and move from customizing how data is displayed to customizing how it's entered.</span></span> <span data-ttu-id="1cad5-148">Vous devez raccorder le sélecteur de dates jQuery dans les vues de modification de l’application afin de fournir une belle façon de spécifier des dates.</span><span class="sxs-lookup"><span data-stu-id="1cad5-148">You'll hook up the jQuery datepicker to the edit views in the application in order to provide a slick way to specify dates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1cad5-149">[Précédent](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [Suivant](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="1cad5-149">[Previous](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
[Next](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)</span></span>
