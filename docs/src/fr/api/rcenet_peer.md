# Documentation de l'API Pair RCENet

Bienvenue dans la documentation de l'API Pair RCENet. Cette section offre un guide approfondi sur la gestion des pairs au sein de RCENet, mettant en évidence les fonctions clés, les structures et les énumérations essentielles pour l'interaction et la gestion des pairs.

## Vue d'ensemble

Les pairs représentent les autres points de terminaison dans un environnement réseau avec lesquels un hôte peut communiquer. Ce document couvre les fonctionnalités fournies par RCENet pour gérer ces pairs, y compris l'interrogation de l'état des pairs, la gestion des connexions, et la gestion de la transmission et de la réception des données.

<br /><br />

## Structures

### `ENetPeer`

Représente un pair réseau, un point de terminaison distant avec lequel des paquets de données peuvent être échangés.

- **Champs :**
    - `dispatchList` : Utilisé en interne pour mettre en file d'attente les événements pour ce pair.
    - `host` : L'objet hôte associé à ce pair.
    - `outgoingPeerID`/`incomingPeerID` : Identifiants utilisés pour distinguer ce pair dans les connexions sortantes/entrantes.
    - `connectID` : Un identifiant unique pour la connexion à ce pair.
    - `outgoingSessionID`/`incomingSessionID` : Identifiants de session pour suivre les changements de session.
    - `address` : L'adresse réseau du pair.
    - `data` : Données définies par l'utilisateur associées au pair, peuvent être utilisées pour stocker des informations spécifiques à l'application.
    - `state` : L'état actuel du pair (par exemple, connecté, en cours de déconnexion, etc.).
    - `channels` : Un tableau de canaux sur lesquels les données sont envoyées et reçues. Les canaux fournissent des flux de communication séparés.
    - `channelCount` : Le nombre de canaux alloués pour ce pair.
    - `incomingBandwidth`/`outgoingBandwidth` : Les limites de bande passante entrante/sortante configurées pour ce pair.
    - `incomingBandwidthThrottleEpoch`/`outgoingBandwidthThrottleEpoch` : Utilisés en interne pour gérer la limitation de bande passante.
    - `incomingDataTotal`/`outgoingDataTotal` : La quantité totale de données entrantes/sortantes depuis la dernière réinitialisation ou initialisation.
    - `lastSendTime`/`lastReceiveTime` : Horodatages des dernières opérations d'envoi/réception.
    - `nextTimeout`/`earliestTimeout` : Utilisés en interne pour gérer les délais de connexion et les retransmissions.
    - `packetLossEpoch` : Utilisé en interne pour calculer la perte de paquets au fil du temps.
    - `packetsSent`/`packetsLost` : Compteurs pour le nombre total de paquets envoyés et perdus.
    - `packetLoss`/`packetLossVariance` : Mesures du taux de perte de paquets et de sa variance.
    - `packetThrottle` : La valeur actuelle de limitation des paquets, affectant le taux de transmission des données.
    - `packetThrottleLimit`/`packetThrottleCounter` : Contrôlent le mécanisme de limitation.
    - `packetThrottleEpoch` : Horodatage de la dernière évaluation de la limitation.
    - `packetThrottleAcceleration`/`Deceleration` : Contrôlent la rapidité avec laquelle la valeur de limitation change.
    - `packetThrottleInterval` : Intervalle de temps pour les évaluations de limitation des paquets.
    - `pingInterval` : Fréquence à laquelle les messages ping sont envoyés pour maintenir la connexion active.
    - `timeoutLimit`/`Minimum`/`Maximum` : Configuration des seuils de délai de connexion.
    - `lastRoundTripTime`/`lowestRoundTripTime`/`Variance` : Statistiques sur le temps de trajet aller-retour (RTT) pour les paquets vers ce pair.
    - `roundTripTime`/`roundTripTimeVariance` : RTT moyen et sa variance.
    - `mtu` : Taille de l'unité de transmission maximale, impactant la taille des paquets envoyés.
    - `windowSize` : Taille de la fenêtre de contrôle de flux pour la livraison fiable des paquets.
    - `reliableDataInTransit` : Quantité de données fiables envoyées mais non encore confirmées.
    - `outgoingReliableSequenceNumber` : Numéro de séquence pour le prochain paquet fiable à envoyer.
    - `ENetList acknowledgements` : Une liste d'accusés de réception à envoyer. Cette liste contient des accusés de réception pour les paquets reçus et confirmés par ce pair.
    - `ENetList sentReliableCommands` : Une liste de commandes fiables envoyées mais non encore confirmées. Cela garantit la livraison fiable des paquets.
    - `ENetList outgoingSendReliableCommands` : Une liste de commandes fiables prêtes à être envoyées. Ces commandes sont mises en file d'attente pour un envoi fiable.
    - `ENetList outgoingCommands` : Une liste de commandes prêtes à être envoyées, incluant les commandes fiables et non fiables.
    - `ENetList dispatchedCommands` : Une liste de commandes dispatchées. Cette liste est utilisée pour gérer les commandes qui ont été traitées et attendent une action supplémentaire, comme une confirmation.
    - `enet_uint16 flags` : Drapeaux pour le contrôle du comportement du pair. Inclut des drapeaux comme `ENET_PEER_FLAG_NEEDS_DISPATCH` et `ENET_PEER_FLAG_CONTINUE_SENDING`, qui contrôlent le comportement du pair concernant l'envoi de messages et de paquets.
    - `enet_uint16 reserved` : Un champ réservé pour une utilisation future, assurant la compatibilité avec les futures versions du protocole ou des extensions de la bibliothèque.
    - `enet_uint16 incomingUnsequencedGroup` : Utilisé pour suivre les paquets entrants non séquencés. Ce champ fait partie de la gestion des paquets envoyés sans ordre spécifique.
    - `enet_uint16 outgoingUnsequencedGroup` : Similaire au groupe non séquencé entrant, mais pour les paquets sortants. Il suit le numéro de groupe pour les paquets non séquencés envoyés.
    - `enet_uint32 unsequencedWindow[ENET_PEER_UNSEQUENCED_WINDOW_SIZE / 32]` : Une fenêtre utilisée pour suivre les paquets non séquencés reçus. Ce tableau aide à gérer et à filtrer les paquets non séquencés en double qui peuvent être reçus.
    - `enet_uint32 eventData` : Ce champ est utilisé pour stocker des données d'événement personnalisées qui peuvent être associées à des actions ou déclencheurs spécifiques dans la communication réseau.
    - `size_t totalWaitingData` : Représente la quantité totale de données en attente d'envoi vers ce pair. Cela inclut tous les paquets et commandes en file d'attente qui n'ont pas encore été transmis.

