# Every device that has the IP protocol MUST have a route table
- Defines where to go, how to get there
- For VMA:
  - 172.16.0.0/16, 172.16.0.2
  - 0.0.0.0, 172.16.0.1
- For Domain Controller:
  - 172.16.0.0/16, 172.16.0.1
  - 192.168.1.0/24, 192.168.1.107
  - 0.0.0.0, 192.168.1.1

- Routers have the same routes as a host computer has as well as any additional routes for internal networks behind other routers
  - Added statically or dynamically (e.g. RIP)
  - RIPv1 (broadcast) vs RIPv2 (multicast)
  - Password protect it!
