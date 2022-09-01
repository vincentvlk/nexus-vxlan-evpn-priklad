# Cisco Nexus 9300v VXLAN BGP-EVPN - Priklad

**UPOZORNENIE: Tento dokument NESLUZI ako vzdelavaci material. Toto su moje studijne
poznamky, ktore postupne menim a doplnam.**

![GNS3 - testovacia VXLAN-EVPN topologia](https://github.com/vincentvlk/nexus-vxlan-evpn-priklad/blob/main/gh-gns3-vxlan-evpn-topologia.png)

# Obsah:

* [Cisco Nexus 9300v VXLAN BGP-EVPN - Priklad](#cisco-nexus-9300v-vxlan-bgp-evpn---priklad)
   * [Pomocne poznamky na mini-projekt "nexus-vxlan-evpn-priklad"](#pomocne-poznamky-na-mini-projekt-nexus-vxlan-evpn-priklad)
      * [Pouzite GNS3 a VM image:](#pouzite-gns3-a-vm-image)
      * [Postrehy z konfiguracie a testovania:](#postrehy-z-konfiguracie-a-testovania)
      * [Testovane scenare:](#testovane-scenare)
      * [Konfiguracia VXLAN-EVPN Underlay](#konfiguracia-vxlan-evpn-underlay)
         * [VXLAN Underlay konfiguracia pre `N91-Leaf1`:](#vxlan-underlay-konfiguracia-pre-n91-leaf1)
         * [VXLAN Underlay konfiguracia pre `N92-Leaf2`:](#vxlan-underlay-konfiguracia-pre-n92-leaf2)
         * [VXLAN Underlay konfiguracia pre `N93-Leaf3`:](#vxlan-underlay-konfiguracia-pre-n93-leaf3)
         * [VXLAN Underlay konfiguracia pre `N94-Leaf4`:](#vxlan-underlay-konfiguracia-pre-n94-leaf4)
         * [VXLAN Underlay konfiguracia pre `N95-Spine1`:](#vxlan-underlay-konfiguracia-pre-n95-spine1)
         * [VXLAN Underlay konfiguracia pre `N96-Spine2`:](#vxlan-underlay-konfiguracia-pre-n96-spine2)
      * [Konfiguracia VXLAN-EVPN Overlay:](#konfiguracia-vxlan-evpn-overlay)
         * [VXLAN Overlay konfiguracia pre `N91-Leaf1`:](#vxlan-overlay-konfiguracia-pre-n91-leaf1)
         * [VXLAN Overlay konfiguracia pre `N92-Leaf2`:](#vxlan-overlay-konfiguracia-pre-n92-leaf2)
         * [VXLAN Overlay konfiguracia pre `N93-Leaf3`:](#vxlan-overlay-konfiguracia-pre-n93-leaf3)
         * [VXLAN Overlay konfiguracia pre `N94-Leaf4`:](#vxlan-overlay-konfiguracia-pre-n94-leaf4)
         * [VXLAN Overlay konfiguracia pre `N95-Spine1`:](#vxlan-overlay-konfiguracia-pre-n95-spine1)
         * [VXLAN Overlay konfiguracia pre `N96-Spine2`:](#vxlan-overlay-konfiguracia-pre-n96-spie2)
      * [Konfiguracia VXLAN-EVPN sluzieb na strane poskytovatela:](#konfiguracia-vxlan-evpn-sluzieb-na-strane-poskytovatela)
         * [VXLAN-EVPN konfiguracia sluzieb pre zakaznika `TenantA` na `N91-Leaf1`:](#vxlan-evpn-konfiguracia-sluzieb-pre-zakaznika-tenanta-na-n91-leaf1)
         * [VXLAN-EVPN konfiguracia sluzieb pre zakaznika `TenantA` na `N92-Leaf2`:](#vxlan-evpn-konfiguracia-sluzieb-pre-zakaznika-tenanta-na-n92-leaf2)
         * [VXLAN-EVPN konfiguracia sluzieb pre zakaznika `TenantA` na `N93-Leaf3`:](#vxlan-evpn-konfiguracia-sluzieb-pre-zakaznika-tenanta-na-n93-leaf3)
         * [VXLAN-EVPN konfiguracia sluzieb pre zakaznika `TenantA` na `N94-Leaf4`:](#vxlan-evpn-konfiguracia-sluzieb-pre-zakaznika-tenanta-na-n94-leaf4)
         * [VXLAN-EVPN konfiguracia sluzieb pre zakaznika `TenantA` na `N95-Spine1`:](#vxlan-evpn-konfiguracia-sluzieb-pre-zakaznika-tenanta-na-n95-spine1)
         * [VXLAN-EVPN konfiguracia sluzieb pre zakaznika `TenantA` na `N96-Spine2`:](#vxlan-evpn-konfiguracia-sluzieb-pre-zakaznika-tenanta-na-n96-spine2)
         * [VXLAN-EVPN konfiguracia sluzieb pre zakaznika `TenantB` na `N91-Leaf1`:](#vxlan-evpn-konfiguracia-sluzieb-pre-zakaznika-tenantb-na-n91-leaf1)
         * [VXLAN-EVPN konfiguracia sluzieb pre zakaznika `TenantB` na `N92-Leaf2`:](#vxlan-evpn-konfiguracia-sluzieb-pre-zakaznika-tenantb-na-n92-leaf2)
         * [VXLAN-EVPN konfiguracia sluzieb pre zakaznika `TenantB` na `N93-Leaf3`:](#vxlan-evpn-konfiguracia-sluzieb-pre-zakaznika-tenantb-na-n93-leaf3)
         * [VXLAN-EVPN konfiguracia sluzieb pre zakaznika `TenantB` na `N94-Leaf4`:](#vxlan-evpn-konfiguracia-sluzieb-pre-zakaznika-tenantb-na-n94-leaf4)
         * [VXLAN-EVPN konfiguracia sluzieb pre zakaznika `TenantB` na `N95-Spine1`:](#vxlan-evpn-konfiguracia-sluzieb-pre-zakaznika-tenantb-na-n95-spine1)
         * [VXLAN-EVPN konfiguracia sluzieb pre zakaznika `TenantB` na `N96-Spine2`:](#vxlan-evpn-konfiguracia-sluzieb-pre-zakaznika-tenantb-na-n96-spine2)
      * [Konfiguracia jednotlivych testovacich scenarov (poskytovatel aj zakaznik):](#konfiguracia-jednotlivych-testovacich-scenarov-poskytovatel-aj-zakaznik)
         * [1. Zakladna L2 konektivita (bez QinVNI), medzi 2 bodmi:](#1-zakladna-l2-konektivita-bez-qinvni-medzi-2-bodmi)
         * [2. Unicast L3 routing medzi L2 segmentami v ramci zakaznikovej VRF:](#2-unicast-l3-routing-medzi-l2-segmentami-v-ramci-zakaznikovej-vrf)
         * [3. Transparentne prepojenie P-to-P cez VXLAN-Xconnect:](#3-transparentne-prepojenie-p-to-p-cez-vxlan-xconnect)
         * [4. Transparentne QinVNI prepojenie medzi 2 bodmi cez VXLAN dot1q tunnel:](#4-transparentne-qinvni-prepojenie-medzi-2-bodmi-cez-vxlan-dot1q-tunnel)
         * [5. Externa konektivita do Inetu z VRF `TenantA` / `TenantB`:](#5-externa-konektivita-do-inetu-z-vrf-tenanta--tenantb)
      * [Testovanie "Failure" scenarov:](#testovanie-failure-scenarov)
         * [1. Scenar, zlyhanie VXLAN/VTEP/Leaf zariadeni vo VXLAN fabricu:](#1-scenar-zlyhanie-vxlanvtepleaf-zariadeni-vo-vxlan-fabricu)
         * [2. Scenar, zlyhanie VXLAN/Spine zariadenia vo VXLAN fabricu:](#2-scenar-zlyhanie-vxlanspine-zariadenia-vo-vxlan-fabricu)
      * [Pouzivane skratky v dokumente:](#pouzivane-skratky-v-dokumente)
      * [Pouzivane diagnosticke prikazy:](#pouzivane-diagnosticke-prikazy)
      * [Skript v jazyku Python3 na generovanie zakladnej VXLAN-EVPN konfiguracie:](#skript-v-jazyku-python3-na-generovanie-zakladnej-vxlan-evpn-konfiguracie)
      * [Obsah tejto stranky bol vygenerovany BASH skritpom:](#obsah-tejto-stranky-bol-vygenerovany-bash-skritpom)

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
  - pravdepodobne zalezitost `GNS3+Qemu+KVM`, problem, pretoze reboot trva velmi dlho

- PROBLEM: Instancia N9300v pri boote "spadne" do User-space interpretu BASH pod systemom NX-OS
  - napr. vypis: `sh-4.3# ...`
  - zatial som riesil s prikazom `sh-4.3# reboot`
  - neviem zatial ci sa da z User-space prostredia znova spustit boot-seq celeho NX-OS

- PROBLEM: N9300V nabehne s neaktivnymi Ifaces, aj ked su zapnute v start-conf
  - stacil `shutdown` / `no shutdown`, sposobuje hlavne vypadok OSPF a teda *Underlay*
  - plati aj pre L3-SVI na N9300v, problemy s OSPFv2 voci IOS-XE
  - pravdepodobne zalezitost `GNS3+Qemu+KVM`, nic vazne

- problem, na strane zakaznika flapuje port-channel ked je v mode `routed` (vIOS-L3-Cat)
  - pravdepodobne zalezitost `GNS3+Qemu+KVM`, nic vazne, ale otravuju logy na konzole :)

- problem, s `BPDU guard`, aj ked je na druhej strane `BPDUfilter`, vypina uplinky
  - este treba otestovat, pretoze problem sa v niektorych pripadoch neprejavil
  - pravdepodobne zalezitost `GNS3+Qemu+KVM`

- problem, nebolo mozne kontaktovat VTEP-Anycast-GW SVI, 
  - trebalo vycistit ARP zaznam na zakaznickom switchy (vIOS-L3-Cat)

- problem s HA, ked vypnem jednu stranu VLAN101 na vPC Peer-e, napr. `N91-Leaf1`
  - nestandarny stav, aj tak treba vypinat celu fyz. cestu v pripade maint.

- conf. backup s: `copy running-config scp://vlkv@192.168.4.244/ vrf default`

- `XConnect` a `QinVNI` je pri vPC obmedzene, treba pouzit prikazy:

  `system dot1q-tunnel transit [vlan vlan-range]` 
   
  spolu s:
  `system nve infra-vlans <vlan-range>`

  - TREBA SI UVEDOMIT, ze tieto 2 funkcie (XConnect/QinVNI) su *SILNE ZAVISLE* od VTEP-HW (ASICov)

Poznamka z dokumentacie:
```
For proper operation during L3 uplink failure scenarios on vPC VTEPs,
configure a backup SVI and enter the
"system nve infra-vlans <backup-svi-vlan>" command.

On Cisco Nexus 9000-EX platform switches,
the backup SVI VLAN needs to be the native VLAN on the peer-link.

 - pouzity Nexus9300v (NX-OSv 9.3.10) tieto 2 prikazy nema, nenasiel som zatial
```

- problem, nejde IPv4 BFD s OSPFv2, prikazy su, ale nezdvihne sa session
  - pravdepodobne zalezitost `GNS3+Qemu+KVM`
  - preto konfigurovane len experimentalne, aby som na to nezabudol

- pozorovanie, VTEP automaticky presmeruva IP traffic na svoju VLAN-gw aj ked
  ma CE zariadenie nastavenu inu L3 gw, ktora je L2 dostupna, ma aj ARP zaznam
  - pozriet ci sa da vypnut/zmenit?

- pozorovanie, fyz. porty, ktore su definovane ako `XConnect-dot1q-tunn`, technologia
  vPC nepovazuje za `Orphan` porty, vid.: `show vpc orphan-ports`

- poznamka, tento dokument sa (zatial) nevenuje ochrane pred tzv. BUM traffic-om
  - napr. pouzitie konceptu Storm-Control
  - nevenuje sa ani problematike ARP Suppression
  - da sa dohladat v pouzitych zdrojoch informacii

- poznamka, na testovanie som vyuzil NX-OS funkciu "Configuration Rollback":

```
Cisco Nexus 9000 Series NX-OS System Management Configuration Guide, Release 9.3(x):

https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus9000/sw/93x/system-management/b-cisco-nexus-9000-series-nx-os-system-management-configuration-guide-93x/b-cisco-nexus-9000-series-nx-os-system-management-configuration-guide-93x_chapter_011001.html
```

---
### Testovane scenare:

1. Zakladna L2 konektivita (bez QinVNI), medzi 2 bodmi bez QinVNI (A:SW1 + A:SW3)
   - funkcne na N9300V + IOSv, aj multipoint
 
2. Unicast L3 routing medzi L2 segmentami v ramci zakaznikovej VRF (A:SW1+2+3+4)
   - funkcne na N9300V + IOSv, aj multipoint

3. Transparentne prepojenie P-to-P cez VXLAN-XConnect (B:SW1 + B:SW3)
   - NEfunkcne na N9300V, VM toto nepodporuje v kombinacii s vPC na NX-OSv ver. 9.3(10)
   - pozriet obmedzenia ohladom HW/SW a vPC (NX-OS 9.X vs. NX-OS 10.X)
   - otazka, aka je podpora QinVNI s pripojenim zakaznika do FEX-ov
   - otazka, ci sa da nasadit BW rate-limit / shaping, zatial neviem

4. Transparentne QinVNI prepojenie medzi 2 bodmi cez VXLAN dot1q tunnel (B:SW2 + B:SW4)
   - NEfunkcne na N9300V, VM toto nepodporuje v kombinacii s vPC na NX-OSv ver. 9.3(10)
     - malo by podporovat na REAL zeleze aj point-to-Multipoint, treba preskumat
   - pozriet obmedzenia ohladom HW/SW a vPC (NX-OS 9.X vs. NX-OS 10.X)
   - OTESTOVAT variantu, kedy je provider port na inom zariadeni ako VTEP-e a treba QinVNI transport
   - otazka, aka je podpora QinVNI s pripojenim zakaznika do FEX-ov
   - otazka, ci sa da nasadit BW rate-limit / shaping, zatial neviem

5. Externa konektivita do Inetu z VRF `TenantA` / `TenantB` (OSPFv2 + Inet-R1 + B:SW4)
   - viacero moznosti na hand-off L3 konektivity do Internetu, typicky s VRF+BGP
   - funkcne na N9300V v kombinacii CSR1000v s IOS-XE, aj multipoint
   - do BGP sa da propagovat v ramci VRF default routa s "network 0.0.0.0/0"
   - na Inet-R1 potrebny VRF aware NAT config
   - problem ked je roztiahnuta VXLAN, a border-gw do INetu je len na 1 Pod-e
     - bolo potrebne vypnut Dynamic Anycast GW na uplinkovych VLAN104+204 pre Tenant-X
     - `Leaf1+2` (NEmaju pripojenu Inet GW) verzus `Leaf3+4` (maju pripojenu Inet GW)

   - zariadenia A:SW4 a B:SW4 maju rovnake IP ale s VRF+NAT sa dostanu v poriadku na Inet
     - priama adresacia zakaznikov vo VRF je samozrejme problematicka, idealne pouzit unikatne IP adresy

---
### Konfiguracia VXLAN-EVPN Underlay

Tento testovaci dizajn pocita s VXLAN Tenant separaciou (pomocou VRF). Je vsak pouzitelny
aj na zakladne L2 transporty medzi portami/lokalitami v ramci Default VRF.
Ako je (snad) vidiet na diagrame, na VXLAN fabric je pouzitych 6 Nexus 9300v VM instancii.
Z toho 2 su pouzite ako Spine box-y, ktore sluzia len ako VXLAN EVPN Route-reflectory a L3 routery.
Zvysne 4 box-y su pouzite ako Leaf VTEP zariadenia, ktore vykonavanju VXLAN Tunnel encaps/decaps a
L2/L3 bridging/routing v ramci VXLAN Tenantov (teda v ramci L3 VRF/VNI).

Z diagramu je vidiet, ze kazdy DC-Pod (ulicka, sala, poschodie, ...) je tvoreny parom
Leaf Switchov, teda mame 2 Pod-y, konkretne (`N91-Leaf1` + `N92-Leaf2`) a (`N93-Leaf3` + `N94-Leaf4`).
Kazdy Pod je este v ramci Ethernet MLAG redundancie nakonfigurovany ako vPC domena.
Leaf switche maju symetricku L3-routed (Underlay) konektivitu na 2 Spine switch-e (`N95-Spine1` + `N96-Spine2`).

Snahou dizajnu je zabezpecit nezavislu a redundantnu konektivitu tak, aby
v pripade vypadku uzlov, napr. (`Leaf1`, `Spine1` a `Leaf3`) alebo
napr. (`Leaf2`, `Spine2` a `Leaf4`), bola stale zachovana konektivita pre zakanzikov, aj ked
s degradovanou sirkou pasma (BW).

Mal som povodny zamer, ze ako underlay pouzijem IPv6 + OSPFv3, tento koncept
ma vsak stale (Jul 2022) obmedzene moznoti a funkcie.
Pre Underlay bol pouzity IGP protokol OSPFv2. Protokol OSPFv3, na NX-OS na rozdiel od IOSu, 
nedokaze propagovat IPv4 prefixy, alebo som to zatial nenasiel/nepochopil.
Konfiguracia routingu je standardna, vyuzivaju sa Loopback rozhrania, ktore su
dolezite pre funckiu VXLAN fabricu, pretoze sa mapuju na VXLAN VTEP rozhranie `nve1`.

---
#### VXLAN Underlay konfiguracia pre `N91-Leaf1`:

(N91-Leaf1) Priprava:
```
!
boot nxos bootflash:/nxos.9.3.10.bin sup-1       ! Aby boot nespadol do boot-loader-u
!
hostname N91-Leaf1
!
nv overlay evpn                 ! Aktivujeme podporu EVPN pre VXLAN
feature ospf
feature bfd
feature bgp
feature interface-vlan
feature vn-segment-vlan-based   ! Aktivujeme podporu mapovania VLAN na VNI
feature lacp
feature vpc
feature nv overlay              ! Aktivujeme funkciu VXLAN
!
```

(N91-Leaf1) Konfiguracia Underlay routingu uplinkov voci Spine-layer:
```
!
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
!
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

---
#### VXLAN Underlay konfiguracia pre `N92-Leaf2`:

(N92-Leaf2) Priprava:
```
!
boot nxos bootflash:/nxos.9.3.10.bin sup-1      ! Aby boot nespadol do boot-loader-u
!
hostname N92-Leaf2
!
nv overlay evpn                 ! Aktivujeme podporu EVPN pre VXLAN
feature ospf
feature bfd
feature bgp
feature interface-vlan
feature vn-segment-vlan-based   ! Aktivujeme podporu mapovania VLAN na VNI
feature lacp
feature vpc
feature nv overlay              ! Aktivujeme funkciu VXLAN
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
!
```

(N92-Leaf2) Konfiguracia Pod vPC:
```
!
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

---
#### VXLAN Underlay konfiguracia pre `N93-Leaf3`:

(N93-Leaf3) Priprava:
```
!
boot nxos bootflash:/nxos.9.3.10.bin sup-1      ! Aby boot nespadol do boot-loader-u
!
hostname N93-Leaf3
!
nv overlay evpn                 ! Aktivujeme podporu EVPN pre VXLAN
feature ospf
feature bfd
feature bgp
feature interface-vlan
feature vn-segment-vlan-based   ! Aktivujeme podporu mapovania VLAN na VNI
feature lacp
feature vpc
feature nv overlay              ! Aktivujeme funkciu VXLAN
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
!
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

---
#### VXLAN Underlay konfiguracia pre `N94-Leaf4`:

(N94-Leaf4) Priprava:
```
!
boot nxos bootflash:/nxos.9.3.10.bin sup-1      ! Aby boot nespadol do boot-loader-u
!
hostname N94-Leaf4
!
nv overlay evpn                 ! Aktivujeme podporu EVPN pre VXLAN
feature ospf
feature bfd
feature bgp
feature interface-vlan
feature vn-segment-vlan-based   ! Aktivujeme podporu mapovania VLAN na VNI
feature lacp
feature vpc
feature nv overlay              ! Aktivujeme funkciu VXLAN
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
!
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

---
#### VXLAN Underlay konfiguracia pre `N95-Spine1`:

(N95-Spine1) Priprava:
```
!
boot nxos bootflash:/nxos.9.3.10.bin sup-1      ! Aby boot nespadol do boot-loader-u
!
hostname N95-Spine1
!
nv overlay evpn                 ! Aktivujeme podporu EVPN pre VXLAN
feature ospf
feature bfd
feature bgp
feature nv overlay              ! Aktivujeme funkciu VXLAN
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

---
#### VXLAN Underlay konfiguracia pre `N96-Spine2`:

(N96-Spine2) Priprava:
```
!
boot nxos bootflash:/nxos.9.3.10.bin sup-1      ! Aby boot nespadol do boot-loader-u
!
hostname N96-Spine2
!
nv overlay evpn                 ! Aktivujeme podporu EVPN pre VXLAN
feature ospf
feature bfd
feature bgp
feature nv overlay              ! Aktivujeme funkciu VXLAN
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

(N91-Leaf1) Konfiguracia `Loopback1023` rozhrania:
```
!
interface loopback1023
  description main-RouterID-VTEP
  ip address 192.0.2.91/32              ! Sluzi aj ako ID/IP pre VTEP 
  ip address 192.0.2.12/32 secondary    ! Sluzi ako ID/IP pre spolocny vPC+VTEP
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
!
```

(N91-Leaf1) Konfiguracia VXLAN EVPN Overlay routingu s BGP:
```
!
router bgp 65001
  router-id 192.0.2.91
  log-neighbor-changes
  address-family ipv4 unicast
    maximum-paths 4             ! Nie je nutne, ale priprava na skalovanie
    maximum-paths ibgp 4        ! Nie je nutne, ale priprava na skalovanie
  address-family l2vpn evpn
    maximum-paths 4             ! Nie je nutne, ale priprava na skalovanie
    maximum-paths ibgp 4        ! Nie je nutne, ale priprava na skalovanie
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

---
#### VXLAN Overlay konfiguracia pre `N92-Leaf2`:

(N92-Leaf2) Konfiguracia `Loopback1023` rozhrania:
```
!
interface loopback1023
  description main-RouterID-VTEP
  ip address 192.0.2.92/32              ! Sluzi aj ako ID/IP pre VTEP
  ip address 192.0.2.12/32 secondary    ! Sluzi ako ID/IP pre spolocny vPC+VTEP
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
!
```

(N92-Leaf2) Konfiguracia VXLAN EVPN Overlay routingu s BGP:
```
!
router bgp 65001
  router-id 192.0.2.92
  log-neighbor-changes
  address-family ipv4 unicast
    maximum-paths 4             ! Nie je nutne, ale priprava na skalovanie
    maximum-paths ibgp 4        ! Nie je nutne, ale priprava na skalovanie
  address-family l2vpn evpn
    maximum-paths 4             ! Nie je nutne, ale priprava na skalovanie
    maximum-paths ibgp 4        ! Nie je nutne, ale priprava na skalovanie
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

---
#### VXLAN Overlay konfiguracia pre `N93-Leaf3`:

(N93-Leaf3) Konfiguracia `Loopback1023` rozhrania:
```
!
interface loopback1023
  description main-RouterID-VTEP
  ip address 192.0.2.93/32              ! Sluzi aj ako ID/IP pre VTEP
  ip address 192.0.2.34/32 secondary    ! Sluzi ako ID/IP pre spolocny vPC+VTEP
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
!
```

(N93-Leaf3) Konfiguracia VXLAN EVPN Overlay routingu s BGP:
```
!
router bgp 65001
  router-id 192.0.2.93
  log-neighbor-changes
  address-family ipv4 unicast
    maximum-paths 4             ! Nie je nutne, ale priprava na skalovanie
    maximum-paths ibgp 4        ! Nie je nutne, ale priprava na skalovanie
  address-family l2vpn evpn
    maximum-paths 4             ! Nie je nutne, ale priprava na skalovanie
    maximum-paths ibgp 4        ! Nie je nutne, ale priprava na skalovanie
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

---
#### VXLAN Overlay konfiguracia pre `N94-Leaf4`:

(N94-Leaf4) Konfiguracia `Loopback1023` rozhrania:
```
!
interface loopback1023
  description main-RouterID-VTEP
  ip address 192.0.2.94/32              ! Sluzi aj ako ID/IP pre VTEP
  ip address 192.0.2.34/32 secondary    ! Sluzi ako ID/IP pre spolocny vPC+VTEP
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
    maximum-paths ibgp 4        ! Nie je nutne, ale priprava na skalovanie
  address-family l2vpn evpn
    maximum-paths 4             ! Nie je nutne, ale priprava na skalovanie
    maximum-paths ibgp 4        ! Nie je nutne, ale priprava na skalovanie
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

---
#### VXLAN Overlay konfiguracia pre `N95-Spine1`:

(N95-Spine1) Konfiguracia `Loopback1023` rozhrania:
```
!
interface loopback1023
  description main-RouterID-VTEP
  ip address 192.0.2.95/32              ! Sluzi len na OSPF/BGP routing, nie VTEP
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
    maximum-paths ibgp 4        ! Nie je nutne, ale priprava na skalovanie
  address-family l2vpn evpn
    maximum-paths 4             ! Nie je nutne, ale priprava na skalovanie
    maximum-paths ibgp 4        ! Nie je nutne, ale priprava na skalovanie
    retain route-target all     ! Potrebne, aby nebolo nutne konfigurovat VNIs/VRFs na SpineX
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
#### VXLAN Overlay konfiguracia pre `N96-Spine2`:

(N96-Spine2) Konfiguracia `Loopback1023` rozhrania:
```
!
interface loopback1023
  description main-RouterID-VTEP
  ip address 192.0.2.96/32              ! Sluzi len na OSPF/BGP routing, nie VTEP
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
    maximum-paths ibgp 4        ! Nie je nutne, ale priprava na skalovanie
  address-family l2vpn evpn
    maximum-paths 4             ! Nie je nutne, ale priprava na skalovanie
    maximum-paths ibgp 4        ! Nie je nutne, ale priprava na skalovanie
    retain route-target all     ! Potrebne, aby nebolo nutne konfigurovat VNIs/VRFs na SpineX
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

(N91-Leaf1) Konfiguracia VLAN SVI rozhrani (Dynamic Anycast Gateway):
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
  fabric forwarding mode anycast-gateway    ! Zapina Dynamic Anycast Gateway
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

(N91-Leaf1) Konfiguracia VTEP rozhrania `nve1` a EVPN databazy:
```
!
interface nve1
  no shutdown
  description N91-VTEP-nve1
  host-reachability protocol bgp        ! Na distribuciu VXLAN EVPN informacii sa pouzije BGP
  source-interface loopback1023         ! Pouziju sa IPv4 adresy na routing a tunelovanie + vPC
!
  member vni 3100 associate-vrf
  member vni 3100101
    ingress-replication protocol bgp    ! Na distribuciu VXLAN EVPN informacii sa pouzije BGP
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
*Na tomto zariadeni je (okrem "interface description") konfiguracia zhodna s N91-Leaf1*
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
*Na tomto zariadeni je (okrem "interface description") konfiguracia zhodna s N91-Leaf1*
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
*Na tomto zariadeni je (okrem "interface description") konfiguracia zhodna s N91-Leaf1*
```

---
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

---
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
!
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

(N91-Leaf1) Konfiguracia VLAN SVI rozhrani (Dynamic Anycast Gateway):
```
!
! *Doplnujeme konfiguraciu o dalsieho zakaznika "TenantB"*
!
interface Vlan201
  description TenantB-seg201-any-gw
  no shutdown
  vrf member TenantB
  no ip redirects
  ip address 192.168.1.254/24
  no ipv6 redirects
  fabric forwarding mode anycast-gateway    ! Zapina Dynamic Anycast Gateway
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

(N91-Leaf1) Konfiguracia VTEP rozhrania `nve1` a EVPN databazy:
```
!
! *Doplnujeme konfiguraciu o dalsieho zakaznika "TenantB"*
!
interface nve1
  member vni 3200 associate-vrf
  member vni 3200201
    ingress-replication protocol bgp    ! Na distribuciu VXLAN EVPN informacii sa pouzije BGP
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
*Na tomto zariadeni je (okrem "interface description") konfiguracia zhodna s N91-Leaf1*
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
*Na tomto zariadeni je (okrem "interface description") konfiguracia zhodna s N91-Leaf1*
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
*Na tomto zariadeni je (okrem "interface description") konfiguracia zhodna s N91-Leaf1*
```

---
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

---
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
  - funckne pri testovani na N9300v + IOSv
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

---
#### 2. Unicast L3 routing medzi L2 segmentami v ramci zakaznikovej VRF:
```
  - zakaznik vyuziva zariadenia "Tenant-A-SW1", "Tenant-A-SW2", "Tenant-A-SW3" a "Tenant-A-SW4"
  - funckne pri testovani na N9300v + IOSv
```

(N91-Leaf1 + N92-Leaf2 + N93-Leaf3 + N94-Leaf4) Konfiguracia VXLAN routingu:
```
** Konfiguracia routingu v ramci VXLAN VRF bola vykonana v casti pripravy VXLAN sluzieb **
```

(N91-Leaf1 + N92-Leaf2) Konfiguracia vPC voci zakaznikovi:
```
!
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
!
! *Doplnime konfiguraciu pre koncove zariadenie "Tenant-A-SW4"*
!
interface port-channel14
  description downlink-to-Tenant-A-SW4-vPC14
  switchport access vlan 103
  spanning-tree port type edge
  spanning-tree bpduguard enable
  mtu 9216
  storm-control broadcast level 1.00
  storm-control action trap
  vpc 14
!
interface Ethernet1/14
  description downlink-to-Tenant-A-SW4-vPC14
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
```

(N91-Leaf1 + N92-Leaf2 + N93-Leaf3 + N94-Leaf4) Konfiguracia routingu u zakaznika:
```
!
! Routing pre zariadenie Tenant-A-SW1:
!
ip route 0.0.0.0 0.0.0.0 192.168.1.254 name default-route-to-AS65001
end
!
! Routing pre zariadenie Tenant-A-SW2:
!
ip route 0.0.0.0 0.0.0.0 192.168.2.254 name default-route-to-AS65001
end
!
! Routing pre zariadenie Tenant-A-SW3:
!
ip route 0.0.0.0 0.0.0.0 192.168.1.254 name default-route-to-AS65001
end
!
! Routing pre zariadenie Tenant-A-SW4:
!
ip route 0.0.0.0 0.0.0.0 192.168.3.254 name default-route-to-AS65001
end
!
```

---
#### 3. Transparentne prepojenie P-to-P cez VXLAN-Xconnect:
```
   - zakaznik vyuziva zariadania "Tenant-B-SW1" a "Tenant-B-SW3"
   - NEfunkcne na N9300V, VM toto nepodporuje v kombinacii s vPC na NX-OSv ver. 9.3(10)

   - chybaju system. prikazy: "system dot1q-tunnel transit [vlan vlan-range]"
                     spolu s: "system nve infra-vlans <backup-svi-vlan>"

   - pozriet obmedzenia ohladom HW/SW a vPC (NX-OS 9.X vs. NX-OS 10.X)
```

(N91-Leaf1) Experimentalna konfiguracia sluzby VXLAN-XConnect voci zakaznikovy:
```
!
! system dot1q-tunnel transit 10        ! Neoverena konfiguracia, nie je na N9300v
! system nve infra-vlans 912            ! Neoverena konfiguracia, nie je na N9300v
!
vlan 10
  name vxlan-Xconn-test
  vn-segment 1010
  xconnect
!
interface nve1
  member vni 1010
    ingress-replication protocol bgp
!
evpn
  vni 1010 l2
    rd auto
    route-target import auto
    route-target export auto
!
interface Ethernet1/21
  description downlink-to-Tenant-B-SW1
  switchport mode dot1q-tunnel
  switchport access vlan 10
  spanning-tree port type edge
  spanning-tree bpdufilter enable
  mtu 9216
  storm-control action trap
!
```

(N93-Leaf3) Experimentalna konfiguracia sluzby VXLAN-XConnect voci zakaznikovi:
```
!
! system dot1q-tunnel transit 10        ! Neoverena konfiguracia
! system nve infra-vlans 934            ! Neoverena konfiguracia
!
vlan 10
  name vxlan-Xconn-test
  vn-segment 1010
  xconnect
!
interface nve1
  member vni 1010
    ingress-replication protocol bgp
!
evpn
  vni 1010 l2
    rd auto
    route-target import auto
    route-target export auto
!
interface Ethernet1/23
  description downlink-to-Tenant-B-SW3
  switchport mode dot1q-tunnel
  switchport access vlan 10
  spanning-tree port type edge
  spanning-tree bpdufilter enable
  mtu 9216
  storm-control action trap
!
```

(Tenant-B-SW1) Experimentalna konfiguracia sluzby VXLAN-XConnect voci pokytovatelovi:

```
!
vlan 201
 name native-uplink-to-AS65001
!
vlan 301
 name VXLAN-Xconn-test-1
!
vlan 302
 name VXLAN-Xconn-test-2
!
vlan 303
 name VXLAN-Xconn-test-3
!
interface GigabitEthernet3/1
  description QinQ-uplink-to-AS65001
  switchport trunk encapsulation dot1q
  switchport trunk native vlan 201
  switchport mode trunk
  mtu 9216
  negotiation auto
  spanning-tree portfast edge trunk
  spanning-tree bpdufilter enable
!
interface Vlan301
  description VXLAN-Xconn-test-1
  ip address 172.16.1.1 255.255.255.0
  no ip redirects
  no ip unreachables
  no ip proxy-arp
  no shutdown
!
interface Vlan302
  description VXLAN-Xconn-test-2
  ip address 172.16.2.1 255.255.255.0
  no ip redirects
  no ip unreachables
  no ip proxy-arp
  no shutdown
!
interface Vlan303
  description VXLAN-Xconn-test-3
  ip address 172.16.3.1 255.255.255.0
  no ip redirects
  no ip unreachables
  no ip proxy-arp
  no shutdown
!
```

(Tenant-B-SW3) Experimentalna konfiguracia sluzby VXLAN-XConnect voci pokytovatelovi:
```
!
vlan 201
 name native-uplink-to-AS65001
!
vlan 301
 name VXLAN-Xconn-test-1
!
vlan 302
 name VXLAN-Xconn-test-2
!
vlan 303
 name VXLAN-Xconn-test-3
!
interface GigabitEthernet3/1
  description QinQ-uplink-to-AS65001
  switchport trunk encapsulation dot1q
  switchport trunk native vlan 201
  switchport mode trunk
  mtu 9216
  negotiation auto
  spanning-tree portfast edge trunk
  spanning-tree bpdufilter enable
!
interface Vlan301
  description VXLAN-Xconn-test-1
  ip address 172.16.1.3 255.255.255.0
  no ip redirects
  no ip unreachables
  no ip proxy-arp
  no shutdown
!
interface Vlan302
  description VXLAN-Xconn-test-2
  ip address 172.16.2.3 255.255.255.0
  no ip redirects
  no ip unreachables
  no ip proxy-arp
  no shutdown
!
interface Vlan303
  description VXLAN-Xconn-test-3
  ip address 172.16.3.3 255.255.255.0
  no ip redirects
  no ip unreachables
  no ip proxy-arp
  no shutdown
!
```

---
#### 4. Transparentne QinVNI prepojenie medzi 2 bodmi cez VXLAN dot1q tunnel:
```
   - zakaznik vyuziva zariadania "Tenant-B-SW2" a "Tenant-B-SW4"
   - NEfunkcne na N9300V, VM toto tunelovanie nepodporuje v kombinacii s vPC na NX-OSv ver. 9.3(10)
     - malo by podporovat na REAL zeleze aj point-to-Multipoint, treba preskumat
   - pozriet obmedzenia ohladom HW/SW a vPC (NX-OS 9.X vs. NX-OS 10.X)

   - OTESTOVAT variantu, kedy je provider port na inom zariadeni ako VTEP a treba QinVNI transport
   - otazka, aka je podpora QinVNI s pripojenim zakaznika do FEX-ov
   - otazka, da sa nasadit BW rate-limit / shaping na "provider" porte, zatial neviem
```

(N91-Leaf1 + N92-Leaf2) Experimentalna konfiguracia sluzby VXLAN dot1q tunnel voci zakaznikovi:
```
!
! system dot1q-tunnel transit 201       ! Neoverena konfiguracia
! system nve infra-vlans 912            ! Neoverena konfiguracia
!
vlan 201
  name TenantB-seg201-L2
  vn-segment 3200201
!
interface nve1
  member vni 3200201
    ingress-replication protocol bgp
!
evpn
  vni 3200201 l2
    rd auto
    route-target import auto
    route-target export auto
!
interface port-channel22
  switchport mode dot1q-tunnel
  switchport access vlan 201
  spanning-tree port type edge
  spanning-tree bpdufilter enable
  mtu 9216
  storm-control action trap
  vpc 22
!
interface Ethernet1/22
  switchport mode dot1q-tunnel
  switchport access vlan 201
  spanning-tree port type edge
  spanning-tree bpdufilter enable
  mtu 9216
  storm-control action trap
  channel-group 22 mode active
!
```

(N93-Leaf3 + N94-Leaf4) Experimentalna konfiguracia sluzby VXLAN dot1q tunnel voci zakaznikovi:
```
!
! system dot1q-tunnel transit 201       ! Neoverena konfiguracia
! system nve infra-vlans 934            ! Neoverena konfiguracia
!
vlan 201
  name TenantB-seg201-L2
  vn-segment 3200201
!
interface nve1
  member vni 3200201
    ingress-replication protocol bgp
!
evpn
  vni 3200201 l2
    rd auto
    route-target import auto
    route-target export auto
!
interface port-channel24
  switchport mode dot1q-tunnel
  switchport access vlan 201
  spanning-tree port type edge
  spanning-tree bpdufilter enable
  mtu 9216
  storm-control action trap
  vpc 24
!
interface Ethernet1/24
  switchport mode dot1q-tunnel
  switchport access vlan 201
  spanning-tree port type edge
  spanning-tree bpdufilter enable
  mtu 9216
  storm-control action trap
  channel-group 24 mode active
!
```

---
#### 5. Externa konektivita do Inetu z VRF `TenantA` / `TenantB`:
```
   - zakaznik "TenantA" vyuziva zariadenia "Tenant-A-SW1", "Tenant-A-SW2", "Tenant-A-SW3" a "Tenant-A-SW4"
   - zakaznik "TenantB" vyuziva zariadanie "Tenant-B-SW4"

   - viacero moznosti na hand-off L3 konektivity do Internetu, typicky s VRF-lite + BGP
   - funkcne na N9300V v kombinacii CSR1000v s IOS-XE, aj multipoint / multi-tenant

   - do BGP sa da propagovat v ramci VRF default routa s "network 0.0.0.0/0"
   - na Inet-R1 potrebny VRF aware NAT config
   - problem ked je roztiahnuta VXLAN, a border-gw do INetu je len na 1 Pod-e
     - bolo potrebne vypnut "Dynamic Anycast GW" na L3 uplinkovych Vlan104 + Vlan204 pre Tenant-A/B
     - `Leaf1+2` (NEmaju pripojenu Inet GW) verzus `Leaf3+4` (maju pripojenu Inet GW)

   - zariadenia A:SW4 a B:SW4 maju rovnake IP ale s VRF+NAT sa dostanu v poriadku na Inet
   - priama adresacia zakaznikov vo VRF je mozna ale, problematicka, idealne pouzit unikatne IP adresy
```

(Inet-R1) Konfiguracia externeho routera pre pristup na Internet:
```
!
! Poznamky: - VRF Instancie zakanzikov "TenantA" / "TenantB" boli doplnene o
!             uplinky do Ineternetu cez L3-SVI s oznacenim Vlan104 / Vlan204
!
!           - Kvoli jednoduchosti je na VRF-Lite Inet routing pouzity protokol OSPFv2
!
!           - Podla diagramu je kvoli prehladnosti "Inet-R1" fyz. pripojeny len
!             na "N93-Leaf3", ale "Inet-R1" ma Bcast OSPF neighbor-ship aj s "N94-Leaf4"
!             - v praxi by bol standardne zapojeny ako dual-homed cez Point-to-Point
!
interface Loopback1023
 description loop-for-Router-ID
 ip address 192.0.2.100 255.255.255.0
!
vrf definition TenantA
 rd 192.0.2.100:3
 !
 address-family ipv4
 exit-address-family
!
vrf definition TenantB
 rd 192.0.2.100:4
 !
 address-family ipv4
 exit-address-family
!
router ospf 65001 vrf TenantA
 router-id 192.0.2.100
 auto-cost reference-bandwidth 4000000
 default-information originate always metric 1000 metric-type 1
!
router ospf 65002 vrf TenantB
 router-id 192.0.2.100
 auto-cost reference-bandwidth 4000000
 default-information originate always metric 1000 metric-type 1
!
interface GigabitEthernet4
 description uplink-to-internet-AS-XYZ
 no cdp enable
 ip address 192.168.5.100 255.255.255.0
 no ip redirects
 no ip unreachables
 no ip proxy-arp
 ip nat outside
 negotiation auto
 no mop enabled
 no mop sysid
!
interface GigabitEthernet1
 description downlink-to-VXLAN-fabric-AS65001
 no ip address
 no ip redirects
 negotiation auto
 no ipv6 redirects
 no mop enabled
 no mop sysid
!
interface GigabitEthernet1.104
 description TenantA-seg104-inet-gw
 encapsulation dot1Q 104
 vrf forwarding TenantA
 ip address 192.168.14.100 255.255.255.0
 no ip redirects
 ip nat inside
 ip ospf network broadcast
 ip ospf 65001 area 0
 no ipv6 redirects
!
interface GigabitEthernet1.204
 description TenantB-seg204-inet-gw
 encapsulation dot1Q 204
 vrf forwarding TenantB
 ip address 192.168.24.100 255.255.255.0
 no ip redirects
 ip nat inside
 ip ospf network broadcast
 ip ospf 65002 area 0
 no ipv6 redirects
!
ip route 0.0.0.0 0.0.0.0 192.168.5.1 name default-internet-access
ip route vrf TenantA 0.0.0.0 0.0.0.0 GigabitEthernet4 192.168.5.1 global name inet-access-for-TenantA
ip route vrf TenantB 0.0.0.0 0.0.0.0 GigabitEthernet4 192.168.5.1 global name inet-access-for-TenantB
!
ip access-list standard acl-nat-TenantA
 10 permit 192.168.14.0 0.0.0.255
 20 permit 192.168.1.0 0.0.0.255
 30 permit 192.168.2.0 0.0.0.255
 40 permit 192.168.3.0 0.0.0.255
!
ip access-list standard acl-nat-TenantB
 10 permit 192.168.24.0 0.0.0.255
 20 permit 192.168.1.0 0.0.0.255
 30 permit 192.168.2.0 0.0.0.255
 40 permit 192.168.3.0 0.0.0.255
!
ip nat inside source list acl-nat-TenantA interface GigabitEthernet4 vrf TenantA overload
ip nat inside source list acl-nat-TenantB interface GigabitEthernet4 vrf TenantB overload
!
```

(N91-Leaf1) Konfiguracia pristupu na Internet voci zakaznikovi:
```
!
vlan 104
  name TenantA-seg104-L2
  vn-segment 3100104
!
vlan 204
  name TenantB-seg204-L2
  vn-segment 3200204
!
interface nve1
  member vni 3100104
    ingress-replication protocol bgp
!
  member vni 3200204
    ingress-replication protocol bgp
!
evpn
  vni 3100104 l2
    rd auto
    route-target import auto
    route-target export auto
!
  vni 3200204 l2
    rd auto
    route-target import auto
    route-target export auto
!
```

(N92-Leaf2) Konfiguracia pristupu na Internet voci zakaznikovi:
```
* Konfiguracia je zhodna so zariadenim "N91-Leaf1" *
```

(N93-Leaf3) Konfiguracia pristupu na Internet voci zakaznikovi:
```
!
vlan 104
  name TenantA-seg104-L2
  vn-segment 3100104
!
vlan 204
  name TenantB-seg204-L2
  vn-segment 3200204
!
interface nve1
  member vni 3100104
    ingress-replication protocol bgp
!
  member vni 3200204
    ingress-replication protocol bgp
!
evpn
  vni 3100104 l2
    rd auto
    route-target import auto
    route-target export auto
!
  vni 3200204 l2
    rd auto
    route-target import auto
    route-target export auto
!
interface Ethernet1/1
  description uplink-to-Inet-R1-GW
  switchport mode trunk
  switchport trunk allowed vlan 104,204
  spanning-tree port type edge trunk
  spanning-tree bpduguard enable
!
interface Vlan104
  description TenantA-seg104-inet-uplink
  no shutdown
  vrf member TenantA
  no ip redirects
  ip address 192.168.14.253/24
  no ipv6 redirects
  ip ospf network broadcast
  ip router ospf as65001 area 0.0.0.0
!
interface Vlan204
  description TenantB-seg204-inet-uplink
  no shutdown
  vrf member TenantB
  no ip redirects
  ip address 192.168.24.253/24
  no ipv6 redirects
  ip ospf network broadcast
  ip router ospf as65001 area 0.0.0.0
!
router ospf as65001
  vrf TenantA
    router-id 192.0.2.93
    log-adjacency-changes
    auto-cost reference-bandwidth 4000 Gbps
  vrf TenantB
    router-id 192.0.2.93
    log-adjacency-changes
    auto-cost reference-bandwidth 4000 Gbps
!
router bgp 65001
  vrf TenantA
    address-family ipv4 unicast
      network 0.0.0.0/0
  vrf TenantB
    address-family ipv4 unicast
      network 0.0.0.0/0
!
```

(N94-Leaf4) Konfiguracia pristupu na Internet voci zakaznikovi:
```
!
vlan 104
  name TenantA-seg104-L2
  vn-segment 3100104
!
vlan 204
  name TenantB-seg204-L2
  vn-segment 3200204
!
interface nve1
  member vni 3100104
    ingress-replication protocol bgp
!
  member vni 3200204
    ingress-replication protocol bgp
!
evpn
  vni 3100104 l2
    rd auto
    route-target import auto
    route-target export auto
!
  vni 3200204 l2
    rd auto
    route-target import auto
    route-target export auto
!
interface Ethernet1/1
  description uplink-to-Inet-R1-GW
  switchport mode trunk
  switchport trunk allowed vlan 104,204
  spanning-tree port type edge trunk
  spanning-tree bpduguard enable
!
interface Vlan104
  description TenantA-seg104-inet-uplink
  no shutdown
  vrf member TenantA
  no ip redirects
  ip address 192.168.14.254/24
  no ipv6 redirects
  ip ospf network broadcast
  ip router ospf as65001 area 0.0.0.0
!
interface Vlan204
  description TenantB-seg204-inet-uplink
  no shutdown
  vrf member TenantB
  no ip redirects
  ip address 192.168.24.254/24
  no ipv6 redirects
  ip ospf network broadcast
  ip router ospf as65001 area 0.0.0.0
!
router ospf as65001
  vrf TenantA
    router-id 192.0.2.94
    log-adjacency-changes
    auto-cost reference-bandwidth 4000 Gbps
  vrf TenantB
    router-id 192.0.2.94
    log-adjacency-changes
    auto-cost reference-bandwidth 4000 Gbps
!
router bgp 65001
  vrf TenantA
    address-family ipv4 unicast
      network 0.0.0.0/0
  vrf TenantB
    address-family ipv4 unicast
      network 0.0.0.0/0
!
```

(Tenant-A-SW4 + Tenant-B-SW4) Konfiguracia pristupu na Internet voci poskytovatelovi:
```
!
! Poznamky: - Dve koncove zariadenie maju rovnaku IP adresu, ale patria roznym zakaznikom
!           - Vdaka konceptu NAT + VRF a BGP EVPN je mozna konektivita pre oboch zakaznikov
!           - V realnom DC/SP nasadeni sa vsak pocita s dosiahnutelnostou "z vonku", preto
!             su potrebne v4/v6 verejne adresy.
!
! Tenant-A-SW4 routing config:
!
interface Vlan103
 description uplink-to-provider-AS65001
 ip address 192.168.3.4 255.255.255.0
 no ip redirects
 no ip unreachables
 no ip proxy-arp
!
ip route 0.0.0.0 0.0.0.0 192.168.3.254 name default-to-provider-AS65001
!
! Tenant-B-SW4 routing config:
!
interface Vlan203
 description uplink-to-provider-AS65001
 ip address 192.168.3.4 255.255.255.0
 no ip redirects
 no ip unreachables
 no ip proxy-arp
!
ip route 0.0.0.0 0.0.0.0 192.168.3.254 name default-to-provider-AS65001
!
! Tenant-X-SW-Y routing config:
!
! Konfiguracia dalsich zariadeni zakaznikov je analogicka s "Tenant-A-SW4" / "Tenant-B-SW4"
!
```

---
### Testovanie "Failure" scenarov:
```
UPOZORNENIE: Tato kapitola je celkovo problematicka v prostredi GNS3/N9300v, preto
             sluzi len ako zdroj poznamok pre dalsie skumanie a testovanie.

VAROVANIE: Prikazy v tejto sekcii su STRIKTNE urcene na LABORATORNE TESTOVANIE!
```
Testovanie konvergencie VXLAN fabricu, pri vypadku urciteho uzla, je ciastocne
obmedzene na virtualnej platforme Nexus9300v. Dalsou vaznou komplikaciou je problem,
ktory uvadzam na zaciatku. Nahodne nabootuje VM instancia N9300v bez pritomnosti VTEP
rozhrania, napr. nasledovny vypis:
````
N92-Leaf2# show interface nve1
                          ^
Invalid interface format at '^' marker.
````
Dalsim prikladom je absencia technologie `BFD`, ktora umoznuje rychlejsiu konvergenciu
smerovacich protokolov. Daju sa ale simulovat ine "katastroficke" scenare. Prikladom
je umyselne "vypnutie" `vPC domeny` na jednom z dvoch clenov tejto domeny. Tymto 
krokom dochadza k *zavaznej inkonzistencii* vo vPC domene. Tym, ze testovany VXLAN fabric
pracuje aj s konceptom vPC, pri deaktivacii vPC domeny na jednom z clenov domeny, je
nutne tohto clena "izolovat", aby sa zabranilo nekontrolovanemu sireniu BUM trafficu.

Poznamka: Plne si uvedomujem, ze taketo synteticke testovanie nema vela
spolocneho s workload-mi z realneho sveta. Mam vsak obmedzene moznosti testovania,
preto testujem aspon zakladnu POC konvergenciu na urovni Control-Plane, na jednotlivych
DUT zariadeniach.

---
#### 1. Scenar, zlyhanie VXLAN/VTEP/Leaf zariadeni vo VXLAN fabricu:

Tento scenar je konkretne zamerany na test vypadku VTEP zariadenia tym, ze
deaktivujeme vPC domenu na zariadeni v role `vPC Primary Peer`. Zaroven vypneme
smerovacie protokoly `OSPF` a `BGP`. Pocas aktivnej TCP/SSH a ICMP/ping komunikacie
medzi zariadeniami `Tenant-A-SW1` a `Tenant-A-SW4` bude deaktivovana `vPC` domena,
`OSPF` a `BGP` na zariadeniach `N91-Leaf1` a `N93-Leaf3`. Vid. diagram topologie.

Pred vypadkom VTEP zariadeni bude zo zariadenia `Tenant-A-SW1` vytvorene SSH
pripojenie typu TCP/client/server na zariadenie `Tenant-A-SW4`. Z SSH/CLI rozhrania bude
spusteny prikaz `TenantA-SW4#ping 192.168.1.1 size 1500 repeat 1000 df-bit`.
Teda zjednodusene, rapidny TCP/echo/reply flow z `Tenant-A-SW4` na `Tenant-A-SW1`.
Na DUT zariadeniach budeme sledovat diagnosticke vypisy.

Simulovane zlyhanie zariadenia `N91-Leaf1`:
```
!
! VAROVANIE: Prikazy v tejto sekcii su STRIKTNE urcene pre LABORATORNE TESTOVANIE
!
N91-Leaf1# configure
N91-Leaf1(config)# vpc domain 912
N91-Leaf1(config-vpc-domain)# shutdown
N91-Leaf1(config-vpc-domain)# router ospf as65001
N91-Leaf1(config-router)# shutdown
N91-Leaf1(config-router)# router bgp 65001
N91-Leaf1(config-router)# shutdown
N91-Leaf1(config-router)# end
```

Diagnostika vypadku:

Diagnosticke vypisy zo zariadenia `Tenant-A-SW4`:
```
TenantA-SW4#ping 192.168.1.1 size 1500 repeat 1000 df-bit
Type escape sequence to abort.
Sending 1000, 1500-byte ICMP Echos to 192.168.1.1, timeout is 2 seconds:
Packet sent with the DF bit set
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!.!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!.......!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!!!!!!!!!!!!!!!!!!!
Success rate is 99 percent (992/1000), round-trip min/avg/max = 22/53/341 ms
```
- prvy vypadok ICMP odpovede (bodka vo vypise) v momente vypnutia vPC domenay na `N91-Leaf1`
- nasledne prerusenie SSH pripojenia pred sekveciou 7 stratenych ICMP odpovedi
- po konvergencii protokolov a vPC bola komunikacia SSH a ICMP paketov obnovena
  BEZ AKEJKOLVEK rucnej intervencie
- UX/UI "zazitok" na zariadeni `Tenant-A-SW4` bola strata a obnovenie CLI reakcie
  a vypisy prikazu `ping` na cca 5 sekund
- podla ocakavania, klientske zariadenie `TenantA-SW1` hlasilo suspendovanie jedneho
uplinku na `N91-Leaf1`, komunikacia pokracovala cez uplink na `N92-Leaf2`:

```
*Aug 23 07:30:22.328: %LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet3/1, changed state to down
*Aug 23 07:30:29.722: %EC-5-L3DONTBNDL2: Gi3/1 suspended: LACP currently not enabled on the remote port.
```

- PROBLEM: komunikacia sa po cca 20 min. (ARP Timeout na zaloznom VTEPe) uplne zastavila
  - sekundarne VTEPy, ktore prebrali rolu `vPC Primary` sa nedokazu "ucit/obnovovat" ARP
    zaznami klientskych zariadeni a teda nie je mozna komunikacia ani len na Default-GW

- konfiguracia VTEP zariadeni bola rozsirena o tuto sekvenciu prikazov:
```
!
vpc domain 912              ! Resp. "vpc domain 934"
  auto-recovery
  fast-convergence
!
router bgp 65001
  address-family l2vpn evpn
    advertise-pip
!
interface nve 1
  advertise virtual-rmac
!
```
ale ani tento doplnok nevyriesil problem s nefunkcnym ARP ucenim/obnovovanim.

Predovsetkym pri testovani chybala (N9300v to nema) aj dolezita uprava vPC konfiguracie,
ktora definuje tzv. zaloznu infrastructure VLAN, pre oboch clenov vPC domeny, napr. pre vPC domenu `912`:
```
N91-Leaf1(config)# sh run vlan 912

vlan 912
  name vPC-peer-link-L3-backup

N91-Leaf1(config)# sh run int vlan 912

interface Vlan912
  description vPC-peer-link-L3-backup
  no shutdown
  bfd ipv4 interval 500 min_rx 500 multiplier 3
  no ip redirects
  ip address 10.9.9.11/29
  no ipv6 redirects
  ip ospf network point-to-point
  ip router ospf as65001 area 0.0.0.0
  ip ospf bfd

N91-Leaf1(config)# system nve infra-vlans 912
                                ^
                                % Invalid command at '^' marker.
```

Moj skromny nazor: Takato situacia moze posluzit ako jedna z demonstracii faktu, ze
zdielanie a synchronizacia Control-plane udajov (CAM/ARP) pre Ethernet/IP Data-plane,
nie je trivialny problem. Konkretne riesenia typu MLAG a Stacking mozu sposobovat
viacero problemov. V kontexte VM instancii ma napada myslienka, ze virtualizacia
sietoveho L2 transportu pre DataCenter potreby Hypervizorov by mohla byt riesena
priamo v Hypervizore. Fyzicke HW servery by mohli byt pripojene L3-routed linkami a
modul Hypervizora moze sluzit ako switch/router/VTEP, vid. napr. projekty `Open vSwitch`,
`OpenStack Neutron`, `FRRouting`, `VMWare NSX (VXLAN)`, `MS Hyper-V (pouzili NVGRE namiesto VXLAN)`, ...

K dispozicii su uz dlhsiu dobu sietove karty, ktore disponuju funkciou VXLAN/VTEP offloading.
Taketo sietove karty poskytuju vyrobcovia ako su napr. Intel, Nvidia (Mellanox), Broadcom a dalsi. 

 - treba pozriet sekciu s Inet odkazmi, mohol by to byt predmet dalsieho studia 
---
#### 2. Scenar, zlyhanie VXLAN/Spine zariadenia vo VXLAN fabricu:

Tento scenar dokazuje uzitocnost technologie `BFD`. Samozrejme sa da namietat 
argumentom, ze je mozne menit casovace smerovacich protokolov, ako su napr. `OSPF` a `BGP`. 
Tento pristup ma vsak svoje nevyhody. Pri tychto sietovych "trikoch" vznika dalsia
komplexnost, ktora zatazuje unifikovane postupy konfiguracie, monitoringu a dokumentacie.
Zvysuje sa aj komplexnost pohladu na sietovu infrastrukturu ako jeden celok, pretoze musime
pocitat s tym, ze ak *nezmenime* casovace v *celom* autonomnom systeme, je potrebne
vediet, v ktorych segmentoch siete mame non-default parametre protokolov. Vplyv
na troubleshooting procesy je teda evidentny.

Simulovane zlyhanie zariadenia `N95-Spine1`:
```
!
! VAROVANIE: Prikazy v tejto sekcii su STRIKTNE urcene pre LABORATORNE TESTOVANIE
!
N95-Spine1# configure
N95-Spine1(config)# router ospf as65001
N95-Spine1(config-router)# shutdown
N95-Spine1(config-router)# router bgp 65001
N95-Spine1(config-router)# shutdown
N95-Spine1(config-router)# end
```

Diagnostika vypadku:

Infrastruktura s VXLAN-EVPN fabricom konvergovala bez problemov na zaklade dizajnovych
doporuceni, podla filozofie L3-routed/ECMP backbone. Vypadok nezaznamenali ziadne testovane
workloady, teda v zmysle obmedzenych moznosti tohto testovania. Samozrejme, pri vypadku
VXLAN Spine zariadenia je vhodne sledovat zatazenie aktivnych liniek, aby nedoslo k neziaducim
efektom ako su napr.: "Traffic Tromboning", "Traffic Polarization", "Extreme Oversubscription".
Taketo extremne pripady mozu vznikat pri tzv. "East-West Elephant Flows", kedy sa napr.
synchronizuje velka Databaza, presuvaju sa zalohy alebo VM Images pre Hypervizory.

Este si treba uvedomit, podla diagramu a konfiguracie, ze medzi VXLAN Spine zariadeniami
neexistuje ziadne "prepojenie" na urovni Control-plane. Je to mozne v tomto jednoduchom
scenare, realne vsak mozeme pracovat s pristupmi, ako su "Collapsed-Spine",
"Single-box Leaf+Spine", "VXLAN Super-Spine", "Multi-site VXLAN Fabric" a pod.
Pri tychto dizajnoch je vyssia komplexnost systemu nevyhnutna a treba s nou pocitat
pri testovani vypadkov.

---
Poznamka: pri testovani scenarov boli pouzite prikazy na rozdelenie regionov TCAM pamate:
          
 - pozor, po zmenach TCAM regionov je nutny restart zariadenia
 - pre realny HW zo serie Nexus9300 nie su potrebne upravy TCAM regionov
 - v pripade N9300v nemali tieto upravy ziaden vplyv pri vypadkoch a konvergencii

```
!
hardware access-list tcam region racl 512
hardware access-list tcam region arp-ether 256 double-wide
!
```

---
### Pouzivane skratky v dokumente:
```
AF      - Address Family (BGP)
ARP     - Address Resolution Protocol
AS      - Autonomous System (BGP)
ASIC    - Application Specific Integrated Circuit (Switch Hardware)
BASH    - Bourne-Again SHell (Unix)
BD      - Bridge Domain (VXLAN)
BFD     - Bidirectional Forwarding Detection
BPDU    - Bridge Protocol Data Unit
BGPv4   - Border Gateway Protocol version 4
BUM     - Broadcast, Unknown unicast, Multicast
BW      - BandWidth
CSR1000 - Cloud Services Router series 1000 (Cisco)
DAG     - Dynamic Anycast Gateway (VXLAN)
ESI     - Ethernet Segment Identifier (VXLAN EVPN Multihoming, alternativa k vPC)
EVI     - EVPN Virtual Instance (VXLAN)             
EVPN    - Ethernet Virtual Private Network (VXLAN)
GNS3    - Graphical Network Simulator version 3
GW      - Gateway (Router)
HW      - Hardware
IGP     - Interior Gateway Protocol
IOS     - Internetwork Operating System (Cisco)
IRB     - Integrated Routing & Bridging (VRF / VXLAN)
KVM     - Kernenel-based Virtual Machine (Linux)
L2      - Layer 2
L3      - Layer 3
LACP    - Link Aggregation Control Protocol
MD5     - Message Digest level 5
MTU     - Maximum Transmission Unit
MLAG    - Multi-chasis Link Aggregation
N91     - Nexus 9300v c. 1 (vid. Diagram) 
NAT     - Network Address Translation
NV      - Network Virtualization (VXLAN)
NVE     - Network Virtualization Edge / Endpoint (VXLAN)
NX-OS   - Nexus-Operating System (Cisco)
OSPFv2  - Open Shortest Path First version 2
OSPFv3  - Open Shortest Path First version 3
POD/Pod - Point Of Delivery
QEMU    - Quick EMUlator (Generic and open source machine emulator and virtualizer)
QinQ    - 802.1Q tag tunneled in 802.1Q tag
QinVNI  - 802.1Q tag tunneled in VNI (VXLAN)
R1      - Router c. 1 (Diagram)
RD      - Route Distinguisher (VRF)
RT      - Route Target (VRF)
SVI     - Switch VLAN / Virtual Interface
SW      - Software
SW1     - Switch c. 1 (Diagram)
TCAM    - Ternary Content Addressable Memory (Switch Hardware)
VLAN    - Virtual Local Area Network
VM      - Virtual Machine
VNI     - Virtual Network Instance (VXLAN)
VNID    - Virtual Network IDentifier (VXLAN)
vPC     - Virtual Port-Channel (Cisco)
VPN     - Virtual Private Network
VRF     - VPN / Virtual Routing & Forwarding
VTEP    - Virtual Tunnel End Point (VXLAN)
VXLAN   - Virtual eXtensible LAN
```

---
### Pouzivane diagnosticke prikazy:
```
# show startup-config | include feature|overlay
# show feature | include enabled
# show boot
# show running-config ospf
# show running-config bgp
# show interface nve1
# show running-config interface nve1
# show ip ospf neighbors
# show ip ospf neighbors vrf TenantA
# show bgp l2vpn evpn summary
# show running-config vlan 101
# show running-config nv overlay
# show running-config | section evpn
# show running-config vrf TenantA
# show bgp l2vpn evpn vrf TenantA
# show mac address-table vlan 101
# show ip arp vlan 103 vrf TenantA
# show nve peers detail
# show nve vrf
# show nve vni
# show nve vni 3100101 detail
# show bgp vrf TenantA l2vpn evpn 192.168.1.1      ! Vo vypise je vidiet VNI Stack "Received label 3100101 3100"
# show l2route mac all
# show nve ethernet-segment
# show bgp l2vpn evpn route-type 4
# show bgp l2vpn evpn route-type 5
# show ip route vrf TenantA                        ! Vo vypise je vidiet VXLAN signalizaciu
# show ip route ospf-as65001 vrf TenantB
# show bgp vrf TenantB ipv4 unicast
# show vpc
# show vpc consistency-parameters ...
# show vpc [VPC_NUM]
# show vpc orphan-ports
```

---
### Pouzite zdroje informacii:

Poznamka: Treba ocakavat, ze uvedene odkazy mozu casom exspirovat.

```
Cisco Nexus 9000 Series NX-OS Fundamentals Configuration Guide, Release 9.3(x):

https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus9000/sw/93x/fundamentals/configuration/guide/b-cisco-nexus-9000-nx-os-fundamentals-configuration-guide-93x.html
```

```
Cisco Nexus 9000 Series NX-OS Interfaces Configuration Guide, Release 9.3(x):

https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus9000/sw/93x/interfaces/configuration/guide/b-cisco-nexus-9000-nx-os-interfaces-configuration-guide-93x.html
```

```
Cisco Nexus 9000 Series NX-OS Layer 2 Switching Configuration Guide, Release 9.3(x):

https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus9000/sw/93x/layer-2-switching/configuration/guide/b-cisco-nexus-9000-nx-os-layer-2-switching-configuration-guide-93x.html
```

```
Cisco Nexus 9000 Series NX-OS Unicast Routing Configuration Guide, Release 9.3(x):

https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus9000/sw/93x/unicast/configuration/guide/b-cisco-nexus-9000-series-nx-os-unicast-routing-configuration-guide-93x.html
```

```
Cisco Nexus 9000 Series NX-OS VXLAN Configuration Guide, Release 9.3(x):

https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus9000/sw/93x/vxlan/configuration/guide/b-cisco-nexus-9000-series-nx-os-vxlan-configuration-guide-93x.html
```

```
Cisco Nexus 9000 Series NX-OS Verified Scalability Guide, Release 9.3(8):

https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus9000/sw/93x/scalability/guide-938/cisco-nexus-9000-series-nx-os-verified-scalability-guide-938.html
```

```
Dalsie odkazy:

https://youtu.be/59aYt_kMviE

https://netcraftsmen.com/designing-cisco-nexus-9k/

https://www.cisco.com/c/dam/m/sl_si/events/2016/cisco_dan_inovativnih_resitev/pdf/cisco_day_slovenia_2016_vxlan_marian_klas_final.pdf

https://www.cisco.com/c/dam/en/us/td/docs/switches/datacenter/nexus9000/sw/vxlan_evpn/VXLAN_EVPN.pdf

https://www.ciscolive.com/c/dam/r/ciscolive/us/docs/2020/pdf/DGTL-BRKDCN-1645.pdf

https://www.cisco.com/c/en/us/products/collateral/switches/nexus-9000-series-switches/guide-c07-734107.pdf

https://www.cisco.com/c/en/us/products/collateral/switches/nexus-9000-series-switches/white-paper-c11-739942.html

https://www.cisco.com/c/en/us/td/docs/dcn/whitepapers/migrating-fabricpath-environment-vxlan-bgp-evpn.html

https://www.youtube.com/watch?v=Fv0CFxxK1pk

https://www.youtube.com/watch?v=pgKJ0MbrmLo

https://www.youtube.com/watch?v=YNqKDI_bnPM

https://www.youtube.com/watch?v=BMIsZiGCml0

https://www.youtube.com/watch?v=2bRec2en7zQ

https://www.youtube.com/watch?v=bSiriF8kM7E

https://community.cisco.com/t5/data-center-switches/vpc-orphan-port/td-p/4070039

https://community.cisco.com/t5/data-center-switches/evpn-multihoming-esi-lag/m-p/4523061#M7920

https://community.cisco.com/t5/data-center-switches/why-vxlan-evpn-ebgp-needs-to-configure-quot-retain-route-target/td-p/4126609

https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/about-writing-and-formatting-on-github

https://frrouting.org/doc/

https://www.openvswitch.org/

https://docs.openstack.org/neutron/latest/

https://www.nvidia.com/en-us/networking/ethernet/connectx-6/

https://www.intel.com/content/www/us/en/developer/articles/technical/the-25-gigabit-ethernet-card-is-here.html

https://www.broadcom.com/products/ethernet-connectivity/network-adapters/n2100g

```
---
### Skript v jazyku Python3 na generovanie zakladnej VXLAN-EVPN konfiguracie:

https://github.com/vincentvlk/nexus-vxlan-evpn-priklad/blob/f56fe86e8977aa5fca36c08fd71faa4113278013/vxlan-cis-nex-generator.py#L1-L238

---
### Obsah tejto stranky bol vygenerovany BASH skritpom:

- na odkaze: `https://github.com/ekalinin/github-markdown-toc`
  - obsah bolo potrebne dodatocne upravit
