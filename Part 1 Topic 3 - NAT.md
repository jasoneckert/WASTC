# NAT 
- Typically implemented by NGFW today.
- But you can turn a Windows Server into a NAT router with Remote Access role + RRAS tool.

![image](https://user-images.githubusercontent.com/40586970/170838808-fee08e43-9872-42aa-b80b-e159b869c13a.png)

![image](https://user-images.githubusercontent.com/40586970/170839187-21b84275-e2b6-4dee-a1e4-8cca42d75fce.png)

1. On your Domain Controller virtual machine, install the Remote Access role.
2. Open the RRAS tool and configure your Domain Controller as a NAT router.
3. On your VMA virtual machine, ensure that the default gateway is set to 172.16.0.1 and verify you have Internet access.
