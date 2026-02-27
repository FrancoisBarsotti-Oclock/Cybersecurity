# 🔰 La BIOS, l’UEFI, le MBR et le GPT

## La Puce BIOS
C’est un composant matériel essentiel de la **carte mère**.

Cette puce est généralement de petite taille et soudée directement sur la carte mère.

Le BIOS stocké dans cette puce est un logiciel de bas niveau, qui contrôle des fonctions critiques comme l’**initialisation de la mémoire**, du **processeur** et des **périphériques**.

Question d’utilité on parle de bas ou haut niveau. Bas niveau c’est tout ce qui est en lien avec la machine et le haut niveau plutôt orienté humain.

Elle stocke le **firmware BIOS**, qui initialise le matériel lors de la mise sous tension de l’ordinateur et permet de démarrer le système d’exploitation.

**Rôle principal** : Assurer la communication entre les composants matériels et le système d’exploitation.

Si l’on a un BIOS on n’a pas un UEFI

![[Pasted image 20251028215515.png]]
### Le Clear CMOS

C’est un processus permettant de réinitialiser les paramètres du **BIOS**  ou de l’**UEFI** à leurs valeurs par défaut d’usine.

Le CMOS (Complimentary Metal-Oxide-Semiconductor) est une petite mémoire sur la carte mère qui stocke les paramètres de configuration du BIOS, comme l’**ordre de démarrage**, la **date et heure**, ou les **paramètres de CPU**.

Le Clear CMOS peut être nécessaire en cas de mauvaise configuration, d’overclocking incorrect ou de problèmes de démarrage. Il se fait en retirant brièvement la pile CMOS ou en utilisant un **jumper Clear CMOS** sur la carte mère.

### Le Firmware
Le **firmware** est un logiciel intégré directement dans le matériel d’un appareil. Il se situe entre le matériel (hardware) et le système d’exploitation, permettant aux composants de fonctionner ensemble.

Les **fimwares** sont présents dans une large gamme d’appareils, des ordinateurs aux smartphones, en passant par les routeur et imprimantes. Ils sont essentiels pour le bon fonctionnement de ces dispositifs, car ils contrôlent les fonctions matérielles de base.

**Exemples courants** : BIOS, UEFI, firmware de cartes graphiques, microcontrôleurs dans les appareils électroniques.

![[Pasted image 20251028215638.png]]
On peut faire poser un système anti surtension directement sur le tableau électrique en entrée, pour les surtensions (200€)

Software = logiciel. Quand il est embarqué dans un composant matériel c’est un Firmware (accès direct sur ton matériel).
# Le BIOS et l’UEFI

Le **BIOS** et l’**UEFI** sont des exemples de firmwares qui gèrent l’**initialisation du système** et le **démarrage de l’OS**.

Le firmware peut être mis à jour pour corriger des bugs, améliorer la sécurité ou ajouter de nouvelles fonctionnalités, mais ces mises à jour doivent être effectuées avec précaution.

Ces technologies gèrent l’**initialisation matérielle**, le **démarrage du système d’exploitation**, ainsi que le **partitionnement des disques**.

Il sont la même chose mais faits différemment

## Le BIOS (Basic Input/Output System)

Le **BIOS** est un firmware qui assure l’**initialisation matérielle** et le **chargement du système d’exploitation** lors du démarrage. Il est souvent utilisé sur les systèmes plus anciens.

Les BIOS initialise les composants matériels comme la **mémoire**, le **processeur**, et les **périphériques**, puis recherche le secteur de démarrage sur le disque pour lancer le système d’exploitation.

L’utilisation du BIOS est basique, et sa navigation est compliquée, car elle se fait uniquement au clavier et les menus sont souvent datés. Chaque BIOS a ses propres réglages et son propre fonctionnement. L’acceès au BIOS varie également selon les constructeurs. En général, les touches **F2** ou **DEL** permettent d’y accéder.

