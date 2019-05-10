# <a name="gdpr-sample"></a>Exemple de RGPD

* Dans *appsettings.json*, affectez la valeur `CheckNotConsentNeeded` à `false` pour demander le consentement ; sinon la valeur est true, ou omettre. Testez l’application avec `CheckNotConsentNeeded` définie sur `false` et la valeur `true`.
* Créer des cookies essentielles et les utilisations non vitales avec chaque variation de `CheckConsentNeeded` et consentement accordé.
* Inscrire un utilisateur.
* Supprimer les cookies.
