---
uid: mvc/overview/advanced/custom-mvc-templates
title: Modèle MVC personnalisé | Microsoft Docs
author: joeloff
description: Créer un modèle comme extension VSIX.
ms.author: aspnetcontent
ms.date: 12/10/2012
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: 67cb01e0fae036f01b1851695ae5a4358136d28e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809775"
---
<a name="custom-mvc-template"></a>Modèle MVC personnalisé
====================
par [Jacques Eloff](https://github.com/joeloff)

La version de MVC 3 Tools Update pour Visual Studio 2010 a introduit un Assistant de projet distinct pour les projets MVC. La modification a été motivée par deux facteurs. Tout d’abord, l’introduction de nouveaux modèles dans MVC 3 et la prise en charge pour les moteurs d’affichage supplémentaires tels que Razor entraîner surpeuplement la boîte de dialogue Nouveau projet dans Visual Studio. Deuxièmement, les clients avaient été demandé pour les points d’extensibilité et l’Assistant Nouveau projet MVC serait nous offre l’opportunité de répondre à ces demandes.

L’ajout de modèles personnalisés était un processus complexe qui s’appuyaient sur l’utilisation du Registre pour que les nouveaux modèles visible par l’Assistant de projet MVC. L’auteur d’un nouveau modèle avait à la ligne à l’intérieur d’un fichier MSI pour vous assurer que les entrées de Registre nécessaires seraient créées au moment de l’installation. L’alternative était de créer un fichier ZIP qui contient le modèle disponible et ont l’utilisateur final de créer les entrées de Registre requises à la main.

Aucun des approches mentionnées précédemment est idéale afin de nous avons décidé de tirer parti de l’infrastructure existante fournie par [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) extensions pour faciliter l’auteur, distribuer et installer des modèles MVC personnalisés à partir de MVC 4 pour Visual Studio 2012. Certains des avantages fournis par cette approche sont :

- Une extension VSIX peut contenir plusieurs modèles qui prennent en charge différentes langues (c# et Visual Basic) et plusieurs moteurs d’affichage (ASPX et Razor).
- Une extension VSIX peut cibler plusieurs références (SKU) de Visual Studio, y compris les références (SKU) Express.
- Le [galerie Visual Studio](https://visualstudiogallery.msdn.microsoft.com/) facilite la distribution de l’extension à un large public.
- Extensions VSIX peuvent être mis à niveau ce qui facilite la création des corrections et mises à jour de vos modèles personnalisés.

## <a name="prerequisites"></a>Prérequis

- Les utilisateurs doivent être familiarisé avec la création de modèles de projet, y compris le balisage requis pour les fichiers vstemplate, etc.
- Les utilisateurs devront disposer de Visual Studio Professional et versions ultérieures. Références SKU Express ne gèrent pas la création de projets VSIX.
- [Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668) installé.

## <a name="example"></a>Exemple

La première étape consiste à créer un projet VSIX à l’aide de c# ou Visual Basic. Sélectionnez **fichier > Nouveau projet**, puis cliquez sur **extensibilité** dans le volet gauche, sélectionnez le **projet VSIX**.

![Nouveau projet](custom-mvc-templates/_static/image1.jpg)

Une fois que le projet est créé, le concepteur VSIX s’ouvre.

![Métadonnées de Concepteur de projet](custom-mvc-templates/_static/image2.jpg)

Le concepteur peut être utilisé pour modifier certaines des propriétés générales de l’extension qui sera affiché aux utilisateurs quand ils installer l’extension ou parcourir les extensions installées dans Visual Studio (**Outils > Extensions et mises à jour**). Une fois que vous avez terminé les informations générales cliquez sur le **onglet de cibles d’installation**.

![Cibles d’installation de Concepteur de projet](custom-mvc-templates/_static/image3.jpg)

Cet onglet permet de spécifier les références (SKU) et les versions de Visual Studio qui sont prises en charge par votre extension. Activez la case à cocher pour **ce VSIX est installée pour tous les utilisateurs** pour activer l’installation sur une machine de l’extension VSIX. Cliquez sur le **New** bouton sur la droite pour ajouter des références supplémentaires tels que Web Developer Express (VWD).

![Ajouter une nouvelle Installation cible](custom-mvc-templates/_static/image4.jpg)

Si vous avez l’intention de prendre en charge tous les professionnels et versions ultérieures SKU (Professional, Premium et Ultimate) il vous suffit de sélectionner la référence (SKU) minimale de la famille, **Microsoft.VisualStudio.Pro**. Pensez à enregistrer toutes vos modifications une fois que vous avez terminé les cibles d’installation.

![Cibles d’installation de Concepteur de projet](custom-mvc-templates/_static/image5.jpg)

Le **actifs** onglet est utilisé pour ajouter tous les fichiers de votre contenu à l’extension VSIX. Étant donné que MVC requiert des métadonnées personnalisées vous aurez à modifier le code XML brut du fichier manifeste VSIX au lieu d’utiliser le **actifs** onglet pour ajouter du contenu. Commencez par ajouter le contenu du modèle de projet VSIX. Il est important que la structure du dossier et le contenu reflète la disposition du projet. L’exemple ci-dessous contient quatre modèles de projet qui ont été dérivées à partir du modèle de projet MVC de base. Assurez-vous que tous les fichiers qui composent votre modèle de projet (tous les éléments sous le dossier ProjectTemplates) sont ajoutés à la **contenu** itemgroup dans l’extension VSIX fichier projet et que chaque élément contient le  **CopyToOutputDirectory** et **IncludeInVsix** métadonnées définies comme indiqué dans l’exemple ci-dessous.

&lt;Inclure du contenu =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;

&lt;CopyToOutputDirectory&gt;Always&lt;/CopyToOutputDirectory&gt;

&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;

&lt;/ Contenu&gt;

Si ce n’est pas le cas, l’IDE va tenter de compiler le contenu du modèle lorsque vous générez l’extension VSIX et vous verrez probablement une erreur. Fichiers de code dans les modèles contiennent souvent spéciale [paramètres de modèle](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) utilisé par Visual Studio lorsque le modèle de projet est instancié et par conséquent ne peut pas être compilé dans l’IDE.

![Explorateur de solutions](custom-mvc-templates/_static/image6.jpg)

Fermez le concepteur VSIX, cliquez avec le bouton droit sur le **source.extension.manifest** de fichiers dans **l’Explorateur de solutions** et sélectionnez **ouvrir avec** et choisissez le **() XML Éditeur de texte)** option.

![Ouvrez la boîte de dialogue](custom-mvc-templates/_static/image7.jpg)

Créer un **&lt;actifs&gt;** élément et ajoutez un **&lt;Asset&gt;** élément pour chaque fichier qui doit être inclus dans le VSIX. Le **Type** attribut de chaque **&lt;Asset&gt;** élément doit être défini sur **Microsoft.VisualStudio.Mvc.Template**. Il s’agit d’un espace de noms personnalisé qui comprend uniquement l’Assistant de projet MVC. Reportez-vous à la documentation de schéma de VSIX 2.0 pour plus d’informations sur la structure et la disposition du fichier manifeste.

Ajouter simplement les fichiers à l’extension VSIX n’est pas suffisante pour inscrire les modèles avec l’Assistant MVC. Vous devez fournir des informations telles que le nom du modèle de désignation, moteurs d’affichage pris en charge et de langage de programmation à l’Assistant MVC. Cette information n’est transmise dans les attributs personnalisés associés à la **&lt;Asset&gt;** élément pour chaque **vstemplate** fichier.

&lt;Ressource d:VsixSubPath =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;

Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;

d:Source=&quot;File&quot;

Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;

ProjectType=&quot;MVC&quot;

Language =&quot;c#&quot;

ViewEngine =&quot;Aspx&quot;

TemplateId=&quot;MyMvcApplication&quot;

Title =&quot;Application Web de base personnalisée&quot;

Description =&quot;dérivée d’un modèle personnalisé à partir d’une application web de base MVC (Razor)&quot;

Version =&quot;4.0&quot;/&gt;

Voici une explication des attributs personnalisés qui doivent être présents :

- **ProjectType** doit être définie sur MVC.
- **Langage** désigne le langage de développement pris en charge par le modèle. Les valeurs valides sont les c# ou VB.
- **ViewEngine** désigne le moteur d’affichage pris en charge par le modèle telles que Aspx ou Razor. Vous pouvez spécifier une valeur personnalisée pour ce champ.
- **TemplateId** permet de regrouper les modèles. Si la valeur correspond à un ID de modèle existant, il sera remplacer des modèles précédemment inscrits avec l’Assistant MVC.
- **Titre** désigne la description courte est affichée dans l’Assistant MVC en dessous de chaque modèle de projet.
- **Description** désigne une description plus détaillée du modèle.

Une fois que vous avez ajouté tous les fichiers du manifeste et enregistré, vous remarquerez que le **actifs** onglet dans le concepteur affiche tous les fichiers, mais pas les attributs personnalisés que vous avez ajouté à la **&lt;Asset&gt;** éléments pour le **vstemplate** fichiers.

![Ressources de Concepteur de projet](custom-mvc-templates/_static/image8.jpg)

Il reste maintenant qu’à compiler le projet VSIX et installez-le.

S’assurer que toutes les instances de Visual Studio sont fermées sur l’ordinateur où vous avez l’intention de tester l’extension VSIX. Visual Studio analyse les nouvelles extensions lors du démarrage, si l’IDE est ouvert lors de l’installation d’une extension VSIX, vous devrez redémarrer Visual Studio. Dans l’Explorateur de solutions, double-cliquez sur le fichier VSIX pour lancer le **programme d’installation VSIX**, cliquez sur **installer** , puis démarrez Visual Studio.

![Programme d’installation VSIX](custom-mvc-templates/_static/image9.jpg)

Dans le menu, sélectionnez **Outils > Extensions et mises à jour** pour confirmer que votre extension a été installée. Si le programme d’installation de VSIX a signalé des erreurs pendant l’installation de l’extension, vous pouvez afficher le journal du programme d’installation VSIX pour plus d’informations. Le journal est généralement créé dans le **%temp%** dossier de l’utilisateur qui a installé l’extension, par exemple **C:\Users\Bob\AppData\Local\Temp**.

![Extensions et mises à jour](custom-mvc-templates/_static/image10.jpg)

Après la fermeture de la fenêtre, vous pouvez créer un projet MVC 4 pour voir si vos nouveaux modèles sont affichés dans l’Assistant MVC.

![Nouveau projet ASP.NET MVC 4](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>Limitations

1. L’Assistant MVC ne gère pas les modèles personnalisés localisées.
2. L’Assistant ne sera pas signalé d’erreur en cas d’échec localiser des modèles personnalisés. Si un des attributs personnalisés requis est absent, le modèle doit simplement être exclu de l’Assistant.
