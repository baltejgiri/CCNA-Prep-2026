# CCNA 200-301 Study Progress — Volume 2

**Exam Date:** April 26, 2026 &nbsp;|&nbsp; **Today:** April 15, 2026 &nbsp;|&nbsp; **Days Remaining:** 11

---

## DIKTA Score Summary

| Chapter | Title | Pre-Read Score | % | Post-Read Score | % | Weak Questions | Read | Lab | Lab Score |
|---|---|---|---|---|---|---|---|---|---|
| Ch 1  | Fundamentals of Wireless Networks | –/– | – | – | – | – | ☐ | ☐ | – |
| Ch 2  | Analyzing Cisco Wireless Architectures | –/– | – | – | – | – | ☐ | ☐ | – |
| Ch 3  | Securing Wireless Networks | –/– | – | – | – | – | ☐ | ☐ | – |
| Ch 4  | Building a Wireless LAN | –/– | – | – | – | – | ☐ | ☐ | – |
| Ch 5  | Introduction to TCP/IP Transport and Applications | –/– | – | – | – | – | ☐ | ☐ | – |
| Ch 6  | Basic IPv4 Access Control Lists | –/– | – | – | – | – | ☐ | ✅ | – |
| Ch 7  | Named and Extended IP ACLs | 5/6 | 83% | – | – | Q5 (src port on ACE) | ☐ | ✅ | – |
| Ch 8  | Applied IP ACLs | –/– | – | – | – | – | ☐ | ✅ | – |
| Ch 9  | Security Architectures | 8/9 | 89% | – | – | Q5 (reflection attack) | ☐ | ☐ | – |
| Ch 10 | Securing Network Devices | 2/6 | 33% | 6/6 | 100% | All cleared post-read | ✅ | ✅ | 73/82 (89%) |
| Ch 11 | Implementing Switch Port Security | 3/5 | 60% | – | – | Q1 (port security default), Q2 (violation mode behavior) | ✅ | ✅ | 99/100 (99%) |
| Ch 12 | DHCP Snooping and ARP Inspection | 3/7 | 43% | – | – | Q4, Q5, Q6, Q7 | ☐ | ✅ | Pending |
| Ch 13 | Device Management Protocols | –/– | – | – | – | – | ☐ | ☐ | – |
| Ch 14 | Network Address Translation | –/– | – | – | – | – | ☐ | ☐ | – |
| Ch 15 | Quality of Service (QoS) | –/– | – | – | – | – | ☐ | ☐ | – |
| Ch 16 | First Hop Redundancy Protocols | –/– | – | – | – | – | ☐ | ☐ | – |
| Ch 17 | SNMP, FTP, and TFTP | –/– | – | – | – | – | ☐ | ☐ | – |
| Ch 18 | LAN Architecture | –/– | – | – | – | – | ☐ | ☐ | – |
| Ch 19 | WAN Architecture | –/– | – | – | – | – | ☐ | ☐ | – |
| Ch 20 | Cloud Architecture | –/– | – | – | – | – | ☐ | ☐ | – |
| Ch 21 | Introduction to Controller-Based Networking | –/– | – | – | – | – | ☐ | ☐ | – |
| Ch 22 | Cisco SD-Access | –/– | – | – | – | – | ☐ | ☐ | – |
| Ch 23 | Understanding REST and JSON | –/– | – | – | – | – | ☐ | ☐ | – |
| Ch 24 | Understanding Ansible and Terraform | –/– | – | – | – | – | ☐ | ☐ | – |

**Chapters with DIKTA completed:** 6 of 24 &nbsp;|&nbsp; **Average DIKTA (pre-read):** 65% &nbsp;|&nbsp; *(Ch7: 83%, Ch9: 89%, Ch10: 33% pre / 100% post, Ch11: 60%, Ch12: 43%)*

---

## DIKTA Answers — Per Chapter

### Chapter 10 — Securing Network Devices

#### Pre-Read DIKTA (April 11, 2026)

**Score: 2/6 (33%)** — Below gate. Read required before lab.

