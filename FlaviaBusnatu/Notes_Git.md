# GIT
At this moment we are working on global table, which means we are setting global configuration, relevant everywhere for this user, until overwritten  
--system - table relevant for the whole machine  
--global - for the current user  
--local (default) for the current repository  

example:  
git config --global user.name "John Doe"  
git config --global user.email johndoe@myemail.com  

`git config --list -a ll` settings are printed.  

Config of the repository  
In the background system created a directory and initialized git repo.There is a .git directory, which is a hidden one (. on the beginning). This directory is created during git init operation.    
- hooks directory contains all custom hooks. These are small (usually) scripts which have to be executed before commit, or after, before push, etc  
- branches - this is deprecated  
- HEAD - pointer to the current branch and its latest commit  
- config - configuration file for the repository  
- info - the place where you stage the files using git add  
- refs - the current state of the whole repo  
- objects - commits, trees and blobs are stored here  
- logs  
- description - description of the repository  

## Initialize the repository  
If you start work on the new repository locally (what means repo wasn't cloned earlier), you need to initialize the repository and build all internal structure needed to work with version control  
Create a directory -> `mkdir test-repo`  
and navigate there -> `cd test-repo`  
The directory is obviously empty -> `ls -al`  
Now it is time to initialize repository -> `git init`  

We are already in proper directory, and git was initialized. Now it is the time to create first file  
`git status`  
In the output you should see that our new file is untracked  
`git add newfile` -> this command added the file to the stage area  
`git add .` will add all files from your current directory and subdirectories, not from repository's root  

Commit is a proces to create a revision of the file. A version which will be stored in Version Control  
`git commit newfile -m "my first commit"`  
By using -a instead, we are able to commit all files from repository. By using . we will commit changes from current path recursively  

## Remove files from stage  
you want to remove all staged files from index -> git rm --cached testfile-01  
`git rm --cached -r . `(we used . to say everything from here and -r which means recursive)  

`git checkout testfile-01 `-> to reset the file to the state from previous commit  
`git checkout .` -> to remove all changes  

git reset moves the current pointer in HEAD and branch refs to specific state  
Reset has three main ways of operating, but we will touch only two of them  
`git reset --soft HEAD~1` -> With --soft parameter we came back to the previous HEAD of the repository, but all changes which we commited are unchanged  
`git reset --hard HEAD~2 `-> We came back two more commits (~HEAD~2) and we said, this time we want to not only move back, but also we want to remove all changes which were done  

`git revert --no-edit HEAD `-> we informed git that we don't want to pass any message and we ask to use default  
`git diff`-> allows to check the differences between HEAD and current working directory  
`git diff --staged `-> In order to see the difference between staged work and HEAD  
`git show `-> This will give you information similar to git log  

Remember to quit git log with q. If logs are longer than your screen, it always goes to interactive mode  
git log --oneline -> hows only most important info about commits. You have only hash and commit message  
git shortlog -> to see how many lines were added or removed in each commit  

git log --graph --pretty="%C(yellow) Hash: %h %C(blue)Date: %ad %C(red) Message: %s " --date=human -> to have more colorful output  

git log --author="John Doe" will show all commits done by specific author  
git log -- testfile-01 -> we inform git that we have files, not branches in mind  
git log --merges -> to look for "merge" messages  
git log --no-merges -> to exclude merge commits  

## Resolve conflict  
Here is our conflict.  
- <<<<<< - this is what we have in current branch  
- ====== = center of our conflict  
- '>>>>>>' - this wants to be merged / added  
To solve ->remove lines from 2 to 5(prima linie <<<<, ultima linie >>>>)  

.gitignore -> Git gives us the possibility to control what will be synchronized with remote and what will be not  

## GIT TAGS  
`git tag -a v2.0 -m "version 2.0. A lot of new features"`-> to create a tag  
`git tag ` ->to show the tags  
`git tag -d v1.0 `-> to delete a tag  
