"# mas"

Pour configurer des VLANs dans une entreprise avec trois départements (par exemple, RH, IT, Finance) en utilisant Cisco Packet Tracer, voici les étapes à suivre :

### Schéma d'implémentation proposé

1. **Topologie réseau** :
   - Un **commutateur central (core switch)** connecté à un **routeur** pour la gestion du routage inter-VLAN.
   - Trois **commutateurs d'accès** pour chaque département :
     - Commutateur pour **RH** (VLAN 10)
     - Commutateur pour **IT** (VLAN 20)
     - Commutateur pour **Finance** (VLAN 30)
   - Chaque commutateur d'accès est relié au commutateur central via un lien de type **trunk** pour transporter le trafic de plusieurs VLANs.

2. **VLANs** :
   - **VLAN 10** : Département RH
   - **VLAN 20** : Département IT
   - **VLAN 30** : Département Finance

3. **Routeur** pour le routage inter-VLAN avec des **sous-interfaces** pour chaque VLAN.

### Étapes de configuration

1. **Ajout de dispositifs** dans Packet Tracer :
   - Placez un routeur, un commutateur central, trois commutateurs d'accès, et plusieurs PCs pour chaque département.

2. **Création des VLANs** sur les commutateurs d'accès :
   Pour chaque commutateur d'accès, utilisez les commandes suivantes :
   
   ```bash
   Switch(config)# vlan 10
   Switch(config-vlan)# name RH
   Switch(config)# vlan 20
   Switch(config-vlan)# name IT
   Switch(config)# vlan 30
   Switch(config-vlan)# name Finance
   ```

3. **Attribution des ports aux VLANs** :
   Sur chaque commutateur d'accès, assignez les ports connectés aux PCs du département au VLAN correspondant :
   
   ```bash
   Switch(config)# interface range fa0/1 - 5
   Switch(config-if-range)# switchport mode access
   Switch(config-if-range)# switchport access vlan 10
   ```

   Faites de même pour les VLANs 20 et 30 pour les autres départements.

4. **Configuration du lien trunk entre les commutateurs d'accès et le commutateur central** :
   
   ```bash
   Switch(config)# interface fa0/24
   Switch(config-if)# switchport mode trunk
   ```

5. **Configuration du routage inter-VLAN sur le routeur** :
   Sur le routeur, configurez des sous-interfaces pour chaque VLAN afin de permettre la communication entre eux :
   
   ```bash
   Router(config)# interface fa0/0.10
   Router(config-subif)# encapsulation dot1Q 10
   Router(config-subif)# ip address 192.168.10.1 255.255.255.0
   
   Router(config)# interface fa0/0.20
   Router(config-subif)# encapsulation dot1Q 20
   Router(config-subif)# ip address 192.168.20.1 255.255.255.0

   Router(config)# interface fa0/0.30
   Router(config-subif)# encapsulation dot1Q 30
   Router(config-subif)# ip address 192.168.30.1 255.255.255.0
   ```

6. **Configuration des PC** :
   Attribuez des adresses IP statiques à chaque PC, en fonction de leur VLAN :
   
   - VLAN 10 (RH) : 192.168.10.x /24
   - VLAN 20 (IT) : 192.168.20.x /24
   - VLAN 30 (Finance) : 192.168.30.x /24

   Assurez-vous que chaque PC a comme passerelle par défaut l'IP de la sous-interface correspondante du routeur (par exemple, 192.168.10.1 pour RH).

7. **Test de connectivité** :
   - Utilisez la commande **ping** pour vérifier la connectivité entre des PCs appartenant au même VLAN.
   - Testez également la communication inter-VLAN en vérifiant si les PCs de différents VLANs peuvent se pinguer, ce qui vérifiera le routage inter-VLAN.

### Simulation dans Packet Tracer

1. **Configurer et tester dans Cisco Packet Tracer** :
   - Déployez la topologie comme décrite ci-dessus.
   - Configurez les équipements avec les commandes listées.
   - Testez la communication avec des commandes **ping** et **traceroute** entre les différents départements.

2. **Vérification de la configuration** :
   Utilisez les commandes suivantes pour vérifier la configuration des VLANs et des trunks sur les commutateurs :
   
   ```bash
   Switch# show vlan brief
   Switch# show interfaces trunk
   Router# show ip interface brief
   ```

Cela devrait vous permettre de configurer efficacement les VLANs et de tester cette configuration en utilisant Cisco Packet Tracer dans un environnement simulé.
