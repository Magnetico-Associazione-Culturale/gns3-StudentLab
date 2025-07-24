Lab FHRP
Topologia



Schema Collegamenti Fisici
Device
Porta Fisica
Porta Logica
To
VLAN
Modalità
SW1-ALS
Ethernet0/0
Port-channel1
SW3-DLS
Trunk (1-4094)
Trunk
SW1-ALS
Ethernet0/1
Port-channel1
SW3-DLS
Trunk (1-4094)
Trunk
SW1-ALS
Ethernet1/0
Port-channel2
SW4-DLS
Trunk (1-4094)
Trunk
SW1-ALS
Ethernet1/1
Port-channel2
SW4-DLS
Trunk (1-4094)
Trunk
SW1-ALS
Ethernet2/0
-
PC1
VLAN10
Access
SW2-ALS
Ethernet0/0
Port-channel4
SW4-DLS
Trunk (1-4094)
Trunk
SW2-ALS
Ethernet0/1
Port-channel4
SW4-DLS
Trunk (1-4094)
Trunk
SW2-ALS
Ethernet1/0
Port-channel3
SW3-DLS
Trunk (1-4094)
Trunk
SW2-ALS
Ethernet1/1
Port-channel3
SW3-DLS
Trunk (1-4094)
Trunk
SW2-ALS
Ethernet2/0
-
PC2
VLAN20
Access
SW3-DLS
Ethernet0/0
Port-channel1
SW1-ALS
Trunk (1-4094)
Trunk
SW3-DLS
Ethernet0/1
Port-channel1
SW1-ALS
Trunk (1-4094)
Trunk
SW3-DLS
Ethernet1/0
Port-channel3
SW2-ALS
Trunk (1-4094)
Trunk
SW3-DLS
Ethernet1/1
Port-channel3
SW2-ALS
Trunk (1-4094)
Trunk
SW4-DLS
Ethernet0/0
Port-channel4
SW2-ALS
Trunk (1-4094)
Trunk
SW4-DLS
Ethernet0/1
Port-channel4
SW2-ALS
Trunk (1-4094)
Trunk
SW4-DLS
Ethernet1/0
Port-channel2
SW1-ALS
Trunk (1-4094)
Trunk
SW4-DLS
Ethernet1/1
Port-channel2
SW1-ALS
Trunk (1-4094)
Trunk
R1
Ethernet0/0
-
SW3-DLS
-
Routed
R1
Ethernet1/0
-
SW4-DLS
-
Routed


Indirizzamenti Logici
Device
Interfaccia
IP Address
Default Gateway
R1


Ethernet0/0
172.16.13.1/30
 n.a.
Ethernet1/0
172.16.14.1/30
n.a.
Loopback 8888
8.8.8.8/32
n.a.
SW3-DLS
Ethernet3/0
172.16.13.2/30
 n.a.
Vlan 10
10.0.10.253/24
10.0.10.254
Vlan 20
10.0.20.253/24
10.0.20.254
Vlan 30
10.0.30.253/24
10.0.30.254
Vlan 40
10.0.40.253/24
10.0.40.254
SW4-DLS
Ethernet3/0
172.16.14.2/30
 n.a.
Vlan 10
10.0.10.253/24
10.0.10.254
Vlan 20
10.0.20.252/24
10.0.20.254
Vlan 30
10.0.30.252/24
10.0.30.254
Vlan 40
10.0.40.252/24
10.0.40.254


Obiettivi
Configurare ed analizzare HSRP e HSRPv2
Configurare ed analizzare VRRP e VRRPv3
Configurare ed implementare Object Tracking
Configurare  DHCP Relay e Redudancy Group

Attivita’
Task #1 - HSRPv1
Configurare il gruppo HSRP 10 per le interfacce VLAN 10 dei dispositivi SW3-DLS e SW4-DLS
Assegnare l’indirizzo 10.0.10.254 come virtual IP.
Permettere al dispositivo SW3-DLS di prendere il ruolo di Active Router grazie ad una priorita’ di 120
Abilitare la preemption su entrambi i dispositivi. Aggiungere un delay = 10s per permettere la corretta convergenza di STP e OSPF
Configurare la password MD5 per il gruppo HSRP 10.  Password Fhrp!
Verificare l’istanza HSRP

 Task #2 - VRRPv2
