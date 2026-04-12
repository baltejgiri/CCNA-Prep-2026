# Lab-10: Securing Network Devices

**CCNA 200-301 v1.1 Exam Topics:**
- 1.1.c — Explain the role and function of Next-Generation Firewalls and IPS
- 4.8 — Configure network devices for remote access using SSH
- 5.3 — Configure and verify device access control using local passwords

**CML Lab ID:** `e4304fec-0bb5-4eee-9e95-9814aecf5cdd`
**Base Topology File:** `Labs/lab10-securing-network-devices-base-topology.yaml`
**Date Created:** 2026-04-12

---

## Topology

```
                       10.1.1.0/24 (Management / Inside)
  [Admin-PC]---eth0---[SW1-Mgmt]---G0/1---[R1]  10.1.1.1/24
  10.1.1.100/24           |
                         G0/2---[R2]  10.1.1.2/24
                          |
                         G0/3---[ASA1 inside G0/0]  10.1.1.254/24
                                 ASA1 outside G0/1  172.16.1.1/24
                                          |
                                     [OUTSIDE-R]    172.16.1.2/24
                                   172.16.1.0/24 (Outside / WAN)
```

| Device    | Interface          | IP Address       | Role                    |
|-----------|--------------------|------------------|-------------------------|
| Admin-PC  | eth0               | 10.1.1.100/24    | Admin workstation / SSH client |
| R1        | GigabitEthernet0/1 | 10.1.1.1/24      | Practice router – SSH & passwords |
| R2        | GigabitEthernet0/1 | 10.1.1.2/24      | Practice router – SSH & passwords |
| ASA1      | GigabitEthernet0/0 (inside)  | 10.1.1.254/24  | Next-Generation Firewall |
| ASA1      | GigabitEthernet0/1 (outside) | 172.16.1.1/24  | Next-Generation Firewall |
| OUTSIDE-R | GigabitEthernet0/1 | 172.16.1.2/24    | Simulated external host |

---

## Lab Base State

Before starting tasks, all nodes have:
- IPs pre-configured and interfaces UP
- **No** enable secret configured on R1 or R2
- **No** local username database
- **No** SSH configured on R1 or R2
- ASA1 has basic interface config (inside/outside) only

---

## Task 1 — Configure Enable Secret and Encrypted Passwords on R1

> **Exam Topic: 5.3** — Configure and verify device access control using local passwords

### Background

IOS devices support two methods of protecting privileged EXEC mode:
- `enable password` — stored in clear text (or encrypted type 7 with `service password-encryption`)
- `enable secret` — stored as a one-way MD5/SHA hash (always preferred)

When **both** are configured, IOS always uses `enable secret` and ignores `enable password`.

`service password-encryption` encrypts any clear-text passwords in the running config (console password, VTY password, `enable password`) using a reversible type-7 algorithm. It does **not** affect `enable secret`.

### Task Instructions

On **R1**, perform the following:

1. Configure an enable secret password of `Cisco!123`.
2. Enable `service password-encryption` globally.
3. Set the console line password to `con!cisco` and require login.
4. Set the VTY lines 0 through 4 password to `vty!cisco` and require login.

### Verification Commands

```
R1# show running-config | include enable
R1# show running-config | section line con
R1# show running-config | section line vty
```

**Expected results:**
- `enable secret 5 $1$...` (MD5 hash — never clear text)
- Console password shown as `password 7 <encrypted>` (type-7 encoding)
- VTY password shown as `password 7 <encrypted>` (type-7 encoding)

### Task 1 Answer Key

```
R1# configure terminal
R1(config)# enable secret Cisco!123
R1(config)# service password-encryption
R1(config)# line con 0
R1(config-line)# password con!cisco
R1(config-line)# login
R1(config-line)# exit
R1(config)# line vty 0 4
R1(config-line)# password vty!cisco
R1(config-line)# login
R1(config-line)# end
R1# write memory
```

---

## Task 2 — Configure Local User Accounts on R1 and R2

> **Exam Topic: 5.3** — Configure and verify device access control using local passwords

### Background

The local username database allows IOS to authenticate users with a username/password stored on the device itself (no RADIUS/TACACS+ server required). When VTY lines are configured with `login local`, IOS checks the local database instead of the line password.

`username <name> privilege <0-15> secret <password>` — privilege 15 grants immediate enable-mode access after login.

### Task Instructions

On **R1 and R2**, perform the following:

1. Create a local user account with username `admin`, privilege level `15`, and secret password `Admin!123`.
2. Configure VTY lines 0–4 to authenticate using the local database (`login local`).
3. Remove the VTY line password (it is no longer needed with `login local`).

### Verification Commands

```
R1# show running-config | section username
R1# show running-config | section line vty
```

**Expected results:**
- `username admin privilege 15 secret 5 $1$...` (hashed)
- `line vty 0 4` shows `login local` (not `login`)
- No `password` command under VTY lines

