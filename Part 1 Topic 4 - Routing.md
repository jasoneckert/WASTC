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

# Routing table practice (write the routing table entries for each router)
![image](https://user-images.githubusercontent.com/40586970/170840544-fdca5b8b-a34e-4c66-b1cb-bcdf68515bec.png)

# Implementing routing
![image](https://user-images.githubusercontent.com/40586970/170840558-65f44d2c-3145-456d-a907-d428f083a310.png)

1. In Hyper-V Manager, add another private virtual switch
2. In the properties of your VMA virtual machine, add a second network interface on the second private virtual switch
3. In Windows Server 2019 on your VMA virtual mahine, configure a static IP of 10.0.0.1/8 on the second network interface
4. Add the Remote Access role (Routing & VPN/DirectAccess) to VMA
5. In Routing and Remote Access on your Domain Controller, unconfigure and reconfigure your system as a regular router
6. In Routing and Remote Access on VMA, configure your system as a regular router
7. Create the necessary routes to the 192.168.1.0/24 and 10.0.0.0/8 networks and ensure that hosts can ping back and forth
8. Delete your static routes & add RIP (add interface for the DMZ network only) - note that the routes are re-added by RIP & ping back and forth to test
