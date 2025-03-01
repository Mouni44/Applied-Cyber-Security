# Subnetting.net

### SubNetting Tutorial

  ### Introduction to IP

IP Address = 191.54.39.15
            
            191 = Your State
            54 = Your Town
            38 = Your Apartment building
            15 = Your Apartment Number

An IP address is like a postal address. It's a unique identifier for devices connected to a network. Just as your home address specifies your location, an IP address designates a device's location on the internet or a local network.

- In this analogy:

  - 191.54.38.15: Think of it as specifying broader to specific areas (State, Town, Building, Apartment).
  - Router: Similar to a post office, it directs messages (emails) between different IP addresses.
  - Subnet: Refers to a specific network, like apartments in a building. Messages within the same subnet stay local.
  - Routing: Sending messages between different subnets or locations involves routers or post offices routing the message to its destination through hops.

- When sending a message:

 - Intra-Subnet: Messages between devices within the same subnet stay local and don't need a router.
 - Inter-Subnet: Messages between different subnets require routers/post offices to forward the message to the right destination, involving multiple hops.


This process involves routers determining the best path for messages based on their destination addresses, much like post offices routing mail to the correct location based on the postal address.

### TCP/IP Address Construct

 - IP Address: It's a unique number assigned to devices on a network, expressed as four sets of numbers (octets) separated by dots. Each octet is represented in binary, consisting of 8 bits.

 - Binary: A number system using 0s and 1s, where each digit is a bit. For instance, the decimal number 5 in binary is 101.

 - Octet: A group of 8 bits, which is represented in decimal as part of an IP address.

 - Subnet Mask: A subnet mask helps identify the network portion and host portion of an IP address. When converted to binary, the 1s in the mask indicate the network part, and the 0s indicate the host part.

     For instance:

       IP Address: 192.168.1.0
       Subnet Mask: 255.255.255.0
    The subnet mask in binary (255.255.255.0) shows that the first three octets are for the network, and the last octet is for hosts. Therefore:

        Network: 192.168.1.0
        Usable Hosts: 192.168.1.1 to 192.168.1.254
        Changing the subnet mask alters the network and host range. 
        
        For example:
            Subnet Mask: 255.255.0.0
            Network: 192.168.0.0
            Usable Hosts: 192.168.0.1 to 192.168.255.254
        
    Additionally:

     - IP Address Classes: They categorize addresses based on the range of their first octet, determining the size of the network and hosts available.

     - Private IP Ranges: These reserved ranges (e.g., 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) allow for private networks, conserving public IP addresses. Devices on private networks have these addresses and are translated to public IPs by routers when accessing the internet.

In essence, IP addresses are like postal codes that help devices communicate within a network or across the internet, and subnet masks define how the address space is divided between networks and hosts. The classes and private ranges ensure efficient use of addresses and enable communication within private networks without using public IP addresses for each device.

### Subnet Mask Short-Hand

 - Subnet Mask Shorthand: Instead of writing out the full subnet mask like 255.255.255.0, it can be expressed in shorthand like /24. This number (/24 in this case) represents the count of continuous 1s in the binary subnet mask.

 - Reserved Host Addresses: For every subnet, the first and last IP addresses are reserved. The first IP is the Network ID, and the last is the Broadcast Address, used for sending traffic to all devices on a network.

        For instance, with an IP Address of 192.168.1.1 and a Subnet Mask of 255.255.255.0:

         - Network ID: 192.168.1.0

         - Broadcast Address: 192.168.1.255

         - Usable IPs: 192.168.1.1 - 192.168.1.254

 - Subnet 0: Traditionally, subnet 0 (like 192.168.1.0 in the example) wasn't used due to networking practices. However, in modern Cisco equipment, it's assumed that subnet 0 can be used. Still, in specific exam scenarios, subnet 0 might need to be excluded from calculations, although this is rare in current exams. Always assume the first network can be used unless explicitly instructed otherwise



