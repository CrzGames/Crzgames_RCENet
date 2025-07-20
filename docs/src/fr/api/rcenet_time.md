# Documentation de l'API Temps RCENet

Bienvenue dans la documentation de l'API Temps RCENet. Cette section fournit des informations détaillées sur la gestion du temps au sein de la bibliothèque RCENet, mettant en évidence les fonctions liées à la mesure et à la gestion du temps.

## Vue d'ensemble

La gestion du temps est cruciale dans les applications réseau pour des tâches telles que la mesure des temps de trajet aller-retour, la mise en œuvre de délais d'expiration et la planification d'événements. RCENet fournit un ensemble de fonctions simples mais efficaces pour travailler avec le temps de manière multiplateforme.

<br /><br />

## Fonctions

### `enet_time_get`

_Retourne le temps actuel en millisecondes._

Cette fonction retourne le temps actuel sous forme de nombre de millisecondes. La valeur initiale n'est pas spécifiée sauf si définie par `enet_time_set`. Elle peut être utilisée pour des opérations de chronométrage, la mise en œuvre de délais d'expiration ou la mesure d'intervalles.

```c
ENET_API enet_uint32 enet_time_get(void);
```

- **Retourne :** Le temps actuel en millisecondes.

<br /><br />

### `enet_time_set`

_Définit le temps actuel en millisecondes._

Cette fonction permet de définir manuellement le temps actuel, ce qui peut être utile dans les scénarios où une gestion du temps synchronisée est nécessaire entre différents composants d'une application.

```c
ENET_API void enet_time_set(enet_uint32 time);
```

- **Paramètres :**
  - `time` : Le nouveau temps actuel en millisecondes.

<br /><br />

## Conclusion

L'API Temps RCENet fournit des fonctionnalités essentielles pour gérer les tâches liées au temps dans les applications réseau, offrant des capacités de récupération et de définition du temps. Une gestion appropriée du temps est essentielle pour assurer une communication efficace et une exécution opportune des opérations réseau.