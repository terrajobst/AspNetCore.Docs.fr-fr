> Certaines commandes ne sont pas pris en charge si l’application utilise SQLite comme magasin de données d’identité. En raison des limitations dans le moteur de base de données, `Alter` commandes lèvent l’exception suivante :
>
> « System.NotSupportedException : SQLite ne prend pas en charge cette opération de migration. » 
>
> Pour contourner ce problème, exécutez les migrations Code First sur la base de données pour modifier les tables.
