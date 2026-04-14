# Lab-11: Implementing Switch Port Security — Task Guide

**CCNA 200-301 v1.1 Exam Topic:**
- 5.7 — Configure and verify Layer 2 security features (port security)

**CML Lab ID:** `5951f588-5611-4dc2-8d9d-1663eca74546`

---

## Topology

```
  [PC1] ------Gi0/1------ [SW1] ------Gi0/2------ [PC2]
  10.1.1.10/24              |                      10.1.1.20/24
                           Gi0/3
                            |
                          [PC3]  10.1.1.30/24
                            |
                           Gi0/0
                            |
                        [UNS1-Hub]
                       /          \
                    [PC4]        [ROGUE]
                 10.1.1.40/24   10.1.1.99/24
```

| Device | Interface | IP Address    | Role                              |
|--------|-----------|---------------|-----------------------------------|
| SW1    | Gi0/0     | —             | Uplink to UNS1 hub                |
| SW1    | Gi0/1     | —             | Access port → PC1                 |
| SW1    | Gi0/2     | —             | Access port → PC2                 |
| SW1    | Gi0/3     | —             | Access port → PC3                 |
| PC1    | eth0      | 10.1.1.10/24  | Legitimate endpoint               |
| PC2    | eth0      | 10.1.1.20/24  | Legitimate endpoint               |
| PC3    | eth0      | 10.1.1.30/24  | Legitimate endpoint               |
| PC4    | eth0      | 10.1.1.40/24  | Legitimate endpoint (behind hub)  |
| ROGUE  | eth0      | 10.1.1.99/24  | Unauthorized device (behind hub)  |

**UNS1** is an unmanaged hub. PC4 and ROGUE both share SW1's Gi0/0 port through it.

---

## Lab Base State

- SW1 all active ports are configured as access ports
- No port security is configured on any port
- All interfaces are UP
- UNS1 is an unmanaged hub — PC4 and ROGUE both appear on SW1 Gi0/0

---

## Task 1 — Verify Access Mode on All Active SW1 Ports

**Exam Topic: 5.7**

Before configuring port security, you must confirm that all active SW1 ports are operating as access ports. Port security can only be applied to access or trunk ports — it cannot be applied to ports in dynamic negotiation mode.

1. On **SW1**, verify the operational mode of all active ports.
2. Confirm that Gi0/0, Gi0/1, Gi0/2, and Gi0/3 are all in **access** mode — not dynamic desirable or dynamic auto.
3. If any port is not explicitly set to access mode, configure it now.

**Verification — run and observe the output:**
```
SW1# show interfaces status
SW1# show running-config | section interface GigabitEthernet
```

---

## Task 2 — Configure Sticky MAC Learning with Shutdown Violation Mode on Gi0/1 (PC1)

**Exam Topic: 5.7**

Gi0/1 connects to PC1. You will configure port security using **sticky MAC address learning** so that the switch automatically learns and saves PC1's MAC address to the running configuration. The violation mode must be set to **shutdown**.

1. On **SW1 Gi0/1**, enable port security.
2. Set the **maximum number of allowed MAC addresses** to **1**.
3. Configure the port to use **sticky** MAC address learning — do not manually specify a MAC address.
4. Set the violation mode to **shutdown**.

**Verification — run and observe the output:**
```
SW1# show port-security interface GigabitEthernet0/1
SW1# show running-config interface GigabitEthernet0/1
```

After verifying the configuration, generate traffic from PC1 (e.g., `ping 10.1.1.20`) and observe the sticky MAC entry:

```
SW1# show port-security address
SW1# show running-config interface GigabitEthernet0/1
```

Confirm that PC1's MAC address now appears as a sticky secure MAC in both the running configuration and the port security address table.

---

## Task 3 — Configure Static MAC with Restrict Violation Mode on Gi0/2 (PC2)

**Exam Topic: 5.7**

