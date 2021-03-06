# Network Interfaces & Services
  * Open a Terminal on your Fedora Workstation virtual machine as root
  - `ifconfig` (note the IP address you received from DHCP)
  - `ip addr show`
  - `nmcli conn show`  
  - `cat /etc/NetworkManager/system-connections` (note that the folder is empty)
  * Log into GNOME desktop as the woot user and navigate to Activities > Show Applications > Settings > Network 
  * Next, modify your network interface to use a static configuration for your network and Apply your changes 
  - `ls /etc/NetworkManager/system-connections` (note the name.nmconnection file)
  - `cat /etc/NetworkManager/system-connections/name.nmconnection` 
  - `nmcli connection down "name" ; nmcli connection up "name"`
  - `nmcli conn show`
  - `ifconfig` (note the new IP address for future exercises)
  - `ip addr show`
  - `ping -c 5 www.yahoo.ca`
  - `hostnamectl set-hostname fedoraworkstation` 
  - `hostname`
  - `cat /etc/hostname`
  - `bash` (new shells will indicate the new hostname)
  - `cat /etc/resolv.conf` (note the redirection to Systemd-resolved)
  - `resolvectl status`
  - `vi /etc/hosts`	(add the line below)
     - `1.2.3.4  fakehost.fakedomain.com  fakehost`
  - `host fakehost.fakedomain.com`
  - `resolvectl query fakehost.fakedomain.com` 
  - `nslookup fakehost.fakedomain.com`
  - `dig fakehost.fakedomain.com`
  - `host www.yahoo.ca`
  - `resolvectl query www.yahoo.ca`
  - `nslookup www.yahoo.ca`
  - `dig www.yahoo.ca`
  - `ip route` (note the two routes for your network and gateway)
  - `traceroute www.yahoo.ca`
  - `tracepath www.yahoo.ca`
  - `mtr www.yahoo.ca` (press `q` to quit)
  - `grep -i HTTP /etc/services | less`
  - `nmap -sT localhost` (note the default services started, and the ports they listen on)
  - `dnf install telnet-server telnet`
  - `systemctl start telnet.socket ; systemctl enable telnet.socket`
  - `nmap -sT localhost` (note the telnet service listening to port 23/tcp, on demand)
  - `telnet localhost` (log in as the root user)
     - `who` (note that you are logged in remotely via a pseudo terminal, pts/0)
     - `ss -t` (note the established telnet socket on your system ??? after initially connecting on port 23/tcp, communication is redirected to a random port above 32768 for the session duration)
     - `exit` (quits your telnet session to localhost)
  - `who` (note that your pseudo terminal is no longer present)
  - `ss -t` (note that your telnet socket is no longer present)
  * Open a Terminal on your Ubuntu Server virtual machine as root
  - `ifconfig` (note the DHCP address assigned for future exercises)
  - `ip addr show`
  - `networkctl`
  - `cat /etc/netplan/00-installer-config.yaml`
  - `hostname`
  - `cat /etc/hostname`
  - `ping -c 5 www.yahoo.ca`
  - `nmap -sT localhost` (we???ll be adding several additional services later)

# SSH
  * Open a Terminal on your Fedora Workstation virtual machine as root
  - `ssh woot@UbuntuIPaddress` (accept the SSH host key and supply the woot user???s password when prompted)
     - `su -` (supply the root user???s password when prompted)
     - `who` (note that you are connected via a remote pseudo terminal session) 
     - `exit`
  - `ssh-keygen` (press `[Enter]` at each prompt to use default settings)
  - `ssh-copy-id -i woot@UbuntuIPaddress`
  - `ssh woot@UbuntuIPaddress` (note that you are not prompted to supply a password!)
     - `exit`
  - `ssh woot@UbuntuIPaddress cat /etc/hosts > downloadedhosts`
  - `cat downloadedhosts`
  * If your host OS is Windows, download and install the Putty Windows SSH client (putty.exe) from the following website: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html. 
  * Next, run the putty.exe executable and connect to the IP address of your Ubuntu Server using SSH on port 22. Accept the SSH host key and log in as the woot user 
    - `who`
    - `exit`
  * If your host OS is macOS, navigate to Applications > Utilities > Terminal to open a shell on your local system 
    - `ssh woot@UbuntuIPaddress` (accept the SSH host key and supply woot???s password when prompted)
    - `who`
    - `exit`
    
