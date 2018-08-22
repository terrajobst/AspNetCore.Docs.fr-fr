---
uid: web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
title: Profils, thèmes et composants WebPart | Microsoft Docs
author: microsoft
description: Voici les principales modifications de configuration et l’instrumentation dans ASP.NET 2.0. La nouvelle API de configuration ASP.NET permet des modifications de configuration qu’il veut pr...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 92df4051-77c6-492c-bd34-23d24189cea4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
msc.type: authoredcontent
ms.openlocfilehash: 98559c2a378c72bc5664faafe5436753050b574f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833223"
---
<a name="profiles-themes-and-web-parts"></a>Profils, thèmes et composants WebPart
====================
par [Microsoft](https://github.com/microsoft)

> Voici les principales modifications de configuration et l’instrumentation dans ASP.NET 2.0. La nouvelle API de configuration ASP.NET permet des modifications de configuration être apportée par programmation. En outre, existent de nombreux nouveaux paramètres de configuration permettant de nouvelles configurations et l’instrumentation.


ASP.NET 2.0 représente une amélioration importante dans la zone de sites Web personnalisés. Outre les fonctionnalités d’appartenance nous avons déjà abordé, les profils ASP.NET, les thèmes et les composants WebPart améliorent considérablement la personnalisation de sites Web.

## <a name="aspnet-profiles"></a>Profils ASP.NET

Les profils ASP.NET sont similaires aux sessions. La différence est qu’un profil est persistant, tandis qu’une session est perdure lorsque le navigateur est fermé. Une autre grande différence entre les profils et les sessions est que les profils sont fortement typées, par conséquent vous fournir IntelliSense pendant le processus de développement.

Un profil est défini dans le fichier de configuration de machines ou le fichier web.config de l’application. (Vous ne pouvez pas définir un profil dans un fichier web.config de sous-dossiers). Le code suivant définit un profil pour stocker les visiteurs du site Web de premier nom et prénom.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample1.xml)]

Le type de données par défaut pour une propriété de profil est System.String. Dans l’exemple ci-dessus, aucun type de données a été spécifié. Par conséquent, les propriétés FirstName et LastName sont tous deux de type chaîne. Comme mentionné précédemment, profil de propriétés sont fortement typées. Le code ci-dessous ajoute une nouvelle propriété pour la durée de vie est de type Int32.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample2.xml)]

Les profils sont généralement utilisées avec l’authentification par formulaire d’ASP.NET. Lorsqu’il est utilisé en association avec l’authentification par formulaire, chaque utilisateur dispose d’un profil distinct associé à son ID utilisateur. Toutefois, il est également possible de permettre l’utilisation de profils dans une application anonyme en utilisant le &lt;anonymousIdentification&gt; élément dans le fichier de configuration avec le **allowAnonymous** attribut en tant que Vous trouverez ci-dessous :

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample3.xml)]

Lorsqu’un utilisateur anonyme parcourt le site, ASP.NET crée une instance de **ProfileCommon** pour l’utilisateur. Ce profil utilise un ID unique, stocké dans un cookie sur le navigateur pour identifier l’utilisateur en tant qu’unique visiteur. De cette façon, vous pouvez stocker des informations de profil pour les utilisateurs qui parcourent de manière anonyme.

## <a name="profile-groups"></a>Groupes de profils

Il est possible de propriétés du groupe de profils. Par les propriétés de regroupement, il est possible de simuler plusieurs profils pour une application spécifique.

La configuration suivante configure une propriété FirstName et LastName pour deux groupes ; Les acheteurs et les Prospects.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample4.xml)]

Il est ensuite possible de définir des propriétés sur un groupe particulier comme suit :

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample5.cs)]

## <a name="storing-complex-objects"></a>Stockage d’objets complexes

Jusqu’ici, les exemples que nous avons couvert ont stocké des types de données simples dans un profil. Il est également possible de stocker des types de données complexes dans un profil en spécifiant la méthode de sérialisation à l’aide du **serializeAs** attribut comme suit :

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample6.xml)]

Dans ce cas, le type est PurchaseInvoice. La classe PurchaseInvoice doit être marqué comme sérialisable et peut contenir n’importe quel nombre de propriétés. Par exemple, si PurchaseInvoice possède une propriété appelée **NumItemsPurchased**, vous pouvez faire référence à cette propriété dans le code comme suit :

[!code-css[Main](profiles-themes-and-web-parts/samples/sample7.css)]

## <a name="profile-inheritance"></a>Héritage de profil

Il est possible de créer un profil à utiliser dans plusieurs applications. En créant une classe de profil qui dérive de ProfileBase, vous pouvez réutiliser un profil dans plusieurs applications à l’aide de la **hérite** attribut comme indiqué ci-dessous :

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample8.xml)]

Dans ce cas, la classe **PurchasingProfile** se présenterait comme suit :

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample9.cs)]

## <a name="profile-providers"></a>Fournisseurs de profils

Les profils d’ASP.NET utilisent le modèle de fournisseur. Le fournisseur par défaut stocke les informations dans une base de données SQL Server Express dans l’application\_dossier de données de l’application Web à l’aide du fournisseur SqlProfileProvider. Si la base de données n’existe pas, ASP.NET crée automatiquement lorsque le profil tente de stocker des informations.

Dans certains cas, toutefois, vous souhaiterez développer votre propre fournisseur de profils. La fonctionnalité de profil ASP.NET vous permet d’utiliser facilement les différents fournisseurs.

Vous créez un fournisseur de profils personnalisé lorsque :

- Vous avez besoin stocker les informations de profil dans une source de données, telles que dans une base de données FoxPro ou dans une base de données Oracle, qui ne prend pas en charge les fournisseurs de profils inclus avec le .NET Framework.
- Vous avez besoin gérer les informations de profil à l’aide d’un schéma de base de données qui est différent du schéma de base de données utilisé par les fournisseurs inclus avec le .NET Framework. Un exemple courant est que vous souhaitez intégrer des informations de profil avec des données utilisateur dans une base de données SQL Server existante.

### <a name="required-classes"></a>Classes requises

Pour implémenter un fournisseur de profils, vous créez une classe qui hérite de la classe abstraite System.Web.Profile.ProfileProvider. Le **ProfileProvider** classe abstraite à son tour hérite de la classe abstraite System.Configuration.SettingsProvider, qui hérite de la classe abstraite System.Configuration.Provider.ProviderBase. En raison de cette chaîne d’héritage, outre les membres requis de la **ProfileProvider** (classe), vous devez implémenter les membres requis de la **SettingsProvider** et  **ProviderBase** classes.

Les tableaux suivants décrivent les propriétés et méthodes que vous devez implémenter à partir de la **ProviderBase**, **SettingsProvider**, et **ProfileProvider** abstraite classes.

### <a name="providerbase-members"></a>Membres ProviderBase

| **Membre** | **Description** |
| --- | --- |
| Initialize (méthode) | Prend comme entrée le nom de l’instance du fournisseur et une collection NameValueCollection des paramètres de configuration. Utilisé pour définir les options et valeurs de propriété pour l’instance de fournisseur, y compris les valeurs spécifiques à l’implémentation et les options spécifiées dans la configuration de l’ordinateur ou le fichier Web.config. |

### <a name="settingsprovider-members"></a>Membres SettingsProvider

