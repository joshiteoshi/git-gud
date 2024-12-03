# git gud script/detailed plan

## lesson plan

 - what is git?
 - how does git work?
 - how can we use git?
 - where to from here?

### what is git?

 - git is a version control system (vcs) software
   - in fact for many people, vcs is git (does any one know svn or hg?)
   - unless you work for facebook, you'll use git
 - git helps track versions of a project directory
   - this can be *any* type of project
   - but usually, git is used with software projects
     - the original version was written by linus torvalds, the linux kernel man
     - in fact, git was made for linux kernel development
 - what does "track versions" mean? git can
   - save "snapshots" of your current work
     - like a save in skyrim
   - make new "branches" to work on experimental changes or new features
   - merge branches when new features are ready
   - identify snapshots which introduced bugs
   - use diff to find buggy code
   - use blame to find who wrote buggy code
   - and several hundred other things
 - benefits
   - when working on a project over time, git helps make sure things don't break
   - if changes are made, you can always restore previous versions
   - collaborating on projects is made easier
   - multiple conflicting changes can be quickly reconciled
 - some other features (which distinguish it from other vcs)
   - nyoom (it's fast, compared to other vcs in many operations)
   - it's distributed - so many team workflows are supported
   - there's good support
     - easy to google problems
     - lots of gui tools
     - lots of repository hosting services

### how does git work?

 - introduction to git, stolen from a presentation, which stole from a great blog post
 - the git parable
   - mostly presented as a story, but important elements are marked !
   - imagine you wanted to build a large piece of software but vcs doesn't yet exist
   - all your computer has is a text editor and some basic file system commands
   - unlike the real you, fake you is responsible and feels there should be a system to track code changes
   - ! you need a way to retrieve code changes and deletions

#### snapshots

 - inspiration
   - you have a photographer friend who works for a "special moments" place
   - spends work taking photos of awkward teens and kid's whose parents force them to
   - tells you about a client who has her daughter's photo taken at the same time every year
     - keeps a photo album of the photos taken
     - images trace her daughter's history
     - help her remember what her daughter was like in each moment in time
 - ! idea for vcs
   - snapshots, an image of your codebase is what you need for version control
     - you can make copies of your codebase each time you made a change
     - and thus review old changes on demand
   - you run off and implement a system for your project
     - you start the project in a directory called `working`
     - with each feature implemented, you save all files and copy `working` to `snapshot-0`
     - in `snapshot-0`, you include `message` - a file summarising the changes made
     - with each new snapshot, you simply increment the number (ie `snapshot-1`, `snapshot-2`, etc.)

#### branches

 - story
   - eventually, a release candidate for your project appears
   - `snapshot-99` becomes the source for release version 1.0
   - software and packaged and released to the public - to a warm reception
   - encouraged, you begin working on new features for version 1.1
     - a few new snapshots are made, `snapshot-100` to `snapshot-109`
   - but soon, bug reports come in
     - you now need to patch the release without including the new features
 - problem
   - since you have a great system, you can bring back `snapshot-99` and copy it to `working` to fix
   - however, naming the fix snapshot is a problem
   - `snapshot-110` implies `snapshot-109` is the previous snapshot
   - but actually, `snapshot-99` is
   - a more powerful system is needed track version ancestry
 - inspiration
   - you decide to take a walk - studies show exposure to nature refreshes the mind
   - you observe the trees and follow their branches
   - no matter where on the tree you are, you can always trace the branches back to the trunk
 - ! this is a great way to track multiple development paths
   - rather than thinking about code history linearly, think of it as a tree graph
   - snapshots do not necessarily follow each other numerically
   - but the parent of each snapshot can be recorded in `message`
   - and they can be followed to find a version's history
 - another problem
   - we don't know what the latest version on each tree branch is anymore
   - `snapshot-110` could be the latest snapshot on any branch
   - we could add "branch names" to each snapshot, but that doesn't really help figure out the latest snapshot
 - ! solution
   - the minimum information to identify a snapshot's branch is the newest snapshot on the branch
   - the rest of the snapshots can be traced by following the parents
   - outside of the snapshot directories, you make a `branches` file
     - this contains `name : snapshot` pairs, pointing each branch name to the newest snapshot on the branch
     - each time a new snapshot on a branch is made, you update `branches`
     - a minor inconvenience for making branches and snapshots easy to track

#### distributed development

 - more story
   - working alone is lonely
   - but you have a friend who has the same computer setup as you
   - since you've organised everything so well, you give them a copy of all your snapshots and work
     - you explain the snapshot system and encourage them to use it also
     - it makes changes easier to track and trace
 - problem
   - you both have your own lives and work on the project seperately
   - when you both meet again, you find a critical flaw in your vcs
     - both of your computers have different snapshots with identical names
     - no one can tell who wrote each snapshot
 - ! the naming system has to be rethought
   - to identify authors, you both include the author and author's emails in snapshot `message` files
   - to name each snapshot, you hash the `message` of each snapshot and use that as the name
     - you decide to use the sha1 algorithm which produces a 40 character output
     - ! this gives the snapshot a unique name, since hashes do not\* collide
 - story continue continues
   - because of the new system, work can now be decentralised
   - your friend takes long trips without internet and can make any number of snapshots
   - connection is only needed when they want to share their snapshots
   - before a trip, both you and your friend start working on a `parsers` branch
     - they work on a very complex html parser
     - you work on a very simple css parser
 - ! merging
   - when he returns, you have the task of merging both your works
   - the next snapshot created is special
     - it does not contain any new changes (except to reconcile yours and their snapshots)
     - it has 2 parents rather than 1

#### changing history, staging, and diffing

 - more story
   - like most software developers, you have a crippling alcohol addiction
   - after a night of drinking, you make you way home and continue coding, making snapshots
   - the next morning, you review your code
     - it's ok, but some early changes are a bit emabrrasing and corrected later on
     - you want to turn your messy snapshots into 2 clean snapshots
 - ! rewriting history
   - taking your newest snapshot, you revert some of the changes
     - but instead of making the drunk snapshot the parent
     - you choose a snapshot from before your fun night
   - then, you copy drunk snapshot back to `working`
     - and make the new snapshot the parent, changing its history
   - from there, you can keep the drunk snapshots around, or delete them
   - and you've just rewritten the history of your snapshots!
 - jumping around features
   - the encounter also made you realise something else
   - sometimes, you'll hop around the project, working on different things
   - changes are then made on different features which should really be 2 different snapshots
 - ! staging
   - to resolve this, you introduce a `staging` directory
   - rather than taking snapshots of `working`, you move changes you want in the next snapshot to `staging` first
     - then, you take snapshots of `staging` instead
     - if you change something in `working` that should not be in the next snapshot, simply disinclude its change `staging`
 - ! too many directories
   - with so many different directories (ie `working`, `staging`, `snapshot-X`), it's hard to track what changed
     - `message` files are nice, but only give summaries
   - to get a full list of changes, you write a "diffing" algorithm to compare versions of any files

#### saving storage space

after some time, timo complains about the file space use of your vcs.

 - many snapshots have almost identical copies of the same files
 - it feels a bit wasteful

TODO: decide if this part of the git parable is necessary

// i'm not sure if i want to discuss this since this doesn't really help use git

// though it could be fun to learn anyway

// we will see

### how can we use git?

 - install git time (if people don't have it installed)
 - introduction to git cli
   - gui options discussed later
   - some time to familiarise if people are unfamiliar with terminal emulators
 - show and try various commands
   - need a playground repository for this

#### init & clone

 - 2 ways to initialise a git repository
   - init > initialise a new repository with no commits, branches etc.
   - clone > copy an existing repository including commits, branches etc.
 - repositories are just special directories that git can operate on and track
 - exercise:
   - try cloning this repository

#### add & commit

TODO: add example of how to stage and commit changes, possibly as files in this repo

// i'm thinking of including either tex or mdbook course notes so maybe participants can look for improvements and add those

#### checkout & branch

 - implementing changes or features commonly done on braches besides main
   - main technically is a branch
   - but most teams treat it as privileged, which is good practice
 - as discussed, branches are just named commits (or pointers to commits)
 - 2 (3) ways to make new branches
   - branch > creates a new branch
   - checkout -b > create a new branch and "select" it
   - switch -c > see checkout -b
 - when commiting to a branch, the branch's pointer moves to the new commit
 - at any given time, a commit is "selected" as the `HEAD`
   - if you haven't touched anything, the files in the directory will reflect the state of the file system at that commit
   - checkout moves the head around
   - checkout -b makes a new branch and "selects" it, moving the `HEAD` to the commit
 - exercise:
   - make a new branch and commit a change

#### status, log, & reflog

 - between commits, staging areas, and the working area, it's hard to remember what is happening all the time
 - git provides tools to figure out what is happening at any given moment
   - status > gives information about the current branch, comparison with origin, diff between working and staged
   - log > shows the commit history (of the branch or project depending on flags)
   - reflog > records branch tip and `HEAD` changes (this is very useful when stuff breaks)
 - exercise:
   - try all the commands and decipher the output

#### diff

TODO: research how `git diff` works cos i've actually never used it

#### collaboration

TODO: figure out how to demonstrate push, merge, fetch, and pull, possibly through github

#### reset, revert, and clean

 - for when git breaks
 - 2 ways to undo changes
   - reset > undoes a set of commits
     - this will break more things if changes are already pushed to remote
   - revert > makes a new commit which undoes the changes of a previous one
     - this is safe to do if changes are pushed
     - the reverts are considered "new changes"
 - clean helps if there are a bunch of unwanted files
   - removes untracked files from the current repository
 - exercise
   - TODO

#### bisect & grep

 - for when your project breaks
 - bisect > search for a commit which introduced a "bad change"
   - select a known "bad" commit and "good" commit
   - binary search to find the first "bad" commit
 - grep > search for a pattern in a work tree, blobs in index file, or blobs in a tree
   - search for a regular expression pattern
   - kinda like the `grep` command but fashioned for git use
   - TODO: need examples for this probably
 - exercise:
   - TODO

#### giving up

 - when everything is truly, completely screwed
 - [relevant xkcd](https://xkcd.com/1597/)
 - if you really cannot fix whatever you did to your repository
   - save your changes somewhere else
   - `rm -rf ./*`
   - re-clone the repository from origin
 - there is no exercise for this; but still somehow the most useful advice i can give

### where to from here?

here is a list of things we did not cover:

 - .gitignore files
   - typical in project directories
   - used typically to hide project secrets (api access keys/key data files/intermediate files that are not impt)
   - <timo needs to add more here but his brain isnt working\>
 - alternatives vcs
   - you can probably go your entire professional career without anything else
   - but it's fun to explore different tech
   - and maybe you'll like another vcs for your workflow better
   - some other ones
     - helix core - standard (?) in game development
     - subversion - better (?) for projects with non-code elements
     - mercurial - distributed like git, but i don't know if anyone uses this
 - remote hosting / git repository hosting services
   - i don't have to tell you about github
   - everyone knows what github is
   - but there are other hosting services (my internship used bitbucket)
   - some have extra features on top that you might like
   - some other services
     - gitlab - devops platform (whatever that means) + repository hosting
     - bitbucket - i hate you
     - github - you already have an account
 - git gui tools
   - commands are not always easy to remember
   - it is good to get used to the command line
   - but if you don't want to, or like being able to visualise source trees
   - some gui tools
     - github desktop - you already have this installed
     - git-gui / gitk - these come with git and look like ms-dos
     - lazygit - this one is terminal-based but makes complicated stuff a bit easier
   - i don't have any other recommendations but there's a list on the git website
