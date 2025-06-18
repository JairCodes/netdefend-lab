# Switch-2960-Setup

## Purpose

This guide walks you through the physical connection, console access, and core configuration steps required to prepare a Cisco 2960 switch for the NetDefend environment.

## Physical Connection
1. Connect a Cisco console cable (RJ-45 to USB adapter) from your PC/Laptop to the **Console** port on the 2960
2. Power on the switch; observe LEDs next to the console port
3. Proceed to your Device Manager and check for:
    - Serial Line (e.g., COM1, COM2, COM3)
    - Bits per second (Speed)
    - Parity
    - Data bits
    - Stop bits
    - Flow control
      
![image](https://github.com/user-attachments/assets/6afebea3-ffb4-4213-ae82-39a36de4db8f) <br>

## Console Access (PuTTY)

1. Launch PuTTY (or your client of choice)
2. Under 'Connection type:' select 'Serial'
    - **COM Port:** e.g., 'COM3'
    - **Speed:** '9600' bits per second
4. Click 'Open' to start the console session

![image](https://github.com/user-attachments/assets/c575408a-bebc-4ae1-b71a-e1859b5e9477)

> Note: In PuTTY under 'SSH' in 'Serial' you can manually configure the serial line

---

## Initial Configuration

Once you’ve connected to the 2960’s console port and opened PuTTY (or your terminal client), perform the following steps:

### 1. Skip the System Configuration Dialog <br>
When the router boots up, you’ll see the “System Configuration Dialog” prompt and asked: <br>
```cmd
Would you like to enter the initial configuration dialog? [yes/no]:
```
**Action**: Type `no` and press Enter. <br>
```cmd
Switch> no
Switch>
``` 
    
![image](https://github.com/user-attachments/assets/dfe8cb9e-77a4-4e7e-96ad-da8f6bd36973)

### 2. Enter Global Configuration Mode
Once you have skipped the initial dialog and returned to the `Switch>` prompt, you need to elevate your privileges so you can make configuration changes:

1. **Enter Privileged EXEC Mode**  
   At the `Switch>` prompt, type:
   ```cmd
   Switch> enable
   ```
   - You should now see Switch#, indicating you have full administrative rights (also known as “privileged EXEC” mode).
2. **Enter Global Configuration Mode**
   From the `Switch#` prompt, enter:
   ```cmd
   Switch# configure terminal 
   ```
   - The prompt changes to Switch(config)#, which is global configuration mode.
   - In this mode, any commands you enter affect the device’s running configuration.
   
![image](https://github.com/user-attachments/assets/68da6ca5-2ae3-4d1c-bae7-9ecad06c6fcf)

### 3. Configure VLAN Database
1. **Enter VLAN Configuration Mode**
   From the `Switch(config)#` prompt, enter:
    ```cmd
    Switch(config)# vlan 10
    ```
    - This command creates (or enters) VLAN 10 in the VLAN database.
    - The prompt changes to Switch(config-vlan)#, showing you are editing VLAN 10’s properties.
2. **Name VLAN 10**
   Still at `Switch(config-vlan)#`, enter:
   ```cmd
   Switch(config-vlan)# name INTERNAL
   ```
3. **Exit Back to Global Config**
   ```cmd
   Switch(config-vlan)# exit
   Switch(config)#
   ```
   - Typing exit returns you from VLAN configuration mode back to global configuration mode (`Switch(config)#`). 

4. **Repeat for VLAN 20**
At `Switch(config)#`, add VLAN 20:
   ```cmd
   Switch(config)# vlan 20
   Switch(config-vlan)# name GUEST
   Switch(config-vlan)# exit
   Switch(config)#
   ```
   
![image](https://github.com/user-attachments/assets/5aefc7f0-2dbb-472d-ac4c-59fe49b0aacf)

### 4. Assign Access Ports to VLANs
From global configuration mode, assign switchports to VLANs:
**Assign Fa0/1 to VLAN 10 (e.g., Laptop)**
```cmd
Switch(config)# interface range FastEthernet0/1
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10
Switch(config-if-range)# exit
```
**Assign Fa0/2 to VLAN 20** (e.g., Raspberry Pi)
```cmd
Switch(config)# interface range FastEthernet0/2
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 20
Switch(config-if-range)# exit
```
Use the following command to verify that interfaces are assigned correctly:
```cmd
Switch# show vlan brief
```
You should see `Fa0/1` under VLAN 10 and `Fa0/2` under VLAN 20.

### 5. Configure Trunk Port to Router
To enable inter-VLAN routing between VLAN 10 and VLAN 20 via an external router (e.g., Cisco 1841), configure the trunk:
```cmd
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan 10,20
Switch(config-if)# no shutdown
Switch(config-if)# exit
Switch(config)# end
```
Save your configuration:
```cmd
Switch# write memory
```
Expected console confirmation: <br>
![image](https://github.com/user-attachments/assets/dfae932d-1a54-40a0-a54c-479d2601ef23)

### 6. Verify Trunk Operation
Use the following commands to confirm that trunking is enabled and VLANs are active on the trunk port:
**Command 1: Interface Switchport Details**
```cmd
Switch# show interface GigabitEthernet0/1 switchport
```
![image](https://github.com/user-attachments/assets/58a7371c-56c8-46ad-b88e-113322325710)

Verify:
- **Administrative Mode:** trunk
- **Operational Mode:** trunk
- **Trunking VLANs Enabled:** 10,20
- **Encapsulation:** dot1q
- **Native VLAN:** 1

**Switch# show interface GigabitEthernet0/1 switchport**
```cmd
Switch# show interfaces trunk
```
![image](https://github.com/user-attachments/assets/f482b8f1-ffec-4b51-9e7f-e7160cfd599a)

Confirm:
- Port Gi0/1 is **trunking**
- **VLANs allowed:** 10,20
- **Native VLAN:** 1
- **Spanning Tree State:** forwarding

> VLAN 10 and 20 were created and assigned to access ports Fa0/1 and Fa0/2 respectively.<br>
> Gi0/1 was configured as a trunk port to carry tagged VLAN traffic to the router. <br>
> Trunking was verified using show interface switchport and show interfaces trunk, confirming proper inter-VLAN forwarding capability.
