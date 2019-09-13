---
title: Gérer les erreurs dans les applications ASP.NET Core éblouissantes
author: guardrex
description: Découvrez comment l’ASP.NET Core éblouissants gère les exceptions non gérées et comment développer des applications qui détectent et gèrent les erreurs.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/06/2019
uid: blazor/handle-errors
ms.openlocfilehash: d3e261e83f375574339a8ce3428e8bfb73df4307
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70963986"
---
# <a name="handle-errors-in-aspnet-core-blazor-apps"></a>Gérer les erreurs dans les applications ASP.NET Core éblouissantes

Par [Steve Sanderson](https://github.com/SteveSandersonMS)

Cet article explique comment éblouissant gère les exceptions non gérées et comment développer des applications qui détectent et gèrent les erreurs.

## <a name="how-the-blazor-framework-reacts-to-unhandled-exceptions"></a>Comment le Framework éblouissant réagit aux exceptions non gérées

Le serveur éblouissant est un Framework avec état. Tandis que les utilisateurs interagissent avec une application, ils maintiennent une connexion au serveur appelé « *circuit*». Le circuit contient des instances de composant actives, ainsi que de nombreux autres aspects de l’État, tels que :

* Sortie du rendu le plus récent des composants.
* Ensemble actuel de délégués de gestion d’événements qui peuvent être déclenchés par les événements côté client.

Si un utilisateur ouvre l’application dans plusieurs onglets de navigateur, il dispose de plusieurs circuits indépendants.

Éblouissant traite la plupart des exceptions non gérées comme étant irrécupérables par le circuit dans lequel elles se produisent. Si un circuit est arrêté en raison d’une exception non gérée, l’utilisateur ne peut continuer à interagir avec l’application qu’en rechargeant la page pour créer un nouveau circuit. Les circuits en dehors de celui qui est terminé, qui sont des circuits pour d’autres utilisateurs ou d’autres onglets de navigateur, ne sont pas affectés. Ce scénario est similaire à une application de bureau qui&mdash;bloque l’application bloquée doit être redémarrée, mais les autres applications ne sont pas affectées.

Un circuit se termine lorsqu’une exception non gérée se produit pour les raisons suivantes :

* Une exception non gérée rend souvent le circuit dans un État indéfini.
* L’opération normale de l’application ne peut pas être garantie après une exception non gérée.
* Des failles de sécurité peuvent apparaître dans l’application si le circuit continue.

## <a name="manage-unhandled-exceptions-in-developer-code"></a>Gérer les exceptions non gérées dans le code du développeur

Pour qu’une application continue après une erreur, l’application doit avoir une logique de gestion des erreurs. Les sections suivantes de cet article décrivent les sources potentielles d’exceptions non gérées.

En production, ne rendez pas les messages d’exception d’infrastructure ou les traces de pile dans l’interface utilisateur. Le rendu des messages d’exception ou des traces de pile peut :

* Divulguer des informations sensibles aux utilisateurs finaux.
* Aidez un utilisateur malveillant à découvrir des faiblesses dans une application qui peuvent compromettre la sécurité de l’application, du serveur ou du réseau.

## <a name="log-errors-with-a-persistent-provider"></a>Consigner les erreurs avec un fournisseur persistant

Si une exception non gérée se produit, l’exception est consignée <xref:Microsoft.Extensions.Logging.ILogger> dans les instances configurées dans le conteneur de service. Par défaut, les applications éblouissantes se connectent à la sortie de la console avec le fournisseur de journalisation de la console. Envisagez de vous connecter à un emplacement plus permanent avec un fournisseur qui gère la taille du journal et la rotation des journaux. Pour plus d'informations, consultez <xref:fundamentals/logging/index>.

Lors du développement, éblouissant envoie généralement les détails complets des exceptions à la console du navigateur pour faciliter le débogage. En production, les erreurs détaillées dans la console du navigateur sont désactivées par défaut, ce qui signifie que les erreurs ne sont pas envoyées aux clients, mais que les détails complets de l’exception sont toujours consignés côté serveur. Pour plus d'informations, consultez <xref:fundamentals/error-handling>.

Vous devez choisir les incidents à enregistrer et le niveau de gravité des incidents journalisés. Les utilisateurs hostiles peuvent être en mesure de déclencher délibérément des erreurs. Par exemple, ne consignez pas un incident à partir d' `ProductId` une erreur où un inconnu est fourni dans l’URL d’un composant qui affiche les détails du produit. Toutes les erreurs ne doivent pas être traitées comme des incidents de gravité élevée pour la journalisation.

## <a name="places-where-errors-may-occur"></a>Emplacements où des erreurs peuvent se produire

Le code d’infrastructure et d’application peut déclencher des exceptions non prises en charge dans l’un des emplacements suivants :

* [Instanciation du composant](#component-instantiation)
* [Méthodes de cycle de vie](#lifecycle-methods)
* [Logique de rendu](#rendering-logic)
* [Gestionnaires d’événements](#event-handlers)
* [Suppression de composants](#component-disposal)
* [Interopérabilité JavaScript](#javascript-interop)
* [Gestionnaires de circuits](#circuit-handlers)
* [Élimination de circuit](#circuit-disposal)
* [Préaffichant](#prerendering)

Les exceptions non gérées précédentes sont décrites dans les sections suivantes de cet article.

### <a name="component-instantiation"></a>Instanciation du composant

Quand éblouissant crée une instance d’un composant :

* Le constructeur du composant est appelé.
* Les constructeurs de tout service d’injection de services non Singleton fourni au constructeur du composant via la [@inject](xref:blazor/dependency-injection#request-a-service-in-a-component) directive ou l’attribut [[Inject]](xref:blazor/dependency-injection#request-a-service-in-a-component) sont appelés. 

Un circuit échoue quand un constructeur exécuté ou une méthode setter pour `[Inject]` une propriété lève une exception non gérée. L’exception est irrécupérable, car l’infrastructure ne peut pas instancier le composant. Si la logique du constructeur peut lever des exceptions, l’application doit intercepter les exceptions à l’aide d’une instruction [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) avec la gestion des erreurs et la journalisation.

### <a name="lifecycle-methods"></a>Méthodes de cycle de vie

Pendant la durée de vie d’un composant, éblouissant appelle les méthodes de cycle de vie :

* `OnInitialized` / `OnInitializedAsync`
* `OnParametersSet` / `OnParametersSetAsync`
* `ShouldRender` / `ShouldRenderAsync`
* `OnAfterRender` / `OnAfterRenderAsync`

Si une méthode de cycle de vie lève une exception, de manière synchrone ou asynchrone, l’exception est irrémédiable pour le circuit. Pour les composants qui gèrent les erreurs dans les méthodes de cycle de vie, ajoutez une logique de gestion des erreurs.

Dans l’exemple suivant, `OnParametersSetAsync` où appelle une méthode pour obtenir un produit :

* Une exception levée dans la `ProductRepository.GetProductByIdAsync` méthode est gérée par `try-catch` une instruction.
* Lorsque le `catch` bloc est exécuté :
  * `loadFailed`a la valeur `true`, qui est utilisée pour afficher un message d’erreur à l’utilisateur.
  * L’erreur est consignée.

[!code-cshtml[](handle-errors/samples_snapshot/3.x/product-details.razor?highlight=11,27-39)]

### <a name="rendering-logic"></a>Logique de rendu

Le balisage déclaratif dans un `.razor` fichier de composant est compilé dans C# une méthode `BuildRenderTree`appelée. Lorsqu’un composant affiche, `BuildRenderTree` exécute et génère une structure de données décrivant les éléments, le texte et les composants enfants du composant rendu.

La logique de rendu peut lever une exception. Un exemple de ce scénario se produit `@someObject.PropertyName` lorsque est évalué `@someObject` , `null`mais est. Une exception non gérée levée par la logique de rendu est irrécupérable pour le circuit.

Pour éviter une exception de référence null dans la logique de rendu, `null` recherchez un objet avant d’accéder à ses membres. Dans l’exemple suivant, `person.Address` les propriétés ne sont `person.Address` pas `null`accessibles si est :

[!code-cshtml[](handle-errors/samples_snapshot/3.x/person-example.razor?highlight=1)]

Le code précédent suppose que `person` ne l’est pas. `null` Souvent, la structure du code garantit l’existence d’un objet au moment du rendu du composant. Dans ce cas, il n’est pas nécessaire de `null` Rechercher dans la logique de rendu. Dans l’exemple précédent, `person` peut être garanti qu’il existe `person` , car est créé lorsque le composant est instancié.

### <a name="event-handlers"></a>Gestionnaires d’événements

Le code côté client déclenche des appels de code lors C# de la création de gestionnaires d’événements à l’aide de :

* `@onclick`
* `@onchange`
* Autres `@on...` attributs
* `@bind`

Le code du gestionnaire d’événements peut lever une exception non gérée dans ces scénarios.

Si un gestionnaire d’événements lève une exception non gérée (par exemple, une requête de base de données échoue), l’exception est irrémédiable au circuit. Si l’application appelle du code qui pourrait échouer pour des raisons externes, interceptez les exceptions à l’aide d’une instruction [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) avec gestion des erreurs et journalisation.

Si le code utilisateur n’intercepte pas et ne gère pas l’exception, le Framework journalise l’exception et met fin au circuit.

### <a name="component-disposal"></a>Suppression de composants

Un composant peut être supprimé de l’interface utilisateur, par exemple, parce que l’utilisateur a accédé à une autre page. Quand un composant qui implémente <xref:System.IDisposable?displayProperty=fullName> est supprimé de l’interface utilisateur, le Framework appelle la méthode <xref:System.IDisposable.Dispose*> du composant. 

Si la méthode du `Dispose` composant lève une exception non gérée, l’exception est irrécupérable pour le circuit. Si la logique de suppression peut lever des exceptions, l’application doit intercepter les exceptions à l’aide d’une instruction [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) avec la gestion des erreurs et la journalisation.

Pour plus d’informations sur la suppression de <xref:blazor/components#component-disposal-with-idisposable>composants, consultez.

### <a name="javascript-interop"></a>Interopérabilité JavaScript

`IJSRuntime.InvokeAsync<T>`permet au code .NET d’effectuer des appels asynchrones au runtime JavaScript dans le navigateur de l’utilisateur.

Les conditions suivantes s’appliquent à la `InvokeAsync<T>`gestion des erreurs avec :

* Si un appel à `InvokeAsync<T>` échoue de façon synchrone, une exception .net se produit. Un appel à `InvokeAsync<T>` mon échec, par exemple, car les arguments fournis ne peuvent pas être sérialisés. Le code du développeur doit intercepter l’exception. Si le code d’application dans une méthode de gestionnaire d’événements ou de cycle de vie de composant ne gère pas une exception, l’exception résultante est irrécupérable pour le circuit.
* Si un appel à `InvokeAsync<T>` échoue de manière asynchrone, le <xref:System.Threading.Tasks.Task> .net échoue. Un appel à `InvokeAsync<T>` peut échouer, par exemple, parce que le code côté JavaScript lève une exception ou retourne un `Promise` qui s’est `rejected`terminé comme. Le code du développeur doit intercepter l’exception. Si vous utilisez l’opérateur [await](/dotnet/csharp/language-reference/keywords/await) , encapsulez l’appel de méthode dans une instruction [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) avec la gestion des erreurs et la journalisation. Dans le cas contraire, le code défaillant entraîne une exception non gérée qui est irrécupérable pour le circuit.
* Par défaut, les appels `InvokeAsync<T>` à doivent se terminer dans un laps de temps donné, sinon l’appel expire. Le délai d’expiration par défaut est d’une minute. Le délai d’attente protège le code contre toute perte de connectivité réseau ou de code JavaScript qui ne renvoie jamais de message d’achèvement. Si l’appel expire, le résultant `Task` échoue avec un <xref:System.OperationCanceledException>. Interceptez et traitez l’exception avec la journalisation.

De même, le code JavaScript peut initier des appels à des méthodes .NET indiquées par l' [attribut [JSInvokable]](xref:blazor/javascript-interop#invoke-net-methods-from-javascript-functions). Si ces méthodes .NET lèvent une exception non gérée :

* L’exception n’est pas traitée comme étant irrécupérable pour le circuit.
* Le côté `Promise` JavaScript est rejeté.

Vous avez la possibilité d’utiliser le code de gestion des erreurs côté .NET ou JavaScript de l’appel de méthode.

Pour plus d'informations, consultez <xref:blazor/javascript-interop>.

### <a name="circuit-handlers"></a>Gestionnaires de circuits

Éblouissant permet au code de définir un *Gestionnaire de circuit*, qui reçoit des notifications lorsque l’état du circuit d’un utilisateur change. Les États suivants sont utilisés :

* `initialized`
* `connected`
* `disconnected`
* `disposed`

Les notifications sont gérées en inscrivant un service d’injection `CircuitHandler` de services qui hérite de la classe de base abstraite.

Si les méthodes d’un gestionnaire de circuit personnalisé lèvent une exception non gérée, l’exception est irrécupérable pour le circuit. Pour tolérer des exceptions dans le code d’un gestionnaire ou les méthodes appelées, encapsulez le code dans une ou plusieurs instructions [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) avec la gestion des erreurs et la journalisation.

### <a name="circuit-disposal"></a>Élimination de circuit

Lorsqu’un circuit se termine parce qu’un utilisateur s’est déconnecté et que l’infrastructure nettoie l’état du circuit, le Framework supprime l’étendue de l’injection de port. La suppression de l’étendue supprime tous les services d’étendue de circuit qui implémentent <xref:System.IDisposable?displayProperty=fullName>. Si un service d’injection de services lève une exception non gérée pendant la suppression, le Framework journalise l’exception.

### <a name="prerendering"></a>Préaffichant

Les composants éblouissant peuvent être prérendus à `Html.RenderComponentAsync` l’aide de afin que le balisage HTML rendu soit renvoyé dans le cadre de la requête http initiale de l’utilisateur. Cela fonctionne de la façon suivante :

* Création d’un nouveau circuit contenant tous les composants prérendus qui font partie de la même page.
* Génération du code HTML initial.
* Traiter le circuit comme `disconnected` jusqu’à ce que le navigateur de l’utilisateur établisse une connexion signalr au même serveur pour reprendre l’interactivité sur le circuit.

Si un composant lève une exception non gérée pendant le prérendu, par exemple, pendant une méthode de cycle de vie ou dans une logique de rendu :

* L’exception est irrécupérable pour le circuit.
* L’exception est levée dans la pile des appels de `Html.RenderComponentAsync` l’appel. Par conséquent, la requête HTTP entière échoue, sauf si l’exception est explicitement interceptée par le code du développeur.

Dans des circonstances normales, lorsque le prérendu échoue, la création et le rendu du composant n’ont pas de sens, car un composant de travail ne peut pas être rendu.

Pour tolérer les erreurs qui peuvent se produire pendant le prérendu, la logique de gestion des erreurs doit être placée à l’intérieur d’un composant qui peut lever des exceptions. Utilisez les instructions [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) avec la gestion des erreurs et la journalisation. Au lieu d’encapsuler `RenderComponentAsync` l’appel `try-catch` à dans une instruction, placez la logique de gestion des `RenderComponentAsync`erreurs dans le composant rendu par.

## <a name="advanced-scenarios"></a>Scénarios avancés

### <a name="recursive-rendering"></a>Rendu récursif

Les composants peuvent être imbriqués de manière récursive. Cela est utile pour représenter des structures de données récursives. Par exemple, un `TreeNode` composant peut restituer `TreeNode` plus de composants pour chacun des enfants du nœud.

Lors du rendu de manière récursive, évitez les modèles de codage qui se traduisent par une récurrence infinie :

* Ne rendez pas de manière récursive une structure de données qui contient un cycle. Par exemple, n’affichez pas un nœud d’arbre dont les enfants s’y trouvent.
* Ne créez pas une chaîne de dispositions qui contiennent un cycle. Par exemple, ne créez pas une disposition dont la disposition est elle-même.
* N’autorisez pas un utilisateur final à enfreindre les invariants de récurrence (règles) par le biais d’une entrée de données malveillante ou d’appels Interop JavaScript.

Boucles infinies pendant le rendu :

* Fait en sorte que le processus de rendu continue de façon permanente.
* Équivaut à créer une boucle non terminée.

Dans ces scénarios, le circuit affecté se bloque et le thread tente généralement d’effectuer les opérations suivantes :

* Consommez le plus de temps processeur autorisé par le système d’exploitation, indéfiniment.
* Consommez une quantité illimitée de mémoire serveur. La consommation de mémoire illimitée est équivalente au scénario dans lequel une boucle non terminée ajoute des entrées à une collection à chaque itération.

Pour éviter les modèles de récurrence infinis, assurez-vous que le code de rendu récursif contient des conditions d’arrêt appropriées.

### <a name="custom-render-tree-logic"></a>Logique d’arborescence de rendu personnalisé

La plupart des composants éblouissants sont implémentés en tant que fichiers *. Razor* et sont compilés pour `RenderTreeBuilder` produire une logique qui opère sur un pour afficher leur sortie. Un développeur peut implémenter `RenderTreeBuilder` manuellement la logique C# à l’aide du code procédural. Pour plus d'informations, consultez <xref:blazor/components#manual-rendertreebuilder-logic>.

> [!WARNING]
> L’utilisation de la logique du générateur d’arborescence de rendu manuel est considérée comme un scénario avancé et risqué, non recommandé pour le développement de composants généraux.

Si `RenderTreeBuilder` le code est écrit, le développeur doit garantir l’exactitude du code. Par exemple, le développeur doit s’assurer que :

* Les appels `OpenElement` à `CloseElement` et sont correctement équilibrés.
* Les attributs sont ajoutés uniquement aux emplacements appropriés.

Une logique incorrecte du générateur d’arborescence de rendu manuel peut entraîner un comportement arbitraire non défini, y compris des pannes, des blocages du serveur et des failles de sécurité.

Envisagez une logique de générateur d’arborescence de rendu manuel sur le même niveau de complexité et avec le même niveau de *danger* que l’écriture de code assembleur ou d’instructions MSIL à la main.