# FTP
  * Open a Terminal on your Ubuntu Server virtual machine as root
  - `apt install vsftpd` 
  - `cp -f /etc/hosts ~woot/file1` 
  - `cp -f /etc/hosts ~woot/file2` 
  - `cp -f /etc/hosts /srv/ftp/file3`
  - `vi /etc/vsftpd.conf` (view the comments and ensure the following lines are uncommented, saving your changes)
     - `anonymous_enable=YES` 
     - `write_enable=YES`
  - `systemctl restart vsftpd.service`
  * Open a Terminal on your Fedora Workstation virtual machine as root
  - `ftp UbuntuIPaddress` (log in as the woot user when prompted)
     - `dir` (note that file1 and file2 are present)
     - `lcd /etc` (changes the directory on Fedora to /etc)
     - `put hosts` (uploads the hosts file to woot???s home directory on the Ubuntu server)
     - `dir` 
     - `lcd /` (changes the directory on Fedora to /)
     - `get hosts` (downloads the hosts file from the Ubuntu server to / on Fedora)
     - `mget file*` (press y when prompted to download the file1 and file2)
     - `help` 
     - `bye`
  - `ls /` (note that hosts, file1, and file2 were downloaded successfully)
  - `ftp UbuntuIPaddress` (log in as the anonymous user when prompted)
     - `dir` (note that file3 is present)
     - `lcd /`
     - `get file3`
     - `put file2` (note that you receive a permission error!)
     - `bye` 
  * On your Windows or macOS host OS, open a Web browser and navigate to ftp://UbuntuIPaddress and note that you can see and download file3 
  * On your Windows or macOS PC, download and install the FileZilla FTP client program from https://filezilla-project.org/ 
  * Next, open FileZilla and connect to sftp://UbuntuIPaddress as the woot user and practice some graphical file transfers 
   
# NFS
  * Open a Terminal on your Ubuntu Server virtual machine as root
  - `apt install nfs-kernel-server` 
  - `vi /etc/exports` (add the following line, saving your changes)
    - `/etc *(rw)` 
  - `exportfs -a` 
  - `systemctl restart nfs-kernel-server`
  - `showmount ???e localhost` 
  - `mount ???t nfs localhost:/etc /mnt` 
  - `df -hT`
  - `cat /mnt/hosts` (note that you are viewing /etc/hosts on the remote system via NFS)
  - `umount /mnt` 
  * Open a Terminal on your Fedora Workstation virtual machine as root
  - `dnf install nfs-utils` (this installs the NFS client utilities if not already installed)
  - `mount ???t nfs IPaddress:/etc /mnt` (where IPaddress is your Ubuntu Server IP address)
  - `df -hT` 
  - `ls /mnt`
  - `cat /mnt/hosts`
  - `umount /mnt` 
  - `dnf install sshfs` (SSHFS is an NFS-like implementation using SSH)
  - `sshfs woot@IPaddress:/etc /mnt` (where IPaddress is your Ubuntu Server IP address)
  - `df -hT`
  - `ls /mnt`
  - `cat /mnt/hosts` 
  - `umount /mnt`

