# Documentation de l'API Hôte RCENet

Bienvenue dans la documentation de l'API Hôte RCENet. Cette section fournit un guide approfondi sur la gestion des hôtes ENet pour la communication réseau, couvrant les fonctionnalités clés et les structures nécessaires à la création, à la gestion et à l'interaction avec les hôtes et les pairs réseau au sein du cadre RCENet.

## Vue d'ensemble
La bibliothèque RCENet offre une abstraction de haut niveau pour la communication réseau, simplifiant considérablement le processus de création et de gestion des connexions réseau. Les hôtes dans RCENet servent de nœuds centraux pour la communication réseau, gérant les connexions entrantes et sortantes, la transmission de données et la gestion de la bande passante. Ce document explore la structure `ENetHost`, détaillant son importance dans la communication réseau et les différentes fonctions disponibles pour une gestion efficace de l’hôte.

<br /><br />

## Énumérations

### `ENetEventType`

Définit les types d’événements dans ENet. Ces événements sont retournés par `enet_host_service()` et décrivent diverses actions ou changements d’état pouvant survenir sur un hôte ou un pair.

- **Types :**
  - `ENET_EVENT_TYPE_NONE` : Aucun événement n’est survenu dans la limite de temps spécifiée.
  - `ENET_EVENT_TYPE_CONNECT` : Un pair s’est connecté avec succès.
  - `ENET_EVENT_TYPE_DISCONNECT` : Un pair s’est déconnecté.
  - `ENET_EVENT_TYPE_RECEIVE` : Un paquet a été reçu d’un pair.
  - `ENET_EVENT_TYPE_DISCONNECT_TIMEOUT` : Un pair s’est déconnecté en raison d’un délai d’attente.

```c
typedef enum _ENetEventType
{
   ENET_EVENT_TYPE_NONE              = 0,
   ENET_EVENT_TYPE_CONNECT           = 1,
   ENET_EVENT_TYPE_DISCONNECT        = 2,
   ENET_EVENT_TYPE_RECEIVE           = 3,
   ENET_EVENT_TYPE_DISCONNECT_TIMEOUT= 4
} ENetEventType;
```

<br /><br />

## Structures

### `ENetEvent`

Représente un événement ENet tel que retourné par `enet_host_service()`. Cette structure contient toutes les informations nécessaires pour traiter l'événement, y compris le type d'événement, le pair concerné, et toutes données ou paquets associés.

- **Champs :**
  - `ENetEventType type` : Le type de l'événement.
  - `ENetPeer* peer` : Le pair qui a généré un événement de connexion, de déconnexion ou de réception. Null pour les événements `ENET_EVENT_TYPE_NONE`.
  - `enet_uint8 channelID` : Le canal sur le pair qui a généré l'événement, si applicable. Pertinent pour les événements `ENET_EVENT_TYPE_RECEIVE`.
  - `enet_uint32 data` : Données associées à l'événement, si applicable. Pour jauets `ENET_EVENT_TYPE_DISCONNECT` et `ENET_EVENT_TYPE_DISCONNECT_TIMEOUT`, cela peut contenir des données fournies par l'utilisateur décrivant la déconnexion.
  - `ENetPacket* packet` : Le paquet associé à l'événement, si applicable. Pertinent pour les événements `ENET_EVENT_TYPE_RECEIVE`. Le paquet doit être détruit avec `enet_packet_destroy` après utilisation.

```c
typedef struct _ENetEvent {
   ENetEventType        type;
   ENetPeer *           peer;
   enet_uint8           channelID;
   enet_uint32          data;
   ENetPacket *         packet;
} ENetEvent;
```

<br /><br />

### `ENetHost`

La structure `ENetHost` est définie comme suit :

