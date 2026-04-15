# Configurer son VPN

L'infrastructure VPN a changé en 2023. Maintenant, il faut récupérer un fichier de configuration ici : [https://hub.imt-atlantique.fr/vpn-user-portal/home](https://hub.imt-atlantique.fr/vpn-user-portal/home). Pour cela il faut être dans le réseau de l'école (physiquement ou passer par [https://campusdistant2.telecom-bretagne.eu](https://campusdistant2.telecom-bretagne.eu)).

## Sur Ubuntu 22.04 et 24.04

Installez les paquets `openvpn`, `network-manager-openvpn` et `network-manager-openvpn-gnome` :
```bash
sudo apt install openvpn network-manager-openvpn network-manager-openvpn-gnome
```

![Configuration VPN Ubuntu networkManager](../img/vpn/VPN-ubuntu.png#center#shadow)


## Sur Windows 10

Télécharger le client OpenVPN ici : [https://openvpn.net/community-downloads/](https://openvpn.net/community-downloads/) et l'installer.

Ouvrir OpenVPN, et importer le fichier de configuration.

## Sur MacOS

Installer le client OpenVPN

  - en mode graphique : https://openvpn.net/client-connect-vpn-for-mac-os/
  - en ligne de commandes : `brew install openvpn`

Importer le fichier de configuration dans le client et lancer le VPN :
  - en mode graphique : glisser-déposer le fichier de configuration OpenVPN dans Import File et lancer le VPN en cliquant sur Connect
  - `openvpn le_fichier.ovpn` en adaptant le chemin complet vers le fichier de configuration

## Source

[https://intranet.imt-atlantique.fr/catalogue-disi/eduvpn/](https://intranet.imt-atlantique.fr/catalogue-disi/eduvpn/)
