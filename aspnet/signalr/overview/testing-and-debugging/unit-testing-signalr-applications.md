---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Tests unitaires d’Applications de SignalR | Microsoft Docs
author: pfletcher
description: Cet article décrit comment utiliser les fonctionnalités Unit Testing de SignalR 2.0.
ms.author: riande
ms.date: 06/10/2014
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: ba8f5d4577403fe9765641d7ee5d88bde045680a
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910822"
---
<a name="unit-testing-signalr-applications"></a>Unité test des Applications SignalR
====================
par [Patrick Fletcher](https://github.com/pfletcher)

> Cet article décrit l’aide des fonctionnalités de SignalR 2 Unit Testing.
>
> ## <a name="software-versions-used-in-this-topic"></a>Versions des logiciels utilisées dans cette rubrique
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR version 2
>
>
>
> ## <a name="questions-and-comments"></a>Questions et commentaires
>
> Veuillez laisser des commentaires sur la façon dont vous avez apprécié ce didacticiel et ce que nous pouvions améliorer dans les commentaires en bas de la page. Si vous avez des questions qui ne sont pas directement liées à ce didacticiel, vous pouvez les publier à le [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [StackOverflow.com](http://stackoverflow.com/).


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a>Tests unitaires d’applications de SignalR

Vous pouvez utiliser les fonctionnalités de test unitaire dans SignalR 2 pour créer des tests unitaires pour votre application SignalR. SignalR 2 inclut le [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interface, ce qui peut être utilisé pour créer un objet factice afin de simuler vos méthodes de concentrateur de test.

Dans cette section, vous allez ajouter des tests unitaires pour l’application créée dans le [didacticiel mise en route](../getting-started/tutorial-getting-started-with-signalr.md) à l’aide de [XUnit.net](https://github.com/xunit/xunit) et [Moq](https://github.com/Moq/moq4).

XUnit.net permet de contrôler le test ; Moq permet de créer un [simuler](http://en.wikipedia.org/wiki/Mock_object) objet pour le test. Autres infrastructures factices peuvent être utilisés si vous le souhaitez ; [NSubstitute](http://nsubstitute.github.io/) est également un bon choix. Ce didacticiel montre comment configurer l’objet factice de deux manières : tout d’abord, à l’aide un `dynamic` objet (introduite dans .NET Framework 4) et la seconde, à l’aide d’une interface.

### <a name="contents"></a>Sommaire

Ce didacticiel contient les sections suivantes.

- [Tests unitaires avec dynamique](#dynamic)
- [Tests unitaires en type](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a>Tests unitaires avec dynamique

Dans cette section, vous allez ajouter un test unitaire pour l’application créée dans le [didacticiel mise en route](../getting-started/tutorial-getting-started-with-signalr.md) à l’aide d’un objet dynamique.

1. Installer le [extension de Test XUnit Runner](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) pour Visual Studio 2013.
2. Terminer la [didacticiel mise en route](../getting-started/tutorial-getting-started-with-signalr.md), ou téléchargez l’application terminée à partir de [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
3. Si vous utilisez la version de téléchargement de l’application de mise en route, ouvrez **Console du Gestionnaire de Package** et cliquez sur **restaurer** pour ajouter le package de SignalR au projet.

    ![Restaurer des Packages](unit-testing-signalr-applications/_static/image1.png)
4. Ajouter un projet à la solution pour le test unitaire. Avec le bouton droit de votre solution dans **l’Explorateur de solutions** et sélectionnez **ajouter**, **nouveau projet...** . Sous le **c#** nœud, sélectionnez le **Windows** nœud. Sélectionnez **bibliothèque de classes**. Nommez le nouveau projet **TestLibrary** et cliquez sur **OK**.

    ![Créer la bibliothèque de tests](unit-testing-signalr-applications/_static/image2.png)
5. Ajoutez une référence dans le projet de bibliothèque de test au projet SignalRChat. Cliquez sur le **TestLibrary** de projet et sélectionnez **ajouter**, **référence...** . Sélectionnez le **projets** nœud sous la **Solution** nœud, puis vérifiez **SignalRChat**. Cliquez sur **OK**.

    ![Ajouter une référence de projet](unit-testing-signalr-applications/_static/image3.png)
6. Ajoutez les packages SignalR, Moq et XUnit pour le **TestLibrary** projet. Dans le **Console du Gestionnaire de Package**, définissez le **projet par défaut** menu déroulant pour **TestLibrary**. Exécutez les commandes suivantes dans la fenêtre de console :

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Installer des Packages](unit-testing-signalr-applications/_static/image4.png)
7. Créez le fichier de test. Cliquez sur le **TestLibrary** projet puis cliquez sur **ajouter...** , **Classe**. Nommez la nouvelle classe **Tests.cs**.
8. Remplacez le contenu de Tests.cs par le code suivant.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    Dans le code ci-dessus, un client de test est créé à l’aide de la `Mock` de l’objet à partir de la [Moq](https://github.com/Moq/moq4) bibliothèque, de type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (dans SignalR 2.1, affectez `dynamic` pour le type paramètre.) Le `IHubCallerConnectionContext` interface est l’objet de proxy avec lequel vous appelez des méthodes sur le client. Le `broadcastMessage` fonction est ensuite définie pour le client fictif afin qu’il peut être appelé le `ChatHub` classe. Le moteur de test appelle alors la `Send` méthode de la `ChatHub` (classe), qui à son tour appelle la factices `broadcastMessage` (fonction).
9. Générez la solution en appuyant sur **F6**.
10. Exécutez le test unitaire. Dans Visual Studio, sélectionnez **Test**, **Windows**, **Explorateur de tests**. Dans la fenêtre Explorateur de tests, cliquez sur **HubsAreMockableViaDynamic** et sélectionnez **exécuter les Tests sélectionnés**.

    ![Explorateur de tests](unit-testing-signalr-applications/_static/image5.png)
11. Vérifiez que le test a réussi en vérifiant le volet inférieur dans la fenêtre Explorateur de tests. La fenêtre affiche que le test a réussi.

    ![Test réussi](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a>Tests unitaires en type

Dans cette section, vous allez ajouter un test de l’application créée dans le [didacticiel mise en route](../getting-started/tutorial-getting-started-with-signalr.md) à l’aide d’une interface qui contient la méthode à tester.

1. Effectuez les étapes 1 à 7 dans le [tests unitaires avec Dynamic](#dynamic) didacticiel ci-dessus.
2. Remplacez le contenu de Tests.cs par le code suivant.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    Dans le code ci-dessus, une interface est créée en définissant la signature de la `broadcastMessage` pour lequel le moteur de test créera un client fictif de méthode. Un client fictif est ensuite créé à l’aide de la `Mock` objet de type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (dans SignalR 2.1, affectez `dynamic` pour le paramètre de type.) Le `IHubCallerConnectionContext` interface est l’objet de proxy avec lequel vous appelez des méthodes sur le client.

    Le test crée ensuite une instance de `ChatHub`, puis crée une version fictive de la `broadcastMessage` (méthode), qui à son tour, est appelé en appelant le `Send` méthode du concentrateur.
3. Générez la solution en appuyant sur **F6**.
4. Exécutez le test unitaire. Dans Visual Studio, sélectionnez **Test**, **Windows**, **Explorateur de tests**. Dans la fenêtre Explorateur de tests, cliquez sur **HubsAreMockableViaDynamic** et sélectionnez **exécuter les Tests sélectionnés**.

    ![Explorateur de tests](unit-testing-signalr-applications/_static/image7.png)
5. Vérifiez que le test a réussi en vérifiant le volet inférieur dans la fenêtre Explorateur de tests. La fenêtre affiche que le test a réussi.

    ![Test réussi](unit-testing-signalr-applications/_static/image8.png)
