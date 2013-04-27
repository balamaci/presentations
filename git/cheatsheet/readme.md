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

   **stage** / **index**  :what goes into the next commit

   **HEAD**	            :pointer to the current commit, parent to the next commit
                         normally a reference to the current branch which is a symbolic reference(but for a 'detached
                         HEAD', HEAD points directly to a commit)


## Branching
### Creating a new branch
    $ git checkout -b new_branch

   # This is the same as doing:

    $ git branch new_branch
    $ git checkout new_branch

### Listing branches including remote ones
    $ git branch -a

## Commiting
### Removing a file from stage
   - Removing a file from the stage(index) to not go in the next commit but keep it with the current modification

    git reset HEAD path_to_file/file.ext

   - Removing all the added files from the stage but leave with your work intact
    git reset HEAD

### Deleting a file from the repository
   - You need to tell git just to remove it from tracked files and thus it's going to be ignored and no longer present
    in future commits

    $ git rm filename


## Reset
#### The reset command moves the **current branch** and **HEAD** to another position.

   Optionally updates the stage the working directory depending on the extra parameters.

   --soft : moves HEAD and current branch pointer, but doesn't touch the staging area or the working tree
        (Neither the ongoing file modifications or what goes into the next commit is affected)

   --mixed(**default**) : overwrittes the staging area with what was in the specified commit where HEAD moved but leaves
         .

   --hard **WARNING**: you'll loose the work of the changed files because also the working directory is rewritten to match
        the image found in the commit.

   http://marklodato.github.io/visual-git-guide/index-en.html

### Undoing the modifications to a file
   You did some changes to a file and that file now appears modified and you want to drop the changes and get it as it
   was in the last commit:

    $ git checkout [commit_sha or reference] path_to_file/file.ext

    //
    $

### git reset vs checkout
   **git reset** the references(HEAD and current branch)


## Referencing commits
### Difference between ^ and ~
    For example HEAD~1 and HEAD^1 both mean same thing
    ^ - is refering to merges
### Using range

### Listing files modified in a certain commit
    git diff-tree --no-commit-id --name-only -r [commit number]

## Recover from mistakes:
### Reflog:
   Git keeps a log of all references updates (ex: checkout, reset, commit, merge).
   Very usefull if you need to reach a previous commit that was made unreachable.
   You can view it by typing:.

    $ git reflog
    530dca7 HEAD@{0}: reset: moving to 530dca7
    4f91201 HEAD@{1}: checkout: moving from 530dca70b736e4465ff688e9e05046cbccc9b4f3

    530dca7 is the commit where the HEAD is currently pointing
    4f91201 is how it got there.
###


### Undoing all the modifications to a file


## Throwing away your last commit(s)
    You need to be careful about undoing commit sure

### Commits have not been pushed to the remotes

    git reset HEAD~3

   - See 'reset' extra parameters to reset that would influence or not the working directory.
   See bellow 'reflog' reference to undo.

### Commits have been pushed but nobody pulled and based work on them.
    git reset HEAD~3
   If you try to push to origin it will complain that you are behind commits since 'origin/HEAD' seems to be pointing
   into the future. So you need to 'force' your push. Be carefull that

    git push


### Commits have been pushed and work done on them.


## Stash - usefull for saving your work in progress

   Place current modifications in a stash(working directory and index). Think of the stash more like a stack.

    $ git stash
     or better give also a name to be more clear
    $ git stash save "work in progress on blue theme"

   Extract the top of the stack and apply the changes to the current .
    $ git stash pop

   Listing all the stash entries:

    $ git stash list
    stash@{0}: WIP on master: 049d078 added the index file
    stash@{1}: WIP on master: c264051... Revert "added file_size"

## Merging
### Steps to fix a merge conflict:
    1. Merge the conflicting files to a final form. Remove any <<<< ==== and >>>> from them.
    2. You need to **add** the files to the 'stage'.
    3. Commit to mark the merge commit with the appropiate message.

    - You can overwritte a

### Merge abort
    $ git merge --abort

## Getting to a specific point in history

   Specific example that we need to fix a bug in version 2.31.
   1. We list all the commits and find out the commit we're interested. We'd most likely have a tag, but we can reference a
   commit with it's hash. So to get back to that particular point:
   A. Detailed way
    $ git checkout 101739f    #you get a message saying that you are in 'detached HEAD' state - issueing a
    $ git branch -a

    //create a branch and commit to it
    $ git branch fix_issue_23

   B. One command
    $ git checkout 101739f -b fix_issue_23

   2. Merge the changes on the branch back into master or whatever branch you need it on

## Rebase


## Reverting pushed commits
   When you already pushed to the remote repositories and someone else has gotten the changes and


## Pulling from remotes:

### Avoiding unnecessary merge commits when pulling:
   When you want to push your changes to a branch, but someone else already pushed before you, you have to pull in
    their changes first. Normally, git does a merge commit in that situation.

   Such merge commits can be numerous, especially between a team of people who push their changes often.
    Those merges convey no useful information to others, and litter the project’s history.



## Other interested

### Find if commit is part of a release:
    $ git name-rev --name-only 50f3754

### Don't show as modified even if file is tracked.
   Usefull for a config property file that even if it's in the repository, every user would have it locally
   modified with his settings

### More concise git status
    $ git status -sb

## Git Aliases

    [alias]
       co = checkout
       st = status
       ci = commit
       br = branch
       ls = log --pretty=format:\"%C(yellow)%h %C(blue)%ad%C(red)%d %C(reset)%s%C(green) [%cn]\" --decorate --date=short
       graph = log --graph --pretty=format':%C(yellow)%h%Cblue%d%Creset %s %C(white) %an, %ar%Creset'

       //alias for showing the aliases
       alias = config --get-regexp 'alias.*'




