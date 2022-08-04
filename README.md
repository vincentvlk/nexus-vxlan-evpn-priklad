# Cisco Nexus 9300v VXLAN BGP-EVPN - Priklad
![GNS3 - testovacia VXLAN-EVPN topologia](https://github.com/vincentvlk/nexus-vxlan-evpn-priklad/blob/main/gh-gns3-vxlan-evpn-topologia.png)

* [Cisco Nexus 9300v VXLAN BGP-EVPN - Priklad](#cisco-nexus-9300v-vxlan-bgp-evpn---priklad)
   * [Pomocne poznamky na mini-projekt "nexus-vxlan-evpn-priklad"](#pomocne-poznamky-na-mini-projekt-nexus-vxlan-evpn-priklad)
      * [Pouzite GNS3 a VM image:](#pouzite-gns3-a-vm-image)
      * [Postrehy z konfiguracie a testovania:](#postrehy-z-konfiguracie-a-testovania)
      * [Testovane scenare:](#testovane-scenare)
      * [Konfiguracia VXLAN-EVPN Underlay](#konfiguracia-vxlan-evpn-underlay)
         * [VXLAN Underlay konfiguracia pre N91-Leaf1:](#vxlan-underlay-konfiguracia-pre-n91-leaf1)
         * [VXLAN Underlay konfiguracia pre N92-Leaf2:](#vxlan-underlay-konfiguracia-pre-n92-leaf2)
         * [VXLAN Underlay konfiguracia pre N93-Leaf3:](#vxlan-underlay-konfiguracia-pre-n93-leaf3)
         * [VXLAN Underlay konfiguracia pre N94-Leaf4:](#vxlan-underlay-konfiguracia-pre-n94-leaf4)
         * [VXLAN Underlay konfiguracia pre N95-Spine1:](#vxlan-underlay-konfiguracia-pre-n95-spine1)
         * [VXLAN Underlay konfiguracia pre N96-Spine2:](#vxlan-underlay-konfiguracia-pre-n96-spine2)
      * [Konfiguracia VXLAN-EVPN Overlay:](#konfiguracia-vxlan-evpn-overlay)
         * [VXLAN Overlay konfiguracia pre N91-Leaf1:](#vxlan-overlay-konfiguracia-pre-n91-leaf1)
         * [VXLAN Overlay konfiguracia pre N92-Leaf2:](#vxlan-overlay-konfiguracia-pre-n92-leaf2)
         * [VXLAN Overlay konfiguracia pre N93-Leaf3:](#vxlan-overlay-konfiguracia-pre-n93-leaf3)
         * [VXLAN Overlay konfiguracia pre N94-Leaf4:](#vxlan-overlay-konfiguracia-pre-n94-leaf4)
         * [VXLAN Overlay konfiguracia pre N95-Spine1:](#vxlan-overlay-konfiguracia-pre-n95-spine1)
         * [VXLAN Overlay konfiguracia pre N96-Spie2:](#vxlan-overlay-konfiguracia-pre-n96-spie2)
      * [Konfiguracia VXLAN-EVPN sluzieb na strane poskytovatela:](#konfiguracia-vxlan-evpn-sluzieb-na-strane-poskytovatela)
         * [VXLAN-EVPN konfiguracia sluzieb pre zakaznika *TenantA* na N91-Leaf1:](#vxlan-evpn-konfiguracia-sluzieb-pre-zakaznika-tenanta-na-n91-leaf1)
         * [VXLAN-EVPN konfiguracia sluzieb pre zakaznika *TenantA* na N92-Leaf2:](#vxlan-evpn-konfiguracia-sluzieb-pre-zakaznika-tenanta-na-n92-leaf2)
         * [VXLAN-EVPN konfiguracia sluzieb pre zakaznika *TenantA* na N93-Leaf3:](#vxlan-evpn-konfiguracia-sluzieb-pre-zakaznika-tenanta-na-n93-leaf3)
         * [VXLAN-EVPN konfiguracia sluzieb pre zakaznika *TenantA* na N94-Leaf4:](#vxlan-evpn-konfiguracia-sluzieb-pre-zakaznika-tenanta-na-n94-leaf4)
         * [VXLAN-EVPN konfiguracia sluzieb pre zakaznika *TenantA* na N95-Spine1:](#vxlan-evpn-konfiguracia-sluzieb-pre-zakaznika-tenanta-na-n95-spine1)
         * [VXLAN-EVPN konfiguracia sluzieb pre zakaznika *TenantA* na N96-Spine2:](#vxlan-evpn-konfiguracia-sluzieb-pre-zakaznika-tenanta-na-n96-spine2)
         * [VXLAN-EVPN konfiguracia sluzieb pre zakaznika *TenantB* na N91-Leaf1:](#vxlan-evpn-konfiguracia-sluzieb-pre-zakaznika-tenantb-na-n91-leaf1)
         * [VXLAN-EVPN konfiguracia sluzieb pre zakaznika *TenantB* na N92-Leaf2:](#vxlan-evpn-konfiguracia-sluzieb-pre-zakaznika-tenantb-na-n92-leaf2)
         * [VXLAN-EVPN konfiguracia sluzieb pre zakaznika *TenantB* na N93-Leaf3:](#vxlan-evpn-konfiguracia-sluzieb-pre-zakaznika-tenantb-na-n93-leaf3)
         * [VXLAN-EVPN konfiguracia sluzieb pre zakaznika *TenantB* na N94-Leaf4:](#vxlan-evpn-konfiguracia-sluzieb-pre-zakaznika-tenantb-na-n94-leaf4)
         * [VXLAN-EVPN konfiguracia sluzieb pre zakaznika *TenantB* na N95-Spine1:](#vxlan-evpn-konfiguracia-sluzieb-pre-zakaznika-tenantb-na-n95-spine1)
         * [VXLAN-EVPN konfiguracia sluzieb pre zakaznika *TenantB* na N96-Spine2:](#vxlan-evpn-konfiguracia-sluzieb-pre-zakaznika-tenantb-na-n96-spine2)
      * [Konfiguracia jednotlivych testovacich scenarov (poskytovatel aj zakaznik):](#konfiguracia-jednotlivych-testovacich-scenarov-poskytovatel-aj-zakaznik)
         * [1. Zakladna L2 konektivita (bez QinVNI), medzi 2 bodmi:](#1-zakladna-l2-konektivita-bez-qinvni-medzi-2-bodmi)
         * [2. Unicast L3 routing medzi L2 segmentami v ramci zakaznikovej VRF:](#2-unicast-l3-routing-medzi-l2-segmentami-v-ramci-zakaznikovej-vrf)

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

1. Zakladna L2 konektivita (bez QinVNI), medzi 2 bodmi bez QinVNI (A:SW1 + A:SW3)
   - funkcne na N9300V, aj multipoint
 
2. Unicast L3 routing medzi L2 segmentami v ramci zakaznikovej VRF (A:SW1+2+3+4)
   - funkcne na N9300V, aj multipoint

3. Transparentne prepojenie P-to-P cez VXLAN-Xconnect (B:SW1 + B:SW3)
   - NEfunkcne na N9300V, VM toto nepodporuje v kombinacii s vPC na NX-OSv ver. 9.3(10)
   - pozriet obmedzenia ohladom HW/SW a vPC (NX-OS 9.X vs. NX-OS 10.X)

4. Transparentne QinVNI prepojenie medzi 2 bodmi cez VXLAN dot1q tunnel (B:SW2 + B:SW4)
   - NEfunkcne na N9300V, VM toto nepodporuje v kombinacii s vPC na NX-OSv ver. 9.3(10)
     - malo by podporovat na REAL zeleze aj point-to-Multipoint, treba preskumat
   - pozriet obmedzenia ohladom HW/SW a vPC (NX-OS 9.X vs. NX-OS 10.X)

5. Externa konektivita do Inetu z VRF `TenantA` / `TenantB` (OSPFv2 + Inet-R1 + B:SW4)
   - viacero moznosti na hand-off L3 konektivity do Internetu, typicky s VRF+BGP
   - funkcne na N9300V v kombinacii CSR1000v s IOS-XE, aj multipoint
   - do BGP sa da propagovat v ramci VRF default routa s "network 0.0.0.0/0"
   - na Inet-R1 potrebny VRF aware NAT config
   - problem ked je roztiahnuta VXLAN, a border-gw do INetu je len na 1 Pod-e
     - bolo potrebne vypnut Dynamic AnycastGW na uplinkovych VLAN104+204 pre Tenant-X
     - Leaf1+2 (nema Inet GW) VS. Leaf3+4 (ma Inet GW)

   - zariadenia A:SW4 a B:SW4 maju rovnake IP ale s VRF+NAT sa dostanu v poriadku na Inet
     - priama adresacia zakaznikov vo VRF je samozrejme problematicka, idealne pouzit unikatne IP adresy
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
---

(N91-Leaf1) Priprava:
```
boot nxos bootflash:/nxos.9.3.10.bin sup-1
!
hostname N91-Leaf1
!
nv overlay evpn
feature ospf
feature bfd
feature bgp
feature interface-vlan
feature vn-segment-vlan-based
feature lacp
feature vpc
feature nv overlay
!
```

(N91-Leaf1) Konfiguracia Underlay routingu uplinkov voci Spine-layer:
```
router ospf as65001
  bfd
  router-id 192.0.2.91
  log-adjacency-changes
  auto-cost reference-bandwidth 4000 Gbps
!
interface Ethernet1/5
  description Underlay prepojenie z N91-Leaf1 na N95-Spine1
  no switchport
  mtu 9216
  bfd ipv4 interval 500 min_rx 500 multiplier 3
  no ip redirects
  ip address 10.1.5.1/24
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
  ip ospf bfd
  no shutdown
!
interface Ethernet1/6
  description Underlay prepojenie z N91-Leaf1 na N96-Spine2
  no switchport
  mtu 9216
  bfd ipv4 interval 500 min_rx 500 multiplier 3
  no ip redirects
  ip address 10.1.6.1/24
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
  ip ospf bfd
  no shutdown
!
``` 

(N91-Leaf1) Konfiguracia Pod vPC:
```
vpc domain 912
  peer-switch
  role priority 30
  peer-keepalive destination 172.16.12.2 source 172.16.12.1
  peer-gateway
  layer3 peer-router
  ip arp synchronize
!
vlan 912
  name vPC-peer-link-L3-backup
!
interface Vlan912
  description vPC-peer-link-L3-backup
  no shutdown
  bfd ipv4 interval 500 min_rx 500 multiplier 3
  no ip redirects
  ip address 10.9.9.11/29
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
  ip ospf bfd
!
interface port-channel89
  description Leaf1-vpc-peer-link-Leaf2
  switchport mode trunk
  switchport trunk native vlan 912
  spanning-tree port type network
  vpc peer-link
!
interface Ethernet1/8
  description Leaf1-vpc-peer-link-Leaf2
  switchport mode trunk
  switchport trunk native vlan 912
  spanning-tree port type network
  channel-group 89 mode active
!
interface Ethernet1/9
  description Leaf1-vpc-peer-link-Leaf2
  switchport mode trunk
  switchport trunk native vlan 912
  spanning-tree port type network
  channel-group 89 mode active
!
```

#### VXLAN Underlay konfiguracia pre `N92-Leaf2`:
---

(N92-Leaf2) Priprava:
```
boot nxos bootflash:/nxos.9.3.10.bin sup-1
!
hostname N92-Leaf2
!
nv overlay evpn
feature ospf
feature bfd
feature bgp
feature interface-vlan
feature vn-segment-vlan-based
feature lacp
feature vpc
feature nv overlay
!
```

(N92-Leaf2) Konfiguracia Underlay routingu uplinkov voci Spine-layer:
```
!
router ospf as65001
  bfd
  router-id 192.0.2.92
  log-adjacency-changes
  auto-cost reference-bandwidth 4000 Gbps
!
interface Ethernet1/5
  description Prepojenie z N92-Leaf2 na N95-Spine1
  no switchport
  mtu 9216
  bfd ipv4 interval 500 min_rx 500 multiplier 3
  no ip redirects
  ip address 10.2.5.2/24
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
  ip ospf bfd
  no shutdown
!
interface Ethernet1/6
  description Prepojenie z N92-Leaf2 na N96-Spine2
  no switchport
  mtu 9216
  bfd ipv4 interval 500 min_rx 500 multiplier 3
  no ip redirects
  ip address 10.2.6.2/24
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
  ip ospf bfd
  no shutdown
```

(N92-Leaf2) Konfiguracia Pod vPC:
```
vpc domain 912
  peer-switch
  role priority 20
  peer-keepalive destination 172.16.12.1 source 172.16.12.2
  peer-gateway
  layer3 peer-router
  ip arp synchronize
!
vlan 912
  name vPC-peer-link-L3-backup
!
interface Vlan912
  description vPC-peer-link-L3-backup
  no shutdown
  bfd ipv4 interval 500 min_rx 500 multiplier 3
  no ip redirects
  ip address 10.9.9.12/29
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
  ip ospf bfd
!
interface port-channel89
  description Leaf2-vpc-peer-link-Leaf1
  switchport mode trunk
  switchport trunk native vlan 912
  spanning-tree port type network
  vpc peer-link
!
interface Ethernet1/8
  description Leaf2-vpc-peer-link-Leaf1
  switchport mode trunk
  switchport trunk native vlan 912
  spanning-tree port type network
  channel-group 89 mode active
!
interface Ethernet1/9
  description Leaf2-vpc-peer-link-Leaf1
  switchport mode trunk
  switchport trunk native vlan 912
  spanning-tree port type network
  channel-group 89 mode active
!
```

#### VXLAN Underlay konfiguracia pre `N93-Leaf3`:
---

(N93-Leaf3) Priprava:
```
boot nxos bootflash:/nxos.9.3.10.bin sup-1
!
hostname N93-Leaf3
!
nv overlay evpn
feature ospf
feature bfd
feature bgp
feature interface-vlan
feature vn-segment-vlan-based
feature lacp
feature vpc
feature nv overlay
!
```

(N93-Leaf3) Konfiguracia Underlay routingu uplinkov voci Spine-layer:
```
!
router ospf as65001
  bfd
  router-id 192.0.2.93
  log-adjacency-changes
  auto-cost reference-bandwidth 4000 Gbps
!
interface Ethernet1/5
  description Prepojenie z N93-Leaf3 na N95-Spine1
  no switchport
  mtu 9216
  bfd ipv4 interval 500 min_rx 500 multiplier 3
  no ip redirects
  ip address 10.3.5.3/24
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
  ip ospf bfd
  no shutdown
!
interface Ethernet1/6
  description Prepojenie z N93-Leaf3 na N96-Spine2
  no switchport
  mtu 9216
  bfd ipv4 interval 500 min_rx 500 multiplier 3
  no ip redirects
  ip address 10.3.6.3/24
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
  ip ospf bfd
  no shutdown
!
```

(N93-Leaf3) Konfiguracia Pod vPC:
```
vpc domain 934
  peer-switch
  role priority 30
  peer-keepalive destination 172.16.34.4 source 172.16.34.3
  peer-gateway
  layer3 peer-router
  ip arp synchronize
!
vlan 934
  name vPC-peer-link-L3-backup
!
interface Vlan934
  description vPC-peer-link-L3-backup
  no shutdown
  bfd ipv4 interval 500 min_rx 500 multiplier 3
  no ip redirects
  ip address 10.9.9.33/29
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
  ip ospf bfd
!
interface port-channel89
  description Leaf3-vpc-peer-link-Leaf4
  switchport mode trunk
  switchport trunk native vlan 934
  spanning-tree port type network
  vpc peer-link
!
interface Ethernet1/8
  description Leaf3-vpc-peer-link-Leaf4
  switchport mode trunk
  switchport trunk native vlan 934
  spanning-tree port type network
  channel-group 89 mode active
!
interface Ethernet1/9
  description Leaf3-vpc-peer-link-Leaf4
  switchport mode trunk
  switchport trunk native vlan 934
  spanning-tree port type network
  channel-group 89 mode active
!
```

#### VXLAN Underlay konfiguracia pre `N94-Leaf4`:
---

(N94-Leaf4) Priprava:
```
boot nxos bootflash:/nxos.9.3.10.bin sup-1
!
hostname N94-Leaf4
!
nv overlay evpn
feature ospf
feature bfd
feature bgp
feature interface-vlan
feature vn-segment-vlan-based
feature lacp
feature vpc
feature nv overlay
!
```

(N94-Leaf4) Konfiguracia Underlay routingu uplinkov voci Spine-layer:
```
!
router ospf as65001
  bfd
  router-id 192.0.2.94
  log-adjacency-changes
  auto-cost reference-bandwidth 4000 Gbps
!
interface Ethernet1/5
  description Prepojenie z N94-Leaf4 na N95-Spine1
  no switchport
  mtu 9216
  bfd ipv4 interval 500 min_rx 500 multiplier 3
  no ip redirects
  ip address 10.4.5.4/24
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
  ip ospf bfd
  no shutdown
!
interface Ethernet1/6
  description Prepojenie z N94-Leaf4 na N96-Spine2
  no switchport
  mtu 9216
  bfd ipv4 interval 500 min_rx 500 multiplier 3
  no ip redirects
  ip address 10.4.6.4/24
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
  ip ospf bfd
  no shutdown
!
```

(N94-Leaf4) Konfiguracia Pod vPC:
```
vpc domain 934
  peer-switch
  role priority 20
  peer-keepalive destination 172.16.34.3 source 172.16.34.4
  peer-gateway
  layer3 peer-router
  ip arp synchronize
!
vlan 934
  name vPC-peer-link-L3-backup
!
interface Vlan934
  description vPC-peer-link-L3-backup
  no shutdown
  bfd ipv4 interval 500 min_rx 500 multiplier 3
  no ip redirects
  ip address 10.9.9.34/29
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
  ip ospf bfd
!
interface port-channel89
  description Leaf4-vpc-peer-link-Leaf3
  switchport mode trunk
  switchport trunk native vlan 934
  spanning-tree port type network
  vpc peer-link
!
interface Ethernet1/8
  description Leaf4-vpc-peer-link-Leaf3
  switchport mode trunk
  switchport trunk native vlan 934
  spanning-tree port type network
  channel-group 89 mode active
!
interface Ethernet1/9
  description Leaf4-vpc-peer-link-Leaf3
  switchport mode trunk
  switchport trunk native vlan 934
  spanning-tree port type network
  channel-group 89 mode active
!
```

#### VXLAN Underlay konfiguracia pre `N95-Spine1`:
---

(N95-Spine1) Priprava:
```
boot nxos bootflash:/nxos.9.3.10.bin sup-1
!
hostname N95-Spine1
!
nv overlay evpn
feature ospf
feature bfd
feature bgp
feature nv overlay
!
```

(N95-Spine1) Konfiguracia Underlay routingu uplinkov voci Leaf-layer:
```
!
router ospf as65001
  bfd
  router-id 192.0.2.95
  log-adjacency-changes
  auto-cost reference-bandwidth 4000 Gbps
!
interface Ethernet1/1
  description Prepojenie z N95-Spine1 na N91-Leaf1  
  no switchport
  mtu 9216
  bfd ipv4 interval 500 min_rx 500 multiplier 3
  no ip redirects
  ip address 10.1.5.5/24
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
  ip ospf bfd
  no shutdown
!
interface Ethernet1/2
  description Prepojenie z N95-Spine1 na N92-Leaf2
  no switchport
  mtu 9216
  bfd ipv4 interval 500 min_rx 500 multiplier 3
  no ip redirects
  ip address 10.2.5.5/24
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
  ip ospf bfd
  no shutdown
!
interface Ethernet1/3
  description Prepojenie z N95-Spine1 na N93-Leaf3
  no switchport
  mtu 9216
  bfd ipv4 interval 500 min_rx 500 multiplier 3
  no ip redirects
  ip address 10.3.5.5/24
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
  ip ospf bfd
  no shutdown
!
interface Ethernet1/4
  description Prepojenie z N95-Spine1 na N94-Leaf4
  no switchport
  mtu 9216
  bfd ipv4 interval 500 min_rx 500 multiplier 3
  no ip redirects
  ip address 10.4.5.5/24
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
  ip ospf bfd
  no shutdown
!
```

#### VXLAN Underlay konfiguracia pre `N96-Spine2`:
---

(N96-Spine2) Priprava:
```
boot nxos bootflash:/nxos.9.3.10.bin sup-1
!
hostname N96-Spine2
!
nv overlay evpn
feature ospf
feature bfd
feature bgp
feature nv overlay
!
```

(N96-Spine2) Konfiguracia Underlay routingu uplinkov voci Leaf-layer:
```
!
router ospf as65001
  bfd
  router-id 192.0.2.96
  log-adjacency-changes
  auto-cost reference-bandwidth 4000 Gbps
!
interface Ethernet1/1
  description Prepojenie z N96-Spine2 na N91-Leaf1
  no switchport
  mtu 9216
  bfd ipv4 interval 500 min_rx 500 multiplier 3
  no ip redirects
  ip address 10.1.6.6/24
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
  ip ospf bfd
  no shutdown
!
interface Ethernet1/2
  description Prepojenie z N96-Spine2 na N92-Leaf2
  no switchport
  mtu 9216
  bfd ipv4 interval 500 min_rx 500 multiplier 3
  no ip redirects
  ip address 10.2.6.6/24
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
  ip ospf bfd
  no shutdown
!
interface Ethernet1/3
  description Prepojenie z N96-Spine2 na switch N93-Leaf3
  no switchport
  mtu 9216
  bfd ipv4 interval 500 min_rx 500 multiplier 3
  no ip redirects
  ip address 10.3.6.6/24
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
  ip ospf bfd
  no shutdown
!
interface Ethernet1/4
  description Prepojenie z N96-Spine2 na N94-Leaf4
  no switchport
  mtu 9216
  bfd ipv4 interval 500 min_rx 500 multiplier 3
  no ip redirects
  ip address 10.4.6.6/24
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
  ip ospf bfd
  no shutdown
!
```
---
### Konfiguracia VXLAN-EVPN Overlay:

Na vymenu informacii medzi VTEP-mi sa pouziva protokol BGPv4 s rozsirenim AF-BGP-EVPN.
Kazdy Leaf switch ma teda 2 x iBGP peering na Spine1 a Spine2 box-y. Tieto Spine zariadenia
sluzia ako BGP Rouer-Reflector-y, aby nebolo nutne konfigurovat full-mesh iBGP peeringy.

#### VXLAN Overlay konfiguracia pre `N91-Leaf1`:
---

(N91-Leaf1) Konfiguracia `Loopback1023` rozhrania
```
interface loopback1023
  description main-RouterID-VTEP
  ip address 192.0.2.91/32              ! Sluzi aj ako ID pre VTEP 
  ip address 192.0.2.12/32 secondary    ! Sluzi ako ID pre spolocny vPC+VTEP
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
!
```

(N91-Leaf1) Konfiguracia VXLAN EVPN Overlay routingu s BGP:
```
router bgp 65001
  router-id 192.0.2.91
  log-neighbor-changes
  address-family ipv4 unicast
    maximum-paths 4             ! Nie je nutne, ale priprava na skalovanie
  address-family l2vpn evpn
    maximum-paths 4             ! Nie je nutne, ale priprava na skalovanie
!
  neighbor 192.0.2.95
    remote-as 65001
    description N95-Spine1
    password 3 07886e35fa43fb6d7ebaf3d037e25344
    update-source loopback1023
    address-family ipv4 unicast
      send-community
      send-community extended
    address-family l2vpn evpn
      send-community
      send-community extended
!
  neighbor 192.0.2.96
    remote-as 65001
    description N96-Spine2
    password 3 07886e35fa43fb6d7ebaf3d037e25344
    update-source loopback1023
    address-family ipv4 unicast
      send-community
      send-community extended
    address-family l2vpn evpn
      send-community
      send-community extended
!
```

#### VXLAN Overlay konfiguracia pre `N92-Leaf2`:
---

(N92-Leaf2) Konfiguracia `Loopback1023` rozhrania
```
interface loopback1023
  description main-RouterID-VTEP
  ip address 192.0.2.92/32              ! Sluzi aj ako ID pre VTEP
  ip address 192.0.2.12/32 secondary    ! Sluzi ako ID pre spolocny vPC+VTEP
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
!
```

(N92-Leaf2) Konfiguracia VXLAN EVPN Overlay routingu s BGP:
```
router bgp 65001
  router-id 192.0.2.92
  log-neighbor-changes
  address-family ipv4 unicast
    maximum-paths 4             ! Nie je nutne, ale priprava na skalovanie
  address-family l2vpn evpn
    maximum-paths 4             ! Nie je nutne, ale priprava na skalovanie
!
  neighbor 192.0.2.95
    remote-as 65001
    description N95-Spine1
    password 3 07886e35fa43fb6d7ebaf3d037e25344
    update-source loopback1023
    address-family ipv4 unicast
      send-community
      send-community extended
    address-family l2vpn evpn
      send-community
      send-community extended
!
  neighbor 192.0.2.96
    remote-as 65001
    description N96-Spine2
    password 3 07886e35fa43fb6d7ebaf3d037e25344
    update-source loopback1023
    address-family ipv4 unicast
      send-community
      send-community extended
    address-family l2vpn evpn
      send-community
      send-community extended
!
```

#### VXLAN Overlay konfiguracia pre `N93-Leaf3`:
---

(N93-Leaf3) Konfiguracia `Loopback1023` rozhrania
```
interface loopback1023
  description main-RouterID-VTEP
  ip address 192.0.2.93/32              ! Sluzi aj ako ID pre VTEP
  ip address 192.0.2.34/32 secondary    ! Sluzi ako ID pre spolocny vPC+VTEP
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
!
```

(N93-Leaf3) Konfiguracia VXLAN EVPN Overlay routingu s BGP:
```
router bgp 65001
  router-id 192.0.2.93
  log-neighbor-changes
  address-family ipv4 unicast
    maximum-paths 4             ! Nie je nutne, ale priprava na skalovanie
  address-family l2vpn evpn
    maximum-paths 4             ! Nie je nutne, ale priprava na skalovanie
!
  neighbor 192.0.2.95
    remote-as 65001
    description N95-Spine1
    password 3 07886e35fa43fb6d7ebaf3d037e25344
    update-source loopback1023
    address-family ipv4 unicast
      send-community
      send-community extended
    address-family l2vpn evpn
      send-community
      send-community extended
!
  neighbor 192.0.2.96
    remote-as 65001
    description N96-Spine2
    password 3 07886e35fa43fb6d7ebaf3d037e25344
    update-source loopback1023
    address-family ipv4 unicast
      send-community
      send-community extended
    address-family l2vpn evpn
      send-community
      send-community extended
!
```

#### VXLAN Overlay konfiguracia pre `N94-Leaf4`:
---

(N94-Leaf4) Konfiguracia `Loopback1023` rozhrania
```
interface loopback1023
  description main-RouterID-VTEP
  ip address 192.0.2.94/32              ! Sluzi aj ako ID pre VTEP
  ip address 192.0.2.34/32 secondary    ! Sluzi ako ID pre spolocny vPC+VTEP
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
!
```

(N94-Leaf4) Konfiguracia VXLAN EVPN Overlay routingu s BGP:
```
router bgp 65001
  router-id 192.0.2.94
  log-neighbor-changes
  address-family ipv4 unicast
    maximum-paths 4             ! Nie je nutne, ale priprava na skalovanie
  address-family l2vpn evpn
    maximum-paths 4             ! Nie je nutne, ale priprava na skalovanie
!
  neighbor 192.0.2.95
    remote-as 65001
    description N95-Spine1
    password 3 07886e35fa43fb6d7ebaf3d037e25344
    update-source loopback1023
    address-family ipv4 unicast
      send-community
      send-community extended
    address-family l2vpn evpn
      send-community
      send-community extended
!
  neighbor 192.0.2.96
    remote-as 65001
    description N96-Spine2
    password 3 07886e35fa43fb6d7ebaf3d037e25344
    update-source loopback1023
    address-family ipv4 unicast
      send-community
      send-community extended
    address-family l2vpn evpn
      send-community
      send-community extended
!
```

#### VXLAN Overlay konfiguracia pre `N95-Spine1`:
---

(N95-Spine1) Konfiguracia `Loopback1023` rozhrania
```
interface loopback1023
  description main-RouterID-VTEP
  ip address 192.0.2.95/32
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
!
```

(N95-Spine1) Konfiguracia VXLAN EVPN Overlay routingu s BGP:
```
router bgp 65001
  router-id 192.0.2.95
  log-neighbor-changes
  address-family ipv4 unicast
    maximum-paths 4             ! Nie je nutne, ale priprava na skalovanie
  address-family l2vpn evpn
    maximum-paths 4             ! Nie je nutne, ale priprava na skalovanie
    retain route-target all
!
  template peer VTEP-peers
    remote-as 65001
    password 3 07886e35fa43fb6d7ebaf3d037e25344
    update-source loopback1023
    address-family ipv4 unicast
      send-community
      send-community extended
      route-reflector-client
    address-family l2vpn evpn
      send-community
      send-community extended
      route-reflector-client
  neighbor 192.0.2.91
    inherit peer VTEP-peers
  neighbor 192.0.2.92
    inherit peer VTEP-peers
  neighbor 192.0.2.93
    inherit peer VTEP-peers
  neighbor 192.0.2.94
    inherit peer VTEP-peers
!
```

#### VXLAN Overlay konfiguracia pre `N96-Spine2`:
---

(N96-Spine2) Konfiguracia `Loopback1023` rozhrania
```
!
interface loopback1023
  description main-RouterID-VTEP
  ip address 192.0.2.96/32
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
!
```

(N96-Spine2) Konfiguracia VXLAN EVPN Overlay routingu s BGP:
```
!
router bgp 65001
  router-id 192.0.2.96
  log-neighbor-changes
  address-family ipv4 unicast
    maximum-paths 4             ! Nie je nutne, ale priprava na skalovanie
  address-family l2vpn evpn
    maximum-paths 4             ! Nie je nutne, ale priprava na skalovanie
    retain route-target all
!
  template peer VTEP-peers
    remote-as 65001
    password 3 07886e35fa43fb6d7ebaf3d037e25344
    update-source loopback1023
    address-family ipv4 unicast
      send-community
      send-community extended
      route-reflector-client
    address-family l2vpn evpn
      send-community
      send-community extended
      route-reflector-client
  neighbor 192.0.2.91
    inherit peer VTEP-peers
  neighbor 192.0.2.92
    inherit peer VTEP-peers
  neighbor 192.0.2.93
    inherit peer VTEP-peers
  neighbor 192.0.2.94
    inherit peer VTEP-peers
!
```

---
### Konfiguracia VXLAN-EVPN sluzieb na strane poskytovatela:

---
#### VXLAN-EVPN konfiguracia sluzieb pre zakaznika `TenantA` na `N91-Leaf1`:

(N91-Leaf1) Konfiguracia VLAN a VRF segmentov (VXLAN IRB domen)
```
!
vlan 101
  name TenantA-seg101-L2
  vn-segment 3100101
!
vlan 102
  name TenantA-seg102-L2
  vn-segment 3100102
!
vlan 103
  name TenantA-seg103-L2
  vn-segment 3100103
!
vlan 3100
  name L3-vni-for-TenantA
  vn-segment 3100
!
vrf context TenantA
  rd auto
  vni 3100
  address-family ipv4 unicast
    route-target both auto
    route-target both auto evpn
!
interface Vlan3100
  description L3-vni-for-TenantA
  no shutdown
  vrf member TenantA
  no ip redirects                   ! Povoluje L3 routing v ramci Tenant VRF
  ip forward
!
```

(N91-Leaf1) Konfiguracia VLAN SVI rozhrani (Dynamic Anycast Gateway)
```
!
fabric forwarding anycast-gateway-mac 2022.2022.2022  ! Zhodna MAC pre vsetky VTEP-y a Tenant-ov
!
interface Vlan101
  description TenantA-seg101-any-gw
  no shutdown
  vrf member TenantA
  no ip redirects
  ip address 192.168.1.254/24
  no ipv6 redirects
  fabric forwarding mode anycast-gateway
!
interface Vlan102
  description TenantA-seg102-any-gw
  no shutdown
  vrf member TenantA
  no ip redirects
  ip address 192.168.2.254/24
  no ipv6 redirects
  fabric forwarding mode anycast-gateway
!
interface Vlan103
  description TenantA-seg103-any-gw
  no shutdown
  vrf member TenantA
  no ip redirects
  ip address 192.168.3.254/24
  no ipv6 redirects
  fabric forwarding mode anycast-gateway
!
```

(N91-Leaf1) Konfiguracia VTEP rozhrania `nve1` a EVPN databazy
```
!
interface nve1
  no shutdown
  description N91-VTEP-nve1
  host-reachability protocol bgp
  source-interface loopback1023         ! Pouziju sa IPv4 adresy na routing a tunelovanie + vPC
!
  member vni 3100 associate-vrf
  member vni 3100101
    ingress-replication protocol bgp
  member vni 3100102
    ingress-replication protocol bgp
  member vni 3100103
    ingress-replication protocol bgp
!
evpn
  vni 3100101 l2
    rd auto
    route-target import auto
    route-target export auto
  vni 3100102 l2
    rd auto
    route-target import auto
    route-target export auto
  vni 3100103 l2
    rd auto
    route-target import auto
    route-target export auto
!
```

---
#### VXLAN-EVPN konfiguracia sluzieb pre zakaznika `TenantA` na `N92-Leaf2`:

(N92-Leaf2) Konfiguracia VLAN a VRF segmentov (VXLAN IRB domen)
```
*Na tomto zariadeni je konfiguracia zhodna s N91-Leaf1*
```

(N92-Leaf2) Konfiguracia VLAN SVI rozhrani (Dynamic Anycast Gateway)
```
*Na tomto zariadeni je konfiguracia zhodna s N91-Leaf1*
```

(N92-Leaf2) Konfiguracia VTEP rozhrania `nve1`
```
*Na tomto zariadeni je konfiguracia zhodna s N91-Leaf1*
```

---
#### VXLAN-EVPN konfiguracia sluzieb pre zakaznika `TenantA` na `N93-Leaf3`:

(N93-Leaf3) Konfiguracia VLAN a VRF segmentov (VXLAN IRB domen)
```
*Na tomto zariadeni je konfiguracia zhodna s N91-Leaf1*
```

(N93-Leaf3) Konfiguracia VLAN SVI rozhrani (Dynamic Anycast Gateway)
```
*Na tomto zariadeni je konfiguracia zhodna s N91-Leaf1*
```

(N93-Leaf3) Konfiguracia VTEP rozhrania `nve1`
```
*Na tomto zariadeni je konfiguracia zhodna s N91-Leaf1*
```

---
#### VXLAN-EVPN konfiguracia sluzieb pre zakaznika `TenantA` na `N94-Leaf4`:

(N94-Leaf4) Konfiguracia VLAN a VRF segmentov (VXLAN IRB domen)
```
*Na tomto zariadeni je konfiguracia zhodna s N91-Leaf1*
```

(N94-Leaf4) Konfiguracia VLAN SVI rozhrani (Dynamic Anycast Gateway)
```
*Na tomto zariadeni je konfiguracia zhodna s N91-Leaf1*
```

(N94-Leaf4) Konfiguracia VTEP rozhrania `nve1`
```
*Na tomto zariadeni je konfiguracia zhodna s N91-Leaf1*
```

#### VXLAN-EVPN konfiguracia sluzieb pre zakaznika `TenantA` na `N95-Spine1`:

(N95-Spine1) Konfiguracia VLAN a VRF segmentov (VXLAN IRB domen)
```
*Na tomto zariadeni nie je potrebna dalsia konfiguracia*
```

(N95-Spine1) Konfiguracia VLAN SVI rozhrani (Dynamic Anycast Gateway)
```
*Na tomto zariadeni nie je potrebna dalsia konfiguracia*
```

(N95-Spine1) Konfiguracia VTEP rozhrania `nve1`
```
*Na tomto zariadeni nie je potrebna dalsia konfiguracia*
```

#### VXLAN-EVPN konfiguracia sluzieb pre zakaznika `TenantA` na `N96-Spine2`:

(N96-Spine2) Konfiguracia VLAN a VRF segmentov (VXLAN IRB domen)
```
*Na tomto zariadeni nie je potrebna dalsia konfiguracia*
```

(N96-Spine2) Konfiguracia VLAN SVI rozhrani (Dynamic Anycast Gateway)
```
*Na tomto zariadeni nie je potrebna dalsia konfiguracia*
```

(N96-Spine2) Konfiguracia VTEP rozhrania `nve1`
```
*Na tomto zariadeni nie je potrebna dalsia konfiguracia*
```

---
#### VXLAN-EVPN konfiguracia sluzieb pre zakaznika `TenantB` na `N91-Leaf1`:

(N91-Leaf1) Konfiguracia VLAN a VRF segmentov (VXLAN IRB domen)
```
! *Doplnujeme konfiguraciu o dalsieho zakaznika "TenantB"*
!
vlan 201
  name TenantB-seg201-L2
  vn-segment 3200201
!
vlan 202
  name TenantB-seg202-L2
  vn-segment 3200202
!
vlan 203
  name TenantB-seg203-L2
  vn-segment 3200203
!
vlan 3200
  name L3-vni-for-TenantB
  vn-segment 3200
!
vrf context TenantB
  vni 3200
  rd auto
  address-family ipv4 unicast
    route-target both auto
    route-target both auto evpn
!
interface Vlan3200
  description L3-vni-for-TenantB
  no shutdown
  vrf member TenantB
  no ip redirects
  ip forward                        ! Povoluje L3 routing v ramci Tenant VRF
!
```

(N91-Leaf1) Konfiguracia VLAN SVI rozhrani (Dynamic Anycast Gateway)
```
! *Doplnujeme konfiguraciu o dalsieho zakaznika "TenantB"*
!
interface Vlan201
  description TenantB-seg201-any-gw
  no shutdown
  vrf member TenantB
  no ip redirects
  ip address 192.168.1.254/24
  no ipv6 redirects
  fabric forwarding mode anycast-gateway
!
interface Vlan202
  description TenantB-seg202-any-gw
  no shutdown
  vrf member TenantB
  no ip redirects
  ip address 192.168.2.254/24
  no ipv6 redirects
  fabric forwarding mode anycast-gateway
!
interface Vlan203
  description TenantB-seg203-any-gw
  no shutdown
  vrf member TenantB
  no ip redirects
  ip address 192.168.3.254/24
  no ipv6 redirects
  fabric forwarding mode anycast-gateway
!
```

(N91-Leaf1) Konfiguracia VTEP rozhrania `nve1` a EVPN databazy 
```
! *Doplnujeme konfiguraciu o dalsieho zakaznika "TenantB"*
!
interface nve1
  member vni 3200 associate-vrf
  member vni 3200201
    ingress-replication protocol bgp
  member vni 3200202
    ingress-replication protocol bgp
  member vni 3200203
    ingress-replication protocol bgp
!
evpn
  vni 3200201 l2
    rd auto
    route-target import auto
    route-target export auto
  vni 3200202 l2
    rd auto
    route-target import auto
    route-target export auto
  vni 3200203 l2
    rd auto
    route-target import auto
    route-target export auto
!
```

---
#### VXLAN-EVPN konfiguracia sluzieb pre zakaznika `TenantB` na `N92-Leaf2`:

(N92-Leaf2) Konfiguracia VLAN a VRF segmentov (VXLAN IRB domen)
```
*Na tomto zariadeni je konfiguracia zhodna s N91-Leaf1*
```

(N92-Leaf2) Konfiguracia VLAN SVI rozhrani (Dynamic Anycast Gateway)
```
*Na tomto zariadeni je konfiguracia zhodna s N91-Leaf1*
```

(N92-Leaf2) Konfiguracia VTEP rozhrania `nve1`
```
*Na tomto zariadeni je konfiguracia zhodna s N91-Leaf1*
```

---
#### VXLAN-EVPN konfiguracia sluzieb pre zakaznika `TenantB` na `N93-Leaf3`:

(N93-Leaf3) Konfiguracia VLAN a VRF segmentov (VXLAN IRB domen)
```
*Na tomto zariadeni je konfiguracia zhodna s N91-Leaf1*
```

(N93-Leaf3) Konfiguracia VLAN SVI rozhrani (Dynamic Anycast Gateway)
```
*Na tomto zariadeni je konfiguracia zhodna s N91-Leaf1*
```

(N93-Leaf3) Konfiguracia VTEP rozhrania `nve1`
```
*Na tomto zariadeni je konfiguracia zhodna s N91-Leaf1*
```

---
#### VXLAN-EVPN konfiguracia sluzieb pre zakaznika `TenantB` na `N94-Leaf4`:

(N94-Leaf4) Konfiguracia VLAN a VRF segmentov (VXLAN IRB domen)
```
*Na tomto zariadeni je konfiguracia zhodna s N91-Leaf1*
```

(N94-Leaf4) Konfiguracia VLAN SVI rozhrani (Dynamic Anycast Gateway)
```
*Na tomto zariadeni je konfiguracia zhodna s N91-Leaf1*
```

(N94-Leaf4) Konfiguracia VTEP rozhrania `nve1`
```
*Na tomto zariadeni je konfiguracia zhodna s N91-Leaf1*
```

#### VXLAN-EVPN konfiguracia sluzieb pre zakaznika `TenantB` na `N95-Spine1`:

(N95-Spine1) Konfiguracia VLAN a VRF segmentov (VXLAN IRB domen)
```
*Na tomto zariadeni nie je potrebna dalsia konfiguracia*
```

(N95-Spine1) Konfiguracia VLAN SVI rozhrani (Dynamic Anycast Gateway)
```
*Na tomto zariadeni nie je potrebna dalsia konfiguracia*
```

(N95-Spine1) Konfiguracia VTEP rozhrania `nve1`
```
*Na tomto zariadeni nie je potrebna dalsia konfiguracia*
```

#### VXLAN-EVPN konfiguracia sluzieb pre zakaznika `TenantB` na `N96-Spine2`:

(N96-Spine2) Konfiguracia VLAN a VRF segmentov (VXLAN IRB domen)
```
*Na tomto zariadeni nie je potrebna dalsia konfiguracia*
```

(N96-Spine2) Konfiguracia VLAN SVI rozhrani (Dynamic Anycast Gateway)
```
*Na tomto zariadeni nie je potrebna dalsia konfiguracia*
```

(N96-Spine2) Konfiguracia VTEP rozhrania `nve1`
```
*Na tomto zariadeni nie je potrebna dalsia konfiguracia*
```

---
### Konfiguracia jednotlivych testovacich scenarov (poskytovatel aj zakaznik):

#### 1. Zakladna L2 konektivita (bez QinVNI), medzi 2 bodmi: 
```
  - zakaznik vyuziva zariadenia "Tenant-A-SW1" a "Tenant-A-SW3"
  - je mozne prepojit aj viac bodov / endpoint-ov
  - funckne pri testovani na N9300v
```

(N91-Leaf1 + N92-Leaf2) Konfiguracia vPC voci zakaznikovi:

```
!
interface port-channel11
  description downlink-to-Tenant-A-SW1-vPC11
  no cdp enable
  switchport access vlan 101
  spanning-tree port type edge
  spanning-tree bpduguard enable
  mtu 9216
  storm-control broadcast level 1.00
  storm-control action trap
  vpc 11
!
interface Ethernet1/11
  description downlink-to-Tenant-A-SW1-vPC11
  no cdp enable
  switchport access vlan 101
  spanning-tree port type edge
  spanning-tree bpduguard enable
  mtu 9216
  storm-control broadcast level 1.00
  storm-control action trap
  channel-group 11 mode active
!
```

(N93-Leaf3 + N94-Leaf4) Konfiguracia vPC voci zakaznikovi:
```
!
interface port-channel13
  description downlink-to-Tenant-A-SW3-vPC13
  no cdp enable
  switchport access vlan 101
  spanning-tree port type edge
  spanning-tree bpduguard enable
  mtu 9216
  storm-control broadcast level 1.00
  storm-control action trap
  vpc 13
!
interface Ethernet1/13
  description downlink-to-Tenant-A-SW3-vPC13
  no cdp enable
  switchport access vlan 101
  spanning-tree port type edge
  spanning-tree bpduguard enable
  mtu 9216
  storm-control broadcast level 1.00
  storm-control action trap
  channel-group 13 mode active
!
```

(Tenant-A-SW1) Konfiguracia L2-switched uplink-u voci poskytovatelovi:
```
!
interface Port-channel11
 description uplink-to-provider-AS65001
 switchport access vlan 101                 ! VLAN ID moze byt u zakaznika odlisne
 spanning-tree portfast edge
 spanning-tree bpdufilter enable
!
interface GigabitEthernet3/1
 description uplink-to-provider-AS65001
 switchport access vlan 101
 negotiation auto
 channel-protocol lacp
 channel-group 11 mode active
 spanning-tree portfast edge
 spanning-tree bpdufilter enable
!
interface GigabitEthernet3/2
 description uplink-to-provider-AS65001
 switchport access vlan 101
 negotiation auto
 channel-protocol lacp
 channel-group 11 mode active
 spanning-tree portfast edge
 spanning-tree bpdufilter enable
!
interface Vlan101
 description uplink-to-provider-AS65001
 ip address 192.168.1.1 255.255.255.0
 no ip redirects
 no ip unreachables
 no ip proxy-arp
!
```

(Tenant-A-SW3) Konfiguracia L3-routed uplink-u voci poskytovatelovi:
```
!
interface Port-channel13
 description uplink-to-provider-AS65001
 no cdp enable
 no switchport
 ip address 192.168.1.3 255.255.255.0
 no ip redirects
 no ip unreachables
 no ip proxy-arp
!
interface GigabitEthernet3/1
 description uplink-to-provider-AS65001
 no cdp enable
 no switchport
 no ip address
 negotiation auto
 channel-protocol lacp
 channel-group 13 mode active
!
interface GigabitEthernet3/2
 description uplink-to-provider-AS65001
 no cdp enable
 no switchport
 no ip address
 negotiation auto
 channel-protocol lacp
 channel-group 13 mode active
!
```

#### 2. Unicast L3 routing medzi L2 segmentami v ramci zakaznikovej VRF:
```
  - zakaznik vyuziva zariadenia "Tenant-A-SW1", "Tenant-A-SW2", "Tenant-A-SW3" a "Tenant-A-SW4"
  - funckne pri testovani na N9300v
```

(N91-Leaf1 + N92-Leaf2 + N93-Leaf3 + N94-Leaf4) Konfiguracia VXLAN routingu:
```
** Konfiguracia routingu v ramci VXLAN VRF bola vykonana v casti pripravy VXLAN sluzieb **
```

(N91-Leaf1 + N92-Leaf2) Konfiguracia vPC voci zakaznikovi:
```
! *Doplnime konfiguraciu pre koncove zariadenie "Tenant-A-SW2"*
!
interface port-channel12
  description downlink-to-Tenant-A-SW2-vPC12
  switchport access vlan 102
  spanning-tree port type edge
  spanning-tree bpduguard enable
  mtu 9216
  storm-control broadcast level 1.00
  storm-control action trap
  vpc 12
!
interface Ethernet1/12
  description downlink-to-Tenant-A-SW2-vPC12
  switchport access vlan 102
  spanning-tree port type edge
  spanning-tree bpduguard enable
  mtu 9216
  storm-control broadcast level 1.00
  storm-control action trap
  channel-group 12 mode active
!
```

(N93-Leaf3 + N94-Leaf4) Konfiguracia vPC voci zakaznikovi:
```
! *Doplnime konfiguraciu pre koncove zariadenie "Tenant-A-SW4"*
!
interface port-channel14
  description downlink-to-Tenant-A-SW2-vPC12
  switchport access vlan 103
  spanning-tree port type edge
  spanning-tree bpduguard enable
  mtu 9216
  storm-control broadcast level 1.00
  storm-control action trap
  vpc 14
!
interface Ethernet1/14
  description downlink-to-Tenant-A-SW2-vPC12
  switchport access vlan 103
  spanning-tree port type edge
  spanning-tree bpduguard enable
  mtu 9216
  storm-control broadcast level 1.00
  storm-control action trap
  channel-group 14 mode active
!
```

(Tenant-A-SW2) Konfiguracia L3-routed uplink-u voci poskytovatelovi:
```
!
interface Port-channel12
 description uplink-to-provider-AS65001
 no cdp enable
 no switchport
 ip address 192.168.2.2 255.255.255.0
 no ip redirects
 no ip unreachables
 no ip proxy-arp
!
interface GigabitEthernet3/1
 description uplink-to-provider-AS65001
 no cdp enable
 no switchport
 no ip address
 negotiation auto
 channel-group 12 mode active
!
interface GigabitEthernet3/2
 description uplink-to-provider-AS65001
 no cdp enable
 no switchport
 no ip address
 negotiation auto
 channel-group 12 mode active
end
```

(Tenant-A-SW4) Konfiguracia L2-switched uplink-u voci poskytovatelovi:
```
!
interface Port-channel14
 description uplink-to-provider-AS65001
 no cdp enable
 switchport access vlan 103
 spanning-tree portfast edge
 spanning-tree bpdufilter enable
!
interface GigabitEthernet3/1
 description uplink-to-provider-AS65001
 no cdp enable
 switchport access vlan 103
 negotiation auto
 channel-group 14 mode active
 spanning-tree portfast edge
 spanning-tree bpdufilter enable
!
interface GigabitEthernet3/2
 description uplink-to-provider-AS65001
 no cdp enable
 switchport access vlan 103
 negotiation auto
 channel-group 14 mode active
 spanning-tree portfast edge
 spanning-tree bpdufilter enable
!
interface Vlan103
 description uplink-to-provider-AS65001
 ip address 192.168.3.4 255.255.255.0
 no ip redirects
 no ip unreachables
 no ip proxy-arp
end
!

(N91-Leaf1 + N92-Leaf2 + N93-Leaf3 + N94-Leaf4) Konfiguracia routingu u zakaznika:
```
!
! Routing pre zariadenie Tenant-A-SW1
!
ip route 0.0.0.0 0.0.0.0 192.168.1.254 name default-route-to-AS65001
end
!
! Routing pre zariadenie Tenant-A-SW2
!
ip route 0.0.0.0 0.0.0.0 192.168.2.254 name default-route-to-AS65001
end
!
! Routing pre zariadenie Tenant-A-SW3
!
ip route 0.0.0.0 0.0.0.0 192.168.1.254 name default-route-to-AS65001
end
!
! Routing pre zariadenie Tenant-A-SW4
!
ip route 0.0.0.0 0.0.0.0 192.168.3.254 name default-route-to-AS65001
end
!
```

