# Documentation de l'API de Codage par Plages RCENet

Bienvenue dans la documentation de l'API de Codage par Plages RCENet. Cette section offre un guide complet sur l'utilisation du codage par plages au sein de la bibliothèque RCENet, mettant en évidence les fonctions clés et leurs applications dans la compression et la décompression de données.

## Vue d'ensemble

La bibliothèque RCENet inclut le support du codage par plages, une forme efficace de codage entropique utilisé dans la compression de données. Le codage par plages est particulièrement efficace pour compresser des données avec des distributions de probabilité connues. Ce document détaille les fonctions disponibles dans RCENet pour utiliser le codage par plages pour la compression et la décompression des données de paquets.

<br /><br />

## Fonctions

### `enet_range_coder_create`

_Crée une nouvelle instance d'un codeur par plages pour les opérations de compression et de décompression._

```c
ENET_API void * enet_range_coder_create(void);
```

- **Retourne :** Un pointeur vers l'instance de codeur par plages nouvellement créée, ou `NULL` en cas d'échec.

<br /><br />

### `enet_range_coder_destroy`

_Détruit une instance de codeur par plages précédemment créée._

```c
ENET_API void enet_range_coder_destroy(void * context);
```

- **Paramètres :**
  - `context` : Un pointeur vers l'instance de codeur par plages à détruire.

<br /><br />

### `enet_range_coder_compress`

_Compresse des données en utilisant le codeur par plages._

```c
ENET_API size_t enet_range_coder_compress(void * context, const ENetBuffer * buffers, size_t bufferCount, size_t inLimit, enet_uint8 * outData, size_t outLimit);
```

- **Paramètres :**
  - `context` : Un pointeur vers l'instance de codeur par plages.
  - `buffers` : Un tableau de structures `ENetBuffer` contenant les données à compresser.
  - `bufferCount` : Le nombre de tampons dans le tableau.
  - `inLimit` : Le nombre maximum d'octets à compresser.
  - `outData` : Le tampon pour stocker les données compressées.
  - `outLimit` : La taille maximale du tampon `outData`.

- **Retourne :** La taille des données compressées en octets, ou `0` en cas d'échec.

<br /><br />

### `enet_range_coder_decompress`

_Décompresse des données précédemment compressées avec un codeur par plages._

```c
ENET_API size_t enet_range_coder_decompress(void * context, const enet_uint8 * inData, size_t inLimit, enet_uint8 * outData, size_t outLimit);
```

- **Paramètres :**
  - `context` : Un pointeur vers l'instance de codeur par plages.
  - `inData` : Le tampon contenant les données compressées.
  - `inLimit` : La taille du tampon `inData` en octets.
  - `outData` : Le tampon pour stocker les données décompressées.
  - `outLimit` : La taille maximale du tampon `outData`.

- **Retourne :** La taille des données décompressées en octets, ou `0` en cas d'échec.

<br /><br />

## Conclusion

L'API de Codage par Plages RCENet fournit un ensemble puissant de fonctions pour compresser et décompresser des données en utilisant le codage par plages. En exploitant ces fonctionnalités, les applications peuvent atteindre une transmission de données efficace, en particulier lorsqu'elles traitent des motifs de données prévisibles. Pour plus de détails ou d'assistance, consultez la documentation officielle de RCENet ou la littérature sur le codage par plages.