```c
typedef struct _ENetPeer { 
   ENetListNode dispatchList;
   struct _ENetHost * host;
   enet_uint16 outgoingPeerID;
   enet_uint16 incomingPeerID;
   enet_uint32 connectID;
   enet_uint8 outgoingSessionID;
   enet_uint8 incomingSessionID;
   ENetAddress address;
   void * data;
   ENetPeerState state;
   ENetChannel * channels;
   size_t channelCount;
   enet_uint32 incomingBandwidth;
   enet_uint32 outgoingBandwidth;
   enet_uint32 incomingBandwidthThrottleEpoch;
   enet_uint32 outgoingBandwidthThrottleEpoch;
   enet_uint32 incomingDataTotal;
   enet_uint32 outgoingDataTotal;
   enet_uint32 lastSendTime;
   enet_uint32 lastReceiveTime;
   enet_uint32 nextTimeout;
   enet_uint32 earliestTimeout;
   enet_uint32 packetLossEpoch;
   enet_uint32 packetsSent;
   enet_uint32 packetsLost;
   enet_uint32 packetLoss;
   enet_uint32 packetLossVariance;
   enet_uint32 packetThrottle;
   enet_uint32 packetThrottleLimit;
   enet_uint32 packetThrottleCounter;
   enet_uint32 packetThrottleEpoch;
   enet_uint32 packetThrottleAcceleration;
   enet_uint32 packetThrottleDeceleration;
   enet_uint32 packetThrottleInterval;
   enet_uint32 pingInterval;
   enet_uint32 timeoutLimit;
   enet_uint32 timeoutMinimum;
   enet_uint32 timeoutMaximum;
   enet_uint32 lastRoundTripTime;
   enet_uint32 lowestRoundTripTime;
   enet_uint32 lastRoundTripTimeVariance;
   enet_uint32 highestRoundTripTimeVariance;
   enet_uint32 roundTripTime;
   enet_uint32 roundTripTimeVariance;
   enet_uint32 mtu;
   enet_uint32 windowSize;
   enet_uint32 reliableDataInTransit;
   enet_uint16 outgoingReliableSequenceNumber;
   ENetList      acknowledgements;
   ENetList      sentReliableCommands;
   ENetList      outgoingSendReliableCommands;
   ENetList      outgoingCommands;
   ENetList      dispatchedCommands;
   enet_uint16   flags;
   enet_uint16   reserved;
   enet_uint16   incomingUnsequencedGroup;
   enet_uint16   outgoingUnsequencedGroup;
   enet_uint32   unsequencedWindow [ENET_PEER_UNSEQUENCED_WINDOW_SIZE / 32]; 
   enet_uint32   eventData;
   size_t        totalWaitingData;
} ENetPeer;
```

