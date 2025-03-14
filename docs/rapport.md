# Rapport de projet long
**Auteurs :**
[Mathieu Benzerrouk](https://www.linkedin.com/in/mathieu-benzerrouk/),
[Yohan Testemale](https://www.linkedin.com/in/yohan-testemale/),
[Robin Augereau](https://www.linkedin.com/in/robin-augereau/),
étudiants à [TLS-SEC](https://tls-sec.github.io/), promotion 2024-2025.

---

## Intitulé du projet
**Titre :** Création d'une architecture de cloud hybride Zero Trust avec une distribution personnalisée.

**Description :**
Ce projet a pour objectif de concevoir et déployer une infrastructure cloud hybride sécurisée, basée sur le modèle Zero Trust. L’accent est mis sur le développement d’une distribution Debian adaptée aux environnements professionnels, intégrant les services nécessaires au bon fonctionnement des applications métiers tout en garantissant une sécurité renforcée.

**Éléments clés :**

1. **Développement d’une distribution personnalisée :**
   - Intégration de logiciels professionnels (suite bureautique, messagerie instantanée, client VPN, etc.).
   - Configuration des services essentiels pour répondre aux besoins d’un environnement d’entreprise.

2. **Mise en place d’une infrastructure Kubernetes :**
   - Déploiement d’une architecture cloud hybride, orchestrée via Kubernetes pour assurer scalabilité, résilience et gestion centralisée.
   - Intégration des principes Zero Trust :
     - Authentification forte et continue.
     - Micro-segmentation pour limiter les privilèges.
     - Surveillance active des flux réseau et des activités utilisateurs.

3. **Gestion des sauvegardes et de la reprise après sinistre :**
   - Mise en place de solutions de sauvegarde des données et configurations, avec des objectifs de temps de retour (RTO) et de points de reprise (RPO) clairement définis.
   - Simulation de scénarios de panne pour valider les procédures de reprise.

---

## Choix de la distribution utilisateur

### Cahier des charges
Les fonctionnalités à intégrer dans la distribution incluent :
- Suite bureautique.
- Messagerie instantanée et client mail.
- Calendrier intégré avec la messagerie.
- Client de synchronisation de fichiers (Drive) avec interface web.
- Authentification Linux sous PAM, avec authentification forte et hors ligne.
- Client de gestion de flotte automatisée.
- Logiciel de sécurité intégré (XDR).
- Client VPN.
- Navigateur avec bloqueur de publicités.
- Firewall local.

> **Interrogation :** Faut-il privilégier des clients légers ou lourds ? Une consultation auprès d’un échantillon de professionnels pourrait éclairer ce choix.

### Comparatif des distributions

| Distribution         | Avantages                                                                 | Inconvénients                                                                 |
|----------------------|---------------------------------------------------------------------------|-------------------------------------------------------------------------------|
| **Debian**           | - Stabilité élevée <br> - Large communauté et documentation <br> - Logiciels bien testés | - Logiciels parfois anciens (cycle de release long) <br> - Moins adapté aux dernières technologies |
| **NixOS**            | - Gestion de paquets reproductible <br> - Configuration déclarative <br> - Isolation des paquets | - Courbe d'apprentissage élevée <br> - Documentation moins accessible pour les débutants |
| **ParrotOS (Home)**  | - Orienté sécurité et vie privée <br> - Léger et rapide <br> - Basé sur Debian (stabilité) | - Moins adapté pour un usage généraliste <br> - Moins de logiciels grand public préinstallés |
| **AlpineLinux**      | - Très léger et rapide <br> - Idéal pour les conteneurs et les systèmes légers <br> - Sécurité renforcée | - Environnement minimaliste (peu convivial pour les débutants) <br> - Moins de logiciels disponibles <br> - Pas d'environnement de bureau intégré |
| **Ubuntu**           | - Très populaire et bien documenté <br> - Support à long terme (LTS) <br> - Large logithèque | - Moins léger que d'autres distributions <br> - Dépendances parfois complexes <br> - Impossible de distribuer une version modifiée |

### Prise en main des distributions

#### **Alpine Linux**
Alpine Linux est reconnu pour sa légèreté et sa simplicité. Cependant, lors de l’installation d’un environnement de bureau, nous avons constaté un temps d’installation long (plus de 10 minutes) et un téléchargement massif de paquets, ce qui alourdit le système. De plus, l’absence de **systemd** complique la gestion des services et l’installation de certains logiciels.

#### **Parrot OS (Home Edition)**
Parrot OS Home est une distribution polyvalente, alliant esthétisme et convivialité, avec des fonctionnalités axées sur la sécurité et la vie privée. Basée sur Debian, elle offre une bonne stabilité. Cependant, il serait intéressant d’extraire ses composants essentiels pour créer une version plus légère et personnalisée directement depuis Debian.

#### **NixOS**
NixOS se distingue par sa configuration déclarative : via le fichier *configuration.nix*, il est possible de décrire l’ensemble du système (logiciels, services, paramètres de sécurité, etc.). Cette approche permet une installation homogène et reproductible sur l’ensemble des postes d’une entreprise, simplifiant ainsi leur gestion. Elle offre également des options d’isolation renforcées et un mécanisme de *rollback* pour revenir à une configuration antérieure en cas d’erreur.

Cependant, NixOS présente une courbe d’apprentissage élevée, avec une syntaxe et une gestion des paquets différentes des distributions classiques, ce qui pourrait compliquer sa maintenance pour des administrateurs IT habitués aux méthodes traditionnelles.

Après plusieurs tests, nous avons finalement opté pour **NixOS** comme système d’exploitation.

---

## Justification de l'architecture Cloud

### Pourquoi un cloud hybride ?
Historiquement, les entreprises utilisaient des clouds privés ("on premises"). Avec l’émergence des clouds publics (AWS, Azure, GCP), une migration partielle des services s’est opérée. Le cloud hybride combine les avantages des deux modèles :
- Données sensibles stockées sur le cloud privé.
- Applications nécessitant une scalabilité hébergées sur le cloud public.
- Établissement d’un tunnel IPSEC entre les deux clouds pour sécuriser les échanges.

### Pourquoi s'orienter vers des micro-services ?
Les applications monolithiques, traditionnellement hébergées sur des VMs, ont cédé la place aux conteneurs (Docker) pour leurs performances, scalabilité et isolation. Les micro-services structurent une application en un ensemble de services faiblement couplés, mais leur gestion reste complexe (orchestration, monitoring, logging, sécurité, etc.).

**Kubernetes** a été choisi pour l’orchestration des micro-services dans le cloud privé.

### Cahier des charges
Les services Kubernetes à intégrer incluent :
- Monitoring (Grafana).
- Logging (Prometheus & Loki).
- Sauvegarde (Velero).
- Sécurité (Kyverno, Falco, Trivy, Kubescape, Cilium).
- Gestion des identités (Kanidm).
- Registre d’images (Harbor).
- Registre de paquets Node/Java.
- Services essentiels à la distribution utilisateur.

---

## Choix de la distribution serveur (pour Kubernetes)

### Comparatif des distributions

| Distribution   | Avantages                                                                 | Inconvénients                                                                 |
|----------------|---------------------------------------------------------------------------|-------------------------------------------------------------------------------|
| **Talos**      | - Conçu spécifiquement pour Kubernetes <br> - Sécurité renforcée (OS immutable) <br> - Gestion simplifiée via API | - Moins polyvalent (uniquement pour Kubernetes) <br> - Courbe d’apprentissage pour la configuration |
| **Debian**     | - Polyvalent et stable <br> - Large communauté et documentation <br> - Compatible avec de nombreux logiciels | - Configuration manuelle nécessaire pour Kubernetes <br> - Moins optimisé pour Kubernetes |

### Prise en main des distributions

#### **Talos**
Talos est conçu spécifiquement pour Kubernetes, avec une sécurité renforcée (OS immutable) et une gestion simplifiée via API. L’utilisation d’[Omni](https://github.com/siderolabs/omni) permet de gérer les VMs et de créer des clusters Kubernetes de manière automatisée. Talos est également très économe en ressources (environ 300 Mo de RAM en idle).

#### **Debian**
Debian, bien que polyvalent et stable, nécessite une configuration manuelle pour Kubernetes, ce qui le rend moins optimisé pour ce cas d’usage.

Notre choix s’est finalement porté sur **Talos**.

---

## Travail réalisé

### Partie cloud
- Achat du nom de domaine **myr-project.eu** et configuration des enregistrements DNS pour rediriger les sous-domaines vers les services hébergés.
- Utilisation de [Helm](https://helm.sh/) pour déployer les services Kubernetes.
- Services hébergés avec succès : Prometheus, Velero, Cert-Manager, CloudNativePostgreSQL, Element, Grafana, Harbor, Kanidm, Loki, Matrix, Minio, Stalwart-Mail, Traefik, Vaultwarden, Wiki, Wireguard.

### Partie distribution
- Installation de packages NixOS : Iptables, Wget, Vim, Git, CurlWithGnuTls, Gnumake, Sudo, Picocrypt, Kanidm_1_5, Openvpn3, Onlyoffice-desktopeditors, Unzip, Binutils.
- Mise en place d’une mise à jour automatique de la configuration via un dépôt GitLab privé.
- Préinstallation d’extensions Firefox (uBlock Origin, Bitwarden) et création de raccourcis bureau pour Picocrypt et OnlyOffice.
- Configuration d’un firewall pour limiter les accès aux ports essentiels (HTTP, HTTPS, SSH, LDAP, LDAPS, DNS).

**Problèmes rencontrés :**
- L’installation de Wazuh a échoué en raison de problèmes de hash et de dépendances.
- Les alternatives (Ossec) ont également posé des problèmes techniques.

## Défis techniques

### Manque de maturité de certains projets
Plusieurs projets rencontrés lors de ce projet étaient prometteurs mais manquaient de maturité. Les principales difficultés étaient le manque de documentation et l’absence de certaines fonctionnalités clés, comme l’authentification SAML, un standard de l’industrie.

La découverte de la **Cloud Native Computing Foundation (CNCF)** a été un tournant. Cette fondation regroupe les projets les plus matures et les plus utilisés dans l’écosystème cloud. S’appuyer sur des projets certifiés CNCF dès le début aurait permis de gagner du temps et d’éviter des problèmes techniques.

### Manque de temps
Ce projet, bien que passionnant, s’est avéré très chronophage. La courbe d’apprentissage de Kubernetes et de son écosystème a nécessité un temps considérable. De plus, beaucoup de temps a été consacré à résoudre des problèmes techniques mineurs mais bloquants, et certaines tâches, comme la configuration fine des services ou l’intégration de solutions de sécurité, ont pris plus de temps que prévu.

Une meilleure estimation des tâches et une priorisation plus stricte auraient permis d’avancer plus efficacement.


### Faible documentation de certains projets
Certains projets open source, bien que techniquement intéressants, souffraient d’une documentation insuffisante ou peu claire. Cela a rendu leur prise en main et leur intégration plus difficile que prévu. Privilégier les projets bien documentés et disposant d’une communauté active est essentiel pour garantir une mise en œuvre rapide et fiable.

---

## Zero Trust : 5 principes clés

| Principe   | Applications
|----------------|---------------------|
|**Gouvernance améliorée de l’identité**|Kanidm a été utilisé pour l’authentification et l’autorisation, tandis que des politiques restrictives ont été mises en place dans Kubernetes pour limiter les accès.|
|**Cloisonnement des ressources**|Le choix d’une architecture cloud hybride a permis de cloisonner les ressources, avec des restrictions d’accès basées sur l’adresse IP pour renforcer la sécurité.|
|**Utilisation des moyens d’authentification à l’état de l’art**|Kanidm propose WebAuthn pour une authentification forte (MFA), et Omni utilise GPG pour authentifier les administrateurs des machines.|
|**Configuration à l’état de l’art quant à la sécurité des services**|Omni utilise mTLS pour sécuriser son API, et Traefik fournit du TLS v1.3 pour tous les services hébergés, garantissant une communication sécurisée.|
|**Conduite du changement à ne pas délaisser**|La mise en œuvre des principes Zero Trust nécessite une formation et une sensibilisation des équipes, une responsabilité qui incombe aux entreprises.

---

## Conclusion
Ce projet a permis de mettre en place une infrastructure cloud hybride sécurisée et une distribution personnalisée adaptée aux besoins professionnels. Il a également été l’occasion de découvrir et de maîtriser des technologies clés utilisées en entreprise, comme **Kubernetes**, qui seront un atout pour une future embauche.

Les points forts incluent la réussite du déploiement d’une architecture cloud hybride Zero Trust, l’intégration de services essentiels (Kanidm, Traefik, Prometheus, etc.), et la création d’une distribution personnalisée basée sur NixOS.

Cependant, des améliorations sont possibles, comme privilégier les projets certifiés CNCF pour gagner en maturité et en stabilité, mieux estimer le temps nécessaire pour chaque tâche, et renforcer la documentation interne pour faciliter la maintenance et l’évolution du projet.

Ce projet a été une expérience enrichissante, tant sur le plan technique que sur le plan organisationnel, et ouvre la voie à de nombreuses perspectives d’amélioration et d’optimisation.
