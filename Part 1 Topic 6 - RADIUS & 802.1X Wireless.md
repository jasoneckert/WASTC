# RADIUS
- Windows RAS servers authenticate users to a DC by default (Windows Authentication)
- Can instead forward to a RADIUS server for central authentication, logging and policies (which can enforce settings/constraints)
- In this case, the VPN server is the “RADIUS client”
- Wireless APs and LAN switches can also be RADIUS clients (802.1X wireless / 802.1X wired)

# Configuring RADIUS on our RAS
- Windows RADIUS is called NPS (Network Policy Server)
- If the RADIUS server is on the same machine as your VPN server, then it’s automatically set up
- If it isn’t:
  - Create a RADIUS client for the VPN server on the RADIUS server (specify a shared secret)
  - Choose RADIUS authentication and logging (alongside the same shared secret) on the RAS

