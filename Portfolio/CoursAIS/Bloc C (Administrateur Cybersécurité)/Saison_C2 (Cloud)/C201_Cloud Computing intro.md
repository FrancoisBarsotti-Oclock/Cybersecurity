# ☁️ Session C201. Introduction au Cloud Computing 

**Notions du jour**
* On-Premises vs. Iaas vs. PaaS vs. SaaS (et tout ce que ça implique !)
* réversibilité & responsabilité
* garanties contractuelles

## Bienvenue
Avant de démarrer, petit test rapide.
Quiz express : qui utilise déjà ...
* Gmail ou Outlook.com ?
* Netflix ou Spotify ?
* Office 365 ?
* Dropbox ou Google Drive ?

**➡️ Vous utilisez déjà le cloud, sans le savoir.**

## Partie 1 : Évolution vers le Cloud

_Comment en est-on arrivé là ?_

### Il était une fois ... (années 1990-2000)
**Le modèle traditionnel** :

🏢 Entreprise = Salle serveurs

* Acheter des serveurs physiques
* Installer dans une salle climatisée
* Câbler réseau, électricité, onduleurs
* Installer OS, applications
* Maintenir, patcher, remplacer

💰 **Investissement initial énorme** 
⏱️ **Délais de plusieurs semaines/mois**

### Les problèmes du On-Premises

**Scénario typique** :

Vous lancez un site e-commerce ...

* Combien de serveurs acheter ?
* Si succès -> serveurs saturés
* Si échec -> argent gaspillé
* Black Friday -> site down
* Janvier -> serveurs inutilisés

➡️ **Impossible de s'adapter rapidement.**

### La révolution Cloud (2006-aujourd'hui)

**2006** : Amazon lance AWS (EC2)
* Louer de la puissance de calcul à l'heure
* Payer uniquement ce qu'on utilise
* Créer/détruire des serveurs en minutes

**L'idée** : L'infrastructure comme l'électricité
* On ne produit pas son électricité
* On la consomme à la demande
* On paie ce qu'on consomme

💡 **Infrastructure as a Service (IAAS) était né**

### Les 5 caractéristiques du Cloud (NIST)
1. **Accès réseau large bande**
* Accessible via Internet
* Depuis n'importe où, n'importe quel appareil

2. **Mise en commun des ressources**
* Ressources partagées entre clients
* Isolation logique garantie

3. **Libre-service à la demande**
* Provisionner sans intervention humaine
* Self-service portail

4. **Élasticité rapide**
* Scale up/down automatiquement
* Adapter aux pics de charge
* Exemple : site e-commerce Black Friday

5. **Service mesuré**
* Métriques et monitoring
* Facturation à l'usage
* Pay-as-you-go

➡️ **Si ça n'a pas ces 5 caractéristiques, ce n'est pas du cloud.**

## Partie 2 : On-Premises

_Le modèle traditionnel_

### Qu'est-ce que le On- Premises ?

**On-Prem** = Sur site, dans vos locaux

**Vous gérez TOUT**:
* 🏢 Espace physique (salle serveurs)
* 🔌 Électricité, climatisation
* ️💻 Serveurs et stockage
* 🌐 Réseau et firewall
* 💿 OS et applications
* 🔒 Sécurité physique et logique
* 💾 Sauvegardes
* 🔧 Maintenance et mises à jour

### Exemple concret : Serveur web On-Prem

**Besoin** : Héberger un site web d'entreprise

**Étapes nécessaires** :
1. Acheter un serveur Dell/HP (~3000€)
2. Installer dans la salle serveurs
3. Configurer réseau, IP publique
4. Installer Linux/Windows Server
5. Installer Apache/IIS
6. Configurer HTTPS, certificats
7. Mettre en place sauvegardes
8. Surveiller, patcher, maintenir

⏱️ **Délai** : 2-4 semaines
💰  **Coût initial** : 5000-10000€

 ### Avantages On-Premises

✅ **Contrôle total**
* Vous décidez de tout
* Aucune dépendance externe

✅ **Sécurité physique maîtrisée**
* Serveurs dans vos murs
* Pas de partage avec d'autres

✅ **Performances prévisibles**
* Ressources dédiées
* Pas de "voisins bruyants"

✅ **Pas de couts récurrents cloud**
* Investissement initial puis maintenance

### Inconvénients On-Premises

❌ **Investissement initial élevé (CAPEX)** 
* Serveurs, stockage, réseau
* Salle serveurs aménagée

❌ **Scalabilité lente**
* Acheter nouveau matériel = semaines
* Impossible de réduire les ressources

❌ **Maintenance lourde**
* Patchs OS, firmware
* Remplacements matériels
* Surveillance 24/7

❌ **Risques de sous/sur-dimensionnement**
* Trop petit -> saturation
* Trop gros - gaspillage

### Quand choisir On-Premises ?

#### Cas d'usage pertinents :

🏥 **Données ultra-sensibles**
* Secteur santé, défense
* Conformité stricte

🏭 **Applications legacy**
* Logiciels propriétaires anciens
* Impossibles à migrer