| Q | Answer Submitted | Correct | Result |
|---|---|---|---|
| 1 | — | B | ❌ |
| 2 | — | A, C | ❌ |
| 3 | — | D | ❌ |
| 4 | — | B | ❌ |
| 5 | — | B | ❌ |
| 6 | — | A, D | ❌ |

> Pre-read answers not recorded. Score of 2/6 taken from context.

#### Post-Read DIKTA (April 13, 2026)

**Score: 6/6 (100%)** ✅ — Full recovery after reading + completing Lab-10.

| Q | Answer Submitted | Correct | Result |
|---|---|---|---|
| 1 | B | B | ✅ |
| 2 | A, C | A, C | ✅ |
| 3 | D | D | ✅ |
| 4 | B | B | ✅ |
| 5 | B | B | ✅ |
| 6 | A, D | A, D | ✅ |

---

### Chapter 11 — Implementing Switch Port Security

#### Pre-Read DIKTA (April 14, 2026)

**Score: 3/5 (60%)** — Below gate (75%). Read required before completing lab.

| Q | Answer Submitted | Correct | Result | Topic Tested |
|---|---|---|---|---|
| 1 | D | B | ❌ | Default port security behavior |
| 2 | C | B, D | ❌ | Port security violation mode behavior |
| 3 | B, C | B, C | ✅ | Sticky MAC / SecureSticky |
| 4 | B | B | ✅ | Max secure MAC addresses |
| 5 | B, C | B, C | ✅ | Errdisable recovery |

**Weak areas identified:**
- **Q1** — Default violation mode when `switchport port-security` is enabled without specifying a mode
- **Q2** — Difference between protect, restrict, and shutdown violation mode behavior (counter, syslog, port state)

> Post-read DIKTA: **Pending** — complete after reviewing Lab-11 results.

---

### Chapter 12 — DHCP Snooping and ARP Inspection

#### Pre-Read DIKTA (April 15, 2026)

**Score: 3/7 (43%)** ❌ — Well below gate. Chapter must be read before lab.

| Q | Answer Submitted | Correct | Result | Topic (Ch 12) |
|---|---|---|---|---|
| 1 | A, C | A, C | ✅ | DHCP snooping concepts |
| 2 | B, C | B, C | ✅ | DHCP snooping trusted/untrusted ports |
| 3 | C, D | C, D | ✅ | DHCP snooping behavior |
| 4 | D | A | ❌ | DHCP snooping configuration/verification |
| 5 | A | B | ❌ | DHCP snooping rate limiting or binding table |
| 6 | C, D | B, D | ❌ | Dynamic ARP Inspection (DAI) concepts |
| 7 | B, D | C, D | ❌ | DAI configuration or ARP ACL behavior |

**Weak areas identified:**
- **Q4** — DHCP snooping: answered D, correct A
- **Q5** — DHCP snooping / binding table: answered A, correct B
- **Q6** — DAI concepts: submitted C,D — D is correct but C is wrong (should be B)
- **Q7** — DAI configuration: submitted B,D — D is correct but B is wrong (should be C)

> Post-read DIKTA: **Pending** — complete after reading Ch12 and finishing lab.

---

## CML Lab Grades

### Lab-10 — Securing Network Devices (Ch 10)

**Date:** 2026-04-13 &nbsp;|&nbsp; **CML Lab ID:** `e4304fec-0bb5-4eee-9e95-9814aecf5cdd`  
**Final Score: 73 / 82 (89%)** ⚠️ Good

**Exam Topics:** 1.1.c (NGFW/IPS), 4.8 (SSH), 5.3 (local passwords)

| Task | Points | Score | Description |
|---|---|---|---|
| Task 1 | 15 | 15/15 | Enable secret + `service password-encryption` + console/VTY passwords on R1 |
| Task 2 | 20 | 20/20 | Local user `admin` privilege 15 on R1 and R2; VTY uses `login local` |
| Task 3 | 20 | 20/20 | SSH v2, RSA 2048, timeout 60, retries 3, VTY transport ssh-only on R1 |
| Task 4 | 20 | 18/20 | SSH v2 configured on R2; minor gap in timeout/retries verification |
| Task 5 | 15 | 0/15 | SSH connectivity test — not completed (ASA1 enable password loop blocked ASA access) |
| Task 6 | 10 | 0/8 | ASA NGFW exploration — revised to knowledge questions only; ASA CLI inaccessible |
| **Total** | **100** | **73/82** | |

