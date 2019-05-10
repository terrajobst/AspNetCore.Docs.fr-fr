---
title: Protection des données ASP.NET Core
author: rick-anderson
description: Découvrez le concept de protection des données et les principes de conception de l’API de Protection des données ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/data-protection/introduction
ms.openlocfilehash: 37f170a3e8a46ef2215b0999358d46dd402636df
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897986"
---
# <a name="aspnet-core-data-protection"></a>Protection des données ASP.NET Core

Les applications Web doivent souvent stocker les données de sécurité sensibles. Windows fournit DPAPI pour les applications de bureau, mais cela ne convient pas pour les applications web. La pile de protection des données ASP.NET Core fournissent une API de chiffrement simple et facile à utiliser un développeur peut utiliser pour protéger les données, y compris la rotation et gestion de clés.

La pile de protection des données ASP.NET Core est conçue pour servir le remplacement à long terme pour le &lt;machineKey&gt; élément dans ASP.NET 1.x - 4.x. Il a été conçu pour résoudre bon nombre des lacunes de l’ancienne pile de chiffrement tout en fournissant une solution complète pour la majorité des cas d’utilisation des applications modernes sont susceptibles de rencontrer.

## <a name="problem-statement"></a>Énoncé du problème

L’énoncé du problème global peut se résumer brièvement dans une phrase unique : J’ai besoin de conserver des informations fiables pour une récupération ultérieure, mais je n’approuve le mécanisme de persistance. Cela peut être écrit en termes de web, comme « J’ai besoin pour effectuer un aller-retour état approuvé via un client non fiable. »

L’exemple canonique de ceci est un cookie d’authentification ou au porteur jeton. Le serveur génère un « Je suis Groot et disposer des autorisations de xyz » du jeton et le remet au client. À une date ultérieure, le client présentera ce jeton sur le serveur, mais le serveur a besoin d’une sorte de garantie que le client n’a pas falsifié le jeton. Par conséquent, la première exigence : authenticité (également appelé) l’intégrité, falsification de la vérification).

Étant donné que l’état persistant est approuvé par le serveur, nous pensons que cet état peut contenir des informations spécifiques à l’environnement d’exploitation. Il peut s’agir de la forme d’un chemin d’accès de fichier, une autorisation, un handle ou autres référence indirecte ou certains autres éléments de données de serveur spécifiques. Ces informations doivent généralement pas être divulguées à un client non fiable. Par conséquent, la deuxième exigence : la confidentialité.

Enfin, étant donné que les applications modernes sont basées sur des composants que nous avons appris sont que les composants individuels souhaitez tirer parti de ce système sans tenir compte des autres composants dans le système. Par exemple, si un composant de jeton du porteur à l’aide de cette pile, il doit fonctionner sans interférence provenant d’un mécanisme d’anti-CSRF qui peut également être à l’aide de la même pile. Par conséquent, l’exigence finale : isolation.

Nous pouvons fournir des contraintes supplémentaires afin de limiter la portée de la configuration requise. Nous partons du principe que tous les services fonctionnant dans les systèmes de chiffrement sont fiables et que les données n’a pas besoin d’être généré ou consommé en dehors des services sous notre contrôle direct. En outre, nous avons besoin que les opérations sont plus vite possible étant donné que chaque demande au service web peut passer par le système de cryptage par une ou plusieurs fois. Cela rend le chiffrement symétrique idéale pour notre scénario, et nous pouvons discount cryptographie asymétrique jusqu'à par exemple une heure dont il a besoin.

## <a name="design-philosophy"></a>Philosophie de conception

Nous avons commencé en identifiant les problèmes liés à la pile existante. Une fois que nous avions qui, nous interrogées le paysage des solutions existantes et conclu qu’aucune solution existante n’était tout à fait les fonctionnalités que nous avons recherché. Ensuite, nous avons conçu une solution basée sur plusieurs principes généraux.

* Le système doit offrir simplicité de configuration. Dans l’idéal, le système serait sans aucune configuration et les développeurs Impossible d’avance. Les situations où les développeurs ont besoin pour configurer un aspect spécifique (par exemple, le dépôt de clé), doit être envisagé pour rendre ces configurations spécifiques simple.

* Offrent une simple API destinées aux consommateurs. L’API doivent être facile à utiliser correctement et difficile à utiliser de manière incorrecte.

* Les développeurs ne doivent pas Découvrez les principes de gestion de clés. Le système doit gérer la sélection de l’algorithme et la durée de vie des clés sur la place du développeur. Dans l’idéal, le développeur ne doit jamais même avoir accès pour le matériel de clé brutes.