![[Pasted image 20251028215728.png]]

Un autre utilisé largement en virtualisation (comme sur LinuxBIOS ou **coreboot**, open source) comme il suit :

![[Pasted image 20251028215759.png]]

### Paramètres majeurs du BIOS

Le BIOS permet de configurer des éléments clés du système. Le paramètre **Boot Order** définit l’ordre dans lequel les périphériques sont vérifiés pour démarrer, tels que le disque dur ou une clé USB.

Le support **Legacy USB** peut être activé pour permettre l’utilisation de périphériques USB pendant le démarrage.

Le BIOS permet également de régler l’**heure et la date du système**, de surveiller l’état des disques avec le **S.M.A.R.T Monitoring**, ou encore de configurer le **processeur** et gérer l’**alimentation** avec des options comme le **Wake-on-LAN**.

SMART Monitoring

![[Pasted image 20251028215835.png]]

Wake-on-LAN (démarrer son PC à distance)

![[Pasted image 20251028215857.png]]

![[Pasted image 20251028215920.png]]
### Paramètres majeurs du BIOS

Le BIOS est responsable de la gestion de la **mémoire RAM**, du **contrôle des ventilateurs** et des **températures**, et de la surveillance de certains composants critiques du système.

Il inclut des options de **sécurité** telles que la définition de **mots de passe** pour protéger l’accès au BIOS et au démarrage.

Il permet d’activer la **virtualisation matérielle** (Intel **VT-x** ou AMD **AMD-V**) et de configurer les **ressources systèmes** comme les **interruptions (IRQ)** et les **accès directs à la mémoire (DMA)**.

Enfin, le BIOS peut être mis à jour (**flashé**) pour prendre en charge de nouvelles fonctionnalités ou corriger des erreurs.
### Flasher ?
Oui, mettre à jour le firmware de sa carte mère permet de rendre son système plus stable et plus performant.

Néanmoins, la mise à jour du BIOS ou de l’UEFI n’est pas une démarche sans risques.

Il y a un risque (certes faible) que la mise à jour plante et si c’est le cas… **la carte mère serait H.S. !**

Quand il se brique (bricked/crashe):
![[Pasted image 20251028215954.png]]

Au contraire

![[Pasted image 20251028220026.png]]
### Limitations du BIOS

Il présente plusieurs limitations. Il ne peut pas gérer des disques de plus de **2 To** et fonctionne en mode **16 bits**, ce qui limite ses performances et sa flexibilité par rapport aux systèmes modernes.

Le BIOS est simple à configurer et compatible avec des systèmes anciens.
Il est limité en termes de capacités de disque et de gestion de ressources. Et en terme de Technologies, pas de USB 3.0 ou de Secure Boot Natif

# L’UEFI la modernité ?

## L’UEFI (United Extensible Firmware Interface

L**’UEFI** est le successeur du BIOS, conçu pour offrir plus de flexibilité et de fonctionnalités modernes.

Contrairement au BIOS, l’UEFI propose une interface plus intuitive avec la prise en charge de la souris, des menus plus graphiques, et des fonctionnalités plus avancées.

Il peut gérer des disques de plus de 2 To grâce à l’utilisation de la table de partitionnement GPT (GUID Partition Table).

Pour quelques marques il ressemble plus ou moins à cela :

![[Pasted image 20251028220229.png]]
### Paramètres majeurs de l’UEFI

L’UEFI offre des paramètres avancés comme le **Secure Boot**, qui empêche le démarrage de systèmes d’exploitation non signés, et le **CSM** (Compatibility Support Module) qui permet d’imiter le BIOS pour assurer la compatibilité avec des anciens systèmes.

Le **Fast Boot** réduit le temps de démarrage en éliminant certaines étapes non essentielles. De plus, l’UEFFI permet de démarrer un système depuis un réseau via le **Network Boot (PXE)**, et supporte des options de sécurité avancées comme le **TPM** (Trusted Platform Module) pour le chiffrement des données.