Cette section couvre l'ensemble initial des champs de la structure `ENetPeer`. Les parties suivantes approfondiront les champs restants, expliquant leurs objectifs et comment ils contribuent à la gestion des pairs réseau.

<br /><br />

### `ENetAcknowledgement`

Représente un accusé de réception dans ENet, envoyé pour confirmer la réception d'un paquet.

- **Champs :**
    - `acknowledgementList` : Nœud dans la liste des accusés de réception.
    - `sentTime` : L'heure à laquelle le paquet correspondant a été envoyé.
    - `command` : La commande de protocole envoyée comme accusé de réception.

```c
typedef struct _ENetAcknowledgement {
   ENetListNode acknowledgementList;
   enet_uint32  sentTime;
   ENetProtocol command;
} ENetAcknowledgement;
```

<br /><br />

### `ENetOutgoingCommand`

Représente une commande sortante dans ENet, un paquet ou une action à envoyer à un pair.

- **Champs :**
    - `outgoingCommandList` : Nœud dans la liste des commandes sortantes.
    - `reliableSequenceNumber` : Numéro de séquence fiable pour le suivi des paquets.
    - `unreliableSequenceNumber` : Numéro de séquence non fiable pour le suivi des paquets.
    - `sentTime` : L'heure à laquelle la commande a été envoyée.
    - `roundTripTimeout` : Délai avant de considérer la commande comme perdue.
    - `queueTime` : Temps avant que la commande ne soit mise en file d'attente pour l'envoi.
    - `fragmentOffset` : Décalage du fragment dans le paquet, si la commande est fragmentée.
    - `fragmentLength` : Longueur du fragment de la commande.
    - `sendAttempts` : Nombre de tentatives d'envoi de la commande.
    - `command` : La commande de protocole à envoyer.
    - `packet` : Le paquet associé à la commande, s'il y en a un.

```c
typedef struct _ENetOutgoingCommand {
   ENetListNode outgoingCommandList;
   enet_uint16  reliableSequenceNumber;
   enet_uint16  unreliableSequenceNumber;
   enet_uint32  sentTime;
   enet_uint32  roundTripTimeout;
   enet_uint32  queueTime;
   enet_uint32  fragmentOffset;
   enet_uint16  fragmentLength;
   enet_uint16  sendAttempts;
   ENetProtocol command;
   ENetPacket * packet;
} ENetOutgoingCommand;
```

<br /><br />

### `ENetIncomingCommand`

Représente une commande entrante dans ENet, des données ou actions reçues d'un pair.

