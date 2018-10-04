---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Détection de classe de démarrage OWIN | Microsoft Docs
author: Praburaj
description: Ce didacticiel montre comment configurer la classe de démarrage OWIN est chargée. Pour plus d’informations sur OWIN, consultez une vue d’ensemble du projet Katana. Ce didacticiel a été...
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 49ea01bf2170698de66b633b260664ce3d105ca1
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577481"
---
<a name="owin-startup-class-detection"></a>Détection de classe de démarrage OWIN
====================
par [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Ce didacticiel montre comment configurer la classe de démarrage OWIN est chargée. Pour plus d’informations sur OWIN, consultez [une vue d’ensemble du projet Katana](an-overview-of-project-katana.md). Ce didacticiel a été rédigé par Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan et Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).
> 
> ## <a name="prerequisites"></a>Prérequis
> 
> [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a>Détection de classe de démarrage OWIN

 Chaque Application OWIN a une classe de démarrage où vous spécifiez des composants pour le pipeline d’application. Il existe différentes façons, vous pouvez vous connecter à votre classe de démarrage avec le runtime, selon le modèle d’hébergement que vous choisissez (OwinHost, IIS et IIS Express). La classe de démarrage présentée dans ce didacticiel peut être utilisée dans chaque application d’hébergement. Vous vous connectez à la classe de démarrage avec l’hébergement runtime à l’aide d’une de ces approches :  

1. **Convention de dénomination**: Katana recherche une classe nommée `Startup` dans l’espace de noms mise en correspondance le nom d’assembly ou de l’espace de noms global.
2. **Attribut de OwinStartup**: c’est l’approche prendra la plupart des développeurs pour spécifier la classe de démarrage. L’attribut suivant définit la classe de démarrage le `TestStartup` classe dans le `StartupDemo` espace de noms. 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   Le `OwinStartup` attribut remplace la convention d’affectation de noms. Vous pouvez également spécifier un nom convivial avec cet attribut, toutefois, à l’aide d’un nom convivial, vous devez également utiliser le `appSetting` élément dans le fichier de configuration.
3. **L’élément appSetting dans le fichier de Configuration**: le `appSetting` élément remplace la `OwinStartup` attribut et la convention d’affectation de noms. Vous pouvez avoir plusieurs classes de démarrage (chacun utilisant une `OwinStartup` attribut) et configurer la classe de démarrage qui sera chargé dans un fichier de configuration à l’aide de balisage similaire à ce qui suit :  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   La clé suivante, qui spécifie explicitement l’assembly et la classe de démarrage peut également être utilisée : 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   Le code XML suivant dans le fichier de configuration spécifie un nom de classe de démarrage convivial `ProductionConfiguration`.  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   Le balisage ci-dessus doit être utilisé par le code suivant `OwinStartup` attribut qui spécifie un nom convivial et provoque la `ProductionStartup2` classe à exécuter.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. Pour désactiver la détection de démarrage OWIN ajouter la `appSetting owin:AutomaticAppStartup` avec la valeur `"false"` dans le fichier web.config.

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>Créer une application de Web ASP.NET à l’aide de démarrage OWIN

1. Créez une application de web Asp.Net vide et nommez-la **StartupDemo**. -Installez `Microsoft.Owin.Host.SystemWeb` à l’aide du Gestionnaire de package NuGet. À partir de la **outils** menu, sélectionnez **Library Package Manager**, puis **Console du Gestionnaire de Package**. Entrez la commande suivante :  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. Ajouter une classe de démarrage OWIN. Dans Visual Studio 2013 avec le bouton droit sur le projet et sélectionnez **ajouter une classe**. - dans le **ajouter un nouvel élément** boîte de dialogue, entrez *OWIN* dans le champ de recherche, puis remplacez le nom par Startup.cs, puis cliquez sur **ajouter**.  
  
     ![](owin-startup-class-detection/_static/image1.png)   
  
   La prochaine fois que vous souhaitez ajouter un *classe de démarrage Owin*, il sera dans disponible à partir de la **ajouter** menu.  
   
     ![](owin-startup-class-detection/_static/image2.png)  
  
   Ou bien, vous pouvez cliquez avec le bouton droit sur le projet et sélectionnez **ajouter**, puis sélectionnez **un nouvel élément**, puis sélectionnez le **classe de démarrage Owin**.  
  
     ![](owin-startup-class-detection/_static/image3.png)  
  
- Remplacez le code généré dans le *Startup.cs* fichier par le code suivant :  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
  Le `app.Use` expression lambda est utilisée pour enregistrer le composant d’intergiciel (middleware) spécifié au pipeline OWIN. Dans ce cas, nous configurons la journalisation des demandes entrantes avant de répondre à la demande entrante. Le `next` paramètre est le délégué ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [tâche](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) au composant suivant dans le pipeline. Le `app.Run` raccorde le pipeline aux demandes entrantes de l’expression lambda et fournit le mécanisme de réponse.
     > [!NOTE]
     > Dans le code ci-dessus nous avons mis en commentaire le `OwinStartup` attribut et nous allons s’appuyer sur la convention de l’exécution de la classe nommée `Startup` .-Press ***F5*** pour exécuter l’application. Cliquez sur Actualiser plusieurs fois.  
  
    ![](owin-startup-class-detection/_static/image4.png)  
  Remarque : Le nombre indiqué dans les images dans ce didacticiel pas correspond le numéro que vous voyez. La chaîne de milliseconde est utilisée pour afficher une nouvelle réponse lorsque vous actualisez la page.  
  Vous pouvez voir les informations de trace dans le **sortie** fenêtre.  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>Ajouter plusieurs Classes de démarrage

Dans cette section, nous allons ajouter une autre classe de démarrage. Vous pouvez ajouter plusieurs classe de démarrage OWIN à votre application. Par exemple, vous souhaiterez peut-être créer des classes de démarrage pour le développement, test et production.

1. Créez une nouvelle classe de démarrage OWIN et nommez-le `ProductionStartup`.
2. Remplacez le code généré par ce qui suit :

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. Appuyez sur F5 de contrôle pour exécuter l’application. Le `OwinStartup` attribut spécifie la classe de démarrage de production est exécutée.  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. Créez une autre classe de démarrage OWIN et nommez-le `TestStartup`.
5. Remplacez le code généré par ce qui suit :  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   Le `OwinStartup` surcharge attribut ci-dessus spécifie `TestingConfiguration` en tant que le *convivial* nom de la classe de démarrage.
6. Ouvrez le *web.config* fichier, puis ajoutez la clé de démarrage OWIN application qui spécifie le nom convivial de la classe de démarrage :

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. Appuyez sur F5 de contrôle pour exécuter l’application. L’élément de paramètres d’application est prioritaire et le test de configuration est exécutée.  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. Supprimer le *convivial* nom de la `OwinStartup` d’attribut dans la `TestStartup` classe.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. Remplacer la clé de démarrage OWIN application dans le *web.config* fichier par le code suivant :

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. Rétablir le `OwinStartup` attribut dans chaque classe pour le code d’attribut par défaut généré par Visual Studio :  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    Chacune des clés de démarrage OWIN application ci-dessous entraînera la classe de production s’exécute. 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    La dernière clé de démarrage spécifie la méthode de configuration de démarrage. La clé de démarrage OWIN application suivante vous permet de modifier le nom de la classe de configuration à `MyConfiguration` .

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>À l’aide d’Owinhost.exe

1. Remplacez le fichier Web.config par le balisage suivant :  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   La dernière clé de wins, donc dans ce cas `TestStartup` est spécifié.
2. Installer Owinhost à partir de la PMC : 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. Accédez au dossier d’application (le dossier qui contient le *Web.config* fichier) et dans une invite de commande et tapez : 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   Affiche la fenêtre de commande : 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. Lancer un navigateur avec l’URL `http://localhost:5000/`.  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
   OwinHost respecté les conventions de démarrage répertoriées ci-dessus.
5. Dans la fenêtre de commande, appuyez sur ENTRÉE pour quitter OwinHost.
6. Dans le `ProductionStartup` de classe, ajoutez l’attribut OwinStartup suivant qui spécifie un nom convivial de *ProductionConfiguration*.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. Dans l’invite de commandes tapez : 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   Chargement de la classe de démarrage de Production.  
    ![](owin-startup-class-detection/_static/image9.png)  
   Notre application a plusieurs classes de démarrage et dans cet exemple, nous avons différée quelle classe de démarrage pour charger jusqu'à l’exécution.
8. Tester les options de démarrage runtime suivantes :

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
