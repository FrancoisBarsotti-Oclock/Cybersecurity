# 🪟☁️ Session A413. Azure & Cloud.

## Azure
C’est la **plateforme cloud de Microsoft** (un service payant sur le Cloud). Elle permet de **louer des ressources informatiques** (serveurs, stockage, bases de données, réseaux, etc.) au lieu de les acheter ou de les héberger sur site.

**En résumé :**

Azure = un **datacenter géant** de Microsoft, accessible via Internet, où tu peux :

- **Créer des machines virtuelles (VM)**
- **Héberger des sites web et applications**
- **Stocker des fichiers ou bases de données**
- **Configurer des réseaux virtuels et des pare-feux**
- **Mettre en place de la cybersécurité** (IAM, sauvegardes, SOC, etc.)

**Ce qu’il faut savoir (bases importantes) :**

1. **Types de services (3 grands modèles) :**

- **IaaS (Infrastructure as a Service)** → tu gères tout : VM, OS, réseau.
- **PaaS (Platform as a Service)** → Microsoft gère la plateforme (tu déploies ton appli).
- **SaaS (Software as a Service)** → tu utilises une appli prête (ex : Microsoft 365).

3. **Régions et zones** : les ressources sont hébergées dans des datacenters du monde entier (ex : France Centre, France Sud).
4. **Comptes et ressources** :

- Tout se gère dans le **Portail Azure** (interface web).
- Chaque ressource (VM, réseau, stockage) appartient à un **groupe de ressources**.

6. **Réseaux Azure** :

- VNet (Virtual Network)
- Subnets, NSG (Network Security Group), VPN Gateway, Bastion…

8. **Sécurité** :

- Azure AD (gestion des identités et accès)
- RBAC (rôles et permissions)
- Defender for Cloud, Sentinel (SIEM/SOAR), Key Vault…

10. **Facturation** :

- Paye à l’usage (CPU, RAM, stockage, bande passante).

Les principales plateformes cloud concurrentes d’**Azure** sont :

**🔹 Les “Big 3” (leaders mondiaux)**

1. **Amazon Web Services (AWS)**

- Le plus ancien et le plus complet.
- Très utilisé pour les infrastructures, DevOps, et big data.
- Services célèbres : EC2 (VM), S3 (stockage), RDS (base de données), IAM (sécurité).

3. **Microsoft Azure**

- Très intégré à l’environnement Windows/Active Directory.
- Fort en services hybrides (cloud + local).
- Populaire chez les entreprises déjà clientes de Microsoft.

5. **Google Cloud Platform (GCP)**

- Réputé pour ses performances et son IA.
- Idéal pour l’analyse de données et le machine learning.
- Services connus : Compute Engine, BigQuery, Vertex AI.

---

**🔹 Autres acteurs importants**

4. **Oracle Cloud Infrastructure (OCI)** – spécialisé bases de données et entreprises Oracle.
5. **IBM Cloud** – orienté entreprise, IA, et sécurité.
6. **Alibaba Cloud** – leader asiatique, fort en e-commerce et big data.
7. **OVHcloud (français)** – cloud souverain européen, conforme RGPD, hébergement en France.
8. **Scaleway (français aussi)** – plus petit, simple à utiliser, bon rapport qualité/prix.

