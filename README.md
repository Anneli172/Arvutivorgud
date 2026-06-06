# Arvutivõrgud – GRE Tunnel ja DHCP Relay

Cisco Packet Traceri praktiline projekt, mille eesmärk oli ühendada kaks kohtvõrku GRE tunneli abil ning võimaldada DHCP teenuse kasutamine üle tunneli.

---

## Projekti eesmärgid

- Seadistada kaks kohtvõrku
- Luua GRE tunnel
- Seadistada DHCP server
- Kasutada DHCP Relay (IP Helper Address)
- Rakendada staatiline marsruutimine
- Seadistada SSH haldus
- Kontrollida ühenduvust kõikide seadmete vahel

---

## Võrgu topoloogia

### Algne topoloogia

![Algne PacketTracer](Screenshots/Algne%20PacketTracer.png)

### Lõplik topoloogia

![Valmis PacketTracer](Screenshots/Valmis%20PacketTracer.png)

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

## Tehnoloogiad

- GRE Tunnel
- DHCP Server
- DHCP Relay (IP Helper Address)
- Staatiline marsruutimine
- SSH haldus
- Cisco IOS

---

## Projekti failid

### Packet Tracer

- [Võrgu_pilet_2.pka](PacketTracer/Võrgu_pilet_2.pka)

### Dokumentatsioon

- [Arvutivorgud eksami dokumentatsioon.docx](Arvutivorgud%20eksami%20dokumentatsioon.docx)

### Seadmete konfiguratsioonid

- [R1 konfiguratsioon](Configs/R1.txt)
- [R2 konfiguratsioon](Configs/R2.txt)
- [R3 konfiguratsioon](Configs/R3.txt)
- [S1 konfiguratsioon](Configs/S1.txt)
- [S2 konfiguratsioon](Configs/S2.txt)

---

## Olulisemad seadistused

### GRE Tunnel

```cisco
R1 Tunnel1: 10.0.0.2/30
R2 Tunnel1: 10.0.0.1/30
```

### DHCP Relay

```cisco
ip helper-address 192.168.5.1
```

### Staatiline marsruutimine

```cisco
R1 -> 192.168.6.0/25 via 10.0.0.1
R2 -> 192.168.5.0/25 via 10.0.0.2
```

---

## Testimine

Kontrollitud:

- GRE tunneli töö
- DHCP aadresside jagamine
- DHCP Relay töö
- LAN-võrkude vaheline ühenduvus
- SSH ühendused
- Staatiline marsruutimine

---

## Autor

**Anneli Tikerber**

Cisco Packet Tracer – Arvutivõrgud
