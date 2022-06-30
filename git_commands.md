# Environment Setup




*FOR UPDATING THE SYSTEM BEFORE INSTALLING GIT
	sudo apt update			#for updating the system
	sudo apt upgrade		#for updating the system
	sudo add-apt-repository ppa:git-core/ppa	
	sudo apt update
	sudo apt install git		#installs git to system
	git --version
*FOR CONFIGURING THE GıT SYSTEM
	git config --global user.name "Your Name"
	git config --global user.email "yourname@example.com"
	git config --global init.defaultBranch main
	git config --global color.ui auto
	git config --get user.name	#just for check
	git config --get user.email	#just for check
*GITHUB SETTINGS FOR CONNECTION WITH SSH 
	ssh-keygen -t ed25519 -C <youremail>	#creates an ssh file
	ssh -T git.github.com			#testing out ssh key
*YOU HAVE TO CREATE YOUR GITHUB REPOSITORY FROM THE WEBSITE

*LOCAL GIT WORKFLOW
	 --------------------------------------------------
	|	*write code                                |
	|	*state changes	($git add)                 |
	|	*commit changes	($git commit)              |
	|	*push changes	($git push)                |
	|	*make a pull request                       |
	 --------------------------------------------------

	GIT BASIC SYNTAX   program | action | destination

	git clone git@github.com:USER_NAME/REPO_NAME.git
	git clone git@github.com:kotiloli/git_test.git


	git add file1.txt file2.css etc.
	git add .
	git commit -m "YOUR COMMENT HERE"
	git add + git commit -m "message" = git -am "message"	#for only modified files
	git push
	git push origin main
	git status
	git log

	git branch
	git branch BRANCH_NAME		#creates a new branch
	git checkout BRANCH_NAME	#jumps a branch
	git checkout -b[ branch_name]	#creates and jumps the branch at the same time
	
	git diff BRANCH_NAME		#shows the diffrences you have made in the new branch
	git merge BRANCH_NAME
	git branch -d BRANCH_NAME	#delete a branch(after merging)
	git rebase main			#after merge rearrange the master list
*RESETTING AFTER A ADD
	git add somefile.txt	=>	git reset somefile.txt {or} git reset	#drops the file from staging area
*RESETTING AFTER A COMMIT
	git reset HEAD~1
	git reset <hashcode>		#the commit unstaged
	git reset --hard <hashcode>	#the commit completely removed


*TIPS AND TRİCKS	
	git archive master –format=zip  –output= ../name-of-file.zip
		It stores all files and data in a zip file rather than the .git directory.
	git bundle create ../repo.bundler master
		This pushes the master branch to a remote branch, only contained in a file instead of a repository.
	

	git status
	git stash		#remove new feature to stash removelike
	git status
		And when you want to re-apply the changes you “stash”ed ,use the command below:
	git stash apply