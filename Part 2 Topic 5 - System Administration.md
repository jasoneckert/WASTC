# Users & Groups
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `less /etc/login.defs`
   - `cat /etc/default/useradd`
   - `touch /etc/skel/policies.txt`
   - `su - woot` (switches to a woot session – no password needed!)
   - `sudo useradd -m larry` (enter woot’s password of Secret555)
   - `sudo useradd -m curly` (sudo caches your previous authentication during the session)
   - `pkexec useradd -m moe` (enter woot’s password of Secret555)
   - `exit`	(returns you to your root session)
   - `passwd larry` (enter Secret555)
   - `passwd curly`(enter Secret555)
   - `passwd moe` (enter Secret555)
   - `groupadd stooges`
   - `usermod -aG stooges larry curly moe`
   - `grep larry /etc/passwd` (interpret the fields shown)
   - `grep larry /etc/shadow` (interpret the fields shown)
   - `grep stooges /etc/group` (interpret the fields shown) 
   - `passwd -l larry`
   - `grep larry /etc/shadow`
   - `passwd -u larry`
   - `grep larry /etc/shadow`
   - `chfn larry` (supply some suitable values of your choice)
   - `grep larry /etc/passwd`
   - `dnf install finger`
   - `finger larry`
   - `ls -a /etc/skel` 
   - `ls -a /home/larry` (note the contents are the same as /etc/skel and include policies.txt)
   - `su - woot`
   - `groups` (note that you are a member of the wheel group)
   - `sudo userdel larry` (supply your password of Secret555 when prompted to confirm)
   - `su -`	(supply the root user password of Secret555 when prompted)
   - `ls -la /home/larry` (note that larry’s old UID owns existing files)
   - `useradd -m -u UID shemp` (where UID is larry’s old UID)
   - `ls -la /home/larry /home/shemp` (note that shemp owns larry’s old files)
   - `exit`
   - `id` (note your UID & GID and that root is your primary group)
   - `touch firstnewfile`
   - `ls -l firstnewfile` (note the owner of root, and group owner of root)
   - `newgrp sys`  
   - `id`	(note your UID & GID and that sys is your primary group)
   - `touch secondnewfile`
   - `ls -l secondnewfile` (note the owner of root, and group owner of sys) 