💰 **Usage prévisible et constant**
* Pas de variations de charge
* Amortissement long terme

🌍 **Zones sans Internet fiable**
* Sites isolés
* Contraintes géographiques

👉 [La CNIL a sanctionné FRANCE TRAVAIL pour ne pas avoir assuré la sécurité des données des personnes en recherche d'emploi](https://cnil.fr/fr/violation-de-donnees-sanction-5millions-france-travail#:~:text=Le%2022%20janvier%202026%2C%20la,personnes%20en%20recherche%20d'emploi)

## Partie 3 : IaaS 

### _Infrastructure as a Service_

### Qu'est-ce que l'IaaS ?

_Infrastructure en tant que Service_

Le provider cloud vous fournit :
* Serveurs virtuels (VMs)
* Stockage
* Réseau
* Équipements physiques

**Vous gérez** :
* Système d'exploitation
* Middleware
* Applications
* Données

### IaaS : La fondation du Cloud

```apache
┌─────────────────────────────────────┐
│ VOS RESPONSABILITÉS                 │
├─────────────────────────────────────┤
│ Applications                        │
│ Données                             │
│ Runtime (Java, .NET, etc.)          │
│ Middleware (serveur web, etc.)      │
│ OS (Windows, Linux)                 │
├─────────────────────────────────────┤
│ RESPONSABILITÉS PROVIDER            │
├─────────────────────────────────────┤
│ Virtualisation                      │
│ Serveurs physiques                  │
│ Stockage                            │
│ Réseau                              │
│ Datacenter (électricité, etc.)      │
├─────────────────────────────────────┤
```

## Exemples de services IaaS

### AWS :
* EC2 (Elastic Compute Cloud)
* EBS (Elastic Block Store)
* VPC (Virtual Private Cloud)
### Google Cloud :
* Compute Engine
* Persistent Disk

### OVHcloud :
* Public Cloud Instances
* Block Storage

## Cas d'usage IaaS

### Migration "Lift & Shift" :
* Déplacer serveurs on-prem->cloud
* Sans modifier l'application
* VM cloud = serveur physique
### Environnements de dev/test :
* Créer/détruire facilement
* Coûts optimisés

### Applications nécessitant contrôle OS :
* Configurations spécifiques
* Logiciels nécessitant admin complet

### Disaster Recovery :
* Infrastructure de secours à la demande

### Avantages IaaS
✅ **Pas d'investissement matériel**
* OPEX vs CAPEX
* Coûts mensuels prévisibles

✅ **Scalabilite rapide**
* Ajouter des VMs en minutes
* Auto-scaling possible

✅ **Haute disponibilité**
* SLA 99.9%+ garantis
* Redondance intégrée

✅ **Flexibilité**
* Contrôle de l'OS
* Configurations personnalisées

### Inconvénients IaaS

❌ **Gestion OS restante**
* Patchs Windows/Linux
* Mises à jour de sécurité
* Configuration et hardening

❌ **Compétences système requises**
* Administrateurs sys/réseau
* Connaissances techniques

❌ **Coûts potentiellement élevés**
* Si mal dimensionné
* VMs allumées inutilement

❌ **Responsabilité sécurité applicative**
* Firewall, antivirus
* Configurations sécurisées

## Partie 4 : PaaS 

### _Platform as a Service_

### Qu'est-ce que le PaaS ?

**Plateforme en tant que Service**

Le provider cloud gère :

* Infrastructure (serveurs, réseau)
* OS et patches
* Runtime (Java, Python, .NET)
* Middleware
* Haute disponibilité

**Vous gérez uniquement** :
* Votre application
* Vos données

### PaaS : Focus sur le code
```apache
┌─────────────────────────────────────┐
│ VOS RESPONSABILITÉS                 │
├─────────────────────────────────────┤
│ Applications (votre code)           │
│ Données                             │
├─────────────────────────────────────┤
│ RESPONSABILITÉS PROVIDER            │
├─────────────────────────────────────┤
│ Runtime (Java, .NET, Python...)     │
│ Middleware (serveur web, etc.)      │
│ OS (invisible pour vous)            │
│ Virtualisation                      │
│ Serveurs, Stockage, Réseau          │
│ Datacenter                          │
└─────────────────────────────────────┘
```

➡️ **Vous ne voyez même pas l’OS.**

## Exemples de services PaaS

### AWS :
* Elastic Beanstalk
* RDS (Relational Database Service)
* Lambda (serverless)
### Google Cloud :
* App Engine
* Cloud SQL
* Cloud Functions
### Heroku :
* Dyno (containers applicatifs)

## Cas d'usage PaaS
### Applications web modernes :
* Déployer du code, c'est tout
* Framework supporté (Node.js, Python, .NET)

### Bases de données managées :
* MySQL, PostgreSQL, SQL Server
* Sauvegardes automatiques
* Haute disponibilité intégrée

### APIs et microservices :
* Déploiement rapide
* Scaling automatique

### Applications serverless :
* Fonctions à la demande
* Pay per exécution

