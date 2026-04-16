# Lab-12: DHCP Snooping and Dynamic ARP Inspection — Task Guide

**CCNA 200-301 v1.1 Exam Topic:**
- 5.7 — Configure and verify Layer 2 security features (DHCP snooping, dynamic ARP inspection)

**CML Lab ID:** `1f68da4b-7452-458a-9dab-1ead741fc1fa`

---

## Topology

```
                      [R1]
                   10.1.1.1/24
                (Gateway + DHCP Server)
                       |
            Gi0/0 ← TRUSTED port (DHCP snooping + DAI)
                       |
                      [SW1]
              /         |          \
           Gi0/1      Gi0/2       Gi0/3
             |           |           |
          [PC1]       [PC2]      [ROGUE]
       10.1.1.10    10.1.1.20   10.1.1.99
      (Tiny Linux) (Tiny Linux) (Tiny Linux)
```

| Device | Interface | IP Address | Role |
|--------|-----------|------------|------|
| R1 | GigabitEthernet0/0 | 10.1.1.1/24 | Gateway + legitimate DHCP server |
| SW1 | Gi0/0 | — | Uplink to R1 — **TRUSTED** |
| SW1 | Gi0/1 | — | Access port → PC1 — untrusted |
| SW1 | Gi0/2 | — | Access port → PC2 — untrusted |
| SW1 | Gi0/3 | — | Access port → ROGUE — untrusted |
| PC1 | eth0 | 10.1.1.10/24 | Legitimate DHCP client |
| PC2 | eth0 | 10.1.1.20/24 | Legitimate DHCP client |
| ROGUE | eth0 | 10.1.1.99/24 | Adversary: rogue DHCP + ARP attacker |

---

## Lab Base State

- SW1: all ports in access mode, **no DHCP snooping or DAI configured**
- R1: `ip dhcp pool CCNA-LAB` configured and ready (network 10.1.1.0/24, gw 10.1.1.1, DNS 8.8.8.8, lease 1 day)
- PC1: static IP 10.1.1.10/24 preconfigured via boot script
- PC2: static IP 10.1.1.20/24 preconfigured via boot script
- ROGUE: static IP 10.1.1.99/24 preconfigured via boot script

### Tiny Core Linux Network Commands

| Purpose | Command |
|---------|---------|
| Check IP | `ifconfig eth0` |
| Release static IP | `ifconfig eth0 0.0.0.0` |
| Request DHCP lease | `udhcpc -i eth0` |
| Set static IP | `ifconfig eth0 <ip> netmask 255.255.255.0 up` |
| Add default route | `route add default gw 10.1.1.1` |
| Test connectivity | `ping <ip>` |
| View ARP table | `arp -n` |

---

## Part 1 — DHCP Snooping

### Task 1 — Verify Base Connectivity Before Configuring DHCP Snooping

**Exam Topic: 5.7**

Before configuring any security features, verify that the base topology is fully operational.

1. From **PC1**, ping **R1** (10.1.1.1) and **PC2** (10.1.1.20).
2. From **PC2**, ping **R1** (10.1.1.1) and **PC1** (10.1.1.10).
3. From **ROGUE**, ping **R1** (10.1.1.1).
4. On **SW1**, confirm all four active ports are in access mode.

**Verification:**
```
SW1# show interfaces status
SW1# show running-config | section interface GigabitEthernet
```

Confirm all four ports show **connected** and are in access mode with no DHCP snooping or DAI commands present in the running config.

---

### Task 2 — Enable DHCP Snooping Globally and on VLAN 1

**Exam Topic: 5.7**

DHCP snooping is disabled by default. It must be enabled in two steps: globally (which activates the feature on the switch) and per-VLAN (which specifies which VLANs to enforce snooping on).

1. On **SW1**, enable DHCP snooping globally.
2. Enable DHCP snooping specifically on **VLAN 1**.

**Important:** After enabling DHCP snooping globally, all ports on the VLAN become **untrusted** by default. Any DHCP server responses (OFFER, ACK, NAK) arriving on an untrusted port will be dropped.

**Verification:**
```
SW1# show ip dhcp snooping
```

Confirm the output shows:
- `DHCP snooping is enabled`
- `DHCP snooping is configured on the following VLANs: 1`
- All four ports listed as **untrusted**

---

### Task 3 — Configure the Trusted Port (Gi0/0 → R1)

**Exam Topic: 5.7**

After enabling DHCP snooping, all ports are untrusted. You must explicitly mark the port connected to the legitimate DHCP server as trusted. Only trusted ports are allowed to forward DHCP server messages (OFFER and ACK). Untrusted ports can only send client messages (DISCOVER and REQUEST).

1. On **SW1**, configure **GigabitEthernet0/0** (the port connected to R1) as a DHCP snooping **trusted** port.

