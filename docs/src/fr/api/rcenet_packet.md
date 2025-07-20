# Documentation de l'API Paquet RCENet

Bienvenue dans la documentation de l'API Paquet RCENet. Cette section fournit un guide approfondi sur la gestion des paquets au sein de RCENet, mettant en évidence les structures clés, les énumérations et les types de rappels essentiels pour la transmission et la réception de paquets.

## Vue d'ensemble

La bibliothèque RCENet offre une approche complète pour la gestion des paquets, mettant l'accent sur la fiabilité, le séquençage et l'efficacité. Ce document explore les composants principaux de la communication par paquets dans RCENet, y compris les drapeaux de paquets, les rappels pour les événements de paquets et la structure `ENetPacket`.

<br /><br />

## Énumérations

### `ENetPacketFlag`

Définit les drapeaux pour contrôler le comportement des paquets dans ENet. Ces drapeaux spécifient des caractéristiques importantes pour la livraison et le traitement des paquets.

- **Drapeaux :**
  - `ENET_PACKET_FLAG_RELIABLE` : Garantit que le paquet doit être reçu par la destination. Des retransmissions sont effectuées jusqu'à ce que le paquet soit livré.
  - `ENET_PACKET_FLAG_UNSEQUENCED` : Indique que le paquet ne sera pas séquencé avec les autres paquets. Non pris en charge pour les paquets fiables.
  - `ENET_PACKET_FLAG_NO_ALLOCATE` : Signale que le paquet n'allouera pas de données ; l'utilisateur doit fournir la mémoire pour les données du paquet.
  - `ENET_PACKET_FLAG_UNRELIABLE_FRAGMENT` : Permet au paquet d'être fragmenté en utilisant des transmissions non fiables si sa taille dépasse l'unité de transmission maximale (MTU).
  - `ENET_PACKET_FLAG_SENT` : Indique si le paquet a été envoyé à partir de toutes les files d'attente dans lesquelles il a été inséré.

```c
typedef enum _ENetPacketFlag
{
   ENET_PACKET_FLAG_RELIABLE          = (1 << 0),
   ENET_PACKET_FLAG_UNSEQUENCED       = (1 << 1),
   ENET_PACKET_FLAG_NO_ALLOCATE       = (1 << 2),
   ENET_PACKET_FLAG_UNRELIABLE_FRAGMENT = (1 << 3),
   ENET_PACKET_FLAG_SENT              = (1 << 8)
} ENetPacketFlag;
```

<br /><br />

## Structures

### `ENetPacket`

Représente un paquet ENet. Ce paquet contient les données à envoyer ou à recevoir sur le réseau.

- **Champs :**
  - `size_t referenceCount` : Compteur de références utilisé en interne par ENet pour la gestion de la mémoire des paquets.
  - `enet_uint32 flags` : Drapeaux du paquet, combinant les valeurs de `ENetPacketFlag` pour spécifier le comportement du paquet.
  - `enet_uint8 * data` : Pointeur vers les données allouées du paquet.
  - `size_t dataLength` : Longueur des données contenues dans le paquet.
  - `ENetPacketFreeCallback freeCallback` : Fonction de rappel appelée lorsque le paquet n'est plus utilisé.
  - `void * userData` : Données utilisateur qui peuvent être librement modifiées par l'application.
  - `enet_uint32 remainingFragments` : Nombre de fragments non confirmés. Lorsque ce nombre atteint 0, `acknowledgeCallback` est déclenché.
  - `ENetPacketAcknowledgedCallback acknowledgeCallback` : Fonction de rappel appelée lorsqu'un paquet fiable a été confirmé par le destinataire.

```c
typedef struct _ENetPacket
{
   size_t                         referenceCount;
   enet_uint32                    flags;
   enet_uint8 *                   data;
   size_t                         dataLength;
   ENetPacketFreeCallback         freeCallback;
   void *                         userData;
   enet_uint32                    remainingFragments;
   ENetPacketAcknowledgedCallback acknowledgeCallback;
} ENetPacket;
```

<br /><br />

## Types de rappels

### `ENetPacketAcknowledgedCallback`

Un type de fonction de rappel appelé lorsqu'un paquet est confirmé par le destinataire. Ce rappel permet d'effectuer des actions spécifiques lors de la confirmation du paquet.

```c
typedef void (ENET_CALLBACK * ENetPacketAcknowledgedCallback) (struct _ENetPacket *);
```

<br /><br />

### `ENetPacketFreeCallback`

Un type de fonction de rappel appelé lorsqu'un paquet est libéré. Ce rappel permet d'effectuer des actions spécifiques lors de la libération de la mémoire d'un paquet, comme la libération des ressources allouées par l'utilisateur.

