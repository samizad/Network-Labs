# VLAN and Trunking — Concepts

## VLANs

A VLAN (Virtual LAN) is a logical grouping of switch ports into separate broadcast domains. Devices in different VLANs cannot communicate directly — they need a Layer 3 device (router or L3 switch) to route between them.

### How VLANs are stored
VLANs are stored in `vlan.dat` on flash memory — **not** in the running-config. This means:
- VLANs survive a reboot even without `write memory`
- Deleting `vlan.dat` removes all VLANs even if startup-config looks correct
- Always verify with `show vlan brief` after configuring

### Reserved VLANs
VLANs 1, 1002–1005 are reserved and cannot be deleted or renamed.

---

## Access Ports

An access port belongs to exactly one VLAN and carries untagged traffic. Used for end devices (PCs, printers).

```
switchport mode access
switchport access vlan 10
```

---

## Voice VLAN

A single physical port that carries two VLANs simultaneously:
- **Data VLAN** — carries PC traffic (untagged)
- **Voice VLAN** — carries IP phone traffic (tagged by the phone)

The switch uses CDP to tell the IP phone which VLAN to tag its traffic with.

```
switchport mode access
switchport access vlan 10
switchport voice vlan 40
```

Important: this port is still an **access port**, not a trunk. The phone handles the tagging internally.

Verify with:
```
show interfaces Fa0/x switchport
```
Look for both `Access Mode VLAN` and `Voice VLAN` in the output.

---

## SVIs (Switched Virtual Interfaces)

An SVI is a Layer 3 interface on a switch, one per VLAN. Used for switch management (SSH, Telnet, ping).

```
interface Vlan99
 ip address 192.168.99.252 255.255.255.0
 no shutdown
```

An SVI only comes up/up if:
1. The VLAN exists in the database
2. At least one physical port assigned to that VLAN is active

---

## Trunk Ports

A trunk port carries multiple VLANs between switches using 802.1Q tagging. Every frame on a trunk (except the native VLAN) gets a 4-byte 802.1Q tag added with the VLAN ID.

### Static trunking
```
switchport mode trunk
switchport nonegotiate
switchport trunk native vlan 100
switchport trunk allowed vlan 10,20,30,40,99
```

### Native VLAN
The one VLAN that crosses a trunk **untagged**. Both ends must match — mismatch causes:
- CDP warning: `%CDP-4-NATIVE_VLAN_MISMATCH`
- STP blocking: `%SPANTREE-2-BLOCK_PVID_LOCAL`
- Potential VLAN hopping vulnerability

Best practice: use an unused VLAN (not VLAN 1) as native.

---

## DTP (Dynamic Trunking Protocol)

Cisco proprietary protocol that automatically negotiates trunking between switches.

### DTP modes

| Mode | Behaviour |
|------|-----------|
| `trunk` | Always trunk, sends DTP frames |
| `dynamic desirable` | Actively tries to form a trunk |
| `dynamic auto` | Passively waits — default on 2960 |
| `access` | Never trunks |
| `nonegotiate` | Always trunk, sends NO DTP frames |

### DTP negotiation matrix

| | trunk | desirable | auto | access |
|---|---|---|---|---|
| **trunk** | trunk ✅ | trunk ✅ | trunk ✅ | ❌ |
| **desirable** | trunk ✅ | trunk ✅ | trunk ✅ | ❌ |
| **auto** | trunk ✅ | trunk ✅ | access ❌ | ❌ |
| **access** | ❌ | ❌ | ❌ | access |

Key point: `auto` + `auto` = **access** — both wait, nobody initiates.

### Security note
DTP should be disabled on all ports facing untrusted devices using `switchport nonegotiate`. Leaving DTP active allows an attacker to negotiate a trunk and gain access to all VLANs.

---

## Console Log Messages Reference

| Message | Meaning | Fix |
|---------|---------|-----|
| `CDP-4-NATIVE_VLAN_MISMATCH` | Native VLAN differs on both ends | Set same native VLAN on both ends |
| `SPANTREE-2-RECV_PVID_ERR` | STP received tagged BPDUs on non-trunk port | Configure both ends as trunk |
| `SPANTREE-2-BLOCK_PVID_LOCAL` | STP blocked port due to VLAN inconsistency | Fix native VLAN mismatch |
| `LINEPROTO-5-UPDOWN` | Port changed state | Normal when changing port mode |