* Les clés doivent être protégées au repos lorsque cela est possible. Le système doit déterminer un mécanisme de protection par défaut approprié et appliquer automatiquement.

Avec ces principes à l’esprit, nous avons développé une simple, [facile à utiliser](xref:security/data-protection/using-data-protection) pile de protection des données.

L’API de protection des données ASP.NET Core s’appliquent pas principalement pour la persistance indéfini de charges utiles confidentielles. Autres technologies telles que [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) et [Azure Rights Management](/rights-management/) sont plus adaptés pour le scénario de stockage indéterminée, et ils ont des fonctionnalités de gestion de clé forte en conséquence. Ceci dit, il n’y a rien interdire à un développeur à l’aide de l’API de protection des données ASP.NET Core pour la protection à long terme des données confidentielles.

## <a name="audience"></a>Public

Le système de protection des données est divisé en cinq packages principaux. Différents aspects de ces API ciblent les trois principaux types de publics ;

1. Le [vue d’ensemble des API de consommateur](xref:security/data-protection/consumer-apis/overview) cibler les développeurs d’application et d’infrastructure.

   « Je ne souhaite en savoir plus sur le fonctionnement de la pile ou sur la façon dont elle est configurée. J’ai simplement souhaitez effectuer une opération aussi simple d’une manière que possible avec une probabilité élevée de l’aide des API avec succès. »

2. Le [configuration API](xref:security/data-protection/configuration/overview) cibler les développeurs d’applications et les administrateurs système.

   « J’ai besoin indiquer le système de protection des données que mon environnement requiert des paramètres ou des chemins d’accès par défaut ».

3. Les développeurs d’extensibilité API cible, responsable de l’implémentation d’une stratégie personnalisée. L’utilisation de ces API est limitée à rares situations et expérimentée, les développeurs prenant en charge de sécurité.

   « Je dois remplacer un composant au sein du système dans son intégralité, car j’ai réellement uniques liées au comportement. Je souhaite en savoir plus rarement utilisé des parties de la surface d’API pour créer un plug-in qui répond à mes exigences. »

## <a name="package-layout"></a>Disposition du package

La pile de protection des données se compose de cinq packages.

* [Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/) contient le <xref:Microsoft.AspNetCore.DataProtection.IDataProtectionProvider> et <xref:Microsoft.AspNetCore.DataProtection.IDataProtector> interfaces pour créer des services de protection des données. Il contient également des méthodes d’extension utiles pour travailler avec ces types (par exemple, [IDataProtector.Protect](xref:Microsoft.AspNetCore.DataProtection.DataProtectionCommonExtensions.Protect*)). Si le système de protection des données est instancié ailleurs et que vous consommez l’API, référence `Microsoft.AspNetCore.DataProtection.Abstractions`.

* [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) contient l’implémentation de base du système de protection de données, y compris les opérations de chiffrement principales, la gestion de clés, la configuration et extensibilité. Pour instancier le système de protection des données (par exemple, l’ajout à un <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>) ou de la modification ou l’extension de son comportement, référencer `Microsoft.AspNetCore.DataProtection`.

* [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contient des API supplémentaires que les développeurs peut se révéler utiles, mais qui n’appartiennent pas dans le package principal. Par exemple, ce package contient des méthodes de fabrique pour instancier le système de protection des données pour stocker les clés à un emplacement sur le système de fichiers sans l’injection de dépendances (voir <xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>). Il contient également des méthodes d’extension pour limiter la durée de vie des charges utiles protégées (voir <xref:Microsoft.AspNetCore.DataProtection.ITimeLimitedDataProtector>).

* [Microsoft.AspNetCore.DataProtection.SystemWeb](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.SystemWeb/) peuvent être installés dans une application de 4.x ASP.NET existante pour rediriger ses `<machineKey>` opérations utilisent la nouvelle pile de protection de données ASP.NET Core. Pour plus d'informations, consultez <xref:security/data-protection/compatibility/replacing-machinekey>.

* [Microsoft.AspNetCore.Cryptography.KeyDerivation](https://www.nuget.org/packages/Microsoft.AspNetCore.Cryptography.KeyDerivation/) fournit une implémentation du mot de passe PBKDF2 routine de hachage et peut être utilisé par les systèmes qui doivent gérer les mots de passe utilisateur en toute sécurité. Pour plus d'informations, consultez <xref:security/data-protection/consumer-apis/password-hashing>.

## <a name="additional-resources"></a>Ressources supplémentaires

<xref:host-and-deploy/web-farm>