```c
typedef void (ENET_CALLBACK * ENetPacketFreeCallback) (struct _ENetPacket *);
```

<br /><br />

## Fonctions

### `enet_packet_create`

_Crée un nouveau paquet pour la transmission._

```c
ENET_API ENetPacket * enet_packet_create (const void * data, size_t dataLength, enet_uint32 flags);
```

- **Paramètres :**
  - `data` : Les données à inclure dans le paquet.
  - `dataLength` : La longueur des données en octets.
  - `flags` : Drapeaux du paquet pour contrôler la livraison et le traitement du paquet.

- **Retourne :** Un pointeur vers le nouveau paquet `ENetPacket` créé, ou NULL en cas d'échec.

<br /><br />

### `enet_packet_destroy`

_Détruit un paquet._

```c
ENET_API void enet_packet_destroy (ENetPacket * packet);
```

- **Paramètres :**
  - `packet` : Le paquet à détruire.

<br /><br />

### `enet_packet_resize`

_Redimensionne les données dans un paquet._

```c
ENET_API int enet_packet_resize (ENetPacket * packet, size_t dataLength);
```

- **Paramètres :**
  - `packet` : Le paquet à redimensionner.
  - `dataLength` : La nouvelle taille pour les données du paquet.

- **Retourne :** `0` en cas de succès, `< 0` en cas d'échec.

<br /><br />

### `enet_crc32`

_Calcule le CRC32 (contrôle de redondance cyclique) pour les tampons donnés._

```c
ENET_API enet_uint32 enet_crc32 (const ENetBuffer * buffers, size_t bufferCount);
```

- **Paramètres :**
  - `buffers` : Un tableau de tampons sur lesquels calculer le CRC32.
  - `bufferCount` : Le nombre de tampons dans le tableau.

- **Retourne :** La valeur CRC32 calculée.

<br /><br />

### `enet_packet_get_data`

_Récupère les données contenues dans un paquet._

```c
ENET_API void* enet_packet_get_data(const ENetPacket * packet);
```

- **Paramètres :**
  - `packet` : Le paquet dont récupérer les données.

- **Retourne :** Un pointeur vers les données contenues dans le paquet.

<br /><br />

### `enet_packet_get_user_data`

_Récupère les données définies par l'utilisateur associées à un paquet._

```c
ENET_API void* enet_packet_get_user_data(const ENetPacket * packet);
```

- **Paramètres :**
  - `packet` : Le paquet dont récupérer les données utilisateur.

- **Retourne :** Un pointeur vers les données utilisateur.

<br /><br />

### `enet_packet_set_user_data`

_Associer des données définies par l'utilisateur à un paquet._

```c
ENET_API void enet_packet_set_user_data(ENetPacket * packet, void * userData);
```

- **Paramètres :**
  - `packet` : Le paquet auquel associer les données utilisateur.
  - `userData` : Les données utilisateur à associer au paquet.

<br /><br />

### `enet_packet_get_length`

_Récupère la longueur des données contenues dans un paquet._

```c
ENET_API int enet_packet_get_length(const ENetPacket * packet);
```

- **Paramètres :**
  - `packet` : Le paquet dont récupérer la longueur des données.

- **Retourne :** La longueur des données du paquet.

<br /><br />

### `enet_packet_set_acknowledge_callback`

_Définit une fonction de rappel à appeler lorsque le paquet est confirmé._

```c
ENET_API void enet_packet_set_acknowledge_callback(ENetPacket * packet, ENetPacketAcknowledgedCallback callback);
```

- **Paramètres :**
  - `packet` : Le paquet à associer au rappel de confirmation.
  - `callback` : La fonction de rappel à appeler lors de la confirmation.

<br /><br />

### `enet_packet_set_free_callback`

_Définit une fonction de rappel à appeler lorsque le paquet est libéré._

```c
ENET_API void enet_packet_set_free_callback(ENetPacket * packet, ENetPacketFreeCallback callback);
```

- **Paramètres :**
  - `packet` : Le paquet à associer au rappel de libération.
  - `callback` : La fonction de rappel à appeler lorsque le paquet est libéré.

<br /><br />

### `enet_packet_check_references`

_Vérifie si un paquet est toujours référencé dans ENet._

```c
ENET_API int enet_packet_check_references(const ENetPacket * packet);
```

- **Paramètres :**
  - `packet` : Le paquet à vérifier pour les références.

- **Retourne :** `1` si le paquet est toujours référencé, `0` sinon.

<br /><br />

### `enet_packet_dispose`

_Supprime un paquet et ses ressources associées._

```c
ENET_API void enet_packet_dispose(ENetPacket * packet);
```

- **Paramètres :**
  - `packet` : Le paquet à supprimer.