# Documentation de l'API Générale RCENet

Bienvenue dans la documentation de l'API Générale RCENet. Cette section fournit des informations détaillées sur les fonctions générales disponibles dans RCENet pour l'initialisation et la gestion de l'état global de la bibliothèque, ainsi que pour l'interrogation des informations de version.

## Vue d'ensemble

Avant d'utiliser l'une des fonctions ou fonctionnalités fournies par RCENet pour la communication réseau, la bibliothèque doit être correctement initialisée. Cette documentation couvre les étapes nécessaires pour initialiser et désinitialiser RCENet, ainsi que la manière de remplacer les rappels par défaut et de vérifier la version de la bibliothèque.

<br /><br />

## Fonctions

### Initialisation et Désinitialisation
<br />

#### `enet_initialize`
_Initialise ENet globalement. Doit être appelé avant d'utiliser toute fonction dans ENet._

- **Prototype** :
  ```c
  ENET_API int enet_initialize(void);
  ```

- **Paramètres** : Aucun.

- **Retourne** : `0` en cas de succès, `< 0` en cas d'échec. Indique si ENet a été initialisé avec succès.

<br /><br />

#### `enet_initialize_with_callbacks`
_Initialise ENet globalement avec des rappels définis par l'utilisateur. Cette fonction doit être appelée à la place de `enet_initialize()` si des rappels personnalisés sont nécessaires._

- **Prototype** :
  ```c
  ENET_API int enet_initialize_with_callbacks(ENetVersion version, const ENetCallbacks *inits);
  ```

- **Paramètres** :
  - `version` : La constante `ENET_VERSION` doit être fournie pour garantir la compatibilité.
  - `inits` : Un pointeur vers une structure `ENetCallbacks` qui permet à l'utilisateur de remplacer certaines fonctions internes. Les rappels `NULL` utiliseront les valeurs par défaut d'ENet.

- **Retourne** : `0` en cas de succès, `< 0` en cas d'échec. Permet une initialisation plus granulaire en fonction des besoins de l'utilisateur.

<br /><br />

#### `enet_deinitialize`
_Ferme ENet globalement. Doit être appelé lorsque le programme qui a initialisé ENet se termine._

- **Prototype** :
  ```c
  ENET_API void enet_deinitialize(void);
  ```

- **Paramètres** : Aucun.

- **Action** : Nettoie les ressources globales d'ENet. Il est crucial d'appeler cette fonction pour éviter les fuites de ressources.

<br /><br />

### Informations sur la version
<br />

#### `enet_linked_version`
_Retourne le numéro de version de la bibliothèque ENet liée._

- **Prototype** :
  ```c
  ENET_API ENetVersion enet_linked_version(void);
  ```

- **Paramètres** : Aucun.

- **Retourne** : Le numéro de version de la bibliothèque ENet. Cela peut être utile pour le débogage ou pour garantir la compatibilité avec certaines versions de la bibliothèque.