Gi0/2 connects to PC2. You will configure port security using a **manually specified (static) MAC address**. The MAC address you configure will intentionally **not match** PC2's actual MAC address — this allows you to observe the restrict violation mode behavior.

1. On **SW1**, determine PC2's actual MAC address.
2. On **SW1 Gi0/2**, enable port security.
3. Set the **maximum number of allowed MAC addresses** to **1**.
4. Configure a **static secure MAC address** of `0000.1111.2222` — this address does not match PC2's real MAC.
5. Set the violation mode to **restrict**.

**Verification — run and observe the output:**
```
SW1# show port-security interface GigabitEthernet0/2
SW1# show port-security address
```

Generate traffic from PC2 (e.g., `ping 10.1.1.10`) to trigger a violation. Then observe:

```
SW1# show port-security interface GigabitEthernet0/2
SW1# show interfaces GigabitEthernet0/2
```

Confirm that the port remains **up** but the **violation counter increments** and traffic from PC2 is dropped. Note: restrict mode does not shut down the port.

---

## Task 4 — Configure Static MAC with Protect Violation Mode on Gi0/3 (PC3)

**Exam Topic: 5.7**

Gi0/3 connects to PC3. You will configure port security using a **manually specified (static) MAC address** that does not match PC3's real MAC address — this allows you to observe the protect violation mode behavior.

1. On **SW1 Gi0/3**, enable port security.
2. Set the **maximum number of allowed MAC addresses** to **1**.
3. Configure a **static secure MAC address** of `0000.3333.4444` — this address does not match PC3's real MAC.
4. Set the violation mode to **protect**.

**Verification — run and observe the output:**
```
SW1# show port-security interface GigabitEthernet0/3
SW1# show port-security address
```

Generate traffic from PC3 (e.g., `ping 10.1.1.10`) to trigger a violation. Then observe:

```
SW1# show port-security interface GigabitEthernet0/3
```

Confirm that the port remains **up**, traffic from PC3 is silently dropped, and **the violation counter does not increment**. Compare this behavior to restrict mode from Task 3.

---

## Task 5 — Configure Shutdown Violation Mode on Gi0/0 and Trigger a Violation

**Exam Topic: 5.7**

Gi0/0 connects to UNS1, which has both PC4 (legitimate) and ROGUE (unauthorized) connected to it. You will configure port security with a maximum of **1 MAC address** and **shutdown** violation mode. Because two devices share the same port through the hub, the second MAC address will trigger an err-disabled state.

1. On **SW1 Gi0/0**, enable port security.
2. Set the **maximum number of allowed MAC addresses** to **1**.
3. Set the violation mode to **shutdown**.

**Trigger the violation:**

4. Generate traffic from **PC4** first (e.g., `ping 10.1.1.10`). PC4's MAC address will be learned as the first secure MAC.
5. Generate traffic from **ROGUE** (e.g., `ping 10.1.1.10`). ROGUE's different MAC address will trigger the violation.

**Verification — run and observe the output:**
```
SW1# show port-security interface GigabitEthernet0/0
SW1# show interfaces GigabitEthernet0/0
SW1# show port-security
```

Confirm that Gi0/0 is now in **err-disabled** state. Note the **last violation MAC address** shown in the port security output. Understand the distinction between the port's line protocol status and the err-disabled cause.

---

## Task 6 — Configure and Test Errdisable Recovery on Gi0/0

**Exam Topic: 5.7**

After a port enters err-disabled state due to a port security violation, it will not recover automatically unless errdisable recovery is configured. In this task you will configure automatic errdisable recovery specifically for port security violations, set a recovery interval, and observe the port come back up.

1. On **SW1**, enable errdisable recovery for the **psecure-violation** cause.
2. Set the errdisable recovery interval to **30 seconds**.
3. Verify the errdisable recovery configuration.

**Verification — run and observe the output:**
```
SW1# show errdisable recovery
```

4. Wait for the recovery interval to elapse. Observe Gi0/0 return to the **connected** state.
5. After recovery, immediately generate traffic from only **PC4** to re-establish the secure MAC before ROGUE sends any frames.