Configurare il gruppo VRRP 20 per le interfacce VLAN 20 dei dispositivi SW3-DLS e SW4-DLS
Assegnare l’indirizzo 10.0.20.254 come virtual IP.
Permettere al dispositivo SW4-DLS di prendere il ruolo di Active Router grazie ad una priorita’ di 120
Aggiungere un delay = 10s per permettere la corretta convergenza di STP e OSPF
Configurare la password per il gruppo VRRP 20.  Password Fhrp!
Verificare l’istanza VRRP
Quale Mac Address viene utilizzato per l’IP Virtuale?
Quali sono i timer utilizzati di default?
E’ possibile utilizzare l’authentication basata su MD5?

Task #3 - VRRPv3
Disabilitare le configurazioni per il gruppo VRRPv2
Abilitare globalmente l’esecuzione di VRRPv3 su dispositivi SW3-DLS e SW4-DLS
Ripristinare le configurazioni per il gruppo VRRP 20
Interfacce VLAN 20
virtual IP 10.0.20.254
Preemption delay 10s
Verificare l’istanza VRRPv3
Quale Mac Address viene utilizzato per l’IP Virtuale?
Quali sono i timer utilizzati di default?
E’ possibile configurare l’authetication?

Task #4 - HSRPv2
Configurare il gruppo HSRPv2 30 per le interfacce VLAN 30 dei dispositivi SW3-DLS e SW4-DLS
Assegnare l’indirizzo 10.0.30.254 come virtual IP.
Permettere al dispositivo SW3-DLS di prendere il ruolo di Active Router grazie ad una priorita’ di 120
Abilitare la preemption su entrambi i dispositivi. Aggiungere un delay = 10s per permettere la corretta convergenza di STP e OSPF
Configurare Hello interval = 0.5s, Hold Time = 1.5 sec
Configurare nome “VLAN30” all’istanza HSRPv2
Configurare una key chain denominata HSRPv2
configurare una chiave #1 - valore  Fhrp#1
configurare una chiave #1 - valore  Fhrp#2
Configurare l’authetication del gruppo HSRP 30 sfruttando la key-chain creata
Verificare l’istanza HSRP


Task #5 - Monitoraggio link verso Switch accesso
Su SW3-DLS Configurare l’oggetto track 1
monitorare stato protocollare del link PortChannel1
Su SW3-DLS Configurare l’oggetto track 3
monitorare stato protocollare del link PortChannel3
Su SW3-DLS Configurare l’oggetto track 13
legare con operatore logico OR i track 1  e 3
Configurare per tutti i gruppi HSRP (sia v1 che v2) il monitoraggio dell’oggetto track 13. Quando il track va in DOWN, le istanze HSRP devono spegnersi.

Task #6 - Monitoraggio raggiungibilita’ default route
Su SW3-DLS Configurare l’oggetto track 100
monitorare la presenza in tabella di routing della rotta 0.0.0.0/0
Su SW4-DLS Configurare l’oggetto track 100
Su SW3-DLS configurare per tutti i gruppi HSRP il monitoraggio dell’oggetto track 100. Quando il track va in DOWN, SW3-DLS deve avere una priority di 80
Su SW4-DLS configurare per il gruppo VRRP il monitoraggio dell’oggetto track 100. Quando il track va in DOWN, SW4-DLS deve avere una priority di 80

Task #7 - IP Redundancy Virtual Router Groups
Su entrambi gli Switch Distribution, configurare la funzionalita’ di DHCP Relay per le VLAN 10 e 30:
Il DHCP Server risponde all’IP 8.8.8.8
E’ necessario evitare entrambi gli swtich inoltrino le richieste DHCP dei client in LAN
Solo il dispositivo che ha ruolo Active deve inoltrare le richieste DHCP
Assegnare ai dispositivi PC1 e PC3 tramite DHCP
Per ulteriori info, visita: https://learningnetwork.cisco.com/s/article/ip-redundancy-virtual-router-groups-vrg-x

