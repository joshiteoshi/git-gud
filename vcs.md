# git gud

## motivation

 - most students (especially in sutd), learn git through github
   - its "that place we store our projects"
   - rather than "that version control tool so we can fix stuff if it breaks"
 - students just memorise the commands and when something breaks, can't easily fix it
   - heck even i'm still not confident in fixing stuff i break
 - design decisions informed by the backend are abstracted to the cl in strange ways
   - no one knows what diff, bisect, reflog, etc. do
 - this course is an attempt to (1) teach git and (2) bridge the gap in knowledge

## lesson plan

 - what is git?
 - how does git work?
 - how can we use git?
 - where to from here?

### what is git?

// do we need a section for everyone to just install git?

git is a version control system (vcs).
in fact for many people, vcs is git (has anyone even heard of subversion or mercurial?).
unless you work for facebook (which made its own vcs), you'll probably have to use git.

 - git helps people keep track of versions of a project directory
 - this can be *any* type of project
 - but most frequently, it is used with software projects
   - its original version was made by linus torvalds, the linux kernel man
   - in fact it was made for linux kernel development

but what does "keep track of versions" mean?
here's a list of things git can do:

 - save a "snapshot" of your current work
   - like a checkpoint or save slot in skyrim
 - make new "branches" to work on experimental changes you don't want in the main project
 - merge branches when new features are ready
 - identify a snapshot which introduced a bug
 - use git diff to find the buggy code
 - use git blame to figure out who introduced buggy code
 - and like several hundred other things

when working on a software project over a period of time, git helps make sure things don't break.
if changes are made, you can always revive previous versions.
git also enables better collaboration on projects.
multiple changes made by individuals can be quickly reconciled and applied to a project.

some of the other (nice but maybe less important to us) features of git.

 - nyoom.
 - nyoom.
 - it's fast (compared to other vcs, for most operations)
 - it's distributed - so many different team workflows are supported
 - there's lots of git repository hosting services

### how does git work?

// maybe discuss the model first and how to manipulate it?
// this should not cover specific commands yet; thats for the how to use section

// working idea is to follow the git parable (there is also a presentation version that can help)

to introduce how git works, i am going to steal from a presentation, which stole from a great blog post.
mostly because its a great introduction to git which helped me understand it better.

imagine you wanted to build a large piece of software but vcs, any vcs, doesn't yet exist.
on your computer, you only have a text editor and some basic file system commands.
now unlike real life, you are a responsible person who feels they should come up with a way to keep track of changes in your code.

 - it would be good to be able to retrieve code changed or deleted

#### snapshots

