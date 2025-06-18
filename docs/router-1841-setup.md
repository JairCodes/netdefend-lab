# Router-1841-Setup

## Purpose
This guide provides step-by-step instructions to physically connect, access, and configure a Cisco 1841 router for use in the NetDefend Lab environment. It focuses on preparing the router for inter-VLAN routing using Router-on-a-Stick and verifying its integration with the 2960 switch.

## Physical Connection
1. Connect a Cisco console cable (RJ-45 to USB adapter) from your PC/Laptop to the Console port on the Cisco 1841 router.
2. Power on the router; observe status LEDs.
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
When the router boots for the first time, it will prompt you with the System Configuration Dialog. Type `no` to skip it and proceed with manual configuration.

### 1. Skip the System Configuration Dialog <br>
 ```cmd
Would you like to enter the initial configuration dialog? [yes/no]: no
Router>
 ```
**Action**: Type `no` and press Enter. <br>

### 2. Enter Global Configuration Mode
 ```cmd
Router> enable
Router# configure terminal
Router(config)#
 ```

### 3. Configure Subinterfaces for VLANs
Since the switch is trunking VLANs 10 and 20 to the router, we’ll configure Router-on-a-Stick subinterfaces on the router’s trunk port (e.g., FastEthernet0/1):

**Subinterface for VLAN 10 (INTERNAL)**
 ```cmd
Router(config)# interface FastEthernet0/1.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0
Router(config-subif)# exit
 ```

**Subinterface for VLAN 20 (GUEST)**
 ```cmd
Router(config)# interface FastEthernet0/1.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0
Router(config-subif)# exit
 ```

### 4. Enable the Trunk Interface
Ensure the physical interface `FastEthernet0/1` is up:
 ```cmd
Router(config)# interface FastEthernet0/1
Router(config-if)# no shutdown
Router(config-if)# exit
 ```

### 5. Save the Configuration
 ```cmd
Router# write memory
 ```

### 6. Verify Interface Status
Run the following command to confirm interface and subinterface status:
 ```cmd
Router# show ip interface brief
 ```
Expected output: <br>
![image](https://github.com/user-attachments/assets/4fb6a6ef-50c3-4bac-98e4-1cb42937b9a7)

> Inter-VLAN routing is enabled using FastEthernet0/1 as a trunk link to the switch.
> VLAN 10 and 20 traffic is handled via dot1Q subinterfaces with IPs 192.168.10.1 and 192.168.20.1.
> Subinterfaces are up, trunking is live, and the router is ready for end device testing.