**Verification after recovery:**
```
SW1# show interfaces GigabitEthernet0/0
SW1# show port-security interface GigabitEthernet0/0
```

Confirm the port is back **up/up** and the secure MAC count is back to 1.

---

## Task 7 — Secure All Unused Ports on SW1

**Exam Topic: 5.7**

Best practice requires that all unused switch ports be administratively disabled to prevent unauthorized devices from connecting to the network.

1. On **SW1**, identify all interfaces that are not currently in use (no active links to PC or hub).
2. Administratively **shut down** all unused ports.
3. Verify the ports are in a down state.

**Verification — run and observe the output:**
```
SW1# show interfaces status
SW1# show running-config | section interface GigabitEthernet
```

Confirm that all unused ports show as **disabled** (administratively down) in the output.

---

## Task 8 — Full Verification Using All Show Commands

**Exam Topic: 5.7**

Run all port security verification commands on SW1 and interpret the output for each active port. You must be able to read and explain every field in the output.

Run each of the following commands and observe the results:

```
SW1# show port-security
SW1# show port-security address
SW1# show port-security interface GigabitEthernet0/0
SW1# show port-security interface GigabitEthernet0/1
SW1# show port-security interface GigabitEthernet0/2
SW1# show port-security interface GigabitEthernet0/3
SW1# show mac address-table
SW1# show interfaces status
SW1# show errdisable recovery
```

For each port, confirm and document:

- Port security is enabled
- Maximum MAC addresses configured
- Current learned MAC count
- Violation mode configured
- Violation count
- Last violation MAC address (where applicable)
- Port status (connected, err-disabled, disabled)

---

## Grading Rubric

| Task | Points | Topic |
|------|--------|-------|
| Task 1 | 5  | Access mode verification on all ports |
| Task 2 | 15 | Sticky MAC + shutdown mode on Gi0/1 |
| Task 3 | 15 | Static MAC + restrict mode on Gi0/2 |
| Task 4 | 15 | Static MAC + protect mode on Gi0/3 |
| Task 5 | 20 | Shutdown mode on Gi0/0 + violation triggered |
| Task 6 | 15 | Errdisable recovery configured and tested |
| Task 7 | 5  | Unused ports shut down |
| Task 8 | 10 | Full show command verification |
| **Total** | **100** | |

### Scoring

| Score | Result |
|-------|--------|
| 90–100 | ✅ Excellent — Exam ready on this topic |
| 75–89  | ⚠️ Good — Minor gaps; review missed items |
| Below 75 | ❌ Needs review — Re-read Ch11 Vol2 and retry |

---

## Port Security Quick Reference

### Violation Mode Comparison

| Mode     | Port Status After Violation | Traffic Dropped | Violation Counter | Syslog/SNMP Alert |
|----------|-----------------------------|-----------------|-------------------|-------------------|
| Protect  | Remains up                  | Yes             | No increment      | No                |
| Restrict | Remains up                  | Yes             | Increments        | Yes               |
| Shutdown | Err-disabled (down)         | Yes             | Increments        | Yes               |

### Key Commands Reference (Ch11 Vol2 Table 11-5 / 11-6)

| Command | Purpose |
|---------|---------|
| `switchport port-security` | Enable port security (required first) |
| `switchport port-security maximum <n>` | Set max allowed MAC addresses |
| `switchport port-security mac-address <mac>` | Configure static secure MAC |
| `switchport port-security mac-address sticky` | Enable sticky MAC learning |
| `switchport port-security violation {protect\|restrict\|shutdown}` | Set violation mode |
| `show port-security` | Summary of all port security ports |
| `show port-security address` | All secure MAC addresses |
| `show port-security interface <if>` | Detail for a specific port |
| `errdisable recovery cause psecure-violation` | Enable auto-recovery for port security |
| `errdisable recovery interval <seconds>` | Set recovery timer |
| `show errdisable recovery` | View recovery settings and timers |
