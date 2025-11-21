# LAB Guide: EIGRP & NAT

## 1. Topologia della Rete e Collegamenti

Questa tabella descrive i collegamenti fisici tra i dispositivi della topologia.

| Dispositivo Sorgente | Interfaccia Sorgente | Interfaccia Destinazione | Dispositivo Destinazione |
|---------------------|----------------------|--------------------------|--------------------------|
| PC1 (VLAN 10)       | e0                   | e1                       | Switch                      |
| PC2 (VLAN 20)       | e0                   | e2                       | Switch                      |
| Switch                 | e0                   | e1/0                     | R1                       |
| R1                  | e0/0                 | e0/0                     | R2                       |
| R2                  | e0/1                 | e0/1                     | R3                       |
| R2                  | e1/0                 | nat0                     | NAT1                     |
| R3                  | e1/0                 | e0                       | PC3                      |
| PC3                 | e0                   | e1/0                     | R3                       |

## 2. Assegnazione degli Indirizzi IP

Questa tabella riassume gli indirizzi IP utilizzati nella topologia.

| Dispositivo | Interfaccia      | Indirizzo IP          | Subnet Mask       | Note                                |
|-------------|------------------|-----------------------|-------------------|-------------------------------------|
| PC1         | e0               | 192.168.110.10/24     | 255.255.255.0     | VLAN 10 UTENTI                      |
| PC2         | e0               | 192.168.120.10/24     | 255.255.255.0     | VLAN 20 GUEST                       |
| R1          | Loopback0        | 1.1.1.1/32            | 255.255.255.255   |                                     |
| R1          | e1/0.10          | 192.168.10.1/24       | 255.255.255.0     | Sub-interface per VLAN 10         |
| R1          | e1/0.20          | 192.168.20.1/24       | 255.255.255.0     | Sub-interface per VLAN 20         |
| R1          | e0/0 (a R2)      | 10.0.0.1/24           | 255.255.255.0     |                                     |
| R2          | Loopback0        | 2.2.2.2/32            | 255.255.255.255   |                                     |
| R2          | e0/0 (a R1)      | 10.0.0.2/24           | 255.255.255.0     |                                     |
| R2          | e0/1 (a R3)      | 10.0.2.1/24           | 255.255.255.0     |                                     |
| R2          | e1/0 (a NAT1)    | DHCP                  | N/A               | Indirizzo IP dinamico da NAT1       |
| R3          | Loopback0        | 3.3.3.3/32            | 255.255.255.255   |                                     |
| R3          | e0/0 (a R2)      | 10.0.2.2/24           | 255.255.255.0     |                                     |
| R3          | e1/0 (a PC3)     | 192.168.3.1/24        | 255.255.255.0     |                                     |
| PC3         | e0               | 192.168.3.10/24        | 255.255.255.0     | Indirizzo di rete di PC3            |


---

## Task 1: Configurazione Loopback0 per ogni Router

**Spiegazione**
In questo task ci occupiamo della configurazione dell'interfaccia Loopback0 su ogni router (R1, R2, R3). L'interfaccia Loopback è un'interfaccia logica che è sempre attiva e viene spesso utilizzata come sorgente per il traffico di gestione o come punto di riferimento stabile all'interno della rete.

**Comandi**
Su R1:
```

configure terminal
interface Loopback0
ip address 1.1.1.1 255.255.255.255
exit

```
Su R2:
```

configure terminal
interface Loopback0
ip address 2.2.2.2 255.255.255.255
exit

```
Su R3:
```

configure terminal
interface Loopback0
ip address 3.3.3.3 255.255.255.255
exit

```

**Verifiche**
Verificare che le interfacce Loopback0 siano configurate correttamente su ogni router.
```

show ip interface brief Loopback0

```

---

## Task 2: Configurazione Sub-Interface su R1

**Spiegazione**
In questo task, configureremo le sub-interfacce su R1 per le VLAN 10 (UTENTI) e VLAN 20 (GUEST). Questo è necessario per instradare correttamente il traffico proveniente dalle diverse VLAN verso la rete.

**Comandi**
Su R1:
```

configure terminal
interface Ethernet1/0.10
encapsulation dot1Q 10
ip address 192.168.110.1 255.255.255.0
exit
interface Ethernet1/0.20
encapsulation dot1Q 20
ip address 192.168.120.1 255.255.255.0
exit

```

**Verifiche**
Verificare che le sub-interfacce siano configurate con l'incapsulamento dot1Q e gli indirizzi IP corretti.
```

show running-config interface Ethernet0/0.10
show running-config interface Ethernet0/0.20
show interface Ethernet0/0.10
show interface Ethernet0/0.20

```

---

## Task 3: Configurazione Router EIGRP

**Spiegazione**
Questo task si occupa della configurazione del protocollo di routing EIGRP su tutti i router. Verranno aggiunte tutte le interfacce di ciascun router al processo di routing EIGRP, consentendo loro di scambiare informazioni di routing e raggiungere la convergenza.

**Comandi**
Su R1:
```

configure terminal
router eigrp 1
network 1.1.1.1 0.0.0.0
network 192.168.110.0 0.0.0.255
network 192.168.120.0 0.0.0.255
network 10.0.12.0 0.0.0.3
no auto-summary
exit

```
Su R2:
```

configure terminal
router eigrp 1
network 2.2.2.2 0.0.0.0
network 10.0.12.0 0.0.0.3
network 10.0.23.0 0.0.0.3
no auto-summary
exit

```
Su R3:
```

configure terminal
router eigrp 1
network 3.3.3.3 0.0.0.0
network 10.0.23.0 0.0.0.3
network 192.168.3.0 0.0.0.255
no auto-summary
exit

```

