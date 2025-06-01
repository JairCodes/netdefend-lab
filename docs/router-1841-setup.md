# Router-1841-Setup

## Purpose

This guide walks you through the physical connection, console access, and core configuration steps required to turn a Cisco 1841 router into a router-on-a-stick for the NetDefend Lab environment. By the end of this exercise, you will have:


## Physical Connection
1. Connect a Cisco console cable (RJ-45 to USB adapter) from your PC/Laptop to the **Console** port on the 1841
2. Power on the router; observe LEDs next to the console port
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

Once you’ve connected to the 1841’s console port and opened PuTTY (or your terminal client), perform the following steps:

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
Once you have skipped the initial dialog and returned to the `Switch>` (or `Router>`) prompt, you need to elevate your privileges so you can make configuration changes:

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
   Still at `Switch(config-vlan)#`. enter:
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