| **Membre** | **Description** |
| --- | --- |
| Propriété ApplicationName | Le nom de l’application qui est stocké avec chaque profil. Le fournisseur de profils utilise le nom de l’application pour stocker les informations de profil séparément pour chaque application. Cela permet à plusieurs applications ASP.NET d’utiliser la même source de données sans un conflit si le même nom d’utilisateur est créé dans différentes applications. Vous pouvez également plusieurs applications ASP.NET peuvent partager une source de données de profil en spécifiant le même nom d’application. |
| GetPropertyValues (méthode) | Prend comme entrée un SettingsContext et un objet SettingsPropertyCollection. Le **SettingsContext** fournit des informations sur l’utilisateur. Vous pouvez utiliser les informations comme clé primaire pour récupérer des informations de propriété de profil pour l’utilisateur. Utilisez le **SettingsContext** objet pour lequel obtenir le nom d’utilisateur et si l’utilisateur est authentifié ou anonyme. Le **SettingsPropertyCollection** contient une collection d’objets de SettingsProperty. Chaque **SettingsProperty** objet fournit le nom et le type de la propriété, ainsi que des informations supplémentaires telles que la valeur par défaut pour la propriété et indique si la propriété est en lecture seule. Le **GetPropertyValues** méthode remplit une collection SettingsPropertyValueCollection avec les objets SettingsPropertyValue selon le **SettingsProperty** objets fournis en tant qu’entrée. Les valeurs à partir de la source de données pour l’utilisateur spécifié sont assignées aux propriétés PropertyValue pour chaque **SettingsPropertyValue** objet et la collection entière est retournée. Appel de la méthode met également à jour la valeur de LastActivityDate pour le profil d’utilisateur spécifié à la date et heure actuelles. |
| SetPropertyValues (méthode) | Prend comme entrée un **SettingsContext** et un **collection SettingsPropertyValueCollection** objet. Le **SettingsContext** fournit des informations sur l’utilisateur. Vous pouvez utiliser les informations comme clé primaire pour récupérer des informations de propriété de profil pour l’utilisateur. Utilisez le **SettingsContext** objet pour lequel obtenir le nom d’utilisateur et si l’utilisateur est authentifié ou anonyme. Le **collection SettingsPropertyValueCollection** contient une collection de **SettingsPropertyValue** objets. Chaque **SettingsPropertyValue** objet fournit le nom, le type et la valeur de la propriété, ainsi que des informations supplémentaires telles que la valeur par défaut pour la propriété et indique si la propriété est en lecture seule. Le **SetPropertyValues** méthode met à jour les valeurs de propriété de profil dans la source de données pour l’utilisateur spécifié. Appeler la méthode met également le **LastActivityDate** et LastUpdatedDate des valeurs pour le profil de l’utilisateur spécifié à la date et heure actuelles. |

### <a name="profileprovider-members"></a>Membres ProfileProvider

| **Membre** | **Description** |
| --- | --- |
| DeleteProfiles (méthode) | Prend comme entrée un tableau de chaînes de l’utilisateur, noms et supprime de la source de données toutes les valeurs d’informations et de la propriété de profil pour les noms spécifiés dont le nom d’application correspond à la **ApplicationName** valeur de propriété. Si votre source de données prend en charge les transactions, il est recommandé d’inclure toutes les opérations de suppression dans une transaction et de restaurer la transaction et de lever une exception si une opération de suppression échoue. |
| DeleteProfiles (méthode) | Prend comme entrée une collection de ProfileInfo objets et supprime de la source de données toutes les valeurs d’informations et de la propriété de profil pour chaque profil dont le nom d’application correspond à la **ApplicationName** valeur de propriété. Si votre source de données prend en charge les transactions, il est recommandé d’inclure toutes les opérations de suppression dans une transaction et de restaurer la transaction et de lever une exception si une opération de suppression échoue. |
| DeleteInactiveProfiles (méthode) | Prend comme entrée une valeur ProfileAuthenticationOption et un objet DateTime et des suppressions à partir des données de source de toutes les informations de profil et les valeurs de propriété où la dernière date d’activité est inférieure ou égale à la date et heure spécifiées et le nom de l’application correspond à la **ApplicationName** valeur de propriété. Le **ProfileAuthenticationOption** paramètre spécifie si seuls les profils anonymes, les profils authentifiés, ou tous les profils doivent être supprimées. Si votre source de données prend en charge les transactions, il est recommandé d’inclure toutes les opérations de suppression dans une transaction et de restaurer la transaction et de lever une exception si une opération de suppression échoue. |
| GetAllProfiles (méthode) | Prend comme entrée un **ProfileAuthenticationOption** valeur, un entier qui spécifie l’index de page, un entier qui spécifie la taille de page et une référence à un entier dont la valeur est le nombre total de profils. Retourne une ProfileInfoCollection contienne **ProfileInfo** objets pour tous les profils dans la source de données où le nom d’application correspond à la **ApplicationName** valeur de propriété. Le **ProfileAuthenticationOption** paramètre spécifie si seuls les profils anonymes, les profils authentifiés, ou tous les profils doivent être retournés. Les résultats retournés par la **GetAllProfiles** méthode sont contraints par les index de page et les valeurs de taille de page. La valeur de taille de page spécifie le nombre maximal de **ProfileInfo** objets à retourner dans le **ProfileInfoCollection**. La valeur d’index de page spécifie la page de résultats à retourner, 1 correspondant à la première page. Le paramètre de nombre total d’enregistrements est un paramètre de sortie (vous pouvez utiliser **ByRef** en Visual Basic) qui est défini sur le nombre total de profils. Par exemple, si le magasin de données contient 13 profils pour l’application et la valeur d’index de page est de 2 avec une taille de page de 5, le **ProfileInfoCollection** retournée contient les sixième à dixième profils. La valeur du nombre total d’enregistrements est définie sur 13 lorsque la méthode est retournée. |
| GetAllInactiveProfiles (méthode) | Prend comme entrée un **ProfileAuthenticationOption** valeur, un **DateTime** objet, un entier qui spécifie l’index de page, un entier qui spécifie la taille de page et une référence à un entier dont la valeur le nombre total de profils. Retourne un **ProfileInfoCollection** contenant **ProfileInfo** objets pour tous les profils dans la source de données où la dernière date d’activité est inférieure ou égale à spécifié **DateTime**  et où le nom d’application correspond à la **ApplicationName** valeur de propriété. Le **ProfileAuthenticationOption** paramètre spécifie si seuls les profils anonymes, les profils authentifiés, ou tous les profils doivent être retournés. Les résultats retournés par la **GetAllInactiveProfiles** méthode sont contraints par les index de page et les valeurs de taille de page. La valeur de taille de page spécifie le nombre maximal de **ProfileInfo** objets à retourner dans le **ProfileInfoCollection**. La valeur d’index de page spécifie la page de résultats à retourner, 1 correspondant à la première page. Le paramètre de nombre total d’enregistrements est un paramètre de sortie (vous pouvez utiliser **ByRef** en Visual Basic) qui est défini sur le nombre total de profils. Par exemple, si le magasin de données contient 13 profils pour l’application et la valeur d’index de page est de 2 avec une taille de page de 5, le **ProfileInfoCollection** retournée contient les sixième à dixième profils. La valeur du nombre total d’enregistrements est définie sur 13 lorsque la méthode est retournée. |
| FindProfilesByUserName (méthode) | Prend comme entrée un **ProfileAuthenticationOption** valeur, une chaîne contenant un nom d’utilisateur, un entier qui spécifie l’index de page, un entier qui spécifie la taille de page et une référence à un entier dont la valeur est le nombre total de profils. Retourne un **ProfileInfoCollection** contenant **ProfileInfo** objets pour tous les profils dans les données source où le nom d’utilisateur correspond au nom d’utilisateur spécifié et où le nom d’application correspond à la **ApplicationName** valeur de propriété. Le **ProfileAuthenticationOption** paramètre spécifie si seuls les profils anonymes, les profils authentifiés, ou tous les profils doivent être retournés. Si votre source de données prend en charge les fonctions de recherche supplémentaires, telles que des caractères génériques, vous pouvez fournir des fonctions de recherche plus étendues pour les noms d’utilisateur. Les résultats retournés par la **FindProfilesByUserName** méthode sont contraints par les index de page et les valeurs de taille de page. La valeur de taille de page spécifie le nombre maximal de **ProfileInfo** objets à retourner dans le **ProfileInfoCollection**. La valeur d’index de page spécifie la page de résultats à retourner, 1 correspondant à la première page. Le paramètre de nombre total d’enregistrements est un paramètre de sortie (vous pouvez utiliser **ByRef** en Visual Basic) qui est défini sur le nombre total de profils. Par exemple, si le magasin de données contient 13 profils pour l’application et la valeur d’index de page est de 2 avec une taille de page de 5, le **ProfileInfoCollection** retournée contient les sixième à dixième profils. La valeur du nombre total d’enregistrements est définie sur 13 lorsque la méthode est retournée. |
| FindInactiveProfilesByUserName (méthode) | Prend comme entrée un **ProfileAuthenticationOption** valeur, une chaîne contenant un nom d’utilisateur, un **DateTime** objet, un entier qui spécifie l’index de page, un entier qui spécifie la taille de page et un référence à un entier dont la valeur est le nombre total de profils. Retourne un **ProfileInfoCollection** contenant **ProfileInfo** objets pour tous les profils dans la source de données où le nom d’utilisateur correspond au nom d’utilisateur spécifié, où la dernière date d’activité est inférieur à ou égal à spécifié **DateTime**, et où le nom d’application correspond à la **ApplicationName** valeur de propriété. Le **ProfileAuthenticationOption** paramètre spécifie si seuls les profils anonymes, les profils authentifiés, ou tous les profils doivent être retournés. Si votre source de données prend en charge les fonctions de recherche supplémentaires, telles que des caractères génériques, vous pouvez fournir des fonctions de recherche plus étendues pour les noms d’utilisateur. Les résultats retournés par la **FindInactiveProfilesByUserName** méthode sont contraints par les index de page et les valeurs de taille de page. La valeur de taille de page spécifie le nombre maximal de **ProfileInfo** objets à retourner dans le **ProfileInfoCollection**. La valeur d’index de page spécifie la page de résultats à retourner, 1 correspondant à la première page. Le paramètre de nombre total d’enregistrements est un paramètre de sortie (vous pouvez utiliser **ByRef** en Visual Basic) qui est défini sur le nombre total de profils. Par exemple, si le magasin de données contient 13 profils pour l’application et la valeur d’index de page est de 2 avec une taille de page de 5, le **ProfileInfoCollection** retournée contient les sixième à dixième profils. La valeur du nombre total d’enregistrements est définie sur 13 lorsque la méthode est retournée. |
| GetNumberOfInActiveProfiles (méthode) | Prend comme entrée un **ProfileAuthenticationOption** valeur et un **DateTime** de l’objet et retourne le nombre de tous les profils dans la source de données où la dernière date d’activité est inférieure ou égale à la spécifié **Date/heure** et où le nom d’application correspond à la **ApplicationName** valeur de propriété. Le **ProfileAuthenticationOption** paramètre spécifie si seuls les profils anonymes, les profils authentifiés, ou tous les profils doivent être comptabilisés. |

