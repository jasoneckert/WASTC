# Virtual Switches in Hyper-V:
![image](https://user-images.githubusercontent.com/40586970/170838173-0cc0b4d3-446f-48b9-aec9-2cb17879f983.png)

- Default Virtual Switch on Win11 = virtual NAT router

# Steps:
1. Download the ISO for Windows Server 2019 (Datacenter) from https://portal.azure.com (we don’t need the key).

2. Ensure that your Win10 host has Hyper-V installed. If not, upgrade to Win10 Pro or Education (https://portal.azure.com).

3. In Hyper-V Manager (`virtmgmt.msc`):
- Create an external, internal & private virtual switch
- Create a new VM called Server2019 
  - Generation 2
  - 2GB RAM (non-dynamic)
  - External/default virtual switch
  - Install from Windows Server 2019 ISO
- In VM Settings, set 2 vCPUs and disable checkpoints.
- Start the VM and install Windows Server 2019 Datacenter (Desktop)