# Samba
  * Open a Terminal on your Ubuntu Server virtual machine as root
  - `apt install samba smbclient cifs-utils`
  - `ps ???ef | grep mbd` (note that smbd and nmbd are running)
  - `vi /etc/samba/smb.conf` 
     * View the comments and then add the following line under the `workgroup = WORKGROUP` line
       - `netbios name = ubuntu`
     * Uncomment/modify the section that shares out all home directories
       - `[homes]`
       - `comment = Home Directories`
       - `browseable = yes`
       - `read only = no`
     * Add the following share definition to the bottom of the file, save your changes and quit vi
       - `[etc]`
       - `comment = The etc directory`
       - `path = /etc`
       - `guest ok = yes`
       - `read only = yes`
       - `browseable = yes`
  - `testparm` (if you made an error, re-edit the /etc/samba/smb.conf file to fix it)
  - `systemctl restart smbd.service ; systemctl restart nmbd.service` 
  - `smbpasswd ???a root` (supply Secret555 when prompted)
  - `smbpasswd ???a woot` (supply Secret555 when prompted)
  - `pdbedit ???L` (note that both root and woot have Samba passwords)
  - `nmblookup ubuntu` (note that your NetBIOS name resolves to your IP address)
  - `smbclient ???L ubuntu` (note your shares shown, including the root home directory)
  - `mount ???t cifs //ubuntu/root /mnt`
  - `df -hT`
  - `ls -a /mnt` (note your home directory contents)
  * If your host OS is Windows, open File Explorer and enter `\\ubuntu` in the search bar - next, open PowerShell as Administrator and run `net use z: \\ubuntu\woot /user:woot Secret555` (access your Z:\ mapped drive afterwards in File Explorer)
  * If your host OS is macOS, open the Finder app and navigate to Go > Connect to Server, enter `smb://ubuntu` and click Connect (note the connection in your Finder)

# Apache
  * Open a Terminal on your Ubuntu Server virtual machine as root
  - `apt install apache2`
  - `systemctl enable apache2.service`
  - `systemctl status apache2.service`
  - `ps -ef | grep apache` (note that additional Apache daemons run as www-data)
  - `grep RUN /etc/apache2/envvars` (note that the default user and group are www-data)
  - `grep ^User /etc/apache2/apache2.conf`
  - `grep ^Group /etc/apache2/apache2.conf`  
  - `grep LOG /etc/apache2/envvars` (note that logs are stored in /var/log/apache2)
  - `grep ErrorLog /etc/apache2/apache2.conf`  
  - `grep LogLevel /etc/apache2/apache2.conf`
  - `cat /var/log/apache/error.log`
  - `grep LogFormat /etc/apache2/apache2.conf` (note the log formats defined)
  - `ls -l /etc/apache2/sites-enabled` (note the symlink to ../sites-available/000-default.conf)
  - `less /etc/apache2/sites-available/000-default.conf` (note the default virtual host that listens for HTTP requests on port 80 and serves Web content from the /var/www/html document root directory, logging each hit to access.log using a combined log format)
  - `less /etc/apache2/apache2.conf` (note the <Directory /> and <Directory /var/www/> block directives that deny access to / and allow access to /var/www/ and its subdirectories)
  - `less /var/www/html/index.html` 
  - `curl http://localhost | less` (note the same HTML webpage)
  * Open a Web browser on your Windows or macOS PC and navigate to http://UbuntuIPaddress to view your webpage
  - `ls -l /var/www/html/index.html` (note the owner/group = root, and the mode = 644)
  - `chmod 640 /var/www/html/index.html` (refresh the webpage in your Web browser and note the Forbidden (HTTP 403) error) 
  - `chgrp www-data /var/www/html/index.html` (refresh the webpage in your Web browser and note the webpage displays properly) 
  - `cat /var/log/apache2/access.log` 
  - `ab -n 1000 -c 100 http://localhost/ `
  - `useradd ???m shrek`
  - `mkdir /home/shrek/public_html` 
  - `vi /home/shrek/public_html/index.html` (add the following, saving changes)
     - `<html><body><h1> Welcome to Shreks Homepage!</h1></body></html>`
  - `cat /etc/apache2/mods-available/userdir.conf` 
  - `ls -l /etc/apache2/mods-enabled` (note the absence of a shortcut to userdir.conf)
  - `a2enmod userdir`
  - `systemctl restart apache2.service`
  - `ls -l /etc/apache2/mods-enabled` (note the shortcut to userdir.conf)
  * Open a Web browser on your Windows or macOS PC and navigate to http://UbuntuIPaddress/~shrek/ to view shrek???s webpage
  - `vi /etc/apache2/apache2.conf` (add the following line to the end, saving changes)
     - `alias /donkey/  /home/shrek/public_html/`
  - `apachectl graceful` 
  * Open a Web browser on your Windows or macOS PC and navigate to http://UbuntuIPaddress/donkey/ to view shrek???s webpage
  - `mkdir /var/www/html2`
  - `vi /var/www/html2/index.html` (add the following, saving changes)
     - `<html><body><h1>It works!</h1></body></html>`
  - `vi /etc/apache2/sites-available/000-default.conf` (uncomment the `ServerName www.example.com` line and add the following lines to the end of the file, saving changes)
     - `<VirtualHost *:80>`
     - `ServerName www.example2.com`
     - `ServerAdmin webmaster@localhost`
     - `DocumentRoot /var/www/html2`
     - `ErrorLog ${APACHE_LOG_DIR}/error.log`
     - `CustomLog ${APACHE_LOG_DIR}/access.log referer`
     - `</VirtualHost>`
  - `systemctl restart apache2.service`
  * If your host OS is Windows, add the following line to the bottom of the C:\Windows\System32\drivers\etc\hosts file
    - `UbuntuIPaddress www.example.com www.example2.com`
  * If your host OS is macOS, add the following line to the bottom of your /etc/hosts file (using `sudo /etc/hosts` at a Terminal prompt) 
    - `UbuntuIPaddress www.example.com www.example2.com`
  * Open a Web browser on your Windows or macOS PC and navigate to http://www.example.com to view the webpage from /var/www/html/
  * Next, navigate to http://www.example2.com to view the webpage from /var/www/html2/