### <a name="applicationname"></a>ApplicationName

Étant donné que les fournisseurs de profils stockent des informations de profil séparément pour chaque application, vous devez vous assurer que votre schéma de données inclut le nom de l’application et que les requêtes et les mises à jour, incluent le nom de l’application. Par exemple, la commande suivante permet de récupérer une valeur de propriété à partir d’une base de données basée sur le nom d’utilisateur et si le profil est anonyme et garantit que le **ApplicationName** valeur est incluse dans la requête.

[!code-sql[Main](profiles-themes-and-web-parts/samples/sample10.sql)]

## <a name="aspnet-themes"></a>Thèmes ASP.NET

## <a name="what-are-aspnet-20-themes"></a>Quelles sont les thèmes ASP.NET 2.0 ?

Un des aspects plus importants d’une application Web est une apparence cohérente sur le site. Les développeurs ASP.NET 1.x utilisent généralement des feuilles de Style en cascade (CSS) pour implémenter une apparence cohérente. ASP.NET 2.0 thèmes améliorent nettement CSS, car ils fournissent le développeur ASP.NET permet de définir l’apparence des contrôles serveur ASP.NET, ainsi que des éléments HTML. Thèmes ASP.NET peuvent être appliqués à des contrôles individuels, une page Web spécifique ou une application Web entière. Thèmes utilisent une combinaison de fichiers CSS, un fichier d’apparence facultatif et un répertoire Images facultatif si les images sont nécessaires. Le fichier d’apparence de contrôle l’apparence visuelle des contrôles serveur ASP.NET.

## <a name="where-are-themes-stored"></a>Où sont stockées des thèmes ?

L’emplacement où sont stockés les thèmes varie en fonction de leur portée. Les thèmes qui peuvent être appliqués à n’importe quelle application sont stockés dans le dossier suivant :

`C:\WINDOWS\Microsoft.NET\Framework\v2.x.xxxxx\ASP.NETClientFiles\Themes\<Theme_Name>`

Un thème qui est spécifique à une application particulière est stocké dans un `App\_Themes\<Theme\_Name>` répertoire à la racine du site Web.

> [!NOTE]
> Un fichier d’apparence devez uniquement modifier les propriétés du contrôle serveur qui affectent l’apparence.

Un thème global est un thème qui peut être appliqué à toute application ou un site Web en cours d’exécution sur le serveur Web. Ces thèmes sont stockés par défaut dans le répertoire ASP.NETClientfiles\Themes qui se trouve dans le répertoire v2.x.xxxxx. Vous pouvez également déplacer les fichiers de thème dans le compte aspnet\_/système client\_web / [version] /Themes/ [thème\_nom] dossier dans la racine de votre site Web.

Les thèmes spécifiques à l’application peuvent uniquement être appliqués à l’application dans lequel résident les fichiers. Ces fichiers sont stockés dans le `App\_Themes/<theme\_name>` répertoire à la racine du site Web.

## <a name="the-components-of-a-theme"></a>Les composants d’un thème

Un thème est constitué d’un ou plusieurs fichiers CSS, un fichier d’apparence facultatif et un dossier Images facultatif. Les fichiers CSS peuvent être n’importe quel nom vous le souhaitez (default.css ou Theme.CSS, etc.) et devez se trouver dans la racine du dossier de thèmes. Les fichiers CSS sont utilisés pour définir des classes CSS ordinaires et les attributs de sélecteurs spécifiques. Pour appliquer une des classes CSS à un élément de page, le **CSSClass** propriété est utilisée.

Le fichier d’apparence est un fichier XML qui contient les définitions de propriétés pour les contrôles serveur ASP.NET. Le code ci-dessous est un exemple de fichier d’apparence.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample11.aspx)]

**Figure 1** ci-dessous montre une page ASP.NET petite consultés sans appliquer un thème. **Figure 2** montre le même fichier avec un thème est appliqué. La couleur d’arrière-plan et la couleur de texte sont configurés via un fichier CSS. L’apparence du bouton et la zone de texte sont configurés à l’aide du fichier d’apparence répertorié ci-dessus.


![Aucun thème](profiles-themes-and-web-parts/_static/image1.gif)

**Figure 1**: aucun thème


![Thème appliqué](profiles-themes-and-web-parts/_static/image2.gif)

**Figure 2**: thème appliqué


Le fichier d’apparence répertorié ci-dessus définit une apparence par défaut pour tous les contrôles de zone de texte et les contrôles de bouton. Cela signifie que chaque contrôle de zone de texte et les insérer sur une page de contrôle de bouton aura cette apparence. Vous pouvez également définir une apparence qui peut être appliquée à des instances spécifiques de ces contrôles à l’aide de la **SkinID** propriété du contrôle.

Le code suivant définit une apparence d’un contrôle bouton. Contrôle de bouton uniquement avec un **SkinID** propriété du **goButton** prendra l’apparence de l’apparence.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample12.aspx)]

Vous ne pouvez avoir une apparence par défaut par le type de contrôle serveur. Si vous avez besoin des apparences supplémentaires, vous devez utiliser la propriété SkinID.

