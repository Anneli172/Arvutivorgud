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
| S1 | Cisco 2960 |
| S2 | Cisco 2960 |
| PC0 | Klient |
| PC1 | Klient |
| PC2 | Klient |
| PC3 | Klient |

---

## Võrgu topoloogia

![Lõplik topoloogia](Screenshots/Topologia_Valmis.png)

![Algne topoloogia](Screenshots/Topologia_Algne.png)

---

## IP-aadresside plaan

| Seade | Liides | IP-aadress |
|--------|---------|---------|
| R1 | G0/0 | 192.168.5.1/25 |
| R1 | S0/0/0 | 201.10.23.1/30 |
| R1 | Tunnel1 | 10.0.0.2/30 |
| R2 | G0/0 | 192.168.6.1/25 |
| R2 | S0/0/1 | 194.25.10.42/30 |
| R2 | Tunnel1 | 10.0.0.1/30 |
| R3 | S0/0/0 | 201.10.23.2/30 |
| R3 | S0/0/1 | 194.25.10.41/30 |