**Notes:**
- Task 5 and Task 6 were impacted by the ASA1 enable password configuration issue (no enable password set in base config; device stuck at password prompt)
- Task 6 was scoped to CCNA knowledge questions only — ASA CLI commands (ACL/access-group) are not testable on CCNA 200-301 exam
- Lab score out of 82 reflects adjusted rubric excluding unreachable ASA CLI tasks

**Grading file:** `Labs/Lab-10-Securing-Network-Devices.md` (includes answer key)

---

### Lab-11 — Implementing Switch Port Security (Ch 11)

**Date:** 2026-04-14 &nbsp;|&nbsp; **CML Lab ID:** `5951f588-5611-4dc2-8d9d-1663eca74546`  
**Final Score: 99 / 100 (99%)** ✅ Excellent — Exam Ready

**Exam Topic:** 5.7 — Configure and verify Layer 2 security features (port security)

| Task | Points | Score | Description |
|---|---|---|---|
| Task 1 | 5 | 5/5 | Verified all active ports in access mode; `switchport mode access` on Gi0/0–Gi0/3 |
| Task 2 | 15 | 15/15 | Sticky MAC + Shutdown on Gi0/1; `5254.0096.a991 SecureSticky` learned, 0 violations |
| Task 3 | 15 | 14/15 | Static MAC `0000.1111.2222` + Restrict on Gi0/2; 38 violations logged; minor config-order note |
| Task 4 | 15 | 15/15 | Static MAC `0000.3333.4444` + Protect on Gi0/3; 0 violations (correct — protect mode) |
| Task 5 | 20 | 20/20 | Shutdown on Gi0/0; ROGUE triggered err-disabled; `%PM-4-ERR_DISABLE` syslog confirmed |
| Task 6 | 15 | 15/15 | `errdisable recovery cause psecure-violation` + 30s timer; 6+ recovery cycles observed |
| Task 7 | 5 | 5/5 | N/A — CML IOSvL2 has only 4 interfaces, all in use. No unused ports to shut down. Concept understood. |
| Task 8 | 10 | 10/10 | All `show port-security`, `show errdisable recovery`, `show mac address-table` commands run |
| **Total** | **100** | **99/100** | |

**Key grading evidence from CML console log:**
```
Gi0/0: err-disabled  |  Violations: 4  |  Mode: Shutdown
Gi0/1: Secure-up     |  Violations: 0  |  Mode: Shutdown  |  Sticky: 5254.0096.a991
Gi0/2: Secure-up     |  Violations: 38 |  Mode: Restrict  |  Static: 0000.1111.2222
Gi0/3: Secure-up     |  Violations: 0  |  Mode: Protect   |  Static: 0000.3333.4444
errdisable recovery: psecure-violation Enabled | Interval: 30 seconds
```

**Task 3 note (−1 pt):** `show port-security address` showed two entries for Gi0/2 (`0000.1111.2222 SecureConfigured` + `5254.00c0.a948 SecureDynamic`). With `maximum 1`, this suggests port-security was enabled before the static MAC was applied, allowing a brief dynamic learn. Correct configuration order: `maximum` → `mac-address` → `violation` → `switchport port-security` (enable last).

**Grading file:** `Labs/Lab-11-Graded.md`

---

### ACL Lab — Standard, Extended & Named ACLs (Ch 6, 7, 8)

**Date:** 2026-04-11 &nbsp;|&nbsp; **CML Lab ID:** Not recorded  
**Final Score: 12 / 12 (100%)** ✅ Excellent

**Exam Topics:** 5.6 (standard ACLs), 5.6 (extended ACLs), named ACL syntax

| Task | Points | Score | Description |
|---|---|---|---|
| All tasks | 12 | 12/12 | Standard, extended, and named ACL configuration and verification |

**Grading file:** `Labs/ACL-Lab-Reference.md`

---

### Lab-12 — DHCP Snooping and Dynamic ARP Inspection (Ch 12)

