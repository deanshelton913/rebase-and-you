Rebase and YOU!
===
* 10 min | what is a pull request, and why are we using it.
* 25 min | walk thru of what the process looks like
* 10 min | integration with codeship, and plans for the future
* 15 min | questions


---
###What is a Pull Request?

- PR's are a feature of **GitHub**, not GIT.
- PR's are just a fancy diff between two branches.
- PR's provide a venue for feedback and testing.
- PR's can be a testing gate for CI systems (codeship, travis, jenkins, et cetera.)

###Why SHS-Web is using PRs
- Master must be deployable.
- Humans make mistakes.
- Our team is growing, and code reviews are helpful.

###Rebasing. Why not just `git pull`?

Lets understand the problem first:

- What does `git pull` actually do?
**pull = fetch + merge**
  	 - fetch = update your tracking branches
     - merge = create a commit that blends two branches
         - GOTCHA: **combine their history using timestamps!**

This has some cofusing results. Especially when you're not done yet!
- Your **INCOMPLETE** feture is now split into diff parts of the history by changes in master. Weak.
```
* e589d92 - feature apples! (6 minutes ago) <Dean Shelton>
*   fe21613 - Merge branch 'master' into feature (9 minutes ago) <Dean Shelton>
|\
| * b3891f9 - master-SUPER-IMPORTANT-THING-YOU-NEED (39 minutes ago) <GARY>
| * 6de3722 - master-cucumber! (39 minutes ago) <GARY>
* | d90186c - feature strawb (38 minutes ago) <Dean Shelton>
* | 3e72b4b - feature-bananas! (40 minutes ago) <Dean Shelton>
|/
* 57fc63c - initial commit! (41 minutes ago) <Dean Shelton>
```

###How does Rebasing solve this?
`git pull --rebase origin master` would have given us this:

```
* e589d92 - (feature) feature apples! (6 minutes ago) <Dean Shelton>
|\
| * d90186c - feature strawb (38 minutes ago) <Dean Shelton>
| * 3e72b4b - feature-bananas! (40 minutes ago) <Dean Shelton>
* | b3891f9 - master-SUPER-IMPORTANT-THING-YOU-NEED (39 minutes ago) <GARY>
* | 6de3722 - master-cucumber! (39 minutes ago) <GARY>
|/
* 57fc63c - initial commit! (41 minutes ago) <Dean Shelton>
```
Note: All of your feature work is together! As it should be. You're not done, and may need to edit your commits before you're ready to merge.

Interactive Rebase
`git rebase -i HEAD~2` Rewind 2 commits back
`git rebase -i master` Rewind since your current branched diverged from master.



###What does the process look like?
1. Joe creates a local feature branch git checkout -b my_feature_branch_name`
1. Joe runs `npm run rebase` to get up-to-date with origin master
1. Joe does some work and makes a commit `git add . && git commit -m 'changed something'`
1. Gary merges a pull request into master with super important changes
1. Joe does a rebase `git pull --rebase origin master` (or `npm run rebase`)
1. Joe resolves a merge conflict, and continues working.
1. Joe makes 3 more commits
1. Joe wants to PR his changes, but some of his commits before the rebase make the history confusing.
1. Joes does an interactive rebase, `git rebase -i master ` to combine two commits into one, and reword one of his commit messages.
1. Joe pushes his code as a feature branch on origin `git push origin my_feature_branch_name`
1. Joe opens his repo in github and creates the pull request.
1. Joes tests are green, and he merges his code


### Things that are now illegal
- `git push origin master`
- `git merge` or `git pull` without using `--rebase`
