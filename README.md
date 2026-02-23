# Crzgames - RCENet

Dernier commit récupérer par rapport à ENet officiel, branche master : `657eaf9`

<br /><br />

---

<br /><br />

## 📋 Grande nouveauté par rapport à ENet Original
- IPv6 / IPv4
- Cryptage / Décryptage (ENetEncryptor)
- Ajout de ENET_EVENT_TYPE_DISCONNECT_TIMEOUT
- ENET_PROTOCOL_MAXIMUM_PEER_ID 4096 remplacé par 65535 pairs simultanés par hôte
- PR pour les corrections qui n'ont jamais été appliquées à l'ENet d'origine et qui y sont incluse
- Nouvelle documentation avec Vitepress
- Nouvelle API
- Ajout de nombreux commentaires dans les sources et les inclusions, dans le style JSDOC
- Le protocole de la bibliothèque ENet d'origine a été modifié et ne fonctionnera donc pas avec le protocole d'origine
- Autres améliorations et corrections

<br /><br />

---

<br /><br />

## 🛠 Tech Stack
- C (Language)
- CI / CD (Github Actions)
- xmake (Build script)
- Compiler (GCC, CL, Clang, Ninja, NDK)

<br /><br />

---

<br /><br />

## 📚 API Documentation
- WebSite Production : https://librcenet.crzgames.com 
- WebSite Staging : https://staging.librcenet.crzgames.com 

<br /><br />

---

<br /><br />

## 📋 Plateformes supportées

- 🟢 supporté  
- 🟡 en cours  
- 🔴 non supporté  

| Platform | Architectures | System Version | Compatible |
|----------|---------------|----------------|------------|
| Windows | x86 / x64 / arm / arm64 / arm64ec | Windows Vista+ | 🟢 |
| macOS | Intel x86_64 / Apple Silicon arm64 | macOS 15.0+ | 🟢 |
| tvOS | x86_64 / arm64 (appletvos) - x86_64 / arm64 (appletvsimulator) | tvOS 18.0+ | 🟢 |
| iOS/iPadOS | arm64 / arm64e (iphoneos) - arm64 / x86_64 (iphonesimulator) | iOS/iPadOS 18.0+ | 🟢 |
| Android | arm64-v8a / armeabi-v7a / x86_64 / x86 | Android 9.0+ | 🟢 |
| BSD | x86 / x86_64 | glibc 2.35+ | 🟢 |
| Linux | x86 / x86_64 / arm / arm64 | glibc 2.35+ | 🟢 |
| Haiku | x86 / x86_64 | glibc 2.35+ | 🟢 |
| Solaris | x86 / x86_64 | glibc 2.35+ | 🟢 |
| Steam Linux | x64 / arm64 | Steam Linux Runtime 4.0 | 🟢 |
| Steam Deck | x64 | Steam Linux Runtime 4.0 | 🟢 |
| Xbox Série X/S | x64 | - | 🔴 |
| Nintendo Switch 1 | arm64 | - | 🔴 |
| Nintendo Switch 2 | arm64 | - | 🔴 |
| Playstation 5 | x64 | - | 🔴 |

<br /><br />

---

<br /><br />

## 📦 RCENet Packages Distribution
| Platform | Format | Compiler | Type |
|----------|-----------|-------|------|
| **Windows-MSVC** | `.lib / .dll` | CL | static / shared |
| **Windows-MinGW** | `.a` | GCC | static |
| **macOS** | `.a / .dylib` | Clang | static / shared |
| **tvOS** | `.a` | Clang | static |
| **iOS/iPadOS** | `.a` | Clang | static |
| **Android** | `.a / .so` | NDK (Clang) | static / shared |
| **Linux** | `.a / .so` | GCC | static / shared |
| **BSD** | `.a / .so` | Clang | static / shared |
| **Haiku** | `.a / .so` | Clang | static / shared |
| **Solaris** | `? / ?` | Clang | ? / ? |
| **Steam Linux** | `.a / .so` | GCC | static / shared |
| **Steam Deck** | `.a / .so` | GCC | static / shared |
| **Xbox** |  |  |
| **Nintendo Switch** |  |  |
| **Playstation** |  |  |

<br /><br />

---

<br /><br />

## ⚙️ Setup Environment Development
1. Clone the project repository using the following commands :
  ```bash
  git clone git@github.com:CrzGames/Crzgames_RCENet.git
  ```
2. (Optional) Download and Install Node.js >= 18.0.0 to run website documentation for RCENet
3. Steps by Platform :
  ```bash  
  # Windows :
  1. Requirements : Windows >= 10
  1. Download and Install Visual Studio == 2022 (MSVC >= v143 + Windows SDK >= 10) : https://visualstudio.microsoft.com/fr/downloads/
  2. Download and Install xmake.


  
  # Linux :
  1. Requirements : glibc >= 2.35.0 (Exemple : Ubuntu >= 22.04 OR Debian >= 12.0), checker via : ldd --version
  2. Download and Install (gcc, g++, make..) :
    sudo apt update
    sudo apt install -y build-essential
  3. Download and Install xmake.



  # macOS :
  1. Requirements : MacOS X >= 15.0.0
  2. Download and Install xCode >= 16.4.0
  3. Download and Install Command Line Tools : xcode-select --install
  4. Download and Install brew : /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
  5. Checker les versions des SDK : 
    xcrun --sdk iphoneos --show-sdk-version
    xcrun --sdk macosx --show-sdk-version
  6. Download and Install xmake.



  # Android (Unix / Windows) :
  1. Download and Install : Android Studio 2025.3.1 or newer
  2. Add environment variable: ANDROID_HOME for path SDK Android (SDK Manager path), sous Windows en général : C:\Users\Corentin\AppData\Local\Android\Sdk
  3. Ouvrir Android Studio et installer certains composants du SDK : Android v16.0, Android SDK v36.1.0, NDK v29.0.14206865, Android SDK Command Line v20, CMake v3.30.3.
  4. Download and Install Java JDK 17 LTS (Temurin) : https://adoptium.net/fr/temurin/releases?version=17&os=any&arch=any  (Pendant l installation du SDK Java de Temurin cocher la case pour ajouté automatiquement la variable d environnement JAVA_HOME)
  5. Si vous avez oublié de coché la case pour ajouter automatiquement la variable d environnement "JAVA_HOME", sous Windows en général : C:\Program Files\Eclipse Adoptium\jdk-17.0.18.8-hotspot
  6. Download and Install xmake.



  # iOS (only macOS) :
  1. Requirements : MacOS X >= 15.0.0
  2. Download and Install xCode >= 16.4.0
  3. Download and Install SDK iOS >= 18.0.0
  4. Download and Install Command Line Tools : xcode-select --install
  5. Download and Install brew : /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
  6. Checker les versions des SDK : 
    xcrun --sdk iphoneos --show-sdk-version
    xcrun --sdk macosx --show-sdk-version
  7. Download and Install xmake.
  ```
  
<br /><br />

---

<br /><br />

## 🔄 Cycle Development
1. Generate project for Executable (Visual Studio 2022, xCode...)
2. Open project with Visual Studio, xCode..
3. Edit or add files in 'src' or 'include' folder
4. Run project
  
<br /><br />

---

<br /><br />

## Production
### ⚙️➡️ Automatic Distribution Process (CI / CD)
#### Si c'est un nouveau projet suivez les instructions : 
1. Ajoutées les SECRETS_GITHUB pour :
   - O2SWITCH_FTP_HOST
   - O2SWITCH_FTP_PASSWORD
   - O2SWITCH_FTP_PORT
   - O2SWITCH_FTP_USERNAME
   - PAT (crée un nouveau token si besoin sur le site de github puis dans le menu du "Profil" puis -> "Settings" -> "Developper Settings' -> 'Personnal Access Tokens' -> Tokens (classic))
