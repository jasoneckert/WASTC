# RADIUS
- Windows RAS servers authenticate users to a DC by default (Windows Authentication)
- Can instead forward to a RADIUS server for central authentication, logging and policies (which can enforce settings/constraints)
- In this case, the VPN server is the “RADIUS client”
- Wireless APs and LAN switches can also be RADIUS clients (802.1X wireless / 802.1X wired) 
  - For 802.1X wireless, the RADIUS server generates the WPA2/3 keys (very strong) for each wireless client that connects to a Wireless Access Point (WAP), ater the wireless client authenticates via RADIUS
  - The WAP is the "RADIUS client" and the wireless client is called the "supplicant"

# Configuring RADIUS on our RAS
- Windows RADIUS is called NPS (Network Policy Server)
- If the RADIUS server is on the same machine as your VPN server, then it’s automatically set up
- If it isn’t:
  - Create a RADIUS client for the VPN server on the RADIUS server (specify a shared secret)
  - Choose RADIUS authentication and logging (alongside the same shared secret) on the RAS

In Server Manager on your DC virtual machine, add the Network Policy and Access Services role.
In Routing and Remote Access tool, right-click your server, click Properties and highlight the Security tab and note that Windows authentication is no longer available because it is automatically configured to use RADIUS on the same machine!
In Server Manager, open the Network Policy Server tool.
Right-click NPS (Local) and notice that your RADIUS server is automatically registered as it is on a DC.
Expand Policies and highlight Network Policies. Note the two default policies.

Right-click Network Policies and click New.
Type Test Remote Access Policy, select Remote Access Server (VPN-Dial up) and click Next.
At the Specify Conditions page, click Add. Click User Groups and click Add. Click Add Groups. Type Domain Admins in the Select Group dialog box, click OK and OK again. Click Next.
At the Specify Access Permission page, note the default option (allow) and click Next.
At the Configure Authentication Methods page, click Add. Select Microsoft: Secure password (EAP-MSCHAP v2) click OK and Next.
At the Configure Constraints page, select Session Timeout, check the Disconnect after the following maximum session time option (default value = 1 minute) and click Next. 
At the Configure Settings page, note the default settings and click Next and Finish. Notice that the new policy is listed with a 1 processing order!
![image](https://user-images.githubusercontent.com/40586970/170846607-885e6857-0dc9-4b79-b088-44df6d64de66.png)
