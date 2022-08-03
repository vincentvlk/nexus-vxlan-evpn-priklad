# Cisco Nexus 9300v VXLAN BGP-EVPN - Priklad
![GNS3 - testovacia VXLAN-EVPN topologia](https://github.com/vincentvlk/nexus-vxlan-evpn-priklad/blob/main/gh-gns3-vxlan-evpn-topologia.png)

## Pomocne poznamky na mini-projekt "nexus-vxlan-evpn-priklad"

### Pouzite GNS3 a VM image:

GNS3: `Ver-2.2.32 (Win10Pro)`

VXLAN Fabric - Cisco Nexus 9300v: `nexus9300v.9.3.10.qcow2`

Tenant Switche - L3-Catalyst-IOSv: `vios_l2-adventerprisek9-m.ssa.high_iron_20200929.qcow2`

Inet-R1 - CSR1000-IOS-XEv: `csr1000v-universalk9.16.12.03-serial.qcow2`

---
### Postrehy z konfiguracie a testovania:

- `vPC + VXLAN + EVPN` - je nutne, aby `VNI config` bol konzistentny medzi Peer-mi (podla ocakavania)

- PROBLEM: N9300V nabehne bez funkcneho `nve1`, chyba polozka v conf. mode, v start-conf pritom ale je
  - zatial `reload`, je to menej konfigurovania, ako odstranit `VXLAN features`

- PROBLEM: N9300V nabehne s neaktivnymi Ifaces, aj ked su zapnute v start-conf
  - stacil `shutdown` / `no shutdown`, sposobuje hlavne vypadok OSPF a teda *Underlay*
  - pravdepodobne zalezitost `GNS3+Qemu+KVM`, nic vazne

- problem, na strane zakaznika flapuje port-channel ked je v mode `routed` (vIOS-L3-Cat)
  - pravdepodobne zalezitost `GNS3+Qemu+KVM`, nic vazne, ale otravuju logy na konzole :)

- problem, s `BPDU guard`, aj ked je na druhej strane `BPDUfilter`, vypina uplinky
  - este treba otestovat, pretoze problem sa v niektorych pripadoch neprejavil
  - pravdepodobne zalezitost `GNS3+Qemu+KVM`

- problem, nebolo mozne kontaktovat VTEP-Anycast-GW SVI, 
  trebalo vycistit ARP zaznam na zakaznickom switchy (vIOS-L3-Cat)

- problem s HA, ked vypnem jednu stranu VLAN101 na vPC Peer-e, napr. `N91-Leaf1`
  - nestandarny stav, aj tak treba vypinat celu fyz. cestu v pripade maint.

- conf. backup s: `copy running-config scp://vlkv@192.168.4.244/ vrf default`

- `Xconnect` a `QinVNI` je pri vPC obmedzene, treba pouzit prikazy:

  `system dot1q-tunnel transit [vlan vlan-range]` 
   
  spolu s:
  `system nve infra-vlans <vlan-range>`

  - treba si uvedomit, ze tieto 2 funkcie su *SILNE ZAVISLE* od VTEP-HW (ASICov)

Poznamka z dokumentacie:
```
For proper operation during L3 uplink failure scenarios on vPC VTEPs,
configure a backup SVI and enter the
"system nve infra-vlans <backup-svi-vlan" command.

On Cisco Nexus 9000-EX platform switches,
the backup SVI VLAN needs to be the native VLAN on the peer-link.

 - pouzity Nexus9300v (NX-OSv 9.3.10) tieto 2 prikazy nema, nenasiel som zatial
```

- problem, nejde IPv4 BFD s OSPFv2, prikazy su, ale nezdvihne sa session
  - pravdepodobne zalezitost `GNS3+Qemu+KVM`

- pozorovanie, VTEP automaticky presmeruva IP traffic na svoju VLAN-gw aj ked
  ma CE zariadenie nastavenu inu L3 gw, ktora je L2 dostupna, ma aj ARP zaznam
  - pozriet ci sa da vypnut/zmenit?

- pozorovanie, fyz. porty, ktore su definovane ako `Xconnect-dot1q-tunn`, technologia
  vPC nepovazuje za `Orphan` porty, vid.: `show vpc orphan-ports`

---
### Testovane scenare:

1. Obycajna L2 konektivita, medzi 2 bodmi bez QinVNI (A:SW1 + A:SW3)

2. Unicast routing medzi L2 segmentami v ramci zakaznikovej VRF (A:SW1+2+3+4)

3. Transparentne prepojenie P-to-P cez VXLAN-Xconnect (B:SW1 + B:SW3)
   - pozriet obmedzenia ohladom HW/SW a vPC (NX-OS 9.X vs. NX-OS 10.X)

4. QinVNI prepojenie medzi 2 bodmi cez VXLAN dot1q tunnel (B:SW2 + B:SW4)
   - pozriet obmedzenia ohladom HW/SW a vPC (NX-OS 9.X vs. NX-OS 10.X)

