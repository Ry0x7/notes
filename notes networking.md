
```cmd

! Step 1: Enter the global configuration mode
Switch> enable
Switch# configure terminal

! Step 2: Assign a hostname to the switch
Switch(config)# hostname S1
S1(config)#

! Step 3: Configure a password for privileged EXEC mode
S1(config)# enable secret class

! Step 4: Configure a password for console access
S1(config)# line console 0
S1(config-line)# password cisco
S1(config-line)# login

! Step 5: Configure a password for vty access
S1(config)# line vty 0 15
S1(config-line)# password cisco
S1(config-line)# login
S1(config)# service password-encryption

! Step 6: Configure an IP address and subnet mask for the switch management interface (VLAN 1)
S1(config)# interface vlan 1
S1(config-if)# ip address 192.168.1.2 255.255.255.0
S1(config-if)# no shutdown

! Step 7: Configure SSH access to the switch
S1(config)# ip domain-name example.com
S1(config)# crypto key generate rsa
! Enter the modulus size (1024 or higher)
S1(config)# 1024
S1(config)# ip ssh version 2
S1(config)# username admin secret class
S1(config)# line vty 0 15
S1(config-line)# transport input ssh

! Step 8: Configure VLANs on the switch
S1(config)# vlan 10
S1(config-vlan)# name Sales
S1(config-vlan)# exit
S1(config)# vlan 20
S1(config-vlan)# name Marketing
S1(config-vlan)# exit

! Step 9: Assign switch ports to VLANs
S1(config)# interface range fastEthernet 0/1 - 12
S1(config-if-range)# switchport mode access
S1(config-if-range)# switchport access vlan 10
S1(config-if-range)# exit
S1(config)# interface range fastEthernet 0/13 - 24
S1(config-if-range)# switchport mode access
S1(config-if-range)# switchport access vlan 20
S1(config-if-range)# exit

! Step 10: Configure Trunking
S1(config)# interface range gigabitEthernet 0/1 - 2
S1(config-if-range)# switchport mode trunk
S1(config-if-range)# switchport trunk allowed vlan all
S1(config-if-range)# exit

! Step 11: Configure Native VLAN on Trunk Ports
S1(config)# interface range gigabitEthernet 0/1 - 2
S1(config-if-range)# switchport trunk native vlan 99
S1(config-if-range)# exit

! Step 12: Setting up Router Subinterface
Router(config)# interface GigabitEthernet0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0
Router(config-subif)# exit

Router(config)# interface GigabitEthernet0/0.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0
Router(config-subif)# exit

! Step 13: Setting switch default gateway for VLANS
S1(config)# interface vlan 10
S1(config-if)# ip address 192.168.10.2 255.255.255.0
S1(config-if)# exit

S1(config)# interface vlan 20
S1(config-if)# ip address 192.168.20.2 255.255.255.0
S1(config-if)# exit

! Step 14: Setting Up DHCPv4 pool for each interface or VLAN
Router(config)# ip dhcp pool VLAN10
Router(dhcp-config)# network 192.168.10.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.10.1
Router(dhcp-config)# exit

Router(config)# ip dhcp pool VLAN20
Router(dhcp-config)# network 192.168.20.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.20.1
Router(dhcp-config)# exit

! Step 15: Enable DHCP on vlan interfaces On the switch, configure the helper address to point to the router’s IP address.
S1(config)# interface vlan 10
S1(config-if)# ip helper-address 192.168.10.1
S1(config-if)# exit

S1(config)# interface vlan 20
S1(config-if)# ip helper-address 192.168.20.1
S1(config-if)# exit


```
