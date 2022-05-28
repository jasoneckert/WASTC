# Rules of IP
- Two hosts can directly talk only if they are on the same LAN
- To communicate to a host on another LAN, the packet must be passed through a router (= default gateway)

# IPv4 Addressing 
- Each IP address has a network & host portion
- Subnet mask is used to determine which portion is the network portion 
- e.g., 192.168.1.55 and 255.255.255.0 = host 55 on the 192.168.1 network

# IPv4 Address Classes
- First number (octet) determines the class and default subnet mask
  - 1-126 = Class A (subnet mask 255.0.0.0)
  - 128-191 = Class B (subnet mask 255.255.0.0)
  - 192-223 = Class C (subnet mask 255.255.255.0)
  - 224+ = Class D/E (multicasting/unused)
- 192.168.1.55 is a Class C, and 62.44.6.2 is a Class A

# IPv4 Address Classes
- We often use CIDR notations for subnet masks:
  - 255.0.0.0 = /8 
  - 255.255.0.0 = /16 
  - 255.255.255.0 = /24
- First address = network ID
  - 192.168.1.0 (= 192.168.1.0 network)
- Last address = broadcast ID
  - 192.168.1.255 (= all hosts on 192.168.1.0 network)
- APIPA (can’t get a DHCP address? 169.254.*.*)

# Today, switchs create LANs
![image](https://user-images.githubusercontent.com/40586970/170838591-d6f94439-1745-434b-b80b-169ab2c07535.png)

# In the 90s, hubs (multiport repeaters) created LANs
![image](https://user-images.githubusercontent.com/40586970/170838574-ad8a7133-658c-4ade-8166-5f19bfa11a8a.png)