## <a name="applying-themes-to-pages"></a>Appliquer des thèmes à des Pages

Un thème peut être appliqué à l’aide de l’une des méthodes suivantes :

- Dans le &lt;pages&gt; élément du fichier web.config
- Dans la @Page directive d’une page
- Par programmation

## <a name="applying-a-theme-in-the-configuration-file"></a>Appliquer un thème dans le fichier de Configuration

Pour appliquer un thème dans le fichier de configuration d’applications, utilisez la syntaxe suivante :

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample13.xml)]

Le nom du thème spécifié ici doit correspondre au nom du dossier de thèmes. Ce dossier peut exister dans l’un des emplacements indiqués plus haut dans ce cours. Si vous essayez d’appliquer un thème qui n’existe pas, une erreur de configuration se produit.

## <a name="applying-a-theme-in-the-page-directive"></a>Appliquer un thème dans la Directive de Page

Vous pouvez également appliquer un thème dans la directive @ Page. Cette méthode vous permet d’utiliser un thème pour une page spécifique.

Pour appliquer un thème dans le @Page directive, utilisez la syntaxe suivante :

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample14.aspx)]

Une fois encore, le thème spécifié ici doit correspondre au dossier thème comme mentionné précédemment. Si vous essayez d’appliquer un thème qui n’existe pas, un échec de génération se produit. Visual Studio sera également mettre en surbrillance de l’attribut et vous avertir qu’Aucun thème n’existe.

## <a name="applying-a-theme-programmatically"></a>Appliquer un thème par programmation

Pour appliquer un thème par programmation, vous devez spécifier le **thème** propriété pour la page dans le **Page\_PreInit** (méthode).

Pour appliquer un thème par programmation, utilisez la syntaxe suivante :

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample15.cs)]

Il est nécessaire appliquer le thème dans la méthode PreInit en raison du cycle de vie de page. Si vous l’appliquez à ce stade, le thème de pages est ont déjà été appliqué par le runtime et une modification à ce stade est trop tard dans le cycle de vie. Si vous appliquez un thème qui n’existe pas, un **HttpException** se produit. Un thème est appliqué par programmation, un avertissement de génération se produit si les contrôles serveur ont une propriété SkinID spécifiée. Cet avertissement est destiné à vous informer qu’aucun thème n’est appliquée de façon déclarative et il peut être ignoré.

## <a name="exercise-1--applying-a-theme"></a>Exercice 1 : Application d’un thème

Dans cet exercice, vous allez appliquer un thème ASP.NET à un site Web.

> [!IMPORTANT]
> Si vous utilisez Microsoft Word pour entrer des informations dans un fichier d’apparence, assurez-vous que vous ne remplacez pas les guillemets par des guillemets. Guillemets courbes provoque des problèmes avec les fichiers d’apparence.

1. Créez un site web ASP.NET.
2. Avec le bouton droit sur le projet dans l’Explorateur de solutions et choisissez Ajouter un nouvel élément.
3. Choisissez le fichier de Configuration Web à partir de la liste des fichiers et cliquez sur Ajouter.
4. Avec le bouton droit sur le projet dans l’Explorateur de solutions et choisissez Ajouter un nouvel élément.
5. Choisissez le fichier d’apparence et cliquez sur Ajouter.
6. Cliquez sur Oui lorsque vous êtes invité si vous souhaitez placer le fichier à l’intérieur de l’application\_dossier de thèmes.
7. Avec le bouton droit sur le dossier SkinFile à l’intérieur de l’application\_dossier thèmes dans l’Explorateur de solutions et choisissez Ajouter un nouvel élément.
8. Choisissez la feuille de Style à partir de la liste des fichiers et cliquez sur Ajouter. Vous avez maintenant tous les fichiers nécessaires à l’implémentation de votre nouveau thème. Toutefois, Visual Studio a nommé votre dossier de thèmes SkinFile. Avec le bouton droit sur ce dossier et remplacez le nom CoolTheme.
9. Ouvrez le fichier SkinFile.skin et ajoutez le code suivant la fin du fichier : 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample16.aspx)]
10. Enregistrez le fichier SkinFile.skin.
11. Ouvrez le StyleSheet.css.
12. Remplacez tout le texte qu’il contient les éléments suivants : 

    [!code-css[Main](profiles-themes-and-web-parts/samples/sample17.css)]
13. Enregistrez le fichier StyleSheet.css.
14. Ouvrez la page Default.aspx.
15. Ajoutez un contrôle de zone de texte et un contrôle bouton.
16. Enregistrez la page. Accédez maintenant la page Default.aspx. Il doit afficher en tant qu’un formulaire Web normal.
17. Ouvrez le fichier web.config.
18. Ajoutez le code suivant directement en dessous de l’ouverture `<system.web>` balise : 

    [!code-xml[Main](profiles-themes-and-web-parts/samples/sample18.xml)]
19. Enregistrez le fichier web.config. Accédez maintenant la page Default.aspx. Il doit afficher avec le thème appliqué.
20. Si elle n’est pas déjà ouverte, ouvrez la page Default.aspx dans Visual Studio.
21. Sélectionnez le bouton.
22. Modifier le **SkinID** propriété goButton. Notez que Visual Studio fournit une liste déroulante avec des valeurs SkinID valides pour un contrôle bouton.
23. Enregistrez la page. Maintenant afficher un aperçu de la page dans votre navigateur à nouveau. Le bouton doit à présent indiquer « go » et doit être plus large d’aspect.

À l’aide de la **SkinID** propriété, vous pouvez facilement configurer différentes apparences pour différentes instances d’un type particulier de contrôle serveur.

## <a name="the-stylesheettheme-property"></a>La propriété StyleSheetTheme

Jusqu’ici, nous avons parlé uniquement appliquer des thèmes à l’aide de la propriété de thème. Lorsque vous utilisez la propriété de thème, le fichier d’apparence remplacera tous les paramètres pour les contrôles serveur déclaratifs. Par exemple, dans l’exercice 1, vous avez spécifié un SkinID de « goButton » pour le contrôle de bouton et qui a été modifiée le texte du bouton pour « go ». Vous avez peut-être remarqué que la propriété de texte du bouton dans le concepteur a été définie sur « Bouton », mais le thème a remplacé qui. Le thème remplacent toujours les paramètres de propriété dans le concepteur.

Si vous souhaitez être en mesure de substituer les propriétés définies dans le fichier d’apparence du thème avec les propriétés spécifiées dans le concepteur, vous pouvez utiliser la **StyleSheetTheme** propriété au lieu de la propriété de thème. La propriété StyleSheetTheme est identique à la propriété de thème, à ceci près qu’il ne substitue pas tous les paramètres de propriété explicite comme le fait de la propriété de thème.

Pour voir cela en action, ouvrez le fichier web.config du projet dans l’exercice 1 et modifier le `<pages>` élément à ce qui suit :

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample19.xml)]

Désormais parcourir la page Default.aspx et vous verrez que le contrôle de bouton a une propriété Text de « Bouton » à nouveau. C’est parce que le paramètre de propriété explicite dans le concepteur substitue la propriété de texte définie par le goButton SkinID.

## <a name="overriding-themes"></a>Substitution de thèmes

Un thème global peut être substitué en appliquant un thème portant le même nom dans l’application\_dossier thèmes de l’application. Toutefois, le thème n’est pas appliqué dans un scénario de remplacement true. Si l’exécution rencontre les fichiers de thème dans l’application\_dossier de thèmes, il applique le thème à l’aide de ces fichiers et ignore le thème global.

La propriété StyleSheetTheme est substituable et peut être remplacée dans le code comme suit :

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample20.cs)]

## <a name="web-parts"></a>WebParts

