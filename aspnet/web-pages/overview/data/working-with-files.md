---
uid: web-pages/overview/data/working-with-files
title: Utilisation des fichiers dans un Site ASP.NET Web Pages (Razor) | Microsoft Docs
author: tfitzmac
description: Ce chapitre explique comment lire, écrire, ajouter, supprimer et télécharger des fichiers.
ms.author: aspnetcontent
ms.date: 02/20/2014
ms.assetid: eee916e4-ba4c-439a-a24e-68df7d45a569
msc.legacyurl: /web-pages/overview/data/working-with-files
msc.type: authoredcontent
ms.openlocfilehash: 3778bc0041c56fe7164265fe565996aea5564bdf
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841602"
---
<a name="working-with-files-in-an-aspnet-web-pages-razor-site"></a>Utilisation des fichiers dans un Site ASP.NET Web Pages (Razor)
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment lire, écrire, ajouter, supprimer et télécharger des fichiers dans un site ASP.NET Web Pages (Razor).
> 
> > [!NOTE]
> > Si vous souhaitez charger des images et de les manipuler (par exemple, retourner ou de les redimensionner), consultez [utilisation des Images dans un Site ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202897).
> 
> 
> **Ce que vous allez apprendre :** 
> 
> - Comment créer un fichier texte et écrire des données.
> - Comment ajouter des données à un fichier existant.
> - Guide pratique pour lire un fichier et afficher à partir de celui-ci.
> - Comment supprimer des fichiers à partir d’un site Web.
> - Explique comment permettre aux utilisateurs de charger un ou plusieurs fichiers.
> 
> Il s’agit de la programmation des fonctionnalités introduites dans l’article ASP.NET :
> 
> - Le `File` objet, qui fournit un moyen de gérer les fichiers.
> - Le `FileUpload` helper.
> - Le `Path` objet, qui fournit des méthodes qui vous permettent de manipuler des noms de fichier et chemin d’accès.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> Ce didacticiel fonctionne également avec WebMatrix 3.


<a id="Creating_a_Text_File"></a>
## <a name="creating-a-text-file-and-writing-data-to-it"></a>Création d’un fichier texte et écrire des données

En plus d’utiliser une base de données dans votre site Web, vous pouvez travailler avec des fichiers. Par exemple, vous pouvez utiliser des fichiers texte comme un moyen simple de stocker des données pour le site. (Un fichier texte qui est utilisé pour stocker des données est parfois appelé un *fichier plat*.) Fichiers texte peuvent être dans différents formats, comme *.txt*, *.xml*, ou *.csv* (valeurs délimitée par des virgules).

Si vous souhaitez stocker les données dans un fichier texte, vous pouvez utiliser la `File.WriteAllText` méthode pour spécifier le fichier à créer et les données pour y écrire. Dans cette procédure, vous allez créer une page qui contient un formulaire simple avec trois `input` éléments (prénom, nom et adresse de messagerie) et un **envoyer** bouton. Lorsque l’utilisateur envoie le formulaire, vous stockerez l’entrée d’utilisateur dans un fichier texte.

