# Packet Tracer Lab - VLAN, VTP, and DTP Configuration

## Lab Overview
In this lab, we will perform the following tasks to configure VLANs, trunking, and VTP modes across three switches, while disabling DTP on specific interfaces. The setup includes various hosts connected to three switches (SW1, SW2, and SW3) within different VLANs. This lab also covers the configuration of VTP modes and the creation and verification of VLAN databases across the switches.
<img src="https://github.com/ro-drick/VLAN-VTP-DTP-Configuration/blob/main/DTP%26VTP.PNG">
## Tasks Overview

### Task 1: Configure Switchports as Trunk Ports

1. Configure the switchports connecting the switches as **trunk ports**.
2. Disable **Dynamic Trunking Protocol (DTP)** on these ports.
3. Verify and confirm the administrative and operational mode of each interface.

#### Commands:
```bash
# On each interface connecting the switches
Switch(config)# interface <interface_id>
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport nonegotiate  # Disables DTP
Switch(config-if)# end
Switch# show interfaces trunk  # Verify trunk status
```
### Task 2: Configure SW1 in VTP Domain
1. Set SW1 as the VTP Server in the CCNA domain.
2. Create VLANs 10, 20, and 30 on SW1.
3. Ensure SW2 and SW3 add VLANs 10, 20, and 30 from SW1 using VTP.
```bash
# On SW1
SW1(config)# vtp domain CCNA
SW1(config)# vtp mode server
SW1(config)# vlan 10
SW1(config-vlan)# name VLAN10
SW1(config-vlan)# vlan 20
SW1(config-vlan)# name VLAN20
SW1(config-vlan)# vlan 30
SW1(config-vlan)# name VLAN30
SW1(config)# end
SW1# show vlan brief  # Verify VLANs

# On SW2 and SW3
SW2(config)# vtp domain CCNA
SW2(config)# vtp mode client
```
### Task 3: Configure SW2 in VTP Transparent Mode
1. Set SW2 to VTP transparent mode.
2. Add VLAN40 on SW2.
3. Verify whether VLAN40 is added to the VLAN databases of SW1 and SW3.
```bash
# On SW2
SW2(config)# vtp mode transparent
SW2(config)# vlan 40
SW2(config-vlan)# name VLAN40
SW2(config)# end
SW2# show vlan brief  # Verify VLAN40 on SW2
SW1# show vlan brief  # Check if VLAN40 is on SW1 (should not be)
SW3# show vlan brief  # Check if VLAN40 is on SW3 (should not be)
```
### Task 4: Configure SW3 in VTP Client Mode
1. Set SW3 to VTP client mode.
2. Attempt to create VLAN50 on SW3.
3. Verify if VLAN50 is added to the VLAN database.
```bash
# On SW3
SW3(config)# vtp mode client
SW3(config)# vlan 50  # Attempt to create VLAN50 (should fail)
SW3# show vlan brief  # Check for VLAN50 (should not appear)
```
### Task 5: Assign VLANs to Hosts
1. Assign the correct VLAN to each switchport connected to hosts.
2. Manually configure these ports as access ports.
3. Check if DTP is still enabled on the access ports.
```bash
# On each switch, assign the correct VLAN to the appropriate interfaces
SW1(config)# interface <interface_id>
SW1(config-if)# switchport mode access
SW1(config-if)# switchport access vlan <VLAN_id>
SW1(config-if)# end
SW1# show interfaces switchport  # Verify if DTP is enabled

# Repeat for all interfaces connecting hosts on SW1, SW2, and SW3
```
### Summary of Configuration
- Trunk Ports: Configured between switches, DTP disabled.
- VTP Mode:
    - SW1: Server mode
    - SW2: Transparent mode
    - SW3: Client mode
- VLANs:
    - VLANs 10, 20, and 30 added to all switches via VTP.
    - VLAN40 added only on SW2 (VTP Transparent).
    - VLAN50 not added due to SW3 being in Client mode.
### Verification and Testing
- Verify trunk ports using the show interfaces trunk command.
- Confirm VLAN database using the show vlan brief command.
- Check VTP mode using show vtp status.
- Ensure DTP is disabled on trunk ports and access ports using the show interfaces switchport command.

## Acknowledgements


Special thanks to **Jeremy's IT Lab** for providing valuable resources and tutorials that greatly contributed to the completion of this exercise. His in-depth explanations and practical demonstrations have been instrumental in enhancing my understanding of Cisco networking concepts and the effective use of Packet Tracer.

For more information and additional resources, visit [Jeremy's IT Lab](https://jeremysitlab.com/) and check out his YouTube for the full course, [Jeremy's IT Lab Free CCNA 200-301 | Complete Course](https://www.youtube.com/playlist?list=PLxbwE86jKRgMpuZuLBivzlM8s2Dk5lXBQ)
