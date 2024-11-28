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



### how can we use git?

knowing how it works, we can look at how git is used.
there are 2 ways to use git, from its command line interface or with a gui.
for this workshop, we will use the command line.
but i will also recommend some gui options, some of which you might already use.

// i should make a playground repository for this

### where to from here?

here is a list of things we did not cover:

 - remote hosting / git repository hosting services

## references

 - [the git parable](https://tom.preston-werner.com/2009/05/19/the-git-parable)
 - [the missing semester of your cs education](https://missing.csail.mit.edu/2020/version-control/)
 - [git magic](https://www-cs-students.stanford.edu/~blynn/gitmagic/pr01.html)