- **Champs :**
  - `ENetSocket socket` : La socket UDP utilisée pour les communications.
  - `ENetAddress address` : L'adresse de l'hôte.
  - `enet_uint32 incomingBandwidth` : La limite de bande passante entrante en octets/seconde.
  - `enet_uint32 outgoingBandwidth` : La limite de bande passante sortante en octets/seconde.
  - `enet_uint32 bandwidthThrottleEpoch` : Utilisé en interne pour gérer la limitation de bande passante.
  - `enet_uint32 mtu` : La taille de l'unité de transmission maximale.
  - `enet_uint32 randomSeed` : Utilisé en interne pour la génération de nombres aléatoires.
  - `int recalculateBandwidthLimits` : Indicateur pour recalculer les limites de bande passante.
  - `ENetPeer *peers` : Tableau des pairs connectés à cet hôte.
  - `size_t peerCount` : Le nombre de pairs.
  - `size_t channelLimit` : Le nombre maximum de canaux autorisés par connexion.
  - `enet_uint32 serviceTime` : Dernier temps de service.
  - `ENetList dispatchQueue` : File d'attente des événements à dispatcher.
  - `enet_uint32 totalQueued` : Nombre total de paquets en attente.
  - `size_t packetSize` : Taille du dernier paquet envoyé ou reçu.
  - `enet_uint16 headerFlags` : Drapeaux d'en-tête du protocole.
  - `ENetProtocol commands[]` : Tableau des commandes de protocole.
  - `size_t commandCount` : Nombre de commandes dans le tableau.
  - `ENetBuffer buffers[]` : Tampons pour les données sortantes.
  - `size_t bufferCount` : Nombre de tampons utilisés.
  - `ENetChecksumCallback checksum` : Fonction de rappel pour le calcul de la somme de contrôle.
  - `ENetCompressor compressor` : Rappels pour la compression de données.
  - `enet_uint8 packetData[][]` : Tableaux de données de paquets pour l'envoi et la réception.
  - `ENetAddress receivedAddress` : Adresse du dernier paquet reçu.
  - `enet_uint8 *receivedData` : Données du dernier paquet reçu.
  - `size_t receivedDataLength` : Longueur des données du dernier paquet reçu.
  - `enet_uint32 totalSentData` : Total des données envoyées.
  - `enet_uint32 totalSentPackets` : Total des paquets envoyés.
  - `enet_uint32 totalReceivedData` : Total des données reçues.
  - `enet_uint32 totalReceivedPackets` : Total des paquets reçus.
  - `ENetInterceptCallback intercept` : Rappel pour l'interception de paquets.
  - `size_t connectedPeers` : Nombre de pairs connectés.
  - `size_t bandwidthLimitedPeers` : Pairs avec limitation de bande passante.
  - `size_t duplicatePeers` : Pairs avec IP en double autorisés.
  - `size_t maximumPacketSize` : Taille maximale des paquets.
  - `size_t maximumWaitingData` : Données en attente maximales.
  - `ENetEncryptor encryptor` : Rappels pour le chiffrement des paquets.

```c
typedef struct _ENetHost {
   ENetSocket socket;
   ENetAddress address;
   enet_uint32 incomingBandwidth;
   enet_uint32 outgoingBandwidth;
   enet_uint32 bandwidthThrottleEpoch;
   enet_uint32 mtu;
   enet_uint32 randomSeed;
   int recalculateBandwidthLimits;
   ENetPeer *peers;
   size_t peerCount;
   size_t channelLimit;
   enet_uint32 serviceTime;
   ENetList dispatchQueue;
   enet_uint32 totalQueued;
   size_t packetSize;
   enet_uint16 headerFlags;
   ENetProtocol commands[ENET_PROTOCOL_MAXIMUM_PACKET_COMMANDS];
   size_t commandCount;
   ENetBuffer buffers[ENET_BUFFER_MAXIMUM];
   size_t bufferCount;
   ENetChecksumCallback checksum;
   ENetCompressor compressor;
   enet_uint8 packetData[2][ENET_PROTOCOL_MAXIMUM_MTU];
   ENetAddress receivedAddress;
   enet_uint8 *receivedData;
   size_t receivedDataLength;
   enet_uint32 totalSentData;
   enet_uint32 totalSentPackets;
   enet_uint32 totalReceivedData;
   enet_uint32 totalReceivedPackets;
   ENetInterceptCallback intercept;
   size_t connectedPeers;
   size_t bandwidthLimitedPeers;
   size_t duplicatePeers;
   size_t maximumPacketSize;
   size_t maximumWaitingData;
   ENetEncryptor encryptor;
} ENetHost;
```

<br /><br />

## Compression et Chiffrement

### `ENetCompressor`

Une structure pour la compression des paquets avant leur transmission ou réception via la socket UDP.

- **Champs :**
  - `void* context` : Données de contexte pour le compresseur. Doit être non-NULL.
  - `compress` : Fonction pour compresser les données. Prend un tableau de `ENetBuffer` en entrée, compresse les données dans `outData`, et produit au plus `outLimit` octets. Doit retourner 0 en cas d'échec.
  - `decompress` : Fonction pour décompresser les données. Prend `inData`, décompresse les données dans `outData`, et produit au plus `outLimit` octets. Doit retourner 0 en cas d'échec.
  - `destroy` : Fonction appelée lorsque la compression est désactivée ou que l'hôte est détruit. Peut être NULL.

