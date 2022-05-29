# ***NOTE: Run the commands in the following sections within a new Terminal on your Fedora Workstation as root:***

# Redirection
   - `df -hT >diskspace` (we examine the `df` command later)
   - `date >>diskspace`
   - `cat diskspace`
   - `ls letter lett >good 2>bad`
   - `cat good`
   - `cat bad`
   - `cp /etc/hosts ~`
   - `cat hosts`
   - `sort hosts >hosts`
   - `cat hosts` (the file is empty because the shell clears an existing file before writing stdout to it with the `>` symbol)
   - `tr a A <letter >newletter`
   - `cat newletter`
   - `ls <letter` (ls runs normally because it does not accept stdin)

# Piping
   - `cd classfiles`
   - `grep -i mother letter | wc -l`
   - `cat letter | pr -d | less`
   - `cat proposal1 | tr a W | sort | tee newfile`
   - `cat newfile`
   - `cat Poems/Shakespeare/sonnet* | wc -l`
   - `grep -i love Poems/Shakespeare/sonnet* | wc -l`

# Shell Variables
   - `VAR1="Hello World"`
   - `echo $VAR1`
   - `set | grep VAR1`
   - `env | grep VAR1` (VAR1 is not shown, as it was not exported)
   - `export VAR1`
   - `env | grep VAR1`
   - `PS1='C:${PWD////\\\\}>  '` 
   - `cat ~/.bash_profile` (on Fedora, it runs .bashrc first if it exists)
   - `cat ~/.bashrc` (note the aliases for `rm`, `cp` and `mv`)
   - `exit` (open a new shell as the root user)
   - `echo $VAR1`
   - `vi ~/.bashrc` (add the following lines, save and quit)
     - `export VAR1="Hello World"`
     - `alias bye="echo Goodbye ; sleep 5 ; exit"`
   - `source ~/.bashrc` (so you don't need to open a new shell)

# Shell Scripts (note that the `tar` command is discussed later)
   - `git clone https://github.com/jasoneckert/shellscripts.git`
   - `cd shellscripts`
   - `cat script1.sh`
   - `cat script2.sh`
   - `cat script3.sh`
   - `cat script5.sh`
   
# Advanced Text Tools
   - `cd classfiles`
   - `cut –d: -f3 /etc/passwd`		
   - `cut –d: -f3 /etc/passwd > passfile`
   - `cut –d: -f6 /etc/passwd > passfile2`
   - `paste passfile passfile2`	
   - `pr –d letter > letter2`	
   - `pr –d –l20 letter > letter2`	 
   - `fmt –30 letter` 
   - `nl letter`
   - `printf "Hello %s! \nYour shell is: %s \n" $LOGNAME $SHELL` 
   - `split –l4 /etc/passwd`
   - `sed /Mother/s/the/BILLY/ letter`	
   - `sed 1s/Mother/Grandma/ letter`		
   - `sed ‘$s/Alan/Mike/’ letter`		
   - `sed 1,8s/the/WOWEE/g letter`
   - `sed /the/d letter`
   - `cat bin/treed` (this is a challenge!)
   - `bin/treed .`
   - `cat ../shellscripts/script4.sh` (note the use of `sed`)
   - `awk '/Mother/ {print}' letter`
   - `awk '/Mother/ {print $1, $4}' letter`	
   - `awk '/Mother/ {print $1 "\t" $4}' letter`
   - `awk –F: '/root/ {print $3, $5}' /etc/passwd`
   
# Git
   - `mkdir /scripts`
  	- `cp scriptname scriptname scriptname /scripts`
  	- `cd /scripts`
   - `git config --global user.name "name"`
  	- `git config --global user.email "email"`
  	- `git init`
  	- `git status`
  	- `git add .`
  	- `git commit -m "My first snapshot"`
  	- `git log`
   - `git checkout -b sample`
  	- `git branch`
   - `vi scriptname`
  	- `git add .`
  	- `git commit -m "Description"`
   - `git checkout master`
  	- `cat scriptname`
   - `git merge sample`
  	- `cat scriptname`

   - Optionally create a free account on GitHub and create a public "scripts" repo. 
   - Next, use the appropriate commands (shown on GitHub after you create a new repo) to push your /scripts repo to GitHub and set it as the origin (this converts your /scripts repo into a cloned copy). 
   - When you push your /scripts repo to GitHub, you will be prompted to log in with your GitHub username and a personal access token (very long password) that is used in place of your real GitHub password for pushing. 
   - You can generate a personal access token on the GitHub website by clicking your user icon and navigating to Settings > Developer settings > Personal access tokens. When generating a personal access token, you must supply 
     a description, expiration and select the repo scope for it to work. 

# The Z Shell (optional extra)
   - `cd classfiles`
   - `grep -i mother letter >file1.txt >file2.txt`
   - `cat file1.txt`
   - `cat file2.txt`
   - `grep -i mother letter >file1.txt | grep -i granada >file2.txt`
   - `cat file1.txt`
   - `cat file2.txt`
   - `sort <proposal1 <greeting >sortedresults.txt`
   - `cat sortedresults.txt`
   - `setopt autocd correct`
   - `grp -i mother letter` (note that it prompts you to correct the command to grep)
   - `autoload -U compinit`
   - `compinit`
   - `date -` (press `[Tab]` to see the available date command options)
   - `vi ~/.zshrc` (add the following lines, save and quit)
      - `setopt autocd correct`
      - `autoload -U compinit`
      - `compinit`
