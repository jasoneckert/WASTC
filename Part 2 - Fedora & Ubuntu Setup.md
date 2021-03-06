# Installing Fedora Workstation & Obtaining Sample Files

1. On your Windows or macOS computer, download the latest ISO image for Fedora Workstation from getfedora.org.
2. Install a free hypervisor on your Windows or macOS system. 
   - For macOS on the Apple M1 platform, you can use UTM/Qemu (mac.getutm.app). 
   - For Windows or macOS on the x64 platform, Oracle Virtualbox (www.virtualbox.org/wiki/Downloads) is recommended, 
     but you can instead use the native Hyper-V hypervisor on Windows. 
   - Refer to the free documentation on the hypervisor website to learn how to create a virtual machine using the hypervisor software.
3. In your hypervisor software, create a new virtual machine called Fedora Workstation that has 4GB of RAM, 64GB of virtual hard disk storage, 
   and a connection to the Internet via the network interface in your computer. 
4. Insert the ISO image for Fedora into the virtual DVD of this virtual machine.
5. Start and then connect to your Fedora Workstation virtual machine. 
6. Press Enter to test your media and boot Fedora Live. 
7. Once the graphical desktop has loaded, select the option Install to Hard Drive from the bar at the bottom of the screen.
8. Select English (United States) and press Continue.      
9. If the time zone shown is incorrect, click Time & Date, choose the correct time zone (e.g. Americas/Toronto) and press Done.      
10. Click Installation Destination.  
    - You should see that your 64GB virtual disk is already selected and called sda (or vda if using UTM/Qemu). 
    - Select Custom and press Done when finished. This will allow you to manually configure your partitions.
    - Choose a partitioning scheme of Standard Partition from the drop-down menu and click Click here to create them automatically. 
    - Highlight the "/home (sda5)" partition, reduce the Desired Capacity to 10GB and click Update Settings. This will leave some space for future use. 
    - Press Done and then click Accept Changes when prompted.
11. Click Begin Installation. When the installation has finished, click Finish Installation. 
12. Click the icon in the upper right corner and choose Power Off / Log Out > Power Off and click Power Off to shut down your Fedora Live installation.
13. In the settings for your virtual machine, remove the Fedora ISO image from the virtual DVD and start your Fedora Workstation virtual machine.
14. On the first boot after installation, you must complete a Welcome wizard. Click Start Setup at this wizard and make the following selections when prompted:
    - Disable Location Services and Automatic Problem Reporting
    - Skip associating your user with online accounts
    - Create a regular user called woot with a password of Secret555
15. At the Welcome to GNOME screen, click No Thanks when finished, and close the Getting Started window.
16. Open a terminal app by navigating to Activities > Show Applications (icon with 9 circles on bar at the bottom of the screen) > Terminal. 
17. Type "sudo passwd root" at the command prompt and press Enter. 
    - Enter the password for your woot user account (Secret555)
    - Next, enter a password of Secret555 for the root user when prompted (twice).
18. Type `su - root` and press Enter. 
    - Supply the root user???s password of Secret555 when prompted. 
    - Type `git clone https://github.com/jasoneckert/classfiles.git` and press Enter. 
    - Type `poweroff` to shut down your Fedora virtual machine.

# Installing Ubuntu Server (this can be done later in Topic 5)

1. On your Windows or macOS computer, download the latest ISO image for Ubuntu Server from ubuntu.com/download/server. 
2. In your hypervisor software, create a new virtual machine called "Ubuntu Server" that has 1GB of RAM, 64GB of virtual hard disk storage, 
   and a connection to the Internet via the network interface in your computer. 
3. Insert the ISO image for Ubuntu into the virtual DVD of this virtual machine.
4. Start and then connect to your Ubuntu Server virtual machine. 
5. At the Welcome screen, ensure that English is selected, and press Enter.
6. At the Keyboard configuration screen, select English (US) and select Done. 
7. At the Ubuntu welcome screen, note the options, ensure that Install Ubuntu is selected, and press Enter.
8. At the Network connections screen, ensure that your network interface is set to DHCP and select Done.
9. At the Configure proxy screen, select Done.
10. At the Configure Ubuntu archive mirror screen, select Done.
11. At the Filesystem setup screen, ensure that Use an entire disk and Set up this disk as an LVM group are selected (the default), and select Done. 
    - The installation program creates a volume group called ubuntu-vg that contains a single logical volume called ubuntu-lv mounted to /. 
    - Navigate the menus using your cursor keys to ensure that this ubuntu-lv uses all remaining space in the volume group (formatted using ext4) 
      and select Done and Continue when finished.
12. At the Profile setup screen, supply the following and select Done when finished:
    - Your name = woot	
    - Your server???s name = ubuntu
    - Username = woot	
    - Password = Secret555
13. At the SSH Setup, select Install OpenSSH server and select Done.
14. At the Featured Server Snaps screen, select Done.
15. At the Install complete screen, select Reboot Now. When prompted to remove your installation medium, press Enter.
16. After the system has booted, log in as woot, type `sudo passwd root` and set the root user password to Secret555. 
17. Type `poweroff` to shut down your Ubuntu Server virtual machine.
