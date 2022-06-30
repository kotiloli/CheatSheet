# Environment Setup

* Below commands can be used for updating the system before installing git.

```bash
	sudo apt update			#for updating the system
	sudo apt upgrade		#for updating the system
	sudo add-apt-repository ppa:git-core/ppa	
	sudo apt update
	sudo apt install git		#installs git to system
	git --version               #current version of git
```

* Below commands for git configuration. The `user.name `and `user.email` is the one that whose name will be stated at the remote repo. 

````bash
    git config --list                               #show all global vars
	git config --global user.name "your name"       #set username
	git config --global user.email "foo@bar.com"    #set email
	git config --global init.defaultBranch main     #set default branch
	git config --global color.ui auto
	git config --get user.name	                    #just for check
	git config --get user.email	                    #just for check
````

* SSH Configuration: SSH enables the git make a secure connection without entering the password evertime we connected. SSH uses `public` and `private` keys to do that. `private` key stored in computer, generaly in `home/.ssh` directory and the `public` key shared with the github.com. If the keys match, you are granted access.

We can use below commands to create  proper ssh keys. run these commands from `home/.git` directory.

```bash
	ssh-keygen -t rsa -b 4096 -C "foo@bar.com"	#creates ssh files(id_rsa and id_rsa.pub)
                                                # -t means type
                                                # -b means bytes
                                                # -C means credentials or comment
	ssh -T git.github.com			            # test connection 
```
* Using a `config` file can be usefull for working multiple github accounts. To succeed this create a file named `config` with no extension in the `.ssh` directory. A sample `config` file is below.

`config` file:
```bash
# aakutlu >> aliaydinkutlu@gmail.com, - the default config
Host github.com-aakutlu github account  # unique name of your host `github.com-xxx`
   HostName github.com                  # real host name
   User git
   IdentityFile ~/.ssh/aakutlu          # corresponding `private` key file
   
# cotillion5561 >> cotillion5561@gmail.com
Host github.com-cotillion5561 github account   
   HostName github.com
   User git
   IdentityFile ~/.ssh/cotillion5561       # corresponding `private` key file

# kotiloli >> kotiloli@gmail.com
Host github.com-kotiloli github account   
   HostName github.com
   User git
   IdentityFile ~/.ssh/kotiloli             # corresponding `private` key file
``` 
* Creating Github repository: Now you need to create a github repo to work with. (Creating the repository from the website is more easy)

# GIT Basic Syntax

```
                        PROGRAM | ACTION | DESTINATION
```
* Basic git workflow

```
1) Write your code, make hacking, add your images, arrange your folders etc.
2) State your changes. Use ** git add <files> ** for this
3) commit your changes. use ```` git commit -m "Commit message here" ```` for this
4) make a push request. now you can push your changes to remote repository. use ```` git push origin main ```` for this
```

* `git clone` Usage

if you are creating a new project make a `git clone ...` just after creating the repository on the web page manualy. Thus, you can import the `.git` file to your workspace which holds the `config` file in it.

```bash
git clone git@github.com:USER_NAME/REPO_NAME.git
git clone git@github.com:kotiloli/ceatsheet.git
```

* `.gitignore` file: You can use a `.gitignore` file to exclude some files from the git lifecycle of the project. A sample of `.gitignore` file is below

`.gitignore` file:
```bash
# Private Files
*.json              # any file with extension .json, .csv, .xlsx
*.csv
*.xlsx

# Mac/OSX
.DS_Store

# Byte-compiled / optimized / DLL files
__pycache__/        #folder declaration
*.py[cod]
*$py.class

# Some Folders You dont want to push to repo
sql_scripts/


```


* `git add` Usage

```bash
git add file1.txt file2.css file3.js    # add 3 files to staging area
git add .                               # add all files exluding .gitignore
```

* `git commit` Usage

```bash
git commit -m "YOUR COMMENT(message) HERE"  # commit the files in staging area
git -am "message"	                        # this special script makes `git add` and `git commit -m "comment"` together. Only commits modified files.
```

* `git push` usage

```bash
git push <remote> <branch>
git push 
git push origin main

```

* Other git commands

```bash
    git status                      # show current situation 
	git log                         # show last commits 

	git branch BRANCH_NAME		    #creates a new branch
	git checkout BRANCH_NAME	    #jumps a branch
	git checkout -b <branch_name>	#creates and jumps the branch at the same time
	
	git diff BRANCH_NAME		    #shows the diffrences you have made in the new branch
	git merge BRANCH_NAME
	git branch -d BRANCH_NAME	    #delete a branch(after merging)
	git rebase main			        #after merge rearrange the master list
```

* Reset an `add` or `commit` operation

```bash
git reset somefile.txt          # drops file from staging area
git reset

git reset HEAD~1
git reset <hashcode>		    # the hashcodeded commit unstaged
git reset --hard <hashcode>	    # the commit completely removed
```
	

* TIPS AND TRİCKS

```bash
	git archive master –format=zip  –output= ../foo.zip        #It stores all files and data in a zip file rather than the .git directory.
	git bundle create ../repo.bundler master                    # This pushes the master branch to a remote branch, only contained in a file instead of a repository.
	git stash           #remove new feature to stash removelike
    git stash apply     # re-apply the changes you "stash"ed
		
```
