# RADIUS
- Windows RAS servers authenticate users to a DC by default (Windows Authentication)
- Can instead forward to a RADIUS server for central authentication, logging and policies (which can enforce settings/constraints)
- In this case, the VPN server is the “RADIUS client”
- Wireless APs and LAN switches can also be RADIUS clients (802.1X wireless / 802.1X wired) 
  - For 802.1X wireless, the RADIUS server generates the WPA2/3 keys (very strong) for each wireless client that connects to a Wireless Access Point (WAP), ater the wireless client authenticates via RADIUS
  - The WAP is the "RADIUS client" and the wireless client is called the "supplicant"

# Configuring RADIUS on our RAS
- Windows RADIUS is called Network Policy and Access Services (NPS)
- If the RADIUS server is on the same machine as your VPN server, then it’s automatically set up
- If it isn’t:
  - Create a RADIUS client for the VPN server on the RADIUS server (specify a shared secret)
  - Choose RADIUS authentication and logging (alongside the same shared secret) on the RAS

1. On your Domain Controller, add the Network Policy and Access Services (NPS) role
2. View the Security tab of the server properties in the Routing and Remote Access tool and note it is automatically configured to forward requests to the local RADIUS server
3. Open the Network Policy Server tool
   - View the 2 default policies under Policies, Network Policies
   - Create a new Network Policy called VPN connections with the following properties:
     - Type of "Remote Access Server (VPN-Dial up)" 
     - Conditions and Access Permission that allow the Domain Admins user group to connect
     - Authentication Methods that also include "Microsoft: Secure password (EAP-MSCHAP v2)"
     - Session Timeout of 1 minute (for testing only)
4. On VMA, disconnect and reconnect your VPN and note that you are disconnected automatically after 1 minute.
5. On your Domain Controller, view the contents of the C:\Windows\System32\LogFiles\IN*.log to view your connections.

