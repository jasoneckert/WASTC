# Manipulating Files & Directories
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `cd classfiles` 
   - `cp proposal1 /var`
   - `ls /var`
   - `cp proposal1 /var` (note that you are prompted to overwrite the file)
   - `cp proposal1 /var/newproposal1`
   - `ls /var`
   - `mv /var/newproposal1 new1`
   - `ls`
   - `cp -R Poems Poems2`
   - `ls`
   - `mv Poems2 Poems3`
   - `ls`
   - `rm -f new1`
   - `rm -Rf Poems3`
   - `ls`

# Finding Files
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `cd classfiles`
   - `touch newfile1`
   - `locate newfile1` (no results in the locate database for this new file)		
   - `updatedb`
   - `locate newfile1` (it works now because you updated the locate database)
   - `echo $PATH` (note the directories in PATH)
   - `which cp` (note that cp is in a directory in PATH)
   - `which newfile1` (no results are shown because newfile1 is not in PATH)
   - `cp newfile1 /usr/bin`
   - `chmod +x /usr/bin/newfile1` (this gives newfile1 execute permission, discussed later)
   - `which newfile1` (the newfile1 executable program is now shown!)
   - `find / -name “*hosts*”`
   - `find /usr -type d`

# File Linking
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `cd classfiles`
   - `ll letter` (note that it is not hard linked or symlinked from the link count)
   - `ln letter hardletter`
   - `ll -i letter hardletter` (note the link count of 2 and duplicate inode number)
   - `rm -f hardletter`
   - `ll -i letter` (note that the link count has returned to 1)
   - `ln -s letter symletter`
   - `ll` (note the file type and target pointer)
   - `rm -f symletter`

# Ownership
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `cd classfiles`			
   - `ll proposal1` (note the user and group owner)
   - `chown nobody proposal1`
   - `chgrp nobody proposal1`
   - `ll proposal1` (note the nobody user and nobody group owner)
   - `chown root.bin proposal1`
   - `ll proposal1` (note the root user and bin group owner)

# Permissions
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `cd classfiles`
   - `chmod u=r,g=rwx,o=w letter`
   - `ll letter` (note the mode is r--rwx-w-)
   - `chmod g-wx,o-w letter`
   - `ll letter` (note the mode is r--r-----)
   - `chmod 000 letter`
   - `ll letter` (note the mode is ---------)
   - `chmod 777 letter`
   - `ll letter` (note the mode is rwxrwxrwx)
   - `chmod 664 letter`
   - `ll letter` (note the mode is rw-rw-r--)

# Default Permissions
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `umask`	(note the default umask of 0022, which is 022)
   - `mkdir sampledir`
   - `ll -d sampledir` (note the default permissions of rwx-r-xr-x)
   - `touch samplefile`
   - `ll samplefile` (note the default permissions of rw--r--r--)
   - `umask	077`			
   - `mkdir sampledir2`
   - `ll -d sampledir2` (note the default permissions of rwx-------)
   - `touch samplefile2`
   - `ll samplefile2` (note the default permissions of rw--------)

# Special Permissions
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `cd classfiles`
   - `chmod 4666 /bin/false`	
   - `ll /bin/false` (note the capitalization indicating absence of x)
   - `chmod 4777 /bin/false`	
   - `ll /bin/false` (users who execute false become the owner!)
   - `mkdir /public`
   - `chmod 1777 /public` (sets the sticky bit)
   - `touch /public/rootfile`
   - `ll /public/rootfile` (note that the owner is root)
   - `exit` (log into a new terminal as woot to execute remaining commands)
   - `touch /public/wootfile`
   - `ll /public/wootfile` (note that the owner is woot)
   - `rm -f /public/wootfile`
   - `rm -f /public/rootfile` (note the error message, even though you have w permission on the directory)
   - `exit` (log into a new terminal as root to execute commands in the next 2 sections)
   
# Modifying the ACL (beyond User, Group, Other)
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `cd classfiles`
   - `setfacl -m u:woot:rw- letter`	
   - `ll letter` (note the + symbol next to the mode)
   - `getfacl letter` (note the mask and entry for woot)
   - `setfacl -b letter`
   - `ll letter` (note the + symbol is no longer present)
   - `getfacl letter` (note the absence of additional ACL entries)

# File Attributes
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `cd classfiles`
   - `lsattr letter`
   - `chattr +i letter`
   - `lsattr letter` (note the immutable attribute is set)
   - `vi letter` (make a change and attempt to save the file – when you are denied, exit vi discarding changes)
   - `chattr -i letter`
   - `lsattr letter`
   - `man chattr` (note the description of other attributes)
