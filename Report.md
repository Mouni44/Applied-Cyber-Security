# The Report on Article 

### "https://www.virtualbox.org/manual/ch06.html"




## Topic 1 : Virtual Networking Hardware

Oracle VM VirtualBox offers various virtual networking hardware options to facilitate communication between host and guest operating systems. The choice of networking hardware impacts guest OS compatibility, performance, and specific functionalities.

1. AMD PCNet PCI II (Am79C970A)

Supported by most operating systems and GRUB boot manager.
Default setting due to wide OS compatibility.

2. AMD PCNet FAST III (Am79C973)

Default choice for compatibility with almost all operating systems.

3. Intel PRO/1000 Series

MT Desktop (82540EM): Suitable for Windows Vista and later versions.
T Server (82543GC): Recognized by Windows XP without additional drivers.
MT Server (82545EM): Facilitates OVF imports from various platforms.

4. Paravirtualized network adapter (virtio-net)

 - Utilizes a special software interface for improved performance.
 - Requires specific drivers and support:
     - Linux kernels version 2.6.25 or later with potential backporting.
     - Windows 2000, XP, and Vista drivers available from the KVM project.

 - Important Notes:
     - Jumbo frames (networking packets >1500 bytes) are supported under certain conditions:
         - Supported with Intel card virtualization and bridged networking.
         - Not compatible with AMD networking devices, causing packet loss.
         - Typically not enabled by default to prevent unexpected behavior.

 - Recommendations:
     - For general compatibility: Use AMD PCNet FAST III or AMD PCNet PCI II.
     - For Windows XP compatibility without additional drivers: Utilize Intel PRO/1000 T Server.
     - For improved performance and specific OS support: Consider Paravirtualized (virtio-net) with appropriate drivers.


This report provides an overview of the networking hardware options in Oracle VM VirtualBox, outlining their compatibility, special features, and recommendations based on different scenarios.





## Topic 2 : Introduction to Networking Modes


Oracle VM VirtualBox Networking Modes

Oracle VM VirtualBox offers a range of networking modes, each designed to facilitate specific network configurations and interactions between host and guest operating systems. These modes cater to various scenarios, from simple web browsing to advanced network simulations and server hosting within virtual environments.

1. Not Attached Mode

     - Reports the presence of a network card but indicates no connection.
     - Allows for disrupting the virtual Ethernet connection, useful for signaling the absence of network availability to the guest OS.
2. Network Address Translation (NAT)

     - Default mode for basic web browsing, file downloads, and email within the guest.
     - Certain limitations exist, especially regarding Windows file sharing.
3. NAT Network

     - An internal network type allowing outbound connections.
     - See Section 6.4, “Network Address Translation Service” for further details.
4. Bridged Networking

     - Suitable for advanced networking needs like network simulations and running servers within a guest.
     - Establishes direct network packet exchange between VirtualBox and installed network cards, bypassing the host OS network stack.
5. Internal Networking

     - Creates a software-based network visible only to selected virtual machines.
     - Isolated from applications on the host and the outside world.
6. Host-Only Networking

     - Establishes a network containing the host and a group of virtual machines.
     - Utilizes a virtual network interface on the host, enabling connectivity among the host and virtual machines.
7. Cloud Networking

     - Connects a local VM to a subnet on a remote cloud service.
     - Facilitates integration between local virtual machines and cloud-based networks.
8. Generic Networking

     - Rarely used modes sharing a generic network interface.
     - Allows selection of a driver included in Oracle VM VirtualBox or distributed in an extension pack.


### Sub-Modes (Examples)

 - UDP Tunnel: Interconnects VMs across different hosts transparently over an existing network.
 - VDE (Virtual Distributed Ethernet) Networking: Connects to a Virtual Distributed Ethernet switch on Linux or FreeBSD hosts (requires compilation from sources).


### Summary Table - Overview of Networking Modes

| Mode	|    VM → Host|	VM ← Host|	VM1 ↔ VM2|	VM → Net/LAN|	VM ← Net/LAN|
| --- | ---| --- | --- | --- | --- |
| Host-only	|    +	  |      +	  |      +	 |       -	  |          - |
| Internal	|    -	  |      -	  |      +	 |       -	  |          - |
| Bridged	|     +	  |      +	  |      +	 |       +	  |          + |
| NAT	   |      +	 |   Port fwd	|    -	 |       +	  |      Port fwd |
| NAT Service	|   +	|   Port fwd |    +	|     +	 |     Port fwd |