Voici un **tableau comparatif clair** des principales plateformes cloud :
![[Pasted image 20251208230913.png]]
# Cloud
Le terme « cloud » désigne les serveurs accessibles sur Internet, ainsi que les logiciels et bases de données qui fonctionnent sur ces serveurs. Les serveurs situés dans le cloud sont hébergés au sein de [datacenters](https://www.cloudflare.com/learning/cdn/glossary/data-center/) répartis dans le monde entier. L'utilisation de l'informatique cloud (cloud computing) permet aux utilisateurs et aux entreprises de s'affranchir de la nécessité de gérer des serveurs physiques eux-mêmes ou d'exécuter des applications logicielles sur leurs propres équipements : [Qu’est-ce que le cloud ? | Définition cloud | Cloudflare](https://www.cloudflare.com/fr-fr/learning/cloud/what-is-the-cloud/#:~:text=Le%20terme%20%C2%AB%20cloud%20%C2%BB%20d%C3%A9signe%20les,r%C3%A9partis%20dans%20le%20monde%20entier.)
![[Pasted image 20251208230955.png]]
Le cloud permet aux utilisateurs d'accéder aux mêmes fichiers et aux mêmes applications à partir de pratiquement n'importe quel appareil, car les processus informatiques et le stockage s'effectuent sur des serveurs au sein d'un datacenter et non localement sur l'appareil de l'utilisateur. C'est pourquoi un utilisateur dont le téléphone est défaillant peut se connecter à son compte Instagram à partir d'un nouveau téléphone et retrouver son compte actif, avec toutes ses photos, vidéos et l'historique de ses conversations. Il en va de même avec les fournisseurs de [messagerie cloud](https://www.cloudflare.com/learning/email-security/what-is-email/) comme Gmail ou Microsoft Office 365 et les fournisseurs de [stockage cloud](https://www.cloudflare.com/learning/cloud/what-is-cloud-storage/) comme Dropbox ou Google Drive.

Pour les entreprises, l’adoption de l’informatique cloud élimine certains coûts informatiques et frais généraux : c’est ainsi, qu’elles n’ont plus besoin de mettre à jour ni de maintenir leurs propres serveurs, le fournisseur de services dans le cloud auquel elles font appel s’en chargeant. Ceci, en particulier, a des répercussions pour les [petites entreprises](https://www.cloudflare.com/small-business/), lesquelles peuvent ne pas être en mesure financièrement de disposer de leur propre infrastructure en interne, mais ont ainsi la possibilité de sous-traiter leurs besoins en la matière d’une manière économique, par le biais du cloud. Le cloud peut également faciliter le fonctionnement à l’international pour les entreprises, car les salariés et les clients peuvent accéder aux mêmes dossiers et applications à partir de n’importe quel emplacement.
## Comment fonctionne l'informatique cloud ?
L'informatique cloud est rendue possible grâce à une technologie dénommée virtualisation. La virtualisation permet la mise en place d'une simulation d'ordinateur « virtuel », uniquement numérique, qui se comporte en tout point comme un ordinateur physique doté de ses équipements physiques propres. En termes techniques, on désigne ce type d'ordinateur sous le nom de [machine virtuelle](https://www.cloudflare.com/learning/cloud/what-is-a-virtual-machine/). Lorsqu'elles sont correctement mises en œuvre, les machines virtuelles situées sur le même ordinateur hôte se voient mises en « sandbox » (c'est-à-dire isolées), de sorte qu'elles n'interagissent pas du tout les unes avec les autres. Les fichiers et les applications figurant sur une machine virtuelle donnée ne sont alors pas visibles par les autres machines, même si ces dernières se trouvent sur la même machine physique.

Les machines virtuelles utilisent également plus efficacement les équipements physiques qui les hébergent. En exécutant simultanément plusieurs machines virtuelles, un serveur devient plusieurs serveurs et un datacenter une multitude de datacenters, capables de servir les besoins de plusieurs entreprises. Les fournisseurs de cloud peuvent ainsi proposer l'utilisation de leurs serveurs à un nombre de clients bien plus important qu'ils ne le pourraient autrement, et ce à un coût modéré.

En règle générale, les serveurs cloud se doivent d'être toujours en ligne et toujours disponibles, même si certains serveurs individuels tombent en panne. Les fournisseurs de cloud sauvegardent habituellement leurs services sur plusieurs machines et dans plusieurs régions.

Les utilisateurs accèdent aux services cloud par le biais d'un navigateur ou d'une application qui se connecte au cloud via Internet (c'est-à-dire, par l'intermédiaire de nombreux réseaux interconnectés), indépendamment des appareils qu'ils utilisent.
## Que sont les services cloud ?
Les ressources disponibles dans le cloud sont connues sous le nom de « services », car elles sont gérées activement par un fournisseur de cloud. Les services cloud comprennent l'infrastructure, les applications, les outils de développement et le stockage de données, entre autres produits. Ces services se répartissent en différentes catégories ou _modèles de services_.
## Quels sont les trois modèles de services de l'informatique cloud ?
![[Pasted image 20251208231029.png]]
**Software-as-a-Service (SaaS)** : plutôt que de faire installer une application aux utilisateurs sur leurs appareils, les applications [SaaS](https://www.cloudflare.com/learning/cloud/what-is-saas/) sont hébergées sur des serveurs cloud et les utilisateurs y accèdent via Internet. Les services SaaS s'apparentent à la location d'une habitation : le propriétaire entretient l'habitation, tandis que le locataire l'utilise comme si elle lui appartenait. Parmi les exemples d'applications SaaS, on citera Salesforce, MailChimp et Slack.

**Platform-as-a-Service (PaaS)** : dans ce modèle, les entreprises ne paient pas pour leurs applications hébergées. Elles paient les composants dont elles ont besoin pour développer leurs propres applications. Les fournisseurs de [PaaS](https://www.cloudflare.com/learning/serverless/glossary/platform-as-a-service-paas/) proposent tous les outils nécessaires à la conception d'une application par Internet, notamment les outils de développement, l'infrastructure et les systèmes d'exploitation. L'approche PaaS peut être comparée à la location des outils et des équipements nécessaires à la construction d'une maison plutôt que de louer la maison elle-même. Heroku et Microsoft Azure sont de bons exemples de services PaaS.

**Infrastructure-as-a-Service (IaaS)** : dans ce modèle, une entreprise loue les serveurs et l'espace de stockage dont elle a besoin à un fournisseur de cloud. Elle peut alors utiliser cette infrastructure cloud pour développer ses propres applications. L'approche [IaaS](https://www.cloudflare.com/learning/cloud/what-is-iaas/) s'apparente à la location d'un terrain par une entreprise : cette dernière peut y construire tout ce qu'elle souhaite, mais elle doit fournir ses propres équipements et matériaux de construction. Parmi les fournisseurs de services IaaS, on citera DigitalOcean, Google Compute Engine et OpenStack.

Auparavant, le SaaS, le PaaS et l'IaaS étaient les trois principaux modèles d'informatique cloud. Pratiquement tous les services cloud relevaient de l'une de ces catégories. Toutefois, ces dernières années ont vu émerger un quatrième modèle :

**Function-as-a-Service (FaaS)** : le modèle [FaaS](https://www.cloudflare.com/learning/serverless/glossary/function-as-a-service-faas/) (également connu sous le nom d'[informatique serverless](https://www.cloudflare.com/learning/serverless/what-is-serverless/)) divise les applications cloud en composants plus petits, uniquement exécutés en cas de besoin. Imaginez qu'il soit possible de louer une maison une pièce à la fois. Le locataire ne louerait, par exemple, que la salle à manger au moment des repas, la chambre quand il va dormir et le salon quand il regarde la télévision, sans avoir à régler le loyer correspondant lorsqu'il n'utilise pas ces pièces.

À l'instar de tous les autres modèles d'informatique cloud, les applications FaaS ou serverless s'exécutent néanmoins toujours sur des serveurs. Toutefois, on les définit comme « serverless », car elles ne s'exécutent pas sur des machines dédiées. Les entreprises qui développent ces applications n'ont ainsi aucun serveur à gérer.

Par ailleurs, les fonctions serverless peuvent évoluer ou être dupliquées lorsqu'un nombre plus élevé d'utilisateurs se servent de l'application. Imaginez que la salle à manger du locataire puisse s'agrandir à la demande lorsque de nouveaux convives se joignent à lui pour le dîner ! [En savoir plus sur l'informatique serverless (FaaS)](https://www.cloudflare.com/learning/serverless/what-is-serverless/).
## Qu'est-ce que l'infrastructure cloud ?
L'infrastructure cloud désigne les ressources nécessaires à l'hébergement et au développement d'applications cloud. Les services IaaS et PaaS sont souvent intégrés à l'infrastructure cloud d'une entreprise, même si l'on peut dire que le SaaS fait également partie de l'infrastructure cloud. Le FaaS, quant à lui, offre la possibilité de bâtir une infrastructure en tant que code.
## Quels sont les différents types de déploiements cloud ?
Contrairement aux modèles évoqués ci-dessus, qui définissent les modalités selon lesquelles les services sont proposés par l'intermédiaire du cloud, ces différents types de déploiement cloud ont trait au lieu où les serveurs se trouvent et à l'identité de ceux qui les gèrent.

Les déploiements cloud les plus courants sont les suivants :

- enter ou un réseau distribué, intégralement dédié à une entreprise.
- **Cloud public** : le terme [cloud public](https://www.cloudflare.com/learning/cloud/what-is-a-public-cloud/) désigne un service géré par un fournisseur externe et pouvant inclure des serveurs situés dans un ou plusieurs datacenters. Contrairement aux clouds privés, les clouds publics sont partagés par de nombreuses entreprises. L'utilisation de machines virtuelles permet de partager des serveurs indépendants entre différentes entreprises. On parle alors d'« [architecture mutualisée](https://www.cloudflare.com/learning/cloud/what-is-multitenancy/) », car plusieurs locataires louent de l'espace serveur au sein du même serveur.
- **Cloud hybride** : les déploiements [cloud hybrides](https://www.cloudflare.com/learning/cloud/what-is-hybrid-cloud/) associent des clouds publics et privés. Ils peuvent même inclure des serveurs traditionnels sur site. Une entreprise peut utiliser son cloud privé pour certains services et son cloud public pour d'autres, à moins qu'elle ne préfère conserver son cloud public comme solution de secours en cas de défaillance de son cloud privé.
- **Multicloud** : l'approche [multicloud](https://www.cloudflare.com/learning/cloud/what-is-multicloud/) constitue un type de déploiement cloud impliquant l'utilisation de plusieurs clouds publics. En d'autres termes, une entreprise qui s'appuie sur un déploiement multicloud loue des serveurs et des services virtuels auprès de plusieurs fournisseurs externes (pour reprendre l'analogie utilisée ci-dessus, l'opération revient à louer plusieurs terrains adjacents auprès de différents propriétaires). Les déploiements multicloud peuvent également concerner des clouds hybrides, et vice-versa.

### Comment Cloudflare aide les entreprises à passer au cloud et à opérer dans le cloud ?

Cloudflare permet de protéger et de gérer n'importe quel type de déploiement cloud. Notre réseau se situe entre les utilisateurs finaux et l'infrastructure cloud du produit ou du service du client. Les clients peuvent gérer les [performances](https://www.cloudflare.com/learning/performance/why-site-speed-matters/), la [sécurité](https://www.cloudflare.com/learning/security/what-is-web-application-security/), le [DNS](https://www.cloudflare.com/learning/dns/what-is-dns/) et d'autres offres Cloudflare pour l'ensemble de leurs déploiements cloud à partir d'un tableau de bord unique. Cloudflare propose un [pare-feu applicatif web](https://www.cloudflare.com/learning/ddos/glossary/web-application-firewall-waf/) pour protéger les propriétés Internet contre les exploitations de vulnérabilités. Le réseau Cloudflare permet également aux entreprises d'intégrer facilement les services FaaS (serverless) au sein de leur déploiement cloud.

Découvrez les [solutions Cloudflare pour SaaS](https://www.cloudflare.com/saas/) et comment un [cloud de connectivité](https://www.cloudflare.com/connectivity-cloud/) peut s'intégrer facilement à n'importe quel type de déploiement cloud.
## En quoi le cloud diffère-t-il du modèle traditionnel client-serveur d'Internet ?
Depuis ses débuts, Internet se compose de serveurs, de clients et de l'infrastructure qui les relient. Les clients adressent des requêtes aux serveurs, qui leur renvoient des réponses. L'informatique cloud diffère de ce modèle en ce que les serveurs situés dans le cloud ne se limitent pas à répondre aux requêtes : ils exécutent également des programmes et stockent des données pour le compte du client.
## Pourquoi le cloud s'appelle-t-il ainsi ?
Le « cloud », littéralement le nuage, est un terme hérité du jargon technique. Aux débuts d'Internet, les diagrammes techniques représentaient souvent les serveurs et l'infrastructure réseau qui composent Internet sous la forme de nuages. Lorsque de plus en plus de processus informatiques ont migré vers cette partie « serveurs et infrastructure » d'Internet, l'expression « dans le nuage » était une manière abrégée de désigner l'endroit où les processus informatiques se déroulaient. Aujourd'hui, le terme « cloud » est largement accepté pour décrire ce type d'informatique.
### Qu'en est-il des conteneurs ? Les conteneurs relèvent-ils de l'IaaS, du PaaS, du SaaS ou du FaaS ?
