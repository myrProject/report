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

### Comparatif des distributions
| Distribution         | Avantages                                                                 | Inconvénients                                                                 |
|----------------------|---------------------------------------------------------------------------|-------------------------------------------------------------------------------|
| **Debian**           | - Stabilité élevée <br> - Large communauté et documentation <br> - Logiciels bien testés | - Logiciels parfois anciens (cycle de release long) <br> - Moins adapté aux dernières technologies |
| **NixOS**            | - Gestion de paquets reproductible <br> - Configuration déclarative <br> - Isolation des paquets | - Courbe d'apprentissage élevée <br> - Documentation moins accessible pour les débutants |
| **ParrotOS (Home)**  | - Orienté sécurité et vie privée <br> - Léger et rapide <br> - Basé sur Debian (stabilité) | - Moins adapté pour un usage généraliste <br> - Moins de logiciels grand public préinstallés |
| **AlpineLinux**      | - Très léger et rapide <br> - Idéal pour les conteneurs et les systèmes légers <br> - Sécurité renforcée | - Environnement minimaliste (peu convivial pour les débutants) <br> - Moins de logiciels disponibles <br> - Pas d'environnement de bureau intégré |
| **Ubuntu**           | - Très populaire et bien documenté <br> - Support à long terme (LTS) <br> - Large logithèque | - Moins léger que d'autres distributions <br> - Dépendances parfois complexes <br> - Impossible de distribuer une version modifiée |

### Prises en main de certaines distributions

#### **AlpineLinux**
En voulant installer un environnement de bureau, nous constatons un très long temps d'installation (<10 min) ainsi que l'installation d'un énorme nombre de paquets.
Cela revient à installer une distribution plus lourde, ce qui n'est pas l'objectif d'AlpineLinux. De plus, Alpine Linux n'inclut pas systemd, responsable de la gestion des sockets, des serives, des processus, des points de montages, des états d'énergie de l'ordinateur entre autres.
Cela rend l'installation de certains logiciels et la gestion d'un OS sur un ordinateur de bureautique plus compliquée.

#### **ParrotOs (Home)**
This edition embodies a versatile operating system with the characteristic ParrotSec aesthetics and usability.
Tailored for everyday computing tasks, privacy-conscious use and software and software development, it meets a wide range of user needs.
While prioritizing usability and accessibility, it also allows users to customize their experience by manually integrating Parrot tools.
This allows the creation of a personalized and streamlined pentesting environment, ensuring flexibility and efficiency in cybersecurity in cybersecurity assessments and operations.


## Choix de la distribution serveur (pour faire tourner Kubernetes)

| Distribution   | Avantages                                                                 | Inconvénients                                                                 |
|----------------|---------------------------------------------------------------------------|-------------------------------------------------------------------------------|
| **Talos**      | - Conçu spécifiquement pour Kubernetes <br> - Sécurité renforcée (OS immutable) <br> - Gestion simplifiée via API | - Moins polyvalent (uniquement pour Kubernetes) <br> - Courbe d'apprentissage pour la configuration |
| **Debian**     | - Polyvalent et stable <br> - Large communauté et documentation <br> - Compatible avec de nombreux logiciels | - Nécessite une configuration manuelle pour Kubernetes <br> - Moins optimisé car beaucoup de paquets annexes inutilisés |