This report outlines the diverse networking modes offered by Oracle VM VirtualBox, emphasizing their functionalities, connectivity directions, and compatibility with different virtual machine setups.





## Topic 3 : Network Address Translation (NAT)

Network Address Translation (NAT) Mode in Oracle VM VirtualBox

NAT mode in Oracle VM VirtualBox offers a straightforward method for virtual machines to access external networks without requiring complex configurations on the host network or guest system. It mirrors the functionality of a real computer connecting to the internet through a router, with VirtualBox acting as the networking engine that transparently maps traffic for each VM.

### Functionality

 - Virtual Machine Behavior:

    - Functions akin to a computer accessing the internet through a router.
    - VirtualBox's networking engine acts as a router between each VM and the host, maximizing security by default (VMs cannot directly communicate with each other).

 - Accessibility and Disadvantages:

    - VM in NAT mode is invisible and unreachable from the outside internet.
    - Inability to run a server without configuring port forwarding due to its router-like behavior.

 - Networking Setup:

    - VirtualBox's NAT engine assigns network addresses and configurations via an integrated DHCP server.
    - The VM receives an IP address on a different network than the host, enabling multiple VMs to use NAT with different private networks.

### Port Forwarding Configuration

- Purpose:
        - Allows selected services within the VM to be accessible to the external world through VirtualBox's port forwarding capabilities.
- Process:
        - Selective forwarding of specific ports from the host to the VM, similar to a physical router.
        - Offers the advantage of service isolation and diverse OS usage within a VM environment.
- Configuration Methods:
        - Graphical Port Forwarding editor in VirtualBox's Network settings for NAT-configured network adaptors.
        - Command line tool VBoxManage for more advanced users (VBoxManage modifyvm).
- Example Command (VBoxManage):
        - Illustrative command for setting up incoming NAT connections to an SSH server in the guest VM with port forwarding.
        - Provides options for specifying guest IP, host ports, and protocols.
### Limitations of NAT Mode

- ICMP Protocol and UDP Broadcast Limitations:
    - ICMP protocol support limitations affect network debugging tools like ping or traceroute.
    - UDP broadcasts may not reliably reach the guest VM.
- Unsupported Protocols:
    - NAT mode supports TCP and UDP but does not accommodate protocols like GRE (used in some VPN products).
 - Port Limitation on UNIX-based Hosts:
    - Binding to ports below 1024 from non-root applications is restricted on UNIX-based hosts, causing VM refusal to start in such cases.


 - Conclusion


    NAT mode in Oracle VM VirtualBox offers simplified external network access for virtual machines while maintaining inherent security by restricting external accessibility. However, limitations concerning ICMP, UDP broadcasts, unsupported protocols, and port bindings below 1024 on UNIX-based hosts should be considered based on specific network requirements and application needs.





## Topic 3 : Network Address Translation Service 

Network Address Translation (NAT) Service in Oracle VM VirtualBox

The NAT service in Oracle VM VirtualBox operates similarly to a home router, creating a network where systems within it can communicate with each other and the external world using TCP and UDP over both IPv4 and IPv6. It ensures that systems outside the network cannot directly access systems inside it, enhancing security while facilitating internal communication.

### NAT Configuration and Creation

 - Internal Network Attachment:
    - The NAT service is linked to an internal network where VMs intended to use it should be connected.
    - Creation of the internal network involves specifying a network name and address using commands like VBoxManage natnetwork add.
 - DHCP Configuration:
    - DHCP server attachment to the internal network is optional and can be enabled using commands like VBoxManage natnetwork modify.
    - Commands like --dhcp on enable DHCP server assignment, providing convenience in network configuration.

### Starting and Stopping the NAT Service

 - Initiating the Service:
    - Activation of the NAT service and associated DHCP server is done using commands such as VBoxManage natnetwork start.
 - Termination of Service:
    - Halting the NAT network service and DHCP server is accomplished via commands like VBoxManage natnetwork stop.

