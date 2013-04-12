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
master	:default development branch - trunk in svn speach
origin	:default upstream repository - usually the one
HEAD	:current branch
HEAD^	:parent of HEAD
HEAD-4	:the great-great grandparent of HEAD

## Commiting
##
    $


