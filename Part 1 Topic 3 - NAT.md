# NAT 
- Typically implemented by NGFW today.
- But you can turn a Windows Server into a NAT router with Remote Access role + RRAS tool.

![image](https://user-images.githubusercontent.com/40586970/170838808-fee08e43-9872-42aa-b80b-e159b869c13a.png)

![image](https://user-images.githubusercontent.com/40586970/170839187-21b84275-e2b6-4dee-a1e4-8cca42d75fce.png)


1. In the settings for your Domain Controller VM, add a second network interface on the private virtual switch
2. In Windows Server 2019 on your Domain Controller VM, configure a static IP on the new network interface of 172.16.0.1/16
3. In Hyper-V Manager (`virtmgmt.msc`):
   - Create a new VM called VMA 
     - Generation 2
     - 2GB RAM (non-dynamic)
     - External/default virtual switch
     - Install from Windows Server 2019 ISO
   - In VM Settings, set 2 vCPUs and disable checkpoints
4. Start VMA and install Windows Server 2019 Datacenter (Desktop)
5. On VMA:
- Set the name (VMA), time/zone, and static IP configuration of 172.16.0.1/16, DNS=172.16.0.1, GW=172.16.0.1
- Attempt to browse the Internet as well as ping yahoo.ca (unsuccessful since your Domain Controller is not a router)
- In VM Settings, set 2 vCPUs and disable checkpoints
6. On your Domain Controller:
- Install the Remote Access role (Routing & VPN/DirectAccess components)
- Open the Routing and Remote Access tool and configure your Domain Controller as a NAT router (adding the correct internal and external interfaces)
7. On VMA, veriy that you now can ping yahoo.ca and have Internet access