Composants WebPart ASP.NET est un ensemble de contrôles de création de sites Web qui permettent aux utilisateurs finaux de modifier le contenu, l’apparence et comportement de pages Web directement à partir d’un navigateur. Les modifications peuvent être appliquées à tous les utilisateurs sur le site ou à des utilisateurs individuels. Lorsque les utilisateurs modifient des pages et des contrôles, les paramètres peuvent être enregistrés pour conserver les préférences personnelles d’un utilisateur via les futures sessions de navigateur, une fonctionnalité appelée personnalisation. Ces fonctionnalités WebPart signifient que les développeurs peuvent autoriser les utilisateurs finaux à personnaliser une application Web de manière dynamique sans intervention de développeur ou administrateur.

Le jeu de composants WebPart, en tant que développeur permettent aux utilisateurs finaux à :

- Personnaliser le contenu de la page. Les utilisateurs peuvent ajouter de nouveaux contrôles WebPart à une page, les supprimer, les masquer ou les réduire comme ordinaire de windows.
- Personnaliser la mise en page. Les utilisateurs peuvent faire glisser un contrôle WebPart à une zone différente sur une page, ou modifier ses propriétés, l’apparence et le comportement.
- Exporter et importer des contrôles. Les utilisateurs peuvent importer ou exporter les paramètres de contrôle WebPart pour une utilisation dans d’autres pages ou sites, en conservant les propriétés, l’apparence et même les données dans les contrôles. Cela réduit les demandes de données entrée et de configuration sur les utilisateurs finaux.
- Créer des connexions. Les utilisateurs peuvent établir des connexions entre les contrôles afin que, par exemple, un contrôle de graphique peut afficher un graphique pour les données dans un contrôle de cotations boursières. Les utilisateurs pourraient personnaliser non seulement la connexion elle-même, mais l’apparence et les détails de la façon dont le contrôle chart affiche les données.
- Gérer et personnaliser les paramètres au niveau du site. Les utilisateurs autorisés peuvent configurer les paramètres au niveau du site, déterminer qui peut accéder à un site ou une page, définir l’accès en fonction du rôle aux contrôles et ainsi de suite. Par exemple, un utilisateur dans un rôle d’administrateur peut définir un contrôle WebPart devant être partagé par tous les utilisateurs et empêcher les utilisateurs qui ne sont pas des administrateurs de personnaliser le contrôle partagé.

Vous allez généralement travailler avec composants WebPart dans un des trois façons : création de pages qui utilisent des contrôles WebPart, la création de contrôles WebPart individuels ou la création d’applications Web complètes, personnalisables, tels que d’un portail.

## <a name="page-development"></a>Développement de pages

Les développeurs de page peuvent utiliser les outils de conception visuelle tel que Microsoft Visual Studio 2005 pour créer des pages qui utilisent des composants WebPart. L’un des avantages à l’aide d’un outil tel que Visual Studio est que le jeu de contrôles WebPart fournit des fonctionnalités de création de glisser-déplacer et la configuration de contrôles WebPart dans un concepteur visuel. Par exemple, vous pouvez utiliser le concepteur pour faire glisser une zone WebPart ou un contrôle d’édition Web Parts, l’aire de conception, puis configurer le contrôle vers la droite dans le concepteur à l’aide de l’interface utilisateur fournie par les composants WebPart jeu de contrôles. Vous pouvez accélérer le développement des applications WebPart et réduire la quantité de code à écrire.

## <a name="control-development"></a>Développement de contrôles

Vous pouvez utiliser n’importe quel contrôle ASP.NET existant comme un contrôle WebPart, y compris les contrôles serveur Web standard, les contrôles serveur personnalisés et les contrôles utilisateur. Pour optimiser le contrôle par programmation de votre environnement, vous pouvez également créer des contrôles WebPart personnalisés qui dérivent de la classe WebPart. Développement de contrôles WebPart individuel, sont en général, soit créer un contrôle utilisateur et utilisez-le comme un contrôle WebPart ou développer un contrôle WebPart personnalisé.

Par exemple du développement d’un contrôle WebPart personnalisé, vous pouvez créer un contrôle pour fournir les fonctionnalités fournies par d’autres contrôles serveur ASP.NET qui peuvent être utiles de package sous la forme d’un contrôle WebPart personnalisable : calendriers, des listes, des informations financières, Actualités, calculatrices, contrôles de texte enrichi pour la mise à jour des grilles de contenu, modifiables qui se connectent aux bases de données, les graphiques qui dynamiquement leurs écrans, de mettre à jour ou la météo et informations de voyage. Si vous fournissez un concepteur visuel avec votre contrôle, puis n’importe quel développeur de page à l’aide de Visual Studio peut simplement faire glisser votre contrôle dans une zone WebPart et configurer au moment du design sans devoir écrire du code supplémentaire.

La personnalisation est le fondement de la fonctionnalité de composants WebPart. Il permet aux utilisateurs de modifier ou de personnaliser--la disposition, l’apparence et comportement des contrôles WebPart sur une page. Les paramètres personnalisés sont durables : ils persistent pas seulement pendant la session de navigateur actuelle (comme avec l’état d’affichage), mais également dans le stockage à long terme, afin que les paramètres d’un utilisateur pour les futures sessions de navigateur ainsi. Personnalisation est activée par défaut pour les pages Web Parts.

Les composants structurels d’interface utilisateur s’appuient sur la personnalisation et fournissent la structure de base et les services nécessaires à tous les contrôles WebPart. Un composant structurels de l’interface utilisateur requis sur chaque page WebPart est le contrôle WebPartManager. Bien que jamais visible, ce contrôle a la tâche critique de coordonner tous les contrôles WebPart sur une page. Par exemple, il effectue le suivi de tous les contrôles WebPart individuels. Il gère les zones WebPart (régions qui contiennent des contrôles WebPart sur une page), et qui sont des contrôles dans les zones. Il effectue le suivi et contrôle les différents modes d’affichage qu'une page peut être dans, Parcourir, de se connecter, de modifier ou de catalogue, et si les modifications de personnalisation s’appliquent à tous les utilisateurs ou à des utilisateurs individuels. Enfin, il lance et effectue le suivi des connexions et la communication entre des contrôles WebPart.

Le deuxième type de composant structurel d’interface utilisateur est la zone. Les zones agissent en tant que gestionnaires de présentation sur une page WebPart. Ils contiennent et organisent les contrôles qui dérivent de la classe du composant (contrôles part) et fournissent la capacité mise en page modulaire en orientation horizontale ou verticale. Les zones offrent également des éléments d’interface utilisateur communes et cohérentes (par exemple, le style d’en-tête et pied de page, titre, style de bordure, boutons d’action et ainsi de suite) pour chaque contrôle qu’ils contiennent ; ces éléments communs sont connus comme le chrome d’un contrôle. Plusieurs types spécialisés de zones sont utilisées dans les différents modes d’affichage et avec différents contrôles. Les différents types de zones sont décrites dans la section contrôles WebPart essentiels ci-dessous.

Les contrôles d’interface utilisateur WebPart, qui dérivent de la **partie** class, composent l’interface utilisateur principale sur une page WebPart. Le jeu de composants WebPart est flexible et inclus dans les options proposées pour la création de contrôles WebPart. Outre la création de vos propres contrôles WebPart personnalisés, vous pouvez également utiliser des contrôles serveur ASP.NET existants, les contrôles utilisateur ou les contrôles serveur personnalisés en tant que contrôles WebPart. Les contrôles essentiels qui sont couramment utilisés pour la création de pages Web Parts sont décrits dans la section suivante.

## <a name="web-parts-essential-controls"></a>Contrôles WebPart essentiels

Le jeu de composants WebPart est complet, mais certains contrôles sont essentiels, car ils sont requis pour les composants WebPart travailler, ou car ils sont les contrôles les plus fréquemment utilisés sur les pages WebPart. Comme vous commencez à l’aide de Web Parts et création de pages de base WebPart, il est utile de se familiariser avec les contrôles WebPart essentiels décrits dans le tableau suivant.

