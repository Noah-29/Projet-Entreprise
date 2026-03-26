#  Installation et Configuration de l'Hyperviseur Proxmox VE

---

##  1. Contexte du Projet
Pour héberger les différents services réseau de l'entreprise (DHCP, DNS, Web), il est nécessaire de mettre en place une solution de **virtualisation de niveau entreprise**. 

Le choix s'est porté sur **Proxmox VE**, une plateforme open-source basée sur Debian, permettant la gestion de machines virtuelles  et de conteneurs via une interface web centralisée.

---

##  2. La Mission
Déployer et configurer le serveur physique (ici virtualisé pour le TP) afin de créer un environnement de laboratoire robuste :
* **Installation du système :** Mise en place de l'hyperviseur sur une base Debian.
* **Segmentation réseau :** Création de Bridges pour isoler le trafic LAN du trafic WAN.
* **Accessibilité :** Configuration de l'interface d'administration Web sécurisée.

---
  
##  3. Stack Technique & Outils
| Composant | Détails |
| :--- | :--- |
| **Hôte de Virtualisation** | VirtualBox|
| **Système** | Proxmox VE 8.2  |
| **Réseau Management** | `192.168.1.222/24` (Accès via le réseau physique) |
| **Réseau LAN Virtuel** | `vmbr1` (Switch virtuel isolé pour les VMs) |
| **Accès Admin** | Interface Web HTTPS sur le port `8006` |

---

##  4. Journal des Problématiques (Troubleshooting)

| Problématique | Cause identifiée | Solution apportée |
| :--- | :--- | :--- |
| **KVM : "Hardware virtualization not found"** | La virtualisation imbriquée n'était pas activée sur VirtualBox. | Commande `VBoxManage modifyvm --nested-hw-virt on` ou activation dans les paramètres processeur. |
| **Pas d'accès à l'interface Web** | Mauvaise configuration de la carte réseau "Accès par pont". | Vérification de l'IP de l'hôte et fixation d'une IP libre dans la même plage (`192.168.1.222`). |
| **VMs sans réseau interne** | Absence d'un switch virtuel pour le LAN. | Création d'un **Linux Bridge** (`vmbr1`) dans l'onglet Network de Proxmox sans IP assignée. |

---

##  5. Étapes de Configuration Clés

### A. Paramétrage Réseau (Post-Installation)
Fichier `/etc/network/interfaces` sur le nœud Proxmox :
```bash
# Carte connectée à la Box (Internet + Admin)
auto vmbr0
iface vmbr0 inet static
    address 192.168.1.222/24
    gateway 192.168.1.1
    bridge-ports eth0

# Carte pour le réseau privé (LAN des VMs)
auto vmbr1
iface vmbr1 inet manual
    bridge-ports none
    bridge-stp off
    bridge-fd 0
