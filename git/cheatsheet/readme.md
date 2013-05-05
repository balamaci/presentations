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
   **master**             :default development branch - trunk in svn speach

   **origin**	        :default upstream repository - the one you cloned from

   **stage** / **index**  :what goes into the next commit

   **HEAD**	            :pointer to the current commit, parent to the next commit normally a reference to the current branch which is a symbolic reference(but for a 'detached HEAD', HEAD points directly to a commit)


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

   --mixed(**default**) : overwrittes the staging area with what was in the specified commit where HEAD moved but leaves working dir unchanged.         

   --hard **WARNING**: you'll loose the work of the changed files because also the working directory is rewritten to match the image found in the commit.

   http://marklodato.github.io/visual-git-guide/index-en.html

### Undoing the modifications to a file
   You did some changes to a file and that file now appears modified and you want to drop the changes and get it as it
   was in the specified commit:

    $ git checkout [commit_sha or reference] path_to_file/file.ext

   -- the following overwrittes file with the one from the index	
    $ git checkout path_to_file/file.ext

   -- overwritte all the files(index and working directory) with the one from the HEAD
    $ git checkout -f

### git reset vs checkout
   **git reset [commit_id]** the references(HEAD and current branch) move to the specified commit. According to the parameter
      --soft, --mixed, --hard the working directory or index is influenced as described above.

   **git checkout** the HEAD moves to the specified commit. Might end up in 'DETACHED HEAD' mode.

  Ex:
    $ git reset --hard
    $ git checkout -f
  also do the same thing, 

## Referencing commits
### Difference between ^ and ~
  For example HEAD~1 and HEAD^1 both mean same thing.
    ^ - is usefull when refering to merge commits. Since merge commits can have at least two parents, the HEAD^2 will mean 2nd parent of the commit, HEAD^3 third parent, etc. This is different than HEAD^^ which will mean the parent of the first parent. 
    ~ - means the commit before. HEAD~10, the 10th commit before current.
   Ex: 10fead^2^~10 - means find 2nd parent of 10fead, get the first parent of that commit and go back 10 commits.

### Using range
  We can also specify a range of commits. 
      $ git log fe2c3a12..HEAD

### Listing files modified in a certain commit

    $ git show [commit_ref] --stat --oneline

   or

    $ git diff-tree --no-commit-id --name-only -r [commit_ref]

   

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


   Display also what files have been modified at each move

    $ git reflog --stat --date=relative

## Throwing away your last commit(s)
   You need to be careful about undoing commits - check the case where you are that you're not rewritting the history some others based work on.

### 1. Commits have not been pushed to the remotes

    $ git reset HEAD~3

   - See 'reset' extra parameters to reset that would influence or not the working directory.
   See bellow 'reflog' reference to undo.

### 2. Commits have been pushed but nobody pulled and based work on them.

    $ git reset HEAD~3
   If you try to push to origin it will complain that you are behind commits since 'origin/HEAD' seems to be pointing
   into the future. So you need to 'force' your push. Be carefull that

    git push -f 
   To force your new version of history upon the repository.


### 3. Commits have been pushed and work done on them.

    $ git revert 3de4af b3f8d5a 

   This will create 2 commits that each reverting the changes of the specified commits.

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

   Listing what files the stash entry modified:
    $ git stash show stash@{2}

## Local multiple repositories
   You can still have multiple clones of the repository locally and pull and merge between them.

    $ git clone . /new/fs_path/
   
   You can even pull in changes on them and pull
    $ git pull /new/fs_path

## Merging
### Steps to fix a merge conflict:
   1. Merge the conflicting files to a final form. Remove any <<<< ==== and >>>> from them.
   2. You need to **add** the files to the 'stage'.
   3. Commit to mark the merge commit with the appropiate message.

  - You can overwritte a file with 'ours' or 'theirs' versions by doing:

    $ git checkout --theirs [path_to_file/file]
    $ git checkout --ours [path_to_file/file]