**Verification:**
```
SW1# show ip dhcp snooping
```

Confirm that Gi0/0 now shows as **trusted** and all other ports remain **untrusted**.

---

### Task 4 — Populate the DHCP Snooping Binding Table

**Exam Topic: 5.7**

The DHCP snooping binding table is populated dynamically when clients receive DHCP leases through the switch. The table records the client's MAC address, assigned IP address, VLAN, port, and lease expiry time. This binding table is also used by Dynamic ARP Inspection.

1. On **PC1**, release the static IP address and request a new IP via DHCP.
2. On **PC2**, release the static IP address and request a new IP via DHCP.

**PC1 and PC2 commands (run on each Tiny Linux node):**
```
ifconfig eth0 0.0.0.0
udhcpc -i eth0
ifconfig eth0
```

After running `udhcpc`, confirm that PC1 and PC2 each received an IP address from R1's DHCP pool (10.1.1.10–10.1.1.98 or 10.1.1.100–10.1.1.254).

**On R1, verify the lease was issued:**
```
R1# show ip dhcp binding
```

---

### Task 5 — Verify the DHCP Snooping Binding Table

**Exam Topic: 5.7**

After PC1 and PC2 obtain their DHCP leases, the binding table on SW1 should contain an entry for each client.

1. On **SW1**, display the DHCP snooping binding table.
2. Identify the MAC address, IP address, VLAN, and interface for each entry.

**Verification:**
```
SW1# show ip dhcp snooping binding
```

Confirm that the table contains entries for PC1 and PC2, showing their MAC addresses, DHCP-assigned IP addresses, the correct VLAN (1), the correct port (Gi0/1 and Gi0/2), and a lease time.

**Also verify on R1:**
```
R1# show ip dhcp binding
R1# show ip dhcp pool CCNA-LAB
```

---

### Task 6 — Configure DHCP Message Rate Limiting on Untrusted Ports

**Exam Topic: 5.7**

DHCP snooping can rate-limit DHCP messages on untrusted ports to protect against DHCP starvation attacks (where an attacker floods the switch with DISCOVER messages using spoofed MAC addresses to exhaust the DHCP pool). Rate limiting caps how many DHCP packets per second an untrusted port will forward.

1. On **SW1**, configure a DHCP snooping rate limit of **10 packets per second** on **GigabitEthernet0/1** (PC1).
2. Configure the same 10 pps rate limit on **GigabitEthernet0/2** (PC2).
3. Configure the same 10 pps rate limit on **GigabitEthernet0/3** (ROGUE).

**Verification:**
```
SW1# show ip dhcp snooping
```

Confirm each untrusted port shows a rate limit of 10 in the output. The trusted port (Gi0/0) does not need a rate limit.

---

## Part 2 — Dynamic ARP Inspection (DAI)

### Task 7 — Enable Dynamic ARP Inspection on VLAN 1

**Exam Topic: 5.7**

Dynamic ARP Inspection (DAI) validates ARP packets against the DHCP snooping binding table. When DAI is enabled on a VLAN, ARP packets received on **untrusted** ports are checked: the sender's MAC address and IP address in the ARP packet must match an entry in the binding table. If there is no match, the ARP packet is dropped.

Without DAI, an attacker can send **gratuitous ARP** (unsolicited ARP replies) to poison the ARP cache of other hosts, redirecting traffic through the attacker's device (a man-in-the-middle attack).

1. On **SW1**, enable Dynamic ARP Inspection on **VLAN 1**.

**Verification:**
```
SW1# show ip arp inspection
SW1# show ip arp inspection vlan 1
```

Confirm DAI is enabled on VLAN 1 and that all ports show as **untrusted** by default.

---

### Task 8 — Configure the Trusted Port for DAI (Gi0/0 → R1)

**Exam Topic: 5.7**

Just like DHCP snooping, DAI also uses trusted and untrusted ports. All ports are untrusted by default when DAI is enabled. The port connected to R1 (the gateway) must be marked as trusted for DAI — otherwise, R1's ARP packets would also be inspected and potentially dropped if R1's IP is not in the snooping binding table.

1. On **SW1**, configure **GigabitEthernet0/0** as a DAI **trusted** port.

**Verification:**
```
SW1# show ip arp inspection
```

Confirm Gi0/0 shows as **Trusted** and all other ports remain **Untrusted**.

---

### Task 9 — Configure Optional DAI Message Checks

**Exam Topic: 5.7**

DAI supports optional additional validation checks beyond the basic MAC-IP binding check. The `ip arp inspection validate` command allows three additional checks:

- **src-mac** — validates that the source MAC in the ARP packet matches the sender hardware address in the ARP body
- **dst-mac** — validates the destination MAC in the ARP reply matches the target hardware address
- **ip** — validates that the sender or target IP address is not 0.0.0.0, 255.255.255.255, or any IP multicast address

1. On **SW1**, configure DAI to validate all three optional checks: **src-mac**, **dst-mac**, and **ip**.

**Note:** All three options must be configured in a single command — issuing the command multiple times replaces the previous setting rather than adding to it.

**Verification:**
```
SW1# show ip arp inspection vlan 1
```

Confirm the output shows all three validation options enabled.

---

### Task 10 — Full Verification Using All Show Commands

**Exam Topic: 5.7**

Run all DHCP snooping and DAI verification commands and interpret each output field.

**DHCP Snooping commands:**
```
SW1# show ip dhcp snooping
SW1# show ip dhcp snooping binding
SW1# show ip dhcp snooping statistics
```

**DAI commands:**
```
SW1# show ip arp inspection
SW1# show ip arp inspection vlan 1
SW1# show ip arp inspection statistics
SW1# show ip arp inspection interfaces
```

**Also verify on R1:**
```
R1# show ip dhcp binding
R1# show ip dhcp pool CCNA-LAB
```

For each command output, confirm and document:

**DHCP Snooping:**
- DHCP snooping is enabled globally
- VLAN 1 is listed as a snooping-enabled VLAN
- Gi0/0 is trusted; all other ports are untrusted
- Rate limits of 10 pps are configured on all untrusted ports
- Binding table contains entries for PC1 and PC2 (MAC, IP, VLAN, port, lease time)

**DAI:**
- DAI is enabled on VLAN 1
- Gi0/0 is trusted for DAI; all other ports are untrusted
- Optional checks src-mac, dst-mac, and ip are all enabled
- DAI statistics show forwarded and dropped ARP count per port

---

## Grading Rubric

| Task | Points | Topic |
|------|--------|-------|
| Task 1 | 5 | Base connectivity verified before security config |
| Task 2 | 10 | DHCP snooping enabled globally and on VLAN 1 |
| Task 3 | 10 | Trusted port configured on Gi0/0 |
| Task 4 | 10 | Binding table populated (PC1 + PC2 DHCP leases) |
| Task 5 | 10 | Binding table verified with correct fields |
| Task 6 | 10 | Rate limiting configured on all untrusted ports |
| Task 7 | 15 | DAI enabled on VLAN 1 |
| Task 8 | 10 | Trusted port configured for DAI on Gi0/0 |
| Task 9 | 10 | Optional DAI checks (src-mac, dst-mac, ip) configured |
| Task 10 | 10 | Full show command verification |
| **Total** | **100** | |

### Scoring

| Score | Result |
|-------|--------|
| 90–100 | ✅ Excellent — Exam ready on this topic |
| 75–89 | ⚠️ Good — Minor gaps; review missed items |
| Below 75 | ❌ Needs review — Re-read Ch12 Vol2 and retry |

---

## Key Concepts Quick Reference

### DHCP Snooping

| Concept | Detail |
|---------|--------|
| Default state | Disabled |
| Enable globally | `ip dhcp snooping` |
| Enable per VLAN | `ip dhcp snooping vlan <id>` |
| Trust a port | `ip dhcp snooping trust` (interface mode) |
| Rate limit | `ip dhcp snooping limit rate <pps>` (interface mode) |
| Default port state | All ports **untrusted** after snooping is enabled |
| Untrusted port drops | OFFER, ACK, NAK (server-to-client messages) |
| Trusted port allows | All DHCP message types |
| Binding table | Show with `show ip dhcp snooping binding` |

### Dynamic ARP Inspection

| Concept | Detail |
|---------|--------|
| Depends on | DHCP snooping binding table |
| Enable per VLAN | `ip arp inspection vlan <id>` |
| Trust a port | `ip arp inspection trust` (interface mode) |
| Rate limit | `ip arp inspection limit rate <pps>` (interface mode) |
| Default port state | All ports **untrusted** after DAI is enabled |
| Optional checks | `ip arp inspection validate {src-mac} {dst-mac} {ip}` |
| Gratuitous ARP | Unsolicited ARP reply — used in ARP poisoning attacks |
| Binding match | ARP sender MAC + IP must match binding table entry |

### Attack Scenarios This Lab Defends Against

| Attack | Defense |
|--------|---------|
| Spurious DHCP server | DHCP snooping drops OFFER/ACK from untrusted ports |
| DHCP starvation | Rate limiting on untrusted ports limits DISCOVER floods |
| ARP poisoning (MITM) | DAI drops ARP packets whose MAC-IP pair is not in binding table |
| Gratuitous ARP injection | DAI on untrusted ports blocks unsolicited ARP replies |
