Le modèle par défaut crée des liens et des pages **RazorPagesMovie**, **Home**, **About** et **Contact**. En fonction de la taille de votre fenêtre de navigateur, vous devrez peut-être cliquer sur l’icône de navigation pour afficher les liens.

![Page d’accueil ou d’index](~/tutorials/razor-pages/razor-pages-start/_static/home2.png)

Testez les liens. Les liens **RazorPagesMovie** et **Home** permettent d’accéder à la page d’index. Les liens **About** et **Contact** permettent d’accéder respectivement aux pages `About` et `Contact`.

## <a name="project-files-and-folders"></a>Dossiers et fichiers projet

Le tableau suivant répertorie les fichiers et dossiers du projet. Pour ce didacticiel, le fichier *Startup.cs* est le plus important à comprendre. Vous n’avez pas besoin de passer en revue chaque lien ci-dessous. Les liens sont fournis en guise de référence quand vous avez besoin d’informations supplémentaires sur un fichier ou un dossier du projet.

| Fichier ou dossier | Objectif |
| -------------- | ------- |
| *wwwroot* | Contient des ressources statiques. Consultez [Fichiers statiques](xref:fundamentals/static-files). |
| *Pages* | Dossier pour [Pages Razor](xref:razor-pages/index). |
| *appsettings.json* | [Configuration](xref:fundamentals/configuration/index) |
| *Program.cs* | Configure [l’hôte](xref:fundamentals/host/index) de l’application ASP.NET Core. |
| *Startup.cs* | Configure les services et le pipeline de requête. Voir [Démarrage](xref:fundamentals/startup). |

### <a name="the-pagesshared-folder"></a>Dossier Pages/Shared

Le fichier *_Layout.cshtml* contient des éléments HTML communs (liens de script et feuille de style) et définit la disposition de l’application. Par exemple, quand vous sélectionnez **RazorPagesMovie**, **Home**, **About** ou **Contact**, un ensemble commun d’éléments s’affiche dans la page web. Les éléments communs sont notamment le menu de navigation en haut et l’en-tête en bas de la fenêtre. Pour plus d’informations, consultez [Disposition](xref:mvc/views/layout).

Le fichier *_ValidationScriptsPartial.cshtml* fournit une référence aux scripts de validation [jQuery](https://jquery.com/). Quand nous ajoutons les pages `Create` et `Edit` plus loin dans ce tutoriel, le fichier *_ValidationScriptsPartial.cshtml* est utilisé.

Le fichier *_CookieConsentPartial.cshtml* fournit une barre de navigation et du contenu pour récapituler la politique de confidentialité et d’utilisation des cookies. Pour plus d’informations sur les ressources RGPD incluses dans le projet, consultez [Prise en charge du règlement général sur la protection des données (RGPD) dans ASP.NET Core](xref:security/gdpr).

### <a name="the-pages-folder"></a>Le dossier Pages

Le fichier *_ViewStart.cshtml* définit la propriété `Layout` des pages Razor pour qu’elle utilise le fichier *_Layout.cshtml*. Pour plus d’informations, consultez [Disposition](xref:mvc/views/layout).

Le fichier *_ViewImports.cshtml* contient des directives Razor qui sont importées dans chaque page Razor. Pour plus d’informations, consultez [Importation de directives partagées](xref:mvc/views/layout#importing-shared-directives).

Les pages `About`, `Contact` et `Index` sont des pages de base que vous pouvez utiliser pour démarrer une application. La page `Error` sert à afficher des informations d’erreur.