- **Champs :**
    - `incomingCommandList` : Nœud dans la liste des commandes entrantes.
    - `reliableSequenceNumber` : Numéro de séquence fiable pour le suivi des paquets.
    - `unreliableSequenceNumber` : Numéro de séquence non fiable pour le suivi des paquets.
    - `command` : La commande de protocole reçue.
    - `fragmentCount` : Nombre total de fragments pour ce paquet.
    - `fragmentsRemaining` : Nombre de fragments restants à recevoir.
    - `fragments` : Champ de bits pour suivre les fragments reçus.
    - `packet` : Le paquet associé à la commande, une fois tous les fragments reçus.

```c
typedef struct _ENetIncomingCommand {
   ENetListNode     incomingCommandList;
   enet_uint16      reliableSequenceNumber;
   enet_uint16      unreliableSequenceNumber;
   ENetProtocol     command;
   enet_uint32      fragmentCount;
   enet_uint32      fragmentsRemaining;
   enet_uint32 *    fragments;
   ENetPacket *     packet;
} ENetIncomingCommand;
```

<br /><br />

## Énumérations

### `_ENetPeerFlag`

Drapeaux contrôlant l'état et le comportement du pair :

- `ENET_PEER_FLAG_NEEDS_DISPATCH` : Indique que le pair a des messages en attente qui doivent être dispatchés.
- `ENET_PEER_FLAG_CONTINUE_SENDING` : Permet au pair de continuer à envoyer des paquets même si la limite de bande passante a été atteinte.

```c
typedef enum _ENetPeerFlag {
   ENET_PEER_FLAG_NEEDS_DISPATCH   = (1 << 0),
   ENET_PEER_FLAG_CONTINUE_SENDING = (1 << 1)
} ENetPeerFlag;
```

<br /><br />

### `ENetPeerState`

Énumère les états possibles d'un pair dans ENet. Ces états représentent le cycle de vie de la connexion d'un pair au réseau ENet.

- **États :**
  - `ENET_PEER_STATE_DISCONNECTED` : Le pair est déconnecté.
  - `ENET_PEER_STATE_CONNECTING` : Le pair est en cours de connexion.
  - `ENET_PEER_STATE_ACKNOWLEDGING_CONNECT` : Le pair accuse réception de la connexion.
  - `ENET_PEER_STATE_CONNECTION_PENDING` : La connexion du pair est en attente.
  - `ENET_PEER_STATE_CONNECTION_SUCCEEDED` : La connexion du pair a réussi.
  - `ENET_PEER_STATE_CONNECTED` : Le pair est connecté.
  - `ENET_PEER_STATE_DISCONNECT_LATER` : Le pair sera déconnecté plus tard.
  - `ENET_PEER_STATE_DISCONNECTING` : Le pair est en cours de déconnexion.
  - `ENET_PEER_STATE_ACKNOWLEDGING_DISCONNECT` : Le pair accuse réception de la déconnexion.
  - `ENET_PEER_STATE_ZOMBIE` : Le pair est dans un état zombie, en attente de nettoyage.

```c
typedef enum _ENetPeerState
{
   ENET_PEER_STATE_DISCONNECTED                = 0,
   ENET_PEER_STATE_CONNECTING                  = 1,
   ENET_PEER_STATE_ACKNOWLEDGING_CONNECT       = 2,
   ENET_PEER_STATE_CONNECTION_PENDING          = 3,
   ENET_PEER_STATE_CONNECTION_SUCCEEDED        = 4,
   ENET_PEER_STATE_CONNECTED                   = 5,
   ENET_PEER_STATE_DISCONNECT_LATER            = 6,
   ENET_PEER_STATE_DISCONNECTING               = 7,
   ENET_PEER_STATE_ACKNOWLEDGING_DISCONNECT    = 8,
   ENET_PEER_STATE_ZOMBIE                      = 9 
} ENetPeerState;
```

<br /><br />

## Fonctions

### Gestion des pairs

### `enet_peer_get_id`

_Récupère l'identifiant unique d'un pair._

```c
ENET_API enet_uint32 enet_peer_get_id(const ENetPeer *peer);
```

