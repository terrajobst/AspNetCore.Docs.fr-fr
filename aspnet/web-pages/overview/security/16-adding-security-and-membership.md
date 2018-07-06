---
uid: web-pages/overview/security/16-adding-security-and-membership
title: Ajout de sécurité et l’appartenance à un ASP.NET Web Pages (Razor) Site | Microsoft Docs
author: tfitzmac
description: Ce chapitre vous explique comment sécuriser votre site Web afin que certaines pages sont disponibles uniquement pour les personnes qui se connectent. (Vous allez également apprendre à créer tha pages...
ms.author: aspnetcontent
ms.date: 02/24/2014
ms.assetid: 7a77c2c0-deea-4290-a9c3-97958891758e
msc.legacyurl: /web-pages/overview/security/16-adding-security-and-membership
msc.type: authoredcontent
ms.openlocfilehash: 722d4bc8ab6b7d8eedac87961abdf4b9f900c613
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817316"
---
<a name="adding-security-and-membership-to-an-aspnet-web-pages-razor-site"></a>Ajout de sécurité et l’appartenance à un Site ASP.NET Web Pages (Razor)
====================
par [Tom FitzMacken](https://github.com/tfitzmac)

> Cet article explique comment sécuriser un site Web ASP.NET Web Pages (Razor) afin que certaines pages sont disponibles uniquement pour les personnes qui se connectent. (Vous verrez également comment créer des pages accessibles à toute personne.)
> 
> **Ce que vous allez apprendre :** 
> 
> - Comment créer un site Web qui a une page d’inscription et une page de connexion afin que pour certaines pages, vous pouvez limiter l’accès aux membres uniquement.
> - Comment créer des pages publiques et membres uniquement.
> - Comment définir des rôles, qui sont des groupes qui ont des autorisations de sécurité différentes sur votre site, et comment affecter des utilisateurs à un rôle.
> - Comment utiliser un CAPTCHA pour empêcher les programmes automatisés (robots) à partir de la création de comptes de membre.
>   
> 
> Voici les fonctionnalités d’ASP.NET introduites dans l’article :
> 
> - Le WebMatrix **Starter Site** modèle.
> - Le `WebSecurity` helper et `Roles` classe.
> - Le `ReCaptcha` helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versions des logiciels utilisées dans le didacticiel
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3
> - ASP.NET Web Helpers Library


Vous pouvez configurer votre site Web afin que les utilisateurs peuvent se connecter dedans &#8212; , autrement dit, afin que le site prend en charge *appartenance*. Cela peut être utile pour de nombreuses raisons. Par exemple, votre site peut avoir des pages qui doivent être disponibles uniquement aux membres. Dans certains cas, vous pouvez avoir besoin d’utilisateurs de se connecter afin de vous envoyer des commentaires ou laisser un commentaire.

Même si votre site Web prend en charge l’appartenance, les utilisateurs ne sont pas nécessairement requises pour vous connecter avant d’utiliser certaines des pages sur le site. Les utilisateurs connectés ne sont pas appelés *les utilisateurs anonymes*.

Un utilisateur peut enregistrer sur votre site Web et pouvez ensuite vous connecter au site. Le site Web nécessite un nom d’utilisateur (une adresse de messagerie) et un mot de passe pour confirmer que les utilisateurs sont ceux qu’il prétend être. Ce processus de connexion et de confirmer l’identité d’un utilisateur est appelé *authentification*.

Vous pouvez définir la sécurité et l’appartenance de différentes façons :

- Si vous utilisez WebMatrix, un moyen simple consiste à créer en tant que nouveau site basé sur le **Starter Site** modèle. Ce modèle est déjà configuré pour la sécurité et l’appartenance et a déjà une page d’inscription, une page de connexion et ainsi de suite.

    Le site créé par le modèle possède également une option pour permettre aux utilisateurs de se connecter à l’aide d’un site externe comme Facebook, Google ou Twitter.
- Si vous souhaitez ajouter la sécurité à un site existant, ou si vous ne souhaitez pas utiliser le **Starter Site** modèle, vous pouvez créer votre propre page d’inscription, page de connexion et ainsi de suite.

Cet article se concentre sur la première option &mdash; comment sécuriser à l’aide de la **Starter Site** modèle. Il fournit également des informations de base sur la façon d’implémenter votre propre sécurité et fournit des liens vers plus d’informations sur la marche à suivre. Il existe également des informations sur l’activation des connexions externes, qui est décrit plus en détail dans un article distinct.

## <a name="creating-website-security-using-the-starter-site-template"></a>Création de sécurité des sites Web à l’aide du modèle de Site Starter

Dans WebMatrix, vous pouvez utiliser la **Starter Site** modèle pour créer un site Web qui contient les éléments suivants :

- Une base de données qui est utilisé pour stocker les noms d’utilisateur et mots de passe pour les membres.
- Une page d’inscription qui peuvent d’enregistrer les utilisateurs anonymes (nouveau).
- Une page de connexion et de déconnexion.
- Une page de récupération et de réinitialisation du mot de passe.

La procédure suivante décrit comment créer le site et la configurer.

1. Démarrer WebMatrix et dans le **démarrage rapide** page, sélectionnez **Site à partir du modèle**.
2. Sélectionnez le **Starter Site** modèle, puis cliquez sur **OK**. WebMatrix crée un nouveau site.
3. Dans le volet gauche, cliquez sur le **fichiers** sélecteur d’espace de travail.
4. Dans le dossier racine de votre site Web, ouvrez le  *\_AppStart.cshtml* fichier, qui est un fichier spécial qui est utilisé pour contenir les paramètres globaux. Il contient des instructions commentées à l’aide de la `//` caractères :

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample1.cs)]

    Ces instructions configurer le `WebMail` helper, qui peut être utilisé pour envoyer un courrier électronique. Le système d’appartenance peut utiliser e-mail pour envoyer des messages de confirmation quand les utilisateurs inscrivent ou lorsqu’ils souhaitent modifier leur mot de passe. (Par exemple, une fois que les utilisateurs s’inscrivent, celles-ci reçoivent un e-mail qui inclut un lien sur lequel ils peuvent cliquer pour terminer le processus d’inscription.)

    Envoi de courriers électroniques requiert l’accès à un serveur SMTP, comme décrit dans [Ajout d’un E-mail à un Site ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202899). Vous allez stocker les paramètres de messagerie dans cette central  *\_AppStart.cshtml* de fichiers afin que vous n’êtes pas obligé de les coder de manière répétée dans chaque page qui peut envoyer des e-mails. (Vous n’avez pas besoin de configurer les paramètres SMTP pour configurer une base de données d’inscription ; vous avez besoin de paramètres SMTP uniquement si vous souhaitez valider des utilisateurs à partir de leurs alias de messagerie et de permettre aux utilisateurs de réinitialiser un mot de passe oublié).
