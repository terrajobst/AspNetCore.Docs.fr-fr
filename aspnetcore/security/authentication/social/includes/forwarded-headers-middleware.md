## <a name="forward-request-information-with-a-proxy-or-load-balancer"></a>Avant de demander des informations avec un proxy ou de l’équilibreur de charge

Si l’application est déployée derrière un serveur proxy ou un équilibreur de charge, certaines informations de demande d’origine peuvent être transféré vers l’application dans les en-têtes de demande. Ces informations comprennent généralement le schéma de demande sécurisé (`https`), hôte et adresse IP du client. Les applications ne lisent pas automatiquement ces en-têtes de demande pour découvrir et d’utiliser les informations de demande d’origine.

Le schéma est utilisé dans la génération de lien qui affecte le flux d’authentification auprès de fournisseurs externes. Perdre le schéma sécurisé (`https`) entraîne l’application de génération des URL de redirection non sécurisé incorrect.

Utilisez intergiciel des en-têtes transférés pour rendre les informations de demande d’origine disponible pour l’application pour traiter une demande.

Pour plus d'informations, consultez <xref:host-and-deploy/proxy-load-balancer>.
