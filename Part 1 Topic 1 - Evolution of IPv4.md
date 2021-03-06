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

# To segment your 90s LAN, we invented subnetting
![image](https://user-images.githubusercontent.com/40586970/170838652-9e9ae16a-db20-4e75-b5ef-63f632a28c91.png)

![image](https://user-images.githubusercontent.com/40586970/170838666-ecb97002-fe22-44c1-9b6a-6682e2a2e2ad.png)

![image](https://user-images.githubusercontent.com/40586970/170838672-a6967a98-dac8-4fdc-a417-810d129f6bdf.png)

![image](https://user-images.githubusercontent.com/40586970/170838683-48751f38-1423-4838-8654-0d9decfba4da.png)

# As the Internet grew, the number of public IPv4 networks wouldn't suffice - so we separated a public Internet from a private (reusable) Internet using proxy servers & NAT routers  
- Private LANs inside organizations could use these IPv4 networks:
  - 10.0.0.0 /8
  - 172.16-31.0.0 /16
  - 192.168.0-255.0 /24
- ISPs retained all other public networks, and typically allocated a single public IP to an organization for use on the external side of their proxy server or NAT router.
- Can also have NAT behind NAT.

![image](https://user-images.githubusercontent.com/40586970/170838808-fee08e43-9872-42aa-b80b-e159b869c13a.png)




