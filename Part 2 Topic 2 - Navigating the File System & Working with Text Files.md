# Navigating the Directory Structure
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `pwd` (you should be in /root)
   - `cd /etc/sysconfig` (switches directory using absolute pathname) 
   - `pwd` (you should be in /etc/sysconfig)
   - `cd ../..` (switches to / using relative pathname)
   - `cd root` (switches to /root using relative pathname)
   - `cd ../etc/sysconfig`	(switches to /etc/sysconfig using relative pathname)
   - `cd ~woot` (takes you to woot’s home directory)
   - `pwd` (you should be in /home/woot)
   - `cd` (takes you to your home directory, /root)	
   - `cd /etc/sysconfig/network-scripts` (instead of typing this out, use the `[Tab]` key to autocomplete the path!)

# Viewing Directories
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `cd classfiles` 
   - `ls -F`
   - `ls -l`
   - `file *` (note the files empty and what_am_i).
   - `ls -l /dev | less` 
   - `ls -l /run/systemd/journal/dev-log` 
   - `stat /run/systemd/journal/dev-log`
   - `cd ; ls -a`
   - `ls -l classfiles/Poems/Shakespeare`
   - `ls -R classfiles/Poems`
   - `tree classfiles/Poems`

# Viewing Text Files
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `cd classfiles`
   - `cat bigfile`
   - `more bigfile` (type `h` for help, `q` to quit)
   - `less bigfile` (type `h` for help, `q` to quit)
   - `cat letter`
   - `tail -8 letter`
   - ` strings letter`
   - `od -x letter`
   - `cat /etc/hosts`
   - `cat Poems/Yeats/mooncat`
   - `cat Poems/Shakespeare/* | less` 	
   - `cat Poems/Blake/tiger`
   - `head -1 Poems/Blake/tiger`

# Common Text Tools
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - ` cd classfiles`
   - ` grep "I " letter` 
   - ` grep -v "I " letter`
   - ` grep -i love Poems/Shakespeare/*` (note the number of lines returned)
   - ` wc -l Poems/Shakespeare/*` (was Shakespeare a romantic?) 

# Editing Text Files
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - ` nano letter` (note the legend with `[Ctrl]` key combinations)
   - ` dnf install vim` (installs the full version of vi improved)
   - ` vimtutor` (interactive vi tutorial, alternatively visit https://vim.is/#exercise)
   - ` vi small_town` (fix the numerous typos and formatting errors, saving your changes when finished)
   - ` vi ~/.vimrc` (add the line `set number` to turn on line numbering on by default)
