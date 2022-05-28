# Rules of IP:
- Two hosts can directly talk only if they are on the same LAN
- To communicate to a host on another LAN, the packet must be passed through a router (= default gateway)

# IPv4 Addressing 
- Each IP address has a network & host portion
- Subnet mask is used to determine which portion is the network portion 
	`192.168.1. .55` 
  `255.255.255.0`
	= host 55 on the 192.168.1 network


# IPv4 Address Classes
- First number (octet) determines the class and default subnet mask
  - 1-126 = Class A (subnet mask 255.0.0.0)
  - 128-191 = Class B (subnet mask 255.255.0.0)
  - 192-223 = Class C (subnet mask 255.255.255.0)
  - 224+ = Class D/E (multicasting/unused)

  192.168.1.55 is a Class C
  62.44.6.2 is a Class A