| **Contrôle WebPart** | **Description** |
| --- | --- |
| WebPartManager | Gère tous les contrôles WebPart sur une page. Un seul (et unique) **WebPartManager** contrôle est requis pour chaque page WebPart. |
| CatalogZone | Contient des contrôles CatalogPart. Utilisez cette zone pour créer un catalogue de contrôles WebPart à partir de laquelle les utilisateurs peuvent sélectionner des contrôles à ajouter à une page. |
| EditorZone | Contient des contrôles EditorPart. Utilisez cette zone pour permettre aux utilisateurs de modifier et personnaliser des contrôles WebPart sur une page. |
| WebPartZone | Contient et fournit la disposition générale pour les contrôles WebPart qui composent l’interface utilisateur principale d’une page. Utilisez cette zone chaque fois que vous créez des pages avec des contrôles WebPart. Les pages peuvent contenir une ou plusieurs zones. |
| ConnectionsZone | Contient des contrôles WebPartConnection et fournit une interface utilisateur pour la gestion des connexions. |
| Composant WebPart (GenericWebPart) | Restitue l’interface utilisateur principale ; la plupart des contrôles d’interface utilisateur WebPart appartiennent à cette catégorie. Pour optimiser le contrôle par programmation, vous pouvez créer des contrôles WebPart personnalisés qui dérivent de la base de **WebPart** contrôle. Vous pouvez également utiliser des contrôles serveur existants, les contrôles utilisateur ou les contrôles personnalisés en tant que contrôles WebPart. Chaque fois qu’un de ces contrôles sont placé dans une zone, le **WebPartManager** contrôle les encapsule automatiquement avec **GenericWebPart** contrôles au moment de l’exécution afin que vous puissiez les utiliser avec les fonctionnalités WebPart. |
| CatalogPart | Contient une liste de contrôles WebPart disponibles que les utilisateurs peuvent ajouter à la page. |
| WebPartConnection | Crée une connexion entre deux contrôles WebPart sur une page. La connexion définit un des contrôles WebPart en tant que fournisseur (de données) et l’autre comme un consommateur. |
| EditorPart | Sert de classe de base pour les contrôles d’édition spécialisés. |
| Contrôles EditorPart (AppearanceEditorPart LayoutEditorPart, BehaviorEditorPart et PropertyGridEditorPart) | Permettre aux utilisateurs de personnaliser différents aspects des contrôles d’interface utilisateur WebPart sur une page |

## <a name="lab-create-a-web-part-page"></a>Atelier pratique : Créer une Page WebPart

Dans cet atelier, vous allez créer une page WebPart qui rend persistantes les informations par le biais de profils ASP.NET.

### <a name="creating-a-simple-page-with-web-parts"></a>Création d’une Page Simple avec composants WebPart

Dans cette partie de la procédure pas à pas, vous créez une page qui utilise des contrôles WebPart pour afficher le contenu statique. La première étape de l’utilisation de composants WebPart consiste à créer une page avec deux éléments structurels requis. Tout d’abord, une page WebPart requiert un contrôle WebPartManager pour suivre et coordonner tous les contrôles WebPart. En second lieu, une page WebPart a besoin d’une ou plusieurs zones, qui sont des contrôles composites qui contiennent le composant WebPart ou autres contrôles serveur et occupent une région spécifiée d’une page.

> [!NOTE]
> Vous n’avez pas besoin de rien à faire pour activer la personnalisation WebPart ; Il est activé par défaut pour le jeu de composants WebPart. Lorsque vous exécutez tout d’abord une page WebPart sur un site, ASP.NET définit un fournisseur de personnalisations par défaut pour stocker les paramètres de personnalisation utilisateur. Pour plus d’informations sur la personnalisation, consultez Vue d’ensemble de personnalisation de parties Web.


### <a name="to-create-a-page-for-containing-web-parts-controls"></a>Pour créer une page contenant des contrôles WebPart

1. Fermez la page par défaut et ajoutez une nouvelle page au site nommé WebPartsDemo.aspx.
2. Basculez vers **conception** vue.
3. À partir de la **vue** menu, vérifiez que le **contrôles Non visuels** et **détails** options sont sélectionnées afin que vous puissiez voir les balises de mise en page et les contrôles qui n’ont pas d’une interface utilisateur.
4. Placez le point d’insertion avant le `<div>` de balises dans la surface de conception, puis appuyez sur ENTRÉE pour ajouter une nouvelle ligne. Placez le point d’insertion avant le caractère de nouvelle ligne, cliquez sur le **Format du bloc** contrôler dans le menu de liste déroulante, puis sélectionnez le **titre 1** option. Dans l’en-tête, ajoutez le texte **Page de démonstration WebPart**.
5. À partir de la **WebParts** onglet de la boîte à outils, faites glisser un **WebPartManager** contrôle sur la page, en plaçant juste après le caractère de nouvelle ligne et avant le `<div>`balises.   
  
   Le **WebPartManager** contrôle ne pas effectuer le rendu de sortie, afin qu’il apparaisse comme une case grise sur l’aire du concepteur.
6. Positionnez le curseur dans le `<div>` balises.
7. Dans le **disposition** menu, cliquez sur **insérer un tableau**et créer une table qui comporte une ligne et trois colonnes. Cliquez sur le **propriétés de cellule** bouton, sélectionnez **haut** à partir de la **Alignement Vertical** la liste déroulante, cliquez sur **OK**et cliquez sur **OK** à nouveau pour créer la table.
8. Dans la colonne de table de gauche, faites glisser un contrôle WebPartZone. Cliquez sur le **WebPartZone** contrôler, choisissez **propriétés**et définissez les propriétés suivantes :   
  
   ID : SidebarZone   
  
   HeaderText : barre latérale
9. Faites glisser un deuxième **WebPartZone** un contrôle dans la colonne de table intermédiaire et définissez les propriétés suivantes :   
  
   ID : MainZone   
  
   HeaderText : principal
10. Enregistrez le fichier.

Votre page dispose maintenant de deux zones distinctes que vous pouvez contrôler séparément. Toutefois, aucune zone n’a aucun contenu, afin de créer du contenu est l’étape suivante. Pour cette procédure pas à pas, vous travaillez avec des contrôles WebPart qui affichent uniquement du contenu statique.

La disposition d’une zone WebPart est spécifiée par un &lt;zonetemplate&gt; élément. Dans le modèle de zone, vous pouvez ajouter n’importe quel contrôle ASP.NET, qu’il s’agisse d’un contrôle WebPart personnalisé, un contrôle utilisateur ou un contrôle serveur existant. Notez que vous utilisez ici le contrôle d’étiquette et pour que vous ajoutez simplement un texte statique. Lorsque vous placez un contrôle serveur régulière dans une **WebPartZone** zone, ASP.NET traite le contrôle comme un contrôle WebPart au moment de l’exécution, ce qui active des fonctions de composants WebPart sur le contrôle.

**Pour créer du contenu pour la zone principale**

1. Dans **conception** afficher, faites glisser un **étiquette** contrôle depuis la **Standard** onglet de la boîte à outils dans la zone de contenu de la zone dont **ID** propriété est défini sur MainZone.
2. Basculez vers **Source** vue. Notez qu’un &lt;zonetemplate&gt; élément a été ajouté pour encapsuler le **étiquette** contrôle dans MainZone.
3. Ajoutez un attribut nommé **titre** à la &lt;asp : label&gt; élément et définissez sa valeur au contenu. Supprimez le texte = attribut « Label » à partir de la &lt;asp : label&gt; élément. Entre les balises d’ouverture et fermeture de la &lt;asp : label&gt; élément, ajouter du texte tel que **Bienvenue sur la Page d’accueil** au sein d’une paire de &lt;h2&gt; balises d’élément. Votre code doit se présenter comme suit. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample21.aspx)]
4. Enregistrez le fichier.

Ensuite, créez un contrôle utilisateur qui peut également être ajouté à la page comme un contrôle WebPart.