.
# Practice questions :

### Question : 
 1.  Enter the last valid host on the network that the host 10.135.79.68 255.255.0.0 is a part of : 

     Answer :  
           
            IP Address : 10.135.79.68 (00001010.10000111.01001111.01000100)

            Subnet Mask : 255.255.0.0(/16) (11111111.11111111.00000000.00000000)

            Network : 10.135.0.0  (00001010.10000111.00000000.00000000)

            16 bits available for hosts means 2^16 - 2 = 65534 hosts
            Network address: 10.135.0.0
            Broadcast address: 10.135.255.255
        
            Therefore, the last valid host on the network that includes 10.135.79.68 with a subnet mask of 255.255.0.0 is 
        
                    10.135.255.254.


 2. Enter the last valid host on the network 172.26.92.60/30:

    Answer : 

            For the Network 172.26.92.60/30, which has a subnet mask of 30 bits (32 - 30 = 2)

            The valid host addresses are from 172.26.92.61 to 172.26.92.62



 3. What is the shorthand corresponding to a subnet mask of 255.240.0.0?

    Answer : 

            The subnet mask 255.240.0.0 can be represented in CIDR notation or shorthand as /12.

            This shorthand (/12) signifies that the first 12 bits of the subnet mask are set to 1, resulting in a subnet mask of 255.240.0.0.


 4. Enter the first valid host on the network 10.246.30.32/27:

    Answer : 

            For the network 10.246.30.32/27, the subnet mask is 27 bits, leaving 5 bits for the hosts (32 total - 27 network bits = 5 host bits).

            The increment between hosts in this subnet is 2^(32-27) = 2^5 = 32. So, the valid host addresses will increment by 32.

            The first valid host address on this network would be 10.246.30.33, as 10.246.30.32 is the network address, and the subsequent address is the first usable host.

 5. Enter the broadcast address for the network 192.168.190.128 255.255.255.128:

    Answer : 
            
            For the network 192.168.190.128 with a subnet mask of 255.255.255.128 (or /25 in CIDR notation), the range of IP addresses for hosts is from 192.168.190.129 to 192.168.190.254.

            The broadcast address in this subnet would be the last address in the range, which is 192.168.190.255.

 6. Enter the valid host range for the network that the IP address 172.28.189.16 255.255.248.0 is a part of:
        First Host:	
        Last Host:

    Answer : 

            To find the valid host range for the network with the IP address 172.28.189.16 and a subnet mask of 255.255.248.0:

             Determine the network address:
             Perform a bitwise AND operation between the IP address and the subnet mask.

             IP Address:      172.28.189.16   = 10101100.00011100.10111101.00010000
             Subnet Mask:     255.255.248.0    = 11111111.11111111.11111000.00000000
             -------------------------------------------------------------
             Network Address:                  = 10101100.00011100.10111000.00000000
             So, the network address is 172.28.184.0.

             Determine the number of hosts per subnet:
             Subtract the number of bits set to 1 in the subnet mask from 32 (total number of bits in an IPv4 address).

             Number of Host Bits = 32 - 21 (from the subnet mask) = 11
             This means there are 2^11 - 2 = 2046 host addresses available in each subnet (subtracting 2 for the network and broadcast addresses).

             Calculate the valid host range:
             The valid host range is from the network address + 1 to the broadcast address - 1.

             Network Address:   172.28.184.0
             Broadcast Address: 172.28.191.255
             Valid Host Range:  172.28.184.1 to 172.28.191.254
             Therefore, for the given IP address (172.28.189.16) with a subnet mask of 255.255.248.0:

             First Host: 172.28.184.1
             Last Host: 172.28.191.254     

 7. Enter the subnet the host 10.237.113.206 255.255.128.0 belongs to:

    Answer : 

             To determine the subnet to which the host 10.237.113.206 with a subnet mask of 255.255.128.0 belongs, we'll follow these steps:

             Convert the IP address and subnet mask to binary:

             IP Address:      10.237.113.206   = 00001010.11101101.01110001.11001110
             Subnet Mask:     255.255.128.0    = 11111111.11111111.10000000.00000000
             Perform a bitwise AND operation between the IP address and the subnet mask to find the network address:

             Network Address:                  = 00001010.11101101.00000000.00000000
             So, the network address is 10.237.0.0.

             Determine the subnet range:

             The subnet mask 255.255.128.0 means there are 2^(32-17) = 2^15 = 32768 subnets in total.
             The valid host range in each subnet is from 0 to 2^17 - 2 (since we subtract 2 for the network and broadcast addresses).
             Calculate the subnet to which the host belongs:

             Divide the third octet of the IP address (113) by the number of subnets per block (2^15).

             Subnet Number = 113 / 32768 ≈ 0.00345
             Since the result is less than 1, the host 10.237.113.206 belongs to the first subnet (subnet 0) within the block.

             Therefore, the subnet to which the host 10.237.113.206 with a subnet mask of 255.255.128.0 belongs is 10.237.0.0.

