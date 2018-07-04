---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Présentation du processus de l’exécution d’ASP.NET MVC | Microsoft Docs
author: microsoft
description: Découvrez comment l’infrastructure ASP.NET MVC traite une demande de navigateur pas à pas.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: d94fcedb6e0ec6134bdbd69729abf1f875fac420
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378269"
---
<a name="understanding-the-aspnet-mvc-execution-process"></a>Présentation du processus de l’exécution d’ASP.NET MVC
====================
par [Microsoft](https://github.com/microsoft)

> Découvrez comment l’infrastructure ASP.NET MVC traite une demande de navigateur pas à pas.


Demandes à une application Web basée sur ASP.NET MVC traversent d’abord le **UrlRoutingModule** objet, qui est un module HTTP. Ce module analyse la demande et effectue une sélection de l’itinéraire. Le **UrlRoutingModule** objet sélectionne le premier objet d’itinéraire qui correspond à la requête actuelle. (Un objet d’itinéraire est une classe qui implémente **RouteBase**, et est généralement une instance de la **itinéraire** classe.) Si aucun itinéraire ne correspond, le **UrlRoutingModule** objet ne fait rien et la demande vous permet de revenir à la demande ASP.NET ou IIS normal traitement.

À partir de l’élément sélectionné **itinéraire** objet, le **UrlRoutingModule** Obtient le **IRouteHandler** objet auquel est associé le **itinéraire**objet. En règle générale, dans une application MVC, ce sera une instance de **MvcRouteHandler**. Le **IRouteHandler** instance crée un **IHttpHandler** objet et lui passe la **IHttpContext** objet. Par défaut, le **IHttpHandler** d’instance pour MVC est la **MvcHandler** objet. Le **MvcHandler** objet sélectionne ensuite le contrôleur qui gérera finalement la demande.

> [!NOTE]
> Quand une application Web ASP.NET MVC s’exécute dans IIS 7.0, aucune extension de nom de fichier n’est requise pour les projets MVC. Toutefois, dans IIS 6.0, le gestionnaire requiert que vous mapper l’extension de nom de fichier .mvc à la DLL ISAPI ASP.NET.


Le module et le gestionnaire sont les points d’entrée à l’infrastructure ASP.NET MVC. Ils effectuent les actions suivantes :

- Sélection du contrôleur approprié dans une application Web MVC.
- Pour obtenir une instance de contrôleur spécifique.
- Appeler le contrôleur **Execute** (méthode).

La liste suivante décrit les étapes d’exécution d’un projet Web MVC :

- Première demande concernant l’application de réception 

    - Dans le fichier Global.asax, **itinéraire** objets sont ajoutés à la **RouteTable** objet.
- Effectuer un routage 

    - Le **UrlRoutingModule** module utilise la première correspondance **itinéraire** de l’objet dans le **RouteTable** collection pour créer le **RouteData** objet, qu’il utilise ensuite pour créer un **RequestContext** (**IHttpContext**) objet.
- Créer le Gestionnaire de demandes MVC 

    - Le **MvcRouteHandler** objet crée une instance de la **MvcHandler** classe et le transmet le **RequestContext** instance.
- Créer le contrôleur 

    - Le **MvcHandler** objet utilise le **RequestContext** instance pour identifier le **IControllerFactory** objet (en général, une instance de la  **DefaultControllerFactory** classe) pour créer l’instance de contrôleur.
- Execute controller - le **MvcHandler** instance appelle le contrôleur s **Execute** (méthode). |
- Appeler l’action 

    - La plupart des contrôleurs héritent le **contrôleur** classe de base. Pour les contrôleurs qui le faire, le **ControllerActionInvoker** objet qui est associé au contrôleur détermine la méthode d’action de la classe de contrôleur à appeler, puis appelle cette méthode.
- Exécution du résultat 

    - Une méthode d’action par défaut peut recevoir l’entrée d’utilisateur, préparer les données de la réponse appropriée, puis exécutez le résultat en retournant un type de résultat. Les types de résultats intégrés qui peuvent être exécutés incluent les éléments suivants : **ViewResult** (qui restitue une vue et est le type de résultat plus fréquemment utilisé), **RedirectToRouteResult**,  **RedirectResult**, **ContentResult**, **JsonResult**, et **EmptyResult**.