- **Paramètres :**
  - `peer` : Pointeur vers le pair dont l'ID est récupéré.
- **Retourne :** L'identifiant unique du pair.

<br /><br />

### `enet_peer_get_ip`

_Récupère l'adresse IP d'un pair._

```c
ENET_API int enet_peer_get_ip(const ENetPeer *peer, char *ip, size_t ipLength);
```

- **Paramètres :**
  - `peer` : Pointeur vers le pair dont l'adresse IP est récupérée.
  - `ip` : Tampon où l'adresse IP sera stockée.
  - `ipLength` : Longueur du tampon `ip`.
- **Retourne :** `0` en cas de succès, `< 0` en cas d'échec.

<br /><br />

### `enet_peer_get_port`

_Récupère le numéro de port d'un pair._

```c
ENET_API enet_uint16 enet_peer_get_port(const ENetPeer *peer);
```

- **Paramètres :**
  - `peer` : Pointeur vers le pair dont le numéro de port est récupéré.
- **Retourne :** Le numéro de port du pair.

<br /><br />

### `enet_peer_get_mtu`

_Récupère la taille de l'unité de transmission maximale (MTU) d'un pair._

```c
ENET_API enet_uint32 enet_peer_get_mtu(const ENetPeer *peer);
```

- **Paramètres :**
  - `peer` : Pointeur vers le pair dont la taille MTU est récupérée.
- **Retourne :** La taille MTU du pair.

<br /><br />

### `enet_peer_get_state`

_Récupère l'état actuel d'un pair._

```c
ENET_API ENetPeerState enet_peer_get_state(const ENetPeer *peer);
```

- **Paramètres :**
  - `peer` : Pointeur vers le pair dont l'état est récupéré.
- **Retourne :** L'état actuel du pair, sous forme d'une valeur de l'énumération `ENetPeerState`.

<br /><br />

### `enet_peer_get_rtt`

_Récupère le temps de trajet aller-retour (RTT) vers un pair._

```c
ENET_API enet_uint32 enet_peer_get_rtt(const ENetPeer *peer);
```

- **Paramètres :**
  - `peer` : Pointeur vers le pair dont le RTT est récupéré.
- **Retourne :** Le RTT vers le pair en millisecondes.

<br /><br />

### `enet_peer_get_last_rtt`

_Récupère le dernier temps de trajet aller-retour (RTT) calculé vers un pair._

```c
ENET_API enet_uint32 enet_peer_get_last_rtt(const ENetPeer *peer);
```

- **Paramètres :**
  - `peer` : Pointeur vers le pair dont le dernier RTT est récupéré.
- **Retourne :** Le dernier RTT calculé vers le pair en millisecondes.

<br /><br />

### `enet_peer_get_lastsendtime`

_Récupère l'heure du dernier envoi d'un paquet à un pair._

```c
ENET_API enet_uint32 enet_peer_get_lastsendtime(const ENetPeer *peer);
```

- **Paramètres :**
  - `peer` : Pointeur vers le pair.
- **Retourne :** L'horodatage du dernier paquet envoyé au pair.

<br /><br />

### `enet_peer_get_lastreceivetime`

_Récupère l'heure de la dernière réception d'un paquet d'un pair._

```c
ENET_API enet_uint32 enet_peer_get_lastreceivetime(const ENetPeer *peer);
```

- **Paramètres :**
  - `peer` : Pointeur vers le pair.
- **Retourne :** L'horodatage du dernier paquet reçu du pair.

<br /><br />

### `enet_peer_get_packets_throttle`

_Récupère la valeur de limitation des paquets pour un pair._

```c
ENET_API float enet_peer_get_packets_throttle(const ENetPeer *peer);
```

- **Paramètres :**
  - `peer` : Pointeur vers le pair dont la valeur de limitation des paquets est récupérée.
- **Retourne :** La valeur de limitation des paquets du pair.

<br /><br />

### `enet_peer_get_data`

_Récupère les données définies par l'utilisateur associées à un pair._

```c
ENET_API void * enet_peer_get_data(const ENetPeer *peer);
```