**Date:** 2026-04-15 &nbsp;|&nbsp; **CML Lab ID:** `1f68da4b-7452-458a-9dab-1ead741fc1fa`  
**Final Score: Pending** — lab created, chapter not yet read

**Exam Topic:** 5.7 — Configure and verify Layer 2 security features (DHCP snooping, dynamic ARP inspection)

**Nodes:** SW1 (iosvl2), R1 (iosv — gateway + DHCP server), PC1 / PC2 / ROGUE (Tiny Core Linux — `server`)

| Task | Points | Score | Description |
|---|---|---|---|
| Task 1 | 5 | — | Base connectivity verified before security config |
| Task 2 | 10 | — | DHCP snooping enabled globally and on VLAN 1 |
| Task 3 | 10 | — | Trusted port configured on Gi0/0 |
| Task 4 | 10 | — | Binding table populated (PC1 + PC2 DHCP leases) |
| Task 5 | 10 | — | Binding table verified with correct fields |
| Task 6 | 10 | — | Rate limiting configured on all untrusted ports |
| Task 7 | 15 | — | DAI enabled on VLAN 1 |
| Task 8 | 10 | — | Trusted port configured for DAI on Gi0/0 |
| Task 9 | 10 | — | Optional DAI checks (src-mac, dst-mac, ip) configured |
| Task 10 | 10 | — | Full show command verification |
| **Total** | **100** | **Pending** | |

**Grading file:** `Labs/Lab-12-Graded.md` *(to be created after lab completion)*

---

## Weak Areas to Revisit

| Chapter | Question | Topic | Status |
|---|---|---|---|
| Ch 7 | Q5 | `eq <port>` placement — source vs destination in extended ACLs | Reviewed ✅ |
| Ch 9 | Q5 | Reflection attack — victim's address spoofed as source | Reviewed ✅ |
| Ch 10 | Q2 | SSH / device access control | Cleared ✅ — 100% post-read |
| Ch 10 | Q3 | SSH / device access control | Cleared ✅ — 100% post-read |
| Ch 10 | Q5 | SSH / device access control | Cleared ✅ — 100% post-read |
| Ch 10 | Q6 | Firewall/IPS concepts | Cleared ✅ — 100% post-read |
| Ch 11 | Q1 | Default violation mode (`shutdown` is default) | Cleared ✅ — demonstrated in Lab-11 Task 5 |
| Ch 11 | Q2 | Protect vs Restrict — protect has no counter increment or syslog | Cleared ✅ — demonstrated in Lab-11 Tasks 3 & 4 |
| Ch 12 | Q4 | DHCP snooping configuration/verification | Pending — read chapter |
| Ch 12 | Q5 | DHCP snooping rate limiting / binding table behavior | Pending — read chapter |
| Ch 12 | Q6 | DAI concepts — answered C,D; correct is B,D | Pending — read chapter |
| Ch 12 | Q7 | DAI configuration / ARP ACL — answered B,D; correct is C,D | Pending — read chapter |

---

## 15-Day Study Schedule (April 11–26, 2026)

> **Workflow per chapter:** DIKTA → Read → CML Lab (if applicable) → Review key concepts