### Modifying and Deleting NAT Network Services

 - DHCP Server Management:
    - Modifications to DHCP server settings within the NAT network involve commands like VBoxManage natnetwork modify.
    - Deletion of the NAT network service is executed using commands like VBoxManage natnetwork remove.

### Port Forwarding

 - Port-forwarding Configuration:
    - Utilization of port-forwarding rules allows specific host ports to map to guest ports on specific VMs.
    - Commands such as --port-forward-4 and --port-forward-6 are used for IPv4 and IPv6 configurations, respectively.

### Additional Configuration and Management

 - Binding to Interfaces:
    - Configuration to bind a NAT service to specified interfaces can be done using commands like VBoxManage setextradata.
 - List and Management of NAT Networks:
    - Retrieval of registered NAT networks is done with VBoxManage list natnetworks.
    - Network creation, deletion, and configuration can also be managed via the Network Manager tool in VirtualBox Manager.

### Note on Access and Loopback Interface
 - Access to Host's Loopback Interface:
    - Despite the NAT service separating VMs from the host, VMs retain access to the host's loopback interface and its running network services.
    - The host's loopback interface is accessible through a specific IP address (e.g., 10.0.2.2) and can be beneficial for certain development scenarios (e.g., running a web app in a VM while the database server operates on the host's loopback interface).






## Topic 4 : Bridged Networking

Bridged Networking in Oracle VM VirtualBox

Bridged networking in Oracle VM VirtualBox employs a net filter driver on the host system to intercept and manipulate data between the physical network adapter and the guest VMs. This process effectively creates a new software-based network interface, allowing the host system to send and receive data to and from the guest VMs as if they were physically connected.

### Functionality and Configuration

 - Net Filter Driver Usage:
    - Utilizes a net filter driver on the host to intercept and manipulate data from the physical network adapter.
    - Creates a new network interface in software, enabling routing or bridging between the guest VM and the rest of the network.

 - Enabling Bridged Networking:
    - Accessible through the Settings dialog of a virtual machine.
    - Select Bridged Network in the Network page's Attached To field.
    - Choose a host interface from the list representing physical network interfaces.

### Wireless Bridged Networking and Limitations

 - Wireless Interface Usage:
    - Bridging to wireless interfaces operates differently due to limited support for promiscuous mode in most wireless adapters.
    - Oracle VM VirtualBox modifies Ethernet headers to ensure proper packet routing between the host and the guest VMs.

 - Limitations based on Host OS:
    - macOS hosts and Linux hosts have specific limitations while using wireless interfaces for bridged networking.
    - Limited functionality for protocols other than IPv4 and IPv6 over wireless connections.

