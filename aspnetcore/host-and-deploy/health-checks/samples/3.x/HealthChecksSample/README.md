# <a name="aspnet-core-health-check-sample"></a>Exemple de contrôle d’intégrité ASP.NET Core

Cet exemple illustre l’utilisation de Health Check Middleware et services. Cet exemple utilise le scénario décrit dans la rubrique [Contrôles d’intégrité dans ASP.NET Core](https://docs.microsoft.com/aspnet/core/host-and-deploy/health-checks).

Pour exécuter l’exemple d’application selon un scénario décrit dans la rubrique, utilisez la commande [dotnet run](https://docs.microsoft.com/dotnet/core/tools/dotnet-run) dans un interpréteur de commandes, à partir du dossier du projet. Passez un commutateur pour le scénario que vous explorez. Par défaut, la configuration de l’application est `basic` lorsqu’aucun commutateur n’est fourni à `dotnet run`.

| Scénario                                               | Commande de l’exemple d’application               | Description |
| ------------------------------------------------------ | -------------------------------- | ----------- |
| Sonde d’intégrité de base (par défaut)                           | `dotnet run --scenario basic`    | Vérifie que l’application peut traiter les requêtes HTTP. |
| Sonde de base de données                                         | `dotnet run --scenario db`       | Vérifie la connexion de base de données SQL Server. Pour obtenir des instructions, consultez la section [Sonde de base de données](https://docs.microsoft.com/aspnet/core/host-and-deploy/health-checks#database-probe) de cette rubrique. |
| Sondes probe readiness et probe liveness                              | `dotnet run --scenario liveness` | Vérifie si une application est en production (*liveness*) ou en préparation à la production (*readiness*). |
| Sonde basée sur les métriques (mémoire)/<br>enregistreur de réponses personnalisé | `dotnet run --scenario writer`   | Vérifie l’utilisation de la mémoire et écrit du code JSON personnalisé lorsque le point de terminaison d’intégrité est vérifié. |
| Filtrer par port                                         | `dotnet run --scenario port`     | Filtre les contrôles d’intégrité selon un port donné. Pour obtenir des instructions, consultez la section [Filtrer par port](https://docs.microsoft.com/aspnet/core/host-and-deploy/health-checks#filter-by-port) de cette rubrique. |

Les scénarios qui impliquent des sondes de base de données et des filtres de port nécessitent une configuration supplémentaire. Pour plus d’informations, consultez la rubrique [Contrôles d’intégrité](https://docs.microsoft.com/aspnet/core/host-and-deploy/health-checks).