- **Paramètres :**
  - `peer` : Pointeur vers le pair dont les données utilisateur sont récupérées.
- **Retourne :** Un pointeur vers les données définies par l'utilisateur associées au pair.

<br /><br />

### `enet_peer_set_data`

_Associer des données définies par l'utilisateur à un pair._

```c
ENET_API void enet_peer_set_data(ENetPeer *peer, void *data);
```

- **Paramètres :**
  - `peer` : Pointeur vers le pair auquel associer les données utilisateur.
  - `data` : Pointeur vers les données définies par l'utilisateur à associer au pair.

<br /><br />

### Transmission et réception de données

### `enet_peer_send`

_Envoie un paquet à un pair spécifique._

```c
ENET_API int enet_peer_send(ENetPeer *peer, enet_uint8 channelID, ENetPacket *packet);
```

- **Paramètres :**
  - `peer` : Le pair cible auquel le paquet sera envoyé.
  - `channelID` : L'ID du canal sur lequel envoyer le paquet.
  - `packet` : Le paquet à envoyer.
- **Retourne :** `0` en cas de succès, `< 0` en cas d'échec.

<br /><br />

### `enet_peer_receive`

_Reçoit le prochain paquet d'un pair spécifique, s'il est disponible._

```c
ENET_API ENetPacket *enet_peer_receive(ENetPeer *peer, enet_uint8 *channelID);
```

- **Paramètres :**
  - `peer` : Le pair dont recevoir le paquet.
  - `channelID` : Un pointeur vers un `enet_uint8` où l'ID du canal du paquet reçu sera stocké.
- **Retourne :** Un pointeur vers le `ENetPacket` reçu, ou `NULL` si aucun paquet n'est disponible.

<br /><br />

### `enet_peer_ping`

_Envoie un ping à un pair._

```c
ENET_API void enet_peer_ping(ENetPeer *peer);
```

- **Paramètres :**
  - `peer` : Le pair à pinguer.
- **Remarques :** Cette fonction peut être utilisée pour mesurer le temps de trajet aller-retour ou pour maintenir la connexion active.

<br /><br />

### `enet_peer_ping_interval`

_Définit l'intervalle auquel les pings sont envoyés automatiquement à un pair._

```c
ENET_API void enet_peer_ping_interval(ENetPeer *peer, enet_uint32 interval);
```

- **Paramètres :**
  - `peer` : Le pair pour lequel définir l'intervalle de ping.
  - `interval` : L'intervalle de ping en millisecondes.
- **Remarques :** Utile pour configurer le comportement de maintien de la connexion.

<br /><br />

### `enet_peer_timeout`

_Définit les limites de délai pour un pair._

```c
ENET_API void enet_peer_timeout(ENetPeer *peer, enet_uint32 timeoutLimit, enet_uint32 timeoutMinimum, enet_uint32 timeoutMaximum);
```

- **Paramètres :**
  - `peer` : Le pair pour lequel définir les limites de délai.
  - `timeoutLimit` : La limite après laquelle la connexion est considérée comme expirée.
  - `timeoutMinimum` : La limite minimale de délai.
  - `timeoutMaximum` : La limite maximale de délai.

<br /><br />

### `enet_peer_reset`

_Réinitialise un pair à son état initial._

```c
ENET_API void enet_peer_reset(ENetPeer *peer);
```

- **Paramètres :**
  - `peer` : Le pair à réinitialiser.
- **Remarques :** Cette fonction est généralement utilisée pour réutiliser rapidement un objet pair pour une nouvelle connexion.

<br /><br />

### `enet_peer_disconnect`

_Initie une déconnexion gracieuse d'un pair._

```c
ENET_API void enet_peer_disconnect(ENetPeer *peer, enet_uint32 data);
```

- **Paramètres :**
  - `peer` : Le pair à déconnecter.
  - `data` : Une valeur de données spécifiée par l'utilisateur, optionnelle, qui peut être récupérée par le pair déconnecté.

<br /><br />

### `enet_peer_disconnect_now`

_Déconnecte immédiatement un pair sans attendre la fin des transmissions en attente._