you have a friend (let's call him wei jie) who is a photographer at one of those "special moments" place.
he spends his days taking photos for awkward teenagers and kids who's parents forced them to.
one time, while having lunch with him, he tells you about one of his frequent clients - jia wen.
at the same time each year, jia wen's mom brings her to have her photo taken, keeping all the photos in one album.
this way, she can remember what her daughter was like in each moment in time.

 - because this is a story, this gives you an idea
 - snapshots, an image of your codebase is what you need for version control
 - what if you had a copy of your codebase each time you made a change
   - this let's you review old code on demand

so you run off to get to work on your project.

 - you start your project in a directory called `working`
 - with each feature you implement, you save all your files and copy the `working` directory to `snapshot-x`
 - and you store all the `snapshots` to a `snapshot` directory
 - to remember the work you did for each snapshot, you add in a special `message` file, a summary of the changes made

#### branches

after some time, a release candidate for your project appears.
`snapshot-99` becomes basis for a release version 1.0.
you package the software and distribute it to the public with a warm reception.

encouraged, you work on adding new features for a 1.1 release.
you make a couple more snapshots, maybe `snapshot-100` to `snapshot-109`.
but soon, bug reports for the 1.0 emerge.
to patch the 1.0 version, you don't want to include the new features.

 - fortunately, you can revive `snapshot-99` and fix the bugs from there
 - you copy `snapshot-99` and move it to the `working` directory
 - and fix the newly reported bugs, giving you a patched 1.0.1 version

but then you encounter a problem: what number should this snapshot be assigned?
if you use `snapshot-110`, the linear flow of snapshot numbers are interrupted.
something more powerful is needed to track version ancestry.

you decide to take a walk - studies show exposure to nature refreshes the mind's energy.
as you walk, you observe the trees and, in particular, their branches.
as you follow each branch, you can always trace your way back to the trunk of the tree, no matter how complex the tree's structure.

 - what a coinkydink, this is a great way to keep track of multiple development paths
 - rather than thinking about the code as a linear progression, you can see it as a tree
 - each snapshot does not necessarily follow the previous snapshot numerically
 - but you can record the "parent" snapshot of each snapshot in its `message` file to always know where a snapshot comes from
 - all you have to do is follow the chain of parents to find a version's history

this produces another problem: we don't know which is the latest snapshot anymore.
obviously the highest number is newest, but we don't know which branch a snapshot belongs to.

 - we could add a "branch name" to each snapshot
 - but that does not necessarily help you identify the newest snapshot
 - the minimum information a branch really needs is the newest snapshot on that branch
   - we can trace the rest of the branch by following parents
 - thus, we can take another approach
   - outside of the snapshots, you make a `branches` file
   - in it, you list all the name : newest snapshot pairs for each branch
   - each time you add a new snapshot to a branch, you simply update the `branches` file
   - a small inconvenience, but now you can make any number of branches and snapshots and always know where they came from

#### distributed development

sometimes it gets lonely working alone.
but fortunately, you have a friend with the same computer setup as you who would like to help.
because you've come up with such a great system so far, you give timo a copy of all your stuff and explain how your makeshift vcs works.

it's nice to have someone to work on projects with but you have different lives, and sometimes don't see each other for weeks.
both of you work on the project in the meantime but when you meet again, you find yet another critical problem with your system.

 - both of your computers now have different snapshots with identical names
 - you also don't know who made each snapshot

this requires rethinking snapshot naming and `message` files.

 - to know who wrote what, you also add the author of each snapshot to its `message`
 - then rather than manually naming snapshots, you give each one a unique name
   - you know hashing functions produce near unique, fixed length names
   - so you use sha1 to hash the `message` and use the hash to name the snapshot

this is very important!
snapshots (and their parents) are identified by a unique, 40 character sha1 hash.
this means names never\* collide.

because of this new system, work doesn't have to be online or centralised anymore.
timo takes a long trip without internet connection and can add as many snapshots as he wants on different branches without worrying about messing up your stuff.
connection is only needed when you want to finally share snapshots with each other.

before timo leaves for his trip, you both start working on a `parsers` branch.
timo gets to work on the very hard task of writing a html parser while you get to the much easier task of writing a css parser.
when he returns, you now have to merge the 2 different branches of development.
since the tasks were seperate, this merge is farily simple.

 - however, you also realise the next snapshot is special
 - it doesn't contain any unique changes (all additions are from its parent snapshots)
 - it has 2 parent snapshots!

#### changing history, staging, and diffing

like most software developers, you have a crippling alcohol addiction.
after a night of alcohol, you make your way home and continue coding, making a few snapshots.
the next morning, you review your code.
it's ok, but some of the early snapshots have mistakes which make you cringe a bit.
you want to turn these messy snapshots into 2 clean snapshots.

 - taking your newest snapshot, you revert some of the changes
   - but instead of making the parent of this snapshot the drunk one
   - you make it the last snapshot before your drunk coding
 - you then copy the drunk snapshot back to `working` and make its parent the new snapshot you made

you've just rewritten the history of a line of commits!
since the drunk code has been moved to another branch, you can delete or leave it.

this encounter also causes you to realise something else.
sometimes, you'll hop around between features only to realise you're working on things which should be seperate snapshots.
to work around this, the idea of a staging directory might help.

 - rather than taking a snapshot of `working` each time you want to save
 - you move the changes you want to a `staging` directory first
 - if you make a change you want to save, you repeat it in `staging`
 - otherwise, you can leave it in `working` for later

introducing so many new directories makes it hard to figure out exactly what is different between versions.
`message` files are nice, but they only give a summary, not the full list of changes.
it would be nice to have a tool that lets you see all the differences between versions.

 - you get to work implementing a diffing algorithms for your vcs
 - using it, you can easily see all of the differences between any 2 versions of the codebase
 - you can also see the differences between your `staging` and `working` directories

#### saving storage space

after some time, timo complains about the file space use of your vcs.

 - many snapshots have almost identical copies of the same files
 - it feels a bit wasteful

// i'm not sure if i want to discuss this since this doesn't really help use git
// though it could be fun to learn anyway
// we will see

### how can we use git?

knowing how it works, we can look at how git is used.
there are 2 ways to use git, from its command line interface or with a gui.
for this workshop, we will use the command line.
but i will also recommend some gui options, some of which you might already use.

// i should make a playground repository for this
// actually maybe just use this repository

### where to from here?

here is a list of things we did not cover:

 - alternatives vcs
   - you can probably go your entire professional career without anything else
   - but it's fun to explore different tech
   - and maybe you'll like another vcs for your workflow better
 - remote hosting / git repository hosting services
   - i don't have to tell you about github
   - everyone knows what github is
   - but there are other hosting services (my internship used bitbucket)
   - some have extra features on top that you might like

## references

 - [the git parable](https://tom.preston-werner.com/2009/05/19/the-git-parable)
 - [the git parable - in presentation form](https://github.com/jherland/git_parable)
 - [the missing semester of your cs education](https://missing.csail.mit.edu/2020/version-control/)
 - [git magic](https://www-cs-students.stanford.edu/~blynn/gitmagic/pr01.html)
