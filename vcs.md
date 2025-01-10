# git gud lesson plan
swww


## motivation

 - most students (especially in sutd), learn git through github
   - its "that place we store our projects"
   - rather than "that version control tool so we can fix stuff if it breaks"
 - students just memorise the commands and when something breaks, can't easily fix it
   - heck even i'm still not confident in fixing stuff i break
 - design decisions informed by the backend are abstracted to the cl in strange ways
   - no one knows what diff, bisect, reflog, etc. do
 - this course is an attempt to (1) teach git and (2) bridge the gap in knowledge

## objectives

 - by the end of the workshop, attendees should be able to
   - define "version control system" and explain why programmers use vcs software
     - list examples of vcs software besides git
   - describe git's underlying data-representation model
   - use a selection of git commands and explain how they interact with git's data model
   - select git gui software and repository hosting services to support their workflow
   - know roughly what to google when things break

## lesson plan

 - what is git?
   - what git can do
   - what it's used for
   - why use git (or like any vcs)
 - how does git work?
   - <s>stolen</s> heavily inspired by the git parable by tom preston-werner
     - snapshots
     - branches
     - distributed development
     - changing history, staging, & diffing
     - saving space (tbc)
   - this part does not cover commands; it's an introduction to git concepts
 - how can we use git?
   - git's command line interface
     - init and clone
     - add and commit
     - checkout, branch, and tag
     - status, log, and reflog
     - diff
   - working with other people
     - push, merge, fetch, and pull
     - github?
   - when everything is borked
     - reset, revert, and clean
     - bisect and grep
     - giving up
   - this part has a "playground" repository to try out the commands and mess with stuff
 - where to from here?
   - alternative vcs?
   - git repository hosting platforms
   - various other git commands
   - recommendation for further reading

## material

 - [slides 0](https://www.canva.com/design/DAGbe06PeE4/hVvKoOrH4PY36i6xq6SlBQ/edit?utm_content=DAGbe06PeE4&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton)
 - [slides 1](https://www.canva.com/design/DAGbl_Zc-ik/-Q8rzUYKupW5T8ndcHOdSw/edit?utm_content=DAGbl_Zc-ik&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton)

## references

 - [the git parable](https://tom.preston-werner.com/2009/05/19/the-git-parable)
 - [the git parable - in presentation form](https://github.com/jherland/git_parable)
 - [the missing semester of your cs education](https://missing.csail.mit.edu/2020/version-control/)
 - [git magic](https://www-cs-students.stanford.edu/~blynn/gitmagic/pr01.html)
 - [oh shit, git?](https://ohshitgit.com/)

 - [learn git branching](https://learngitbranching.js.org/)
