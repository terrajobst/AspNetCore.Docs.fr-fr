# <a name="gdpr-sample"></a>Exemple de PIBR

* Dans *appsettings.json*, définissez `CheckNotConsentNeeded` à `false` pour demander le consentement ; sinon la valeur true ou omettre. Tester l’application avec `CheckNotConsentNeeded` la valeur `false` et la valeur `true`.
* Créer des cookies essentielles et les utilisations non vitales avec chaque variation de `CheckConsentNeeded` et l’autorisation accordée.
* Inscrire un utilisateur.
* Supprimer les cookies.