```c
typedef struct _ENetCompressor
{
   void * context;
   size_t (ENET_CALLBACK * compress) (void * context, const ENetBuffer * inBuffers, size_t inBufferCount, size_t inLimit, enet_uint8 * outData, size_t outLimit);
   size_t (ENET_CALLBACK * decompress) (void * context, const enet_uint8 * inData, size_t inLimit, enet_uint8 * outData, size_t outLimit);
   void (ENET_CALLBACK * destroy) (void * context);
} ENetCompressor;
```

<br /><br />

### `ENetEncryptor`

Une structure pour le chiffrement des paquets avant leur transmission ou réception, effectué avant la compression.

- **Champs :**
  - `void* context` : Données de contexte pour le chiffreur. Doit être non-NULL.
  - `encrypt` : Fonction pour chiffrer les données. Prend un tableau de `ENetBuffer` en entrée, chiffre les données dans `outData`, et produit au plus `outLimit` octets. Doit retourner 0 en cas d'échec.
  - `decrypt` : Fonction pour déchiffrer les paquets reçus d'un pair (peut être NULL pour un paquet de connexion), à partir de `inData`, déchiffre les données dans `outData`, et produit au plus `outLimit` octets. Doit retourner 0 en cas d'échec.
  - `destroy` : Fonction appelée lorsque le chiffrement est désactivé ou que l'hôte est détruit. Peut être NULL.

```c
typedef struct _ENetEncryptor
{
   void * context;
   size_t (ENET_CALLBACK * encrypt) (void * context, ENetPeer * peer, const ENetBuffer * inBuffers, size_t inBufferCount, size_t inLimit, enet_uint8 * outData, size_t outLimit);
   size_t (ENET_CALLBACK * decrypt) (void * context, ENetPeer * peer, const enet_uint8 * inData, size_t inLimit, enet_uint8 * outData, size_t outLimit);
   void (ENET_CALLBACK * destroy) (void * context);
} ENetEncryptor;
```

<br /><br />

## Rappels

### `ENetChecksumCallback`

Rappel pour calculer la somme de contrôle des données contenues dans les tampons. Cette fonction est utilisée pour vérifier l'intégrité des données transmises.

```c
typedef enet_uint32 (ENET_CALLBACK * ENetChecksumCallback) (const ENetBuffer * buffers, size_t bufferCount);
```

<br /><br />

### `ENetInterceptCallback`

Rappel pour intercepter les paquets UDP bruts reçus. Cette fonction permet d'inspecter, de modifier ou de rejeter les paquets avant qu'ils ne soient traités par ENet.

```c
typedef int (ENET_CALLBACK * ENetInterceptCallback) (struct _ENetHost * host, struct _ENetEvent * event);
```

<br /><br />

## Fonctions

### Gestion des hôtes

### `enet_host_create`

_Crée un hôte ENet pour communiquer sur le réseau. L'hôte peut agir en tant que serveur, client, ou les deux._

```c
ENET_API ENetHost * enet_host_create (ENetAddressType type, const ENetAddress *address, size_t peerCount, size_t channelLimit, enet_uint32 incomingBandwidth, enet_uint32 outgoingBandwidth);
```

- **Paramètres :**
  - `type` : Le type d'adresse (IPv4, IPv6) que l'hôte utilisera.
  - `address` : L'adresse locale à laquelle lier l'hôte. Utilisez NULL pour une adresse non spécifiée.
  - `peerCount` : Le nombre maximum de pairs que l'hôte doit supporter. Le maximum est de 65535 clients, peut être utilisé avec la macro : `ENET_PROTOCOL_MAXIMUM_PEER_ID`.
  - `channelLimit` : Le nombre maximum de canaux autorisés ; si 0, cela équivaut à `ENET_PROTOCOL_MAXIMUM_CHANNEL_COUNT` (255).
  - `incomingBandwidth` : La bande passante entrante de l'hôte en octets/seconde. Si 0, ENet suppose une bande passante illimitée.
  - `outgoingBandwidth` : La bande passante sortante de l'hôte en octets/seconde. Si 0, ENet suppose une bande passante illimitée.

- **Retourne :** Un pointeur vers le nouvel hôte `ENetHost` créé, ou NULL en cas d'échec.

<br /><br />

### `enet_host_destroy`

_Détruit l'hôte spécifié, déconnectant tous les pairs connectés._

```c
ENET_API void enet_host_destroy (ENetHost *host);
```

- **Paramètres :**
  - `host` : L'hôte à détruire.

<br /><br />

### `enet_host_connect`

_Initie une connexion à un hôte étranger._

