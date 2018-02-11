---
title: Application de SSL dans une application ASP.NET Core
author: rick-anderson
description: "Montre comment exiger le protocole SSL dans un cœur d’ASP.NET web app"
manager: wpickett
ms.author: riande
ms.date: 07/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 3b72cddb7a240ad6d6e1427796e9bb4f7003a3f7
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/03/2018
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a>Application de SSL dans une application ASP.NET Core

Par [Rick Anderson](https://twitter.com/RickAndMSFT)

Ce document montre comment :

- Exiger le protocole SSL pour toutes les demandes (uniquement pour les requêtes HTTPS).
- Rediriger toutes les demandes HTTP vers HTTPS.

## <a name="require-ssl"></a>Exiger SSL

L'attribut [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) est utilisé pour exiger le protocol SSL. Vous pouvez décorer les contrôleurs ou méthodes avec cet attribut, ou vous pouvez l’appliquer globalement, comme indiqué ci-dessous :

Ajoutez le code suivant à la méthode `ConfigureServices` dans la classe `Startup`:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Le code en surbrillance ci-dessus require l’utilisation du protocol `HTTPS` pour toutes les demandes, par conséquent, les requêtes HTTP sont ignorés. 
Le code en surbrillance suivant redirige toutes les demandes HTTP vers HTTPS :

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Consultez la documentation [Réécriture d’URL intergiciel (middleware)](xref:fundamentals/url-rewriting) pour plus d’informations.

Exiger le protocol HTTPS globalement (`options.Filters.Add(new RequireHttpsAttribute());`) est une meilleure pratique de sécurité. Appliquer l'attribut `[RequireHttps]` à tous les contrôleur n’est pas considéré aussi sécurisée que exiger le HTTPS globalement, car vous ne pouvez pas garantir la sécurité aux nouveaux contrôleurs ajoutées à posteriori dans votre application. Il faudra donc penser à appliquer l'attribut `[RequireHttps]` sur ces nouveaux contrôleurs.
