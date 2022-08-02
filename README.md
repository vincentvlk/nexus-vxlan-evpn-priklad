# Cisco Nexus 9300v VXLAN BGP-EVPN - Priklad
![GNS3 - testovacia VXLAN-EVPN topologia](https://github.com/vincentvlk/nexus-vxlan-evpn-priklad/blob/main/gh-gns3-vxlan-evpn-topologia.png)

## Pomocne poznamky na mini-projekt "nexus-vxlan-evpn-priklad"

### Pouzite GNS3 a VM image:

GNS3: `Ver-2.2.32 (Win10Pro)`

VXLAN Fabric - Cisco Nexus 9300v: `nexus9300v.9.3.10.qcow2`

Tenant Switche - L3-Catalyst-IOSv: `vios_l2-adventerprisek9-m.ssa.high_iron_20200929.qcow2`

Inet-R1 - CSR1000-IOS-XE: `csr1000v-universalk9.16.12.03-serial.qcow2`

### Postrehy z konfiguracie a testovania:

- vPC + VXLAN + EVPN - je nutne aby VNI config bol konzistentny (podla ocakavania)

- PROBLEM: N9300V nabehne bez funkcneho nve1, chyba polozka v conf. mode, v start-conf ale je
  - zatial `reload`, je to menej konfigurovania, ako odstranit `VXLAN features`

- PROBLEM: N9300V nabehne s neaktivnymi Ifaces, aj ked su zapnute v start-conf
  - stacil `shutdown` / `no shutdown`, sposobuje hlavne vypadok OSPF a teda *Underlay*

- problem, na strane zakaznika flapuje port-channel ked je "routed" (vIOS-L3-Cat)
  - pravdepodobne zalezitost GNS3+Qemu+KVM, nic vazne

- problem, s BPDU guardom, aj ked je na druhej strane BPDUfilter

- problem, trebalo vycistit ARP zaznam na zakaznickom switchy (vIOS-L2)

- problem s HA, ked vypnem jednu stranu VLAN101 na VPC parente, napr. N91

- conf. backup s: copy running-config scp://vlkv@192.168.4.244/ vrf default

- Xconnect a QinVNI je pri vPC obmedzene, treba pouzit prikazy:
  "system dot1q-tunnel transit [vlan vlan-range]" spolu s:
  "system nve infra-vlans <vlan-range>"
  - pouzity Nexus9300v (NX-OSv 9.3.10) tieto 2 prikazy nema, nenasiel som zatial

- problem, nejde IPv4 BFD s OSPFv2, prikazy su, ale nezdvyhne sa session

For proper operation during L3 uplink failure scenarios on vPC VTEPs,
configure a backup SVI and enter the
"system nve infra-vlans <backup-svi-vlan" command.
On Cisco Nexus 9000-EX platform switches,
the backup SVI VLAN needs to be the native VLAN on the peer-link.

- pozorovanie, VTEP automaticky presmeruva IP traffic na svju VLAN-gw aj ked
  ma CE zariadenie nastavenu inu L3 gw, ktora je L2 dostupna, ma aj ARP zaznam
  - pozriet ci sa da vypnut/zmenit?

- pozorovanie, fyz. porty ktore su definovane ako Xconnect-dot1q, technologia
  vPC nepovazuje za "Orphan" porty, vid.: "show vpc orphan-ports"
