git-advantages-presentation
===
This is a technical lecture about why you could consider switching from **SVN** to **GIT**


Talk Time
---------
  about 1 hour.


Contents
--------



## Why Git over SVN?

### Linus Torvalds designed it with his own experience of maintaining a large distributed development project like Linux and with his knowledge of the file systems.
  - Designed it as a necessity for a replacement for BitKeeper which he used for Linux kernel development until the copyright holder withdrew free use.
  - Linus hates CVS and SVN(quote: "if there are any SVN users in the audience, you might want to leave") and did research at that time looking for a solution that
    - supported distributed workflows
    - safeguard against data corruption either accidental or malicious.
    - had high performance(was very fast)
  - Nothing that he looked at satisfied him at that time, so he came up with Git because "his brain was not rotten away by CVS way of thinking".
  - development started 3 April 2005
  - on 26 June 2005 they were using Git to manage the release for the Linux kernel.


### Git is distributed while SVN is not
  - Distributed means
  - Being a DVCS, everyone has their own version of the whole repository.
    - With SVN, you typically only receive a specific snapshot from the project central repository by doing a checkout.
    - With Git, you **clone** the entire repository, the **whole history** of the project from the inception to the current state.
    This means that immediately after the clone, there is basically no information about that project that the server you cloned from has that you do not have.

### Go offline, go out, enjoy the speed
  - Now that you have your own clone of the repository you can remove the network from the equation and just work with the local repo.
  - Network introduces a lot of latency, by getting rid of it, Git is extremely FAST since all the operations except pull and push are LOCAL.
    - Commit changes.
    - Create branches.
    - Merge branches.
    - Performing a diff.
    - View file history.
    - Obtain any revision of a file.
  - You can browse through the file history and file revisions without having network access. You can work offline.
  - Having the whole repository on your computer would make you think it will take a lot of space. This is not true(see below)
  - Cloning an entire git repository can be quite as fast as checking out from SVN the latest version.

