git-advantages-presentation
===
This is a technical lecture about why you could consider switching from SVN to GIT


Talk Time
---------
  about 30 minutes


Contents
--------

## Why Git over SVN?

### Linus Torvalds is using it
  - Working on the linux kernel on it.
  - Linus hates CVS and SVN and did research at that time looking for a solution that was .
  - Nothing that he looked at satisfied him at that time, so he came up with git because "his brain was not rotten away by CVS way of thinking.

### Git is distributed while SVN is not
  - Distributed means
  - Being a DVCS, everyone has their own version of the whole repository.
    With SVN, you typically only receive a specific snapshot from the project central repository by doing a checkout.
    With Git, you clone the entire repository, the whole history of the project from the inception to the current state.
    This means that immediately after the clone, there is basically no information about that project that the server you cloned from has that you do not have.

### Go offline, go out, enjoy the speed
  - Now that you have your own clone of the repository you can remove the network from the equation and just work with the local repo.
  - Network introduces latency, by getting rid of it GIT is extremely FAST since all the operations except pull and push are local.
    - Commit changes.
    - Create branches.
    - Merge branches.
    - Performing a diff.
    - View file history.
    - Obtain any revision of a file.

  - Cloning an entire git repository can be quite as fast as checking out from SVN the latest version.


### Branching is 'cheap'.
  - 40 bytes cheap. A branch is just a pointer to a commit.
  - switching to a branch
  - in SVN branching means make copy of the

### Git workings
  -

### Multiple Remotes

### Workflows
  - Moving away from giving "commiter" status to a developer.
  - Github


### Multiple Backups
  Again an added bonus of the repository clone.

### Clean




### Git Hooks
  - Deploy in Heroku


### Disadvantages to GIT over SVN
  - No fine control of user rights readonly for some users. You can have clone of repository to be readonly from which you only push to.
  - No way to checkout just a specific branch of a project.
  - Binary files used in the project at some time(that may be of no use in the latest development) will be downloaded with the cloning.


Sources of info
----------------
  - Linus Torvalds on git http://www.youtube.com/watch?v=4XpnKHJAok8[p
  - http://thinkvitamin.com/code/why-you-should-switch-from-subversion-to-git/
  - http://www.slideshare.net/emilerl/git-presentation-purple-scout-ab-malm
  - http://think-like-a-git.net/sections/graphs-and-git/references.html
  - Also explicit comparison of git vs svn http://www.codeforest.net/git-vs-svn
  - Video presentation of what commits and branches mean http://blip.tv/open-source-developers-conference/git-for-ages-4-and-up-4460524


Git cheat sheets
----------------

  - http://zrusin.blogspot.com/2007/09/git-cheat-sheet.html
  - http://ndpsoftware.com/git-cheatsheet.html (interactive)
  - http://devcheatsheet.com/tag/git/ (list of some good sheets)
