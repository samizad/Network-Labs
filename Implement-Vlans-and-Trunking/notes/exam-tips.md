# VLAN and Trunking — ENCOR Exam Tips

## Before You Touch the CLI

1. **Read the entire lab first** — understand the end state before configuring anything
2. **Draw the topology** — mark every port, trunk link, and VLAN assignment
3. **Copy VLAN names exactly** — capitalisation matters (`Management` ≠ `management`)
4. **Note which trunks are static vs dynamic** — determines which commands to use

---

## Recommended Configuration Order

```
1. VLANs on ALL switches first          (Part 1)
2. Access ports on edge switches        (Part 2)
3. Voice VLAN ports                     (Part 2)
4. SVIs on all switches                 (Part 2)
5. Static trunk ports                   (Part 3)
6. Dynamic trunk ports                  (Part 4)
```

Never assign a port to a VLAN before creating the VLAN — it will go into VLAN 1 by default.

---

## Verify After Every Step

| After configuring | Run this command |
|-------------------|-----------------|
| VLANs | `show vlan brief` |
| Access ports | `show vlan brief` |
| Voice VLAN | `show interfaces Fa0/x switchport` |
| SVI | `show interfaces vlan 99` |
| Trunk | `show interface trunk` |
| Everything | `show running-config` |

---

## Common Mistakes That Cost Marks

### 1. Wrong VLAN name capitalisation
```
❌  name management
✅  name Management
```
Always copy names exactly from the lab table.

### 2. Forgetting `no shutdown` on SVIs
```
❌  interface Vlan99
    ip address 192.168.99.252 255.255.255.0

✅  interface Vlan99
    ip address 192.168.99.252 255.255.255.0
    no shutdown
```

### 3. Native VLAN mismatch
Configure native VLAN on **both ends** immediately — never do one end and move on.
```
switchport trunk native vlan 100   ← required on BOTH switches
```

### 4. Using `nonegotiate` on dynamic trunks
```
❌  Dynamic trunk:
    switchport mode dynamic desirable
    switchport nonegotiate           ← kills DTP, trunk won't form

✅  Dynamic trunk:
    switchport mode dynamic desirable
    (no nonegotiate)
```

### 5. Leaving `nonegotiate` off static trunks
```
❌  Static trunk:
    switchport mode trunk
    (missing nonegotiate = DTP still running = security risk)

✅  Static trunk:
    switchport mode trunk
    switchport nonegotiate
```

### 6. Not restricting allowed VLANs
```
❌  (default = allows all VLANs 1-1005)

✅  switchport trunk allowed vlan 10,20,30,40,99
```

### 7. auto + auto = no trunk
If both ends are left at default `dynamic auto`, no trunk forms.
One end must be `dynamic desirable` or `trunk`.

---

## Key Show Commands Explained

### `show vlan brief`
- Confirms VLANs exist with correct names
- Shows which ports belong to each VLAN
- Does NOT show trunk ports (they don't appear here)

### `show interface trunk`
- Shows all active trunk ports
- Check: Mode (`on`/`desirable`/`auto`), Native VLAN, Allowed VLANs, Status (`trunking`)
- If a port doesn't appear here it is NOT trunking

### `show interfaces Fa0/x switchport`
- Shows full switchport details for any port
- For voice VLAN: look for both `Access Mode VLAN` and `Voice VLAN`
- For trunks: look for `Administrative Mode` and `Operational Mode`

### `show interfaces vlan 99`
- Confirms SVI is up/up
- Shows IP address assigned

---

## DTP Quick Reference

| Combination | Result |
|------------|--------|
| trunk + trunk | trunk ✅ |
| trunk + desirable | trunk ✅ |
| trunk + auto | trunk ✅ |
| desirable + desirable | trunk ✅ |
| desirable + auto | trunk ✅ |
| auto + auto | access ❌ |
| anything + access | access ❌ |
| nonegotiate + nonegotiate | trunk ✅ (no DTP) |
| nonegotiate + auto/desirable | trunk ✅ (no DTP on one side) |

---

## Time Budget for This Lab Type

| Section | Target time |
|---------|------------|
| Read + draw topology | 5 min |
| Part 1 — VLANs | 5 min |
| Part 2 — Ports + SVIs | 8 min |
| Part 3 — Static trunks | 8 min |
| Part 4 — Dynamic trunks | 5 min |
| Final verification | 5 min |
| **Total** | **~35 min** |

If stuck on one section for more than 5 minutes, move on and come back.

---

## Final Verification Checklist

Before submitting run this on every switch:

- [ ] `show vlan brief` — all VLANs present, correct names, correct ports
- [ ] `show interface trunk` — all trunk ports show `trunking`, native VLAN 100, correct allowed VLANs
- [ ] `show interfaces vlan 99` — SVI is up/up with correct IP
- [ ] No CDP native VLAN mismatch messages in console
- [ ] No STP PVID error messages in console