### Avantages PaaS

✅ **Focus sur le code**
* Pas de gestion d'OS
* Pas de patchs à gérer

✅ **Déploiement ultra-rapide**
* Git push -+ application déployée
* Minutes vs semaines

✅ **Scaling automatique**
* Le provider gère la montée en charge
* Transparent pour vous

✅ **Intégrations natives**
* CI/CD, monitoring, logs
* DevOps simplifié

### Inconvénients PaaS

❌ **Moins de contrôle**
* Pas d'accès à l'OS
* Configurations limitées

❌ **Lock-in potentiel**
* Dépendance aux services du provider
* Migration plus complexe

❌ **Contraintes du runtime**
* Versions imposées
* Pas tous les langages/frameworks

❌ **Coût parfois plus élevé**
* Que laaS pour mêmes ressources
* Mais temps admin économisé

## Partie 5 : SaaS 

### _Software as a Service, genre Netflix_

### Qu'est-ce que le SaaS ?
**Logiciel en tant que Service**

Le provider gère **TOUT** :
* Infrastructure
* OS et middleware
* Application complète
* Mises à jour
* Sauvegardes
* Sécurité

**Vous utilisez simplement** :
* L'application via navigateur/app
* Vos données

### SaaS : Prêt à l'emploi

```apache
┌─────────────────────────────────────┐
│ VOS RESPONSABILITÉS                 │
├─────────────────────────────────────┤
│ Données utilisateur                 │
│ Configuration (paramètres)          │
├─────────────────────────────────────┤
│ RESPONSABILITÉS PROVIDER            │
├─────────────────────────────────────┤
│ Application complète                │
│ Runtime, Middleware                 │
│ OS                                  │
│ Infrastructure complète             │
│ Datacenter                          │
└─────────────────────────────────────┘
```

➡️ **Vous ne gérez que vos utilisateurs et données.**

## Exemples de services SaaS

### Productivité :
* Microsoft 365 (Word, Excel, Teams)
* Google Workspace (Gmail, Docs, Drive)
* Notion, Slack

### CRM & ERP :
* Salesforce
* HubSpot
* SAP S/4HANA Cloud

### Collaboration :
* Zoom, Microsoft Teams
* Dropbox, Box

### Autres :
* Netflix, Spotify (streaming)
* Adobe Creative Cloud
## Cas d'usage SaaS
### Applications métier standards :
* CRM, ERP, RH, Comptabilité
* Pas besoin de personnalisation lourde

### Outils de collaboration :
* Email, visioconférence
* Partage de fichiers

### Applications grand public :
* Streaming, réseaux sociaux
* Services en ligne

### PME sans IT :
* Pas de compétences techniques
* Tout géré par le provider

### Avantages SaaS

✅ **Aucune gestion technique**
* Tout est géré par le provider
* Mises à jour automatiques

✅ **Déploiement immédiat**
* S'inscrire → utiliser
* Minutes, pas semaines

✅ **Coût prévisible**
* Abonnement mensuel par user
* Pas d'investissement initial

✅ **Accessibilité**
* Depuis n'importe où
* N'importe quel device

### Inconvénients SaaS

❌ **Zéro personnalisation technique**
* Application figée
* Configurations limitées

❌ **Dépendance totale au provider**
* Lock-in maximal
* Données chez le provider

❌ **Conformité et souveraineté**
* Où sont les données ?
* Contrôle limité

❌ **Coûts cumulés**
* Abonnement à vie
* Peut dépasser le TCO d'une solution on-prem

## Comparaison des modèles

### _Quel modèle pour quel besoin ?_

### Tableau comparatif
| **Aspect** | **On-Prem** | **IaaS** | **PaaS** | **SaaS** |
| -- | :--: | :--: | :--: | :--: |
| **Contrôle** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ | ⭐ |
| **Simplicité** | ⭐ | ⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Coût initial** | ⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Scalabilité** | ⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Compétences IT** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ | ⭐ |
| **Personnalisation** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐ |
| **Time to market** | ⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |

### Matrice de responsabilité 

| **Composant** | **On-Prem** | **IaaS** | **PaaS** | **SaaS** |
| :--: | :--: | :--: | :--: | :--: |
| **Application** | 👤Vous | 👤Vous | 👤Vous | ☁️Provider |
| **Données** | 👤Vous | 👤Vous | 👤Vous | 👤Vous |
| **Runtime** | 👤Vous | 👤Vous | ☁️Provider | ☁️Provider |
| **Middleware** | 👤Vous | 👤Vous | ☁️Provider | ☁️Provider |
| **OS** | 👤Vous | 👤Vous | ☁️Provider | ☁️Provider |
| **Virtualisation** | 👤Vous | ☁️Provider | ☁️Provider | ☁️Provider |
| **Serveurs** | 👤Vous | ☁️Provider | ☁️Provider | ☁️Provider |
| **Stockage** | 👤Vous | ☁️Provider | ☁️Provider | ☁️Provider |
| **Réseau** | 👤Vous | ☁️Provider | ☁️Provider | ☁️Provider |

