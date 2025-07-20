# Documentation de l'API Socket RCENet

Bienvenue dans la documentation de l'API Socket RCENet. Cette section offre un guide approfondi sur la gestion des sockets au sein de RCENet, mettant en évidence les fonctions clés et les structures pour la création, la gestion et l'utilisation des sockets pour la communication réseau.

## Vue d'ensemble

La bibliothèque RCENet abstrait les complexités de la programmation des sockets, offrant une interface simplifiée pour la communication réseau. Ce document explore en détail l'`ENetSocket`, un aspect fondamental de la communication réseau, couvrant leur création, les options et leur utilisation au sein de la bibliothèque RCENet.

<br /><br />

## Énumérations

### `ENetSocketType`

Définit le comportement et les caractéristiques des sockets utilisés dans les communications réseau.

- **Types :**
  - `ENET_SOCKET_TYPE_STREAM` : Socket de flux (TCP), fournissant des transmissions fiables, séquencées et orientées connexion.
  - `ENET_SOCKET_TYPE_DATAGRAM` : Socket de datagramme (UDP), utilisé pour des transmissions sans connexion.

```c
typedef enum _ENetSocketType
{
   ENET_SOCKET_TYPE_STREAM   = 1,
   ENET_SOCKET_TYPE_DATAGRAM = 2
} ENetSocketType;
```

<br /><br />

### `ENetSocketWait`

Spécifie les conditions qu'un socket doit attendre pendant les opérations de sélection.

- **Conditions :**
  - `ENET_SOCKET_WAIT_NONE` : Aucune condition d'attente.
  - `ENET_SOCKET_WAIT_SEND` : Attendre que le socket soit prêt à envoyer des données.
  - `ENET_SOCKET_WAIT_RECEIVE` : Attendre que le socket soit prêt à recevoir des données.
  - `ENET_SOCKET_WAIT_INTERRUPT` : Attendre une interruption.

```c
typedef enum _ENetSocketWait
{
   ENET_SOCKET_WAIT_NONE      = 0,
   ENET_SOCKET_WAIT_SEND      = (1 << 0),
   ENET_SOCKET_WAIT_RECEIVE   = (1 << 1),
   ENET_SOCKET_WAIT_INTERRUPT = (1 << 2)
} ENetSocketWait;
```

<br /><br />

### `ENetSocketOption`

Options pour configurer le comportement des sockets au niveau du système d'exploitation.

- **Options :**
  - `ENET_SOCKOPT_NONBLOCK` : Active le mode non bloquant pour le socket.
  - `ENET_SOCKOPT_BROADCAST` : Active l'envoi de paquets de diffusion.
  - `ENET_SOCKOPT_RCVBUF` : Taille du tampon de réception.
  - `ENET_SOCKOPT_SNDBUF` : Taille du tampon d'envoi.
  - `ENET_SOCKOPT_REUSEADDR` : Permet la réutilisation des adresses locales.
  - `ENET_SOCKOPT_RCVTIMEO` : Délai pour la réception.
  - `ENET_SOCKOPT_SNDTIMEO` : Délai pour l'envoi.
  - `ENET_SOCKOPT_ERROR` : Obtenir l'erreur en attente sur le socket.
  - `ENET_SOCKOPT_NODELAY` : Désactive l'algorithme de Nagle (pour les sockets TCP).
  - `ENET_SOCKOPT_TTL` : Définit le TTL pour les paquets IP.
  - `ENET_SOCKOPT_IPV6ONLY` : Socket uniquement IPv6, sans support pour les adresses IPv4 mappées.

```c
typedef enum _ENetSocketOption
{
   ENET_SOCKOPT_NONBLOCK  = 1,
   ENET_SOCKOPT_BROADCAST = 2,
   ENET_SOCKOPT_RCVBUF    = 3,
   ENET_SOCKOPT_SNDBUF    = 4,
   ENET_SOCKOPT_REUSEADDR = 5,
   ENET_SOCKOPT_RCVTIMEO  = 6,
   ENET_SOCKOPT_SNDTIMEO  = 7,
   ENET_SOCKOPT_ERROR     = 8,
   ENET_SOCKOPT_NODELAY   = 9,
   ENET_SOCKOPT_TTL       = 10,
   ENET_SOCKOPT_IPV6ONLY  = 11
} ENetSocketOption;
```

<br /><br />

### `ENetSocketShutdown`

Modes pour les opérations de fermeture de socket.

- **Modes :**
  - `ENET_SOCKET_SHUTDOWN_READ` : Ferme le côté réception du socket.
  - `ENET_SOCKET_SHUTDOWN_WRITE` : Ferme le côté envoi du socket.
  - `ENET_SOCKET_SHUTDOWN_READ_WRITE` : Ferme les côtés envoi et réception.