### <a name="to-create-a-user-control"></a>Pour créer un contrôle utilisateur

1. Ajouter un nouveau contrôle utilisateur de Web à votre site comme un contrôle de recherche. Désélectionnez l’option **placer le code source dans un fichier distinct**. Ajoutez-le dans le même répertoire que la page WebPartsDemo.aspx et nommez-le SearchUserControl.ascx.   
  
    > [!NOTE]
    > Le contrôle utilisateur pour cette procédure pas à pas n’implémente pas les fonctionnalités de recherche réelles ; Il est utilisé uniquement pour illustrer les fonctionnalités de Web Parts.
2. Basculez vers **conception** vue. À partir de la **Standard** onglet de la boîte à outils, faites glisser un contrôle de zone de texte sur la page.
3. Placez le point d’insertion après la zone de texte que vous venez d’ajouter et appuyez sur ENTRÉE pour ajouter une nouvelle ligne.
4. Faites glisser un contrôle de bouton sur la page sur la nouvelle ligne sous la zone de texte que vous venez d’ajouter.
5. Basculez vers **Source** vue. Assurez-vous que le code source pour le contrôle utilisateur ressemble à l’exemple suivant. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample22.aspx)]
6. Enregistrez et fermez le fichier.

Vous pouvez maintenant ajouter des contrôles WebPart à la zone de barre latérale. Vous ajoutez deux contrôles à la zone de barre latérale, un contenant une liste de liens et l’autre est le contrôle utilisateur que vous avez créé dans la procédure précédente. Les liens sont ajoutés comme une norme **étiquette** contrôle de serveur, la même manière que vous avez créé le texte statique pour la zone principale. Toutefois, bien que les contrôles serveur contenus dans le contrôle utilisateur peut être contenu directement dans la zone (par exemple, le contrôle d’étiquette), dans ce cas ils ne le sont pas. Au lieu de cela, ils font partie du contrôle utilisateur que vous avez créé dans la procédure précédente. Cet exemple montre une méthode courante d’empaqueter tous les contrôles et fonctionnalités supplémentaires que vous souhaitez dans un contrôle utilisateur, puis de référencer ce contrôle dans une zone comme un contrôle WebPart.

Au moment de l’exécution, le jeu de composants WebPart inclut des contrôles avec des contrôles de GenericWebPart. Quand un **GenericWebPart** contrôle encapsule un contrôle serveur Web, le contrôle part générique est le contrôle parent, et vous pouvez accéder au contrôle serveur via la propriété de ChildControl du contrôle parent. Cette utilisation de contrôles WebPart générique permet à des contrôles serveur Web standard d’avoir le même comportement de base et les attributs sous forme de contrôles WebPart qui dérivent de la **WebPart** classe.

### <a name="to-add-web-parts-controls-to-the-sidebar-zone"></a>Pour ajouter des contrôles WebPart à la zone de barre latérale

1. Ouvrez la page WebPartsDemo.aspx.
2. Basculez vers **conception** vue.
3. Faites glisser la page de contrôle utilisateur que vous avez créé, SearchUserControl.ascx, de **l’Explorateur de solutions** dans la zone dont **ID** propriété a la valeur SidebarZone et placez-le à cet endroit.
4. Enregistrez la page WebPartsDemo.aspx.
5. Basculez vers **Source** vue.
6. À l’intérieur de la &lt;asp : webpartzone&gt; élément pour SidebarZone, juste au-dessus de la référence à votre contrôle utilisateur, ajoutez un &lt;asp : label&gt; élément avec le contenu des liens, comme indiqué dans l’exemple suivant. En outre, ajouter un **titre** la balise du contrôle utilisateur, avec une valeur de l’attribut **recherche**, comme indiqué. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample23.aspx)]
7. Enregistrez et fermez le fichier.

Vous pouvez maintenant tester votre page en y accédant dans votre navigateur. La page affiche les deux zones. La capture d’écran suivante montre la page.

**Page de démonstration WebPart avec deux zones**


![Capture d’écran de procédure pas à pas 1 Web VS des WebParts](profiles-themes-and-web-parts/_static/image3.gif)

**Figure 3**: Web VS des WebParts capture d’écran procédure pas à pas 1


Dans le titre de la barre de chaque contrôle est une flèche vers le bas qui permet d’accéder à un menu d’actions verbales des actions disponibles, que vous pouvez effectuer sur un contrôle. Cliquez sur le menu d’actions verbales pour un des contrôles, puis cliquez sur le **réduire** verbe et notez que le contrôle est réduit. Dans le menu d’actions verbales, cliquez sur **restaurer**, et le contrôle retourne à sa taille normale.

### <a name="enabling-users-to-edit-pages-and-change-layout"></a>Permettre aux utilisateurs de modifier des Pages et de modifier la disposition

Composants WebPart offre la possibilité aux utilisateurs de modifier la disposition des contrôles WebPart en les faisant glisser à partir d’une zone à l’autre. En plus de permettre aux utilisateurs de déplacer **WebPart** contrôles d’un fuseau vers un autre, vous pouvez autoriser les utilisateurs de modifier différentes caractéristiques des contrôles, y compris leur apparence, la disposition et le comportement. Fournit des fonctionnalités de modification de base pour le jeu de composants WebPart **WebPart** contrôles. Bien que vous n’aurez pas à le faire dans cette procédure pas à pas, vous pouvez également créer des contrôles d’édition personnalisés qui permettent aux utilisateurs de modifier les fonctionnalités de **WebPart** contrôles. Comme avec la modification de l’emplacement d’un **WebPart** contrôle, modification des propriétés d’un contrôle repose sur la personnalisation d’ASP.NET pour enregistrer les modifications apportées par les utilisateurs.

Dans cette partie de la procédure pas à pas, vous ajoutez la possibilité aux utilisateurs de modifier les caractéristiques de base de n’importe quel **WebPart** contrôle sur la page. Pour activer ces fonctionnalités, vous ajoutez un autre contrôle utilisateur personnalisé à la page, avec un &lt;asp : editorzone&gt; élément et deux contrôles d’édition.

### <a name="to-create-a-user-control-that-enables-changing-page-layout"></a>Pour créer un contrôle utilisateur qui permet la modification de mise en page

1. Dans Visual Studio, sur le **fichier** menu, sélectionnez le **New** sous-menu, puis cliquez sur le **fichier** option.
2. Dans le **ajouter un nouvel élément** boîte de dialogue, sélectionnez **contrôle utilisateur Web**. Nommez le nouveau fichier DisplayModeMenu.ascx. Désélectionnez l’option **placer le code source dans un fichier distinct**.
3. Cliquez sur Ajouter pour créer le nouveau contrôle.
4. Basculez vers **Source** vue.
5. Supprimer tout le code existant dans le nouveau fichier et collez le code suivant. Ce code de contrôle utilisateur utilise des fonctionnalités de l’ensemble du contrôle WebPart qui permettent une page de modifier son mode d’affichage et vous permet également de modifier l’apparence physique et la disposition de la page pendant que vous sont dans certains modes d’affichage. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample24.aspx)]
6. Enregistrez le fichier en cliquant sur l’enregistrement icône dans la barre d’outils, ou en sélectionnant **enregistrer** sur le **fichier** menu.

### <a name="to-enable-users-to-change-the-layout"></a>Pour permettre aux utilisateurs de modifier la disposition

