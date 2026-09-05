# Infrastructure Réseau d'Entreprise Sécurisée (Cisco)

<img width="873" height="360" alt="Capture d&#39;écran 2026-09-03 213945" src="https://github.com/user-attachments/assets/47e5e21f-e8f2-4743-bcac-c5e5d9eac3aa" />

## Présentation du Projet
Ce projet documente la conception et la simulation d'un réseau d'entreprise complet sous **Cisco Packet Tracer**. L'architecture intègre une segmentation logique stricte, un cœur de réseau redondant, un routage dynamique OSPF, des services centralisés, ainsi que des politiques de sécurité avancées.

## Architecture & Segmentation
Le réseau est divisé en quatre zones distinctes. Le trafic entre ces zones est géré par un **routage inter-VLAN (Router-on-a-stick)** opéré par les routeurs R2 et R3 via des sous-interfaces 802.1Q, tandis que les commutateurs multicouches assurent la distribution d'accès (Niveau 2) :
* **VLAN 10 (Admin) :** Administration et gestion du réseau.
* **VLAN 20 (Employés) :** Postes de travail du personnel.
* **VLAN 30 (Serveurs) :** Zone hébergeant les services critiques de l'entreprise (`192.168.30.0/24`).
* **VLAN 40 (Invités) :** Accès restreint pour les visiteurs et terminaux mobiles.

## Technologies et Protocoles Déployés
* **Routage Dynamique (OSPF) :** Implémenté entre les routeurs R2, R3 et le routeur du FAI (ISP) pour un échange dynamique et optimisé des routes.
* **Relais DHCP (IP Helper) :** Utilisation de la fonctionnalité `ip helper-address` sur le routeur pour relayer les requêtes DHCP des VLANs clients vers le serveur central de la zone serveurs.
* **Sécurité Périmétrique (Pare-feu Cisco ASA 5506-X) :**
  * **NAT/PAT (Surcharge) :** Traduction des adresses privées internes pour l'accès au réseau étendu (WAN).
  * **Stateful Inspection & Security Levels :** Autorisation et suivi du trafic sortant (Inside vers Outside) tout en bloquant par défaut les tentatives d'intrusion extérieures.
  * **Inspection de Protocoles :** Configuration explicite de la `global_policy` pour inspecter spécifiquement les trafics DNS, FTP, ICMP et TFTP.
* **Sécurité Interne et Filtrage (ACL) :** Mise en place de listes de contrôle d'accès étendues (Extended ACL) pour chaque VLAN afin de cloisonner les flux et de protéger la zone des serveurs.
* **Serveur DHCP (`192.168.30.10`) :** Attribution dynamique des adresses IP, des passerelles et du DNS.
* **Serveur Web & DNS (`192.168.30.11`) :** Hébergement de l'intranet local (`www.entreprise.com`) et résolution de noms (Enregistrement A).

## Guide de Test Rapide
1. Ouvrir le fichier `.pkt` inclus avec Cisco Packet Tracer.
2. Depuis un PC du VLAN 20, lancer un `ping` vers le routeur FAI (`200.0.0.2`) pour vérifier le bon fonctionnement de l'OSPF et du NAT.
3. Ouvrir le navigateur d'un poste client et naviguer vers `www.entreprise.com` pour valider les services (DHCP/DNS) et l'autorisation des requêtes HTTP via les ACL.