**Verifiche**
Verificare la tabella di adiacenza EIGRP e le rotte apprese su ciascun router.
```

show ip eigrp neighbors
show ip eigrp topology
show ip route eigrp

```

---

## Task 4: Configurazione passive interface per le interfacce LAN

**Spiegazione**
In questo task, configureremo le interfacce collegate alle reti LAN come "passive" in EIGRP. Questo impedisce l'invio di aggiornamenti di routing EIGRP su queste interfacce, migliorando la sicurezza e riducendo il traffico non necessario sulla LAN.

**Comandi**
Su R1:
```

configure terminal
router eigrp 1
passive-interface Ethernet0/0.10
passive-interface Ethernet0/0.20
exit

```
Su R3:
```

configure terminal
router eigrp 1
passive-interface Ethernet1/0
exit

```

**Verifiche**
Verificare le interfacce passive configurate in EIGRP.
```

show running-config | section eigrp

```

---

## Task 5: Propagazione default route da R2

**Spiegazione**
Questo task si occupa di propagare una rotta predefinita (default route) da R2 agli altri router EIGRP. In questo scenario, R2 riceve la rotta predefinita tramite DHCP.
Per il router, la rotta ricevuta tramite DHCP e' pari ad una rotta statica.
È essenziale che questa rotta sia **_redistribuita_** in EIGRP affinché R1 e R3 possano raggiungere destinazioni al di fuori della rete locale, come Internet, tramite R2.

**Comandi**
Su R2:
```

router eigrp 1
redistribute static
default-information originate
exit

```

**Verifiche**
Verificare che la rotta predefinita sia presente nella tabella di routing degli altri router (R1 e R3).
```

show ip route

```

---

## Task 6: Configurazione access-group su e0/0 di R3 per scartare il traffico proveniente dalla rete guest

**Spiegazione**
In questo task, configureremo un access-group su R3 sull'interfaccia e0/0 per scartare il traffico proveniente dalla rete guest (VLAN 20) di R1. Questo è un requisito di sicurezza per isolare la rete guest. Verrà utilizzata una access-list standard.

**Comandi**
Su R3:
```

configure terminal
access-list 1 deny 192.168.120.0 0.0.0.255
access-list 1 permit any
interface Ethernet0/0
ip access-group 1 in
exit

```

**Verifiche**
Verificare la configurazione dell'access-list e la sua applicazione all'interfaccia.
```

show access-lists
show running-config interface Ethernet0/0

```
Provare a pingare dalla rete guest (PC2) verso PC3 (dovrebbe fallire). Provare a pingare dalla rete utenti (PC1) verso PC3 (dovrebbe avere successo).

---

## Task 7: Configurazione access-group su e0/0.20 di R1 per scartare il traffico proveniente dalla rete guest e destinato verso reti private RFC 1918

**Spiegazione**
Questo task si occupa della configurazione di un access-group sull'interfaccia e0/0.20 di R1 per impedire al traffico proveniente dalla rete guest di raggiungere altre reti private RFC 1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16). Verrà utilizzata una IP access-list named.

**Comandi**
Su R1:
```

configure terminal
ip access-list extended GUEST-ONLY-INTERNET
deny ip 192.168.120.0 0.0.0.255 10.0.0.0 0.255.255.255
deny ip 192.168.120.0 0.0.0.255 172.16.0.0 0.15.255.255
deny ip 192.168.120.0 0.0.0.255 192.168.0.0 0.0.255.255
permit ip any any
interface Ethernet0/0.20
ip access-group GUEST-ONLY-INTERNET in
exit

```

**Verifiche**
Verificare la configurazione dell'access-list e la sua applicazione all'interfaccia.
```

show ip access-lists GUEST-ONLY-INTERNET
show running-config interface Ethernet0/0.20

```
Provare a pingare dalla rete guest (PC2) verso un indirizzo IP in una rete privata RFC 1918 (ad esempio, 192.168.10.1 di R1 o 10.0.0.2 di R2) (dovrebbe fallire). Provare a pingare dalla rete guest verso un indirizzo pubblico (dovrebbe avere successo dopo la configurazione del NAT).

---

## Task 8: Configurazione PAT su R2

**Spiegazione**
Questo task si occupa della configurazione del Port Address Translation (PAT) su R2. Il PAT consentirà alle reti interne (VLAN 10, VLAN 20, e 192.168.3.0/24) di accedere a Internet utilizzando un singolo indirizzo IP pubblico sull'interfaccia esterna di R2 (connessa a NAT1).

**Comandi**
Su R2:
```

configure terminal
ip access-list standard NAT_ACL
permit 192.168.110.0 0.0.0.255
permit 192.168.120.0 0.0.0.255
permit 192.168.3.0 0.0.0.255
exit
interface Ethernet1/0
ip nat outside
exit
interface Ethernet0/0
ip nat inside
exit
interface Ethernet0/1
ip nat inside
exit
ip nat inside source list NAT_ACL interface Ethernet1/0 overload
exit

```

**Verifiche**
Verificare la configurazione del NAT e che le traduzioni avvengano correttamente quando il traffico esce verso NAT1.
```

show ip nat translations
show ip nat statistics

```
Provare a pingare da PC1, PC2, e PC3 verso un indirizzo IP esterno (ad esempio, un server DNS pubblico come 8.8.8.8). Le traduzioni dovrebbero apparire in `show ip nat translations`.
