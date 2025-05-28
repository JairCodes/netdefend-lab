# NetDefend Lab

**NetDefend Lab** is a hands-on hybrid IT/security laboratory that leverages enterprise Cisco hardware alongside open-source tools to simulate, attack, and defend a segmented network environment.

---

## Architecture

- **VLAN 10 “INTERNAL”** – Blue Team (Laptop/Defender)  
- **VLAN 20 “GUEST”** – Red Team (Raspberry Pi/Attacker)  
- **Trunk (Gi0/1):** Carries VLAN 10 & 20 between switch and router  

**Router-on-a-Stick** (Cisco 1841 Fa0/1):  
- Sub-interface `.10` → IP `192.168.10.1/24` (VLAN 10)  
- Sub-interface `.20` → IP `192.168.20.1/24` (VLAN 20)  

**Switch** (Cisco 2960):  
- `Fa0/1` → VLAN 10 (Laptop/Defender)  
- `Fa0/2` → VLAN 20 (Raspberry Pi/Attacker)

## Prerequisites

- **Harware:**
  - Cisco 2960 switch
  - Cisco 1841 router
  - Raspberry Pi (with Rasploit)
  - Laptop/PC (as Defender)
- **Software/Tools**
  - PuTTY or equivalent SSH/console client
  - Wireshark for traffic capture and analysis
  - Rasploit framework on Raspberry Pi
  - A central log collection system for ingesting and analyzing device logs
