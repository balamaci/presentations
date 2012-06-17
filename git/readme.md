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
  - Linus hates CVS and SVN.
  - Linus is the guy who created Linux.
  - QED.

### Git is distributed while SVN is not

  - Being a DVCS, everyone has their own version of the whole repository.
    With SVN, you typically only receive a specific snapshot from the project central repository by doing a checkout.
    With Git, you clone the entire repository, the whole history of the project from the inception to the current state.
    This means that immediately after the clone, there is basically no information about that project that the server you cloned from has that you do not have.
  - Cloning an entire git repository can be quite as fast as checking out from SVN the latest version.

### Go offline, go out, enjoy the speed
  - Now that you have your own clone of the repository you can remove the network from the equation and just work with the local repo.
    You can do operations such as tagging, branching and diff without having to be connected to the central server.

  - Network introduces latency, get rid of it and the result is damn faaast.

### Branching is 'cheap'.
  - 40 bytes cheap.
  - switching to a branch


### Multiple Remotes


### Multiple Backups
  Again an added bonus of the repository clone.

### Clean




### Git Hooks
  - Deploy in Heroku



Sources of info
----------------

  - http://thinkvitamin.com/code/why-you-should-switch-from-subversion-to-git/
  - http://www.slideshare.net/emilerl/git-presentation-purple-scout-ab-malm
  - http://think-like-a-git.net/sections/graphs-and-git/references.html
  - http://blip.tv/open-source-developers-conference/git-for-ages-4-and-up-4460524


Git cheat sheets
----------------

  - http://zrusin.blogspot.com/2007/09/git-cheat-sheet.html
  - http://ndpsoftware.com/git-cheatsheet.html (interactive)
  - http://devcheatsheet.com/tag/git/ (list of some good sheets)