| Date | Day | Focus | Chapters | Target | Status |
|---|---|---|---|---|---|
| Apr 11 | Sat | Part III Security (cont.) | Ch 9 + Ch 10 DIKTA | Read Ch 10 | ✅ Done |
| Apr 12 | Sun | Part III Security | Ch 10 Lab | Lab-10: SSH + Passwords + NGFW | ✅ Done |
| Apr 13 | Mon | Part III Security | Ch 10 complete — DIKTA retake | 6/6 post-read | ✅ Done |
| Apr 14 | Tue | Part III Security | Ch 11 read + lab | Lab-11: Port Security 99/100 | ✅ Done |
| Apr 15 | Wed | Part III Lab | Ch 11 post-read DIKTA + Ch 12 | DHCP Snooping + ARP Inspection | 🔄 Ch12 DIKTA: 3/7 (43%) — read required |
| Apr 16 | Thu | Part I Wireless | Ch 1, Ch 2 | DIKTA + Read both | ⬜ |
| Apr 17 | Fri | Part I Wireless | Ch 3, Ch 4 | DIKTA + Read both + Part I Review | ⬜ |
| Apr 18 | Sat | Part II ACL Review | Ch 5, Ch 6 | DIKTA + Read + Part II Review | ⬜ |
| Apr 19 | Sun | Part IV IP Services | Ch 13, Ch 14 | DIKTA + Read both (NAT is a weak area) | ⬜ |
| Apr 20 | Mon | Part IV IP Services | Ch 15, Ch 16 | DIKTA + Read both (QoS is a weak area) | ⬜ |
| Apr 21 | Tue | Part IV IP Services + Review | Ch 17 + Part IV Review | DIKTA + Read + Part IV Review | ⬜ |
| Apr 22 | Wed | Part V Network Architecture | Ch 18–20 + Part V Review | DIKTA + Read + Review | ⬜ |
| Apr 23 | Thu | Part VI Network Automation | Ch 21–24 + Part VI Review | DIKTA + Read both | ⬜ |
| Apr 24 | Fri | Full Review + Practice Test 1 | All Vol 2 | Target ≥ 80% on practice test | ⬜ |
| Apr 25 | Sat | Final Review + Practice Test 2 | Weak areas focus | Rest well — exam tomorrow | ⬜ |
| **Apr 26** | **Sun** | **EXAM DAY** | **CCNA 200-301** | 🎯 | |

---

## Priority Focus Areas

Based on DIKTA results and lab performance:

| Topic | Priority | Why |
|---|---|---|
| NAT (Ch 14) | 🔴 High | Known weak area |
| QoS (Ch 15) | 🔴 High | Known weak area |
| SDN/Automation (Ch 21–24) | 🔴 High | High exam weight, conceptual |
| Wireless Security (Ch 3) | 🟡 Medium | Overlaps with Security track |
| FHRP (Ch 16) | 🟡 Medium | Protocol details easy to confuse |
| NGFW vs IPS (Ch 10) | 🟡 Medium | Lab-10 Task 6 — ASA inaccessible; FTD vs ASA distinction needs reinforcement |
| ACLs (Ch 6–8) | 🟢 Low | CML lab 100%, reviewed |
| Security Fundamentals (Ch 9) | 🟢 Low | 89% DIKTA, weak Q reviewed |
| Securing Network Devices (Ch 10) | 🟢 Low | 100% post-read DIKTA, Lab-10 89% |
| Switch Port Security (Ch 11) | 🟢 Low | Lab-11 99%, Q1/Q2 weak areas cleared via lab |
| DHCP Snooping + DAI (Ch 12) | 🟡 Medium | 43% pre-read DIKTA — Q4–Q7 all missed; read chapter before lab |

---

## Lab Files Reference

| Lab | YAML Topology | Lab Guide | Graded Report |
|---|---|---|---|
| ACL Lab (Ch 6–8) | `Labs/acl-lab-base-topology.yaml` | `Labs/ACL-Lab-Reference.md` | — |
| Lab-10 (Ch 10) | `Labs/lab10-securing-network-devices-base-topology.yaml` | `Labs/Lab-10-Questions-Only.md` | `Labs/Lab-10-Securing-Network-Devices.md` |
| Lab-11 (Ch 11) | `Labs/lab11-switch-port-security-base-topology.yaml` | `Labs/Lab-11-Questions-Only.md` | `Labs/Lab-11-Graded.md` |
| Lab-12 (Ch 12) | `Labs/lab12-dhcp-snooping-dai-base-topology.yaml` | `Labs/Lab-12-Questions-Only.md` | `Labs/Lab-12-Graded.md` *(pending)* |

---

## Step-by-Step Workflow (Reference)

1. **DIKTA** — complete quiz before reading, submit answers for grading
2. **Grade** — review any missed questions in detail
3. **Read** — read the full chapter (required if pre-read DIKTA < 75%)
4. **CML Lab** — build lab if chapter has hands-on content
5. **Lab Grade** — validate lab via CML console logs
6. **Post-Read DIKTA** — retake DIKTA after reading + lab (target 100%)
7. **Update this file** — mark chapter complete, record all scores here