### Task 2 Answer Key

```
R1# configure terminal
R1(config)# username admin privilege 15 secret Admin!123
R1(config)# line vty 0 4
R1(config-line)# login local
R1(config-line)# no password
R1(config-line)# end
R1# write memory
```

Repeat on **R2** (same commands).

---

## Task 3 — Configure SSH Version 2 on R1

> **Exam Topic: 4.8** — Configure network devices for remote access using SSH

### Background

SSH (Secure Shell) encrypts all management traffic between the client and the router. Telnet sends all data — including passwords — in clear text and should never be used in production.

**SSH configuration requirements on IOS:**
1. Hostname must be set (not the default `Router`)
2. A domain name must be configured (`ip domain-name`)
3. RSA key pair must be generated (`crypto key generate rsa`)
4. SSH version 2 must be selected (`ip ssh version 2`)
5. VTY lines must be restricted to SSH only (`transport input ssh`)

### Task Instructions

On **R1**, perform the following:

1. Verify the hostname is `R1` (already set in base config).
2. Confirm the domain name `ccna.lab` is set (already in base config).
3. Generate RSA keys with a modulus of **2048 bits**.
4. Set SSH version to **2**.
5. Set the SSH idle timeout to **60 seconds**.
6. Set the SSH authentication retries to **3**.
7. Configure VTY lines 0–4 to accept **SSH only** (no Telnet).

### Verification Commands

```
R1# show ip ssh
R1# show running-config | section line vty
R1# show running-config | include transport
```

**Expected output from `show ip ssh`:**
```
SSH Enabled - version 2.0
Authentication timeout: 60 secs; Authentication retries: 3
...
```

### Task 3 Answer Key

```
R1# configure terminal
R1(config)# crypto key generate rsa modulus 2048
R1(config)# ip ssh version 2
R1(config)# ip ssh time-out 60
R1(config)# ip ssh authentication-retries 3
R1(config)# line vty 0 4
R1(config-line)# transport input ssh
R1(config-line)# end
R1# write memory
```

---

## Task 4 — Configure SSH Version 2 on R2

> **Exam Topic: 4.8** — Configure network devices for remote access using SSH

### Task Instructions

Repeat Task 3 on **R2** — configure RSA 2048-bit keys, SSH v2, timeout 60, retries 3, and restrict VTY to SSH only.

### Task 4 Answer Key

```
R2# configure terminal
R2(config)# crypto key generate rsa modulus 2048
R2(config)# ip ssh version 2
R2(config)# ip ssh time-out 60
R2(config)# ip ssh authentication-retries 3
R2(config)# line vty 0 4
R2(config-line)# transport input ssh
R2(config-line)# end
R2# write memory
```

---

## Task 5 — Verify and Test SSH Connectivity

> **Exam Topic: 4.8** — Configure network devices for remote access using SSH

### Background

SSH can be tested from:
- **Admin-PC** (Linux desktop) using the `ssh` command
- **Another IOS device** using `ssh -l <username> <ip>` or `ssh -v 2 -l <username> <ip>`

### Task Instructions

1. From **Admin-PC**, open a terminal and SSH into R1:
   ```
   ssh admin@10.1.1.1
   ```
   Enter password `Admin!123` when prompted. Confirm you reach R1's CLI.

2. From **Admin-PC**, SSH into R2:
   ```
   ssh admin@10.1.1.2
   ```

3. From **OUTSIDE-R**, SSH into R1 using IOS SSH client:
   ```
   OUTSIDE-R# ssh -l admin -v 2 10.1.1.1
   ```
   > Note: OUTSIDE-R cannot reach 10.1.1.1 directly (the ASA is in the path). This step confirms whether the ASA permits/blocks this. What do you observe?

4. On R1, verify active SSH sessions:
   ```
   R1# show ssh
   R1# show users
   ```

### Verification Commands

```
R1# show ip ssh
R1# show ssh
R1# show users
```

**Expected behavior:**
- SSH from Admin-PC to R1 and R2 succeeds (same subnet, no ASA in path)
- SSH from OUTSIDE-R to R1 will fail — ASA blocks it by default (no ACL/NAT configured for inbound)
- `show ssh` shows active session details (version, state, encryption)

---

## Task 6 — Explore ASA1 as a Next-Generation Firewall

> **Exam Topic: 1.1.c** — Explain the role and function of Next-Generation Firewalls and IPS

### Background

**Traditional (Stateless) Firewall** — permits or denies packets based only on static ACL rules (source IP, dest IP, port). Does not track connection state.

**Stateful Firewall** — tracks the state of active connections in a connection table. Return traffic for established sessions is automatically permitted without a separate inbound ACL rule.