```c
ENET_API ENetPeer * enet_host_connect (ENetHost *host, const ENetAddress *address, size_t channelCount, enet_uint32 data);
```

- **Paramètres :**
  - `host` : L'hôte local tentant d'initier la connexion.
  - `address` : L'adresse de l'hôte étranger auquel se connecter.
  - `channelCount` : Le nombre de canaux à allouer pour la connexion.
  - `data` : Données fournies par l'utilisateur pour la tentative de connexion.

- **Retourne :** Un pointeur vers un `ENetPeer` représentant la connexion, ou NULL en cas d'échec.

<br /><br />

### `enet_host_service`

_Dispatche les événements en attente et envoie ou reçoit des paquets._

```c
ENET_API int enet_host_service (ENetHost *host, ENetEvent *event, enet_uint32 timeout);
```

- **Paramètres :**
  - `host` : L'hôte à traiter.
  - `event` : Pointeur vers une structure `ENetEvent` où l'événement (s'il y en a) sera placé.
  - `timeout` : Le nombre de millisecondes à attendre pour un événement. Une valeur de 0 retournera immédiatement.

- **Retourne :** > 0 si un événement a été dispatché, 0 si aucun événement n'est survenu, < 0 en cas d'échec.

<br /><br />

### `enet_host_flush`

_Envoie immédiatement tous les paquets en attente sur l'hôte._

```c
ENET_API void enet_host_flush (ENetHost *host);
```

- **Paramètres :**
  - `host` : L'hôte dont la file d'attente de paquets sera vidée.

<br /><br />

### `enet_host_broadcast`

_Diffuse un paquet à tous les pairs connectés._

```c
ENET_API void enet_host_broadcast (ENetHost *host, enet_uint8 channelID, ENetPacket *packet);
```

- **Paramètres :**
  - `host` : L'hôte à partir duquel le paquet sera diffusé.
  - `channelID` : L'ID du canal sur lequel le paquet sera envoyé.
  - `packet` : Le paquet à diffuser.

<br /><br />

## Chiffrement

### `enet_host_encrypt`

_Définit le chiffreur de paquets que l'hôte doit utiliser pour chiffrer et déchiffrer les paquets._

```c
void enet_host_encrypt(ENetHost * host, const ENetEncryptor * encryptor);
```

- **Paramètres :**
  - `host` : L'hôte pour lequel activer ou désactiver le chiffrement.
  - `encryptor` : Rappels pour le chiffreur de paquets ; si NULL, le chiffrement est désactivé.

- **Remarques :**
  - L'activation du chiffrement active le protocole étendu RCENet et rompt la compatibilité avec le protocole ENet standard. Il est crucial de s'assurer que le chiffrement est activé ou désactivé de manière cohérente sur tous les hôtes communicants dans votre application pour maintenir la compatibilité du protocole.

<br /><br />

## Compression

### `enet_host_compress`

_Active la compression des paquets en utilisant les rappels de compression spécifiés._

```c
ENET_API void enet_host_compress (ENetHost *host, const ENetCompressor *compressor);
```

- **Paramètres :**
  - `host` : L'hôte pour lequel activer la compression.
  - `compressor` : Un pointeur vers les rappels de compression à utiliser.

<br /><br />

### `enet_host_compress_with_range_coder`

_Active la compression par codage par plages pour l'hôte spécifié._

```c
ENET_API int enet_host_compress_with_range_coder(ENetHost * host);
```

- **Paramètres :**
  - `host` : L'hôte pour lequel activer la compression par codage par plages.

- **Retourne :**
  - `0` en cas de succès, indiquant que la compression par codage par plages a été activée.
  - `< 0` en cas d'échec, indiquant que la compression par codage par plages n'a pas pu être activée.

- **Remarques :**
  - Le codage par plages est une forme de codage entropique utilisé dans la compression. Activer cette option peut améliorer les taux de compression des données pour certains types de données, améliorant potentiellement l'efficacité du réseau.

<br /><br />

### `enet_host_bandwidth_throttle`

_Ajuste la limitation de bande passante pour l'hôte, recalculant les tailles de fenêtre des pairs en fonction des conditions actuelles._

```c
extern void enet_host_bandwidth_throttle(ENetHost * host);
```

- **Paramètres :**
  - `host` : L'hôte dont la limitation de bande passante sera ajustée.

- **Remarques :**
  - Cette fonction est généralement appelée pour ajuster dynamiquement l'utilisation de la bande passante en fonction des conditions réseau actuelles. Elle recalcule la taille de la fenêtre pour chaque pair connecté, augmentant ou diminuant potentiellement le débit auquel l'hôte envoie des données pour gérer la congestion et maintenir une utilisation efficace de la bande passante disponible.