### Secure Boot
C’est une fonctionnalité de sécurité intégrée dans l’**UEFI** qui garantit que seul un système d’exploitation signé et approuvé peut démarrer sur un ordinateur. Il empêche le chargement de logiciels malveillants au démarrage, comme des **rootkits** ou d’autres programmes non autorisés.

Lors du démarrage, le Secure Boot vérifie que le **charger de démarrage** du système d’exploitation possède une signature numérique valide. Si la signature est approuvée, le processus de démarrage se poursuit. Sinon, l’ordinateur refusera de démarrer le système, protégeant ainsi contre les menaces.

Cette fonctionnalité peut être activée ou désactivée dans les paramètres de l’UEFI, selon les besoins de l’utilisateur.

### Fast Boot
Le **Fast Boot** est une fonctionnalité de l’**UEFI** qui accélère le processus de démarrage en réudisant ou en sautant certaines étapes d’initialisation matérielle non essentielles. Cette option est conçue pour améliorer les temps de démarrage, notamment sur les ordinateurs modernes.

Lorsque le **Fast Boot** est activé, l’UEFI passe rapidement certaines vérifications matérielles, comme les tests de mémoire RAM et la détection exhaustive des périphériques. Cela permet de démarrer le système d’exploitation beaucoup plus rapidement.
## Comparaison BIOS vs UEFI
Le **BIOS** et l’**UEFI** diffèrent sur plusieurs points importants, le BIOS fonctionne en mode **16 bits** et est limité à des **disques de 2 To**, tandis que l’UEFI supporte des **disques de plus de 2 To** et fonctionne en **32 ou 64 bits**.

L’interface de BIOS est textuelle et simple, tandis que l’UEFI propose une **interface graphique** plus intuitive, avec des options supplémentaires comme le **Secure Boot** et la gestion des **disques modernes**.
# MBR, GPT ?

![[Pasted image 20251028220332.png]]
## MBR (Master Boot Record)
Le **MBR** est un ancien système de partitionnement qui gère l’organisation et le démarrage des disques durs. Il est situé dans le tout premier secteur du disque et contient les informations nécessaires pour démarrer le système d’exploitation.

Le MBR divise un disque en **quatre partitions principales** au maximum. Si plus de partitions sont nécessaires, une des partitions principales doit être une **partition étendue**, qui peut contenir plusieurs **partitions logiques**. Le MBR est utilisé principalement sur les anciens systèmes.
### Structure et limitations du MBR
Le MBR limite le nombre de **partitions** à **4 principales** et ne supporte que des disques d’une taille maximale de **2 To**. Il est aussi vulnérable à la **corruption** car toutes les informations de démarrage sont stockées dans un seul secteur.

Tiens, la limitation de taille du disque est la même que le BIOS !

![[Pasted image 20251028220428.png]]

Avec des disques étendus :

![[Pasted image 20251028220454.png]]

De toute façon il n’est pas recommendable de faire des partitions à moins que bien justifié

# Le GPT (GUID Partition Table)

## Le GPT
Le **GPT** est un système de partitionnement moderne conçu pour remplacer le MBR. Il utilise des **ID uniques** pour chaque partition et supporte jusqu’à **128 partitions** par disque.
### Avantages du GPT
Le GPT supporte des **disques de plus de 2 To** et offre une meilleure **protection contre la corruption de données** grâce à un mécanisme de **CRC32**.

Il est également conçu pour fonctionner de manière optimale avec l’**UEFI**, permettant une plus grande flexibilité et sécurité.

Le **CRC32** (Cyclic Redudancy Checksum ou Contrôle de Redondance Cyclique) est un nombre calculé sur la base de 32 bits (d’où vient le suffixe32) et qui représente une signature (unique) de données quelconques.

