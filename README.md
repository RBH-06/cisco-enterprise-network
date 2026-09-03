# Infrastructure Réseau d'Entreprise Sécurisée (Cisco)

![Topologie Réseau](./image_2ea94b.png)

## Présentation du Projet
Ce projet documente la conception et la simulation d'un réseau d'entreprise complet sous **Cisco Packet Tracer**. L'architecture intègre une segmentation logique stricte, un cœur de réseau redondant, un routage dynamique OSPF, des services centralisés, ainsi que des politiques de sécurité avancées.

## Architecture & Segmentation
L'infrastructure s'articule autour de deux commutateurs multicouches (Multilayer Switches) garantissant un routage inter-VLAN performant. Le réseau est divisé en quatre zones distinctes :
* **VLAN 10 (Admin) :** Administration et gestion du réseau.
* **VLAN 20 (Employés) :** Postes de travail du personnel.
* **VLAN 30 (Invités) :** Accès restreint pour les visiteurs et terminaux mobiles.
* **VLAN 40 (Serveurs) :** Zone hébergeant les services critiques de l'entreprise.

## Technologies et Protocoles Déployés
* **Routage Dynamique (OSPF) :** Implémenté entre les routeurs R2, R3 et le routeur du FAI (ISP) pour un échange dynamique et optimisé des routes.
* **Sécurité Périmétrique (Pare-feu Cisco ASA 5506-X) :** 
  * **NAT/PAT (Surcharge) :** Traduction des adresses privées internes pour l'accès au réseau étendu (WAN).
  * **Stateful Inspection & Security Levels :** Autorisation et suivi du trafic sortant (LAN vers WAN) tout en bloquant par défaut toute tentative d'intrusion initiée depuis l'extérieur (WAN vers LAN).
* **Sécurité Interne et Filtrage (ACL) :** Mise en place de listes de contrôle d'accès pour chaque VLAN afin de cloisonner les flux internes et protéger la zone des serveurs (VLAN 40).
* **Serveur DHCP (`192.168.30.10`) :** Attribution dynamique des adresses IP, des passerelles et du DNS selon le VLAN (10, 20, 30).
* **Serveur Web & DNS (`192.168.30.11`) :** Hébergement de l'intranet local (`www.entreprise.com`) et résolution de noms (Enregistrement A).

## Guide de Test Rapide
1. Ouvrir le fichier `.pkt` inclus avec Cisco Packet Tracer.
2. Depuis un PC du VLAN 20, lancer un `ping` vers le routeur FAI pour vérifier le bon fonctionnement de l'OSPF.
3. Ouvrir le navigateur d'un poste client et naviguer vers `www.entreprise.com` pour valider les services (DHCP/DNS) et l'autorisation des requêtes HTTP via les ACL.
