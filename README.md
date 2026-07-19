# OHM Rainmeter Dashboard

Tableau de bord matériel compact pour **Rainmeter**, conçu pour afficher les principales informations système dans une interface sombre et homogène.

Le package inclut plusieurs modules indépendants : processeur, carte graphique, mémoire, stockage, réseau et horloge. Les données matérielles sont récupérées grâce au plugin **LibreMeter**.

## Aperçu des modules

| Module | Informations affichées |
|---|---|
| **CPU** | Charge, température, puissance, tension, fréquence maximale et état des six P-Cores |
| **GPU** | Charge, températures GPU/hotspot/mémoire, puissance, tension, fréquences, VRAM, moteurs D3D/vidéo et ventilateurs |
| **Memory** | Utilisation de la RAM et du fichier d’échange |
| **Disk** | Capacité utilisée, capacité totale et pourcentage d’occupation |
| **Network** | Adresse IP publique, débit descendant et débit montant |
| **Clock** | Heure, jour et date |

## Configuration prévue

Les profils matériels fournis sont configurés pour :

- **CPU : Intel Core i5-13600K**
- **GPU : NVIDIA GeForce RTX 4070 12 Go**
- **Disque principal : `C:`**
- **Connexion réseau : jusqu’à 1 Gbit/s en téléchargement et en envoi**

Les identifiants de capteurs peuvent varier selon le matériel et l’ordre de détection dans Libre Hardware Monitor. Une adaptation des fichiers `.ini` peut donc être nécessaire sur une autre configuration.

## Prérequis

- Windows 7 ou version ultérieure
- Rainmeter **4.5.26.3894** ou version ultérieure
- **Dernière version de Libre Hardware Monitor**, lancée en arrière-plan
- Plugin **LibreMeter**, inclus dans le package en versions 32 et 64 bits
- Police **Segoe UI**, fournie avec Windows

## Installation

1. Installer Rainmeter.
2. Télécharger et lancer la **dernière version de Libre Hardware Monitor**, puis vérifier que les capteurs sont correctement détectés.
3. Ouvrir le fichier `OHM_1.rmskin`.
4. Cliquer sur **Install** dans l’installateur Rainmeter.
5. Depuis Rainmeter, charger les modules situés dans le dossier `OHM`.
6. Positionner les différents panneaux sur le bureau selon vos préférences.

## Structure du package

```text
OHM/
├── CLOCK/
│   └── Clock.ini
├── CPU/
│   └── cpu.ini
├── DISK/
│   └── 1 Disk.ini
├── GPU/
│   └── gpu.ini
├── MEMORY/
│   └── MEMORY.ini
└── NETWORK/
    └── Network.ini

Plugins/
├── 32bit/
│   └── LibreMeter.dll
└── 64bit/
    └── LibreMeter.dll
```

## Personnalisation

### Processeur

Le module CPU utilise actuellement les capteurs suivants :

```ini
SensorId=/intelcpu/0/...
```

Les principales limites peuvent être modifiées dans `CPU/cpu.ini` :

```ini
maxClock=5200
maxPower=181
sleepThreshold=800
```

`maxClock` définit l’échelle maximale des fréquences, `maxPower` l’échelle de puissance et `sleepThreshold` le seuil sous lequel un P-Core est indiqué comme étant en veille.

Le module suit uniquement les six P-Cores du Core i5-13600K. Les E-Cores ne sont pas affichés dans cette version.

### Carte graphique

Le module GPU utilise actuellement les capteurs NVIDIA suivants :

```ini
SensorId=/gpu-nvidia/0/...
```

Les limites sont modifiables dans `GPU/gpu.ini` :

```ini
maxGPUClock=3000
maxMemoryClock=12000
maxPower=200
maxFan=3500
```

La quantité de VRAM est actuellement fixée à **12 Go** dans la formule suivante :

```ini
Formula=(measureGPUMemoryLoad * 12.0) / 100
```

Pour une autre carte graphique, remplacez `12.0` par sa quantité réelle de mémoire vidéo et adaptez les identifiants des capteurs.

### Disque

Le disque surveillé est défini dans `DISK/1 Disk.ini` :

```ini
disk1=C:
```

Remplacez `C:` par la lettre du volume à afficher.

### Réseau

Les valeurs maximales sont exprimées en octets par seconde dans `NETWORK/Network.ini` :

```ini
maxDownload=125000000
maxUpload=125000000
```

Exemples :

| Débit de la connexion | Valeur à utiliser |
|---:|---:|
| 100 Mbit/s | `12500000` |
| 500 Mbit/s | `62500000` |
| 1 Gbit/s | `125000000` |
| 2,5 Gbit/s | `312500000` |

L’adresse IP publique est récupérée depuis `checkip.amazonaws.com` toutes les quatre heures.

### Apparence

Les couleurs et dimensions principales sont définies au début de chaque fichier `.ini` :

```ini
colorText=245,245,245,225
colorMuted=160,160,160,200
colorBar=235,170,0,255
skinWidth=240
```

Le quatrième nombre d’une couleur correspond à son niveau de transparence, compris entre `0` et `255`.

## Raccourcis intégrés

- Un clic sur le titre **Memory** ouvre le Gestionnaire des tâches.
- Un clic sur le titre **Network** ouvre les connexions réseau Windows.
- Un clic sur le titre **Disk** ouvre l’explorateur de fichiers.
- Un clic sur le volume ouvre directement le disque surveillé.

## Dépannage

### Les valeurs CPU ou GPU restent à zéro

- Vérifiez que Libre Hardware Monitor est lancé.
- Vérifiez que le matériel apparaît correctement dans Libre Hardware Monitor.
- Contrôlez les `SensorId` utilisés dans les fichiers CPU et GPU.
- Redémarrez Rainmeter après toute modification du plugin ou des capteurs.

### Les capteurs ne correspondent pas à mon matériel

L’ordre des capteurs peut changer selon le processeur, le GPU, le pilote ou la version de Libre Hardware Monitor. Modifiez les lignes `SensorId=` afin qu’elles correspondent aux capteurs exposés sur votre machine.

### Les barres réseau sont toujours pleines ou presque invisibles

Ajustez `maxDownload` et `maxUpload` à la vitesse réelle de votre connexion. Ces valeurs servent uniquement à définir l’échelle graphique.

### La VRAM affichée est incorrecte

Le module estime la quantité utilisée à partir du pourcentage de charge mémoire et d’une capacité fixe de 12 Go. Modifiez la formule pour correspondre à votre carte graphique.

## Limites connues

- Configuration CPU spécifique au Core i5-13600K et à ses six P-Cores.
- Configuration GPU spécifique à une RTX 4070 de 12 Go.
- Les identifiants LibreMeter peuvent devoir être adaptés manuellement.
- La VRAM affichée est une estimation calculée depuis le pourcentage d’utilisation mémoire.
- L’adresse IP publique nécessite une connexion Internet.

## Auteur

- Skin Rainmeter : **Guillaume**
- Package RMSKIN : **Maarek**
- Version du package : **1**

## Licence

Aucune licence n’est actuellement incluse dans le package.
