git-cheatsheet
===

Contents
--------


## Git Setup

    // Show all your config settings
    git config --list

    // Global setup of name and email - this will be visible to others
    // also could be used by CI tools to send the email to whom broke the build
    git config --global user.name "Your Name"
    git config --global user.email "your_email@whatever.com"

    // Use colors in terminal
    git config --global color.ui true

    // Setup Line Ending Preferences (UNIX/Mac)
    git config --global core.autocrlf input
    // Setup Line Ending Preferences (Windows)
    git config --global core.autocrlf true

## Concepts
    **master** 	        :default development branch - trunk in svn speach

    **origin**	        :default upstream repository - the one you cloned from

    **stage**/**index** :what goes into the next commit

    **HEAD**	        :pointer to the current commit, parent to the next commit
                         normally a reference to the current branch which is a symbolic reference(but for a 'detached
                         HEAD' a direct commit)

    **HEAD^**	        :previous commit
    **HEAD-1**          :previous commit same as above
    **HEAD-4**	        :previous 4th commit ago from current


## Branching
### Creating a new branch
    $ git checkout -b new_branch

   # This is the same as doing:

    $ git branch new_branch
    $ git checkout new_branch

### Listing branches including remote
    $ git branch -a

## Commiting
### Removing a file from stage
   If you want to remove a file from the stage(index) to not go in the next commit but keep it with the current modification

    git reset HEAD path_to_file/file.ext

   Removing all the added files from the stage but leave with your work intact
    git reset HEAD

### **'git reset'** - The reset command moves the current branch to another position, and optionally updates the stage and
the working directory.
It also is used to copy files from the history to the stage without touching the working directory.

     --soft moves HEAD but doesn't touch the staging area or the working tree(Neither the modifications or what goes into the next commit is affected)
     --mixed it's the default
     --hard WARNING

### Undoing the modifications to a file
   You did some changes to a file and that file now appears modified and you want to drop the changes and get it as it
   was in the last commit:

    $ git checkout path_to_file/file.ext

    //
    $

### git reset vs rebase
   *git reset* moves only the reference, while *git rebase* makes rewrites to the previous commits.
### git reset vs checkout

### Listing files modified in a certain commit
    git diff-tree --no-commit-id --name-only -r [commit number]

### Reflog
   Git keeps a log of all ref updates (ex: checkout, reset, commit, merge).
   Very usefull if you need to reach a previous commit. Think of you get back too much in history.
   You can view it by typing:.

    $ git reflog
    530dca7 HEAD@{0}: reset: moving to 530dca7
    4f91201 HEAD@{1}: checkout: moving from 530dca70b736e4465ff688e9e05046cbccc9b4f3



### Undoing all the modifications to a file

## Throwing away your last commit(s)
   Why would you want to do it?
    git reset 101739f

   If you change your mind
    git reset HEAD-3

### Stash - usefull for saving your work in progress

   Place current modifications in a stash

    $ git stash
     or better give also a name to be more clear
    $ git stash save "work in progress on blue theme"


   Listing all the stash entries:

    $ git stash list


## Getting to a specific point in history

   Specific example that we need to fix a bug in version 2.31.
   1. We list all the commits and find out the commit we're interested. We'd most likely have a tag, but we can reference a
   commit with it's hash. So to get back to that particular point:

    $ git checkout 101739f    #you get a message saying that you are in 'detached HEAD' state - issueing a
    $ git branch -a

    //create a branch and commit to it
    $ git branch fix_issue_23

    //merge the changes on the branch back into master or whatever branch you need it on


## Aliases:

    [alias]
       co = checkout
       st = status
       ci = commit
       br = branch
       ls = log --pretty=format:\"%C(yellow)%h %C(blue)%ad%C(red)%d %C(reset)%s%C(green) [%cn]\" --decorate --date=short
       graph = log --graph --pretty=format':%C(yellow)%h%Cblue%d%Creset %s %C(white) %an, %ar%Creset'

       //alias for showing the aliases
       alias = config --get-regexp 'alias.*'