### Merge abort
    $ git merge --abort
   or
    $ git reset --merge

## Getting to a specific point in history
### Fix a certain issue on a release
   Specific example that we need to fix a bug in version 2.31.
   
   1. We list all the commits and find out the commit we're interested. We'd most likely have a tag, but we can reference a commit with it's hash. So to get back to that particular point:
  
  Detailed way    
  
    $ git checkout 101739f #you get a message saying that you are in 'detached HEAD' state - issueing a
    $ git branch -a

    //create a branch and commit to it
    $ git branch fix_issue_23

   Doing the same but with one command
   
    $ git checkout 101739f -b fix_issue_23

   2. Merge the changes on the branch back into master or whatever branch you need it on.

## Rebase

### Interactive
   Great way to delete some local commits, reorder or group together.

    $ git rebase -i HEAD~5


## Cherry pick
   Pick a specific commit and apply it to the current branch. It will create a different SHA1 commit that has the same contents of the specified commit.
   
    $ git cherry-pick fix_security^^


## Pulling from remotes:

### Avoiding unnecessary merge commits when pulling:
   When you want to push your changes to a branch, but someone else already pushed before you, you have to pull in their changes first. Normally, git does a merge commit in that situation.
   Such merge commits can be numerous, especially between a team of people who push their changes often. These merges convey no useful information to others, and litter the project’s history.

    $ git pull --rebase
   
## Pushing to remotes:

### Short summary of what's changed on the remote.

#### Files that have changed, summary

    $ git diff --stat [remote/branch] HEAD

   -- the following considers files in working dir also
    $ git diff --stat [remote/branch]

   The --stat provide the short summary of involved files. It could be used with other commands like 'log' for example.

#### Commits missing from remote, list

    $ git cherry [commit] [remote/branch]
    $ git cherry HEAD~5 origin/develop


## Log filtering
### Certain file history changes
    $ git log --oneline pom.xml


### Limit 'Between dates'
    $ git log --since=2013.1.1 --until=2013.4.15
    $ git log --since '1 day ago'

### Limit 'Changes made by ...'

    $ git log --author <author_email>

## Git Blame
### Finding out who made changes to a certain line of code

    $ git blame -w  # ignores white space
    $ git blame -M  # ignores moving text


## Other interesting commands

### Find commits where a change matches regex pattern

  Actual search of the commits for diffs that added or removed a string. 

    $ git log -G<regex> [file_path/file_name]
    $ git log -G'checkstyle'

  added some filtering

    $ git log -G'checkstyle' --since=2013.1.1 --until=2013.1.10

### Find first commit with a 'commit message' that matches regex
    
    $ git show :/checkstyle

### Find if commit is part of a release:
    
    $ git name-rev --name-only 50f3754

### Don't show as modified even if file is tracked.
   Usefull for a config property file that even if it's in the repository, every user would have it locally
   modified with his settings.
	
    $ git update-index --assume-unchanged <FILENAME>

### More concise git status    
    $ git status -sb

  Don't show untracked files(-uno aka --untracked=no)
    
    $ git status -sb -uno

### Clean all untracked files in directory
    $ git clean -f


## Git Aliases - modify you .gitconfig file and add

    [alias]
       co = checkout
       st = status
       ci = commit -v --uno
       br = branch
       ls = log --pretty=format:\"%C(yellow)%h %C(blue)%ad%C(red)%d %C(reset)%s%C(green) [%cn]\" --decorate --date=short
       graph = log --graph --pretty=format':%C(yellow)%h%Cblue%d%Creset %s %C(white) %an, %ar%Creset'
       sl  = stash list
       ss  = stash save
       sp  = stash pop
       standup = log --since '1 day ago' --oneline --author 'author_email@mail.com'

       #alias for showing the aliases
       alias = config --get-regexp 'alias.*'



