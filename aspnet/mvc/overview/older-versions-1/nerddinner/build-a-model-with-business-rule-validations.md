---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: Créer un modèle avec des Validations de règles d’entreprise | Microsoft Docs
author: microsoft
description: Étape 3 montre comment créer un modèle que nous pouvons utiliser pour les deux requêtes et mettre à jour de la base de données pour notre application NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: b735c414c629b9ff6617dbf80782d57543306c00
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835665"
---
<a name="build-a-model-with-business-rule-validations"></a>Créer un modèle avec des Validations de règles d’entreprise
====================
par [Microsoft](https://github.com/microsoft)

[Télécharger PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Il s’agit d’étape 3 a gratuit [« « l’application NerdDinner](introducing-the-nerddinner-tutorial.md) qui présente en détail comment créer un petit mais terminé, l’application web à l’aide d’ASP.NET MVC 1.
> 
> Étape 3 montre comment créer un modèle que nous pouvons utiliser pour les deux requêtes et mettre à jour de la base de données pour notre application NerdDinner.
> 
> Si vous utilisez ASP.NET MVC 3, nous vous recommandons de suivre le [mise en route avec MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [Store de musique MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) didacticiels.


## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner étape 3 : Création du modèle

Dans une infrastructure model-view-controller, le terme « modèle » fait référence aux objets qui représentent les données de l’application, ainsi que la logique de domaine correspondante qui s’intègre de validation et des règles d’entreprise avec lui. Le modèle est à bien des égards « cœur » d’une application basée sur MVC et comme nous le verrons plus tard fondamentalement lecteurs le comportement de celle-ci.

L’infrastructure ASP.NET MVC prend en charge à l’aide de toute technologie d’accès aux données et les développeurs peuvent choisir parmi un éventail de riches options de données .NET pour implémenter leurs modèles, y compris : LINQ to Entities, LINQ to SQL, NHibernate, LLBLGen Pro, SubSonic, WilsonORM ou ADO brute. NET DataReaders ou jeux de données.

Pour notre application NerdDinner, nous allons utiliser LINQ to SQL pour créer un modèle simple qui correspond assez près à notre conception de base de données et ajoute des règles d’entreprise et la logique de validation personnalisée. Nous sera ensuite implémenter une classe de référentiel qui permet d’abstraite de suite l’implémentation de persistance des données du reste de l’application et vous permet de nous unité facilement le tester.

### <a name="linq-to-sql"></a>LINQ to SQL

LINQ to SQL est un ORM (mappeur relationnel objet) qui est fourni avec .NET 3.5.

LINQ to SQL fournit un moyen simple pour mapper des tables de base de données aux classes .NET que nous pouvons coder. Pour notre application NerdDinner nous allons l’utiliser pour mapper les tables dîners et RSVP dans notre base de données aux classes Dinner et RSVP. Les colonnes des tables dîners et RSVP correspond aux propriétés sur les classes Dinner et RSVP. Chaque objet dîner et RSVP représente une ligne distincte dans les tables dîners ou RSVP dans la base de données.

LINQ to SQL nous permet à éviter d’avoir à construire des instructions SQL pour récupérer et mettre à jour dîner et RSVP manuellement les objets de base de données. Au lieu de cela, nous allons définir les classes Dinner et RSVP, comment elles sont mappées vers/à partir de la base de données et les relations entre eux. LINQ to SQL sera, prend soin de génération de la logique d’exécution SQL appropriée à utiliser lors de l’exécution lorsque nous interagir et que vous les utilisez.

Nous pouvons utiliser la prise en charge du langage LINQ dans Visual Basic et c# pour écrire des requêtes expressives qui récupèrent dîner et RSVP objets à partir de la base de données. Cela réduit la quantité de code de données nous devons écrire, et nous permet de créer des applications vraiment soignées.

### <a name="adding-linq-to-sql-classes-to-our-project"></a>Ajout Classes LINQ to SQL à notre projet

Nous allons commencer en cliquant sur le dossier « Models » au sein de notre projet, puis sélectionnez le **Add -&gt;un nouvel élément** commande de menu :

![](build-a-model-with-business-rule-validations/_static/image1.png)

Cela fera apparaître la boîte de dialogue « Ajouter un nouvel élément ». Nous allons filtrer par la catégorie « Données » et sélectionnez le modèle de « Classes de LINQ à SQL » qu’il contient :

![](build-a-model-with-business-rule-validations/_static/image2.png)

Nous allons nommer l’élément « NerdDinner » et cliquez sur le bouton « Ajouter ». Visual Studio ajoute un fichier NerdDinner.dbml sous notre répertoire \Models et puis ouvrez le concepteur LINQ to SQL objet relationnel :

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>Création Classes de modèle de données avec LINQ to SQL

LINQ to SQL vous permet de créer rapidement des classes de modèle de données à partir de schéma de base de données existant. Action cela nous allons ouvrir la base de données de NerdDinner dans l’Explorateur de serveurs, puis sélectionnez les Tables nous souhaitons modéliser qu’il contient :

![](build-a-model-with-business-rule-validations/_static/image4.png)

Ensuite, nous pouvons faire glisser les tables sur LINQ à l’aire du concepteur SQL. Quand nous faisons ce LINQ to SQL crée automatiquement dîner et classes RSVP en utilisant le schéma des tables (avec des propriétés de la classe qui mappent aux colonnes de table de base de données) :

![](build-a-model-with-business-rule-validations/_static/image5.png)

Par défaut LINQ to SQL designer automatiquement « Pluralise » les noms de table et de colonne lorsqu’il crée des classes basées sur un schéma de base de données. Par exemple : la table « Dîners » dans notre exemple ci-dessus a entraîné une classe « Dîner ». Cette classe d’affectation de noms permet de rendre nos modèles cohérent avec les conventions d’affectation de noms .NET, et généralement trouver celui dont la correction du concepteur cela des pratique (en particulier lorsque vous ajoutez un grand nombre de tables). Si vous n’aimez pas le nom d’une classe ou une propriété généré par le concepteur, bien que vous pouvez toujours remplacer et remplacez-le par le nom de que votre choix. Faire cela en modifiant la propriété d’entité/nom en ligne dans le concepteur ou en la modifiant par le biais de la grille des propriétés.

Par défaut, le concepteur LINQ to SQL également inspecte les relations de clé étrangère/clé primaire des tables et selon automatiquement crée par défaut « associations relation » entre les classes de modèle différent qu'il crée. Par exemple, lorsque nous avons fait glisser les dîners et RSVP tables sur le concepteur LINQ to SQL, une association de relation un-à-plusieurs entre les deux a été inférée basé sur le fait que la table RSVP avait une clé étrangère à la table dîners (cela est indiqué par la flèche dans le Concepteur) :

![](build-a-model-with-business-rule-validations/_static/image6.png)

L’association ci-dessus entraîne LINQ to SQL pour ajouter une propriété de « Dîner » fortement typée à la classe RSVP que les développeurs peuvent utiliser pour accéder au dîner associé à un protocole RSVP donné. Cela entraînera également la classe dîner avoir une propriété de collection « RSVPs » qui permet aux développeurs de récupérer et mettre à jour des objets RSVP associés à un dîner particulier.

Vous trouverez ci-dessous un exemple d’intellisense dans Visual Studio lorsque nous créer un nouvel objet RSVP et l’ajouter à la collection de RSVP d’un dîner. Notez comment LINQ to SQL ajouté automatiquement une collection de « RSVP » sur l’objet Dinner :

![](build-a-model-with-business-rule-validations/_static/image7.png)

En ajoutant l’objet RSVP pour collection de RSVP du dîner, nous demandons à LINQ to SQL pour associer une relation de clé étrangère entre le dîner et la ligne RSVP dans notre base de données :

![](build-a-model-with-business-rule-validations/_static/image8.png)

Si vous n’aimez pas comment le concepteur a modélisé ou nommée d’une association de table, vous pouvez le remplacer. Cliquez sur la flèche d’association dans le concepteur et accéder à ses propriétés par le biais de la grille des propriétés à renommer, supprimer ou modifier. Pour notre application NerdDinner, cependant, les règles d’association par défaut fonctionnent bien pour les classes de modèle de données que nous créons et nous pouvons simplement utiliser le comportement par défaut.

### <a name="nerddinnerdatacontext-class"></a>Classe de NerdDinnerDataContext

Visual Studio crée automatiquement les classes .NET qui représentent les modèles et les relations de base de données définies à l’aide du concepteur LINQ to SQL. Une classe LINQ to SQL DataContext est également généré pour chaque fichier LINQ to SQL concepteur ajouté à la solution. Étant donné que nous avons nommé notre LINQ à l’élément de classe SQL « NerdDinner », la classe DataContext créée sera appelée « NerdDinnerDataContext ». Cette classe NerdDinnerDataContext est le principal moyen de que nous interagissent avec la base de données.

Notre classe NerdDinnerDataContext expose deux propriétés - « Dîners » et « RSVPs » - qui représentent les deux tables que nous FAÇONNÉES dans la base de données. Nous pouvons utiliser c# pour écrire des requêtes LINQ sur ces propriétés dans la requête et la récupération d’objets dîner et RSVP à partir de la base de données.

Le code suivant montre comment instancier un objet NerdDinnerDataContext et exécuter une requête LINQ pour obtenir une séquence de dîners qui se produisent dans le futur. Visual Studio fournit des fonctionnalités intellisense complètes lors de l’écriture de la requête LINQ et les objets retournés à partir de celui-ci sont fortement typées et prennent également en charge intellisense :

![](build-a-model-with-business-rule-validations/_static/image9.png)

En plus de ce qui nous permet d’interroger des objets dîner et RSVP, un NerdDinnerDataContext suit également automatiquement les modifications que nous apportez par la suite aux objets dîner et RSVP que nous récupérons par son intermédiaire. Nous pouvons utiliser cette fonctionnalité pour facilement enregistrer les modifications apportées à la base de données - sans avoir à écrire de code de mise à jour SQL explicite.

Par exemple, le code ci-dessous montre comment utiliser une requête LINQ pour récupérer un seul objet dîner à partir de la base de données, mettre à jour deux des propriétés dîner et puis enregistrez les modifications apportées à la base de données :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

L’objet NerdDinnerDataContext dans le code ci-dessus suivies automatiquement les modifications de propriété apportées à l’objet dîner récupérés à partir de celui-ci. Lorsque nous avons appelé la méthode « SubmitChanges() », il exécutera une instruction « UPDATE » SQL appropriée pour la base de données pour conserver les valeurs de mise à jour.

### <a name="creating-a-dinnerrepository-class"></a>Création d’une classe DinnerRepository

Pour les petites applications, il est parfois bien d’avoir des contrôleurs de travailler directement sur une classe LINQ to SQL DataContext et incorporer des requêtes LINQ dans les contrôleurs. Les applications deviennent plus volumineuses, cependant, cette approche devient difficile à maintenir et à tester. Il peut également entraîner nous dupliquer les mêmes requêtes LINQ à plusieurs endroits.

Une des approches qui peuvent rendre les applications plus faciles à maintenir et à tester sont d’utiliser un modèle de « dépôt ». Une classe de dépôt permet d’encapsuler des données interrogation et logique de persistance et élimine les détails d’implémentation de la persistance des données à partir de l’application. En plus de rendre le code d’application plus propre, à l’aide d’un modèle de référentiel peut faciliter modifier les implémentations de stockage de données à l’avenir, et elle peut aider à faciliter le test unitaire une application sans nécessiter une véritable base de données.

Pour notre application NerdDinner, nous allons définir une classe DinnerRepository avec les dessous de signature :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*Remarque : Plus loin dans ce chapitre nous extraire une interface IDinnerRepository à partir de cette classe et activer l’injection de dépendances avec elle sur nos contrôleurs. Pour commencer, cependant, nous allons démarrer simple et simplement fonctionner directement avec la classe DinnerRepository.*

Pour implémenter cette classe nous avec le bouton droit sur le dossier « Models » et choisissez le **Add -&gt;un nouvel élément** commande de menu. Dans la boîte de dialogue « Ajouter un nouvel élément », nous allons sélectionner le modèle de « Classe » et nommez le fichier « DinnerRepository.cs » :

![](build-a-model-with-business-rule-validations/_static/image10.png)

Nous pouvons ensuite implémenter notre classe DinnerRespository en utilisant le code ci-dessous :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>Récupération, la mise à jour, insertion et suppression à l’aide de la classe DinnerRepository

Maintenant que nous avons créé notre classe DinnerRepository, nous allons examiner quelques exemples de code qui illustrent des tâches courantes, que nous pouvons faire avec celui-ci :

#### <a name="querying-examples"></a>Exemples d’interrogation

Le code ci-dessous récupère un dîner unique à l’aide de la valeur DinnerID :


[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

Le code ci-dessous récupère tous les prochains dîners et boucles au-dessus d’eux :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>Insertion et des exemples de mise à jour

Le code ci-dessous illustre l’ajout de deux dîners de nouveau. Ajouts/modifications au référentiel ne sont pas validées dans la base de données jusqu'à ce que la méthode « Save() » est appelée sur lui. LINQ to SQL encapsule automatiquement toutes les modifications dans une transaction de base de données : soit toutes les modifications se produisent ou aucun d'entre eux ne lors de notre référentiel enregistre :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

Le code ci-dessous récupère un objet dîner existant et modifie les deux propriétés sur celle-ci. Les modifications sont validées dans la base de données lorsque la méthode « Save() » est appelée sur notre référentiel :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

Le code ci-dessous récupère un dîner, puis ajoute un protocole RSVP à celui-ci. Cela à l’aide de la collection des RSVP sur l’objet dîner que LINQ to SQL est créé pour nous (car il existe une relation de clé/étrangère-clé primaire entre les deux dans la base de données). Cette modification est rendue persistante dans la base de données en tant qu’une nouvelle ligne de table RSVP lorsque la méthode « Save() » est appelée sur le référentiel :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>Exemple de suppression

Le code suivant récupère un objet dîner existant et le marque à supprimer. Lorsque la méthode « Save() » est appelée sur le référentiel il valide la suppression à la base de données :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>L’intégration de Validation et une logique métier avec les Classes de modèle

L’intégration de validation et la règle d’entreprise logique est une partie essentielle de toute application qui fonctionne avec les données.

#### <a name="schema-validation"></a>Validation de schéma

Lorsque les classes de modèle sont définis en utilisant le concepteur LINQ to SQL, les types de données des propriétés dans les classes de modèle de données correspondent pour les types de données de la table de base de données. Par exemple : si la colonne « EventDate » dans la table dîners est une valeur « datetime », la classe de modèle de données créée par LINQ to SQL sera de type « DateTime » (qui est un type de données .NET intégré). Cela signifie que vous obtenez des erreurs de compilation si vous tentez d’affecter un entier ou une valeur booléenne à celui-ci à partir du code, et il génère une erreur automatiquement si vous essayez de convertir implicitement un type de chaîne non valide à celui-ci lors de l’exécution.

LINQ to SQL gère également automatiquement les valeurs de séquence d’échappement SQL pour vous lors de l’utilisation de chaînes - qui vous aide à vous protéger contre les attaques par injection SQL lors de son utilisation.

#### <a name="validation-and-business-rule-logic"></a>Validation et une logique métier

Validation de schéma est utile dans un premier temps, mais il est rarement suffisante. La plupart des scénarios réels requièrent la capacité de spécifier la logique de validation plus riches qui permettre s’étendre sur plusieurs propriétés, d’exécuter du code et ont souvent une connaissance d’un état de modèle (par exemple : est il créé/mis à jour/supprimées, ou dans un état spécifique à un domaine par exemple « archivés »). Il existe une variété de différents modèles et des infrastructures qui peuvent être utilisées pour définir et appliquer des règles de validation aux classes de modèle, et il existe plusieurs .NET en infrastructures quasi-certitude qui peut être utilisé pour aider à ce sujet. Vous pouvez utiliser à peu près un d’eux dans les applications ASP.NET MVC.

Dans le cadre de notre application NerdDinner, nous allons utiliser un modèle relativement simple et directe où nous exposer une propriété IsValid et une méthode GetRuleViolations() sur notre objet de modèle dîner. La propriété IsValid renvoie true ou false selon que les validation et les règles métier sont tous valides. La méthode GetRuleViolations() retourne une liste de toutes les erreurs règle.

Nous allons implémenter IsValid et GetRuleViolations() pour notre modèle dîner en ajoutant une « classe partielle » à notre projet. Classes partielles peuvent être utilisées pour ajouter des méthodes/propriétés/événements aux classes gérées par un concepteur de Visual Studio (par exemple, la classe dîner généré par le concepteur LINQ to SQL) et d’éviter l’outil à partir de se compliquer la vie avec notre code. Nous pouvons ajouter une nouvelle classe partielle à notre projet en cliquant sur le dossier \Models, puis sélectionnez la commande de menu « Ajouter un nouvel élément ». Nous pouvons ensuite choisir le modèle « Classe » dans la boîte de dialogue « Ajouter un nouvel élément » et nommez-le Dinner.cs.

![](build-a-model-with-business-rule-validations/_static/image11.png)

En cliquant sur le bouton « Ajouter » pour ajouter un fichier Dinner.cs à notre projet et ouvrez-le dans l’IDE. Nous pouvons ensuite implémenter un framework de mise en œuvre de base/validation de la règle à l’aide du code ci-dessous :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

Quelques remarques concernant le code ci-dessus :

- La classe dîner est précédée d’un mot clé « partiels » – ce qui signifie que le code qu’il contient est combiné avec la classe générée/conservés par le concepteur LINQ to SQL et compilé dans une classe unique.
- La classe RuleViolation est une classe d’assistance que nous allons ajouter au projet qui nous permet de fournir plus de détails sur une violation de règle.
- La méthode Dinner.GetRuleViolations() provoque nos règles de validation et d’entreprise doit être évaluée (nous allons les implémenter sous peu). Il renvoie ensuite une séquence d’objets RuleViolation qui fournissent plus de détails sur les erreurs de règle.
- La propriété Dinner.IsValid fournit une propriété d’assistance pratiques qui indique si l’objet dîner a tout RuleViolations actives. Il peut être vérifié de façon proactive par un développeur à l’aide de l’objet dîner à tout moment (et ne lève pas d’exception).
- La méthode partielle Dinner.OnValidate() est un raccordement par LINQ to SQL qui nous permet d’être averti chaque fois que l’objet dîner est sur le point d’être rendues persistantes dans la base de données. Notre implémentation OnValidate() ci-dessus garantit que le dîner n’a aucune RuleViolations avant d’être enregistré. S’il est dans un état non valide, il déclenche une exception, ce qui provoque LINQ to SQL pour abandonner la transaction.

Cette approche fournit une infrastructure simple qui nous pouvons intégrer validation et des règles d’entreprise. Pour l’instant, nous allons ajouter les règles à notre méthode GetRuleViolations() ci-dessous :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

Nous utilisons la fonctionnalité « yield return » de c# pour retourner une séquence de n’importe quel RuleViolations. La six règle vérifie d’abord ci-dessus applique simplement que les propriétés de chaîne sur notre dîner ne peut pas être null ou vide. La dernière règle est un peu plus intéressante et appelle une méthode d’assistance de PhoneValidator.IsValidNumber() que nous l’ajoutions à notre projet pour vérifier que le ContactPhone numéro pays de format correspondances le dîner.

Nous pouvons utiliser. Prise en charge des expressions régulières du NET pour implémenter cette prise en charge de validation de téléphone. Voici une implémentation simple PhoneValidator que nous pouvons ajouter à notre projet qui permet d’ajouter des contrôles de modèle Regex spécifiques à un pays :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>Gestion des Violations de logique métier et de Validation

Maintenant que nous avons ajouté le code de règle de validation et entreprise ci-dessus, à tout moment que nous essayons de créer ou mettre à jour d’un dîner, nos règles de logique de validation seront évaluées et appliquées.

Les développeurs peuvent écrire de code semblable au suivant pour déterminer de manière proactive si un objet dîner est valide et récupérer une liste de toutes les violations qu’elle contient sans déclencher des exceptions :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

Si vous essayez d’enregistrer un dîner dans un état non valide, une exception sera levée lorsque nous appelons la méthode Save() sur le DinnerRepository. Cela se produit, car LINQ to SQL appelle automatiquement notre méthode partielle Dinner.OnValidate() avant elle enregistre les modifications du dîner, et nous avons ajouté le code à Dinner.OnValidate() pour lever une exception si les violations de règle existent dans le dîner. Nous pouvons intercepter cette exception et récupère une liste des violations de résoudre de manière réactive :

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

Étant donné que notre validation et les règles métier sont implémentées au sein de la couche de notre modèle et non pas dans la couche d’interface utilisateur, ils seront appliqués et utilisés pour tous les scénarios au sein de notre application. Nous pouvons ultérieurement modifier ou ajouter des règles d’entreprise et avoir tout le code que fonctionne avec nos objets dîner les respecte pas.

Avoir la possibilité de modifier les règles d’entreprise au même endroit, sans avoir à ces modifications ripple tout au long de l’application et la logique d’interface utilisateur, est un signe d’une application bien écrite et un avantage une infrastructure MVC à encourager.

### <a name="next-step"></a>Étape suivante

Nous avons maintenant un modèle que nous pouvons utiliser pour interroger et de mettre à jour de notre base de données.

Nous allons maintenant ajouter des contrôleurs et des vues au projet que nous pouvons utiliser pour créer une expérience de l’interface utilisateur HTML autour d’elle.

> [!div class="step-by-step"]
> [Précédent](create-a-database.md)
> [Suivant](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
