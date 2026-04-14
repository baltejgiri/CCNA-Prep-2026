# Lab-10: Securing Network Devices — Task Guide

**CCNA 200-301 v1.1 Exam Topics:**
- 1.1.c — Explain the role and function of Next-Generation Firewalls and IPS
- 4.8 — Configure network devices for remote access using SSH
- 5.3 — Configure and verify device access control using local passwords

**CML Lab ID:** `e4304fec-0bb5-4eee-9e95-9814aecf5cdd`

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

| Device    | Interface                    | IP Address      | Role                              |
|-----------|------------------------------|-----------------|-----------------------------------|
| Admin-PC  | eth0                         | 10.1.1.100/24   | Admin workstation / SSH client    |
| R1        | GigabitEthernet0/1           | 10.1.1.1/24     | Practice router – SSH & passwords |
| R2        | GigabitEthernet0/1           | 10.1.1.2/24     | Practice router – SSH & passwords |
| ASA1      | GigabitEthernet0/0 (inside)  | 10.1.1.254/24   | Next-Generation Firewall          |
| ASA1      | GigabitEthernet0/1 (outside) | 172.16.1.1/24   | Next-Generation Firewall          |
| OUTSIDE-R | GigabitEthernet0/1           | 172.16.1.2/24   | Simulated external host           |

---

## Lab Base State

All nodes are pre-configured with IP addresses and interfaces are UP. No password security or SSH has been configured — complete all tasks below.

---

## Task 1 — Configure Enable Secret and Encrypted Passwords on R1

**Exam Topic: 5.3**

On **R1**, complete the following:

1. Configure a privileged EXEC mode **secret** password. Use a strong password of your choice.
2. Enable the IOS global feature that encrypts all clear-text passwords stored in the running configuration.
3. Configure the **console line** to require a password for access and enable login.
4. Configure **VTY lines 0–4** to require a password for access and enable login.

**Verification — run and observe the output:**
```
R1# show running-config | include enable
R1# show running-config | section line con
R1# show running-config | section line vty
```

---

## Task 2 — Configure Local User Accounts on R1 and R2

**Exam Topic: 5.3**

On **both R1 and R2**, complete the following:

1. Create a local user account with a username of your choice, **privilege level 15**, and a **secret** password.
2. Configure VTY lines 0–4 to authenticate using the **local user database** instead of the line password.
3. Remove the line password from the VTY lines since it is no longer needed.

**Verification — run and observe the output:**
```
R1# show running-config | section username
R1# show running-config | section line vty
R2# show running-config | section username
R2# show running-config | section line vty
```

---

## Task 3 — Configure SSH Version 2 on R1

**Exam Topic: 4.8**

On **R1**, complete the following:

1. Confirm the hostname is set to `R1` and the domain name `ccna.lab` is configured.
2. Generate an RSA key pair using a modulus of **2048 bits**.
3. Set the SSH version to **version 2** only.
4. Set the SSH idle timeout to **60 seconds**.
5. Set the maximum SSH authentication retries to **3**.
6. Configure VTY lines 0–4 to accept **SSH connections only** — block Telnet.

**Verification — run and observe the output:**
```
R1# show ip ssh
R1# show running-config | section line vty
R1# show running-config | include transport
```

---

## Task 4 — Configure SSH Version 2 on R2

**Exam Topic: 4.8**

Repeat all steps from Task 3 on **R2**.

**Verification — run and observe the output:**
```
R2# show ip ssh
R2# show running-config | section line vty
```

---

## Task 5 — Verify and Test SSH Connectivity

**Exam Topic: 4.8**

1. From **Admin-PC**, open a terminal and SSH into R1 using the local username created in Task 2. Confirm you reach R1's privileged EXEC prompt.
2. From **Admin-PC**, SSH into R2 and confirm access.
3. From **OUTSIDE-R**, attempt to SSH into R1. Observe and explain what happens and why.
4. On **R1**, verify active SSH sessions and confirm the SSH version in use.

**Verification — run and observe the output:**
```
R1# show ssh
R1# show users
R1# show ip ssh
```

---

## Task 6 — Explain the Role and Function of NGFW and IPS

**Exam Topic: 1.1.c** *(Knowledge task — no CLI required)*

Answer the following questions in your own words:

1. What is the difference between a traditional **stateful firewall** and a **Next-Generation Firewall (NGFW)**? What additional capabilities does an NGFW provide?

2. What is the difference between an **IDS** and an **IPS**? Which one actively blocks traffic?

3. Where is a **firewall or NGFW** typically placed in a network topology?

4. What is the name of Cisco's **NGFW platform**? What two technologies does it combine?

5. What does **stateful inspection** mean? How does it differ from a simple ACL?

6. Based on the topology in this lab, what security function is **ASA1** performing and why?

---

## Grading Rubric

| Task | Points | Topic |
|------|--------|-------|
| Task 1 | 15 | Enable secret, encrypted passwords, console/VTY |
| Task 2 | 20 | Local user database, login local on R1 and R2 |
| Task 3 | 20 | SSH v2, RSA 2048, timeout, retries, transport on R1 |
| Task 4 | 20 | SSH v2 on R2 |
| Task 5 | 15 | SSH connectivity verification |
| Task 6 | 10 | NGFW and IPS conceptual knowledge |
| **Total** | **100** | |