8. Enter the valid host range for the network that the IP address 192.168.17.26 255.255.255.252 is a part of:
   First Host:	
   Last Host:	

    Answer : 

             To find the valid host range for the network with the IP address 192.168.17.26 and a subnet mask of 255.255.255.252, follow these steps:

             Convert the IP address and subnet mask to binary:

             IP Address:      192.168.17.26    = 11000000.10101000.00010001.00011010
             Subnet Mask:     255.255.255.252  = 11111111.11111111.11111111.11111100
             Perform a bitwise AND operation between the IP address and the subnet mask to find the network address:

             Network Address:                  = 11000000.10101000.00010001.00011000
             So, the network address is 192.168.17.24.

             Determine the number of hosts per subnet:

             The subnet mask 255.255.255.252 allows for 2 host addresses in each subnet (excluding network and broadcast addresses).
             Calculate the valid host range:

             The first host is obtained by adding 1 to the network address.
             The last host is obtained by subtracting 1 from the broadcast address.
             Network Address:   192.168.17.24
             First Host:         192.168.17.25
             Last Host:          192.168.17.26
             Therefore, for the given IP address (192.168.17.26) with a subnet mask of 255.255.255.252:

             First Host: 192.168.17.25
             Last Host: 192.168.17.26

 9. What is the Subnet Mask corresponding to a shorthand of /9?

    Answer : 255.128.0.0

             The subnet mask corresponding to a shorthand of /9 is 255.128.0.0 in dotted-decimal notation. In binary form, it is represented as 
             11111111.10000000.00000000.00000000.

             A /9 subnet means that the first 9 bits of the 32-bit IPv4 address are used for the network portion, leaving 23 bits for host addresses within each subnet.

 10. Enter the first valid host on the network that the host 172.30.0.15/23 is a part of:

     Answer :

             To find the first valid host on the network that the host 172.30.0.15/23 is a part of, we can follow these steps:

             Convert the IP address and subnet mask to binary:

             IP Address:      172.30.0.15     = 10101100.00011110.00000000.00001111
             Subnet Mask:     255.255.254.0   = 11111111.11111111.11111110.00000000
             Perform a bitwise AND operation between the IP address and the subnet mask to find the network address:

             Network Address:                  = 10101100.00011110.00000000.00000000
             So, the network address is 172.30.0.0.

             Determine the number of hosts per subnet:

             The subnet mask 255.255.254.0 means there are 2 host addresses available in each subnet (excluding network and broadcast addresses).
             Calculate the first valid host:

             The first valid host is obtained by adding 1 to the network address.

             Network Address:   172.30.0.0
             First Valid Host:  172.30.0.1
             Therefore, the first valid host on the network that the host 172.30.0.15/23 is a part of is 172.30.0.1.








