# Arvutivõrgud – GRE Tunnel ja DHCP Relay

## Projekti ülevaade

Käesolev projekt on koostatud Cisco Packet Traceri praktilise ülesande lahendusena.

### Projekti eesmärgid

- Seadistada kaks kohtvõrku
- Ühendada võrgud GRE tunneli abil
- Seadistada DHCP server
- Kasutada DHCP Relay (IP Helper Address)
- Seadistada staatiline marsruutimine
- Võimaldada SSH kaudu haldamine
- Testida ühenduvust kõikide seadmete vahel

---

## Kasutatud seadmed

| Seade | Tüüp |
|--------|--------|
| R1 | Cisco 1941 |
| R2 | Cisco 1941 |
| R3 | Cisco 1941 |
| S1 | Cisco 2960-24TT |
| S2 | Cisco 2960-24TT |
| PC0 | Klient |
| PC1 | Klient |
| PC2 | Klient |
| PC3 | Klient |

---

## Võrgu topoloogia

### Lõplik topoloogia

![Topoloogia](Screenshots/Topologia_Valmis.png)

### Algne topoloogia

![Algne topoloogia](Screenshots/Topologia_Algne.png)

---

## Kaabeldus

### WAN ühendused

| Seade | Liides | Seade | Liides |
|---------|---------|---------|---------|
| R1 | Serial0/0/0 | R3 | Serial0/0/0 |
| R2 | Serial0/0/1 | R3 | Serial0/0/1 |

### LAN ühendused

| Seade | Liides | Seade | Liides |
|---------|---------|---------|---------|
| R1 | GigabitEthernet0/0 | S1 | FastEthernet0/1 |
| R2 | GigabitEthernet0/0 | S2 | FastEthernet0/1 |

---

## IP-aadresside plaan

| Seade | Liides | IP-aadress |
|---------|---------|---------|
| R1 | GigabitEthernet0/0 | 192.168.5.1/25 |
| R1 | Serial0/0/0 | 201.10.23.1/30 |
| R1 | Tunnel1 | 10.0.0.2/30 |
| R2 | GigabitEthernet0/0 | 192.168.6.1/25 |
| R2 | Serial0/0/1 | 194.25.10.42/30 |
| R2 | Tunnel1 | 10.0.0.1/30 |
| R3 | Serial0/0/0 | 201.10.23.2/30 |
| R3 | Serial0/0/1 | 194.25.10.41/30 |

---

## DHCP seadistus

DHCP server asub ruuteril **R1**.

### Välistatud aadressid

```cisco
ip dhcp excluded-address 192.168.5.1 192.168.5.10
ip dhcp excluded-address 192.168.6.1 192.168.6.10
```

### DHCP Pool LAN

```cisco
ip dhcp pool LAN
 network 192.168.5.0 255.255.255.128
 default-router 192.168.5.1
 dns-server 1.1.1.1
 domain-name too.net
```

### DHCP Pool LAN1

```cisco
ip dhcp pool LAN1
 network 192.168.6.0 255.255.255.128
 default-router 192.168.6.1
 dns-server 1.1.1.1
 domain-name too.net
```

---

## GRE Tunnel

### R1

```cisco
interface Tunnel1
 ip address 10.0.0.2 255.255.255.252
 tunnel source Serial0/0/0
 tunnel destination 194.25.10.42
```

### R2

```cisco
interface Tunnel1
 ip address 10.0.0.1 255.255.255.252
 tunnel source Serial0/0/1
 tunnel destination 201.10.23.1
```

---

## DHCP Relay

Kuna DHCP server asub R1 peal, kasutatakse R2 LAN-võrgus DHCP Relay lahendust.

```cisco
interface GigabitEthernet0/0
 ip helper-address 192.168.5.1
```

---

## Staatiline marsruutimine

### R1

```cisco
ip route 192.168.6.0 255.255.255.128 10.0.0.1
ip route 0.0.0.0 0.0.0.0 201.10.23.2
```

### R2

```cisco
ip route 192.168.5.0 255.255.255.128 10.0.0.2
ip route 0.0.0.0 0.0.0.0 194.25.10.41
```

---

## SSH seadistus

```cisco
username admin privilege 15 secret Passw0rd!
ip domain-name too.net

crypto key generate rsa
1024
```

```cisco
line vty 0 15
 login local
 transport input ssh
```

---

## Tööjaamade seadistamine

Kõik tööjaamad kasutavad DHCP konfiguratsiooni.

- PC0 → Desktop → IP Configuration → DHCP
- PC1 → Desktop → IP Configuration → DHCP
- PC2 → Desktop → IP Configuration → DHCP
- PC3 → Desktop → IP Configuration → DHCP

---

## Konfiguratsioonifailid

- [R