5. Supprimez les instructions en supprimant `//` situé devant chacun d’eux.

    Si vous ne souhaitez pas configurer de confirmation par courrier électronique, vous pouvez ignorer cette étape et l’étape suivante. Si les valeurs SMTP ne sont pas définies, le nouveau compte est immédiatement disponible sans un e-mail de confirmation.
6. Modifier les paramètres suivants liés à la messagerie dans le code :

   - Définissez `WebMail.SmtpServer` au nom du serveur SMTP que vous avez accès.
   - Laissez `WebMail.EnableSsl` défini sur `true`. Ce paramètre permet de sécuriser les informations d’identification sont envoyées au serveur SMTP en les chiffrant.
   - Définissez `WebMail.UserName` au nom d’utilisateur pour votre compte de serveur SMTP.
   - Définissez `WebMail.Password` au mot de passe pour votre compte de serveur SMTP.
   - Définissez `WebMail.From` à votre propre adresse e-mail. Il s’agit de l’adresse de messagerie, le message est envoyé à partir de.

     > [!NOTE] 
     > 
     > **Conseil** pour plus d’informations sur les valeurs de ces propriétés, consultez [configuration des paramètres de messagerie](https://go.microsoft.com/fwlink/?LinkID=202906#configuring_email_settings) dans [personnalisation du comportement de l’échelle du Site pour ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkID=202906).
7. Enregistrez et fermez  *\_AppStart.cshtml*.
8. Exécutez le *Default.cshtml* page dans un navigateur.

    ![sécurité-appartenance-2](16-adding-security-and-membership/_static/image1.png)

   > [!NOTE]
   > Si vous voyez une erreur indiquant qu’une propriété doit être une instance de `ExtendedMembershipProvider`, le site ne peut pas être configuré pour utiliser le système d’appartenance ASP.NET Web Pages (SimpleMembership). Cela peut parfois se produire si le serveur d’un fournisseur d’hébergement est configuré différemment de votre serveur local. Pour résoudre ce problème, ajoutez l’élément suivant sur le site *Web.config* fichier :
   > 
   > [!code-xml[Main](16-adding-security-and-membership/samples/sample2.xml)]
   > 
   > Ajoutez cet élément en tant qu’enfant de le `<configuration>` élément et en tant qu’homologue de la `<system.web>` élément.
9. Dans le coin supérieur droit de la page, cliquez sur le **inscrire** lien. Le *Register.cshtml* page s’affiche.
10. Entrez un nom d’utilisateur et le mot de passe, puis activez **inscrire**.

    ![sécurité-appartenance-3](16-adding-security-and-membership/_static/image2.png)

    Lorsque vous avez créé le site Web à partir de la **Starter Site** modèle, une base de données nommée *StarterSite.sdf* a été créé dans le site *application\_données* dossier. Pendant l’inscription, vos informations d’utilisateur sont ajoutées à la base de données. Si vous définissez les valeurs SMTP, un message est envoyé à l’adresse e-mail que vous avez utilisé afin de vous pouvez terminer l’inscription.

    ![sécurité-appartenance-4](16-adding-security-and-membership/_static/image3.png)
11. Accédez à votre programme de messagerie et rechercher le message, ce qui aura votre code de confirmation et un lien hypertexte vers le site.
12. Cliquez sur le lien hypertexte pour activer votre compte. Le lien hypertexte de confirmation s’ouvre une page de confirmation d’inscription.

    ![sécurité-appartenance-5](16-adding-security-and-membership/_static/image4.png)
13. Cliquez sur le **connexion** lien et connectez-vous en utilisant le compte que vous avez inscrite.

      Après vous être connecté, le **connexion** et **inscrire** liens sont remplacés par un **déconnexion** lien. Votre nom de connexion s’affiche sous forme de lien. (Le lien vous permet d’accéder à une page où vous pouvez modifier votre mot de passe.)

      ![sécurité-appartenance-6](16-adding-security-and-membership/_static/image5.png)

      > [!NOTE]
      > Par défaut, ASP.NET web pages envoyer des informations d’identification au serveur en texte clair (en tant que texte lisible). Un site de production doit-elle utiliser le protocole HTTP sécurisé (https://, également connu comme le *SSL* ou SSL) pour chiffrer les informations sensibles qui sont échangées avec le serveur. Vous pouvez e-mail nécessaire messages soient envoyés à l’aide de SSL en définissant `WebMail.EnableSsl=true` comme dans l’exemple précédent. Pour plus d’informations sur le protocole SSL, consultez [sécurisation des Communications Web : certificats SSL et https://](https://go.microsoft.com/fwlink/?LinkId=208660).

## <a name="additional-membership-functionality-in-the-site"></a>Fonctionnalités d’appartenance supplémentaires dans le Site

Votre site contient les autres fonctionnalités qui permet aux utilisateurs de gérer leurs comptes. Les utilisateurs peuvent les opérations suivantes :

- Modifier leurs mots de passe. Une fois connecté, ils peuvent cliquer le nom d’utilisateur (qui est un lien). Cela mène à une page où ils peuvent créer un nouveau mot de passe (*Account/ChangePassword.cshtml*).
- Récupérer un mot de passe oublié. Sur la page de connexion, il existe un lien (**vous avez oublié votre mot de passe ?**) qui redirige les utilisateurs vers une page (*Account/ForgotPassword.cshtml*) dans lequel ils peuvent entrer une adresse de messagerie. Le site leur envoie un message électronique qui a un lien sur lequel ils peuvent cliquer pour définir un nouveau mot de passe (*Account/PasswordReset.cshtml*).

Vous pouvez également laisser les utilisateurs peuvent également se connecter en utilisant un site externe, comme expliqué plus loin.

<a id="Creating_a_Members-Only_Page"></a>
## <a name="creating-a-members-only-page"></a>Création d’une Page réservée aux membres

Pour l’instant, tout le monde peut accéder à une page dans votre site Web. Mais vous souhaiterez peut-être avoir des pages qui sont disponibles uniquement pour les personnes qui se sont connectés (autrement dit, aux membres). ASP.NET vous permet de créer des pages qui sont accessibles que par les membres connecté. En règle générale, si les utilisateurs anonymes essaient d’accéder à une page membres uniquement, vous les rediriger vers la page de connexion.

Dans cette procédure, vous allez créer un dossier qui contiendra les pages qui sont disponibles uniquement pour les utilisateurs connectés.

1. À la racine du site, créez un dossier. (Dans le ruban, cliquez sur la flèche située sous **New** , puis **nouveau dossier**.)
2. Nommez le nouveau dossier *membres*.
3. À l’intérieur de la *membres* dossier, créez une page et l’ai nommée *MembersInformation.cshtml*.
4. Remplacez le contenu existant par le code et le balisage suivant :

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample3.cshtml)]

    Ce code teste le `IsAuthenticated` propriété de la `WebSecurity` object, qui retourne `true` si l’utilisateur s’est connecté. Si l’utilisateur n’est pas connecté dans le code appelle `Response.Redirect` à envoyer à l’utilisateur pour le *Login.cshtml* page dans le *compte* dossier.

    L’URL de la redirection inclut un `returnUrl` interroger la valeur de chaîne qui utilise `Request.Url.LocalPath` pour définir le chemin d’accès de la page actuelle. Si vous définissez la `returnUrl` valeur dans la chaîne de requête comme suit (et si l’URL de retour est un chemin d’accès local), la page de connexion renverra les utilisateurs à cette page une fois, ils se connectent.

    Le code affecte également  *\_SiteLayout.cshtml* page en tant que sa page de disposition. (Pour plus d’informations sur les pages de disposition, consultez [création d’une disposition cohérente dans les Sites ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202891).)
