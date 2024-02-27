**inter-VLAN trunking** and **SSH configuration**. I'll provide an overview of each topic along with relevant commands.

## Inter-VLAN Trunking Configuration Cheat Sheet

### Traditional Inter-VLAN Routing
- **Description**: This method relies on a router with multiple physical interfaces, where each interface is connected to a switch (one for each VLAN). The router interfaces become default gateways for hosts in each VLAN.
- **Steps**:
    1. Configure router interfaces with IP addresses for each VLAN.
    2. Set switch ports connected to the router in **access mode**.
    3. Ensure that the router's routing table includes routes for all VLANs.
    4. Hosts in different VLANs can communicate via the router.

### Router-on-a-Stick Inter-VLAN Routing
- **Description**: In this approach, a single router interface is used for all VLANs. The router performs inter-VLAN routing based on subinterfaces (sub-interfaces with different VLAN tags).
- **Steps**:
    1. Create subinterfaces on the router interface (e.g., `FastEthernet0/0.10` for VLAN 10).
    2. Assign VLAN tags to subinterfaces (e.g., VLAN 10 = tag 10).
    3. Configure IP addresses on subinterfaces.
    4. Set the switch port connected to the router in **trunk mode**.
    5. Hosts in different VLANs communicate via the router's subinterfaces.

### Multilayer Switch Inter-VLAN Routing
- **Description**: Multilayer switches (Layer 3 switches) can perform inter-VLAN routing without an external router. They have VLAN interfaces and routing capabilities.
- **Steps**:
    1. Create VLANs on the switch.
    2. Assign IP addresses to VLAN interfaces (e.g., `interface VLAN10`).
    3. Enable IP routing (`ip routing` command).
    4. Set switch ports in **access mode** or **trunk mode** as needed.
    5. Hosts communicate via the switch's VLAN interfaces.

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