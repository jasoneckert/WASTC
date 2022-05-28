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

Create the VPN setup shown on the previous slide:
Remove your WINS replication partner configuration and then remove the WINS server service from both your VMA and DC virtual machines. 
Ensure that the IPv4 configuration for your VMA and DC virtual machines no longer list a WINS server.
Remove your DHCP failover configuration and then remove the DHCP server service from your VMA virtual machine. 
Next, remove ALL scopes from the DHCP console on your DC virtual machine. 
Finally, create and activate a new scope in the DHCP console on your Domain Controller virtual machine that hands out an IP range of 172.16.0.100-200, a default gateway of 172.16.0.1, and a DNS server of 172.16.0.1.

Remove any conditional forwarders and secondary zones you created in DNS on your DC virtual machine. Next, remove the DNS server service from your VMA virtual machine.
On your VMA virtual machine, remove your RRAS configuration and then remove the Remote Access role.
On your VMA virtual machine, remove the second network interface that is connected to the second private virtual switch. Change the first network interface so that it connects to your external virtual switch (instead of your first private virtual switch). 
On your VMA virtual machine, change the IPv4 configuration of your network interface to receive a DHCP address, but use a static preferred DNS server. The static preferred DNS server should be the IP address of your DC virtual machine on the external virtual switch (e.g. 192.168.1.107). 

In the Routing and Remote Access tool on your DC virtual machine:
Right-click your server and click Configure and Enable Routing and Remote Access. 
Choose Custom configuration and click Next.
Select VPN access and LAN routing and click Next.
Click Finish and then Start to start your RAS service.

Expand IPv4 and highlight DHCP Relay Agent. Note that the relay agent is not configured to listen to DHCPDISCOVER packets on any interface.
Right-click DHCP Relay Agent and click New Interface. Select the network interface that is on your private virtual switch (NOT the one that just says “Internal”), click OK and OK again.
Right-click DHCP Relay Agent again and click Properties. Type a Server address of 172.16.0.1 and click Add and then OK to ensure that DHCPDISCOVER requests are forwarded to 172.16.0.1.
Right-click Ports in the navigation pane and click Properties. How many ports are configured for each VPN type by default?

Right-click your server in the navigation pane and click Properties. Note that your server is configured for IPv4 routing and remote access by default.
Highlight the Security tab. Note that your server is configured to use Windows Authentication to authenticate clients and then click Authentication Methods. Which authentication methods are enabled by default? Click OK.
Highlight the IPv4 tab. Note that VPN IP configuration will be obtained from a DHCP server (via the DHCP relay agent). 
In Server Manager, open Active Directory Users & Computers 
In the Users folder under your domain, right-click Administrator and click Properties.
Highlight the Dial-in tab, select Allow access and click OK.
Close Active Directory Users and Computers.


On your VMA virtual machine, right-click Start and click Network Connections. In the navigation pane, highlight VPN and click Add a VPN connection.
Select the Windows (built-in) VPN provider, and supply a name of Work VPN in the Connection name text box.
Type the IP address of your DC virtual machine on the external virtual switch (e.g. 192.168.1.107) in the Server name or address box.
Select Point to Point Tunneling Protocol (PPTP) and click Save.
Click Work VPN, Connect. Log in as administrator@lala.com to connect to your VPN (it should say “Connected”). 
Open a command prompt and run Get-NetIPConfiguration. Note that you have two network interfaces: your original one and a new one called “PPP adapter Work VPN” that has an IP on the 172.16.0.0/16 network. Also note that your default gateway is set to 0.0.0.0 for this additional network interface to force traffic through the VPN!

On your DC virtual machine, run Get-NetIPConfiguration. Note that you have your original two interfaces (on the external and private virtual switches), as well as a “PPP adapter RAS (Dial In) Interface” that has an IP address on the  172.16.0.0/16 network for the server side of the VPN tunnel!  
On your VMA virtual machine, type \\IPaddress (where IPaddress is the IP address of your DC virtual machine on the external virtual switch). Note the shared folders that you can access unencrypted. Next, type \\172.16.0.1 to access the same shared folders across the encrypted VPN tunnel. If you can do this, you can also access anything else in your 172.16.0.0 DMZ since the VPN server routed you to that network!
In the Routing and Remote Access tool on your DC virtual machine, highlight Ports. Do you have an active PPTP port? Right-click this port, click Status, note the info shown and then click Disconnect. 
Open the C:\Windows\system32\logfiles\INYYMM.log file in notepad (DD=day of month, MM=month of year). What authentication method was used for your connection?