**Next-Generation Firewall (NGFW)** — includes all stateful firewall features PLUS:
- **Application Identification** — recognizes Layer 7 apps (YouTube, Dropbox, etc.) regardless of port
- **Intrusion Prevention System (IPS)** — inspects packet payloads for known attack signatures
- **User Identity Awareness** — ties policies to usernames, not just IPs
- **URL/Content Filtering** — blocks categories of websites
- **TLS/SSL Inspection** — decrypts HTTPS traffic for inspection
- **Advanced Malware Protection (AMP)**

The Cisco ASAv in this lab is a stateful firewall. The Cisco Firepower Threat Defense (FTD) is Cisco's full NGFW platform combining ASA firewall + Firepower IPS.

**ASA Security Levels:**
- `security-level 100` = most trusted (inside)
- `security-level 0` = least trusted (outside)
- By default: higher security level → lower security level traffic is permitted; the reverse requires an explicit ACL

### Task Instructions

1. On **ASA1**, verify interface status and security levels:
   ```
   ASA1# show interface ip brief
   ASA1# show nameif
   ```

2. From **Admin-PC**, ping **OUTSIDE-R** (172.16.1.2):
   ```
   ping 172.16.1.2
   ```
   Observe whether the ping succeeds. Why or why not?

3. On **ASA1**, check the connection table after pinging:
   ```
   ASA1# show conn
   ASA1# show conn count
   ```

4. From **OUTSIDE-R**, ping **Admin-PC** (10.1.1.100):
   ```
   OUTSIDE-R# ping 10.1.1.100
   ```
   Does this succeed? Explain why not (default ASA stateful behavior).

5. Configure ASA1 to permit ICMP from outside to inside (exam scenario — normally you would NOT permit this in production):
   ```
   ASA1(config)# access-list OUTSIDE-IN extended permit icmp any any
   ASA1(config)# access-group OUTSIDE-IN in interface outside
   ```
   Re-test the ping from OUTSIDE-R. Does it now work?

6. On **ASA1**, view platform/IPS information:
   ```
   ASA1# show version
   ASA1# show threat-detection statistics
   ```

### Key Concepts to Understand for the Exam

| Concept | ASA (Traditional/Stateful) | FTD/NGFW |
|---------|---------------------------|-----------|
| Stateful inspection | ✅ | ✅ |
| ACL-based rules | ✅ | ✅ |
| Application awareness (Layer 7) | ❌ | ✅ |
| IPS/IDS engine | Limited (basic IPS module) | ✅ Full Snort |
| URL filtering | ❌ | ✅ |
| SSL/TLS inspection | ❌ | ✅ |
| Management | ASDM / CLI | FMC / FDM |

**IPS vs IDS:**
- **IDS (Intrusion Detection System)** — passively monitors and alerts; does not block traffic
- **IPS (Intrusion Prevention System)** — inline; actively blocks malicious traffic

---

## Lab Grading Rubric

| Task | Points | Description |
|------|--------|-------------|
| Task 1 | 15 | Enable secret configured, service password-encryption on, console/VTY passwords encrypted |
| Task 2 | 20 | Local user `admin` privilege 15 on R1 and R2; VTY uses `login local` |
| Task 3 | 20 | SSH v2, RSA 2048, timeout 60, retries 3, VTY transport ssh-only on R1 |
| Task 4 | 20 | Same as Task 3 on R2 |
| Task 5 | 15 | SSH from Admin-PC to R1 and R2 verified; `show ssh` output correct |
| Task 6 | 10 | ASA stateful behavior demonstrated; NGFW vs traditional firewall explained |
| **Total** | **100** | |

### Scoring

| Score | Result |
|-------|--------|
| 90–100 | ✅ Excellent — Exam ready on this topic |
| 75–89  | ⚠️ Good — Minor gaps; review missed items |
| Below 75 | ❌ Needs review — Re-read Ch10 Vol2 and retry |

---

## Quick Reference

### IOS Password Types

| Command | Storage | Reversible? | Use? |
|---------|---------|-------------|------|
| `enable password` | Clear / Type-7 | Yes (type-7) | ❌ Never |
| `enable secret` | MD5/SHA hash | No | ✅ Always |
| `service password-encryption` | Type-7 on other passwords | Yes | ✅ Yes (as extra layer) |
| `username … secret` | MD5/SHA hash | No | ✅ Always |
| `username … password` | Clear / Type-7 | Yes (type-7) | ❌ Avoid |

### SSH Troubleshooting Checklist

```
✅ hostname <not Router>
✅ ip domain-name <configured>
✅ crypto key generate rsa modulus 2048
✅ ip ssh version 2
✅ line vty 0 4 → login local → transport input ssh
✅ username configured in local database
```

### ASA Security Level Rules (Default Behavior)

```
Higher security → Lower security : PERMITTED (inside → outside by default)
Lower security → Higher security : DENIED (outside → inside requires ACL)
Same security level             : DENIED (requires same-security-traffic permit)
```
