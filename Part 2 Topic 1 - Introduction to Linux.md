# Basic Linux Usage
   - Power on your Fedora Workstation system. You are on the tty1 terminal viewing the gdm (graphical login).
   - Switch between the other local command line terminals by using the appropriate key combinations (`[Ctrl]+[Alt]+F2` through `F6`).
   - Switch to tty3 and log in as the user root (# prompt in the bash shell). 
   - Switch to tty4 and log in as the user woot ($ prompt in the bash shell). 
   - Switch to tty1 and log into the gdm as the woot user to start the GNOME desktop environment. 
   - Open a command line terminal by clicking Activities and typing Terminal. At the terminal, run the following commands:
       `who`
       `su - root` 
   - Switch to tty3 and run the "tmux" command. 
   - Press the `[Ctrl]+b` key combination and then type `%` to split your screen. 
   - Press the `[Ctrl]+b` key combination and then type `“` to split your screen in the opposite direction. 
   - Run the `who` command and note that tmux runs all screens within the tty3 terminal. 
   - Practice switching between these three screens by pressing the `[Ctrl]+b` key combination followed by a cursor key (up down left right). 
   - Type `exit` within each of your screens to kill each bash shell, returning you to your original bash shell.
   - Run the following commands to install the KDE Plasma Workspaces desktop environment:
       `dnf groupinstall “KDE Plasma Workspaces”`
       `reboot`
   - After your system has rebooted, choose the Settings (cog wheel icon) menu in the gdm and select Plasma. 
   - Complete your login as the user woot.
   - Use the KDE start menu to locate the Terminal app to obtain a bash shell and run the who command. 
   - Log out of the KDE desktop environment. 

# ***Run the following commands in a new Terminal on your Fedora Workstation as root:***

# Basic Shell Usage
   `date`
   `who`
   `w`
   `uname -a`
   `date;id;who`
   `echo We use the $SHELL shell`
   `echo 'You owe me $4.50'`
   `echo "You owe me $4.50"`
   `echo You owe me \$4.50`
   `clear`
   `reset`
   `wall "The system will reboot in 5 minutes"`
   `shutdown -r +5 minutes`
   `shutdown -c`(cancels reboot)
   `reboot`

# Obtaining Command Help
   `whatis crontab`
   `man crontab` (use `q` or `[Ctrl]+c` to quit out of any interactive utility)
   `man 5 crontab`
   `man -k cron`
   `info crontab`
   `help | more`
   `help echo`
   `help help`
