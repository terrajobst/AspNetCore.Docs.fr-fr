---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Self-Host ASP.NET Web API 1 (c#) | Microsoft Docs
author: MikeWasson
description: API Web ASP.NET ne nécessite pas d’IIS. Vous pouvez Self-host une API web dans votre propre processus hôte. Ce didacticiel montre comment héberger une API web à l’intérieur d’une console appl...
ms.author: riande
ms.date: 01/26/2012
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: cac0d5aeaf49f45051d062935e0e9207ce59c7eb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833437"
---
<a name="self-host-aspnet-web-api-1-c"></a>Self-Host ASP.NET Web API 1 (c#)
====================
par [Mike Wasson](https://github.com/MikeWasson)

> API Web ASP.NET ne nécessite pas d’IIS. Vous pouvez Self-host une API web dans votre propre processus hôte. Ce didacticiel montre comment héberger une API web à l’intérieur d’une application console.
> 
> **Nouvelles applications doivent utiliser OWIN pour auto-héberger API Web.** Consultez [utiliser OWIN pour auto-héberger ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - Web API 1
> - Visual Studio 2012


## <a name="create-the-console-application-project"></a>Créer le projet d’Application Console

Démarrez Visual Studio et sélectionnez **nouveau projet** à partir de la **Démarrer** page. Ou, à partir de la **fichier** menu, sélectionnez **New** , puis **projet**.

Dans le **modèles** volet, sélectionnez **modèles installés** et développez le **Visual C#** nœud. Sous **Visual C#**, sélectionnez **Windows**. Dans la liste des modèles de projet, sélectionnez **Application Console**. Nommez le projet &quot;SelfHost&quot; et cliquez sur **OK**.

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>Définir le Framework cible (Visual Studio 2010)

Si vous utilisez Visual Studio 2010, modifier le framework cible .NET Framework 4.0. (Par défaut, le modèle de projet cible le [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)

Dans l’Explorateur de solutions, cliquez sur le projet et sélectionnez **propriétés**. Dans le **framework cible** liste déroulante liste, modifier le framework cible .NET Framework 4.0. Lorsque vous y êtes invité pour appliquer la modification, cliquez sur **Oui**.

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>Installer le Gestionnaire de Package NuGet

Le Gestionnaire de Package NuGet est le moyen le plus simple pour ajouter les assemblys de l’API Web à un projet non-ASP.NET.

Pour vérifier si le Gestionnaire de Package NuGet est installé, cliquez sur le **outils** menu dans Visual Studio. Si vous voyez un menu appelé **Library Package Manager**, vous devez le Gestionnaire de Package NuGet.

Pour installer le Gestionnaire de Package NuGet :

1. Démarrez Visual Studio.
2. À partir de la **outils** menu, sélectionnez **Extensions et mises à jour**.
3. Dans le **Extensions et mises à jour** boîte de dialogue, sélectionnez **Online**.
4. Si vous ne voyez pas « Gestionnaire de Package NuGet », tapez « Gestionnaire de package nuget » dans la zone de recherche.
5. Sélectionnez le Gestionnaire de Package NuGet, cliquez sur **télécharger**.
6. Une fois le téléchargement terminé, vous seront invités à installer.
7. Une fois l’installation terminée, vous pouvez être invité à redémarrer Visual Studio.

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>Ajouter le Package API NuGet Web

Une fois que le Gestionnaire de Package NuGet est installé, ajoutez le package d’auto-héberger des API Web à votre projet.

1. À partir de la **outils** menu, sélectionnez **Library Package Manager**. *Remarque*: Si ne vous voyez pas ce menu item, assurez-vous que ce gestionnaire de Package NuGet installé correctement.
2. Sélectionnez **gérer les Packages NuGet pour la Solution...**
3. Dans le **gérer les Packages NuGet** boîte de dialogue, sélectionnez **Online**.
4. Dans la zone de recherche, tapez &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.
5. Sélectionnez le package ASP.NET Web API Self hôte et cliquez sur **installer**.
6. Une fois le package installé, cliquez sur **fermer** pour fermer la boîte de dialogue.

> [!NOTE]
> Veillez à installer le package nommé Microsoft.AspNet.WebApi.SelfHost, AspNetWebApi.SelfHost pas.


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>Créer le modèle et le contrôleur

Ce didacticiel utilise les mêmes classes de modèle et du contrôleur en tant que le [mise en route](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) didacticiel.

Ajoutez une classe publique nommée `Product`.

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

Ajoutez une classe publique nommée `ProductsController`. Cette classe dérive **System.Web.Http.ApiController**.

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

Pour plus d’informations sur le code dans ce contrôleur, consultez le [mise en route](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) didacticiel. Ce contrôleur définit trois actions GET :

| URI | Description |
| --- | --- |
| / api/produits | Obtenir une liste de tous les produits. |
| /API/produits/*id* | Obtenir un produit par ID. |
| / API/produits / ? catégorie =*catégorie* | Obtenir la liste des produits par catégorie. |

## <a name="host-the-web-api"></a>Héberger l’API Web

Ouvrez le fichier Program.cs et ajoutez le code suivant à l’aide d’instructions :

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

Ajoutez le code suivant à la **programme** classe.

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>(Facultatif) Ajouter une réservation Namespace d’URL HTTP

Cette application écoute `http://localhost:8080/`. Par défaut, à l’écoute sur une adresse HTTP particulière nécessite des privilèges d’administrateur. Lorsque vous exécutez le didacticiel, par conséquent, vous pouvez obtenir cette erreur : « HTTP n’a pas pu inscrire URL http://+:8080/« il existe deux façons d’éviter cette erreur :

- Exécuter Visual Studio avec des autorisations d’administrateur avec élévation de privilèges, ou
- Utilisez Netsh.exe pour accorder des autorisations de votre compte pour réserver l’URL.

Pour utiliser Netsh.exe, ouvrez une invite de commandes avec des privilèges d’administrateur et entrez la commande suivante de la commande : suivantes :

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

où *ORDINATEUR\nom d’utilisateur* est votre compte d’utilisateur.

Lorsque vous avez terminé d’auto-hébergement, veillez à supprimer la réservation :

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>Appeler l’API Web à partir d’une Application cliente (c#)

Nous allons écrire une application console simple qui appelle l’API web.

Ajouter un nouveau projet d’application console à la solution :

- Dans l’Explorateur de solutions, cliquez sur la solution et sélectionnez **ajouter un nouveau projet**.
- Créer une nouvelle application console nommée &quot;ClientApp&quot;.

![](self-host-a-web-api/_static/image5.png)

Utilisez le Gestionnaire de Package NuGet pour ajouter le package de bibliothèques de Core ASP.NET Web API :

- Dans le menu Outils, sélectionnez **Library Package Manager**.
- Sélectionnez **gérer les Packages NuGet pour la Solution...**
- Dans le **gérer les Packages NuGet** boîte de dialogue, sélectionnez **Online**.
- Dans la zone de recherche, tapez &quot;Microsoft.AspNet.WebApi.Client&quot;.
- Sélectionnez le package de bibliothèques clientes de Microsoft ASP.NET Web API et cliquez sur **installer**.

Ajoutez une référence dans ClientApp au projet SelfHost :

- Dans l’Explorateur de solutions, cliquez sur le projet ClientApp.
- Sélectionnez **Ajouter une référence**.
- Dans le **Gestionnaire de références** boîte de dialogue, sous **Solution**, sélectionnez **projets**.
- Sélectionnez le projet SelfHost.
- Cliquez sur **OK**.

![](self-host-a-web-api/_static/image6.png)

Ouvrez le fichier Client/Program.cs. Ajoutez le code suivant **à l’aide de** instruction :

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

Ajouter un mappage statique **HttpClient** instance :

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

Ajoutez les méthodes suivantes pour répertorier tous les produits, un produit par ID de liste et répertorient les produits par catégorie.

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

Chacune de ces méthodes suit le même modèle :

1. Appelez **HttpClient.GetAsync** pour envoyer une demande GET à l’URI approprié.
2. Appelez **HttpResponseMessage.EnsureSuccessStatusCode**. Cette méthode lève une exception si l’état de réponse HTTP est un code d’erreur.
3. Appelez **ReadAsAsync&lt;T&gt;**  pour désérialiser un type CLR de la réponse HTTP. Cette méthode est une méthode d’extension définie dans **System.Net.Http.HttpContentExtensions**.

Le **GetAsync** et **ReadAsAsync** méthodes sont toutes les deux asynchrones. Elles retournent **tâche** objets qui représentent l’opération asynchrone. Obtention de la **résultat** propriété bloque le thread jusqu'à ce que l’opération se termine.

Pour plus d’informations sur l’utilisation de HttpClient, y compris comment effectuer des appels non bloquant, consultez [appelant une Web API à partir d’un Client .NET](../advanced/calling-a-web-api-from-a-net-client.md).

Avant d’appeler ces méthodes, définissez la propriété BaseAddress sur l’instance de HttpClient pour «`http://localhost:8080`». Exemple :

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

Cela doit générer ce qui suit. (N’oubliez pas d’exécuter d’abord l’application SelfHost).

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
