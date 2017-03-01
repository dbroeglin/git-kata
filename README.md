# Git Kata

This document is a very brief introduction to git commands by practice.

Short URL to this file: https://goo.gl/U9clhp

# Setup

## Windows
    
From: https://chocolatey.org/install and logged in as administrator in an elevated PowerShell session:
     
     iwr https://chocolatey.org/install.ps1 -UseBasicParsing | iex
     # Close and reopen the PowerShell session
     choco install git -y
     choco install poshgit -y

## Mac OS X

On Mac OS X git is already instaled, however, if you want a newer version execute the following in your shell:

    brew install git

# Configuration

Basic git configuration:

	git config --global user.name "<YOUR FIRST AND LAST NAME>"
	git config --global user.email "<YOUR EMAIL ADDRESS>"

Some helpful aliases:

	git config --global alias.s "status -s -b"
	git config --global alias.l "log --decorate --graph --date=relative" # use this while doing the kata
	git config --global alias.l "log --decorate --graph --date=short" # use this for everyday work
	
	git config --global format.pretty "format:%C(cyan)%ad%Creset %C(yellow)%h%C(green)%d%Creset %C(cyan)%s %C(magenta) [%an]%Creset"
	git config --global log.date "iso"

Take a look at _$HOME/.gitconfig_ to see what just happened.

To list the configuration: 

     git config --global --list
     git config --list

# Working locally

## Create your own repository

    mkdir myrepo
    cd myrepo
    git init
    git log

## Add some content

    mkdir Src
    mkdir Test
    "echo 'Hello world !'" | Out-File -Encoding UTF8 .\Src\A.ps1

What do you see when you execute:

     git status

And when you execute: 

    git s

## First commit

Execute:

    git add Src

What do you see when you execute:

    git s
    git l

Commit your changes:

     git commit # Input "Initial commit" in the editor

Alternatively if you have VIM allergies ;-) :

     git commit -m 'Initial commit'

What do you see when you execute:

     git s
     git l

## Ignore some files

Generate a file that we want to ignore:

    "Content we want to ignore" > .\Src\A.ps1~
    git s

Ask git to ignore the file:

    "*~" | Out-File -Encoding UTF8 .gitignore
    git s
    git add -A
    git commit -m "Added a .gitignore"
    git l

Example of more elaborate ignore files: https://github.com/github/gitignore

## Changing files

Let's change some files:

    "echo 'Hello Universe!'" | Out-File -Encoding UTF8 .\Src\A.ps1
    "echo 'Hello Old World!'" | Out-File -Encoding UTF8 .\Src\B.ps1

    git s
    git diff

Add `A.ps1` to the _index_:

    git add Src/A.ps1

Why does the following command return nothing:

    git diff

Try: 

    git diff --staged

Why don't we see `B.ps1` in the diff ?

Commit:
    
    git commit -m "Universe"
    git l -p

Let's commit the changes in `B.ps1`:

    git add Src
    git commit -m "Adding B.ps1"
    git l

## Working with branches

List branches:

    git branch

Create a new _branch_:

    git checkout -b mybranch
    git s
    git branch

Let's create some changes in the new branch:

    "echo 'Hello Old Old World!'" | Out-File -Encoding UTF8 .\Src\B.ps1
    git commit -a -m "Old Old"
    git l

What do we see in `master`:

    git l master

Go back to the `master` branch:

    git checkout master
    git l
    ls Src

Go back to `mybranch` again:

    git checkout mybranch
    git l
    ls Src

Let's checkout `master` and commit some changes:

    git checkout master
    "echo 'Hello Small Universe!'" | Out-File -Encoding UTF8 .\Src\A.ps1
    git commit -m "Small universe" -a
    git l

`mybranch` has not changed:

    git l mybranch

Let's merge `mybranch` into `master`:

    git merge mybranch
    git l

# Working collaboratively

## Clone a repository

    cd $HOME
    git clone https://github.com/dbroeglin/Forge.git
    cd Forge
    git l

To list all branches, even remote branches:

    git branch -a

## Return to the past

Execute: 

    git reset --hard db322c
    git l

What happened ?

## Back to the future

To synchronize on a remote repository which is _ahead_ of ours:

    git fetch

Check the changes:

    git l master origin/master

Execute:

    git merge origin/master
    git l

What happened ? Observe the "fast-forward" mention.

## Pushing changes

Edit a file and commit the changes with:

    git commit -m "My first contribution" -a
    git l

Observe the difference between `master` and `origin/master`.

To push changes:

    git push

It won't work but that's the spirit! Thanks for your contribution! ;-)

## Collaborative changes (keeping history intact)

Let's go back to the past again:

    git reset --hard db322c

Change a few lines in `README.md` and commit:

    git commit -m "My second contribution" -a
    git l

Lets synchronize with the remote repository:

    git fetch
    git l master origin/master

Oh my! While we were adding our contribution someone else added one too!

Let's merge our changes with the remote changes:

    git merge origin/master
    git l

# Collaborative changes (rewriting my history)

Redo the collaborative section but at the end use:

    git rebase origin/master
    git l

Observe the commit id for your contribution before and after the _rebase_ operation.

# Open questions and reference documentation

* http://git-scm.com/book
* http://git-scm.com/documentation
* Scortt Chacon's git presentations: https://speakerdeck.com/schacon/introduction-to-git