1. Créez un dossier nommé *application\_données*, s’il n’existe pas déjà.
2. À la racine de votre site Web, créez un fichier nommé *UserData.cshtml*.
3. Remplacez le contenu existant par le suivant : 

    [!code-cshtml[Main](working-with-files/samples/sample1.cshtml)]

    Le balisage HTML crée le formulaire avec les trois zones de texte. Dans le code, vous utilisez le `IsPost` propriété afin de déterminer si la page a été envoyée avant de commencer le traitement.

    La première tâche consiste à obtenir l’entrée d’utilisateur et l’assigner à des variables. Le code concatène ensuite les valeurs des variables distinctes dans une chaîne délimitée par des virgules, qui est ensuite stockée dans une variable différente. Notez que la virgule de séparation est une chaîne contenue dans les guillemets («, »), étant donné que vous incorporez littéralement une virgule dans la chaîne de grande taille que vous créez. À la fin des données qui vous concaténez, vous ajoutez `Environment.NewLine`. Cette opération ajoute un saut de ligne (un caractère de saut de ligne). Ce que vous créez avec tous les cette concaténation est une chaîne qui ressemble à ceci :

    [!code-css[Main](working-with-files/samples/sample2.css)]

    (Avec un saut de ligne invisibles à la fin.)

    Vous créez ensuite une variable (`dataFile`) qui contient l’emplacement et le nom du fichier pour stocker les données dans. Définition de l’emplacement nécessite une gestion spéciale. Dans les sites Web, il est une mauvaise pratique pour faire référence dans le code à des chemins d’accès absolus comme *C:\Folder\File.txt* pour les fichiers sur le serveur web. Si un site Web est déplacé, est incorrecte en un chemin d’accès absolu. En outre, pour un site hébergé (plutôt que sur votre propre ordinateur) généralement sans savoir quel est le chemin d’accès correct lorsque vous écrivez le code.

    Un chemin d’accès complet est nécessaire, mais parfois, (par exemple, maintenant, pour l’écriture d’un fichier). La solution consiste à utiliser le `MapPath` méthode de la `Server` objet. Cela retourne le chemin d’accès complet à votre site Web. Pour obtenir le chemin d’accès pour la racine du site Web, vous utilisateur le `~` opérateur (à represen le site virtuel de la racine) pour `MapPath`. (Vous pouvez également passer un nom de sous-dossier, telles que *~/App\_données /*, afin d’obtenir le chemin d’accès pour ce sous-dossier.) Vous pouvez ensuite concaténer des informations supplémentaires sur quelle que soit la méthode retourne pour créer un chemin d’accès complet. Dans cet exemple, vous ajoutez un nom de fichier. (Vous trouverez plus d’informations sur l’utilisation des chemins d’accès de fichier et dossier dans [Introduction à ASP.NET Web Pages programmation à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    Le fichier est enregistré dans le *application\_données* dossier. Ce dossier est un dossier spécial dans ASP.NET est utilisé pour stocker les fichiers de données, comme décrit dans [Introduction à l’utilisation avec une base de données dans les Sites ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=195209).

    Le `WriteAllText` méthode de la `File` objet écrit les données dans le fichier. Cette méthode accepte deux paramètres : le nom (avec le chemin d’accès) du fichier à écrire dans et les données réelles à écrire. Notez que le nom du premier paramètre a une `@` caractère en tant que préfixe. Cela indique à ASP.NET que vous fournissez un littéral de chaîne textuelle, et que les caractères tels que « / » ne doit pas être interprétée de façon particulière. (Pour plus d’informations, consultez [Introduction à ASP.NET Web Programming à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths).)

    > [!NOTE]
    > Afin que votre code enregistrer les fichiers dans le *application\_données* dossier, l’application a besoin d’autorisations de lecture-écriture pour ce dossier. Sur votre ordinateur de développement cela n’est pas généralement un problème. Toutefois, lorsque vous publiez votre site au serveur de web du fournisseur d’hébergement, vous devez explicitement définir ces autorisations. Si vous exécutez ce code sur le serveur d’un fournisseur d’hébergement et que vous obtenez des erreurs, contactez le fournisseur d’hébergement pour savoir comment définir ces autorisations.

- Exécutez la page dans un navigateur. 

    ![](working-with-files/_static/image1.jpg)
- Entrez des valeurs dans les champs, puis **envoyer**.
- Fermez le navigateur.
- Retournez au projet et actualisez la vue.
- Ouvrez le *data.txt* fichier. Les données fournies dans le formulaire sont dans le fichier. 

    ![[image]](working-with-files/_static/image2.jpg)
- Fermer le *data.txt* fichier.

<a id="Appending_Data"></a>
## <a name="appending-data-to-an-existing-file"></a>Ajout de données à un fichier existant

Dans l’exemple précédent, vous avez utilisé `WriteAllText` pour créer un fichier texte qui a un seul élément de données qu’il contient. Si vous appelez la méthode à nouveau et lui transmettez le même nom de fichier, le fichier existant est complètement remplacé. Toutefois, une fois que vous avez créé un fichier vous souhaiterez ajouter de nouvelles données à la fin du fichier. Vous pouvez faire cela à l’aide du `AppendAllText` méthode de la `File` objet.

1. Dans le site Web, faites une copie de la *UserData.cshtml* de fichiers et le nom de la copie *UserDataMultiple.cshtml*.
2. Remplacez le bloc de code avant l’ouverture `<!DOCTYPE html>` balise avec le bloc de code suivant : 

    [!code-cshtml[Main](working-with-files/samples/sample3.cshtml)]

    Ce code comporte une modification de l’exemple précédent. Au lieu d’utiliser `WriteAllText`, elle utilise `the AppendAllText` (méthode). Les méthodes sont similaires, sauf que `AppendAllText` ajoute les données à la fin du fichier. Comme avec `WriteAllText`, `AppendAllText` crée le fichier s’il n’existe pas déjà.
3. Exécutez la page dans un navigateur.
4. Entrez des valeurs pour les champs, puis **envoyer**.
5. Ajouter plus de données et envoyer le formulaire à nouveau.
6. Revenez à votre projet, cliquez sur le dossier du projet et puis cliquez sur **Actualiser**.
7. Ouvrez le *data.txt* fichier. Il contient désormais les nouvelles données que vous venez d’entrer. 

    ![[image]](working-with-files/_static/image3.jpg)

<a id="Reading_and_Displaying_Data"></a>
## <a name="reading-and-displaying-data-from-a-file"></a>Lecture et l’affichage des données à partir d’un fichier

Même si vous n’avez pas besoin d’écrire des données dans un fichier texte, vous devrez probablement parfois lire des données à partir d’un. Pour ce faire, vous pouvez à nouveau utiliser la `File` objet. Vous pouvez utiliser le `File` objet pour lire chaque ligne individuellement (séparés par des sauts de ligne) ou pour lire un élément individuel, quel que soit la façon dont ils sont classés.

Cette procédure vous montre comment lire et afficher les données que vous avez créé dans l’exemple précédent.

1. À la racine de votre site Web, créez un fichier nommé *DisplayData.cshtml*.
2. Remplacez le contenu existant par le suivant : 

    [!code-cshtml[Main](working-with-files/samples/sample4.cshtml)]

    Le code commence par la lecture du fichier que vous avez créé dans l’exemple précédent dans une variable nommée `userData`, à l’aide de cet appel de méthode :

    [!code-css[Main](working-with-files/samples/sample5.css)]

    Le code permettant cela est à l’intérieur d’un `if` instruction. Lorsque vous souhaitez lire un fichier, il est judicieux d’utiliser le `File.Exists` méthode afin de déterminer tout d’abord si le fichier est disponible. Le code vérifie également si le fichier est vide.

    Le corps de la page contient deux `foreach` effectue une itération, imbriqués à l’intérieur de l’autre. Externe `foreach` boucle Obtient une ligne à la fois à partir du fichier de données. Dans ce cas, les lignes sont définies par des sauts de ligne dans le fichier &#8212; , chaque élément de données est sur sa propre ligne. La boucle externe crée un nouvel élément (`<li>` élément) à l’intérieur d’une liste ordonnée (`<ol>` élément).

    La boucle interne divise chaque ligne de données en utilisant une virgule comme séparateur des éléments (champs). (En fonction de l’exemple précédent, cela signifie que chaque ligne contient trois champs &#8212; le prénom, un nom et une adresse de messagerie, séparées par une virgule.) La boucle interne crée également un `<ul>` liste et affiche une liste d’éléments pour chaque champ dans la ligne de données.

    Le code illustre comment utiliser deux types de données, un tableau et le `char` type de données. Le tableau est nécessaire, car le `File.ReadAllLines` méthode retourne les données sous forme de tableau. Le `char` type de données est nécessaire car le `Split` méthode retourne un `array` dans lequel chaque élément est du type `char`. (Pour plus d’informations sur les tableaux, consultez [Introduction à ASP.NET Web Programming à l’aide de la syntaxe Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects).)
3. Exécutez la page dans un navigateur. Les données que vous avez entré pour les exemples précédents sont affichées. 

    ![[image]](working-with-files/_static/image4.jpg)

> [!TIP] 
> 
> **Affichage des données à partir d’un fichier délimité par des virgules de Microsoft Excel**
> 
> Vous pouvez utiliser Microsoft Excel pour enregistrer les données contenues dans une feuille de calcul sous forme de fichier délimité par des virgules (*.csv* fichier). Lorsque vous le faites, le fichier est enregistré en texte brut, pas au format Excel. Chaque ligne dans la feuille de calcul est séparé par un saut de ligne dans le fichier texte, et chaque élément de données est séparé par une virgule. Vous pouvez utiliser le code indiqué dans l’exemple précédent pour lire un fichier délimité par des virgules d’Excel simplement en modifiant le nom du fichier de données dans votre code.


<a id="Deleting_Files"></a>
## <a name="deleting-files"></a>Suppression de fichiers

Pour supprimer les fichiers à partir de votre site Web, vous pouvez utiliser le `File.Delete` (méthode). Cette procédure montre comment permettre aux utilisateurs de supprimer une image (*.jpg* fichier) à partir d’un *images* dossier s’ils savent le nom du fichier.

> [!NOTE] 
> 
> **Important** dans un site Web de production, vous généralement Limitez les personnes autorisées à apporter des modifications aux données. Pour plus d’informations sur comment configurer l’appartenance et sur les façons d’autoriser les utilisateurs à effectuer des tâches sur le site, consultez [Ajout de la sécurité et l’appartenance à un Site ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202904).


1. Dans le site Web, créez un sous-dossier nommé *images*.
2. Copie un ou plusieurs *.jpg* de fichiers dans le *images* dossier.
3. À la racine du site Web, créez un nouveau fichier nommé *FileDelete.cshtml*.
4. Remplacez le contenu existant par le suivant : 

    [!code-cshtml[Main](working-with-files/samples/sample6.cshtml)]

    Cette page contient un formulaire dans lequel les utilisateurs peuvent entrer le nom d’un fichier image. Ils n’entrent pas le *.jpg* extension de nom de fichier ; en limitant le nom de fichier comme suit, vous aide empêche les utilisateurs de supprimer des fichiers arbitraires sur votre site.

    Le code lit le nom de fichier que l’utilisateur a entré le verrou et qu’il construit ensuite un chemin d’accès complet. Pour créer le chemin d’accès, le code utilise le chemin d’accès du site Web actuel (tel que retourné par la `Server.MapPath` méthode), la *images* nom du dossier, le nom que l’utilisateur a fourni et « .jpg » sous forme de chaîne littérale.

    Pour supprimer le fichier, le code appelle la `File.Delete` méthode, en lui transmettant le chemin d’accès complet que vous venez de créer. À la fin du balisage, le code affiche un message de confirmation que le fichier a été supprimé.
5. Exécutez la page dans un navigateur. 

    ![[image]](working-with-files/_static/image5.jpg)
6. Entrez le nom du fichier à supprimer, puis cliquez sur **envoyer**. Si le fichier a été supprimé, le nom du fichier est affiché en bas de la page.

<a id="Letting_Users_Upload_a_File"></a>
## <a name="letting-users-upload-a-file"></a>Utilisateur de vous permettant de télécharger un fichier

Le `FileUpload` helper permet aux utilisateurs de télécharger des fichiers vers votre site Web. La procédure suivante vous montre comment permettre aux utilisateurs de charger un fichier unique.

1. Ajoutez la bibliothèque de programmes d’assistance de Web ASP.NET à votre site Web, comme décrit dans [l’installation des programmes d’assistance dans un Site ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si vous n’avez pas l’ajouté précédemment.
2. Dans le *application\_données* dossier, créer un dossier et nommez-le *UploadedFiles*.
3. À la racine, créez un nouveau fichier nommé *FileUpload.cshtml*.
4. Remplacez le contenu existant dans la page avec les éléments suivants : 

    [!code-cshtml[Main](working-with-files/samples/sample7.cshtml)]

    La partie du corps de la page utilise le `FileUpload` helper pour créer la zone de téléchargement et les boutons que vous êtes probablement familiarisé avec :

    ![[image]](working-with-files/_static/image6.jpg)

    Les propriétés que vous définissez pour le `FileUpload` helper spécifier que vous voulez qu’une zone unique pour le fichier à charger et que vous souhaitez le bouton Envoyer pour lire **télécharger**. (Vous allez ajouter des zones plus loin dans cet article.)

    Lorsque l’utilisateur clique sur **télécharger**, le code en haut de la page Obtient le fichier et l’enregistre. Le `Request` objet que vous utilisez normalement pour obtenir les valeurs de champs de formulaire a également un `Files` tableau qui contient le (ou les fichiers) qui ont été téléchargés. Vous pouvez obtenir des fichiers individuels en dehors des positions spécifiques dans le tableau &#8212; , par exemple, pour obtenir le premier fichier téléchargé, vous obtenez `Request.Files[0]`, afin d’obtenir le deuxième fichier, vous obtenez `Request.Files[1]`, et ainsi de suite. (N’oubliez pas que dans la programmation, généralement commence le comptage à zéro).

    Lorsque vous récupérez un fichier téléchargé, placez-le dans une variable (ici, `uploadedFile`) afin que vous puissiez les manipuler. Pour déterminer le nom du fichier téléchargé, que vous venez d’obtenir son `FileName` propriété. Toutefois, lorsque l’utilisateur télécharge un fichier, `FileName` contient le nom de l’utilisateur d’origine, qui inclut le chemin d’accès complet. Il peut se présenter comme suit :

    *C:\Users\Public\Sample.txt*

    Vous ne voulez cependant, toutes ces informations de chemin d’accès, car il s’agit du chemin d’accès sur l’ordinateur de l’utilisateur, pas votre serveur. Vous voulez simplement le nom de fichier réel (*Sample.txt*). Vous pouvez éliminer simplement le fichier à partir d’un chemin d’accès à l’aide de la `Path.GetFileName` méthode, comme suit :

    [!code-csharp[Main](working-with-files/samples/sample8.cs)]

    Le `Path` objet est un utilitaire qui a un nombre de méthodes de cette façon que vous pouvez utiliser pour supprimer des chemins d’accès, combiner des tracés et ainsi de suite.

    Une fois que vous avez obtenu le nom du fichier téléchargé, vous pouvez créer un nouveau chemin d’accès où vous souhaitez stocker le fichier chargé dans votre site Web. Dans ce cas, vous combinez `Server.MapPath`, les noms de dossiers (*application\_données/UploadedFiles*) et le nom de fichier qui vient d’être supprimé pour créer un nouveau chemin d’accès. Vous pouvez ensuite appeler le fichier téléchargé `SaveAs` méthode pour enregistrer le fichier.
5. Exécutez la page dans un navigateur. 

    ![[image]](working-with-files/_static/image7.jpg)
6. Cliquez sur **Parcourir** , puis sélectionnez un fichier à charger. 

    ![[image]](working-with-files/_static/image8.jpg)

    La zone de texte suivant pour le **Parcourir** bouton contiendra l’emplacement de chemin d’accès et de fichier.

    ![[image]](working-with-files/_static/image9.jpg)
7. Cliquez sur **Upload**.
8. Dans le site Web, cliquez sur le dossier du projet, puis sélectionnez **Actualiser**.
9. Ouvrez le *UploadedFiles* dossier. Le fichier que vous avez téléchargé est dans le dossier. 

    ![[image]](working-with-files/_static/image10.jpg)

<a id="Letting_Users_Upload_Multiple_Files"></a>
## <a name="letting-users-upload-multiple-files"></a>Laisser les utilisateurs téléchargement plusieurs fichiers

Dans l’exemple précédent, vous laisser les utilisateurs télécharger un fichier. Mais vous pouvez utiliser le `FileUpload` helper pour charger plusieurs fichiers à la fois. Cela est pratique pour les scénarios, tels que le téléchargement de photos, où il est fastidieux de téléchargement d’un fichier à la fois. (Vous trouverez sur le téléchargement de photos dans [utilisation des Images dans un Site ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202897).) Cet exemple montre comment permettre aux utilisateurs de télécharger deux à la fois, bien que vous pouvez utiliser la même technique pour télécharger de plus.

1. Ajoutez la bibliothèque de programmes d’assistance de Web ASP.NET à votre site Web, comme décrit dans [l’installation des programmes d’assistance dans un Site ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si vous n’avez pas déjà.
2. Créer une page nommée *FileUploadMultiple.cshtml*.
3. Remplacez le contenu existant dans la page avec les éléments suivants :  

    [!code-cshtml[Main](working-with-files/samples/sample9.cshtml)]

    Dans cet exemple, le `FileUpload` helper dans le corps de la page est configuré pour permettre aux utilisateurs de télécharger deux fichiers par défaut. Étant donné que `allowMoreFilesToBeAdded` est défini sur `true`, le helper restitue un lien qui permet à utilisateur d’ajouter davantage de zones de téléchargement :

    ![[image]](working-with-files/_static/image11.jpg)

    Pour traiter les fichiers de charge de l’utilisateur, le code utilise la même technique de base que vous avez utilisé dans l’exemple précédent &#8212; obtenir un fichier à partir de `Request.Files` puis enregistrez-le. (Y compris les différentes opérations que vous devez faire pour obtenir le nom de fichier approprié et d’un chemin d’accès). Les innovations ce stade est que l’utilisateur peut charger plusieurs fichiers et que vous ne savez pas grand nombre. Pour découvrir, vous pouvez obtenir `Request.Files.Count`.

    Avec ce numéro à portée de main, vous pouvez itérer sur `Request.Files`, extraire chaque fichier à son tour, puis enregistrez-le. Lorsque vous souhaitez boucle un nombre déterminé de fois sur une collection, vous pouvez utiliser un `for` boucle, comme suit :

    [!code-csharp[Main](working-with-files/samples/sample10.cs)]

    La variable `i` est simplement un compteur temporaire qui ira de zéro à toute limite supérieure que vous définissez. Dans ce cas, la limite supérieure est le nombre de fichiers. Mais étant donné que le compteur commence à zéro, comme cela est courant pour le comptage des scénarios dans ASP.NET, la limite supérieure est en fait inférieur au nombre de fichiers. (Si trois fichiers sont téléchargés, le nombre est zéro et 2).

    Le `uploadedCount` variable additionne tous les fichiers sont correctement chargés et enregistrés. Ce code compte le risque qu’un fichier attendu n’est peut-être pas pu être chargées.
4. Exécutez la page dans un navigateur. Le navigateur affiche la page et ses boîtes de téléchargement de deux.
5. Sélectionnez deux fichiers à charger.
6. Cliquez sur **ajouter un autre fichier**. La page affiche une nouvelle zone de téléchargement. 

    ![[image]](working-with-files/_static/image12.jpg)
7. Cliquez sur **Upload**.
8. Dans le site Web, cliquez sur le dossier du projet, puis sélectionnez **Actualiser**.
9. Ouvrez le *UploadedFiles* dossier pour afficher les fichiers téléchargés avec succès.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ressources supplémentaires


[Utilisation des Images dans un Site ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202897)

[Exportation vers un fichier CSV](https://msdn.microsoft.com/library/ms155919.aspx)