5. Exécutez le site. Si vous êtes toujours connecté, cliquez sur le **déconnexion** bouton en haut de la page.
6. Dans le navigateur, demandez la page */membres/MembersInformation*. Par exemple, l’URL peut ressembler à ceci :

    `http://localhost:38366/Members/MembersInformation`

    (Le numéro de port (38366) sera probablement différent dans votre URL.)

    Vous êtes redirigé vers la *Login.cshtml* page, car vous n’êtes pas connecté.
7. Ouvrez une session en utilisant le compte que vous avez créé précédemment. Vous êtes redirigé vers la *MembersInformation* page. Étant donné que vous êtes connecté, cette fois-ci vous consultez le contenu de la page.

Pour sécuriser l’accès à plusieurs pages, vous pouvez effectuer ceci :

- Ajouter la vérification de sécurité à chaque page.
- Créer un  *\_PageStart.cshtml* page dans le dossier où vous gardez les pages protégées et ajouter la vérification de la sécurité. Le  *\_PageStart.cshtml* page agit comme une sorte de page global pour toutes les pages dans le dossier. Cette technique est expliquée plus en détail dans [personnalisation du comportement de l’échelle du Site pour ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Using__PageStart.cshtml_to_Restrict_Folder_Access).

## <a name="creating-security-for-groups-of-users-roles"></a>Création de sécurité pour les groupes d’utilisateurs (rôles)