1. Ouvrez la page WebPartsDemo.aspx, puis basculez vers **conception** vue.
2. Placez le point d’insertion dans le **conception** afficher juste après le **WebPartManager** contrôle que vous avez ajouté précédemment. Ajoute un retour chariot après le texte pour qu’il y a un saut de ligne après le **WebPartManager** contrôle. Placez le point d’insertion sur la ligne vide.
3. Faites glisser le contrôle utilisateur que vous venez de créer (le fichier est nommé DisplayModeMenu.ascx) dans le WebPartsDemo.aspx page et déposez-la sur la ligne vide.
4. Faites glisser un contrôle EditorZone de la **WebParts** section de la boîte à outils vers la cellule de tableau open restantes dans la page WebPartsDemo.aspx.
5. À partir de la **WebParts** section de la boîte à outils, faites glisser un contrôle AppearanceEditorPart et un contrôle LayoutEditorPart dans le **EditorZone** contrôle.
6. Basculez vers **Source** vue. Le code résultant dans la cellule de table doit ressembler au code suivant. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample25.aspx)]
7. Enregistrez le fichier WebPartsDemo.aspx. Vous avez créé un contrôle utilisateur qui vous permet de modifier les modes d’affichage et de modifier la mise en page, et vous avez référencé le contrôle sur la page Web principale.

Vous pouvez maintenant tester la capacité de modifier des pages et mise en page.

### <a name="to-test-layout-changes"></a>Pour tester les modifications de disposition

1. Chargez la page dans un navigateur.
2. Cliquez sur le **Mode d’affichage** menu déroulant, puis sélectionnez **modifier**. Les titres de zone sont affichés.
3. Faites glisser le **Mes liens** contrôle par sa barre de titre de la zone de barre latérale vers le bas de la zone principale. Votre page doit ressembler à la capture d’écran suivante.

### <a name="web-parts-demo-page-with-my-links-control-moved"></a>Page de démonstration WebPart avec contrôle de mes liens déplacé


![Capture d’écran de procédure pas à pas 2 Web VS des WebParts](profiles-themes-and-web-parts/_static/image4.gif)

**Figure 4**: Web capture d’écran de la procédure pas à pas 2 VS des WebParts


1. Cliquez sur le **Mode d’affichage** menu déroulant, puis sélectionnez **Parcourir**. La page est actualisée, les noms de zones disparaissent et le **Mes liens** contrôle reste où vous l’avez positionné.
2. Pour démontrer que la personnalisation fonctionne, fermez le navigateur et puis charger à nouveau la page. Les modifications apportées sont enregistrées pour les futures sessions de navigateur.
3. À partir de la **Mode d’affichage** menu, sélectionnez **modifier**.   
  
   Chaque contrôle sur la page est maintenant affiché avec une flèche vers le bas dans sa barre de titre, qui contient la liste déroulante d’actions verbales.
4. Cliquez sur la flèche pour afficher le menu d’actions verbales sur le **Mes liens** contrôle. Cliquez sur le **modifier** verbe.   
  
   Le **EditorZone** contrôle apparaît, affichant le EditorPart des contrôles que vous avez ajouté.
5. Dans le **apparence** section du contrôle d’édition, modifier le **titre** dans Mes favoris, utilisez le **Chrome Type** liste déroulante pour sélectionner **titre uniquement**, puis cliquez sur **appliquer**. La capture d’écran suivante montre la page en mode édition.

### <a name="web-parts-demo-page-in-edit-mode"></a>Page de démonstration WebPart en mode édition


![Capture d’écran de procédure pas à pas 3 Web VS des WebParts](profiles-themes-and-web-parts/_static/image5.gif)

**Figure 5**: Web capture d’écran de la procédure pas à pas 3 VS des WebParts


1. Cliquez sur le **Mode d’affichage** menu, puis sélectionnez **Parcourir** pour repasser en mode de navigation.
2. Le contrôle a maintenant un titre mis à jour et aucune bordure, comme indiqué dans la capture d’écran suivante.

### <a name="edited-web-parts-demo-page"></a>Page de démonstration WebPart modifiée


![Capture d’écran de procédure pas à pas 4 Web VS des WebParts](profiles-themes-and-web-parts/_static/image6.gif)

**Figure 4**: Web capture d’écran de la procédure pas à pas 4 VS des WebParts


### <a name="adding-web-parts-at-run-time"></a>Ajout de composants WebPart en cours d’exécution

Vous pouvez également autoriser les utilisateurs à ajouter des contrôles WebPart à leur page en cours d’exécution. Pour ce faire, configurez la page avec un catalogue de composants WebPart, qui contient une liste de contrôles WebPart que vous souhaitez rendre disponibles aux utilisateurs.

**Pour permettre aux utilisateurs d’ajouter des composants WebPart en cours d’exécution**

1. Ouvrez la page WebPartsDemo.aspx, puis basculez vers **conception** vue.
2. À partir de la **WebParts** onglet de la boîte à outils, faites glisser un contrôle CatalogZone dans la colonne de droite de la table, en dessous du **EditorZone** contrôle.   
  
   Les deux contrôles peuvent être dans la même cellule de table, car ils ne seront pas affichés en même temps.
3. Dans le volet Propriétés, affectez la chaîne **ajouter des composants WebPart** à la propriété HeaderText de la **CatalogZone** contrôle.
4. À partir de la **WebParts** section de la boîte à outils, faites glisser un contrôle DeclarativeCatalogPart dans la zone de contenu de la **CatalogZone** contrôle.
5. Cliquez sur la flèche dans le coin supérieur droit de la **DeclarativeCatalogPart** contrôler pour exposer son menu tâches, puis sélectionnez **modifier les modèles**.
6. À partir de la **Standard** section de la boîte à outils, faites glisser un **FileUpload** contrôle et un **calendrier** un contrôle dans le **WebPartsTemplate** section de la **DeclarativeCatalogPart** contrôle.
7. Basculez vers **Source** vue. Inspecter le code source de la &lt;asp : catalogzone&gt; élément. Notez que le **DeclarativeCatalogPart** contrôle contient un &lt;webpartstemplate&gt; élément avec les deux contrôles serveur délimités auxquelles vous serez en mesure d’ajouter à votre page à partir du catalogue.
8. Ajouter un **titre** propriété pour chacun des contrôles que vous avez ajouté au catalogue, à l’aide de la valeur de chaîne affichée pour chaque titre dans l’exemple de code ci-dessous. Même si le titre n’est pas une propriété vous pouvez normalement définir sur ces deux contrôles serveur au moment du design, lorsqu’un utilisateur ajoute ces contrôles à un **WebPartZone** zone à partir du catalogue au moment de l’exécution, ils sont chacun encapsulés dans un  **GenericWebPart** contrôle. Cela leur permet d’agir en tant que contrôles WebPart, par conséquent, ils pourront afficher les titres.   
  
   Le code pour les deux contrôles contenus dans le **DeclarativeCatalogPart** contrôle doit se présenter comme suit. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample26.aspx)]
9. Enregistrez la page.

Vous pouvez maintenant tester le catalogue.

### <a name="to-test-the-web-parts-catalog"></a>Pour tester le catalogue de composants WebPart

1. Chargez la page dans un navigateur.
2. Cliquez sur le **Mode d’affichage** menu déroulant, puis sélectionnez **catalogue**.   
  
   Le catalogue intitulé **ajouter des composants WebPart** s’affiche.
3. Faites glisser le **Mes favoris** contrôler à partir de la zone principale vers le haut de la zone de barre latérale et placez-le à cet endroit.
4. Dans le **ajouter des composants WebPart** de catalogue, sélectionnez les deux cases à cocher, puis **Main** dans la liste déroulante qui contient les zones disponibles.
5. Cliquez sur **ajouter** dans le catalogue. Les contrôles sont ajoutés à la zone principale. Si vous le souhaitez, vous pouvez ajouter plusieurs instances de contrôles à partir du catalogue à votre page.   
  
   La capture d’écran suivante montre la page avec le contrôle de téléchargement de fichier et le calendrier dans la zone principale. 

![Contrôles ajoutés à la zone Main à partir du catalogue](profiles-themes-and-web-parts/_static/image7.gif)

    **Figure 5**: Controls added to Main zone from the catalog
6. Cliquez sur le **Mode d’affichage** menu déroulant, puis sélectionnez **Parcourir**. Le catalogue disparaît et la page est actualisée.
7. Fermez le navigateur. Charger à nouveau la page. Les modifications apportées persistent.
