VPN servers authenticate users to a DC by default (Windows Authentication)
Can instead forward to a RADIUS (NPS) server for central authentication, logging and policies

In this case, the VPN server is the “RADIUS client”

Wireless APs and LAN switches can also be RADIUS clients (802.1x wireless / 802.1x wired)


If the RADIUS server is on the same machine as your VPN server, then it’s automatically set up

If it isn’t;
Create a RADIUS client for the VPN server on the RADIUS server (Secret555)
Choose RADIUS authentication (:1812/1645), logging (:1813/1646) and Secret555 on the VPN server

RADIUS policies:
Connection request policy 
Does this RADIUS server authenticate, or do we forward it to a head office RADIUS server? (RADIUS server group)

Network connection policy 
You apply the first one you match with conditions
Can include the remote access permission
You must satisfy the restrictions (constraints)
Optional settings can also be handed out
![image](https://user-images.githubusercontent.com/40586970/170841382-10937cfa-09b4-4fbd-a8fc-8062f16daa7f.png)