Si votre site comporte un grand nombre de membres, il n’est pas efficace de vérifier l’autorisation pour chaque utilisateur individuellement avant de les laisser une page s’affiche. Ce que vous pouvez faire est à la place pour créer des groupes, ou *rôles*, que les membres individuels appartiennent à. Vous pouvez ensuite vérifier les autorisations en fonction du rôle. Dans cette section, vous allez créer un &quot;administrateur&quot; rôle, puis créez une page qui est accessible aux utilisateurs qui sont dans (qui appartient au) ce rôle.

Le système d’appartenance ASP.NET est configuré pour prendre en charge les rôles. Cependant, contrairement à l’inscription de l’appartenance et de connexion, le **Starter Site** modèle ne contient pas les pages qui vous aident à gérer les rôles. (La gestion des rôles sont une tâche administrative, plutôt qu’une tâche utilisateur.) Toutefois, vous pouvez ajouter des groupes directement dans la base de données d’appartenance dans WebMatrix.

1. Dans WebMatrix, cliquez sur le **bases de données** sélecteur d’espace de travail.
2. Dans le volet gauche, ouvrez le *StarterSite.sdf* ouverture d’un nœud, le **Tables** nœud, puis double-cliquez sur le *pages Web\_rôles* table.

    ![sécurité-appartenance-7](16-adding-security-and-membership/_static/image6.png)
