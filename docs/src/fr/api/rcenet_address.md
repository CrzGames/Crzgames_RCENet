# Documentation de l’API RCENet Address

Bienvenue dans la documentation de l’API RCENet Address. Cette section fournit un guide complet pour gérer les adresses réseau avec RCENet, couvrant les fonctions et structures essentielles pour la manipulation et l’interaction des adresses IP au sein de la bibliothèque RCENet.

## Vue d’ensemble

La bibliothèque RCENet simplifie la programmation réseau en abstrahant des opérations réseau complexes, y compris la gestion des adresses. Ce document se concentre sur la structure `ENetAddress` et les fonctions associées, qui jouent un rôle clé dans la communication réseau en représentant les points de terminaison réseau.

<br /><br />

## Structure

### `ENetAddress`

Représente un point de terminaison réseau avec une adresse IP et un numéro de port.

- **Champs :**
  - `ENetAddressType type` : Le type d’adresse (IPv4, IPv6).
  - `enet_uint16 port` : Le numéro de port en ordre d’octet hôte.
  - `union host` : Adresse IP en ordre d’octet réseau. Utilisez `v4` pour IPv4 ou `v6` pour IPv6.

```c
typedef struct _ENetAddress {
   ENetAddressType type;
   enet_uint16 port;
   union {
       enet_uint8 v4[4];
       enet_uint16 v6[8];
   } host;
} ENetAddress;
```

<br /><br />

## Fonctions

### Fonctions de comparaison et de validation
<br />

#### `enet_address_equal_host`
Compare la partie hôte de deux adresses pour déterminer si elles sont égales.

- **Prototype** :
  ```c
  ENET_API int enet_address_equal_host(const ENetAddress * firstAddress, const ENetAddress * secondAddress);
  ```

- **Paramètres :**
  - `firstAddress` : Pointeur vers la première adresse à comparer.
  - `secondAddress` : Pointeur vers la seconde adresse à comparer.

- **Retourne** : `1` si les parties hôte sont égales, `0` sinon.

<br /><br />

#### `enet_address_equal`
Compare deux adresses, y compris leurs ports, pour déterminer si elles sont égales.

- **Prototype** :
  ```c
  ENET_API int enet_address_equal(const ENetAddress * firstAddress, const ENetAddress * secondAddress);
  ```

- **Paramètres :**
  - `firstAddress` : Pointeur vers la première adresse à comparer.
  - `secondAddress` : Pointeur vers la seconde adresse à comparer.

- **Retourne** : `1` si les parties hôte et les ports sont égaux, `0` sinon.

<br /><br />

#### `enet_address_is_any`
Vérifie si une adresse est l’adresse spéciale « any », qui écoute sur toutes les interfaces.

- **Prototype** :
  ```c
  ENET_API int enet_address_is_any(const ENetAddress * address);
  ```

- **Paramètres :**
  - `address` : Pointeur vers l’adresse à vérifier.

- **Retourne** : `1` si l’adresse est l’adresse « any », `0` sinon.

<br /><br />

#### `enet_address_is_broadcast`
Détermine si une adresse est une adresse de broadcast.

- **Prototype** :
  ```c
  ENET_API int enet_address_is_broadcast(const ENetAddress * address);
  ```

- **Paramètres :**
  - `address` : Pointeur vers l’adresse à vérifier.

- **Retourne** : `1` si l’adresse est une adresse de broadcast, `0` sinon.

<br /><br />

#### `enet_address_is_loopback`
Vérifie si une adresse est une adresse de loopback.

- **Prototype** :
  ```c
  ENET_API int enet_address_is_loopback(const ENetAddress * address);
  ```

- **Paramètres :**
  - `address` : Pointeur vers l’adresse à vérifier.

- **Retourne** : `1` si l’adresse est une adresse de loopback, `0` sinon.

<br /><br />

### Fonctions de résolution et de conversion
<br />

#### `enet_address_set_host_ip`
Analyse une adresse IP sous forme de chaîne et configure l’objet adresse.

- **Prototype** :
  ```c
  ENET_API int enet_address_set_host_ip(ENetAddress * address, const char * hostName);
  ```

- **Paramètres :**
  - `address` : Pointeur vers l’objet adresse à configurer.
  - `hostName` : Adresse IP au format chaîne.

- **Retourne** : `0` en cas de succès, `< 0` en cas d’échec.

<br /><br />

#### `enet_address_set_host`
Résout un nom d’hôte en adresse IP et le stocke dans un objet adresse.

- **Prototype** :
  ```c
  ENET_API int enet_address_set_host(ENetAddress * address, ENetAddressType type, const char * hostName);
  ```

- **Paramètres :**
  - `address` : Pointeur vers l’objet adresse à configurer.
  - `type` : Type de l’adresse (IPv4 ou IPv6).
  - `hostName` : Nom d’hôte à résoudre.

- **Retourne** : `0` en cas de succès, `< 0` en cas d’échec.

<br /><br />

#### `enet_address_get_host_ip`
Récupère l’adresse IP imprimable à partir d’un objet adresse.

- **Prototype** :
  ```c
  ENET_API int enet_address_get_host_ip(const ENetAddress * address, char * hostName, size_t nameLength);
  ```

- **Paramètres :**
  - `address` : Pointeur vers l’objet adresse.
  - `hostName` : Tampon pour stocker l’adresse IP imprimable.
  - `nameLength` : Longueur du tampon.

- **Retourne** : `0` en cas de succès, `< 0` en cas d’échec.

<br /><br />

#### `enet_address_get_host`
Effectue une résolution inverse de l’adresse IP pour obtenir le nom d’hôte.

- **Prototype** :
  ```c
  ENET_API int enet_address_get_host(const ENetAddress * address, char * hostName, size_t nameLength);
  ```

- **Paramètres :**
  - `address` : Pointeur vers l’objet adresse.
  - `hostName` : Tampon pour stocker le nom d’hôte.
  - `nameLength` : Longueur du tampon.

- **Retourne** : `0` en cas de succès, `< 0` en cas d’échec.

<br /><br />

## Conclusion

Cette documentation vise à éclaircir les concepts fondamentaux liés à la gestion des adresses dans RCENet. Pour plus d’informations ou d’assistance, veuillez consulter la documentation officielle de RCENet ou le guide de programmation ENet.