```c
ENET_API void enet_peer_disconnect_now(ENetPeer *peer, enet_uint32 data);
```

- **Paramètres :**
  - `peer` : Le pair à déconnecter immédiatement.
  - `data` : Une valeur de données spécifiée par l'utilisateur, optionnelle, qui peut être récupérée par le pair déconnecté.

<br /><br />

### `enet_peer_disconnect_later`

_Programme une déconnexion d'un pair après que tous les paquets envoyés ont été livrés._

```c
ENET_API void enet_peer_disconnect_later(ENetPeer *peer, enet_uint32 data);
```

- **Paramètres :**
  - `peer` : Le pair à déconnecter plus tard.
  - `data` : Une valeur de données spécifiée par l'utilisateur, optionnelle, qui peut être récupérée par le pair déconnecté.

<br /><br />

### `enet_peer_throttle_configure`

_Configure les paramètres de limitation des paquets pour un pair._

```c
ENET_API void enet_peer_throttle_configure(ENetPeer *peer, enet_uint32 interval, enet_uint32 acceleration, enet_uint32 deceleration);
```

- **Paramètres :**
  - `peer` : Le pair pour lequel configurer les paramètres de limitation.
  - `interval` : La fenêtre temporelle sur laquelle les conditions de limitation sont mesurées.
  - `acceleration` : Le taux auquel la valeur de limitation des paquets est augmentée lorsque les conditions le permettent.
  - `deceleration` : Le taux auquel la valeur de limitation des paquets est diminuée lorsque les conditions l'exigent.

<br /><br />

### `enet_peer_reset_queues`

_Réinitialise les files d'attente de paquets pour un pair._

```c
extern void enet_peer_reset_queues(ENetPeer *peer);
```

- **Description** : Cette fonction vide toutes les files d'attente de paquets associées au pair, réinitialisant efficacement son état de communication.
- **Paramètres :**
  - `peer` : Pointeur vers le pair dont les files d'attente de paquets doivent être réinitialisées.

<br /><br />

### `enet_peer_has_outgoing_commands`

_Vérifie s'il y a des commandes sortantes qui doivent être envoyées à un pair._

```c
extern int enet_peer_has_outgoing_commands(ENetPeer *peer);
```

- **Description** : Détermine s'il y a des commandes dans la file d'attente sortante qui n'ont pas encore été envoyées au pair. Utile pour décider s'il faut vider l'hôte ou continuer à accumuler des commandes.
- **Paramètres :**
  - `peer` : Pointeur vers le pair vérifié pour les commandes sortantes.
- **Retourne :** `1` s'il y a des commandes sortantes, `0` sinon.

<br /><br />

### `enet_peer_setup_outgoing_command`

_Prépare une commande sortante à envoyer à un pair._

```c
extern void enet_peer_setup_outgoing_command (ENetPeer *peer, ENetOutgoingCommand *command);
```

- **Paramètres :**
  - `peer` : Le pair auquel la commande doit être envoyée.
  - `command` : La commande sortante à configurer.

- **Exemple d'utilisation :**
  ```c
  ENetOutgoingCommand command;
  // Initialiser la commande...
  enet_peer_setup_outgoing_command(peer, &command);
  ```

<br /><br />

### `enet_peer_queue_outgoing_command`

_Met en file d'attente une commande sortante pour la transmission à un pair._

```c
extern ENetOutgoingCommand * enet_peer_queue_outgoing_command (ENetPeer *peer, const ENetProtocol *protocol, ENetPacket *packet, enet_uint32 offset, enet_uint16 length);
```

- **Paramètres :**
  - `peer` : Le pair auquel la commande sera envoyée.
  - `protocol` : La commande de protocole à envoyer.
  - `packet` : Le paquet à envoyer avec la commande.
  - `offset` : Le décalage dans le paquet où la commande commence.
  - `length` : La longueur des données de la commande.

- **Exemple d'utilisation :**
  ```c
  ENetPacket *packet = enet_packet_create(data, length, ENET_PACKET_FLAG_RELIABLE);
  enet_peer_queue_outgoing_command(peer, &protocol, packet, 0, packet->dataLength);
  ```