### Git inner workings
  - Git is "content-addressable filesystem  with a VCS user interface written on top of it" which basically translates as being a key-value storage to which you can write using VCS commands.
  - The **key** being the checksum of the content that you commited.(So retrieval is done by content's value rather by filename hence the "content-addressable" name)
    - The checksum is actually a **SHA1** hash digest of the content of the commited files. When you commit, Git spits back this SHA1 key by which the commit will be referenced. This can be thought as like SVN "revision nr"(which always increase by 1) equivalent. You can always tag your commit for more humanly readable

  - The **content** being an object containing the tree structure with the files that you commited.
  - The commit content also contains reference to the one before it.
  - Git compresses up the files(uses zlib compression) and packs up the files to be space efficient. "Packing up" means that a small change to a large file in a different version, packed together will compress greatly since they are very similar and not need two versions of the file being kept. That means that even if there are many revisions of the files disk size will not grow much.

### Can have it locally, you don't need a remote server to have version control for a project
  '''
    $sudo apt-get install git-core
    $git init
    $git add .
    $git commit -m "initial commit" '''


### Branching is 'cheap' with Git.
  - 40 bytes cheap. A branch is just a pointer(bookmark) of the last commit in the branch.
![Branch is a bookmark to a last commmit in the branch](http://www.gitguys.com/gitguys/branches/images/img4.png)
  - With SVN you see branching as something "big" to be done when releasing a new version of the project or for a different brand/customer because others will see the branches you create and may not have meaning for them.
  - in SVN branching means make another directory with a full copy of the source that you branched from so it's space consuming.
  - Many developers when working with svn branches keep local directories with the checked out branches and *cd* through them and make commit to a specific branch.
     - /trunk,  /2.10, /2.11
  - With Git you can keep work in the same directory when switching to another branch.
     - We create a new branch and switch to it: git checkout -b release2.10 Now every commit goes into that branch.
     - Case 1: No modified files exists: Switching to another branch is a local operation and the existing files in the working directory get replaced with the one in the branch.
     - Case 2: There are file modified and uncommited. Switching to another branch attempts to merge the files in working dir into the ones in the branch.
     - Working on some feature while some issue on production appears? Git has a simple "stash" concept in which you push you current work in progress, then switch branch afterwards unstash your files and continue working.
  - Git encourages you to create "Topic branches" or "Feature Branches" - a branch for every new issue/feature you are working on.
  - With Git if you experiment a feature on a branch and feel that it's a dead end you can always choose to delete or not to push it further and nobody else would ever see/know about it.


### Merging with Git
  - A merge commit has two parents so it keeps reference to the commits that led to it.
![Merge commit has two parents](http://www.gitguys.com/gitguys/merging/images/img1.png)

  - This allows to distinguish about who actually made the changes from which merged them.

### Multiple Remotes
  - You don't have to collaborate only with the repository from which you cloned. However at cloning time you get an easy reference to the repository you clone from by the name *origin*.
  - You can always add other remote repositories and pull from different remotes to yours.
  - This leads to the possibility of having very diverse workflows:


### Workflows
  - Moving away from giving "commiter" status to a developer to a more pull changes that add value to a project.
  - Github way of working. Suppose you have a simple/great idea/fix for a project that you want to share:
        1. Fork the repository - a clone of the original project owner repository
        2. Create a branch and make your changes.
        3. Pull in periodically commits by others developers to the original and merge them into yours.
        4. To share the work send the owner a **Pull request**: "Hey I've just fixed your mispelled words. Why don't you review what I've done and if you like it you can pull the changes from my repository to yours. You're awesome!"
  - Other ex: Linus for Linux kernel has his "lieutenants" that are in charge of different kernel modules. They pull in the contributors changes and in turn Linus picks from their what gets pulled into his repository
![Dictator workflow](http://git-scm.com/figures/18333fig0503-tn.png)

### Multiple Backups
  - Since you clone other repositories it means that there are multiple backups even if one of the machine fails.
  - Tampering foolproof.
       - Since every commit is stored and addressable by the checksum(SHA1 hash) of the content and other metadata like the commiter and commit message.
       - Change even a single bit an the resulting checksum will differ.
       - Commit are chained together as every commit has a reference to the previous one(or two if it was the result of a merge of branches) so any change in the "links" would break the chain.

### Cleaner
  - SVN infects all your sub directories with a hidden .svn directory.
  - With GIT you only have .git at the top of your directory and that is ALL the repository data is being held.
![Multiple .svn directories vs single top .git directory](http://cl.ly/462l3n1g2e1l3B3n450X/Untitled-2.png)

### Git Hooks
  - Deploy in Heroku can be done just by pushing the changes to repository.
  - Same for [Github pages](http://pages.github.com/).


### Disadvantages to GIT over SVN
  - No fine control of user rights readonly for some users. You can have clone of repository to be readonly from which you only push to.
  - No way to checkout just a specific branch of a project. (You can however make a "shallow" clone by specifying how much steps back you want the history, however you'd not be able to push the changes to that repository).
  - Binary files used in the project at some time(that may be of no use in the latest development) will be downloaded with the cloning.

Sources of info
----------------
  - Linus Torvalds on git http://www.youtube.com/watch?v=4XpnKHJAok8[p is what made it popular. Explains in Linus own words the reasoning behind it.
  - Interactive and Fun way to learn it http://try.github.com/
  - Video presentation of what commits and branches mean http://blip.tv/open-source-developers-conference/git-for-ages-4-and-up-4460524
  - Free book on Git: http://git-scm.com/book
  - Good tutorials http://www.gitguys.com/topics/
  - http://thinkvitamin.com/code/why-you-should-switch-from-subversion-to-git/
  - http://www.slideshare.net/emilerl/git-presentation-purple-scout-ab-malm
  - http://think-like-a-git.net/sections/graphs-and-git/references.html
  - Also explicit comparison of git vs svn http://www.codeforest.net/git-vs-svn
  - http://marklodato.github.com/visual-git-guide/index-en.html


Git cheat sheets
----------------
  - http://refcardz.dzone.com/refcardz/getting-started-git
  - http://zrusin.blogspot.com/2007/09/git-cheat-sheet.html
  - http://ndpsoftware.com/git-cheatsheet.html (interactive)
  - http://devcheatsheet.com/tag/git/ (list of some good sheets)