5. Externa kon. do Inetu z VRF "TenantA" / "TenantB" (OSPFv2 + Inet-R1 + B:SW4)
   - do BGP sa da propagovat v ramci VRF default routa s "network 0.0.0.0/0"
   - na Inet-R1 potrebny VRF aware NAT config
   - problem ked je roztiahnuta VXLAN, a border-gw do INetu je len na 1 Pod-e
     - bolo potrebne vypnut AnycastGW na uplinkovych VLAN104+204 pre Tenant-X
     - Leaf1+2 (nema Inet GW) VS. Leaf3+4 (ma Inet GW)

   - zariadenia A:SW4 a B:SW4 maju rovnake IP ale s VRF+NAT sa dostanu v poriadku na Inet

---

### Konfiguracia VXLAN-EVPN Underlay

Tento testovaci dizajn pocita s VXLAN Tenant separaciou (pomocou VRF). Je vsak pouzitelny
aj na zakladne L2 transporty medzi portami/lokalitami v ramci Default VRF.
Ako je (snad) vidiet na diagrame, na VXLAN fabric je pouzitych 6 Nexus9300v VM instancii.
Z toho 2 su pouzite ako Spine box-y, ktore sluzia len ako VXLAN EVPN Route-reflectory a L3 routery.
Zvysne 4 box-y su pouzite ako Leaf VTEP zariadenia, ktore vykonavanju VXLAN Tunnel encaps/decaps
a L2/L3 bridging/routing v ramci VXLAN Tenantov (teda v ramci L3 VRF/VNI).

Z diagramu je vidiet, ze kazdy DC-Pod (ulicka, sala, poschodie, ...) je tvoreny parom
Leaf Switchov, teda mame 2 Pod-y, konkretne (`N91-Leaf1` + `N92-Leaf2`) a (`N93-Leaf3` + `N94-Leaf4`).
Kazdy Pod je este v ramci Ethernet MLAG redundancie nakonfigurovany ako vPC domena.
Leaf switche maju symetricku L3-routed (Underlay) konektivitu na 2 Spine switche (N95-Spine1 + N96-Spine2).
Snahou dizajnu je zabezpecit nezavislu a redundantnu konektivitu, tak aby
v pripade vypadku uzlov napr. (Leaf1, Spine1 a Leaf3) alebo napr. (Leaf2, Spine2 a Leaf4)
bola stale zachovana konektivita pre zakanzikov.

Mal som povodny zamer, ze ako underlay pouzijem IPv6 + OSPFv3, tento koncept
ma vsak stale (Jul2022) obmedzene moznoti a funkcie.
Pre Underlay bol pouzity IGP protokol OSPFv2. Protokol OSPFv3 na NX-OS (na rozdiel od IOSu) 
nedokaze propagovat IPv4 prefixy, alebo som to zatial nenasiel/nepochopil.
Konfiguracia routingu je standardna, vyuzivaju sa Loopback rozhrania, ktore su
dolezite pre funckiu VXLAN fabricu, pretoze sa mapuju na VXLAN VTEP rozhranie `nve1`.

#### VXLAN Underlay konfiguracia pre `N91-Leaf1`:

(N91-Leaf1) Priprava:
```
hostname N91-Leaf1
!
nv overlay evpn
feature ospf
feature bgp
feature interface-vlan
feature vn-segment-vlan-based
feature lacp
feature vpc
feature nv overlay
!
```

(N91-Leaf1) Konfiguracia Pod vPC:
```
vpc domain 912
  peer-switch
  role priority 20
  peer-keepalive destination 172.16.12.2 source 172.16.12.1
  peer-gateway
  layer3 peer-router
!
interface port-channel89
  description Leaf1-vpc-peer-link-Leaf2
  switchport mode trunk
  spanning-tree port type network
  vpc peer-link
!
interface Ethernet1/8
  switchport mode trunk
  spanning-tree port type network
  channel-group 89 mode active
!
interface Ethernet1/9
  switchport mode trunk
  spanning-tree port type network
  channel-group 89 mode active
!
```

(N91-Leaf1) Konfiguracia Underlay routingu uplinkov na Spine-layer:
```
router ospf as65001
  router-id 192.0.2.91
  log-adjacency-changes
  auto-cost reference-bandwidth 4000 Gbps
!
interface Ethernet1/5
  description Underlay prepojenie z N91-Leaf1 na N95-Spine1
  no switchport
  mtu 9216
  no ip redirects
  ip address 10.1.5.1/24
  ipv6 address 2001:db8:1:5::1/64
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
  no shutdown
!
interface Ethernet1/6
  description Underlay prepojenie z N91-Leaf1 na N96-Spine2
  no switchport
  mtu 9216
  no ip redirects
  ip address 10.1.6.1/24
  ipv6 address 2001:db8:1:6::1/64
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
  no shutdown
!
``` 
#### VXLAN Underlay konfiguracia pre `N92-Leaf2`:
