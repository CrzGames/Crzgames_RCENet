# Crzgames - RCENet

Dernier commit récupérer par rapport à ENet officiel, branche master : `657eaf9`

<br />

## 📋 Great novelty compared to ENet Original
- IPv6 / IPv4
- Encrypt / Decrypt (ENetEncryptor)
- Add ENET_EVENT_TYPE_DISCONNECT_TIMEOUT
- ENET_PROTOCOL_MAXIMUM_PEER_ID 4096 replace to 65535 peers simultaneously by host
- PRs for fixes that were never applied to the original ENet that are included in it
- New documentation with Vitepress
- New API
- Addition of a lot of comments in the sources and includes, a JSDOC style
- The protocol of the original ENet lib has been modified so will not work with the original protocol
- Other improvements and fixes

<br /><br /><br /><br />


## 🛠 Tech Stack
- C (Language)
- CI / CD (Github Actions)
- xmake (Build script)
- Compiler (GCC, CL, Clang, Ninja, NDK)

<br /><br /><br /><br />


## 📚 API Documentation
- WebSite Production : https://librcenet.crzcommon2.com 
- WebSite Staging : https://staging.librcenet.crzcommon2.com 

<br /><br /><br /><br />


## 📋 Supported platforms :
Ensuring that your game runs smoothly requires understanding the minimal system requirements for each platforms. Below are the requirements that users need to meet to experience your game without issues.

| Platform | Architectures | System Version | Compatible |
|----------|---------------|----------------|------------|
| **Windows** | x86 / x64 / arm / arm64 / arm64ec | Windows Vista+  | ✓          |
| **macOS** | Intel x86_64 / Apple Silicon arm64 | macOS 11.0+ | ✓ |
| **tvOS** | x86_64 / arm64 (appletvos) - x86_64 / arm64 (appletvsimulator) | tvOS 12.0+ | ✓ |
| **iOS/iPadOS** | arm64 / arm64e (iphoneos) - arm64 / x86_64 (iphonesimulator) | iOS 12.0+ | ✓ |
| **Android** | arm64-v8a / armeabi-v7a / x86_64 / x86 | Android 6.0+ | ✓ |
| **BSD** | x86 / x86_64 | glibc 2.35+ | ✓ |
| **Linux** | x86 / x86_64 / arm / arm64 | glibc 2.35+ | ✓ |
| **Haiku** | x86 / x86_64 | glibc 2.35+ | ✓ |
| **Solaris** | x86 / x86_64 | glibc 2.35+ | ✓ |
| **Steam Linux** | x64 / arm64 | Steam Linux Runtime 3.0 (Sniper) | ✓ |
| **Steam Deck** | x64 | Steam Linux Runtime 3.0 (Sniper) | ✓ |
| **Xbox** | x64 | Xbox Série X/S+ |  |
| **Nintendo Switch** | arm64 | Nintendo Switch 1+ |  |
| **Playstation** | x64 | Playstation 5+ |  |

<br /><br /><br /><br />


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

<br /><br /><br /><br />


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
  1. Run command (replace debian for name) : sudo usermod -aG sudo debian
  2. Download and Install brew : /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
  3. Après l'installation de homebrew il faut importer les variables d'environnement et installer les deux librairies : 
    echo '# Set PATH, MANPATH, etc., for Homebrew.' >> /home/debian/.bashrc && 
    echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> /home/debian/.bashrc && 
    eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)" &&
    sudo apt-get install -y build-essential &&
    brew install gcc
  4. Download and Install xmake.


  # macOS :
  1. Requirements : MacOS X >= 15.5.0
  2. Download and Install xCode >= 16.4.0
  3. Download and Install Command Line Tools : xcode-select --install
  4. Download and Install xmake.


  # Android (run in Windows) :
  1. Download and Install : Android Studio 2025.1.1 or newer
  2. Add environment variable: ANDROID_HOME for path SDK Android (SDK Manager path)
  3. Download and Install xmake.
  4. Download and Install Java JDK LTS (Oracle) == 17.0.10.

  # Android (run in Unix) :
  1. TODO: A faire


  # iOS (only macOS) :
  1. Requirements : MacOS X >= 15.5.0
  2. Download and Install Command Line Tools : xcode-select --install
  3. Download and Install xCode >= 16.4.0
  4. Download and Install SDK iOS >= 18.0.0
  5. Download and Install brew : /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
  6. Download and Install xmake.
  ```
  
<br /><br /><br /><br />


## 🔄 Cycle Development
1. Generate project for Executable (Visual Studio 2022, xCode...)
2. Open project with Visual Studio, xCode..
3. Edit or add files in 'src' or 'include' folder
4. Run project
  
<br /><br /><br /><br />


## Production
### ⚙️➡️ Automatic Distribution Process (CI / CD)
#### Si c'est un nouveau projet suivez les instructions : 
1. Ajoutées les SECRETS_GITHUB pour :
   - O2SWITCH_FTP_HOST
   - O2SWITCH_FTP_PASSWORD
   - O2SWITCH_FTP_PORT
   - O2SWITCH_FTP_USERNAME
   - PAT (crée un nouveau token si besoin sur le site de github puis dans le menu du "Profil" puis -> "Settings" -> "Developper Settings' -> 'Personnal Access Tokens' -> Tokens (classic))
