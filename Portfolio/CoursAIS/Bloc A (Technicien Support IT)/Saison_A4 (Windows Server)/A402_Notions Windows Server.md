# 🪟 Session A402. Windows Server (notions)

## Notions à aborder
•	Windows serveur : à quoi ça sert ? 
•	Active Directory, utilisateurs et groupes
•	GPO
•	Partage de fichiers
•	Déploiement d'OS, de logiciels avec WDS
•	Centralisation des mises à jour
•	Bureau à distance avec Hyper-V
•	Serveur de messagerie électronique (Exchange)

Windows server (WS) est un système d’exploitation (OS) proposé par Microsoft pour installer sur nos serveurs physiques, qui date des débuts des prémisses de Windows OS : Dès le départ ils ont commencé à travailler sur un OS pour les PC des utilisateurs et, ils se sont rendu compte qu’en entreprise, il faudrait des serveurs pour gérer différents services ; alors, Windows Server a été la solution proposée par Microsoft pour répondre à ce besoin-là.

Bien sûr on pourrait remplacer WS par un serveur Linux. Or, en entreprises françaises WS est extrêmement présent sur certains services spécifiques (sauf sur des serveurs web, pour héberger des sites web).

Cette omniprésence de WS sur certaines tâches en entreprise fait qu’il soit incontournable pour vérifier la sécurité d’un serveur Windows, en tant qu’expert cybersécurité, Hacker ou pentester.

Windows Server est la solution serveur de Microsoft, conçue pour répondre aux besoins des entreprises en matière d'infrastructure informatique. Son histoire remonte aux années 1990 et a suivi l'évolution des technologies et des exigences du marché.

Au fil des années, Windows Server est devenu un élément clé de nombreuses entreprises, offrant des fonctionnalités telles que la gestion des utilisateurs, la sécurité renforcée et la virtualisation. Les différentes versions de Windows Server, telles que 2000, 2003, 2008, 2012 et 2016, ont apporté des améliorations constantes en termes de performance, de stabilité et de fonctionnalités innovantes.

Imaginons un lycée avec des centaines d’ordinateurs : l’administrateur systèmes n’aura bien sûr pas besoin de passer sur chaque pc du parquet informatique, pour créer un utilisateur pour chaque élève et pour chaque année, avec la possibilité de trouver toujours ses dossiers sur n’importe quel ordinateur qui fasse partie du même LAN. Pour cela il y a WS. 

Imaginons aussi qu’un salarié quitte l’entreprise sans rendre l’ordinateur corporatif, nous aurons alors besoin de couper ses accès. Cela pourra être possible grâce à la gestion centralisée de WS : on désactive son compte utilisateur côté serveur, et il ne pourra plus se connecter sur ce pc.

Aujourd'hui, Windows Server continue d'évoluer pour répondre aux besoins changeants du monde de l'informatique en entreprise.
## Les débuts de Windows Serveur
Windows NT 3.1 (1993)
![[Pasted image 20251203131520.png]]
Windows NT 3.1, sorti en 1993, a marqué les débuts de la gamme Windows Server. C'était la première version stable et sécurisée, offrant une plateforme multi architecturale capable de s'exécuter aussi bien sur les processeurs x86 que RISC.

RISC est un acronyme pour "Reduced Instruction Set Computer", qui désigne une architecture de processeur à jeu d'instructions réduit. La prise en charge de l'architecture RISC dans Windows NT 3.1 était une avancée majeure, permettant à la plateforme de s'adapter aux différents types de processeurs présents sur le marché.

Cette flexibilité a contribué à l'adoption croissante de Windows Server dans le domaine de l'informatique en entreprise.