### Comparaison MBR vs GPT
Le **MBR** est limité à **4 partitions** principales et à des **disques de 2 To**, tandis que le **GPT** permet de créer jusqu’à **128 partitions** et supporte des **disques de grande capacité**.

Le GPT offre également une meilleure protection contre la **corruption des données** et est plus adapté aux systèmes modernes utilisant l’UEFI.
### Tableau comparatif MBR vs GPT

![[Pasted image 20251028220656.png]]
**Revoir la loi de Moore.**

# Systèmes de fichiers : Introduction

Les **systèmes de fichiers** sont des structures qui organisent et gèrent la manière dont les données sont stockées et récupérées sur des disques. Trois des systèmes de fichiers les plus utilisés sont **NTFS**, **FAT32**, et **exFAT**.

Chaque système de fichiers a ses propres caractéristiques, avantages et limitations, et est utilisé dans différents contextes selon les besoins en matière de **compatibilité**, de **performances**, et de  **sécurité.**

## NTFS (New Technology File System)

Le **NTFS** est un système de fichiers développé par Microsoft pour les systèmes **Windows**.

Il est le système par défaut pour les disques durs modernes utilisés sur Windows.

### Caractéristiques du NTFS :

-Supporte des **fichiers de grande taille** (plus Ide 4 Go) et des **volumes jusqu’à 16 To** et plus.

-Offre des **fonctionnalités avancées** comme le chiffrement des fichiers, la compression, et des permissions de sécurité détaillées.

-Utilise un **journal de fichiers** pour protéger contre la corruption des données en cas de panne.

**Inconvénients** : Compatibilité limitée avec les systèmes autres que Windows.

## FAT32 (File Allocation Table 32)

Le **FAT32** est un système de fichiers plus ancien et largement compatible. Il est couramment utilisé sur des périphériques de stockage comme les clés USB et les cartes mémoire.
### Caractéristiques du FAT32
- Compatible avec la majorité des systèmes d’exploitation, y compris **Windows**, **macOS, Linux**, et la plupart des appareils électroniques.
- Limité à des **fichiers de 4 Go maximum** et à des **volumes de 32 Go** pour Windows.

**Inconvénients** : Ne supporte pas les grands fichiers (>4 Go) et offre moins de sécurité et de fonctionnalités avancées que NTFS.

## exFAT (Extended File Allocation Table)
Le **exFAT** est un système de fichiers créé pour combler l’écart entre **FAT32** et **NTFS**, particulièrement pour les supports de stockage amovibles comme les clés USB et les disques externes.
### Caractéristiques de exFat
- Supporte des **fichiers de très grande taille** (au-delà de 4 Go) sans les limitations de FAT32.
- Compatible avec **Windows, mcOS,** et de nombreux appareils, tout en étant plus léger que NTFS.
**Avantages** : Support des grands fichiers, bonne compatibilité entre systèmes.
**Inconvénients :** Moins de fonctionnalités avancées par rapport à NTFS (comme le chiffrement ou les permissions).
### Comparaison des systèmes de fichiers

![[Pasted image 20251028220823.png]]

Chaque système de fichiers répond à des besoins spécifiques en matière de stockage, que ce soit pour des **périphériques amovibles** ou des **disques internes**

### Choisir le bon système de fichiers

Le choix du système de fichiers dépend de plusieurs facteurs :
- **NTFS** est idéal pour les **disques durs internes** dans un environnement Windows, où la **sécurité** et la gestion de **grands fichiers** sont essentielles.
- **FAT32** est recommandé pour les **clés USB** ou les **cartes mémoire** destinées à être utilisées sur une variété d’appareils, mais limité à de petits fichiers.
- **exFAT** est le meilleur choix pour les **périphériques amovibles** nécessitant la comptabilité multi-plateformes avec des fichiers de grande taille.

Voir un peu sur ext4 (Linux) pour la comparer avec les SO Windows après

**Cours qui suit:** [[ITIL V4]]
