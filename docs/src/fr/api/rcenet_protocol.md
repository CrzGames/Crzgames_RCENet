# Documentation de l'API Protocole RCENet

Bienvenue dans la documentation de l'API Protocole RCENet. Cette section fournit des informations détaillées sur la fonction de taille des commandes de protocole au sein de la bibliothèque RCENet, essentielle pour la gestion des communications réseau et des structures de paquets de données.

## Vue d'ensemble

La bibliothèque RCENet offre une approche complète pour la communication réseau, encapsulant les détails réseau de bas niveau dans une API de haut niveau. La fonction `enet_protocol_command_size` est un composant crucial, utilisé pour déterminer la taille des différentes commandes de protocole.

<br /><br />

## Fonctions

### `enet_protocol_command_size`

_Détermine la taille d'une commande de protocole donnée._

```c
extern size_t enet_protocol_command_size(enet_uint8 command);
```

- **Paramètres :**
  - `command` : La commande dont la taille doit être déterminée. Cela doit être l'une des valeurs définies dans `ENetProtocolCommand`.
- **Retourne :**
  - La taille de la commande de protocole en octets.

- **Exemple d'utilisation :**

  Cette fonction est généralement utilisée dans les scénarios où la taille d'une commande doit être déterminée dynamiquement, comme lors de l'analyse ou de la construction de paquets réseau.

  ```c
  enet_uint8 command = ENET_PROTOCOL_COMMAND_SEND_RELIABLE;
  size_t commandSize = enet_protocol_command_size(command);

  printf("Taille de la commande SEND_RELIABLE : %zu octets\n", commandSize);
  ```

- **Remarques :**
  - Il est crucial d'utiliser cette fonction pour dimensionner précisément les commandes de protocole, car des tailles incorrectes peuvent entraîner des paquets mal formés et des erreurs de communication.
  - L'énumération `ENetProtocolCommand` contient toutes les commandes prises en charge, chacune pouvant avoir une taille différente.