3. Ajoutez un rôle nommé &quot;administrateur&quot;. Le *RoleId* est renseigné automatiquement. (Il est la clé primaire et a été défini doit être un champ d’identité, comme expliqué dans [Introduction à l’utilisation avec une base de données dans les Sites ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202893).)
4. Prenez note de ce qui est la valeur pour le *RoleId* champ. (S’il s’agit du premier rôle que vous définissez, il sera 1.)

    ![sécurité-appartenance-8](16-adding-security-and-membership/_static/image7.png)
5. Fermer le *pages Web\_rôles* table.
6. Ouvrez le *UserProfile* table.
7. Prenez note de la *UserId* valeur d’une ou plusieurs des utilisateurs dans la table et puis fermez la table.
8. Ouvrez le *pages Web\_UserInRoles* table et entrez un *UserID* et un *RoleID* valeur dans la table. Par exemple, pour placer l’utilisateur 2 dans la &quot;administrateur&quot; rôle, vous devez entrer ces valeurs :

    ![sécurité-appartenance-9](16-adding-security-and-membership/_static/image8.png)
9. Fermer le *pages Web\_UsersInRoles* table.

    Maintenant que vous avez définis des rôles, vous pouvez configurer une page qui est accessible aux utilisateurs qui se trouvent dans ce rôle.
10. Dans le dossier racine du site Web, créez une page nommée *AdminError.cshtml* et remplacez le contenu existant par le code suivant. Il s’agit de la page utilisateurs sont redirigés vers si elles ne sont pas autorisés à accéder à une page.

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample4.cshtml)]
11. Dans le dossier racine du site Web, créez une page nommée *AdminOnly.cshtml* et remplacez le code existant par le code suivant :

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample5.cshtml)]

    Le `Roles.IsUserInRole` retourne de la méthode `true` si l’utilisateur actuel est membre du rôle spécifié (dans ce cas, le rôle « admin »).