### Host OS-Specific Limitations and Issues

 - macOS Hosts:
    - Bridged networking limitations exist when using AirPort (Mac's wireless networking system).
    - Limited functionality for certain protocols such as IPX, requiring selection of a wired interface for support.

 - Linux Hosts:
    - Similar limitations with wireless interfaces for bridged networking, primarily supporting IPv4 and IPv6.
    - Wired interfaces may be required for protocols like IPX due to limitations.

### Specific Host OS Issues and Known Problems

 - macOS and Linux hosts:
    - Set MTU to less than 1500 bytes on specific wired interfaces (e.g., Marvell Yukon II EC Ultra Ethernet NIC) can cause packet losses.
    - Hardware-based stripping of VLAN tags may limit VLAN trunking with pre-2.6.27 Linux kernels or non-Linux host operating systems.

 - Oracle Solaris hosts:
    - No support for wireless interfaces.
    - Limitations exist with IPFilter due to technical restrictions in the networking subsystem.

### Oracle Solaris 11 Hosts and Special Configurations

 - Oracle Solaris 11 (build 159 and above):
    - Support for using Oracle Solaris Crossbow Virtual Network Interfaces (VNICs) directly with VirtualBox without additional configuration.
    - Specific naming schemes like PPA-hack are essential for VLAN interfaces to ensure proper packet reception.







## Topic 5 : Internal Networking

Internal Networking in Oracle VM VirtualBox

Internal Networking within Oracle VM VirtualBox allows virtual machines (VMs) to communicate directly with each other while being secluded from the external network. This setup permits communication limited to VMs connected to the same internal network on the host system.

### Functionality and Purpose

 - Network Isolation:
    - VMs on the same host can communicate exclusively within the internal network.
    - External connectivity is restricted, enabling private communication between VMs while shielding their data from the host system and user.

 - Automatic Network Creation:
    - Internal networks are dynamically generated as needed, identified solely by their assigned names.
    - Multiple active virtual network cards with the same internal network ID automatically link via VirtualBox's support driver, which operates as an Ethernet switch.

### Configuration and Usage

 - Setting up Internal Networking:
    - VM's network card configuration can be set to Internal Networking via VirtualBox Manager or command line (VBoxManage modifyvm).
    - VirtualBox Manager's Settings window allows selection of Internal Network mode and the choice of an existing internal network or the creation of a new one by name.

 - Command Line Configuration:
    - Command-line example: VBoxManage modifyvm "VM name" --nic<x> intnet.
    - Optional specification of a network name: VBoxManage modifyvm "VM name" --intnet<x> "network name".

### DHCP Server and IP Management

 - DHCP Server Usage:
    - Oracle VM VirtualBox incorporates a built-in DHCP server to manage IP addresses for internal networks.
    - DHCP utilization is recommended for managing IP addresses unless static IP configurations are implemented within guest operating systems.

### Security Measures and User Accessibility

 - Security by Default:
    - The Linux implementation of internal networking restricts VMs under the same user ID to establish an internal network.
    - Default setting enhances security by limiting internal network access to VMs under the same user ID.

 - Shared Networking Interface Access:
    - Possibility exists to create a shared internal networking interface accessible by users with varying user IDs.
    - Offers flexibility in user accessibility while maintaining a degree of security within the internal network.






## Topic 6 : Host - Only Networking

Host-only Networking in Oracle VM VirtualBox

Host-only Networking represents a networking mode in Oracle VM VirtualBox that combines elements from both bridged and internal networking modes. It facilitates communication among virtual machines (VMs) and the host system while isolating them from external networks.

### Functionalities and Purpose

 - Virtual Isolation and Communication:
    - VMs can interact with each other and the host system as if connected through a physical Ethernet switch.
    - External connectivity is restricted, preventing communication beyond the host environment.

 - Interface Creation on Host:
    - Oracle VM VirtualBox generates a new software interface on the host, functioning similarly to a loopback interface.
    - This interface enables communication among VMs and the host, creating a closed network environment.

### Configuration and Usage

 - Setup Process:
    - Host-only Networking requires the creation of a new loopback interface on the host, distinct from physical network interfaces.
    - Configuration within VirtualBox Manager involves selecting the Host-Only Network option for network adapters in the VM's Settings dialog.
    - Command-line configuration via VBoxManage modifyvm vmname --nicx hostonly is also available for setting up host-only networking.

### DHCP Server and IP Management

 - DHCP Utilization:
    - Oracle VM VirtualBox incorporates a built-in DHCP server for managing IP addresses within host-only networks.
    - DHCP server enables automated IP address assignment, offering convenience in network setup and configuration.

 - Configuration via Network Manager:
    - The Network Manager tool within VirtualBox Manager allows configuration of DHCP server settings for host-only networks.

### Limitations and Constraints

 - Platform-Specific Limitations:
    - Recent macOS versions do not support host-only adapters; instead, host-only networks define IP address ranges.
    - macOS hosts utilize Host-Only Networks in place of host-only adapters, defining IP address ranges where the host receives the lowest address.

 - Interface Number and Address Range Constraints:
    - Limited number of host-only interfaces on Linux and macOS hosts (up to 128).
    - Specific IP address ranges (192.168.56.0/21 for IPv4 and link-local addresses for IPv6) are allowed on Linux, macOS, and Solaris hosts. Custom ranges require configuration in /etc/vbox/networks.conf.
 - Custom Address Range Configuration:
    - Configuration of additional IP address ranges is possible by specifying allowed ranges in /etc/vbox/networks.conf file for IPv4 and IPv6.






## Topic 7 : UDP Tunneling Networking

UDP Tunnel Networking Mode in Oracle VM VirtualBox

The UDP Tunnel networking mode in Oracle VM VirtualBox enables the interconnection of virtual machines running on different hosts. It achieves this by encapsulating Ethernet frames into UDP/IP datagrams, transmitting them over available networks between hosts.

### Technical Operations

 - Encapsulation and Transmission:
    - Ethernet frames from the guest network card are encapsulated into UDP/IP datagrams for transmission.
    - The host listens on a specific UDP port, forwarding arriving datagrams to the respective guest network card.

 - Parameter Definitions:
    - Source UDP Port: Host's listening port for incoming datagrams from any source address, forwarded to the guest network card.
    - Destination Address: IP address of the target host receiving transmitted data.
    - Destination UDP Port: Port number where transmitted data is directed.

### Configuration and Setup

 - Command-line Configuration:
    - Configuration involves modifying VM settings using VBoxManage commands to specify the networking mode, driver, destination IP, and UDP ports.
    - Configuration on both hosts involves setting up generic networking mode, specifying UDPTunnel as the driver, and setting destination IP and UDP ports.

### Interconnecting Virtual Machines

 - Inter-host Configuration:
    - Example configuration involves VMs on different hosts, swapping IP addresses, and configuring source and destination UDP ports accordingly.

 - Intra-host Configuration:
    - VMs on the same host can be interconnected by setting the destination address parameter to 127.0.0.1 on both VMs.
    - Traffic visibility: Unlike a typical internal network, this setup allows the host to view the network traffic.

### Notes and Limitations

 - Port Binding Limitation (UNIX-based Hosts):
    - Limitation on UNIX-based hosts (Linux, Oracle Solaris, Mac OS X) prevents binding to ports below 1024 by non-root applications.
    - Configuration of source UDP ports below 1024 is restricted, leading to VM refusal to start in such cases.






## Topic 8 : VDE Networking

Virtual Distributed Ethernet (VDE) in Oracle VM VirtualBox

Virtual Distributed Ethernet (VDE) is an advanced virtual network infrastructure system integrated into Oracle VM VirtualBox. It facilitates secure L2/L3 switching, VLANs, spanning-tree protocols, and WAN emulation across multiple hosts. Developed by Renzo Davoli, an Associate Professor at the University of Bologna, Italy, VDE offers a flexible and secure network environment using VDE switches, plugs, and wires.

### Key Components

 - VDE Infrastructure Components:
    - VDE Switches: The central units enabling networking capabilities and switching functionalities.
    - VDE Plugs: Elements that interconnect and facilitate communication between switches.
    - VDE Wires: Connections established between switches within the VDE infrastructure.

### Configuration and Setup

 - Creating a VDE Switch:
    - Creation using the vde_switch command: vde_switch -s /tmp/switch1.

 - Configuring VMs via Command-line:
    - VDE Driver Setup: Modify VM settings to utilize the VDE driver.

    - Connecting to VDE Switch:
        - Automatic switch port connection: VBoxManage modifyvm "VM name" --nic-property<x> network=/tmp/switch1.
        - Connecting to a specific switch port: VBoxManage modifyvm "VM name" --nic-property<x> network=/tmp/switch1[<n>] for VLANs.

 - VLAN Mapping (Optional):
    - Mapping VDE switch ports to VLANs using VDE switch command line:

```json

vde$ vlan/create <VLAN>
vde$ port/setvlan <port> <VLAN>

```

### Requirements and Availability

 - Host and Software Requirements:
    - VDE is available on Linux and FreeBSD hosts only.
    - Requires installation of VDE software and the VDE plugin library from the VirtualSquare project on the host system.

 - Linux Host Considerations:
    - Availability of the shared library libvdeplug.so in the shared libraries' search path for Linux hosts.
 - Additional Notes and References
    - For Linux hosts, ensuring the availability of required shared libraries is essential.
    - Detailed setup and configuration instructions can be found in the accompanying software documentation.
    - Further information can be accessed via the VirtualSquare project's wiki: http://wiki.virtualsquare.org.







## Topic 9 : Cloud Networks

Cloud Networks in Oracle VM VirtualBox

Introduction

Cloud networks in Oracle VM VirtualBox facilitate connections from a local virtual machine to a subnet on a remote Oracle Cloud Infrastructure instance. This feature is particularly useful for establishing connections between local virtual machines and remote cloud-based infrastructure, enhancing network flexibility and functionality.

### Creating and Configuring Cloud Networks

1. To create and configure a cloud network:

 - Using Network Manager Tool: Access the Network Manager tool in VirtualBox Manager by navigating to Section 6.11.3, “Cloud Networks Tab.” Follow the tool's guidelines to create and configure a cloud network.

2. Through Virtual Machine Settings:

 - Open the Settings dialog of the respective virtual machine.
 - Go to the Network page and select the Adapter tab.
 - Ensure the Enable Network Adapter checkbox is selected.
 - Choose "Cloud Network" from the Attached To field.

3. Via Command Line: Utilize the VBoxManage tool with the following command:

```css

VBoxManage modifyvm vmname --nicx cloud

```

### Enabling Cloud Network Interface

 - To enable a cloud network interface for a virtual machine:
    - Access the Settings dialog of the VM.
    - Navigate to the Network page and Adapter tab.
    - Enable the Network Adapter and select "Cloud Network" as the Attached To field.
    - Alternatively, use the VBoxManage command-line tool to modify the VM settings.

### Benefits of Cloud Networks

 - Remote Connectivity: Facilitates connections from local VMs to remote Oracle Cloud Infrastructure instances.
 - Enhanced Flexibility: Enables flexible networking configurations between local and cloud-based resources.
 - Simplified Setup: User-friendly tools and command-line options for seamless configuration.


### Conclusion

Cloud networks in Oracle VM VirtualBox offer an efficient means to establish connections between local virtual machines and remote Oracle Cloud Infrastructure instances. By leveraging these networks, users can enhance connectivity and streamline interactions between local and cloud-based resources.






## Topic 10 : Network Manager

Network Manager Tool in VirtualBox Manager

The Network Manager tool within VirtualBox Manager provides comprehensive control over various network configurations used by Oracle VM VirtualBox. It enables the creation, deletion, and configuration of the following network types:

### 1. Host-Only Networks (Section 6.11.1)

 - Features:
    - List View: Displays all currently active host-only networks.
    - Create: Adds a new host-only network to the list.
    - Remove: Deletes a host-only network from the existing list.
    - Properties: Shows or hides settings for the selected host-only network.
 - Configuration:
    - Adapter Tab: Configures the network adapter for the host-only network.
    - DHCP Server Tab: Manages DHCP server settings for automatic IP address assignment.

### 2. NAT Networks (Section 6.11.2)

 - Functionalities:
    - Overview: Lists all currently utilized NAT networks.
    - Create: Adds a new NAT network to the existing list.
    - Remove: Deletes a NAT network from the current list.
    - Properties: Shows or hides settings for the selected NAT network.
 - Configuration:
    - General Options Tab: Configures network settings like network address and mask.
    - Port Forwarding Tab: Sets up port forwarding rules for the NAT network.

### 3. Cloud Networks (Section 6.11.3)

 - Capabilities:
    - Overview: Displays all active cloud networks.
    - Create: Adds a new cloud network to the network list.
    - Remove: Deletes a cloud network from the available list.
    - Properties: Displays or hides settings for the selected cloud network.
 - Configuration:
    - Name: Specifies the name for the cloud network.
    - Provider: Identifies the cloud service provider (e.g., Oracle Cloud Infrastructure).
    - Profile: Specifies the cloud profile for connecting to the network.
    - ID: Specifies the OCID for the cloud tunneling network, also provides access to available subnets on Oracle Cloud Infrastructure for traffic tunneling.

### Conclusion

The Network Manager tool in VirtualBox Manager simplifies the management of various network types used by Oracle VM VirtualBox. It offers a user-friendly interface for creating, modifying, and removing host-only networks, NAT networks, and cloud networks. This tool streamlines network configuration for virtual environments, enhancing flexibility and control.

For more detailed guidance on using VBoxManage commands for creating and configuring virtual cloud networks (VCN) on Oracle Cloud Infrastructure, refer to Section 1.16.10, “Using a Cloud Network”.






## Topic 11 : Limiting Bandwidth for Network Input/Output

Bandwidth Control in Oracle VM VirtualBox

Oracle VM VirtualBox facilitates the management of maximum network bandwidth transmission for enhanced control over network traffic. Bandwidth limits can be set for individual adapters or grouped adapters sharing a specified limit. Here's an overview of the process:

### Bandwidth Control Configuration

1. Creation of Bandwidth Group:

 - Example: Creating a bandwidth group named "Limit" and setting the limit to 20 Mbps.

```bash
 VBoxManage bandwidthctl "VM name" add Limit --type network --limit 20m
```

2. Assigning Bandwidth Groups to Adapters:

 - Example: Assigning the "Limit" group to the first and second adapters of the VM.

```bash
VBoxManage modifyvm "VM name" --nicbandwidthgroup1 Limit
VBoxManage modifyvm "VM name" --nicbandwidthgroup2 Limit

```

3. Bandwidth Sharing Within Groups:

 - All adapters within a group share the specified bandwidth limit. For instance, if the limit is 20 Mbps and two adapters are in the same group, the combined bandwidth usage cannot exceed 20 Mbps. However, if one adapter isn't utilizing its bandwidth allocation, the other adapter can utilize the remaining bandwidth within the group.

### Dynamic Limit Modifications

 - Adjusting Bandwidth Limits on the Fly:

    - The bandwidth limits for groups can be dynamically altered while the VM is running, with immediate effect.

        ```bash

        VBoxManage bandwidthctl "VM name" set Limit --limit 100k

        ```

 - Disabling Bandwidth Shaping:

    - Completely disabling bandwidth shaping for a specific adapter:

        ```bash

        VBoxManage modifyvm "VM name" --nicbandwidthgroup1 none

        ```
    - Disabling shaping for all adapters within a bandwidth group while the VM is running:

        ```bash

        VBoxManage bandwidthctl "VM name" set Limit --limit 0

        ```

### Note

VirtualBox shapes traffic in the transmit direction by delaying outgoing packets but doesn't limit incoming traffic to virtual machines.

### Conclusion

Oracle VM VirtualBox offers robust bandwidth control capabilities, allowing for the establishment of bandwidth limits on individual or grouped adapters. This feature enhances network management by providing dynamic control over bandwidth allocation to optimize network performance.






## Topic 12 : Improving Network Performance

Enhancing Network Performance in Oracle VM VirtualBox

### 1. Optimal Adapter Choices

 - Prefer virtio over Emulated Adapters:
    - virtio network adapters offer superior performance compared to Intel PRO/1000 emulated adapters.
    - Intel PRO/1000 adapters are preferred over PCNet adapters.
 - Segmentation and Checksum Offloading:
    - Both virtio and Intel PRO/1000 adapters support offloading, reducing context switches and enhancing packet transmission.
    - Note: Windows XP guests with these drivers lack segmentation offloading.

### 2. Attachment Type and Performance

 - Internal, Bridged, Host-Only Attachment:
    - Internal type is slightly faster, utilizing fewer CPU cycles as packets bypass the host's network stack.
    - Bridged and Host-Only attachments offer similar performance.
 - NAT Attachment:
    - Slower but more secure due to Network Address Translation (NAT).
 - Generic Driver Attachment:
    - Specialized and not an alternative to other attachment types.

### 3. Considerations for Network Optimization

 - CPU Allocation:
    - Increasing CPU count does not necessarily enhance network performance and might sometimes degrade it due to increased concurrency within the guest.
 - Configuration Checks:
    - Ensure virtio adapters are used whenever feasible, followed by Intel PRO/1000 adapters.
    - Choose Bridged attachment over NAT for improved performance.

### 4. Offloading Configuration and Analysis

 - Segmentation Offloading:
    - Confirm offloading is enabled in the guest OS for enhanced performance.
    - Utilize tools like 'ethtool' in Linux guests to verify and modify offloading settings.
 - Detailed Network Analysis:
    - Employ third-party tools like Wireshark for comprehensive network traffic analysis.
    - Enable promiscuous mode for the VM's network adapter to analyze traffic.

### 5. Promiscuous Mode Policies

 - Modes for Network Adapter:
    - deny: Default setting, hides unrelated traffic.
    - allow-vms: Conceals host traffic but permits communication between VMs.
    - allow-all: Removes all restrictions, allowing the VM's adapter to capture all traffic.
