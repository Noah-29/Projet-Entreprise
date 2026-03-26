#  Déploiement d'un Serveur DHCP.

Ce dépôt documente l'installation, la configuration et la résolution de problèmes rencontrés lors de la mise en place d'un service DHCP dans un environnement virtualisé **Proxmox**.

##  1. Environnement Technique
* **Hyperviseur :** Proxmox VE (hébergé sur VirtualBox 7.x)
* **OS Serveur :** Debian 13 "Trixie" (Netinst amd64)
* **Service :** `isc-dhcp-server`
* **Réseau Privé (LAN) :** `192.168.10.0/24`
* **Interface LAN :** `ens18` (IP Fixe : `192.168.10.1`)

---

##  2. Étapes d'Installation

### A. Installation de l'OS
L'installation a été réalisée en **mode texte** ( pas de os graphics) pour garantir la stabilité de l'affichage en environnement virtualisé et optimiser les ressources système (RAM/CPU).

**Sélection des logiciels :**
- [x] SSH Server
- [x] Standard System Utilities
- [ ] Debian Desktop Environment (GNOME/KDE désactivés)

### B. Configuration de l'IP Statique
Fichier modifié : `/etc/network/interfaces`
```bash
auto ens18
iface ens18 inet static
    address 192.168.10.1
    netmask 255.255.255.0 
```

---

##  3. Journal des Problématiques 

| Problématique | Cause identifiée | Solution apportée |
| :--- | :--- | :--- |
| **Écran noir au boot** | Incompatibilité de l'accélération matérielle (KVM/GPU) en VM. | Passage à l'installateur **mode texte**  plus stable. |
| **Ping échoué ** | Sécurité de VirtualBox bloquant les flux réseau imbriqués. | Activation du **Mode Promiscuité : Autoriser tout** dans VirtualBox. |
| **Apt install impossible** | Sources APT limitées au CD-ROM (installation hors-ligne). | Modification du fichier `/etc/apt/sources.list` pour ajouter les miroirs web. |
| **Paquet introuvable** | Absence de résolution de noms (DNS) et de route par défaut. | Configuration manuelle temporaire de `ip route` et du fichier `/etc/resolv.conf`. |
| **Service DHCP en "Failed"** | Configuration manquante ou erronée des interfaces d'écoute. | Édition du fichier `/etc/default/isc-dhcp-server` pour déclarer l'interface `ens18`. |

---

##  4. Configuration du Service DHCP
Fichier `/etc/default/isc-dhcp-server`
On spécifie l'interface d'écoute :
```bash
INTERFACESv4="ens18"
```
Fichier `/etc/dhcp/dhcpd.conf`
```bash
subnet 192.168.10.0 netmask 255.255.255.0 {
  range 192.168.10.10 192.168.10.50;      
  option routers 192.168.10.1;            
  option domain-name-servers 8.8.8.8;     
  default-lease-time 600;                 
  max-lease-time 7200;                    
}
```
##  5. Commandes de Gestion
Vérifier que le service est bien démarré :
```bash
systemctl status isc-dhcp-server
```

Redémarrer le service après une modification :
```systemctl restart isc-dhcp-server
```

Consulter les IP actuellement distribués :
```
cat /var/lib/dhcp/dhcpd.leases
```
---

##  6. Sécurisation
Une fois le service installé et configuré, la carte réseau temporaire (ens19 sur vmbr0) a été supprimée pour garantir l'isolation totale du réseau local d'entreprise.

Le serveur ne possède désormais aucune route vers l'extérieur, limitant ainsi la surface d'attaque.