RISC ? [Processeur à jeu d'instructions réduit — Wikipédia](https://fr.wikipedia.org/wiki/Processeur_%C3%A0_jeu_d%27instructions_r%C3%A9duit)
## Première version de Windows NT.

- Nouveauté majeure : Début de la prise en charge système d'exploitation 32 bits.
- Interface graphique similaire à Windows 3.1.
- Support des applications DOS et OS/2 16 bits.
### Windows NT 3.5 et 3.51
Améliorations et stabilité
![[Pasted image 20251203131703.png]]
Les versions successives de Windows NT 3.5 (1994) et 3.51 (1995) ont apporté des améliorations significatives en termes de stabilité et de performances.

La gestion de la mémoire a été optimisée, offrant une meilleure fiabilité du système.

De plus, la prise en charge de l'architecture 32 bits a permis d'exploiter pleinement les capacités des processeurs de l'époque, boostant les performances générales.

- Support de Win32 pour les applications 32 bits.
- Meilleure compatibilité avec Windows 95.
- Introduction du service de téléphonie TAPI.
### Windows NT 4.0
Version pro de Microsoft orientée réseau et sécurité. Contrairement à Windows 95, elle ne repose pas sur MS-DOS, même si l’interface Windows 95 y est intégrée. [Windows NT 4.0 — Wikipédia](https://fr.wikipedia.org/wiki/Windows_NT_4.0)
![[Pasted image 20251203131744.png]]
Windows NT 4.0, lancé en 1996, adopte l'interface graphique familière de Windows 95, rendant le système plus accessible aux utilisateurs Windows.

De plus, Windows NT 4.0 offrait une compatibilité améliorée avec les périphériques et les logiciels Windows 95, ce qui a simplifié la migration vers cette nouvelle version serveur.

- Interface utilisateur similaire à Windows 95.
- Intégration d'Internet Explorer 2.0.
- Introduction de DirectX.
- Service de Terminal Server.
### Windows 2000
Un OS robuste et fiable
![[Pasted image 20251203131821.png]]
Windows 2000 a offert une plus grande fiabilité et stabilité, réduisant les arrêts intempestifs et améliorant la productivité des entreprises.

Il introduit aussi Active Directory pour la gestion efficace d’un grand nombre d’utilisateurs et de machines, ainsi que la prise en charge du cryptage de fichiers.

- Plug and Play facilité, compatibilité logicielle accrue.
- MMC (Microsoft Management Console), DNS dynamique, NTFS 3.0.
### Windows Server 2003
Serveurs et Web
![[Pasted image 20251203131905.png]]
Windows Server 2003 a marqué une étape décisive dans l'essor des serveurs Microsoft.

Cette version a apporté une meilleure prise en charge des technologies Web, permettant aux entreprises de déployer plus facilement leurs applications en ligne. Les outils de gestion et

d'administration des serveurs ont été améliorés, tout comme les fonctionnalités de sécurité réseau, offrant une fiabilité et des performances accrues.

Un version R2 a vu le jour, apportant encore plus de fonctionnalités et d'améliorations. Cela incluait une connectivité Web plus avancée, des outils de gestion et d'administration simplifiés et une sécurité renforcée.

- Services IIS 6.0, Volume Shadow Copy.
- Active Directory amélioré, DFS-R, File Server Resource Manager.
- Interopérabilité Unix renforcée.

Avec la version **R2**,

- Améliorations dans la gestion des fichiers avec File Server Resource Manager.
- Introduction de DFS-R (Distributed File System Réplication).
- Améliorations de l'intégration Unix avec les services d'interopérabilité ([Interopérabilité en informatique — Wikipédia](https://fr.wikipedia.org/wiki/Interop%C3%A9rabilit%C3%A9_en_informatique))
### Windows Server 2008 :
Virtualisation et cloud
![[Pasted image 20251203131941.png]]
Avec Windows Server 2008, Microsoft a mis l'accent sur la virtualisation et le cloud computing, des technologies en pleine expansion à l'époque. La consolidation des serveurs physiques en

machines virtuelles a permis une utilisation plus efficace des ressources.

Windows Server 2008 a facilité l'adoption du modèle cloud, offrant une infrastructure d'hébergement flexible et hautement disponible pour les entreprises.

Une version allégée, Server Core, est introduite pour réduire l'empreinte des serveurs.

- Interface Server Core, Hyper-V.
- Server Manager, NAP.
### Windows Server 2008 R2
![[Pasted image 20251203132011.png]]
Renforcement de la virtualisation, avec Live Migration et meilleure gestion du cloud hybride.

- PowerShell 2.0, Active Directory Center.
- BranchCache pour réseaux étendus.
### Windows Server 2012 :
Cloud computing moderne
![[Pasted image 20251203132150.png]]
Avec des fonctionnalités telles que le contrôle d'accès dynamique et la protection contre les logiciels malveillants, Windows Server 2012 a renforcé la sécurité des déploiements cloud privés.

De plus, il a apporté des améliorations en termes de conformité en permettant aux administrateurs de configurer des politiques de sécurité plus strictes et de surveiller activement les activités des utilisateurs.

Avec la version **R2**, amélioration des performances et de la gestion des ressources.

- Interface Metro, Hyper-V évolué.
- ReFS, Dynamic Access Control.
- Storage Spaces, Network Virtualization.
- Accès distant via DirectAccess.
### Windows Server 2016 :
Sécurité et conteneurs
![[Pasted image 20251203132259.png]]
Avec Windows Server 2016 , Microsoft a également introduit la fonctionnalité de virtualisation de stockage, qui permet de gérer et de sécuriser les espaces de stockage directement à partir du serveur, simplifiant ainsi la gestion et l'administration du stockage.

Cette version a également inclus des améliorations significatives en termes de performances et de fiabilité, offrant ainsi une expérience utilisateur améliorée pour les applications et les services déployés sur Windows Server 2016.

- Introduction de Nano Server pour une installation ultra-minimale.
- Conteneurs Windows et Hyper-V pour les déploiements en conteneurs.
- Améliorations de sécurité avec Credential Guard et Device Guard.
- Nouveaux outils pour la gestion des clusters de stockage.
### Windows Server 2019 :
Performance et fiabilité
![[Pasted image 20251203132334.png]]
Windows Server 2019 apporte des améliorations significatives en termes de performances et de fiabilité.

Les moteurs de calcul et de mémoire ont été optimisés pour offrir une plus grande puissance de traitement, tandis que la redondance et la haute disponibilité ont été renforcées pour assurer une meilleure continuité de service.

- Intégration avec Azure pour des hybrides cloud plus faciles.
- Améliorations de la sécurité avec Windows Defender ATP (Advanced Threat Protection).
- Système de fichiers ReFS amélioré pour la résilience des données.
- Introduction de Kubernetes pour la gestion des conteneurs.
### Windows Server 2022 :
L’avenir !
![[Pasted image 20251203135556.png]]
Windows Server 2022 continue d'étendre les capacités des versions précédentes, avec un accent particulier sur la sécurité avancée, l'intégration avec le cloud, et la gestion simplifiée des environnements hybrides.

- Sécurité avancée : Introduction de "Secured-core server" pour une protection renforcée contre les menaces matérielles et firmware, avec des fonctionnalités comme la sécurité basée sur la virtualisation (VBS) et la protection des informations d'identification.
- Améliorations de la connectivité : Support de TLS 1.3 par défaut, améliorations de la connectivité SMB (Server Message Block) avec SMB over QUIC, permettant des connexions sécurisées et rapides sur Internet.
- Windows Admin Center : Intégration et améliorations pour une gestion simplifiée et centralisée des serveurs, avec des capacités étendues pour la surveillance et la gestion des clusters.
- Azure Arc : Capacité de gérer et de sécuriser les serveurs physiques, virtuels et Kubernetes, qu'ils soient sur site, dans Azure, ou dans d'autres clouds via Azure Arc.
- Conteneurs : Améliorations des conteneurs Windows, notamment des images de conteneurs plus petites, une compatibilité améliorée des applications et des outils de gestion de conteneurs plus robustes.
- Stockage : Introduction de nouvelles fonctionnalités pour Storage Spaces Direct, comme des performances améliorées et une latence réduite pour les déploiements hyper-convergés.
- Mises à jour des services : Améliorations continues des services tels que DNS, DHCP, et des capacités de gestion de la mise en réseau.

**Pour la culture**, la part de marché de Windows Server dans le monde est d’environ 70%
![[Pasted image 20251203135629.png]]
Source: [Linux Statistics 2025 - TrueList](https://truelist.co/blog/linux-statistics/)

Mais il y a une Grosse nuance à apporter : cela inclut aussi les serveurs Windows dans les environnements cloud, où Windows Server est largement utilisé pour héberger des applications et des services.

Le Web lui est en grande partie dominé par Linux, mais Windows Server reste un acteur majeur dans les entreprises et les environnements hybrides : [Usage Statistics and Market Share of Operating Systems for Websites, November 2025](https://w3techs.com/technologies/overview/operating_system)

## L’impact sur l’industrie

Windows Server a standardisé les infrastructures, encouragé l’adoption massive et poussé l’innovation, avec une intégration forte à l’écosystème Microsoft (Office, Azure...).

## Conclusion

Windows Server a connu une transformation remarquable, s’adaptant aux besoins changeants du secteur IT, devenant un pilier des datacenters.