<br /><br />

### `enet_host_bandwidth_limit`

_Limite la bande passante entrante et sortante de l'hôte._

```c
ENET_API void enet_host_bandwidth_limit (ENetHost *host, enet_uint32 incomingBandwidth, enet_uint32 outgoingBandwidth);
```

- **Paramètres :**
  - `host` : L'hôte pour lequel définir les limites de bande passante.
  - `incomingBandwidth` : La bande passante entrante maximale en octets/seconde.
  - `outgoingBandwidth` : La bande passante sortante maximale en octets/seconde.

<br /><br />

### `enet_host_channel_limit`

_Limite le nombre maximum de canaux autorisés pour les connexions futures._

```c
ENET_API void enet_host_channel_limit (ENetHost *host, size_t channelLimit);
```

- **Paramètres :**
  - `host` : L'hôte pour lequel définir la limite de canaux.
  - `channelLimit` : Le nombre maximum de canaux.

<br /><br />

## Statistiques et Configuration

### `enet_host_get_peers_count`

_Retourne le nombre de pairs connectés._

```c
ENET_API enet_uint32 enet_host_get_peers_count(const ENetHost *host);
```

<br /><br />

### `enet_host_get_packets_sent`

_Retourne le nombre de paquets envoyés par l'hôte._

```c
ENET_API enet_uint32 enet_host_get_packets_sent(const ENetHost *host);
```

<br /><br />

### `enet_host_get_packets_received`

_Retourne le nombre de paquets reçus par l'hôte._

```c
ENET_API enet_uint32 enet_host_get_packets_received(const ENetHost *host);
```

<br /><br />

### `enet_host_get_bytes_sent`

_Retourne le nombre total d'octets envoyés par l'hôte._

```c
ENET_API enet_uint32 enet_host_get_bytes_sent(const ENetHost *host);
```

<br /><br />

### `enet_host_get_bytes_received`

_Retourne le nombre total d'octets reçus par l'hôte._

```c
ENET_API enet_uint32 enet_host_get_bytes_received(const ENetHost *host);
```

<br /><br />

### `enet_host_set_max_duplicate_peers`

_Définit le nombre maximum de pairs autorisés avec la même adresse IP._

```c
ENET_API void enet_host_set_max_duplicate_peers(ENetHost *host, enet_uint16 number);
```

- **Paramètres :**
  - `host` : L'hôte pour lequel définir le nombre maximum de pairs en double.
  - `number` : Le nombre maximum de pairs en double autorisés.

<br /><br />

### `enet_host_set_intercept_callback`

_Définit une fonction de rappel pour intercepter les paquets reçus._

```c
ENET_API void enet_host_set_intercept_callback(ENetHost *host, ENetInterceptCallback callback);
```

- **Paramètres :**
  - `host` : L'hôte pour lequel définir le rappel d'interception.
  - `callback` : La fonction de rappel à utiliser pour intercepter les paquets.

<br /><br />

### `enet_host_set_checksum_callback`

_Définit une fonction de rappel pour calculer les sommes de contrôle des paquets._

```c
ENET_API void enet_host_set_checksum_callback(ENetHost *host, ENetChecksumCallback callback);
```

- **Paramètres :**
  - `host` : L'hôte pour lequel définir le rappel de somme de contrôle.
  - `callback` : La fonction de rappel à utiliser pour calculer les sommes de contrôle des paquets.

<br /><br />

### `enet_host_random_seed`

_Génère une graine aléatoire pour initialiser le générateur de nombres aléatoires._

```c
extern enet_uint32 enet_host_random_seed(void);
```

- **Retourne :** Une valeur `enet_uint32` représentant la graine aléatoire.

Cette fonction est généralement appelée au démarrage d'une application pour s'assurer que les appels ultérieurs aux fonctions de génération de nombres aléatoires dans RCENet produisent des séquences de nombres uniques basées sur cette graine initiale.

<br /><br />

### `enet_host_random`

_Génère un nombre aléatoire en utilisant le générateur de nombres aléatoires de l'hôte._

```c
extern enet_uint32 enet_host_random(ENetHost *host);
```

- **Paramètres :**
  - `host` : Pointeur vers l'instance `ENetHost`.
  
- **Retourne :** Un nombre aléatoire `enet_uint32`.

Cette fonction exploite le générateur de nombres aléatoires interne d'un `ENetHost` donné pour produire un nombre aléatoire. Elle peut être utilisée à diverses fins dans l'application réseau, comme générer des identifiants uniques ou sélectionner des éléments aléatoires dans un ensemble.