# PostgreSQL
  * Open a Terminal on your Ubuntu Server virtual machine as root
  - `apt install postgresql` 
  - `passwd postgres` (specify Secret555 when prompted)
  - `su - postgres` 
  - `createdb quarry`  
  - `psql quarry` 
     - `CREATE TABLE employee (Name char(20),Dept char(20),jobTitle char(20));`
     - `INSERT INTO employee VALUES ('Fred Flintstone','Quarry','Digger');`
     - `INSERT INTO employee VALUES ('Wilma Flintstone','Finance','Analyst');`
     - ` INSERT into employee VALUES ('Barney Rubble','Sales','Neighbor');`
     - `INSERT INTO employee VALUES ('Betty Rubble','IT','Neighbor');`
     - `SELECT * from employee;`
     - `\d`
     - `\d employee`
     - `\l`
     - `\q`

# DNS
  * Open a Terminal on your Fedora Workstation virtual machine as root
  - `dnf install bind`
  - `curl http://triosdevelopers.com/jason.eckert/trios/example.com.dns --output /var/named/example.com.dns` 	
  - `cat /var/named/example.com.dns` 
  - `chmod 644 /var/named/example.com.dns` 
  - `curl http://triosdevelopers.com/jason.eckert/trios/named.conf.additions --output named.conf.additions` 
  - `vi /etc/named.conf`
     * Go to the end of the file and type `:r named.conf.additions` 
     * Ensure that the last line (the `forwarders` option for 8.8.8.8) is added to the options block near the top of the file (remove the word `options` when adding it to this block), and then comment this last line 
     * Save your changes and quit vi
  - `systemctl start named.service ; systemctl enable named.service` 
  * Log into the GNOME desktop environment and set a static DNS server of 127.0.0.1 for your network interface in the Network tool
  - `nmcli connection down "name"` (where name is the name of your network interface)
  - `nmcli connection up "name"` (where name is the name of your network interface)
  - `nslookup server1.example.com` (note the successful name resolution)
  - `nslookup www.yahoo.com` (this worked because of the forwarder configured)
  - `vi /var/named/example.com.dns` 	(create several resource records of your choice, saving changes)
  - `systemctl restart named.service` 
  - `dig @localhost example.com ANY` (note that your new records are shown)