Soluzioni
Task #1

SW3-DLS
conf t
!
interface Vlan10
 standby 10 ip 10.0.10.254
 standby 10 priority 120
 standby 10 preempt delay minimum 10
 standby 10 authentication md5 key-string Fhrp!
!
end
!
show standby
SW4-DLS
conf t
!
interface Vlan10
 standby 10 ip 10.0.10.254
 standby 10 preempt delay minimum 10
 standby 10 authentication md5 key-string Fhrp!
!
end
!
show standby


Task #2

SW3-DLS
conf t
!
interface Vlan20
 vrrp 20 ip 10.0.20.254
 vrrp 20 preempt delay minimum 10
 vrrp 20 authentication text Fhrp!

!
end
!
show vrrp
SW4-DLS
conf t
!
interface Vlan20
 vrrp 20 ip 10.0.20.254
 vrrp 20 priority 120
 vrrp 20 preempt delay minimum 10
 vrrp 20 authentication text Fhrp!

!
end
!
show vrrp


Task #3

SW3-DLS
conf t
!
interface Vlan20
 no vrrp 20
 !
exit
fhrp version vrrp v3
!
interface Vlan20
 vrrp 20 address-family ipv4
  address 10.0.20.254
  preempt delay minimum 10
!
end
!
show vrrp
SW4-DLS
conf t
!
interface Vlan20
 no vrrp 20
 !
exit
fhrp version vrrp v3
!
interface Vlan20
 vrrp 20 address-family ipv4
  address 10.0.20.254
  preempt delay minimum 10
  priority 120
!
end
!
show vrrp


Task #4

SW3-DLS
conf t
!
key chain HSRPv2
 key 1
  key-string Fhrp#1
 key 2
  key-string Fhrp#2
!
interface Vlan30
 standby version 2
 standby 30 ip 10.0.30.254
 standby 30 timers msec 500 msec 1500
 standby 30 priority 120
 standby 30 preempt delay minimum 10
 standby 30 authentication md5 key-chain HSRPv2
 standby 30 name VLAN30
!
exit
!
show standby
SW4-DLS
conf t
!
key chain HSRPv2
 key 1
  key-string Fhrp#1
 key 2
  key-string Fhrp#2
!
interface Vlan30
 standby version 2
 standby 30 ip 10.0.30.254
 standby 30 timers msec 500 msec 1500
 standby 30 preempt delay minimum 10
 standby 30 authentication md5 key-chain HSRPv2
 standby 30 name VLAN30
!
end
!
show standby


Task #5

SW3-DLS
conf t
!
track 1 interface Port-channel1 line-protocol
track 3 interface Port-channel3 line-protocol
track 13 list boolean or
 object 1
 object 3
!
interface Vlan10
 standby 10 track 13 shutdown
!
interface Vlan30
 standby 30 track 13 shutdown
!
exit
!
show standby


Task #6

SW3-DLS
conf t
!
track 100 ip route 0.0.0.0 0.0.0.0 reachability
!
interface Vlan10
 standby 10 track 100 decrement 30
!
interface Vlan30
 standby 30 track 100 decrement 30
!
exit
!
show standby
SW4-DLS
conf t
!
track 100 ip route 0.0.0.0 0.0.0.0 reachability
!
interface Vlan20
 vrrp 20 address-family ipv4
  track 100 decrement 30
!
end
!
show vrrp


Task #7

SW3-DLS
conf t
!
interface Vlan10
 ip helper-address 8.8.8.8 redundancy hsrp-Vl10-10
interface Vlan30
 ip helper-address 8.8.8.8 redundancy VLAN30
!
SW4-DLS
conf t
!
interface Vlan10
 ip helper-address 8.8.8.8 redundancy hsrp-Vl10-10
interface Vlan30
 ip helper-address 8.8.8.8 redundancy VLAN30


