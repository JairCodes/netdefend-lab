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
> Note: In PuTTY under 'SSH' in 'Serial' you can manually configure the serial line

---

## 3. Initial Configuration

![image](https://github.com/user-attachments/assets/fcc57e86-b113-4158-b7e5-c2271da2068d) <br>
  
  If prompted 'Would you like to enter the initial configuration dialog? [yes/no]:' <br>
  Enter 'no'

![image](https://github.com/user-attachments/assets/4d795489-4c32-4f02-ab63-24b227abebbb)

![image](https://github.com/user-attachments/assets/8f5352ce-2b1b-43d5-8715-36485962cb57)
