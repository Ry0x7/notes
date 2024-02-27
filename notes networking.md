
```bash
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
```


## SSH Configuration Cheat Sheet

### Enable SSH on Cisco Devices
1. Access the device's CLI.
2. Generate an RSA key pair:
   ```
   crypto key generate rsa
   ```
3. Configure the SSH version and authentication methods:
   ```
   ip ssh version 2
   ip ssh authentication-retries 3
   ```
4. Create a user account and set a strong password:
   ```
   username admin privilege 15 secret <password>
   ```
5. Enable SSH:
   ```
   line vty 0 15
   transport input ssh
   login local
   ```

### Access Control for SSH
- Use ACLs to restrict SSH access to specific IP addresses:
  ```
  access-list 10 permit host <trusted_IP>
  line vty 0 15
  access-class 10 in
  ```

Remember to replace `<trusted_IP>` with the actual trusted IP address.