
# Arvutivorgud
Projekti ülevaade
Käesolev projekt on koostatud Cisco Packet Traceri praktilise ülesande lahendusena.
Eesmärgiks oli:
•	seadistada kaks kohtvõrku
•	ühendada võrgud GRE tunneli abil
•	seadistada DHCP server
•	kasutada DHCP Relay (IP Helper Address)
•	seadistada staatiline marsruutimine
•	võimaldada SSH kaudu haldamine
•	testida ühenduvust kõikide seadmete vahel
________________________________________
Kasutatud seadmed
Seade	Tüüp
R1	Cisco 1941
R2	Cisco 1941
R3	Cisco 1941
S1	Cisco 2960
S2	Cisco 2960
PC0	Klient
PC1	Klient
PC2	Klient
PC3	Klient
________________________________________
Võrgu topoloogia
                 GRE Tunnel
              10.0.0.0 /30

          Tunnel1              Tunnel1
         10.0.0.2            10.0.0.1

             R1 ---------------- R2
              \                  /
               \                /
             201.10.23.0/30   194.25.10.40/30
                 \            /
                  \          /
                     R3

       LAN1                        LAN2

192.168.5.0/25              192.168.6.0/25

PC0 ----\
          \
PC1 ----- S1 ----- R1

R2 ----- S2 ----- PC2
                 |
                 |
                PC3
________________________________________
Kaabeldus
WAN ühendused
Seade	Liides	Seade	Liides
R1	Serial0/0/0	R3	Serial0/0/0
R2	Serial0/0/1	R3	Serial0/0/1
________________________________________
LAN ühendused
R1 pool
Seade	Liides
R1	GigabitEthernet0/0
S1	FastEthernet0/1
R2 pool
Seade	Liides
R2	GigabitEthernet0/0
S2	FastEthernet0/1
________________________________________
Tööjaamad
S1
Seade	Port
PC0	Fa0
PC1	Fa0
S2
Seade	Port
PC2	Fa0
PC3	Fa0
________________________________________
IP-aadresside plaan
R1
Liides	IP
G0/0	192.168.5.1/25
S0/0/0	201.10.23.1/30
Tunnel1	10.0.0.2/30
________________________________________
R2
Liides	IP
G0/0	192.168.6.1/25
S0/0/1	194.25.10.42/30
Tunnel1	10.0.0.1/30
________________________________________
R3
Liides	IP
S0/0/0	201.10.23.2/30
S0/0/1	194.25.10.41/30
________________________________________
DHCP seadistus
DHCP server asub ruuteril R1.
Välistatud aadressid
ip dhcp excluded-address 192.168.5.1 192.168.5.10
ip dhcp excluded-address 192.168.6.1 192.168.6.10
DHCP Pool LAN
ip dhcp pool LAN
 network 192.168.5.0 255.255.255.128
 default-router 192.168.5.1
 dns-server 1.1.1.1
 domain-name too.net
DHCP Pool LAN1
ip dhcp pool LAN1
 network 192.168.6.0 255.255.255.128
 default-router 192.168.6.1
 dns-server 1.1.1.1
 domain-name too.net
________________________________________
GRE Tunnel
R1
interface Tunnel1
 ip address 10.0.0.2 255.255.255.252
 tunnel source Serial0/0/0
 tunnel destination 194.25.10.42
R2
interface Tunnel1
 ip address 10.0.0.1 255.255.255.252
 tunnel source Serial0/0/1
 tunnel destination 201.10.23.1
________________________________________
DHCP Relay
Kuna DHCP server asub R1 peal, tuleb R2 LAN võrgus kasutada DHCP Relay lahendust.
interface GigabitEthernet0/0
 ip helper-address 192.168.5.1
________________________________________
Staatiline marsruutimine
R1
ip route 192.168.6.0 255.255.255.128 10.0.0.1
ip route 0.0.0.0 0.0.0.0 201.10.23.2
R2
ip route 192.168.5.0 255.255.255.128 10.0.0.2
ip route 0.0.0.0 0.0.0.0 194.25.10.41
________________________________________
SSH seadistus
Kõikidel seadmetel:
username admin privilege 15 secret Passw0rd!
ip domain-name too.net
crypto key generate rsa
1024
VTY liinid:
line vty 0 15
 login local
 transport input ssh
________________________________________
PC-de seadistamine
Kõik tööjaamad kasutavad DHCP konfiguratsiooni.
PC0
Desktop → IP Configuration → DHCP
PC1
Desktop → IP Configuration → DHCP
PC2
Desktop → IP Configuration → DHCP
PC3
Desktop → IP Configuration → DHCP
________________________________________
Kontrollkäsklused
Tunneli kontroll
show ip interface brief
Marsruutimise kontroll
show ip route
DHCP kontroll
show ip dhcp binding
Konfiguratsiooni kontroll
show running-config
________________________________________
Testimine
Tunnel
ping 10.0.0.1
ping 10.0.0.2
LAN ühenduvus
ping 192.168.5.11
ping 192.168.6.11
DHCP
ipconfig
Kõik tööjaamad peavad saama IP-aadressi automaatselt DHCP serverilt.
________________________________________
Õppetunnid
Töö käigus lahendati järgmised probleemid:
•	GRE tunneli konfiguratsioon
•	DHCP Relay seadistamine
•	Staatilise marsruutimise kontroll
•	WAN ühenduste testimine
•	SSH halduse seadistamine
•	Packet Traceri hindamissüsteemi eripärad
________________________________________

