# CML Lab — Standard, Extended & Named ACLs

**CCNA Objective:** Vol 2, Part II — Access Control Lists (Ch 6–8)
**Completed:** 2026-04-11
**Lab Tool:** Cisco Modeling Lab (CML)

---

## Overview

This lab covers all three ACL types on the CCNA exam:

| ACL Type | Filters On | Placement |
|---|---|---|
| Standard | Source IP only | Near **destination** |
| Extended | Src IP + Dst IP + Protocol + Port | Near **source** |
| Named Extended | Same as Extended, plus sequence number editing | Near **source** |

---

## Topology

```
[PC1-DeptA]──[SW1-DeptA]──G0/1──[  R1  ]──G0/3──WAN──[  R2  ]──G0/2──[SW3-Servers]──[WebServer 10.1.1.10]
                                    |                                                   [FTPServer 10.1.1.20]
[PC2-DeptB]──[SW2-DeptB]──G0/2──┘
```

### IP Addressing

| Node | Interface | IP Address | Role |
|---|---|---|---|
| R1 | G0/1 | 192.168.1.1/24 | Dept-A gateway |
| R1 | G0/2 | 192.168.2.1/24 | Dept-B gateway |
| R1 | G0/3 | 10.0.0.1/30 | WAN link |
| R2 | G0/1 | 10.0.0.2/30 | WAN link |
| R2 | G0/2 | 10.1.1.1/24 | Server network gateway |
| PC1-DeptA | eth0 | 192.168.1.10/24 | Dept-A host |
| PC2-DeptB | eth0 | 192.168.2.10/24 | Dept-B host |
| WebServer | eth0 | 10.1.1.10/24 | Web server |
| FTPServer | eth0 | 10.1.1.20/24 | FTP server |

### Static Routes

```
! R1 — reach server network via R2
ip route 10.1.1.0 255.255.255.0 10.0.0.2

! R2 — reach both dept networks via R1
ip route 192.168.1.0 255.255.255.0 10.0.0.1
ip route 192.168.2.0 255.255.255.0 10.0.0.1
```

---

## Lab 1 — Standard ACL

**Scenario:** Permit only Dept-A (192.168.1.0/24) to reach the server network. Block Dept-B.

**Rule:** Standard ACLs filter on source IP only → place near the **destination** (applied on R2).

### Configuration (R2)

```
ip access-list standard PERMIT-DEPTA
 permit 192.168.1.0 0.0.0.255
 deny   any
!
interface GigabitEthernet0/2
 ip access-group PERMIT-DEPTA in
```

### Verification

```
R2# show access-lists
Standard IP access list PERMIT-DEPTA
    10 permit 192.168.1.0, wildcard bits 0.0.0.255
    20 deny   any

R2# show ip interface GigabitEthernet0/2
  Inbound  access list is PERMIT-DEPTA
```

**Result:** ✅ Dept-A can reach servers; Dept-B is blocked.

---

## Lab 2 — Extended ACL

**Scenario:** Block Dept-B (192.168.2.0/24) from reaching WebServer (10.1.1.10) on HTTP (port 80). All other traffic permitted.

**Rule:** Extended ACLs filter on src + dst + protocol + port → place near the **source** (applied on R1, inbound G0/2).

### Configuration (R1)

```
ip access-list extended BLOCK-HTTP-WEBSERVER
 deny   tcp 192.168.2.0 0.0.0.255 host 10.1.1.10 eq 80
 permit ip any any
!
interface GigabitEthernet0/2
 ip access-group BLOCK-HTTP-WEBSERVER in
```

### Verification

```
R1# show access-lists
Extended IP access list BLOCK-HTTP-WEBSERVER
    10 deny tcp 192.168.2.0 0.0.0.255 host 10.1.1.10 eq www
    20 permit ip any any
```

**Result:** ✅ HTTP to WebServer blocked from Dept-B; ICMP, Telnet, and all other traffic still permitted.

---

## Lab 3 — Named Extended ACL

**Scenario:** Block all Telnet (port 23) from Dept-B to the entire server network (10.1.1.0/24). All other traffic permitted.

**Rule:** Named ACLs use descriptive names and support sequence number editing (`no 10` removes a rule; re-add with desired sequence).

### Configuration (R1)

```
ip access-list extended BLOCK_TELNET_TO_SERVER_NETWORK
 10 deny   tcp 192.168.2.0 0.0.0.255 10.1.1.0 0.0.0.255 eq 23 log
 20 permit ip any any
!
interface GigabitEthernet0/2
 ip access-group BLOCK_TELNET_TO_SERVER_NETWORK in
```

> The `log` keyword on deny rules generates syslog messages on each match — useful for verification and troubleshooting.

### Verification

```
R1# show access-lists
Extended IP access list BLOCK_TELNET_TO_SERVER_NETWORK
    10 deny tcp 192.168.2.0 0.0.0.255 10.1.1.0 0.0.0.255 eq telnet log
    20 permit ip any any

R1# show ip interface GigabitEthernet0/2
  Inbound  access list is BLOCK_TELNET_TO_SERVER_NETWORK
```

**Syslog confirmation (from CML console):**

```
%SEC-6-IPACCESSLOGP: list BLOCK_TELNET_TO_SERVER_NETWORK denied tcp 192.168.2.10(55994) -> 10.1.1.10(23), 1 packet
%SEC-6-IPACCESSLOGP: list BLOCK_TELNET_TO_SERVER_NETWORK denied tcp 192.168.2.10(47542) -> 10.1.1.20(23), 1 packet
```

**Result:** ✅ Telnet blocked to both WebServer and FTPServer; all other traffic permitted.

---

## Key Takeaways

- The **implicit deny any** at the end of every ACL means you must always include an explicit `permit ip any any` if you only want to block specific traffic.
- Standard ACLs placed near the source would block too much traffic — always place near the destination.
- Extended ACLs placed near the destination waste WAN bandwidth — always place near the source.
- Named ACLs are the production standard because rules can be added, removed, or reordered by sequence number without rewriting the entire ACL.
- Use `show access-lists` to see hit counters and verify rules are being matched.
- Use `show ip interface <intf>` to confirm which ACL is applied and in which direction.

---

## Useful Commands Reference

| Command | Purpose |
|---|---|
| `show access-lists` | View all ACLs and hit counters |
| `show ip interface <intf>` | See ACL applied to an interface |
| `show run \| section access-list` | View ACL configs in running config |
| `show run \| section interface` | View interface configs including access-groups |
| `no ip access-group <name> in` | Remove ACL from interface |
| `no ip access-list extended <name>` | Delete a named ACL |
| `ip access-list extended <name>` then `no <seq>` | Delete a single rule by sequence number |