12. Exécutez *Default.cshtml* dans un navigateur, mais ne pas se connecter. (Si vous êtes déjà connecté, déconnectez-vous.)
13. Dans la barre d’adresses du navigateur, ajoutez *AdminOnly* dans l’URL. (En d’autres termes, demander la *AdminOnly.cshtml* fichier.) Vous êtes redirigé vers la *AdminError.cshtml* page, car vous n’êtes pas actuellement connecté en tant qu’utilisateur dans le &quot;administrateur&quot; rôle.
14. Retour à *Default.cshtml* et connectez-vous en tant que l’utilisateur que vous avez ajouté à la &quot;administrateur&quot; rôle.
15. Accédez à *AdminOnly.cshtml* page. Cette fois, vous voyez la page.

## <a name="preventing-automated-programs-from-joining-your-website"></a>Empêche les programmes automatisés d’accéder à votre site Web

La page de connexion s’arrête pas les programmes automatisés (parfois appelé *robots web* ou *robots*) auprès de votre site Web. Cette procédure décrit comment permettre à un test ReCaptcha pour la page d’inscription.

![/Media/38777/ch16securitymembership-18.jpg](16-adding-security-and-membership/_static/image1.jpg)

1. Inscrire votre site Web à ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Lorsque vous avez terminé l’inscription, vous obtiendrez une clé publique et une clé privée.
2. Ajoutez la bibliothèque de programmes d’assistance de Web ASP.NET à votre site Web, comme décrit dans [l’installation des programmes d’assistance dans un Site ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si vous n’avez pas déjà.
3. Dans le *compte* dossier, ouvrez le fichier nommé *Register.cshtml*.
4. Dans le code en haut de la page, recherchez les lignes suivantes et supprimez les marques de commentaire en supprimant les `//` caractères de commentaire :

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample6.cs)]
5. Remplacez `PRIVATE_KEY` par votre propre clé privée ReCaptcha.
6. Dans le balisage de la page, supprimez le `@*` et `*@` caractères de commentaire autour des lignes suivantes dans le balisage de page à partir de :

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample7.cshtml)]
7. Remplacez `PUBLIC_KEY` avec votre clé.
8. Si vous n’avez pas supprimé il déjà, supprimez le `<div>` élément qui contient le texte qui commence par « Pour activer la vérification CAPTCHA... ». (Supprimez la totalité de `<div>` élément et son contenu.)

9. Exécutez *Default.cshtml* dans un navigateur. Si vous êtes connecté au site, cliquez sur le **déconnexion** lien.
10. Cliquez sur le **inscrire** lier et tester l’inscription à l’aide du test CAPTCHA.

     ![sécurité-appartenance-10](16-adding-security-and-membership/_static/image9.png)

Pour plus d’informations sur la `ReCaptcha` helper, consultez [à l’aide un CATPCHA pour empêcher les programmes automatisés (robots) à partir d’à l’aide de votre Site Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251967).

<a id="Additional_Resources"></a>
## <a name="letting-users-log-in-using-an-external-site"></a>Permettant aux utilisateurs d’ouvrir une session à l’aide d’un Site externe

Le **Starter Site** modèle inclut le code et le balisage qui permet aux utilisateurs de se connecter à l’aide de Facebook, Windows Live, Twitter, Google et Yahoo. Par défaut, cette fonctionnalité n’est pas activée. La procédure générale pour l’utilisation de définition par l’utilisateur se connecter en utilisant ces fournisseurs externes s’agit-il :

- Décider lequel vous souhaitez prendre en charge des sites externes.
- Si nécessaire, accédez à ce site et configurer une application de connexion. (Par exemple, vous devez procéder ainsi afin d’autoriser les connexions par Facebook.)
- Dans votre site, configurez le fournisseur. Dans la plupart des cas, il vous suffit de ne pas commenter du code dans le  *\_AppStart.cshtml* fichier.
- Ajoutez le balisage à la page d’inscription permettant aux utilisateurs de lien vers le site externe pour la connexion. Vous pouvez copier généralement le balisage que vous avez besoin et de modifier légèrement le texte.

Vous trouverez des instructions pas à pas dans la rubrique [l’activation de la connexion à partir de Sites externes dans un Site ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251969).

