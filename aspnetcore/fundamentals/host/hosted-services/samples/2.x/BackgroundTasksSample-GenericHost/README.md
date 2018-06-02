# <a name="aspnet-background-tasks-sample-generic-host"></a>Exemples de tâches d’arrière-plan ASP.NET (hôte générique)

Cet exemple montre l’utilisation de [IHostedService](https://docs.microsoft.com/dotnet/api/microsoft.extensions.hosting.ihostedservice). Cet exemple montre les fonctionnalités décrites dans la rubrique [Tâches d’arrière-plan avec des services hébergés dans ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services).

Lors de l’exemple de l’exécution [Visual Studio Code](https://code.visualstudio.com/), définissez la valeur **console** de la configuration de la console dans *.vscode/launch.json* sur `externalTerminal` ou `integratedTerminal`. L’utilisation de la `internalConsole` n’est pas compatible avec l’entrée de la séquence de touches de la console que l’application utilise pour mettre en file d’attente des éléments de travail en arrière-plan.
