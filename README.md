# About freshlocate

**freshlocate** is a self created linux command made using bash scripting.

## What does freshlocate do
 - freshlocate is used to update the db which locate uses to run lightning fast and simultaneously find the file.
 - Current version contains the logical structure of what it'll do. Subsequent versions will improve it to add various features.
## How To Create Your Own Bash Script:
 Before we begin let us understand what bash is!
 - The core concept of bash scripting is to make our shell to read the file so that it can execute it.
 - Bash is the shell which reads terminal commands and executes them.
 - It is also the scripting language which is used to add functionality.
 - We can think of 4 main parts of Bash script:
	- 1.) shebang - `#!/usr/bin/env bash` which basically says when the file is executed tell the OS to go to `usr/bin/env` and run bash which then reads our fil.e
	- 2.) Documentation/comments(#) - Used to make our scripts readable so that others can understand what each part expression or statement does.
	- 3.) Variables - We use "$" symbol to access the value stored in the variable name e.g: "$PATH" gives "/home/username".
	- 4.) Logic/commands - commands which we need the script to execute for our functionality.

---
 Now we can start our script! IN THE NEXT GIT COMMIT HEH

 #How to use git (BASICS):
 - Create a directory where you want to store your projects
   ```mkdir -p projects/<your_project-name>```
 - Then make an empty git repository inside your <your_project-name> dir by typing
   ```git init [for the newer git versions] else git init -b main```
 - You have now created an empty git repo LOCALLY. View the status of it by
	    ```git status
	    ###If you have created a file which you were thinking to add to the repo for example "read.txt":
	       On branch master
	
	No commits yet [### This is the staging area. This is where you'll see all the contents which youll be commiting]
	
	Untracked files: 
	  (use "git add <file>..." to include in what will be committed)
		read.txt
	
	nothing added to commit but untracked files present (use "git add" to track)
	else you'll get 
	On branch main
	Your branch is up to date with 'origin/main'.
	
	nothing to commit, working tree clean
	```
 - Add files to commit using
   	``` git add <file_name>
 - Now, Go to github and create a new repo [Once per new project]
 - Next, we can access that repo by using
    ```git remote add origin "Your repo link"
    	use git remote -v to check where out git points to```
 - Commit your changes by
   	```git commit -b "Your comment"```
 - We can change our username and email cause when we use `git log` we get "[main...]". Just helps to know who did it.
    ``` git config --global user.name "Your name"
    	git config --global user.email "Your email"
    ```
- Now we push by doing:
  ```git push -u origin main
  	itll show
  	username ... : "Your username"
  	password ... : "Your SSH KEY or Go to dev settings and create a key"
 ```
- Done