Une fois un utilisateur se connecte à partir d’un autre site, l’utilisateur retourne à votre site et *associe* que la connexion avec votre site. En effet, cela crée une entrée d’appartenance dans votre site pour une connexion externe de l’utilisateur. Cela vous permet d’utiliser les fonctions normales d’appartenance (tels que les rôles) avec la connexion externe.

## <a name="adding-security-to-an-existing-website"></a>Ajout de sécurité à un site Web existant

La procédure décrite précédemment dans cet article s’appuie sur l’utilisation de la **Starter Site** modèle comme base pour la sécurité du site Web. Si elle n’est pas pratique pour vous permettre de démarrer à partir de la **Starter Site** modèle ou pour copier les pages concernées à partir d’un site basé sur ce modèle, vous pouvez implémenter le même type de sécurité dans votre propre site en codant vous-même. Vous créez les mêmes types de pages, l’inscription, connexion et ainsi de suite, puis utilisez les classes et les programmes d’assistance pour configurer l’appartenance.

Le processus de base est décrite dans le billet de blog [la façon la plus simple pour implémenter la sécurité de ASP.NET Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240). La plupart du travail est effectué à l’aide les méthodes et propriétés de suivantes le `WebSecurity` helper :

- [WebSecurty.UserExists](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.userexists(v=vs.99).aspx), [WebSecurity.CreateUserAndAccount](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.createuserandaccount(v=vs.99).aspx). Ces méthodes vous permettent de déterminer si un utilisateur est déjà inscrit et les enregistrer.
- [WebSecurty.IsAuthenticated](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.isauthenticated(v=vs.99).aspx). Cette propriété vous permet de déterminer si l’utilisateur actuel est connecté. Cela est utile pour rediriger les utilisateurs vers une page de connexion si elles ne sont pas connectés.
- [WebSecurity.Login](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.login(v=vs.99).aspx), [WebSecurity.Logout](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.logout(v=vs.99).aspx). Ces méthodes connecter à un utilisateur ou out.
- [WebSecurity.CurrentUserName](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.currentusername(v=vs.99).aspx). Cette propriété est utile pour afficher le nom de connexion de l’utilisateur actuel (si l’utilisateur est connecté).
- [WebSecurity.ConfirmAccount](https://msdn.microsoft.com/library/gg569286(v=vs.99).aspx). Cette méthode est utile si vous configurez l’e-mail de confirmation pour l’inscription. (Détails sont décrits dans le billet de blog [à l’aide de la fonctionnalité de confirmation pour la sécurité d’ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267).)

Pour gérer les rôles, vous pouvez utiliser la [rôles](https://msdn.microsoft.com/library/gg538398(v=vs.99).aspx) et [appartenance](https://msdn.microsoft.com/library/gg569035(v=vs.99).aspx) des classes, comme décrit dans l’entrée de blog.

## <a name="additional-resources"></a>Ressources supplémentaires

- [Personnalisation du comportement à l’échelle du site](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Sécurisation des Communications Web : Certificats SSL et https://](https://go.microsoft.com/fwlink/?LinkId=208660)
- [La façon la plus simple pour implémenter la sécurité de ASP.NET Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240) et [à l’aide de la fonctionnalité de confirmation pour la sécurité d’ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267). Il s’agit des billets de blog qui décrivent comment implémenter des fonctionnalités d’appartenance ASP.NET sans utiliser le **Starter Site** modèle.
- [Activation de la connexion à partir de sites externes dans un site ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251969)
- [Référence de l’API de classe WebSecurity](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity(v=vs.99)) (MSDN)
- [Référence de l’API de classe SimpleRoleProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simpleroleprovider(v=vs.99)) (MSDN)
- [Référence de l’API de classe SimpleMembershipProvider](https://msdn.microsoft.com/library/webmatrix.webdata.simplemembershipprovider(v=vs.99)) (MSDN)
