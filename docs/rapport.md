# Rapport de projet long
[Mathieu Benzerrouk](https://www.linkedin.com/in/mathieu-benzerrouk/) &
[Yohan Testemale](https://www.linkedin.com/in/yohan-testemale/) &
[Robin Augereau](https://www.linkedin.com/in/robin-augereau/),
étudiants à [TLS-SEC](https://tls-sec.github.io/), promotion 2024-2025.

## Intitulé du projet
**Titre :** Création d'une architecture de cloud hybride Zero Trust avec une distribution personnalisée.

**Description :**
Ce projet vise à concevoir et mettre en œuvre une infrastructure cloud hybride sécurisée basée sur le modèle Zero Trust. L’objectif est de développer une distribution Debian adaptée à un environnement professionnel et d’intégrer les services nécessaires au fonctionnement sécurisé des applications métiers.

**Eléments clés :**

* Développement d’une distribution personnalisée
  * Intégration des logiciels professionnels tels qu’une suite bureautique, des applications de messagerie (instantanée et e-mails) et un client VPN.
  * Configuration des services essentiels pour répondre aux besoins d’un environnement d’entreprise.

* Mise en place d’une infrastructure Kubernetes
  * Déploiement d’une architecture cloud hybride avec orchestration via Kubernetes pour assurer la scalabilité, la résilience et une gestion centralisée des services.
  * Intégration des principes de sécurité Zero Trust, notamment :
    * Authentification forte et continue.
    * Micro-segmentation pour limiter les privilèges.
    * Surveillance active des flux réseau et des activités des utilisateurs.

* Prise en compte des contraintes de sauvegarde et de récupération
  * Mise en place de solutions de sauvegarde des données et des configurations avec des temps de retour (RTO) et points de reprise (RPO) clairement définis.
  * Simulation de scénarios de panne pour tester et valider les procédures de reprise.

## Choix de la distribution utilisateur

### Cahier des charges

Fonctionnalités à intégrer dans la distribution :
- Suite office
- Messagerie Instantanée
- Client mail spécificités
- Calendrier qui s'intègre avec la messagerie instantanée et le client mail
- Drive : client sur le poste qui synchronise les fichiers et une interface web
- Authentification Linux sous PAM, avec authentification hors ligne et forte
- Client d'un serveur d'automatisation d'une flotte
- Logiciel de sécurité intégré (XDR)
- Client VPN
- Navigateur avec extension bloqueur de pub
- Firewall local

> Interrogation autour des clients légers / lourds : consulation d'un échantillon de profesionnels sur leurs préférences ?


### Comparatif des distributions
| Distribution         | Avantages                                                                 | Inconvénients                                                                 |
|----------------------|---------------------------------------------------------------------------|-------------------------------------------------------------------------------|
| **Debian**           | - Stabilité élevée <br> - Large communauté et documentation <br> - Logiciels bien testés | - Logiciels parfois anciens (cycle de release long) <br> - Moins adapté aux dernières technologies |
| **NixOS**            | - Gestion de paquets reproductible <br> - Configuration déclarative <br> - Isolation des paquets | - Courbe d'apprentissage élevée <br> - Documentation moins accessible pour les débutants |
| **ParrotOS (Home)**  | - Orienté sécurité et vie privée <br> - Léger et rapide <br> - Basé sur Debian (stabilité) | - Moins adapté pour un usage généraliste <br> - Moins de logiciels grand public préinstallés |
| **AlpineLinux**      | - Très léger et rapide <br> - Idéal pour les conteneurs et les systèmes légers <br> - Sécurité renforcée | - Environnement minimaliste (peu convivial pour les débutants) <br> - Moins de logiciels disponibles <br> - Pas d'environnement de bureau intégré |
| **Ubuntu**           | - Très populaire et bien documenté <br> - Support à long terme (LTS) <br> - Large logithèque | - Moins léger que d'autres distributions <br> - Dépendances parfois complexes <br> - Impossible de distribuer une version modifiée |

### Prise en main de certaines distributions

#### **Alpine Linux**
Alpine Linux est une distribution reconnue pour sa légèreté et sa simplicité.
Cependant, lors de l'installation d'un environnement de bureau, nous avons constaté un temps d'installation assez long (plus de 10 minutes) ainsi que le téléchargement d'un grand nombre de paquets.
Cela alourdit considérablement le système, ce qui va à l'encontre de l'objectif principal de la distribution.

De plus, Alpine Linux n'utilise pas **systemd**, le gestionnaire de services responsable de la gestion des sockets, des processus, des points de montage, des états d'énergie, entre autres.
Cette absence peut compliquer l'installation de certains logiciels ainsi que la gestion du système d'exploitation sur un poste de travail.

#### **Parrot OS (Home Edition)**
Parrot OS Home est une distribution polyvalente, combinant esthétisme et convivialité, tout en offrant des fonctionnalités axées sur la protection de la vie privée et le développement logiciel.
Conçue pour répondre à des besoins variés, elle s'appuie sur une base Debian, assurant stabilité et compatibilité.

Néanmoins, il serait intéréssant d'extraire les composants essentiels de Parrot OS Home afin de créer une version plus légère et personnalisée, directement depuis Debian.
Cette flexibilité permet d'adapter la distribution selon les préférences et les exigences de notre cahier des charges de cette distribution.

#### **NixOS**
NixOS est une distribution qui permet une configuration déclarative : en éditant le fichier *configuration.nix*, il est possible de décrire l'ensemble du système souhaité : logiciels installés, services activés, paramètres de sécurité, gestion des utilisateurs, etc. Contrairement aux distributions classiques où les configurations sont appliquées manuellement via des commandes et des fichiers dispersés, ici tout est défini en une seule fois puis appliqué via une reconstruction du système. Cette spécificité permet une installation homogène et reproductible sur l'ensemble des postes d'une entreprise et simplifie grandement leur gestion *(un administrateur peut gérer et modifier toute la flotte d'ordinateurs en ne modifiant qu'un seul fichier)*. Elle offre également des options d'isolation renforcées ainsi qu'un mécanisme de *rollback* qui permet de revenir instantanément à une configuration antérieure en cas d'erreur de configuration.

Le point négatif de cette distribution réside surtout dans l'apprentissage qu'elle nécessite. En effet, **NixOS** possède une syntaxe et une gestion des paquets bien différente des autres distributions Linux et pourrait être compliquée à maintenir pour des administrateurs IT habitués aux méthodes plus classiques.

## Choix de la distribution serveur (pour faire tourner Kubernetes)

### Cahier des charges

Services k8s à intégrer :
- Services de monitoring (Grafana)
- Services de logging (Prometheus & Loki)
- Services de backup (velero)
- Services de sécurité (Kyverno for Policy Enforcement,  Falco for Runtime Security, Trivy for Vulnerability Scanning, Kubescape for Compliance and Hardening, Cilium for Network Security)
- Services de gestion des identités (KaniDM)
- Services de registre des images contenarisées (Harbor)
- Service de registers des paquets Node (et Java?)
- Tous les services nécéssaires aux fonctionnement de la distribution utilisateur

dont des services hébergés bénéficiant aux employés :
- Services de ticketting
- Gestionnaire Mot de passe
- Espace drive perso
- Logiciel de signature en ligne
- Logiciel de wiki
- Logiciel de discussion communautaire

### Comparatif des distributions

| Distribution   | Avantages                                                                 | Inconvénients                                                                 |
|----------------|---------------------------------------------------------------------------|-------------------------------------------------------------------------------|
| **Talos**      | - Conçu spécifiquement pour Kubernetes <br> - Sécurité renforcée (OS immutable) <br> - Gestion simplifiée via API | - Moins polyvalent (uniquement pour Kubernetes) <br> - Courbe d'apprentissage pour la configuration |
| **Debian**     | - Polyvalent et stable <br> - Large communauté et documentation <br> - Compatible avec de nombreux logiciels | - Nécessite une configuration manuelle pour Kubernetes <br> - Moins optimisé car beaucoup de paquets annexes inutilisés |