<br /><br />

### `enet_peer_queue_incoming_command`

_Met en file d'attente une commande entrante reçue d'un pair._

```c
extern ENetIncomingCommand * enet_peer_queue_incoming_command (ENetPeer *peer, const ENetProtocol *protocol, const void *data, size_t dataLength, enet_uint32 flags, enet_uint32 fragmentCount);
```

- **Paramètres :**
  - `peer` : Le pair dont la commande a été reçue.
  - `protocol` : La commande de protocole reçue.
  - `data` : Les données de la commande reçue.
  - `dataLength` : La longueur des données de la commande.
  - `flags` : Les drapeaux associés à la commande entrante.
  - `fragmentCount` : Le nombre de fragments pour la commande.

- **Exemple d'utilisation :**
  ```c
  enet_peer_queue_incoming_command(peer, &protocol, receivedData, dataLength, flags, fragmentCount);
  ```

<br /><br />

### `enet_peer_queue_acknowledgement`

_Met en file d'attente un accusé de réception pour une commande reçue._

```c
extern ENetAcknowledgement * enet_peer_queue_acknowledgement (ENetPeer *peer, const ENetProtocol *protocol, enet_uint16 sentTime);
```

- **Paramètres :**
  - `peer` : Le pair pour lequel l'accusé de réception est mis en file d'attente.
  - `protocol` : La commande de protocole accusée de réception.
  - `sentTime` : L'heure à laquelle la commande a été envoyée.

- **Exemple d'utilisation :**
  ```c
  enet_peer_queue_acknowledgement(peer, &protocol, sentTime);
  ```

<br /><br />

### `enet_peer_dispatch_incoming_unreliable_commands`

_Dispatche toutes les commandes entrantes non fiables pour un pair._

```c
extern void enet_peer_dispatch_incoming_unreliable_commands (ENetPeer *peer, ENetChannel *channel, ENetIncomingCommand *command);
```

- **Paramètres :**
  - `peer` : Le pair pour lequel les commandes sont dispatchées.
  - `channel` : Le canal sur lequel les commandes ont été reçues.
  - `command` : La première commande dans la file d'attente des commandes entrantes non fiables.

- **Exemple d'utilisation :**
  ```c
  enet_peer_dispatch_incoming_unreliable_commands(peer, &channel, &command);
  ```

<br /><br />

### `enet_peer_dispatch_incoming_reliable_commands`

_Dispatche toutes les commandes entrantes fiables pour un pair._

```c
extern void enet_peer_dispatch_incoming_reliable_commands (ENetPeer *peer, ENetChannel *channel, ENetIncomingCommand *command);
```

- **Paramètres :**
  - `peer` : Le pair pour lequel les commandes sont dispatchées.
  - `channel` : Le canal sur lequel les commandes ont été reçues.
  - `command` : La première commande dans la file d'attente des commandes entrantes fiables.

- **Exemple d'utilisation :**
  ```c
  enet_peer_dispatch_incoming_reliable_commands(peer, &channel, &command);
  ```

<br /><br />

### `enet_peer_on_connect`

_Gère l'événement de connexion d'un pair._

```c
extern void enet_peer_on_connect(ENetPeer *peer);
```

- **Description** : Cette fonction est appelée lorsqu'un pair se connecte avec succès. Elle est responsable de l'initialisation de tout état spécifique à la nouvelle connexion.
- **Paramètres :**
  - `peer` : Pointeur vers le pair qui vient de se connecter.

<br /><br />

### `enet_peer_on_disconnect`

_Gère l'événement de déconnexion d'un pair._

```c
extern void enet_peer_on_disconnect(ENetPeer *peer);
```

- **Description** : Invoquée lorsqu'un pair se déconnecte ou est déconnecté. Elle nettoie l'état du pair et le prépare pour une éventuelle réutilisation.
- **Paramètres :**
  - `peer` : Pointeur vers le pair en cours de déconnexion.