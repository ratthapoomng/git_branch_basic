### Using GIT

## Prerequisite
Please download and install git on your pc (https://git-scm.com/downloads). If you are not sure, you can google the process according to your OS (MacOS, Windows, Linux).

## Cloning
Cloning basically means clone the repository from remote(in this case, monash gitlab server) repository into local machine(your pc).
The repository in your machine will be called "local repository".

To clone a repository, in your machine terminal, navigate to the path you want to clone a repository, aka the folder you want to download the repo into,
then run this command

```
git clone <insert repository url here>
```

You usually have two options to do it, via SSH or via HTTPS, either one is fine.
You can find those two URLs by pressing the code dropdown on the main page or the repository.
Note: Monash gitlab only allow you to use SSH, so skip the HTTPS part.

If you use HTTPS, it will usually authenticate by asking you to login.
The HTTPS URL will look like this: https://git.infotech.monash.edu/fit5136/fit5136-s1-2024/thursday-2-4-3.git

If you use SSH, you will have to put your PC SSH public key and add it to the gitlab profile(Edit profile>SSH Keys>Add new key) first.
The SSH URL will look like this: git@git.infotech.monash.edu:fit5136/fit5136-s1-2024/thursday-2-4-3.git

Now that you cloned a repository, please change directory(using cd command) to the repository that you have cloned.
All of the commands below will not work if you are outside a git repository.

## Pulling
Pulling generally means pulling updates from the remote git repository to our local branch, unless specified otherwise.
It is used to sync your local repo with the remote repo. 
The difference between cloning and pulling is that cloning downloads a new repository, while pulling updates the current repository.

The command is as follow
```
git pull
```

Notice that you don't have to specify the URL at all, because the values needed are already inside a config file in the repository.
You can access the file by turning on hidden files in folder option, then go to .git folder inside the repository and read the config file.

## Pushing
Pushing generally means pushing the updates from our local branch to the remote repository, which is basically reverse of pulling. 
Most of the time, the remote repository is referred to as "origin" in git system. 

The command is as follow
```
git push <remote> <branch name>
```

In this case, we can subsitute remote with origin. In the future, there might be other remote name, but for now we can stick with origin.


## Branching
Now you should start asking "How about branch? What is it?".
In most of the repositories, there will be either a main or master branch, which is the branch that contains the working code.
However, if many people works on the same branch at the same time, it could creates conflict when pushing the code to the remote repository.
To prevent that, we will do something called "branching".
Branching basically means creating a snapshot of the current(or main) branch into you new branch, and then you can start working on your new branch without affecting the current(or main) branch.

You can create a new branch using this command
```
git branch <new branch name>
```

You can also see the current branch you are on using this command
```
git branch
```


## Changing branch
The next one after creating a branch is changing your current branch. You can do this using the following command
```
git checkout <branch name>
```

## Adding changes
Now that you are in a new branch, let say you write some new code and you want to save the changes to git.
You can do this by calling the following command
```
git add <path to file>
```
This will add the file into git staging area, which then will be committed(saved) to git system later.

You can also add all the files at the same time using the following command
```
git add .
```
The "." in this case refers to the current directory, which means git will add every files inside this directory.
However, you need to be cautious because it can also add unneccesary files or sensitive files.


## Checking staging area
Now if you have added all the files, you can check the difference between the last commit and the staging area by using the following command
```
git diff --staged
```
This will shows you which part of the files will be added or removed when committing(saving) the changes to git.

## Resetting changes
If you added a wrong file, you can use the following command to reset it back
```
git reset <path to file>
```

you can also reset all the added changes using the follow command
```
git reset
```

Please note that the reset does not change content inside the file, unless specified via option.

## Committing changes
Once you checked, you can commit the changes in the staging area to the git system using the following command
```
git commit -m '<insert your message here>'
```
You should always use useful commit message, basically what you did between the last commit and this commit. 

## Merging
Once you finished experimenting and testing you code, and also done committing every changes, you can push your branch to the remote repository and then merge into the main branch.
In order to merge your branch to the main branch(or any other branch), you will have to create a merge request (or pull request if you are using github), which can easily be done in the gitlab UI. Then you can merge your branch into the main branch.

## Merge conflict
In some cases, there might be a merge conflict when trying to merge into the main branch. This happens when multiple branch working on the same region of the same file are subsequently merged, which create a conflict on which change should the git system keeps. This is why git pull should be called everytime you want to create a new branch, to mitigate this issue. When having a conflict, easiest way would be solving with UI tools (Most online git repo hosting services like gitlab and github has this built-in already). 

## Quick Summary
1. clone
2. pull
3. create new branch
4. switch to the new branch
5. write/change code + test
6. commit
7. repeat from 5 if needed, else go to 8
8. push your branch
9. merge
10. switch back to main branch
11. restart from 2


## Example
1. clone
```
git clone https://git.infotech.monash.edu/fit5136/fit5136-s1-2024/thursday-2-4-3.git
```

2. pull
```
git pull
```

3. create new branch
```
git branch feature-1-login
```

4.1 Switch the new branch
```
git checkout feature-1-login
```

4.2 Check if the current branch is correct
```
git branch
```

5. write/change code + test
6. commit
add file to staging area
```
git add AccountManagement.java
```

check the diff
```
git diff --staged
```

```
git commit -m "Added AccountManagement"
```

7. skip to 8
8. push to remote repo
```
git push origin feature-1-login
```

9. create merge request and merge (on gitlab UI)

10. switch back to master branch
```
git checkout main
```

11. repeat 2.

