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

- PROBLEM: N9300V nabehne bez funkcneho `nve1`, chyba polozka v conf. mode, v start-conf ale je
  - zatial `reload`, je to menej konfigurovania, ako odstranit `VXLAN features`

- PROBLEM: N9300V nabehne s neaktivnymi Ifaces, aj ked su zapnute v start-conf
  - stacil `shutdown` / `no shutdown`, sposobuje hlavne vypadok OSPF a teda *Underlay*

- problem, na strane zakaznika flapuje port-channel ked je "routed" (vIOS-L3-Cat)
  - pravdepodobne zalezitost `GNS3+Qemu+KVM`, nic vazne, ale otravuju logy na konzole :)

- problem, s BPDU guardom, aj ked je na druhej strane BPDUfilter
  - este treba otestovat, pretoze problem sa v niektorych pripadoch neprejavil
  - pravdepodobne zalezitost `GNS3+Qemu+KVM`

- problem, trebalo vycistit ARP zaznam na zakaznickom switchy (vIOS-L3)

- problem s HA, ked vypnem jednu stranu VLAN101 na vPC parente, napr. `N91-Leaf1`
  - nestandarny stav, aj tak treba vypinat celu fyz. cestu v pripade maint.

- conf. backup s: `copy running-config scp://vlkv@192.168.4.244/ vrf default`

- `Xconnect` a `QinVNI` je pri vPC obmedzene, treba pouzit prikazy:

  `system dot1q-tunnel transit [vlan vlan-range]` 
   
  spolu s:
  `system nve infra-vlans <vlan-range>`

  - treba si uvedomit, ze tieto 2 funkcie si *silne zavisle* od VTEP-HW (ASICov)

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

- pozorovanie, fyz. porty, ktore su definovane ako Xconnect-dot1q, technologia
  vPC nepovazuje za "Orphan" porty, vid.: `show vpc orphan-ports`

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

   - zariadenia A:SW4 a B:SW4 maju rovnake IP ale s VRF+NAT sa dostanu na Inet

---