# DHCP
  * Open a Terminal on your Fedora Workstation virtual machine as root
  - `dnf install dhcp` 
  - `vi /etc/dhcp/dhcpd.conf` (provide an IPv4 configuration for your network using the syntax at https://docs.fedoraproject.org/en-US/Fedora/14/html/Deployment_Guide/s1-dhcp-configuring-server.html)
  - `systemctl start dhcpd.service` 
  - `ps ???ef | grep dhcpd` (if dhcpd is not present, you made a typo in dhcpd.conf)
  - `firewall-cmd --add-service dhcp` (allows DHCP in your firewall, discussed later)
  - `firewall-cmd --add-service dhcp --permanent` 
  * Open a Terminal on your Ubuntu Server virtual machine as root
  - `ifconfig name down ; dhclient name ; ifconfig name up` (where name is your interface name) 
  * Open a Terminal on your Fedora Workstation virtual machine as root
  - `cat /var/lib/dhcpd/dhcpd.leases` (note the IP lease information)

# NTP
  * Open a Terminal on your Fedora Workstation virtual machine as root
  - `less /etc/chrony.conf` (note the default pool line for NTP servers)
  - `vi /etc/chrony.conf` (add the `allow subnet` line, where subnet is your local IP subnet ??? e.g. 192.168.1/24)
  - `systemctl restart chronyd.service` 
  - `firewall-cmd --add-service ntp` (allows NTP in your firewall, discussed later)
  - `firewall-cmd --add-service ntp --permanent` 
  - `chronyc sources -v` (note the NTP servers queried for time) 
  * Open a Terminal on your Ubuntu Server virtual machine as root
  - `apt install ntp`
  - `vi /etc/ntp.conf` (add this line before other pool entries: `pool FedoraIP iburst`)
  - `systemctl restart ntp.service`
  - `ntpq -p` (note your Fedora time server listed) 

# Security Basics
  * Open a Terminal on your Fedora Workstation virtual machine as root
  - `nmap -sT localhost` (note that telnet is not necessary)
  - `systemctl stop telnet.socket ; systemctl disable telnet.socket`
  - `nmap -sT localhost` (note that your attack surface is now lower)
  - `tcpdump -i eth0`	(press `[Ctrl]+c` after a few minutes)
  - `ulimit -u`	(note the default value) 
  - `ulimit -u 100 ; ulimit -u`
  - `less /etc/security/limits.conf` (read the comments and example lines)
  - `dnf install expect` 
  - `mkpasswd-expect ???l 12 ???d 1 ???c 1 ???C 1 ???s 1 -v` (note the strong password generated)
  - `who /var/log/wtmp`
  - `last`
  - `lastb`
  - `gpg --gen-key`	(supply your email and Secret555 passphrase)
  - `cd classfiles`
  - `gpg --encrypt --sign -r email letter`
  - `file letter* ; head -1 letter*`
  - `gpg letter.gpg`	(choose to overwrite letter)
  - `head -1 letter`
  - `poweroff`
  * In the Settings of your virtual machine, add an additional 8GB virtual hard disk (/dev/sdc) to your Fedora Workstation virtual machine and start it. 
  - `fdisk /dev/sdc` (create a single partition that space the entire disk)
  - `cryptsetup luksFormat /dev/sdc1`	(specify lUkS-555 as the passphrase)
  - `cryptsetup luksOpen /dev/sdc1 private`
  - `mkfs -t ext4 /dev/mapper/private`
  - `mkdir /private`
  - `mount /dev/mapper/private /private` 
  - `cp -R classfiles/* /private`
  - `vi /etc/crypttab (add the line: private /dev/sdc1)`
  - `vi /etc/fstab` (add the line: `/dev/mapper/private /private ext4 defaults 0 0`)
  - `reboot` (supply the lUkS-555 passphrase to mount /dev/sdc1, then obtain a shell as root)
  - `lsblk` (verify that /dev/sdc1 is mounted as a crypt volume)
  - `ls /private`
  * Open a Terminal on your Ubuntu Server virtual machine as root
  - `vi /etc/pam.d/common-auth` (add the following line above the `pam_unix.so` line) 
     - `auth required pam_tally2.so deny=3 unlock_time=7200`
  * Log into tty5 three times as woot with an incorrect password and note the warning that your account was locked when attempting to log in a fouth time 
  * Next, return to your previous Terminal as root
  - `pam_tally2` (note that woot has been locked out)
  - `pam_tally2 --reset --user woot`
  - `apt-get install tripwire` (create a passphrase of Secret555, use other defaults)
  - `tripwire --init` (this creates a database with the checksums/hashes of key system files)
  - `tripwire --check >outputfile` (this compares the checksums/hashes of files to those within the tripwire database to see if any were modified)
  - `less outputfile`

# Firewalls
  * Open a Terminal on your Ubuntu Server virtual machine as root
  - `iptables -L` (note the default policy for each chain)
  - `iptables ???P INPUT DROP`  
  - `ping gateway`	(where gateway is your default gateway ??? this should fail)
  - `iptables -L` (note the default policy for the INPUT chain)
  - `iptables ???A INPUT ???s subnet -j ACCEPT`  (where subnet is your local IP network)
  - `iptables -L` (note the firewall rule under the INPUT chain)
  - `ping gateway`	(where gateway is your default gateway ??? this works now because the ping reply is now allowed into your system)
  - `iptables ???F ; iptables ???P INPUT ACCEPT` 
  - `iptables -L` (note that your configuration has been reverted to defaults)
  - `apt install nftables`
  - `nft add table ip filter`
  - `nft add chain ip filter input`
  - `nft add rule ip filter input drop`
  - `nft add rule ip filter input tcp dport 22 accept`
  - `nft add rule ip filter input tcp dport 80 accept`
  - `nft list table ip filter`
  - `nft list ruleset` (same output since the ip table is the only one defined)
  - `nft delete table ip filter`
  - `ufw enable` 
  - `ufw status verbose`
  - `ufw allow ssh ; ufw allow ftp ; ufw allow nfs` 
  - `ufw allow samba ; ufw allow http ; ufw allow postgres`
  - `ufw status verbose`
  - `ufw disable` 
  * Open a Terminal on your Fedora Workstation virtual machine as root
  - `iptables -L`
  - `firewall-cmd --get-zones` 
  - `firewall-cmd --get-services` 
  - `firewall-cmd --list-all-zones` 
  - `firewall-cmd --get-default-zone`
  - `firewall-cmd --get-active-zones`
  - `firewall-cmd --list-all`  
  - `firewall-cmd --add-service samba` 
  - `firewall-cmd --permanent --add-service samba`
  - `firewall-cmd --query-service=samba`  
  * Log into the GNOME desktop as the woot user and explore the graphical Firewall configuration tool (Activities > Show Applications > Sundry > Firewall)

# Security Services (SELinux & AppArmor)
  * Open a Terminal on your Fedora Workstation virtual machine as root
  - `sestatus ???v` (note that SELinux is enabled and enforcing by default)
  - `cat /etc/selinux/config`
  - `setenforce permissive`
  - `sestatus -v`
  - `setenforce enforcing`
  - `sestatus -v`
  - `ps -eZ | less`
  - `ls -lZ /var/* | less`
  - `ls -lZ classfiles` (note the default type of admin_home)
  - `mv text.err /var`
  - `ls -lZ /var` (note that text.err still has the same type)
  - `restorecon /var/text.err`
  - `ls -lZ /var` (note that text.err now has a generic var type)
  - `getsebool -a | less`
  - `less /var/log/audit/audit.log` (note any SELinux violations)
  - `audit2why </var/log/audit/audit.log | less` (note the summaries and remediations)
  * Log into the GNOME desktop environment as woot and navigate to Activities > Show Applications > SELinux Troubleshooter 
    - Note the most recent SELinux violation and click Troubleshoot to see the commands that you can run to allow it (should it be allowed)
    - Next, click List all Alerts to view all of the unique SELinux violations that occurred (these are parsed from /var/log/audit/audit.log)
  * Open a Terminal on your Ubuntu Server virtual machine as root
  - `aa-status` (note the AppArmor profiles configured in enforce mode, the PowerShell profile in complain mode and the NTP daemon process in enforce mode)
  - `apt install apparmor-utils`
  - `aa-complain /usr/sbin/ntpd`
  - `aa-status`
  - `aa-enforce /usr/sbin/ntpd`
  - `aa-status`
  - `less /etc/apparmor.d/usr.sbin.ntpd`
  - `ls /etc/apparmor.d/tuneables`
  - `cat /etc/apparmor.d/tuneables/home`
