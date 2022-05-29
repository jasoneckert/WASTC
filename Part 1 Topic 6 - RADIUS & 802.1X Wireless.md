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
3. Open the Network Policy Server console
   - View the 2 default policies under Policies, Network Policies
   - Create a new Network Policy called VPN connections with the following properties:
     - Type of "Remote Access Server (VPN-Dial up)" 
     - Conditions and Access Permission that allow the Domain Admins user group to connect
     - Authentication Methods that also include "Microsoft: Secure password (EAP-MSCHAP v2)"
     - Session Timeout of 1 minute (for testing only)
4. On VMA, disconnect and reconnect your VPN and note that you are disconnected automatically after 1 minute.
5. On your Domain Controller, view the contents of the C:\Windows\System32\LogFiles\IN*.log to view your connections.

# Configuring RADIUS for 802.1X Wireless
1. On your Domain Controller:
   - Add the Active Directory Certificate Services role (only the Certification Authority feature) and complete the post installation task with the default options to configure your Domain Controller as a Certification Authority
   - Open the Certification Authority console, expand your server, right-click the Certificate Templates folder and choose Manage
     - Double-click the RAS & IAS Server template
     - On the Security tab, give each user/group listed Read and Enroll permission at minimum (don’t remove any existing permissions)
     - Close the Certificate Templates window
   - Right-click the Certificate Templates folder, choose New > Certificate Template to Issue, and select RAS & IAS Server
   - At a command prompt or PowerShell window, type `certlm.msc`
     - Expand Personal, right-click the Certificates folder and choose All Tasks > Request New Certificate
     - Complete the wizard (select the RAS & IAS certificate template)
   - Open the Network Policy Server console 
     - Right-click NPS (Local) at the top and click Register server in Active Directory
     - Highlight NPS (Local) and select RADIUS server for 802.1X Wireless or Wired Connections and Configure 802.1X 
     - Complete the wizard using the following parameters:
        - Select Secure Wireless Connections
        - Add a new RADIUS client for a WAP on the LAN (supply sample info and a shared secret)
        - Select "Microsoft: Protected EAP (PEAP)" authentication
2. On your WAP, configure the Wireless Security section to use the following parameters:
   - WPA2/3 (AES)
   - Enteprise/RADIUS 
   - IP address of RADIUS server 
   - Shared secret 
