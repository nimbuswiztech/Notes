# Git rebase

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*K4anH9QzRcPqLCv-7HyiCQ.png" alt="" height="235" width="700"><figcaption><p>Git-Rebase</p></figcaption></figure>

In this post, we’ll understand what is git rebase, how it is different from git merge and when to use the rebase command. The git rebase command is one of those commands which can work magic for managing the future development of a product by simplifying git history but it can be disastrous if not used carefully. Essentially, git merge and git rebase do the same thing, i.e., bring the contents of two branches together. However both of these commands execute this change, in entirely different ways.

## Use caution <a href="#id-20a3" id="id-20a3"></a>

Some things to keep in mind before you rebase:

1. Never rebase commits that have been pushed and shared with others. The only exception to this rule is when you are certain no one on your team is using the commits or the branch you pushed.
2. Use rebase to catch up with the commits on another branch as you work with a local feature branch. This is especially useful when working in long-running feature branches to check how your changes work with the latest updates on the master branch.
3. You can’t update a published branch with a `push` after you've rebased the local branch. You'll need to force push the branch to rewrite the history of the remote branch to match the local history. Never force push branches in use by others.

## Concept of Git Rebase <a href="#id-9253" id="id-9253"></a>

### Problem Statement <a href="#id-856a" id="id-856a"></a>

Consider that our team just completed the production release from master branch. Now they have started working on a entirely new feature in one of the dedicate branch named as **dev-feature01** branch. However, some one found a bug in the production release. Therefore, one of the developers created a **quickfix** branch to fix the bug and merged his/her code into **master** branch. Now we need to bring together the **master** branch and **dev-feature01** branch.

Below is graphical summary of what commit tree looks like:

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*P8HvnNGEgOp7uuw5Uvf-HA.png" alt="" height="412" width="700"><figcaption><p>problem-statement-git-branches-with-different-history</p></figcaption></figure>

### Solution 01: Git Merge <a href="#id-4091" id="id-4091"></a>

The merge is the most widely used method. To incorporate changes from **master** branch to **dev-feature01** branch, first we need to checkout the **dev-feature01** branch and then merge the **master** branch:

```
git checkout dev-feature01
git merge master
```

Or, we can condense this to a one-liner:

```
git checkout master dev-feature01
```

Once, its successful, the result commit tree would look something like below:

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*bMfmJ0HjxtpR1tyT8-EV5w.png" alt="" height="387" width="700"><figcaption><p>what-happens-after-git-merge</p></figcaption></figure>

Merging is nice because it’s a _non-destructive_ operation. The existing branches are not changed in any way. However, it adds an extra commit called merge commit. This is added every time we need to incorporate changes from other branches into this branch.

### Solution 02: Git Rebase <a href="#ee29" id="ee29"></a>

As an alternative to merging, we can rebase the **dev-feature01** branch onto **master** branch using the following commands:

```
git checkout dev-feature01
git rebase master
```

This moves the entire **dev-feature01** branch to begin on the tip of the master branch, effectively incorporating all of the new commits in **master**. But, instead of using a merge commit, rebasing _re-writes_ the project history by _creating brand new commits_ for each commit in the original branch.

So our commit tree would look something like below:

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*z810gkDE9jR_fi0bT52eqA.png" alt="" height="315" width="700"><figcaption><p>the-resultant-commit-tree-after-rebasing</p></figcaption></figure>

So instead of commits E and F, we have the commits E’ and F’. Note that the commits E and F are still there in git history, but they are just not accessible any more.

## Benefits of Git Rebase <a href="#id-18e0" id="id-18e0"></a>

The major benefit of rebasing is that we get a much cleaner project history. First, it eliminates the unnecessary merge commits required by git merge. Second, as we can see in the above diagram, rebasing also results in a perfectly linear project history — we can follow the tip of feature all the way to the beginning of the project without any forks. This makes it easier to navigate the git history of the project.

## The Golden Rule of Git Rebase <a href="#ed8d" id="ed8d"></a>

Since git rebase command essentially re-writes git history, it should never be used on a branch which is shared with another developer (Unless both developers are kind of git experts). Or as its also said, never use the rebasing for public branches.

The rebase moves all of the commits in master onto the tip of **dev-feature01** branch. The problem is that this only happened in your version of the repository. All of the other developers are still working with the original master branch which they got initially. Since rebasing results in brand new commits, Git will think that your **master** branch’s history has diverged from everybody else’s. The only way to synchronize the two **master** branches is to merge them back together, resulting in an extra merge commit and two sets of commits that contain the same changes (the original ones, and the ones from your rebased branch).

To understand more, let’s consider below state of commit tree on same product on developer A, Central Server (GitHub, Bitbucket, VSTS etc.) and developer B:

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*7ZGs9BaYYA8J7U4ycPdVSQ.png" alt="" height="235" width="700"><figcaption><p>git-commit-history-comparison-01</p></figcaption></figure>

We can see that all 3 have same versions.

Now, consider that developer A breaks the golden rule by rebasing master branch with dev-feature01 branch. In this case, the resultant tree would look like:

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*a5_V-PS6xf_oUr6_inTIoA.png" alt="" height="295" width="700"><figcaption><p>git-commit-history-comparison-02</p></figcaption></figure>

As you can see, the commit tree of developer A is now different from others. Meanwhile, developer B has gone ahead and made another commit E to his/her version of repository. None of them has sync’d their code to the central server.

Let’s consider that developer A tries to sync his **master** branch to the central server. He’ll get denied because the his **master** branch is now different from the **master** branch in the central server. One solution for him/her is to push his branch forcefully to the central server using git push –force. It will essentially overwrites remote branch with his/her version.

Now, the developer B has to resync his **master** branch before he/she can sync his/her code. So git will first do a pull and then a merge in order to resolve code conflicts. This is what it will look like once its complete:

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*BshC_MNWQ3HPHFP9g7eQ3w.png" alt="" height="309" width="700"><figcaption><p>git-commit-history-comparison-03</p></figcaption></figure>

Developer B is now finally able to push code back into central server and then developer A has to resync his code to get the latest version. In the end, the commit trees would look like:

<figure><img src="https://miro.medium.com/v2/resize:fit:1400/1*xSub_o2tpZzF_noV57QGlA.png" alt="" height="359" width="700"><figcaption><p>git-commit-history-comparison-04</p></figcaption></figure>

You can also notice duplicate commits in the repository — D and D’ , which have the same set of changes. The number of duplicate commits can be as large as the number of commits inside your **rebased** branch.

As you can see, it has become hard to understand the flow of commits from one branch to another branch.

## When to use Git Rebase <a href="#edc8" id="edc8"></a>

Above illustration makes it look like that git rebase is essentially not good to work on public branches. However, git rebase can also be done on the same branch. By periodically performing an interactive rebase on local branch, you can make sure each commit mentioned in git history is focused and meaningful. This lets you write your code without worrying about breaking it up into isolated commits — you can fix it up after the fact. This process is also sometimes known as the local cleanup.
