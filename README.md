# Projet-Entreprise

#  Projet : Mise en place d'une Infrastructure Réseau Centralisée

##  1. Contexte du Projet
Dans le cadre de l'évolution du système d'information de l'entreprise, j'ai été chargé de moderniser la gestion de l'adressage IP du parc informatique. 

Auparavant géré de manière statique (source d'erreurs et de conflits d'IP), le réseau devait migrer vers une solution **automatisée, robuste et isolée** pour garantir la continuité de service des collaborateurs.

##  2. La Mission
Ma mission consistait à déployer un serveur de distribution d'adresses dynamiques (**DHCP**) répondant aux exigences suivantes :
* **Isolation :** Le serveur doit opérer sur un segment réseau privé (LAN).
* **Performance :** Utilisation d'un OS serveur léger sans interface graphique.
* **Sécurité :** Limitation des services installés au strict nécessaire (Principe du moindre privilège).

##  3. Stack Technique & Outils
| Catégorie | Outils utilisés |
| :--- | :--- |
| **Virtualisation** | Proxmox VE , VirtualBox |
| **Système d'Exploitation** | Debian  |
| **Service Réseau** | ISC-DHCP-Server |
| **Administration** | Bash, Nano, Systemd, APT |
| **Documentation** | Markdown / GitHub |

## 4. Compétences Développées
Ce projet m'a permis de valider plusieurs compétances;

* **Gérer le patrimoine informatique :** Installation et configuration d'un système d'exploitation serveur.
* **Répondre aux incidents :** Analyse de logs (`journalctl`) et résolution de problèmes réseau complexes (mode promiscuité, routage).
* **Développer la présence en ligne de l'organisation :** Documentation technique rigoureuse sur GitHub.
* **Travailler en mode projet :** Respect d'un cahier des charges et des contraintes d'isolation réseau.

---