```c
typedef enum _ENetSocketShutdown
{
    ENET_SOCKET_SHUTDOWN_READ       = 0,
    ENET_SOCKET_SHUTDOWN_WRITE      = 1,
    ENET_SOCKET_SHUTDOWN_READ_WRITE = 2
} ENetSocketShutdown;
```

<br /><br />

## Fonctions

Cette section fournit un aperçu détaillé des fonctions liées aux sockets dans la bibliothèque RCENet, permettant la création, la gestion et l'utilisation des sockets réseau pour les communications UDP et TCP.

### `enet_socket_create`

Crée un socket du type spécifié.

- **Prototype** :
  ```c
  ENET_API ENetSocket enet_socket_create(ENetAddressType addressType, ENetSocketType socketType);
  ```
- **Paramètres :**
  - `addressType` : Le type d'adresse (IPv4, IPv6) que le socket utilisera.
  - `socketType` : Le type de socket à créer (STREAM pour TCP, DATAGRAM pour UDP).
- **Retourne** : Un handle vers le socket nouvellement créé, ou `ENET_SOCKET_NULL` en cas d'échec.

<br /><br />

### `enet_socket_bind`

Lie un socket à une adresse locale.

- **Prototype** :
  ```c
  ENET_API int enet_socket_bind(ENetSocket socket, const ENetAddress *address);
  ```
- **Paramètres :**
  - `socket` : Le socket à lier.
  - `address` : L'adresse locale à laquelle lier le socket.
- **Retourne** : `0` en cas de succès, `< 0` en cas d'échec.

<br /><br />

### `enet_socket_listen`

Écoute les connexions entrantes sur un socket.

- **Prototype** :
  ```c
  ENET_API int enet_socket_listen(ENetSocket socket, int backlog);
  ```
- **Paramètres :**
  - `socket` : Le socket sur lequel écouter.
  - `backlog` : La longueur maximale de la file d'attente des connexions en attente.
- **Retourne** : `0` en cas de succès, `< 0` en cas d'échec.

<br /><br />

### Transmission de données

### `enet_socket_send`

Envoie des données à une destination spécifiée.

- **Prototype** :
  ```c
  ENET_API int enet_socket_send(ENetSocket socket, const ENetAddress *destination, const ENetBuffer *buffers, size_t bufferCount);
  ```
- **Paramètres :**
  - `socket` : Le socket à partir duquel les données sont envoyées.
  - `destination` : L'adresse du destinataire.
  - `buffers` : Un tableau de tampons de données à envoyer.
  - `bufferCount` : Le nombre de tampons de données.
- **Retourne** : Le nombre total d'octets envoyés, ou `< 0` en cas d'échec.

<br /><br />

### `enet_socket_receive`

Reçoit des données d'un socket.

- **Prototype** :
  ```c
  ENET_API int enet_socket_receive(ENetSocket socket, ENetAddress *sender, ENetBuffer *buffers, size_t bufferCount);
  ```
- **Paramètres :**
  - `socket` : Le socket sur lequel les données sont reçues.
  - `sender` : L'adresse de l'expéditeur.
  - `buffers` : Un tableau de tampons pour stocker les données reçues.
  - `bufferCount` : Le nombre de tampons de données.
- **Retourne** : Le nombre total d'octets reçus, ou `< 0` en cas d'échec.

<br /><br />

### Configuration des sockets

### `enet_socket_set_option`

Définit une option sur un socket.

- **Prototype** :
  ```c
  ENET_API int enet_socket_set_option(ENetSocket socket, ENetSocketOption option, int value);
  ```
- **Paramètres :**
  - `socket` : Le socket à configurer.
  - `option` : L'option spécifique à définir.
  - `value` : La valeur à définir pour l'option.
- **Retourne** : `0` en cas de succès, `< 0` en cas d'échec.

<br /><br />

### Fermeture et destruction des sockets

### `enet_socket_shutdown`

Ferme une partie d'une connexion bidirectionnelle.

- **Prototype** :
  ```c
  ENET_API int enet_socket_shutdown(ENetSocket socket, ENetSocketShutdown how);
  ```
- **Paramètres :**
  - `socket` : Le socket à fermer.
  - `how` : La méthode de fermeture (lecture, écriture, ou les deux).
- **Retourne** : `0` en cas de succès, `< 0` en cas d'échec.

<br /><br />

### `enet_socket_destroy`

Ferme et détruit un socket.

- **Prototype** :
  ```c
  ENET_API void enet_socket_destroy(ENetSocket socket);
  ```
- **Paramètres :**
  - `socket` : Le socket à détruire.

<br /><br />

## Conclusion

L'API Socket RCENet simplifie la programmation réseau en fournissant une interface de haut niveau pour les opérations sur les sockets, permettant aux développeurs de se concentrer sur la logique principale de leurs applications réseau. Pour plus d'informations ou d'assistance, veuillez consulter la documentation officielle de RCENet ou le guide de programmation ENet.