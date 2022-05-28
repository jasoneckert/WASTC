# Remote Access (RDP, SSH, Web portal, VPN, etc.)
- Allows you to access the resources in your DMZ and corporate LANs from outside
- Outside clients connect to a Remote Access Server (RAS)
- We'll focus on VPN remote access (RAS = VPN server)

![image](https://user-images.githubusercontent.com/40586970/170840812-683b73d0-ac33-4e2f-a6cd-da6f97fa3a3d.png)

# RAS can located within or on the edge of the DMZ 
![image](https://user-images.githubusercontent.com/40586970/170840892-1a7f077e-298d-476e-803a-437f3977e82c.png)

![image](https://user-images.githubusercontent.com/40586970/170840896-06ef1392-9e66-4208-8136-6359c83bbd40.png)

# Implementing a Microsoft VPN RAS
![image](https://user-images.githubusercontent.com/40586970/170840936-8a88c12b-df67-4ee5-b232-815b08d95aaa.png)

1. On VMA, remove the Remote Access role
2. In the settings for your VMA VM, remove the second network interface connected to the second private virtual switch, and change the first network interface so that it connects to your external virtual switch (instead of your first private virtual switch)
3. In Windows Server 2019 on VMA, change the IPv4 configuration of your network interface to receive a DHCP address
4. In the Routing and Remote Access tool on your Domain Controller:
   - Unconfigure and reconfigure your system for VPN access and LAN routing
   - In the properties of your server object, ensure that the RAS server hands out IPv4 addresses 172.16.100.1-172.16.100.254 on the VPN
   - Optionally examine authentication methods and VPN types/ports 
5. In Active Directory Users & Computers on your Domain Controller, ensure that Allow access is selected on the Dial-in tab of Administrator's user properties
6. On VMA, add a new VPN connection in the Settings app using the following parameters:
   - Windows (built-in) VPN provider
   - Name = Work VPN 
   - Server name or address = IP of the Domain Controller on the external virtual switch (e.g. 192.168.1.107) 
   - VPN type = Point to Point Tunneling Protocol (PPTP) 
7. Connect to your VPN (sign in as Administrator in your domain).
8. On VMA, examine your IP configuration using `ipconfig` or `Get-NetIPConfiguration` (note that you have two network interfaces: your original one and a new one called “PPP adapter Work VPN” that has an IP on the 172.16.0.0/16 network. Also note that your default gateway is set to 0.0.0.0 for this additional network interface to force traffic through the VPN)
9. Repeat the previous step on your Domain Controller
10. In Routing and Remote Access on your Domain Controller, view your active connection
11. On VMA
   - Connect to \\IPaddress (where IPaddress is the IP address of your Domain Controller on the external virtual switch) - note the shared folders that you can access unencrypted
   - Connect to \\172.16.0.1 (in your DMZ) to access the same shared folders across the encrypted VPN tunnel