# Processes 
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `ps -ef | less` (view the processes on your system)
   - `ps -el | grep Z` (do you have any zombie processes on your system?)
   - `pstree` (use `[Shift]+[PageUp]` and `Shift]+[PageDown]` to navigate)
   - `top` (press `q` to quit when finished)
   - `dnf install htop`
   - `htop`	(press `F10` to quit when finished)
   - `ps` (note the PID of your bash shell for the following commands)
   - `kill -2 PID` (note that bash trapped the signal)
   - `kill -3 PID` (note that bash trapped the signal)
   - `kill -15 PID` (note that bash trapped the signal)
   - `kill -9 PID` (note that the process was killed successfully - log back in as root afterwards)
   - `sleep 10` (note that you do not receive your prompt until sleep terminates)
   - `sleep 5000000& ; sleep 5000000& ; sleep 5000000&`
   - `jobs`
   - `fg %3`	
   - `[Ctrl]+c` (this sends a 2/SIGINT kill signal to the foreground process)
   - `killall -9 sleep`
   - `nice -n 19 ps -l` (note the priority and nice values)
   - `nice -n -20 ps -l` (note the priority and nice values)
   - `nice -n 11 sleep 50000&` (note the PID for the next command)
   - `renice -5 PID`
   - `exec ls` (you were logged out after this command was run because you directed your shell to not fork() a subshell - log back in as root afterwards)
   - `at now + 1 minute` 
      - `echo Hello World >>/testfile`
      - `Press [Ctrl]+d`
   - `cat /testfile` (to this in about 1 minute to see the results)
   - `crontab -e` (save your changes when finished - use https://crontab.guru/ to interpret when /bin/false is scheduled to run!)
      - `15  6   1-6  *  2  /bin/false`
      - `20  21  1-6  *  2  /bin/false`
   - `crontab -l` 
   - `ls -F /proc/1` (this is the PID for the first daemon on the system)
   - `cat /proc/cpuinfo`
   - `cat /proc/meminfo`
   - `cat /proc/uptime`
   - `cat /proc/sys/net/ipv4/ip_forward` 
  
# Printing 
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `lpadmin -p p1 -E -v /dev/null -m raw`  (creates a fake printer that prints to /dev/null)
   - `lpadmin -d p1` (sets the system default printer to p1)
   - `cupsaccept p1`
   - `cupsdisable p1` (this holds print jobs in the print queue)
   - `lpstat -t`
   - `less /etc/cups/printers.conf`
   - `lp /etc/issue`
   - `lpstat`
   - `ls /var/spool/cups`
   - `lpr /etc/issue`
   - `lpq`
   - `ls /var/spool/cups`
   - `lpc status`
   - `lpstat`
   - `cancel p1-1 p1-2`
   - `lpstat`
   * Also explore the graphical CUPS admin tool in Firefox (http://localhost:631)

# Storage
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `ls -l /dev/sda*` (note the block file type, major number and minor numbers)
   - `ls -l /dev/tty[0-6]` (note the character file type, major number and minor numbers)
   * In the settings for your virtual machine, insert your Fedora ISO image into the virtual DVD drive. 
   * In the GNOME desktop, navigate to Activities > Files and note that your DVD is shown as mounted. 
   - `df -hT` (note the standard partitions are mounted to /, /home and /boot, and DVD automounted to /run/media/woot/label) 
   - `lsblk` (swap is not mounted) 
   - `ls /sys/block` 
   - `less /proc/devices`
   - `ls /run/media/woot/label`
   - `umount /run/media/woot/label`
   - `df -hT` (verify that the virtual DVD is no longer mounted)
   - `mount /dev/cdrom /mnt` (/dev/cdrom is a symlink to your CD/DVD device file)
   - `ls /mnt` (note the DVD contents)
   - `fdisk /dev/sda` 
      - `p` 
      - `n`
      - `[Enter]` 
      - `[Enter]`
      - `p`
      - `w`
   - `partprobe` 
   - `mkfs -t ext4 /dev/sdan` (where n is the partition number from fdisk)
   - `mkdir /data`
   - `mount /dev/sdan /data`
   - `lsblk`
   - `cp -R /root/classfiles /data`
   - `ls /data`
   - `ls /data/classfiles`
   - `df -hT`
   - `umount /data`
   - `pvcreate /dev/sdan` (choose `y` to remove the existing ext4 filesystem)
   - `vgcreate vg00 /dev/sdan`  
   - `vgdisplay` (note the value next to VG Size in GB for creating LVs)
   - `lvcreate -L size -n data vg00` (where size is the VG Size from the last command)
   - `mkfs -t ext4 /dev/vg00/data`
   - `mount /dev/vg00/data /data`
   - `lvdisplay`
   - `lsblk`
   - `df -hT`
   - `vi /etc/fstab` (add the following line and save your changes)
      - `/dev/vg00/data   /data  ext4  defaults  0  0` 
   - `reboot` (after the system has booted, open a Terminal as root)
   - `lsblk` (verify that the data LV is mounted)
   * In your virtualization software, add a second virtual hard disk to your Fedora Workstation virtual machine (this will be /dev/sdb). 
   - `lsblk` 
   - `fdisk /dev/sdb`
     - `n`
     - `[Enter]`
     - `[Enter]`
     - `p`
     - `w`
   - `pvcreate /dev/sdb1`
   - `vgextend vg00 /dev/sdb1`
   - `vgdisplay`  (note the additional space available in the vg00 pool)
   - `lvextend -L +size –r vg00/data` (where size is the additional space)
   - `lsblk`
   - `df -hT`  (note that your /data volume is now much larger!)
   - `umount /data`
   - `fsck -f /dev/vg00/data`
   - `echo $?` (note that you receive 0, which indicates that no other errors exist)
   - `mount /data` (this works because of the line in /etc/fstab)
   - `cd classfiles`
   - `mkisofs -RJ -o poems.iso Poems`
   - `ls -l poems.iso`
   - `mount -o loop -r -t iso9660 poems.iso /mnt`
   - `lsblk`
   - `df -hT`
   - `ls /mnt`
   - `ls -R /mnt`
   - `umount /mnt`

# Server Installation
   * Install Ubuntu Server (if you have not already)
   * Open a Terminal on your Ubuntu Server virtual machine as root
   - `lsblk`
   - `fdisk -l`
   - `lvdisplay`
   - `df -hT`
   - `apt install net-tools cockpit`
   - `systemctl start cockpit`
   - `ifconfig` (note your IP address)
   * On your Windows or macOS host computer, open up your Web browser and access your Cockpit console (https://IPaddress:9090)
   * Log into Cockpit as the root user, navigate the interface and note the configuration options

# Server Storage
   * In the Settings of your Ubuntu Server virtual machine, add 4 additional virtual disks (8GB each)
   * Boot your Ubuntu Server virtual machine and open a Terminal as root 
   - `lsblk` (note that your new block devices are detected) 
   - `mdadm --create /dev/md0 --level=5 --raid-devices=3 /dev/sdb /dev/sdc /dev/sdd` (to create a softare RAID 5)
   - `mkfs –t ext4 /dev/md0`
   - `mkdir /raid5`
   - `mount /dev/md0 /raid5` 
   - `lsblk`
   - `df –hT`
   - `cat /proc/mdstat`
   - `mdadm --detail /dev/md0` 
   - `sync`
   - `mdadm --manage /dev/md0 --fail /dev/sdb` (to simulate a failure on sdb)
   - `mdadm --detail /dev/md0` 
   - `cat /proc/mdstat` 
   - `mdadm --manage /dev/md0 --remove /dev/sdb`
   - `mdadm --manage /dev/md0 --add /dev/sde` 
   - `mdadm --detail /dev/md0` (after a few minutes, you should see it fully synchronized)  
   - `umount /raid5` 
   - `mdadm --stop /dev/md0` 
   - `fdisk /dev/sdb`
     - `w` (this will remove the existing RAID signature from sdb)
   * Repeat this fdisk command for /dev/sdc, /dev/sdd, and /dev/sde
   - `apt-get update`
   - `add-apt-repository main`
   - `add-apt-repository restricted`
   - `add-apt-repository universe`
   - `add-apt-repository multiverse`
   - `apt-get install zfsutils-linux`
   - `zpool create lala /dev/sdb -f`
   - `zpool list`
   - `lsblk`
   - `cp /etc/hosts /lala`
   - `ls -l /lala`
   - `zpool destroy lala`
   - `zpool create po mirror /dev/sdb /dev/sdc -f`
   - `zpool list`
   - `lsblk`
   - `cp /etc/hosts /po`
   - `zpool status po`
   - `dd if=/dev/zero of=/dev/sdb1 count=100000000` (press `[Ctrl]+c` after 5-6 seconds)
   - `zpool scrub po`
   - `zpool status po`
   - `zpool detach po /dev/sdb`
   - `zpool status po`
   - `zpool attach po /dev/sdc /dev/sdd -f`
   - `zpool list`
   - `zpool status po`
   - `zpool iostat -v po`
   - `zpool destroy po`
   - `zpool create noonoo raidz /dev/sdb /dev/sdc /dev/sdd /dev/sde -f`
   - `zpool status noonoo`
   - `zpool iostat -v noonoo`
   -  `mkdir /noonoo/larry`
   - `mkdir /noonoo/curly`
   - `mkdir /noonoo/moe`
   - `zfs list`
   - `zfs create noonoo/larry`
   - `zfs create noonoo/curly`
   - `zfs create noonoo/moe`
   - `zfs list`
   - `ls /noonoo`
   - `zfs get all noonoo/larry | less`
   - `zfs set quota=1G noonoo/larry`
   - `zfs set compression=lz4 noonoo/larry`
   - `zfs get all noonoo/larry`
   - `zpool destroy noonoo`
   - `fdisk /dev/sdb`
     - `d` (this removes the ZFS state partition)
     - `d` (this removes the ZFS data partition)
   * Repeat this fdisk command for /dev/sdc, /dev/sdd, and /dev/sde
   - `mkfs.btrfs -m raid0 -d raid0 /dev/sdb /dev/sdc /dev/sdd -f`
   - `mkdir /btrfs`
   - `mount /dev/sdb /btrfs`
   - `df –hT`
   - `lsblk`
   - `btrfs device add -f /dev/sde /btrfs`
   - `btrfs filesystem balance /btrfs`
   - `df -hT`
   - `btrfs subvolume list /btrfs` 
   - `btrfs subvolume create /btrfs/stuff` 
   - `btrfs subvolume list /btrfs` 
   - `cp /etc/services /btrfs/stuff`
   - `mkdir /stuff` 
   - `mount -o subvol=stuff,compress=lzo /dev/sdb /stuff` 
   - `df –hT` 
   - `ls –l /stuff` 
   - `umount /stuff` 
   - `umount /btrfs` 
   - `mkfs.btrfs -m raid1 -d raid1 /dev/sdb /dev/sdc /dev/sdd /dev/sde -f` 
   - `btrfs filesystem show /dev/sdb` 
   - `mount /dev/sdb /btrfs`
   - `df -hT` 
   - `lsblk` 
   - `btrfs filesystem df /btrfs`
   - `umount /btrfs`
   - `mkfs.btrfs -m raid5 -d raid5 /dev/sdb /dev/sdc /dev/sdd /dev/sde -f`
   - `btrfs filesystem show /dev/sdb`
   - `mount /dev/sdb /btrfs`
   - `df -hT` 
   -  `lsblk` 
   - `btrfs filesystem df /btrfs` 
   - `umount /btrfs`
   - `btrfs check /dev/sdb`
   - `btrfsck /dev/sdb`
   - `poweroff`
   * In the Settings of your Ubuntu Server virtual machine, remove the 4 additional 8GB virtual disks you added earlier.

# System Startup & Daemons
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `vi /etc/default/grub` (change the `GRUB_TIMEOUT` to `30` seconds, saving your changes)
   - `grub2-mkconfig –o /boot/grub2/grub.cfg` (if your hypervisor emulates standard BIOS)
   - `grub2-mkconfig –o /boot/efi/EFI/fedora/grub.cfg` (if your hypervisor emulates UEFI)
   - `less /boot/grub2/grub.cfg` (if standard BIOS, noting the timeout value)
   - `less /boot/efi/EFI/fedora/grub.cfg` (if UEFI, noting the timeout value)
   - `reboot`	(at the GRUB menu, press `c` to obtain a command prompt and run the following)
     - `[Tab]`
     - `ls`
     - `[Esc]`
     - `e` (scroll down to the `linux ($root)/vmlinuz-*` line, and append the word `single` to it) 
     - `[F10]` (supply the root password once the system reaches single user mode) 
   - `runlevel` 
   - `reboot` (press `[Enter]` at the GRUB menu to boot the default kernel normally - following boot, open a Terminal as root)  
   - `runlevel` (note that your current runlevel is 5 and previous is N)
   - `init 3`
   - `runlevel` (note that your current runlevel is 3 and previous is 5)
   - `[Ctrl]+[Alt]+[F1]` (note that the gdm is not available)
   - `[Ctrl]+[Alt]+[F3]` (log in as root)
   - `startx` (log out of GNOME when finished)
   - `init 6` (following boot, open a Teminal as root)
   - `chkconfig --list` (note the livesys and livesys-late legacy daemons)
   - `systemctl | grep crond` (note that crond is a modern Systemd-managed daemon)
   - `service crond restart`	(this works because Systemd is init-compatible)
   - `systemctl restart crond.service`
   - `systemctl status crond.service` (crond is started, and set to start in the default target)
   - `systemctl disable crond.service`
   - `systemctl status crond.service` (note that crond not set to start in the default target)
   - `systemctl enable crond.service`
   - `systemctl get-default` (note that graphical.target is the default target)
   * Optionally explore various unit files under the /lib/systemd/system folder, interpreting their contents. 

# Localization
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `date +%s` 
   - `hwclock`
   - `date`
   - `timedatectl`
   - `ls -l /etc/localtime`
   - `cat /proc/cmdline` (note the kernel that was loaded during the previous boot did not receive a locale from GRUB)
   - `locale`
   - `localectl`
   - `locale –a | less`
   - `localectl list-locales | less`
   - `localectl list-keymaps | less` (note the us-mac keymap for U.S. Apple keyboards) 

# Compression
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `dnf install ncompress`	
   - `cd classfiles`
   - `compress -v bigfile` (note the compression ratio)
   - `uncompress bigfile`
   - `zip -v bigfile.zip bigfile` (note the compression ratio)
   - `unzip bigfile.zip` (choose to overwrite bigfile)
   - `gzip -v -9 bigfile` (note the compression ratio)
   - `gunzip bigfile.gz`
   - `xz -v -9 bigfile` (note the compression ratio)
   - `unxz bigfile.xz`
   - `bzip2 -v bigfile` (note the compression ratio is better than other commands)
   -  `bunzip2 bigfile.bz2`		
 
# Backup
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `cd classfiles`
   - `tar -zcvf /sample.tar.gz Poems`
   - `tar -ztvf /sample.tar.gz`
   - `mkdir test1 && cd test1`
   - `tar -zxvf /sample.tar.gz`
   - `ls -R`
   - `cd ~/classfiles`
   - `find Poems | cpio -vocBL -O /sample.cpio`
   - `cpio -Bitv -I /sample.cpio`
   - `mkdir test2 && cd test2`
   - `cpio -vicdumB -I /sample.cpio`
   - `dnf install dump`
   - `dump -0uf /sda1.dump0 /dev/sda1` (if UEFI, use sda2 instead of sda1)
   - `dump -1uf /sda1.dump1 /dev/sda1` (if UEFI, use sda2 instead of sda1)
   - `cat /etc/dumpdates` (note the two backups and their dump levels)
   - `dd if=/dev/sda1 of=/sda1.dd`
   - `df -hT`
   - `ls -l /sda1.dd` (note the size of the backup = the size of the filesystem)
  
# Software
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `dnf groupinstall “Development Tools”`   
   - `git clone https://github.com/bartobri/no-more-secrets.git`
   - `cd no-more-secrets` 
   - `less README.md`
   - `make nms` 
   - `make sneakers` 
   - `make install`
   - `which sneakers`
   - `which nms`
   - `sneakers`
   - `ls -l | nms` 
   - `dnf install nmap`
   - `rpm -qi nmap`
   - `rpm -V nmap`
   - `rpm -ql nmap`
   - `dnf info nmap`
   - `dnf grouplist`
   - `dnf list installed`
   - `rpm -qf /bin/bash`
   * Open a Terminal on your Ubuntu Server virtual machine as root
   - `apt install nmap`
   - `dpkg -l`
   - `dpkg -l '*nmap*'`
   - `dpkg -L nmap | less`
   - `snap install powershell --classic`
   - `snap info powershell`
   - `snap list`
   - `powershell`
     - `Get-Host`
     - `exit`
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `flatpak --system remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo` 
   - `flatpak search vlc`
   - `flatpak install flathub org.videolan.VLC` (search for VLC in the GNOME desktop to run it)
   * In Firefox on your Fedora Workstation (as woot), download the sample Subsurface .appimage package from https://appimage.org/ for your architecture. 
   * Next, open a Terminal app as woot and run the following commands to run your AppImage package:
   - `cd Downloads`
   - `chmod +x Subsurface*.appimage`
   - `./Subsurface*.appimage` 

# Logs
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `ls /var/log`
   - `less /etc/logrotate.conf`
   - `ls /etc/logrotate.d`
   - `less /etc/logrotate.d/*`
   - `who /var/log/wtmp`
   - `journalctl _COMM=su`
   - `journalctl _COMM=gdm --since “08:00”`
   - `journalctl --unit=crond.service --since “08:00”`
   - `journalctl --unit=cups.service --since “08:00”`
   - `journalctl -b`
   - `journalctl -k`
   - `dmesg | less`
   * Open a Terminal on your Ubuntu Server virtual machine as root
   - `ls /var/log`
   - `less /etc/logrotate.conf`
   - `ls /etc/logrotate.d`
   - `less /etc/logrotate.d/*`
   - `who -f /var/log/wtmp`
   - `less /etc/rsyslog.conf`
   - `less /etc/rsyslog.d/50-default.conf`
   - `less /var/log/kern.log`
   - `less /var/log/syslog`
   - `less /var/log/boot.log` 

# Performance
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `dnf install sysstat iotop ioping iftop`
   - `mpstat 2 5`
   - `pidstat`
   - `iostat`
   - `vmstat`
   - `free`
   - `sar -u 2 5`
   - `sar -q 2 5`
   - `sar -d 2 5`
   - `sar -f /var/log/sa/saX` (where X is the day of the month – this will work tomorrow!)
   - `top`
   - `iotop`
   - `ioping /dev/sda`
   - `iftop`
   - `uptime`
   - `tload` (use `[Ctrl]+c` to quit)
   - `w` 

# Devices
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `dnf install lshw`
   - `lshw -class network` (note the driver used)
   - `lsmod | less` (note that your driver is listed)
   - `modprobe dummy` (dummy is a fake network card driver used for testing only) 
   - `lsmod | grep dummy` (note that your driver is listed)
   - `modinfo dummy`
   - `rmmod dummy`
   - `less /etc/modprobe.d/*` (note any manually-loaded modules)
   - `poweroff` (for next section) 

# System Rescue
   * In the settings of your Fedora Workstation virtual machine:
     - Insert the Fedora installation ISO into the virtual DVD, and 
     - Ensure that the virtual DVD is listed first in the boot order 
   * Start your Fedora Workstation virtual machine - once the live distro has loaded, open a Terminal 
     - `su -`
     - `df -hT`	(note the partitions that are mounted)
     - `ls -l /` (note that there are fewer directories under the root of the live distribution)
     - `fsck -f device1` (where device1 is the device file for your root filesystem – e.g. /dev/sda3)  
     - `mount device1 /mnt`
     - `mount device2 /mnt/boot` (where device2 is the device file for your /boot filesystem – e.g. /dev/sda1)
     - `mount -t proc proc /proc`	(this activates the /proc filesystem)
     - `chroot /mnt` (changes the system root from the live distribution to the root of the system installed on /dev/sda as the root user)
     - `ls -l /` (note that you are on the root filesystem on /dev/sda)
     - `cat /etc/fstab`	(note the entry you created earlier in this chapter)
     - `passwd root` (specify a new root password of your choice)
     - `exit`
     - `poweroff`
   * In the settings of your Fedora Workstation virtual machine, remove the Fedora Workstation installation media from the virtual DVD drive
   * Boot your Fedora Workstation virtual machine and obtain a Terminal as root (using your new password!)
